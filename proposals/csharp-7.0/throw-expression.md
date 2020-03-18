---
ms.openlocfilehash: 2532a24e867930d2f27614f19c77585dbce80dfa
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484543"
---
# <a name="throw-expression"></a><span data-ttu-id="32e1c-101">Výraz throw</span><span class="sxs-lookup"><span data-stu-id="32e1c-101">Throw expression</span></span>

<span data-ttu-id="32e1c-102">Rozšiřujeme sadu formulářů výrazů, které se mají zahrnout.</span><span class="sxs-lookup"><span data-stu-id="32e1c-102">We extend the set of expression forms to include</span></span>

```antlr
throw_expression
    : 'throw' null_coalescing_expression
    ;

null_coalescing_expression
    : throw_expression
    ;
```

<span data-ttu-id="32e1c-103">Pravidla typu jsou následující:</span><span class="sxs-lookup"><span data-stu-id="32e1c-103">The type rules are as follows:</span></span>

- <span data-ttu-id="32e1c-104">*Throw_expression* nemá žádný typ.</span><span class="sxs-lookup"><span data-stu-id="32e1c-104">A *throw_expression* has no type.</span></span>
- <span data-ttu-id="32e1c-105">*Throw_expression* je převoditelná na každý typ implicitním převodem.</span><span class="sxs-lookup"><span data-stu-id="32e1c-105">A *throw_expression* is convertible to every type by an implicit conversion.</span></span>

<span data-ttu-id="32e1c-106">*Výraz throw* vyvolá hodnotu vytvořenou vyhodnocením *null_coalescing_expression*, který musí poznamenat hodnotu typu třídy `System.Exception`typu třídy, která je odvozena z `System.Exception` nebo typu parametru typu, který má `System.Exception` (nebo podtřídu) jako platnou základní třídu.</span><span class="sxs-lookup"><span data-stu-id="32e1c-106">A *throw expression* throws the value produced by evaluating the *null_coalescing_expression*, which must denote a value of the class type `System.Exception`, of a class type that derives from `System.Exception` or of a type parameter type that has `System.Exception` (or a subclass thereof) as its effective base class.</span></span> <span data-ttu-id="32e1c-107">Pokud vyhodnocení výrazu vytvoří `null`, je místo toho vyvolána `System.NullReferenceException`.</span><span class="sxs-lookup"><span data-stu-id="32e1c-107">If evaluation of the expression produces `null`, a `System.NullReferenceException` is thrown instead.</span></span>

<span data-ttu-id="32e1c-108">Chování při běhu vyhodnocení *výrazu throw* je stejné [jako zadané pro *příkaz throw*](../../spec/statements.md#the-throw-statement).</span><span class="sxs-lookup"><span data-stu-id="32e1c-108">The behavior at runtime of the evaluation of a *throw expression* is the same [as specified for a *throw statement*](../../spec/statements.md#the-throw-statement).</span></span>

<span data-ttu-id="32e1c-109">Pravidla analýzy toků jsou následující:</span><span class="sxs-lookup"><span data-stu-id="32e1c-109">The flow-analysis rules are as follows:</span></span>

- <span data-ttu-id="32e1c-110">Pro každou proměnnou *v*, *v* se jednoznačně přiřadí před *null_coalescing_expression* *throw_expression* Pokud je jednoznačně přiřazen před *throw_expression*.</span><span class="sxs-lookup"><span data-stu-id="32e1c-110">For every variable *v*, *v* is definitely assigned before the *null_coalescing_expression* of a *throw_expression* iff it is definitely assigned before the *throw_expression*.</span></span>
- <span data-ttu-id="32e1c-111">Pro každou proměnnou *v*, *v* je po *throw_expression*jednoznačně přiřazená.</span><span class="sxs-lookup"><span data-stu-id="32e1c-111">For every variable *v*, *v* is definitely assigned after *throw_expression*.</span></span>

<span data-ttu-id="32e1c-112">*Výraz throw* je povolen pouze v následujících syntaktických kontextech:</span><span class="sxs-lookup"><span data-stu-id="32e1c-112">A *throw expression* is permitted in only the following syntactic contexts:</span></span>
- <span data-ttu-id="32e1c-113">Jako druhý nebo třetí operand ternárního podmíněného operátoru `?:`</span><span class="sxs-lookup"><span data-stu-id="32e1c-113">As the second or third operand of a ternary conditional operator `?:`</span></span>
- <span data-ttu-id="32e1c-114">Jako druhý operand operátoru slučování null `??`</span><span class="sxs-lookup"><span data-stu-id="32e1c-114">As the second operand of a null coalescing operator `??`</span></span>
- <span data-ttu-id="32e1c-115">Jako tělo výrazu lambda nebo metody Expression-těle.</span><span class="sxs-lookup"><span data-stu-id="32e1c-115">As the body of an expression-bodied lambda or method.</span></span>
