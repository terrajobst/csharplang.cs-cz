---
ms.openlocfilehash: 4e2a536bab00859b003e8d967cb1927a99a9fa21
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484536"
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

- **Libovolný typ struktury**
- **Libovolný typ odkazu**
- **Libovolný parametr typu** s konstruktorem nebo omezením `struct`

s následujícími výjimkami:

- **Výčtové typy:** ne všechny typy výčtů obsahují konstantu nula, takže by měl být vhodné použít explicitního člena výčtu.
- **Typy rozhraní:** jedná se o funkci mezery a měla by být vhodnější výslovně uvést typ.
- **Typy polí:** pole potřebují pro zadání délky speciální syntaxi.
- **Výchozí konstruktor struct**: Tato pravidla vyprší všem primitivním typům a většině hodnotových typů. Pokud jste chtěli použít výchozí hodnotu takových typů, můžete místo toho napsat `default`.

Všechny ostatní typy, které nejsou povoleny v *object_creation_expression* jsou vyloučeny také, například typy ukazatelů.

> **Otevření problému:** Pokud chcete, abychom jako cílový typ povolili delegáty a řazené kolekce členů?

Výše uvedená pravidla zahrnují delegáty (typ odkazu) a řazené kolekce členů (typ struktury). I když jsou oba typy constructible, je-li typ odvozen, anonymní funkce nebo literál řazené kolekce členů již lze použít.
```cs
(int a, int b) t = new(1, 2); // "new" is redundant
Action a = new(() => {}); // "new" is redundant

(int a, int b) t = new(); // ruled out by "use of struct default constructor"
Action a = new(); // no constructor found

var x = new() == (1, 2); // ruled out by "use of struct default constructor"
var x = new(1, 2) == (1, 2) // "new" is redundant
```


> **Otevření problému:** měli bychom `throw new()` jako cílový typ používat jako `Exception`?

V dnešní době jsme `throw null`i, ale ne `throw default` (i když by to vedlo ke stejnému účinku). Na druhé straně `throw new()` možné být ve skutečnosti užitečné jako zkratky pro `throw new Exception(...)`. Všimněte si, že už je povolená aktuální specifikací. `Exception` je odkazový typ a specifikace pro příkaz throw říká, že je výraz převeden na `Exception`.

> **Otevření problému:** je potřeba, abychom povolili použití cílového typu `new` s uživatelsky definovaným porovnáním a aritmetickými operátory?

Pro účely porovnání `default` podporuje pouze rovnost (uživatelsky definované a předdefinované) operátory. Měla by smysl podporovat i jiné operátory pro `new()`?

## <a name="drawbacks"></a>Nevýhody
[drawbacks]: #drawbacks

Žádné.

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
