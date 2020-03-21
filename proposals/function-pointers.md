---
ms.openlocfilehash: 8bf3a18dc42e225e64bd3ccda2106aed89b421ed
ms.sourcegitcommit: 9aa177443b83116fe1be2ab28e2c7291947fe32d
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/21/2020
ms.locfileid: "80108386"
---
# <a name="function-pointers"></a><span data-ttu-id="63e1e-101">Ukazatele na funkce</span><span class="sxs-lookup"><span data-stu-id="63e1e-101">Function Pointers</span></span>

## <a name="summary"></a><span data-ttu-id="63e1e-102">Souhrn</span><span class="sxs-lookup"><span data-stu-id="63e1e-102">Summary</span></span>

<span data-ttu-id="63e1e-103">Tento návrh poskytuje jazykové konstrukce, které zveřejňují operační kódy IL, ke kterým momentálně nelze efektivně přistupovat, nebo C# vůbec, v dnešní době: `ldftn` a `calli`.</span><span class="sxs-lookup"><span data-stu-id="63e1e-103">This proposal provides language constructs that expose IL opcodes that cannot currently be accessed efficiently, or at all, in C# today: `ldftn` and `calli`.</span></span> <span data-ttu-id="63e1e-104">Tyto operační kódy IL můžou být důležité v kódu s vysokým výkonem a vývojáři potřebují účinný způsob, jak k nim přistupovat.</span><span class="sxs-lookup"><span data-stu-id="63e1e-104">These IL opcodes can be important in high performance code and developers need an efficient way to access them.</span></span>

## <a name="motivation"></a><span data-ttu-id="63e1e-105">Motivační</span><span class="sxs-lookup"><span data-stu-id="63e1e-105">Motivation</span></span>

<span data-ttu-id="63e1e-106">Motivace a pozadí této funkce jsou popsány v následujícím problému (jako je potenciální implementace funkce):</span><span class="sxs-lookup"><span data-stu-id="63e1e-106">The motivations and background for this feature are described in the following issue (as is a potential implementation of the feature):</span></span>

https://github.com/dotnet/csharplang/issues/191

<span data-ttu-id="63e1e-107">Toto je alternativní návrh návrhu pro [vnitřní objekty kompilátoru](https://github.com/dotnet/csharplang/blob/master/proposals/intrinsics.md) .</span><span class="sxs-lookup"><span data-stu-id="63e1e-107">This is an alternate design proposal to [compiler intrinsics](https://github.com/dotnet/csharplang/blob/master/proposals/intrinsics.md)</span></span>

## <a name="detailed-design"></a><span data-ttu-id="63e1e-108">Podrobný návrh</span><span class="sxs-lookup"><span data-stu-id="63e1e-108">Detailed Design</span></span>

### <a name="function-pointers"></a><span data-ttu-id="63e1e-109">Ukazatele na funkce</span><span class="sxs-lookup"><span data-stu-id="63e1e-109">Function pointers</span></span>

<span data-ttu-id="63e1e-110">Jazyk umožní deklaraci ukazatelů na funkce pomocí syntaxe `delegate*`.</span><span class="sxs-lookup"><span data-stu-id="63e1e-110">The language will allow for the declaration of function pointers using the `delegate*` syntax.</span></span> <span data-ttu-id="63e1e-111">Úplná syntaxe je podrobněji popsána v následující části, ale je určena pro použití syntaxe `Func` a `Action` deklarace typu.</span><span class="sxs-lookup"><span data-stu-id="63e1e-111">The full syntax is described in detail in the next section but it is meant to resemble the syntax used by `Func` and `Action` type declarations.</span></span>

``` csharp
unsafe class Example {
    void Example(Action<int> a, delegate*<int, void> f) {
        a(42);
        f(42);
    }
}
```

<span data-ttu-id="63e1e-112">Tyto typy jsou reprezentovány pomocí typu ukazatele na funkci, jak je uvedeno v ECMA-335.</span><span class="sxs-lookup"><span data-stu-id="63e1e-112">These types are represented using the function pointer type as outlined in ECMA-335.</span></span> <span data-ttu-id="63e1e-113">To znamená, že volání `delegate*` bude používat `calli`, kde vyvolání `delegate` bude používat `callvirt` na `Invoke` metodě.</span><span class="sxs-lookup"><span data-stu-id="63e1e-113">This means invocation of a `delegate*` will use `calli` where invocation of a `delegate` will use `callvirt` on the `Invoke` method.</span></span>
<span data-ttu-id="63e1e-114">Syntakticky, i když je volání identické pro obě konstrukce.</span><span class="sxs-lookup"><span data-stu-id="63e1e-114">Syntactically though invocation is identical for both constructs.</span></span>

<span data-ttu-id="63e1e-115">Definice těchto ukazatelů metody ECMA-335 zahrnuje konvenci volání jako součást signatury typu (oddíl 7,1).</span><span class="sxs-lookup"><span data-stu-id="63e1e-115">The ECMA-335 definition of method pointers includes the calling convention as part of the type signature (section 7.1).</span></span>
<span data-ttu-id="63e1e-116">Výchozí konvence volání bude `managed`.</span><span class="sxs-lookup"><span data-stu-id="63e1e-116">The default calling convention will be `managed`.</span></span> <span data-ttu-id="63e1e-117">Alternativní formuláře lze určit přidáním vhodného modifikátoru za `delegate*` syntaxe: `managed`, `cdecl`, `stdcall`, `thiscall`nebo `unmanaged`.</span><span class="sxs-lookup"><span data-stu-id="63e1e-117">Alternate forms can be specified by adding the appropriate modifier after the `delegate*` syntax: `managed`, `cdecl`, `stdcall`, `thiscall`, or `unmanaged`.</span></span> <span data-ttu-id="63e1e-118">Příklad:</span><span class="sxs-lookup"><span data-stu-id="63e1e-118">Example:</span></span>

``` csharp
// This method will be invoked using the cdecl calling convention
delegate* cdecl<int, int>;

// This method will be invoked using the stdcall calling convention
delegate* stdcall<int, int>;
```

<span data-ttu-id="63e1e-119">Převody mezi typy `delegate*` se provádí na základě jejich signatury, včetně konvence volání.</span><span class="sxs-lookup"><span data-stu-id="63e1e-119">Conversions between `delegate*` types is done based on their signature including the calling convention.</span></span>

``` csharp
unsafe class Example {
    void Conversions() {
        delegate*<int, int, int> p1 = ...;
        delegate* managed<int, int, int> p2 = ...;
        delegate* cdecl<int, int, int> p3 = ...;

        p1 = p2; // okay p1 and p2 have compatible signatures
        Console.WriteLine(p2 == p1); // True
        p2 = p3; // error: calling conventions are incompatible
    }
}
```

<span data-ttu-id="63e1e-120">Typ `delegate*` je typ ukazatele, což znamená, že má všechny možnosti a omezení standardního typu ukazatele:</span><span class="sxs-lookup"><span data-stu-id="63e1e-120">A `delegate*` type is a pointer type which means it has all of the capabilities and restrictions of a standard pointer type:</span></span>

- <span data-ttu-id="63e1e-121">Platné pouze v kontextu `unsafe`.</span><span class="sxs-lookup"><span data-stu-id="63e1e-121">Only valid in an `unsafe` context.</span></span>
- <span data-ttu-id="63e1e-122">Metody, které obsahují parametr `delegate*` nebo návratový typ, lze volat pouze z kontextu `unsafe`.</span><span class="sxs-lookup"><span data-stu-id="63e1e-122">Methods which contain a `delegate*` parameter or return type can only be called from an `unsafe` context.</span></span>
- <span data-ttu-id="63e1e-123">Nelze převést na `object`.</span><span class="sxs-lookup"><span data-stu-id="63e1e-123">Cannot be converted to `object`.</span></span>
- <span data-ttu-id="63e1e-124">Nelze použít jako obecný argument.</span><span class="sxs-lookup"><span data-stu-id="63e1e-124">Cannot be used as a generic argument.</span></span>
- <span data-ttu-id="63e1e-125">Může implicitně převést `delegate*` na `void*`.</span><span class="sxs-lookup"><span data-stu-id="63e1e-125">Can implicitly convert `delegate*` to `void*`.</span></span>
- <span data-ttu-id="63e1e-126">Lze explicitně převést z `void*` na `delegate*`.</span><span class="sxs-lookup"><span data-stu-id="63e1e-126">Can explicitly convert from `void*` to `delegate*`.</span></span>

<span data-ttu-id="63e1e-127">Podléhající</span><span class="sxs-lookup"><span data-stu-id="63e1e-127">Restrictions:</span></span>

- <span data-ttu-id="63e1e-128">Vlastní atributy nelze použít pro `delegate*` ani žádný z jejích elementů.</span><span class="sxs-lookup"><span data-stu-id="63e1e-128">Custom attributes cannot be applied to a `delegate*` or any of its elements.</span></span>
- <span data-ttu-id="63e1e-129">Parametr `delegate*` nelze označit jako `params`</span><span class="sxs-lookup"><span data-stu-id="63e1e-129">A `delegate*` parameter cannot be marked as `params`</span></span>
- <span data-ttu-id="63e1e-130">`delegate*` typ má všechna omezení normálního typu ukazatele.</span><span class="sxs-lookup"><span data-stu-id="63e1e-130">A `delegate*` type has all of the restrictions of a normal pointer type.</span></span>

### <a name="function-pointer-syntax"></a><span data-ttu-id="63e1e-131">Syntaxe ukazatele na funkci</span><span class="sxs-lookup"><span data-stu-id="63e1e-131">Function pointer syntax</span></span>

<span data-ttu-id="63e1e-132">Úplná syntaxe ukazatele na funkci je reprezentovaná následující gramatikou:</span><span class="sxs-lookup"><span data-stu-id="63e1e-132">The full function pointer syntax is represented by the following grammar:</span></span>

```antlr
pointer_type
    : ...
    | funcptr_type
    ;

funcptr_type
    : 'delegate' '*' calling_convention? '<' (funcptr_parameter_modifier? type ',')* funcptr_return_modifier? return_type '>'
    ;

calling_convention
    : 'cdecl'
    | 'managed'
    | 'stdcall'
    | 'thiscall'
    | 'unmanaged'
    ;

funcptr_parameter_modifier
    : 'ref'
    | 'out'
    | 'in'
    ;

funcptr_return_modifier
    : 'ref'
    | 'ref readonly'
    ;
```

<span data-ttu-id="63e1e-133">Konvence volání `unmanaged` představuje výchozí konvenci volání pro nativní kód na aktuální platformě a je kódována jako WINAPI.</span><span class="sxs-lookup"><span data-stu-id="63e1e-133">The `unmanaged` calling convention represents the default calling convention for native code on the current platform, and is encoded as winapi.</span></span>
<span data-ttu-id="63e1e-134">Všechny `calling_convention`s jsou kontextová klíčová slova, pokud předcházejí `delegate*`.</span><span class="sxs-lookup"><span data-stu-id="63e1e-134">All `calling_convention`s are contextual keywords when preceded by a `delegate*`.</span></span>

``` csharp
delegate int Func1(string s);
delegate Func1 Func2(Func1 f);

// Function pointer equivalent without calling convention
delegate*<string, int>;
delegate*<delegate*<string, int>, delegate*<string, int>>;

// Function pointer equivalent with calling convention
delegate* managed<string, int>;
delegate*<delegate* managed<string, int>, delegate*<string, int>>;
```

### <a name="function-pointer-conversions"></a><span data-ttu-id="63e1e-135">Převody ukazatelů na funkce</span><span class="sxs-lookup"><span data-stu-id="63e1e-135">Function pointer conversions</span></span>

<span data-ttu-id="63e1e-136">V nezabezpečeném kontextu je sada dostupných implicitních převodů (implicitní převody) rozšířena tak, aby zahrnovala následující implicitní převody ukazatelů:</span><span class="sxs-lookup"><span data-stu-id="63e1e-136">In an unsafe context, the set of available implicit conversions (Implicit conversions) is extended to include the following implicit pointer conversions:</span></span>
- [<span data-ttu-id="63e1e-137">_Existující převody_</span><span class="sxs-lookup"><span data-stu-id="63e1e-137">_Existing conversions_</span></span>](https://github.com/dotnet/csharplang/blob/master/spec/unsafe-code.md#pointer-conversions)
- <span data-ttu-id="63e1e-138">Z _funcptr\_typ_ `F0` na jiný _typ\_funcptr_ `F1`, pokud jsou splněné všechny následující podmínky:</span><span class="sxs-lookup"><span data-stu-id="63e1e-138">From _funcptr\_type_ `F0` to another _funcptr\_type_ `F1`, provided all of the following are true:</span></span>
    - <span data-ttu-id="63e1e-139">`F0` a `F1` mají stejný počet parametrů, přičemž každý parametr `D0n` v `F0` má stejné `ref`, `out`nebo `in` modifikátory jako odpovídající parametr `D1n` v `F1`.</span><span class="sxs-lookup"><span data-stu-id="63e1e-139">`F0` and `F1` have the same number of parameters, and each parameter `D0n` in `F0` has the same `ref`, `out`, or `in` modifiers as the corresponding parameter `D1n` in `F1`.</span></span>
    - <span data-ttu-id="63e1e-140">Pro každý parametr hodnoty (parametr bez `ref`, `out`nebo modifikátor `in`), konverze identity, implicitní převod odkazu nebo implicitní převod ukazatele existují z typu parametru v `F0` k odpovídajícímu typu parametru v `F1`.</span><span class="sxs-lookup"><span data-stu-id="63e1e-140">For each value parameter (a parameter with no `ref`, `out`, or `in` modifier), an identity conversion, implicit reference conversion, or implicit pointer conversion exists from the parameter type in `F0` to the corresponding parameter type in `F1`.</span></span>
    - <span data-ttu-id="63e1e-141">Pro každý `ref`, `out`nebo parametr `in` je typ parametru v `F0` stejný jako odpovídající typ parametru v `F1`.</span><span class="sxs-lookup"><span data-stu-id="63e1e-141">For each `ref`, `out`, or `in` parameter, the parameter type in `F0` is the same as the corresponding parameter type in `F1`.</span></span>
    - <span data-ttu-id="63e1e-142">Pokud je návratový typ podle hodnoty (No `ref` ani `ref readonly`), existuje identita, implicitní odkaz nebo implicitní převod ukazatele z návratového typu `F1` na návratový typ `F0`.</span><span class="sxs-lookup"><span data-stu-id="63e1e-142">If the return type is by value (no `ref` or `ref readonly`), an identity, implicit reference, or implicit pointer conversion exists from the return type of `F1` to the return type of `F0`.</span></span>
    - <span data-ttu-id="63e1e-143">Pokud je návratový typ odkazem (`ref` nebo `ref readonly`), návratový typ a modifikátory `ref` `F1` jsou stejné jako návratový typ a `ref` modifikátory `F0`.</span><span class="sxs-lookup"><span data-stu-id="63e1e-143">If the return type is by reference (`ref` or `ref readonly`), the return type and `ref` modifiers of `F1` are the same as the return type and `ref` modifiers of `F0`.</span></span>
    - <span data-ttu-id="63e1e-144">Konvence volání `F0` je stejná jako konvence volání `F1`.</span><span class="sxs-lookup"><span data-stu-id="63e1e-144">The calling convention of `F0` is the same as the calling convention of `F1`.</span></span>

### <a name="allow-address-of-to-target-methods"></a><span data-ttu-id="63e1e-145">Povolení adres pro cílové metody</span><span class="sxs-lookup"><span data-stu-id="63e1e-145">Allow address-of to target methods</span></span>

<span data-ttu-id="63e1e-146">Skupiny metod budou nyní povoleny jako argumenty pro výraz adresy.</span><span class="sxs-lookup"><span data-stu-id="63e1e-146">Method groups will now be allowed as arguments to an address-of expression.</span></span> <span data-ttu-id="63e1e-147">Typ takového výrazu bude `delegate*`, který má ekvivalentní signaturu cílové metody a spravované konvence volání:</span><span class="sxs-lookup"><span data-stu-id="63e1e-147">The type of such an expression will be a `delegate*` which has the equivalent signature of the target method and a managed calling convention:</span></span>

``` csharp
unsafe class Util {
    public static void Log() { }

    void Use() {
        delegate*<void> ptr1 = &Util.Log;

        // Error: type "delegate*<void>" not compatible with "delegate*<int>";
        delegate*<int> ptr2 = &Util.Log;

        // Okay. Conversion to void* is always allowed.
        void* v = &Util.Log;
   }
}
```

<span data-ttu-id="63e1e-148">V nezabezpečeném kontextu je metoda `M` kompatibilní s typem ukazatele funkce `F` Pokud jsou splněné všechny následující podmínky:</span><span class="sxs-lookup"><span data-stu-id="63e1e-148">In an unsafe context, a method `M` is compatible with a function pointer type `F` if all of the following are true:</span></span>
- <span data-ttu-id="63e1e-149">`M` a `F` mají stejný počet parametrů, přičemž každý parametr v `D` má jako odpovídající parametr v `out`stejné modifikátory `ref`, `in` nebo `F`.</span><span class="sxs-lookup"><span data-stu-id="63e1e-149">`M` and `F` have the same number of parameters, and each parameter in `D` has the same `ref`, `out`, or `in` modifiers as the corresponding parameter in `F`.</span></span>
- <span data-ttu-id="63e1e-150">Pro každý parametr hodnoty (parametr bez `ref`, `out`nebo modifikátor `in`), konverze identity, implicitní převod odkazu nebo implicitní převod ukazatele existují z typu parametru v `M` k odpovídajícímu typu parametru v `F`.</span><span class="sxs-lookup"><span data-stu-id="63e1e-150">For each value parameter (a parameter with no `ref`, `out`, or `in` modifier), an identity conversion, implicit reference conversion, or implicit pointer conversion exists from the parameter type in `M` to the corresponding parameter type in `F`.</span></span>
- <span data-ttu-id="63e1e-151">Pro každý `ref`, `out`nebo parametr `in` je typ parametru v `M` stejný jako odpovídající typ parametru v `F`.</span><span class="sxs-lookup"><span data-stu-id="63e1e-151">For each `ref`, `out`, or `in` parameter, the parameter type in `M` is the same as the corresponding parameter type in `F`.</span></span>
- <span data-ttu-id="63e1e-152">Pokud je návratový typ podle hodnoty (No `ref` ani `ref readonly`), existuje identita, implicitní odkaz nebo implicitní převod ukazatele z návratového typu `F` na návratový typ `M`.</span><span class="sxs-lookup"><span data-stu-id="63e1e-152">If the return type is by value (no `ref` or `ref readonly`), an identity, implicit reference, or implicit pointer conversion exists from the return type of `F` to the return type of `M`.</span></span>
- <span data-ttu-id="63e1e-153">Pokud je návratový typ odkazem (`ref` nebo `ref readonly`), návratový typ a modifikátory `ref` `F` jsou stejné jako návratový typ a `ref` modifikátory `M`.</span><span class="sxs-lookup"><span data-stu-id="63e1e-153">If the return type is by reference (`ref` or `ref readonly`), the return type and `ref` modifiers of `F` are the same as the return type and `ref` modifiers of `M`.</span></span>
- <span data-ttu-id="63e1e-154">Konvence volání `M` je stejná jako konvence volání `F`.</span><span class="sxs-lookup"><span data-stu-id="63e1e-154">The calling convention of `M` is the same as the calling convention of `F`.</span></span>
- <span data-ttu-id="63e1e-155">`M` je statická metoda.</span><span class="sxs-lookup"><span data-stu-id="63e1e-155">`M` is a static method.</span></span>

<span data-ttu-id="63e1e-156">V nezabezpečeném kontextu existuje implicitní převod z výrazu adresy, jehož cílem je skupina metod `E` na kompatibilní typ ukazatele na funkci `F` Pokud `E` obsahuje alespoň jednu metodu, která je použita v běžné formě na seznam argumentů konstruovaný pomocí typů parametrů a modifikátorů `F`, jak je popsáno v následujícím tématu.</span><span class="sxs-lookup"><span data-stu-id="63e1e-156">In an unsafe context, an implicit conversion exists from an address-of expression whose target is a method group `E` to a compatible function pointer type `F` if `E` contains at least one method that is applicable in its normal form to an argument list constructed by use of the parameter types and modifiers of `F`, as described in the following.</span></span>
- <span data-ttu-id="63e1e-157">`M` je vybrána jedna metoda, která odpovídá vyvolání metody `E(A)` formuláře s těmito úpravami:</span><span class="sxs-lookup"><span data-stu-id="63e1e-157">A single method `M` is selected corresponding to a method invocation of the form `E(A)` with the following modifications:</span></span>
   - <span data-ttu-id="63e1e-158">Seznam argumentů `A` je seznam výrazů, každý klasifikovaný jako proměnná a s typem a modifikátorem (`ref`, `out`nebo `in`) odpovídajícího _formálního\_parametru\_seznam_ `D`.</span><span class="sxs-lookup"><span data-stu-id="63e1e-158">The arguments list `A` is a list of expressions, each classified as a variable and with the type and modifier (`ref`, `out`, or `in`) of the corresponding _formal\_parameter\_list_ of `D`.</span></span>
   - <span data-ttu-id="63e1e-159">Kandidátské metody jsou pouze metody, které jsou použitelné v jejich normální podobě, nikoli ty, které platí v rozbaleném formuláři.</span><span class="sxs-lookup"><span data-stu-id="63e1e-159">The candidate methods are only those methods that are applicable in their normal form, not those applicable in their expanded form.</span></span>
   - <span data-ttu-id="63e1e-160">Kandidátské metody jsou pouze metody, které jsou statické.</span><span class="sxs-lookup"><span data-stu-id="63e1e-160">The candidate methods are only those methods that are static.</span></span>
- <span data-ttu-id="63e1e-161">Pokud algoritmus vyvolání metody vyvolá chybu, dojde k chybě při kompilaci.</span><span class="sxs-lookup"><span data-stu-id="63e1e-161">If the algorithm of Method invocations produces an error, then a compile-time error occurs.</span></span> <span data-ttu-id="63e1e-162">V opačném případě algoritmus vytvoří jednu nejlepší metodu `M` se stejným počtem parametrů jako `F` a převod je považován za existující.</span><span class="sxs-lookup"><span data-stu-id="63e1e-162">Otherwise, the algorithm produces a single best method `M` having the same number of parameters as `F` and the conversion is considered to exist.</span></span>
- <span data-ttu-id="63e1e-163">Vybraná metoda `M` musí být kompatibilní (jak je definováno výše) s typem ukazatele na funkci `F`.</span><span class="sxs-lookup"><span data-stu-id="63e1e-163">The selected method `M` must be compatible (as defined above) with the function pointer type `F`.</span></span> <span data-ttu-id="63e1e-164">V opačném případě dojde k chybě při kompilaci.</span><span class="sxs-lookup"><span data-stu-id="63e1e-164">Otherwise, a compile-time error occurs.</span></span>
- <span data-ttu-id="63e1e-165">Výsledkem převodu je ukazatel na funkci typu `F`.</span><span class="sxs-lookup"><span data-stu-id="63e1e-165">The result of the conversion is a function pointer of type `F`.</span></span>

<span data-ttu-id="63e1e-166">Implicitní převod existuje z výrazu adresy, jehož cílem je skupina metod `E` `void*`, pokud je v `E`jenom jedna statická metoda `M`.</span><span class="sxs-lookup"><span data-stu-id="63e1e-166">An implicit conversion exists from an address-of expression whose target is a method group `E` to `void*` if there is only one static method `M` in `E`.</span></span>
<span data-ttu-id="63e1e-167">Pokud existuje jedna statická metoda, pak je `M`jedinou nejlepší metodou z `E`.</span><span class="sxs-lookup"><span data-stu-id="63e1e-167">If there is one static method, then the single best method from `E` is `M`.</span></span>
<span data-ttu-id="63e1e-168">V opačném případě dojde k chybě při kompilaci.</span><span class="sxs-lookup"><span data-stu-id="63e1e-168">Otherwise, a compile-time error occurs.</span></span>

<span data-ttu-id="63e1e-169">To znamená, že vývojáři mohou záviset na pravidlech rozlišení přetížení pro práci ve spojení s operátorem adresy:</span><span class="sxs-lookup"><span data-stu-id="63e1e-169">This means developers can depend on overload resolution rules to work in conjunction with the address-of operator:</span></span>

``` csharp
unsafe class Util {
    public static void Log() { }
    public static void Log(string p1) { }
    public static void Log(int i) { };

    void Use() {
        delegate*<void> a1 = &Log; // Log()
        delegate*<int, void> a2 = &Log; // Log(int i)

        // Error: ambiguous conversion from method group Log to "void*"
        void* v = &Log;
    }
```

<span data-ttu-id="63e1e-170">Operátor address-of bude implementován pomocí instrukcí `ldftn`.</span><span class="sxs-lookup"><span data-stu-id="63e1e-170">The address-of operator will be implemented using the `ldftn` instruction.</span></span>

<span data-ttu-id="63e1e-171">Omezení této funkce:</span><span class="sxs-lookup"><span data-stu-id="63e1e-171">Restrictions of this feature:</span></span>

- <span data-ttu-id="63e1e-172">Platí pouze pro metody označené jako `static`.</span><span class="sxs-lookup"><span data-stu-id="63e1e-172">Only applies to methods marked as `static`.</span></span>
- <span data-ttu-id="63e1e-173">Místní funkce, které nejsou`static`, se nedají použít v `&`.</span><span class="sxs-lookup"><span data-stu-id="63e1e-173">Non-`static` local functions cannot be used in `&`.</span></span> <span data-ttu-id="63e1e-174">Podrobnosti implementace těchto metod jsou záměrně Nespecifikovány jazykem.</span><span class="sxs-lookup"><span data-stu-id="63e1e-174">The implementation details of these methods are deliberately not specified by the language.</span></span> <span data-ttu-id="63e1e-175">To zahrnuje to, jestli jsou statické vs. instance, nebo přesně to, s jakou signaturou se generují.</span><span class="sxs-lookup"><span data-stu-id="63e1e-175">This includes whether they are static vs. instance or exactly what signature they are emitted with.</span></span>

### <a name="better-function-member"></a><span data-ttu-id="63e1e-176">Lepší člen funkce</span><span class="sxs-lookup"><span data-stu-id="63e1e-176">Better function member</span></span>

<span data-ttu-id="63e1e-177">Specifikace členů lepší funkce se změní tak, aby zahrnovala následující řádek:</span><span class="sxs-lookup"><span data-stu-id="63e1e-177">The better function member specification will be changed to include the following line:</span></span>

> <span data-ttu-id="63e1e-178">`delegate*` je konkrétnější než `void*`</span><span class="sxs-lookup"><span data-stu-id="63e1e-178">A `delegate*` is more specific than `void*`</span></span>

<span data-ttu-id="63e1e-179">To znamená, že je možné přetížit na `void*` a `delegate*` a stále sensibly použít operátor address-of.</span><span class="sxs-lookup"><span data-stu-id="63e1e-179">This means that it is possible to overload on `void*` and a `delegate*` and still sensibly use the address-of operator.</span></span>

## <a name="open-issues"></a><span data-ttu-id="63e1e-180">Otevřené problémy</span><span class="sxs-lookup"><span data-stu-id="63e1e-180">Open Issues</span></span>

### <a name="nativecallableattribute"></a><span data-ttu-id="63e1e-181">NativeCallableAttribute</span><span class="sxs-lookup"><span data-stu-id="63e1e-181">NativeCallableAttribute</span></span>

<span data-ttu-id="63e1e-182">Toto je atribut používaný modulem CLR, aby při vyvolání nedocházelo ke spravovanému nativnímu prologu.</span><span class="sxs-lookup"><span data-stu-id="63e1e-182">This is an attribute used by the CLR to avoid the managed to native prologue when invoking.</span></span> <span data-ttu-id="63e1e-183">Metody označené tímto atributem lze volat pouze z nativního kódu, nikoli spravované (nemohou volat metody, vytvořit delegáta atd.).</span><span class="sxs-lookup"><span data-stu-id="63e1e-183">Methods marked by this attribute are only callable from native code, not managed (can’t call methods, create a delegate, etc …).</span></span> <span data-ttu-id="63e1e-184">Atribut není pro mscorlib specifický; modul runtime zpracuje všechny atributy s tímto názvem se stejnou sémantikou.</span><span class="sxs-lookup"><span data-stu-id="63e1e-184">The attribute is not special to mscorlib; the runtime will treat any attribute with this name with the same semantics.</span></span>

<span data-ttu-id="63e1e-185">Je možné, že modul runtime a jazyk společně spolupracují, aby to plně podporoval.</span><span class="sxs-lookup"><span data-stu-id="63e1e-185">It's possible for the runtime and language to work together to fully support this.</span></span> <span data-ttu-id="63e1e-186">Jazyk by se mohl rozvažovat za `static` členů s atributem `NativeCallable` jako `delegate*` se zadanou konvencí volání.</span><span class="sxs-lookup"><span data-stu-id="63e1e-186">The language could choose to treat address-of `static` members with a `NativeCallable` attribute as a `delegate*` with the specified calling convention.</span></span>

``` csharp
unsafe class NativeCallableExample {
    [NativeCallable(CallingConvention.CDecl)]
    static void CloseHandle(IntPtr p) => Marshal.FreeHGlobal(p);

    void Use() {
        delegate*<IntPtr, void> p1 = &CloseHandle; // Error: Invalid calling convention

        delegate* cdecl<IntPtr, void> p2 = &CloseHandle; // Okay
    }
}

```

<span data-ttu-id="63e1e-187">Dále by byl jazyk nejspíš chtít:</span><span class="sxs-lookup"><span data-stu-id="63e1e-187">Additionally the language would likely also want to:</span></span>

- <span data-ttu-id="63e1e-188">Označte jakákoli spravovaná volání metody označené pomocí `NativeCallable` jako chybu.</span><span class="sxs-lookup"><span data-stu-id="63e1e-188">Flag any managed calls to a method tagged with `NativeCallable` as an error.</span></span> <span data-ttu-id="63e1e-189">Vzhledem k tomu, že funkce nemůže být vyvolána ze spravovaného kódu, kompilátor by měl vývojářům zabránit v pokusu o takové vyvolání.</span><span class="sxs-lookup"><span data-stu-id="63e1e-189">Given the function can't be invoked from managed code the compiler should prevent developers from attempting such an invocation.</span></span>
- <span data-ttu-id="63e1e-190">Zabraňuje převodům skupin metod na `delegate`, když je metoda označena pomocí `NativeCallable`.</span><span class="sxs-lookup"><span data-stu-id="63e1e-190">Prevent method group conversions to `delegate` when the method is tagged with `NativeCallable`.</span></span>

<span data-ttu-id="63e1e-191">Tato není nutná k podpoře `NativeCallable` i když.</span><span class="sxs-lookup"><span data-stu-id="63e1e-191">This is not necessary to support `NativeCallable` though.</span></span> <span data-ttu-id="63e1e-192">Kompilátor může podporovat atribut `NativeCallable`, protože používá existující syntaxi.</span><span class="sxs-lookup"><span data-stu-id="63e1e-192">The compiler can support the `NativeCallable` attribute as is using the existing syntax.</span></span> <span data-ttu-id="63e1e-193">Aby bylo možné tento program přetypování na správný podpis `delegate*`, je třeba přetypovat na `void*`.</span><span class="sxs-lookup"><span data-stu-id="63e1e-193">The program would simply need to cast to `void*` before casting to the correct `delegate*` signature.</span></span> <span data-ttu-id="63e1e-194">To by nebylo co nejhorší než podpora dnes.</span><span class="sxs-lookup"><span data-stu-id="63e1e-194">That would be no worse than the support today.</span></span>

``` csharp
void* v = &CloseHandle;
delegate* cdecl<IntPtr, bool> f1 = (delegate* cdecl<IntPtr, bool>)v;
```

### <a name="extensible-set-of-unmanaged-calling-conventions"></a><span data-ttu-id="63e1e-195">Rozšiřitelná sada nespravovaných konvencí volání</span><span class="sxs-lookup"><span data-stu-id="63e1e-195">Extensible set of unmanaged calling conventions</span></span>

<span data-ttu-id="63e1e-196">Sada nespravovaných konvencí volání podporovaná aktuálními kódováními ECMA-335 je zastaralá.</span><span class="sxs-lookup"><span data-stu-id="63e1e-196">The set of unmanaged calling conventions supported by the current ECMA-335 encodings is outdated.</span></span> <span data-ttu-id="63e1e-197">Viděli jsme žádosti o přidání podpory pro více konvencí nespravovaného volání, například:</span><span class="sxs-lookup"><span data-stu-id="63e1e-197">We have seen requests to add support for more unmanaged calling conventions, for example:</span></span>

- <span data-ttu-id="63e1e-198">[vectorcall](https://docs.microsoft.com/cpp/cpp/vectorcall) https://github.com/dotnet/coreclr/issues/12120</span><span class="sxs-lookup"><span data-stu-id="63e1e-198">[vectorcall](https://docs.microsoft.com/cpp/cpp/vectorcall) https://github.com/dotnet/coreclr/issues/12120</span></span>
- <span data-ttu-id="63e1e-199">StdCall s explicitním tímto https://github.com/dotnet/coreclr/pull/23974#issuecomment-482991750</span><span class="sxs-lookup"><span data-stu-id="63e1e-199">StdCall with explicit this https://github.com/dotnet/coreclr/pull/23974#issuecomment-482991750</span></span>

<span data-ttu-id="63e1e-200">Návrh této funkce by měl v budoucnu umožňovat rozšiřování sady nespravovaných konvencí volání.</span><span class="sxs-lookup"><span data-stu-id="63e1e-200">The design of this feature should allow extending the set of unmanaged calling conventions as needed in future.</span></span> <span data-ttu-id="63e1e-201">Tyto problémy zahrnují omezené místo pro konvence volání pro kódování (12 z 16 hodnot, které jsou pořízeny v `IMAGE_CEE_CS_CALLCONV_MASK`) a počet míst, která je potřeba retušovat, aby bylo možné přidat novou konvenci volání.</span><span class="sxs-lookup"><span data-stu-id="63e1e-201">The problems include limited space for encoding calling conventions (12 out of 16 values are taken in `IMAGE_CEE_CS_CALLCONV_MASK`) and number of places that need to be touched in order to add a new calling convention.</span></span> <span data-ttu-id="63e1e-202">Potenciálním řešením je zavést nové kódování, které představuje konvenci volání pomocí [`System.Runtime.InteropServices.CallingConvention`](https://docs.microsoft.com/en-us/dotnet/api/system.runtime.interopservices.callingconvention) Enum.</span><span class="sxs-lookup"><span data-stu-id="63e1e-202">A potential solution is to introduce a new encoding that represents the calling convention using [`System.Runtime.InteropServices.CallingConvention`](https://docs.microsoft.com/en-us/dotnet/api/system.runtime.interopservices.callingconvention) enum.</span></span>

<span data-ttu-id="63e1e-203">Pro referenci https://github.com/llvm/llvm-project/blob/master/llvm/include/llvm/IR/CallingConv.h obsahuje seznam konvencí volání, které podporuje LLVM.</span><span class="sxs-lookup"><span data-stu-id="63e1e-203">For reference, https://github.com/llvm/llvm-project/blob/master/llvm/include/llvm/IR/CallingConv.h has the list of calling conventions supported by LLVM.</span></span> <span data-ttu-id="63e1e-204">I když je pravděpodobné, že .NET bude někdy potřebovat podporu všech těchto, ukazuje, že prostor konvencí volání je velmi bohatý.</span><span class="sxs-lookup"><span data-stu-id="63e1e-204">While it is unlikely that .NET will ever need to support all of them, it demonstrates that the space of calling conventions is very rich.</span></span>

## <a name="considerations"></a><span data-ttu-id="63e1e-205">Požadavky</span><span class="sxs-lookup"><span data-stu-id="63e1e-205">Considerations</span></span>

### <a name="allow-instance-methods"></a><span data-ttu-id="63e1e-206">Povolení metod instancí</span><span class="sxs-lookup"><span data-stu-id="63e1e-206">Allow instance methods</span></span>

<span data-ttu-id="63e1e-207">Návrh se dá rozšířit tak, aby podporoval metody instance, využitím `EXPLICITTHIS` konvence volání CLI (s názvem `instance` C# v kódu).</span><span class="sxs-lookup"><span data-stu-id="63e1e-207">The proposal could be extended to support instance methods by taking advantage of the `EXPLICITTHIS` CLI calling convention (named `instance` in C# code).</span></span> <span data-ttu-id="63e1e-208">Tato forma ukazatelů na funkce rozhraní příkazového řádku vloží parametr `this` jako explicitní první parametr syntaxe ukazatele na funkci.</span><span class="sxs-lookup"><span data-stu-id="63e1e-208">This form of CLI function pointers puts the `this` parameter as an explicit first parameter of the function pointer syntax.</span></span>

``` csharp
unsafe class Instance {
    void Use() {
        delegate* instance<Instance, string> f = &ToString;
        f(this);
    }
}
```

<span data-ttu-id="63e1e-209">To je zvuk, ale přidává k návrhu nějaké komplikace.</span><span class="sxs-lookup"><span data-stu-id="63e1e-209">This is sound but adds some complication to the proposal.</span></span> <span data-ttu-id="63e1e-210">Zejména vzhledem k tomu, že ukazatele na funkce, které se liší konvencí volání `instance` a `managed` by byly nekompatibilní, i když jsou oba případy použity k C# vyvolání spravovaných metod se stejnou signaturou.</span><span class="sxs-lookup"><span data-stu-id="63e1e-210">Particularly because function pointers which differed by the calling convention `instance` and `managed` would be incompatible even though both cases are used to invoke managed methods with the same C# signature.</span></span> <span data-ttu-id="63e1e-211">V každém případě se v každém případě považuje za užitečné, aby existovala jednoduchá práce: použijte `static` místní funkci.</span><span class="sxs-lookup"><span data-stu-id="63e1e-211">Also in every case considered where this would be valuable to have there was a simple work around: use a `static` local function.</span></span>

``` csharp
unsafe class Instance {
    void Use() {
        static string toString(Instance i) = i.ToString();
        delgate*<Instance, string> f = &toString;
        f(this);
    }
}
```

### <a name="dont-require-unsafe-at-declaration"></a><span data-ttu-id="63e1e-212">Nevyžadovat bezpečné v deklaraci</span><span class="sxs-lookup"><span data-stu-id="63e1e-212">Don't require unsafe at declaration</span></span>

<span data-ttu-id="63e1e-213">Místo vyžadování `unsafe` při každém použití `delegate*`ji vyžadovat pouze v místě, kde je skupina metod převedena na `delegate*`.</span><span class="sxs-lookup"><span data-stu-id="63e1e-213">Instead of requiring `unsafe` at every use of a `delegate*`, only require it at the point where a method group is converted to a `delegate*`.</span></span> <span data-ttu-id="63e1e-214">Zde jsou uvedené základní bezpečnostní problémy, které jsou v provozu (s vědomím, že toto sestavení nelze uvolnit, pokud je hodnota aktivní).</span><span class="sxs-lookup"><span data-stu-id="63e1e-214">This is where the core safety issues come into play (knowing that the containing assembly cannot be unloaded while the value is alive).</span></span> <span data-ttu-id="63e1e-215">Vyžadování `unsafe` v jiných umístěních je možné zobrazit jako nadměrné.</span><span class="sxs-lookup"><span data-stu-id="63e1e-215">Requiring `unsafe` on the other locations can be seen as excessive.</span></span>

<span data-ttu-id="63e1e-216">Tímto způsobem byl návrh původně zamýšlen.</span><span class="sxs-lookup"><span data-stu-id="63e1e-216">This is how the design was originally intended.</span></span> <span data-ttu-id="63e1e-217">Ale výsledná jazyková pravidla jsou velmi neužitečná.</span><span class="sxs-lookup"><span data-stu-id="63e1e-217">But the resulting language rules felt very awkward.</span></span> <span data-ttu-id="63e1e-218">Není možné skrýt fakt, že se jedná o hodnotu ukazatele a že se uchovává i bez klíčového slova `unsafe`.</span><span class="sxs-lookup"><span data-stu-id="63e1e-218">It's impossible to hide the fact that this is a pointer value and it kept peeking through even without the `unsafe` keyword.</span></span> <span data-ttu-id="63e1e-219">Například převod na `object` nelze povolit, nemůže být členem `class`atd... C# Návrh je vyžadován `unsafe` pro všechny použití ukazatelů, a proto tento návrh následuje.</span><span class="sxs-lookup"><span data-stu-id="63e1e-219">For example the conversion to `object` can't be allowed, it can't be a member of a `class`, etc ... The C# design is to require `unsafe` for all pointer uses and hence this design follows that.</span></span>

<span data-ttu-id="63e1e-220">Vývojáři budou mít stále možnost předcházet _zabezpečenou_ obálku nad `delegate*`mi hodnotami stejným způsobem jako v dnešní době pro normální typy ukazatelů.</span><span class="sxs-lookup"><span data-stu-id="63e1e-220">Developers will still be capable of presenting a _safe_ wrapper on top of `delegate*` values the same way that they do for normal pointer types today.</span></span> <span data-ttu-id="63e1e-221">Vezměte v úvahu:</span><span class="sxs-lookup"><span data-stu-id="63e1e-221">Consider:</span></span>

``` csharp
unsafe struct Action {
    delegate*<void> _ptr;

    Action(delegate*<void> ptr) => _ptr = ptr;
    public void Invoke() => _ptr();
}
```

### <a name="using-delegates"></a><span data-ttu-id="63e1e-222">Použití delegátů</span><span class="sxs-lookup"><span data-stu-id="63e1e-222">Using delegates</span></span>

<span data-ttu-id="63e1e-223">Místo použití nového prvku syntaxe `delegate*`jednoduše použijte existující typy `delegate` s `*` za typ:</span><span class="sxs-lookup"><span data-stu-id="63e1e-223">Instead of using a new syntax element, `delegate*`, simply use existing `delegate` types with a `*` following the type:</span></span>

``` csharp
Func<object, object, bool>* ptr = &object.ReferenceEquals;
```

<span data-ttu-id="63e1e-224">Zpracování konvence volání lze provést zadáním poznámky k `delegate` typů s atributem, který určuje `CallingConvention` hodnotu.</span><span class="sxs-lookup"><span data-stu-id="63e1e-224">Handling calling convention can be done by annotating the `delegate` types with an attribute that specifies a `CallingConvention` value.</span></span> <span data-ttu-id="63e1e-225">Chybějící atribut by znamenal spravovanou konvenci volání.</span><span class="sxs-lookup"><span data-stu-id="63e1e-225">The lack of an attribute would signify the managed calling convention.</span></span>

<span data-ttu-id="63e1e-226">Kódování tohoto v IL je problematické.</span><span class="sxs-lookup"><span data-stu-id="63e1e-226">Encoding this in IL is problematic.</span></span> <span data-ttu-id="63e1e-227">Nadřazená hodnota musí být reprezentovaná jako ukazatel, ale také musí:</span><span class="sxs-lookup"><span data-stu-id="63e1e-227">The underlying value needs to be represented as a pointer yet it also must:</span></span>

1. <span data-ttu-id="63e1e-228">Mají jedinečný typ pro povolení přetížení s různými typy ukazatelů na funkce.</span><span class="sxs-lookup"><span data-stu-id="63e1e-228">Have a unique type to allow for overloads with different function pointer types.</span></span>
1. <span data-ttu-id="63e1e-229">Být ekvivalentní pro účely OHI napříč hranicemi sestavení.</span><span class="sxs-lookup"><span data-stu-id="63e1e-229">Be equivalent for OHI purposes across assembly boundaries.</span></span>

<span data-ttu-id="63e1e-230">Poslední bod je zvláště problematický.</span><span class="sxs-lookup"><span data-stu-id="63e1e-230">The last point is particularly problematic.</span></span> <span data-ttu-id="63e1e-231">To znamená, že každé sestavení, které používá `Func<int>*` musí kódovat ekvivalentní typ v metadatech, přestože `Func<int>*` je definováno v sestavení, ale neřídí.</span><span class="sxs-lookup"><span data-stu-id="63e1e-231">This mean that every assembly which uses `Func<int>*` must encode an equivalent type in metadata even though `Func<int>*` is defined in an assembly though don't control.</span></span>
<span data-ttu-id="63e1e-232">Navíc jakýkoli jiný typ, který je definován s názvem `System.Func<T>` v sestavení, které není mscorlib, musí být jiný než verze definovaná v mscorlib.</span><span class="sxs-lookup"><span data-stu-id="63e1e-232">Additionally any other type which is defined with the name `System.Func<T>` in an assembly that is not mscorlib must be different than the version defined in mscorlib.</span></span>

<span data-ttu-id="63e1e-233">Jedna z možností, které se prozkoumala, byl takový ukazatel, který se vyvolal jako `mod_req(Func<int>) void*`.</span><span class="sxs-lookup"><span data-stu-id="63e1e-233">One option that was explored was emitting such a pointer as `mod_req(Func<int>) void*`.</span></span> <span data-ttu-id="63e1e-234">Tato činnost nefunguje, i když `mod_req` nemůže vytvořit vazby k `TypeSpec` a tudíž nemůže cílit na obecné vytváření instancí.</span><span class="sxs-lookup"><span data-stu-id="63e1e-234">This doesn't work though as a `mod_req` cannot bind to a `TypeSpec` and hence cannot target generic instantiations.</span></span>

### <a name="named-function-pointers"></a><span data-ttu-id="63e1e-235">Pojmenované ukazatele funkcí</span><span class="sxs-lookup"><span data-stu-id="63e1e-235">Named function pointers</span></span>

<span data-ttu-id="63e1e-236">Syntaxe ukazatele na funkci může být náročná, zejména ve složitých případech, jako jsou vnořené ukazatele na funkce.</span><span class="sxs-lookup"><span data-stu-id="63e1e-236">The function pointer syntax can be cumbersome, particularly in complex cases like nested function pointers.</span></span> <span data-ttu-id="63e1e-237">Namísto toho, aby vývojáři nastavili podpis pokaždé, když jazyk může pojmenovat deklarace ukazatelů na funkce, jak je provedeno `delegate`.</span><span class="sxs-lookup"><span data-stu-id="63e1e-237">Rather than have developers type out the signature every time the language could allow for named declarations of function pointers as is done with `delegate`.</span></span>

``` csharp
func* void Action();

unsafe class NamedExample {
    void M(Action a) {
        a();
    }
}
```

<span data-ttu-id="63e1e-238">Zde je uvedená část problému. základní primitivní rozhraní CLI nemá názvy, proto by byl čistě C# vynálezný a vyžaduje, aby bylo možné povolit bitovou kopii metadat.</span><span class="sxs-lookup"><span data-stu-id="63e1e-238">Part of the problem here is the underlying CLI primitive doesn't have names hence this would be purely a C# invention and require a bit of metadata work to enable.</span></span> <span data-ttu-id="63e1e-239">To je doable, ale jedná se o důležitou práci.</span><span class="sxs-lookup"><span data-stu-id="63e1e-239">That is doable but is a significant about of work.</span></span> <span data-ttu-id="63e1e-240">V podstatě je potřeba C# , aby měl k typu def tabulky čistě pro tyto názvy.</span><span class="sxs-lookup"><span data-stu-id="63e1e-240">It essentially requires C# to have a companion to the type def table purely for these names.</span></span>

<span data-ttu-id="63e1e-241">I když byly prověřeny argumenty pojmenovaných funkcí, které jsme zjistili, že by mohly být stejné i pro řadu dalších scénářů.</span><span class="sxs-lookup"><span data-stu-id="63e1e-241">Also when the arguments for named function pointers were examined we found they could apply equally well to a number of other scenarios.</span></span> <span data-ttu-id="63e1e-242">Například by bylo vhodné deklarovat pojmenované řazené kolekce členů a snížit tak nutnost zadávat úplný podpis ve všech případech.</span><span class="sxs-lookup"><span data-stu-id="63e1e-242">For example it would be just as convenient to declare named tuples to reduce the need to type out the full signature in all cases.</span></span>

``` csharp
(int x, int y) Point;

class NamedTupleExample {
    void M(Point p) {
        Console.WriteLine(p.x);
    }
}
```

<span data-ttu-id="63e1e-243">Po diskuzi se rozhodli, že nepovolíte pojmenovanou deklaraci `delegate*`ch typů.</span><span class="sxs-lookup"><span data-stu-id="63e1e-243">After discussion we decided to not allow named declaration of `delegate*` types.</span></span> <span data-ttu-id="63e1e-244">Pokud zjistíme, že je potřeba na základě zpětné vazby na používání ze zákaznického oddělení, prozkoumáme řešení pojmenování, které funguje pro ukazatele na funkce, řazené kolekce členů, obecné typy atd... To je pravděpodobně podobné jako u jiných návrhů, jako je plná podpora `typedef` v jazyce.</span><span class="sxs-lookup"><span data-stu-id="63e1e-244">If we find there is significant need for this based on customer usage feedback then we will investigate a naming solution that works for function pointers, tuples, generics, etc ... This is likely to be similar in form to other suggestions like full `typedef` support in the language.</span></span>

## <a name="future-considerations"></a><span data-ttu-id="63e1e-245">Budoucí požadavky</span><span class="sxs-lookup"><span data-stu-id="63e1e-245">Future Considerations</span></span>

### <a name="static-local-functions"></a><span data-ttu-id="63e1e-246">Statické místní funkce</span><span class="sxs-lookup"><span data-stu-id="63e1e-246">static local functions</span></span>

<span data-ttu-id="63e1e-247">To odkazuje na [Návrh](https://github.com/dotnet/csharplang/issues/1565) , který povolí modifikátor `static` na místních funkcích.</span><span class="sxs-lookup"><span data-stu-id="63e1e-247">This refers to [the proposal](https://github.com/dotnet/csharplang/issues/1565) to allow the `static` modifier on local functions.</span></span> <span data-ttu-id="63e1e-248">Tato funkce by měla být zaručená, že se bude generovat jako `static` a s přesným podpisem zadaným ve zdrojovém kódu.</span><span class="sxs-lookup"><span data-stu-id="63e1e-248">Such a function would be guaranteed to be emitted as `static` and with the exact signature specified in source code.</span></span> <span data-ttu-id="63e1e-249">Tato funkce by měla být platným argumentem pro `&`, protože neobsahuje žádné z problémů, které místní funkce mají dnes</span><span class="sxs-lookup"><span data-stu-id="63e1e-249">Such a function should be a valid argument to `&` as it contains none of the problems local functions have today</span></span>

### <a name="static-delegates"></a><span data-ttu-id="63e1e-250">statické Delegáti</span><span class="sxs-lookup"><span data-stu-id="63e1e-250">static delegates</span></span>

<span data-ttu-id="63e1e-251">To odkazuje na [Návrh](https://github.com/dotnet/csharplang/issues/302) , který umožňuje deklaraci `delegate` typů, které mohou odkazovat pouze na `static` členy.</span><span class="sxs-lookup"><span data-stu-id="63e1e-251">This refers to [the proposal](https://github.com/dotnet/csharplang/issues/302) to allow for the declaration of `delegate` types which can only refer to `static` members.</span></span> <span data-ttu-id="63e1e-252">Výhodou toho, že tyto instance `delegate` můžou být ve scénářích citlivých na výkon a lepší přidělit.</span><span class="sxs-lookup"><span data-stu-id="63e1e-252">The advantage being that such `delegate` instances can be allocation free and better in performance sensitive scenarios.</span></span>

<span data-ttu-id="63e1e-253">Pokud je funkce ukazatele na funkci implementována, `static delegate` návrh pravděpodobně bude zavřen. Navrhovanou výhodou této funkce je přidělení volného místa.</span><span class="sxs-lookup"><span data-stu-id="63e1e-253">If the function pointer feature is implemented the `static delegate` proposal will likely be closed out. The proposed advantage of that feature is the allocation free nature.</span></span> <span data-ttu-id="63e1e-254">Nedávno se ale zjistilo, že v důsledku uvolňování sestavení není možné dosáhnout jejich posledního šetření.</span><span class="sxs-lookup"><span data-stu-id="63e1e-254">However recent investigations have found that is not possible to achieve due to assembly unloading.</span></span> <span data-ttu-id="63e1e-255">Musí existovat silný popisovač od `static delegate` k metodě, na kterou odkazuje, aby bylo sestavení uvolněno z něj.</span><span class="sxs-lookup"><span data-stu-id="63e1e-255">There must be a strong handle from the `static delegate` to the method it refers to in order to keep the assembly from being unloaded out from under it.</span></span>

<span data-ttu-id="63e1e-256">Aby bylo možné zachovat všechny `static delegate` instance, bude nutné přidělit novému popisovači, který spustí čítač pro cíle návrhu.</span><span class="sxs-lookup"><span data-stu-id="63e1e-256">To maintain every `static delegate` instance would be required to allocate a new handle which runs counter to the goals of the proposal.</span></span> <span data-ttu-id="63e1e-257">Existovaly některé návrhy, u kterých by bylo možné přidělení vytvořit v rámci jednoho přidělení na pracovišti, ale to bylo složité a nemohlo by to působit za obchod.</span><span class="sxs-lookup"><span data-stu-id="63e1e-257">There were some designs where the allocation could be amortized to a single allocation per call-site but that was a bit complex and didn't seem worth the trade off.</span></span>

<span data-ttu-id="63e1e-258">To znamená, že se vývojářům v podstatě musí rozhodnout mezi těmito kompromisy:</span><span class="sxs-lookup"><span data-stu-id="63e1e-258">That means developers essentially have to decide between the following trade offs:</span></span>

1. <span data-ttu-id="63e1e-259">Bezpečnost na tváři uvolnění sestavení: to vyžaduje přidělení, takže `delegate` je už dostatečná možnost.</span><span class="sxs-lookup"><span data-stu-id="63e1e-259">Safety in the face of assembly unloading: this requires allocations and hence `delegate` is already a sufficient option.</span></span>
1. <span data-ttu-id="63e1e-260">Žádná bezpečnost při uvolňování sestavení: použijte `delegate*`.</span><span class="sxs-lookup"><span data-stu-id="63e1e-260">No safety in face of assembly unloading: use a `delegate*`.</span></span> <span data-ttu-id="63e1e-261">To lze zabalit do `struct`, aby bylo možné použití mimo `unsafe` kontext ve zbytku kódu.</span><span class="sxs-lookup"><span data-stu-id="63e1e-261">This can be wrapped in a `struct` to allow usage outside an `unsafe` context in the rest of the code.</span></span>
