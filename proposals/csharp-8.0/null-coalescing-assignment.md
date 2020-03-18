---
ms.openlocfilehash: fdd858c895d56a7a6b410e6ea7be3032e4851fd6
ms.sourcegitcommit: 5a88d5432d32c690c6b870fc4be32cf26cadd76f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/11/2019
ms.locfileid: "79484956"
---
# <a name="null-coalescing-assignment"></a><span data-ttu-id="a8b70-101">Přiřazení sloučení s hodnotou null</span><span class="sxs-lookup"><span data-stu-id="a8b70-101">null coalescing assignment</span></span>

* <span data-ttu-id="a8b70-102">Navrženo [x]</span><span class="sxs-lookup"><span data-stu-id="a8b70-102">[x] Proposed</span></span>
* <span data-ttu-id="a8b70-103">[] Prototyp: Nezahájeno</span><span class="sxs-lookup"><span data-stu-id="a8b70-103">[ ] Prototype: Not Started</span></span>
* <span data-ttu-id="a8b70-104">[] Implementace: Nezahájeno</span><span class="sxs-lookup"><span data-stu-id="a8b70-104">[ ] Implementation: Not Started</span></span>
* <span data-ttu-id="a8b70-105">[] Specifikace: níže</span><span class="sxs-lookup"><span data-stu-id="a8b70-105">[ ] Specification: Below</span></span>

## <a name="summary"></a><span data-ttu-id="a8b70-106">Souhrn</span><span class="sxs-lookup"><span data-stu-id="a8b70-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="a8b70-107">Zjednodušuje společný vzor kódování, kde je proměnné přiřazena hodnota, pokud je null.</span><span class="sxs-lookup"><span data-stu-id="a8b70-107">Simplifies a common coding pattern where a variable is assigned a value if it is null.</span></span>

<span data-ttu-id="a8b70-108">V rámci tohoto návrhu také využijeme požadavky typu na `??`, aby bylo možné použít výraz, jehož typ je neomezeníný parametr typu, který se použije na levé straně.</span><span class="sxs-lookup"><span data-stu-id="a8b70-108">As part of this proposal, we will also loosen the type requirements on `??` to allow an expression whose type is an unconstrained type parameter to be used on the left-hand side.</span></span>

## <a name="motivation"></a><span data-ttu-id="a8b70-109">Motivační</span><span class="sxs-lookup"><span data-stu-id="a8b70-109">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="a8b70-110">Je běžné, že se zobrazí kód formuláře.</span><span class="sxs-lookup"><span data-stu-id="a8b70-110">It is common to see code of the form</span></span>

```csharp
if (variable == null)
{
    variable = expression;
}
```

<span data-ttu-id="a8b70-111">Tento návrh přidá k jazyku, který provádí tuto funkci, nepřetížený binární operátor.</span><span class="sxs-lookup"><span data-stu-id="a8b70-111">This proposal adds a non-overloadable binary operator to the language that performs this function.</span></span>

<span data-ttu-id="a8b70-112">Pro tuto funkci bylo minimálně osm samostatných požadavků komunity.</span><span class="sxs-lookup"><span data-stu-id="a8b70-112">There have been at least eight separate community requests for this feature.</span></span>

## <a name="detailed-design"></a><span data-ttu-id="a8b70-113">Podrobný návrh</span><span class="sxs-lookup"><span data-stu-id="a8b70-113">Detailed design</span></span>
[design]: #detailed-design

<span data-ttu-id="a8b70-114">Přidáme novou formu operátoru přiřazení.</span><span class="sxs-lookup"><span data-stu-id="a8b70-114">We add a new form of assignment operator</span></span>

``` antlr
assignment_operator
    : '??='
    ;
```

<span data-ttu-id="a8b70-115">Ta se řídí [stávajícími sémantickými pravidly pro operátory složeného přiřazení](../../spec/expressions.md#compound-assignment)s tím rozdílem, že Elide přiřazení, pokud není levá strana prázdná.</span><span class="sxs-lookup"><span data-stu-id="a8b70-115">Which follows the [existing semantic rules for compound assignment operators](../../spec/expressions.md#compound-assignment), except that we elide the assignment if the left-hand side is non-null.</span></span> <span data-ttu-id="a8b70-116">Pravidla této funkce jsou následující.</span><span class="sxs-lookup"><span data-stu-id="a8b70-116">The rules for this feature are as follows.</span></span>

<span data-ttu-id="a8b70-117">Zadaný `a ??= b`, kde `A` je typ `a`, `B` je typ `b`a `A0` je základní typ `A`, pokud `A` je typ hodnoty s možnou hodnotou null:</span><span class="sxs-lookup"><span data-stu-id="a8b70-117">Given `a ??= b`, where `A` is the type of `a`, `B` is the type of `b`, and `A0` is the underlying type of `A` if `A` is a nullable value type:</span></span>

1. <span data-ttu-id="a8b70-118">Pokud `A` neexistuje nebo se jedná o typ hodnoty, která není null, dojde k chybě při kompilaci.</span><span class="sxs-lookup"><span data-stu-id="a8b70-118">If `A` does not exist or is a non-nullable value type, a compile-time error occurs.</span></span>
2. <span data-ttu-id="a8b70-119">Pokud `B` není implicitně převoditelná na `A` nebo `A0` (Pokud `A0` existuje), dojde k chybě při kompilaci.</span><span class="sxs-lookup"><span data-stu-id="a8b70-119">If `B` is not implicitly convertible to `A` or `A0` (if `A0` exists), a compile-time error occurs.</span></span>
3. <span data-ttu-id="a8b70-120">Pokud `A0` existuje a `B` je implicitně převoditelná na `A0`a `B` není dynamická, pak je `a ??= b` typ `A0`.</span><span class="sxs-lookup"><span data-stu-id="a8b70-120">If `A0` exists and `B` is implicitly convertible to `A0`, and `B` is not dynamic, then the type of `a ??= b` is `A0`.</span></span> <span data-ttu-id="a8b70-121">`a ??= b` se vyhodnocuje za běhu jako:</span><span class="sxs-lookup"><span data-stu-id="a8b70-121">`a ??= b` is evaluated at runtime as:</span></span>
   ```C#
   var tmp = a.GetValueOrDefault();
   if (!a.HasValue) { tmp = b; a = tmp; }
   tmp
   ```
   <span data-ttu-id="a8b70-122">s výjimkou `a` je vyhodnocena pouze jednou.</span><span class="sxs-lookup"><span data-stu-id="a8b70-122">Except that `a` is only evaluated once.</span></span>
4. <span data-ttu-id="a8b70-123">V opačném případě je typ `a ??= b` `A`.</span><span class="sxs-lookup"><span data-stu-id="a8b70-123">Otherwise, the type of `a ??= b` is `A`.</span></span> <span data-ttu-id="a8b70-124">`a ??= b` je vyhodnocena za běhu jako `a ?? (a = b)`, s výjimkou toho, že `a` vyhodnocena pouze jednou.</span><span class="sxs-lookup"><span data-stu-id="a8b70-124">`a ??= b` is evaluated at runtime as `a ?? (a = b)`, except that `a` is only evaluated once.</span></span>


<span data-ttu-id="a8b70-125">Pro zmírnění požadavků typu `??`aktualizujeme specifikaci, kde v současné době uvádí `a ?? b`, kde `A` je typ `a`:</span><span class="sxs-lookup"><span data-stu-id="a8b70-125">For the relaxation of the type requirements of `??`, we update the spec where it currently states that, given `a ?? b`, where `A` is the type of `a`:</span></span>

> 1. <span data-ttu-id="a8b70-126">Pokud existuje a není typu s možnou hodnotou null nebo odkazovým typem, dojde k chybě při kompilaci.</span><span class="sxs-lookup"><span data-stu-id="a8b70-126">If A exists and is not a nullable type or a reference type, a compile-time error occurs.</span></span>

<span data-ttu-id="a8b70-127">Tento požadavek zmírnit na:</span><span class="sxs-lookup"><span data-stu-id="a8b70-127">We relax this requirement to:</span></span>

1. <span data-ttu-id="a8b70-128">Pokud existuje a je typ hodnoty, která není null, dojde k chybě při kompilaci.</span><span class="sxs-lookup"><span data-stu-id="a8b70-128">If A exists and is a non-nullable value type, a compile-time error occurs.</span></span>

<span data-ttu-id="a8b70-129">To umožňuje operátoru slučování null pracovat na neomezeném parametru typu, protože existuje parametr neomezeného typu, který není typu Nullable, a není odkazový typ.</span><span class="sxs-lookup"><span data-stu-id="a8b70-129">This allows the null coalescing operator to work on unconstrained type parameters, as the unconstrained type parameter T exists, is not a nullable type, and is not a reference type.</span></span>

## <a name="drawbacks"></a><span data-ttu-id="a8b70-130">Nevýhody</span><span class="sxs-lookup"><span data-stu-id="a8b70-130">Drawbacks</span></span>
[drawbacks]: #drawbacks

<span data-ttu-id="a8b70-131">Stejně jako u libovolné jazykové funkce musíme dotazovat, jestli se další složitost tohoto jazyka znovu vyplácí v další čistotě, která je k dispozici pro tělo C# programů, které by mohly využít výhod této funkce.</span><span class="sxs-lookup"><span data-stu-id="a8b70-131">As with any language feature, we must question whether the additional complexity to the language is repaid in the additional clarity offered to the body of C# programs that would benefit from the feature.</span></span>

## <a name="alternatives"></a><span data-ttu-id="a8b70-132">Alternativy</span><span class="sxs-lookup"><span data-stu-id="a8b70-132">Alternatives</span></span>
[alternatives]: #alternatives

<span data-ttu-id="a8b70-133">Programátor může psát `(x = x ?? y)`, `if (x == null) x = y;`nebo `x ?? (x = y)` rukou.</span><span class="sxs-lookup"><span data-stu-id="a8b70-133">The programmer can write `(x = x ?? y)`, `if (x == null) x = y;`, or `x ?? (x = y)` by hand.</span></span>

## <a name="unresolved-questions"></a><span data-ttu-id="a8b70-134">Nevyřešené dotazy</span><span class="sxs-lookup"><span data-stu-id="a8b70-134">Unresolved questions</span></span>
[unresolved]: #unresolved-questions

- <span data-ttu-id="a8b70-135">[] Vyžaduje revizi LDM.</span><span class="sxs-lookup"><span data-stu-id="a8b70-135">[ ] Requires LDM review</span></span>
- <span data-ttu-id="a8b70-136">[] By také podporovaly `&&=` a `||=`?</span><span class="sxs-lookup"><span data-stu-id="a8b70-136">[ ] Should we also support `&&=` and `||=` operators?</span></span>

## <a name="design-meetings"></a><span data-ttu-id="a8b70-137">Schůzky pro návrh</span><span class="sxs-lookup"><span data-stu-id="a8b70-137">Design meetings</span></span>

<span data-ttu-id="a8b70-138">Žádné.</span><span class="sxs-lookup"><span data-stu-id="a8b70-138">None.</span></span>
