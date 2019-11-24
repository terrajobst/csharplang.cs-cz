---
ms.openlocfilehash: ff31585520c9090ad92893a930327112743c8e77
ms.sourcegitcommit: 892af9016b3317a8fae12d195014dc38ba51cf16
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/01/2019
ms.locfileid: "71704010"
---
# <a name="basic-concepts"></a>Základní koncepty

## <a name="application-startup"></a>Spuštění aplikace

Sestavení, které má ***vstupní bod*** , se nazývá ***aplikace***. Při spuštění aplikace se vytvoří nová ***doména aplikace*** . V jednom počítači může existovat několik různých instancí aplikace současně a každá z nich má svou vlastní doménu aplikace.

Doména aplikace umožňuje izolaci aplikace působit jako kontejner pro stav aplikace. Doména aplikace funguje jako kontejner a hranice pro typy definované v aplikaci a knihovny tříd, které používá. Typy načtené do jedné domény aplikace se liší od stejného typu načteného do jiné domény aplikace a instance objektů nejsou přímo sdíleny mezi doménami aplikace. Každý aplikační doména má například vlastní kopii statických proměnných pro tyto typy a statický konstruktor pro typ je spuštěn nejvýše jednou pro doménu aplikace. Implementace jsou bezplatné pro poskytování zásad nebo mechanismů specifických pro implementaci pro vytváření a zničení aplikačních domén.

***Spuštění aplikace*** nastane, pokud spouštěcí prostředí volá určenou metodu, která je označována jako vstupní bod aplikace. Tato metoda vstupního bodu je vždy pojmenována `Main`a může mít jednu z následujících signatur:

```csharp
static void Main() {...}

static void Main(string[] args) {...}

static int Main() {...}

static int Main(string[] args) {...}
```

Jak je znázorněno, může vstupní bod volitelně vracet `int` hodnotu. Tato návratová hodnota se používá při ukončení aplikace ([ukončení aplikace](basic-concepts.md#application-termination)).

Vstupní bod může volitelně mít jeden formální parametr. Parametr může mít libovolný název, ale typ parametru musí být `string[]`. Pokud je k dispozici formální parametr, spouštěcí prostředí vytvoří a předává `string[]` argument obsahující argumenty příkazového řádku, které byly zadány při spuštění aplikace. Argument `string[]` nemá nikdy hodnotu null, ale v případě, že nebyly zadány žádné argumenty příkazového řádku, může mít nulovou délku.

Vzhledem C# k tomu, že podporuje přetížení metody, třída nebo struktura může obsahovat více definicí některé metody, pokud každý má jiný podpis. V rámci jednoho programu však žádná třída ani struktura nesmí obsahovat více než jednu metodu nazvanou `Main` jejíž definice je pro použití jako vstupní bod aplikace. Další přetížené verze `Main` jsou povoleny, avšak za předpokladu, že mají více než jeden parametr, nebo je jejich jediný parametr jiný než typ `string[]`.

Aplikace může být tvořena více třídami nebo strukturami. Je možné, aby více než jedna z těchto tříd nebo struktur obsahovala metodu nazvanou `Main` jejíž definice je určena k použití jako vstupní bod aplikace. V takových případech je nutné použít externí mechanismus (například možnost kompilátoru příkazového řádku) k výběru jedné z následujících metod `Main` jako vstupní bod.

V C#nástroji musí být každá metoda definována jako člen třídy nebo struktury. Obecně deklarovaná přístupnost ([deklarovaná přístupnost](basic-concepts.md#declared-accessibility)) metody je určena modifikátory přístupu ([modifikátory přístupu](classes.md#access-modifiers)) určenými v deklaraci a podobně deklarovaná přístupnost typu je určena modifikátory přístupu určenými v deklaraci. Aby bylo možné volat danou metodu daného typu, musí být typ i člen přístupný. Vstupní bod aplikace je však zvláštní případ. Konkrétně spouštěcí prostředí má přístup ke vstupnímu bodu aplikace bez ohledu na jeho deklaraci, a to bez ohledu na deklaraci přístupnosti jejich nadřazených typů deklarací.

Metoda vstupního bodu aplikace nemůže být v deklaraci obecné třídy.

Ve všech ostatních ohledech se metody vstupního bodu chovají stejně jako ty, které nejsou vstupními body.

## <a name="application-termination"></a>Ukončení aplikace

***Ukončení aplikace*** vrátí řízení spouštěcímu prostředí.

Pokud je návratový typ metody ***vstupního bodu*** aplikace `int`, vrácená hodnota slouží jako ***kód stavu ukončení***aplikace. Účelem tohoto kódu je umožnění komunikace s úspěchem nebo selháním spouštěcího prostředí.

Pokud je návratový typ metody vstupního bodu `void`, dosáhnete pravé složené závorky (`}`), která tuto metodu ukončí, nebo provedením příkazu `return`, který nemá žádný výraz, výsledkem je stavový kód ukončení `0`.

Před ukončením aplikace jsou destruktory pro všechny své objekty, které ještě nebyly shromažďovány do paměti, volány, pokud takové vyčištění nebylo potlačeno (voláním metody knihovny `GC.SuppressFinalize`, například).

## <a name="declarations"></a>Deklarace

Deklarace v C# programu definují prvky prvku programu. C#programy jsou uspořádány pomocí oborů názvů ([obory](namespaces.md)názvů), které mohou obsahovat deklarace typu a vnořené deklarace oboru názvů. Deklarace typu ([deklarace typu](namespaces.md#type-declarations)) se používají k definování tříd ([tříd](classes.md)), struktur ([struktur](structs.md)), rozhraní ([rozhraní](interfaces.md)), výčtů ([výčtů](enums.md)) a delegátů ([delegátů](delegates.md)). Typy členů povolených v deklaraci typu závisí na formuláři deklarace typu. Například deklarace třídy mohou obsahovat deklarace pro konstanty ([konstanty](classes.md#constants)), pole ([pole](classes.md#fields)), metody ([metody](classes.md#methods)), vlastnosti ([vlastnosti](classes.md#properties)), události ([události](classes.md#events)), indexery ([indexery](classes.md#indexers)), operátory ([operátory](classes.md#operators)), konstruktory instancí ([konstruktory instancí](classes.md#instance-constructors)), statické konstruktory ([statické konstruktory](classes.md#static-constructors)), destruktory ([destruktory](classes.md#destructors)) a vnořené typy ([vnořené typy](classes.md#nested-types)).

Deklarace definuje název v ***prostoru deklarace*** , ke kterému patří deklarace. S výjimkou přetížených členů ([signatury a přetížení](basic-concepts.md#signatures-and-overloading)) se jedná o chybu při kompilaci, která má dvě nebo více deklarací, které zavádějí členy se stejným názvem v prostoru deklarací. V prostoru deklarací není nikdy možné obsahovat různé druhy členů se stejným názvem. Například prostor deklarace nesmí nikdy obsahovat pole a metodu se stejným názvem.

Existuje několik různých typů prostorů deklarací, jak je popsáno v následujícím tématu.

*  V rámci všech zdrojových souborů programu *namespace_member_declaration*s bez uzavíracích *namespace_declaration* členy jednoho kombinovaného prostoru deklarací, který se nazývá ***globální místo deklarace***.
*  V rámci všech zdrojových souborů programu *namespace_member_declaration*s v *namespace_declaration*s, které mají stejný plně kvalifikovaný obor názvů, jsou členy jednoho kombinovaného prostoru deklarací.
*  Každá deklarace třídy, struktury nebo rozhraní vytvoří nové místo deklarace. Názvy se do tohoto prostoru deklarace přivádějí prostřednictvím *class_member_declaration*s, *struct_member_declaration*s, *interface_member_declaration*s nebo *type_parameter*s. Výjimkou jsou deklarace konstruktoru přetížené instance a deklarace statických konstruktorů, třídy nebo struktury nemůžou obsahovat deklaraci členu se stejným názvem jako třída nebo struktura. Třída, struktura nebo rozhraní povolují deklaraci přetížených metod a indexerů. Třída nebo struktura navíc umožňuje deklaraci přetížených konstruktorů instancí a operátorů. Například třída, struktura nebo rozhraní mohou obsahovat více deklarací metod se stejným názvem, za předpokladu, že se tyto deklarace metod liší v podpisu ([signatury a přetížení](basic-concepts.md#signatures-and-overloading)). Všimněte si, že základní třídy nepřispívají k prostoru deklarací třídy a základní rozhraní nepřispívají k prostoru deklarací rozhraní. Proto odvozená třída nebo rozhraní může deklarovat člen se stejným názvem jako zděděný člen. Tento člen je označován za účelem ***skrytí*** zděděného člena.
*  Každá deklarace delegáta vytvoří nové místo deklarace. Názvy jsou představeny do tohoto prostoru deklarací prostřednictvím formálních parametrů (*fixed_parameter*s a *parameter_array*) a *type_parameter*s.
*  Každá deklarace výčtu vytvoří nové místo deklarace. Názvy jsou představeny do tohoto prostoru deklarací prostřednictvím *enum_member_declarations*.
*  Každá deklarace metody, deklarace indexeru, deklarace operátoru, deklarace konstruktoru instance a anonymní funkce vytvoří nové místo deklarace s názvem ***místní proměnná proměnné Space***. Názvy jsou představeny do tohoto prostoru deklarací prostřednictvím formálních parametrů (*fixed_parameter*s a *parameter_array*) a *type_parameter*s. Tělo členu funkce nebo anonymní funkce, pokud existuje, je považováno za vnořené v rámci prostoru deklarace místní proměnné. Jedná se o chybu pro místní prostor deklarace proměnné a vnořené místo deklarace místní proměnné, které obsahuje prvky se stejným názvem. Proto v rámci vnořeného prostoru deklarací není možné deklarovat místní proměnnou nebo konstantu se stejným názvem jako místní proměnná nebo konstanta v ohraničujícím prostoru deklarace. Je možné, že dva prostory deklarace obsahují prvky se stejným názvem, pokud ani prostor deklarace neobsahuje.
*  Každý *blok* nebo *switch_block* , stejně jako příkaz *for*, *foreach* a *using* , vytvoří místní prostor deklarací proměnných pro lokální proměnné a místní konstanty. Názvy jsou představeny do tohoto prostoru deklarací prostřednictvím *local_variable_declaration*s a *local_constant_declaration*s. Všimněte si, že bloky, ke kterým dochází jako nebo v těle členu funkce nebo anonymní funkce, jsou vnořené v rámci prostoru deklarací místní proměnné deklarované pomocí těchto funkcí pro své parametry. Proto se jedná o chybu, například metoda s místní proměnnou a parametr se stejným názvem.
*  Každý *blok* nebo *switch_block* vytvoří pro popisky samostatný prostor deklarace. Názvy jsou představeny do tohoto prostoru deklarací prostřednictvím *labeled_statement*s a názvy jsou odkazovány prostřednictvím *goto_statement*s. ***Prostor deklarace popisku*** bloku obsahuje všechny vnořené bloky. Proto v rámci vnořeného bloku není možné deklarovat popisek se stejným názvem jako popisek v ohraničujícím bloku.

Textové pořadí, ve kterém jsou názvy deklarovány, je obecně bez významu. Konkrétně textové pořadí není významné pro deklaraci a použití oborů názvů, konstant, metod, vlastností, událostí, indexerů, operátorů, konstruktorů instancí, destruktorů, statických konstruktorů a typů. Pořadí deklarací je významné v následujících ohledech:

*  Pořadí deklarací pro deklarace polí a deklarace místních proměnných Určuje pořadí, ve kterém jsou provedeny jejich Inicializátory (pokud existují).
*  Lokální proměnné musí být definovány předtím, než se použijí ([rozsahy](basic-concepts.md#scopes)).
*  Pořadí deklarací pro deklarace členů výčtu ([členy výčtu](enums.md#enum-members)) je důležité, pokud jsou vynechány *constant_expression* hodnoty.

Prostor deklarací oboru názvů je "otevřený konec" a dvě deklarace oboru názvů se stejným plně kvalifikovaným názvem přispívají do stejného prostoru deklarace. Například
```csharp
namespace Megacorp.Data
{
    class Customer
    {
        ...
    }
}

namespace Megacorp.Data
{
    class Order
    {
        ...
    }
}
```

Dvě deklarace oboru názvů výše přispívají do stejného prostoru deklarace. v tomto případě deklaruje dvě třídy s plně kvalifikovanými názvy `Megacorp.Data.Customer` a `Megacorp.Data.Order`. Vzhledem k tomu, že dvě deklarace přispívají ke stejnému prostoru deklarace, by to vedlo k chybě při kompilaci, pokud každá z nich obsahuje deklaraci třídy se stejným názvem.

Jak je uvedeno výše, místo deklarace bloku obsahuje všechny vnořené bloky. Proto v následujícím příkladu metody `F` a `G` způsobí chybu při kompilaci, protože název `i` je deklarován ve vnějším bloku a nemůže být znovu deklarován ve vnitřním bloku. Nicméně metody `H` a `I` jsou platné, protože dvě `i`jsou deklarovány v samostatných nevnořených blocích.

```csharp
class A
{
    void F() {
        int i = 0;
        if (true) {
            int i = 1;            
        }
    }

    void G() {
        if (true) {
            int i = 0;
        }
        int i = 1;                
    }

    void H() {
        if (true) {
            int i = 0;
        }
        if (true) {
            int i = 1;
        }
    }

    void I() {
        for (int i = 0; i < 10; i++)
            H();
        for (int i = 0; i < 10; i++)
            H();
    }
}
```

## <a name="members"></a>Členové

Obory názvů a typy mají ***členy***. Členové entity jsou všeobecně k dispozici prostřednictvím kvalifikovaného názvu, který začíná odkazem na entitu a následuje tokenem "`.`" následovaným názvem člena.

Členy typu jsou deklarovány buď v deklaraci typu, nebo ***zděděny*** ze základní třídy typu. Když typ dědí ze základní třídy, všichni členové základní třídy, s výjimkou konstruktorů instancí, destruktorů a statických konstruktorů, se stanou členy odvozeného typu. Deklarovaná přístupnost člena základní třídy neurčuje, jestli je člen zděděný – dědičnost rozšiřuje na jakýkoliv člen, který není konstruktorem instance, statický konstruktor nebo destruktor. Zděděný člen však nemusí být přístupný v odvozeném typu, buď z důvodu deklarovaného přístupnosti ([deklarovaného přístupnosti](basic-concepts.md#declared-accessibility)), nebo vzhledem k tomu, že je skrytý deklarací v samotném typu ([skrytím prostřednictvím dědičnosti](basic-concepts.md#hiding-through-inheritance)).

### <a name="namespace-members"></a>Členové oboru názvů

Obory názvů a typy, které nemají obor názvů nadřazených, jsou členy ***globálního oboru názvů***. To odpovídá přímo názvům deklarovaným v globálním prostoru deklarací.

Obory názvů a typy deklarované v rámci oboru názvů jsou členy tohoto oboru názvů. To odpovídá přímo názvům deklarovaným v prostoru deklarací oboru názvů.

Obory názvů nemají žádná omezení přístupu. Není možné deklarovat privátní, chráněné nebo interní obory názvů a názvy oborů názvů jsou vždy veřejně přístupné.

### <a name="struct-members"></a>Členové struktury

Členy struktury jsou členy deklarované ve struktuře a členy zděděné z přímé základní třídy struktury `System.ValueType` a nepřímou základní třídou `object`.

Členové jednoduchého typu odpovídají přímo členům typu struktury s aliasem jednoduchý typ:

*  Členové `sbyte` jsou členy struktury `System.SByte`.
*  Členové `byte` jsou členy struktury `System.Byte`.
*  Členové `short` jsou členy struktury `System.Int16`.
*  Členové `ushort` jsou členy struktury `System.UInt16`.
*  Členové `int` jsou členy struktury `System.Int32`.
*  Členové `uint` jsou členy struktury `System.UInt32`.
*  Členové `long` jsou členy struktury `System.Int64`.
*  Členové `ulong` jsou členy struktury `System.UInt64`.
*  Členové `char` jsou členy struktury `System.Char`.
*  Členové `float` jsou členy struktury `System.Single`.
*  Členové `double` jsou členy struktury `System.Double`.
*  Členové `decimal` jsou členy struktury `System.Decimal`.
*  Členové `bool` jsou členy struktury `System.Boolean`.

### <a name="enumeration-members"></a>Členové výčtu

Členy výčtu jsou konstanty deklarované ve výčtu a členy zděděné z přímé základní třídy výčtu `System.Enum` a nepřímé základní třídy `System.ValueType` a `object`.

### <a name="class-members"></a>Členové třídy

Členy třídy jsou členy deklarované ve třídě a členy zděděné ze základní třídy (kromě třídy `object`, která nemá žádnou základní třídu). Členy zděděné ze základní třídy zahrnují konstanty, pole, metody, vlastnosti, události, indexery, operátory a typy základní třídy, ale ne konstruktory instancí, destruktory a statické konstruktory základní třídy. Členové základní třídy jsou děděni bez ohledu na jejich přístupnost.

Deklarace třídy může obsahovat deklarace konstant, pole, metody, vlastnosti, události, indexery, operátory, konstruktory instancí, destruktorů, statických konstruktorů a typů.

Členy `object` a `string` odpovídají přímo členům typů tříd, které aliasují:

*  Členy `object` jsou členy třídy `System.Object`.
*  Členy `string` jsou členy třídy `System.String`.

### <a name="interface-members"></a>Členy rozhraní

Členy rozhraní jsou členy deklarované v rozhraní a ve všech základních rozhraních rozhraní. Členy ve třídě `object` nejsou, výhradně řečeno, členové libovolného rozhraní ([Členové rozhraní](interfaces.md#interface-members)). Nicméně členové ve třídě `object` jsou k dispozici prostřednictvím vyhledávání členů v jakémkoli typu rozhraní ([vyhledávání členů](expressions.md#member-lookup)).

### <a name="array-members"></a>Členové pole

Členy pole jsou členy děděny ze třídy `System.Array`.

### <a name="delegate-members"></a>Členové delegáta

Členové delegáta jsou členy dědění ze třídy `System.Delegate`.

## <a name="member-access"></a>Přístup ke členům

Deklarace členů umožňují kontrolu nad přístupem členů. Přístupnost člena je založena na deklaraci přístupnosti ([deklarovaný přístup](basic-concepts.md#declared-accessibility)) člena v kombinaci s přístupností bezprostředně obsahujícího typu, pokud existuje.

Když je povolený přístup ke konkrétnímu členu, bude se jednat o člena, který je ***přístupný***. Naopak, pokud je zakázaný přístup ke konkrétnímu členovi, říká se, že je ***nepřístupný***. Přístup ke členu je povolený, když je textové umístění, ve kterém se přístup provádí, zahrnuté v doméně přístupnosti ([domény přístupnosti](basic-concepts.md#accessibility-domains)) člena.

### <a name="declared-accessibility"></a>Deklarovaná přístupnost

***Deklarovaná přístupnost*** člena může být jedna z následujících:

*  Public, který je vybrán zahrnutím modifikátoru `public` v deklaraci členu. Intuitivní význam `public` je "Access not Unlimited".
*  Protected, který je vybrán zahrnutím modifikátoru `protected` v deklaraci členu. Intuitivní význam `protected` je "přístup omezený na obsahující třídu nebo typy odvozené z nadřazené třídy".
*  Interní, který je vybrán zahrnutím modifikátoru `internal` v deklaraci členu. Intuitivní význam `internal` je "přístup omezený na tento program".
*  Chráněn interní (tj. chráněný nebo interní), který je vybrán zahrnutím `protected` a modifikátorem `internal` v deklaraci členu. Intuitivní význam `protected internal` je "přístup omezený na tento program nebo typy odvozené z nadřazené třídy".
*  Private, který je vybrán zahrnutím modifikátoru `private` v deklaraci členu. Intuitivní význam `private` je "přístup omezený na obsažený typ".

V závislosti na kontextu, ve kterém je deklarace členů provedena, jsou povoleny pouze určité typy deklarovaného usnadnění. Kromě toho, když deklarace člena neobsahuje žádné modifikátory přístupu, určuje kontext, ve kterém se deklarace provede, výchozí deklarovaná přístupnost.

*  Obory názvů mají implicitně `public` deklarovaný přístup. U deklarací oboru názvů nejsou povoleny žádné modifikátory přístupu.
*  Typy deklarované v jednotkách kompilace nebo oborech názvů můžou mít `public` nebo `internal` deklarované přístupnosti a výchozí hodnoty pro `internal` deklarovaného přístupu.
*  Členové třídy mohou mít kterýkoli z pěti druhů deklarovaného přístupnosti a výchozích možností, aby `private` deklarovaného přístupnosti. (Všimněte si, že typ deklarovaný jako člen třídy může mít kterýkoli z pěti druhů deklarovaného přístupnosti, zatímco typ deklarovaný jako člen oboru názvů může mít pouze `public` nebo `internal` deklarovaného přístupnosti.)
*  Členové struktury můžou mít `public`, `internal`nebo `private` deklarovaného přístupnosti a výchozí hodnoty pro `private` deklarovaná přístupnost, protože struktury jsou implicitně zapečetěné. Členy struktury představené ve struktuře (to znamená, že není zděděná touto strukturou) nemohou mít deklaraci Accessibility `protected` ani `protected internal`. (Všimněte si, že typ deklarovaný jako člen struktury může mít `public`, `internal`nebo `private` deklarovaného usnadnění, zatímco typ deklarovaný jako člen oboru názvů může mít pouze `public` nebo `internal` deklarovaný přístup.)
*  Členy rozhraní mají implicitně `public` deklarovaný přístup. V deklaracích členů rozhraní nejsou povoleny žádné modifikátory přístupu.
*  Členové výčtu mají implicitně `public` deklarovaný přístup. Pro deklarace členů výčtu nejsou povoleny žádné modifikátory přístupu.

### <a name="accessibility-domains"></a>Domény přístupnosti

***Doména přístupnosti*** člena se skládá z (případně nesouvislých) oddílů textu programu, ve kterém je povolený přístup ke členu. Pro účely definování domény přístupnosti člena je člen označován jako ***nejvyšší úroveň*** , pokud není deklarován v rámci typu, a člen je označován jako ***vnořený*** v případě, že je deklarován v rámci jiného typu. Kromě toho ***text*** programu programu je definován jako veškerý text programu obsažený ve všech zdrojových souborech programu a text programu typu je definován jako veškerý text programu obsažený v *type_declaration*s daného typu (včetně, případně typů, které jsou vnořeny do typu).

Doména přístupnosti předdefinovaného typu (například `object`, `int`nebo `double`) je neomezená.

Doména přístupnosti `T` nevázaného typu na nejvyšší úrovni ([vázané a nevázané typy](types.md#bound-and-unbound-types)), která je deklarována v programu `P` je definována takto:

*  Pokud je deklarovaná přístupnost `T` `public`, doména přístupnosti `T` je text programu `P` a jakýkoliv program, který odkazuje na `P`.
*  Pokud je deklarovaná přístupnost `T` `internal`, doména přístupnosti `T` je text programu `P`.

Z těchto definic se řídí tím, že doména přístupnosti typu bez vazby na nejvyšší úrovni je vždy alespoň text programu programu, ve kterém je tento typ deklarován.

Doména přístupnosti pro konstruovaný typ `T<A1, ..., An>` je průsečík domény přístupnosti nevázaného obecného typu `T` a domén přístupnosti argumentů typu `A1, ..., An`.

Doména přístupnosti vnořeného člena `M` deklarovaného v `T` typu v rámci programu `P` je definována takto (s označením, že by mohl být typ pravděpodobně sám `M`):

*  Pokud je deklarovaná přístupnost `M` `public`, doména přístupnosti `M` je doména přístupnosti `T`.
*  Pokud je deklarovaná přístupnost `M` `protected internal`, nechte `D` být sjednocením textu programu `P` a text programu libovolného typu odvozeného z `T`, který je deklarovaný mimo `P`. Doména přístupnosti `M` je průnik domény přístupnosti `T` s `D`.
*  Pokud je deklarovaná přístupnost `M` `protected`, nechte `D` být sjednocením textu programu `T` a textem programu libovolného typu odvozeného z `T`. Doména přístupnosti `M` je průnik domény přístupnosti `T` s `D`.
*  Pokud je deklarovaná přístupnost `M` `internal`, doména přístupnosti `M` je průnik domény přístupnosti `T` s textem programu `P`.
*  Pokud je deklarovaná přístupnost `M` `private`, doména přístupnosti `M` je text programu `T`.

Z těchto definic následuje, že doména přístupnosti vnořeného člena je vždy alespoň text programu typu, ve kterém je člen deklarován. Navíc následuje za tím, že doména přístupnosti člena není nikdy více zahrnutá než doména přístupnosti typu, ve kterém je člen deklarovaný.

V intuitivních případech platí, že při přístupu k typu nebo členu `M` je vyhodnocen následující postup, který zajistí, že přístup je povolen:

*  První, pokud je `M` deklarována v rámci typu (na rozdíl od kompilační jednotky nebo oboru názvů), dojde k chybě při kompilaci, pokud tento typ není přístupný.
*  Pokud je pak `M` `public`, povolený přístup.
*  V opačném případě, pokud je `M` `protected internal`, je přístup povolen, pokud k tomu dojde v rámci programu, ve kterém je deklarována `M`, nebo pokud k ní dojde v rámci třídy odvozené od třídy, ve které je `M` deklarována a provedena prostřednictvím odvozené třídy ([chráněný přístup pro členy instance](basic-concepts.md#protected-access-for-instance-members)).
*  V opačném případě, pokud je `M` `protected`, je přístup povolen, pokud se vyskytuje v rámci třídy, ve které je deklarován `M`, nebo pokud k ní dojde v rámci třídy odvozené od třídy, ve které `M` deklarována a provedena prostřednictvím odvozené třídy ([chráněný přístup pro členy instance](basic-concepts.md#protected-access-for-instance-members)).
*  V opačném případě, pokud je `M` `internal`, je přístup povolen, pokud k němu dojde v programu, ve kterém je deklarován `M`.
*  V opačném případě, pokud je `M` `private`, je přístup povolen, pokud k němu dojde v rámci typu, ve kterém je deklarováno `M`.
*  V opačném případě je typ nebo člen nepřístupný a dojde k chybě při kompilaci.

V příkladu
```csharp
public class A
{
    public static int X;
    internal static int Y;
    private static int Z;
}

internal class B
{
    public static int X;
    internal static int Y;
    private static int Z;

    public class C
    {
        public static int X;
        internal static int Y;
        private static int Z;
    }

    private class D
    {
        public static int X;
        internal static int Y;
        private static int Z;
    }
}
```
třídy a členové mají následující domény přístupnosti:

*  Doména přístupnosti `A` a `A.X` je neomezená.
*  Doména přístupnosti `A.Y`, `B`, `B.X`, `B.Y`, `B.C`, `B.C.X`a `B.C.Y` je text programu obsahujícího programu.
*  Doména přístupnosti `A.Z` je text programu `A`.
*  Doména přístupnosti `B.Z` a `B.D` je text programu `B`, včetně textu programu `B.C` a `B.D`.
*  Doména přístupnosti `B.C.Z` je text programu `B.C`.
*  Doména přístupnosti `B.D.X` a `B.D.Y` je text programu `B`, včetně textu programu `B.C` a `B.D`.
*  Doména přístupnosti `B.D.Z` je text programu `B.D`.

Jak ukazuje příklad, doména přístupnosti člena není nikdy větší než v nadřazeném typu. Například i v případě, že všichni `X` členové mají veřejnou deklaraci přístupnosti, všechny, ale `A.X` mají domény přístupnosti, které jsou omezeny nadřazeným typem.

Jak je popsáno v tématu [Členové](basic-concepts.md#members), jsou všechny členy základní třídy, s výjimkou konstruktorů instancí, destruktorů a statických konstruktorů, děděny odvozenými typy. To zahrnuje i soukromé členy základní třídy. Doména přístupnosti privátního člena však obsahuje pouze text programu typu, ve kterém je člen deklarován. V příkladu
```csharp
class A
{
    int x;

    static void F(B b) {
        b.x = 1;        // Ok
    }
}

class B: A
{
    static void F(B b) {
        b.x = 1;        // Error, x not accessible
    }
}
```
Třída `B` dědí soukromý člen `x` z třídy `A`. Vzhledem k tomu, že je člen soukromý, je přístupný pouze v rámci *class_body* `A`. Proto je přístup k `b.x` v metodě `A.F` úspěšný, ale v metodě `B.F` selže.

### <a name="protected-access-for-instance-members"></a>Chráněný přístup pro členy instance

Pokud je člen instance `protected` přístupný mimo text programu třídy, ve které je deklarována, a pokud je člen instance `protected internal` přístupný mimo text programu programu, ve kterém je deklarován, přístup musí probíhat v rámci deklarace třídy, která je odvozena od třídy, ve které je deklarována. Kromě toho je nutné přístup provést prostřednictvím instance daného typu odvozené třídy nebo z něj vytvořeného typu třídy. Toto omezení zabrání jedné odvozené třídě v přístupu k chráněným členům jiných odvozených tříd, a to i v případě, že jsou členové děděni ze stejné základní třídy.

Nechejte `B` být základní třídou, která deklaruje člena chráněné instance `M`a nechat `D` být třída, která je odvozena od `B`. V rámci *class_body* `D`může přístup k `M` mít jednu z následujících forem:

*  Nekvalifikovaný *TYPE_NAME* nebo *primary_expression* `M`formuláře.
*  *Primary_expression* `E.M`formuláře za předpokladu, že typ `E` je `T` nebo třída odvozená od `T`, kde `T` je typ třídy `D`nebo typ třídy vytvořený z `D`
*  `base.M`*primary_expression* formuláře.

Kromě těchto forem přístupu může odvozená třída přistupovat k konstruktoru chráněné instance základní třídy v *constructor_initializer* ([Inicializátory konstruktoru](classes.md#constructor-initializers)).

V příkladu
```csharp
public class A
{
    protected int x;

    static void F(A a, B b) {
        a.x = 1;        // Ok
        b.x = 1;        // Ok
    }
}

public class B: A
{
    static void F(A a, B b) {
        a.x = 1;        // Error, must access through instance of B
        b.x = 1;        // Ok
    }
}
```
v rámci `A`je možné přistupovat `x` prostřednictvím instancí obou `A` a `B`, protože v obou případech dojde k přístupu prostřednictvím instance `A` nebo třídy odvozené od `A`. V rámci `B`ale není možné získat přístup `x` prostřednictvím instance `A`, protože `A` neodvozuje z `B`.

V příkladu
```csharp
class C<T>
{
    protected T x;
}

class D<T>: C<T>
{
    static void F() {
        D<T> dt = new D<T>();
        D<int> di = new D<int>();
        D<string> ds = new D<string>();
        dt.x = default(T);
        di.x = 123;
        ds.x = "test";
    }
}
```
tři přiřazení k `x` jsou povolena, protože všechna probíhají prostřednictvím instancí typů třídy vytvořených z obecného typu.

### <a name="accessibility-constraints"></a>Omezení přístupnosti

Několik konstrukcí v C# jazyce vyžaduje, aby byl typ ***aspoň dostupný jako*** člen nebo jiný typ. Typ `T` se říká, že má být nejméně tak přístupný jako člen nebo typ `M`, pokud je doména přístupnosti `T` nadmnožinou domény přístupnosti `M`. Jinými slovy, `T` je alespoň tak přístupný jako `M`, pokud je `T` přístupná ve všech kontextech, ve kterých `M` k dispozici.

Existují následující omezení přístupnosti:

*  Přímá základní třída typu třídy musí být alespoň tak přístupná jako typ třídy samotné.
*  Explicitní základní rozhraní typu rozhraní musí být alespoň tak přístupná jako typ rozhraní.
*  Návratový typ a typy parametrů typu delegáta musí být k dispozici alespoň jako typ delegáta.
*  Typ konstanty musí být alespoň tak přístupný jako konstanta sama.
*  Typ pole musí být alespoň tak přístupný jako pole samotné.
*  Návratový typ a typy parametrů metody musí být k dispozici alespoň jako metoda samotná.
*  Typ vlastnosti musí být alespoň tak přístupný jako vlastnost sama.
*  Typ události musí být alespoň tak přístupný jako událost samotná.
*  Typy a parametry indexeru musí být alespoň tak přístupné jako indexer samotný.
*  Návratový typ a typy parametrů operátoru musí být k dispozici alespoň jako samotný operátor.
*  Typy parametrů konstruktoru instance musí být alespoň tak přístupné jako samotný konstruktor instance.

V příkladu
```csharp
class A {...}

public class B: A {...}
```
u `B` třídy dojde k chybě při kompilaci, protože `A` není alespoň tak přístupná jako `B`.

Podobně v příkladu
```csharp
class A {...}

public class B
{
    A F() {...}

    internal A G() {...}

    public A H() {...}
}
```
Metoda `H` v `B` má za následek chybu při kompilaci, protože návratový typ `A` není k dispozici jako metoda.

## <a name="signatures-and-overloading"></a>Signatury a přetížení

Metody, konstruktory instancí, indexery a operátory jsou charakterizovány svými ***signaturami***:

*  Signatura metody se skládá z názvu metody, počtu parametrů typu a typu a druhu (hodnota, odkaz nebo výstup) každého jeho formálního parametru, který je považován za v pořadí zleva doprava. Pro tyto účely je jakýkoli parametr typu metody, která se vyskytuje v typu formálního parametru, identifikován jako není jeho názvem, ale podle jeho ordinální pozice v seznamu argumentů typu metody. Signatura metody konkrétně neobsahuje návratový typ, modifikátor `params`, který může být zadán pro parametr Right-nejvíc, ani volitelná omezení parametrů typu.
*  Signatura konstruktoru instance se skládá z typu a druhu (hodnota, odkaz nebo výstup) každého jeho formálního parametru, který je považován za v pořadí zleva doprava. Signatura konstruktoru konstruktoru konkrétně neobsahuje modifikátor `params`, který může být zadán pro parametr Right-nejvíc.
*  Signatura indexeru se skládá z typu každého ze svých formálních parametrů, které se považují za v pořadí zleva doprava. Podpis indexeru konkrétně nezahrnuje typ elementu, ani neobsahuje modifikátor `params`, který může být zadán pro parametr Right-nejvíc.
*  Signatura operátoru se skládá z názvu operátoru a typu každého z jeho formálních parametrů, které se považují za v pořadí zleva doprava. Podpis operátora konkrétně neobsahuje typ výsledku.

Signatury jsou mechanismy povolování pro ***přetížení*** členů ve třídách, strukturách a rozhraních:

*  Přetížení metod umožňuje třídy, struktury nebo rozhraní deklarovat více metod se stejným názvem, pokud jsou jejich podpisy v rámci této třídy, struktury nebo rozhraní jedinečné.
*  Přetížení konstruktorů instancí umožňuje třídě nebo struktuře deklarovat více konstruktorů instancí za předpokladu, že jejich signatury jsou v rámci této třídy nebo struktury jedinečné.
*  Přetížení indexerů umožňuje třídy, struktury nebo rozhraní deklarovat více indexerů za předpokladu, že jejich signatury jsou v rámci této třídy, struktury nebo rozhraní jedinečné.
*  Přetížení operátorů umožňuje třídě nebo struktuře deklarovat více operátorů se stejným názvem, pokud jsou jejich podpisy v rámci této třídy nebo struktury jedinečné.

I když jsou modifikátory parametrů `out` a `ref` považovány za součást signatury, členy deklarované v jednom typu se nemohou lišit v signatuře pouze pomocí `ref` a `out`. K chybě v době kompilace dojde v případě, že dva členy jsou deklarovány ve stejném typu s signaturami, které by byly stejné, pokud se všechny parametry v obou metodách s modifikátory `out` změnily na `ref` modifikátory. Pro jiné účely porovnávání signatur (například skrytí nebo přepsání) `ref` a `out` se považují za součást signatury a vzájemně se neshodují. (Toto omezení je umožněno C# snadné překladu programů na Common Language Infrastructure (CLI), což neposkytuje způsob, jak definovat metody, které se liší pouze v `ref` a `out`.)

Pro účely signatur se typy `object` a `dynamic` považují za stejné. Členy deklarované v jednom typu se proto nemohou lišit v signatuře pouze pomocí `object` a `dynamic`.

Následující příklad ukazuje sadu přetížených deklarací metod spolu s jejich podpisy.
```csharp
interface ITest
{
    void F();                        // F()

    void F(int x);                   // F(int)

    void F(ref int x);               // F(ref int)

    void F(out int x);               // F(out int)      error

    void F(int x, int y);            // F(int, int)

    int F(string s);                 // F(string)

    int F(int x);                    // F(int)          error

    void F(string[] a);              // F(string[])

    void F(params string[] a);       // F(string[])     error
}
```

Všimněte si, že všechny modifikátory parametrů `ref` a `out` ([parametry metody](classes.md#method-parameters)) jsou součástí signatury. Proto jsou `F(int)` a `F(ref int)` jedinečnými podpisy. `F(ref int)` a `F(out int)` však nelze deklarovat v rámci stejného rozhraní, protože jejich signatury se liší výhradně pomocí `ref` a `out`. Všimněte si také, že návratový typ a modifikátor `params` nejsou součástí signatury, takže není možné přetížit pouze na základě návratového typu nebo při zahrnutí nebo vyloučení modifikátoru `params`. V takovém případě deklarace metod `F(int)` a `F(params string[])` zjistily výše uvedený výsledek při kompilaci.

## <a name="scopes"></a>Oboru

***Rozsah*** názvu je oblast textu programu, ve které je možné odkazovat na entitu deklarované názvem bez kvalifikace názvu. Obory můžou být ***vnořené***a vnitřní rozsah může znovu deklarovat význam názvu z vnějšího oboru (to ale neodebere omezení vyplývající z [deklarací](basic-concepts.md#declarations) , které v rámci vnořeného bloku není možné deklarovat místní proměnnou se stejným názvem jako místní proměnná v ohraničujícím bloku.) Název z vnějšího oboru se pak bude považovat za ***skrytý*** v oblasti textu programu, který je pokrytý vnitřním rozsahem, a přístup k vnějšímu názvu je možný jenom pomocí kvalifikovaného názvu.

*  Rozsah člena oboru názvů deklarovaného *namespace_member_declaration* ([členy oboru názvů](namespaces.md#namespace-members)) bez uzavírací *namespace_declaration* je celý text programu.
*  Rozsah člena oboru názvů deklarovaného *namespace_member_declaration* v rámci *namespace_declaration* , jehož plně kvalifikovaný název je `N` *namespace_body* všech *namespace_declaration* , jejichž plně kvalifikovaný název je `N` nebo začíná na `N`a za tečkou.
*  Rozsah názvu definovaného *extern_alias_directive* se rozšíří přes *using_directive*s, *global_attributes* a *namespace_member_declaration*s jeho okamžitou součástí, který obsahuje kompilační jednotka nebo tělo oboru názvů. *Extern_alias_directive* nepřispívají žádné nové členy do podkladového prostoru deklarací. Jinými slovy *extern_alias_directive* není přenositelné, ale místo toho ovlivňuje pouze kompilační jednotku nebo tělo oboru názvů, ve kterém se vyskytuje.
*  Rozsah názvu definovaného nebo importovaného pomocí *using_directive* ([direktivy using](namespaces.md#using-directives)) se rozšíří přes *namespace_member_declaration*s *compilation_unit* nebo *namespace_body* , ve kterém se *using_directive* vyskytuje. *Using_directive* může v rámci konkrétní *compilation_unit* nebo *namespace_body*vydávat nula nebo více názvů, typ nebo názvy členů, ale nepřispívají žádné nové členy do podkladového prostoru deklarací. Jinými slovy *using_directive* není přenosný, ale má vliv pouze na *compilation_unit* nebo *namespace_body* , ve kterém se vyskytuje.
*  Rozsah parametru typu deklarovaného *type_parameter_list* v *class_declaration* ([deklarace tříd](classes.md#class-declarations)) je *class_base*, *type_parameter_constraints_clause*s a *class_body* tohoto *class_declaration*.
*  Rozsah parametru typu deklarovaného *type_parameter_list* v *struct_declaration* ([deklarace struktury](structs.md#struct-declarations)) je *struct_interfaces*, *type_parameter_constraints_clause*s a *struct_body* tohoto *struct_declaration*.
*  Rozsah parametru typu deklarovaného *type_parameter_list* v *interface_declaration* ([deklarace rozhraní](interfaces.md#interface-declarations)) je *interface_base*, *type_parameter_constraints_clause*s a *interface_body* tohoto *interface_declaration*.
*  Rozsah parametru typu deklarovaného *type_parameter_list* v *delegate_declaration* ([deklarace delegátů](delegates.md#delegate-declarations)) je *return_type*, *formal_parameter_list*a *type_parameter_constraints_clause*s daného *delegate_declaration*.
*  Rozsah člena deklarovaného *class_member_declaration* ([tělo třídy](classes.md#class-body)) je *class_body* , ve kterém k deklaraci dojde. Kromě toho rozsah člena třídy rozšiřuje na *class_body* těch odvozených tříd, které jsou zahrnuty v doméně přístupnosti ([domény](basic-concepts.md#accessibility-domains)přístupnosti) člena.
*  Rozsah člena deklarovaného *struct_member_declaration* ([Členové struktury](structs.md#struct-members)) je *struct_body* , ve kterém k deklaraci dojde.
*  Rozsah člena deklarovaného *enum_member_declaration* ([členy výčtu](enums.md#enum-members)) je *enum_body* , ve kterém k deklaraci dojde.
*  Rozsah parametru deklarovaného v *method_declaration* ([metody](classes.md#methods)) je *method_body* tohoto *method_declaration*.
*  Rozsah parametru deklarovaného v *indexer_declaration* ([indexery](classes.md#indexers)) je *accessor_declarations* tohoto *indexer_declaration*.
*  Rozsah parametru deklarovaného v *operator_declaration* ([Operators](classes.md#operators)) je *blok* tohoto *operator_declaration*.
*  Rozsah parametru deklarovaného v *constructor_declaration* ([konstruktory Instance](classes.md#instance-constructors)) je *constructor_initializer* a *blok* tohoto *constructor_declaration*.
*  Rozsah parametru deklarovaného v *lambda_expression* ([anonymní výrazy funkce](expressions.md#anonymous-function-expressions)) je *anonymous_function_body* tohoto *lambda_expression*
*  Rozsah parametru deklarovaného v *anonymous_method_expression* ([anonymní výrazy funkce](expressions.md#anonymous-function-expressions)) je *blok* tohoto *anonymous_method_expression*.
*  Rozsah popisku deklarovaného v *labeled_statement* ([příkazy s popiskem](statements.md#labeled-statements)) je *blok* , ve kterém k deklaraci dojde.
*  Rozsah místní proměnné deklarované v *local_variable_declaration* ([deklarace místní proměnné](statements.md#local-variable-declarations)) je blok, ve kterém se deklarace vyskytuje.
*  Rozsah místní proměnné deklarované v *switch_block* příkazu `switch` ([příkaz switch](statements.md#the-switch-statement)) je *switch_block*.
*  Rozsah místní proměnné deklarované v *for_initializer* příkazu `for` ([pro příkaz for](statements.md#the-for-statement)) je *for_initializer*, *for_condition*, *for_iterator*a obsažený *příkaz* `for`.
*  Rozsah místní konstanty deklarované v *local_constant_declaration* ([místní konstanta deklarace](statements.md#local-constant-declarations)) je blok, ve kterém k deklaraci dojde. Jedná se o chybu při kompilaci, která odkazuje na místní konstantu v textové pozici, která předchází jeho *constant_declarator*.
*  Rozsah proměnné deklarované jako součást *foreach_statement*, *using_statement*, *lock_statement* nebo *query_expression* je určen rozšířením dané konstrukce.

V rámci rozsahu oboru názvů, třídy, struktury nebo člena výčtu je možné odkazovat na člen v textové pozici, která předchází deklaraci členu. Například
```csharp
class A
{
    void F() {
        i = 1;
    }

    int i = 0;
}
```
Tady je platný, aby `F` odkazoval na `i` předtím, než se deklaruje.

V rámci rozsahu místní proměnné se jedná o chybu při kompilaci, která odkazuje na místní proměnnou na textové pozici, která předchází *local_variable_declarator* místní proměnné. Například
```csharp
class A
{
    int i = 0;

    void F() {
        i = 1;                  // Error, use precedes declaration
        int i;
        i = 2;
    }

    void G() {
        int j = (j = 1);        // Valid
    }

    void H() {
        int a = 1, b = ++a;    // Valid
    }
}
```

V metodě `F` výše první přiřazení `i` výslovně neodkazuje na pole deklarované ve vnějším oboru. Místo toho odkazuje na místní proměnnou a má za následek chybu při kompilaci, protože předá deklaraci proměnné. V metodě `G` je použití `j` v inicializátoru pro deklaraci `j` platné, protože použití nepředchází *local_variable_declarator*. V metodě `H` následující *local_variable_declarator* správně odkazuje na místní proměnnou deklarovanou v dřívějším *local_variable_declarator* v rámci stejného *local_variable_declaration*.

Pravidla oboru pro lokální proměnné jsou navržena tak, aby zaručila, že význam názvu používaného v kontextu výrazu je vždy stejný v rámci bloku. Pokud byl rozsah místní proměnné rozšířen pouze z deklarace na konec bloku, pak v příkladu výše se první přiřazení přiřadí proměnné instance a druhé přiřazení by se mohlo přiřazovat k místní proměnné, což může vést k chyby při kompilaci, pokud byly příkazy bloku později přeuspořádány.

Význam názvu v rámci bloku se může lišit v závislosti na kontextu, ve kterém je název použit. V příkladu
```csharp
using System;

class A {}

class Test
{
    static void Main() {
        string A = "hello, world";
        string s = A;                            // expression context

        Type t = typeof(A);                      // type context

        Console.WriteLine(s);                    // writes "hello, world"
        Console.WriteLine(t);                    // writes "A"
    }
}
```
název `A` se používá v kontextu výrazu pro odkazování na místní proměnnou `A` a v kontextu typu pro odkazování na `A`třídy.

### <a name="name-hiding"></a>Skrytí názvu

Rozsah entity obvykle zahrnuje větší text programu než místo deklarace entity. Konkrétně rozsah entity může zahrnovat deklarace, které zavádějí nové prostory deklarací obsahující entity se stejným názvem. Tyto deklarace způsobí, že původní entita bude ***Skrytá***. Naopak entita je označována jako ***viditelná*** , pokud není skrytá.

K skrývání názvů dojde, když se obory překrývají prostřednictvím vnořování a když se obory překrývají prostřednictvím dědičnosti. Vlastnosti dvou typů skrytí jsou popsány v následujících částech.

#### <a name="hiding-through-nesting"></a>Skrytí prostřednictvím vnořování

Název, který se skrývá prostřednictvím vnořování, může nastat jako výsledek vnořování oborů názvů nebo typů v oborech názvů jako výsledek vnořování typů v rámci tříd nebo struktur, a v důsledku deklarací parametrů a místních proměnných.

V příkladu
```csharp
class A
{
    int i = 0;

    void F() {
        int i = 1;
    }

    void G() {
        i = 1;
    }
}
```
v rámci metody `F` je proměnná instance `i` skrytá místní proměnnou `i`, ale v rámci metody `G` `i` stále odkazuje na proměnnou instance.

Když název ve vnitřním oboru skrývá název ve vnějším oboru, skryje všechny přetížené výskyty daného názvu. V příkladu
```csharp
class Outer
{
    static void F(int i) {}

    static void F(string s) {}

    class Inner
    {
        void G() {
            F(1);              // Invokes Outer.Inner.F
            F("Hello");        // Error
        }

        static void F(long l) {}
    }
}
```
volání `F(1)` vyvolá `F` deklarované v `Inner`, protože vnitřní deklarace má skryté všechny vnější výskyty `F`. Ze stejného důvodu `F("Hello")` volání způsobí chybu při kompilaci.

#### <a name="hiding-through-inheritance"></a>Skrytí prostřednictvím dědičnosti

K skrývání názvů prostřednictvím dědičnosti dojde, když třídy nebo struktury znovu deklaruje názvy, které byly děděny ze základních tříd. Tento typ názvu skrývání má jednu z následujících forem:

*  Konstanta, pole, vlastnost, událost nebo typ zavedený ve třídě nebo struktuře skrývá všechny členy základní třídy se stejným názvem.
*  Metoda zavedená ve třídě nebo struktuře skrývá všechny členy základní třídy bez metod se stejným názvem a všechny metody základní třídy se stejným podpisem (název metody a počet parametrů, modifikátory a typy).
*  Indexer zavedený ve třídě nebo struktuře skrývá všechny základní třídy indexery se stejnou signaturou (počet parametrů a typy).

Pravidla řídící deklarace operátorů ([operátory](classes.md#operators)) znemožňují, aby odvozená třída deklarovala operátor se stejnou signaturou jako operátor v základní třídě. Proto operátory nikdy neskrývají.

V rozporu s skrytím názvu z vnějšího oboru způsobí skrytí přístupového názvu z zděděného oboru upozornění. V příkladu
```csharp
class Base
{
    public void F() {}
}

class Derived: Base
{
    public void F() {}        // Warning, hiding an inherited name
}
```
deklarace `F` v `Derived` způsobí, že bude Hlášeno upozornění. Skrytí zděděného názvu není specificky chyba, protože by to vedlo k samostatnému vývoji základních tříd. Výše uvedená situace mohla například proniknout, protože novější verze `Base` představila `F` metodu, která nebyla přítomna v dřívější verzi třídy. V výše uvedené situaci došlo k chybě, takže jakákoli změna základní třídy v knihovně tříd oddělené verze by mohla potenciálně způsobit, že odvozené třídy stanou být neplatné.

Upozornění způsobené skrytím zděděného názvu lze odstranit pomocí modifikátoru `new`:
```csharp
class Base
{
    public void F() {}
}

class Derived: Base
{
    new public void F() {}
}
```

Modifikátor `new` označuje, že `F` v `Derived` je "nové" a že je skutečně určena pro skrytí zděděného člena.

Deklarace nového člena skrývá zděděného člena pouze v rámci oboru nového člena.

```csharp
class Base
{
    public static void F() {}
}

class Derived: Base
{
    new private static void F() {}    // Hides Base.F in Derived only
}

class MoreDerived: Derived
{
    static void G() { F(); }          // Invokes Base.F
}
```

V předchozím příkladu deklarace `F` v `Derived` skrývá `F`, která byla zděděná od `Base`, ale vzhledem k tomu, že nový `F` `Derived` má privátní přístup, jeho obor se nerozšiřuje na `MoreDerived`. Proto je volání `F()` v `MoreDerived.G` platné a vyvolá `Base.F`.

## <a name="namespace-and-type-names"></a>Obor názvů a názvy typů

Několik kontextů v C# programu vyžaduje zadání *namespace_name* nebo *TYPE_NAME* .

```antlr
namespace_name
    : namespace_or_type_name
    ;

type_name
    : namespace_or_type_name
    ;

namespace_or_type_name
    : identifier type_argument_list?
    | namespace_or_type_name '.' identifier type_argument_list?
    | qualified_alias_member
    ;
```

*Namespace_name* je *namespace_or_type_name* , který odkazuje na obor názvů. Následující řešení, jak je popsáno níže, *namespace_or_type_name* *namespace_name* musí odkazovat na obor názvů nebo v případě chyby při kompilaci. V *namespace_name* se nesmí vyskytovat žádné argumenty typu ([argumenty typu](types.md#type-arguments)) (argumenty typu můžou mít jenom typy).

*TYPE_NAME* je *namespace_or_type_name* , který odkazuje na typ. Následující řešení, jak je popsáno níže, *namespace_or_type_name* *TYPE_NAME* musí odkazovat na typ nebo jinak dojde k chybě při kompilaci.

Pokud *namespace_or_type_name* je kvalifikovaný alias členu, jak je popsáno v [kvalifikátorech aliasu oboru názvů](namespaces.md#namespace-alias-qualifiers). V opačném případě *namespace_or_type_name* má jednu ze čtyř forem:

*  `I`
*  `I<A1, ..., Ak>`
*  `N.I`
*  `N.I<A1, ..., Ak>`

Pokud je `I` jediným identifikátorem, `N` je *namespace_or_type_name* a `<A1, ..., Ak>` je volitelná *type_argument_list*. Pokud není zadaný žádný *type_argument_list* , zvažte `k` na nulu.

Význam *namespace_or_type_name* se určuje takto:

*   Pokud má *namespace_or_type_name* `I` nebo formuláře `I<A1, ..., Ak>`:
    * Pokud je `K` nula a *namespace_or_type_name* se objeví v rámci deklarace obecné metody ([metody](classes.md#methods)) a pokud tato deklarace obsahuje parametr typu ([parametry typu](classes.md#type-parameters)) s názvem `I`, pak *namespace_or_type_name* odkazuje na tento parametr typu.
    * V opačném případě, pokud se *namespace_or_type_name* objeví v rámci deklarace typu, pak pro každý typ instance `T` ([typ instance](classes.md#the-instance-type)), počínaje typem instance této deklarace a pokračování s typem instance každé nadřazené třídy nebo deklarace struktury (pokud existuje):
        * Pokud je `K` nula a deklarace `T` obsahuje parametr typu s názvem `I`, pak *namespace_or_type_name* odkazuje na tento parametr typu.
        * V opačném případě, pokud se *namespace_or_type_name* objeví v těle deklarace typu a `T` nebo některý z jeho základních typů obsahuje vnořený přístupný typ, který má název `I` a `K` parametry typu, pak *namespace_or_type_name* odkazuje na tento typ sestavený pomocí daných argumentů typu. Pokud existuje více než jeden takový typ, je vybrán typ deklarovaný v rámci více odvozeného typu. Všimněte si, že netypové členy (konstanty, pole, metody, vlastnosti, indexery, operátory, konstruktory instancí, destruktory a statické konstruktory) a členy typu s jiným počtem parametrů typu jsou při určování významu *namespace_or_type_name*ignorovány.
    * Pokud předchozí kroky byly neúspěšné, potom pro každý `N`oboru názvů počínaje oborem názvů, ve kterém *namespace_or_type_name* dochází, pokračuje s každým ohraničujícím oborem názvů (pokud existuje) a končí globálním oborem názvů, jsou vyhodnoceny následující kroky, dokud není nalezena entita:
        * Pokud je `K` nula a `I` je název oboru názvů v `N`, pak:
            * Pokud se umístění, kde *namespace_or_type_name* dochází, je uzavřeno deklarací oboru názvů pro `N` a deklarace oboru názvů obsahuje *extern_alias_directive* nebo *using_alias_directive* , které přidruží název `I` k oboru názvů nebo typu, *namespace_or_type_name* je nejednoznačný a dojde k chybě při kompilaci.
            * V opačném případě *namespace_or_type_name* odkazuje na obor názvů s názvem `I` v `N`.
        * Jinak, pokud `N` obsahuje přístupný typ s názvem `I` a `K` parametry typu, pak:
            * Pokud je `K` nula a umístění, kde se *namespace_or_type_name* vyskytuje, je uzavřeno deklarací oboru názvů pro `N` a deklarace oboru názvů obsahuje *extern_alias_directive* nebo *using_alias_directive* , který přidruží název `I` k oboru názvů nebo typu, *namespace_or_type_name* je nejednoznačný a dojde k chybě při kompilaci.
            * V opačném případě *namespace_or_type_name* odkazuje na typ sestavený pomocí daných argumentů typu.
        * V opačném případě, pokud je umístění, kde *namespace_or_type_name* dojde, uzavřeno deklarací oboru názvů pro `N`:
            * Pokud je `K` nula a deklarace oboru názvů obsahuje *extern_alias_directive* nebo *using_alias_directive* , které přidruží název `I` k importovanému oboru názvů nebo typu, pak *namespace_or_type_name* odkazuje na tento obor názvů nebo typ.
            * V opačném případě, pokud deklarace oborů názvů a typů importované pomocí *using_namespace_directive*s a *using_alias_directive*s deklarací oboru názvů obsahují přesně jeden přístupný typ s názvem `I` a `K` parametry typu, pak *namespace_or_type_name* odkazuje na tento typ sestavený pomocí daných argumentů typu.
            * V opačném případě, pokud deklarace oborů názvů a typů importované pomocí *using_namespace_directive*s a *using_alias_directive*s deklarací oboru názvů obsahují více než jeden přístupný typ s názvem `I` a `K` parametry typu, *namespace_or_type_name* je nejednoznačný a dojde k chybě.
    * V opačném případě *namespace_or_type_name* není definován a dojde k chybě při kompilaci.
*  V opačném případě je *namespace_or_type_name* `N.I` formuláře nebo formuláře `N.I<A1, ..., Ak>`. `N` je nejprve vyřešen jako *namespace_or_type_name*. Pokud řešení `N` neproběhne úspěšně, dojde k chybě při kompilaci. V opačném případě `N.I` nebo `N.I<A1, ..., Ak>` vyřešeny následujícím způsobem:
    * Pokud je `K` nula a `N` odkazuje na obor názvů a `N` obsahuje vnořený obor názvů s názvem `I`, pak *namespace_or_type_name* odkazuje na tento vnořený obor názvů.
    * V opačném případě, pokud `N` odkazuje na obor názvů a `N` obsahuje přístupný typ, který má název `I` a `K` parametry typu, pak *namespace_or_type_name* odkazuje na tento typ sestavený pomocí daných argumentů typu.
    * V opačném případě, pokud `N` odkazuje na (případně konstruovaný) třídu nebo typ struktury a `N` nebo některá z jeho základních tříd obsahuje vnořený přístupný typ, který má název `I` a `K` parametry typu, pak *namespace_or_type_name* odkazuje na tento typ vytvořený pomocí daných argumentů typu. Pokud existuje více než jeden takový typ, je vybrán typ deklarovaný v rámci více odvozeného typu. Všimněte si, že pokud je význam `N.I` určován jako součást překladu specifikace základní třídy `N` pak je přímá základní třída `N` považována za Object ([základní třídy](classes.md#base-classes)).
    * V opačném případě je `N.I` neplatný *namespace_or_type_name*a dojde k chybě při kompilaci.

*Namespace_or_type_name* je povolen odkazování na statickou třídu ([statické třídy](classes.md#static-classes)) pouze v případě, že

*  *Namespace_or_type_name* je `T` v *namespace_or_type_name* formuláře `T.I`nebo
*  *Namespace_or_type_name* je `T` ve *typeof_expression* ([seznam argumentů](expressions.md#argument-lists)1) `typeof(T)`formuláře.

### <a name="fully-qualified-names"></a>Plně kvalifikované názvy

Každý obor názvů a typ má ***plně kvalifikovaný název***, který jednoznačně identifikuje obor názvů nebo typ mezi všemi ostatními. Plně kvalifikovaný název oboru názvů nebo typu `N` je určen následujícím způsobem:

*  Pokud je `N` členem globálního oboru názvů, je jeho plně kvalifikovaný název `N`.
*  V opačném případě je jeho plně kvalifikovaný název `S.N`, kde `S` je plně kvalifikovaný název oboru názvů nebo typu, ve kterém je deklarován `N`.

Jinými slovy je plně kvalifikovaný název `N` úplnou hierarchickou cestu identifikátorů, které vedou k `N`, počínaje globálním oborem názvů. Vzhledem k tomu, že každý člen oboru názvů nebo typu musí mít jedinečný název, následuje za tím, že plně kvalifikovaný název oboru názvů nebo typu je vždycky jedinečný.

Následující příklad ukazuje několik deklarací obor názvů a typů spolu s přidruženými plně kvalifikovanými názvy.
```csharp
class A {}                // A

namespace X               // X
{
    class B               // X.B
    {
        class C {}        // X.B.C
    }

    namespace Y           // X.Y
    {
        class D {}        // X.Y.D
    }
}

namespace X.Y             // X.Y
{
    class E {}            // X.Y.E
}
```

## <a name="automatic-memory-management"></a>Automatická správa paměti

C#využívá automatickou správu paměti, která vývojářům uvolňuje v ručním přidělování a uvolňování paměti obsazené objekty. Automatické zásady správy paměti jsou implementované systémem ***uvolňování***paměti. Životní cyklus správy paměti objektu je následující:

1. Když je objekt vytvořen, je pro něj přidělena paměť, konstruktor je spuštěn a objekt je považován za dynamický.
2. V případě, že k objektu nebo jakékoli jeho části nelze mít k dispozici žádné možné pokračování v provádění, jiné než spuštění destruktorů, objekt je považován za již nepoužit a bude způsobilý k zničení. C# Kompilátor a systém uvolňování paměti se můžou rozhodnout analyzovat kód a určit, které odkazy na objekt se můžou v budoucnu použít. Například pokud místní proměnná, která je v oboru, je jediným existujícím odkazem na objekt, ale tato místní proměnná nikdy neodkazuje na jakékoli možné pokračování spuštění z aktuálního bodu spuštění v proceduře, může být systém uvolňování paměti (ale není vyžadováno pro), aby byl objekt považován za již nepoužitý.
3. Jakmile je objekt způsobilý k zničení, v některých nespecifikovaných pozdějších případech je spuštěn[destruktor (pokud existuje) pro](classes.md#destructors)daný objekt. Za normálních okolností je destruktor pro objekt spuštěn pouze jednou, i když rozhraní API specifická pro implementaci mohou toto chování přepsat.
4. Po spuštění destruktoru pro objekt, pokud je k tomuto objektu nebo jakékoli části k němu nelze získat přístup jakýmkoli možným pokračováním v provádění, včetně spuštění destruktorů, je objekt považován za nedostupný a objekt se bude jednat o oprávněný pro kolekci.
5. Nakonec, v některých případech, kdy se objekt bude oprávněn pro shromažďování, uvolní uvolňování paměti paměť přidruženou k tomuto objektu.

Systém uvolňování paměti uchovává informace o použití objektů a používá tyto informace k tomu, aby bylo možné provádět rozhodnutí týkající se správy paměti, například kde v paměti vyhledat nově vytvořený objekt, kdy se má objekt přemístit, a když se objekt již nepoužívá nebo není přístupný.

Podobně jako u jiných jazyků, které předpokládají existence systému uvolňování C# paměti, je navrženo tak, aby systém uvolňování paměti mohl implementovat široké spektrum zásad správy paměti. C# Například nevyžaduje, aby byly destruktory spuštěny, nebo aby byly shromažďovány, jakmile jsou způsobilé, nebo že destruktory jsou spouštěny v libovolném konkrétním pořadí nebo v jakémkoli konkrétním vlákně.

Chování systému uvolňování paměti lze na určitou míru ovládat prostřednictvím statických metod třídy `System.GC`. Tato třída se dá použít k podání žádosti o kolekci, destruktory, které se mají spustit (nebo nespouštět), a tak dále.

Vzhledem k tomu, že systém uvolňování paměti je povolený rozsáhlá Zeměpisná šířka při rozhodování o shromažďování objektů a spouštění destruktorů, může vyhovující implementace způsobit výstup, který se liší od toho, který je zobrazen v následujícím kódu. Program
```csharp
using System;

class A
{
    ~A() {
        Console.WriteLine("Destruct instance of A");
    }
}

class B
{
    object Ref;

    public B(object o) {
        Ref = o;
    }

    ~B() {
        Console.WriteLine("Destruct instance of B");
    }
}

class Test
{
    static void Main() {
        B b = new B(new A());
        b = null;
        GC.Collect();
        GC.WaitForPendingFinalizers();
    }
}
```
Vytvoří instanci třídy `A` a instanci `B`třídy. Tyto objekty jsou způsobilé pro uvolňování paměti, když je proměnné `b` přiřazena hodnota `null`, protože po této době není možné, aby k nim měl přístup žádný kód, který by měl být vytvořen uživatelem. Výstup může být buď

```console
Destruct instance of A
Destruct instance of B
```
nebo
```console
Destruct instance of B
Destruct instance of A
```
vzhledem k tomu, že jazyk ukládá žádná omezení v pořadí, ve kterém jsou objekty uvolněny z paměti.

V nejemných případech může být rozdíl mezi "způsobilým zničením" a "způsobilou pro kolekci" důležitý. Například
```csharp
using System;

class A
{
    ~A() {
        Console.WriteLine("Destruct instance of A");
    }

    public void F() {
        Console.WriteLine("A.F");
        Test.RefA = this;
    }
}

class B
{
    public A Ref;

    ~B() {
        Console.WriteLine("Destruct instance of B");
        Ref.F();
    }
}

class Test
{
    public static A RefA;
    public static B RefB;

    static void Main() {
        RefB = new B();
        RefA = new A();
        RefB.Ref = RefA;
        RefB = null;
        RefA = null;

        // A and B now eligible for destruction
        GC.Collect();
        GC.WaitForPendingFinalizers();

        // B now eligible for collection, but A is not
        if (RefA != null)
            Console.WriteLine("RefA is not null");
    }
}
```

Pokud se ve výše uvedeném programu systém uvolňování paměti rozhodne spustit destruktor `A` před Destruktorem `B`, může být výstup tohoto programu následující:
```console
Destruct instance of A
Destruct instance of B
A.F
RefA is not null
```

Všimněte si, že i když se instance `A` nepoužívá a byl spuštěný destruktor `A`, je stále možné používat metody `A` (v tomto případě `F`), které mají být volány z jiného destruktoru. Všimněte si také, že při spuštění destruktoru může dojít k opakovanému použití objektu v programu hlavní. V tomto případě spuštění destruktoru `B`způsobilo, že se instance `A`, která se dřív nepoužila k zpřístupnění z živého odkazu `Test.RefA`. Po volání `WaitForPendingFinalizers`má instance `B` nárok na kolekci, ale instance `A` není z důvodu referenčního `Test.RefA`.

Aby nedošlo k nejasnostem a neočekávanému chování, je obecně vhodné, aby destruktory prováděly pouze čištění dat uložených ve vlastních polích objektu a aby neprováděly žádné akce s odkazovanými objekty nebo statickými poli.

Alternativou k použití destruktorů je umožnit třídě implementovat rozhraní `System.IDisposable`. To umožňuje klientovi objektu určit, kdy uvolnit prostředky objektu, obvykle přístupem k objektu jako prostředku v příkazu `using` ([příkaz using](statements.md#the-using-statement)).

## <a name="execution-order"></a>Pořadí spouštění

Spuštění C# programu pokračuje tak, že vedlejší účinky každého zpracovávaného vlákna jsou zachovány v kritických bodech provádění. ***Vedlejší efekt*** je definován jako čtení nebo zápis nestálého pole, zápis do nestálé proměnné, zápis do externího prostředku a vyvolání výjimky. Kritické body provádění, při kterých je třeba zachovat pořadí těchto vedlejších účinků, jsou odkazy na nestálá pole ([pole s modifikátorem volatile](classes.md#volatile-fields)), `lock` příkazy ([příkaz Lock](statements.md#the-lock-statement)) a vytvoření a ukončení vlákna. Spouštěcí prostředí je zdarma, aby bylo možné změnit pořadí provádění C# programu, a to s těmito omezeními:

*  Závislost dat se zachovává v rámci vlákna provádění. To znamená, že hodnota každé proměnné je vypočítána jako v případě, že všechny příkazy ve vlákně byly provedeny v původní pořadí programu.
*  Pravidla pro pořadí inicializace jsou zachována ([inicializace polí](classes.md#field-initialization) a [Inicializátory proměnných](classes.md#variable-initializers)).
*  Pořadí vedlejších účinků je zachováno s ohledem na nestálá čtení a zápisy ([pole s stálým](classes.md#volatile-fields)počtem). Kromě toho prostředí pro spuštění nemusí vyhodnotit část výrazu, pokud může odvodit, že hodnota tohoto výrazu není použita a že nejsou vytvořeny žádné požadované vedlejší účinky (včetně jakékoli příčiny způsobené voláním metody nebo přístupu k nestálému poli). Když je provádění programu přerušeno asynchronní událostí (například výjimka vyvolaná jiným vláknem), není zaručeno, že pozorovatelé vedlejší účinky jsou viditelné v původním pořadí programu.
