---
ms.openlocfilehash: 7ea62713416ef82034963aef06f3cb11703342ed
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484606"
---
# <a name="stackalloc-array-initializers"></a>Inicializátory pole stackalloc

## <a name="summary"></a>Souhrn
[summary]: #summary

Povolí použití syntaxe inicializátoru pole s `stackalloc`.

## <a name="motivation"></a>Motivační
[motivation]: #motivation

Běžná pole mohou mít své prvky inicializovány při vytváření. Jeví se jako přijatelné, aby to bylo možné v `stackalloc`m případě.

Otázka, proč taková syntaxe není povolená, `stackalloc` se poměrně často vyskytuje.  
Podívejte se například [#1112](https://github.com/dotnet/csharplang/issues/1112)

## <a name="detailed-design"></a>Podrobný návrh

Běžná pole lze vytvořit pomocí následující syntaxe:

```csharp
new int[3]
new int[3] { 1, 2, 3 }
new int[] { 1, 2, 3 }
new[] { 1, 2, 3 }
```

Měli byste dovolit, aby pole přidělená zásobníkem byla vytvořená prostřednictvím:  

```csharp
stackalloc int[3]               // currently allowed
stackalloc int[3] { 1, 2, 3 }
stackalloc int[] { 1, 2, 3 }
stackalloc[] { 1, 2, 3 }
```

Sémantika všech případů je přibližně stejná jako u polí.  
Příklad: v posledním případě je typ elementu odvozený od inicializátoru a musí se jednat o "nespravovaný" typ.

Poznámka: Tato funkce není závislá na cíli, který je `Span<T>`. Je to v `T*`, jak je to možné, takže se nejeví jako predikát v případě `Span<T>`ho případu rozumný.  

## <a name="translation"></a>Překlad

Implementace Naive by mohla inicializovat pole hned po vytvoření prostřednictvím řady přiřazení prvků.  

Podobně jako v případě polí může být možné a žádoucí detekovat případy, kde jsou všechny nebo většinu prvků typu přenositelné, a používejte efektivnější techniky kopírováním v předem vytvořeném stavu všech konstantních prvků. 

## <a name="drawbacks"></a>Nevýhody
[drawbacks]: #drawbacks

## <a name="alternatives"></a>Alternativy
[alternatives]: #alternatives

Tato funkce je pohodlná. Je možné pouze Neprovádět žádnou akci.

## <a name="unresolved-questions"></a>Nevyřešené dotazy
[unresolved]: #unresolved-questions

## <a name="design-meetings"></a>Schůzky pro návrh

Žádná ještě není. 
