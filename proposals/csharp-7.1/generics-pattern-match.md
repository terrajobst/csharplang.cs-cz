---
ms.openlocfilehash: 4f66c0f60d05ed6509a1d0843318a71d1b36c351
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484704"
---
# <a name="pattern-matching-with-generics"></a>porovnávání vzorů s obecnými typy

* Navrženo [x]
* [] Prototyp:
* [] Implementace:
* [] Specifikace:

## <a name="summary"></a>Souhrn
[summary]: #summary

Specifikace pro [ C# existující operátor as](../../spec/expressions.md#the-as-operator) umožňuje, aby nedocházelo k převodu mezi typem operandu a zadaným typem, pokud je buď otevřený typ. Ve C# 7 vzor `Type identifier` ale vyžaduje převod mezi typem vstupu a daným typem.

Navrhujeme zmírnit tuto možnost a změnit `expression is Type identifier`, a to i v případě, že jsou povolené v podmínkách, pokud C# je povolená v 7, a taky povolit, pokud by `expression as Type` povolené. Konkrétně nové případy jsou případy, kdy je typ výrazu nebo zadaného typu otevřený. 

## <a name="motivation"></a>Motivační
[motivation]: #motivation

Případy, kdy by porovnávání vzorů mělo být "zjevně" povoleno v současnosti není možné kompilovat. Podívejte se například https://github.com/dotnet/roslyn/issues/16195.

## <a name="detailed-design"></a>Podrobný návrh
[design]: #detailed-design

Změnili jsme odstavec ve specifikaci porovnávání vzorů (navržené přidání je zobrazeno tučně):

> Určité kombinace statického typu na levé straně a daného typu se považují za nekompatibilní a výsledkem je chyba při kompilaci. Hodnota statického typu `E` je označována jako *vzor kompatibilní* s typem `T`, pokud existuje převod identity, implicitní převod odkazu, převod zabalení, explicitní převod odkazu nebo převod rozbalení z `E` na `T`**nebo pokud je buď `E` nebo `T` otevřeným typem**. Jedná se o chybu při kompilaci, pokud výraz typu `E` není kompatibilní se vzorem typu v rámci vzoru typu, se kterým se shoduje.

## <a name="drawbacks"></a>Nevýhody
[drawbacks]: #drawbacks

Žádné.

## <a name="alternatives"></a>Alternativy
[alternatives]: #alternatives

Žádné.

## <a name="unresolved-questions"></a>Nevyřešené dotazy
[unresolved]: #unresolved-questions

Žádné.

## <a name="design-meetings"></a>Schůzky pro návrh

LOGICKÝ disk se považuje za tuto otázku a zjistil, že došlo ke změně úrovně opravit chybu. Zpracováváme ji jako samostatnou jazykovou funkci, protože pouhá změna po vydání jazyka by mohla představovat dopřednou nekompatibilitu. Použití navrhované změny vyžaduje, aby programátor určil jazykovou verzi 7,1.
