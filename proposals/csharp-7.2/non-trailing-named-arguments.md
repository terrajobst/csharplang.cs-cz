---
ms.openlocfilehash: ac2b233eb703b5eea3bd2dfdbeeadd7494b0c695
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484669"
---
# <a name="non-trailing-named-arguments"></a><span data-ttu-id="d6cc8-101">Nekoncové pojmenované argumenty</span><span class="sxs-lookup"><span data-stu-id="d6cc8-101">Non-trailing named arguments</span></span>

## <a name="summary"></a><span data-ttu-id="d6cc8-102">Souhrn</span><span class="sxs-lookup"><span data-stu-id="d6cc8-102">Summary</span></span>
[summary]: #summary
<span data-ttu-id="d6cc8-103">Povolí použití pojmenovaných argumentů v nekoncové pozici, pokud se používají na jejich správné pozici.</span><span class="sxs-lookup"><span data-stu-id="d6cc8-103">Allow named arguments to be used in non-trailing position, as long as they are used in their correct position.</span></span> <span data-ttu-id="d6cc8-104">Například: `DoSomething(isEmployed:true, name, age);`.</span><span class="sxs-lookup"><span data-stu-id="d6cc8-104">For example: `DoSomething(isEmployed:true, name, age);`.</span></span>

## <a name="motivation"></a><span data-ttu-id="d6cc8-105">Motivační</span><span class="sxs-lookup"><span data-stu-id="d6cc8-105">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="d6cc8-106">Hlavní motivace je zabránit psaní redundantních informací.</span><span class="sxs-lookup"><span data-stu-id="d6cc8-106">The main motivation is to avoid typing redundant information.</span></span> <span data-ttu-id="d6cc8-107">Je běžné pojmenovat argument, který je literál (například `null`, `true`) pro účely objasnění kódu namísto předávání argumentů mimo pořadí.</span><span class="sxs-lookup"><span data-stu-id="d6cc8-107">It is common to name an argument that is a literal (such as `null`, `true`) for the purpose of clarifying the code, rather than of passing arguments out-of-order.</span></span>
<span data-ttu-id="d6cc8-108">To je momentálně zakázáno (`CS1738`), pokud nejsou všechny následující argumenty také pojmenovány.</span><span class="sxs-lookup"><span data-stu-id="d6cc8-108">That is currently disallowed (`CS1738`) unless all the following arguments are also named.</span></span>

```csharp
DoSomething(isEmployed:true, name, age); // currently disallowed, even though all arguments are in position
// CS1738 "Named argument specifications must appear after all fixed arguments have been specified"
```

<span data-ttu-id="d6cc8-109">Některé další příklady:</span><span class="sxs-lookup"><span data-stu-id="d6cc8-109">Some additional examples:</span></span>
```csharp
public void DoSomething(bool isEmployed, string personName, int personAge) { ... }

DoSomething(isEmployed:true, name, age); // currently CS1738, but would become legal
DoSomething(true, personName:name, age); // currently CS1738, but would become legal
DoSomething(name, isEmployed:true, age); // remains illegal
DoSomething(name, age, isEmployed:true); // remains illegal
DoSomething(true, personAge:age, personName:name); // already legal
```

<span data-ttu-id="d6cc8-110">To by také mohlo fungovat s parametry:</span><span class="sxs-lookup"><span data-stu-id="d6cc8-110">This would also work with params:</span></span>
```csharp
public class Task
{
    public static Task When(TaskStatus all, TaskStatus any, params Task[] tasks);
}
Task.When(all: TaskStatus.RanToCompletion, any: TaskStatus.Faulted, task1, task2)
```

## <a name="detailed-design"></a><span data-ttu-id="d6cc8-111">Podrobný návrh</span><span class="sxs-lookup"><span data-stu-id="d6cc8-111">Detailed design</span></span>
[design]: #detailed-design

<span data-ttu-id="d6cc8-112">V § 7.5.1 (seznamy argumentů) aktuálně uvedená specifikace uvádí:</span><span class="sxs-lookup"><span data-stu-id="d6cc8-112">In §7.5.1 (Argument lists), the spec currently says:</span></span>
> <span data-ttu-id="d6cc8-113">*Argument* s *argumentem-name* je označován jako __pojmenovaný argument__, zatímco *Argument* bez *argumentu-Name* je __poziční argument__.</span><span class="sxs-lookup"><span data-stu-id="d6cc8-113">An *argument* with an *argument-name* is referred to as a __named argument__, whereas an *argument* without an *argument-name* is a __positional argument__.</span></span> <span data-ttu-id="d6cc8-114">Je-li pozice argumentu uvedena po pojmenovaném argumentu v *seznamu argumentů*, jedná se o chybu.</span><span class="sxs-lookup"><span data-stu-id="d6cc8-114">It is an error for a positional argument to appear after a named argument in an *argument-list*.</span></span>

<span data-ttu-id="d6cc8-115">Účelem tohoto návrhu je odebrat tuto chybu a aktualizovat pravidla pro hledání odpovídajícího parametru argumentu (§ 7.5.1.1):</span><span class="sxs-lookup"><span data-stu-id="d6cc8-115">The proposal is to remove this error and update the rules for finding the corresponding parameter for an argument (§7.5.1.1):</span></span>

<span data-ttu-id="d6cc8-116">Argumenty v argumentu – seznam konstruktorů instancí, metod, indexerů a delegátů:</span><span class="sxs-lookup"><span data-stu-id="d6cc8-116">Arguments in the argument-list of instance constructors, methods, indexers and delegates:</span></span>
- <span data-ttu-id="d6cc8-117">[existující pravidla]</span><span class="sxs-lookup"><span data-stu-id="d6cc8-117">[existing rules]</span></span>
- <span data-ttu-id="d6cc8-118">Nepojmenovaný argument odpovídá žádnému parametru, je-li za názvem pojmenovaného argumentu nebo pojmenovaným param.</span><span class="sxs-lookup"><span data-stu-id="d6cc8-118">An unnamed argument corresponds to no parameter when it is after an out-of-position named argument or a named params argument.</span></span>

<span data-ttu-id="d6cc8-119">Konkrétně to brání vyvolání `void M(bool a = true, bool b = true, bool c = true, );` s `M(c: false, valueB);`.</span><span class="sxs-lookup"><span data-stu-id="d6cc8-119">In particular, this prevents invoking `void M(bool a = true, bool b = true, bool c = true, );` with `M(c: false, valueB);`.</span></span> <span data-ttu-id="d6cc8-120">První argument se používá mimo umístění (argument se používá jako první pozice, ale parametr s názvem "c" je na třetí pozici), takže by měly být pojmenovány následující argumenty.</span><span class="sxs-lookup"><span data-stu-id="d6cc8-120">The first argument is used out-of-position (the argument is used in first position, but the parameter named "c" is in third position), so the following arguments should be named.</span></span>

<span data-ttu-id="d6cc8-121">Jinými slovy, nekoncové pojmenované argumenty jsou povoleny pouze v případě, že název a pozice mají za následek vyhledání stejného odpovídajícího parametru.</span><span class="sxs-lookup"><span data-stu-id="d6cc8-121">In other words, non-trailing named arguments are only allowed when the name and the position result in finding the same corresponding parameter.</span></span>

## <a name="drawbacks"></a><span data-ttu-id="d6cc8-122">Nevýhody</span><span class="sxs-lookup"><span data-stu-id="d6cc8-122">Drawbacks</span></span>
[drawbacks]: #drawbacks

<span data-ttu-id="d6cc8-123">Tento návrh exacerbates existující odlišností s pojmenovanými argumenty v řešení přetížení.</span><span class="sxs-lookup"><span data-stu-id="d6cc8-123">This proposal exacerbates existing subtleties with named arguments in overload resolution.</span></span> <span data-ttu-id="d6cc8-124">Příklad:</span><span class="sxs-lookup"><span data-stu-id="d6cc8-124">For instance:</span></span>

```csharp
void M(int x, int y) { }
void M<T>(T y, int x) { }

void M2()
{
    M(3, 4);
    M(y: 3, x: 4); // Invokes M(int, int)
    M(y: 3, 4); // Invokes M<T>(T, int)
}
```

<span data-ttu-id="d6cc8-125">Tuto situaci můžete získat ještě dnes zahozením parametrů:</span><span class="sxs-lookup"><span data-stu-id="d6cc8-125">You could get this situation today by swapping the parameters:</span></span>

```csharp
void M(int y, int x) { }
void M<T>(int x, T y) { }

void M2()
{
    M(3, 4);
    M(x: 3, y: 4); // Invokes M(int, int)
    M(3, y: 4); // Invokes M<T>(int, T)
}
```

<span data-ttu-id="d6cc8-126">Podobně, pokud máte dvě metody `void M(int a, int b)` a `void M(int x, string y)`, bude chybné vyvolání `M(x: 1, 2)` vyvolat diagnostiku na základě druhého přetížení ("nelze převést z ' int ' na ' String ').</span><span class="sxs-lookup"><span data-stu-id="d6cc8-126">Similarly, if you have two methods `void M(int a, int b)` and `void M(int x, string y)`, the mistaken invocation `M(x: 1, 2)` will produce a diagnostic based on the second overload ("cannot convert from 'int' to 'string'").</span></span> <span data-ttu-id="d6cc8-127">Tento problém již existuje, pokud je pojmenovaný argument použit na koncové pozici.</span><span class="sxs-lookup"><span data-stu-id="d6cc8-127">This problem already exists when the named argument is used in a trailing position.</span></span>

## <a name="alternatives"></a><span data-ttu-id="d6cc8-128">Alternativy</span><span class="sxs-lookup"><span data-stu-id="d6cc8-128">Alternatives</span></span>
[alternatives]: #alternatives

<span data-ttu-id="d6cc8-129">Je potřeba vzít v úvahu několik alternativ:</span><span class="sxs-lookup"><span data-stu-id="d6cc8-129">There are a couple of alternatives to consider:</span></span>

- <span data-ttu-id="d6cc8-130">Stav quo</span><span class="sxs-lookup"><span data-stu-id="d6cc8-130">The status quo</span></span>
- <span data-ttu-id="d6cc8-131">Poskytnutí pomoci IDE pro vyplňování všech názvů koncových argumentů při psaní konkrétního názvu uprostřed.</span><span class="sxs-lookup"><span data-stu-id="d6cc8-131">Providing IDE assistance to fill-in all the names of trailing arguments when you type specific a name in the middle.</span></span>

<span data-ttu-id="d6cc8-132">Obě z nich jsou z větší podrobností, protože zavádějí více pojmenovaných argumentů i v případě, že pouze potřebujete pouze jeden název literálu na začátku seznamu argumentů.</span><span class="sxs-lookup"><span data-stu-id="d6cc8-132">Both of those suffer from more verbosity, as they introduce multiple named arguments even if you just need one name of a literal at the beginning of the argument list.</span></span>

## <a name="unresolved-questions"></a><span data-ttu-id="d6cc8-133">Nevyřešené dotazy</span><span class="sxs-lookup"><span data-stu-id="d6cc8-133">Unresolved questions</span></span>
[unresolved]: #unresolved-questions

## <a name="design-meetings"></a><span data-ttu-id="d6cc8-134">Schůzky pro návrh</span><span class="sxs-lookup"><span data-stu-id="d6cc8-134">Design meetings</span></span>
[ldm]: #ldm
<span data-ttu-id="d6cc8-135">Tato funkce byla stručně popsána v nástroji LDM v případě 16. května 2017 s schválením v principu (pro přechod na návrh/prototyp).</span><span class="sxs-lookup"><span data-stu-id="d6cc8-135">The feature was briefly discussed in LDM on May 16th 2017, with approval in principle (ok to move to proposal/prototype).</span></span> <span data-ttu-id="d6cc8-136">Byla také stručně popsána v červnu 28 2017.</span><span class="sxs-lookup"><span data-stu-id="d6cc8-136">It was also briefly discussed on June 28th 2017.</span></span>

<span data-ttu-id="d6cc8-137">Vztahuje se na úvodní diskuzi https://github.com/dotnet/csharplang/issues/518 vztah k problému s championed https://github.com/dotnet/csharplang/issues/570</span><span class="sxs-lookup"><span data-stu-id="d6cc8-137">Relates to initial discussion https://github.com/dotnet/csharplang/issues/518 Relates to championed issue https://github.com/dotnet/csharplang/issues/570</span></span>
