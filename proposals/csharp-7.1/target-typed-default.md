---
ms.openlocfilehash: 1852356b830e29c3537a4de559fef32fd2c2f8b6
ms.sourcegitcommit: f7952cdddf85316a4beec493a0ecc14fcb3241c8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/30/2019
ms.locfileid: "79484984"
---
# <a name="target-typed-default-literal"></a><span data-ttu-id="ea394-101">Literál "default" cílového typu</span><span class="sxs-lookup"><span data-stu-id="ea394-101">Target-typed "default" literal</span></span>

* <span data-ttu-id="ea394-102">Navrženo [x]</span><span class="sxs-lookup"><span data-stu-id="ea394-102">[x] Proposed</span></span>
* <span data-ttu-id="ea394-103">[x] prototyp</span><span class="sxs-lookup"><span data-stu-id="ea394-103">[x] Prototype</span></span>
* <span data-ttu-id="ea394-104">[x] implementace</span><span class="sxs-lookup"><span data-stu-id="ea394-104">[x] Implementation</span></span>
* <span data-ttu-id="ea394-105">[] Specifikace</span><span class="sxs-lookup"><span data-stu-id="ea394-105">[ ] Specification</span></span>

## <a name="summary"></a><span data-ttu-id="ea394-106">Souhrn</span><span class="sxs-lookup"><span data-stu-id="ea394-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="ea394-107">`default` funkce Target-Type je kratší varianta operátoru `default(T)`, která umožňuje vynechat typ.</span><span class="sxs-lookup"><span data-stu-id="ea394-107">The target-typed `default` feature is a shorter form variation of the `default(T)` operator, which allows the type to be omitted.</span></span> <span data-ttu-id="ea394-108">Jeho typ je odvozený podle cíle a zadáním.</span><span class="sxs-lookup"><span data-stu-id="ea394-108">Its type is inferred by target-typing instead.</span></span> <span data-ttu-id="ea394-109">Kromě toho se chová jako `default(T)`.</span><span class="sxs-lookup"><span data-stu-id="ea394-109">Aside from that, it behaves like `default(T)`.</span></span>

## <a name="motivation"></a><span data-ttu-id="ea394-110">Motivační</span><span class="sxs-lookup"><span data-stu-id="ea394-110">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="ea394-111">Hlavní motivace je zabránit psaní redundantních informací.</span><span class="sxs-lookup"><span data-stu-id="ea394-111">The main motivation is to avoid typing redundant information.</span></span>

<span data-ttu-id="ea394-112">Například při vyvolání `void Method(ImmutableArray<SomeType> array)`*výchozí* literál umožňuje `M(default)` místo `M(default(ImmutableArray<SomeType>))`.</span><span class="sxs-lookup"><span data-stu-id="ea394-112">For instance, when invoking `void Method(ImmutableArray<SomeType> array)`, the *default* literal allows `M(default)` in place of `M(default(ImmutableArray<SomeType>))`.</span></span>

<span data-ttu-id="ea394-113">To platí pro několik scénářů, například:</span><span class="sxs-lookup"><span data-stu-id="ea394-113">This is applicable in a number of scenarios, such as:</span></span>

- <span data-ttu-id="ea394-114">deklarování místních hodnot (`ImmutableArray<SomeType> x = default;`)</span><span class="sxs-lookup"><span data-stu-id="ea394-114">declaring locals (`ImmutableArray<SomeType> x = default;`)</span></span>
- <span data-ttu-id="ea394-115">Ternární operace (`var x = flag ? default : ImmutableArray<SomeType>.Empty;`)</span><span class="sxs-lookup"><span data-stu-id="ea394-115">ternary operations (`var x = flag ? default : ImmutableArray<SomeType>.Empty;`)</span></span>
- <span data-ttu-id="ea394-116">návrat v metodách a výrazech lambda (`return default;`)</span><span class="sxs-lookup"><span data-stu-id="ea394-116">returning in methods and lambdas (`return default;`)</span></span>
- <span data-ttu-id="ea394-117">deklarování výchozích hodnot pro volitelné parametry (`void Method(ImmutableArray<SomeType> arrayOpt = default)`)</span><span class="sxs-lookup"><span data-stu-id="ea394-117">declaring default values for optional parameters (`void Method(ImmutableArray<SomeType> arrayOpt = default)`)</span></span>
- <span data-ttu-id="ea394-118">zahrnutí výchozích hodnot do výrazů vytváření pole (`var x = new[] { default, ImmutableArray.Create(y) };`)</span><span class="sxs-lookup"><span data-stu-id="ea394-118">including default values in array creation expressions (`var x = new[] { default, ImmutableArray.Create(y) };`)</span></span>


## <a name="detailed-design"></a><span data-ttu-id="ea394-119">Podrobný návrh</span><span class="sxs-lookup"><span data-stu-id="ea394-119">Detailed design</span></span>
[design]: #detailed-design

<span data-ttu-id="ea394-120">Je představen nový výraz, což je *výchozí* literál.</span><span class="sxs-lookup"><span data-stu-id="ea394-120">A new expression is introduced, the *default* literal.</span></span> <span data-ttu-id="ea394-121">Výraz s touto klasifikací lze implicitně převést na libovolný typ pomocí *výchozího literálového převodu*.</span><span class="sxs-lookup"><span data-stu-id="ea394-121">An expression with this classification can be implicitly converted to any type, by a *default literal conversion*.</span></span> 

<span data-ttu-id="ea394-122">Odvození typu pro *výchozí* literál funguje stejně jako u literálu s *hodnotou null* , s výjimkou, že je povolen libovolný typ (nikoli pouze odkazové typy).</span><span class="sxs-lookup"><span data-stu-id="ea394-122">The inference of the type for the *default* literal works the same as that for the *null* literal, except that any type is allowed (not just reference types).</span></span>

<span data-ttu-id="ea394-123">Tento převod vytvoří výchozí hodnotu odvozeného typu.</span><span class="sxs-lookup"><span data-stu-id="ea394-123">This conversion produces the default value of the inferred type.</span></span>

<span data-ttu-id="ea394-124">*Výchozí* literál může mít konstantní hodnotu v závislosti na odvozeném typu.</span><span class="sxs-lookup"><span data-stu-id="ea394-124">The *default* literal may have a constant value, depending on the inferred type.</span></span> <span data-ttu-id="ea394-125">Takže `const int x = default;` je právní, ale `const int? y = default;` ne.</span><span class="sxs-lookup"><span data-stu-id="ea394-125">So `const int x = default;` is legal, but `const int? y = default;` is not.</span></span>

<span data-ttu-id="ea394-126">*Výchozí* literál může být operand operátorů rovnosti, pokud má druhý operand typ.</span><span class="sxs-lookup"><span data-stu-id="ea394-126">The *default* literal can be the operand of equality operators, as long as the other operand has a type.</span></span> <span data-ttu-id="ea394-127">Takže `default == x` a `x == default` jsou platné výrazy, ale `default == default` je neplatné.</span><span class="sxs-lookup"><span data-stu-id="ea394-127">So `default == x` and `x == default` are valid expressions, but `default == default` is illegal.</span></span>

## <a name="drawbacks"></a><span data-ttu-id="ea394-128">Nevýhody</span><span class="sxs-lookup"><span data-stu-id="ea394-128">Drawbacks</span></span>
[drawbacks]: #drawbacks

<span data-ttu-id="ea394-129">Menší nevýhoda je, že *výchozí* literál lze použít místo literálu s *hodnotou null* ve většině kontextů.</span><span class="sxs-lookup"><span data-stu-id="ea394-129">A minor drawback is that *default* literal can be used in place of *null* literal in most contexts.</span></span> <span data-ttu-id="ea394-130">Dvě výjimky jsou `throw null;` a `null == null`, které jsou povoleny pro literál s *hodnotou null* , ale nikoli *výchozí* literál.</span><span class="sxs-lookup"><span data-stu-id="ea394-130">Two of the exceptions are `throw null;` and `null == null`, which are allowed for the *null* literal, but not the *default* literal.</span></span>

## <a name="alternatives"></a><span data-ttu-id="ea394-131">Alternativy</span><span class="sxs-lookup"><span data-stu-id="ea394-131">Alternatives</span></span>
[alternatives]: #alternatives

<span data-ttu-id="ea394-132">Je potřeba vzít v úvahu několik alternativ:</span><span class="sxs-lookup"><span data-stu-id="ea394-132">There are a couple of alternatives to consider:</span></span>

- <span data-ttu-id="ea394-133">Stav quo: funkce není odůvodněná na vlastní hodnoty a vývojáři nadále používají výchozí operátor s explicitním typem.</span><span class="sxs-lookup"><span data-stu-id="ea394-133">The status quo:  The feature is not justified on its own merits and developers continue to use the default operator with an explicit type.</span></span>
- <span data-ttu-id="ea394-134">Rozšíření literálu s hodnotou null: Jedná se o přístup VB s `Nothing`.</span><span class="sxs-lookup"><span data-stu-id="ea394-134">Extending the null literal: This is the VB approach with `Nothing`.</span></span> <span data-ttu-id="ea394-135">Můžeme dovolit `int x = null;`.</span><span class="sxs-lookup"><span data-stu-id="ea394-135">We could allow `int x = null;`.</span></span>

## <a name="unresolved-questions"></a><span data-ttu-id="ea394-136">Nevyřešené dotazy</span><span class="sxs-lookup"><span data-stu-id="ea394-136">Unresolved questions</span></span>
[unresolved]: #unresolved-questions

- <span data-ttu-id="ea394-137">[x] má být jako Operand operátoru *is* nebo *as* povoleno *výchozí nastavení* ?</span><span class="sxs-lookup"><span data-stu-id="ea394-137">[x] Should *default* be allowed as the operand of the *is* or *as* operators?</span></span> <span data-ttu-id="ea394-138">Odpověď: zakázat `default is T`, povolit `x is default`, povolit `default as RefType` (s upozorněním Always-null)</span><span class="sxs-lookup"><span data-stu-id="ea394-138">Answer:  disallow `default is T`, allow `x is default`, allow `default as RefType` (with always-null warning)</span></span>

## <a name="design-meetings"></a><span data-ttu-id="ea394-139">Schůzky pro návrh</span><span class="sxs-lookup"><span data-stu-id="ea394-139">Design meetings</span></span>

- [<span data-ttu-id="ea394-140">LDM 3/7/2017</span><span class="sxs-lookup"><span data-stu-id="ea394-140">LDM 3/7/2017</span></span>](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-03-07.md)
- [<span data-ttu-id="ea394-141">LDM 3/28/2017</span><span class="sxs-lookup"><span data-stu-id="ea394-141">LDM 3/28/2017</span></span>](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-03-28.md)
- [<span data-ttu-id="ea394-142">LDM 5/31/2017</span><span class="sxs-lookup"><span data-stu-id="ea394-142">LDM 5/31/2017</span></span>](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-05-31.md#default-in-operators)
