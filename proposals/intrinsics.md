---
ms.openlocfilehash: ac4c8760e3b6a0934f01ae634f666af60aa1c0fe
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484564"
---
# <a name="compiler-intrinsics"></a><span data-ttu-id="2cfa3-101">Vnitřní funkce kompilátoru</span><span class="sxs-lookup"><span data-stu-id="2cfa3-101">Compiler Intrinsics</span></span>

## <a name="summary"></a><span data-ttu-id="2cfa3-102">Souhrn</span><span class="sxs-lookup"><span data-stu-id="2cfa3-102">Summary</span></span>

<span data-ttu-id="2cfa3-103">Tento návrh poskytuje jazykové konstrukce, které zveřejňují operační kódy IL nízké úrovně, které momentálně nelze efektivně přistupovat, nebo vůbec: `ldftn`, `ldvirtftn`, `ldtoken` a `calli`.</span><span class="sxs-lookup"><span data-stu-id="2cfa3-103">This proposal provides language constructs that expose low level IL opcodes that cannot currently be accessed efficiently, or at all: `ldftn`, `ldvirtftn`, `ldtoken` and `calli`.</span></span> <span data-ttu-id="2cfa3-104">Tyto operační kódy nízké úrovně můžou být důležité v kódu s vysokým výkonem a vývojáři potřebují účinný způsob, jak k nim přistupovat.</span><span class="sxs-lookup"><span data-stu-id="2cfa3-104">These low level opcodes can be important in high performance code and developers need an efficient way to access them.</span></span>

## <a name="motivation"></a><span data-ttu-id="2cfa3-105">Motivační</span><span class="sxs-lookup"><span data-stu-id="2cfa3-105">Motivation</span></span>

<span data-ttu-id="2cfa3-106">Motivace a pozadí této funkce jsou popsány v následujícím problému (jako je potenciální implementace funkce):</span><span class="sxs-lookup"><span data-stu-id="2cfa3-106">The motivations and background for this feature are described in the following issue (as is a potential implementation of the feature):</span></span> 

https://github.com/dotnet/csharplang/issues/191

<span data-ttu-id="2cfa3-107">Tento alternativní návrh návrhu se zobrazí po kontrole implementace prototypu původního návrhu @msjabby a také při použití v rámci významné základní znakové sady.</span><span class="sxs-lookup"><span data-stu-id="2cfa3-107">This alternate design proposal comes after reviewing a prototype implementation of the original proposal by @msjabby as well as the use throughout a significant code base.</span></span> <span data-ttu-id="2cfa3-108">Tento návrh byl proveden se značným vstupem z @mjsabby, @tmat a @jkotas.</span><span class="sxs-lookup"><span data-stu-id="2cfa3-108">This design was done with significant input from @mjsabby, @tmat and @jkotas.</span></span>

## <a name="detailed-design"></a><span data-ttu-id="2cfa3-109">Podrobný návrh</span><span class="sxs-lookup"><span data-stu-id="2cfa3-109">Detailed Design</span></span> 

### <a name="allow-address-of-to-target-methods"></a><span data-ttu-id="2cfa3-110">Povolení adresy pro cílové metody</span><span class="sxs-lookup"><span data-stu-id="2cfa3-110">Allow address of to target methods</span></span>

<span data-ttu-id="2cfa3-111">Skupiny metod budou nyní povoleny jako argumenty pro výraz adresy.</span><span class="sxs-lookup"><span data-stu-id="2cfa3-111">Method groups will now be allowed as arguments to an address-of expression.</span></span> <span data-ttu-id="2cfa3-112">Typ takového výrazu bude `void*`.</span><span class="sxs-lookup"><span data-stu-id="2cfa3-112">The type of such an expression will be `void*`.</span></span> 

``` csharp
class Util { 
    public static void Log() { } 
}

// ldftn Util.Log
void* ptr = &Util.Log; 
```

<span data-ttu-id="2cfa3-113">V tomto případě neexistuje žádný převod delegáta. jediným mechanismem pro filtrování členů ve skupině metod je statický přístup k instanci.</span><span class="sxs-lookup"><span data-stu-id="2cfa3-113">Given there is no delegate conversion here the only mechanism for filtering members in the method group is by static / instance access.</span></span> <span data-ttu-id="2cfa3-114">Pokud to nemůže rozlišovat členy, dojde k chybě při kompilaci.</span><span class="sxs-lookup"><span data-stu-id="2cfa3-114">If that cannot distinguish the members then a compile time error will occur.</span></span>

``` csharp
class Util { 
    public void Log() { } 
    public void Log(string p1) { } 
    public static void Log(int i) { };
}

unsafe {
    // Error: Method group Log has more than one applicable candidate.
    void* ptr1 = &Log; 

    // Okay: only one static member to consider here.
    void* ptr2 = &Util.Log;
}
```

<span data-ttu-id="2cfa3-115">Výraz AddressOf v tomto kontextu bude implementován následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="2cfa3-115">The addressof expression in this context will be implemented in the following manner:</span></span>

- <span data-ttu-id="2cfa3-116">ldftn: Pokud je metoda nevirtuální.</span><span class="sxs-lookup"><span data-stu-id="2cfa3-116">ldftn: when the method is non-virtual.</span></span>
- <span data-ttu-id="2cfa3-117">ldvirtftn: když je metoda virtuální.</span><span class="sxs-lookup"><span data-stu-id="2cfa3-117">ldvirtftn: when the method is virtual.</span></span>

<span data-ttu-id="2cfa3-118">Omezení této funkce:</span><span class="sxs-lookup"><span data-stu-id="2cfa3-118">Restrictions of this feature:</span></span>

- <span data-ttu-id="2cfa3-119">Metody instance se dají zadat jenom při použití výrazu vyvolání pro hodnotu.</span><span class="sxs-lookup"><span data-stu-id="2cfa3-119">Instance methods can only be specified when using an invocation expression on a value</span></span>
- <span data-ttu-id="2cfa3-120">V `&`nelze použít místní funkce.</span><span class="sxs-lookup"><span data-stu-id="2cfa3-120">Local functions cannot be used in `&`.</span></span> <span data-ttu-id="2cfa3-121">Podrobnosti implementace těchto metod jsou záměrně Nespecifikovány jazykem.</span><span class="sxs-lookup"><span data-stu-id="2cfa3-121">The implementation details of these methods are deliberately not specified by the language.</span></span> <span data-ttu-id="2cfa3-122">To zahrnuje to, jestli jsou statické vs. instance, nebo přesně to, s jakou signaturou se generují.</span><span class="sxs-lookup"><span data-stu-id="2cfa3-122">This includes whether they are static vs. instance or exactly what signature they are emitted with.</span></span>

### <a name="handleof"></a><span data-ttu-id="2cfa3-123">handleof</span><span class="sxs-lookup"><span data-stu-id="2cfa3-123">handleof</span></span>

<span data-ttu-id="2cfa3-124">Kontextové klíčové slovo `handleof` převede pole, člen nebo typ do odpovídajícího typu `RuntimeHandle` pomocí instrukce `ldtoken`.</span><span class="sxs-lookup"><span data-stu-id="2cfa3-124">The `handleof` contextual keyword will translate a field, member or type into their equivalent `RuntimeHandle` type using the `ldtoken` instruction.</span></span> <span data-ttu-id="2cfa3-125">Přesný typ výrazu bude záviset na typu názvu v `handleof`:</span><span class="sxs-lookup"><span data-stu-id="2cfa3-125">The exact type of the expression will depend on the kind of the name in `handleof`:</span></span>

- <span data-ttu-id="2cfa3-126">pole: `RuntimeFieldHandle`</span><span class="sxs-lookup"><span data-stu-id="2cfa3-126">field: `RuntimeFieldHandle`</span></span>
- <span data-ttu-id="2cfa3-127">Zadejte: `RuntimeTypeHandle`</span><span class="sxs-lookup"><span data-stu-id="2cfa3-127">type: `RuntimeTypeHandle`</span></span>
- <span data-ttu-id="2cfa3-128">Metoda: `RuntimeMethodHandle`</span><span class="sxs-lookup"><span data-stu-id="2cfa3-128">method: `RuntimeMethodHandle`</span></span>

<span data-ttu-id="2cfa3-129">Argumenty `handleof` jsou stejné jako `nameof`.</span><span class="sxs-lookup"><span data-stu-id="2cfa3-129">The arguments to `handleof` are identical to `nameof`.</span></span> <span data-ttu-id="2cfa3-130">Musí se jednat o jednoduchý název, kvalifikovaný název, přístup ke členům, základní přístup se zadaným členem nebo tento přístup se zadaným členem.</span><span class="sxs-lookup"><span data-stu-id="2cfa3-130">It must be a simple name, qualified name, member access, base access with a specified member, or this access with a specified member.</span></span> <span data-ttu-id="2cfa3-131">Výraz argumentu identifikuje definici kódu, ale není nikdy vyhodnocena.</span><span class="sxs-lookup"><span data-stu-id="2cfa3-131">The argument expression identifies a code definition, but it is never evaluated.</span></span>

<span data-ttu-id="2cfa3-132">Výraz `handleof` je vyhodnocen za běhu a má návratový typ `RuntimeHandle`.</span><span class="sxs-lookup"><span data-stu-id="2cfa3-132">The `handleof` expression is evaluated at runtime and has a return type of `RuntimeHandle`.</span></span> <span data-ttu-id="2cfa3-133">To lze provést v bezpečném kódu i v případě nebezpečného.</span><span class="sxs-lookup"><span data-stu-id="2cfa3-133">This can be executed in safe code as well as unsafe.</span></span> 

``` 
RuntimeHandle stringHandle = handleof(string);
```

<span data-ttu-id="2cfa3-134">Omezení této funkce:</span><span class="sxs-lookup"><span data-stu-id="2cfa3-134">Restrictions of this feature:</span></span>

- <span data-ttu-id="2cfa3-135">Ve výrazu `handleof` nelze použít vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="2cfa3-135">Properties cannot be used in a `handleof` expression.</span></span>
- <span data-ttu-id="2cfa3-136">Výraz `handleof` nelze použít, je-li v oboru existující název `handleof`.</span><span class="sxs-lookup"><span data-stu-id="2cfa3-136">The `handleof` expression cannot be used when there is an existing `handleof` name in scope.</span></span> <span data-ttu-id="2cfa3-137">Například typ, obor názvů atd...</span><span class="sxs-lookup"><span data-stu-id="2cfa3-137">For example a type, namespace, etc ...</span></span>

### <a name="calli"></a><span data-ttu-id="2cfa3-138">Calli</span><span class="sxs-lookup"><span data-stu-id="2cfa3-138">calli</span></span>

<span data-ttu-id="2cfa3-139">Kompilátor přidá podporu pro nový typ funkce `extern`, který se efektivně převede do `.calli` instrukcí.</span><span class="sxs-lookup"><span data-stu-id="2cfa3-139">The compiler will add support for a new type of `extern` function that efficiently translates into a `.calli` instruction.</span></span> <span data-ttu-id="2cfa3-140">Atribut extern bude označen atributem následujícího tvaru:</span><span class="sxs-lookup"><span data-stu-id="2cfa3-140">The extern attribute will be marked with an attribute of the following shape:</span></span>

``` csharp
[AttributeUsage(AttributeTargets.Method)]
public sealed class CallIndirectAttribute : Attribute
{
    public CallingConvention CallingConvention { get; }
    public CallIndirectAttribute(CallingConvention callingConvention)
    {
        CallingConvention = callingConvention;
    }
}
```

<span data-ttu-id="2cfa3-141">To umožňuje vývojářům definovat metody v následujícím tvaru:</span><span class="sxs-lookup"><span data-stu-id="2cfa3-141">This allows developers to define methods in the following form:</span></span>

``` csharp
[CallIndirect(CallingConvention.Cdecl)]
static extern int MapValue(string s, void *ptr);

unsafe {
    var i = MapValue("42", &int.Parse);
    Console.WriteLine(i);
}
```

<span data-ttu-id="2cfa3-142">Omezení pro metodu, která má použit atribut `CallIndirect`:</span><span class="sxs-lookup"><span data-stu-id="2cfa3-142">Restrictions on the method which has the `CallIndirect` attribute applied:</span></span>

- <span data-ttu-id="2cfa3-143">Atribut `DllImport` nelze vytvořit.</span><span class="sxs-lookup"><span data-stu-id="2cfa3-143">Cannot have a `DllImport` attribute.</span></span>
- <span data-ttu-id="2cfa3-144">Nemůže být obecná.</span><span class="sxs-lookup"><span data-stu-id="2cfa3-144">Cannot be generic.</span></span>

## <a name="open-issues"></a><span data-ttu-id="2cfa3-145">Otevřené problémy</span><span class="sxs-lookup"><span data-stu-id="2cfa3-145">Open Issues</span></span>

### <a name="callingconvention"></a><span data-ttu-id="2cfa3-146">CallingConvention</span><span class="sxs-lookup"><span data-stu-id="2cfa3-146">CallingConvention</span></span>

<span data-ttu-id="2cfa3-147">`CallIndirectAttribute`, jak je navrženo, používá výčet `CallingConvention`, který postrádá položku pro konvence spravovaného volání.</span><span class="sxs-lookup"><span data-stu-id="2cfa3-147">The `CallIndirectAttribute` as designed uses the `CallingConvention` enum which lacks an entry for managed calling conventions.</span></span> <span data-ttu-id="2cfa3-148">Výčet musí být rozšířen tak, aby zahrnoval tuto konvenci volání, nebo aby atribut mohl použít jiný přístup.</span><span class="sxs-lookup"><span data-stu-id="2cfa3-148">The enum either needs to be extended to include this calling convention or the attribute needs to take a different approach.</span></span>

## <a name="considerations"></a><span data-ttu-id="2cfa3-149">Požadavky</span><span class="sxs-lookup"><span data-stu-id="2cfa3-149">Considerations</span></span>

### <a name="disambiguating-method-groups"></a><span data-ttu-id="2cfa3-150">Nejednoznačnost skupin metod</span><span class="sxs-lookup"><span data-stu-id="2cfa3-150">Disambiguating method groups</span></span>

<span data-ttu-id="2cfa3-151">K dispozici je diskuze o funkcích, které by usnadnily nejednoznačnost skupin metod předaných do výrazu adresy.</span><span class="sxs-lookup"><span data-stu-id="2cfa3-151">There was some discussion around features that would make it easier to disambiguate method groups passed to an address-of expression.</span></span> <span data-ttu-id="2cfa3-152">Například pokud chcete přidat prvky podpisu do syntaxe:</span><span class="sxs-lookup"><span data-stu-id="2cfa3-152">For instance potentially adding signature elements to the syntax:</span></span>

``` csharp
class Util {
    public static void Log() { ... }
    public static void Log(string) { ... }
}

unsafe {
    // Error: ambiguous Log
    void *ptr1 = &Util.Log;

    // Use Util.Log();
    void *ptr2 = &Util.Log();
}
```

<span data-ttu-id="2cfa3-153">To bylo odmítnuto, protože se nepovedlo udělat nějaký přesvědčivý případ, a tady se nedá předcházet jednoduché syntaxe.</span><span class="sxs-lookup"><span data-stu-id="2cfa3-153">This was rejected because a compelling case could not be made nor could a simple syntax be envisioned here.</span></span> <span data-ttu-id="2cfa3-154">K dispozici je také poměrně rovnou dopředné řešení: jednoduché definujte jinou metodu, která je jednoznačná a používá C# kód pro volání požadované funkce.</span><span class="sxs-lookup"><span data-stu-id="2cfa3-154">Also there is a fairly straight forward work around: simple define another method that is unambiguous and uses C# code to call into the desired function.</span></span> 

``` csharp
class Workaround {
    public static void LocalLog() => Util.Log();
}
unsafe { 
    void* ptr = &Workaround.LocalLog;
}
```

<span data-ttu-id="2cfa3-155">To je ještě jednodušší, pokud `static` místní funkce zadejte jazyk.</span><span class="sxs-lookup"><span data-stu-id="2cfa3-155">This becomes even simpler if `static` local functions enter the language.</span></span> <span data-ttu-id="2cfa3-156">Rozhlasná operace by mohla být definována ve stejné funkci, která používala nejednoznačnou operaci adresy:</span><span class="sxs-lookup"><span data-stu-id="2cfa3-156">Then the work around could be defined in the same function that used the ambiguous address-of operation:</span></span>

``` csharp
unsafe { 
    static void LocalLog() => Util.Log();
    void* ptr = &Workaround.LocalLog;
}
```

### <a name="loadtypetokenint32"></a><span data-ttu-id="2cfa3-157">LoadTypeTokenInt32</span><span class="sxs-lookup"><span data-stu-id="2cfa3-157">LoadTypeTokenInt32</span></span>

<span data-ttu-id="2cfa3-158">Původní návrh povolený pro tokeny metadat se načítá jako hodnoty `int` v době kompilace.</span><span class="sxs-lookup"><span data-stu-id="2cfa3-158">The original proposal allowed for metadata tokens to be loaded as `int` values at compile time.</span></span> <span data-ttu-id="2cfa3-159">V podstatě je `tokenof`, které mají stejné argumenty jako `handleof`, ale vyhodnocují se v době kompilace do `int` konstanty.</span><span class="sxs-lookup"><span data-stu-id="2cfa3-159">Essentially have `tokenof` that has the same arguments as `handleof` but is evaluated at compile time to an `int` constant.</span></span> 

<span data-ttu-id="2cfa3-160">Tato chyba byla odmítnuta, protože způsobuje významné problémy s přepsáními IL (z nichž má mnoho rozhraní .NET).</span><span class="sxs-lookup"><span data-stu-id="2cfa3-160">This was rejected as it causes significant problem for IL rewrites (of which .NET has many).</span></span> <span data-ttu-id="2cfa3-161">Tato přezapisovače často manipuluje s tabulkami metadat způsobem, který by mohl tyto hodnoty neověřit.</span><span class="sxs-lookup"><span data-stu-id="2cfa3-161">Such rewriters often manipulate the metadata tables in a way that could invalidate these values.</span></span> <span data-ttu-id="2cfa3-162">Neexistuje žádný rozumný způsob, jakým by tato přezapisovače aktualizovala tyto hodnoty, když jsou uloženy jako jednoduché hodnoty `int`.</span><span class="sxs-lookup"><span data-stu-id="2cfa3-162">There is no reasonable way for such rewriters to update these values when they are stored as simple `int` values.</span></span>

<span data-ttu-id="2cfa3-163">Základní nápad, který má mít neprůhledný popisovač pro položky metadat, bude nadále prozkoumávat tým modulu runtime.</span><span class="sxs-lookup"><span data-stu-id="2cfa3-163">The underlying idea of having an opaque handle for metadata entries will continue to be explored by the runtime team.</span></span> 

## <a name="future-considerations"></a><span data-ttu-id="2cfa3-164">Budoucí požadavky</span><span class="sxs-lookup"><span data-stu-id="2cfa3-164">Future Considerations</span></span>

### <a name="static-local-functions"></a><span data-ttu-id="2cfa3-165">statické místní funkce</span><span class="sxs-lookup"><span data-stu-id="2cfa3-165">static local functions</span></span>

<span data-ttu-id="2cfa3-166">To odkazuje na [Návrh](https://github.com/dotnet/csharplang/issues/1565) , který povolí modifikátor `static` na místních funkcích.</span><span class="sxs-lookup"><span data-stu-id="2cfa3-166">This refers to [the proposal](https://github.com/dotnet/csharplang/issues/1565) to allow the `static` modifier on local functions.</span></span> <span data-ttu-id="2cfa3-167">Tato funkce by měla být zaručená, že se bude generovat jako `static` a s přesným podpisem zadaným ve zdrojovém kódu.</span><span class="sxs-lookup"><span data-stu-id="2cfa3-167">Such a function would be guaranteed to be emitted as `static` and with the exact signature specified in source code.</span></span> <span data-ttu-id="2cfa3-168">Tato funkce by měla být platným argumentem pro `&`, protože neobsahuje žádné z problémů, které místní funkce dnes mají.</span><span class="sxs-lookup"><span data-stu-id="2cfa3-168">Such a function should be a valid argument to `&` as it contains none of the problems local functions have today.</span></span>

### <a name="nativecallableattribute"></a><span data-ttu-id="2cfa3-169">NativeCallableAttribute</span><span class="sxs-lookup"><span data-stu-id="2cfa3-169">NativeCallableAttribute</span></span>

<span data-ttu-id="2cfa3-170">Modul CLR má funkci, která umožňuje vygenerovat spravované metody takovým způsobem, že jsou přímo z nativního kódu.</span><span class="sxs-lookup"><span data-stu-id="2cfa3-170">The CLR has a feature that allows for managed methods to be emitted in such a way that they are directly callable from native code.</span></span> <span data-ttu-id="2cfa3-171">To se provádí přidáním `NativeCallableAttribute` do metod.</span><span class="sxs-lookup"><span data-stu-id="2cfa3-171">This is done by adding the `NativeCallableAttribute` to methods.</span></span> <span data-ttu-id="2cfa3-172">Taková metoda se dá volat jenom z nativního kódu, takže musí v signatuře obsahovat jenom přenositelné typy.</span><span class="sxs-lookup"><span data-stu-id="2cfa3-172">Such a method is only callable from native code and hence must contain only blittable types in the signature.</span></span> <span data-ttu-id="2cfa3-173">Volání ze spravovaného kódu vede k chybě za běhu.</span><span class="sxs-lookup"><span data-stu-id="2cfa3-173">Calling from managed code results in a runtime error.</span></span> 

<span data-ttu-id="2cfa3-174">Tato funkce by vypadala dobře s tímto návrhem, protože to umožní:</span><span class="sxs-lookup"><span data-stu-id="2cfa3-174">This feature would pattern well with this proposal as it would allow:</span></span>

- <span data-ttu-id="2cfa3-175">Předání funkce definované ve spravovaném kódu do nativního kódu jako ukazatele na funkci (přes adresu) bez režie ve spravovaném nebo nativním kódu.</span><span class="sxs-lookup"><span data-stu-id="2cfa3-175">Passing a function defined in managed code to native code as a function pointer (via address-of) with no overhead in managed or native code.</span></span> 
- <span data-ttu-id="2cfa3-176">Modul runtime může zavést chyby webu pro tyto funkce ve spravovaném kódu a zabránit tak jejich vyvolání v době kompilace.</span><span class="sxs-lookup"><span data-stu-id="2cfa3-176">Runtime can introduce use site errors for such functions in managed code to prevent them from being invoked at compile time.</span></span>




