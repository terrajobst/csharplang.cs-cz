---
ms.openlocfilehash: 1457c1eb018e12af30ce5b38be704bf8851d4b25
ms.sourcegitcommit: 7f7fc6e9e195e51b7ff8229aeaa70aa9fbbb63cb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/10/2019
ms.locfileid: "79485012"
---
# <a name="efficient-params-and-string-formatting"></a>Efektivní parametry a formátování řetězců

## <a name="summary"></a>Souhrn
Tato kombinace funkcí zvýší efektivitu formátování `string` hodnoty a předávání argumentů stylu `params`.

## <a name="motivation"></a>Motivační
Režie přidělení formátování `string` hodnoty může podsáhnout výkon mnoha textových aplikací: od pokuty `struct` typů, `object[]` přidělení pro `params` a průběžné `string` alokace během `string.Format` volání. Aby bylo možné zajistit efektivitu takových aplikací, často potřebují opustit funkce pro zvýšení produktivity, jako je například `params` a `string` interpolace a přechod na nestandardní, ručně kódovaná řešení. 

Jako příklad zvažte MSBuild. To je napsáno pomocí mnoha moderních C# funkcí vývojářům, kteří mají vědomu výkonu. V jednom z reprezentativních ukázek buildů MSBuild se ale pomocí minimální podrobností vygeneruje 262MB přidělení `string`. Z této 1/2 přidělení jsou krátkodobá přidělení v rámci `string.Format`. Tyto funkce by tyto funkce odebraly mnohem z těchto funkcí v prostředí .NET Desktop a v důsledku dostupnosti `Span<T>` tak téměř nulou v .NET Core.

Sada jazykových funkcí popsaných tady umožní aplikacím dál používat tyto funkce, a to s hodně nebo bez jakýchkoli změn základu aplikačního kódu a zároveň odebrat nezamýšlené nároky na přidělení ve většině případů.

## <a name="detailed-design"></a>Podrobný návrh 
Pro dosažení těchto výsledků se tady používá sada funkcí:

- Rozbalení `params` pro podporu širší sady typů kolekcí.
- Umožnění vývojářům přizpůsobit, jak je dosaženo `string` interpolace. 
- Umožnění interpolace `string` vázání na efektivnější `string.Format` přetížení.

### <a name="extending-params"></a>Rozšiřování parametrů
Jazyk umožní `params` v signatuře metody mít typy `Span<T>`, `ReadOnlySpan<T>` a `IEnumerable<T>`. Stejná pravidla pro vyvolání se vztahují na tyto nové typy, které platí pro `params T[]`:

- Nejde přetížit, kde jediným rozdílem je klíčové slovo `params`.
- Lze vyvolat předáním řady argumentů, které jsou implicitně konvertibilníelné na `T` nebo jednoho `Span<T>` / 
`ReadOnlySpan<T>`argument  / `IEnumerable<T>`.
- Musí se jednat o poslední parametr v signatuře metody.
- Atd... 

Varianty `Span<T>` a `ReadOnlySpan<T>` se pro jednoduchost označují jako `Span<T>` níže. V případech, kdy se chování `ReadOnlySpan<T>` liší, bude explicitně vyvoláno. 

Výhodou `Span<T>` varianty `params` poskytuje kompilátor skvělou flexibilitu při přidělování záložního úložiště pro hodnotu `Span<T>`. U `params T[]` kompilátor musí přidělit nové `T[]` pro každé vyvolání metody `params`. Opakované použití není možné, protože musí předpokládat volaný uložený a znovu použitý parametr. To může vést k velké neefektivitě v metodách s velkým množstvím `params`ch volání.

Zadané varianty `Span<T>` jsou `ref struct` volaný nemůže uložit argument. Kompilátor proto může optimalizovat lokality voláním pomocí akcí, jako je opakované použití argumentu. Díky tomu může být opakované vyvolání ve srovnání s `T[]`velmi efektivní. Jazyk ale neposkytuje žádné zvláštní záruky týkající se toho, jak jsou tyto callsites optimalizované. Všimněte si, že kompilátor je zdarma používat jiné hodnoty než `T[]` při vyvolání metody `params Span<T>`. 

Jedna taková možná implementace je následující. Zvažte všechna `params` vyvolání v těle metody. Kompilátor může přidělit pole, které má velikost rovnou největšímu `params` vyvolání, a použít ho pro všechna vyvolání vytvořením vhodně se `Span<T>` instancí v poli. Příklad:

``` csharp
static class OneAllocation {
    static void Use(params Span<string> spans) {
        ...
    }

    static void Go() {
        Use("jaredpar");
        Use("hello", "world");
        Use("a", "longer", "set");
    }
}
```

Kompilátor může zvolit, aby vygeneroval tělo `Go` následujícím způsobem:

``` csharp
    static void Go() {
        var args = new string[3];
        args[0] = "jaredpar";
        Use(new Span<string>(args, start: 0, length: 1));

        args[0] = "hello";
        args[1] = "world";
        Use(new Span<string>(args, start: 0, length: 2));

        args[0] = "a";
        args[1] = "longer";
        args[2] = "set";
        Use(new Span<string>(args, start: 0, length: 3));
   }
```

To může významně snížit počet polí přidělených v aplikaci. Přidělení se dá ještě víc snížit, pokud modul runtime poskytuje nástroje pro vyřazení polí do paměti pro inteligentnější zásobník.

Tuto optimalizaci nelze vždy použít, i když. I když volaný nemůže zachytit argument `params`, může být stále zachycena v případě, že existuje `ref` nebo parametr `out / ref`, který je sám `ref struct` typu. 

``` csharp
static class SneakyCapture {
    static ref int M(params Span<T> span) => ref span[0];

    static void Oops() {
        // This now holds onto the memory backing the Span<T> 
        ref int r = ref M(42);
    }
}
```

Tyto případy jsou staticky zjistitelné i přesto. K tomu může dojít, když se `ref` vrátí nebo `ref struct` parametr předaný `out` nebo `ref`. V takovém případě musí kompilátor přidělit nové `T[]` pro každé vyvolání. 

Na konci tohoto dokumentu je vysvětleno několik dalších možných strategií optimalizace.

`IEnumerable<T>` variant je pouze pohodlíně přetížení. To je užitečné ve scénářích, které často využívají `IEnumerable<T>`, ale také mají spoustu `params` využití. Při vyvolání ve formě argumentu `T` se záložní úložiště přidělí jako `T[]`, stejně jako v současné době `params T[]`.

### <a name="params-overload-resolution-changes"></a>změny rozlišení přetížení parametrů
Tento návrh znamená, že jazyk teď má čtyři varianty `params`, kde předtím, než nějaký byl. Je rozumné pro metody definování přetížení metod, které se liší pouze u typu deklarace `params`. 

Vezměte v úvahu, že `StringBuilder.AppendFormat` by jistě kromě `params object[]`musel přidat přetížení `params ReadOnlySpan<object>`. To by umožnilo významně zvýšit výkon snížením přidělení kolekce bez nutnosti jakýchkoli změn volajícího kódu. 

Aby bylo možné tento jazyk usnadnit, zavede následující pravidlo pro přerušení propojení v rámci rozlišení přetížení. Pokud se kandidátské metody liší jenom parametrem `params`, pak kandidáti budou upřednostňováni v následujícím pořadí:

1. `ReadOnlySpan<T>`
1. `Span<T>`
1. `T[]`
1. `IEnumerable<T>`

Toto pořadí je co nejmenších nejúčinnějších pro obecný případ.

### <a name="variant"></a>Varianty
CoreFX je prototypování nového spravovaného typu s názvem [variant](https://github.com/dotnet/corefxlab/pull/2595). Tento typ je určen pro použití v rozhraních API, které očekávají heterogenní hodnoty, ale nechtějí režii, která se používá `object` jako parametr. Typ `Variant` poskytuje univerzální úložiště, ale nebrání přidělení zabalení pro nejčastěji používané typy. Použití tohoto typu v rozhraních API, jako je `string.Format`, může eliminovat režijní náklady ve většině případů.

Tento typ samotný není nutně speciální pro jazyk. V tomto dokumentu se zavádí samostatně, ale v takovém případě se jedná o podrobnosti o implementaci jiných částí návrhu. 

### <a name="efficient-interpolated-strings"></a>Efektivní interpolované řetězce
Interpolované řetězce jsou zatím neefektivní funkce v C#. Nejběžnější syntaxe s použitím interpolované `string` jako `string`se přeloží na `string.Format(string, params object[])` volání. To bude mít za následek přidělení zabalení pro všechny typy hodnot, zprostředkující `string` přidělení v rámci implementace do značné míry používá `object.ToString` pro formátování a také přidělení pole, když počet argumentů překročí množství parametrů u přetížení "Fast" `string.Format`. 

Jazyk změní svou interpolaci na nižší, aby bylo možné zvážit alternativní přetížení `string.Format`. Bude uvažovat o všech formulářích `string.Format(string, params)` a vybrat přetížení "nejlepší", které splňuje typy argumentů.
Přetížení "nejlepšího" `params` bude určeno pravidly uvedenými výše. To znamená, že interpolovaná `string` se teď můžou navazovat na velmi efektivní přetížení, jako je `string.Format(string format, params ReadOnlySpan<Variant> args)`. V mnoha případech se tím odeberou všechny mezilehlé alokace.

### <a name="customizable-interpolated-strings"></a>Přizpůsobitelné interpolované řetězce
Vývojáři mohou přizpůsobit chování interpolované řetězce pomocí `FormattableString`. Obsahuje data, která přecházejí do interpolované řetězce: formát `string` a argumenty jako pole. I když má stále přidělení pole zabalení a argumentu a také přidělení pro `FormattableString` (Jedná se o `abstract class`). Proto se pro aplikace, které jsou ve formátování `string`, nepoužívá.

Chcete-li efektivně zformátovat formátování řetězce, bude jazyk rozpoznávat nový typ: `System.ValueFormattableString`. Všechny interpolované řetězce budou mít převod cílového typu na tento typ. To bude implementováno posunutím interpolované řetězce do volání `ValueFormattableString.Create` přesně tak, jak je provedeno pro `FormattableString.Create` dnes. Jazyk bude podporovat všechny možnosti `params` popsané v tomto dokumentu při hledání nejvhodnější `ValueFormattableString.Create` metody. 

``` csharp
readonly struct ValueFormattableString {
    public static ValueFormattableString Create(Variant v) { ... } 
    public static ValueFormattableString Create(string s) { ... } 
    public static ValueFormattableString Create(string s, params ReadOnlySpan<Variant> collection) { ... } 
}

class ConsoleEx { 
    static void Write(ValueFormattableString f) { ... }
}

class Program { 
    static void Main() { 
        ConsoleEx.Write(42);
        ConsoleEx.Write($"hello {DateTime.UtcNow}");

        // Translates into 
        ConsoleEx.Write(ValueFormattableString.Create((Variant)42));
        ConsoleEx.Write(ValueFormattableString.Create(
            "hello {0}", 
            new Variant(DateTime.UtcNow));
    }
}
```

Pravidla rozlišení přetížení budou změněna tak, aby preferovat `ValueFormattableString` přes `string`, pokud je argumentem interpolovaná řetězec. To znamená, že bude užitečné mít přetížení, která se liší jenom `string` a `ValueFormattableString`. Toto přetížení dnes s `FormattableString` není užitečné, protože kompilátor vždy upřednostňuje `string` verzi (pokud vývojář nepoužívá explicitní přetypování). 

## <a name="open-issues"></a>Otevřené problémy

### <a name="valueformattablestring-breaking-change"></a>ValueFormattableStringá změna
Tato změna upřednostňuje `ValueFormattableString` během překladu přetěžování přes `string` je zásadní změna. Je možné, aby vývojář definoval typ nazvaný `ValueFormattableString` dnes a použil ho v přetížení metody s `string`. Tato navrhovaná změna způsobí, že kompilátor po implementaci této sady funkcí vybere jiné přetížení. 

Tato možnost se jeví jako rozumně nízká. Typ by potřeboval `System.ValueFormattableString` celé jméno a musí mít `static` metody s názvem `Create`. Vzhledem k tomu, že se vývojářům při definování libovolného typu v oboru názvů `System` důrazně nedoporučuje, toto přerušení se jeví jako rozumné ohrožení.

### <a name="expanding-to-more-types"></a>Rozšíření na více typů
Vzhledem k tomu, že jsme v oblasti, měli byste zvážit přidání `IList<T>`, `ICollection<T>` a `IReadOnlyList<T>` do sady kolekcí, pro které `params` podporuje. V souvislosti s implementací bude v rámci této jiné práce vyhodnocena malá část.

Správce logických disků se musí rozhodnout, jestli to bude mít komplikace v jazyce. Přidání `IEnumerable<T>` odstraní velmi konkrétní bod tření. Při nedostatku tohoto `params` řešení bylo při volání metody `params` nuceno přidělit `T[]` od `IEnumerable<T>`. Přidání `IEnumerable<T>` tuto situaci opravuje. V tuto chvíli není k dispozici žádný zvláštní bod dopravení. 

## <a name="considerations"></a>Požadavky

### <a name="variant2-and-variant3"></a>Variant2 a Variant3
Tým CoreFX má také nepřidělení sady typů úložiště pro až tři argumenty `Variant`. Jedná se o jeden `Variant``Variant2` a `Variant3`. Všechny mají dvojici metod pro získání bezplatné `Span<Variant>` mimo ně: `CreateSpan` a `KeepAlive`. To znamená, že u `params Span<Variant>` až tří argumentů může být lokalita volání zcela rozdělená.

``` csharp
static class ZeroAllocation {
    static void Use(params Span<Variant> spans) {
        ...
    }

    static void Go() {
        Use("hello", "world");
    }
}
```

Metodu `Go` lze snížit na následující:

``` csharp
static class ZeroAllocation {
    static void Go() {
        Variant2 _v;
        _v.Variant1 = new Variant("hello");
        _v.Variant2 = new Variant("word");
        Use(_v.CreateSpan());
        _v.KeepAlive();
    }
}
```

To vyžaduje hodně práce nad návrhem, aby bylo možné znovu použít `T[]` mezi voláními `params Span<T>`. Kompilátor už potřebuje ke správě dočasného za volání a vyčištění práce po (i když v jednom případě je pouze označení interního tempa jako bezplatné). 

Poznámka: funkce `KeepAlive` je nutná jenom na ploše. V .NET Core nebude metoda k dispozici, a proto Kompilátor negeneruje volání.

### <a name="clr-stack-allocation-helpers"></a>Pomocník pro přidělení zásobníku CLR
Modul CLR pouze poskytuje [Nastavení localloc](https://docs.microsoft.com/en-us/dotnet/api/system.reflection.emit.opcodes.localloc?redirectedfrom=MSDN&view=netframework-4.7.2) k alokaci zásobníku sousedící paměti. Tato instrukce je omezená, protože funguje jenom pro `unmanaged` typy. To znamená, že se nedá použít jako univerzální řešení pro efektivní přidělování záložního úložiště pro `params 
 Span<T>`. 

Toto omezení není některými základními omezeními, ale místo toho více než artefaktem historie. Modul CLR se může rozhodnout přidat nové operační kódy a vnitřní objekty, které poskytují univerzální přidělování zásobníku. Ty pak můžete použít k přidělení záložního úložiště pro většinu `params Span<T>` volání.

``` csharp
static class BetterAllocation {
    static void Use(params Span<string> spans) {
        ...
    }

    static void Go() {
        Use("hello", "world");
    }
}
```

Metodu `Go` lze snížit na následující:

``` csharp
static class ZeroAllocation {
    static void Go() {
        Span<T> span = RuntimeIntrinsic.StackAlloc<string>(length: 2);
        span[0] = "hello";
        span[1] = "world";
        Use(span);
    }
}
```

I když je tento přístup pro haldu velmi efektivní, způsobuje dodatečné použití zásobníku. V algoritmu, který má hlubokou `params` využití, může to způsobit, že se `StackOverflowException` vygeneruje, kde by bylo úspěšné přidělení jednoduchého `T[]`. 

Bohužel C# není nastaven pro typ analýzy mezi jednotlivými metodami, kde by mohla být kvalifikovaně rozhodnuto, zda má volání použít zásobník nebo přidělení haldy `params`. Může skutečně zvážit pouze každé volání.

CLR je nejvhodnějším nastavením pro tento typ určení za běhu. Proto by byl modul runtime pravděpodobně pro alokaci univerzálního zásobníku k dispozici dvěma způsoby:

1. `Span<T> StackAlloc<T>(int length)`: Toto má stejné chování a omezení `localloc` s tím rozdílem, že může pracovat na jakémkoli typu `T`. 
1. `Span<T> MaybeStackAlloc<T>(int length)`: Tento modul runtime se může rozhodnout implementovat pomocí zásobníku nebo přidělení haldy. Modul runtime pak může použít kontext spuštění, ve kterém je volána k určení způsobu přidělení `Span<T>`. Volající, ale bude ho vždycky nakládat, jako by byl přidělen zásobníku.

U velmi jednoduchých případů, jako je například jeden až dva argumenty C# , může kompilátor vždy použít `StackAlloc<T>` variantu. Ve většině případů není pravděpodobně významně přispívat k vyčerpání zásobníku. Pro jiné případy může kompilátor zvolit použití `MaybeStackAlloc<T>` místo toho, aby modul runtime mohl volání provést.

Způsob, jakým zvolíme, bude nejspíš potřebovat hlubší šetření a zkoumání reálných aplikací. Ale pokud jsou tyto nové vnitřní funkce dostupné, poskytne vám tento typ flexibility.

### <a name="why-not-varargs"></a>Proč neexistují VarArgs? 
Existující funkce [VarArgs](https://docs.microsoft.com/en-us/cpp/windows/variable-argument-lists-dot-dot-dot-cpp-cli?view=vs-2017) byla považována za možné řešení. Tato funkce se ale primárně používá pro C++scénáře/CLI a má známé otvory pro jiné scénáře. Při přenosu do systému UNIX se navíc významně účtují významné náklady. Proto se neviděl jako životaschopné řešení.

## <a name="related-issues"></a>Související problémy
Tato specifikace se týká následujících problémů: 

- https://github.com/dotnet/csharplang/issues/1757
- https://github.com/dotnet/csharplang/issues/179
- https://github.com/dotnet/corefxlab/pull/2595

