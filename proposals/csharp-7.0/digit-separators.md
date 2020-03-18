---
ms.openlocfilehash: 5476f4438ad79a26b3615154f789d8ed04cb61aa
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484501"
---
# <a name="digit-separators"></a>Oddělovače číslic

Schopnost seskupit číslice ve velkých literálech by měl mít skvělý dopad na čitelnost a žádné významné nevýhodou. 

Přidávání binárních literálů (#215) by zvýšilo pravděpodobnost numerických literálů, takže tyto dvě funkce se vzájemně zvyšují. 

Budeme sledovat jazyky Java a další a jako oddělovač číslic použít `_` podtržení. Může se nacházet všude v číselném literálu (kromě prvního a posledního znaku), protože různá seskupení můžou mít smysl v různých scénářích a zejména různé číselné základny:

```csharp
int bin = 0b1001_1010_0001_0100;
int hex = 0x1b_a0_44_fe;
int dec = 33_554_432;
int weird = 1_2__3___4____5_____6______7_______8________9;
double real = 1_000.111_1e-1_000;
```

Jakákoli posloupnost číslic může být oddělená podtržítky, možná více než jedním podtržítkem mezi dvěma po sobě jdoucími číslicemi. Jsou povoleny v desetinných číslech i exponentech, ale podle předchozího pravidla se nemusí zobrazit vedle desetinného čísla (`10_.0`), vedle znaku exponentu (`1.1e_1`) nebo vedle specifikátoru typu (`10_f`). Při použití v binárních a šestnáctkových literálech se nemusí zobrazovat hned za `0x` nebo `0b`.

Syntaxe je jednoduchá a oddělovače nemají žádný sémantický dopad – jsou jednoduše ignorovány.

To má širokou hodnotu a snadnou implementaci.
