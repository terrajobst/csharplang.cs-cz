---
ms.openlocfilehash: b9697fc1d772ba59ed3b1de339a5a3d4eb24b1bd
ms.sourcegitcommit: 36b028f4d6e88bd7d4a843c6d384d1b63cc73334
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/19/2020
ms.locfileid: "79485222"
---
# <a name="simple-programs"></a><span data-ttu-id="bf71d-101">Jednoduché programy</span><span class="sxs-lookup"><span data-stu-id="bf71d-101">Simple programs</span></span>

* <span data-ttu-id="bf71d-102">Navrženo [x]</span><span class="sxs-lookup"><span data-stu-id="bf71d-102">[x] Proposed</span></span>
* <span data-ttu-id="bf71d-103">[x] prototyp: spuštěno</span><span class="sxs-lookup"><span data-stu-id="bf71d-103">[x] Prototype: Started</span></span>
* <span data-ttu-id="bf71d-104">[] Implementace: Nezahájeno</span><span class="sxs-lookup"><span data-stu-id="bf71d-104">[ ] Implementation: Not Started</span></span>
* <span data-ttu-id="bf71d-105">[] Specifikace: Nezahájeno</span><span class="sxs-lookup"><span data-stu-id="bf71d-105">[ ] Specification: Not Started</span></span>

## <a name="summary"></a><span data-ttu-id="bf71d-106">Souhrn</span><span class="sxs-lookup"><span data-stu-id="bf71d-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="bf71d-107">Povolí sekvenci *příkazů* , které se mají provést přímo před *namespace_member_declaration*s *compilation_unit* (tj. zdrojový soubor).</span><span class="sxs-lookup"><span data-stu-id="bf71d-107">Allow a sequence of *statements* to occur right before the *namespace_member_declaration*s of a *compilation_unit* (i.e. source file).</span></span>

<span data-ttu-id="bf71d-108">Sémantika znamená, že pokud je taková sekvence *příkazů* přítomna, bude vyvolána následující deklarace typu (modulo) skutečný název typu a název metody.</span><span class="sxs-lookup"><span data-stu-id="bf71d-108">The semantics are that if such a sequence of *statements* is present, the following type declaration, modulo the actual type name and the method name, would be emitted:</span></span>

``` c#
static class Program
{
    static async Task Main()
    {
        // statements
    }
}
```

<span data-ttu-id="bf71d-109">Viz také https://github.com/dotnet/csharplang/issues/3117.</span><span class="sxs-lookup"><span data-stu-id="bf71d-109">See also https://github.com/dotnet/csharplang/issues/3117.</span></span>

## <a name="motivation"></a><span data-ttu-id="bf71d-110">Motivační</span><span class="sxs-lookup"><span data-stu-id="bf71d-110">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="bf71d-111">K dispozici je určité množství často používaného textu, a to i v případě, že je potřeba explicitní `Main` metoda.</span><span class="sxs-lookup"><span data-stu-id="bf71d-111">There's a certain amount of boilerplate surrounding even the simplest of programs, because of the need for an explicit `Main` method.</span></span> <span data-ttu-id="bf71d-112">Zdá se, že se dostanete na základě jazyka učení a srozumitelnosti programu.</span><span class="sxs-lookup"><span data-stu-id="bf71d-112">This seems to get in the way of language learning and program clarity.</span></span> <span data-ttu-id="bf71d-113">Hlavním cílem této funkce je tedy dovolit programům bez C# zbytečného vynechání kolem nich, a to v případě učících a srozumitelnosti kódu.</span><span class="sxs-lookup"><span data-stu-id="bf71d-113">The primary goal of the feature therefore is to allow C# programs without unnecessary boilerplate around them, for the sake of learners and the clarity of code.</span></span>

## <a name="detailed-design"></a><span data-ttu-id="bf71d-114">Podrobný návrh</span><span class="sxs-lookup"><span data-stu-id="bf71d-114">Detailed design</span></span>
[design]: #detailed-design

### <a name="syntax"></a><span data-ttu-id="bf71d-115">Syntaxe</span><span class="sxs-lookup"><span data-stu-id="bf71d-115">Syntax</span></span>

<span data-ttu-id="bf71d-116">Jedinou další syntaxí je povolení sekvence *příkazů*v kompilační jednotce těsně před *namespace_member_declaration*s:</span><span class="sxs-lookup"><span data-stu-id="bf71d-116">The only additional syntax is allowing a sequence of *statement*s in a compilation unit, just before the *namespace_member_declaration*s:</span></span>

``` antlr
compilation_unit
    : extern_alias_directive* using_directive* global_attributes? statement* namespace_member_declaration*
    ;
```

<span data-ttu-id="bf71d-117">Ve všech, ale v jednom *compilation_unit* *příkaz*s musí být všechny deklarace místní funkce.</span><span class="sxs-lookup"><span data-stu-id="bf71d-117">In all but one *compilation_unit* the *statement*s must all be local function declarations.</span></span> 

<span data-ttu-id="bf71d-118">Příklad:</span><span class="sxs-lookup"><span data-stu-id="bf71d-118">Example:</span></span>

``` c#
// File 1 - any statements
if (args.Length == 0
    || !int.TryParse(args[0], out int n)
    || n < 0) return;
Console.WriteLine(Fib(n).curr);

// File 2 - only local functions
(int curr, int prev) Fib(int i)
{
    if (i == 0) return (1, 0);
    var (curr, prev) = Fib(i - 1);
    return (curr + prev, curr);
}
```

### <a name="semantics"></a><span data-ttu-id="bf71d-119">Sémantiku</span><span class="sxs-lookup"><span data-stu-id="bf71d-119">Semantics</span></span>

<span data-ttu-id="bf71d-120">Pokud jsou jakékoli příkazy nejvyšší úrovně přítomny v jakékoli kompilační jednotce programu, je význam, jako kdyby byly kombinovány v těle bloku `Main` metody `Program` třídy v globálním oboru názvů následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="bf71d-120">If any top-level statements are present in any compilation unit of the program, the meaning is as if they were combined in the block body of a `Main` method of a `Program` class in the global namespace, as follows:</span></span>

``` c#
static class Program
{
    static async Task Main()
    {
        // File 1 statements
        // File 2 local functions
        // ...
    }
}
```

<span data-ttu-id="bf71d-121">Všimněte si, že názvy "program" a "Main" jsou používány pouze pro ilustraci, skutečné názvy používané kompilátorem jsou závislé na implementaci a ani typ ani na metodu nelze odkazovat pomocí názvu ze zdrojového kódu.</span><span class="sxs-lookup"><span data-stu-id="bf71d-121">Note that the names "Program" and "Main" are used only for illustrations purposes, actual names used by compiler are implementation dependent and neither the type, nor the method can be referenced by name from source code.</span></span>

<span data-ttu-id="bf71d-122">Metoda je určena jako vstupní bod programu.</span><span class="sxs-lookup"><span data-stu-id="bf71d-122">The method is designated as the entry point of the program.</span></span> <span data-ttu-id="bf71d-123">Explicitně deklarované metody, které by mohly být považovány za kandidáty vstupního bodu, se ignorují.</span><span class="sxs-lookup"><span data-stu-id="bf71d-123">Explicitly declared methods that by convention could be considered as an entry point candidates are ignored.</span></span> <span data-ttu-id="bf71d-124">V případě, že k tomu dojde, je hlášeno upozornění.</span><span class="sxs-lookup"><span data-stu-id="bf71d-124">A warning is reported when that happens.</span></span> <span data-ttu-id="bf71d-125">Je-li zadán `-main:<type>` přepínač kompilátoru, jedná se o chybu.</span><span class="sxs-lookup"><span data-stu-id="bf71d-125">It is an error to specify `-main:<type>` compiler switch.</span></span>

<span data-ttu-id="bf71d-126">Pokud některá jednotka kompilace obsahuje příkazy jiné než deklarace místní funkce, napřed příkazy z této jednotky kompilace nastávají.</span><span class="sxs-lookup"><span data-stu-id="bf71d-126">If any one compilation unit has statements other than local function declarations, statements from that compilation unit occur first.</span></span> <span data-ttu-id="bf71d-127">To způsobí, že je právní pro místní funkce v jednom souboru, aby odkazovaly na lokální proměnné v jiném.</span><span class="sxs-lookup"><span data-stu-id="bf71d-127">This causes it to be legal for local functions in one file to reference local variables in another.</span></span> <span data-ttu-id="bf71d-128">Pořadí příspěvků příkazu (které by měly být místní funkce) z jiných kompilačních jednotek, není definováno.</span><span class="sxs-lookup"><span data-stu-id="bf71d-128">The order of statement contributions (which would all be local functions) from other compilation units is undefined.</span></span>

<span data-ttu-id="bf71d-129">Asynchronní operace jsou povoleny v příkazech nejvyšší úrovně do rozsahu, v němž jsou povoleny v příkazech v rámci běžné metody asynchronního vstupního bodu.</span><span class="sxs-lookup"><span data-stu-id="bf71d-129">Async operations are allowed in top-level statements to the degree they are allowed in statements within a regular async entry point method.</span></span> <span data-ttu-id="bf71d-130">Nejsou však požadovány, pokud `await` výrazy a jiné asynchronní operace vynechány, není vytvořeno žádné upozornění.</span><span class="sxs-lookup"><span data-stu-id="bf71d-130">However, they are not required, if `await` expressions and other async operations are omitted, no warning is produced.</span></span> <span data-ttu-id="bf71d-131">Místo toho je podpis generované metody vstupního bodu ekvivalentní</span><span class="sxs-lookup"><span data-stu-id="bf71d-131">Instead the signature of the generated entry point method is equivalent to</span></span> 
``` c#
    static void Main()
```

<span data-ttu-id="bf71d-132">Výše uvedený příklad by vrátil následující `$Main` deklaraci metody:</span><span class="sxs-lookup"><span data-stu-id="bf71d-132">The example above would yield the following `$Main` method declaration:</span></span>

``` c#
static class $Program
{
    static void $Main()
    {
        // Statements from File 1
        if (args.Length == 0
            || !int.TryParse(args[0], out int n)
            || n < 0) return;
        Console.WriteLine(Fib(n).curr);
        
        // Local functions from File 2
        (int curr, int prev) Fib(int i)
        {
            if (i == 0) return (1, 0);
            var (curr, prev) = Fib(i - 1);
            return (curr + prev, curr);
        }
    }
}
```

<span data-ttu-id="bf71d-133">Ve stejnou dobu jako příklad:</span><span class="sxs-lookup"><span data-stu-id="bf71d-133">At the same time an example like this:</span></span>
``` c#
// File 1
await System.Threading.Tasks.Task.Delay(1000);
System.Console.WriteLine("Hi!");
```

<span data-ttu-id="bf71d-134">by to vedlo k těmto akcím:</span><span class="sxs-lookup"><span data-stu-id="bf71d-134">would  yield:</span></span>
``` c#
static class $Program
{
    static async Task $Main()
    {
        // Statements from File 1
        await System.Threading.Tasks.Task.Delay(1000);
        System.Console.WriteLine("Hi!");
    }
}
```

### <a name="scope-of-top-level-local-variables-and-local-functions"></a><span data-ttu-id="bf71d-135">Rozsah místních proměnných nejvyšší úrovně a místních funkcí</span><span class="sxs-lookup"><span data-stu-id="bf71d-135">Scope of top-level local variables and local functions</span></span>

<span data-ttu-id="bf71d-136">I když jsou lokální proměnné a funkce nejvyšší úrovně "zabalené" do generované metody vstupního bodu, měly by být stále v rozsahu celého programu.</span><span class="sxs-lookup"><span data-stu-id="bf71d-136">Even though top-level local variables and functions are "wrapped" into the generated entry point method, they should still be in scope throughout the program.</span></span>
<span data-ttu-id="bf71d-137">Pro účely vyhodnocení jednoduchého názvu po dosažení globálního oboru názvů:</span><span class="sxs-lookup"><span data-stu-id="bf71d-137">For the purpose of simple-name evaluation, once the global namespace is reached:</span></span>
- <span data-ttu-id="bf71d-138">Nejprve se provede pokus o vyhodnocení názvu v rámci generované metody vstupního bodu a jenom v případě, že se tento pokus nezdaří.</span><span class="sxs-lookup"><span data-stu-id="bf71d-138">First, an attempt is made to evaluate the name within the generated entry point method and only if this attempt fails</span></span> 
- <span data-ttu-id="bf71d-139">Vyhodnocování "regular" v rámci deklarace globálního oboru názvů je provedeno.</span><span class="sxs-lookup"><span data-stu-id="bf71d-139">The "regular" evaluation within the global namespace declaration is performed.</span></span> 

<span data-ttu-id="bf71d-140">To může vést k vytvoření stínové kopie oborů názvů a typů deklarovaných v globálním oboru názvů a také k vytváření stínů importovaných názvů.</span><span class="sxs-lookup"><span data-stu-id="bf71d-140">This could lead to name shadowing of namespaces and types declared within the global namespace as well as to shadowing of imported names.</span></span>

<span data-ttu-id="bf71d-141">Pokud se k vyhodnocení jednoduchého názvu dochází mimo příkazy nejvyšší úrovně a vyhodnocení má za následek místní proměnnou nebo funkci nejvyšší úrovně, která by měla vést k chybě.</span><span class="sxs-lookup"><span data-stu-id="bf71d-141">If the simple name evaluation occurs outside of the top-level statements and the evaluation yields a top-level local variable or function, that should lead to an error.</span></span>

<span data-ttu-id="bf71d-142">Tímto způsobem chráníme naši budoucí schopnost lépe řešit "funkce nejvyšší úrovně" (scénář 2 v https://github.com/dotnet/csharplang/issues/3117)a je možné poskytnout užitečnou diagnostiku uživatelům, kteří se omylem domnívají, že jsou podporované.</span><span class="sxs-lookup"><span data-stu-id="bf71d-142">In this way we protect our future ability to better address "Top-level functions" (scenario 2 in https://github.com/dotnet/csharplang/issues/3117), and are able to give useful diagnostics to users who mistakenly believe them to be supported.</span></span>

