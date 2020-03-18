---
ms.openlocfilehash: fdd858c895d56a7a6b410e6ea7be3032e4851fd6
ms.sourcegitcommit: 5a88d5432d32c690c6b870fc4be32cf26cadd76f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/11/2019
ms.locfileid: "79484956"
---
# <a name="null-coalescing-assignment"></a>Přiřazení sloučení s hodnotou null

* Navrženo [x]
* [] Prototyp: Nezahájeno
* [] Implementace: Nezahájeno
* [] Specifikace: níže

## <a name="summary"></a>Souhrn
[summary]: #summary

Zjednodušuje společný vzor kódování, kde je proměnné přiřazena hodnota, pokud je null.

V rámci tohoto návrhu také využijeme požadavky typu na `??`, aby bylo možné použít výraz, jehož typ je neomezeníný parametr typu, který se použije na levé straně.

## <a name="motivation"></a>Motivační
[motivation]: #motivation

Je běžné, že se zobrazí kód formuláře.

```csharp
if (variable == null)
{
    variable = expression;
}
```

Tento návrh přidá k jazyku, který provádí tuto funkci, nepřetížený binární operátor.

Pro tuto funkci bylo minimálně osm samostatných požadavků komunity.

## <a name="detailed-design"></a>Podrobný návrh
[design]: #detailed-design

Přidáme novou formu operátoru přiřazení.

``` antlr
assignment_operator
    : '??='
    ;
```

Ta se řídí [stávajícími sémantickými pravidly pro operátory složeného přiřazení](../../spec/expressions.md#compound-assignment)s tím rozdílem, že Elide přiřazení, pokud není levá strana prázdná. Pravidla této funkce jsou následující.

Zadaný `a ??= b`, kde `A` je typ `a`, `B` je typ `b`a `A0` je základní typ `A`, pokud `A` je typ hodnoty s možnou hodnotou null:

1. Pokud `A` neexistuje nebo se jedná o typ hodnoty, která není null, dojde k chybě při kompilaci.
2. Pokud `B` není implicitně převoditelná na `A` nebo `A0` (Pokud `A0` existuje), dojde k chybě při kompilaci.
3. Pokud `A0` existuje a `B` je implicitně převoditelná na `A0`a `B` není dynamická, pak je `a ??= b` typ `A0`. `a ??= b` se vyhodnocuje za běhu jako:
   ```C#
   var tmp = a.GetValueOrDefault();
   if (!a.HasValue) { tmp = b; a = tmp; }
   tmp
   ```
   s výjimkou `a` je vyhodnocena pouze jednou.
4. V opačném případě je typ `a ??= b` `A`. `a ??= b` je vyhodnocena za běhu jako `a ?? (a = b)`, s výjimkou toho, že `a` vyhodnocena pouze jednou.


Pro zmírnění požadavků typu `??`aktualizujeme specifikaci, kde v současné době uvádí `a ?? b`, kde `A` je typ `a`:

> 1. Pokud existuje a není typu s možnou hodnotou null nebo odkazovým typem, dojde k chybě při kompilaci.

Tento požadavek zmírnit na:

1. Pokud existuje a je typ hodnoty, která není null, dojde k chybě při kompilaci.

To umožňuje operátoru slučování null pracovat na neomezeném parametru typu, protože existuje parametr neomezeného typu, který není typu Nullable, a není odkazový typ.

## <a name="drawbacks"></a>Nevýhody
[drawbacks]: #drawbacks

Stejně jako u libovolné jazykové funkce musíme dotazovat, jestli se další složitost tohoto jazyka znovu vyplácí v další čistotě, která je k dispozici pro tělo C# programů, které by mohly využít výhod této funkce.

## <a name="alternatives"></a>Alternativy
[alternatives]: #alternatives

Programátor může psát `(x = x ?? y)`, `if (x == null) x = y;`nebo `x ?? (x = y)` rukou.

## <a name="unresolved-questions"></a>Nevyřešené dotazy
[unresolved]: #unresolved-questions

- [] Vyžaduje revizi LDM.
- [] By také podporovaly `&&=` a `||=`?

## <a name="design-meetings"></a>Schůzky pro návrh

Žádné.
