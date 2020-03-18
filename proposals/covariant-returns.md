---
ms.openlocfilehash: 392d932459ff0a4cb0d6d32c1606f73f9b913c68
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484452"
---
# <a name="covariant-return-types"></a><span data-ttu-id="3132a-101">kovariantní návratové typy</span><span class="sxs-lookup"><span data-stu-id="3132a-101">covariant return types</span></span>

* <span data-ttu-id="3132a-102">Navrženo [x]</span><span class="sxs-lookup"><span data-stu-id="3132a-102">[x] Proposed</span></span>
* <span data-ttu-id="3132a-103">[] Prototyp: Nezahájeno</span><span class="sxs-lookup"><span data-stu-id="3132a-103">[ ] Prototype: Not Started</span></span>
* <span data-ttu-id="3132a-104">[] Implementace: Nezahájeno</span><span class="sxs-lookup"><span data-stu-id="3132a-104">[ ] Implementation: Not Started</span></span>
* <span data-ttu-id="3132a-105">[] Specifikace: Nezahájeno</span><span class="sxs-lookup"><span data-stu-id="3132a-105">[ ] Specification: Not Started</span></span>

## <a name="summary"></a><span data-ttu-id="3132a-106">Souhrn</span><span class="sxs-lookup"><span data-stu-id="3132a-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="3132a-107">Podporuje _kovariantní návratové typy_.</span><span class="sxs-lookup"><span data-stu-id="3132a-107">Support _covariant return types_.</span></span> <span data-ttu-id="3132a-108">Konkrétně umožněte přepsání metody, aby měl více odvozený odkazový typ než metoda, kterou Přepisuje.</span><span class="sxs-lookup"><span data-stu-id="3132a-108">Specifically, allow an overriding method to have a more derived reference type than the method it overrides.</span></span>

## <a name="motivation"></a><span data-ttu-id="3132a-109">Motivační</span><span class="sxs-lookup"><span data-stu-id="3132a-109">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="3132a-110">Je to společný vzor v kódu, který musí být vymysleli různými názvy metod, aby bylo možné obejít omezení jazyka, které přepisy musí vracet stejný typ jako Potlačená metoda.</span><span class="sxs-lookup"><span data-stu-id="3132a-110">It is a common pattern in code that different method names have to be invented to work around the language constraint that overrides must return the same type as the overridden method.</span></span> <span data-ttu-id="3132a-111">Viz níže pro příklad ze základu kódu Roslyn.</span><span class="sxs-lookup"><span data-stu-id="3132a-111">See below for an example from the Roslyn code base.</span></span>

## <a name="detailed-design"></a><span data-ttu-id="3132a-112">Podrobný návrh</span><span class="sxs-lookup"><span data-stu-id="3132a-112">Detailed design</span></span>
[design]: #detailed-design

<span data-ttu-id="3132a-113">Podporuje _kovariantní návratové typy_.</span><span class="sxs-lookup"><span data-stu-id="3132a-113">Support _covariant return types_.</span></span> <span data-ttu-id="3132a-114">Konkrétně umožněte přepsání metody, aby měl více odvozený odkazový typ než metoda, kterou Přepisuje.</span><span class="sxs-lookup"><span data-stu-id="3132a-114">Specifically, allow an overriding method to have a more derived reference type than the method it overrides.</span></span> <span data-ttu-id="3132a-115">To by mělo platit pro metody a vlastnosti a podporuje se v třídách a rozhraních.</span><span class="sxs-lookup"><span data-stu-id="3132a-115">This would apply to methods and properties, and be supported in classes and interfaces.</span></span>

<span data-ttu-id="3132a-116">To by bylo užitečné ve výrobním modelu.</span><span class="sxs-lookup"><span data-stu-id="3132a-116">This would be useful in the factory pattern.</span></span> <span data-ttu-id="3132a-117">Například v základu kódu Roslyn bychom měli</span><span class="sxs-lookup"><span data-stu-id="3132a-117">For example, in the Roslyn code base we would have</span></span>

``` cs
class Compilation ...
{
    virtual Compilation WithOptions(Options options)...
}
```

``` cs
class CSharpCompilation : Compilation
{
    override CSharpCompilation WithOptions(Options options)...
}
```

<span data-ttu-id="3132a-118">Implementace by mohla být pro kompilátor, aby vygeneroval přepsání metody jako "novou" metodu, která skrývá metodu základní třídy, spolu s _metodou mostu_ , která implementuje metodu základní třídy se voláním metody odvozené třídy.</span><span class="sxs-lookup"><span data-stu-id="3132a-118">The implementation of this would be for the compiler to emit the overriding method as a "new" virtual method that hides the base class method, along with a _bridge method_ that implements the base class method with a call to the derived class method.</span></span>

## <a name="drawbacks"></a><span data-ttu-id="3132a-119">Nevýhody</span><span class="sxs-lookup"><span data-stu-id="3132a-119">Drawbacks</span></span>
[drawbacks]: #drawbacks

- <span data-ttu-id="3132a-120">[] Každá změna jazyka musí platit pro sebe sama.</span><span class="sxs-lookup"><span data-stu-id="3132a-120">[ ] Every language change must pay for itself.</span></span>
- <span data-ttu-id="3132a-121">[] Měli byste zajistit, aby výkon byl přiměřený, i v případě hlubokých hierarchií dědičnosti.</span><span class="sxs-lookup"><span data-stu-id="3132a-121">[ ] We should ensure that the performance is reasonable, even in the case of deep inheritance hierarchies</span></span>
- <span data-ttu-id="3132a-122">[] Měli byste zajistit, aby artefakty strategie překladu neovlivnily sémantiku jazyka, ani když spotřebovávají nové IL ze starých kompilátorů.</span><span class="sxs-lookup"><span data-stu-id="3132a-122">[ ] We should ensure that artifacts of the translation strategy do not affect language semantics, even when consuming new IL from old compilers.</span></span>

## <a name="alternatives"></a><span data-ttu-id="3132a-123">Alternativy</span><span class="sxs-lookup"><span data-stu-id="3132a-123">Alternatives</span></span>
[alternatives]: #alternatives

<span data-ttu-id="3132a-124">Tato jazyková pravidla můžeme mírně zmírnit, aby bylo možné, ve zdroji,</span><span class="sxs-lookup"><span data-stu-id="3132a-124">We could relax the language rules slightly to allow, in source,</span></span>

```csharp
abstract class Cloneable
{
    public abstract Cloneable Clone();
}

class Digit : Cloneable
{
    public override Cloneable Clone()
    {
        return this.Clone();
    }

    public new Digit Clone() // Error: 'Digit' already defines a member called 'Clone' with the same parameter types
    {
        return this;
    }
}
```

## <a name="unresolved-questions"></a><span data-ttu-id="3132a-125">Nevyřešené dotazy</span><span class="sxs-lookup"><span data-stu-id="3132a-125">Unresolved questions</span></span>
[unresolved]: #unresolved-questions

- <span data-ttu-id="3132a-126">[] Jak budou rozhraní API, která byla zkompilována pro použití této funkce, fungovat ve starších verzích jazyka?</span><span class="sxs-lookup"><span data-stu-id="3132a-126">[ ] How will APIs that have been compiled to use this feature work in older versions of the language?</span></span>

## <a name="design-meetings"></a><span data-ttu-id="3132a-127">Schůzky pro návrh</span><span class="sxs-lookup"><span data-stu-id="3132a-127">Design meetings</span></span>

<span data-ttu-id="3132a-128">Žádná ještě není.</span><span class="sxs-lookup"><span data-stu-id="3132a-128">None yet.</span></span> <span data-ttu-id="3132a-129">Došlo k nějaké diskuzi na <https://github.com/dotnet/roslyn/issues/357>.</span><span class="sxs-lookup"><span data-stu-id="3132a-129">There has been some discussion at <https://github.com/dotnet/roslyn/issues/357>.</span></span>