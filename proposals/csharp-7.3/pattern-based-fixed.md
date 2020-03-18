---
ms.openlocfilehash: 2070cf3b3269585055791adc3427cbd134df444d
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484655"
---
# <a name="pattern-based-fixed-statement"></a><span data-ttu-id="3f99e-101">Příkaz `fixed` na základě vzoru</span><span class="sxs-lookup"><span data-stu-id="3f99e-101">Pattern-based `fixed` statement</span></span>

## <a name="summary"></a><span data-ttu-id="3f99e-102">Souhrn</span><span class="sxs-lookup"><span data-stu-id="3f99e-102">Summary</span></span>
[summary]: #summary

<span data-ttu-id="3f99e-103">Zaveďte vzor, který umožní, aby se typy účastnily `fixed` příkazy.</span><span class="sxs-lookup"><span data-stu-id="3f99e-103">Introduce a pattern that would allow types to participate in `fixed` statements.</span></span> 

## <a name="motivation"></a><span data-ttu-id="3f99e-104">Motivační</span><span class="sxs-lookup"><span data-stu-id="3f99e-104">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="3f99e-105">Jazyk poskytuje mechanismus pro připnutí spravovaných dat a získání nativního ukazatele na podkladovou vyrovnávací paměť.</span><span class="sxs-lookup"><span data-stu-id="3f99e-105">The language provides a mechanism for pinning managed data and obtain a native pointer to the underlying buffer.</span></span>

```csharp
fixed(byte* ptr = byteArray)
{
   // ptr is a native pointer to the first element of the array
   // byteArray is protected from being moved/collected by the GC for the duration of this block 
}

```

<span data-ttu-id="3f99e-106">Sada typů, které se mohou účastnit `fixed`, je pevně zakódované a omezena na pole a `System.String`.</span><span class="sxs-lookup"><span data-stu-id="3f99e-106">The set of types that can participate in `fixed` is hardcoded and limited to arrays and `System.String`.</span></span> <span data-ttu-id="3f99e-107">Zakódujeme "Special" typy se neškálují, když jsou zavedeny nové primitivní prvky jako `ImmutableArray<T>`, `Span<T>``Utf8String`.</span><span class="sxs-lookup"><span data-stu-id="3f99e-107">Hardcoding "special" types does not scale when new primitives such as `ImmutableArray<T>`, `Span<T>`, `Utf8String` are introduced.</span></span> 

<span data-ttu-id="3f99e-108">Kromě toho aktuální řešení pro `System.String` spoléhá na poměrně tuhé rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="3f99e-108">In addition, the current solution for `System.String` relies on a fairly rigid API.</span></span> <span data-ttu-id="3f99e-109">Tvar rozhraní API znamená, že `System.String` je souvislý objekt, který vkládá UTF16 kódovaná data s pevným posunem z hlavičky objektu.</span><span class="sxs-lookup"><span data-stu-id="3f99e-109">The shape of the API implies that `System.String` is a contiguous object that embeds UTF16 encoded data at a fixed offset from the object header.</span></span> <span data-ttu-id="3f99e-110">Takový přístup se našel problematický v několika návrzích, které by mohly vyžadovat změny v podkladovém rozložení.</span><span class="sxs-lookup"><span data-stu-id="3f99e-110">Such approach has been found problematic in several proposals that could require changes to the underlying layout.</span></span> <span data-ttu-id="3f99e-111">Je žádoucí, aby bylo možné přepnout na flexibilnější flexibilitu, která odděluje `System.String` objekt od jeho interní reprezentace za účelem nespravovaného interoperability.</span><span class="sxs-lookup"><span data-stu-id="3f99e-111">It would be desirable to be able to switch to something more flexible that decouples `System.String` object from its internal representation for the purpose of unmanaged interop.</span></span> 

## <a name="detailed-design"></a><span data-ttu-id="3f99e-112">Podrobný návrh</span><span class="sxs-lookup"><span data-stu-id="3f99e-112">Detailed design</span></span>
[design]: #detailed-design

## <a name="pattern"></a><span data-ttu-id="3f99e-113">*Vzor*</span><span class="sxs-lookup"><span data-stu-id="3f99e-113">*Pattern*</span></span> ##
<span data-ttu-id="3f99e-114">Životaschopná "pevná", která je založená na vzorovém, musí:</span><span class="sxs-lookup"><span data-stu-id="3f99e-114">A viable pattern-based “fixed” need to:</span></span>
-   <span data-ttu-id="3f99e-115">Poskytněte spravované odkazy pro připnutí instance a inicializaci ukazatele (Pokud je to ten, co je stejný odkaz).</span><span class="sxs-lookup"><span data-stu-id="3f99e-115">Provide the managed references to pin the instance and to initialize the pointer (preferably this is the same reference)</span></span>
-   <span data-ttu-id="3f99e-116">Vyjádřit jednoznačně typ nespravovaného elementu (tj. "char" pro "String")</span><span class="sxs-lookup"><span data-stu-id="3f99e-116">Convey unambiguously the type of the unmanaged element   (i.e. “char” for “string”)</span></span>
-   <span data-ttu-id="3f99e-117">Nařídí chování prázdného případu, pokud neexistuje žádný odkaz na.</span><span class="sxs-lookup"><span data-stu-id="3f99e-117">Prescribe the behavior in "empty" case when there is nothing to refer to.</span></span> 
-   <span data-ttu-id="3f99e-118">Neměl by vytvářet autory rozhraní API pro rozhodování o návrhu, které snížit použití typu mimo `fixed`.</span><span class="sxs-lookup"><span data-stu-id="3f99e-118">Should not push API authors toward design decisions that hurt the use of the type outside of `fixed`.</span></span>

<span data-ttu-id="3f99e-119">Domnívám se, že výše uvedená by mohla být splněna rozpoznáváním speciálně pojmenovaného člena vracejícího odkaz: `ref [readonly] T GetPinnableReference()`.</span><span class="sxs-lookup"><span data-stu-id="3f99e-119">I think the above could be satisfied by recognizing a specially named ref-returning member: `ref [readonly] T GetPinnableReference()`.</span></span>

<span data-ttu-id="3f99e-120">Aby bylo možné použít příkaz `fixed`, musí být splněny následující podmínky:</span><span class="sxs-lookup"><span data-stu-id="3f99e-120">In order to be used by the `fixed` statement the following conditions must be met:</span></span>

1. <span data-ttu-id="3f99e-121">Pro typ je k dispozici pouze jeden takový člen.</span><span class="sxs-lookup"><span data-stu-id="3f99e-121">There is only one such member provided for a type.</span></span>
1. <span data-ttu-id="3f99e-122">Vrátí `ref` nebo `ref readonly`.</span><span class="sxs-lookup"><span data-stu-id="3f99e-122">Returns by `ref` or `ref readonly`.</span></span> <span data-ttu-id="3f99e-123">(`readonly` je povolená, aby autoři neměnných/ReadOnly typů mohli implementovat vzor bez přidání zapisovatelného rozhraní API, které by bylo možné použít v bezpečném kódu)</span><span class="sxs-lookup"><span data-stu-id="3f99e-123">(`readonly` is permitted so that authors of immutable/readonly types could implement the pattern without adding writeable API that could be used in safe code)</span></span>
1. <span data-ttu-id="3f99e-124">T je nespravovaný typ.</span><span class="sxs-lookup"><span data-stu-id="3f99e-124">T is an unmanaged type.</span></span>
<span data-ttu-id="3f99e-125">(vzhledem k tomu, že `T*` se změní na typ ukazatele.</span><span class="sxs-lookup"><span data-stu-id="3f99e-125">(since `T*` becomes the pointer type.</span></span> <span data-ttu-id="3f99e-126">Omezení se přirozeně rozšíří, pokud je rozbalený pojem "nespravovaný".</span><span class="sxs-lookup"><span data-stu-id="3f99e-126">The restriction will naturally expand if/when the notion of "unmanaged" is expanded)</span></span>
1. <span data-ttu-id="3f99e-127">Vrátí spravované `nullptr`, když není k dispozici žádná data k připnutí – pravděpodobně nejlevnější způsob, jak vyjádřit vyprázdnění.</span><span class="sxs-lookup"><span data-stu-id="3f99e-127">Returns managed `nullptr` when there is no data to pin – probably the cheapest way to convey emptiness.</span></span>
<span data-ttu-id="3f99e-128">(Všimněte si, že řetězec "" vrátí odkaz na \ 0, protože řetězce jsou zakončené hodnotou null).</span><span class="sxs-lookup"><span data-stu-id="3f99e-128">(note that “” string returns a ref to '\0' since strings are null-terminated)</span></span>

<span data-ttu-id="3f99e-129">Případně pro `#3` můžeme výsledek v prázdných případech nechat nedefinovaný nebo specifický pro implementaci.</span><span class="sxs-lookup"><span data-stu-id="3f99e-129">Alternatively for the `#3` we can allow the result in empty cases be undefined or implementation-specific.</span></span> <span data-ttu-id="3f99e-130">To však může zajistit, že rozhraní API bude bezpečnější a náchylné k zneužití a nezamýšlenému zatížení kompatibility.</span><span class="sxs-lookup"><span data-stu-id="3f99e-130">That, however, may make the API more dangerous and prone to abuse and unintended compatibility burdens.</span></span> 

## <a name="translation"></a><span data-ttu-id="3f99e-131">*Překlad*</span><span class="sxs-lookup"><span data-stu-id="3f99e-131">*Translation*</span></span> ##

```csharp
fixed(byte* ptr = thing)
{ 
    // <BODY>
}
```

<span data-ttu-id="3f99e-132">se bude jednat o následující pseudokódu (ne všechny C#vyhodnotit v)</span><span class="sxs-lookup"><span data-stu-id="3f99e-132">becomes the following pseudocode (not all expressible in C#)</span></span>

```csharp
byte* ptr;
// specially decorated "pinned" IL local slot, not visible to user code.
pinned ref byte _pinned;

try
{
    // NOTE: null check is omitted for value types 
    // NOTE: `thing` is evaluated only once (temporary is introduced if necessary) 
    if (thing != null)
    {
        // obtain and "pin" the reference
        _pinned = ref thing.GetPinnableReference();

        // unsafe cast in IL
        ptr = (byte*)_pinned;
    }
    else
    {
        ptr = default(byte*);
    }

    // <BODY> 
}
finally   // finally can be omitted when not observable
{
    // "unpin" the object
    _pinned = nullptr;
}
```

## <a name="drawbacks"></a><span data-ttu-id="3f99e-133">Nevýhody</span><span class="sxs-lookup"><span data-stu-id="3f99e-133">Drawbacks</span></span>
[drawbacks]: #drawbacks

- <span data-ttu-id="3f99e-134">GetPinnableReference je určena pouze pro použití v `fixed`, ale nic nebrání jeho použití v bezpečném kódu, takže implementátor musí mít na paměti.</span><span class="sxs-lookup"><span data-stu-id="3f99e-134">GetPinnableReference is intended to be used only in `fixed`, but nothing prevents its use in safe code, so implementor must keep that in mind.</span></span>

## <a name="alternatives"></a><span data-ttu-id="3f99e-135">Alternativy</span><span class="sxs-lookup"><span data-stu-id="3f99e-135">Alternatives</span></span>
[alternatives]: #alternatives

<span data-ttu-id="3f99e-136">Uživatelé mohou zavést GetPinnableReference nebo podobného člena a použít ho jako</span><span class="sxs-lookup"><span data-stu-id="3f99e-136">Users can introduce GetPinnableReference or similar member and use it as</span></span>
 
```csharp
fixed(byte* ptr = thing.GetPinnableReference())
{ 
    // <BODY>
}
```

<span data-ttu-id="3f99e-137">Pro `System.String` neexistuje žádné řešení, pokud je potřeba alternativní řešení.</span><span class="sxs-lookup"><span data-stu-id="3f99e-137">There is no solution for `System.String` if alternative solution is desired.</span></span>

## <a name="unresolved-questions"></a><span data-ttu-id="3f99e-138">Nevyřešené dotazy</span><span class="sxs-lookup"><span data-stu-id="3f99e-138">Unresolved questions</span></span>
[unresolved]: #unresolved-questions

- <span data-ttu-id="3f99e-139">[] Chování je ve stavu "Empty".</span><span class="sxs-lookup"><span data-stu-id="3f99e-139">[ ] Behavior in "empty" state.</span></span><span data-ttu-id="3f99e-140"> - `nullptr` nebo `undefined`?</span><span class="sxs-lookup"><span data-stu-id="3f99e-140"> - `nullptr` or `undefined` ?</span></span> 
- <span data-ttu-id="3f99e-141">[] By se měly považovat metody rozšíření?</span><span class="sxs-lookup"><span data-stu-id="3f99e-141">[ ] Should the extension methods be considered ?</span></span> 
- <span data-ttu-id="3f99e-142">[] Pokud se na `System.String`zjistí vzor, měl by se získat?</span><span class="sxs-lookup"><span data-stu-id="3f99e-142">[ ] If a pattern is detected on `System.String`, should it win over ?</span></span> 

## <a name="design-meetings"></a><span data-ttu-id="3f99e-143">Schůzky pro návrh</span><span class="sxs-lookup"><span data-stu-id="3f99e-143">Design meetings</span></span>

<span data-ttu-id="3f99e-144">Žádná ještě není.</span><span class="sxs-lookup"><span data-stu-id="3f99e-144">None yet.</span></span> 
