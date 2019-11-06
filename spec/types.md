---
ms.openlocfilehash: 088c4a77cecde490c556c44c239a3496f896582e
ms.sourcegitcommit: 4ddf18d000734c1b6d0a48127bf338086fc3f2c3
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/05/2019
ms.locfileid: "73616142"
---
# <a name="types"></a>Typy

Typy C# jazyka jsou rozděleny do dvou hlavních kategorií: ***typy hodnot*** a ***odkazové typy***. Oba typy hodnot i typy odkazů můžou být ***Obecné typy***, které přebírají jeden nebo víc ***parametrů typu***. Parametry typu mohou určovat typy hodnot a odkazové typy.

```antlr
type
    : value_type
    | reference_type
    | type_parameter
    | type_unsafe
    ;
```

Konečná kategorie typů, ukazatelů, je k dispozici pouze v nebezpečném kódu. Tento popis je podrobněji popsán v tématu [typy ukazatelů](unsafe-code.md#pointer-types).

Typy hodnot se liší od typů odkazů v tom, že proměnné typů hodnot přímo obsahují svá data, zatímco proměnné referenčních typů ukládají ***odkazy*** na jejich data a ta se označují jako ***objekty***. U typů odkazů je možné, aby dvě proměnné odkazovaly na stejný objekt a bylo tak možné, aby operace na jedné proměnné ovlivnily objekt, na který je odkazováno z jiné proměnné. S typy hodnot mají proměnné, které mají svou vlastní kopii dat, a není možné, že operace na jednom mají vliv na ostatní.

C#systém typů je sjednocený tak, že hodnota libovolného typu může být zpracována jako objekt. Každý typ C# přímo nebo nepřímo je odvozen z typu `object` třídy a `object` je nejvyšší základní třídou všech typů. Hodnoty typů odkazů se považují za objekty pouhým zobrazením hodnot jako typu `object`. Hodnoty typů hodnot se považují za objekty prováděním zabalení a rozbalení operací (zabalení[a rozbalení](types.md#boxing-and-unboxing)).

## <a name="value-types"></a>Typy hodnot

Typ hodnoty je buď typ struktury, nebo typ výčtu. C#poskytuje sadu předdefinovaných typů struktur označovaných jako ***jednoduché typy***. Jednoduché typy jsou identifikovány prostřednictvím vyhrazených slov.

```antlr
value_type
    : struct_type
    | enum_type
    ;

struct_type
    : type_name
    | simple_type
    | nullable_type
    ;

simple_type
    : numeric_type
    | 'bool'
    ;

numeric_type
    : integral_type
    | floating_point_type
    | 'decimal'
    ;

integral_type
    : 'sbyte'
    | 'byte'
    | 'short'
    | 'ushort'
    | 'int'
    | 'uint'
    | 'long'
    | 'ulong'
    | 'char'
    ;

floating_point_type
    : 'float'
    | 'double'
    ;

nullable_type
    : non_nullable_value_type '?'
    ;

non_nullable_value_type
    : type
    ;

enum_type
    : type_name
    ;
```

Na rozdíl od proměnné typu odkazu může proměnná typu hodnoty obsahovat hodnotu `null` pouze v případě, že typ hodnoty je typ s možnou hodnotou null.  Pro každý typ hodnoty, která není null, existuje odpovídající typ hodnoty s možnou hodnotou null, který označuje stejnou sadu hodnot a hodnotu `null`.

Přiřazení proměnné typu hodnoty vytvoří kopii hodnoty, která je přiřazena. To se liší od přiřazení k proměnné typu odkazu, který kopíruje odkaz, ale nikoli objekt identifikovaný odkazem.

### <a name="the-systemvaluetype-type"></a>Typ System. ValueType

Všechny typy hodnot jsou implicitně děděny od třídy `System.ValueType`, která zase dědí z třídy `object`. Není možné, aby žádný typ byl odvozen od typu hodnoty a typy hodnot jsou tedy implicitně zapečetěné ([zapečetěné třídy](classes.md#sealed-classes)).

Všimněte si, že `System.ValueType` *value_type*sám o sobě. Místo toho je *class_type* , ze kterého jsou automaticky odvozeny všechny *value_typey*.

### <a name="default-constructors"></a>Výchozí konstruktory

Všechny typy hodnot implicitně deklaruje veřejný konstruktor instance bez parametrů s názvem ***výchozí konstruktor***. Výchozí konstruktor vrátí instanci s nulovou inicializací, která je známá jako ***Výchozí hodnota*** pro typ hodnoty:

*  V případě všech *Simple_Type*s je výchozí hodnotou hodnota vytvořená pomocí bitového vzoru všech nul:
    * V případě `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`a `ulong`, je výchozí hodnota `0`.
    * Pro `char`je výchozí hodnota `'\x0000'`.
    * Pro `float`je výchozí hodnota `0.0f`.
    * Pro `double`je výchozí hodnota `0.0d`.
    * Pro `decimal`je výchozí hodnota `0.0m`.
    * Pro `bool`je výchozí hodnota `false`.
*  Pro *enum_type* `E`je výchozí hodnota `0`převedená na typ `E`.
*  Pro *struct_type*je výchozí hodnotou hodnota vytvořená nastavením všech polí hodnotového typu na jejich výchozí hodnotu a všechna pole odkazového typu na `null`.
*  Pro *nullable_type* výchozí hodnota je instance, pro kterou je vlastnost `HasValue` false a vlastnost `Value` není definována. Výchozí hodnota je také známá jako ***hodnota null*** typu s možnou hodnotou null.

Podobně jako jakýkoli jiný konstruktor instance je výchozí konstruktor hodnotového typu vyvolán pomocí operátoru `new`. Z důvodu efektivity není tento požadavek určen jako ve skutečnosti, že implementace konstruktoru vygenerovala volání konstruktoru. V následujícím příkladu jsou proměnné `i` a `j` inicializovány na hodnotu nula.

```csharp
class A
{
    void F() {
        int i = 0;
        int j = new int();
    }
}
```

Vzhledem k tomu, že každý typ hodnoty má implicitně veřejný konstruktor instance bez parametrů, není možné, aby typ struktury obsahoval explicitní deklaraci konstruktoru bez parametrů. Typ struktury je však povoleno deklarovat parametrizované konstruktory instancí ([konstruktory](structs.md#constructors)).

### <a name="struct-types"></a>Typy struktury

Typ struktury je typ hodnoty, který může deklarovat konstanty, pole, metody, vlastnosti, indexery, operátory, konstruktory instancí, statické konstruktory a vnořené typy. Deklarace typů struktury je popsána v tématu [deklarace struktury](structs.md#struct-declarations).

### <a name="simple-types"></a>Jednoduché typy

C#poskytuje sadu předdefinovaných typů struktur označovaných jako ***jednoduché typy***. Jednoduché typy jsou identifikovány prostřednictvím rezervovaných slov, ale tato vyhrazená slova jsou jednoduše aliasy pro předdefinované typy struktury v oboru názvů `System`, jak je popsáno v následující tabulce.


| __Rezervované slovo__ | __Typ s aliasem__ |
|-------------------|------------------|
| `sbyte`           | `System.SByte`   | 
| `byte`            | `System.Byte`    | 
| `short`           | `System.Int16`   | 
| `ushort`          | `System.UInt16`  | 
| `int`             | `System.Int32`   | 
| `uint`            | `System.UInt32`  | 
| `long`            | `System.Int64`   | 
| `ulong`           | `System.UInt64`  | 
| `char`            | `System.Char`    | 
| `float`           | `System.Single`  | 
| `double`          | `System.Double`  | 
| `bool`            | `System.Boolean` | 
| `decimal`         | `System.Decimal` | 

Vzhledem k tomu, že jednoduchý typ aliasuje typ struktury, každý jednoduchý typ má členy. Například `int` má členy deklarované v `System.Int32` a členy zděděné z `System.Object`a jsou povoleny následující příkazy:

```csharp
int i = int.MaxValue;           // System.Int32.MaxValue constant
string s = i.ToString();        // System.Int32.ToString() instance method
string t = 123.ToString();      // System.Int32.ToString() instance method
```

Jednoduché typy se liší od jiných typů struktury v tom, že umožňují určité další operace:

*  Většina jednoduchých typů povoluje vytváření hodnot zápisem *literálů* ([literály](lexical-structure.md#literals)). Například `123` je literál typu `int` a `'a'` je literál typu `char`. C#neposkytuje žádné zřizování pro literály typů struktury obecně a jiné než výchozí hodnoty jiných typů struktury jsou nakonec vždy vytvářeny prostřednictvím konstruktorů instancí těchto typů struktury.
*  Pokud jsou operandy výrazu všechny jednoduché konstanty typu, je možné, že kompilátor vyhodnotí výraz v době kompilace. Takový výraz je označován jako *constant_expression* ([konstantní výrazy](expressions.md#constant-expressions)). Výrazy zahrnující operátory definované jinými typy struktury nejsou považovány za konstantní výrazy.
*  Prostřednictvím deklarace `const` je možné deklarovat konstanty jednoduchých typů ([konstant](classes.md#constants)). Není možné mít konstanty jiných typů struktury, ale podobný efekt poskytuje `static readonly` pole.
*  Převody zahrnující jednoduché typy se mohou účastnit vyhodnocení operátorů převodu definovaných jinými typy struktury, ale uživatelsky definovaný operátor převodu se nikdy nemůže zúčastnit vyhodnocení jiného uživatelsky definovaného operátoru ([vyhodnocení uživatelem definované převody](conversions.md#evaluation-of-user-defined-conversions)).

### <a name="integral-types"></a>Celočíselné typy

C#podporuje devět integrálních typů: `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`a `char`. Integrální typy mají následující velikosti a rozsahy hodnot:

*  Typ `sbyte` představuje 8bitové celé číslo se znaménkem hodnoty mezi-128 a 127.
*  Typ `byte` představuje 8bitové celé číslo bez znaménka s hodnotami mezi 0 a 255.
*  Typ `short` představuje 16bitová celá čísla se znaménkem hodnoty mezi-32768 a 32767.
*  Typ `ushort` představuje nepodepsaná 16bitová celá čísla s hodnotami mezi 0 a 65535.
*  Typ `int` představuje podepsaná 32 celých čísel s hodnotami mezi-2147483648 a 2147483647.
*  Typ `uint` představuje nepodepsaná 32 celých čísel s hodnotami mezi 0 a 4294967295.
*  Typ `long` představuje podepsaná 64 celých čísel s hodnotami mezi-9223372036854775808 a 9223372036854775807.
*  Typ `ulong` představuje nepodepsaná 64 celých čísel s hodnotami mezi 0 a 18446744073709551615.
*  Typ `char` představuje nepodepsaná 16bitová celá čísla s hodnotami mezi 0 a 65535. Sada možných hodnot pro typ `char` odpovídá sadě znaků Unicode. I když `char` má stejnou reprezentaci jako `ushort`, nejsou povoleny všechny operace povolené u jednoho typu.

Unární a binární operátory integrálního typu vždy pracují s podepsanou 32-bitovou přesností, nepodepsaná 32-bit Precision, podepsaná 64-bit Precision nebo bez 64 znaménka na.

*  U unárních operátorů `+` a `~` je operand převeden na typ `T`, kde `T` je první z `int`, `uint`, `long`a `ulong`, které mohou plně představovat všechny možné hodnoty operandu. Operace se pak provede pomocí přesnosti typu `T`a typ výsledku je `T`.
*  Pro unární operátor `-` je operand převeden na typ `T`, kde `T` je první `int` a `long`, který může plně reprezentovat všechny možné hodnoty operandu. Operace se pak provede pomocí přesnosti typu `T`a typ výsledku je `T`. Unární operátor `-` nelze použít na operandy typu `ulong`.
*  Pro binární `+`, `-`, `*`, `/`, `%`, `&`, `^`, `|`, `==`, `!=`, `>`, `<`, `>=`a `<=` operátory , jsou operandy převedeny na typ `T`, kde `T` je první z `int`, `uint`, `long`a `ulong`, které mohou plně představovat všechny možné hodnoty obou operandů. Operace se pak provede pomocí přesnosti typu `T`a typ výsledku je `T` (nebo `bool` pro relační operátory). Není povoleno, aby jeden operand byl typu `long` a druhý pro typ `ulong` s binárními operátory.
*  Pro binární `<<` a `>>` operátory je levý operand převeden na typ `T`, kde `T` je první z `int`, `uint`, `long`a `ulong`, které mohou plně představovat všechny možné hodnoty operandu. Operace se pak provede pomocí přesnosti typu `T`a typ výsledku je `T`.

Typ `char` je klasifikován jako integrální typ, ale liší se od ostatních integrálních typů dvěma způsoby:

*  Neexistují žádné implicitní převody z jiných typů na typ `char`. Zejména, i když `sbyte`, `byte`a `ushort` typy mají rozsah hodnot, které jsou plně reprezentovatelné pomocí `char`ho typu, implicitní převody z `sbyte`, `byte`nebo `ushort` na `char` neexistuje.
*  Konstanty `char`ho typu musí být zapsány jako *character_literal*s nebo jako *integer_literal*s v kombinaci s přetypováním na typ `char`. Například `(char)10` je stejný jako `'\x000A'`.

Operátory a příkazy `checked` a `unchecked` slouží k řízení kontroly přetečení pro aritmetické operace typu integrálního typu a pro převody ([kontrolované a nezaškrtnuté operátory](expressions.md#the-checked-and-unchecked-operators)). V kontextu `checked` přetečení generuje chybu při kompilaci nebo způsobí, že `System.OverflowException` vyvoláno. V kontextu `unchecked` jsou přetečení ignorovány a jakékoli bity s vysokým pořadím, které se nevejdou do cílového typu, budou zahozeny.

### <a name="floating-point-types"></a>Typy s plovoucí desetinnou čárkou

C#podporuje dva typy s plovoucí desetinnou čárkou: `float` a `double`. Typy `float` a `double` jsou reprezentovány pomocí 32 formátů IEEE 754 s jednou přesností a 64 s jednou přesností, které poskytují následující sady hodnot:

*  Kladná nula a záporná nula. Ve většině případů se kladné nula a záporná nula chovají stejně jako jednoduchá hodnota nula, ale některé operace rozlišují mezi dvěma ([operátor dělení](expressions.md#division-operator)).
*  Kladné nekonečno a záporné nekonečno. Nekonečny jsou vytvářeny takovými operacemi, které vydělí nenulové číslo nulou. Například `1.0 / 0.0` poskytne kladné nekonečno a `-1.0 / 0.0` výsledkem záporné nekonečno.
*  Hodnota ***není číslo*** , často zkrácená NaN. Hodnoty NaN jsou vytvářeny pomocí neplatných operací s plovoucí desetinnou čárkou, jako je například dělení nulou nulou.
*  Konečná sada nenulových hodnot formuláře `s * m * 2^e`, kde `s` je 1 nebo-1 a `m` a `e` určují konkrétní typ s plovoucí desetinnou čárkou: pro `float``0 < m < 2^24` a `-149 <= e <= 104`a pro `double``0 < m < 2^53` a `-1075 <= e <= 970`. Denormalizovaná čísla s plovoucí desetinnou čárkou se považují za platné nenulové hodnoty.

Typ `float` může reprezentovat hodnoty od přibližně `1.5 * 10^-45` po `3.4 * 10^38` s přesností 7 číslic.

Typ `double` může představovat hodnoty od přibližně `5.0 * 10^-324` až po `1.7 × 10^308` s přesností 15-16 číslic.

Pokud je jeden z operandů binárního operátoru typu s plovoucí desetinnou čárkou, pak druhý operand musí být integrálního typu nebo typu s plovoucí desetinnou čárkou a operace je vyhodnocena takto:

*  Pokud je jeden z operandů integrálního typu, pak je tento operand převeden na typ s plovoucí desetinnou čárkou v jiném operandu.
*  Poté, pokud je jeden z operandů typu `double`, je druhý operand převeden na `double`, operace se provádí s minimálním `double`m rozsahem a přesností a typ výsledku je `double` (nebo `bool` pro relační operátory).
*  V opačném případě se operace provádí pomocí alespoň `float` rozsahu a přesnosti a typ výsledku je `float` (nebo `bool` pro relační operátory).

Operátory s plovoucí desetinnou čárkou, včetně operátorů přiřazení, nikdy nevyvolávají výjimky. Místo toho se ve výjimečných situacích operace s plovoucí desetinnou čárkou vydávají nuly, nekonečno nebo NaN, jak je popsáno níže:

*  Pokud je výsledek operace s plovoucí desetinnou čárkou příliš malý pro cílový formát, výsledek operace se bude kladná nula nebo záporná nula.
*  Pokud je výsledek operace s plovoucí desetinnou čárkou příliš velký pro cílový formát, výsledkem operace bude kladné nekonečno nebo záporné nekonečno.
*  Pokud je operace s plovoucí desetinnou čárkou neplatná, výsledek operace se nastaví na hodnotu NaN.
*  Pokud je jeden nebo oba operandy operace s plovoucí desetinnou čárkou NaN, výsledkem operace bude NaN.

Operace s plovoucí desetinnou čárkou mohou být provedeny s větší přesností než typ výsledku operace. Například některé hardwarové architektury podporují typ s plovoucí desetinnou čárkou "rozšířený" nebo "long double" s větším rozsahem a přesností než `double` typ a implicitně provádějí všechny operace s plovoucí desetinnou čárkou, které používají tento typ vyšší přesnosti. K provádění operací s plovoucí desetinnou čárkou s menší přesností můžou být takové hardwarové architektury prováděné jenom za nadměrných nákladů a místo toho, aby implementace napadly výkon i C# přesnost, umožňovaly vyšší typ přesnosti. bude použito pro všechny operace s plovoucí desetinnou čárkou. Kromě dodávání přesnější výsledků má tato zřídka jakékoli měřitelné účinky. Nicméně ve výrazech formuláře `x * y / z`, kde násobení vytvoří výsledek, který je mimo `double` rozsah, ale další dělení vrátí dočasný výsledek zpět do `double` rozsahu, fakt, že výraz je vyhodnocen v Formát většího rozsahu může způsobit, že se vytvoří konečný výsledek namísto nekonečna.

### <a name="the-decimal-type"></a>Typ Decimal

Typ `decimal` je 128 datový typ, který je vhodný pro finanční a peněžní výpočty. Typ `decimal` může představovat hodnoty od `1.0 * 10^-28` po přibližně `7.9 * 10^28` s 28-29mi platnými číslicemi.

Konečná sada hodnot typu `decimal` má formu `(-1)^s * c * 10^-e`, kde `s` znaménka je 0 nebo 1, `c` koeficient je dán `0 <= *c* < 2^96`a `e` škálování je takové, `0 <= e <= 28`. Typ `decimal` nepodporuje podepsané nuly, nekonečny nebo NaN. `decimal` je reprezentován jako 96 celé číslo, které se škáluje mocninou deseti. Pro `decimal`s absolutní hodnotou menší než `1.0m`je hodnota přesně na 28 desetinné místo, ale ještě ne. Pro `decimal`s absolutní hodnotou větší nebo rovnou `1.0m`je hodnota přesně na 28 nebo 29 číslic. V rozporu s datovými typy `float` a `double` mohou být desítkové zlomkové hodnoty, například 0,1, zobrazeny přesně v `decimal` reprezentaci. V `float` a `double` reprezentace jsou taková čísla často nekonečné zlomky, takže tyto reprezentace jsou lépe náchylné k chybám zaokrouhlení.

Pokud je jeden z operandů binárního operátoru typu `decimal`, pak druhý operand musí být integrálního typu nebo typu `decimal`. Pokud je přítomen operand integrálního typu, je převedena na `decimal` před provedením operace.

Výsledkem operace s hodnotami typu `decimal` je to, což by vedlo k výpočtu přesného výsledku (zachovávání stupnice, jak je definováno pro každý operátor) a pak zaokrouhlování podle reprezentace. Výsledky se zaokrouhlují na nejbližší možnou hodnotu a v případě, že se výsledek rovnoměrně blíží dvěma hodnotám, na který má sudé číslo v nejméně významném umístění číslice (označuje se jako "zaokrouhlení bank"). Nulový výsledek vždy má znak 0 a měřítko 0.

Pokud Desítková aritmetická operace vytvoří hodnotu menší nebo rovnou `5 * 10^-29` v absolutní hodnotě, výsledek operace se rovná nule. Pokud `decimal` aritmetické operace vytvoří výsledek, který je příliš velký pro formát `decimal`, je vyvolána `System.OverflowException`.

Typ `decimal` má větší přesnost, ale menší rozsah než typy s plovoucí desetinnou čárkou. Proto převody z typů s plovoucí desetinnou čárkou na `decimal` mohou generovat výjimky přetečení a převody z `decimal` na typy s plovoucí desetinnou čárkou mohou způsobit ztrátu přesnosti. Z těchto důvodů neexistují žádné implicitní převody mezi typy s plovoucí desetinnou čárkou a `decimal`a bez explicitního přetypování, není možné ve stejném výrazu kombinovat operandy s plovoucí desetinnou čárkou a `decimal`.

### <a name="the-bool-type"></a>Typ bool

Typ `bool` představuje logické logické množství. Možné hodnoty typu `bool` jsou `true` a `false`.

Mezi `bool` a jinými typy neexistují žádné standardní převody. Konkrétně je typ `bool` jedinečný a oddělený od integrálních typů a hodnotu `bool` nelze použít místo celočíselné hodnoty a naopak.

V jazycích C a C++ , nulových integrálních hodnot nebo hodnot s plovoucí desetinnou čárkou, nebo ukazatel s hodnotou null lze převést na logickou hodnotu `false`a nenulovou hodnotu integrálu nebo hodnoty s plovoucí desetinnou čárkou, nebo ukazatel, který není null, lze převést na logickou hodnotu `true`. V C#je tyto převody provedeny explicitním porovnáním hodnoty integrální nebo plovoucí desetinné čárky na nulu nebo explicitním porovnáním odkazu na objekt `null`.

### <a name="enumeration-types"></a>Výčtové typy

Typ výčtu je odlišný typ s pojmenovanými konstantami. Každý typ výčtu má základní typ, který musí být `byte`, `sbyte`, `short`, `ushort`, `int`, `uint`, `long` nebo `ulong`. Sada hodnot typu výčtu je stejná jako sada hodnot základního typu. Hodnoty typu výčtu nejsou omezeny na hodnoty s názvem konstanty. Výčtové typy jsou definovány prostřednictvím deklarací výčtu ([deklarace výčtu](enums.md#enum-declarations)).

### <a name="nullable-types"></a>Typy s povolenou hodnotou Null

Typ s možnou hodnotou null může představovat všechny hodnoty svého ***nadřízeného typu*** a další hodnotu null. Typ s možnou hodnotou null je napsaný `T?`, kde `T` je základní typ. Tato syntaxe je zkrácený pro `System.Nullable<T>`a dva formuláře lze zaměnit.

***Typ hodnoty*** , která není null, je naopak libovolný typ hodnoty jiný než `System.Nullable<T>` a jeho zkrácený `T?` (pro všechny `T`) a jakýkoli parametr typu, který je omezený na typ hodnoty, která není null (to znamená, že libovolný parametr typu s `struct` omezení). Typ `System.Nullable<T>` určuje omezení typu hodnoty pro `T` ([omezení parametrů typu](classes.md#type-parameter-constraints)), což znamená, že nadřízený typ typu s možnou hodnotou null může být typ hodnoty, která není null. Podkladový typ typu s možnou hodnotou null nemůže být typ s možnou hodnotou null nebo odkazový typ. Například `int??` a `string?` jsou neplatné typy.

Instance typu s možnou hodnotou null `T?` má dvě veřejné vlastnosti jen pro čtení:

*  Vlastnost `HasValue` typu `bool`
*  Vlastnost `Value` typu `T`

Instance, pro kterou je `HasValue` true, se říká, že to není null. Instance, která není null, obsahuje známou hodnotu a `Value` tuto hodnotu vrací.

Instance, pro kterou je `HasValue` false, je označována jako null. Instance null má nedefinovanou hodnotu. Při pokusu o čtení `Value` instance null dojde k vyvolání `System.InvalidOperationException`. Proces přístupu k vlastnosti `Value` instance s možnou hodnotou null je označován jako ***rozbalení***.

Kromě výchozího konstruktoru každý typ s povolenou hodnotou null `T?` má veřejný konstruktor, který přijímá jeden argument typu `T`. Předaná hodnota `x` typu `T`, volání konstruktoru formuláře.

```csharp
new T?(x)
```
Vytvoří instanci, která není null `T?` pro kterou je `x`vlastnost `Value`. Proces vytvoření instance s možnou hodnotou null pro danou hodnotu je označován jako ***zalamování***.

Implicitní převody jsou k dispozici z `null`ho literálu pro `T?` ([převody literálů s hodnotou null](conversions.md#null-literal-conversions)) a z `T` na `T?` ([implicitní převody s možnou hodnotou null](conversions.md#implicit-nullable-conversions)).

## <a name="reference-types"></a>Odkazové typy

Odkazový typ je typ třídy, typ rozhraní, typ pole nebo typ delegáta.

```antlr
reference_type
    : class_type
    | interface_type
    | array_type
    | delegate_type
    ;

class_type
    : type_name
    | 'object'
    | 'dynamic'
    | 'string'
    ;

interface_type
    : type_name
    ;

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

delegate_type
    : type_name
    ;
```

Hodnota typu odkazu je odkaz na ***instanci*** typu, druhá se nazývá ***objekt***. Speciální hodnota `null` je kompatibilní se všemi typy odkazů a označuje absenci instance.

### <a name="class-types"></a>Typy tříd

Typ třídy definuje strukturu dat, která obsahuje datové členy (konstanty a pole), členy funkce (metody, vlastnosti, události, indexery, operátory, konstruktory instancí, destruktory a statické konstruktory) a vnořené typy. Typy tříd podporují dědičnost, mechanismus, který odvozené třídy mohou roztáhnout a specializovat základní třídy. Instance typů tříd jsou vytvořeny pomocí *object_creation_expression*s ([výrazy pro vytváření objektů](expressions.md#object-creation-expressions)).

Typy tříd jsou popsány v tématu [třídy](classes.md).

Některé předdefinované typy tříd mají v C# jazyce zvláštní význam, jak je popsáno v následující tabulce.


| __Typ třídy__     | __Popis__                                         |
|--------------------|---------------------------------------------------------|
| `System.Object`    | Nejvyšší základní třída všech ostatních typů. Podívejte [se na typ objektu](types.md#the-object-type). | 
| `System.String`    | Typ řetězce C# jazyka Podívejte [se na typ řetězce](types.md#the-string-type).         |
| `System.ValueType` | Základní třída všech typů hodnot. Podívejte [se na typ System. ValueType](types.md#the-systemvaluetype-type).          |
| `System.Enum`      | Základní třída všech typů výčtu. Viz [výčty](enums.md).              |
| `System.Array`     | Základní třída všech typů polí. Viz [pole](arrays.md).             |
| `System.Delegate`  | Základní třída všech typů delegátů. Viz [Delegáti](delegates.md).          |
| `System.Exception` | Základní třída všech typů výjimek. Viz [výjimky](exceptions.md).         |

### <a name="the-object-type"></a>Typ objektu

Typ třídy `object` je nejvyšší základní třídou všech ostatních typů. Každý typ C# přímo nebo nepřímo je odvozen z typu `object` třídy.

Klíčové slovo `object` je pouze alias pro předdefinovaný `System.Object`třídy.

### <a name="the-dynamic-type"></a>Dynamický typ

Typ `dynamic`, jako je `object`, může odkazovat na libovolný objekt. Pokud jsou operátory aplikovány na výrazy typu `dynamic`, jejich rozlišení je odloženo, dokud se program nespustí. Proto, pokud operátor nemůže být právně použit na odkazovaný objekt, není během kompilace uvedena žádná chyba. Namísto toho dojde k výjimce, pokud překlad operátoru selže v době běhu.

Jeho účelem je umožnění dynamické vazby, která je podrobněji popsána v tématu [dynamická vazba](expressions.md#dynamic-binding).

`dynamic` se považuje za identické s `object` s výjimkou následujících hledisek:

*  Operace s výrazy typu `dynamic` můžou být dynamicky vázané ([dynamická vazba](expressions.md#dynamic-binding)).
*  Odvození typu ([odvození typu](expressions.md#type-inference)) upřednostňuje `dynamic` přes `object`, pokud jsou oba kandidáti.

Z důvodu této ekvivalence jsou následující:

*  Existuje implicitní převod identity mezi `object` a `dynamic`a mezi konstruovanými typy, které jsou stejné při nahrazování `dynamic` s `object`
*  Implicitní a explicitní převody na a z `object` vztahují také na a z `dynamic`.
*  Signatury metod, které jsou stejné při nahrazování `dynamic` s `object`, se považují za stejnou signaturu.
*  Typ `dynamic` nelze odlišit od `object` v době běhu.
*  Výraz typu `dynamic` je označován jako ***dynamický výraz***.

### <a name="the-string-type"></a>Typ řetězce

Typ `string` je zapečetěný typ třídy, který dědí přímo z `object`. Instance `string` třídy reprezentují řetězce znaků Unicode.

Hodnoty typu `string` lze zapsat jako řetězcové literály ([řetězcové literály](lexical-structure.md#string-literals)).

Klíčové slovo `string` je pouze alias pro předdefinovaný `System.String`třídy.

### <a name="interface-types"></a>Typy rozhraní

Rozhraní definuje kontrakt. Třída nebo struktura, která implementuje rozhraní, musí vyhovovat jeho kontraktu. Rozhraní může dědit z více základních rozhraní a třída nebo struktura může implementovat více rozhraní.

Typy rozhraní jsou popsány v tématu [rozhraní](interfaces.md).

### <a name="array-types"></a>Typy polí

Pole je datová struktura, která obsahuje nula nebo více proměnných, které jsou k dispozici prostřednictvím vypočítaných indexů. Proměnné obsažené v poli, označované také jako prvky pole, jsou všechny stejného typu a tento typ se nazývá typ elementu pole.

Typy polí jsou popsány v [polích](arrays.md).

### <a name="delegate-types"></a>Typy delegátů

Delegát je datová struktura, která odkazuje na jednu nebo více metod. Pro metody instance také odkazuje na odpovídající instance objektů.

Nejbližší ekvivalent delegáta v jazyce C nebo C++ je ukazatel na funkci, ale zatímco ukazatel funkce může odkazovat pouze na statické funkce, delegát může odkazovat jak na statické, tak na metody instance. V druhém případě delegát ukládá nejen odkaz na vstupní bod metody, ale také odkaz na instanci objektu, na které se má metoda vyvolat.

Typy delegátů jsou popsány v tématu [Delegáti](delegates.md).

## <a name="boxing-and-unboxing"></a>Zabalení a rozbalení

Pojem zabalení a rozbalení je od centrálního C#typu systém. Poskytuje most mezi *value_type*a a *reference_type*s tím, že umožňuje převod jakékoli hodnoty *value_type* na typ a z typu `object`. Zabalení a rozbalení umožňuje jednotný pohled na typ systému, ve kterém je hodnota libovolného typu považována za objekt.

### <a name="boxing-conversions"></a>Převody zabalení

Převod zabalení umožňuje *value_type* implicitně převést na *reference_type*. Existují následující převody zabalení:

*  Z libovolného *value_type* na typ `object`.
*  Z libovolného *value_type* na typ `System.ValueType`.
*  Z libovolného *non_nullable_value_type* na libovolný *INTERFACE_TYPE* implementovaný v *value_type*.
*  Z libovolného *nullable_type* na libovolný *INTERFACE_TYPE* implementovaný podkladovým typem *nullable_type*.
*  Z libovolného *enum_type* na typ `System.Enum`.
*  Z libovolného *nullable_typeu* s podkladovým *enum_type* na typ `System.Enum`.
*  Všimněte si, že implicitní převod z parametru typu bude proveden jako převod zabalení, pokud v době běhu skončí převod z typu hodnoty na typ odkazu ([implicitní převody zahrnující parametry typu](conversions.md#implicit-conversions-involving-type-parameters)).

Zabalení hodnoty *non_nullable_value_type* se skládá z přidělení instance objektu a zkopírování hodnoty *non_nullable_value_type* do této instance.

Zabalením hodnoty *nullable_type* se vytvoří odkaz s hodnotou null, pokud se jedná o hodnotu `null` (`HasValue` je `false`), nebo výsledek rozbalení a zabalení podkladové hodnoty v opačném případě.

Skutečný proces zabalení hodnoty *non_nullable_value_type* je nejlépe vysvětleno tím, že se porozumí existence obecné ***třídy zabalení***, která se chová, jako by byla deklarována takto:

```csharp
sealed class Box<T>: System.ValueType
{
    T value;

    public Box(T t) {
        value = t;
    }
}
```

Zabalení hodnoty `v` typu `T` nyní se skládá z spuštění `new Box<T>(v)`výrazu a vrácení výsledné instance jako hodnoty typu `object`. Příkazy tedy
```csharp
int i = 123;
object box = i;
```
koncepčně souhlasí
```csharp
int i = 123;
object box = new Box<int>(i);
```

Třída zabalení, jako je výše uvedená `Box<T>`, ve skutečnosti neexistuje a dynamický typ zabalené hodnoty není ve skutečnosti typu třídy. Místo toho je zabalená hodnota typu `T`a `T`dynamického typu a při kontrole dynamického typu pomocí operátoru `is` lze jednoduše odkazovat typ `T`. Například
```csharp
int i = 123;
object box = i;
if (box is int) {
    Console.Write("Box contains an int");
}
```
provede výstup řetězce "`Box contains an int`" v konzole nástroje.

Převod zabalení předpokládá, že je kopie hodnoty zabalená. To se liší od konverze *reference_type* na typ `object`, ve kterém hodnota nadále odkazuje na stejnou instanci a jednoduše je považována za méně odvozený typ `object`. Například s ohledem na deklaraci
```csharp
struct Point
{
    public int x, y;

    public Point(int x, int y) {
        this.x = x;
        this.y = y;
    }
}
```
následující příkazy
```csharp
Point p = new Point(10, 10);
object box = p;
p.x = 20;
Console.Write(((Point)box).x);
```
vypíše hodnotu 10 v konzole, protože operace implicitního zabalení, ke které dojde v přiřazení `p` k `box` způsobí, že se zkopíruje hodnota `p`. Bylo `Point` deklarované `class` namísto toho, hodnota 20 by byla výstup, protože `p` a `box` by odkazovaly na stejnou instanci.

### <a name="unboxing-conversions"></a>Převody rozbalení

Převod rozbalení umožňuje *reference_type* explicitně převést na *value_type*. Existují následující převody rozbalení:

*  Z typu `object` na libovolný *value_type*.
*  Z typu `System.ValueType` na libovolný *value_type*.
*  Z libovolného *INTERFACE_TYPE* na libovolný *non_nullable_value_type* , který implementuje *INTERFACE_TYPE*.
*  Z libovolného *INTERFACE_TYPE* na všechny *nullable_type* , jejichž základní typ implementuje *INTERFACE_TYPE*.
*  Z typu `System.Enum` na libovolný *enum_type*.
*  Z typu `System.Enum` do libovolného *nullable_type* s podkladovým *enum_type*.
*  Všimněte si, že explicitní převod na parametr typu bude proveden jako převod rozbalení, pokud v době běhu skončí převod z typu odkazu na typ hodnoty ([explicitní dynamické převody](conversions.md#explicit-dynamic-conversions)).

Rozbalení operace *non_nullable_value_type* se skládá z první kontroly, zda je instance objektu zabalená hodnota daného *non_nullable_value_type*a pak se hodnota zkopíruje mimo instanci.

Rozbalení na *nullable_type* vytvoří hodnotu null *nullable_type* , pokud je zdrojový operand `null`, nebo zabalený výsledek rozbalení instance objektu do základního typu *nullable_type* v opačném případě.

Odkazy na imaginární třídu zabalení popsanou v předchozí části, převod rozbalení objektu `box` na *value_type* `T` se skládá z spuštění `((Box<T>)box).value`výrazu. Příkazy tedy
```csharp
object box = 123;
int i = (int)box;
```
koncepčně souhlasí
```csharp
object box = new Box<int>(123);
int i = ((Box<int>)box).value;
```

V případě převodu rozbalení na daný *non_nullable_value_type* v době běhu musí být hodnota zdrojového operandu odkazem na zabalenou hodnotu tohoto *non_nullable_value_type*. Pokud je zdrojový operand `null`, je vyvolána `System.NullReferenceException`. Pokud je zdrojový operand odkazem na nekompatibilní objekt, je vyvolána `System.InvalidCastException`.

V případě převodu rozbalení na daný *nullable_type* v době běhu musí být hodnota zdrojového operandu buď `null`, nebo odkaz na zabalenou hodnotu podkladového *non_nullable_value_typeu* *nullable_type*. Pokud je zdrojový operand odkazem na nekompatibilní objekt, je vyvolána `System.InvalidCastException`.

## <a name="constructed-types"></a>Vytvořené typy

Deklarace obecného typu sám o sobě označuje ***nevázaný obecný typ*** , který se používá jako "podrobný plán" pro vytvoření mnoha různých typů, a to tak, že použijete ***argumenty typu***. Argumenty typu jsou zapsány v lomených závorkách (`<` a `>`) hned za názvem obecného typu. Typ, který obsahuje alespoň jeden argument typu, se nazývá ***konstruovaný typ***. Konstruovaný typ lze použít na většině míst v jazyce, ve kterém se může zobrazit název typu. Nevázaný obecný typ se dá použít jenom v rámci *typeof_expression* ([operátor typeof](expressions.md#the-typeof-operator)).

Vytvořené typy lze také použít ve výrazech jako jednoduché názvy ([jednoduché názvy](expressions.md#simple-names)) nebo při přístupu ke členu ([přístupu ke členu](expressions.md#member-access)).

Při vyhodnocování *namespace_or_type_name* se považují pouze obecné typy se správným počtem parametrů typu. Proto je možné použít stejný identifikátor k identifikaci různých typů, pokud typy mají různé počty parametrů typu. To je užitečné při kombinování obecných a neobecných tříd ve stejném programu:

```csharp
namespace Widgets
{
    class Queue {...}
    class Queue<TElement> {...}
}

namespace MyApplication
{
    using Widgets;

    class X
    {
        Queue q1;            // Non-generic Widgets.Queue
        Queue<int> q2;       // Generic Widgets.Queue
    }
}
```

*TYPE_NAME* může identifikovat konstruovaný typ, i když neurčuje parametry typu přímo. K tomu může dojít, když je typ vnořený v deklaraci obecné třídy a typ instance obsahující deklarace se implicitně používá pro vyhledávání názvů ([vnořené typy v obecných třídách](classes.md#nested-types-in-generic-classes)):

```csharp
class Outer<T>
{
    public class Inner {...}

    public Inner i;                // Type of i is Outer<T>.Inner
}
```

V nebezpečném kódu nelze použít konstruovaný typ jako *unmanaged_type* ([typy ukazatelů](unsafe-code.md#pointer-types)).

### <a name="type-arguments"></a>Argumenty typu

Každý argument v seznamu argumentů typu je jednoduše *typ*.

```antlr
type_argument_list
    : '<' type_arguments '>'
    ;

type_arguments
    : type_argument (',' type_argument)*
    ;

type_argument
    : type
    ;
```

V nebezpečném kódu ([nezabezpečený kód](unsafe-code.md)) nesmí být *type_argument* typem ukazatele. Každý argument typu musí splňovat jakákoli omezení na odpovídajícím parametru typu ([omezení parametrů typu](classes.md#type-parameter-constraints)).

### <a name="open-and-closed-types"></a>Otevřené a uzavřené typy

Všechny typy mohou být klasifikovány buď jako ***otevřené typy*** nebo ***uzavřené typy***. Otevřený typ je typ, který zahrnuje parametry typu. Konkrétně:

*  Parametr typu definuje otevřený typ.
*  Typ pole je otevřený typ pouze v případě, že je jeho typ prvku otevřený typ.
*  Konstruovaný typ je otevřený typ pouze v případě, že jeden nebo více jeho argumentů typu je otevřený typ. Konstruovaný vnořený typ je otevřený typ, pokud a pouze v případě, že jeden nebo více jeho argumentů typu nebo argumenty typu jeho obsahujícího typu je otevřený typ.

Uzavřený typ je typ, který není otevřený typ.

V době běhu je veškerý kód v rámci deklarace obecného typu proveden v kontextu uzavřeného typu, který byl vytvořen pomocí argumentů typu pro obecnou deklaraci. Každý parametr typu v rámci obecného typu je vázán na určitý typ běhu. Běhové zpracování všech příkazů a výrazů vždy probíhá s uzavřenými typy a otevřené typy nastávají pouze během zpracování při kompilaci.

Každý uzavřený konstruovaný typ má svou vlastní sadu statických proměnných, které nejsou sdíleny s žádnými jinými uzavřenými konstruovanými typy. Vzhledem k tomu, že otevřený typ neexistuje v době běhu, nejsou k otevřenému typu přidruženy žádné statické proměnné. Dva uzavřené konstruované typy jsou stejného typu, pokud jsou vyrobeny ze stejného obecného typu bez vazby a jejich odpovídající argumenty typu jsou stejného typu.

### <a name="bound-and-unbound-types"></a>Vázané a nevázané typy

Termín ***nevázaný typ*** odkazuje na neobecný typ nebo na nevázaný obecný typ. Pojem ***vázaný typ*** odkazuje na neobecný typ nebo konstruovaný typ.

Nevázaný typ odkazuje na entitu deklarovanou deklarací typu. Nevázaný obecný typ není sám o sobě typu a nedá se použít jako typ proměnné, argumentu nebo návratové hodnoty nebo jako základní typ. Jedinou konstrukcí, ve které je možné odkazovat na nevázaný obecný typ, je výraz `typeof` ([operátor typeof](expressions.md#the-typeof-operator)).

### <a name="satisfying-constraints"></a>Splnění omezení

Pokaždé, když je odkazováno na konstruovaný typ nebo obecnou metodu, jsou zadávané argumenty typu zkontrolovány proti omezení parametru typu deklarovaném na obecném typu nebo metodě ([omezení parametrů typu](classes.md#type-parameter-constraints)). Pro každou klauzuli `where` se pro každé omezení kontroluje argument typu `A`, který odpovídá parametru pojmenovaného typu, a to následujícím způsobem:

*  Pokud je omezení typem třídy, typem rozhraní nebo parametrem typu, nechejte `C` představovat toto omezení s poskytnutými argumenty typu nahrazené pro všechny parametry typu, které se zobrazí v omezení. Aby bylo omezení splněno, musí se jednat o případ, že typ `A` je převoditelné na typ `C` jedním z následujících způsobů:
    * Převod identity ([převod identity](conversions.md#identity-conversion))
    * Implicitní převod odkazu ([implicitní převody odkazů](conversions.md#implicit-reference-conversions))
    * Převod zabalení ([převody zabalení](conversions.md#boxing-conversions)) za předpokladu, že typ A je typ hodnoty, která není null.
    * Implicitní odkaz, zabalení nebo převod parametru typu z parametru typu `A` do `C`.
*  Pokud je omezení omezení typu odkazu (`class`), typ `A` musí splňovat jednu z následujících možností:
    * `A` je typ rozhraní, typ třídy, typ delegáta nebo typ pole. Všimněte si, že `System.ValueType` a `System.Enum` jsou odkazové typy, které toto omezení vyhoví.
    * `A` je parametr typu, který je známý jako typ odkazu ([omezení parametrů typu](classes.md#type-parameter-constraints)).
*  Pokud je omezení typu hodnoty (`struct`), typ `A` musí splňovat jednu z následujících hodnot:
    * `A` je typ struktury nebo typ výčtu, ale ne typ s možnou hodnotou null. Všimněte si, že `System.ValueType` a `System.Enum` jsou odkazové typy, které nesplňují toto omezení.
    * `A` je parametr typu s omezením typu hodnoty ([omezení parametrů typu](classes.md#type-parameter-constraints)).
*  Pokud je omezením `new()`omezení konstruktoru, typ `A` nesmí být `abstract` a musí mít veřejný konstruktor bez parametrů. To je splněno, pokud je splněna jedna z následujících podmínek:
    * `A` je hodnotový typ, protože všechny typy hodnot mají veřejný výchozí konstruktor ([výchozí konstruktory](types.md#default-constructors)).
    * `A` je parametr typu s omezením konstruktoru ([omezení parametrů typu](classes.md#type-parameter-constraints)).
    * `A` je parametr typu s omezením typu hodnoty ([omezení parametrů typu](classes.md#type-parameter-constraints)).
    * `A` je třída, která není `abstract` a obsahuje explicitně deklarovaný `public` konstruktor bez parametrů.
    * `A` není `abstract` a má výchozí konstruktor ([výchozí konstruktory](classes.md#default-constructors)).

V době kompilace dojde k chybě, pokud některé z omezení parametru typu nejsou splněny danými argumenty typu.

Vzhledem k tomu, že parametry typu nejsou děděny, nejsou omezení nikdy děděna. V následujícím příkladu `D` nutné zadat omezení pro svůj parametr typu `T` tak, aby `T` splňovala omezení, které je stanoveno `B<T>`ou základní třídy. Naproti tomu třída `E` nemusí určovat omezení, protože `List<T>` implementuje `IEnumerable` pro všechny `T`.

```csharp
class B<T> where T: IEnumerable {...}

class D<T>: B<T> where T: IEnumerable {...}

class E<T>: B<List<T>> {...}
```

## <a name="type-parameters"></a>Parametry typu

Parametr typu je identifikátor označující typ hodnoty nebo typ odkazu, na který je parametr vázán za běhu.

```antlr
type_parameter
    : identifier
    ;
```

Vzhledem k tomu, že je možné vytvořit instanci parametru typu s mnoha různými skutečnými argumenty typu, mají parametry typu mírně různé operace a omezení než jiné typy. Zde jsou některé z nich:

*  Parametr typu nelze použít přímo k deklaraci základní třídy ([základní třídy](classes.md#base-class)) nebo rozhraní ([seznamů parametrů variant typu](interfaces.md#variant-type-parameter-lists)).
*  Pravidla pro vyhledávání členů u parametrů typu závisí na omezeních, pokud jsou použita pro parametr typu. Jsou podrobně popsány v [hledání členů](expressions.md#member-lookup).
*  Dostupné převody pro parametr typu závisí na omezeních, pokud jsou použita pro parametr typu. Jsou podrobně popsány v [implicitních převodech týkajících se parametrů typu](conversions.md#implicit-conversions-involving-type-parameters) a [explicitních dynamických převodů](conversions.md#explicit-dynamic-conversions).
*  Literál `null` nelze převést na typ předaný parametrem typu, s výjimkou toho, pokud je parametr typu znám jako typ odkazu ([implicitní převody zahrnující parametry typu](conversions.md#implicit-conversions-involving-type-parameters)). Místo toho se ale dá použít výraz `default` ([výrazy s výchozí hodnotou](expressions.md#default-value-expressions)). Kromě toho může být hodnota typu zadaná parametrem typu porovnána s `null` pomocí `==` a `!=` ([operátory rovnosti typu odkazu](expressions.md#reference-type-equality-operators)), pokud parametr typu nemá omezení typu hodnoty.
*  Výraz `new` ([výrazy vytvoření objektu](expressions.md#object-creation-expressions)) lze použít pouze s parametrem typu, pokud je parametr typu omezen hodnotou *constructor_constraint* nebo omezením typu hodnoty ([omezení parametrů typu](classes.md#type-parameter-constraints)).
*  Parametr typu se nedá použít kdekoli v rámci atributu.
*  Parametr typu se nedá použít v přístupu ke členu ([přístup k přístupu](expressions.md#member-access)ke členu) nebo názvu typu ([obor názvů a názvy typů](basic-concepts.md#namespace-and-type-names)) k identifikaci statického člena nebo vnořeného typu.
*  V nebezpečném kódu parametr typu nelze použít jako *unmanaged_type* ([typy ukazatelů](unsafe-code.md#pointer-types)).

Jako typ jsou parametry typu čistě konstrukce v čase kompilace. V době běhu jsou jednotlivé parametry typu vázány na typ modulu runtime, který byl zadán zadáním argumentu typu pro deklaraci obecného typu. Proto typ proměnné deklarované s parametrem typu v době běhu bude uzavřený konstruovaný typ ([otevřené a uzavřené typy](types.md#open-and-closed-types)). Spuštění všech příkazů a výrazů týkajících se parametrů typu za běhu používá skutečný typ, který byl zadán jako argument typu pro daný parametr.

## <a name="expression-tree-types"></a>Typy stromu výrazů

***Stromy výrazů*** umožňují znázornit výrazy lambda jako datové struktury namísto spustitelného kódu. Stromy výrazů jsou hodnoty ***typů stromové struktury výrazu*** `System.Linq.Expressions.Expression<D>`formuláře, kde `D` je jakýkoli typ delegáta. Pro zbytek této specifikace budeme na tyto typy odkazovat pomocí `Expression<D>`zkráceným na tyto typy.

Pokud převod existuje z výrazu lambda na typ delegáta `D`, existuje také převod na typ stromu výrazu `Expression<D>`. Zatímco převod výrazu lambda na typ delegáta vygeneruje delegáta, který odkazuje na spustitelný kód pro lambda výraz, převod na typ stromové struktury výrazu vytvoří reprezentace stromu výrazu lambda výrazu.

Stromy výrazů jsou efektivní reprezentace dat v paměti pro výrazy lambda a vytvoření struktury výrazu lambda transparentní a explicitní.

Podobně jako typ delegáta `D`, `Expression<D>` se říká, že mají parametry a návratové typy, které jsou stejné jako u `D`.

Následující příklad představuje výraz lambda jak jako spustitelný kód, tak jako strom výrazu. Vzhledem k tomu, že existuje převod na `Func<int,int>`, existuje také převod na `Expression<Func<int,int>>`:

```csharp
Func<int,int> del = x => x + 1;                    // Code

Expression<Func<int,int>> exp = x => x + 1;        // Data
```

Po těchto přiřazeních `del` delegát odkazuje na metodu, která vrací `x + 1`a strom výrazu `exp` odkazuje na strukturu dat, která popisuje výraz `x => x + 1`.

Přesná definice obecného typu `Expression<D>` a také přesné pravidla pro sestavování stromu výrazu v případě, že je výraz lambda převeden na typ stromu výrazu, jsou oba mimo rozsah této specifikace.

Existují dvě věci, které je třeba provést explicitně:

*  Ne všechny výrazy lambda lze převést na stromy výrazů. Například lambda výrazy s tělomi příkazů a lambda výrazy obsahující výrazy přiřazení nelze znázornit. V těchto případech převod stále existuje, ale v době kompilace se nezdaří. Tyto výjimky jsou podrobně popsané v [anonymních převodech funkcí](conversions.md#anonymous-function-conversions).
*   `Expression<D>` nabízí metodu instance `Compile`, která vytvoří delegáta typu `D`:

    ```csharp
    Func<int,int> del2 = exp.Compile();
    ```

    Volání tohoto delegáta způsobí spuštění kódu reprezentovaného stromovou strukturou výrazu. Proto jsou s ohledem na výše uvedené definice ekvivalentní del a del2 a následující dva příkazy budou mít stejný účinek:

    ```csharp
    int i1 = del(1);
    
    int i2 = del2(1);
    ```

    Po spuštění tohoto kódu budou `i1` a `i2` obě hodnoty `2`.

