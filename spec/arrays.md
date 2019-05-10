---
ms.openlocfilehash: 155c1beecddfdfcce2e7948bcb8d6b80428fbd7a
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/25/2019
ms.locfileid: "64488845"
---
# <a name="arrays"></a>Pole

Pole je datová struktura, která obsahuje několik proměnných, které jsou přístupné prostřednictvím vypočítané indexy. Proměnné, které jsou obsaženy v poli, také nazývají prvky pole, jsou všechny stejného typu a tento typ se nazývá typ prvku pole.

Pole má řazení, která určuje počet indexů, které jsou přidružené k každý prvek pole. Rozměr pole se také označuje jako rozměry pole. Pole s rozměrem jedna se nazývá ***jednorozměrné pole***. Pole s rozměrem větší než jedna se nazývá ***vícerozměrné pole***. Konkrétní velikosti vícerozměrná pole jsou často označovány jako dvourozměrné pole, trojrozměrného pole a tak dále.

Každé pole má přidružené délky, což je je celé číslo větší než nebo rovna nule. Délky dimenzí nejsou součástí typu pole, ale místo toho jsou vytvořeny při vytvoření instance typu pole v době běhu. Délka dimenze určuje platný rozsah indexů pro daná dimenze: Pro dimenzi délky `N`, musí být v rozsahu indexy `0` k `N - 1` (včetně). Celkový počet prvků v poli je produktem délek každé dimenze v poli. Pokud jeden nebo více dimenzí pole mít nulovou délku, se říká, že pole bylo prázdné.

Typ elementu pole může být libovolného typu, včetně typu pole.

## <a name="array-types"></a>Typy polí

Typ pole je zapsán jako *non_array_type* následovaného jednou nebo více *rank_specifier*s:

```antlr
array_type
    : non_array_type rank_specifier+
    ;

non_array_type
    : type
    ;

rank_specifier
    : '[' dim_separator* ']'
    ;

dim_separator
    : ','
    ;
```

A *non_array_type* libovolnou *typ* , který je sám *array_type*.

Řád objektu typu pole je dán první *rank_specifier* v *array_type*: A *rank_specifier* znamená, že pole je pole s rozměrem jednu plus počet "`,`" tokeny v *rank_specifier*.

Typ prvku typu pole je typ, který je výsledkem odstranění první *rank_specifier*:

*  Typ pole formuláře `T[R]` je pole s pořadím `R` a typ bez pole elementu `T`.
*  Typ pole formuláře `T[R][R1]...[Rn]` je pole s pořadím `R` a typ elementu `T[R1]...[Rn]`.

V důsledku toho *rank_specifier*s se čtou zleva doprava před posledním prvkem mimo pole typu. Typ `int[][,,][,]` je jednorozměrné pole trojrozměrného pole dvojrozměrné pole `int`.

V době běhu, může být hodnota typu pole `null` nebo odkaz na instanci daného typu pole.

### <a name="the-systemarray-type"></a>Typ System.Array

Typ `System.Array` je abstraktní základní typ pro všechny typy polí. Implicitní referenční převod ([odkaz na implicitní převody](conversions.md#implicit-reference-conversions)) existuje z libovolného typu pole k `System.Array`a převodu explicitní odkaz ([odkaz na explicitní převody](conversions.md#explicit-reference-conversions)) existuje z `System.Array` na libovolný typ pole. Všimněte si, že `System.Array` není samotné *array_type*. Místo toho je *class_type* z kterého jsou všechny *array_type*s jsou odvozeny.

V době běhu, hodnota typu `System.Array` může být `null` nebo odkaz na instanci typu pole.

### <a name="arrays-and-the-generic-ilist-interface"></a>Pole a její obecná rozhraní IList

Jednorozměrné pole `T[]` implementuje rozhraní `System.Collections.Generic.IList<T>` (`IList<T>` zkráceně) a jeho základní rozhraní. Proto je implicitní převod z `T[]` k `IList<T>` a jeho základní rozhraní. Kromě toho, pokud je implicitní referenční převod z `S` k `T` pak `S[]` implementuje `IList<T>` a implicitní referenční převod z `S[]` k `IList<T>` a její základní rozhraní () [Odkaz na implicitní převody](conversions.md#implicit-reference-conversions)). Při konverzi explicitní odkaz z `S` k `T` je převod explicitní odkaz `S[]` k `IList<T>` a jeho základní rozhraní ([odkaz na explicitní převody](conversions.md#explicit-reference-conversions)). Příklad:
```csharp
using System.Collections.Generic;

class Test
{
    static void Main() {
        string[] sa = new string[5];
        object[] oa1 = new object[5];
        object[] oa2 = sa;

        IList<string> lst1 = sa;                    // Ok
        IList<string> lst2 = oa1;                   // Error, cast needed
        IList<object> lst3 = sa;                    // Ok
        IList<object> lst4 = oa1;                   // Ok

        IList<string> lst5 = (IList<string>)oa1;    // Exception
        IList<string> lst6 = (IList<string>)oa2;    // Ok
    }
}
```

Přiřazení `lst2 = oa1` vygeneruje chybu kompilace od převod z `object[]` k `IList<string>` je explicitní převod, ne implicitní. Přetypování `(IList<string>)oa1` způsobí výjimku, která je vyvolána v době běhu od `oa1` odkazy `object[]` a ne `string[]`. Ale přetypování `(IList<string>)oa2` nezpůsobí výjimku, která je vyvolána od `oa2` odkazy `string[]`.

Pokaždé, když se explicitní nebo implicitní referenční převod z `S[]` k `IList<T>`, je také odkaz na explicitní převod z `IList<T>` a její základní rozhraní pro `S[]` ([explicitní odkaz převody](conversions.md#explicit-reference-conversions)).

Pokud typ pole `S[]` implementuje `IList<T>`, některé členy implementovaného rozhraní může vyvolat výjimky. Přesné chování implementaci rozhraní je nad rámec téhle specifikaci.

## <a name="array-creation"></a>Vytvoření pole

Pole instancí jsou vytvářeny *array_creation_expression*s ([pole vytváření výrazů](expressions.md#array-creation-expressions)) nebo pole nebo místní deklarace proměnných, které patří *array_initializer*([Inicializátory pole](arrays.md#array-initializers)).

Když je vytvořena instance pole, počet rozměrů a délka každé dimenze jsou vytvořeny a pak zůstal neměnný po celou dobu života instance. Jinými slovy není možné změnit pořadí existujícího pole instance, ani je možné ke změně velikosti jeho rozměrů.

Pole instance je vždy typu pole. `System.Array` Typ je abstraktní typ, který se nedá vytvořit instance.

Prvky pole vytvořené *array_creation_expression*s jsou vždy inicializovat na výchozí hodnoty ([výchozí hodnoty](variables.md#default-values)).

## <a name="array-element-access"></a>Přístup k prvkům pole

Prvky pole jsou přístupné pomocí *element_access* výrazy ([pole přístup](expressions.md#array-access)) ve formátu `A[I1, I2, ..., In]`, kde `A` je výraz typu pole a každý `Ix` je Výraz typu `int`, `uint`, `long`, `ulong`, nebo lze implicitně převést na jeden nebo více z těchto typů. Výsledek přístupového prvku pole je proměnná, konkrétně prvku pole zvolila indexy.

Prvky pole jsou uvedené použití `foreach` – příkaz ([příkazu foreach](statements.md#the-foreach-statement)).

## <a name="array-members"></a>Členy pole

Každé pole Typ dědí členy deklarované pomocí `System.Array` typu.

## <a name="array-covariance"></a>Kovariance polí

Pro jakékoliv dva *reference_type*s `A` a `B`, pokud implicitní referenční převod ([odkaz na implicitní převody](conversions.md#implicit-reference-conversions)) nebo explicitní odkaz na převod ([ Odkaz na explicitní převody](conversions.md#explicit-reference-conversions)) existuje z `A` k `B`, pak také existuje stejný referenční převod z typu pole `A[R]` na typ pole `B[R]`, kde `R` je libovolný daný *rank_specifier* (ale stejný pro pole typů). Tento vztah se označuje jako ***kovariance polí***. Kovariance polí zejména znamená, že hodnota typu pole `A[R]` ve skutečnosti může být odkazem na instanci typu pole `B[R]`, pokud existuje implicitní referenční převod z `B` k `A`.

Z důvodu kovariance polí přiřazení k prvkům pole typu odkazu zahrnovat kontrolu za běhu, který zajišťuje, že hodnota přiřazením k prvku pole je ve skutečnosti povolených typu ([jednoduché přiřazení](expressions.md#simple-assignment)). Příklad:
```csharp
class Test
{
    static void Fill(object[] array, int index, int count, object value) {
        for (int i = index; i < index + count; i++) array[i] = value;
    }

    static void Main() {
        string[] strings = new string[100];
        Fill(strings, 0, 100, "Undefined");
        Fill(strings, 0, 10, null);
        Fill(strings, 90, 10, 0);
    }
}
```

Přiřazení `array[i]` v `Fill` metoda implicitně zahrnuje kontrolu za běhu, který zajistí, že objekt odkazovaný zadaným parametrem `value` je buď `null` nebo instanci, která je kompatibilní s typem skutečným prvkem `array`. V `Main`, první dvě volání `Fill` úspěšné, ale třetí způsobí vyvolání `System.ArrayTypeMismatchException` při spuštění první přiřazení k vyvolání `array[i]`. K výjimce dochází, protože zabalený `int` nelze ukládat v `string` pole.

Kovariance polí konkrétně nerozšiřuje polí *value_type*s. Například neexistuje žádný převod tohoto povolení `int[]` zacházeno jako `object[]`.

## <a name="array-initializers"></a>Inicializátory pole

Inicializátory pole je možné zadat deklarace pole ([pole](classes.md#fields)), místní deklarace proměnné ([místní deklarace proměnné](statements.md#local-variable-declarations)) a pole vytváření výrazů ([vytvoření pole výrazy](expressions.md#array-creation-expressions)):

```antlr
array_initializer
    : '{' variable_initializer_list? '}'
    | '{' variable_initializer_list ',' '}'
    ;

variable_initializer_list
    : variable_initializer (',' variable_initializer)*
    ;

variable_initializer
    : expression
    | array_initializer
    ;
```

Inicializátor pole se skládá z posloupnost proměnné inicializátory, ohraničená "`{`"a"`}`"tokeny a oddělené"`,`" tokeny. Každý inicializátor proměnné je výraz, nebo v případě vícerozměrné pole inicializátor vnořeného pole.

Kontext, ve kterém se používá inicializátor pole určuje typ pole, který je inicializován. V výraz vytvoření pole Typ pole bezprostředně předchází inicializátor nebo je odvozen z výrazů v inicializátoru pole. Typ pole je v poli nebo deklarace proměnné typu pole nebo deklarované proměnné. Při inicializátor pole se používá v poli nebo deklarace proměnné, jako například:
```csharp
int[] a = {0, 2, 4, 6, 8};
```
je jednoduše zkratka pro výraz vytvoření pole ekvivalentní:
```csharp
int[] a = new int[] {0, 2, 4, 6, 8};
```

Pro jednorozměrné pole musí obsahovat inicializátor pole sekvenci výrazů, které jsou kompatibilní s typem elementu pole přiřazení. Výrazy inicializaci prvků pole ve vzestupném pořadí, počínaje element na pozici nula. Počet výrazů v inicializátoru pole určuje vytváří instance pole. Například výše inicializátor pole vytvoří `int[]` instance o délce 5 a potom inicializuje instanci s použitím následujících hodnot:
```csharp
a[0] = 0; a[1] = 2; a[2] = 4; a[3] = 6; a[4] = 8;
```

Vícerozměrné pole musí mít inicializátor pole libovolný počet úrovní vnoření jako dimenze v poli. Nejkrajnější úroveň vnoření odpovídá úplně vlevo dimenze a vnitřní úroveň vnoření odpovídá rozměr nejvíce vpravo. Délka každé dimenze pole je určeno počtem prvků na odpovídající úroveň vnoření v inicializátoru pole. Pro každý inicializátor vnořeného pole. počet prvků, které musí být stejná jako jiné Inicializátory polí na stejné úrovni. Příklad:
```csharp
int[,] b = {{0, 1}, {2, 3}, {4, 5}, {6, 7}, {8, 9}};
```
Vytvoří dvourozměrné pole o délce 5 pro první dimenzi a o délce 2 pro rozměr nejvíce vpravo:
```csharp
int[,] b = new int[5, 2];
```
a pak inicializuje pole instance s použitím následujících hodnot:
```csharp
b[0, 0] = 0; b[0, 1] = 1;
b[1, 0] = 2; b[1, 1] = 3;
b[2, 0] = 4; b[2, 1] = 5;
b[3, 0] = 6; b[3, 1] = 7;
b[4, 0] = 8; b[4, 1] = 9;
```

Dimenzi než úplně vpravo je s nulovou délkou směru, je-li předpokládá se, že následující dimenze jsou také mít nulovou délku. Příklad:
```csharp
int[,] c = {};
```
Vytvoří dvourozměrné pole o délce 0 pro první a rozměr nejvíce vpravo:
```csharp
int[,] c = new int[0, 0];
```

Pokud výraz vytvoření pole zahrnuje explicitní dimenze délky a inicializátor pole, délky musí být konstantní výrazy a počtem prvků na každou úroveň vnoření musí shodovat s délkou odpovídající dimenze. Následuje několik příkladů:
```csharp
int i = 3;
int[] x = new int[3] {0, 1, 2};        // OK
int[] y = new int[i] {0, 1, 2};        // Error, i not a constant
int[] z = new int[3] {0, 1, 2, 3};     // Error, length/initializer mismatch
```

Tady, inicializátor pro `y` výsledkem chyba kompilace, protože výraz délky dimenzí není konstantní a inicializátor pro `z` výsledkem chyba kompilace, protože délka a počet prvků v Inicializátor vzájemně nesouhlasí.
