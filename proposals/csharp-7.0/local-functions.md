---
ms.openlocfilehash: a4b0fbbc600eaf1e705ad8e6bd9fcecb44100761
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484438"
---
# <a name="local-functions"></a><span data-ttu-id="6b235-101">Lokální funkce</span><span class="sxs-lookup"><span data-stu-id="6b235-101">Local functions</span></span>

<span data-ttu-id="6b235-102">Rozšířili C# jsme podporu deklarace funkcí v oboru bloku.</span><span class="sxs-lookup"><span data-stu-id="6b235-102">We extend C# to support the declaration of functions in block scope.</span></span> <span data-ttu-id="6b235-103">Lokální funkce můžou používat (zachytit) proměnné z nadřazeného oboru.</span><span class="sxs-lookup"><span data-stu-id="6b235-103">Local functions may use (capture) variables from the enclosing scope.</span></span>

<span data-ttu-id="6b235-104">Kompilátor používá analýzu toků ke zjištění, které proměnné místní funkce používá před přiřazením hodnoty.</span><span class="sxs-lookup"><span data-stu-id="6b235-104">The compiler uses flow analysis to detect which variables a local function uses before assigning it a value.</span></span> <span data-ttu-id="6b235-105">Každé volání funkce vyžaduje, aby tyto proměnné byly jednoznačně přiřazeny.</span><span class="sxs-lookup"><span data-stu-id="6b235-105">Every call of the function requires such variables to be definitely assigned.</span></span> <span data-ttu-id="6b235-106">Podobně kompilátor určuje, které proměnné jsou jednoznačně přiřazeny při návratu.</span><span class="sxs-lookup"><span data-stu-id="6b235-106">Similarly the compiler determines which variables are definitely assigned on return.</span></span> <span data-ttu-id="6b235-107">Tyto proměnné jsou považovány za neomezené přiřazení po vyvolání místní funkce.</span><span class="sxs-lookup"><span data-stu-id="6b235-107">Such variables are considered definitely assigned after the local function is invoked.</span></span>

<span data-ttu-id="6b235-108">Lokální funkce mohou být volány z lexikálního bodu před jeho definicí.</span><span class="sxs-lookup"><span data-stu-id="6b235-108">Local functions may be called from a lexical point before its definition.</span></span> <span data-ttu-id="6b235-109">Příkazy deklarace místní funkce nezpůsobí upozornění, pokud nejsou dostupné.</span><span class="sxs-lookup"><span data-stu-id="6b235-109">Local function declaration statements do not cause a warning when they are not reachable.</span></span>

<span data-ttu-id="6b235-110">TODO: _zapsat specifikaci_</span><span class="sxs-lookup"><span data-stu-id="6b235-110">TODO: _WRITE SPEC_</span></span>

## <a name="syntax-grammar"></a><span data-ttu-id="6b235-111">Gramatika syntaxe</span><span class="sxs-lookup"><span data-stu-id="6b235-111">Syntax grammar</span></span>

<span data-ttu-id="6b235-112">Tato gramatika je vyjádřena jako rozdíl z aktuální gramatiky specifikace.</span><span class="sxs-lookup"><span data-stu-id="6b235-112">This grammar is represented as a diff from the current spec grammar.</span></span>

```diff
declaration-statement
    : local-variable-declaration ';'
    | local-constant-declaration ';'
+   | local-function-declaration
    ;

+local-function-declaration
+   : local-function-header local-function-body
+   ;

+local-function-header
+   : local-function-modifiers? return-type identifier type-parameter-list?
+       ( formal-parameter-list? ) type-parameter-constraints-clauses
+   ;

+local-function-modifiers
+   : (async | unsafe)
+   ;

+local-function-body
+   : block
+   | arrow-expression-body
+   ;
```

<span data-ttu-id="6b235-113">Místní funkce můžou používat proměnné definované v ohraničujícím oboru.</span><span class="sxs-lookup"><span data-stu-id="6b235-113">Local functions may use variables defined in the enclosing scope.</span></span> <span data-ttu-id="6b235-114">Aktuální implementace vyžaduje, aby každá proměnná čtená v lokální funkci byla jednoznačně přiřazena, jako při provádění místní funkce v jejím bodě definice.</span><span class="sxs-lookup"><span data-stu-id="6b235-114">The current implementation requires that every variable read inside a local function be definitely assigned, as if executing the local function at its point of definition.</span></span> <span data-ttu-id="6b235-115">Kromě toho musí být definice místní funkce spuštěná v jakémkoli bodě použití.</span><span class="sxs-lookup"><span data-stu-id="6b235-115">Also, the local function definition must have been "executed" at any use point.</span></span>

<span data-ttu-id="6b235-116">Po experimentování s tímto bitem (například není možné definovat dvě vzájemně se rekurzivní lokální funkce), protože jsme změnili, jak chceme, aby jednoznačné přiřazení fungovalo.</span><span class="sxs-lookup"><span data-stu-id="6b235-116">After experimenting with that a bit (for example, it is not possible to define two mutually recursive local functions), we've since revised how we want the definite assignment to work.</span></span> <span data-ttu-id="6b235-117">Revize (ještě není implementovaná) je, že všechny místní proměnné načtené v místní funkci musí být jednoznačně přiřazené při každém vyvolání místní funkce.</span><span class="sxs-lookup"><span data-stu-id="6b235-117">The revision (not yet implemented) is that all local variables read in a local function must be definitely assigned at each invocation of the local function.</span></span> <span data-ttu-id="6b235-118">To je ve skutečnosti jednodušší než zvuk, a to je zbývající množství práce, která funguje.</span><span class="sxs-lookup"><span data-stu-id="6b235-118">That's actually more subtle than it sounds, and there is a bunch of work remaining to make it work.</span></span> <span data-ttu-id="6b235-119">Až to bude hotové, budete moct místní funkce přesunout na konec svého nadřazeného bloku.</span><span class="sxs-lookup"><span data-stu-id="6b235-119">Once it is done you'll be able to move your local functions to the end of its enclosing block.</span></span>

<span data-ttu-id="6b235-120">Nová pravidla jednoznačného přiřazení jsou nekompatibilní s odvozením návratového typu místní funkce, proto je pravděpodobně vhodné odebrat podporu pro odvození návratového typu.</span><span class="sxs-lookup"><span data-stu-id="6b235-120">The new definite assignment rules are incompatible with inferring the return type of a local function, so we'll likely be removing support for inferring the return type.</span></span>

<span data-ttu-id="6b235-121">Pokud lokální funkci nepřevedete na delegáta, zachytávání se provádí do snímků, které jsou typy hodnot.</span><span class="sxs-lookup"><span data-stu-id="6b235-121">Unless you convert a local function to a delegate, capturing is done into frames that are value types.</span></span> <span data-ttu-id="6b235-122">To znamená, že nezískáte žádný tlak GC z použití místních funkcí se zachytáváním.</span><span class="sxs-lookup"><span data-stu-id="6b235-122">That means you don't get any GC pressure from using local functions with capturing.</span></span>

### <a name="reachability"></a><span data-ttu-id="6b235-123">Připojení</span><span class="sxs-lookup"><span data-stu-id="6b235-123">Reachability</span></span>

<span data-ttu-id="6b235-124">Přidáme do specifikace.</span><span class="sxs-lookup"><span data-stu-id="6b235-124">We add to the spec</span></span>

> <span data-ttu-id="6b235-125">Tělo výrazu lambda příkazu těle nebo místní funkce je považováno za dosažitelné.</span><span class="sxs-lookup"><span data-stu-id="6b235-125">The body of a statement-bodied lambda expression or local function is considered reachable.</span></span>
