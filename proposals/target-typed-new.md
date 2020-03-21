---
ms.openlocfilehash: 38740069a2e105f920fa275c443f4560055e2901
ms.sourcegitcommit: 9aa177443b83116fe1be2ab28e2c7291947fe32d
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/21/2020
ms.locfileid: "80108364"
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

### Miscellaneous

`throw new()` is disallowed.

Target-typed `new` is not allowed with binary operators.

It is disallowed when there is no type to target: unary operators, collection of a `foreach`, in a `using`, in a deconstruction, in an `await` expression, as an anonymous type property (`new { Prop = new() }`), in a `lock` statement, in a `sizeof`, in a `fixed` statement, in a member access (`new().field`), in a dynamically dispatched operation (`someDynamic.Method(new())`), in a LINQ query, as the operand of the `is` operator, as the left operand of the `??` operator,  ...

It is also disallowed as a `ref`.

## Drawbacks
[drawbacks]: #drawbacks

There were some concerns with target-typed `new` creating new categories of breaking changes, but we already have that with `null` and `default`, and that has not been a significant problem.

## Alternatives
[alternatives]: #alternatives

Most of complaints about types being too long to duplicate in field initialization is about *type arguments* not the type itself, we could infer only type arguments like `new Dictionary(...)` (or similar) and infer type arguments locally from arguments or the collection initializer.

## Questions
[questions]: #questions

- Should we forbid usages in expression trees? (no)
- How the feature interacts with `dynamic` arguments? (no special treatment)
- How IntelliSense should work with `new()`? (only when there is a single target-type)

## Design meetings

- [LDM-2017-10-18](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-10-18.md#100)
- [LDM-2018-05-21](https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-05-21.md)
- [LDM-2018-06-25](https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-06-25.md)
- [LDM-2018-08-22](https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-08-22.md#target-typed-new)
- [LDM-2018-10-17](https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md)
