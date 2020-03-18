---
ms.openlocfilehash: 25e95b3ab8c384a7e66e59a7f9422cc9699074d7
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484718"
---
# <a name="infer-tuple-names-aka-tuple-projection-initializers"></a>Odvodit názvy řazených kolekcí členů (neboli. Inicializátory v řazené kolekci členů)

## <a name="summary"></a>Souhrn
[summary]: #summary

V mnoha běžných případech tato funkce umožňuje vynechat názvy elementů řazené kolekce členů a místo toho je odvodit. Například namísto psaní `(f1: x.f1, f2: x?.f2)`se názvy elementů "F1" a "F2" dají odvodit z `(x.f1, x?.f2)`.

To paralelně chování anonymních typů, které umožňují odvození názvů členů během vytváření. `new { x.f1, y?.f2 }` například deklaruje členy "F1" a "F2".

To je užitečné zejména při použití řazených kolekcí členů v LINQ:

```csharp
// "c" and "result" have element names "f1" and "f2"
var result = list.Select(c => (c.f1, c.f2)).Where(t => t.f2 == 1); 
```

## <a name="detailed-design"></a>Podrobný návrh
[design]: #detailed-design

Tato změna má dvě části:

1.  Zkuste odvodit kandidáta na název každého prvku řazené kolekce členů, který nemá explicitní název:
    -   Použití stejných pravidel jako odvození názvu pro anonymní typy.
        - V C#nástroji to umožňuje tři případy: `y` (identifikátor), `x.y` (přístup pomocí jednoduchých členů) a `x?.y` (podmíněný přístup).
        - V jazyce VB to umožňuje další případy, například `x.y()`.
    -   Odmítání vyhrazených názvů řazené kolekce členů C#(rozlišuje velká a malá písmena v jazyce VB), protože jsou buď zakázané, nebo už implicitní. Například `ItemN`, `Rest`a `ToString`.
    -   Pokud jsou názvy kandidátů duplicitní (rozlišuje velká C#a malá písmena v VB) v rámci celé řazené kolekce členů, vynecháváme tyto kandidáty,
2.  Během převodů (které kontrolují a upozorňující na vyřazení názvů z literálů řazené kolekce členů) odvozené názvy nevytvořily žádná upozornění. Tím předejdete přerušení stávajícího kódu řazené kolekce členů.

Všimněte si, že pravidlo pro zpracování duplicit je jiné než pro anonymní typy. `new { x.f1, x.f1 }` například vyvolá chybu, ale `(x.f1, x.f1)` by se pořád povolil (jenom bez názvů s odvozenými názvy). Tím předejdete přerušení stávajícího kódu řazené kolekce členů.

Pro zajištění konzistence by se totéž mělo vztahovat na řazené kolekce členů vytvořené pomocí dekonstrukce C#– přiřazení (v):

```csharp
// tuple has element names "f1" and "f2" 
var tuple = ((x.f1, x?.f2) = (1, 2));
```

Stejné by se také vztahují na řazené kolekce členů VB pomocí pravidel specifických pro jazyk VB pro odvození názvu z výrazů a porovnávání názvů bez rozlišení velkých a malých písmen.

Pokud používáte kompilátor C# 7,1 (nebo novější) s jazykovou verzí "7,0", názvy prvků budou odvozeny (bez ohledu na to, která funkce není k dispozici), ale při pokusu o přístup k nim dojde k chybě použití webu. Tím se omezí přidání nového kódu, který by později znamenal problém s kompatibilitou (popsaný níže).

## <a name="drawbacks"></a>Nevýhody
[drawbacks]: #drawbacks

Hlavní nevýhodou je, že představuje přerušení kompatibility od C# 7,0:

```csharp
Action y = () => M();
var t = (x: x, y);
t.y(); // this might have previously picked up an extension method called “y”, but would now call the lambda.
```

Rada kompatibility našla toto přerušení jako přijatelné, vzhledem k tomu, že je omezené a časový interval od odeslání řazené kolekce C# členů (v 7,0) je krátký.

## <a name="references"></a>Odkazy
- [LDM od 4. dubna 2017](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-05.md#tuple-names)
- [Diskuze na GitHubu](https://github.com/dotnet/csharplang/issues/370) (děkujeme @alrz pro uvedení tohoto problému)
- [Návrh řazených kolekcí členů](https://github.com/dotnet/roslyn/blob/master/docs/features/tuples.md)
