---
ms.openlocfilehash: a01cf9387b8dc47de036bf0bd1496c19a441d81c
ms.sourcegitcommit: 7f7fc6e9e195e51b7ff8229aeaa70aa9fbbb63cb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/10/2019
ms.locfileid: "70876794"
---
# <a name="variables"></a>Proměnné

Proměnné reprezentují umístění úložiště. Každá proměnná má typ, který určuje, jaké hodnoty mohou být uloženy v proměnné. C#je typově bezpečný jazyk a C# kompilátor garantuje, že hodnoty uložené v proměnných jsou vždy příslušného typu. Hodnotu proměnné lze změnit pomocí přiřazení nebo `++` pomocí operátorů a. `--`

Aby bylo možné získat jeho hodnotu, musí být proměnná ***jednoznačně přiřazená*** ([jednoznačné přiřazení](variables.md#definite-assignment)).

Jak je popsáno v následujících oddílech, jsou proměnné buď ***zpočátku přiřazené*** , nebo ***zpočátku nepřiřazené***. Původně přiřazená proměnná má jasně definovanou počáteční hodnotu a je vždy považována za jednoznačně přiřazenou. Počáteční Nepřiřazená proměnná nemá počáteční hodnotu. Pro původně nepřiřazenou proměnnou, která má být považována za jednoznačně přiřazenou v určitém umístění, musí být přiřazení proměnné provedeno v každé možné cestě provádění, která vede k tomuto umístění.

## <a name="variable-categories"></a>Kategorie proměnných

C#definuje sedm kategorií proměnných: statické proměnné, proměnné instancí, prvky pole, parametry hodnot, referenční parametry, výstupní parametry a místní proměnné. Níže uvedené části popisují každou z těchto kategorií.

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
`x`je statická proměnná, `y` je proměnná instance, `v[0]` je elementem pole, `a` je parametrem hodnoty, `b` je parametrem odkazu, `c` je výstupní parametr a `i` je místní proměnná. .

### <a name="static-variables"></a>Statické proměnné

Pole deklarované s `static` modifikátorem se nazývá ***statická proměnná***. Statická proměnná se vyskytuje před provedením statického konstruktoru ([statických konstruktorů](classes.md#static-constructors)) pro svůj nadřazený typ a přestane existovat, pokud přidružená aplikační doména přestane existovat.

Počáteční hodnota statické proměnné je výchozí hodnota ([výchozí hodnoty](variables.md#default-values)) typu proměnné.

Pro účely jednoznačné kontroly přiřazení je statická proměnná považována za původně přiřazenou.

### <a name="instance-variables"></a>Proměnné instance

Pole deklarované bez `static` modifikátoru se nazývá ***instance proměnné***.

#### <a name="instance-variables-in-classes"></a>Proměnné instancí ve třídách

Instance proměnné třídy se vyskytuje, když je vytvořena nová instance této třídy, a přestane existovat, pokud neexistují žádné odkazy na tuto instanci a destruktor instance (pokud existuje).

Počáteční hodnota proměnné instance třídy je výchozí hodnota ([výchozí hodnoty](variables.md#default-values)) typu proměnné.

Pro účely jednoznačné kontroly přiřazení je proměnná instance třídy považována za původně přiřazenou.

#### <a name="instance-variables-in-structs"></a>Proměnné instancí ve strukturách

Proměnná instance struktury má přesně stejnou životnost jako proměnná struktury, ke které patří. Jinými slovy, pokud je proměnná typu struktury v výskytu nebo přestane existovat, tak je také nutné proměnné instance struktury.

Počáteční stav přiřazení proměnné instance struktury je stejný jako objekt obsahující proměnnou struktury. Jinými slovy, pokud je proměnná struktury považována za původně přiřazenou, tedy příliš jsou její proměnné instance a pokud je proměnná struktury považována za původně nepřiřazenou, její proměnné instance jsou stejně nepřiřazeny.

### <a name="array-elements"></a>Prvky pole

Prvky pole přicházejí v případě, že je vytvořena instance pole, a přestanou existovat, pokud žádné odkazy na tuto instanci pole neexistují.

Počáteční hodnota každého prvku pole je výchozí hodnota ([výchozí hodnoty](variables.md#default-values)) typu prvků pole.

Pro účely jednoznačné kontroly přiřazení je prvek pole považován za původně přiřazený.

### <a name="value-parameters"></a>Parametry hodnoty

Parametr deklarovaný bez `ref` modifikátoru nebo `out` je ***parametrem hodnoty***.

Parametr hodnoty se vyskytuje v případě volání členu funkce (metoda, konstruktor instance, přistupující objekt nebo operátor) nebo anonymní funkce, do které parametr patří, a je inicializován s hodnotou argumentu uvedeného v vyvolání. Parametr hodnoty obvykle po vrácení členu funkce nebo anonymní funkce přestane existovat. Pokud je však parametr value zachycen anonymní funkcí ([anonymními výrazy funkce](expressions.md#anonymous-function-expressions)), bude jeho životní cyklus prodloužen alespoň do doby, než je vytvořen delegát nebo strom výrazů vytvořený z této anonymní funkce, který je vhodný pro uvolňování paměti.

Pro účely jednoznačné kontroly přiřazení je parametr hodnoty považován za původně přiřazený.

### <a name="reference-parameters"></a>Parametry odkazu

Parametr deklarovaný s `ref` modifikátorem je ***parametr odkazu***.

Parametr reference nevytváří nové umístění úložiště. Místo toho parametr reference představuje stejné umístění úložiště jako proměnná zadaná jako argument členu funkce nebo anonymní volání funkce. Proto hodnota referenčního parametru je vždy stejná jako podkladová proměnná.

Následující pravidla přiřazení platí pro referenční parametry. Všimněte si různých pravidel pro výstupní parametry popsaných v tématu [výstupní parametry](variables.md#output-parameters).

*  Proměnná musí být jednoznačně přiřazena ([jednoznačné přiřazení](variables.md#definite-assignment)) předtím, než může být předána jako parametr reference v členu funkce nebo volání delegáta.
*  V rámci členu funkce nebo anonymní funkce je referenční parametr považován za původně přiřazený.

V rámci metody instance nebo přístupového objektu instance typu `this` struktury se klíčové slovo chová přesně jako referenční parametr typu struktury ([Tento přístup](expressions.md#this-access)).

### <a name="output-parameters"></a>Výstupní parametry

Parametr deklarovaný s `out` modifikátorem je ***výstupní parametr***.

Výstupní parametr nevytváří nové umístění úložiště. Namísto toho parametr Output představuje stejné umístění úložiště jako proměnná zadaná jako argument členu funkce nebo volání delegáta. Proto hodnota výstupní parametr je vždy stejná jako podkladová proměnná.

Následující pravidla přiřazení se vztahují na výstupní parametry. Poznamenejte si různá pravidla pro referenční parametry popsané v [parametrech reference](variables.md#reference-parameters).

*  Proměnná nemusí být jednoznačně přiřazena, než může být předána jako výstupní parametr v členu funkce nebo volání delegáta.
*  Po normálním dokončení členu funkce nebo delegáta delegáta se v této cestě provádění považuje každá proměnná, která byla předána jako výstupní parametr.
*  V rámci členu funkce nebo anonymní funkce je výstupní parametr považován za původně nepřiřazený.
*  Každému výstupnímu parametru členu funkce nebo anonymní funkce se musí jednoznačně přiřadit ([jednoznačné přiřazení](variables.md#definite-assignment)) před tím, než se člen funkce nebo anonymní funkce vrátí běžným způsobem.

V rámci konstruktoru instance typu `this` struktury se klíčové slovo chová přesně jako výstupní parametr typu struktury ([Tento přístup](expressions.md#this-access)).

### <a name="local-variables"></a>Místní proměnné

***Lokální proměnná*** je deklarována pomocí *local_variable_declaration*, ke kterému může dojít v *bloku*, *for_statement*, *switch_statement* nebo *using_statement*; nebo *foreach_statement* nebo *specific_catch_clause* pro *try_statement*.

Doba života místní proměnné je část provádění programu, během které je zaručeno, že je úložiště rezervováno pro něj. Tato doba života sahá aspoň od vstupu do *bloku*, *for_statement*, *switch_statement*, *using_statement*, *foreach_statement*nebo *specific_catch_clause* , ke kterému je přidružená, až do provádění tohoto *bloku*, *for_statement*, *switch_statement*, *using_statement*, *foreach_statement*nebo *specific_catch_clause* končí jakýmkoli způsobem. (Vstup do uzavřeného *bloku* nebo volání metody pozastaví, ale ne na konec, spuštění aktuálního *bloku*, *for_statement*, *switch_statement*, *using_statement*, *foreach_statement*nebo *specific_ catch_clause*.) Pokud je lokální proměnná zachycena anonymní funkcí ([zaznamenanými vnějšími proměnnými](expressions.md#captured-outer-variables)), její životnost přesahuje alespoň do doby, než se vytvoří delegát nebo strom výrazů vytvořeného z anonymní funkce, spolu s dalšími objekty, které jsou na odkaz zachycená proměnná, která je vhodná pro uvolňování paměti.

Pokud se nadřazený *blok*, *for_statement*, *switch_statement*, *using_statement*, *foreach_statement*nebo *specific_catch_clause* zadá rekurzivně, vytvoří se nová instance místní proměnné. čas a jeho *local_variable_initializer*, pokud jsou nějaké, se vyhodnotí pokaždé.

Lokální proměnná zavedená *local_variable_declaration* se neinicializuje automaticky, takže nemá žádnou výchozí hodnotu. Pro účely jednoznačné kontroly přiřazení je místní proměnná zavedená *local_variable_declaration* považována za původně nepřiřazenou. *Local_variable_declaration* může obsahovat *local_variable_initializer*. v takovém případě se proměnná považuje za jednoznačně přiřazenou až po inicializaci výrazu ([příkazy deklarace](variables.md#declaration-statements)).

V rámci rozsahu místní proměnné zavedené *local_variable_declaration*se jedná o chybu při kompilaci, která odkazuje na tuto místní proměnnou v textové pozici, která předchází své *local_variable_declarator*. Pokud je deklarace lokální proměnné implicitní ([místní deklarace proměnných](statements.md#local-variable-declarations)), je také chyba pro odkazování na proměnnou v rámci jejího *local_variable_declarator*.

Místní proměnná zavedená *foreach_statement* nebo *specific_catch_clause* se považuje za jednoznačně přiřazenou v celém oboru.

Skutečná doba života místní proměnné je závislá na implementaci. Kompilátor může například staticky určit, že místní proměnná v bloku je použita pouze pro malou část tohoto bloku. Pomocí této analýzy může kompilátor vygenerovat kód, který má za následek, že úložiště proměnných má kratší dobu života, než je blok, který ho obsahuje.

Úložiště, na které odkazuje místní referenční proměnná, se uvolní nezávisle na době života této místní referenční proměnné ([Automatická správa paměti](basic-concepts.md#automatic-memory-management)).

## <a name="default-values"></a>Výchozí hodnoty

Následující kategorie proměnných jsou automaticky inicializovány na výchozí hodnoty:

*  Statické proměnné.
*  Proměnné instance instancí třídy.
*  Prvky pole.

Výchozí hodnota proměnné závisí na typu proměnné a je určena následujícím způsobem:

*  Pro proměnnou *value_type*je výchozí hodnota shodná s hodnotou vypočítanou výchozím konstruktorem *value_type*([výchozí konstruktory](types.md#default-constructors)).
*  Pro proměnnou *reference_type*je `null`výchozí hodnota.

Inicializace na výchozí hodnoty je obvykle prováděna tím, že správce paměti nebo systém uvolňování paměti inicializuje paměť na všechny bity – nula, než je přiděleno k použití. Z tohoto důvodu je vhodné použít všechny bity-Zero k vyjádření nulového odkazu.

## <a name="definite-assignment"></a>Jednoznačné přiřazení

V daném umístění v kódu spustitelného členu funkce je proměnná označena jako ***jednoznačně přiřazená*** , pokud kompilátor může prokázat konkrétní statickou analýzu toku ([přesné pravidlo pro určení jednoznačného přiřazení](variables.md#precise-rules-for-determining-definite-assignment)), že proměnná byl automaticky inicializován nebo byl cílem alespoň jednoho přiřazení. Pravidla jednoznačného přiřazení jsou neformálně uvedená:

*  Původně přiřazená proměnná ([původně přiřazené proměnné](variables.md#initially-assigned-variables)) se vždycky považuje za jednoznačně přiřazenou.
*  Původně Nepřiřazená proměnná ([zpočátku nepřiřazené proměnné](variables.md#initially-unassigned-variables)) se považuje za jednoznačně přiřazenou v daném umístění, pokud všechny možné cesty spuštění, které vede k tomuto umístění, obsahují alespoň jednu z následujících možností:
    * Jednoduché přiřazení ([jednoduché přiřazení](expressions.md#simple-assignment)), ve kterém je proměnná levý operand.
    * Výraz vyvolání ([výrazy vyvolání](expressions.md#invocation-expressions)) nebo výraz pro vytvoření objektu (výrazy pro[vytvoření objektu](expressions.md#object-creation-expressions)), který tuto proměnnou předává jako výstupní parametr.
    * Místní proměnná, deklaraci lokální proměnné ([deklarace místní proměnné](statements.md#local-variable-declarations)), která obsahuje inicializátor proměnné.

Formální specifikace uvedená výše neformálních pravidel je popsána v [úvodně přiřazených proměnných](variables.md#initially-assigned-variables), [počátečních nepřiřazených proměnných](variables.md#initially-unassigned-variables)a [přesných pravidlech pro určení jednoznačného přiřazení](variables.md#precise-rules-for-determining-definite-assignment).

Přesné stavy přiřazení proměnných instance proměnné *struct_type* jsou sledovány jednotlivě a společně. Kromě výše uvedených pravidel platí následující pravidla pro proměnné *struct_type* a jejich proměnné instance:

*  Proměnná instance se považuje za jednoznačně přiřazenou, pokud je její obsahující proměnná *struct_type* považována za jednoznačně přiřazenou.
*  Proměnná *struct_type* se považuje za jednoznačně přiřazenou, pokud je každá z jejích proměnných instance považována za jednoznačně přiřazenou.

Jednoznačné přiřazení je požadavek v následujících kontextech:

*  Proměnná musí být jednoznačně přiřazena na každém místě, kde je jeho hodnota získána. Tím se zajistí, že nedefinované hodnoty nebudou nikdy provedeny. Výskyt proměnné ve výrazu je považován za získání hodnoty proměnné, s výjimkou případů, kdy
    * proměnná je levý operand jednoduchého přiřazení,
    * proměnná se předává jako výstupní parametr nebo
    * proměnná je *struct_type* proměnná a nastane jako levý operand přístupu ke členu.
*  Proměnná musí být jednoznačně přiřazena na každém místě, kde je předána jako parametr reference. Tím je zajištěno, že vyvolaná člen funkce může uvažovat o počátečním přiřazeném parametru odkazu.
*  Všechny výstupní parametry členu funkce musí být jednoznačně přiřazeny v každém umístění, kde vrací člen funkce (prostřednictvím `return` příkazu nebo prostřednictvím provádění, které se blíží konci těla člena funkce). Tím se zajistí, že členové funkce nebudou vracet nedefinované hodnoty v parametrech Output, takže Kompilátor považuje volání člena funkce, které přebírá proměnnou jako výstupní parametr ekvivalentní přiřazení k proměnné.
*  Proměnná konstruktoru instance struct_type musí být jednoznačně přiřazena v každém umístění, kde se konstruktor instance vrátí. `this`

### <a name="initially-assigned-variables"></a>Původně přiřazené proměnné

Následující kategorie proměnných jsou klasifikovány jako původně přiřazené:

*  Statické proměnné.
*  Proměnné instance instancí třídy.
*  Proměnné instance původně přiřazených proměnných struktury.
*  Prvky pole.
*  Parametry hodnoty.
*  Parametry odkazu
*  Proměnné deklarované v `catch` klauzuli `foreach` nebo příkazu.

### <a name="initially-unassigned-variables"></a>Původně nepřiřazené proměnné

Následující kategorie proměnných jsou klasifikovány jako původně nepřiřazené:

*  Proměnné instance počátečních nepřiřazených proměnných struktury.
*  Výstupní parametry, včetně `this` proměnné konstruktory instance struktury.
*  Místní proměnné, s výjimkou těch, `catch` které jsou deklarovány v klauzuli `foreach` nebo příkazu.

### <a name="precise-rules-for-determining-definite-assignment"></a>Přesné pravidla pro určení jednoznačného přiřazení

Aby bylo možné určit, že je každá použitá proměnná jednoznačně přiřazená, kompilátor musí použít proces, který je ekvivalentní k tomu popsanému v této části.

Kompilátor zpracovává tělo každého členu funkce, který má jednu nebo více počátečních nepřiřazených proměnných. Pro každou původně nepřiřazenou proměnnou *v*kompilátor *určuje v každém* z následujících bodů člena funkce ***jednoznačný stav přiřazení*** :

*  Na začátku každého příkazu
*  Na koncovém bodu ([koncové body a dosažitelnost](statements.md#end-points-and-reachability)) každého příkazu
*  U každého oblouku, který přenáší řízení na jiný příkaz nebo na koncový bod příkazu
*  Na začátku každého výrazu
*  Na konci každého výrazu

Určitý stav přiřazení *v* aplikaci může být:

*  Jednoznačně přiřazené. To znamená, že u všech možných toků řízení k tomuto bodu *v* je přiřazena hodnota.
*  Není jednoznačně přiřazeno. Pro stav proměnné na konci výrazu typu `bool`může stav proměnné, která není jednoznačně přiřazená, být (ale nemusí nutně) patřit do jednoho z následujících podřízených stavů:
    * Po hodnotě true se přiřadí výraz s omezením. Tento stav označuje, že hodnota *v* je jednoznačně přiřazená, pokud se logický výraz vyhodnotí jako true, ale není nutně přiřazený, pokud se logický výraz vyhodnotí jako false.
    * Po omezení se přiřadí po nepravdivém výrazu. Tento stav označuje, že hodnota *v* je jednoznačně přiřazená, pokud se logický výraz vyhodnotí jako false, ale není nutně přiřazený, pokud se logický výraz vyhodnotí jako true.

Následující pravidla určují, jak se určuje stav proměnné *v* jednotlivých umístěních.

#### <a name="general-rules-for-statements"></a>Obecná pravidla pro příkazy

*  *v* není jednoznačně přiřazen na začátku těla členu funkce.
*  *v* je jednoznačně přiřazen na začátku libovolného nedosažitelného příkazu.
*  Určitý stav přiřazení *v* v na začátku jakéhokoli jiného příkazu je určen kontrolou omezeného stavu přiřazení *v* u všech přenosů toku řízení, které cílí na začátek tohoto příkazu. If (a pouze if) *v* se jednoznačně přiřazují u všech takových přenosů toku řízení, pak je *v* systému jednoznačně přiřazen na začátku prohlášení. Sada možných přenosů toků řízení je určena stejným způsobem jako při ověřování dostupnosti příkazů ([koncových bodů a dostupnosti](statements.md#end-points-and-reachability)).
*  Jednoznačný stav přiřazení *v v* koncovém bodě `checked`bloku,, `lock` `foreach` `do` `while` `unchecked` `if`,,,, `for`,,, `using` nebo`switch`příkaz je určen kontrolou omezeného stavu přiřazení *v* v u všech přenosů toku řízení, které cílí na koncový bod tohoto příkazu. Pokud je *v* jednoznačně přiřazeno pro všechny takové přenosy toku řízení, pak *v* je jednoznačně přiřazeno na koncový bod příkazu. Případech *v* není jednoznačně přiřazen na koncový bod příkazu. Sada možných přenosů toků řízení je určena stejným způsobem jako při ověřování dostupnosti příkazů ([koncových bodů a dostupnosti](statements.md#end-points-and-reachability)).

#### <a name="block-statements-checked-and-unchecked-statements"></a>Příkazy bloku, zkontrolované a nezaškrtnuté příkazy

Nekonečný stav přiřazení v *v ovládacím* prvku přenáší do prvního příkazu seznamu příkazů v bloku (nebo na koncový bod bloku, pokud je seznam příkazů prázdný) stejný jako u zvláštního příkazu přiřazení *v před blok* . , `checked` nebo`unchecked` příkaz.

#### <a name="expression-statements"></a>Příkazy výrazu

Pro příkaz výrazu *stmt* , který se skládá z výrazu *expr*:

*  *v* má stejný jednoznačný stav přiřazení na začátku *výrazu* jako na začátku *stmt*.
*  Pokud je *v* případě jednoznačně přiřazeno na konci *výrazu expr*, je jednoznačně přiřazen na koncový bod *stmt*; případech neomezeně se nepřiřazuje na koncový bod *stmt*.

#### <a name="declaration-statements"></a>Příkazy deklarace

*  Pokud *stmt* je příkaz deklarace bez inicializátorů, pak *v* má stejný jednoznačný stav přiřazení na koncovém bodu *stmt* jako na začátku *stmt*.
*  Pokud je *stmt* příkazem deklarace s inicializátory, pak je stanovený stav přiřazení *v v v* , jako by *stmt* byl seznam příkazů s jedním příkazem přiřazení pro každou deklaraci s inicializátorem (v pořadí deklarace).

#### <a name="if-statements"></a>Příkazy if

Pro příkaz stmt ve tvaru: `if`
```csharp
if ( expr ) then_stmt else else_stmt
```

*  *v* má stejný jednoznačný stav přiřazení na začátku *výrazu* jako na začátku *stmt*.
*  Pokud je *v* systému jednoznačně přiřazen na konci *výrazu expr*, pak je jednoznačně přiřazen k přenosu toku řízení do *then_stmt* a buď *else_stmt* , nebo do koncového bodu *stmt* , pokud neexistuje klauzule else.
*  Pokud *v* má na konci výrazu výraz "s omezením" jednoznačně přiřazený po hodnotě *true, pak*je jednoznačně přiřazený k přenosu toku řízení do *then_stmt*a není jednoznačně přiřazený k přenosu toku řízení *else_. stmt* nebo na koncový bod *stmt* , pokud neexistuje klauzule else.
*  Pokud *v* má stav "po nepravdivém výrazu" po nepravdivém výrazu "na *konci výrazu", pak*je jednoznačně přiřazen k přenosu toku řízení do *else_stmt*a není jednoznačně přiřazen k přenosu toku řízení do *then_stmt* . V případě, že je v konečném bodě *stmt* k dispozici, je jednoznačně přiřazená, pokud je k dispozici pouze v případě, že je na koncovém bodu *then_stmt*.
*  V opačném případě je *v* tomto případě považována za neomezenou hodnotu pro přenos toku řízení do *then_stmt* nebo *else_stmt*, nebo do koncového bodu *stmt* , pokud neexistuje klauzule else.

#### <a name="switch-statements"></a>Příkazy Switch

V příkazu stmt *s výrazem řízení výrazu:* `switch`

*  Určitý stav přiřazení *v v* na začátku *výrazu* je stejný jako stav *v* na začátku *stmt*.
*  Určitý stav přiřazení *v* v je přenos toku řízení do seznamu příkazů bloku, který je k dispozici, je stejný jako jednoznačný stav přiřazení *v* v na konci *výrazu expr*.

#### <a name="while-statements"></a>Příkazy while

Pro příkaz stmt ve formátu: `while`
```csharp
while ( expr ) while_body
```

*  *v* má stejný jednoznačný stav přiřazení na začátku *výrazu* jako na začátku *stmt*.
*  Pokud je *v* systému jednoznačně přiřazen na konci *výrazu expr*, pak je jednoznačně přiřazen k přenosu toku řízení do *while_body* a koncovému bodu *stmt*.
*  Pokud *v* má na konci výrazu výraz "s omezením" jednoznačně přiřazený po hodnotě *true, pak*je jednoznačně přiřazený k přenosu toku řízení do *while_body*, ale ne jednoznačně přiřazený na koncový bod *stmt*.
*  Pokud *v* má stav "po nepravdivém výrazu" po nepravdivém výrazu *", pak*je jednoznačně přiřazen k přenosu toku řízení na koncový bod *stmt*, ale ne jednoznačně přiřazený k přenosu toku *řízení do _body*.

#### <a name="do-statements"></a>Do – příkazy

Pro příkaz stmt ve formátu: `do`
```csharp
do do_body while ( expr ) ;
```

*  *v* má stejný určitý stav přiřazení pro přenos toku řízení od začátku *stmt* do *do_body* jako na začátku *stmt*.
*  *v* má stejný jednoznačný stav přiřazení na začátku *výrazu* , jako na koncovém bodu *do_body*.
*  Pokud je *v* systému jednoznačně přiřazen na konci *výrazu expr*, pak je jednoznačně přiřazen k přenosu toku řízení do koncového bodu *stmt*.
*  Pokud *v* má stav "po nepravdivém výrazu" po nepravdivém výrazu "na *konci výrazu", pak*je jednoznačně přiřazen k přenosu toku řízení do koncového bodu *stmt*.

#### <a name="for-statements"></a>Příkazy for

Kontrola jednoznačného přiřazení pro `for` příkaz formuláře:
```csharp
for ( for_initializer ; for_condition ; for_iterator ) embedded_statement
```
je proveden jako při zápisu příkazu:
```csharp
{
    for_initializer ;
    while ( for_condition ) {
        embedded_statement ;
        for_iterator ;
    }
}
```

Pokud se *for_condition* z `for` příkazu vynechá, vyhodnocování jednoznačného přiřazení pokračuje, jako kdyby se `true` for_condition nahradilo ve výše uvedeném rozšíření.

#### <a name="break-continue-and-goto-statements"></a>Příkazy Break, Continue a goto

Určitý stav přiřazení *v* v na přenosu toku řízení `break`, který je způsoben příkazem, nebo `goto` , `continue`je stejný jako jednoznačný stav přiřazení *v* na začátku příkazu.

#### <a name="throw-statements"></a>Příkazy throw

Pro příkaz *stmt* ve formuláři
```csharp
throw expr ;
```

Určitý stav přiřazení *v v* na začátku *výrazu* je stejný jako jednoznačný stav přiřazení *v* na začátku *stmt*.

#### <a name="return-statements"></a>Příkazy Return

Pro příkaz *stmt* ve formuláři
```csharp
return expr ;
```

*  Určitý stav přiřazení *v v* na začátku *výrazu* je stejný jako jednoznačný stav přiřazení *v* na začátku *stmt*.
*  Pokud *v* je výstupní parametr, musí být jednoznačně přiřazen buď:
    * Po *výrazu*
    * nebo na `finally` konci bloku `try` - nebo`try` ,který`return` uzavře příkaz. - `finally` `catch` - `finally`

Pro příkaz stmt ve formátu:
```csharp
return ;
```

*  Pokud *v* je výstupní parametr, musí být jednoznačně přiřazen buď:
    * před *stmt*
    * nebo na `finally` konci bloku `try` - nebo`try` ,který`return` uzavře příkaz. - `finally` `catch` - `finally`

#### <a name="try-catch-statements"></a>Příkazy try-catch

Pro příkaz *stmt* ve formátu:
```csharp
try try_block
catch(...) catch_block_1
...
catch(...) catch_block_n
```

*  Určitý stav přiřazení *v v* na začátku *try_block* je stejný jako jednoznačný stav přiřazení *v* na začátku *stmt*.
*  Stanovený stav přiřazení *v* v na začátku *catch_block_i* (pro libovolný *i*) je stejný jako jednoznačný stav přiřazení *v* na začátku *stmt*.
*  Určitý stav přiřazení *v* v koncových bodech *stmt* je jednoznačně přiřazený, pokud (a pouze pokud) *v* se jednoznačně přiřazují v koncovém bodě *try_block* a každé *catch_block_i* (pro každý z 1 do *n.* ).

#### <a name="try-finally-statements"></a>Try-finally – příkazy

Pro příkaz stmt ve formátu: `try`
```csharp
try try_block finally finally_block
```

*  Určitý stav přiřazení *v v* na začátku *try_block* je stejný jako jednoznačný stav přiřazení *v* na začátku *stmt*.
*  Určitý stav přiřazení *v v* na začátku *finally_block* je stejný jako jednoznačný stav přiřazení *v* na začátku *stmt*.
*  Určitý stav přiřazení *v* v koncovém bodě *stmt* je jednoznačně přiřazený, pokud (a pouze pokud) alespoň jedna z následujících možností:
    * *v* je jednoznačně přiřazený na koncový bod *try_block* .
    * *v* je jednoznačně přiřazený na koncový bod *finally_block* .

Pokud je `goto` proveden přenos toku řízení (například příkaz), který začíná v *try_block*a končí mimo *try_block*, pak v je také v případě, že je v rámci tohoto přenosu toku řízení, *v* případě jednoznačně přiřazené k koncovému bodu *finally_block*. (Nejedná se o pouze v případě, že – pokud je *v* systému jednoznačně přiřazen jiný důvod pro přenos toku řízení, pak je stále považován za jednoznačně přiřazený.)

#### <a name="try-catch-finally-statements"></a>Try-catch-finally – příkazy

Analýza jednoznačného přiřazení pro `try` - `catch` příkazformuláře-: `finally`
```csharp
try try_block
catch(...) catch_block_1
...
catch(...) catch_block_n
finally *finally_block*
```
je proveden, jako kdyby byl příkaz `try` `finally` - příkazem ohraničujícím `try` - `catch` příkaz:
```csharp
try {
    try try_block
    catch(...) catch_block_1
    ...
    catch(...) catch_block_n
}
finally finally_block
```

Následující příklad ukazuje, jak různé bloky `try` příkazu ([příkaz try](statements.md#the-try-statement)) ovlivňují jednoznačné přiřazení.
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

Pro příkaz stmt ve formátu: `foreach`
```csharp
foreach ( type identifier in expr ) embedded_statement
```

*  Určitý stav přiřazení *v v* na začátku *výrazu* je stejný jako stav *v* na začátku *stmt*.
*  Určitý stav přiřazení *v* v v případě přenosů toku řízení do *embedded_statement* nebo na koncový bod *stmt* je stejný jako stav *v* elementu na konci *výrazu expr*.

#### <a name="using-statements"></a>Příkazy using

Pro příkaz stmt ve formátu: `using`
```csharp
using ( resource_acquisition ) embedded_statement
```

*  Určitý stav přiřazení *v v* na začátku *resource_acquisition* je stejný jako stav *v* v na začátku *stmt*.
*  Určitý stav přiřazení *v* v v případě přenosu toku řízení do *embedded_statement* je stejný jako stav *v* v na konci *resource_acquisition*.

#### <a name="lock-statements"></a>Příkazy LOCK

Pro příkaz stmt ve formátu: `lock`
```csharp
lock ( expr ) embedded_statement
```

*  Určitý stav přiřazení *v v* na začátku *výrazu* je stejný jako stav *v* na začátku *stmt*.
*  Určitý stav přiřazení *v* v v případě přenosu toku řízení do *embedded_statement* je stejný jako stav *v* elementu na konci *výrazu expr*.

#### <a name="yield-statements"></a>Příkazy yield

Pro příkaz stmt ve formátu: `yield return`
```csharp
yield return expr ;
```

*  Určitý stav přiřazení *v v* na začátku *výrazu* je stejný jako stav *v* na začátku *stmt*.
*  Jednoznačný stav přiřazení *v v* na konci *stmt* je stejný jako stav *v* elementu na konci *výrazu expr*.
*  `yield break` Příkaz nemá žádný vliv na určitý stav přiřazení.

#### <a name="general-rules-for-simple-expressions"></a>Obecná pravidla pro jednoduché výrazy

Následující pravidlo se vztahuje na tyto druhy výrazů: literály ([literály](expressions.md#literals)), jednoduché názvy ([jednoduché názvy](expressions.md#simple-names)), výrazy přístupu členů ([přístup členů](expressions.md#member-access)), neindexovaných základních výrazů přístupu ([základní přístup](expressions.md#base-access)), `typeof`výrazy ([operátor typeof](expressions.md#the-typeof-operator)), výchozí hodnoty výrazy ([výrazy výchozích hodnot](expressions.md#default-value-expressions)) a `nameof` výrazy ([výrazy nameof](expressions.md#nameof-expressions)).

*  Absolutní stav přiřazení *v* rámci na konci takového výrazu je stejný jako jednoznačný stav přiřazení *v* na začátku výrazu.

#### <a name="general-rules-for-expressions-with-embedded-expressions"></a>Obecná pravidla pro výrazy s vloženými výrazy

Následující pravidla se vztahují na tyto typy výrazů: výrazy v závorkách ([výrazy v závorkách](expressions.md#parenthesized-expressions)), výrazy přístupu k prvkům ([přístup k prvkům](expressions.md#element-access)), základní výrazy přístupu s indexováním ([základní přístup](expressions.md#base-access)), zvýšení a snížení výrazů ([operátory přírůstku a snížení přípony](expressions.md#postfix-increment-and-decrement-operators), [operátory přírůstku a snížení předpony](expressions.md#prefix-increment-and-decrement-operators)), výrazy přetypování `+`( `-`[výrazy přetypování](expressions.md#cast-expressions)), Unární,, `~`, `*`výrazy, binární `+` `-` ,`*`, ,,`%`,,,, ,`>`,, `>=` `/` `<<` `>>` `<` `<=` `==`, ,`is`,, ,`&` [](expressions.md#shift-operators)[](expressions.md#arithmetic-operators), výrazy(aritmetickéoperátory,operátory`^` posunutí, relační testování a testování typu `|` `!=` `as` [ operátory](expressions.md#relational-and-type-testing-operators), [logické operátory](expressions.md#logical-operators)), výrazy složeného přiřazení ( `checked` [složené přiřazení](expressions.md#compound-assignment)) a `unchecked` výrazy ([kontrolované a nezaškrtnuté operátory](expressions.md#the-checked-and-unchecked-operators)), plus pole a delegát výrazy vytváření ([operátor New](expressions.md#the-new-operator)).

Každý z těchto výrazů má jeden nebo více dílčích výrazů, které jsou bezpodmínečně vyhodnoceny v pevně uvedeném pořadí. Například binární `%` operátor vyhodnocuje levou stranu operátora a pak pravou stranu. Operace indexování vyhodnotí indexovaný výraz a pak vyhodnotí všechny indexové výrazy v pořadí zleva doprava. Výraz *expr*, který má dílčí výrazy *E1, E2,..., EN*, vyhodnocený v tomto pořadí:

*  Určitý stav přiřazení *v v* na začátku *E1* je stejný jako určitý stav přiřazení na začátku *výrazu expr*.
*  Stanovený stav přiřazení *v v* na začátku *EI* (*i* větší než jeden) je stejný jako u jednoznačného stavu přiřazení na konci předchozího dílčího výrazu.
*  Určitý stav přiřazení *v v* na konci *výrazu* je stejný jako jednoznačný stav přiřazení na konci *EN*

#### <a name="invocation-expressions-and-object-creation-expressions"></a>Výrazy vyvolání a výrazy pro vytváření objektů

*Výraz výrazu vyvolání* formuláře:
```csharp
primary_expression ( arg1 , arg2 , ... , argN )
```
nebo výraz pro vytvoření objektu ve formátu:
```csharp
new type ( arg1 , arg2 , ... , argN )
```

*  Pro výraz vyvolání je jednoznačný stav přiřazení *v v* před *primary_expression* shodný se stavem *v* před *výrazem*.
*  U výrazu vyvolání je jednoznačný stav přiřazení *v v* před *arg1* stejný jako stav *v v* po *primary_expression*.
*  V případě výrazu pro vytvoření objektu je jednoznačný stav přiřazení *v v* před *arg1* stejný jako stav *v* před *výrazem*.
*  U každého argumentu *Argi*je stanovený jednoznačný stav přiřazení *v v* po *Argi* pravidla pro normální výrazy, přičemž se ignorují `ref` všechny `out` nebo modifikátory.
*  U každého argumentu *Argi* pro libovolný *i* větší než jeden je určený stav přiřazení *v* před *Argi* stejný jako stav *v* za předchozí *arg*.
*  Pokud je proměnná *v* předána jako `out` argument (tj. argument formuláře `out v`) v některém z argumentů, pak je stav *v v* za *výraz* jednoznačně přiřazen. Případech stav *v v* po *výrazu expr* je stejný jako stav *v* za po *argn*.
*  Pro Inicializátory pole ([výrazy vytváření pole](expressions.md#array-creation-expressions)), Inicializátory objektů ([Inicializátory objektů](expressions.md#object-initializers)), inicializátory kolekce ([inicializátory kolekce](expressions.md#collection-initializers)) a Inicializátory anonymních objektů ([vytváření anonymních objektů výrazy](expressions.md#anonymous-object-creation-expressions)), jednoznačný stav přiřazení je určen rozšířením, které jsou definovány ve smyslu.

#### <a name="simple-assignment-expressions"></a>Výrazy jednoduchého přiřazení

Výraz *expr* formuláře `w = expr_rhs`:

*  Určitý stav přiřazení *v* rámci před *expr_rhs* je stejný jako jednoznačný stav přiřazení *v* před *výrazem*.
*  Jednoznačný stav přiřazení *v v* po *výrazu* určuje:
   * Je-li *w* stejná proměnná jako *v*, pak je jednoznačný stav přiřazení *v v* After *expr* jednoznačně přiřazen.
   * V opačném případě, pokud je přiřazení provedeno v rámci konstruktoru instance typu struktury, pokud je *w* přístup k vlastnosti určení automaticky implementované vlastnosti *P* na instanci, kterou vytváříte, a *v* je skryté pole zálohování *P*, pak bude jednoznačný stav přiřazení *v v* After *expr* jednoznačně přiřazen.
   * V opačném případě bude jednoznačný stav přiřazení *v v* After *expr* stejný jako jednoznačný stav přiřazení *v* po *expr_rhs*.

#### <a name="-conditional-and-expressions"></a>& & (podmíněné a) výrazy

Výraz *expr* formuláře `expr_first && expr_second`:

*  Určitý stav přiřazení *v* rámci před *expr_first* je stejný jako jednoznačný stav přiřazení *v* před *výrazem*.
*  Skutečný stav přiřazení *v v* před *expr_second* je jednoznačně přiřazený, pokud je stav *v v* po *expr_first* buď jednoznačně přiřazen, nebo "jednoznačně přiřazeno po výrazu true". V opačném případě se nepřiřazují jako neomezeně.
*  Jednoznačný stav přiřazení *v v* po *výrazu* určuje:
    * Pokud *expr_first* `false`je konstantní výraz s hodnotou, pak bude mít jednoznačný stav přiřazení *v v* After *expr* stejný jako pro určitý stav přiřazení *v* za za *expr_first*.
    * V opačném případě, pokud je stav *v v* po *expr_first* přiřazení jednoznačně přiřazen, je stav *v v* po jednoznačně přiřazeném *výrazu* .
    * V opačném případě platí, že pokud je stav *v v* po *expr_secondně* přiřazený, a stav *v* po po *expr_first* je "jednoznačně přiřazený po nepravdivém výrazu", pak je stav *v v* After *expr* jednoznačně přiřazení.
    * V opačném případě platí, že *Pokud je stav* *v v* po *expr_second* jednoznačně přiřazený nebo "jednoznačně přiřazený po hodnotě true", pak je stav *v* po výrazu "jednoznačně přiřazený po výrazu true".
    * V opačném případě, pokud je stav *v* po po *expr_first* "jednoznačně přiřazený po nepravdivém výrazu", a stav *v v* After *expr_second* je "jednoznačně přiřazený po nepravdivém výrazu", pak stav *v* za  *výraz* je "jednoznačně přiřazený po nepravdivém výrazu".
    * V opačném případě stav *v* případě, kdy *výraz* není jednoznačně přiřazen.

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
proměnná `i` se považuje za jednoznačně přiřazenou v jednom z vložených příkazů `if` příkazu, ale ne v druhé. V příkazu v metodě `F`je proměnná `i` jednoznačně přiřazena v prvním vloženém příkazu, protože provádění výrazu `(i = y)` vždy předchází provedení tohoto vloženého příkazu. `if` Naopak proměnná `i` není jednoznačně přiřazena v druhém vloženém příkazu, protože `x >= 0` by mohla být testována na false, výsledkem je, že proměnná `i` je Nepřiřazená.

#### <a name="-conditional-or-expressions"></a>|| (podmíněné nebo) výrazy

Výraz *expr* formuláře `expr_first || expr_second`:

*  Určitý stav přiřazení *v* rámci před *expr_first* je stejný jako jednoznačný stav přiřazení *v* před *výrazem*.
*  Určitý stav přiřazení *v v* před *expr_second* je jednoznačně přiřazený, pokud je stav *v v* po *expr_first* buď jednoznačně přiřazen, nebo "jednoznačně přiřazený po false výrazu". V opačném případě se nepřiřazují jako neomezeně.
*  Jednoznačné příkazy přiřazení *v v* After *expr* určují:
    * Pokud *expr_first* `true`je konstantní výraz s hodnotou, pak bude mít jednoznačný stav přiřazení *v v* After *expr* stejný jako pro určitý stav přiřazení *v* za za *expr_first*.
    * V opačném případě, pokud je stav *v v* po *expr_first* přiřazení jednoznačně přiřazen, je stav *v v* po jednoznačně přiřazeném *výrazu* .
    * V opačném případě platí, že pokud je stav *v v* po *expr_secondně* přiřazený, a stav *v* po po *expr_first* je "jednoznačně přiřazený po true Expression", pak je stav *v v* After *expr* jednoznačně přiřazení.
    * V opačném případě platí, že pokud je stav *v v* po *expr_second* jednoznačně přiřazený nebo "jednoznačně přiřazený po nepravdivém výrazu", pak je stav *v v* After *expr* "jednoznačně přiřazený po nepravdivém výrazu".
    * V opačném případě, pokud je stav *v* po po *expr_first* "jednoznačně přiřazený za true Expression", a stav *v v* After *expr_second* je "jednoznačně přiřazený za true Expression", pak stav *v v* After *expr* je "jednoznačně přiřazeno po výrazu true".
    * V opačném případě stav *v* případě, kdy *výraz* není jednoznačně přiřazen.

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
proměnná `i` se považuje za jednoznačně přiřazenou v jednom z vložených příkazů `if` příkazu, ale ne v druhé. V příkazu v metodě `G`je proměnná `i` jednoznačně přiřazena v druhém vloženém příkazu, protože provádění výrazu `(i = y)` vždy předchází provedení tohoto vloženého příkazu. `if` V opačném případě proměnná `i` není jednoznačně přiřazena v prvním vloženém příkazu, protože `x >= 0` by mohla být testována na hodnotu true, výsledkem `i` je, že proměnná je Nepřiřazená.

#### <a name="-logical-negation-expressions"></a>! (logické negace) – výrazy

Výraz *expr* formuláře `! expr_operand`:

*  Určitý stav přiřazení *v* rámci před *expr_operand* je stejný jako jednoznačný stav přiřazení *v* před *výrazem*.
*  Jednoznačný stav přiřazení *v v* po *výrazu* určuje:
    * Pokud je stav *v* po za * expr_operand * jednoznačně přiřazený, pak je stav *v v* po jednoznačně přiřazeném *výrazu* .
    * Pokud stav *v v* After * expr_operand * není jednoznačně přiřazen, pak stav *v v* po *výrazu* není jednoznačně přiřazen.
    * Pokud stav *v v* After * expr_operand * je "jednoznačně přiřazený po nepravdivém výrazu", pak je stav *v v* After *expr* "jednoznačně přiřazený po výrazu true".
    * Pokud je stav *v v* After * expr_operand * "po true Expression" "" jednoznačně přiřazen ", pak je stav v *po výrazu* " jednoznačně přiřazený po nepravdivém výrazu ".

#### <a name="-null-coalescing-expressions"></a>?? (slučovací výrazy s hodnotou null)

Výraz *expr* formuláře `expr_first ?? expr_second`:

*  Určitý stav přiřazení *v* rámci před *expr_first* je stejný jako jednoznačný stav přiřazení *v* před *výrazem*.
*  Určitý stav přiřazení *v* rámci před *expr_second* je stejný jako jednoznačný stav přiřazení *v* za za *expr_first*.
*  Jednoznačné příkazy přiřazení *v v* After *expr* určují:
    * Pokud *expr_first* je konstantní výraz ([konstantní výrazy](expressions.md#constant-expressions)) s hodnotou null, pak je stav *v v* After *expr* stejný jako stav *v v* za *expr_second*.
*  V opačném případě je stav *v v* After *expr* stejný jako jednoznačný stav přiřazení *v* po *expr_first*.

#### <a name="-conditional-expressions"></a>?: (podmíněné) výrazy

Výraz *expr* formuláře `expr_cond ? expr_true : expr_false`:

*  Určitý stav přiřazení *v* rámci před *expr_cond* je stejný jako stav *v* před *výrazem*.
*  Určitý stav přiřazení *v v* před *expr_true* se jednoznačně přiřadí, pokud a pouze v případě, že je k dispozici jeden z následujících:
    * *expr_cond* je konstantní výraz s hodnotou.`false`
    * stav *v v* po po *expr_cond* je jednoznačně přiřazený nebo "jednoznačně přiřazeno po výrazu true".
*  Určitý stav přiřazení *v v* před *expr_false* se jednoznačně přiřadí, pokud a pouze v případě, že je k dispozici jeden z následujících:
    * *expr_cond* je konstantní výraz s hodnotou.`true`
*  stav *v v* po *expr_cond* je jednoznačně přiřazený nebo "jednoznačně přiřazený po nepravdivém výrazu".
*  Jednoznačný stav přiřazení *v v* po *výrazu* určuje:
    * Pokud *expr_cond* je konstantní výraz ([konstantní výrazy](expressions.md#constant-expressions)) s hodnotou `true` , pak stav *v v* po *výrazu expr* je stejný jako stav *v* za *expr_true*.
    * V opačném případě, pokud *expr_cond* je konstantní výraz ([konstantní výrazy](expressions.md#constant-expressions)) `false` s hodnotou, pak stav *v v* After *expr* je stejný jako stav *v* za za *expr_false*.
    * V opačném případě, pokud je stav *v v* po *expr_trueně* přiřazený a stav *v v* po, kdy *expr_false* je jednoznačně přiřazený, pak je stav *v v* After *expr* jednoznačně přiřazený.
    * V opačném případě stav *v* případě, kdy *výraz* není jednoznačně přiřazen.

#### <a name="anonymous-functions"></a>Anonymní funkce

Výraz *lambda_expression* nebo *anonymous_method_expression* *expr* *s tělo (* buď *blok* nebo *výraz*):

*  Určitý stav přiřazení vnější proměnné *v* před *textovým tělem* je stejný jako stav *v* před *výrazem*. To znamená, že je z kontextu anonymní funkce zděděný pouze stav přiřazení vnějších proměnných.
*  Jednoznačný stav přiřazení vnější proměnné *v* po *výrazu expr* je stejný jako stav *v* před *výrazem*.

Příklad
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
vygeneruje chybu při kompilaci, protože `max` není jednoznačně přiřazen, kde je deklarace anonymní funkce deklarována. Příklad
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
také generuje chybu při kompilaci, protože přiřazení k `n` v anonymní funkci nemá žádný vliv na jednoznačný `n` stav přiřazení vně anonymní funkce.

## <a name="variable-references"></a>Odkazy na proměnné

*Variable_reference* je *výraz* , který je klasifikován jako proměnná. *Variable_reference* označuje umístění úložiště, ke kterému lze získat pøístup pro načtení aktuální hodnoty a uložení nové hodnoty.

```antlr
variable_reference
    : expression
    ;
```

V jazyce C C++a je *variable_reference* označován jako *l*-hodnota.

## <a name="atomicity-of-variable-references"></a>Nedělitelnost odkazů na proměnné

Čtení a zápisy následujících datových typů jsou atomické: `bool` `byte`, `char` `ushort` `short` `sbyte`,,,,, `uint`, `int`, `float`a odkazové typy. Kromě toho jsou operace čtení a zápisy typů výčtu s nadřízeným typem v předchozím seznamu také atomické. Čtení a zápisy jiných typů, včetně `long` `double`, `ulong`, a `decimal`i uživatelsky definovaných typů, nejsou zaručeny jako atomické. Kromě funkcí v knihovně, které jsou navrženy pro tento účel, neexistuje žádná záruka atomické úpravy pro čtení a zápis, například v případě zvýšení nebo snížení.

