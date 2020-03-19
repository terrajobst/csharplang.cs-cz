---
ms.openlocfilehash: 0c8bc2b5072ea7f86189b41a1cdbf2a449661b05
ms.sourcegitcommit: 33a60a1db1d42d21d959acfeb127e647150173aa
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/18/2020
ms.locfileid: "79509505"
---
# <a name="attributes-on-local-functions"></a><span data-ttu-id="9c856-101">Atributy místních funkcí</span><span class="sxs-lookup"><span data-stu-id="9c856-101">Attributes on local functions</span></span>

## <a name="attributes"></a><span data-ttu-id="9c856-102">Atributy</span><span class="sxs-lookup"><span data-stu-id="9c856-102">Attributes</span></span>

<span data-ttu-id="9c856-103">Deklarace místních funkcí teď mají povolené [atributy](../spec/attributes.md).</span><span class="sxs-lookup"><span data-stu-id="9c856-103">Local function declarations are now permitted to have [attributes](../spec/attributes.md).</span></span> <span data-ttu-id="9c856-104">Parametry a parametry typu u místních funkcí mají také povolené atributy.</span><span class="sxs-lookup"><span data-stu-id="9c856-104">Parameters and type parameters on local functions are also allowed to have attributes.</span></span>

<span data-ttu-id="9c856-105">Atributy se zadaným významem při použití na metodu, její parametry nebo parametry typu budou mít stejný význam při použití na místní funkci, její parametry nebo parametry typu v uvedeném pořadí.</span><span class="sxs-lookup"><span data-stu-id="9c856-105">Attributes with a specified meaning when applied to a method, its parameters, or its type parameters will have the same meaning when applied to a local function, its parameters, or its type parameters, respectively.</span></span>

<span data-ttu-id="9c856-106">Místní funkce může být podmíněná ve stejném smyslu jako [podmíněná metoda](../spec/attributes.md#the-conditional-attribute) tím, že ji upravení s `[ConditionalAttribute]`.</span><span class="sxs-lookup"><span data-stu-id="9c856-106">A local function can be made conditional in the same sense as a [conditional method](../spec/attributes.md#the-conditional-attribute) by decorating it with a `[ConditionalAttribute]`.</span></span> <span data-ttu-id="9c856-107">Podmínkou místní funkce musí být také `static`.</span><span class="sxs-lookup"><span data-stu-id="9c856-107">A conditional local function must also be `static`.</span></span> <span data-ttu-id="9c856-108">Všechna omezení u podmíněných metod platí také pro podmíněné místní funkce, včetně toho, že návratový typ musí být `void`.</span><span class="sxs-lookup"><span data-stu-id="9c856-108">All restrictions on conditional methods also apply to conditional local functions, including that the return type must be `void`.</span></span>

## <a name="extern"></a><span data-ttu-id="9c856-109">Extern</span><span class="sxs-lookup"><span data-stu-id="9c856-109">Extern</span></span>

<span data-ttu-id="9c856-110">Modifikátor `extern` je teď povolený u místních funkcí.</span><span class="sxs-lookup"><span data-stu-id="9c856-110">The `extern` modifier is now permitted on local functions.</span></span> <span data-ttu-id="9c856-111">Výsledkem je, že místní funkce je externě ve stejném smyslu jako [externí metoda](../spec/classes.md#external-methods).</span><span class="sxs-lookup"><span data-stu-id="9c856-111">This makes the local function external in the same sense as an [external method](../spec/classes.md#external-methods).</span></span>

<span data-ttu-id="9c856-112">Podobně jako externí metoda musí být *tělo místní funkce* externí místní funkce středníkem.</span><span class="sxs-lookup"><span data-stu-id="9c856-112">Similarly to an external method, the *local-function-body* of an external local function must be a semicolon.</span></span> <span data-ttu-id="9c856-113">Text střední *místní funkce* je povolen pouze pro externí místní funkci.</span><span class="sxs-lookup"><span data-stu-id="9c856-113">A semicolon *local-function-body* is only permitted on an external local function.</span></span> 

<span data-ttu-id="9c856-114">Externí místní funkce musí být také `static`.</span><span class="sxs-lookup"><span data-stu-id="9c856-114">An external local function must also be `static`.</span></span>

## <a name="syntax"></a><span data-ttu-id="9c856-115">Syntaxe</span><span class="sxs-lookup"><span data-stu-id="9c856-115">Syntax</span></span>

<span data-ttu-id="9c856-116">[Gramatika místních funkcí](csharp-7.0/local-functions.md#syntax-grammar) se upraví takto:</span><span class="sxs-lookup"><span data-stu-id="9c856-116">The [local functions grammar](csharp-7.0/local-functions.md#syntax-grammar) is modified as follows:</span></span>
```
local-function-header
    : attributes? local-function-modifiers? return-type identifier type-parameter-list?
        ( formal-parameter-list? ) type-parameter-constraints-clauses
    ;

local-function-modifiers
    : (async | unsafe | static | extern)*
    ;

local-function-body
    : block
    | arrow-expression-body
    | ';'
    ;
```
