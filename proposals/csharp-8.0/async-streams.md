---
ms.openlocfilehash: 9ed1aa75d581cecbf754a84d1f523c6334b8c0ac
ms.sourcegitcommit: 5278336b61519956240a6f7d83bcb4322019e032
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/10/2020
ms.locfileid: "79485257"
---
# <a name="async-streams"></a>Asynchronní streamy

* Navrženo [x]
* [x] prototyp
* [] Implementace
* [] Specifikace

## <a name="summary"></a>Souhrn
[summary]: #summary

C#má podporu metod iterátoru a asynchronních metod, ale nepodporuje metodu, která je iterátorem a asynchronní metodou.  Měli byste to napravit tak, že umožníte použití `await` v nové formě iterátoru `async`, což vrátí `IAsyncEnumerable<T>` nebo `IAsyncEnumerator<T>` místo `IEnumerable<T>` nebo `IEnumerator<T>`, a to s `IAsyncEnumerable<T>`m příplatnou v novém `await foreach`.  Rozhraní `IAsyncDisposable` se používá také k povolení asynchronního vyčištění.

## <a name="related-discussion"></a>Související diskuze
- https://github.com/dotnet/roslyn/issues/261
- https://github.com/dotnet/roslyn/issues/114

## <a name="detailed-design"></a>Podrobný návrh
[design]: #detailed-design

## <a name="interfaces"></a>Rozhraní

### <a name="iasyncdisposable"></a>IAsyncDisposable

Existuje mnohem diskuze o `IAsyncDisposable` (například https://github.com/dotnet/roslyn/issues/114) a o tom, jestli je to dobrý nápad.  Je však vyžadován koncept pro přidání v podpoře asynchronních iterátorů.  Vzhledem k tomu, že bloky `finally` můžou obsahovat `await`a, protože `finally` bloky je potřeba spustit jako součást odstraňování iterátorů, potřebujeme asynchronní vyřazení.  To je také všeobecně užitečné při mazání prostředků, například při zavírání souborů (vyžadování vyprázdnění), zrušení registrace zpětných volání a poskytnutí způsobu, jak zjistit, kdy byla Odregistrace dokončena atd.

Následující rozhraní se přidá do základních knihoven .NET (např. System. Private. CoreLib/System. Runtime):
```csharp
namespace System
{
    public interface IAsyncDisposable
    {
        ValueTask DisposeAsync();
    }
}
```
Stejně jako u `Dispose`, vyvolávání `DisposeAsync` víckrát je přijatelné a následné vyvolání po prvním by se mělo považovat za nops, vrácení asynchronně dokončeně úspěšné úlohy (`DisposeAsync` nemusí být bezpečná pro přístup z více vláken, ale nemusí podporovat souběžné vyvolání).  Další typy mohou implementovat jak `IDisposable`, tak `IAsyncDisposable`a v případě, že to dělají, je obdobně přijatelné vyvolat `Dispose` a pak `DisposeAsync` nebo naopak, ale pouze první by měla být smysluplná a následné vyvolání by měla být NOP.  V takovém případě, pokud typ implementuje obojí, doporučujeme, aby spotřebitelé volali jednou a pouze jednou relevantní metodu založenou na kontextu, `Dispose` v synchronních kontextech a `DisposeAsync` v asynchronních typech.

(Ponechávám diskuzi o tom, jak `IAsyncDisposable` komunikuje s `using` k samostatné diskuzi.  A pokrytí toho, jak komunikuje s `foreach`, se zpracovává později v tomto návrhu.)

Uvažované alternativy:
- _`DisposeAsync` přijetí `CancellationToken`_ : i když teoreticky dává smysl, že může dojít ke zrušení jakékoli asynchronní operace, vyřazení je o vyčištění, ukončování objektů, free'ingch prostředcích atd., což obecně není něco, co by mělo být zrušeno. vyčištění je pro práci, která je zrušená, pořád důležité.  Stejná `CancellationToken`, která způsobila zrušení skutečné práce, by představovala stejný token předaný `DisposeAsync`a `DisposeAsync` bezcenné, protože zrušení práce by způsobilo, `DisposeAsync` být NOP.  Pokud někdo chce zabránit zablokování čekání na odstranění, může se jim vyhýbat čekání na výsledný `ValueTask`nebo počkat na určitou dobu.
- _`DisposeAsync` vrácení `Task`_ : teď, když existuje neobecný `ValueTask` a dá se vytvořit ze `IValueTaskSource`, návratová `ValueTask` z `DisposeAsync` umožňuje, aby se existující objekt znovu použil jako příslib představující konečné dokončení asynchronního `DisposeAsync`, a to v případě, že se `Task` dokončí asynchronně.`DisposeAsync`
- _Konfigurace `DisposeAsync` s `bool continueOnCapturedContext` (`ConfigureAwait`)_ : i když mohou nastat problémy související s tím, jak je takový koncept vystavený `using`, `foreach`a dalších jazykových konstrukcích, které tuto funkci využívají, z perspektivy rozhraní není ve skutečnosti žádná `await`a a není nutné nic konfigurovat... Uživatelé `ValueTask` můžou tuto možnost spotřebovat, ale chtějí.
- _`IAsyncDisposable` dědí `IDisposable`_ : vzhledem k tomu, že by měla být použita pouze jedna nebo druhá, nemá smysl vynutit typy pro implementaci obou.
- _`IDisposableAsync` místo `IAsyncDisposable`_ : pojmenováváme, že se jedná o "asynchronní něco", zatímco operace jsou "dokončené Async", takže typy mají "Async" jako předponu a metody mají "Async" jako příponu.

### <a name="iasyncenumerable--iasyncenumerator"></a>IAsyncEnumerable / IAsyncEnumerator

Do základních knihoven .NET jsou přidány dvě rozhraní:

```csharp
namespace System.Collections.Generic
{
    public interface IAsyncEnumerable<out T>
    {
        IAsyncEnumerator<T> GetAsyncEnumerator(CancellationToken cancellationToken = default);
    }

    public interface IAsyncEnumerator<out T> : IAsyncDisposable
    {
        ValueTask<bool> MoveNextAsync();
        T Current { get; }
    }
}
```

Typickou spotřebou (bez dalších funkcí jazyka) by vypadalo takto:

```csharp
IAsyncEnumerator<T> enumerator = enumerable.GetAsyncEnumerator();
try
{
    while (await enumerator.MoveNextAsync())
    {
        Use(enumerator.Current);
    }
}
finally { await enumerator.DisposeAsync(); }
```

Považované za zahozené možnosti:
- _`Task<bool> MoveNextAsync(); T current { get; }`_ : použití `Task<bool>` by podporovalo použití objektu úlohy v mezipaměti k reprezentaci synchronních, úspěšných volání `MoveNextAsync`, ale pro asynchronní dokončení by se vyžadovalo přidělení.  Vrácením `ValueTask<bool>`umožníme, aby objekt enumerátoru implementoval sám sebe `IValueTaskSource<bool>` a měl by se používat jako zálohování pro `ValueTask<bool>` vrácené z `MoveNextAsync`, což zase umožňuje významně omezené režijní náklady.
- _`ValueTask<(bool, T)> MoveNextAsync();`_ : Nepoužívejte jen těžší, ale to znamená, že `T` již nesmí být kovariantní.
- _`ValueTask<T?> TryMoveNextAsync();`_ : nejedná se o kovariantu.
- _`Task<T?> TryMoveNextAsync();`_ : nejedná se o kovariantu, přidělení při každém volání atd.
- _`ITask<T?> TryMoveNextAsync();`_ : nejedná se o kovariantu, přidělení při každém volání atd.
- _`ITask<(bool,T)> TryMoveNextAsync();`_ : nejedná se o kovariantu, přidělení při každém volání atd.
- _`Task<bool> TryMoveNextAsync(out T result);`_ : výsledek `out` by se musel nastavit, když operace vrátí synchronní hodnotu, ne, když asynchronně dokončí úlohu potenciálně delší dobu v budoucnosti, a proto neexistuje žádný způsob, jak sdělit výsledek.
- _`IAsyncEnumerator<T>` neimplementuje `IAsyncDisposable`_ : můžeme se rozhodnout oddělit.  Nicméně v důsledku toho ztěžuje některé další oblasti návrhu, protože kód musí být schopný zabývat se možností, že enumerátor neposkytuje vyřazení, což ztěžuje psaní vzorových pomocníků.  Kromě toho bude běžné, že enumerátory budou mít potřebu vyřazení (například každý C# asynchronní iterátor, který má blok finally, většinu věcí vyčíslit data z připojení k síti atd.), a pokud k tomu nedojde, je jednoduché implementovat metodu čistě jako `public ValueTask DisposeAsync() => default(ValueTask);` s minimální další režií.
- _ `IAsyncEnumerator<T> GetAsyncEnumerator()`: není zadán žádný parametr tokenu zrušení.

#### <a name="viable-alternative"></a>Životaschopná alternativa:

```csharp
namespace System.Collections.Generic
{
    public interface IAsyncEnumerable<out T>
    {
        IAsyncEnumerator<T> GetAsyncEnumerator();
    }

    public interface IAsyncEnumerator<out T> : IAsyncDisposable
    {
        ValueTask<bool> WaitForNextAsync();
        T TryGetNext(out bool success);
    }
}
```

`TryGetNext` se používá ve vnitřní smyčce ke spotřebě položek s jedním voláním rozhraní, pokud jsou k dispozici synchronně.  Když další položku nelze získat synchronně, vrátí hodnotu false a kdykoli vrátí hodnotu false, volající musí následně vyvolat `WaitForNextAsync`, aby čekal na zpřístupnění další položky, nebo aby zjistil, že nikdy nebude další položkou. Typickou spotřebou (bez dalších funkcí jazyka) by vypadalo takto:

```csharp
IAsyncEnumerator<T> enumerator = enumerable.GetAsyncEnumerator();
try
{
    while (await enumerator.WaitForNextAsync())
    {
        while (true)
        {
            int item = enumerator.TryGetNext(out bool success);
            if (!success) break;
            Use(item);
        }
    }
}
finally { await enumerator.DisposeAsync(); }
```

Výhodou tohoto je dvě přeložení, jedna z nich a jedna hlavní:
- _Podverze: umožňuje enumerátoru podporovat více uživatelů_. Mohou nastat situace, kdy je to pro enumerátor užitečné pro podporu více souběžných uživatelů.  Tato funkce se nedá dosáhnout, když `MoveNextAsync` a `Current` jsou oddělené tak, že implementace nemůže učinit atomickou spotřebu.  Naproti tomu tento přístup poskytuje jedinou metodu `TryGetNext`, která podporuje vložení enumerátoru do dalšího a získání další položky, takže enumerátor může v případě potřeby povolit nedělitelnost.  Je však možné, že takové scénáře mohou být také povoleny tím, že každý příjemce získá vlastní enumerátor ze sdíleného výčtu.  Ještě nepotřebujeme vynutit, aby všechny enumerátory podporovaly souběžné používání, protože by to znamenalo netriviální režijních hlav, které to nevyžadují, což znamená, že se na příjemce rozhraní všeobecně nedaří spoléhat.
- _Hlavní: výkon_. `MoveNextAsync`/`Current` vyžaduje dvě volání rozhraní na operaci, zatímco nejlepší pro `WaitForNextAsync`/`TryGetNext` je, že většina iterací je dokončena synchronně a umožňuje úzkou vnitřní smyčku s `TryGetNext`, takže máme pouze jedno volání rozhraní na operaci.  To může mít měřitelný dopad v situacích, kdy rozhraní volá toto výpočetní prostředí.

Existují však netriviální downsides, včetně významně zvýšené složitosti při jejich využívání, a zvýšení pravděpodobnosti, že při jejich používání dojde k chybám.  A i když se výhody výkonu projeví v mikrosrovnávacích testech, nevěříme, že budou mít dopad na velká většina reálného využití.  Pokud dojde k jejich zapínání, můžeme současně zavést druhou sadu rozhraní.

Považované za zahozené možnosti:
- `ValueTask<bool> WaitForNextAsync(); bool TryGetNext(out T result);`: parametry `out` nemohou být kovariantní.  Je zde také malý dopad (problém se vzorem try obecně), který se tím nejspíš zavede jako bariéra zápisu za běhu pro výsledky referenčního typu.

#### <a name="cancellation"></a>Zrušení

Existuje několik možných přístupů k podpoře zrušení:
1. `IAsyncEnumerable<T>`/`IAsyncEnumerator<T>` zrušení – nezávislá: `CancellationToken` se nezobrazí kdekoli.  Zrušení je dosaženo logicky pečením `CancellationToken` do vyčíslitelného a/nebo enumerátoru jakýmkoli způsobem, například při volání iterátoru, předání `CancellationToken` jako argumentu metodě iterátoru a jeho použití v těle iterátoru, jak je provedeno s jakýmkoli jiným parametrem.
2. `IAsyncEnumerator<T>.GetAsyncEnumerator(CancellationToken)`: předáváte `CancellationToken` `GetAsyncEnumerator`a další `MoveNextAsync` operace se dotýkají IT oddělení.
3. `IAsyncEnumerator<T>.MoveNextAsync(CancellationToken)`: předáte `CancellationToken` každému jednotlivému volání `MoveNextAsync`.
4. 1 & & 2: oba vložíte `CancellationToken`do výčtu nebo enumerátoru a předáte je `CancellationToken`do `GetAsyncEnumerator`.
5. 1 & & 3: oba vložíte `CancellationToken`y do výčtu nebo enumerátoru a předáte `CancellationToken`do `MoveNextAsync`.

Z čistě teoretického hlediska je (5) nejvíc robustní, v tom, že (a) `MoveNextAsync` přijímá `CancellationToken` umožňuje nejvíce jemně odstupňovanou kontrolu nad tím, co se zrušilo a (b) `CancellationToken` je pouze jakýkoli jiný typ, který se může předat jako argument iterátorům, vkládat do libovolného typu atd.

Existuje však několik problémů s tímto přístupem:
- Jakým způsobem `CancellationToken` předává, aby `GetAsyncEnumerator` převedl do těla iterátoru?  Mohli bychom vystavit nové klíčové slovo `iterator`, na které byste se mohli dostat z, abyste získali přístup k `CancellationToken` předanému `GetEnumerator`. ale) to je spousta dalších strojových strojů, b) vytvoříme ho velmi první občan a c) 99% případ by byl stejný kód při volání iterátoru a volání `GetAsyncEnumerator`. v takovém případě může pouze předat `CancellationToken` jako argument do metody.
- Jak se předává `CancellationToken` `MoveNextAsync` dostat do těla metody?  To je ještě horší, jako kdyby bylo vystaveno z `iterator` lokálnímu objektu, jeho hodnota se může změnit v rámci await, což znamená, že jakýkoliv kód, který se zaregistruje s tokenem, bude muset zrušit registraci před čekáním a pak znovu zaregistrovat po; je také možné, že by bylo potřeba takové registrace a zrušení registrace při každém `MoveNextAsync` volání, bez ohledu na to, zda je implementováno kompilátorem v iterátoru nebo vývojářem ručně.
- Jak vývojář zruší `foreach` smyčka?  Pokud to provedete tak, že `CancellationToken` přidělíte výčtu výčtů nebo enumerátorů, potřebujeme, aby podporovala `foreach`i nad enumerátory, díky tomu, aby se stali vašimi občany první třídy, a teď je potřeba začít myslet na ekosystém sestavený kolem enumerátorů (např. metody LINQ `WithCancellation`) nebo b) musíme vložit `CancellationToken` do výčtu, aby bylo možné `IAsyncEnumerable<T>`, která by uložil poskytnutý token, a předejte ho do `GetAsyncEnumerator` se zabaleným výčtem, když `GetAsyncEnumerator` ve vrácené struktuře. je vyvolána (ignorování tohoto tokenu).  Nebo můžete pouze použít `CancellationToken` v těle foreach.
- Pokud jsou podporovány porozumění dotazů, jak by byl `CancellationToken` poskytnut `GetEnumerator` nebo `MoveNextAsync` předávat do každé klauzule?  Nejjednodušší způsob, jak by měla být klauzule zachytávání, a v takovém případě jakýkoli token je předán `GetAsyncEnumerator`/`MoveNextAsync` je ignorován.

Starší verze tohoto dokumentu doporučila (1), ale od té doby jsme přešli na (4).

Dva hlavní problémy s (1):
- výrobci výčtů zrušit musí implementovat nějaký často používaný text a můžou využít jenom podporu kompilátoru pro asynchronní iterátory k implementaci `IAsyncEnumerator<T> GetAsyncEnumerator(CancellationToken)` metody.
- je možné, že velký počet výrobců by se chtěl místo toho přidat parametr `CancellationToken` do jejich Async-vyčíslitelné signatury, což zabrání uživatelům v předání tokenu zrušení, který chtějí, pokud budou mít `IAsyncEnumerable` typ.

Existují dva hlavní scénáře spotřeby:
1. `await foreach (var i in GetData(token)) ...`, kde příjemce volá metodu asynchronního iterátoru,
2. `await foreach (var i in givenIAsyncEnumerable.WithCancellation(token)) ...`, kde se příjemce zabývá daným `IAsyncEnumerable` instancí.

Zjistili jsme, že přiměřená ohrožení pro podporu obou scénářů způsobem, který je pohodlný pro producenta i spotřebitele asynchronních datových proudů, je použití speciálně nakomentovaného parametru v metodě Async-iterátoru. Atribut `[EnumeratorCancellation]` se používá pro tento účel. Umístění tohoto atributu pro parametr instruuje kompilátor, že pokud je token předán metodě `GetAsyncEnumerator`, musí být tento token použit místo hodnoty původně předané pro parametr.

Zvažte `IAsyncEnumerable<int> GetData([EnumeratorCancellation] CancellationToken token = default)`. Implementátor této metody může jednoduše použít parametr v těle metody. Příjemce může použít vzorce spotřeby výše:
1. Použijete-li `GetData(token)`, je token uložen do asynchronního výčtu a bude použit v iteraci,
2. Použijete-li `givenIAsyncEnumerable.WithCancellation(token)`, bude token předaný `GetAsyncEnumerator` nahrazen všemi tokeny uloženými v asynchronním výčtu.

## <a name="foreach"></a>foreach

`foreach` bude rozšířena tak, aby podporovala `IAsyncEnumerable<T>` kromě stávající podpory pro `IEnumerable<T>`.  A bude podporovat ekvivalent `IAsyncEnumerable<T>` jako vzor, pokud jsou relevantní členové veřejně vystaveni, návrat k použití rozhraní přímo, pokud není, aby bylo možné povolit rozšíření založená na struktuře, které se vyhne přidělení, a také použití alternativních awaitables jako návratový typ `MoveNextAsync` a `DisposeAsync`.

### <a name="syntax"></a>Syntaxe

Pomocí syntaxe:

```csharp
foreach (var i in enumerable)
```

C#bude nadále zacházet se `enumerable` jako synchronní výčty, takže i když zveřejňuje relevantní rozhraní API pro asynchronní výčty (vystavení vzoru nebo implementace rozhraní), bude uvažovat pouze o synchronních rozhraních API.

Chcete-li vynutit `foreach` pouze v případě, že je třeba použít asynchronní rozhraní API, `await` vložena takto:

```csharp
await foreach (var i in enumerable)
```

Neposkytla se žádná syntaxe, která by podporovala buď rozhraní API pro asynchronní nebo synchronizační použití. Vývojář musí zvolit v závislosti na použité syntaxi.

Považované za zahozené možnosti:
- _`foreach (var i in await enumerable)`_ : Toto je již platná syntaxe a změna jejího významu by byla zásadní změnou.  To znamená, že `await` `enumerable`se z něj provede synchronním přechodem a pak synchronně projde to.
- _`foreach (var i await in enumerable)`, `foreach (var await i in enumerable)``foreach (await var i in enumerable)`_ : Tyto všechny navrhují, že čekáme na další položku, ale v příkazu foreach existují i jiné očekávání, zejména pokud je výčtový `IAsyncDisposable`, bude `await`"jejich asynchronní vyřazení.  Tento příkaz await je jako rozsah sady foreach, nikoli pro každý jednotlivý prvek, a proto klíčové slovo `await` zachovává na úrovni `foreach`.  Další s tím, že je přidružená k `foreach` nám poskytuje způsob, jak popsat `foreach` s jiným termínem, například "await foreach".  Důležitější je však, že existuje hodnota v zvažování `foreach` syntaxe ve stejnou dobu jako `using` syntaxe, aby zůstaly vzájemně konzistentní a `using (await ...)` je již platná syntaxe.
- _`foreach await (var i in enumerable)`_

Pořád zvažte:
- `foreach` dnes nepodporuje iterace prostřednictvím enumerátoru.  Očekáváme, že bude častější, že budete mít `IAsyncEnumerator<T>`s využitím, takže se bude považovat za podporu `await foreach` s `IAsyncEnumerable<T>` i `IAsyncEnumerator<T>`.  Ale jakmile tuto podporu přidáte, zavádíme otázku, zda `IAsyncEnumerator<T>` je občanem první třídy a zda musíme mít přetížení kombinátory, které pracují s enumerátory kromě výčtů?    Chceme místo výčtu vyzvat metody pro vrácení čítačů? Tento postup bychom měli dál diskutovat.  Pokud se rozhodnete, že ji nechcete podporovat, můžeme zavést metodu rozšíření `public static IAsyncEnumerable<T> AsEnumerable<T>(this IAsyncEnumerator<T> enumerator);`, která by umožnila stále `foreach`výčtu.  Pokud se rozhodnete, že ji chceme podporovat, je nutné také rozhodnout, zda `await foreach` být odpovědni za volání `DisposeAsync` na enumerátoru, a odpověď je pravděpodobně "No, měla by být řízení vyřazení zpracována ve stejném znění jako `GetEnumerator`."

### <a name="pattern-based-compilation"></a>Kompilace založená na vzorcích

Kompilátor vytvoří propojení s rozhraními API založenými na vzorech, pokud existují, přičemž před použitím rozhraní (vzor může být spokojen s metodami instance nebo metodami rozšíření).  Požadavky pro vzor:
- Vyčíslitelné musí vystavit `GetAsyncEnumerator` metodu, která může být volána bez argumentů a vrátí enumerátor, který splňuje příslušný vzor.
- Enumerátor musí vystavit `MoveNextAsync` metodu, která může být volána bez argumentů a vrátí něco, co může být `await`Ed a jehož `GetResult()` vrací `bool`.
- Enumerátor musí také zveřejnit vlastnost `Current`, jejíž getter vrátí `T` reprezentující druh dat, která jsou vytvářena.
- Enumerátor může volitelně vystavit `DisposeAsync` metodu, která může být vyvolána bez argumentů a vrátí něco, co může být `await`Ed a jehož `GetResult()` vrací `void`.

Tento kód:

```csharp
var enumerable = ...;
await foreach (T item in enumerable)
{
   ...
}
```

je přeložen na ekvivalent:

```csharp
var enumerable = ...;
var enumerator = enumerable.GetAsyncEnumerator();
try
{
    while (await enumerator.MoveNextAsync())
    {
       T item = enumerator.Current;
       ...
    }
}
finally
{
    await enumerator.DisposeAsync(); // omitted, along with the try/finally, if the enumerator doesn't expose DisposeAsync
}
```

Pokud iterace typu nevystavuje správný vzor, budou použita rozhraní.

### <a name="configureawait"></a>ConfigureAwait

Tato kompilace založená na vzorech umožní použít `ConfigureAwait` k použití na všech operátorech await prostřednictvím metody rozšíření `ConfigureAwait`:

```csharp
await foreach (T item in enumerable.ConfigureAwait(false))
{
   ...
}
```

Tato akce bude založena na typech, které přidáme také do .NET, což je pravděpodobně System. Threading. Tasks. Extensions. dll:

```csharp
// Approximate implementation, omitting arg validation and the like
namespace System.Threading.Tasks
{
    public static class AsyncEnumerableExtensions
    {
        public static ConfiguredAsyncEnumerable<T> ConfigureAwait<T>(this IAsyncEnumerable<T> enumerable, bool continueOnCapturedContext) =>
            new ConfiguredAsyncEnumerable<T>(enumerable, continueOnCapturedContext);

        public struct ConfiguredAsyncEnumerable<T>
        {
            private readonly IAsyncEnumerable<T> _enumerable;
            private readonly bool _continueOnCapturedContext;

            internal ConfiguredAsyncEnumerable(IAsyncEnumerable<T> enumerable, bool continueOnCapturedContext)
            {
                _enumerable = enumerable;
                _continueOnCapturedContext = continueOnCapturedContext;
            }

            public ConfiguredAsyncEnumerator<T> GetAsyncEnumerator() =>
                new ConfiguredAsyncEnumerator<T>(_enumerable.GetAsyncEnumerator(), _continueOnCapturedContext);

            public struct Enumerator
            {
                private readonly IAsyncEnumerator<T> _enumerator;
                private readonly bool _continueOnCapturedContext;

                internal Enumerator(IAsyncEnumerator<T> enumerator, bool continueOnCapturedContext)
                {
                    _enumerator = enumerator;
                    _continueOnCapturedContext = continueOnCapturedContext;
                }

                public ConfiguredValueTaskAwaitable<bool> MoveNextAsync() =>
                    _enumerator.MoveNextAsync().ConfigureAwait(_continueOnCapturedContext);

                public T Current => _enumerator.Current;

                public ConfiguredValueTaskAwaitable DisposeAsync() =>
                    _enumerator.DisposeAsync().ConfigureAwait(_continueOnCapturedContext);
            }
        }
    }
}
```

Všimněte si, že tento přístup nebude umožňovat použití `ConfigureAwait` s výčty založenými na vzorcích, ale pak už to není možné, ale v případě, že je `ConfigureAwait` vystavená jenom rozšíření na `Task`/`Task<T>`/`ValueTask`/`ValueTask<T>` a nedá se použít na libovolné neočekávané věci, protože to má smysl jenom při použití na úlohy (řídí chování implementované v rámci podpory pokračování úlohy), a proto nedává smysl při použití modelu, který může být neočekávanými úkoly.  Každý, kdo vrací očekávané věci, může v takových pokročilých scénářích poskytnout své vlastní chování.

(Pokud můžeme mít nějaký způsob, jak podporovat obor nebo `ConfigureAwait` řešení na úrovni sestavení, pak to nebude nutné.)

## <a name="async-iterators"></a>Asynchronní iterátory

Jazyk/kompilátor bude podporovat vytváření `IAsyncEnumerable<T>`s a `IAsyncEnumerator<T>`s, aby je bylo možné spotřebovat. V současné době jazyk podporuje zápis iterátoru jako:

```csharp
static IEnumerable<int> MyIterator()
{
    try
    {
        for (int i = 0; i < 100; i++)
        {
            Thread.Sleep(1000);
            yield return i;
        }
    }
    finally
    {
        Thread.Sleep(200);
        Console.WriteLine("finally");
    }
}
```

ale v těle těchto iterátorů se `await` nedají použít.  Přidáme tuto podporu.

### <a name="syntax"></a>Syntaxe

Stávající jazyková podpora pro iterátory odvodí iterátor metody v závislosti na tom, zda obsahuje `yield`s.  Totéž bude platit pro asynchronní iterátory.  Takové asynchronní iterátory budou vyohraničeny a odlišeny od synchronních iterátorů prostřednictvím přidání `async` k podpisu a musí také mít `IAsyncEnumerable<T>` nebo `IAsyncEnumerator<T>` jako svůj návratový typ.  Například výše uvedený příklad může být zapsán jako asynchronní iterátor následujícím způsobem:

```csharp
static async IAsyncEnumerable<int> MyIterator()
{
    try
    {
        for (int i = 0; i < 100; i++)
        {
            await Task.Delay(1000);
            yield return i;
        }
    }
    finally
    {
        await Task.Delay(200);
        Console.WriteLine("finally");
    }
}
```

Uvažované alternativy:
- _Nepoužívá `async` v signatuře_: použití `async` je pravděpodobná kompilátorem, protože používá k určení, zda je `await` v daném kontextu platný.  Ale i v případě, že to není nutné, jsme navázali, že `await` lze použít pouze v metodách označených jako `async`a zdá se, že je důležité zachovat konzistenci.
- _Povolují se vlastní tvůrci pro `IAsyncEnumerable<T>`_ : to je něco, co můžeme v budoucnu najít, ale strojní zařízení je složité a nepodporujeme ho pro synchronní protějšky.
- Použití _klíčového slova `iterator` v signatuře_: asynchronní iterátory by používaly `async iterator` v signatuře a `yield` lze použít pouze v `async` metodách, které zahrnují `iterator`; `iterator` by pak byly volitelné na synchronních iterátorech.  V závislosti na perspektivě má tato výhoda výhodu, že by signatura metody měla být velmi jasné, ať už je povolená `yield` a zda je metoda ve skutečnosti určena pro návrat instancí typu `IAsyncEnumerable<T>` spíše než kompilátor výroby, na základě toho, zda kód používá `yield` nebo ne.  Ale liší se od synchronních iterátorů, které nevyžadují a není možné je provést.  Navíc někteří vývojáři nevypadají jako další syntaxe.  Pokud jsme ho navrhuji od začátku, pravděpodobně jsme to vyžádali, ale v tomto okamžiku existuje mnohem více hodnot v udržování asynchronních iterátorů blízko k synchronizaci iterátorů.

## <a name="linq"></a>LINQ

Existuje více než ~ 200 přetížení metod na `System.Linq.Enumerable` třídy, přičemž všechna fungují v `IEnumerable<T>`. Některé z těchto přijetí `IEnumerable<T>`, některé z nich vyprodukuje `IEnumerable<T>`a mnoho z nich.  Přidání podpory LINQ pro `IAsyncEnumerable<T>` by mohlo vést k duplikaci všech těchto přetížení pro jinou ~ 200.  A vzhledem k tomu, že `IAsyncEnumerator<T>` je pravděpodobně běžnější jako samostatná entita v asynchronním světě, než `IEnumerator<T>` je na synchronním světě, můžeme potenciálně potřebovat další přetížení ~ 200, která pracují s `IAsyncEnumerator<T>`.  Kromě toho velký počet přetížení vede k predikátům (například `Where`, které přebírají `Func<T, bool>`), a může být žádoucí, aby bylo přetížení založené na `IAsyncEnumerable<T>`, které se zabývat synchronním i asynchronním predikátem (například `Func<T, ValueTask<bool>>` `Func<T, bool>`).  I když to neplatí pro všechna z nich. ~ 400 nová přetížení, hrubý výpočet je, že by měl být použitelný na polovinu, což znamená další ~ 200 přetížení, celkem ~ 600 nové metody.

To je rovnoměrné rozmístění počtu rozhraní API, které je možné ještě více při zvážení knihoven rozšíření, jako jsou interaktivní rozšíření (IX).  Ale IX už má implementaci mnoha z nich a zdá se, že nebudete mít velký důvod na duplikaci této práce; místo toho doporučujeme komunitě vylepšit IX a doporučit ji, když vývojáři chtějí používat LINQ s `IAsyncEnumerable<T>`.

K dispozici je také chyba syntaxe porozumění dotazu.  Porovnávací metoda porozumění dotazu by jim umožnila "pracovat" s některými operátory, např. Pokud IX poskytuje následující metody:

```csharp
public static IAsyncEnumerable<TResult> Select<TSource, TResult>(this IAsyncEnumerable<TSource> source, Func<TSource, TResult> func);
public static IAsyncEnumerable<T> Where(this IAsyncEnumerable<T> source, Func<T, bool> func);
```

pak tento C# kód bude "jenom fungovat":

```csharp
IAsyncEnumerable<int> enumerable = ...;
IAsyncEnumerable<int> result = from item in enumerable
                               where item % 2 == 0
                               select item * 2;
```

Neexistuje však žádná syntaxe porozumění dotazu, která podporuje použití `await` v klauzulích, takže pokud bylo přidáno IX, například:

```csharp
public static IAsyncEnumerable<TResult> Select<TSource, TResult>(this IAsyncEnumerable<TSource> source, Func<TSource, ValueTask<TResult>> func);
```

pak by to bylo "jenom fungovat":

```csharp
IAsyncEnumerable<string> result = from url in urls
                                  where item % 2 == 0
                                  select SomeAsyncMethod(item);

async ValueTask<int> SomeAsyncMethod(int item)
{
    await Task.Yield();
    return item * 2;
}
```

ale neexistuje žádný způsob, jak ho zapsat pomocí `await` vloženého do klauzule `select`.  V rámci samostatného úsilí jsme se mohli podívat na přidání `async { ... }`ch výrazů do jazyka. v tomto bodě bychom mohli dovolit použití v porozumění dotazům a výše uvedené místo toho může být zapsáno jako:

```csharp
IAsyncEnumerable<int> result = from item in enumerable
                               where item % 2 == 0
                               select async
                               {
                                   await Task.Yield();
                                   return item * 2;
                               };
```

nebo pokud chcete povolit použití `await` přímo ve výrazech, jako je podpora `async from`.  Je ale nepravděpodobné, že by to mělo vliv na zbytek funkce nastavenou jedním ze způsobů nebo na druhou, a to není zvlášť vysoká hodnota pro investici do té chvíle, takže v tomto návrhu není ještě nic dalšího.

## <a name="integration-with-other-asynchronous-frameworks"></a>Integrace s dalšími asynchronními rozhraními

Integrace s `IObservable<T>` a dalšími asynchronními rozhraními (např. reaktivní streamy) by se měla provádět na úrovni knihovny, nikoli na úrovni jazyka.  Všechna data z `IAsyncEnumerator<T>` lze například publikovat do `IObserver<T>` jednoduše pomocí `await foreach`"ze enumerátoru a `OnNext`" dat do pozorovatele, aby mohla být metoda rozšíření `AsObservable<T>` možná.  Použití `IObservable<T>` v `await foreach` vyžaduje ukládání dat do vyrovnávací paměti (v případě, že se stále zpracovává předchozí položka), ale takový adaptér Push-Pull lze snadno implementovat, aby bylo možné `IObservable<T>` získat z `IAsyncEnumerator<T>`.  Atd.  RX/IX již poskytuje prototypy těchto implementací a knihovny jako https://github.com/dotnet/corefx/tree/master/src/System.Threading.Channels poskytují různé druhy datových struktur vyrovnávací paměti.  Jazyk nemusí být v této fázi zahrnut.
