---
ms.openlocfilehash: 833ea0469149cbd434e8950e844740a3adb4d386
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484592"
---
# <a name="improved-overload-candidates"></a>Vylepšení kandidátů přetížení

## <a name="summary"></a>Souhrn
[summary]: #summary

Pravidla řešení přetížení se aktualizovaly při téměř každé C# aktualizaci jazyka, aby se zlepšilo prostředí programátorů, takže nejednoznačné vyvolání vybere "zřejmou" volbu. To se musí provádět opatrně, aby se zajistila zpětná kompatibilita, ale vzhledem k tomu, že obvykle vyřešíte, co by jinak bylo v chybovém případě, tato vylepšení obvykle fungují předem.

1. Pokud skupina metod obsahuje instanci i statické členy, zrušíme členy instance, pokud jsou vyvolány bez příjemce instance nebo kontextu, a zahodí statické členy, pokud jsou vyvolány pomocí příjemce instance. Pokud není k dispozici žádný přijímač, zahrneme pouze statické členy ve statickém kontextu, jinak statických i členů instancí. Pokud je příjemce jednoznačně instance nebo typ z důvodu situace s barevnými barvami, zahrneme obojí. Statický kontext, kde nelze použít implicitního příjemce instance, zahrnuje text členů, pokud není definován žádný, například statické členy, a místa, kde tuto nelze použít, například Inicializátory polí a Inicializátory konstruktoru.
2. Pokud skupina metod obsahuje některé obecné metody, jejichž argumenty typu nesplňují jejich omezení, jsou tyto členy ze sady kandidátů odebrány.
3. Pro převod skupiny metod se ze sady odeberou metody kandidáta, jejichž návratový typ se neshoduje s návratovým typem delegáta.
