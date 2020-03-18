---
ms.openlocfilehash: a8822137c85f449444ed675c6f2912315c041691
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484473"
---
# <a name="static-delegates"></a><span data-ttu-id="0dccd-101">Statické Delegáti</span><span class="sxs-lookup"><span data-stu-id="0dccd-101">Static Delegates</span></span>

* <span data-ttu-id="0dccd-102">Navrženo [x]</span><span class="sxs-lookup"><span data-stu-id="0dccd-102">[x] Proposed</span></span>
* <span data-ttu-id="0dccd-103">[] Prototyp: Nezahájeno</span><span class="sxs-lookup"><span data-stu-id="0dccd-103">[ ] Prototype: Not Started</span></span>
* <span data-ttu-id="0dccd-104">[] Implementace: Nezahájeno</span><span class="sxs-lookup"><span data-stu-id="0dccd-104">[ ] Implementation: Not Started</span></span>
* <span data-ttu-id="0dccd-105">[] Specifikace: Nezahájeno</span><span class="sxs-lookup"><span data-stu-id="0dccd-105">[ ] Specification: Not Started</span></span>

## <a name="summary"></a><span data-ttu-id="0dccd-106">Souhrn</span><span class="sxs-lookup"><span data-stu-id="0dccd-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="0dccd-107">Poskytněte pro C# jazyk základní a zjednodušenou funkci zpětného volání pro obecné účely.</span><span class="sxs-lookup"><span data-stu-id="0dccd-107">Provide a general-purpose, lightweight callback capability to the C# language.</span></span>

## <a name="motivation"></a><span data-ttu-id="0dccd-108">Motivační</span><span class="sxs-lookup"><span data-stu-id="0dccd-108">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="0dccd-109">V dnešní době mají uživatelé možnost vytvářet zpětná volání pomocí `System.Delegate`ho typu.</span><span class="sxs-lookup"><span data-stu-id="0dccd-109">Today, users have the ability to create callbacks using the `System.Delegate` type.</span></span> <span data-ttu-id="0dccd-110">Jsou to však poměrně těžké (například vyžadovat přidělení haldy a vždy mají za zpracování zřetězení zpětného volání).</span><span class="sxs-lookup"><span data-stu-id="0dccd-110">However, these are fairly heavyweight (such as requiring a heap allocation and always having handling for chaining callbacks together).</span></span>

<span data-ttu-id="0dccd-111">Kromě toho `System.Delegate` neposkytuje nejlepší spolupráci s nespravovanými ukazateli na funkce, konkrétně v případě nešíření a vyžadování zařazování, kdykoli IT přechází ze spravovaného a nespravovaného ohraničení.</span><span class="sxs-lookup"><span data-stu-id="0dccd-111">Additionally, `System.Delegate` does not provide the best interop with unmanaged function pointers, namely due being non-blittable and requiring marshalling anytime it crosses the managed/unmanaged boundary.</span></span>

<span data-ttu-id="0dccd-112">U několika menších vylepšení můžeme poskytnout nový typ delegáta, který je odlehčený, obecný a spolupracuje s nativním kódem.</span><span class="sxs-lookup"><span data-stu-id="0dccd-112">With a few minor tweaks, we could provide a new type of delegate that is lightweight, general-purpose, and interops well with native code.</span></span>

## <a name="detailed-design"></a><span data-ttu-id="0dccd-113">Podrobný návrh</span><span class="sxs-lookup"><span data-stu-id="0dccd-113">Detailed design</span></span>
[design]: #detailed-design

<span data-ttu-id="0dccd-114">Jedna by mohla deklarovat statický delegát přes následující:</span><span class="sxs-lookup"><span data-stu-id="0dccd-114">One would declare a static delegate via the following:</span></span>

```C#
static delegate int Func()
```

<span data-ttu-id="0dccd-115">Jedna by mohla dále atributem deklarace s podobným `System.Runtime.InteropServices.UnmanagedFunctionPointer`, aby bylo možné řídit konvence volání, zařazování řetězců a nastavit poslední chybu chování.</span><span class="sxs-lookup"><span data-stu-id="0dccd-115">One could additionally attribute the declaration with something similar to `System.Runtime.InteropServices.UnmanagedFunctionPointer` so that the calling convention, string marshalling, and set last error behavior can be controlled.</span></span> <span data-ttu-id="0dccd-116">Poznámka: použití samotného `System.Runtime.InteropServices.UnmanagedFunctionPointer` nebude fungovat, protože je použitelné pouze pro delegáty.</span><span class="sxs-lookup"><span data-stu-id="0dccd-116">NOTE: Using `System.Runtime.InteropServices.UnmanagedFunctionPointer` itself will not work, as it is only usable on Delegates.</span></span>

<span data-ttu-id="0dccd-117">Deklarace by byla přeložena do interní reprezentace kompilátorem, který je podobný následujícímu.</span><span class="sxs-lookup"><span data-stu-id="0dccd-117">The declaration would get translated into an internal representation by the compiler that is similar to the following</span></span>

```C#
struct <Func>e__StaticDelegate
{
    IntPtr pFunction;

    static int WellKnownCompilerName();
}
```

<span data-ttu-id="0dccd-118">To znamená, že je interně reprezentovaná strukturou, která má jednoho člena typu `IntPtr` (taková struktura je přenositelná a neposkytuje žádné přidělení haldy):</span><span class="sxs-lookup"><span data-stu-id="0dccd-118">That is to say, it is internally represented by a struct that has a single member of type `IntPtr` (such a struct is blittable and does not incur any heap allocations):</span></span>
* <span data-ttu-id="0dccd-119">Člen obsahuje adresu funkce, která má být zpětné volání.</span><span class="sxs-lookup"><span data-stu-id="0dccd-119">The member contains the address of the function that is to be the callback.</span></span>
* <span data-ttu-id="0dccd-120">Typ deklaruje metodu, která odpovídá signatuře metody zpětného volání.</span><span class="sxs-lookup"><span data-stu-id="0dccd-120">The type declares a method matching the method signature of the callback.</span></span>
* <span data-ttu-id="0dccd-121">Název struktury by neměl být User-constructible (stejně jako u jiných interně generovaných záložních struktur).</span><span class="sxs-lookup"><span data-stu-id="0dccd-121">The name of the struct should not be user-constructible (as we do with other internally generated backing structures).</span></span>
 * <span data-ttu-id="0dccd-122">Například: vyrovnávací paměti pevné velikosti generují strukturu s názvem ve formátu `<name>e__FixedBuffer` (`<` a `>` jsou součástí identifikátoru a constructible se, že identifikátor není v C#, ale stále funguje v Il).</span><span class="sxs-lookup"><span data-stu-id="0dccd-122">For example: fixed size buffers generate a struct with a name in the format of `<name>e__FixedBuffer` (`<` and `>` are part of the identifier and make the identifier not constructible in C#, but still useable in IL).</span></span>
* <span data-ttu-id="0dccd-123">Název deklarace metody by měl být známý název, který se používá ve všech typech statických delegátů (díky tomu může kompilátor znát název, který se má při určování podpisu najít).</span><span class="sxs-lookup"><span data-stu-id="0dccd-123">The name of the method declaration should be a well known name used across all static delegate types (this allows the compiler to know the name to look for when determining the signature).</span></span>

<span data-ttu-id="0dccd-124">Hodnota statického delegáta může být vázána pouze na statickou metodu, která odpovídá signatuře zpětného volání.</span><span class="sxs-lookup"><span data-stu-id="0dccd-124">The value of the static delegate can only be bound to a static method that matches the signature of the callback.</span></span>

<span data-ttu-id="0dccd-125">Zřetězení zpětných volání není podporováno.</span><span class="sxs-lookup"><span data-stu-id="0dccd-125">Chaining callbacks together is not supported.</span></span>

<span data-ttu-id="0dccd-126">Vyvolání zpětného volání by bylo implementováno `calli` instrukcí.</span><span class="sxs-lookup"><span data-stu-id="0dccd-126">Invocation of the callback would be implemented by the `calli` instruction.</span></span>

## <a name="drawbacks"></a><span data-ttu-id="0dccd-127">Nevýhody</span><span class="sxs-lookup"><span data-stu-id="0dccd-127">Drawbacks</span></span>
[drawbacks]: #drawbacks

<span data-ttu-id="0dccd-128">Statické Delegáti nefungují se stávajícími rozhraními API, která používají běžné delegáty (jeden by musel zabalit tohoto statického delegáta do pravidelného delegáta stejné signatury).</span><span class="sxs-lookup"><span data-stu-id="0dccd-128">Static Delegates would not work with existing APIs that use regular delegates (one would need to wrap said static delegate in a regular delegate of the same signature).</span></span>
* <span data-ttu-id="0dccd-129">Vzhledem k tomu, že `System.Delegate` je interně zastoupen jako sada `object` a `IntPtr` polí (http://source.dot.net/#System.Private.CoreLib/src/System/Delegate.cs), bylo možné umožnit implicitní převod statického delegáta na `System.Delegate`, který má odpovídající signaturu metody.</span><span class="sxs-lookup"><span data-stu-id="0dccd-129">Given that `System.Delegate` is represented internally as a set of `object` and `IntPtr` fields (http://source.dot.net/#System.Private.CoreLib/src/System/Delegate.cs), it would be possible to allow implicit conversion of a static delegate to a `System.Delegate` that has a matching method signature.</span></span> <span data-ttu-id="0dccd-130">Je také možné poskytnout explicitní převod v opačném směru za předpokladu, že `System.Delegate` vyhovuje všem požadavkům statického delegáta.</span><span class="sxs-lookup"><span data-stu-id="0dccd-130">It would also be possible to provide an explicit conversion in the opposite direction, provided the `System.Delegate` conformed to all the requirements of being a static delegate.</span></span>

<span data-ttu-id="0dccd-131">Pro zajištění snadného použití statického delegáta v rámci základní architektury by se vyžadovala další práce.</span><span class="sxs-lookup"><span data-stu-id="0dccd-131">Additional work would be needed to make Static Delegate readily usable in the core framework.</span></span>

## <a name="alternatives"></a><span data-ttu-id="0dccd-132">Alternativy</span><span class="sxs-lookup"><span data-stu-id="0dccd-132">Alternatives</span></span>
[alternatives]: #alternatives

<span data-ttu-id="0dccd-133">Bude doplněno</span><span class="sxs-lookup"><span data-stu-id="0dccd-133">TBD</span></span>

## <a name="unresolved-questions"></a><span data-ttu-id="0dccd-134">Nevyřešené dotazy</span><span class="sxs-lookup"><span data-stu-id="0dccd-134">Unresolved questions</span></span>
[unresolved]: #unresolved-questions

<span data-ttu-id="0dccd-135">Jaké části návrhu jsou stále TBD?</span><span class="sxs-lookup"><span data-stu-id="0dccd-135">What parts of the design are still TBD?</span></span>

## <a name="design-meetings"></a><span data-ttu-id="0dccd-136">Schůzky pro návrh</span><span class="sxs-lookup"><span data-stu-id="0dccd-136">Design meetings</span></span>

<span data-ttu-id="0dccd-137">Odkaz na poznámky k návrhu, které mají vliv na tento návrh, a popište jednu větu pro každou změnu, na kterou vedla.</span><span class="sxs-lookup"><span data-stu-id="0dccd-137">Link to design notes that affect this proposal, and describe in one sentence for each what changes they led to.</span></span>


