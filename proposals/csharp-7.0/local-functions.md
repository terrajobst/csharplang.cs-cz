---
ms.openlocfilehash: a4b0fbbc600eaf1e705ad8e6bd9fcecb44100761
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484438"
---
# <a name="local-functions"></a>Lokální funkce

Rozšířili C# jsme podporu deklarace funkcí v oboru bloku. Lokální funkce můžou používat (zachytit) proměnné z nadřazeného oboru.

Kompilátor používá analýzu toků ke zjištění, které proměnné místní funkce používá před přiřazením hodnoty. Každé volání funkce vyžaduje, aby tyto proměnné byly jednoznačně přiřazeny. Podobně kompilátor určuje, které proměnné jsou jednoznačně přiřazeny při návratu. Tyto proměnné jsou považovány za neomezené přiřazení po vyvolání místní funkce.

Lokální funkce mohou být volány z lexikálního bodu před jeho definicí. Příkazy deklarace místní funkce nezpůsobí upozornění, pokud nejsou dostupné.

TODO: _zapsat specifikaci_

## <a name="syntax-grammar"></a>Gramatika syntaxe

Tato gramatika je vyjádřena jako rozdíl z aktuální gramatiky specifikace.

```diff
declaration-statement
    : local-variable-declaration ';'
    | local-constant-declaration ';'
+   | local-function-declaration
    ;

+local-function-declaration
+   : local-function-header local-function-body
+   ;

+local-function-header
+   : local-function-modifiers? return-type identifier type-parameter-list?
+       ( formal-parameter-list? ) type-parameter-constraints-clauses
+   ;

+local-function-modifiers
+   : (async | unsafe)
+   ;

+local-function-body
+   : block
+   | arrow-expression-body
+   ;
```

Místní funkce můžou používat proměnné definované v ohraničujícím oboru. Aktuální implementace vyžaduje, aby každá proměnná čtená v lokální funkci byla jednoznačně přiřazena, jako při provádění místní funkce v jejím bodě definice. Kromě toho musí být definice místní funkce spuštěná v jakémkoli bodě použití.

Po experimentování s tímto bitem (například není možné definovat dvě vzájemně se rekurzivní lokální funkce), protože jsme změnili, jak chceme, aby jednoznačné přiřazení fungovalo. Revize (ještě není implementovaná) je, že všechny místní proměnné načtené v místní funkci musí být jednoznačně přiřazené při každém vyvolání místní funkce. To je ve skutečnosti jednodušší než zvuk, a to je zbývající množství práce, která funguje. Až to bude hotové, budete moct místní funkce přesunout na konec svého nadřazeného bloku.

Nová pravidla jednoznačného přiřazení jsou nekompatibilní s odvozením návratového typu místní funkce, proto je pravděpodobně vhodné odebrat podporu pro odvození návratového typu.

Pokud lokální funkci nepřevedete na delegáta, zachytávání se provádí do snímků, které jsou typy hodnot. To znamená, že nezískáte žádný tlak GC z použití místních funkcí se zachytáváním.

### <a name="reachability"></a>Připojení

Přidáme do specifikace.

> Tělo výrazu lambda příkazu těle nebo místní funkce je považováno za dosažitelné.
