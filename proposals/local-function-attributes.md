---
ms.openlocfilehash: 0c8bc2b5072ea7f86189b41a1cdbf2a449661b05
ms.sourcegitcommit: 33a60a1db1d42d21d959acfeb127e647150173aa
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/18/2020
ms.locfileid: "79509505"
---
# <a name="attributes-on-local-functions"></a>Atributy místních funkcí

## <a name="attributes"></a>Atributy

Deklarace místních funkcí teď mají povolené [atributy](../spec/attributes.md). Parametry a parametry typu u místních funkcí mají také povolené atributy.

Atributy se zadaným významem při použití na metodu, její parametry nebo parametry typu budou mít stejný význam při použití na místní funkci, její parametry nebo parametry typu v uvedeném pořadí.

Místní funkce může být podmíněná ve stejném smyslu jako [podmíněná metoda](../spec/attributes.md#the-conditional-attribute) tím, že ji upravení s `[ConditionalAttribute]`. Podmínkou místní funkce musí být také `static`. Všechna omezení u podmíněných metod platí také pro podmíněné místní funkce, včetně toho, že návratový typ musí být `void`.

## <a name="extern"></a>Extern

Modifikátor `extern` je teď povolený u místních funkcí. Výsledkem je, že místní funkce je externě ve stejném smyslu jako [externí metoda](../spec/classes.md#external-methods).

Podobně jako externí metoda musí být *tělo místní funkce* externí místní funkce středníkem. Text střední *místní funkce* je povolen pouze pro externí místní funkci. 

Externí místní funkce musí být také `static`.

## <a name="syntax"></a>Syntaxe

[Gramatika místních funkcí](csharp-7.0/local-functions.md#syntax-grammar) se upraví takto:
```
local-function-header
    : attributes? local-function-modifiers? return-type identifier type-parameter-list?
        ( formal-parameter-list? ) type-parameter-constraints-clauses
    ;

local-function-modifiers
    : (async | unsafe | static | extern)*
    ;

local-function-body
    : block
    | arrow-expression-body
    | ';'
    ;
```
