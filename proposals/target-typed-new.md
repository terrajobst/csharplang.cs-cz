---
ms.openlocfilehash: 07b4afe4a3fcbf10c978f05e642dfd8a47d53ea5
ms.sourcegitcommit: 194a043db72b9244f8db45db326cc82de6cec965
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/24/2020
ms.locfileid: "80217200"
---

# <a name="target-typed-new-expressions"></a>Výrazy `new` s cílovým typem

* Navrženo [x]
* [x] [prototyp](https://github.com/alrz/roslyn/tree/features/target-typed-new)
* [] Implementace
* [] Specifikace

## <a name="summary"></a>Souhrn
[summary]: #summary

Nevyžadovat specifikaci typu pro konstruktory, když je typ znám. 

## <a name="motivation"></a>Motivační
[motivation]: #motivation

Povoluje inicializaci pole bez duplikace typu.
```cs
Dictionary<string, List<int>> field = new() {
    { "item1", new() { 1, 2, 3 } }
};
```

Povolí vynechání typu, když se dá odvodit z použití.
```cs
XmlReader.Create(reader, new() { IgnoreWhitespace = true });
```

Vytvořte instanci objektu bez nutnosti hláskování typu.
```cs
private readonly static object s_syncObj = new();
```

## <a name="detailed-design"></a>Podrobný návrh
[design]: #detailed-design

Syntaxe *object_creation_expression* se upraví tak, aby byl *typ* volitelný, pokud jsou k dispozici závorky. Tato je nutná k vyřešení nejednoznačnosti pomocí *anonymous_object_creation_expression*.
```antlr
object_creation_expression
    : 'new' type? '(' argument_list? ')' object_or_collection_initializer?
    | 'new' type object_or_collection_initializer
    ;
```

`new` cílového typu je převoditelná na libovolný typ. V důsledku toho nepřispívá k řešení přetížení. To se většinou vyhnout nepředvídatelným změnám.

Seznam argumentů a inicializační výrazy budou vázány po určení typu.

Typ výrazu by byl odvozen z cílového typu, který by byl vyžadován pro jednu z následujících:

- **Libovolný typ struktury** (včetně typů řazené kolekce členů)
- **Jakýkoli typ odkazu** (včetně typů delegátů)
- **Libovolný parametr typu** s konstruktorem nebo omezením `struct`

s následujícími výjimkami:

- **Výčtové typy:** ne všechny typy výčtů obsahují konstantu nula, takže by měl být vhodné použít explicitního člena výčtu.
- **Typy rozhraní:** jedná se o funkci mezery a měla by být vhodnější výslovně uvést typ.
- **Typy polí:** pole potřebují pro zadání délky speciální syntaxi.
- **dynamické:** nepovolujeme `new dynamic()`, takže nepovolujeme `new()` s `dynamic` jako cílový typ.

Všechny ostatní typy, které nejsou povoleny v *object_creation_expression* jsou vyloučeny také, například typy ukazatelů.

Když je cílovým typem typ hodnoty s možnou hodnotou null, `new` cílový typ se převede na nadřízený typ místo typu s možnou hodnotou null.

> **Otevření problému:** Pokud chcete, abychom jako cílový typ povolili delegáty a řazené kolekce členů?

Výše uvedená pravidla zahrnují delegáty (typ odkazu) a řazené kolekce členů (typ struktury). I když jsou oba typy constructible, je-li typ odvozen, anonymní funkce nebo literál řazené kolekce členů již lze použít.
```cs
(int a, int b) t = new(1, 2); // "new" is redundant
Action a = new(() => {}); // "new" is redundant

(int a, int b) t = new(); // ruled out by "use of struct default constructor"
Action a = new(); // no constructor found
```

### <a name="miscellaneous"></a>Různé

`throw new()` není povoleno.

`new` cílového typu není povolený u binárních operátorů.

Není povoleno, není-li k dispozici žádný typ k cíli: unární operátory, kolekce `foreach`, v `using`ve formě dekonstrukce ve výrazu `await` jako vlastnost anonymního typu (`new { Prop = new() }`) v příkazu `lock`, v `sizeof`v příkazu `fixed` v příkazu`new().field`v rámci člena přístup (`someDynamic.Method(new())`) v dynamicky odesílané operaci (`is`) v dotazu LINQ jako Operand operátoru `??`, jako levý operand operátoru ,  ...

Nepovoluje se taky jako `ref`.

## <a name="drawbacks"></a>Nevýhody
[drawbacks]: #drawbacks

Došlo k nějakým potížím s cílovým typem `new` vytváření nových kategorií podstatných změn, ale už to máme s `null` a `default`a že nedošlo k významnému problému.

## <a name="alternatives"></a>Alternativy
[alternatives]: #alternatives

Většina stížností na typy, které jsou příliš dlouhé na duplicity při inicializaci pole, se týká *typů argumentů* , které nejsou samotný typ, můžeme odvodit jenom argumenty typu jako `new Dictionary(...)` (nebo podobné) a odvodit argumenty typu místně z argumentů nebo inicializátoru kolekce.

## <a name="questions"></a>Otázky
[questions]: #questions

- Pokud byste měli zakázat použití ve stromech výrazů? žádné
- Jak funkce komunikuje s argumenty `dynamic`? (žádné zvláštní zacházení)
- Jak má IntelliSense pracovat s `new()`? (pouze v případě, že existuje jeden cílový typ)

## <a name="design-meetings"></a>Schůzky pro návrh

- [LDM-2017-10-18](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-10-18.md#100)
- [LDM-2018-05-21](https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-05-21.md)
- [LDM-2018-06-25](https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-06-25.md)
- [LDM-2018-08-22](https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-08-22.md#target-typed-new)
- [LDM-2018-10-17](https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md)
