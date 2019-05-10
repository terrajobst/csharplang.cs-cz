---
ms.openlocfilehash: b7bb7dd575d9e2e6d5dd85bdd3e535411e29fcf4
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/25/2019
ms.locfileid: "64488868"
---
# <a name="variables"></a>Proměnné

Proměnné představují umístění úložiště. Každá proměnná má typ, který určuje, jaké hodnoty můžou být uložené v proměnné. C# je jazyk zajišťující bezpečnost typů a kompilátor jazyka C# zaručuje, že hodnoty uložené v proměnné jsou vždy příslušného typu. Hodnota proměnné lze změnit prostřednictvím přiřazení nebo použití `++` a `--` operátory.

Proměnná musí být ***jednoznačně přiřazena*** ([jednoznačného přiřazení](variables.md#definite-assignment)) před jeho hodnotu lze získat.

Jak je popsáno v následujících částech, proměnné jsou buď ***původně přiřazené*** nebo ***zpočátku nepřiřazené***. Na začátku přiřazenou proměnnou nemá jasně definované počáteční hodnotu a je vždy považován za jednoznačně přiřazena. Zpočátku Nepřiřazená proměnná nemá počáteční hodnotu. Pro proměnnou zpočátku nepřiřazené považovat jednoznačně přiřazené do určitých umístění se musí objevit přiřazení k proměnné v každé možné spuštění cesta, která vede do tohoto umístění.

## <a name="variable-categories"></a>Proměnné kategorie

C# definuje proměnných sedm kategorií: statické proměnné, instance proměnné, prvky pole, parametry s hodnotou, parametry odkazů, výstupní parametry a lokální proměnné. Následující části popisují každou z těchto kategorií.

V příkladu
```csharp
class A
{
    public static int x;
    int y;

    void F(int[] v, int a, ref int b, out int c) {
        int i = 1;
        c = a + b++;
    }
}
```
`x` je statická proměnná `y` je proměnná instance `v[0]` je k elementu pole `a` je parametr hodnota `b` je parametr odkaz `c` je výstupní parametr, a `i` místní proměnná.

### <a name="static-variables"></a>Statické proměnné

Pole deklarované s `static` modifikátor se volá ***statická proměnná***. Statická proměnná stává existence před spuštěním statického konstruktoru ([statické konstruktory](classes.md#static-constructors)) pro jeho nadřazeného typu a přestane existovat, pokud domény přidružené aplikace přestane existovat.

Počáteční hodnota statická proměnná je výchozí hodnota ([výchozí hodnoty](variables.md#default-values)) typu proměnné.

Pro účely kontroly jednoznačného přiřazení statická proměnná je považován za původně přiřazený.

### <a name="instance-variables"></a>Proměnné instance

Pole deklarované bez `static` Modifikátor je volána ***proměnnou instance***.

#### <a name="instance-variables-in-classes"></a>Instance proměnné ve třídách

Proměnná instance třídy existence stává, když se vytvoří novou instanci této třídy a přestanou existovat, pokud neexistují žádné odkazy do této instance a po provedení instance – destruktor (pokud existuje).

Počáteční hodnota proměnné instance třídy, je výchozí hodnota ([výchozí hodnoty](variables.md#default-values)) typu proměnné.

Pro účely kontroly jednoznačného přiřazení proměnnou instance třídy se považuje za původně přiřazený.

#### <a name="instance-variables-in-structs"></a>Proměnné instance ve strukturách

Proměnná instance struktury má přesně stejnou dobu platnosti jako proměnnou struktury, ke kterému patří. Jinými slovy, při vstupu do existenci nebo přestane existovat, tak proměnnou typu Struktura příliš proveďte proměnné instance struktury.

Počáteční přiřazení stavu instance proměnné struktury je stejné jako u obsahujícího proměnnou struktury. Jinými slovy, když proměnné struktury je považován za původně přiřazené, tak příliš jsou své instance proměnné a při proměnné struktury je považován za zpočátku nepřiřazené, své instance proměnné jsou rovněž nepřiřazené.

### <a name="array-elements"></a>Prvky pole

Prvky pole existence začne, když je vytvořena instance pole a přestanou existovat, pokud neexistují žádné odkazy do této instance pole.

Počáteční hodnotu všech prvků pole je výchozí hodnota ([výchozí hodnoty](variables.md#default-values)) typu prvků pole.

Pro účely kontroly jednoznačného přiřazení k elementu pole se považuje za původně přiřazený.

### <a name="value-parameters"></a>Parametry s hodnotou

Parametr deklarovaný bez `ref` nebo `out` modifikátor ***parametr hodnoty***.

Hodnota parametru stává existence při volání členské funkce (metoda, konstruktor instance, přístupový objekt nebo operátor) nebo anonymní funkce, které parametr patří, a je inicializován s hodnotou argumentu zadaný ve volání. Hodnota parametru obvykle přestane existovat po návratu funkce člena nebo anonymní funkce. Nicméně pokud má parametr hodnoty zachycena anonymní funkce ([výrazy anonymní funkce](expressions.md#anonymous-function-expressions)), jeho životnosti rozšiřuje alespoň dokud delegát nebo strom výrazu, které jsou vytvořené z této anonymní funkce má nárok na uvolnění paměti.

Pro účely kontroly jednoznačného přiřazení parametr hodnoty se považuje za původně přiřazený.

### <a name="reference-parameters"></a>Parametry odkazu

Parametr deklarovaný s `ref` modifikátor ***odkazovat na parametr***.

Parametr odkazu nevytváří nového umístění úložiště. Místo toho referenční parametr představuje stejné úložiště jako proměnná, zadaný jako argument v členské funkci nebo volání anonymní funkce. Díky tomu se hodnota parametru odkazu je vždy stejný jako základní proměnné.

Platí následující pravidla jednoznačného přiřazení k parametrům reference. Poznámka: jiná pravidla pro výstupní parametry, které jsou popsané v [výstupních parametrů](variables.md#output-parameters).

*  Proměnná musí být jednoznačně přiřazena ([jednoznačného přiřazení](variables.md#definite-assignment)) předtím, než může být předán jako referenční parametr ve volání funkce člena nebo delegáta.
*  V rámci funkce člena nebo anonymní funkce je považován za původně přiřazené parametr odkazu.

V rámci metodu instance nebo přístupový objekt instance typu Struktura `this` – klíčové slovo se chová stejně jako odkaz na parametr typu struktury ([tento přístup](expressions.md#this-access)).

### <a name="output-parameters"></a>Výstupní parametry

Parametr deklarovaný pomocí `out` modifikátor ***výstupní parametr***.

Výstupní parametr nevytváří jinam úložiště. Místo toho výstupní parametr představuje stejné úložiště jako proměnná, zadaný jako argument ve volání funkce člena nebo delegáta. Díky tomu se hodnota výstupního parametru je vždy stejný jako základní proměnné.

Platí následující pravidla jednoznačného přiřazení do výstupní parametry. Všimněte si různých pravidel pro parametry odkazů je popsáno v [odkazovat na parametry](variables.md#reference-parameters).

*  Proměnná není jednoznačně přiřazena dříve, než může být předán jako výstupní parametr v členské funkci nebo vyvolání delegáta.
*  V důsledku normálního dokončení volání funkce člena nebo delegát každou proměnnou, která byla předána jako výstupní parametr je považován za přiřazené v této cestě spuštění.
*  V rámci funkce člena nebo anonymní funkce je považován za zpočátku nepřiřazené výstupní parametr.
*  Každou výstupní parametr funkce člena nebo anonymní funkce musí být jednoznačně přiřazena ([jednoznačného přiřazení](variables.md#definite-assignment)) před funkce člena nebo anonymní funkce vrátí normálně.

V rámci konstruktoru instance typu Struktura `this` – klíčové slovo se chová stejně jako výstupní parametr typu struktury ([tento přístup](expressions.md#this-access)).

### <a name="local-variables"></a>Lokální proměnné

A ***lokální proměnná*** deklaroval *local_variable_declaration*, kterému může dojít v *bloku*, *for_statement*, *switch_statement* nebo *using_statement*; nebo *foreach_statement* nebo *specific_catch_clause* pro *try_statement*.

Doba života lokální proměnná je část provádění programu, během které je zaručeno úložiště, které budou rezervovány pro něj. Tato doba života rozšiřuje nejméně ze vstupu *bloku*, *for_statement*, *switch_statement*, *using_statement*, *foreach_statement*, nebo *specific_catch_clause* s jakou je přidružený, dokud nebude spuštění, který *bloku*, *for_statement*, *switch_statement*, *using_statement*, *foreach_statement*, nebo *specific_catch_clause* končí žádným způsobem. (Zadání uzavřené *bloku* nebo volání metody pozastaví, ale nekončí, provádění aktuálního *bloku*, *for_statement*, *switch_statement* , *using_statement*, *foreach_statement*, nebo *specific_catch_clause*.) Pokud místní proměnná je zachycen anonymní funkce ([zachyceným proměnným vnější](expressions.md#captured-outer-variables)), dobu života rozšiřuje alespoň do stromu delegáta nebo výraz anonymní funkce, společně s další objekty, které jsou vytvořené zachycené proměnné odkazovat, jsou vhodné pro uvolnění paměti.

Pokud nadřazená *bloku*, *for_statement*, *switch_statement*, *using_statement*, *foreach_statement*, nebo *specific_catch_clause* zadání rekurzivně, je vytvořena nová instance lokální proměnné pokaždé, když a jeho *local_variable_initializer*, pokud existuje, je vyhodnocen pokaždé, když.

Lokální proměnná zavedené *local_variable_declaration* nejsou inicializovány automaticky, a proto nemá žádnou výchozí hodnotu. Pro účely kontroly jednoznačného přiřazení místní proměnné zavedené *local_variable_declaration* se považuje za zpočátku nepřiřazené. A *local_variable_declaration* mohou zahrnovat *local_variable_initializer*, v takovém případě proměnné se považuje za jednoznačně přiřazená jenom po inicializační výraz ([ Příkazy deklarace](variables.md#declaration-statements)).

V rámci oboru zavedených v místní proměnné *local_variable_declaration*, je chyba kompilace k odkazování na tuto místní proměnnou v textové pozici, která předchází jeho *local_variable_declarator*. Pokud deklarace lokální proměnné je implicitní ([místní deklarace proměnné](statements.md#local-variable-declarations)), je také chybou odkazovat na proměnné v rámci jeho *local_variable_declarator*.

Lokální proměnná zavedené *foreach_statement* nebo *specific_catch_clause* považován za jeho celý rozsah jednoznačně přiřazené.

Skutečná doba života lokální proměnná je závislý na implementaci. Například kompilátor může být staticky určit, že místní proměnné v bloku se používá jenom pro malá část tohoto bloku. Pomocí této analýzy, kompilátor může vygenerovat kód, který má za následek úložiště proměnné s kratší doby života než jeho obsahujícího bloku.

Úložiště, na které odkazuje místní referenční proměnné je uvolněn nezávisle na životnost místní referenční proměnné ([Automatická správa paměti](basic-concepts.md#automatic-memory-management)).

## <a name="default-values"></a>Výchozí hodnoty

Následující kategorie proměnné jsou automaticky inicializovány na výchozí hodnoty:

*  Statické proměnné.
*  Instance proměnné instance třídy.
*  Prvky pole.

Výchozí hodnota proměnné a závisí na typu proměnné a je stanoven následujícím způsobem:

*  Pro proměnnou *value_type*, výchozí hodnota je stejná jako hodnota vypočítaná aplikací *value_type*jeho výchozí konstruktor ([výchozí konstruktory](types.md#default-constructors)).
*  Pro proměnnou *reference_type*, výchozí hodnota je `null`.

Inicializace na výchozí hodnoty se obvykle provádí tak, že správce paměti nebo systému uvolňování paměti inicializovat paměť na hodnotu nula všechny bity, předtím, než je přidělená k použití. Z tohoto důvodu je vhodné použít všechny bity žádnou k reprezentaci nulový odkaz.

## <a name="definite-assignment"></a>Jednoznačného přiřazení

V daném místě v spustitelného kódu z členské funkce, proměnné se říká, že ***jednoznačně přiřazena*** Pokud kompilátor může být velmi, podle konkrétního toku statické analýzy ([přesné pravidla pro určování jednoznačného přiřazení](variables.md#precise-rules-for-determining-definite-assignment)), proměnná automaticky inicializován nebo má cíl alespoň jedno přiřazení. Neformálně jsme uvedli, jsou pravidla jednoznačného přiřazení:

*  Na začátku přiřazenou proměnnou ([původně přiřazený proměnné](variables.md#initially-assigned-variables)) je vždy považován za jednoznačně přiřazené.
*  Na začátku nepřiřazené proměnnou ([původně nepřiřazené proměnné](variables.md#initially-unassigned-variables)) se považuje za jednoznačně přiřazené do daného umístění. Pokud všemi možnými cestami spuštění vede do tohoto umístění obsahovat alespoň jeden z následujících akcí:
    * Jednoduché přiřazení ([jednoduché přiřazení](expressions.md#simple-assignment)) v které proměnná je levý operand.
    * Výraz volání ([výrazy typu Invocation](expressions.md#invocation-expressions)) nebo výraz vytvoření objektu ([výrazy vytvoření objektu](expressions.md#object-creation-expressions)), která předá proměnná jako výstupní parametr.
    * Pro místní proměnnou, místní deklarace proměnné ([místní deklarace proměnné](statements.md#local-variable-declarations)), který obsahuje inicializátoru proměnné.

Formální specifikaci základní výše uvedených pravidel neformální je popsána v [původně přiřazený proměnné](variables.md#initially-assigned-variables), [původně nepřiřazené proměnné](variables.md#initially-unassigned-variables), a [přesné pravidla pro určování jednoznačného přiřazení](variables.md#precise-rules-for-determining-definite-assignment).

Stavy jednoznačného přiřazení instance proměnné *struct_type* proměnné jsou sledovány jako jednotlivě i hromadně. V další pravidla výše uvedené, platí následující pravidla pro *struct_type* proměnné a jejich instance proměnné:

*  Proměnná instance se považuje za jednoznačně přiřazené, pokud jeho obsahující *struct_type* proměnné se považuje za jednoznačně přiřazené.
*  A *struct_type* proměnné se považuje za jednoznačně přiřazené, pokud všechny její instance proměnné je považován za jednoznačně přiřazené.

Jednoznačného přiřazení je požadavek v následujících kontextů:

*  Proměnná musí být jednoznačně přiřazena v každém umístění, kde získat jeho hodnotu. Tím se zajistí, že nikdy nedojde nedefinované hodnoty. Získat hodnotu proměnné, kromě případů, kdy považován za výskyt proměnné ve výrazu
    * Proměnná je levý operand jednoduché přiřazení
    * Proměnná je předána jako výstupní parametr, nebo
    * Proměnná je *struct_type* proměnné a vyskytuje se jako levý operand přístup ke členu.
*  Proměnná musí být jednoznačně přiřazena v každém umístění, kde je předán jako parametr odkazu. Tím se zajistí, že můžete členské funkce volané zvažte referenční parametr původně přiřazený.
*  Všechny výstupní parametry funkce člena musí být jednoznačně přiřazena v každém umístění, kde členské funkce vrátí (až `return` příkaz nebo prostřednictvím spuštění dojde na konec těla členské funkce). Tím se zajistí, že členy funkce nevracejí nedefinované hodnoty ve výstupních parametrů, což umožní kompilátoru vzít v úvahu volání členské funkce, která přebírá proměnnou jako výstupní parametr ekvivalentní k přiřazení k proměnné.
*  `this` Proměnnou *struct_type* konstruktor instance musí být jednoznačně přiřazena v každém umístění, kde vrátí tento konstruktor instance.

### <a name="initially-assigned-variables"></a>Zpočátku přiřazených proměnných

Následující kategorie proměnné jsou klasifikovány jako původně přiřazený:

*  Statické proměnné.
*  Instance proměnné instance třídy.
*  Instance proměnné původně přiřazené struktury proměnných.
*  Prvky pole.
*  Parametry s hodnotou.
*  Odkaz na parametry.
*  Proměnné deklarované v `catch` klauzule nebo `foreach` příkazu.

### <a name="initially-unassigned-variables"></a>Zpočátku nepřiřazené proměnné

Následující kategorie proměnné jsou klasifikovány jako původně nepřiřazené:

*  Instance proměnné proměnných zpočátku nepřiřazenou strukturu.
*  Výstupní parametry, včetně `this` proměnnou struktury konstruktory instancí.
*  Lokální proměnné, s výjimkou deklarované v `catch` klauzule nebo `foreach` příkazu.

### <a name="precise-rules-for-determining-definite-assignment"></a>Přesná pravidla pro určování jednoznačného přiřazení

Aby bylo možné určit, že každou používá proměnnou je jednoznačně přiřazena, musíte použít kompilátor proces, který je ekvivalentní popsané v této části.

Kompilátor zpracovává text každého funkce člena, který má jednu nebo více zpočátku nepřiřazené proměnných. Pro každou proměnnou zpočátku nepřiřazené *v*, kompilátor Určuje ***jednoznačného přiřazení stavu*** pro *v* v každé z následujících bodů členské funkce:

*  Na začátku každého příkazu
*  Na koncovém bodu ([koncové body a dostupnosti](statements.md#end-points-and-reachability)) každého příkazu
*  Na každý oblouk který přenese ovládací prvek do jiného příkazu a koncový bod příkazu
*  Na začátku každého výrazu
*  Na konci každého výrazu

Stav jednoznačného přiřazení *v* může být buď:

*  Jednoznačně přiřazena. To znamená, že na všechny toky možné ovládací prvek do této chvíle *v* byla přiřazena hodnota.
*  Není jednoznačně přiřazena. Pro stav proměnnou na konci výrazu typu `bool`, stav proměnná, která není jednoznačně přiřazena může (ale ne nutně) spadají do jedné z následujících stavů dílčí:
    * Jednoznačně přiřazena po výraz hodnotu true. Tento stav znamená, že *v* je jednoznačně přiřazovat, je-li logický výraz vyhodnotí jako true, ale není přiřazen nutně pokud logický výraz vyhodnotí jako false.
    * Jednoznačně přiřazena po výraz hodnotu false. Tento stav znamená, že *v* je jednoznačně přiřazovat, je-li logický výraz vyhodnocen jako chybný, ale není přiřazen nutně pokud logický výraz vyhodnotí jako true.

Následující pravidla určují, jak stavu proměnné *v* je určen v každém umístění.

#### <a name="general-rules-for-statements"></a>Obecná pravidla pro příkazy

*  *v* není jednoznačně přiřazena na začátku těla členské funkce.
*  *v* je jednoznačně přiřazovat na začátku všechny nedostupné příkazu.
*  Stav jednoznačného přiřazení *v* na začátku další příkaz je určeno kontroluje se stav jednoznačného přiřazení *v* na všechny přenosy řízení toku, které se zaměřují na začátek příkaz. Pokud (a pouze v případě) *v* je jednoznačně přiřazovat na všechny tyto přenosy tok řízení, se pak *v* je jednoznačně přiřazovat na začátku prohlášení. Sadu možných řízení toku přenosů, je určena stejným způsobem jako u kontroly dostupnosti – příkaz ([koncové body a dostupnosti](statements.md#end-points-and-reachability)).
*  Stav jednoznačného přiřazení *v* na koncovém bodu bloku, `checked`, `unchecked`, `if`, `while`, `do`, `for`, `foreach`, `lock`, `using`, nebo `switch` se určí podle kontroluje se stav jednoznačného přiřazení *v* na všechny přenosy řízení toku, které se zaměřují koncový bod, který tento příkaz. Pokud *v* je jednoznačně přiřazovat na všechny tyto přenosy tok řízení, se pak *v* je jednoznačně přiřazovat na koncovém bodu příkazu. Jinak. *v* není jednoznačně přiřazena na koncovém bodu příkazu. Sadu možných řízení toku přenosů, je určena stejným způsobem jako u kontroly dostupnosti – příkaz ([koncové body a dostupnosti](statements.md#end-points-and-reachability)).

#### <a name="block-statements-checked-and-unchecked-statements"></a>Blok příkazů, zaškrtnutí a zaškrtnuté políčko příkazů

Stav jednoznačného přiřazení *v* na ovládacím prvku přenos první příkaz seznamu příkazů v bloku (nebo koncový bod bloku, pokud je prázdný seznam příkazů) je stejný jako příkaz jednoznačného přiřazení *v* před blokem, `checked`, nebo `unchecked` příkazu.

#### <a name="expression-statements"></a>Příkazy výrazů

Pro příkaz výrazu *příkazu Insert* , který se skládá z výrazu *expr*:

*  *v* má stejné jednoznačného přiřazení stavu na začátku *expr* stejně jako na začátku *příkazu Insert*.
*  Pokud *v* pokud jednoznačně přiřazena na konci *expr*, je jednoznačně přiřazena na koncový bod *příkazu Insert*; jinak vrátí hodnotu; není jednoznačně přiřazena na koncový bod *příkazu Insert*.

#### <a name="declaration-statements"></a>Příkazy deklarace

*  Pokud *příkazu Insert* příkazu deklarace bez inicializátorů, pak je *v* má stejný stav jednoznačného přiřazení na koncový bod *příkazu Insert* stejně jako na začátku *příkazu Insert*.
*  Pokud *příkazu Insert* příkazu deklarace s inicializátory, pak je stav jednoznačného přiřazení *v* je určen jako *příkazu Insert* byly seznam příkazů, pomocí jedné úlohy příkaz pro každou deklaraci s inicializátorem (v pořadí deklarace).

#### <a name="if-statements"></a>Pokud se příkazy

Pro `if` příkaz *příkazu Insert* formuláře:
```csharp
if ( expr ) then_stmt else else_stmt
```

*  *v* má stejné jednoznačného přiřazení stavu na začátku *expr* stejně jako na začátku *příkazu Insert*.
*  Pokud *v* je jednoznačně přiřazovat na konci *expr*, pak je jednoznačně přiřazena na přenos řízení toku do *then_stmt* a buď *else_stmt*  nebo koncový bod z *příkazu Insert* Pokud neexistuje žádná klauzule else.
*  Pokud *v* má stav "jednoznačně přiřazena po hodnotu true, výraz" na konci *expr*, pak je jednoznačně přiřazena na přenos řízení toku do *then_stmt*a ne určitě přiřazené na toku přenosu ovládacího prvku buď *else_stmt* nebo koncový bod z *příkazu Insert* Pokud neexistuje žádná klauzule else.
*  Pokud *v* má stav "jednoznačně přiřazena po false výraz" na konci *expr*, pak je jednoznačně přiřazena na přenos řízení toku do *else_stmt*a ne na přenos řízení toku do jednoznačně přiřazena *then_stmt*. Je jednoznačně přiřazena na koncový bod z *příkazu Insert* pouze v případě je jednoznačně přiřazena na koncový bod z *then_stmt*.
*  V opačném případě *v* je považováno za nevyhovující jednoznačně přiřazené na toku přenosu ovládacího prvku buď *then_stmt* nebo *else_stmt*, nebo koncový bod z  *příkazu Insert* Pokud neexistuje žádná klauzule else.

#### <a name="switch-statements"></a>Příkazy Switch

V `switch` příkaz *příkazu Insert* s řídicí výraz *expr*:

*  Stav jednoznačného přiřazení *v* na začátku *expr* je stejné jako stav *v* na začátku *příkazu Insert*.
*  Stav jednoznačného přiřazení *v* v toku řízení přenosu do seznamu dostupný přepínač bloku příkazu je stejné jako stav jednoznačného přiřazení *v* na konci *expr*.

#### <a name="while-statements"></a>While – příkazy

Pro `while` příkaz *příkazu Insert* formuláře:
```csharp
while ( expr ) while_body
```

*  *v* má stejné jednoznačného přiřazení stavu na začátku *expr* stejně jako na začátku *příkazu Insert*.
*  Pokud *v* je jednoznačně přiřazovat na konci *expr*, pak je jednoznačně přiřazena na přenos řízení toku do *while_body* a koncový bod  *příkazu Insert*.
*  Pokud *v* má stav "jednoznačně přiřazena po hodnotu true, výraz" na konci *expr*, pak je jednoznačně přiřazena na přenos řízení toku do *while_body*, ale ne jednoznačně přiřazena na koncový bod z *příkazu Insert*.
*  Pokud *v* má stav "jednoznačně přiřazena po false výraz" na konci *expr*, pak je jednoznačně přiřazena na toku přenosu ovládacího prvku do koncového bodu *příkazu Insert* , ale není přiřazen jednoznačně na přenos řízení toku do *while_body*.

#### <a name="do-statements"></a>Proveďte příkazy

Pro `do` příkaz *příkazu Insert* formuláře:
```csharp
do do_body while ( expr ) ;
```

*  *v* má stejné jednoznačného přiřazení stavu v přenos řízení toku od začátku *příkazu Insert* k *do_body* stejně jako na začátku *příkazu Insert*.
*  *v* má stejné jednoznačného přiřazení stavu na začátku *expr* za koncový bod *do_body*.
*  Pokud *v* je jednoznačně přiřazovat na konci *expr*, pak je jednoznačně přiřazena na toku přenosu ovládacího prvku do koncového bodu *příkazu Insert*.
*  Pokud *v* má stav "jednoznačně přiřazena po false výraz" na konci *expr*, pak je jednoznačně přiřazena na toku přenosu ovládacího prvku do koncového bodu *příkazu Insert* .

#### <a name="for-statements"></a>Pro příkazy

Kontrola jednoznačného přiřazení `for` příkazu ve formátu:
```csharp
for ( for_initializer ; for_condition ; for_iterator ) embedded_statement
```
Probíhá, jako kdyby byly napsány příkaz:
```csharp
{
    for_initializer ;
    while ( for_condition ) {
        embedded_statement ;
        for_iterator ;
    }
}
```

Pokud *for_condition* je vynecháno z `for` příkaz a pak vyhodnocování pokračuje jednoznačného přiřazení jakoby *for_condition* byla nahrazena `true` ve výše uvedené rozšíření .

#### <a name="break-continue-and-goto-statements"></a>Přerušit, pokračovat a příkazy goto

Stav jednoznačného přiřazení *v* na přenos řízení toku způsobené `break`, `continue`, nebo `goto` příkaz je stejné jako stav jednoznačného přiřazení *v* na začátek příkazu.

#### <a name="throw-statements"></a>Throw – příkazy

Pro příkaz *příkazu Insert* formuláře
```csharp
throw expr ;
```

Stav jednoznačného přiřazení *v* na začátku *expr* je stejné jako stav jednoznačného přiřazení *v* na začátku *příkazu Insert*.

#### <a name="return-statements"></a>Příkazy Return

Pro příkaz *příkazu Insert* formuláře
```csharp
return expr ;
```

*  Stav jednoznačného přiřazení *v* na začátku *expr* je stejné jako stav jednoznačného přiřazení *v* na začátku *příkazu Insert*.
*  Pokud *v* je výstupní parametr, pak ho musí být jednoznačně přiřazena buď:
    * Po *výraz*
    * nebo na konci `finally` bloku `try` - `finally` nebo `try` - `catch` - `finally` , který obklopuje `return` příkazu.

Pro příkaz příkazu INSERT formuláře:
```csharp
return ;
```

*  Pokud *v* je výstupní parametr, pak ho musí být jednoznačně přiřazena buď:
    * před *příkazu INSERT*
    * nebo na konci `finally` bloku `try` - `finally` nebo `try` - `catch` - `finally` , který obklopuje `return` příkazu.

#### <a name="try-catch-statements"></a>Try-catch – příkazy

Pro příkaz *příkazu Insert* formuláře:
```csharp
try try_block
catch(...) catch_block_1
...
catch(...) catch_block_n
```

*  Stav jednoznačného přiřazení *v* na začátku *try_block* je stejné jako stav jednoznačného přiřazení *v* na začátku *příkazu Insert*.
*  Stav jednoznačného přiřazení *v* na začátku *catch_block_i* (pro všechny *můžu*) je stejný jako stav jednoznačného přiřazení *v*na začátku *příkazu Insert*.
*  Stav jednoznačného přiřazení *v* na koncový bod z *příkazu Insert* je jednoznačně přiřazené if (a pouze v případě) *v* je jednoznačně přiřazovat na koncový bod z  *try_block* a každý *catch_block_i* (pro každý *můžu* od 1 do *n*).

#### <a name="try-finally-statements"></a>Try-finally – příkazy

Pro `try` příkaz *příkazu Insert* formuláře:
```csharp
try try_block finally finally_block
```

*  Stav jednoznačného přiřazení *v* na začátku *try_block* je stejné jako stav jednoznačného přiřazení *v* na začátku *příkazu Insert*.
*  Stav jednoznačného přiřazení *v* na začátku *finally_block* je stejné jako stav jednoznačného přiřazení *v* na začátku *příkazu INSERT* .
*  Stav jednoznačného přiřazení *v* na koncový bod z *příkazu Insert* je jednoznačně přiřazené if (a pouze v případě) platí alespoň jedna z následujících akcí:
    * *v* je jednoznačně přiřazovat na koncový bod z *try_block*
    * *v* je jednoznačně přiřazovat na koncový bod z *finally_block*

Pokud přenos toku řízení (například `goto` příkaz), která začne v rámci *try_block*a končí mimo *try_block*, pak *v* je také považován za jednoznačně přiřazené na přenos řízení toku v případě *v* je jednoznačně přiřazovat na koncový bod z *finally_block*. (Toto není právě tehdy, pokud *v* je jednoznačně přiřazovat z nějakého jiného důvodu na tento převod řízení toku, a přesto považuje jednoznačně přiřazené.)

#### <a name="try-catch-finally-statements"></a>Konstrukce try-catch – finally – příkazy

Analýza jednoznačného přiřazení `try` - `catch` - `finally` příkazu ve formátu:
```csharp
try try_block
catch(...) catch_block_1
...
catch(...) catch_block_n
finally *finally_block*
```
se provádí, jako kdyby byly příkaz `try` - `finally` uzavírající příkazu `try` - `catch` – příkaz:
```csharp
try {
    try try_block
    catch(...) catch_block_1
    ...
    catch(...) catch_block_n
}
finally finally_block
```

Následující příklad ukazuje, jak různé bloky `try` – příkaz ([příkazu try](statements.md#the-try-statement)) ovlivnit jednoznačného přiřazení.
```csharp
class A
{
    static void F() {
        int i, j;
        try {
            goto LABEL;
            // neither i nor j definitely assigned
            i = 1;
            // i definitely assigned
        }

        catch {
            // neither i nor j definitely assigned
            i = 3;
            // i definitely assigned
        }

        finally {
            // neither i nor j definitely assigned
            j = 5;
            // j definitely assigned
            }
        // i and j definitely assigned
        LABEL:;
        // j definitely assigned

    }
}
```

#### <a name="foreach-statements"></a>Příkazy foreach

Pro `foreach` příkaz *příkazu Insert* formuláře:
```csharp
foreach ( type identifier in expr ) embedded_statement
```

*  Stav jednoznačného přiřazení *v* na začátku *expr* je stejné jako stav *v* na začátku *příkazu Insert*.
*  Stav jednoznačného přiřazení *v* na přenos řízení toku do *embedded_statement* nebo koncový bod *příkazu Insert* je stejné jako stav *v* na konci *expr*.

#### <a name="using-statements"></a>Pomocí příkazů

Pro `using` příkaz *příkazu Insert* formuláře:
```csharp
using ( resource_acquisition ) embedded_statement
```

*  Stav jednoznačného přiřazení *v* na začátku *resource_acquisition* je stejné jako stav *v* na začátku *příkazu Insert*.
*  Stav jednoznačného přiřazení *v* na přenos řízení toku do *embedded_statement* je stejné jako stav *v* na konci *resource_ získání*.

#### <a name="lock-statements"></a>Příkazy zámku

Pro `lock` příkaz *příkazu Insert* formuláře:
```csharp
lock ( expr ) embedded_statement
```

*  Stav jednoznačného přiřazení *v* na začátku *expr* je stejné jako stav *v* na začátku *příkazu Insert*.
*  Stav jednoznačného přiřazení *v* na přenos řízení toku do *embedded_statement* je stejné jako stav *v* na konci *expr*.

#### <a name="yield-statements"></a>Příkazy YIELD

Pro `yield return` příkaz *příkazu Insert* formuláře:
```csharp
yield return expr ;
```

*  Stav jednoznačného přiřazení *v* na začátku *expr* je stejné jako stav *v* na začátku *příkazu Insert*.
*  Stav jednoznačného přiřazení *v* na konci *příkazu Insert* je stejné jako stav *v* na konci *expr*.
*  A `yield break` příkaz nemá žádný vliv na stav jednoznačného přiřazení.

#### <a name="general-rules-for-simple-expressions"></a>Obecná pravidla pro jednoduché výrazy

Následující pravidlo se vztahuje na tyto druhy výrazů: literály ([literály](expressions.md#literals)), jednoduché názvy ([jednoduché názvy](expressions.md#simple-names)), výrazy přístupu členů ([přístup ke členu](expressions.md#member-access)), výrazy neindexovanou základní přístupu ([základní přístup](expressions.md#base-access)), `typeof` výrazy ([typeof – operátor](expressions.md#the-typeof-operator)), výchozí hodnota výrazy ([výchozí hodnotu výrazů ](expressions.md#default-value-expressions)) a `nameof` výrazy ([výrazy Nameof](expressions.md#nameof-expressions)).

*  Stav jednoznačného přiřazení *v* na konci takový výraz je stejné jako stav jednoznačného přiřazení *v* na začátku výrazu.

#### <a name="general-rules-for-expressions-with-embedded-expressions"></a>Obecná pravidla pro výrazy s vložené výrazy

Následující pravidla platí pro tyto druhy výrazů: výrazy v závorkách ([výrazech se závorkami](expressions.md#parenthesized-expressions)), výrazy přístupu – element ([přístup k prvkům](expressions.md#element-access)), základní přístup výrazy s indexování ([základní přístup](expressions.md#base-access)), zvýší a sníží výrazy ([Příponové operátory Inkrementace a dekrementace operátory](expressions.md#postfix-increment-and-decrement-operators), [předpony Inkrementace a dekrementace operátory](expressions.md#prefix-increment-and-decrement-operators)), výrazy přetypování ([výrazy přetypování](expressions.md#cast-expressions)), unární `+`, `-`, `~`, `*` výrazy, binární `+`, `-`, `*`, `/`, `%`, `<<`, `>>`, `<`, `<=`, `>`, `>=`, `==`, `!=`, `is`, `as`, `&`, `|`, `^` výrazy ([aritmetické operátory](expressions.md#arithmetic-operators), [operátory posunutí](expressions.md#shift-operators), [relační a typové zkoušky operátory](expressions.md#relational-and-type-testing-operators) [Logické operátory](expressions.md#logical-operators)), složené výrazy přiřazení ([složené přiřazení](expressions.md#compound-assignment)), `checked` a `unchecked` výrazy ([zaškrtnuto a nezaškrtnuto operátory](expressions.md#the-checked-and-unchecked-operators)), plus pole a delegáta vytváření výrazů ([operátor new](expressions.md#the-new-operator)).

Každá z těchto výrazů má nejméně jeden dílčí výrazy, které jsou bezpodmínečně vyhodnocovány v pořadí pevné. Například binární `%` operátor vyhodnotí levé straně operátoru a potom na pravé straně. Operace indexování vyhodnotí indexovaným výrazem a pak vyhodnotí každý indexové výrazy v pořadí zleva doprava. Pro výraz *expr*, který má dílčí výrazy *e1, e2,..., eN*, Vyhodnocená v tomto pořadí:

*  Stav jednoznačného přiřazení *v* na začátku *e1* je stejný jako jednoznačného přiřazení stavu na začátku *expr*.
*  Stav jednoznačného přiřazení *v* na začátku *ei* (*můžu* větší než jedna) je stejný jako stav jednoznačného přiřazení na konci předchozí dílčí výraz.
*  Stav jednoznačného přiřazení *v* na konci *expr* je stejné jako stav jednoznačného přiřazení na konci *eN*

#### <a name="invocation-expressions-and-object-creation-expressions"></a>Výrazy typu Invocation a vytváření výrazy

Výraz volání *expr* formuláře:
```csharp
primary_expression ( arg1 , arg2 , ... , argN )
```
nebo výraz vytvoření objektu ve formátu:
```csharp
new type ( arg1 , arg2 , ... , argN )
```

*  Pro výraz vyvolání jednoznačného přiřazení stavu *v* před *primary_expression* je stejné jako stav *v* před *expr*.
*  Pro výraz vyvolání jednoznačného přiřazení stavu *v* před *arg1* je stejné jako stav *v* po *primary_expression*.
*  Pro vytvoření výrazu objektu, stav jednoznačného přiřazení *v* před *arg1* je stejné jako stav *v* před *expr*.
*  Pro každý argument *argi*, stav jednoznačného přiřazení *v* po *argi* se určuje podle pravidla normální výrazů, ignoruje všechny `ref` nebo `out`modifikátory.
*  Pro každý argument *argi* libovolné *můžu* větší než jedna, stav jednoznačného přiřazení *v* před *argi* je stejné jako stav *v* po předchozí *arg*.
*  Pokud proměnná *v* je předán jako `out` argument (tj, argument formuláře `out v`) v některé z argumentů, potom stav *v* po *expr* je jednoznačně přiřazena. Jinak. Stav *v* po *expr* je stejné jako stav *v* po *argN*.
*  Pro Inicializátory polí ([pole vytváření výrazů](expressions.md#array-creation-expressions)), inicializátorech objektu ([inicializátorech objektu](expressions.md#object-initializers)), inicializátory kolekce ([inicializátory kolekce](expressions.md#collection-initializers)) a Inicializátory anonymních objektů ([anonymní objekt vytváření výrazů](expressions.md#anonymous-object-creation-expressions)), stav jednoznačného přiřazení je určeno rozšíření, které tyto konstrukce jsou definovány z hlediska.

#### <a name="simple-assignment-expressions"></a>Jednoduché přiřazení výrazy

Pro výraz *expr* formuláře `w = expr_rhs`:

*  Stav jednoznačného přiřazení *v* před *expr_rhs* je stejné jako stav jednoznačného přiřazení *v* před *expr*.
*  Stav jednoznačného přiřazení *v* po *expr* se určuje podle:
   * Pokud *w* stejnou proměnnou, jako je *v*, pak jednoznačného přiřazení stavu *v* po *expr* jednoznačně přiřazena.
   * Jinak, pokud dojde k přiřazení v rámci konstruktoru instance typu struktury, pokud *w* je přístup k vlastnosti s vyznačením automaticky implementovanou vlastnost *P* vytváří instance a *v* je pole Skrytá zálohování *P*, pak jednoznačného přiřazení stavu *v* po *expr* se o výborný prostředek přiřadit.
   * V opačném případě jednoznačného přiřazení stavu *v* po *expr* je stejné jako stav jednoznačného přiřazení *v* po *expr_rhs*.

#### <a name="-conditional-and-expressions"></a>& & (podmiňovací operátor AND) výrazů

Pro výraz *expr* formuláře `expr_first && expr_second`:

*  Stav jednoznačného přiřazení *v* před *expr_first* je stejné jako stav jednoznačného přiřazení *v* před *expr*.
*  Stav jednoznačného přiřazení *v* před *expr_second* je jednoznačně přiřazovat, pokud státu *v* po *expr_first* je buď určitě přiřazené nebo "přiřazené jednoznačně po Výraz true". V opačném případě ji není jednoznačně přiřazena.
*  Stav jednoznačného přiřazení *v* po *expr* se určuje podle:
    * Pokud *expr_first* je konstantní výraz s hodnotou `false`, pak jednoznačného přiřazení stavu *v* po *expr* je stejný jako jednoznačného přiřazení Stav *v* po *expr_first*.
    * Jinak, pokud státu *v* po *expr_first* jednoznačně přiřazena, potom stav *v* po *expr* jednoznačně přiřazena.
    * Jinak, pokud stav *v* po *expr_second* jednoznačně přiřazena a stav *v* po *expr_first* je "jednoznačně přiřazené po false výraz"a pak stav *v* po *expr* jednoznačně přiřazena.
    * Jinak, pokud státu *v* po *expr_second* jednoznačně přiřazena nebo "přiřazené jednoznačně po hodnotu true, výraz", potom stav *v* po  *výraz* "jednoznačně přiřazena po Výraz true".
    * Jinak, pokud státu *v* po *expr_first* je "přiřazené jednoznačně po false výraz" a stav *v* po *expr_second* "přiřazené jednoznačně po false výraz", je stav *v* po *expr* "jednoznačně přiřazena po výrazu false".
    * V opačném případě stav *v* po *expr* není jednoznačně přiřazena.

V příkladu
```csharp
class A
{
    static void F(int x, int y) {
        int i;
        if (x >= 0 && (i = y) >= 0) {
            // i definitely assigned
        }
        else {
            // i not definitely assigned
        }
        // i not definitely assigned
    }
}
```
proměnné `i` se považuje za jednoznačně přiřazené v jednom z vložené příkazy `if` příkazu, ale ne v jiném. V `if` příkaz v metodě `F`, proměnná `i` je jednoznačně přiřazena první příkaz vložený, protože provádění výrazu `(i = y)` vždy předchází spuštění tohoto příkazu embedded. Naproti tomu, proměnná `i` není jednoznačně přiřazena v druhém příkazu vložený, protože `x >= 0` může otestovali false, což vede k proměnné `i` se nepřiřazený.

#### <a name="-conditional-or-expressions"></a>|| (podmiňovací operátor OR) výrazů

Pro výraz *expr* formuláře `expr_first || expr_second`:

*  Stav jednoznačného přiřazení *v* před *expr_first* je stejné jako stav jednoznačného přiřazení *v* před *expr*.
*  Stav jednoznačného přiřazení *v* před *expr_second* je jednoznačně přiřazovat, pokud státu *v* po *expr_first* je buď určitě přiřazené nebo "přiřazené jednoznačně po výrazu false". V opačném případě ji není jednoznačně přiřazena.
*  Příkaz jednoznačného přiřazení *v* po *expr* se určuje podle:
    * Pokud *expr_first* je konstantní výraz s hodnotou `true`, pak jednoznačného přiřazení stavu *v* po *expr* je stejný jako jednoznačného přiřazení Stav *v* po *expr_first*.
    * Jinak, pokud státu *v* po *expr_first* jednoznačně přiřazena, potom stav *v* po *expr* jednoznačně přiřazena.
    * Jinak, pokud stav *v* po *expr_second* jednoznačně přiřazena a stav *v* po *expr_first* je "jednoznačně přiřazené po hodnotu true, výraz"a pak stav *v* po *expr* jednoznačně přiřazena.
    * Jinak, pokud státu *v* po *expr_second* jednoznačně přiřazena nebo "přiřazené jednoznačně po false výraz", potom stav *v* po *expr* "jednoznačně přiřazena po výrazu false".
    * Jinak, pokud státu *v* po *expr_first* je "přiřazené jednoznačně po hodnotu true, výraz" a stav *v* po *expr_second*"přiřazené jednoznačně po hodnotu true, výraz", je stav *v* po *expr* "jednoznačně přiřazena po Výraz true".
    * V opačném případě stav *v* po *expr* není jednoznačně přiřazena.

V příkladu
```csharp
class A
{
    static void G(int x, int y) {
        int i;
        if (x >= 0 || (i = y) >= 0) {
            // i not definitely assigned
        }
        else {
            // i definitely assigned
        }
        // i not definitely assigned
    }
}
```
proměnné `i` se považuje za jednoznačně přiřazené v jednom z vložené příkazy `if` příkazu, ale ne v jiném. V `if` příkaz v metodě `G`, proměnná `i` jednoznačně přiřazena v druhém příkazu vložený protože provádění výrazu `(i = y)` vždy předchází spuštění tohoto příkazu embedded. Naproti tomu, proměnná `i` není jednoznačně přiřazena v první příkaz vložený, protože `x >= 0` může otestovali nastavena hodnota true, což vede k proměnné `i` se nepřiřazený.

#### <a name="-logical-negation-expressions"></a>! výrazy (Logická negace)

Pro výraz *expr* formuláře `! expr_operand`:

*  Stav jednoznačného přiřazení *v* před *expr_operand* je stejné jako stav jednoznačného přiřazení *v* před *expr*.
*  Stav jednoznačného přiřazení *v* po *expr* se určuje podle:
    * Pokud stav *v* po * expr_operand * je jednoznačně přiřazena, potom stav *v* po *expr* jednoznačně přiřazena.
    * Pokud státu *v* po * expr_operand * není jednoznačně přiřazena, potom stav *v* po *expr* není jednoznačně přiřazena.
    * Pokud stav *v* po * expr_operand * "přiřazené jednoznačně po false výraz", je stav *v* po *expr* "jednoznačně přiřazena po true výraz".
    * Pokud stav *v* po * expr_operand * "přiřazené jednoznačně po hodnotu true, výraz", je stav *v* po *expr* "jednoznačně přiřazena po false výraz".

#### <a name="-null-coalescing-expressions"></a>?? výrazy (nulové sloučení)

Pro výraz *expr* formuláře `expr_first ?? expr_second`:

*  Stav jednoznačného přiřazení *v* před *expr_first* je stejné jako stav jednoznačného přiřazení *v* před *expr*.
*  Stav jednoznačného přiřazení *v* před *expr_second* je stejné jako stav jednoznačného přiřazení *v* po *expr_first*.
*  Příkaz jednoznačného přiřazení *v* po *expr* se určuje podle:
    * Pokud *expr_first* je konstantní výraz ([konstantní výrazy](expressions.md#constant-expressions)) s hodnotou null, pak bude stav *v* po *expr* stejný jako stav *v* po *expr_second*.
*  V opačném případě stav *v* po *expr* je stejné jako stav jednoznačného přiřazení *v* po *expr_first*.

#### <a name="-conditional-expressions"></a>?: (podmínky)

Pro výraz *expr* formuláře `expr_cond ? expr_true : expr_false`:

*  Stav jednoznačného přiřazení *v* před *expr_cond* je stejné jako stav *v* před *expr*.
*  Stav jednoznačného přiřazení *v* před *expr_true* jednoznačně přiřazena pouze v případě obsahuje jeden z následujících akcí:
    * *expr_cond* je konstantní výraz s hodnotou `false`
    * Stav *v* po *expr_cond* je jednoznačně přiřazovat nebo "jednoznačně přiřazena po Výraz true".
*  Stav jednoznačného přiřazení *v* před *expr_false* jednoznačně přiřazena pouze v případě obsahuje jeden z následujících akcí:
    * *expr_cond* je konstantní výraz s hodnotou `true`
*  Stav *v* po *expr_cond* je jednoznačně přiřazovat nebo "jednoznačně přiřazena po výrazu false".
*  Stav jednoznačného přiřazení *v* po *expr* se určuje podle:
    * Pokud *expr_cond* je konstantní výraz ([konstantní výrazy](expressions.md#constant-expressions)) s hodnotou `true` pak stav *v* po *expr* je stejný jako stav *v* po *expr_true*.
    * Jinak, pokud *expr_cond* je konstantní výraz ([konstantní výrazy](expressions.md#constant-expressions)) s hodnotou `false` pak stav *v* po *expr* je stejné jako stav *v* po *expr_false*.
    * Jinak, pokud státu *v* po *expr_true* je jednoznačně přiřazovat a stav *v* po *expr_false* se o výborný prostředek pak přiřazen stav *v* po *expr* jednoznačně přiřazena.
    * V opačném případě stav *v* po *expr* není jednoznačně přiřazena.

#### <a name="anonymous-functions"></a>Anonymní funkce

Pro *lambda_expression* nebo *anonymous_method_expression* *expr* s tělem (buď *bloku* nebo *výraz* ) *tělo*:

*  Stav jednoznačného přiřazení vnější proměnná *v* před *tělo* je stejné jako stav *v* před *expr*. To znamená jednoznačného přiřazení stavu vnější proměnné je zděděno z kontextu anonymní funkce.
*  Stav jednoznačného přiřazení vnější proměnná *v* po *expr* je stejné jako stav *v* před *expr*.

V příkladu
```csharp
delegate bool Filter(int i);

void F() {
    int max;

    // Error, max is not definitely assigned
    Filter f = (int n) => n < max;

    max = 5;
    DoWork(f);
}
```
vygeneruje chybu kompilace od `max` není jednoznačně přiřazena, ve kterém je deklarována jako anonymní funkce. V příkladu
```csharp
delegate void D();

void F() {
    int n;
    D d = () => { n = 1; };

    d();

    // Error, n is not definitely assigned
    Console.WriteLine(n);
}
```
také vygeneruje chybu kompilace od přiřazení `n` v anonymní funkce nemá žádný vliv na stav jednoznačného přiřazení `n` vně anonymní funkce.

## <a name="variable-references"></a>Odkazy na proměnné

A *variable_reference* je *výraz* , který je klasifikován jako proměnnou. A *variable_reference* označuje umístění úložiště, který je přístupný načíst aktuální hodnotu a uložte novou hodnotu.

```antlr
variable_reference
    : expression
    ;
```

V jazyce C a C++, *variable_reference* se označuje jako *l-hodnoty*.

## <a name="atomicity-of-variable-references"></a>Atomicitu odkazy na proměnné

Čtení a zápisy následující datové typy jsou atomické: `bool`, `char`, `byte`, `sbyte`, `short`, `ushort`, `uint`, `int`, `float`a typy odkazů. Kromě toho přečtených a zapsaných typů výčtů se základním typem v předchozím seznamu jsou také atomické. Operace čtení a zápisu z ostatních typů, včetně `long`, `ulong`, `double`, a `decimal`, a také uživatelem definovaných typů, nemusí být atomické. Kromě funkcí knihovny určená pro tento účel neexistuje žádná záruka z atomických čtení modify-write, například v případě zvýšení nebo snížení.

