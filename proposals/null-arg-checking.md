---
ms.openlocfilehash: 76065293f652979ab395e131d657e44899c5a65b
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484550"
---
# <a name="simplified-null-argument-checking"></a>Zjednodušená kontrola argumentů null

## <a name="summary"></a>Souhrn
Tento návrh poskytuje zjednodušenou syntaxi pro ověřování argumentů metody, které nejsou `null` a jejich `ArgumentNullException` správně.

## <a name="motivation"></a>Motivační
Práce na návrhu referenčních typů s možnou hodnotou null nám způsobila kontrole kódu potřebného pro `null` ověřování argumentu. Vzhledem k tomu, že NRT nemá vliv na vývojáře spouštění kódu, musí stále přidávat kód `if (arg is null) throw`ch desek, a to i v projektech, které jsou plně `null` čisté. Díky tomu máme přání prozkoumat minimální syntaxi pro argument `null` ověřování v jazyce. 

I když se očekává, že se tato syntaxe pro ověřování parametrů `null`a často spáruje s NRT, že návrh je zcela nezávislý. Syntaxi lze použít nezávisle na direktivách `#nullable`.

## <a name="detailed-design"></a>Podrobný návrh 

### <a name="null-validation-parameter-syntax"></a>Syntaxe parametru ověřování null
Operátor vykřičník, `!`, může být umístěn za názvem parametru v seznamu parametrů a to způsobí, že C# kompilátor vygeneruje standardní kontrolu `null` kódu pro daný parametr. Tento postup se označuje jako `null` Syntaxe ověřovacího parametru. Příklad:

``` csharp
void M(string name!) {
    ...
}
```

Budou přeloženy do:

``` csharp
void M(string name) {
    if (name is null) {
        throw new ArgumentNullException(nameof(name));
    }
    ...
}
```

Vygenerovaná `null` se objeví před jakýmkoli vývojářem vytvořeným kódem v metodě. Pokud více parametrů obsahuje operátor `!`, budou kontroly provedeny ve stejném pořadí, ve kterém jsou deklarovány parametry.

``` csharp
void M(string p1, string p2) {
    if (p1 is null) {
        throw new ArgumentNullException(nameof(p1));
    }
    if (p2 is null) {
        throw new ArgumentNullException(nameof(p2));
    }
    ...
}
```

Tato kontrolu bude určeno konkrétně pro referenční rovnost na `null`, nevyvolává `==` ani uživatelsky definované operátory. To také znamená, že operátor `!` lze přidat pouze k parametrům, jejichž typ lze testovat pro rovnost před `null`. To znamená, že se nedá použít u parametru, jehož typ je známý jako typ hodnoty.

``` csharp
// Error: Cannot use ! on parameters who types derive from System.ValueType
void G<T>(T arg!) where T : struct {

}
```

V případě konstruktoru dojde k ověření `null` před jakýmkoli jiným kódem v konstruktoru. To zahrnuje: 

- Řetězení k jiným konstruktorům pomocí `this` nebo `base` 
- Inicializátory polí, které se implicitně vyskytují v konstruktoru

Příklad:

``` csharp
class C {
    string field = GetString();
    C(string name!): this(name) {
        ...
    }
}
```

Přibližně se převede na následující:

``` csharp
class C {
    C(string name)
        if (name is null) {
            throw new ArgumentNullException(nameof(name));
        }
        field = GetString();
        :this(name);
        ...
}
```

Poznámka: Toto není právní C# kód, ale stačí pouze aproximace toho, co implementace dělá. 

Syntaxe parametru `null` ověření bude také platná v seznamech parametrů lambda. To platí i v syntaxi jediného parametru, která postrádá Parens.

``` csharp
void G() {
    // An identity lambda which throws on a null input
    Func<string, string> s = x! => x;
}
```

Syntaxe je také platná u parametrů pro metody iterátoru. Na rozdíl od jiného kódu v iterátoru dojde k ověření `null`, když je vyvolána metoda iterátoru, ne při vás provedl základního výčtu. To platí pro tradiční nebo `async` iterátory.

``` csharp
class Iterators {
    IEnumerable<char> GetCharacters(string s!) {
        foreach (var c in s) {
            yield return c;
        }
    }

    void Use() {
        // The invocation of GetCharacters will throw
        IEnumerable<char> e = GetCharacters(null);
    }
}
```

Operátor `!` lze použít pouze pro seznamy parametrů, které mají související tělo metody. To znamená, že se nedá použít v `abstract` metodě, `interface`, `delegate` nebo `partial` definici metody.

### <a name="extending-is-null"></a>Rozšíření má hodnotu null.
Typy, pro které je výraz `is null` platný, budou rozšířeny tak, aby zahrnovaly neomezení parametrů typu. Tím umožníte, aby vyplnil záměr kontroly `null` u všech typů, které jsou kontrolní `null` platné. Konkrétně to znamená, že typy, které nejsou jednoznačně známy typů hodnot. Například parametry typu, které jsou omezeny na `struct` nelze použít s touto syntaxí.

``` csharp
void NullCheck<T1, T2>(T1 p1, T2 p2) where T2 : struct {
    // Okay: T1 could be a class or struct here.
    if (p1 is null) {
        ...
    }

    // Error 
    if (p2 is null) { 
        ...
    }
}
```

Chování `is null` u parametru typu bude stejné jako `== null` dnes. V případech, kdy je parametr typu vytvořen jako typ hodnoty, bude kód vyhodnocen jako `false`. Pro případy, kde je odkazový typ, bude kód provádět správnou `is null` kontrolu.

### <a name="intersection-with-nullable-reference-types"></a>Průnik s odkazy s možnou hodnotou null
Jakýkoli parametr, u kterého je použit operátor `!`, bude začínat ne`null`m stavu s možnou hodnotou null. To platí i v případě, že typ samotného parametru je potenciálně `null`. K tomu může dojít s explicitním typem s možnou hodnotou null, například řekněme `string?`nebo s neomezeným parametrem typu. 

Pokud je syntaxe `!` v parametrech kombinována s typem s možnou hodnotou null pro parametr, bude kompilátor vydávat upozornění:

``` csharp
void WarnCase<T>(
    string? name!, // Warning: combining explicit null checking with a nullable type
    T value1 // Okay
)
```

## <a name="open-issues"></a>Otevřené problémy
Žádná

## <a name="considerations"></a>Požadavky

### <a name="constructors"></a>Konstruktory
Generování kódu pro konstruktory znamená, že při přechodu ze standardního ověřování `null` dnes a `null` Syntaxe ověřovacího parametru (`!`) je k dispozici malá, ale nepozorovatelná změna chování. Ověřování na úrovni Standard `null` proběhne po inicializátorech polí a všech voláních `base` nebo `this`. To znamená, že vývojář nemůže nutně migrovat 100% jejich `null` ověřování na novou syntaxi. Konstruktory alespoň vyžadují určitou kontrolu.

Po diskuzi, i když bylo rozhodnuto, že to je velmi nepravděpodobné, že by došlo k významným problémům při přijímání. Je více logické, že `null` spustit před jakoukoli logikou v konstruktoru. Může znovu navštěvovat, pokud jsou zjištěny významné problémy s kompatibilitou.

### <a name="warning-when-mixing--and-"></a>Upozornění při smíchání? ani!
Při použití syntaxe `!` na parametr, který je explicitně zadaný na typ s možnou hodnotou null, se musí zadat upozornění, zda by mělo být vydáno upozornění. Na povrchu vypadá jako deklarace nesmyslná vývojářem, ale existují případy, kdy hierarchie typů můžou vynutit od vývojářů takovou situaci. 

Vezměte v úvahu následující hierarchii tříd v rámci řady sestavení (za předpokladu, že všechny jsou kompilovány s povolenou kontrolou `null`):

``` csharp
// Assembly1
abstract class C1 {
    protected abstract void M(object o); 
}

// Assembly2
abstract class C2 : C1 {

}

// Assembly3
abstract class C3 : C2 { 
    protected override void M(object o!) {
        ...
    }
}
```

Tady se autor `C3` rozhodl přidat `null` ověření do parametru `o`. To je zcela v souladu s tím, jak je funkce určena pro použití.

Nyní si představte, že se autor Assembly2 rozhodne přidat následující přepsání:

``` csharp
// Assembly2
abstract class C2 : C1 {
   protected override void M(object? o) { 
       ...
   }
}
```

To je povoleno pomocí typů odkazů s možnou hodnotou null, protože je právní, aby byl kontrakt flexibilnější pro vstupní pozice. Funkce NRT obecně umožňuje rozumným spolu/kontravariancem u parametrů nebo vrácení hodnoty null. Jazyk ale provede kontrolu souběžného/kontravariance na základě nejvíce konkrétního přepsání, nikoli původní deklarace. To znamená, že autor Assembly3 zobrazí upozornění týkající se typu `o` neshoduje a bude muset změnit signaturu na následující, aby se vyloučila: 

``` csharp
// Assembly3
abstract class C3 : C2 { 
    protected override void M(object? o!) {
        ...
    }
}
```

V tomto okamžiku má autor Assembly3 několik možností:

- Můžou přijmout nebo potlačit upozornění týkající se `object?` a `object` neshody.
- Můžou přijmout nebo potlačit upozornění týkající se `object?` a `!` neshody.
- Můžou jenom odebrat kontrolu ověřování `null` (odstranit `!` a provést explicitní kontrolu).

Jedná se o reálný scénář, ale je teď nápad, že se vám upozornění přesune vpřed. Pokud se upozornění stane častěji, než jsme předpokládali, můžeme ho později odebrat (obráceně není pravda).

### <a name="implicit-property-setter-arguments"></a>Argumenty setter pro implicitní vlastnost
Argument `value` parametru je implicitní a nezobrazuje se v žádném seznamu parametrů. To znamená, že nemůže být cílem této funkce. Syntaxe setter vlastnosti se dá rozšířit tak, aby zahrnovala seznam parametrů, aby bylo možné použít operátor `!`. Ale to se podílí na nápadu této funkce, která usnadňuje `null` ověřování. Vzhledem k tomu, že argument implicitní `value` nebude s touto funkcí fungovat.

## <a name="future-considerations"></a>Budoucí požadavky
Žádná
