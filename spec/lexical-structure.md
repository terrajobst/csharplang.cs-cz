---
ms.openlocfilehash: 4676bcd3f0a92260b4e5e20a0aa5b5ec00bf204e
ms.sourcegitcommit: 892af9016b3317a8fae12d195014dc38ba51cf16
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/01/2019
ms.locfileid: "71704076"
---
# <a name="lexical-structure"></a>Lexikální struktura

## <a name="programs"></a>Programy

C# ***Program*** se skládá z jednoho nebo více ***zdrojových souborů***, známých ve formě ***kompilačních jednotek*** ([kompilačních jednotek](namespaces.md#compilation-units)). Zdrojový soubor je uspořádaná sekvence znaků Unicode. Zdrojové soubory mají obvykle korespondenci se soubory v systému souborů, ale tato korespondence se nevyžaduje. Pro maximální přenositelnost doporučujeme, aby soubory v systému souborů byly kódovány pomocí kódování UTF-8.

Koncepčně řečeno, program se kompiluje pomocí tří kroků:

1. Transformace, která převede soubor z konkrétního znakového repertoáru a schématu kódování do sekvence znaků Unicode.
2. Lexikální analýza, která překládá datový proud vstupních znaků Unicode do datového proudu tokenů.
3. Syntaktická analýza, která překládá datový proud tokenů do spustitelného kódu.

## <a name="grammars"></a>Gramatiky

Tato specifikace prezentuje syntaxi C# programovacího jazyka pomocí dvou gramatik. ***Lexikální gramatika*** ([lexikální gramatika](lexical-structure.md#lexical-grammar)) definuje, jakým způsobem jsou znaky Unicode kombinovány při koncích řádků formulářů, prázdných znaků, komentářů, tokenech a direktivách předběžného zpracování. ***Syntaktická gramatika*** ([syntaktická gramatika](lexical-structure.md#syntactic-grammar)) definuje, jak jsou tokeny vyplývající z lexikální gramatiky C# kombinovány do formulářových programů.

### <a name="grammar-notation"></a>Gramatický zápis

Lexikální a syntaktické gramatiky jsou prezentovány ve formě Backus-Naur pomocí zápisu gramatického nástroje ANTLR.

### <a name="lexical-grammar"></a>Gramatika slov

Lexikální C# gramatika je prezentována v [lexikální analýze](lexical-structure.md#lexical-analysis), [tokenech](lexical-structure.md#tokens)a [direktivách předběžného zpracování](lexical-structure.md#pre-processing-directives). Symboly terminálu lexikální gramatiky jsou znaky znakové sady Unicode a lexikální gramatika určuje způsob, jakým jsou kombinovány znaky pro formuláře ([tokeny](lexical-structure.md#tokens)), prázdné znaky ([prázdné](lexical-structure.md#white-space)znaky), komentáře ([Komentáře](lexical-structure.md#comments)) a direktivy předběžného zpracování ([předem zpracovávané direktivy](lexical-structure.md#pre-processing-directives)).

Každý zdrojový soubor v C# programu musí být v souladu se *vstupní* výrobou lexikální gramatiky ([lexikální analýza](lexical-structure.md#lexical-analysis)).

### <a name="syntactic-grammar"></a>Syntaktická gramatika

Syntaktická gramatika C# je prezentována v kapitolách a dodatcích, které následují v této kapitole. Symboly terminálu syntaktické gramatiky jsou tokeny definované lexikální gramatikou a syntaktická gramatika určuje, jak jsou tokeny kombinovány do formulářových C# programů.

Každý zdrojový soubor v C# programu musí splňovat *compilation_unitou* produkci syntaktické gramatiky ([kompilační jednotky](namespaces.md#compilation-units)).

## <a name="lexical-analysis"></a>Lexikální analýza

*Vstupní* výroba definuje lexikální strukturu C# zdrojového souboru. Každý zdrojový soubor v C# programu musí být v souladu s touto lexikální gramatickou výrobou.

```antlr
input
    : input_section?
    ;

input_section
    : input_section_part+
    ;

input_section_part
    : input_element* new_line
    | pp_directive
    ;

input_element
    : whitespace
    | comment
    | token
    ;
```

Pět základních elementů tvoří lexikální strukturu C# zdrojového souboru: Ukončení řádků ([ukončovací](lexical-structure.md#line-terminators)znaky), prázdné znaky ([prázdné](lexical-structure.md#white-space)znaky), komentáře ([Komentáře](lexical-structure.md#comments)), tokeny ([tokeny](lexical-structure.md#tokens)) a direktivy předběžného zpracování ([předběžné zpracování direktiv](lexical-structure.md#pre-processing-directives)). Z těchto základních prvků jsou v syntaktické gramatice C# programu významné jenom tokeny ([syntaktická gramatika](lexical-structure.md#syntactic-grammar)).

Lexikální zpracování C# zdrojového souboru sestává z zmenšení souboru do posloupnosti tokenů, která se do syntaktické analýzy stává vstupem. Ukončení řádků, prázdné znaky a komentáře mohou sloužit k oddělení tokenů a direktivy předběžného zpracování mohou způsobit přeskočení oddílů zdrojového souboru, ale jinak tyto lexikální prvky nemají žádný vliv na syntaktickou strukturu C# programu.

V případě interpolovaná řetězcové literály ([interpolované řetězcové literály](lexical-structure.md#interpolated-string-literals)) je zpočátku jeden token vytvořen lexikální analýzou, ale je rozdělen do několika vstupních elementů, které jsou opakovaně vyčleněny na lexikální analýzu, dokud neproběhne všechny interpoly. byly přeloženy řetězcové literály. Výsledné tokeny pak slouží jako vstup k syntaktické analýze.

V případě, že se několik lexikálních gramatických příčin shoduje se sekvencí znaků ve zdrojovém souboru, lexikální zpracování vždy tvoří nejdelší možný lexikální element. Například sekvence `//` znaků je zpracována jako začátek jednořádkového komentáře, protože tento lexikální prvek je delší než jeden `/` token.

### <a name="line-terminators"></a>Ukončení řádků

Zakončení řádků rozdělí znaky C# zdrojového souboru na řádky.

```antlr
new_line
    : '<Carriage return character (U+000D)>'
    | '<Line feed character (U+000A)>'
    | '<Carriage return character (U+000D) followed by line feed character (U+000A)>'
    | '<Next line character (U+0085)>'
    | '<Line separator character (U+2028)>'
    | '<Paragraph separator character (U+2029)>'
    ;
```

Z důvodu kompatibility s nástroji pro úpravy zdrojového kódu, které přidávají značky konce souboru a umožňují zobrazení zdrojového souboru jako sekvence správně ukončených řádků, jsou v daném pořadí aplikovány následující transformace, a to na každý zdrojový soubor v C# programu:

*  Pokud poslední znak zdrojového souboru je ovládací prvek – znak Z (`U+001A`), tento znak se odstraní.
*  Znak návratu na začátek řádku (`U+000D`) se přidá na konec zdrojového souboru, pokud je tento zdrojový soubor neprázdný a pokud poslední znak zdrojového souboru není znak návratu na začátek řádku (`U+000D`), znak čáry (`U+000A`), oddělovač`U+2028`řádků() nebo oddělovačem odstavce (`U+2029`).

### <a name="comments"></a>Komentáře

Podporovány jsou dvě formy komentářů: Jednořádkový komentář a komentáře s oddělovači. ***Jednořádkový komentář*** začíná znaky `//` a rozšířil na konec řádku zdroje. ***Komentáře s oddělovači*** začínají znaky `/*` a končí znaky `*/`. Komentáře s oddělovači mohou být rozloženy na více řádků.

```antlr
comment
    : single_line_comment
    | delimited_comment
    ;

single_line_comment
    : '//' input_character*
    ;

input_character
    : '<Any Unicode character except a new_line_character>'
    ;

new_line_character
    : '<Carriage return character (U+000D)>'
    | '<Line feed character (U+000A)>'
    | '<Next line character (U+0085)>'
    | '<Line separator character (U+2028)>'
    | '<Paragraph separator character (U+2029)>'
    ;

delimited_comment
    : '/*' delimited_comment_section* asterisk+ '/'
    ;

delimited_comment_section
    : '/'
    | asterisk* not_slash_or_asterisk
    ;

asterisk
    : '*'
    ;

not_slash_or_asterisk
    : '<Any Unicode character except / or *>'
    ;
```

Komentáře nevnořeny. `/*` Sekvence znaků a `*/` nemají `//` `/*` žádné zvláštní významy v rámci komentářeasekvenceznakůanemajížádnýzvláštnívýznamvrámcioddělenýchkomentářů.`//`

Komentáře nejsou zpracovány v rámci znakových a řetězcových literálů.

Příklad
```csharp
/* Hello, world program
   This program writes "hello, world" to the console
*/
class Hello
{
    static void Main() {
        System.Console.WriteLine("hello, world");
    }
}
```
obsahuje oddělený komentář.

Příklad
```csharp
// Hello, world program
// This program writes "hello, world" to the console
//
class Hello // any name will do for this class
{
    static void Main() { // this method must be named "Main"
        System.Console.WriteLine("hello, world");
    }
}
```
zobrazí několik jednořádkových komentářů.

### <a name="white-space"></a>Prázdné znaky

Prázdné znaky jsou definovány jako libovolný znak s třídou Unicode ZS (což zahrnuje znak mezery) a také vodorovný znak tabulátoru, znak svislé čáry a znak kanálu formuláře.

```antlr
whitespace
    : '<Any character with Unicode class Zs>'
    | '<Horizontal tab character (U+0009)>'
    | '<Vertical tab character (U+000B)>'
    | '<Form feed character (U+000C)>'
    ;
```

## <a name="tokens"></a>Tokeny

Existuje několik typů tokenů: identifikátory, klíčová slova, literály, operátory a interpunkční znaky. Prázdné znaky a komentáře nejsou tokeny, i když fungují jako oddělovače pro tokeny.

```antlr
token
    : identifier
    | keyword
    | integer_literal
    | real_literal
    | character_literal
    | string_literal
    | interpolated_string_literal
    | operator_or_punctuator
    ;
```

### <a name="unicode-character-escape-sequences"></a>Řídicí sekvence znaků Unicode

Řídicí sekvence znaků Unicode představuje znak Unicode. Řídicí sekvence znaků Unicode jsou zpracovávány v identifikátorech ([identifikátory](lexical-structure.md#identifiers)), literálech znaků ([literály znaků](lexical-structure.md#character-literals)) a regulárních řetězcových literálů ([řetězcové literály](lexical-structure.md#string-literals)). Řídicí znak Unicode se nezpracovává na žádném jiném místě (například pro vytvoření operátoru, punctuator nebo klíčového slova).

```antlr
unicode_escape_sequence
    : '\\u' hex_digit hex_digit hex_digit hex_digit
    | '\\U' hex_digit hex_digit hex_digit hex_digit hex_digit hex_digit hex_digit hex_digit
    ;
```

Řídicí sekvence Unicode představuje jeden znak Unicode tvořený šestnáctkovým číslem za`\u`znaky`\U`nebo. Vzhledem C# k tomu, že používá 16bitové kódování bodů kódu Unicode do znaků a hodnot řetězce, znak Unicode v rozsahu U + 10000 až U + 10FFFF není povolený ve znakovém literálu a je reprezentován pomocí náhradní dvojice Unicode v řetězcovém literálu. Znaky Unicode s body kódu nad 0x10FFFF nejsou podporovány.

Vícenásobné překlady se neprovádí. Například řetězcový literál "`\u005Cu005C`" je ekvivalentem "`\u005C`", nikoli "`\`". Hodnota `\u005C` Unicode je`\`znak "".

Příklad
```csharp
class Class1
{
    static void Test(bool \u0066) {
        char c = '\u0066';
        if (\u0066)
            System.Console.WriteLine(c.ToString());
    }        
}
```
ukazuje několik použití `\u0066`, což je řídicí sekvence pro`f`písmeno "". Program je ekvivalentní
```csharp
class Class1
{
    static void Test(bool f) {
        char c = 'f';
        if (f)
            System.Console.WriteLine(c.ToString());
    }        
}
```

### <a name="identifiers"></a>Identifikátory

Pravidla pro identifikátory uvedené v této části odpovídají přesně těch, které doporučila příloha Standard Unicode Standard, s tím rozdílem, že podtržítko je povolené jako počáteční znak (jako tradiční v programovacím jazyce C), řídicí sekvence Unicode jsou povoluje se v identifikátorech a znak`@`"" je povolen jako předpona, aby bylo možné používat klíčová slova jako identifikátory.

```antlr
identifier
    : available_identifier
    | '@' identifier_or_keyword
    ;

available_identifier
    : '<An identifier_or_keyword that is not a keyword>'
    ;

identifier_or_keyword
    : identifier_start_character identifier_part_character*
    ;

identifier_start_character
    : letter_character
    | '_'
    ;

identifier_part_character
    : letter_character
    | decimal_digit_character
    | connecting_character
    | combining_character
    | formatting_character
    ;

letter_character
    : '<A Unicode character of classes Lu, Ll, Lt, Lm, Lo, or Nl>'
    | '<A unicode_escape_sequence representing a character of classes Lu, Ll, Lt, Lm, Lo, or Nl>'
    ;

combining_character
    : '<A Unicode character of classes Mn or Mc>'
    | '<A unicode_escape_sequence representing a character of classes Mn or Mc>'
    ;

decimal_digit_character
    : '<A Unicode character of the class Nd>'
    | '<A unicode_escape_sequence representing a character of the class Nd>'
    ;

connecting_character
    : '<A Unicode character of the class Pc>'
    | '<A unicode_escape_sequence representing a character of the class Pc>'
    ;

formatting_character
    : '<A Unicode character of the class Cf>'
    | '<A unicode_escape_sequence representing a character of the class Cf>'
    ;
```

Informace o třídách znaků Unicode uvedených výše najdete v části Standard Unicode verze 3,0, část 4,5.

Příklady platných identifikátorů:`identifier1`"", "`_identifier2`" a "`@if`".

Identifikátor v koformovém programu musí být v kanonickém formátu definovaném formulářem normalizace Unicode C definovaným standardem Unicode Standard přílohy 15. Chování při zjištění identifikátoru, který není v normalizačním tvaru C je definováno implementací; Diagnostika ale není nutná.

Předpona "`@`" umožňuje používat klíčová slova jako identifikátory, což je užitečné při navzájemení s jinými programovacími jazyky. Znak `@` není ve skutečnosti součástí identifikátoru, proto se identifikátor může zobrazit v jiných jazycích jako běžný identifikátor bez předpony. Identifikátor s `@` předponou se nazývá ***doslovné identifikátor***. `@` Použití předpony pro identifikátory, které nejsou klíčovými slovy, je povoleno, ale důrazně nedoporučujeme ve stylu.

Příklad:
```csharp
class @class
{
    public static void @static(bool @bool) {
        if (@bool)
            System.Console.WriteLine("true");
        else
            System.Console.WriteLine("false");
    }    
}

class Class1
{
    static void M() {
        cl\u0061ss.st\u0061tic(true);
    }
}
```
definuje třídu s názvem`class`"" se statickou metodou nazvanou "`static`", která`bool`přebírá parametr s názvem "". Všimněte si, že protože řídicí znaky Unicode nejsou v klíčových slovech povoleny,`cl\u0061ss`token "" je identifikátor a je stejný identifikátor jako "`@class`".

Dva identifikátory jsou považovány za stejné, pokud jsou stejné po použití následujících transformací, v uvedeném pořadí:

*  Předpona "`@`", pokud je použita, je odebrána.
*  Každý *unicode_escape_sequence* se transformuje na příslušný znak Unicode.
*  Všechny *formatting_character*se odeberou.

Identifikátory, které obsahují dvě po sobě jdoucí podtržítka (`U+005F`), jsou vyhrazeny pro použití implementací. Například implementace může poskytovat rozšířená klíčová slova, která začínají dvěma podtržítky.

### <a name="keywords"></a>klíčová slova

***Klíčové slovo*** je posloupnost znaků, které jsou rezervovány, a nelze jej použít jako identifikátor s výjimkou, kdy je `@` použit znak.

```antlr
keyword
    : 'abstract' | 'as'       | 'base'       | 'bool'      | 'break'
    | 'byte'     | 'case'     | 'catch'      | 'char'      | 'checked'
    | 'class'    | 'const'    | 'continue'   | 'decimal'   | 'default'
    | 'delegate' | 'do'       | 'double'     | 'else'      | 'enum'
    | 'event'    | 'explicit' | 'extern'     | 'false'     | 'finally'
    | 'fixed'    | 'float'    | 'for'        | 'foreach'   | 'goto'
    | 'if'       | 'implicit' | 'in'         | 'int'       | 'interface'
    | 'internal' | 'is'       | 'lock'       | 'long'      | 'namespace'
    | 'new'      | 'null'     | 'object'     | 'operator'  | 'out'
    | 'override' | 'params'   | 'private'    | 'protected' | 'public'
    | 'readonly' | 'ref'      | 'return'     | 'sbyte'     | 'sealed'
    | 'short'    | 'sizeof'   | 'stackalloc' | 'static'    | 'string'
    | 'struct'   | 'switch'   | 'this'       | 'throw'     | 'true'
    | 'try'      | 'typeof'   | 'uint'       | 'ulong'     | 'unchecked'
    | 'unsafe'   | 'ushort'   | 'using'      | 'virtual'   | 'void'
    | 'volatile' | 'while'
    ;
```

Na některých místech gramatiky mají konkrétní identifikátory zvláštní význam, ale nejedná se o klíčová slova. Takové identifikátory se někdy označují jako "kontextová klíčová slova". Například v rámci deklarace vlastnosti mají identifikátory "`get`" a "`set`" speciální význam ([přistupující objekty](classes.md#accessors)). Identifikátor jiný než `get` nebo `set` není nikdy povolen v těchto umístěních, takže toto použití není v konfliktu s použitím těchto slov jako identifikátorů. V jiných případech, například s identifikátorem "`var`" v implicitním typu deklarace lokální proměnné ([místní proměnné deklarace](statements.md#local-variable-declarations)), klíčové slovo kontextové může být v konfliktu s deklarovanými názvy. V takových případech má deklarovaný název přednost před použitím identifikátoru jako klíčového slova kontextové.

### <a name="literals"></a>Literály

***Literál*** je reprezentace hodnoty ve zdrojovém kódu.

```antlr
literal
    : boolean_literal
    | integer_literal
    | real_literal
    | character_literal
    | string_literal
    | null_literal
    ;
```

#### <a name="boolean-literals"></a>Logické literály

Existují dvě logické hodnoty literálu: `true` a `false`.

```antlr
boolean_literal
    : 'true'
    | 'false'
    ;
```

Typ *boolean_literal* je `bool`.

#### <a name="integer-literals"></a>Celočíselné literály

Celočíselné literály se používají pro zápis hodnot typů `int`, `uint`, `long`a `ulong`. Celočíselné literály mají dvě možné formy: Decimal a hexadecimální.

```antlr
integer_literal
    : decimal_integer_literal
    | hexadecimal_integer_literal
    ;

decimal_integer_literal
    : decimal_digit+ integer_type_suffix?
    ;

decimal_digit
    : '0' | '1' | '2' | '3' | '4' | '5' | '6' | '7' | '8' | '9'
    ;

integer_type_suffix
    : 'U' | 'u' | 'L' | 'l' | 'UL' | 'Ul' | 'uL' | 'ul' | 'LU' | 'Lu' | 'lU' | 'lu'
    ;

hexadecimal_integer_literal
    : '0x' hex_digit+ integer_type_suffix?
    | '0X' hex_digit+ integer_type_suffix?
    ;

hex_digit
    : '0' | '1' | '2' | '3' | '4' | '5' | '6' | '7' | '8' | '9'
    | 'A' | 'B' | 'C' | 'D' | 'E' | 'F' | 'a' | 'b' | 'c' | 'd' | 'e' | 'f';
```

Typ celočíselného literálu je určen následujícím způsobem:

*  Pokud literál nemá žádnou příponu, má první z těchto typů, ve kterém lze jeho `int`hodnotu reprezentovat:, `uint`, `long`, `ulong`.
*  Pokud je `U` literál namapován pomocí nebo `u`, má první z těchto typů, ve kterém může být jeho hodnota reprezentovaná: `uint`, `ulong`.
*  Pokud je `L` literál namapován pomocí nebo `l`, má první z těchto typů, ve kterém může být jeho hodnota reprezentovaná: `long`, `ulong`.
*  Pokud je `UL`literál s příponou, `LU` `ul` `Ul`, `uL`,,, `Lu`, `lU`nebo `ulong`, jetypu.`lu`

Pokud hodnota reprezentovaná celočíselným literálem je mimo rozsah `ulong` typu, dojde k chybě při kompilaci.

V ohledu na styl je`L`navržena možnost "" použít místo "`l`" při zápisu literálů typu `long`, protože je snadné Zaměňujte písmeno`l`"" s číslicí "`1`".

Chcete-li povolit, `int` aby `long` byly nejmenší možné hodnoty a hodnoty zapsány jako desítkové celočíselné literály, existují následující dvě pravidla:

* Pokud se *decimal_integer_literal* s hodnotou 2147483648 (2 ^ 31) a žádné *integer_type_suffix* se nezobrazí jako token bezprostředně po unárním tokenu minus operátor ([unární operátor mínus](expressions.md#unary-minus-operator)), výsledkem je konstanta typu `int`. s hodnotou-2147483648 (-2 ^ 31). Ve všech ostatních situacích taková *decimal_integer_literal* je typu `uint`.
* Pokud se *decimal_integer_literal* s hodnotou 9223372036854775808 (2 ^ 63) a No *integer_type_suffix* nebo *integer_type_suffix* `L` nebo `l`, zobrazí se jako token bezprostředně po unárním tokenu minus operátor ([ Unární operátor minus](expressions.md#unary-minus-operator): Výsledkem je konstanta typu `long` s hodnotou-9223372036854775808 (-2 ^ 63). Ve všech ostatních situacích taková *decimal_integer_literal* je typu `ulong`.

#### <a name="real-literals"></a>Reálné literály

Reálné literály se používají pro zápis hodnot typů `float`, `double`a `decimal`.

```antlr
real_literal
    : decimal_digit+ '.' decimal_digit+ exponent_part? real_type_suffix?
    | '.' decimal_digit+ exponent_part? real_type_suffix?
    | decimal_digit+ exponent_part real_type_suffix?
    | decimal_digit+ real_type_suffix
    ;

exponent_part
    : 'e' sign? decimal_digit+
    | 'E' sign? decimal_digit+
    ;

sign
    : '+'
    | '-'
    ;

real_type_suffix
    : 'F' | 'f' | 'D' | 'd' | 'M' | 'm'
    ;
```

Pokud není zadán žádný *real_type_suffix* , typ reálného literálu je `double`. Jinak přípona reálného typu Určuje typ reálného literálu následujícím způsobem:

*  Skutečný literál s příponou `F` nebo `f` je typu `float`. Například literály `1f`, `1.5f`, `1e10f`a `123.456F` jsou všechny typu `float`.
*  Skutečný literál s příponou `D` nebo `d` je typu `double`. Například literály `1d`, `1.5d`, `1e10d`a `123.456D` jsou všechny typu `double`.
*  Skutečný literál s příponou `M` nebo `m` je typu `decimal`. Například literály `1m`, `1.5m`, `1e10m`a `123.456M` jsou všechny typu `decimal`. Tento literál je převeden na `decimal` hodnotu tím, že převezme přesnou hodnotu a v případě potřeby se zaokrouhluje na nejbližší reprezentovatelné hodnoty pomocí zaokrouhlování bank ([desítkový typ](types.md#the-decimal-type)). Jakékoli patrné zjevné v literálu se zachová, pokud hodnota není zaokrouhlená nebo je hodnota nulová (v opačném případě bude znaménko a měřítko 0). Proto se literál `2.900m` bude analyzovat za účelem vytvoření desetinné čárky pomocí znaménka `0`, koeficientu `2900`a stupnice `3`.

Pokud zadaný literál nemůže být reprezentovaný v zadaném typu, dojde k chybě při kompilaci.

Hodnota reálného literálu typu `float` nebo `double` je určena pomocí režimu zaokrouhlení IEEE na nejbližší.

Všimněte si, že v reálném literálu jsou desítkové číslice vždy požadovány za desetinnou čárkou. Například je skutečný `1.3F` literál, ale `1.F` není.

#### <a name="character-literals"></a>Literály znaků

Znakový literál představuje jeden znak a obvykle se skládá ze znaku v uvozovkách, jako v `'a'`.

Poznámka: Gramatický zápis ANTLR způsobuje následující matoucí! Při psaní `\'` v ANTLR se pro jednu uvozovku `'`postupuje. A při psaní `\\` se zaznamená jedno zpětné lomítko `\`. Proto první pravidlo pro znakový literál znamená, že začíná jednoduchou uvozovkou, pak znakem, pak jednoduchou uvozovku. A jedenáct možných jednoduchých řídicích sekvencí `\'`jsou `\"`, `\\`, `\0` `\a`,, `\b`, `\f`, `\n`, `\r`, ,`\t` ,`\v`.

```antlr
character_literal
    : '\'' character '\''
    ;

character
    : single_character
    | simple_escape_sequence
    | hexadecimal_escape_sequence
    | unicode_escape_sequence
    ;

single_character
    : '<Any character except \' (U+0027), \\ (U+005C), and new_line_character>'
    ;

simple_escape_sequence
    : '\\\'' | '\\"' | '\\\\' | '\\0' | '\\a' | '\\b' | '\\f' | '\\n' | '\\r' | '\\t' | '\\v'
    ;

hexadecimal_escape_sequence
    : '\\x' hex_digit hex_digit? hex_digit? hex_digit?;
```

Znak, který následuje znak zpětného lomítka`\`() ve *znaku* , musí být jedním z následujících znaků: `'`, `"`, `\`, `0`, `a`, `b`, `f` , `n`, `r`, `t`, `u`, `U`, `x`, `v`. V opačném případě dojde k chybě při kompilaci.

Šestnáctková řídicí sekvence představuje jeden znak Unicode s hodnotou vytvořenou hexadecimálním číslem za`\x`"".

Pokud je hodnota reprezentovaná znakovým literálem větší než `U+FFFF`, dojde k chybě při kompilaci.

Řídicí sekvence znaků Unicode ([řídicí sekvence znaků Unicode](lexical-structure.md#unicode-character-escape-sequences)) ve znakovém literálu musí být v rozsahu `U+0000` až. `U+FFFF`

Jednoduchá řídicí sekvence představuje kódování znaků Unicode, jak je popsáno v následující tabulce.


| __Řídicí sekvence__ | __Název znaku__ | __Kódování Unicode__ |
|---------------------|--------------------|----------------------|
| `\'`                | Jednoduchá uvozovka       | `0x0027`             | 
| `\"`                | Dvojité uvozovky       | `0x0022`             | 
| `\\`                | Zpětné lomítko          | `0x005C`             | 
| `\0`                | Null               | `0x0000`             | 
| `\a`                | Výstrahy              | `0x0007`             | 
| `\b`                | Backspace          | `0x0008`             | 
| `\f`                | Informační kanál formuláře          | `0x000C`             | 
| `\n`                | Nový řádek           | `0x000A`             | 
| `\r`                | Návrat na začátek řádku    | `0x000D`             | 
| `\t`                | Horizontální tabulátor     | `0x0009`             | 
| `\v`                | Vertikální tabulátor       | `0x000B`             | 

Typ *character_literal* je `char`.

#### <a name="string-literals"></a>Řetězcové literály

C#podporuje dva formy řetězcových literálů: ***regulární řetězcové literály*** a ***doslovné řetězce literálů***.

Regulární řetězcový literál se skládá z nula nebo více znaků uzavřených v dvojitých uvozovkách, `"hello"`jako v a může zahrnovat jednoduché řídicí sekvence ( `\t` například pro znak tabulátoru) a hexadecimální sekvence znaků a kódování Unicode.

Doslovné řetězcový literál se skládá ze `@` znaku následovaného znakem dvojité uvozovky, nula nebo více znaků a uzavíracího znaku dvojité uvozovky. Jednoduchý příklad je `@"hello"`. V doslovném řetězcovém literálu jsou znaky mezi oddělovači interpretovány doslovné, jedinou výjimkou je *quote_escape_sequence*. Zejména jednoduché řídicí sekvence a šestnáctkové a znakové sekvence Unicode nejsou zpracovány v doslovném řetězci literálů. Doslovné řetězcový literál může zahrnovat více řádků.

```antlr
string_literal
    : regular_string_literal
    | verbatim_string_literal
    ;

regular_string_literal
    : '"' regular_string_literal_character* '"'
    ;

regular_string_literal_character
    : single_regular_string_literal_character
    | simple_escape_sequence
    | hexadecimal_escape_sequence
    | unicode_escape_sequence
    ;

single_regular_string_literal_character
    : '<Any character except " (U+0022), \\ (U+005C), and new_line_character>'
    ;

verbatim_string_literal
    : '@"' verbatim_string_literal_character* '"'
    ;

verbatim_string_literal_character
    : single_verbatim_string_literal_character
    | quote_escape_sequence
    ;

single_verbatim_string_literal_character
    : '<any character except ">'
    ;

quote_escape_sequence
    : '""'
    ;
```

Znak, který následuje znak zpětného lomítka (`\`) ve *regular_string_literal_character* , musí být jedním z následujících znaků: `'`, `"`, `\`, `0`, `a`, `b`, `f`, `n`, 0, 1 @no__ t-12, 3, 4 5. V opačném případě dojde k chybě při kompilaci.

Příklad
```csharp
string a = "hello, world";                   // hello, world
string b = @"hello, world";                  // hello, world

string c = "hello \t world";                 // hello      world
string d = @"hello \t world";                // hello \t world

string e = "Joe said \"Hello\" to me";       // Joe said "Hello" to me
string f = @"Joe said ""Hello"" to me";      // Joe said "Hello" to me

string g = "\\\\server\\share\\file.txt";    // \\server\share\file.txt
string h = @"\\server\share\file.txt";       // \\server\share\file.txt

string i = "one\r\ntwo\r\nthree";
string j = @"one
two
three";
```
zobrazuje různé řetězcové literály. Poslední řetězcový literál, `j`je doslovné textový literál, který pokrývá více řádků. Znaky mezi uvozovkami, včetně prázdných znaků, jako jsou znaky nového řádku, jsou zachovány ve znění.

Vzhledem k tomu, že hexadecimální řídicí sekvence může mít proměnlivý počet šestnáctkových číslic, `"\x123"` řetězcový literál obsahuje jeden znak s šestnáctkovou hodnotou 123. Chcete-li vytvořit řetězec, který obsahuje znak s šestnáctkovou hodnotou 12 následovaný znakem 3, může `"\x00123"` jeden `"\x12" + "3"` napsat nebo místo něj.

Typ *string_literal* je `string`.

Každý řetězcový literál nemusí nutně mít za následek novou instanci řetězce. V případě, že dva nebo více řetězcových literálů, které jsou ekvivalentní vzhledem k operátoru rovnosti řetězců ([operátory rovnosti řetězců](expressions.md#string-equality-operators)), se nachází ve stejném programu, tyto řetězcové literály odkazují na stejnou instanci řetězce. Například výstup vytvářený
```csharp
class Test
{
    static void Main() {
        object a = "hello";
        object b = "hello";
        System.Console.WriteLine(a == b);
    }
}
```
je `True` to proto, že dva literály odkazují na stejnou instanci řetězce.

#### <a name="interpolated-string-literals"></a>Interpolované řetězcové literály

Interpolované řetězcové literály jsou podobné řetězcovým literálům, ale obsahují otvory oddělené `{` znakem `}`a, mohou nastat výrazy. V době běhu jsou výrazy vyhodnocovány s účelem použití jejich textových formulářů, které jsou nahrazeny řetězcem na místě, kde se objeví otvor. Syntaxe a sémantika řetězcové interpolace je popsána v části ([interpolované řetězce](expressions.md#interpolated-strings)).

Podobně jako řetězcové literály mohou být interpolované řetězcové literály buď pravidelné, nebo doslovné. Interpolované regulární řetězcové literály jsou odděleny `$"` a `"`a interpolované řetězcové literály jsou odděleny hodnotami `$@"` a `"`.

Podobně jako jiné literály, lexikální analýza interpolované řetězcové literály, zpočátku vede k jednomu tokenu, jak je uvedeno níže. Nicméně před syntaktickou analýzou je jeden token interpolované řetězcové literály rozdělen do několika tokenů pro části řetězce ohraničujících otvory a vstupní prvky, ke kterým dochází v otvorech, se znovu analyzují. To může mít za následek zpracování více interpolovaná řetězcové literály, ale v případě, že je lexikální správnost, nakonec vede k sekvenci tokenů pro syntaktickou analýzu, která bude zpracována.

```antlr
interpolated_string_literal
    : '$' interpolated_regular_string_literal
    | '$' interpolated_verbatim_string_literal
    ;

interpolated_regular_string_literal
    : interpolated_regular_string_whole
    | interpolated_regular_string_start  interpolated_regular_string_literal_body interpolated_regular_string_end
    ;

interpolated_regular_string_literal_body
    : regular_balanced_text
    | interpolated_regular_string_literal_body interpolated_regular_string_mid regular_balanced_text
    ;

interpolated_regular_string_whole
    : '"' interpolated_regular_string_character* '"'
    ;

interpolated_regular_string_start
    : '"' interpolated_regular_string_character* '{'
    ;

interpolated_regular_string_mid
    : interpolation_format? '}' interpolated_regular_string_characters_after_brace? '{'
    ;

interpolated_regular_string_end
    : interpolation_format? '}' interpolated_regular_string_characters_after_brace? '"'
    ;

interpolated_regular_string_characters_after_brace
    : interpolated_regular_string_character_no_brace
    | interpolated_regular_string_characters_after_brace interpolated_regular_string_character
    ;

interpolated_regular_string_character
    : single_interpolated_regular_string_character
    | simple_escape_sequence
    | hexadecimal_escape_sequence
    | unicode_escape_sequence
    | open_brace_escape_sequence
    | close_brace_escape_sequence
    ;

interpolated_regular_string_character_no_brace
    : '<Any interpolated_regular_string_character except close_brace_escape_sequence and any hexadecimal_escape_sequence or unicode_escape_sequence designating } (U+007D)>'
    ;

single_interpolated_regular_string_character
    : '<Any character except \" (U+0022), \\ (U+005C), { (U+007B), } (U+007D), and new_line_character>'
    ;

open_brace_escape_sequence
    : '{{'
    ;

close_brace_escape_sequence
    : '}}'
    ;
    
regular_balanced_text
    : regular_balanced_text_part+
    ;

regular_balanced_text_part
    : single_regular_balanced_text_character
    | delimited_comment
    | '@' identifier_or_keyword
    | string_literal
    | interpolated_string_literal
    | '(' regular_balanced_text ')'
    | '[' regular_balanced_text ']'
    | '{' regular_balanced_text '}'
    ;
    
single_regular_balanced_text_character
    : '<Any character except / (U+002F), @ (U+0040), \" (U+0022), $ (U+0024), ( (U+0028), ) (U+0029), [ (U+005B), ] (U+005D), { (U+007B), } (U+007D) and new_line_character>'
    | '</ (U+002F), if not directly followed by / (U+002F) or * (U+002A)>'
    ;
    
interpolation_format
    : interpolation_format_character+
    ;
    
interpolation_format_character
    : '<Any character except \" (U+0022), : (U+003A), { (U+007B) and } (U+007D)>'
    ;
    
interpolated_verbatim_string_literal
    : interpolated_verbatim_string_whole
    | interpolated_verbatim_string_start interpolated_verbatim_string_literal_body interpolated_verbatim_string_end
    ;

interpolated_verbatim_string_literal_body
    : verbatim_balanced_text
    | interpolated_verbatim_string_literal_body interpolated_verbatim_string_mid verbatim_balanced_text
    ;
    
interpolated_verbatim_string_whole
    : '@"' interpolated_verbatim_string_character* '"'
    ;
    
interpolated_verbatim_string_start
    : '@"' interpolated_verbatim_string_character* '{'
    ;
    
interpolated_verbatim_string_mid
    : interpolation_format? '}' interpolated_verbatim_string_characters_after_brace? '{'
    ;
    
interpolated_verbatim_string_end
    : interpolation_format? '}' interpolated_verbatim_string_characters_after_brace? '"'
    ;
    
interpolated_verbatim_string_characters_after_brace
    : interpolated_verbatim_string_character_no_brace
    | interpolated_verbatim_string_characters_after_brace interpolated_verbatim_string_character
    ;
    
interpolated_verbatim_string_character
    : single_interpolated_verbatim_string_character
    | quote_escape_sequence
    | open_brace_escape_sequence
    | close_brace_escape_sequence
    ;
    
interpolated_verbatim_string_character_no_brace
    : '<Any interpolated_verbatim_string_character except close_brace_escape_sequence>'
    ;
    
single_interpolated_verbatim_string_character
    : '<Any character except \" (U+0022), { (U+007B) and } (U+007D)>'
    ;
    
verbatim_balanced_text
    : verbatim_balanced_text_part+
    ;

verbatim_balanced_text_part
    : single_verbatim_balanced_text_character
    | comment
    | '@' identifier_or_keyword
    | string_literal
    | interpolated_string_literal
    | '(' verbatim_balanced_text ')'
    | '[' verbatim_balanced_text ']'
    | '{' verbatim_balanced_text '}'
    ;
    
single_verbatim_balanced_text_character
    : '<Any character except / (U+002F), @ (U+0040), \" (U+0022), $ (U+0024), ( (U+0028), ) (U+0029), [ (U+005B), ] (U+005D), { (U+007B) and } (U+007D)>'
    | '</ (U+002F), if not directly followed by / (U+002F) or * (U+002A)>'
    ;
```

*Interpolated_string_literal* token se překládá jako více tokenů a dalších vstupních elementů následujícím způsobem, v pořadí podle výskytu v *interpolated_string_literal*:

* Výskyty následujících jsou překládány jako samostatné jednotlivé tokeny: přední `$` Sign, *interpolated_regular_string_whole*, *interpolated_regular_string_start*, *interpolated_regular_string_mid*,  *interpolated_regular_string_end*, *interpolated_verbatim_string_whole*, *interpolated_verbatim_string_start*, *interpolated_verbatim_string_mid* a *interpolated_verbatim_string_end*.
* Výskyty hodnot *regular_balanced_text* a *verbatim_balanced_text* mezi nimi se znovu zpracovávají jako *input_section* ([lexikální analýza](lexical-structure.md#lexical-analysis)) a jsou překládány jako výsledná sekvence vstupních prvků. Ty můžou zase zahrnovat interpolované řetězcové literály tokeny, které se mají přeinterpretovat.

Syntaktická analýza znovu sloučí tokeny do *interpolated_string_expression* ([interpolované řetězce](expressions.md#interpolated-strings)).

Příklady TODO


#### <a name="the-null-literal"></a>Literál s hodnotou null

```antlr
null_literal
    : 'null'
    ;
```

*Null_literal* se dá implicitně převést na typ odkazu nebo na typ s možnou hodnotou null.

### <a name="operators-and-punctuators"></a>Operátory a interpunkční znaky

Existuje několik druhů operátorů a interpunkční znaky. Operátory jsou používány ve výrazech k popisu operací, které zahrnují jeden nebo více operandů. Například výraz `a + b` `a` používá operátor k přidání dvou operandů a `b`. `+` Interpunkční znaky jsou pro seskupování a oddělování.

```antlr
operator_or_punctuator
    : '{'  | '}'  | '['  | ']'  | '('   | ')'  | '.'  | ','  | ':'  | ';'
    | '+'  | '-'  | '*'  | '/'  | '%'   | '&'  | '|'  | '^'  | '!'  | '~'
    | '='  | '<'  | '>'  | '?'  | '??'  | '::' | '++' | '--' | '&&' | '||'
    | '->' | '==' | '!=' | '<=' | '>='  | '+=' | '-=' | '*=' | '/=' | '%='
    | '&=' | '|=' | '^=' | '<<' | '<<=' | '=>'
    ;

right_shift
    : '>>'
    ;

right_shift_assignment
    : '>>='
    ;
```

Svislá čára v rámci *right_shift* a *right_shift_assignment* se používá k označení toho, že na rozdíl od jiných výrob v syntaktické gramatice nejsou mezi tokeny povoleny žádné znaky libovolného typu (ne ani prázdné znaky). Tyto výroby jsou speciálně ošetřeny, aby bylo možné povolit správné zpracování *type_parameter_list*s ([parametry typu](classes.md#type-parameters)).

## <a name="pre-processing-directives"></a>Direktivy předběžného zpracování

Direktivy předběžného zpracování poskytují možnost podmíněně přeskočit oddíly zdrojových souborů, hlásit chybové a varovné podmínky a rozlišit různé oblasti zdrojového kódu. Termín "direktivy předběžného zpracování" se používá pouze pro konzistenci s programovacími jazyky jazyka C a C++ . V C#není k dispozici žádný samostatný krok před zpracováním; direktivy předběžného zpracování jsou zpracovávány jako součást lexikální analytické fáze.

```antlr
pp_directive
    : pp_declaration
    | pp_conditional
    | pp_line
    | pp_diagnostic
    | pp_region
    | pp_pragma
    ;
```

K dispozici jsou následující direktivy předběžného zpracování:

*  `#define`a `#undef`, které jsou použity k definování a zrušení definice, v uvedeném pořadí, symboly podmíněné kompilace ([deklarace direktiv](lexical-structure.md#declaration-directives)).
*  `#if`, `#elif`, `#else`, a`#endif`, které jsou použity k podmíněnému přeskočení sekcí zdrojového kódu ([direktivy podmíněné kompilace](lexical-structure.md#conditional-compilation-directives)).
*  `#line`, který se používá k řízení čísel řádků vygenerovaných pro chyby a upozornění ([direktivy řádku](lexical-structure.md#line-directives)).
*  `#error`a `#warning`, které se používají k vydávání chyb a upozornění, v uvedeném pořadí ([diagnostické direktivy](lexical-structure.md#diagnostic-directives)).
*  `#region`a `#endregion`, které se používají k explicitnímu označení oddílů zdrojového kódu ([direktivy oblasti](lexical-structure.md#region-directives)).
*  `#pragma`, který se používá k určení volitelných kontextových informací kompilátoru ([direktivy pragma](lexical-structure.md#pragma-directives)).

Direktiva před zpracováním vždy zabírá samostatný řádek zdrojového kódu a vždy začíná `#` znakem a názvem direktivy před zpracováním. Před `#` znakem a `#` mezi znakem a názvem direktivy může být mezera.

Zdrojový řádek obsahující `#define`direktivu, `#undef`, `#if` `#else` `#endif`,,, ,`#line`nebo může`#endregion` končit jedním řádkem komentáře. `#elif` Znaky s oddělovači ( `/* */` styl komentářů) nejsou povoleny pro zdrojové řádky obsahující direktivy předběžného zpracování.

Direktivy předběžného zpracování nejsou tokeny a nejsou součástí syntaktické gramatiky C#. Direktivy předběžného zpracování však lze použít k zahrnutí nebo vyloučení sekvencí tokenů a může tak v takovém smyslu ovlivnit význam C# programu. Například při kompilování programu:
```csharp
#define A
#undef B

class C
{
#if A
    void F() {}
#else
    void G() {}
#endif

#if B
    void H() {}
#else
    void I() {}
#endif
}
```
má za následek přesně stejnou sekvenci tokenů jako program:
```csharp
class C
{
    void F() {}
    void I() {}
}
```

Vzhledem k tomu, že tyto dva programy jsou proto v lexikálním rozdílovém, syntakticky stejné.

### <a name="conditional-compilation-symbols"></a>Symboly podmíněné kompilace

Funkce `#if`podmíněné kompilace poskytované `#endif` direktivami, `#elif`, `#else`a jsou řízeny prostřednictvím předzpracování výrazů ([předzpracování výrazů](lexical-structure.md#pre-processing-expressions)) a podmíněné symboly kompilace.

```antlr
conditional_symbol
    : '<Any identifier_or_keyword except true or false>'
    ;
```

Symbol podmíněné kompilace má dva možné stavy: ***definováno*** nebo ***undefined***. Na začátku lexikálního zpracování zdrojového souboru je nedefinovaný symbol podmíněné kompilace, pokud nebyl explicitně definován externím mechanismem (například možnost kompilátoru příkazového řádku). Když je zpracována direktiva, symbol podmíněné kompilace s názvem v této direktivě bude definován v tomto zdrojovém souboru. `#define` Symbol zůstane definován, dokud `#undef` není zpracována direktiva pro stejný symbol, nebo dokud není dosaženo konce zdrojového souboru. Nejedná se o to, že `#define` direktivy a `#undef` v jednom zdrojovém souboru nemají žádný vliv na jiné zdrojové soubory ve stejném programu.

V případě, že je odkazováno ve výrazu před zpracováním, má definovaný podmíněný symbol kompilace `true`logickou hodnotu a nedefinovaný symbol podmíněné kompilace má logickou `false`hodnotu. Neexistuje žádný požadavek, aby symboly podmíněné kompilace byly explicitně deklarovány předtím, než jsou odkazovány v předzpracování výrazů. Místo toho jsou nedeklarované symboly jednoduše nedefinovány a mají tedy hodnotu `false`.

Obor názvů pro symboly podmíněné kompilace je jedinečný a oddělený od všech ostatních pojmenovaných entit C# v programu. Na symboly podmíněné kompilace lze odkazovat pouze v `#define` direktivách and `#undef` a ve výrazech předběžného zpracování.

### <a name="pre-processing-expressions"></a>Výrazy předběžného zpracování

Výrazy předběžného zpracování se můžou `#if` vyskytovat `#elif` v direktivách a. Operátory `!`, `==`, ajsou`!=`povolenyve výrazech předběžného zpracování a závorky lze použít k seskupení. `||` `&&`

```antlr
pp_expression
    : whitespace? pp_or_expression whitespace?
    ;

pp_or_expression
    : pp_and_expression
    | pp_or_expression whitespace? '||' whitespace? pp_and_expression
    ;

pp_and_expression
    : pp_equality_expression
    | pp_and_expression whitespace? '&&' whitespace? pp_equality_expression
    ;

pp_equality_expression
    : pp_unary_expression
    | pp_equality_expression whitespace? '==' whitespace? pp_unary_expression
    | pp_equality_expression whitespace? '!=' whitespace? pp_unary_expression
    ;

pp_unary_expression
    : pp_primary_expression
    | '!' whitespace? pp_unary_expression
    ;

pp_primary_expression
    : 'true'
    | 'false'
    | conditional_symbol
    | '(' whitespace? pp_expression whitespace? ')'
    ;
```

V případě, že je odkazováno ve výrazu před zpracováním, má definovaný podmíněný symbol kompilace `true`logickou hodnotu a nedefinovaný symbol podmíněné kompilace má logickou `false`hodnotu.

Vyhodnocení předzpracovávaného výrazu vždy vrací logickou hodnotu. Pravidla vyhodnocení pro předzpracování výrazu jsou stejná jako u konstantního výrazu ([konstantní výrazy](expressions.md#constant-expressions)), s tím rozdílem, že pouze uživatelsky definované entity, na které lze odkazovat, jsou podmíněné symboly kompilace.

### <a name="declaration-directives"></a>Direktivy deklarace

Direktivy deklarace slouží k definování nebo zrušení definování symbolů podmíněné kompilace.

```antlr
pp_declaration
    : whitespace? '#' whitespace? 'define' whitespace conditional_symbol pp_new_line
    | whitespace? '#' whitespace? 'undef' whitespace conditional_symbol pp_new_line
    ;

pp_new_line
    : whitespace? single_line_comment? new_line
    ;
```

Zpracování `#define` direktiv způsobí, že daný symbol podmíněné kompilace bude definován, počínaje zdrojovým řádkem, který následuje po direktivě. Podobně zpracování `#undef` direktiv způsobí, že se daný symbol podmíněné kompilace stane nedefinovaným, počínaje zdrojovým řádkem, který následuje po direktivě.

Všechny `#define` direktivy ve zdrojovém souboru musí nastat před prvním tokenem ([tokeny](lexical-structure.md#tokens)) ve zdrojovém souboru. v opačném případě dojde k chybě při kompilaci. `#undef` V intuitivních `#define` výrazech `#undef` a direktivy musí předcházet libovolnému "skutečnému kódu" ve zdrojovém souboru.

Příklad:
```csharp
#define Enterprise

#if Professional || Enterprise
    #define Advanced
#endif

namespace Megacorp.Data
{
    #if Advanced
    class PivotTable {...}
    #endif
}
```
je platný, protože `#define` direktivy předcházejí prvnímu tokenu `namespace` (klíčové slovo) ve zdrojovém souboru.

V následujícím příkladu dojde k chybě při kompilaci, protože `#define` následuje skutečný kód:
```csharp
#define A
namespace N
{
    #define B
    #if B
    class Class1 {}
    #endif
}
```

Může definovat symbol podmíněné kompilace, který je již definován, aniž by došlo `#undef` k jeho použití pro tento symbol. `#define` Následující příklad definuje podmíněný kompilační symbol `A` a pak ho znovu definuje.
```csharp
#define A
#define A
```

`#undef` Může "undefine" undefine "undefined" – symbol podmíněné kompilace, který není definován. Následující příklad definuje podmíněný kompilační symbol `A` a pak je nedefinuje dvakrát; i když druhý `#undef` nemá žádný vliv, je stále platný.
```csharp
#define A
#undef A
#undef A
```

### <a name="conditional-compilation-directives"></a>Direktivy podmíněné kompilace

Direktivy podmíněné kompilace jsou používány k podmíněnému zahrnutí nebo vyloučení částí zdrojového souboru.

```antlr
pp_conditional
    : pp_if_section pp_elif_section* pp_else_section? pp_endif
    ;

pp_if_section
    : whitespace? '#' whitespace? 'if' whitespace pp_expression pp_new_line conditional_section?
    ;

pp_elif_section
    : whitespace? '#' whitespace? 'elif' whitespace pp_expression pp_new_line conditional_section?
    ;

pp_else_section:
    | whitespace? '#' whitespace? 'else' pp_new_line conditional_section?
    ;

pp_endif
    : whitespace? '#' whitespace? 'endif' pp_new_line
    ;

conditional_section
    : input_section
    | skipped_section
    ;

skipped_section
    : skipped_section_part+
    ;

skipped_section_part
    : skipped_characters? new_line
    | pp_directive
    ;

skipped_characters
    : whitespace? not_number_sign input_character*
    ;

not_number_sign
    : '<Any input_character except #>'
    ;
```

Jak je určeno syntaxí, direktivy podmíněné kompilace musí být zapsány jako sady, které se skládají `#if` z, v pořadí, `#elif` direktiva, nula nebo `#else` více direktiv, nula `#endif` nebo jedna direktiva a direktiva. Mezi direktivami jsou podmíněné oddíly zdrojového kódu. Každý oddíl je řízen hned předchozí direktivou. Podmíněný oddíl může sám obsahovat vnořené direktivy podmíněné kompilace, pokud tyto direktivy tvoří kompletní sady.

*Pp_conditional* vybere maximálně jeden z obsažených *conditional_section*s pro normální lexikální zpracování:

*  *Pp_expression*s direktivami `#if` a `#elif` jsou vyhodnocovány v pořadí, dokud jedna z hodnot `true`. Pokud výraz vrací `true`, je vybrána *conditional_section* odpovídající direktiva.
*  Pokud je *pp_expressiona*`false` a pokud je přítomna direktiva `#else`, vybere se *conditional_section* direktiva `#else`.
*  V opačném případě není vybrána žádná *conditional_section* .

Vybraná *conditional_section*, pokud existuje, je zpracována jako normální *input_section*: zdrojový kód obsažený v části musí splňovat lexikální gramatiku; tokeny jsou generovány ze zdrojového kódu v oddílu; a direktivy předběžného zpracování v oddílu mají předepsané účinky.

Zbývající *conditional_section*, pokud nějaké jsou zpracovávány jako *skipped_section*s s výjimkou předzpracování direktiv, zdrojový kód v části nemusí vyhovovat lexikální gramatice; v oddílu nejsou vygenerovány žádné tokeny ze zdrojového kódu. a direktivy předběžného zpracování v oddílu musí být lexikální, ale nejsou jinak zpracovány. V rámci *conditional_section* , který se zpracovává jako *skipped_section*, všechny vnořené *conditional_section*s (obsažené ve vnořených `#if`... `#endif` a `#region`... `#endregion`) se také zpracovávají jako skipped_.  *oddíl*s.

Následující příklad ukazuje, jak mohou direktivy podmíněné kompilace vnořovat:
```csharp
#define Debug       // Debugging on
#undef Trace        // Tracing off

class PurchaseTransaction
{
    void Commit() {
        #if Debug
            CheckConsistency();
            #if Trace
                WriteToLog(this.ToString());
            #endif
        #endif
        CommitHelper();
    }
}
```

S výjimkou předběžně zpracovávaných direktiv nepodléhá zdrojový kód lexikální analýze. Následující příklad je platný bez ohledu na neukončený komentář v `#else` části:
```csharp
#define Debug        // Debugging on

class PurchaseTransaction
{
    void Commit() {
        #if Debug
            CheckConsistency();
        #else
            /* Do something else
        #endif
    }
}
```

Všimněte si však, že direktivy předběžného zpracování jsou nutné k lexikálnímu opravení i v vynechaných částech zdrojového kódu.

Direktivy předběžného zpracování nejsou zpracovávány při jejich zobrazení uvnitř víceřádkových vstupních prvků. Například program:
```csharp
class Hello
{
    static void Main() {
        System.Console.WriteLine(@"hello, 
#if Debug
        world
#else
        Nebraska
#endif
        ");
    }
}
```
výsledek výstupu:
```console
hello,
#if Debug
        world
#else
        Nebraska
#endif
```

V běžných případech může sada předzpracovaných direktiv, které jsou zpracovány, záviset na vyhodnocení *pp_expression*. Příklad:
```csharp
#if X
    /*
#else
    /* */ class Q { }
#endif
```
vždy vytvoří stejný datový proud tokenu`class` ( `}` `Q` `{` ) bez ohledu na to, zda `X` je definován. Pokud `X` je definován, jediné zpracované direktivy jsou `#if` a `#endif`z důvodu víceřádkového komentáře. Pokud `X` není definován, pak tři direktivy (`#if`, `#else`, `#endif`) jsou součástí sady direktiv.

### <a name="diagnostic-directives"></a>Diagnostické direktivy

Diagnostické direktivy slouží k explicitnímu generování chybových zpráv a upozornění, které jsou hlášeny stejným způsobem jako jiné chyby při kompilaci a upozornění.

```antlr
pp_diagnostic
    : whitespace? '#' whitespace? 'error' pp_message
    | whitespace? '#' whitespace? 'warning' pp_message
    ;

pp_message
    : new_line
    | whitespace input_character* new_line
    ;
```

Příklad:
```csharp
#warning Code review needed before check-in

#if Debug && Retail
    #error A build can't be both debug and retail
#endif

class Test {...}
```
vždy vytvoří upozornění ("revize kódu nutná před vrácením se změnami") a vytvoří chybu při kompilaci ("sestavení nemůže být ladění a prodej"), pokud jsou definovány podmíněné symboly `Debug` a. `Retail` Všimněte si, že *pp_message* může obsahovat libovolný text; konkrétně nemusí obsahovat tokeny ve správném formátu, jak je znázorněno v jedné uvozovkě ve slově `can't`.

### <a name="region-directives"></a>Direktivy oblasti

Direktivy oblasti se používají k explicitnímu označení oblastí zdrojového kódu.

```antlr
pp_region
    : pp_start_region conditional_section? pp_end_region
    ;

pp_start_region
    : whitespace? '#' whitespace? 'region' pp_message
    ;

pp_end_region
    : whitespace? '#' whitespace? 'endregion' pp_message
    ;
```

K oblasti není připojen žádný sémantický význam. oblasti jsou určeny pro účely programátora nebo automatizovaných nástrojů k označení oddílu zdrojového kódu. Zpráva zadaná v `#region` direktivě nebo `#endregion` má také žádný sémantický význam; slouží pouze k identifikaci oblasti. Shoda s `#region` a direktivami `#endregion` mohou mít různé *pp_message*.

Lexikální zpracování oblasti:
```csharp
#region
...
#endregion
```
odpovídá přesně lexikálnímu zpracování direktivy podmíněné kompilace ve formátu:
```csharp
#if true
...
#endif
```

### <a name="line-directives"></a>Line – direktivy

Direktivy řádku lze použít k změně čísel řádků a názvů zdrojových souborů, které jsou hlášeny kompilátorem, jako jsou upozornění a chyby a které jsou používány atributy informací o volajícím ([atributy informací o volajícím](attributes.md#caller-info-attributes)).

Direktivy řádku se nejčastěji používají v nástrojích pro meta programování, C# které generují zdrojový kód z nějakého jiného textového vstupu.

```antlr
pp_line
    : whitespace? '#' whitespace? 'line' whitespace line_indicator pp_new_line
    ;

line_indicator
    : decimal_digit+ whitespace file_name
    | decimal_digit+
    | 'default'
    | 'hidden'
    ;

file_name
    : '"' file_name_character+ '"'
    ;

file_name_character
    : '<Any input_character except ">'
    ;
```

Pokud nejsou `#line` přítomny žádné direktivy, kompilátor ohlásí hodnoty true a názvy zdrojových souborů ve svém výstupu. Při zpracování direktivy `#line`, která obsahuje *line_indicator* , která není `default`, kompilátor zpracovává řádek za direktivou, která má dané číslo řádku (a název souboru, pokud je zadán).

`#line default` Direktiva obrátí účinek všech předchozích direktiv #line. Kompilátor ohlásí hodnotu true pro následné řádky, přesně tak, že nebyly zpracovány `#line` žádné direktivy.

`#line hidden` Direktiva nemá žádný vliv na soubor a čísla řádků uvedená v chybových zprávách, ale má vliv na ladění na úrovni zdroje. Při ladění nejsou všechny řádky mezi `#line hidden` direktivou a následnou `#line` direktivou (které nejsou `#line hidden`) žádné informace o čísle řádku. Při procházení kódu v ladicím programu budou tyto řádky zcela vynechány.

Všimněte si, že se *název_souboru* liší od regulárního řetězcového literálu v tom, že řídicí znaky nejsou zpracovány; znak "`\`" jednoduše označuje normální znak zpětného lomítka v rámci *název_souboru*.

### <a name="pragma-directives"></a>Direktivy pragma

`#pragma` Direktiva předzpracování se používá k určení volitelných kontextových informací kompilátoru. Informace, které jsou zadány v `#pragma` direktivě, nebudou nikdy měnit sémantiku programu.

```antlr
pp_pragma
    : whitespace? '#' whitespace? 'pragma' whitespace pragma_body pp_new_line
    ;

pragma_body
    : pragma_warning_body
    ;
```

C#poskytuje `#pragma` direktivy pro řízení upozornění kompilátoru. Budoucí verze jazyka mohou zahrnovat další `#pragma` direktivy. Aby byla zajištěna interoperabilita s jinými C# kompilátory C# , kompilátor společnosti Microsoft nevydá chyby kompilace `#pragma` pro neznámé direktivy; takové direktivy však generují upozornění.

#### <a name="pragma-warning"></a>pragma – upozornění

`#pragma warning` Direktiva slouží k zakázání nebo obnovení všech nebo konkrétní sady varovných zpráv během kompilace následného textu programu.

```antlr
pragma_warning_body
    : 'warning' whitespace warning_action
    | 'warning' whitespace warning_action whitespace warning_list
    ;

warning_action
    : 'disable'
    | 'restore'
    ;

warning_list
    : decimal_digit+ (whitespace? ',' whitespace? decimal_digit+)*
    ;
```

`#pragma warning` Direktiva, která vynechává seznam upozornění, má vliv na všechna upozornění. `#pragma warning` Direktiva, která obsahuje seznam upozornění, má vliv pouze na upozornění, která jsou uvedena v seznamu.

`#pragma warning disable` Direktiva zakáže všechny nebo danou sadu upozornění.

`#pragma warning restore` Direktiva obnoví všechny nebo danou sadu upozornění na stav, který byl platný na začátku kompilační jednotky. Všimněte si, že pokud bylo určité upozornění zakázáno externě, `#pragma warning restore` a (zda pro všechna nebo konkrétní upozornění) nebude toto upozornění znovu povoleno.

Následující příklad ukazuje použití `#pragma warning` k dočasnému zakázání upozornění nahlášeného při odkazování na zastaralé členy pomocí čísla upozornění z kompilátoru společnosti Microsoft. C#
```csharp
using System;

class Program
{
    [Obsolete]
    static void Foo() {}

    static void Main() {
#pragma warning disable 612
    Foo();
#pragma warning restore 612
    }
}
```
