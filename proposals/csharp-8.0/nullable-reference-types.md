---
ms.openlocfilehash: ecdad8c863d0695bc901e4d96d9ca3decbc248eb
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484613"
---
# <a name="nullable-reference-types-in-c"></a>Typy odkazů s možnou hodnotou null vC# #

Cílem této funkce je:

* Umožňuje vývojářům vyjádřit, zda je proměnná, parametr nebo výsledek odkazového typu na hodnotu null.
* Poskytněte upozornění, když se tyto proměnné, parametry a výsledky nepoužijí v souladu s tímto záměrem.

## <a name="expression-of-intent"></a>Výraz záměru

Jazyk již obsahuje syntaxi `T?` pro typy hodnot. Tuto syntaxi je jednoduché roztáhnout na odkazové typy.

Předpokládá se, že záměr takového ne`T`ho odkazového typu je, že by měl mít hodnotu, která není null.

## <a name="checking-of-nullable-references"></a>Kontrola odkazů s možnou hodnotou null

Analýza toku sleduje proměnné odkazů s možnou hodnotou null. Pokud se analýza domnívá, že by neměla mít hodnotu null (například po kontrole nebo přiřazení), jejich hodnota se považuje za odkaz, který není null.

Odkaz s možnou hodnotou null může být také explicitně považován za nenulový s příponou `x!` operátor (operátor "Dammit"), pokud analýza toku nemůže vytvořit situaci, která není null, na kterou vývojář ví.

V opačném případě se zobrazí upozornění, pokud je odkaz s možnou hodnotou null nebo převeden na typ, který není null.

Při převodu z `S[]` na `T?[]` a z `S?[]` na `T[]`se zobrazí upozornění.

Při převodu z `C<S>` na `C<T?>` se zobrazí upozornění s výjimkou toho, že parametr typu je kovariantní (`out`), a při převodu z `C<S?>` na `C<T>` s výjimkou toho, že parametr typu je kontravariantní (`in`).

Pokud parametr typu má omezení bez hodnoty null, je dána výstraha na `C<T?>`. 

## <a name="checking-of-non-null-references"></a>Kontrola odkazů, které nejsou null

Je zadáno upozornění, pokud je literál null přiřazen proměnné, která není null nebo předána jako parametr, který není null.

V případě, že konstruktor explicitně neinicializuje odkazová pole, která nejsou null, je také poskytnuto upozornění.

Nelze dostatečně sledovat, zda jsou inicializovány všechny prvky pole nenulových odkazů. Nicméně jsme mohli vydat upozornění, pokud není přiřazen žádný prvek nově vytvořeného pole před čtením nebo předáním pole. To může zvládnout běžný případ bez vysokého šumu.

Musíme rozhodnout, jestli `default(T)` vygeneruje upozornění, nebo se prostě považuje za typ `T?`.

## <a name="metadata-representation"></a>Reprezentace metadat

Doplňky s hodnotou null by měly být reprezentovány v metadatech jako atributy. To znamená, že kompilátory nižší úrovně je budou ignorovat.

Musíme se rozhodnout, jestli jsou zahrnuté jenom anotace s možnou hodnotou null, nebo jestli se v sestavení objevila hodnota, která není null.

## <a name="generics"></a>Obecné typy

Pokud parametr typu `T` má omezení nepovolující hodnotu null, je v rámci svého rozsahu považován za nenulovou hodnotu.

Pokud parametr typu není omezen nebo má pouze omezení s možnou hodnotou null, je tato situace poněkud složitější: to znamená, že odpovídající argument typu může mít *buď* možnou hodnotu null, nebo nemůže mít hodnotu null. V takovém případě je bezpečné v takovém případě zacházet s parametrem *typu jako s* možnou hodnotou null a nepovolenou hodnotou null a poskytuje upozornění, když je porušena. 

Je vhodné zvážit, zda by měla být povolena omezení explicitních odkazů s možnou hodnotou null. Upozorňujeme však, že nemůžeme mít *implicitní* typy odkazů s možnou hodnotou null v určitých případech (zděděná omezení).

Omezení `class` má hodnotu jinou než null. Můžeme zvážit, zda by mělo být `class?` platné omezení hodnoty null, které označuje "typ odkazu s možnou hodnotou null".

## <a name="type-inference"></a>Odvození typu

Pokud je typ přispívajícího typu odkaz na typ, který je odvozen jako typ odkazu na hodnotu Nullable, výsledný typ by měl mít hodnotu null. Jinými slovy, je rozšířena hodnota null.

Měli byste zvážit, zda `null` literál jako podílový výraz by měl přispět k hodnotě null. Ještě ne: u hodnotových typů vede k chybě, zatímco pro odkazové typy se hodnota null úspěšně převede na jednoduchý typ. 

```csharp
string? n = "world";
var x = b ? "Hello" : n; // string?
var y = b ? "Hello" : null; // string? or error
var z = b ? 7 : null; // Error today, could be int?
```

## <a name="breaking-changes"></a>Změny způsobující chyby

Upozornění, která nejsou null, jsou zjevnými zásadními změnami stávajícího kódu a měly by být doprovázeny mechanismem pro výslovný souhlas.

Méně zjevně, upozornění z typů s možnou hodnotou null (jak je popsáno výše) jsou zásadní změnou stávajícího kódu v určitých situacích, kdy je hodnota null implicitní:

* Parametry neomezeného typu budou považovány za implicitně s možnou hodnotou null, takže je přiřadíte k `object` nebo k přístupu, např. `ToString` budou vracet upozornění.
* Pokud odvození typu odvodí hodnotu null z `null` výrazy, pak existující kód někdy bude vracet hodnotu null, nikoli nenullable typy, což může vést k novým upozorněním.

Proto musí být upozornění s možnou hodnotou null také volitelná.

Nakonec přidání poznámek do existujícího rozhraní API bude zásadní změnou u uživatelů, kteří se při upgradu knihovny přihlásili k upozorněním. Tato možnost je moc volitelná a může se odhlásit. "Chci opravit chyby, ale nemůžu se pustit na nové poznámky"

V souhrnu je potřeba, abyste byli schopni se rozhodnout nebo odsouhlasit:
* Upozornění s možnou hodnotou null
* Upozornění jiná než null
* Upozornění z poznámek v jiných souborech

Členitost výslovných odchylek navrhuje model podobný analyzátoru, kde swaths kódu může začínat a odsouhlasit pomocí direktiv pragma a úrovní závažnosti, které uživatel může vybrat. Kromě toho možnosti pro každou knihovnu ("ignorovat poznámky z JSON.NET, dokud nebudete připraveni na zabývat se), mohou být vyhodnotit v kódu jako atributy.

Pro úspěch a užitečnost této funkce je rozhodující návrh možností přihlášení a přechodů. Musíme zajistit, aby:

* Uživatelé mohou postupně přijmout možnost kontroly hodnoty null, protože chtějí
* Autoři knihovny můžou přidávat anotace s hodnotou null bez obav zákazníků.
* Navzdory těmto možnostem není smysl "konfigurace Nightmare".

## <a name="tweaks"></a>Sekce

Můžeme zvážit, že nepoužíváme `?` anotace na lokálních, ale jenom na základě toho, jestli se používají v souladu s tím, co se jim přiřadí. Tohle nechci; Myslím, že chceme, aby lidé vyjádřili svůj záměr.

V parametrech můžeme považovat za zkrácený `T! x`, který automaticky vygeneruje kontrolu null za běhu.

Některé vzory na obecných typech, jako je například `FirstOrDefault` nebo `TryGet`, mají mírně divné chování s argumenty typu bez hodnoty null, protože explicitně přinesou výchozí hodnoty v určitých situacích. Můžeme se pokusit nuance systém typů, aby se lépe pokryl. Můžeme například `?` u nezarovnaných parametrů typu, i když argument typu již může mít hodnotu null. Pochybnosti, že stojí za to, a vede k weirdnessům souvisejícím s interakcí s typy *hodnot* s možnou hodnotou null. 

## <a name="nullable-value-types"></a>Typy hodnot s povolenou hodnotou Null

U typů hodnot s možnou hodnotou null můžeme zvážit přijetí některých z výše uvedených sémantik.

Již jsme uvedli odvození typu, kde můžeme odvodit `int?` z `(7, null)`namísto pouhého poskytnutí chyby.

Další příležitostí je použít analýzu toku na typy hodnot s možnou hodnotou null. V případě, že jsou považovány za nenulové, můžeme ve skutečnosti použít jako typ, který nepovoluje hodnotu null, konkrétní způsoby (například přístup ke členům). Musíme být jenom opatrní, aby pro účely kompatibility s možnou hodnotou, která je *už* v typu hodnot s možnou hodnotou null, mohla stačit akce.
