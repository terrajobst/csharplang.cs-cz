---
ms.openlocfilehash: 9647fff40a1e45bef917f140612ae4e91abea958
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484571"
---
# <a name="lambda-attributes"></a>Atributy lambda

* Navrženo [x]
* [] Prototyp
* [] Implementace
* [] Specifikace

## <a name="summary"></a>Souhrn
[summary]: #summary

Povolí použití atributů pro výrazy lambda (a anonymní metody) a pro parametry lambda/anonymní, protože mohou být v pravidelných metodách.

## <a name="motivation"></a>Motivační
[motivation]: #motivation

Dvě primární motivace:

1. K poskytnutí metadat pro analyzátory v době kompilace.
2. Pro zajištění viditelnosti metadat pro reflexi a nástroje za běhu.

Jako příklad (1): pro kód citlivý na výkon je užitečné mít v případě, že je možné mít k dispozici analyzátor, který je v případě, že jsou uzavírány a Delegáti přiděleni pro výrazy lambda, které se blíží stavu.  Vývojáři takového kódu se často přestanou přecházet ze svého způsobu, aby se zabránilo zachycení jakéhokoli stavu, takže kompilátor může vygenerovat statickou metodu a pro tuto metodu delegáta s možností ukládání do mezipaměti, nebo vývojář zajistí, že jediný stav, který je uzavřený, je `this`, a to tak, aby kompilátor nemusel přidělit objekt ukončení.  Ale bez podpory jazyků pro omezení toho, co by mohlo být zachyceno, je příliš snadné omylem blízko stavu.  To je užitečné, pokud vývojář může opatřit výrazy lambda s atributy, aby označovali, jaký stav je dovoleno zavřít, například:

```csharp
[CaptureNone] // can't close over any instance state
[CaptureThis] // can only capture `this` and no other instance state
[CaptureAny] // can close over any instance state
```

Analyzátor se pak může zapsat na příznak při nesprávném zachycení stavu, například:

```csharp
var results = collection.Select([CaptureNone](i) => Process(item)); // Analyzer error: [CaptureNone] lambdas captures `this`
...
private U Process(T item) { ... }
```

## <a name="detailed-design"></a>Podrobný návrh
[design]: #detailed-design

- Pomocí stejné syntaxe atributu jako u běžných metod lze atributy použít na začátku lambda nebo anonymní metody, například:

```csharp
[SomeAttribute(...)] () => { ... }
[SomeAttribute(...)] delegate (int i) { ... }
```

- Aby nedocházelo k nejednoznačnosti, pokud se atribut vztahuje na metodu lambda nebo na jeden z argumentů, atributy mohou být použity pouze v případě, že se Parens používají kolem argumentů, například:

```csharp
[SomeAttribute] i => { ... } // ERROR
[SomeAttribute] (i) => { ... } // Ok
[SomeAttribute] (int i) => { ... } // Ok
```

- V případě anonymních metod nejsou Parens potřeba pro použití atributu pro metodu před klíčovým slovem `delegate`, například:

```csharp
[SomeAttribute] delegate { ... } // Ok
[SomeAttribute] delegate (int i) => { ... } // Ok
```

- Lze použít více atributů, a to buď prostřednictvím standardní syntaxe oddělené čárkami, nebo prostřednictvím syntaxe úplného atributu, například:

```csharp
[FirstAttribute, SecondAttribute] (i) => { ... } // Ok
[FirstAttribute] [SecondAttribute] (i) => { .... } // Ok
```

- Atributy mohou být použity pro parametry anonymní metody nebo lambda, ale pouze v případě, že se Parens používají kolem argumentů, například:

```csharp
[SomeAttribute] i => { ... } // ERROR
([SomeAttribute] i) => { .... } // Ok
([SomeAttribute] int i) => { ... } // Ok
([SomeAttribute] i, [SomeOtherAttribute] j) => { ... } // Ok
```

- Pro parametry anonymní metody nebo výrazu lambda lze použít více atributů, a to buď pomocí syntaxe oddělených čárkami, nebo úplných atributů, například:

```csharp
([FirstAttribute, SecondAttribute] i) => { ... } // Ok
([FirstAttribute] [SecondAttribute] i) => { ... } // Ok
```

- pro výrazy lambda lze také použít cílené atributy `return`, například:

```csharp
([return: SomeAttribute] (i) => { ... }) // Ok
```

- Kompilátor výstupuje atributy na vygenerovanou metodu a argumenty do těchto metod, jako by to bylo pro jakoukoliv jinou metodu.

## <a name="drawbacks"></a>Nevýhody
[drawbacks]: #drawbacks

neuvedeno

## <a name="alternatives"></a>Alternativy
[alternatives]: #alternatives

neuvedeno

## <a name="unresolved-questions"></a>Nevyřešené dotazy
[unresolved]: #unresolved-questions

neuvedeno

## <a name="design-meetings"></a>Schůzky pro návrh

neuvedeno