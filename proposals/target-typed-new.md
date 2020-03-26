---
ms.openlocfilehash: f000dda7eeb1c4f17c26f94c326a12a9d0014288
ms.sourcegitcommit: 1e1c7c72b156e2fbc54d6d6ac8d21bca9934d8d2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/26/2020
ms.locfileid: "80281967"
---

# <a name="target-typed-new-expressions"></a><span data-ttu-id="e2a9f-101">Výrazy `new` s cílovým typem</span><span class="sxs-lookup"><span data-stu-id="e2a9f-101">Target-typed `new` expressions</span></span>

* <span data-ttu-id="e2a9f-102">Navrženo [x]</span><span class="sxs-lookup"><span data-stu-id="e2a9f-102">[x] Proposed</span></span>
* <span data-ttu-id="e2a9f-103">[x] [prototyp](https://github.com/alrz/roslyn/tree/features/target-typed-new)</span><span class="sxs-lookup"><span data-stu-id="e2a9f-103">[x] [Prototype](https://github.com/alrz/roslyn/tree/features/target-typed-new)</span></span>
* <span data-ttu-id="e2a9f-104">[] Implementace</span><span class="sxs-lookup"><span data-stu-id="e2a9f-104">[ ] Implementation</span></span>
* <span data-ttu-id="e2a9f-105">[] Specifikace</span><span class="sxs-lookup"><span data-stu-id="e2a9f-105">[ ] Specification</span></span>

## <a name="summary"></a><span data-ttu-id="e2a9f-106">Souhrn</span><span class="sxs-lookup"><span data-stu-id="e2a9f-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="e2a9f-107">Nevyžadovat specifikaci typu pro konstruktory, když je typ znám.</span><span class="sxs-lookup"><span data-stu-id="e2a9f-107">Do not require type specification for constructors when the type is known.</span></span> 

## <a name="motivation"></a><span data-ttu-id="e2a9f-108">Motivační</span><span class="sxs-lookup"><span data-stu-id="e2a9f-108">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="e2a9f-109">Povoluje inicializaci pole bez duplikace typu.</span><span class="sxs-lookup"><span data-stu-id="e2a9f-109">Allow field initialization without duplicating the type.</span></span>
```cs
Dictionary<string, List<int>> field = new() {
    { "item1", new() { 1, 2, 3 } }
};
```

<span data-ttu-id="e2a9f-110">Povolí vynechání typu, když se dá odvodit z použití.</span><span class="sxs-lookup"><span data-stu-id="e2a9f-110">Allow omitting the type when it can be inferred from usage.</span></span>
```cs
XmlReader.Create(reader, new() { IgnoreWhitespace = true });
```

<span data-ttu-id="e2a9f-111">Vytvořte instanci objektu bez nutnosti hláskování typu.</span><span class="sxs-lookup"><span data-stu-id="e2a9f-111">Instantiate an object without spelling out the type.</span></span>
```cs
private readonly static object s_syncObj = new();
```

## <a name="specification"></a><span data-ttu-id="e2a9f-112">Specifikace</span><span class="sxs-lookup"><span data-stu-id="e2a9f-112">Specification</span></span>
[design]: #detailed-design

<span data-ttu-id="e2a9f-113">Nové syntaktické formuláře, *target_typed_new* *object_creation_expression* , jsou přijaty, ve kterém je *typ* volitelný.</span><span class="sxs-lookup"><span data-stu-id="e2a9f-113">A new syntactic form, *target_typed_new* of the *object_creation_expression* is accepted in which the *type* is optional.</span></span>

```antlr
object_creation_expression
    : 'new' type '(' argument_list? ')' object_or_collection_initializer?
    | 'new' type object_or_collection_initializer
    | target_typed_new
    ;
target_typed_new
    : 'new' '(' argument_list? ')' object_or_collection_initializer?
    ;
```

<span data-ttu-id="e2a9f-114">Výraz *target_typed_new* nemá typ.</span><span class="sxs-lookup"><span data-stu-id="e2a9f-114">A *target_typed_new* expression does not have a type.</span></span> <span data-ttu-id="e2a9f-115">Existuje však nový *Převod vytvoření objektu* , který je implicitní převod z výrazu, který existuje z *target_typed_new* na každý typ.</span><span class="sxs-lookup"><span data-stu-id="e2a9f-115">However, there is a new *object creation conversion* that is an implicit conversion from expression, that exists from a *target_typed_new* to every type.</span></span>

<span data-ttu-id="e2a9f-116">Pokud `T` je instance `System.Nullable`, je typ zadaného cílového typu `T``T`nadřízený typ `T0`.</span><span class="sxs-lookup"><span data-stu-id="e2a9f-116">Given a target type `T`, the type `T0` is `T`'s underlying type if `T` is an instance of `System.Nullable`.</span></span> <span data-ttu-id="e2a9f-117">Jinak `T0` `T`.</span><span class="sxs-lookup"><span data-stu-id="e2a9f-117">Otherwise `T0` is `T`.</span></span> <span data-ttu-id="e2a9f-118">Význam výrazu *target_typed_new* , který je převeden na typ `T` je stejný jako význam odpovídající *object_creation_expression* , který specifikuje `T0` jako typ.</span><span class="sxs-lookup"><span data-stu-id="e2a9f-118">The meaning of a *target_typed_new* expression that is converted to the type `T` is the same as the meaning of a corresponding *object_creation_expression* that specifies `T0` as the type.</span></span>

<span data-ttu-id="e2a9f-119">Jedná se o chybu při kompilaci, pokud je *target_typed_new* použito jako operand unárního nebo binárního operátoru, nebo pokud se používá, pokud není předmětem *převodu na vytvoření objektu*.</span><span class="sxs-lookup"><span data-stu-id="e2a9f-119">It is a compile-time error if a *target_typed_new* is used as an operand of a unary or binary operator, or if it is used where it is not subject to an *object creation conversion*.</span></span>

> <span data-ttu-id="e2a9f-120">**Otevření problému:** Pokud chcete, abychom jako cílový typ povolili delegáty a řazené kolekce členů?</span><span class="sxs-lookup"><span data-stu-id="e2a9f-120">**Open Issue:** should we allow delegates and tuples as the target-type?</span></span>

<span data-ttu-id="e2a9f-121">Výše uvedená pravidla zahrnují delegáty (typ odkazu) a řazené kolekce členů (typ struktury).</span><span class="sxs-lookup"><span data-stu-id="e2a9f-121">The above rules include delegates (a reference type) and tuples (a struct type).</span></span> <span data-ttu-id="e2a9f-122">I když jsou oba typy constructible, je-li typ odvozen, anonymní funkce nebo literál řazené kolekce členů již lze použít.</span><span class="sxs-lookup"><span data-stu-id="e2a9f-122">Although both types are constructible, if the type is inferable, an anonymous function or a tuple literal can already be used.</span></span>
```cs
(int a, int b) t = new(1, 2); // "new" is redundant
Action a = new(() => {}); // "new" is redundant

(int a, int b) t = new(); // OK; same as (0, 0)
Action a = new(); // no constructor found
```

### <a name="miscellaneous"></a><span data-ttu-id="e2a9f-123">Různé</span><span class="sxs-lookup"><span data-stu-id="e2a9f-123">Miscellaneous</span></span>

<span data-ttu-id="e2a9f-124">Níže jsou uvedené důsledky této specifikace:</span><span class="sxs-lookup"><span data-stu-id="e2a9f-124">The following are consequences of the specification:</span></span>

- <span data-ttu-id="e2a9f-125">`throw new()` je povolený (typ cíle je `System.Exception`).</span><span class="sxs-lookup"><span data-stu-id="e2a9f-125">`throw new()` is allowed (the target type is `System.Exception`)</span></span>
- <span data-ttu-id="e2a9f-126">`new` cílového typu není povolený u binárních operátorů.</span><span class="sxs-lookup"><span data-stu-id="e2a9f-126">Target-typed `new` is not allowed with binary operators.</span></span>
- <span data-ttu-id="e2a9f-127">Není povoleno, není-li k dispozici žádný typ k cíli: unární operátory, kolekce `foreach`, v `using`ve formě dekonstrukce ve výrazu `await` jako vlastnost anonymního typu (`new { Prop = new() }`) v příkazu `lock`, v `sizeof`v příkazu `fixed` v příkazu`new().field`v rámci člena přístup (`someDynamic.Method(new())`) v dynamicky odesílané operaci (`is`) v dotazu LINQ jako Operand operátoru `??`, jako levý operand operátoru ,  ...</span><span class="sxs-lookup"><span data-stu-id="e2a9f-127">It is disallowed when there is no type to target: unary operators, collection of a `foreach`, in a `using`, in a deconstruction, in an `await` expression, as an anonymous type property (`new { Prop = new() }`), in a `lock` statement, in a `sizeof`, in a `fixed` statement, in a member access (`new().field`), in a dynamically dispatched operation (`someDynamic.Method(new())`), in a LINQ query, as the operand of the `is` operator, as the left operand of the `??` operator,  ...</span></span>
- <span data-ttu-id="e2a9f-128">Nepovoluje se taky jako `ref`.</span><span class="sxs-lookup"><span data-stu-id="e2a9f-128">It is also disallowed as a `ref`.</span></span>
- <span data-ttu-id="e2a9f-129">Následující typy typů nejsou povolené jako cíle převodu.</span><span class="sxs-lookup"><span data-stu-id="e2a9f-129">The following kinds of types are not permitted as targets of the conversion</span></span>
  - <span data-ttu-id="e2a9f-130">**Typy výčtu:** `new()` budou fungovat (jako `new Enum()` funguje jako výchozí hodnota), ale `new(1)` nebudou fungovat, protože výčtové typy nemají konstruktor.</span><span class="sxs-lookup"><span data-stu-id="e2a9f-130">**Enum types:** `new()` will work (as `new Enum()` works to give the default value), but `new(1)` will not work as enum types do not have a constructor.</span></span>
  - <span data-ttu-id="e2a9f-131">**Typy rozhraní:** To bude fungovat stejně jako odpovídající výraz vytvoření pro typy modelu COM.</span><span class="sxs-lookup"><span data-stu-id="e2a9f-131">**Interface types:** This would work the same as the corresponding creation expression for COM types.</span></span>
  - <span data-ttu-id="e2a9f-132">**Typy polí:** pole potřebují pro zadání délky speciální syntaxi.</span><span class="sxs-lookup"><span data-stu-id="e2a9f-132">**Array types:** arrays need a special syntax to provide the length.</span></span>    
  - <span data-ttu-id="e2a9f-133">**dynamické:** nepovolujeme `new dynamic()`, takže nepovolujeme `new()` s `dynamic` jako cílový typ.</span><span class="sxs-lookup"><span data-stu-id="e2a9f-133">**dynamic:** we don't allow `new dynamic()`, so we don't allow `new()` with `dynamic` as a target type.</span></span>
  - <span data-ttu-id="e2a9f-134">**řazené kolekce členů:** Mají stejný význam jako vytvoření objektu pomocí nadřazeného typu.</span><span class="sxs-lookup"><span data-stu-id="e2a9f-134">**tuples:** These have the same meaning as an object creation using the underlying type.</span></span>
  - <span data-ttu-id="e2a9f-135">Všechny ostatní typy, které nejsou povoleny v *object_creation_expression* jsou vyloučeny také, například typy ukazatelů.</span><span class="sxs-lookup"><span data-stu-id="e2a9f-135">All the other types that are not permitted in the *object_creation_expression* are excluded as well, for instance, pointer types.</span></span>   

## <a name="drawbacks"></a><span data-ttu-id="e2a9f-136">Nevýhody</span><span class="sxs-lookup"><span data-stu-id="e2a9f-136">Drawbacks</span></span>
[drawbacks]: #drawbacks

<span data-ttu-id="e2a9f-137">Došlo k nějakým potížím s cílovým typem `new` vytváření nových kategorií podstatných změn, ale už to máme s `null` a `default`a že nedošlo k významnému problému.</span><span class="sxs-lookup"><span data-stu-id="e2a9f-137">There were some concerns with target-typed `new` creating new categories of breaking changes, but we already have that with `null` and `default`, and that has not been a significant problem.</span></span>

## <a name="alternatives"></a><span data-ttu-id="e2a9f-138">Alternativy</span><span class="sxs-lookup"><span data-stu-id="e2a9f-138">Alternatives</span></span>
[alternatives]: #alternatives

<span data-ttu-id="e2a9f-139">Většina stížností na typy, které jsou příliš dlouhé na duplicity při inicializaci pole, se týká *typů argumentů* , které nejsou samotný typ, můžeme odvodit jenom argumenty typu jako `new Dictionary(...)` (nebo podobné) a odvodit argumenty typu místně z argumentů nebo inicializátoru kolekce.</span><span class="sxs-lookup"><span data-stu-id="e2a9f-139">Most of complaints about types being too long to duplicate in field initialization is about *type arguments* not the type itself, we could infer only type arguments like `new Dictionary(...)` (or similar) and infer type arguments locally from arguments or the collection initializer.</span></span>

## <a name="questions"></a><span data-ttu-id="e2a9f-140">Otázky</span><span class="sxs-lookup"><span data-stu-id="e2a9f-140">Questions</span></span>
[questions]: #questions

- <span data-ttu-id="e2a9f-141">Pokud byste měli zakázat použití ve stromech výrazů?</span><span class="sxs-lookup"><span data-stu-id="e2a9f-141">Should we forbid usages in expression trees?</span></span> <span data-ttu-id="e2a9f-142">žádné</span><span class="sxs-lookup"><span data-stu-id="e2a9f-142">(no)</span></span>
- <span data-ttu-id="e2a9f-143">Jak funkce komunikuje s argumenty `dynamic`?</span><span class="sxs-lookup"><span data-stu-id="e2a9f-143">How the feature interacts with `dynamic` arguments?</span></span> <span data-ttu-id="e2a9f-144">(žádné zvláštní zacházení)</span><span class="sxs-lookup"><span data-stu-id="e2a9f-144">(no special treatment)</span></span>
- <span data-ttu-id="e2a9f-145">Jak má IntelliSense pracovat s `new()`?</span><span class="sxs-lookup"><span data-stu-id="e2a9f-145">How IntelliSense should work with `new()`?</span></span> <span data-ttu-id="e2a9f-146">(pouze v případě, že existuje jeden cílový typ)</span><span class="sxs-lookup"><span data-stu-id="e2a9f-146">(only when there is a single target-type)</span></span>

## <a name="design-meetings"></a><span data-ttu-id="e2a9f-147">Schůzky pro návrh</span><span class="sxs-lookup"><span data-stu-id="e2a9f-147">Design meetings</span></span>

- [<span data-ttu-id="e2a9f-148">LDM-2017-10-18</span><span class="sxs-lookup"><span data-stu-id="e2a9f-148">LDM-2017-10-18</span></span>](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-10-18.md#100)
- [<span data-ttu-id="e2a9f-149">LDM-2018-05-21</span><span class="sxs-lookup"><span data-stu-id="e2a9f-149">LDM-2018-05-21</span></span>](https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-05-21.md)
- [<span data-ttu-id="e2a9f-150">LDM-2018-06-25</span><span class="sxs-lookup"><span data-stu-id="e2a9f-150">LDM-2018-06-25</span></span>](https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-06-25.md)
- [<span data-ttu-id="e2a9f-151">LDM-2018-08-22</span><span class="sxs-lookup"><span data-stu-id="e2a9f-151">LDM-2018-08-22</span></span>](https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-08-22.md#target-typed-new)
- [<span data-ttu-id="e2a9f-152">LDM-2018-10-17</span><span class="sxs-lookup"><span data-stu-id="e2a9f-152">LDM-2018-10-17</span></span>](https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md)
- [<span data-ttu-id="e2a9f-153">LDM-2020-03-25</span><span class="sxs-lookup"><span data-stu-id="e2a9f-153">LDM-2020-03-25</span></span>](https://github.com/dotnet/csharplang/blob/master/meetings/2020/LDM-2020-03-25.md)
