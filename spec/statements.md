---
ms.openlocfilehash: 8f9551b9e7f70379836c23a60f0d37dc02f8e18e
ms.sourcegitcommit: 3fc033b6e98ed7ecdf46a85c79b00a3a3ddcf963
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/18/2019
ms.locfileid: "47229949"
---
# <a name="statements"></a>Příkazy

C# poskytuje celou paletu příkazů. Většina z těchto příkazů bude zkušenosti vývojáře, kteří mají programovat v jazycích C a C++.

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

*Embedded_statement* neterminálu se používá pro příkazy, které se zobrazují v rámci jiných příkazů. Použití *embedded_statement* spíše než *příkaz* vyloučí použití příkazy deklarace a příkazy s popiskem v těchto kontextech. V příkladu
```csharp
void F(bool b) {
    if (b)
        int i = 44;
}
```
výsledkem chyba kompilace, protože `if` příkazu vyžaduje *embedded_statement* spíše než *příkaz* pro jeho Pokud větev. Pokud tento kód byly převody povoleny, pak proměnná `i` by být deklarovány, ale může nikdy nepoužívá. Mějte na paměti, ale, tak, že `i`na deklaraci v bloku, v příkladu je platný.

## <a name="end-points-and-reachability"></a>Koncové body a dostupnosti

Každý příkaz má ***koncový bod***. Intuitivní řečeno koncový bod příkazu je umístění, které bezprostředně následuje po příkazu. Spuštění pravidla pro složené příkazy (příkazy, které obsahují vložené příkazy) určují akce, která se provede, když ovládací prvek dosáhne koncového bodu vloženým příkazem. Například když ovládací prvek dosáhne příkazu v bloku koncový bod, je kontrola předána dalšímu příkazu v bloku.

Pokud příkaz může být dosažitelný z provádění, příkaz se říká, že ***dostupný***. Naopak, pokud není možné, že se spustí příkaz, příkaz se říká, že ***nedostupný***.

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
druhé volání `Console.WriteLine` nedosažitelný, proto není možné, že se spustí příkaz.

Upozornění bude nahlášena, pokud kompilátor zjistí, že příkaz nedosažitelný. Je speciálně není chyba pro příkaz nedostupná.

Pokud chcete zjistit, jestli je konkrétní příkaz nebo koncový bod dostupný, kompilátor provede analýzu toku podle dostupnosti pravidel definovaných příkazu for each. Analýzy toku bere v úvahu hodnoty konstantních výrazů ([konstantní výrazy](expressions.md#constant-expressions)), které řídí chování příkazy, ale nejsou považovány za možných hodnot, která není konstantní výrazy. Jinými slovy pro účely analýzy toku řízení nekonstantní výraz daného typu se považuje za všechny možné hodnotu daného typu.

V příkladu
```csharp
void F() {
    const int i = 1;
    if (i == 2) Console.WriteLine("unreachable");
}
```
logický výraz `if` příkazu je konstantní výraz, protože oba operandy `==` operátor jsou konstanty. Jako konstantní výraz je vyhodnocen v době kompilace, vytváření hodnota `false`, `Console.WriteLine` volání se považuje za nedostupný. Nicméně pokud `i` se změní, aby byla místní proměnné
```csharp
void F() {
    int i = 1;
    if (i == 2) Console.WriteLine("reachable");
}
```
`Console.WriteLine` volání se považuje za dostupný, i když ve skutečnosti, bude nikdy spuštěn.

*Bloku* funkce člena je vždy považován za dostupný. Postupně po vyhodnocení pravidla dostupnosti každého příkazu v bloku, se dá určit dostupnosti jakékoli daný příkaz.

V příkladu
```csharp
void F(int x) {
    Console.WriteLine("start");
    if (x < 0) Console.WriteLine("negative");
}
```
připojení z druhé `Console.WriteLine` je stanoven následujícím způsobem:

*  První `Console.WriteLine` příkaz výrazu je dostupný protože bloku `F` metoda je dostupný.
*  Koncový bod první `Console.WriteLine` příkaz výrazu je dostupný, protože tento příkaz je dostupný.
*  `if` Příkaz je dostupný, protože koncový bod prvního `Console.WriteLine` příkaz výrazu je dostupný.
*  Druhá `Console.WriteLine` příkaz výrazu je dostupný protože logický výraz `if` příkaz nemá konstantní hodnotu `false`.

Existují dvě situace, ve kterých je chyba kompilace pro koncový bod příkazu být dostupná pro:

*  Vzhledem k tomu, `switch` příkaz neumožňuje části přepínače, aby "" k další části přepínače, je chyba kompilace pro koncový bod seznamu příkazů oddíl přepínače být dostupná. Pokud k této chybě dochází, je obvykle jako ukazatel toho, který `break` příkaz nebyl nalezen.
*  Je chyba kompilace pro koncový bod bloku funkce člena, který vypočítá hodnotu být dostupná. Pokud k této chybě dochází, obvykle je údaj, který `return` příkaz nebyl nalezen.

## <a name="blocks"></a>Bloky

A *bloku* povoluje více příkazů, které má být zapsán v kontextech, kde je povoleno jeden příkaz.

```antlr
block
    : '{' statement_list? '}'
    ;
```

A *bloku* se skládá z volitelné *statement_list* ([příkaz uvádí](statements.md#statement-lists)), uzavřené ve složených závorkách. Pokud je vynechán seznam příkazů, se říká, že blok prázdný.

Blok mohou obsahovat příkazy deklarace ([příkazy deklarace](statements.md#declaration-statements)). Obor lokální proměnné nebo konstantní deklarované v bloku je blok.

Blok provádí následujícím způsobem:

*  Pokud blok je prázdný, ovládací prvek bude převeden na koncový bod bloku.
*  Pokud blok není prázdná, ovládací prvek bude převeden do seznamu příkazů. Když a ovládací prvek dosáhne koncového bodu seznam příkazů, ovládací prvek bude převeden na koncový bod bloku.

Seznam příkazů bloku je dostupná, pokud je dostupný samotného bloku.

Koncový bod bloku je dostupná, pokud blok je prázdný nebo pokud je dostupný koncový bod seznamu příkazů.

A *bloku* , který obsahuje jeden nebo více `yield` příkazy ([příkaz yield](statements.md#the-yield-statement)) se nazývá blok iterátoru. Iterátor bloky se používají k implementaci funkce členy jako iterátory ([iterátory](classes.md#iterators)). Některé další omezení se vztahují na iterátor bloků:

*  Je chyba kompilace pro `return` příkazu se zobrazí v blok iterátoru (ale `yield return` příkazy jsou povolené).
*  Je chyba kompilace pro blok iterátoru tak, aby obsahovala kontextu unsafe ([nezabezpečený kontext](unsafe-code.md#unsafe-contexts)). Blok iterátoru vždy definuje bezpečné kontextu, i v případě, že jeho deklarace je vnořená v nezabezpečeném kontextu.

### <a name="statement-lists"></a>Seznamy – příkaz

A ***seznamu příkazů*** se skládá z jednoho nebo více příkazů, které jsou napsané v pořadí. Probíhá příkaz seznamy *bloku*s ([bloky](statements.md#blocks)) a v *switch_block*s ([příkazu switch](statements.md#the-switch-statement)).

```antlr
statement_list
    : statement+
    ;
```

Seznam příkazů provádí přenos řízení na první příkaz. Když a ovládací prvek dosáhne příkazu koncový bod, je kontrola předána dalšímu příkazu. Když a ovládací prvek dosáhne koncového bodu poslední příkaz, ovládací prvek bude převeden na koncový bod seznamu příkazů.

Příkaz v seznamu příkazů je dostupný, pokud platí alespoň jedna z následujících akcí:

*  Příkaz je prvním příkazem a samotný seznam příkaz je dostupný.
*  Koncový bod předchozí příkaz je dostupný.
*  Příkaz je příkaz s popiskem a popisek se odkazuje dostupné `goto` příkazu.

Pokud je dostupný koncový bod poslední příkaz v seznamu je dostupný koncový bod seznamu příkazů.

## <a name="the-empty-statement"></a>Prázdný příkaz

*Empty_statement* nemá žádný účinek.

```antlr
empty_statement
    : ';'
    ;
```

Prázdný příkaz se používá, když nejsou žádná operace mají provést v kontextu, ve kterém jsou vyžadována příkaz.

Provádění prázdný příkaz jednoduše přenese ovládací prvek na konec příkazu. Koncový bod prázdný příkaz je tedy pokud prázdný příkaz je dostupný.

Prázdný příkaz může být použit při zápisu `while` příkaz s hodnotou null text:
```csharp
bool ProcessMessage() {...}

void ProcessMessages() {
    while (ProcessMessage())
        ;
}
```

Navíc prázdný příkaz je možné deklarovat popisek těsně před uzavírací "`}`" bloku:
```csharp
void F() {
    ...
    if (done) goto exit;
    ...
    exit: ;
}
```

## <a name="labeled-statements"></a>Příkaz s popiskem

A *labeled_statement* povoluje příkazu k mít předponu popisek. Příkaz s popiskem jsou povolené v blocích, ale nejsou povolené jako vložené příkazy.

```antlr
labeled_statement
    : identifier ':' statement
    ;
```

Příkaz s popiskem deklaruje popisek s názvem dána *identifikátor*. Obor popisku je celý blok ve kterém je deklarována popisek, včetně všech vnořených bloků. Je chyba kompilace pro dva popisky se stejným názvem, aby mají překrývající se rozsahy.

Popisek můžete odkazovat z `goto` příkazy ([příkazu goto](statements.md#the-goto-statement)) v rámci oboru popisku. To znamená, že `goto` příkazů může přenést řízení v rámci bloků a z bloků, ale nikdy do bloků.

Popisky své vlastní prohlášení místa a nejsou v konfliktu s další identifikátory. V příkladu
```csharp
int F(int x) {
    if (x >= 0) goto x;
    x = -x;
    x: return x;
}
```
je platný a používá název `x` jako parametr a popisek.

Spuštění příkaz s popiskem přesně odpovídá provádění příkazu za příkazem popisek.

Kromě dostupnosti poskytuje běžný tok řízení, je dostupná, pokud popisek se odkazuje dostupné příkaz s popiskem `goto` příkazu. (Výjimka: Pokud `goto` příkazu se nachází uvnitř `try` , který obsahuje `finally` bloku a příkaz s popiskem je mimo `try`a koncový bod `finally` bloku nedostupný, pak příkaz s popiskem není dosažitelný z který `goto` příkazu.)

## <a name="declaration-statements"></a>Příkazy deklarace

A *declaration_statement* deklaruje místní proměnnou nebo konstantu. Příkazy deklarace jsou povolené v blocích, ale nejsou povolené jako vložené příkazy.

```antlr
declaration_statement
    : local_variable_declaration ';'
    | local_constant_declaration ';'
    ;
```

### <a name="local-variable-declarations"></a>Místní deklarace proměnné

A *local_variable_declaration* deklaruje jednoho nebo několika lokálními proměnnými.

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

*Local_variable_type* z *local_variable_declaration* přímo určuje typ proměnné zavedeným deklarací, nebo označuje s identifikátorem `var` , který typ by měl odvodit podle inicializátor. Typ následuje seznam *local_variable_declarator*s, z nichž každý představuje novou proměnnou. A *local_variable_declarator* se skládá ze *identifikátor* , název proměnné, může volitelně následovat "`=`" token a *local_variable_initializer* , která obsahuje počáteční hodnotu proměnné.

V rámci deklarace lokální proměnné, var identifikátor funguje jako kontextové klíčové slovo ([klíčová slova](lexical-structure.md#keywords)). Když *local_variable_type* je zadán jako `var` a žádný typ s názvem `var` je v oboru, je deklarace ***implicitně typované lokální proměnné deklarace***, jehož typ je odvodit z typu přidružené inicializačního výrazu. Implicitně typované lokální deklarace proměnných, které se vztahují následující omezení:

*  *Local_variable_declaration* nesmí obsahovat více *local_variable_declarator*s.
*  *Local_variable_declarator* musí obsahovat *local_variable_initializer*.
*  *Local_variable_initializer* musí být *výraz*.
*  Inicializátor *výraz* musí mít typ za kompilace.
*  Inicializátor *výraz* nemůže odkazovat na proměnné deklarované samotný

Následují příklady nesprávné implicitně typované lokální deklarace proměnných:

```csharp
var x;               // Error, no initializer to infer type from
var y = {1, 2, 3};   // Error, array initializer not permitted
var z = null;        // Error, null does not have a type
var u = x => x + 1;  // Error, anonymous functions do not have a type
var v = v++;         // Error, initializer cannot refer to variable itself
```

V pomocí výrazu se získá hodnota místní proměnné *simple_name* ([jednoduché názvy](expressions.md#simple-names)), a hodnotu místní proměnné je upravit pomocí *přiřazení* () [Operátory přiřazení](expressions.md#assignment-operators)). Lokální proměnná musí být jednoznačně přiřazena ([jednoznačného přiřazení](variables.md#definite-assignment)) v každém umístění, kde získat jeho hodnotu.

Lokální proměnná deklarovaná v rozsahu *local_variable_declaration* je blok ve kterém dochází k deklaraci. Jedná se o chybu k odkazování na místní proměnnou v textové pozici, která předchází *local_variable_declarator* lokální proměnné. V rámci oboru místní proměnná je chyba kompilace, chcete-li deklarovat jinou místní proměnné nebo konstantní se stejným názvem.

Je ekvivalentní více deklarací jedné proměnné se stejným typem deklarace lokální proměnné, která deklaruje několik proměnných. Kromě toho inicializátoru proměnné v deklaraci lokální proměnné přesně odpovídá přiřazovací příkaz, který je vložen hned za deklaraci.

V příkladu
```csharp
void F() {
    int x = 1, y, z = x * 2;
}
```
přesně odpovídá
```csharp
void F() {
    int x; x = 1;
    int y;
    int z; z = x * 2;
}
```

V implicitně typované lokální proměnné deklarace typ místní proměnné deklarované se používá stejný jako typ výrazu použitý k inicializaci proměnné. Příklad:
```csharp
var i = 5;
var s = "Hello";
var d = 1.0;
var numbers = new int[] {1, 2, 3};
var orders = new Dictionary<int,Order>();
```

Implicitně typovaná místní proměnné prohlášení výše jsou přesně odpovídá explicitně následující deklarace:
```csharp
int i = 5;
string s = "Hello";
double d = 1.0;
int[] numbers = new int[] {1, 2, 3};
Dictionary<int,Order> orders = new Dictionary<int,Order>();
```

### <a name="local-constant-declarations"></a>Místní deklarace konstanty

A *local_constant_declaration* deklaruje nejmíň jeden místní konstanty.

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

*Typ* z *local_constant_declaration* Určuje typ konstanty zavedeným deklarací. Typ následuje seznam *constant_declarator*s, z nichž každý představuje nové konstantou. A *constant_declarator* se skládá ze *identifikátor* této názvy – konstanta, za nímž následuje "`=`" token a po něm *constant_expression* ([ Výrazy konstant](expressions.md#constant-expressions)), který vrací hodnotu konstanty.

*Typ* a *constant_expression* místní deklarace konstanty musí řídit stejnými pravidly jako deklarace konstantní členské ([konstanty](classes.md#constants)).

Hodnota lokální konstanta je získanou v pomocí výrazu *simple_name* ([jednoduché názvy](expressions.md#simple-names)).

Obor lokální konstanta je blok ve kterém dochází k deklaraci. Jedná se o chybu k odkazování na lokální konstanta v textové pozici, která předchází jeho *constant_declarator*. V rámci oboru místní konstantu je chyba kompilace, chcete-li deklarovat jinou místní proměnné nebo konstantní se stejným názvem.

Místní deklarace konstanty, který deklaruje více konstant je ekvivalentní více deklarací jedné konstanty se stejným typem.

## <a name="expression-statements"></a>Příkazy výrazů

*Expression_statement* vyhodnotí zadaný výraz. Hodnota vypočítaná aplikací výrazu, pokud existuje, se zahodí.

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

Ne všechny výrazy nejsou povoleny jako příkazy. Ve výrazech konkrétní, jako například `x + y` a `x == 1` , který pouze výpočtu hodnoty (což se zahodí), nejsou povolené jako příkazy.

Spuštění *expression_statement* vyhodnotí výraz v omezením a pak přenese ovládací prvek na koncový bod *expression_statement*. Koncový bod *expression_statement* dostupný, pokud to *expression_statement* je dostupný.

## <a name="selection-statements"></a>Příkazy výběru

Příkazy výběru vyberte jedním z možných příkazů pro spuštění na základě hodnoty některých výrazu.

```antlr
selection_statement
    : if_statement
    | switch_statement
    ;
```

### <a name="the-if-statement"></a>If – příkaz

`if` Příkaz vybere příkaz pro spuštění na základě hodnoty logického výrazu.

```antlr
if_statement
    : 'if' '(' boolean_expression ')' embedded_statement
    | 'if' '(' boolean_expression ')' embedded_statement 'else' embedded_statement
    ;
```

`else` Část souvisí s předchozím lexikálně nejbližší `if` , který je povolen pomocí syntaxe. Proto `if` příkaz formuláře
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

`if` Je proveden příkaz takto:

*  *Boolean_expression* ([logické výrazy](expressions.md#boolean-expressions)) je vyhodnocen.
*  Je-li logický výraz vrací `true`, ovládací prvek bude převeden na první příkaz vložený. Když, pokud ovládací prvek dosáhne koncového bodu, který tento příkaz Ovládací prvek bude převeden na koncový bod `if` příkazu.
*  Je-li logický výraz vrací `false` a pokud `else` část je k dispozici, ovládací prvek bude převeden na druhý vloženým příkazem. Když, pokud ovládací prvek dosáhne koncového bodu, který tento příkaz Ovládací prvek bude převeden na koncový bod `if` příkazu.
*  Je-li logický výraz vrací `false` a pokud `else` část není k dispozici, ovládací prvek bude převeden na koncový bod `if` příkaz.

Vložené první příkaz `if` příkaz je dostupný Pokud `if` příkaz je dostupný a logický výraz nemá konstantní hodnotu `false`.

Druhá vložené prohlášení o `if` příkazu, pokud jsou k dispozici, je dostupný Pokud `if` příkaz je dostupný a logický výraz nemá konstantní hodnotu `true`.

Koncový bod `if` příkaz je dostupný, pokud je dostupný koncový bod alespoň jeden z jeho vložených příkazů. Kromě toho koncový bod `if` příkaz bez `else` část je dostupný Pokud `if` příkaz je dostupný a logický výraz nemá konstantní hodnotu `true`.

### <a name="the-switch-statement"></a>Příkaz switch

Příkaz switch vybere pro spuštění seznamu příkazů s popisek přidružený přepínač, který odpovídá hodnotě výraz přepínače.

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

A *switch_statement* se skládá z klíčového slova `switch`, za nímž následuje výrazu v závorkách (označované jako výraz přepínače), za nímž následuje *switch_block*. *Switch_block* se skládá z nula nebo více *switch_section*s uzavřeny ve složených závorkách. Každý *switch_section* obsahuje jeden nebo více *switch_label*s za nímž následuje *statement_list* ([příkaz uvádí](statements.md#statement-lists)).

***Řídící typ*** z `switch` příkaz pokládáme stav, výrazem přepnutí.

*  Pokud je typ výrazu přepínače `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `bool`, `char`, `string`, nebo *enum_type*, nebo pokud je typ připouštějící hodnotu Null odpovídá jedné z těchto typů, pak je správní typ `switch` příkazu.
*  V opačném případě právě jeden uživatelský implicitní převod ([uživatelem definované převody](conversions.md#user-defined-conversions)) z typu výrazu přepínače, musí existovat na jednu z následujících možných řídící typy: `sbyte`, `byte`, `short` , `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `string`, nebo typ připouštějící hodnotu Null odpovídá jedné z těchto typů.
*  Jinak pokud neexistuje žádný implicitní převod, nebo pokud více než jedna implicitní převod existuje, dojde k chybě kompilace.

Konstantní výraz každé `case` popisku musí označují hodnotu, která implicitně převést ([implicitních převodů](conversions.md#implicit-conversions)) celopodnikové typu `switch` příkazu. Chyba kompilace nastane, pokud dva nebo více `case` popisky ve stejném `switch` příkazu zadejte stejnou hodnotu konstanty.

Může existovat maximálně jeden `default` popisků v příkazu switch.

A `switch` je proveden příkaz takto:

*  Výraz přepínače je vyhodnotit a převedeny na typ řízení.
*  Pokud některou z konstant podle `case` popisek ve stejném `switch` příkaz je rovna hodnotě výraz přepínače, ovládací prvek bude převeden do seznamu příkazů podle odpovídajícího `case` popisek.
*  Pokud žádná konstanta zadané v `case` popisky ve stejném `switch` příkaz je rovna hodnotě výraz přepínače a pokud `default` popisek je k dispozici, ovládací prvek bude převeden na příkaz následující seznam `default` Popisek.
*  Pokud žádná konstanta podle `case` popisky ve stejném `switch` příkaz je rovna hodnotě výraz přepínače a pokud ne `default` popisek je k dispozici, ovládací prvek bude převeden na koncový bod `switch` příkaz.

Pokud je dostupný koncový bod seznamu příkazů části přepínače, dojde k chybě kompilace. To se označuje jako "žádné fall prostřednictvím" pravidlo. V příkladu
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
je platný, protože žádný oddíl přepínače má dosažitelný koncový bod. Na rozdíl od jazyka C a C++ spouštěcí oddíl přepínače není dovoleno "předat" k další části přepínače a v příkladu
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
výsledkem chyba kompilace. Při provádění oddíl přepínače má následovat provádění jiné části přepínače, explicitní `goto case` nebo `goto default` musíte použít příkaz:
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

Více popisky jsou povoleny v *switch_section*. V příkladu
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
je platný. V příkladu nebudou porušovat pravidla "žádné fall prostřednictvím" protože popisky `case 2:` a `default:` jsou součástí stejného *switch_section*.

Pravidlo "žádné fall prostřednictvím" Zabrání běžné třídy chyb, ke kterým dochází v jazyce C a C++ při `break` příkazy jsou vynechány omylem. Kromě toho kvůli tomuto pravidlu oddílů přepínače `switch` příkaz může být libovolně změnit jejich uspořádání aniž by to ovlivnilo chování příkazu. Například oddíly `switch` výše uvedeného příkazu je možné vrátit zpět, aniž by to ovlivnilo chování příkazu:
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

Seznam příkazů oddíl přepínače se obvykle ukončí v `break`, `goto case`, nebo `goto default` je povolen příkaz, ale žádné konstrukci, která vykreslí koncový bod seznamu příkazů nedostupný. Například `while` příkaz řídí logický výraz `true` známé nikdy dosah její koncový bod. Podobně `throw` nebo `return` příkaz vždy přenese ovládací prvek jinde a nikdy dosáhne její koncový bod. Proto je platný v následujícím příkladu:
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

Typ řízení `switch` příkaz může být typu `string`. Příklad:
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

Operátory rovnosti řetězce, jako jsou ([řetězec operátory rovnosti](expressions.md#string-equality-operators)), `switch` příkazu je velká a malá písmena a spustí daný přepínač části pouze v případě, že řetězec výraz přepínače se přesně shoduje `case` popisek Toto je konstanta.

Když správní typ `switch` příkaz je `string`, hodnota `null` smí obsahovat jako konstanta popisek případu.

*Statement_list*s *switch_block* mohou obsahovat příkazy deklarace ([příkazy deklarace](statements.md#declaration-statements)). Obor lokální proměnné nebo konstantní deklarované v bloku přepínačů je blok switch.

Seznam příkazů daný přepínač oddílu je dostupný Pokud `switch` příkaz je dostupný a platí alespoň jedna z následujících akcí:

*  Výraz přepnutí má hodnotu, která není konstantní.
*  Výraz přepnutí má konstantní hodnotu, která odpovídá `case` popisek v části přepínače.
*  Výraz přepnutí má konstantní hodnotu, která neodpovídá žádnému `case` popisek a oddíl přepínače obsahuje `default` popisek.
*  Popisek přepínače oddíl přepínače se odkazuje dostupné `goto case` nebo `goto default` příkazu.

Koncový bod `switch` příkaz je dostupný, pokud platí alespoň jedna z následujících akcí:

*  `switch` Obsahuje prohlášení dostupný `break` příkaz, který ukončí `switch` příkazu.
*  `switch` Příkaz je dostupný, výraz přepnutí má hodnotu, která není konstantní a ne `default` popisek je k dispozici.
*  `switch` Příkaz je dostupný, výraz přepnutí má konstantní hodnotu, která neodpovídá žádnému `case` popisek a ne `default` popisek je k dispozici.

## <a name="iteration-statements"></a>Příkazy iterace

Příkazy iterace spouštět opakovaně vloženým příkazem.

```antlr
iteration_statement
    : while_statement
    | do_statement
    | for_statement
    | foreach_statement
    ;
```

### <a name="the-while-statement"></a>While – příkaz

`while` Příkaz podmíněně provede vloženým příkazem nulakrát nebo vícekrát.

```antlr
while_statement
    : 'while' '(' boolean_expression ')' embedded_statement
    ;
```

A `while` je proveden příkaz takto:

*  *Boolean_expression* ([logické výrazy](expressions.md#boolean-expressions)) je vyhodnocen.
*  Je-li logický výraz vrací `true`, je kontrola předána příkazu embedded. Pokud a v případě, že ovládací prvek dosáhne koncového bodu vloženým příkazem (pravděpodobně z provádění `continue` příkaz), ovládací prvek bude převeden na začátek `while` příkazu.
*  Je-li logický výraz vrací `false`, ovládací prvek bude převeden na koncový bod `while` příkazu.

V rámci příkazu vložený `while` příkaz, `break` – příkaz ([příkaz break](statements.md#the-break-statement)) slouží k řízení je převedeno na koncový bod `while` – příkaz (tedy končící iteraci vložený příkaz) a `continue` – příkaz ([příkaz continue](statements.md#the-continue-statement)) slouží k řízení je převedeno na koncový bod vloženým příkazem (tedy provádí další iteraci `while` příkaz).

Příkaz vložený `while` příkaz je dostupný Pokud `while` příkaz je dostupný a logický výraz nemá konstantní hodnotu `false`.

Koncový bod `while` příkaz je dostupný, pokud platí alespoň jedna z následujících akcí:

*  `while` Obsahuje prohlášení dostupný `break` příkaz, který ukončí `while` příkazu.
*  `while` Příkaz je dostupný a logický výraz nemá konstantní hodnotu `true`.

### <a name="the-do-statement"></a>– Příkaz

`do` Příkaz podmíněně provede vloženým příkazem jednou nebo vícekrát.

```antlr
do_statement
    : 'do' embedded_statement 'while' '(' boolean_expression ')' ';'
    ;
```

A `do` je proveden příkaz takto:

*  Ovládací prvek bude převeden na vloženým příkazem.
*  Pokud a v případě, že ovládací prvek dosáhne koncového bodu vloženým příkazem (pravděpodobně z provádění `continue` příkaz), *boolean_expression* ([logické výrazy](expressions.md#boolean-expressions)) je vyhodnocen. Je-li logický výraz vrací `true`, ovládací prvek bude převeden na začátek `do` příkazu. V opačném případě ovládací prvek bude převeden na koncový bod `do` příkazu.

V rámci příkazu vložený `do` příkaz, `break` – příkaz ([příkaz break](statements.md#the-break-statement)) slouží k řízení je převedeno na koncový bod `do` – příkaz (tedy končící iteraci vložený příkaz) a `continue` – příkaz ([příkaz continue](statements.md#the-continue-statement)) slouží k řízení je převedeno na koncový bod vloženým příkazem.

Příkaz vložený `do` příkaz je dostupný Pokud `do` příkaz je dostupný.

Koncový bod `do` příkaz je dostupný, pokud platí alespoň jedna z následujících akcí:

*  `do` Obsahuje prohlášení dostupný `break` příkaz, který ukončí `do` příkazu.
*  Koncový bod vloženým příkazem je dostupný a logický výraz nemá konstantní hodnotu `true`.

### <a name="the-for-statement"></a>Příkazu for

`for` Příkaz vyhodnocen jako posloupnost výrazy inicializace a poté, dokud je podmínka true, opakovaně provede vloženým příkazem a vyhodnocuje sekvenci výrazů iterace.

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

*For_initializer*, pokud jsou k dispozici, se skládá buď *local_variable_declaration* ([místní deklarace proměnné](statements.md#local-variable-declarations)) nebo seznam *statement_ výraz*s ([příkazy výrazů](statements.md#expression-statements)) oddělený čárkami. Obor lokální proměnná deklarovaná příkazem *for_initializer* začíná *local_variable_declarator* proměnné a rozšiřuje na konec vloženým příkazem. Rozsah zahrnuje *for_condition* a *for_iterator*.

*For_condition*, pokud jsou k dispozici, musí být *boolean_expression* ([logické výrazy](expressions.md#boolean-expressions)).

*For_iterator*, pokud jsou k dispozici, se skládá ze seznamu *statement_expression*s ([příkazy výrazů](statements.md#expression-statements)) oddělený čárkami.

Pro příkaz je proveden následujícím způsobem:

*  Pokud *for_initializer* je k dispozici, proměnné inicializátory nebo výrazy příkazu jsou provedeny v pořadí, jsou zapsány. Tento krok se provádí pouze jednou.
*  Pokud *for_condition* je k dispozici, je vyhodnocen.
*  Pokud *for_condition* není k dispozici nebo pokud je výsledkem vyhodnocení `true`, je kontrola předána příkazu embedded. Pokud a v případě, že ovládací prvek dosáhne koncového bodu vloženým příkazem (pravděpodobně z provádění `continue` příkaz), výrazů operátoru *for_iterator*, pokud existují, jsou vyhodnocovány v pořadí, a pak je jiné iterace provést, počínaje vyhodnocení *for_condition* v předchozím kroku.
*  Pokud *for_condition* je k dispozici a vyhodnocení vrací `false`, ovládací prvek bude převeden na koncový bod `for` příkazu.

V rámci příkazu vložený `for` příkaz, `break` – příkaz ([příkaz break](statements.md#the-break-statement)) slouží k řízení je převedeno na koncový bod `for` – příkaz (tedy končící iteraci vložený příkaz) a `continue` – příkaz ([příkaz continue](statements.md#the-continue-statement)) slouží k řízení je převedeno na koncový bod vloženým příkazem (tedy provádění *for_iterator* a provedení další iteraci `for` prohlášení, počínaje *for_condition*).

Příkaz vložený `for` příkaz je dostupný, pokud platí jedna z následujících akcí:

*  `for` Příkaz je dostupný a ne *for_condition* je k dispozici.
*  `for` Příkaz je dostupný a *for_condition* je k dispozici a nemá konstantní hodnotu `false`.

Koncový bod `for` příkaz je dostupný, pokud platí alespoň jedna z následujících akcí:

*  `for` Obsahuje prohlášení dostupný `break` příkaz, který ukončí `for` příkazu.
*  `for` Příkaz je dostupný a *for_condition* je k dispozici a nemá konstantní hodnotu `true`.

### <a name="the-foreach-statement"></a>Příkaz foreach

`foreach` Příkaz vytvoří výčet prvků kolekce, provádění vloženým příkazem pro každý prvek kolekce.

```antlr
foreach_statement
    : 'foreach' '(' local_variable_type identifier 'in' expression ')' embedded_statement
    ;
```

*Typ* a *identifikátor* z `foreach` deklarovat příkaz ***proměnné iterace*** příkazu. Pokud `var` identifikátor je zadána jako *local_variable_type*a žádný typ s názvem `var` je v oboru, iterační proměnná se říká, že ***proměnné implicitně typované iterace***, a její typ slov za typ prvku `foreach` prohlášení, jak je uvedeno níže. Iterační proměnná odpovídá jen pro čtení místní proměnné s rozsahem, který rozšiřuje nad vloženým příkazem. Během provádění `foreach` prohlášení, iterační proměnná představuje prvek kolekce, pro který je aktuálně provádění iterace. Chyba kompilace v případě vloženým příkazem se pokusí upravit proměnné iterace (prostřednictvím přiřazení nebo `++` a `--` operátory) nebo předejte jako proměnnou iterace `ref` nebo `out` parametru.

V následujícím příkladu jako stručný výtah `IEnumerable`, `IEnumerator`, `IEnumerable<T>` a `IEnumerator<T>` odkazují na odpovídající typy v oborech názvů `System.Collections` a `System.Collections.Generic`.

Kompilace zpracování příkazu foreach nejdřív zjistí, ***typ kolekce***, ***výčtovým typem*** a ***typ elementu*** výrazu. Určení probíhá následujícím způsobem:

*  Pokud typ `X` z *výraz* je typ pole je implicitní referenční převod z `X` k `IEnumerable` rozhraní (protože `System.Array` implementuje toto rozhraní). ***Typ kolekce*** je `IEnumerable` rozhraní, ***výčtovým typem*** je `IEnumerator` rozhraní a ***typ elementu*** je typ prvku Typ pole `X`.
*  Pokud typ `X` z *výraz* je `dynamic` je implicitní převod z *výraz* k `IEnumerable` rozhraní ([implicitní dynamic převody](conversions.md#implicit-dynamic-conversions)). ***Typ kolekce*** je `IEnumerable` rozhraní a ***výčtovým typem*** je `IEnumerator` rozhraní. Pokud `var` identifikátor je zadána jako *local_variable_type* pak bude ***typ elementu*** je `dynamic`, v opačném případě je `object`.
*  V opačném případě určit, zda typ `X` má odpovídající `GetEnumerator` metody:
   * Provádět vyhledávání člen v typu `X` s identifikátorem `GetEnumerator` a žádné argumenty typu. Pokud je výsledkem vyhledávání člen není shoda, nebo ho vytvoří nejednoznačnost, nebo je výsledný shodu, která není skupinou – metoda, hledá vyčíslitelná rozhraní jak je popsáno níže. Doporučuje se, že se objeví upozornění, pokud člen vyhledávání vytvoří nic s výjimkou skupinu metod nebo žádná shoda.
   * Proveďte řešení přetížení pomocí výsledný skupinu metod a prázdným seznamem argumentů. Řešení přetížení výsledků v žádné příslušné metody, výsledkem nejednoznačnost, zda výsledkem jednoho nejlepší metody, ale, že metoda je buď statický, nebo není veřejné vyhledat vyčíslitelná rozhraní, jak je popsáno níže. Doporučuje se, že se objeví upozornění v případě přetížení vytvoří nic s výjimkou metody jednoznačný veřejné instance nebo žádné použitelné metody.
   * Pokud návratový typ `E` z `GetEnumerator` metoda se nejedná o třídu, je vytvořen typ struktury nebo rozhraní, chybu a jsou přesměrováni žádné další kroky.
   * Člen vyhledávání se provádí na `E` s identifikátorem `Current` a žádné argumenty typu. Pokud člen vyhledávání vytvoří žádná shoda, výsledkem je chyba nebo výsledek je kromě veřejnou vlastnost instance, která umožňuje čtení, je vytvořen chybu a jsou přesměrováni žádné další kroky.
   * Člen vyhledávání se provádí na `E` s identifikátorem `MoveNext` a žádné argumenty typu. Pokud člen vyhledávání vytvoří žádná shoda, výsledkem je chyba nebo kromě skupinu metod. Výsledkem je, je vytvořen chybu a jsou přesměrováni žádné další kroky.
   * Řešení přetížení se provádí na skupinu metod s prázdným seznamem argumentů. Pokud výsledky rozlišení přetížení v žádné příslušné metody, výsledky v nejednoznačnost nebo výsledky v jediné nejlepší metody, ale tato metoda je buď statický, nebo není veřejné nebo její typ vrácené hodnoty není `bool`, je vytvořen chybu a jsou přesměrováni žádné další kroky.
   * ***Typ kolekce*** je `X`, ***výčtovým typem*** je `E`a ***typ elementu*** je typ `Current` vlastnost.

*  V opačném případě zkontrolujte vyčíslitelná rozhraní:
   * Pokud mezi všechny typy `Ti` pro něž neexistuje implicitní převod z `X` k `IEnumerable<Ti>`, je jedinečný typ `T` tak, aby `T` není `dynamic` a pro všechny ostatní `Ti` je implicitní převod z `IEnumerable<T>` k `IEnumerable<Ti>`, pak bude ***typ kolekce*** je rozhraní `IEnumerable<T>`, ***výčtovým typem*** je rozhraní `IEnumerator<T>`a ***typ elementu*** je `T`.
   * Jinak, pokud existuje více než jeden takový typ `T`, je vytvořen chybu a jsou provedeny žádné další kroky.
   * Jinak, pokud je implicitní převod z `X` k `System.Collections.IEnumerable` rozhraní, pak bude ***typ kolekce*** je toto rozhraní ***výčtovým typem*** je rozhraní `System.Collections.IEnumerator`a ***typ elementu*** je `object`.
   * V opačném případě je vytvořen chybu a jsou přesměrováni žádné další kroky.

Výše uvedené kroky, pokud je úspěšná, jednoznačně vytvořit typ kolekce `C`, výčtovým typem `E` a typ prvku `T`. Příkaz foreach formuláře
```csharp
foreach (V v in x) embedded_statement
```
je pak rozšířen pro:
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

Proměnná `e` není viditelná nebo přístupné pro výraz `x` nebo vloženým příkazem nebo jiný zdrojový kód programu. Proměnná `v` je jen pro čtení v vloženým příkazem. Pokud není k dispozici explicitní převod ([explicitních převodů](conversions.md#explicit-conversions)) z `T` (typ prvku) pro `V` ( *local_variable_type* v příkazu foreach), vytvořilo chybu a jsou přesměrováni žádné další kroky. Pokud `x` má hodnotu `null`, `System.NullReferenceException` je vyvolána v době běhu.

Implementace je povolený pro implementaci daného příkazu foreach odlišně, např. z důvodů výkonu za předpokladu, chování je konzistentní s rozšiřující se výše.

Umístění `v` uvnitř while je důležité, jak je zachycena v jakékoli anonymní funkci, ke kterým dochází v smyčky *embedded_statement*.

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
Pokud `v` byla deklarována mimo while smyčky, by být sdílena mezi všech iterací a jeho hodnota po smyčky by být konečná hodnota `13`, který je co vyvolání typu `f` vytiskněte. Místo toho protože každé iterace má svůj vlastní proměnnou `v`, je zachycená výrazem `f` v první iteraci budou pro uchování hodnoty `7`, což je, co budou zobrazeny. (Poznámka: starší verze jazyka C# deklarované `v` smyčku while mimo.)

Text nakonec bloku je vytvořený podle následujících kroků:

*  Pokud je implicitní převod z `E` k `System.IDisposable` rozhraní, pak
   *  Pokud `E` je typ hodnoty Null bude nakonec klauzule rozbalen a sémantické ekvivalent:

      ```csharp
      finally {
          ((System.IDisposable)e).Dispose();
      }
      ```

   *  V opačném případě nakonec klauzule rozbalen a sémantické ekvivalent:

      ```csharp
      finally {
          if (e != null) ((System.IDisposable)e).Dispose();
      }
      ```

   s výjimkou, že pokud `E` je hodnotový typ nebo parametr typu instance typu hodnoty a pak přetypování souřadnic `e` k `System.IDisposable` nezpůsobí zabalení dochází.

*  Jinak, pokud `E` je zapečetěný typ, nakonec klauzule rozbalen a prázdný blok:

   ```csharp
   finally {
   }
   ```

*  V opačném případě nakonec klauzule rozbalen do:

   ```csharp
   finally {
       System.IDisposable d = e as System.IDisposable;
       if (d != null) d.Dispose();
   }
   ```    

   Lokální proměnná `d` není viditelné pro nebo přístup k veškerému kódu uživatele. Konkrétně to není v konfliktu s jakoukoli jinou proměnnou, jejíž obor obsahuje bloku finally.

Pořadí, ve kterém `foreach` prochází přes prvky pole, vypadá takto: Pro prvky jednorozměrná pole je provázán ve vzestupném pořadí indexů, počínaje indexem `0` a konče index `Length - 1`. Pro vícerozměrná pole jsou prvky procházet tak, že indexy rozměr nejvíce vpravo se zvýšenou první pak další levé dimenze nalevo a tak dále.

Následující příklad vytiskne každé hodnoty v dvojrozměrné pole, v pořadí elementů:
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
Výstup vytvořený vypadá takto:
```csharp
1.2 2.3 3.4 4.5 5.6 6.7 7.8 8.9
```

V příkladu
```csharp
int[] numbers = { 1, 3, 5, 7, 9 };
foreach (var n in numbers) Console.WriteLine(n);
```
typ `n` odvozena jako `int`, typ elementu `numbers`.

## <a name="jump-statements"></a>Jump – příkazy

Příkazy skoku bezpodmínečně přenést řízení.

```antlr
jump_statement
    : break_statement
    | continue_statement
    | goto_statement
    | return_statement
    | throw_statement
    ;
```

Umístění, ke kterému předává příkaz skoku řízení se nazývá ***cílové*** příkazu jump.

Pokud v rámci bloku dojde k výpisu odkazů, a cíl, který tento příkaz skoku je mimo tento blok, příkaz skoku se říká, že ***ukončit*** bloku. Při výpisu odkazů může přenést řízení mimo blok, se nikdy nepřenášejí ovládacího prvku do bloku.

Spuštění příkazů skoku je složité přítomností intervenující `try` příkazy. Chybí těchto `try` příkazy, příkaz skoku bezpodmínečně přenese ovládací prvek z příkazu skoku k cíli. Za přítomnosti takové intervenující `try` příkazy, provedení je složitější. Pokud se příkaz skoku ukončí jeden nebo více `try` přidružené bloky s `finally` bloky, ovládací prvek zpočátku bude převeden na `finally` bloku nejvnitřnější `try` příkazu. Pokud a v případě, že ovládací prvek dosáhne koncového bodu `finally` bloku, ovládací prvek bude převeden na `finally` další uzavírající blok `try` příkazu. Tento proces se opakuje, dokud `finally` všechny bloky použité `try` příkazy byly provedeny.

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
`finally` bloky, které jsou přidružené dvě `try` příkazy jsou spouštěny před ovládací prvek bude převeden na cíl příkazu skoku.

Výstup vytvořený vypadá takto:
```
Before break
Innermost finally block
Outermost finally block
After break
```

### <a name="the-break-statement"></a>Příkaz break

`break` Ukončí nejbližšího obklopujícího příkazu `switch`, `while`, `do`, `for`, nebo `foreach` příkazu.

```antlr
break_statement
    : 'break' ';'
    ;
```

Cíl `break` příkaz je koncový bod nejbližšího obklopujícího `switch`, `while`, `do`, `for`, nebo `foreach` příkazu. Pokud `break` příkazu není uzavřená v `switch`, `while`, `do`, `for`, nebo `foreach` příkaz, vyvolá chybu v době kompilace.

Když více `switch`, `while`, `do`, `for`, nebo `foreach` příkazy jsou vnořeny do sebe navzájem `break` příkaz platí jenom pro vnitřní příkaz. Přenos řízení napříč více úrovní vnoření, `goto` – příkaz ([příkazu goto](statements.md#the-goto-statement)) musí být použita.

A `break` příkaz nemůže ukončit `finally` blok ([příkazu try](statements.md#the-try-statement)). Při `break` příkazu vyvolá se v rámci `finally` blokovat, cíl `break` příkaz musí být v rámci stejného `finally` blokovat; v opačném případě dojde k chybě kompilace.

A `break` je proveden příkaz takto:

*  Pokud `break` příkaz ukončí jeden nebo více `try` přidružené bloky s `finally` bloky, ovládací prvek zpočátku bude převeden na `finally` bloku nejvnitřnější `try` příkaz. Pokud a v případě, že ovládací prvek dosáhne koncového bodu `finally` bloku, ovládací prvek bude převeden na `finally` další uzavírající blok `try` příkazu. Tento proces se opakuje, dokud `finally` všechny bloky použité `try` příkazy byly provedeny.
*  Ovládací prvek bude převeden do cílového místa `break` příkazu.

Protože `break` příkaz bezpodmínečně převede ovládací prvek jinde, koncový bod `break` příkazu není dostupný.

### <a name="the-continue-statement"></a>Příkaz continue

`continue` Spustí novou iteraci nejbližšího ohraničujícího příkazu `while`, `do`, `for`, nebo `foreach` příkazu.

```antlr
continue_statement
    : 'continue' ';'
    ;
```

Cíl `continue` příkaz je koncový bod vloženým příkazem nejbližšího obklopujícího `while`, `do`, `for`, nebo `foreach` příkazu. Pokud `continue` příkazu není uzavřená v `while`, `do`, `for`, nebo `foreach` příkaz, vyvolá chybu v době kompilace.

Když více `while`, `do`, `for`, nebo `foreach` příkazy jsou vnořeny do sebe navzájem `continue` příkaz platí jenom pro vnitřní příkaz. Přenos řízení napříč více úrovní vnoření, `goto` – příkaz ([příkazu goto](statements.md#the-goto-statement)) musí být použita.

A `continue` příkaz nemůže ukončit `finally` blok ([příkazu try](statements.md#the-try-statement)). Při `continue` příkazu vyvolá se v rámci `finally` blokovat, cíl `continue` příkaz musí být v rámci stejného `finally` blokovat; v opačném případě dojde k chybě kompilace.

A `continue` je proveden příkaz takto:

*  Pokud `continue` příkaz ukončí jeden nebo více `try` přidružené bloky s `finally` bloky, ovládací prvek zpočátku bude převeden na `finally` bloku nejvnitřnější `try` příkaz. Pokud a v případě, že ovládací prvek dosáhne koncového bodu `finally` bloku, ovládací prvek bude převeden na `finally` další uzavírající blok `try` příkazu. Tento proces se opakuje, dokud `finally` všechny bloky použité `try` příkazy byly provedeny.
*  Ovládací prvek bude převeden do cílového místa `continue` příkazu.

Protože `continue` příkaz bezpodmínečně převede ovládací prvek jinde, koncový bod `continue` příkazu není dostupný.

### <a name="the-goto-statement"></a>Goto – příkaz

`goto` Příkaz přenese ovládací prvek na příkaz, který je označen popiskem.

```antlr
goto_statement
    : 'goto' identifier ';'
    | 'goto' 'case' constant_expression ';'
    | 'goto' 'default' ';'
    ;
```

Cíl `goto` *identifikátor* příkaz je příkaz s popiskem s danou popiskem. Pokud popisek se zadaným názvem neexistuje v aktuálním členské funkce, nebo pokud `goto` příkaz není v rámci oboru popisku, dojde k chybě kompilace. Toto pravidlo povoluje použití `goto` příkaz k předání řízení z vnořené oboru, ale ne do vnořené oboru. V příkladu
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
`goto` prohlášení se používá k přenosu řízení z vnořené oboru.

Cíl `goto case` příkaz je seznamu příkazů ve těsně uzavírajícím `switch` – příkaz ([příkazu switch](statements.md#the-switch-statement)), obsahující `case` popisek s danou hodnotu konstanty. Pokud `goto case` příkazu není uzavřená v `switch` příkazu, pokud *constant_expression* není implicitně převést ([implicitní převody](conversions.md#implicit-conversions)) celopodnikové typu nejbližší uzavírající `switch` příkazu, nebo pokud nejbližšího obklopujícího `switch` neobsahuje příkaz `case` popisek s danou konstantní hodnoty, dojde k chybě kompilace.

Cíl `goto default` příkaz je seznamu příkazů ve těsně uzavírajícím `switch` – příkaz ([příkazu switch](statements.md#the-switch-statement)), obsahující `default` popisek. Pokud `goto default` příkazu není uzavřená v `switch` příkazu, nebo pokud nejbližšího obklopujícího `switch` příkaz neobsahuje `default` popisek, dojde k chybě kompilace.

A `goto` příkaz nemůže ukončit `finally` blok ([příkazu try](statements.md#the-try-statement)). Při `goto` příkazu vyvolá se v rámci `finally` blokovat, cíl `goto` příkaz musí být v rámci stejného `finally` blok, nebo v opačném případě dojde k chybě kompilace.

A `goto` je proveden příkaz takto:

*  Pokud `goto` příkaz ukončí jeden nebo více `try` přidružené bloky s `finally` bloky, ovládací prvek zpočátku bude převeden na `finally` bloku nejvnitřnější `try` příkaz. Pokud a v případě, že ovládací prvek dosáhne koncového bodu `finally` bloku, ovládací prvek bude převeden na `finally` další uzavírající blok `try` příkazu. Tento proces se opakuje, dokud `finally` všechny bloky použité `try` příkazy byly provedeny.
*  Ovládací prvek bude převeden do cílového místa `goto` příkazu.

Protože `goto` příkaz bezpodmínečně převede ovládací prvek jinde, koncový bod `goto` příkazu není dostupný.

### <a name="the-return-statement"></a>Příkaz return

`return` Příkaz vrátí řízení volajícímu aktuální funkce, ve kterém `return` objevuje příkaz.

```antlr
return_statement
    : 'return' expression? ';'
    ;
```

A `return` příkaz s žádný výraz lze použít pouze v funkce členu, který nelze vypočítat hodnotu tedy metoda s výsledným typem ([tělo metody](classes.md#method-body)) `void`, `set` přistupující objekt vlastnosti nebo indexer, který je `add` a `remove` přistupující objekty událost, konstruktor instance, statický konstruktor nebo destruktor.

A `return` příkaz s výrazem jde použít jenom v členské funkci, která vypočítá hodnotu tedy metoda s typem výsledku není void, `get` přistupující objekt vlastnosti nebo indexeru nebo uživatelem definovaný operátor. Implicitní převod ([implicitních převodů](conversions.md#implicit-conversions)) z typu výrazu, musí existovat na návratový typ obsahující členské funkce.

Vrátí příkazy lze použít také v těle výrazu anonymní funkce ([výrazy anonymní funkce](expressions.md#anonymous-function-expressions)) a účast při určování, které převody existovat pro tyto funkce.

Je chyba kompilace pro `return` příkazu se zobrazí v `finally` blok ([příkazu try](statements.md#the-try-statement)).

A `return` je proveden příkaz takto:

*  Pokud `return` příkaz Určuje výraz, výraz je vyhodnocen a výsledná hodnota je převedena na typ vrácené hodnoty funkce obsahující podle implicitní převod. Výsledkem převodu se změní hodnota výsledku vytvořeného funkcí.
*  Pokud `return` příkazu není uzavřen v jedné nebo více `try` nebo `catch` přidružené bloky s `finally` bloky, ovládací prvek zpočátku bude převeden na `finally` bloku nejvnitřnější `try` příkaz. Pokud a v případě, že ovládací prvek dosáhne koncového bodu `finally` bloku, ovládací prvek bude převeden na `finally` další uzavírající blok `try` příkazu. Tento proces se opakuje, dokud `finally` blokuje všechny nadřazené `try` příkazy byly provedeny.
*  Pokud obsahující funkce není asynchronní funkce, ovládací prvek se vrátí volajícímu metody obsahující funkce včetně hodnoty výsledku pokud existuje.
*  Pokud je asynchronní funkce obsahující funkce, ovládací prvek se vrátí volajícímu aktuální a výslednou hodnotu, pokud existuje, je zaznamenán v Vrácená úloha jak je popsáno v ([rozhraní pro výčty](classes.md#enumerator-interfaces)).

Protože `return` příkaz bezpodmínečně převede ovládací prvek jinde, koncový bod `return` příkazu není dostupný.

### <a name="the-throw-statement"></a>Příkaz throw

`throw` Příkazu dojde k výjimce.

```antlr
throw_statement
    : 'throw' expression? ';'
    ;
```

A `throw` Vyvolá příkaz s výrazem hodnotu vytvořenou testovaným vyhodnocení výrazu. Výraz musí označují hodnotu typu třídy `System.Exception`, typu třídy, která je odvozena z `System.Exception` nebo typu parametru typu, který má `System.Exception` (nebo jejich podtřída) jako jeho základní třída. V případě vyhodnocování výrazu vytvoří `null`, `System.NullReferenceException` místo toho je vyvolána výjimka.

A `throw` příkaz s žádný výraz se dá použít jenom ve `catch` blokovat, v takovém případě, který tento příkaz znovu vyvolá výjimku, která je aktuálně zpracovávanou díky tomu `catch` bloku.

Protože `throw` příkaz bezpodmínečně převede ovládací prvek jinde, koncový bod `throw` příkazu není dostupný.

Když je vyvolána výjimka, ovládací prvek bude převeden na první `catch` klauzuli v nadřazeném `try` příkaz, který může zpracovat výjimku. Proces, který se provádí z bodu výjimky do místa přenos řízení na obslužnou rutinu vhodná výjimka se označuje jako ***šíření výjimek***. Šíření výjimky se skládá z opakovaného vyhodnocování následující kroky, dokud `catch` nenajde klauzuli, která odpovídá výjimku. V tomto popisu ***throw bodu*** je zpočátku umístění, ve kterém je vyvolána výjimka.

*  V aktuální funkce členu každý `try` příkaz, který obklopuje bodu throw je zkontrolován. Příkazu for each `S`počínaje nejvnitřnější `try` příkazu a konče nejkrajnější `try` prohlášení, jsou vyhodnocovány následující kroky:

   * Pokud `try` bloku `S` obklopuje bodu throw a pokud S má jeden nebo více `catch` klauzulí se operátor `catch` klauzule jsou zkoumány podle pořadí vzhled pro vyhledání vhodné obslužné rutiny pro výjimky, podle pravidel zadaných v Část [příkazu try](statements.md#the-try-statement). Pokud odpovídající `catch` klauzule nachází, šíření výjimek je dokončit přenos řízení na blok, který `catch` klauzuli.

   * Jinak, pokud `try` bloku nebo `catch` bloku `S` obklopuje bodu throw a pokud `S` má `finally` bloku, ovládací prvek bude převeden na `finally` bloku. Pokud `finally` bloku vyvolá další výjimku, se ukončí zpracování aktuální výjimky. Jinak, když ovládací prvek dosáhne koncového bodu `finally` bloku, pokračovat zpracování aktuální výjimky.

*  Pokud nebyl v aktuálním volání funkce obslužné rutiny výjimek, volání funkce se ukončí a dojde k jedné z následujících akcí:

   * Pokud je aktuální funkce není asynchronní, se opakují výše uvedených kroků pro volající funkci s bodem throw odpovídající příkaz, ze kterého byla vyvolána členské funkce.

   * Pokud je aktuální funkce async a vracející úlohy, výjimka se zaznamená do vrácené úlohou, které přejde do stavu chybovém nebo bylo zrušeno, jak je popsáno v [rozhraní pro výčty](classes.md#enumerator-interfaces).

   * Pokud aktuální funkci je asynchronní a vracející hodnotu void, je oznámení kontext synchronizace aktuálního vlákna, jak je popsáno v [vyčíslitelná rozhraní](classes.md#enumerable-interfaces).

*  Zpracování výjimek ukončí všechny volání členské funkce v aktuálním vlákně, označující, že vlákno nemá žádná obslužná rutina výjimky, pak vlákna je ukončen. Dopad ukončení, je definováno implementací.

## <a name="the-try-statement"></a>Příkaz try

`try` Příkaz poskytuje mechanismus pro zachycování výjimek, ke kterým dochází při provádění bloku. Kromě toho `try` poskytuje možnost určit blok kódu, který je vždy spuštěn, když ovládací prvek opustí příkaz `try` příkazu.

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

Existují tři možné formy `try` příkazy:

*  A `try` bloku, za nímž následuje jedna nebo více `catch` bloky.
*  A `try` bloku, za nímž následuje `finally` bloku.
*  A `try` bloku, za nímž následuje jedna nebo více `catch` bloky, za nímž následuje `finally` bloku.

Při `catch` určuje klauzuli *exception_specifier*, musí být typu `System.Exception`, typ, který je odvozen od `System.Exception` nebo typ parametru typu, který má `System.Exception` (nebo jejich podtřída) jako jeho platit Základní třída.

Při `catch` klauzule určuje, jak *exception_specifier* s *identifikátor*, ***proměnné exception*** zadaný název a typ je deklarován. Proměnné exception odpovídá na místní proměnnou s rozsahem, který rozšiřuje přes `catch` klauzuli. Během provádění *exception_filter* a *bloku*, proměnné exception představuje aktuálně zpracovávanou výjimku. Pro účely kontroly jednoznačného přiřazení proměnné exception se považuje za jednoznačně přiřazena v jeho celý rozsah.

Pokud `catch` klauzule obsahuje název proměnné výjimky, není možné získat přístup k objektu výjimka ve filtru a `catch` bloku.

A `catch` klauzuli, která neurčuje *exception_specifier* nazývá Obecné `catch` klauzuli.

Některé z nich můžou podporovat výjimky, které nejsou reprezentovat objekt odvozena z `System.Exception`, i když tyto výjimky může nebudou nikdy generována pomocí kódu jazyka C#. Obecné `catch` klauzuli může použít k zachycení takové výjimky. Proto Obecné `catch` je klauzule sémanticky rozdílnými z jednoho, který určuje typ `System.Exception`, v tom, že dříve uvedené může také zachytit výjimky z jiných jazyků.

Aby bylo možné najít obslužnou rutinu výjimky, `catch` klauzule jsou zkoumány podle pořadí slov. Pokud `catch` klauzule určuje typ, ale žádný filtr výjimek, je chyba kompilace pro pozdější `catch` klauzule ve stejném `try` příkaz a zadejte typ, který je stejný jako, nebo je odvozen z typu. Pokud `catch` klauzule určuje žádný typ a žádný filtr, musí být poslední `catch` klauzuli, která `try` příkazu.

V rámci `catch` bloku, `throw` – příkaz ([příkazu throw](statements.md#the-throw-statement)) s žádný výraz lze použít pro opětovné vyvolání výjimky, která byla zachycena `catch` bloku. Přiřazení proměnné výjimka nemění, který je znovu vyvolána výjimka.

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
Metoda `F` zachytí výjimku, zapíše některé diagnostické informace do konzoly, mění proměnné exception a znovu vyvolá výjimku. Výjimka, která je znovu vyvolána je původní výjimky, takže je výstup vytvořený:
```
Exception in F: G
Exception in Main: G
```

Pokud měl vyvolána první blok catch `e` místo opětné vyvolání aktuální výjimky, výstup vytvořený by měl vypadat takto:
```csharp
Exception in F: G
Exception in Main: F
```

Je chyba kompilace pro `break`, `continue`, nebo `goto` příkazu k předání řízení z celkového počtu `finally` bloku. Při `break`, `continue`, nebo `goto` příkaz probíhá `finally` bloku, cílem příkazu musí být v rámci stejného `finally` bloku, nebo v opačném případě dojde k chybě kompilace.

Je chyba kompilace pro `return` vyskytuje v příkazu `finally` bloku.

A `try` je proveden příkaz takto:

*  Ovládací prvek bude převeden na `try` bloku.
*  Pokud a v případě, že ovládací prvek dosáhne koncového bodu `try` blok:
   *  Pokud `try` příkaz má `finally` bloku `finally` je blok proveden.
   *  Ovládací prvek bude převeden na koncový bod `try` příkazu.

*  Pokud se výjimka šíří do `try` příkaz během provádění `try` blok:
   *  `catch` Klauzule, pokud existují, jsou zkoumány podle pořadí vzhled najít vhodné obslužné rutiny výjimky. Pokud `catch` klauzule není zadán typ nebo určuje typ výjimky nebo základní typ typu výjimky:
      *  Pokud `catch` klauzule deklaruje proměnnou výjimky, je přiřazen objekt výjimky do proměnné výjimky.
      *  Pokud `catch` deklaruje klauzule filtru výjimky, je vyhodnocen filtru. Pokud je vyhodnocen jako `false`klauzule catch není shoda a pokračuje v hledání prostřednictvím libovolného následné `catch` klauzule pro obslužnou rutinu vhodná.
      *  V opačném případě `catch` klauzule považován za shodný a ovládací prvek bude převeden na odpovídající `catch` bloku.
      *  Pokud a v případě, že ovládací prvek dosáhne koncového bodu `catch` blok:
         * Pokud `try` příkaz má `finally` bloku `finally` je blok proveden.
         * Ovládací prvek bude převeden na koncový bod `try` příkazu.
      *  Pokud se výjimka šíří do `try` příkaz během provádění `catch` blok:
         *  Pokud `try` příkaz má `finally` bloku `finally` je blok proveden.
         *  Výjimka se šíří do další uzavírající `try` příkazu.
   *  Pokud `try` příkazu nemá žádné `catch` klauzule nebo pokud žádná `catch` klauzule odpovídá výjimku:
      *  Pokud `try` příkaz má `finally` bloku `finally` je blok proveden.
      *  Výjimka se šíří do další uzavírající `try` příkazu.

Příkazy `finally` bloku jsou provedeny vždy, když ovládací prvek opustí `try` příkazu. Je hodnota true Určuje, zda přenos ovládacího prvku dojde v důsledku normálního provedení, v důsledku spuštění `break`, `continue`, `goto`, nebo `return` příkazu, nebo v důsledku šíření výjimky z `try` příkaz.

Pokud dojde k výjimce za běhu `finally` blok a není zachycena v rámci stejného bloku finally, se výjimka šíří do další značka `try` příkazu. Pokud právě rozšířen byla další výjimku, tato výjimka bude ztracena. Proces šíření výjimky je popsána dále v popisu `throw` – příkaz ([příkazu throw](statements.md#the-throw-statement)).

`try` Bloku `try` příkaz je dostupný Pokud `try` příkaz je dostupný.

A `catch` bloku `try` příkaz je dostupný Pokud `try` příkaz je dostupný.

`finally` Bloku `try` příkaz je dostupný Pokud `try` příkaz je dostupný.

Koncový bod `try` příkaz je dostupný, pokud jsou splněny obě z následujících akcí:

*  Koncový bod `try` blok je dostupný nebo koncový bod alespoň jeden `catch` blok je dostupný.
*  Pokud `finally` blok je k dispozici, koncový bod `finally` blok je dostupný.

## <a name="the-checked-and-unchecked-statements"></a>Příkazy zaškrtnuto a nezaškrtnuto

`checked` a `unchecked` příkazy se používají k řízení ***kontextu kontroly přetečení*** integrálového typu aritmetické operace a převody.

```antlr
checked_statement
    : 'checked' block
    ;

unchecked_statement
    : 'unchecked' block
    ;
```

`checked` Příkaz způsobí, že všechny výrazy v *bloku* který se má vyhodnotit ve zkontrolovaném kontextu a `unchecked` příkaz způsobí, že všechny výrazy v *bloku* má být vyhodnocen v nezkontrolovaném kontextu.

`checked` a `unchecked` příkazy jsou přesně odpovídá `checked` a `unchecked` operátory ([operátory zaškrtnuto a nezaškrtnuto](expressions.md#the-checked-and-unchecked-operators)), s tím rozdílem, že tyto nástroje fungují na bloky namísto výrazů .

## <a name="the-lock-statement"></a>Příkaz lock

`lock` Příkaz získá zámek pro vzájemné vyloučení pro daný objekt, provede příkaz a poté uvolní zámek.

```antlr
lock_statement
    : 'lock' '(' expression ')' embedded_statement
    ;
```

Výraz `lock` příkaz musí označují hodnotu typu známé jako *reference_type*. Žádný převod na uzavřené určení implicitní ([zabalení převody](conversions.md#boxing-conversions)) je nikdy neprováděl pro výraz `lock` příkaz a proto je chyba kompilace pro výraz, který má označení hodnotu *value_type*.

A `lock` příkaz formuláře
```csharp
lock (x) ...
```
kde `x` je výraz *reference_type*, přesně odpovídá
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
s tím rozdílem, že `x` se jenom vyhodnotí jednou.

Dokud je držen zámek vzájemné vyloučení, provádění kódu ve stejném vláknu spuštění můžete také získat a uvolní zámek. Nicméně spuštění kódu v jiných vláken se blokuje získání zámku, dokud se zámek je uvolněn.

Uzamčení `System.Type` objektů, aby se synchronizovaly přístup ke statickým datům se nedoporučuje. Další kód může zamknout na stejný typ, který může vést k zablokování. Lepším řešením je synchronizovat přístup ke statickým datům zamčením privátní statický objekt. Příklad:
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

`using` Příkaz získá jeden nebo více zdrojů, provede příkaz a uvolní prostředku.

```antlr
using_statement
    : 'using' '(' resource_acquisition ')' embedded_statement
    ;

resource_acquisition
    : local_variable_declaration
    | expression
    ;
```

A ***prostředků*** je třída nebo struktura, která implementuje `System.IDisposable`, což zahrnuje jeden konstruktor bez parametrů metodu s názvem `Dispose`. Můžete volat kód, který používá prostředek `Dispose` k označení, že prostředek už nepotřebujete. Pokud `Dispose` není volána, dojde k automatické vyřazení nakonec následkem uvolňování paměti.

Pokud formu *resource_acquisition* je *local_variable_declaration* potom je typ *local_variable_declaration* musí být buď `dynamic` nebo typ. který lze implicitně převést na `System.IDisposable`. Pokud formu *resource_acquisition* je *výraz* tento výraz musí být implicitně převeditelný na `System.IDisposable`.

Lokální proměnné deklarované v *resource_acquisition* jsou jen pro čtení a musí obsahovat inicializátor. Chyba kompilace v případě vloženým příkazem se pokusí upravit tyto místní proměnné (prostřednictvím přiřazení nebo `++` a `--` operátory), převzít adresu proměnné je, nebo předejte jako `ref` nebo `out` parametry.

A `using` příkaz je přeložen do tří částí: získání, využití a vyřazení. Využití prostředku je implicitně uzavřen v `try` příkaz, který zahrnuje `finally` klauzuli. To `finally` klauzule uvolní prostředku. Pokud `null` prostředku je požadován, pak bez volání `Dispose` se provádí, a není vyvolána žádná výjimka. Pokud prostředek je typu `dynamic` převede se dynamicky prostřednictvím implicitního převodu dynamického ([implicitních převodů dynamické](conversions.md#implicit-dynamic-conversions)) k `IDisposable` během pořízení, aby se zajistilo, že je převod úspěšná, až poté využití a vyřazení.

A `using` příkaz formuláře
```csharp
using (ResourceType resource = expression) statement
```
představuje jednu ze tří možných rozšíření. Když `ResourceType` je typ hodnoty Null, je rozšíření
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

V opačném případě `ResourceType` je typ s možnou hodnotou Null nebo odkazového typu jiného než `dynamic`, je rozšíření
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

V opačném případě `ResourceType` je `dynamic`, je rozšíření
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

V obou rozšíření `resource` je proměnná jen pro čtení v příkazu vložené a `d` proměnná se do nedostupný a neviditelná, vloženým příkazem.

Implementace je povolený pro implementaci daného příkazu using odlišně, např. z důvodů výkonu za předpokladu, chování je konzistentní s rozšiřující se výše.

A `using` příkaz formuláře
```csharp
using (expression) statement
```
má stejné tři možné rozšíření. V tomto případě `ResourceType` je implicitně typu kompilace `expression`, má-li nějaký. V opačném případě rozhraní `IDisposable` samotné se používá jako `ResourceType`. `resource` Proměnná se do nedostupný a neviditelná, vloženým příkazem.

Když *resource_acquisition* má formu *local_variable_declaration*, je možné získat více prostředků daného typu. A `using` příkaz formuláře
```csharp
using (ResourceType r1 = e1, r2 = e2, ..., rN = eN) statement
```
je vnořená přesně odpovídá sekvenci `using` příkazy:
```csharp
using (ResourceType r1 = e1)
    using (ResourceType r2 = e2)
        ...
            using (ResourceType rN = eN)
                statement
```

Následující příklad vytvoří soubor s názvem `log.txt` a zapíše dva řádky textu do souboru. V příkladu pak otevře že stejný soubor pro čtení a zkopíruje omezením řádků textu do konzoly.
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

Protože `TextWriter` a `TextReader` implementace třídy `IDisposable` rozhraní, můžete použít v příkladu `using` příkazy a ujistěte se, že je podkladový soubor řádně uzavřeny po zápisu operace čtení.

## <a name="the-yield-statement"></a>Příkaz yield

`yield` Prohlášení se používá v blok iterátoru ([bloky](statements.md#blocks)) na hodnotu, která objekt enumerator ([výčtu objektů](classes.md#enumerator-objects)) nebo vyčíslitelný objekt ([vyčíslitelné objekty](classes.md#enumerable-objects)) iterátoru nebo který signalizuje, že konce iterace.

```antlr
yield_statement
    : 'yield' 'return' expression ';'
    | 'yield' 'break' ';'
    ;
```

`yield` není vyhrazené slovo má zvláštní význam pouze bezprostředně před `return` nebo `break` – klíčové slovo. V jiných kontextech `yield` lze použít jako identifikátor.

Existuje několik omezení umístění, ve kterém `yield` příkazu můžete zobrazit, jak je popsáno v následující.

*  Je chyba kompilace pro `yield` – příkaz (z obou tvarech) zobrazit mimo *method_body*, *operator_body* nebo *accessor_body*
*  Je chyba kompilace pro `yield` – příkaz (z obou tvarech) se zobrazí uvnitř anonymní funkce.
*  Je chyba kompilace pro `yield` – příkaz (z obou tvarech) se zobrazí v `finally` klauzuli `try` příkazu.
*  Je chyba kompilace pro `yield return` vyskytovat kdekoli v příkazu `try` příkaz, který obsahuje některý `catch` klauzule.

Následující příklad ukazuje některé platné a neplatné použití `yield` příkazy.

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

Implicitní převod ([implicitních převodů](conversions.md#implicit-conversions)) z typu výrazu v, musí existovat `yield return` příkazu yield typ ([Yield typ](classes.md#yield-type)) iterátoru.

A `yield return` je proveden příkaz takto:

*  Výraz zadaný v příkazu je vyhodnocen, implicitně převeden na typ yield a přiřazená `Current` vlastnost v objektu enumerátor.
*  Spouštění bloku iterátoru je pozastaveno. Pokud `yield return` příkaz je v jedné nebo více `try` blokuje, přidružené `finally` v tuto chvíli nejsou provedeny bloky.
*  `MoveNext` Vrátí metoda objekt enumerator `true` na volající funkci, která udává, že objekt enumerator byly úspěšně posunuty na další položku.

Další volání objekt enumerator `MoveNext` metoda pokračuje v provádění bloku iterátoru od kdy posledního pozastavení.

A `yield break` je proveden příkaz takto:

*  Pokud `yield break` příkazu není uzavřen v jedné nebo více `try` přidružené bloky s `finally` bloky, ovládací prvek zpočátku bude převeden na `finally` bloku nejvnitřnější `try` příkaz. Pokud a v případě, že ovládací prvek dosáhne koncového bodu `finally` bloku, ovládací prvek bude převeden na `finally` další uzavírající blok `try` příkazu. Tento proces se opakuje, dokud `finally` blokuje všechny nadřazené `try` příkazy byly provedeny.
*  Ovládací prvek se vrátí volajícímu metody blok iterátoru. Je to `MoveNext` metoda nebo `Dispose` metodu objektu enumerátor.

Protože `yield break` příkaz bezpodmínečně převede ovládací prvek jinde, koncový bod `yield break` příkazu není dostupný.
