---
ms.openlocfilehash: 2532a24e867930d2f27614f19c77585dbce80dfa
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484543"
---
# <a name="throw-expression"></a>Výraz throw

Rozšiřujeme sadu formulářů výrazů, které se mají zahrnout.

```antlr
throw_expression
    : 'throw' null_coalescing_expression
    ;

null_coalescing_expression
    : throw_expression
    ;
```

Pravidla typu jsou následující:

- *Throw_expression* nemá žádný typ.
- *Throw_expression* je převoditelná na každý typ implicitním převodem.

*Výraz throw* vyvolá hodnotu vytvořenou vyhodnocením *null_coalescing_expression*, který musí poznamenat hodnotu typu třídy `System.Exception`typu třídy, která je odvozena z `System.Exception` nebo typu parametru typu, který má `System.Exception` (nebo podtřídu) jako platnou základní třídu. Pokud vyhodnocení výrazu vytvoří `null`, je místo toho vyvolána `System.NullReferenceException`.

Chování při běhu vyhodnocení *výrazu throw* je stejné [jako zadané pro *příkaz throw*](../../spec/statements.md#the-throw-statement).

Pravidla analýzy toků jsou následující:

- Pro každou proměnnou *v*, *v* se jednoznačně přiřadí před *null_coalescing_expression* *throw_expression* Pokud je jednoznačně přiřazen před *throw_expression*.
- Pro každou proměnnou *v*, *v* je po *throw_expression*jednoznačně přiřazená.

*Výraz throw* je povolen pouze v následujících syntaktických kontextech:
- Jako druhý nebo třetí operand ternárního podmíněného operátoru `?:`
- Jako druhý operand operátoru slučování null `??`
- Jako tělo výrazu lambda nebo metody Expression-těle.
