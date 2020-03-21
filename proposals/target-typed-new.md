---
ms.openlocfilehash: 38740069a2e105f920fa275c443f4560055e2901
ms.sourcegitcommit: 9aa177443b83116fe1be2ab28e2c7291947fe32d
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/21/2020
ms.locfileid: "80108364"
---

# <a name="target-typed-new-expressions"></a><span data-ttu-id="b6325-101">Výrazy `new` s cílovým typem</span><span class="sxs-lookup"><span data-stu-id="b6325-101">Target-typed `new` expressions</span></span>

* <span data-ttu-id="b6325-102">Navrženo [x]</span><span class="sxs-lookup"><span data-stu-id="b6325-102">[x] Proposed</span></span>
* <span data-ttu-id="b6325-103">[x] [prototyp](https://github.com/alrz/roslyn/tree/features/target-typed-new)</span><span class="sxs-lookup"><span data-stu-id="b6325-103">[x] [Prototype](https://github.com/alrz/roslyn/tree/features/target-typed-new)</span></span>
* <span data-ttu-id="b6325-104">[] Implementace</span><span class="sxs-lookup"><span data-stu-id="b6325-104">[ ] Implementation</span></span>
* <span data-ttu-id="b6325-105">[] Specifikace</span><span class="sxs-lookup"><span data-stu-id="b6325-105">[ ] Specification</span></span>

## <a name="summary"></a><span data-ttu-id="b6325-106">Souhrn</span><span class="sxs-lookup"><span data-stu-id="b6325-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="b6325-107">Nevyžadovat specifikaci typu pro konstruktory, když je typ znám.</span><span class="sxs-lookup"><span data-stu-id="b6325-107">Do not require type specification for constructors when the type is known.</span></span> 

## <a name="motivation"></a><span data-ttu-id="b6325-108">Motivační</span><span class="sxs-lookup"><span data-stu-id="b6325-108">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="b6325-109">Povoluje inicializaci pole bez duplikace typu.</span><span class="sxs-lookup"><span data-stu-id="b6325-109">Allow field initialization without duplicating the type.</span></span>
```cs
Dictionary<string, List<int>> field = new() {
    { "item1", new() { 1, 2, 3 } }
};
```

<span data-ttu-id="b6325-110">Povolí vynechání typu, když se dá odvodit z použití.</span><span class="sxs-lookup"><span data-stu-id="b6325-110">Allow omitting the type when it can be inferred from usage.</span></span>
```cs
XmlReader.Create(reader, new() { IgnoreWhitespace = true });
```

<span data-ttu-id="b6325-111">Vytvořte instanci objektu bez nutnosti hláskování typu.</span><span class="sxs-lookup"><span data-stu-id="b6325-111">Instantiate an object without spelling out the type.</span></span>
```cs
private readonly static object s_syncObj = new();
```

## <a name="detailed-design"></a><span data-ttu-id="b6325-112">Podrobný návrh</span><span class="sxs-lookup"><span data-stu-id="b6325-112">Detailed design</span></span>
[design]: #detailed-design

<span data-ttu-id="b6325-113">Syntaxe *object_creation_expression* se upraví tak, aby byl *typ* volitelný, pokud jsou k dispozici závorky.</span><span class="sxs-lookup"><span data-stu-id="b6325-113">The *object_creation_expression* syntax would be modified to make the *type* optional when parentheses are present.</span></span> <span data-ttu-id="b6325-114">Tato je nutná k vyřešení nejednoznačnosti pomocí *anonymous_object_creation_expression*.</span><span class="sxs-lookup"><span data-stu-id="b6325-114">This is required to address the ambiguity with *anonymous_object_creation_expression*.</span></span>
```antlr
object_creation_expression
    : 'new' type? '(' argument_list? ')' object_or_collection_initializer?
    | 'new' type object_or_collection_initializer
    ;
```

<span data-ttu-id="b6325-115">`new` cílového typu je převoditelná na libovolný typ.</span><span class="sxs-lookup"><span data-stu-id="b6325-115">A target-typed `new` is convertible to any type.</span></span> <span data-ttu-id="b6325-116">V důsledku toho nepřispívá k řešení přetížení.</span><span class="sxs-lookup"><span data-stu-id="b6325-116">As a result, it does not contribute to overload resolution.</span></span> <span data-ttu-id="b6325-117">To se většinou vyhnout nepředvídatelným změnám.</span><span class="sxs-lookup"><span data-stu-id="b6325-117">This is mainly to avoid unpredictable breaking changes.</span></span>

<span data-ttu-id="b6325-118">Seznam argumentů a inicializační výrazy budou vázány po určení typu.</span><span class="sxs-lookup"><span data-stu-id="b6325-118">The argument list and the initializer expressions will be bound after the type is determined.</span></span>

<span data-ttu-id="b6325-119">Typ výrazu by byl odvozen z cílového typu, který by byl vyžadován pro jednu z následujících:</span><span class="sxs-lookup"><span data-stu-id="b6325-119">The type of the expression would be inferred from the target-type which would be required to be one of the following:</span></span>

- <span data-ttu-id="b6325-120">**Libovolný typ struktury** (včetně typů řazené kolekce členů)</span><span class="sxs-lookup"><span data-stu-id="b6325-120">**Any struct type** (including tuple types)</span></span>
- <span data-ttu-id="b6325-121">**Jakýkoli typ odkazu** (včetně typů delegátů)</span><span class="sxs-lookup"><span data-stu-id="b6325-121">**Any reference type** (including delegate types)</span></span>
- <span data-ttu-id="b6325-122">**Libovolný parametr typu** s konstruktorem nebo omezením `struct`</span><span class="sxs-lookup"><span data-stu-id="b6325-122">**Any type parameter** with a constructor or a `struct` constraint</span></span>

<span data-ttu-id="b6325-123">s následujícími výjimkami:</span><span class="sxs-lookup"><span data-stu-id="b6325-123">with the following exceptions:</span></span>

- <span data-ttu-id="b6325-124">**Výčtové typy:** ne všechny typy výčtů obsahují konstantu nula, takže by měl být vhodné použít explicitního člena výčtu.</span><span class="sxs-lookup"><span data-stu-id="b6325-124">**Enum types:** not all enum types contain the constant zero, so it should be desirable to use the explicit enum member.</span></span>
- <span data-ttu-id="b6325-125">**Typy rozhraní:** jedná se o funkci mezery a měla by být vhodnější výslovně uvést typ.</span><span class="sxs-lookup"><span data-stu-id="b6325-125">**Interface types:** this is a niche feature and it should be preferable to explicitly mention the type.</span></span>
- <span data-ttu-id="b6325-126">**Typy polí:** pole potřebují pro zadání délky speciální syntaxi.</span><span class="sxs-lookup"><span data-stu-id="b6325-126">**Array types:** arrays need a special syntax to provide the length.</span></span>
- <span data-ttu-id="b6325-127">**dynamické:** nepovolujeme `new dynamic()`, takže nepovolujeme `new()` s `dynamic` jako cílový typ.</span><span class="sxs-lookup"><span data-stu-id="b6325-127">**dynamic:** we don't allow `new dynamic()`, so we don't allow `new()` with `dynamic` as a target type.</span></span>

<span data-ttu-id="b6325-128">Všechny ostatní typy, které nejsou povoleny v *object_creation_expression* jsou vyloučeny také, například typy ukazatelů.</span><span class="sxs-lookup"><span data-stu-id="b6325-128">All the other types that are not permitted in the *object_creation_expression* are excluded as well, for instance, pointer types.</span></span>

<span data-ttu-id="b6325-129">Když je cílovým typem typ hodnoty s možnou hodnotou null, `new` cílový typ se převede na nadřízený typ místo typu s možnou hodnotou null.</span><span class="sxs-lookup"><span data-stu-id="b6325-129">When the target type is a nullable value type, the target-typed `new` will be converted to the underlying type instead of the nullable type.</span></span>

> <span data-ttu-id="b6325-130">**Otevření problému:** Pokud chcete, abychom jako cílový typ povolili delegáty a řazené kolekce členů?</span><span class="sxs-lookup"><span data-stu-id="b6325-130">**Open Issue:** should we allow delegates and tuples as the target-type?</span></span>

<span data-ttu-id="b6325-131">Výše uvedená pravidla zahrnují delegáty (typ odkazu) a řazené kolekce členů (typ struktury).</span><span class="sxs-lookup"><span data-stu-id="b6325-131">The above rules include delegates (a reference type) and tuples (a struct type).</span></span> <span data-ttu-id="b6325-132">I když jsou oba typy constructible, je-li typ odvozen, anonymní funkce nebo literál řazené kolekce členů již lze použít.</span><span class="sxs-lookup"><span data-stu-id="b6325-132">Although both types are constructible, if the type is inferable, an anonymous function or a tuple literal can already be used.</span></span>
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
