---
ms.openlocfilehash: f238a711e710bbac7f5b7400fa938bd85dec00c6
ms.sourcegitcommit: 5278336b61519956240a6f7d83bcb4322019e032
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/10/2020
ms.locfileid: "79485250"
---
# <a name="support-for--and--on-tuple-types"></a>Podpora = = a! = u typů řazené kolekce členů

Umožňuje výrazy `t1 == t2`, kde `t1` a `t2` jsou řazené kolekce členů nebo typy řazené kolekce členů stejné mohutnosti, a jsou vyhodnoceny přibližně jako `temp1.Item1 == temp2.Item1 && temp1.Item2 == temp2.Item2` (za předpokladu, že `var temp1 = t1; var temp2 = t2;`).

Naopak umožní `t1 != t2` a vyhodnotit ho jako `temp1.Item1 != temp2.Item1 || temp1.Item2 != temp2.Item2`.

V případě možného případu s možnou hodnotou null se použijí další kontroly `temp1.HasValue` a `temp2.HasValue`. `nullableT1 == nullableT2` například vyhodnotí jako `temp1.HasValue == temp2.HasValue ? (temp1.HasValue ? ... : true) : false`.

Vrátí-li se v porovnání s prvkem výsledek nebool (například při použití nelogického `operator ==` nebo `operator !=`, nebo v dynamickém porovnání), pak bude tento výsledek převeden na `bool` nebo prostřednictvím `operator true` nebo `operator false`, aby mohl získat `bool`. Porovnání řazené kolekce členů vždy ukončí vrácení `bool`.

Od C# 7,2, tento kód vytvoří chybu (`error CS0019: Operator '==' cannot be applied to operands of type '(...)' and '(...)'`), pokud není k dispozici `operator==`definovaný uživatelem.

## <a name="details"></a>Podrobnosti

Při vytváření vazby operátoru `==` (nebo `!=`) jsou stávající pravidla: (1) dynamický případ, (2) rozlišení přetěžování a (3) selže.
Tento návrh přidá případ řazené kolekce členů mezi (1) a (2): jsou-li oba operandy relačního operátoru řazené kolekce členů (mají typy řazené kolekce členů nebo jsou literály řazené kolekce členů) a mají shodné mohutnosti, je provedeno porovnání na základě prvku. Rovnost řazené kolekce členů je také přetažena na napouštějící řazené kolekce členů.

Oba operandy (a v případě literálů řazené kolekce členů, jejich prvky) jsou vyhodnocovány v pořadí zleva doprava. Každý pár prvků se pak použije jako operandy k navázání operátoru `==` (nebo `!=`) rekurzivně. Všechny prvky s typem kompilace, `dynamic` způsobují chybu. Výsledky těchto porovnání prvků jsou používány jako operandy v řetězu operátorů podmíněná a (nebo).

Například v kontextu `(int, (int, int)) t1, t2;``t1 == (1, (2, 3))` vyhodnocen jako `temp1.Item1 == temp2.Item1 && temp1.Item2.Item1 == temp2.Item2.Item1 && temp2.Item2.Item2 == temp2.Item2.Item2`.

Pokud je literál řazené kolekce členů použit jako operand (na obou stranách), obdrží převedený typ řazené kolekce členů vytvořený převody prvků, které jsou zavedeny při vázání operátoru `==` (nebo `!=`) prvku. 

Například v `(1L, 2, "hello") == (1, 2L, null)`je převedený typ pro literály řazené kolekce členů `(long, long, string)` a druhý literál nemá žádný přirozený typ.


### <a name="deconstruction-and-conversions-to-tuple"></a>Dekonstrukce a převody na řazenou kolekci členů
V `(a, b) == x`skutečnost, že `x` lze dekonstruovat do dvou prvků, nehraje roli. To může být vhodné v budoucím návrhu, i když by to vyvolalo otázky týkající se `x == y` (Jedná se o jednoduché porovnání nebo porovnání s prvkem), a pokud ano, pomocí kterého mohutnosti?).
Podobně převody na řazené kolekce členů nehrají žádné role.

### <a name="tuple-element-names"></a>Názvy elementů řazené kolekce členů

Při převodu literálu řazené kolekce členů se zobrazí upozornění, když se v literálu poskytl explicitní název prvku řazené kolekce členů, ale neshoduje se s názvem elementu cílové řazené kolekce členů.
V porovnání řazené kolekce členů používáme stejné pravidlo, takže předpokládáme `(int a, int b) t` `d` v `t == (c, d: 0)`.

### <a name="non-bool-element-wise-comparison-results"></a>Nebool – výsledky porovnávacích prvků

Pokud je v rovnosti řazené kolekce členů dynamické porovnání elementu, používáme dynamické vyvolání operátoru `false` a negaci, které získá `bool` a pokračuje s dalšími porovnávacími prvky. 

Pokud se v rovnosti řazené kolekce členů vrátí nějaký typ porovnání elementu, existují dva případy:
- Pokud se typ, který není bool, převede na `bool`, použijeme tento převod.
- Pokud žádný takový převod neexistuje, ale typ má operátor `false`, použijeme tento výsledek a negaci.

V nerovnosti řazené kolekce členů se stejná pravidla použijí s výjimkou toho, že použijeme operátor `true` (bez negace) namísto operátoru `false`.

Tato pravidla jsou podobná pravidlům pro použití nebool typu v příkazu `if` a některých dalších existujících kontextech.

## <a name="evaluation-order-and-special-cases"></a>Pořadí vyhodnocení a speciální případy
Hodnota na levé straně se vyhodnocuje jako první, pak na pravé straně, pak na základě porovnání elementů zleva doprava (včetně převodů a při předčasném ukončení na základě stávajících pravidel pro podmíněné a/nebo operátory).

Například pokud existuje převod typu `A` na typ `B` a metoda `(A, A) GetTuple()`, vyhodnocování `(new A(1), (new B(2), new B(3))) == (new B(4), GetTuple())` znamená:
- `new A(1)`
- `new B(2)`
- `new B(3)`
- `new B(4)`
- `GetTuple()`
- pak se vyhodnotí převody a porovnání podle prvků a podmíněná logika (převést `new A(1)` na typ `B`a pak je porovnejte s `new B(4)`a tak dále).

### <a name="comparing-null-to-null"></a>Porovnávání `null` `null`

Jedná se o zvláštní případ z normálního porovnání, které se přenáší na porovnání řazené kolekce členů. Je povoleno porovnání `null == null` a literály `null` nezískají žádný typ.
V rovnosti řazené kolekce členů to znamená, `(0, null) == (0, null)` je také povoleno a `null` a řazené kolekce členů nezískají typ ani jednu z těchto znaků.

### <a name="comparing-a-nullable-struct-to-null-without-operator"></a>Porovnávání struktury s možnou hodnotou null a `null` bez `operator==`

Jedná se o další zvláštní případ z normálního porovnání, které se přenáší na porovnání řazené kolekce členů.
Pokud máte `struct S` bez `operator==`, je povolené porovnání `(S?)x == null` a je interpretováno jako `((S?).x).HasValue`.
V rovnosti řazené kolekce členů se používá stejné pravidlo, takže `(0, (S?)x) == (0, null)` je povolená.

## <a name="compatibility"></a>Kompatibilita

Pokud někdo zapsal vlastní `ValueTuple` typy s implementací relačního operátoru, dříve byl dříve vyzvednutý řešením přetížení. Ale vzhledem k tomu, že nový případ řazené kolekce členů pochází před řešením přetížení, bychom tento případ poznamenali s porovnáním řazené kolekce členů namísto spoléhání na porovnávání definované uživatelem.

----

Vztah k [operátorům testování relačních a typů](../../spec/expressions.md#relational-and-type-testing-operators) má vztah k [#190](https://github.com/dotnet/csharplang/issues/190)
