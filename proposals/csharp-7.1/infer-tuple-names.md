---
ms.openlocfilehash: 25e95b3ab8c384a7e66e59a7f9422cc9699074d7
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484718"
---
# <a name="infer-tuple-names-aka-tuple-projection-initializers"></a><span data-ttu-id="44b38-101">Odvodit názvy řazených kolekcí členů (neboli.</span><span class="sxs-lookup"><span data-stu-id="44b38-101">Infer tuple names (aka.</span></span> <span data-ttu-id="44b38-102">Inicializátory v řazené kolekci členů)</span><span class="sxs-lookup"><span data-stu-id="44b38-102">tuple projection initializers)</span></span>

## <a name="summary"></a><span data-ttu-id="44b38-103">Souhrn</span><span class="sxs-lookup"><span data-stu-id="44b38-103">Summary</span></span>
[summary]: #summary

<span data-ttu-id="44b38-104">V mnoha běžných případech tato funkce umožňuje vynechat názvy elementů řazené kolekce členů a místo toho je odvodit.</span><span class="sxs-lookup"><span data-stu-id="44b38-104">In a number of common cases, this feature allows the tuple element names to be omitted and instead be inferred.</span></span> <span data-ttu-id="44b38-105">Například namísto psaní `(f1: x.f1, f2: x?.f2)`se názvy elementů "F1" a "F2" dají odvodit z `(x.f1, x?.f2)`.</span><span class="sxs-lookup"><span data-stu-id="44b38-105">For instance, instead of typing `(f1: x.f1, f2: x?.f2)`, the element names "f1" and "f2" can be inferred from `(x.f1, x?.f2)`.</span></span>

<span data-ttu-id="44b38-106">To paralelně chování anonymních typů, které umožňují odvození názvů členů během vytváření.</span><span class="sxs-lookup"><span data-stu-id="44b38-106">This parallels the behavior of  anonymous types, which allow inferring member names during creation.</span></span> <span data-ttu-id="44b38-107">`new { x.f1, y?.f2 }` například deklaruje členy "F1" a "F2".</span><span class="sxs-lookup"><span data-stu-id="44b38-107">For instance, `new { x.f1, y?.f2 }` declares members "f1" and "f2".</span></span>

<span data-ttu-id="44b38-108">To je užitečné zejména při použití řazených kolekcí členů v LINQ:</span><span class="sxs-lookup"><span data-stu-id="44b38-108">This is particularly handy when using tuples in LINQ:</span></span>

```csharp
// "c" and "result" have element names "f1" and "f2"
var result = list.Select(c => (c.f1, c.f2)).Where(t => t.f2 == 1); 
```

## <a name="detailed-design"></a><span data-ttu-id="44b38-109">Podrobný návrh</span><span class="sxs-lookup"><span data-stu-id="44b38-109">Detailed design</span></span>
[design]: #detailed-design

<span data-ttu-id="44b38-110">Tato změna má dvě části:</span><span class="sxs-lookup"><span data-stu-id="44b38-110">There are two parts to the change:</span></span>

1.  <span data-ttu-id="44b38-111">Zkuste odvodit kandidáta na název každého prvku řazené kolekce členů, který nemá explicitní název:</span><span class="sxs-lookup"><span data-stu-id="44b38-111">Try to infer a candidate name for each tuple element which does not have an explicit name:</span></span>
    -   <span data-ttu-id="44b38-112">Použití stejných pravidel jako odvození názvu pro anonymní typy.</span><span class="sxs-lookup"><span data-stu-id="44b38-112">Using same rules as name inference for anonymous types.</span></span>
        - <span data-ttu-id="44b38-113">V C#nástroji to umožňuje tři případy: `y` (identifikátor), `x.y` (přístup pomocí jednoduchých členů) a `x?.y` (podmíněný přístup).</span><span class="sxs-lookup"><span data-stu-id="44b38-113">In C#, this allows three cases: `y` (identifier), `x.y` (simple member access) and `x?.y` (conditional access).</span></span>
        - <span data-ttu-id="44b38-114">V jazyce VB to umožňuje další případy, například `x.y()`.</span><span class="sxs-lookup"><span data-stu-id="44b38-114">In VB, this allows for additional cases, such as `x.y()`.</span></span>
    -   <span data-ttu-id="44b38-115">Odmítání vyhrazených názvů řazené kolekce členů C#(rozlišuje velká a malá písmena v jazyce VB), protože jsou buď zakázané, nebo už implicitní.</span><span class="sxs-lookup"><span data-stu-id="44b38-115">Rejecting reserved tuple names (case-sensitive in C#, case-insensitive in VB), as they are either forbidden or already implicit.</span></span> <span data-ttu-id="44b38-116">Například `ItemN`, `Rest`a `ToString`.</span><span class="sxs-lookup"><span data-stu-id="44b38-116">For instance, such as `ItemN`, `Rest`, and `ToString`.</span></span>
    -   <span data-ttu-id="44b38-117">Pokud jsou názvy kandidátů duplicitní (rozlišuje velká C#a malá písmena v VB) v rámci celé řazené kolekce členů, vynecháváme tyto kandidáty,</span><span class="sxs-lookup"><span data-stu-id="44b38-117">If any candidate names are duplicates (case-sensitive in C#, case-insensitive in VB) within the entire tuple, we drop those candidates,</span></span>
2.  <span data-ttu-id="44b38-118">Během převodů (které kontrolují a upozorňující na vyřazení názvů z literálů řazené kolekce členů) odvozené názvy nevytvořily žádná upozornění.</span><span class="sxs-lookup"><span data-stu-id="44b38-118">During conversions (which check and warn about dropping names from tuple literals), inferred names would not produce any warnings.</span></span> <span data-ttu-id="44b38-119">Tím předejdete přerušení stávajícího kódu řazené kolekce členů.</span><span class="sxs-lookup"><span data-stu-id="44b38-119">This avoids breaking existing tuple code.</span></span>

<span data-ttu-id="44b38-120">Všimněte si, že pravidlo pro zpracování duplicit je jiné než pro anonymní typy.</span><span class="sxs-lookup"><span data-stu-id="44b38-120">Note that the rule for handling duplicates is different than that for anonymous types.</span></span> <span data-ttu-id="44b38-121">`new { x.f1, x.f1 }` například vyvolá chybu, ale `(x.f1, x.f1)` by se pořád povolil (jenom bez názvů s odvozenými názvy).</span><span class="sxs-lookup"><span data-stu-id="44b38-121">For instance, `new { x.f1, x.f1 }` produces an error, but `(x.f1, x.f1)` would still be allowed (just without any inferred names).</span></span> <span data-ttu-id="44b38-122">Tím předejdete přerušení stávajícího kódu řazené kolekce členů.</span><span class="sxs-lookup"><span data-stu-id="44b38-122">This avoids breaking existing tuple code.</span></span>

<span data-ttu-id="44b38-123">Pro zajištění konzistence by se totéž mělo vztahovat na řazené kolekce členů vytvořené pomocí dekonstrukce C#– přiřazení (v):</span><span class="sxs-lookup"><span data-stu-id="44b38-123">For consistency, the same would apply to tuples produced by deconstruction-assignments (in C#):</span></span>

```csharp
// tuple has element names "f1" and "f2" 
var tuple = ((x.f1, x?.f2) = (1, 2));
```

<span data-ttu-id="44b38-124">Stejné by se také vztahují na řazené kolekce členů VB pomocí pravidel specifických pro jazyk VB pro odvození názvu z výrazů a porovnávání názvů bez rozlišení velkých a malých písmen.</span><span class="sxs-lookup"><span data-stu-id="44b38-124">The same would also apply to VB tuples, using the VB-specific rules for inferring name from expression and case-insensitive name comparisons.</span></span>

<span data-ttu-id="44b38-125">Pokud používáte kompilátor C# 7,1 (nebo novější) s jazykovou verzí "7,0", názvy prvků budou odvozeny (bez ohledu na to, která funkce není k dispozici), ale při pokusu o přístup k nim dojde k chybě použití webu.</span><span class="sxs-lookup"><span data-stu-id="44b38-125">When using the C# 7.1 compiler (or later) with language version "7.0", the element names will be inferred (despite the feature not being available), but there will be a use-site error for trying to access them.</span></span> <span data-ttu-id="44b38-126">Tím se omezí přidání nového kódu, který by později znamenal problém s kompatibilitou (popsaný níže).</span><span class="sxs-lookup"><span data-stu-id="44b38-126">This will limit additions of new code that would later face the compatibility issue (described below).</span></span>

## <a name="drawbacks"></a><span data-ttu-id="44b38-127">Nevýhody</span><span class="sxs-lookup"><span data-stu-id="44b38-127">Drawbacks</span></span>
[drawbacks]: #drawbacks

<span data-ttu-id="44b38-128">Hlavní nevýhodou je, že představuje přerušení kompatibility od C# 7,0:</span><span class="sxs-lookup"><span data-stu-id="44b38-128">The main drawback is that this introduces a compatibility break from C# 7.0:</span></span>

```csharp
Action y = () => M();
var t = (x: x, y);
t.y(); // this might have previously picked up an extension method called “y”, but would now call the lambda.
```

<span data-ttu-id="44b38-129">Rada kompatibility našla toto přerušení jako přijatelné, vzhledem k tomu, že je omezené a časový interval od odeslání řazené kolekce C# členů (v 7,0) je krátký.</span><span class="sxs-lookup"><span data-stu-id="44b38-129">The compatibility council found this break acceptable, given that it is limited and the time window since tuples shipped (in C# 7.0) is short.</span></span>

## <a name="references"></a><span data-ttu-id="44b38-130">Odkazy</span><span class="sxs-lookup"><span data-stu-id="44b38-130">References</span></span>
- [<span data-ttu-id="44b38-131">LDM od 4. dubna 2017</span><span class="sxs-lookup"><span data-stu-id="44b38-131">LDM April 4th 2017</span></span>](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-05.md#tuple-names)
- <span data-ttu-id="44b38-132">[Diskuze na GitHubu](https://github.com/dotnet/csharplang/issues/370) (děkujeme @alrz pro uvedení tohoto problému)</span><span class="sxs-lookup"><span data-stu-id="44b38-132">[Github discussion](https://github.com/dotnet/csharplang/issues/370) (thanks @alrz for bringing this issue up)</span></span>
- [<span data-ttu-id="44b38-133">Návrh řazených kolekcí členů</span><span class="sxs-lookup"><span data-stu-id="44b38-133">Tuples design</span></span>](https://github.com/dotnet/roslyn/blob/master/docs/features/tuples.md)
