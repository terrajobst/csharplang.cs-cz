---
ms.openlocfilehash: d2064ec1637e50962262c9380281abd5e1711402
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484578"
---
# <a name="declaration-expressions"></a>Výrazy deklarace

Podpora přiřazení deklarací jako výrazů.

## <a name="motivation"></a>Motivační
[motivation]: #motivation

Povolit inicializaci v místě deklarace ve více případech, zjednodušit kód a povolit `var` použít.

```csharp
SpecialType ReferenceType =>
    (var st = _type.SpecialType).IsValueType() ? SpecialType.None : st;
```

Povolí deklarace pro argumenty `ref`, podobně jako `out var`.

```csharp
Convert(source, destination, ref List<Diagnostic> diagnostics = null);
```

## <a name="detailed-design"></a>Podrobný návrh
[design]: #detailed-design

Výrazy se rozšiřují tak, aby zahrnovaly přiřazení deklarace. Priorita je shodná s přiřazením.

```antlr
expression
    : non_assignment_expression
    | assignment
    | declaration_assignment_expression // new
    ;
declaration_assignment_expression // new
    : declaration_expression '=' local_variable_initializer
    ;
declaration_expression // C# 7.0
    | type variable_designation
    ;
```

Přiřazení deklarace je jediné místní.

Typ výrazu přiřazení deklarace je typ deklarace.
Pokud je typ `var`, odvozený typ je typ inicializačního výrazu. 

Výraz přiřazení deklarace může být l-hodnotou pro `ref` hodnoty argumentu, konkrétně.

Pokud výraz přiřazení deklarace deklaruje typ hodnoty a výraz je hodnota r, hodnota výrazu je kopie.

Výraz přiřazení deklarace může deklarovat `ref` místní.
Při použití `ref` pro výraz deklarace v argumentu `ref` je nejednoznačnost.
Lokální proměnná pro inicializátor určuje, zda je deklarace `ref` místní.

```csharp
F(ref int x = IntFunc());    // int x;
F(ref int y = RefIntFunc()); // ref int y;
```

Rozsah místních hodnot deklarovaných ve výrazech přiřazení deklarace je stejný jako rozsah odpovídajících výrazů deklarace v jazyce C # 7.0.

Jedná se o chybu doby kompilace pro odkazování na místní text v předchozím výrazu deklarace.

## <a name="alternatives"></a>Alternativy
[alternatives]: #alternatives
Žádná změna. Tato funkce je pouze syntakticky zkráceným způsobem.

Obecnější výrazy sekvence: viz [#377](https://github.com/dotnet/csharplang/issues/377).

Aby bylo možné `var` použít ve více případech, umožněte samostatnou deklaraci a přiřazení `var` místních hodnot a odvodit typ z přiřazení ze všech cest kódu.

## <a name="see-also"></a>Viz také
[see-also]: #see-also
Viz výraz základní deklarace v [#595](https://github.com/dotnet/csharplang/issues/595).

Viz deklarace dekonstrukce v funkci [dekonstrukce](https://github.com/dotnet/roslyn/blob/master/docs/features/deconstruction.md) .
