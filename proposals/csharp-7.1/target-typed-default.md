---
ms.openlocfilehash: 1852356b830e29c3537a4de559fef32fd2c2f8b6
ms.sourcegitcommit: f7952cdddf85316a4beec493a0ecc14fcb3241c8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/30/2019
ms.locfileid: "79484984"
---
# <a name="target-typed-default-literal"></a>Literál "default" cílového typu

* Navrženo [x]
* [x] prototyp
* [x] implementace
* [] Specifikace

## <a name="summary"></a>Souhrn
[summary]: #summary

`default` funkce Target-Type je kratší varianta operátoru `default(T)`, která umožňuje vynechat typ. Jeho typ je odvozený podle cíle a zadáním. Kromě toho se chová jako `default(T)`.

## <a name="motivation"></a>Motivační
[motivation]: #motivation

Hlavní motivace je zabránit psaní redundantních informací.

Například při vyvolání `void Method(ImmutableArray<SomeType> array)`*výchozí* literál umožňuje `M(default)` místo `M(default(ImmutableArray<SomeType>))`.

To platí pro několik scénářů, například:

- deklarování místních hodnot (`ImmutableArray<SomeType> x = default;`)
- Ternární operace (`var x = flag ? default : ImmutableArray<SomeType>.Empty;`)
- návrat v metodách a výrazech lambda (`return default;`)
- deklarování výchozích hodnot pro volitelné parametry (`void Method(ImmutableArray<SomeType> arrayOpt = default)`)
- zahrnutí výchozích hodnot do výrazů vytváření pole (`var x = new[] { default, ImmutableArray.Create(y) };`)


## <a name="detailed-design"></a>Podrobný návrh
[design]: #detailed-design

Je představen nový výraz, což je *výchozí* literál. Výraz s touto klasifikací lze implicitně převést na libovolný typ pomocí *výchozího literálového převodu*. 

Odvození typu pro *výchozí* literál funguje stejně jako u literálu s *hodnotou null* , s výjimkou, že je povolen libovolný typ (nikoli pouze odkazové typy).

Tento převod vytvoří výchozí hodnotu odvozeného typu.

*Výchozí* literál může mít konstantní hodnotu v závislosti na odvozeném typu. Takže `const int x = default;` je právní, ale `const int? y = default;` ne.

*Výchozí* literál může být operand operátorů rovnosti, pokud má druhý operand typ. Takže `default == x` a `x == default` jsou platné výrazy, ale `default == default` je neplatné.

## <a name="drawbacks"></a>Nevýhody
[drawbacks]: #drawbacks

Menší nevýhoda je, že *výchozí* literál lze použít místo literálu s *hodnotou null* ve většině kontextů. Dvě výjimky jsou `throw null;` a `null == null`, které jsou povoleny pro literál s *hodnotou null* , ale nikoli *výchozí* literál.

## <a name="alternatives"></a>Alternativy
[alternatives]: #alternatives

Je potřeba vzít v úvahu několik alternativ:

- Stav quo: funkce není odůvodněná na vlastní hodnoty a vývojáři nadále používají výchozí operátor s explicitním typem.
- Rozšíření literálu s hodnotou null: Jedná se o přístup VB s `Nothing`. Můžeme dovolit `int x = null;`.

## <a name="unresolved-questions"></a>Nevyřešené dotazy
[unresolved]: #unresolved-questions

- [x] má být jako Operand operátoru *is* nebo *as* povoleno *výchozí nastavení* ? Odpověď: zakázat `default is T`, povolit `x is default`, povolit `default as RefType` (s upozorněním Always-null)

## <a name="design-meetings"></a>Schůzky pro návrh

- [LDM 3/7/2017](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-03-07.md)
- [LDM 3/28/2017](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-03-28.md)
- [LDM 5/31/2017](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-05-31.md#default-in-operators)
