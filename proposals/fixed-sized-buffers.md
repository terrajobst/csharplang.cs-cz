---
ms.openlocfilehash: b51f27b2f58fd19851c80beb9cedcbd32b80b165
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484585"
---
# <a name="fixed-sized-buffers"></a><span data-ttu-id="5184e-101">Vyrovnávací paměti pevné velikosti</span><span class="sxs-lookup"><span data-stu-id="5184e-101">Fixed Sized Buffers</span></span>

* <span data-ttu-id="5184e-102">Navrženo [x]</span><span class="sxs-lookup"><span data-stu-id="5184e-102">[x] Proposed</span></span>
* <span data-ttu-id="5184e-103">[] Prototyp: Nezahájeno</span><span class="sxs-lookup"><span data-stu-id="5184e-103">[ ] Prototype: Not Started</span></span>
* <span data-ttu-id="5184e-104">[] Implementace: Nezahájeno</span><span class="sxs-lookup"><span data-stu-id="5184e-104">[ ] Implementation: Not Started</span></span>
* <span data-ttu-id="5184e-105">[] Specifikace: Nezahájeno</span><span class="sxs-lookup"><span data-stu-id="5184e-105">[ ] Specification: Not Started</span></span>

## <a name="summary"></a><span data-ttu-id="5184e-106">Souhrn</span><span class="sxs-lookup"><span data-stu-id="5184e-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="5184e-107">Poskytněte obecný a bezpečný mechanismus pro deklarování vyrovnávacích pamětí s pevnou velikostí do C# jazyka.</span><span class="sxs-lookup"><span data-stu-id="5184e-107">Provide a general-purpose and safe mechanism for declaring fixed sized buffers to the C# language.</span></span>

## <a name="motivation"></a><span data-ttu-id="5184e-108">Motivační</span><span class="sxs-lookup"><span data-stu-id="5184e-108">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="5184e-109">V současné době uživatelé mají možnost vytvářet vyrovnávací paměti s pevnou velikostí v nezabezpečeném kontextu.</span><span class="sxs-lookup"><span data-stu-id="5184e-109">Today, users have the ability to create fixed-sized buffers in an unsafe-context.</span></span> <span data-ttu-id="5184e-110">To však vyžaduje, aby uživatel mohl pracovat s ukazateli, ručně provádět kontroly hranic a podporuje pouze omezenou sadu typů (`bool`, `byte`, `char`, `short`, `int`, `long`, `sbyte`, `ushort`, `uint`, `ulong`, `float`a `double`).</span><span class="sxs-lookup"><span data-stu-id="5184e-110">However, this requires the user to deal with pointers, manually perform bounds checks, and only supports a limited set of types (`bool`, `byte`, `char`, `short`, `int`, `long`, `sbyte`, `ushort`, `uint`, `ulong`, `float`, and `double`).</span></span>

<span data-ttu-id="5184e-111">Nejběžnější stížnost je, že vyrovnávací paměti s pevnou velikostí nelze indexovat v bezpečném kódu.</span><span class="sxs-lookup"><span data-stu-id="5184e-111">The most common complaint is that fixed-size buffers cannot be indexed in safe code.</span></span> <span data-ttu-id="5184e-112">Neschopnost používat další typy jsou sekundy.</span><span class="sxs-lookup"><span data-stu-id="5184e-112">Inability to use more types is the second.</span></span>

<span data-ttu-id="5184e-113">V několika menších vylepšeních jsme mohli poskytnout obecné vyrovnávací paměti s pevnou velikostí, které podporují libovolný typ, mohou být použity v bezpečném kontextu a provedeny automatické kontroly hranic.</span><span class="sxs-lookup"><span data-stu-id="5184e-113">With a few minor tweaks, we could provide general-purpose fixed-sized buffers which support any type, can be used in a safe context, and have automatic bounds checking performed.</span></span>

## <a name="detailed-design"></a><span data-ttu-id="5184e-114">Podrobný návrh</span><span class="sxs-lookup"><span data-stu-id="5184e-114">Detailed design</span></span>
[design]: #detailed-design

<span data-ttu-id="5184e-115">Jedna z nich by mohla deklarovat zabezpečenou vyrovnávací paměť s pevnou velikostí pomocí následujících možností:</span><span class="sxs-lookup"><span data-stu-id="5184e-115">One would declare a safe fixed-sized buffer via the following:</span></span>

```csharp
public fixed DXGI_RGB GammaCurve[1025];
```

<span data-ttu-id="5184e-116">Deklarace by byla přeložena do interní reprezentace kompilátorem, který je podobný následujícímu.</span><span class="sxs-lookup"><span data-stu-id="5184e-116">The declaration would get translated into an internal representation by the compiler that is similar to the following</span></span>

```csharp
[FixedBuffer(typeof(DXGI_RGB), 1024)]
public ConsoleApp1.<Buffer>e__FixedBuffer_1024<DXGI_RGB> GammaCurve;

// Pack = 0 is the default packing and should result in indexable layout.
[CompilerGenerated, UnsafeValueType, StructLayout(LayoutKind.Sequential, Pack = 0)]
struct <Buffer>e__FixedBuffer_1024<T>
{
    private T _e0;
    private T _e1;
    // _e2 ... _e1023
    private T _e1024;

    public ref T this[int index] => ref (uint)index <= 1024u ?
                                         ref RefAdd<T>(ref _e0, index):
                                         throw new IndexOutOfRange();
}
```

<span data-ttu-id="5184e-117">Vzhledem k tomu, že vyrovnávací paměti s pevnou velikostí již nevyžadují použití `fixed`, má smysl, aby byl povolen libovolný typ elementu.</span><span class="sxs-lookup"><span data-stu-id="5184e-117">Since such fixed-sized buffers no longer require use of `fixed`, it makes sense to allow any element type.</span></span>  

> <span data-ttu-id="5184e-118">Poznámka: `fixed` se pořád podporuje, ale jenom v případě, že je typ elementu `blittable`</span><span class="sxs-lookup"><span data-stu-id="5184e-118">NOTE: `fixed` will still be supported, but only if the element type is `blittable`</span></span>

## <a name="drawbacks"></a><span data-ttu-id="5184e-119">Nevýhody</span><span class="sxs-lookup"><span data-stu-id="5184e-119">Drawbacks</span></span>
[drawbacks]: #drawbacks

* <span data-ttu-id="5184e-120">Mohlo dojít k problémům s zpětnou kompatibilitou, ale vzhledem k tomu, že existující vyrovnávací paměti s pevnou velikostí fungují pouze s výběrem primitivních typů, je vhodné, aby kompilátor pokračoval "Just-buffer", pokud uživatel považuje pevnou vyrovnávací paměť jako ukazatele.</span><span class="sxs-lookup"><span data-stu-id="5184e-120">There could be some challenges with backwards compatibility, but given that the existing fixed-sized buffers only work with a selection of primitive types, it should be possible for the compiler to continue "just-working" if the user treats the fixed-buffer as a pointer.</span></span>
* <span data-ttu-id="5184e-121">Nekompatibilní konstrukce mohou vyžadovat použití mírně odlišného kódování `v2` pro skrytí polí ze starého kompilátoru.</span><span class="sxs-lookup"><span data-stu-id="5184e-121">Incompatible constructs may need to use slightly different `v2` encoding to hide the fields from old compiler.</span></span>
* <span data-ttu-id="5184e-122">Balení není správně definované v specifikaci IL pro obecné typy.</span><span class="sxs-lookup"><span data-stu-id="5184e-122">Packing is not well defined in IL spec for generic types.</span></span> <span data-ttu-id="5184e-123">I když by měl tento přístup fungovat, bude se vycházet z nedokumentované reakce.</span><span class="sxs-lookup"><span data-stu-id="5184e-123">While the approach should work, we will be bordering on undocumented behavior.</span></span> <span data-ttu-id="5184e-124">Měli bychom tuto dokumentaci dělat a zajistěte, aby další nepovolují kompilátory JIT, jako je mono, měly stejné chování.</span><span class="sxs-lookup"><span data-stu-id="5184e-124">We should make that documented and make sure other JITs like Mono have the same behavior.</span></span>
* <span data-ttu-id="5184e-125">Zadání samostatného typu pro každou délku (případně další pro `readonly` pole, pokud se podporuje) bude mít vliv na metadata.</span><span class="sxs-lookup"><span data-stu-id="5184e-125">Specifying a separate type for every length (an possibly another for `readonly` fields, if supported) will have impact on metadata.</span></span> <span data-ttu-id="5184e-126">Bude vázán podle počtu polí různých velikostí v dané aplikaci.</span><span class="sxs-lookup"><span data-stu-id="5184e-126">It will be bound by the number of arrays of different sizes in the given app.</span></span>
* <span data-ttu-id="5184e-127">`ref` Math nelze formálně ověřit (protože je nebezpečný).</span><span class="sxs-lookup"><span data-stu-id="5184e-127">`ref` math is not formally verifiable (since it is unsafe).</span></span> <span data-ttu-id="5184e-128">Budeme muset najít způsob, jak aktualizovat pravidla ověřování, abychom věděli, že naše použití je v pořádku.</span><span class="sxs-lookup"><span data-stu-id="5184e-128">We will need to find a way to update verification rules to know that our use is ok.</span></span>

## <a name="alternatives"></a><span data-ttu-id="5184e-129">Alternativy</span><span class="sxs-lookup"><span data-stu-id="5184e-129">Alternatives</span></span>
[alternatives]: #alternatives

<span data-ttu-id="5184e-130">Manuálně deklarujte své struktury a pomocí nebezpečného kódu Sestavujte indexery.</span><span class="sxs-lookup"><span data-stu-id="5184e-130">Manually declare your structures and use unsafe code to construct indexers.</span></span>

## <a name="unresolved-questions"></a><span data-ttu-id="5184e-131">Nevyřešené dotazy</span><span class="sxs-lookup"><span data-stu-id="5184e-131">Unresolved questions</span></span>
[unresolved]: #unresolved-questions

- <span data-ttu-id="5184e-132">Měli bychom `readonly`?</span><span class="sxs-lookup"><span data-stu-id="5184e-132">should we allow `readonly`?</span></span>  <span data-ttu-id="5184e-133">(s indexerem jen pro čtení)</span><span class="sxs-lookup"><span data-stu-id="5184e-133">(with readonly indexer)</span></span>
- <span data-ttu-id="5184e-134">je potřeba, abychom povolili Inicializátory polí?</span><span class="sxs-lookup"><span data-stu-id="5184e-134">should we allow array initializers?</span></span>
- <span data-ttu-id="5184e-135">je klíčové slovo `fixed` nutné?</span><span class="sxs-lookup"><span data-stu-id="5184e-135">is `fixed` keyword necessary?</span></span>
- <span data-ttu-id="5184e-136">`foreach`?</span><span class="sxs-lookup"><span data-stu-id="5184e-136">`foreach`?</span></span>
- <span data-ttu-id="5184e-137">pouze pole instancí v strukturách?</span><span class="sxs-lookup"><span data-stu-id="5184e-137">only instance fields in structs?</span></span>

## <a name="design-meetings"></a><span data-ttu-id="5184e-138">Schůzky pro návrh</span><span class="sxs-lookup"><span data-stu-id="5184e-138">Design meetings</span></span>

<span data-ttu-id="5184e-139">Odkaz na poznámky k návrhu, které mají vliv na tento návrh, a popište jednu větu pro každou změnu, na kterou vedla.</span><span class="sxs-lookup"><span data-stu-id="5184e-139">Link to design notes that affect this proposal, and describe in one sentence for each what changes they led to.</span></span>