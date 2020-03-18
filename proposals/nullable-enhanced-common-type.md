---
ms.openlocfilehash: 032cb8590a0b6e83f8ab6201e10720f1b254c605
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484529"
---
# <a name="nullable-enhanced-common-type"></a><span data-ttu-id="6353a-101">Rozšířený běžný typ s možnou hodnotou null</span><span class="sxs-lookup"><span data-stu-id="6353a-101">Nullable-Enhanced Common Type</span></span>

* <span data-ttu-id="6353a-102">Navrženo [x]</span><span class="sxs-lookup"><span data-stu-id="6353a-102">[x] Proposed</span></span>
* <span data-ttu-id="6353a-103">[] Prototyp: none</span><span class="sxs-lookup"><span data-stu-id="6353a-103">[ ] Prototype: None</span></span>
* <span data-ttu-id="6353a-104">[] Implementace: žádné</span><span class="sxs-lookup"><span data-stu-id="6353a-104">[ ] Implementation: None</span></span>
* <span data-ttu-id="6353a-105">[] Specifikace: viz níže</span><span class="sxs-lookup"><span data-stu-id="6353a-105">[ ] Specification: See below</span></span>

## <a name="summary"></a><span data-ttu-id="6353a-106">Souhrn</span><span class="sxs-lookup"><span data-stu-id="6353a-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="6353a-107">Existuje situace, kdy aktuální výsledky algoritmu Common-Type jsou intuitivní a výsledky v programátorovi přidávají, jak se má redundantní přetypování do kódu.</span><span class="sxs-lookup"><span data-stu-id="6353a-107">There is a situation in which the current common-type algorithm results are counter-intuitive, and results in the programmer adding what feels like a redundant cast to the code.</span></span> <span data-ttu-id="6353a-108">Při této změně by výraz, jako je například `condition ? 1 : null`, měl za následek hodnotu typu `int?`a výraz, jako je například `condition ? x : 1.0`, kde `x` je typu `int?`, by způsobil hodnotu typu `double?`.</span><span class="sxs-lookup"><span data-stu-id="6353a-108">With this change, an expression such as `condition ? 1 : null` would result in a value of type `int?`, and an expression such as `condition ? x : 1.0` where `x` is of type `int?` would result in a value of type `double?`.</span></span>

## <a name="motivation"></a><span data-ttu-id="6353a-109">Motivační</span><span class="sxs-lookup"><span data-stu-id="6353a-109">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="6353a-110">Jedná se o běžnou příčinu toho, co je to pro programátora, jako je třeba nepotřebný často používaný kód.</span><span class="sxs-lookup"><span data-stu-id="6353a-110">This is a common cause of what feels to the programmer like needless boilerplate code.</span></span>

## <a name="detailed-design"></a><span data-ttu-id="6353a-111">Podrobný návrh</span><span class="sxs-lookup"><span data-stu-id="6353a-111">Detailed design</span></span>
[design]: #detailed-design

<span data-ttu-id="6353a-112">Změnou specifikace pro [hledání nejlepšího společného typu sady výrazů](https://github.com/dotnet/csharplang/blob/master/spec/expressions.md#finding-the-best-common-type-of-a-set-of-expressions) můžete ovlivnit následující situace:</span><span class="sxs-lookup"><span data-stu-id="6353a-112">We modify the specification for [finding the best common type of a set of expressions](https://github.com/dotnet/csharplang/blob/master/spec/expressions.md#finding-the-best-common-type-of-a-set-of-expressions) to affect the following situations:</span></span>

- <span data-ttu-id="6353a-113">Pokud je jeden výraz typu hodnoty bez hodnoty null `T` a druhý je literál null, výsledek je typu `T?`.</span><span class="sxs-lookup"><span data-stu-id="6353a-113">If one expression is of a non-nullable value type `T` and the other is a null literal, the result is of type `T?`.</span></span>
- <span data-ttu-id="6353a-114">Pokud je jeden výraz typu s možnou hodnotou null `T?` a druhý je hodnotový typ `U`a existuje implicitní převod z `T` na `U`, pak výsledek je typu `U?`.</span><span class="sxs-lookup"><span data-stu-id="6353a-114">If one expression is of a nullable value type `T?` and the other is of a value type `U`, and there is an implicit conversion from `T` to `U`, then the result is of type `U?`.</span></span>

<span data-ttu-id="6353a-115">Očekává se, že bude ovlivněno následující aspekty jazyka:</span><span class="sxs-lookup"><span data-stu-id="6353a-115">This is expected to affect the following aspects of the language:</span></span>

- <span data-ttu-id="6353a-116">[Ternární výraz](https://github.com/dotnet/csharplang/blob/master/spec/expressions.md#conditional-operator)</span><span class="sxs-lookup"><span data-stu-id="6353a-116">the [ternary expression](https://github.com/dotnet/csharplang/blob/master/spec/expressions.md#conditional-operator)</span></span>
- <span data-ttu-id="6353a-117">[výraz vytváření pole](https://github.com/dotnet/csharplang/blob/master/spec/expressions.md#array-creation-expressions) implicitně typovaného typu</span><span class="sxs-lookup"><span data-stu-id="6353a-117">implicitly typed [array creation expression](https://github.com/dotnet/csharplang/blob/master/spec/expressions.md#array-creation-expressions)</span></span>
- <span data-ttu-id="6353a-118">odvození [návratového typu výrazu lambda](https://github.com/dotnet/csharplang/blob/master/spec/expressions.md#inferred-return-type) pro odvození typu</span><span class="sxs-lookup"><span data-stu-id="6353a-118">inferring the [return type of a lambda](https://github.com/dotnet/csharplang/blob/master/spec/expressions.md#inferred-return-type) for type inference</span></span>
- <span data-ttu-id="6353a-119">případy zahrnující obecné typy, jako je například vyvolání `M<T>(T a, T b)` jako `M(1, null)`.</span><span class="sxs-lookup"><span data-stu-id="6353a-119">cases involving generics, such as invoking `M<T>(T a, T b)` as `M(1, null)`.</span></span>

<span data-ttu-id="6353a-120">V následujících oddílech specifikace měníme přesnější (vkládání tučně, mazání v přeškrtnutí):</span><span class="sxs-lookup"><span data-stu-id="6353a-120">More precisely, we change the following sections of the specification (insertions in bold, deletions in strikethrough):</span></span>

> #### <a name="output-type-inferences"></a><span data-ttu-id="6353a-121">Odvození typu výstupu</span><span class="sxs-lookup"><span data-stu-id="6353a-121">Output type inferences</span></span>
> 
> <span data-ttu-id="6353a-122">*Odvození typu výstupu* je provedeno *z* výrazu `E` *do* typu `T` následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="6353a-122">An *output type inference* is made *from* an expression `E` *to* a type `T` in the following way:</span></span>
> 
> *  <span data-ttu-id="6353a-123">Je-li `E` anonymní funkce s odvozeným návratovým typem `U` ([odvozený návratový typ](expressions.md#inferred-return-type)) a `T` je typu delegáta nebo stromové struktury výrazu s návratovým typem `Tb`, pak *odvozená odvozená odvozená* ([odvozená odvozená](expressions.md#lower-bound-inferences)) je vytvořena *z* `U` *až* `Tb`.</span><span class="sxs-lookup"><span data-stu-id="6353a-123">If `E` is an anonymous function with inferred return type  `U` ([Inferred return type](expressions.md#inferred-return-type)) and `T` is a delegate type or expression tree type with return type `Tb`, then a *lower-bound inference* ([Lower-bound inferences](expressions.md#lower-bound-inferences)) is made *from* `U` *to* `Tb`.</span></span>
> *  <span data-ttu-id="6353a-124">V opačném případě, *Pokud je `E`* jako skupina metod a `T` je typ delegáta nebo typ stromu výrazů s typy parametrů `T1...Tk` a návratovým typem `Tb`a řešení přetěžování `E` s typy `T1...Tk` poskytuje jedinou metodu s návratovým typem `U`, pak *z* `U` *na* `Tb`.</span><span class="sxs-lookup"><span data-stu-id="6353a-124">Otherwise, if `E` is a method group and `T` is a delegate type or expression tree type with parameter types `T1...Tk` and return type `Tb`, and overload resolution of `E` with the types `T1...Tk` yields a single method with return type `U`, then a *lower-bound inference* is made *from* `U` *to* `Tb`.</span></span>
> *  <span data-ttu-id="6353a-125">\* \* V opačném případě, pokud `E` je výraz s hodnotou Nullable typu `U?`, pak je *z* `U` *na* `T` proveden odvozený objekt pro *odvození* a *hodnota null* je přidána do `T`.</span><span class="sxs-lookup"><span data-stu-id="6353a-125">\*\*Otherwise, if `E` is an expression with nullable value type `U?`, then a *lower-bound inference* is made *from* `U` *to* `T` and a *null bound* is added to `T`.</span></span> **
> *  <span data-ttu-id="6353a-126">V opačném případě, pokud `E` je výraz s typem `U`, pak je *odvozeno odvození* *z* `U` *na* `T`.</span><span class="sxs-lookup"><span data-stu-id="6353a-126">Otherwise, if `E` is an expression with type `U`, then a *lower-bound inference* is made *from* `U` *to* `T`.</span></span>
> *  <span data-ttu-id="6353a-127">**V opačném případě, pokud je `E` konstantní výraz s hodnotou `null`, je do `T` přidána *vázaná hodnota null***</span><span class="sxs-lookup"><span data-stu-id="6353a-127">**Otherwise, if `E` is a constant expression with value `null`, then a *null bound* is added to `T`**</span></span> 
> *  <span data-ttu-id="6353a-128">Jinak nejsou provedeny žádné odvozené.</span><span class="sxs-lookup"><span data-stu-id="6353a-128">Otherwise, no inferences are made.</span></span>

> #### <a name="fixing"></a><span data-ttu-id="6353a-129">Opravě</span><span class="sxs-lookup"><span data-stu-id="6353a-129">Fixing</span></span>
> 
> <span data-ttu-id="6353a-130">*Nepevná* proměnná typu `Xi` se sadou hranic je *opravena* následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="6353a-130">An *unfixed* type variable `Xi` with a set of bounds is *fixed* as follows:</span></span>
> 
> *  <span data-ttu-id="6353a-131">Sada *typů kandidátů* `Uj` začíná jako sada všech typů v sadě mezí pro `Xi`.</span><span class="sxs-lookup"><span data-stu-id="6353a-131">The set of *candidate types* `Uj` starts out as the set of all types in the set of bounds for `Xi`.</span></span>
> *  <span data-ttu-id="6353a-132">Následně prověříme všechny vazby pro `Xi`: pro každý přesný vázaný `U` `Xi` všechny typy `Uj`, které nejsou identické s `U` jsou ze sady kandidátů odebrány.</span><span class="sxs-lookup"><span data-stu-id="6353a-132">We then examine each bound for `Xi` in turn: For each exact bound `U` of `Xi` all types `Uj` which are not identical to `U` are removed from the candidate set.</span></span> <span data-ttu-id="6353a-133">Pro každý dolní vázaný `U` `Xi` všechny typy `Uj` na *které není implicitní* převod z `U` ze sady kandidátů odebrán.</span><span class="sxs-lookup"><span data-stu-id="6353a-133">For each lower bound `U` of `Xi` all types `Uj` to which there is *not* an implicit conversion from `U` are removed from the candidate set.</span></span> <span data-ttu-id="6353a-134">Pro každý horní vázaný `U` `Xi` všechny typy `Uj`, ze *kterých není implicitní* převod na `U` ze sady kandidátů odebrán.</span><span class="sxs-lookup"><span data-stu-id="6353a-134">For each upper bound `U` of `Xi` all types `Uj` from which there is *not* an implicit conversion to `U` are removed from the candidate set.</span></span>
> *  <span data-ttu-id="6353a-135">Pokud je mezi zbývajícími typy kandidátů `Uj` jedinečný typ `V`, ze kterého existuje implicitní převod na všechny jiné typy kandidátů, pak ~~je`Xi` na `V`vyřešen.~~</span><span class="sxs-lookup"><span data-stu-id="6353a-135">If among the remaining candidate types `Uj` there is a unique type `V` from which there is an implicit conversion to all the other candidate types, then ~~`Xi` is fixed to `V`.~~</span></span>
>     -  <span data-ttu-id="6353a-136">**Je-li `V` typ hodnoty a pro `Xi`je *svázána hodnota null* , `Xi` je opravena `V?`**</span><span class="sxs-lookup"><span data-stu-id="6353a-136">**If `V` is a value type and there is a *null bound* for `Xi`, then `Xi` is fixed to `V?`**</span></span>
>     -  <span data-ttu-id="6353a-137">**Jinak je `Xi` pevně daná `V`**</span><span class="sxs-lookup"><span data-stu-id="6353a-137">**Otherwise   `Xi` is fixed to `V`**</span></span>
> *  <span data-ttu-id="6353a-138">V opačném případě se odvození typu nezdařilo.</span><span class="sxs-lookup"><span data-stu-id="6353a-138">Otherwise, type inference fails.</span></span>

## <a name="drawbacks"></a><span data-ttu-id="6353a-139">Nevýhody</span><span class="sxs-lookup"><span data-stu-id="6353a-139">Drawbacks</span></span>
[drawbacks]: #drawbacks

<span data-ttu-id="6353a-140">V tomto návrhu můžou být nějaké nekompatibility, které přináší.</span><span class="sxs-lookup"><span data-stu-id="6353a-140">There may be some incompatibilities introduced by this proposal.</span></span>

## <a name="alternatives"></a><span data-ttu-id="6353a-141">Alternativy</span><span class="sxs-lookup"><span data-stu-id="6353a-141">Alternatives</span></span>
[alternatives]: #alternatives

<span data-ttu-id="6353a-142">Žádné.</span><span class="sxs-lookup"><span data-stu-id="6353a-142">None.</span></span>

## <a name="unresolved-questions"></a><span data-ttu-id="6353a-143">Nevyřešené dotazy</span><span class="sxs-lookup"><span data-stu-id="6353a-143">Unresolved questions</span></span>
[unresolved]: #unresolved-questions

- <span data-ttu-id="6353a-144">[] Jaká je závažnost nekompatibility zavedená tímto návrhem, pokud existuje, a jak se dá považovat za moderovanou?</span><span class="sxs-lookup"><span data-stu-id="6353a-144">[ ] What is the severity of incompatibility introduced by this proposal, if any, and how can it be moderated?</span></span>

## <a name="design-meetings"></a><span data-ttu-id="6353a-145">Schůzky pro návrh</span><span class="sxs-lookup"><span data-stu-id="6353a-145">Design meetings</span></span>

<span data-ttu-id="6353a-146">Žádné.</span><span class="sxs-lookup"><span data-stu-id="6353a-146">None.</span></span>
