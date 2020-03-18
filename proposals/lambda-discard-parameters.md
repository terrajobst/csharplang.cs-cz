---
ms.openlocfilehash: 6695664c3d5ca73f66e792b7ce2ec9993aceea05
ms.sourcegitcommit: 42ef673ecc883009c865f8384249881a546df216
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/14/2019
ms.locfileid: "79485152"
---
# <a name="lambda-discard-parameters"></a><span data-ttu-id="279dc-101">Parametry lambda zahození</span><span class="sxs-lookup"><span data-stu-id="279dc-101">Lambda discard parameters</span></span>

## <a name="summary"></a><span data-ttu-id="279dc-102">Souhrn</span><span class="sxs-lookup"><span data-stu-id="279dc-102">Summary</span></span>

<span data-ttu-id="279dc-103">Povolí použití zahození (`_`) jako parametrů výrazů lambda a anonymních metod.</span><span class="sxs-lookup"><span data-stu-id="279dc-103">Allow discards (`_`) to be used as parameters of lambdas and anonymous methods.</span></span>
<span data-ttu-id="279dc-104">Příklad:</span><span class="sxs-lookup"><span data-stu-id="279dc-104">For example:</span></span>
- <span data-ttu-id="279dc-105">výrazy lambda: `(_, _) => 0`, `(int _, int _) => 0`</span><span class="sxs-lookup"><span data-stu-id="279dc-105">lambdas: `(_, _) => 0`, `(int _, int _) => 0`</span></span>
- <span data-ttu-id="279dc-106">anonymní metody: `delegate(int _, int _) { return 0; }`</span><span class="sxs-lookup"><span data-stu-id="279dc-106">anonymous methods: `delegate(int _, int _) { return 0; }`</span></span>

## <a name="motivation"></a><span data-ttu-id="279dc-107">Motivační</span><span class="sxs-lookup"><span data-stu-id="279dc-107">Motivation</span></span>

<span data-ttu-id="279dc-108">Nepoužívané parametry není nutné pojmenovat.</span><span class="sxs-lookup"><span data-stu-id="279dc-108">Unused parameters do not need to be named.</span></span> <span data-ttu-id="279dc-109">Záměr výmětů je jasný, to znamená, že se nepoužívá nebo zahodí.</span><span class="sxs-lookup"><span data-stu-id="279dc-109">The intent of discards is clear, i.e. they are unused/discarded.</span></span>

## <a name="detailed-design"></a><span data-ttu-id="279dc-110">Podrobný návrh</span><span class="sxs-lookup"><span data-stu-id="279dc-110">Detailed design</span></span>

<span data-ttu-id="279dc-111">[Parametry metody](https://github.com/dotnet/csharplang/blob/master/spec/classes.md#method-parameters) V seznamu parametrů lambda nebo anonymní metody s více než jedním parametrem s názvem `_`, jsou tyto parametry zahozeny parametry.</span><span class="sxs-lookup"><span data-stu-id="279dc-111">[Method parameters](https://github.com/dotnet/csharplang/blob/master/spec/classes.md#method-parameters) In the parameter list of a lambda or anonymous method with more than one parameter named `_`, such parameters are discard parameters.</span></span>
<span data-ttu-id="279dc-112">Poznámka: Pokud je jeden parametr pojmenován `_` pak se jedná o regulární parametr z důvodů zpětné kompatibility.</span><span class="sxs-lookup"><span data-stu-id="279dc-112">Note: if a single parameter is named `_` then it is a regular parameter for backwards compatibility reasons.</span></span>

<span data-ttu-id="279dc-113">Zahození parametrů nezavádí žádné názvy do žádných oborů.</span><span class="sxs-lookup"><span data-stu-id="279dc-113">Discard parameters do not introduce any names to any scopes.</span></span>
<span data-ttu-id="279dc-114">Všimněte si, že to znamená, že nezpůsobí skryté názvy `_` (podtržítka).</span><span class="sxs-lookup"><span data-stu-id="279dc-114">Note this implies they do not cause any `_` (underscore) names to be hidden.</span></span>

<span data-ttu-id="279dc-115">[Jednoduché názvy](https://github.com/dotnet/csharplang/blob/master/spec/expressions.md#simple-names) Pokud je `K` nula a *simple_name* se objeví v rámci *bloku* *a v případě*, že prostor deklarace místní[proměnné (nebo](basic-concepts.md#declarations)ohraničujícího *bloku*) obsahuje místní proměnnou, parametr (s výjimkou parametrů zahození) nebo konstantu s názvem `I`, *simple_name* odkazuje na tuto místní proměnnou, parametr nebo konstantu a je klasifikována jako proměnná nebo hodnota.</span><span class="sxs-lookup"><span data-stu-id="279dc-115">[Simple names](https://github.com/dotnet/csharplang/blob/master/spec/expressions.md#simple-names) If `K` is zero and the *simple_name* appears within a *block* and if the *block*'s (or an enclosing *block*'s) local variable declaration space ([Declarations](basic-concepts.md#declarations)) contains a local variable, parameter (with the exception of discard parameters) or constant with name `I`, then the *simple_name* refers to that local variable, parameter or constant and is classified as a variable or value.</span></span>

<span data-ttu-id="279dc-116">[Rozsahy](https://github.com/dotnet/csharplang/blob/master/spec/basic-concepts.md#scopes) S výjimkou parametrů zahození je rozsah parametru deklarovaného v *lambda_expression* ([anonymní výrazy funkce](expressions.md#anonymous-function-expressions)) *anonymous_function_body* tohoto *lambda_expression* s výjimkou parametrů zahození, obor parametru deklarovaného v *anonymous_method_expression* ([anonymní výrazy funkce](expressions.md#anonymous-function-expressions)) je *blok* tohoto *anonymous_method_expression*.</span><span class="sxs-lookup"><span data-stu-id="279dc-116">[Scopes](https://github.com/dotnet/csharplang/blob/master/spec/basic-concepts.md#scopes) With the exception of discard parameters, the scope of a parameter declared in a *lambda_expression* ([Anonymous function expressions](expressions.md#anonymous-function-expressions)) is the *anonymous_function_body* of that *lambda_expression* With the exception of discard parameters, the scope of a parameter declared in an *anonymous_method_expression* ([Anonymous function expressions](expressions.md#anonymous-function-expressions)) is the *block* of that *anonymous_method_expression*.</span></span>

## <a name="related-spec-sections"></a><span data-ttu-id="279dc-117">Související oddíly specifikace</span><span class="sxs-lookup"><span data-stu-id="279dc-117">Related spec sections</span></span>
- [<span data-ttu-id="279dc-118">Odpovídající parametry</span><span class="sxs-lookup"><span data-stu-id="279dc-118">Corresponding parameters</span></span>](https://github.com/dotnet/csharplang/blob/master/spec/expressions.md#corresponding-parameters)
