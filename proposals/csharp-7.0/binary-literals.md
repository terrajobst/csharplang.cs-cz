---
ms.openlocfilehash: 6f6c24e826e9fe9b9e8c97549add1029f00bcf60
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484424"
---
# <a name="binary-literals"></a>Binární literály

Existuje poměrně společný požadavek na přidání binárních literálů do C# a VB. V případě vyčíslení (např. výčty příznaků) se to jeví jako skutečně užitečné, ale bude to ale skvělé jenom pro vzdělávací účely.

Binární literály by vypadaly takto:

```csharp
int nineteen = 0b10011;
```

Syntakticky a sémanticky jsou identické s šestnáctkovými literály, s výjimkou použití `b`/`B` místo `x`/`X`, má pouze číslice `0` a `1` a jsou interpretovány v základní 2 místo 16.

K implementaci těchto a malým koncepčním nárokům na uživatele tohoto jazyka se účtují malé náklady.

## <a name="syntax"></a>Syntaxe

Gramatika by vypadala takto:

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
