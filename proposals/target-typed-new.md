---
ms.openlocfilehash: 07b4afe4a3fcbf10c978f05e642dfd8a47d53ea5
ms.sourcegitcommit: 194a043db72b9244f8db45db326cc82de6cec965
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/24/2020
ms.locfileid: "80217200"
---

# <a name="target-typed-new-expressions"></a><span data-ttu-id="bb07e-101">Výrazy `new` s cílovým typem</span><span class="sxs-lookup"><span data-stu-id="bb07e-101">Target-typed `new` expressions</span></span>

* <span data-ttu-id="bb07e-102">Navrženo [x]</span><span class="sxs-lookup"><span data-stu-id="bb07e-102">[x] Proposed</span></span>
* <span data-ttu-id="bb07e-103">[x] [prototyp](https://github.com/alrz/roslyn/tree/features/target-typed-new)</span><span class="sxs-lookup"><span data-stu-id="bb07e-103">[x] [Prototype](https://github.com/alrz/roslyn/tree/features/target-typed-new)</span></span>
* <span data-ttu-id="bb07e-104">[] Implementace</span><span class="sxs-lookup"><span data-stu-id="bb07e-104">[ ] Implementation</span></span>
* <span data-ttu-id="bb07e-105">[] Specifikace</span><span class="sxs-lookup"><span data-stu-id="bb07e-105">[ ] Specification</span></span>

## <a name="summary"></a><span data-ttu-id="bb07e-106">Souhrn</span><span class="sxs-lookup"><span data-stu-id="bb07e-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="bb07e-107">Nevyžadovat specifikaci typu pro konstruktory, když je typ znám.</span><span class="sxs-lookup"><span data-stu-id="bb07e-107">Do not require type specification for constructors when the type is known.</span></span> 

## <a name="motivation"></a><span data-ttu-id="bb07e-108">Motivační</span><span class="sxs-lookup"><span data-stu-id="bb07e-108">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="bb07e-109">Povoluje inicializaci pole bez duplikace typu.</span><span class="sxs-lookup"><span data-stu-id="bb07e-109">Allow field initialization without duplicating the type.</span></span>
```cs
Dictionary<string, List<int>> field = new() {
    { "item1", new() { 1, 2, 3 } }
};
```

<span data-ttu-id="bb07e-110">Povolí vynechání typu, když se dá odvodit z použití.</span><span class="sxs-lookup"><span data-stu-id="bb07e-110">Allow omitting the type when it can be inferred from usage.</span></span>
```cs
XmlReader.Create(reader, new() { IgnoreWhitespace = true });
```

<span data-ttu-id="bb07e-111">Vytvořte instanci objektu bez nutnosti hláskování typu.</span><span class="sxs-lookup"><span data-stu-id="bb07e-111">Instantiate an object without spelling out the type.</span></span>
```cs
private readonly static object s_syncObj = new();
```

## <a name="detailed-design"></a><span data-ttu-id="bb07e-112">Podrobný návrh</span><span class="sxs-lookup"><span data-stu-id="bb07e-112">Detailed design</span></span>
[design]: #detailed-design

<span data-ttu-id="bb07e-113">Syntaxe *object_creation_expression* se upraví tak, aby byl *typ* volitelný, pokud jsou k dispozici závorky.</span><span class="sxs-lookup"><span data-stu-id="bb07e-113">The *object_creation_expression* syntax would be modified to make the *type* optional when parentheses are present.</span></span> <span data-ttu-id="bb07e-114">Tato je nutná k vyřešení nejednoznačnosti pomocí *anonymous_object_creation_expression*.</span><span class="sxs-lookup"><span data-stu-id="bb07e-114">This is required to address the ambiguity with *anonymous_object_creation_expression*.</span></span>
```antlr
object_creation_expression
    : 'new' type? '(' argument_list? ')' object_or_collection_initializer?
    | 'new' type object_or_collection_initializer
    ;
```

<span data-ttu-id="bb07e-115">`new` cílového typu je převoditelná na libovolný typ.</span><span class="sxs-lookup"><span data-stu-id="bb07e-115">A target-typed `new` is convertible to any type.</span></span> <span data-ttu-id="bb07e-116">V důsledku toho nepřispívá k řešení přetížení.</span><span class="sxs-lookup"><span data-stu-id="bb07e-116">As a result, it does not contribute to overload resolution.</span></span> <span data-ttu-id="bb07e-117">To se většinou vyhnout nepředvídatelným změnám.</span><span class="sxs-lookup"><span data-stu-id="bb07e-117">This is mainly to avoid unpredictable breaking changes.</span></span>

<span data-ttu-id="bb07e-118">Seznam argumentů a inicializační výrazy budou vázány po určení typu.</span><span class="sxs-lookup"><span data-stu-id="bb07e-118">The argument list and the initializer expressions will be bound after the type is determined.</span></span>

<span data-ttu-id="bb07e-119">Typ výrazu by byl odvozen z cílového typu, který by byl vyžadován pro jednu z následujících:</span><span class="sxs-lookup"><span data-stu-id="bb07e-119">The type of the expression would be inferred from the target-type which would be required to be one of the following:</span></span>

- <span data-ttu-id="bb07e-120">**Libovolný typ struktury** (včetně typů řazené kolekce členů)</span><span class="sxs-lookup"><span data-stu-id="bb07e-120">**Any struct type** (including tuple types)</span></span>
- <span data-ttu-id="bb07e-121">**Jakýkoli typ odkazu** (včetně typů delegátů)</span><span class="sxs-lookup"><span data-stu-id="bb07e-121">**Any reference type** (including delegate types)</span></span>
- <span data-ttu-id="bb07e-122">**Libovolný parametr typu** s konstruktorem nebo omezením `struct`</span><span class="sxs-lookup"><span data-stu-id="bb07e-122">**Any type parameter** with a constructor or a `struct` constraint</span></span>

<span data-ttu-id="bb07e-123">s následujícími výjimkami:</span><span class="sxs-lookup"><span data-stu-id="bb07e-123">with the following exceptions:</span></span>

- <span data-ttu-id="bb07e-124">**Výčtové typy:** ne všechny typy výčtů obsahují konstantu nula, takže by měl být vhodné použít explicitního člena výčtu.</span><span class="sxs-lookup"><span data-stu-id="bb07e-124">**Enum types:** not all enum types contain the constant zero, so it should be desirable to use the explicit enum member.</span></span>
- <span data-ttu-id="bb07e-125">**Typy rozhraní:** jedná se o funkci mezery a měla by být vhodnější výslovně uvést typ.</span><span class="sxs-lookup"><span data-stu-id="bb07e-125">**Interface types:** this is a niche feature and it should be preferable to explicitly mention the type.</span></span>
- <span data-ttu-id="bb07e-126">**Typy polí:** pole potřebují pro zadání délky speciální syntaxi.</span><span class="sxs-lookup"><span data-stu-id="bb07e-126">**Array types:** arrays need a special syntax to provide the length.</span></span>
- <span data-ttu-id="bb07e-127">**dynamické:** nepovolujeme `new dynamic()`, takže nepovolujeme `new()` s `dynamic` jako cílový typ.</span><span class="sxs-lookup"><span data-stu-id="bb07e-127">**dynamic:** we don't allow `new dynamic()`, so we don't allow `new()` with `dynamic` as a target type.</span></span>

<span data-ttu-id="bb07e-128">Všechny ostatní typy, které nejsou povoleny v *object_creation_expression* jsou vyloučeny také, například typy ukazatelů.</span><span class="sxs-lookup"><span data-stu-id="bb07e-128">All the other types that are not permitted in the *object_creation_expression* are excluded as well, for instance, pointer types.</span></span>

<span data-ttu-id="bb07e-129">Když je cílovým typem typ hodnoty s možnou hodnotou null, `new` cílový typ se převede na nadřízený typ místo typu s možnou hodnotou null.</span><span class="sxs-lookup"><span data-stu-id="bb07e-129">When the target type is a nullable value type, the target-typed `new` will be converted to the underlying type instead of the nullable type.</span></span>

> <span data-ttu-id="bb07e-130">**Otevření problému:** Pokud chcete, abychom jako cílový typ povolili delegáty a řazené kolekce členů?</span><span class="sxs-lookup"><span data-stu-id="bb07e-130">**Open Issue:** should we allow delegates and tuples as the target-type?</span></span>

<span data-ttu-id="bb07e-131">Výše uvedená pravidla zahrnují delegáty (typ odkazu) a řazené kolekce členů (typ struktury).</span><span class="sxs-lookup"><span data-stu-id="bb07e-131">The above rules include delegates (a reference type) and tuples (a struct type).</span></span> <span data-ttu-id="bb07e-132">I když jsou oba typy constructible, je-li typ odvozen, anonymní funkce nebo literál řazené kolekce členů již lze použít.</span><span class="sxs-lookup"><span data-stu-id="bb07e-132">Although both types are constructible, if the type is inferable, an anonymous function or a tuple literal can already be used.</span></span>
```cs
(int a, int b) t = new(1, 2); // "new" is redundant
Action a = new(() => {}); // "new" is redundant

(int a, int b) t = new(); // ruled out by "use of struct default constructor"
Action a = new(); // no constructor found
```

### <a name="miscellaneous"></a><span data-ttu-id="bb07e-133">Různé</span><span class="sxs-lookup"><span data-stu-id="bb07e-133">Miscellaneous</span></span>

<span data-ttu-id="bb07e-134">`throw new()` není povoleno.</span><span class="sxs-lookup"><span data-stu-id="bb07e-134">`throw new()` is disallowed.</span></span>

<span data-ttu-id="bb07e-135">`new` cílového typu není povolený u binárních operátorů.</span><span class="sxs-lookup"><span data-stu-id="bb07e-135">Target-typed `new` is not allowed with binary operators.</span></span>

<span data-ttu-id="bb07e-136">Není povoleno, není-li k dispozici žádný typ k cíli: unární operátory, kolekce `foreach`, v `using`ve formě dekonstrukce ve výrazu `await` jako vlastnost anonymního typu (`new { Prop = new() }`) v příkazu `lock`, v `sizeof`v příkazu `fixed` v příkazu`new().field`v rámci člena přístup (`someDynamic.Method(new())`) v dynamicky odesílané operaci (`is`) v dotazu LINQ jako Operand operátoru `??`, jako levý operand operátoru ,  ...</span><span class="sxs-lookup"><span data-stu-id="bb07e-136">It is disallowed when there is no type to target: unary operators, collection of a `foreach`, in a `using`, in a deconstruction, in an `await` expression, as an anonymous type property (`new { Prop = new() }`), in a `lock` statement, in a `sizeof`, in a `fixed` statement, in a member access (`new().field`), in a dynamically dispatched operation (`someDynamic.Method(new())`), in a LINQ query, as the operand of the `is` operator, as the left operand of the `??` operator,  ...</span></span>

<span data-ttu-id="bb07e-137">Nepovoluje se taky jako `ref`.</span><span class="sxs-lookup"><span data-stu-id="bb07e-137">It is also disallowed as a `ref`.</span></span>

## <a name="drawbacks"></a><span data-ttu-id="bb07e-138">Nevýhody</span><span class="sxs-lookup"><span data-stu-id="bb07e-138">Drawbacks</span></span>
[drawbacks]: #drawbacks

<span data-ttu-id="bb07e-139">Došlo k nějakým potížím s cílovým typem `new` vytváření nových kategorií podstatných změn, ale už to máme s `null` a `default`a že nedošlo k významnému problému.</span><span class="sxs-lookup"><span data-stu-id="bb07e-139">There were some concerns with target-typed `new` creating new categories of breaking changes, but we already have that with `null` and `default`, and that has not been a significant problem.</span></span>

## <a name="alternatives"></a><span data-ttu-id="bb07e-140">Alternativy</span><span class="sxs-lookup"><span data-stu-id="bb07e-140">Alternatives</span></span>
[alternatives]: #alternatives

<span data-ttu-id="bb07e-141">Většina stížností na typy, které jsou příliš dlouhé na duplicity při inicializaci pole, se týká *typů argumentů* , které nejsou samotný typ, můžeme odvodit jenom argumenty typu jako `new Dictionary(...)` (nebo podobné) a odvodit argumenty typu místně z argumentů nebo inicializátoru kolekce.</span><span class="sxs-lookup"><span data-stu-id="bb07e-141">Most of complaints about types being too long to duplicate in field initialization is about *type arguments* not the type itself, we could infer only type arguments like `new Dictionary(...)` (or similar) and infer type arguments locally from arguments or the collection initializer.</span></span>

## <a name="questions"></a><span data-ttu-id="bb07e-142">Otázky</span><span class="sxs-lookup"><span data-stu-id="bb07e-142">Questions</span></span>
[questions]: #questions

- <span data-ttu-id="bb07e-143">Pokud byste měli zakázat použití ve stromech výrazů?</span><span class="sxs-lookup"><span data-stu-id="bb07e-143">Should we forbid usages in expression trees?</span></span> <span data-ttu-id="bb07e-144">žádné</span><span class="sxs-lookup"><span data-stu-id="bb07e-144">(no)</span></span>
- <span data-ttu-id="bb07e-145">Jak funkce komunikuje s argumenty `dynamic`?</span><span class="sxs-lookup"><span data-stu-id="bb07e-145">How the feature interacts with `dynamic` arguments?</span></span> <span data-ttu-id="bb07e-146">(žádné zvláštní zacházení)</span><span class="sxs-lookup"><span data-stu-id="bb07e-146">(no special treatment)</span></span>
- <span data-ttu-id="bb07e-147">Jak má IntelliSense pracovat s `new()`?</span><span class="sxs-lookup"><span data-stu-id="bb07e-147">How IntelliSense should work with `new()`?</span></span> <span data-ttu-id="bb07e-148">(pouze v případě, že existuje jeden cílový typ)</span><span class="sxs-lookup"><span data-stu-id="bb07e-148">(only when there is a single target-type)</span></span>

## <a name="design-meetings"></a><span data-ttu-id="bb07e-149">Schůzky pro návrh</span><span class="sxs-lookup"><span data-stu-id="bb07e-149">Design meetings</span></span>

- [<span data-ttu-id="bb07e-150">LDM-2017-10-18</span><span class="sxs-lookup"><span data-stu-id="bb07e-150">LDM-2017-10-18</span></span>](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-10-18.md#100)
- [<span data-ttu-id="bb07e-151">LDM-2018-05-21</span><span class="sxs-lookup"><span data-stu-id="bb07e-151">LDM-2018-05-21</span></span>](https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-05-21.md)
- [<span data-ttu-id="bb07e-152">LDM-2018-06-25</span><span class="sxs-lookup"><span data-stu-id="bb07e-152">LDM-2018-06-25</span></span>](https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-06-25.md)
- [<span data-ttu-id="bb07e-153">LDM-2018-08-22</span><span class="sxs-lookup"><span data-stu-id="bb07e-153">LDM-2018-08-22</span></span>](https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-08-22.md#target-typed-new)
- [<span data-ttu-id="bb07e-154">LDM-2018-10-17</span><span class="sxs-lookup"><span data-stu-id="bb07e-154">LDM-2018-10-17</span></span>](https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md)
