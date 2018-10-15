# <a name="enums"></a>Výčty

***Typ výčtu*** je typ odlišné hodnoty ([typů hodnot](types.md#value-types)), který deklaruje sadou pojmenovaných konstant.

V příkladu

```csharp
enum Color
{
    Red,
    Green,
    Blue
}
```

deklaruje typ výčtu s názvem `Color` s členy `Red`, `Green`, a `Blue`.

## <a name="enum-declarations"></a>Deklarace výčtu

Deklarace výčtu deklaruje nový typ výčtu. Deklarace výčtu začíná klíčovým slovem `enum`a definuje název, dostupnost, nadřízený typ a členy výčtu.

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

Každý typ enum má odpovídající integrální typ s názvem ***nadřízený typ*** typu výčtu. Tento typ musí být schopno představovat všechny hodnoty výčtu definované ve výčtu. Základní typ explicitně deklarovat deklarace výčtu `byte`, `sbyte`, `short`, `ushort`, `int`, `uint`, `long` nebo `ulong`. Všimněte si, že `char` nelze použít jako základního typu. Deklarace výčtu, která nedeklaruje explicitně nadřízený typ má základní typ `int`.

V příkladu

```csharp
enum Color: long
{
    Red,
    Green,
    Blue
}
```

deklaruje výčet s základního typu `long`. Vývojáři zvolit použití základního typu `long`, jako v příkladu umožní použít hodnoty, které jsou v rozsahu `long` , ale není v rozsahu `int`, nebo zachovat tuto možnost v budoucnosti.

## <a name="enum-modifiers"></a>Modifikátory výčtu

*Enum_declaration* může volitelně zahrnovat posloupnost modifikátory výčtu:

```antlr
enum_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    ;
```

Je chyba kompilace pro stejný modifikátor se zobrazí více než jednou v deklaraci výčtu.

Modifikátory deklarace výčtu mají stejný význam jako deklarace třídy ([třídy modifikátory](classes.md#class-modifiers)). Upozorňujeme však, který `abstract` a `sealed` modifikátory nejsou povolené v deklaraci výčtu. Výčty nemohou být abstraktní a není povoleno odvození.

## <a name="enum-members"></a>Členy výčtu

Tělo deklaraci typu výčtu definuje nula nebo více členy výčtu, které jsou pojmenované konstanty typu výčtu. Žádné dva členy výčtu může mít stejný název.

```antlr
enum_member_declarations
    : enum_member_declaration (',' enum_member_declaration)*
    ;

enum_member_declaration
    : attributes? identifier ('=' constant_expression)?
    ;
```

Každý člen výčtu má přidruženou konstantní hodnotu. Typ této hodnoty je základní typ pro obsahující Výčet. Konstantní hodnoty pro každého člena výčtu musí být v rozsahu nadřízený typ výčtu. V příkladu

```csharp
enum Color: uint
{
    Red = -1,
    Green = -2,
    Blue = -3
}
```

výsledkem chyba kompilace, protože konstantní hodnoty `-1`, `-2`, a `-3` nejsou v rozsahu podkladových celočíselného typu `uint`.

Více členů výčtu může sdílet stejné přidruženou hodnotu. V příkladu

```csharp
enum Color 
{
    Red,
    Green,
    Blue,

    Max = Blue
}
```

Zobrazí výčet v dva členy výčtu – `Blue` a `Max` --mají stejné přidružené hodnoty.

Přidružená hodnota člen výčtu je přiřazena implicitně nebo explicitně. Pokud má deklarace člena výčtu *constant_expression* inicializátor, hodnota tento konstantní výraz, implicitně převeden na podkladový typ výčtového typu, je přidružená hodnota člena výčtu. Pokud deklarace výčtu člen nemá žádný inicializátor, je její přidružené hodnoty nastavit implicitně, následujícím způsobem:

*  Pokud je člen výčtu první člen výčtu deklarovaný ve výčtu typu, je jeho přidruženou hodnotu nula.
*  V opačném případě se přidružená hodnota člena výčtu získá zvýšením přidružená hodnota textový předchozí člen výčtu o jednu. Tato vyšší hodnota musí být v rozsahu hodnot, které může být reprezentován základní typ, jinak dojde k chybě kompilace.

V příkladu

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

Vytiskne názvy členů výčtu a jejich přidružené hodnoty. Výstup bude:

```
Red = 0
Green = 10
Blue = 11
```

z následujících důvodů:

*  člen výčtu `Red` se automaticky přiřadí hodnotu nula (protože nemá žádný inicializátor a je první člen výčtu);
*  člen výčtu `Green` explicitně přiřazena hodnota `10`;
*  a člen výčtu `Blue` je automaticky přiřazena hodnota větší než člena, který mu předchází pomocí textu.

Přidružená hodnota člen výčtu není, může přímo nebo nepřímo, použijte hodnotu vlastní člen přidružené výčtu. Než toto omezení Cykličnost mohou odkazovat na jiné inicializátory členů výčtu, bez ohledu na jejich umístění textové volně inicializátory členů výčtu. V rámci inicializátor člena výčtu jsou hodnoty ostatních členů výčtu vždy považovány za typu základní typ, tak, aby přetypování nejsou nutné k odkazování na ostatní členy výčtu.

V příkladu

```csharp
enum Circular
{
    A = B,
    B
}
```

výsledkem chyba kompilace, protože prohlášení o `A` a `B` jsou cyklické. `A` závisí na `B` explicitně, a `B` závisí na `A` implicitně.

Jsou členové výčtu s názvem a obor způsobem přesně obdobná na pole v rámci třídy. Rozsah člen výčtu je tělo jeho nadřazený typ výčtu. V daném oboru členy výčtu může být podle jejich jednoduchý název. Název na člena výčtu musí být kvalifikován s názvem typu výčtu, od jiného kódu. Členů výčtu nemají žádné deklarovaná přístupnost – člen výčtu je přístupný, pokud jeho nadřazený typ výčtu je přístupný.

## <a name="the-systemenum-type"></a>Typ System.Enum

Typ `System.Enum` je abstraktní základní třída všech typů výčtu (to se liší a liší se od základní typ typu výčtu) a členy zděděné z `System.Enum` jsou k dispozici v libovolný typ výčtu. Převod na uzavřené určení ([zabalení převody](types.md#boxing-conversions)) existuje z libovolného typu výčtu na `System.Enum`a unboxingového převodu ([rozbalení převody](types.md#unboxing-conversions)) existuje z `System.Enum` na libovolný typ výčtu.

Všimněte si, že `System.Enum` není samotné *enum_type*. Místo toho je *class_type* z kterého jsou všechny *enum_type*s jsou odvozeny. Typ `System.Enum` dědí z typu `System.ValueType` ([typ The System.ValueType](types.md#the-systemvaluetype-type)), který zase dědí z typu `object`. V době běhu, hodnota typu `System.Enum` může být `null` nebo odkaz zabalené hodnoty libovolného typu výčtu.

## <a name="enum-values-and-operations"></a>Hodnoty výčtu a operace

Každý typ výčtu definuje odlišný typ; konverzi explicitní výčet ([výčet explicitní převody](conversions.md#explicit-enumeration-conversions)) je nutné k převodu mezi výčtovým typem a celočíselného typu, nebo mezi dvěma typy výčtu. Sadu hodnot, které může přijmout typ výčtu není omezena jeho členy výčtu. Konkrétně se libovolná hodnota základního typu výčtu lze převést na typ výčtu a odlišné platná hodnota daného typu výčtu.

Členové výčtu mají typ jejich nadřazeným typem výčtu (s výjimkou v rámci jiné inicializátory členů výčtu: naleznete v tématu [členy výčtu](enums.md#enum-members)). Hodnota člen výčtu deklarovaný v typu výčtu `E` s přidruženou hodnotu `v` je `(E)v`.

U hodnot typu výčtu typů lze použít následující operátory: `==`, `!=`, `<`, `>`, `<=`, `>=` ([operátory porovnání výčet](expressions.md#enumeration-comparison-operators)), binární soubor `+` ([Operátor sčítání](expressions.md#addition-operator)), binární `-` ([operátor odčítání](expressions.md#subtraction-operator)), `^`, `&`, `|` ([výčet logické operátory](expressions.md#enumeration-logical-operators)), `~` ([operátor bitového doplňku](expressions.md#bitwise-complement-operator)), `++` a `--` ([Příponové operátory Inkrementace a dekrementace operátory](expressions.md#postfix-increment-and-decrement-operators) a [ Předpona Inkrementace a dekrementace operátory](expressions.md#prefix-increment-and-decrement-operators)).

Každý typ výčtu je automaticky odvozen ze třídy `System.Enum` (která, pak je odvozena z `System.ValueType` a `object`). Proto zděděné metody a vlastnosti této třídy lze použít u hodnot typu výčtu.
