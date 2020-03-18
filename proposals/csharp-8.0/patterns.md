---
ms.openlocfilehash: fad1425e2871f395a2eb1f39faccbc773d88d6a3
ms.sourcegitcommit: da1180f7eacdd5067b32d291a76e6764159e00fe
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/05/2019
ms.locfileid: "79485061"
---
# <a name="recursive-pattern-matching"></a>Rekurzivní porovnávání vzorů

## <a name="summary"></a>Souhrn
[summary]: #summary

Rozšíření pro porovnávání vzorů pro C# zajištění mnoha výhod algebraických datových typů a porovnávání vzorů z funkčních jazyků, ale způsobem, který hladce integruje s chováním základního jazyka. Prvky tohoto přístupu jsou nechte inspirovat souvisejícími funkcemi v programovacích jazycích [F#](http://www.msr-waypoint.net/pubs/79947/p29-syme.pdf "Rozšiřitelné porovnávání vzorů přes zjednodušený jazyk") a [Scala](https://link.springer.com/content/pdf/10.1007%2F978-3-540-73589-2.pdf "Vyhovující objekty se vzory, stránka 273").

## <a name="detailed-design"></a>Podrobný návrh
[design]: #detailed-design

### <a name="is-expression"></a>Výraz is

Operátor `is` je rozšířen o testování výrazu na základě *vzoru*.

```antlr
relational_expression
    : is_pattern_expression
    ;
is_pattern_expression
    : relational_expression 'is' pattern
    ;
```

Tato forma *relational_expression* je kromě existujících formulářů ve C# specifikaci. Jedná se o chybu při kompilaci, pokud *relational_expression* vlevo od `is` tokenu neurčuje hodnotu nebo nemá typ.

Každý *identifikátor* vzoru zavádí novou místní proměnnou, která je *omezena* na základě `true`ho operátoru `is` (tj. *jednoznačně přiřazený při hodnotě true*).

> Poznámka: existují technicky nejednoznačnost mezi *typem* v `is-expression` a *constant_pattern*, z nichž každá může být platným analýzou kvalifikovaného identifikátoru. Pokusíme se vytvořit propojení jako typ pro kompatibilitu s předchozími verzemi jazyka; jenom v případě, že se to nepodaří, vyřešíme ho jako výraz v jiných kontextech na první nalezenou věc (která musí být buď konstanta, nebo typ). Tato nejednoznačnost je k dispozici pouze na pravé straně výrazu `is`.

### <a name="patterns"></a>Vzory

Vzory jsou používány v operátoru *is_pattern* , v *switch_statement*a v *switch_expression* pro vyjádření tvaru dat, proti kterým jsou příchozí data (která volají vstupní hodnotu) porovnána. Vzorce mohou být rekurzivní, aby mohly být části dat porovnány s dílčími vzory.

```antlr
pattern
    : declaration_pattern
    | constant_pattern
    | var_pattern
    | positional_pattern
    | property_pattern
    | discard_pattern
    ;
declaration_pattern
    : type simple_designation
    ;
constant_pattern
    : expression
    ;
var_pattern
    : 'var' designation
    ;
positional_pattern
    : type? '(' subpatterns? ')' property_subpattern? simple_designation?
    ;
subpatterns
    : subpattern
    | subpattern ',' subpatterns
    ;
subpattern
    : pattern
    | identifier ':' pattern
    ;
property_subpattern
    : '{' subpatterns? '}'
    ;
property_pattern
    : type? property_subpattern simple_designation?
    ;
simple_designation
    : single_variable_designation
    | discard_designation
    ;
discard_pattern
    : '_'
    ;
```

#### <a name="declaration-pattern"></a>Vzor deklarace

```antlr
declaration_pattern
    : type simple_designation
    ;
```

*Declaration_pattern* testuje, zda je výraz daný typ, a přetypování na tento typ, pokud je test úspěšný. To může představovat místní proměnnou daného typu pojmenované daným identifikátorem, pokud je označení *single_variable_designation*. Tato lokální proměnná je *jednoznačně přiřazena* , pokud je výsledek operace porovnávání se vzorem `true`.

Sémantika modulu runtime tohoto výrazu je, že testuje typ modulu runtime *relational_expression* operandu na levé straně proti *typu* ve vzoru.  Pokud se jedná o tento typ modulu runtime (nebo některé podtypy) a ne `null`, výsledek `is operator` je `true`.

Určité kombinace statického typu na levé straně a daného typu se považují za nekompatibilní a výsledkem je chyba při kompilaci. Hodnota statického typu `E` je označována jako *kompatibilní se vzorem* typu `T` v případě, že existuje převod identity, implicitní převod odkazu, převod zabalení, explicitní převod odkazu nebo převod rozbalení z `E` na `T`nebo pokud jeden z těchto typů je otevřený typ. Jedná se o chybu při kompilaci, pokud není vstup typu `E` *kompatibilní se vzorem* *typu v rámci* vzoru typu, se kterým se shoduje.

Vzor typu je vhodný pro provádění testů typu odkazu v době běhu a nahrazuje idiom

```csharp
var v = expr as Type;
if (v != null) { // code using v
```

S mírným výstižným

```csharp
if (expr is Type v) { // code using v
```

Jedná se o chybu, pokud je *typ* hodnota s možnou hodnotou null.

Vzor typu lze použít k otestování hodnot typů s možnou hodnotou null: hodnota typu `Nullable<T>` (nebo zabalená `T`) odpovídá vzoru typu `T2 id`, pokud hodnota není null a typ `T2` je `T`nebo některý základní typ nebo rozhraní `T`. Například v fragmentu kódu

```csharp
int? x = 3;
if (x is int v) { // code using v
```

Podmínka příkazu `if` je `true` za běhu a proměnná `v` obsahuje hodnotu `3` typu `int` uvnitř bloku. Po bloku je proměnná `v` v oboru, ale není jednoznačně přiřazená.

#### <a name="constant-pattern"></a>Konstantní vzorek

```antlr
constant_pattern
    : constant_expression
    ;
```

Konstantní vzor testuje hodnotu výrazu s konstantní hodnotou. Konstanta může být libovolný konstantní výraz, jako je například literál, název deklarované `const` proměnné nebo konstanta výčtu. Pokud vstupní hodnota není otevřený typ, konstantní výraz je implicitně převeden na typ odpovídajícího výrazu; Pokud typ vstupní hodnoty není *kompatibilní se vzorem* s typem konstantního výrazu, operace porovnávání se vzorem je chyba.

Vzor *c* se považuje za vyhovující převedenou vstupní hodnotu *e* , pokud `object.Equals(c, e)` vrátí `true`.

Očekáváme, `e is null` jako nejběžnější způsob testování `null` v nově vytvořeném kódu, protože nemůže vyvolat uživatelsky definovanou `operator==`.

#### <a name="var-pattern"></a>Variabilní vzorek

```antlr
var_pattern
    : 'var' designation
    ;
designation
    : simple_designation
    | tuple_designation
    ;
simple_designation
    : single_variable_designation
    | discard_designation
    ;
single_variable_designation
    : identifier
    ;
discard_designation
    : _
    ;
tuple_designation
    : '(' designations? ')'
    ;
designations
    : designation
    | designations ',' designation
    ;
```

Pokud je *označením* *simple_designation*, výraz *e* odpovídá vzoru. Jinými slovy, shoda se *vzorkem var* vždy uspěje s *simple_designation*. Pokud je *simple_designation* *single_variable_designation*, hodnota *e* je vázána na nově zavedenou místní proměnnou. Typ lokální proměnné je statický typ *e*.

Pokud je *označením* *tuple_designation*, pak je vzor ekvivalentní *positional_pattern* formuláře `(var` *označení*,... `)` kde *označení*s se nacházejí v *tuple_designation*.  Například vzor `var (x, (y, z))` je ekvivalentem `(var x, (var y, var z))`.

Pokud se název `var` váže k typu, jedná se o chybu.

#### <a name="discard-pattern"></a>Zahodit vzor

```antlr
discard_pattern
    : '_'
    ;
```

Výraz *e* odpovídá vzorci `_` vždy. Jinými slovy, každý výraz odpovídá vzoru zahození.

Vzor zahození nesmí být použit jako vzor *is_pattern_expression*.

#### <a name="positional-pattern"></a>Poziční vzor

Poziční vzor kontroluje, zda vstupní hodnota není `null`, vyvolá příslušnou metodu `Deconstruct` a provede další porovnávání vzorů pro výsledné hodnoty.  Podporuje také syntaxi vzoru podobné řazené kolekce členů (bez zadání zadaného typu), pokud je typ vstupní hodnoty stejný jako typ obsahující `Deconstruct`, nebo pokud je typ vstupní hodnoty typ řazené kolekce členů, nebo pokud je typ vstupní hodnoty `object` nebo `ITuple` a typ vstupní hodnoty je nebo a běhový typ výrazu implementuje `ITuple`.

```antlr
positional_pattern
    : type? '(' subpatterns? ')' property_subpattern? simple_designation?
    ;
subpatterns
    : subpattern
    | subpattern ',' subpatterns
    ;
subpattern
    : pattern
    | identifier ':' pattern
    ;
```

Pokud je tento *typ* vynechán, převezmeme to jako statický typ vstupní hodnoty.

Vzhledem k hodnotě vstupní hodnoty pro *typ* vzoru `(` *subpattern_list* `)`, je vybrána metoda hledáním v *typu* pro přístupné deklarace `Deconstruct` a výběrem jednoho z nich pomocí stejných pravidel jako u deklarace dekonstrukce.

Jedná se o chybu, pokud *positional_pattern* vynechá typ, má jeden dílčí *vzor* bez *identifikátoru*, nemá žádné *property_subpattern* a neobsahuje žádné *simple_designation*. Tato nejednoznačnost mezi *constant_pattern* , která je v závorkách, a *positional_pattern*.

Aby bylo možné extrahovat hodnoty odpovídající vzorům v seznamu,
- Pokud byl *typ* vynechán a typ vstupní hodnoty je typ řazené kolekce členů, musí být počet dílčích vzorů stejný jako mohutnost řazené kolekce členů. Každý prvek řazené kolekce členů je porovnán s odpovídajícím dílčím *vzorem*a shoda je úspěšná, pokud je vše úspěšné. Pokud má jakýkoliv dílčí *vzor* *identifikátor*, pak musí název prvku řazené kolekce členů na odpovídající pozici v typu řazené kolekce členů.
- V opačném případě, pokud existuje vhodný `Deconstruct` jako člen *typu*, jedná se o chybu při kompilaci, pokud typ vstupní hodnoty není *kompatibilní se vzorem* *typu*. Za běhu je vstupní hodnota testována proti *typu*. Pokud se to nepovede, neproběhne porovnávání pozičního vzoru. V případě úspěchu se vstupní hodnota převede na tento typ a `Deconstruct` je vyvolána s novými proměnnými generovanými kompilátorem pro příjem parametrů `out`. Každá přijatá hodnota je porovnána s odpovídajícím dílčím *vzorem*a shoda bude úspěšná, pokud je vše úspěšné. Pokud má jakýkoliv dílčí *vzor* *identifikátor*, pak musí parametr pojmenovat na odpovídající pozici `Deconstruct`.
- Jinak, pokud byl *typ* vynechán, a vstupní hodnota je typu `object` nebo `ITuple` nebo nějaký typ, který lze převést na `ITuple` implicitním převodem odkazu, a v rámci dílčích vzorů se neobjeví žádný *identifikátor* , bude spárováno s použitím `ITuple`.
- V opačném případě se jedná o chybu při kompilaci.

Pořadí, ve kterém se podvzorci shodují za běhu, není specifikováno a neúspěšná shoda se nemůže pokusit porovnat všechny dílčí vzory.

##### <a name="example"></a>Příklad

Tento příklad používá mnoho funkcí popsaných v této specifikaci.

``` c#
    var newState = (GetState(), action, hasKey) switch {
        (DoorState.Closed, Action.Open, _) => DoorState.Opened,
        (DoorState.Opened, Action.Close, _) => DoorState.Closed,
        (DoorState.Closed, Action.Lock, true) => DoorState.Locked,
        (DoorState.Locked, Action.Unlock, true) => DoorState.Closed,
        (var state, _, _) => state };
```

#### <a name="property-pattern"></a>Vzor vlastnosti

Vzor vlastnosti kontroluje, zda vstupní hodnota není `null` a rekurzivně odpovídá hodnotám extrahovaných pomocí přístupných vlastností nebo polí.

```antlr
property_pattern
    : type? property_subpattern simple_designation?
    ;
property_subpattern
    : '{' '}'
    | '{' subpatterns ','? '}'
    ;
```

Jedná se o chybu, pokud jakýkoli dílčí _vzor_ _property_pattern_ neobsahuje _identifikátor_ (musí být druhý tvar, který má _identifikátor_).  Koncová čárka po posledním podvzoru je volitelná.

Všimněte si, že vzor pro kontrolu hodnoty null nespadají do vzoru triviální vlastnosti. Chcete-li zjistit, zda `s` řetězec není null, můžete napsat libovolný z následujících formulářů

```csharp
if (s is object o) ... // o is of type object
if (s is string x) ... // x is of type string
if (s is {} x) ... // x is of type string
if (s is {}) ...
```

Vzhledem k tomu, že se shoda s výrazem *e* pro *typ* vzoru `{` *property_pattern_list* `}`, jedná se o chybu při kompilaci, pokud výraz *e* není *kompatibilní se vzorem* typu *t* , který je určen *typem*. Pokud tento typ chybí, převezmeme to jako statický typ *e*. Pokud je *identifikátor* přítomen, deklaruje proměnnou *vzoru typu typ.* Každý identifikátor, který se zobrazuje na levé straně *property_pattern_list* , musí určovat přístupnou vlastnost nebo pole *T*, které lze číst. Pokud je *simple_designation* přítomna simple_designation *property_pattern* , definuje proměnnou vzoru typu *T*.

Za běhu je výraz testován proti *T*. Pokud se to nepovede, neproběhne shoda vzoru vlastností a výsledek je `false`. Pokud je to úspěšné, pak je přečteno každé *property_subpattern* pole nebo vlastnost a její hodnota se shoduje s odpovídajícím vzorem. Výsledek celé shody je `false` pouze v případě, že výsledek kteréhokoli z nich je `false`. Pořadí, ve kterém se podvzorci shodují, není zadáno a neúspěšná shoda nesmí odpovídat všem podvzorům za běhu. Je-li shoda úspěšná a *simple_designation* *property_pattern* je *Single_variable_designation*, definuje proměnnou typu *T* , která je přiřazena odpovídající hodnota.

> Poznámka: vzor vlastnosti lze použít pro porovnávání vzorů s anonymními typy.

##### <a name="example"></a>Příklad

```csharp
if (o is string { Length: 5 } s)
```

### <a name="switch-expression"></a>Výraz Switch

K podpoře sémantiky `switch`jako pro kontext výrazu se přidá *switch_expression* .

Syntaxe C# jazyka se rozšiřuje s následujícími syntaktickými výrobou:

```antlr
multiplicative_expression
    : switch_expression
    | multiplicative_expression '*' switch_expression
    | multiplicative_expression '/' switch_expression
    | multiplicative_expression '%' switch_expression
    ;
switch_expression
    : range_expression 'switch' '{' '}'
    | range_expression 'switch' '{' switch_expression_arms ','? '}'
    ;
switch_expression_arms
    : switch_expression_arm
    | switch_expression_arms ',' switch_expression_arm
    ;
switch_expression_arm
    : pattern case_guard? '=>' expression
    ;
case_guard
    : 'when' null_coalescing_expression
    ;
```

*Switch_expression* není povolen jako *expression_statement*.

> Požadavků to v budoucí revizi.

Typ *switch_expression* je [*nejlepší běžný typ*](https://github.com/dotnet/csharplang/blob/master/spec/expressions.md#finding-the-best-common-type-of-a-set-of-expressions) výrazů zobrazený napravo od `=>`ch tokenů *switch_expression_arm*s, pokud takový typ existuje a výraz v každém ARM výrazu Switch lze implicitně převést na tento typ.  Kromě toho přidáme nový *Převod výrazu Switch*, což je předdefinovaný implicitní převod z výrazu Switch na každý typ `T`, pro který existuje implicitní převod z výrazu každého arm na `T`.

Jedná se o chybu, pokud některý vzorek *switch_expression_arm*nemůže ovlivnit výsledek, protože předchozí vzor a ochrana se vždy shodují.

Výraz přepínače je označován jako *vyčerpávající* , pokud některé ARM výrazu Switch zpracovává všechny hodnoty jeho vstupu.  Kompilátor vydá upozornění, pokud výraz přepínače není *vyčerpávající*.

Za běhu je výsledkem *switch_expression* hodnota *výrazu* prvního *switch_expression_arm* , pro který výraz na levé straně *switch_expression* odpovídá vzorci *switch_expression_arm*a který *case_guard* *switch_expression_arm*, pokud je k dispozici, je vyhodnocen jako `true`. Pokud neexistuje žádný takový *switch_expression_arm*, *switch_expression* vyvolá instanci výjimky `System.Runtime.CompilerServices.SwitchExpressionException`.

### <a name="optional-parens-when-switching-on-a-tuple-literal"></a>Volitelné Parens při přepínání na literál řazené kolekce členů

Aby bylo možné přepnout na literál řazené kolekce členů pomocí *switch_statement*, musíte napsat, co vypadá jako redundantní Parens

```csharp
switch ((a, b))
{
```

Povolení

```csharp
switch (a, b)
{
```

kulaté závorky příkazu switch jsou nepovinné, když je výraz přepnutý na literál řazené kolekce členů.

### <a name="order-of-evaluation-in-pattern-matching"></a>Pořadí vyhodnocení při porovnávání vzorů

Poskytnutí flexibility kompilátoru při změně pořadí operací provedených během porovnávání vzorů může umožňovat flexibilitu, kterou lze použít ke zlepšení efektivity porovnávání vzorů. Požadavek (nevykonatelný) by byl takový, že vlastnosti, ke kterým se přistupoval, a metody dekonstrukce musí být "Pure" (vedlejší účinky jsou bezplatné, idempotentní atd.). To neznamená, že by se vám jako koncept jazyka přidala čistota, ale jenom to, že by to mělo být pro kompilátor flexibilita při operacích s přeřazením.

**Řešení 2018-04-04 LDM**: potvrzeno: kompilátor má oprávnění změnit pořadí volání `Deconstruct`, přístup k vlastnostem a vyvolání metod v `ITuple`a může předpokládat, že vrácené hodnoty jsou stejné z více volání. Kompilátor by neměl volat funkce, které nemůžou mít vliv na výsledek, a před provedením jakýchkoli změn v pořadí vygenerovaných kompilátorem vyhodnocování v budoucnu budeme mít pozor.

### <a name="some-possible-optimizations"></a>Některé možné optimalizace

Kompilace porovnávání se vzorci může využít běžné části vzorů. Například pokud je test typu nejvyšší úrovně dvou po sobě jdoucích vzorů v *switch_statement* stejný typ, generovaný kód může přeskočit test typu pro druhý vzor.

Pokud jsou některé ze vzorů celé číslo nebo řetězce, kompilátor může generovat stejný druh kódu, který generuje pro příkaz switch-v dřívějších verzích jazyka.

Další informace o těchto typech optimalizací najdete v článku [[Scott a Ramsey (2000)]](https://www.cs.tufts.edu/~nr/cs257/archive/norman-ramsey/match.pdf "Kdy se shodují s heuristickými heuristickými metodami?").
