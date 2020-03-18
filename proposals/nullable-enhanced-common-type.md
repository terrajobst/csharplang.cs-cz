---
ms.openlocfilehash: 032cb8590a0b6e83f8ab6201e10720f1b254c605
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484529"
---
# <a name="nullable-enhanced-common-type"></a>Rozšířený běžný typ s možnou hodnotou null

* Navrženo [x]
* [] Prototyp: none
* [] Implementace: žádné
* [] Specifikace: viz níže

## <a name="summary"></a>Souhrn
[summary]: #summary

Existuje situace, kdy aktuální výsledky algoritmu Common-Type jsou intuitivní a výsledky v programátorovi přidávají, jak se má redundantní přetypování do kódu. Při této změně by výraz, jako je například `condition ? 1 : null`, měl za následek hodnotu typu `int?`a výraz, jako je například `condition ? x : 1.0`, kde `x` je typu `int?`, by způsobil hodnotu typu `double?`.

## <a name="motivation"></a>Motivační
[motivation]: #motivation

Jedná se o běžnou příčinu toho, co je to pro programátora, jako je třeba nepotřebný často používaný kód.

## <a name="detailed-design"></a>Podrobný návrh
[design]: #detailed-design

Změnou specifikace pro [hledání nejlepšího společného typu sady výrazů](https://github.com/dotnet/csharplang/blob/master/spec/expressions.md#finding-the-best-common-type-of-a-set-of-expressions) můžete ovlivnit následující situace:

- Pokud je jeden výraz typu hodnoty bez hodnoty null `T` a druhý je literál null, výsledek je typu `T?`.
- Pokud je jeden výraz typu s možnou hodnotou null `T?` a druhý je hodnotový typ `U`a existuje implicitní převod z `T` na `U`, pak výsledek je typu `U?`.

Očekává se, že bude ovlivněno následující aspekty jazyka:

- [Ternární výraz](https://github.com/dotnet/csharplang/blob/master/spec/expressions.md#conditional-operator)
- [výraz vytváření pole](https://github.com/dotnet/csharplang/blob/master/spec/expressions.md#array-creation-expressions) implicitně typovaného typu
- odvození [návratového typu výrazu lambda](https://github.com/dotnet/csharplang/blob/master/spec/expressions.md#inferred-return-type) pro odvození typu
- případy zahrnující obecné typy, jako je například vyvolání `M<T>(T a, T b)` jako `M(1, null)`.

V následujících oddílech specifikace měníme přesnější (vkládání tučně, mazání v přeškrtnutí):

> #### <a name="output-type-inferences"></a>Odvození typu výstupu
> 
> *Odvození typu výstupu* je provedeno *z* výrazu `E` *do* typu `T` následujícím způsobem:
> 
> *  Je-li `E` anonymní funkce s odvozeným návratovým typem `U` ([odvozený návratový typ](expressions.md#inferred-return-type)) a `T` je typu delegáta nebo stromové struktury výrazu s návratovým typem `Tb`, pak *odvozená odvozená odvozená* ([odvozená odvozená](expressions.md#lower-bound-inferences)) je vytvořena *z* `U` *až* `Tb`.
> *  V opačném případě, *Pokud je `E`* jako skupina metod a `T` je typ delegáta nebo typ stromu výrazů s typy parametrů `T1...Tk` a návratovým typem `Tb`a řešení přetěžování `E` s typy `T1...Tk` poskytuje jedinou metodu s návratovým typem `U`, pak *z* `U` *na* `Tb`.
> *  \* * V opačném případě, pokud `E` je výraz s hodnotou Nullable typu `U?`, pak je *z* `U` *na* `T` proveden odvozený objekt pro *odvození* a *hodnota null* je přidána do `T`. **
> *  V opačném případě, pokud `E` je výraz s typem `U`, pak je *odvozeno odvození* *z* `U` *na* `T`.
> *  **V opačném případě, pokud je `E` konstantní výraz s hodnotou `null`, je do `T` přidána *vázaná hodnota null*** 
> *  Jinak nejsou provedeny žádné odvozené.

> #### <a name="fixing"></a>Opravě
> 
> *Nepevná* proměnná typu `Xi` se sadou hranic je *opravena* následujícím způsobem:
> 
> *  Sada *typů kandidátů* `Uj` začíná jako sada všech typů v sadě mezí pro `Xi`.
> *  Následně prověříme všechny vazby pro `Xi`: pro každý přesný vázaný `U` `Xi` všechny typy `Uj`, které nejsou identické s `U` jsou ze sady kandidátů odebrány. Pro každý dolní vázaný `U` `Xi` všechny typy `Uj` na *které není implicitní* převod z `U` ze sady kandidátů odebrán. Pro každý horní vázaný `U` `Xi` všechny typy `Uj`, ze *kterých není implicitní* převod na `U` ze sady kandidátů odebrán.
> *  Pokud je mezi zbývajícími typy kandidátů `Uj` jedinečný typ `V`, ze kterého existuje implicitní převod na všechny jiné typy kandidátů, pak ~~je`Xi` na `V`vyřešen.~~
>     -  **Je-li `V` typ hodnoty a pro `Xi`je *svázána hodnota null* , `Xi` je opravena `V?`**
>     -  **Jinak je `Xi` pevně daná `V`**
> *  V opačném případě se odvození typu nezdařilo.

## <a name="drawbacks"></a>Nevýhody
[drawbacks]: #drawbacks

V tomto návrhu můžou být nějaké nekompatibility, které přináší.

## <a name="alternatives"></a>Alternativy
[alternatives]: #alternatives

Žádné.

## <a name="unresolved-questions"></a>Nevyřešené dotazy
[unresolved]: #unresolved-questions

- [] Jaká je závažnost nekompatibility zavedená tímto návrhem, pokud existuje, a jak se dá považovat za moderovanou?

## <a name="design-meetings"></a>Schůzky pro návrh

Žádné.
