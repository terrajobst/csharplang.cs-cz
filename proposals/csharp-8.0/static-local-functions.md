---
ms.openlocfilehash: fecd5a6c1e0f6c7a7a7beac0e4e60445281c7846
ms.sourcegitcommit: 1b488e76c2c07aafc377bc5e8a7197252c82f425
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/24/2019
ms.locfileid: "79485096"
---
# <a name="static-local-functions"></a>Statické místní funkce

## <a name="summary"></a>Souhrn

Podporují místní funkce, které zakazují stav zachytávání z nadřazeného oboru.

## <a name="motivation"></a>Motivační

Vyhněte se neúmyslnému zachycení stavu z ohraničujícího kontextu.
Povolí použití místních funkcí ve scénářích, kde se vyžaduje metoda `static`.

## <a name="detailed-design"></a>Podrobný návrh

Místní funkce deklarovaná `static` nemůže zachytit stav z ohraničujícího oboru.
V důsledku toho nejsou v rámci `static` místní funkce k dispozici místní hodnoty, parametry a `this` z nadřazeného oboru.

Místní funkce `static` nemůže odkazovat na členy instance z implicitního nebo explicitního odkazu `this` nebo `base`.

`static` místní funkce může odkazovat `static` členy z ohraničujícího oboru.

`static` místní funkce může odkazovat `constant` definice z nadřazeného oboru.

`nameof()` v místní funkci `static` můžou odkazovat na lokální hodnoty, parametry nebo `this` nebo `base` z nadřazeného oboru.

Pravidla přístupnosti pro členy `private` v ohraničujícím oboru jsou stejná pro `static` a jiné než`static` lokální funkce.

Definice místní funkce `static` je generována jako `static` metoda v metadatech, i když je použita pouze v delegátu.

Lokální funkce bez`static` nebo výraz lambda může zachytit stav z nadřazené `static` místní funkce, ale nemůže zachytit stav mimo ohraničující `static` místní funkci.

Ve stromové struktuře výrazu nelze vyvolat `static` místní funkci.

Volání místní funkce je vygenerováno jako `call` spíše než `callvirt`, bez ohledu na to, zda je místní funkce `static`.

Rozlišení přetěžování volání v rámci místní funkce nemá vliv na to, zda je místní funkce `static`.

Odebrání modifikátoru `static` z místní funkce v platném programu nemění význam programu.

## <a name="design-meetings"></a>Schůzky pro návrh

https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-09-10.md#static-local-functions
