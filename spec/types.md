# <a name="types"></a>Typy

Typy jazyka C# jsou rozděleny do dvou hlavních kategorií: ***typů hodnot*** a ***referenční typy***. Typy hodnot a odkazové typy mohou být ***obecných typů***, což trvat jednu nebo více ***parametry typu***. Parametry typu můžete určit oba typy hodnot a typy odkazů.

```antlr
type
    : value_type
    | reference_type
    | type_parameter
    | type_unsafe
    ;
```

Poslední kategorie typů ukazatele, je k dispozici pouze v nezabezpečený kód. Tento postup je popsán dále v [typy ukazatelů](unsafe-code.md#pointer-types).

Typy hodnot se liší od typy odkazů v tom, že proměnné typů hodnoty přímo obsahovat svá data, zatímco proměnné odkazu na typy úložiště ***odkazy*** ke svým datům, ten se označuje jako ***objekty***. V případě typů odkazu je možné pro dvě proměnné odkazovat na stejný objekt a proto možná pro operace v rámci jedné proměnné ovlivňovat objekt odkazovaný jinou proměnnou. S typy hodnot proměnných každý mají své vlastní kopii dat a není možné pro operace se na nich se má vliv na jinou.

Systém typů jazyka C# je jednotný tak, aby jako objekt lze považovat hodnotu libovolného typu. Všechny typy v jazyce C# přímo nebo nepřímo odvozuje od `object` typ, třídy a `object` je ultimate základní třída všech typů. Hodnoty odkazové typy jsou považovány za objekty jednoduše tak, že zobrazení hodnoty jako typ `object`. Hodnoty typy hodnot jsou považovány za objekty pomocí provádí operace zabalení a rozbalení ([zabalení a rozbalení](types.md#boxing-and-unboxing)).

## <a name="value-types"></a>Typy hodnot

Typ hodnoty je struktura typu nebo typu výčtu. Jazyk C# poskytuje sadu předdefinovaných struktury typů volá se, ***jednoduché typy***. Jednoduché typy jsou označeny pomocí vyhrazených slov.

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

Na rozdíl od proměnné typu odkazu, může obsahovat proměnné hodnotového typu hodnota `null` pouze v případě, že typ hodnoty je typ připouštějící hodnotu Null.  Pro každý typ hodnotu null je odpovídající typ s možnou hodnotou Null představující stejnou sadu hodnot a hodnoty `null`.

Přiřazení proměnné typu hodnoty vytvoří kopii přiřazené hodnoty. Tím se liší od přiřazení k proměnné typu odkazu, který zkopíruje odkaz, ale ne identifikovaný odkaz na objekt.

### <a name="the-systemvaluetype-type"></a>Typ System.ValueType

Všechny hodnotové typy implicitně dědí z třídy `System.ValueType`, který zase dědí z třídy `object`. Není možné pro jakýkoli typ odvození od typu hodnoty a typy hodnot jsou proto implicitně zapečetěné ([zapečetěné třídy](classes.md#sealed-classes)).

Všimněte si, že `System.ValueType` sám není *value_type*. Místo toho je *class_type* z kterého jsou všechny *value_type*s je automaticky odvozen.

### <a name="default-constructors"></a>Výchozí konstruktory

Všechny hodnotové typy implicitně deklarovat konstruktor instance bez parametrů, volá se, ***výchozí konstruktor***. Výchozí konstruktor vrátí instanci inicializován nulou označované jako ***výchozí hodnota*** pro typ hodnoty:

*  Pro všechny *simple_type*s, výchozí hodnota je hodnotu vytvořenou testovaným bitový vzor všechny nul:
    * Pro `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, a `ulong`, výchozí hodnota je `0`.
    * Pro `char`, výchozí hodnota je `'\x0000'`.
    * Pro `float`, výchozí hodnota je `0.0f`.
    * Pro `double`, výchozí hodnota je `0.0d`.
    * Pro `decimal`, výchozí hodnota je `0.0m`.
    * Pro `bool`, výchozí hodnota je `false`.
*  Pro *enum_type* `E`, výchozí hodnota je `0`, převést na typ `E`.
*  Pro *struct_type*, výchozí hodnota je hodnota vytvořen nastavením všechna pole na výchozí hodnoty a referenční dokumentace všech polí typu `null`.
*  Pro *nullable_type* výchozí hodnota je instance, pro kterou `HasValue` vlastnost má hodnotu false a `Value` vlastnost není definována. Výchozí hodnota je také ***hodnota null*** typu s možnou hodnotou Null.

Stejně jako ostatní konstruktor instance je vyvolán pomocí výchozího konstruktoru typu hodnoty `new` operátor. Z důvodu efektivity není tento požadavek určen ve skutečnosti mít implementaci generovat volání konstruktoru. V příkladu níže proměnné `i` a `j` jsou inicializovány na nulu.

```csharp
class A
{
    void F() {
        int i = 0;
        int j = new int();
    }
}
```

Vzhledem k tomu, že každý typ hodnoty implicitně obsahuje konstruktor instance bez parametrů, není možné u typu struktura obsahuje explicitní deklarace konstruktor bez parametrů. Chcete-li deklarovat instanci parametrizované konstruktory ale smí typu Struktura ([konstruktory](structs.md#constructors)).

### <a name="struct-types"></a>Typy struktury

Typ struktura je hodnotový typ, který lze deklarovat konstanty, pole, metody, vlastnosti, indexery, operátory, konstruktory instancí, statické konstruktory a vnořené typy. Deklarace typy struktury je popsána v [deklarace struktury](structs.md#struct-declarations).

### <a name="simple-types"></a>Jednoduché typy

Jazyk C# poskytuje sadu předdefinovaných struktury typů volá se, ***jednoduché typy***. Jednoduché typy jsou označeny pomocí vyhrazených slov, ale jsou jednoduše aliasy pro typy předdefinované struktury v těchto vyhrazených slov `System` obor názvů, jak je popsáno v následující tabulce.


| __Vyhrazené slovo__ | __Typ s aliasem__ |
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

Protože jednoduchý typ aliasy typu Struktura, má každý jednoduchý typ členů. Například `int` má členy deklarované v `System.Int32` a členy zděděné z `System.Object`, a jsou povoleny následující příkazy:

```csharp
int i = int.MaxValue;           // System.Int32.MaxValue constant
string s = i.ToString();        // System.Int32.ToString() instance method
string t = 123.ToString();      // System.Int32.ToString() instance method
```

Jednoduché typy liší od jiné typy struct, že umožňují některé další operace:

*  Povolit většinu jednoduchých typů hodnot má být vytvořen napsáním *literály* ([literály](lexical-structure.md#literals)). Například `123` je literál typu `int` a `'a'` je literál typu `char`. C# umožňuje žádné ustanovení pro literály typy struktury obecně a jiné než výchozí hodnoty jiné typy struktury jsou nakonec vždy vytvořené prostřednictvím konstruktory instancí z těchto typů struktury.
*  Když jsou jako operandy výrazu všechny jednoduchý typ konstanty, je možné kompilátor pro vyhodnocení výrazu v době kompilace. Takový výraz se označuje jako *constant_expression* ([konstantní výrazy](expressions.md#constant-expressions)). Výrazy zahrnující operátory definované ve jiné typy struct se nepovažují za konstantní výrazy.
*  Prostřednictvím `const` deklarace je možné deklarovat konstanty jednoduché typy ([konstanty](classes.md#constants)). Není možné mít konstanty jiné typy struct, ale Azure nenabízí podobné efekt `static readonly` pole.
*  Jednoduché typy zahrnující převody mohl podílet na vyhodnocení operátorů převodu definován jiným typem struct, ale operátor uživatelsky definovaný převod nikdy mohl podílet na vyhodnocení jiného uživatelem definovaný operátor ([hodnocení uživatelem definované převody](conversions.md#evaluation-of-user-defined-conversions)).

### <a name="integral-types"></a>Celočíselné typy

C# podporuje devět celočíselnými typy: `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, a `char`. Integrální typy mají následující velikostí a oblastí hodnot:

*  `sbyte` Představuje typ podepsaný 8bitová celá čísla se hodnoty v rozmezí -128 až 127.
*  `byte` Představuje typ bez znaménka 8bitová celá čísla se hodnoty v rozmezí 0 až 255.
*  `short` Představuje typ podepsaný 16bitová celá čísla s hodnotami mezi -32768 a 32767.
*  `ushort` Typ představuje celých čísel bez znaménka 16bitové hodnoty mezi 0 a 65535.
*  `int` Představuje typ podepsaný 32bitová celá čísla s hodnotami mezi -2147483648 a 2147483647.
*  `uint` Představuje typ bez znaménka 32bitových celých čísel pomocí hodnoty v rozmezí 0 až 4294967295.
*  `long` Představuje typ podepsaný 64bitová celá čísla mezi -9223372036854775808 a 9223372036854775807 hodnotami.
*  `ulong` Představuje typ bez znaménka 64bitová celá čísla se hodnoty v rozmezí 0 až 18446744073709551615.
*  `char` Typ představuje celých čísel bez znaménka 16bitové hodnoty mezi 0 a 65535. Sadu možných hodnot `char` typu odpovídá znakové sady Unicode. I když `char` má stejnou reprezentaci jako `ushort`, ne všechny operace povolena u jednoho typu nejsou povoleny na druhé.

Integrálového typu jednočlenné a binární operátory jsou vždy pracovat s podepsaný 32 bitů přesnosti, bez znaménka 32 bitů přesnosti, podepsaný 64 bitů přesnosti nebo bez znaménka 64 bitů přesnosti:

*  Pro unární `+` a `~` operátory, operand je převeden na typ `T`, kde `T` je prvním příspěvkem `int`, `uint`, `long`, a `ulong` plně, který může představovat všechny možné hodnoty operandu. Operace se pak provede pomocí přesnost typu `T`, a typ výsledku je `T`.
*  Pro unární `-` operátor operand je převeden na typ `T`, kde `T` je prvním příspěvkem `int` a `long` plně představující všechny možné hodnoty operandu. Operace se pak provede pomocí přesnost typu `T`, a typ výsledku je `T`. Unární `-` operátor nelze použít pro operandy typu `ulong`.
*  Pro binární soubor `+`, `-`, `*`, `/`, `%`, `&`, `^`, `|`, `==`, `!=`, `>`, `<`, `>=`, a `<=` operátory, operandy jsou převedeny na typ `T`, kde `T` je prvním příspěvkem `int`, `uint`, `long`, a `ulong` plně představující všechny možné hodnoty pro oba operandy. Operace se pak provede pomocí přesnost typu `T`, a typ výsledku je `T` (nebo `bool` pro relační operátory). Není povoleno pro jeden operand typu `long` a druhou typu `ulong` s binárními operátory.
*  Pro binární soubor `<<` a `>>` operátory, levý operand je převeden na typ `T`, kde `T` je prvním příspěvkem `int`, `uint`, `long`, a `ulong` plně, který může představovat všechny možné hodnoty operandu. Operace se pak provede pomocí přesnost typu `T`, a typ výsledku je `T`.

`char` Typ je klasifikován jako s integrálním typem, ale liší se od jiné typy celých čísel dvěma způsoby:

*  Neexistují žádné implicitní převody z jiných typů `char` typu. Zejména i když `sbyte`, `byte`, a `ushort` typy mají rozsahy hodnot, které jsou plně reprezentovat pomocí `char` implicitní převody z typů `sbyte`, `byte`, nebo `ushort` na `char` neexistují.
*  Konstanty objektu `char` typ musí zapsat, protože *character_literal*s nebo jako *integer_literal*s v kombinaci s přetypování na typ `char`. Například `(char)10` je stejný jako `'\x000A'`.

`checked` a `unchecked` operátorů a příkazů, které se používají k řízení přetečení zjišťování integrálového typu aritmetické operace a převody ([operátory zaškrtnuto a nezaškrtnuto](expressions.md#the-checked-and-unchecked-operators)). V `checked` kontextu, přetečení způsobí chybu kompilace nebo způsobí, že `System.OverflowException` vyvolání. V `unchecked` kontextu, jsou ignorovány přetečení a se zahodí všechny nejvyšším bity, které se nevejdou do cílového typu.

### <a name="floating-point-types"></a>Typy s plovoucí desetinnou čárkou

C# podporuje dva typy s plovoucí desetinnou čárkou: `float` a `double`. `float` a `double` typy jsou reprezentovány pomocí 32-bit jednoduchou přesností a 64-bit dvojité přesnosti IEEE 754 formátů, což poskytuje následující sadu hodnot:

*  Kladné nula a záporná nula. Ve většině situací, kladné nula a záporná nula chovají stejně jako jednoduchý hodnota nula, ale určité operace rozlišovat mezi dvěma ([operátor dělení](expressions.md#division-operator)).
*  Kladné nekonečno a záporné nekonečno. Tyto operace jako nenulové číslo dělení nulou vytvářených nekonečno. Například `1.0 / 0.0` vrací kladné nekonečno, a `-1.0 / 0.0` výnosy záporné nekonečno.
*  ***Not a Number*** hodnoty často označovaná zkratkou NaN. Neplatná operace s plovoucí desetinnou čárkou, např. dělení nulou nula generované hodnoty NaN.
*  Konečná sada nenulové hodnoty ve formuláři `s * m * 2^e`, kde `s` 1 nebo -1, a `m` a `e` se určují podle konkrétního typu s plovoucí desetinnou čárkou: Pro `float`, `0 < m < 2^24` a `-149 <= e <= 104`a pro `double`, `0 < m < 2^53` a `1075 <= e <= 970`. Nenormalizovaná čísla s plovoucí desetinnou čárkou jsou považovány za platné nenulové hodnoty.

`float` Typ může představovat hodnotu přibližně `1.5 * 10^-45` k `3.4 * 10^38` s přesností 7 číslic.

`double` Typ může představovat hodnotu přibližně `5.0 * 10^-324` k `1.7 × 10^308` s přesností 15 až 16 číslic.

Pokud jeden z operandů binárního operátoru s plovoucí desetinnou čárkou typu, pak je druhý operand musí být integrálního typu nebo typu s plovoucí desetinnou čárkou a operace se vyhodnotí takto:

*  Pokud jeden z operandů je typu celé číslo, tento operand je převeden na typ s plovoucí desetinnou čárkou je druhý operand.
*  Potom, pokud platí některá z operandů typu `double`, je druhý operand je převeden na `double`, operace se provádí pomocí alespoň `double` rozsah a přesnost a typ výsledku je `double` (nebo `bool` pro relační operátory).
*  V opačném případě se operace provádí používat alespoň `float` rozsah a přesnost a typ výsledku je `float` (nebo `bool` pro relační operátory).

Operátory s plovoucí desetinnou čárkou, včetně operátorů přiřazení nikdy vytvořit výjimky. Místo toho ve výjimečných případech s plovoucí desetinnou čárkou operací se dostaví nula, nekonečno a NaN, jak je popsáno níže:

*  Pokud výsledek operace s plovoucí desetinnou čárkou je příliš malá pro cílový formát, výsledek operace stane kladné nula nebo záporná nula.
*  Pokud výsledek operace s plovoucí desetinnou čárkou je příliš velký pro cílový formát, bude výsledek operace kladné nekonečno nebo záporné nekonečno.
*  Výsledek operace s plovoucí desetinnou čárkou operace je neplatná, stane NaN.
*  Pokud jeden nebo oba operandy operaci s plovoucí desetinnou čárkou je NaN, stane se výsledek operace NaN.

S vyšší přesností než typ výsledku operace lze provádět operace s plovoucí desetinnou čárkou. Například některé hardwarovou architekturou podporovat "Rozšířená" nebo "long double" s plovoucí desetinnou čárkou typu s větší rozsah a přesnost než `double` zadejte a implicitně vykonává všechny operace s plovoucí desetinnou čárkou pomocí tohoto typu vyšší přesnost. Pouze na nadměrné náklady na výkon takový hardwarovou architekturou provádět k provádění operací s plovoucí desetinnou čárkou s přesností na menší a nikoli vyžadovat implementaci ztrácí výkonu a přesnosti, C# umožňuje vyšší přesnost typu bude používá pro všechny operace s plovoucí desetinnou čárkou. Než jasné přesnější výsledky, to má jen zřídka měřitelné důsledky. Nicméně ve výrazech formuláře `x * y / z`, kde násobení vytváří výsledek, který je mimo `double` rozsah, ale následné dělení přináší dočasné výsledek zpět do `double` v rozsahu, skutečnost, že je výraz vyhodnoceny v rozsahu vyšší formátu může způsobit konečných výsledků bude vytvořen místo nekonečno.

### <a name="the-decimal-type"></a>Typ decimal

`decimal` Typ je typ 128bitových dat. vhodný pro výpočty finančních a přepočty měn. `decimal` Typ může představovat hodnotu `1.0 * 10^-28` na přibližně `7.9 * 10^28` s 28 až 29 platnými číslicemi.

Omezená sada hodnot typu `decimal` jsou ve tvaru `(-1)^s * c * 10^-e`, kde znaménko `s` je 0 nebo 1, koeficient `c` je dán `0 <= *c* < 2^96`a rozšířit možnosti škálování `e` je tak, aby `0 <= e <= 28`. `decimal` Typ nepodporuje podepsaný nuly, nekonečno nebo na NaN. A `decimal` je reprezentována jako celé číslo verze 96 měřítkem řídit sílu deset. Pro `decimal`s absolutní hodnota menší než `1.0m`, hodnota je přesné 28 desetinné čárky, ale žádné další. Pro `decimal`s absolutní hodnota větší než nebo rovna hodnotě `1.0m`, hodnota je přesné 28 nebo 29 číslic. Contrary k `float` a `double` datové typy, decimal desetinná čísla, jako je například 0,1 může být reprezentován přesně `decimal` reprezentace. V `float` a `double` reprezentace, tato čísla jsou často nekonečné zlomky provádění těchto reprezentace náchylnější k zaokrouhlení chyby.

Pokud je jeden z operandů binární operátor typu `decimal`, je druhý operand musí být integrálního typu nebo typu `decimal`. Je-li operand celočíselný typ je k dispozici, je převedena na `decimal` předtím, než se operace provádí.

Výsledkem operace na hodnotách typu `decimal` je, který by byl výsledkem výpočtu přesné výsledky (zachování škálování, jak jsou definovány pro každý operátor) a potom zaokrouhlení přizpůsobena reprezentace. Výsledky jsou zaokrouhleny na nejbližší reprezentovatelnou hodnotu, a pokud výsledek je stejně blízko dvě reprezentovatelných hodnot, hodnotu, která má sudé číslo nejméně významných číslic pozici (to se označuje jako "banker je zaokrouhlení"). Žádný výsledek má vždy znaménko čísla 0 a měřítkem 0.

V případě desítkové aritmetické operace vytvoří hodnotu menší než nebo rovna hodnotě `5 * 10^-29` výsledek operace v absolutní hodnota klesne na nulu. Pokud `decimal` vytváří výsledek, který je příliš velká pro aritmetické operace `decimal` formátu, `System.OverflowException` je vyvolána.

`decimal` Typ má zato větší přesnost ale menší rozsah než u typů s plovoucí desetinnou čárkou. Proto převody z typů s plovoucí desetinnou čárkou na `decimal` vzniknout výjimky přetečení a převody z `decimal` na typy s plovoucí desetinnou čárkou může způsobit ztrátu přesnosti. Z těchto důvodů, neexistuje žádný implicitní převod mezi typy s plovoucí desetinnou čárkou a `decimal`, a bez explicitního přetypování, není možné kombinovat s plovoucí desetinnou čárkou a `decimal` operandy ve stejném výrazu.

### <a name="the-bool-type"></a>Typ bool

`bool` Typ představuje logickou logické množství. Možné hodnoty typu `bool` jsou `true` a `false`.

Neexistuje žádný standardní převod mezi `bool` a dalších typů. Konkrétně se `bool` typ je samostatný a oddělený od celočíselných typů a `bool` hodnotu nelze použít namísto celé číslo a naopak.

V jazycích C a C++ nulová hodnota integrálního typu nebo s plovoucí desetinnou čárkou nebo ukazatel s hodnotou null lze převést na logickou hodnotu `false`, a nenulové hodnoty s plovoucí desetinnou čárkou nebo celočíselné nebo jiných nulového ukazatele lze převést na logickou hodnotu `true`. V jazyce C#, můžete tyto převody provést explicitní porovnání hodnotu s plovoucí desetinnou čárkou nebo celočíselné hodnotě nula, nebo explicitní odkaz na objekt k porovnání `null`.

### <a name="enumeration-types"></a>Výčtové typy

Typ výčtu je odlišný typ se pojmenovaných konstant. Každý typ výčtu se základní typ, který musí být `byte`, `sbyte`, `short`, `ushort`, `int`, `uint`, `long` nebo `ulong`. Sadu hodnot typu výčtu je stejný jako sadu hodnot ze základního typu. Hodnoty typu výčtu nejsou omezené na hodnoty pojmenovaných konstant. Výčtové typy jsou definované pomocí deklarace výčtů ([deklarace výčtu](enums.md#enum-declarations)).

### <a name="nullable-types"></a>Typy s povolenou hodnotou Null

Typ připouštějící hodnotu Null, může představovat všechny hodnoty jeho ***nadřízený typ*** plus další hodnotu null. Typ s možnou hodnotou Null se zapíše `T?`, kde `T` je základní typ. Tato syntaxe je zkratka pro `System.Nullable<T>`, a dvě různými formami zaměnitelné.

A ***hodnotu Null typu*** a naopak je libovolný typ hodnoty jiné než `System.Nullable<T>` a jeho zkrácený tvar vlastností `T?` (pro všechny `T`), a navíc všechny parametr typu, který je omezen na hodnotu Null typu (to znamená všechny parametr typu s `struct` omezení). `System.Nullable<T>` Typ Určuje typ omezení hodnoty pro `T` ([omezení parametru typu](classes.md#type-parameter-constraints)), což znamená, že základní typ s možnou hodnotou Null typu může být libovolný typ hodnotu Null. Základní typ s možnou hodnotou Null typu nemůže být typ připouštějící hodnotu null nebo typ odkazu. Například `int??` a `string?` jsou neplatné typy.

Instanci typu s možnou hodnotou Null `T?` mají dvě veřejné vlastnosti jen pro čtení:

*  A `HasValue` vlastnost typu `bool`
*  A `Value` vlastnost typu `T`

Instance, pro kterou `HasValue` je PRAVDA se říká, že chcete mít hodnotu null. Nenulové instance obsahuje známou hodnotou a `Value` vrátí tuto hodnotu.

Instance, pro kterou `HasValue` je false se říká, že chcete mít hodnotu null. Nedefinovaná hodnota je instanci null. Při čtení `Value` instance null způsobí, že `System.InvalidOperationException` vyvolání. Přístup k procesu `Value` vlastnost instance s možnou hodnotou Null se označuje jako ***rozbalení***.

Kromě výchozího konstruktoru, každý typ s možnou hodnotou Null `T?` má veřejný konstruktor, který přijímá jeden argument typu `T`. Zadána hodnota `x` typu `T`, vyvolání konstruktoru formuláře

```csharp
new T?(x)
```
vytvoří instanci nenulovou `T?` pro kterou `Value` vlastnost `x`. Proces vytváření nenulovou instanci typu s možnou hodnotou Null pro dané hodnoty se označuje jako ***obtékání***.

Implicitní převody jsou k dispozici na `null` literál `T?` ([převody literál s hodnotou Null](conversions.md#null-literal-conversions)) a z `T` k `T?` ([implicitních převodů s možnou hodnotou Null](conversions.md#implicit-nullable-conversions)).

## <a name="reference-types"></a>Odkazové typy

Odkazový typ je typ třídy, typem rozhraní, typem pole nebo typ delegáta.

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

Hodnota typu odkazu je odkazem na ***instance*** typu, ten se označuje jako ***objekt***. Zvláštní hodnota `null` je kompatibilní se všemi typy odkazu a ukazuje na nepřítomnost instance.

### <a name="class-types"></a>Typy tříd

Typ třídy definuje datová struktura, která obsahuje datové členy (konstant a polí), funkce členy (metody, vlastnosti, události, indexery, operátory, konstruktory instancí, destruktory a statické konstruktory) a vnořené typy. Typy tříd podporují dědičnost, mechanismus, kterým odvozené třídy můžete rozšířit a specialize základní třídy. Instance typů tříd jsou vytvořené pomocí *object_creation_expression*s ([výrazy vytvoření objektu](expressions.md#object-creation-expressions)).

Typy tříd jsou popsány v [třídy](classes.md).

Některé typy předdefinované třídy mají zvláštní význam v jazyce C#, jak je popsáno v následující tabulce.


| __Typ třídy.__     | __Popis__                                         |
|--------------------|---------------------------------------------------------|
| `System.Object`    | Ultimate základní třídy pro všechny ostatní typy. Zobrazit [typ objektu](types.md#the-object-type). | 
| `System.String`    | Typ řetězec jazyka C#. Zobrazit [typ řetězce](types.md#the-string-type).         |
| `System.ValueType` | Základní třída všechny hodnotové typy. Zobrazit [typ The System.ValueType](types.md#the-systemvaluetype-type).          |
| `System.Enum`      | Základní třída všech typů výčtu. Zobrazit [výčty](enums.md).              |
| `System.Array`     | Základní třída všechny typy polí. Zobrazit [pole](arrays.md).             |
| `System.Delegate`  | Základní třídy pro všemi typy delegátů. Zobrazit [delegáti](delegates.md).          |
| `System.Exception` | Základní třída všech typů výjimek. Zobrazit [výjimky](exceptions.md).         |

### <a name="the-object-type"></a>Typ objektu

`object` Typu třídy je ultimate základní třídy pro všechny ostatní typy. Všechny typy v jazyce C# přímo nebo nepřímo odvozuje od `object` typ třídy.

Klíčové slovo `object` je pouze aliasem pro třídu předdefinované `System.Object`.

### <a name="the-dynamic-type"></a>Dynamický typ.

`dynamic` Typ, jako je třeba `object`, můžete odkazovat na libovolný objekt. Kdy jsou operátory pro výrazy typu používat `dynamic`, jejich rozlišení je odloženo, dokud spuštění programu. Proto pokud operátor nelze použít právně pro odkazovaného objektu, je předána žádná chyba během kompilace. Místo toho bude vyvolána výjimka při překladu operátor, který selže v době běhu.

Jeho účelem je umožnit dynamické vazby, která je popsána v části [dynamické vazby](expressions.md#dynamic-binding).

`dynamic` je považován za shodný s `object` s výjimkou v těchto ohledech:

*  Operace na výrazy typu `dynamic` může být vázaný dynamicky ([dynamické vazby](expressions.md#dynamic-binding)).
*  Odvození typu ([odvození typu](expressions.md#type-inference)) preferovali `dynamic` přes `object` Pokud jsou obě kandidáty.

Z důvodu této ekvivalence obsahuje následující:

*  Zde je implicitní identity převod mezi `object` a `dynamic`a mezi sestavené typy, které jsou stejné při nahrazování `dynamic` s `object`
*  Implicitní a explicitní převody do a z `object` platí také do a z `dynamic`.
*  Podpisy metod, které jsou stejné při nahrazování `dynamic` s `object` jsou považovány za stejného podpisu
*  Typ `dynamic` je nerozeznatelná od `object` v době běhu.
*  Výraz typu `dynamic` se označuje jako ***dynamického výrazu***.

### <a name="the-string-type"></a>Typ string

`string` Typ je typ třídy sealed, který dědí přímo z `object`. Instance `string` třídy představují řetězce znaků Unicode.

Hodnoty `string` typu lze zapsat jako řetězec literálů ([řetězcových literálů](lexical-structure.md#string-literals)).

Klíčové slovo `string` je pouze aliasem pro třídu předdefinované `System.String`.

### <a name="interface-types"></a>Typy rozhraní

Rozhraní definuje kontrakt. Třída nebo struktura, která implementuje rozhraní musí dodržovat jeho kontrakt. Rozhraní může dědit z více základních rozhraní a třídy nebo struktury může implementovat více rozhraní.

Typy rozhraní jsou popsány v [rozhraní](interfaces.md).

### <a name="array-types"></a>Typy polí

Pole je datová struktura, která obsahuje nula nebo více proměnných, které jsou přístupné prostřednictvím vypočítané indexy. Proměnné, které jsou obsaženy v poli, také nazývají prvky pole, jsou všechny stejného typu a tento typ se nazývá typ prvku pole.

Typy pole jsou popsány v [pole](arrays.md).

### <a name="delegate-types"></a>Typy delegátů

Delegát je datová struktura, která odkazuje na jeden nebo více metod. Pro instanci metody, také odkazuje na jejich odpovídající instance objektů.

Nejbližší ekvivalent delegáta v jazyce C nebo C++ je ukazatel na funkci, ale že ukazatele na funkci, mohou odkazovat pouze na statické funkce, můžete odkazovat na statické a instanci metody delegáta. V takovém případě delegáta ukládá pouze odkaz na metody vstupního bodu, ale také odkaz na instanci objektu, na kterém se má vyvolat metodu.

Typy delegátů jsou popsány v [delegáti](delegates.md).

## <a name="boxing-and-unboxing"></a>Zabalení a rozbalení

Pojem zabalení a rozbalení je systém typů jazyka C#. Představuje most mezi *value_type*s a *reference_type*s tím, že všechny výhody *value_type* má být převeden do a z typů `object`. Zabalení a rozbalení umožňuje jednotný přehled o systému typů, ve které hodnotu libovolného typu lze nakonec zacházet jako objekt.

### <a name="boxing-conversions"></a>Zabalení převody

Umožňuje převod na uzavřené určení *value_type* má implicitně převést na *reference_type*. Existují následující převody zabalení:

*  Z libovolného *value_type* typu `object`.
*  Z libovolného *value_type* typu `System.ValueType`.
*  Z libovolného *non_nullable_value_type* k libovolnému *interface_type* implementované *value_type*.
*  Z libovolného *nullable_type* k libovolnému *interface_type* implementovaných základní typ *nullable_type*.
*  Z libovolného *enum_type* typu `System.Enum`.
*  Z libovolného *nullable_type* s jako základ *enum_type* typu `System.Enum`.
*  Vezměte na vědomí, že implicitní převod z typu parametru jako převod na uzavřené určení bude proveden, pokud v době běhu skončilo převod z typu hodnoty na typ odkazu ([implicitních převodů zahrnující parametry typu](conversions.md#implicit-conversions-involving-type-parameters)).

Zabalení hodnotu *non_nullable_value_type* se skládá z přidělování instancí objektu a kopírování *non_nullable_value_type* hodnoty do této instance.

Zabalení hodnotu *nullable_type* vytvoří odkaz s hodnotou null, pokud je `null` hodnotu (`HasValue` je `false`), nebo výsledek rozbalení a jinak zabalení zdrojovou hodnotu.

Samotný proces zabalení hodnotu *non_nullable_value_type* je nejlépe vysvětlit užívat existenci obecný ***třída zabalení***, který se chová jako kdyby byly deklarovány následujícím způsobem:

```csharp
sealed class Box<T>: System.ValueType
{
    T value;

    public Box(T t) {
        value = t;
    }
}
```

Zabalení hodnoty `v` typu `T` nyní se skládá z provádění výrazu `new Box<T>(v)`a vrátí výsledný instance jako hodnotu typu `object`. Díky tomu se příkazy
```csharp
int i = 123;
object box = i;
```
koncepčně odpovídají
```csharp
int i = 123;
object box = new Box<int>(i);
```

Třída zabalení jako `Box<T>` výše neexistuje ve skutečnosti a dynamického typu zabalené hodnoty není ve skutečnosti typem třídy. Místo toho zabalené hodnoty typu `T` má dynamického typu `T`a pomocí dynamického typu kontroly `is` operátor můžete jednoduše odkazovat na typ `T`. Například
```csharp
int i = 123;
object box = i;
if (box is int) {
    Console.Write("Box contains an int");
}
```
výstup řetězec "`Box contains an int`" v konzole.

Převod na uzavřené určení znamená vytvoření kopie hodnoty se v poli. Tím se liší od převod *reference_type* na typ `object`, kterými se hodnota dál odkazovat na stejnou instanci a jednoduše se považuje za méně odvozený typ `object`. Mějme například deklarace
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
Následující příkazy
```csharp
Point p = new Point(10, 10);
object box = p;
p.x = 20;
Console.Write(((Point)box).x);
```
bude výstupní hodnotu 10 v konzole, protože operace implicitního zabalení, ke které dochází v přiřazení `p` k `box` způsobí, že hodnota `p` ke zkopírování. Měl `Point` byla deklarována `class` hodnota 20 místo toho bude výstup, protože `p` a `box` by odkazovat na stejnou instanci.

### <a name="unboxing-conversions"></a>Rozbalení převody

Povoluje unboxingového převodu *reference_type* má být explicitně převeden na *value_type*. Existují následující rozbalení převody:

*  Z typu `object` k libovolnému *value_type*.
*  Z typu `System.ValueType` k libovolnému *value_type*.
*  Z libovolného *interface_type* k libovolnému *non_nullable_value_type* , který implementuje *interface_type*.
*  Z libovolného *interface_type* k libovolnému *nullable_type* jehož základní typ implementuje *interface_type*.
*  Z typu `System.Enum` k libovolnému *enum_type*.
*  Z typu `System.Enum` k libovolnému *nullable_type* s jako základ *enum_type*.
*  Vezměte na vědomí, že explicitní převod typu parametru jako unboxingového převodu. bude proveden, pokud v době běhu skončilo převod z typu odkazu na typ hodnoty ([explicitních převodů dynamické](conversions.md#explicit-dynamic-conversions)).

Rozbalení operace *non_nullable_value_type* se skládá z nejdřív zkontrolovali, že se instance objektu je zabalený hodnotu daný *non_nullable_value_type*a zkopírujte hodnotu z celkového počtu instance.

K rozbalení *nullable_type* vytvoří hodnotu null *nullable_type* zdrojového operandu, je-li `null`, nebo zabalená výsledek rozbalení instance objektu určeného k základního typu *nullable_type* jinak.

Odkaz na třídu imaginární zabalení je popsáno v předchozí části, unboxingového převodu objektu `box` k *value_type* `T` se skládá z provádění výrazu `((Box<T>)box).value`. Díky tomu se příkazy
```csharp
object box = 123;
int i = (int)box;
```
koncepčně odpovídají
```csharp
object box = new Box<int>(123);
int i = ((Box<int>)box).value;
```

Pro rozbalení převod na daný *non_nullable_value_type* úspěšné v době běhu, hodnota zdrojový operand musí být odkaz na o zabalenou hodnotu, která *non_nullable_value_type*. Pokud je zdrojový operand `null`, `System.NullReferenceException` je vyvolána výjimka. Pokud zdrojový operand je odkaz na objekt není kompatibilní `System.InvalidCastException` je vyvolána výjimka.

Pro rozbalení převod na daný *nullable_type* úspěšné v době běhu, hodnota zdrojový operand musí být buď `null` nebo odkaz na základní o zabalenou hodnotu *non_nullable_value_type* z *nullable_type*. Pokud zdrojový operand je odkaz na objekt není kompatibilní `System.InvalidCastException` je vyvolána výjimka.

## <a name="constructed-types"></a>Sestavené typy

Deklarace obecného typu, samostatně, označuje ***nevázaných obecného typu*** jako "plán", který se používá k vytvoření mnoho různých typů, mimo jiné použití ***argumenty typu***. Zadejte argumenty jsou zapsány v lomených závorkách (`<` a `>`) hned za název obecného typu. Typ, který obsahuje alespoň jeden argument typu je volána ***konstruovaný typ***. Konstruovaný typ. je možné ve většině případů v jazyce, ve kterém můžete zobrazit název typu. Nevázaný parametr generického typu jde použít jenom v rámci *typeof_expression* ([operátor typeof](expressions.md#the-typeof-operator)).

Sestavené typy lze také použít ve výrazech jako jednoduchý názvy ([jednoduché názvy](expressions.md#simple-names)), nebo když přístup ke členovi ([přístup ke členu](expressions.md#member-access)).

Když *namespace_or_type_name* je Vyhodnocená, jenom obecné typy s správné číslo, které jsou považovány za parametry typu. Proto je možné použít stejný identifikátor pro identifikaci různých typů, tak dlouho, dokud typy mají různý počet parametrů typu. To je užitečné při kombinování obecných a neobecných třídách ve stejném programu:

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

A *type_name* konstruovaný typ může identifikovat, i když není přímo zadat parametry typu. Tato situace může nastat, pokud typu je vnořená v deklaraci obecné třídy a instance typu obsahující deklarace se implicitně používá pro vyhledávání názvu ([vnořené typy obecných tříd](classes.md#nested-types-in-generic-classes)):

```csharp
class Outer<T>
{
    public class Inner {...}

    public Inner i;                // Type of i is Outer<T>.Inner
}
```

Nezabezpečený kód konstruovaný typ nelze použít jako *unmanaged_type* ([typy ukazatelů](unsafe-code.md#pointer-types)).

### <a name="type-arguments"></a>Argumenty typu

Každý argument v seznamu argumentů typu se používá jednoduchý *typ*.

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

Nezabezpečený kód ([nezabezpečený kód](unsafe-code.md)), *type_argument* nesmí být typu ukazatel. Každý argument typu musí splňovat žádné omezení na odpovídající parametr typu ([omezení parametru typu](classes.md#type-parameter-constraints)).

### <a name="open-and-closed-types"></a>Otevřené a uzavřené typy

Všechny typy mohou být klasifikovány jako buď ***otevřete typy*** nebo ***uzavření typů***. Otevřený typ je typ, který zahrnuje parametry typu. A konkrétně:

*  Parametr typu definuje otevřeného typu.
*  Typ pole je otevřený typ pouze v případě jeho typ prvku je otevřeného typu.
*  Konstruovaný typ je otevřený typ a pouze v případě jeden nebo více jeho argumentů typu je otevřeného typu. Konstruovaný vnořený typ je otevřený typ a pouze v případě nejméně jeden z argumentů typu nebo argumenty typu nadřazeného typu (typů) je otevřeného typu.

Uzavřený typ je typ, který není otevřeného typu.

V době běhu veškerý kód v rámci deklarace obecného typu je provedena v kontextu uzavřený konstruovaný typ, který byl vytvořen použití argumentů typu pro obecný deklarace. Každý z parametrů typu v rámci obecného typu je vázán na konkrétním typu za běhu. Zpracování za běhu všechny příkazy a výrazy vždy dojde k zavřené typy a otevřené typy dojde jenom během kompilace zpracování.

Každý uzavřený konstruovaný typ. má svou vlastní sadu statických proměnných, které nejsou sdíleny s jinými uzavřený konstruovaný typy. Protože otevřeného typu v době běhu neexistuje, nejsou žádné statických proměnných spojených s otevřeného typu. Dva typy uzavřený konstruovaný jsou stejného typu, pokud jsou konstruovány ze stejné nevázaný parametr generického typu a jejich odpovídající argumenty typu jsou stejného typu.

### <a name="bound-and-unbound-types"></a>Provázaná a nevázaného typy

Termín ***nevázaný typ*** odkazuje na jiného než obecného typu nebo nevázaný parametr generického typu. Termín ***vázaný typ*** odkazuje na jiného než obecného typu nebo konstruovaný typ.

Nevázanému typu odkazuje na entity deklarované v deklaraci typu. Nevázaný parametr generického typu je sám není typem a nelze použít jako typ proměnné, argumentu nebo návratovou hodnotu, nebo jako základního typu. Jediný konstruktor, ve kterém může být odkazováno nevázaný parametr generického typu je `typeof` výrazu ([operátor typeof](expressions.md#the-typeof-operator)).

### <a name="satisfying-constraints"></a>Splňující omezení

Pokaždé, když je odkazováno konstruovaný typ nebo obecné metody, zadaný typ argumenty jsou porovnávána s omezeními parametrů typů deklarována v obecném typu nebo metodě ([omezení parametru typu](classes.md#type-parameter-constraints)). Pro každou `where` klauzule, argument typu `A` , který odpovídá pojmenovaný parametr typu je porovnávána s každé omezení následujícím způsobem:

*  Pokud je omezení typu třídy, typ rozhraní nebo parametr typu, dejte `C` představují, že omezení s zadaný typ argumenty nahrazené za všechny parametry typu, které se zobrazují v omezení. Tím se uspokojí omezení, musí být případ, který typ `A` převést na typ `C` pomocí jedné z následujících akcí:
    * Konverzi identity ([Identity převod](conversions.md#identity-conversion))
    * Implicitní referenční převod ([odkaz na implicitní převody](conversions.md#implicit-reference-conversions))
    * Převod na uzavřené určení ([zabalení převody](conversions.md#boxing-conversions)), za předpokladu, že je typ A typ hodnotu Null.
    * Implicitní převod parametrů typu odkazu, zabalení nebo z parametru typu `A` k `C`.
*  Pokud je omezení, omezení typu odkazu (`class`), typ `A` musí splňovat jeden z následujících akcí:
    * `A` je typ rozhraní, typ třídy, typ delegáta nebo typ pole. Všimněte si, že `System.ValueType` a `System.Enum` jsou odkazové typy, které odpovídají tomuto omezení.
    * `A` je parametr typu, který je znám jako typ odkazu ([omezení parametru typu](classes.md#type-parameter-constraints)).
*  Pokud je typ omezení hodnoty omezení (`struct`), typ `A` musí splňovat jeden z následujících akcí:
    * `A` je typ struktury nebo výčtového typu, ale není typu s možnou hodnotou Null. Všimněte si, že `System.ValueType` a `System.Enum` jsou odkazové typy, které nesplňují omezení.
    * `A` je parametr typu s omezení typu hodnoty ([omezení parametru typu](classes.md#type-parameter-constraints)).
*  Pokud je omezení, omezení konstruktoru `new()`, typ `A` nesmí být `abstract` a musí mít veřejný konstruktor bez parametrů. Tím je splněna, pokud platí jedna z následujících akcí:
    * `A` je typ hodnoty, protože všechny hodnotové typy mít veřejný výchozí konstruktor ([výchozí konstruktory](types.md#default-constructors)).
    * `A` je parametr typu s omezení konstruktoru ([omezení parametru typu](classes.md#type-parameter-constraints)).
    * `A` je parametr typu s omezení typu hodnoty ([omezení parametru typu](classes.md#type-parameter-constraints)).
    * `A` je třída, která není `abstract` a obsahuje explicitně deklarované `public` konstruktor bez parametrů.
    * `A` není `abstract` a má výchozí konstruktor ([výchozí konstruktory](classes.md#default-constructors)).

Chyba kompilace nastane, pokud jeden nebo více omezení parametru typu nejsou splněné argumenty daného typu.

Protože nedědí parametry typu, omezení nejsou nikdy buď dědí. V následujícím příkladu `D` musí určovat omezení u jeho typu parametru `T` tak, aby `T` splňuje omezení stanovené základní třídy `B<T>`. Naproti tomu třídy `E` nemusí zadat omezení, protože `List<T>` implementuje `IEnumerable` libovolné `T`.

```csharp
class B<T> where T: IEnumerable {...}

class D<T>: B<T> where T: IEnumerable {...}

class E<T>: B<List<T>> {...}
```

## <a name="type-parameters"></a>Parametry typu

Parametr typu je identifikátor označující typ hodnoty nebo typ odkazu, který parametr vázán v době běhu.

```antlr
type_parameter
    : identifier
    ;
```

Protože parametr typu může být vytvořena s mnoha různých skutečný typ argumentů, parametry typu mají mírně odlišné operace a omezení než u jiných typů. Zde jsou některé z nich:

*  Parametr typu nelze použít přímo na deklarace základní třídy ([základní třída](classes.md#base-class)) nebo rozhraní ([seznamy parametru typu Variant](interfaces.md#variant-type-parameter-lists)).
*  Pravidla pro vyhledávání člen v typu, že parametry závisí na omezení, pokud existuje, použitý u parametru typu. Jsou podrobně popsány v [člen vyhledávání](expressions.md#member-lookup).
*  K dispozici převody pro parametr typu závisí na omezení, pokud existuje, použitý u parametru typu. Jsou podrobně popsány v [implicitních převodů zahrnující parametry typu](conversions.md#implicit-conversions-involving-type-parameters) a [explicitních převodů dynamické](conversions.md#explicit-dynamic-conversions).
*  Literál `null` nelze převést na typ daný parametr typu s výjimkou případů, pokud je parametr typu je znám jako typ odkazu ([implicitních převodů zahrnující parametry typu](conversions.md#implicit-conversions-involving-type-parameters)). Ale `default` výrazu ([výchozí hodnotu výrazy](expressions.md#default-value-expressions)) lze použít. Kromě toho lze porovnat hodnotu s typ daný parametr typu s `null` pomocí `==` a `!=` ([operátory rovnosti pro typ odkazu](expressions.md#reference-type-equality-operators)), pokud parametr typu neobsahuje omezení typu hodnoty.
*  A `new` výrazu ([výrazy vytvoření objektu](expressions.md#object-creation-expressions)) lze použít pouze s parametrem typu Pokud parametr typu je omezená *constructor_constraint* nebo typ omezení (hodnoty[ Typ omezení parametru](classes.md#type-parameter-constraints)).
*  Parametr typu nelze použít kdekoli v rámci atributu.
*  Parametr typu nelze použít v přístupu ke členu ([přístup ke členu](expressions.md#member-access)) nebo název typu ([Namespace a zadejte názvy](basic-concepts.md#namespace-and-type-names)) k identifikaci statických členů nebo vnořeného typu.
*  Nebezpečný kód, parametr typu nelze použít jako *unmanaged_type* ([typy ukazatelů](unsafe-code.md#pointer-types)).

Jako typ parametry typu jsou čistě konstrukci za kompilace. V době běhu je každý parametr typu vázán na run-time typu, který byl zadán zadáním argument typu pro obecný typ deklarace. Typ proměnné deklarované pomocí parametru typ se v době běhu, proto být uzavřený konstruovaný typ. ([otevřené a uzavřené typy](types.md#open-and-closed-types)). Provádění běhu všechny příkazy a výrazy, které zahrnují parametry typu používá skutečný typ, který byl zadán jako argument typu pro tento parametr.

## <a name="expression-tree-types"></a>Typy stromu výrazů

***Stromy výrazů*** povolit lambda výrazy, aby se dala vyjádřit jako datové struktury místo spustitelného kódu. Stromy výrazů jsou hodnoty ***typy stromu výrazů*** formuláře `System.Linq.Expressions.Expression<D>`, kde `D` libovolný typ delegáta. Zbytek této specifikaci jsme bude odkazovat na tyto typy pomocí zkrácený `Expression<D>`.

Pokud existuje z výrazu lambda převod na typ delegáta `D`, také existuje převod do typu stromu výrazu `Expression<D>`. Zatímco převodu výrazu lambda, typu delegátu generuje delegáta, který odkazuje na spustitelného kódu pro výraz lambda, převod na typu stromu výrazu vytvoří reprezentaci výrazu strom výrazu lambda.

Stromy výrazů jsou efektivní data v paměti reprezentace výrazů lambda a struktuře výrazu lambda transparentní a explicitní.

Stejně jako typ delegáta `D`, `Expression<D>` má parametr a návratové typy, které jsou stejné jako u `D`.

Následující příklad představuje výraz lambda jako spustitelný kód i ve stromu výrazu. Vzhledem k tomu, že existuje převod na `Func<int,int>`, také existuje převod na `Expression<Func<int,int>>`:

```csharp
Func<int,int> del = x => x + 1;                    // Code

Expression<Func<int,int>> exp = x => x + 1;        // Data
```

Následující tato přiřazení delegáta `del` odkazuje na metodu, která vrací `x + 1`a strom výrazu `exp` odkazuje datová struktura, která popisuje výraz `x => x + 1`.

Přesné definici obecného typu `Expression<D>` a také přesné pravidla pro vytváření stromu výrazů při převodu typu stromu výrazu lambda výraz, jsou nad rámec téhle specifikaci.

Je důležité, aby explicitní dvě věci:

*  Ne všechny lambda výrazy můžete převést na stromy výrazů. Nelze reprezentovat například lambda výrazy s těla příkazu a výrazů lambda, který obsahuje výrazy přiřazení. V těchto případech převod stále existuje, ale selže v době kompilace. Tyto výjimky jsou podrobně popsány v [anonymní funkce převody](conversions.md#anonymous-function-conversions).
*   `Expression<D>` poskytuje metodu instance `Compile` produkuje delegát typu `D`:

    ```csharp
    Func<int,int> del2 = exp.Compile();
    ```

    Vyvolání tohoto delegáta způsobí, že kód reprezentována stromu výrazů má být proveden. Proto výše uvedené definice, del a del2 jsou ekvivalentní a následující dva příkazy bude mít stejný účinek:

    ```csharp
    int i1 = del(1);
    
    int i2 = del2(1);
    ```

    Po spuštění tohoto kódu `i1` a `i2` budou mít obě hodnotu `2`.

