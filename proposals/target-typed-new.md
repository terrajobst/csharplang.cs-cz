---
ms.openlocfilehash: 4e2a536bab00859b003e8d967cb1927a99a9fa21
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484536"
---

# <a name="target-typed-new-expressions"></a><span data-ttu-id="9e5bf-101">Výrazy `new` s cílovým typem</span><span class="sxs-lookup"><span data-stu-id="9e5bf-101">Target-typed `new` expressions</span></span>

* <span data-ttu-id="9e5bf-102">Navrženo [x]</span><span class="sxs-lookup"><span data-stu-id="9e5bf-102">[x] Proposed</span></span>
* <span data-ttu-id="9e5bf-103">[x] [prototyp](https://github.com/alrz/roslyn/tree/features/target-typed-new)</span><span class="sxs-lookup"><span data-stu-id="9e5bf-103">[x] [Prototype](https://github.com/alrz/roslyn/tree/features/target-typed-new)</span></span>
* <span data-ttu-id="9e5bf-104">[] Implementace</span><span class="sxs-lookup"><span data-stu-id="9e5bf-104">[ ] Implementation</span></span>
* <span data-ttu-id="9e5bf-105">[] Specifikace</span><span class="sxs-lookup"><span data-stu-id="9e5bf-105">[ ] Specification</span></span>

## <a name="summary"></a><span data-ttu-id="9e5bf-106">Souhrn</span><span class="sxs-lookup"><span data-stu-id="9e5bf-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="9e5bf-107">Nevyžadovat specifikaci typu pro konstruktory, když je typ znám.</span><span class="sxs-lookup"><span data-stu-id="9e5bf-107">Do not require type specification for constructors when the type is known.</span></span> 

## <a name="motivation"></a><span data-ttu-id="9e5bf-108">Motivační</span><span class="sxs-lookup"><span data-stu-id="9e5bf-108">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="9e5bf-109">Povoluje inicializaci pole bez duplikace typu.</span><span class="sxs-lookup"><span data-stu-id="9e5bf-109">Allow field initialization without duplicating the type.</span></span>
```cs
Dictionary<string, List<int>> field = new() {
    { "item1", new() { 1, 2, 3 } }
};
```
<span data-ttu-id="9e5bf-110">Povolí vynechání typu, když se dá odvodit z použití.</span><span class="sxs-lookup"><span data-stu-id="9e5bf-110">Allow omitting the type when it can be inferred from usage.</span></span>
```cs
XmlReader.Create(reader, new() { IgnoreWhitespace = true });
```
<span data-ttu-id="9e5bf-111">Vytvořte instanci objektu bez nutnosti hláskování typu.</span><span class="sxs-lookup"><span data-stu-id="9e5bf-111">Instantiate an object without spelling out the type.</span></span>
```cs
private readonly static object s_syncObj = new();
```
## <a name="detailed-design"></a><span data-ttu-id="9e5bf-112">Podrobný návrh</span><span class="sxs-lookup"><span data-stu-id="9e5bf-112">Detailed design</span></span>
[design]: #detailed-design

<span data-ttu-id="9e5bf-113">Syntaxe *object_creation_expression* se upraví tak, aby byl *typ* volitelný, pokud jsou k dispozici závorky.</span><span class="sxs-lookup"><span data-stu-id="9e5bf-113">The *object_creation_expression* syntax would be modified to make the *type* optional when parentheses are present.</span></span> <span data-ttu-id="9e5bf-114">Tato je nutná k vyřešení nejednoznačnosti pomocí *anonymous_object_creation_expression*.</span><span class="sxs-lookup"><span data-stu-id="9e5bf-114">This is required to address the ambiguity with *anonymous_object_creation_expression*.</span></span>
```antlr
object_creation_expression
    : 'new' type? '(' argument_list? ')' object_or_collection_initializer?
    | 'new' type object_or_collection_initializer
    ;
```
<span data-ttu-id="9e5bf-115">`new` cílového typu je převoditelná na libovolný typ.</span><span class="sxs-lookup"><span data-stu-id="9e5bf-115">A target-typed `new` is convertible to any type.</span></span> <span data-ttu-id="9e5bf-116">V důsledku toho nepřispívá k řešení přetížení.</span><span class="sxs-lookup"><span data-stu-id="9e5bf-116">As a result, it does not contribute to overload resolution.</span></span> <span data-ttu-id="9e5bf-117">To se většinou vyhnout nepředvídatelným změnám.</span><span class="sxs-lookup"><span data-stu-id="9e5bf-117">This is mainly to avoid unpredictable breaking changes.</span></span>

<span data-ttu-id="9e5bf-118">Seznam argumentů a inicializační výrazy budou vázány po určení typu.</span><span class="sxs-lookup"><span data-stu-id="9e5bf-118">The argument list and the initializer expressions will be bound after the type is determined.</span></span>

<span data-ttu-id="9e5bf-119">Typ výrazu by byl odvozen z cílového typu, který by byl vyžadován pro jednu z následujících:</span><span class="sxs-lookup"><span data-stu-id="9e5bf-119">The type of the expression would be inferred from the target-type which would be required to be one of the following:</span></span>

- <span data-ttu-id="9e5bf-120">**Libovolný typ struktury**</span><span class="sxs-lookup"><span data-stu-id="9e5bf-120">**Any struct type**</span></span>
- <span data-ttu-id="9e5bf-121">**Libovolný typ odkazu**</span><span class="sxs-lookup"><span data-stu-id="9e5bf-121">**Any reference type**</span></span>
- <span data-ttu-id="9e5bf-122">**Libovolný parametr typu** s konstruktorem nebo omezením `struct`</span><span class="sxs-lookup"><span data-stu-id="9e5bf-122">**Any type parameter** with a constructor or a `struct` constraint</span></span>

<span data-ttu-id="9e5bf-123">s následujícími výjimkami:</span><span class="sxs-lookup"><span data-stu-id="9e5bf-123">with the following exceptions:</span></span>

- <span data-ttu-id="9e5bf-124">**Výčtové typy:** ne všechny typy výčtů obsahují konstantu nula, takže by měl být vhodné použít explicitního člena výčtu.</span><span class="sxs-lookup"><span data-stu-id="9e5bf-124">**Enum types:** not all enum types contain the constant zero, so it should be desirable to use the explicit enum member.</span></span>
- <span data-ttu-id="9e5bf-125">**Typy rozhraní:** jedná se o funkci mezery a měla by být vhodnější výslovně uvést typ.</span><span class="sxs-lookup"><span data-stu-id="9e5bf-125">**Interface types:** this is a niche feature and it should be preferable to explicitly mention the type.</span></span>
- <span data-ttu-id="9e5bf-126">**Typy polí:** pole potřebují pro zadání délky speciální syntaxi.</span><span class="sxs-lookup"><span data-stu-id="9e5bf-126">**Array types:** arrays need a special syntax to provide the length.</span></span>
- <span data-ttu-id="9e5bf-127">**Výchozí konstruktor struct**: Tato pravidla vyprší všem primitivním typům a většině hodnotových typů.</span><span class="sxs-lookup"><span data-stu-id="9e5bf-127">**Struct default constructor**: this rules out all primitive types and most value types.</span></span> <span data-ttu-id="9e5bf-128">Pokud jste chtěli použít výchozí hodnotu takových typů, můžete místo toho napsat `default`.</span><span class="sxs-lookup"><span data-stu-id="9e5bf-128">If you wanted to use the default value of such types you could write `default` instead.</span></span>

<span data-ttu-id="9e5bf-129">Všechny ostatní typy, které nejsou povoleny v *object_creation_expression* jsou vyloučeny také, například typy ukazatelů.</span><span class="sxs-lookup"><span data-stu-id="9e5bf-129">All the other types that are not permitted in the *object_creation_expression* are excluded as well, for instance, pointer types.</span></span>

> <span data-ttu-id="9e5bf-130">**Otevření problému:** Pokud chcete, abychom jako cílový typ povolili delegáty a řazené kolekce členů?</span><span class="sxs-lookup"><span data-stu-id="9e5bf-130">**Open Issue:** should we allow delegates and tuples as the target-type?</span></span>

<span data-ttu-id="9e5bf-131">Výše uvedená pravidla zahrnují delegáty (typ odkazu) a řazené kolekce členů (typ struktury).</span><span class="sxs-lookup"><span data-stu-id="9e5bf-131">The above rules include delegates (a reference type) and tuples (a struct type).</span></span> <span data-ttu-id="9e5bf-132">I když jsou oba typy constructible, je-li typ odvozen, anonymní funkce nebo literál řazené kolekce členů již lze použít.</span><span class="sxs-lookup"><span data-stu-id="9e5bf-132">Although both types are constructible, if the type is inferable, an anonymous function or a tuple literal can already be used.</span></span>
```cs
(int a, int b) t = new(1, 2); // "new" is redundant
Action a = new(() => {}); // "new" is redundant

(int a, int b) t = new(); // ruled out by "use of struct default constructor"
Action a = new(); // no constructor found

var x = new() == (1, 2); // ruled out by "use of struct default constructor"
var x = new(1, 2) == (1, 2) // "new" is redundant
```


> <span data-ttu-id="9e5bf-133">**Otevření problému:** měli bychom `throw new()` jako cílový typ používat jako `Exception`?</span><span class="sxs-lookup"><span data-stu-id="9e5bf-133">**Open Issue:** should we allow `throw new()` with `Exception` as the target-type?</span></span>

<span data-ttu-id="9e5bf-134">V dnešní době jsme `throw null`i, ale ne `throw default` (i když by to vedlo ke stejnému účinku).</span><span class="sxs-lookup"><span data-stu-id="9e5bf-134">We have `throw null` today, but not `throw default` (though it would have the same effect).</span></span> <span data-ttu-id="9e5bf-135">Na druhé straně `throw new()` možné být ve skutečnosti užitečné jako zkratky pro `throw new Exception(...)`.</span><span class="sxs-lookup"><span data-stu-id="9e5bf-135">On the other hand, `throw new()` could be actually useful as a shorthand for `throw new Exception(...)`.</span></span> <span data-ttu-id="9e5bf-136">Všimněte si, že už je povolená aktuální specifikací.</span><span class="sxs-lookup"><span data-stu-id="9e5bf-136">Note that it is already allowed by the current specification.</span></span> <span data-ttu-id="9e5bf-137">`Exception` je odkazový typ a specifikace pro příkaz throw říká, že je výraz převeden na `Exception`.</span><span class="sxs-lookup"><span data-stu-id="9e5bf-137">`Exception` is a reference type, and the specification for the throw statement says that the expression is converted to `Exception`.</span></span>

> <span data-ttu-id="9e5bf-138">**Otevření problému:** je potřeba, abychom povolili použití cílového typu `new` s uživatelsky definovaným porovnáním a aritmetickými operátory?</span><span class="sxs-lookup"><span data-stu-id="9e5bf-138">**Open Issue:** should we allow usages of a target-typed `new` with user-defined comparison and arithmetic operators?</span></span>

<span data-ttu-id="9e5bf-139">Pro účely porovnání `default` podporuje pouze rovnost (uživatelsky definované a předdefinované) operátory.</span><span class="sxs-lookup"><span data-stu-id="9e5bf-139">For comparison, `default` only supports equality (user-defined and built-in) operators.</span></span> <span data-ttu-id="9e5bf-140">Měla by smysl podporovat i jiné operátory pro `new()`?</span><span class="sxs-lookup"><span data-stu-id="9e5bf-140">Would it make sense to support other operators for `new()` as well?</span></span>

## <a name="drawbacks"></a><span data-ttu-id="9e5bf-141">Nevýhody</span><span class="sxs-lookup"><span data-stu-id="9e5bf-141">Drawbacks</span></span>
[drawbacks]: #drawbacks

<span data-ttu-id="9e5bf-142">Žádné.</span><span class="sxs-lookup"><span data-stu-id="9e5bf-142">None.</span></span>

## <a name="alternatives"></a><span data-ttu-id="9e5bf-143">Alternativy</span><span class="sxs-lookup"><span data-stu-id="9e5bf-143">Alternatives</span></span>
[alternatives]: #alternatives

<span data-ttu-id="9e5bf-144">Většina stížností na typy, které jsou příliš dlouhé na duplicity při inicializaci pole, se týká *typů argumentů* , které nejsou samotný typ, můžeme odvodit jenom argumenty typu jako `new Dictionary(...)` (nebo podobné) a odvodit argumenty typu místně z argumentů nebo inicializátoru kolekce.</span><span class="sxs-lookup"><span data-stu-id="9e5bf-144">Most of complaints about types being too long to duplicate in field initialization is about *type arguments* not the type itself, we could infer only type arguments like `new Dictionary(...)` (or similar) and infer type arguments locally from arguments or the collection initializer.</span></span>

## <a name="questions"></a><span data-ttu-id="9e5bf-145">Otázky</span><span class="sxs-lookup"><span data-stu-id="9e5bf-145">Questions</span></span>
[questions]: #questions

- <span data-ttu-id="9e5bf-146">Pokud byste měli zakázat použití ve stromech výrazů?</span><span class="sxs-lookup"><span data-stu-id="9e5bf-146">Should we forbid usages in expression trees?</span></span> <span data-ttu-id="9e5bf-147">žádné</span><span class="sxs-lookup"><span data-stu-id="9e5bf-147">(no)</span></span>
- <span data-ttu-id="9e5bf-148">Jak funkce komunikuje s argumenty `dynamic`?</span><span class="sxs-lookup"><span data-stu-id="9e5bf-148">How the feature interacts with `dynamic` arguments?</span></span> <span data-ttu-id="9e5bf-149">(žádné zvláštní zacházení)</span><span class="sxs-lookup"><span data-stu-id="9e5bf-149">(no special treatment)</span></span>
- <span data-ttu-id="9e5bf-150">Jak má IntelliSense pracovat s `new()`?</span><span class="sxs-lookup"><span data-stu-id="9e5bf-150">How IntelliSense should work with `new()`?</span></span> <span data-ttu-id="9e5bf-151">(pouze v případě, že existuje jeden cílový typ)</span><span class="sxs-lookup"><span data-stu-id="9e5bf-151">(only when there is a single target-type)</span></span>
## <a name="design-meetings"></a><span data-ttu-id="9e5bf-152">Schůzky pro návrh</span><span class="sxs-lookup"><span data-stu-id="9e5bf-152">Design meetings</span></span>

- [<span data-ttu-id="9e5bf-153">LDM-2017-10-18</span><span class="sxs-lookup"><span data-stu-id="9e5bf-153">LDM-2017-10-18</span></span>](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-10-18.md#100)
