---
ms.openlocfilehash: f61039abd6bd557ac0ea625e6aac1c8bafa57b02
ms.sourcegitcommit: 892af9016b3317a8fae12d195014dc38ba51cf16
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/01/2019
ms.locfileid: "71704077"
---
# <a name="expressions"></a>Výrazy

Výraz je sekvence operátorů a operandů. Tato kapitola definuje syntaxi, pořadí vyhodnocování operandů a operátorů a význam výrazů.

## <a name="expression-classifications"></a>Klasifikace výrazů

Výraz je klasifikován jako jeden z následujících:

*  Hodnota. Každá hodnota má přidružený typ.
*  Proměnná. Každá proměnná má přidružený typ, konkrétně deklarovaný typ proměnné.
*  Obor názvů. Výraz s touto klasifikací se může vyskytovat jenom jako levá strana *member_access* ([přístup ke členům](expressions.md#member-access)). V jakémkoli jiném kontextu výraz klasifikovaný jako obor názvů způsobí chybu při kompilaci.
*  Typ. Výraz s touto klasifikací se může vyskytovat jenom jako levá strana *member_access* ([přístup ke členu](expressions.md#member-access)) nebo jako Operand operátoru `as` ([operátor as](expressions.md#the-as-operator)), operátor `is` (operátor[is](expressions.md#the-is-operator)) nebo @no __t-6 – operátor ([operátor typeof](expressions.md#the-typeof-operator)) V jakémkoli jiném kontextu výraz klasifikovaný jako typ způsobí chybu při kompilaci.
*  Skupina metod, která je sadou přetížených metod, která vyplývají z vyhledávání členů ([vyhledávání členů](expressions.md#member-lookup)). Skupina metod může mít přidružený výraz instance a přidružený seznam argumentů typu. Když je vyvolána metoda instance, výsledek vyhodnocení výrazu instance se projeví jako instance reprezentované `this` ([Tento přístup](expressions.md#this-access)). Skupina metod je povolena ve *invocation_expression* ([výrazy vyvolání](expressions.md#invocation-expressions)), *delegate_creation_expression* ([výrazy pro vytváření delegátů](expressions.md#delegate-creation-expressions)) a jako levou stranu operátoru is a může být implicitně. převedeno na kompatibilní typ delegáta ([převody skupin metod](conversions.md#method-group-conversions)). V jakémkoli jiném kontextu výraz klasifikovaný jako skupina metod způsobí chybu při kompilaci.
*  Literál s hodnotou null. Výraz s touto klasifikací se dá implicitně převést na typ odkazu nebo na typ s možnou hodnotou null.
*  Anonymní funkce. Výraz s touto klasifikací lze implicitně převést na kompatibilní typ delegáta nebo strom výrazu.
*  Přístup k vlastnosti. Každý přístup k vlastnosti má přidružený typ, konkrétně typ vlastnosti. Kromě toho může mít přístup k vlastnosti přidružený výraz instance. Když je vyvolán přistupující objekt (blok `get` nebo `set`) vlastnosti instance, výsledek vyhodnocení výrazu instance se projeví jako instance reprezentované `this` ([Tento přístup](expressions.md#this-access)).
*  Přístup k události. Každý přístup k události má přidružený typ, konkrétně typ události. Kromě toho může mít přístup k události přidružený výraz instance. Přístup k události se může zobrazit jako levý operand operátorů `+=` a `-=` ([přiřazení události](expressions.md#event-assignment)). V jakémkoli jiném kontextu výraz klasifikovaný jako přístup k události způsobí chybu při kompilaci.
*  Přístup indexeru Každý přístup k indexeru má přidružený typ, konkrétně typ prvku indexeru. Kromě toho má k přístupu indexeru přidružený výraz instance a seznam přidružených argumentů. Když je vyvolán přistupující objekt (blok `get` nebo `set`) přístupu indexeru, výsledek vyhodnocení výrazu instance se projeví jako instance reprezentované `this` ([Tento přístup](expressions.md#this-access)) a výsledek vyhodnocení seznamu argumentů se projeví jako seznam parametrů vyvolání
*  Žádným. K tomu dochází, když je výraz voláním metody s návratovým typem `void`. Výraz klasifikovaný jako Nothing je platný pouze v kontextu *statement_expression* ([příkazy výrazu](statements.md#expression-statements)).

Konečný výsledek výrazu není nikdy obor názvů, typ, skupina metod ani přístup k události. Namísto toho, jak je uvedeno výše, jsou tyto kategorie výrazů zprostředkující konstrukce, které jsou povoleny pouze v určitých kontextech.

Přístup k vlastnosti nebo k indexeru je vždy překlasifikován jako hodnota prostřednictvím vyvolání *přístupového objektu Get* nebo přístupového *objektu set*. Konkrétní přistupující objekt je určen kontextem vlastnosti nebo přístupu indexeru: Pokud je přístup cílem přiřazení, vyvolá se *přistupující objekt set* , který přiřadí novou hodnotu ([jednoduché přiřazení](expressions.md#simple-assignment)). V opačném případě se vyvolá *přístupový objekt get* , který získá aktuální hodnotu ([hodnoty výrazů](expressions.md#values-of-expressions)).

### <a name="values-of-expressions"></a>Hodnoty výrazů

Většina konstrukcí, které zahrnují výraz, nakonec vyžadují výraz k označení ***hodnoty***. V takových případech, pokud skutečný výraz označuje obor názvů, typ, skupinu metod nebo nic, dojde k chybě při kompilaci. Nicméně pokud výraz označuje přístup k vlastnosti, přístup indexeru nebo proměnnou, hodnota vlastnosti, indexeru nebo proměnné je implicitně nahrazena:

*  Hodnota proměnné je jednoduše hodnota, která je aktuálně uložena v umístění úložiště identifikovaném proměnnou. Proměnná musí být považována za jednoznačně přiřazenou ([jednoznačné přiřazení](variables.md#definite-assignment)), než se hodnota Získá nebo v opačném případě dojde k chybě při kompilaci.
*  Hodnota výrazu přístupu k vlastnosti je získána vyvoláním *přístupového objektu Get* vlastnosti. Pokud vlastnost nemá žádný *přistupující objekt get*, dojde k chybě při kompilaci. V opačném případě se provede vyvolání člena funkce ([Kontrola dynamického přetěžování při kompilaci](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) a výsledek vyvolání se stává hodnotou výrazu přístupu k vlastnosti.
*  Hodnota výrazu přístupu indexeru se získá vyvoláním *přístupové metody Get* indexeru. Pokud indexer nemá *přístup k Get*, dojde k chybě při kompilaci. V opačném případě je provedeno vyvolání člena funkce ([Kontrola dynamického přetěžování při kompilaci](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) se seznamem argumentů přidruženým k výrazu přístupu indexeru a výsledek vyvolání se stává hodnotou přístupu indexeru. vyjádření.

## <a name="static-and-dynamic-binding"></a>Statická a dynamická vazba

Proces určení významu operace v závislosti na typu nebo hodnotě výrazů prvků (argumenty, operandy, přijímače) se často označují jako ***vazby***. Například význam volání metody je určen podle typu přijímače a argumentů. Význam operátoru je určen na základě typu jeho operandů.

Ve C# smyslu operace je obvykle určena v době kompilace na základě typu za kompilace svých základních výrazů. Podobně pokud výraz obsahuje chybu, je zjištěna chyba a hlášena kompilátorem. Tento přístup je známý jako ***statická vazba***.

Nicméně pokud je výraz dynamický výraz (tj. má typ `dynamic`), znamená to, že jakákoli vazba, na kterou se podílí, by měla být založena na typu běhu (tj. skutečný typ objektu, který označuje za běhu), a nikoli typu, v němž má čas kompilace. Vazba takové operace je proto odložena až do doby, kdy má být operace provedena během chodu programu. Tento postup se označuje jako ***dynamická vazba***.

Je-li operace dynamicky svázána, je kompilátorem provedena pouze malá nebo žádná kontrola. Místo toho, pokud se vazba za běhu nezdařila, jsou chyby hlášeny jako výjimky za běhu.

Následující operace v C# nástroji jsou předmětem vazby:

*  Přístup ke členu: `e.M`
*  Vyvolání metody: `e.M(e1, ..., eN)`
*  Volání delegáta: `e(e1, ..., eN)`
*  Přístup k elementu: `e[e1, ..., eN]`
*  Vytvoření objektu: `new C(e1, ..., eN)`
*  Přetížené unární operátory: `+`, `-`, `!`, `~`, `++`, `--`, `true`, `false`
*  Přetížené binární operátory: `+`, `-`, `*`, `/`, `%`, `&`, `&&`, `|`, `||`, `??`, 1 @no__t 2, 3, 4, 6, 7, 8
*  Operátory přiřazení: `=`, `+=`, `-=`, `*=`, `/=`, `%=`, `&=`, `|=`, `^=`, `<<=`, 0
*  Implicitní a explicitní převody

Pokud nejsou zapojeny žádné dynamické výrazy C# , výchozí hodnota je statická vazba, což znamená, že jsou v procesu výběru použity typy výrazů v čase kompilace. Pokud je však jeden ze základních výrazů v operacích uvedených výše dynamickým výrazem, operace je místo toho dynamicky svázána.

### <a name="binding-time"></a>Čas vazby

Statická vazba probíhá v době kompilace, zatímco v době běhu probíhá dynamická vazba. V následujících částech pojem ***Doba vazby*** odkazuje buď na čas kompilace, nebo za běhu v závislosti na tom, kdy dojde k vazbě.

Následující příklad znázorňuje pojem statické a dynamické vazby a dobu vazby:
```csharp
object  o = 5;
dynamic d = 5;

Console.WriteLine(5);  // static  binding to Console.WriteLine(int)
Console.WriteLine(o);  // static  binding to Console.WriteLine(object)
Console.WriteLine(d);  // dynamic binding to Console.WriteLine(int)
```

První dvě volání jsou staticky svázána: přetížení `Console.WriteLine` je vybráno na základě typu kompilace jejich argumentu. Proto je čas vytvoření vazby v čase kompilace.

Třetí volání je dynamicky svázáno: přetížení `Console.WriteLine` je vybráno na základě běhového typu argumentu. K tomu dochází, protože argument je dynamický výraz – jeho typ při kompilaci je `dynamic`. Proto je čas vazby za třetí volání za běhu.

### <a name="dynamic-binding"></a>Dynamická vazba

Účelem dynamické vazby je povolit C# programům pracovat s ***dynamickými objekty***, tj. objekty, které nedodržují normální pravidla systému C# typů. Dynamické objekty mohou být objekty z jiných programovacích jazyků s různými systémy typů, nebo mohou být objekty, které jsou programově nastaveny k implementaci sémantiky vytváření vazeb pro různé operace.

Mechanismus, kterým dynamický objekt implementuje svou vlastní sémantiku je definována implementace. Dané rozhraní zadané pro implementaci je znovu definováno – je implementováno dynamickými objekty k signalizaci za C# běhu, které mají speciální sémantiku. Proto vždy, když jsou operace s dynamickým objektem dynamicky svázány, jejich vlastní sémantika vazby namísto těch C# , které jsou zadány v tomto dokumentu, převezmou.

I když je účel dynamické vazby umožněna spolupráce s dynamickými objekty, C# umožňuje dynamickou vazbu na všech objektech, bez ohledu na to, zda jsou dynamické. To umožňuje plynulou integraci dynamických objektů, protože výsledky operací na nich nemusejí být dynamické objekty, ale v době kompilace jsou stále typu, který není známý pro programátory. Dynamická vazba také může přispět k eliminaci kódu založeného na reflexi, i když žádné nesouvisející objekty nejsou dynamické objekty.

Následující oddíly popisují jednotlivé konstrukce v jazyce přesně tehdy, když je aplikována dynamická vazba, která kontrola doby kompilace – Pokud je použita libovolná a jaké je výsledek kompilace a klasifikace výrazu.

### <a name="types-of-constituent-expressions"></a>Typy výrazů prvků

Je-li operace staticky svázána, typ výrazu elementu (například přijímač, argument, index nebo operand) je vždy považován za typ doby kompilace tohoto výrazu.

Je-li operace dynamicky svázána, je typ výrazu elementu určován různými způsoby v závislosti na typu při kompilaci výrazu prvku:

*  Výraz prvku typu kompilace `dynamic` se považuje za typ skutečné hodnoty, na kterou se výraz vyhodnotí za běhu.
*  Výraz prvku, jehož typ kompilace je, se považuje za parametr typu, který má typ, ke kterému je vázán parametr typu za běhu.
*  V opačném případě je výraz prvku považován za jeho typ při kompilaci.

## <a name="operators"></a>Operátory

Výrazy jsou vytvořené z ***operandů*** a ***operátorů***. Operátory výrazu označují, které operace se mají použít u operandů. Příklady operátorů zahrnují `+`, `-`, `*` `/`, a .`new` Příklady operandů zahrnují literály, pole, místní proměnné a výrazy.

Existují tři typy operátorů:

*  Unární operátory. Unární operátory přebírají jeden operand a používají buď zápis předpony (například `--x`) nebo notaci přípony (například `x++`).
*  Binární operátory. Binární operátory přebírají dva operandy a používají zápis vpony (například `x + y`).
*  Ternární operátor Pouze jeden Ternární operátor, `?:`, existuje; využívá tři operandy a používá notaci vpony (`c ? x : y`).

Pořadí vyhodnocení operátorů ve výrazu je určeno ***prioritou*** a ***asociativita*** operátorů ([Priorita operátorů a asociativita](expressions.md#operator-precedence-and-associativity)).

Operandy ve výrazu jsou vyhodnocovány zleva doprava. Například v `F(i) + G(i++) * H(i)` je metoda `F` volána pomocí staré hodnoty `i`, pak metoda `G` je volána se starou hodnotou `i` a nakonec metoda `H` je volána s novou hodnotou `i`. To je oddělené od a s prioritou operátoru.

Některé operátory mohou být ***přetíženy***. Přetížení operátoru umožňuje zadat uživatelsky definované implementace operátorů pro operace, kde jeden nebo oba operandy jsou uživatelsky definované třídy nebo typu struktury ([Přetěžování operátorů](expressions.md#operator-overloading)).

### <a name="operator-precedence-and-associativity"></a>Priorita operátorů a asociativita

Pokud výraz obsahuje více operátorů, ***Priorita*** operátorů řídí pořadí, ve kterém jsou jednotlivé operátory vyhodnocovány. Například výraz `x + y * z` je vyhodnocen jako `x + (y * z)`, protože operátor `*` má vyšší prioritu než binární operátor `+`. Priorita operátoru je založena na definici své související provozní gramatiky. Například *additive_expression* se skládá z sekvence *multiplicative_expression*s oddělenými operátory `+` nebo `-`, takže operátory `+` a `-` budou mít nižší prioritu než `*` `/` a @no__ operátory t-8.

Následující tabulka shrnuje všechny operátory v pořadí podle priority od nejvyšších po nejnižší:

| __Section__                                                                                   | __Kategorie__                | __Operátory__ | 
|-----------------------------------------------------------------------------------------------|-----------------------------|---------------|
| [Primární výrazy](expressions.md#primary-expressions)                                     | Primární                     | `x.y`  `f(x)`  `a[x]`  `x++`  `x--`  `new`  `typeof`  `default`  `checked`  `unchecked`  `delegate` | 
| [Unární operátory](expressions.md#unary-operators)                                             | Unární                       | `+`  `-`  `!`  `~`  `++x`  `--x`  `(T)x` | 
| [Aritmetické operátory](expressions.md#arithmetic-operators)                                   | Multiplikativní              | `*`  `/`  `%` | 
| [Aritmetické operátory](expressions.md#arithmetic-operators)                                   | Přičítáním                    | `+`  `-`      | 
| [Operátory posunutí](expressions.md#shift-operators)                                             | SHIFT                       | `<<`  `>>`    | 
| [Relační operátory a operátory testování typů](expressions.md#relational-and-type-testing-operators) | Relační testování a testování typu | `<`  `>`  `<=`  `>=`  `is`  `as` | 
| [Relační operátory a operátory testování typů](expressions.md#relational-and-type-testing-operators) | Rovnost                    | `==`  `!=`    | 
| [Logické operátory](expressions.md#logical-operators)                                         | Logický operátor AND                 | `&`           | 
| [Logické operátory](expressions.md#logical-operators)                                         | Logický operátor XOR                 | `^`           | 
| [Logické operátory](expressions.md#logical-operators)                                         | Logický operátor OR                  | <code>&#124;</code>           |
| [Podmíněné logické operátory](expressions.md#conditional-logical-operators)                 | Podmiňovací operátor AND             | `&&`          | 
| [Podmíněné logické operátory](expressions.md#conditional-logical-operators)                 | Podmiňovací operátor OR              | <code>&#124;&#124;</code>          | 
| [Operátor slučování s hodnotou null](expressions.md#the-null-coalescing-operator)                   | Nulové sloučení             | `??`          | 
| [Podmíněný operátor](expressions.md#conditional-operator)                                   | Podmiňovací operátor                 | `?:`          | 
| [Operátory přiřazení](expressions.md#assignment-operators), [anonymní výrazy funkcí](expressions.md#anonymous-function-expressions)  | Přiřazení a výraz lambda | `=`  `*=`  `/=`  `%=`  `+=`  `-=`  `<<=`  `>>=`  `&=`  `^=`  <code>&#124;=</code>  `=>` | 

Když dojde k operandu mezi dvěma operátory se stejnou prioritou, asociativita operátor řídí pořadí, ve kterém jsou operace prováděny:

*  S výjimkou operátorů přiřazení a slučovacího operátoru null jsou všechny binární operátory ***asociativní***, což znamená, že operace jsou prováděny zleva doprava. Například `x + y + z` je vyhodnocen jako `(x + y) + z`.
*  Operátory přiřazení, operátor sloučení null a podmíněný operátor (`?:`) jsou ***asociativní zprava***, což znamená, že operace jsou prováděny zprava doleva. Například `x = y = z` je vyhodnocen jako `x = (y = z)`.

Priority a asociativita lze ovládat pomocí závorek. Například `x + y * z` nejprve vynásobí `y` hodnotou `z` a následně výsledek přidá do `x`, ale `(x + y) * z` nejprve přidá `x` a `y` a pak výsledek vynásobí `z`.

### <a name="operator-overloading"></a>Přetěžování operátoru

Všechny unární a binární operátory mají předdefinované implementace, které jsou automaticky k dispozici v jakémkoli výrazu. Kromě předdefinovaných implementací lze uživatelsky definované implementace začlenit zahrnutím `operator` deklarací do tříd a struktur ([Operators](classes.md#operators)). Uživatelsky definované implementace operátoru vždycky mají přednost před předdefinovanými implementacemi operátorů: Pouze v případě, že neexistují žádné použitelné uživatelsky definované implementace operátoru, budou použity předdefinované implementace operátoru, jak je popsáno v tématu [řešení přetížení unárního operátoru](expressions.md#unary-operator-overload-resolution) a [rozlišení přetížení binárního operátoru](expressions.md#binary-operator-overload-resolution).

***Přetížené unární operátory*** jsou:
```csharp
+   -   !   ~   ++   --   true   false
```

I když `true` a `false` se ve výrazech nepoužívají explicitně (a proto nejsou zahrnuty v tabulce priorit v [prioritě operátorů a asociativita](expressions.md#operator-precedence-and-associativity)), považují se za operátory, protože jsou vyvolány v několika výrazech. kontexty: logické výrazy ([logické výrazy](expressions.md#boolean-expressions)) a výrazy týkající se podmíněného ([podmíněného](expressions.md#conditional-operator)) a podmíněných logických operátorů ([podmíněných logických](expressions.md#conditional-logical-operators)operátorů).

***Přetížené binární operátory*** jsou:
```csharp
+   -   *   /   %   &   |   ^   <<   >>   ==   !=   >   <   >=   <=
```

Pouze operátory uvedené výše mohou být přetíženy. Konkrétně není možné přetížit přístup ke členům, vyvolání metody ani `=`, `&&` `||`, `??`, `?:`, `=>`, `checked`, `unchecked`, `new`, `typeof`, 0, 1 a 2.

Při přetížení binárního operátoru je odpovídající operátor přiřazení také implicitně přetížený. Například přetížení operátoru `*` je také přetížením operátoru `*=`. Tento postup je podrobněji popsán v tématu [složené přiřazení](expressions.md#compound-assignment). Všimněte si, že samotný operátor přiřazení (`=`) nemůže být přetížený. Přiřazení vždy provádí jednoduchou bitovou kopii hodnoty do proměnné.

Operace přetypování, jako je například `(T)x`, jsou přetíženy poskytnutím uživatelsky definovaných převodů ([uživatelem definované převody](conversions.md#user-defined-conversions)).

Přístup k prvkům, jako je například `a[x]`, není považován za přetížený operátor. Místo toho je uživatelsky definované indexování podporováno prostřednictvím indexerů ([indexerů](classes.md#indexers)).

Ve výrazech jsou operátory odkazovány pomocí zápisu operátorů a v deklaracích jsou operátory odkazovány pomocí funkčního zápisu. Následující tabulka ukazuje vztah mezi operátorem a funkčními zápisy pro unární a binární operátory. V první položce *op* označuje jakýkoli přetížený operátor unární předpony. Ve druhé položce *op* označuje unární příponu `++` a `--`. Třetí položka *op* označuje jakýkoli přetížený binární operátor.


| __Zápis operátoru__ | __Funkční zápis__ |
|-----------------------|-------------------------|
| `op x`                | `operator op(x)`        | 
| `x op`                | `operator op(x)`        | 
| `x op y`              | `operator op(x,y)`      | 

Deklarace operátoru definované uživatelem vždy vyžadují, aby alespoň jeden z parametrů byl typu třídy nebo struktury, který obsahuje deklaraci operátoru. Proto není možné, aby uživatelem definovaný operátor měl stejnou signaturu jako předdefinovaný operátor.

Deklarace uživatelem definovaných operátorů nemůže měnit syntaxi, prioritu ani asociativita operátoru. Například operátor `/` je vždy binárním operátorem, vždy má úroveň priority specifikovanou v [prioritě operátorů a asociativita](expressions.md#operator-precedence-and-associativity)a je vždy asociativní zprava.

I když je možné, aby uživatelsky definovaný operátor prováděl jakékoli výpočty, implementace, které vytváří jiné výsledky než ty, které jsou intuitivní, se důrazně nedoporučuje. Například implementace `operator ==` by měla porovnat dva operandy pro rovnost a vracet odpovídající `bool` výsledek.

Popisy jednotlivých operátorů v [primárních výrazech](expressions.md#primary-expressions) prostřednictvím [podmíněných logických operátorů](expressions.md#conditional-logical-operators) určují předdefinované implementace operátorů a všechna další pravidla, která platí pro každý operátor. Popisy využívají výrazy ***unárního přetížení operátoru***, ***rozlišení přetížení binárního operátoru***a ***číselnou povýšení***, definice, které se nacházejí v následujících částech.

### <a name="unary-operator-overload-resolution"></a>Unární rozlišení přetížení operátoru

Operace formuláře `op x` nebo `x op`, kde `op` je přetížený unární operátor a `x` je výraz typu `X`, zpracován následujícím způsobem:

*  Sada kandidátů definovaných uživatelem, které poskytuje `X` pro operaci `operator op(x)`, je určena pomocí pravidel [kandidátů definovaných uživatelem](expressions.md#candidate-user-defined-operators).
*  Pokud sada kandidátů definovaných uživatelem není prázdná, pak se jedná o sadu kandidátických operátorů pro operaci. V opačném případě se předdefinovaná unární implementace `operator op`, včetně jejich vypadlých formulářů, stanou sadou kandidátů pro operaci. Předdefinované implementace daného operátoru jsou zadány v popisu operátoru ([primární výrazy](expressions.md#primary-expressions) a [unární operátory](expressions.md#unary-operators)).
*  Pravidla rozlišení přetížení [rozlišení přetěžování](expressions.md#overload-resolution) jsou aplikována na sadu kandidátních operátorů pro výběr nejlepšího operátoru s ohledem na seznam argumentů `(x)`. Tento operátor se projeví v důsledku procesu rozlišení přetížení. Pokud Rozlišení přetěžování nedokáže vybrat jeden nejlepší operátor, dojde k chybě při vazbě.

### <a name="binary-operator-overload-resolution"></a>Rozlišení přetížení binárního operátoru

Operace formuláře `x op y`, kde `op` je přetížený binární operátor, `x` je výraz typu `X` a `y` je výraz typu `Y`, zpracován následujícím způsobem:

*  Je určena sada kandidátů definovaných uživatelem, které poskytuje `X` a `Y` pro operaci `operator op(x,y)`. Sada se skládá z sjednocení operátorů kandidátů, které poskytuje `X` a kandidátské operátory poskytované `Y`, z nichž každá je určena pomocí pravidel [operátorů kandidáta definovaných uživatelem](expressions.md#candidate-user-defined-operators). Pokud jsou `X` a `Y` stejného typu, nebo pokud `X` a `Y` jsou odvozeny ze společného základního typu, pak jsou sdílené kandidáty operátory provedeny pouze v kombinované sadě.
*  Pokud sada kandidátů definovaných uživatelem není prázdná, pak se jedná o sadu kandidátických operátorů pro operaci. V opačném případě se předdefinovaná binární implementace `operator op`, včetně jejich vypadlých formulářů, stane sadou kandidátů pro tuto operaci. Předdefinované implementace daného operátoru jsou zadány v popisu operátoru ([aritmetické operátory](expressions.md#arithmetic-operators) prostřednictvím [podmíněných logických operátorů](expressions.md#conditional-logical-operators)). U předdefinovaných operátorů výčtu a delegátů jsou považovány za ty, které jsou definovány výčtovým typem nebo typem delegáta, který je typem pro dobu vazby jednoho z operandů.
*  Pravidla rozlišení přetížení [rozlišení přetěžování](expressions.md#overload-resolution) jsou aplikována na sadu kandidátních operátorů pro výběr nejlepšího operátoru s ohledem na seznam argumentů `(x,y)`. Tento operátor se projeví v důsledku procesu rozlišení přetížení. Pokud Rozlišení přetěžování nedokáže vybrat jeden nejlepší operátor, dojde k chybě při vazbě.

### <a name="candidate-user-defined-operators"></a>Kandidátské operátory definované uživatelem

Při zadání typu `T` a operace `operator op(A)`, kde `op` je přetížený operátor a `A` je seznam argumentů, sada kandidátů definovaných uživatelem, kterou poskytují `T` pro `operator op(A)`, je určena následujícím způsobem:

*  Určete typ `T0`. Pokud je `T` typem s možnou hodnotou null, `T0` je jeho nadřízený typ, jinak `T0` se rovná `T`.
*  U všech deklarací `operator op` v `T0` a všech vydaných formách takových operátorů platí, že pokud je použit alespoň jeden operátor ([platný člen funkce](expressions.md#applicable-function-member)) s ohledem na seznam argumentů `A`, pak sada kandidátů, která se skládá ze všech takových příslušné operátory v `T0`.
*  V opačném případě, pokud je `T0` `object`, sada kandidát Operators je prázdná.
*  Jinak sada kandidátů, kterou poskytuje `T0`, je sada kandidátů, kterou poskytuje přímá základní třída `T0` nebo účinná základní třída `T0`, pokud `T0` je parametr typu.

### <a name="numeric-promotions"></a>Číselné propagační akce

Číselná propagace se skládá z automatického provádění určitých implicitních převodů operandů předdefinovaných unárních a binárních číselných operátorů. Číselná propagace není odlišným mechanismem, ale místo toho, aby se použilo Rozlišení přetěžování na předdefinované operátory. Číselná propagace konkrétně neovlivňuje hodnocení uživatelsky definovaných operátorů, i když uživatelsky definované operátory mohou být implementovány tak, aby se projevily podobné účinky.

Jako příklad číselné povýšení zvažte předdefinované implementace binárního operátoru `*`:

```csharp
int operator *(int x, int y);
uint operator *(uint x, uint y);
long operator *(long x, long y);
ulong operator *(ulong x, ulong y);
float operator *(float x, float y);
double operator *(double x, double y);
decimal operator *(decimal x, decimal y);
```

Když se pro tuto sadu operátorů aplikují pravidla rozlišení přetížení ([rozlišení přetížení](expressions.md#overload-resolution)), efekt je vybrat první z operátorů, pro které existují implicitní převody z typů operandů. Například pro operaci `b * s`, kde `b` je `byte` a `s` je `short`, řešení přetížení vybere `operator *(int,int)` jako nejlepší operátor. Proto platí, že `b` a `s` jsou převedeny na `int` a typ výsledku je `int`. Podobně pro operaci `i * d`, kde `i` je `int` a `d` je `double`, řešení přetížení vybere `operator *(double,double)` jako nejlepší operátor.

#### <a name="unary-numeric-promotions"></a>Unární číselné propagační akce

Unární číselná propagace nastane pro operandy unárních operátorů `+`, `-` a `~`. Unární číselná propagace jednoduše sestává z převodu operandů typu `sbyte`, `byte`, `short`, `ushort` nebo `char` na typ `int`. Pro unární operátor `-` navíc unární číselná propagace převádí operandy typu `uint` na typ `long`.

#### <a name="binary-numeric-promotions"></a>Binární číselné propagační akce

Pro operandy předdefinovaných `+`, `-`, `*`, `/`, `%`, `&`, `|`, `^`, `==`, `!=`, 0, 1, 2 a 3 binárních operátorů dojde k binárnímu navýšení číselného povýšení. Binární číselná propagace implicitně převede oba operandy na společný typ, který, v případě nerelačních operátorů, se také změní na výsledný typ operace. Binární číselná propagace se skládá z použití následujících pravidel v pořadí, v jakém se zobrazují:

*  Pokud je operand typu `decimal`, je druhý operand převeden na typ `decimal` nebo dojde k chybě při vazbě v případě, že druhý operand je typu `float` nebo `double`.
*  V opačném případě, pokud je jeden operand typu `double`, je druhý operand převeden na typ `double`.
*  V opačném případě, pokud je jeden operand typu `float`, je druhý operand převeden na typ `float`.
*  V opačném případě, pokud je jeden operand typu `ulong`, je druhý operand převeden na typ `ulong` nebo dojde k chybě při vazbě, pokud je druhý operand typu `sbyte`, `short`, `int` nebo `long`.
*  V opačném případě, pokud je jeden operand typu `long`, je druhý operand převeden na typ `long`.
*  V opačném případě, pokud je jeden operand typu `uint` a druhý operand je typu `sbyte`, `short` nebo `int`, jsou oba operandy převedeny na typ `long`.
*  V opačném případě, pokud je jeden operand typu `uint`, je druhý operand převeden na typ `uint`.
*  V opačném případě jsou oba operandy převedeny na typ `int`.

Všimněte si, že první pravidlo zakáže všechny operace, které budou kombinovat typ `decimal` s typy `double` a `float`. Pravidlo následuje ze skutečnosti, že neexistují žádné implicitní převody mezi typem `decimal` a typy `double` a `float`.

Všimněte si také, že není možné, aby operand byl typu `ulong`, pokud je druhý operand typu se znaménkem integrálu. Důvodem je, že neexistuje žádný integrální typ, který by mohl představovat plný rozsah `ulong`, jakož i podepsaný integrální typ.

V obou výše uvedených případech lze výraz přetypování použít k explicitnímu převodu jednoho operandu na typ, který je kompatibilní s druhým operandem.

V příkladu
```csharp
decimal AddPercent(decimal x, double percent) {
    return x * (1.0 + percent / 100.0);
}
```
dojde k chybě při vazbě, protože `decimal` nelze vynásobit `double`. Chybu lze vyřešit explicitním převodem druhého operandu na `decimal`, a to následujícím způsobem:

```csharp
decimal AddPercent(decimal x, double percent) {
    return x * (decimal)(1.0 + percent / 100.0);
}
```

### <a name="lifted-operators"></a>Zrušené operátory

Přenesené ***operátory*** povolují předdefinované a uživatelsky definované operátory, které pracují s typy hodnot, které neumožňují hodnotu null, pro použití s připouštějící formuláře těchto typů. Přenesené operátory jsou vyrobeny z předdefinovaných a uživatelem definovaných operátorů, které splňují určité požadavky, jak je popsáno v následujícím tématu:

*   Pro unární operátory

    ```csharp
    +  ++  -  --  !  ~
    ```

    Vyzdvižený tvar operátoru existuje, pokud operandy a typy výsledků jsou typy hodnot bez hodnoty null. Převedený formulář je vytvořen přidáním jednoho modifikátoru `?` do typu operand a výsledek. Vyzdvižený operátor vytvoří hodnotu null, pokud je operand null. V opačném případě převedený operátor zruší zalomení operandu, použije základní operátor a zabalí výsledek.

*   Pro binární operátory

    ```csharp
    +  -  *  /  %  &  |  ^  <<  >>
    ```

    Vyzdvižený tvar operátoru existuje, pokud jsou typy operandů a výsledků všechny typy hodnot bez hodnoty null. Vyzdvižený formulář je vytvořen přidáním jednoho modifikátoru `?` do každého operandu a typu výsledku. Převedený operátor vytvoří hodnotu null, pokud je jeden nebo oba operandy NULL (výjimkou jsou operátory `&` a `|` typu `bool?`, jak je popsáno v tématu [logické logické operátory](expressions.md#boolean-logical-operators)). V opačném případě Vyzdvižený operátor rozbalí operandy, použije základní operátor a zabalí výsledek.

*   Pro operátory rovnosti

    ```csharp
    ==  !=
    ```

    Vyzdvižený tvar operátoru existuje, pokud typy operandů jsou typy hodnot bez hodnoty null a je-li typ výsledku `bool`. Vyzdvižený formulář je vytvořen přidáním jednoho modifikátoru `?` do každého typu operandu. Předaný operátor považuje dvě hodnoty null za stejné a hodnota null se nerovná žádné hodnotě, která není null. Pokud jsou oba operandy nenulové, Vyzdvižený operátor rozbalí operandy a aplikuje základní operátor k výrobě výsledku `bool`.

*   Pro relační operátory

    ```csharp
    <  >  <=  >=
    ```

    Vyzdvižený tvar operátoru existuje, pokud typy operandů jsou typy hodnot bez hodnoty null a je-li typ výsledku `bool`. Vyzdvižený formulář je vytvořen přidáním jednoho modifikátoru `?` do každého typu operandu. Převedený operátor vytvoří hodnotu `false`, pokud je jeden nebo oba operandy NULL. V opačném případě převedený operátor rozbalí operandy a aplikuje příslušný operátor, aby vytvořil výsledek `bool`.

## <a name="member-lookup"></a>Vyhledávání členů

Vyhledávání členů je proces, při kterém je určen význam názvu v kontextu typu. Vyhledávání členů může nastat jako součást vyhodnocení *simple_name* ([jednoduchých názvů](expressions.md#simple-names)) nebo *member_access* ([přístupu ke členu](expressions.md#member-access)) ve výrazu. Pokud k *simple_name* nebo *member_access* dojde jako *primary_expression* *invocation_expression* ([vyvolání metod](expressions.md#method-invocations)), člen je označován jako vyvolán.

Pokud je členem metoda nebo událost, nebo pokud se jedná o konstantu, pole nebo vlastnost buď typu delegáta ([Delegáti](delegates.md)), nebo typu `dynamic` ([dynamický typ](types.md#the-dynamic-type)), pak je člen označován jako *nevyvolatelný*.

Vyhledávání členů nebere v úvahu nejen název členu, ale také počet parametrů typu, které má člen a zda je člen přístupný. Pro účely vyhledávání členů mají obecné metody a vnořené obecné typy počet parametrů typu uvedených v příslušných deklaracích a všichni ostatní členové mají nulové parametry typu.

Vyhledávání členů názvu @ no__t-0 s parametry `K` @ no__t-2type v typu @ no__t-3 je zpracováno následujícím způsobem:

*  Nejprve je určena sada přístupných členů s názvem @ no__t-0:
    * Pokud je parametrem typu `T`, pak je množina sjednocení sad přístupných členů s názvem @ no__t-1 v každém z typů určených jako primární omezení nebo sekundární omezení ([omezení parametrů typu](classes.md#type-parameter-constraints)) pro @ no__t-3 společně se sadou přístupní členové s názvem @ no__t-4 v `object`.
    * V opačném případě se sada skládá ze všech přístupných členů ([přístupu členů](basic-concepts.md#member-access)) s názvem @ no__t-1 v @ no__t-2, včetně zděděných členů a přístupných členů s názvem @ no__t-3 v `object`. Pokud je `T` konstruovaný typ, sada členů je získána nahrazením argumentů typu, jak je popsáno v tématu [Členové konstruovaných typů](classes.md#members-of-constructed-types). Členy, které obsahují modifikátor `override`, jsou vyloučeny ze sady.
*  Pokud je `K` nula, všechny vnořené typy, jejichž deklarace zahrnují parametry typu, se odeberou. Pokud `K` není nula, budou odebrány všechny členy s jiným počtem parametrů typu. Všimněte si, že pokud `K` je nula, metody s parametry typu nejsou odebrány, protože proces odvození typu ([odvození typu](expressions.md#type-inference)) může být schopný odvodit argumenty typu.
*  Dále, pokud je člen *vyvolán*, všechny*nenevyvolatelný* členové budou ze sady odebrány.
*  Dále jsou členové, kteří jsou skryti jinými členy, ze sady odebrány. Pro každý člen `S.M` v sadě, kde `S` je typ, ve kterém je deklarován člen @ no__t-2, jsou použita následující pravidla:
    * Pokud je `M` konstantou, polem, vlastností, událostí nebo výčtovým členem, pak jsou ze sady odebrány všechny členy deklarované v základním typu `S`.
    * Pokud je `M` deklarace typu, pak všechny netypy deklarované v základním typu `S` jsou ze sady odebrány a všechny deklarace typu se stejným počtem parametrů typu jako `M` deklarované v základním typu `S` jsou ze sady odebrány.
    * Pokud je metoda `M`, pak všechny členy, které nejsou deklarovány v základním typu `S`, budou ze sady odebrány.
*  Dále jsou členy rozhraní skryté ze sady odebrány. Tento krok má efekt pouze v případě, že `T` je parametr typu a `T` má platnou základní třídu kromě `object` a neprázdnou sadu platných rozhraní ([omezení parametrů typu](classes.md#type-parameter-constraints)). Pro každý členský `S.M` v sadě, kde `S` je typ, ve kterém je deklarován člen `M`, jsou použita následující pravidla, pokud `S` je deklarace třídy jiná než `object`:
    * Pokud `M` je konstanta, pole, vlastnost, událost, člen výčtu nebo deklarace typu, pak všechny členy deklarované v deklaraci rozhraní jsou ze sady odebrány.
    * Pokud je metoda `M`, pak všechny členy, které nejsou deklarovány v deklaraci rozhraní, jsou ze sady odebrány a všechny metody se stejným podpisem jako `M` deklarované v deklaraci rozhraní jsou ze sady odebrány.
*  Nakonec, po odebrání skrytých členů, se určí výsledek vyhledávání:
    * Pokud se sada skládá z jednoho člena, který není metodou, pak je tento člen výsledkem vyhledávání.
    * V opačném případě, pokud sada obsahuje pouze metody, pak je tato skupina metod výsledkem vyhledávání.
    * V opačném případě je vyhledávání dvojznačné a dojde k chybě při vazbě.

Pro vyhledávání členů v jiných typech než parametry typu a rozhraní a vyhledávání členů v rozhraních, která jsou výhradně jedinou dědičností (každé rozhraní v řetězu dědičnosti má přesně nula nebo jedno přímé základní rozhraní), je efekt vyhledávacích pravidel pouze odvoditelné členy skryjí základní členy se stejným názvem nebo signaturou. Tato vyhledávání s jednou dědičností nejsou nikdy dvojznačná. Nejednoznačnosti, které mohou nastat při hledání členů v rozhraních vícenásobné dědičnosti, jsou popsány v tématu [přístup ke členu rozhraní](interfaces.md#interface-member-access).

### <a name="base-types"></a>Základní typy

Pro účely vyhledávání členů je typ `T` považován za následující základní typy:

*  Pokud je `T` `object`, pak `T` nemá žádný základní typ.
*  Pokud `T` je *enum_type*, základní typy `T` jsou typy tříd `System.Enum`, `System.ValueType` a `object`.
*  Pokud `T` je *struct_type*, základní typy `T` jsou typy třídy `System.ValueType` a `object`.
*  Pokud `T` je *class_type*, základní typy `T` jsou základní třídy `T`, včetně typu třídy `object`.
*  Pokud `T` je *INTERFACE_TYPE*, základní typy `T` jsou základními rozhraními `T` a typem třídy `object`.
*  Pokud `T` je *array_type*, základní typy `T` jsou typy třídy `System.Array` a `object`.
*  Pokud `T` je *delegate_type*, základní typy `T` jsou typy třídy `System.Delegate` a `object`.

## <a name="function-members"></a>Členové funkce

Členy funkce jsou členy, které obsahují spustitelné příkazy. Členové funkce jsou vždy členy typu a nemohou být členy oborů názvů. C#definuje následující kategorie členů funkcí:

*  Metody
*  properties
*  Duration
*  Indexery
*  Uživatelem definované operátory
*  Konstruktory instancí
*  Statické konstruktory
*  Destruktory

S výjimkou destruktorů a statických konstruktorů (které nelze vyvolat explicitně) jsou příkazy, které jsou obsaženy v členech funkce, spouštěny prostřednictvím vyvolání členů funkce. Skutečná syntaxe pro zápis člena funkce závisí na konkrétní kategorii člena funkce.

Seznam argumentů ([seznamy argumentů](expressions.md#argument-lists)) vyvolání člena funkce poskytuje skutečné hodnoty nebo odkazy na proměnné pro parametry člena funkce.

Volání obecných metod mohou využívat odvození typu pro určení sady argumentů typu, které mají být předávány metodě. Tento proces je popsán v tématu [odvození typu](expressions.md#type-inference).

Volání metod, indexerů, operátorů a konstruktorů instancí využívají rozlišení přetěžování k určení, které z kandidátních sad členů funkce mají být vyvolány. Tento proces je popsán v tématu [řešení přetížení](expressions.md#overload-resolution).

Po zjištění konkrétního člena funkce v době vytváření vazby, případně prostřednictvím řešení přetížení, je skutečný proces spuštění volání členu funkce popsán v [době kompilace dynamického překladu přetížení](expressions.md#compile-time-checking-of-dynamic-overload-resolution).

Následující tabulka shrnuje zpracování, které je provedeno v konstrukcích, které zahrnují šest kategorií členů funkce, které lze explicitně vyvolat. V tabulce `e`, `x`, `y` a `value` označují výrazy klasifikované jako proměnné nebo hodnoty, `T` označuje výraz klasifikovaný jako typ, `F` je jednoduchý název metody a `P` je jednoduchý název vlastnosti.


| __Contains__     | __Příklad__    | __Popis__ |
|-------------------|----------------|-----------------|
| Vyvolání metody | `F(x,y)`       | Rozlišení přetěžování je použito pro výběr nejlepší metody `F` v nadřazené třídě nebo struktuře. Metoda je vyvolána se seznamem argumentů `(x,y)`. Pokud metoda není `static`, je výraz instance `this`. | 
|                   | `T.F(x,y)`     | Rozlišení přetěžování je použito pro výběr nejlepší metody `F` ve třídě nebo struktuře `T`. Pokud není metoda `static`, dojde k chybě v době vazby. Metoda je vyvolána se seznamem argumentů `(x,y)`. | 
|                   | `e.F(x,y)`     | Rozlišení přetěžování je použito pro výběr nejlepší metody F ve třídě, struktuře nebo rozhraní zadané typem `e`. Pokud je metoda `static`, dojde k chybě při vazbě. Metoda je vyvolána s výrazem instance `e` a seznamem argumentů `(x,y)`. | 
| Přístup k vlastnosti   | `P`            | Vyvolá se přistupující objekt `get` vlastnosti `P` v nadřazené třídě nebo struktuře. Pokud je `P` pouze pro zápis, dojde k chybě v době kompilace. Pokud `P` není `static`, výraz instance je `this`. | 
|                   | `P = value`    | Přístupový objekt `set` vlastnosti `P` v nadřazené třídě nebo struktuře je vyvolán se seznamem argumentů `(value)`. Pokud je `P` jen pro čtení, dojde k chybě při kompilaci. Pokud `P` není `static`, výraz instance je `this`. | 
|                   | `T.P`          | Vyvolá se přistupující objekt `get` vlastnosti `P` ve třídě nebo struktuře `T`. Pokud `P` není `static` nebo pokud `P` je pouze pro zápis, dojde k chybě v době kompilace. | 
|                   | `T.P = value`  | Přístupový objekt `set` vlastnosti `P` ve třídě nebo struktuře `T` je vyvolán pomocí seznamu argumentů `(value)`. K chybě při kompilaci dojde, pokud `P` není `static` nebo pokud je `P` jen pro čtení. | 
|                   | `e.P`          | Přistupující objekt `get` vlastnosti `P` ve třídě, struktuře nebo rozhraní zadané typem `e` je vyvolán s výrazem instance `e`. Pokud je hodnota `P` `static` nebo pokud je `P` pouze pro zápis, dojde k chybě při vazbě. | 
|                   | `e.P = value`  | Přistupující objekt `set` vlastnosti `P` ve třídě, struktuře nebo rozhraní zadané typem `e` je vyvolán s výrazem instance `e` a seznamem argumentů `(value)`. Pokud je `P` `static` nebo pokud je `P` jen pro čtení, dojde k chybě při vazbě. | 
| Přístup k události      | `E += value`   | Byl vyvolán přistupující objekt `add` události `E` v nadřazené třídě nebo struktuře. Pokud `E` není statická, je výraz instance `this`. | 
|                   | `E -= value`   | Byl vyvolán přistupující objekt `remove` události `E` v nadřazené třídě nebo struktuře. Pokud `E` není statická, je výraz instance `this`. | 
|                   | `T.E += value` | Je vyvolán přistupující objekt `add` události `E` ve třídě nebo struktuře `T`. Pokud `E` není statická, dojde k chybě při vazbě. | 
|                   | `T.E -= value` | Je vyvolán přistupující objekt `remove` události `E` ve třídě nebo struktuře `T`. Pokud `E` není statická, dojde k chybě při vazbě. | 
|                   | `e.E += value` | Přistupující objekt `add` události `E` ve třídě, struktuře nebo rozhraní zadané typem `e` je vyvolán s výrazem instance `e`. Pokud je `E` statický, dojde k chybě při vazbě. | 
|                   | `e.E -= value` | Přistupující objekt `remove` události `E` ve třídě, struktuře nebo rozhraní zadané typem `e` je vyvolán s výrazem instance `e`. Pokud je `E` statický, dojde k chybě při vazbě. | 
| Přístup indexeru    | `e[x,y]`       | Rozlišení přetěžování je použito pro výběr nejlepšího indexeru ve třídě, struktuře nebo rozhraní, které je zadáno pomocí typu e. Přístupový objekt `get` indexeru je vyvolán s výrazem instance `e` a seznamem argumentů `(x,y)`. V případě, že je indexer určen pouze pro zápis, dojde k chybě při vazbě. | 
|                   | `e[x,y] = value` | Rozlišení přetěžování je použito pro výběr nejlepšího indexeru ve třídě, struktuře nebo rozhraní zadaného typem `e`. Přístupový objekt `set` indexeru je vyvolán s výrazem instance `e` a seznamem argumentů `(x,y,value)`. V případě, že je indexer určen jen pro čtení, dojde k chybě při vazbě. | 
| Vyvolání operátoru | `-x`         | Rozlišení přetěžování je použito pro výběr nejlepšího unárního operátoru ve třídě nebo struktuře zadané typem `x`. Vybraný operátor je vyvolán se seznamem argumentů `(x)`. | 
|                     | `x + y`      | Rozlišení přetěžování je použito pro výběr nejlepšího binárního operátoru v třídách nebo strukturách daných typy `x` a `y`. Vybraný operátor je vyvolán se seznamem argumentů `(x,y)`. | 
| Vyvolání konstruktoru instance | `new T(x,y)` | Rozlišení přetěžování je použito pro výběr nejlepšího konstruktoru instance ve třídě nebo struktuře `T`. Konstruktor instance je vyvolán se seznamem argumentů `(x,y)`. | 

### <a name="argument-lists"></a>Seznamy argumentů

Každý člen funkce a volání delegáta obsahují seznam argumentů, který poskytuje skutečné hodnoty nebo odkazy na proměnné pro parametry člena funkce. Syntaxe pro určení seznamu argumentů vyvolání člena funkce závisí na kategorii člena funkce:

*  Pro konstruktory instancí, metody, indexery a delegáty jsou argumenty zadány jako *argument_list*, jak je popsáno níže. U indexerů při vyvolání přístupového objektu `set` zahrnuje seznam argumentů také výraz zadaný jako pravý operand operátoru přiřazení.
*  V případě vlastností je seznam argumentů prázdný při vyvolání přístupového objektu `get` a skládá se z výrazu zadaného jako pravý operand operátoru přiřazení při vyvolání přístupového objektu `set`.
*  V případě událostí se seznam argumentů skládá z výrazu zadaného jako pravý operand operátoru `+=` nebo `-=`.
*  V případě uživatelem definovaných operátorů se seznam argumentů skládá z jednoho operandu unárního operátoru nebo dvou operandů binárního operátoru.

Argumenty vlastností ([vlastnosti](classes.md#properties)), události ([události](classes.md#events)) a uživatelsky definované operátory ([operátory](classes.md#operators)) jsou vždy předány jako parametry hodnoty ([parametry hodnot](classes.md#value-parameters)). Argumenty indexerů ([indexerů](classes.md#indexers)) jsou vždy předány jako parametry hodnoty ([parametry hodnot](classes.md#value-parameters)) nebo pole parametrů ([pole parametrů](classes.md#parameter-arrays)). Parametry reference a Output nejsou podporovány pro tyto kategorie členů funkce.

Argumenty konstruktoru instance, metody, indexeru nebo delegovaného volání jsou zadány jako *argument_list*:

```antlr
argument_list
    : argument (',' argument)*
    ;

argument
    : argument_name? argument_value
    ;

argument_name
    : identifier ':'
    ;

argument_value
    : expression
    | 'ref' variable_reference
    | 'out' variable_reference
    ;
```

*Argument_list* se skládá z jednoho nebo více *argumentů*, které jsou odděleny čárkami. Každý argument se skládá z volitelného *argument_name* následovanýho *argument_value*. *Argument* s *argument_name* je označován jako ***pojmenovaný argument***, zatímco *Argument* bez *argument_name* je ***poziční argument***. Je-li pozice argumentu uvedena po pojmenovaném argumentu v *argument_list*, jedná se o chybu.

*Argument_value* může mít jednu z následujících forem:

*  *Výraz*, který označuje, že argument je předán jako parametr hodnoty ([parametry hodnoty](classes.md#value-parameters)).
*  Klíčové slovo `ref` následované *variable_reference* ([odkazy na proměnné](variables.md#variable-references)), které značí, že argument je předán jako referenční parametr ([referenční parametry](classes.md#reference-parameters)). Proměnná musí být jednoznačně přiřazena ([jednoznačné přiřazení](variables.md#definite-assignment)) předtím, než může být předána jako parametr reference. Klíčové slovo `out` následované *variable_reference* ([odkazy na proměnné](variables.md#variable-references)), které značí, že argument je předán jako výstupní parametr ([výstupní parametry](classes.md#output-parameters)). Proměnná se považuje za jednoznačně přiřazená ([jednoznačné přiřazení](variables.md#definite-assignment)) za voláním členů funkce, ve kterém je proměnná předána jako výstupní parametr.

#### <a name="corresponding-parameters"></a>Odpovídající parametry

Pro každý argument v seznamu argumentů musí být příslušný parametr v členu funkce nebo vyvolán delegát.

Seznam parametrů použitý v následujícím příkladu je určen následujícím způsobem:

*  Pro virtuální metody a indexery definované ve třídách je seznam parametrů převzat z nejvíce specifické deklarace nebo přepsání člena funkce, počínaje statickým typem příjemce a hledáním v rámci jeho základních tříd.
*  Pro metody rozhraní a indexery je seznam parametrů vybrán z konkrétní definice člena, počínaje typem rozhraní a hledáním v základních rozhraních. Není-li nalezen žádný jedinečný seznam parametrů, je vytvořen seznam parametrů s nepřístupnými jmény a bez volitelných parametrů, aby vyvolání nemohlo používat pojmenované parametry nebo vynechat volitelné argumenty.
*  Pro částečné metody je použit seznam parametrů definující deklaraci částečné metody.
*  Pro všechny ostatní členy a delegáty funkcí je pouze jeden seznam parametrů, který je použit.

Pozice argumentu nebo parametru je definována jako počet argumentů nebo parametrů předcházejících v seznamu argumentů nebo seznamu parametrů.

Odpovídající parametry pro argumenty členů funkce jsou vytvořeny následujícím způsobem:

*  Argumenty v *argument_list* konstruktorů instancí, metod, indexerů a delegátů:
    * Poziční argument, ve kterém se vyskytuje pevný parametr na stejné pozici v seznamu parametrů, odpovídá tomuto parametru.
    * Poziční argument členu funkce s polem parametrů vyvolaným v jeho normálním formuláři odpovídá poli parametrů, které se musí vyskytovat na stejné pozici v seznamu parametrů.
    * Poziční argument členu funkce s polem parametrů vyvolaným v rozbaleném formuláři, kde žádný pevný parametr neprobíhá na stejné pozici v seznamu parametrů, odpovídá prvku v poli parametrů.
    * Pojmenovaný argument odpovídá parametru se stejným názvem v seznamu parametrů.
    * U indexerů při vyvolání přístupového objektu `set` odpovídá výraz zadaný jako pravý operand operátoru přiřazení implicitní parametr `value` deklarace přístupového objektu `set`.
*  U vlastností při vyvolání přístupového objektu `get` nejsou žádné argumenty. Při vyvolání přístupového objektu `set` odpovídá výraz zadaný jako pravý operand operátoru přiřazení implicitní parametr `value` deklarace přístupového objektu `set`.
*  V případě uživatelem definovaných unárních operátorů (včetně převodů) odpovídá jeden operand jednomu parametru deklarace operátoru.
*  V případě uživatelem definovaných binárních operátorů odpovídá levý operand prvnímu parametru a pravý operand odpovídá druhému parametru deklarace operátoru.

#### <a name="run-time-evaluation-of-argument-lists"></a>Zkušební doba běhu seznamů argumentů

Během běhu při vyvolání člena funkce ([Kontrola dynamického přetěžování při kompilaci](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) jsou výrazy nebo odkazy na proměnné seznamu argumentů vyhodnocovány v pořadí zleva doprava následujícím způsobem:

*  Pro parametr hodnoty je vyhodnocen výraz argumentu a je proveden implicitní převod ([implicitní převody](conversions.md#implicit-conversions)) na odpovídající typ parametru. Výsledná hodnota se stala počáteční hodnotou parametru value ve volání členu funkce.
*  Pro odkaz nebo výstupní parametr je vyhodnocen odkaz na proměnnou a výsledné umístění úložiště se bude umístěním úložiště reprezentovaným parametrem ve volání člena funkce. Pokud je odkaz na proměnnou zadaný jako odkaz nebo výstupní parametr prvkem pole *reference_type*, provede se kontrola za běhu, aby se zajistilo, že typ prvku pole je stejný jako typ parametru. Pokud se tato chyba kontroly nezdařila, je vyvolána `System.ArrayTypeMismatchException`.

Metody, indexery a konstruktory instancí mohou deklarovat svůj parametr, který je nejvíce vpravo, aby bylo pole parametrů ([pole parametrů](classes.md#parameter-arrays)). Tyto členy funkce jsou vyvolány v jejich normálním tvaru nebo v rozbaleném formuláři v závislosti na tom, která z nich je použita ([příslušný člen funkce](expressions.md#applicable-function-member)):

*  Je-li člen funkce s polem parametrů vyvolána v normálním tvaru, argument zadaný pro pole parametrů musí být jeden výraz, který je implicitně převeden ([implicitní převod](conversions.md#implicit-conversions)) na typ pole parametrů. V tomto případě pole parametrů funguje přesně jako parametr hodnoty.
*  Pokud je v rozbaleném formuláři vyvolána člen funkce s polem parametrů, musí být volání zadáno nula nebo více pozičních argumentů pro pole parametrů, kde každý argument je výraz, který je implicitně převoditelný ([implicitní převody ](conversions.md#implicit-conversions)) na typ prvku pole parametrů. V tomto případě vyvolání vytvoří instanci typu pole parametru s délkou odpovídající počtu argumentů, inicializuje prvky instance pole pomocí daných hodnot argumentů a použije nově vytvořenou instanci pole jako aktuální. Argument.

Výrazy seznamu argumentů jsou vždy vyhodnocovány v pořadí, ve kterém jsou zapsány. Proto příklad
```csharp
class Test
{
    static void F(int x, int y = -1, int z = -2) {
        System.Console.WriteLine("x = {0}, y = {1}, z = {2}", x, y, z);
    }

    static void Main() {
        int i = 0;
        F(i++, i++, i++);
        F(z: i++, x: i++);
    }
}
```
Vytvoří výstup
```console
x = 0, y = 1, z = 2
x = 4, y = -1, z = 3
```

Pravidla koodchylky pole ([kovariance pole](arrays.md#array-covariance)) povolují hodnotu typu pole `A[]`, aby byla odkazem na instanci typu pole `B[]`, za předpokladu, že se implicitní převod odkazu vyskytuje z `B` na `A`. Z důvodu těchto pravidel platí, že když je prvek pole *reference_type* předán jako odkazový nebo výstupní parametr, je vyžadována kontrola za běhu, aby se zajistilo, že skutečný typ prvku pole je stejný jako parametr. V příkladu
```csharp
class Test
{
    static void F(ref object x) {...}

    static void Main() {
        object[] a = new object[10];
        object[] b = new string[10];
        F(ref a[0]);        // Ok
        F(ref b[1]);        // ArrayTypeMismatchException
    }
}
```
druhé vyvolání `F` způsobí vyvolání `System.ArrayTypeMismatchException`, protože skutečný typ prvku `b` je `string` a nikoli `object`.

Když je v rozbaleném formuláři vyvolána člen funkce s polem parametrů, je volání zpracováno přesně tak, jako by byl vložen výraz vytvoření pole s inicializátorem pole ([výrazy vytvoření pole](expressions.md#array-creation-expressions)) kolem rozšířených parametrů. Například s ohledem na deklaraci
```csharp
void F(int x, int y, params object[] args);
```
následující vyvolání rozšířené formy metody
```csharp
F(10, 20);
F(10, 20, 30, 40);
F(10, 20, 1, "hello", 3.0);
```
odpovídat přesně na
```csharp
F(10, 20, new object[] {});
F(10, 20, new object[] {30, 40});
F(10, 20, new object[] {1, "hello", 3.0});
```

Konkrétně si všimněte, že prázdné pole je vytvořeno, pokud jsou pro pole parametrů zadány nula argumentů.

Pokud jsou argumenty vynechány od členu funkce s odpovídajícími nepovinnými parametry, výchozí argumenty deklarace člena funkce budou implicitně předány. Vzhledem k tomu, že jsou vždycky konstantní, jejich hodnocení nebude mít vliv na pořadí vyhodnocení zbývajících argumentů.

### <a name="type-inference"></a>Odvození typu

Při volání obecné metody bez určení argumentů typu se proces ***odvození typu*** pokusí odvodit argumenty typu pro volání. Přítomnost odvození typu umožňuje, aby se pro volání obecné metody používala pohodlnější syntaxe a aby programátor nezadal redundantní informace o typu. Například s ohledem na deklaraci metody:
```csharp
class Chooser
{
    static Random rand = new Random();

    public static T Choose<T>(T first, T second) {
        return (rand.Next(2) == 0)? first: second;
    }
}
```
je možné vyvolat metodu `Choose` bez explicitního určení argumentu typu:
```csharp
int i = Chooser.Choose(5, 213);                 // Calls Choose<int>

string s = Chooser.Choose("foo", "bar");        // Calls Choose<string>
```

V rámci odvození typu jsou argumenty typu `int` a `string` určeny z argumentů metody.

K odvození typu dojde jako součást zpracování volání metody v době vazby ([vyvolání metod](expressions.md#method-invocations)) a provádí se před krokem vyřešení přetížení volání. Pokud je konkrétní skupina metod určena v volání metody a žádné argumenty typu nejsou zadány jako součást volání metody, odvození typu je aplikováno na každou obecnou metodu ve skupině metod. Pokud odvození typu je úspěšné, pak se k určení typů argumentů pro následné řešení přetížení použijí argumenty odvozených typů. Pokud řešení přetížení zvolí obecnou metodu jako metodu, která má být vyvolána, pak jsou argumenty odvozeného typu použity jako skutečné argumenty typu pro vyvolání. Pokud je odvození typu pro určitou metodu neúspěšné, tato metoda není součástí řešení přetížení. Selhání pro odvození typu, v a samotné, nezpůsobí chybu při vazbě. Nicméně často vede k chybě při vazbě v případě, že řešení přetížení pak nenalezne žádné použitelné metody.

Pokud se zadaný počet argumentů liší od počtu parametrů v metodě, pak se rozhraní odvození okamžitě nezdařilo. V opačném případě Předpokládejme, že obecná metoda má následující signaturu:
```csharp
Tr M<X1,...,Xn>(T1 x1, ..., Tm xm)
```

Pomocí volání metody formuláře `M(E1...Em)` úloha typu odvození je najít jedinečné argumenty typu `S1...Sn` pro každý parametr typu `X1...Xn`, takže volání `M<S1...Sn>(E1...Em)` bude platné.

Během procesu odvození každého parametru typu `Xi` je buď *pevně* nastavená na konkrétní typ `Si` nebo *unfixed* s přidruženou sadou *hranic*. Každá z mezí je typu `T`. Zpočátku je každá proměnná typu `Xi` neopravena s prázdnou sadou mezí.

Odvození typu se provádí ve fázích. Každá fáze se pokusí odvodit argumenty typu pro více proměnných typu na základě zjištění předchozí fáze. První fáze vytvoří několik počátečních odvození hranic, zatímco druhá fáze opraví proměnné typu na konkrétní typy a odvodí další meze. Druhá fáze se možná bude muset opakovat několikrát.

*Poznámka:* Odvození typu bude provedeno nejen při volání obecné metody. Odvození typu pro převod skupin metod je popsáno v tématu [odvození typu pro převod skupin metod](expressions.md#type-inference-for-conversion-of-method-groups) a nalezení nejlepšího společného typu sady výrazů je popsána v tématu [vyhledání nejlepšího typu](expressions.md#finding-the-best-common-type-of-a-set-of-expressions)sady výrazů.

#### <a name="the-first-phase"></a>První fáze

Pro každý argument metody `Ei`:

*   Pokud je `Ei` anonymní funkce, provede se *explicitní odvození typu parametru* ([odvození typu explicitního typu parametru](expressions.md#explicit-parameter-type-inferences)) z `Ei` na `Ti`.
*   V opačném případě, pokud `Ei` má typ `U` a `xi` je parametr hodnoty, pak se pro *odvození dolní hranice* vytvoří *od* `U` *do* `Ti`.
*   V opačném případě, pokud `Ei` má typ `U` a `xi` je parametrem `ref` nebo `out`, pak bude *Přesná odvozená* *od* `U` *až* `Ti`.
*   V opačném případě není pro tento argument provedeno odvození.


#### <a name="the-second-phase"></a>Druhá fáze

Druhá fáze pokračuje následujícím způsobem:

*   Všechny *neopravené* proměnné typu `Xi`, které *nezávisí na* ([závislost](expressions.md#dependence)) jakékoli `Xj` jsou opraveny ([Oprava](expressions.md#fixing)).
*   Pokud žádné takové proměnné typu neexistují, jsou *opraveny* všechny proměnné *nefixního* typu `Xi`, pro které jsou všechny následující podrženy:
    *   Existuje alespoň jedna proměnná typu `Xj`, která závisí na `Xi`.
    *   `Xi` má neprázdnou sadu mezí.
*   Pokud žádné takové proměnné typu neexistují a existují stále *nepevné* proměnné typu, odvození typu se nezdařilo.
*   V opačném případě, pokud žádné další *nepevné* proměnné typu existují, odvození typu je úspěšné.
*   Jinak platí, že[](expressions.md#input-types) [](expressions.md#output-types) *pro všechny argumenty `Ei` s odpovídajícím typem parametru `Ti`, kde výstupní typy (výstupní typy) obsahují nepevné proměnné typu `Xj`, ale vstupní typy (vstupní typy) nepodporují. odvození typu výstupu* ([odvození typu výstupu](expressions.md#output-type-inferences)) se provádí *z* 1 *do* 3. Druhá fáze se pak opakuje.

#### <a name="input-types"></a>Typy vstupu

Pokud je `E` skupina metod nebo implicitně typové anonymní funkce a `T` je typ delegáta nebo strom výrazů, pak všechny typy parametrů `T` jsou *vstupní typy* `E` *s typem* `T`.

####  <a name="output-types"></a>Výstupní typy

Pokud je `E` skupinou metod nebo anonymní funkcí a `T` je typ delegáta nebo typ stromu výrazu, pak návratový typ `T` je *výstupní typ* `E` *s typem* `T`.

#### <a name="dependence"></a>Drog

*Neopravená* proměnná typu `Xi` *závisí přímo na* nepevné proměnné typu `Xj`, pokud pro některý argument `Ek` s typem `Tk` `Xj` dojde v *vstupním typu* `Ek` s typem `Tk` a 0 dochází v  *Typ výstupu* 2 s typem 3.

`Xj` *závisí na* `Xi`, pokud `Xj` *závisí přímo na* `Xi` nebo pokud `Xi` závisí *přímo na* `Xk` a `Xk` *závisí na* 1. Proto "závisí na" je přenosný, ale ne reflexivní uzavření "závisí přímo na".

#### <a name="output-type-inferences"></a>Odvození typu výstupu

*Odvození typu výstupu* je provedeno *z* výrazu `E` *na* typ `T` následujícím způsobem:

*  Je-li `E` anonymní funkce s odvozeným návratovým typem `U` ([odvozený návratový typ](expressions.md#inferred-return-type)) a `T` je typ delegáta nebo typ stromu výrazu s návratovým typem `Tb`, pak *odvození s nižší vazbou* ([odvození s nižšími hranicemi](expressions.md#lower-bound-inferences)) provede se *z* `U` *až* 0.
*  V opačném případě, pokud `E` je skupina metod a `T` je typ delegáta nebo typ stromu výrazů s typy parametrů `T1...Tk` a návratový typ `Tb` a řešení přetížení `E` s typy `T1...Tk` vydává jedinou metodu s návratovým typem `U`. , pak je *odvozený* *od* `U` *k* 1.
*  V opačném případě, pokud je `E` výraz s typem `U`, pak se *odvození dolní hranice* provede *od* `U` *do* `T`.
*  Jinak nejsou provedeny žádné odvozené.

#### <a name="explicit-parameter-type-inferences"></a>Explicitní odvození typu parametru

*Explicitní odvození typu parametru* je provedeno *z* výrazu `E` *na* typ `T` následujícím způsobem:

*  Pokud je `E` explicitně typové anonymní funkce s typy parametrů `U1...Uk` a `T` je typ delegáta nebo typ stromu výrazů s typy parametrů `V1...Vk` potom pro každé `Ui` je proveden [](expressions.md#exact-inferences) *přesný odvození (neodvozeno). z* `Ui` *na* odpovídající 0.

#### <a name="exact-inferences"></a>Přesná odvozená

*Přesné odvození* *z* typu `U` *na* typ `V` je provedeno následujícím způsobem:

*  Pokud je `V` jednou z *neopravených* `Xi`, `U` se přidá do sady přesných hranic pro `Xi`.

*  V opačném případě se nastaví `V1...Vk` a `U1...Uk` tím, že se zjistí, jestli platí některý z následujících případů:

   *  `V` je typ pole `V1[...]` a `U` je typ pole `U1[...]` stejného rozměru.
   *  `V` je typ `V1?` a `U` je typ `U1?`.
   *  `V` je konstruovaný typ `C<V1...Vk>`and `U` je konstruovaný typ `C<U1...Uk>`.

   Pokud se některý z těchto případů použije, bude proveden *přesný odvození* *z* každé `Ui` *k* odpovídajícímu `Vi`.

*  Jinak nejsou k dispozici žádná odvozená.

#### <a name="lower-bound-inferences"></a>Odvození s nižší vazbou

*Odvození s nižší vazbou* *z* typu `U` *na* typ `V` je provedeno následujícím způsobem:

*  Pokud je `V` jednou z *neopravených* `Xi`, `U` se přidá do sady dolních mezí pro `Xi`.
*  V opačném případě, pokud `V` je typ `V1?`and `U` je typ `U1?`, pak je od `U1` k `V1` proveden nižší vázaný odvození.
*  V opačném případě se nastaví `U1...Uk` a `V1...Vk` tím, že se zjistí, jestli platí některý z následujících případů:
   *  `V` je typ pole `V1[...]` a `U` je typ pole `U1[...]` (nebo parametr typu, jehož účinný základní typ je `U1[...]`) stejného pořadí.
   *  `V` je jedním z `IEnumerable<V1>`, `ICollection<V1>` nebo `IList<V1>` a `U` je jednorozměrné pole typu `U1[]` (nebo parametr typu, jehož účinný základní typ je `U1[]`).
   *  `V` je konstruovaný typ třídy, struktury, rozhraní nebo delegáta `C<V1...Vk>` a existuje jedinečný typ `C<U1...Uk>`, například `U` (nebo, pokud `U` je parametr typu, jeho účinná základní třída nebo jakýkoli člen jeho efektivní sady rozhraní) je stejný jako , dědí z (přímo nebo nepřímo) nebo implementuje (přímo nebo nepřímo) `C<U1...Uk>`.

      ("Jedinečnost" omezení znamená, že v případě rozhraní `C<T> {} class U: C<X>, C<Y> {}` není při odvodit od `U` do `C<T>` provedena žádná odvození, protože `U1` by mohla být `X` nebo `Y`.)

   Pokud se některý z těchto případů aplikuje, odvození *z* každé `Ui` *k* odpovídajícímu `Vi` je provedeno následujícím způsobem:

   *  Pokud `Ui` není znám jako typ odkazu, pak je proveden *přesný odvození*
   *  V opačném případě, pokud `U` je typ pole, bude provedeno *odvození dolní hranice* .
   *  V opačném případě, pokud `V` je `C<V1...Vk>` pak odvození závisí na parametru i-th typu `C`:
      *  Pokud je kovariantní, je provedeno *odvození s nižší vazbou* .
      *  Pokud je kontravariantní, je provedeno *odvození horní meze* .
      *  Pokud je neutrální, pak je proveden *přesný odvození* .
*  Jinak nejsou provedeny žádné odvozené.

#### <a name="upper-bound-inferences"></a>Odvozené vazby na horní hranice

*Odvození horní meze* *z* typu `U` *na* typ `V` je provedeno následujícím způsobem:

*  Pokud je `V` jednou z *neopravených* `Xi`, `U` se přidá do sady horních mezí pro `Xi`.
*  V opačném případě se nastaví `V1...Vk` a `U1...Uk` tím, že se zjistí, jestli platí některý z následujících případů:
   *  `U` je typ pole `U1[...]` a `V` je typ pole `V1[...]` stejného rozměru.
   *  `U` je jedním z `IEnumerable<Ue>`, `ICollection<Ue>` nebo `IList<Ue>` a `V` je jednorozměrné pole typu `Ve[]`.
   *  `U` je typ `U1?` a `V` je typ `V1?`.
   *  `U` je konstrukce třídy, struktury, rozhraní nebo typu delegáta `C<U1...Uk>` a `V` je třída, struktura, rozhraní nebo typ delegáta, který je totožný s, dědí z (přímo nebo nepřímo) nebo implementuje (přímo nebo nepřímo) jedinečný typ `C<V1...Vk>`.

      (Omezení jedinečnosti znamená, že pokud máme `interface C<T>{} class V<Z>: C<X<Z>>, C<Y<Z>>{}`, není při odvodit od `C<U1>` do `V<Q>` provedena žádná odvození. Odvození není provedeno z `U1` do buď `X<Q>` nebo `Y<Q>`.)

   Pokud se některý z těchto případů aplikuje, odvození *z* každé `Ui` *k* odpovídajícímu `Vi` je provedeno následujícím způsobem:
   *  Pokud `Ui` není znám jako typ odkazu, pak je proveden *přesný odvození*
   *  V opačném případě, pokud `V` je typ pole, je proveden *odvození horní meze*
   *  V opačném případě, pokud `U` je `C<U1...Uk>` pak odvození závisí na parametru i-th typu `C`:
      *  Pokud je kovariantní, pak se provede *odvození horní meze* .
      *  Pokud je kontravariantní, je provedeno *odvození s nižší vazbou* .
      *  Pokud je neutrální, pak je proveden *přesný odvození* .
*  Jinak nejsou provedeny žádné odvozené.   

#### <a name="fixing"></a>Opravě

*Nepevná* proměnná typu `Xi` se sadou mezí je *opravena* následujícím způsobem:

*  Sada *typů kandidátů* `Uj` začíná jako sada všech typů v sadě mezí pro `Xi`.
*  Pak prověříme všechny vazby pro `Xi` v tuto akci: Pro každý přesný vázaný `U` z `Xi` všechny typy `Uj`, které nejsou identické s `U`, se ze sady kandidátů odeberou. Pro každý dolní mez `U` z `Xi` všechny typy `Uj`, na *které není implicitní* převod z `U` odebraný ze sady kandidátů. Pro každé horní vázané `U` z `Xi` všechny typy `Uj`, ze *kterých není implicitní* převod na `U`, se ze sady kandidátů odeberou.
*  Pokud se jedná o zbývající typy kandidátů `Uj` existuje jedinečný typ `V`, ze kterého existuje implicitní převod na všechny jiné typy kandidátů, pak `Xi` je pevně nastavená na `V`.
*  V opačném případě se odvození typu nezdařilo.

#### <a name="inferred-return-type"></a>Odvozený návratový typ

Odvozený návratový typ anonymní funkce `F` se používá při odvozování typu a řešení přetížení. Odvozený návratový typ lze určit pouze pro anonymní funkci, kde jsou známy všechny typy parametrů, buď protože jsou explicitně uděleny, poskytnuté prostřednictvím konverze anonymní funkce nebo odvozené při odvozování typu v nadřazeném obecném prvku. volání metody.

***Odvozený typ výsledku*** je určen následujícím způsobem:

*  Pokud je tělo `F` *výraz* , který má typ, pak odvozený typ výsledku `F` je typ tohoto výrazu.
*  Pokud je tělo `F` *blok* a sada výrazů v příkazech `return` bloku má nejlepší společný typ `T` ([hledání nejlepšího společného typu sady výrazů](expressions.md#finding-the-best-common-type-of-a-set-of-expressions)), pak odvozený typ výsledku `F` je `T`.
*  V opačném případě nelze typ výsledku odvodit pro `F`.

***Odvozený návratový typ*** je určen následujícím způsobem:

*  Pokud je `F` asynchronní a tělo `F` je buď výraz klasifikovaný jako Nothing ([klasifikace výrazů](expressions.md#expression-classifications)), nebo blok příkazu, pokud žádné návratové příkazy nemají výrazy, odvozený návratový typ je `System.Threading.Tasks.Task`.
*  Pokud je `F` asynchronní a má odvozený typ výsledku `T`, odvozený návratový typ je `System.Threading.Tasks.Task<T>`.
*  Pokud je `F` neasynchronní a má odvozený typ výsledku `T`, odvozený návratový typ je `T`.
*  Jinak nelze odvodit návratový typ pro `F`.

Jako příklad odvození typu zahrnujícího anonymní funkce zvažte, že metoda rozšíření `Select` deklarovaná ve třídě `System.Linq.Enumerable`:
```csharp
namespace System.Linq
{
    public static class Enumerable
    {
        public static IEnumerable<TResult> Select<TSource,TResult>(
            this IEnumerable<TSource> source,
            Func<TSource,TResult> selector)
        {
            foreach (TSource element in source) yield return selector(element);
        }
    }
}
```

Za předpokladu, že se obor názvů `System.Linq` importoval s klauzulí `using` a třída `Customer` s vlastností `Name` typu `string`, lze použít metodu `Select` k výběru názvů seznamu zákazníků:
```csharp
List<Customer> customers = GetCustomerList();
IEnumerable<string> names = customers.Select(c => c.Name);
```

Volání rozšiřující metody ([vyvolání rozšiřujících metod](expressions.md#extension-method-invocations)) `Select` se zpracovává přepsáním vyvolání do statické metody vyvolání:
```csharp
IEnumerable<string> names = Enumerable.Select(customers, c => c.Name);
```

Vzhledem k tomu, že argumenty typu nebyly explicitně zadány, použije se odvození typu k odvození argumentů typu. Nejprve argument `customers` souvisí s parametrem `source`, odvozením `T` bude `Customer`. Pak pomocí procesu odvozování anonymního typu funkce, který je popsaný výše, `c` předaný typ `Customer` a výraz `c.Name` se vztahuje na návratový typ parametru `selector`, odvodit `S` na `string`. Proto je vyvolání ekvivalentní
```csharp
Sequence.Select<Customer,string>(customers, (Customer c) => c.Name)
```
a výsledek je typu `IEnumerable<string>`.

Následující příklad ukazuje, jak odvození typu anonymní funkce umožňuje informace typu "Flow" mezi argumenty v rámci obecného volání metody. Zadaná metoda:
```csharp
static Z F<X,Y,Z>(X value, Func<X,Y> f1, Func<Y,Z> f2) {
    return f2(f1(value));
}
```

Odvození typu pro vyvolání:
```csharp
double seconds = F("1:15:30", s => TimeSpan.Parse(s), t => t.TotalSeconds);
```
pokračuje následujícím způsobem: Nejprve argument `"1:15:30"` souvisí s parametrem `value`, odvozením `X` bude `string`. Pak parametr první anonymní funkce `s` má odvozený typ `string` a výraz `TimeSpan.Parse(s)` se vztahuje na návratový typ `f1`, odvodit `Y` na `System.TimeSpan`. Nakonec parametr druhé anonymní funkce `t` má odvozený typ `System.TimeSpan` a výraz `t.TotalSeconds` se vztahuje na návratový typ `f2`, odvodit `Z` na `double`. Proto je výsledek vyvolání typu `double`.

#### <a name="type-inference-for-conversion-of-method-groups"></a>Odvození typu pro převod skupin metod

Podobně jako u volání obecných metod musí být odvození typu použito také v případě, že skupina metod `M` obsahující obecnou metodu, je převedena na daný typ delegáta `D` ([Převod skupin metod](conversions.md#method-group-conversions)). Daná metoda
```csharp
Tr M<X1...Xn>(T1 x1 ... Tm xm)
```
a skupina metod `M` přiřazená k typu delegáta `D` úloha typu odvození je hledání argumentů typu `S1...Sn`, takže výraz:
```csharp
M<S1...Sn>
```
bude kompatibilní ([deklarace delegátů](delegates.md#delegate-declarations)) s `D`.

Na rozdíl od algoritmu odvození typu pro volání obecných metod, v tomto případě jsou k dispozici pouze *typy*argumentů, žádné *výrazy*argumentů. Konkrétně nejsou k dispozici žádné anonymní funkce, a proto není potřeba více fází odvození.

Místo *toho se všechny* `Xi` považují za *neopravené*a *z* každého typu argumentu `Uj` `D` *k* odpovídajícímu typu parametru `Tj` `M`. Pokud se nenašly žádné meze `Xi`, odvození typu se nezdařilo. Jinak všechny `Xi` jsou *opraveny* na odpovídající `Si`, což jsou výsledek odvození typu.

#### <a name="finding-the-best-common-type-of-a-set-of-expressions"></a>Hledání nejlepšího běžného typu sady výrazů

V některých případech je třeba odvodit společný typ pro sadu výrazů. Konkrétně jsou v tomto způsobu nalezeny typy prvků implicitně typovaného pole a návratové typy anonymních funkcí s poli *Block* .

V intuitivním případě s ohledem na sadu výrazů `E1...Em` Toto odvození by mělo být ekvivalentní volání metody.
```csharp
Tr M<X>(X x1 ... X xm)
```
s `Ei` jako argumenty.

Odvození je přesnější a začíná *nepevným* typem proměnné `X`. *Odvození typu výstupu* pak probíhá *z* každé `Ei` *na* `X`. Nakonec je `X` *opraven* a v případě úspěchu je výsledný typ `S` výsledným nejlépe běžným typem pro výrazy. Pokud žádné takové `S` neexistuje, výrazy nemají žádný nejlepší běžný typ.

### <a name="overload-resolution"></a>Rozlišení přetěžování

Rozlišení přetěžování je mechanizmus pro dobu vazby pro výběr nejlepšího člena funkce pro vyvolání daného seznamu argumentů a sady kandidátních členů funkce. Řešení přetížení vybere člena funkce k vyvolání v následujících odlišných kontextech v rámci C#:

*  Vyvolání metody s názvem v *invocation_expression* ([vyvolání metod](expressions.md#method-invocations)).
*  Vyvolání konstruktoru instance s názvem v *object_creation_expression* ([výrazy pro vytváření objektů](expressions.md#object-creation-expressions)).
*  Volání přístupového objektu indexeru prostřednictvím *element_access* ([přístup k elementu](expressions.md#element-access)).
*  Vyvolání předdefinovaného uživatelem definovaného operátoru, na který se odkazuje ve výrazu ([rozlišení přetížení unárního](expressions.md#unary-operator-overload-resolution) operátoru a [rozlišení přetížení binárního operátoru](expressions.md#binary-operator-overload-resolution)).

Každý z těchto kontextů definuje sadu kandidátních členů a seznam argumentů vlastním jedinečným způsobem, jak je popsáno podrobněji v částech uvedených výše. Například sada kandidátů pro vyvolání metody nezahrnuje metody označené `override` ([vyhledávání členů](expressions.md#member-lookup)) a metody v základní třídě nejsou kandidáty, pokud je jakákoliv metoda v odvozené třídě platná ([vyvolání metod](expressions.md#method-invocations)).

Po identifikaci členů kandidáta funkce a seznamu argumentů je výběr nejlepšího člena funkce stejný ve všech případech:

*  Vzhledem k sadě příslušných členů kandidátních funkcí se nachází nejlepší člen funkce v této sadě. Pokud sada obsahuje pouze jeden člen funkce, pak je tento člen funkce nejlepším členem funkce. V opačném případě je nejlepším členem funkce jeden člen funkce, který je lepší než všechny ostatní členy funkce s ohledem na daný seznam argumentů za předpokladu, že každý člen funkce je porovnán se všemi ostatními členy funkce pomocí pravidel v [lepší funkci. člen](expressions.md#better-function-member). Pokud není k dispozici přesně jeden člen funkce, který je lepší než všechny ostatní členy funkce, pak dojde k nejednoznačnému vyvolání člena funkce a dojde k chybě při vazbě.

V následujících oddílech jsou definovány přesné významy ***platných členů funkce*** a ***lepšího člena funkce***.

#### <a name="applicable-function-member"></a>Příslušný člen funkce

Člen funkce je označován jako ***příslušný člen funkce*** s ohledem na seznam argumentů `A`, pokud jsou splněny všechny následující podmínky:

*  Každý argument v `A` odpovídá parametru v deklaraci členu funkce, jak je popsáno v [odpovídajících parametrech](expressions.md#corresponding-parameters), a jakýkoli parametr, na který žádný argument neodpovídá, je volitelným parametrem.
*  Pro každý argument v `A` je režim předání parametru argumentu (tj. hodnota, `ref` nebo `out`) totožný s parametrem předávání odpovídajícího parametru.
   *  pro parametr hodnoty nebo pole parametrů existuje implicitní převod ([implicitní převody](conversions.md#implicit-conversions)) z argumentu na typ odpovídajícího parametru nebo
   *  pro parametr `ref` nebo `out` je typ argumentu totožný s typem odpovídajícího parametru. Po všech případech je parametr `ref` nebo `out` alias pro předaný argument.

Pro člena funkce, který obsahuje pole parametrů, pokud je člen funkce použitelný výše uvedenými pravidly, říká se, že se má použít v ***normální podobě***. Pokud člen funkce, který obsahuje pole parametrů, není použitelný v normálním tvaru, člen funkce může být použit v ***rozbaleném formátu***:

*  Rozbalený formulář se vytvoří tak, že nahradí pole parametrů v deklaraci členu funkce nula nebo více parametrů hodnot typu prvku pole parametrů tak, aby počet argumentů v seznamu argumentů `A` odpovídal celkovému počtu. parametrů. Pokud `A` má méně argumentů než počet pevných parametrů v deklaraci členu funkce, rozbalená forma členu funkce nemůže být vytvořena a není proto platná.
*  V opačném případě je rozšířený formulář použitelný, pokud pro každý argument v `A` je režim předávání parametru argumentu totožný s parametrem předávání odpovídajícího parametru a
   *  pro parametr pevné hodnoty nebo parametr hodnoty, který byl vytvořen rozšířením, existuje implicitní převod ([implicitní převod](conversions.md#implicit-conversions)) z typu argumentu na typ odpovídajícího parametru nebo
   *  pro parametr `ref` nebo `out` je typ argumentu totožný s typem odpovídajícího parametru.

#### <a name="better-function-member"></a>Lepší člen funkce

Pro účely určení lepšího člena funkce je vytvořen seznam argumentů, který obsahuje pouze výrazy argumentů v pořadí, v jakém jsou uvedeny v seznamu původní argument.

Seznamy parametrů pro každý člen kandidátních funkcí jsou konstruovány následujícím způsobem:

*  Rozšířený formulář se používá v případě, že byl člen funkce použit pouze v rozbaleném formuláři.
*  Volitelné parametry bez odpovídajících argumentů se ze seznamu parametrů odeberou.
*  Parametry jsou přeuspořádané tak, že se vyskytují na stejné pozici jako odpovídající argument v seznamu argumentů.

Předaný seznam argumentů `A` se sadou výrazů argumentů `{E1, E2, ..., En}` a dvou platných členů funkce `Mp` a `Mq` s typy parametrů `{P1, P2, ..., Pn}` a `{Q1, Q2, ..., Qn}`, `Mp` je definován jako ***lepší člen funkce*** než `Mq`, pokud

*  u každého argumentu je implicitní převod z `Ex` na `Qx` lepší než implicitní převod z `Ex` na `Px` a
*  pro alespoň jeden argument je převod z `Ex` na `Px` lepší než převod z `Ex` na `Qx`.

Pokud se provádí toto vyhodnocení, je-li v rozbalené formě použit parametr `Mp` nebo `Mq`, odkaz `Px` nebo `Qx` odkazuje na parametr v rozšířené podobě seznamu parametrů.

V případě, že parametry typu parametru @ no__t-0 a `{Q1, Q2, ..., Qn}` jsou ekvivalentní (to znamená, že každý `Pi` má převod identity na odpovídající `Qi`), jsou použita následující pravidla pro dělení na propojení, aby bylo možné určit lepší člen funkce.

*  Pokud je `Mp` neobecnou metodou a `Mq` je obecná metoda, pak `Mp` je lepší než `Mq`.
*  Jinak, pokud je `Mp` použitelný v normálním tvaru a `Mq` má pole `params` a je použitelné pouze v rozbaleném formuláři, pak `Mp` je lepší než `Mq`.
*  Jinak, pokud `Mp` má více deklarované parametry než `Mq`, pak `Mp` je lepší než `Mq`. K tomu může dojít, pokud obě metody mají @no__t pole-0 a jsou použitelné pouze v rozbalených formulářích.
*  Jinak, pokud všechny parametry `Mp` mají odpovídající argument, zatímco výchozí argumenty je potřeba nahradit aspoň u jednoho volitelného parametru v `Mq`, `Mp` je lepší než `Mq`.
*  Jinak, pokud `Mp` má konkrétnější typy parametrů než `Mq`, pak `Mp` je lepší než `Mq`. Let `{R1, R2, ..., Rn}` a `{S1, S2, ..., Sn}` představuje Nerozbalené a nerozšiřované typy parametrů `Mp` a `Mq`. typy parametrů `Mp` jsou konkrétnější než `Mq`, pokud pro každý parametr `Rx` není méně specifická než `Sx` a pro alespoň jeden parametr je `Rx` konkrétnější než `Sx`:
   *  Parametr typu je méně specifický než parametr bez typu.
   *  Rekurzivně, konstruovaný typ je konkrétnější než jiný konstruovaný typ (se stejným počtem argumentů typu), pokud je alespoň jeden argument typu konkrétnější a žádný argument typu není méně specifický než odpovídající argument typu v druhé.
   *  Typ pole je konkrétnější než jiný typ pole (se stejným počtem rozměrů), pokud typ prvku prvního je konkrétnější než typ elementu druhé.
*  V opačném případě, pokud je jeden člen jako nezdvižený operátor a druhý je předaný operátor, je lepší, než je nezatížený.
*  V opačném případě není ani člen funkce lepší.

#### <a name="better-conversion-from-expression"></a>Lepší převod výrazu

Předaný implicitní převod `C1`, který se převede z výrazu `E` na typ `T1` a implicitní převod `C2`, který se převede z výrazu `E` na typ `T2`, `C1` je ***lepší převod*** než `C2`, pokud @no__ t-9 neodpovídá přesně 0 a nejméně jeden z následujících blokování:

* `E` odpovídá přesně `T1` ([přesně odpovídající výraz](expressions.md#exactly-matching-expression))
* `T1` je lepším cílem převodu než `T2` ([cíl pro lepší převod](expressions.md#better-conversion-target)).

#### <a name="exactly-matching-expression"></a>Přesně shodný výraz

Vzhledem k výrazu `E` a typu `T` `E` přesně odpovídá `T`, pokud je k dispozici jeden z následujících typů:

*  `E` má typ `S` a v `S` na `T` existuje převod identity.
*  `E` je anonymní funkce, `T` je buď typ delegáta `D`, nebo typ stromu výrazu `Expression<D>` a jedna z následujících možností obsahuje:
   *  Odvozený návratový typ `X` existuje pro `E` v kontextu seznamu parametrů `D` ([odvozený návratový typ](expressions.md#inferred-return-type)) a převod identity existuje z `X` na návratový typ `D`.
   *  Buď `E` je neasynchronní a `D` má návratový typ `Y` nebo `E` je asynchronní a `D` má návratový typ `Task<Y>` a jeden z následujících blokování:
      * Tělo `E` je výraz, který přesně odpovídá `Y`.
      * Tělo `E` je blok příkazu, kde každý příkaz return vrátí výraz, který přesně odpovídá `Y`.

#### <a name="better-conversion-target"></a>Lepší cíl převodu

Zadané dva různé typy `T1` a `T2` `T1` je lepším cílem převodu než `T2`, pokud neexistuje implicitní převod z `T2` na `T1` a nejméně jeden z následujících blokování:

*  Implicitní převod z `T1` na `T2` existuje.
*  `T1` je buď typ delegáta `D1` nebo typ stromu výrazu `Expression<D1>`, `T2` je buď typ delegáta `D2`, nebo typ stromu výrazu `Expression<D2>`, `D1` má návratový typ `S1` a jeden z následujících blokování :
   * `D2` je vracející typ void.
   * `D2` má návratový typ `S2` a `S1` je lepším cílem převodu než `S2`.
*  `T1` je `Task<S1>`, `T2` je `Task<S2>` a `S1` je lepším cílem převodu než `S2`.
*  `T1` je `S1` nebo `S1?`, kde `S1` je celočíselný typ se znaménkem a `T2` je `S2` nebo `S2?`, kde `S2` je celočíselný typ bez znaménka. Určen
   * `S1` je `sbyte` a `S2` je `byte`, `ushort`, `uint` nebo `ulong`.
   * `S1` je `short` a `S2` je `ushort`, `uint` nebo `ulong`.
   * `S1` je `int` a `S2` je `uint` nebo `ulong`.
   * `S1` je `long` a `S2` je `ulong`.

#### <a name="overloading-in-generic-classes"></a>Přetížení v obecných třídách

I když signatury, které jsou deklarovány, musí být jedinečné, je možné, že nahrazení argumentů typu vede k identickým podpisům. V takových případech se pravidla, která jsou ve výše uvedeném Rozlišení přetěžování, budou vybírat nejvíce konkrétního člena.

Následující příklady znázorňují přetížení, které jsou platné a neplatné podle tohoto pravidla:

```csharp
interface I1<T> {...}

interface I2<T> {...}

class G1<U>
{
    int F1(U u);                  // Overload resolution for G<int>.F1
    int F1(int i);                // will pick non-generic

    void F2(I1<U> a);             // Valid overload
    void F2(I2<U> a);
}

class G2<U,V>
{
    void F3(U u, V v);            // Valid, but overload resolution for
    void F3(V v, U u);            // G2<int,int>.F3 will fail

    void F4(U u, I1<V> v);        // Valid, but overload resolution for    
    void F4(I1<V> v, U u);        // G2<I1<int>,int>.F4 will fail

    void F5(U u1, I1<V> v2);      // Valid overload
    void F5(V v1, U u2);

    void F6(ref U u);             // valid overload
    void F6(out V v);
}
```

### <a name="compile-time-checking-of-dynamic-overload-resolution"></a>Kontrola dynamického překladu přetížení při kompilaci

Pro většinu dynamicky vázaných operací není sada možných kandidátů na rozlišení v době kompilace neznámá. V některých případech je však kandidátská sada známá v době kompilace:

*  Volání statických metod s dynamickými argumenty
*  Metoda instance volá, kde příjemce není dynamický výraz.
*  Indexer volá, kde příjemce není dynamický výraz.
*  Konstruktor volá s dynamickými argumenty.

V těchto případech je k dispozici omezená doba kompilace pro každého kandidáta, aby bylo možné zjistit, zda by některá z nich mohla být použita v době běhu. Tato kontrolu se skládá z následujících kroků:

*  Odvození částečného typu: Jakýkoli argument typu, který není závislý přímo ani nepřímo na argumentu typu `dynamic` je odvozený pomocí pravidel [odvození typu](expressions.md#type-inference). Zbývající argumenty typu nejsou známy.
*  Částečná kontroly použitelnosti: Použitelnost je kontrolována v závislosti na [platném členu funkce](expressions.md#applicable-function-member), ale ignoruje parametry, jejichž typy jsou neznámé.
*  Pokud žádný kandidát neprojde tímto testem, dojde k chybě při kompilaci.

### <a name="function-member-invocation"></a>Vyvolání člena funkce

Tato část popisuje proces, který probíhá za běhu k vyvolání konkrétního člena funkce. Předpokládá se, že proces vytvoření vazby již určil konkrétního člena k vyvolání, pravděpodobně použitím řešení přetížení pro sadu kandidátních členů funkce.

Pro účely popisu procesu vyvolání jsou členové funkce rozděleni do dvou kategorií:

*  Členové statických funkcí. Jedná se o konstruktory instancí, statické metody, přistupující objekty statických vlastností a uživatelsky definované operátory. Členy statických funkcí jsou vždy nevirtuální.
*  Členové funkce instance. Jedná se o metody instancí, přistupující objekty vlastnosti instance a přistupující objekty indexeru. Členy funkce instance jsou buď nevirtuální, nebo virtuální a jsou vždy vyvolány na konkrétní instanci. Instance je vypočítána výrazem instance a je zpřístupněna v rámci člena funkce jako `this` ([Tento přístup](expressions.md#this-access)).

Běhové zpracování vyvolání členů funkce se skládá z následujících kroků, kde `M` je členem funkce a pokud `M` je členem instance, `E` je výraz instance:

*  Pokud je `M` členem statické funkce:
   * Seznam argumentů je vyhodnocován, jak je popsáno v [seznamech argumentů](expressions.md#argument-lists).
   * `M`je vyvolána.

*  Pokud `M` je člen funkce instance deklarované v *value_type*:
   * je vyhodnocena hodnota `E`. Pokud toto vyhodnocení způsobí výjimku, nejsou provedeny žádné další kroky.
   * Pokud `E` není klasifikován jako proměnná, je vytvořena dočasná lokální proměnná typu `E` a hodnota `E` je přiřazena k této proměnné. `E` je pak znovu klasifikován jako odkaz na tuto dočasnou místní proměnnou. Dočasná proměnná je k dispozici jako `this` v rámci `M`, ale ne jiným způsobem. Proto pouze v případě, že `E` je skutečná proměnná, může volající sledovat změny, které `M` dává `this`.
   * Seznam argumentů je vyhodnocován, jak je popsáno v [seznamech argumentů](expressions.md#argument-lists).
   * `M`je vyvolána. Proměnná, na kterou odkazuje `E`, se stala proměnnou, na kterou odkazuje `this`.

*  Pokud `M` je člen funkce instance deklarované v *reference_type*:
   * je vyhodnocena hodnota `E`. Pokud toto vyhodnocení způsobí výjimku, nejsou provedeny žádné další kroky.
   * Seznam argumentů je vyhodnocován, jak je popsáno v [seznamech argumentů](expressions.md#argument-lists).
   * Pokud je typ `E` *value_type*, převod na zabalení (převody na[zabalení](types.md#boxing-conversions)) provede převod `E` na typ `object` a `E` se považuje za typ `object` v následujících krocích. V tomto případě by `M` mohla být jenom členem `System.Object`.
   * Hodnota `E` je zkontrolována jako platná. Pokud je hodnota `E` `null`, je vyvolána `System.NullReferenceException` a nejsou provedeny žádné další kroky.
   * Je určena implementace členu funkce k vyvolání:
     * Pokud je typ doby vazby `E` rozhraním, člen funkce, který se má vyvolat, je implementace `M` poskytnutá typem za běhu instance, na kterou odkazuje `E`. Tento člen funkce je určen pomocí pravidel mapování rozhraní ([mapování rozhraní](interfaces.md#interface-mapping)) k určení implementace `M` poskytnutého typem za běhu instance, na kterou odkazuje `E`.
     * V opačném případě, pokud je `M` členem virtuální funkce, je člen funkce, který se má volat, implementace `M` poskytnutého typem za běhu instance, na kterou odkazuje `E`. Tento člen funkce je určen použitím pravidel pro určení nejvíce odvozené implementace ([virtuální metody](classes.md#virtual-methods)) `M` s ohledem na typ modulu runtime instance, na kterou odkazuje `E`.
     * V opačném případě je `M` členem nevirtuální funkce a člen funkce, který se má vyvolat, je `M`.
   * Je vyvolána implementace členu funkce určená v kroku výše. Objekt, na který odkazuje `E`, se stal objektem, na který odkazuje `this`.

#### <a name="invocations-on-boxed-instances"></a>Vyvolání u zabalených instancí

Člen funkce implementovaný ve *value_type* může být vyvolán prostřednictvím zabalené instance tohoto *value_type* v následujících situacích:

*  Když je člen funkce `override` metody zděděné z typu `object` a vyvolá se pomocí výrazu instance typu `object`.
*  Když je člen funkce implementací člena funkce rozhraní a je vyvolán prostřednictvím výrazu instance *INTERFACE_TYPE*.
*  Když je člen funkce vyvolán prostřednictvím delegáta.

V těchto situacích je zabalená instance považována za obsahující proměnnou *value_type*a tato proměnná se na proměnnou, na kterou odkazuje `this` v rámci vyvolání člena funkce. Konkrétně to znamená, že pokud je člen funkce vyvolán na zabalenou instanci, může člen funkce změnit hodnotu obsaženou v zabalené instanci.

## <a name="primary-expressions"></a>Primární výrazy

Primární výrazy obsahují nejjednodušší formy výrazů.

```antlr
primary_expression
    : primary_no_array_creation_expression
    | array_creation_expression
    ;

primary_no_array_creation_expression
    : literal
    | interpolated_string_expression
    | simple_name
    | parenthesized_expression
    | member_access
    | invocation_expression
    | element_access
    | this_access
    | base_access
    | post_increment_expression
    | post_decrement_expression
    | object_creation_expression
    | delegate_creation_expression
    | anonymous_object_creation_expression
    | typeof_expression
    | checked_expression
    | unchecked_expression
    | default_value_expression
    | nameof_expression
    | anonymous_method_expression
    | primary_no_array_creation_expression_unsafe
    ;
```

Primární výrazy jsou rozdělené mezi *array_creation_expression*s a *primary_no_array_creation_expression*s. Tímto způsobem se s použitím výrazu pro vytvoření pole vytvoří výraz, spíše než jeho výpis spolu s dalšími formuláři jednoduchých výrazů umožňuje, aby gramatika zakázala potenciálně matoucí kód, jako např.
```csharp
object o = new int[3][1];
```
které by jinak bylo interpretováno jako
```csharp
object o = (new int[3])[1];
```

### <a name="literals"></a>Literály

*Primary_expression* , který se skládá z *literálu* ([literály](lexical-structure.md#literals)), je klasifikován jako hodnota.


### <a name="interpolated-strings"></a>Interpolované řetězce

*Interpolated_string_expression* se skládá z znaku `$` následovaného regulárním nebo doslovném řetězcovým literálem, přičemž jsou ohraničeny díry, oddělené `{` a `}`, uzavřít výrazy a specifikace formátování. Výraz interpolovaná řetězcové konstanty je výsledkem *interpolated_string_literal* , který byl rozdělen na jednotlivé tokeny, jak je popsáno v [interpolovaná řetězcové literály](lexical-structure.md#interpolated-string-literals).

```antlr
interpolated_string_expression
    : '$' interpolated_regular_string
    | '$' interpolated_verbatim_string
    ;

interpolated_regular_string
    : interpolated_regular_string_whole
    | interpolated_regular_string_start interpolated_regular_string_body interpolated_regular_string_end
    ;

interpolated_regular_string_body
    : interpolation (interpolated_regular_string_mid interpolation)*
    ;

interpolation
    : expression
    | expression ',' constant_expression
    ;

interpolated_verbatim_string
    : interpolated_verbatim_string_whole
    | interpolated_verbatim_string_start interpolated_verbatim_string_body interpolated_verbatim_string_end
    ;

interpolated_verbatim_string_body
    : interpolation (interpolated_verbatim_string_mid interpolation)+
    ;
```

*Constant_expression* v interpolaci musí mít implicitní převod na `int`.

*Interpolated_string_expression* je klasifikován jako hodnota. Pokud je okamžitě převedena na `System.IFormattable` nebo `System.FormattableString` s implicitním interpolacem interpolovaná řetězcové konverzemi ([implicitní interpolované převody řetězců](conversions.md#implicit-interpolated-string-conversions)), výraz řetězce má tento typ. V opačném případě má typ `string`.

Pokud je typ interpolované řetězcové `System.IFormattable` nebo `System.FormattableString`, znamená to, že se jedná o volání `System.Runtime.CompilerServices.FormattableStringFactory.Create`. Pokud je typ `string`, význam výrazu je volání `string.Format`. V obou případech se seznam argumentů volání skládá z řetězcového literálu formátu se zástupnými symboly pro každou interpolaci a argument pro každý výraz odpovídající držitelům umístění.

Řetězcový literál formátu je konstruován takto, kde `N` je počet interpolů v *interpolated_string_expression*:

*  Pokud *interpolated_regular_string_whole* nebo *interpolated_verbatim_string_whole* následuje symbol `$`, pak je řetězcový literál formátu tímto tokenem.
*  V opačném případě se řetězcový literál formátu skládá z: 
   *  První *interpolated_regular_string_start* nebo *interpolated_verbatim_string_start*
   *  Potom pro každé číslo `I` od `0` do `N-1`: 
      * Desítková reprezentace `I`
      * V případě, že odpovídající *interpolace* má *constant_expression*, je `,` (čárka) následovaná desítkovou reprezentací hodnoty *constant_expression* .
      * Pak *interpolated_regular_string_mid*, *interpolated_regular_string_end*, *interpolated_verbatim_string_mid* nebo *interpolated_verbatim_string_end* hned za odpovídající interpolaci.

Následující argumenty jsou jednoduše *výrazy* z *interpolace* (pokud existují), v daném pořadí.

TODO: příklady


### <a name="simple-names"></a>Jednoduché názvy

*Simple_name* se skládá z identifikátoru, volitelně následovaný seznamem argumentů typu:

```antlr
simple_name
    : identifier type_argument_list?
    ;
```

*Simple_name* je jedna z těchto formulářů `I` nebo z formuláře `I<A1,...,Ak>`, kde `I` je jeden identifikátor a `<A1,...,Ak>` je volitelná *type_argument_list*. Pokud není zadán žádný *type_argument_list* , zvažte `K`, aby byla nulová. *Simple_name* je vyhodnocen a klasifikován následujícím způsobem:

*  Pokud je hodnota `K` nula a *simple_name* se zobrazí v *bloku* a v případě *, že*prostor deklarace místní proměnné (nebo ohraničujícího *bloku*) ([deklarace](basic-concepts.md#declarations)) obsahuje místní proměnnou, parametr nebo konstantu s hodnotou název @ no__t-6, potom *simple_name* odkazuje na tuto místní proměnnou, parametr nebo konstantu a je klasifikován jako proměnná nebo hodnota.
*  Pokud `K` je nula a *simple_name* se zobrazí v těle deklarace obecné metody a pokud tato deklarace obsahuje parametr typu s názvem @ no__t-2, pak *simple_name* odkazuje na tento parametr typu.
*  V opačném případě pro každý typ instance @ no__t-0 ([typ instance](classes.md#the-instance-type)), počínaje typem instance bezprostředně ohraničujícího typ deklarace a pokračování s typem instance každé nadřazené třídy nebo deklarace struktury (pokud existuje):
   *  Pokud je `K` nula a deklarace `T` obsahuje parametr typu s názvem @ no__t-2, pak *simple_name* odkazuje na tento parametr typu.
   *  V opačném případě, pokud vyhledávání členů ([vyhledávání členů](expressions.md#member-lookup)) `I` v `T` s argumenty `K` @ no__t-4Type vytvoří shodu:
      * Pokud `T` je typ instance bezprostředně ohraničujícího typu třídy nebo struktury a vyhledávání identifikuje jednu nebo více metod, výsledkem je skupina metod s přidruženým výrazem instance `this`. Pokud byl zadán seznam argumentů typu, je použit při volání obecné metody ([vyvolání metod](expressions.md#method-invocations)).
      * V opačném případě, pokud `T` je typ instance bezprostředně ohraničujícího typu třídy nebo struktury, pokud vyhledávání identifikuje člen instance a v případě, že k odkazu dojde v těle konstruktoru instance, metody instance nebo přistupujícího objektu instance, výsledek je stejný jako členský přístup ([členský přístup](expressions.md#member-access)) formuláře `this.I`. K tomu může dojít pouze v případě, že je hodnota `K` nulová.
      * V opačném případě je výsledek stejný jako členský přístup ([členský přístup](expressions.md#member-access)) formuláře `T.I` nebo `T.I<A1,...,Ak>`. V tomto případě se jedná o chybu při vazbě, kterou *simple_name* odkazuje na člen instance.

*  V opačném případě pro každý obor názvů @ no__t-0 počínaje oborem názvů, ve kterém dojde k *simple_name* , pokračuje v každém ohraničujícím oboru názvů (pokud existuje) a končí globálním oborem názvů, jsou vyhodnoceny následující kroky, dokud není nalezena entita:
   *  Pokud je `K` nula a `I` je název oboru názvů v @ no__t-2, pak:
      * Pokud je umístění, kde se nachází *simple_name* , uzavřeno deklarací oboru názvů pro `N` a deklarace oboru názvů obsahuje *extern_alias_directive* nebo *using_alias_directive* , který přidruží název @ no__t-4 k obor názvů nebo typ, *simple_name* je nejednoznačný a dojde k chybě při kompilaci.
      * V opačném případě *simple_name* odkazuje na obor názvů s názvem `I` v `N`.
   *  Jinak, pokud `N` obsahuje přístupný typ s parametry Name @ no__t-1 a `K` @ no__t-3type, pak:
      * Pokud je `K` nula a umístění, kde se nachází *simple_name* , je uzavřeno deklarací oboru názvů pro `N` a deklarace oboru názvů obsahuje *extern_alias_directive* nebo *using_alias_directive* , který přidruží k název @ no__t-5 s oborem názvů nebo typem, *simple_name* je nejednoznačný a dojde k chybě při kompilaci.
      * V opačném případě *namespace_or_type_name* odkazuje na typ sestavený pomocí daných argumentů typu.
   *  V opačném případě, pokud je umístění, kde se nachází *simple_name* , uzavřeno deklarací oboru názvů pro @ no__t-1:
      * Pokud je `K` nula a deklarace oboru názvů obsahuje *extern_alias_directive* nebo *using_alias_directive* , který přidruží název @ no__t-3 k importovanému oboru názvů nebo typu, pak *simple_name* odkazuje na tento obor názvů nebo textový.
      * V opačném případě, pokud obory názvů a deklarace typů importované pomocí *using_namespace_directive*s a *using_static_directive*s deklarací oboru názvů obsahují přesně jeden přístupný typ nebo statický člen bez přípony s názvem @ no__ parametry t-2 a `K` @ no__t-4Type, pak *simple_name* odkazuje na tento typ nebo člen vytvořený pomocí daných argumentů typu.
      * V opačném případě, pokud obory názvů a typy importované do *using_namespace_directive*s deklarací oboru názvů obsahují více než jeden přístupný typ nebo statický člen nerozšiřující metody, který má název @ no__t-1 a `K` @ no__t-3type parametry, *simple_name* je nejednoznačný a dojde k chybě.

   Všimněte si, že celý krok je přesně rovnoběžný s odpovídajícím krokem při zpracování *namespace_or_type_name* ([obor názvů a názvy typů](basic-concepts.md#namespace-and-type-names)).

*  V opačném případě *simple_name* není definován a dojde k chybě při kompilaci.


### <a name="parenthesized-expressions"></a>Výrazy v závorkách

*Parenthesized_expression* se skládá z *výrazu* uzavřeného v závorkách.

```antlr
parenthesized_expression
    : '(' expression ')'
    ;
```

*Parenthesized_expression* se vyhodnocuje vyhodnocením *výrazu* v závorkách. Pokud *výraz* v závorkách označuje obor názvů nebo typ, dojde k chybě při kompilaci. V opačném případě výsledek *parenthesized_expression* je výsledkem vyhodnocení obsaženého *výrazu*.

### <a name="member-access"></a>Přístup ke členům

*Member_access* se skládá z *primary_expression*, *predefined_type*nebo *qualified_alias_member*, po kterém následuje token "`.`" následovaný *identifikátorem*, volitelně následovaným *type_argument_list* .

```antlr
member_access
    : primary_expression '.' identifier type_argument_list?
    | predefined_type '.' identifier type_argument_list?
    | qualified_alias_member '.' identifier
    ;

predefined_type
    : 'bool'   | 'byte'  | 'char'  | 'decimal' | 'double' | 'float' | 'int' | 'long'
    | 'object' | 'sbyte' | 'short' | 'string'  | 'uint'   | 'ulong' | 'ushort'
    ;
```

Výroba *qualified_alias_member* je definována v [kvalifikátorech aliasů oboru názvů](namespaces.md#namespace-alias-qualifiers).

*Member_access* je jedna z těchto formulářů `E.I` nebo z formuláře `E.I<A1, ..., Ak>`, kde `E` je primární výraz, `I` je jeden identifikátor a `<A1, ..., Ak>` je volitelná *type_argument_list*. Pokud není zadán žádný *type_argument_list* , zvažte `K`, aby byla nulová.

*Member_access* s *primary_expression* typu `dynamic` je dynamicky vázaná ([dynamická vazba](expressions.md#dynamic-binding)). V tomto případě kompilátor klasifikuje přístup členů jako vlastnost přístupu typu `dynamic`. Níže uvedená pravidla určující význam *member_access* se pak použijí v době běhu, a to pomocí běhového typu místo při kompilaci *primary_expression*. Pokud tato klasifikace za běhu vede na skupinu metod, musí být přístup člena *primary_expression* typu *invocation_expression*.

*Member_access* je vyhodnocen a klasifikován následujícím způsobem:

*  Pokud je `K` nula a `E` je obor názvů a `E` obsahuje vnořený obor názvů s názvem @ no__t-3, pak je výsledkem tento obor názvů.
*  V opačném případě, pokud `E` je obor názvů a `E` obsahuje přístupný typ s parametry Name @ no__t-2 a `K` @ no__t-4Type, pak je výsledkem typ sestavený pomocí daných argumentů typu.
*  Pokud je `E` *predefined_type* nebo *primary_expression* klasifikovaný jako typ, pokud `E` není parametr typu, a pokud vyhledávání členů ([vyhledávání členů](expressions.md#member-lookup)) `I` v `E` @no__t s parametrem-7 @ no__t-8type vytvoří shodu, pak je `E.I` vyhodnocena a klasifikována následujícím způsobem:
   *  Pokud `I` identifikuje typ, pak je výsledkem typ konstruovaný pomocí daných argumentů typu.
   *  Pokud `I` identifikuje jednu nebo více metod, pak je výsledkem skupina metod bez přidruženého výrazu instance. Pokud byl zadán seznam argumentů typu, je použit při volání obecné metody ([vyvolání metod](expressions.md#method-invocations)).
   *  Pokud `I` identifikuje vlastnost `static`, pak je výsledkem přístup k vlastnosti bez přidruženého výrazu instance.
   *  Pokud `I` identifikuje pole `static`:
      * Pokud je pole `readonly` a odkaz se nachází mimo statický konstruktor třídy nebo struktury, ve které je pole deklarováno, pak je výsledkem hodnota, konkrétně hodnota statického pole @ no__t-1 v @ no__t-2.
      * V opačném případě je výsledkem proměnná, konkrétně pole static @ no__t-0 v @ no__t-1.
   *  Pokud `I` identifikuje událost `static`:
      * Pokud se odkaz vyskytuje v rámci třídy nebo struktury, ve které je událost deklarována, a událost byla deklarována bez *event_accessor_declarations* ([events](classes.md#events)), pak `E.I` je zpracována přesně, jako by byla `I` statická pole.
      * V opačném případě je výsledkem přístup k události bez přidruženého výrazu instance.
   *  Pokud `I` identifikuje konstantu, pak je výsledkem hodnota, jmenovitě hodnota této konstanty.
    * Pokud `I` identifikuje člen výčtu, pak je výsledkem hodnota, konkrétně hodnota tohoto člena výčtu.
    * V opačném případě je `E.I` neplatný odkaz na člena a dojde k chybě při kompilaci.
*  Pokud `E` je přístup k vlastnostem, přístup k indexeru, proměnná nebo hodnota, typ, který je @ no__t-1 a vyhledávání členů ([vyhledávání členů](expressions.md#member-lookup)) `I` v `T` s argumenty `K` @ no__t-6type, je vyhodnocena a pak `E.I`. klasifikované následujícím způsobem:
   *  První, pokud `E` je vlastnost nebo přístup indexeru, je získána hodnota vlastnosti nebo přístupu indexeru ([hodnoty výrazů](expressions.md#values-of-expressions)) a `E` je překlasifikována jako hodnota.
   *  Pokud `I` identifikuje jednu nebo více metod, pak je výsledkem skupina metod s přidruženým výrazem instance `E`. Pokud byl zadán seznam argumentů typu, je použit při volání obecné metody ([vyvolání metod](expressions.md#method-invocations)).
   *  Pokud `I` identifikuje vlastnost instance,
      * Pokud je `E` `this`, `I` identifikuje automaticky implementovanou vlastnost ([automaticky implementované vlastnosti](classes.md#automatically-implemented-properties)) bez metody setter a odkaz se objeví v rámci konstruktoru instance pro třídu nebo typ struktury `T`, pak výsledek je proměnná, konkrétně skryté pole zálohování pro automatickou vlastnost zadanou parametrem `I` v instanci `T`, kterou používá `this`.
      * V opačném případě je výsledkem přístup k vlastnosti s přidruženým výrazem instance @ no__t-0.
   *  Pokud `T` je *class_type* a `I` identifikuje pole instance daného *class_type*:
      * Pokud je hodnota `E` `null`, je vyvolána `System.NullReferenceException`.
      * V opačném případě, pokud je pole `readonly` a odkaz se vyskytuje mimo konstruktor instance třídy, ve které je pole deklarováno, pak výsledek je hodnota, jmenovitá hodnota pole @ no__t-1 v objektu, na který odkazuje @ no__t-2.
      * V opačném případě výsledkem je proměnná, konkrétně pole @ no__t-0 v objektu, na který odkazuje @ no__t-1.
   *  Pokud `T` je *struct_type* a `I` identifikuje pole instance daného *struct_type*:
      * Pokud je `E` hodnota, nebo pokud je pole `readonly` a odkaz se objeví mimo konstruktor instance struktury, ve které je pole deklarováno, pak je výsledkem hodnota, konkrétně hodnota pole @ no__t-2 v instanci struktury, kterou má hodnota @ no__t-3.
      * V opačném případě výsledkem je proměnná, konkrétně pole @ no__t-0 v instanci struct dané @ no__t-1.
   *  Pokud `I` identifikuje událost instance:
      * Pokud se odkaz vyskytuje v rámci třídy nebo struktury, ve které je událost deklarována, a událost byla deklarována bez *event_accessor_declarations* ([events](classes.md#events)) a odkaz se nevyskytuje jako levá strana operátoru `+=` nebo `-=` a pak je `E.I` zpracována přesně, jako by `I` bylo polem instance.
      * V opačném případě je výsledkem přístup k události s přidruženým výrazem instance @ no__t-0.
*  V opačném případě se pokusí zpracovat `E.I` jako volání rozšiřující metody ([vyvolání rozšiřující metody](expressions.md#extension-method-invocations)). Pokud se to nepovede, `E.I` je neplatný odkaz na člena a dojde k chybě při vazbě.

#### <a name="identical-simple-names-and-type-names"></a>Stejné jednoduché názvy a názvy typů

V případě členství ve formuláři `E.I`, pokud `E` je jediný identifikátor, a pokud je význam `E` jako *simple_name* ([jednoduché názvy](expressions.md#simple-names)) konstanta, pole, vlastnost, místní proměnná nebo parametr se stejným typem jako hodnota `E` jako *TYPE_NAME* ([obor názvů a názvy typů](basic-concepts.md#namespace-and-type-names)), jsou povoleny oba možné významy `E`. Dvě možné významy `E.I` nikdy nejednoznačné, protože `I` musí být nutně členem typu `E` v obou případech. Jinými slovy pravidlo jednoduše povoluje přístup ke statickým členům a vnořeným typům `E`, kde by jinak došlo k chybě při kompilaci. Příklad:
```csharp
struct Color
{
    public static readonly Color White = new Color(...);
    public static readonly Color Black = new Color(...);

    public Color Complement() {...}
}

class A
{
    public Color Color;                // Field Color of type Color

    void F() {
        Color = Color.Black;           // References Color.Black static member
        Color = Color.Complement();    // Invokes Complement() on Color field
    }

    static void G() {
        Color c = Color.White;         // References Color.White static member
    }
}
```

#### <a name="grammar-ambiguities"></a>Nejednoznačnost gramatiky

Výroby pro *simple_name* ([jednoduché názvy](expressions.md#simple-names)) a *member_access* ([přístup do členů](expressions.md#member-access)) mohou vést k nejednoznačnosti v gramatice pro výrazy. Například příkaz:
```csharp
F(G<A,B>(7));
```
může být interpretován jako volání `F` se dvěma argumenty, `G < A` a `B > (7)`. Alternativně může být interpretován jako volání `F` s jedním argumentem, což je volání obecné metody @ no__t-1 se dvěma argumenty typu a jedním regulárním argumentem.

Pokud je možné sekvenci tokenů analyzovat (v kontextu) jako *simple_name* ([jednoduché názvy](expressions.md#simple-names)), *member_access* ([přístup ke členu](expressions.md#member-access)) nebo *pointer_member_access* ([Přístup člena ukazatele](unsafe-code.md#pointer-member-access)) končící na *type_argument_ list* ([argumenty typu](types.md#type-arguments)): vyhodnotí se token hned za uzavíracím tokenem `>`. Pokud je jeden z
```csharp
(  )  ]  }  :  ;  ,  .  ?  ==  !=  |  ^
```
*type_argument_list* se pak uchová jako součást *simple_name*, *member_access* nebo *pointer_member_access* a jakékoli další možné analýzy sekvence tokenů se zahodí. V opačném případě se *type_argument_list* nepovažuje za součást *simple_name*, *member_access* nebo *pointer_member_access*, a to i v případě, že není k dispozici žádné jiné možné analýzy sekvence tokenů. Všimněte si, že tato pravidla nejsou aplikována při analýze *type_argument_list* v *namespace_or_type_name* ([obor názvů a názvy typů](basic-concepts.md#namespace-and-type-names)). Příkaz
```csharp
F(G<A,B>(7));
```
bude podle tohoto pravidla interpretován jako volání `F` s jedním argumentem, což je volání obecné metody `G` se dvěma argumenty typu a jedním regulárním argumentem. Příkazy
```csharp
F(G < A, B > 7);
F(G < A, B >> 7);
```
všechna budou interpretována jako volání `F` se dvěma argumenty. Příkaz
```csharp
x = F < A > +y;
```
bude interpretován jako operátor menší než, operátor větší než a unární operátor plus, jako kdyby byl příkaz napsán `x = (F < A) > (+y)` namísto jako *simple_name* s *type_argument_list* následovaným binárním operátorem Plus. V příkazu
```csharp
x = y is C<T> + z;
```
tokeny `C<T>` jsou interpretovány jako *namespace_or_type_name* s *type_argument_list*.

### <a name="invocation-expressions"></a>Výrazy vyvolání

*Invocation_expression* se používá k vyvolání metody.

```antlr
invocation_expression
    : primary_expression '(' argument_list? ')'
    ;
```

*Invocation_expression* je dynamicky svázán ([dynamická vazba](expressions.md#dynamic-binding)), pokud alespoň jedna z následujících možností obsahuje:

* *Primary_expression* má typ doby kompilace `dynamic`.
* Nejméně jeden argument volitelného *argument_list* má typ doby kompilace `dynamic` a *primary_expression* nemá typ delegáta.

V tomto případě kompilátor klasifikuje *invocation_expression* jako hodnotu typu `dynamic`. Níže uvedená pravidla určující význam *invocation_expression* se pak použijí v době běhu, a to pomocí běhového typu místo za kompilace pro *primary_expression* a argumenty, které mají typ doby kompilace @no_ _T – 2. Pokud *primary_expression* nemá typ doby kompilace `dynamic`, pak volání metody bude mít za následek omezené doby kompilace, jak je popsáno v tématu [Kontrola dynamického přetěžování při kompilaci](expressions.md#compile-time-checking-of-dynamic-overload-resolution).

*Primary_expression* *invocation_expression* musí být skupinou metod nebo hodnotou *delegate_type*. Pokud je *primary_expression* skupinou metod, *invocation_expression* je volání metody ([vyvolání](expressions.md#method-invocations)metod). Pokud je *primary_expression* hodnotou *delegate_type*, *invocation_expression* je volání delegáta ([vyvolání delegátů](expressions.md#delegate-invocations)). Pokud *primary_expression* není ani skupina metod ani hodnota *delegate_type*, dojde k chybě při vazbě.

Volitelné *argument_list* ([seznamy argumentů](expressions.md#argument-lists)) poskytují hodnoty nebo odkazy na proměnné pro parametry metody.

Výsledek vyhodnocení *invocation_expression* je klasifikován následujícím způsobem:

*  Pokud *invocation_expression* vyvolá metodu nebo delegáta, který vrací `void`, výsledek není Nothing. Výraz, který je klasifikován jako Nothing, je povolen pouze v kontextu *statement_expression* ([příkazy výrazu](statements.md#expression-statements)) nebo jako tělo *lambda_expression* ([anonymní výrazy Functions](expressions.md#anonymous-function-expressions)). V opačném případě dojde k chybě při vazbě.
*  V opačném případě je výsledkem hodnota typu vráceného metodou nebo delegátem.

#### <a name="method-invocations"></a>Vyvolání metod

Pro vyvolání metody musí být *primary_expression* metody *invocation_expression* skupinou metod. Skupina metod identifikuje jednu metodu, která se má vyvolat, nebo sadu přetížených metod, ze kterých se má vybrat konkrétní metoda, která se má vyvolat. V druhém případě je určení konkrétní metody vyvolání odvozeno z kontextu poskytnutého typy argumentů v *argument_list*.

Zpracování volání metody formuláře `M(A)`, kde `M` je skupina metod (případně včetně *type_argument_list*) a `A` je volitelná *argument_list*, sestává z následujících kroků:

*  Sada kandidátních metod pro vyvolání metody je vytvořena. Pro každou metodu `F` přidruženou ke skupině metod `M`:
   *  Pokud je `F` neobecné, `F` je kandidátem v těchto případech:
      * `M` nemá žádný typ seznamu argumentů a
      * `F` platí pro `A` ([platný člen funkce](expressions.md#applicable-function-member)).
   *  Pokud je `F` obecný a `M` nemá žádný seznam argumentů typu, `F` je kandidátem v těchto případech:
      * Odvození typu ([odvození typu](expressions.md#type-inference)) je úspěšné, odvození seznamu argumentů typu pro volání a
      * Jakmile jsou argumenty odvozených typů nahrazeny odpovídajícími parametry typu metody, všechny vytvořené typy v seznamu parametrů F splní svá omezení ([splňující omezení](types.md#satisfying-constraints)) a seznam parametrů `F` lze použít s v souvislosti s `A` ([platný člen funkce](expressions.md#applicable-function-member)).
   *  Pokud je `F` obecný a `M` obsahuje seznam argumentů typu, `F` je kandidátem v těchto případech:
      * `F` má stejný počet parametrů typu metody, které byly zadány v seznamu argumentů typu.
      * Jakmile jsou argumenty typu nahrazeny odpovídajícími parametry typu metody, všechny vytvořené typy v seznamu parametrů F splní svá omezení ([splňující omezení](types.md#satisfying-constraints)) a seznam parametrů `F` je použitelný s ohledem na `A` ([platný člen funkce](expressions.md#applicable-function-member)).
*  Sada kandidátních metod je zmenšena tak, aby obsahovala pouze metody z nejvíc odvozených typů: Pro každou metodu `C.F` v sadě, kde `C` je typ, ve kterém je metoda `F` deklarována, všechny metody deklarované v základním typu `C` jsou ze sady odebrány. Kromě toho, pokud `C` je typ třídy jiný než `object`, všechny metody deklarované v typu rozhraní jsou ze sady odebrány. (Toto druhé pravidlo má vliv pouze v případě, že skupina metod byla výsledkem vyhledávání členů u parametru typu, který má platnou základní třídu jinou než objekt a neprázdnou sadu efektivních rozhraní.)
*  Pokud je výsledná sada kandidátních metod prázdná, pak je další zpracování v následujících krocích opuštěno a místo toho je proveden pokus o zpracování vyvolání jako volání metody rozšíření ([vyvolání rozšiřující metody](expressions.md#extension-method-invocations)). Pokud se to nepovede, neexistují žádné použitelné metody a dojde k chybě při vazbě.
*  Nejlepší metoda sady kandidátních metod je identifikována pomocí pravidel řešení přetížení v rámci [rozlišení přetížení](expressions.md#overload-resolution). Pokud nelze identifikovat jednu nejlepší metodu, volání metody je nejednoznačné a dojde k chybě při vazbě. Při provádění řešení přetížení jsou parametry obecné metody zváženy po nahrazení argumentů typu (dodaných nebo odvozených) pro odpovídající parametry typu metody.
*  Konečné ověření zvolené nejlepší metody je provedeno:
   * Metoda je ověřena v kontextu skupiny metod: Pokud je nejlepší metodou statická metoda, skupina metod musí být výsledkem *simple_name* nebo *member_access* prostřednictvím typu. Pokud je nejlepším způsobem metoda instance, skupina metod musí mít výsledek z *simple_name*, *member_access* prostřednictvím proměnné nebo hodnoty nebo *base_access*. Pokud ani jedna z těchto požadavků není pravdivá, dojde k chybě při vazbě.
   * Pokud je nejlepší metodou obecná metoda, jsou argumenty typu (dodané nebo odvozené) zkontrolovány proti omezením ([vyhovujícím omezením](types.md#satisfying-constraints)) deklarovaným v obecné metodě. Pokud jakýkoli argument typu nesplňuje odpovídající omezení pro parametr typu, dojde k chybě při vazbě.

Jakmile je metoda vybrána a ověřována v době vytvoření vazby výše uvedenými kroky, je skutečné vyvolání za běhu zpracováno podle pravidel volání člena funkce popsaných v tématu [Kontrola dynamického přetěžování při kompilaci](expressions.md#compile-time-checking-of-dynamic-overload-resolution).

Intuitivní efekt výše popsaných pravidel řešení je následující: Chcete-li najít konkrétní metodu volanou voláním metody, začněte typem označeným voláním metody a přejděte do řetězce dědičnosti, dokud není nalezena alespoň jedna platná, dostupná, nepřepisovaná deklarace metody. Pak proveďte odvození typu a rozlišení přetěžování v sadě použitelných, přístupných metod, které nejsou deklarovány v tomto typu a zavolejte metodu, která je vybrána. Pokud nebyla nalezena žádná metoda, zkuste místo toho zpracovat vyvolání jako volání metody rozšíření.

#### <a name="extension-method-invocations"></a>Vyvolání metod rozšíření

V volání metody ([vyvolání u zabalených instancí](expressions.md#invocations-on-boxed-instances)) jednoho z formulářů
```csharp
expr . identifier ( )

expr . identifier ( args )

expr . identifier < typeargs > ( )

expr . identifier < typeargs > ( args )
```
Pokud normální zpracování vyvolání nenajde žádné použitelné metody, je proveden pokus o zpracování konstrukce jako volání metody rozšíření. Pokud *expr* nebo kterákoli z *argumentů* má typ doby kompilace `dynamic`, metody rozšíření nebudou použity.

Cílem je najít nejlepší *type_name* `C`, aby mohlo dojít k odpovídajícím voláním statické metody:
```csharp
C . identifier ( expr )

C . identifier ( expr , args )

C . identifier < typeargs > ( expr )

C . identifier < typeargs > ( expr , args )
```

Metoda rozšíření `Ci.Mj` je ***způsobila*** v případě, že:

*  `Ci` je neobecná nevnořená třída.
*  Název `Mj` je *identifikátor*
*  `Mj` je přístupná a použitelná při použití na argumenty jako statická metoda, jak je uvedeno výše.
*  Implicitní identita, odkaz nebo převod zabalení existují z *výrazu expr* na typ prvního parametru `Mj`.

Hledání `C` pokračuje následujícím způsobem:

*  Počínaje rozhraním nejbližší ohraničující deklarace oboru názvů pokračuje v každé deklaraci ohraničujícího oboru názvů a končí obsahující kompilační jednotka, pro nalezení kandidátních metod rozšíření je možné provést následující pokusy:
   * Pokud zadaný obor názvů nebo kompilační jednotka přímo obsahuje deklarace neobecného typu `Ci` s oprávněnými metodami rozšíření `Mj`, pak sada těchto metod rozšíření je kandidátská sada.
   * Pokud typy `Ci` importované pomocí *using_static_declarations* a přímo deklarované v oborech názvů importovaných pomocí *using_namespace_directive*s v daném oboru názvů nebo jednotce kompilace přímo obsahují opravňující metody rozšíření `Mj`, pak sada těchto rozšiřujících metod je kandidátská sada.
*  Pokud se v žádné ohraničující deklaraci oboru názvů nebo jednotce kompilace nenajde žádná sada kandidátů, dojde k chybě při kompilaci.
*  V opačném případě se rozlišení přetěžování aplikuje na sadu kandidátů, jak je popsáno v tématu ([řešení přetížení](expressions.md#overload-resolution)). Pokud se nenajde žádná nejlepší metoda, dojde k chybě při kompilaci.
*  `C` je typ, ve kterém je nejlepší metoda deklarována jako metoda rozšíření.

Pokud jako cíl použijete `C`, volání metody se pak zpracuje jako volání statické metody ([při kontrole dynamického překladu přetěžování za kompilace](expressions.md#compile-time-checking-of-dynamic-overload-resolution)).

Předchozí pravidla znamenají, že metody instance mají přednost před rozšiřujícími metodami, tyto metody rozšíření dostupné v deklaracích vnitřních oborů názvů mají přednost před rozšiřujícími metodami dostupnými v deklaracích vnějších oborů názvů a s tímto rozšířením. metody deklarované přímo v oboru názvů mají přednost před metodami rozšíření importovanými do stejného oboru názvů s použitím direktivy Namespace. Příklad:
```csharp
public static class E
{
    public static void F(this object obj, int i) { }

    public static void F(this object obj, string s) { }
}

class A { }

class B
{
    public void F(int i) { }
}

class C
{
    public void F(object obj) { }
}

class X
{
    static void Test(A a, B b, C c) {
        a.F(1);              // E.F(object, int)
        a.F("hello");        // E.F(object, string)

        b.F(1);              // B.F(int)
        b.F("hello");        // E.F(object, string)

        c.F(1);              // C.F(object)
        c.F("hello");        // C.F(object)
    }
}
```

V příkladu má metoda `B` přednost před první metodou rozšíření a metoda `C` má přednost před oběma metodami rozšíření.

```csharp
public static class C
{
    public static void F(this int i) { Console.WriteLine("C.F({0})", i); }
    public static void G(this int i) { Console.WriteLine("C.G({0})", i); }
    public static void H(this int i) { Console.WriteLine("C.H({0})", i); }
}

namespace N1
{
    public static class D
    {
        public static void F(this int i) { Console.WriteLine("D.F({0})", i); }
        public static void G(this int i) { Console.WriteLine("D.G({0})", i); }
    }
}

namespace N2
{
    using N1;

    public static class E
    {
        public static void F(this int i) { Console.WriteLine("E.F({0})", i); }
    }

    class Test
    {
        static void Main(string[] args)
        {
            1.F();
            2.G();
            3.H();
        }
    }
}
```

Výstup tohoto příkladu je:
```console
E.F(1)
D.G(2)
C.H(3)
```
`D.G` má přednost před `C.G` a `E.F` má přednost před `D.F` a `C.F`.

#### <a name="delegate-invocations"></a>Volání delegátů

Pro vyvolání delegáta musí být *primary_expression* hodnoty *invocation_expression* hodnotou *delegate_type*. Vzhledem k tomu, že je *delegate_type* členem funkce se stejným seznamem parametrů jako *delegate_type*, musí být *delegate_type* platný ([platný člen funkce](expressions.md#applicable-function-member)) s ohledem na *argument_ seznam* *invocation_expression*

Běhové zpracování volání delegáta ve formě `D(A)`, kde `D` je *primary_expression* *delegate_type* a `A` je volitelná *argument_list*, skládá se z následujících kroků:

*  je vyhodnocena hodnota `D`. Pokud toto vyhodnocení způsobí výjimku, nejsou provedeny žádné další kroky.
*  Hodnota `D` je zkontrolována jako platná. Pokud je hodnota `D` `null`, je vyvolána `System.NullReferenceException` a nejsou provedeny žádné další kroky.
*  V opačném případě `D` odkaz na instanci delegáta. Volání členů funkce ([Kontrola dynamického přetěžování při kompilaci](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) se provádí na každé z těchto entit v seznamu volání delegáta. Pro volatelné entity skládající se z instance a metody instance je instance vyvolání instance obsažena v entitě, která se nedá volat.

### <a name="element-access"></a>Přístup k prvkům

*Element_access* se skládá z *primary_no_array_creation_expression*, po kterém následuje token "`[`" následovaný *argument_listem*, za kterým následuje token "`]`". *Argument_list* se skládá z jednoho nebo více *argumentů*, které jsou odděleny čárkami.

```antlr
element_access
    : primary_no_array_creation_expression '[' expression_list ']'
    ;
```

*Argument_list* *element_access* není povoleno obsahovat argumenty `ref` nebo `out`.

*Element_access* je dynamicky svázán ([dynamická vazba](expressions.md#dynamic-binding)), pokud alespoň jedna z následujících možností obsahuje:

* *Primary_no_array_creation_expression* má typ doby kompilace `dynamic`.
* Nejméně jeden výraz *argument_list* má typ doby kompilace `dynamic` a *primary_no_array_creation_expression* nemá typ pole.

V tomto případě kompilátor klasifikuje *element_access* jako hodnotu typu `dynamic`. Níže uvedená pravidla určující význam *element_access* se pak použijí za běhu za běhu za použití běhového typu místo pro typ doby kompilace pro *primary_no_array_creation_expression* a *argument_list* . výrazy, které mají typ pro dobu kompilace `dynamic`. Pokud *primary_no_array_creation_expression* nemá typ pro dobu kompilace `dynamic`, pak přístup k elementu bude podstoupit omezená doba kompilace, jak je popsáno v tématu [Kontrola dynamického přetěžování přetížení v době kompilace](expressions.md#compile-time-checking-of-dynamic-overload-resolution).

Pokud je *primary_no_array_creation_expression* typu *element_access* hodnotou *array_type*, *element_access* je přístup k poli ([přístup k poli](expressions.md#array-access)). V opačném případě musí být *primary_no_array_creation_expression* proměnnou nebo hodnotou třídy, struktury nebo typu rozhraní, která má jednoho nebo více členů indexeru. v takovém případě je *element_access* přístup k indexeru ([přístup indexeru](expressions.md#indexer-access)).

#### <a name="array-access"></a>Přístup k poli

Pro přístup k poli musí být *primary_no_array_creation_expression* hodnoty *element_access* hodnotou *array_type*. Kromě toho nepovoluje *argument_list* přístupu k poli obsahovat pojmenované argumenty. Počet výrazů v *argument_list* musí být stejný jako pořadí *array_type*a každý výraz musí být typu `int`, `uint`, `long`, `ulong`, nebo musí být implicitně převoditelné na jeden nebo více těchto typů.

Výsledek vyhodnocení přístupu k poli je proměnná typu prvku pole, jmenovitým prvkem pole vybraným hodnotami (y) výrazů v *argument_list*.

Běhové zpracování přístupu k poli ve formátu `P[A]`, kde `P` je *primary_no_array_creation_expression* *array_type* a `A` je *argument_list*, skládá se z následujících kroků:

*  je vyhodnocena hodnota `P`. Pokud toto vyhodnocení způsobí výjimku, nejsou provedeny žádné další kroky.
*  Výrazy indexu *argument_list* jsou vyhodnocovány v pořadí zleva doprava. Po vyhodnocení každého indexového výrazu se provede implicitní převod ([implicitní převody](conversions.md#implicit-conversions)) na jeden z následujících typů: `int`, `uint`, `long`, `ulong`. První typ v tomto seznamu, pro který existuje implicitní převod. Například pokud je Indexový výraz typu `short`, pak je proveden implicitní převod na `int`, protože implicitní převody z `short` na `int` a z `short` na `long` jsou možné. Pokud vyhodnocení výrazu indexu nebo následný implicitní převod způsobí výjimku, nejsou vyhodnoceny žádné další indexové výrazy a nejsou spuštěny žádné další kroky.
*  Hodnota `P` je zkontrolována jako platná. Pokud je hodnota `P` `null`, je vyvolána `System.NullReferenceException` a nejsou provedeny žádné další kroky.
*  Hodnota každého výrazu v *argument_list* je zkontrolována proti skutečným hranicím jednotlivých dimenzí instance pole, na kterou odkazuje `P`. Pokud jedna nebo více hodnot je mimo rozsah, je vyvolána `System.IndexOutOfRangeException` a nejsou provedeny žádné další kroky.
*  Je vypočítáno umístění elementu pole uvedeného v indexových výrazech a toto umístění se projeví v důsledku přístupu k poli.

#### <a name="indexer-access"></a>Přístup indexeru

Pro přístup indexeru musí být *primary_no_array_creation_expression* objektu *element_access* proměnnou nebo hodnotou třídy, struktury nebo typu rozhraní a tento typ musí implementovat jeden nebo více indexerů, které jsou použitelné s ohledem na *argument_list* *element_access*.

Zpracování přístupu indexeru z formuláře `P[A]`, kde `P` je *primary_no_array_creation_expression* typu třídy, struktury nebo rozhraní `T` a `A` je *argument_list*, skládá se z následujících uvedené

*  Sada indexerů, kterou poskytuje `T`, je vytvořena. Sada se skládá ze všech indexerů deklarovaných v `T` nebo základní typ `T`, které nejsou deklaracemi `override` a jsou přístupné v aktuálním kontextu ([přístup ke členům](basic-concepts.md#member-access)).
*  Tato sada se zmenší na ty indexery, které jsou k dispozici, a ne skryté jinými indexery. Následující pravidla jsou použita pro každý indexer `S.I` v sadě, kde `S` je typ, ve kterém je deklarován indexer `I`:
   * Pokud `I` nelze použít s ohledem na `A` ([platný člen funkce](expressions.md#applicable-function-member)), je `I` odebrán ze sady.
   * Pokud je `I` použitelný s ohledem na `A` ([platný člen funkce](expressions.md#applicable-function-member)), pak jsou ze sady odebrány všechny indexery deklarované v základním typu `S`.
   * Pokud je `I` použitelný s ohledem na `A` ([příslušný člen funkce](expressions.md#applicable-function-member)) a `S` je typ třídy jiný než `object`, všechny indexery deklarované v rozhraní jsou ze sady odebrány.
*  Pokud je výsledná sada kandidátských indexerů prázdná, neexistují žádné odpovídající indexery a dojde k chybě při vazbě.
*  Nejlepší indexer sady kandidátských indexerů se identifikuje pomocí pravidel řešení přetížení v rámci [rozlišení přetížení](expressions.md#overload-resolution). Pokud nelze identifikovat jeden nejlepší indexer, přístup k indexeru je nejednoznačný a dojde k chybě při vazbě.
*  Výrazy indexu *argument_list* jsou vyhodnocovány v pořadí zleva doprava. Výsledkem zpracování přístupu indexeru je výraz klasifikovaný jako přístup indexeru. Výraz přístupu indexeru odkazuje na indexer určený v kroku výše a má přidružený výraz instance `P` a seznam přidružených argumentů `A`.

V závislosti na kontextu, ve kterém se používá, přístup indexeru způsobí vyvolání buď přístupového *objektu Get* , nebo *přístupového objektu* pro indexer. Pokud je přístup indexerem cílem přiřazení, vyvolá se *přistupující objekt set* , který přiřadí novou hodnotu ([jednoduché přiřazení](expressions.md#simple-assignment)). Ve všech ostatních případech je vyvolán *přistupující objekt get* , který získá aktuální hodnotu ([hodnoty výrazů](expressions.md#values-of-expressions)).

### <a name="this-access"></a>Tento přístup

*This_Access* se skládá z rezervovaného slova `this`.

```antlr
this_access
    : 'this'
    ;
```

*This_Access* je povolen pouze v *bloku* konstruktoru instance, metodě instance nebo přistupujícího objektu instance. Má jeden z následujících významů:

*  Pokud je `this` použit v *primary_expression* v rámci konstruktoru instance třídy, je klasifikován jako hodnota. Typ hodnoty je typ instance ([typ instance](classes.md#the-instance-type)) třídy, ve které dojde k použití, a hodnota je odkaz na vytvořený objekt.
*  Pokud se `this` používá v *primary_expression* v rámci metody instance nebo přístupového objektu instance třídy, je klasifikována jako hodnota. Typ hodnoty je typ instance ([typ instance](classes.md#the-instance-type)) třídy, ve které dojde k použití, a hodnota je odkaz na objekt, pro který byla metoda nebo přistupující objekt vyvolána.
*  Pokud je `this` použit v *primary_expression* v rámci konstruktoru instance struktury, je klasifikován jako proměnná. Typ proměnné je typ instance ([typ instance](classes.md#the-instance-type)) struktury, ve které dojde k použití, a proměnná představuje vytvořenou strukturu. Proměnná `this` konstruktoru instance struktury se chová stejně jako parametr `out` typu struktury – konkrétně to znamená, že proměnná musí být jednoznačně přiřazena v každé cestě spuštění konstruktoru instance.
*  Pokud se `this` používá v *primary_expression* v rámci metody instance nebo přístupového objektu instance struktury, je klasifikován jako proměnná. Typ proměnné je typ instance ([typ instance](classes.md#the-instance-type)) struktury, ve které dojde k použití.
   * Pokud metoda nebo přistupující objekt není iterátor ([iterátory](classes.md#iterators)), proměnná `this` představuje strukturu, pro kterou byla metoda nebo přistupující objekt vyvolána, a chová se stejně jako parametr `ref` typu struktury.
   * Pokud je metoda nebo přistupující objekt iterátor, proměnná `this` představuje kopii struktury, pro kterou byla metoda nebo přistupující objekt vyvolána, a chová se stejně jako parametr hodnoty typu struktury.

Použití `this` v *primary_expression* v jiném kontextu, než je uvedeno výše, je chyba při kompilaci. Konkrétně není možné odkazovat na `this` ve statické metodě, přístupovém objektu statické vlastnosti nebo v *variable_initializer* deklarace pole.

### <a name="base-access"></a>Základní přístup

*Base_access* se skládá z rezervovaného slova `base` následované tokenem "`.`" a identifikátorem nebo *argument_list* uzavřeným v hranatých závorkách:

```antlr
base_access
    : 'base' '.' identifier
    | 'base' '[' expression_list ']'
    ;
```

*Base_access* se používá pro přístup ke členům základní třídy, které jsou skryté podobným názvem členy v aktuální třídě nebo struktuře. *Base_access* je povolen pouze v *bloku* konstruktoru instance, metodě instance nebo přistupujícího objektu instance. Pokud `base.I` dojde ve třídě nebo struktuře, `I` musí poznamenat člena základní třídy této třídy nebo struktury. Podobně platí, že pokud ve třídě dojde k `base[E]`, musí v základní třídě existovat příslušný indexer.

V době vazby jsou výrazy *base_access* formuláře `base.I` a `base[E]` vyhodnocovány přesně, jako kdyby byly napsány `((B)this).I` a `((B)this)[E]`, kde `B` je základní třídou třídy nebo struktury, ve které konstrukce probíhá. Proto `base.I` a `base[E]` odpovídají `this.I` a `this[E]`, s výjimkou `this` se zobrazuje jako instance základní třídy.

Pokud *base_access* odkazuje na člen virtuální funkce (metoda, vlastnost nebo indexer), určení toho, který člen funkce se má vyvolat za běhu ([Kontrola dynamického přetěžování při kompilaci](expressions.md#compile-time-checking-of-dynamic-overload-resolution)), se změní. Vyvolaný člen funkce je určen tak, že najde největší odvozenou implementaci ([virtuální metody](classes.md#virtual-methods)) člena funkce s ohledem na `B` (místo z hlediska běhového typu `this`, jako by byl obvykle v nezákladním přístupu). . Proto v rámci `override` členu funkce `virtual` lze *base_access* použít k vyvolání zděděné implementace členu funkce. Pokud je člen funkce, na který odkazuje *base_access* , abstraktní, dojde k chybě při vazbě.

### <a name="postfix-increment-and-decrement-operators"></a>Operátory přírůstku a snížení přípony

```antlr
post_increment_expression
    : primary_expression '++'
    ;

post_decrement_expression
    : primary_expression '--'
    ;
```

Operandem operace zvýšení nebo snížení přípony musí být výraz klasifikovaný jako proměnná, přístup k vlastnosti nebo přístup indexeru. Výsledkem operace je hodnota stejného typu jako operand.

Pokud má *primary_expression* typ při kompilaci `dynamic`, operátor je dynamicky svázán ([dynamická vazba](expressions.md#dynamic-binding)), *post_increment_expression* nebo *post_decrement_expression* má typ doby kompilace `dynamic`. a následující pravidla jsou použita v době běhu pomocí běhového typu *primary_expression*.

Pokud je operandem operace přírůstku nebo snížení přípony vlastnost nebo indexer, vlastnost nebo indexer musí mít přístup k `get` i k přístupovému objektu `set`. Pokud se nejedná o tento případ, dojde k chybě při vazbě.

Pro výběr implementace konkrétního operátoru je použito rozlišení přetížení unárního operátoru ([rozlišení přetížení unárního operátoru](expressions.md#unary-operator-overload-resolution)). Předdefinované operátory `++` a `--` existují v následujících typech: `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, 0, 1, 2, 3 a jakýkoli typ výčtu. Předdefinované operátory `++` vrací hodnotu vytvořenou přidáním 1 k operandu a předdefinované operátory `--` vrátí hodnotu vytvořenou odečtením hodnoty 1 od operandu. V kontextu `checked` je-li výsledek tohoto sčítání nebo odčítání mimo rozsah výsledného typu a typ výsledku je celočíselný typ nebo typ výčtu, je vyvolána `System.OverflowException`.

Běhové zpracování přírůstku nebo operace snížení přípony na formuláři `x++` nebo `x--` se skládá z následujících kroků:

*   Je-li hodnota `x` klasifikována jako proměnná:
    * `x` se vyhodnotí, aby se vytvořila proměnná.
    * Hodnota `x` je uložena.
    * Vybraný operátor je vyvolán s uloženou hodnotou `x` jako argument.
    * Hodnota vrácená operátorem je uložena v umístění uvedeném vyhodnocením `x`.
    * Uložená hodnota `x` se stala výsledkem operace.
*   Pokud je `x` klasifikován jako vlastnost nebo přístup indexeru:
    * Výraz instance (Pokud `x` není `static`) a seznam argumentů (Pokud `x` je indexerový přístup) přidružený k `x` jsou vyhodnoceny a výsledky se používají v následujících @no__t – 4 a `set` volání přistupujícího objektu.
    * Vyvolá se přistupující objekt `get` `x` a vrácená hodnota se uloží.
    * Vybraný operátor je vyvolán s uloženou hodnotou `x` jako argument.
    * Přistupující objekt `set` `x` je vyvolán s hodnotou vrácenou operátorem jako jeho argument `value`.
    * Uložená hodnota `x` se stala výsledkem operace.

Operátory `++` a `--` také podporují zápis předpony ([operátory přírůstku a snížení předpony](expressions.md#prefix-increment-and-decrement-operators)). Výsledek `x++` nebo `x--` je obvykle hodnota `x` před operací, zatímco výsledek `++x` nebo `--x` je hodnota `x` po operaci. V obou případech má `x` samé hodnotu po operaci.

Implementaci `operator ++` nebo `operator --` lze vyvolat pomocí přípony nebo notace předpony. Pro tyto dva zápisy není možné mít implementace samostatného operátoru.

### <a name="the-new-operator"></a>Operátor New

Operátor `new` slouží k vytváření nových instancí typů.

Existují tři formy `new` výrazů:

*  Výrazy vytvoření objektu slouží k vytváření nových instancí typů tříd a hodnot.
*  Výrazy vytváření polí slouží k vytváření nových instancí typů polí.
*  Výrazy vytvoření delegáta slouží k vytváření nových instancí typů delegátů.

Operátor `new` implikuje vytvoření instance typu, ale nemusí nutně znamenat dynamické přidělování paměti. Konkrétně instance typů hodnot nevyžadují žádnou další paměť nad rámec, ve kterém jsou umístěny, a žádná dynamická alokace není k dispozici, když `new` slouží k vytváření instancí typů hodnot.

#### <a name="object-creation-expressions"></a>Výrazy pro vytváření objektů

*Object_creation_expression* se používá k vytvoření nové instance třídy *class_type* nebo *value_type*.

```antlr
object_creation_expression
    : 'new' type '(' argument_list? ')' object_or_collection_initializer?
    | 'new' type object_or_collection_initializer
    ;

object_or_collection_initializer
    : object_initializer
    | collection_initializer
    ;
```

*Typ* *object_creation_expression* musí být *class_type*, *value_type* nebo *type_parameter*. *Typ* nemůže být `abstract` *class_type*.

Volitelné *argument_list* ([seznamy argumentů](expressions.md#argument-lists)) jsou povoleny pouze v případě, že je *typ typu* *class_type* nebo *struct_type*.

Výraz pro vytvoření objektu může vynechat seznam argumentů konstruktoru a uzavřít závorky, pokud obsahuje inicializátor objektu nebo inicializátor kolekce. Vynechání seznamu argumentů konstruktoru a uzavíracích závorek je ekvivalentem zadání prázdného seznamu argumentů.

Zpracování výrazu vytvoření objektu, který obsahuje inicializátor objektu nebo inicializátor kolekce, se skládá z prvního zpracování konstruktoru instance a následným zpracováním inicializace členů nebo elementů zadaných inicializátorem objektu ([ Inicializátory objektů](expressions.md#object-initializers)) nebo inicializátor kolekce ([inicializátory kolekce](expressions.md#collection-initializers)).

Pokud některý z argumentů v volitelné *argument_list* má typ pro dobu kompilace `dynamic`, *object_creation_expression* je dynamicky svázán ([dynamická vazba](expressions.md#dynamic-binding)) a následující pravidla jsou použita za běhu za běhu pomocí rutiny run-time. Typ argumentů *argument_list* , které mají typ doby kompilace `dynamic`. Vytvoření objektu však podchází omezené době kompilace, jak je popsáno v tématu [Kontrola dynamického přetěžování při kompilaci](expressions.md#compile-time-checking-of-dynamic-overload-resolution).

Zpracování *object_creation_expression* formuláře v čase vazby `new T(A)`, kde `T` je *class_type* nebo *value_type* a `A` je volitelné *argument_list*, sestává z následujících kroků:

*   Pokud je `T` *value_type* a `A` není k dispozici:
    * *Object_creation_expression* je výchozí vyvolání konstruktoru. Výsledek *object_creation_expression* je hodnota typu `T`, konkrétně výchozí hodnota pro `T`, jak je definována v [typu System. ValueType](types.md#the-systemvaluetype-type).
*   Jinak, pokud `T` je *type_parameter* a `A` není k dispozici:
    * Pokud pro `T` není zadáno omezení typu hodnoty nebo omezení konstruktoru ([omezení parametrů typu](classes.md#type-parameter-constraints)), dojde k chybě při vazbě.
    * Výsledek *object_creation_expression* je hodnota běhového typu, ke kterému byl parametr typu vázán, konkrétně výsledek vyvolání výchozího konstruktoru tohoto typu. Typ běhu může být odkazový typ nebo typ hodnoty.
*   Jinak, pokud `T` je *class_type* nebo *struct_type*:
    * Pokud `T` je `abstract` *class_type*, dojde k chybě při kompilaci.
    * Konstruktor instance, který se má vyvolat, je určen pomocí pravidel řešení přetížení v [rozlišení přetěžování](expressions.md#overload-resolution). Sada konstruktorů instancí kandidátů se skládá ze všech přístupných konstruktorů instancí deklarovaných v `T`, které platí pro `A` ([platný člen funkce](expressions.md#applicable-function-member)). Pokud je sada konstruktorů instancí kandidátů prázdná nebo pokud jeden nejlepší konstruktor instance nelze identifikovat, dojde k chybě při vazbě.
    * Výsledek *object_creation_expression* je hodnota typu `T`, jmenovitá hodnota vytvořená vyvoláním konstruktoru instance určeného v kroku výše.
*  V opačném případě je *object_creation_expression* neplatný a dojde k chybě při vazbě.

I v případě, že je *object_creation_expression* dynamicky svázán, je typ kompilace stále `T`.

Běhové zpracování *object_creation_expression* formuláře `new T(A)`, kde `T` je *class_type* nebo *struct_type* a `A` je volitelná *argument_list*, sestává z následujících kroků:

*   Pokud `T` je *class_type*:
    * Je přidělena nová instance třídy `T`. Pokud není k dispozici dostatek paměti pro přidělení nové instance, je vyvolána `System.OutOfMemoryException` a nejsou provedeny žádné další kroky.
    * Všechna pole nové instance jsou inicializována na výchozí hodnoty ([výchozí hodnoty](variables.md#default-values)).
    * Konstruktor instance je vyvolán v souladu s pravidly vyvolání člena funkce ([Kontrola při kompilaci dynamického přetěžování](expressions.md#compile-time-checking-of-dynamic-overload-resolution)). Odkaz na nově přidělenou instanci je automaticky předán konstruktoru instance a k instanci lze v rámci tohoto konstruktoru přistoupit jako `this`.
*   Pokud `T` je *struct_type*:
    * Instance typu `T` je vytvořena přidělením dočasné místní proměnné. Vzhledem k tomu, že konstruktor instance třídy *struct_type* je vyžadován k omezení přiřazení hodnoty každému poli instance, kterou vytváříte, není nutné inicializovat dočasnou proměnnou.
    * Konstruktor instance je vyvolán v souladu s pravidly vyvolání člena funkce ([Kontrola při kompilaci dynamického přetěžování](expressions.md#compile-time-checking-of-dynamic-overload-resolution)). Odkaz na nově přidělenou instanci je automaticky předán konstruktoru instance a k instanci lze v rámci tohoto konstruktoru přistoupit jako `this`.

#### <a name="object-initializers"></a>Inicializátory objektů

***Inicializátor objektu*** určuje hodnoty pro nula nebo více polí, vlastností nebo indexovaných prvků objektu.

```antlr
object_initializer
    : '{' member_initializer_list? '}'
    | '{' member_initializer_list ',' '}'
    ;

member_initializer_list
    : member_initializer (',' member_initializer)*
    ;

member_initializer
    : initializer_target '=' initializer_value
    ;

initializer_target
    : identifier
    | '[' argument_list ']'
    ;

initializer_value
    : expression
    | object_or_collection_initializer
    ;
```

Inicializátor objektu se skládá z sekvence inicializátorů členů, které jsou uzavřeny tokeny `{` a `}` a oddělené čárkami. Každý *member_initializer* určuje cíl pro inicializaci. *Identifikátor* musí pojmenovat přístupné pole nebo vlastnost objektu, který je inicializován, zatímco *argument_list* uzavřený v hranatých závorkách musí zadat argumenty pro přístupný indexer u objektu, který se inicializuje. Pokud má inicializátor objektu zahrnout více než jeden inicializátor členů pro stejné pole nebo vlastnost, jedná se o chybu.

Každý *initializer_target* je následován symbolem rovná se a buď výraz, inicializátor objektu nebo inicializátor kolekce. Pro výrazy v inicializátoru objektu není možné odkazovat na nově vytvořený objekt, který inicializuje.

Inicializátor člena, který určuje výraz po zpracování znaménka rovná se, se zpracovává stejným způsobem jako přiřazení ([jednoduché přiřazení](expressions.md#simple-assignment)) k cíli.

Inicializátor členu, který určuje inicializátor objektu po znaménku rovná se ***vnořeným objektem***, tj. inicializací vloženého objektu. Namísto přiřazení nové hodnoty k poli nebo vlastnosti jsou přiřazení v inicializátoru vnořeného objektu považována za přiřazení členů pole nebo vlastnosti. Inicializátory vnořených objektů nelze použít pro vlastnosti s typem hodnoty nebo pro pole jen pro čtení s typem hodnoty.

Inicializátor člena, který určuje inicializátor kolekce po znaménku rovná se, představuje inicializaci vložené kolekce. Namísto přiřazení nové kolekce k cílovému poli, vlastnosti nebo indexeru jsou prvky zadané v inicializátoru přidány do kolekce, na kterou odkazuje cíl. Cíl musí být typu kolekce, který splňuje požadavky zadané v [inicializátorech kolekce](expressions.md#collection-initializers).

Argumenty inicializátoru indexu budou vždy vyhodnocovány právě jednou. Proto i v případě, že argumenty nejsou nikdy použity (například z důvodu prázdného vnořeného inicializátoru), budou vyhodnoceny pro své vedlejší účinky.

Následující třída reprezentuje bod se dvěma souřadnicemi:
```csharp
public class Point
{
    int x, y;

    public int X { get { return x; } set { x = value; } }
    public int Y { get { return y; } set { y = value; } }
}
```

Instanci `Point` lze vytvořit a inicializovat následujícím způsobem:
```csharp
Point a = new Point { X = 0, Y = 1 };
```
který má stejný účinek jako
```csharp
Point __a = new Point();
__a.X = 0;
__a.Y = 1; 
Point a = __a;
```
kde `__a` je jinak neviditelná a nepřístupná dočasná proměnná. Následující třída představuje obdélník vytvořený ze dvou bodů:
```csharp
public class Rectangle
{
    Point p1, p2;

    public Point P1 { get { return p1; } set { p1 = value; } }
    public Point P2 { get { return p2; } set { p2 = value; } }
}
```

Instanci `Rectangle` lze vytvořit a inicializovat následujícím způsobem:
```csharp
Rectangle r = new Rectangle {
    P1 = new Point { X = 0, Y = 1 },
    P2 = new Point { X = 2, Y = 3 }
};
```
který má stejný účinek jako
```csharp
Rectangle __r = new Rectangle();
Point __p1 = new Point();
__p1.X = 0;
__p1.Y = 1;
__r.P1 = __p1;
Point __p2 = new Point();
__p2.X = 2;
__p2.Y = 3;
__r.P2 = __p2; 
Rectangle r = __r;
```
kde `__r`, `__p1` a `__p2` jsou dočasné proměnné, které jsou jinak skryté a nedostupné.

Pokud konstruktor `Rectangle` přidělí dvě vložené instance `Point`.
```csharp
public class Rectangle
{
    Point p1 = new Point();
    Point p2 = new Point();

    public Point P1 { get { return p1; } }
    public Point P2 { get { return p2; } }
}
```
následující konstruktor lze použít k inicializaci vložených instancí `Point` namísto přiřazení nových instancí:
```csharp
Rectangle r = new Rectangle {
    P1 = { X = 0, Y = 1 },
    P2 = { X = 2, Y = 3 }
};
```
který má stejný účinek jako
```csharp
Rectangle __r = new Rectangle();
__r.P1.X = 0;
__r.P1.Y = 1;
__r.P2.X = 2;
__r.P2.Y = 3;
Rectangle r = __r;
```

V důsledku příslušné definice jazyka C, následující příklad:
```csharp
var c = new C {
    x = true,
    y = { a = "Hello" },
    z = { 1, 2, 3 },
    ["x"] = 5,
    [0,0] = { "a", "b" },
    [1,2] = {}
};
```
je ekvivalentem této řady přiřazení:
```csharp
C __c = new C();
__c.x = true;
__c.y.a = "Hello";
__c.z.Add(1); 
__c.z.Add(2);
__c.z.Add(3);
string __i1 = "x";
__c[__i1] = 5;
int __i2 = 0, __i3 = 0;
__c[__i2,__i3].Add("a");
__c[__i2,__i3].Add("b");
int __i4 = 1, __i5 = 2;
var c = __c;
```
kde `__c` atd. jsou generovány proměnné, které jsou neviditelné a nepřístupné ke zdrojovému kódu. Všimněte si, že argumenty pro `[0,0]` jsou vyhodnocovány pouze jednou a argumenty pro `[1,2]` jsou vyhodnoceny jednou, i když nejsou nikdy použity.

#### <a name="collection-initializers"></a>Inicializátory kolekce

Inicializátor kolekce Určuje prvky kolekce.

```antlr
collection_initializer
    : '{' element_initializer_list '}'
    | '{' element_initializer_list ',' '}'
    ;

element_initializer_list
    : element_initializer (',' element_initializer)*
    ;

element_initializer
    : non_assignment_expression
    | '{' expression_list '}'
    ;

expression_list
    : expression (',' expression)*
    ;
```

Inicializátor kolekce se skládá z sekvence inicializátorů elementů, které jsou uzavřeny tokeny `{` a `}` a oddělené čárkami. Každý inicializátor elementu určuje prvek, který má být přidán do objektu kolekce, který se inicializuje, a skládá se ze seznamu výrazů, které jsou uzavřeny `{` a `}` tokeny a odděleny čárkami.  Inicializátor elementu s jedním výrazem se dá zapsat bez složených závorek, ale nemůže být výrazem přiřazení, aby nedocházelo k nejednoznačnosti u inicializátorů členů. Výroba *non_assignment_expression* je definovaná ve [výrazu](expressions.md#expression).

Následuje příklad výrazu vytvoření objektu, který obsahuje inicializátor kolekce:
```csharp
List<int> digits = new List<int> { 0, 1, 2, 3, 4, 5, 6, 7, 8, 9 };
```

Objekt kolekce, pro který je použit inicializátor kolekce, musí být typu, který implementuje `System.Collections.IEnumerable` nebo dojde k chybě při kompilaci. U každého zadaného elementu v pořadí vyvolá inicializátor kolekce v cílovém objektu metodu `Add` se seznamem výrazů inicializátoru prvku jako seznam argumentů, použití normálního vyhledávání členů a rozlišení přetěžování pro každé vyvolání. Proto objekt kolekce musí mít platnou instanci nebo metodu rozšíření s názvem `Add` pro každý inicializátor elementu.

Následující třída reprezentuje kontakt s názvem a seznamem telefonních čísel:
```csharp
public class Contact
{
    string name;
    List<string> phoneNumbers = new List<string>();

    public string Name { get { return name; } set { name = value; } }

    public List<string> PhoneNumbers { get { return phoneNumbers; } }
}
```

@No__t-0 lze vytvořit a inicializovat následujícím způsobem:
```csharp
var contacts = new List<Contact> {
    new Contact {
        Name = "Chris Smith",
        PhoneNumbers = { "206-555-0101", "425-882-8080" }
    },
    new Contact {
        Name = "Bob Harris",
        PhoneNumbers = { "650-555-0199" }
    }
};
```
který má stejný účinek jako
```csharp
var __clist = new List<Contact>();
Contact __c1 = new Contact();
__c1.Name = "Chris Smith";
__c1.PhoneNumbers.Add("206-555-0101");
__c1.PhoneNumbers.Add("425-882-8080");
__clist.Add(__c1);
Contact __c2 = new Contact();
__c2.Name = "Bob Harris";
__c2.PhoneNumbers.Add("650-555-0199");
__clist.Add(__c2);
var contacts = __clist;
```
kde `__clist`, `__c1` a `__c2` jsou dočasné proměnné, které jsou jinak skryté a nedostupné.

#### <a name="array-creation-expressions"></a>Výrazy pro vytváření polí

*Array_creation_expression* se používá k vytvoření nové instance *array_type*.

```antlr
array_creation_expression
    : 'new' non_array_type '[' expression_list ']' rank_specifier* array_initializer?
    | 'new' array_type array_initializer
    | 'new' rank_specifier array_initializer
    ;
```

Výraz vytvoření pole prvního formuláře přiděluje instanci pole typu, který je výsledkem odstranění každého jednotlivého výrazu ze seznamu výrazů. Například výraz vytvoření pole `new int[10,20]` vytvoří instanci pole typu `int[,]` a výraz pro vytvoření pole `new int[10][,]` vytvoří pole typu `int[][,]`. Každý výraz v seznamu výrazů musí být typu `int`, `uint`, `long` nebo `ulong` nebo implicitně převést na jeden nebo více těchto typů. Hodnota každého výrazu určuje délku odpovídající dimenze v nově přidělené instanci pole. Vzhledem k tomu, že délka dimenze pole musí být nezáporná, jedná se o chybu při kompilaci, která má v seznamu výrazů *constant_expression* se zápornou hodnotou.

S výjimkou nebezpečného kontextu ([nezabezpečené](unsafe-code.md#unsafe-contexts)kontexty) je rozložení polí Neurčeno.

Pokud výraz pro vytvoření pole prvního formuláře obsahuje inicializátor pole, každý výraz v seznamu výrazů musí být konstanta a délka pořadí a dimenze, které jsou určeny seznamem výrazů, musí odpovídat hodnotám inicializátoru pole.

Ve výrazu pro vytvoření pole druhého nebo třetího formuláře musí pořadí zadaného typu pole nebo specifikátoru řazení odpovídat hodnotě inicializátoru pole. Jednotlivé délky dimenzí jsou odvozeny z počtu prvků v každé z odpovídajících úrovní vnoření inicializátoru pole. Proto výraz
```csharp
new int[,] {{0, 1}, {2, 3}, {4, 5}}
```
přesně odpovídá
```csharp
new int[3, 2] {{0, 1}, {2, 3}, {4, 5}}
```

Výraz pro vytvoření pole třetího formuláře je označován jako ***implicitně typový výraz vytvoření pole***. Je podobný druhému formuláři s tím rozdílem, že typ elementu pole není explicitně přidělen, ale je určen jako nejlepší společný typ ([hledání nejlepšího společného typu sady výrazů](expressions.md#finding-the-best-common-type-of-a-set-of-expressions)) sady výrazů v inicializátoru pole. Pro multidimenzionální pole, tj. jeden, kde *rank_specifier* obsahuje alespoň jednu čárku, tato sada zahrnuje všechny *výrazy*, které byly nalezeny ve vnořených *array_initializer*.

Inicializátory polí jsou podrobněji popsány v [inicializátorech polí](arrays.md#array-initializers).

Výsledek vyhodnocení výrazu vytvoření pole je klasifikován jako hodnota, konkrétně odkaz na nově přidělenou instanci pole. Zpracování výrazu pro vytvoření pole se skládá z následujících kroků:

*  Výrazy délky dimenze *expression_list* jsou vyhodnocovány v pořadí, zleva doprava. Po vyhodnocení každého výrazu se provede implicitní převod ([implicitní převody](conversions.md#implicit-conversions)) na jeden z následujících typů: `int`, `uint`, `long`, `ulong`. První typ v tomto seznamu, pro který existuje implicitní převod. Pokud vyhodnocení výrazu nebo následný implicitní převod způsobí výjimku, nebudou vyhodnoceny žádné další výrazy a nejsou spuštěny žádné další kroky.
*  Vypočítané hodnoty pro délky dimenzí jsou ověřeny následujícím způsobem. Pokud je jedna nebo více hodnot menší než nula, je vyvolána `System.OverflowException` a nejsou provedeny žádné další kroky.
*  Je přidělena instance pole s danými délkami dimenzí. Pokud není k dispozici dostatek paměti pro přidělení nové instance, je vyvolána `System.OutOfMemoryException` a nejsou provedeny žádné další kroky.
*  Všechny prvky nové instance pole jsou inicializovány na výchozí hodnoty ([výchozí hodnoty](variables.md#default-values)).
*  Pokud výraz pro vytvoření pole obsahuje inicializátor pole, pak se každý výraz v inicializátoru pole vyhodnotí a přiřadí odpovídajícímu prvku pole. Vyhodnocení a přiřazení jsou prováděny v pořadí, ve kterém jsou výrazy zapsány v inicializátoru pole – jinými slovy, prvky jsou inicializovány ve vzestupném pořadí indexů, přičemž výše uvedená dimenze nejvíce roste. Pokud vyhodnocení daného výrazu nebo následné přiřazení k odpovídajícímu elementu pole způsobí výjimku, nebudou inicializovány žádné další prvky (a zbývající prvky budou mít výchozí hodnoty).

Výraz pro vytvoření pole povoluje vytvoření instance pole s prvky typu pole, ale prvky takového pole musí být manuálně inicializovány. Například příkaz
```csharp
int[][] a = new int[100][];
```
Vytvoří jednorozměrné pole s 100 prvky typu `int[]`. Počáteční hodnota každého prvku je `null`. Je možné, že stejný výraz vytvoření pole také nemůže vytvořit instanci dílčích polí a příkaz
```csharp
int[][] a = new int[100][5];        // Error
```
má za následek chybu při kompilaci. Instance dílčích polí musí být provedena ručně, jako v
```csharp
int[][] a = new int[100][];
for (int i = 0; i < 100; i++) a[i] = new int[5];
```

Když pole pole má tvar "obdélníkový", který je v případě, že jsou dílčí pole všechny stejné délky, je efektivnější použít multidimenzionální pole. Ve výše uvedeném příkladu vytváří instance pole polí objekty 101 – jedno vnější pole a 100 dílčích polí. Naproti tomu
```csharp
int[,] = new int[100, 5];
```
vytvoří pouze jeden objekt, dvourozměrné pole a provede přidělení v jednom příkazu.

Následují příklady implicitně typované výrazy vytváření pole:
```csharp
var a = new[] { 1, 10, 100, 1000 };                       // int[]

var b = new[] { 1, 1.5, 2, 2.5 };                         // double[]

var c = new[,] { { "hello", null }, { "world", "!" } };   // string[,]

var d = new[] { 1, "one", 2, "two" };                     // Error
```

Poslední výraz způsobí chybu při kompilaci, protože žádná z `int` ani `string` je implicitně převoditelná na druhou, a proto neexistuje žádný nejlepší společný typ. V tomto případě musí být použit explicitní výraz pro vytvoření pole, například zadání typu, který má být `object[]`. Alternativně je možné jeden z prvků přetypovat na společný základní typ, který by pak představoval odvozený typ elementu.

Výrazy vytváření implicitně typovaného pole lze kombinovat s Inicializátory anonymních objektů ([výrazy vytváření anonymních objektů](expressions.md#anonymous-object-creation-expressions)) k vytváření anonymních typů datových struktur. Příklad:
```csharp
var contacts = new[] {
    new {
        Name = "Chris Smith",
        PhoneNumbers = new[] { "206-555-0101", "425-882-8080" }
    },
    new {
        Name = "Bob Harris",
        PhoneNumbers = new[] { "650-555-0199" }
    }
};
```

#### <a name="delegate-creation-expressions"></a>Výrazy vytváření delegátů

*Delegate_creation_expression* se používá k vytvoření nové instance *delegate_type*.

```antlr
delegate_creation_expression
    : 'new' delegate_type '(' expression ')'
    ;
```

Argument výrazu vytvoření delegáta musí být skupina metod, anonymní funkce nebo hodnota buď typu času kompilace `dynamic` nebo *delegate_type*. Pokud je argumentem skupina metod, identifikuje metodu a pro metodu instance objekt, pro který chcete vytvořit delegáta. Pokud je argumentem anonymní funkce, přímo definuje parametry a tělo metody v cíli delegáta. Pokud je argumentem hodnota, která identifikuje instanci delegáta, pro kterou chcete vytvořit kopii.

Pokud má *výraz* typ doby kompilace `dynamic`, je *delegate_creation_expression* dynamicky svázán ([dynamická vazba](expressions.md#dynamic-binding)) a níže uvedená pravidla jsou použita v době běhu pomocí běhového typu *výrazu*. V opačném případě pravidla jsou použita v době kompilace.

Zpracování *delegate_creation_expression* formuláře v čase vazby `new D(E)`, kde `D` je *delegate_type* a `E` je *výraz*, skládá se z následujících kroků:

*  Pokud je `E` skupinou metod, je výraz vytvoření delegáta zpracován stejným způsobem jako převod skupiny metod ([Převod skupin metod](conversions.md#method-group-conversions)) z `E` na `D`.
*  Pokud je `E` anonymní funkce, je výraz vytvoření delegáta zpracován stejným způsobem jako anonymní převod funkce ([anonymní převody funkcí](conversions.md#anonymous-function-conversions)) z `E` na `D`.
*  Pokud je `E` hodnota, musí být `E` kompatibilní ([deklarace delegátů](delegates.md#delegate-declarations)) s `D` a výsledkem je odkaz na nově vytvořený delegát typu `D`, který odkazuje na stejný seznam volání jako `E`. Pokud `E` není kompatibilní s `D`, dojde k chybě při kompilaci.

Běhové zpracování *delegate_creation_expression* formuláře `new D(E)`, kde `D` je *delegate_type* a `E` je *výraz*, skládá se z následujících kroků:

*   Pokud je `E` skupinou metod, vyhodnotí se výraz vytvoření delegáta jako převod skupiny metod ([převody skupin metod](conversions.md#method-group-conversions)) z `E` na `D`.
*   Pokud je `E` anonymní funkce, vytvoření delegáta se vyhodnotí jako anonymní konverze funkce z `E` na `D` ([anonymní převody funkcí](conversions.md#anonymous-function-conversions)).
*   Pokud `E` je hodnota *delegate_type*:
    * je vyhodnocena hodnota `E`. Pokud toto vyhodnocení způsobí výjimku, nejsou provedeny žádné další kroky.
    * Pokud je hodnota `E` `null`, je vyvolána `System.NullReferenceException` a nejsou provedeny žádné další kroky.
    * Je přidělena nová instance typu delegáta `D`. Pokud není k dispozici dostatek paměti pro přidělení nové instance, je vyvolána `System.OutOfMemoryException` a nejsou provedeny žádné další kroky.
    * Nová instance delegáta se inicializuje se stejným seznamem vyvolání jako instance delegáta zadaná `E`.

Seznam volání delegáta je určen při vytváření instance delegáta a pak zůstává konstantní pro celou dobu života delegáta. Jinými slovy, nemůžete změnit cílovou entitu volat jako delegované, jakmile se vytvoří. Pokud jsou dva Delegáti zkombinováni nebo jeden z nich odebrán z jiné ([deklarace delegátů](delegates.md#delegate-declarations)), nové výsledky delegáta; u žádného existujícího delegáta se změnil jeho obsah.

Není možné vytvořit delegáta, který odkazuje na vlastnost, indexer, uživatelsky definovaný operátor, konstruktor instance, destruktor nebo statický konstruktor.

Jak je popsáno výše, při vytvoření delegáta ze skupiny metod, seznam formálních parametrů a návratový typ delegáta určují, které z přetížených metod vybrat. V příkladu
```csharp
delegate double DoubleFunc(double x);

class A
{
    DoubleFunc f = new DoubleFunc(Square);

    static float Square(float x) {
        return x * x;
    }

    static double Square(double x) {
        return x * x;
    }
}
```
pole `A.f` je inicializováno pomocí delegáta, který odkazuje na druhou metodu `Square`, protože tato metoda přesně odpovídá seznamu formálních parametrů a návratový typ `DoubleFunc`. Nebyla nalezena druhá metoda `Square`, došlo k chybě při kompilaci.

#### <a name="anonymous-object-creation-expressions"></a>Výrazy pro vytváření anonymních objektů

*Anonymous_object_creation_expression* se používá k vytvoření objektu anonymního typu.

```antlr
anonymous_object_creation_expression
    : 'new' anonymous_object_initializer
    ;

anonymous_object_initializer
    : '{' member_declarator_list? '}'
    | '{' member_declarator_list ',' '}'
    ;

member_declarator_list
    : member_declarator (',' member_declarator)*
    ;

member_declarator
    : simple_name
    | member_access
    | base_access
    | null_conditional_member_access
    | identifier '=' expression
    ;
```

Inicializátor anonymního objektu deklaruje anonymní typ a vrátí instanci tohoto typu. Anonymní typ je Nameless typ třídy, který dědí přímo z `object`. Členové anonymního typu jsou sekvence vlastností jen pro čtení odvozeny od inicializátoru anonymního objektu použitého k vytvoření instance typu. Konkrétně inicializátor anonymních objektů ve formuláři
```csharp
new { p1 = e1, p2 = e2, ..., pn = en }
```
deklaruje anonymní typ formuláře.
```csharp
class __Anonymous1
{
    private readonly T1 f1;
    private readonly T2 f2;
    ...
    private readonly Tn fn;

    public __Anonymous1(T1 a1, T2 a2, ..., Tn an) {
        f1 = a1;
        f2 = a2;
        ...
        fn = an;
    }

    public T1 p1 { get { return f1; } }
    public T2 p2 { get { return f2; } }
    ...
    public Tn pn { get { return fn; } }

    public override bool Equals(object __o) { ... }
    public override int GetHashCode() { ... }
}
```
kde každý `Tx` je typ odpovídajícího výrazu `ex`. Výraz použitý v *member_declarator* musí mít typ. Proto se jedná o chybu při kompilaci pro výraz v *member_declarator* , který má být null nebo anonymní funkce. Také se jedná o chybu při kompilaci, aby výraz měl nezabezpečený typ.

Názvy anonymního typu a parametru jeho metody `Equals` jsou automaticky generovány kompilátorem a nelze na něj odkazovat v textu programu.

V rámci stejného programu budou dva Inicializátory anonymních objektů, které určují sekvenci vlastností se stejnými názvy a typy pro kompilaci ve stejném pořadí, vytvořit instance stejného anonymního typu.

V příkladu
```csharp
var p1 = new { Name = "Lawnmower", Price = 495.00 };
var p2 = new { Name = "Shovel", Price = 26.95 };
p1 = p2;
```
přiřazení na posledním řádku je povoleno, protože `p1` a `p2` jsou stejného anonymního typu.

Metody `Equals` a `GetHashcode` v anonymních typech přepisují metody děděné z `object` a jsou definovány ve smyslu `Equals` a `GetHashcode` vlastností, aby se dva instance stejného anonymního typu rovnaly pouze tehdy, pokud jsou všechny jejich vlastnosti. výši.

Člen deklarátor může být zkrácen na jednoduchý název ([odvození typu](expressions.md#type-inference)), přístup ke členu ([Kontrola při kompilaci dynamického překladu přetížení](expressions.md#compile-time-checking-of-dynamic-overload-resolution)), základní přístup ([základní přístup](expressions.md#base-access)) nebo hodnota null podmíněného přístupu člena ([ Podmíněné výrazy s hodnotou null jako Inicializátory projekce](expressions.md#null-conditional-expressions-as-projection-initializers). Tento postup se nazývá ***inicializátor projekce*** a je zkrácený pro deklaraci a přiřazení na vlastnost se stejným názvem. Konkrétně členské deklarátory formuláře
```csharp
identifier
expr.identifier
```
jsou přesně stejné jako v následujícím pořadí:
```csharp
identifier = identifier
identifier = expr.identifier
```

Proto v inicializátoru projekce *identifikátor* vybere jak hodnotu, tak pole nebo vlastnost, ke kterým je přiřazena hodnota. Intuitivní, projekty inicializátorů projekce nestačí pouze jako hodnota, ale také název hodnoty.

### <a name="the-typeof-operator"></a>Operátor typeof

Operátor `typeof` slouží k získání objektu `System.Type` pro typ.

```antlr
typeof_expression
    : 'typeof' '(' type ')'
    | 'typeof' '(' unbound_type_name ')'
    | 'typeof' '(' 'void' ')'
    ;

unbound_type_name
    : identifier generic_dimension_specifier?
    | identifier '::' identifier generic_dimension_specifier?
    | unbound_type_name '.' identifier generic_dimension_specifier?
    ;

generic_dimension_specifier
    : '<' comma* '>'
    ;

comma
    : ','
    ;
```

První forma *typeof_expression* se skládá z klíčového slova `typeof` následovaného *typem*v závorkách. Výsledek výrazu tohoto formuláře je objekt `System.Type` pro zadaný typ. Pro libovolný daný typ je k dispozici pouze jeden objekt `System.Type`. To znamená, že pro typ @ no__t-0 `typeof(T) == typeof(T)` je vždycky true. *Typ* nemůže být `dynamic`.

Druhá forma *typeof_expression* se skládá z klíčového slova `typeof` následovaného *unbound_type_nameem*v závorkách. *Unbound_type_name* je velmi podobný jako *TYPE_NAME* ([obor názvů a názvy typů](basic-concepts.md#namespace-and-type-names)) s tím rozdílem, že *unbound_type_name* obsahuje *generic_dimension_specifier*, kde *TYPE_NAME* obsahuje *type_ argument_list*s. Když je operandem *typeof_expression* sekvence tokenů, které vyhovují gramatikám obou *unbound_type_name* a *TYPE_NAME*, konkrétně v případě, že neobsahuje ani *generic_dimension_specifier* ani *type_argument _List*se pořadí tokenů považuje za *TYPE_NAME*. Význam *unbound_type_name* je určen následovně:

*  Převeďte sekvenci tokenů na *TYPE_NAME* nahrazením každého *generic_dimension_specifier* řetězcem *type_argument_list* , který má stejný počet čárek a klíčové slovo `object` jako každé *type_argument*.
*  Vyhodnotí výsledný *TYPE_NAME*a ignoruje všechna omezení parametrů typu.
*  *Unbound_type_name* překládá na nevázaný obecný typ přidružený k výslednému konstruovanému typu ([vázané a nevázané typy](types.md#bound-and-unbound-types)).

Výsledkem *typeof_expression* je objekt `System.Type` pro výsledný nevázaný obecný typ.

Třetí forma *typeof_expression* se skládá z klíčového slova `typeof` následované klíčovým slovem `void` v závorkách. Výsledek výrazu tohoto formuláře je objekt `System.Type`, který představuje absenci typu. Objekt typu vrácený funkcí `typeof(void)` je odlišný od objektu Type vráceného pro libovolný typ. Tento speciální objekt typu je užitečný v knihovnách tříd, které umožňují reflexi do metod v jazyce, kde tyto metody mají mít způsob, jak vyjádřit návratový typ jakékoli metody, včetně metod void, s instancí `System.Type`.

Operátor `typeof` lze použít pro parametr typu. Výsledkem je objekt `System.Type` pro typ modulu runtime, který byl svázán s parametrem typu. Operátor `typeof` lze také použít pro konstruovaný typ nebo nevázaný obecný typ ([vázané a nevázané typy](types.md#bound-and-unbound-types)). Objekt `System.Type` pro nevázaný obecný typ není stejný jako objekt `System.Type` typu instance. Typ instance je vždy uzavřený konstruovaný typ za běhu, takže jeho objekt `System.Type` závisí na používaných argumentech běhového typu, zatímco obecný typ bez vazby nemá žádné argumenty typu.

Příklad
```csharp
using System;

class X<T>
{
    public static void PrintTypes() {
        Type[] t = {
            typeof(int),
            typeof(System.Int32),
            typeof(string),
            typeof(double[]),
            typeof(void),
            typeof(T),
            typeof(X<T>),
            typeof(X<X<T>>),
            typeof(X<>)
        };
        for (int i = 0; i < t.Length; i++) {
            Console.WriteLine(t[i]);
        }
    }
}

class Test
{
    static void Main() {
        X<int>.PrintTypes();
    }
}
```
Vytvoří následující výstup:
```console
System.Int32
System.Int32
System.String
System.Double[]
System.Void
System.Int32
X`1[System.Int32]
X`1[X`1[System.Int32]]
X`1[T]
```

Všimněte si, že `int` a `System.Int32` jsou stejného typu.

Všimněte si také, že výsledek `typeof(X<>)` není závislý na argumentu typu, ale výsledkem `typeof(X<T>)`.

### <a name="the-checked-and-unchecked-operators"></a>Kontrolované a nezaškrtnuté operátory

Operátory `checked` a `unchecked` slouží k řízení ***kontextu kontroly přetečení*** pro aritmetické operace a převody integrálního typu.

```antlr
checked_expression
    : 'checked' '(' expression ')'
    ;

unchecked_expression
    : 'unchecked' '(' expression ')'
    ;
```

Operátor `checked` vyhodnocuje obsažený výraz v kontrolovaném kontextu a operátor `unchecked` vyhodnocuje obsažený výraz v nekontrolovaném kontextu. *Checked_expression* nebo *unchecked_expression* odpovídá přesně *parenthesized_expression* ([výrazům v závorkách](expressions.md#parenthesized-expressions)) s tím rozdílem, že obsažený výraz je vyhodnocen v daném kontextu kontroly přetečení. .

Kontext kontroly přetečení lze také ovládat pomocí příkazů `checked` a `unchecked` ([příkazy Checked a unchecked](statements.md#the-checked-and-unchecked-statements)).

Následující operace jsou ovlivněny kontextem kontroly přetečení vytvořeným operátory `checked` a `unchecked` a příkazy:

*  Předdefinované operátory `++` a `--` (operátory[přírůstku a](expressions.md#postfix-increment-and-decrement-operators) snížení [předpony a operátory přírůstku a snížení](expressions.md#prefix-increment-and-decrement-operators)), pokud je operand integrálního typu.
*  Předdefinovaný operátor `-` (Unární[operátor mínus](expressions.md#unary-minus-operator)), pokud je operand integrálního typu.
*  Předdefinované `+`, `-`, `*` a binární operátory `/` ([aritmetické operátory](expressions.md#arithmetic-operators)), jsou-li oba operandy integrálního typu.
*  Explicitní číselné převody ([explicitní číselné převody](conversions.md#explicit-numeric-conversions)) z jednoho integrálního typu na jiný celočíselný typ nebo z `float` nebo `double` na celočíselný typ.

Když jedna z výše uvedených operací vytvoří výsledek, který je příliš velký pro reprezentaci v cílovém typu, kontext, ve kterém je operace provedena, řídí výsledné chování:

*  Pokud je operace v kontextu `checked` konstantní výraz ([konstantní výrazy](expressions.md#constant-expressions)), dojde k chybě při kompilaci. V opačném případě, pokud je operace provedena v době běhu, je vyvolána `System.OverflowException`.
*  V kontextu `unchecked` je výsledek zkrácen tím, že zahodí všechny bity s vysokým pořadím, které se nevejdou do cílového typu.

Pro nekonstantní výrazy (výrazy, které jsou vyhodnocovány za běhu), které nejsou uzavřeny žádnými operátory nebo příkazy `checked` nebo `unchecked`, je výchozí kontext kontroly přetečení `unchecked`, pokud nedošlo k externím faktorům (například přepínačům kompilátoru a Konfigurace prostředí spuštění) volání `checked` vyhodnocení.

V případě konstantních výrazů (výrazy, které mohou být plně vyhodnocovány v době kompilace) je výchozí kontext kontroly přetečení vždy `checked`. Pokud není konstantní výraz explicitně umístěn v kontextu `unchecked`, přetečení, ke kterým dojde během kompilace výrazu, vždy způsobí chyby při kompilaci.

Tělo anonymní funkce není ovlivněno `checked` nebo `unchecked` kontexty, ve kterých se anonymní funkce vyskytuje.

V příkladu
```csharp
class Test
{
    static readonly int x = 1000000;
    static readonly int y = 1000000;

    static int F() {
        return checked(x * y);      // Throws OverflowException
    }

    static int G() {
        return unchecked(x * y);    // Returns -727379968
    }

    static int H() {
        return x * y;               // Depends on default
    }
}
```
nejsou hlášeny žádné chyby při kompilaci, protože není možné vyhodnotit žádné výrazy v době kompilace. V době běhu vyvolá metoda `F` `System.OverflowException` a metoda `G` vrátí-727379968 (dolní 32 bity z rozsahu mimo rozsah). Chování metody `H` závisí na výchozím kontextu kontroly přetečení pro kompilaci, ale je buď stejné jako `F` nebo stejné jako `G`.

V příkladu
```csharp
class Test
{
    const int x = 1000000;
    const int y = 1000000;

    static int F() {
        return checked(x * y);      // Compile error, overflow
    }

    static int G() {
        return unchecked(x * y);    // Returns -727379968
    }

    static int H() {
        return x * y;               // Compile error, overflow
    }
}
```
k přetečení, ke kterým dochází při vyhodnocení konstantních výrazů v `F` a `H` způsobí, že budou hlášeny chyby při kompilaci, protože výrazy jsou vyhodnocovány v kontextu `checked`. K přetečení dojde také při vyhodnocení konstantního výrazu v `G`, ale vzhledem k tomu, že probíhá vyhodnocení v kontextu `unchecked`, přetečení není hlášeno.

Operátory `checked` a `unchecked` ovlivňují pouze kontext kontroly přetečení u těchto operací, které jsou v textu obsaženy v tokenech "`(`" a "`)`". Operátory nemají žádný vliv na členy funkce, které jsou vyvolány v důsledku vyhodnocení obsaženého výrazu. V příkladu
```csharp
class Test
{
    static int Multiply(int x, int y) {
        return x * y;
    }

    static int F() {
        return checked(Multiply(1000000, 1000000));
    }
}
```
použití `checked` v `F` nemá vliv na vyhodnocení `x * y` v `Multiply`, takže `x * y` je vyhodnoceno ve výchozím kontextu kontroly přetečení.

Operátor `unchecked` je vhodný při psaní konstanty podepsaných integrálních typů v šestnáctkovém zápisu. Příklad:
```csharp
class Test
{
    public const int AllBits = unchecked((int)0xFFFFFFFF);

    public const int HighBit = unchecked((int)0x80000000);
}
```

Oba šestnáctkové konstanty jsou typu `uint`. Vzhledem k tomu, že jsou konstanty mimo rozsah `int` bez operátoru `unchecked`, přetypování na `int` by způsobilo chyby při kompilaci.

Operátory a příkazy `checked` a `unchecked` umožňují programátorům řídit určité aspekty některých číselných výpočtů. Chování některých numerických operátorů ale závisí na datových typech operandů. Například násobení dvou desetinných míst vždy způsobí výjimku přetečení i v rámci explicitního `unchecked` konstrukce. Podobně násobení dvou objektů float nikdy nevede k výjimce přetečení i v rámci explicitního `checked` konstrukce. Kromě toho jiné operátory nejsou nikdy ovlivněny režimem kontroly, bez ohledu na to, zda jsou výchozí nebo explicitní.

### <a name="default-value-expressions"></a>výrazy výchozích hodnot

Výchozí hodnota výrazu je použita k získání výchozí hodnoty ([výchozí hodnoty](variables.md#default-values)) typu. Obvykle je výraz výchozí hodnoty použit pro parametry typu, protože nemusí být znám, pokud je parametr typu hodnotový typ nebo odkazový typ. (Žádný převod neexistuje z literálu `null` na parametr typu, pokud parametr typu není známý jako odkazový typ.)

```antlr
default_value_expression
    : 'default' '(' type ')'
    ;
```

Pokud *typ* v *default_value_expression* vyhodnocuje za běhu na odkazový typ, výsledek je `null` převeden na tento typ. Pokud je *typ* ve *default_value_expression* vyhodnocen za běhu na hodnotový typ, výsledkem je výchozí hodnota *value_type*([výchozí konstruktory](types.md#default-constructors)).

*Default_value_expression* je konstantní výraz ([konstantní výrazy](expressions.md#constant-expressions)), pokud je typ odkazový typ nebo parametr typu, který je známý jako odkazový typ ([omezení parametrů typu](classes.md#type-parameter-constraints)). Kromě toho je *default_value_expression* konstantní výraz, pokud je typ jedním z následujících typů hodnot: `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, 0, 1, 2, 3 nebo jakýkoli typ výčtu.


### <a name="nameof-expressions"></a>Výrazy nameof

*Nameof_expression* se používá k získání názvu entity programu jako konstantního řetězce.

```antlr
nameof_expression
    : 'nameof' '(' named_entity ')'
    ;

named_entity
    : simple_name
    | named_entity_target '.' identifier type_argument_list?
    ;

named_entity_target
    : 'this'
    | 'base'
    | named_entity 
    | predefined_type 
    | qualified_alias_member
    ;
```

Gramaticky řečeno, operand *named_entity* je vždy výraz. Protože `nameof` není rezervované klíčové slovo, výraz nameof je vždycky syntakticky dvojznačný s voláním jednoduchého názvu `nameof`. Z důvodu kompatibility, pokud je vyhledávání názvů ([jednoduché názvy](expressions.md#simple-names)) `nameof` úspěšné, výraz se považuje za *invocation_expression* , bez ohledu na to, zda je vyvolání právní. V opačném případě se jedná o *nameof_expression*.

Význam *named_entityu* *nameof_expression* je jeho význam jako výraz; To znamená buď jako *simple_name*, *base_access* nebo *member_access*. Nicméně pokud vyhledávání popsané v [jednoduchých názvech](expressions.md#simple-names) a [přístupu ke členům](expressions.md#member-access) má za následek chybu, protože člen instance byl nalezen ve statickém kontextu, *nameof_expression* nevytváří žádnou takovou chybu.

Jedná se o chybu při kompilaci pro *named_entity* určení skupiny metod, která má *type_argument_list*. Jedná se o chybu v době kompilace, kterou může *named_entity_target* mít typ `dynamic`.

*Nameof_expression* je konstantní výraz typu `string` a nemá žádný vliv za běhu. Konkrétně není vyhodnocena jeho *named_entity* a je ignorována pro účely přesné analýzy přiřazení ([Obecná pravidla pro jednoduché výrazy](variables.md#general-rules-for-simple-expressions)). Jeho hodnota je poslední identifikátor *named_entity* před nepovinným konečným *type_argument_list*, transformované následujícím způsobem:

* Předpona "`@`", pokud je použita, je odebrána.
* Každý *unicode_escape_sequence* se transformuje na příslušný znak Unicode.
* Všechny *formatting_characters* se odeberou.

Jedná se o stejné transformace použité v [identifikátorech](lexical-structure.md#identifiers) při testování rovnosti mezi identifikátory.

TODO: příklady

### <a name="anonymous-method-expressions"></a>Anonymní výrazy metod

*Anonymous_method_expression* je jedním ze dvou způsobů definování anonymní funkce. Tyto jsou dále popsány ve [výrazech anonymních funkcí](expressions.md#anonymous-function-expressions).

## <a name="unary-operators"></a>Unární operátory

Operátory `?`, `+`, `-`, `!`, `~`, `++`, `--`, cast a `await` se nazývají unární operátory.

```antlr
unary_expression
    : primary_expression
    | null_conditional_expression
    | '+' unary_expression
    | '-' unary_expression
    | '!' unary_expression
    | '~' unary_expression
    | pre_increment_expression
    | pre_decrement_expression
    | cast_expression
    | await_expression
    | unary_expression_unsafe
    ;
```

Pokud má operand *unary_expression* typ při kompilaci `dynamic`, je dynamicky svázán ([dynamická vazba](expressions.md#dynamic-binding)). V tomto případě je typ doby kompilace *unary_expression* `dynamic` a řešení popsané níže bude provedeno za běhu pomocí běhového typu operandu.

### <a name="null-conditional-operator"></a>Podmíněný operátor s hodnotou null

Podmíněný operátor s hodnotou null aplikuje seznam operací na operand pouze v případě, že tento operand je jiný než null. V opačném případě je výsledek použití operátoru `null`.

```antlr
null_conditional_expression
    : primary_expression null_conditional_operations
    ;

null_conditional_operations
    : null_conditional_operations? '?' '.' identifier type_argument_list?
    | null_conditional_operations? '?' '[' argument_list ']'
    | null_conditional_operations '.' identifier type_argument_list?
    | null_conditional_operations '[' argument_list ']'
    | null_conditional_operations '(' argument_list? ')'
    ;
```

Seznam operací může zahrnovat přístup členů a operace přístupu k prvkům (které mohou být samy o hodnotě null) a také vyvolání.

Například výraz `a.b?[0]?.c()` je *null_conditional_expression* s *primary_expression* `a.b` a *null_conditional_operations* `?[0]` (přístup k prvku s hodnotou null), `?.c` (podmíněný člen s hodnotou null). přístup) a `()` (vyvolání).

U *null_conditional_expression* `E` s *primary_expression* `P` nechejte `E0` výraz získaný pomocí textu, který odstraní úvodní `?` od každého *null_conditional_operationsu* `E`. máte jednu z nich. Koncepční, `E0` je výraz, který se vyhodnotí, pokud žádná z kontrol null reprezentovaných `?` nenalezne `null`.

Také `E1` být výraz získaný textovým odebráním počátečního `?` z pouze prvního *null_conditional_operations* v `E`. To může vést k *primárnímu výrazu* (pokud existuje jen jeden `?`) nebo na jiný *null_conditional_expression*.

Pokud je například `E` výraz `a.b?[0]?.c()`, pak `E0` je výraz `a.b[0].c()` a `E1` je výraz `a.b[0]?.c()`.

Je-li hodnota `E0` klasifikována jako Nothing, je `E` klasifikována jako Nothing. V opačném případě je E klasifikován jako hodnota.

`E0` a `E1` se používají k určení významu `E`:

*  Pokud `E` nastane jako *statement_expression* , význam `E` je stejný jako příkaz

   ```csharp
   if ((object)P != null) E1;
   ```

   s výjimkou, že P je vyhodnocována pouze jednou.

*  V opačném případě, pokud je `E0` klasifikována jako Nothing, dojde k chybě při kompilaci.

*  V opačném případě nechte `T0` typu `E0`.

   *  Pokud `T0` je parametr typu, který není známý jako odkazový typ nebo typ hodnoty, který neumožňuje hodnotu null, dojde k chybě při kompilaci.

   *  Pokud je `T0` typ hodnoty, který neumožňuje hodnotu null, pak je typ `E` `T0?` a význam `E` je stejný jako

      ```csharp
      ((object)P == null) ? (T0?)null : E1
      ```

      s výjimkou, že `P` je vyhodnocena pouze jednou.

   *  V opačném případě typ E je T0 a význam E je stejný jako

      ```csharp
      ((object)P == null) ? null : E1
      ```

      s výjimkou, že `P` je vyhodnocena pouze jednou.

Pokud je `E1` sám o sobě *null_conditional_expression*, pak se znovu aplikují tato pravidla a vnořování testů pro `null`, dokud nejsou žádné další `?`, a výraz byl zmenšen tak, aby byl na primárním výrazu `E0`.

Například pokud výraz `a.b?[0]?.c()` nastane jako výraz příkazu, jako v příkazu:
```csharp
a.b?[0]?.c();
```
jeho význam je stejný jako:
```csharp
if (a.b != null) a.b[0]?.c();
```
který je opět stejný jako:
```csharp
if (a.b != null) if (a.b[0] != null) a.b[0].c();
```
S výjimkou, že `a.b` a `a.b[0]` jsou vyhodnocovány pouze jednou.

Pokud k tomu dojde v kontextu, ve kterém je použita jeho hodnota, jako v:
```csharp
var x = a.b?[0]?.c();
```
a za předpokladu, že typ finálního vyvolání není typ hodnoty, který neumožňuje hodnotu null, jeho význam je ekvivalentní:
```csharp
var x = (a.b == null) ? null : (a.b[0] == null) ? null : a.b[0].c();
```
s výjimkou, že `a.b` a `a.b[0]` jsou vyhodnocovány pouze jednou.

#### <a name="null-conditional-expressions-as-projection-initializers"></a>Podmíněné výrazy s hodnotou null jako Inicializátory projekce

Podmíněný výraz s hodnotou null je povolen pouze jako *member_declarator* v *anonymous_object_creation_expression* (výrazy pro[vytváření anonymních objektů](expressions.md#anonymous-object-creation-expressions)), pokud je ukončen s přístupem člena (volitelně null-Conditional). Gramaticky lze tento požadavek vyjádřit takto:

```antlr
null_conditional_member_access
    : primary_expression null_conditional_operations? '?' '.' identifier type_argument_list?
    | primary_expression null_conditional_operations '.' identifier type_argument_list?
    ;
```

Toto je speciální případ gramatiky pro *null_conditional_expression* výše. Výroba pro *member_declarator* ve [výrazech vytváření anonymních objektů](expressions.md#anonymous-object-creation-expressions) pak zahrnuje pouze *null_conditional_member_access*.

#### <a name="null-conditional-expressions-as-statement-expressions"></a>Podmíněné výrazy s hodnotou null jako výrazy příkazu

Podmíněný výraz s hodnotou null je povolen pouze jako *statement_expression* ([příkazy výrazu](statements.md#expression-statements)), pokud končí voláním. Gramaticky lze tento požadavek vyjádřit takto:

```antlr
null_conditional_invocation_expression
    : primary_expression null_conditional_operations '(' argument_list? ')'
    ;
```

Toto je speciální případ gramatiky pro *null_conditional_expression* výše. Výroba pro *statement_expression* v [příkazech výrazu](statements.md#expression-statements) pak obsahuje jenom *null_conditional_invocation_expression*.


### <a name="unary-plus-operator"></a>Jednočlenný operátor plus

Pro vybírání konkrétní implementace operátoru se používá pro operaci formuláře `+x`, rozlišení přetížení unárního operátoru ([řešení přetížení unárního operátoru](expressions.md#unary-operator-overload-resolution)). Operand je převeden na typ parametru vybraného operátoru a typ výsledku je návratový typ operátoru. Předdefinované unární operátory Plus jsou:

```csharp
int operator +(int x);
uint operator +(uint x);
long operator +(long x);
ulong operator +(ulong x);
float operator +(float x);
double operator +(double x);
decimal operator +(decimal x);
```

Pro každý z těchto operátorů je výsledkem jednoduše hodnota operandu.

### <a name="unary-minus-operator"></a>Unární operátor minus

Pro vybírání konkrétní implementace operátoru se používá pro operaci formuláře `-x`, rozlišení přetížení unárního operátoru ([řešení přetížení unárního operátoru](expressions.md#unary-operator-overload-resolution)). Operand je převeden na typ parametru vybraného operátoru a typ výsledku je návratový typ operátoru. Předdefinované operátory negace jsou:

*  Negace typu Integer:

   ```csharp
   int operator -(int x);
   long operator -(long x);
   ```

   Výsledek je vypočítán odčítáním `x` od nuly. Pokud je hodnota `x` nejmenší reprezentovatelné hodnoty typu operandu (-2 ^ 31 pro `int` nebo-2 ^ 63 pro `long`), pak se matematické negace `x` neprezentují v rámci typu operandu. Pokud k tomu dojde v kontextu `checked`, je vyvolána `System.OverflowException`; Pokud k tomu dojde v kontextu `unchecked`, výsledkem je hodnota operandu a přetečení není hlášeno.

   Pokud je Operand operátoru negace typu `uint`, je převeden na typ `long` a typ výsledku je `long`. Výjimkou je pravidlo, které povoluje zápis hodnoty `int`-2147483648 (-2 ^ 31) jako desítkový celočíselný literál ([literály typu Integer](lexical-structure.md#integer-literals)).

   Pokud je Operand operátoru negace typu `ulong`, dojde k chybě při kompilaci. Výjimka je pravidlo, které povoluje zápis hodnoty `long` Value-9223372036854775808 (-2 ^ 63) jako desítkový celočíselný literál ([literály typu Integer](lexical-structure.md#integer-literals)).

*  Negace plovoucí desetinné čárky:

   ```csharp
   float operator -(float x);
   double operator -(double x);
   ```

   Výsledkem je hodnota `x` s jeho znaménkem obráceným. Pokud je `x` NaN, výsledek je také NaN.

*  Desetinné negace:

   ```csharp
   decimal operator -(decimal x);
   ```

   Výsledek je vypočítán odčítáním `x` od nuly. Desítková negace je ekvivalentní k použití unárního operátoru mínus typu `System.Decimal`.

### <a name="logical-negation-operator"></a>Logický operátor negace

Pro vybírání konkrétní implementace operátoru se používá pro operaci formuláře `!x`, rozlišení přetížení unárního operátoru ([řešení přetížení unárního operátoru](expressions.md#unary-operator-overload-resolution)). Operand je převeden na typ parametru vybraného operátoru a typ výsledku je návratový typ operátoru. Existuje pouze jeden předdefinovaný logický operátor negace:
```csharp
bool operator !(bool x);
```

Tento operátor vypočítá logickou negaci operandu: Pokud je operand `true`, výsledek je `false`. Pokud je operand `false`, výsledek je `true`.

### <a name="bitwise-complement-operator"></a>Operátor bitového doplňku

Pro vybírání konkrétní implementace operátoru se používá pro operaci formuláře `~x`, rozlišení přetížení unárního operátoru ([řešení přetížení unárního operátoru](expressions.md#unary-operator-overload-resolution)). Operand je převeden na typ parametru vybraného operátoru a typ výsledku je návratový typ operátoru. Předdefinované bitové operátory doplňku jsou:
```csharp
int operator ~(int x);
uint operator ~(uint x);
long operator ~(long x);
ulong operator ~(ulong x);
```

Pro každý z těchto operátorů je výsledkem operace bitový doplněk `x`.

Každý typ výčtu `E` implicitně poskytuje následující bitový operátor doplňku:

```csharp
E operator ~(E x);
```

Výsledek vyhodnocení `~x`, kde `x` je výraz typu výčtu `E` s podkladovým typem `U`, je naprosto stejný jako vyhodnocování `(E)(~(U)x)` s tím rozdílem, že převod na `E` je vždy proveden jako if v kontextu `unchecked` ( [Zkontrolovaných a nekontrolovaných operátorů](expressions.md#the-checked-and-unchecked-operators)).

### <a name="prefix-increment-and-decrement-operators"></a>Operátory inkrementace a dekrementace předpony

```antlr
pre_increment_expression
    : '++' unary_expression
    ;

pre_decrement_expression
    : '--' unary_expression
    ;
```

Operandem operace zvýšení nebo snížení předpony musí být výraz klasifikovaný jako proměnná, přístup k vlastnosti nebo přístup indexeru. Výsledkem operace je hodnota stejného typu jako operand.

Pokud je operandem operace zvýšení nebo snížení předpony vlastnost nebo indexer, vlastnost nebo indexer musí mít přístup k `get` i k přístupovému objektu `set`. Pokud se nejedná o tento případ, dojde k chybě při vazbě.

Pro výběr implementace konkrétního operátoru je použito rozlišení přetížení unárního operátoru ([rozlišení přetížení unárního operátoru](expressions.md#unary-operator-overload-resolution)). Předdefinované operátory `++` a `--` existují v následujících typech: `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, 0, 1, 2, 3 a jakýkoli typ výčtu. Předdefinované operátory `++` vrací hodnotu vytvořenou přidáním 1 k operandu a předdefinované operátory `--` vrátí hodnotu vytvořenou odečtením hodnoty 1 od operandu. V kontextu `checked` je-li výsledek tohoto sčítání nebo odčítání mimo rozsah výsledného typu a typ výsledku je celočíselný typ nebo typ výčtu, je vyvolána `System.OverflowException`.

Běhové zpracování operace zvýšení nebo snížení předpony ve formátu `++x` nebo `--x` se skládá z následujících kroků:

*   Je-li hodnota `x` klasifikována jako proměnná:
    * `x` se vyhodnotí, aby se vytvořila proměnná.
    * Vybraný operátor je vyvolán s hodnotou `x` jako argument.
    * Hodnota vrácená operátorem je uložena v umístění uvedeném vyhodnocením `x`.
    * Hodnota vrácená operátorem se vrátí k výsledku operace.
*   Pokud je `x` klasifikován jako vlastnost nebo přístup indexeru:
    * Výraz instance (Pokud `x` není `static`) a seznam argumentů (Pokud `x` je indexerový přístup) přidružený k `x` jsou vyhodnoceny a výsledky se používají v následujících @no__t – 4 a `set` volání přistupujícího objektu.
    * Vyvolá se přistupující objekt `get` `x`.
    * Vybraný operátor je vyvolán s hodnotou vrácenou přistupujícím objektem `get` jako argument.
    * Přistupující objekt `set` `x` je vyvolán s hodnotou vrácenou operátorem jako jeho argument `value`.
    * Hodnota vrácená operátorem se vrátí k výsledku operace.

Operátory `++` a `--` také podporují notaci přípon ([operátory přírůstku a snížení přípony](expressions.md#postfix-increment-and-decrement-operators)). Výsledek `x++` nebo `x--` je obvykle hodnota `x` před operací, zatímco výsledek `++x` nebo `--x` je hodnota `x` po operaci. V obou případech má `x` samé hodnotu po operaci.

Implementaci `operator++` nebo `operator--` lze vyvolat pomocí přípony nebo notace předpony. Pro tyto dva zápisy není možné mít implementace samostatného operátoru.

### <a name="cast-expressions"></a>Výrazy cast

*Cast_expression* se používá k explicitnímu převodu výrazu na daný typ.

```antlr
cast_expression
    : '(' type ')' unary_expression
    ;
```

*Cast_expression* formuláře `(T)E`, kde `T` je *typ* a `E` je *unary_expression*, provede explicitní převod ([explicitní převody](conversions.md#explicit-conversions)) hodnoty `E` na typ `T`. Pokud neexistuje žádný explicitní převod z `E` na `T`, dojde k chybě při vazbě. V opačném případě je výsledkem hodnota vytvořená explicitním převodem. Výsledek je vždy klasifikován jako hodnota, i když `E` označuje proměnnou.

Gramatika pro *cast_expression* způsobuje určité syntaktické nejednoznačnosti. Například výraz `(x)-y` může být interpretován jako *cast_expression* (přetypování `-y` na typ `x`) nebo jako *additive_expression* v kombinaci s *parenthesized_expression* (který vypočítá hodnotu `x - y)`.

Pro vyřešení nejednoznačnosti *cast_expression* existuje následující pravidlo: Sekvence jednoho nebo více *tokenů*s ([prázdné místo](lexical-structure.md#white-space)) uzavřená v závorkách je považována za začátek *cast_expression* pouze v případě, že je splněna alespoň jedna z následujících podmínek:

*  Pořadí tokenů je správná gramatika pro *typ*, ale ne pro *výraz*.
*  Pořadí tokenů je správné gramatiky pro daný *typ*a token hned za pravou závorkou je token "`~`", token "`!`", token "`(`", *identifikátor* ([znak Unicode řídicí sekvence ](lexical-structure.md#unicode-character-escape-sequences)), *literál* ([literály](lexical-structure.md#literals)) nebo libovolná *klíčová slova* ([klíčová slova](lexical-structure.md#keywords)) s výjimkou 0 a 1.

Termín "správná gramatika" výše znamená pouze to, že pořadí tokenů musí odpovídat konkrétní gramatické výrobě. Konkrétně nebere v úvahu skutečný význam jakýchkoli identifikátorů prvků. Například pokud `x` a `y` jsou identifikátory, pak `x.y` je správná gramatika pro typ, a to i v případě, že `x.y` není ve skutečnosti označení typu.

Následující pravidlo nejednoznačnosti sleduje, že pokud `x` a `y` jsou identifikátory, `(x)y`, `(x)(y)` a `(x)(-y)` jsou *cast_expression*s, ale `(x)-y` není, i když `x` identifikuje typ. Pokud je však `x` klíčové slovo, které identifikuje předdefinovaný typ (například `int`), pak všechny čtyři formuláře jsou *cast_expression*s (protože takové klíčové slovo by nebylo pravděpodobně výrazem samotné).

### <a name="await-expressions"></a>Výrazy await

Operátor await se používá k pozastavení vyhodnocení ohraničující asynchronní funkce, dokud není dokončena asynchronní operace reprezentované operandem.

```antlr
await_expression
    : 'await' unary_expression
    ;
```

*Await_expression* je povolený jenom v těle asynchronní funkce ([iterátory](classes.md#iterators)). V rámci nejbližší ohraničující asynchronní funkce se *await_expression* nemusí vyskytovat na těchto místech:

*  Uvnitř vnořené (neasynchronní) anonymní funkce
*  Uvnitř bloku *lock_statement*
*  V nezabezpečeném kontextu

Všimněte si, že *await_expression* nemůže být na většině míst v rámci *query_expression*, protože jsou syntakticky transformované na použití neasynchronních výrazů lambda.

Uvnitř asynchronní funkce nelze použít `await` jako identifikátor. Z tohoto důvodu není k dispozici žádná syntaktická nejednoznačnost mezi výrazy await a různými výrazy zahrnujícími identifikátory. Mimo asynchronní funkce `await` funguje jako běžný identifikátor.

Operandem *await_expression* se říká ***úkol***. Představuje asynchronní operaci, která může nebo nemusí být dokončena v době, kdy je *await_expression* vyhodnoceno. Účelem operátoru await je pozastavení provádění nadřazené asynchronní funkce, dokud není dokončen očekávaný úkol, a pak získá výsledek.

#### <a name="awaitable-expressions"></a>Výrazy await

Je nutné, aby úkol výrazu await byl možné ***očekávat***. Výraz `t` je neočekávaný, pokud je k dispozici jeden z následujících:

*  `t` je typ doby kompilace `dynamic`.
*  `t` má přístupnou metodu instance nebo rozšíření nazvanou `GetAwaiter` bez parametrů a žádné parametry typu a návratový typ `A`, pro který všechna následující blokování:
   * `A` implementuje rozhraní `System.Runtime.CompilerServices.INotifyCompletion` (dále označované jako `INotifyCompletion` pro zkrácení).
   * `A` má přístupnou, čitelnou vlastnost instance `IsCompleted` typu `bool`.
   * `A` má přístupnou metodu instance `GetResult` bez parametrů a žádné parametry typu.

Účelem metody `GetAwaiter` je získat pro úkol ***operátor await*** . Typ `A` se nazývá ***typ await*** pro výraz await.

Účelem vlastnosti `IsCompleted` je určit, zda je úloha již dokončena. Pokud ano, není nutné pozastavit vyhodnocení.

Účelem metody `INotifyCompletion.OnCompleted` je zaregistrování "pokračování" na úlohu; například delegát (typu `System.Action`), který bude vyvolán po dokončení úkolu.

Účelem metody `GetResult` je získat výsledek úkolu po jeho dokončení. Tento výsledek může být úspěšný, možná s výslednou hodnotou, nebo může být výjimka, která je vyvolána metodou `GetResult`.

#### <a name="classification-of-await-expressions"></a>Klasifikace výrazů await

Výraz `await t` je klasifikován stejným způsobem jako výraz `(t).GetAwaiter().GetResult()`. Proto pokud je návratový typ `GetResult` `void`, *await_expression* je klasifikován jako Nothing. Pokud má návratový typ, který není typu void `T`, je *await_expression* klasifikován jako hodnota typu `T`.

#### <a name="runtime-evaluation-of-await-expressions"></a>Vyhodnocení za běhu pro výrazy await

Výraz `await t` je za běhu vyhodnocen takto:

*  Operátor await `a` se získá vyhodnocením výrazu `(t).GetAwaiter()`.
*  @No__t-0 `b` se získá vyhodnocením výrazu `(a).IsCompleted`.
*  Pokud je `b` `false`, vyhodnocování bude záviset na tom, zda `a` implementuje rozhraní `System.Runtime.CompilerServices.ICriticalNotifyCompletion` (dále označované jako `ICriticalNotifyCompletion` pro zkrácení). Tato kontroler se provádí v době vytváření vazby. To znamená za běhu, pokud `a` má typ čas kompilace `dynamic` a v čase kompilace v opačném případě. Let `r` označuje delegáta pokračování ([iterátory](classes.md#iterators)):
    * Pokud `a` neimplementuje `ICriticalNotifyCompletion`, je vyhodnocen výraz `(a as (INotifyCompletion)).OnCompleted(r)`.
    * Pokud `a` implementuje `ICriticalNotifyCompletion`, je vyhodnocen výraz `(a as (ICriticalNotifyCompletion)).UnsafeOnCompleted(r)`.
    * Vyhodnocení je pak pozastaveno a řízení je vráceno aktuálnímu volajícímu asynchronní funkce.
*  Výraz `(a).GetResult()` se vyhodnotí hned po (Pokud `b` byl `true`) nebo při pozdějším vyvolání delegáta obnovení (pokud by `b` bylo `false`). Pokud vrátí hodnotu, je tato hodnota výsledkem *await_expression*. V opačném případě je výsledek Nothing.

Implementace volání metody rozhraní `INotifyCompletion.OnCompleted` a `ICriticalNotifyCompletion.UnsafeOnCompleted` by měla způsobit vyvolání delegáta `r`. V opačném případě chování ohraničující asynchronní funkce není definováno.

## <a name="arithmetic-operators"></a>Aritmetické operátory

Operátory `*`, `/`, `%`, `+` a `-` se nazývají aritmetické operátory.

```antlr
multiplicative_expression
    : unary_expression
    | multiplicative_expression '*' unary_expression
    | multiplicative_expression '/' unary_expression
    | multiplicative_expression '%' unary_expression
    ;

additive_expression
    : multiplicative_expression
    | additive_expression '+' multiplicative_expression
    | additive_expression '-' multiplicative_expression
    ;
```

Pokud operand aritmetického operátoru má typ doby kompilace `dynamic`, je výraz dynamicky svázán ([dynamická vazba](expressions.md#dynamic-binding)). V tomto případě je typ doby kompilace výrazu `dynamic` a řešení popsané níže bude provedeno za běhu pomocí běhového typu u operandů, které mají typ doby kompilace `dynamic`.

### <a name="multiplication-operator"></a>Operátor násobení

Pro vybírání konkrétní implementace operátoru se použije pro operaci formuláře `x * y`, rozlišení přetížení binárního operátoru ([rozlišení přetížení binárního operátoru](expressions.md#binary-operator-overload-resolution)). Operandy jsou převedeny na typy parametrů vybraného operátoru a typ výsledku je návratový typ operátoru.

Předdefinované operátory násobení jsou uvedeny níže. Všechny operátory počítají produkt `x` a `y`.

*  Násobení celočíselného čísla:

   ```csharp
   int operator *(int x, int y);
   uint operator *(uint x, uint y);
   long operator *(long x, long y);
   ulong operator *(ulong x, ulong y);
   ```

   Pokud je produkt v kontextu `checked` mimo rozsah výsledného typu, je vyvolána `System.OverflowException`. V kontextu `unchecked` nejsou přetečení hlášeny a jakékoli významné bity vysokého řádu mimo rozsah výsledného typu jsou zahozeny.


*  Násobení plovoucí desetinné čárky:

   ```csharp
   float operator *(float x, float y);
   double operator *(double x, double y);
   ```

   Produkt se vypočítává podle pravidel aritmetické operace IEEE 754. V následující tabulce jsou uvedeny výsledky všech možných kombinací nenulových konečných hodnot, nul, nekonečno a NaN. V tabulce `x` a `y` jsou kladné konečné hodnoty. `z` je výsledkem `x * y`. Pokud je výsledek pro cílový typ příliš velký, `z` je nekonečno. Pokud je výsledek pro cílový typ příliš malý, `z` je nula.

   |      |      |      |     |     |      |      |     |
   |:----:|-----:|:----:|:---:|:---:|:----:|:----:|:----|
   |      | \+ y   | -y   | +0  | -0  | \+ INF | – INF | NaN | 
   | +x   | +z   | -z   | +0  | -0  | \+ INF | – INF | NaN | 
   | -x   | -z   | +z   | -0  | +0  | – INF | \+ INF | NaN | 
   | +0   | +0   | -0   | +0  | -0  | NaN  | NaN  | NaN | 
   | -0   | -0   | +0   | -0  | +0  | NaN  | NaN  | NaN | 
   | \+ INF | \+ INF | – INF | NaN | NaN | \+ INF | – INF | NaN | 
   | – INF | – INF | \+ INF | NaN | NaN | – INF | \+ INF | NaN | 
   | NaN  | NaN  | NaN  | NaN | NaN | NaN  | NaN  | NaN | 

*  Násobení desítkových čísel:

   ```csharp
   decimal operator *(decimal x, decimal y);
   ```

   Pokud je výsledná hodnota příliš velká pro reprezentaci ve formátu `decimal`, je vyvolána `System.OverflowException`. Pokud je výsledná hodnota příliš malá, aby byla reprezentována ve formátu `decimal`, výsledek je nula. Měřítko výsledku, před jakýmkoli zaokrouhlením, je součtem stupnice dvou operandů.

   Násobení v desítkovém formátu je ekvivalentem použití operátoru násobení typu `System.Decimal`.


### <a name="division-operator"></a>Operátor dělení

Pro vybírání konkrétní implementace operátoru se použije pro operaci formuláře `x / y`, rozlišení přetížení binárního operátoru ([rozlišení přetížení binárního operátoru](expressions.md#binary-operator-overload-resolution)). Operandy jsou převedeny na typy parametrů vybraného operátoru a typ výsledku je návratový typ operátoru.

Předdefinované operátory dělení jsou uvedeny níže. Všechny operátory počítají podíl `x` a `y`.

*  Dělení celého čísla:

   ```csharp
   int operator /(int x, int y);
   uint operator /(uint x, uint y);
   long operator /(long x, long y);
   ulong operator /(ulong x, ulong y);
   ```

   Je-li hodnota pravého operandu nula, je vyvolána `System.DivideByZeroException`.

   Dělení zaokrouhlí výsledek směrem k nule. Proto absolutní hodnota výsledku je největší možné celé číslo, které je menší než nebo rovno absolutní hodnotě podílu dvou operandů. Výsledkem je nula nebo kladné, pokud dva operandy mají stejné znaménko a nula nebo záporné, pokud mají dva operandy opačné znaménko.

   Pokud je levý operand nejmenší reprezentovatelné @no__t hodnoty-0 nebo `long` a pravý operand je `-1`, dojde k přetečení. V kontextu `checked` způsobí vyvolání `System.ArithmeticException` (nebo jeho podtříd). V kontextu `unchecked` je implementace definovaná jako k tomu, zda je vyvolána `System.ArithmeticException` (nebo podtřída), nebo přetečení nebude vykazovat výslednou hodnotou levého operandu.

*  Dělení plovoucí desetinné čárky:

   ```csharp
   float operator /(float x, float y);
   double operator /(double x, double y);
   ```

   Podíl se vypočítává podle pravidel aritmetické operace IEEE 754. V následující tabulce jsou uvedeny výsledky všech možných kombinací nenulových konečných hodnot, nul, nekonečno a NaN. V tabulce `x` a `y` jsou kladné konečné hodnoty. `z` je výsledkem `x / y`. Pokud je výsledek pro cílový typ příliš velký, `z` je nekonečno. Pokud je výsledek pro cílový typ příliš malý, `z` je nula.

   |      |      |      |      |      |      |      |      |
   |:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|
   |      | \+ y   | -y   | +0   | -0   | \+ INF | – INF | NaN  | 
   | +x   | +z   | -z   | \+ INF | – INF | +0   | -0   | NaN  | 
   | -x   | -z   | +z   | – INF | \+ INF | -0   | +0   | NaN  | 
   | +0   | +0   | -0   | NaN  | NaN  | +0   | -0   | NaN  | 
   | -0   | -0   | +0   | NaN  | NaN  | -0   | +0   | NaN  | 
   | \+ INF | \+ INF | – INF | \+ INF | – INF | NaN  | NaN  | NaN  | 
   | – INF | – INF | \+ INF | – INF | \+ INF | NaN  | NaN  | NaN  | 
   | NaN  | NaN  | NaN  | NaN  | NaN  | NaN  | NaN  | NaN  | 

*  Desetinné dělení:

   ```csharp
   decimal operator /(decimal x, decimal y);
   ```

   Je-li hodnota pravého operandu nula, je vyvolána `System.DivideByZeroException`. Pokud je výsledná hodnota příliš velká pro reprezentaci ve formátu `decimal`, je vyvolána `System.OverflowException`. Pokud je výsledná hodnota příliš malá, aby byla reprezentována ve formátu `decimal`, výsledek je nula. Měřítko výsledku je nejmenší měřítko, které zachová výsledek, který se rovná nejbližší hodnotě v desítkové soustavě k skutečnému matematickému výsledku.

   Desetinné dělení je ekvivalentní použití operátoru dělení typu `System.Decimal`.


### <a name="remainder-operator"></a>Operátor zbývající

Pro vybírání konkrétní implementace operátoru se použije pro operaci formuláře `x % y`, rozlišení přetížení binárního operátoru ([rozlišení přetížení binárního operátoru](expressions.md#binary-operator-overload-resolution)). Operandy jsou převedeny na typy parametrů vybraného operátoru a typ výsledku je návratový typ operátoru.

Předdefinované zbývající operátory jsou uvedeny níže. Operátory všechny počítají zbývající část rozdělení mezi `x` a `y`.

*  Celočíselný zbytek:

   ```csharp
   int operator %(int x, int y);
   uint operator %(uint x, uint y);
   long operator %(long x, long y);
   ulong operator %(ulong x, ulong y);
   ```

   Výsledkem `x % y` je hodnota vytvořená `x - (x / y) * y`. Je-li hodnota `y` nula, je vyvolána `System.DivideByZeroException`.

   Pokud je levý operand nejmenší hodnota `int` nebo `long` a pravý operand je `-1`, je vyvolána `System.OverflowException`. V žádném případě `x % y` vyvolá výjimku, kde `x / y` by nevolalo výjimku.

*  Zbytek s plovoucí desetinnou čárkou:

   ```csharp
   float operator %(float x, float y);
   double operator %(double x, double y);
   ```

   V následující tabulce jsou uvedeny výsledky všech možných kombinací nenulových konečných hodnot, nul, nekonečno a NaN. V tabulce `x` a `y` jsou kladné konečné hodnoty. `z` je výsledkem `x % y` a počítá se jako `x - n * y`, kde `n` je největší možné celé číslo, které je menší nebo rovno `x / y`. Tato metoda výpočtu zbytku je analogická jako u operandů typu Integer, ale liší se od definice IEEE 754 (v tom, že `n` je celé číslo nejbližší k `x / y`).

   |      |      |      |      |      |      |      |      |
   |:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|
   |      | \+ y   | -y   | +0   | -0   | \+ INF | – INF | NaN  | 
   | +x   | +z   | +z   | NaN  | NaN  | x    | x    | NaN  | 
   | -x   | -z   | -z   | NaN  | NaN  | -x   | -x   | NaN  | 
   | +0   | +0   | +0   | NaN  | NaN  | +0   | +0   | NaN  | 
   | -0   | -0   | -0   | NaN  | NaN  | -0   | -0   | NaN  | 
   | \+ INF | NaN  | NaN  | NaN  | NaN  | NaN  | NaN  | NaN  | 
   | – INF | NaN  | NaN  | NaN  | NaN  | NaN  | NaN  | NaN  | 
   | NaN  | NaN  | NaN  | NaN  | NaN  | NaN  | NaN  | NaN  | 

*  Desetinná část:

   ```csharp
   decimal operator %(decimal x, decimal y);
   ```

   Je-li hodnota pravého operandu nula, je vyvolána `System.DivideByZeroException`. Měřítko výsledku, před zaokrouhlením, je větší z škály dvou operandů a znaménko výsledku, pokud není nula, je stejné jako `x`.

   Desítkový zbytek je ekvivalentem použití operátoru zbytek typu `System.Decimal`.


### <a name="addition-operator"></a>Operátor sčítání

Pro vybírání konkrétní implementace operátoru se použije pro operaci formuláře `x + y`, rozlišení přetížení binárního operátoru ([rozlišení přetížení binárního operátoru](expressions.md#binary-operator-overload-resolution)). Operandy jsou převedeny na typy parametrů vybraného operátoru a typ výsledku je návratový typ operátoru.

Předdefinované operátory sčítání jsou uvedeny níže. Pro číselné a výčtové typy jsou předdefinované operátory sčítání počítány součtem obou operandů. Když je jeden nebo oba operandy typu String, předdefinované operátory sčítání zřetězí řetězcovou reprezentaci operandů.

*  Sčítání celého čísla:

   ```csharp
   int operator +(int x, int y);
   uint operator +(uint x, uint y);
   long operator +(long x, long y);
   ulong operator +(ulong x, ulong y);
   ```

   Je-li v kontextu `checked`, je-li součet mimo rozsah výsledného typu, je vyvolána `System.OverflowException`. V kontextu `unchecked` nejsou přetečení hlášeny a jakékoli významné bity vysokého řádu mimo rozsah výsledného typu jsou zahozeny.

*  Přidání plovoucí desetinné čárky:

   ```csharp
   float operator +(float x, float y);
   double operator +(double x, double y);
   ```

   Součet se vypočítává podle pravidel aritmetické operace IEEE 754. V následující tabulce jsou uvedeny výsledky všech možných kombinací nenulových konečných hodnot, nul, nekonečno a NaN. V tabulce jsou `x` a `y` nenulové konečné hodnoty a `z` je výsledkem `x + y`. Pokud `x` a `y` mají stejnou velikost, ale opačné znaménko, `z` je kladné nula. Pokud je `x + y` příliš velký pro reprezentaci v cílovém typu, `z` je nekonečno se stejným znaménkem jako `x + y`.

   |      |      |      |      |      |      |      |
   |:----:|:----:|:----:|:----:|:----:|:----:|:----:|
   |      | Y    | +0   | -0   | \+ INF | – INF | NaN  | 
   | x    | z    | x    | x    | \+ INF | – INF | NaN  | 
   | +0   | Y    | +0   | +0   | \+ INF | – INF | NaN  | 
   | -0   | Y    | +0   | -0   | \+ INF | – INF | NaN  | 
   | \+ INF | \+ INF | \+ INF | \+ INF | \+ INF | NaN  | NaN  | 
   | – INF | – INF | – INF | – INF | NaN  | – INF | NaN  | 
   | NaN  | NaN  | NaN  | NaN  | NaN  | NaN  | NaN  | 

*  Sčítání desetinných míst:

   ```csharp
   decimal operator +(decimal x, decimal y);
   ```

   Pokud je výsledná hodnota příliš velká pro reprezentaci ve formátu `decimal`, je vyvolána `System.OverflowException`. Měřítko výsledku, před jakýmkoli zaokrouhlením, je větší z škály dvou operandů.

   Sčítání desetinných míst je ekvivalentní použití operátoru sčítání typu `System.Decimal`.

*  Sčítání výčtu. Každý typ výčtu implicitně poskytuje následující předdefinované operátory, kde `E` je typ výčtu a `U` je základní typ `E`:

   ```csharp
   E operator +(E x, U y);
   E operator +(U x, E y);
   ```

   V době běhu jsou tyto operátory vyhodnocovány přesně tak, jak `(E)((U)x + (U)y)`.

*  Zřetězení řetězců:

   ```csharp
   string operator +(string x, string y);
   string operator +(string x, object y);
   string operator +(object x, string y);
   ```

   Tato přetížení binárního operátoru `+` provádí zřetězení řetězců. Pokud je operand zřetězení řetězců `null`, je nahrazen prázdný řetězec. V opačném případě je libovolný neřetězcový argument převeden na jeho řetězcové vyjádření vyvoláním metody Virtual `ToString` zděděné z typu `object`. Pokud `ToString` vrátí `null`, bude nahrazen prázdný řetězec.

   ```csharp
   using System;
   
   class Test
   {
       static void Main() {
           string s = null;
           Console.WriteLine("s = >" + s + "<");        // displays s = ><
           int i = 1;
           Console.WriteLine("i = " + i);               // displays i = 1
           float f = 1.2300E+15F;
           Console.WriteLine("f = " + f);               // displays f = 1.23E+15
           decimal d = 2.900m;
           Console.WriteLine("d = " + d);               // displays d = 2.900
       }
   }
   ```

   Výsledek operátoru zřetězení řetězce je řetězec, který se skládá z znaků levého operandu následovaného znaky pravého operandu. Operátor zřetězení řetězců nikdy nevrací hodnotu `null`. @No__t-0 může být vyvolána, pokud není k dispozici dostatek paměti pro přidělení výsledného řetězce.

*  Kombinace delegátů. Každý typ delegáta implicitně poskytuje následující předdefinovaný operátor, kde `D` je typ delegáta:

   ```csharp
   D operator +(D x, D y);
   ```

   Binární operátor `+` provádí kombinaci delegáta, pokud jsou oba operandy určitého typu delegáta `D`. (Pokud operandy mají odlišné typy delegátů, dojde k chybě při vazbě.) Pokud je první operand `null`, výsledkem operace je hodnota druhého operandu (i v případě, že je také `null`). V opačném případě, pokud je druhý operand `null`, pak výsledek operace je hodnota prvního operandu. V opačném případě je výsledkem operace nová instance delegáta, která při vyvolání vyvolá první operand a potom vyvolá druhý operand. Příklady kombinace delegátů naleznete v tématu [operátor odčítání](expressions.md#subtraction-operator) a [volání delegáta](delegates.md#delegate-invocation). Protože `System.Delegate` není typ delegáta, `operator` @ no__t-2 není pro něj definováno.

### <a name="subtraction-operator"></a>Operátor odčítání

Pro vybírání konkrétní implementace operátoru se použije pro operaci formuláře `x - y`, rozlišení přetížení binárního operátoru ([rozlišení přetížení binárního operátoru](expressions.md#binary-operator-overload-resolution)). Operandy jsou převedeny na typy parametrů vybraného operátoru a typ výsledku je návratový typ operátoru.

Předdefinované operátory odčítání jsou uvedeny níže. Všechny operátory odečtou `y` od `x`.

*  Celočíselná odčítání:

   ```csharp
   int operator -(int x, int y);
   uint operator -(uint x, uint y);
   long operator -(long x, long y);
   ulong operator -(ulong x, ulong y);
   ```

   Pokud je rozdíl mimo rozsah výsledného typu, je vyvolána `System.OverflowException` v kontextu `checked`. V kontextu `unchecked` nejsou přetečení hlášeny a jakékoli významné bity vysokého řádu mimo rozsah výsledného typu jsou zahozeny.

*  Odčítání plovoucí desetinné čárky:

   ```csharp
   float operator -(float x, float y);
   double operator -(double x, double y);
   ```

   Rozdíl se vypočítává podle pravidel aritmetické operace IEEE 754. V následující tabulce jsou uvedeny výsledky všech možných kombinací nenulových konečných hodnot, nul, nekonečno a hodnoty NaN. V tabulce jsou `x` a `y` nenulové konečné hodnoty a `z` je výsledkem `x - y`. Je-li hodnota `x` a `y` rovna nule, `z` je kladné nula. Pokud je `x - y` příliš velký pro reprezentaci v cílovém typu, `z` je nekonečno se stejným znaménkem jako `x - y`.

   |      |      |      |      |      |      |     |
   |:----:|:----:|:----:|:----:|:----:|:----:|:---:|
   |      | Y    | +0   | -0   | \+ INF | – INF | NaN | 
   | x    | z    | x    | x    | – INF | \+ INF | NaN | 
   | +0   | -y   | +0   | +0   | – INF | \+ INF | NaN | 
   | -0   | -y   | -0   | +0   | – INF | \+ INF | NaN | 
   | \+ INF | \+ INF | \+ INF | \+ INF | NaN  | \+ INF | NaN | 
   | – INF | – INF | – INF | – INF | – INF | NaN  | NaN | 
   | NaN  | NaN  | NaN  | NaN  | NaN  | NaN  | NaN | 

*  Desetinné odčítání:

   ```csharp
   decimal operator -(decimal x, decimal y);
   ```

   Pokud je výsledná hodnota příliš velká pro reprezentaci ve formátu `decimal`, je vyvolána `System.OverflowException`. Měřítko výsledku, před jakýmkoli zaokrouhlením, je větší z škály dvou operandů.

   Desetinné odčítání je ekvivalentem použití operátoru odčítání typu `System.Decimal`.

*  Odčítání výčtu. Každý typ výčtu implicitně poskytuje následující předdefinovaný operátor, kde `E` je typ výčtu a `U` je základní typ `E`:

   ```csharp
   U operator -(E x, E y);
   ```

   Tento operátor je vyhodnocen přesně jako `(U)((U)x - (U)y)`. Jinými slovy, operátor vypočítá rozdíl mezi ordinálními hodnotami `x` a `y` a typ výsledku je základní typ výčtu.

   ```csharp
   E operator -(E x, U y);
   ```

   Tento operátor je vyhodnocen přesně jako `(E)((U)x - y)`. Jinými slovy, operátor odečte hodnotu od základního typu výčtu a vrátí hodnotu výčtu.

*  Odebrání delegáta. Každý typ delegáta implicitně poskytuje následující předdefinovaný operátor, kde `D` je typ delegáta:

   ```csharp
   D operator -(D x, D y);
   ```

   Binární operátor `-` provádí odebrání delegáta, pokud jsou oba operandy určitého typu delegáta `D`. Pokud mají operandy různé typy delegátů, dojde k chybě při vazbě. Pokud je první operand `null`, výsledek operace je `null`. V opačném případě, pokud je druhý operand `null`, pak výsledek operace je hodnota prvního operandu. V opačném případě oba operandy znázorňují seznamy volání ([deklarace delegátů](delegates.md#delegate-declarations)), které mají jednu nebo více položek, a výsledkem je nový seznam vyvolání, který se skládá ze seznamu prvního operandu s odebranými položkami druhého operandu za sekundu. seznam operandů je správný souvislý podseznam první z nich.     (Chcete-li určit rovnost podseznamu, odpovídající položky jsou porovnány s operátorem rovnosti delegáta ([operátory rovnosti delegátů](expressions.md#delegate-equality-operators)).) V opačném případě je výsledkem hodnota levého operandu. V procesu nejsou změněny ani žádné seznamy operandů. Pokud je v seznamu druhý operand shodný s více podseznamy souvislých položek v seznamu prvního operandu, je odebrán odpovídající podseznam sousedících položek. Pokud je výsledkem odebrání prázdný seznam, výsledek je @no__t – 0. Příklad:

   ```csharp
   delegate void D(int x);
   
   class C
   {
       public static void M1(int i) { /* ... */ }
       public static void M2(int i) { /* ... */ }
   }

   class Test
   {
       static void Main() { 
           D cd1 = new D(C.M1);
           D cd2 = new D(C.M2);
           D cd3 = cd1 + cd2 + cd2 + cd1;   // M1 + M2 + M2 + M1
           cd3 -= cd1;                      // => M1 + M2 + M2
   
           cd3 = cd1 + cd2 + cd2 + cd1;     // M1 + M2 + M2 + M1
           cd3 -= cd1 + cd2;                // => M2 + M1
   
           cd3 = cd1 + cd2 + cd2 + cd1;     // M1 + M2 + M2 + M1
           cd3 -= cd2 + cd2;                // => M1 + M1
   
           cd3 = cd1 + cd2 + cd2 + cd1;     // M1 + M2 + M2 + M1
           cd3 -= cd2 + cd1;                // => M1 + M2
   
           cd3 = cd1 + cd2 + cd2 + cd1;     // M1 + M2 + M2 + M1
           cd3 -= cd1 + cd1;                // => M1 + M2 + M2 + M1
       }
   }
   ```

## <a name="shift-operators"></a>Operátory posunutí

Operátory `<<` a `>>` se používají k provádění operací bitových posunutí.

```antlr
shift_expression
    : additive_expression
    | shift_expression '<<' additive_expression
    | shift_expression right_shift additive_expression
    ;
```

Pokud má operand *shift_expression* typ při kompilaci `dynamic`, pak je výraz dynamicky svázán ([dynamická vazba](expressions.md#dynamic-binding)). V tomto případě je typ doby kompilace výrazu `dynamic` a řešení popsané níže bude provedeno za běhu pomocí běhového typu u operandů, které mají typ doby kompilace `dynamic`.

Pro vybírání implementace konkrétního operátora se použije pro operaci formuláře `x << count` nebo `x >> count`, řešení přetížení binárního operátoru ([rozlišení přetížení binárního operátoru](expressions.md#binary-operator-overload-resolution)). Operandy jsou převedeny na typy parametrů vybraného operátoru a typ výsledku je návratový typ operátoru.

Při deklaraci přetíženého operátoru Shift musí být typ prvního operandu vždy třídou nebo strukturou obsahující deklaraci operátoru a typ druhého operandu musí být vždy `int`.

Předdefinované operátory posunutí jsou uvedeny níže.

*  Posunout doleva:

   ```csharp
   int operator <<(int x, int count);
   uint operator <<(uint x, int count);
   long operator <<(long x, int count);
   ulong operator <<(ulong x, int count);
   ```

   Operátor `<<` posune `x` doleva o několik bitů vypočítaných podle popisu níže.

   Vysoké pořadí bitů mimo rozsah výsledného typu `x` se zahodí, zbývající bity se posunou doleva a dolní pořadí prázdných pozic je nastavené na hodnotu nula.

*  Posun doprava:

   ```csharp
   int operator >>(int x, int count);
   uint operator >>(uint x, int count);
   long operator >>(long x, int count);
   ulong operator >>(ulong x, int count);
   ```

   Operátor `>>` posouvá `x` vpravo počtem bitů vypočítaných podle popisu níže.

   Pokud je `x` typu `int` nebo `long`, jsou zrušené bity `x` zahozeny, zbývající bity se posunou vpravo a vysoké prázdné bitové pozice jsou nastaveny na hodnotu nula, pokud je `x` nezáporné a nastavte na jednu, pokud je `x` záporné.

   Pokud je `x` typu `uint` nebo `ulong`, jsou zahozeny dolní pořadí `x`, zbývající bity se posunou vpravo a vysoké pořadí prázdných pozic jsou nastaveny na hodnotu nula.

V případě předdefinovaných operátorů se počet bitů na posunutí vypočítá takto:

*  Pokud je typ `x` `int` nebo `uint`, je počet posunu dán dolním pěti bity `count`. Jinými slovy, počet posunů je vypočítán z `count & 0x1F`.
*  Pokud je typ `x` `long` nebo `ulong`, je počet posunu dán nízkým počtem šesti bitů `count`. Jinými slovy, počet posunů je vypočítán z `count & 0x3F`.

Pokud je počet výsledných posunutí nula, operátory posunutí jednoduše vrátí hodnotu `x`.

Operace posunutí nikdy nezpůsobí přetečení a vytvářejí stejné výsledky v `checked` a kontextech `unchecked`.

V případě, že levý operand operátoru `>>` má podepsaný integrální typ, operátor provede aritmetické posunutí vpravo, kde je hodnota nejvýznamnějšího bitu (bit znaménka) operandu rozšířena na prázdné bitové pozice v horním pořadí. V případě, že levý operand operátoru `>>` je celočíselného typu bez znaménka, operátor provede logický posun vpravo ve vysokém pořadí prázdné bitové pozice jsou vždy nastaveny na hodnotu nula. Chcete-li provést opačnou operaci, která je odvozena od typu operandu, lze použít explicitní přetypování. Například pokud `x` je proměnná typu `int`, operace `unchecked((int)((uint)x >> y))` provede logický posun napravo od `x`.

## <a name="relational-and-type-testing-operators"></a>Relační operátory a operátory testování typů

Operátory `==`, `!=`, `<`, `>`, `<=`, `>=`, `is` a `as` se nazývají relační operátory a operátory testování typu.

```antlr
relational_expression
    : shift_expression
    | relational_expression '<' shift_expression
    | relational_expression '>' shift_expression
    | relational_expression '<=' shift_expression
    | relational_expression '>=' shift_expression
    | relational_expression 'is' type
    | relational_expression 'as' type
    ;

equality_expression
    : relational_expression
    | equality_expression '==' relational_expression
    | equality_expression '!=' relational_expression
    ;
```

Operátor `is` je popsán v operátoru [is](expressions.md#the-is-operator) a operátor `as` je popsán v [operátoru as](expressions.md#the-as-operator).

Operátory ***porovnání***jsou `==`, `!=`, `<`, `>`, `<=` a `>=`.

Pokud operand relačního operátoru má typ doby kompilace `dynamic`, pak je výraz dynamicky svázán ([dynamická vazba](expressions.md#dynamic-binding)). V tomto případě je typ doby kompilace výrazu `dynamic` a řešení popsané níže bude provedeno za běhu pomocí běhového typu u operandů, které mají typ doby kompilace `dynamic`.

Pro provoz formuláře `x` *op* `y`, kde *op* je operátor porovnání, je pro výběr konkrétní implementace operátoru použito rozlišení přetížení ([rozlišení přetěžování binárních operátorů](expressions.md#binary-operator-overload-resolution)). Operandy jsou převedeny na typy parametrů vybraného operátoru a typ výsledku je návratový typ operátoru.

Předdefinované operátory porovnání jsou popsány v následujících částech. Všechny předdefinované operátory porovnání vracejí výsledek typu `bool`, jak je popsáno v následující tabulce.


| __Operace__ | __výsledek__                                                       |
|---------------|------------------------------------------------------------------|
| `x == y`      | `true`, pokud je `x` rovno `y`, `false`, jinak                 | 
| `x != y`      | `true`, pokud `x` není rovno `y`, `false`, jinak             | 
| `x < y`       | `true`, pokud `x` je menší než `y`, `false`, jinak                | 
| `x > y`       | `true`, pokud `x` je větší než `y`, `false`, jinak             | 
| `x <= y`      | `true`, pokud `x` je menší nebo rovno `y`, `false`, jinak    | 
| `x >= y`      | `true`, pokud `x` je větší nebo rovno `y`, `false`, jinak | 

### <a name="integer-comparison-operators"></a>Operátory porovnání celých čísel

Předdefinované celočíselné operátory porovnání jsou:
```csharp
bool operator ==(int x, int y);
bool operator ==(uint x, uint y);
bool operator ==(long x, long y);
bool operator ==(ulong x, ulong y);

bool operator !=(int x, int y);
bool operator !=(uint x, uint y);
bool operator !=(long x, long y);
bool operator !=(ulong x, ulong y);

bool operator <(int x, int y);
bool operator <(uint x, uint y);
bool operator <(long x, long y);
bool operator <(ulong x, ulong y);

bool operator >(int x, int y);
bool operator >(uint x, uint y);
bool operator >(long x, long y);
bool operator >(ulong x, ulong y);

bool operator <=(int x, int y);
bool operator <=(uint x, uint y);
bool operator <=(long x, long y);
bool operator <=(ulong x, ulong y);

bool operator >=(int x, int y);
bool operator >=(uint x, uint y);
bool operator >=(long x, long y);
bool operator >=(ulong x, ulong y);
```

Každý z těchto operátorů porovná číselné hodnoty dvou celočíselných operandů a vrátí hodnotu `bool`, která určuje, zda je konkrétní vztah `true` nebo `false`.

### <a name="floating-point-comparison-operators"></a>Operátory porovnání s plovoucí desetinnou čárkou

Předdefinované operátory porovnání s plovoucí desetinnou čárkou jsou:
```csharp
bool operator ==(float x, float y);
bool operator ==(double x, double y);

bool operator !=(float x, float y);
bool operator !=(double x, double y);

bool operator <(float x, float y);
bool operator <(double x, double y);

bool operator >(float x, float y);
bool operator >(double x, double y);

bool operator <=(float x, float y);
bool operator <=(double x, double y);

bool operator >=(float x, float y);
bool operator >=(double x, double y);
```

Operátory porovnávají operandy podle pravidel standardu IEEE 754:

*  Pokud je jeden z operandů NaN, výsledek je `false` pro všechny operátory s výjimkou `!=`, pro které je výsledek `true`. U všech dvou operandů `x != y` vždy vytvoří stejný výsledek jako `!(x == y)`. Pokud je však jeden nebo oba operandy NaN, operátory `<`, `>`, `<=` a `>=` nepřinesou stejné výsledky jako logickou negaci protějšího operátoru. Pokud je například jedna z `x` a `y` NaN, pak `x < y` je `false`, ale `!(x >= y)` je `true`.
*  Pokud žádný operand není NaN, operátory porovnávají hodnoty dvou operandů s plovoucí desetinnou čárkou s ohledem na řazení.

   ```csharp
   -inf < -max < ... < -min < -0.0 == +0.0 < +min < ... < +max < +inf
   ```

   kde `min` a `max` jsou nejmenší a největší kladné hodnoty, které lze reprezentovat v daném formátu s plovoucí desetinnou čárkou. Mezi významné účinky tohoto řazení patří:
   * Záporné a kladné nuly se považují za stejné.
   * Záporné nekonečno je považováno za méně než všechny ostatní hodnoty, ale je rovno jinému zápornému nekonečnu.
   * Kladné nekonečno je považováno za větší než všechny ostatní hodnoty, ale je rovno jinému kladnému nekonečnu.

### <a name="decimal-comparison-operators"></a>Operátory porovnávání desetinných míst

Předdefinované operátory porovnávání desetinných míst jsou:
```csharp
bool operator ==(decimal x, decimal y);
bool operator !=(decimal x, decimal y);
bool operator <(decimal x, decimal y);
bool operator >(decimal x, decimal y);
bool operator <=(decimal x, decimal y);
bool operator >=(decimal x, decimal y);
```

Každý z těchto operátorů porovná číselné hodnoty dvou desetinných operandů a vrátí hodnotu `bool`, která určuje, zda je konkrétní vztah `true` nebo `false`. Každé desetinné porovnání je ekvivalentní s použitím odpovídajícího relačního nebo operátoru rovnosti typu `System.Decimal`.

### <a name="boolean-equality-operators"></a>Logické operátory rovnosti

Předdefinované logické operátory rovnosti jsou:
```csharp
bool operator ==(bool x, bool y);
bool operator !=(bool x, bool y);
```

Výsledek `==` je `true`, pokud jsou `x` i `y` `true` nebo pokud `x` a `y` jsou `false`. V opačném případě je výsledkem `false`.

Výsledek `!=` je `false`, pokud jsou `x` i `y` `true` nebo pokud `x` a `y` jsou `false`. V opačném případě je výsledkem `true`. Pokud jsou operandy typu `bool`, operátor `!=` vytvoří stejný výsledek jako operátor `^`.

### <a name="enumeration-comparison-operators"></a>Operátory porovnání výčtu

Každý typ výčtu implicitně poskytuje následující předdefinované operátory porovnání:
```csharp
bool operator ==(E x, E y);
bool operator !=(E x, E y);
bool operator <(E x, E y);
bool operator >(E x, E y);
bool operator <=(E x, E y);
bool operator >=(E x, E y);
```

Výsledek vyhodnocení `x op y`, kde `x` a `y` jsou výrazy typu výčtu `E` s podkladovým typem `U` a `op` je jedním z relačních operátorů, je naprosto stejný jako vyhodnocování `((U)x) op ((U)y)`. Jinými slovy, operátory porovnání typu výčtu jednoduše porovnávají základní celočíselné hodnoty dvou operandů.

### <a name="reference-type-equality-operators"></a>Operátory rovnosti typu odkazu

Předdefinované operátory rovnosti referenčního typu jsou:
```csharp
bool operator ==(object x, object y);
bool operator !=(object x, object y);
```

Operátory vrátí výsledek porovnání dvou odkazů pro rovnost nebo nerovnost.

Vzhledem k tomu, že předdefinované operátory rovnosti typu odkazu přijímají operandy typu `object`, vztahují se na všechny typy, které nedeklarují příslušné členy `operator ==` a `operator !=`. Naopak všechny použitelné uživatelsky definované operátory rovnosti efektivně skryjí předdefinované operátory rovnosti typu odkazu.

Předdefinované operátory rovnosti referenčního typu vyžadují jednu z následujících možností:

*  Oba operandy jsou hodnota typu, který je známý jako *reference_type* nebo literál `null`. Kromě toho existuje explicitní převod odkazu ([explicitní převody odkazů](conversions.md#explicit-reference-conversions)) z typu jednoho operandu na typ druhého operandu.
*  Jeden operand je hodnota typu `T`, kde `T` je *type_parameter* a druhý operand je literál `null`. Navíc `T` nemá omezení typu hodnoty.

Pokud jedna z těchto podmínek není pravdivá, dojde k chybě při vazbě. Mezi významné důsledky těchto pravidel patří:

*  Jedná se o chybu při vazbě k použití předdefinovaného operátoru rovnosti referenčního typu pro porovnání dvou odkazů, které se označují jako odlišné v době vytváření vazby. Například pokud jsou typy v čase vazby operandy dva typy třídy `A` a `B` a pokud ani `A` ani `B` jsou odvozeny od druhé, pak by nebylo možné, že dva operandy odkazují na stejný objekt. Proto se operace považuje za chybu při vazbě.
*  Předdefinované operátory rovnosti referenčního typu nepovolují porovnání operandů typu hodnoty. Proto pokud typ struktury deklaruje své vlastní operátory rovnosti, není možné porovnat hodnoty tohoto typu struktury.
*  Předdefinované operátory rovnosti typu odkazu nikdy nezpůsobí operace zabalení pro své operandy. To by znamenalo, že by se tyto operace zabalení prováděly, protože odkazy na nově přiřazené zabalené instance by se nutně lišily od všech ostatních odkazů.
*  Pokud je operand typu parametru typu `T` porovnána s hodnotou `null` a typem běhu `T` je typ hodnoty, výsledkem porovnání je `false`.

Následující příklad ověří, zda je argument typu parametru bez omezení typu `null`.
```csharp
class C<T>
{
    void F(T x) {
        if (x == null) throw new ArgumentNullException();
        ...
    }
}
```

Konstrukce `x == null` je povolena, i když `T` může představovat typ hodnoty a výsledek je jednoduše definován jako `false`, pokud `T` je typ hodnoty.

Pro provoz formuláře `x == y` nebo `x != y`, pokud existují jakékoli platné `operator ==` nebo `operator !=`, pravidla pro rozlišení přetížení operátoru ([řešení přetížení binárního operátoru](expressions.md#binary-operator-overload-resolution)) vybere tento operátor místo předdefinované typu odkazu. podnikatel. Je však vždy možné vybrat předdefinovaný operátor rovnosti typu odkazu explicitním přetypováním jednoho nebo obou operandů na typ `object`. Příklad
```csharp
using System;

class Test
{
    static void Main() {
        string s = "Test";
        string t = string.Copy(s);
        Console.WriteLine(s == t);
        Console.WriteLine((object)s == t);
        Console.WriteLine(s == (object)t);
        Console.WriteLine((object)s == (object)t);
    }
}
```
Vytvoří výstup
```console
True
False
False
False
```

Proměnné `s` a `t` odkazují na dvě instance `string`, které obsahují stejné znaky. První porovnávací výstupy `True`, protože je vybrán předdefinovaný operátor rovnosti řetězců ([operátory rovnosti řetězců](expressions.md#string-equality-operators)), pokud jsou oba operandy typu `string`. Zbývající porovnávání všech výstupů `False`, protože je vybrán operátor rovnosti typu odkazu, když je jeden nebo oba operandy typu `object`.

Všimněte si, že výše uvedená technika není smysluplná pro typy hodnot. Příklad
```csharp
class Test
{
    static void Main() {
        int i = 123;
        int j = 123;
        System.Console.WriteLine((object)i == (object)j);
    }
}
```
výstupy `False`, protože přetypování vytvoří odkazy na dvě samostatné instance zabalené hodnoty `int`.

### <a name="string-equality-operators"></a>Operátory rovnosti řetězců

Předdefinované řetězcové operátory rovnosti jsou:
```csharp
bool operator ==(string x, string y);
bool operator !=(string x, string y);
```

Dvě hodnoty `string` se považují za stejné, pokud je splněna jedna z následujících hodnot:

*  Obě hodnoty jsou `null`.
*  Obě hodnoty jsou odkazy, které nejsou null, na instance řetězců, které mají stejné délky a stejné znaky v každé pozici znaku.

Operátory rovnosti řetězců porovnávají řetězcové hodnoty, nikoli odkazy na řetězce. Pokud dvě samostatné instance řetězců obsahují přesně stejnou sekvenci znaků, jsou hodnoty řetězců stejné, ale odkazy se liší. Jak je popsáno v tématu [operátory rovnosti typu odkazu](expressions.md#reference-type-equality-operators), lze operátory rovnosti typu odkazu použít k porovnání odkazů na řetězce namísto řetězcových hodnot.

### <a name="delegate-equality-operators"></a>Operátory rovnosti delegátů

Každý typ delegáta implicitně poskytuje následující předdefinované operátory porovnání:

```csharp
bool operator ==(System.Delegate x, System.Delegate y);
bool operator !=(System.Delegate x, System.Delegate y);
```

Dvě instance delegáta jsou považovány za stejné jako následující:

*  Pokud je jedna z instancí delegátů `null`, jsou shodné, pokud jsou `null`.
*  Pokud mají delegáty jiný typ běhu, nejsou nikdy rovny.
*  Pokud obě instance delegáta mají seznam volání ([deklarace delegátů](delegates.md#delegate-declarations)), jsou tyto instance stejné, pokud jsou jejich seznamy volání stejné délky, a každá položka v seznamu volání je stejná (jak je definováno níže) k odpovídajícímu záznam v pořadí v seznamu vyvolání.

Následující pravidla určují rovnost položek seznamu volání:

*  Pokud dvě položky seznamu volání odkazují na stejnou statickou metodu, pak jsou položky stejné.
*  Pokud dvě položky seznamu volání odkazují na stejnou nestatickou metodu na stejném cílovém objektu (jak je definováno operátory rovnosti referencí), pak jsou položky stejné.
*  Položky seznamu volání vytvořené z vyhodnocení sémanticky identických *anonymous_method_expression*s nebo *lambda_expression*s stejnou (možná prázdnou) sadou zachycených instancí vnějších proměnných jsou povolené (ale nevyžadují se). výši.

### <a name="equality-operators-and-null"></a>Operátory rovnosti a hodnota null

Operátory `==` a `!=` umožňují, aby jeden operand byl hodnota typu s možnou hodnotou null a druhý jako literál `null`, a to i v případě, že pro danou operaci neexistuje žádný předdefinovaný operátor nebo uživatelem definovaný operátor (v vyzdvižené nebo přezdvižené formě).

Pro provoz jedné z forem
```csharp
x == null
null == x
x != null
null != x
```
kde `x` je výraz typu s možnou hodnotou null, pokud řešení přetížení operátoru ([rozlišení přetížení binárního operátoru](expressions.md#binary-operator-overload-resolution)) nenajde příslušný operátor, výsledek je místo toho vypočítán z vlastnosti `HasValue` `x`. Konkrétně jsou první dva formuláře přeloženy do `!x.HasValue` a poslední dva formuláře jsou přeloženy do `x.HasValue`.

### <a name="the-is-operator"></a>Operátor is

Operátor `is` slouží k dynamické kontrole, zda je typ běhu objektu kompatibilní s daným typem. Výsledek operace `E is T`, kde `E` je výraz a `T` je typ, je logická hodnota označující, zda lze `E` převést na typ `T` pomocí převodu odkazu, převodu zabalení nebo převodu rozbalení. Tato operace je vyhodnocena následujícím způsobem, poté, co byly argumenty typu nahrazeny pro všechny parametry typu:

*  Pokud je `E` anonymní funkce, dojde k chybě při kompilaci.
*  Pokud je `E` skupinou metod nebo `null` literál, pokud je typ `E` odkazový typ nebo typ s možnou hodnotou null a hodnota `E` je null, výsledek je false.
*  Jinak Nechť `D` představuje dynamický typ `E` následujícím způsobem:
   * Pokud typ `E` je odkazový typ, `D` je běhový typ odkazu instance pomocí `E`.
   * Pokud typ `E` je typ s možnou hodnotou null, `D` je základní typ tohoto typu s možnou hodnotou null.
   * Pokud typ `E` je typ hodnoty, který neumožňuje hodnotu null, `D` je typ `E`.
*  Výsledek operace závisí na `D` a `T` následujícím způsobem:
   * Pokud je `T` odkazový typ, výsledek je true, pokud `D` a `T` mají stejný typ, pokud `D` je odkazový typ a implicitní referenční převod z `D` na `T` existuje, nebo pokud `D` je typ hodnoty a převod zabalení z `D`. na `T` existuje.
   * Pokud je `T` typem s možnou hodnotou null, výsledek je true, pokud je `D` základní typ `T`.
   * Pokud je `T` typ hodnoty, který neumožňuje hodnotu null, výsledek je true, pokud `D` a `T` jsou stejného typu.
   * V opačném případě je výsledkem false.

Počítejte s tím, že uživatelsky definované převody nejsou považovány za operátor `is`.

### <a name="the-as-operator"></a>Operátor as

Operátor `as` slouží k explicitnímu převodu hodnoty na daný typ odkazu nebo na typ s možnou hodnotou null. Na rozdíl od výrazu přetypování ([výrazy přetypování](expressions.md#cast-expressions)) operátor `as` nikdy nevyvolá výjimku. Místo toho, pokud uvedený převod není možný, výsledná hodnota je `null`.

V operaci formuláře `E as T` musí být `E` výraz a `T` musí být odkazový typ, parametr typu, který je známý jako typ odkazu, nebo typ s možnou hodnotou null. Kromě toho musí být alespoň jedna z následujících podmínek pravdivá nebo jinak dojde k chybě při kompilaci:

*  Identita ([převod identity](conversions.md#identity-conversion)), implicitně Nullable ([implicitní převody s možnou hodnotou null](conversions.md#implicit-nullable-conversions)), implicitní odkaz ([implicitní převody odkazů](conversions.md#implicit-reference-conversions)), zabalení ([převody zabalení](conversions.md#boxing-conversions)), explicitní Nullable (s[možnou hodnotou null) převody](conversions.md#explicit-nullable-conversions)), explicitní odkaz ([explicitní převody odkazů](conversions.md#explicit-reference-conversions)) nebo rozbalení ([převody rozbalení](conversions.md#unboxing-conversions)) existují z `E` na `T`.
*  Typ `E` nebo `T` je otevřený typ.
*  `E` je literál `null`.

Pokud typ doby kompilace `E` není `dynamic`, operace `E as T` vytvoří stejný výsledek jako
```csharp
E is T ? (T)(E) : (T)null
```
s výjimkou, že `E` je vyhodnocena pouze jednou. Kompilátor je možné očekávat pro optimalizaci `E as T`, aby bylo možné provést maximálně jednu kontrolu dynamického typu, a to na rozdíl od dvou kontrol dynamického typu, které jsou odvozené od výše uvedeného rozšíření.

Pokud je typ doby kompilace `E` `dynamic`, na rozdíl od operátoru přetypování není operátor `as` dynamicky svázán ([dynamická vazba](expressions.md#dynamic-binding)). Proto rozšíření v tomto případě je:
```csharp
E is T ? (T)(object)(E) : (T)null
```

Všimněte si, že některé převody, například uživatelsky definované převody, nejsou možné s operátorem `as` a měly by se provádět místo toho, aby je bylo možné provádět pomocí výrazů přetypování.

V příkladu
```csharp
class X
{

    public string F(object o) {
        return o as string;        // OK, string is a reference type
    }

    public T G<T>(object o) where T: Attribute {
        return o as T;             // Ok, T has a class constraint
    }

    public U H<U>(object o) {
        return o as U;             // Error, U is unconstrained 
    }
}
```
parametr typu `T` `G` je známý jako typ odkazu, protože má omezení třídy. Parametr typu `U` `H` ale nikoli; proto použití operátoru `as` v `H` není povoleno.

## <a name="logical-operators"></a>Logické operátory

Operátory `&`, `^` a `|` se nazývají logické operátory.

```antlr
and_expression
    : equality_expression
    | and_expression '&' equality_expression
    ;

exclusive_or_expression
    : and_expression
    | exclusive_or_expression '^' and_expression
    ;

inclusive_or_expression
    : exclusive_or_expression
    | inclusive_or_expression '|' exclusive_or_expression
    ;
```

Pokud má operand logického operátoru typ doby kompilace `dynamic`, je výraz dynamicky svázán ([dynamická vazba](expressions.md#dynamic-binding)). V tomto případě je typ doby kompilace výrazu `dynamic` a řešení popsané níže bude provedeno za běhu pomocí běhového typu u operandů, které mají typ doby kompilace `dynamic`.

Pro operaci formuláře `x op y`, kde `op` je jedním z logických operátorů, je pro výběr konkrétní implementace operátoru použito rozlišení přetížení ([rozlišení přetěžování binárních operátorů](expressions.md#binary-operator-overload-resolution)). Operandy jsou převedeny na typy parametrů vybraného operátoru a typ výsledku je návratový typ operátoru.

Předdefinované logické operátory jsou popsány v následujících částech.

### <a name="integer-logical-operators"></a>Logické operátory typu Integer

Předdefinované logické operátory typu Integer jsou:
```csharp
int operator &(int x, int y);
uint operator &(uint x, uint y);
long operator &(long x, long y);
ulong operator &(ulong x, ulong y);

int operator |(int x, int y);
uint operator |(uint x, uint y);
long operator |(long x, long y);
ulong operator |(ulong x, ulong y);

int operator ^(int x, int y);
uint operator ^(uint x, uint y);
long operator ^(long x, long y);
ulong operator ^(ulong x, ulong y);
```

Operátor `&` vypočítá bitový logický `AND` dvou operandů, operátor `|` Vypočítá bitový logický operátor `OR` dvou operandů a operátor `^` vypočítá bitový logický exkluzivní `OR` dvou operandů. Z těchto operací nejsou možné žádné přetečení.

### <a name="enumeration-logical-operators"></a>Logické operátory výčtu

Každý typ výčtu `E` implicitně poskytuje následující předdefinované logické operátory:

```csharp
E operator &(E x, E y);
E operator |(E x, E y);
E operator ^(E x, E y);
```

Výsledek vyhodnocení `x op y`, kde `x` a `y` jsou výrazy typu výčtu `E` s podkladovým typem `U` a `op` je jedním z logických operátorů, je naprosto stejný jako vyhodnocování `(E)((U)x op (U)y)`. Jinými slovy, logické operátory typu výčtu jednoduše provádějí logickou operaci na základním typu dvou operandů.

### <a name="boolean-logical-operators"></a>Logické operátory

Předdefinované logické operátory Boolean jsou:
```csharp
bool operator &(bool x, bool y);
bool operator |(bool x, bool y);
bool operator ^(bool x, bool y);
```

Výsledek `x & y` je `true`, pokud jsou `x` i `y` `true`. V opačném případě je výsledkem `false`.

Výsledek `x | y` je `true`, pokud je `x` nebo `y` `true`. V opačném případě je výsledkem `false`.

Výsledek `x ^ y` je `true`, pokud `x` je `true` a `y` je `false` nebo `x` je `false` a `y` je `true`. V opačném případě je výsledkem `false`. Pokud jsou operandy typu `bool`, operátor `^` vypočítá stejný výsledek jako operátor `!=`.

### <a name="nullable-boolean-logical-operators"></a>Logické operátory s možnou hodnotou null

Typ Boolean s možnou hodnotou null `bool?` může představovat tři hodnoty, `true`, `false` a `null` a je koncepčně podobný typu se třemi hodnotami použitými pro logické výrazy v SQL. Chcete-li zajistit, aby výsledky vytvořené operátory `&` a `|` pro operandy `bool?` byly konzistentní s logikou se třemi hodnotami SQL, jsou k dispozici následující předdefinované operátory:

```csharp
bool? operator &(bool? x, bool? y);
bool? operator |(bool? x, bool? y);
```

V následující tabulce jsou uvedeny výsledky, které tyto operátory vygenerovaly pro všechny kombinace hodnot `true`, `false` a `null`.

| `x`     | `y`     | `x & y` | <code>x &#124; y</code> |
|:-------:|:-------:|:-------:|:-------:|
| `true`  | `true`  | `true`  | `true`  | 
| `true`  | `false` | `false` | `true`  | 
| `true`  | `null`  | `null`  | `true`  | 
| `false` | `true`  | `false` | `true`  | 
| `false` | `false` | `false` | `false` | 
| `false` | `null`  | `false` | `null`  | 
| `null`  | `true`  | `null`  | `true`  | 
| `null`  | `false` | `false` | `null`  | 
| `null`  | `null`  | `null`  | `null`  | 

## <a name="conditional-logical-operators"></a>Podmíněné logické operátory

Operátory `&&` a `||` se nazývají Podmíněné logické operátory. Označují se také jako logické operátory "krátkodobého okruhu".

```antlr
conditional_and_expression
    : inclusive_or_expression
    | conditional_and_expression '&&' inclusive_or_expression
    ;

conditional_or_expression
    : conditional_and_expression
    | conditional_or_expression '||' conditional_and_expression
    ;
```

Operátory `&&` a `||` jsou podmíněné verze operátorů `&` a `|`:

*  Operace `x && y` odpovídá operaci `x & y` s tím rozdílem, že `y` je vyhodnocena pouze v případě, že `x` není `false`.
*  Operace `x || y` odpovídá operaci `x | y` s tím rozdílem, že `y` je vyhodnocena pouze v případě, že `x` není `true`.

Pokud má operand podmíněného logického operátoru typ doby kompilace `dynamic`, pak je výraz dynamicky svázán ([dynamická vazba](expressions.md#dynamic-binding)). V tomto případě je typ doby kompilace výrazu `dynamic` a řešení popsané níže bude provedeno za běhu pomocí běhového typu u operandů, které mají typ doby kompilace `dynamic`.

Operace formuláře `x && y` nebo `x || y` se zpracovává pomocí Rozlišení přetěžování ([rozlišení přetížení binárního operátoru](expressions.md#binary-operator-overload-resolution)), jako kdyby byla operace zapsána `x & y` nebo `x | y`. Stisknutím

*  Pokud rozlišení přetížení nenajde jeden nejlepší operátor, nebo pokud řešení přetížení vybere jeden z předdefinovaných logických logických operátorů, dojde k chybě při vazbě.
*  V opačném případě, pokud je vybraný operátor jedním z předdefinovaných logických logických operátorů ([logických logických operátorů](expressions.md#boolean-logical-operators)) nebo hodnot s možnou hodnotou null logické operátory (s[možnou hodnotou null logických operátorů](expressions.md#nullable-boolean-logical-operators)), bude zpracován [ Logické Podmíněné logické operátory](expressions.md#boolean-conditional-logical-operators).
*  V opačném případě je vybraný operátor uživatelem definovaný operátor a operace je zpracována tak, jak je popsáno v [uživatelsky definovaných podmíněných logických operátorech](expressions.md#user-defined-conditional-logical-operators).

Není možné přímo přetížit Podmíněné logické operátory. Vzhledem k tomu, že podmíněné logické operátory jsou vyhodnocovány v souvislosti s běžnými logickými operátory, jsou přetížení běžných logických operátorů, s určitými omezeními, také považována za přetížení podmíněných logických operátorů. To je podrobněji popsáno v [uživatelsky definovaných podmíněných logických operátorech](expressions.md#user-defined-conditional-logical-operators).

### <a name="boolean-conditional-logical-operators"></a>Logické Podmíněné logické operátory

Pokud jsou operandy `&&` nebo `||` typu `bool` nebo pokud jsou operandy typů, které nedefinují příslušné `operator &` nebo `operator |`, ale definují implicitní převody na `bool`, operace je zpracována následujícím způsobem. :

*  Operace `x && y` je vyhodnocena jako `x ? y : false`. Jinými slovy, `x` se nejprve vyhodnotí a převede na typ `bool`. Pokud je pak `x` `true`, `y` se vyhodnotí a převede na typ `bool`, což se stalo výsledkem operace. V opačném případě je výsledkem operace `false`.
*  Operace `x || y` je vyhodnocena jako `x ? true : y`. Jinými slovy, `x` se nejprve vyhodnotí a převede na typ `bool`. Pokud je `x` `true`, výsledek operace je @no__t – 2. V opačném případě je `y` vyhodnocena a převedena na typ `bool`, což se stalo výsledkem operace.

### <a name="user-defined-conditional-logical-operators"></a>Podmíněné logické operátory definované uživatelem

Pokud jsou operandy `&&` nebo `||` typu, který deklaruje příslušné uživatelsky definované `operator &` nebo `operator |`, musí být obě následující hodnoty true, kde `T` je typ, ve kterém je deklarovaný vybraný operátor:

*  Návratový typ a typ každého parametru vybraného operátoru musí být `T`. Jinými slovy, operátor musí vypočítat logický `AND` nebo logický `OR` dvou operandech typu `T` a musí vracet výsledek typu `T`.
*  `T` musí obsahovat deklarace `operator true` a `operator false`.

Pokud některý z těchto požadavků není splněn, dojde k chybě při vazbě. V opačném případě je operace `&&` nebo `||` vyhodnocena kombinací uživatelsky definovaného `operator true` nebo `operator false` s vybraným uživatelem definovaným operátorem:

*  Operace `x && y` je vyhodnocena jako `T.false(x) ? x : T.&(x, y)`, kde `T.false(x)` je vyvolání `operator false` deklarovaného v `T` a `T.&(x, y)` je vyvolání vybrané `operator &`. Jinými slovy, `x` se nejprve vyhodnotí a vyvolá se `operator false` ve výsledku, aby bylo možné zjistit, jestli je `x` jednoznačně false. Pokud je `x` jednoznačně false, výsledek operace je hodnota dříve vypočítaná pro `x`. V opačném případě je vyhodnocena hodnota `y` a vybraný `operator &` je vyvolán na hodnotu dříve vypočítanou pro `x` a hodnotu vypočítanou pro `y` pro získání výsledku operace.
*  Operace `x || y` je vyhodnocena jako `T.true(x) ? x : T.|(x, y)`, kde `T.true(x)` je vyvolání `operator true` deklarovaného v `T` a `T.|(x,y)` je vyvolání vybrané `operator|`. Jinými slovy @no__t – 0 se nejprve vyhodnotí a `operator true` je vyvoláno ve výsledku, abyste zjistili, zda je hodnota `x` jednoznačně pravdivá. Pokud je `x` jednoznačně true, výsledek operace je hodnota dříve vypočítaná pro `x`. V opačném případě je vyhodnocena hodnota `y` a vybraný `operator |` je vyvolán na hodnotu dříve vypočítanou pro `x` a hodnotu vypočítanou pro `y` pro získání výsledku operace.

V některé z těchto operací je výraz zadaný pomocí `x` vyhodnocován pouze jednou a výraz zadaný `y` není vyhodnocen nebo vyhodnocován přesně jednou.

Příklad typu, který implementuje `operator true` a `operator false`, naleznete v tématu [Database Boolean Type](structs.md#database-boolean-type).

## <a name="the-null-coalescing-operator"></a>Operátor slučování s hodnotou null

Operátor `??` se nazývá slučovací operátor null.

```antlr
null_coalescing_expression
    : conditional_or_expression
    | conditional_or_expression '??' null_coalescing_expression
    ;
```

Nulový slučovací výraz formuláře `a ?? b` vyžaduje, aby `a` bylo typu s možnou hodnotou null nebo odkazem. Pokud `a` není null, výsledkem `a ?? b` je `a`; v opačném případě je výsledkem `b`. Tato operace vyhodnotí `b` pouze v případě, že `a` má hodnotu null.

Operátor slučování s hodnotou null je asociativní zprava, což znamená, že operace jsou seskupeny zprava doleva. Například výraz formuláře `a ?? b ?? c` je vyhodnocen jako `a ?? (b ?? c)`. Obecně platí, že výraz formuláře `E1 ?? E2 ?? ... ?? En` vrací první z operandů, které nejsou null, nebo hodnotu null, pokud jsou všechny operandy NULL.

Typ výrazu `a ?? b` závisí na tom, které implicitní převody jsou k dispozici u operandů. V upřednostňovaném pořadí je typ `a ?? b` `A0`, `A` nebo `B`, kde `A` je typ `a` (za předpokladu, že `a` je typu), `B` je typ `b` (za předpokladu, že `b` má typ). a 0 je základní typ 1, pokud 2 je typ s možnou hodnotou null nebo 3 v opačném případě. Konkrétně `a ?? b` se zpracovává takto:

*  Pokud `A` existuje a není to typ s možnou hodnotou null nebo odkazový typ, dojde k chybě při kompilaci.
*  Pokud je `b` dynamický výraz, je typ výsledku `dynamic`. V době běhu se nejprve vyhodnotí `a`. Pokud `a` není null, je `a` převedena na Dynamic, což se změní na výsledek. V opačném případě je vyhodnocena hodnota `b`, což se zobrazí jako výsledek.
*  Jinak, pokud `A` existuje a jde o typ s možnou hodnotou null a implicitní převod existuje z `b` na `A0`, je typ výsledku `A0`. V době běhu se nejprve vyhodnotí `a`. Pokud `a` není null, `a` je rozbalením do typu `A0`, a to se stalo výsledkem. V opačném případě je `b` vyhodnocena a převedena na typ `A0`, což se zobrazí jako výsledek.
*  V opačném případě, pokud `A` existuje a implicitní převod existuje z `b` na `A`, je typ výsledku `A`. V době běhu se nejprve vyhodnotí `a`. Pokud `a` není null, bude výsledkem `a`. V opačném případě je `b` vyhodnocena a převedena na typ `A`, což se zobrazí jako výsledek.
*  Jinak, pokud `b` má typ `B` a implicitní převod existuje z `a` na `B`, je typ výsledku `B`. V době běhu se nejprve vyhodnotí `a`. Pokud `a` není null, `a` se rozbalí do typu `A0` (Pokud `A` existuje a je null) a převedeno na typ `B`, což se stalo výsledkem. V opačném případě se vyhodnotí `b` a výsledkem bude výsledek.
*  V opačném případě jsou `a` a `b` nekompatibilní a dojde k chybě při kompilaci.

## <a name="conditional-operator"></a>Podmíněný operátor

Operátor `?:` se nazývá podmíněný operátor. Je někdy také označován jako Ternární operátor.

```antlr
conditional_expression
    : null_coalescing_expression
    | null_coalescing_expression '?' expression ':' expression
    ;
```

Podmíněný výraz formuláře `b ? x : y` nejprve vyhodnotí podmínku `b`. Pokud je pak `b` `true`, vyhodnotí se `x` a výsledkem operace je. V opačném případě se `y` vyhodnotí a výsledkem operace se. Podmíněný výraz nikdy nevyhodnotí `x` a `y`.

Podmíněný operátor je asociativní zprava, což znamená, že operace jsou seskupeny zprava doleva. Například výraz formuláře `a ? b : c ? d : e` je vyhodnocen jako `a ? b : (c ? d : e)`.

Prvním operandem operátoru `?:` musí být výraz, který lze implicitně převést na `bool` nebo výraz typu, který implementuje `operator true`. Pokud ani jeden z těchto požadavků není splněn, dojde k chybě při kompilaci.

Druhý a třetí operand `x` a `y` ovládacího prvku operátoru `?:` typ podmíněného výrazu.

*  Pokud je `x` typu `X` a `y` je typu `Y` then
   * Pokud implicitní převod ([implicitní převody](conversions.md#implicit-conversions)) existuje z `X` na `Y`, ale ne z `Y` na `X`, pak `Y` je typ podmíněného výrazu.
   * Pokud implicitní převod ([implicitní převody](conversions.md#implicit-conversions)) existuje z `Y` na `X`, ale ne z `X` na `Y`, pak `X` je typ podmíněného výrazu.
   * V opačném případě nelze určit žádný typ výrazu a dojde k chybě při kompilaci.
*  Pokud je typ pouze jeden z `x` a `y`, je implicitně převoditelné na tento typ a obou `x` a `y`, a to je typ podmíněného výrazu.
*  V opačném případě nelze určit žádný typ výrazu a dojde k chybě při kompilaci.

Běhové zpracování podmíněného výrazu ve formátu `b ? x : y` se skládá z následujících kroků:

*  Nejprve je vyhodnocena hodnota `b` a je určena hodnota `bool` `b`:
   * Pokud existuje implicitní převod z typu `b` na `bool`, pak se tento implicitní převod provede tak, aby se vytvořila hodnota `bool`.
   * V opačném případě je vyvoláno `operator true` definované typem `b`, aby se vytvořila hodnota `bool`.
*  Pokud hodnota `bool` vytvořená výše v kroku je `true`, pak `x` je vyhodnocena a převedena na typ podmíněného výrazu, a to se projeví v důsledku podmíněného výrazu.
*  V opačném případě je `y` vyhodnocena a převedena na typ podmíněného výrazu, a to se projeví v důsledku podmíněného výrazu.

## <a name="anonymous-function-expressions"></a>Anonymní výrazy funkcí

***Anonymní funkce*** je výraz, který představuje definici metody "in-line". Anonymní funkce nemá hodnotu ani typ v rámci sebe, ale je převoditelná na kompatibilního delegáta nebo na typ stromu výrazů. Vyhodnocení konverze anonymní funkce závisí na typu cíle převodu: Pokud se jedná o typ delegáta, převod se vyhodnotí na hodnotu delegáta odkazující na metodu, kterou definuje anonymní funkce. Pokud se jedná o typ stromu výrazu, převod se vyhodnotí na strom výrazu, který představuje strukturu metody jako strukturu objektu.

Z historických důvodů existují dvě syntaktické charakter anonymních funkcí, konkrétně *lambda_expression*s a *anonymous_method_expression*s. Pro téměř všechny účely jsou *lambda_expression*s stručnější a výraznou výjimkou *anonymous_method_expression*s, které zůstávají v jazyce pro zpětnou kompatibilitu.

```antlr
lambda_expression
    : anonymous_function_signature '=>' anonymous_function_body
    ;

anonymous_method_expression
    : 'delegate' explicit_anonymous_function_signature? block
    ;

anonymous_function_signature
    : explicit_anonymous_function_signature
    | implicit_anonymous_function_signature
    ;

explicit_anonymous_function_signature
    : '(' explicit_anonymous_function_parameter_list? ')'
    ;

explicit_anonymous_function_parameter_list
    : explicit_anonymous_function_parameter (',' explicit_anonymous_function_parameter)*
    ;

explicit_anonymous_function_parameter
    : anonymous_function_parameter_modifier? type identifier
    ;

anonymous_function_parameter_modifier
    : 'ref'
    | 'out'
    ;

implicit_anonymous_function_signature
    : '(' implicit_anonymous_function_parameter_list? ')'
    | implicit_anonymous_function_parameter
    ;

implicit_anonymous_function_parameter_list
    : implicit_anonymous_function_parameter (',' implicit_anonymous_function_parameter)*
    ;

implicit_anonymous_function_parameter
    : identifier
    ;

anonymous_function_body
    : expression
    | block
    ;
```

Operátor `=>` má stejnou prioritu jako přiřazení (`=`) a je asociativní zprava.

Anonymní funkce s modifikátorem `async` je asynchronní funkce a postupuje podle pravidel popsaných v [iterátorech](classes.md#iterators).

Parametry anonymní funkce ve formě *lambda_expression* mohou být explicitně nebo implicitně typované. V seznamu explicitně typovaného parametru je typ každého parametru explicitně uveden. V seznamu implicitně typových parametrů jsou typy parametrů odvoditelné z kontextu, ve kterém se anonymní funkce vyskytují – konkrétně, když je anonymní funkce převedena na kompatibilní typ delegáta nebo na typ stromu výrazů, který tento typ poskytuje. typy parametrů ([anonymní převody funkcí](conversions.md#anonymous-function-conversions)).

V anonymní funkci s jedním implicitně typovým parametrem mohou být kulaté závorky vynechány v seznamu parametrů. Jinými slovy, anonymní funkce formuláře
```csharp
( param ) => expr
```
může být zkrácen na
```csharp
param => expr
```

Seznam parametrů anonymní funkce ve formě *anonymous_method_expression* je nepovinný. Je-li tento parametr zadán, musí být parametry explicitně zadány. V takovém případě je anonymní funkce převoditelná na delegáta s jakýmkoli seznamem parametrů, který neobsahuje parametry `out`.

Tělo *bloku* anonymní funkce je dosažitelné ([koncové body a dosažitelnost](statements.md#end-points-and-reachability)), pokud anonymní funkce nedochází uvnitř nedosažitelného příkazu.

Příklady anonymních funkcí jsou následující:

```csharp
x => x + 1                              // Implicitly typed, expression body
x => { return x + 1; }                  // Implicitly typed, statement body
(int x) => x + 1                        // Explicitly typed, expression body
(int x) => { return x + 1; }            // Explicitly typed, statement body
(x, y) => x * y                         // Multiple parameters
() => Console.WriteLine()               // No parameters
async (t1,t2) => await t1 + await t2    // Async
delegate (int x) { return x + 1; }      // Anonymous method expression
delegate { return 1 + 1; }              // Parameter list omitted
```

Chování *lambda_expression*s a *anonymous_method_expression*s se shoduje s výjimkou následujících bodů:

*  *anonymous_method_expression*s umožní, aby byl seznam parametrů zcela vynechán, a to tak, že převoditelnosti na typy delegátů všech seznamů hodnot parametrů.
*  *lambda_expression*s povoluje, aby byly typy parametrů vynechány a odvozeny, zatímco *anonymous_method_expression*s vyžadují explicitní zadání typů parametrů.
*  Tělo *lambda_expression* může být výraz nebo blok příkazu, zatímco tělo *anonymous_method_expression* musí být blok příkazu.
*  Pouze *lambda_expression*mají převody na kompatibilní typy stromu výrazů ([typy stromu výrazů](types.md#expression-tree-types)).

### <a name="anonymous-function-signatures"></a>Podpisy anonymních funkcí

Volitelná *anonymous_function_signature* anonymní funkce definuje názvy a volitelně typy formálních parametrů anonymní funkce. Rozsah parametrů anonymní funkce je *anonymous_function_body*. ([Rozsahy](basic-concepts.md#scopes)) Společně se seznamem parametrů (Pokud je uvedeno), představuje tělo anonymní metody místo deklarace ([deklarace](basic-concepts.md#declarations)). V důsledku toho je chyba kompilace pro název parametru anonymní funkce tak, aby odpovídala názvu místní proměnné, místní konstanty nebo parametru, jehož obor zahrnuje *anonymous_method_expression* nebo *lambda_expression*.

Pokud má anonymní funkce *explicit_anonymous_function_signature*, pak sada kompatibilních typů delegátů a typů stromu výrazů je omezena na ty, které mají stejné typy parametrů a modifikátory ve stejném pořadí. Na rozdíl od převodů skupin metod ([Převod skupin metod](conversions.md#method-group-conversions)) je kontraindikace Nepodporovaná – variance typů parametrů anonymní funkce. Pokud anonymní funkce nemá *anonymous_function_signature*, pak sada kompatibilních typů delegátů a typů stromu výrazů je omezena na ty, které nemají žádné parametry `out`.

Všimněte si, že *anonymous_function_signature* nemůže obsahovat atributy ani pole parametrů. Nicméně *anonymous_function_signature* může být kompatibilní s typem delegáta, jehož seznam parametrů obsahuje pole parametrů.

Všimněte si také, že převod na typ stromu výrazu, i když je kompatibilní, může stále selhat v době kompilace ([typy stromu výrazů](types.md#expression-tree-types)).

### <a name="anonymous-function-bodies"></a>Anonymní orgány funkcí

Tělo (*výraz* nebo *blok*) anonymní funkce podléhá následujícím pravidlům:

*  Pokud anonymní funkce obsahuje podpis, jsou v těle k dispozici parametry zadané v podpisu. Pokud anonymní funkce nemá žádný podpis, může být převedena na typ delegáta nebo typ výrazu s parametry ([anonymní převody funkcí](conversions.md#anonymous-function-conversions)), ale k těmto parametrům nelze přistupovat v těle.
*  S výjimkou parametrů `ref` nebo `out` zadaných v signatuře (pokud existuje) nejbližší nadřazené anonymní funkce se jedná o chybu při kompilaci pro tělo pro přístup k parametru `ref` nebo `out`.
*  Pokud je typ `this` typem struktury, jedná se o chybu při kompilaci, která je pro tělo přístup k `this`. To je pravdivé bez ohledu na to, zda je přístup explicitní (jako v `this.x`) nebo implicitní (jako v `x`, kde `x` je člen instance struktury). Toto pravidlo jednoduše zakazuje takový přístup a nemá vliv na to, jestli výsledky hledání členů jsou v členu struktury.
*  Tělo má přístup k vnějším proměnným ([vnějším proměnným](expressions.md#outer-variables)) anonymní funkce. Přístup k vnější proměnné bude odkazovat na instanci proměnné, která je aktivní v okamžiku vyhodnocení *lambda_expression* nebo *anonymous_method_expression* ([vyhodnocení anonymních výrazů funkce](expressions.md#evaluation-of-anonymous-function-expressions)).
*  Jedná se o chybu při kompilaci pro tělo, které obsahuje příkaz `goto`, příkaz `break` nebo příkaz `continue`, jehož cíl je mimo tělo nebo v těle obsažené anonymní funkce.
*  Příkaz `return` v těle vrátí řízení od vyvolání nejbližší nadřazené anonymní funkce, nikoli nadřazeného člena funkce. Výraz zadaný v příkazu `return` musí být implicitně převoditelný na návratový typ typu delegáta nebo stromu výrazů, na který je převeden nejbližší nadřazený objekt *lambda_expression* nebo *anonymous_method_expression* ( [Anonymní převody funkcí](conversions.md#anonymous-function-conversions)).

Explicitně neurčuje, zda existuje nějaký způsob, jak spustit blok anonymní funkce jiné než prostřednictvím vyhodnocení a volání *lambda_expression* nebo *anonymous_method_expression*. Konkrétně se může kompilátor rozhodnout implementovat anonymní funkci vysyntetizující jednu nebo více pojmenovaných metod nebo typů. Názvy těchto syntetizních prvků musí mít tvar vyhrazený pro použití kompilátorem.

### <a name="overload-resolution-and-anonymous-functions"></a>Rozlišení přetěžování a anonymní funkce

Anonymní funkce v seznamu argumentů se účastní odvození typu a řešení přetížení. Přesné pravidla naleznete v tématu věnovaném [odvození typů](expressions.md#type-inference) a [rozlišení přetěžování](expressions.md#overload-resolution) .

Následující příklad ukazuje účinek anonymních funkcí na řešení přetížení.

```csharp
class ItemList<T>: List<T>
{
    public int Sum(Func<T,int> selector) {
        int sum = 0;
        foreach (T item in this) sum += selector(item);
        return sum;
    }

    public double Sum(Func<T,double> selector) {
        double sum = 0;
        foreach (T item in this) sum += selector(item);
        return sum;
    }
}
```

Třída `ItemList<T>` má dvě metody `Sum`. Každá z nich přebírá argument `selector`, který extrahuje hodnotu, která se má sečíst z položky seznamu. Extrahovaná hodnota může být buď `int`, nebo `double` a výsledný součet je také buď `int`, nebo `double`.

Metody `Sum` můžete použít například k výpočtu součtů ze seznamu řádků podrobností v objednávce.

```csharp
class Detail
{
    public int UnitCount;
    public double UnitPrice;
    ...
}

void ComputeSums() {
    ItemList<Detail> orderDetails = GetOrderDetails(...);
    int totalUnits = orderDetails.Sum(d => d.UnitCount);
    double orderTotal = orderDetails.Sum(d => d.UnitPrice * d.UnitCount);
    ...
}
```

Při prvním vyvolání `orderDetails.Sum` lze použít obě metody `Sum`, protože anonymní funkce `d => d. UnitCount` je kompatibilní s `Func<Detail,int>` a `Func<Detail,double>`. Řešení přetížení však vybere první metodu `Sum`, protože převod na `Func<Detail,int>` je lepší než převod na `Func<Detail,double>`.

Při druhém vyvolání `orderDetails.Sum` lze použít pouze druhou metodu `Sum`, protože anonymní funkce `d => d.UnitPrice * d.UnitCount` vytvoří hodnotu typu `double`. Proto řešení přetížení vybere druhou metodu `Sum` pro toto vyvolání.

### <a name="anonymous-functions-and-dynamic-binding"></a>Anonymní funkce a dynamická vazba

Anonymní funkce nemůže být přijímač, argument nebo operand dynamicky vázané operace.

### <a name="outer-variables"></a>Vnější proměnné

Všechny místní proměnné, hodnoty parametru nebo pole parametrů, jejichž obor zahrnuje *lambda_expression* nebo *anonymous_method_expression* , se nazývají ***vnější proměnná*** anonymní funkce. V členu funkce instance třídy je hodnota `this` považována za parametr hodnoty a je vnější proměnná všech anonymních funkcí obsažených v rámci člena funkce.

#### <a name="captured-outer-variables"></a>Zachycené vnější proměnné

Pokud je vnější proměnná odkazována anonymní funkcí, je vnější proměnná označována jako ***zachycena*** anonymní funkcí. Obvykle je doba života místní proměnné omezena na provedení bloku nebo příkazu, ke kterému je přidruženo ([místní proměnné](variables.md#local-variables)). Doba trvání zachycené vnější proměnné se ale rozšířila aspoň na to, dokud se stromové struktury nebo stromu výrazů vytvořené z anonymní funkce neprojeví pro uvolňování paměti.

V příkladu
```csharp
using System;

delegate int D();

class Test
{
    static D F() {
        int x = 0;
        D result = () => ++x;
        return result;
    }

    static void Main() {
        D d = F();
        Console.WriteLine(d());
        Console.WriteLine(d());
        Console.WriteLine(d());
    }
}
```
lokální proměnná `x` je zachycena anonymní funkcí a životnost `x` je prodloužena alespoň do doby, než se delegát vrácený z `F` stane nárokem na uvolňování paměti (ke kterému nedojde až do konce programu). Vzhledem k tomu, že každé vyvolání anonymní funkce funguje na stejné instanci `x`, výstup tohoto příkladu je:
```console
1
2
3
```

Je-li lokální proměnná nebo parametr hodnoty zachycen anonymní funkcí, místní proměnná nebo parametr již není považována za pevnou proměnnou ([pevné a mobilní proměnné](unsafe-code.md#fixed-and-moveable-variables)), ale místo toho je považována za pohyblivou proměnnou. Proto každý kód `unsafe`, který přebírá adresu zachycené vnější proměnné, musí nejprve použít příkaz `fixed` k opravě proměnné.

Všimněte si, že na rozdíl od nezachycené proměnné může být zachycená lokální proměnná současně vystavena více vláknům provádění.

#### <a name="instantiation-of-local-variables"></a>Vytváření instancí místních proměnných

Místní proměnná je považována za ***instanci*** , pokud provádění vstoupí do oboru proměnné. Například pokud je vyvolána následující metoda, místní proměnná `x` je vytvořena a inicializována třikrát – jednou pro každou iteraci smyčky.

```csharp
static void F() {
    for (int i = 0; i < 3; i++) {
        int x = i * 2 + 1;
        ...
    }
}
```

Nicméně přesun deklarace `x` mimo smyčku má za následek jednu instanci `x`:
```csharp
static void F() {
    int x;
    for (int i = 0; i < 3; i++) {
        x = i * 2 + 1;
        ...
    }
}
```

Pokud není zachyceno, neexistuje způsob, jak přesně sledovat, jak často je vytvořena instance místní proměnné – protože životnost instancí jsou nesouvislé, je možné, že každá instance bude jednoduše používat stejné umístění úložiště. Nicméně, pokud anonymní funkce zachytí místní proměnnou, projeví se zřejmé účinky vytváření instancí.

Příklad
```csharp
using System;

delegate void D();

class Test
{
    static D[] F() {
        D[] result = new D[3];
        for (int i = 0; i < 3; i++) {
            int x = i * 2 + 1;
            result[i] = () => { Console.WriteLine(x); };
        }
        return result;
    }

    static void Main() {
        foreach (D d in F()) d();
    }
}
```
Vytvoří výstup:
```console
1
3
5
```

Pokud je však deklarace `x` přesunuta mimo smyčku:
```csharp
static D[] F() {
    D[] result = new D[3];
    int x;
    for (int i = 0; i < 3; i++) {
        x = i * 2 + 1;
        result[i] = () => { Console.WriteLine(x); };
    }
    return result;
}
```
Výstup je:
```console
5
5
5
```

Pokud smyčka for deklaruje proměnnou iterace, tato proměnná je považována za deklaraci mimo smyčku. Proto, pokud je změněn příklad pro zachycení samotné proměnné iterace:

```csharp
static D[] F() {
    D[] result = new D[3];
    for (int i = 0; i < 3; i++) {
        result[i] = () => { Console.WriteLine(i); };
    }
    return result;
}
```
je zachycena pouze jedna instance iterační proměnné, která vytváří výstup:
```console
3
3
3
```

Je možné, aby delegáti anonymní funkce sdíleli některé zachycené proměnné, ale mají samostatné instance jiných. Například pokud se `F` změní na
```csharp
static D[] F() {
    D[] result = new D[3];
    int x = 0;
    for (int i = 0; i < 3; i++) {
        int y = 0;
        result[i] = () => { Console.WriteLine("{0} {1}", ++x, ++y); };
    }
    return result;
}
```
tři Delegáti zachytí stejnou instanci `x`, ale samostatné instance `y`, a výstupem je:
```console
1 1
2 1
3 1
```

Samostatné anonymní funkce mohou zachytit stejnou instanci vnější proměnné. V tomto příkladu:
```csharp
using System;

delegate void Setter(int value);

delegate int Getter();

class Test
{
    static void Main() {
        int x = 0;
        Setter s = (int value) => { x = value; };
        Getter g = () => { return x; };
        s(5);
        Console.WriteLine(g());
        s(10);
        Console.WriteLine(g());
    }
}
```
Tyto dvě anonymní funkce zachytí stejnou instanci místní proměnné `x` a mohou tedy "komunikovat" prostřednictvím této proměnné. Výstup tohoto příkladu je:
```console
5
10
```

### <a name="evaluation-of-anonymous-function-expressions"></a>Vyhodnocení anonymních výrazů funkce

Anonymní funkce `F` musí být vždy převedena na typ delegáta `D` nebo na typ stromu výrazu `E`, a to buď přímo, nebo prostřednictvím provedení výrazu vytvoření delegáta `new D(F)`. Tento převod Určuje výsledek anonymní funkce, jak je popsáno v tématu [anonymní převody funkcí](conversions.md#anonymous-function-conversions).

## <a name="query-expressions"></a>Výrazy dotazů

***Výrazy dotazů*** poskytují syntaxi jazyka integrovanou do dotazů, které jsou podobné relačním a hierarchickým dotazovacím jazykům, jako jsou SQL a XQuery.

```antlr
query_expression
    : from_clause query_body
    ;

from_clause
    : 'from' type? identifier 'in' expression
    ;

query_body
    : query_body_clauses? select_or_group_clause query_continuation?
    ;

query_body_clauses
    : query_body_clause
    | query_body_clauses query_body_clause
    ;

query_body_clause
    : from_clause
    | let_clause
    | where_clause
    | join_clause
    | join_into_clause
    | orderby_clause
    ;

let_clause
    : 'let' identifier '=' expression
    ;

where_clause
    : 'where' boolean_expression
    ;

join_clause
    : 'join' type? identifier 'in' expression 'on' expression 'equals' expression
    ;

join_into_clause
    : 'join' type? identifier 'in' expression 'on' expression 'equals' expression 'into' identifier
    ;

orderby_clause
    : 'orderby' orderings
    ;

orderings
    : ordering (',' ordering)*
    ;

ordering
    : expression ordering_direction?
    ;

ordering_direction
    : 'ascending'
    | 'descending'
    ;

select_or_group_clause
    : select_clause
    | group_clause
    ;

select_clause
    : 'select' expression
    ;

group_clause
    : 'group' expression 'by' expression
    ;

query_continuation
    : 'into' identifier query_body
    ;
```

Výraz dotazu začíná klauzulí `from` a končí klauzulí `select` nebo `group`. Počáteční klauzule `from` může následovat nula nebo více klauzulí `from`, `let`, `where`, `join` nebo `orderby`. Každá klauzule `from` je generátorem, který zavádí ***proměnnou rozsahu*** , která je v rozsahu nad prvky ***sekvence***. Každá klauzule `let` zavádí proměnnou rozsahu reprezentující hodnotu vypočítanou pomocí předchozích proměnných rozsahu. Každá klauzule `where` je filtr, který vyloučí položky z výsledku. Každá klauzule `join` porovná zadané klíče zdrojové sekvence s klíči jiné sekvence a předává dvojice párů. Každá klauzule `orderby` změní pořadí položek podle zadaných kritérií. Poslední klauzule `select` nebo `group` určuje tvar výsledku z hodnot proměnných rozsahu. Nakonec můžete použít klauzuli `into` k "spojování" dotazům tím, že se výsledky jednoho dotazu považují za generátor v následném dotazu.

### <a name="ambiguities-in-query-expressions"></a>Nejednoznačné výrazy ve výrazech dotazů

Výrazy dotazů obsahují počet kontextových klíčových slov, tj. identifikátory, které mají v daném kontextu zvláštní význam. Konkrétně se jedná o `from`, `where`, `join`, `on`, `equals`, `into`, `let`, `orderby`, `ascending`, `descending`, 0, 1 a 2. Aby nedocházelo k nejednoznačnosti ve výrazech dotazů způsobených smíšeným použitím těchto identifikátorů jako klíčová slova nebo jednoduché názvy, jsou tyto identifikátory považovány za klíčová slova při výskytu kamkoli v rámci výrazu dotazu.

Pro tento účel výraz dotazu je libovolný výraz, který začíná řetězcem "`from identifier`" následovaný jakýmkoli tokenem, s výjimkou "`;`", "`=`" nebo "`,`".

Aby bylo možné použít tato slova jako identifikátory v rámci výrazu dotazu, mohou být předponou "`@`" ([identifikátory](lexical-structure.md#identifiers)).

### <a name="query-expression-translation"></a>Překlad výrazu dotazu

C# Jazyk neurčuje sémantiku provádění výrazů dotazů. Místo toho jsou výrazy dotazu přeloženy na vyvolání metod, které vyhovují *vzoru výrazu dotazu* ([vzor výrazu dotazu](expressions.md#the-query-expression-pattern)). Výrazy dotazů jsou konkrétně přeloženy na vyvolání metod s názvem `Where`, `Select`, `SelectMany`, `Join`, `GroupJoin`, `OrderBy`, `OrderByDescending`, `ThenBy`, `ThenByDescending`, `GroupBy` a 0. U těchto metod se očekává, že budou mít konkrétní signatury a typy výsledků, jak je popsáno ve [vzoru výrazu dotazu](expressions.md#the-query-expression-pattern). Tyto metody mohou být instanční metody objektu, na který se dotazuje, nebo metody rozšíření, které jsou pro objekt externí, a implementují skutečné provedení dotazu.

Překlad z výrazů dotazů na vyvolání metody je syntaktické mapování, které se vyskytuje před provedením jakékoli vazby typu nebo vyřešení přetížení. Je zaručeno, že překlad bude syntakticky správný, ale není zaručeno vydávat sémanticky správný C# kód. Po překladu výrazů dotazů jsou výsledná volání metody zpracovávána jako běžná volání metod, což může vést k chybám při zjištění chyb, například pokud tyto metody neexistují, pokud argumenty mají chybné typy nebo pokud jsou metody obecné a odvození typu se nezdařilo.

Výraz dotazu se zpracovává opakovaným použitím následujících překladů, dokud nebudou možná žádná další snížení. Překlady jsou uvedeny v pořadí aplikace: každá část předpokládá, že překlady v předchozích částech byly vyčerpány a po jejím vyčerpání se oddíl nebude později znovu navštěvovat při zpracování stejného výrazu dotazu.

Přiřazení k proměnným rozsahu není ve výrazech dotazů povoleno. Nicméně C# implementace je dovoleno, aby toto omezení vynutilo vždy, protože to může být někdy možné pouze pomocí syntaktického schématu překladu, který je zde zobrazen.

Některé překlady vloží proměnné rozsahu s průhlednými identifikátory, které jsou označeny `*`. Speciální vlastnosti transparentních identifikátorů jsou podrobněji popsány v [transparentní identifikátory](expressions.md#transparent-identifiers).

#### <a name="select-and-groupby-clauses-with-continuations"></a>Klauzule SELECT a GroupBy s pokračováními

Výraz dotazu s pokračováním
```csharp
from ... into x ...
```
je přeloženo do
```csharp
from x in ( from ... ) ...
```

Překlady v následujících částech předpokládají, že dotazy nemají žádné pokračování `into`.

Příklad
```csharp
from c in customers
group c by c.Country into g
select new { Country = g.Key, CustCount = g.Count() }
```
je přeloženo do
```csharp
from g in
    from c in customers
    group c by c.Country
select new { Country = g.Key, CustCount = g.Count() }
```
konečný překlad, který je
```csharp
customers.
GroupBy(c => c.Country).
Select(g => new { Country = g.Key, CustCount = g.Count() })
```

#### <a name="explicit-range-variable-types"></a>Explicitní typy proměnných rozsahu

Klauzule `from`, která explicitně určuje typ proměnné rozsahu
```csharp
from T x in e
```
je přeloženo do
```csharp
from x in ( e ) . Cast < T > ( )
```

Klauzule `join`, která explicitně určuje typ proměnné rozsahu
```csharp
join T x in e on k1 equals k2
```
je přeloženo do
```csharp
join x in ( e ) . Cast < T > ( ) on k1 equals k2
```

Překlady v následujících částech předpokládají, že dotazy nemají žádné explicitní typy proměnných rozsahu.

Příklad
```csharp
from Customer c in customers
where c.City == "London"
select c
```
je přeloženo do
```csharp
from c in customers.Cast<Customer>()
where c.City == "London"
select c
```
konečný překlad, který je
```csharp
customers.
Cast<Customer>().
Where(c => c.City == "London")
```

Explicitní typy proměnných rozsahu jsou užitečné pro dotazování kolekce, které implementují neobecné rozhraní @no__t 0, ale ne obecného rozhraní `IEnumerable<T>`. V předchozím příkladu se jedná o případ, kdy `customers` byly typu `ArrayList`.

#### <a name="degenerate-query-expressions"></a>Degenerovat výrazy dotazů

Výraz dotazu formuláře
```csharp
from x in e select x
```
je přeloženo do
```csharp
( e ) . Select ( x => x )
```

Příklad
```csharp
from c in customers
select c
```
je přeloženo do
```csharp
customers.Select(c => c)
```

Negenerovaný výraz dotazu je ten, který triviální výběr prvků zdroje. Později v pozdější fázi překladu se odstraní negenerované dotazy, které zavádějí jiné kroky překladu, a nahradí je jejich zdrojem. Je však důležité zajistit, aby výsledek výrazu dotazu nebyl nikdy samotný zdrojový objekt, protože by bylo možné odhalit typ a identitu zdroje pro klienta dotazu. Proto tento krok chrání před vygenerováním dotazů napsaných přímo ve zdrojovém kódu explicitním voláním `Select` na zdroj. Následně až po implementátori `Select` a dalších operátorů dotazu, které zajistí, že tyto metody nikdy nevrátí samotný zdrojový objekt.

#### <a name="from-let-where-join-and-orderby-clauses"></a>Klauzule FROM, let, WHERE, Join a OrderBy

Výraz dotazu s druhou klauzulí `from` následovaný klauzulí `select`
```csharp
from x1 in e1
from x2 in e2
select v
```
je přeloženo do
```csharp
( e1 ) . SelectMany( x1 => e2 , ( x1 , x2 ) => v )
```

Výraz dotazu s druhou klauzulí `from` následovaný jinou výjimkou jako klauzule `select`:

```csharp
from x1 in e1
from x2 in e2
...
```
je přeloženo do
```csharp
from * in ( e1 ) . SelectMany( x1 => e2 , ( x1 , x2 ) => new { x1 , x2 } )
...
```

Výraz dotazu s klauzulí `let`
```csharp
from x in e
let y = f
...
```
je přeloženo do
```csharp
from * in ( e ) . Select ( x => new { x , y = f } )
...
```

Výraz dotazu s klauzulí `where`
```csharp
from x in e
where f
...
```
je přeloženo do
```csharp
from x in ( e ) . Where ( x => f )
...
```

Výraz dotazu s klauzulí `join` bez `into` následovaný klauzulí `select`
```csharp
from x1 in e1
join x2 in e2 on k1 equals k2
select v
```
je přeloženo do
```csharp
( e1 ) . Join( e2 , x1 => k1 , x2 => k2 , ( x1 , x2 ) => v )
```

Výraz dotazu s klauzulí `join` bez `into` následovaný jinou klauzulí `select`
```csharp
from x1 in e1
join x2 in e2 on k1 equals k2
...
```
je přeloženo do
```csharp
from * in ( e1 ) . Join( e2 , x1 => k1 , x2 => k2 , ( x1 , x2 ) => new { x1 , x2 })
...
```

Výraz dotazu s klauzulí `join` s `into` následovaný klauzulí `select`
```csharp
from x1 in e1
join x2 in e2 on k1 equals k2 into g
select v
```
je přeloženo do
```csharp
( e1 ) . GroupJoin( e2 , x1 => k1 , x2 => k2 , ( x1 , g ) => v )
```

Výraz dotazu s klauzulí `join` s `into` následovaný jinou výjimkou klauzule `select`
```csharp
from x1 in e1
join x2 in e2 on k1 equals k2 into g
...
```
je přeloženo do
```csharp
from * in ( e1 ) . GroupJoin( e2 , x1 => k1 , x2 => k2 , ( x1 , g ) => new { x1 , g })
...
```

Výraz dotazu s klauzulí `orderby`
```csharp
from x in e
orderby k1 , k2 , ..., kn
...
```
je přeloženo do
```csharp
from x in ( e ) . 
OrderBy ( x => k1 ) . 
ThenBy ( x => k2 ) .
... .
ThenBy ( x => kn )
...
```

Pokud klauzule ORDER určuje směrové indikátory `descending`, místo toho se vytvoří volání `OrderByDescending` nebo `ThenByDescending`.

Následující překlady předpokládají, že nejsou k dispozici žádné klauzule `let`, `where`, `join` nebo `orderby` a v každém výrazu dotazu nejsou větší než jedna počáteční klauzule @no__t 4.

Příklad
```csharp
from c in customers
from o in c.Orders
select new { c.Name, o.OrderID, o.Total }
```
je přeloženo do
```csharp
customers.
SelectMany(c => c.Orders,
     (c,o) => new { c.Name, o.OrderID, o.Total }
)
```

Příklad
```csharp
from c in customers
from o in c.Orders
orderby o.Total descending
select new { c.Name, o.OrderID, o.Total }
```
je přeloženo do
```csharp
from * in customers.
    SelectMany(c => c.Orders, (c,o) => new { c, o })
orderby o.Total descending
select new { c.Name, o.OrderID, o.Total }
```
konečný překlad, který je
```csharp
customers.
SelectMany(c => c.Orders, (c,o) => new { c, o }).
OrderByDescending(x => x.o.Total).
Select(x => new { x.c.Name, x.o.OrderID, x.o.Total })
```
kde `x` je identifikátor generovaný kompilátorem, který je jinak neviditelný a nepřístupný.

Příklad
```csharp
from o in orders
let t = o.Details.Sum(d => d.UnitPrice * d.Quantity)
where t >= 1000
select new { o.OrderID, Total = t }
```
je přeloženo do
```csharp
from * in orders.
    Select(o => new { o, t = o.Details.Sum(d => d.UnitPrice * d.Quantity) })
where t >= 1000 
select new { o.OrderID, Total = t }
```
konečný překlad, který je
```csharp
orders.
Select(o => new { o, t = o.Details.Sum(d => d.UnitPrice * d.Quantity) }).
Where(x => x.t >= 1000).
Select(x => new { x.o.OrderID, Total = x.t })
```
kde `x` je identifikátor generovaný kompilátorem, který je jinak neviditelný a nepřístupný.

Příklad
```csharp
from c in customers
join o in orders on c.CustomerID equals o.CustomerID
select new { c.Name, o.OrderDate, o.Total }
```
je přeloženo do
```csharp
customers.Join(orders, c => c.CustomerID, o => o.CustomerID,
    (c, o) => new { c.Name, o.OrderDate, o.Total })
```

Příklad
```csharp
from c in customers
join o in orders on c.CustomerID equals o.CustomerID into co
let n = co.Count()
where n >= 10
select new { c.Name, OrderCount = n }
```
je přeloženo do
```csharp
from * in customers.
    GroupJoin(orders, c => c.CustomerID, o => o.CustomerID,
        (c, co) => new { c, co })
let n = co.Count()
where n >= 10 
select new { c.Name, OrderCount = n }
```
konečný překlad, který je
```csharp
customers.
GroupJoin(orders, c => c.CustomerID, o => o.CustomerID,
    (c, co) => new { c, co }).
Select(x => new { x, n = x.co.Count() }).
Where(y => y.n >= 10).
Select(y => new { y.x.c.Name, OrderCount = y.n)
```
kde `x` a `y` jsou identifikátory generované kompilátorem, které jsou jinak neviditelné a nedostupné.

Příklad
```csharp
from o in orders
orderby o.Customer.Name, o.Total descending
select o
```
má konečný překlad
```csharp
orders.
OrderBy(o => o.Customer.Name).
ThenByDescending(o => o.Total)
```

#### <a name="select-clauses"></a>Vybrat klauzule

Výraz dotazu formuláře
```csharp
from x in e select v
```
je přeloženo do
```csharp
( e ) . Select ( x => v )
```
s výjimkou případů, kdy v je identifikátor x, je překlad jednoduše
```csharp
( e )
```

Například
```csharp
from c in customers.Where(c => c.City == "London")
select c
```
je jednoduše přeložen na
```csharp
customers.Where(c => c.City == "London")
```

#### <a name="groupby-clauses"></a>Klauzule GroupBy

Výraz dotazu formuláře
```csharp
from x in e group v by k
```
je přeloženo do
```csharp
( e ) . GroupBy ( x => k , x => v )
```
s výjimkou případů, kdy v je identifikátor x, je překlad
```csharp
( e ) . GroupBy ( x => k )
```

Příklad
```csharp
from c in customers
group c.Name by c.Country
```
je přeloženo do
```csharp
customers.
GroupBy(c => c.Country, c => c.Name)
```

#### <a name="transparent-identifiers"></a>Transparentní identifikátory

Některé překlady vloží proměnné rozsahu s ***průhlednými identifikátory*** , které jsou označeny `*`. Transparentní identifikátory nejsou funkcí jazyka správné verze; Existují pouze jako zprostředkující krok v procesu překladu výrazu dotazu.

Když překlad dotazů vloží transparentní identifikátor, další kroky překladu šíří transparentní identifikátor do anonymních funkcí a inicializátorů anonymních objektů. V těchto kontextech mají transparentní identifikátory následující chování:

*  Pokud dojde k transparentnímu identifikátoru jako parametr v anonymní funkci, členové přidruženého anonymního typu jsou automaticky v oboru v těle anonymní funkce.
*  Pokud je člen s transparentním identifikátorem v oboru, členy tohoto člena jsou také v oboru.
*  Když dojde k transparentnímu identifikátoru jako člen deklarátor v inicializátoru anonymního objektu, zavádí člen s transparentním identifikátorem.
*  V krocích překladu výše jsou transparentní identifikátory vždy představeny spolu s anonymními typy s úmyslem zachytávání více proměnných rozsahu jako členů jednoho objektu. Implementace C# je oprávněná používat jiný mechanismus než anonymní typy pro seskupení více proměnných rozsahu. V následujících příkladech překladu se předpokládá, že se používají anonymní typy, a ukazuje, jak se dají překládat transparentní identifikátory.

Příklad
```csharp
from c in customers
from o in c.Orders
orderby o.Total descending
select new { c.Name, o.Total }
```
je přeloženo do
```csharp
from * in customers.
    SelectMany(c => c.Orders, (c,o) => new { c, o })
orderby o.Total descending
select new { c.Name, o.Total }
```

který je dále přeložen do
```csharp
customers.
SelectMany(c => c.Orders, (c,o) => new { c, o }).
OrderByDescending(* => o.Total).
Select(* => new { c.Name, o.Total })
```
který, když jsou transparentní identifikátory smazány, je ekvivalentní
```csharp
customers.
SelectMany(c => c.Orders, (c,o) => new { c, o }).
OrderByDescending(x => x.o.Total).
Select(x => new { x.c.Name, x.o.Total })
```
kde `x` je identifikátor generovaný kompilátorem, který je jinak neviditelný a nepřístupný.

Příklad
```csharp
from c in customers
join o in orders on c.CustomerID equals o.CustomerID
join d in details on o.OrderID equals d.OrderID
join p in products on d.ProductID equals p.ProductID
select new { c.Name, o.OrderDate, p.ProductName }
```
je přeloženo do
```csharp
from * in customers.
    Join(orders, c => c.CustomerID, o => o.CustomerID, 
        (c, o) => new { c, o })
join d in details on o.OrderID equals d.OrderID
join p in products on d.ProductID equals p.ProductID
select new { c.Name, o.OrderDate, p.ProductName }
```
což je dále sníženo na
```csharp
customers.
Join(orders, c => c.CustomerID, o => o.CustomerID, (c, o) => new { c, o }).
Join(details, * => o.OrderID, d => d.OrderID, (*, d) => new { *, d }).
Join(products, * => d.ProductID, p => p.ProductID, (*, p) => new { *, p }).
Select(* => new { c.Name, o.OrderDate, p.ProductName })
```
konečný překlad, který je
```csharp
customers.
Join(orders, c => c.CustomerID, o => o.CustomerID,
    (c, o) => new { c, o }).
Join(details, x => x.o.OrderID, d => d.OrderID,
    (x, d) => new { x, d }).
Join(products, y => y.d.ProductID, p => p.ProductID,
    (y, p) => new { y, p }).
Select(z => new { z.y.x.c.Name, z.y.x.o.OrderDate, z.p.ProductName })
```
kde `x`, `y` a `z` jsou identifikátory generované kompilátorem, které jsou jinak skryté a nedostupné.

### <a name="the-query-expression-pattern"></a>Vzor výrazu dotazu

***Vzor výrazu dotazu*** vytváří vzor metod, které mohou typy implementovat pro podporu výrazů dotazů. Vzhledem k tomu, že výrazy dotazů jsou přeloženy do vyvolání metod pomocí syntaktického mapování, typy mají značnou flexibilitu v tom, jak implementují vzor výrazu dotazu. Například metody vzoru lze implementovat jako metody instance nebo jako metody rozšíření, protože obě mají stejnou syntaxi vyvolání a metody mohou požadovat delegáty nebo stromy výrazů, protože anonymní funkce jsou převoditelné na obojí.

Doporučený tvar obecného typu `C<T>`, který podporuje vzor výrazu dotazu, je uveden níže. Obecný typ se používá pro ilustraci správných vztahů mezi typy parametrů a výsledků, ale je možné implementovat také vzor pro neobecné typy.

```csharp
delegate R Func<T1,R>(T1 arg1);

delegate R Func<T1,T2,R>(T1 arg1, T2 arg2);

class C
{
    public C<T> Cast<T>();
}

class C<T> : C
{
    public C<T> Where(Func<T,bool> predicate);

    public C<U> Select<U>(Func<T,U> selector);

    public C<V> SelectMany<U,V>(Func<T,C<U>> selector,
        Func<T,U,V> resultSelector);

    public C<V> Join<U,K,V>(C<U> inner, Func<T,K> outerKeySelector,
        Func<U,K> innerKeySelector, Func<T,U,V> resultSelector);

    public C<V> GroupJoin<U,K,V>(C<U> inner, Func<T,K> outerKeySelector,
        Func<U,K> innerKeySelector, Func<T,C<U>,V> resultSelector);

    public O<T> OrderBy<K>(Func<T,K> keySelector);

    public O<T> OrderByDescending<K>(Func<T,K> keySelector);

    public C<G<K,T>> GroupBy<K>(Func<T,K> keySelector);

    public C<G<K,E>> GroupBy<K,E>(Func<T,K> keySelector,
        Func<T,E> elementSelector);
}

class O<T> : C<T>
{
    public O<T> ThenBy<K>(Func<T,K> keySelector);

    public O<T> ThenByDescending<K>(Func<T,K> keySelector);
}

class G<K,T> : C<T>
{
    public K Key { get; }
}
```

Výše uvedené metody používají obecné typy delegátů `Func<T1,R>` a `Func<T1,T2,R>`, ale mohly by stejně dobře používat jiné typy delegátů nebo strom výrazů se stejnými relacemi v parametrech a typech výsledků.

Všimněte si doporučené relace mezi `C<T>` a `O<T>`, která zajišťuje, že metody `ThenBy` a `ThenByDescending` jsou k dispozici pouze pro výsledek `OrderBy` nebo `OrderByDescending`. Všimněte si také doporučeného tvaru výsledku `GroupBy`--sekvence sekvencí, kde má každá vnitřní sekvence další vlastnost `Key`.

Obor názvů `System.Linq` poskytuje implementaci vzoru operátoru dotazu pro jakýkoli typ, který implementuje rozhraní `System.Collections.Generic.IEnumerable<T>`.

## <a name="assignment-operators"></a>Operátory přiřazení

Operátory přiřazení přiřadí novou hodnotu proměnné, vlastnosti, události nebo prvku indexeru.

```antlr
assignment
    : unary_expression assignment_operator expression
    ;

assignment_operator
    : '='
    | '+='
    | '-='
    | '*='
    | '/='
    | '%='
    | '&='
    | '|='
    | '^='
    | '<<='
    | right_shift_assignment
    ;
```

Levý operand přiřazení musí být výraz klasifikovaný jako proměnná, přístup k vlastnosti, přístup indexeru nebo přístup k události.

Operátor `=` se nazývá ***jednoduchý operátor přiřazení***. Přiřadí hodnotu pravého operandu proměnné, vlastnosti nebo prvku indexeru, který je dán levým operandem. Levý operand operátoru jednoduchého přiřazení nemůže být přístupem k události (s výjimkou popsaných v [událostech podobných poli](classes.md#field-like-events)). Jednoduchý operátor přiřazení je popsán v tématu [jednoduché přiřazení](expressions.md#simple-assignment).

Operátory přiřazení jiné než operátor `=` se nazývají ***operátory složeného přiřazení***. Tyto operátory provádějí určenou operaci na dvou operandech a následně přiřadí výslednou hodnotu proměnné, vlastnosti nebo prvku indexeru, který je dán levým operandem. Složené operátory přiřazení jsou popsány ve [složeném přiřazení](expressions.md#compound-assignment).

Operátory `+=` a `-=` s výrazem přístupu k události jako levý operand se nazývají *operátory přiřazení události*. Žádný jiný operátor přiřazení není platný s přístupem k události jako levý operand. Operátory přiřazení událostí jsou popsány v tématu [přiřazení události](expressions.md#event-assignment).

Operátory přiřazení jsou asociativní zprava, což znamená, že operace jsou seskupeny zprava doleva. Například výraz formuláře `a = b = c` je vyhodnocen jako `a = (b = c)`.

### <a name="simple-assignment"></a>Jednoduché přiřazení

Operátor `=` se nazývá jednoduchý operátor přiřazení.

Je-li levý operand jednoduchého přiřazení ve formátu `E.P` nebo `E[Ei]`, kde `E` je typ doby kompilace `dynamic`, pak je přiřazení dynamicky svázáno ([dynamická vazba](expressions.md#dynamic-binding)). V tomto případě je typ doby kompilace výrazu přiřazení `dynamic` a řešení popsané níže bude provedeno za běhu na základě typu modulu runtime `E`.

V jednoduchém přiřazení musí být pravý operand výraz, který lze implicitně převést na typ levého operandu. Operace přiřadí hodnotu pravého operandu proměnné, vlastnosti nebo prvku indexeru, který je dán levým operandem.

Výsledkem jednoduchého výrazu přiřazení je hodnota přiřazená k levému operandu. Výsledek má stejný typ jako levý operand a je vždycky klasifikován jako hodnota.

Pokud levý operand je vlastnost nebo přístup indexeru, vlastnost nebo indexer musí mít přistupující objekt `set`. Pokud se nejedná o tento případ, dojde k chybě při vazbě.

Běhové zpracování jednoduchého přiřazení formuláře `x = y` se skládá z následujících kroků:

*  Je-li hodnota `x` klasifikována jako proměnná:
   * `x` se vyhodnotí, aby se vytvořila proměnná.
   * hodnota `y` je vyhodnocena a v případě potřeby převedena na typ `x` prostřednictvím implicitního převodu ([implicitní převody](conversions.md#implicit-conversions)).
   * Pokud proměnná zadaná pomocí `x` je prvkem pole *reference_type*, je provedena kontrola za běhu, aby se zajistilo, že hodnota vypočítaná pro `y` je kompatibilní s instancí pole, pro kterou je prvek `x` prvkem. Pokud je `y` `null` nebo pokud implicitní převod odkazu ([implicitní převody odkazů](conversions.md#implicit-reference-conversions)) existuje ze skutečného typu instance, na kterou odkazuje `y`, na skutečný typ prvku instance pole, která obsahuje `x`, je ověření úspěšné. V opačném případě je vyvolána `System.ArrayTypeMismatchException`.
   * Hodnota, která je výsledkem vyhodnocení a konverze `y`, je uložena do umístění uvedeného vyhodnocením `x`.
*  Pokud je `x` klasifikován jako vlastnost nebo přístup indexeru:
   * Výraz instance (Pokud `x` není `static`) a seznam argumentů (Pokud `x` je indexerový přístup) přidružený k `x` jsou vyhodnocovány a výsledky se používají v následných voláních přístupových práv @no__t 4.
   * hodnota `y` je vyhodnocena a v případě potřeby převedena na typ `x` prostřednictvím implicitního převodu ([implicitní převody](conversions.md#implicit-conversions)).
   * Přistupující objekt `set` `x` je vyvolán s hodnotou vypočítanou pro `y` jako svůj argument `value`.

Pravidla koodchylky pole ([kovariance pole](arrays.md#array-covariance)) povolují hodnotu typu pole `A[]`, aby byla odkazem na instanci typu pole `B[]`, za předpokladu, že se implicitní převod odkazu vyskytuje z `B` na `A`. Z důvodu těchto pravidel přiřazení k elementu pole *reference_type* vyžaduje kontrolu za běhu, aby bylo zajištěno, že přiřazená hodnota je kompatibilní s instancí Array. V příkladu
```csharp
string[] sa = new string[10];
object[] oa = sa;

oa[0] = null;               // Ok
oa[1] = "Hello";            // Ok
oa[2] = new ArrayList();    // ArrayTypeMismatchException
```
Poslední přiřazení způsobí vyvyvolání `System.ArrayTypeMismatchException`, protože instance `ArrayList` nemůže být uložena v prvku `string[]`.

Je-li vlastnost nebo indexovací člen deklarovaný v *struct_type* cílem přiřazení, musí být výraz instance přidružený k vlastnosti nebo přístupu indexeru klasifikován jako proměnná. Pokud je výraz instance klasifikován jako hodnota, dojde k chybě při vazbě. Vzhledem k tomu, že je [členem](expressions.md#member-access), platí stejné pravidlo i pro pole.

S ohledem na deklarace:
```csharp
struct Point
{
    int x, y;

    public Point(int x, int y) {
        this.x = x;
        this.y = y;
    }

    public int X {
        get { return x; }
        set { x = value; }
    }

    public int Y {
        get { return y; }
        set { y = value; }
    }
}

struct Rectangle
{
    Point a, b;

    public Rectangle(Point a, Point b) {
        this.a = a;
        this.b = b;
    }

    public Point A {
        get { return a; }
        set { a = value; }
    }

    public Point B {
        get { return b; }
        set { b = value; }
    }
}
```
V příkladu
```csharp
Point p = new Point();
p.X = 100;
p.Y = 100;
Rectangle r = new Rectangle();
r.A = new Point(10, 10);
r.B = p;
```
přiřazení `p.X`, `p.Y`, `r.A` a `r.B` jsou povolena, protože `p` a `r` jsou proměnné. V tomto příkladu se ale
```csharp
Rectangle r = new Rectangle();
r.A.X = 10;
r.A.Y = 10;
r.B.X = 100;
r.B.Y = 100;
```
přiřazení jsou neplatná, protože `r.A` a `r.B` nejsou proměnné.

### <a name="compound-assignment"></a>Složené přiřazení

Je-li levý operand složeného přiřazení ve formátu `E.P` nebo `E[Ei]`, kde `E` je typ doby kompilace `dynamic`, pak je přiřazení dynamicky svázáno ([dynamická vazba](expressions.md#dynamic-binding)). V tomto případě je typ doby kompilace výrazu přiřazení `dynamic` a řešení popsané níže bude provedeno za běhu na základě typu modulu runtime `E`.

Operace formuláře `x op= y` se zpracovává pomocí řešení přetížení binárního operátoru ([rozlišení přetížení binárního operátoru](expressions.md#binary-operator-overload-resolution)), jako kdyby byla operace zapsána `x op y`. Stisknutím

*  Pokud je návratový typ vybraného operátoru implicitně převoditelný na typ `x`, operace se vyhodnotí jako `x = x op y` s tím rozdílem, že `x` se vyhodnotí jenom jednou.
*  V opačném případě, pokud je vybraný operátor předdefinovaným operátorem, je-li návratový typ vybraného operátoru explicitně převeden na typ `x` a pokud `y` je implicitně převeden na typ `x` nebo je operátor Shift , operace je vyhodnocena jako `x = (T)(x op y)`, kde `T` je typ `x` s tím rozdílem, že `x` je vyhodnocena pouze jednou.
*  V opačném případě je složené přiřazení neplatné a dojde k chybě při vazbě.

Termín "vyhodnocený pouze jednou" znamená, že při vyhodnocování `x op y` jsou výsledky jakýchkoliv výrazů `x` dočasně uloženy a následně znovu použity při provádění přiřazení do `x`. Například v přiřazení `A()[B()] += C()`, kde `A` je metoda, která vrací `int[]` a `B` a `C` jsou metody vracející `int`, metody jsou vyvolány pouze jednou, v pořadí `A`, `B`, `C`.

Když je levý operand složeného přiřazení přístup k vlastnosti nebo indexer, vlastnost nebo indexer musí mít přístupový objekt `get` a přístupový objekt `set`. Pokud se nejedná o tento případ, dojde k chybě při vazbě.

Druhé pravidlo výše povoluje `x op= y` vyhodnotit jako `x = (T)(x op y)` v určitých kontextech. Toto pravidlo existuje, protože předdefinované operátory lze použít jako složené operátory, pokud je levý operand typu `sbyte`, `byte`, `short`, `ushort` nebo `char`. I v případě, že oba argumenty mají jeden z těchto typů, předdefinované operátory vytvoří výsledek typu `int`, jak je popsáno v tématu [binární číselné propagační akce](expressions.md#binary-numeric-promotions). Bez přetypování proto by nebylo možné přiřadit výsledek k levému operandu.

Intuitivní efekt pravidla pro předdefinované operátory je jednoduše tím, že `x op= y` je povolen, pokud jsou povoleny obě `x op y` a `x = y`. V příkladu
```csharp
byte b = 0;
char ch = '\0';
int i = 0;

b += 1;             // Ok
b += 1000;          // Error, b = 1000 not permitted
b += i;             // Error, b = i not permitted
b += (byte)i;       // Ok

ch += 1;            // Error, ch = 1 not permitted
ch += (char)1;      // Ok
```
intuitivním důvodem pro každou chybu je to, že došlo k chybě odpovídající jednoduchého přiřazení.

To také znamená, že operace složeného přiřazení podporují přenesené operace. V příkladu
```csharp
int? i = 0;
i += 1;             // Ok
```
je použit předaný operátor `+(int?,int?)`.

### <a name="event-assignment"></a>Přiřazení události

Pokud je levý operand operátoru `+=` nebo `-=` klasifikován jako přístup k události, je výraz vyhodnocen takto:

*  Výraz instance, pokud existuje, je vyhodnocen.
*  Pravý operand operátoru `+=` nebo `-=` je vyhodnocen a v případě potřeby převeden na typ levého operandu pomocí implicitního převodu ([implicitní převody](conversions.md#implicit-conversions)).
*  Přístup k události události je vyvolán s seznamem argumentů, který se skládá z pravého operandu po vyhodnocení a v případě potřeby konverze. Pokud byl operátor `+=`, je vyvolán přistupující objekt `add`. Pokud byl operátor `-=`, je vyvolán přistupující objekt `remove`.

Výraz přiřazení události nepřinese hodnotu. Proto je výraz přiřazení události platný pouze v kontextu *statement_expression* ([příkazy výrazu](statements.md#expression-statements)).

## <a name="expression"></a>Výraz

*Výraz* je buď *non_assignment_expression* , nebo *přiřazení*.

```antlr
expression
    : non_assignment_expression
    | assignment
    ;

non_assignment_expression
    : conditional_expression
    | lambda_expression
    | query_expression
    ;
```

## <a name="constant-expressions"></a>Výrazy konstant

*Constant_expression* je výraz, který lze plně vyhodnotit v době kompilace.

```antlr
constant_expression
    : expression
    ;
```

Konstantní výraz musí být literál `null` nebo hodnota s jedním z následujících typů: `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, @no__t-@no__t `ulong`, `char`, 0, 1, 2, 3,-14 , 5 nebo jakýkoli typ výčtu. V konstantních výrazech jsou povoleny pouze následující konstrukce:

*  Literály (včetně literálu `null`).
*  Odkazy na `const` členů typů třídy a struktury.
*  Odkazy na členy výčtových typů.
*  Odkazy na parametry `const` nebo místní proměnné
*  Dílčí výrazy v závorkách, které jsou samotnými konstantními výrazy.
*  Výrazy přetypování, za předpokladu, že cílový typ je jeden z výše uvedených typů.
*  @no__t – 0 a `unchecked` výrazy
*  výrazy výchozích hodnot
*  Výrazy nameof
*  Předdefinovaná `+`, `-`, `!` a unární operátory `~`.
*  Předdefinovaná `+`, `-`, `*`, `/`, `%`, `<<`, `>>`, `&`, `|`, `^`, 0, 1, 2, 3, 4, 5, 6 a 7 Binary, za předpokladu, že každý operand je typ uvedený výše.
*  Podmíněný operátor `?:`

V konstantních výrazech jsou povoleny následující převody:

*  Převody identity
*  Číselné převody
*  Převody výčtu
*  Převody konstantních výrazů
*  Implicitní a explicitní převody odkazů za předpokladu, že zdrojem převodů je konstantní výraz, který se vyhodnocuje na hodnotu null.

Další převody, včetně zabalení, rozbalení a implicitních převodů odkazů na hodnoty, které nejsou null, nejsou povoleny v konstantních výrazech. Příklad:
```csharp
class C {
    const object i = 5;         // error: boxing conversion not permitted
    const object str = "hello"; // error: implicit reference conversion
}
```
Inicializace i je chyba, protože je vyžadován převod zabalení. Inicializace str je chyba, protože je vyžadován implicitní převod odkazu z hodnoty, která není null.

Pokaždé, když výraz splní požadavky uvedené výše, je výraz vyhodnocen při kompilaci. To platí i v případě, že výraz je dílčí výraz většího výrazu, který obsahuje nekonstantní konstrukce.

Vyhodnocení konstantních výrazů v době kompilace používá stejná pravidla jako vyhodnocení za běhu nekonstantních výrazů, s výjimkou toho, že při vyhodnocení za běhu by došlo k chybě při kompilaci.

Pokud není konstantní výraz explicitně umístěn v kontextu `unchecked`, přetečení, ke kterým dochází v aritmetických operacích typu integrál a převody během kompilace výrazu, vždy způsobují chyby při kompilaci ([konstanta výrazy](expressions.md#constant-expressions)).

Konstantní výrazy se vyskytují v kontextech uvedených níže. V těchto kontextech dojde k chybě při kompilaci, pokud výraz nemůže být plně vyhodnocen v době kompilace.

*  Konstantní deklarace ([konstanty](classes.md#constants)).
*  Deklarace členů výčtu ([členy výčtu](enums.md#enum-members)).
*  Výchozí argumenty formálních seznamů parametrů ([parametry metody](classes.md#method-parameters))
*  popisky `case` příkazu `switch` ([příkaz switch](statements.md#the-switch-statement))
*  příkazy `goto case` ([příkaz goto](statements.md#the-goto-statement)).
*  Délky dimenzí ve výrazu vytváření pole ([výrazy vytváření pole](expressions.md#array-creation-expressions)), které obsahují inicializátor.
*  Atributy ([atributy](attributes.md)).

Implicitní převod konstantních výrazů ([implicitní převody konstantních výrazů](conversions.md#implicit-constant-expression-conversions)) povoluje převod konstantního výrazu typu `int` na `sbyte`, `byte`, `short`, `ushort`, `uint` nebo `ulong` za předpokladu, že hodnota konstantní výraz je v rozsahu cílového typu.

## <a name="boolean-expressions"></a>logické výrazy

*Boolean_expression* je výraz, který vrací výsledek typu `bool`; buď přímo, nebo prostřednictvím aplikace `operator true` v některých kontextech, jak je uvedeno v následujícím seznamu.

```antlr
boolean_expression
    : expression
    ;
```

Řízení podmíněného výrazu pro *if_statement* ([příkaz if](statements.md#the-if-statement)), *while_statement* ([příkaz While](statements.md#the-while-statement)), *do_statement* ([příkaz](statements.md#the-do-statement)do) nebo *for_statement* ([pro příkaz](statements.md#the-for-statement)) je *Boolean_expression*. Řízení podmíněného výrazu operátoru `?:` ([podmíněný operátor](expressions.md#conditional-operator)) se řídí stejnými pravidly jako *Boolean_expression*, ale z důvodů priority operátoru je klasifikován jako *conditional_or_expression*.

*Boolean_expression* `E` je nutné, aby bylo možné vytvořit hodnotu typu `bool` následujícím způsobem:

*  Pokud je `E` implicitně převést na `bool`, pak za běhu, že se používá implicitní převod.
*  V opačném případě se k nalezení jedinečné nejlepší implementace operátoru `true` v `E` používá rozlišení přetížení unárního operátoru ([řešení přetížení unárního operátoru](expressions.md#unary-operator-overload-resolution)) a tato implementace se aplikuje za běhu.
*  Pokud žádný takový operátor nenajde, dojde k chybě při vazbě.

Typ struktury `DBBool` v typu [logická hodnota databáze](structs.md#database-boolean-type) poskytuje příklad typu, který implementuje `operator true` a `operator false`.
