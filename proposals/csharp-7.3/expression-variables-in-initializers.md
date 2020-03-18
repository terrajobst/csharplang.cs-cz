---
ms.openlocfilehash: a78567594d39fc4e204e12c4f2f247b8d6995c38
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484921"
---
# <a name="expression-variables-in-initializers"></a><span data-ttu-id="d9a54-101">Proměnné výrazů v inicializátorech</span><span class="sxs-lookup"><span data-stu-id="d9a54-101">Expression variables in initializers</span></span>

## <a name="summary"></a><span data-ttu-id="d9a54-102">Souhrn</span><span class="sxs-lookup"><span data-stu-id="d9a54-102">Summary</span></span>
[summary]: #summary

<span data-ttu-id="d9a54-103">Funkce, které jsou představené C# v 7, rozšiřujeme tak, aby povolovaly výrazy obsahující proměnné výrazu (out deklarace proměnných a vzory deklarace) v inicializátorech polí, inicializátorech vlastností, inicializátorech ctor a klauzulích dotazu.</span><span class="sxs-lookup"><span data-stu-id="d9a54-103">We extend the features introduced in C# 7 to permit expressions containing expression variables (out variable declarations and declaration patterns) in field initializers, property initializers, ctor-initializers, and query clauses.</span></span>

## <a name="motivation"></a><span data-ttu-id="d9a54-104">Motivační</span><span class="sxs-lookup"><span data-stu-id="d9a54-104">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="d9a54-105">Tím se dokončí některé z hrubých okrajů zbývajících C# v jazyce z důvodu nedostatku času.</span><span class="sxs-lookup"><span data-stu-id="d9a54-105">This completes a couple of the rough edges left in the C# language due to lack of time.</span></span>

## <a name="detailed-design"></a><span data-ttu-id="d9a54-106">Podrobný návrh</span><span class="sxs-lookup"><span data-stu-id="d9a54-106">Detailed design</span></span>
[design]: #detailed-design

<span data-ttu-id="d9a54-107">Odstraníme omezení zabraňující deklaraci proměnných výrazu (deklarace proměnných a vzory deklarace) v inicializátoru ctor.</span><span class="sxs-lookup"><span data-stu-id="d9a54-107">We remove the restriction preventing the declaration of expression variables (out variable declarations and declaration patterns) in a ctor-initializer.</span></span> <span data-ttu-id="d9a54-108">Taková deklarovaná proměnná je v oboru v celém těle konstruktoru.</span><span class="sxs-lookup"><span data-stu-id="d9a54-108">Such a declared variable is in scope throughout the body of the constructor.</span></span>

<span data-ttu-id="d9a54-109">Odstraníme omezení, které brání deklaraci proměnných výrazů (deklarace proměnných out a vzory deklarace) v inicializátoru pole nebo vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="d9a54-109">We remove the restriction preventing the declaration of expression variables (out variable declarations and declaration patterns) in a field or property initializer.</span></span> <span data-ttu-id="d9a54-110">Taková deklarovaná proměnná je v oboru celého inicializačního výrazu.</span><span class="sxs-lookup"><span data-stu-id="d9a54-110">Such a declared variable is in scope throughout the initializing expression.</span></span>

<span data-ttu-id="d9a54-111">Odebereme omezení zabraňující deklaraci proměnných výrazů (deklarace proměnných out a vzory deklarace) v klauzuli výrazu dotazu, která je přeložena do těla výrazu lambda.</span><span class="sxs-lookup"><span data-stu-id="d9a54-111">We remove the restriction preventing the declaration of expression variables (out variable declarations and declaration patterns) in a query expression clause that is translated into the body of a lambda.</span></span> <span data-ttu-id="d9a54-112">Taková deklarovaná proměnná je v oboru celého výrazu klauzule dotazu.</span><span class="sxs-lookup"><span data-stu-id="d9a54-112">Such a declared variable is in scope throughout that expression of the query clause.</span></span>

## <a name="drawbacks"></a><span data-ttu-id="d9a54-113">Nevýhody</span><span class="sxs-lookup"><span data-stu-id="d9a54-113">Drawbacks</span></span>
[drawbacks]: #drawbacks

<span data-ttu-id="d9a54-114">Žádné.</span><span class="sxs-lookup"><span data-stu-id="d9a54-114">None.</span></span>

## <a name="alternatives"></a><span data-ttu-id="d9a54-115">Alternativy</span><span class="sxs-lookup"><span data-stu-id="d9a54-115">Alternatives</span></span>
[alternatives]: #alternatives

<span data-ttu-id="d9a54-116">Příslušný rozsah proměnných výrazů deklarovaných v těchto kontextech není zjevný a zachovává další diskuzi LDM.</span><span class="sxs-lookup"><span data-stu-id="d9a54-116">The appropriate scope for expression variables declared in these contexts is not obvious, and deserves further LDM discussion.</span></span>

## <a name="unresolved-questions"></a><span data-ttu-id="d9a54-117">Nevyřešené dotazy</span><span class="sxs-lookup"><span data-stu-id="d9a54-117">Unresolved questions</span></span>
[unresolved]: #unresolved-questions

- <span data-ttu-id="d9a54-118">[] Jaký je příslušný obor pro tyto proměnné?</span><span class="sxs-lookup"><span data-stu-id="d9a54-118">[ ] What is the appropriate scope for these variables?</span></span>

## <a name="design-meetings"></a><span data-ttu-id="d9a54-119">Schůzky pro návrh</span><span class="sxs-lookup"><span data-stu-id="d9a54-119">Design meetings</span></span>

<span data-ttu-id="d9a54-120">Žádné.</span><span class="sxs-lookup"><span data-stu-id="d9a54-120">None.</span></span>
