---
ms.openlocfilehash: 130898a8b5a7b8eb986b314cb4cf78038e840b02
ms.sourcegitcommit: 09e0ddec3bb6aa99b7340158bbac86a5a8243b43
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/24/2019
ms.locfileid: "66193932"
---
# <a name="expressions"></a>Výrazy

Výraz je posloupnost operátorů a operandů. Tato kapitola definuje syntaxe pořadí vyhodnocení operandů a operátory a výrazy význam.

## <a name="expression-classifications"></a>Klasifikacích výraz

Výraz je klasifikován tak jeden z následujících akcí:

*  Hodnota. Každá hodnota má přidruženého typu.
*  Proměnná. Každá proměnná je přidružen typ, jmenovitě deklarovaný typ proměnné.
*  Obor názvů. Výraz s této klasifikace může být použit pouze jako levé straně *member_access* ([přístup ke členu](expressions.md#member-access)). V libovolném kontextu výrazu jsou klasifikovány jako obor názvů způsobí chybu kompilace.
*  Typ. Výraz s této klasifikace může být použit pouze jako levé straně *member_access* ([přístup ke členu](expressions.md#member-access)), nebo jako operand pro `as` – operátor ([operátor as ](expressions.md#the-as-operator)), `is` – operátor ([je operátor](expressions.md#the-is-operator)), nebo `typeof` – operátor ([operátor typeof](expressions.md#the-typeof-operator)). V libovolném kontextu jsou klasifikovány jako typ výrazu způsobí chybu kompilace.
*  Metoda skupinu, která je sada přetížené metody, výsledkem vyhledávání člena ([člen vyhledávání](expressions.md#member-lookup)). Skupinu metod. může mít přidruženou instanci výraz a seznam argumentů přidruženého typu. Při vyvolání metody instance, bude výsledkem vyhodnocení výrazu instance instance reprezentována `this` ([tento přístup](expressions.md#this-access)). Skupinu metod. je povolen v *invocation_expression* ([výrazy typu Invocation](expressions.md#invocation-expressions)), *delegate_creation_expression* ([delegovat vytváření výrazy](expressions.md#delegate-creation-expressions)) a jako levé straně je operátor a může být implicitně převeden na typ delegáta kompatibilní ([Metoda skupiny převody](conversions.md#method-group-conversions)). V libovolném kontextu výrazu jsou klasifikovány jako skupinu metod způsobí chybu kompilace.
*  Literál null. Výraz s této klasifikace lze implicitně převést na typ odkazu nebo typ připouštějící hodnotu Null.
*  Anonymní funkce. Výraz s této klasifikace lze implicitně převést na typ kompatibilní delegáta nebo typu stromu výrazu.
*  Přístup k vlastnosti. Každý přístup k vlastnosti je přidružen typ, jmenovitě typ vlastnosti. Přístup k vlastnosti kromě toho může mít přidruženou instanci výrazu. Když přístupový objekt ( `get` nebo `set` bloku) instance je vyvolán přístup k vlastnostem, výsledek vyhodnocení výrazu instance se změní instance reprezentována `this` ([tento přístup](expressions.md#this-access)).
*  Přístup k události. Každý přístup k události je přidružen typ, jmenovitě typ události. Kromě toho události přístupu může mít přidruženou instanci výrazu. O událost přístupu se může zobrazit jako operand levé straně `+=` a `-=` operátory ([události přiřazení](expressions.md#event-assignment)). V libovolném kontextu výrazu jsou klasifikovány jako událost přístupu způsobí chybu kompilace.
*  Přístup indexeru. Každý přístup indexeru je přidružen typ, a to typ elementu indexeru. Kromě toho přístup indexeru má přidruženou instanci výraz a seznam argumentů přidružený. Když přístupový objekt ( `get` nebo `set` bloku) z indexeru přístup, je vyvolána, výsledek vyhodnocení výrazu instance se změní instance reprezentována `this` ([tento přístup](expressions.md#this-access)) a výsledek tohoto objektu hodnocení seznamu argumentů se změní na seznamu parametrů vyvolání.
*  Nothing. Proběhne, když je výraz vyvolání metody s návratovým typem `void`. Výraz klasifikován jako nic je platná pouze v kontextu *statement_expression* ([příkazy výrazů](statements.md#expression-statements)).

Konečný výsledek výrazu se nikdy obor názvů, typ, metodu skupiny nebo přístup k události. Místo toho jak bylo uvedeno výše, tyto výrazy jsou zprostředkující konstrukce, které jsou povolené jenom v určitých kontextech.

Přístup k vlastnosti nebo přístup indexeru je vždy překlasifikován sám jako hodnotu pomocí provádí vyvolání *načtení přístupového objektu* nebo *nastavení přístupového objektu*. Zejména přístupový objekt je určen podle kontextu vlastnost nebo indexovací člen přístupu: Pokud je cílem přiřazení, přístup *nastavení přístupového objektu* se vyvolá, aby přiřadí novou hodnotu ([jednoduché přiřazení](expressions.md#simple-assignment)). V opačném případě *načtení přístupového objektu* se vyvolá k získání aktuální hodnoty ([hodnot výrazů](expressions.md#values-of-expressions)).

### <a name="values-of-expressions"></a>Hodnoty výrazů

Většina konstrukce, které se týkají výraz takže v konečném důsledku vyžaduje výraz, který má označení ***hodnota***. V takových případech Pokud skutečný výraz označuje obor názvů, typ, skupinu metod nebo nic, dojde k chybě kompilace. Ale pokud výraz označuje přístup k vlastnosti, indexeru přístupu nebo proměnná, hodnota pro vlastnost, indexer nebo proměnná je implicitně nahrazena:

*  Hodnota proměnné je jednoduše hodnota aktuálně uložené v umístění úložiště, který je identifikován proměnné. Proměnná je třeba zvážit, jsou přiřazené ([jednoznačného přiřazení](variables.md#definite-assignment)) před jeho hodnotu lze získat nebo jinak dojde k chybě kompilace.
*  Získá hodnota výraz přístupu k vlastnosti s vyvoláním *načtení přístupového objektu* vlastnosti. Pokud vlastnost nemá žádný *načtení přístupového objektu*, dojde k chybě kompilace. V opačném případě vyvolání členské funkce ([kompilace kontrolu dynamické přetížení](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) se provádí, a výsledek volání se stane hodnotou výrazu přístup k vlastnosti.
*  Hodnota výrazu přístup indexeru je získala při vyvolání *načtení přístupového objektu* indexeru. Pokud nemá žádné indexeru *načtení přístupového objektu*, dojde k chybě kompilace. V opačném případě vyvolání členské funkce ([kompilace kontrolu dynamické přetížení](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) se provádí v argumentu seznamu přidružená k výrazu přístup indexeru a výsledek volání se stane hodnota výrazu přístup indexeru.

## <a name="static-and-dynamic-binding"></a>Statické a dynamické vazby

Procesu, který určuje význam operace na základě typu nebo hodnota základních výrazů (argumenty, operandy, příjemci) se často označuje jako ***vazby***. Pro instanci význam volání metody závisí na typu příjemce a argumenty. Význam operátoru je určen na základě typu operandů.

V jazyce C# významu operace je obvykle určena v době kompilace, založená na kompilaci typu z jeho základních výrazů. Podobně pokud výraz obsahuje chybu, chyba rozpoznáno a Ohlášeno kompilátorem. Tento postup se označuje jako ***statické vazby***.

Ale pokud je výraz dynamického výrazu (tj. nemá typ `dynamic`) to znamená, že všechny vazby, který je součástí by měla vycházet z jeho typ za běhu (tj. skutečný typ objektu je označuje za běhu) místo typu má v za kompilace. Vazba tato operace je proto odložena až do doby, kde je operace má být proveden za běhu programu. To se označuje jako ***dynamické vazby***.

Když se operace dynamicky vazba, žádné nebo téměř žádné kontrola se neprovádí kompilátorem. Místo toho pokud vazbu za běhu selže, jsou hlášeny chyby jako výjimky za běhu.

Následující operace v jazyce C# jsou v souladu s vazby:

*  Přístup ke členu: `e.M`
*  Volání metody: `e.M(e1, ..., eN)`
*  Volání delegáta:`e(e1, ..., eN)`
*  Přístup k prvkům: `e[e1, ..., eN]`
*  Vytvoření objektu: `new C(e1, ..., eN)`
*  Přetížení unárních operátorů: `+`, `-`, `!`, `~`, `++`, `--`, `true`, `false`
*  Přetížené operátory binární: `+`, `-`, `*`, `/`, `%`, `&`, `&&`, `|`, `||`, `??`, `^`, `<<` , `>>`, `==`,`!=`, `>`, `<`, `>=`, `<=`
*  Operátory přiřazení: `=`, `+=`, `-=`, `*=`, `/=`, `%=`, `&=`, `|=`, `^=`, `<<=`, `>>=`
*  Implicitní a explicitní převody

Pokud se jedná o žádné dynamické výrazy jazyka C# výchozí statické vazby, což znamená, že typy kompilace základních výrazů se používají v procesu výběru. Ale po dynamického výrazu jedné z nich základních výrazů v operacích výše uvedené operace místo dynamicky vázán.

### <a name="binding-time"></a>Doba vazby

Statická vazba probíhá v době kompilace, zatímco dynamická vazba se provádí v době běhu. V následujících částech se termín ***doba vazby*** odkazuje na za kompilace nebo za běhu, v závislosti na tom, kdy vazba probíhá.

Následující příklad ukazuje názory statické a dynamické vazby a doby vazby:
```csharp
object  o = 5;
dynamic d = 5;

Console.WriteLine(5);  // static  binding to Console.WriteLine(int)
Console.WriteLine(o);  // static  binding to Console.WriteLine(object)
Console.WriteLine(d);  // dynamic binding to Console.WriteLine(int)
```

První dvě volání jsou staticky vázány: přetížení `Console.WriteLine` se vybere na základě typu kompilace svého argumentu. Doba vazby tedy za kompilace.

Třetí volání dynamicky vázán: přetížení `Console.WriteLine` se vybere na základě typu za běhu svého argumentu. Důvodem je argument dynamického výrazu – jeho typ za kompilace je `dynamic`. Doba vazby pro třetí volání je tedy za běhu.

### <a name="dynamic-binding"></a>Dynamické vazby

Účelem dynamické vazby je umožňuje programům C# pro interakci s ***dynamických objektů***, například systém typů objektů, které se neřídí běžných pravidel jazyka C#. Dynamické objekty mohou být objekty v jiných programovacích jazycích systémech různých typů nebo mohou být objekty, které jsou nastavené pro implementaci sémantiky vlastní vazby pro různé operace prostřednictvím kódu programu.

Mechanismus, pomocí kterého dynamický objekt implementuje vlastní sémantiku je definován implementací. V příslušném rozhraní – znovu definován implementací – je implementováno dynamických objektů na signál pro C# doba běhu, ke kterým mají zvláštní sémantiku. Díky tomu se pokaždé, když se operace na dynamický objekt se dynamicky vázán, sémantika vlastní vazby, ne ty jazyka C# uvedené v tomto dokumentu, převzít kontrolu nad.

Účely dynamické vazby je chcete povolit vzájemné spolupráce s dynamickými objekty, C# umožňuje dynamické vazby u všech objektů, ať už jsou dynamické nebo ne. To umožňuje zajistit plynulejší integraci dynamických objektů, jako výsledky operací s nimi samy o sobě může být dynamické objekty, ale jsou stále typu Neznámý na programátorovi v době kompilace. Dynamické vazby také může pomoct eliminovat náchylné založenými na reflexi kódu i v případě, že nejsou žádné objekty, které jsou zahrnuté dynamických objektů.

Následující části popisují pro každý konstrukce v jazyce přesně při dynamické vazby se používá, co kompilaci kontrolu za – Pokud se použije některý – a jaké kompilaci výrazu and výsledek klasifikace je.

### <a name="types-of-constituent-expressions"></a>Typy základních výrazů

Při operaci je staticky vázán, typ výrazu základní (např. příjemce, argument, index nebo operand) je vždy považuje za kompilace typ tohoto výrazu.

Když se operace dynamicky vazba, typ základní výrazu je určen různými způsoby v závislosti na kompilaci typu základní výraz:

*  Základní výraz typu kompilace `dynamic` je považován za mít typ skutečnou hodnotu, která se výraz vyhodnotí za běhu
*  Základní výraz, jehož typ za kompilace je parametr typu má mít typ, který typ parametru je vázán na za běhu
*  Jinak základní výraz je považován za mít jeho typ za kompilace.

## <a name="operators"></a>Operátory

Výrazy se vytvářejí na základě ***operandy*** a ***operátory***. Operátory výrazu označují operace, které chcete použít pro operandy. Příklady operátorů `+`, `-`, `*`, `/`, a `new`. Příklady operandy: literály, pole, místní proměnné a výrazy.

Existují tři typy operátorů:

*  Unární operátory. Unární operátory používají jeden operand a použijte buď předpony (jako `--x`) nebo přípony zápis (například `x++`).
*  Binární operátory. Binární operátory přebírají dva operandy a všechny používají infixová notace (například `x + y`).
*  Ternární operátor. Pouze jeden Ternární operátor `?:`, existuje; má tři operandy a používá infix zápis (`c ? x : y`).

Pořadí vyhodnocení operátorů ve výrazu má vliv ***prioritu*** a ***asociativita*** operátorů ([Priorita a asociativita operátora](expressions.md#operator-precedence-and-associativity)) .

Operandy ve výrazu jsou vyhodnoceny zleva doprava. Například v `F(i) + G(i++) * H(i)`, metoda `F` jsou volány pomocí původní hodnota `i`, then – metoda `G` volána s původní hodnota `i`a nakonec – metoda `H` je volána s novou hodnotou `i`. Toto je oddělené od a nesouvisejících priorita operátorů.

Některé operátory lze ***přetížené***. Přetížení operátoru umožňuje uživatelem definovaný operátor implementace rozhraní u operací zadat, pokud jeden nebo oba operandy jsou uživatelem definované třídě nebo struktuře typu ([přetížení operátoru](expressions.md#operator-overloading)).

### <a name="operator-precedence-and-associativity"></a>Priorita a asociativita operátora

Pokud výraz obsahuje více operátorů ***prioritu*** operátorů určuje pořadí, ve kterém jsou jednotlivé operátory vyhodnocovány. Například výraz `x + y * z` se vyhodnotí jako `x + (y * z)` vzhledem k tomu, `*` operátor má vyšší prioritu než binární soubor `+` – operátor. Přednost operátoru pokládáme stav, definicí jeho přidružené gramatiky výroby. Například *additive_expression* se skládá z posloupnost *multiplicative_expression*s oddělené `+` nebo `-` operátory, což `+` a `-` nižší prioritu než operátory `*`, `/`, a `%` operátory.

Následující tabulka shrnuje všechny operátory v pořadí dle priority od nejvyšší po nejnižší:

| __Oddíl__                                                                                   | __Kategorie__                | __Operátory__ | 
|-----------------------------------------------------------------------------------------------|-----------------------------|---------------|
| [Primární výrazy](expressions.md#primary-expressions)                                     | Primární                     | `x.y`  `f(x)`  `a[x]`  `x++`  `x--`  `new`  `typeof`  `default`  `checked`  `unchecked`  `delegate` | 
| [Unární operátory](expressions.md#unary-operators)                                             | Unární                       | `+`  `-`  `!`  `~`  `++x`  `--x`  `(T)x` | 
| [Aritmetické operátory](expressions.md#arithmetic-operators)                                   | Násobení              | `*`  `/`  `%` | 
| [Aritmetické operátory](expressions.md#arithmetic-operators)                                   | Additive                    | `+`  `-`      | 
| [Operátory posunutí](expressions.md#shift-operators)                                             | SHIFT                       | `<<`  `>>`    | 
| [Operátory relační a typové zkoušky](expressions.md#relational-and-type-testing-operators) | Relační a typové zkoušky | `<`  `>`  `<=`  `>=`  `is`  `as` | 
| [Operátory relační a typové zkoušky](expressions.md#relational-and-type-testing-operators) | Rovnost                    | `==`  `!=`    | 
| [Logické operátory](expressions.md#logical-operators)                                         | Logický operátor AND                 | `&`           | 
| [Logické operátory](expressions.md#logical-operators)                                         | Logický operátor XOR                 | `^`           | 
| [Logické operátory](expressions.md#logical-operators)                                         | Logický operátor OR                  | <code>&#124;</code>           |
| [Podmíněné logické operátory](expressions.md#conditional-logical-operators)                 | Podmiňovací operátor AND             | `&&`          | 
| [Podmíněné logické operátory](expressions.md#conditional-logical-operators)                 | Podmiňovací operátor OR              | <code>&#124;&#124;</code>          | 
| [Null operátor sloučení](expressions.md#the-null-coalescing-operator)                   | Nulové sloučení             | `??`          | 
| [Podmíněný operátor](expressions.md#conditional-operator)                                   | Podmiňovací operátor                 | `?:`          | 
| [Operátory přiřazení](expressions.md#assignment-operators), [výrazy anonymní funkce](expressions.md#anonymous-function-expressions)  | Přiřazení a výraz lambda | `=`  `*=`  `/=`  `%=`  `+=`  `-=`  `<<=`  `>>=`  `&=`  `^=`  <code>&#124;=</code>  `=>` | 

V případě operand mezi dva operátory se stejnou prioritou – ovládací prvky pořadí, ve kterém jsou operace prováděny asociativity operátorů:

*  S výjimkou operátory přiřazení a null operátor sloučení, všechny binární operátory jsou ***asociativní zleva***, což znamená, že operace se provádějí zleva doprava. Například `x + y + z` se vyhodnotí jako `(x + y) + z`.
*  Operátory přiřazení, null operátor sloučení a podmiňovací operátor (`?:`) jsou ***asociativní zprava***, což znamená, že operace se provádějí zprava doleva. Například `x = y = z` se vyhodnotí jako `x = (y = z)`.

Přednost a asociativita operátorů lze ovládat pomocí závorek. Například `x + y * z` nejprve vynásobí `y` podle `z` a pak přidá výsledek, který má `x`, ale `(x + y) * z` nejprve přidá `x` a `y` a pak vynásobí výsledků `z`.

### <a name="operator-overloading"></a>Přetížení operátoru

Všechny jednočlenné a binární operátory mají předdefinované implementace, které jsou automaticky dostupné v libovolný výraz. Kromě předdefinovaných implementací, uživatelsky definované implementace může být zavedeno zahrnutím `operator` prohlášení do třídy a struktury ([operátory](classes.md#operators)). Uživatelem definovaný operátor implementace vždy přednost před implementací předdefinovaný operátor: Pouze pokud neexistují žádné použitelné uživatelem definovaný operátor implementace bude brát předdefinovaný operátor implementace, jak je popsáno v [unární operátor rozlišení přetěžování](expressions.md#unary-operator-overload-resolution) a [binární operátor přetížení rozlišení](expressions.md#binary-operator-overload-resolution).

***Očekával se přetěžovatelný unární operátory*** jsou:
```csharp
+   -   !   ~   ++   --   true   false
```

I když `true` a `false` nejsou explicitně použít ve výrazech (a proto nejsou zahrnuty v tabulce priority v [Priorita a asociativita operátora](expressions.md#operator-precedence-and-associativity)), protože jsou jsou považovány za operátory vyvolána v několika kontextech výraz: logické výrazy ([logické výrazy](expressions.md#boolean-expressions)) a výrazy, které obsahují podmínku ([Podmiňovací operátor](expressions.md#conditional-operator)) a podmíněné logické operátory ([podmíněné logické operátory](expressions.md#conditional-logical-operators)).

***Očekával se přetěžovatelný binární operátory*** jsou:
```csharp
+   -   *   /   %   &   |   ^   <<   >>   ==   !=   >   <   >=   <=
```

Mohou být přetíženy pouze operátory, které jsou uvedené výše. Zejména, není možné přetížit přístup ke členu, volání metody nebo `=`, `&&`, `||`, `??`, `?:`, `=>`, `checked`, `unchecked`, `new`, `typeof`, `default`, `as`, a `is` operátory.

Při je binární operátor přetížen, odpovídající operátor přiřazení, pokud existuje, je také implicitně přetížená. Například přetížení operátoru `*` je také přetížení operátoru `*=`. Toto je popsáno dále v [složené přiřazení](expressions.md#compound-assignment). Všimněte si, že operátor přiřazení (`=`) se nedají přetěžovat. Přiřazení vždy provádí jednoduché kopírování bitové hodnoty do proměnné.

Operace, jako například přetypování `(T)x`, jsou přetížené poskytnutím uživatelem definované převody ([uživatelem definované převody](conversions.md#user-defined-conversions)).

Přístup k elementu, jako například `a[x]`, není považováno za očekával se přetěžovatelný operátor. Místo toho uživatelem definované indexování se podporuje na indexery ([indexery](classes.md#indexers)).

Ve výrazech operátory je odkazováno pomocí zápisu operátor a v deklaracích, operátory jsou odkazovány pomocí funkční zápis. Následující tabulka znázorňuje vztah mezi funkční zápisy jednočlenné a binární operátory a operátor. V první položce *op* označuje všechny očekával se přetěžovatelný unární operátor předpony. V položce druhý *op* označuje příponový unární `++` a `--` operátory. V položce třetí *op* označuje všechny očekával se přetěžovatelný binární operátor.


| __Zápis operátoru__ | __Funkční zápis__ |
|-----------------------|-------------------------|
| `op x`                | `operator op(x)`        | 
| `x op`                | `operator op(x)`        | 
| `x op y`              | `operator op(x,y)`      | 

Uživatelem definovaný operátor deklarace vždy vyžadují alespoň jeden z parametrů typu třídy nebo struktury, obsahující deklarace operátoru. Proto není možné pro uživatelem definovaný operátor mít stejný podpis jako předdefinovaný operátor.

Uživatelem definovaný operátor deklarace nelze upravit, syntaxe, Priorita a asociativita operátora. Například `/` operátor je vždy binárních operátorů, vždy má úroveň priority zadané v [Priorita a asociativita operátora](expressions.md#operator-precedence-and-associativity)a vždy je asociativní zprava doleva.

I když je možné pro uživatelem definovaný operátor provádět jakékoli výpočty, které ho pleases, implementace, které výsledky než ty, kteří budou intuitivně se důrazně nedoporučuje. Například implementace `operator ==` musí porovnat dva operandy na rovnost a vrátí odpovídající `bool` výsledek.

Popisy jednotlivých operátorech v [primární výrazy](expressions.md#primary-expressions) prostřednictvím [podmíněné logické operátory](expressions.md#conditional-logical-operators) zadejte předdefinované implementace operátorů a veškerá další pravidla, které se vztahují Každý operátor. Ujistěte se, popisy použijte podmínek ***unární operátor rozlišení přetěžování***, ***binárním operátorem rozlišení přetěžování***, a ***číselné povýšení***, definice, které jsou najít v následujících částech.

### <a name="unary-operator-overload-resolution"></a>Řešení přetížení unární operátor

Operace formuláře `op x` nebo `x op`, kde `op` se očekával se přetěžovatelný unární operátor a `x` je výraz typu `X`, zpracování následujícím způsobem:

*  Sadu Release candidate uživatelsky definované operátory poskytované `X` pro operaci `operator op(x)` je určen pomocí pravidel pro [Release Candidate uživatelsky definované operátory](expressions.md#candidate-user-defined-operators).
*  Pokud sadu Release candidate uživatelsky definované operátory není prázdná, to bude sadu operátorů Release candidate pro operaci. V opačném případě předdefinované unární `operator op` implementací, včetně jejich zdvižené formulářů, stane sadu operátorů Release candidate pro operaci. Předdefinované implementace daný operátor jsou uvedeny v popisu operátoru ([primární výrazy](expressions.md#primary-expressions) a [unárních operátorů](expressions.md#unary-operators)).
*  Pravidla rozlišení přetížení [rozlišení přetěžování](expressions.md#overload-resolution) aplikují i na sadu operátorů Release candidate vybrat nejlepší operátor s ohledem na seznam argumentů `(x)`, a tento operátor změní výsledek přetížení Proces překladu. Pokud se nepodaří určit přetížení k výběru jednoho nejlepší operátoru, dojde k chybě vazby čas.

### <a name="binary-operator-overload-resolution"></a>Binární operátor rozlišení přetížení

Operace formuláře `x op y`, kde `op` se očekával se přetěžovatelný binární operátor `x` je výraz typu `X`, a `y` je výraz typu `Y`, zpracování následujícím způsobem:

*  Sadu Release candidate uživatelsky definované operátory poskytované `X` a `Y` pro operaci `operator op(x,y)` je určen. Sada zahrnuje union operátorů Release candidate poskytované `X` operátory a operátory Release candidate poskytované `Y`, každý určené pomocí pravidel pro [Release Candidate uživatelsky definované operátory](expressions.md#candidate-user-defined-operators). Pokud `X` a `Y` jsou stejného typu, nebo pokud `X` a `Y` jsou odvozeny z běžné základní typ, pak sdílené Release candidate operátory vyskytovat jenom v sadě kombinované jednou.
*  Pokud sadu Release candidate uživatelsky definované operátory není prázdná, to bude sadu operátorů Release candidate pro operaci. V opačném případě předdefinovaný binární `operator op` implementací, včetně jejich zdvižené formulářů, stane sadu operátorů Release candidate pro operaci. Předdefinované implementace daný operátor jsou uvedeny v popisu operátoru ([aritmetické operátory](expressions.md#arithmetic-operators) prostřednictvím [podmíněné logické operátory](expressions.md#conditional-logical-operators)). Pro předdefinované operátory výčtu a delegáta jen operátory považovat za jsou definované typem enum nebo delegate, který je typ vazby time jeden z operandů.
*  Pravidla rozlišení přetížení [rozlišení přetěžování](expressions.md#overload-resolution) aplikují i na sadu operátorů Release candidate vybrat nejlepší operátor s ohledem na seznam argumentů `(x,y)`, a tento operátor změní výsledek přetížení Proces překladu. Pokud se nepodaří určit přetížení k výběru jednoho nejlepší operátoru, dojde k chybě vazby čas.

### <a name="candidate-user-defined-operators"></a>Uživatelem definované operátory Release Candidate

Typ zadaný `T` a operace `operator op(A)`, kde `op` se očekával se přetěžovatelný operátor a `A` je seznam argumentů, sadu Release candidate uživatelsky definované operátory poskytované `T` pro `operator op(A)` je určen následujícím způsobem:

*  Určení typu `T0`. Pokud `T` je typ připouštějící hodnotu Null, `T0` je základní typ, v opačném případě `T0` rovná `T`.
*  Pro všechny `operator op` deklarace v `T0` a všechny zrušeno formy takových operátorů, pokud platí aspoň jeden – operátor ([použitelná funkce člena](expressions.md#applicable-function-member)) s ohledem na seznam argumentů `A`, pak sada operátory Release Candidate se skládá z takových příslušných operátorů v `T0`.
*  Jinak, pokud `T0` je `object`, sadu operátorů Release candidate je prázdný.
*  V opačném případě poskytuje sadu operátorů Release candidate `T0` sadu Release candidate operátory poskytované přímé základní třídy `T0`, nebo efektivní základní třídu `T0` Pokud `T0` je parametr typu.

### <a name="numeric-promotions"></a>Číselné propagačních akcí

Číselné povýšení se skládá z automaticky provádět určité implicitní převody operandů předdefinované jednočlenné a binární číselný operátory. Číselné promoakce není odlišné mechanismus, ale spíše efekt použití předdefinované operátory řešení přetížení. Číselné povýšení konkrétně nemá vliv na vyhodnocení operátory definované uživatelem, i když je možné implementovat uživatelsky definované operátory vykazovat podobné účinky.

Jako příklad číselné povýšení, vezměte v úvahu předdefinované implementace binárního souboru `*` operátor:

```csharp
int operator *(int x, int y);
uint operator *(uint x, uint y);
long operator *(long x, long y);
ulong operator *(ulong x, ulong y);
float operator *(float x, float y);
double operator *(double x, double y);
decimal operator *(decimal x, decimal y);
```

Při přetížení pravidla řešení ([rozlišení přetěžování](expressions.md#overload-resolution)) se použijí pro tuto sadu operátorů, je vybrat první operátory, pro které existuje implicitní převody z typů operand efekt. Například pro operaci `b * s`, kde `b` je `byte` a `s` je `short`, vybere rozlišení přetížení `operator *(int,int)` jako nejlepší operátor. Proto efekt je, že `b` a `s` jsou převedeny na `int`, a typ výsledku je `int`. Podobně pro operaci `i * d`, kde `i` je `int` a `d` je `double`, vybere rozlišení přetížení `operator *(double,double)` jako nejlepší operátor.

#### <a name="unary-numeric-promotions"></a>Unární číselné propagačních akcí

Pro operandy předdefinovaného dojde k povýšení číselné unární `+`, `-`, a `~` unárních operátorů. Unární číselné povýšení jednoduše se skládá z převodu operandy typu `sbyte`, `byte`, `short`, `ushort`, nebo `char` na typ `int`. Kromě toho pro unární `-` operátor unární číselné povýšení převede operandy typu `uint` na typ `long`.

#### <a name="binary-numeric-promotions"></a>Binární číselný propagačních akcí

Pro operandy předdefinovaného dojde k binární číselný povýšení `+`, `-`, `*`, `/`, `%`, `&`, `|`, `^`, `==`, `!=`, `>`, `<`, `>=`, a `<=` binární operátory. Oba operandy binární číselný povýšení implicitně převede na společný typ, který se v případě – relační operátory stane typ výsledku operace. Binární číselný povýšení se skládá z použití následující pravidla v pořadí, ve kterém jsou uvedeny zde:

*  Pokud některý operand je typu `decimal`, je druhý operand je převeden na typ `decimal`, nebo pokud je druhý operand je typu, dojde k chybě vazby čas `float` nebo `double`.
*  Jinak, pokud některý operand je typu `double`, je druhý operand je převeden na typ `double`.
*  Jinak, pokud některý operand je typu `float`, je druhý operand je převeden na typ `float`.
*  Jinak, pokud některý operand je typu `ulong`, je druhý operand je převeden na typ `ulong`, nebo pokud je druhý operand je typu, dojde k chybě vazby čas `sbyte`, `short`, `int`, nebo `long`.
*  Jinak, pokud některý operand je typu `long`, je druhý operand je převeden na typ `long`.
*  Jinak, pokud některý operand je typu `uint` a druhý operand je typu `sbyte`, `short`, nebo `int`, jsou oba operandy převedeny na typ `long`.
*  Jinak, pokud některý operand je typu `uint`, je druhý operand je převeden na typ `uint`.
*  Jinak jsou oba operandy převedeny na typ `int`.

Všimněte si, že první pravidlo nepovoluje žádné operace, které kombinovat `decimal` typ s `double` a `float` typy. Následující pravidlo ze skutečnosti, že neexistují žádné implicitní převody mezi `decimal` typ a `double` a `float` typy.

Všimněte si také, že není možné pro operand typu `ulong` Pokud je druhý operand je celočíselný typ se znaménkem. Důvodem je, že neexistuje žádné celočíselného typu, který může představovat celou škálu `ulong` a také podepsaných integrálních typů.

V obou případech výše výraz přetypování lze explicitně převést na typ, který je kompatibilní s je druhý operand jeden operand.

V příkladu
```csharp
decimal AddPercent(decimal x, double percent) {
    return x * (1.0 + percent / 100.0);
}
```
Doba vazby chybě dochází, protože `decimal` nelze bude vynásobené hodnotou `double`. Explicitně převedením Druhý operand, který se problém nevyřeší `decimal`, následujícím způsobem:

```csharp
decimal AddPercent(decimal x, double percent) {
    return x * (decimal)(1.0 + percent / 100.0);
}
```

### <a name="lifted-operators"></a>Zdvižené operátory

***Zrušeno vs. operátory*** povolit předdefinované a uživatelem definované operátory, které pracují s typy hodnot neumožňující hodnotu lze použít také s možnou hodnotou Null formuláře z těchto typů. Zdvižené operátory se vytvářejí na základě předdefinovaných a uživatelem definované operátory, které splňují určité požadavky, jak je popsáno v následujících tématech:

*   Unárních operátorů

    ```csharp
    +  ++  -  --  !  ~
    ```

    zdvižené formulář operátoru existuje, pokud jsou typy operandů a výsledek oba typy hodnot neumožňující hodnotu. Zdvižené formuláře je vytvořený tak, že přidáte jediného `?` Modifikátor pro typy operandů a výsledek. Zdvižené operátor vytvoří hodnotu null, je-li operand hodnotu null. V opačném případě operátor zdvižené rozbalí operand, použije operátor základní a zabalí výsledek.

*   Pro binární operátory

    ```csharp
    +  -  *  /  %  &  |  ^  <<  >>
    ```

    zdvižené formulář operátoru existuje-li operand a výsledek typy jsou všechny typy hodnot neumožňující hodnotu. Zdvižené formuláře je vytvořený tak, že přidáte jediného `?` Modifikátor pro každý typ operandu a výsledek. Zdvižené operátor vytvoří hodnotu null, pokud jeden nebo oba operandy hodnotu null (se výjimka `&` a `|` provozovatelé `bool?` zadejte, jak je popsáno v [logické logické operátory](expressions.md#boolean-logical-operators)). V opačném případě operátor zdvižené rozbalí operandy, použije operátor základní a zabalí výsledek.

*   Pro operátory rovnosti

    ```csharp
    ==  !=
    ```

    zdvižené formulář operátoru existuje-li typy operandů jsou typy hodnot neumožňující hodnotu a zda je typ výsledku `bool`. Zdvižené formuláře je vytvořený tak, že přidáte jediného `?` Modifikátor pro každý typ operandu. Operátor zdvižené bere v úvahu rovnosti dvou hodnot null a hodnota null nerovnost na libovolnou hodnotu jinou hodnotu než null. Pokud jsou oba operandy jinou hodnotu než null, zdvižené operátor, který se rozbalí operandy a použije základní operátoru pro vytvoření `bool` výsledek.

*   Pro relační operátory

    ```csharp
    <  >  <=  >=
    ```

    zdvižené formulář operátoru existuje-li typy operandů jsou typy hodnot neumožňující hodnotu a zda je typ výsledku `bool`. Zdvižené formuláře je vytvořený tak, že přidáte jediného `?` Modifikátor pro každý typ operandu. Zdvižené operátor vytvoří hodnotu `false` Pokud jeden nebo oba operandy jsou null. V opačném případě operátor zdvižené rozbalí operandy a použije základní operátoru pro vytvoření `bool` výsledek.

## <a name="member-lookup"></a>Člen vyhledávání

Člen vyhledávání je proces, kterým se určuje podle názvu v kontextu typu. Člen vyhledávání může dojít v rámci vyhodnocování *simple_name* ([jednoduché názvy](expressions.md#simple-names)) nebo *member_access* ([přístup ke členu](expressions.md#member-access)) v výraz. Pokud *simple_name* nebo *member_access* vyskytuje se jako *primary_expression* ze *invocation_expression* ([ Volání metod](expressions.md#method-invocations)), člen se říká, že má být volána.

Pokud je člen metody nebo události, nebo pokud je konstanta, pole nebo vlastnost typu delegáta ([delegáti](delegates.md)) nebo typ `dynamic` ([dynamického typu](types.md#the-dynamic-type)), pak člen je označen jako *nevyvolatelný*.

Člen vyhledávání bere v úvahu nejen názvu členem, ale také počet parametrů typu, který člen má a určuje, zda je přístupný člen. Pro účely vyhledávání člen obecné metody a vnořených obecných typech mají počet parametrů typu, které jsou uvedené v jejich odpovídajících deklarací a všechny ostatní členové mají nulové parametry typu.

Člen vyhledávání názvu `N` s `K`  zadejte parametry typu `T` zpracování následujícím způsobem:

*  První, sadu přístupní členové s názvem `N` závisí:
    * Pokud `T` je parametr typu sjednocení sad přístupní členové s názvem je sada `N` v jednotlivých typů stanoveno, omezení pro primární nebo sekundární omezení ([omezení parametru typu](classes.md#type-parameter-constraints)) pro  `T`, spolu s sadu přístupní členové s názvem `N` v `object`.
    * V opačném případě sada zahrnuje vše je přístupné ([přístup ke členu](basic-concepts.md#member-access)) členové s názvem `N` v `T`, včetně zděděných členů a přístupní členové s názvem `N` v `object`. Pokud `T` konstruovaný typ, je získat sadu členů nahrazením argumentů typu, jak je popsáno v [členy sestavené typy](classes.md#members-of-constructed-types). Členy, které patří `override` modifikátor jsou vyloučeny ze sady.
*  Dále, pokud `K` je nula, všechny vnořené typy deklarací, jejichž zahrnují parametry typu se odeberou. Pokud `K` není nulový, všichni členové s jiným číslem typu parametry se odeberou. Všimněte si, že `K` je nula, metody s parametry nejsou odebrány od procesu odvození typu typ ([odvození typu](expressions.md#type-inference)) možné odvodit argumenty typu.
*  Další, pokud je člen *vyvolána*, to všechno bez-*nevyvolatelný* členy jsou odebrány z objektu set.
*  V dalším kroku členy, které jsou skryté členy jiné se odeberou ze sady. Pro každého člena `S.M` v sadě, kde `S` typ, ve kterém je člen `M` je teď deklarována, se použijí následující pravidla:
    * Pokud `M` je – konstanta, pole, vlastnosti, události nebo člen výčtu, pak všechny členy deklarované v základní typ `S` se odeberou ze sady.
    * Pokud `M` je deklarace typu, pak všechny jiné typy deklarovaný v základní typ `S` se odeberou ze sady, a všechny deklarace s stejný počet parametrů typu jako `M` deklarovaný v základní typ `S` jsou odebrány ze sady.
    * Pokud `M` je metoda, pak všechny členy jiné metody deklarované v základní typ `S` se odeberou ze sady.
*  V dalším kroku členy rozhraní, které jsou skryté členy třídy jsou odebrány z objektu set. Tento krok má vliv pouze, pokud `T` je parametr typu a `T` i základní třída má jiné než `object` a efektivní rozhraní neprázdný nastavit ([omezení parametru typu](classes.md#type-parameter-constraints)). Pro každého člena `S.M` v sadě, kde `S` typ, ve kterém je člen `M` je teď deklarována, pokud se použijí následující pravidla `S` deklarace třídy je jiné než `object`:
    * Pokud `M` je – konstanta, pole, vlastnosti, události, člen výčtu nebo deklarace typu, pak všechny členy deklarované v deklaraci rozhraní jsou odebrány z objektu set.
    * Pokud `M` je metoda, pak všechny členy jiné metody deklarované v deklaraci rozhraní se odeberou ze sady a všech metod se stejným podpisem jako `M` deklarované v rozhraní deklarace se odeberou ze sady.
*  Nakonec odebrání skryté členy, se určit výsledek vyhledávání:
    * Pokud sada obsahuje jeden člen, který není metoda, je tento člen výsledků vyhledávání.
    * Jinak, pokud sada obsahuje pouze metody, pak tato skupina metod je výsledek vyhledávání.
    * V opačném případě vyhledávání je nejednoznačný a dojde k chybě vazby čas.

Pro člen vyhledávání v jiné typy než typy parametrů a rozhraní a člen vyhledávání v rozhraní, která jsou výhradně jednoduchou dědičností (každé rozhraní v řetězu dědičnosti má přesně žádnou nebo jednu přímou základní rozhraní), je efekt pravidla vyhledávání jednoduše, která odvozena základních členů skrýt členy s totožným názvem a signaturou. Tyto jednoduché dědičnosti vyhledávání nejsou nikdy nejednoznačný. Nejednoznačnosti, které může být mohou vyplývat z vyhledávání člena v rozhraní vícenásobné dědičnosti jsou popsány v [rozhraní přístup ke členu](interfaces.md#interface-member-access).

### <a name="base-types"></a>Základní typy

Pro účely člen vyhledávání, typ `T` se považuje za mají následující základní typy:

*  Pokud `T` je `object`, pak `T` nemá žádný základní typ.
*  Pokud `T` je *enum_type*, základní typy `T` jsou typy tříd `System.Enum`, `System.ValueType`, a `object`.
*  Pokud `T` je *struct_type*, základní typy `T` jsou typy tříd `System.ValueType` a `object`.
*  Pokud `T` je *class_type*, základní typy `T` jsou základní třídy `T`, včetně typu třídy `object`.
*  Pokud `T` je *interface_type*, základní typy `T` jsou základní rozhraní `T` a typ třídy `object`.
*  Pokud `T` je *array_type*, základní typy `T` jsou typy tříd `System.Array` a `object`.
*  Pokud `T` je *delegate_type*, základní typy `T` jsou typy tříd `System.Delegate` a `object`.

## <a name="function-members"></a>Členové – funkce

Funkční členy jsou členy, které obsahují spustitelné příkazy. Funkční členy jsou vždy členy typů a nemůžou být členy skupiny obory názvů. C# definuje následující kategorie funkcí členů:

*  Metody
*  Vlastnosti
*  Události
*  Indexery
*  Uživatelem definované operátory
*  Konstruktory instancí
*  Statické konstruktory
*  Destruktory

S výjimkou destruktory a statické konstruktory (což nesmí být volány explicitně) jsou spuštěny příkazy součástí členy funkce prostřednictvím volání členské funkce. Skutečná syntaxe pro zápis volání členské funkce závisí na kategorii členu konkrétní funkce.

Seznam argumentů ([seznamy argumentů](expressions.md#argument-lists)) z členské funkce volání poskytuje skutečné hodnoty nebo odkazy na proměnné parametrů členské funkce.

Volání obecné metody mohou použít odvození typu k určení sady argumenty typu pro metodu. Tento proces je popsán v [odvození typu](expressions.md#type-inference).

Volání metody, indexery, operátory a konstruktory instancí využívat řešení přetížení pro určení, které Release candidate sadu členů funkce vyvolat. Tento proces je popsán v [rozlišení přetěžování](expressions.md#overload-resolution).

Po chvíli vazba byla zjištěna členem určitou funkci, pravděpodobně prostřednictvím řešení přetížení volání členské funkce skutečný proces za běhu je popsáno v [kontrolu dynamické přetíženíkompilace](expressions.md#compile-time-checking-of-dynamic-overload-resolution).

Následující tabulka shrnuje zpracování, které u něho v konstruktorech zahrnující šesti kategorií funkce členy, které lze explicitně vyvolat. V tabulce `e`, `x`, `y`, a `value` označení výrazy, které jsou klasifikovány jako proměnné nebo hodnoty, `T` Určuje výraz, který jsou klasifikovány jako typ, `F` je jednoduchý název metody a `P` je jednoduchý název vlastnosti.


| __Konstrukce__     | __Příklad__    | __Popis__ |
|-------------------|----------------|-----------------|
| Volání metody | `F(x,y)`       | Řešení přetížení se použije pro výběr nejlepší metody `F` v obsahující třídy nebo struktury. Metoda je volána s seznamu argumentů `(x,y)`. Pokud metoda není `static`, je výraz instance `this`. | 
|                   | `T.F(x,y)`     | Řešení přetížení se použije pro výběr nejlepší metody `F` ve třídě nebo struktuře `T`. Chyba vazby za nastane, pokud metoda není `static`. Metoda je volána s seznamu argumentů `(x,y)`. | 
|                   | `e.F(x,y)`     | Řešení přetížení se použije k výběru nejvhodnější způsob F v třídy, struktury nebo rozhraní uvedena v každém typu `e`. Pokud je metoda, dojde k chybě doba vazby `static`. Metoda je volána s výraz instance `e` a seznam argumentů `(x,y)`. | 
| Přístup k vlastnosti   | `P`            | `get` Přistupující objekt vlastnosti `P` v obsahující třídy nebo struktury je vyvolána. Pokud dojde k chybě kompilace `P` je jen pro zápis. Pokud `P` není `static`, je výraz instance `this`. | 
|                   | `P = value`    | `set` Přistupující objekt vlastnosti `P` v obsahující třídy nebo struktury je vyvolán pomocí seznamu argumentů `(value)`. Pokud dojde k chybě kompilace `P` je jen pro čtení. Pokud `P` není `static`, je výraz instance `this`. | 
|                   | `T.P`          | `get` Přistupující objekt vlastnosti `P` ve třídě nebo struktuře `T` je vyvolána. Pokud dojde k chybě kompilace `P` není `static` nebo, pokud `P` je jen pro zápis. | 
|                   | `T.P = value`  | `set` Přistupující objekt vlastnosti `P` ve třídě nebo struktuře `T` vyvolání seznamu argumentů `(value)`. Pokud dojde k chybě kompilace `P` není `static` nebo, pokud `P` je jen pro čtení. | 
|                   | `e.P`          | `get` Přistupující objekt vlastnosti `P` v třídy, struktury nebo rozhraní uvedena v každém typu `e` vyvolání výraz instance `e`. Pokud dojde k chybě vazby čas `P` je `static` nebo, pokud `P` je jen pro zápis. | 
|                   | `e.P = value`  | `set` Přistupující objekt vlastnosti `P` v třídy, struktury nebo rozhraní uvedena v každém typu `e` vyvolání výraz instance `e` a seznam argumentů `(value)`. Pokud dojde k chybě vazby čas `P` je `static` nebo, pokud `P` je jen pro čtení. | 
| Přístup k události      | `E += value`   | `add` Přístupového objektu události `E` v obsahující třídy nebo struktury je vyvolána. Pokud `E` není statická, je výraz instance `this`. | 
|                   | `E -= value`   | `remove` Přístupového objektu události `E` v obsahující třídy nebo struktury je vyvolána. Pokud `E` není statická, je výraz instance `this`. | 
|                   | `T.E += value` | `add` Přístupového objektu události `E` ve třídě nebo struktuře `T` je vyvolána. Pokud dojde k chybě vazby čas `E` není statická. | 
|                   | `T.E -= value` | `remove` Přístupového objektu události `E` ve třídě nebo struktuře `T` je vyvolána. Pokud dojde k chybě vazby čas `E` není statická. | 
|                   | `e.E += value` | `add` Přístupového objektu události `E` v třídy, struktury nebo rozhraní uvedena v každém typu `e` vyvolání výraz instance `e`. Pokud dojde k chybě vazby čas `E` je statická. | 
|                   | `e.E -= value` | `remove` Přístupového objektu události `E` v třídy, struktury nebo rozhraní uvedena v každém typu `e` vyvolání výraz instance `e`. Pokud dojde k chybě vazby čas `E` je statická. | 
| Přístup indexeru    | `e[x,y]`       | Řešení přetížení se použije k výběru nejvhodnější indexer v třídy, struktury nebo rozhraní uvedena v každém typu e. `get` Vyvolání přistupující objekt indexer s výrazem instance `e` a seznam argumentů `(x,y)`. Pokud indexeru je jen pro zápis, dojde k chybě vazby – za. | 
|                   | `e[x,y] = value` | Řešení přetížení se použije k výběru nejvhodnější indexer v třídy, struktury nebo rozhraní uvedena v každém typu `e`. `set` Vyvolání přistupující objekt indexer s výrazem instance `e` a seznam argumentů `(x,y,value)`. Pokud indexeru je jen pro čtení, dojde k chybě vazby – za. | 
| Operátor vyvolání | `-x`         | Řešení přetížení se použije k výběru nejvhodnější unární operátor v dané třídy nebo struktury uvedena v každém typu `x`. Vybraný operátor vyvolání seznamu argumentů `(x)`. | 
|                     | `x + y`      | Řešení přetížení se použije k výběru nejvhodnější binární operátor v třídy nebo struktury, které jsou uvedeny typy `x` a `y`. Vybraný operátor vyvolání seznamu argumentů `(x,y)`. | 
| Vyvolání konstruktoru instance | `new T(x,y)` | Řešení přetížení se použije k výběru nejvhodnější konstruktor instance ve třídě nebo struktuře `T`. Vyvolání konstruktoru instance seznamu argumentů `(x,y)`. | 

### <a name="argument-lists"></a>Seznamy argumentů

Každý člen a delegáta volání funkce obsahuje seznam argumentů, které poskytuje skutečné hodnoty nebo odkazy na proměnné parametrů členské funkce. Syntaxe pro zadání seznamu argumentů volání členské funkce závisí na kategorii členu funkce:

*  Pro instanci konstruktorů, metod, indexerů a delegátů, argumenty jsou zadané jako *argument_list*, jak je popsáno níže. Pro indexery, při vyvolání `set` přístupový objekt, v seznamu argumentů navíc obsahuje výraz zadaný jako pravý operand operátoru přiřazení.
*  Pro vlastnosti, je prázdný seznam argumentů, při vyvolání `get` přístupový objekt a skládá se z výraz zadaný jako pravý operand operátoru přiřazení při vyvolání `set` přistupujícího objektu.
*  Pro události, seznam argumentů se skládá z výraz zadaný jako pravý operand `+=` nebo `-=` operátor.
*  Pro uživatelsky definované operátory seznamu argumentů se skládá z jediného operandu unární operátor nebo dva operandy binární operátor.

Argumenty vlastnosti ([vlastnosti](classes.md#properties)), události ([události](classes.md#events)) a uživatelsky definované operátory ([operátory](classes.md#operators)) jsou vždy předány jako parametry s hodnotou ([ Hodnoty parametrů](classes.md#value-parameters)). Argumenty indexery ([indexery](classes.md#indexers)) jsou vždy předány jako parametry s hodnotou ([hodnoty parametrů](classes.md#value-parameters)) nebo pole parametrů ([pole parametrů](classes.md#parameter-arrays)). Parametry odkazu a výstup nejsou podporovány pro tyto kategorie funkce členů.

Argumenty vyvolání konstruktoru, metoda, indexer nebo delegáta instance jsou zadány jako *argument_list*:

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

*Argument_list* obsahuje jeden nebo více *argument*s, oddělené čárkami. Každý argument se skládá z volitelné *argument_name* následovaný *argument_value*. *Argument* s *argument_name* se označuje jako ***pojmenovaný argument***, že *argument* bez  *argument_name* je ***poziční argument***. Jedná se o chybu pro poziční argument, který se zobrazí za pojmenovaným argumentem v *argument_list*.

*Argument_value* můžete provést jednu z následujících forem:

*  *Výraz*, což indikuje, že argument je předán jako parametr hodnoty ([hodnoty parametrů](classes.md#value-parameters)).
*  Klíčové slovo `ref` následovaný *variable_reference* ([odkazy na proměnné](variables.md#variable-references)), což indikuje, že argument je předán jako parametr odkazu ([odkazovat na parametry ](classes.md#reference-parameters)). Proměnná musí být jednoznačně přiřazena ([jednoznačného přiřazení](variables.md#definite-assignment)) předtím, než může být předán jako parametr odkazu. Klíčové slovo `out` následovaný *variable_reference* ([odkazy na proměnné](variables.md#variable-references)), což indikuje, že argument je předán jako výstupní parametr ([výstupních parametrů](classes.md#output-parameters)). Proměnná je považován za jednoznačně přiřazené ([jednoznačného přiřazení](variables.md#definite-assignment)) po volání členské funkce ve které se předá proměnná jako výstupní parametr.

#### <a name="corresponding-parameters"></a>Odpovídající parametry

Pro každý argument v seznamu argumentů musí existovat odpovídající parametr v členské funkci nebo vyvolání delegáta.

Seznam parametrů používaných tímto se určuje následujícím způsobem:

*  Pro virtuální metody a indexery definované ve třídách seznam parametrů se vybere od nejkonkrétnější deklarace nebo přepsat z členské funkce, počínaje statický typ příjemce a prohledávat jejích základních tříd.
*  Pro metody rozhraní a indexery, seznam parametrů výběru formuláře nejspecifičtější definice člena, počínaje typ rozhraní a prohledávat základních rozhraní. Pokud seznam jedinečných parametrů nenajde, je vytvořen seznam parametrů s nepřístupný názvy a žádné volitelné parametry, tak, aby volání nelze použít pojmenované parametry nebo vynechejte volitelné argumenty.
*  Pro částečné metody se používá parametr seznam definující deklarace částečné metody.
*  Pro všechny ostatní funkce členy a delegáti je pouze jeden seznam parametrů, které se bude používat.

Pozice argumentu nebo parametr se definuje jako počet argumentů nebo parametry předcházející v seznam argumentů nebo seznamu parametrů.

Odpovídající parametry pro argumenty pro členské funkce jsou vytvořeny následujícím způsobem:

*  Argumenty v *argument_list* konstruktory instancí, metod, indexerů a delegátů:
    * Poziční argument, kde dochází k dlouhodobého parametr na stejné pozici v seznamu parametrů odpovídá tomuto parametru.
    * Poziční argument členské funkce s polem parametrů vyvolání v podobě normální odpovídá pole parametrů, které se musí vyskytovat na stejné pozici v seznamu parametrů.
    * Poziční argument členské funkce s polem parametrů vyvolání v podobě rozšířené žádná pevná parametr kde dojde k na stejné pozici v seznamu parametrů, odpovídá na prvek v poli parametrů.
    * Pojmenovaný argument odpovídající parametru se stejným názvem v seznamu parametrů.
    * Pro indexery, při vyvolání `set` přístupový objekt, výraz zadaný jako pravý operand operátoru přiřazení odpovídá implicitní `value` parametr `set` deklarace přistupujícího objektu.
*  Pro vlastnosti, při vyvolání `get` existuje přístupový objekt se žádné argumenty. Při vyvolání `set` přístupový objekt, výraz zadaný jako pravý operand operátoru přiřazení odpovídá implicitní `value` parametr `set` deklarace přistupujícího objektu.
*  Uživatelem definované unárních operátorů (včetně převodů) jeden operand odpovídá jeden parametr deklarace operátoru.
*  Pro binární operátory definované uživatelem levý operand odpovídá na první parametr a pravý operand odpovídá druhý parametr deklarace operátoru.

#### <a name="run-time-evaluation-of-argument-lists"></a>Vyhodnocení seznamů argumentů

Při zpracování volání členské funkce za běhu ([kompilace kontrolu dynamické přetížení](expressions.md#compile-time-checking-of-dynamic-overload-resolution)), výrazy nebo odkazy na proměnné seznamu argumentů se vyhodnocují v pořadí zleva doprava, jako následující:

*  Hodnota parametru, je vyhodnocen výraz argumentu a implicitní převod ([implicitních převodů](conversions.md#implicit-conversions)) do odpovídajícího parametru typu provést. Výsledná hodnota bude počáteční hodnota parametru hodnotu ve volání funkce člena.
*  Pro parametr odkaz nebo výstup reference proměnné je vyhodnocena a výsledný umístění úložiště, změní umístění úložiště, který je reprezentován parametrem ve volání funkce člena. Pokud odkaz na proměnnou zadána jako parametr odkaz nebo výstup je prvek pole *reference_type*, kontrolu za běhu se provádí za účelem zajištění, že typ prvku pole je stejný jako typ parametru. Pokud tato kontrola neúspěšná, `System.ArrayTypeMismatchException` je vyvolána výjimka.

Metody, indexery a konstruktory instancí může deklarovat jejich krajní pravý parametr bude pole parametrů ([pole parametrů](classes.md#parameter-arrays)). Tyto funkce členy jsou vyvolány v jejich normálním formuláře nebo v jejich rozšířené podobě, v závislosti na které se vztahuje ([použít funkční člen](expressions.md#applicable-function-member)):

*  Po vyvolání členské funkce s polem parametrů v podobě normální argument pro pole parametrů musí být jediný výraz, který je implicitně převést ([implicitních převodů](conversions.md#implicit-conversions)) na typ pole parametrů. V tomto případě pole parametrů funguje přesně jako hodnotu parametru.
*  Po vyvolání členské funkce s polem parametrů v podobě rozšířené vyvolání musíte zadat nuly nebo více poziční argumenty pro pole parametrů, kde každý argument je výraz, který je implicitně převést ([implicitní převody](conversions.md#implicit-conversions)) na typ elementu pole parametrů. V takovém případě vyvolání vytvoří instanci typu parametru pole s délkou odpovídající počet argumentů, inicializuje prvky instance pole s hodnotami daný argument a používá instanci nově vytvořené pole jako skutečný argument.

Výrazy seznam argumentů jsou vždy vyhodnoceny v pořadí, ve kterém jsou zapsány. Díky tomu se v příkladu
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
```
x = 0, y = 1, z = 2
x = 4, y = -1, z = 3
```

Pravidla pole společné odchylky ([kovariance polí](arrays.md#array-covariance)) povolit hodnotu typu pole `A[]` byl odkaz na instanci typu pole `B[]`, pokud existuje implicitní referenční převod z `B` do `A`. Z důvodu tato pravidla, pokud prvek pole *reference_type* je předán jako parametr odkaz nebo výstup, je potřeba zajistit, že typ skutečným prvkem pole je stejná jako parametru kontrolu za běhu. V příkladu
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
druhé volání `F` způsobí, že `System.ArrayTypeMismatchException` vyvolání, protože typ skutečným prvkem `b` je `string` a ne `object`.

Při vyvolání členské funkce s polem parametrů v podobě rozšířené, volání se zpracovává stejně, jako kdyby výraz vytvoření pole s inicializátorem pole ([pole vytváření výrazů](expressions.md#array-creation-expressions)) byl vložen kolem rozšířené parametry. Mějme například deklarace
```csharp
void F(int x, int y, params object[] args);
```
Následující volání formuláři Rozšířené metody
```csharp
F(10, 20);
F(10, 20, 30, 40);
F(10, 20, 1, "hello", 3.0);
```
přesně odpovídají
```csharp
F(10, 20, new object[] {});
F(10, 20, new object[] {30, 40});
F(10, 20, new object[] {1, "hello", 3.0});
```

Všimněte si zejména, pokud nejsou žádné argumenty pro pole parametrů se vytvoří prázdné pole.

Pokud argumenty jsou vynechány z členské funkce s odpovídající volitelné parametry, výchozí argumenty deklarace členské funkce jsou implicitně předány. Protože jsou vždy konstantní, jejich vyhodnocení neovlivní pořadí vyhodnocování zbývajících argumentů.

### <a name="type-inference"></a>Odvození typu

Při volání obecné metody bez zadání argumentů, ***odvození typu*** proces pokusí odvodit typ argumenty pro volání. Přítomnost odvození typu umožňuje pohodlnější syntaxi pro volání obecné metody a umožňuje programátorovi, aby se vyhnout zadávání informací o typu redundantní. Mějme například deklarace metody:
```csharp
class Chooser
{
    static Random rand = new Random();

    public static T Choose<T>(T first, T second) {
        return (rand.Next(2) == 0)? first: second;
    }
}
```
je možné vyvolat `Choose` metody bez explicitního určení argumentu typu:
```csharp
int i = Chooser.Choose(5, 213);                 // Calls Choose<int>

string s = Chooser.Choose("foo", "bar");        // Calls Choose<string>
```

Prostřednictvím odvození typu proměnné, argumenty typu `int` a `string` se stanoví z argumentů metody.

Dojde k odvození typu při zpracování časově vazbu volání metody ([volání metod](expressions.md#method-invocations)) a probíhá před krokem rozlišení přetížení volání. Když konkrétní metody skupiny je zadaný ve volání metody a jako součást volání metody nejsou zadány žádné argumenty typu, odvození typu platí pro každý obecnou metodu skupinu metod. Pokud bude úspěšné odvození typu proměnné, argumenty typu slouží k určení typů argumentů pro následné přetížení. Pokud řešení přetížení vybere obecné metody jako ta, která se má vyvolat, argumenty typu slouží jako skutečný typ argumenty pro vyvolání. Pokud se nepodaří odvození typu proměnné pro konkrétní metody, této metodě není součástí řešení přetížení. Selhání odvození typu, v a sám sebe, nezpůsobí chybu v době vazby. Však to často vede k chybu v době vazby při řešení přetížení pak nepodaří najít žádné použitelné metody.

Pokud zadaný počet argumentů, které se liší od počtu parametrů v metodě, pak okamžitě odvození. V opačném případě Předpokládejme, že obecné metody obsahuje následující podpis:
```csharp
Tr M<X1,...,Xn>(T1 x1, ..., Tm xm)
```

Pomocí volání metody formuláře `M(E1...Em)` úlohou odvození typu je najít jedinečnou argumentů `S1...Sn` pro každý z parametrů typu `X1...Xn` tak, aby volání `M<S1...Sn>(E1...Em)` vstupuje v platnost.

Během procesu odvození každý parametr typu `Xi` je buď *oprava* na určitý typ `Si` nebo *nevyřešené* s přidruženou sadu *hranice*. Každá hranice je nějaký typ `T`. Zpočátku každý typ proměnné `Xi` je laťky s prázdnou sadou mezí.

Odvození typu proměnné se provádí ve fázích. Jednotlivé fáze se pokusí odvodit argumenty typu pro další proměnné typu závislosti na zjištěních předchozí fáze. První fáze provede některé počáteční závěry mezí, zatímco druhá fáze oprav typ proměnné na konkrétní typy a odvodí z nich další hranice. Druhá fáze může být třeba několikrát opakuje.

*Poznámka:* Typ odvození probíhá nejen při volání obecné metody. Odvození typu pro převod skupin metoda je popsaná v [odvození pro převod skupin metoda typu](expressions.md#type-inference-for-conversion-of-method-groups) a najít nejlepší společný typ sada výrazů je popsána v [hledání nejlepší společný typ sady výrazů](expressions.md#finding-the-best-common-type-of-a-set-of-expressions).

#### <a name="the-first-phase"></a>První fáze

Pro každou argumentů metody `Ei`:

*   Pokud `Ei` je anonymní funkce *odvození typu explicitní parametr* ([explicitní parametr typu závěry](expressions.md#explicit-parameter-type-inferences)) se provádí z `Ei` do `Ti`
*   Jinak, pokud `Ei` má typ `U` a `xi` je hodnota parametru o *dolní mez odvození* tvoří *z* `U` *k* `Ti`.
*   Jinak, pokud `Ei` má typ `U` a `xi` je `ref` nebo `out` parametr pak *přesná odvození* tvoří *z* `U` *k* `Ti`.
*   V opačném případě se provádí bez odvození pro tento argument.


#### <a name="the-second-phase"></a>Druhá fáze

Druhá fáze probíhá následujícím způsobem:

*   Všechny *nevyřešené* typ proměnné `Xi` které nikoli *závisí na* ([závislost](expressions.md#dependence)) jakékoli `Xj` jsou pevné ([řešení](expressions.md#fixing)).
*   Pokud neexistuje žádný takový typ proměnné, všechny *nevyřešené* typ proměnné `Xi` jsou *oprava* pro které všechny tyto uchování:
    *   Existuje alespoň jednu proměnnou typu `Xj` , který závisí na `Xi`
    *   `Xi` má jiné prázdnou sadu mezí
*   Pokud neexistuje žádný takový typ proměnné a stále dochází k *nevyřešené* typ proměnné, typu odvození selže.
*   Jinak, pokud žádné další *nevyřešené* proměnné typu neexistuje, proběhne úspěšně odvození typu proměnné.
*   Jinak pro všechny argumenty `Ei` s odpovídající typ parametru `Ti` kde *výstupní typy* ([výstupní typy](expressions.md#output-types)) obsahují *nevyřešené* Zadejte proměnné `Xj` ale *typů vstupu* ([typů vstupu](expressions.md#input-types)) nepodporují, *výstup odvození typu* ([závěry typ výstupu ](expressions.md#output-type-inferences)) se provádí *z* `Ei` *k* `Ti`. Druhá fáze se pak opakuje.

#### <a name="input-types"></a>Vstupní typy.

Pokud `E` je skupinu metod nebo implicitně typované anonymní funkce a `T` je delegát typu nebo typu stromu výrazu a všechny typy parametrů `T` jsou *typů vstupu* z `E` *s typem* `T`.

####  <a name="output-types"></a>Výstupní typy

Pokud `E` skupinu metod nebo anonymní funkce a `T` typu nebo typu stromu výrazu pak návratový typ je delegát `T` je *výstupní typ* `E` *s typem*  `T`.

#### <a name="dependence"></a>Závislost

*Nevyřešené* proměnná typu `Xi` *závisí přímo na* proměnnou typu laťky `Xj` po dobu některé argument `Ek` s typem `Tk` `Xj` Vyvolá se v *typ vstupu* z `Ek` s typem `Tk` a `Xi` probíhá *typ výstupu* z `Ek` s typem `Tk`.

`Xj` *závisí na* `Xi` Pokud `Xj` *závisí přímo na* `Xi` nebo, pokud `Xi` *závisí přímo na* `Xk` a `Xk` *závisí na* `Xj`. Tedy "závisí na" přechodné, ale ne reflexivní uzavření "závisí přímo na".

#### <a name="output-type-inferences"></a>Výstupní typ závěry

*Výstup odvození typu* tvoří *z* výraz `E` *k* typu `T` následujícím způsobem:

*  Pokud `E` je anonymní funkce s odvozený návratový typ `U` ([Inferred návratový typ](expressions.md#inferred-return-type)) a `T` není typ delegáta nebo typu stromu výrazu s návratovým typem `Tb`, pak *dolní mez odvození* ([dolní mez závěry](expressions.md#lower-bound-inferences)) se provádí *z* `U` *k* `Tb`.
*  Jinak, pokud `E` je skupinu metod a `T` není typ delegáta nebo typu stromu výrazu se typy parametrů `T1...Tk` a návratový typ `Tb`a rozlišení přetížení `E` s typy `T1...Tk` dává Jediná metoda s návratovým typem `U`, o *dolní mez odvození* tvoří *z* `U` *k* `Tb`.
*  Jinak, pokud `E` je výraz s typem `U`, o *dolní mez odvození* tvoří *z* `U` *k* `T`.
*  V opačném případě nebudou provedeny žádné závěry.

#### <a name="explicit-parameter-type-inferences"></a>Explicitní parametr typu závěry

*Odvození typu explicitní parametr* tvoří *z* výraz `E` *k* typu `T` následujícím způsobem:

*  Pokud `E` je explicitně anonymní funkce se typy parametrů `U1...Uk` a `T` není typ delegáta nebo typu stromu výrazu se typy parametrů `V1...Vk` pak pro každou `Ui` *přesné odvození* ([přesná závěry](expressions.md#exact-inferences)) se provádí *z* `Ui` *k* odpovídající `Vi`.

#### <a name="exact-inferences"></a>Přesné závěry

*Přesná odvození* *z* typu `U` *k* typu `V` se provádí následujícím způsobem:

*  Pokud `V` je jedním z *nevyřešené* `Xi` pak `U` je přidána do sady přesné mezí pro `Xi`.

*  Jinak, nastaví `V1...Vk` a `U1...Uk` určuje kontrolu, pokud platí kterákoli z následujících případech:

   *  `V` je typ pole `V1[...]` a `U` je typem pole `U1[...]` stejného řádu
   *  `V` je typ `V1?` a `U` je typ `U1?`
   *  `V` je konstruovaný typ `C<V1...Vk>`a `U` je konstruovaný typ. `C<U1...Uk>`

   Pokud pak platí kterákoli z těchto případů *přesná odvození* tvoří *z* každý `Ui` *k* odpovídající `Vi`.

*  V opačném případě nebudou provedeny žádné závěry.

#### <a name="lower-bound-inferences"></a>Dolní mez závěry

A *dolní mez odvození* *z* typu `U` *k* typu `V` se provádí následujícím způsobem:

*  Pokud `V` je jedním z *nevyřešené* `Xi` pak `U` je přidat do sady dolní meze `Xi`.
*  Jinak, pokud `V` je typ `V1?`a `U` je typ `U1?` pak dolní mez odvození se provádí z `U1` k `V1`.
*  Jinak, nastaví `U1...Uk` a `V1...Vk` určuje kontrolu, pokud platí kterákoli z následujících případech:
   *  `V` je typ pole `V1[...]` a `U` je typem pole `U1[...]` (nebo parametr typu, jehož efektivní základní typ je `U1[...]`) ze stejné pořadí
   *  `V` je jedním z `IEnumerable<V1>`, `ICollection<V1>` nebo `IList<V1>` a `U` je jednorozměrné pole typu `U1[]`(nebo parametr typu, jehož efektivní základní typ je `U1[]`)
   *  `V` je konstruovaný typ třídy, struktury, rozhraní nebo delegát `C<V1...Vk>` a je jedinečný typ `C<U1...Uk>` tak, aby `U` (nebo pokud `U` je parametr typu, její základní třída nebo členem její sada efektivní rozhraní) je stejné jako, dědí z (přímo nebo nepřímo), nebo implementuje (přímo nebo nepřímo) `C<U1...Uk>`.

      (Omezení "jedinečnost" znamená, že v případě rozhraní `C<T> {} class U: C<X>, C<Y> {}`, pak žádný odvození, provádí při odvozování z `U` k `C<T>` protože `U1` může být `X` nebo `Y`.)

   Pokud platí kterákoli z těchto případů, pak se provádí odvození *z* každý `Ui` *k* odpovídající `Vi` následujícím způsobem:

   *  Pokud `Ui` není známa jako typ odkazu a *přesná odvození* tvoří
   *  Jinak, pokud `U` je typem pole o *dolní mez odvození* provedení
   *  Jinak, pokud `V` je `C<V1...Vk>` pak odvození závisí na parametr typu i tý `C`:
      *  Pokud se jedná se o kovariantní o *dolní mez odvození* tvoří.
      *  Pokud je kontravariantní pak *horní mez odvození* tvoří.
      *  Pokud je invariantní pak *přesná odvození* tvoří.
*  V opačném případě nebudou provedeny žádné závěry.

#### <a name="upper-bound-inferences"></a>Horní mez závěry

*Horní mez odvození* *z* typu `U` *k* typu `V` se provádí následujícím způsobem:

*  Pokud `V` je jedním z *nevyřešené* `Xi` pak `U` je přidat do sady horní mez pro `Xi`.
*  Jinak, nastaví `V1...Vk` a `U1...Uk` určuje kontrolu, pokud platí kterákoli z následujících případech:
   *  `U` je typ pole `U1[...]` a `V` je typem pole `V1[...]` stejného řádu
   *  `U` je jedním z `IEnumerable<Ue>`, `ICollection<Ue>` nebo `IList<Ue>` a `V` je jednorozměrné pole typu `Ve[]`
   *  `U` je typ `U1?` a `V` je typ `V1?`
   *  `U` je vytvořený třídy, struktury, rozhraní nebo delegát typu `C<U1...Uk>` a `V` (přímo nebo nepřímo) je třída, struktura, rozhraní nebo delegát typu, který se shoduje s dědí z (přímo nebo nepřímo) nebo implementuje jedinečný typ `C<V1...Vk>`

      (Omezení "jedinečnost" znamená, že pokud budeme mít `interface C<T>{} class V<Z>: C<X<Z>>, C<Y<Z>>{}`, pak žádný odvození, provádí při odvozování z `C<U1>` k `V<Q>`. Závěry nejsou provedeny z `U1` buď `X<Q>` nebo `Y<Q>`.)

   Pokud platí kterákoli z těchto případů, pak se provádí odvození *z* každý `Ui` *k* odpovídající `Vi` následujícím způsobem:
   *  Pokud `Ui` není známa jako typ odkazu a *přesná odvození* tvoří
   *  Jinak, pokud `V` typ pole je pak *horní mez odvození* provedení
   *  Jinak, pokud `U` je `C<U1...Uk>` pak odvození závisí na parametr typu i tý `C`:
      *  Pokud se jedná se o kovariantní pak *horní mez odvození* tvoří.
      *  Pokud je kontravariantní o *dolní mez odvození* tvoří.
      *  Pokud je invariantní pak *přesná odvození* tvoří.
*  V opačném případě nebudou provedeny žádné závěry.   

#### <a name="fixing"></a>Oprava

*Nevyřešené* proměnná typu `Xi` sadu hranice je *oprava* následujícím způsobem:

*  Sada *Release candidate typy* `Uj` začíná jako sadu v sadě hranice pro všechny typy `Xi`.
*  Následně prozkoumáme každý mez pro `Xi` pak: Pro každý přesné mez `U` z `Xi` všechny typy `Uj` které nejsou identické s `U` se odeberou ze sady Release candidate. Pro každý dolní mez `U` z `Xi` všechny typy `Uj` do nichž je *není* implicitní převod z `U` se odeberou ze sady Release candidate. Pro každý horní mez `U` z `Xi` všechny typy `Uj` z nichž je *není* implicitní převod na `U` se odeberou ze sady Release candidate.
*  Pokud mezi zbývající typy Release candidate `Uj` je jedinečný typ `V` ze které je implicitní převod na všech ostatních Release candidate typů, pak `Xi` je pevně `V`.
*  V opačném případě odvození typu nezdaří.

#### <a name="inferred-return-type"></a>Odvozený návratový typ.

Odvozený typ vrácené hodnoty anonymní funkci `F` se používá při překladu typu odvození a přetížení. Odvozený návratový typ lze určit pouze pro anonymní funkce, kde všech parametrů, které typy jsou označovány buď protože jsou výslovně uvedené, zajišťována konverzi anonymní funkce nebo vyvozen při odvození typu na nadřazeném obecné volání metody.

***Odvodit typ výsledku*** je stanoven následujícím způsobem:

*  Pokud tělo `F` je *výraz* , která má typ, pak typ odvozený výsledku `F` je typ tohoto výrazu.
*  Pokud tělo `F` je *bloku* a sada výrazů v bloku `return` příkazů má doporučené společný typ `T` ([hledání nejlepší společný typ sada výrazů](expressions.md#finding-the-best-common-type-of-a-set-of-expressions)), pak odvozený typ výsledku `F` je `T`.
*  V opačném případě nejde odvodit typ výsledku pro `F`.

***Odvodit návratový typ*** je stanoven následujícím způsobem:

*  Pokud `F` je asynchronní a tělo `F` je výrazem jsou klasifikovány jako nic ([výrazu klasifikace](expressions.md#expression-classifications)), nebo blok příkazů, kde mají žádné návratové příkazy výrazů, odvozený návratový typ je `System.Threading.Tasks.Task`
*  Pokud `F` je asynchronní a má typ odvozené výsledek `T`, odvozený návratový typ je `System.Threading.Tasks.Task<T>`.
*  Pokud `F` je jiné asynchronní a má typ odvozené výsledek `T`, odvozený návratový typ je `T`.
*  V opačném případě nejde odvodit návratový typ pro `F`.

Jako příklad zahrnující anonymní funkce odvození typu, zvažte `Select` metodu rozšíření deklarovat v `System.Linq.Enumerable` třídy:
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

Za předpokladu, že `System.Linq` obor názvů byla importována pomocí `using` klauzule a zadané třídy `Customer` s `Name` vlastnost typu `string`, `Select` metodu je možné vybrat názvy seznam zákazníků:
```csharp
List<Customer> customers = GetCustomerList();
IEnumerable<string> names = customers.Select(c => c.Name);
```

Volání metody rozšíření ([volání metod rozšíření](expressions.md#extension-method-invocations)) z `Select` zpracovává přepis volání do volání statické metody:
```csharp
IEnumerable<string> names = Enumerable.Select(customers, c => c.Name);
```

Vzhledem k tomu, že argumenty typu nejsou explicitně zadané, odvození typu je použít k odvození argumenty typu. Nejprve je potřeba `customers` argument má vztah k `source` parametr odvození `T` bude `Customer`. Potom pomocí anonymní funkce zadejte procesu odvození popsané výš, `c` je daný typ `Customer`a výraz `c.Name` souvisí s návratový typ `selector` parametr, odvození `S` bude `string`. Proto je ekvivalentní k vyvolání
```csharp
Sequence.Select<Customer,string>(customers, (Customer c) => c.Name)
```
a výsledek je typu `IEnumerable<string>`.

Následující příklad ukazuje, jak anonymní typ funkce odvození umožňuje informace o typu "tok" mezi argumenty ve volání obecné metody. Zadané metody:
```csharp
static Z F<X,Y,Z>(X value, Func<X,Y> f1, Func<Y,Z> f2) {
    return f2(f1(value));
}
```

Odvození typu připouštějícího pro vyvolání:
```csharp
double seconds = F("1:15:30", s => TimeSpan.Parse(s), t => t.TotalSeconds);
```
Probíhá následujícím způsobem: První, argument `"1:15:30"` souvisí s `value` parametr odvození `X` bude `string`. Potom, parametr první anonymní funkce `s`, dostane odvozený typ `string`a výraz `TimeSpan.Parse(s)` souvisí s návratovým typem `f1`, odvození `Y` bude `System.TimeSpan`. Nakonec parametr druhý anonymní funkce `t`, dostane odvozený typ `System.TimeSpan`a výraz `t.TotalSeconds` souvisí s návratovým typem `f2`, odvození `Z` bude `double`. Proto je výsledek volání typu `double`.

#### <a name="type-inference-for-conversion-of-method-groups"></a>Odvození typu pro převod skupin – metoda

Podobně jako u volání obecné metody se odvození typu musí také použít při skupinu metod `M` obsahující obecnou metodu je převeden na delegáta daného typu `D` ([Metoda skupiny převody](conversions.md#method-group-conversions)). Zadané metody
```csharp
Tr M<X1...Xn>(T1 x1 ... Tm xm)
```
a skupina metod `M` přiřazením na typ delegáta `D` úlohou odvození typu je najít argumenty typu `S1...Sn` tak, aby výraz:
```csharp
M<S1...Sn>
```
stane se kompatibilní ([delegovat deklarace](delegates.md#delegate-declarations)) s `D`.

Na rozdíl od algoritmus odvození typu pro volání obecné metody, v tomto případě existují pouze argument *typy*, žádný argument *výrazy*. Zejména, neexistují žádné anonymní funkce a proto není nutné více fázích odvození.

Místo toho všechny `Xi` jsou považovány za *nevyřešené*a *dolní mez odvození* tvoří *z* každému typu argumentu `Uj` z `D` *k* odpovídající typ parametru `Tj` z `M`. Pokud pro některou `Xi` nebyly nalezeny žádné hranice, typ odvození. V opačném případě všechny `Xi` jsou *oprava* odpovídající `Si`, které jsou výsledkem odvození typu proměnné.

#### <a name="finding-the-best-common-type-of-a-set-of-expressions"></a>Vyhledání nejlepších společný typ sada výrazů

V některých případech se musí odvodit pro sadu výrazů stejného typu. Zejména typy elementů implicitně typovaná pole a návratové typy anonymní funkce s *bloku* úřadů, které se nacházejí v tímto způsobem.

Intuitivně, vzhledem k sadě výrazů `E1...Em` tento odvození by měl být ekvivalentní volání metody
```csharp
Tr M<X>(X x1 ... X xm)
```
s `Ei` jako argumenty.

Přesněji řečeno, odvození začíná *nevyřešené* proměnná typu `X`. *Výstupní typ závěry* pak vyvinou *z* každý `Ei` *k* `X`. Nakonec `X` je *oprava* a pokud je úspěšná, výsledný typ `S` je Výsledný nejlepší společný typ pro výraz. Pokud neexistuje žádný takový `S` existuje, výrazy mají žádné doporučené společný typ.

### <a name="overload-resolution"></a>Rozlišení přetěžování

Řešení přetížení se doba vazby mechanismus pro výběr nejlepší funkce člena k vyvolání zadaný seznam argumentů a sadu Release candidate funkce členů. Řešení přetížení vybere členské funkce k vyvolání v následujících různých kontextech v rámci jazyka C#:

*  Vyvolání metody s názvem v *invocation_expression* ([volání metod](expressions.md#method-invocations)).
*  Vyvolání konstruktoru instance s názvem v *object_creation_expression* ([výrazy vytvoření objektu](expressions.md#object-creation-expressions)).
*  Vyvolání indexeru přistupující objekt prostřednictvím *element_access* ([přístup k prvkům](expressions.md#element-access)).
*  Vyvolání předdefinovaných nebo uživatelem definovaný operátor odkazuje ve výrazu ([unární operátor rozlišení přetěžování](expressions.md#unary-operator-overload-resolution) a [binárním operátorem rozlišení přetěžování](expressions.md#binary-operator-overload-resolution)).

Každá z těchto kontextech definuje sadu Release candidate funkce členů a seznamu argumentů svůj vlastní jedinečný způsobem, jak je podrobně popsán v části výše uvedených. Například sadu kandidáty pro volání metody nezahrnuje metody označené `override` ([člen vyhledávání](expressions.md#member-lookup)), a metody základní třídy nejsou kandidáty pokud platí všechny metody v odvozené třídě ([ Volání metod](expressions.md#method-invocations)).

Jakmile se zjistily členy funkce Release candidate a seznam argumentů, výběr nejlepší členské funkce je stejná ve všech případech:

*  Zadaný sadu členů funkce použít Release candidate, nejlepší funkce člena v tom, že sada se nachází. Pokud sada obsahuje pouze jeden člen funkce, je daný člen funkce nejlepší členské funkce. V opačném případě je nejlepší členské funkce člena jednu funkci, která je obecně lepší než všechny ostatní funkce členy s ohledem na zadaného seznamu argumentů, za předpokladu, že každý člen funkce je ve srovnání s všichni členové funkce pomocí pravidel v [ Lepší členské funkce](expressions.md#better-function-member). Pokud existuje přesně jeden člen funkce, která je obecně lepší než všechny ostatní funkce členové, pak je nejednoznačné volání funkce člena a dojde k chybě vazby čas.

V dalších částech definovat přesně význam podmínky ***použitelná funkce člena*** a ***lepší členské funkce***.

#### <a name="applicable-function-member"></a>Člen příslušné funkce

Členské funkce se říká, že ***použitelná funkce člena*** s ohledem na seznam argumentů `A` Pokud jsou splněny všechny z následujících akcí:

*  Každý argument v `A` odpovídá parametru v deklaraci členské funkce, jak je popsáno v [odpovídající parametry](expressions.md#corresponding-parameters), a žádné parametry, které odpovídá žádný argument je volitelný parametr.
*  Pro každý argument v `A`, předávání režimu argumentu parametrů (například, hodnota `ref`, nebo `out`) je stejný jako režim parametru předávání příslušného parametru a
   *  pro parametr hodnoty nebo pole parametrů, implicitní převod ([implicitních převodů](conversions.md#implicit-conversions)) existuje v argumentu na typ odpovídající parametr nebo
   *  pro `ref` nebo `out` parametr, typ argumentu je stejné jako odpovídající parametr typu. Po všech `ref` nebo `out` parametr je alias pro předaného argumentu.

Funkce člena, která zahrnuje parametr pole Pokud je člen funkce použít podle výše uvedených pravidel, se říká fungovaly v jeho ***normalizačním formuláři***. Pokud člen funkce, která zahrnuje parametr pole se nedá použít v podobě normální, členské funkce může být místo toho použít v jeho ***rozšířit formuláře***:

*  Rozšířené formuláře je vytvořený tak, že nahradíte pole parametrů v deklaraci členské funkce s hodnotou nula nebo více parametrů hodnotu parametru typu prvku pole, tento počet argumentů v seznamu argumentů `A` odpovídá celkové počet parametrů. Pokud `A` má menší počet argumentů, než je počet parametrů pevné v deklaraci členské funkce, rozšířené formu členské funkce nelze zkonstruovat a se tedy nedá použít.
*  V opačném případě se vztahuje Pokud pro každý argument v rozšířené formuláře `A` režim předávání parametru argumentu je stejný jako režim parametru předávání příslušného parametru a
   *  pro parametr pevná hodnota nebo hodnota parametru vytvořené rozšíření implicitní převod ([implicitních převodů](conversions.md#implicit-conversions)) existuje z typu argumentu na typ odpovídající parametr nebo
   *  pro `ref` nebo `out` parametr, typ argumentu je stejné jako odpovídající parametr typu.

#### <a name="better-function-member"></a>Lepší členské funkce

Pro účely stanovení lepší členské funkce je vytvořen seznam stripped-down argumentů A obsahující pouze výrazy argument sami v pořadí, ve kterém jsou uvedeny v původní seznam argumentů.

Parametr uvádí pro jednotlivé funkce Release candidate, do které jsou členy vytvořeny následujícím způsobem:

*  Rozšířené forma se používá, pokud bylo použitelné pouze ve formuláři Rozšířené členské funkce.
*  Volitelné parametry bez odpovídající argumentů se odeberou ze seznamu parametrů
*  Parametry jsou přeuspořádají, aby k nim dojde na stejné pozici jako příslušný argument v seznamu argumentů.

Zadaný seznam argumentů `A` sadu výrazy argumentu `{E1, E2, ..., En}` a dva členy příslušné funkce `Mp` a `Mq` se typy parametrů `{P1, P2, ..., Pn}` a `{Q1, Q2, ..., Qn}`, `Mp` je definován jako ***lepší členské funkce*** než `Mq` Pokud

*  pro každý argument implicitní převod z `Ex` k `Qx` není lepší než implicitní převod z `Ex` k `Px`, a
*  pro nejméně jeden argument, převod z `Ex` k `Px` je obecně lepší než převod z `Ex` k `Qx`.

Při provádění vyhodnocení, pokud `Mp` nebo `Mq` platí v podobě rozšířené, pak `Px` nebo `Qx` odkazuje na parametr ve formuláři rozbaleného seznamu parametrů.

V případě, že parametr typu pořadí `{P1, P2, ..., Pn}` a `{Q1, Q2, ..., Qn}` jsou ekvivalentní (to znamená každou `Pi` má konverzi identity do odpovídajících `Qi`), se použijí následující pravidla shody, v pořadí, chcete-li určit, tím lepší Členské funkce.

*  Pokud `Mp` je neobecnou metodu a `Mq` je obecná metoda, pak `Mp` je obecně lepší než `Mq`.
*  Jinak, pokud `Mp` lze použít v podobě normální a `Mq` má `params` pole a platí jenom v podobě rozšířené, pak `Mp` je obecně lepší než `Mq`.
*  Jinak, pokud `Mp` prohlásil více parametrů než `Mq`, pak `Mp` je obecně lepší než `Mq`. Tato situace může nastat, pokud obě metody mají `params` pole a jsou použitelné jenom v rozšířené formuláře.
*  Jinak pokud všechny parametry `Mp` mít odpovídající argument, že výchozí argumenty nahrazené za nejméně jeden volitelný parametr `Mq` pak `Mp` je obecně lepší než `Mq`.
*  Jinak, pokud `Mp` má zvláštní typy parametrů než `Mq`, pak `Mp` je obecně lepší než `Mq`. Umožní `{R1, R2, ..., Rn}` a `{S1, S2, ..., Sn}` představují typů bez instancí a nerozbalený parametrů `Mp` a `Mq`. `Mp`na typy parametrů jsou podrobnější než `Mq`Pokud pro každý parametr `Rx` není méně specifické než `Sx`a pro nejméně jeden parametr, `Rx` je konkrétnější než `Sx`:
   *  Parametr typu je méně specifické než beztypový parametr.
   *  Rekurzivně, konstruovaný typ je konkrétnější než jiné konstruovaný typ (se stejným číslem argumenty typu), pokud alespoň jeden argument typu je konkrétnější a žádný argument typu je méně specifické než argument typu v jiném.
   *  Typ pole je konkrétnější než jiný typ pole (s stejný počet rozměrů), pokud je typ prvku prvního konkrétnější než druhý typ elementu.
*  Jinak Pokud jeden člen je operátor zrušeno a druhý je operátor zdvižené,-zrušeno ten je lepší.
*  V opačném případě je lepší ani členské funkce.

#### <a name="better-conversion-from-expression"></a>Lepší převod z výrazu

Zadaný implicitní převod `C1` , která převede z výrazu `E` na typ `T1`a implicitní převod `C2` , která převede z výrazu `E` na typ `T2`, `C1` je ***lepší převod*** než `C2` Pokud `E` přesně neodpovídá `T2` a obsahuje alespoň jeden z následujících akcí:

* `E` přesně odpovídá `T1` ([přesně odpovídající výraz](expressions.md#exactly-matching-expression))
* `T1` je lepší cílem převod než `T2` ([lepší převod cílové](expressions.md#better-conversion-target))

#### <a name="exactly-matching-expression"></a>Přesně odpovídající výraz

Zadaný výraz `E` a typ `T`, `E` přesně odpovídá `T` pokud obsahuje jeden z následujících akcí:

*  `E` má typ `S`, a převod identity `S` do `T`
*  `E` je anonymní funkce `T` je typ delegátu `D` nebo typu stromu výrazu `Expression<D>` a obsahuje jeden z následujících akcí:
   *  Odvozený návratový typ `X` existuje pro `E` v rámci seznamu parametrů `D` ([Inferred návratový typ](expressions.md#inferred-return-type)), a převod identity `X` na návratový typ `D`
   *  Buď `E` je jiné asynchronní a `D` má návratový typ `Y` nebo `E` je asynchronní a `D` má návratový typ `Task<Y>`, a obsahuje jeden z následujících akcí:
      * Tělo `E` je výraz, který přesně odpovídá `Y`
      * Tělo `E` je blok příkazů, kde každý návratový příkaz vrátí výraz, který přesně odpovídá `Y`

#### <a name="better-conversion-target"></a>Lepší převod cíl

Uvedené dva různé typy `T1` a `T2`, `T1` je cílem převod lepší než `T2` Pokud žádný implicitní převod z `T2` k `T1` existuje, a obsahuje alespoň jeden z následujících akcí:

*  Implicitní převod z `T1` k `T2` existuje
*  `T1` je typ delegátu `D1` nebo typu stromu výrazu `Expression<D1>`, `T2` je typ delegátu `D2` nebo typu stromu výrazu `Expression<D2>`, `D1` má návratový typ `S1` a jeden z obsahuje následující:
   * `D2` vrací typ void
   * `D2` má návratový typ `S2`, a `S1` je cílem převod lepší než `S2`
*  `T1` je `Task<S1>`, `T2` je `Task<S2>`, a `S1` je cílem převod lepší než `S2`
*  `T1` je `S1` nebo `S1?` kde `S1` je celočíselný typ se znaménkem, a `T2` je `S2` nebo `S2?` kde `S2` je celočíselný typ bez znaménka. Konkrétně:
   * `S1` je `sbyte` a `S2` je `byte`, `ushort`, `uint`, nebo `ulong`
   * `S1` je `short` a `S2` je `ushort`, `uint`, nebo `ulong`
   * `S1` je `int` a `S2` je `uint`, nebo `ulong`
   * `S1` je `long` a `S2` je `ulong`

#### <a name="overloading-in-generic-classes"></a>Přetížení v obecné třídy

Podpisy, jak je deklarován musí být jedinečný, je možné, že nahrazení argumentů typu výsledkem identickými podpisy. V takových případech pravidla shody výše uvedené řešení přetížení vybere nejspecifičtější člena.

Následující příklady ukazují přetížení, které jsou platné a neplatné podle tohoto pravidla:

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

### <a name="compile-time-checking-of-dynamic-overload-resolution"></a>Kompilace kontrolu dynamické přetížení

Pro operace s nejvíce dynamicky vazbou sadu možné kandidáty pro rozlišení neznámé v době kompilace. V některých případech ale sadu Release candidate je známý v době kompilace:

*  Volání statické metody s dynamických argumentů
*  Pokud příjemce není výraz dynamického volání metod instance
*  Pokud příjemce není výraz dynamického volání indexeru
*  Volání konstruktoru pomocí dynamických argumentů

V těchto případech se provádí omezené kontrola v době kompilace pro každého kandidáta zobrazíte, pokud některý z nich může být použít v době běhu. Tato kontrola se skládá z následujících kroků:

*  Odvození částečného typu: Jakýkoli typ argumentu, který přímo nebo nepřímo není závislý na argumentu typu `dynamic` odvozena pomocí pravidel pro [odvození typu](expressions.md#type-inference). Zbývající argumenty typu nejsou známé.
*  Kontrola použitelnosti částečné: Použitelnost proběhne podle [použít funkční člen](expressions.md#applicable-function-member), ale ignoruje parametry, jejichž typy neznámé.
*  Pokud žádný kandidát předá tento test, dojde k chybě v době kompilace.

### <a name="function-member-invocation"></a>Volání funkce člena

Tato část popisuje proces, který se provádí v době běhu k vyvolání konkrétní funkce člena. Předpokládá se, že proces doba vazby již určil konkrétního členu, který chcete vyvolat, případně s použitím rozlišení přetěžování na sadu Release candidate funkční členy.

Pro účely popisu vyvolání procesu jsou členy funkce rozdělit do dvou kategorií:

*  Funkce statických členů. Toto jsou konstruktory instancí, statické metody, přistupující objekty statické vlastnosti a operátory definované uživatelem. Funkce statických členů jsou vždy nevirtuální.
*  Členy instance funkce. Toto jsou metody instance, přistupující objekty vlastnosti instance a přístupové objekty indexeru. Členy instance funkce jsou nevirtuální nebo virtuální a jsou vždy vyvolána na konkrétní instanci. Instanci se počítají tak, že výraz instance a bude dostupný v rámci členských funkcí jako `this` ([tento přístup](expressions.md#this-access)).

Zpracování za běhu volání členských funkcí se skládá z následujících kroků, kde `M` je členské funkce a v případě `M` je členem instance `E` je výraz instance:

*  Pokud `M` členem statické funkce:
   * Seznam argumentů je vyhodnocen, jak je popsáno v [seznamy argumentů](expressions.md#argument-lists).
   * `M` je vyvolána.

*  Pokud `M` je členem instance funkce deklarované v *value_type*:
   * `E` je vyhodnocen. Je-li toto vyhodnocení způsobí výjimku, jsou spuštěny žádné další kroky.
   * Pokud `E` není jsou klasifikovány jako proměnnou a dočasný místní proměnná `E`typ je vytvořen a hodnota `E` je přiřazená k této proměnné. `E` je pak překlasifikován sám jako odkaz na tento dočasný místní proměnné. Dočasná proměnná je dostupné jako `this` v rámci `M`, ale ne v žádným jiným způsobem. Proto pouze tehdy, když `E` je true proměnné je možné pro volajícího, aby sledovat změny, které `M` provede `this`.
   * Seznam argumentů je vyhodnocen, jak je popsáno v [seznamy argumentů](expressions.md#argument-lists).
   * `M` je vyvolána. Proměnná odkazuje `E` stane proměnná odkazuje `this`.

*  Pokud `M` je členem instance funkce deklarované v *reference_type*:
   * `E` je vyhodnocen. Je-li toto vyhodnocení způsobí výjimku, jsou spuštěny žádné další kroky.
   * Seznam argumentů je vyhodnocen, jak je popsáno v [seznamy argumentů](expressions.md#argument-lists).
   * Pokud typ `E` je *value_type*, převod na uzavřené určení ([zabalení převody](types.md#boxing-conversions)) se provádí za účelem převodu `E` na typ `object`, a `E` se považuje za Chcete-li být typu `object` v následujících krocích. V takovém případě `M` může být jenom členem `System.Object`.
   * Hodnota `E` je zaškrtnuté políčko platný. Pokud hodnota `E` je `null`, `System.NullReferenceException` je vyvolána, a že jsou provedeny žádné další kroky.
   * Implementace členské funkce k vyvolání se určuje:
     * Pokud je typ vazby time `E` je rozhraní, členské funkce k vyvolání je implementace `M` poskytuje run-time typu instance odkazované procedurou `E`. Tento člen funkce je určeno použitím z pravidel mapování rozhraní ([mapování rozhraní](interfaces.md#interface-mapping)) k určení provádění `M` poskytuje run-time typu instance odkazované procedurou `E`.
     * Jinak, pokud `M` členem virtuální funkce, je člen funkce k vyvolání provádění `M` poskytuje run-time typu instance odkazované procedurou `E`. Tento člen funkce je určeno použitím z pravidel pro určení implementace Většina odvozených ([virtuální metody](classes.md#virtual-methods)) z `M` s ohledem na run-time typu instance odkazované procedurou `E`.
     * V opačném případě `M` je člen bez virtuální funkce a členské funkce k vyvolání `M` samotný.
   * Implementace členské funkce nadefinovali v předchozím kroku, je vyvolána. Objekt odkazovaný zadaným parametrem `E` stane objekt odkazovaný zadaným parametrem `this`.

#### <a name="invocations-on-boxed-instances"></a>Volání na zabalený instancí

Členské funkce implementované v *value_type* lze vyvolat pomocí zabalený instanci, která *value_type* v následujících situacích:

*  Kdy je člen funkce `override` metody zděděné z typu `object` a je vyvolána prostřednictvím výrazu instance typu `object`.
*  Když členské funkce je implementace funkce člena rozhraní a je vyvolána prostřednictvím výrazu instance *interface_type*.
*  Členské funkce je při vyvolání prostřednictvím delegáta.

V těchto situacích pevně určené instance se považuje za obsahovat proměnnou *value_type*, a že tato proměnná bude proměnná odkazuje `this` v rámci volání členské funkce. Konkrétně to znamená, že při vyvolání funkce člena pevně určené instance, je možné pro členské funkce ke změně hodnoty obsažené v pevně určené instance.

## <a name="primary-expressions"></a>Primární výrazy

Primární výrazy zahrnují nejjednodušší typy výrazů.

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

Primární výrazy jsou rozděleny mezi *array_creation_expression*s a *primary_no_array_creation_expression*s. Výraz vytvoření pole se zpracuje tímto způsobem, místo výpis společně s další jednoduchý výraz formuláře, umožňuje gramatiky tak potenciálně matoucí kódu, jako
```csharp
object o = new int[3][1];
```
které by jinak interpretován jako
```csharp
object o = (new int[3])[1];
```

### <a name="literals"></a>Literály

A *primary_expression* , který se skládá z *literálu* ([literály](lexical-structure.md#literals)) je klasifikován jako hodnotu.


### <a name="interpolated-strings"></a>Interpolované řetězce

*Interpolated_string_expression* se skládá z `$` znaménka následovaného pravidelného či verbatim řetězcový literál, ve které otvory odděleny `{` a `}`, uzavřete výrazy a formátování specifikace. Interpolovaný řetězcový výraz ale je výsledkem *interpolated_string_literal* , které bylo přerušeno na jednotlivé tokeny, jak je popsáno v [interpolované řetězce literálů](lexical-structure.md#interpolated-string-literals).

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

*Constant_expression* interpolaci musí mít implicitní převod na `int`.

*Interpolated_string_expression* klasifikovaný jako hodnotu. Pokud je okamžitě převést na `System.IFormattable` nebo `System.FormattableString` s konverzi implicitní interpolovaném řetězci ([implicitní interpolované řetězce převody](conversions.md#implicit-interpolated-string-conversions)), má interpolovaný řetězcový výraz tohoto typu. V opačném případě má typ `string`.

Pokud je typem interpolovaného řetězce `System.IFormattable` nebo `System.FormattableString`, význam je volání `System.Runtime.CompilerServices.FormattableStringFactory.Create`. Pokud je typ `string`, význam výrazu je volání `string.Format`. V obou případech se seznamem argumentů volání se skládá z formátu řetězcový literál s zástupné symboly pro každý interpolace a argument pro každý výraz odpovídá místo zástupné znaky.

Formát řetězcového literálu se vypočte takto, kde `N` je počet interpolace v *interpolated_string_expression*:

*  Pokud *interpolated_regular_string_whole* nebo *interpolated_verbatim_string_whole* následuje `$` podepsat, pak tento token je literál řetězce formátu.
*  Formát řetězcového literálu, jinak se skládá z: 
   *  První *interpolated_regular_string_start* nebo *interpolated_verbatim_string_start*
   *  Pak pro každé číslo `I` z `0` k `N-1`: 
      * Desítkový zápis `I`
      * Když se poté odpovídající *interpolace* má *constant_expression*, `,` (čárka), za nímž následuje Desítková vyjádření hodnoty *constant_expression*
      * Pak bude *interpolated_regular_string_mid*, *interpolated_regular_string_end*, *interpolated_verbatim_string_mid* nebo *interpolated_ verbatim_string_end* hned za odpovídající interpolace.

Další argumenty jsou jednoduše *výrazy* z *interpolace* (pokud existuje), v pořadí.

TODO: příklady.


### <a name="simple-names"></a>Jednoduché názvy

A *simple_name* identifikátor, může volitelně následovat seznam argumentů typu se skládá ze:

```antlr
simple_name
    : identifier type_argument_list?
    ;
```

A *simple_name* je buď ve formátu `I` nebo formuláře `I<A1,...,Ak>`, kde `I` je jediným identifikátorem a `<A1,...,Ak>` je volitelný *type_argument_list*. Pokud ne *type_argument_list* je zadán, vezměte v úvahu `K` být nula. *Simple_name* je vyhodnocen a klasifikovat následujícím způsobem:

*  Pokud `K` je nula a *simple_name* se zobrazí v rámci *bloku* a, pokud *bloku*společnosti (nebo nadřazeném *bloku*společnosti) místní Deklarace proměnné místa ([deklarace](basic-concepts.md#declarations)) obsahuje místní proměnná, parametr nebo konstanta s názvem `I`, pak bude *simple_name* odkazuje na tuto místní proměnnou parametr nebo konstantu a je klasifikován jako na proměnnou nebo hodnotu.
*  Pokud `K` je nula a *simple_name* se zobrazí v těle deklarace obecné metody a pokud tato deklarace obsahuje parametr typu s názvem `I`, pak bude *simple_name*odkazuje na parametr typu.
*  Jinak pro každý typ instance `T` ([typ instance](classes.md#the-instance-type)), počínaje instance typu bezprostředně vložená deklarace typu a budete pokračovat s typem instance každé nadřazené třídu nebo strukturu deklarace (pokud existuje):
   *  Pokud `K` je nula a deklarace `T` obsahuje parametr typu s názvem `I`, pak bude *simple_name* odkazuje na parametr typu.
   *  Jinak, pokud člen vyhledávání ([člen vyhledávání](expressions.md#member-lookup)) z `I` v `T` s `K`  argumenty typu vytváří shoda:
      * Pokud `T` je typ instance bezprostředně nadřazeného typu třídy nebo struktury a vyhledávání označuje jeden nebo více metod, výsledek je skupina metoda s výrazem přidruženou instanci `this`. Pokud byl zadán seznam argumentů typu se používá při volání obecné metody ([volání metod](expressions.md#method-invocations)).
      * Jinak, pokud `T` je typ instance bezprostředně nadřazeného typu třídy nebo struktury, pokud vyhledávání identifikuje člena instance, a pokud odkaz nastane v těle konstruktoru instance, metoda instance nebo přístupový objekt instance výsledek je stejný jako přístup ke členu ([přístup ke členu](expressions.md#member-access)) ve formátu `this.I`. K tomu může dojít pouze při `K` je nula.
      * Jinak, výsledek je stejný jako přístup ke členu ([přístup ke členu](expressions.md#member-access)) ve formátu `T.I` nebo `T.I<A1,...,Ak>`. V takovém případě je chyba doba vazby pro *simple_name* k odkazování na člena instance.

*  Jinak pro každý obor názvů `N`začíná s oborem názvů, ve kterém *simple_name* dojde, budete pokračovat s každý nadřazený obor názvů (pokud existuje) a konče globální obor názvů následující kroky jsou vyhodnocen, dokud se entita nachází:
   *  Pokud `K` je nula a `I` je název oboru názvů v `N`, pak:
      * Pokud umístění ve kterém *simple_name* dojde k není uzavřen v deklaraci oboru názvů pro `N` a obsahuje deklaraci oboru názvů *extern_alias_directive* nebo  *using_alias_directive* , která přidruží název `I` s oborem názvů nebo typ, pak bude *simple_name* je nejednoznačný a dojde k chybě kompilace.
      * V opačném případě *simple_name* odkazuje na obor názvů s názvem `I` v `N`.
   *  Jinak, pokud `N` obsahuje přístupného typu s názvem `I` a `K`  parametry typu, pak:
      * Pokud `K` je nula a umístění, kde *simple_name* dojde k není uzavřen v deklaraci oboru názvů pro `N` a obsahuje deklaraci oboru názvů *extern_alias_directive*nebo *using_alias_directive* , která přidruží název `I` s oborem názvů nebo typ, pak bude *simple_name* je nejednoznačný a dojde k chybě kompilace.
      * V opačném případě *namespace_or_type_name* odkazuje na typ vytvořený s argumenty daného typu.
   *  Jinak, pokud umístění ve kterém *simple_name* dojde k není uzavřen v deklaraci oboru názvů pro `N`:
      * Pokud `K` je nula a obsahuje deklaraci oboru názvů *extern_alias_directive* nebo *using_alias_directive* , která přidruží název `I` s importovaným oborem názvů nebo typ, pak bude *simple_name* odkazuje na tento obor názvů nebo typ.
      * Jinak, pokud deklarace oborů názvů a typ importované tímto seznamem *using_namespace_directive*s a *using_static_directive*s deklarace oboru názvů obsahovat právě jeden typ přístupné nebo rozšíření statický člen s názvem `I` a `K`  parametry typu, pak bude *simple_name* odkazuje na tento typ nebo člen vytvořený s argumenty daného typu.
      * Jinak, pokud jsou obory názvů a typy importoval *using_namespace_directive*s deklarace oboru názvů obsahovat více než jeden dostupný typ nebo statický člen rozšiřující metoda s názvem `I` a `K`  parametry typu, pak bude *simple_name* je nejednoznačný a dojde k chybě.

   Mějte na paměti, že celý tento krok je přesně paralelní na odpovídající krok zpracování *namespace_or_type_name* ([Namespace a zadejte názvy](basic-concepts.md#namespace-and-type-names)).

*  V opačném případě *simple_name* je nedefinovaný a dojde k chybě kompilace.


### <a name="parenthesized-expressions"></a>Výrazy v závorkách.

A *parenthesized_expression* se skládá ze *výraz* uzavřený do závorek.

```antlr
parenthesized_expression
    : '(' expression ')'
    ;
```

A *parenthesized_expression* se vyhodnocuje na základě vyhodnocení *výraz* v závorkách. Pokud *výraz* v závorkách označuje obor názvů nebo typ, dojde k chybě kompilace. V opačném případě výsledek *parenthesized_expression* je výsledkem vyhodnocení uzavřeného *výraz*.

### <a name="member-access"></a>Přístup ke členům

A *member_access* se skládá z *primary_expression*, *predefined_type*, nebo *qualified_alias_member*následovaný "`.`"token a po něm *identifikátor*volitelně následovaný *type_argument_list*.

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

*Qualified_alias_member* produkčním prostředí je definována v [Namespace alias kvalifikátory](namespaces.md#namespace-alias-qualifiers).

A *member_access* je buď ve formátu `E.I` nebo formuláře `E.I<A1, ..., Ak>`, kde `E` je primární výraz, `I` je jediným identifikátorem a `<A1, ..., Ak>` je volitelný  *type_argument_list*. Pokud ne *type_argument_list* je zadán, vezměte v úvahu `K` být nula.

A *member_access* s *primary_expression* typu `dynamic` dynamicky vázán ([dynamické vazby](expressions.md#dynamic-binding)). V tomto případě kompilátor klasifikuje přístup ke členu jako přístup k vlastnosti typu `dynamic`. Pravidly dole určit význam *member_access* se pak použijí v době běhu, místo kompilaci typu run-time typu *primary_expression*. Pokud tato klasifikace za běhu vede na skupinu metod, pak musí být přístup ke členu *primary_expression* ze *invocation_expression*.

*Member_access* je vyhodnocen a klasifikovat následujícím způsobem:

*  Pokud `K` je nula a `E` je obor názvů a `E` obsahuje vnořené oboru názvů s názvem `I`, výsledek je tento obor názvů.
*  Jinak, pokud `E` je obor názvů a `E` obsahuje přístupného typu s názvem `I` a `K`  zadejte parametry, výsledkem je vytvořen s daným typem argumenty typu.
*  Pokud `E` je *predefined_type* nebo *primary_expression* klasifikovat jako typ, pokud `E` se nejedná o parametr typu a pokud člen vyhledávání ([člen vyhledávání](expressions.md#member-lookup)) z `I` v `E` s `K`  parametry typu vytváří shodu, pak `E.I` je vyhodnocen a klasifikovat následujícím způsobem:
   *  Pokud `I` identifikuje typ, výsledek je vytvořen s daným typem argumenty typu.
   *  Pokud `I` identifikuje jednu nebo více metod, výsledek je skupinu metod s žádný výraz přidruženou instanci. Pokud byl zadán seznam argumentů typu se používá při volání obecné metody ([volání metod](expressions.md#method-invocations)).
   *  Pokud `I` identifikuje `static` vlastnost a potom výsledek je přístup k vlastnosti s žádný výraz přidruženou instanci.
   *  Pokud `I` identifikuje `static` pole:
      * Pokud je datové pole `readonly` a odkaz proběhne mimo statický konstruktor třídy nebo struktury, ve kterém je deklarována pole a výsledkem je hodnota, konkrétně hodnota statické pole `I` v `E`.
      * Jinak, výsledek je proměnná, konkrétně statické pole `I` v `E`.
   *  Pokud `I` identifikuje `static` události:
      * Pokud dojde k odkazu v rámci třídy nebo struktury, ve kterém je deklarována jako události a události byla deklarovaná bez *event_accessor_declarations* ([události](classes.md#events)), pak `E.I` se právě zpracovává. jakoby `I` byly statické pole.
      * Jinak výsledkem je přístup k události pomocí žádný výraz přidruženou instanci.
   *  Pokud `I` identifikuje konstantě, a výsledkem je hodnota, konkrétně hodnota této konstanty.
    * Pokud `I` identifikuje na člena výčtu a výsledkem je hodnota, konkrétně hodnotu tohoto člena výčtu.
    * V opačném případě `E.I` je odkaz na člena je neplatný a dojde k chybě kompilace.
*  Pokud `E` přístup k vlastnostem, přístup indexeru, proměnné nebo hodnotu, jehož typ je `T`a člen vyhledávání ([člen vyhledávání](expressions.md#member-lookup)) z `I` v `T` s `K`  argumenty typu vytváří shodu, pak `E.I` je vyhodnocen a klasifikovat následujícím způsobem:
   *  První, pokud `E` vlastnost nebo indexer přístup, je hodnota vlastnosti nebo získat přístup indexeru ([hodnot výrazů](expressions.md#values-of-expressions)) a `E` přeřazených jako hodnotu.
   *  Pokud `I` identifikuje jednu nebo více metod, výsledek je skupinu metod s výrazem přidruženou instanci `E`. Pokud byl zadán seznam argumentů typu se používá při volání obecné metody ([volání metod](expressions.md#method-invocations)).
   *  Pokud `I` identifikuje vlastnost instance,
      * Pokud `E` je `this`, `I` identifikuje automaticky implementované vlastnosti ([automaticky implementované vlastnosti](classes.md#automatically-implemented-properties)) bez setter a odkaz nastane v rámci konstruktoru instance pro Typ třídy nebo struktury `T`, výsledek je proměnná, konkrétně pole Skrytá zálohování pro automatickou vlastnost Dal `I` v instanci `T` určené pomocí `this`.
      * Jinak, výsledkem je přístup k vlastnosti s výrazem přidruženou instanci `E`.
   *  Pokud `T` je *class_type* a `I` pole instance, která identifikuje *class_type*:
      * Pokud hodnota `E` je `null`, o `System.NullReferenceException` je vyvolána výjimka.
      * Jinak, pokud je datové pole `readonly` a odkaz proběhne mimo konstruktor instance třídy, ve kterém je deklarována pole a výsledkem je hodnota, konkrétně hodnota pole `I` v objekt odkazovaný zadaným parametrem `E`.
      * Jinak, výsledek je proměnná, konkrétně pole `I` v objekt odkazovaný zadaným parametrem `E`.
   *  Pokud `T` je *struct_type* a `I` pole instance, která identifikuje *struct_type*:
      * Pokud `E` je hodnota, nebo pokud je datové pole `readonly` a odkaz proběhne mimo konstruktor instance struktury, ve kterém je deklarována pole a výsledkem je hodnota, konkrétně hodnota pole `I` v dána instance – struktura  `E`.
      * Jinak, výsledek je proměnná, konkrétně pole `I` v instanci struktury Dal `E`.
   *  Pokud `I` identifikuje instanci události:
      * Pokud dojde k odkazu v rámci třídy nebo struktury, ve kterém je deklarována jako události a události byla deklarovaná bez *event_accessor_declarations* ([události](classes.md#events)), a nedojde k jako odkaz Levá strana příkazu `+=` nebo `-=` operátor, pak `E.I` se právě zpracovává jako `I` bylo pole instance.
      * Jinak, výsledkem je přístup k události s výrazem přidruženou instanci `E`.
*  V opačném případě je proveden pokus o zpracování `E.I` jako volání metody rozšíření ([volání metod rozšíření](expressions.md#extension-method-invocations)). Když se to nepovede, `E.I` je odkaz na člena je neplatný a dojde k chybě vazby čas.

#### <a name="identical-simple-names-and-type-names"></a>Stejný jednoduchý název a názvy typů

V přístupu ke členu formuláře `E.I`, pokud `E` je jediným identifikátorem a pokud význam `E` jako *simple_name* ([jednoduché názvy](expressions.md#simple-names)) je konstanta, pole, vlastnost, lokální proměnná ani parametr stejného typu jako význam `E` jako *type_name* ([Namespace a zadejte názvy](basic-concepts.md#namespace-and-type-names)), pak oba možných významů z `E` jsou povolené. Dva možné význam `E.I` nejsou nikdy nejednoznačný, protože `I` nutně musí být členem typu `E` v obou případech. Jinými slovy, pravidlo jednoduše umožňuje přístup na statické členy a vnořené typy `E` kde kompilace by jinak mají došlo k chybě. Příklad:
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

#### <a name="grammar-ambiguities"></a>Gramatika nejednoznačnosti

Výroby pro *simple_name* ([jednoduché názvy](expressions.md#simple-names)) a *member_access* ([přístup ke členu](expressions.md#member-access)) může mít za následek nejednoznačnosti v Gramatika výrazů. Například příkaz:
```
F(G<A,B>(7));
```
možné interpretovat jako volání `F` se dvěma argumenty, `G < A` a `B > (7)`. Alternativně se dá interpretovat jako volání `F` s jedním argumentem, který je volání obecné metody `G` pomocí dva argumenty typu a pravidelné jeden argument.

Pokud posloupnost tokeny můžete analyzovat (v kontextu) jako *simple_name* ([jednoduché názvy](expressions.md#simple-names)), *member_access* ([přístup ke členu](expressions.md#member-access)), nebo *pointer_member_access* ([přístupu k členovi](unsafe-code.md#pointer-member-access)) končí *type_argument_list* ([argumenty typu](types.md#type-arguments)), Token hned za uzavírací `>` token je zkontrolován. Pokud je jeden z
```csharp
(  )  ]  }  :  ;  ,  .  ?  ==  !=  |  ^
```
pak bude *type_argument_list* se uchovávají v rámci *simple_name*, *member_access* nebo *pointer_member_access* a jakékoli Další možné parsovat posloupnost tokeny se zahodí. V opačném případě *type_argument_list* se nepovažuje za součást *simple_name*, *member_access* nebo *pointer_member_access*i v případě, že neexistuje žádná možné parsovat sekvence tokenů. Všimněte si, že tato pravidla se použijí při analýze *type_argument_list* v *namespace_or_type_name* ([Namespace a zadejte názvy](basic-concepts.md#namespace-and-type-names)). Příkaz
```csharp
F(G<A,B>(7));
```
podle tohoto pravidla se, se bude interpretovat jako volání `F` s jedním argumentem, který je volání obecné metody `G` pomocí dva argumenty typu a pravidelné jeden argument. Příkazy
```csharp
F(G < A, B > 7);
F(G < A, B >> 7);
```
bude každý interpretovat jako volání `F` se dvěma argumenty. Příkaz
```csharp
x = F < A > +y;
```
budou interpretovány jako méně než operátor větší než operátor, operátor a Unární plus, jako by měl byl zapsán příkaz `x = (F < A) > (+y)`, místo jako *simple_name* s *type_argument_list* Následuje operátor plus binární soubor. V příkazu
```csharp
x = y is C<T> + z;
```
tokeny `C<T>` jsou interpretovány jako *namespace_or_type_name* s *type_argument_list*.

### <a name="invocation-expressions"></a>Výrazy typu Invocation

*Invocation_expression* se používá k volání metody.

```antlr
invocation_expression
    : primary_expression '(' argument_list? ')'
    ;
```

*Invocation_expression* dynamicky vázán ([dynamické vazby](expressions.md#dynamic-binding)) Pokud obsahuje nejméně jednu z následujících akcí:

* *Primary_expression* má typ kompilace `dynamic`.
* Alespoň jeden argument nepovinný *argument_list* má typ kompilace `dynamic` a *primary_expression* nemá typ delegáta.

V tomto případě kompilátor klasifikuje *invocation_expression* jako hodnotu typu `dynamic`. Pravidly dole určit význam *invocation_expression* se pak použijí v době běhu, run-time typu místo kompilaci typu těch, které *primary_expression* a argumenty, které mají typ kompilace `dynamic`. Pokud *primary_expression* nemá typ kompilace `dynamic`, pak volání metody projde Kontrola času kompilace omezená, jak je popsáno v [kompilace kontrolu dynamické přetížení ](expressions.md#compile-time-checking-of-dynamic-overload-resolution).

*Primary_expression* ze *invocation_expression* musí být metoda skupiny nebo hodnotu *delegate_type*. Pokud *primary_expression* je skupinu metod *invocation_expression* je volání metody ([volání metod](expressions.md#method-invocations)). Pokud *primary_expression* . má hodnotu *delegate_type*, *invocation_expression* vyvolání delegáta ([delegáta volání](expressions.md#delegate-invocations)). Pokud *primary_expression* není skupinu metod ani hodnotu *delegate_type*, dojde k chybě vazby čas.

Volitelný *argument_list* ([seznamy argumentů](expressions.md#argument-lists)) obsahuje hodnoty nebo odkazy na proměnné pro parametry metody.

Výsledek vyhodnocení výrazu *invocation_expression* klasifikovaný následujícím způsobem:

*  Pokud *invocation_expression* vyvolá metoda nebo delegát, který vrátí `void`, výsledek má hodnotu nothing. Výraz, který je klasifikován jako nic je povolený jenom v kontextu *statement_expression* ([příkazy výrazů](statements.md#expression-statements)) nebo jako text *lambda_expression*([Výrazy anonymní funkce](expressions.md#anonymous-function-expressions)). Jinak dojde k chybě vazby čas.
*  Jinak výsledkem je hodnota typu vrácené z metody nebo delegáta.

#### <a name="method-invocations"></a>Volání metod

Pro volání metody *primary_expression* z *invocation_expression* musí být skupinu metod. Skupinu metod identifikuje jednu metodu, která se má vyvolat nebo sadu přetížených metod, ze kterého mohou vybírat konkrétní metody, která se má vyvolat. V druhém případě je stanovení konkrétní metoda k vyvolání založené na poskytované typy argumentů v kontextu *argument_list*.

Vazby – čas zpracování volání metody formuláře `M(A)`, kde `M` je skupina – metoda (verzovaným *type_argument_list*), a `A` je volitelný *argument_ seznam*, se skládá z následujících kroků:

*  Sadu metod pro volání metody je vytvořený. Pro každou metodu `F` přidružené ke skupině metoda `M`:
   *  Pokud `F` není obecná, `F` je Release candidate při:
      * `M` nemá žádný seznam argumentů typu a
      * `F` lze použít s ohledem na `A` ([použít funkční člen](expressions.md#applicable-function-member)).
   *  Pokud `F` je obecný a `M` nemá žádný seznam argumentů typu `F` je Release candidate při:
      * Odvození typu ([odvození typu](expressions.md#type-inference)) úspěšná, odvozování seznam argumentů pro volání, a
      * Jakmile se argumenty typu jsou substituovány za parametry typu odpovídající metoda, všechny sestavené typy v seznamu parametrů F vyhovět jejich omezením ([neodpovídajících omezení](types.md#satisfying-constraints)) a seznamu parametrů `F` lze použít s ohledem na `A` ([použít funkční člen](expressions.md#applicable-function-member)).
   *  Pokud `F` je obecný a `M` obsahuje seznam argumentů typu, `F` je Release candidate při:
      * `F` byly zadány v seznamu argumentů typu má stejný počet parametrů typu metoda a
      * Jakmile se argumenty typu jsou substituovány za parametry typu odpovídající metoda, všechny sestavené typy v seznamu parametrů F vyhovět jejich omezením ([neodpovídajících omezení](types.md#satisfying-constraints)) a seznamu parametrů `F` lze použít s ohledem na `A` ([použít funkční člen](expressions.md#applicable-function-member)).
*  Sadu metod Release candidate je omezit tak, aby obsahovala pouze metody ze nejvíce odvozené typy: Pro každou metodu `C.F` v sadě, kde `C` je typ, ve kterém metoda `F` je teď deklarována, všechny metody deklarované v základní typ `C` se odeberou ze sady. Kromě toho pokud `C` typu třídy je jiné než `object`, všechny metody deklarované v rozhraní typu se odeberou ze sady. (Toto druhé pravidlo pouze nemá vliv při výsledek vyhledávání člena pro parametr typu s efektivní základní třídy, než je objekt a efektivní rozhraní neprázdný nastavit skupinu metod.)
*  Pokud výslednou sadu metod je prázdný, další zpracování podél následující kroky jsou opuštěných a místo toho proveden pokus o zpracování volání jako volání metody rozšíření ([volání metod rozšíření](expressions.md#extension-method-invocations)). Pokud se to nepodaří, neexistují žádné použitelné metody a dojde k chybě vazby čas.
*  Nejlepší metody sadu metod Release candidate je identifikován pomocí pravidel rozlišení přetížení [rozlišení přetěžování](expressions.md#overload-resolution). Pokud nebylo možné identifikovat jeden nejlepší metody, je nejednoznačné volání metody a dojde k chybě vazby čas. Při překladu přetížení parametrů Obecné metody jsou považovány za po nahrazení argumentů typu (zadaný, nebo vyvozen) pro odpovídající typ parametry metody.
*  Poslední zvolené nejlepší metody ověřování:
   * Metoda ověření v kontextu skupinu metod: Nejlepší metodou je statickou metodu, musí mít skupinu metod. výsledkem *simple_name* nebo *member_access* prostřednictvím typu. Nejlepší způsob je metoda instance, musí mít skupinu metod. výsledkem *simple_name*, *member_access* prostřednictvím proměnné nebo hodnotu, nebo *base_access*. Pokud ani jedno z těchto požadavků je hodnota true, dojde k chybě vazby čas.
   * Pokud je nejlepší metodou je obecná metoda, zadejte argumenty (poskytnuté nebo odvozené) jsou porovnávána s omezením ([neodpovídajících omezení](types.md#satisfying-constraints)) deklarovat v obecné metody. Pokud některý z argumentů typu nevyhovuje odpovídající omezení u parametru typu, dojde k chybě vazby čas.

Jakmile metodu byla vybrána a ověřený v době vazby výše uvedené kroky, skutečné vyvolání za běhu se zpracovává podle pravidel objektů volání členské funkce popsané v [kompilace kontrolu dynamické přetížení ](expressions.md#compile-time-checking-of-dynamic-overload-resolution).

Intuitivní efekt výše popsaná pravidla překladu je následujícím způsobem: Vyhledat konkrétní metody vyvolané volání metody, začněte s typ označený volání metody a pokračovat, dokud nebude nalezen alespoň jednu metodu použít, přístupná, bez přepsání deklarace celým řetězcem dědičnosti. Následně odvození typu proměnné a řešení v sadě použít, přístupná, bez přepsání metody deklarované v tomto typu přetížení a vyvolat metodu tedy vybrali. Pokud nebyla nalezena žádná metoda, zkuste místo toho zpracovat volání jako volání metody rozšíření.

#### <a name="extension-method-invocations"></a>Volání metod rozšíření

Ve volání metody ([volání na instancích zabalený](expressions.md#invocations-on-boxed-instances)) jednoho z formuláře
```csharp
expr . identifier ( )

expr . identifier ( args )

expr . identifier < typeargs > ( )

expr . identifier < typeargs > ( args )
```
Pokud normálním zpracování volání najde žádné použitelné metody, je proveden pokus o zpracování konstrukce jako volání metody rozšíření. Pokud *expr* nebo některý z *args* má typ kompilace `dynamic`, rozšiřující metody se nepoužije.

Cílem je najít nejlepší *type_name* `C`, takže může proběhnout odpovídající volání statické metody:
```csharp
C . identifier ( expr )

C . identifier ( expr , args )

C . identifier < typeargs > ( expr )

C . identifier < typeargs > ( expr , args )
```

Metody rozšíření `Ci.Mj` je ***oprávněné*** pokud:

*  `Ci` je jiná než obecná, ne vnořené třídy
*  Název `Mj` je *identifikátor*
*  `Mj` je přístupný a použitelný, při použití na argumenty jako statická metoda, jak je uvedeno výše
*  Existuje implicitní identity, odkaz nebo převod na uzavřené určení z *expr* typu první parametr `Mj`.

Hledání `C` probíhá následujícím způsobem:

*  Od nejbližší uzavírající deklarace oboru názvů, pokračujte v každé nadřazené deklarace oboru názvů a konče obsahující kompilační jednotky byly provedeny po sobě jdoucích pokusy o vyhledání kandidátských sadu rozšiřujících metod:
   * Pokud daný obor názvů nebo kompilace jednotka přímo obsahuje neobecný typ deklarace `Ci` s oprávněné rozšiřující metody `Mj`, pak sada z těchto metod rozšíření je sada Release candidate.
   * Pokud se typy `Ci` importované tímto seznamem *using_static_declarations* a přímo deklarovány v oborech názvů importované tímto seznamem *using_namespace_directive*s v daném oboru názvů nebo kompilace jednotce přímo obsahují metody rozšíření oprávněné `Mj`, pak sada z těchto metod rozšíření je sada Release candidate.
*  Pokud není nastaven žádný kandidát nenajde v libovolné nadřazené jednotce prohlášení nebo kompilace obor názvů, dojde k chybě v době kompilace.
*  V opačném případě řešení přetížení se použije na verzi Release candidate popsaného v ([rozlišení přetěžování](expressions.md#overload-resolution)). Pokud se nenajde žádný jediné nejlepší metody, dojde k chybě kompilace.
*  `C` je typ, ve kterém je nejlepší metody deklarován jako metody rozšíření.

Pomocí `C` jako cíl, volání metody, které pak zpracovány jako volání statické metody ([kompilace kontrolu dynamické přetížení](expressions.md#compile-time-checking-of-dynamic-overload-resolution)).

Výše uvedená pravidla znamená, že metody instance přednost metody rozšíření, že rozšiřující metody, které jsou k dispozici v oboru názvů vnitřní deklarace přednost rozšiřující metody, které jsou dostupné na vnější obor názvů deklarace a rozšíření metody s deklarací přímo v oboru názvů přednost metody rozšíření importován Tento stejný obor názvů pomocí direktivy oboru názvů. Příklad:
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

V tomto příkladu `B`metoda má přednost před první metodu rozšíření, a `C`metoda má přednost před obě metody rozšíření.

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
```
E.F(1)
D.G(2)
C.H(3)
```
`D.G` má přednost před `C.G`, a `E.F` má přednost před obě `D.F` a `C.F`.

#### <a name="delegate-invocations"></a>Vyvolání delegáta

Pro vyvolání typu delegáta *primary_expression* z *invocation_expression* musí být hodnota *delegate_type*. Navíc vzhledem k tomu *delegate_type* funkce člena se stejným seznamem parametrů jako *delegate_type*, *delegate_type* musí být příslušný () [Použitelná funkce člena](expressions.md#applicable-function-member)) s ohledem na *argument_list* z *invocation_expression*.

Za běhu zpracování volání delegáta formuláře `D(A)`, kde `D` je *primary_expression* z *delegate_type* a `A` je volitelné *argument_list*, se skládá z následujících kroků:

*  `D` je vyhodnocen. Je-li toto vyhodnocení způsobí výjimku, jsou spuštěny žádné další kroky.
*  Hodnota `D` je zaškrtnuté políčko platný. Pokud hodnota `D` je `null`, `System.NullReferenceException` je vyvolána, a že jsou provedeny žádné další kroky.
*  V opačném případě `D` je odkaz na instanci delegáta. Volání členské funkce ([kompilace kontrolu dynamické přetížení](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) se provádí na všech volatelných entit v seznamu vyvolání delegáta. Volatelný entit skládající se z instance a metoda instance služby je instance pro vyvolání instance obsaženého v entitě volatelná aplikacemi.

### <a name="element-access"></a>Přístup k prvkům

*Element_access* se skládá z *primary_no_array_creation_expression*a po něm "`[`" token a po něm *argument_list*a po něm " `]`"token. *Argument_list* obsahuje jeden nebo více *argument*s, oddělené čárkami.

```antlr
element_access
    : primary_no_array_creation_expression '[' expression_list ']'
    ;
```

*Argument_list* ze *element_access* nesmí obsahovat `ref` nebo `out` argumenty.

*Element_access* dynamicky vázán ([dynamické vazby](expressions.md#dynamic-binding)) Pokud obsahuje nejméně jednu z následujících akcí:

* *Primary_no_array_creation_expression* má typ kompilace `dynamic`.
* Nejméně jeden výraz *argument_list* má typ kompilace `dynamic` a *primary_no_array_creation_expression* nemá typ pole.

V tomto případě kompilátor klasifikuje *element_access* jako hodnotu typu `dynamic`. Pravidly dole určit význam *element_access* se pak použijí v době běhu, run-time typu místo kompilaci typu těch, které *primary_no_array_creation_expression*a *argument_list* výrazy, které mají typ kompilace `dynamic`. Pokud *primary_no_array_creation_expression* nemá typu v době kompilace `dynamic`, pak přístup k prvkům projde Kontrola času kompilace omezená, jak je popsáno v [kontrolu dynamická kompilace Rozlišení přetěžování](expressions.md#compile-time-checking-of-dynamic-overload-resolution).

Pokud *primary_no_array_creation_expression* z *element_access* . má hodnotu *array_type*, *element_access* je Array – přístup ([pole přístup](expressions.md#array-access)). V opačném případě *primary_no_array_creation_expression* musí být proměnná nebo hodnota třídy, struktury nebo typu rozhraní, který má jeden nebo více členů indexer, v takovém případě *element_access* je přístup indexeru ([přístup indexeru](expressions.md#indexer-access)).

#### <a name="array-access"></a>Přístup k poli

Pro přístup k poli *primary_no_array_creation_expression* z *element_access* musí být hodnota *array_type*. Kromě toho *argument_list* pole přístup nesmí obsahovat pojmenované argumenty. Počet výrazů v *argument_list* musí být stejné jako pořadí *array_type*, a každý výraz musí být typu `int`, `uint`, `long`, `ulong`, nebo musí být implicitně převeditelný na jeden nebo více z těchto typů.

Výsledek vyhodnocení výrazu přístup k poli je proměnná typu prvku pole, konkrétně prvku pole zvolila hodnoty výrazů v *argument_list*.

Zpracování za běhu přístup k poli formuláře `P[A]`, kde `P` je *primary_no_array_creation_expression* ze *array_type* a `A` je *argument_list*, se skládá z následujících kroků:

*  `P` je vyhodnocen. Je-li toto vyhodnocení způsobí výjimku, jsou spuštěny žádné další kroky.
*  Výrazy indexu *argument_list* se vyhodnocují v pořadí zleva doprava. Vyhodnocení výrazu každý index implicitní převod ([implicitních převodů](conversions.md#implicit-conversions)) se provádí na jednu z následujících typů: `int`, `uint`, `long`, `ulong`. První typ v tomto seznamu, pro kterou existuje implicitní převod je vybrán. Například pokud Indexový výraz je typu `short` pak implicitní převod na `int` provádí, protože implicitní převody z `short` k `int` a z `short` k `long` jsou možné. Pokud limit vyhodnocení výrazu indexu nebo následné implicitní převod způsobí výjimku, žádné další indexové výrazy jsou vyhodnoceny a žádné další provádění kroků.
*  Hodnota `P` je zaškrtnuté políčko platný. Pokud hodnota `P` je `null`, `System.NullReferenceException` je vyvolána, a že jsou provedeny žádné další kroky.
*  Hodnota každý výraz v *argument_list* je porovnávána s skutečné mezí jednotlivých rozměrů pole instance odkazuje `P`. Pokud jeden nebo více hodnot je mimo rozsah, `System.IndexOutOfRangeException` je vyvolána, a že jsou provedeny žádné další kroky.
*  Umístění prvku pole, které jsou uvedena v každém výrazů indexu je vypočítán a toto umístění bude výsledek přístup k poli.

#### <a name="indexer-access"></a>Přístup indexeru

Pro přístup indexeru *primary_no_array_creation_expression* z *element_access* musí být proměnná nebo hodnota třídy, struktury nebo typ rozhraní, a tento typ musí implementovat jednu nebo více indexery, které se dají použít s ohledem na *argument_list* z *element_access*.

Vazba čas zpracování přístupového indexer formuláře `P[A]`, kde `P` je *primary_no_array_creation_expression* třídy, struktury nebo rozhraní typu `T`, a `A` je *argument_list*, se skládá z následujících kroků:

*  Sada indexery poskytované `T` je vytvořený. Sada se skládá ze všech indexerů, které jsou deklarované v `T` nebo základního typu `T` , které nejsou `override` deklarace a jsou v aktuálním kontextu k dispozici ([přístup ke členu](basic-concepts.md#member-access)).
*  Sada je omezit na tyto indexerů, které jsou použitelné a nikoli podle jinými indexery. Následující pravidla platí pro každý indexer `S.I` v sadě, kde `S` je typ, ve kterém indexeru `I` je deklarována jako:
   * Pokud `I` se nedá použít s ohledem na `A` ([použitelná funkce člena](expressions.md#applicable-function-member)), pak `I` je odebrán ze sady.
   * Pokud `I` lze použít s ohledem na `A` ([použitelná funkce člena](expressions.md#applicable-function-member)), pak všechny indexery deklarovaný v základní typ `S` se odeberou ze sady.
   * Pokud `I` lze použít s ohledem na `A` ([použitelná funkce člena](expressions.md#applicable-function-member)) a `S` typu třídy je jiné než `object`, všechny indexery deklarované v rozhraní se odeberou ze sady.
*  Pokud výslednou sadu Release candidate indexerů je prázdný, pak neexistují žádné použitelné indexery a dojde k chybě vazby čas.
*  Nejlepší indexer sadu Release candidate indexerů je identifikován pomocí pravidel rozlišení přetížení [rozlišení přetěžování](expressions.md#overload-resolution). Pokud jeden nejlepší indexer nelze identifikovat, přístup indexeru je nejednoznačný a dojde k chybě vazby čas.
*  Výrazy indexu *argument_list* se vyhodnocují v pořadí zleva doprava. Výsledek zpracování přístup indexeru je výraz jsou klasifikovány jako přístup indexeru. Výraz přístup indexeru odkazuje na indexer nadefinovali v předchozím kroku a má přidruženou instanci výraz `P` a seznam argumentů přidružený `A`.

V závislosti na kontextu, ve kterém se používá, přístup indexeru způsobí, že volání buď *načtení přístupového objektu* nebo *nastavení přístupového objektu* indexeru. Pokud přístup indexeru je cílem přiřazení, *nastavení přístupového objektu* se vyvolá, aby přiřadí novou hodnotu ([jednoduché přiřazení](expressions.md#simple-assignment)). Ve všech ostatních případech *načtení přístupového objektu* se vyvolá k získání aktuální hodnoty ([hodnot výrazů](expressions.md#values-of-expressions)).

### <a name="this-access"></a>Tento přístup

A *this_access* se skládá z vyhrazené slovo `this`.

```antlr
this_access
    : 'this'
    ;
```

A *this_access* smí obsahovat pouze v *bloku* konstruktor instance, metoda instance nebo přístupový objekt instance. Má jednu z následujících význam:

*  Když `this` je používán *primary_expression* v rámci konstruktoru instance třídy, je klasifikován jako hodnotu. Typ hodnoty je typ instance ([typ instance](classes.md#the-instance-type)) třídy, ve kterém dojde k použití a hodnota je odkaz na objekt, který vytváří.
*  Když `this` je používán *primary_expression* v rámci metody instance nebo přístupový objekt instance třídy, je klasifikován jako hodnotu. Typ hodnoty je typ instance ([typ instance](classes.md#the-instance-type)) třídy, ve kterém dojde k použití a hodnota je odkaz na objekt, pro kterou byla vyvolána metoda nebo přístupový objekt.
*  Když `this` je používán *primary_expression* v rámci konstruktoru instance struktury, je klasifikován jako proměnnou. Typ proměnné je typ instance ([typ instance](classes.md#the-instance-type)) struktury, ve kterém dojde k použití a představuje proměnnou struktury vytváří. `this` Proměnné konstruktor instance struktury se chová stejně jako `out` parametr typu struct – konkrétně to znamená, že proměnná musí být jednoznačně přiřazena v jednotlivých cestách spuštění instance konstruktor.
*  Když `this` je používán *primary_expression* v rámci metody instance nebo přístupový objekt instance struktury, je klasifikován jako proměnnou. Typ proměnné je typ instance ([typ instance](classes.md#the-instance-type)) struktury, ve kterém dochází k použití.
   * Pokud metoda nebo přístupový objekt není iterátor ([iterátory](classes.md#iterators)), `this` proměnná představuje strukturu, pro který se vyvolala metodu nebo přistupující objekt a jak se bude chovat stejně jako `ref` parametr typu Struktura.
   * Pokud metoda nebo přístupový objekt iterátoru, `this` proměnná představuje kopii struktury, pro který byla vyvolána metoda nebo přístupový objekt a chová stejně jako hodnota parametru typu Struktura.

Použití `this` v *primary_expression* v jiném kontextu než jsou výše uvedené je chyba kompilace. Zejména, není možné odkazovat na `this` v statická metoda, přístupový objekt statické vlastnosti, nebo *variable_initializer* deklarace pole.

### <a name="base-access"></a>Základní přístup

A *base_access* se skládá z vyhrazené slovo `base` následována buď "`.`" token a identifikátor nebo s *argument_list* uzavřeny do hranatých závorek:

```antlr
base_access
    : 'base' '.' identifier
    | 'base' '[' expression_list ']'
    ;
```

A *base_access* slouží k přístupu k členům základní třídy, které jsou skryté členy podobně pojmenovaných v aktuální třídě nebo struktuře. A *base_access* smí obsahovat pouze v *bloku* konstruktor instance, metoda instance nebo přístupový objekt instance. Když `base.I` v třídě nebo struktuře, dojde k `I` musí označení člena základní třídy této třídy nebo struktury. Podobně když `base[E]` ve třídě, vyvolá se v základní třídě musí existovat odpovídající indexer.

Během vazby *base_access* výrazy formuláře `base.I` a `base[E]` vyhodnocují stejně, jako kdyby byly napsány `((B)this).I` a `((B)this)[E]`, kde `B` je základní třídou třídy nebo struktury, ve kterém dojde k vytvoření. Proto `base.I` a `base[E]` odpovídají `this.I` a `this[E]`, s výjimkou `this` se pohlíží jako instance základní třídy.

Když *base_access* odkazuje na virtuální funkce člena (metoda, vlastnost nebo indexer), určení funkce člena, který má být vyvolán při spuštění ([kompilace kontrolu dynamické přetížení ](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) se změní. Člen funkce, která je vyvolána je určen tím, že hledá implementace Většina odvozených ([virtuální metody](classes.md#virtual-methods)) z členské funkce s ohledem na `B` (místo s ohledem na typu za běhu `this`, jako by obvykle v jiných základní přístup). Díky tomu se v rámci `override` z `virtual` členské funkce *base_access* můžete použít k vyvolání zděděná implementace metody členské funkce. Pokud odkazuje členské funkce *base_access* je abstraktní, dojde k chybě vazby čas.

### <a name="postfix-increment-and-decrement-operators"></a>Příponové operátory Inkrementace a dekrementace operátory

```antlr
post_increment_expression
    : primary_expression '++'
    ;

post_decrement_expression
    : primary_expression '--'
    ;
```

Operand operace přípony zvýšení nebo snížení musí být výraz jsou klasifikovány jako proměnné, přístup k vlastnosti nebo indexeru přístupu. Výsledkem operace je hodnota stejného typu jako operand.

Pokud *primary_expression* má typ kompilace `dynamic` pak operátor, který je vázán dynamicky ([dynamické vazby](expressions.md#dynamic-binding)), *post_increment_expression*nebo *post_decrement_expression* má typ kompilace `dynamic` a za běhu pomocí typu za běhu se použijí následující pravidla *primary_expression*.

Je-li zvýšit operand příponový operace dekrementace je vlastnost nebo indexer přístup, vlastnost nebo indexer musí mít obě `get` a `set` přistupujícího objektu. Pokud to není tento případ, dojde k chybě vazby čas.

Unární operátor rozlišení přetěžování ([unární operátor rozlišení přetěžování](expressions.md#unary-operator-overload-resolution)) se použije k výběru na konkrétní operátor implementace. Předdefinované `++` a `--` operátory existovat pro následující typy: `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char` , `float`, `double`, `decimal`a jakýkoli typ výčtu. Předdefinované `++` operátory návratová hodnota vytváří přidáním 1 operand a předdefinované `--` operátory návratová hodnota vytváří tak, že se 1 z operand. V `checked` kontextu, pokud výsledek této sčítání a odčítání je mimo rozsah typu výsledku a typ výsledku je integrální typ nebo typ výčtu `System.OverflowException` je vyvolána výjimka.

Zpracování za běhu přípony přírůstek nebo snížení operace formuláře `x++` nebo `x--` se skládá z následujících kroků:

*   Pokud `x` je klasifikován jako proměnnou:
    * `x` je vyhodnocen pro vytvoření proměnné.
    * Hodnota `x` se uloží.
    * S použitím uložené hodnoty z je vyvolána zvoleném operátorovi `x` jako svůj argument.
    * Hodnota vrácená operátorem je uložen v umístění Dal vyhodnocení `x`.
    * Uložená hodnota `x` výsledek operace.
*   Pokud `x` klasifikovaný jako vlastnost nebo indexovací člen přístupu:
    * Výraz instance (Pokud `x` není `static`) a v seznamu argumentů (Pokud `x` představuje přístup k indexer) přidružené k `x` jsou vyhodnocovány, a výsledky se používají v následné `get` a `set` přístupový objekt volání.
    * `get` Přistupující objekt `x` je vyvolána a vrácené hodnoty se uloží.
    * S použitím uložené hodnoty z je vyvolána zvoleném operátorovi `x` jako svůj argument.
    * `set` Přistupující objekt `x` vyvolání hodnota vrácená operátorem jako jeho `value` argument.
    * Uložená hodnota `x` výsledek operace.

`++` a `--` operátory také podporují předpony ([předpony Inkrementace a dekrementace operátory](expressions.md#prefix-increment-and-decrement-operators)). Obvykle, výsledek `x++` nebo `x--` je hodnota `x` před provedením operace, zatímco výsledek `++x` nebo `--x` je hodnota `x` po provedení této operace. V obou případech `x` sám po provedení této operace má stejnou hodnotu.

`operator ++` Nebo `operator --` implementace lze vyvolat pomocí uplatněna přípona nebo předpona zápisu. Není možné mít samostatné operátor implementace pro zápisy dva.

### <a name="the-new-operator"></a>New – operátor

`new` Operátor se používá k vytvoření nové instance typů.

Existují tři formy `new` výrazy:

*  Výrazy vytváření objektů se používají k vytvoření nové instance typů tříd a typů hodnot.
*  Výrazy vytvoření pole se používají k vytvoření nové instance typy polí.
*  Výrazy vytvoření delegáta se používají k vytvoření nové instance delegáta typů.

`new` Operátor zahrnuje vytvoření instance typu, ale nutně neznamená dynamické přidělování paměti. Konkrétně se instance typů hodnot vyžadují žádné další paměť nad rámec proměnné, ve kterých se nacházejí, a dojde k žádné dynamické přidělení při `new` slouží k vytvoření instancí typů hodnot.

#### <a name="object-creation-expressions"></a>Výrazy vytváření objektů

*Object_creation_expression* umožňuje vytvořit novou instanci třídy *class_type* nebo *value_type*.

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

*Typ* ze *object_creation_expression* musí být *class_type*, *value_type* nebo *type_parameter* . *Typ* nemůže být `abstract` *class_type*.

Volitelný *argument_list* ([seznamy argumentů](expressions.md#argument-lists)) je povolený jenom v případě *typ* je *class_type* nebo *struct_ typ*.

Vytvoření výrazu objektu lze vynechat seznam argumentů konstruktoru a uzavírající závorky za předpokladu, že obsahuje objekt inicializátor nebo inicializátor kolekce. Vynechání seznamu argumentů konstruktoru a uzavírající závorky je ekvivalentní se zadáním prázdným seznamem argumentů.

Zpracování výrazu vytvoření objektu, který obsahuje objekt inicializátor nebo inicializátor kolekce se skládá z první zpracování konstruktor instance a potom zpracování zadaná pomocí inicializátoru objektů (inicializacíčlenneboelement[ Inicializátory objektu](expressions.md#object-initializers)) nebo inicializátor kolekce ([inicializátory kolekce](expressions.md#collection-initializers)).

Pokud některý z argumentů volitelné *argument_list* má typ kompilace `dynamic` pak bude *object_creation_expression* dynamicky vázán ([dynamické vazby](expressions.md#dynamic-binding)) a za běhu pomocí typu za běhu z těchto argumentů se použijí následující pravidla *argument_list* , které mají typ času kompilace `dynamic`. Ale při vytvoření objektu Kontrola času kompilace omezené jak je popsáno v [kompilace kontrolu dynamické přetížení](expressions.md#compile-time-checking-of-dynamic-overload-resolution).

Zpracování doba vazby *object_creation_expression* formuláře `new T(A)`, kde `T` je *class_type* nebo *value_type* a `A` je volitelný *argument_list*, se skládá z následujících kroků:

*   Pokud `T` je *value_type* a `A` není k dispozici:
    * *Object_creation_expression* je vyvolání výchozího konstruktoru. Výsledek *object_creation_expression* je hodnota typu `T`, a to výchozí hodnota pro `T` jak jsou definovány v [typ The System.ValueType](types.md#the-systemvaluetype-type).
*   Jinak, pokud `T` je *type_parameter* a `A` není k dispozici:
    * Pokud žádná hodnota typu omezení nebo omezení konstruktoru ([omezení parametru typu](classes.md#type-parameter-constraints)) byl zadán pro `T`, dojde k chybě vazby čas.
    * Výsledek *object_creation_expression* je hodnota typu za běhu, svázané parametr typu, jmenovitě výsledek volání výchozího konstruktoru typu. Run-time typu může být typem odkazu nebo hodnotového typu.
*   Jinak, pokud `T` je *class_type* nebo *struct_type*:
    * Pokud `T` je `abstract` *class_type*, dojde k chybě kompilace.
    * Konstruktor instance, který má být vyvolán je určen pomocí pravidel rozlišení přetížení [rozlišení přetěžování](expressions.md#overload-resolution). Sada konstruktory instancí Release candidate zahrnuje všechny dostupné instance konstruktory deklarované v `T` které se dají použít s ohledem na `A` ([použít funkční člen](expressions.md#applicable-function-member)). Pokud sadu Release candidate konstruktory instancí je prázdná nebo jediný konstruktor instance nejlepší nelze identifikovat, dojde k chybě vazby čas.
    * Výsledkem *object_creation_expression* je hodnota typu `T`, konkrétně hodnota vytvořen zavoláním konstruktoru instance nadefinovali v předchozím kroku.
*  V opačném případě *object_creation_expression* není platný, a dojde k chybě vazby čas.

I v případě, *object_creation_expression* je dynamicky vázán, typ kompilace je stále `T`.

Zpracování za běhu *object_creation_expression* formuláře `new T(A)`, kde `T` je *class_type* nebo *struct_type* a `A` je volitelný *argument_list*, se skládá z následujících kroků:

*   Pokud `T` je *class_type*:
    * Vytvoření nové instance třídy `T` je přidělen. Pokud není k dispozici dostatek paměti k přidělení nové instance `System.OutOfMemoryException` je vyvolána, a že jsou provedeny žádné další kroky.
    * Všechna pole nové instance jsou inicializovány na výchozí hodnoty ([výchozí hodnoty](variables.md#default-values)).
    * Podle pravidel objektů volání členské funkce je volána konstruktor instance ([kompilace kontrolu dynamické přetížení](expressions.md#compile-time-checking-of-dynamic-overload-resolution)). Odkaz na nově přidělenou instanci automaticky předána do konstruktoru instance a instance je přístupný z v rámci tohoto konstruktoru jako `this`.
*   Pokud `T` je *struct_type*:
    * Instance typu `T` vytvoří dočasnou proměnnou místního přidělení. Od verze konstruktoru instance *struct_type* je potřeba jednoznačně přiřadit hodnotu každému poli instance vytváří, není inicializace dočasné proměnné, je nezbytné.
    * Podle pravidel objektů volání členské funkce je volána konstruktor instance ([kompilace kontrolu dynamické přetížení](expressions.md#compile-time-checking-of-dynamic-overload-resolution)). Odkaz na nově přidělenou instanci automaticky předána do konstruktoru instance a instance je přístupný z v rámci tohoto konstruktoru jako `this`.

#### <a name="object-initializers"></a>Inicializátory objektů

***Objektu inicializátoru*** určuje hodnoty pro nula nebo více polí, vlastnosti nebo indexované prvků objektu.

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

Inicializátor objektu se skládá z posloupnost inicializátory členů, ohraničená `{` a `}` tokeny a oddělené čárkami. Každý *member_initializer* určuje cíle pro inicializaci. *Identifikátor* nutné pojmenovat přístupné pole nebo vlastnost objektu inicializována, že *argument_list* ohraničeno hranaté závorky, musíte zadat argumenty pro indexer přístupné na objekt, který je inicializován. Jedná se o chybu pro inicializátoru objektu, který chcete zahrnout více než jeden inicializátor členu pro stejné pole nebo vlastnost.

Každý *initializer_target* následuje znak rovná se a výrazu, inicializátoru objektu nebo inicializátor kolekce. Není možné pro výrazy v inicializátoru objektu, který neodkazuje na nově vytvořený objekt, který je inicializace.

Inicializátor členu, který určuje výraz, až se zpracují znaménko rovná se stejným způsobem jako přiřazení ([jednoduché přiřazení](expressions.md#simple-assignment)) k cíli.

Inicializátor členu, který určuje inicializátoru objektu po znaménko rovná se ***inicializátor vnořeného objektu***, to znamená inicializace vloženého objektu. Namísto přiřazení nové hodnoty pro pole nebo vlastnost, jsou považovány přiřazení v inicializátoru vnořený objekt za přiřazení pro členy pole nebo vlastnost. Inicializátory objektů vnořené nejde použít s typem hodnoty vlastnosti nebo pole jen pro čtení s typem hodnoty.

Inicializátor členu, který určuje inicializátoru kolekce po inicializaci vložené kolekce znaménko rovná se. Namísto přiřazení novou kolekci do cílového pole, vlastnost nebo indexer, jsou prvky uvedené v inicializátoru přidat do kolekce odkazuje cíl. Cíl musí být typu kolekce, který splňuje požadavky uvedené v [inicializátory kolekce](expressions.md#collection-initializers).

Argumenty, které mají index inicializátor vždy se vyhodnotí pouze jednou. Díky tomu se i v případě, že argumenty skončit, získávání nikdy použit (např. z důvodu prázdný inicializátor vnořeného), bude se vyhodnocují hlediska jejich vedlejších účinků.

Následující třída reprezentuje bod se souřadnicemi dvě:
```csharp
public class Point
{
    int x, y;

    public int X { get { return x; } set { x = value; } }
    public int Y { get { return y; } set { y = value; } }
}
```

Instance `Point` může být vytvořeno a inicializováno následujícím způsobem:
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
kde `__a` je dočasná proměnná jinak neviditelné a nejsou přístupné. Následující třída reprezentuje obdélník vytvořené z dva body:
```csharp
public class Rectangle
{
    Point p1, p2;

    public Point P1 { get { return p1; } set { p1 = value; } }
    public Point P2 { get { return p2; } set { p2 = value; } }
}
```

Instance `Rectangle` může být vytvořeno a inicializováno následujícím způsobem:
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
kde `__r`, `__p1` a `__p2` jsou dočasné proměnné, které jsou jinak neviditelné a nejsou přístupné.

Pokud `Rectangle`pro konstruktor přiděluje dvě vložené `Point` instancí
```csharp
public class Rectangle
{
    Point p1 = new Point();
    Point p2 = new Point();

    public Point P1 { get { return p1; } }
    public Point P2 { get { return p2; } }
}
```
Následující konstruktor umožňuje inicializovat vložený `Point` instance namísto přiřazení nových instancí:
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

Zadaný odpovídající definici jazyka C, v následujícím příkladu:
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
je ekvivalentní k této sérii přiřazení:
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
kde `__c`atd jsou generované proměnné, které jsou skryté a nepřístupný pro zdrojový kód. Všimněte si, že argumenty `[0,0]` je Vyhodnocená jenom jednou a argumenty `[1,2]` jsou vyhodnoceny jednou, i když se nikdy používají.

#### <a name="collection-initializers"></a>Inicializátory kolekce

Inicializátor kolekce určuje prvků kolekce.

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

Inicializátor kolekce se skládá z posloupnost inicializátorů prvku, ohraničená `{` a `}` tokeny a oddělené čárkami. Každý prvek inicializátor určuje element, který má být přidána do objektu kolekce, který je inicializován a obsahuje seznam výrazů uzavřená v `{` a `}` tokeny a oddělené čárkami.  Inicializátor prvku jedním výrazem lze napsat bez závorek, ale pak nemůže být výraz přiřazení, aby se zabránilo nejednoznačnosti s inicializátory členů. *Non_assignment_expression* produkčním prostředí je definována v [výraz](expressions.md#expression).

Následuje příklad výrazu vytvoření objektu, která obsahuje inicializátor kolekce:
```csharp
List<int> digits = new List<int> { 0, 1, 2, 3, 4, 5, 6, 7, 8, 9 };
```

Objekt kolekce, do které se použije inicializátoru kolekce musí být typu, který implementuje `System.Collections.IEnumerable` nebo dojde k chybě kompilace. Pro každý zadaný element v pořadí, vyvolá inicializátor kolekce `Add` metodu na cílovém objektu se seznamem výrazu inicializátor prvku jako seznam argumentů, použití členu vyhledávání a řešení pro každé vyvolání přetížení. Proto objekt kolekce musí mít použitelnou metodu instance nebo rozšíření s názvem `Add` pro každý prvek inicializátoru.

Následující třída reprezentuje kontakt s názvem a seznam telefonních čísel:
```csharp
public class Contact
{
    string name;
    List<string> phoneNumbers = new List<string>();

    public string Name { get { return name; } set { name = value; } }

    public List<string> PhoneNumbers { get { return phoneNumbers; } }
}
```

A `List<Contact>` může být vytvořeno a inicializováno následujícím způsobem:
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
kde `__clist`, `__c1` a `__c2` jsou dočasné proměnné, které jsou jinak neviditelné a nejsou přístupné.

#### <a name="array-creation-expressions"></a>Výrazy vytvoření pole

*Array_creation_expression* slouží k vytvoření nové instance *array_type*.

```antlr
array_creation_expression
    : 'new' non_array_type '[' expression_list ']' rank_specifier* array_initializer?
    | 'new' array_type array_initializer
    | 'new' rank_specifier array_initializer
    ;
```

Výraz vytvoření pole ve formuláři první přiděluje pole instance typu, který je výsledkem odstraňuje všechny jednotlivé výrazy v seznamu výrazů. Například výraz vytvoření pole `new int[10,20]` vytvoří pole instance typu `int[,]`a výraz vytvoření pole `new int[10][,]` vytvoří pole typu `int[][,]`. Každý výraz v seznamu výrazů musí být typu `int`, `uint`, `long`, nebo `ulong`, nebo implicitně převést na jeden nebo více z těchto typů. Hodnota každý výraz určuje délku odpovídající dimenze v nově přidělenou pole instance. Protože rozměr pole musí být nezáporné, je chyba kompilace mít *constant_expression* se zápornou hodnotou v seznamu výrazů.

S výjimkou v kontextu unsafe ([nezabezpečený kontext](unsafe-code.md#unsafe-contexts)), rozložení polí není zadána.

Pokud výraz vytvoření pole ve formuláři první obsahuje inicializátor pole, každý výraz v seznamu výrazů musí být konstanta a počet rozměrů a dimenze délky určený seznam výrazů musí odpovídat názvům inicializátor pole.

Ve výraz vytvoření pole formuláře druhý nebo třetí řád specifikátor typu nebo řazení zadané pole musí odpovídat inicializátor pole. Délky jednotlivých dimenze jsou odvozeny z počtu prvků v každém z odpovídajících úrovní vnoření inicializátoru pole. Proto výraz
```csharp
new int[,] {{0, 1}, {2, 3}, {4, 5}}
```
přesně odpovídá
```csharp
new int[3, 2] {{0, 1}, {2, 3}, {4, 5}}
```

Výraz vytvoření pole ve formuláři třetí se označuje jako ***implicitně typované výraz vytvoření pole***. S tím rozdílem, že typ elementu pole není uvedena explicitní, ale určena jako nejlépe společný typ je podobné pro druhý formulář ([hledání nejlepší společný typ sada výrazů](expressions.md#finding-the-best-common-type-of-a-set-of-expressions)) sady výrazů v poli inicializátor. Pro multidimenzionální pole například jednoho, kde *rank_specifier* obsahuje nejméně jednu čárku, tato sada zahrnuje všechny *výraz*s součástí vnořené *array_initializer*s.

Inicializátory polí jsou popsány dále v [Inicializátory pole](arrays.md#array-initializers).

Výsledek vyhodnocení výrazu výraz vytvoření pole je klasifikován jako hodnotu, konkrétně odkaz na nově přidělenou pole instance. Zpracování za běhu výraz vytvoření pole se skládá z následujících kroků:

*  Výrazy délka dimenze *expression_list* se vyhodnocují v pořadí zleva doprava. Vyhodnocení každý výraz implicitní převod ([implicitních převodů](conversions.md#implicit-conversions)) se provádí na jednu z následujících typů: `int`, `uint`, `long`, `ulong`. První typ v tomto seznamu, pro kterou existuje implicitní převod je vybrán. Pokud limit vyhodnocení výrazu nebo následné implicitní převod způsobí výjimku, žádné další výrazy jsou vyhodnoceny a jsou provedeny žádné další kroky.
*  Vypočítané hodnoty pro délky dimenzí se ověří následujícím způsobem. Pokud jeden nebo více hodnot je menší než nula, `System.OverflowException` je vyvolána, a že jsou provedeny žádné další kroky.
*  Instance pole s délkou dané dimenze je přidělen. Pokud není k dispozici dostatek paměti k přidělení nové instance `System.OutOfMemoryException` je vyvolána, a že jsou provedeny žádné další kroky.
*  Všechny prvky nové instance pole jsou inicializovány na výchozí hodnoty ([výchozí hodnoty](variables.md#default-values)).
*  Pokud výraz vytvoření pole obsahuje inicializátor pole, každý výraz v inicializátoru pole je vyhodnocen a přiřazen do svého odpovídajícího prvku pole. Hodnocení a přiřazení se provádějí v pořadí výrazy jsou napsané v inicializátoru pole – jinými slovy, prvky jsou inicializovány ve vzestupném pořadí index s zvýšení první rozměr nejvíce vpravo. Pokud zkušební daného výrazu nebo následné přiřazení odpovídající prvek pole způsobí výjimku, žádné další prvky jsou inicializovány (a zbývající prvky budou mít výchozí hodnoty).

Výraz vytvoření pole umožňuje vytvoření instance pole s prvky typu pole, ale prvky takové pole musí být inicializován ručně. Například příkaz
```csharp
int[][] a = new int[100][];
```
Vytvoří jednorozměrné pole 100 elementy typu `int[]`. Počáteční hodnota každý element je `null`. Není možné pro stejný výraz vytvoření pole se také vytvořit instanci dílčí pole a příkaz
```csharp
int[][] a = new int[100][5];        // Error
```
výsledkem chyba kompilace. Vytvoření instance dílčí pole je nutné místo toho provést ručně, jako v
```csharp
int[][] a = new int[100][];
for (int i = 0; i < 100; i++) a[i] = new int[5];
```

Pokud má pole polí "obdélníkový" tvar, který je dílčí pole je všechny stejnou délku, je výhodnější používat vícerozměrné pole. V předchozím příkladu vytvoření instance pole polí vytvoří objekty 101 – jedno pole vnější a 100 dílčí pole. Naproti tomu
```csharp
int[,] = new int[100, 5];
```
vytvoří pouze jeden objekt, dvourozměrné pole a provede přidělení v jediném příkazu.

Následují příklady vytváření výrazů implicitně typované pole:
```csharp
var a = new[] { 1, 10, 100, 1000 };                       // int[]

var b = new[] { 1, 1.5, 2, 2.5 };                         // double[]

var c = new[,] { { "hello", null }, { "world", "!" } };   // string[,]

var d = new[] { 1, "one", 2, "two" };                     // Error
```

Posledního výrazu způsobí chybu kompilace, protože ani `int` ani `string` implicitně převést na druhé a tady je již nejlépe běžné zadejte. Výraz vytvoření pole explicitně musí použít v tomto případě třeba určující typ, který má být `object[]`. Můžete také jeden z prvků lze převést na běžné základní typ, který by pak můžou stát typ odvozený element.

Implicitně typovaná pole vytváření výrazů je možné kombinovat s inicializátory anonymních objektů ([anonymní objekt vytváření výrazů](expressions.md#anonymous-object-creation-expressions)) vytvořte anonymně zadané datové struktury. Příklad:
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

#### <a name="delegate-creation-expressions"></a>Výrazy vytvoření delegáta

A *delegate_creation_expression* umožňuje vytvořit novou instanci třídy *delegate_type*.

```antlr
delegate_creation_expression
    : 'new' delegate_type '(' expression ')'
    ;
```

Argument výraz vytvářející delegáta musí být skupinu metod, anonymní funkce nebo hodnota typ času kompilace `dynamic` nebo *delegate_type*. Pokud je argumentem skupinu metod, identifikuje metodu a pro metodu instance objektu, pro který chcete vytvořit delegáta. Pokud argument je anonymní funkce přímo definuje parametry a tělo metody delegáta cíle. Pokud je argument hodnota identifikuje instanci delegáta, které chcete vytvořit kopii.

Pokud *výraz* má typ kompilace `dynamic`, *delegate_creation_expression* dynamicky vázán ([dynamické vazby](expressions.md#dynamic-binding)) a následující pravidla platí se za běhu pomocí typu za běhu *výraz*. V opačném případě pravidla se použijí v době kompilace.

Vazba čas zpracování *delegate_creation_expression* formuláře `new D(E)`, kde `D` je *delegate_type* a `E` je *výraz* , se skládá z následujících kroků:

*  Pokud `E` je skupina metodu, výraz vytvoření delegáta je zpracována stejným způsobem jako převod skupin – metoda ([převody skupiny – metoda](conversions.md#method-group-conversions)) z `E` k `D`.
*  Pokud `E` je anonymní funkce, výraz vytvoření delegáta je zpracována stejným způsobem jako konverzi anonymní funkce ([anonymní funkce převody](conversions.md#anonymous-function-conversions)) z `E` k `D`.
*  Pokud `E` je hodnota, `E` musí být kompatibilní ([delegovat deklarace](delegates.md#delegate-declarations)) s `D`, a výsledkem je odkaz na nově vytvořený delegát typu `D` , který odkazuje na stejnou vyvolání jako seznam `E`. Pokud `E` není kompatibilní s `D`, dojde k chybě kompilace.

Zpracování za běhu *delegate_creation_expression* formuláře `new D(E)`, kde `D` je *delegate_type* a `E` je *výraz* , se skládá z následujících kroků:

*   Pokud `E` je skupina metodu, výraz vytvoření delegáta se vyhodnotí jako převod skupin – metoda ([převody skupiny – metoda](conversions.md#method-group-conversions)) z `E` k `D`.
*   Pokud `E` je anonymní funkce, vytvoření delegáta se vyhodnotí jako konverzi anonymní funkce z `E` k `D` ([anonymní funkce převody](conversions.md#anonymous-function-conversions)).
*   Pokud `E` . má hodnotu *delegate_type*:
    * `E` je vyhodnocen. Je-li toto vyhodnocení způsobí výjimku, jsou spuštěny žádné další kroky.
    * Pokud hodnota `E` je `null`, `System.NullReferenceException` je vyvolána, a že jsou provedeny žádné další kroky.
    * Nová instance typu delegáta `D` je přidělen. Pokud není k dispozici dostatek paměti k přidělení nové instance `System.OutOfMemoryException` je vyvolána, a že jsou provedeny žádné další kroky.
    * Novou instanci delegáta je inicializována pomocí seznamu vyvolání jako instanci delegáta Dal `E`.

Seznamu vyvolání delegáta je určena při delegáta je vytvořena instance a pak zůstává konstantní po celou dobu života delegáta. Jinými slovy není možné po vytvoření změnit volatelných entity cílového delegáta. Při dvou delegátů jsou zkombinované nebo jeden se odebere z jiného ([delegovat deklarace](delegates.md#delegate-declarations)), Nový delegát výsledky; nemá žádné existující delegáta jeho obsah změnit.

Není možné k vytvoření delegáta, který odkazuje na vlastnost, indexer, uživatelem definovaný operátor, konstruktor instance, destruktor nebo statický konstruktor.

Jak je popsáno výše, při vytvoření delegáta z metody skupiny, seznam formálních parametrů a návratový typ delegáta zjistit, které z přetížených metod k výběru. V příkladu
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
`A.f` pole je inicializována pomocí delegáta, který odkazuje na druhý `Square` metoda vzhledem k tomu, že tato metoda přesně odpovídá seznamu formálních parametrů a návratový typ `DoubleFunc`. Měl druhý `Square` metoda nebyly k dispozici, za kompilace by došlo k chybě.

#### <a name="anonymous-object-creation-expressions"></a>Anonymní objekt vytváření výrazů

*Anonymous_object_creation_expression* slouží k vytvoření objektu anonymního typu.

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

Inicializátor anonymních objektů deklaruje anonymního typu a vrátí instance daného typu. Anonymní typ není typem nameless třídy, který dědí přímo z `object`. Členové anonymního typu jsou posloupnost odvodit z inicializátoru anonymní objekt použitý k vytvoření instance typu vlastnosti jen pro čtení. Konkrétně inicializátor anonymních objektů ve formátu
```csharp
new { p1 = e1, p2 = e2, ..., pn = en }
```
deklaruje anonymního typu formuláře
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
kde každý `Tx` je typ odpovídající výraz `ex`. Výraz použitý v *member_declarator* musí být typu. Díky tomu se jedná o chybu kompilace výrazu v *member_declarator* na hodnotu null nebo je anonymní funkce. Také se jedná o chybu kompilace pro výraz unsafe typu.

Názvy anonymního typu a parametru k jeho `Equals` metody jsou automaticky generovány v kompilátoru a se nedá odkazovat v textu programu.

Ve stejném programu vytvoří dvě inicializátory anonymních objektů určující posloupnost vlastnosti stejné názvy a typy v době kompilace ve stejném pořadí výskyty stejného anonymního typu.

V příkladu
```csharp
var p1 = new { Name = "Lawnmower", Price = 495.00 };
var p2 = new { Name = "Shovel", Price = 26.95 };
p1 = p2;
```
přiřazení na posledním řádku je povolen, protože `p1` a `p2` jsou stejné anonymního typu.

`Equals` a `GetHashcode` metody anonymních typů přepište metody zděděné z `object`a jsou definovány z hlediska `Equals` a `GetHashcode` vlastností tak, aby dva výskyty stejného anonymního typu jsou si rovny Pokud a pouze v případě, že jejich vlastnosti jsou stejné.

Deklarátor členské lze zkrátit na jednoduchý název ([odvození typu](expressions.md#type-inference)), přístup ke členu ([kompilace kontrolu dynamické přetížení](expressions.md#compile-time-checking-of-dynamic-overload-resolution)), základní přístup ([základní přístup](expressions.md#base-access)) nebo člen null podmíněný přístup ([Null podmíněné výrazy jako inicializátory projekce](expressions.md#null-conditional-expressions-as-projection-initializers)). Tento postup se nazývá ***projekce inicializátor*** a představuje zkratku pro deklaraci a přiřazení k vlastnosti se stejným názvem. Konkrétně deklarátory členů formulářů
```csharp
identifier
expr.identifier
```
jsou přesně odpovídá následující příkaz, v uvedeném pořadí:
```csharp
identifier = identifier
identifier = expr.identifier
```

Proto v inicializátoru projekce *identifikátor* vybere hodnota a pole nebo vlastnost, ke kterému je přiřazena hodnota. Inicializátor projekce intuitivně, projekty, nejen hodnotu, ale také název hodnoty.

### <a name="the-typeof-operator"></a>Typeof – operátor

`typeof` Operátor se používá k získání `System.Type` pro typ objektu.

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

První formulář *typeof_expression* se skládá z `typeof` – klíčové slovo následované v závorce *typ*. Výsledkem výrazu tohoto formuláře je `System.Type` pro zvolený typ objektu. Existuje pouze jeden `System.Type` objekt daného typu. To znamená, že pro typ `T`, `typeof(T) == typeof(T)` má vždy hodnotu true. *Typ* nemůže být `dynamic`.

Tedy o druhou podobu *typeof_expression* se skládá z `typeof` – klíčové slovo následované v závorce *unbound_type_name*. *Unbound_type_name* je velmi podobný *type_name* ([Namespace a zadejte názvy](basic-concepts.md#namespace-and-type-names)) s tím rozdílem, že *unbound_type_name* obsahuje *generic_dimension_specifier*s kde *type_name* obsahuje *type_argument_list*s. Když operand *typeof_expression* je sekvence tokenů, který splňuje gramatiky obou *unbound_type_name* a *type_name*, zejména pokud obsahuje ani *generic_dimension_specifier* ani *type_argument_list*, posloupnost tokeny se považuje za *type_name*. Význam *unbound_type_name* je stanoven následujícím způsobem:

*  Převod sekvence tokenů *type_name* nahrazením každého *generic_dimension_specifier* s *type_argument_list* mají stejný počet čárkami a klíčové slovo `object` jako každý *type_argument*.
*  Vyhodnocení výsledný *type_name*, při ignoruje všechny omezeními parametrů typů.
*  *Unbound_type_name* přeloží na nevázaný parametr generického typu přidružené výsledný konstruovaný typ ([vázaný a nevázaných typy](types.md#bound-and-unbound-types)).

Výsledkem *typeof_expression* je `System.Type` objektu pro výsledný nevázaných obecného typu.

Třetí forma *typeof_expression* se skládá z `typeof` – klíčové slovo následované v závorce `void` – klíčové slovo. Výsledkem výrazu tohoto formuláře je `System.Type` objekt, který reprezentuje neexistence typu. Typ objektu vrácený `typeof(void)` se liší od typu objekt vrácený pro libovolného typu. Tento objekt speciální typ je užitečné v knihovnách tříd, umožňujících reflexe do metody v jazyce, ve kterém chcete způsob, jak reprezentaci návratový typ jakoukoli metodu, včetně void metody instance těchto metod `System.Type`.

`typeof` Operátor lze použít pro parametr typu. Výsledkem je `System.Type` objektu pro typ za běhu, která byla vázána na parametr typu. `typeof` Operátoru lze také na konstruovaný typ nebo nevázaný parametr generického typu ([vázaný a nevázaných typy](types.md#bound-and-unbound-types)). `System.Type` Objektu pro nevázaný parametr generického typu není stejný jako `System.Type` objekt typu instance. Typ instance je vždy uzavřený konstruovaný typ v době běhu tak jeho `System.Type` objektu závisí na argumenty typu modulu runtime používá, zatímco nevázaný parametr generického typu nemá žádné argumenty typu.

V příkladu
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
vytvoří následující výstup:
```
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

Všimněte si také, že výsledek `typeof(X<>)` není závislý na argument typu, ale výsledek `typeof(X<T>)` nepodporuje.

### <a name="the-checked-and-unchecked-operators"></a>Operátory zaškrtnuto a nezaškrtnuto

`checked` a `unchecked` operátory jsou slouží ke kontrole, ***kontextu kontroly přetečení*** integrálového typu aritmetické operace a převody.

```antlr
checked_expression
    : 'checked' '(' expression ')'
    ;

unchecked_expression
    : 'unchecked' '(' expression ')'
    ;
```

`checked` Operátor vyhodnotí uzavřeného výrazu ve zkontrolovaném kontextu a `unchecked` operátor vyhodnotí uzavřeného výrazu v nezkontrolovaném kontextu. A *checked_expression* nebo *unchecked_expression* přesně odpovídá *parenthesized_expression* ([výrazech se závorkami](expressions.md#parenthesized-expressions)), s tím rozdílem, že omezením výraz je vyhodnocen v dané kontroly kontextu přetečení.

Přetečení kontrola kontextu je možné řídit také prostřednictvím `checked` a `unchecked` příkazy ([příkazy zaškrtnuto a nezaškrtnuto](statements.md#the-checked-and-unchecked-statements)).

Tyto operace jsou ovlivněny kontroly kontextu stanovené přetečení `checked` a `unchecked` operátorů a příkazů:

*  Předdefinované `++` a `--` unárních operátorů ([Příponové operátory Inkrementace a dekrementace operátory](expressions.md#postfix-increment-and-decrement-operators) a [předpony Inkrementace a dekrementace operátory](expressions.md#prefix-increment-and-decrement-operators)), když je celočíselný operand Zadejte.
*  Předdefinované `-` unárního operátoru ([unární operátor minus](expressions.md#unary-minus-operator)), když je operand typu celé číslo.
*  Předdefinované `+`, `-`, `*`, a `/` binárních operátorů ([aritmetické operátory](expressions.md#arithmetic-operators)), pokud jsou oba operandy integrální typy.
*  Explicitních číselných převodů ([explicitních číselných převodů](conversions.md#explicit-numeric-conversions)) z jednoho celočíselného typu na jiný celočíselný typ nebo z `float` nebo `double` na celočíselný typ.

Při jedné z výše uvedených operací vytvoření výsledku, který je příliš velký pro reprezentaci typu cílového, kontext, ve které operaci provádí ovládací prvky výsledné chování:

*  V `checked` kontextu, pokud je operace konstantní výraz ([konstantní výrazy](expressions.md#constant-expressions)), dojde k chybě kompilace. Jinak, pokud se operace provádí za běhu, `System.OverflowException` je vyvolána výjimka.
*  V `unchecked` je rozdělená do kontextu, výsledek se zahodí všechny nejvyšším bity, které se nevejdou do cílového typu.

Pro nekonstantní výrazy (výrazy, které jsou vyhodnocovány v době běhu), které se nenacházejí žádné `checked` nebo `unchecked` operátory nebo příkazy, je výchozí přetečení kontrola kontextu `unchecked` Pokud externí faktory (jako je například kompilátor přepínače a konfiguraci prostředí spuštění) vyžadují `checked` hodnocení.

Pro konstantní výrazy (výrazy, které může být plně vyhodnocen v době kompilace), je vždy kontroly kontextu přetečení výchozí `checked`. Pokud není konstantní výraz se explicitně umístí do `unchecked` kontextu přetečení, ke kterým dochází při vyhodnocení za kompilace výrazu vždy způsobit chyby kompilace.

Není ovlivněna tělo anonymní funkce `checked` nebo `unchecked` kontextech, ve kterých dojde k anonymní funkce.

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
vzhledem k tomu, že ani jeden z výrazů může být vyhodnocen v době kompilace, jsou hlášeny žádné chyby kompilace. V době běhu `F` vyvolá metoda výjimku `System.OverflowException`a `G` metoda vrátí-727379968 (nižší 32 bitů výsledek mimo rozsah). Chování `H` metoda závisí na výchozí přetečení kontrola kontext kompilace, ale je buď stejná jako `F` nebo stejná jako `G`.

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
přetečení, ke kterým dochází při vyhodnocování konstantní výrazy u `F` a `H` způsobit chyby kompilace nahlásit, protože jsou výrazy vyhodnocovány v `checked` kontextu. Přetečení také vyvolá se při vyhodnocení konstantního výrazu v `G`, ale protože vyhodnocení se provádí `unchecked` kontextu, není hlášena přetečení.

`checked` a `unchecked` operátory ovlivní pouze kontroly kontext pro tyto operace, které jsou obsaženy pomocí textu v rámci přetečení "`(`"a"`)`" tokeny. Operátory nemají žádný vliv na funkce členy, které jsou vyvolány jako výsledek vyhodnocení výrazu omezením. V příkladu
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
použití `checked` v `F` nemá vliv na vyhodnocení `x * y` v `Multiply`, takže `x * y` je vyhodnocen v přetečení výchozí kontrola kontextu.

`unchecked` Operátor je vhodné při zápisu konstanty z podepsaných integrálních typů v šestnáctkové soustavě. Příklad:
```csharp
class Test
{
    public const int AllBits = unchecked((int)0xFFFFFFFF);

    public const int HighBit = unchecked((int)0x80000000);
}
```

Obě výše šestnáctkové konstanty jsou typu `uint`. Protože konstanty jsou mimo `int` v rozsahu, aniž by `unchecked` operátorů, přetypování na `int` byste mohli vytvořit chyby kompilace.

`checked` a `unchecked` operátorů a příkazů umožňují programátorům ovládat některé aspekty některé číselné výpočty. Chování některé numerické operátory však závisí na datové typy jeho operandů. Například vždy vynásobí dvě desetinná místa má za následek výjimku při přetečení i v rámci explicitně `unchecked` vytvořit. Obdobně součin dvou čísel s plovoucí čárkou nikdy výsledky v výjimku při přetečení i v rámci explicitně `checked` vytvořit. Kromě toho ostatní operátory jsou nikdy ovlivněny režimu kontroly, zda výchozí nebo explicitní.

### <a name="default-value-expressions"></a>Výrazy s výchozími hodnotami

Výraz výchozí hodnoty se používá k získání výchozí hodnota ([výchozí hodnoty](variables.md#default-values)) typu. Výraz výchozí hodnoty se obvykle používá pro parametry typu, protože nemusí být známé, pokud parametr typu je typ hodnoty nebo typ odkazu. (Neexistuje žádný převod z `null` literál na parametr typu. Pokud je parametr typu je znám jako typ odkazu.)

```antlr
default_value_expression
    : 'default' '(' type ')'
    ;
```

Pokud *typ* v *default_value_expression* vyhodnotí v době běhu na typ odkazu, výsledkem je `null` převést na typu. Pokud *typ* v *default_value_expression* vyhodnotí v době běhu na typ hodnoty, výsledkem je *value_type*výchozí hodnota ([výchozí Konstruktory](types.md#default-constructors)).

A *default_value_expression* je konstantní výraz ([konstantní výrazy](expressions.md#constant-expressions)) Pokud je typ typem odkazu nebo parametr typu, který je znám jako typ odkazu ([parametr typu omezení](classes.md#type-parameter-constraints)). Kromě toho *default_value_expression* je konstantní výraz, pokud je typ některý z následujících typů hodnot: `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, `decimal`, `bool`, nebo libovolný typ výčtu.


### <a name="nameof-expressions"></a>Výrazy Nameof

A *nameof_expression* slouží k získání název programu entity jako konstanty typu řetězec.

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

Gramaticky vzato *named_entity* operand je vždy výraz. Protože `nameof` není rezervované klíčové slovo, výraz nameof je vždy syntakticky dvojznačný pomocí volání jednoduchý název `nameof`. Kvůli kompatibilitě, pokud vyhledávání názvu ([jednoduché názvy](expressions.md#simple-names)) názvu `nameof` proběhne úspěšně, je považován za výraz *invocation_expression* – bez ohledu na to, zda je při vyvolání právní. V opačném případě se jedná *nameof_expression*.

Význam *named_entity* z *nameof_expression* znamená ho jako výraz; to znamená, buď jako *simple_name*, *base_access*  nebo *member_access*. Nicméně pokud vyhledávání je popsáno v [jednoduché názvy](expressions.md#simple-names) a [přístup ke členu](expressions.md#member-access) způsobí chybu, protože člen instance nebyla nalezena ve statickém kontextu *nameof_expression*vytváří žádné takové chyby.

Je chyba kompilace pro *named_entity* určit skupinu metod. Chcete-li mít *type_argument_list*. Jedná se chybu čas kompilace *named_entity_target* mít typ `dynamic`.

A *nameof_expression* je konstantní výraz typu `string`, a nemá žádný vliv za běhu. Konkrétně jeho *named_entity* , není hodnocena a je ignorován pro účely analýzy jednoznačného přiřazení ([obecná pravidla pro jednoduché výrazy](variables.md#general-rules-for-simple-expressions)). Její hodnota je poslední identifikátor *named_entity* před volitelné koncový *type_argument_list*, transformovaný následujícím způsobem:

* Předpona "`@`", pokud použijete, se odebere.
* Každý *unicode_escape_sequence* se transformuje na jeho odpovídající znak Unicode.
* Žádné *formatting_characters* se odeberou.

Jedná se o stejné transformace použita v [identifikátory](lexical-structure.md#identifiers) při testování rovnosti mezi identifikátory.

TODO: Příklady

### <a name="anonymous-method-expressions"></a>Výrazy anonymní metoda

*Anonymous_method_expression* je jedním ze dvou způsobů definování anonymní funkce. Ty jsou podrobně popsány v [výrazy anonymní funkce](expressions.md#anonymous-function-expressions).

## <a name="unary-operators"></a>Unární operátory

`?`, `+`, `-`, `!`, `~`, `++`, `--`, Vícesměrového vysílání, a `await` operátory označují jako unární operátory.

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

Pokud operand *unary_expression* má typ kompilace `dynamic`, je vázán dynamicky ([dynamické vazby](expressions.md#dynamic-binding)). V tomto případě typ době kompilace *unary_expression* je `dynamic`, a rozlišení je popsáno níže se provede v době běhu pomocí run-time typu operandu.

### <a name="null-conditional-operator"></a>Null – podmíněný operátor

Operátor podmíněného null seznam operací platí pro operand pouze v případě, že operand je jiná než null. Jinak je výsledek použití operátoru `null`.

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

Přístup ke členu a operací přístupu k elementu, (které mohou být samy podmíněných null), jakož i volání může obsahovat seznam operací.

Například výraz `a.b?[0]?.c()` je *null_conditional_expression* s *primary_expression* `a.b` a *null_conditional_operations* `?[0]` (přístup k prvkům podmíněných null), `?.c` (člen null podmíněný přístup) a `()` (vyvolání).

Pro *null_conditional_expression* `E` s *primary_expression* `P`, umožněte `E0` se výraz získaný pomocí textu odstraněním úvodního `?`ze všech *null_conditional_operations* z `E` , které mají jednu. Koncepčně `E0` je výraz, který bude vyhodnocen, pokud žádná z kontroly hodnoty null reprezentována `?`s najít `null`.

Navíc umožňují `E1` se výraz získaný pomocí textu odstraněním úvodního `?` z jenom na prvního *null_conditional_operations* v `E`. To může vést k *primární výraz* (pokud existuje pouze jedna `?`) nebo do jiného *null_conditional_expression*.

Například pokud `E` je výraz `a.b?[0]?.c()`, pak `E0` je výraz `a.b[0].c()` a `E1` je výraz `a.b[0]?.c()`.

Pokud `E0` klasifikovaný jako nothing, pak `E` klasifikovaný jako nothing. V opačném případě E je klasifikován jako hodnotu.

`E0` a `E1` slouží k určení význam `E`:

*  Pokud `E` vyskytuje se jako *statement_expression* význam `E` je stejný jako příkaz

   ```csharp
   if ((object)P != null) E1;
   ```

   s tím rozdílem, že P je vyhodnocen pouze jednou.

*  Jinak, pokud `E0` je klasifikován tak, co dojde k chybě kompilace.

*  V opačném případě nechat `T0` být typu `E0`.

   *  Pokud `T0` je parametr typu, který není známé jako odkaz na typ nebo hodnotu Null, dojde k chybě kompilace.

   *  Pokud `T0` typu hodnotu Null, je typu `E` je `T0?`a význam `E` je stejný jako

      ```csharp
      ((object)P == null) ? (T0?)null : E1
      ```

      s tím rozdílem, že `P` se vyhodnotí pouze jednou.

   *  V opačném případě je T0 typu E a význam E je stejný jako

      ```csharp
      ((object)P == null) ? null : E1
      ```

      s tím rozdílem, že `P` se vyhodnotí pouze jednou.

Pokud `E1` sama o sobě *null_conditional_expression*, pak tato pravidla se použijí znovu, vnoření testy pro `null` až nebudou existovat žádné další `?`společnosti, a zmenšili jsme výraz úplně dolů primární výraz `E0`.

Například pokud výraz `a.b?[0]?.c()` vyskytuje se jako příkaz výraz, stejně jako v příkazu:
```csharp
a.b?[0]?.c();
```
je ekvivalentní k jeho význam:
```csharp
if (a.b != null) a.b[0]?.c();
```
což znovu je totéž jako:
```csharp
if (a.b != null) if (a.b[0] != null) a.b[0].c();
```
S tím rozdílem, že `a.b` a `a.b[0]` se vyhodnocují jenom jednou.

Pokud k němu dojde v kontextu, pokud jeho hodnota se používá, například:
```csharp
var x = a.b?[0]?.c();
```
a za předpokladu, že typ posledním volání není typu hodnotu Null, je ekvivalentní k jeho význam:
```csharp
var x = (a.b == null) ? null : (a.b[0] == null) ? null : a.b[0].c();
```
S tím rozdílem, že `a.b` a `a.b[0]` se vyhodnocují jenom jednou.

#### <a name="null-conditional-expressions-as-projection-initializers"></a>Null podmíněné výrazy jako inicializátory projekce

Null – podmíněný výraz je povolen pouze jako *member_declarator* v *anonymous_object_creation_expression* ([anonymní objekt vytváření výrazů](expressions.md#anonymous-object-creation-expressions)) Pokud končí přístup ke členu (volitelně null podmíněné). Tento požadavek gramaticky, může být vyjádřený jako:

```antlr
null_conditional_member_access
    : primary_expression null_conditional_operations? '?' '.' identifier type_argument_list?
    | primary_expression null_conditional_operations '.' identifier type_argument_list?
    ;
```

Toto je zvláštní případ gramatiky *null_conditional_expression* výše. Produkčních prostředích *member_declarator* v [anonymní objekt vytváření výrazů](expressions.md#anonymous-object-creation-expressions) zahrnuje pouze *null_conditional_member_access*.

#### <a name="null-conditional-expressions-as-statement-expressions"></a>Null – podmíněné výrazy jako výrazy příkazu

Null – podmíněný výraz je povolen pouze jako *statement_expression* ([příkazy výrazů](statements.md#expression-statements)) Pokud končí vyvolání. Tento požadavek gramaticky, může být vyjádřený jako:

```antlr
null_conditional_invocation_expression
    : primary_expression null_conditional_operations '(' argument_list? ')'
    ;
```

Toto je zvláštní případ gramatiky *null_conditional_expression* výše. Produkčních prostředích *statement_expression* v [příkazy výrazů](statements.md#expression-statements) zahrnuje pouze *null_conditional_invocation_expression*.


### <a name="unary-plus-operator"></a>Jednočlenný operátor plus

Operace formuláře `+x`, unárního operátoru rozlišení přetěžování ([unární operátor rozlišení přetěžování](expressions.md#unary-operator-overload-resolution)) se použije k výběru na konkrétní operátor implementace. Operand je převeden na typ parametru zvoleném operátorovi a typ výsledku je návratový typ operátoru. Předdefinované Unární plus operátory jsou:

```csharp
int operator +(int x);
uint operator +(uint x);
long operator +(long x);
ulong operator +(ulong x);
float operator +(float x);
double operator +(double x);
decimal operator +(decimal x);
```

Pro každý z těchto operátorů výsledkem je jednoduše hodnota operandu.

### <a name="unary-minus-operator"></a>Unární operátor minus

Operace formuláře `-x`, unárního operátoru rozlišení přetěžování ([unární operátor rozlišení přetěžování](expressions.md#unary-operator-overload-resolution)) se použije k výběru na konkrétní operátor implementace. Operand je převeden na typ parametru zvoleném operátorovi a typ výsledku je návratový typ operátoru. Předdefinované negace operátory jsou:

*  Celé číslo negace:

   ```csharp
   int operator -(int x);
   long operator -(long x);
   ```

   Výsledkem je vypočítán odečtením `x` od nuly. Pokud hodnota `x` je nejmenší reprezentovatelnou hodnotu typu operandu (-2 ^ 31 pro `int` nebo -2 ^ 63 pro `long`), pak matematické negaci `x` není reprezentovatelná v typu operandu. Pokud k tomu dojde v rámci `checked` kontextu, `System.OverflowException` je vyvolána, pokud se vyskytuje v `unchecked` kontextu, výsledkem je hodnota operandu a není hlášena přetečení.

   Pokud je operand operátoru negace typu `uint`, je převeden na typ `long`, a typ výsledku je `long`. Výjimkou je pravidlo, které povoluje `int` hodnota -2147483648 (-2 ^ 31) má být zapsán jako desítkové celé číslo literálu ([literály celých čísel](lexical-structure.md#integer-literals)).

   Pokud je operand operátoru negace typu `ulong`, dojde k chybě kompilace. Výjimkou je pravidlo, které povoluje `long` hodnota -9223372036854775808 (-2 ^ 63) má být zapsán jako desítkové celé číslo literálu ([literály celých čísel](lexical-structure.md#integer-literals)).

*  S plovoucí desetinnou čárkou negace:

   ```csharp
   float operator -(float x);
   double operator -(double x);
   ```

   Výsledkem je hodnota `x` jeho symbolem obrácený. Pokud `x` NaN, je vrácená hodnota je také NaN.

*  Desetinné negace:

   ```csharp
   decimal operator -(decimal x);
   ```

   Výsledkem je vypočítán odečtením `x` od nuly. Je ekvivalentní k použití unární mínus – operátor typu Decimal negace `System.Decimal`.

### <a name="logical-negation-operator"></a>Logický operátor negace

Operace formuláře `!x`, unárního operátoru rozlišení přetěžování ([unární operátor rozlišení přetěžování](expressions.md#unary-operator-overload-resolution)) se použije k výběru na konkrétní operátor implementace. Operand je převeden na typ parametru zvoleném operátorovi a typ výsledku je návratový typ operátoru. Existuje pouze jeden předdefinovaný logický operátor negace:
```csharp
bool operator !(bool x);
```

Tento operátor vypočítá Logická negace operand: Pokud je operand `true`, výsledkem je `false`. Pokud je operand `false`, výsledkem je `true`.

### <a name="bitwise-complement-operator"></a>Operátor bitového doplňku

Operace formuláře `~x`, unárního operátoru rozlišení přetěžování ([unární operátor rozlišení přetěžování](expressions.md#unary-operator-overload-resolution)) se použije k výběru na konkrétní operátor implementace. Operand je převeden na typ parametru zvoleném operátorovi a typ výsledku je návratový typ operátoru. Předdefinované bitového doplňku operátory jsou:
```csharp
int operator ~(int x);
uint operator ~(uint x);
long operator ~(long x);
ulong operator ~(ulong x);
```

Pro každý z těchto operátorů je výsledek operace bitový doplněk `x`.

Každý typ výčtu `E` implicitně poskytuje následující operátor bitového doplňku:

```csharp
E operator ~(E x);
```

Výsledek vyhodnocení výrazu `~x`, kde `x` je výraz typu výčtu `E` s podkladovým typem `U`, je stejný jako vaše rozhodnutí vyzkoušet `(E)(~(U)x)`, s tím rozdílem, že převod `E` je vždy provést, protože pokud v `unchecked` kontextu ([operátory zaškrtnuto a nezaškrtnuto](expressions.md#the-checked-and-unchecked-operators)).

### <a name="prefix-increment-and-decrement-operators"></a>Předpona Inkrementace a dekrementace operátory

```antlr
pre_increment_expression
    : '++' unary_expression
    ;

pre_decrement_expression
    : '--' unary_expression
    ;
```

Operand operace předponu zvýšení nebo snížení musí být výraz jsou klasifikovány jako proměnné, přístup k vlastnosti nebo indexeru přístupu. Výsledkem operace je hodnota stejného typu jako operand.

Je-li zvýšit operand předponu operace dekrementace je vlastnost nebo indexer přístup, vlastnost nebo indexer musí mít obě `get` a `set` přistupujícího objektu. Pokud to není tento případ, dojde k chybě vazby čas.

Unární operátor rozlišení přetěžování ([unární operátor rozlišení přetěžování](expressions.md#unary-operator-overload-resolution)) se použije k výběru na konkrétní operátor implementace. Předdefinované `++` a `--` operátory existovat pro následující typy: `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char` , `float`, `double`, `decimal`a jakýkoli typ výčtu. Předdefinované `++` operátory návratová hodnota vytváří přidáním 1 operand a předdefinované `--` operátory návratová hodnota vytváří tak, že se 1 z operand. V `checked` kontextu, pokud výsledek této sčítání a odčítání je mimo rozsah typu výsledku a typ výsledku je integrální typ nebo typ výčtu `System.OverflowException` je vyvolána výjimka.

Zpracování za běhu předponu Inkrementace nebo dekrementace operace formuláře `++x` nebo `--x` se skládá z následujících kroků:

*   Pokud `x` je klasifikován jako proměnnou:
    * `x` je vyhodnocen pro vytvoření proměnné.
    * S použitím hodnoty z je vyvolána zvoleném operátorovi `x` jako svůj argument.
    * Hodnota vrácená operátorem je uložen v umístění Dal vyhodnocení `x`.
    * Hodnota vrácená operátorem změní výsledek operace.
*   Pokud `x` klasifikovaný jako vlastnost nebo indexovací člen přístupu:
    * Výraz instance (Pokud `x` není `static`) a v seznamu argumentů (Pokud `x` představuje přístup k indexer) přidružené k `x` jsou vyhodnocovány, a výsledky se používají v následné `get` a `set` přístupový objekt volání.
    * `get` Přistupující objekt `x` je vyvolána.
    * Vybraný operátor je volána s hodnotou vrácenou `get` přístupového objektu jako svůj argument.
    * `set` Přistupující objekt `x` vyvolání hodnota vrácená operátorem jako jeho `value` argument.
    * Hodnota vrácená operátorem změní výsledek operace.

`++` a `--` také podporují Příponové operátory zápis ([Příponové operátory Inkrementace a dekrementace operátory](expressions.md#postfix-increment-and-decrement-operators)). Obvykle, výsledek `x++` nebo `x--` je hodnota `x` před provedením operace, zatímco výsledek `++x` nebo `--x` je hodnota `x` po provedení této operace. V obou případech `x` sám po provedení této operace má stejnou hodnotu.

`operator++` Nebo `operator--` implementace lze vyvolat pomocí uplatněna přípona nebo předpona zápisu. Není možné mít samostatné operátor implementace pro zápisy dva.

### <a name="cast-expressions"></a>Výrazy přetypování

A *cast_expression* slouží k explicitnímu převodu výrazu na daného typu.

```antlr
cast_expression
    : '(' type ')' unary_expression
    ;
```

A *cast_expression* formuláře `(T)E`, kde `T` je *typ* a `E` je *unary_expression*, provádí explicitní převod ([explicitních převodů](conversions.md#explicit-conversions)) hodnoty `E` na typ `T`. Pokud neexistuje žádný explicitní převod z `E` k `T`, dojde k chybě vazby čas. Jinak výsledkem je hodnota vytvářených explicitní převod. Výsledek je vždy klasifikován jako hodnotu, i když `E` označuje proměnnou.

Gramatika *cast_expression* vede k určité syntaktické nejednoznačnosti. Například výraz `(x)-y` buď možné interpretovat jako *cast_expression* (přetypování z `-y` na typ `x`) nebo jako *additive_expression* v kombinaci s *parenthesized_expression* (které vypočítá hodnotu `x - y)`.

Chcete-li vyřešit *cast_expression* nejasnosti, existuje následující pravidlo: Pořadí jednoho nebo více *token*s ([prázdné znaky](lexical-structure.md#white-space)) uzavřený v závorkách je považován za spuštění *cast_expression* pouze v případě, že platí alespoň jedna z následujících akcí:

*  Sekvence tokenů je správná gramatika pro *typ*, ale ne pro *výraz*.
*  Posloupnost tokeny je správná gramatika pro *typ*, a hned za pravými závorkami token je token "`~`", token "`!`", token "`(`",  *identifikátor* ([znak – řídicí sekvence Unicode](lexical-structure.md#unicode-character-escape-sequences)), *literálu* ([literály](lexical-structure.md#literals)), nebo všechny *– klíčové slovo*([Klíčová slova](lexical-structure.md#keywords)) s výjimkou `as` a `is`.

Termín "správná gramatika" nad znamená pouze to, že posloupnost tokeny musí odpovídat konkrétní gramatické produkčního prostředí. To konkrétně nebere v úvahu skutečné význam všechny základní identifikátory. Například pokud `x` a `y` jsou identifikátory, pak `x.y` je správná gramatika pro typ, i v případě `x.y` není ve skutečnosti označení typu.

Z pravidla mnohoznačnosti vyplývá, že pokud `x` a `y` jsou identifikátory, `(x)y`, `(x)(y)`, a `(x)(-y)` jsou *cast_expression*s, ale `(x)-y` není, i v případě `x` identifikuje typ. Ale pokud `x` je klíčové slovo, který identifikuje předdefinovaný typ (například `int`), pak se všechny čtyři způsoby *cast_expression*s (protože takové klíčové slovo nebylo možné pravděpodobně výraz samostatně).

### <a name="await-expressions"></a>Výrazy await

Operátor await se používá k vyhodnocení nadřazené asynchronní funkce pozastavit až do dokončení asynchronní operace reprezentovány operandem.

```antlr
await_expression
    : 'await' unary_expression
    ;
```

*Await_expression* je povolený jenom v těle asynchronní funkci ([iterátory](classes.md#iterators)). V rámci nejbližšího obklopujícího asynchronní funkce *await_expression* nelze provádět v těchto umístěních:

*  Uvnitř anonymní funkce. vnořené (bez asynchronní)
*  Uvnitř bloku *lock_statement*
*  V nezabezpečeném kontextu.

Všimněte si, že *await_expression* nemůže nastat ve většině případů v rámci *query_expression*, protože ty jsou syntakticky transformuje na použití jiných asynchronní výrazy lambda.

V asynchronní funkci `await` nelze použít jako identifikátor. Proto neexistuje žádná syntaktická byla zjištěna dvojznačnost mezi výrazy await a různé výrazy zahrnující identifikátory. Mimo asynchronních funkcí `await` funguje jako normální identifikátor.

Operand *await_expression* je volána ***úloh***. Představuje asynchronní operaci, která může nebo nemusí být v době dokončení *await_expression* vyhodnocena. Je účel operátor await k pozastavení provádění nadřazené funkce asynchronní, dokud není dokončen očekávaný úkol a získat jeho výsledek.

#### <a name="awaitable-expressions"></a>Očekávatelný výrazy

Úloha výraz await musí být ***očekávatelný***. Výraz `t` může používat await, obsahuje jeden z následujících akcí:

*  `t` je typu v době kompilace `dynamic`
*  `t` má dostupné instance nebo rozšiřující metoda volá `GetAwaiter` bez parametrů a žádné parametry typu a návratový typ `A` pro které všechny tyto uchování:
   * `A` implementuje rozhraní `System.Runtime.CompilerServices.INotifyCompletion` (nazývaným jako `INotifyCompletion` pro zkrácení)
   * `A` má vlastnost instance přístupné, čitelné `IsCompleted` typu `bool`
   * `A` je dostupná metoda instance `GetResult` bez parametrů a žádné parametry typu

Účelem `GetAwaiter` metodou je použít k získání ***awaiter*** pro úlohu. Typ `A` je volána ***awaiter typ*** pro výraz await.

Účelem `IsCompleted` vlastnost je určit, pokud úloha je již dokončena. Pokud ano, není nutné pozastavit hodnocení.

Účelem `INotifyCompletion.OnCompleted` metoda, je registrace "pokračování" k úkolu; to znamená delegáta (typu `System.Action`), která bude volána po dokončení úlohy.

Účelem `GetResult` metodou je získat výsledek úkolu, jakmile je dokončená. Tento výsledek může být úspěšné dokončení, případně se je výsledná hodnota, nebo může být výjimku, která je vyvolána `GetResult` metody.

#### <a name="classification-of-await-expressions"></a>Výrazy await klasifikace

Výraz `await t` klasifikovaný stejným způsobem jako výraz `(t).GetAwaiter().GetResult()`. Proto pokud návratový typ `GetResult` je `void`, *await_expression* klasifikovaný jako nothing. Pokud má návratový typ jiný než void `T`, *await_expression* klasifikovaný jako hodnotu typu `T`.

#### <a name="runtime-evaluation-of-await-expressions"></a>Výrazy await vyhodnocení modulu runtime

Za běhu, výraz `await t` se vyhodnotí takto:

*  Awaiter `a` je získanou vyhodnocením výrazu `(t).GetAwaiter()`.
*  A `bool` `b` je získanou vyhodnocením výrazu `(a).IsCompleted`.
*  Pokud `b` je `false` pak vyhodnocení závisí na tom, zda `a` implementuje rozhraní `System.Runtime.CompilerServices.ICriticalNotifyCompletion` (nazývaným jako `ICriticalNotifyCompletion` pro zkrácení). Tato kontrola se provádí na vazby čas; například v době běhu Pokud `a` má typ času kompilace `dynamic`a v době kompilace, jinak. Umožní `r` označení delegát pokračování ([iterátory](classes.md#iterators)):
    * Pokud `a` neimplementuje `ICriticalNotifyCompletion`, pak výraz `(a as (INotifyCompletion)).OnCompleted(r)` vyhodnocena.
    * Pokud `a` implementovat `ICriticalNotifyCompletion`, pak výraz `(a as (ICriticalNotifyCompletion)).UnsafeOnCompleted(r)` vyhodnocena.
    * Vyhodnocení pak pozastavena a ovládací prvek se vrátí aktuální volajícímu asynchronní funkci.
*  Buď ihned po (Pokud `b` byl `true`), nebo na novější vyvolání delegáta obnovení činnosti (Pokud `b` byl `false`), výraz `(a).GetResult()` vyhodnocena. Pokud vrátí hodnotu, je tato hodnota výsledku *await_expression*. Výsledek jinak má hodnotu nothing.

Awaiter implementaci metody rozhraní `INotifyCompletion.OnCompleted` a `ICriticalNotifyCompletion.UnsafeOnCompleted` by se měl delegáta `r` má být volána nejvýše jednou. V opačném případě nadřazené funkce asynchronní chování není definováno.

## <a name="arithmetic-operators"></a>Aritmetické operátory

`*`, `/`, `%`, `+`, A `-` operátory jsou volány aritmetické operátory.

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

Pokud operand aritmetický operátor má typ kompilace `dynamic`, pak je dynamicky vázán výraz ([dynamické vazby](expressions.md#dynamic-binding)). V tomto případě je kompilaci typu výrazu `dynamic`, a rozlišení je popsáno níže se provede v době běhu pomocí run-time typu těchto operandů, která mají typ kompilace `dynamic`.

### <a name="multiplication-operator"></a>Operátor násobení

Operace formuláře `x * y`, binárním operátorem rozlišení přetěžování ([binárním operátorem rozlišení přetěžování](expressions.md#binary-operator-overload-resolution)) se použije k výběru na konkrétní operátor implementace. Operandy jsou převedeny na zvoleném operátorovi typy parametrů a typ výsledku je návratový typ operátoru.

Operátory násobení předdefinované jsou uvedeny níže. Všechny operátory vypočítat součin `x` a `y`.

*  Násobení celé číslo:

   ```csharp
   int operator *(int x, int y);
   uint operator *(uint x, uint y);
   long operator *(long x, long y);
   ulong operator *(ulong x, ulong y);
   ```

   V `checked` kontextu, pokud je mimo rozsah typu výsledku, `System.OverflowException` je vyvolána výjimka. V `unchecked` kontextu, nezobrazují, přetečení a žádné významné implikovaný bits mimo rozsah typu výsledku se zahodí.


*  Násobení s plovoucí desetinnou čárkou:

   ```csharp
   float operator *(float x, float y);
   double operator *(double x, double y);
   ```

   Produkt se počítá podle pravidel objektů aritmetické IEEE 754. V následující tabulce jsou uvedeny výsledky všech možných kombinací nenulové hodnoty konečné, nul, nekonečno a NaN. V tabulce `x` a `y` jsou konečné kladné hodnoty. `z` je výsledkem `x * y`. Pokud je výsledek příliš velký pro cílový typ `z` je nekonečno. Pokud je výsledek příliš malá pro cílový typ `z` je nula.

   |      |      |      |     |     |      |      |     |
   |:----:|-----:|:----:|:---:|:---:|:----:|:----:|:----|
   |      | + y   | -y   | +0  | -0  | + inf | -inf | NaN | 
   | +x   | +z   | -z   | +0  | -0  | + inf | -inf | NaN | 
   | -x   | -z   | +z   | -0  | +0  | -inf | + inf | NaN | 
   | +0   | +0   | -0   | +0  | -0  | NaN  | NaN  | NaN | 
   | -0   | -0   | +0   | -0  | +0  | NaN  | NaN  | NaN | 
   | + inf | + inf | -inf | NaN | NaN | + inf | -inf | NaN | 
   | -inf | -inf | + inf | NaN | NaN | -inf | + inf | NaN | 
   | NaN  | NaN  | NaN  | NaN | NaN | NaN  | NaN  | NaN | 

*  Desetinné násobení:

   ```csharp
   decimal operator *(decimal x, decimal y);
   ```

   Pokud výsledná hodnota je příliš velká pro reprezentaci v jazyce `decimal` formátu, `System.OverflowException` je vyvolána výjimka. Pokud hodnota výsledku je příliš malá pro reprezentaci v jazyce `decimal` formátu, výsledek je nula. Škálování výsledek před všechny zaokrouhlení je součtem váhy dva operandy.

   Je ekvivalentní k použití operátor násobení typu Decimal násobení `System.Decimal`.


### <a name="division-operator"></a>Operátor dělení

Operace formuláře `x / y`, binárním operátorem rozlišení přetěžování ([binárním operátorem rozlišení přetěžování](expressions.md#binary-operator-overload-resolution)) se použije k výběru na konkrétní operátor implementace. Operandy jsou převedeny na zvoleném operátorovi typy parametrů a typ výsledku je návratový typ operátoru.

Předdefinované dělení operátory jsou uvedeny níže. Všechny operátory compute podíl `x` a `y`.

*  Dělení celého čísla:

   ```csharp
   int operator /(int x, int y);
   uint operator /(uint x, uint y);
   long operator /(long x, long y);
   ulong operator /(ulong x, ulong y);
   ```

   Pokud hodnota pravého operandu je nula, `System.DivideByZeroException` je vyvolána výjimka.

   Rozdělení zaokrouhlí výsledek směrem k nule. Proto absolutní hodnota výsledku je největší možné číslo, které je menší než absolutní hodnota kvocientu dva operandy. Výsledkem může být nula nebo pozitivní při dva operandy mají stejné znaménko a nula nebo záporná, pokud mají dva operandy opačný příznaky.

   Pokud levý operand je nejmenší reprezentovatelné `int` nebo `long` hodnotu a pravý operand je `-1`, dojde k přetečení. V `checked` kontextu, způsobí to, že `System.ArithmeticException` (nebo jejich podtřída) Chcete-li být vyvolána. V `unchecked` kontextu, je definováno implementací, zda `System.ArithmeticException` (nebo jejich podtřída) je vyvolána nebo přetečení doprovází neohlášených výslednou hodnotu, přičemž levého operandu.

*  Dělení s pohyblivou čárkou:

   ```csharp
   float operator /(float x, float y);
   double operator /(double x, double y);
   ```

   Podíl se počítá podle pravidel objektů aritmetické IEEE 754. V následující tabulce jsou uvedeny výsledky všech možných kombinací nenulové hodnoty konečné, nul, nekonečno a NaN. V tabulce `x` a `y` jsou konečné kladné hodnoty. `z` je výsledkem `x / y`. Pokud je výsledek příliš velký pro cílový typ `z` je nekonečno. Pokud je výsledek příliš malá pro cílový typ `z` je nula.

   |      |      |      |      |      |      |      |      |
   |:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|
   |      | + y   | -y   | +0   | -0   | + inf | -inf | NaN  | 
   | +x   | +z   | -z   | + inf | -inf | +0   | -0   | NaN  | 
   | -x   | -z   | +z   | -inf | + inf | -0   | +0   | NaN  | 
   | +0   | +0   | -0   | NaN  | NaN  | +0   | -0   | NaN  | 
   | -0   | -0   | +0   | NaN  | NaN  | -0   | +0   | NaN  | 
   | + inf | + inf | -inf | + inf | -inf | NaN  | NaN  | NaN  | 
   | -inf | -inf | + inf | -inf | + inf | NaN  | NaN  | NaN  | 
   | NaN  | NaN  | NaN  | NaN  | NaN  | NaN  | NaN  | NaN  | 

*  Dělení desetinného čísla:

   ```csharp
   decimal operator /(decimal x, decimal y);
   ```

   Pokud hodnota pravého operandu je nula, `System.DivideByZeroException` je vyvolána výjimka. Pokud výsledná hodnota je příliš velká pro reprezentaci v jazyce `decimal` formátu, `System.OverflowException` je vyvolána výjimka. Pokud hodnota výsledku je příliš malá pro reprezentaci v jazyce `decimal` formátu, výsledek je nula. Škálování výsledek je nejmenší měřítko, které budou zachovány výsledek rovná nejbližší reprezentovatelné desítkovou hodnotu na true matematické výsledek.

   Dělení desetinného čísla je ekvivalentní k použití operátor dělení typu `System.Decimal`.


### <a name="remainder-operator"></a>Operátor zbytku

Operace formuláře `x % y`, binárním operátorem rozlišení přetěžování ([binárním operátorem rozlišení přetěžování](expressions.md#binary-operator-overload-resolution)) se použije k výběru na konkrétní operátor implementace. Operandy jsou převedeny na zvoleném operátorovi typy parametrů a typ výsledku je návratový typ operátoru.

Předdefinované zbývající operátory jsou uvedeny níže. Všechny operátory vypočítat zbývající části rozdělení mezi `x` a `y`.

*  Zbývající celé číslo:

   ```csharp
   int operator %(int x, int y);
   uint operator %(uint x, uint y);
   long operator %(long x, long y);
   ulong operator %(ulong x, ulong y);
   ```

   Výsledek `x % y` hodnota vytvořil `x - (x / y) * y`. Pokud `y` je nula, `System.DivideByZeroException` je vyvolána výjimka.

   Pokud levý operand je nejmenší `int` nebo `long` hodnotu a pravý operand je `-1`, `System.OverflowException` je vyvolána výjimka. V žádném případě nemá `x % y` vyvolání výjimky, kde `x / y` nemusí vyvolat výjimku.

*  Zbytek s plovoucí desetinnou čárkou:

   ```csharp
   float operator %(float x, float y);
   double operator %(double x, double y);
   ```

   V následující tabulce jsou uvedeny výsledky všech možných kombinací nenulové hodnoty konečné, nul, nekonečno a NaN. V tabulce `x` a `y` jsou konečné kladné hodnoty. `z` je výsledkem `x % y` a je vypočítán jako `x - n * y`, kde `n` je největší možné číslo, které je menší než nebo rovna hodnotě `x / y`. Tato metoda výpočetních zbytek je obdobou, který používá pro celočíselné operandy, ale se liší od definice IEEE 754 (ve kterém `n` je celé číslo nejblíže `x / y`).

   |      |      |      |      |      |      |      |      |
   |:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|
   |      | + y   | -y   | +0   | -0   | + inf | -inf | NaN  | 
   | +x   | +z   | +z   | NaN  | NaN  | x    | x    | NaN  | 
   | -x   | -z   | -z   | NaN  | NaN  | -x   | -x   | NaN  | 
   | +0   | +0   | +0   | NaN  | NaN  | +0   | +0   | NaN  | 
   | -0   | -0   | -0   | NaN  | NaN  | -0   | -0   | NaN  | 
   | + inf | NaN  | NaN  | NaN  | NaN  | NaN  | NaN  | NaN  | 
   | -inf | NaN  | NaN  | NaN  | NaN  | NaN  | NaN  | NaN  | 
   | NaN  | NaN  | NaN  | NaN  | NaN  | NaN  | NaN  | NaN  | 

*  Desetinné zbývající:

   ```csharp
   decimal operator %(decimal x, decimal y);
   ```

   Pokud hodnota pravého operandu je nula, `System.DivideByZeroException` je vyvolána výjimka. Škálování výsledek před všechny zaokrouhlení větší stupnic dva operandy, a znaménko výsledku platí, pokud nenulová, je stejné jako u `x`.

   Je ekvivalentní k použití operátor zbytku typu Decimal zbytek `System.Decimal`.


### <a name="addition-operator"></a>Operátor sčítání

Operace formuláře `x + y`, binárním operátorem rozlišení přetěžování ([binárním operátorem rozlišení přetěžování](expressions.md#binary-operator-overload-resolution)) se použije k výběru na konkrétní operátor implementace. Operandy jsou převedeny na zvoleném operátorovi typy parametrů a typ výsledku je návratový typ operátoru.

Operátory sčítání předdefinované jsou uvedeny níže. Operátory sčítání předdefinované číselné a výčet typů, nalezení součtu dva operandy. Pokud jeden nebo oba operandy jsou typu String, zřetězení operátory sčítání předdefinované řetězcové vyjádření operandy.

*  Přidání celé číslo:

   ```csharp
   int operator +(int x, int y);
   uint operator +(uint x, uint y);
   long operator +(long x, long y);
   ulong operator +(ulong x, ulong y);
   ```

   V `checked` kontextu, pokud součet je mimo rozsah typu výsledku, `System.OverflowException` je vyvolána výjimka. V `unchecked` kontextu, nezobrazují, přetečení a žádné významné implikovaný bits mimo rozsah typu výsledku se zahodí.

*  Přidání s plovoucí desetinnou čárkou:

   ```csharp
   float operator +(float x, float y);
   double operator +(double x, double y);
   ```

   Součet je vypočítán podle pravidel objektů aritmetické IEEE 754. V následující tabulce jsou uvedeny výsledky všech možných kombinací nenulové hodnoty konečné, nul, nekonečno a NaN. V tabulce `x` a `y` jsou omezené nenulové hodnoty, a `z` je výsledkem `x + y`. Pokud `x` a `y` mají stejnou velikost, ale opačným znaménka, `z` je kladné nula. Pokud `x + y` je příliš velký pro reprezentaci typu cílového `z` je nekonečno s stejné znaménko jako `x + y`.

   |      |      |      |      |      |      |      |
   |:----:|:----:|:----:|:----:|:----:|:----:|:----:|
   |      | y    | +0   | -0   | + inf | -inf | NaN  | 
   | x    | z    | x    | x    | + inf | -inf | NaN  | 
   | +0   | y    | +0   | +0   | + inf | -inf | NaN  | 
   | -0   | y    | +0   | -0   | + inf | -inf | NaN  | 
   | + inf | + inf | + inf | + inf | + inf | NaN  | NaN  | 
   | -inf | -inf | -inf | -inf | NaN  | -inf | NaN  | 
   | NaN  | NaN  | NaN  | NaN  | NaN  | NaN  | NaN  | 

*  Desetinné přidání:

   ```csharp
   decimal operator +(decimal x, decimal y);
   ```

   Pokud výsledná hodnota je příliš velká pro reprezentaci v jazyce `decimal` formátu, `System.OverflowException` je vyvolána výjimka. Škálování výsledek před všechny zaokrouhlení, je větší stupnic dva operandy.

   Je ekvivalentní k použití operátoru sčítání typu Decimal přidání `System.Decimal`.

*  Přidání výčtu. Každý typ výčtu implicitně poskytuje následující předdefinované operátory, kde `E` je typ výčtu a `U` je základní typ `E`:

   ```csharp
   E operator +(E x, U y);
   E operator +(U x, E y);
   ```

   V době běhu těchto operátorů jsou vyhodnocovány stejným způsobem jako `(E)((U)x + (U)y)`.

*  Zřetězení řetězců:

   ```csharp
   string operator +(string x, string y);
   string operator +(string x, object y);
   string operator +(object x, string y);
   ```

   Tato přetížení binárního souboru `+` provádět operátor zřetězení řetězců. Pokud je operand zřetězení řetězců `null`, je nahrazen prázdný řetězec. V opačném případě je některý argument jiné než řetězec převést na jeho řetězcovou reprezentaci vyvoláním virtuální `ToString` metody zděděné z typu `object`. Pokud `ToString` vrátí `null`, je nahrazen prázdný řetězec.

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

   Výsledek operátoru pro zřetězení řetězců je řetězec, který se skládá ze znaků levý operand, následované znaky pravého operandu. Nikdy nevrátí operátoru pro zřetězení řetězců `null` hodnotu. A `System.OutOfMemoryException` může být vyvolána, pokud není k dispozici dostatek paměti k přidělení výsledný řetězec.

*  Kombinaci delegátů. Každý typ delegáta implicitně poskytuje následující předdefinovaný operátor, kde `D` je typ delegátu:

   ```csharp
   D operator +(D x, D y);
   ```

   Binární soubor `+` operátor provádí delegátů, pokud jsou oba operandy typu delegáta `D`. (Pokud operandy různých delegáta typy, dojde k chybě vazby čas.) Pokud je první operand `null`, výsledkem operace je hodnota druhého operandu (i v případě, že to je také `null`). Jinak, pokud je druhý operand `null`, pak výsledek operace hodnotu prvního operandu. Výsledek operace v opačném případě se nový delegát instance, která při vyvolání, vyvolá prvním operandem a poté vyvolá druhého operandu. Příklady delegátů, najdete v článku [operátor odčítání](expressions.md#subtraction-operator) a [delegovat vyvolání](delegates.md#delegate-invocation). Protože `System.Delegate` není typ delegáta, `operator`  `+` není definována.

### <a name="subtraction-operator"></a>Operátor odčítání

Operace formuláře `x - y`, binárním operátorem rozlišení přetěžování ([binárním operátorem rozlišení přetěžování](expressions.md#binary-operator-overload-resolution)) se použije k výběru na konkrétní operátor implementace. Operandy jsou převedeny na zvoleném operátorovi typy parametrů a typ výsledku je návratový typ operátoru.

Předdefinované odčítání operátory jsou uvedeny níže. Operátory všechny odečíst `y` z `x`.

*  Odčítání celé číslo:

   ```csharp
   int operator -(int x, int y);
   uint operator -(uint x, uint y);
   long operator -(long x, long y);
   ulong operator -(ulong x, ulong y);
   ```

   V `checked` kontextu, pokud rozdíl je mimo rozsah typu výsledku, `System.OverflowException` je vyvolána výjimka. V `unchecked` kontextu, nezobrazují, přetečení a žádné významné implikovaný bits mimo rozsah typu výsledku se zahodí.

*  Odčítání s plovoucí desetinnou čárkou:

   ```csharp
   float operator -(float x, float y);
   double operator -(double x, double y);
   ```

   Rozdíl je vypočítán podle pravidel objektů aritmetické IEEE 754. V následující tabulce jsou uvedeny výsledky všech možných kombinací nenulové hodnoty konečné, nul, nekonečno a hodnoty NaN. V tabulce `x` a `y` jsou omezené nenulové hodnoty, a `z` je výsledkem `x - y`. Pokud `x` a `y` jsou si rovny, `z` je kladné nula. Pokud `x - y` je příliš velký pro reprezentaci typu cílového `z` je nekonečno s stejné znaménko jako `x - y`.

   |      |      |      |      |      |      |     |
   |:----:|:----:|:----:|:----:|:----:|:----:|:---:|
   |      | y    | +0   | -0   | + inf | -inf | NaN | 
   | x    | z    | x    | x    | -inf | + inf | NaN | 
   | +0   | -y   | +0   | +0   | -inf | + inf | NaN | 
   | -0   | -y   | -0   | +0   | -inf | + inf | NaN | 
   | + inf | + inf | + inf | + inf | NaN  | + inf | NaN | 
   | -inf | -inf | -inf | -inf | -inf | NaN  | NaN | 
   | NaN  | NaN  | NaN  | NaN  | NaN  | NaN  | NaN | 

*  Desetinné odčítání:

   ```csharp
   decimal operator -(decimal x, decimal y);
   ```

   Pokud výsledná hodnota je příliš velká pro reprezentaci v jazyce `decimal` formátu, `System.OverflowException` je vyvolána výjimka. Škálování výsledek před všechny zaokrouhlení, je větší stupnic dva operandy.

   Je ekvivalentní k použití operátoru odčítání typu Decimal odčítání `System.Decimal`.

*  Výčet odčítání. Každý typ výčtu implicitně poskytuje následující předdefinovaný operátor, kde `E` je typ výčtu a `U` je základní typ `E`:

   ```csharp
   U operator -(E x, E y);
   ```

   Tento operátor se vyhodnotí přesně jako `(U)((U)x - (U)y)`. Jinými slovy, operátor, který vypočítá rozdíl mezi pořadové hodnoty `x` a `y`, a typ výsledku je základní typ výčtu.

   ```csharp
   E operator -(E x, U y);
   ```

   Tento operátor se vyhodnotí přesně jako `(E)((U)x - y)`. Jinými slovy operátor, který odečte hodnotu od základní typ výčtu, což má za následek hodnotu výčtu.

*  Odebrání delegovat. Každý typ delegáta implicitně poskytuje následující předdefinovaný operátor, kde `D` je typ delegátu:

   ```csharp
   D operator -(D x, D y);
   ```

   Binární soubor `-` operátor provádí odebrání delegátů, pokud jsou oba operandy typu delegáta `D`. Pokud operandy typů různých delegátů, dojde k chybě vazby čas. Pokud je první operand `null`, je výsledek operace `null`. Jinak, pokud je druhý operand `null`, pak výsledek operace hodnotu prvního operandu. V opačném případě oba operandy představovat vyvolání seznamy ([delegovat deklarace](delegates.md#delegate-declarations)) má jednu nebo více položek a výsledkem je nový seznam vyvolání skládající se z prvního operandu seznam s položkami druhého operandu odebrán z je k dispozici seznam Druhý operand je správné souvislých podseznam při prvním.     (K určení rovnosti podseznam, jsou porovnány odpovídající položky jako operátor rovnosti delegáta ([delegovat operátory rovnosti](expressions.md#delegate-equality-operators)).) Jinak výsledkem je hodnota levého operandu. Ani jeden z operandů Zobrazí se změnilo v procesu. Pokud je druhý operand seznamu odpovídá několika dílčích souvislých položky v seznamu je první operand, odpovídající dílčí seznam úplně vpravo souvislých položek, které se odebere. Pokud je odebrání výsledkem je seznam prázdný, výsledkem je `null`. Příklad:

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

`<<` a `>>` operátory jsou používány k provádění operací posunu bit.

```antlr
shift_expression
    : additive_expression
    | shift_expression '<<' additive_expression
    | shift_expression right_shift additive_expression
    ;
```

Pokud operand operátoru *shift_expression* má typ kompilace `dynamic`, a dynamicky je vázán výraz ([dynamické vazby](expressions.md#dynamic-binding)). V tomto případě je kompilaci typu výrazu `dynamic`, a rozlišení je popsáno níže se provede v době běhu pomocí run-time typu těchto operandů, která mají typ kompilace `dynamic`.

Operace formuláře `x << count` nebo `x >> count`, binárním operátorem rozlišení přetěžování ([binárním operátorem rozlišení přetěžování](expressions.md#binary-operator-overload-resolution)) se použije k výběru na konkrétní operátor implementace. Operandy jsou převedeny na zvoleném operátorovi typy parametrů a typ výsledku je návratový typ operátoru.

Při deklarování přetěžovaného operátoru shift, typ je první operand musí být vždy dané třídy nebo struktury obsahující deklarace operátoru a typu Druhý operand musí být vždy `int`.

Operátory posunutí předdefinované jsou uvedeny níže.

*  Posun doleva:

   ```csharp
   int operator <<(int x, int count);
   uint operator <<(uint x, int count);
   long operator <<(long x, int count);
   ulong operator <<(ulong x, int count);
   ```

   `<<` Operátor staffhubu `x` doleva o počet bitů vypočítat, jak je popsáno níže.

   Nejvyšším bits mimo rozsah typu výsledku `x` jsou zahozeny, zbývající bity posunuty vlevo a nižšího řádu prázdný bitové pozice jsou nastaveny na hodnotu nula.

*  Posun doprava:

   ```csharp
   int operator >>(int x, int count);
   uint operator >>(uint x, int count);
   long operator >>(long x, int count);
   ulong operator >>(ulong x, int count);
   ```

   `>>` Operátor staffhubu `x` doprava o počet bitů vypočítat, jak je popsáno níže.

   Při `x` je typu `int` nebo `long`, bity nižšího řádu `x` jsou zahozeny, zbývající bity jsou posunuta doprava a implikovaný prázdný bitové pozice jsou nastaveny na nula, pokud `x` je nastaven na nezáporné a nastavte, pokud jeden `x` je záporný.

   Když `x` je typu `uint` nebo `ulong`, bity nižšího řádu `x` jsou zahozeny, jsou zbývající bity posunuta doprava a implikovaný prázdný bitové pozice jsou nastaveny na hodnotu nula.

Pro předdefinované operátory počet bitů, chcete-li posunout vypočítává následujícím způsobem:

*  Pokud typ `x` je `int` nebo `uint`, počet posunů je dán pět bity nižšího řádu `count`. Jinými slovy, počet posunů je vypočítán z `count & 0x1F`.
*  Pokud typ `x` je `long` nebo `ulong`, počet posunů je dán šest bity nižšího řádu `count`. Jinými slovy, počet posunů je vypočítán z `count & 0x3F`.

Pokud výsledný počet posunů je nula, operátory posunutí jednoduše vrátit hodnotu `x`.

Operace posunutí nikdy způsobit přetečení a produkuje stejné výsledky v `checked` a `unchecked` kontexty.

Pokud levý operand `>>` operátor je celočíselný typ se znaménkem, operátor, který provede aritmetický posun přímo ve které se šíří výhody nejvýznamnější bit (bit znaménka) operand na vyšší řád prázdný bitové pozice. Pokud levý operand `>>` operátor je celočíselný typ bez znaménka, operátor, který provádí logický posun přímo ve které implikovaný prázdný bitové pozice jsou vždy nastaveny na hodnotu nula. Pokud chcete provést opačný operace, které odvozen z typu operandu, je možné explicitní přetypování. Například pokud `x` je proměnná typu `int`, operace `unchecked((int)((uint)x >> y))` provede logický Posun vpravo `x`.

## <a name="relational-and-type-testing-operators"></a>Operátory relační a typové zkoušky

`==`, `!=`, `<`, `>`, `<=`, `>=`, `is` a `as` operátory jsou volány operátory relační a typové zkoušky.

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

`is` Operátor je popsána v [je operátor](expressions.md#the-is-operator) a `as` operátor je popsána v [operátor as](expressions.md#the-as-operator).

`==`, `!=`, `<`, `>`, `<=` a `>=` operátory jsou ***operátory porovnání***.

Pokud má operand operátoru porovnání typů za kompilace `dynamic`, a dynamicky je vázán výraz ([dynamické vazby](expressions.md#dynamic-binding)). V tomto případě je kompilaci typu výrazu `dynamic`, a rozlišení je popsáno níže se provede v době běhu pomocí run-time typu těchto operandů, která mají typ kompilace `dynamic`.

Operace formuláře `x` *op* `y`, kde *op* je relační operátor rozlišení přetížení ([binárním operátorem rozlišenípřetěžování](expressions.md#binary-operator-overload-resolution)) se použije k výběru na konkrétní operátor implementace. Operandy jsou převedeny na zvoleném operátorovi typy parametrů a typ výsledku je návratový typ operátoru.

Operátory předdefinované porovnání jsou popsány v následujících částech. Všechny předdefinované porovnávací operátory vrací výsledek typu `bool`, jak je popsáno v následující tabulce.


| __Operace__ | __výsledek__                                                       |
|---------------|------------------------------------------------------------------|
| `x == y`      | `true` Pokud `x` rovná `y`, `false` jinak                 | 
| `x != y`      | `true` Pokud `x` není roven `y`, `false` jinak             | 
| `x < y`       | `true` Pokud `x` je menší než `y`, `false` jinak                | 
| `x > y`       | `true` Pokud `x` je větší než `y`, `false` jinak             | 
| `x <= y`      | `true` Pokud `x` je menší než nebo rovna hodnotě `y`, `false` jinak    | 
| `x >= y`      | `true` Pokud `x` je větší než nebo rovna hodnotě `y`, `false` jinak | 

### <a name="integer-comparison-operators"></a>Operátory porovnání celé číslo

Celé číslo předdefinované operátory porovnání jsou:
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

Každý z těchto operátorů porovnání číselných hodnot dvě celočíselné operandy a vrátí `bool` hodnotu, která určuje, zda je konkrétní vztah `true` nebo `false`.

### <a name="floating-point-comparison-operators"></a>Operátory porovnání s plovoucí desetinnou čárkou

Předdefinované s plovoucí desetinnou čárkou porovnávací operátory jsou:
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

Operátory porovnání operandy pravidlům standardní IEEE 754:

*  Pokud některý operand je NaN, výsledkem je `false` pro všechny operátory s výjimkou `!=`, pro který je výsledkem `true`. Pro jakékoli dva operandy `x != y` vždy vytváří stejný výsledek jako `!(x == y)`. Pokud jeden nebo oba operandy jsou však NaN, `<`, `>`, `<=`, a `>=` operátory záměrně neprodukují stejné výsledky jako Logická negace opačný operátor. Například, pokud buď z `x` a `y` NaN, pak je `x < y` je `false`, ale `!(x >= y)` je `true`.
*  Pokud ani jeden operand není NaN, operátory porovnání hodnoty s plovoucí desetinnou čárkou dva operandy s ohledem na řazení

   ```
   -inf < -max < ... < -min < -0.0 == +0.0 < +min < ... < +max < +inf
   ```

   kde `min` a `max` jsou nejnižší a nejvyšší kladné konečné hodnoty, které můžou být vyjádřeny v daném formátu s plovoucí desetinnou čárkou. Toto uspořádání významné důsledky jsou:
   * Kladné a záporné nuly jsou považovány za shodné.
   * Záporné nekonečno je považován za méně než všechny ostatní hodnoty, ale rovna jiné záporné nekonečno.
   * Kladné nekonečno je považován za větší než všechny ostatní hodnoty, ale rovna jiné kladné nekonečno.

### <a name="decimal-comparison-operators"></a>Desetinné relační operátory

Předdefinované desítkové porovnávací operátory jsou:
```csharp
bool operator ==(decimal x, decimal y);
bool operator !=(decimal x, decimal y);
bool operator <(decimal x, decimal y);
bool operator >(decimal x, decimal y);
bool operator <=(decimal x, decimal y);
bool operator >=(decimal x, decimal y);
```

Každý z těchto operátorů porovnání číselných hodnot dvě desetinné operandů a vrací `bool` hodnotu, která určuje, zda je konkrétní vztah `true` nebo `false`. Každé desetinné porovnání je ekvivalentní k použití odpovídající relační nebo operátor rovnosti typu `System.Decimal`.

### <a name="boolean-equality-operators"></a>Operátory rovnosti Boolean

Rovnost předdefinované logické operátory jsou:
```csharp
bool operator ==(bool x, bool y);
bool operator !=(bool x, bool y);
```

Výsledek `==` je `true` Pokud mají oba `x` a `y` jsou `true` nebo pokud `x` a `y` jsou `false`. V opačném případě je výsledek `false`.

Výsledek `!=` je `false` Pokud mají oba `x` a `y` jsou `true` nebo pokud `x` a `y` jsou `false`. V opačném případě je výsledek `true`. Když jsou operandy typu `bool`, `!=` operátor vytvoří stejný výsledek jako `^` operátor.

### <a name="enumeration-comparison-operators"></a>Operátory porovnání výčet

Každý typ výčtu implicitně poskytuje následující předdefinované relační operátory:
```csharp
bool operator ==(E x, E y);
bool operator !=(E x, E y);
bool operator <(E x, E y);
bool operator >(E x, E y);
bool operator <=(E x, E y);
bool operator >=(E x, E y);
```

Výsledek vyhodnocení výrazu `x op y`, kde `x` a `y` jsou výrazy typu výčtu `E` s podkladovým typem `U`, a `op` je jedním z operátorů porovnání, je stejný jako vyhodnocení `((U)x) op ((U)y)`. Jinými slovy operátory porovnání typu výčtu jednoduše porovnat základní integrální hodnoty dva operandy.

### <a name="reference-type-equality-operators"></a>Operátory rovnosti pro typ odkazu

Operátory rovnosti předdefinovaný odkaz typu jsou:
```csharp
bool operator ==(object x, object y);
bool operator !=(object x, object y);
```

Operátory vrací výsledek porovnání dvou odkazy a zjistí rovnost či jiných rovnosti.

Protože předdefinovaných odkazovací operátory rovnosti typu přijmout operandy typu `object`, se vztahují na všechny typy, které nedeklarujte příslušné `operator ==` a `operator !=` členy. Naopak všechny příslušné rovnosti uživatelem definované operátory efektivně skrýt předdefinované odkazovací operátory rovnosti typu.

Operátory rovnosti typu předdefinovaných referenčních vyžadovat jeden z následujících akcí:

*  Oba operandy hodnotu typu ví *reference_type* nebo literál `null`. Kromě toho konverzi explicitní odkaz ([odkaz na explicitní převody](conversions.md#explicit-reference-conversions)) existuje z typu jeden z operandů typu je druhý operand.
*  Jeden operand je hodnota typu `T` kde `T` je *type_parameter* a druhý operand je literál `null`. Kromě toho `T` nemá omezení typu hodnoty.

Pokud platí jedna z těchto podmínek, dojde k chybě vazby čas. Významné důsledky z těchto pravidel jsou:

*  Je chyba vazby za použití předdefinovaných odkazovací operátory rovnosti typ k porovnání dva odkazy, které jsou známé jako jiný v době vazby. Například, pokud doba vazby typy operandů jsou dva typy tříd `A` a `B`a pokud `A` ani `B` je odvozena z jiných, potom by bylo možné pro dva operandy, chcete-li odkazovat na stejný objekt. Operace proto se považuje za chybu v době vazby.
*  Operátory rovnosti typu předdefinovaných referenčních neumožňují hodnotu operandy typu, který se má porovnat. Proto pokud typu Struktura deklaruje vlastní operátory rovnosti, není možné porovnat hodnoty tohoto typu struktury.
*  Operátory rovnosti typu předdefinovaných referenčních nikdy nezpůsobí operace zabalení na výskyt svých operandů. Bylo by provádět tyto operace zabalení, protože odkazy do nově přiděleného zabalený instancí by nutně lišit od jiných odkazů na nemá význam.
*  Pokud typ parametru typu operandu `T` je ve srovnání s `null`a za běhu typu `T` je typ hodnoty, je výsledkem porovnání `false`.

Následující příklad zkontroluje, zda je argument typu bez omezení parametru typu `null`.
```csharp
class C<T>
{
    void F(T x) {
        if (x == null) throw new ArgumentNullException();
        ...
    }
}
```

`x == null` Konstrukce smí obsahovat i v případě `T` by mohly představovat typ hodnoty a výsledek je jednoduše definován jako `false` při `T` je typ hodnoty.

Operace formuláře `x == y` nebo `x != y`, pokud je k dispozici žádná `operator ==` nebo `operator !=` existuje, rozlišení přetížení operátoru ([binárním operátorem rozlišení přetěžování](expressions.md#binary-operator-overload-resolution)) pravidla, která vybere operátor místo operátor rovnosti typ předdefinovaný odkaz. Je ale vždy možné vybrat operátor rovnosti předdefinovaných referenčních typu explicitně přetypováním jeden nebo oba operandy na typ `object`. V příkladu
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
```
True
False
False
False
```

`s` a `t` proměnná odkazuje na dvou rozdílných `string` instancí, který obsahuje stejné znaky. První porovnání výstupy `True` protože předdefinované řetězcové operátor rovnosti ([řetězec operátory rovnosti](expressions.md#string-equality-operators)) je vybrána, když jsou oba operandy typu `string`. Všechny zbývající porovnání výstup `False` protože operátor rovnosti předdefinovaných referenčních typů je vybrána, když jeden nebo oba operandy jsou typu `object`.

Všimněte si, že výše uvedené postup není smysl pro typy hodnot. V příkladu
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
Vypíše `False` protože vytvořit odkazy na dvě samostatné instance položky CAST v poli `int` hodnoty.

### <a name="string-equality-operators"></a>Operátory rovnosti řetězců

Jsou předdefinované řetězcové operátory rovnosti:
```csharp
bool operator ==(string x, string y);
bool operator !=(string x, string y);
```

Dvě `string` hodnoty jsou považovány za shodné, když je splněna jedna z následujících akcí:

*  Jsou obě hodnoty `null`.
*  Odkazy na jinou hodnotu než null řetězec instancí, které mají stejné délky a stejné znaky v každé pozici znaku jsou obě hodnoty.

Operátory rovnosti řetězec porovnání hodnoty řetězce místo odkazy na řetězec. Když dvě instance samostatné řetězec obsahovat přesně stejnou sekvenci znaků, jsou hodnoty řetězce stejné, ale odkazy se liší. Jak je popsáno v [operátory rovnosti pro typ odkazu](expressions.md#reference-type-equality-operators), operátory rovnosti reference typu lze porovnat řetězec odkazy místo řetězcové hodnoty.

### <a name="delegate-equality-operators"></a>Operátory rovnosti delegáta

Každý typ delegáta implicitně poskytuje následující předdefinované relační operátory:

```csharp
bool operator ==(System.Delegate x, System.Delegate y);
bool operator !=(System.Delegate x, System.Delegate y);
```

Instance dvou delegátů jsou považovány za shodné následujícím způsobem:

*  Pokud je jedna instance delegátů `null`, pokud jsou obě jsou shodné `null`.
*  Pokud delegáty mají různé run-time typu nikdy jsou stejné.
*  Pokud obě instance delegáta máte seznam volání ([delegovat deklarace](delegates.md#delegate-declarations)), tyto instance jsou stejné, a pouze v případě jejich vyvolání seznamy mají stejnou délku, a každá položka v seznamu vyvolání jeden z rovná (jak je definována níže) na odpovídající položku v pořadí, v seznamu vyvolání druhé strany.

Rovnost hodnoty položky seznamu vyvolání platí následující pravidla:

*  Pokud obě dvě seznamu položky vyvolání odkazovat na stejný statická metoda pak položky jsou stejné.
*  Pokud dvě volání položky seznamu oba odkazují na stejný nestatickou metodu na stejném cílovém objektu (definované operátory rovnosti reference) položky jsou stejné.
*  Vyvolání seznamu položek vytvořený ze zkušební verze sémanticky identických *anonymous_method_expression*s nebo *lambda_expression*s (pravděpodobně prázdná) stejnou sadou zachycené vnější proměnné instance se můžou (ale není nutné) musí rovnat.

### <a name="equality-operators-and-null"></a>Operátory rovnosti a null

`==` a `!=` operátory povolit jeden operand bude hodnota typem s možnou hodnotou Null a druhý, aby `null` literálu, i v případě, že pro operaci neexistuje žádný předdefinovaných nebo uživatelem definovaný operátor (v unlifted nebo zrušeno vs. formuláře).

Operace jednoho z formuláře
```csharp
x == null
null == x
x != null
null != x
```
kde `x` je výraz typu s možnou hodnotou Null, pokud řešení přetížení operátoru ([binárním operátorem rozlišení přetěžování](expressions.md#binary-operator-overload-resolution)) se nedaří najít příslušné operátora, výsledek se místo toho vypočítá ze `HasValue` Vlastnost `x`. Konkrétně první dvě různými formami jsou přeloženy do `!x.HasValue`, a poslední dvě různými formami jsou přeloženy do `x.HasValue`.

### <a name="the-is-operator"></a>Is – operátor

`is` Operátor se používá k dynamicky zkontrolujte, jestli je kompatibilní s daným typem run-time typu objektu. Výsledek operace `E is T`, kde `E` je výraz a `T` je typ, je logická hodnota hodnotu, která udává, zda `E` můžete úspěšně převeden na typ `T` převodem odkaz, zabalení převod, nebo unboxingového převodu. Operace se vyhodnotí takto, jakmile byla nahrazena argumentů typu pro všechny parametry typu:

*  Pokud `E` je anonymní funkce, dojde k chybě kompilace
*  Pokud `E` je skupinu metod nebo `null` literálu, pokud typ `E` je typem odkazu nebo typ připouštějící hodnotu Null a hodnota `E` je null, je výsledek false.
*  V opačném případě nechat `D` představují dynamického typu `E` následujícím způsobem:
   * Pokud typ `E` je typem odkazu `D` je run-time typu odkazu na instanci podle `E`.
   * Pokud typ `E` je typ připouštějící hodnotu Null, `D` je základní typ tohoto typu s možnou hodnotou Null.
   * Pokud typ `E` je typ hodnoty Null, `D` je typ `E`.
*  Výsledek operace závisí na `D` a `T` následujícím způsobem:
   * Pokud `T` je typem odkazu, je výsledek true Pokud `D` a `T` jsou stejného typu, pokud `D` je typem odkazu a implicitní referenční převod z `D` k `T` existuje, nebo pokud `D` je typ hodnoty a převod na uzavřené určení z `D` k `T` existuje.
   * Pokud `T` je typ připouštějící hodnotu Null, je výsledek true Pokud `D` je základní typ `T`.
   * Pokud `T` je typ hodnoty Null, je výsledek true Pokud `D` a `T` jsou stejného typu.
   * Výsledkem je, jinak false.

Všimněte si, že nejsou uznán uživatelem definované převody `is` operátor.

### <a name="the-as-operator"></a>Operátor as

`as` Operátor se používá k explicitnímu převodu hodnoty na typ daného odkazu nebo typ připouštějící hodnotu Null. Na rozdíl od výraz přetypování ([výrazy přetypování](expressions.md#cast-expressions)), `as` operátor nikdy nevyvolá výjimku. Místo toho, pokud zadaný převod není možný, výsledná hodnota je `null`.

V operaci formuláře `E as T`, `E` musí být výraz a `T` musí být typ odkazu, parametr typu známé jako typ odkazu nebo typ připouštějící hodnotu Null. Kromě toho aspoň jednu z následujících musí mít hodnotu true, nebo jinak dojde k chybě kompilace:

*  Identita ([Identity převod](conversions.md#identity-conversion)), implicitní s možnou hodnotou Null ([implicitní převody typu s možnou hodnotou Null](conversions.md#implicit-nullable-conversions)), implicitní odkaz ([odkaz na implicitní převody](conversions.md#implicit-reference-conversions)), zabalení ([ Zabalení převody](conversions.md#boxing-conversions)), explicitní s možnou hodnotou Null ([explicitní převody s možnou hodnotou Null](conversions.md#explicit-nullable-conversions)), přímý odkaz ([převody explicitní odkaz](conversions.md#explicit-reference-conversions)), nebo rozbalení ([Rozbalení převody](conversions.md#unboxing-conversions)) existuje převod z `E` k `T`.
*  Typ `E` nebo `T` je otevřeného typu.
*  `E` je `null` literálu.

Pokud typ době kompilace `E` není `dynamic`, operace `E as T` vytváří stejný výsledek jako
```csharp
E is T ? (T)(E) : (T)null
```
s tím rozdílem, že `E` se jenom vyhodnotí jednou. Kompilátor může očekávat, že optimalizace `E as T` provádět maximálně jeden dynamický typ kontrolu na rozdíl od dvou kontroly dynamického typu odvozené od výše uvedených rozšíření.

Pokud typ době kompilace `E` je `dynamic`, na rozdíl od operátoru přetypování `as` operátor není vázán dynamicky ([dynamické vazby](expressions.md#dynamic-binding)). Proto v tomto případě je rozšíření:
```csharp
E is T ? (T)(object)(E) : (T)null
```

Všimněte si, že některé převody, jako je například uživatelem definované převody nejsou s `as` operátor a by místo toho provádět pomocí výrazy přetypování.

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
parametr typu `T` z `G` známý typ odkazu, protože nemá omezení třídy. Parametr typu `U` z `H` není ale; proto používání `as` operátor v `H` se nepovoluje.

## <a name="logical-operators"></a>Logické operátory

`&`, `^`, A `|` operátory jsou volány logické operátory.

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

Pokud operand logického operátoru má typ kompilace `dynamic`, pak je dynamicky vázán výraz ([dynamické vazby](expressions.md#dynamic-binding)). V tomto případě je kompilaci typu výrazu `dynamic`, a rozlišení je popsáno níže se provede v době běhu pomocí run-time typu těchto operandů, která mají typ kompilace `dynamic`.

Operace formuláře `x op y`, kde `op` je jedním z logické operátory přetížení ([binárním operátorem rozlišení přetěžování](expressions.md#binary-operator-overload-resolution)) se použije k výběru na konkrétní operátor implementace. Operandy jsou převedeny na zvoleném operátorovi typy parametrů a typ výsledku je návratový typ operátoru.

Předdefinované logické operátory jsou popsány v následujících částech.

### <a name="integer-logical-operators"></a>Celé číslo logické operátory

Předdefinované celé číslo logické operátory jsou:
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

`&` Vypočítá bitový operátor logického `AND` dvou operandů `|` vypočítá bitový operátor logického `OR` dvou operandů a `^` vypočítá bitový exkluzivní logický operátor `OR` dvou operandů. Žádné přetečení jsou z těchto operací.

### <a name="enumeration-logical-operators"></a>Výčet logické operátory

Každý typ výčtu `E` implicitně poskytuje následující předdefinované logické operátory:

```csharp
E operator &(E x, E y);
E operator |(E x, E y);
E operator ^(E x, E y);
```

Výsledek vyhodnocení výrazu `x op y`, kde `x` a `y` jsou výrazy typu výčtu `E` s podkladovým typem `U`, a `op` je jedním z logické operátory, je stejný jako vyhodnocení `(E)((U)x op (U)y)`. Logické operátory typu výčtu jinými slovy, stačí provést logická operace s základního typu dva operandy.

### <a name="boolean-logical-operators"></a>Logické operátory

Předdefinované logické logické operátory jsou:
```csharp
bool operator &(bool x, bool y);
bool operator |(bool x, bool y);
bool operator ^(bool x, bool y);
```

Výsledek `x & y` je `true` Pokud mají oba `x` a `y` jsou `true`. V opačném případě je výsledek `false`.

Výsledek `x | y` je `true` Pokud `x` nebo `y` je `true`. V opačném případě je výsledek `false`.

Výsledek `x ^ y` je `true` Pokud `x` je `true` a `y` je `false`, nebo `x` je `false` a `y` je `true`. V opačném případě je výsledek `false`. Když jsou operandy typu `bool`, `^` operátor vypočítá stejný výsledek jako `!=` operátor.

### <a name="nullable-boolean-logical-operators"></a>Logická logické operátory s povolenou hodnotou Null

Typ s možnou hodnotou Null logická `bool?` může představovat tří hodnot `true`, `false`, a `null`a se koncepčně podobá tři vracející typ použitý pro logické výrazy v jazyce SQL. Chcete-li zajistit výsledky vytvořené metodou `&` a `|` operátory pro `bool?` operandy jsou konzistentní s logikou s hodnotou tři SQL, následující předdefinované operátory jsou k dispozici:

```csharp
bool? operator &(bool? x, bool? y);
bool? operator |(bool? x, bool? y);
```

V následující tabulce jsou uvedeny výsledky vytvořené metodou tyto operátory pro všechny kombinace hodnot `true`, `false`, a `null`.

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

`&&` a `||` operátory jsou volány podmíněné logické operátory. Označují se také jako "short-circuiting" logické operátory.

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

`&&` a `||` operátory jsou podmíněné verze `&` a `|` operátory:

*  Operace `x && y` odpovídá operaci `x & y`, s tím rozdílem, že `y` je vyhodnocen pouze v případě `x` není `false`.
*  Operace `x || y` odpovídá operaci `x | y`, s tím rozdílem, že `y` je vyhodnocen pouze v případě `x` není `true`.

Pokud operand logického operátoru podmíněného má typ kompilace `dynamic`, pak je dynamicky vázán výraz ([dynamické vazby](expressions.md#dynamic-binding)). V tomto případě je kompilaci typu výrazu `dynamic`, a rozlišení je popsáno níže se provede v době běhu pomocí run-time typu těchto operandů, která mají typ kompilace `dynamic`.

Operace formuláře `x && y` nebo `x || y` zpracování s použitím řešení přetížení ([binárním operátorem rozlišení přetěžování](expressions.md#binary-operator-overload-resolution)) jako kdyby byla zapsána operaci `x & y` nebo `x | y`. Potom

*  Pokud se nepodaří určit přetížení k vyhledání jednoho nejlepší operátoru nebo řešení přetížení vybere jeden z předdefinovaných celé číslo logické operátory, dojde k chybě vazby čas.
*  Jinak, pokud vybraný operátor je jedním z předdefinované logické logické operátory ([logické logické operátory](expressions.md#boolean-logical-operators)) nebo logická logické operátory s povolenou hodnotou Null ([logické operátory s povolenou hodnotou Null logická](expressions.md#nullable-boolean-logical-operators)), operace se nezpracovala, jak je popsáno v [logické podmíněné logické operátory](expressions.md#boolean-conditional-logical-operators).
*  V opačném případě zvoleném operátorovi je uživatelem definovaný operátor a bude operace zpracována, jak je popsáno v [podmíněné logické operátory definované uživatelem](expressions.md#user-defined-conditional-logical-operators).

Není možné přímo přetížení podmíněné logické operátory. Ale protože podmíněné logické operátory jsou vyhodnocovány z hlediska regulární logické operátory, přetíženími pravidelné logických operátorů, s určitými omezeními také považují přetížení podmíněné logických operátorů. Toto je popsáno dále v [podmíněné logické operátory definované uživatelem](expressions.md#user-defined-conditional-logical-operators).

### <a name="boolean-conditional-logical-operators"></a>Logická podmíněné logické operátory

Když operandy `&&` nebo `||` jsou typu `bool`, nebo když jsou jako operandy typů, které nedefinují příslušném `operator &` nebo `operator |`, ale definovat implicitní převod na `bool`, operace se zpracování následujícím způsobem:

*  Operace `x && y` se vyhodnotí jako `x ? y : false`. Jinými slovy `x` je nejdřív vyhodnotit a převedeny na typ `bool`. Když se poté `x` je `true`, `y` je vyhodnocen a převeden na typ `bool`, a toto řešení je výsledek operace. V opačném případě je výsledek operace `false`.
*  Operace `x || y` se vyhodnotí jako `x ? true : y`. Jinými slovy `x` je nejdřív vyhodnotit a převedeny na typ `bool`. Když se poté `x` je `true`, je výsledek operace `true`. V opačném případě `y` je vyhodnocen a převeden na typ `bool`, a toto řešení je výsledek operace.

### <a name="user-defined-conditional-logical-operators"></a>Podmíněné logické operátory definované uživatelem

Když operandy `&&` nebo `||` jsou typy, které deklarují příslušném uživatelem definované `operator &` nebo `operator |`, z následujících možností musí mít hodnotu true, pokud `T` je typ, ve kterém je deklarována zvoleném operátorovi:

*  Musí být návratový typ a zadejte každý parametr zvoleném operátorovi `T`. Jinými slovy, musíte vypočítat logický operátor, který `AND` nebo logickém `OR` dvou operandů typu `T`a musí vrátit výsledek typu `T`.
*  `T` musí obsahovat deklarace `operator true` a `operator false`.

Pokud některý z těchto požadavků není splněno, dojde k chybě vazby – za. V opačném případě `&&` nebo `||` operace se vyhodnocuje na základě kombinace uživatelsky definované `operator true` nebo `operator false` s vybranou uživatelem definovaný operátor:

*  Operace `x && y` se vyhodnotí jako `T.false(x) ? x : T.&(x, y)`, kde `T.false(x)` je vyvolání `operator false` deklarované v `T`, a `T.&(x, y)` je vyvolání vybraného `operator &`. Jinými slovy `x` nejdřív vyhodnotit a `operator false` se vyvolá u výsledku k určení, zda `x` je jednoznačně false. Když se poté `x` je jednoznačně false, je hodnota vypočítaná dříve pro výsledek operace `x`. V opačném případě `y` je vyhodnocen a vybraný `operator &` se vyvolá u hodnota vypočítaná dříve pro `x` a hodnotu vypočítat pro `y` vytvoří výsledek operace.
*  Operace `x || y` se vyhodnotí jako `T.true(x) ? x : T.|(x, y)`, kde `T.true(x)` je vyvolání `operator true` deklarované v `T`, a `T.|(x,y)` je vyvolání vybraného `operator|`. Jinými slovy `x` nejdřív vyhodnotit a `operator true` se vyvolá u výsledku k určení, zda `x` jednoznačně platí. Když se poté `x` je jednoznačně true, je hodnota vypočítaná dříve pro výsledek operace `x`. V opačném případě `y` je vyhodnocen a vybraný `operator |` se vyvolá u hodnota vypočítaná dříve pro `x` a hodnotu vypočítat pro `y` vytvoří výsledek operace.

V některém z těchto operací výraz Dal `x` je jen pro vyhodnotí jednou a výraz Dal `y` je buď není vyhodnocen nebo vyhodnotí přesně jednou.

Příklad typu, který implementuje `operator true` a `operator false`, naleznete v tématu [databáze typu boolean](structs.md#database-boolean-type).

## <a name="the-null-coalescing-operator"></a>Null operátor sloučení

`??` Operátor je volána null operátor sloučení.

```antlr
null_coalescing_expression
    : conditional_or_expression
    | conditional_or_expression '??' null_coalescing_expression
    ;
```

Nulový slučovací výraz ve tvaru `a ?? b` vyžaduje `a` bude typu nebo odkaz na typ připouštějící hodnotu Null. Pokud `a` je jiná než null, výsledek `a ?? b` je `a`; jinak vrátí hodnotu, výsledek je `b`. Výsledkem operace `b` pouze tehdy, pokud `a` má hodnotu null.

Null operátor sloučení je asociativní zprava, což znamená, že operace se seskupují zleva doprava. Například výraz ve tvaru `a ?? b ?? c` se vyhodnotí jako `a ?? (b ?? c)`. Obecně platí podmínky výrazu v podobě `E1 ?? E2 ?? ... ?? En` vrátí první operandy, jinou hodnotu než null, nebo hodnotu null, pokud jsou všechny operandy hodnotu null.

Typ výrazu `a ?? b` závisí na které implicitní převody jsou k dispozici na operandy. V pořadí podle priority, typu `a ?? b` je `A0`, `A`, nebo `B`, kde `A` je typ `a` (za předpokladu, že `a` má typ), `B` je typ `b` () za předpokladu, že `b` má typ), a `A0` je základní typ `A` Pokud `A` je typ připouštějící hodnotu Null, nebo `A` jinak. Konkrétně `a ?? b` zpracování následujícím způsobem:

*  Pokud `A` existuje a není typ připouštějící hodnotu null nebo typ odkazu, dojde k chybě kompilace.
*  Pokud `b` dynamického výrazu je typ výsledku je `dynamic`. V době běhu `a` nejprve vyhodnocena. Pokud `a` nemá hodnotu null, `a` je převést na dynamický, a to se stane výsledek. V opačném případě `b` vyhodnocena, a to se stane výsledek.
*  Jinak, pokud `A` existuje a je typ připouštějící hodnotu Null a existuje implicitní převod z `b` k `A0`, typ výsledku je `A0`. V době běhu `a` nejprve vyhodnocena. Pokud `a` nemá hodnotu null, `a` je neobalený, aby typ `A0`, a to se stane výsledek. V opačném případě `b` je vyhodnocen a převeden na typ `A0`, a to se stane výsledek.
*  Jinak, pokud `A` existuje a že existuje implicitní převod z `b` k `A`, typ výsledku je `A`. V době běhu `a` nejprve vyhodnocena. Pokud `a` nemá hodnotu null, `a` výsledek. V opačném případě `b` je vyhodnocen a převeden na typ `A`, a to se stane výsledek.
*  Jinak, pokud `b` má typ `B` a existuje implicitní převod z `a` k `B`, typ výsledku je `B`. V době běhu `a` nejprve vyhodnocena. Pokud `a` nemá hodnotu null, `a` je neobalený, aby typ `A0` (Pokud `A` existuje a může mít hodnotu Null) a převedeny na typ `B`, a toto řešení je výsledek. V opačném případě `b` jsou vyhodnoceny a výsledek.
*  V opačném případě `a` a `b` jsou nekompatibilní, za kompilace dojde k chybě.

## <a name="conditional-operator"></a>Podmíněný operátor

`?:` Operátor se nazývá podmiňovací operátor. V některých případech také nazývá tříhodnotový operátor.

```antlr
conditional_expression
    : null_coalescing_expression
    | null_coalescing_expression '?' expression ':' expression
    ;
```

Podmíněný výraz ve tvaru `b ? x : y` nejprve tuto podmínku vyhodnotí `b`. Když se poté `b` je `true`, `x` jsou vyhodnoceny a výsledek operace. V opačném případě `y` jsou vyhodnoceny a výsledek operace. Podmíněný výraz nikdy vyhodnotí oba `x` a `y`.

Podmíněný operátor je asociativní zprava, což znamená, že operace se seskupují zleva doprava. Například výraz ve tvaru `a ? b : c ? d : e` se vyhodnotí jako `a ? b : (c ? d : e)`.

První operand `?:` operator musí být výraz, který lze implicitně převést na `bool`, nebo výraz typu, který implementuje `operator true`. Pokud ani jeden z těchto požadavků není splněna, dojde k chybě v době kompilace.

Druhý a třetí operand `x` a `y`, z `?:` operátor řídit typ podmíněného výrazu.

*  Pokud `x` má typ `X` a `y` má typ `Y` pak
   * Pokud implicitní převod ([implicitních převodů](conversions.md#implicit-conversions)) existuje z `X` k `Y`, ale ne z `Y` k `X`, pak `Y` je typ podmíněného výrazu.
   * Pokud implicitní převod ([implicitních převodů](conversions.md#implicit-conversions)) existuje z `Y` k `X`, ale ne z `X` k `Y`, pak `X` je typ podmíněného výrazu.
   * V opačném případě se dá určit bez typu výrazu a dojde k chybě kompilace.
*  Pokud pouze jeden z `x` a `y` má typ a obě `x` a `y`, aplikace se implicitně převést na typ, pak je typ podmíněného výrazu.
*  V opačném případě se dá určit bez typu výrazu a dojde k chybě kompilace.

Zpracování za běhu podmíněného výrazu v podobě `b ? x : y` se skládá z následujících kroků:

*  První, `b` je vyhodnocen a `bool` hodnotu `b` závisí:
   * Pokud implicitní převod z typu `b` k `bool` existuje, tento implicitní převod se provádí za účelem vytvoření `bool` hodnotu.
   * V opačném případě `operator true` definovaný podle typu `b` se vyvolá k vytvoření `bool` hodnotu.
*  Pokud `bool` hodnotu vytvořenou testovaným výše uvedeném kroku je `true`, pak `x` je vyhodnotit a převedeny na typ podmíněného výrazu, a toto řešení je výsledek podmíněného výrazu.
*  V opačném případě `y` je vyhodnotit a převedeny na typ podmíněného výrazu, a to se stane výsledek podmíněného výrazu.

## <a name="anonymous-function-expressions"></a>Výrazy anonymní funkce

***Anonymní funkce*** je výraz, který představuje definici metody "in-line". Anonymní funkce nemá hodnotu nebo typ v a sama o sobě, ale lze převést na kompatibilní typ. strom delegáta nebo výraz. Vyhodnocení anonymní funkce převodu závisí na cílový typ převodu: Pokud je typ delegátu, převod vyhodnocen na hodnotu delegáta, které se odkazuje na metodu, která definuje anonymní funkce. Pokud je typu stromu výrazu, se vyhodnotí jako převod na strom výrazu, která reprezentuje strukturu těchto metodu jako objektovou strukturu.

Z historických důvodů jsou mají dvě syntaktické varianty anonymní funkce, a to *lambda_expression*s a *anonymous_method_expression*s. Pro téměř všechny účely *lambda_expression*s jsou stručné a expresivní než *anonymous_method_expression*s, která zůstanou v jazyce pro zpětnou kompatibilitu.

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

`=>` Operátor má stejnou prioritu jako přiřazení (`=`) a je asociativní zprava.

Anonymní funkce s `async` Modifikátor je asynchronní funkci a postupuje pravidel popsaných v [iterátory](classes.md#iterators).

Parametry anonymní funkce v podobě *lambda_expression* může být explicitně nebo implicitně typu. V seznamu parametrů explicitně je výslovně uvedeno každý parametr typu. V seznamu implicitně typované parametrů typy parametrů jsou odvozeny z kontextu, ve kterém dochází anonymní funkce – konkrétně, když je anonymní funkce převedena na typ kompatibilní delegáta nebo typu stromu výrazu, který poskytuje typ typy parametrů ([anonymní funkce převody](conversions.md#anonymous-function-conversions)).

Anonymní funkce s parametrem jeden, implicitně typované můžete vynechat závorky ze seznamu parametrů. Jinými slovy anonymní funkci formuláře
```csharp
( param ) => expr
```
lze zkrátit na
```csharp
param => expr
```

Seznam parametrů anonymní funkce v podobě *anonymous_method_expression* je volitelný. Pokud tento parametr zadaný, parametry musí být explicitně určeny typy. Pokud ne, je převeden na delegáta se žádné parametry anonymní funkce seznamu neobsahující `out` parametry.

A *bloku* tělo anonymní funkce je dostupná ([koncové body a dostupnosti](statements.md#end-points-and-reachability)) Pokud do nedostupný příkaz nedojde anonymní funkce.

Některé příklady anonymní funkce, postupujte podle následujících:

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

Chování *lambda_expression*s a *anonymous_method_expression*s je stejná s výjimkou následujících bodů:

*  *anonymous_method_expression*s povolit seznam parametrů pro zcela vynechat získávání převoditelnosti na typy v seznamu parametrů hodnot delegátů.
*  *lambda_expression*povolit s typy parametrů pro tento parametr vynechán a odvodit, zatímco *anonymous_method_expression*vyžadují s typy parametrů pro výslovně uvedeno.
*  Text *lambda_expression* může být výraz nebo blok příkazů, že text *anonymous_method_expression* musí být blok příkazů.
*  Pouze *lambda_expression*y mají převody na typy stromu výrazů kompatibilní ([typy stromu výrazů](types.md#expression-tree-types)).

### <a name="anonymous-function-signatures"></a>Anonymní funkce podpisy

Volitelný *anonymous_function_signature* anonymní funkce definuje název a volitelně typy formálních parametrů pro anonymní funkce. Rozsah parametry anonymní funkce je *anonymous_function_body*. ([Obory](basic-concepts.md#scopes)) společně s seznamu parametrů (Pokud je zadaný) představuje anonymní--tělo metody místa deklarace ([deklarace](basic-concepts.md#declarations)). Proto je chyba kompilace pro název parametru anonymní funkce, která se shodovat s názvem lokální proměnná, lokální konstanta ani parametr, jehož obor zahrnuje *anonymous_method_expression* nebo *lambda_ výraz*.

Pokud je anonymní funkce *explicit_anonymous_function_signature*, pak sada delegáta kompatibilní typy a typy stromu výrazů je omezené na ty, které mají stejné typy parametrů a modifikátory ve stejném pořadí. Na rozdíl od skupiny převody – metoda ([Metoda skupiny převody](conversions.md#method-group-conversions)), odchylku opravné položky k typy parametrů anonymní funkce není podporována. Pokud anonymní funkce nemá *anonymous_function_signature*, pak sada delegáta kompatibilní typy a typy stromu výrazů je omezené na ty, které nemají `out` parametry.

Všimněte si, že *anonymous_function_signature* nesmí obsahovat atributy nebo pole parametrů. Nicméně *anonymous_function_signature* může být kompatibilní s typem delegáta, jehož seznam parametrů obsahuje pole parametrů.

Nezapomeňte tento převod do typu stromu výrazu, i v případě kompatibilní, může stále selžou v době kompilace ([typy stromu výrazů](types.md#expression-tree-types)).

### <a name="anonymous-function-bodies"></a>Anonymní funkce těla

Text (*výraz* nebo *bloku*) anonymní funkce se vztahují následující pravidla:

*  Pokud anonymní funkce obsahuje podpis, jsou k dispozici v těle parametry zadaná v signatuře. Pokud anonymní funkce nemá žádný podpis lze převést na typ delegáta nebo typ výrazu s parametry ([anonymní funkce převody](conversions.md#anonymous-function-conversions)), ale parametry nelze získat přístup v textu.
*  S výjimkou `ref` nebo `out` parametry zadaná v signatuře (pokud existuje) nejbližšího obklopujícího anonymní funkce, je chyba kompilace pro tělo pro přístup k `ref` nebo `out` parametru.
*  Pokud typ `this` je typ struktury je chyba kompilace pro tělo pro přístup k `this`. Je hodnota true Určuje, zda je explicitní přístup (jako v `this.x`) nebo implicitní (jako v `x` kde `x` je členem instance struktury). Toto pravidlo jednoduše zakazují takový přístup a nemá vliv, zda člen vyhledávání výsledkem členem struktury.
*  Text má přístup do vnější proměnné ([vnější proměnné](expressions.md#outer-variables)) anonymní funkce. Přístup vnější proměnné odkazovat na instanci proměnné, která je aktivní v okamžiku *lambda_expression* nebo *anonymous_method_expression* vyhodnocena ([hodnocení anonymní funkce výrazy](expressions.md#evaluation-of-anonymous-function-expressions)).
*  Je chyba kompilace pro text tak, aby obsahovala `goto` příkazu `break` příkazu, nebo `continue` jehož cíl je mimo tělo, nebo v těle obsažené anonymní funkce.
*  A `return` příkaz v těle nevrátí řízení z nejbližšího obklopujícího vyvolání anonymní funkce, nikoli z nadřazeného členské funkce. Výraz zadaný v `return` příkaz musí být implicitně převoditelná na návratový typ na typ delegáta nebo typu stromu výrazu, ke kterému nejbližšího obklopujícího *lambda_expression* nebo *anonymous_ method_expression* převeden ([anonymní funkce převody](conversions.md#anonymous-function-conversions)).

Je explicitně nespecifikovaná, zda neexistuje žádný způsob, jak spustit blok anonymní funkce jiných než prostřednictvím zkušební verze a vyvolání *lambda_expression* nebo *anonymous_method_expression*. Konkrétně se kompilátor můžete implementovat anonymní funkci syntetizační jeden nebo více metod nebo typy. Názvy těchto syntetizovaný prvků musí být vyhrazená pro použití kompilátoru formuláře.

### <a name="overload-resolution-and-anonymous-functions"></a>Rozlišení přetížení a anonymní funkce

Anonymní funkce v seznam argumentů účastnit odvození typu proměnné a řešení přetížení. Najdete [odvození typu](expressions.md#type-inference) a [rozlišení přetěžování](expressions.md#overload-resolution) přesná pravidla.

Následující příklad ukazuje účinek anonymní funkce v řešení přetížení.

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

`ItemList<T>` Třída má dvě `Sum` metody. Každý má `selector` argument, který extrahuje hodnotu Součet přes ze seznamu položek. Extrahovaná hodnota může být buď `int` nebo `double` a výsledný součet je rovněž `int` nebo `double`.

`Sum` Metody může třeba vypočítat součtů ze seznamu řádky podrobností v pořadí.

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

V prvním vyvoláním služby `orderDetails.Sum`, obě `Sum` metody jsou použitelné protože anonymní funkce `d => d. UnitCount` je kompatibilní s oběma `Func<Detail,int>` a `Func<Detail,double>`. Ale řešení přetížení vybere první `Sum` metoda protože převod na `Func<Detail,int>` je obecně lepší než převod na `Func<Detail,double>`.

V druhém volání `orderDetails.Sum`, pouze druhý `Sum` metodu je možné použít protože anonymní funkce `d => d.UnitPrice * d.UnitCount` produkuje hodnotu typu `double`. Proto přetížení rozlišení zvolí druhý `Sum` metodu pro tohoto volání.

### <a name="anonymous-functions-and-dynamic-binding"></a>Anonymní funkce a dynamické vazby

Anonymní funkce nemůže být příjemce, argument nebo operand dynamicky vázané operace.

### <a name="outer-variables"></a>Vnější proměnné

Všechny místní proměnná, parametr hodnoty nebo pole parametrů, jehož obor zahrnuje *lambda_expression* nebo *anonymous_method_expression* je volána ***vnější proměnné*** anonymní funkce. V instanci funkce člena třídy `this` hodnoty je považován za hodnotu parametru a vnější proměnný všechny anonymní funkce obsažené v členské funkce.

#### <a name="captured-outer-variables"></a>Zachycené vnější proměnné

Když vnější proměnná odkazuje anonymní funkce, vnější proměnné se říká, že byly ***zachycené*** anonymní funkce. Obvykle je omezená na provedení bloku nebo příkazu, ke kterému je přidružené životního cyklu lokální proměnné ([lokální proměnné](variables.md#local-variables)). Však životnost zachycené vnější proměnné je rozšířená alespoň do delegáta nebo strom výrazu, které jsou vytvořené z anonymní funkce je způsobilý pro uvolňování paměti.

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
lokální proměnná `x` zachycena anonymní funkce a životnost `x` rozšířené alespoň do delegáta vrácená `F` stane nárok uvolňování paměti (které nedojde do konce velmi program). Protože každé vyvolání sady anonymní funkce pracuje na stejnou instanci `x`, výstup v příkladu je:
```
1
2
3
```

Při zachytávání lokální proměnná ani parametr hodnoty anonymní funkcí lokální proměnná ani parametr se již považuje za pevná proměnná ([Fixed a přesunutelný proměnné](unsafe-code.md#fixed-and-moveable-variables)), ale místo toho považováno za přesouvat Proměnná. Proto všechny `unsafe` musíte nejprve použít kód, který přebírá adresu zachycené vnější proměnné `fixed` příkaz vyřešit proměnnou.

Všimněte si, že na rozdíl od nezachycené proměnnou, zachycené lokální proměnné může být současně vystavena více vláken provádění.

#### <a name="instantiation-of-local-variables"></a>Vytvoření instance lokální proměnné

Místní proměnná se považuje za ***vytvořena instance*** při provádění zadá rozsah proměnné. Například když je vyvolána následující metodu, místní proměnná `x` je vytvořena instance a inicializován třikrát – jednou pro každou iteraci smyčky.

```csharp
static void F() {
    for (int i = 0; i < 3; i++) {
        int x = i * 2 + 1;
        ...
    }
}
```

Ale přesunutí deklarace `x` mimo smyčku výsledky v jedné instance `x`:
```csharp
static void F() {
    int x;
    for (int i = 0; i < 3; i++) {
        x = i * 2 + 1;
        ...
    }
}
```

Při není zachycena, neexistuje žádný způsob, jak sledovat, přesně jak často je vytvořena instance místní proměnné, protože životnosti vytváření instancí je nesouvislý, je možné, že každá instance jednoduše používat stejné umístění úložiště. Ale když anonymní funkci explicitně zaznamenává místní proměnnou, účinky instanciace stanou zjevnými.

V příkladu
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
```
1
3
5
```

Nicméně, když deklarace `x` je přesunut mimo smyčku:
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
Výstup bude:
```
5
5
5
```

Pokud smyčky for vycházející z deklaruje proměnnou iterace, proměnné samotné se považuje za deklarované mimo smyčku. Proto pokud v příkladu se změní na zachycení proměnné iterace, sama:

```csharp
static D[] F() {
    D[] result = new D[3];
    for (int i = 0; i < 3; i++) {
        result[i] = () => { Console.WriteLine(i); };
    }
    return result;
}
```
je zachycena pouze jedna instance proměnné iterace, která vytvoří výstup:
```
3
3
3
```

Je možné pro delegáty anonymní funkce pro sdílení některých zachyceným proměnným, ale mají samostatné instance ostatních. Například pokud `F` se změní na
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
tři delegáty zachycení stejnou instanci `x` ale samostatným instancím `y`, a zobrazí se výstup:
```
1 1
2 1
3 1
```

Samostatné anonymní funkce můžete zachytit stejnou instanci vnější proměnná. V tomto příkladu:
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
dvě anonymní funkce capture stejnou instanci lokální proměnná `x`, a jejich komunikace mohla probíhat tedy"" prostřednictvím dané proměnné. Výstup v příkladu je:
```
5
10
```

### <a name="evaluation-of-anonymous-function-expressions"></a>Vyhodnocování výrazů anonymní funkce

Anonymní funkce `F` vždy musí být převeden na typ delegáta `D` nebo typu stromu výrazu `E`, buď přímo nebo prostřednictvím spuštění výraz vytvářející delegáta `new D(F)`. Tento převod určuje výsledek anonymní funkce, jak je popsáno v [anonymní funkce převody](conversions.md#anonymous-function-conversions).

## <a name="query-expressions"></a>Výrazy dotazů

***Výrazy dotazu*** poskytují syntaxi jazyka pro dotazy, které je podobné jako u jazyků relačních a hierarchických dotazů, jako jsou SQL a XQuery.

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

Výraz dotazu začíná `from` klauzule a končí buď `select` nebo `group` klauzuli. Počáteční `from` klauzule může následovat nula nebo více `from`, `let`, `where`, `join` nebo `orderby` klauzule. Každý `from` klauzule je generátor Představujeme ***proměnnou rozsahu*** které rozsahy nad elementy ***pořadí***. Každý `let` klauzule představuje proměnnou rozsahu představující hodnotu vypočítat pomocí předchozích proměnných rozsahu. Každý `where` klauzule je filtr, který vyloučí položky z výsledku. Každý `join` klauzule porovnání zadaného klíče zdrojové sekvence s klíči jiné pořadí, což má za následek odpovídající dvojice. Každý `orderby` klauzule změní pořadí položek podle zadaných kritérií. Finální `select` nebo `group` klauzule určuje tvar výsledku z hlediska proměnné rozsahu. A konečně `into` klauzuli lze použít k "splice" dotazy zpracováním výsledků dotazu jako generátor v následných dotazu.

### <a name="ambiguities-in-query-expressions"></a>Nejednoznačnosti ve výrazech dotazů

Výrazy dotazů obsahují počet "kontextová klíčová slova", to znamená, identifikátory, které mají speciální význam v daném kontextu. Konkrétně jde o `from`, `where`, `join`, `on`, `equals`, `into`, `let`, `orderby`, `ascending`, `descending`, `select`, `group` a `by`. Aby bylo možné vyhnout se nejasnostem ve výrazech dotazů způsobené smíšené použití těchto identifikátorů jako klíčová slova nebo jednoduché názvy, považují tyto identifikátory klíčová slova při, ke kterým dochází kdekoli ve výrazu dotazu.

Pro tento účel, výrazu dotazu je libovolný výraz, který začíná na "`from identifier`"následované žádný token s výjimkou"`;`","`=`"nebo"`,`".

Chcete-li použít tato slova jako identifikátory v rámci výrazu dotazu, mohou mít předponu "`@`" ([identifikátory](lexical-structure.md#identifiers)).

### <a name="query-expression-translation"></a>Překlad výrazu dotazu

Jazyk C# neurčuje sémantika provádění – výrazy dotazů. Místo toho – výrazy dotazů jsou přeloženy do volání metod, které se řídí *vzor výrazu dotazu* ([vzor výrazu dotazu](expressions.md#the-query-expression-pattern)). Konkrétně – výrazy dotazů jsou přeloženy do volání metod s názvem `Where`, `Select`, `SelectMany`, `Join`, `GroupJoin`, `OrderBy`, `OrderByDescending`, `ThenBy`, `ThenByDescending`, `GroupBy`, a `Cast`. Tyto metody se očekává, mají konkrétní podpisy a typy výsledků, jak je popsáno v [vzor výrazu dotazu](expressions.md#the-query-expression-pattern). Tyto metody mohou být metody instance objektu, která je dotazována nebo rozšiřující metody, které jsou externí vzhledem k objektu, a implementují vlastní provedení dotazu.

Překlad z – výrazy dotazů volání metod je syntaktické mapování, která předchází libovolný typ vazby nebo provedl řešení přetížení. Překlad je zaručeno, že syntakticky správný, ale není zaručena pro vytvoření sémanticky kód jazyka C#. Překlad výrazů dotazů výsledné volání metod jsou zpracovány jako volání metod regulárních, a to může zase odhalte chyby, například pokud metody neexistují, pokud máte nesprávné typy argumentů, nebo pokud jsou obecné metody, a odvození typu proměnné se nezdaří.

Výraz dotazu je zpracován opakovaně použití následujících překlady, dokud nedojde k žádné další snížení je to možné. Překlady jsou uvedeny v pořadí použití: každá část předpokládá, že byly provedeny překlady v předchozích částech vyčerpávajícím způsobem a po vyčerpání, oddíl nebude později být znovu obrácena pozornost při zpracování stejném výrazu dotazu.

Přiřazení k proměnné rozsahu není povolené ve výrazech dotazů. Implementace jazyka C# je však povoleno ne vždy vynutit toto omezení, protože to není v některých případech možné schématem syntaktické překlad okomentovat.

Některé překlady vložení proměnné rozsahu s transparentní identifikátory, označena `*`. Speciální vlastnosti transparentní identifikátorů, které jsou popsány dále v [transparentní identifikátory](expressions.md#transparent-identifiers).

#### <a name="select-and-groupby-clauses-with-continuations"></a>Klauzule SELECT a groupby s pokračováními

Výraz dotazu s pokračováním
```csharp
from ... into x ...
```
převést na
```csharp
from x in ( from ... ) ...
```

Překlady v následujících částech Předpokládejme dotazy nemají `into` pokračování.

V příkladu
```csharp
from c in customers
group c by c.Country into g
select new { Country = g.Key, CustCount = g.Count() }
```
převést na
```csharp
from g in
    from c in customers
    group c by c.Country
select new { Country = g.Key, CustCount = g.Count() }
```
je konečný překlad
```csharp
customers.
GroupBy(c => c.Country).
Select(g => new { Country = g.Key, CustCount = g.Count() })
```

#### <a name="explicit-range-variable-types"></a>Typy proměnných explicitní rozsahu

A `from` klauzuli, která explicitně určuje typ proměnné rozsahu
```csharp
from T x in e
```
převést na
```csharp
from x in ( e ) . Cast < T > ( )
```

A `join` klauzuli, která explicitně určuje typ proměnné rozsahu
```
join T x in e on k1 equals k2
```
převést na
```
join x in ( e ) . Cast < T > ( ) on k1 equals k2
```

Překlady v následujících částech předpokládá, že dotazy bez explicitního rozsahu se tyto typy proměnných.

V příkladu
```csharp
from Customer c in customers
where c.City == "London"
select c
```
převést na
```csharp
from c in customers.Cast<Customer>()
where c.City == "London"
select c
```
je konečný překlad
```csharp
customers.
Cast<Customer>().
Where(c => c.City == "London")
```

Jsou užitečné pro dotazování na kolekce, které implementují neobecné typy proměnných explicitní rozsah `IEnumerable` rozhraní, ale ne Obecné `IEnumerable<T>` rozhraní. V předchozím příkladu to může být v případě, pokud `customers` byly typu `ArrayList`.

#### <a name="degenerate-query-expressions"></a>Výrazy degenerovanou dotazů

Výraz dotazu ve formuláři
```csharp
from x in e select x
```
převést na
```csharp
( e ) . Select ( x => x )
```

V příkladu
```csharp
from c in customers
select c
```
převést na
```csharp
customers.Select(c => c)
```

Výraz degenerovanou dotazu je ten, který triviálně vybere elementy zdroje. Pozdější fáze překladu odebere degenerovanou dotazy zavedené dalších kroků a nahradí je jejich zdroji. Je důležité, ale zajistit, aby výsledek dotazu výrazu není nikdy zdrojového objektu samotného, podle typu a identitu zdroje, které by odhalit klientovi dotazu. Proto tento krok chrání degenerovanou dotazy napsané přímo ve zdrojovém kódu explicitně voláním `Select` ve zdroji. Je pak až implementátorům `Select` a jiných operátorů dotazu pro zajištění, že tyto metody nikdy vrátí samotného objektu zdroje.

#### <a name="from-let-where-join-and-orderby-clauses"></a>FROM, where, umožní klauzule join a řadit podle

Výrazu dotazu s druhou `from` klauzuli za nímž následuje `select` – klauzule
```csharp
from x1 in e1
from x2 in e2
select v
```
převést na
```csharp
( e1 ) . SelectMany( x1 => e2 , ( x1 , x2 ) => v )
```

Výraz dotazu s druhou `from` klauzuli za nímž následuje něco jiného než `select` klauzule:

```csharp
from x1 in e1
from x2 in e2
...
```
převést na
```csharp
from * in ( e1 ) . SelectMany( x1 => e2 , ( x1 , x2 ) => new { x1 , x2 } )
...
```

Výraz dotazu s `let` – klauzule
```csharp
from x in e
let y = f
...
```
převést na
```csharp
from * in ( e ) . Select ( x => new { x , y = f } )
...
```

Výraz dotazu s `where` – klauzule
```csharp
from x in e
where f
...
```
převést na
```csharp
from x in ( e ) . Where ( x => f )
...
```

Výrazu dotazu s `join` klauzule bez `into` následovaný `select` – klauzule
```csharp
from x1 in e1
join x2 in e2 on k1 equals k2
select v
```
převést na
```csharp
( e1 ) . Join( e2 , x1 => k1 , x2 => k2 , ( x1 , x2 ) => v )
```

Výrazu dotazu s `join` klauzule bez `into` za nímž následuje něco jiného než `select` – klauzule
```csharp
from x1 in e1
join x2 in e2 on k1 equals k2
...
```
převést na
```csharp
from * in ( e1 ) . Join( e2 , x1 => k1 , x2 => k2 , ( x1 , x2 ) => new { x1 , x2 })
...
```

Výrazu dotazu s `join` klauzule `into` následovaný `select` – klauzule
```csharp
from x1 in e1
join x2 in e2 on k1 equals k2 into g
select v
```
převést na
```csharp
( e1 ) . GroupJoin( e2 , x1 => k1 , x2 => k2 , ( x1 , g ) => v )
```

Výrazu dotazu s `join` klauzule `into` za nímž následuje něco jiného než `select` – klauzule
```csharp
from x1 in e1
join x2 in e2 on k1 equals k2 into g
...
```
převést na
```csharp
from * in ( e1 ) . GroupJoin( e2 , x1 => k1 , x2 => k2 , ( x1 , g ) => new { x1 , g })
...
```

Výraz dotazu pomocí `orderby` – klauzule
```csharp
from x in e
orderby k1 , k2 , ..., kn
...
```
převést na
```csharp
from x in ( e ) . 
OrderBy ( x => k1 ) . 
ThenBy ( x => k2 ) .
... .
ThenBy ( x => kn )
...
```

Pokud má za výsledek řazení klauzule určuje `descending` směrové, vyvolání `OrderByDescending` nebo `ThenByDescending` místo toho je vytvořen.

Tyto překlady se předpokládá, že neexistují žádné `let`, `where`, `join` nebo `orderby` klauzule a více než jednu počáteční `from` klauzuli v každém výrazu dotazu.

V příkladu
```csharp
from c in customers
from o in c.Orders
select new { c.Name, o.OrderID, o.Total }
```
převést na
```csharp
customers.
SelectMany(c => c.Orders,
     (c,o) => new { c.Name, o.OrderID, o.Total }
)
```

V příkladu
```csharp
from c in customers
from o in c.Orders
orderby o.Total descending
select new { c.Name, o.OrderID, o.Total }
```
převést na
```csharp
from * in customers.
    SelectMany(c => c.Orders, (c,o) => new { c, o })
orderby o.Total descending
select new { c.Name, o.OrderID, o.Total }
```
je konečný překlad
```csharp
customers.
SelectMany(c => c.Orders, (c,o) => new { c, o }).
OrderByDescending(x => x.o.Total).
Select(x => new { x.c.Name, x.o.OrderID, x.o.Total })
```
kde `x` identifikátor generovaný kompilátorem, který je jinak neviditelné a nejsou přístupné.

V příkladu
```csharp
from o in orders
let t = o.Details.Sum(d => d.UnitPrice * d.Quantity)
where t >= 1000
select new { o.OrderID, Total = t }
```
převést na
```csharp
from * in orders.
    Select(o => new { o, t = o.Details.Sum(d => d.UnitPrice * d.Quantity) })
where t >= 1000 
select new { o.OrderID, Total = t }
```
je konečný překlad
```csharp
orders.
Select(o => new { o, t = o.Details.Sum(d => d.UnitPrice * d.Quantity) }).
Where(x => x.t >= 1000).
Select(x => new { x.o.OrderID, Total = x.t })
```
kde `x` identifikátor generovaný kompilátorem, který je jinak neviditelné a nejsou přístupné.

V příkladu
```csharp
from c in customers
join o in orders on c.CustomerID equals o.CustomerID
select new { c.Name, o.OrderDate, o.Total }
```
převést na
```csharp
customers.Join(orders, c => c.CustomerID, o => o.CustomerID,
    (c, o) => new { c.Name, o.OrderDate, o.Total })
```

V příkladu
```csharp
from c in customers
join o in orders on c.CustomerID equals o.CustomerID into co
let n = co.Count()
where n >= 10
select new { c.Name, OrderCount = n }
```
převést na
```csharp
from * in customers.
    GroupJoin(orders, c => c.CustomerID, o => o.CustomerID,
        (c, co) => new { c, co })
let n = co.Count()
where n >= 10 
select new { c.Name, OrderCount = n }
```
je konečný překlad
```csharp
customers.
GroupJoin(orders, c => c.CustomerID, o => o.CustomerID,
    (c, co) => new { c, co }).
Select(x => new { x, n = x.co.Count() }).
Where(y => y.n >= 10).
Select(y => new { y.x.c.Name, OrderCount = y.n)
```
kde `x` a `y` jsou identifikátory generovaný kompilátorem, které jsou jinak neviditelné a nejsou přístupné.

V příkladu
```csharp
from o in orders
orderby o.Customer.Name, o.Total descending
select o
```
je konečný překlad
```csharp
orders.
OrderBy(o => o.Customer.Name).
ThenByDescending(o => o.Total)
```

#### <a name="select-clauses"></a>Klauzule FROM

Výraz dotazu ve formuláři
```csharp
from x in e select v
```
převést na
```csharp
( e ) . Select ( x => v )
```
s výjimkou případu, kdy v identifikátor x, překlad je jednoduše
```csharp
( e )
```

Příklad
```csharp
from c in customers.Where(c => c.City == "London")
select c
```
je jednoduše převést na
```csharp
customers.Where(c => c.City == "London")
```

#### <a name="groupby-clauses"></a>Klauzule GroupBy

Výraz dotazu ve formuláři
```csharp
from x in e group v by k
```
převést na
```csharp
( e ) . GroupBy ( x => k , x => v )
```
s výjimkou případu, kdy v identifikátoru je x, překlad
```csharp
( e ) . GroupBy ( x => k )
```

V příkladu
```csharp
from c in customers
group c.Name by c.Country
```
převést na
```csharp
customers.
GroupBy(c => c.Country, c => c.Name)
```

#### <a name="transparent-identifiers"></a>Transparentní identifikátory

Některé překlady vložení proměnné rozsahu s ***transparentní identifikátory*** udávají `*`. Transparentní identifikátory nejsou správné jazykové funkce; existují pouze jako přechodný krok v procesu překladu výrazu dotazu.

Při překladu dotazu vkládá transparentním identifikátorem, další kroky šířit do anonymní funkce a anonymní objekt inicializátory transparentním identifikátorem. V těchto kontextech transparentní identifikátory mají následující chování:

*  Dojde-li k transparentním identifikátorem jako anonymní funkce, jsou členy přidružených anonymního typu automaticky v oboru v těle anonymní funkce.
*  Pokud člen s transparentním identifikátorem je v oboru, členy, se kterou jsou v oboru také.
*  Pokud dojde k transparentním identifikátorem jako deklarátor členu v inicializátoru anonymní objekt, představuje člena s transparentním identifikátorem.
*  Ve výše uvedeného postupu překladu jsou transparentní identifikátory vždy zavedli spolu s anonymní typy se záměrem zachycení více proměnných rozsahu jako členy jednoho objektu. Implementace jazyka C# je povoleno používat jiným způsobem než anonymní typy k seskupení více proměnných rozsahu. Následující příklady překlad předpokládají, že anonymní typy se používají a zobrazit průhledné identifikátory lze přeložit okamžitě.

V příkladu
```csharp
from c in customers
from o in c.Orders
orderby o.Total descending
select new { c.Name, o.Total }
```
převést na
```csharp
from * in customers.
    SelectMany(c => c.Orders, (c,o) => new { c, o })
orderby o.Total descending
select new { c.Name, o.Total }
```

což je další přeloženy do
```csharp
customers.
SelectMany(c => c.Orders, (c,o) => new { c, o }).
OrderByDescending(* => o.Total).
Select(* => new { c.Name, o.Total })
```
což, když transparentní identifikátory jsou vymazány, je ekvivalentní k
```csharp
customers.
SelectMany(c => c.Orders, (c,o) => new { c, o }).
OrderByDescending(x => x.o.Total).
Select(x => new { x.c.Name, x.o.Total })
```
kde `x` identifikátor generovaný kompilátorem, který je jinak neviditelné a nejsou přístupné.

V příkladu
```csharp
from c in customers
join o in orders on c.CustomerID equals o.CustomerID
join d in details on o.OrderID equals d.OrderID
join p in products on d.ProductID equals p.ProductID
select new { c.Name, o.OrderDate, p.ProductName }
```
převést na
```csharp
from * in customers.
    Join(orders, c => c.CustomerID, o => o.CustomerID, 
        (c, o) => new { c, o })
join d in details on o.OrderID equals d.OrderID
join p in products on d.ProductID equals p.ProductID
select new { c.Name, o.OrderDate, p.ProductName }
```
což je dále omezit na
```csharp
customers.
Join(orders, c => c.CustomerID, o => o.CustomerID, (c, o) => new { c, o }).
Join(details, * => o.OrderID, d => d.OrderID, (*, d) => new { *, d }).
Join(products, * => d.ProductID, p => p.ProductID, (*, p) => new { *, p }).
Select(* => new { c.Name, o.OrderDate, p.ProductName })
```
je konečný překlad
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
kde `x`, `y`, a `z` jsou identifikátory generovaný kompilátorem, které jsou jinak neviditelné a nejsou přístupné.

### <a name="the-query-expression-pattern"></a>Vzor výrazu dotazu

***Vzor výrazu dotazu*** vytváří vzor metody, které implementují typy pro podporu výrazy dotazu. Protože výrazy dotazu jsou převedeny na volání metod prostřednictvím syntaktické mapování, typy mají značnou flexibilitu ve způsobu implementace vzorku dotazu výrazu. Například metody vzor je možné implementovat jako metody instance nebo jako metody rozšíření protože dva mají stejnou syntaxi volání a metody můžete vyžádat delegáty nebo stromů výrazů, protože lze převést na obojí jsou anonymní funkce.

Doporučené tvar obecného typu `C<T>` podporuje vzor výrazu dotazu je uveden níže. Obecný typ se používá k ilustraci správné relace mezi typy parametrů a výsledek, ale je možné implementovat vzor pro obecné typy a také.

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

Výše uvedené metody pomocí typů obecného delegátu `Func<T1,R>` a `Func<T1,T2,R>`, ale jejich mohl stejně dobře použít jiné typy delegát nebo výraz stromu stejné relace v typy parametrů a výsledek.

Všimněte si, že doporučená vztah mezi `C<T>` a `O<T>` který zajišťuje, že `ThenBy` a `ThenByDescending` jsou k dispozici pouze na výsledek metody `OrderBy` nebo `OrderByDescending`. Všimněte si také doporučené tvar výsledku `GroupBy` --sekvence sekvencí, ve kterém má každé vnitřní řady dalších `Key` vlastnost.

`System.Linq` Obor názvů obsahuje implementace vzorku dotazu operátor pro libovolný typ, který implementuje `System.Collections.Generic.IEnumerable<T>` rozhraní.

## <a name="assignment-operators"></a>Operátory přiřazení

Operátory přiřazení přiřadí novou hodnotu do proměnné, vlastnosti, události nebo elementu indexeru.

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

Levý operand přiřazení musí být výraz jsou klasifikovány jako proměnné, přístup k vlastnosti, přístup indexeru nebo události přístupu.

`=` Operátor je volána ***operátor jednoduchého přiřazení***. Přiřadí hodnotu pravý operand elementu proměnná, vlastnost nebo indexer Dal levý operand. Levý operand operátor jednoduchého přiřazení nemusí být přístup k události (s výjimkou případů popsaných v [události podobné poli](classes.md#field-like-events)). Operátor jednoduchého přiřazení je popsána v [jednoduché přiřazení](expressions.md#simple-assignment).

Operátory přiřazení jinak než `=` označují jako operátor ***operátory přiřazení složení***. Tyto operátory provádějí uvedené operace na dvou operandech a výslednou hodnotu pak přiřaďte elementu proměnná, vlastnost nebo indexer Dal levý operand. Složené operátory přiřazení jsou popsány v [složené přiřazení](expressions.md#compound-assignment).

`+=` a `-=` se nazývají operátory s výrazem přístup události jako levý operand *operátory přiřazení pro událost*. Žádný operátor přiřazení je platné s přístupem události jako levý operand. Operátory přiřazení události jsou popsány v [události přiřazení](expressions.md#event-assignment).

Operátory přiřazení jsou asociativní zprava, což znamená, že operace se seskupují zleva doprava. Například výraz ve tvaru `a = b = c` se vyhodnotí jako `a = (b = c)`.

### <a name="simple-assignment"></a>Jednoduché přiřazení

`=` Operátor se nazývá operátor jednoduchého přiřazení.

Pokud levý operand jednoduché přiřazení má formát `E.P` nebo `E[Ei]` kde `E` má typ kompilace `dynamic`, pak přiřazení je vázán dynamicky ([dynamické vazby](expressions.md#dynamic-binding)). V tomto případě je typ kompilaci výrazu přiřazení `dynamic`, a rozlišení je popsáno níže se provede v době běhu na základě typu za běhu `E`.

V jednoduché přiřazení pravý operand musí být výraz, který je implicitně převést na typ levého operandu. Operace přiřadí hodnotu pravý operand elementu proměnná, vlastnost nebo indexer Dal levý operand.

Výsledek výrazu jednoduché přiřazení je hodnota přiřazená k levý operand. Výsledek je stejného typu jako levý operand a vždy je klasifikován jako hodnotu.

Pokud levý operand je přístup vlastnost nebo indexer, vlastnost nebo indexovací člen musí mít `set` přistupujícího objektu. Pokud to není tento případ, dojde k chybě vazby čas.

Zpracování za běhu jednoduché přiřazení formuláře `x = y` se skládá z následujících kroků:

*  Pokud `x` je klasifikován jako proměnnou:
   * `x` je vyhodnocen pro vytvoření proměnné.
   * `y` je vyhodnocen a v případě potřeby převeden na typ `x` prostřednictvím implicitního převodu ([implicitních převodů](conversions.md#implicit-conversions)).
   * Pokud proměnná Dal `x` je prvek pole *reference_type*, kontrolu za běhu se provádí za účelem zajištění, že hodnotu vypočítat pro `y` je kompatibilní s poli instance `x` je element. Kontrola proběhne úspěšně, pokud `y` je `null`, nebo pokud implicitní referenční převod ([odkaz na implicitní převody](conversions.md#implicit-reference-conversions)) z skutečné typu instance odkazované procedurou `y` skutečným prvkem typu pole instance nadřazeného `x`. V opačném případě `System.ArrayTypeMismatchException` je vyvolána výjimka.
   * Hodnota plynoucí z hodnocení a převod `y` jsou uložena do umístění, Dal po vyhodnocení `x`.
*  Pokud `x` klasifikovaný jako vlastnost nebo indexovací člen přístupu:
   * Výraz instance (Pokud `x` není `static`) a seznam argumentů (Pokud `x` představuje přístup k indexer) přidružené k `x` jsou vyhodnocovány, a výsledky se používají v následné `set` vyvolání přistupujícího objektu.
   * `y` je vyhodnocen a v případě potřeby převeden na typ `x` prostřednictvím implicitního převodu ([implicitních převodů](conversions.md#implicit-conversions)).
   * `set` Přistupující objekt `x` je volána s hodnotou pro vypočítat `y` jako jeho `value` argument.

Pravidla pole společné odchylky ([kovariance polí](arrays.md#array-covariance)) povolit hodnotu typu pole `A[]` byl odkaz na instanci typu pole `B[]`, pokud existuje implicitní referenční převod z `B` do `A`. Protože tato pravidla přiřazení k prvku pole *reference_type* vyžaduje kontrolu za běhu k zajištění, že přiřazené hodnoty je kompatibilní s poli instance. V příkladu
```csharp
string[] sa = new string[10];
object[] oa = sa;

oa[0] = null;               // Ok
oa[1] = "Hello";            // Ok
oa[2] = new ArrayList();    // ArrayTypeMismatchException
```
způsobí, že poslední přiřazení `System.ArrayTypeMismatchException` vyvolání, protože instance `ArrayList` nelze ukládat v prvku `string[]`.

Pokud vlastnost nebo indexovací člen deklarovaný v *struct_type* cílem přiřazení, výraz instance přidružené vlastnost nebo indexer přístupu musí být zařazeny jako proměnnou. Pokud výraz instance je klasifikován tak hodnotu, dojde k chybě vazby čas. Z důvodu [přístup ke členu](expressions.md#member-access), stejné pravidlo platí také pro pole.

Zadané deklarace:
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
přiřazení k `p.X`, `p.Y`, `r.A`, a `r.B` se nepovoluje, protože `p` a `r` jsou proměnné. V tomto příkladu však
```csharp
Rectangle r = new Rectangle();
r.A.X = 10;
r.A.Y = 10;
r.B.X = 100;
r.B.Y = 100;
```
přiřazení jsou všechny neplatné, protože `r.A` a `r.B` nejsou proměnné.

### <a name="compound-assignment"></a>Složené přiřazení

Pokud levý operand složeného přiřazení má formát `E.P` nebo `E[Ei]` kde `E` má typ kompilace `dynamic`, pak přiřazení je vázán dynamicky ([dynamické vazby](expressions.md#dynamic-binding)). V tomto případě je typ kompilaci výrazu přiřazení `dynamic`, a rozlišení je popsáno níže se provede v době běhu na základě typu za běhu `E`.

Operace formuláře `x op= y` zpracování s použitím binárním operátorem rozlišení přetížení ([binárním operátorem rozlišení přetěžování](expressions.md#binary-operator-overload-resolution)) jako kdyby byla zapsána operaci `x op y`. Potom

*  Pokud je implicitně převést na typ návratového typu zvoleném operátorovi `x`, operace se vyhodnotí jako `x = x op y`, s tím rozdílem, že `x` se vyhodnotí pouze jednou.
*  Jinak, pokud vybraný operátor je předdefinovaný operátor, pokud je výslovně převeditelný na typ návratového typu zvoleném operátorovi `x`a pokud `y` implicitně převést na typ `x` nebo operátor, který je operátor, pak operace se vyhodnotí jako posunu `x = (T)(x op y)`, kde `T` je typ `x`, s tím rozdílem, že `x` se vyhodnotí pouze jednou.
*  V opačném případě složeného přiřazení je neplatný a dojde k chybě vazby čas.

Termín "vyhodnotí pouze jednou" znamená, že při vyhodnocení `x op y`, výsledky všech základních výrazy `x` jsou dočasně uloženy a pak znovu použít při provádění přiřazení `x`. Například v přiřazení `A()[B()] += C()`, kde `A` je metody vracející `int[]`, a `B` a `C` jsou metody vracející `int`, jsou metody vyvolány pouze jednou, v pořadí `A`, `B`, `C`.

Pokud levý operand složeného přiřazení je přístup k vlastnosti nebo indexeru přístup, vlastnost nebo indexovací člen musí mít obojí `get` přístupového objektu a `set` přistupujícího objektu. Pokud to není tento případ, dojde k chybě vazby čas.

Druhé pravidlo nad povolení `x op= y` má být vyhodnocen jako `x = (T)(x op y)` v některých kontextech. Existuje pravidlo tak, aby předdefinované operátory lze používat jako složené operátory, pokud levý operand je typu `sbyte`, `byte`, `short`, `ushort`, nebo `char`. I když oba argumenty jsou jednoho z těchto typů, předdefinované operátory vytvoření výsledku typu `int`, jak je popsáno v [binární číselný propagace](expressions.md#binary-numeric-promotions). Proto bez přetypování ji nebude možné přiřadit výsledek levému operandu.

Intuitivní efekt pravidla pro předdefinované operátory je jednoduše `x op= y` je povolené, pokud obě z `x op y` a `x = y` nejsou povoleny. V příkladu
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
intuitivní důvod u každé chyby je, že odpovídající jednoduché přiřazení také bylo chybu.

Zároveň to znamená, že složeného přiřazení, které podporují operace zrušeno vs. operace. V příkladu
```csharp
int? i = 0;
i += 1;             // Ok
```
operátor zdvižené `+(int?,int?)` se používá.

### <a name="event-assignment"></a>Přiřazení události

Pokud levý operand `+=` nebo `-=` operátor je klasifikován tak přístup události, pak je výraz vyhodnocen následujícím způsobem:

*  Výraz instance, pokud existuje, události přístupu je vyhodnocen.
*  Pravý operand `+=` nebo `-=` operátor je vyhodnocen a, v případě potřeby převeden na typ levého operandu prostřednictvím implicitního převodu ([implicitních převodů](conversions.md#implicit-conversions)).
*  Přístupový objekt události události se vyvolala s seznam argumentů, který se skládá z pravého operandu po vyhodnocení a v případě potřeby převodu. Pokud se operátor `+=`, `add` přístupového objektu je vyvolán; Pokud operátor, který byl `-=`, `remove` přístupového objektu je vyvolán.

Výraz přiřazení události nevydává hodnotu. Proto je platný pouze v kontextu výrazu přiřazení událostí *statement_expression* ([příkazy výrazů](statements.md#expression-statements)).

## <a name="expression"></a>Výraz

*Výraz* je buď *non_assignment_expression* nebo *přiřazení*.

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

A *constant_expression* je výraz, který může být plně vyhodnocen v době kompilace.

```antlr
constant_expression
    : expression
    ;
```

Musí být konstantní výraz `null` literál nebo hodnotu s jedním z následujících typů: `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char` , `float`, `double`, `decimal`, `bool`, `object`, `string`, nebo libovolný typ výčtu. Pouze následující konstrukce jsou povoleny v konstantních výrazech:

*  Literály (včetně `null` literál).
*  Odkazy na `const` členy třídy a struktury typů.
*  Odkazy na členy typů výčtu.
*  Odkazy na `const` parametry nebo lokální proměnné
*  V závorkách dílčí výrazy, které představují samy o sobě konstantní výrazy.
*  Výrazy přetypování, poskytuje cílový typ je jedním z typů uvedených výše.
*  `checked` a `unchecked` výrazy
*  Výrazy s výchozími hodnotami
*  Výrazy Nameof
*  Předdefinované `+`, `-`, `!`, a `~` unárních operátorů.
*  Předdefinované `+`, `-`, `*`, `/`, `%`, `<<`, `>>`, `&`, `|`, `^`, `&&`, `||`, `==`, `!=`, `<`, `>`, `<=`, a `>=` binární operátory, k dispozici každý operand je typu uvedené výše.
*  `?:` Podmiňovací operátor.

Následující převody jsou povoleny v konstantních výrazech:

*  Převody identity
*  Číselné převody
*  Výčet převody
*  Konstantní výraz převody
*  Odkaz na implicitní a explicitní převody, za předpokladu, že zdroj převody je konstantní výraz vyhodnocený na hodnotu null.

Ostatní převody, včetně zabalení, rozbalení a implicitní převody odkazů z hodnoty null nejsou povolené v konstantních výrazech. Příklad:
```csharp
class C {
    const object i = 5;         // error: boxing conversion not permitted
    const object str = "hello"; // error: implicit reference conversion
}
```
Inicializace i se o chybu, protože je vyžadován převod na uzavřené určení. Inicializace str se o chybu, protože je vyžadován implicitní referenční převod z nenulová hodnota.

Pokaždé, když se výraz splňuje všechny požadavky uvedené výše, je výraz vyhodnocen v době kompilace. To platí i v případě, že je dílčí výraz rozsáhlejšího výrazu, který obsahuje konstrukce, která není konstantní výraz.

Vyhodnocení za kompilace konstantní výrazy používá stejná pravidla jako vyhodnocení výrazů, která není konstantní, s tím rozdílem, kde by být vyvolána hodnocení za běhu výjimku, vyhodnocení za kompilace způsobí chybu kompilace dojde k.

Pokud není konstantní výraz se explicitně umístí do `unchecked` kontextu, přetečení, ke kterým dochází v integrálového typu aritmetické operace a převody během vyhodnocení za kompilace výrazu vždy způsobit chyby kompilace ([Konstantní výrazy](expressions.md#constant-expressions)).

Konstantní výrazy dojít v kontextech uvedených níže. V těchto kontextech dojde k chybě kompilace Pokud výraz nejde vyhodnotit plně v době kompilace.

*  Deklarace konstant ([konstanty](classes.md#constants)).
*  Deklarace členů výčtu ([členy výčtu](enums.md#enum-members)).
*  Výchozí argumenty seznam formálních parametrů ([parametry metody](classes.md#method-parameters))
*  `case` popisky `switch` – příkaz ([příkazu switch](statements.md#the-switch-statement)).
*  `goto case` příkazy ([příkazu goto](statements.md#the-goto-statement)).
*  Dimenze délky ve výraz vytvoření pole ([pole vytváření výrazů](expressions.md#array-creation-expressions)), která obsahuje inicializátor.
*  Atributy ([atributy](attributes.md)).

Konverzi implicitní konstantní výraz ([konstantní výraz implicitní převody](conversions.md#implicit-constant-expression-conversions)) umožňuje konstantní výraz typu `int` má být převeden na `sbyte`, `byte`, `short`, `ushort`, `uint`, nebo `ulong`, pokud má konstantní výraz hodnotu v rozsahu cílového typu.

## <a name="boolean-expressions"></a>logické výrazy

A *boolean_expression* je výraz, který poskytuje výsledek typu `bool`; buď přímo nebo prostřednictvím použití `operator true` v některých kontextech, jak je uvedeno v následující.

```antlr
boolean_expression
    : expression
    ;
```

Řízení podmíněného výrazu *if_statement* ([if – příkaz](statements.md#the-if-statement)), *while_statement* ([while – příkaz](statements.md#the-while-statement)), *do_statement* ([proveďte příkaz](statements.md#the-do-statement)), nebo *for_statement* ([pro příkaz](statements.md#the-for-statement)) je *boolean_ výraz*. Řízení podmíněného výrazu `?:` – operátor ([Podmiňovací operátor](expressions.md#conditional-operator)) následuje stejná pravidla jako *boolean_expression*, ale z důvodu operátoru je sestavení klasifikováno prioritu jako *conditional_or_expression*.

A *boolean_expression* `E` musí být schopny vytvořit hodnotu typu `bool`, následujícím způsobem:

*  Pokud `E` implicitně převést na `bool` pak za běhu se použije implicitní převodu.
*  V opačném případě unární operátor rozlišení přetěžování ([unární operátor rozlišení přetěžování](expressions.md#unary-operator-overload-resolution)) slouží k vyhledání jedinečný nejlepší implementace operátoru `true` na `E`, a tuto implementaci se použije v době běhu.
*  Pokud se nenajde žádný takový operátor, dojde k chybě vazby čas.

`DBBool` Typu Struktura v [databáze typu boolean](structs.md#database-boolean-type) poskytuje příklad, typ, který implementuje `operator true` a `operator false`.
