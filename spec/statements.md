---
ms.openlocfilehash: 94346034a667ad4af26796c0c4bbc96d6ed79aba
ms.sourcegitcommit: 7f7fc6e9e195e51b7ff8229aeaa70aa9fbbb63cb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/10/2019
ms.locfileid: "70876836"
---
# <a name="statements"></a>Příkazy

C#poskytuje celou řadu příkazů. Většina těchto příkazů bude obeznámena s vývojáři, kteří mají program v jazyce C a C++.

```antlr
statement
    : labeled_statement
    | declaration_statement
    | embedded_statement
    ;

embedded_statement
    : block
    | empty_statement
    | expression_statement
    | selection_statement
    | iteration_statement
    | jump_statement
    | try_statement
    | checked_statement
    | unchecked_statement
    | lock_statement
    | using_statement
    | yield_statement
    | embedded_statement_unsafe
    ;
```

Neterminál *embedded_statement* se používá pro příkazy, které se zobrazí v jiných příkazech. Použití příkazu *embedded_statement* spíše než *příkaz* vyloučí v těchto kontextech příkazy deklarace a příkazy s popiskem. Příklad
```csharp
void F(bool b) {
    if (b)
        int i = 44;
}
```
Výsledkem je chyba při kompilaci, protože `if` příkaz vyžaduje *embedded_statement* , nikoli *příkaz* pro svou větev if. Pokud byl tento kód povolen, proměnná `i` by byla deklarována, ale nebyla nikdy použita. Všimněte si však, že `i`vložením deklarace do bloku je platný příklad.

## <a name="end-points-and-reachability"></a>Koncové body a dosažitelnost

Každý příkaz má ***koncový bod***. V intuitivních výrazech je koncový bod příkazu umístěním, které bezprostředně následuje za příkazem. Pravidla spouštění pro složené příkazy (příkazy, které obsahují vložené příkazy) určují akci, která je provedena, když ovládací prvek dosáhne koncového bodu vloženého příkazu. Například když ovládací prvek dosáhne koncového bodu příkazu v bloku, je ovládací prvek převeden na další příkaz v bloku.

Pokud je možné, že je příkaz dosažitelný provedením, je příkaz označen jako ***dostupný***. Naopak, pokud není k dispozici možnost provedení příkazu, bude prohlášení ***nedostupné***.

V příkladu
```csharp
void F() {
    Console.WriteLine("reachable");
    goto Label;
    Console.WriteLine("unreachable");
    Label:
    Console.WriteLine("reachable");
}
```
druhé vyvolání `Console.WriteLine` je nedosažitelné, protože neexistuje možnost, že se příkaz spustí.

Pokud kompilátor zjistí, že je příkaz nedosažitelný, je hlášeno upozornění. Konkrétně není chyba, pokud příkaz nebude dostupný.

Chcete-li určit, zda je konkrétní příkaz nebo koncový bod dosažitelný, kompilátor provede analýzu toků podle pravidel dostupnosti definovaných pro každý příkaz. Analýza toku bere v úvahu hodnoty konstantních výrazů ([konstantní výrazy](expressions.md#constant-expressions)), které řídí chování příkazů, ale možné hodnoty nekonstantních výrazů nejsou považovány za. Jinými slovy pro účely analýzy toku řízení je nekonstantní výraz daného typu považován za to, že má libovolnou možnou hodnotu tohoto typu.

V příkladu
```csharp
void F() {
    const int i = 1;
    if (i == 2) Console.WriteLine("unreachable");
}
```
logický výraz `if` příkazu je konstantní výraz, protože oba operandy `==` operátoru jsou konstanty. Jelikož je konstantní výraz vyhodnocen v době kompilace, což vyprodukuje hodnotu `false` `Console.WriteLine` , vyvolání je považováno za nedosažitelné. Pokud `i` se ale změní na lokální proměnnou
```csharp
void F() {
    int i = 1;
    if (i == 2) Console.WriteLine("reachable");
}
```
`Console.WriteLine` volání je považováno za dosažitelné, i když ve skutečnosti nebude nikdy provedeno.

*Blok* člena funkce je vždy považován za dostupný. Po úspěšném vyhodnocení pravidel dostupnosti každého příkazu v bloku je možné určit dostupnost jakéhokoli daného příkazu.

V příkladu
```csharp
void F(int x) {
    Console.WriteLine("start");
    if (x < 0) Console.WriteLine("negative");
}
```
dostupnost druhé `Console.WriteLine` je určena následujícím způsobem:

*  První `Console.WriteLine` příkaz výrazu je dosažitelný, protože blok `F` metody je dosažitelný.
*  Koncový bod prvního `Console.WriteLine` příkazu výrazu je dosažitelný, protože tento příkaz je dosažitelný.
*  Příkaz je dosažitelný, protože koncový bod prvního `Console.WriteLine` příkazu výrazu je dosažitelný. `if`
*  Druhý `Console.WriteLine` příkaz výrazu je dosažitelný, protože logický výraz `if` příkazu nemá konstantní hodnotu `false`.

Existují dvě situace, ve kterých se jedná o chybu při kompilaci koncového bodu příkazu, aby byla dostupná:

*  Vzhledem k tomu, že příkaznepovolujepřesměrovatoddílpřepínačedodalšíhooddílupřepínače,jednáseochybupřikompilacikoncovéhoboduseznamupříkazůoddílupřepínače,abybyldostupný.`switch` Pokud k této chybě dojde, obvykle se jedná o indikaci `break` chybějícího příkazu.
*  Jedná se o chybu při kompilaci koncového bodu bloku členu funkce, který počítá hodnotu, která je dostupná. Pokud k této chybě dojde, obvykle se jedná o indikaci `return` chybějícího příkazu.

## <a name="blocks"></a>Bloky

*Blok* povoluje zápis více příkazů v kontextech, kde je povolen jediný příkaz.

```antlr
block
    : '{' statement_list? '}'
    ;
```

*Blok* se skládá z volitelných *statement_list* ([seznamů příkazů](statements.md#statement-lists)) uzavřených do složených závorek. Pokud je seznam příkazů vynechán, je tento blok označován jako prázdný.

Blok může obsahovat příkazy deklarace ([příkazy deklarace](statements.md#declaration-statements)). Rozsah místní proměnné nebo konstanty deklarované v bloku je blok.

Blok se spustí takto:

*  Pokud je blok prázdný, řízení se převede na koncový bod bloku.
*  Pokud blok není prázdný, ovládací prvek se přenese do seznamu příkazů. Když a když ovládací prvek dosáhne koncového bodu seznamu příkazů, ovládací prvek se převede na koncový bod bloku.

Seznam příkazů bloku je dosažitelný, pokud je samotný blok dostupný.

Koncový bod bloku je dosažitelný, pokud je blok prázdný nebo pokud je koncový bod seznamu příkazů dostupný.

*Blok* , který obsahuje jeden nebo více `yield` příkazů ([příkaz yield](statements.md#the-yield-statement)), se nazývá blok iterátoru. Bloky iterátoru slouží k implementaci členů funkce jako iterátory ([iterátory](classes.md#iterators)). U bloků iterátoru platí některá další omezení:

*  Jedná se o chybu `return` při kompilaci, aby se příkaz objevil v bloku iterátoru (ale `yield return` příkazy jsou povoleny).
*  Jedná se o chybu při kompilaci, aby blok iterátoru obsahoval nezabezpečený kontext ([nezabezpečené](unsafe-code.md#unsafe-contexts)kontexty). Blok iterátoru vždy definuje bezpečný kontext, a to i v případě, že je jeho deklarace vnořena v nezabezpečeném kontextu.

### <a name="statement-lists"></a>Seznamy příkazů

***Seznam příkazů*** se skládá z jednoho nebo více příkazů zapsaných v sekvenci. Seznamy příkazů se vyskytují v blocích s ( *Blocks*[) a](statements.md#blocks)v *switch_block*s ([příkaz switch](statements.md#the-switch-statement)).

```antlr
statement_list
    : statement+
    ;
```

Seznam příkazů je spuštěn převodem řízení na první příkaz. Když a pokud ovládací prvek dosáhne koncového bodu příkazu, bude ovládací prvek převeden na další příkaz. Když a když ovládací prvek dosáhne koncového bodu posledního příkazu, ovládací prvek se převede na koncový bod seznamu příkazů.

Příkaz v seznamu příkazů je dosažitelný, pokud je splněna alespoň jedna z následujících podmínek:

*  Příkaz je prvním příkazem a samotný seznam příkazů je dosažitelný.
*  Koncový bod předchozího příkazu je dosažitelný.
*  Příkaz je příkaz s popiskem a popisek je odkazován pomocí dosažitelného `goto` příkazu.

Koncový bod seznamu příkazů je dosažitelný, pokud je koncový bod posledního příkazu v seznamu dosažitelný.

## <a name="the-empty-statement"></a>Prázdný příkaz

*Empty_statement* neprovede žádnou akci.

```antlr
empty_statement
    : ';'
    ;
```

Prázdný příkaz se používá, pokud nejsou žádné operace k provedení v kontextu, kde je vyžadován příkaz.

Provedení prázdného příkazu jednoduše převede řízení na koncový bod příkazu. Proto je koncový bod prázdného příkazu dosažitelný, pokud je prázdný příkaz dostupný.

Prázdný příkaz lze použít při zápisu `while` příkazu s textem s hodnotou null:
```csharp
bool ProcessMessage() {...}

void ProcessMessages() {
    while (ProcessMessage())
        ;
}
```

Prázdný příkaz lze také použít k deklaraci popisku těsně před uzavíracím znakem "`}`" bloku:
```csharp
void F() {
    ...
    if (done) goto exit;
    ...
    exit: ;
}
```

## <a name="labeled-statements"></a>Příkazy s popiskem

*Labeled_statement* umožňuje, aby příkaz byl předponou. Příkazy s popiskem jsou povolené v blocích, ale nejsou povolené jako vložené příkazy.

```antlr
labeled_statement
    : identifier ':' statement
    ;
```

Příkaz s popiskem deklaruje popisek s názvem daným *identifikátorem*. Rozsah popisku je celý blok, ve kterém je popisek deklarován, včetně všech vnořených bloků. Jedná se o chybu při kompilaci pro dva popisky se stejným názvem, aby měly překrývající se obory.

Na popisek lze odkazovat z `goto` příkazů ([příkaz goto](statements.md#the-goto-statement)) v rámci rozsahu popisku. To znamená, `goto` že příkazy mohou přenášet řízení v rámci bloků a mimo bloky, ale nikdy do bloků.

Popisky mají vlastní prostor deklarací a neovlivňují jiné identifikátory. Příklad
```csharp
int F(int x) {
    if (x >= 0) goto x;
    x = -x;
    x: return x;
}
```
je platný a používá název `x` jako parametr i popisek.

Provedení příkaz s popiskem odpovídá přesně provedení příkazu za popiskem.

Kromě dosažitelnosti, které poskytuje běžný tok řízení, je příkaz s popiskem dosažitelný, pokud je popisek odkazován pomocí dosažitelného `goto` příkazu. Jímka `finally` `finally` `try`Pokud je `try` příkaz uvnitř, který obsahuje blok, a příkaz s popiskem je mimo a koncový bod bloku je nedosažitelný, pak příkaz s popiskem není dosažitelný z `goto` Tento `goto` příkaz.)

## <a name="declaration-statements"></a>Příkazy deklarace

*Declaration_statement* deklaruje místní proměnnou nebo konstantu. Příkazy deklarace jsou povolené v blocích, ale nejsou povolené jako vložené příkazy.

```antlr
declaration_statement
    : local_variable_declaration ';'
    | local_constant_declaration ';'
    ;
```

### <a name="local-variable-declarations"></a>Deklarace místních proměnných

*Local_variable_declaration* deklaruje jednu nebo více místních proměnných.

```antlr
local_variable_declaration
    : local_variable_type local_variable_declarators
    ;

local_variable_type
    : type
    | 'var'
    ;

local_variable_declarators
    : local_variable_declarator
    | local_variable_declarators ',' local_variable_declarator
    ;

local_variable_declarator
    : identifier
    | identifier '=' local_variable_initializer
    ;

local_variable_initializer
    : expression
    | array_initializer
    | local_variable_initializer_unsafe
    ;
```

*Local_variable_type* *local_variable_declaration* buď přímo určuje typ proměnných zavedených deklarací, nebo označuje identifikátor `var` , který má být typu odvozen na základě inicializátor. Po typu následuje seznam *local_variable_declarator*, z nichž každá zavádí novou proměnnou. *Local_variable_declarator* se skládá z *identifikátoru* , který proměnnou pojmenovává, volitelně následovaný "`=`" tokenem a *local_variable_initializer* , který poskytuje počáteční hodnotu proměnné.

V kontextu deklarace lokální proměnné funguje var jako kontextové klíčové slovo ([klíčová slova](lexical-structure.md#keywords)). Pokud je *local_variable_type* zadán jako `var` a žádný typ s názvem `var` není v oboru, deklarace je ***implicitně typovou deklarací lokální proměnné***, jejíž typ je odvozen od typu přidruženého inicializátoru. vyjádření. Implicitně typované deklarace lokálních proměnných podléhá následujícím omezením:

*  *Local_variable_declaration* nemůže obsahovat více *local_variable_declarator*s.
*  *Local_variable_declarator* musí zahrnovat *local_variable_initializer*.
*  *Local_variable_initializer* musí být *výraz*.
*  *Výraz* inicializátoru musí mít typ pro čas kompilace.
*  *Výraz* inicializátoru nemůže odkazovat na samotný deklarovaný proměnnou.

Následují příklady nesprávných implicitních typů deklarací místních proměnných:

```csharp
var x;               // Error, no initializer to infer type from
var y = {1, 2, 3};   // Error, array initializer not permitted
var z = null;        // Error, null does not have a type
var u = x => x + 1;  // Error, anonymous functions do not have a type
var v = v++;         // Error, initializer cannot refer to variable itself
```

Hodnota místní proměnné je získána ve výrazu pomocí *simple_name* ([jednoduché názvy](expressions.md#simple-names)) a hodnota lokální proměnné je upravena pomocí *přiřazení* ([operátory přiřazení](expressions.md#assignment-operators)). Místní proměnná musí být jednoznačně přiřazena ([jednoznačné přiřazení](variables.md#definite-assignment)) na každém místě, kde je jeho hodnota získána.

Rozsah místní proměnné deklarované ve *local_variable_declaration* je blok, ve kterém se nachází deklarace. Jedná se o chybu, která odkazuje na místní proměnnou v textové pozici, která předchází *local_variable_declarator* místní proměnné. V rámci rozsahu místní proměnné se jedná o chybu při kompilaci k deklaraci jiné místní proměnné nebo konstanty se stejným názvem.

Deklarace místní proměnné, která deklaruje více proměnných, je ekvivalentní více deklaracím jednotlivých proměnných stejného typu. Kromě toho inicializátor proměnné v deklaraci lokální proměnné odpovídá přesně příkazu přiřazení, který je vložen ihned po deklaraci.

Příklad
```csharp
void F() {
    int x = 1, y, z = x * 2;
}
```
odpovídá přesně
```csharp
void F() {
    int x; x = 1;
    int y;
    int z; z = x * 2;
}
```

V implicitní typové deklaraci lokální proměnné je typ deklarované místní proměnné stejný jako typ výrazu použitého k inicializaci proměnné. Příklad:
```csharp
var i = 5;
var s = "Hello";
var d = 1.0;
var numbers = new int[] {1, 2, 3};
var orders = new Dictionary<int,Order>();
```

Implicitně typové deklarace místních proměnných jsou přesně ekvivalentem následujících explicitně typované deklarace:
```csharp
int i = 5;
string s = "Hello";
double d = 1.0;
int[] numbers = new int[] {1, 2, 3};
Dictionary<int,Order> orders = new Dictionary<int,Order>();
```

### <a name="local-constant-declarations"></a>Deklarace místní konstanty

*Local_constant_declaration* deklaruje jednu nebo více místních konstant.

```antlr
local_constant_declaration
    : 'const' type constant_declarators
    ;

constant_declarators
    : constant_declarator (',' constant_declarator)*
    ;

constant_declarator
    : identifier '=' constant_expression
    ;
```

*Typ* *local_constant_declaration* určuje typ konstant zavedený deklarací. Po typu následuje seznam *constant_declarator*, z nichž každá zavádí novou konstantu. *Constant_declarator* se skládá z *identifikátoru* , který obsahuje název konstanty následovaný "`=`" tokenem následovaným *constant_expression* ([konstantními výrazy](expressions.md#constant-expressions)), které poskytují hodnotu konstanty.

*Typ* a *constant_expression* deklarace místní konstanty musí splňovat stejná pravidla jako deklarace konstantního člena ([konstanty](classes.md#constants)).

Hodnota místní konstanty je získána ve výrazu pomocí *simple_name* ([jednoduché názvy](expressions.md#simple-names)).

Rozsah místní konstanty je blok, ve kterém k deklaraci dojde. Odkaz na místní konstantu v textové pozici, která předchází jeho *constant_declarator*, je chyba. V rámci rozsahu místní konstanty se jedná o chybu při kompilaci k deklaraci jiné místní proměnné nebo konstanty se stejným názvem.

Deklarace místní konstanty, která deklaruje více konstant, je ekvivalentní více deklaracím s jedním konstantou stejného typu.

## <a name="expression-statements"></a>Příkazy výrazu

*Expression_statement* vyhodnocuje daný výraz. Hodnota vypočítaná výrazem, pokud existuje, je zahozena.

```antlr
expression_statement
    : statement_expression ';'
    ;

statement_expression
    : invocation_expression
    | null_conditional_invocation_expression
    | object_creation_expression
    | assignment
    | post_increment_expression
    | post_decrement_expression
    | pre_increment_expression
    | pre_decrement_expression
    | await_expression
    ;
```

Ne všechny výrazy jsou povoleny jako příkazy. Konkrétně se jako příkazy nepovolují `x + y` výrazy `x == 1` jako a, které pouze počítají hodnotu (která se zahodí).

Provedení *expression_statement* vyhodnotí obsažený výraz a poté přenáší řízení na koncový bod *expression_statement*. Koncový bod *expression_statement* je dosažitelný, pokud je tento *expression_statement* dosažitelný.

## <a name="selection-statements"></a>Příkazy výběru

Příkazy výběru vyberou jeden z několika možných příkazů pro spuštění na základě hodnoty nějakého výrazu.

```antlr
selection_statement
    : if_statement
    | switch_statement
    ;
```

### <a name="the-if-statement"></a>Příkaz if

`if` Příkaz vybere příkaz pro spuštění na základě hodnoty logického výrazu.

```antlr
if_statement
    : 'if' '(' boolean_expression ')' embedded_statement
    | 'if' '(' boolean_expression ')' embedded_statement 'else' embedded_statement
    ;
```

Část je přidružena k lexikálnímu `if` nejbližšímu, který je povolen syntaxí. `else` `if` Proto příkaz formuláře
```csharp
if (x) if (y) F(); else G();
```
je ekvivalentem
```csharp
if (x) {
    if (y) {
        F();
    }
    else {
        G();
    }
}
```

`if` Příkaz se spustí takto:

*  *Boolean_expression* ([booleovské výrazy](expressions.md#boolean-expressions)) jsou vyhodnoceny.
*  Pokud logický výraz vrací `true`, je ovládací prvek převeden do prvního vloženého příkazu. Když a když ovládací prvek dosáhne koncového bodu tohoto příkazu, ovládací prvek se převede na koncový bod `if` příkazu.
*  Pokud logický výraz `false` vyhodnotí a `else` Pokud je část přítomna, řízení je převedeno do druhého vloženého příkazu. Když a když ovládací prvek dosáhne koncového bodu tohoto příkazu, ovládací prvek se převede na koncový bod `if` příkazu.
*  Pokud logický výraz vrací `false` a `else` Pokud část není přítomna, řízení je převedeno na koncový bod `if` příkazu.

První vložený příkaz `if` příkazu je dosažitelný, `if` Pokud je příkaz dosažitelný a logický výraz nemá konstantní hodnotu `false`.

Druhý vložený příkaz `if` příkazu, je-li k dispozici, je dosažitelný, `if` Pokud je příkaz dosažitelný a logický výraz nemá konstantní hodnotu. `true`

Koncový bod `if` příkazu je dosažitelný, pokud je koncový bod alespoň jednoho z jeho vložených příkazů dosažitelný. Kromě `if` toho koncový bod příkazu bez `else` části `if` je dosažitelný, pokud je příkaz dosažitelný a logický výraz nemá konstantní hodnotu `true`.

### <a name="the-switch-statement"></a>Příkaz switch

Příkaz switch vybere příkaz pro spuštění seznamu příkazů, který má přidružený popisek přepínače, který odpovídá hodnotě výrazu přepínače.

```antlr
switch_statement
    : 'switch' '(' expression ')' switch_block
    ;

switch_block
    : '{' switch_section* '}'
    ;

switch_section
    : switch_label+ statement_list
    ;

switch_label
    : 'case' constant_expression ':'
    | 'default' ':'
    ;
```

*Switch_statement* se skládá z klíčového `switch`slova následovaného výrazem v závorkách (nazývaný výraz Switch) následovaným *switch_block*. *Switch_block* se skládá z nuly nebo více *switch_section*s uzavřenými závorkami. Každý *switch_section* se skládá z jednoho nebo více *switch_label*, po kterých následuje *statement_list* ([seznamy příkazů](statements.md#statement-lists)).

Typ`switch` ***řízení*** příkazu je vytvořen výrazem přepínače.

*  Pokud je `sbyte`typ výrazu přepínače `ushort`, `byte`, `short` ,,`uint` ,,`long` ,`char`,, ,`string`nebo `int` `ulong` `bool`  *enum_type*, nebo pokud se jedná o typ s možnou hodnotou null odpovídající jednomu z těchto typů, pak to je typ `switch` řízení příkazu.
*  V opačném případě musí existovat přesně jeden uživatelem definovaný implicitní převod ([uživatelem definované převody](conversions.md#user-defined-conversions)) z typu výrazu Switch na jeden z následujících možných typů řízení `sbyte`:, `byte`,, `ushort` `short` , `int` ,`uint`, ,`char`,, nebo`string`, typ s možnou hodnotou null, který odpovídá jednomu z těchto typů. `ulong` `long`
*  V opačném případě, pokud žádný takový implicitní převod neexistuje nebo pokud existuje více než jeden takový implicitní převod, dojde k chybě při kompilaci.

Konstantní výraz každého `case` popisku musí znamenat hodnotu, která je implicitně převoditelná ([implicitní převod](conversions.md#implicit-conversions)) na typ `switch` řízení příkazu. Pokud dva nebo více `case` jmenovek v rámci stejného `switch` příkazu určuje stejnou konstantní hodnotu, dojde k chybě v době kompilace.

V příkazu switch může být nejvýše `default` jeden popisek.

`switch` Příkaz se spustí takto:

*  Výraz přepínače je vyhodnocen a převeden na typ řízení.
*  Pokud je jedna z konstant určená v `case` popisku v rámci stejného `switch` příkazu rovna hodnotě výrazu Switch, je ovládací prvek převeden do seznamu příkazů po odpovídajícím `case` popisku.
*  Pokud žádná `case` konstanta zadaná v popisku v rámci stejného `switch` příkazu není rovna hodnotě `default` výrazu Switch, a pokud je popisek přítomen, ovládací prvek `default` se přenese do seznamu příkazů za popisek.
*  Pokud žádná `case` konstanta zadaná v popisku v rámci stejného `switch` příkazu není rovna hodnotě výrazu Switch, a pokud není k dispozici žádný `default` popisek, `switch` je ovládací prvek převeden na koncový bod příkazu.

Pokud je koncový bod seznamu příkazů oddílu přepínače dosažitelný, dojde k chybě při kompilaci. Tento postup se označuje jako pravidlo nespadající do rozsahu. Příklad
```csharp
switch (i) {
case 0:
    CaseZero();
    break;
case 1:
    CaseOne();
    break;
default:
    CaseOthers();
    break;
}
```
je platný, protože žádný oddíl Switch má dosažitelný koncový bod. Na rozdíl od jazyka C++C a provedení oddílu přepínače není povoleno "přejít do" do dalšího oddílu Switch a příklad
```csharp
switch (i) {
case 0:
    CaseZero();
case 1:
    CaseZeroOrOne();
default:
    CaseAny();
}
```
má za následek chybu při kompilaci. V případě provedení oddílu přepínače, který je následován provedením jiného oddílu přepínače, je nutné použít explicitní `goto case` příkaz `goto default` nebo.
```csharp
switch (i) {
case 0:
    CaseZero();
    goto case 1;
case 1:
    CaseZeroOrOne();
    goto default;
default:
    CaseAny();
    break;
}
```

V *switch_section*je povoleno více popisků. Příklad
```csharp
switch (i) {
case 0:
    CaseZero();
    break;
case 1:
    CaseOne();
    break;
case 2:
default:
    CaseTwo();
    break;
}
```
je platný. Tento příklad nerušuje pravidlo "nespadají do", protože popisky `case 2:` a `default:` jsou součástí stejného *switch_section*.

Pravidlo "nespadající do" zabraňuje společné třídě chyb, ke kterým dojde v C a C++ kdy `break` jsou výrazy náhodně vynechány. Kromě toho z důvodu tohoto pravidla mohou být oddíly `switch` přepínače v příkazu libovolně přeuspořádány bez ovlivnění chování příkazu. Například oddíly `switch` výše uvedeného příkazu mohou být vráceny bez ovlivnění chování příkazu:
```csharp
switch (i) {
default:
    CaseAny();
    break;
case 1:
    CaseZeroOrOne();
    goto default;
case 0:
    CaseZero();
    goto case 1;
}
```

Seznam příkazů oddílu přepínače obvykle končí v `break`příkazu, `goto case`nebo `goto default` , ale všechny konstrukce, které vykreslují koncový bod seznamu příkazů, jsou povoleny. Například příkaz, který `while` je ovládán pomocí logického výrazu `true` , je znám tak, že nikdy nedosáhnou koncového bodu. Stejně tak příkaz `return`nebovždy přenáší řízení jinde a nikdy nedosáhne svého koncového bodu. `throw` Proto je následující příklad platný:
```csharp
switch (i) {
case 0:
    while (true) F();
case 1:
    throw new ArgumentException();
case 2:
    return;
}
```

Typ řízení `switch` příkazu může být typ `string`. Příklad:
```csharp
void DoCommand(string command) {
    switch (command.ToLower()) {
    case "run":
        DoRun();
        break;
    case "save":
        DoSave();
        break;
    case "quit":
        DoQuit();
        break;
    default:
        InvalidCommand(command);
        break;
    }
}
```

Podobně jako operátory rovnosti řetězců ([operátory rovnosti řetězců](expressions.md#string-equality-operators)) `switch` , příkaz rozlišuje velká a malá písmena a provede daný oddíl přepínače pouze v `case` případě, že řetězec výrazu Switch přesně odpovídá konstantě popisku.

Pokud je `switch` `string`typ řízení příkazu, hodnota `null` je povolena jako konstanta jmenovky Case.

*Statement_list*s *switch_block* může obsahovat příkazy deklarace ([příkazy deklarace](statements.md#declaration-statements)). Rozsah místní proměnné nebo konstanty deklarované v bloku Switch je blok přepínače.

Seznam příkazů daného oddílu přepínače je dosažitelný, pokud `switch` je příkaz dosažitelný a alespoň jedna z následujících možností je pravdivá:

*  Výraz přepínače je nekonstantní hodnota.
*  Výraz přepínače je konstantní hodnota, která odpovídá `case` popisku v části Switch.
*  Výraz přepínače je konstantní hodnota, která neodpovídá žádnému `case` popisku a oddíl Switch `default` obsahuje popisek.
*  Na jmenovku přepínače oddílu Switch se odkazuje dosažitelný `goto case` příkaz nebo. `goto default`

Koncový bod `switch` příkazu je dosažitelný, pokud je splněna alespoň jedna z následujících podmínek:

*  Příkaz obsahuje `break` dostupný příkaz, který ukončí příkaz.`switch` `switch`
*  Příkaz je dosažitelný, výraz přepínače je nekonstantní hodnota a není k dispozici žádný `default` popisek. `switch`
*  Příkaz je dosažitelný, výraz přepínače je konstantní hodnota, která neodpovídá žádnému `case` popisku a není k dispozici `default` žádný popisek. `switch`

## <a name="iteration-statements"></a>Příkazy iterace

Příkazy iterace opakovaně spouštějí vložený příkaz.

```antlr
iteration_statement
    : while_statement
    | do_statement
    | for_statement
    | foreach_statement
    ;
```

### <a name="the-while-statement"></a>Příkaz While

`while` Příkaz podmíněně spustí vložený příkaz nula nebo vícekrát.

```antlr
while_statement
    : 'while' '(' boolean_expression ')' embedded_statement
    ;
```

`while` Příkaz se spustí takto:

*  *Boolean_expression* ([booleovské výrazy](expressions.md#boolean-expressions)) jsou vyhodnoceny.
*  Pokud logický výraz vrací `true`, je ovládací prvek převeden do vloženého příkazu. Když a pokud ovládací prvek dosáhne koncového bodu vloženého příkazu (případně z provádění `continue` příkazu), řízení je převedeno na začátek `while` příkazu.
*  Je-li logický výraz `false`výsledkem, je ovládací prvek převeden na koncový bod `while` příkazu.

`while` V rámci vloženého příkazu `break` příkazu může být příkaz ([příkaz break](statements.md#the-break-statement)) použit k přenosu `while` ovládacího prvku na koncový bod příkazu (takže koncová iterace vloženého příkazu) a `continue` příkaz ([příkaz Continue](statements.md#the-continue-statement)) lze použít k přenosu ovládacího prvku do koncového bodu vloženého příkazu (čímž se provádí další `while` iterace příkazu).

Vložený příkaz `while` příkazu je dosažitelný, `while` Pokud je příkaz dosažitelný a logický výraz nemá konstantní hodnotu `false`.

Koncový bod `while` příkazu je dosažitelný, pokud je splněna alespoň jedna z následujících podmínek:

*  Příkaz obsahuje `break` dostupný příkaz, který ukončí příkaz.`while` `while`
*  Příkaz je dosažitelný a logický výraz nemá konstantní hodnotu `true`. `while`

### <a name="the-do-statement"></a>Příkaz do

`do` Příkaz podmíněně spustí vložený příkaz jednou nebo vícekrát.

```antlr
do_statement
    : 'do' embedded_statement 'while' '(' boolean_expression ')' ';'
    ;
```

`do` Příkaz se spustí takto:

*  Ovládací prvek se přenese do vloženého příkazu.
*  Když a pokud ovládací prvek dosáhne koncového bodu vloženého příkazu (případně z provádění `continue` příkazu), je vyhodnocen *Boolean_expression* ([booleovské výrazy](expressions.md#boolean-expressions)). Je-li logický výraz `true`výsledkem, je ovládací prvek převeden na začátek `do` příkazu. V opačném případě je ovládací prvek převeden na koncový bod `do` příkazu.

`do` V rámci vloženého příkazu `break` příkazu může být příkaz ([příkaz break](statements.md#the-break-statement)) použit k přenosu `do` ovládacího prvku na koncový bod příkazu (takže koncová iterace vloženého příkazu) a `continue` příkaz ([příkaz Continue](statements.md#the-continue-statement)) lze použít k přenosu řízení na koncový bod vloženého příkazu.

Vložený příkaz `do` příkazu je dosažitelný, `do` Pokud je příkaz dosažitelný.

Koncový bod `do` příkazu je dosažitelný, pokud je splněna alespoň jedna z následujících podmínek:

*  Příkaz obsahuje `break` dostupný příkaz, který ukončí příkaz.`do` `do`
*  Koncový bod vloženého příkazu je dosažitelný a logický výraz nemá konstantní hodnotu `true`.

### <a name="the-for-statement"></a>Příkaz for

`for` Příkaz vyhodnocuje sekvenci inicializačních výrazů a potom, zatímco je podmínka pravdivá, opakovaně spouští vložený příkaz a vyhodnocuje sekvenci výrazů iterace.

```antlr
for_statement
    : 'for' '(' for_initializer? ';' for_condition? ';' for_iterator? ')' embedded_statement
    ;

for_initializer
    : local_variable_declaration
    | statement_expression_list
    ;

for_condition
    : boolean_expression
    ;

for_iterator
    : statement_expression_list
    ;

statement_expression_list
    : statement_expression (',' statement_expression)*
    ;
```

*For_initializer*, pokud je k dispozici, se skládá buď z *local_variable_declaration* ([místní proměnná deklarace](statements.md#local-variable-declarations)), nebo seznamu *statement_expression*s ([příkazy výrazu](statements.md#expression-statements)) oddělené čárkami. Rozsah místní proměnné deklarované *for_initializer* začíná na *local_variable_declarator* pro proměnnou a rozšiřuje na konec vloženého příkazu. Obor zahrnuje *for_condition* a *for_iterator*.

*For_condition*, pokud je přítomen, musí být *Boolean_expression* ([booleovské výrazy](expressions.md#boolean-expressions)).

*For_iterator*, pokud je k dispozici, se skládá ze seznamu *statement_expression*s ([příkazy výrazu](statements.md#expression-statements)), které jsou odděleny čárkami.

Příkaz for se spustí takto:

*  Pokud je přítomen *for_initializer* , Inicializátory proměnných nebo výrazy příkazů jsou spouštěny v pořadí, ve kterém jsou zapsány. Tento krok se provádí jenom jednou.
*  Pokud je přítomen *for_condition* , vyhodnotí se.
*  Pokud *for_condition* není k dispozici nebo pokud dojde k vyhodnocení `true`, řízení se přenese do vloženého příkazu. Když a pokud ovládací prvek dosáhne koncového bodu vloženého příkazu (případně z provádění `continue` příkazu), jsou výrazy *for_iterator*, pokud existují, vyhodnocovány v sekvenci a poté je provedena další iterace, počínaje vyhodnocení *for_condition* v kroku výše.
*  Pokud je přítomen *for_condition* a vyhodnocení `false`, řízení je převedeno na `for` koncový bod příkazu.

`for` V rámci vloženého příkazu `break` příkazu může být příkaz ([příkaz break](statements.md#the-break-statement)) použit k přenosu `for` ovládacího prvku na koncový bod příkazu (takže koncová iterace vloženého příkazu) a `continue` příkaz ([příkaz Continue](statements.md#the-continue-statement)) se dá použít k přenosu řízení na koncový bod vloženého příkazu (takže se spustí *for_iterator* a `for` provádí se další iterace příkazu, počínaje *for_condition*).

Vložený příkaz `for` příkazu je dosažitelný, pokud je splněna jedna z následujících podmínek:

*  Příkaz je dosažitelný a není k dispozici žádný *for_condition.* `for`
*  Příkaz je dosažitelný a je přítomen *for_condition* a nemá konstantní hodnotu `false`. `for`

Koncový bod `for` příkazu je dosažitelný, pokud je splněna alespoň jedna z následujících podmínek:

*  Příkaz obsahuje `break` dostupný příkaz, který ukončí příkaz.`for` `for`
*  Příkaz je dosažitelný a je přítomen *for_condition* a nemá konstantní hodnotu `true`. `for`

### <a name="the-foreach-statement"></a>Příkaz foreach

`foreach` Příkaz vytvoří výčet prvků kolekce a spouští vložený příkaz pro každý prvek kolekce.

```antlr
foreach_statement
    : 'foreach' '(' local_variable_type identifier 'in' expression ')' embedded_statement
    ;
```

*Typ* a *identifikátor* `foreach` příkazu deklaruje ***proměnnou iterace*** příkazu. Pokud je `var`identifikátor přidělen jako local_variable_type a žádný typ s názvem není v oboru, proměnná iterace je označována jako implicitně typovou proměnnou iterace a jejím typem je typ prvku `var` `foreach` , jak je uvedeno níže. Proměnná iterace odpovídá místní proměnné určené jen pro čtení s oborem, který se rozšíří přes vložený příkaz. Během provádění `foreach` příkazu představuje proměnná iterace prvek kolekce, pro který je iterace právě prováděna. K chybě při kompilaci dojde v případě, že se vložený příkaz pokusí změnit proměnnou iterace (prostřednictvím přiřazení nebo `++` operátorů a `--` ) nebo předat proměnnou iterace jako `ref` parametr `out` or.

V následujících případech pro `IEnumerable`zkrácení, `IEnumerator` `IEnumerable<T>` , a `IEnumerator<T>` odkazují na odpovídající typy v oborech názvů `System.Collections` a `System.Collections.Generic`.

Nejprve zpracování příkazu foreach v době kompilace Určuje ***typ kolekce***, ***typ enumerátoru*** a ***typ elementu*** výrazu. Toto určení pokračuje následujícím způsobem:

*  Pokud typ `X` *výrazu* je typ pole, pak existuje implicitní `X` referenční `IEnumerable` převod z na rozhraní (od `System.Array` implementace tohoto rozhraní). ***Typ kolekce*** je `IEnumerable` rozhraní, ***typ enumerátoru*** je `IEnumerator` rozhraní a ***typ prvku*** je typ prvku typu `X`pole.
*  Pokud je `X` `IEnumerable` [](conversions.md#implicit-dynamic-conversions)typ výrazu poté, existuje implicitní převod z výrazu na rozhraní (implicitní dynamické převody). `dynamic` ***Typ kolekce*** je `IEnumerable` rozhraní a ***typ enumerátoru*** je `IEnumerator` rozhraní. `dynamic` `object` Pokud je identifikátorpřidělenjakolocal_variable_type,pakjetypprvku,jinak.`var`
*  V opačném případě určete, `X` zda má typ `GetEnumerator` odpovídající metodu:
   * Proveďte vyhledávání členů u typu `X` s identifikátorem `GetEnumerator` bez argumentů typu. Pokud vyhledávání členů nevytvoří shodu, nebo vytvoří nejednoznačnost, nebo vytvoří shodu, která není skupinou metod, vyhledejte vyčíslitelné rozhraní, jak je popsáno níže. Doporučuje se vystavovat upozornění, pokud vyhledávání členů vytvoří cokoli s výjimkou skupiny metod nebo neodpovídá.
   * Proveďte rozlišení přetížení pomocí výsledné skupiny metod a prázdného seznamu argumentů. Pokud výsledkem rozlišení přetížení nejsou žádné použitelné metody, výsledkem je nejednoznačnost nebo výsledkem je jediná nejlepší metoda, ale tato metoda je statická nebo není veřejná, vyhledejte vyčíslitelné rozhraní, jak je popsáno níže. Doporučuje se vydávat upozornění, pokud řešení přetížení vytvoří cokoli kromě nejednoznačné metody veřejné instance nebo žádné použitelné metody.
   * Pokud návratový typ `E` `GetEnumerator` metody není typu třída, struktura ani typ rozhraní, je vytvořena chyba a nejsou provedeny žádné další kroky.
   * Vyhledávání členů se provádí `E` s identifikátorem `Current` bez argumentů typu. Pokud vyhledávání členů nevrátí žádnou shodu, výsledkem je chyba, nebo výsledkem je cokoli s výjimkou veřejné vlastnosti instance, která umožňuje čtení, je vytvořena chyba a nejsou provedeny žádné další kroky.
   * Vyhledávání členů se provádí `E` s identifikátorem `MoveNext` bez argumentů typu. Pokud vyhledávání členů nevrátí žádnou shodu, výsledkem je chyba, nebo je výsledek cokoli kromě skupiny metod, je vytvořena chyba a nejsou provedeny žádné další kroky.
   * Rozlišení přetěžování se provádí ve skupině metod s prázdným seznamem argumentů. Pokud výsledkem rozlišení přetížení nejsou žádné použitelné metody, výsledkem je nejednoznačnost nebo výsledkem je jediná nejlepší metoda, ale tato metoda je buď statická, nebo není veřejná, nebo její návratový typ `bool`není, je vytvořena chyba a nejsou provedeny žádné další kroky.
   * ***Typ kolekce*** je `X`, ***typ enumerátoru*** je `E` `Current` a ***typ elementu*** je typ vlastnosti.

*  V opačném případě Ověřte vyčíslitelné rozhraní:
   * Pokud je mezi všemi typy `Ti` , pro které existuje implicitní převod z `X` na `IEnumerable<Ti>`, existuje jedinečný typ `T` , který `T` `dynamic` není a pro všechny ostatní `Ti` . implicitní převod z `IEnumerable<T>` na `IEnumerable<Ti>`, potom ***typ kolekce*** `IEnumerable<T>`je rozhraní, ***typ enumerátoru*** je rozhraní `IEnumerator<T>`a ***typ elementu*** je `T`.
   * V opačném případě, pokud existuje více než jeden `T`takový typ, je vytvořena chyba a nejsou provedeny žádné další kroky.
   * V opačném případě, pokud existuje implicitní převod `X` z `System.Collections.IEnumerable` na rozhraní, pak ***typ kolekce*** je toto rozhraní, ***typ enumerátoru*** je rozhraní `System.Collections.IEnumerator`a ***typ elementu*** je `object`.
   * V opačném případě se vytvoří chyba a neprovádí se žádné další kroky.

Výše uvedené kroky, pokud bylo úspěšné, jednoznačně vytvoří typ `C`kolekce, typ `E` enumerátoru a typ `T`elementu. Příkaz foreach ve formátu
```csharp
foreach (V v in x) embedded_statement
```
je pak rozbalen na:
```csharp
{
    E e = ((C)(x)).GetEnumerator();
    try {
        while (e.MoveNext()) {
            V v = (V)(T)e.Current;
            embedded_statement
        }
    }
    finally {
        ... // Dispose e
    }
}
```

Proměnná `e` není viditelná ani přístupná k výrazu `x` nebo vloženému příkazu nebo žádnému jinému zdrojovému kódu tohoto programu. Proměnná `v` je určena jen pro čtení v příkazu Embedded. Pokud není k dispozici explicitní převod ([explicitní převody](conversions.md#explicit-conversions)) z `T` typu (typ elementu) na `V` ( *local_variable_type* v příkazu foreach), je vytvořena chyba a nejsou provedeny žádné další kroky. Pokud `x` má hodnotu `null`, `System.NullReferenceException` je vyvolána v době běhu.

Implementace má povolenou implementaci daného příkazu foreach, například z důvodů výkonu, pokud je chování konzistentní s výše uvedeným rozšířením.

Umístění `v` uvnitř smyčky while je důležité pro způsob, jakým je zachycena všemi anonymními funkcemi, ke kterým dochází v *embedded_statement*.

Příklad:
```csharp
int[] values = { 7, 9, 13 };
Action f = null;

foreach (var value in values)
{
    if (f == null) f = () => Console.WriteLine("First value: " + value);
}

f();
```
Pokud `v` byla deklarována mimo smyčku while, bude sdílena mezi všemi iteracemi a její hodnotou za smyčkou for by byla konečná hodnota, `13`, což je `f` to, co by bylo vyvoláno. Místo toho, protože každá iterace má svou `v`vlastní proměnnou, bude hodnota `f` zachycená v první iteraci nadále uchovávat hodnotu `7`, která bude vytištěna. (Poznámka: starší verze C# deklarované `v` mimo smyčku while.)

Tělo bloku finally je konstruováno podle následujících kroků:

*  Pokud existuje implicitní převod z `E` `System.IDisposable` na rozhraní, pak
   *  Pokud `E` je typ hodnoty, která není null, klauzule finally je rozšířena na sémantický ekvivalent:

      ```csharp
      finally {
          ((System.IDisposable)e).Dispose();
      }
      ```

   *  V opačném případě je klauzule finally rozšířena na sémantický ekvivalent:

      ```csharp
      finally {
          if (e != null) ((System.IDisposable)e).Dispose();
      }
      ```

   s výjimkou `E` toho, že pokud je typ hodnoty nebo parametr typu, který je vytvořen jako typ hodnoty, `e` přetypování na `System.IDisposable` nezpůsobí, že by došlo k zabalení.

*  V opačném `E` případě, pokud je zapečetěný typ, je klauzule finally rozbalena na prázdný blok:

   ```csharp
   finally {
   }
   ```

*  V opačném případě je klauzule finally rozšířena na:

   ```csharp
   finally {
       System.IDisposable d = e as System.IDisposable;
       if (d != null) d.Dispose();
   }
   ```    

   Místní proměnná `d` není viditelná ani přístupná pro žádný uživatelský kód. Konkrétně není v konfliktu s žádnou jinou proměnnou, jejíž obor obsahuje blok finally.

Pořadí, ve kterém `foreach` procházejí prvky pole, je následující: Pro jednorozměrná prvky pole jsou prochází ve vzestupném pořadí indexu počínaje indexem `0` a končí indexem. `Length - 1` U multidimenzionálních polí jsou elementy procházeny tak, že jsou nejprve zvyšovány indexy pravého rozměru, potom následující levá dimenze a tak dále doleva.

Následující příklad vytiskne každou hodnotu v dvojrozměrném poli v pořadí prvků:
```csharp
using System;

class Test
{
    static void Main() {
        double[,] values = {
            {1.2, 2.3, 3.4, 4.5},
            {5.6, 6.7, 7.8, 8.9}
        };

        foreach (double elementValue in values)
            Console.Write("{0} ", elementValue);

        Console.WriteLine();
    }
}
```
Výstup vyprodukovaný je následující:
```csharp
1.2 2.3 3.4 4.5 5.6 6.7 7.8 8.9
```

V příkladu
```csharp
int[] numbers = { 1, 3, 5, 7, 9 };
foreach (var n in numbers) Console.WriteLine(n);
```
typ `n` je `int` odvozený`numbers`, typ elementu.

## <a name="jump-statements"></a>Jump – příkazy

Příkazy skoku nepodmíněný přenos řízení.

```antlr
jump_statement
    : break_statement
    | continue_statement
    | goto_statement
    | return_statement
    | throw_statement
    ;
```

Umístění, do kterého příkaz skoku přenáší řízení, se nazývá ***cíl*** příkazu skoku.

V případě, že se k příkazu skoku dojde v rámci bloku a cíl tohoto příkazu skoku je mimo tento blok, je příkaz skoku označován pro ***ukončení*** bloku. Zatímco příkaz skoku může přenést řízení z bloku, nemůže nikdy přenést ovládací prvek do bloku.

Spuštění příkazů skoku je komplikované přítomností `try` v rámci příkazů. V případě absence takových `try` příkazů příkaz skoku nepodmíněné přenáší řízení z příkazu skok na jeho cíl. V přítomnosti těchto `try` vydaných příkazů je provádění složitější. Pokud příkaz skoku `try` ukončí jeden nebo více bloků s přidruženými `finally` bloky, `finally` je počáteční přenos ovládacího prvku do bloku nejvnitřnějšího `try` příkazu. Když a pokud ovládací prvek dosáhne koncového bodu `finally` bloku, je ovládací prvek převeden `finally` do bloku dalšího ohraničujícího `try` příkazu. Tento proces se opakuje, `finally` dokud nebudou provedeny bloky všech `try` předaných příkazů.

V příkladu
```csharp
using System;

class Test
{
    static void Main() {
        while (true) {
            try {
                try {
                    Console.WriteLine("Before break");
                    break;
                }
                finally {
                    Console.WriteLine("Innermost finally block");
                }
            }
            finally {
                Console.WriteLine("Outermost finally block");
            }
        }
        Console.WriteLine("After break");
    }
}
```
bloky přidružené ke dvěma `try` příkazům jsou spouštěny před přesměrováním ovládacího prvku na cíl příkazu skoku. `finally`

Výstup vyprodukovaný je následující:
```
Before break
Innermost finally block
Outermost finally block
After break
```

### <a name="the-break-statement"></a>Příkaz break

Příkazukončí`switch`nejbližší ohraničující `while` příkaz,`foreach` ,, nebo. `do` `break` `for`

```antlr
break_statement
    : 'break' ';'
    ;
```

`break` Cílem příkazu je koncový bod nejbližšího `do` `switch`ohraničujícího `while`příkazu,, `for`, nebo `foreach` . `switch` `do` `while`Pokud příkaz není uzavřen v příkazu, ,`for`, nebo`foreach` , dojde k chybě při kompilaci. `break`

Je- `switch` `while` `do` `for`li více příkazů,, `foreach` , nebo v sobě vnořeno, příkaz se vztahuje pouze na nejvnitřnější výraz. `break` Chcete-li přenést řízení napříč více úrovněmi vnoření `goto` , je nutné použít příkaz ([příkaz goto](statements.md#the-goto-statement)).

Příkaz nemůže ukončit blok ([příkaz try).](statements.md#the-try-statement) `finally` `break` V případě `finally` `finally` `break` ,žedojdekpříkazuvrámcibloku,musíbýtcílpříkazuvrámcistejnéhobloku;vopačném`break` případě dojde k chybě při kompilaci.

`break` Příkaz se spustí takto:

*  `finally` `try` `finally` Pokud příkaz ukončí jeden nebo více `try` bloků s přidruženými bloky, je ovládací prvek zpočátku převeden do bloku nejvnitřnějšího příkazu. `break` Když a pokud ovládací prvek dosáhne koncového bodu `finally` bloku, je ovládací prvek převeden `finally` do bloku dalšího ohraničujícího `try` příkazu. Tento proces se opakuje, `finally` dokud nebudou provedeny bloky všech `try` předaných příkazů.
*  Řízení je převedeno na cíl `break` příkazu.

Vzhledem k `break` tomu, že příkaz nepodmíněně přenáší řízení jinde, koncový bod `break` příkazu není nikdy dosažitelný.

### <a name="the-continue-statement"></a>Příkaz Continue

`do` `while` `foreach` `for`Příkaz spustí novou iteraci nejbližšího ohraničujícího příkazu,, nebo. `continue`

```antlr
continue_statement
    : 'continue' ';'
    ;
```

Cíl `continue` příkazu je koncový bod vloženého příkazu nejbližšího `while`ohraničujícího `do`příkazu,, `for`nebo `foreach` . `while` `do` `for`Pokud příkaz není uzavřen v příkazu,, nebo `foreach` , dojde k chybě při kompilaci. `continue`

Je- `while` `do` `for` `foreach` li více příkazů,, nebo vnořen mezi sebou, příkaz se vztahuje pouze na nejvnitřnější výraz. `continue` Chcete-li přenést řízení napříč více úrovněmi vnoření `goto` , je nutné použít příkaz ([příkaz goto](statements.md#the-goto-statement)).

Příkaz nemůže ukončit blok ([příkaz try).](statements.md#the-try-statement) `finally` `continue` V případě `finally` `finally` `continue` ,žedojdekpříkazuvrámcibloku,musíbýtcílpříkazuvrámcistejnéhobloku;vopačném`continue` případě dojde k chybě při kompilaci.

`continue` Příkaz se spustí takto:

*  `finally` `try` `finally` Pokud příkaz ukončí jeden nebo více `try` bloků s přidruženými bloky, je ovládací prvek zpočátku převeden do bloku nejvnitřnějšího příkazu. `continue` Když a pokud ovládací prvek dosáhne koncového bodu `finally` bloku, je ovládací prvek převeden `finally` do bloku dalšího ohraničujícího `try` příkazu. Tento proces se opakuje, `finally` dokud nebudou provedeny bloky všech `try` předaných příkazů.
*  Řízení je převedeno na cíl `continue` příkazu.

Vzhledem k `continue` tomu, že příkaz nepodmíněně přenáší řízení jinde, koncový bod `continue` příkazu není nikdy dosažitelný.

### <a name="the-goto-statement"></a>Příkaz goto

`goto` Příkaz přenáší řízení na příkaz, který je označen popiskem.

```antlr
goto_statement
    : 'goto' identifier ';'
    | 'goto' 'case' constant_expression ';'
    | 'goto' 'default' ';'
    ;
```

Cíl `goto` příkazu *identifikátoru* je popisek příkazu se zadaným popiskem. Pokud popisek s daným názvem neexistuje v aktuálním členovi funkce nebo pokud `goto` příkaz není v rámci oboru popisku, dojde k chybě při kompilaci. Toto pravidlo povoluje použití `goto` příkazu pro přenos řízení z vnořeného oboru, ale ne do vnořeného oboru. V příkladu
```csharp
using System;

class Test
{
    static void Main(string[] args) {
        string[,] table = {
            {"Red", "Blue", "Green"},
            {"Monday", "Wednesday", "Friday"}
        };

        foreach (string str in args) {
            int row, colm;
            for (row = 0; row <= 1; ++row)
                for (colm = 0; colm <= 2; ++colm)
                    if (str == table[row,colm])
                         goto done;

            Console.WriteLine("{0} not found", str);
            continue;
    done:
            Console.WriteLine("Found {0} at [{1}][{2}]", str, row, colm);
        }
    }
}
```
`goto` příkaz se používá k přenosu řízení z vnořeného oboru.

Cíl `goto case` příkazu je seznam příkazů v `switch` příkazu bezprostředně ohraničujícího příkaz ( `case` [příkaz switch](statements.md#the-switch-statement)), který obsahuje popisek s danou konstantní hodnotou. `switch` `switch` [](conversions.md#implicit-conversions)Pokud příkaz není uzavřený příkazem, pokud constant_expression není implicitně konvertibilní (implicitní převody) na typ řízení nejbližšího ohraničujícího příkazu, nebo pokud `goto case` nejbližší ohraničující `switch` příkaz `case` neobsahuje popisek s danou konstantní hodnotou, dojde k chybě při kompilaci.

Cíl `goto default` příkazu je seznam příkazů v `switch` příkazu bezprostředně ohraničujícího příkaz ( `default` [příkaz switch](statements.md#the-switch-statement)), který obsahuje popisek. Pokud příkaz není ohraničen `switch` příkazem, nebo `switch` Pokud nejbližší nadřazený příkaz `default` neobsahuje popisek, dojde k chybě při kompilaci. `goto default`

Příkaz nemůže ukončit blok ([příkaz try).](statements.md#the-try-statement) `finally` `goto` V případě `finally` `finally` `goto` ,žedojdekpříkazuvrámcibloku,cílpříkazumusíbýtvrámcistejnéhoblokunebovopačném`goto` případě dojde k chybě při kompilaci.

`goto` Příkaz se spustí takto:

*  `finally` `try` `finally` Pokud příkaz ukončí jeden nebo více `try` bloků s přidruženými bloky, je ovládací prvek zpočátku převeden do bloku nejvnitřnějšího příkazu. `goto` Když a pokud ovládací prvek dosáhne koncového bodu `finally` bloku, je ovládací prvek převeden `finally` do bloku dalšího ohraničujícího `try` příkazu. Tento proces se opakuje, `finally` dokud nebudou provedeny bloky všech `try` předaných příkazů.
*  Řízení je převedeno na cíl `goto` příkazu.

Vzhledem k `goto` tomu, že příkaz nepodmíněně přenáší řízení jinde, koncový bod `goto` příkazu není nikdy dosažitelný.

### <a name="the-return-statement"></a>Příkaz return

Příkaz vrátí řízení aktuálnímu volajícímu funkce, ve `return` které se příkaz zobrazí. `return`

```antlr
return_statement
    : 'return' expression? ';'
    ;
```

`add` `void` `set` [](classes.md#method-body)Příkaz bez výrazu lze použít pouze ve členovi funkce, který nepočítá hodnotu, tedy metodu s typem výsledku (tělo metody), přistupující objekt vlastnosti nebo indexeru, `return` a `remove` přistupující objekty události, konstruktor instance, statický konstruktor nebo destruktor.

Příkaz s výrazem lze použít pouze v členovi funkce, který vypočítá hodnotu, což je metoda s neprázdným typem výsledku `get` , přístupový objekt vlastnosti nebo indexeru nebo uživatelem definovaný operátor. `return` Implicitní převod ([implicitní převody](conversions.md#implicit-conversions)) musí existovat z typu výrazu na návratový typ obsahujícího člena funkce.

Příkazy Return lze také použít v těle anonymních výrazů funkce ([výrazy anonymních funkcí](expressions.md#anonymous-function-expressions)) a účastnit se určení, které převody pro tyto funkce existují.

Jedná se o chybu `return` `finally` v době kompilace pro zobrazení příkazu v bloku ([příkaz try](statements.md#the-try-statement)).

`return` Příkaz se spustí takto:

*  `return` Pokud příkaz určuje výraz, je vyhodnocen výraz a výsledná hodnota je převedena na návratový typ obsahující funkce implicitním převodem. Výsledek převodu se stala výslednou hodnotou vytvořenou funkcí.
*  `finally` `catch` `try` `finally` Je-li `try` příkaz uzavřen jedním nebo více nebo bloku s přidruženými bloky, je ovládací prvek zpočátku převeden do bloku nejvnitřnějšího příkazu. `return` Když a pokud ovládací prvek dosáhne koncového bodu `finally` bloku, je ovládací prvek převeden `finally` do bloku dalšího ohraničujícího `try` příkazu. Tento proces se opakuje, `finally` dokud nebudou provedeny bloky všech `try` ohraničujících příkazů.
*  Pokud je nadřazená funkce neasynchronní funkcí, ovládací prvek je vrácen volajícímu obsahující funkci spolu s výslednou hodnotou, pokud existuje.
*  Pokud je nadřazená funkce asynchronní funkce, vrátí se řízení aktuálnímu volajícímu a výsledná hodnota, pokud existuje, je zaznamenána v úloze vrácení, jak je popsáno v tématu ([rozhraní enumerátorů](classes.md#enumerator-interfaces)).

Vzhledem k `return` tomu, že příkaz nepodmíněně přenáší řízení jinde, koncový bod `return` příkazu není nikdy dosažitelný.

### <a name="the-throw-statement"></a>Příkaz throw

`throw` Příkaz vyvolá výjimku.

```antlr
throw_statement
    : 'throw' expression? ';'
    ;
```

`throw` Příkaz s výrazem vyvolá hodnotu vytvořenou vyhodnocením výrazu. Výraz musí poznamenat hodnotu typu `System.Exception`třídy typu třídy, který je odvozen z `System.Exception` nebo typu parametru typu, který má `System.Exception` (nebo podtřídu) jako platnou základní třídu. Pokud vygeneruje `null`vyhodnocení výrazu `System.NullReferenceException` , je vyvolána místo toho.

Příkaz bez výrazu lze použít pouze `catch` v bloku, v takovém případě příkaz znovu vyvolá výjimku, která je `catch` aktuálně zpracovávána tímto blokem. `throw`

Vzhledem k `throw` tomu, že příkaz nepodmíněně přenáší řízení jinde, koncový bod `throw` příkazu není nikdy dosažitelný.

Je-li vyvolána výjimka, je ovládací prvek převeden do první `catch` klauzule v `try` ohraničujícím příkazu, který může zpracovat výjimku. Proces, který probíhá z bodu výjimky vyvolané v bodě přenosu řízení na vhodnou obslužnou rutinu výjimky, je označován jako ***šíření výjimky***. Šíření výjimky se skládá z opakovaného vyhodnocení následujících kroků, dokud `catch` není nalezena klauzule, která se shoduje s výjimkou. V tomto popisu je ***bod throw*** zpočátku umístěním, kde je vyvolána výjimka.

*  V aktuálním členovi funkce je zkontrolován `try` každý příkaz, který je ohraničený bodem throw. Pro každý příkaz `S`, který začíná nejvnitřnějším `try` příkazem a končí vnějším `try` příkazem, jsou vyhodnoceny následující kroky:

   * `try` Pokud `catch` `catch` blok uzavíracího bodu uzavřete a pokud má parametr S jednu nebo více klauzulí, jsou klauzule zkontrolovány v pořadí podle vzhledu pro vyhledání vhodné obslužné rutiny pro výjimku podle pravidel uvedených v `S` [V části příkaz try](statements.md#the-try-statement). Pokud je nalezena `catch` odpovídající klauzule, šíření výjimky je dokončeno přenesením ovládacího prvku na blok `catch` této klauzule.

   * V opačném případě `try` , pokud blok `catch` nebo blok `S` uzavíracího bodu a `S` má `finally` blok, je ovládací prvek převeden do `finally` bloku. `finally` Pokud blok vyvolá jinou výjimku, zpracování aktuální výjimky je ukončeno. V opačném případě, pokud ovládací prvek dosáhne koncového bodu `finally` bloku, pokračuje zpracování aktuální výjimky.

*  Pokud nebyla obslužná rutina výjimky umístěna v aktuálním volání funkce, volání funkce je ukončeno a nastane jedna z následujících situací:

   * Pokud je aktuální funkce neasynchronní, výše uvedené kroky jsou opakovány pro volající funkce s bodem throw, který odpovídá příkazu, ze kterého byl člen funkce vyvolán.

   * Pokud je aktuální funkce asynchronní a vrácení úlohy zpět, je výjimka zaznamenána do návratové úlohy, která je vložena do chybného nebo zrušeného stavu, jak je popsáno v tématu [rozhraní výčtu](classes.md#enumerator-interfaces).

   * Pokud je aktuální funkce asynchronní a návratovou hodnotou void, je kontext synchronizace aktuálního vlákna upozorněn, jak je popsáno v tématu [výčtová rozhraní](classes.md#enumerable-interfaces).

*  Pokud zpracování výjimek ukončí všechna volání členů funkce v aktuálním vlákně, což značí, že vlákno neobsahuje žádnou obslužnou rutinu pro výjimku, vlákno je ukončeno. Dopad takového ukončení je definován implementací.

## <a name="the-try-statement"></a>Příkaz try

`try` Příkaz poskytuje mechanismus pro zachycení výjimek, ke kterým dochází během provádění bloku. Kromě toho `try` příkaz poskytuje možnost zadat blok kódu, který je vždy spuštěn, když ovládací prvek `try` opustí příkaz.

```antlr
try_statement
    : 'try' block catch_clause+
    | 'try' block finally_clause
    | 'try' block catch_clause+ finally_clause
    ;

catch_clause
    : 'catch' exception_specifier? exception_filter?  block
    ;

exception_specifier
    : '(' type identifier? ')'
    ;

exception_filter
    : 'when' '(' expression ')'
    ;

finally_clause
    : 'finally' block
    ;
```

Existují tři možné formy `try` příkazů:

*  Blok následovaný jedním nebo více `catch` bloky. `try`
*  `try` Blok následovaný`finally` blokem.
*  Blok následovaný jedním nebo více `catch` bloky následovanými `finally` blokem. `try`

`System.Exception` `System.Exception` `System.Exception`Když klauzule určuje exception_specifier, musí být typ, typ, který je odvozen z nebo typ parametru typu, který má (nebo podtříd) jako jeho efektivní základní třídu. `catch`

Když klauzule určuje jak exception_specifier s *identifikátorem*, je deklarována ***Proměnná výjimky*** daného názvu a typu. `catch` Proměnná výjimky odpovídá místní proměnné s rozsahem, který překračuje `catch` klauzuli. Během provádění *exception_filter* a *bloku*představuje proměnná výjimky Aktuálně zpracovávanou výjimku. Pro účely jednoznačné kontroly přiřazení je proměnná výjimky považována za jednoznačně přiřazenou v celém oboru.

Pokud klauzule nezahrnuje název proměnné výjimky, není nemožné získat přístup k objektu výjimky ve filtru a `catch` bloku. `catch`

Klauzule, která neurčuje *exception_specifier* , se nazývá obecná `catch` klauzule. `catch`

Některé programovací jazyky mohou podporovat výjimky, které nejsou reprezentovány jako objekt odvozený z `System.Exception`, i když takové výjimky by nikdy neměly být C# generovány kódem. K zachycení `catch` takových výjimek se dá použít obecná klauzule. Proto obecná `catch` klauzule je sémanticky odlišná od jednoho, který určuje typ `System.Exception`, v tom, že předchozí může také zachytit výjimky z jiných jazyků.

Aby bylo možné najít obslužnou rutinu pro výjimku, `catch` jsou v lexikálním pořadí přezkoumány klauzule. Pokud klauzule určuje typ, ale filtr výjimek, jedná se o chybu při kompilaci pro pozdější `catch` klauzuli v rámci stejného `try` příkazu k určení typu, který je stejný jako nebo je odvozen z typu, který je typu. `catch` Pokud klauzule neurčuje žádný typ bez filtru, musí se jednat o poslední `catch` klauzuli pro tento `try` příkaz. `catch`

V rámci `throw` `catch` bloku lze příkaz ([příkaz throw](statements.md#the-throw-statement)) bez výrazu použít k opětovnému vyvolání výjimky, která byla zachycena blokem. `catch` Přiřazení proměnné výjimky nemění výjimku, která je znovu vyvolána.

V příkladu
```csharp
using System;

class Test
{
    static void F() {
        try {
            G();
        }
        catch (Exception e) {
            Console.WriteLine("Exception in F: " + e.Message);
            e = new Exception("F");
            throw;                // re-throw
        }
    }

    static void G() {
        throw new Exception("G");
    }

    static void Main() {
        try {
            F();
        }
        catch (Exception e) {
            Console.WriteLine("Exception in Main: " + e.Message);
        }
    }
}
```
Metoda `F` zachytí výjimku, zapisuje do konzoly nějaké diagnostické informace, změní proměnnou výjimky a znovu vyvolá výjimku. Výjimka, která je znovu vyvolána, je původní výjimka, takže výstup vytvářený je:
```
Exception in F: G
Exception in Main: G
```

Pokud první blok catch vyvolal `e` místo opětovného vyvolání aktuální výjimky, vyprodukovaný výstup by byl následující:
```
Exception in F: G
Exception in Main: F
```

Jedná se o `break`chybu při kompilaci příkazu, `continue`nebo `goto` pro přenos řízení `finally` z bloku. Když v `break` `finally` bloku `continue`dojde k `goto` příkazu, nebo, musí být cíl příkazu v rámci stejného `finally` bloku nebo jinak dojde k chybě při kompilaci.

Jedná se o chybu při kompilaci, ke které `return` může příkaz `finally` v bloku dojít.

`try` Příkaz se spustí takto:

*  Ovládací prvek se přenese `try` do bloku.
*  Když a když ovládací prvek dosáhne koncového bodu `try` bloku:
   *  Pokud příkaz má blok, je spuštěn `finally` blok. `finally` `try`
   *  Ovládací prvek se přenese do koncového bodu `try` příkazu.

*  Pokud je výjimka rozšířena na `try` příkaz během provádění `try` bloku:
   *  `catch` Klauzule, pokud existují, jsou zkontrolovány v pořadí podle vzhledu pro vyhledání vhodné obslužné rutiny pro výjimku. `catch` Pokud klauzule neurčuje typ, nebo typ výjimky nebo základní typ typu výjimky:
      *  `catch` Pokud klauzule deklaruje proměnnou výjimky, je objekt výjimky přiřazen proměnné výjimky.
      *  `catch` Pokud klauzule deklaruje filtr výjimek, je vyhodnocen filtr. Pokud se vyhodnotí `false`jako, klauzule catch není shodná a hledání pokračuje prostřednictvím jakékoli další `catch` klauzule pro vhodnou obslužnou rutinu.
      *  V opačném případě je `catch` klauzulepovažovánazashoduaovládacíprveksepřenesedoodpovídajícíhobloku.`catch`
      *  Když a když ovládací prvek dosáhne koncového bodu `catch` bloku:
         * Pokud příkaz má blok, je spuštěn `finally` blok. `finally` `try`
         * Ovládací prvek se přenese do koncového bodu `try` příkazu.
      *  Pokud je výjimka rozšířena na `try` příkaz během provádění `catch` bloku:
         *  Pokud příkaz má blok, je spuštěn `finally` blok. `finally` `try`
         *  Výjimka je šířena do dalšího ohraničujícího `try` příkazu.
   *  Pokud příkaz nemá žádné `catch` klauzule nebo pokud žádná `catch` klauzule neodpovídá výjimce: `try`
      *  Pokud příkaz má blok, je spuštěn `finally` blok. `finally` `try`
      *  Výjimka je šířena do dalšího ohraničujícího `try` příkazu.

Příkazy `finally` bloku jsou vždy spouštěny, pokud ovládací prvek `try` opustí příkaz. To platí, zda je přenos ovládacího prvku proveden jako výsledek normálního spuštění, v `break`důsledku provedení příkazu, `continue`, `goto`nebo `return` nebo v důsledku rozšiřování výjimky z `try` vydá.

Pokud je vyvolána výjimka během provádění `finally` bloku a není zachycena v rámci stejného bloku finally, je výjimka rozšířena do dalšího `try` ohraničujícího příkazu. Pokud byla v procesu rozšiřování další výjimka, dojde ke ztrátě této výjimky. Proces šíření výjimky je popsán dále v popisu `throw` příkazu ([příkaz throw](statements.md#the-throw-statement)).

Blok příkazu je dosažitelný, pokud je příkazdosažitelný.`try` `try` `try`

Blok příkazu je dosažitelný, pokud je příkazdosažitelný.`try` `try` `catch`

Blok příkazu je dosažitelný, pokud je příkazdosažitelný.`try` `try` `finally`

Koncový bod `try` příkazu je dosažitelný, pokud jsou splněny obě následující podmínky:

*  Koncový bod `try` bloku je dosažitelný nebo je dostupný koncový bod alespoň jednoho `catch` bloku.
*  Pokud je `finally` k dispozici blok, koncový bod `finally` bloku je dosažitelný.

## <a name="the-checked-and-unchecked-statements"></a>Kontrolované a nezaškrtnuté příkazy

Příkazy `checked` a`unchecked` slouží k řízení ***kontextu kontroly přetečení*** pro aritmetické operace a převody integrálního typu.

```antlr
checked_statement
    : 'checked' block
    ;

unchecked_statement
    : 'unchecked' block
    ;
```

Příkaz způsobí vyhodnocení všech výrazů v *bloku* v kontrolovaném kontextu a příkaz způsobí, že `unchecked` všechny výrazy v bloku budou vyhodnocovány v nekontrolovaném kontextu. `checked`

`unchecked` Příkazy `checked` a `unchecked` jsoupřesnéekvivalentemoperátorůa(zkontrolovanýchanekontrolovanýchoperátorů),stímrozdílem`checked` , že pracují na blocích namísto výrazů.[](expressions.md#the-checked-and-unchecked-operators)

## <a name="the-lock-statement"></a>Příkaz Lock

`lock` Příkaz získá zámek vzájemného vyloučení pro daný objekt, provede příkaz a pak uvolní zámek.

```antlr
lock_statement
    : 'lock' '(' expression ')' embedded_statement
    ;
```

Výraz `lock` příkazu musí znamenat hodnotu typu, který je známý jako *reference_type*. Pro výraz `lock` příkazu není nikdy proveden žádný implicitní převod na zabalení ([převody zabalení](conversions.md#boxing-conversions)), a proto se jedná o chybu při kompilaci, která označuje hodnotu *value_type*.

`lock` Příkaz formuláře
```csharp
lock (x) ...
```
kde `x` je výrazem *reference_type*, je přesně ekvivalentní
```csharp
bool __lockWasTaken = false;
try {
    System.Threading.Monitor.Enter(x, ref __lockWasTaken);
    ...
}
finally {
    if (__lockWasTaken) System.Threading.Monitor.Exit(x);
}
```
s výjimkou, že `x` je vyhodnocena pouze jednou.

I když je držen zámek vzájemného vyloučení, může kód spuštěný ve stejném vláknu spuštění také získat a uvolnit zámek. Nicméně kód, který je spuštěn v jiných vláknech, je blokován pro získání zámku, dokud se zámek neuvolní.

Uzamčení `System.Type` objektů za účelem synchronizace přístupu ke statickým datům se nedoporučuje. Jiný kód může být uzamčen na stejném typu, což může vést k zablokování. Lepším řešením je synchronizovat přístup ke statickým datům uzamknutím privátního statického objektu. Příklad:
```csharp
class Cache
{
    private static readonly object synchronizationObject = new object();

    public static void Add(object x) {
        lock (Cache.synchronizationObject) {
            ...
        }
    }

    public static void Remove(object x) {
        lock (Cache.synchronizationObject) {
            ...
        }
    }
}
```

## <a name="the-using-statement"></a>Pomocí příkazu using

`using` Příkaz získá jeden nebo více prostředků, provede příkaz a pak odstraní prostředek.

```antlr
using_statement
    : 'using' '(' resource_acquisition ')' embedded_statement
    ;

resource_acquisition
    : local_variable_declaration
    | expression
    ;
```

***Prostředek*** je třída nebo struktura, která implementuje `System.IDisposable`rozhraní, které zahrnuje jednu metodu bez parametrů s názvem. `Dispose` Kód, který používá prostředek, může zavolat `Dispose` , aby označoval, že prostředek již není potřeba. Pokud `Dispose` není voláno, pak automatické vyřazení nakonec probíhá jako důsledek uvolňování paměti.

Pokud je forma *resource_acquisition* *local_variable_declaration* , pak typ *local_variable_declaration* musí být buď `dynamic` nebo typ, který lze implicitně převést na. `System.IDisposable` Pokud je tvar výrazu *resource_acquisition* , pak tento výraz musí být implicitně převoditelné `System.IDisposable`na.

Místní proměnné deklarované v *resource_acquisition* jsou jen pro čtení a musí zahrnovat inicializátor. K chybě při kompilaci dojde v případě, že se vložený příkaz pokusí změnit tyto místní proměnné (prostřednictvím přiřazení nebo `++` operátorů a `--` ), přebírat jejich adresu nebo předat `ref` parametry nebo `out` .

`using` Příkaz je přeložen na tři části: získání, využití a vyřazení. Využití prostředku je implicitně uzavřeno v `try` příkazu, který `finally` obsahuje klauzuli. Tato `finally` klauzule odstraní prostředek. Pokud je získán `Dispose` prostředek,neníprovedenožádnévoláníanení`null` vyvolána žádná výjimka. Pokud je prostředek typu `dynamic` , je dynamicky převeden prostřednictvím implicitního dynamického převodu ([implicitní dynamické převody](conversions.md#implicit-dynamic-conversions)) na `IDisposable` během akvizice, aby bylo zajištěno, že převod proběhne úspěšně před využitím a odvod.

`using` Příkaz formuláře
```csharp
using (ResourceType resource = expression) statement
```
odpovídá jedné ze tří možných rozšíření. Pokud `ResourceType` je typ hodnoty, která není null, je rozšíření
```csharp
{
    ResourceType resource = expression;
    try {
        statement;
    }
    finally {
        ((IDisposable)resource).Dispose();
    }
}
```

V opačném `ResourceType` případě, kdy je typ hodnoty s možnou hodnotou null `dynamic`nebo typ odkazu jiný než je rozšíření
```csharp
{
    ResourceType resource = expression;
    try {
        statement;
    }
    finally {
        if (resource != null) ((IDisposable)resource).Dispose();
    }
}
```

`ResourceType` V `dynamic`opačném případě je rozšíření
```csharp
{
    ResourceType resource = expression;
    IDisposable d = (IDisposable)resource;
    try {
        statement;
    }
    finally {
        if (d != null) d.Dispose();
    }
}
```

V obou rozšířeních `resource` je proměnná v příkazu Embedded jen pro čtení `d` a proměnná je nepřístupná v a neviditelná pro vložený příkaz.

Implementace má povolenou implementaci daného příkazu Using, například z důvodů výkonu, pokud je chování konzistentní s výše uvedeným rozšířením.

`using` Příkaz formuláře
```csharp
using (expression) statement
```
má stejné tři možné rozšíření. V tomto případě `ResourceType` je implicitně typ doby kompilace `expression`, pokud má,, pokud má nějaký typ. V opačném `IDisposable` případě se samotné rozhraní používá `ResourceType`jako. Tato `resource` proměnná je nepřístupná v a v rámci vloženého příkazu není viditelná.

Pokud má *resource_acquisition* formu *local_variable_declaration*, je možné získat více prostředků daného typu. `using` Příkaz formuláře
```csharp
using (ResourceType r1 = e1, r2 = e2, ..., rN = eN) statement
```
je přesně ekvivalentní sekvenci vnořených `using` příkazů:
```csharp
using (ResourceType r1 = e1)
    using (ResourceType r2 = e2)
        ...
            using (ResourceType rN = eN)
                statement
```

Následující příklad vytvoří soubor s názvem `log.txt` a zapíše dva řádky textu do souboru. Příklad následně otevře stejný soubor pro čtení a zkopíruje obsažené řádky textu do konzoly.
```csharp
using System;
using System.IO;

class Test
{
    static void Main() {
        using (TextWriter w = File.CreateText("log.txt")) {
            w.WriteLine("This is line one");
            w.WriteLine("This is line two");
        }

        using (TextReader r = File.OpenText("log.txt")) {
            string s;
            while ((s = r.ReadLine()) != null) {
                Console.WriteLine(s);
            }

        }
    }
}
```

Vzhledem k tomu `TextReader` , že třídy `IDisposable` `using` a implementují rozhraní, může příklad použít příkazy k zajištění toho, aby byl podkladový soubor správně uzavřen po operacích zápisu nebo čtení. `TextWriter`

## <a name="the-yield-statement"></a>Příkaz yield

Příkaz se používá v bloku iterátoru ([bloky](statements.md#blocks)) k získání hodnoty objektu Enumerator ([objekty enumerátoru](classes.md#enumerator-objects)) nebo vyčíslitelného objektu ([vyčíslitelné objekty](classes.md#enumerable-objects)) iterátoru nebo k signalizaci konce iterace. `yield`

```antlr
yield_statement
    : 'yield' 'return' expression ';'
    | 'yield' 'break' ';'
    ;
```

`yield`není rezervované slovo; má zvláštní význam pouze v případě, že je použit `return` bezprostředně `break` před klíčovým slovem or. V jiných kontextech `yield` lze použít jako identifikátor.

Existuje několik omezení, kde `yield` se může zobrazit příkaz, jak je popsáno v následujícím tématu.

*  Jedná se o chybu `yield` při kompilaci pro příkaz (z obou formulářů), který se zobrazí mimo *method_body*, *operator_body* nebo *accessor_body* .
*  Jedná se o chybu při kompilaci pro `yield` příkaz (z obou formulářů), který se má objevit uvnitř anonymní funkce.
*  Jedná se o chybu při kompilaci pro `yield` příkaz (z obou formulářů), který se má objevit `finally` v klauzuli `try` příkazu.
*  Jedná se o chybu `yield return` při kompilaci, aby se příkaz objevil kdekoli `try` v příkazu, který obsahuje jakékoli `catch` klauzule.

Následující příklad ukazuje několik platných a neplatných použití `yield` příkazů.

```csharp
delegate IEnumerable<int> D();

IEnumerator<int> GetEnumerator() {
    try {
        yield return 1;        // Ok
        yield break;           // Ok
    }
    finally {
        yield return 2;        // Error, yield in finally
        yield break;           // Error, yield in finally
    }

    try {
        yield return 3;        // Error, yield return in try...catch
        yield break;           // Ok
    }
    catch {
        yield return 4;        // Error, yield return in try...catch
        yield break;           // Ok
    }

    D d = delegate { 
        yield return 5;        // Error, yield in an anonymous function
    }; 
}

int MyMethod() {
    yield return 1;            // Error, wrong return type for an iterator block
}
```

Implicitní převod ([implicitní převody](conversions.md#implicit-conversions)) musí existovat z typu výrazu v `yield return` příkazu na typ Yield ([typ výtěžnosti](classes.md#yield-type)) iterátoru.

`yield return` Příkaz se spustí takto:

*  Výraz uvedený v příkazu je vyhodnocen, implicitně převeden na typ yield a přiřazený `Current` vlastnosti objektu enumerátor.
*  Provádění bloku iterátoru je pozastaveno. Pokud je `try` `finally` příkaz v jednom nebo více blocích, přidružené bloky nejsou v tuto chvíli spuštěny. `yield return`
*  Metoda objektu Enumerator se vrátí `true` svému volajícímu, což značí, že objekt enumerátoru se úspěšně pokročil na další položku. `MoveNext`

Další volání `MoveNext` metody objektu Enumerator pokračuje v provádění bloku iterátoru, ze kterého byl naposled pozastaven.

`yield break` Příkaz se spustí takto:

*  `try` `finally` `finally` `try` Pokud je příkazuzavřenjednímnebovíceblokyspřidruženýmibloky,jeovládacíprvekzpočátkupřevedendoblokunejvnitřnějšíhopříkazu.`yield break` Když a pokud ovládací prvek dosáhne koncového bodu `finally` bloku, je ovládací prvek převeden `finally` do bloku dalšího ohraničujícího `try` příkazu. Tento proces se opakuje, `finally` dokud nebudou provedeny bloky všech `try` ohraničujících příkazů.
*  Ovládací prvek se vrátí volajícímu bloku iterátoru. Toto je buď `MoveNext` metoda, nebo `Dispose` metoda objektu Enumerator.

Vzhledem k `yield break` tomu, že příkaz nepodmíněně přenáší řízení jinde, koncový bod `yield break` příkazu není nikdy dosažitelný.
