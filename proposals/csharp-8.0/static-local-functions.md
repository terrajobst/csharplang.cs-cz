---
ms.openlocfilehash: fecd5a6c1e0f6c7a7a7beac0e4e60445281c7846
ms.sourcegitcommit: 1b488e76c2c07aafc377bc5e8a7197252c82f425
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/24/2019
ms.locfileid: "79485096"
---
# <a name="static-local-functions"></a><span data-ttu-id="76a75-101">Statické místní funkce</span><span class="sxs-lookup"><span data-stu-id="76a75-101">Static local functions</span></span>

## <a name="summary"></a><span data-ttu-id="76a75-102">Souhrn</span><span class="sxs-lookup"><span data-stu-id="76a75-102">Summary</span></span>

<span data-ttu-id="76a75-103">Podporují místní funkce, které zakazují stav zachytávání z nadřazeného oboru.</span><span class="sxs-lookup"><span data-stu-id="76a75-103">Support local functions that disallow capturing state from the enclosing scope.</span></span>

## <a name="motivation"></a><span data-ttu-id="76a75-104">Motivační</span><span class="sxs-lookup"><span data-stu-id="76a75-104">Motivation</span></span>

<span data-ttu-id="76a75-105">Vyhněte se neúmyslnému zachycení stavu z ohraničujícího kontextu.</span><span class="sxs-lookup"><span data-stu-id="76a75-105">Avoid unintentionally capturing state from the enclosing context.</span></span>
<span data-ttu-id="76a75-106">Povolí použití místních funkcí ve scénářích, kde se vyžaduje metoda `static`.</span><span class="sxs-lookup"><span data-stu-id="76a75-106">Allow local functions to be used in scenarios where a `static` method is required.</span></span>

## <a name="detailed-design"></a><span data-ttu-id="76a75-107">Podrobný návrh</span><span class="sxs-lookup"><span data-stu-id="76a75-107">Detailed design</span></span>

<span data-ttu-id="76a75-108">Místní funkce deklarovaná `static` nemůže zachytit stav z ohraničujícího oboru.</span><span class="sxs-lookup"><span data-stu-id="76a75-108">A local function declared `static` cannot capture state from the enclosing scope.</span></span>
<span data-ttu-id="76a75-109">V důsledku toho nejsou v rámci `static` místní funkce k dispozici místní hodnoty, parametry a `this` z nadřazeného oboru.</span><span class="sxs-lookup"><span data-stu-id="76a75-109">As a result, locals, parameters, and `this` from the enclosing scope are not available within a `static` local function.</span></span>

<span data-ttu-id="76a75-110">Místní funkce `static` nemůže odkazovat na členy instance z implicitního nebo explicitního odkazu `this` nebo `base`.</span><span class="sxs-lookup"><span data-stu-id="76a75-110">A `static` local function cannot reference instance members from an implicit or explicit `this` or `base` reference.</span></span>

<span data-ttu-id="76a75-111">`static` místní funkce může odkazovat `static` členy z ohraničujícího oboru.</span><span class="sxs-lookup"><span data-stu-id="76a75-111">A `static` local function may reference `static` members from the enclosing scope.</span></span>

<span data-ttu-id="76a75-112">`static` místní funkce může odkazovat `constant` definice z nadřazeného oboru.</span><span class="sxs-lookup"><span data-stu-id="76a75-112">A `static` local function may reference `constant` definitions from the enclosing scope.</span></span>

<span data-ttu-id="76a75-113">`nameof()` v místní funkci `static` můžou odkazovat na lokální hodnoty, parametry nebo `this` nebo `base` z nadřazeného oboru.</span><span class="sxs-lookup"><span data-stu-id="76a75-113">`nameof()` in a `static` local function may reference locals, parameters, or `this` or `base` from the enclosing scope.</span></span>

<span data-ttu-id="76a75-114">Pravidla přístupnosti pro členy `private` v ohraničujícím oboru jsou stejná pro `static` a jiné než`static` lokální funkce.</span><span class="sxs-lookup"><span data-stu-id="76a75-114">Accessibility rules for `private` members in the enclosing scope are the same for `static` and non-`static` local functions.</span></span>

<span data-ttu-id="76a75-115">Definice místní funkce `static` je generována jako `static` metoda v metadatech, i když je použita pouze v delegátu.</span><span class="sxs-lookup"><span data-stu-id="76a75-115">A `static` local function definition is emitted as a `static` method in metadata, even if only used in a delegate.</span></span>

<span data-ttu-id="76a75-116">Lokální funkce bez`static` nebo výraz lambda může zachytit stav z nadřazené `static` místní funkce, ale nemůže zachytit stav mimo ohraničující `static` místní funkci.</span><span class="sxs-lookup"><span data-stu-id="76a75-116">A non-`static` local function or lambda can capture state from an enclosing `static` local function but cannot capture state outside the enclosing `static` local function.</span></span>

<span data-ttu-id="76a75-117">Ve stromové struktuře výrazu nelze vyvolat `static` místní funkci.</span><span class="sxs-lookup"><span data-stu-id="76a75-117">A `static` local function cannot be invoked in an expression tree.</span></span>

<span data-ttu-id="76a75-118">Volání místní funkce je vygenerováno jako `call` spíše než `callvirt`, bez ohledu na to, zda je místní funkce `static`.</span><span class="sxs-lookup"><span data-stu-id="76a75-118">A call to a local function is emitted as `call` rather than `callvirt`, regardless of whether the local function is `static`.</span></span>

<span data-ttu-id="76a75-119">Rozlišení přetěžování volání v rámci místní funkce nemá vliv na to, zda je místní funkce `static`.</span><span class="sxs-lookup"><span data-stu-id="76a75-119">Overload resolution of a call within a local function not affected by whether the local function is `static`.</span></span>

<span data-ttu-id="76a75-120">Odebrání modifikátoru `static` z místní funkce v platném programu nemění význam programu.</span><span class="sxs-lookup"><span data-stu-id="76a75-120">Removing the `static` modifier from a local function in a valid program does not change the meaning of the program.</span></span>

## <a name="design-meetings"></a><span data-ttu-id="76a75-121">Schůzky pro návrh</span><span class="sxs-lookup"><span data-stu-id="76a75-121">Design meetings</span></span>

https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-09-10.md#static-local-functions
