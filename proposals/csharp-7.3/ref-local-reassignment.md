---
ms.openlocfilehash: c1a77d9337ecd16f5ea1c30d18f6422552b0dfb2
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484599"
---
# <a name="ref-local-reassignment"></a><span data-ttu-id="99b16-101">Místní opětovné přiřazení odkazu</span><span class="sxs-lookup"><span data-stu-id="99b16-101">Ref Local Reassignment</span></span>

<span data-ttu-id="99b16-102">V C# 7,3 přidáváme podporu pro převázání referenční místní proměnné ref nebo ref Parameter.</span><span class="sxs-lookup"><span data-stu-id="99b16-102">In C# 7.3, we add support for rebinding the referent of a ref local variable or a ref parameter.</span></span>

<span data-ttu-id="99b16-103">Do sady `assignment_operator`s přidáme následující:</span><span class="sxs-lookup"><span data-stu-id="99b16-103">We add the following to the set of `assignment_operator`s.</span></span>

```antlr
assignment_operator
    : '=' 'ref'
    ;
```

<span data-ttu-id="99b16-104">Operátor `=ref` se nazývá ***operátor přiřazení ref***.</span><span class="sxs-lookup"><span data-stu-id="99b16-104">The `=ref` operator is called the ***ref assignment operator***.</span></span> <span data-ttu-id="99b16-105">Nejedná se o *složený operátor přiřazení*.</span><span class="sxs-lookup"><span data-stu-id="99b16-105">It is not a *compound assignment operator*.</span></span> <span data-ttu-id="99b16-106">Levý operand musí být výraz, který se váže na místní proměnnou ref, parametr ref (jiný než `this`) nebo výstupní parametr.</span><span class="sxs-lookup"><span data-stu-id="99b16-106">The left operand must be an expression that binds to a ref local variable, a ref parameter (other than `this`), or an out parameter.</span></span> <span data-ttu-id="99b16-107">Pravý operand musí být výraz, který je výsledkem určení hodnoty stejného typu jako levý operand.</span><span class="sxs-lookup"><span data-stu-id="99b16-107">The right operand must be an expression that yields an lvalue designating a value of the same type as the left operand.</span></span>

<span data-ttu-id="99b16-108">Pravý operand musí být jednoznačně přiřazen v bodě přiřazení ref.</span><span class="sxs-lookup"><span data-stu-id="99b16-108">The right operand must be definitely assigned at the point of the ref assignment.</span></span>

<span data-ttu-id="99b16-109">Když se levý operand připojí k parametru `out`, jedná se o chybu, pokud se tento parametr `out` nejednoznačně přiřadil na začátku operátoru přiřazení ref.</span><span class="sxs-lookup"><span data-stu-id="99b16-109">When the left operand binds to an `out` parameter, it is an error if that `out` parameter has not been definitely assigned at the beginning of the ref assignment operator.</span></span>

<span data-ttu-id="99b16-110">Pokud je levý operand zapisovatelný odkazem (tj. označuje cokoli jiného než `ref readonly` místní nebo `in` parametr), pravý operand musí být zapisovatelnou hodnotou lvalue.</span><span class="sxs-lookup"><span data-stu-id="99b16-110">If the left operand is a writeable ref (i.e. it designates anything other than a `ref readonly` local or  `in` parameter), then the right operand must be a writeable lvalue.</span></span>

<span data-ttu-id="99b16-111">Operátor přiřazení ref vrací l-hodnotu přiřazeného typu.</span><span class="sxs-lookup"><span data-stu-id="99b16-111">The ref assignment operator yields an lvalue of the assigned type.</span></span> <span data-ttu-id="99b16-112">Dá se zapisovat, pokud je levý operand zapisovatelný (tj. není `ref readonly` nebo `in`).</span><span class="sxs-lookup"><span data-stu-id="99b16-112">It is writeable if the left operand is writeable (i.e. not `ref readonly` or `in`).</span></span>

<span data-ttu-id="99b16-113">Bezpečnostní pravidla pro tento operátor jsou:</span><span class="sxs-lookup"><span data-stu-id="99b16-113">The safety rules for this operator are:</span></span>

- <span data-ttu-id="99b16-114">Pro `e1 = ref e2`pro změnu přiřazení ref musí být `e2` s označením *ref-to-Escape* přinejmenším v rámci rozsahu, který je *bezpečný pro přístup k referenčnímu znaku* `e1`.</span><span class="sxs-lookup"><span data-stu-id="99b16-114">For a ref reassignment `e1 = ref e2`, the *ref-safe-to-escape* of `e2` must be at least as wide a scope as the *ref-safe-to-escape* of `e1`.</span></span>

<span data-ttu-id="99b16-115">Kde *ref-Safe-to-Escape* je definováno v [bezpečí pro typy s odkazem na typ odkazu](../csharp-7.2/span-safety.md)</span><span class="sxs-lookup"><span data-stu-id="99b16-115">Where *ref-safe-to-escape* is defined in [Safety for ref-like types](../csharp-7.2/span-safety.md)</span></span>
