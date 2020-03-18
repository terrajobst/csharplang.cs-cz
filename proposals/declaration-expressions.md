---
ms.openlocfilehash: d2064ec1637e50962262c9380281abd5e1711402
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484578"
---
# <a name="declaration-expressions"></a><span data-ttu-id="f17c6-101">Výrazy deklarace</span><span class="sxs-lookup"><span data-stu-id="f17c6-101">Declaration expressions</span></span>

<span data-ttu-id="f17c6-102">Podpora přiřazení deklarací jako výrazů.</span><span class="sxs-lookup"><span data-stu-id="f17c6-102">Support declaration assignments as expressions.</span></span>

## <a name="motivation"></a><span data-ttu-id="f17c6-103">Motivační</span><span class="sxs-lookup"><span data-stu-id="f17c6-103">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="f17c6-104">Povolit inicializaci v místě deklarace ve více případech, zjednodušit kód a povolit `var` použít.</span><span class="sxs-lookup"><span data-stu-id="f17c6-104">Allow initialization at the point of declaration in more cases, simplifying code, and allowing `var` to be used.</span></span>

```csharp
SpecialType ReferenceType =>
    (var st = _type.SpecialType).IsValueType() ? SpecialType.None : st;
```

<span data-ttu-id="f17c6-105">Povolí deklarace pro argumenty `ref`, podobně jako `out var`.</span><span class="sxs-lookup"><span data-stu-id="f17c6-105">Allow declarations for `ref` arguments, similar to `out var`.</span></span>

```csharp
Convert(source, destination, ref List<Diagnostic> diagnostics = null);
```

## <a name="detailed-design"></a><span data-ttu-id="f17c6-106">Podrobný návrh</span><span class="sxs-lookup"><span data-stu-id="f17c6-106">Detailed design</span></span>
[design]: #detailed-design

<span data-ttu-id="f17c6-107">Výrazy se rozšiřují tak, aby zahrnovaly přiřazení deklarace.</span><span class="sxs-lookup"><span data-stu-id="f17c6-107">Expressions are extended to include declaration assignment.</span></span> <span data-ttu-id="f17c6-108">Priorita je shodná s přiřazením.</span><span class="sxs-lookup"><span data-stu-id="f17c6-108">Precedence is the same as assignment.</span></span>

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

<span data-ttu-id="f17c6-109">Přiřazení deklarace je jediné místní.</span><span class="sxs-lookup"><span data-stu-id="f17c6-109">The declaration assignment is of a single local.</span></span>

<span data-ttu-id="f17c6-110">Typ výrazu přiřazení deklarace je typ deklarace.</span><span class="sxs-lookup"><span data-stu-id="f17c6-110">The type of a declaration assignment expression is the type of the declaration.</span></span>
<span data-ttu-id="f17c6-111">Pokud je typ `var`, odvozený typ je typ inicializačního výrazu.</span><span class="sxs-lookup"><span data-stu-id="f17c6-111">If the type is `var`, the inferred type is the type of the initializing expression.</span></span> 

<span data-ttu-id="f17c6-112">Výraz přiřazení deklarace může být l-hodnotou pro `ref` hodnoty argumentu, konkrétně.</span><span class="sxs-lookup"><span data-stu-id="f17c6-112">The declaration assignment expression may be an l-value, for `ref` argument values in particular.</span></span>

<span data-ttu-id="f17c6-113">Pokud výraz přiřazení deklarace deklaruje typ hodnoty a výraz je hodnota r, hodnota výrazu je kopie.</span><span class="sxs-lookup"><span data-stu-id="f17c6-113">If the declaration assignment expression declares a value type, and the expression is an r-value, the value of the expression is a copy.</span></span>

<span data-ttu-id="f17c6-114">Výraz přiřazení deklarace může deklarovat `ref` místní.</span><span class="sxs-lookup"><span data-stu-id="f17c6-114">The declaration assignment expression may declare a `ref` local.</span></span>
<span data-ttu-id="f17c6-115">Při použití `ref` pro výraz deklarace v argumentu `ref` je nejednoznačnost.</span><span class="sxs-lookup"><span data-stu-id="f17c6-115">There is an ambiguity when `ref` is used for a declaration expression in a `ref` argument.</span></span>
<span data-ttu-id="f17c6-116">Lokální proměnná pro inicializátor určuje, zda je deklarace `ref` místní.</span><span class="sxs-lookup"><span data-stu-id="f17c6-116">The local variable initializer determines whether the declaration is a `ref` local.</span></span>

```csharp
F(ref int x = IntFunc());    // int x;
F(ref int y = RefIntFunc()); // ref int y;
```

<span data-ttu-id="f17c6-117">Rozsah místních hodnot deklarovaných ve výrazech přiřazení deklarace je stejný jako rozsah odpovídajících výrazů deklarace v jazyce C # 7.0.</span><span class="sxs-lookup"><span data-stu-id="f17c6-117">The scope of locals declared in declaration assignment expressions is the same the scope of corresponding declaration expressions from C#7.0.</span></span>

<span data-ttu-id="f17c6-118">Jedná se o chybu doby kompilace pro odkazování na místní text v předchozím výrazu deklarace.</span><span class="sxs-lookup"><span data-stu-id="f17c6-118">It is a compile time error to refer to a local in text preceding the declaration expression.</span></span>

## <a name="alternatives"></a><span data-ttu-id="f17c6-119">Alternativy</span><span class="sxs-lookup"><span data-stu-id="f17c6-119">Alternatives</span></span>
[alternatives]: #alternatives
<span data-ttu-id="f17c6-120">Žádná změna.</span><span class="sxs-lookup"><span data-stu-id="f17c6-120">No change.</span></span> <span data-ttu-id="f17c6-121">Tato funkce je pouze syntakticky zkráceným způsobem.</span><span class="sxs-lookup"><span data-stu-id="f17c6-121">This feature is just syntactic shorthand after all.</span></span>

<span data-ttu-id="f17c6-122">Obecnější výrazy sekvence: viz [#377](https://github.com/dotnet/csharplang/issues/377).</span><span class="sxs-lookup"><span data-stu-id="f17c6-122">More general sequence expressions: see [#377](https://github.com/dotnet/csharplang/issues/377).</span></span>

<span data-ttu-id="f17c6-123">Aby bylo možné `var` použít ve více případech, umožněte samostatnou deklaraci a přiřazení `var` místních hodnot a odvodit typ z přiřazení ze všech cest kódu.</span><span class="sxs-lookup"><span data-stu-id="f17c6-123">To allow use of `var` in more cases, allow separate declaration and assignment of `var` locals, and infer the type from assignments from all code paths.</span></span>

## <a name="see-also"></a><span data-ttu-id="f17c6-124">Viz také</span><span class="sxs-lookup"><span data-stu-id="f17c6-124">See also</span></span>
[see-also]: #see-also
<span data-ttu-id="f17c6-125">Viz výraz základní deklarace v [#595](https://github.com/dotnet/csharplang/issues/595).</span><span class="sxs-lookup"><span data-stu-id="f17c6-125">See Basic Declaration Expression in [#595](https://github.com/dotnet/csharplang/issues/595).</span></span>

<span data-ttu-id="f17c6-126">Viz deklarace dekonstrukce v funkci [dekonstrukce](https://github.com/dotnet/roslyn/blob/master/docs/features/deconstruction.md) .</span><span class="sxs-lookup"><span data-stu-id="f17c6-126">See Deconstruction Declaration in the [deconstruction](https://github.com/dotnet/roslyn/blob/master/docs/features/deconstruction.md) feature.</span></span>
