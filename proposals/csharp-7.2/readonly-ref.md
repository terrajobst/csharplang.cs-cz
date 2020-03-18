---
ms.openlocfilehash: 39fb0aab5e8bb0d422f25fd2e92ab3d8256d3f59
ms.sourcegitcommit: b8f1103eb686c5d82e294837c9386d9b667da292
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/29/2019
ms.locfileid: "79485110"
---
# <a name="readonly-references"></a><span data-ttu-id="03618-101">Odkazy jen pro čtení</span><span class="sxs-lookup"><span data-stu-id="03618-101">Readonly references</span></span>

* <span data-ttu-id="03618-102">Navrženo [x]</span><span class="sxs-lookup"><span data-stu-id="03618-102">[x] Proposed</span></span>
* <span data-ttu-id="03618-103">[x] prototyp</span><span class="sxs-lookup"><span data-stu-id="03618-103">[x] Prototype</span></span>
* <span data-ttu-id="03618-104">[x] implementace: spuštěno</span><span class="sxs-lookup"><span data-stu-id="03618-104">[x] Implementation: Started</span></span>
* <span data-ttu-id="03618-105">[] Specifikace: Nezahájeno</span><span class="sxs-lookup"><span data-stu-id="03618-105">[ ] Specification: Not Started</span></span>

## <a name="summary"></a><span data-ttu-id="03618-106">Souhrn</span><span class="sxs-lookup"><span data-stu-id="03618-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="03618-107">Funkce "odkazy jen pro čtení" je ve skutečnosti skupina funkcí, které využívají efektivitu předávání proměnných podle odkazu, ale bez odhalení dat do úprav:</span><span class="sxs-lookup"><span data-stu-id="03618-107">The "readonly references" feature is actually a group of features that leverage the efficiency of passing variables by reference, but without exposing the data to modifications:</span></span>  
- <span data-ttu-id="03618-108">parametry `in`</span><span class="sxs-lookup"><span data-stu-id="03618-108">`in` parameters</span></span>
- <span data-ttu-id="03618-109">`ref readonly` vrátí</span><span class="sxs-lookup"><span data-stu-id="03618-109">`ref readonly` returns</span></span>
- <span data-ttu-id="03618-110">`readonly` struktury</span><span class="sxs-lookup"><span data-stu-id="03618-110">`readonly` structs</span></span>
- <span data-ttu-id="03618-111">metody rozšíření `ref`/`in`</span><span class="sxs-lookup"><span data-stu-id="03618-111">`ref`/`in` extension methods</span></span>
- <span data-ttu-id="03618-112">`ref readonly` národní prostředí</span><span class="sxs-lookup"><span data-stu-id="03618-112">`ref readonly` locals</span></span>
- <span data-ttu-id="03618-113">`ref` podmíněné výrazy</span><span class="sxs-lookup"><span data-stu-id="03618-113">`ref` conditional expressions</span></span>

## <a name="passing-arguments-as-readonly-references"></a><span data-ttu-id="03618-114">Předávání argumentů jako odkazů jen pro čtení.</span><span class="sxs-lookup"><span data-stu-id="03618-114">Passing arguments as readonly references.</span></span>

<span data-ttu-id="03618-115">K dispozici je existující návrh, který se dotýká tohoto tématu https://github.com/dotnet/roslyn/issues/115 jako speciální případ parametrů ReadOnly, aniž by došlo k mnoha podrobnostem.</span><span class="sxs-lookup"><span data-stu-id="03618-115">There is an existing proposal that touches this topic https://github.com/dotnet/roslyn/issues/115 as a special case of readonly parameters without going into many details.</span></span>
<span data-ttu-id="03618-116">Tady bych chtěl potvrdit, že nápad sám o sobě není příliš nový.</span><span class="sxs-lookup"><span data-stu-id="03618-116">Here I just want to acknowledge that the idea by itself is not very new.</span></span>

### <a name="motivation"></a><span data-ttu-id="03618-117">Motivační</span><span class="sxs-lookup"><span data-stu-id="03618-117">Motivation</span></span>

<span data-ttu-id="03618-118">Před touto funkcí C# neexistoval účinný způsob, jak vyjádřit potřebu předat proměnné struktury do volání metody pro účely ReadOnly bez úmyslné úpravy.</span><span class="sxs-lookup"><span data-stu-id="03618-118">Prior to this feature C# did not have an efficient way of expressing a desire to pass struct variables into method calls for readonly purposes with no intention of modifying.</span></span> <span data-ttu-id="03618-119">Hodnota argumentu Regular by pass zahrnuje kopírování, které přidává nepotřebné náklady.</span><span class="sxs-lookup"><span data-stu-id="03618-119">Regular by-value argument passing implies copying, which adds unnecessary costs.</span></span>  <span data-ttu-id="03618-120">Který řídí, aby uživatelé použili argument-REF předávající a spoléhají na komentáře/dokumentaci, aby označovali, že volaný by data neměla být ovlivněna.</span><span class="sxs-lookup"><span data-stu-id="03618-120">That drives users to use by-ref argument passing and rely on comments/documentation to indicate that the data is not supposed to be mutated by the callee.</span></span> <span data-ttu-id="03618-121">Nejedná se o dobré řešení z mnoha důvodů.</span><span class="sxs-lookup"><span data-stu-id="03618-121">It is not a good solution for many reasons.</span></span>  
<span data-ttu-id="03618-122">Příklady jsou řady-Vector/matic Math Operators v grafických knihovnách, jako je například rozhraní [XNA](https://msdn.microsoft.com/library/bb194944.aspx) , se nazývají referenční operandy čistě z důvodů výkonu.</span><span class="sxs-lookup"><span data-stu-id="03618-122">The examples are numerous - vector/matrix math operators in graphics libraries like [XNA](https://msdn.microsoft.com/library/bb194944.aspx) are known to have ref operands purely because of performance considerations.</span></span> <span data-ttu-id="03618-123">V samotném kompilátoru Roslyn existuje kód, který používá struktury k zamezení přidělení a pak je předává odkazům, aby nedocházelo ke kopírování nákladů.</span><span class="sxs-lookup"><span data-stu-id="03618-123">There is code in Roslyn compiler itself that uses structs to avoid allocations and then passes them by reference to avoid copying costs.</span></span>

### <a name="solution-in-parameters"></a><span data-ttu-id="03618-124">Řešení (`in` parametry)</span><span class="sxs-lookup"><span data-stu-id="03618-124">Solution (`in` parameters)</span></span>

<span data-ttu-id="03618-125">Podobně jako u parametrů `out` jsou `in` parametry předány jako spravované odkazy s dalšími zárukami od volaného.</span><span class="sxs-lookup"><span data-stu-id="03618-125">Similarly to the `out` parameters, `in` parameters are passed as managed references with additional guarantees from the callee.</span></span>  
<span data-ttu-id="03618-126">Na rozdíl od parametrů `out`, které _musí_ před jakýmkoli jiným použitím přiřazovat volaný, `in` parametry nelze přiřadit volaným.</span><span class="sxs-lookup"><span data-stu-id="03618-126">Unlike `out` parameters which _must_ be assigned by the callee before any other use, `in` parameters cannot be assigned by the callee at all.</span></span>

<span data-ttu-id="03618-127">V důsledku toho `in` parametry umožňují efektivitu předávání nepřímých argumentů bez vystavení argumentů pro mutace volanými.</span><span class="sxs-lookup"><span data-stu-id="03618-127">As a result `in` parameters allow for effectiveness of indirect argument passing without exposing arguments to mutations by the callee.</span></span>

### <a name="declaring-in-parameters"></a><span data-ttu-id="03618-128">Deklarace `in`ch parametrů</span><span class="sxs-lookup"><span data-stu-id="03618-128">Declaring `in` parameters</span></span>

<span data-ttu-id="03618-129">parametry `in` jsou deklarovány pomocí klíčového slova `in` jako modifikátor v podpisu parametru.</span><span class="sxs-lookup"><span data-stu-id="03618-129">`in` parameters are declared by using `in` keyword as a modifier in the parameter signature.</span></span>

<span data-ttu-id="03618-130">Pro všechny účely se parametr `in` považuje za `readonly` proměnnou.</span><span class="sxs-lookup"><span data-stu-id="03618-130">For all purposes the `in` parameter is treated as a `readonly` variable.</span></span> <span data-ttu-id="03618-131">Většina omezení pro použití parametrů `in` v rámci metody je stejná jako u polí `readonly`.</span><span class="sxs-lookup"><span data-stu-id="03618-131">Most of the restrictions on the use of `in` parameters inside the method are the same as with `readonly` fields.</span></span>

> <span data-ttu-id="03618-132">Ve skutečnosti může představovat parametr `in` `readonly` pole.</span><span class="sxs-lookup"><span data-stu-id="03618-132">Indeed an `in` parameter may represent a `readonly` field.</span></span> <span data-ttu-id="03618-133">Podobnost omezení nepředstavuje kodopad.</span><span class="sxs-lookup"><span data-stu-id="03618-133">Similarity of restrictions is not a coincidence.</span></span>

<span data-ttu-id="03618-134">Například pole parametru `in`, který má typ struktury, jsou rekurzivně klasifikovány jako `readonly` proměnné.</span><span class="sxs-lookup"><span data-stu-id="03618-134">For example fields of an `in` parameter which has a struct type are all recursively classified as `readonly` variables .</span></span>

```csharp
static Vector3 Add (in Vector3 v1, in Vector3 v2)
{
    // not OK!!
    v1 = default(Vector3);

    // not OK!!
    v1.X = 0;

    // not OK!!
    foo(ref v1.X);

    // OK
    return new Vector3(v1.X + v2.X, v1.Y + v2.Y, v1.Z + v2.Z);
}
```

- <span data-ttu-id="03618-135">parametry `in` jsou povoleny kdekoli, kde jsou povoleny běžné parametry ByVal.</span><span class="sxs-lookup"><span data-stu-id="03618-135">`in` parameters are allowed anywhere where ordinary byval parameters are allowed.</span></span> <span data-ttu-id="03618-136">Patří sem indexery, operátory (včetně převodů), delegáti, výrazy lambda, místní funkce.</span><span class="sxs-lookup"><span data-stu-id="03618-136">This includes indexers, operators (including conversions), delegates, lambdas, local functions.</span></span>

> ```csharp
>  (in int x) => x                                                     // lambda expression  
>  TValue this[in TKey index];                                         // indexer
>  public static Vector3 operator +(in Vector3 x, in Vector3 y) => ... // operator
>  ```

- <span data-ttu-id="03618-137">`in` není povolený v kombinaci s `out` nebo s cokoli, na které `out` nekombinuje.</span><span class="sxs-lookup"><span data-stu-id="03618-137">`in` is not allowed in combination with `out` or with anything that `out` does not combine with.</span></span>

- <span data-ttu-id="03618-138">Nepovoluje se přetížení na `ref`/`out`/ch `in`ch rozdílech.</span><span class="sxs-lookup"><span data-stu-id="03618-138">It is not permitted to overload on `ref`/`out`/`in` differences.</span></span>

- <span data-ttu-id="03618-139">Je povoleno přetížit na běžných a `in`ch rozdílech.</span><span class="sxs-lookup"><span data-stu-id="03618-139">It is permitted to overload on ordinary byval and `in` differences.</span></span>

- <span data-ttu-id="03618-140">Pro účely OHI (přetížení, skrytí, implementace) se `in` chová podobně jako `out` parametr.</span><span class="sxs-lookup"><span data-stu-id="03618-140">For the purpose of OHI (Overloading, Hiding, Implementing), `in` behaves similarly to an `out` parameter.</span></span>
<span data-ttu-id="03618-141">Platí všechna stejná pravidla.</span><span class="sxs-lookup"><span data-stu-id="03618-141">All the same rules apply.</span></span>
<span data-ttu-id="03618-142">Například přepsání metody bude muset odpovídat parametrům `in` s parametry `in` typu, který je převoditelné pomocí identity.</span><span class="sxs-lookup"><span data-stu-id="03618-142">For example the overriding method will have to match `in` parameters with `in` parameters of an identity-convertible type.</span></span>

- <span data-ttu-id="03618-143">Pro účely převodů skupin Delegate/lambda/Method se `in` chová podobně jako `out` parametr.</span><span class="sxs-lookup"><span data-stu-id="03618-143">For the purpose of delegate/lambda/method group conversions, `in` behaves similarly to an `out` parameter.</span></span>
<span data-ttu-id="03618-144">Výrazy lambda a použitelné převody skupin metod budou muset odpovídat `in` parametrům cílového delegáta s `in` parametry typu, který se dá převést na identitu.</span><span class="sxs-lookup"><span data-stu-id="03618-144">Lambdas and applicable method group conversion candidates will have to match `in` parameters of the target delegate with `in` parameters of an identity-convertible type.</span></span>

- <span data-ttu-id="03618-145">Pro účely obecné odchylky jsou parametry `in` nevariantní.</span><span class="sxs-lookup"><span data-stu-id="03618-145">For the purpose of generic variance, `in` parameters are nonvariant.</span></span>

> <span data-ttu-id="03618-146">Poznámka: nejsou k dispozici žádná upozornění na parametry `in`, které mají typ odkazu nebo primitivní typy.</span><span class="sxs-lookup"><span data-stu-id="03618-146">NOTE: There are no warnings on `in` parameters that have reference or primitives types.</span></span>
<span data-ttu-id="03618-147">Může být obecně bezúčelné, ale v některých případech musí uživatel nebo chtít předat primitivní prvky jako `in`.</span><span class="sxs-lookup"><span data-stu-id="03618-147">It may be pointless in general, but in some cases user must/want to pass primitives as `in`.</span></span> <span data-ttu-id="03618-148">Příklady – přepsání obecné metody, jako je například `Method(in T param)`, když `T` nahradila `int`nebo když mají metody jako `Volatile.Read(in int location)`</span><span class="sxs-lookup"><span data-stu-id="03618-148">Examples - overriding a generic method like `Method(in T param)` when `T` was substituted to be `int`, or when having methods like `Volatile.Read(in int location)`</span></span>
>
> <span data-ttu-id="03618-149">Je možné mít analyzátor, který upozorňuje v případech neefektivního použití parametrů `in`, ale pravidla pro takovou analýzu by byla příliš Přibližná jako součást specifikace jazyka.</span><span class="sxs-lookup"><span data-stu-id="03618-149">It is conceivable to have an analyzer that warns in cases of inefficient use of `in` parameters, but the rules for such analysis would be too fuzzy to be a part of a language specification.</span></span>

### <a name="use-of-in-at-call-sites-in-arguments"></a><span data-ttu-id="03618-150">Použití `in` na webech volání.</span><span class="sxs-lookup"><span data-stu-id="03618-150">Use of `in` at call sites.</span></span> <span data-ttu-id="03618-151">(`in` argumenty)</span><span class="sxs-lookup"><span data-stu-id="03618-151">(`in` arguments)</span></span>

<span data-ttu-id="03618-152">Existují dva způsoby, jak předat argumenty `in`m parametrům.</span><span class="sxs-lookup"><span data-stu-id="03618-152">There are two ways to pass arguments to `in` parameters.</span></span>

#### <a name="in-arguments-can-match-in-parameters"></a><span data-ttu-id="03618-153">argumenty `in` můžou odpovídat parametrům `in`:</span><span class="sxs-lookup"><span data-stu-id="03618-153">`in` arguments can match `in` parameters:</span></span>

<span data-ttu-id="03618-154">Argument s modifikátorem `in` na webu volání může odpovídat parametrům `in`.</span><span class="sxs-lookup"><span data-stu-id="03618-154">An argument with an `in` modifier at the call site can match `in` parameters.</span></span>

```csharp
int x = 1;

void M1<T>(in T x)
{
  // . . .
}

var x = M1(in x);  // in argument to a method

class D
{
    public string this[in Guid index];
}

D dictionary = . . . ;
var y = dictionary[in Guid.Empty]; // in argument to an indexer
```

- <span data-ttu-id="03618-155">Argument `in` musí být _čitelný_ LValue (\*).</span><span class="sxs-lookup"><span data-stu-id="03618-155">`in` argument must be a _readable_ LValue(\*).</span></span>
<span data-ttu-id="03618-156">Příklad: `M1(in 42)` je neplatný</span><span class="sxs-lookup"><span data-stu-id="03618-156">Example: `M1(in 42)` is invalid</span></span>

> <span data-ttu-id="03618-157">(\*) Pojem [l-hodnota/rvalue](https://en.wikipedia.org/wiki/Value_(computer_science)#lrvalue) se liší mezi jazyky.</span><span class="sxs-lookup"><span data-stu-id="03618-157">(\*) The notion of [LValue/RValue](https://en.wikipedia.org/wiki/Value_(computer_science)#lrvalue) vary between languages.</span></span>  
<span data-ttu-id="03618-158">V tomto případě je v důsledku hodnoty LValue I výraz, který představuje umístění, na které lze odkazovat přímo.</span><span class="sxs-lookup"><span data-stu-id="03618-158">Here, by LValue I mean an expression that represent a location that can be referred to directly.</span></span>
<span data-ttu-id="03618-159">A RValue označuje výraz, který poskytuje dočasný výsledek, který není trvale trvalý.</span><span class="sxs-lookup"><span data-stu-id="03618-159">And RValue means an expression that yields a temporary result which does not persist on its own.</span></span>  

- <span data-ttu-id="03618-160">Konkrétně je platný pro předání `readonly` polí, `in` parametrů nebo jiných formálně `readonly` proměnných jako argumentů `in`.</span><span class="sxs-lookup"><span data-stu-id="03618-160">In particular it is valid to pass `readonly` fields, `in` parameters or other formally `readonly` variables as `in` arguments.</span></span>
<span data-ttu-id="03618-161">Příklad: `dictionary[in Guid.Empty]` je právní.</span><span class="sxs-lookup"><span data-stu-id="03618-161">Example: `dictionary[in Guid.Empty]` is legal.</span></span> <span data-ttu-id="03618-162">`Guid.Empty` je statické pole jen pro čtení.</span><span class="sxs-lookup"><span data-stu-id="03618-162">`Guid.Empty` is a static readonly field.</span></span>

- <span data-ttu-id="03618-163">`in` argument musí mít typ parametru, který lze _převést_ na typ parametru.</span><span class="sxs-lookup"><span data-stu-id="03618-163">`in` argument must have type _identity-convertible_ to the type of the parameter.</span></span>
<span data-ttu-id="03618-164">Příklad: `M1<object>(in Guid.Empty)` je neplatný.</span><span class="sxs-lookup"><span data-stu-id="03618-164">Example: `M1<object>(in Guid.Empty)` is invalid.</span></span> <span data-ttu-id="03618-165">`Guid.Empty` není _převoditelné ze identity_ na `object`</span><span class="sxs-lookup"><span data-stu-id="03618-165">`Guid.Empty` is not _identity-convertible_ to `object`</span></span>

<span data-ttu-id="03618-166">Motivace výše uvedených pravidel je taková, že `in` argumenty zaručují _alias_ proměnné argumentu.</span><span class="sxs-lookup"><span data-stu-id="03618-166">The motivation for the above rules is that `in` arguments guarantee _aliasing_ of the argument variable.</span></span> <span data-ttu-id="03618-167">Volaný vždy obdrží přímý odkaz na stejné umístění, které je reprezentované argumentem.</span><span class="sxs-lookup"><span data-stu-id="03618-167">The callee always receives a direct reference to the same location as represented by the argument.</span></span>

- <span data-ttu-id="03618-168">v případech, kdy je `in` argumentů nutné přecházet z zásobníku z důvodu `await` výrazů, které jsou použity jako operandy stejného volání, je chování stejné jako u `out` a `ref`ch argumentů – Pokud proměnná nemůže být začínat způsobem transparentním způsobem, je hlášena chyba.</span><span class="sxs-lookup"><span data-stu-id="03618-168">in rare situations when `in` arguments must be stack-spilled due to `await` expressions used as operands of the same call, the behavior is the same as with `out` and `ref` arguments - if the variable cannot be spilled in referentially-transparent manner, an error is reported.</span></span>

<span data-ttu-id="03618-169">Příklady:</span><span class="sxs-lookup"><span data-stu-id="03618-169">Examples:</span></span>
1. <span data-ttu-id="03618-170">`M1(in staticField, await SomethingAsync())` je platný.</span><span class="sxs-lookup"><span data-stu-id="03618-170">`M1(in staticField, await SomethingAsync())`  is valid.</span></span>
<span data-ttu-id="03618-171">`staticField` je statické pole, ke kterému lze přistupovat více než jednou bez pozorovatelných vedlejších účinků.</span><span class="sxs-lookup"><span data-stu-id="03618-171">`staticField` is a static field which can be accessed more than once without observable side effects.</span></span> <span data-ttu-id="03618-172">Proto lze zadat jak pořadí vedlejších účinků, tak i požadavky na alias.</span><span class="sxs-lookup"><span data-stu-id="03618-172">Therefore both the order of side effects and aliasing requirements can be provided.</span></span>
2. <span data-ttu-id="03618-173">`M1(in RefReturningMethod(), await SomethingAsync())` vytvoří chybu.</span><span class="sxs-lookup"><span data-stu-id="03618-173">`M1(in RefReturningMethod(), await SomethingAsync())`  will produce an error.</span></span>
<span data-ttu-id="03618-174">`RefReturningMethod()` je metoda, kterou vrací `ref`.</span><span class="sxs-lookup"><span data-stu-id="03618-174">`RefReturningMethod()` is a `ref` returning method.</span></span> <span data-ttu-id="03618-175">Volání metody může mít pozorovatelící vedlejší účinky, proto musí být vyhodnocen před `SomethingAsync()` operandem.</span><span class="sxs-lookup"><span data-stu-id="03618-175">A method call may have observable side effects, therefore it must be evaluated before the `SomethingAsync()` operand.</span></span> <span data-ttu-id="03618-176">Výsledek vyvolání je však odkaz, který nemůže být zachován přes `await` bod pozastavení, který neumožňuje požadavek na přímý odkaz.</span><span class="sxs-lookup"><span data-stu-id="03618-176">However the result of the invocation is a reference that cannot be preserved across the `await` suspension point which make the direct reference requirement impossible.</span></span>

> <span data-ttu-id="03618-177">Poznámka: Chyby při zakrývání zásobníku se považují za omezení specifická pro konkrétní implementaci.</span><span class="sxs-lookup"><span data-stu-id="03618-177">NOTE: the stack spilling errors are considered to be implementation-specific limitations.</span></span> <span data-ttu-id="03618-178">Proto nemají vliv na rozlišení přetěžování ani odvození výrazu lambda.</span><span class="sxs-lookup"><span data-stu-id="03618-178">Therefore they do not have effect on overload resolution or lambda inference.</span></span>

#### <a name="ordinary-byval-arguments-can-match-in-parameters"></a><span data-ttu-id="03618-179">Běžné argumenty ByVal můžou odpovídat parametrům `in`:</span><span class="sxs-lookup"><span data-stu-id="03618-179">Ordinary byval arguments can match `in` parameters:</span></span>

<span data-ttu-id="03618-180">Parametry `in` nemohou odpovídat regulárním argumentům bez modifikátorů.</span><span class="sxs-lookup"><span data-stu-id="03618-180">Regular arguments without modifiers can match `in` parameters.</span></span> <span data-ttu-id="03618-181">V takovém případě mají argumenty stejné odlehčené omezení jako běžné argumenty ByVal.</span><span class="sxs-lookup"><span data-stu-id="03618-181">In such case the arguments have the same relaxed constraints as an ordinary byval arguments would have.</span></span>

<span data-ttu-id="03618-182">Motivací pro tento scénář je, že `in` parametry v rozhraních API mohou mít za následek nepohodlí pro uživatele, pokud argumenty nelze předat jako přímý odkaz-ex: literály, vypočítané nebo výsledky `await`-Ed nebo argumenty, které se mají považovat za konkrétnější typy.</span><span class="sxs-lookup"><span data-stu-id="03618-182">The motivation for this scenario is that `in` parameters in APIs may result in inconveniences for the user when arguments cannot be passed as a direct reference - ex: literals, computed or `await`-ed results or arguments that happen to have more specific types.</span></span>  
<span data-ttu-id="03618-183">Všechny tyto případy mají triviální řešení ukládání hodnoty argumentu do dočasného lokálního typu a předání tohoto místního jako argumentu `in`.</span><span class="sxs-lookup"><span data-stu-id="03618-183">All these cases have a trivial solution of storing the argument value in a temporary local of appropriate type and passing that local as an `in` argument.</span></span>  
<span data-ttu-id="03618-184">Aby nedocházelo k tomu, že tento často používaný kompilátor kódu, může v případě potřeby provádět stejnou transformaci, když `in` modifikátor není přítomen na webu volání.</span><span class="sxs-lookup"><span data-stu-id="03618-184">To reduce the need for such boilerplate code compiler can perform the same transformation, if needed, when `in` modifier is not present at the call site.</span></span>  

<span data-ttu-id="03618-185">V některých případech, jako jsou například vyvolání operátorů nebo metody rozšíření `in`, neexistuje žádná syntaktická možnost zadat `in` vůbec.</span><span class="sxs-lookup"><span data-stu-id="03618-185">In addition, in some cases, such as invocation of operators, or `in` extension methods, there is no syntactical way to specify `in` at all.</span></span> <span data-ttu-id="03618-186">Tato samostatně vyžaduje určení chování běžných argumentů ByVal, pokud odpovídají `in` parametrům.</span><span class="sxs-lookup"><span data-stu-id="03618-186">That alone requires specifying the behavior of ordinary byval arguments when they match `in` parameters.</span></span>

<span data-ttu-id="03618-187">Zejména jde o toto:</span><span class="sxs-lookup"><span data-stu-id="03618-187">In particular:</span></span>

- <span data-ttu-id="03618-188">předání rvalue je neplatné.</span><span class="sxs-lookup"><span data-stu-id="03618-188">it is valid to pass RValues.</span></span>
<span data-ttu-id="03618-189">V takovém případě se předává odkaz na dočasnou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="03618-189">A reference to a temporary is passed in such case.</span></span>
<span data-ttu-id="03618-190">Příklad:</span><span class="sxs-lookup"><span data-stu-id="03618-190">Example:</span></span>
```csharp
Print("hello");      // not an error.

void Print<T>(in T x)
{
  //. . .
}
```

- <span data-ttu-id="03618-191">implicitní převody jsou povoleny.</span><span class="sxs-lookup"><span data-stu-id="03618-191">implicit conversions are allowed.</span></span>

> <span data-ttu-id="03618-192">To je vlastně zvláštní případ předávání RValue.</span><span class="sxs-lookup"><span data-stu-id="03618-192">This is actually a special case of passing an RValue</span></span>  

<span data-ttu-id="03618-193">V takovém případě se předává odkaz na dočasný podíl převedený na hodnotu.</span><span class="sxs-lookup"><span data-stu-id="03618-193">A reference to a temporary holding converted value is passed in such case.</span></span>
<span data-ttu-id="03618-194">Příklad:</span><span class="sxs-lookup"><span data-stu-id="03618-194">Example:</span></span>
```csharp
Print<int>(Short.MaxValue)     // not an error.
```

- <span data-ttu-id="03618-195">v případě přijímače metody rozšíření `in` (na rozdíl od `ref` metod rozšíření) jsou povoleny rvalue nebo implicitní _převody tohoto argumentu_ .</span><span class="sxs-lookup"><span data-stu-id="03618-195">in a case of a receiver of an `in` extension method (as opposed to `ref` extension methods), RValues or implicit _this-argument-conversions_ are allowed.</span></span>
<span data-ttu-id="03618-196">V takovém případě se předává odkaz na dočasný podíl převedený na hodnotu.</span><span class="sxs-lookup"><span data-stu-id="03618-196">A reference to a temporary holding converted value is passed in such case.</span></span>
<span data-ttu-id="03618-197">Příklad:</span><span class="sxs-lookup"><span data-stu-id="03618-197">Example:</span></span>
```csharp
public static IEnumerable<T> Concat<T>(in this (IEnumerable<T>, IEnumerable<T>) arg)  => . . .;

("aa", "bb").Concat<char>()    // not an error.
```
<span data-ttu-id="03618-198">Další informace o `ref`/`in` metody rozšíření jsou k dispozici dále v tomto dokumentu.</span><span class="sxs-lookup"><span data-stu-id="03618-198">More information on `ref`/`in` extension methods is provided further in this document.</span></span>

- <span data-ttu-id="03618-199">vynechání argumentu z důvodu `await` operandů by v případě potřeby mohlo proložení "podle hodnoty".</span><span class="sxs-lookup"><span data-stu-id="03618-199">argument spilling due to `await` operands could spill "by-value", if necessary.</span></span>
<span data-ttu-id="03618-200">Ve scénářích, kde není možné zadat přímý odkaz na argument, není možné, protože se jedná o `await` kopie hodnoty argumentu je mimo místo.</span><span class="sxs-lookup"><span data-stu-id="03618-200">In scenarios where providing a direct reference to the argument is not possible due to intervening `await` a copy of the argument's value is spilled instead.</span></span>  
<span data-ttu-id="03618-201">Příklad:</span><span class="sxs-lookup"><span data-stu-id="03618-201">Example:</span></span>
```csharp
M1(RefReturningMethod(), await SomethingAsync())   // not an error.
```
<span data-ttu-id="03618-202">Vzhledem k tomu, že výsledek vedlejšího vyvolání je odkaz, který nelze zachovat v rámci `await`ho pozastavení, bude místo toho uložen dočasný obsah obsahující skutečnou hodnotu (jako by to byl běžný případ parametrů ByVal).</span><span class="sxs-lookup"><span data-stu-id="03618-202">Since the result of a side-effecting invocation is a reference that cannot be preserved across `await` suspension, a temporary containing the actual value will be preserved instead (as it would in an ordinary byval parameter case).</span></span>

#### <a name="omitted-optional-arguments"></a><span data-ttu-id="03618-203">Vynechané nepovinné argumenty</span><span class="sxs-lookup"><span data-stu-id="03618-203">Omitted optional arguments</span></span>

<span data-ttu-id="03618-204">Je povoleno, aby parametr `in` určil výchozí hodnotu.</span><span class="sxs-lookup"><span data-stu-id="03618-204">It is permitted for an `in` parameter to specify a default value.</span></span> <span data-ttu-id="03618-205">Který nastaví odpovídající argument jako volitelný.</span><span class="sxs-lookup"><span data-stu-id="03618-205">That makes the corresponding argument optional.</span></span>

<span data-ttu-id="03618-206">Vynechání volitelného argumentu na webu volání má za následek předání výchozí hodnoty přes dočasné.</span><span class="sxs-lookup"><span data-stu-id="03618-206">Omitting optional argument at the call site results in passing the default value via a temporary.</span></span>

```csharp
Print("hello");      // not an error, same as
Print("hello", c: Color.Black);

void Print(string s, in Color c = Color.Black)
{
    // . . .
}
```

### <a name="aliasing-behavior-in-general"></a><span data-ttu-id="03618-207">Obecné chování při vytváření aliasů</span><span class="sxs-lookup"><span data-stu-id="03618-207">Aliasing behavior in general</span></span>

<span data-ttu-id="03618-208">Stejně jako proměnné `ref` a `out`, `in` proměnné jsou odkazy nebo aliasy na existující umístění.</span><span class="sxs-lookup"><span data-stu-id="03618-208">Just like `ref` and `out` variables, `in` variables are references/aliases to existing locations.</span></span>

<span data-ttu-id="03618-209">I když volaný není povolen k zápisu do nich, čtení `in` parametr může sledovat různé hodnoty jako vedlejší účinek jiných hodnocení.</span><span class="sxs-lookup"><span data-stu-id="03618-209">While callee is not allowed to write into them, reading an `in` parameter can observe different values as a side effect of other evaluations.</span></span>

<span data-ttu-id="03618-210">Příklad:</span><span class="sxs-lookup"><span data-stu-id="03618-210">Example:</span></span>

```C#
static Vector3 v = Vector3.UnitY;

static void Main()
{
    Test(v);
}

static void Test(in Vector3 v1)
{
    Debug.Assert(v1 == Vector3.UnitY);
    // changes v1 deterministically (no races required)
    ChangeV();
    Debug.Assert(v1 == Vector3.UnitX);
}

static void ChangeV()
{
    v = Vector3.UnitX;
}
```

### <a name="in-parameters-and-capturing-of-local-variables"></a><span data-ttu-id="03618-211">`in` parametry a zachytávání místních proměnných.</span><span class="sxs-lookup"><span data-stu-id="03618-211">`in` parameters and capturing of local variables.</span></span>  
<span data-ttu-id="03618-212">Pro účely zachycení lambda nebo asynchronního zachytávání `in` parametry se chovají stejně jako parametry `out` a `ref`.</span><span class="sxs-lookup"><span data-stu-id="03618-212">For the purpose of lambda/async capturing `in` parameters behave the same as `out` and `ref` parameters.</span></span>

- <span data-ttu-id="03618-213">parametry `in` se nedají zachytit v uzávěrce.</span><span class="sxs-lookup"><span data-stu-id="03618-213">`in` parameters cannot be captured in a closure</span></span>
- <span data-ttu-id="03618-214">parametry `in` nejsou povolené v metodách iterátoru.</span><span class="sxs-lookup"><span data-stu-id="03618-214">`in` parameters are not allowed in iterator methods</span></span>
- <span data-ttu-id="03618-215">parametry `in` nejsou povolené v asynchronních metodách.</span><span class="sxs-lookup"><span data-stu-id="03618-215">`in` parameters are not allowed in async methods</span></span>

### <a name="temporary-variables"></a><span data-ttu-id="03618-216">Dočasné proměnné.</span><span class="sxs-lookup"><span data-stu-id="03618-216">Temporary variables.</span></span>  
<span data-ttu-id="03618-217">Některé použití parametrů `in` předávání může vyžadovat nepřímé použití dočasné místní proměnné:</span><span class="sxs-lookup"><span data-stu-id="03618-217">Some uses of `in` parameter passing may require indirect use of a temporary local variable:</span></span>  
- <span data-ttu-id="03618-218">argumenty `in` jsou vždy předány jako přímé aliasy, když se používá web Call `in`.</span><span class="sxs-lookup"><span data-stu-id="03618-218">`in` arguments are always passed as direct aliases when call-site uses `in`.</span></span> <span data-ttu-id="03618-219">V takovém případě se v takovém případě nikdy nepoužívá.</span><span class="sxs-lookup"><span data-stu-id="03618-219">Temporary is never used in such case.</span></span>
- <span data-ttu-id="03618-220">argumenty `in` nejsou vyžadovány pro přímé aliasy, pokud volání-web nepoužívá `in`.</span><span class="sxs-lookup"><span data-stu-id="03618-220">`in` arguments are not required to be direct aliases when call-site does not use `in`.</span></span> <span data-ttu-id="03618-221">Pokud argument není l-hodnota, lze použít dočasné.</span><span class="sxs-lookup"><span data-stu-id="03618-221">When argument is not an LValue, a temporary may be used.</span></span>
- <span data-ttu-id="03618-222">parametr `in` může mít výchozí hodnotu.</span><span class="sxs-lookup"><span data-stu-id="03618-222">`in` parameter may have default value.</span></span> <span data-ttu-id="03618-223">Pokud je odpovídající argument vynechán na webu volání, je výchozí hodnota předána prostřednictvím dočasné.</span><span class="sxs-lookup"><span data-stu-id="03618-223">When corresponding argument is omitted at the call site, the default value are passed via a temporary.</span></span>
- <span data-ttu-id="03618-224">argumenty `in` můžou mít implicitní převody, včetně těch, které nezachovávají identitu.</span><span class="sxs-lookup"><span data-stu-id="03618-224">`in` arguments may have implicit conversions, including those that do not preserve identity.</span></span> <span data-ttu-id="03618-225">V těchto případech se používá dočasný.</span><span class="sxs-lookup"><span data-stu-id="03618-225">A temporary is used in those cases.</span></span>
- <span data-ttu-id="03618-226">přijímače běžných volání struktur nemůžou být zapisovatelné hodnoty lvalue (**existující případ!** ).</span><span class="sxs-lookup"><span data-stu-id="03618-226">receivers of ordinary struct calls may not be writeable LValues (**existing case!**).</span></span> <span data-ttu-id="03618-227">V těchto případech se používá dočasný.</span><span class="sxs-lookup"><span data-stu-id="03618-227">A temporary is used in those cases.</span></span>

<span data-ttu-id="03618-228">Doba života argumentu dočasné objekty odpovídá nejbližšímu rozsahu, který zahrnuje rozsah volání – Web.</span><span class="sxs-lookup"><span data-stu-id="03618-228">The life time of the argument temporaries matches the closest encompassing scope of the call-site.</span></span>

<span data-ttu-id="03618-229">Formální doba života dočasných proměnných je sémanticky významná ve scénářích, které se týkají řídicích analýz proměnných vrácených odkazem.</span><span class="sxs-lookup"><span data-stu-id="03618-229">The formal life time of temporary variables is semantically significant in scenarios involving escape analysis of variables returned by reference.</span></span>

### <a name="metadata-representation-of-in-parameters"></a><span data-ttu-id="03618-230">Reprezentace metadat `in` parametrů</span><span class="sxs-lookup"><span data-stu-id="03618-230">Metadata representation of `in` parameters.</span></span>
<span data-ttu-id="03618-231">Při použití `System.Runtime.CompilerServices.IsReadOnlyAttribute` pro parametr ByRef to znamená, že parametr je `in` parametr.</span><span class="sxs-lookup"><span data-stu-id="03618-231">When `System.Runtime.CompilerServices.IsReadOnlyAttribute` is applied to a byref parameter, it means that the parameter is an `in` parameter.</span></span>

<span data-ttu-id="03618-232">Kromě toho, pokud je metoda *abstraktní* nebo *virtuální*, musí mít signatura takových parametrů (a pouze takové parametry) `modreq[System.Runtime.InteropServices.InAttribute]`.</span><span class="sxs-lookup"><span data-stu-id="03618-232">In addition, if the method is *abstract* or *virtual*, then the signature of such parameters (and only such parameters) must have `modreq[System.Runtime.InteropServices.InAttribute]`.</span></span>

<span data-ttu-id="03618-233">**Motivace**: Tato akce zajistí, že v případě metody přepisu nebo implementace parametrů `in` se shodují.</span><span class="sxs-lookup"><span data-stu-id="03618-233">**Motivation**: this is done to ensure that in a case of method overriding/implementing the `in` parameters match.</span></span>

<span data-ttu-id="03618-234">Stejné požadavky platí pro `Invoke` metody v delegátech.</span><span class="sxs-lookup"><span data-stu-id="03618-234">Same requirements apply to `Invoke` methods in delegates.</span></span>

<span data-ttu-id="03618-235">**Motivace**: je potřeba zajistit, aby existující kompilátory při vytváření nebo přiřazování delegátů nemohly jednoduše ignorovat `readonly`.</span><span class="sxs-lookup"><span data-stu-id="03618-235">**Motivation**: this is to ensure that existing compilers cannot simply ignore `readonly` when creating or assigning delegates.</span></span>

## <a name="returning-by-readonly-reference"></a><span data-ttu-id="03618-236">Vrací se odkazem jen pro čtení.</span><span class="sxs-lookup"><span data-stu-id="03618-236">Returning by readonly reference.</span></span>

### <a name="motivation"></a><span data-ttu-id="03618-237">Motivační</span><span class="sxs-lookup"><span data-stu-id="03618-237">Motivation</span></span>
<span data-ttu-id="03618-238">Motivace této dílčí funkce je přibližně symetricky k důvodům pro parametry `in` – zamezení kopírování, ale na návratové straně.</span><span class="sxs-lookup"><span data-stu-id="03618-238">The motivation for this sub-feature is roughly symmetrical to the reasons for the `in` parameters - avoiding copying, but on the returning side.</span></span> <span data-ttu-id="03618-239">Před touto funkcí byly v metodě nebo indexeru dvě možnosti: 1) vrácení odkazem a zpřístupnění možných mutací nebo 2) vrátí hodnotu, která má za následek kopírování.</span><span class="sxs-lookup"><span data-stu-id="03618-239">Prior to this feature, a method or an indexer had two options: 1) return by reference and be exposed to possible mutations or 2) return by value which results in copying.</span></span>

### <a name="solution-ref-readonly-returns"></a><span data-ttu-id="03618-240">Řešení (`ref readonly` vrátí)</span><span class="sxs-lookup"><span data-stu-id="03618-240">Solution (`ref readonly` returns)</span></span>  
<span data-ttu-id="03618-241">Funkce umožňuje členovi vracet proměnné podle odkazu bez jejich vystavení pro mutace.</span><span class="sxs-lookup"><span data-stu-id="03618-241">The feature allows a member to return variables by reference without exposing them to mutations.</span></span>

### <a name="declaring-ref-readonly-returning-members"></a><span data-ttu-id="03618-242">Deklarace `ref readonly` vracející členy</span><span class="sxs-lookup"><span data-stu-id="03618-242">Declaring `ref readonly` returning members</span></span>

<span data-ttu-id="03618-243">Kombinace modifikátorů `ref readonly` pro návratový podpis slouží k označení toho, že člen vrací odkaz jen pro čtení.</span><span class="sxs-lookup"><span data-stu-id="03618-243">A combination of modifiers `ref readonly` on the return signature is used to to indicate that the member returns a readonly reference.</span></span>

<span data-ttu-id="03618-244">Pro všechny účely se `ref readonly` člen považuje za `readonly` proměnnou, podobně jako `readonly` pole a `in` parametry.</span><span class="sxs-lookup"><span data-stu-id="03618-244">For all purposes a `ref readonly` member is treated as a `readonly` variable - similar to `readonly` fields and `in` parameters.</span></span>

<span data-ttu-id="03618-245">Například pole člena `ref readonly`, který má typ struktury, jsou rekurzivně klasifikována jako `readonly` proměnné.</span><span class="sxs-lookup"><span data-stu-id="03618-245">For example fields of `ref readonly` member which has a struct type are all recursively classified as `readonly` variables.</span></span> <span data-ttu-id="03618-246">– Je povoleno je předat jako argumenty `in`, ale ne jako argumenty `ref` nebo `out`.</span><span class="sxs-lookup"><span data-stu-id="03618-246">- It is permitted to pass them as `in` arguments, but not as `ref` or `out` arguments.</span></span>

```csharp
ref readonly Guid Method1()
{
}

Method2(in Method1()); // valid. Can pass as `in` argument.

Method3(ref Method1()); // not valid. Cannot pass as `ref` argument
```

- <span data-ttu-id="03618-247">na stejných místech jsou povolené `ref readonly` vrácení `ref` jsou povolené.</span><span class="sxs-lookup"><span data-stu-id="03618-247">`ref readonly` returns are allowed in the same places were `ref` returns are allowed.</span></span>
<span data-ttu-id="03618-248">Patří sem indexery, delegáti, výrazy lambda a místní funkce.</span><span class="sxs-lookup"><span data-stu-id="03618-248">This includes indexers, delegates, lambdas, local functions.</span></span>

- <span data-ttu-id="03618-249">Není povoleno přetížení v `ref`/`ref readonly`/rozdíly.</span><span class="sxs-lookup"><span data-stu-id="03618-249">It is not permitted to overload on `ref`/`ref readonly` /  differences.</span></span>

- <span data-ttu-id="03618-250">Je povoleno přetížení na běžných ByVal a `ref readonly` vrácení rozdílů.</span><span class="sxs-lookup"><span data-stu-id="03618-250">It is permitted to overload on ordinary byval and `ref readonly` return differences.</span></span>

- <span data-ttu-id="03618-251">Pro účely OHI (přetížení, skrytí, implementace) `ref readonly` je podobná, ale liší se od `ref`.</span><span class="sxs-lookup"><span data-stu-id="03618-251">For the purpose of OHI (Overloading, Hiding, Implementing), `ref readonly` is similar but distinct from `ref`.</span></span>
<span data-ttu-id="03618-252">Například metoda a, která přepisuje `ref readonly` jeden, musí být `ref readonly` a musí být typu, který je převoditelný identitou.</span><span class="sxs-lookup"><span data-stu-id="03618-252">For example the a method that overrides `ref readonly` one, must itself be `ref readonly` and have identity-convertible type.</span></span>

- <span data-ttu-id="03618-253">Pro účely převodů skupin Delegate/lambda nebo method je `ref readonly` podobná, ale liší se od `ref`.</span><span class="sxs-lookup"><span data-stu-id="03618-253">For the purpose of delegate/lambda/method group conversions, `ref readonly` is similar but distinct from `ref`.</span></span>
<span data-ttu-id="03618-254">Výrazy lambda a příslušné metody převodu skupiny metod musí odpovídat `ref readonly` vrácení cílového delegáta s `ref readonly` vrácení typu, který je převoditelnou identitou.</span><span class="sxs-lookup"><span data-stu-id="03618-254">Lambdas and applicable method group conversion candidates have to match `ref readonly` return of the target delegate with `ref readonly` return of the type that is identity-convertible.</span></span>

- <span data-ttu-id="03618-255">Pro účely obecné odchylky je `ref readonly` vrácení nevariantní.</span><span class="sxs-lookup"><span data-stu-id="03618-255">For the purpose of generic variance, `ref readonly` returns are nonvariant.</span></span>

> <span data-ttu-id="03618-256">Poznámka: k dispozici nejsou žádná upozornění na `ref readonly` vrátí, která mají typ odkazu nebo primitivní typy.</span><span class="sxs-lookup"><span data-stu-id="03618-256">NOTE: There are no warnings on `ref readonly` returns that have reference or primitives types.</span></span>
<span data-ttu-id="03618-257">Může být obecně bezúčelné, ale v některých případech musí uživatel nebo chtít předat primitivní prvky jako `in`.</span><span class="sxs-lookup"><span data-stu-id="03618-257">It may be pointless in general, but in some cases user must/want to pass primitives as `in`.</span></span> <span data-ttu-id="03618-258">Příklady – přepsání obecné metody, jako je `ref readonly T Method()`, když `T` nahradila `int`.</span><span class="sxs-lookup"><span data-stu-id="03618-258">Examples - overriding a generic method like `ref readonly T Method()` when `T` was substituted to be `int`.</span></span>
>
><span data-ttu-id="03618-259">Je možné mít analyzátor, který upozorňuje v případech neefektivního použití `ref readonly` vrátí, ale pravidla pro takovou analýzu by byla příliš Přibližná jako součást specifikace jazyka.</span><span class="sxs-lookup"><span data-stu-id="03618-259">It is conceivable to have an analyzer that warns in cases of inefficient use of `ref readonly` returns, but the rules for such analysis would be too fuzzy to be a part of a language specification.</span></span>

### <a name="returning-from-ref-readonly-members"></a><span data-ttu-id="03618-260">Vracení z `ref readonly`ch členů</span><span class="sxs-lookup"><span data-stu-id="03618-260">Returning from `ref readonly` members</span></span>
<span data-ttu-id="03618-261">V těle metody je syntaxe stejná jako s běžným návratovým argumentem.</span><span class="sxs-lookup"><span data-stu-id="03618-261">Inside the method body the syntax is the same as with regular ref returns.</span></span> <span data-ttu-id="03618-262">`readonly` bude odvozena z nadřazené metody.</span><span class="sxs-lookup"><span data-stu-id="03618-262">The `readonly` will be inferred from the containing method.</span></span>

<span data-ttu-id="03618-263">Motivace je taková, že `return ref readonly <expression>` není zbytečný a umožňuje pouze neshoda na `readonly` část, která by vždycky vedla k chybám.</span><span class="sxs-lookup"><span data-stu-id="03618-263">The motivation is that `return ref readonly <expression>` is unnecessary long and only allows for mismatches on the `readonly` part that would always result in errors.</span></span>
<span data-ttu-id="03618-264">`ref` se ale vyžaduje pro konzistenci s jinými scénáři, kdy je něco předáno pomocí striktního aliasu vs. podle hodnoty.</span><span class="sxs-lookup"><span data-stu-id="03618-264">The `ref` is, however, required for consistency with other scenarios where something is passed via strict aliasing vs. by value.</span></span>

> <span data-ttu-id="03618-265">Na rozdíl od případu s parametry `in` `ref readonly` vrátí nikdy návrat prostřednictvím místní kopie.</span><span class="sxs-lookup"><span data-stu-id="03618-265">Unlike the case with `in` parameters, `ref readonly` returns never return via a local copy.</span></span> <span data-ttu-id="03618-266">Vzhledem k tomu, že by se kopírování po vrácení takového postupu v případě, že by tento postup zastavilo, bezúčelné a nebezpečné.</span><span class="sxs-lookup"><span data-stu-id="03618-266">Considering that the copy would cease to exist immediately upon returning such practice would be pointless and dangerous.</span></span> <span data-ttu-id="03618-267">Proto `ref readonly` vrátí vždy přímé odkazy.</span><span class="sxs-lookup"><span data-stu-id="03618-267">Therefore `ref readonly` returns are always direct references.</span></span>

<span data-ttu-id="03618-268">Příklad:</span><span class="sxs-lookup"><span data-stu-id="03618-268">Example:</span></span>

```csharp
struct ImmutableArray<T>
{
    private readonly T[] array;

    public ref readonly T ItemRef(int i)
    {
        // returning a readonly reference to an array element
        return ref this.array[i];
    }
}

```

- <span data-ttu-id="03618-269">Argumentem `return ref` musí být l-hodnota (**stávající pravidlo**).</span><span class="sxs-lookup"><span data-stu-id="03618-269">An argument of `return ref` must be an LValue (**existing rule**)</span></span>
- <span data-ttu-id="03618-270">Argument `return ref` musí být "bezpečné pro návrat" (**stávající pravidlo**).</span><span class="sxs-lookup"><span data-stu-id="03618-270">An argument of `return ref` must be "safe to return" (**existing rule**)</span></span>
- <span data-ttu-id="03618-271">V členu `ref readonly` není _nutné zapisovat_ do argumentu `return ref`.</span><span class="sxs-lookup"><span data-stu-id="03618-271">In a `ref readonly` member an argument of `return ref` is _not required to be writeable_ .</span></span>
<span data-ttu-id="03618-272">Například tento člen může vracet REF pole jen pro čtení nebo jeden z jeho `in` parametrů.</span><span class="sxs-lookup"><span data-stu-id="03618-272">For example such member can ref-return a readonly field or one of its `in` parameters.</span></span>

### <a name="safe-to-return-rules"></a><span data-ttu-id="03618-273">V bezpečí můžete vracet pravidla.</span><span class="sxs-lookup"><span data-stu-id="03618-273">Safe to Return rules.</span></span>
<span data-ttu-id="03618-274">Normální zabezpečení pro návratová pravidla pro odkazy se vztahují také na odkazy jen pro čtení.</span><span class="sxs-lookup"><span data-stu-id="03618-274">Normal safe to return rules for references will apply to readonly references as well.</span></span>

<span data-ttu-id="03618-275">Všimněte si, že `ref readonly` lze získat z obyčejné `ref` lokální/parametr/Return, ale nikoli jiným způsobem.</span><span class="sxs-lookup"><span data-stu-id="03618-275">Note that a `ref readonly` can be obtained from a regular `ref` local/parameter/return, but not the other way around.</span></span> <span data-ttu-id="03618-276">V opačném případě je zabezpečení `ref readonly`ch vrácení odvozeno stejným způsobem jako u regulárních `ref`ch vrácených.</span><span class="sxs-lookup"><span data-stu-id="03618-276">Otherwise the safety of `ref readonly` returns is inferred the same way as for regular `ref` returns.</span></span>

<span data-ttu-id="03618-277">Vzhledem k tomu, že rvalue se dá předat jako parametr `in` a vrátí se jako `ref readonly` potřebujeme ještě jedno další pravidlo – **rvalue se nedá bezpečně vrátit odkazem**.</span><span class="sxs-lookup"><span data-stu-id="03618-277">Considering that RValues can be passed as `in` parameter and returned as `ref readonly` we need one more rule - **RValues are not safe-to-return by reference**.</span></span>

> <span data-ttu-id="03618-278">Zvažte situaci, kdy se RValue předává do parametru `in` prostřednictvím kopie a pak se vrátí zpět ve formě `ref readonly`.</span><span class="sxs-lookup"><span data-stu-id="03618-278">Consider the situation when an RValue is passed to an `in` parameter via a copy and then returned back in a form of a `ref readonly`.</span></span> <span data-ttu-id="03618-279">V kontextu volajícího je výsledkem takového vyvolání odkaz na místní data, protože to není bezpečné.</span><span class="sxs-lookup"><span data-stu-id="03618-279">In the context of the caller the result of such invocation is a reference to local data and as such is unsafe to return.</span></span>
> <span data-ttu-id="03618-280">Jakmile rvalue není bezpečné vracet, stávající pravidlo `#6` už tento případ zpracovává.</span><span class="sxs-lookup"><span data-stu-id="03618-280">Once RValues are not safe to return, the existing rule `#6` already handles this case.</span></span>

<span data-ttu-id="03618-281">Příklad:</span><span class="sxs-lookup"><span data-stu-id="03618-281">Example:</span></span>
```csharp
ref readonly Vector3 Test1()
{
    // can pass an RValue as "in" (via a temp copy)
    // but the result is not safe to return
    // because the RValue argument was not safe to return by reference
    return ref Test2(default(Vector3));
}

ref readonly Vector3 Test2(in Vector3 r)
{
    // this is ok, r is returnable
    return ref r;
}
```

<span data-ttu-id="03618-282">Aktualizované `safe to return` pravidla:</span><span class="sxs-lookup"><span data-stu-id="03618-282">Updated `safe to return` rules:</span></span>

1.  <span data-ttu-id="03618-283">**odkazy na proměnné na haldě se bezpečně vracejí**</span><span class="sxs-lookup"><span data-stu-id="03618-283">**refs to variables on the heap are safe to return**</span></span>
2.  <span data-ttu-id="03618-284">**parametry ref/in jsou bezpečné pro návrat**
`in` parametrů přirozeně se dají vracet jenom jako jen pro čtení.</span><span class="sxs-lookup"><span data-stu-id="03618-284">**ref/in parameters are safe to return**
`in` parameters naturally can only be returned as readonly.</span></span>
3.  <span data-ttu-id="03618-285">**výstupní parametry se dají bezpečně vrátit** (ale musí se jednoznačně přiřadit, protože už dnes platí).</span><span class="sxs-lookup"><span data-stu-id="03618-285">**out parameters are safe to return** (but must be definitely assigned, as is already the case today)</span></span>
4.  <span data-ttu-id="03618-286">**pole struktury instancí je bezpečné, aby se vracely, dokud se příjemce bezpečně vrátí.**</span><span class="sxs-lookup"><span data-stu-id="03618-286">**instance struct fields are safe to return as long as the receiver is safe to return**</span></span>
5.  <span data-ttu-id="03618-287">**možnost this není bezpečná pro návrat ze členů struktury.**</span><span class="sxs-lookup"><span data-stu-id="03618-287">**'this' is not safe to return from struct members**</span></span>
6.  <span data-ttu-id="03618-288">**odkaz, vrácený z jiné metody, se může bezpečně vrátit, pokud všechny odkazy/objekty předané této metodě jako formální parametry byly bezpečně vraceny.** 
*konkrétně není důležité, pokud se příjemce může bezpečně vrátit bez ohledu na to, jestli je příjemce strukturou, třídou nebo typem jako parametr obecného typu.*</span><span class="sxs-lookup"><span data-stu-id="03618-288">**a ref, returned from another method is safe to return if all refs/outs passed to that method as formal parameters were safe to return.**
*Specifically it is irrelevant if receiver is safe to return, regardless whether receiver is a struct, class or typed as a generic type parameter.*</span></span>
7.  <span data-ttu-id="03618-289">**Rvalue nelze bezpečně vrátit odkazem.** 
*konkrétně rvalue je bezpečné předat jako parametry.*</span><span class="sxs-lookup"><span data-stu-id="03618-289">**RValues are not safe to return by reference.**
*Specifically RValues are safe to pass as in parameters.*</span></span>

> <span data-ttu-id="03618-290">Poznámka: existují další pravidla týkající se bezpečnosti návratů, která přicházejí do hry, když jsou zapojeny typy s odkazem na typ ref a změna počtu odkazů.</span><span class="sxs-lookup"><span data-stu-id="03618-290">NOTE: There are additional rules regarding safety of returns that come into play when ref-like types and ref-reassignments are involved.</span></span>
> <span data-ttu-id="03618-291">Pravidla platí stejně pro `ref` a `ref readonly` členy, a proto zde nejsou zmíněny.</span><span class="sxs-lookup"><span data-stu-id="03618-291">The rules equally apply to `ref` and `ref readonly` members and therefore are not mentioned here.</span></span>

### <a name="aliasing-behavior"></a><span data-ttu-id="03618-292">Chování při vytváření aliasů.</span><span class="sxs-lookup"><span data-stu-id="03618-292">Aliasing behavior.</span></span>
<span data-ttu-id="03618-293">`ref readonly` členové poskytují stejné chování při vytváření aliasů jako běžné `ref` členy (s výjimkou nejenom pro čtení).</span><span class="sxs-lookup"><span data-stu-id="03618-293">`ref readonly` members provide the same aliasing behavior as ordinary `ref` members (except for being readonly).</span></span>
<span data-ttu-id="03618-294">Proto pro účely zachycení výrazů lambda, asynchronní, iterátory, překrývání zásobníku atd... platí stejná omezení.</span><span class="sxs-lookup"><span data-stu-id="03618-294">Therefore for the purpose of capturing in lambdas, async, iterators, stack spilling etc... the same restrictions apply.</span></span> <span data-ttu-id="03618-295">t.</span><span class="sxs-lookup"><span data-stu-id="03618-295">- I.E.</span></span> <span data-ttu-id="03618-296">z důvodu neschopnosti zachytit skutečné odkazy a z důvodu vedlejších účinků pro vyhodnocení členů takové scénáře nejsou povoleny.</span><span class="sxs-lookup"><span data-stu-id="03618-296">due to inability to capture the actual references and due to side-effecting nature of member evaluation such scenarios are disallowed.</span></span>

> <span data-ttu-id="03618-297">Je povolen a vyžadován pro vytvoření kopie, když `ref readonly` návrat je přijímačem běžných metod struktury, které `this` jako standardní zapisovatelný odkaz.</span><span class="sxs-lookup"><span data-stu-id="03618-297">It is permitted and required to make a copy when `ref readonly` return is a receiver of regular struct methods, which take `this` as an ordinary writeable reference.</span></span> <span data-ttu-id="03618-298">Historicky ve všech případech, kde jsou tato volání použita pro proměnnou jen pro čtení, je vytvořena místní kopie.</span><span class="sxs-lookup"><span data-stu-id="03618-298">Historically in all cases where such invocations are applied to readonly variable a local copy is made.</span></span>

### <a name="metadata-representation"></a><span data-ttu-id="03618-299">Reprezentace metadat</span><span class="sxs-lookup"><span data-stu-id="03618-299">Metadata representation.</span></span>
<span data-ttu-id="03618-300">Pokud je použita `System.Runtime.CompilerServices.IsReadOnlyAttribute` na vrácení návratové metody ByRef, znamená to, že metoda vrátí odkaz jen pro čtení.</span><span class="sxs-lookup"><span data-stu-id="03618-300">When `System.Runtime.CompilerServices.IsReadOnlyAttribute` is applied to the return of a byref returning method, it means that the method returns a readonly reference.</span></span>

<span data-ttu-id="03618-301">Kromě toho musí mít signatura výsledku takových metod (a pouze tyto metody) `modreq[System.Runtime.CompilerServices.IsReadOnlyAttribute]`.</span><span class="sxs-lookup"><span data-stu-id="03618-301">In addition, the result signature of such methods (and only those methods) must have `modreq[System.Runtime.CompilerServices.IsReadOnlyAttribute]`.</span></span>

<span data-ttu-id="03618-302">**Motivace**: je potřeba zajistit, aby existující kompilátory nemohly ignorovat `readonly` při volání metod s `ref readonly`.</span><span class="sxs-lookup"><span data-stu-id="03618-302">**Motivation**: this is to ensure that existing compilers cannot simply ignore `readonly` when invoking methods with `ref readonly` returns</span></span>

## <a name="readonly-structs"></a><span data-ttu-id="03618-303">Struktury jen pro čtení</span><span class="sxs-lookup"><span data-stu-id="03618-303">Readonly structs</span></span>
<span data-ttu-id="03618-304">Stručná funkce, která vytváří `this` parametr všech členů instance struktury, s výjimkou konstruktorů, `in` parametr.</span><span class="sxs-lookup"><span data-stu-id="03618-304">In short - a feature that makes `this` parameter of all instance members of a struct, except for constructors, an `in` parameter.</span></span>

### <a name="motivation"></a><span data-ttu-id="03618-305">Motivační</span><span class="sxs-lookup"><span data-stu-id="03618-305">Motivation</span></span>
<span data-ttu-id="03618-306">Kompilátor musí předpokládat, že jakékoli volání metody v instanci struktury může změnit instanci.</span><span class="sxs-lookup"><span data-stu-id="03618-306">Compiler must assume that any method call on a struct instance may modify the instance.</span></span> <span data-ttu-id="03618-307">Do metody jako `this` parametr je ve skutečnosti předán zapisovatelný odkaz, který toto chování plně povoluje.</span><span class="sxs-lookup"><span data-stu-id="03618-307">Indeed a writeable reference is passed to the method as `this` parameter and fully enables this behavior.</span></span> <span data-ttu-id="03618-308">Chcete-li takové vyvolání pro proměnné `readonly` použít, jsou volání použita pro dočasné kopie.</span><span class="sxs-lookup"><span data-stu-id="03618-308">To allow such invocations on `readonly` variables, the invocations are applied to temp copies.</span></span> <span data-ttu-id="03618-309">To může být neintuitivní a někdy nutí uživatele opustit `readonly` z důvodů výkonu.</span><span class="sxs-lookup"><span data-stu-id="03618-309">That could be unintuitive and sometimes forces people to abandon `readonly` for performance reasons.</span></span>  
<span data-ttu-id="03618-310">Příklad: https://codeblog.jonskeet.uk/2014/07/16/micro-optimization-the-surprising-inefficiency-of-readonly-fields/</span><span class="sxs-lookup"><span data-stu-id="03618-310">Example: https://codeblog.jonskeet.uk/2014/07/16/micro-optimization-the-surprising-inefficiency-of-readonly-fields/</span></span>

<span data-ttu-id="03618-311">Po přidání podpory pro parametry `in` a `ref readonly` vrátí problém s kopírováním obrannou linií, protože proměnné jen pro čtení se budou běžnější.</span><span class="sxs-lookup"><span data-stu-id="03618-311">After adding support for `in` parameters and `ref readonly` returns the problem of defensive copying will get worse since readonly variables will become more common.</span></span>

### <a name="solution"></a><span data-ttu-id="03618-312">Řešení</span><span class="sxs-lookup"><span data-stu-id="03618-312">Solution</span></span>
<span data-ttu-id="03618-313">Povolí modifikátor `readonly` v deklaracích struktury, což by vedlo k tomu, že `this` zpracováván jako `in` parametr u všech metod instance struktury s výjimkou konstruktorů.</span><span class="sxs-lookup"><span data-stu-id="03618-313">Allow `readonly` modifier on struct declarations which would result in `this` being treated as `in` parameter on all struct instance methods except for constructors.</span></span>

```csharp
static void Test(in Vector3 v1)
{
    // no need to make a copy of v1 since Vector3 is a readonly struct
    System.Console.WriteLine(v1.ToString());
}

readonly struct Vector3
{
    . . .

    public override string ToString()
    {
        // not OK!!  `this` is an `in` parameter
        foo(ref this.X);

        // OK
        return $"X: {X}, Y: {Y}, Z: {Z}";
    }
}
```

### <a name="restrictions-on-members-of-readonly-struct"></a><span data-ttu-id="03618-314">Omezení členů struktury jen pro čtení</span><span class="sxs-lookup"><span data-stu-id="03618-314">Restrictions on members of readonly struct</span></span>
- <span data-ttu-id="03618-315">Pole instance struktury jen pro čtení musí být jen pro čtení.</span><span class="sxs-lookup"><span data-stu-id="03618-315">Instance fields of a readonly struct must be readonly.</span></span>  
<span data-ttu-id="03618-316">**Motivace:** lze zapisovat pouze externě, ale ne prostřednictvím členů.</span><span class="sxs-lookup"><span data-stu-id="03618-316">**Motivation:** can only be written to externally, but not through members.</span></span>
- <span data-ttu-id="03618-317">Instance autoproperties struktury jen pro čtení musí být jen pro získání.</span><span class="sxs-lookup"><span data-stu-id="03618-317">Instance autoproperties of a readonly struct must be get-only.</span></span>  
<span data-ttu-id="03618-318">**Motivace:** důsledek omezení pro pole instance.</span><span class="sxs-lookup"><span data-stu-id="03618-318">**Motivation:** consequence of restriction on instance fields.</span></span>
- <span data-ttu-id="03618-319">Struktura ReadOnly nemůže deklarovat události podobné poli.</span><span class="sxs-lookup"><span data-stu-id="03618-319">Readonly struct may not declare field-like events.</span></span>  
<span data-ttu-id="03618-320">**Motivace:** důsledek omezení pro pole instance.</span><span class="sxs-lookup"><span data-stu-id="03618-320">**Motivation:** consequence of restriction on instance fields.</span></span>

### <a name="metadata-representation"></a><span data-ttu-id="03618-321">Reprezentace metadat</span><span class="sxs-lookup"><span data-stu-id="03618-321">Metadata representation.</span></span>
<span data-ttu-id="03618-322">Pokud je použita `System.Runtime.CompilerServices.IsReadOnlyAttribute` pro typ hodnoty, znamená to, že typ je `readonly struct`.</span><span class="sxs-lookup"><span data-stu-id="03618-322">When `System.Runtime.CompilerServices.IsReadOnlyAttribute` is applied to a value type, it means that the type is a `readonly struct`.</span></span>

<span data-ttu-id="03618-323">Zejména jde o toto:</span><span class="sxs-lookup"><span data-stu-id="03618-323">In particular:</span></span>
-  <span data-ttu-id="03618-324">Identita typu `IsReadOnlyAttribute` je neimportovaná.</span><span class="sxs-lookup"><span data-stu-id="03618-324">The identity of the `IsReadOnlyAttribute` type is unimportant.</span></span> <span data-ttu-id="03618-325">Ve skutečnosti může být vložen kompilátorem v nadřazeném sestavení v případě potřeby.</span><span class="sxs-lookup"><span data-stu-id="03618-325">In fact it can be embedded by the compiler in the containing assembly if needed.</span></span>

## <a name="refin-extension-methods"></a><span data-ttu-id="03618-326">metody rozšíření `ref`/`in`</span><span class="sxs-lookup"><span data-stu-id="03618-326">`ref`/`in` extension methods</span></span>
<span data-ttu-id="03618-327">Ve skutečnosti existuje stávající návrh (https://github.com/dotnet/roslyn/issues/165) a odpovídající prototyp žádosti o přijetí změn (https://github.com/dotnet/roslyn/pull/15650).</span><span class="sxs-lookup"><span data-stu-id="03618-327">There is actually an existing proposal (https://github.com/dotnet/roslyn/issues/165) and corresponding prototype PR (https://github.com/dotnet/roslyn/pull/15650).</span></span>
<span data-ttu-id="03618-328">Chci jenom potvrdit, že tento nápad není zcela nový.</span><span class="sxs-lookup"><span data-stu-id="03618-328">I just want to acknowledge that this idea is not entirely new.</span></span> <span data-ttu-id="03618-329">Je však zde relevantní, protože `ref readonly` elegantním způsobem odebírá contentious problém těchto metod – co dělat s přijímači RValue.</span><span class="sxs-lookup"><span data-stu-id="03618-329">It is, however, relevant here since `ref readonly` elegantly removes the most contentious issue about such methods - what to do with RValue receivers.</span></span>

<span data-ttu-id="03618-330">Obecný nápad umožňuje rozšiřujícím metodám přebírat parametr `this` odkazem, pokud je známý jako typ struktury.</span><span class="sxs-lookup"><span data-stu-id="03618-330">The general idea is allowing extension methods to take the `this` parameter by reference, as long as the type is known to be a struct type.</span></span>

```csharp
public static void Extension(ref this Guid self)
{
    // do something
}
```

<span data-ttu-id="03618-331">Důvody pro zápis takových rozšiřujících metod jsou primárně:</span><span class="sxs-lookup"><span data-stu-id="03618-331">The reasons for writing such extension methods are primarily:</span></span>  
1.  <span data-ttu-id="03618-332">Vyhněte se kopírováním, když je přijímač velkou strukturou</span><span class="sxs-lookup"><span data-stu-id="03618-332">Avoid copying when receiver is a large struct</span></span>
2.  <span data-ttu-id="03618-333">Povolení metod rozšíření pro struktury</span><span class="sxs-lookup"><span data-stu-id="03618-333">Allow mutating extension methods on structs</span></span>

<span data-ttu-id="03618-334">Důvody, proč nechci tuto třídu u tříd povolit</span><span class="sxs-lookup"><span data-stu-id="03618-334">The reasons why we do not want to allow this on classes</span></span>  
1.  <span data-ttu-id="03618-335">Má velmi omezený účel.</span><span class="sxs-lookup"><span data-stu-id="03618-335">It would be of very limited purpose.</span></span>
2.  <span data-ttu-id="03618-336">Došlo by k přerušení dlouhého stálého nevarianty, kterou volání metody nemůže zapnout, aby se ne`null` příjemce po vyvolání `null`.</span><span class="sxs-lookup"><span data-stu-id="03618-336">It would break long standing invariant that a method call cannot turn non-`null` receiver to become `null` after invocation.</span></span>
> <span data-ttu-id="03618-337">Ve skutečnosti se v současné době ne`null` proměnná nestane `null`, pokud _explicitně_ nepřiřazuje nebo nepředává `ref` nebo `out`.</span><span class="sxs-lookup"><span data-stu-id="03618-337">In fact, currently a non-`null` variable cannot become `null` unless _explicitly_ assigned or passed by `ref` or `out`.</span></span>
> <span data-ttu-id="03618-338">To může významně vyhodnotit čitelnost nebo jiné formy "může to být tato" hodnota "null".</span><span class="sxs-lookup"><span data-stu-id="03618-338">That greatly aids readability or other forms of "can this be a null here" analysis.</span></span>
3.  <span data-ttu-id="03618-339">Bylo by těžké sjednotit se sémantikou "vyhodnotit jednou" pro podmíněné přístupy s hodnotou null.</span><span class="sxs-lookup"><span data-stu-id="03618-339">It would be hard to reconcile with "evaluate once" semantics of null-conditional accesses.</span></span>
<span data-ttu-id="03618-340">Příklad: `obj.stringField?.RefExtension(...)` – je třeba zachytit kopii `stringField`, aby byla hodnota null smysluplná, ale přiřazení `this` uvnitř RefExtension se neprojeví zpět do pole.</span><span class="sxs-lookup"><span data-stu-id="03618-340">Example: `obj.stringField?.RefExtension(...)` - need to capture a copy of `stringField` to make the null check meaningful, but then assignments to `this` inside RefExtension would not be reflected back to the field.</span></span>

<span data-ttu-id="03618-341">Schopnost deklarovat metody rozšíření ve **strukturách** , které přijímají první argument odkazem, byl dlouhotrvající požadavek.</span><span class="sxs-lookup"><span data-stu-id="03618-341">An ability to declare extension methods on **structs** that take the first argument by reference was a long-standing request.</span></span> <span data-ttu-id="03618-342">Jedním z aspektů blokování bylo "co se stane, když příjemce není LValue?".</span><span class="sxs-lookup"><span data-stu-id="03618-342">One of the blocking consideration was "what happens if receiver is not an LValue?".</span></span>

- <span data-ttu-id="03618-343">Existuje předchůdce, který může být také volán jako statická metoda (někdy jde o jediný způsob, jak vyřešit nejednoznačnost).</span><span class="sxs-lookup"><span data-stu-id="03618-343">There is a precedent that any extension method could also be called as a static method (sometimes it is the only way to resolve ambiguity).</span></span> <span data-ttu-id="03618-344">Mělo by to být, že přijímače RValue by měly být nepovolené.</span><span class="sxs-lookup"><span data-stu-id="03618-344">It would dictate that RValue receivers should be disallowed.</span></span>
- <span data-ttu-id="03618-345">Na druhé straně existuje postup, jak vyvolat volání při kopírování v podobných situacích, kdy jsou zapojeny metody instance struktury.</span><span class="sxs-lookup"><span data-stu-id="03618-345">On the other hand there is a practice of making invocation on a copy in similar situations when struct instance methods are involved.</span></span>

<span data-ttu-id="03618-346">Důvod, proč existuje implicitní kopírování, je, protože většina metod struktury ve skutečnosti nemění strukturu, ale nedokáže ji indikovat.</span><span class="sxs-lookup"><span data-stu-id="03618-346">The reason why the "implicit copying" exists is because the majority of struct methods do not actually modify the struct while not being able to indicate that.</span></span> <span data-ttu-id="03618-347">Z toho důvodu by bylo praktické řešení pouze udělat při vyvolání kopie, ale tento postup je znám pro poškození výkonu a způsobuje chyby.</span><span class="sxs-lookup"><span data-stu-id="03618-347">Therefore the most practical solution was to just make the invocation on a copy, but this practice is known for harming performance and causing bugs.</span></span>

<span data-ttu-id="03618-348">Nyní s dostupností parametrů `in` je možné, že rozšíření signalizuje záměr.</span><span class="sxs-lookup"><span data-stu-id="03618-348">Now, with availability of `in` parameters, it is possible for an extension to signal the intent.</span></span> <span data-ttu-id="03618-349">Conundrum je proto možné vyřešit vyžadováním rozšíření `ref`, která mají být volána s zapisovatelnou přijímači, zatímco rozšíření `in` povolují v případě potřeby implicitní kopírování.</span><span class="sxs-lookup"><span data-stu-id="03618-349">Therefore the conundrum can be resolved by requiring `ref` extensions to be called with writeable receivers while `in` extensions permit implicit copying if necessary.</span></span>

```csharp
// this can be called on either RValue or an LValue
public static void Reader(in this Guid self)
{
    // do something nonmutating.
    WriteLine(self == default(Guid));
}

// this can be called only on an LValue
public static void Mutator(ref this Guid self)
{
    // can mutate self
    self = new Guid();
}
```

### <a name="in-extensions-and-generics"></a><span data-ttu-id="03618-350">`in` rozšíření a obecné typy.</span><span class="sxs-lookup"><span data-stu-id="03618-350">`in` extensions and generics.</span></span>
<span data-ttu-id="03618-351">Účelem `ref`ch rozšiřujících metod je použití přijímače přímo nebo vyvoláním obdobných členů.</span><span class="sxs-lookup"><span data-stu-id="03618-351">The purpose of `ref` extension methods is to mutate the receiver directly or by invoking mutating members.</span></span> <span data-ttu-id="03618-352">Proto jsou rozšíření `ref this T` povolena, pokud je `T` omezené na strukturu.</span><span class="sxs-lookup"><span data-stu-id="03618-352">Therefore `ref this T` extensions are allowed as long as `T` is constrained to be a struct.</span></span>

<span data-ttu-id="03618-353">Na druhé straně `in` rozšiřující metody existují specificky pro omezení implicitního kopírování.</span><span class="sxs-lookup"><span data-stu-id="03618-353">On the other hand `in` extension methods exist specifically to reduce implicit copying.</span></span> <span data-ttu-id="03618-354">Nicméně jakékoli použití parametru `in T` bude nutné provést prostřednictvím členu rozhraní.</span><span class="sxs-lookup"><span data-stu-id="03618-354">However any use of an `in T` parameter will have to be done through an interface member.</span></span> <span data-ttu-id="03618-355">Vzhledem k tomu, že všechny členy rozhraní jsou považovány za mutace, takové použití by vyžadovalo kopii.</span><span class="sxs-lookup"><span data-stu-id="03618-355">Since all interface members are considered mutating, any such use would require a copy.</span></span> <span data-ttu-id="03618-356">– Místo zmenšení kopírování by tento efekt byl opakem.</span><span class="sxs-lookup"><span data-stu-id="03618-356">- Instead of reducing copying, the effect would be the opposite.</span></span> <span data-ttu-id="03618-357">Proto `in this T` není povoleno, pokud `T` je parametr obecného typu bez ohledu na omezení.</span><span class="sxs-lookup"><span data-stu-id="03618-357">Therefore `in this T` is not allowed when `T` is a generic type parameter regardless of constraints.</span></span>

### <a name="valid-kinds-of-extension-methods-recap"></a><span data-ttu-id="03618-358">Platný druh rozšiřujících metod (rekapitulace):</span><span class="sxs-lookup"><span data-stu-id="03618-358">Valid kinds of extension methods (recap):</span></span>
<span data-ttu-id="03618-359">Nyní jsou povoleny následující formuláře deklarace `this` v metodě rozšíření:</span><span class="sxs-lookup"><span data-stu-id="03618-359">The following forms of `this` declaration in an extension method are now allowed:</span></span>
1) <span data-ttu-id="03618-360">`this T arg` – regulární rozšíření ByVal</span><span class="sxs-lookup"><span data-stu-id="03618-360">`this T arg` - regular byval extension.</span></span> <span data-ttu-id="03618-361">(**existující případ**)</span><span class="sxs-lookup"><span data-stu-id="03618-361">(**existing case**)</span></span>
- <span data-ttu-id="03618-362">T může být jakýkoli typ, včetně typů odkazů nebo parametrů typu.</span><span class="sxs-lookup"><span data-stu-id="03618-362">T can be any type, including reference types or type parameters.</span></span>
<span data-ttu-id="03618-363">Instance bude po volání stejnou proměnnou.</span><span class="sxs-lookup"><span data-stu-id="03618-363">Instance will be the same variable after the call.</span></span>
<span data-ttu-id="03618-364">Povoluje implicitní převody _tohoto typu konverze argumentu_ .</span><span class="sxs-lookup"><span data-stu-id="03618-364">Allows implicit conversions of _this-argument-conversion_ kind.</span></span>
<span data-ttu-id="03618-365">Lze ji volat v rvalue.</span><span class="sxs-lookup"><span data-stu-id="03618-365">Can be called on RValues.</span></span>

- <span data-ttu-id="03618-366">`in this T self` - `in` rozšíření.</span><span class="sxs-lookup"><span data-stu-id="03618-366">`in this T self` - `in` extension.</span></span>
<span data-ttu-id="03618-367">T musí být skutečným typem struktury.</span><span class="sxs-lookup"><span data-stu-id="03618-367">T must be an actual struct type.</span></span>
<span data-ttu-id="03618-368">Instance bude po volání stejnou proměnnou.</span><span class="sxs-lookup"><span data-stu-id="03618-368">Instance will be the same variable after the call.</span></span>
<span data-ttu-id="03618-369">Povoluje implicitní převody _tohoto typu konverze argumentu_ .</span><span class="sxs-lookup"><span data-stu-id="03618-369">Allows implicit conversions of _this-argument-conversion_ kind.</span></span>
<span data-ttu-id="03618-370">Může být volána v rvalue (v případě potřeby může být vyvolána v dočasném případě).</span><span class="sxs-lookup"><span data-stu-id="03618-370">Can be called on RValues (may be invoked on a temp if needed).</span></span>

- <span data-ttu-id="03618-371">`ref this T self` - `ref` rozšíření.</span><span class="sxs-lookup"><span data-stu-id="03618-371">`ref this T self` - `ref` extension.</span></span>
<span data-ttu-id="03618-372">T musí být typ struktury nebo parametr obecného typu, který je omezený na strukturu.</span><span class="sxs-lookup"><span data-stu-id="03618-372">T must be a struct type or a generic type parameter constrained to be a struct.</span></span>
<span data-ttu-id="03618-373">Instance může být zapsána voláním.</span><span class="sxs-lookup"><span data-stu-id="03618-373">Instance may be written to by the invocation.</span></span>
<span data-ttu-id="03618-374">Povoluje pouze převody identity.</span><span class="sxs-lookup"><span data-stu-id="03618-374">Allows only identity conversions.</span></span>
<span data-ttu-id="03618-375">Musí být volána pro zapisovatelnou LValue.</span><span class="sxs-lookup"><span data-stu-id="03618-375">Must be called on writeable LValue.</span></span> <span data-ttu-id="03618-376">(nikdy se nevolá přes Temp).</span><span class="sxs-lookup"><span data-stu-id="03618-376">(never invoked via a temp).</span></span>

## <a name="readonly-ref-locals"></a><span data-ttu-id="03618-377">Lokální hodnoty REF ReadOnly.</span><span class="sxs-lookup"><span data-stu-id="03618-377">Readonly ref locals.</span></span>

### <a name="motivation"></a><span data-ttu-id="03618-378">Motivační.</span><span class="sxs-lookup"><span data-stu-id="03618-378">Motivation.</span></span>
<span data-ttu-id="03618-379">Po zavlečení `ref readonly` členů bylo jasné, že se nepoužily k párování s vhodným druhem místní.</span><span class="sxs-lookup"><span data-stu-id="03618-379">Once `ref readonly` members were introduced, it was clear from the use that they need to be paired with appropriate kind of local.</span></span> <span data-ttu-id="03618-380">Vyhodnocení člena může vytvořit nebo sledovat vedlejší účinky, proto pokud by výsledek měl být použit více než jednou, je nutné jej uložit.</span><span class="sxs-lookup"><span data-stu-id="03618-380">Evaluation of a member may produce or observe side effects, therefore if the result must be used more than once, it needs to be stored.</span></span> <span data-ttu-id="03618-381">Běžné `ref` národní prostředí vám k tomu nemůžeme, protože jim nelze přiřadit odkaz na `readonly`.</span><span class="sxs-lookup"><span data-stu-id="03618-381">Ordinary `ref` locals do not help here since they cannot be assigned a `readonly` reference.</span></span>   

### <a name="solution"></a><span data-ttu-id="03618-382">Řešení.</span><span class="sxs-lookup"><span data-stu-id="03618-382">Solution.</span></span>
<span data-ttu-id="03618-383">Umožňuje deklarovat `ref readonly` národní prostředí.</span><span class="sxs-lookup"><span data-stu-id="03618-383">Allow declaring `ref readonly` locals.</span></span> <span data-ttu-id="03618-384">Toto je nový druh `ref`ch národních prostředí, která nelze zapsat.</span><span class="sxs-lookup"><span data-stu-id="03618-384">This is a new kind of `ref` locals that is not writeable.</span></span> <span data-ttu-id="03618-385">V důsledku `ref readonly` národní prostředí mohou přijímat odkazy na proměnné jen pro čtení bez vystavení těchto proměnných pro zápisy.</span><span class="sxs-lookup"><span data-stu-id="03618-385">As a result `ref readonly` locals can accept references to readonly variables without exposing these variables to writes.</span></span>

### <a name="declaring-and-using-ref-readonly-locals"></a><span data-ttu-id="03618-386">Deklarování a používání `ref readonly`ch lokálních hodnot.</span><span class="sxs-lookup"><span data-stu-id="03618-386">Declaring and using `ref readonly` locals.</span></span>

<span data-ttu-id="03618-387">Syntaxe takových lokálních hodnot používá modifikátory `ref readonly` v lokalitě deklarací (v tomto konkrétním pořadí).</span><span class="sxs-lookup"><span data-stu-id="03618-387">The syntax of such locals uses `ref readonly` modifiers at declaration site (in that specific order).</span></span> <span data-ttu-id="03618-388">Podobně jako u běžných `ref`ch lokálních hodnot musí být `ref readonly` lokálních hodnot inicializovány v deklaraci.</span><span class="sxs-lookup"><span data-stu-id="03618-388">Similarly to ordinary `ref` locals, `ref readonly` locals must be ref-initialized at declaration.</span></span> <span data-ttu-id="03618-389">Na rozdíl od běžných `ref` místních hodnot můžou `ref readonly` místních hodnot odkazovat na `readonly` hodnoty lvalue, jako jsou `in` parametry, `readonly`ch polí, `ref readonly`ch metod.</span><span class="sxs-lookup"><span data-stu-id="03618-389">Unlike regular `ref` locals, `ref readonly` locals can refer to `readonly` LValues like `in` parameters, `readonly` fields, `ref readonly` methods.</span></span>

<span data-ttu-id="03618-390">Pro všechny účely se jako `readonly` proměnná považuje `ref readonly` Local.</span><span class="sxs-lookup"><span data-stu-id="03618-390">For all purposes a `ref readonly` local is treated as a `readonly` variable.</span></span> <span data-ttu-id="03618-391">Většina omezení pro použití je stejná jako u `readonly` polí nebo `in` parametrů.</span><span class="sxs-lookup"><span data-stu-id="03618-391">Most of the restrictions on the use are the same as with `readonly` fields or `in` parameters.</span></span>

<span data-ttu-id="03618-392">Například pole parametru `in`, který má typ struktury, jsou rekurzivně klasifikovány jako `readonly` proměnné.</span><span class="sxs-lookup"><span data-stu-id="03618-392">For example fields of an `in` parameter which has a struct type are all recursively classified as `readonly` variables .</span></span>   

```csharp
static readonly ref Vector3 M1() => . . .

static readonly ref Vector3 M1_Trace()
{
    // OK
    ref readonly var r1 = ref M1();

    // Not valid. Need an LValue
    ref readonly Vector3 r2 = ref default(Vector3);

    // Not valid. r1 is readonly.
    Mutate(ref r1);

    // OK.
    Print(in r1);

    // OK.
    return ref r1;
}
```

### <a name="restrictions-on-use-of-ref-readonly-locals"></a><span data-ttu-id="03618-393">Omezení použití `ref readonly`ch národních prostředí</span><span class="sxs-lookup"><span data-stu-id="03618-393">Restrictions on use of `ref readonly` locals</span></span>
<span data-ttu-id="03618-394">S výjimkou jejich `readonly` povahy se `ref readonly` místních se chovají jako běžné `ref` národní prostředí a řídí se přesně stejnými omezeními.</span><span class="sxs-lookup"><span data-stu-id="03618-394">Except for their `readonly` nature, `ref readonly` locals behave like ordinary `ref` locals and are subject to exactly same restrictions.</span></span>  
<span data-ttu-id="03618-395">Například omezení týkající se zachycení v uzávěrech, deklarace v `async`ch metod nebo `safe-to-return` analýzy se rovnoměrně vztahují na `ref readonly` národní prostředí.</span><span class="sxs-lookup"><span data-stu-id="03618-395">For example restrictions related to capturing in closures, declaring in `async` methods or the `safe-to-return` analysis equally applies to `ref readonly` locals.</span></span>

## <a name="ternary-ref-expressions-aka-conditional-lvalues"></a><span data-ttu-id="03618-396">Ternární `ref` výrazy</span><span class="sxs-lookup"><span data-stu-id="03618-396">Ternary `ref` expressions.</span></span> <span data-ttu-id="03618-397">(neboli "podmíněný hodnoty lvalue")</span><span class="sxs-lookup"><span data-stu-id="03618-397">(aka "Conditional LValues")</span></span>

### <a name="motivation"></a><span data-ttu-id="03618-398">Motivační</span><span class="sxs-lookup"><span data-stu-id="03618-398">Motivation</span></span>
<span data-ttu-id="03618-399">Použití `ref` a `ref readonly`, která se zveřejňují, je nutné, aby taková národní prostředí byla inicializována s jednou nebo jinou cílovou proměnnou na základě podmínky.</span><span class="sxs-lookup"><span data-stu-id="03618-399">Use of `ref` and `ref readonly` locals exposed a need to ref-initialize such locals with one or another target variable based on a condition.</span></span>

<span data-ttu-id="03618-400">Typickým řešením je zavést metodu jako:</span><span class="sxs-lookup"><span data-stu-id="03618-400">A typical workaround is to introduce a method like:</span></span>

```csharp
ref T Choice(bool condition, ref T consequence, ref T alternative)
{
    if (condition)
    {
         return ref consequence;
    }
    else
    {
         return ref alternative;
    }
}
```

<span data-ttu-id="03618-401">Všimněte si, že `Choice` není přesným nahrazením Ternární, protože _všechny_ argumenty musí být vyhodnoceny na webu volání, což vedlo k neintuitivnímu chování a chybám.</span><span class="sxs-lookup"><span data-stu-id="03618-401">Note that `Choice` is not an exact replacement of a ternary since _all_ arguments must be evaluated at the call site, which was leading to unintuitive behavior and bugs.</span></span>

<span data-ttu-id="03618-402">Následující nebudou fungovat podle očekávání:</span><span class="sxs-lookup"><span data-stu-id="03618-402">The following will not work as expected:</span></span>

```csharp
    // will crash with NRE because 'arr[0]' will be executed unconditionally
    ref var r = ref Choice(arr != null, ref arr[0], ref otherArr[0]);
```

### <a name="solution"></a><span data-ttu-id="03618-403">Řešení</span><span class="sxs-lookup"><span data-stu-id="03618-403">Solution</span></span>
<span data-ttu-id="03618-404">Povolí zvláštní druh podmíněného výrazu, který se vyhodnotí jako odkaz na jeden z argumentů LValue na základě podmínky.</span><span class="sxs-lookup"><span data-stu-id="03618-404">Allow special kind of conditional expression that evaluates to a reference to one of LValue argument based on a condition.</span></span>

### <a name="using-ref-ternary-expression"></a><span data-ttu-id="03618-405">Použití výrazu `ref` Ternární</span><span class="sxs-lookup"><span data-stu-id="03618-405">Using `ref` ternary expression.</span></span>

<span data-ttu-id="03618-406">Syntaxe `ref`ho charakteru podmíněného výrazu je `<condition> ? ref <consequence> : ref <alternative>;`</span><span class="sxs-lookup"><span data-stu-id="03618-406">The syntax for the `ref` flavor of a conditional expression is `<condition> ? ref <consequence> : ref <alternative>;`</span></span>

<span data-ttu-id="03618-407">Stejně jako u běžného podmíněného výrazu `<consequence>` nebo `<alternative>` vyhodnocen v závislosti na výsledku výrazu logické podmínky.</span><span class="sxs-lookup"><span data-stu-id="03618-407">Just like with the ordinary conditional expression only `<consequence>` or `<alternative>` is evaluated depending on result of the boolean condition expression.</span></span>

<span data-ttu-id="03618-408">Na rozdíl od obyčejného podmíněného výrazu `ref` podmíněný výraz:</span><span class="sxs-lookup"><span data-stu-id="03618-408">Unlike ordinary conditional expression, `ref` conditional expression:</span></span>
- <span data-ttu-id="03618-409">vyžaduje, aby se `<consequence>` a `<alternative>` hodnoty lvalue.</span><span class="sxs-lookup"><span data-stu-id="03618-409">requires that `<consequence>` and `<alternative>` are LValues.</span></span>
- <span data-ttu-id="03618-410">`ref` samotného podmíněného výrazu je l-hodnota a</span><span class="sxs-lookup"><span data-stu-id="03618-410">`ref` conditional expression itself is an LValue and</span></span>
- <span data-ttu-id="03618-411">`ref` podmíněný výraz je zapisovatelný, pokud jsou `<consequence>` i `<alternative>` zapisovatelné hodnoty lvalue</span><span class="sxs-lookup"><span data-stu-id="03618-411">`ref` conditional expression is writeable if both `<consequence>` and `<alternative>` are writeable LValues</span></span>

<span data-ttu-id="03618-412">Příklady:</span><span class="sxs-lookup"><span data-stu-id="03618-412">Examples:</span></span>  
<span data-ttu-id="03618-413">`ref` Ternární je l-hodnota a jako taková může být předána/přiřazena/vrácena odkazem;</span><span class="sxs-lookup"><span data-stu-id="03618-413">`ref` ternary is an LValue and as such it can be passed/assigned/returned by reference;</span></span>
```csharp
     // pass by reference
     foo(ref (arr != null ? ref arr[0]: ref otherArr[0]));

     // return by reference
     return ref (arr != null ? ref arr[0]: ref otherArr[0]);
```

<span data-ttu-id="03618-414">Je-li to hodnota l-hodnota, lze ji také přiřadit.</span><span class="sxs-lookup"><span data-stu-id="03618-414">Being an LValue, it can also be assigned to.</span></span>
```csharp
     // assign to
     (arr != null ? ref arr[0]: ref otherArr[0]) = 1;

     // error. readOnlyField is readonly and thus conditional expression is readonly
     (arr != null ? ref arr[0]: ref obj.readOnlyField) = 1;
```

<span data-ttu-id="03618-415">Dá se použít jako přijímač volání metody a pokud je to potřeba, můžete v případě potřeby vynechat kopírování.</span><span class="sxs-lookup"><span data-stu-id="03618-415">Can be used as a receiver of a method call and skip copying if necessary.</span></span>
```csharp
     // no copies
     (arr != null ? ref arr[0]: ref otherArr[0]).StructMethod();

     // invoked on a copy.
     // The receiver is `readonly` because readOnlyField is readonly.
     (arr != null ? ref arr[0]: ref obj.readOnlyField).StructMethod();

     // no copies. `ReadonlyStructMethod` is a method on a `readonly` struct
     // and can be invoked directly on a readonly receiver
     (arr != null ? ref arr[0]: ref obj.readOnlyField).ReadonlyStructMethod();
```

<span data-ttu-id="03618-416">`ref` Ternární lze použít také v běžném (nikoli ref) kontextu.</span><span class="sxs-lookup"><span data-stu-id="03618-416">`ref` ternary can be used in a regular (not ref) context as well.</span></span>
```csharp
     // only an example
     // a regular ternary could work here just the same
     int x = (arr != null ? ref arr[0]: ref otherArr[0]);
```

### <a name="drawbacks"></a><span data-ttu-id="03618-417">Nevýhody</span><span class="sxs-lookup"><span data-stu-id="03618-417">Drawbacks</span></span>
[drawbacks]: #drawbacks

<span data-ttu-id="03618-418">Vidím dva hlavní argumenty s rozšířenou podporou pro odkazy a odkazy jen pro čtení:</span><span class="sxs-lookup"><span data-stu-id="03618-418">I can see two major arguments against enhanced support for references and readonly references:</span></span>

1) <span data-ttu-id="03618-419">Problémy, které jsou zde vyřešeny, jsou velmi staré.</span><span class="sxs-lookup"><span data-stu-id="03618-419">The problems that are solved here are very old.</span></span> <span data-ttu-id="03618-420">Proč je teď už náhle vyřešte, zejména proto, že by vám nepomohl existující kód?</span><span class="sxs-lookup"><span data-stu-id="03618-420">Why suddenly solve them now, especially since it would not help existing code?</span></span>

<span data-ttu-id="03618-421">Jak jsme našli C# a používali .NET v nových doménách, některé problémy se výrazně projeví.</span><span class="sxs-lookup"><span data-stu-id="03618-421">As we find C# and .Net used in new domains, some problems become more prominent.</span></span>  
<span data-ttu-id="03618-422">Jako příklady prostředí, která jsou z hlediska výpočetních režijních nákladů důležitější než průměr, můžu seznam</span><span class="sxs-lookup"><span data-stu-id="03618-422">As examples of environments that are more critical than average about computation overheads, I can list</span></span>

* <span data-ttu-id="03618-423">scénáře cloudu nebo Datacenter, kde se účtují výpočty a reakce, je konkurenční výhoda.</span><span class="sxs-lookup"><span data-stu-id="03618-423">cloud/datacenter scenarios where computation is billed for and responsiveness is a competitive advantage.</span></span>
* <span data-ttu-id="03618-424">Hry/VR/AR s požadavky tichého v reálném čase na latenci</span><span class="sxs-lookup"><span data-stu-id="03618-424">Games/VR/AR with soft-realtime requirements on latencies</span></span>     

<span data-ttu-id="03618-425">Tato funkce nebrání žádné z existujících pevností, jako je například bezpečnost typů, a přitom umožňuje nižší režijní náklady v některých běžných scénářích.</span><span class="sxs-lookup"><span data-stu-id="03618-425">This feature does not sacrifice any of the existing strengths such as type-safety, while allowing to lower overheads in some common scenarios.</span></span>

2) <span data-ttu-id="03618-426">Můžeme rozumně zaručit, že volaný bude při výslovnýí kontraktů `readonly` do smlouvy přehrajte pravidla?</span><span class="sxs-lookup"><span data-stu-id="03618-426">Can we reasonably guarantee that the callee will play by the rules when it opts into `readonly` contracts?</span></span>

<span data-ttu-id="03618-427">Při použití `out`máme podobný vztah důvěryhodnosti.</span><span class="sxs-lookup"><span data-stu-id="03618-427">We have similar trust when using `out`.</span></span> <span data-ttu-id="03618-428">Nesprávná implementace `out` může způsobit nespecifikované chování, ale ve skutečnosti to nastane jenom zřídka.</span><span class="sxs-lookup"><span data-stu-id="03618-428">Incorrect implementation of `out` can cause unspecified behavior, but in reality it rarely happens.</span></span>  

<span data-ttu-id="03618-429">Provádění formálních ověřovacích pravidel, která jsou známá `ref readonly`, by dále zmírnily problém důvěryhodnosti.</span><span class="sxs-lookup"><span data-stu-id="03618-429">Making the formal verification rules familiar with `ref readonly` would further mitigate the trust issue.</span></span>

### <a name="alternatives"></a><span data-ttu-id="03618-430">Alternativy</span><span class="sxs-lookup"><span data-stu-id="03618-430">Alternatives</span></span>
[alternatives]: #alternatives

<span data-ttu-id="03618-431">Hlavní konkurenční návrh je skutečně "nedělat nic".</span><span class="sxs-lookup"><span data-stu-id="03618-431">The main competing design is really "do nothing".</span></span>

### <a name="unresolved-questions"></a><span data-ttu-id="03618-432">Nevyřešené dotazy</span><span class="sxs-lookup"><span data-stu-id="03618-432">Unresolved questions</span></span>
[unresolved]: #unresolved-questions

### <a name="design-meetings"></a><span data-ttu-id="03618-433">Schůzky pro návrh</span><span class="sxs-lookup"><span data-stu-id="03618-433">Design meetings</span></span>

<span data-ttu-id="03618-434">https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-02-22.md https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-03-01.md https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-08-28.md https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-09-25.md https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-09-27.md</span><span class="sxs-lookup"><span data-stu-id="03618-434">https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-02-22.md https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-03-01.md https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-08-28.md https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-09-25.md https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-09-27.md</span></span>
