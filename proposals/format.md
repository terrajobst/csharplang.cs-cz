---
ms.openlocfilehash: 1457c1eb018e12af30ce5b38be704bf8851d4b25
ms.sourcegitcommit: 7f7fc6e9e195e51b7ff8229aeaa70aa9fbbb63cb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/10/2019
ms.locfileid: "79485012"
---
# <a name="efficient-params-and-string-formatting"></a><span data-ttu-id="cda77-101">Efektivní parametry a formátování řetězců</span><span class="sxs-lookup"><span data-stu-id="cda77-101">Efficient Params and String Formatting</span></span>

## <a name="summary"></a><span data-ttu-id="cda77-102">Souhrn</span><span class="sxs-lookup"><span data-stu-id="cda77-102">Summary</span></span>
<span data-ttu-id="cda77-103">Tato kombinace funkcí zvýší efektivitu formátování `string` hodnoty a předávání argumentů stylu `params`.</span><span class="sxs-lookup"><span data-stu-id="cda77-103">This combination of features will increase the efficiency of formatting `string` values and passing of `params` style arguments.</span></span>

## <a name="motivation"></a><span data-ttu-id="cda77-104">Motivační</span><span class="sxs-lookup"><span data-stu-id="cda77-104">Motivation</span></span>
<span data-ttu-id="cda77-105">Režie přidělení formátování `string` hodnoty může podsáhnout výkon mnoha textových aplikací: od pokuty `struct` typů, `object[]` přidělení pro `params` a průběžné `string` alokace během `string.Format` volání.</span><span class="sxs-lookup"><span data-stu-id="cda77-105">The allocation overhead of formatting `string` values can dominate the performance of many text based applications: from the boxing penalty of `struct` types, the `object[]` allocation for `params` and the intermediate `string` allocations during `string.Format` calls.</span></span> <span data-ttu-id="cda77-106">Aby bylo možné zajistit efektivitu takových aplikací, často potřebují opustit funkce pro zvýšení produktivity, jako je například `params` a `string` interpolace a přechod na nestandardní, ručně kódovaná řešení.</span><span class="sxs-lookup"><span data-stu-id="cda77-106">In order to maintain efficiency such applications often need to abandon productivity features such as `params` and `string` interpolation and move to non-standard, hand coded solutions.</span></span> 

<span data-ttu-id="cda77-107">Jako příklad zvažte MSBuild.</span><span class="sxs-lookup"><span data-stu-id="cda77-107">Consider MSBuild as an example.</span></span> <span data-ttu-id="cda77-108">To je napsáno pomocí mnoha moderních C# funkcí vývojářům, kteří mají vědomu výkonu.</span><span class="sxs-lookup"><span data-stu-id="cda77-108">This is written using a lot of modern C# features by developers who are conscious of performance.</span></span> <span data-ttu-id="cda77-109">V jednom z reprezentativních ukázek buildů MSBuild se ale pomocí minimální podrobností vygeneruje 262MB přidělení `string`.</span><span class="sxs-lookup"><span data-stu-id="cda77-109">Yet in one representative build sample MSBuild will generate 262MB of `string` allocation using minimal verbosity.</span></span> <span data-ttu-id="cda77-110">Z této 1/2 přidělení jsou krátkodobá přidělení v rámci `string.Format`.</span><span class="sxs-lookup"><span data-stu-id="cda77-110">Of that 1/2 of the allocations are short lived allocations inside `string.Format`.</span></span> <span data-ttu-id="cda77-111">Tyto funkce by tyto funkce odebraly mnohem z těchto funkcí v prostředí .NET Desktop a v důsledku dostupnosti `Span<T>` tak téměř nulou v .NET Core.</span><span class="sxs-lookup"><span data-stu-id="cda77-111">These features would remove much of that on .NET Desktop and get it down to nearly zero on .NET Core due to the availability of `Span<T>`</span></span>

<span data-ttu-id="cda77-112">Sada jazykových funkcí popsaných tady umožní aplikacím dál používat tyto funkce, a to s hodně nebo bez jakýchkoli změn základu aplikačního kódu a zároveň odebrat nezamýšlené nároky na přidělení ve většině případů.</span><span class="sxs-lookup"><span data-stu-id="cda77-112">The set of language features described here will enable applications to continue using these features, with very little or no churn to their application code base, while removing the unintended allocation overhead in the majority of cases.</span></span>

## <a name="detailed-design"></a><span data-ttu-id="cda77-113">Podrobný návrh</span><span class="sxs-lookup"><span data-stu-id="cda77-113">Detailed Design</span></span> 
<span data-ttu-id="cda77-114">Pro dosažení těchto výsledků se tady používá sada funkcí:</span><span class="sxs-lookup"><span data-stu-id="cda77-114">There are a set of features that will be used here to achieve these results:</span></span>

- <span data-ttu-id="cda77-115">Rozbalení `params` pro podporu širší sady typů kolekcí.</span><span class="sxs-lookup"><span data-stu-id="cda77-115">Expanding `params` to support a broader set of collection types.</span></span>
- <span data-ttu-id="cda77-116">Umožnění vývojářům přizpůsobit, jak je dosaženo `string` interpolace.</span><span class="sxs-lookup"><span data-stu-id="cda77-116">Allowing for developers to customize how `string` interpolation is achieved.</span></span> 
- <span data-ttu-id="cda77-117">Umožnění interpolace `string` vázání na efektivnější `string.Format` přetížení.</span><span class="sxs-lookup"><span data-stu-id="cda77-117">Allowing for interpolated `string` to bind to more efficient `string.Format` overloads.</span></span>

### <a name="extending-params"></a><span data-ttu-id="cda77-118">Rozšiřování parametrů</span><span class="sxs-lookup"><span data-stu-id="cda77-118">Extending params</span></span>
<span data-ttu-id="cda77-119">Jazyk umožní `params` v signatuře metody mít typy `Span<T>`, `ReadOnlySpan<T>` a `IEnumerable<T>`.</span><span class="sxs-lookup"><span data-stu-id="cda77-119">The language will allow for `params` in a method signature to have the types `Span<T>`, `ReadOnlySpan<T>` and `IEnumerable<T>`.</span></span> <span data-ttu-id="cda77-120">Stejná pravidla pro vyvolání se vztahují na tyto nové typy, které platí pro `params T[]`:</span><span class="sxs-lookup"><span data-stu-id="cda77-120">The same rules for invocation will apply to these new types that apply to `params T[]`:</span></span>

- <span data-ttu-id="cda77-121">Nejde přetížit, kde jediným rozdílem je klíčové slovo `params`.</span><span class="sxs-lookup"><span data-stu-id="cda77-121">Can't overload where the only difference is a `params` keyword.</span></span>
- <span data-ttu-id="cda77-122">Lze vyvolat předáním řady argumentů, které jsou implicitně konvertibilníelné na `T` nebo jednoho `Span<T>` / 
`ReadOnlySpan<T>`argument  / `IEnumerable<T>`.</span><span class="sxs-lookup"><span data-stu-id="cda77-122">Can invoke by passing a series of arguments that are implicitly convertible to `T` or a single `Span<T>` / 
`ReadOnlySpan<T>` / `IEnumerable<T>` argument.</span></span>
- <span data-ttu-id="cda77-123">Musí se jednat o poslední parametr v signatuře metody.</span><span class="sxs-lookup"><span data-stu-id="cda77-123">Must be the last parameter in a method signature.</span></span>
- <span data-ttu-id="cda77-124">Atd...</span><span class="sxs-lookup"><span data-stu-id="cda77-124">Etc ...</span></span> 

<span data-ttu-id="cda77-125">Varianty `Span<T>` a `ReadOnlySpan<T>` se pro jednoduchost označují jako `Span<T>` níže.</span><span class="sxs-lookup"><span data-stu-id="cda77-125">The `Span<T>` and `ReadOnlySpan<T>` variants will be referred to as `Span<T>` below for simplicity.</span></span> <span data-ttu-id="cda77-126">V případech, kdy se chování `ReadOnlySpan<T>` liší, bude explicitně vyvoláno.</span><span class="sxs-lookup"><span data-stu-id="cda77-126">In cases where the behavior of `ReadOnlySpan<T>` differs it will be explicitly called out.</span></span> 

<span data-ttu-id="cda77-127">Výhodou `Span<T>` varianty `params` poskytuje kompilátor skvělou flexibilitu při přidělování záložního úložiště pro hodnotu `Span<T>`.</span><span class="sxs-lookup"><span data-stu-id="cda77-127">The advantage the `Span<T>` variants of `params` provides is it gives the compiler great flexibility in how it allocates the backing storage for the `Span<T>` value.</span></span> <span data-ttu-id="cda77-128">U `params T[]` kompilátor musí přidělit nové `T[]` pro každé vyvolání metody `params`.</span><span class="sxs-lookup"><span data-stu-id="cda77-128">With a `params T[]` the compiler must allocate a new `T[]` for every invocation of a `params` method.</span></span> <span data-ttu-id="cda77-129">Opakované použití není možné, protože musí předpokládat volaný uložený a znovu použitý parametr.</span><span class="sxs-lookup"><span data-stu-id="cda77-129">Re-use is not possible because it must assume the callee stored and reused the parameter.</span></span> <span data-ttu-id="cda77-130">To může vést k velké neefektivitě v metodách s velkým množstvím `params`ch volání.</span><span class="sxs-lookup"><span data-stu-id="cda77-130">This can lead to a large inefficiency in methods with lots of `params` invocations.</span></span>

<span data-ttu-id="cda77-131">Zadané varianty `Span<T>` jsou `ref struct` volaný nemůže uložit argument.</span><span class="sxs-lookup"><span data-stu-id="cda77-131">Given `Span<T>` variants are `ref struct` the callee cannot store the argument.</span></span> <span data-ttu-id="cda77-132">Kompilátor proto může optimalizovat lokality voláním pomocí akcí, jako je opakované použití argumentu.</span><span class="sxs-lookup"><span data-stu-id="cda77-132">Hence the compiler can optimize the call sites by taking actions like re-using the argument.</span></span> <span data-ttu-id="cda77-133">Díky tomu může být opakované vyvolání ve srovnání s `T[]`velmi efektivní.</span><span class="sxs-lookup"><span data-stu-id="cda77-133">This can make repeated invocations very efficient as compared to `T[]`.</span></span> <span data-ttu-id="cda77-134">Jazyk ale neposkytuje žádné zvláštní záruky týkající se toho, jak jsou tyto callsites optimalizované.</span><span class="sxs-lookup"><span data-stu-id="cda77-134">The language though will make no specific guarantees about how such callsites are optimized.</span></span> <span data-ttu-id="cda77-135">Všimněte si, že kompilátor je zdarma používat jiné hodnoty než `T[]` při vyvolání metody `params Span<T>`.</span><span class="sxs-lookup"><span data-stu-id="cda77-135">Only note that the compiler is free to use values other than `T[]` when invoking a `params Span<T>` method.</span></span> 

<span data-ttu-id="cda77-136">Jedna taková možná implementace je následující.</span><span class="sxs-lookup"><span data-stu-id="cda77-136">One such potential implementation is the following.</span></span> <span data-ttu-id="cda77-137">Zvažte všechna `params` vyvolání v těle metody.</span><span class="sxs-lookup"><span data-stu-id="cda77-137">Consider all `params` invocation in a method body.</span></span> <span data-ttu-id="cda77-138">Kompilátor může přidělit pole, které má velikost rovnou největšímu `params` vyvolání, a použít ho pro všechna vyvolání vytvořením vhodně se `Span<T>` instancí v poli.</span><span class="sxs-lookup"><span data-stu-id="cda77-138">The compiler could allocate an array which has a size equal to the largest `params` invocation and use that for all of the invocations by creating appropriately sized `Span<T>` instances over the array.</span></span> <span data-ttu-id="cda77-139">Příklad:</span><span class="sxs-lookup"><span data-stu-id="cda77-139">For example:</span></span>

``` csharp
static class OneAllocation {
    static void Use(params Span<string> spans) {
        ...
    }

    static void Go() {
        Use("jaredpar");
        Use("hello", "world");
        Use("a", "longer", "set");
    }
}
```

<span data-ttu-id="cda77-140">Kompilátor může zvolit, aby vygeneroval tělo `Go` následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="cda77-140">The compiler could choose to emit the body of `Go` as follows:</span></span>

``` csharp
    static void Go() {
        var args = new string[3];
        args[0] = "jaredpar";
        Use(new Span<string>(args, start: 0, length: 1));

        args[0] = "hello";
        args[1] = "world";
        Use(new Span<string>(args, start: 0, length: 2));

        args[0] = "a";
        args[1] = "longer";
        args[2] = "set";
        Use(new Span<string>(args, start: 0, length: 3));
   }
```

<span data-ttu-id="cda77-141">To může významně snížit počet polí přidělených v aplikaci.</span><span class="sxs-lookup"><span data-stu-id="cda77-141">This can significantly reduce the number of arrays allocated in an application.</span></span> <span data-ttu-id="cda77-142">Přidělení se dá ještě víc snížit, pokud modul runtime poskytuje nástroje pro vyřazení polí do paměti pro inteligentnější zásobník.</span><span class="sxs-lookup"><span data-stu-id="cda77-142">Allocations can be even further reduced if the runtime provides utilities for smarter stack allocation of arrays.</span></span>

<span data-ttu-id="cda77-143">Tuto optimalizaci nelze vždy použít, i když.</span><span class="sxs-lookup"><span data-stu-id="cda77-143">This optimization cannot always be applied though.</span></span> <span data-ttu-id="cda77-144">I když volaný nemůže zachytit argument `params`, může být stále zachycena v případě, že existuje `ref` nebo parametr `out / ref`, který je sám `ref struct` typu.</span><span class="sxs-lookup"><span data-stu-id="cda77-144">Even though the callee cannot capture the `params` argument it can still be captured in the caller when there is a `ref` or a `out / ref` parameter that is itself a `ref struct` type.</span></span> 

``` csharp
static class SneakyCapture {
    static ref int M(params Span<T> span) => ref span[0];

    static void Oops() {
        // This now holds onto the memory backing the Span<T> 
        ref int r = ref M(42);
    }
}
```

<span data-ttu-id="cda77-145">Tyto případy jsou staticky zjistitelné i přesto.</span><span class="sxs-lookup"><span data-stu-id="cda77-145">These cases are statically detectable though.</span></span> <span data-ttu-id="cda77-146">K tomu může dojít, když se `ref` vrátí nebo `ref struct` parametr předaný `out` nebo `ref`.</span><span class="sxs-lookup"><span data-stu-id="cda77-146">It potentially occurs whenever there is a `ref` return or a `ref struct` parameter passed by `out` or `ref`.</span></span> <span data-ttu-id="cda77-147">V takovém případě musí kompilátor přidělit nové `T[]` pro každé vyvolání.</span><span class="sxs-lookup"><span data-stu-id="cda77-147">In such a case the compiler must allocate a fresh `T[]` for every invocation.</span></span> 

<span data-ttu-id="cda77-148">Na konci tohoto dokumentu je vysvětleno několik dalších možných strategií optimalizace.</span><span class="sxs-lookup"><span data-stu-id="cda77-148">Several other potential optimization strategies are discussed at the end of this document.</span></span>

<span data-ttu-id="cda77-149">`IEnumerable<T>` variant je pouze pohodlíně přetížení.</span><span class="sxs-lookup"><span data-stu-id="cda77-149">The `IEnumerable<T>` variant is a merely a convenience overload.</span></span> <span data-ttu-id="cda77-150">To je užitečné ve scénářích, které často využívají `IEnumerable<T>`, ale také mají spoustu `params` využití.</span><span class="sxs-lookup"><span data-stu-id="cda77-150">It's useful in scenarios which have frequent uses of `IEnumerable<T>` but also have lots of `params` usage.</span></span> <span data-ttu-id="cda77-151">Při vyvolání ve formě argumentu `T` se záložní úložiště přidělí jako `T[]`, stejně jako v současné době `params T[]`.</span><span class="sxs-lookup"><span data-stu-id="cda77-151">When invoked in `T` argument form the backing storage will be allocated as a `T[]` just as `params T[]` is done today.</span></span>

### <a name="params-overload-resolution-changes"></a><span data-ttu-id="cda77-152">změny rozlišení přetížení parametrů</span><span class="sxs-lookup"><span data-stu-id="cda77-152">params overload resolution changes</span></span>
<span data-ttu-id="cda77-153">Tento návrh znamená, že jazyk teď má čtyři varianty `params`, kde předtím, než nějaký byl.</span><span class="sxs-lookup"><span data-stu-id="cda77-153">This proposal means the language now has four variants of `params` where before it had one.</span></span> <span data-ttu-id="cda77-154">Je rozumné pro metody definování přetížení metod, které se liší pouze u typu deklarace `params`.</span><span class="sxs-lookup"><span data-stu-id="cda77-154">It is sensible for methods to define overloads of methods that differ only on the type of a `params` declarations.</span></span> 

<span data-ttu-id="cda77-155">Vezměte v úvahu, že `StringBuilder.AppendFormat` by jistě kromě `params object[]`musel přidat přetížení `params ReadOnlySpan<object>`.</span><span class="sxs-lookup"><span data-stu-id="cda77-155">Consider that `StringBuilder.AppendFormat` would certainly add a `params ReadOnlySpan<object>` overload in addition to the `params object[]`.</span></span> <span data-ttu-id="cda77-156">To by umožnilo významně zvýšit výkon snížením přidělení kolekce bez nutnosti jakýchkoli změn volajícího kódu.</span><span class="sxs-lookup"><span data-stu-id="cda77-156">This would allow it to substantially improve performance by reducing collection allocations without requiring any changes to the calling code.</span></span> 

<span data-ttu-id="cda77-157">Aby bylo možné tento jazyk usnadnit, zavede následující pravidlo pro přerušení propojení v rámci rozlišení přetížení.</span><span class="sxs-lookup"><span data-stu-id="cda77-157">To facilitate this the language will introduce the following overload resolution tie breaking rule.</span></span> <span data-ttu-id="cda77-158">Pokud se kandidátské metody liší jenom parametrem `params`, pak kandidáti budou upřednostňováni v následujícím pořadí:</span><span class="sxs-lookup"><span data-stu-id="cda77-158">When the candidate methods differ only by the `params` parameter then the candidates will be preferred in the following order:</span></span>

1. `ReadOnlySpan<T>`
1. `Span<T>`
1. `T[]`
1. `IEnumerable<T>`

<span data-ttu-id="cda77-159">Toto pořadí je co nejmenších nejúčinnějších pro obecný případ.</span><span class="sxs-lookup"><span data-stu-id="cda77-159">This order is the most to the least efficient for the general case.</span></span>

### <a name="variant"></a><span data-ttu-id="cda77-160">Varianty</span><span class="sxs-lookup"><span data-stu-id="cda77-160">Variant</span></span>
<span data-ttu-id="cda77-161">CoreFX je prototypování nového spravovaného typu s názvem [variant](https://github.com/dotnet/corefxlab/pull/2595).</span><span class="sxs-lookup"><span data-stu-id="cda77-161">CoreFX is prototyping a new managed type named [Variant](https://github.com/dotnet/corefxlab/pull/2595).</span></span> <span data-ttu-id="cda77-162">Tento typ je určen pro použití v rozhraních API, které očekávají heterogenní hodnoty, ale nechtějí režii, která se používá `object` jako parametr.</span><span class="sxs-lookup"><span data-stu-id="cda77-162">This type is meant to be used in APIs which expect heterogeneous values but don't want the overhead brought on by using `object` as the parameter.</span></span> <span data-ttu-id="cda77-163">Typ `Variant` poskytuje univerzální úložiště, ale nebrání přidělení zabalení pro nejčastěji používané typy.</span><span class="sxs-lookup"><span data-stu-id="cda77-163">The `Variant` type provides universal storage but avoids the boxing allocation for the most commonly used types.</span></span> <span data-ttu-id="cda77-164">Použití tohoto typu v rozhraních API, jako je `string.Format`, může eliminovat režijní náklady ve většině případů.</span><span class="sxs-lookup"><span data-stu-id="cda77-164">Using this type in APIs like `string.Format` can eliminate the boxing overhead in the majority of cases.</span></span>

<span data-ttu-id="cda77-165">Tento typ samotný není nutně speciální pro jazyk.</span><span class="sxs-lookup"><span data-stu-id="cda77-165">This type itself is not necessarily special to the language.</span></span> <span data-ttu-id="cda77-166">V tomto dokumentu se zavádí samostatně, ale v takovém případě se jedná o podrobnosti o implementaci jiných částí návrhu.</span><span class="sxs-lookup"><span data-stu-id="cda77-166">It is being introduced in this document separately though as it becomes an implementation detail of other parts of the proposal.</span></span> 

### <a name="efficient-interpolated-strings"></a><span data-ttu-id="cda77-167">Efektivní interpolované řetězce</span><span class="sxs-lookup"><span data-stu-id="cda77-167">Efficient interpolated strings</span></span>
<span data-ttu-id="cda77-168">Interpolované řetězce jsou zatím neefektivní funkce v C#.</span><span class="sxs-lookup"><span data-stu-id="cda77-168">Interpolated strings are a popular yet inefficient feature in C#.</span></span> <span data-ttu-id="cda77-169">Nejběžnější syntaxe s použitím interpolované `string` jako `string`se přeloží na `string.Format(string, params object[])` volání.</span><span class="sxs-lookup"><span data-stu-id="cda77-169">The most common syntax, using an interpolated `string` as a `string`, translates into a `string.Format(string, params object[])` call.</span></span> <span data-ttu-id="cda77-170">To bude mít za následek přidělení zabalení pro všechny typy hodnot, zprostředkující `string` přidělení v rámci implementace do značné míry používá `object.ToString` pro formátování a také přidělení pole, když počet argumentů překročí množství parametrů u přetížení "Fast" `string.Format`.</span><span class="sxs-lookup"><span data-stu-id="cda77-170">That will incur boxing allocations for all value types, intermediate `string` allocations as the implementation largely uses `object.ToString` for formatting as well as array allocations once the number of arguments exceeds the amount of parameters on the "fast" overloads of `string.Format`.</span></span> 

<span data-ttu-id="cda77-171">Jazyk změní svou interpolaci na nižší, aby bylo možné zvážit alternativní přetížení `string.Format`.</span><span class="sxs-lookup"><span data-stu-id="cda77-171">The language will change its interpolation lowering to consider alternate overloads of `string.Format`.</span></span> <span data-ttu-id="cda77-172">Bude uvažovat o všech formulářích `string.Format(string, params)` a vybrat přetížení "nejlepší", které splňuje typy argumentů.</span><span class="sxs-lookup"><span data-stu-id="cda77-172">It will consider all forms of `string.Format(string, params)` and pick the "best" overload which satisfies the argument types.</span></span>
<span data-ttu-id="cda77-173">Přetížení "nejlepšího" `params` bude určeno pravidly uvedenými výše.</span><span class="sxs-lookup"><span data-stu-id="cda77-173">The "best" `params` overload will be determined by the rules discussed above.</span></span> <span data-ttu-id="cda77-174">To znamená, že interpolovaná `string` se teď můžou navazovat na velmi efektivní přetížení, jako je `string.Format(string format, params ReadOnlySpan<Variant> args)`.</span><span class="sxs-lookup"><span data-stu-id="cda77-174">This means interpolated `string` can now bind to very efficient overloads like `string.Format(string format, params ReadOnlySpan<Variant> args)`.</span></span> <span data-ttu-id="cda77-175">V mnoha případech se tím odeberou všechny mezilehlé alokace.</span><span class="sxs-lookup"><span data-stu-id="cda77-175">In many cases this will remove all intermediate allocations.</span></span>

### <a name="customizable-interpolated-strings"></a><span data-ttu-id="cda77-176">Přizpůsobitelné interpolované řetězce</span><span class="sxs-lookup"><span data-stu-id="cda77-176">Customizable interpolated strings</span></span>
<span data-ttu-id="cda77-177">Vývojáři mohou přizpůsobit chování interpolované řetězce pomocí `FormattableString`.</span><span class="sxs-lookup"><span data-stu-id="cda77-177">Developers are able to customize the behavior of interpolated strings with `FormattableString`.</span></span> <span data-ttu-id="cda77-178">Obsahuje data, která přecházejí do interpolované řetězce: formát `string` a argumenty jako pole.</span><span class="sxs-lookup"><span data-stu-id="cda77-178">This contains the data which goes into an interpolated string: the format `string` and the arguments as an array.</span></span> <span data-ttu-id="cda77-179">I když má stále přidělení pole zabalení a argumentu a také přidělení pro `FormattableString` (Jedná se o `abstract class`).</span><span class="sxs-lookup"><span data-stu-id="cda77-179">This though still has the boxing and argument array allocation as well as the allocation for `FormattableString` (it's an `abstract class`).</span></span> <span data-ttu-id="cda77-180">Proto se pro aplikace, které jsou ve formátování `string`, nepoužívá.</span><span class="sxs-lookup"><span data-stu-id="cda77-180">Hence it's of little use to applications which are allocation heavy in `string` formatting.</span></span>

<span data-ttu-id="cda77-181">Chcete-li efektivně zformátovat formátování řetězce, bude jazyk rozpoznávat nový typ: `System.ValueFormattableString`.</span><span class="sxs-lookup"><span data-stu-id="cda77-181">To make interpolated string formatting efficient the language will recognize a new type: `System.ValueFormattableString`.</span></span> <span data-ttu-id="cda77-182">Všechny interpolované řetězce budou mít převod cílového typu na tento typ.</span><span class="sxs-lookup"><span data-stu-id="cda77-182">All interpolated strings will have a target type conversion to this type.</span></span> <span data-ttu-id="cda77-183">To bude implementováno posunutím interpolované řetězce do volání `ValueFormattableString.Create` přesně tak, jak je provedeno pro `FormattableString.Create` dnes.</span><span class="sxs-lookup"><span data-stu-id="cda77-183">This will be implemented by translating the interpolated string into the call `ValueFormattableString.Create` exactly as is done for `FormattableString.Create` today.</span></span> <span data-ttu-id="cda77-184">Jazyk bude podporovat všechny možnosti `params` popsané v tomto dokumentu při hledání nejvhodnější `ValueFormattableString.Create` metody.</span><span class="sxs-lookup"><span data-stu-id="cda77-184">The language will support all `params` options described in this document when looking for the most suitable `ValueFormattableString.Create` method.</span></span> 

``` csharp
readonly struct ValueFormattableString {
    public static ValueFormattableString Create(Variant v) { ... } 
    public static ValueFormattableString Create(string s) { ... } 
    public static ValueFormattableString Create(string s, params ReadOnlySpan<Variant> collection) { ... } 
}

class ConsoleEx { 
    static void Write(ValueFormattableString f) { ... }
}

class Program { 
    static void Main() { 
        ConsoleEx.Write(42);
        ConsoleEx.Write($"hello {DateTime.UtcNow}");

        // Translates into 
        ConsoleEx.Write(ValueFormattableString.Create((Variant)42));
        ConsoleEx.Write(ValueFormattableString.Create(
            "hello {0}", 
            new Variant(DateTime.UtcNow));
    }
}
```

<span data-ttu-id="cda77-185">Pravidla rozlišení přetížení budou změněna tak, aby preferovat `ValueFormattableString` přes `string`, pokud je argumentem interpolovaná řetězec.</span><span class="sxs-lookup"><span data-stu-id="cda77-185">Overload resolution rules will be changed to prefer `ValueFormattableString` over `string` when the argument is an interpolated string.</span></span> <span data-ttu-id="cda77-186">To znamená, že bude užitečné mít přetížení, která se liší jenom `string` a `ValueFormattableString`.</span><span class="sxs-lookup"><span data-stu-id="cda77-186">This means it will be valuable to have overloads which differ only on `string` and `ValueFormattableString`.</span></span> <span data-ttu-id="cda77-187">Toto přetížení dnes s `FormattableString` není užitečné, protože kompilátor vždy upřednostňuje `string` verzi (pokud vývojář nepoužívá explicitní přetypování).</span><span class="sxs-lookup"><span data-stu-id="cda77-187">Such an overload today with `FormattableString` is not valuable as the compiler will always prefer the `string` version (unless the developer uses an explicit cast).</span></span> 

## <a name="open-issues"></a><span data-ttu-id="cda77-188">Otevřené problémy</span><span class="sxs-lookup"><span data-stu-id="cda77-188">Open Issues</span></span>

### <a name="valueformattablestring-breaking-change"></a><span data-ttu-id="cda77-189">ValueFormattableStringá změna</span><span class="sxs-lookup"><span data-stu-id="cda77-189">ValueFormattableString breaking change</span></span>
<span data-ttu-id="cda77-190">Tato změna upřednostňuje `ValueFormattableString` během překladu přetěžování přes `string` je zásadní změna.</span><span class="sxs-lookup"><span data-stu-id="cda77-190">The change to prefer `ValueFormattableString` during overload resolution over `string` is a breaking change.</span></span> <span data-ttu-id="cda77-191">Je možné, aby vývojář definoval typ nazvaný `ValueFormattableString` dnes a použil ho v přetížení metody s `string`.</span><span class="sxs-lookup"><span data-stu-id="cda77-191">It is possible for a developer to have defined a type called `ValueFormattableString` today and use it in method overloads with `string`.</span></span> <span data-ttu-id="cda77-192">Tato navrhovaná změna způsobí, že kompilátor po implementaci této sady funkcí vybere jiné přetížení.</span><span class="sxs-lookup"><span data-stu-id="cda77-192">This proposed change would cause the compiler to pick a different overload once this set of features was implemented.</span></span> 

<span data-ttu-id="cda77-193">Tato možnost se jeví jako rozumně nízká.</span><span class="sxs-lookup"><span data-stu-id="cda77-193">The possibility of this seems reasonably low.</span></span> <span data-ttu-id="cda77-194">Typ by potřeboval `System.ValueFormattableString` celé jméno a musí mít `static` metody s názvem `Create`.</span><span class="sxs-lookup"><span data-stu-id="cda77-194">The type would need the full name `System.ValueFormattableString` and it would need to have `static` methods named `Create`.</span></span> <span data-ttu-id="cda77-195">Vzhledem k tomu, že se vývojářům při definování libovolného typu v oboru názvů `System` důrazně nedoporučuje, toto přerušení se jeví jako rozumné ohrožení.</span><span class="sxs-lookup"><span data-stu-id="cda77-195">Given that developers are strongly discouraged from defining any type in the `System` namespace this break seems like a reasonable compromise.</span></span>

### <a name="expanding-to-more-types"></a><span data-ttu-id="cda77-196">Rozšíření na více typů</span><span class="sxs-lookup"><span data-stu-id="cda77-196">Expanding to more types</span></span>
<span data-ttu-id="cda77-197">Vzhledem k tomu, že jsme v oblasti, měli byste zvážit přidání `IList<T>`, `ICollection<T>` a `IReadOnlyList<T>` do sady kolekcí, pro které `params` podporuje.</span><span class="sxs-lookup"><span data-stu-id="cda77-197">Given we're in the area we should consider adding `IList<T>`, `ICollection<T>` and `IReadOnlyList<T>` to the set of collections for which `params` is supported.</span></span> <span data-ttu-id="cda77-198">V souvislosti s implementací bude v rámci této jiné práce vyhodnocena malá část.</span><span class="sxs-lookup"><span data-stu-id="cda77-198">In terms of implementation it will cost a small amount over the other work here.</span></span>

<span data-ttu-id="cda77-199">Správce logických disků se musí rozhodnout, jestli to bude mít komplikace v jazyce.</span><span class="sxs-lookup"><span data-stu-id="cda77-199">LDM needs to decide if the complication to the language is worth it though.</span></span> <span data-ttu-id="cda77-200">Přidání `IEnumerable<T>` odstraní velmi konkrétní bod tření.</span><span class="sxs-lookup"><span data-stu-id="cda77-200">The addition of `IEnumerable<T>` removes a very specific friction point.</span></span> <span data-ttu-id="cda77-201">Při nedostatku tohoto `params` řešení bylo při volání metody `params` nuceno přidělit `T[]` od `IEnumerable<T>`.</span><span class="sxs-lookup"><span data-stu-id="cda77-201">Lacking this `params` solution many customers were forced to allocate `T[]` from an `IEnumerable<T>` when calling a `params` method.</span></span> <span data-ttu-id="cda77-202">Přidání `IEnumerable<T>` tuto situaci opravuje.</span><span class="sxs-lookup"><span data-stu-id="cda77-202">The addition of `IEnumerable<T>` fixes this though.</span></span> <span data-ttu-id="cda77-203">V tuto chvíli není k dispozici žádný zvláštní bod dopravení.</span><span class="sxs-lookup"><span data-stu-id="cda77-203">There is no specific friction point that the other interfaces fix here.</span></span> 

## <a name="considerations"></a><span data-ttu-id="cda77-204">Požadavky</span><span class="sxs-lookup"><span data-stu-id="cda77-204">Considerations</span></span>

### <a name="variant2-and-variant3"></a><span data-ttu-id="cda77-205">Variant2 a Variant3</span><span class="sxs-lookup"><span data-stu-id="cda77-205">Variant2 and Variant3</span></span>
<span data-ttu-id="cda77-206">Tým CoreFX má také nepřidělení sady typů úložiště pro až tři argumenty `Variant`.</span><span class="sxs-lookup"><span data-stu-id="cda77-206">The CoreFX team also has a non-allocating set of storage types for up to three `Variant` arguments.</span></span> <span data-ttu-id="cda77-207">Jedná se o jeden `Variant``Variant2` a `Variant3`.</span><span class="sxs-lookup"><span data-stu-id="cda77-207">These are a single `Variant`, `Variant2` and `Variant3`.</span></span> <span data-ttu-id="cda77-208">Všechny mají dvojici metod pro získání bezplatné `Span<Variant>` mimo ně: `CreateSpan` a `KeepAlive`.</span><span class="sxs-lookup"><span data-stu-id="cda77-208">All have a pair of methods for getting an allocation free `Span<Variant>` off of them: `CreateSpan` and `KeepAlive`.</span></span> <span data-ttu-id="cda77-209">To znamená, že u `params Span<Variant>` až tří argumentů může být lokalita volání zcela rozdělená.</span><span class="sxs-lookup"><span data-stu-id="cda77-209">This means for a `params Span<Variant>` of up to three arguments the call site can be entirely allocation free.</span></span>

``` csharp
static class ZeroAllocation {
    static void Use(params Span<Variant> spans) {
        ...
    }

    static void Go() {
        Use("hello", "world");
    }
}
```

<span data-ttu-id="cda77-210">Metodu `Go` lze snížit na následující:</span><span class="sxs-lookup"><span data-stu-id="cda77-210">The `Go` method can be lowered to the following:</span></span>

``` csharp
static class ZeroAllocation {
    static void Go() {
        Variant2 _v;
        _v.Variant1 = new Variant("hello");
        _v.Variant2 = new Variant("word");
        Use(_v.CreateSpan());
        _v.KeepAlive();
    }
}
```

<span data-ttu-id="cda77-211">To vyžaduje hodně práce nad návrhem, aby bylo možné znovu použít `T[]` mezi voláními `params Span<T>`.</span><span class="sxs-lookup"><span data-stu-id="cda77-211">This requires very little work on top of the proposal to re-use `T[]` between `params Span<T>` calls.</span></span> <span data-ttu-id="cda77-212">Kompilátor už potřebuje ke správě dočasného za volání a vyčištění práce po (i když v jednom případě je pouze označení interního tempa jako bezplatné).</span><span class="sxs-lookup"><span data-stu-id="cda77-212">The compiler already needs to manage a temporary per call and do clean up work after (even if in one case it's just marking an internal temp as free).</span></span> 

<span data-ttu-id="cda77-213">Poznámka: funkce `KeepAlive` je nutná jenom na ploše.</span><span class="sxs-lookup"><span data-stu-id="cda77-213">Note: the `KeepAlive` function is only necessary on desktop.</span></span> <span data-ttu-id="cda77-214">V .NET Core nebude metoda k dispozici, a proto Kompilátor negeneruje volání.</span><span class="sxs-lookup"><span data-stu-id="cda77-214">On .NET Core the method will not be available and hence the compiler won't emit a call to it.</span></span>

### <a name="clr-stack-allocation-helpers"></a><span data-ttu-id="cda77-215">Pomocník pro přidělení zásobníku CLR</span><span class="sxs-lookup"><span data-stu-id="cda77-215">CLR stack allocation helpers</span></span>
<span data-ttu-id="cda77-216">Modul CLR pouze poskytuje [Nastavení localloc](https://docs.microsoft.com/en-us/dotnet/api/system.reflection.emit.opcodes.localloc?redirectedfrom=MSDN&view=netframework-4.7.2) k alokaci zásobníku sousedící paměti.</span><span class="sxs-lookup"><span data-stu-id="cda77-216">The CLR only provides only [localloc](https://docs.microsoft.com/en-us/dotnet/api/system.reflection.emit.opcodes.localloc?redirectedfrom=MSDN&view=netframework-4.7.2) for stack allocation of contiguous memory.</span></span> <span data-ttu-id="cda77-217">Tato instrukce je omezená, protože funguje jenom pro `unmanaged` typy.</span><span class="sxs-lookup"><span data-stu-id="cda77-217">This instruction is limited in that it only works for `unmanaged` types.</span></span> <span data-ttu-id="cda77-218">To znamená, že se nedá použít jako univerzální řešení pro efektivní přidělování záložního úložiště pro `params 
 Span<T>`.</span><span class="sxs-lookup"><span data-stu-id="cda77-218">This means it can't be used as a universal solution for efficiently allocating the backing storage for `params 
 Span<T>`.</span></span> 

<span data-ttu-id="cda77-219">Toto omezení není některými základními omezeními, ale místo toho více než artefaktem historie.</span><span class="sxs-lookup"><span data-stu-id="cda77-219">This limitation is not some fundamental restriction though but instead more an artifact of history.</span></span> <span data-ttu-id="cda77-220">Modul CLR se může rozhodnout přidat nové operační kódy a vnitřní objekty, které poskytují univerzální přidělování zásobníku.</span><span class="sxs-lookup"><span data-stu-id="cda77-220">The CLR could choose to add new op codes / intrinsics which provide universal stack allocation.</span></span> <span data-ttu-id="cda77-221">Ty pak můžete použít k přidělení záložního úložiště pro většinu `params Span<T>` volání.</span><span class="sxs-lookup"><span data-stu-id="cda77-221">These could then be used to allocate the backing storage for most `params Span<T>` calls.</span></span>

``` csharp
static class BetterAllocation {
    static void Use(params Span<string> spans) {
        ...
    }

    static void Go() {
        Use("hello", "world");
    }
}
```

<span data-ttu-id="cda77-222">Metodu `Go` lze snížit na následující:</span><span class="sxs-lookup"><span data-stu-id="cda77-222">The `Go` method can be lowered to the following:</span></span>

``` csharp
static class ZeroAllocation {
    static void Go() {
        Span<T> span = RuntimeIntrinsic.StackAlloc<string>(length: 2);
        span[0] = "hello";
        span[1] = "world";
        Use(span);
    }
}
```

<span data-ttu-id="cda77-223">I když je tento přístup pro haldu velmi efektivní, způsobuje dodatečné použití zásobníku.</span><span class="sxs-lookup"><span data-stu-id="cda77-223">While this approach is very heap efficient it does cause extra stack usage.</span></span> <span data-ttu-id="cda77-224">V algoritmu, který má hlubokou `params` využití, může to způsobit, že se `StackOverflowException` vygeneruje, kde by bylo úspěšné přidělení jednoduchého `T[]`.</span><span class="sxs-lookup"><span data-stu-id="cda77-224">In an algorithm which has a deep stack and lots of `params` usage it's possible this could cause a `StackOverflowException` to be generated where a simple `T[]` allocation would succeed.</span></span> 

<span data-ttu-id="cda77-225">Bohužel C# není nastaven pro typ analýzy mezi jednotlivými metodami, kde by mohla být kvalifikovaně rozhodnuto, zda má volání použít zásobník nebo přidělení haldy `params`.</span><span class="sxs-lookup"><span data-stu-id="cda77-225">Unfortunately C# is not set up for the type of inter-method analysis where it could make an educated determination of whether or not call should use stack or heap allocation of `params`.</span></span> <span data-ttu-id="cda77-226">Může skutečně zvážit pouze každé volání.</span><span class="sxs-lookup"><span data-stu-id="cda77-226">It can only really consider each call on its own.</span></span>

<span data-ttu-id="cda77-227">CLR je nejvhodnějším nastavením pro tento typ určení za běhu.</span><span class="sxs-lookup"><span data-stu-id="cda77-227">The CLR is best setup for making this type of determination at runtime.</span></span> <span data-ttu-id="cda77-228">Proto by byl modul runtime pravděpodobně pro alokaci univerzálního zásobníku k dispozici dvěma způsoby:</span><span class="sxs-lookup"><span data-stu-id="cda77-228">Hence we'd likely have the runtime provide two methods for universal stack allocation:</span></span>

1. <span data-ttu-id="cda77-229">`Span<T> StackAlloc<T>(int length)`: Toto má stejné chování a omezení `localloc` s tím rozdílem, že může pracovat na jakémkoli typu `T`.</span><span class="sxs-lookup"><span data-stu-id="cda77-229">`Span<T> StackAlloc<T>(int length)`: this has the same behaviors and limitations of `localloc` except it can work on any type `T`.</span></span> 
1. <span data-ttu-id="cda77-230">`Span<T> MaybeStackAlloc<T>(int length)`: Tento modul runtime se může rozhodnout implementovat pomocí zásobníku nebo přidělení haldy.</span><span class="sxs-lookup"><span data-stu-id="cda77-230">`Span<T> MaybeStackAlloc<T>(int length)`: this runtime can choose to implement this by doing a stack or heap allocation.</span></span> <span data-ttu-id="cda77-231">Modul runtime pak může použít kontext spuštění, ve kterém je volána k určení způsobu přidělení `Span<T>`.</span><span class="sxs-lookup"><span data-stu-id="cda77-231">The runtime can then use the execution context in which it's called to determine how the `Span<T>` is allocated.</span></span> <span data-ttu-id="cda77-232">Volající, ale bude ho vždycky nakládat, jako by byl přidělen zásobníku.</span><span class="sxs-lookup"><span data-stu-id="cda77-232">The caller though will always treat it as if it were stack allocated.</span></span>

<span data-ttu-id="cda77-233">U velmi jednoduchých případů, jako je například jeden až dva argumenty C# , může kompilátor vždy použít `StackAlloc<T>` variantu.</span><span class="sxs-lookup"><span data-stu-id="cda77-233">For very simple cases, like one to two arguments, the C# compiler could always use `StackAlloc<T>` variant.</span></span> <span data-ttu-id="cda77-234">Ve většině případů není pravděpodobně významně přispívat k vyčerpání zásobníku.</span><span class="sxs-lookup"><span data-stu-id="cda77-234">This is unlikely to significantly contribute to stack exhaustion in most cases.</span></span> <span data-ttu-id="cda77-235">Pro jiné případy může kompilátor zvolit použití `MaybeStackAlloc<T>` místo toho, aby modul runtime mohl volání provést.</span><span class="sxs-lookup"><span data-stu-id="cda77-235">For other cases the compiler could choose to use `MaybeStackAlloc<T>` instead and let the runtime make the call.</span></span>

<span data-ttu-id="cda77-236">Způsob, jakým zvolíme, bude nejspíš potřebovat hlubší šetření a zkoumání reálných aplikací.</span><span class="sxs-lookup"><span data-stu-id="cda77-236">How we choose will likely require a deeper investigation and examination of real world apps.</span></span> <span data-ttu-id="cda77-237">Ale pokud jsou tyto nové vnitřní funkce dostupné, poskytne vám tento typ flexibility.</span><span class="sxs-lookup"><span data-stu-id="cda77-237">But if these new intrinsics are available then it will give us this type of flexibility.</span></span>

### <a name="why-not-varargs"></a><span data-ttu-id="cda77-238">Proč neexistují VarArgs?</span><span class="sxs-lookup"><span data-stu-id="cda77-238">Why not varargs?</span></span> 
<span data-ttu-id="cda77-239">Existující funkce [VarArgs](https://docs.microsoft.com/en-us/cpp/windows/variable-argument-lists-dot-dot-dot-cpp-cli?view=vs-2017) byla považována za možné řešení.</span><span class="sxs-lookup"><span data-stu-id="cda77-239">The existing [varargs](https://docs.microsoft.com/en-us/cpp/windows/variable-argument-lists-dot-dot-dot-cpp-cli?view=vs-2017) feature was considered here as a possible solution.</span></span> <span data-ttu-id="cda77-240">Tato funkce se ale primárně používá pro C++scénáře/CLI a má známé otvory pro jiné scénáře.</span><span class="sxs-lookup"><span data-stu-id="cda77-240">This feature though is meant primarily for C++/CLI scenarios and has known holes for other scenarios.</span></span> <span data-ttu-id="cda77-241">Při přenosu do systému UNIX se navíc významně účtují významné náklady.</span><span class="sxs-lookup"><span data-stu-id="cda77-241">Additionally there is significant cost in porting this to Unix.</span></span> <span data-ttu-id="cda77-242">Proto se neviděl jako životaschopné řešení.</span><span class="sxs-lookup"><span data-stu-id="cda77-242">Hence it wasn't seen as a viable solution.</span></span>

## <a name="related-issues"></a><span data-ttu-id="cda77-243">Související problémy</span><span class="sxs-lookup"><span data-stu-id="cda77-243">Related Issues</span></span>
<span data-ttu-id="cda77-244">Tato specifikace se týká následujících problémů:</span><span class="sxs-lookup"><span data-stu-id="cda77-244">This spec is related to the following issues:</span></span> 

- https://github.com/dotnet/csharplang/issues/1757
- https://github.com/dotnet/csharplang/issues/179
- https://github.com/dotnet/corefxlab/pull/2595

