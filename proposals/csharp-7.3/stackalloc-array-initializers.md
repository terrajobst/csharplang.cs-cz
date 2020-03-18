---
ms.openlocfilehash: 7ea62713416ef82034963aef06f3cb11703342ed
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484606"
---
# <a name="stackalloc-array-initializers"></a><span data-ttu-id="17859-101">Inicializátory pole stackalloc</span><span class="sxs-lookup"><span data-stu-id="17859-101">Stackalloc array initializers</span></span>

## <a name="summary"></a><span data-ttu-id="17859-102">Souhrn</span><span class="sxs-lookup"><span data-stu-id="17859-102">Summary</span></span>
[summary]: #summary

<span data-ttu-id="17859-103">Povolí použití syntaxe inicializátoru pole s `stackalloc`.</span><span class="sxs-lookup"><span data-stu-id="17859-103">Allow array initializer syntax to be used with `stackalloc`.</span></span>

## <a name="motivation"></a><span data-ttu-id="17859-104">Motivační</span><span class="sxs-lookup"><span data-stu-id="17859-104">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="17859-105">Běžná pole mohou mít své prvky inicializovány při vytváření.</span><span class="sxs-lookup"><span data-stu-id="17859-105">Ordinary arrays can have their elements initialized at creation time.</span></span> <span data-ttu-id="17859-106">Jeví se jako přijatelné, aby to bylo možné v `stackalloc`m případě.</span><span class="sxs-lookup"><span data-stu-id="17859-106">It seems reasonable to allow that in `stackalloc` case.</span></span>

<span data-ttu-id="17859-107">Otázka, proč taková syntaxe není povolená, `stackalloc` se poměrně často vyskytuje.</span><span class="sxs-lookup"><span data-stu-id="17859-107">The question of why such syntax is not allowed with `stackalloc` arises fairly frequently.</span></span>  
<span data-ttu-id="17859-108">Podívejte se například [#1112](https://github.com/dotnet/csharplang/issues/1112)</span><span class="sxs-lookup"><span data-stu-id="17859-108">See, for example, [#1112](https://github.com/dotnet/csharplang/issues/1112)</span></span>

## <a name="detailed-design"></a><span data-ttu-id="17859-109">Podrobný návrh</span><span class="sxs-lookup"><span data-stu-id="17859-109">Detailed design</span></span>

<span data-ttu-id="17859-110">Běžná pole lze vytvořit pomocí následující syntaxe:</span><span class="sxs-lookup"><span data-stu-id="17859-110">Ordinary arrays can be created through the following syntax:</span></span>

```csharp
new int[3]
new int[3] { 1, 2, 3 }
new int[] { 1, 2, 3 }
new[] { 1, 2, 3 }
```

<span data-ttu-id="17859-111">Měli byste dovolit, aby pole přidělená zásobníkem byla vytvořená prostřednictvím:</span><span class="sxs-lookup"><span data-stu-id="17859-111">We should allow stack allocated arrays be created through:</span></span>  

```csharp
stackalloc int[3]               // currently allowed
stackalloc int[3] { 1, 2, 3 }
stackalloc int[] { 1, 2, 3 }
stackalloc[] { 1, 2, 3 }
```

<span data-ttu-id="17859-112">Sémantika všech případů je přibližně stejná jako u polí.</span><span class="sxs-lookup"><span data-stu-id="17859-112">The semantics of all cases is roughly the same as with arrays.</span></span>  
<span data-ttu-id="17859-113">Příklad: v posledním případě je typ elementu odvozený od inicializátoru a musí se jednat o "nespravovaný" typ.</span><span class="sxs-lookup"><span data-stu-id="17859-113">For example: in the last case the element type is inferred from the initializer and must be an "unmanaged" type.</span></span>

<span data-ttu-id="17859-114">Poznámka: Tato funkce není závislá na cíli, který je `Span<T>`.</span><span class="sxs-lookup"><span data-stu-id="17859-114">NOTE: the feature is not dependent on the target being a `Span<T>`.</span></span> <span data-ttu-id="17859-115">Je to v `T*`, jak je to možné, takže se nejeví jako predikát v případě `Span<T>`ho případu rozumný.</span><span class="sxs-lookup"><span data-stu-id="17859-115">It is just as applicable in `T*` case, so it does not seem reasonable to predicate it on `Span<T>` case.</span></span>  

## <a name="translation"></a><span data-ttu-id="17859-116">Překlad</span><span class="sxs-lookup"><span data-stu-id="17859-116">Translation</span></span>

<span data-ttu-id="17859-117">Implementace Naive by mohla inicializovat pole hned po vytvoření prostřednictvím řady přiřazení prvků.</span><span class="sxs-lookup"><span data-stu-id="17859-117">The naive implementation could just initialize the array right after creation through a series of element-wise assignments.</span></span>  

<span data-ttu-id="17859-118">Podobně jako v případě polí může být možné a žádoucí detekovat případy, kde jsou všechny nebo většinu prvků typu přenositelné, a používejte efektivnější techniky kopírováním v předem vytvořeném stavu všech konstantních prvků.</span><span class="sxs-lookup"><span data-stu-id="17859-118">Similarly to the case with arrays, it might be possible and desirable to detect cases where all or most of the elements are blittable types and use more efficient techniques by copying over the pre-created state of all the constant elements.</span></span> 

## <a name="drawbacks"></a><span data-ttu-id="17859-119">Nevýhody</span><span class="sxs-lookup"><span data-stu-id="17859-119">Drawbacks</span></span>
[drawbacks]: #drawbacks

## <a name="alternatives"></a><span data-ttu-id="17859-120">Alternativy</span><span class="sxs-lookup"><span data-stu-id="17859-120">Alternatives</span></span>
[alternatives]: #alternatives

<span data-ttu-id="17859-121">Tato funkce je pohodlná.</span><span class="sxs-lookup"><span data-stu-id="17859-121">This is a convenience feature.</span></span> <span data-ttu-id="17859-122">Je možné pouze Neprovádět žádnou akci.</span><span class="sxs-lookup"><span data-stu-id="17859-122">It is possible to just do nothing.</span></span>

## <a name="unresolved-questions"></a><span data-ttu-id="17859-123">Nevyřešené dotazy</span><span class="sxs-lookup"><span data-stu-id="17859-123">Unresolved questions</span></span>
[unresolved]: #unresolved-questions

## <a name="design-meetings"></a><span data-ttu-id="17859-124">Schůzky pro návrh</span><span class="sxs-lookup"><span data-stu-id="17859-124">Design meetings</span></span>

<span data-ttu-id="17859-125">Žádná ještě není.</span><span class="sxs-lookup"><span data-stu-id="17859-125">None yet.</span></span> 
