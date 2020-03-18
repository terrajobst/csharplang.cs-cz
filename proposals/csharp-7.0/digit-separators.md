---
ms.openlocfilehash: 5476f4438ad79a26b3615154f789d8ed04cb61aa
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484501"
---
# <a name="digit-separators"></a><span data-ttu-id="1f235-101">Oddělovače číslic</span><span class="sxs-lookup"><span data-stu-id="1f235-101">Digit separators</span></span>

<span data-ttu-id="1f235-102">Schopnost seskupit číslice ve velkých literálech by měl mít skvělý dopad na čitelnost a žádné významné nevýhodou.</span><span class="sxs-lookup"><span data-stu-id="1f235-102">Being able to group digits in large numeric literals would have great readability impact and no significant downside.</span></span> 

<span data-ttu-id="1f235-103">Přidávání binárních literálů (#215) by zvýšilo pravděpodobnost numerických literálů, takže tyto dvě funkce se vzájemně zvyšují.</span><span class="sxs-lookup"><span data-stu-id="1f235-103">Adding binary literals (#215) would increase the likelihood of numeric literals being long, so the two features enhance each other.</span></span> 

<span data-ttu-id="1f235-104">Budeme sledovat jazyky Java a další a jako oddělovač číslic použít `_` podtržení.</span><span class="sxs-lookup"><span data-stu-id="1f235-104">We would follow Java and others, and use an underscore `_` as a digit separator.</span></span> <span data-ttu-id="1f235-105">Může se nacházet všude v číselném literálu (kromě prvního a posledního znaku), protože různá seskupení můžou mít smysl v různých scénářích a zejména různé číselné základny:</span><span class="sxs-lookup"><span data-stu-id="1f235-105">It would be able to occur everywhere in a numeric literal (except as the first and last character), since different groupings may make sense in different scenarios and especially for different numeric bases:</span></span>

```csharp
int bin = 0b1001_1010_0001_0100;
int hex = 0x1b_a0_44_fe;
int dec = 33_554_432;
int weird = 1_2__3___4____5_____6______7_______8________9;
double real = 1_000.111_1e-1_000;
```

<span data-ttu-id="1f235-106">Jakákoli posloupnost číslic může být oddělená podtržítky, možná více než jedním podtržítkem mezi dvěma po sobě jdoucími číslicemi.</span><span class="sxs-lookup"><span data-stu-id="1f235-106">Any sequence of digits may be separated by underscores, possibly more than one underscore between two consecutive digits.</span></span> <span data-ttu-id="1f235-107">Jsou povoleny v desetinných číslech i exponentech, ale podle předchozího pravidla se nemusí zobrazit vedle desetinného čísla (`10_.0`), vedle znaku exponentu (`1.1e_1`) nebo vedle specifikátoru typu (`10_f`).</span><span class="sxs-lookup"><span data-stu-id="1f235-107">They are allowed in decimals as well as exponents, but following the previous rule, they may not appear next to the decimal (`10_.0`), next to the exponent character (`1.1e_1`), or next to the type specifier (`10_f`).</span></span> <span data-ttu-id="1f235-108">Při použití v binárních a šestnáctkových literálech se nemusí zobrazovat hned za `0x` nebo `0b`.</span><span class="sxs-lookup"><span data-stu-id="1f235-108">When used in binary and hexadecimal literals, they may not appear immediately following the `0x` or `0b`.</span></span>

<span data-ttu-id="1f235-109">Syntaxe je jednoduchá a oddělovače nemají žádný sémantický dopad – jsou jednoduše ignorovány.</span><span class="sxs-lookup"><span data-stu-id="1f235-109">The syntax is straightforward, and the separators have no semantic impact - they are simply ignored.</span></span>

<span data-ttu-id="1f235-110">To má širokou hodnotu a snadnou implementaci.</span><span class="sxs-lookup"><span data-stu-id="1f235-110">This has broad value and is easy to implement.</span></span>
