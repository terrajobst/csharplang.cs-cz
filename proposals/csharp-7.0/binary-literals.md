---
ms.openlocfilehash: 6f6c24e826e9fe9b9e8c97549add1029f00bcf60
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484424"
---
# <a name="binary-literals"></a><span data-ttu-id="16552-101">Binární literály</span><span class="sxs-lookup"><span data-stu-id="16552-101">Binary literals</span></span>

<span data-ttu-id="16552-102">Existuje poměrně společný požadavek na přidání binárních literálů do C# a VB.</span><span class="sxs-lookup"><span data-stu-id="16552-102">There’s a relatively common request to add binary literals to C# and VB.</span></span> <span data-ttu-id="16552-103">V případě vyčíslení (např. výčty příznaků) se to jeví jako skutečně užitečné, ale bude to ale skvělé jenom pro vzdělávací účely.</span><span class="sxs-lookup"><span data-stu-id="16552-103">For bitmasks (e.g. flag enums) this seems genuinely useful, but it would also be great just for educational purposes.</span></span>

<span data-ttu-id="16552-104">Binární literály by vypadaly takto:</span><span class="sxs-lookup"><span data-stu-id="16552-104">Binary literals would look like this:</span></span>

```csharp
int nineteen = 0b10011;
```

<span data-ttu-id="16552-105">Syntakticky a sémanticky jsou identické s šestnáctkovými literály, s výjimkou použití `b`/`B` místo `x`/`X`, má pouze číslice `0` a `1` a jsou interpretovány v základní 2 místo 16.</span><span class="sxs-lookup"><span data-stu-id="16552-105">Syntactically and semantically they are identical to hexadecimal literals, except for using `b`/`B` instead of `x`/`X`, having only digits `0` and `1` and being interpreted in base 2 instead of 16.</span></span>

<span data-ttu-id="16552-106">K implementaci těchto a malým koncepčním nárokům na uživatele tohoto jazyka se účtují malé náklady.</span><span class="sxs-lookup"><span data-stu-id="16552-106">There’s little cost to implementing these, and little conceptual overhead to users of the language.</span></span>

## <a name="syntax"></a><span data-ttu-id="16552-107">Syntaxe</span><span class="sxs-lookup"><span data-stu-id="16552-107">Syntax</span></span>

<span data-ttu-id="16552-108">Gramatika by vypadala takto:</span><span class="sxs-lookup"><span data-stu-id="16552-108">The grammar would be as follows:</span></span>

```antlr
integer-literal:
    : ...
    | binary-integer-literal
    ;
binary-integer-literal:
    : `0b` binary-digits integer-type-suffix-opt
    | `0B` binary-digits integer-type-suffix-opt
    ;
binary-digits:
    : binary-digit
    | binary-digits binary-digit
    ;
binary-digit:
    : `0`
    | `1`
    ;
```
