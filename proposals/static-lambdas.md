---
ms.openlocfilehash: 6f05bbc1365e59d737103b586db9d4a242c6d306
ms.sourcegitcommit: e9afb74cc1abd56db93b4b50bd5e6765e27c1c5d
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/22/2019
ms.locfileid: "79485082"
---
# <a name="static-lambdas"></a><span data-ttu-id="560d8-101">Statické výrazy lambda</span><span class="sxs-lookup"><span data-stu-id="560d8-101">Static lambdas</span></span>

## <a name="summary"></a><span data-ttu-id="560d8-102">Souhrn</span><span class="sxs-lookup"><span data-stu-id="560d8-102">Summary</span></span>

<span data-ttu-id="560d8-103">Podporují výrazy lambda, které zakazují stav zachycení z nadřazeného oboru.</span><span class="sxs-lookup"><span data-stu-id="560d8-103">Support lambdas that disallow capturing state from the enclosing scope.</span></span>

## <a name="motivation"></a><span data-ttu-id="560d8-104">Motivační</span><span class="sxs-lookup"><span data-stu-id="560d8-104">Motivation</span></span>

<span data-ttu-id="560d8-105">Vyhněte se neúmyslnému zachycení stavu z ohraničujícího kontextu.</span><span class="sxs-lookup"><span data-stu-id="560d8-105">Avoid unintentionally capturing state from the enclosing context.</span></span>

## <a name="detailed-design"></a><span data-ttu-id="560d8-106">Podrobný návrh</span><span class="sxs-lookup"><span data-stu-id="560d8-106">Detailed design</span></span>

<span data-ttu-id="560d8-107">Výrazy lambda s `static` nemohou zachytit stav z ohraničujícího oboru.</span><span class="sxs-lookup"><span data-stu-id="560d8-107">A lambdas with `static` cannot capture state from the enclosing scope.</span></span>
<span data-ttu-id="560d8-108">V důsledku toho nejsou v rámci `static` výrazu lambda k dispozici místní hodnoty, parametry a `this` z nadřazeného oboru.</span><span class="sxs-lookup"><span data-stu-id="560d8-108">As a result, locals, parameters, and `this` from the enclosing scope are not available within a `static` lambda.</span></span>

<span data-ttu-id="560d8-109">Výraz lambda `static` nemůže odkazovat na členy instance z implicitního nebo explicitního odkazu `this` nebo `base`.</span><span class="sxs-lookup"><span data-stu-id="560d8-109">A `static` lambda cannot reference instance members from an implicit or explicit `this` or `base` reference.</span></span>

<span data-ttu-id="560d8-110">Výraz lambda `static` může odkazovat `static` členů z ohraničujícího oboru.</span><span class="sxs-lookup"><span data-stu-id="560d8-110">A `static` lambda may reference `static` members from the enclosing scope.</span></span>

<span data-ttu-id="560d8-111">Výraz lambda `static` může odkazovat `constant` definice z nadřazeného oboru.</span><span class="sxs-lookup"><span data-stu-id="560d8-111">A `static` lambda may reference `constant` definitions from the enclosing scope.</span></span>

<span data-ttu-id="560d8-112">`nameof()` ve výrazu lambda `static` mohou odkazovat na lokální hodnoty, parametry nebo `this` nebo `base` z nadřazeného oboru.</span><span class="sxs-lookup"><span data-stu-id="560d8-112">`nameof()` in a `static` lambda may reference locals, parameters, or `this` or `base` from the enclosing scope.</span></span>

<span data-ttu-id="560d8-113">Pravidla přístupnosti pro členy `private` v ohraničujícím oboru jsou pro výrazy lambda `static` a bez`static` stejné.</span><span class="sxs-lookup"><span data-stu-id="560d8-113">Accessibility rules for `private` members in the enclosing scope are the same for `static` and non-`static` lambdas.</span></span>

<span data-ttu-id="560d8-114">Není nijak zaručeno, zda je `static` definice lambda vygenerována jako `static` metoda v metadatech.</span><span class="sxs-lookup"><span data-stu-id="560d8-114">No guarantee is made as to whether a `static` lambda definition is emitted as a `static` method in metadata.</span></span> <span data-ttu-id="560d8-115">To je ponecháno až do implementace kompilátoru pro optimalizaci.</span><span class="sxs-lookup"><span data-stu-id="560d8-115">This is left up to the compiler implementation to optimize.</span></span>

<span data-ttu-id="560d8-116">Lokální funkce bez`static` nebo lambda může zachytit stav z ohraničujícího `static` lambda, ale nemůže zachytit stav mimo ohraničující `static` lambda.</span><span class="sxs-lookup"><span data-stu-id="560d8-116">A non-`static` local function or lambda can capture state from an enclosing `static` lambda but cannot capture state outside the enclosing `static` lambda.</span></span>

<span data-ttu-id="560d8-117">Výraz lambda `static` lze použít ve stromu výrazu.</span><span class="sxs-lookup"><span data-stu-id="560d8-117">A `static` lambda can be used in an expression tree.</span></span>

<span data-ttu-id="560d8-118">Odebrání modifikátoru `static` z výrazu lambda v platném programu nemění význam programu.</span><span class="sxs-lookup"><span data-stu-id="560d8-118">Removing the `static` modifier from a lambda in a valid program does not change the meaning of the program.</span></span>
