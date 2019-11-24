---
ms.openlocfilehash: 3b142d7dbda8a94a4cf2c973a2e380065dcbf5ee
ms.sourcegitcommit: 892af9016b3317a8fae12d195014dc38ba51cf16
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/01/2019
ms.locfileid: "71703969"
---
# <a name="enums"></a>Výčty

***Typ výčtu*** je jedinečný typ hodnoty ([typy hodnot](types.md#value-types)), který deklaruje sadu pojmenovaných konstant.

Příklad

```csharp
enum Color
{
    Red,
    Green,
    Blue
}
```

Deklaruje typ výčtu s názvem `Color` s členy `Red`, `Green`a `Blue`.

## <a name="enum-declarations"></a>Deklarace výčtu

Deklarace výčtu deklaruje nový typ výčtu. Deklarace výčtu začíná klíčovým slovem `enum`a definuje název, přístupnost, nadřízený typ a členy výčtu.

```antlr
enum_declaration
    : attributes? enum_modifier* 'enum' identifier enum_base? enum_body ';'?
    ;

enum_base
    : ':' integral_type
    ;

enum_body
    : '{' enum_member_declarations? '}'
    | '{' enum_member_declarations ',' '}'
    ;
```

Každý typ výčtu má odpovídající celočíselný typ nazvaný ***základní typ*** výčtového typu. Tento nadřízený typ musí být schopný reprezentovat všechny hodnoty výčtu definované ve výčtu. Deklarace výčtu může explicitně deklarovat základní typ `byte`, `sbyte`, `short`, `ushort`, `int`, `uint`, `long` nebo `ulong`. Všimněte si, že `char` nelze použít jako podkladový typ. Deklarace výčtu, která explicitně nedeklaruje nadřízený typ, má podkladový typ `int`.

Příklad

```csharp
enum Color: long
{
    Red,
    Green,
    Blue
}
```

deklaruje výčet s podkladovým typem `long`. Vývojář se může rozhodnout použít základní typ `long`, jako v příkladu, pro povolení použití hodnot, které jsou v rozsahu `long`, ale ne v rozsahu `int`, nebo pro zachování této možnosti pro budoucnost.

## <a name="enum-modifiers"></a>Modifikátory výčtu

*Enum_declaration* může volitelně zahrnovat posloupnost modifikátorů výčtu:

```antlr
enum_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    ;
```

Jedná se o chybu při kompilaci, aby se stejný modifikátor zobrazoval víckrát v deklaraci výčtu.

Modifikátory výčtového typu mají stejný význam jako deklarace třídy ([modifikátory třídy](classes.md#class-modifiers)). Upozorňujeme však, že v deklaraci výčtu nejsou povoleny modifikátory `abstract` a `sealed`. Výčty nemohou být abstraktní a nepovolují odvození.

## <a name="enum-members"></a>Členy výčtu

Tělo deklarace výčtového typu definuje nula nebo více členů výčtu, které jsou pojmenované konstanty typu výčtu. Žádný ze dvou členů výčtu nesmí mít stejný název.

```antlr
enum_member_declarations
    : enum_member_declaration (',' enum_member_declaration)*
    ;

enum_member_declaration
    : attributes? identifier ('=' constant_expression)?
    ;
```

Každý člen výčtu má přidruženou konstantní hodnotu. Typ této hodnoty je základní typ pro obsažený výčet. Hodnota konstanty pro každý člen výčtu musí být v rozsahu nadřazeného typu pro výčet. Příklad

```csharp
enum Color: uint
{
    Red = -1,
    Green = -2,
    Blue = -3
}
```

má za následek chybu při kompilaci, protože hodnoty konstant `-1`, `-2`a `-3` nejsou v rozsahu základního integrálního typu `uint`.

Víc členů výčtu může sdílet stejnou přidruženou hodnotu. Příklad

```csharp
enum Color 
{
    Red,
    Green,
    Blue,

    Max = Blue
}
```

zobrazuje výčet, ve kterém dva členy výčtu--`Blue` a `Max`--mají stejnou přidruženou hodnotu.

Přidružená hodnota člena výčtu je přiřazena implicitně nebo explicitně. Pokud deklarace člena výčtu má inicializátor *constant_expression* , hodnota tohoto konstantního výrazu implicitně převedená na nadřízený typ výčtu je přidružená hodnota člena výčtu. Pokud deklarace člena výčtu nemá žádný inicializátor, je jeho přidružená hodnota nastavena implicitně následujícím způsobem:

*  Pokud je člen výčtu prvním členem výčtu deklarovaným v typu výčtu, jeho přidružená hodnota je nula.
*  V opačném případě se přidružená hodnota člena výčtu získá zvýšením přidružené hodnoty textu před výčtem, který je členem. Tato zvýšená hodnota musí být v rozsahu hodnot, které mohou být reprezentovány podkladovým typem, v opačném případě dojde k chybě při kompilaci.

Příklad

```csharp
using System;

enum Color
{
    Red,
    Green = 10,
    Blue
}

class Test
{
    static void Main() {
        Console.WriteLine(StringFromColor(Color.Red));
        Console.WriteLine(StringFromColor(Color.Green));
        Console.WriteLine(StringFromColor(Color.Blue));
    }

    static string StringFromColor(Color c) {
        switch (c) {
            case Color.Red: 
                return String.Format("Red = {0}", (int) c);

            case Color.Green:
                return String.Format("Green = {0}", (int) c);

            case Color.Blue:
                return String.Format("Blue = {0}", (int) c);

            default:
                return "Invalid color";
        }
    }
}
```

vytiskne názvy členů výčtu a jejich přidružené hodnoty. Výstup je:

```console
Red = 0
Green = 10
Blue = 11
```

z následujících důvodů:

*  `Red` členů výčtu je automaticky přiřazena hodnota nula (protože nemá žádný inicializátor a je prvním členem výčtu);
*  člen výčtu `Green` je explicitně přidělena hodnota `10`;
*  a člen výčtu `Blue` je automaticky přiřazena hodnota, která je vyšší než člen, který předá text před ním.

Přidružená hodnota člena výčtu nesmí přímo ani nepřímo používat hodnotu vlastního přidruženého člena výčtu. Kromě tohoto omezení cyklace mohou Inicializátory členů výčtu volně odkazovat na jiné Inicializátory členů výčtu, bez ohledu na jejich textovou pozici. V rámci inicializátoru členů výčtu jsou hodnoty jiných členů výčtu vždy zpracovány jako typ jejich nadřazeného typu, takže přetypování není nutné při odkazování na jiné členy výčtu.

Příklad

```csharp
enum Circular
{
    A = B,
    B
}
```

má za následek chybu v době kompilace, protože deklarace `A` a `B` jsou cyklické. `A` závisí na `B` explicitně a `B` závisí na `A` implicitně.

Členy výčtu jsou pojmenovány a vymezeny způsobem, který je přesně podobný polím v rámci tříd. Rozsah člena výčtu je tělo jeho obsahujícího typu výčtu. V rámci tohoto oboru můžou být členy výčtu odkazováni podle jejich jednoduchého názvu. Ze všech ostatních kódů musí být název člena výčtu kvalifikován názvem jeho typu výčtu. Členové výčtu nemají žádné deklarované přístupnost – člen výčtu je přístupný, pokud je jeho nadřazený typ výčtu přístupný.

## <a name="the-systemenum-type"></a>Typ System. Enum

Typ `System.Enum` je abstraktní základní třída všech typů výčtu (to se liší a liší od základního typu typu výčtu) a členové dědění z `System.Enum` jsou k dispozici v jakémkoli typu výčtu. Převod zabalení ([převody zabalení](types.md#boxing-conversions)) existuje z libovolného typu výčtu na `System.Enum`a převod rozbalení ([převody rozbalení](types.md#unboxing-conversions)) existuje z `System.Enum` na libovolný typ výčtu.

Všimněte si, že `System.Enum` sám o sobě není *enum_type*. Místo toho se jedná o *class_type* , ze kterého jsou odvozeny všechny *enum_type*s. Typ `System.Enum` dědí z typu `System.ValueType` ([typ System. ValueType](types.md#the-systemvaluetype-type)), který naopak dědí z typu `object`. V době běhu může být hodnota typu `System.Enum` `null` nebo odkaz na zabalenou hodnotu libovolného typu výčtu.

## <a name="enum-values-and-operations"></a>Výčtové hodnoty a operace

Každý typ výčtu definuje odlišný typ; pro převod mezi výčtovým typem a integrálním typem nebo mezi dvěma typy výčtu se vyžaduje explicitní převod výčtu ([explicitní převody výčtu](conversions.md#explicit-enumeration-conversions)). Sada hodnot, na jejichž základě může typ výčtu pobírat, není omezena členy výčtu. Konkrétně jakákoli hodnota nadřazeného typu výčtu může být převedena na typ výčtu a je odlišná platná hodnota tohoto typu výčtu.

Členy výčtu mají typ svého obsahujícího výčtového typu (s výjimkou jiných inicializátorů členů výčtu: viz [výčet členů](enums.md#enum-members)). Hodnota člena výčtu deklarovaného v typu výčtu `E` s přidruženou hodnotou `v` je `(E)v`.

Následující operátory lze použít na hodnoty typů výčtu: `==`, `!=`, `<`, `>`, `<=`, `>=` ([operátory porovnání výčtu](expressions.md#enumeration-comparison-operators)), binární `+` ([operátor sčítání](expressions.md#addition-operator)), binární `-` ([operátor odčítání](expressions.md#subtraction-operator)), `^`, `&`, `|` ([logické operátory výčtu](expressions.md#enumeration-logical-operators)), `~` ([operátor bitového doplňku](expressions.md#bitwise-complement-operator)), `++` a `--` ( [Operátory přírůstku a snížení přípony](expressions.md#postfix-increment-and-decrement-operators) a [operátory přírůstku a snížení předpony](expressions.md#prefix-increment-and-decrement-operators).

Každý typ výčtu je automaticky odvozen od třídy `System.Enum` (což je naopak odvozeno z `System.ValueType` a `object`). Proto lze zděděné metody a vlastnosti této třídy použít pro hodnoty typu výčtu.
