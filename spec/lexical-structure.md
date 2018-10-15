# <a name="lexical-structure"></a>Lexikální struktura

## <a name="programs"></a>Programy

C# ***program*** obsahuje jeden nebo více ***zdrojové soubory***známé formálně jako ***kompilačních jednotek*** ([kompilačních jednotek](namespaces.md#compilation-units)). Zdrojový soubor je seřazená posloupnost znaků Unicode. Zdrojové soubory obvykle korespondovaly s soubory v systému souborů, ale tento korespondenci se nevyžaduje. Pro maximální přenositelnost, se doporučuje, aby soubory v systému souborů být zakódován pomocí kódování UTF-8 kódování.

Obecně vzato programu je zkompilován pomocí tří kroků:

1. Transformace, která převede soubor z určitého znaku repertoáru a schéma kódování na sekvenci znaků Unicode.
2. Lexikální analýzu, která převede datový proud vstupních znaků Unicode do datového proudu tokenů.
3. Syntaktické analýzy, která převede datový proud s tokeny do spustitelného kódu.

## <a name="grammars"></a>Gramatika

Tato specifikace uvede syntaxe jazyka C# programování pomocí dvou gramatiky. ***Gramatika slov*** ([gramatika slov](lexical-structure.md#lexical-grammar)) definuje, jak jsou zkombinované znaků Unicode pro zakončení řádku formuláře, prázdné znaky, komentáře, tokenů a direktivy předběžného zpracování. ***Syntaktické gramatiky*** ([syntaktické gramatiky](lexical-structure.md#syntactic-grammar)) definuje, jak se tokeny vyplývající z gramatika slov zkombinovala, aby programy formuláře C#.

### <a name="grammar-notation"></a>Zápis gramatiky

Gramatika slov a syntaktické jsou uvedeny v Backus-Naur formuláře pomocí notace nástroj ANTLR gramatiky.

### <a name="lexical-grammar"></a>Gramatika slov

Gramatika slov jazyka C# je uveden v [provést lexikální analýzu](lexical-structure.md#lexical-analysis), [tokeny](lexical-structure.md#tokens), a [předběžné zpracování direktiv](lexical-structure.md#pre-processing-directives). Terminálu symboly gramatika slov jsou znaky znakové sady Unicode a gramatika slov Určuje, jak jsou zkombinované znaků na formulář tokeny ([tokeny](lexical-structure.md#tokens)), mezer ([prázdných](lexical-structure.md#white-space)), komentáře ([komentáře](lexical-structure.md#comments)) a direktivy předběžného zpracování ([předběžné zpracování direktiv](lexical-structure.md#pre-processing-directives)).

Každý zdrojový soubor v programu v jazyce C# musí odpovídat *vstupní* produkční gramatika slov ([provést lexikální analýzu](lexical-structure.md#lexical-analysis)).

### <a name="syntactic-grammar"></a>Syntaktické gramatiky

Syntaktické gramatika C# jsou zobrazena v kapitolách a přílohy, které následují této kapitole. Terminálu symboly syntaktické gramatiky jsou tokeny definované gramatika slov a syntaktické gramatiky Určuje, jak se tokeny zkombinovala, aby programy jazyka C# formulářů.

Každý zdrojový soubor v programu v jazyce C# musí odpovídat *compilation_unit* produkční syntaktické gramatiky ([kompilačních jednotek](namespaces.md#compilation-units)).

## <a name="lexical-analysis"></a>Lexikální analýzu

*Vstupní* produkční definuje lexikální struktura zdrojový soubor jazyka C#. Tato výroba gramatika slov musí odpovídat každý zdrojový soubor v programu v jazyce C#.

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

Pět základních prvků, které tvoří lexikální struktura zdrojový soubor jazyka C#: řádek ukončovací znaky ([řádek ukončovací znaky](lexical-structure.md#line-terminators)), mezer ([prázdných](lexical-structure.md#white-space)), komentáře ([komentáře](lexical-structure.md#comments)), tokeny ([tokeny](lexical-structure.md#tokens)) a direktivy předběžného zpracování ([předběžné zpracování direktiv](lexical-structure.md#pre-processing-directives)). Tyto základní prvky pouze tokeny jsou rozhodující pro syntaktické gramatiky programu v jazyce C# ([syntaktické gramatiky](lexical-structure.md#syntactic-grammar)).

Lexikální zpracování zdrojový soubor jazyka C# se skládá z snížení soubor do sekvence tokenů, který bude vstup pro syntaktické analýzy. Řádku, prázdné znaky a komentáře, může sloužit k oddělení tokeny a direktivy předběžného zpracování může způsobit části zdrojového souboru přeskočit, ale jinak tyto Lexikální prvky mít vliv na syntaktické struktura programu v jazyce C#.

V případě interpolovaný řetězec literálů ([interpolované řetězce literálů](lexical-structure.md#interpolated-string-literals)) do jednoho tokenu původně vytvořil provést lexikální analýzu, ale je rozdělený do několika vstupních elementy, které jsou opakovaně podroben provést lexikální analýzu dokud všechny literály interpolovaném řetězci byly vyřešeny. Použitím výsledných tokenů, sloužit jako vstup pro syntaktické analýzy.

Když několik výroby gramatika slov odpovídají pořadí znaků ve zdrojovém souboru, lexikální se vždy zpracování forms nejdelší možný lexikální elementu. Například sekvence znaků `//` je zpracovat jako začátek jednořádkový komentář, protože je delší než jeden tento element lexikální `/` token.

### <a name="line-terminators"></a>Řádku

Řádku rozdělit řádky znaků zdrojový soubor jazyka C#.

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

Pro kompatibilitu s zdrojový kód nástroje pro úpravy, které aplikacím dodávají endovém souborovém značky a umožňuje zdroji soubor prohlížení jako posloupnost správně ukončena řádky, se použijí následující transformace, aby každý zdrojový soubor v programu v jazyce C#:

*  Pokud poslední znak zdrojového souboru, je znak Z ovládacího prvku (`U+001A`), se odstraní tento znak.
*  Znak pro návrat na začátek řádku (`U+000D`) se přidá do konce zdrojového souboru, pokud se tento zdrojový soubor je prázdný, a pokud poslední znak zdrojového souboru není zalomení řádku (`U+000D`), odřádkování (`U+000A`), oddělovač řádků (`U+2028`), nebo oddělovač odstavců (`U+2029`).

### <a name="comments"></a>Komentáře

Jsou podporovány dvě formy komentáře: Jednořádkové komentáře a poznámky s oddělovači. ***Jednořádkové komentáře*** začínají znaky `//` a rozšířit na konec řádku zdroje. ***Oddělené komentáře*** začínají znaky `/*` a končit znaky `*/`. Komentáře s oddělovači může zahrnovat více řádků.

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
    : '/*' delimited_comment_section* asterisk* '/'
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

Komentáře nelze vnořovat. Sekvence znaků `/*` a `*/` nemají zvláštní význam v rámci `//` komentář a znakovými sekvencemi `//` a `/*` nemají zvláštní význam v rámci oddělovači komentář.

V rámci znakové a řetězcové literály nejsou zpracovány poznámky.

V příkladu
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
obsahuje komentář s oddělovači.

V příkladu
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
ukazuje několik Jednořádkové komentáře.

### <a name="white-space"></a>Prázdné znaky

Prázdné místo je definována jako libovolný znak Unicode třída Zs (která obsahuje znak mezery) a znak horizontálního tabulátoru, znak svislého tabulátoru a formulář se znakem kanálu.

```antlr
whitespace
    : '<Any character with Unicode class Zs>'
    | '<Horizontal tab character (U+0009)>'
    | '<Vertical tab character (U+000B)>'
    | '<Form feed character (U+000C)>'
    ;
```

## <a name="tokens"></a>Tokeny

Existuje několik druhů tokenů: identifikátory, klíčová slova, literály, operátory a interpunkční znaky. Prázdné znaky a komentáře nejsou tokeny, i když působí jako oddělovače pro tokeny.

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

### <a name="unicode-character-escape-sequences"></a>Znak – řídicí sekvence Unicode

Znak řídicí sekvence Unicode hodnota představuje znak Unicode. Znak – řídicí sekvence Unicode se zpracovávají v identifikátorech ([identifikátory](lexical-structure.md#identifiers)), znakové literály ([literály znaků](lexical-structure.md#character-literals)) a pravidelné řetězcových literálů ([řetězcových literálů](lexical-structure.md#string-literals)). Řídicí znak Unicode není zpracována v jiném umístění (například formulář operátor, interpunkci nebo – klíčové slovo).

```antlr
unicode_escape_sequence
    : '\\u' hex_digit hex_digit hex_digit hex_digit
    | '\\U' hex_digit hex_digit hex_digit hex_digit hex_digit hex_digit hex_digit hex_digit
    ;
```

Řídicí sekvence Unicode představuje jeden znak Unicode tvořen šestnáctkové číslo následující "`\u`"nebo"`\U`" znaků. Protože jazyk C# používá 16 bitů kódování kódové body sady Unicode v znaky a řetězcové hodnoty, znak Unicode v rozsahu U + 10000 až U + 10FFFF není povolená ve znakového literálu a se vyjadřuje náhradní pár kódu Unicode v řetězcovém literálu. Znaky Unicode s body kódu nad 0x10FFFF nejsou podporovány.

Více posunutí se neprovádí. Například textový literál "`\u005Cu005C`"je ekvivalentní k"`\u005C`"místo"`\`". Hodnota Unicode `\u005C` je znak "`\`".

V příkladu
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
ukazuje použití několika `\u0066`, což je řídicí sekvence pro písmeno "`f`". Program je ekvivalentní
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

Pravidla pro identifikátory, které jsou uvedené v této části přesně odpovídat doporučené Unicode Standard Annex 31, s tím rozdílem, že podtržítka je povolen jako počáteční znak (jako je tradiční v programovacím jazyce C), řídicí sekvence Unicode povolené v identifikátorech a "`@`" znak je povolený jako předponu povolit klíčová slova používat jako identifikátory.

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

Informace o třídách znaků Unicode uvedených výše viz standardu Unicode, verze 3.0, bod 4.5.

Příklady platné identifikátory "`identifier1`","`_identifier2`", a "`@if`".

Identifikátor vyhovující aplikace musí být v kanonickém formátu definovaném v normalizačním formuláři Unicode C, jak jsou definovány v Unicode Standard Annex 15. Chování při zjištění identifikátor není v normalizace formuláře C, je definováno implementací; Diagnostika však není povinné.

Předpona "`@`" umožňuje použít klíčová slova jako identifikátory, což je užitečné při propojování s jinými programovací jazyky. Znak `@` není ve skutečnosti součástí identifikátoru, a proto identifikátor může zobrazit v jiných jazycích jako normální identifikátor, bez předpony. Identifikátor s `@` předpona je volána ***Doslovný identifikátor***. Použití `@` předpona pro identifikátory, které nejsou klíčových slov je povolené, ale důrazně nedoporučuje jak stylu.

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
definuje třídu s názvem "`class`"se statickou metodu s názvem"`static`", která přebírá parametr s názvem"`bool`". Všimněte si, že vzhledem k tomu, že řídicí sekvence Unicode nejsou povolené v klíčových slov, token "`cl\u0061ss`"je identifikátor, a je stejný identifikátor jako"`@class`".

Dva identifikátory jsou považovány za totéž případě, že jsou identické, až tyto transformace jsou použity v zadaném pořadí:

*  Předpona "`@`", pokud použijete, se odebere.
*  Každý *unicode_escape_sequence* se transformuje na jeho odpovídající znak Unicode.
*  Žádné *formatting_character*byly odebrány.

Identifikátory obsahující dvě po sobě jdoucí znaky podtržení (`U+005F`) jsou vyhrazené pro použití v implementaci. Implementace může například poskytovat klíčových slov rozšířených začínající dvěma podtržítky.

### <a name="keywords"></a>Klíčová slova

A ***– klíčové slovo*** je identifikátor jako posloupnost znaků, který je vyhrazen a nelze jej použít jako identifikátoru s výjimkou při uvedena ve `@` znak.

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

V některých místech v gramatice zvláštní identifikátory mají zvláštní význam, ale nejsou klíčová slova. Tyto identifikátory jsou někdy označovány jako "kontextová klíčová slova". Například v deklaraci vlastnosti "`get`"a"`set`" mají zvláštní význam identifikátory ([přistupující objekty](classes.md#accessors)). Identifikátor jiných než `get` nebo `set` nikdy smí v těchto umístěních, takže toto použití nejsou v konfliktu s využitím tato slova jako identifikátory. V ostatních případech, například stejně jako identifikátor "`var`" v deklaracích implicitně typované lokální proměnné ([místní deklarace proměnné](statements.md#local-variable-declarations)), kontextové klíčové slovo může dojít ke konfliktu s názvy deklarované. V takových případech deklarovaný název má přednost před použití identifikátor jako kontextové klíčové slovo.

### <a name="literals"></a>Literály

A ***literálu*** je reprezentace hodnotu zdrojového kódu.

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

#### <a name="boolean-literals"></a>Logické hodnoty literálu

Existují dvě logické hodnoty literálu: `true` a `false`.

```antlr
boolean_literal
    : 'true'
    | 'false'
    ;
```

Typ *boolean_literal* je `bool`.

#### <a name="integer-literals"></a>Literály celých čísel

Literály celých čísel se používá k zápisu hodnoty typů `int`, `uint`, `long`, a `ulong`. Literály celých čísel mít dva možné typy: desetinných míst a šestnáctková.

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

Typ celočíselného literálu je stanoven následujícím způsobem:

*  Pokud literálu bez přípony, má první z těchto typů, ve kterých může být reprezentována jeho hodnota: `int`, `uint`, `long`, `ulong`.
*  Pokud je literál příponou podle `U` nebo `u`, má první z těchto typů, ve kterých může být reprezentována jeho hodnota: `uint`, `ulong`.
*  Pokud je literál příponou podle `L` nebo `l`, má první z těchto typů, ve kterých může být reprezentována jeho hodnota: `long`, `ulong`.
*  Pokud je literál příponou podle `UL`, `Ul`, `uL`, `ul`, `LU`, `Lu`, `lU`, nebo `lu`, je typu `ulong`.

Pokud hodnota reprezentovaná celočíselného literálu je mimo rozsah `ulong` zadejte, dojde k chybě kompilace.

Jak styl, je určeno, který "`L`"měl použít namísto"`l`" při zápisu literály typu `long`, protože je snadno zaměnit písmeno "`l`"se číslice"`1`".

Tak, aby povolovala nejmenší `int` a `long` hodnot má být zapsán jako literály desítkové celé číslo, existují následující dvě pravidla:

* Když *decimal_integer_literal* s hodnotou 2147483648 (2 ^ 31) a ne *integer_type_suffix* se zobrazí jako token hned za unární operátor minus token operátoru ([Unární minus operátor](expressions.md#unary-minus-operator)), výsledkem je konstanta typu `int` s hodnotou -2147483648 (-2 ^ 31). V jiných situacích takové *decimal_integer_literal* je typu `uint`.
* Když *decimal_integer_literal* s hodnotou 9223372036854775808 (2 ^ 63) a ne *integer_type_suffix* nebo *integer_type_suffix* `L` nebo `l` se zobrazí jako token hned za unární operátor minus token – operátor ([unární operátor minus](expressions.md#unary-minus-operator)), výsledkem je konstanta typu `long` s hodnotou -9223372036854775808 (-2 ^ 63). V jiných situacích takové *decimal_integer_literal* je typu `ulong`.

#### <a name="real-literals"></a>Skutečné literály

Skutečné literály se používají pro zápis hodnoty typů `float`, `double`, a `decimal`.

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

Pokud ne *real_type_suffix* není zadána, je typu literál reálného čísla `double`. V opačném případě přípona reálný typ Určuje typ literál reálného čísla, následujícím způsobem:

*  Literál reálného čísla příponou podle `F` nebo `f` je typu `float`. Například literály `1f`, `1.5f`, `1e10f`, a `123.456F` jsou všechny typu `float`.
*  Literál reálného čísla příponou podle `D` nebo `d` je typu `double`. Například literály `1d`, `1.5d`, `1e10d`, a `123.456D` jsou všechny typu `double`.
*  Literál reálného čísla příponou podle `M` nebo `m` je typu `decimal`. Například literály `1m`, `1.5m`, `1e10m`, a `123.456M` jsou všechny typu `decimal`. Tato literál je převedena na `decimal` hodnotu tak, že přesná hodnota, v případě potřeby zaokrouhlení na nejbližší reprezentovatelnou hodnotu pomocí společnosti bankovní zaokrouhlení ([typem decimal](types.md#the-decimal-type)). Libovolného rozsahu zřejmé v literálu se zachovají, pokud je tato hodnota zaokrouhlena nebo hodnota je nula (v tom druhém případě přihlašování a škálování bude 0). Proto je literál `2.900m` tvoří desetinné čárky se znaménkem, bude analyzována `0`, koeficient `2900`a škálování `3`.

Pokud v zvolený typ nelze reprezentovat zadaný literál, dojde k chybě v době kompilace.

Hodnota literál reálného čísla typu `float` nebo `double` je určit pomocí standardu IEEE "zaokrouhlí na nejbližší" režimu.

Všimněte si, že v literál reálného čísla, desítkových číslic jsou vždy požadované za desetinnou čárkou. Například `1.3F` je literál typu reálné číslo ale `1.F` není.

#### <a name="character-literals"></a>Znakové literály

Znakový literál představuje jeden znak a obvykle obsahuje znak do uvozovek, stejně jako v `'a'`.

Poznámka: Zápis gramatiky ANTLR provede následující matoucí! V ANTLR při psaní `\'` zkratka pro jednoduchá uvozovka `'`. A při psaní `\\` zkratka pro jedno zpětné lomítko `\`. Proto první pravidlo pro literální znak znamená, že začíná jednoduchá uvozovka, pak znak pak jednoduché uvozovky. A jsou jedenáct možné jednoduchá řídící sekvence `\'`, `\"`, `\\`, `\0`, `\a`, `\b`, `\f`, `\n`, `\r`, `\t`, `\v`.

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

Znak, který následuje znak zpětného lomítka (`\`) v *znak* musí být jedna z následujících znaků: `'`, `"`, `\`, `0`, `a`, `b` , `f`, `n`, `r`, `t`, `u`, `U`, `x`, `v`. V opačném případě dojde k chybě v době kompilace.

Šestnáctková řídicí sekvence představuje jeden znak Unicode s hodnotou tvořen šestnáctkové číslo následující "`\x`".

Pokud je hodnota reprezentovaná literální znak větší než `U+FFFF`, dojde k chybě kompilace.

Řídicí sekvence znaků Unicode ([znak – řídicí sekvence Unicode](lexical-structure.md#unicode-character-escape-sequences)) ve znakového literálu musí být v rozsahu `U+0000` k `U+FFFF`.

Jednoduchá řídící sekvence představuje kódování Unicode znaku, jak je popsáno v následující tabulce.


| __Řídicí sekvence__ | __Název znaku__ | __Kódování Unicode__ |
|---------------------|--------------------|----------------------|
| `\'`                | jednoduché uvozovky       | `0x0027`             | 
| `\"`                | dvojité uvozovky       | `0x0022`             | 
| `\\`                | Zpětné lomítko          | `0x005C`             | 
| `\0`                | Null               | `0x0000`             | 
| `\a`                | Upozornění              | `0x0007`             | 
| `\b`                | Backspace          | `0x0008`             | 
| `\f`                | Posun strany          | `0x000C`             | 
| `\n`                | Nový řádek           | `0x000A`             | 
| `\r`                | Návrat na začátek řádku    | `0x000D`             | 
| `\t`                | Horizontální tabulátor     | `0x0009`             | 
| `\v`                | Vertikální tabulátor       | `0x000B`             | 

Typ *character_literal* je `char`.

#### <a name="string-literals"></a>Řetězcové literály

C# podporuje dvě formy řetězcových literálů: ***regulární řetězcové literály*** a ***doslovný řetězec literálů***.

Pravidelné Textový literál skládá z nula nebo více znaků uzavřených v uvozovkách, stejně jako v `"hello"`a může jich obsahovat i jednoduchá řídící sekvence (jako například `\t` pro znak tabulátoru) a šestnáctkové a řídicí sekvence Unicode.

Doslovný řetězec literálu se skládá z `@` znak následovaný znak dvojitých uvozovek, nula nebo více znaků a koncový znak dvojitých uvozovek. Je jednoduchý příklad `@"hello"`. Začal v doslovném řetězci literálu, jsou znaky mezi oddělovači interpretovány verbatim, pouze se výjimka *quote_escape_sequence*. Zejména jednoduchá řídící sekvence a šestnáctkové a řídicí sekvence Unicode nejsou zpracovány v doslovném řetězci literály. Doslovný řetězec literálu může zahrnovat více řádků.

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

Znak, který následuje znak zpětného lomítka (`\`) v *regular_string_literal_character* musí být jedna z následujících znaků: `'`, `"`, `\`, `0`, `a` , `b`, `f`, `n`, `r`, `t`, `u`, `U`, `x`, `v`. V opačném případě dojde k chybě v době kompilace.

V příkladu
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
ukazuje různé řetězcové literály. Poslední řetězcový literál, `j`, je doslovný řetězec literálu, která zahrnuje více řádků. Znaky mezi uvozovkami, včetně prázdných znaků, jako je například znaky nového řádku, jsou zachovány verbatim.

Protože šestnáctkové řídicí sekvence může mít proměnný počet šestnáctkových číslic, řetězcový literál `"\x123"` obsahuje jeden znak s šestnáctkovou hodnotou 123. Řetězec obsahující znak s šestnáctkovou hodnotou 12 následované znakem 3 vytvoříte jeden napsat `"\x00123"` nebo `"\x12" + "3"` místo.

Typ *řetězcový_literál* je `string`.

Každý řetězec literálu nemá za následek nutně novou instanci řetězce. Když dva nebo více řetězcových literálů, které jsou ekvivalentní podle operátoru rovnosti řetězců ([řetězec operátory rovnosti](expressions.md#string-equality-operators)) se zobrazí ve stejném programu, tyto řetězcové literály odkazují na stejnou instanci řetězce. Například výstup vyprodukovaný
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
je `True` vzhledem k tomu dva literály odkazují na stejnou instanci řetězce.

#### <a name="interpolated-string-literals"></a>Interpolované řetězce literálů

Interpolované řetězcové literály jsou podobné řetězcové literály, ale obsahovat děr odděleny `{` a `}`, ve které může dojít, výrazy. Za běhu jsou výrazy vyhodnocovány za účelem s jejich textové formulářů nahradit do řetězce na místě, kde dochází k riziko. Syntaxe a sémantiky interpolace řetězců jsou popsány v části ([interpolovaných řetězců](expressions.md#interpolated-strings)).

Například řetězcové literály může být interpolovaný řetězec literálů pravidelné nebo verbatim. Interpolované regulární řetězcové literály jsou odděleny `$"` a `"`, a jsou odděleny interpolovaný řetězec verbatim literály `$@"` a `"`.

Stejně jako ostatní literály provést lexikální analýzu interpolovaného řetězce literálu zpočátku výsledků do jednoho tokenu podle gramatika níže. Ale před syntaktické analýzy jeden token interpolovaného řetězce literálu je rozdělená do několika tokeny pro části řetězec uzavírající děr a vstupní prvky, ke kterým dochází v děr analyzují lexikálně znovu. To zase může vytvořit více interpolované řetězcových literálů na zpracování, ale, pokud lexikálně opravit, bude nakonec vést k posloupnost tokeny pro syntaktické analýzu ke zpracování.

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

*Interpolated_string_literal* token je interpretovány jako více tokenů a dalších vstupních prvků následujícím způsobem, v pořadí podle výskytu v *interpolated_string_literal*:

* Výskyty následující jsou interpretovány jako samostatné jednotlivé tokeny: úvodního `$` znaménko, *interpolated_regular_string_whole*, *interpolated_regular_string_start*, *interpolated_regular_string_mid*, *interpolated_regular_string_end*, *interpolated_verbatim_string_whole*,  *interpolated_verbatim_string_start*, *interpolated_verbatim_string_mid* a *interpolated_verbatim_string_end*.
* Výskyty *regular_balanced_text* a *verbatim_balanced_text* mezi těmito se naplánovalo jako *input_section* ([provést lexikální analýzu ](lexical-structure.md#lexical-analysis)) a jsou interpretovány jako výsledné pořadí elementů vstupu. Může pak patří mezi ně interpolovaném řetězci opětovnou interpretaci bitového literálové tokeny.

Syntaktické analýzy se opětovnému zkombinování tokeny do *interpolated_string_expression* ([interpolovaných řetězců](expressions.md#interpolated-strings)).

Příklady TODO


#### <a name="the-null-literal"></a>Literál s hodnotou null

```antlr
null_literal
    : 'null'
    ;
```

*Null_literal* lze implicitně převést na typ odkazu nebo typ připouštějící hodnotu Null.

### <a name="operators-and-punctuators"></a>Operátory a interpunkční znaky

Existuje několik druhů operátory a interpunkční znaky. Operátory se ve výrazech používají k popisu operace, které zahrnují jeden nebo více operandů. Například výraz `a + b` používá `+` operátor přidat dva operandy `a` a `b`. Interpunkční znaky jsou určené pro seskupování a oddělení.

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

Svislá čára v *right_shift* a *right_shift_assignment* výroby slouží k označení, že, na rozdíl od jiných výroby v syntaxi gramatice žádné znaky jakéhokoli druhu (dokonce ani prázdné znaky) jsou povolené mezi tokeny. Tyto výroby jsou zpracovány speciálně, chcete-li povolit správné zpracování *type_parameter_list*s ([parametry typu](classes.md#type-parameters)).

## <a name="pre-processing-directives"></a>Direktivy předběžného zpracování

Direktivy předběžného zpracování umožní v podmíněně přeskočit oddílů zdrojové soubory k nahlášení chyby a upozornění podmínky a zobrazit různé oblasti zdrojového kódu. Termín "předběžné zpracování direktiv" se používá pouze pro soulad s programovacích jazyků C a C++. V jazyce C# neexistuje žádný samostatný krok předběžného zpracování. direktivy předběžného zpracování jsou zpracovávány jako součást fáze provést lexikální analýzu.

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

*  `#define` a `#undef`, které se používají k definování a, respektive nedefinované symboly podmíněné kompilace ([deklaračních direktiv](lexical-structure.md#declaration-directives)).
*  `#if`, `#elif`, `#else`, a `#endif`, které se používají k podmíněně vynechá části zdrojového kódu ([direktivy podmíněné kompilace](lexical-structure.md#conditional-compilation-directives)).
*  `#line`, který se používá k řízení čísla řádků pro chyby a upozornění ([řádek direktivy](lexical-structure.md#line-directives)).
*  `#error` a `#warning`, které se používají k vydávání chyby a upozornění, respektive ([diagnostických direktivy](lexical-structure.md#diagnostic-directives)).
*  `#region` a `#endregion`, které se používají k explicitně označit části zdrojového kódu ([direktivy oblastí](lexical-structure.md#region-directives)).
*  `#pragma`, který se používá k určení volitelné kontextové informace kompilátoru ([direktivy Pragma](lexical-structure.md#pragma-directives)).

Direktivy předběžného zpracování vždy zabírá samostatný řádek zdrojového kódu a vždy začíná službami `#` znaků a název direktivy předběžného zpracování. Prázdné znaky může dojít před `#` znak a mezi `#` znaků a název direktivy.

Zdrojový řádek obsahující `#define`, `#undef`, `#if`, `#elif`, `#else`, `#endif`, `#line`, nebo `#endregion` – direktiva mohou končit jednořádkový komentář. Komentáře s oddělovači ( `/* */` Styl komentáře) nejsou povoleny na zdrojové řádky obsahující direktivy předběžného zpracování.

Direktivy předběžného zpracování nejsou tokeny a nejsou součástí syntaktické gramatika C#. Direktivy předběžného zpracování však lze použít k zahrnutí nebo vyloučení sekvence tokenů a můžete tímto způsobem ovlivnit význam programu v jazyce C#. Například při kompilaci programu:
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
výsledky v přesně stejnou sekvenci tokeny jako programu:
```csharp
class C
{
    void F() {}
    void I() {}
}
```

To znamená že lexikálně, dva programy je značně odlišná, syntakticky, že jsou identické.

### <a name="conditional-compilation-symbols"></a>Symboly podmíněné kompilace

Podmíněná kompilace funkce poskytované službou `#if`, `#elif`, `#else`, a `#endif` direktivy je řízen pomocí předběžného zpracování výrazů ([předběžného zpracování výrazů](lexical-structure.md#pre-processing-expressions)) a symboly podmíněné kompilace.

```antlr
conditional_symbol
    : '<Any identifier_or_keyword except true or false>'
    ;
```

Symbol podmíněné kompilace má dva možné stavy: ***definované*** nebo ***nedefinované***. Na začátku lexikální zpracování zdrojového souboru, je definován symbol podmíněné kompilace, pokud byly explicitně definovány pomocí externího mechanismu (jako je například možnost příkazového řádku kompilátoru). Když `#define` – direktiva se zpracovává, stane definován symbol podmíněné kompilace v náhradu této direktivy s názvem v daném zdrojovém souboru. Symbol zůstává definovaný až `#undef` směrnice pro zpracování stejným symbolu, nebo dokud je dosaženo konce zdrojového souboru. Důsledkem tohoto je, že `#define` a `#undef` direktivy v jednom zdrojovém souboru nemají žádný vliv na jiných zdrojových souborech ve stejném programu.

Při odkazování ve výrazu předběžného zpracování, symbol definovaný podmíněné kompilace má logická hodnota `true`, a symbol definován Podmíněná kompilace je logická hodnota `false`. Neexistuje žádný požadavek, symboly podmíněné kompilace explicitně deklarovat předtím, než jsou odkazovány v předběžného zpracování výrazů. Místo toho nedeklarovaný symboly nejsou jednoduše definovány a proto mají hodnotu `false`.

Obor názvů pro symboly podmíněné kompilace je samostatný a oddělený od jiných pojmenovaných entit v programu v jazyce C#. Symboly podmíněné kompilace může být odkazováno pouze v `#define` a `#undef` direktivy a předběžného zpracování výrazů.

### <a name="pre-processing-expressions"></a>Předběžné zpracování výrazů

Předběžné zpracování výrazů může dojít v `#if` a `#elif` direktivy. Operátory `!`, `==`, `!=`, `&&` a `||` jsou povolené v předběžného zpracování výrazů, a mohou být použity závorky pro seskupení.

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

Při odkazování ve výrazu předběžného zpracování, symbol definovaný podmíněné kompilace má logická hodnota `true`, a symbol definován Podmíněná kompilace je logická hodnota `false`.

Vyhodnocení výrazu předběžného zpracování vždy vrátí hodnotu typu boolean. Pravidla vyhodnocení výrazu předběžného zpracování jsou stejné jako pro konstantní výraz ([konstantní výrazy](expressions.md#constant-expressions)), s tím rozdílem, že pouze entity definovaný uživatelem, které může být odkazováno jsou symboly podmíněné kompilace .

### <a name="declaration-directives"></a>Deklaračních direktiv

Deklaračních direktiv se používají k definování nebo nedefinované symboly podmíněné kompilace.

```antlr
pp_declaration
    : whitespace? '#' whitespace? 'define' whitespace conditional_symbol pp_new_line
    | whitespace? '#' whitespace? 'undef' whitespace conditional_symbol pp_new_line
    ;

pp_new_line
    : whitespace? single_line_comment? new_line
    ;
```

Zpracování `#define` – direktiva způsobí, že symbol daného podmíněné kompilace budou definovat, počínaje zdrojového řádku, který následuje direktivu. Podobně, zpracování `#undef` – direktiva způsobí, že symbol daného podmíněné kompilace se nedefinované, počínaje zdrojového řádku, který následuje direktivu.

Žádné `#define` a `#undef` směrnice ve zdrojovém souboru se musí vyskytovat před první *token* ([tokeny](lexical-structure.md#tokens)) ve zdrojovém souboru; jinak kompilace dojde k chybě. Intuitivní řečeno `#define` a `#undef` direktivy musí předcházet všechny "skutečného kódu" ve zdrojovém souboru.

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
je platný vzhledem k tomu `#define` direktivy předcházet první token ( `namespace` – klíčové slovo) ve zdrojovém souboru.

Následující příklad vrátí chybu v době kompilace, protože `#define` následuje skutečného kódu:
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

A `#define` může definovat symbol podmíněné kompilace, která je již definován, aniž by bylo jakékoli intervenující `#undef` pro tento symbol. Následující příklad definuje symbol podmíněné kompilace `A` a znovu ji definuje.
```csharp
#define A
#define A
```

A `#undef` může "definice" podmíněné kompilace symbolu, která není definovaná. Následující příklad definuje symbol podmíněné kompilace `A` a potom nedefinovaných hodnot ji dvakrát; i když druhý `#undef` nemá žádný vliv, je stále platný.
```csharp
#define A
#undef A
#undef A
```

### <a name="conditional-compilation-directives"></a>Direktivy podmíněné kompilace

Direktivy podmíněné kompilace umožňují podmíněně zahrnout nebo vyloučit částí zdrojového souboru.

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

Jak je uvedeno v syntaxi, direktivy podmíněné kompilace musí být napsaná jako sady skládající se z, v pořadí, `#if` směrnice, nula nebo více `#elif` direktivy, nula nebo `#else` směrnice a `#endif` – direktiva. Mezi direktivy jsou podmíněné sekce zdrojového kódu. Každý oddíl se řídí bezprostředně předcházející směrnice. Podmíněný oddíl může obsahovat direktivy podmíněné kompilace vnořené zadané tyto direktivy tvoří kompletní sady.

A *pp_conditional* vybere jenom jedna z uzavřeného *conditional_section*s normální lexikální zpracování:

*  *Pp_expression*s `#if` a `#elif` direktivy jsou vyhodnocovány v pořadí, dokud jeden vrací `true`. Pokud výraz vrací `true`, *conditional_section* je vybrána odpovídající direktivy.
*  Pokud všechny *pp_expression*s yield `false`a pokud `#else` – direktiva je k dispozici, *conditional_section* z `#else` – direktiva je vybrána.
*  V opačném případě ne *conditional_section* zaškrtnuto.

Vybrané *conditional_section*, pokud existuje, se zpracovává jako normální *input_section*: musí dodržovat zdrojového kódu obsažen v části gramatika slov; tokeny jsou generovány ze zdroje kód v části; a předběžné zpracování direktiv v části mají předepsané účinky.

Zbývající *conditional_section*s, je-li k dispozici, jsou zpracovávány jako *skipped_section*s: s výjimkou direktivy předběžného zpracování, zdrojový kód v části nemusí splňovat lexikální gramatiky; ne tokeny jsou generovány ze zdrojového kódu v části; a předběžné zpracování direktiv v části musí být lexikálně správná, ale nebudou zpracovány jinak. V rámci *conditional_section* , který je zpracováván jako *skipped_section*jakékoli vnořené *conditional_section*s (obsažené ve vnořených `#if`... `#endif` a `#region`... `#endregion` vytvoří) zpracovávají jako *skipped_section*s.

Následující příklad ukazuje, jak Podmíněná kompilace můžete vnořit direktivy:
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

S výjimkou direktivy předběžného zpracování, přeskočené zdrojový kód není lze provést lexikální analýzu. Například následující platí bez ohledu na Neukončený komentář `#else` části:
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

Všimněte si však, že jsou direktivy předběžného zpracování musí být lexikálně správné i v bylo přeskočeno částí zdrojového kódu.

Direktivy předběžného zpracování nejsou zpracovány, když se zobrazí uvnitř Víceřádkový vstupní prvky. Například program:
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
výsledky ve výstupu:
```
hello,
#if Debug
        world
#else
        Nebraska
#endif
```

V případech specifické sadu direktivy předběžného zpracování, která je zpracována může záviset na vyhodnocení *pp_expression*. Příklad:
```csharp
#if X
    /*
#else
    /* */ class Q { }
#endif
```
vždy vytváří stejný token datového proudu (`class` `Q` `{` `}`), bez ohledu na to, jestli `X` je definována. Pokud `X` je definován, jsou pouze zpracovaných direktivy `#if` a `#endif`z důvodu víceřádkových komentářů. Pokud `X` je nedefinovaný, pak tři direktivy (`#if`, `#else`, `#endif`) jsou součástí sady direktiv.

### <a name="diagnostic-directives"></a>Diagnostické direktivy

Direktivy diagnostiky se používají k generovat explicitně chybové zprávy a upozornění, které jsou hlášeny stejným způsobem jako jiné chyby při kompilaci a upozornění.

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
vždy vyvolá upozornění ("revize kódu potřebný před vrácení se změnami") a způsobí chybu kompilace ("sestavení nemůže být ladění a maloobchodní licence") Pokud podmíněné symboly `Debug` a `Retail` jsou definovány. Všimněte si, že *pp_message* může obsahovat libovolný text, konkrétně, nemusí znázorněno jednoduché uvozovky klíčové slovo obsahovat tokenů ve správném formátu, `can't`.

### <a name="region-directives"></a>Direktivy oblastí

Direktivy oblastí se používají k explicitně označit oblasti zdrojového kódu.

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

Žádné sémantický význam je připojen k oblasti; oblasti jsou určeny k použití programátor nebo automatizované nástroje pro označení část zdrojového kódu. Zprávu určenou ve `#region` nebo `#endregion` – direktiva podobně nemá žádný sémantický význam; slouží jenom k identifikaci oblasti. Odpovídající `#region` a `#endregion` direktivy mohou mít různé *pp_message*s.

Lexikální zpracování oblasti:
```csharp
#region
...
#endregion
```
odpovídá přesně lexikální zpracování direktivy podmíněné kompilace ve tvaru:
```csharp
#if true
...
#endif
```

### <a name="line-directives"></a>Direktivy Line

Direktivy Line slouží ke změně čísla řádku a názvy zdrojových souborů, které jsou hlášeny sadou kompilátoru ve výstupu, jako je například upozornění a chyby a které jsou používány atributy informace o volajícím ([atributy informace o volajícím](attributes.md#caller-info-attributes)).

Direktivy line se běžně používají v meta programovací nástroje, které generují zdrojový kód C# z některých jiných textový vstup.

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

Pokud ne `#line` direktivy jsou k dispozici, kompilátor oznámí čísla řádků true a názvů zdrojových souborů v jeho výstup. Při zpracování `#line` direktiva, která zahrnuje *line_indicator* , který není `default`, kompilátor považuje za direktivou tak, že má zadaná čísla řádků (a název souboru, je-li zadán) na řádku.

A `#line default` obrátí účinek metody všechny předchozí direktiv #line – direktiva. Kompilátor sestavy true řádku informace pro následné řádky, přesně jak když není `#line` zpracoval direktivy.

A `#line hidden` – direktiva nemá žádný vliv na souboru a čísla řádků, které jsou uvedeny v chybové zprávy, ale neovlivňuje ladění na úrovni zdroje. Při ladění, všechny řádky mezi `#line hidden` směrnice a následné `#line` – direktiva (není `#line hidden`) mít žádné informace o číslech řádků. Při krokování kódu v ladicím programu, se přeskočí zcela tyto řádky.

Všimněte si, že *název_souboru* se liší od pravidelných řetězcový literál v tom, že řídicí znaky nejsou zpracovávány; "`\`" znak jednoduše určuje v rámci běžných lomítka *název_souboru*.

### <a name="pragma-directives"></a>Direktivy pragma

`#pragma` Direktiva předzpracování se používá k určení volitelné kontextové informace pro kompilátor. Informace poskytnuty v `#pragma` – direktiva se nikdy nezmění sémantiku programu.

```antlr
pp_pragma
    : whitespace? '#' whitespace? 'pragma' whitespace pragma_body pp_new_line
    ;

pragma_body
    : pragma_warning_body
    ;
```

Jazyk C# poskytuje `#pragma` direktivy řízení upozornění kompilátoru. Budoucí verze jazyka mohou zahrnovat další `#pragma` direktivy. Aby vzájemná funkční spolupráce s jinými kompilátory C#, kompilátor jazyka Microsoft C# bez vyvolání chyby při kompilaci pro neznámý `#pragma` direktivy, proveďte tyto direktivy ale generovat upozornění.

#### <a name="pragma-warning"></a>– Direktiva pragma upozornění

`#pragma warning` – Direktiva slouží k zakázání nebo obnovit všechny nebo konkrétní sadu upozornění zpráv během kompilace programu následující text.

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

A `#pragma warning` direktiva, která vynechává seznam upozornění ovlivňuje všechna upozornění. A `#pragma warning` – direktiva obsahuje seznam upozornění se týká pouze upozornění, které jsou uvedeny v seznamu.

A `#pragma warning disable` direktiv zakáže všechny nebo danou sadu upozornění.

A `#pragma warning restore` direktiv obnoví všechny nebo danou sadu upozornění na stav, který byl v platnosti na začátku kompilační jednotky. Všimněte si, že pokud konkrétní upozornění zakázal externě, `#pragma warning restore` (, jestli se pro všechny nebo konkrétního upozornění) nebude znovu povolit tohoto upozornění.

Následující příklad ukazuje použití `#pragma warning` dočasně zakázat upozornění oznamují zastaralé členy odkazují, pomocí čísla upozornění z kompilátoru jazyka Microsoft C#.
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
