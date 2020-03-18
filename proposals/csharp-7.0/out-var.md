---
ms.openlocfilehash: a0d80afc47e9f0073237db9b8d7a4f0b045c1b0b
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484508"
---
# <a name="out-variable-declarations"></a>Deklarace proměnné out

Funkce *deklarace out proměnné* umožňuje deklaraci proměnné v umístění, které je předávána jako `out` argument.

```antlr
argument_value
    : 'out' type identifier
    | ...
    ;
```

Proměnná deklarovaná tímto způsobem se nazývá *Výstupní proměnná*. Jako typ proměnné můžete použít klíčové slovo `var`. Obor bude stejný jako *Proměnná Pattern* zavedená pomocí porovnávání vzorů.

Podle specifikace jazyka (přístup k elementu 7.6.7 oddílu) argument-seznam element-Access (Indexový výraz) neobsahuje argumenty ref nebo out. Jsou však povoleny kompilátorem pro různé scénáře, například indexery deklarované v metadatech, které přijímají `out`.

V rámci rozsahu místní proměnné zavedené argument_value se jedná o chybu při kompilaci, která odkazuje na tuto místní proměnnou na textové pozici, která předchází jeho deklaraci.

Je také chyba odkazování na implicitně typované proměnné (§ 8.5.1) do stejného seznamu argumentů, který okamžitě obsahuje svou deklaraci.

Rozlišení přetěžování je změněno následujícím způsobem:

Přidáme nový převod:

> Existuje *Převod z výrazu* z implicitně typované deklarace proměnné na každý typ.

Lze

> Typ argumentu explicitně typovaného objektu je deklarovaný typ.

a

> Argument proměnné s implicitním typem není typu.

*Konverze z výrazu* z implicitně typované deklarace proměnné není považována za lepší, než je jakýkoli jiný *Převod výrazu*.

Typ implicitně typované proměnné je typ odpovídajícího parametru v signatuře metody vybrané řešením přetížení.

K reprezentaci deklarace v argumentu out var se přidá nový uzel syntaxe `DeclarationExpressionSyntax`.
