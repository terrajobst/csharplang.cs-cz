---
ms.openlocfilehash: 54ae4ffabde6dca49b7e6bfb626d65837eabc8f5
ms.sourcegitcommit: 1e1c7c72b156e2fbc54d6d6ac8d21bca9934d8d2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/26/2020
ms.locfileid: "80281941"
---
# <a name="simple-programs"></a><span data-ttu-id="4aea7-101">Jednoduché programy</span><span class="sxs-lookup"><span data-stu-id="4aea7-101">Simple programs</span></span>

* <span data-ttu-id="4aea7-102">Navrženo [x]</span><span class="sxs-lookup"><span data-stu-id="4aea7-102">[x] Proposed</span></span>
* <span data-ttu-id="4aea7-103">[x] prototyp: spuštěno</span><span class="sxs-lookup"><span data-stu-id="4aea7-103">[x] Prototype: Started</span></span>
* <span data-ttu-id="4aea7-104">[] Implementace: Nezahájeno</span><span class="sxs-lookup"><span data-stu-id="4aea7-104">[ ] Implementation: Not Started</span></span>
* <span data-ttu-id="4aea7-105">[] Specifikace: Nezahájeno</span><span class="sxs-lookup"><span data-stu-id="4aea7-105">[ ] Specification: Not Started</span></span>

## <a name="summary"></a><span data-ttu-id="4aea7-106">Souhrn</span><span class="sxs-lookup"><span data-stu-id="4aea7-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="4aea7-107">Povolí sekvenci *příkazů* , které se mají provést přímo před *namespace_member_declaration*s *compilation_unit* (tj. zdrojový soubor).</span><span class="sxs-lookup"><span data-stu-id="4aea7-107">Allow a sequence of *statements* to occur right before the *namespace_member_declaration*s of a *compilation_unit* (i.e. source file).</span></span>

<span data-ttu-id="4aea7-108">Sémantika znamená, že pokud je taková sekvence *příkazů* přítomna, bude vyvolána následující deklarace typu (modulo) skutečný název typu a název metody.</span><span class="sxs-lookup"><span data-stu-id="4aea7-108">The semantics are that if such a sequence of *statements* is present, the following type declaration, modulo the actual type name and the method name, would be emitted:</span></span>

``` c#
static class Program
{
    static async Task Main()
    {
        // statements
    }
}
```

<span data-ttu-id="4aea7-109">Viz také https://github.com/dotnet/csharplang/issues/3117.</span><span class="sxs-lookup"><span data-stu-id="4aea7-109">See also https://github.com/dotnet/csharplang/issues/3117.</span></span>

## <a name="motivation"></a><span data-ttu-id="4aea7-110">Motivační</span><span class="sxs-lookup"><span data-stu-id="4aea7-110">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="4aea7-111">K dispozici je určité množství často používaného textu, a to i v případě, že je potřeba explicitní `Main` metoda.</span><span class="sxs-lookup"><span data-stu-id="4aea7-111">There's a certain amount of boilerplate surrounding even the simplest of programs, because of the need for an explicit `Main` method.</span></span> <span data-ttu-id="4aea7-112">Zdá se, že se dostanete na základě jazyka učení a srozumitelnosti programu.</span><span class="sxs-lookup"><span data-stu-id="4aea7-112">This seems to get in the way of language learning and program clarity.</span></span> <span data-ttu-id="4aea7-113">Hlavním cílem této funkce je tedy dovolit programům bez C# zbytečného vynechání kolem nich, a to v případě učících a srozumitelnosti kódu.</span><span class="sxs-lookup"><span data-stu-id="4aea7-113">The primary goal of the feature therefore is to allow C# programs without unnecessary boilerplate around them, for the sake of learners and the clarity of code.</span></span>

## <a name="detailed-design"></a><span data-ttu-id="4aea7-114">Podrobný návrh</span><span class="sxs-lookup"><span data-stu-id="4aea7-114">Detailed design</span></span>
[design]: #detailed-design

### <a name="syntax"></a><span data-ttu-id="4aea7-115">Syntaxe</span><span class="sxs-lookup"><span data-stu-id="4aea7-115">Syntax</span></span>

<span data-ttu-id="4aea7-116">Jedinou další syntaxí je povolení sekvence *příkazů*v kompilační jednotce těsně před *namespace_member_declaration*s:</span><span class="sxs-lookup"><span data-stu-id="4aea7-116">The only additional syntax is allowing a sequence of *statement*s in a compilation unit, just before the *namespace_member_declaration*s:</span></span>

``` antlr
compilation_unit
    : extern_alias_directive* using_directive* global_attributes? statement* namespace_member_declaration*
    ;
```

<span data-ttu-id="4aea7-117">Pouze jeden *compilation_unit* může mít příkaz s *příkazem*.</span><span class="sxs-lookup"><span data-stu-id="4aea7-117">Only one *compilation_unit* is allowed to have *statement*s.</span></span> 

<span data-ttu-id="4aea7-118">Příklad:</span><span class="sxs-lookup"><span data-stu-id="4aea7-118">Example:</span></span>

``` c#
if (System.Environment.CommandLine.Length == 0
    || !int.TryParse(System.Environment.CommandLine, out int n)
    || n < 0) return;
Console.WriteLine(Fib(n).curr);

(int curr, int prev) Fib(int i)
{
    if (i == 0) return (1, 0);
    var (curr, prev) = Fib(i - 1);
    return (curr + prev, curr);
}
```

### <a name="semantics"></a><span data-ttu-id="4aea7-119">Sémantiku</span><span class="sxs-lookup"><span data-stu-id="4aea7-119">Semantics</span></span>

<span data-ttu-id="4aea7-120">Pokud jsou jakékoli příkazy nejvyšší úrovně přítomny v jakékoli kompilační jednotce programu, je význam, jako kdyby byly kombinovány v těle bloku `Main` metody `Program` třídy v globálním oboru názvů následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="4aea7-120">If any top-level statements are present in any compilation unit of the program, the meaning is as if they were combined in the block body of a `Main` method of a `Program` class in the global namespace, as follows:</span></span>

``` c#
static class Program
{
    static async Task Main()
    {
        // statements
    }
}
```

<span data-ttu-id="4aea7-121">Všimněte si, že názvy "program" a "Main" jsou používány pouze pro ilustraci, skutečné názvy používané kompilátorem jsou závislé na implementaci a ani typ ani na metodu nelze odkazovat pomocí názvu ze zdrojového kódu.</span><span class="sxs-lookup"><span data-stu-id="4aea7-121">Note that the names "Program" and "Main" are used only for illustrations purposes, actual names used by compiler are implementation dependent and neither the type, nor the method can be referenced by name from source code.</span></span>

<span data-ttu-id="4aea7-122">Metoda je určena jako vstupní bod programu.</span><span class="sxs-lookup"><span data-stu-id="4aea7-122">The method is designated as the entry point of the program.</span></span> <span data-ttu-id="4aea7-123">Explicitně deklarované metody, které by mohly být považovány za kandidáty vstupního bodu, se ignorují.</span><span class="sxs-lookup"><span data-stu-id="4aea7-123">Explicitly declared methods that by convention could be considered as an entry point candidates are ignored.</span></span> <span data-ttu-id="4aea7-124">V případě, že k tomu dojde, je hlášeno upozornění.</span><span class="sxs-lookup"><span data-stu-id="4aea7-124">A warning is reported when that happens.</span></span> <span data-ttu-id="4aea7-125">Pokud existují příkazy na nejvyšší úrovni, při určení `-main:<type>` přepínač kompilátoru se jedná o chybu.</span><span class="sxs-lookup"><span data-stu-id="4aea7-125">It is an error to specify `-main:<type>` compiler switch when there are top-level statements.</span></span>

<span data-ttu-id="4aea7-126">Asynchronní operace jsou povoleny v příkazech nejvyšší úrovně do rozsahu, v němž jsou povoleny v příkazech v rámci běžné metody asynchronního vstupního bodu.</span><span class="sxs-lookup"><span data-stu-id="4aea7-126">Async operations are allowed in top-level statements to the degree they are allowed in statements within a regular async entry point method.</span></span> <span data-ttu-id="4aea7-127">Nejsou však požadovány, pokud `await` výrazy a jiné asynchronní operace vynechány, není vytvořeno žádné upozornění.</span><span class="sxs-lookup"><span data-stu-id="4aea7-127">However, they are not required, if `await` expressions and other async operations are omitted, no warning is produced.</span></span> <span data-ttu-id="4aea7-128">Místo toho je podpis generované metody vstupního bodu ekvivalentní</span><span class="sxs-lookup"><span data-stu-id="4aea7-128">Instead the signature of the generated entry point method is equivalent to</span></span> 
``` c#
    static void Main()
```

<span data-ttu-id="4aea7-129">Výše uvedený příklad by vrátil následující `$Main` deklaraci metody:</span><span class="sxs-lookup"><span data-stu-id="4aea7-129">The example above would yield the following `$Main` method declaration:</span></span>

``` c#
static class $Program
{
    static void $Main()
    {
        if (System.Environment.CommandLine.Length == 0
            || !int.TryParse(System.Environment.CommandLine, out int n)
            || n < 0) return;
        Console.WriteLine(Fib(n).curr);
        
        (int curr, int prev) Fib(int i)
        {
            if (i == 0) return (1, 0);
            var (curr, prev) = Fib(i - 1);
            return (curr + prev, curr);
        }
    }
}
```

<span data-ttu-id="4aea7-130">Ve stejnou dobu jako příklad:</span><span class="sxs-lookup"><span data-stu-id="4aea7-130">At the same time an example like this:</span></span>
``` c#
await System.Threading.Tasks.Task.Delay(1000);
System.Console.WriteLine("Hi!");
```

<span data-ttu-id="4aea7-131">by to vedlo k těmto akcím:</span><span class="sxs-lookup"><span data-stu-id="4aea7-131">would  yield:</span></span>
``` c#
static class $Program
{
    static async Task $Main()
    {
        await System.Threading.Tasks.Task.Delay(1000);
        System.Console.WriteLine("Hi!");
    }
}
```

### <a name="scope-of-top-level-local-variables-and-local-functions"></a><span data-ttu-id="4aea7-132">Rozsah místních proměnných nejvyšší úrovně a místních funkcí</span><span class="sxs-lookup"><span data-stu-id="4aea7-132">Scope of top-level local variables and local functions</span></span>

<span data-ttu-id="4aea7-133">I když jsou lokální proměnné a funkce nejvyšší úrovně "zabalené" do generované metody vstupního bodu, měly by být stále v rozsahu v rámci programu v každé kompilační jednotce.</span><span class="sxs-lookup"><span data-stu-id="4aea7-133">Even though top-level local variables and functions are "wrapped" into the generated entry point method, they should still be in scope throughout the program in every compilation unit.</span></span>
<span data-ttu-id="4aea7-134">Pro účely vyhodnocení jednoduchého názvu po dosažení globálního oboru názvů:</span><span class="sxs-lookup"><span data-stu-id="4aea7-134">For the purpose of simple-name evaluation, once the global namespace is reached:</span></span>
- <span data-ttu-id="4aea7-135">Nejprve se provede pokus o vyhodnocení názvu v rámci generované metody vstupního bodu a jenom v případě, že se tento pokus nezdaří.</span><span class="sxs-lookup"><span data-stu-id="4aea7-135">First, an attempt is made to evaluate the name within the generated entry point method and only if this attempt fails</span></span> 
- <span data-ttu-id="4aea7-136">Vyhodnocování "regular" v rámci deklarace globálního oboru názvů je provedeno.</span><span class="sxs-lookup"><span data-stu-id="4aea7-136">The "regular" evaluation within the global namespace declaration is performed.</span></span> 

<span data-ttu-id="4aea7-137">To může vést k vytvoření stínové kopie oborů názvů a typů deklarovaných v globálním oboru názvů a také k vytváření stínů importovaných názvů.</span><span class="sxs-lookup"><span data-stu-id="4aea7-137">This could lead to name shadowing of namespaces and types declared within the global namespace as well as to shadowing of imported names.</span></span>

<span data-ttu-id="4aea7-138">Pokud se k vyhodnocení jednoduchého názvu dochází mimo příkazy nejvyšší úrovně a vyhodnocení má za následek místní proměnnou nebo funkci nejvyšší úrovně, která by měla vést k chybě.</span><span class="sxs-lookup"><span data-stu-id="4aea7-138">If the simple name evaluation occurs outside of the top-level statements and the evaluation yields a top-level local variable or function, that should lead to an error.</span></span>

<span data-ttu-id="4aea7-139">Tímto způsobem chráníme naši budoucí schopnost lépe řešit "funkce nejvyšší úrovně" (scénář 2 v https://github.com/dotnet/csharplang/issues/3117)a je možné poskytnout užitečnou diagnostiku uživatelům, kteří se omylem domnívají, že jsou podporované.</span><span class="sxs-lookup"><span data-stu-id="4aea7-139">In this way we protect our future ability to better address "Top-level functions" (scenario 2 in https://github.com/dotnet/csharplang/issues/3117), and are able to give useful diagnostics to users who mistakenly believe them to be supported.</span></span>

