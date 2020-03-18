---
ms.openlocfilehash: 3d10cacef98e802333c8cbe65edb93c19c74cabf
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484494"
---
# <a name="null-conditional-await"></a>null – podmíněné čekání

* Navrženo [x]
* [] Prototyp: none
* [] Implementace: žádné
* [] Specifikace: spuštěno, níže

## <a name="summary"></a>Souhrn
[summary]: #summary

Podporuje výraz formuláře `await? e`, který čeká `e`, pokud není null, jinak má za následek `null`.

## <a name="motivation"></a>Motivační
[motivation]: #motivation

Toto je společný vzor pro kódování a tato funkce by mohla mít příjemné synergii se stávajícími operátory slučování s hodnotou null a s hodnotou null.

## <a name="detailed-design"></a>Podrobný návrh
[design]: #detailed-design

Přidáme novou formu *await_expression*:

```antlr
await_expression
    : 'await' '?' unary_expression
    ;
```

Operátor s hodnotou null-Conditional `await` čeká na operand pouze v případě, že tento operand je jiný než null. V opačném případě je výsledek použití operátoru null.

Typ výsledku je vypočítán pomocí [pravidel pro operátor s hodnotou null-Conditioned](https://github.com/dotnet/csharplang/blob/master/spec/expressions.md#null-conditional-operator).

> **Poznámka:** Pokud je `e` typu `Task`, pak `await? e;` neudělá nic, pokud `e` `null`, a očekává `e`, pokud není `null`.
>
> Pokud je `e` typu `Task<K>`, kde `K` je typ hodnoty, `await? e` by měla být hodnota typu `K?`.

## <a name="drawbacks"></a>Nevýhody
[drawbacks]: #drawbacks

Stejně jako u libovolné jazykové funkce musíme dotazovat, jestli se další složitost tohoto jazyka znovu vyplácí v další čistotě, která je k dispozici pro tělo C# programů, které by mohly využít výhod této funkce.

## <a name="alternatives"></a>Alternativy
[alternatives]: #alternatives

I když vyžaduje nějaký často používaný kód, použití tohoto operátoru lze často nahradit výrazem, jako je například `(e == null) ? null : await e` nebo příkaz jako `if (e != null) await e`.

## <a name="unresolved-questions"></a>Nevyřešené dotazy
[unresolved]: #unresolved-questions

- [] Vyžaduje revizi LDM.

## <a name="design-meetings"></a>Schůzky pro návrh

Žádné.
