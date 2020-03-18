---
ms.openlocfilehash: 6ec94aaabb2c52393a87ee450dbae972b6a50bd5
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484711"
---
# <a name="allow-digit-separator-after-0b-or-0x"></a>Povolení oddělovače číslic po 0b nebo 0x

V C# 7,2 jsme rozšířili sadu míst, kde oddělovače číslic (znak podtržítka) se může objevit v integrálních literálech. [Počínaje C# 7,0 jsou oddělovače povoleny mezi číslicemi literálu](../csharp-7.0/digit-separators.md). Nyní v C# 7,2 umožňují také oddělovače číslic před první významnou číslicí binárního nebo šestnáctkového literálu za předponou.

```csharp
    123      // permitted in C# 1.0 and later
    1_2_3    // permitted in C# 7.0 and later
    0x1_2_3  // permitted in C# 7.0 and later
    0b101    // binary literals added in C# 7.0
    0b1_0_1  // permitted in C# 7.0 and later

    // in C# 7.2, _ is permitted after the `0x` or `0b`
    0x_1_2   // permitted in C# 7.2 and later
    0b_1_0_1 // permitted in C# 7.2 and later
```

Nepovolujeme, aby desítkový celočíselný literál měl počáteční podtržítko. Token, jako je `_123`, je identifikátor.
