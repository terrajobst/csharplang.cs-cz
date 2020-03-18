---
ms.openlocfilehash: 11e9d21bda9e69be692c5c5f5aee80c2da1894ab
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484620"
---
# <a name="unmanaged-type-constraint"></a><span data-ttu-id="538cd-101">Omezení nespravovaného typu</span><span class="sxs-lookup"><span data-stu-id="538cd-101">Unmanaged type constraint</span></span>

## <a name="summary"></a><span data-ttu-id="538cd-102">Souhrn</span><span class="sxs-lookup"><span data-stu-id="538cd-102">Summary</span></span>
[summary]: #summary

<span data-ttu-id="538cd-103">Funkce nespravovaného omezení poskytuje vynucení jazyka pro třídu typů, která je známá jako "nespravované typy C# " ve specifikaci jazyka.  To je definováno v sekci 18,2 jako typ, který není odkazový typ a neobsahuje pole referenčního typu na jakékoli úrovni vnořování.</span><span class="sxs-lookup"><span data-stu-id="538cd-103">The unmanaged constraint feature will give language enforcement to the class of types known as "unmanaged types" in the C# language spec.  This is defined in section 18.2 as a type which is not a reference type and doesn't contain reference type fields at any level of nesting.</span></span>  

## <a name="motivation"></a><span data-ttu-id="538cd-104">Motivační</span><span class="sxs-lookup"><span data-stu-id="538cd-104">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="538cd-105">Primární motivace je usnadnit vytváření kódu spolupráce na nízké úrovni v C#.</span><span class="sxs-lookup"><span data-stu-id="538cd-105">The primary motivation is to make it easier to author low level interop code in C#.</span></span> <span data-ttu-id="538cd-106">Nespravované typy jsou jedním ze základních stavebních bloků pro interoperabilitu kód, ale chybějící podpora v obecných typech znemožňuje vytváření opakovaně použitelných rutin napříč všemi nespravovanými typy.</span><span class="sxs-lookup"><span data-stu-id="538cd-106">Unmanaged types are one of the core building blocks for interop code, yet the lack of support in generics makes it impossible to create re-usable routines across all unmanaged types.</span></span> <span data-ttu-id="538cd-107">Místo toho mohou vývojáři vytvářet stejný kód kotlového talíře pro každý nespravovaný typ v knihovně:</span><span class="sxs-lookup"><span data-stu-id="538cd-107">Instead developers are forced to author the same boiler plate code for every unmanaged type in their library:</span></span>

```csharp
int Hash(Point point) { ... } 
int Hash(TimeSpan timeSpan) { ... } 
```

<span data-ttu-id="538cd-108">Pro povolení tohoto typu scénáře bude jazyk zavádět nové omezení: nespravované:</span><span class="sxs-lookup"><span data-stu-id="538cd-108">To enable this type of scenario the language will be introducing a new constraint: unmanaged:</span></span>

```csharp
void Hash<T>(T value) where T : unmanaged
{
    ...
}
```

<span data-ttu-id="538cd-109">Toto omezení může být splněno pouze typy, které se vejdou do definice nespravovaného typu C# ve specifikaci jazyka. Dalším způsobem, jak to zjistit, je, že typ splňuje nespravovaná omezení pokud může být použit také jako ukazatel.</span><span class="sxs-lookup"><span data-stu-id="538cd-109">This constraint can only be met by types which fit into the unmanaged type definition in the C# language spec. Another way of looking at it is that a type satisfies the unmanaged constraint iff it can also be used as a pointer.</span></span> 

```csharp
Hash(new Point()); // Okay 
Hash(42); // Okay
Hash("hello") // Error: Type string does not satisfy the unmanaged constraint
```

<span data-ttu-id="538cd-110">Parametry typu s nespravovaným omezením můžou využívat všechny funkce, které jsou k dispozici pro nespravované typy: ukazatelé, pevné atd...</span><span class="sxs-lookup"><span data-stu-id="538cd-110">Type parameters with the unmanaged constraint can use all the features available to unmanaged types: pointers, fixed, etc ...</span></span> 

```csharp
void Hash<T>(T value) where T : unmanaged
{
    // Okay
    fixed (T* p = &value) 
    { 
        ...
    }
}
```

<span data-ttu-id="538cd-111">Toto omezení také umožní efektivní převody mezi strukturovanými daty a proudy bajtů.</span><span class="sxs-lookup"><span data-stu-id="538cd-111">This constraint will also make it possible to have efficient conversions between structured data and streams of bytes.</span></span> <span data-ttu-id="538cd-112">Toto je operace, která je společná v síťových zásobníkech a vrstvách serializace:</span><span class="sxs-lookup"><span data-stu-id="538cd-112">This is an operation that is common in networking stacks and serialization layers:</span></span>

```csharp
Span<byte> Convert<T>(ref T value) where T : unmanaged 
{
    ...
}
```

<span data-ttu-id="538cd-113">Tyto rutiny jsou výhodné, protože jsou v době kompilace prokazatelně bezpečné a přidělení je bezplatné.</span><span class="sxs-lookup"><span data-stu-id="538cd-113">Such routines are advantageous because they are provably safe at compile time and allocation free.</span></span>  <span data-ttu-id="538cd-114">Autoři spolupracují dnes nemůžou to udělat (i když se nachází ve vrstvě, kde je výkon kritický).</span><span class="sxs-lookup"><span data-stu-id="538cd-114">Interop authors today can not do this (even though it's at a layer where perf is critical).</span></span>  <span data-ttu-id="538cd-115">Místo toho musí spoléhat na přidělení rutin, které mají náročné kontroly za běhu, aby ověřily hodnoty správně nespravované.</span><span class="sxs-lookup"><span data-stu-id="538cd-115">Instead they need to rely on allocating routines that have expensive runtime checks to verify values are correctly unmanaged.</span></span>

## <a name="detailed-design"></a><span data-ttu-id="538cd-116">Podrobný návrh</span><span class="sxs-lookup"><span data-stu-id="538cd-116">Detailed design</span></span>
[design]: #detailed-design

<span data-ttu-id="538cd-117">Jazyk zavede nové omezení s názvem `unmanaged`.</span><span class="sxs-lookup"><span data-stu-id="538cd-117">The language will introduce a new constraint named `unmanaged`.</span></span> <span data-ttu-id="538cd-118">Aby bylo toto omezení splněno, musí být typem struktura a všechna pole typu musí být rozdělena do jedné z následujících kategorií:</span><span class="sxs-lookup"><span data-stu-id="538cd-118">In order to satisfy this constraint a type must be a struct and all the fields of the type must fall into one of the following categories:</span></span>

- <span data-ttu-id="538cd-119">Mít typ `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, `decimal`, `bool`, `IntPtr` nebo `UIntPtr`.</span><span class="sxs-lookup"><span data-stu-id="538cd-119">Have the type `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, `decimal`, `bool`, `IntPtr` or `UIntPtr`.</span></span>
- <span data-ttu-id="538cd-120">Být libovolný typ `enum`.</span><span class="sxs-lookup"><span data-stu-id="538cd-120">Be any `enum` type.</span></span>
- <span data-ttu-id="538cd-121">Být typu ukazatel.</span><span class="sxs-lookup"><span data-stu-id="538cd-121">Be a pointer type.</span></span>
- <span data-ttu-id="538cd-122">Musí se jednat o uživatelsky definovanou strukturu, která satsifies omezení `unmanaged`.</span><span class="sxs-lookup"><span data-stu-id="538cd-122">Be a user defined struct that satsifies the `unmanaged` constraint.</span></span>

<span data-ttu-id="538cd-123">Tato omezení musí splňovat i pole instance generovaná kompilátorem, jako jsou například tyto automaticky implementované vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="538cd-123">Compiler generated instance fields, such as those backing auto-implemented properties, must also meet these constraints.</span></span> 

<span data-ttu-id="538cd-124">Příklad:</span><span class="sxs-lookup"><span data-stu-id="538cd-124">For example:</span></span>

```csharp
// Unmanaged type
struct Point 
{ 
    int X;
    int Y {get; set;}
}

// Not an unmanaged type
struct Student 
{ 
    string FirstName;
    string LastName;
}
``` 

<span data-ttu-id="538cd-125">Omezení `unmanaged` nelze kombinovat s `struct`, `class` ani `new()`.</span><span class="sxs-lookup"><span data-stu-id="538cd-125">The `unmanaged` constraint cannot be combined with `struct`, `class` or `new()`.</span></span> <span data-ttu-id="538cd-126">Toto omezení je odvozeno od faktu, že `unmanaged` implikuje `struct` takže ostatní omezení nedělají smysl.</span><span class="sxs-lookup"><span data-stu-id="538cd-126">This restriction derives from the fact that `unmanaged` implies `struct` hence the other constraints do not make sense.</span></span>

<span data-ttu-id="538cd-127">Omezení `unmanaged` není vynutilo modulem CLR, pouze podle jazyka.</span><span class="sxs-lookup"><span data-stu-id="538cd-127">The `unmanaged` constraint is not enforced by CLR, only by the language.</span></span> <span data-ttu-id="538cd-128">Aby nedocházelo k neoprávněnému použití v jiných jazycích, metody, které mají toto omezení, budou chráněny pomocí mod-REQ. To zabrání jiným jazykům v použití argumentů typu, které nejsou nespravovanými typy.</span><span class="sxs-lookup"><span data-stu-id="538cd-128">To prevent mis-use by other languages, methods which have this constraint will be protected by a mod-req. This will prevent other languages from using type arguments which are not unmanaged types.</span></span>

<span data-ttu-id="538cd-129">Token `unmanaged` v omezení není klíčové slovo, ani kontextové klíčové slovo.</span><span class="sxs-lookup"><span data-stu-id="538cd-129">The token `unmanaged` in the constraint is not a keyword, nor a contextual keyword.</span></span> <span data-ttu-id="538cd-130">Místo toho je třeba `var` v tom, že se vyhodnotí v tomto umístění a bude mít jednu z těchto hodnot:</span><span class="sxs-lookup"><span data-stu-id="538cd-130">Instead it is like `var` in that it is evaluated at that location and will either:</span></span>

- <span data-ttu-id="538cd-131">Vytvoření vazby na uživatelem definovaný nebo odkazovaný typ s názvem `unmanaged`: Tato akce bude zpracována stejně jako jakékoli jiné omezení pojmenovaného typu.</span><span class="sxs-lookup"><span data-stu-id="538cd-131">Bind to user defined or referenced type named `unmanaged`: This will be treated just as any other named type constraint is treated.</span></span> 
- <span data-ttu-id="538cd-132">Vazba bez typu: Tato akce bude interpretována jako omezení `unmanaged`.</span><span class="sxs-lookup"><span data-stu-id="538cd-132">Bind to no type: This will be interpreted as the `unmanaged` constraint.</span></span>

<span data-ttu-id="538cd-133">V případě, že existuje typ s názvem `unmanaged` a je k dispozici bez kvalifikace v aktuálním kontextu, nebude možné použít omezení `unmanaged`.</span><span class="sxs-lookup"><span data-stu-id="538cd-133">In the case there is a type named `unmanaged` and it is available without qualification in the current context, then there will be no way to use the `unmanaged` constraint.</span></span> <span data-ttu-id="538cd-134">To paralelně pravidla obklopující `var` a uživatelsky definované typy se stejným názvem.</span><span class="sxs-lookup"><span data-stu-id="538cd-134">This parallels the rules surrounding the feature `var` and user defined types of the same name.</span></span> 

## <a name="drawbacks"></a><span data-ttu-id="538cd-135">Nevýhody</span><span class="sxs-lookup"><span data-stu-id="538cd-135">Drawbacks</span></span>
[drawbacks]: #drawbacks

<span data-ttu-id="538cd-136">Hlavní nevýhodou této funkce je, že obsluhuje malý počet vývojářů: typicky nízká úroveň autorů knihoven nebo architektur.</span><span class="sxs-lookup"><span data-stu-id="538cd-136">The primary drawback of this feature is that it serves a small number of developers: typically low level library authors or frameworks.</span></span>  <span data-ttu-id="538cd-137">Proto je u malého počtu vývojářů útrata úžasného jazyka.</span><span class="sxs-lookup"><span data-stu-id="538cd-137">Hence it's spending precious language time for a small number of developers.</span></span> 

<span data-ttu-id="538cd-138">Tyto architektury jsou ale často základem pro většinu aplikací .NET.</span><span class="sxs-lookup"><span data-stu-id="538cd-138">Yet these frameworks are often the basis for the majority of .NET applications out there.</span></span>  <span data-ttu-id="538cd-139">Proto výkon/správnost služby WINS na této úrovni může mít na ekosystém .NET vliv na Ripple.</span><span class="sxs-lookup"><span data-stu-id="538cd-139">Hence performance / correctness wins at this level can have a ripple effect on the .NET ecosystem.</span></span>  <span data-ttu-id="538cd-140">Díky tomu se tato funkce zvažuje i u omezené cílové skupiny.</span><span class="sxs-lookup"><span data-stu-id="538cd-140">This makes the feature worth considering even with the limited audience.</span></span>

## <a name="alternatives"></a><span data-ttu-id="538cd-141">Alternativy</span><span class="sxs-lookup"><span data-stu-id="538cd-141">Alternatives</span></span>
[alternatives]: #alternatives

<span data-ttu-id="538cd-142">Je potřeba vzít v úvahu několik alternativ:</span><span class="sxs-lookup"><span data-stu-id="538cd-142">There are a couple of alternatives to consider:</span></span>

- <span data-ttu-id="538cd-143">Stav quo: funkce není odůvodněná na své vlastní vlastnosti a vývojáři dál používají chování implicitního výslovného souhlasu.</span><span class="sxs-lookup"><span data-stu-id="538cd-143">The status quo:  The feature is not justified on its own merits and developers continue to use the implicit opt in behavior.</span></span>

## <a name="questions"></a><span data-ttu-id="538cd-144">Otázky</span><span class="sxs-lookup"><span data-stu-id="538cd-144">Questions</span></span>
[quesions]: #questions

### <a name="metadata-representation"></a><span data-ttu-id="538cd-145">Reprezentace metadat</span><span class="sxs-lookup"><span data-stu-id="538cd-145">Metadata Representation</span></span>

<span data-ttu-id="538cd-146">F# Jazyk kóduje omezení v souboru signatury, což znamená C# , že nelze znovu použít jejich reprezentaci.</span><span class="sxs-lookup"><span data-stu-id="538cd-146">The F# language encodes the constraint in the signature file which means C# cannot re-use their representation.</span></span> <span data-ttu-id="538cd-147">Pro toto omezení bude nutné vybrat nový atribut.</span><span class="sxs-lookup"><span data-stu-id="538cd-147">A new attribute will need to be chosen for this constraint.</span></span> <span data-ttu-id="538cd-148">Kromě toho musí být metoda, která má toto omezení, chráněná pomocí mod-REQ.</span><span class="sxs-lookup"><span data-stu-id="538cd-148">Additionally a method which has this constraint must be protected by a mod-req.</span></span>

### <a name="blittable-vs-unmanaged"></a><span data-ttu-id="538cd-149">Nepřenositelný vs. nespravovaný</span><span class="sxs-lookup"><span data-stu-id="538cd-149">Blittable vs. Unmanaged</span></span>
<span data-ttu-id="538cd-150">F# Jazyk má velmi [podobnou funkci](https://docs.microsoft.com/dotnet/articles/fsharp/language-reference/generics/constraints) , která používá nespravovaný klíčová slova.</span><span class="sxs-lookup"><span data-stu-id="538cd-150">The F# language has a very [similar feature](https://docs.microsoft.com/dotnet/articles/fsharp/language-reference/generics/constraints) which uses the keyword unmanaged.</span></span> <span data-ttu-id="538cd-151">Název přenositeli pochází z použití v Midori.</span><span class="sxs-lookup"><span data-stu-id="538cd-151">The blittable name comes from the use in Midori.</span></span>  <span data-ttu-id="538cd-152">Možná budete chtít, aby tady vypadala priorita a místo toho používala nespravované.</span><span class="sxs-lookup"><span data-stu-id="538cd-152">May want to look to precedence here and use unmanaged instead.</span></span> 

<span data-ttu-id="538cd-153">**Řešení** Jazyk, který se rozhodne použít jako nespravovaný</span><span class="sxs-lookup"><span data-stu-id="538cd-153">**Resolution** The language decide to use unmanaged</span></span> 

### <a name="verifier"></a><span data-ttu-id="538cd-154">Verifier</span><span class="sxs-lookup"><span data-stu-id="538cd-154">Verifier</span></span>

<span data-ttu-id="538cd-155">Je nutné aktualizovat ověřovatel/modul runtime, aby bylo možné pochopit použití ukazatelů na Obecné parametry typu?</span><span class="sxs-lookup"><span data-stu-id="538cd-155">Does the verifier / runtime need to be updated to understand the use of pointers to generic type parameters?</span></span>  <span data-ttu-id="538cd-156">Nebo může fungovat jenom tak, jak je beze změn?</span><span class="sxs-lookup"><span data-stu-id="538cd-156">Or can it simply work as is without changes?</span></span>

<span data-ttu-id="538cd-157">**Řešení** Nepotřebujete žádné změny.</span><span class="sxs-lookup"><span data-stu-id="538cd-157">**Resolution** No changes needed.</span></span> <span data-ttu-id="538cd-158">Všechny typy ukazatelů jsou jednoduše neověřené.</span><span class="sxs-lookup"><span data-stu-id="538cd-158">All pointer types are simply unverifiable.</span></span> 

## <a name="design-meetings"></a><span data-ttu-id="538cd-159">Schůzky pro návrh</span><span class="sxs-lookup"><span data-stu-id="538cd-159">Design meetings</span></span>

<span data-ttu-id="538cd-160">neuvedeno</span><span class="sxs-lookup"><span data-stu-id="538cd-160">n/a</span></span>
