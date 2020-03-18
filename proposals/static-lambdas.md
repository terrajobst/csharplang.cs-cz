---
ms.openlocfilehash: 6f05bbc1365e59d737103b586db9d4a242c6d306
ms.sourcegitcommit: e9afb74cc1abd56db93b4b50bd5e6765e27c1c5d
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/22/2019
ms.locfileid: "79485082"
---
# <a name="static-lambdas"></a>Statické výrazy lambda

## <a name="summary"></a>Souhrn

Podporují výrazy lambda, které zakazují stav zachycení z nadřazeného oboru.

## <a name="motivation"></a>Motivační

Vyhněte se neúmyslnému zachycení stavu z ohraničujícího kontextu.

## <a name="detailed-design"></a>Podrobný návrh

Výrazy lambda s `static` nemohou zachytit stav z ohraničujícího oboru.
V důsledku toho nejsou v rámci `static` výrazu lambda k dispozici místní hodnoty, parametry a `this` z nadřazeného oboru.

Výraz lambda `static` nemůže odkazovat na členy instance z implicitního nebo explicitního odkazu `this` nebo `base`.

Výraz lambda `static` může odkazovat `static` členů z ohraničujícího oboru.

Výraz lambda `static` může odkazovat `constant` definice z nadřazeného oboru.

`nameof()` ve výrazu lambda `static` mohou odkazovat na lokální hodnoty, parametry nebo `this` nebo `base` z nadřazeného oboru.

Pravidla přístupnosti pro členy `private` v ohraničujícím oboru jsou pro výrazy lambda `static` a bez`static` stejné.

Není nijak zaručeno, zda je `static` definice lambda vygenerována jako `static` metoda v metadatech. To je ponecháno až do implementace kompilátoru pro optimalizaci.

Lokální funkce bez`static` nebo lambda může zachytit stav z ohraničujícího `static` lambda, ale nemůže zachytit stav mimo ohraničující `static` lambda.

Výraz lambda `static` lze použít ve stromu výrazu.

Odebrání modifikátoru `static` z výrazu lambda v platném programu nemění význam programu.
