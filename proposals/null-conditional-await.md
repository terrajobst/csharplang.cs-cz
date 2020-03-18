---
ms.openlocfilehash: 3d10cacef98e802333c8cbe65edb93c19c74cabf
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484494"
---
# <a name="null-conditional-await"></a><span data-ttu-id="481ae-101">null – podmíněné čekání</span><span class="sxs-lookup"><span data-stu-id="481ae-101">null-conditional await</span></span>

* <span data-ttu-id="481ae-102">Navrženo [x]</span><span class="sxs-lookup"><span data-stu-id="481ae-102">[x] Proposed</span></span>
* <span data-ttu-id="481ae-103">[] Prototyp: none</span><span class="sxs-lookup"><span data-stu-id="481ae-103">[ ] Prototype: None</span></span>
* <span data-ttu-id="481ae-104">[] Implementace: žádné</span><span class="sxs-lookup"><span data-stu-id="481ae-104">[ ] Implementation: None</span></span>
* <span data-ttu-id="481ae-105">[] Specifikace: spuštěno, níže</span><span class="sxs-lookup"><span data-stu-id="481ae-105">[ ] Specification: Started, below</span></span>

## <a name="summary"></a><span data-ttu-id="481ae-106">Souhrn</span><span class="sxs-lookup"><span data-stu-id="481ae-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="481ae-107">Podporuje výraz formuláře `await? e`, který čeká `e`, pokud není null, jinak má za následek `null`.</span><span class="sxs-lookup"><span data-stu-id="481ae-107">Support an expression of the form `await? e`, which awaits `e` if it is non-null, otherwise it results in `null`.</span></span>

## <a name="motivation"></a><span data-ttu-id="481ae-108">Motivační</span><span class="sxs-lookup"><span data-stu-id="481ae-108">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="481ae-109">Toto je společný vzor pro kódování a tato funkce by mohla mít příjemné synergii se stávajícími operátory slučování s hodnotou null a s hodnotou null.</span><span class="sxs-lookup"><span data-stu-id="481ae-109">This is a common coding pattern, and this feature would have nice synergy with the existing null-propagating and null-coalescing operators.</span></span>

## <a name="detailed-design"></a><span data-ttu-id="481ae-110">Podrobný návrh</span><span class="sxs-lookup"><span data-stu-id="481ae-110">Detailed design</span></span>
[design]: #detailed-design

<span data-ttu-id="481ae-111">Přidáme novou formu *await_expression*:</span><span class="sxs-lookup"><span data-stu-id="481ae-111">We add a new form of the *await_expression*:</span></span>

```antlr
await_expression
    : 'await' '?' unary_expression
    ;
```

<span data-ttu-id="481ae-112">Operátor s hodnotou null-Conditional `await` čeká na operand pouze v případě, že tento operand je jiný než null.</span><span class="sxs-lookup"><span data-stu-id="481ae-112">The null-conditional `await` operator awaits its operand only if that operand is non-null.</span></span> <span data-ttu-id="481ae-113">V opačném případě je výsledek použití operátoru null.</span><span class="sxs-lookup"><span data-stu-id="481ae-113">Otherwise the result of applying the operator is null.</span></span>

<span data-ttu-id="481ae-114">Typ výsledku je vypočítán pomocí [pravidel pro operátor s hodnotou null-Conditioned](https://github.com/dotnet/csharplang/blob/master/spec/expressions.md#null-conditional-operator).</span><span class="sxs-lookup"><span data-stu-id="481ae-114">The type of the result is computed using the [rules for the null-conditional operator](https://github.com/dotnet/csharplang/blob/master/spec/expressions.md#null-conditional-operator).</span></span>

> <span data-ttu-id="481ae-115">**Poznámka:** Pokud je `e` typu `Task`, pak `await? e;` neudělá nic, pokud `e` `null`, a očekává `e`, pokud není `null`.</span><span class="sxs-lookup"><span data-stu-id="481ae-115">**NOTE:** If `e` is of type `Task`, then `await? e;` would do nothing if `e` is `null`, and await `e` if it is not `null`.</span></span>
>
> <span data-ttu-id="481ae-116">Pokud je `e` typu `Task<K>`, kde `K` je typ hodnoty, `await? e` by měla být hodnota typu `K?`.</span><span class="sxs-lookup"><span data-stu-id="481ae-116">If `e` is of type `Task<K>` where `K` is a value type, then `await? e` would yield a value of type `K?`.</span></span>

## <a name="drawbacks"></a><span data-ttu-id="481ae-117">Nevýhody</span><span class="sxs-lookup"><span data-stu-id="481ae-117">Drawbacks</span></span>
[drawbacks]: #drawbacks

<span data-ttu-id="481ae-118">Stejně jako u libovolné jazykové funkce musíme dotazovat, jestli se další složitost tohoto jazyka znovu vyplácí v další čistotě, která je k dispozici pro tělo C# programů, které by mohly využít výhod této funkce.</span><span class="sxs-lookup"><span data-stu-id="481ae-118">As with any language feature, we must question whether the additional complexity to the language is repaid in the additional clarity offered to the body of C# programs that would benefit from the feature.</span></span>

## <a name="alternatives"></a><span data-ttu-id="481ae-119">Alternativy</span><span class="sxs-lookup"><span data-stu-id="481ae-119">Alternatives</span></span>
[alternatives]: #alternatives

<span data-ttu-id="481ae-120">I když vyžaduje nějaký často používaný kód, použití tohoto operátoru lze často nahradit výrazem, jako je například `(e == null) ? null : await e` nebo příkaz jako `if (e != null) await e`.</span><span class="sxs-lookup"><span data-stu-id="481ae-120">Although it requires some boilerplate code, uses of this operator can often be replaced by an expression something like `(e == null) ? null : await e` or a statement like `if (e != null) await e`.</span></span>

## <a name="unresolved-questions"></a><span data-ttu-id="481ae-121">Nevyřešené dotazy</span><span class="sxs-lookup"><span data-stu-id="481ae-121">Unresolved questions</span></span>
[unresolved]: #unresolved-questions

- <span data-ttu-id="481ae-122">[] Vyžaduje revizi LDM.</span><span class="sxs-lookup"><span data-stu-id="481ae-122">[ ] Requires LDM review</span></span>

## <a name="design-meetings"></a><span data-ttu-id="481ae-123">Schůzky pro návrh</span><span class="sxs-lookup"><span data-stu-id="481ae-123">Design meetings</span></span>

<span data-ttu-id="481ae-124">Žádné.</span><span class="sxs-lookup"><span data-stu-id="481ae-124">None.</span></span>
