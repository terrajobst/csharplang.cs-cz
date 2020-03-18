---
ms.openlocfilehash: a78567594d39fc4e204e12c4f2f247b8d6995c38
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484921"
---
# <a name="expression-variables-in-initializers"></a>Proměnné výrazů v inicializátorech

## <a name="summary"></a>Souhrn
[summary]: #summary

Funkce, které jsou představené C# v 7, rozšiřujeme tak, aby povolovaly výrazy obsahující proměnné výrazu (out deklarace proměnných a vzory deklarace) v inicializátorech polí, inicializátorech vlastností, inicializátorech ctor a klauzulích dotazu.

## <a name="motivation"></a>Motivační
[motivation]: #motivation

Tím se dokončí některé z hrubých okrajů zbývajících C# v jazyce z důvodu nedostatku času.

## <a name="detailed-design"></a>Podrobný návrh
[design]: #detailed-design

Odstraníme omezení zabraňující deklaraci proměnných výrazu (deklarace proměnných a vzory deklarace) v inicializátoru ctor. Taková deklarovaná proměnná je v oboru v celém těle konstruktoru.

Odstraníme omezení, které brání deklaraci proměnných výrazů (deklarace proměnných out a vzory deklarace) v inicializátoru pole nebo vlastnosti. Taková deklarovaná proměnná je v oboru celého inicializačního výrazu.

Odebereme omezení zabraňující deklaraci proměnných výrazů (deklarace proměnných out a vzory deklarace) v klauzuli výrazu dotazu, která je přeložena do těla výrazu lambda. Taková deklarovaná proměnná je v oboru celého výrazu klauzule dotazu.

## <a name="drawbacks"></a>Nevýhody
[drawbacks]: #drawbacks

Žádné.

## <a name="alternatives"></a>Alternativy
[alternatives]: #alternatives

Příslušný rozsah proměnných výrazů deklarovaných v těchto kontextech není zjevný a zachovává další diskuzi LDM.

## <a name="unresolved-questions"></a>Nevyřešené dotazy
[unresolved]: #unresolved-questions

- [] Jaký je příslušný obor pro tyto proměnné?

## <a name="design-meetings"></a>Schůzky pro návrh

Žádné.
