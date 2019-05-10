---
ms.openlocfilehash: e103f6629a363c6cd76607699ff74d69aa73ed57
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/25/2019
ms.locfileid: "64488973"
---
# <a name="lexical-structure"></a><span data-ttu-id="9f966-101">Lexikální struktura</span><span class="sxs-lookup"><span data-stu-id="9f966-101">Lexical structure</span></span>

## <a name="programs"></a><span data-ttu-id="9f966-102">Programy</span><span class="sxs-lookup"><span data-stu-id="9f966-102">Programs</span></span>

<span data-ttu-id="9f966-103">C# ***program*** obsahuje jeden nebo více ***zdrojové soubory***známé formálně jako ***kompilačních jednotek*** ([kompilačních jednotek](namespaces.md#compilation-units)).</span><span class="sxs-lookup"><span data-stu-id="9f966-103">A C# ***program*** consists of one or more ***source files***, known formally as ***compilation units*** ([Compilation units](namespaces.md#compilation-units)).</span></span> <span data-ttu-id="9f966-104">Zdrojový soubor je seřazená posloupnost znaků Unicode.</span><span class="sxs-lookup"><span data-stu-id="9f966-104">A source file is an ordered sequence of Unicode characters.</span></span> <span data-ttu-id="9f966-105">Zdrojové soubory obvykle korespondovaly s soubory v systému souborů, ale tento korespondenci se nevyžaduje.</span><span class="sxs-lookup"><span data-stu-id="9f966-105">Source files typically have a one-to-one correspondence with files in a file system, but this correspondence is not required.</span></span> <span data-ttu-id="9f966-106">Pro maximální přenositelnost, se doporučuje, aby soubory v systému souborů být zakódován pomocí kódování UTF-8 kódování.</span><span class="sxs-lookup"><span data-stu-id="9f966-106">For maximal portability, it is recommended that files in a file system be encoded with the UTF-8 encoding.</span></span>

<span data-ttu-id="9f966-107">Obecně vzato programu je zkompilován pomocí tří kroků:</span><span class="sxs-lookup"><span data-stu-id="9f966-107">Conceptually speaking, a program is compiled using three steps:</span></span>

1. <span data-ttu-id="9f966-108">Transformace, která převede soubor z určitého znaku repertoáru a schéma kódování na sekvenci znaků Unicode.</span><span class="sxs-lookup"><span data-stu-id="9f966-108">Transformation, which converts a file from a particular character repertoire and encoding scheme into a sequence of Unicode characters.</span></span>
2. <span data-ttu-id="9f966-109">Lexikální analýzu, která převede datový proud vstupních znaků Unicode do datového proudu tokenů.</span><span class="sxs-lookup"><span data-stu-id="9f966-109">Lexical analysis, which translates a stream of Unicode input characters into a stream of tokens.</span></span>
3. <span data-ttu-id="9f966-110">Syntaktické analýzy, která převede datový proud s tokeny do spustitelného kódu.</span><span class="sxs-lookup"><span data-stu-id="9f966-110">Syntactic analysis, which translates the stream of tokens into executable code.</span></span>

## <a name="grammars"></a><span data-ttu-id="9f966-111">Gramatika</span><span class="sxs-lookup"><span data-stu-id="9f966-111">Grammars</span></span>

<span data-ttu-id="9f966-112">Tato specifikace uvede syntaxe jazyka C# programování pomocí dvou gramatiky.</span><span class="sxs-lookup"><span data-stu-id="9f966-112">This specification presents the syntax of the C# programming language using two grammars.</span></span> <span data-ttu-id="9f966-113">***Gramatika slov*** ([gramatika slov](lexical-structure.md#lexical-grammar)) definuje, jak jsou zkombinované znaků Unicode pro zakončení řádku formuláře, prázdné znaky, komentáře, tokenů a direktivy předběžného zpracování.</span><span class="sxs-lookup"><span data-stu-id="9f966-113">The ***lexical grammar*** ([Lexical grammar](lexical-structure.md#lexical-grammar)) defines how Unicode characters are combined to form line terminators, white space, comments, tokens, and pre-processing directives.</span></span> <span data-ttu-id="9f966-114">***Syntaktické gramatiky*** ([syntaktické gramatiky](lexical-structure.md#syntactic-grammar)) definuje, jak se tokeny vyplývající z gramatika slov zkombinovala, aby programy formuláře C#.</span><span class="sxs-lookup"><span data-stu-id="9f966-114">The ***syntactic grammar*** ([Syntactic grammar](lexical-structure.md#syntactic-grammar)) defines how the tokens resulting from the lexical grammar are combined to form C# programs.</span></span>

### <a name="grammar-notation"></a><span data-ttu-id="9f966-115">Zápis gramatiky</span><span class="sxs-lookup"><span data-stu-id="9f966-115">Grammar notation</span></span>

<span data-ttu-id="9f966-116">Gramatika slov a syntaktické jsou uvedeny v Backus-Naur formuláře pomocí notace nástroj ANTLR gramatiky.</span><span class="sxs-lookup"><span data-stu-id="9f966-116">The lexical and syntactic grammars are presented in Backus-Naur form using the notation of the ANTLR grammar tool.</span></span>

### <a name="lexical-grammar"></a><span data-ttu-id="9f966-117">Gramatika slov</span><span class="sxs-lookup"><span data-stu-id="9f966-117">Lexical grammar</span></span>

<span data-ttu-id="9f966-118">Gramatika slov jazyka C# je uveden v [provést lexikální analýzu](lexical-structure.md#lexical-analysis), [tokeny](lexical-structure.md#tokens), a [předběžné zpracování direktiv](lexical-structure.md#pre-processing-directives).</span><span class="sxs-lookup"><span data-stu-id="9f966-118">The lexical grammar of C# is presented in [Lexical analysis](lexical-structure.md#lexical-analysis), [Tokens](lexical-structure.md#tokens), and [Pre-processing directives](lexical-structure.md#pre-processing-directives).</span></span> <span data-ttu-id="9f966-119">Terminálu symboly gramatika slov jsou znaky znakové sady Unicode a gramatika slov Určuje, jak jsou zkombinované znaků na formulář tokeny ([tokeny](lexical-structure.md#tokens)), mezer ([prázdných](lexical-structure.md#white-space)), komentáře ([komentáře](lexical-structure.md#comments)) a direktivy předběžného zpracování ([předběžné zpracování direktiv](lexical-structure.md#pre-processing-directives)).</span><span class="sxs-lookup"><span data-stu-id="9f966-119">The terminal symbols of the lexical grammar are the characters of the Unicode character set, and the lexical grammar specifies how characters are combined to form tokens ([Tokens](lexical-structure.md#tokens)), white space ([White space](lexical-structure.md#white-space)), comments ([Comments](lexical-structure.md#comments)), and pre-processing directives ([Pre-processing directives](lexical-structure.md#pre-processing-directives)).</span></span>

<span data-ttu-id="9f966-120">Každý zdrojový soubor v programu v jazyce C# musí odpovídat *vstupní* produkční gramatika slov ([provést lexikální analýzu](lexical-structure.md#lexical-analysis)).</span><span class="sxs-lookup"><span data-stu-id="9f966-120">Every source file in a C# program must conform to the *input* production of the lexical grammar ([Lexical analysis](lexical-structure.md#lexical-analysis)).</span></span>

### <a name="syntactic-grammar"></a><span data-ttu-id="9f966-121">Syntaktické gramatiky</span><span class="sxs-lookup"><span data-stu-id="9f966-121">Syntactic grammar</span></span>

<span data-ttu-id="9f966-122">Syntaktické gramatika C# jsou zobrazena v kapitolách a přílohy, které následují této kapitole.</span><span class="sxs-lookup"><span data-stu-id="9f966-122">The syntactic grammar of C# is presented in the chapters and appendices that follow this chapter.</span></span> <span data-ttu-id="9f966-123">Terminálu symboly syntaktické gramatiky jsou tokeny definované gramatika slov a syntaktické gramatiky Určuje, jak se tokeny zkombinovala, aby programy jazyka C# formulářů.</span><span class="sxs-lookup"><span data-stu-id="9f966-123">The terminal symbols of the syntactic grammar are the tokens defined by the lexical grammar, and the syntactic grammar specifies how tokens are combined to form C# programs.</span></span>

<span data-ttu-id="9f966-124">Každý zdrojový soubor v C# programu musí odpovídat *compilation_unit* produkční syntaktické gramatiky ([kompilačních jednotek](namespaces.md#compilation-units)).</span><span class="sxs-lookup"><span data-stu-id="9f966-124">Every source file in a C# program must conform to the *compilation_unit* production of the syntactic grammar ([Compilation units](namespaces.md#compilation-units)).</span></span>

## <a name="lexical-analysis"></a><span data-ttu-id="9f966-125">Lexikální analýzu</span><span class="sxs-lookup"><span data-stu-id="9f966-125">Lexical analysis</span></span>

<span data-ttu-id="9f966-126">*Vstupní* produkční definuje lexikální struktura zdrojový soubor jazyka C#.</span><span class="sxs-lookup"><span data-stu-id="9f966-126">The *input* production defines the lexical structure of a C# source file.</span></span> <span data-ttu-id="9f966-127">Tato výroba gramatika slov musí odpovídat každý zdrojový soubor v programu v jazyce C#.</span><span class="sxs-lookup"><span data-stu-id="9f966-127">Each source file in a C# program must conform to this lexical grammar production.</span></span>

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

<span data-ttu-id="9f966-128">Pět základních prvků, které tvoří lexikální struktura C# zdrojový soubor: Řádek ukončovací znaky ([řádek ukončovací znaky](lexical-structure.md#line-terminators)), mezer ([prázdných](lexical-structure.md#white-space)), komentáře ([komentáře](lexical-structure.md#comments)), tokeny ([tokeny](lexical-structure.md#tokens)), a direktivy předběžného zpracování ([předběžné zpracování direktiv](lexical-structure.md#pre-processing-directives)).</span><span class="sxs-lookup"><span data-stu-id="9f966-128">Five basic elements make up the lexical structure of a C# source file: Line terminators ([Line terminators](lexical-structure.md#line-terminators)), white space ([White space](lexical-structure.md#white-space)), comments ([Comments](lexical-structure.md#comments)), tokens ([Tokens](lexical-structure.md#tokens)), and pre-processing directives ([Pre-processing directives](lexical-structure.md#pre-processing-directives)).</span></span> <span data-ttu-id="9f966-129">Tyto základní prvky pouze tokeny jsou rozhodující pro syntaktické gramatiky programu v jazyce C# ([syntaktické gramatiky](lexical-structure.md#syntactic-grammar)).</span><span class="sxs-lookup"><span data-stu-id="9f966-129">Of these basic elements, only tokens are significant in the syntactic grammar of a C# program ([Syntactic grammar](lexical-structure.md#syntactic-grammar)).</span></span>

<span data-ttu-id="9f966-130">Lexikální zpracování zdrojový soubor jazyka C# se skládá z snížení soubor do sekvence tokenů, který bude vstup pro syntaktické analýzy.</span><span class="sxs-lookup"><span data-stu-id="9f966-130">The lexical processing of a C# source file consists of reducing the file into a sequence of tokens which becomes the input to the syntactic analysis.</span></span> <span data-ttu-id="9f966-131">Řádku, prázdné znaky a komentáře, může sloužit k oddělení tokeny a direktivy předběžného zpracování může způsobit části zdrojového souboru přeskočit, ale jinak tyto Lexikální prvky mít vliv na syntaktické struktura programu v jazyce C#.</span><span class="sxs-lookup"><span data-stu-id="9f966-131">Line terminators, white space, and comments can serve to separate tokens, and pre-processing directives can cause sections of the source file to be skipped, but otherwise these lexical elements have no impact on the syntactic structure of a C# program.</span></span>

<span data-ttu-id="9f966-132">V případě interpolovaný řetězec literálů ([interpolované řetězce literálů](lexical-structure.md#interpolated-string-literals)) do jednoho tokenu původně vytvořil provést lexikální analýzu, ale je rozdělený do několika vstupních elementy, které jsou opakovaně podroben provést lexikální analýzu dokud všechny literály interpolovaném řetězci byly vyřešeny.</span><span class="sxs-lookup"><span data-stu-id="9f966-132">In the case of interpolated string literals ([Interpolated string literals](lexical-structure.md#interpolated-string-literals)) a single token is initially produced by lexical analysis, but is broken up into several input elements which are repeatedly subjected to lexical analysis until all interpolated string literals have been resolved.</span></span> <span data-ttu-id="9f966-133">Použitím výsledných tokenů, sloužit jako vstup pro syntaktické analýzy.</span><span class="sxs-lookup"><span data-stu-id="9f966-133">The resulting tokens then serve as input to the syntactic analysis.</span></span>

<span data-ttu-id="9f966-134">Když několik výroby gramatika slov odpovídají pořadí znaků ve zdrojovém souboru, lexikální se vždy zpracování forms nejdelší možný lexikální elementu.</span><span class="sxs-lookup"><span data-stu-id="9f966-134">When several lexical grammar productions match a sequence of characters in a source file, the lexical processing always forms the longest possible lexical element.</span></span> <span data-ttu-id="9f966-135">Například sekvence znaků `//` je zpracovat jako začátek jednořádkový komentář, protože je delší než jeden tento element lexikální `/` token.</span><span class="sxs-lookup"><span data-stu-id="9f966-135">For example, the character sequence `//` is processed as the beginning of a single-line comment because that lexical element is longer than a single `/` token.</span></span>

### <a name="line-terminators"></a><span data-ttu-id="9f966-136">Řádku</span><span class="sxs-lookup"><span data-stu-id="9f966-136">Line terminators</span></span>

<span data-ttu-id="9f966-137">Řádku rozdělit řádky znaků zdrojový soubor jazyka C#.</span><span class="sxs-lookup"><span data-stu-id="9f966-137">Line terminators divide the characters of a C# source file into lines.</span></span>

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

<span data-ttu-id="9f966-138">Pro kompatibilitu s zdrojový kód nástroje pro úpravy, které aplikacím dodávají endovém souborovém značky a umožňuje zdroji soubor prohlížení jako posloupnost správně ukončena řádky, se použijí následující transformace, aby každý zdrojový soubor v programu v jazyce C#:</span><span class="sxs-lookup"><span data-stu-id="9f966-138">For compatibility with source code editing tools that add end-of-file markers, and to enable a source file to be viewed as a sequence of properly terminated lines, the following transformations are applied, in order, to every source file in a C# program:</span></span>

*  <span data-ttu-id="9f966-139">Pokud poslední znak zdrojového souboru, je znak Z ovládacího prvku (`U+001A`), se odstraní tento znak.</span><span class="sxs-lookup"><span data-stu-id="9f966-139">If the last character of the source file is a Control-Z character (`U+001A`), this character is deleted.</span></span>
*  <span data-ttu-id="9f966-140">Znak pro návrat na začátek řádku (`U+000D`) se přidá do konce zdrojového souboru, pokud se tento zdrojový soubor je prázdný, a pokud poslední znak zdrojového souboru není zalomení řádku (`U+000D`), odřádkování (`U+000A`), oddělovač řádků (`U+2028`), nebo oddělovač odstavců (`U+2029`).</span><span class="sxs-lookup"><span data-stu-id="9f966-140">A carriage-return character (`U+000D`) is added to the end of the source file if that source file is non-empty and if the last character of the source file is not a carriage return (`U+000D`), a line feed (`U+000A`), a line separator (`U+2028`), or a paragraph separator (`U+2029`).</span></span>

### <a name="comments"></a><span data-ttu-id="9f966-141">Komentáře</span><span class="sxs-lookup"><span data-stu-id="9f966-141">Comments</span></span>

<span data-ttu-id="9f966-142">Jsou podporovány dvě formy komentáře: Jednořádkové komentáře a poznámky s oddělovači.</span><span class="sxs-lookup"><span data-stu-id="9f966-142">Two forms of comments are supported: single-line comments and delimited comments.</span></span> <span data-ttu-id="9f966-143">***Jednořádkové komentáře*** začínají znaky `//` a rozšířit na konec řádku zdroje.</span><span class="sxs-lookup"><span data-stu-id="9f966-143">***Single-line comments*** start with the characters `//` and extend to the end of the source line.</span></span> <span data-ttu-id="9f966-144">***Oddělené komentáře*** začínají znaky `/*` a končit znaky `*/`.</span><span class="sxs-lookup"><span data-stu-id="9f966-144">***Delimited comments*** start with the characters `/*` and end with the characters `*/`.</span></span> <span data-ttu-id="9f966-145">Komentáře s oddělovači může zahrnovat více řádků.</span><span class="sxs-lookup"><span data-stu-id="9f966-145">Delimited comments may span multiple lines.</span></span>

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

<span data-ttu-id="9f966-146">Komentáře nelze vnořovat.</span><span class="sxs-lookup"><span data-stu-id="9f966-146">Comments do not nest.</span></span> <span data-ttu-id="9f966-147">Sekvence znaků `/*` a `*/` nemají zvláštní význam v rámci `//` komentář a znakovými sekvencemi `//` a `/*` nemají zvláštní význam v rámci oddělovači komentář.</span><span class="sxs-lookup"><span data-stu-id="9f966-147">The character sequences `/*` and `*/` have no special meaning within a `//` comment, and the character sequences `//` and `/*` have no special meaning within a delimited comment.</span></span>

<span data-ttu-id="9f966-148">V rámci znakové a řetězcové literály nejsou zpracovány poznámky.</span><span class="sxs-lookup"><span data-stu-id="9f966-148">Comments are not processed within character and string literals.</span></span>

<span data-ttu-id="9f966-149">V příkladu</span><span class="sxs-lookup"><span data-stu-id="9f966-149">The example</span></span>
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
<span data-ttu-id="9f966-150">obsahuje komentář s oddělovači.</span><span class="sxs-lookup"><span data-stu-id="9f966-150">includes a delimited comment.</span></span>

<span data-ttu-id="9f966-151">V příkladu</span><span class="sxs-lookup"><span data-stu-id="9f966-151">The example</span></span>
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
<span data-ttu-id="9f966-152">ukazuje několik Jednořádkové komentáře.</span><span class="sxs-lookup"><span data-stu-id="9f966-152">shows several single-line comments.</span></span>

### <a name="white-space"></a><span data-ttu-id="9f966-153">Prázdné znaky</span><span class="sxs-lookup"><span data-stu-id="9f966-153">White space</span></span>

<span data-ttu-id="9f966-154">Prázdné místo je definována jako libovolný znak Unicode třída Zs (která obsahuje znak mezery) a znak horizontálního tabulátoru, znak svislého tabulátoru a formulář se znakem kanálu.</span><span class="sxs-lookup"><span data-stu-id="9f966-154">White space is defined as any character with Unicode class Zs (which includes the space character) as well as the horizontal tab character, the vertical tab character, and the form feed character.</span></span>

```antlr
whitespace
    : '<Any character with Unicode class Zs>'
    | '<Horizontal tab character (U+0009)>'
    | '<Vertical tab character (U+000B)>'
    | '<Form feed character (U+000C)>'
    ;
```

## <a name="tokens"></a><span data-ttu-id="9f966-155">Tokeny</span><span class="sxs-lookup"><span data-stu-id="9f966-155">Tokens</span></span>

<span data-ttu-id="9f966-156">Existuje několik druhů tokenů: identifikátory, klíčová slova, literály, operátory a interpunkční znaky.</span><span class="sxs-lookup"><span data-stu-id="9f966-156">There are several kinds of tokens: identifiers, keywords, literals, operators, and punctuators.</span></span> <span data-ttu-id="9f966-157">Prázdné znaky a komentáře nejsou tokeny, i když působí jako oddělovače pro tokeny.</span><span class="sxs-lookup"><span data-stu-id="9f966-157">White space and comments are not tokens, though they act as separators for tokens.</span></span>

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

### <a name="unicode-character-escape-sequences"></a><span data-ttu-id="9f966-158">Znak – řídicí sekvence Unicode</span><span class="sxs-lookup"><span data-stu-id="9f966-158">Unicode character escape sequences</span></span>

<span data-ttu-id="9f966-159">Znak řídicí sekvence Unicode hodnota představuje znak Unicode.</span><span class="sxs-lookup"><span data-stu-id="9f966-159">A Unicode character escape sequence represents a Unicode character.</span></span> <span data-ttu-id="9f966-160">Znak – řídicí sekvence Unicode se zpracovávají v identifikátorech ([identifikátory](lexical-structure.md#identifiers)), znakové literály ([literály znaků](lexical-structure.md#character-literals)) a pravidelné řetězcových literálů ([řetězcových literálů](lexical-structure.md#string-literals)).</span><span class="sxs-lookup"><span data-stu-id="9f966-160">Unicode character escape sequences are processed in identifiers ([Identifiers](lexical-structure.md#identifiers)), character literals ([Character literals](lexical-structure.md#character-literals)), and regular string literals ([String literals](lexical-structure.md#string-literals)).</span></span> <span data-ttu-id="9f966-161">Řídicí znak Unicode není zpracována v jiném umístění (například formulář operátor, interpunkci nebo – klíčové slovo).</span><span class="sxs-lookup"><span data-stu-id="9f966-161">A Unicode character escape is not processed in any other location (for example, to form an operator, punctuator, or keyword).</span></span>

```antlr
unicode_escape_sequence
    : '\\u' hex_digit hex_digit hex_digit hex_digit
    | '\\U' hex_digit hex_digit hex_digit hex_digit hex_digit hex_digit hex_digit hex_digit
    ;
```

<span data-ttu-id="9f966-162">Řídicí sekvence Unicode představuje jeden znak Unicode tvořen šestnáctkové číslo následující "`\u`"nebo"`\U`" znaků.</span><span class="sxs-lookup"><span data-stu-id="9f966-162">A Unicode escape sequence represents the single Unicode character formed by the hexadecimal number following the "`\u`" or "`\U`" characters.</span></span> <span data-ttu-id="9f966-163">Protože jazyk C# používá 16 bitů kódování kódové body sady Unicode v znaky a řetězcové hodnoty, znak Unicode v rozsahu U + 10000 až U + 10FFFF není povolená ve znakového literálu a se vyjadřuje náhradní pár kódu Unicode v řetězcovém literálu.</span><span class="sxs-lookup"><span data-stu-id="9f966-163">Since C# uses a 16-bit encoding of Unicode code points in characters and string values, a Unicode character in the range U+10000 to U+10FFFF is not permitted in a character literal and is represented using a Unicode surrogate pair in a string literal.</span></span> <span data-ttu-id="9f966-164">Znaky Unicode s body kódu nad 0x10FFFF nejsou podporovány.</span><span class="sxs-lookup"><span data-stu-id="9f966-164">Unicode characters with code points above 0x10FFFF are not supported.</span></span>

<span data-ttu-id="9f966-165">Více posunutí se neprovádí.</span><span class="sxs-lookup"><span data-stu-id="9f966-165">Multiple translations are not performed.</span></span> <span data-ttu-id="9f966-166">Například textový literál "`\u005Cu005C`"je ekvivalentní k"`\u005C`"místo"`\`".</span><span class="sxs-lookup"><span data-stu-id="9f966-166">For instance, the string literal "`\u005Cu005C`" is equivalent to "`\u005C`" rather than "`\`".</span></span> <span data-ttu-id="9f966-167">Hodnota Unicode `\u005C` je znak "`\`".</span><span class="sxs-lookup"><span data-stu-id="9f966-167">The Unicode value `\u005C` is the character "`\`".</span></span>

<span data-ttu-id="9f966-168">V příkladu</span><span class="sxs-lookup"><span data-stu-id="9f966-168">The example</span></span>
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
<span data-ttu-id="9f966-169">ukazuje použití několika `\u0066`, což je řídicí sekvence pro písmeno "`f`".</span><span class="sxs-lookup"><span data-stu-id="9f966-169">shows several uses of `\u0066`, which is the escape sequence for the letter "`f`".</span></span> <span data-ttu-id="9f966-170">Program je ekvivalentní</span><span class="sxs-lookup"><span data-stu-id="9f966-170">The program is equivalent to</span></span>
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

### <a name="identifiers"></a><span data-ttu-id="9f966-171">Identifikátory</span><span class="sxs-lookup"><span data-stu-id="9f966-171">Identifiers</span></span>

<span data-ttu-id="9f966-172">Pravidla pro identifikátory, které jsou uvedené v této části přesně odpovídat doporučené Unicode Standard Annex 31, s tím rozdílem, že podtržítka je povolen jako počáteční znak (jako je tradiční v programovacím jazyce C), řídicí sekvence Unicode povolené v identifikátorech a "`@`" znak je povolený jako předponu povolit klíčová slova používat jako identifikátory.</span><span class="sxs-lookup"><span data-stu-id="9f966-172">The rules for identifiers given in this section correspond exactly to those recommended by the Unicode Standard Annex 31, except that underscore is allowed as an initial character (as is traditional in the C programming language), Unicode escape sequences are permitted in identifiers, and the "`@`" character is allowed as a prefix to enable keywords to be used as identifiers.</span></span>

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

<span data-ttu-id="9f966-173">Informace o třídách znaků Unicode uvedených výše viz standardu Unicode, verze 3.0, bod 4.5.</span><span class="sxs-lookup"><span data-stu-id="9f966-173">For information on the Unicode character classes mentioned above, see The Unicode Standard, Version 3.0, section 4.5.</span></span>

<span data-ttu-id="9f966-174">Příklady platné identifikátory "`identifier1`","`_identifier2`", a "`@if`".</span><span class="sxs-lookup"><span data-stu-id="9f966-174">Examples of valid identifiers include "`identifier1`", "`_identifier2`", and "`@if`".</span></span>

<span data-ttu-id="9f966-175">Identifikátor vyhovující aplikace musí být v kanonickém formátu definovaném v normalizačním formuláři Unicode C, jak jsou definovány v Unicode Standard Annex 15.</span><span class="sxs-lookup"><span data-stu-id="9f966-175">An identifier in a conforming program must be in the canonical format defined by Unicode Normalization Form C, as defined by Unicode Standard Annex 15.</span></span> <span data-ttu-id="9f966-176">Chování při zjištění identifikátor není v normalizace formuláře C, je definováno implementací; Diagnostika však není povinné.</span><span class="sxs-lookup"><span data-stu-id="9f966-176">The behavior when encountering an identifier not in Normalization Form C is implementation-defined; however, a diagnostic is not required.</span></span>

<span data-ttu-id="9f966-177">Předpona "`@`" umožňuje použít klíčová slova jako identifikátory, což je užitečné při propojování s jinými programovací jazyky.</span><span class="sxs-lookup"><span data-stu-id="9f966-177">The prefix "`@`" enables the use of keywords as identifiers, which is useful when interfacing with other programming languages.</span></span> <span data-ttu-id="9f966-178">Znak `@` není ve skutečnosti součástí identifikátoru, a proto identifikátor může zobrazit v jiných jazycích jako normální identifikátor, bez předpony.</span><span class="sxs-lookup"><span data-stu-id="9f966-178">The character `@` is not actually part of the identifier, so the identifier might be seen in other languages as a normal identifier, without the prefix.</span></span> <span data-ttu-id="9f966-179">Identifikátor s `@` předpona je volána ***Doslovný identifikátor***.</span><span class="sxs-lookup"><span data-stu-id="9f966-179">An identifier with an `@` prefix is called a ***verbatim identifier***.</span></span> <span data-ttu-id="9f966-180">Použití `@` předpona pro identifikátory, které nejsou klíčových slov je povolené, ale důrazně nedoporučuje jak stylu.</span><span class="sxs-lookup"><span data-stu-id="9f966-180">Use of the `@` prefix for identifiers that are not keywords is permitted, but strongly discouraged as a matter of style.</span></span>

<span data-ttu-id="9f966-181">Příklad:</span><span class="sxs-lookup"><span data-stu-id="9f966-181">The example:</span></span>
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
<span data-ttu-id="9f966-182">definuje třídu s názvem "`class`"se statickou metodu s názvem"`static`", která přebírá parametr s názvem"`bool`".</span><span class="sxs-lookup"><span data-stu-id="9f966-182">defines a class named "`class`" with a static method named "`static`" that takes a parameter named "`bool`".</span></span> <span data-ttu-id="9f966-183">Všimněte si, že vzhledem k tomu, že řídicí sekvence Unicode nejsou povolené v klíčových slov, token "`cl\u0061ss`"je identifikátor, a je stejný identifikátor jako"`@class`".</span><span class="sxs-lookup"><span data-stu-id="9f966-183">Note that since Unicode escapes are not permitted in keywords, the token "`cl\u0061ss`" is an identifier, and is the same identifier as "`@class`".</span></span>

<span data-ttu-id="9f966-184">Dva identifikátory jsou považovány za totéž případě, že jsou identické, až tyto transformace jsou použity v zadaném pořadí:</span><span class="sxs-lookup"><span data-stu-id="9f966-184">Two identifiers are considered the same if they are identical after the following transformations are applied, in order:</span></span>

*  <span data-ttu-id="9f966-185">Předpona "`@`", pokud použijete, se odebere.</span><span class="sxs-lookup"><span data-stu-id="9f966-185">The prefix "`@`", if used, is removed.</span></span>
*  <span data-ttu-id="9f966-186">Každý *unicode_escape_sequence* se transformuje na jeho odpovídající znak Unicode.</span><span class="sxs-lookup"><span data-stu-id="9f966-186">Each *unicode_escape_sequence* is transformed into its corresponding Unicode character.</span></span>
*  <span data-ttu-id="9f966-187">Žádné *formatting_character*byly odebrány.</span><span class="sxs-lookup"><span data-stu-id="9f966-187">Any *formatting_character*s are removed.</span></span>

<span data-ttu-id="9f966-188">Identifikátory obsahující dvě po sobě jdoucí znaky podtržení (`U+005F`) jsou vyhrazené pro použití v implementaci.</span><span class="sxs-lookup"><span data-stu-id="9f966-188">Identifiers containing two consecutive underscore characters (`U+005F`) are reserved for use by the implementation.</span></span> <span data-ttu-id="9f966-189">Implementace může například poskytovat klíčových slov rozšířených začínající dvěma podtržítky.</span><span class="sxs-lookup"><span data-stu-id="9f966-189">For example, an implementation might provide extended keywords that begin with two underscores.</span></span>

### <a name="keywords"></a><span data-ttu-id="9f966-190">klíčová slova</span><span class="sxs-lookup"><span data-stu-id="9f966-190">Keywords</span></span>

<span data-ttu-id="9f966-191">A ***– klíčové slovo*** je identifikátor jako posloupnost znaků, který je vyhrazen a nelze jej použít jako identifikátoru s výjimkou při uvedena ve `@` znak.</span><span class="sxs-lookup"><span data-stu-id="9f966-191">A ***keyword*** is an identifier-like sequence of characters that is reserved, and cannot be used as an identifier except when prefaced by the `@` character.</span></span>

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

<span data-ttu-id="9f966-192">V některých místech v gramatice zvláštní identifikátory mají zvláštní význam, ale nejsou klíčová slova.</span><span class="sxs-lookup"><span data-stu-id="9f966-192">In some places in the grammar, specific identifiers have special meaning, but are not keywords.</span></span> <span data-ttu-id="9f966-193">Tyto identifikátory jsou někdy označovány jako "kontextová klíčová slova".</span><span class="sxs-lookup"><span data-stu-id="9f966-193">Such identifiers are sometimes referred to as "contextual keywords".</span></span> <span data-ttu-id="9f966-194">Například v deklaraci vlastnosti "`get`"a"`set`" mají zvláštní význam identifikátory ([přistupující objekty](classes.md#accessors)).</span><span class="sxs-lookup"><span data-stu-id="9f966-194">For example, within a property declaration, the "`get`" and "`set`" identifiers have special meaning ([Accessors](classes.md#accessors)).</span></span> <span data-ttu-id="9f966-195">Identifikátor jiných než `get` nebo `set` nikdy smí v těchto umístěních, takže toto použití nejsou v konfliktu s využitím tato slova jako identifikátory.</span><span class="sxs-lookup"><span data-stu-id="9f966-195">An identifier other than `get` or `set` is never permitted in these locations, so this use does not conflict with a use of these words as identifiers.</span></span> <span data-ttu-id="9f966-196">V ostatních případech, například stejně jako identifikátor "`var`" v deklaracích implicitně typované lokální proměnné ([místní deklarace proměnné](statements.md#local-variable-declarations)), kontextové klíčové slovo může dojít ke konfliktu s názvy deklarované.</span><span class="sxs-lookup"><span data-stu-id="9f966-196">In other cases, such as with the identifier "`var`" in implicitly typed local variable declarations ([Local variable declarations](statements.md#local-variable-declarations)), a contextual keyword can conflict with declared names.</span></span> <span data-ttu-id="9f966-197">V takových případech deklarovaný název má přednost před použití identifikátor jako kontextové klíčové slovo.</span><span class="sxs-lookup"><span data-stu-id="9f966-197">In such cases, the declared name takes precedence over the use of the identifier as a contextual keyword.</span></span>

### <a name="literals"></a><span data-ttu-id="9f966-198">Literály</span><span class="sxs-lookup"><span data-stu-id="9f966-198">Literals</span></span>

<span data-ttu-id="9f966-199">A ***literálu*** je reprezentace hodnotu zdrojového kódu.</span><span class="sxs-lookup"><span data-stu-id="9f966-199">A ***literal*** is a source code representation of a value.</span></span>

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

#### <a name="boolean-literals"></a><span data-ttu-id="9f966-200">Logické hodnoty literálu</span><span class="sxs-lookup"><span data-stu-id="9f966-200">Boolean literals</span></span>

<span data-ttu-id="9f966-201">Existují dvě logické hodnoty literálu: `true` a `false`.</span><span class="sxs-lookup"><span data-stu-id="9f966-201">There are two boolean literal values: `true` and `false`.</span></span>

```antlr
boolean_literal
    : 'true'
    | 'false'
    ;
```

<span data-ttu-id="9f966-202">Typ *boolean_literal* je `bool`.</span><span class="sxs-lookup"><span data-stu-id="9f966-202">The type of a *boolean_literal* is `bool`.</span></span>

#### <a name="integer-literals"></a><span data-ttu-id="9f966-203">Literály celých čísel</span><span class="sxs-lookup"><span data-stu-id="9f966-203">Integer literals</span></span>

<span data-ttu-id="9f966-204">Literály celých čísel se používá k zápisu hodnoty typů `int`, `uint`, `long`, a `ulong`.</span><span class="sxs-lookup"><span data-stu-id="9f966-204">Integer literals are used to write values of types `int`, `uint`, `long`, and `ulong`.</span></span> <span data-ttu-id="9f966-205">Literály celých čísel mít dva možné typy: desetinných míst a šestnáctková.</span><span class="sxs-lookup"><span data-stu-id="9f966-205">Integer literals have two possible forms: decimal and hexadecimal.</span></span>

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

<span data-ttu-id="9f966-206">Typ celočíselného literálu je stanoven následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="9f966-206">The type of an integer literal is determined as follows:</span></span>

*  <span data-ttu-id="9f966-207">Pokud literálu bez přípony, má první z těchto typů, ve kterých může být reprezentována jeho hodnota: `int`, `uint`, `long`, `ulong`.</span><span class="sxs-lookup"><span data-stu-id="9f966-207">If the literal has no suffix, it has the first of these types in which its value can be represented: `int`, `uint`, `long`, `ulong`.</span></span>
*  <span data-ttu-id="9f966-208">Pokud je literál příponou podle `U` nebo `u`, má první z těchto typů, ve kterých může být reprezentována jeho hodnota: `uint`, `ulong`.</span><span class="sxs-lookup"><span data-stu-id="9f966-208">If the literal is suffixed by `U` or `u`, it has the first of these types in which its value can be represented: `uint`, `ulong`.</span></span>
*  <span data-ttu-id="9f966-209">Pokud je literál příponou podle `L` nebo `l`, má první z těchto typů, ve kterých může být reprezentována jeho hodnota: `long`, `ulong`.</span><span class="sxs-lookup"><span data-stu-id="9f966-209">If the literal is suffixed by `L` or `l`, it has the first of these types in which its value can be represented: `long`, `ulong`.</span></span>
*  <span data-ttu-id="9f966-210">Pokud je literál příponou podle `UL`, `Ul`, `uL`, `ul`, `LU`, `Lu`, `lU`, nebo `lu`, je typu `ulong`.</span><span class="sxs-lookup"><span data-stu-id="9f966-210">If the literal is suffixed by `UL`, `Ul`, `uL`, `ul`, `LU`, `Lu`, `lU`, or `lu`, it is of type `ulong`.</span></span>

<span data-ttu-id="9f966-211">Pokud hodnota reprezentovaná celočíselného literálu je mimo rozsah `ulong` zadejte, dojde k chybě kompilace.</span><span class="sxs-lookup"><span data-stu-id="9f966-211">If the value represented by an integer literal is outside the range of the `ulong` type, a compile-time error occurs.</span></span>

<span data-ttu-id="9f966-212">Jak styl, je určeno, který "`L`"měl použít namísto"`l`" při zápisu literály typu `long`, protože je snadno zaměnit písmeno "`l`"se číslice"`1`".</span><span class="sxs-lookup"><span data-stu-id="9f966-212">As a matter of style, it is suggested that "`L`" be used instead of "`l`" when writing literals of type `long`, since it is easy to confuse the letter "`l`" with the digit "`1`".</span></span>

<span data-ttu-id="9f966-213">Tak, aby povolovala nejmenší `int` a `long` hodnot má být zapsán jako literály desítkové celé číslo, existují následující dvě pravidla:</span><span class="sxs-lookup"><span data-stu-id="9f966-213">To permit the smallest possible `int` and `long` values to be written as decimal integer literals, the following two rules exist:</span></span>

* <span data-ttu-id="9f966-214">Když *decimal_integer_literal* s hodnotou 2147483648 (2 ^ 31) a ne *integer_type_suffix* se zobrazí jako token hned za unární operátor minus token operátoru ([Unární minus operátor](expressions.md#unary-minus-operator)), výsledkem je konstanta typu `int` s hodnotou -2147483648 (-2 ^ 31).</span><span class="sxs-lookup"><span data-stu-id="9f966-214">When a *decimal_integer_literal* with the value 2147483648 (2^31) and no *integer_type_suffix* appears as the token immediately following a unary minus operator token ([Unary minus operator](expressions.md#unary-minus-operator)), the result is a constant of type `int` with the value -2147483648 (-2^31).</span></span> <span data-ttu-id="9f966-215">V jiných situacích takové *decimal_integer_literal* je typu `uint`.</span><span class="sxs-lookup"><span data-stu-id="9f966-215">In all other situations, such a *decimal_integer_literal* is of type `uint`.</span></span>
* <span data-ttu-id="9f966-216">Když *decimal_integer_literal* s hodnotou 9223372036854775808 (2 ^ 63) a ne *integer_type_suffix* nebo *integer_type_suffix* `L` nebo `l` se zobrazí jako token hned za unární operátor minus token – operátor ([unární operátor minus](expressions.md#unary-minus-operator)), výsledkem je konstanta typu `long` s hodnotou -9223372036854775808 (-2 ^ 63).</span><span class="sxs-lookup"><span data-stu-id="9f966-216">When a *decimal_integer_literal* with the value 9223372036854775808 (2^63) and no *integer_type_suffix* or the *integer_type_suffix* `L` or `l` appears as the token immediately following a unary minus operator token ([Unary minus operator](expressions.md#unary-minus-operator)), the result is a constant of type `long` with the value -9223372036854775808 (-2^63).</span></span> <span data-ttu-id="9f966-217">V jiných situacích takové *decimal_integer_literal* je typu `ulong`.</span><span class="sxs-lookup"><span data-stu-id="9f966-217">In all other situations, such a *decimal_integer_literal* is of type `ulong`.</span></span>

#### <a name="real-literals"></a><span data-ttu-id="9f966-218">Skutečné literály</span><span class="sxs-lookup"><span data-stu-id="9f966-218">Real literals</span></span>

<span data-ttu-id="9f966-219">Skutečné literály se používají pro zápis hodnoty typů `float`, `double`, a `decimal`.</span><span class="sxs-lookup"><span data-stu-id="9f966-219">Real literals are used to write values of types `float`, `double`, and `decimal`.</span></span>

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

<span data-ttu-id="9f966-220">Pokud ne *real_type_suffix* není zadána, je typu literál reálného čísla `double`.</span><span class="sxs-lookup"><span data-stu-id="9f966-220">If no *real_type_suffix* is specified, the type of the real literal is `double`.</span></span> <span data-ttu-id="9f966-221">V opačném případě přípona reálný typ Určuje typ literál reálného čísla, následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="9f966-221">Otherwise, the real type suffix determines the type of the real literal, as follows:</span></span>

*  <span data-ttu-id="9f966-222">Literál reálného čísla příponou podle `F` nebo `f` je typu `float`.</span><span class="sxs-lookup"><span data-stu-id="9f966-222">A real literal suffixed by `F` or `f` is of type `float`.</span></span> <span data-ttu-id="9f966-223">Například literály `1f`, `1.5f`, `1e10f`, a `123.456F` jsou všechny typu `float`.</span><span class="sxs-lookup"><span data-stu-id="9f966-223">For example, the literals `1f`, `1.5f`, `1e10f`, and `123.456F` are all of type `float`.</span></span>
*  <span data-ttu-id="9f966-224">Literál reálného čísla příponou podle `D` nebo `d` je typu `double`.</span><span class="sxs-lookup"><span data-stu-id="9f966-224">A real literal suffixed by `D` or `d` is of type `double`.</span></span> <span data-ttu-id="9f966-225">Například literály `1d`, `1.5d`, `1e10d`, a `123.456D` jsou všechny typu `double`.</span><span class="sxs-lookup"><span data-stu-id="9f966-225">For example, the literals `1d`, `1.5d`, `1e10d`, and `123.456D` are all of type `double`.</span></span>
*  <span data-ttu-id="9f966-226">Literál reálného čísla příponou podle `M` nebo `m` je typu `decimal`.</span><span class="sxs-lookup"><span data-stu-id="9f966-226">A real literal suffixed by `M` or `m` is of type `decimal`.</span></span> <span data-ttu-id="9f966-227">Například literály `1m`, `1.5m`, `1e10m`, a `123.456M` jsou všechny typu `decimal`.</span><span class="sxs-lookup"><span data-stu-id="9f966-227">For example, the literals `1m`, `1.5m`, `1e10m`, and `123.456M` are all of type `decimal`.</span></span> <span data-ttu-id="9f966-228">Tato literál je převedena na `decimal` hodnotu tak, že přesná hodnota, v případě potřeby zaokrouhlení na nejbližší reprezentovatelnou hodnotu pomocí společnosti bankovní zaokrouhlení ([typem decimal](types.md#the-decimal-type)).</span><span class="sxs-lookup"><span data-stu-id="9f966-228">This literal is converted to a `decimal` value by taking the exact value, and, if necessary, rounding to the nearest representable value using banker's rounding ([The decimal type](types.md#the-decimal-type)).</span></span> <span data-ttu-id="9f966-229">Libovolného rozsahu zřejmé v literálu se zachovají, pokud je tato hodnota zaokrouhlena nebo hodnota je nula (v tom druhém případě přihlašování a škálování bude 0).</span><span class="sxs-lookup"><span data-stu-id="9f966-229">Any scale apparent in the literal is preserved unless the value is rounded or the value is zero (in which latter case the sign and scale will be 0).</span></span> <span data-ttu-id="9f966-230">Proto je literál `2.900m` tvoří desetinné čárky se znaménkem, bude analyzována `0`, koeficient `2900`a škálování `3`.</span><span class="sxs-lookup"><span data-stu-id="9f966-230">Hence, the literal `2.900m` will be parsed to form the decimal with sign `0`, coefficient `2900`, and scale `3`.</span></span>

<span data-ttu-id="9f966-231">Pokud v zvolený typ nelze reprezentovat zadaný literál, dojde k chybě v době kompilace.</span><span class="sxs-lookup"><span data-stu-id="9f966-231">If the specified literal cannot be represented in the indicated type, a compile-time error occurs.</span></span>

<span data-ttu-id="9f966-232">Hodnota literál reálného čísla typu `float` nebo `double` je určit pomocí standardu IEEE "zaokrouhlí na nejbližší" režimu.</span><span class="sxs-lookup"><span data-stu-id="9f966-232">The value of a real literal of type `float` or `double` is determined by using the IEEE "round to nearest" mode.</span></span>

<span data-ttu-id="9f966-233">Všimněte si, že v literál reálného čísla, desítkových číslic jsou vždy požadované za desetinnou čárkou.</span><span class="sxs-lookup"><span data-stu-id="9f966-233">Note that in a real literal, decimal digits are always required after the decimal point.</span></span> <span data-ttu-id="9f966-234">Například `1.3F` je literál typu reálné číslo ale `1.F` není.</span><span class="sxs-lookup"><span data-stu-id="9f966-234">For example, `1.3F` is a real literal but `1.F` is not.</span></span>

#### <a name="character-literals"></a><span data-ttu-id="9f966-235">Znakové literály</span><span class="sxs-lookup"><span data-stu-id="9f966-235">Character literals</span></span>

<span data-ttu-id="9f966-236">Znakový literál představuje jeden znak a obvykle obsahuje znak do uvozovek, stejně jako v `'a'`.</span><span class="sxs-lookup"><span data-stu-id="9f966-236">A character literal represents a single character, and usually consists of a character in quotes, as in `'a'`.</span></span>

<span data-ttu-id="9f966-237">Poznámka: Zápis gramatiky ANTLR provede následující matoucí!</span><span class="sxs-lookup"><span data-stu-id="9f966-237">Note: The ANTLR grammar notation makes the following confusing!</span></span> <span data-ttu-id="9f966-238">V ANTLR při psaní `\'` zkratka pro jednoduchá uvozovka `'`.</span><span class="sxs-lookup"><span data-stu-id="9f966-238">In ANTLR, when you write `\'` it stands for a single quote `'`.</span></span> <span data-ttu-id="9f966-239">A při psaní `\\` zkratka pro jedno zpětné lomítko `\`.</span><span class="sxs-lookup"><span data-stu-id="9f966-239">And when you write `\\` it stands for a single backslash `\`.</span></span> <span data-ttu-id="9f966-240">Proto první pravidlo pro literální znak znamená, že začíná jednoduchá uvozovka, pak znak pak jednoduché uvozovky.</span><span class="sxs-lookup"><span data-stu-id="9f966-240">Therefore the first rule for a character literal means it starts with a single quote, then a character, then a single quote.</span></span> <span data-ttu-id="9f966-241">A jsou jedenáct možné jednoduchá řídící sekvence `\'`, `\"`, `\\`, `\0`, `\a`, `\b`, `\f`, `\n`, `\r`, `\t`, `\v`.</span><span class="sxs-lookup"><span data-stu-id="9f966-241">And the eleven possible simple escape sequences are `\'`, `\"`, `\\`, `\0`, `\a`, `\b`, `\f`, `\n`, `\r`, `\t`, `\v`.</span></span>

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

<span data-ttu-id="9f966-242">Znak, který následuje znak zpětného lomítka (`\`) v *znak* musí být jedna z následujících znaků: `'`, `"`, `\`, `0`, `a`, `b` , `f`, `n`, `r`, `t`, `u`, `U`, `x`, `v`.</span><span class="sxs-lookup"><span data-stu-id="9f966-242">A character that follows a backslash character (`\`) in a *character* must be one of the following characters: `'`, `"`, `\`, `0`, `a`, `b`, `f`, `n`, `r`, `t`, `u`, `U`, `x`, `v`.</span></span> <span data-ttu-id="9f966-243">V opačném případě dojde k chybě v době kompilace.</span><span class="sxs-lookup"><span data-stu-id="9f966-243">Otherwise, a compile-time error occurs.</span></span>

<span data-ttu-id="9f966-244">Šestnáctková řídicí sekvence představuje jeden znak Unicode s hodnotou tvořen šestnáctkové číslo následující "`\x`".</span><span class="sxs-lookup"><span data-stu-id="9f966-244">A hexadecimal escape sequence represents a single Unicode character, with the value formed by the hexadecimal number following "`\x`".</span></span>

<span data-ttu-id="9f966-245">Pokud je hodnota reprezentovaná literální znak větší než `U+FFFF`, dojde k chybě kompilace.</span><span class="sxs-lookup"><span data-stu-id="9f966-245">If the value represented by a character literal is greater than `U+FFFF`, a compile-time error occurs.</span></span>

<span data-ttu-id="9f966-246">Řídicí sekvence znaků Unicode ([znak – řídicí sekvence Unicode](lexical-structure.md#unicode-character-escape-sequences)) ve znakového literálu musí být v rozsahu `U+0000` k `U+FFFF`.</span><span class="sxs-lookup"><span data-stu-id="9f966-246">A Unicode character escape sequence ([Unicode character escape sequences](lexical-structure.md#unicode-character-escape-sequences)) in a character literal must be in the range `U+0000` to `U+FFFF`.</span></span>

<span data-ttu-id="9f966-247">Jednoduchá řídící sekvence představuje kódování Unicode znaku, jak je popsáno v následující tabulce.</span><span class="sxs-lookup"><span data-stu-id="9f966-247">A simple escape sequence represents a Unicode character encoding, as described in the table below.</span></span>


| <span data-ttu-id="9f966-248">__Řídicí sekvence__</span><span class="sxs-lookup"><span data-stu-id="9f966-248">__Escape sequence__</span></span> | <span data-ttu-id="9f966-249">__Název znaku__</span><span class="sxs-lookup"><span data-stu-id="9f966-249">__Character name__</span></span> | <span data-ttu-id="9f966-250">__Kódování Unicode__</span><span class="sxs-lookup"><span data-stu-id="9f966-250">__Unicode encoding__</span></span> |
|---------------------|--------------------|----------------------|
| `\'`                | <span data-ttu-id="9f966-251">jednoduché uvozovky</span><span class="sxs-lookup"><span data-stu-id="9f966-251">Single quote</span></span>       | `0x0027`             | 
| `\"`                | <span data-ttu-id="9f966-252">dvojité uvozovky</span><span class="sxs-lookup"><span data-stu-id="9f966-252">Double quote</span></span>       | `0x0022`             | 
| `\\`                | <span data-ttu-id="9f966-253">Zpětné lomítko</span><span class="sxs-lookup"><span data-stu-id="9f966-253">Backslash</span></span>          | `0x005C`             | 
| `\0`                | <span data-ttu-id="9f966-254">Null</span><span class="sxs-lookup"><span data-stu-id="9f966-254">Null</span></span>               | `0x0000`             | 
| `\a`                | <span data-ttu-id="9f966-255">Výstrahy</span><span class="sxs-lookup"><span data-stu-id="9f966-255">Alert</span></span>              | `0x0007`             | 
| `\b`                | <span data-ttu-id="9f966-256">Backspace</span><span class="sxs-lookup"><span data-stu-id="9f966-256">Backspace</span></span>          | `0x0008`             | 
| `\f`                | <span data-ttu-id="9f966-257">Posun strany</span><span class="sxs-lookup"><span data-stu-id="9f966-257">Form feed</span></span>          | `0x000C`             | 
| `\n`                | <span data-ttu-id="9f966-258">Nový řádek</span><span class="sxs-lookup"><span data-stu-id="9f966-258">New line</span></span>           | `0x000A`             | 
| `\r`                | <span data-ttu-id="9f966-259">Návrat na začátek řádku</span><span class="sxs-lookup"><span data-stu-id="9f966-259">Carriage return</span></span>    | `0x000D`             | 
| `\t`                | <span data-ttu-id="9f966-260">Horizontální tabulátor</span><span class="sxs-lookup"><span data-stu-id="9f966-260">Horizontal tab</span></span>     | `0x0009`             | 
| `\v`                | <span data-ttu-id="9f966-261">Vertikální tabulátor</span><span class="sxs-lookup"><span data-stu-id="9f966-261">Vertical tab</span></span>       | `0x000B`             | 

<span data-ttu-id="9f966-262">Typ *character_literal* je `char`.</span><span class="sxs-lookup"><span data-stu-id="9f966-262">The type of a *character_literal* is `char`.</span></span>

#### <a name="string-literals"></a><span data-ttu-id="9f966-263">Řetězcové literály</span><span class="sxs-lookup"><span data-stu-id="9f966-263">String literals</span></span>

<span data-ttu-id="9f966-264">C# podporuje dvě formy řetězcových literálů: ***regulární řetězcové literály*** a ***doslovný řetězec literálů***.</span><span class="sxs-lookup"><span data-stu-id="9f966-264">C# supports two forms of string literals: ***regular string literals*** and ***verbatim string literals***.</span></span>

<span data-ttu-id="9f966-265">Pravidelné Textový literál skládá z nula nebo více znaků uzavřených v uvozovkách, stejně jako v `"hello"`a může jich obsahovat i jednoduchá řídící sekvence (jako například `\t` pro znak tabulátoru) a šestnáctkové a řídicí sekvence Unicode.</span><span class="sxs-lookup"><span data-stu-id="9f966-265">A regular string literal consists of zero or more characters enclosed in double quotes, as in `"hello"`, and may include both simple escape sequences (such as `\t` for the tab character), and hexadecimal and Unicode escape sequences.</span></span>

<span data-ttu-id="9f966-266">Doslovný řetězec literálu se skládá z `@` znak následovaný znak dvojitých uvozovek, nula nebo více znaků a koncový znak dvojitých uvozovek.</span><span class="sxs-lookup"><span data-stu-id="9f966-266">A verbatim string literal consists of an `@` character followed by a double-quote character, zero or more characters, and a closing double-quote character.</span></span> <span data-ttu-id="9f966-267">Je jednoduchý příklad `@"hello"`.</span><span class="sxs-lookup"><span data-stu-id="9f966-267">A simple example is `@"hello"`.</span></span> <span data-ttu-id="9f966-268">Začal v doslovném řetězci literálu, jsou znaky mezi oddělovači interpretovány verbatim, pouze se výjimka *quote_escape_sequence*.</span><span class="sxs-lookup"><span data-stu-id="9f966-268">In a verbatim string literal, the characters between the delimiters are interpreted verbatim, the only exception being a *quote_escape_sequence*.</span></span> <span data-ttu-id="9f966-269">Zejména jednoduchá řídící sekvence a šestnáctkové a řídicí sekvence Unicode nejsou zpracovány v doslovném řetězci literály.</span><span class="sxs-lookup"><span data-stu-id="9f966-269">In particular, simple escape sequences, and hexadecimal and Unicode escape sequences are not processed in verbatim string literals.</span></span> <span data-ttu-id="9f966-270">Doslovný řetězec literálu může zahrnovat více řádků.</span><span class="sxs-lookup"><span data-stu-id="9f966-270">A verbatim string literal may span multiple lines.</span></span>

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

<span data-ttu-id="9f966-271">Znak, který následuje znak zpětného lomítka (`\`) v *regular_string_literal_character* musí být jedna z následujících znaků: `'`, `"`, `\`, `0`, `a` , `b`, `f`, `n`, `r`, `t`, `u`, `U`, `x`, `v`.</span><span class="sxs-lookup"><span data-stu-id="9f966-271">A character that follows a backslash character (`\`) in a *regular_string_literal_character* must be one of the following characters: `'`, `"`, `\`, `0`, `a`, `b`, `f`, `n`, `r`, `t`, `u`, `U`, `x`, `v`.</span></span> <span data-ttu-id="9f966-272">V opačném případě dojde k chybě v době kompilace.</span><span class="sxs-lookup"><span data-stu-id="9f966-272">Otherwise, a compile-time error occurs.</span></span>

<span data-ttu-id="9f966-273">V příkladu</span><span class="sxs-lookup"><span data-stu-id="9f966-273">The example</span></span>
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
<span data-ttu-id="9f966-274">ukazuje různé řetězcové literály.</span><span class="sxs-lookup"><span data-stu-id="9f966-274">shows a variety of string literals.</span></span> <span data-ttu-id="9f966-275">Poslední řetězcový literál, `j`, je doslovný řetězec literálu, která zahrnuje více řádků.</span><span class="sxs-lookup"><span data-stu-id="9f966-275">The last string literal, `j`, is a verbatim string literal that spans multiple lines.</span></span> <span data-ttu-id="9f966-276">Znaky mezi uvozovkami, včetně prázdných znaků, jako je například znaky nového řádku, jsou zachovány verbatim.</span><span class="sxs-lookup"><span data-stu-id="9f966-276">The characters between the quotation marks, including white space such as new line characters, are preserved verbatim.</span></span>

<span data-ttu-id="9f966-277">Protože šestnáctkové řídicí sekvence může mít proměnný počet šestnáctkových číslic, řetězcový literál `"\x123"` obsahuje jeden znak s šestnáctkovou hodnotou 123.</span><span class="sxs-lookup"><span data-stu-id="9f966-277">Since a hexadecimal escape sequence can have a variable number of hex digits, the string literal `"\x123"` contains a single character with hex value 123.</span></span> <span data-ttu-id="9f966-278">Řetězec obsahující znak s šestnáctkovou hodnotou 12 následované znakem 3 vytvoříte jeden napsat `"\x00123"` nebo `"\x12" + "3"` místo.</span><span class="sxs-lookup"><span data-stu-id="9f966-278">To create a string containing the character with hex value 12 followed by the character 3, one could write `"\x00123"` or `"\x12" + "3"` instead.</span></span>

<span data-ttu-id="9f966-279">Typ *řetězcový_literál* je `string`.</span><span class="sxs-lookup"><span data-stu-id="9f966-279">The type of a *string_literal* is `string`.</span></span>

<span data-ttu-id="9f966-280">Každý řetězec literálu nemá za následek nutně novou instanci řetězce.</span><span class="sxs-lookup"><span data-stu-id="9f966-280">Each string literal does not necessarily result in a new string instance.</span></span> <span data-ttu-id="9f966-281">Když dva nebo více řetězcových literálů, které jsou ekvivalentní podle operátoru rovnosti řetězců ([řetězec operátory rovnosti](expressions.md#string-equality-operators)) se zobrazí ve stejném programu, tyto řetězcové literály odkazují na stejnou instanci řetězce.</span><span class="sxs-lookup"><span data-stu-id="9f966-281">When two or more string literals that are equivalent according to the string equality operator ([String equality operators](expressions.md#string-equality-operators)) appear in the same program, these string literals refer to the same string instance.</span></span> <span data-ttu-id="9f966-282">Například výstup vyprodukovaný</span><span class="sxs-lookup"><span data-stu-id="9f966-282">For instance, the output produced by</span></span>
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
<span data-ttu-id="9f966-283">je `True` vzhledem k tomu dva literály odkazují na stejnou instanci řetězce.</span><span class="sxs-lookup"><span data-stu-id="9f966-283">is `True` because the two literals refer to the same string instance.</span></span>

#### <a name="interpolated-string-literals"></a><span data-ttu-id="9f966-284">Interpolované řetězce literálů</span><span class="sxs-lookup"><span data-stu-id="9f966-284">Interpolated string literals</span></span>

<span data-ttu-id="9f966-285">Interpolované řetězcové literály jsou podobné řetězcové literály, ale obsahovat děr odděleny `{` a `}`, ve které může dojít, výrazy.</span><span class="sxs-lookup"><span data-stu-id="9f966-285">Interpolated string literals are similar to string literals, but contain holes delimited by `{` and `}`, wherein expressions can occur.</span></span> <span data-ttu-id="9f966-286">Za běhu jsou výrazy vyhodnocovány za účelem s jejich textové formulářů nahradit do řetězce na místě, kde dochází k riziko.</span><span class="sxs-lookup"><span data-stu-id="9f966-286">At runtime, the expressions are evaluated with the purpose of having their textual forms substituted into the string at the place where the hole occurs.</span></span> <span data-ttu-id="9f966-287">Syntaxe a sémantiky interpolace řetězců jsou popsány v části ([interpolovaných řetězců](expressions.md#interpolated-strings)).</span><span class="sxs-lookup"><span data-stu-id="9f966-287">The syntax and semantics of string interpolation are described in section ([Interpolated strings](expressions.md#interpolated-strings)).</span></span>

<span data-ttu-id="9f966-288">Například řetězcové literály může být interpolovaný řetězec literálů pravidelné nebo verbatim.</span><span class="sxs-lookup"><span data-stu-id="9f966-288">Like string literals, interpolated string literals can be either regular or verbatim.</span></span> <span data-ttu-id="9f966-289">Interpolované regulární řetězcové literály jsou odděleny `$"` a `"`, a jsou odděleny interpolovaný řetězec verbatim literály `$@"` a `"`.</span><span class="sxs-lookup"><span data-stu-id="9f966-289">Interpolated regular string literals are delimited by `$"` and `"`, and interpolated verbatim string literals are delimited by `$@"` and `"`.</span></span>

<span data-ttu-id="9f966-290">Stejně jako ostatní literály provést lexikální analýzu interpolovaného řetězce literálu zpočátku výsledků do jednoho tokenu podle gramatika níže.</span><span class="sxs-lookup"><span data-stu-id="9f966-290">Like other literals, lexical analysis of an interpolated string literal initially results in a single token, as per the grammar below.</span></span> <span data-ttu-id="9f966-291">Ale před syntaktické analýzy jeden token interpolovaného řetězce literálu je rozdělená do několika tokeny pro části řetězec uzavírající děr a vstupní prvky, ke kterým dochází v děr analyzují lexikálně znovu.</span><span class="sxs-lookup"><span data-stu-id="9f966-291">However, before syntactic analysis, the single token of an interpolated string literal is broken into several tokens for the parts of the string enclosing the holes, and the input elements occurring in the holes are lexically analysed again.</span></span> <span data-ttu-id="9f966-292">To zase může vytvořit více interpolované řetězcových literálů na zpracování, ale, pokud lexikálně opravit, bude nakonec vést k posloupnost tokeny pro syntaktické analýzu ke zpracování.</span><span class="sxs-lookup"><span data-stu-id="9f966-292">This may in turn produce more interpolated string literals to be processed, but, if lexically correct, will eventually lead to a sequence of tokens for syntactic analysis to process.</span></span>

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

<span data-ttu-id="9f966-293">*Interpolated_string_literal* token je interpretovány jako více tokenů a dalších vstupních prvků následujícím způsobem, v pořadí podle výskytu v *interpolated_string_literal*:</span><span class="sxs-lookup"><span data-stu-id="9f966-293">An *interpolated_string_literal* token is reinterpreted as multiple tokens and other input elements as follows, in order of occurrence in the *interpolated_string_literal*:</span></span>

* <span data-ttu-id="9f966-294">Výskyty následující jsou interpretovány jako samostatné jednotlivé tokeny: úvodního `$` znaménko, *interpolated_regular_string_whole*, *interpolated_regular_string_start*, *interpolated_regular_string_mid*, *interpolated_regular_string_end*, *interpolated_verbatim_string_whole*,  *interpolated_verbatim_string_start*, *interpolated_verbatim_string_mid* a *interpolated_verbatim_string_end*.</span><span class="sxs-lookup"><span data-stu-id="9f966-294">Occurrences of the following are reinterpreted as separate individual tokens: the leading `$` sign, *interpolated_regular_string_whole*, *interpolated_regular_string_start*, *interpolated_regular_string_mid*, *interpolated_regular_string_end*, *interpolated_verbatim_string_whole*, *interpolated_verbatim_string_start*, *interpolated_verbatim_string_mid* and *interpolated_verbatim_string_end*.</span></span>
* <span data-ttu-id="9f966-295">Výskyty *regular_balanced_text* a *verbatim_balanced_text* mezi těmito se naplánovalo jako *input_section* ([provést lexikální analýzu ](lexical-structure.md#lexical-analysis)) a jsou interpretovány jako výsledné pořadí elementů vstupu.</span><span class="sxs-lookup"><span data-stu-id="9f966-295">Occurrences of *regular_balanced_text* and *verbatim_balanced_text* between these are reprocessed as an *input_section* ([Lexical analysis](lexical-structure.md#lexical-analysis)) and are reinterpreted as the resulting sequence of input elements.</span></span> <span data-ttu-id="9f966-296">Může pak patří mezi ně interpolovaném řetězci opětovnou interpretaci bitového literálové tokeny.</span><span class="sxs-lookup"><span data-stu-id="9f966-296">These may in turn include interpolated string literal tokens to be reinterpreted.</span></span>

<span data-ttu-id="9f966-297">Syntaktické analýzy se opětovnému zkombinování tokeny do *interpolated_string_expression* ([interpolovaných řetězců](expressions.md#interpolated-strings)).</span><span class="sxs-lookup"><span data-stu-id="9f966-297">Syntactic analysis will recombine the tokens into an *interpolated_string_expression* ([Interpolated strings](expressions.md#interpolated-strings)).</span></span>

<span data-ttu-id="9f966-298">Příklady TODO</span><span class="sxs-lookup"><span data-stu-id="9f966-298">Examples TODO</span></span>


#### <a name="the-null-literal"></a><span data-ttu-id="9f966-299">Literál s hodnotou null</span><span class="sxs-lookup"><span data-stu-id="9f966-299">The null literal</span></span>

```antlr
null_literal
    : 'null'
    ;
```

<span data-ttu-id="9f966-300">*Null_literal* lze implicitně převést na typ odkazu nebo typ připouštějící hodnotu Null.</span><span class="sxs-lookup"><span data-stu-id="9f966-300">The  *null_literal* can be implicitly converted to a reference type or nullable type.</span></span>

### <a name="operators-and-punctuators"></a><span data-ttu-id="9f966-301">Operátory a interpunkční znaky</span><span class="sxs-lookup"><span data-stu-id="9f966-301">Operators and punctuators</span></span>

<span data-ttu-id="9f966-302">Existuje několik druhů operátory a interpunkční znaky.</span><span class="sxs-lookup"><span data-stu-id="9f966-302">There are several kinds of operators and punctuators.</span></span> <span data-ttu-id="9f966-303">Operátory se ve výrazech používají k popisu operace, které zahrnují jeden nebo více operandů.</span><span class="sxs-lookup"><span data-stu-id="9f966-303">Operators are used in expressions to describe operations involving one or more operands.</span></span> <span data-ttu-id="9f966-304">Například výraz `a + b` používá `+` operátor přidat dva operandy `a` a `b`.</span><span class="sxs-lookup"><span data-stu-id="9f966-304">For example, the expression `a + b` uses the `+` operator to add the two operands `a` and `b`.</span></span> <span data-ttu-id="9f966-305">Interpunkční znaky jsou určené pro seskupování a oddělení.</span><span class="sxs-lookup"><span data-stu-id="9f966-305">Punctuators are for grouping and separating.</span></span>

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

<span data-ttu-id="9f966-306">Svislá čára v *right_shift* a *right_shift_assignment* výroby slouží k označení, že, na rozdíl od jiných výroby v syntaxi gramatice žádné znaky jakéhokoli druhu (dokonce ani prázdné znaky) jsou povolené mezi tokeny.</span><span class="sxs-lookup"><span data-stu-id="9f966-306">The vertical bar in the *right_shift* and *right_shift_assignment* productions are used to indicate that, unlike other productions in the syntactic grammar, no characters of any kind (not even whitespace) are allowed between the tokens.</span></span> <span data-ttu-id="9f966-307">Tyto výroby jsou zpracovány speciálně, chcete-li povolit správné zpracování *type_parameter_list*s ([parametry typu](classes.md#type-parameters)).</span><span class="sxs-lookup"><span data-stu-id="9f966-307">These productions are treated specially in order to enable the correct  handling of *type_parameter_list*s ([Type parameters](classes.md#type-parameters)).</span></span>

## <a name="pre-processing-directives"></a><span data-ttu-id="9f966-308">Direktivy předběžného zpracování</span><span class="sxs-lookup"><span data-stu-id="9f966-308">Pre-processing directives</span></span>

<span data-ttu-id="9f966-309">Direktivy předběžného zpracování umožní v podmíněně přeskočit oddílů zdrojové soubory k nahlášení chyby a upozornění podmínky a zobrazit různé oblasti zdrojového kódu.</span><span class="sxs-lookup"><span data-stu-id="9f966-309">The pre-processing directives provide the ability to conditionally skip sections of source files, to report error and warning conditions, and to delineate distinct regions of source code.</span></span> <span data-ttu-id="9f966-310">Termín "předběžné zpracování direktiv" se používá pouze pro soulad s programovacích jazyků C a C++.</span><span class="sxs-lookup"><span data-stu-id="9f966-310">The term "pre-processing directives" is used only for consistency with the C and C++ programming languages.</span></span> <span data-ttu-id="9f966-311">V jazyce C# neexistuje žádný samostatný krok předběžného zpracování. direktivy předběžného zpracování jsou zpracovávány jako součást fáze provést lexikální analýzu.</span><span class="sxs-lookup"><span data-stu-id="9f966-311">In C#, there is no separate pre-processing step; pre-processing directives are processed as part of the lexical analysis phase.</span></span>

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

<span data-ttu-id="9f966-312">K dispozici jsou následující direktivy předběžného zpracování:</span><span class="sxs-lookup"><span data-stu-id="9f966-312">The following pre-processing directives are available:</span></span>

*  <span data-ttu-id="9f966-313">`#define` a `#undef`, které se používají k definování a, respektive nedefinované symboly podmíněné kompilace ([deklaračních direktiv](lexical-structure.md#declaration-directives)).</span><span class="sxs-lookup"><span data-stu-id="9f966-313">`#define` and `#undef`, which are used to define and undefine, respectively, conditional compilation symbols ([Declaration directives](lexical-structure.md#declaration-directives)).</span></span>
*  <span data-ttu-id="9f966-314">`#if`, `#elif`, `#else`, a `#endif`, které se používají k podmíněně vynechá části zdrojového kódu ([direktivy podmíněné kompilace](lexical-structure.md#conditional-compilation-directives)).</span><span class="sxs-lookup"><span data-stu-id="9f966-314">`#if`, `#elif`, `#else`, and `#endif`, which are used to conditionally skip sections of source code ([Conditional compilation directives](lexical-structure.md#conditional-compilation-directives)).</span></span>
*  <span data-ttu-id="9f966-315">`#line`, který se používá k řízení čísla řádků pro chyby a upozornění ([řádek direktivy](lexical-structure.md#line-directives)).</span><span class="sxs-lookup"><span data-stu-id="9f966-315">`#line`, which is used to control line numbers emitted for errors and warnings ([Line directives](lexical-structure.md#line-directives)).</span></span>
*  <span data-ttu-id="9f966-316">`#error` a `#warning`, které se používají k vydávání chyby a upozornění, respektive ([diagnostických direktivy](lexical-structure.md#diagnostic-directives)).</span><span class="sxs-lookup"><span data-stu-id="9f966-316">`#error` and `#warning`, which are used to issue errors and warnings, respectively ([Diagnostic directives](lexical-structure.md#diagnostic-directives)).</span></span>
*  <span data-ttu-id="9f966-317">`#region` a `#endregion`, které se používají k explicitně označit části zdrojového kódu ([direktivy oblastí](lexical-structure.md#region-directives)).</span><span class="sxs-lookup"><span data-stu-id="9f966-317">`#region` and `#endregion`, which are used to explicitly mark sections of source code ([Region directives](lexical-structure.md#region-directives)).</span></span>
*  <span data-ttu-id="9f966-318">`#pragma`, který se používá k určení volitelné kontextové informace kompilátoru ([direktivy Pragma](lexical-structure.md#pragma-directives)).</span><span class="sxs-lookup"><span data-stu-id="9f966-318">`#pragma`, which is used to specify optional contextual information to the compiler ([Pragma directives](lexical-structure.md#pragma-directives)).</span></span>

<span data-ttu-id="9f966-319">Direktivy předběžného zpracování vždy zabírá samostatný řádek zdrojového kódu a vždy začíná službami `#` znaků a název direktivy předběžného zpracování.</span><span class="sxs-lookup"><span data-stu-id="9f966-319">A pre-processing directive always occupies a separate line of source code and always begins with a `#` character and a pre-processing directive name.</span></span> <span data-ttu-id="9f966-320">Prázdné znaky může dojít před `#` znak a mezi `#` znaků a název direktivy.</span><span class="sxs-lookup"><span data-stu-id="9f966-320">White space may occur before the `#` character and between the `#` character and the directive name.</span></span>

<span data-ttu-id="9f966-321">Zdrojový řádek obsahující `#define`, `#undef`, `#if`, `#elif`, `#else`, `#endif`, `#line`, nebo `#endregion` – direktiva mohou končit jednořádkový komentář.</span><span class="sxs-lookup"><span data-stu-id="9f966-321">A source line containing a `#define`, `#undef`, `#if`, `#elif`, `#else`, `#endif`, `#line`, or `#endregion` directive may end with a single-line comment.</span></span> <span data-ttu-id="9f966-322">Komentáře s oddělovači ( `/* */` Styl komentáře) nejsou povoleny na zdrojové řádky obsahující direktivy předběžného zpracování.</span><span class="sxs-lookup"><span data-stu-id="9f966-322">Delimited comments (the `/* */` style of comments) are not permitted on source lines containing pre-processing directives.</span></span>

<span data-ttu-id="9f966-323">Direktivy předběžného zpracování nejsou tokeny a nejsou součástí syntaktické gramatika C#.</span><span class="sxs-lookup"><span data-stu-id="9f966-323">Pre-processing directives are not tokens and are not part of the syntactic grammar of C#.</span></span> <span data-ttu-id="9f966-324">Direktivy předběžného zpracování však lze použít k zahrnutí nebo vyloučení sekvence tokenů a můžete tímto způsobem ovlivnit význam programu v jazyce C#.</span><span class="sxs-lookup"><span data-stu-id="9f966-324">However, pre-processing directives can be used to include or exclude sequences of tokens and can in that way affect the meaning of a C# program.</span></span> <span data-ttu-id="9f966-325">Například při kompilaci programu:</span><span class="sxs-lookup"><span data-stu-id="9f966-325">For example, when compiled, the program:</span></span>
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
<span data-ttu-id="9f966-326">výsledky v přesně stejnou sekvenci tokeny jako programu:</span><span class="sxs-lookup"><span data-stu-id="9f966-326">results in the exact same sequence of tokens as the program:</span></span>
```csharp
class C
{
    void F() {}
    void I() {}
}
```

<span data-ttu-id="9f966-327">To znamená že lexikálně, dva programy je značně odlišná, syntakticky, že jsou identické.</span><span class="sxs-lookup"><span data-stu-id="9f966-327">Thus, whereas lexically, the two programs are quite different, syntactically, they are identical.</span></span>

### <a name="conditional-compilation-symbols"></a><span data-ttu-id="9f966-328">Symboly podmíněné kompilace</span><span class="sxs-lookup"><span data-stu-id="9f966-328">Conditional compilation symbols</span></span>

<span data-ttu-id="9f966-329">Podmíněná kompilace funkce poskytované službou `#if`, `#elif`, `#else`, a `#endif` direktivy je řízen pomocí předběžného zpracování výrazů ([předběžného zpracování výrazů](lexical-structure.md#pre-processing-expressions)) a symboly podmíněné kompilace.</span><span class="sxs-lookup"><span data-stu-id="9f966-329">The conditional compilation functionality provided by the `#if`, `#elif`, `#else`, and `#endif` directives is controlled through pre-processing expressions ([Pre-processing expressions](lexical-structure.md#pre-processing-expressions)) and conditional compilation symbols.</span></span>

```antlr
conditional_symbol
    : '<Any identifier_or_keyword except true or false>'
    ;
```

<span data-ttu-id="9f966-330">Symbol podmíněné kompilace má dva možné stavy: ***definované*** nebo ***nedefinované***.</span><span class="sxs-lookup"><span data-stu-id="9f966-330">A conditional compilation symbol has two possible states: ***defined*** or ***undefined***.</span></span> <span data-ttu-id="9f966-331">Na začátku lexikální zpracování zdrojového souboru, je definován symbol podmíněné kompilace, pokud byly explicitně definovány pomocí externího mechanismu (jako je například možnost příkazového řádku kompilátoru).</span><span class="sxs-lookup"><span data-stu-id="9f966-331">At the beginning of the lexical processing of a source file, a conditional compilation symbol is undefined unless it has been explicitly defined by an external mechanism (such as a command-line compiler option).</span></span> <span data-ttu-id="9f966-332">Když `#define` – direktiva se zpracovává, stane definován symbol podmíněné kompilace v náhradu této direktivy s názvem v daném zdrojovém souboru.</span><span class="sxs-lookup"><span data-stu-id="9f966-332">When a `#define` directive is processed, the conditional compilation symbol named in that directive becomes defined in that source file.</span></span> <span data-ttu-id="9f966-333">Symbol zůstává definovaný až `#undef` směrnice pro zpracování stejným symbolu, nebo dokud je dosaženo konce zdrojového souboru.</span><span class="sxs-lookup"><span data-stu-id="9f966-333">The symbol remains defined until an `#undef` directive for that same symbol is processed, or until the end of the source file is reached.</span></span> <span data-ttu-id="9f966-334">Důsledkem tohoto je, že `#define` a `#undef` direktivy v jednom zdrojovém souboru nemají žádný vliv na jiných zdrojových souborech ve stejném programu.</span><span class="sxs-lookup"><span data-stu-id="9f966-334">An implication of this is that `#define` and `#undef` directives in one source file have no effect on other source files in the same program.</span></span>

<span data-ttu-id="9f966-335">Při odkazování ve výrazu předběžného zpracování, symbol definovaný podmíněné kompilace má logická hodnota `true`, a symbol definován Podmíněná kompilace je logická hodnota `false`.</span><span class="sxs-lookup"><span data-stu-id="9f966-335">When referenced in a pre-processing expression, a defined conditional compilation symbol has the boolean value `true`, and an undefined conditional compilation symbol has the boolean value `false`.</span></span> <span data-ttu-id="9f966-336">Neexistuje žádný požadavek, symboly podmíněné kompilace explicitně deklarovat předtím, než jsou odkazovány v předběžného zpracování výrazů.</span><span class="sxs-lookup"><span data-stu-id="9f966-336">There is no requirement that conditional compilation symbols be explicitly declared before they are referenced in pre-processing expressions.</span></span> <span data-ttu-id="9f966-337">Místo toho nedeklarovaný symboly nejsou jednoduše definovány a proto mají hodnotu `false`.</span><span class="sxs-lookup"><span data-stu-id="9f966-337">Instead, undeclared symbols are simply undefined and thus have the value `false`.</span></span>

<span data-ttu-id="9f966-338">Obor názvů pro symboly podmíněné kompilace je samostatný a oddělený od jiných pojmenovaných entit v programu v jazyce C#.</span><span class="sxs-lookup"><span data-stu-id="9f966-338">The name space for conditional compilation symbols is distinct and separate from all other named entities in a C# program.</span></span> <span data-ttu-id="9f966-339">Symboly podmíněné kompilace může být odkazováno pouze v `#define` a `#undef` direktivy a předběžného zpracování výrazů.</span><span class="sxs-lookup"><span data-stu-id="9f966-339">Conditional compilation symbols can only be referenced in `#define` and `#undef` directives and in pre-processing expressions.</span></span>

### <a name="pre-processing-expressions"></a><span data-ttu-id="9f966-340">Předběžné zpracování výrazů</span><span class="sxs-lookup"><span data-stu-id="9f966-340">Pre-processing expressions</span></span>

<span data-ttu-id="9f966-341">Předběžné zpracování výrazů může dojít v `#if` a `#elif` direktivy.</span><span class="sxs-lookup"><span data-stu-id="9f966-341">Pre-processing expressions can occur in `#if` and `#elif` directives.</span></span> <span data-ttu-id="9f966-342">Operátory `!`, `==`, `!=`, `&&` a `||` jsou povolené v předběžného zpracování výrazů, a mohou být použity závorky pro seskupení.</span><span class="sxs-lookup"><span data-stu-id="9f966-342">The operators `!`, `==`, `!=`, `&&` and `||` are permitted in pre-processing expressions, and parentheses may be used for grouping.</span></span>

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

<span data-ttu-id="9f966-343">Při odkazování ve výrazu předběžného zpracování, symbol definovaný podmíněné kompilace má logická hodnota `true`, a symbol definován Podmíněná kompilace je logická hodnota `false`.</span><span class="sxs-lookup"><span data-stu-id="9f966-343">When referenced in a pre-processing expression, a defined conditional compilation symbol has the boolean value `true`, and an undefined conditional compilation symbol has the boolean value `false`.</span></span>

<span data-ttu-id="9f966-344">Vyhodnocení výrazu předběžného zpracování vždy vrátí hodnotu typu boolean.</span><span class="sxs-lookup"><span data-stu-id="9f966-344">Evaluation of a pre-processing expression always yields a boolean value.</span></span> <span data-ttu-id="9f966-345">Pravidla vyhodnocení výrazu předběžného zpracování jsou stejné jako pro konstantní výraz ([konstantní výrazy](expressions.md#constant-expressions)), s tím rozdílem, že pouze entity definovaný uživatelem, které může být odkazováno jsou symboly podmíněné kompilace .</span><span class="sxs-lookup"><span data-stu-id="9f966-345">The rules of evaluation for a pre-processing expression are the same as those for a constant expression ([Constant expressions](expressions.md#constant-expressions)), except that the only user-defined entities that can be referenced are conditional compilation symbols.</span></span>

### <a name="declaration-directives"></a><span data-ttu-id="9f966-346">Deklaračních direktiv</span><span class="sxs-lookup"><span data-stu-id="9f966-346">Declaration directives</span></span>

<span data-ttu-id="9f966-347">Deklaračních direktiv se používají k definování nebo nedefinované symboly podmíněné kompilace.</span><span class="sxs-lookup"><span data-stu-id="9f966-347">The declaration directives are used to define or undefine conditional compilation symbols.</span></span>

```antlr
pp_declaration
    : whitespace? '#' whitespace? 'define' whitespace conditional_symbol pp_new_line
    | whitespace? '#' whitespace? 'undef' whitespace conditional_symbol pp_new_line
    ;

pp_new_line
    : whitespace? single_line_comment? new_line
    ;
```

<span data-ttu-id="9f966-348">Zpracování `#define` – direktiva způsobí, že symbol daného podmíněné kompilace budou definovat, počínaje zdrojového řádku, který následuje direktivu.</span><span class="sxs-lookup"><span data-stu-id="9f966-348">The processing of a `#define` directive causes the given conditional compilation symbol to become defined, starting with the source line that follows the directive.</span></span> <span data-ttu-id="9f966-349">Podobně, zpracování `#undef` – direktiva způsobí, že symbol daného podmíněné kompilace se nedefinované, počínaje zdrojového řádku, který následuje direktivu.</span><span class="sxs-lookup"><span data-stu-id="9f966-349">Likewise, the processing of an `#undef` directive causes the given conditional compilation symbol to become undefined, starting with the source line that follows the directive.</span></span>

<span data-ttu-id="9f966-350">Žádné `#define` a `#undef` směrnice ve zdrojovém souboru se musí vyskytovat před první *token* ([tokeny](lexical-structure.md#tokens)) ve zdrojovém souboru; jinak kompilace dojde k chybě.</span><span class="sxs-lookup"><span data-stu-id="9f966-350">Any `#define` and `#undef` directives in a source file must occur before the first *token* ([Tokens](lexical-structure.md#tokens)) in the source file; otherwise a compile-time error occurs.</span></span> <span data-ttu-id="9f966-351">Intuitivní řečeno `#define` a `#undef` direktivy musí předcházet všechny "skutečného kódu" ve zdrojovém souboru.</span><span class="sxs-lookup"><span data-stu-id="9f966-351">In intuitive terms, `#define` and `#undef` directives must precede any "real code" in the source file.</span></span>

<span data-ttu-id="9f966-352">Příklad:</span><span class="sxs-lookup"><span data-stu-id="9f966-352">The example:</span></span>
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
<span data-ttu-id="9f966-353">je platný vzhledem k tomu `#define` direktivy předcházet první token ( `namespace` – klíčové slovo) ve zdrojovém souboru.</span><span class="sxs-lookup"><span data-stu-id="9f966-353">is valid because the `#define` directives precede the first token (the `namespace` keyword) in the source file.</span></span>

<span data-ttu-id="9f966-354">Následující příklad vrátí chybu v době kompilace, protože `#define` následuje skutečného kódu:</span><span class="sxs-lookup"><span data-stu-id="9f966-354">The following example results in a compile-time error because a `#define` follows real code:</span></span>
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

<span data-ttu-id="9f966-355">A `#define` může definovat symbol podmíněné kompilace, která je již definován, aniž by bylo jakékoli intervenující `#undef` pro tento symbol.</span><span class="sxs-lookup"><span data-stu-id="9f966-355">A `#define` may define a conditional compilation symbol that is already defined, without there being any intervening `#undef` for that symbol.</span></span> <span data-ttu-id="9f966-356">Následující příklad definuje symbol podmíněné kompilace `A` a znovu ji definuje.</span><span class="sxs-lookup"><span data-stu-id="9f966-356">The example below defines a conditional compilation symbol `A` and then defines it again.</span></span>
```csharp
#define A
#define A
```

<span data-ttu-id="9f966-357">A `#undef` může "definice" podmíněné kompilace symbolu, která není definovaná.</span><span class="sxs-lookup"><span data-stu-id="9f966-357">A `#undef` may "undefine" a conditional compilation symbol that is not defined.</span></span> <span data-ttu-id="9f966-358">Následující příklad definuje symbol podmíněné kompilace `A` a potom nedefinovaných hodnot ji dvakrát; i když druhý `#undef` nemá žádný vliv, je stále platný.</span><span class="sxs-lookup"><span data-stu-id="9f966-358">The example below defines a conditional compilation symbol `A` and then undefines it twice; although the second `#undef` has no effect, it is still valid.</span></span>
```csharp
#define A
#undef A
#undef A
```

### <a name="conditional-compilation-directives"></a><span data-ttu-id="9f966-359">Direktivy podmíněné kompilace</span><span class="sxs-lookup"><span data-stu-id="9f966-359">Conditional compilation directives</span></span>

<span data-ttu-id="9f966-360">Direktivy podmíněné kompilace umožňují podmíněně zahrnout nebo vyloučit částí zdrojového souboru.</span><span class="sxs-lookup"><span data-stu-id="9f966-360">The conditional compilation directives are used to conditionally include or exclude portions of a source file.</span></span>

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

<span data-ttu-id="9f966-361">Jak je uvedeno v syntaxi, direktivy podmíněné kompilace musí být napsaná jako sady skládající se z, v pořadí, `#if` směrnice, nula nebo více `#elif` direktivy, nula nebo `#else` směrnice a `#endif` – direktiva.</span><span class="sxs-lookup"><span data-stu-id="9f966-361">As indicated by the syntax, conditional compilation directives must be written as sets consisting of, in order, an `#if` directive, zero or more `#elif` directives, zero or one `#else` directive, and an `#endif` directive.</span></span> <span data-ttu-id="9f966-362">Mezi direktivy jsou podmíněné sekce zdrojového kódu.</span><span class="sxs-lookup"><span data-stu-id="9f966-362">Between the directives are conditional sections of source code.</span></span> <span data-ttu-id="9f966-363">Každý oddíl se řídí bezprostředně předcházející směrnice.</span><span class="sxs-lookup"><span data-stu-id="9f966-363">Each section is controlled by the immediately preceding directive.</span></span> <span data-ttu-id="9f966-364">Podmíněný oddíl může obsahovat direktivy podmíněné kompilace vnořené zadané tyto direktivy tvoří kompletní sady.</span><span class="sxs-lookup"><span data-stu-id="9f966-364">A conditional section may itself contain nested conditional compilation directives provided these directives form complete sets.</span></span>

<span data-ttu-id="9f966-365">A *pp_conditional* vybere jenom jedna z uzavřeného *conditional_section*s normální lexikální zpracování:</span><span class="sxs-lookup"><span data-stu-id="9f966-365">A *pp_conditional* selects at most one of the contained *conditional_section*s for normal lexical processing:</span></span>

*  <span data-ttu-id="9f966-366">*Pp_expression*s `#if` a `#elif` direktivy jsou vyhodnocovány v pořadí, dokud jeden vrací `true`.</span><span class="sxs-lookup"><span data-stu-id="9f966-366">The *pp_expression*s of the `#if` and `#elif` directives are evaluated in order until one yields `true`.</span></span> <span data-ttu-id="9f966-367">Pokud výraz vrací `true`, *conditional_section* je vybrána odpovídající direktivy.</span><span class="sxs-lookup"><span data-stu-id="9f966-367">If an expression yields `true`, the *conditional_section* of the corresponding directive is selected.</span></span>
*  <span data-ttu-id="9f966-368">Pokud všechny *pp_expression*s yield `false`a pokud `#else` – direktiva je k dispozici, *conditional_section* z `#else` – direktiva je vybrána.</span><span class="sxs-lookup"><span data-stu-id="9f966-368">If all *pp_expression*s yield `false`, and if an `#else` directive is present, the *conditional_section* of the `#else` directive is selected.</span></span>
*  <span data-ttu-id="9f966-369">V opačném případě ne *conditional_section* zaškrtnuto.</span><span class="sxs-lookup"><span data-stu-id="9f966-369">Otherwise, no *conditional_section* is selected.</span></span>

<span data-ttu-id="9f966-370">Vybrané *conditional_section*, pokud existuje, se zpracovává jako normální *input_section*: musí dodržovat zdrojového kódu obsažen v části gramatika slov; tokeny jsou generovány ze zdroje kód v části; a předběžné zpracování direktiv v části mají předepsané účinky.</span><span class="sxs-lookup"><span data-stu-id="9f966-370">The selected *conditional_section*, if any, is processed as a normal *input_section*: the source code contained in the section must adhere to the lexical grammar; tokens are generated from the source code in the section; and pre-processing directives in the section have the prescribed effects.</span></span>

<span data-ttu-id="9f966-371">Zbývající *conditional_section*s, je-li k dispozici, jsou zpracovávány jako *skipped_section*s: s výjimkou direktivy předběžného zpracování, zdrojový kód v části nemusí splňovat lexikální gramatiky; ne tokeny jsou generovány ze zdrojového kódu v části; a předběžné zpracování direktiv v části musí být lexikálně správná, ale nebudou zpracovány jinak.</span><span class="sxs-lookup"><span data-stu-id="9f966-371">The remaining *conditional_section*s, if any, are processed as *skipped_section*s: except for pre-processing directives, the source code in the section need not adhere to the lexical grammar; no tokens are generated from the source code in the section; and pre-processing directives in the section must be lexically correct but are not otherwise processed.</span></span> <span data-ttu-id="9f966-372">V rámci *conditional_section* , který je zpracováván jako *skipped_section*jakékoli vnořené *conditional_section*s (obsažené ve vnořených `#if`... `#endif` a `#region`... `#endregion` vytvoří) zpracovávají jako *skipped_section*s.</span><span class="sxs-lookup"><span data-stu-id="9f966-372">Within a *conditional_section* that is being processed as a *skipped_section*, any nested *conditional_section*s (contained in nested `#if`...`#endif` and `#region`...`#endregion` constructs) are also processed as *skipped_section*s.</span></span>

<span data-ttu-id="9f966-373">Následující příklad ukazuje, jak Podmíněná kompilace můžete vnořit direktivy:</span><span class="sxs-lookup"><span data-stu-id="9f966-373">The following example illustrates how conditional compilation directives can nest:</span></span>
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

<span data-ttu-id="9f966-374">S výjimkou direktivy předběžného zpracování, přeskočené zdrojový kód není lze provést lexikální analýzu.</span><span class="sxs-lookup"><span data-stu-id="9f966-374">Except for pre-processing directives, skipped source code is not subject to lexical analysis.</span></span> <span data-ttu-id="9f966-375">Například následující platí bez ohledu na Neukončený komentář `#else` části:</span><span class="sxs-lookup"><span data-stu-id="9f966-375">For example, the following is valid despite the unterminated comment in the `#else` section:</span></span>
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

<span data-ttu-id="9f966-376">Všimněte si však, že jsou direktivy předběžného zpracování musí být lexikálně správné i v bylo přeskočeno částí zdrojového kódu.</span><span class="sxs-lookup"><span data-stu-id="9f966-376">Note, however, that pre-processing directives are required to be lexically correct even in skipped sections of source code.</span></span>

<span data-ttu-id="9f966-377">Direktivy předběžného zpracování nejsou zpracovány, když se zobrazí uvnitř Víceřádkový vstupní prvky.</span><span class="sxs-lookup"><span data-stu-id="9f966-377">Pre-processing directives are not processed when they appear inside multi-line input elements.</span></span> <span data-ttu-id="9f966-378">Například program:</span><span class="sxs-lookup"><span data-stu-id="9f966-378">For example, the program:</span></span>
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
<span data-ttu-id="9f966-379">výsledky ve výstupu:</span><span class="sxs-lookup"><span data-stu-id="9f966-379">results in the output:</span></span>
```
hello,
#if Debug
        world
#else
        Nebraska
#endif
```

<span data-ttu-id="9f966-380">V případech specifické sadu direktivy předběžného zpracování, která je zpracována může záviset na vyhodnocení *pp_expression*.</span><span class="sxs-lookup"><span data-stu-id="9f966-380">In peculiar cases, the set of pre-processing directives that is processed might depend on the evaluation of the *pp_expression*.</span></span> <span data-ttu-id="9f966-381">Příklad:</span><span class="sxs-lookup"><span data-stu-id="9f966-381">The example:</span></span>
```csharp
#if X
    /*
#else
    /* */ class Q { }
#endif
```
<span data-ttu-id="9f966-382">vždy vytváří stejný token datového proudu (`class` `Q` `{` `}`), bez ohledu na to, jestli `X` je definována.</span><span class="sxs-lookup"><span data-stu-id="9f966-382">always produces the same token stream (`class` `Q` `{` `}`), regardless of whether or not `X` is defined.</span></span> <span data-ttu-id="9f966-383">Pokud `X` je definován, jsou pouze zpracovaných direktivy `#if` a `#endif`z důvodu víceřádkových komentářů.</span><span class="sxs-lookup"><span data-stu-id="9f966-383">If `X` is defined, the only processed directives are `#if` and `#endif`, due to the multi-line comment.</span></span> <span data-ttu-id="9f966-384">Pokud `X` je nedefinovaný, pak tři direktivy (`#if`, `#else`, `#endif`) jsou součástí sady direktiv.</span><span class="sxs-lookup"><span data-stu-id="9f966-384">If `X` is undefined, then three directives (`#if`, `#else`, `#endif`) are part of the directive set.</span></span>

### <a name="diagnostic-directives"></a><span data-ttu-id="9f966-385">Diagnostické direktivy</span><span class="sxs-lookup"><span data-stu-id="9f966-385">Diagnostic directives</span></span>

<span data-ttu-id="9f966-386">Direktivy diagnostiky se používají k generovat explicitně chybové zprávy a upozornění, které jsou hlášeny stejným způsobem jako jiné chyby při kompilaci a upozornění.</span><span class="sxs-lookup"><span data-stu-id="9f966-386">The diagnostic directives are used to explicitly generate error and warning messages that are reported in the same way as other compile-time errors and warnings.</span></span>

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

<span data-ttu-id="9f966-387">Příklad:</span><span class="sxs-lookup"><span data-stu-id="9f966-387">The example:</span></span>
```csharp
#warning Code review needed before check-in

#if Debug && Retail
    #error A build can't be both debug and retail
#endif

class Test {...}
```
<span data-ttu-id="9f966-388">vždy vyvolá upozornění ("revize kódu potřebný před vrácení se změnami") a způsobí chybu kompilace ("sestavení nemůže být ladění a maloobchodní licence") Pokud podmíněné symboly `Debug` a `Retail` jsou definovány.</span><span class="sxs-lookup"><span data-stu-id="9f966-388">always produces a warning ("Code review needed before check-in"), and produces a compile-time error ("A build can't be both debug and retail") if the conditional symbols `Debug` and `Retail` are both defined.</span></span> <span data-ttu-id="9f966-389">Všimněte si, že *pp_message* může obsahovat libovolný text, konkrétně, nemusí znázorněno jednoduché uvozovky klíčové slovo obsahovat tokenů ve správném formátu, `can't`.</span><span class="sxs-lookup"><span data-stu-id="9f966-389">Note that a *pp_message* can contain arbitrary text; specifically, it need not contain well-formed tokens, as shown by the single quote in the word `can't`.</span></span>

### <a name="region-directives"></a><span data-ttu-id="9f966-390">Direktivy oblastí</span><span class="sxs-lookup"><span data-stu-id="9f966-390">Region directives</span></span>

<span data-ttu-id="9f966-391">Direktivy oblastí se používají k explicitně označit oblasti zdrojového kódu.</span><span class="sxs-lookup"><span data-stu-id="9f966-391">The region directives are used to explicitly mark regions of source code.</span></span>

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

<span data-ttu-id="9f966-392">Žádné sémantický význam je připojen k oblasti; oblasti jsou určeny k použití programátor nebo automatizované nástroje pro označení část zdrojového kódu.</span><span class="sxs-lookup"><span data-stu-id="9f966-392">No semantic meaning is attached to a region; regions are intended for use by the programmer or by automated tools to mark a section of source code.</span></span> <span data-ttu-id="9f966-393">Zprávu určenou ve `#region` nebo `#endregion` – direktiva podobně nemá žádný sémantický význam; slouží jenom k identifikaci oblasti.</span><span class="sxs-lookup"><span data-stu-id="9f966-393">The message specified in a `#region` or `#endregion` directive likewise has no semantic meaning; it merely serves to identify the region.</span></span> <span data-ttu-id="9f966-394">Odpovídající `#region` a `#endregion` direktivy mohou mít různé *pp_message*s.</span><span class="sxs-lookup"><span data-stu-id="9f966-394">Matching `#region` and `#endregion` directives may have different *pp_message*s.</span></span>

<span data-ttu-id="9f966-395">Lexikální zpracování oblasti:</span><span class="sxs-lookup"><span data-stu-id="9f966-395">The lexical processing of a region:</span></span>
```csharp
#region
...
#endregion
```
<span data-ttu-id="9f966-396">odpovídá přesně lexikální zpracování direktivy podmíněné kompilace ve tvaru:</span><span class="sxs-lookup"><span data-stu-id="9f966-396">corresponds exactly to the lexical processing of a conditional compilation directive of the form:</span></span>
```csharp
#if true
...
#endif
```

### <a name="line-directives"></a><span data-ttu-id="9f966-397">Direktivy Line</span><span class="sxs-lookup"><span data-stu-id="9f966-397">Line directives</span></span>

<span data-ttu-id="9f966-398">Direktivy Line slouží ke změně čísla řádku a názvy zdrojových souborů, které jsou hlášeny sadou kompilátoru ve výstupu, jako je například upozornění a chyby a které jsou používány atributy informace o volajícím ([atributy informace o volajícím](attributes.md#caller-info-attributes)).</span><span class="sxs-lookup"><span data-stu-id="9f966-398">Line directives may be used to alter the line numbers and source file names that are reported by the compiler in output such as warnings and errors, and that are used by caller info attributes ([Caller info attributes](attributes.md#caller-info-attributes)).</span></span>

<span data-ttu-id="9f966-399">Direktivy line se běžně používají v meta programovací nástroje, které generují zdrojový kód C# z některých jiných textový vstup.</span><span class="sxs-lookup"><span data-stu-id="9f966-399">Line directives are most commonly used in meta-programming tools that generate C# source code from some other text input.</span></span>

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

<span data-ttu-id="9f966-400">Pokud ne `#line` direktivy jsou k dispozici, kompilátor oznámí čísla řádků true a názvů zdrojových souborů v jeho výstup.</span><span class="sxs-lookup"><span data-stu-id="9f966-400">When no `#line` directives are present, the compiler reports true line numbers and source file names in its output.</span></span> <span data-ttu-id="9f966-401">Při zpracování `#line` direktiva, která zahrnuje *line_indicator* , který není `default`, kompilátor považuje za direktivou tak, že má zadaná čísla řádků (a název souboru, je-li zadán) na řádku.</span><span class="sxs-lookup"><span data-stu-id="9f966-401">When processing a `#line` directive that includes a *line_indicator* that is not `default`, the compiler treats the line after the directive as having the given line number (and file name, if specified).</span></span>

<span data-ttu-id="9f966-402">A `#line default` obrátí účinek metody všechny předchozí direktiv #line – direktiva.</span><span class="sxs-lookup"><span data-stu-id="9f966-402">A `#line default` directive reverses the effect of all preceding #line directives.</span></span> <span data-ttu-id="9f966-403">Kompilátor sestavy true řádku informace pro následné řádky, přesně jak když není `#line` zpracoval direktivy.</span><span class="sxs-lookup"><span data-stu-id="9f966-403">The compiler reports true line information for subsequent lines, precisely as if no `#line` directives had been processed.</span></span>

<span data-ttu-id="9f966-404">A `#line hidden` – direktiva nemá žádný vliv na souboru a čísla řádků, které jsou uvedeny v chybové zprávy, ale neovlivňuje ladění na úrovni zdroje.</span><span class="sxs-lookup"><span data-stu-id="9f966-404">A `#line hidden` directive has no effect on the file and line numbers reported in error messages, but does affect source level debugging.</span></span> <span data-ttu-id="9f966-405">Při ladění, všechny řádky mezi `#line hidden` směrnice a následné `#line` – direktiva (není `#line hidden`) mít žádné informace o číslech řádků.</span><span class="sxs-lookup"><span data-stu-id="9f966-405">When debugging, all lines between a `#line hidden` directive and the subsequent `#line` directive (that is not `#line hidden`) have no line number information.</span></span> <span data-ttu-id="9f966-406">Při krokování kódu v ladicím programu, se přeskočí zcela tyto řádky.</span><span class="sxs-lookup"><span data-stu-id="9f966-406">When stepping through code in the debugger, these lines will be skipped entirely.</span></span>

<span data-ttu-id="9f966-407">Všimněte si, že *název_souboru* se liší od pravidelných řetězcový literál v tom, že řídicí znaky nejsou zpracovávány; "`\`" znak jednoduše určuje v rámci běžných lomítka *název_souboru*.</span><span class="sxs-lookup"><span data-stu-id="9f966-407">Note that a *file_name* differs from a regular string literal in that escape characters are not processed; the "`\`" character simply designates an ordinary backslash character within a *file_name*.</span></span>

### <a name="pragma-directives"></a><span data-ttu-id="9f966-408">Direktivy pragma</span><span class="sxs-lookup"><span data-stu-id="9f966-408">Pragma directives</span></span>

<span data-ttu-id="9f966-409">`#pragma` Direktiva předzpracování se používá k určení volitelné kontextové informace pro kompilátor.</span><span class="sxs-lookup"><span data-stu-id="9f966-409">The `#pragma` preprocessing directive is used to specify optional contextual information to the compiler.</span></span> <span data-ttu-id="9f966-410">Informace poskytnuty v `#pragma` – direktiva se nikdy nezmění sémantiku programu.</span><span class="sxs-lookup"><span data-stu-id="9f966-410">The information supplied in a `#pragma` directive will never change program semantics.</span></span>

```antlr
pp_pragma
    : whitespace? '#' whitespace? 'pragma' whitespace pragma_body pp_new_line
    ;

pragma_body
    : pragma_warning_body
    ;
```

<span data-ttu-id="9f966-411">Jazyk C# poskytuje `#pragma` direktivy řízení upozornění kompilátoru.</span><span class="sxs-lookup"><span data-stu-id="9f966-411">C# provides `#pragma` directives to control compiler warnings.</span></span> <span data-ttu-id="9f966-412">Budoucí verze jazyka mohou zahrnovat další `#pragma` direktivy.</span><span class="sxs-lookup"><span data-stu-id="9f966-412">Future versions of the language may include additional `#pragma` directives.</span></span> <span data-ttu-id="9f966-413">Aby vzájemná funkční spolupráce s jinými kompilátory C#, kompilátor jazyka Microsoft C# bez vyvolání chyby při kompilaci pro neznámý `#pragma` direktivy, proveďte tyto direktivy ale generovat upozornění.</span><span class="sxs-lookup"><span data-stu-id="9f966-413">To ensure interoperability with other C# compilers, the Microsoft C# compiler does not issue compilation errors for unknown `#pragma` directives; such directives do however generate warnings.</span></span>

#### <a name="pragma-warning"></a><span data-ttu-id="9f966-414">– Direktiva pragma upozornění</span><span class="sxs-lookup"><span data-stu-id="9f966-414">Pragma warning</span></span>

<span data-ttu-id="9f966-415">`#pragma warning` – Direktiva slouží k zakázání nebo obnovit všechny nebo konkrétní sadu upozornění zpráv během kompilace programu následující text.</span><span class="sxs-lookup"><span data-stu-id="9f966-415">The `#pragma warning` directive is used to disable or restore all or a particular set of warning messages during compilation of the subsequent program text.</span></span>

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

<span data-ttu-id="9f966-416">A `#pragma warning` direktiva, která vynechává seznam upozornění ovlivňuje všechna upozornění.</span><span class="sxs-lookup"><span data-stu-id="9f966-416">A `#pragma warning` directive that omits the warning list affects all warnings.</span></span> <span data-ttu-id="9f966-417">A `#pragma warning` – direktiva obsahuje seznam upozornění se týká pouze upozornění, které jsou uvedeny v seznamu.</span><span class="sxs-lookup"><span data-stu-id="9f966-417">A `#pragma warning` directive the includes a warning list affects only those warnings that are specified in the list.</span></span>

<span data-ttu-id="9f966-418">A `#pragma warning disable` direktiv zakáže všechny nebo danou sadu upozornění.</span><span class="sxs-lookup"><span data-stu-id="9f966-418">A `#pragma warning disable` directive disables all or the given set of warnings.</span></span>

<span data-ttu-id="9f966-419">A `#pragma warning restore` direktiv obnoví všechny nebo danou sadu upozornění na stav, který byl v platnosti na začátku kompilační jednotky.</span><span class="sxs-lookup"><span data-stu-id="9f966-419">A `#pragma warning restore` directive restores all or the given set of warnings to the state that was in effect at the beginning of the compilation unit.</span></span> <span data-ttu-id="9f966-420">Všimněte si, že pokud konkrétní upozornění zakázal externě, `#pragma warning restore` (, jestli se pro všechny nebo konkrétního upozornění) nebude znovu povolit tohoto upozornění.</span><span class="sxs-lookup"><span data-stu-id="9f966-420">Note that if a particular warning was disabled externally, a `#pragma warning restore` (whether for all or the specific warning) will not re-enable that warning.</span></span>

<span data-ttu-id="9f966-421">Následující příklad ukazuje použití `#pragma warning` dočasně zakázat upozornění oznamují zastaralé členy odkazují, pomocí čísla upozornění z kompilátoru jazyka Microsoft C#.</span><span class="sxs-lookup"><span data-stu-id="9f966-421">The following example shows use of `#pragma warning` to temporarily disable the warning reported when obsoleted members are referenced, using the warning number from the Microsoft C# compiler.</span></span>
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
