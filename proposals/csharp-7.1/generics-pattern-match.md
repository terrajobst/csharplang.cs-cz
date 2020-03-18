---
ms.openlocfilehash: 4f66c0f60d05ed6509a1d0843318a71d1b36c351
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484704"
---
# <a name="pattern-matching-with-generics"></a><span data-ttu-id="196e1-101">porovnávání vzorů s obecnými typy</span><span class="sxs-lookup"><span data-stu-id="196e1-101">pattern-matching with generics</span></span>

* <span data-ttu-id="196e1-102">Navrženo [x]</span><span class="sxs-lookup"><span data-stu-id="196e1-102">[x] Proposed</span></span>
* <span data-ttu-id="196e1-103">[] Prototyp:</span><span class="sxs-lookup"><span data-stu-id="196e1-103">[ ] Prototype:</span></span>
* <span data-ttu-id="196e1-104">[] Implementace:</span><span class="sxs-lookup"><span data-stu-id="196e1-104">[ ] Implementation:</span></span>
* <span data-ttu-id="196e1-105">[] Specifikace:</span><span class="sxs-lookup"><span data-stu-id="196e1-105">[ ] Specification:</span></span>

## <a name="summary"></a><span data-ttu-id="196e1-106">Souhrn</span><span class="sxs-lookup"><span data-stu-id="196e1-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="196e1-107">Specifikace pro [ C# existující operátor as](../../spec/expressions.md#the-as-operator) umožňuje, aby nedocházelo k převodu mezi typem operandu a zadaným typem, pokud je buď otevřený typ.</span><span class="sxs-lookup"><span data-stu-id="196e1-107">The specification for the [existing C# as operator](../../spec/expressions.md#the-as-operator) permits there to be no conversion between the type of the operand and the specified type when either is an open type.</span></span> <span data-ttu-id="196e1-108">Ve C# 7 vzor `Type identifier` ale vyžaduje převod mezi typem vstupu a daným typem.</span><span class="sxs-lookup"><span data-stu-id="196e1-108">However, in C# 7 the `Type identifier` pattern requires there be a conversion between the type of the input and the given type.</span></span>

<span data-ttu-id="196e1-109">Navrhujeme zmírnit tuto možnost a změnit `expression is Type identifier`, a to i v případě, že jsou povolené v podmínkách, pokud C# je povolená v 7, a taky povolit, pokud by `expression as Type` povolené.</span><span class="sxs-lookup"><span data-stu-id="196e1-109">We propose to relax this and change `expression is Type identifier`, in addition to being permitted in the conditions when it is permitted in C# 7, to also be permitted when `expression as Type` would be allowed.</span></span> <span data-ttu-id="196e1-110">Konkrétně nové případy jsou případy, kdy je typ výrazu nebo zadaného typu otevřený.</span><span class="sxs-lookup"><span data-stu-id="196e1-110">Specifically, the new cases are cases where the type of the expression or the specified type is an open type.</span></span> 

## <a name="motivation"></a><span data-ttu-id="196e1-111">Motivační</span><span class="sxs-lookup"><span data-stu-id="196e1-111">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="196e1-112">Případy, kdy by porovnávání vzorů mělo být "zjevně" povoleno v současnosti není možné kompilovat.</span><span class="sxs-lookup"><span data-stu-id="196e1-112">Cases where pattern-matching should "obviously" be permitted currently fail to compile.</span></span> <span data-ttu-id="196e1-113">Podívejte se například https://github.com/dotnet/roslyn/issues/16195.</span><span class="sxs-lookup"><span data-stu-id="196e1-113">See, for example, https://github.com/dotnet/roslyn/issues/16195.</span></span>

## <a name="detailed-design"></a><span data-ttu-id="196e1-114">Podrobný návrh</span><span class="sxs-lookup"><span data-stu-id="196e1-114">Detailed design</span></span>
[design]: #detailed-design

<span data-ttu-id="196e1-115">Změnili jsme odstavec ve specifikaci porovnávání vzorů (navržené přidání je zobrazeno tučně):</span><span class="sxs-lookup"><span data-stu-id="196e1-115">We change the paragraph in the pattern-matching specification (the proposed addition is shown in bold):</span></span>

> <span data-ttu-id="196e1-116">Určité kombinace statického typu na levé straně a daného typu se považují za nekompatibilní a výsledkem je chyba při kompilaci.</span><span class="sxs-lookup"><span data-stu-id="196e1-116">Certain combinations of static type of the left-hand-side and the given type are considered incompatible and result in compile-time error.</span></span> <span data-ttu-id="196e1-117">Hodnota statického typu `E` je označována jako *vzor kompatibilní* s typem `T`, pokud existuje převod identity, implicitní převod odkazu, převod zabalení, explicitní převod odkazu nebo převod rozbalení z `E` na `T`**nebo pokud je buď `E` nebo `T` otevřeným typem**.</span><span class="sxs-lookup"><span data-stu-id="196e1-117">A value of static type `E` is said to be *pattern compatible* with the type `T` if there exists an identity conversion, an implicit reference conversion, a boxing conversion, an explicit reference conversion, or an unboxing conversion from `E` to `T`**, or if either `E` or `T` is an open type**.</span></span> <span data-ttu-id="196e1-118">Jedná se o chybu při kompilaci, pokud výraz typu `E` není kompatibilní se vzorem typu v rámci vzoru typu, se kterým se shoduje.</span><span class="sxs-lookup"><span data-stu-id="196e1-118">It is a compile-time error if an expression of type `E` is not pattern compatible with the type in a type pattern that it is matched with.</span></span>

## <a name="drawbacks"></a><span data-ttu-id="196e1-119">Nevýhody</span><span class="sxs-lookup"><span data-stu-id="196e1-119">Drawbacks</span></span>
[drawbacks]: #drawbacks

<span data-ttu-id="196e1-120">Žádné.</span><span class="sxs-lookup"><span data-stu-id="196e1-120">None.</span></span>

## <a name="alternatives"></a><span data-ttu-id="196e1-121">Alternativy</span><span class="sxs-lookup"><span data-stu-id="196e1-121">Alternatives</span></span>
[alternatives]: #alternatives

<span data-ttu-id="196e1-122">Žádné.</span><span class="sxs-lookup"><span data-stu-id="196e1-122">None.</span></span>

## <a name="unresolved-questions"></a><span data-ttu-id="196e1-123">Nevyřešené dotazy</span><span class="sxs-lookup"><span data-stu-id="196e1-123">Unresolved questions</span></span>
[unresolved]: #unresolved-questions

<span data-ttu-id="196e1-124">Žádné.</span><span class="sxs-lookup"><span data-stu-id="196e1-124">None.</span></span>

## <a name="design-meetings"></a><span data-ttu-id="196e1-125">Schůzky pro návrh</span><span class="sxs-lookup"><span data-stu-id="196e1-125">Design meetings</span></span>

<span data-ttu-id="196e1-126">LOGICKÝ disk se považuje za tuto otázku a zjistil, že došlo ke změně úrovně opravit chybu.</span><span class="sxs-lookup"><span data-stu-id="196e1-126">LDM considered this question and felt it was a bug-fix level change.</span></span> <span data-ttu-id="196e1-127">Zpracováváme ji jako samostatnou jazykovou funkci, protože pouhá změna po vydání jazyka by mohla představovat dopřednou nekompatibilitu.</span><span class="sxs-lookup"><span data-stu-id="196e1-127">We are treating it as a separate language feature because just making the change after the language has been released would introduce a forward incompatibility.</span></span> <span data-ttu-id="196e1-128">Použití navrhované změny vyžaduje, aby programátor určil jazykovou verzi 7,1.</span><span class="sxs-lookup"><span data-stu-id="196e1-128">Using the proposed change requires that the programmer specify language version 7.1.</span></span>
