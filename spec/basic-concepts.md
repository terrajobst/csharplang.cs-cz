---
ms.openlocfilehash: 1c3d05674f8f7b69e70e0d9e06021537fc45f7ed
ms.sourcegitcommit: 3fc033b6e98ed7ecdf46a85c79b00a3a3ddcf963
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/18/2019
ms.locfileid: "47229916"
---
# <a name="basic-concepts"></a>Základní koncepty

## <a name="application-startup"></a>Spuštění aplikace

Sestavení, který má ***vstupní bod*** je volána ***aplikace***. Pokud je aplikace spustit nový ***aplikační domény*** se vytvoří. Několik různých instancí aplikace může existovat ve stejném počítači ve stejnou dobu, a každý má své vlastní domény aplikace.

Domény aplikace umožňuje izolace aplikací tak, že funguje jako kontejner pro stav aplikace. Domény aplikace funguje jako kontejner a hranice pro typy definované v aplikaci a knihovny tříd, které používá. Typy načtena do jedné domény aplikace se liší od stejného typu načtena do jiné domény aplikace a instance objektů nejsou sdíleny přímo mezi doménami aplikace. Například každá doména aplikace má vlastní kopii statické proměnné pro tyto typy a statický konstruktor pro typ je spustit maximálně jednou pro každé aplikační doméně. Implementace je zdarma k poskytování specifický pro implementaci zásad nebo mechanismy pro vytváření a ničení aplikačních domén.

***Spuštění aplikace*** nastane, pokud prostředí provádění volání určené metody, která se označuje jako vstupní bod aplikace. Tuto metodu vstupního bodu je vždy pojmenováno `Main`a může mít jednu z následujících podpisech:

```csharp
static void Main() {...}

static void Main(string[] args) {...}

static int Main() {...}

static int Main(string[] args) {...}
```

Jak je znázorněno, může volitelně vrátit vstupním bodem `int` hodnotu. To vrátit hodnota se používá v ukončení aplikace ([ukončení aplikace](basic-concepts.md#application-termination)).

Vstupní bod může mít jeden formální parametr. Tento parametr může mít libovolný název, ale musí být typu parametru `string[]`. Pokud je k dispozici formální parametr, vytvoří prostředí pro spouštění a předá `string[]` argument obsahující argumenty příkazového řádku, které bylo zadané při spuštění aplikace. `string[]` Nikdy argument má hodnotu null, ale může mít nulovou délku, pokud nebyly zadány žádné argumenty příkazového řádku.

Od C# podporuje metodu přetížení, třídy nebo struktury může obsahovat několik definic některé metody, pokud každý má jiný podpis. Ale v jediném programu, žádné třídy nebo struktury může obsahovat více než jednu metodu s názvem `Main` jehož definice kvalifikuje to má být použit jako vstupní bod aplikace. Další přetížené verze `Main` jsou povolené, ale pokud mají více než jeden parametr nebo jejich jediným parametrem je jiné než typu `string[]`.

Aplikace lze vytvořit více tříd nebo struktur. Je možné pro více než jeden z těchto tříd nebo struktur obsahuje metodu nazvanou `Main` jehož definice kvalifikuje to má být použit jako vstupní bod aplikace. V takových případech musíte použít externího mechanismu (jako je například možnost příkazového řádku kompilátoru) vyberte jednu z těchto `Main` metody jako vstupní bod.

V jazyce C# každé metody musí být definován jako člen třídy nebo struktury. Obvykle je deklarovaná přístupnost ([deklarovaná přístupnost](basic-concepts.md#declared-accessibility)) metody je určeno modifikátory přístupu ([modifikátorů přístupu](classes.md#access-modifiers)) zadanou v jeho deklaraci a podobně deklarovaný Přístupnost typu se určuje podle přístupu modifikátory přístupu zadaná v jeho deklaraci. Aby dané metody daného typu bude volat musí být typu a členu přístupné. Vstupní bod aplikace, ale je zvláštní případ. Konkrétně spouštěcí prostředí mají přístup k vstupní bod aplikace bez ohledu na jeho deklarovaná přístupnost a bez ohledu na to je deklarovaná přístupnost její nadřazený typ deklarace.

Metodu vstupního bodu aplikace nemusí být v deklaraci obecné třídy.

V ostatních ohledech metody vstupního bodu chovají jako ty, které nejsou vstupní body.

## <a name="application-termination"></a>Ukončení aplikace

***Ukončení aplikace*** vrátí řízení do prostředí pro spuštění.

Pokud návratový typ aplikace ***vstupní bod*** je metoda `int`, vrácená hodnota slouží jako aplikace ***stavový kód ukončení***. Účelem tohoto kódu je umožnit komunikaci úspěšné či neúspěšné spuštění prostředí.

Pokud je návratový typ metodu vstupního bodu `void`, dosáhnout pravou složenou závorku (`}`), který ukončí metody nebo provádění `return` příkaz, který nemá žádný výraz výsledkem stavový kód ukončení `0`.

Před ukončení aplikace, jsou volány destruktory všech objektů, které ještě nebyly vynuceno uvolnění paměti, pokud takové vyčistit došlo k potlačení (voláním metody knihovny `GC.SuppressFinalize`, například).

## <a name="declarations"></a>Deklarace

Deklarace v programu v jazyce C# definuje rozložení na základní prvky programu. Programy jazyka C# jsou uspořádané pomocí oborů názvů ([obory názvů](namespaces.md)), který může obsahovat typ deklarace a deklarace vnořené oboru názvů. Zadejte prohlášení ([typ deklarace](namespaces.md#type-declarations)) se používají k definování tříd ([třídy](classes.md)), struktur ([struktury](structs.md)), rozhraní ([rozhraní](interfaces.md) ), výčtů ([výčty](enums.md)) a delegáti ([delegáti](delegates.md)). Seznam typů členů v deklaraci typu závisí na formuláři deklaraci typu. Například, deklarace tříd může obsahovat deklarace konstanty ([konstanty](classes.md#constants)), pole ([pole](classes.md#fields)), metody ([metody](classes.md#methods)), vlastností ([ Vlastnosti](classes.md#properties)), události ([události](classes.md#events)), indexery ([indexery](classes.md#indexers)), operátory ([operátory](classes.md#operators)), konstruktory instancí ([ Konstruktory instancí](classes.md#instance-constructors)), statické konstruktory ([statické konstruktory](classes.md#static-constructors)), destruktory ([destruktory](classes.md#destructors)) a vnořené typy ([vnořené typy](classes.md#nested-types)).

Definuje název v deklaraci ***místa deklarace*** , ke které patří deklarace. S výjimkou přetížení členů ([podpisy a přetížení](basic-concepts.md#signatures-and-overloading)), je chyba kompilace mít dva nebo více deklarací, které zavádějí členy se stejným názvem v prostoru deklarace. Nikdy je možné místo deklarace obsahující různé druhy členů se stejným názvem. Například místo deklarace můžete, nikdy neobsahuje pole a metody se stejným názvem.

Existuje několik různých typů prostorů prohlášení, jak je popsáno v následující.

*  V rámci všech zdrojových souborů programu *namespace_member_declaration*s s uzavíracím žádné *namespace_declaration* členem jedné kombinované deklaraci místo volána ***globální deklarace místo***.
*  V rámci všech zdrojových souborů programu *namespace_member_declaration*s v rámci *namespace_declaration*, které mají stejný plně kvalifikovaný obor názvů jsou členy jedné kombinované deklaraci místo.
*  Každou deklaraci rozhraní, třídy nebo struktury vytvoří nové prohlášení prostor. Názvy jsou zavedeny do tohoto prostoru deklarace prostřednictvím *class_member_declaration*s, *struct_member_declaration*s, *interface_member_declaration*s, nebo *type_parameter*s. S výjimkou konstruktor instance přetížené deklarace a statický konstruktor deklarace, třídy nebo struktury nemohou obsahovat deklarace člena se stejným názvem jako třída nebo struktura. Třídy, struktury nebo rozhraní umožňuje deklaraci přetížené metody, indexery. Kromě toho třídě nebo struktuře umožňuje deklaraci instance přetížené konstruktory a operátory. Například třídy, struktury nebo rozhraní může obsahovat více deklarace metod se stejným názvem, pokud tyto deklarace metody se liší v jejich podpisu ([podpisy a přetížení](basic-concepts.md#signatures-and-overloading)). Všimněte si, že základní třídy se nepočítají do místa deklarace třídy a základních rozhraní nepočítají do místa deklarace rozhraní. Proto odvozené třídy nebo rozhraní může deklarovat člena se stejným názvem jako zděděného člena. Tento člen se říká, že ***skrýt*** zděděný člen.
*  Každou deklaraci delegáta vytvoří nové prohlášení prostor. Názvy jsou zavedeny do tohoto prostoru deklarace prostřednictvím formální parametry (*fixed_parameter*s a *parameter_array*s) a *type_parameter*s.
*  Každou deklaraci výčtu vytvoří nové prohlášení prostor. Názvy jsou zavedeny do tohoto prostoru deklarace prostřednictvím *enum_member_declarations*.
*  Každou deklaraci metody, indexer deklarace, operátor, deklarace konstruktoru instance a anonymní funkce vytvoří nový prostor deklarace volá ***místa deklarace lokální proměnné***. Názvy jsou zavedeny do tohoto prostoru deklarace prostřednictvím formální parametry (*fixed_parameter*s a *parameter_array*s) a *type_parameter*s. Tělo funkce člena nebo anonymní funkce, pokud existuje, se považuje za být vnořen v rámci deklarace lokální proměnné místa. Jedná se o chybu pro prostor deklarace lokální proměnné a prostor vnořené deklarace lokální proměnné tak, aby obsahovala elementy se stejným názvem. Díky tomu se v rámci oboru vnořené deklarace není možné k deklarování místní proměnné nebo konstantní se stejným názvem jako místní proměnná nebo konstantní v nadřazeném místa deklarace. Je možné pro dvě deklarace mezery obsahovat elementy se stejným názvem jako ani jedna deklarace prostor obsahuje druhý.
*  Každý *bloku* nebo *switch_block* , a také *pro*, *foreach* a *pomocí* vytvoří příkaz, deklarace lokální proměnné prostoru pro místní proměnné a místní konstanty. Názvy jsou zavedeny do tohoto prostoru deklarace prostřednictvím *local_variable_declaration*s a *local_constant_declaration*s. Všimněte si, že jsou vnořené bloky, ke kterým dochází jako nebo v rámci těla členské funkce nebo anonymní funkce v rámci deklarace lokální proměnné prostoru deklarované pomocí těchto funkcí pro své parametry. Proto se bude chyba, která máte třeba metodu s místní proměnné a parametr se stejným názvem.
*  Každý *bloku* nebo *switch_block* vytvoří samostatné prohlášení prostor pro popisky. Názvy jsou zavedeny do tohoto prostoru deklarace prostřednictvím *labeled_statement*s a názvy jsou odkazovány prostřednictvím *goto_statement*s. ***Popisek místa deklarace*** bloku zahrnuje jakékoli vnořené bloky. Proto ve vnořeném bloku není možné deklarovat popisek se stejným názvem jako popisek z vnějšího bloku.

Textové pořadí, ve kterém jsou deklarovány názvy je obecně žádný význam. Zejména textové pořadí není důležité pro deklaraci a použití oborů názvů, konstanty, metody, vlastnosti, události, indexery, operátory, konstruktory instancí, destruktory, statické konstruktory a typy. Deklarace záleží na pořadí následujícími způsoby:

*  Pořadí deklarace pro pole deklarace a místní deklarace proměnné určuje pořadí, ve kterém jsou prováděna jejich inicializátory (pokud existuje).
*  Lokální proměnné musí být definovány před jejich použitím ([obory](basic-concepts.md#scopes)).
*  Deklarace pořadí deklarací členů výčtu ([členy výčtu](enums.md#enum-members)) je důležité, když *constant_expression* hodnoty jsou vynechány.

Místo deklarace oboru názvů je "Otevřít zakončeno" a dvěma obor názvů, které deklarace se stejným plně kvalifikovaným názvem přispívat na stejné místo prohlášení. Příklad
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

výše uvedené dvě deklarace oboru názvů přispívat na stejné místo prohlášení, v tomto případě deklarovat dvě třídy s plně kvalifikované názvy `Megacorp.Data.Customer` a `Megacorp.Data.Order`. Vzhledem k tomu, že dvě deklarace přispívat na stejné místo prohlášení, to by způsobila Chyba kompilace pokud každá obsahovala deklaraci třídy se stejným názvem.

Jak je uvedeno výše místo prohlášení bloku zahrnuje jakékoli vnořené bloky. Díky tomu se v následujícím příkladu `F` a `G` metody způsobit chybu kompilace, protože název `i` je deklarovaný ve vnějším bloku a nejde deklarovat ve vnitřním bloku. Ale `H` a `I` metod jsou platné od obou `i`společnosti jsou deklarovány v samostatných blocích-nested.

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

Obory názvů a typy mají ***členy***. Členy entity jsou obecně dostupné prostřednictvím úplný název, který začíná na odkaz na entitu, za nímž následuje "`.`" token, následovaný název člena.

Členy typu jsou buď deklarovány v deklaraci typu nebo ***zděděné*** ze základní třídy typu. Pokud typ dědí ze základní třídy, všechny členy základní třídy, s výjimkou instance konstruktory, destruktory a statické konstruktory stanou členy odvozeného typu. Je deklarovaná přístupnost člena základní třídy neřídí, zda člen zděděný – dědičnosti rozšiřuje do jakéhokoli členu, který není konstruktor instance, statický konstruktor nebo destruktor. Ale nemusí být přístupné v odvozeném typu, buď z důvodu jeho deklarovaná přístupnost zděděného člena ([deklarovaná přístupnost](basic-concepts.md#declared-accessibility)) nebo protože je skrytá deklarací v samotném typu ([skrytí prostřednictvím dědičnost](basic-concepts.md#hiding-through-inheritance)).

### <a name="namespace-members"></a>Namespace členy

Obory názvů a typy, které mají žádný nadřazený obor názvů jsou členy ***globální obor názvů***. To odpovídá přímo názvy deklarované v deklaraci globálního místa.

Obory názvů a typy deklarované v rámci oboru názvů jsou členy tohoto oboru názvů. To odpovídá přímo názvy deklarované v prostoru deklarace oboru názvů.

Obory názvů nemá žádné omezení přístupu. Není možné deklarovat privátní, chráněné nebo interní oborů názvů a názvy oborů názvů jsou vždy veřejně přístupná.

### <a name="struct-members"></a>Členy struktury

Členy struktury jsou členy deklarované ve struktuře a členy zděděné od přímé základní třídy struktura, obsahovat `System.ValueType` a nepřímá základní třída `object`.

Členové jednoduchý typ. odpovídají přímo na členy struktury alias typu pomocí jednoduchého typu:

*  Členové `sbyte` jsou členy `System.SByte` struktury.
*  Členové `byte` jsou členy `System.Byte` struktury.
*  Členové `short` jsou členy `System.Int16` struktury.
*  Členové `ushort` jsou členy `System.UInt16` struktury.
*  Členové `int` jsou členy `System.Int32` struktury.
*  Členové `uint` jsou členy `System.UInt32` struktury.
*  Členové `long` jsou členy `System.Int64` struktury.
*  Členové `ulong` jsou členy `System.UInt64` struktury.
*  Členové `char` jsou členy `System.Char` struktury.
*  Členové `float` jsou členy `System.Single` struktury.
*  Členové `double` jsou členy `System.Double` struktury.
*  Členové `decimal` jsou členy `System.Decimal` struktury.
*  Členové `bool` jsou členy `System.Boolean` struktury.

### <a name="enumeration-members"></a>Členy výčtu

Členy výčtu jsou deklarovány ve výčtu konstant a členy zděděné od přímé základní třídy výčtu `System.Enum` a nepřímé třídy base `System.ValueType` a `object`.

### <a name="class-members"></a>Členy třídy

Členy třídy jsou členy deklarované ve třídě a členy zděděné ze základní třídy (s výjimkou třídy `object` který nemá žádnou základní třídu). Členy zděděné ze základní třídy zahrnují konstanty, pole, metody, vlastnosti, události, indexery, operátory a typy základní třídy, ale není instance konstruktory, destruktory a statické konstruktory základní třídy. Bez ohledu na jejich přístupnost jsou zděděné členy základní třídy.

Deklarace třídy mohou obsahovat deklarace konstanty, pole, metody, vlastnosti, události, indexery, operátory, konstruktory instancí, destruktory, statické konstruktory a typy.

Členové `object` a `string` odpovídají přímo členy typů tříd, alias:

*  Členové `object` jsou členy `System.Object` třídy.
*  Členové `string` jsou členy `System.String` třídy.

### <a name="interface-members"></a>Členy rozhraní

Členy rozhraní jsou členy deklarované v rozhraní a ve všech základních rozhraní rozhraní. Členy ve třídě `object` nejsou, přísně vzato členy libovolné rozhraní ([členy rozhraní](interfaces.md#interface-members)). Ale členy ve třídě `object` jsou k dispozici prostřednictvím člen vyhledávání v libovolný typ rozhraní ([člen vyhledávání](expressions.md#member-lookup)).

### <a name="array-members"></a>Členy pole

Členy pole jsou členy zděděné z třídy `System.Array`.

### <a name="delegate-members"></a>Delegát členy

Členové delegáta jsou členy zděděné z třídy `System.Delegate`.

## <a name="member-access"></a>Přístup ke členům

Deklarace členů umožňují řídit přístup ke členu. Usnadnění přístupu člena je vytvořeno je deklarovaná přístupnost ([deklarovaná přístupnost](basic-concepts.md#declared-accessibility)) člena kombinovat s přístupnosti bezprostředně nadřazeného typu, pokud existuje.

Když je povolen přístup k určitým členem, člen se říká, že ***přístupné***. Naopak při přístupu k určitým členem je zakázané, člen se říká, že ***nepřístupný***. Je povolen přístup ke členovi textové umístění, ve kterém probíhá přístup je obsažen v tak doména přístupnosti ([usnadnění domén](basic-concepts.md#accessibility-domains)) člena.

### <a name="declared-accessibility"></a>Deklarovaná přístupnost

***Deklarovaná přístupnost*** člena může být jedna z následujících akcí:

*  Public, který je vybrán zahrnutím `public` modifikátor v deklaraci člena. Intuitivní význam `public` je "přístup mimo jiné".
*  Chráněný, což je vybraná zahrnutím `protected` modifikátor v deklaraci člena. Intuitivní význam `protected` je "přístup omezen na obsahující třídu nebo typy odvozené ze třídy obsahující".
*  Interní, který je vybrán zahrnutím `internal` modifikátor v deklaraci člena. Intuitivní význam `internal` je "přístup omezen na tento program".
*  Chráněné vnitřní (tj. chráněné nebo interní), která se zvolila včetně `protected` a `internal` modifikátor v deklaraci člena. Intuitivní význam `protected internal` "přístup omezen na tento program nebo typy odvozené od třídy obsahující".
*  Privátní, který je vybrán zahrnutím `private` modifikátor v deklaraci člena. Intuitivní význam `private` je "přístup omezen na obsahující typ".

V závislosti na kontextu, ve kterém přijímá deklarace člena umístíte, jsou povoleny pouze určité typy deklarovaná přístupnost. Kromě toho při deklaraci členské neobsahuje žádné modifikátory přístupu, kontext, ve kterém probíhá deklarace Určuje výchozí deklarovaná přístupnost.

*  Obory názvů mají implicitně `public` deklarovaná přístupnost. Deklarace oboru názvů jsou povoleny žádné modifikátory přístupu.
*  Typy deklarované v jednotkách kompilace nebo obory názvů můžou mít `public` nebo `internal` deklarované usnadnění přístupu a výchozí `internal` deklarovaná přístupnost.
*  Členy třídy lze mít pět typů deklarovaná přístupnost a ve výchozím nastavení `private` deklarovaná přístupnost. (Všimněte si, že typ deklarované jako člen třídy, může mít některý z pěti typů deklarovaná přístupnost, že typ deklarovaný s členem oboru názvů můžou mít jenom `public` nebo `internal` deklarovaná přístupnost.)
*  Členy struktury mohou mít `public`, `internal`, nebo `private` deklarované usnadnění přístupu a výchozí `private` deklaraci přístupnost, protože jsou implicitně zapečetěné struktury. Členy struktury zavedený – struktura (který je, nejsou děděna této struktury) nemůže mít `protected` nebo `protected internal` deklarovaná přístupnost. (Všimněte si, že typ deklarovaný jako člen struktury mohou mít `public`, `internal`, nebo `private` deklarovaná přístupnost, že typ deklarovaný s členem oboru názvů můžou mít jenom `public` nebo `internal` deklarovaná přístupnost.)
*  Členy rozhraní mají implicitně `public` deklarovaná přístupnost. Deklarace člena rozhraní jsou povoleny žádné modifikátory přístupu.
*  Členy výčtu mají implicitně `public` deklarovaná přístupnost. U člena deklarací výčtu jsou povoleny žádné modifikátory přístupu.

### <a name="accessibility-domains"></a>Usnadnění domén

***Doména přístupnosti*** člena se skládá z (pravděpodobně nesouvislý) části textu programu, ve kterém je povolen přístup ke členu. Pro účely definování tak doména přístupnosti člena, je označen jako člen ***nejvyšší úrovně*** Pokud není deklarován v rámci typu, a je označen jako člen ***vnořené*** případě, že je deklarována v rámci jiného typu. Kromě toho ***programu text*** programu je definován tak, že všechny programu text obsažen ve všech zdrojových souborech programu a textem programu typ je definován tak, že všechny obsažené v textu programu *type_declaration*s tohoto typu (včetně, případně také typy, které jsou vnořené v rámci typu).

Doména přístupnosti člena předdefinovaný typ (například `object`, `int`, nebo `double`) neomezený.

Doména přístupnosti člena na nejvyšší úrovni nevázaný typ `T` ([vázaný a nevázaných typy](types.md#bound-and-unbound-types)), který je deklarován v programu `P` je definovaná následujícím způsobem:

*  Pokud je deklarovaná přístupnost člena `T` je `public`, tak doména přístupnosti člena `T` je text programu `P` a libovolného programu, který odkazuje na `P`.
*  Pokud je deklarovaná přístupnost člena `T` je `internal`, tak doména přístupnosti člena `T` je text programu `P`.

Z těchto definic následuje, tak doména přístupnosti člena nejvyšší úrovně nevázaný typ je vždy alespoň textem programu program, který typ je deklarován.

Doména přístupnosti pro konstruovaný typ. `T<A1, ..., An>` je průsečík domény přístupnosti typu nevázaný parametr generického typu `T` a usnadnění domény argumentů typu `A1, ..., An`.

Doména přístupnosti vnořeného člena `M` deklarovaného v typu `T` v rámci programu `P` je definována takto (konstatujme, že `M` pravděpodobně sám může být typ):

*  Pokud je deklarovaná přístupnost člena `M` je `public`, tak doména přístupnosti člena `M` je doména přístupnosti člena `T`.
*  Pokud je deklarovaná přístupnost člena `M` je `protected internal`, umožněte `D` být sjednocení textem programu `P` a textem programu libovolného typu odvozené z `T`, který je deklarován mimo `P`. Doména přístupnosti člena `M` je průsečík domény přístupnosti typu `T` s `D`.
*  Pokud je deklarovaná přístupnost člena `M` je `protected`, umožněte `D` být sjednocení textem programu `T` a textem programu libovolného typu odvozené z `T`. Doména přístupnosti člena `M` je průsečík domény přístupnosti typu `T` s `D`.
*  Pokud je deklarovaná přístupnost člena `M` je `internal`, tak doména přístupnosti člena `M` je průsečík domény přístupnosti typu `T` s textem programu `P`.
*  Pokud je deklarovaná přístupnost člena `M` je `private`, tak doména přístupnosti člena `M` je text programu `T`.

Z tyto definice vyplývá, že doména přístupnosti vnořeného člena je vždy alespoň textem programu typu, ve kterém člen je deklarovaný. Kromě toho vyplývá, tak doména přístupnosti člena se nikdy výstižnějších než doména přístupnosti typu, ve kterém člen je deklarovaný.

Intuitivní řečeno pokud typ nebo člen `M` je přístup, následující kroky jsou vyhodnoceny Ujistěte se, že je povolen přístup:

*  První, pokud `M` je deklarována v rámci typu (oproti kompilační jednotky nebo oboru názvů), Chyba kompilace nastane, pokud tento typ není dostupný.
*  Když se poté `M` je `public`, je povolen přístup.
*  Jinak, pokud `M` je `protected internal`, přístup je povolený, pokud se vyskytuje v programu, ve kterém `M` je teď deklarována, nebo pokud dojde k v rámci třídy odvozené z třídy, ve které `M` je deklarovaný a probíhá prostřednictvím odvozené Typ třídy ([chráněného přístupu pro instanci členy](basic-concepts.md#protected-access-for-instance-members)).
*  Jinak, pokud `M` je `protected`, přístup je povolený, pokud k němu dojde v rámci třídy, ve které `M` je teď deklarována, nebo pokud dojde k v rámci třídy odvozené z třídy, ve které `M` je deklarovaný a probíhá prostřednictvím odvozené Typ třídy ([chráněného přístupu pro instanci členy](basic-concepts.md#protected-access-for-instance-members)).
*  Jinak, pokud `M` je `internal`, pokud se vyskytuje v programu, ve kterém smí obsahovat přístup `M` je deklarována.
*  Jinak, pokud `M` je `private`, pokud k němu dojde v rámci typu, ve kterém smí obsahovat přístup `M` je deklarována.
*  V opačném případě tento typ nebo člen je přístupný a dojde k chybě kompilace.

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
třídy a členy mají následující usnadnění domén:

*  Doména přístupnosti člena `A` a `A.X` neomezený.
*  Doména přístupnosti člena `A.Y`, `B`, `B.X`, `B.Y`, `B.C`, `B.C.X`, a `B.C.Y` je text programu obsahující programu.
*  Doména přístupnosti člena `A.Z` je text programu `A`.
*  Doména přístupnosti člena `B.Z` a `B.D` je text programu `B`, včetně textem programu `B.C` a `B.D`.
*  Doména přístupnosti člena `B.C.Z` je text programu `B.C`.
*  Doména přístupnosti člena `B.D.X` a `B.D.Y` je text programu `B`, včetně textem programu `B.C` a `B.D`.
*  Doména přístupnosti člena `B.D.Z` je text programu `B.D`.

Jak ukazuje příklad, tak doména přístupnosti člena se nikdy větší než u nadřazeného typu. Například i když všechny `X` členové mají veřejné deklarovaná přístupnost, vše kromě `A.X` usnadnění domén, které jsou omezeny nadřazeného typu.

Jak je popsáno v [členy](basic-concepts.md#members), odvozené typy dědí všechny členy základní třídy, s výjimkou pro instanci konstruktory, destruktory a statické konstruktory. To zahrnuje i soukromé členy základní třídy. Doména přístupnosti člena soukromý člen ale obsahuje pouze textem programu typu, ve kterém člen je deklarovaný. V příkladu
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
`B` třída dědí soukromému členu `x` z `A` třídy. Protože je privátní člen, je dostupná jenom v rámci *class_body* z `A`. Proto přístup k `b.x` orientovaný `A.F` metody, ale selže v `B.F` metody.

### <a name="protected-access-for-instance-members"></a>Chráněného přístupu pro členy instance

Při `protected` člena instance je přistupováno mimo textem programu třídy, ve kterém je deklarována, a kdy `protected internal` člena instance je přístupná mimo textem programu program, ve kterém je deklarována, přístup musí proběhnout v rámci deklarace třídy, která je odvozena z třídy, ve kterém je deklarována. Kromě toho přístup je potřeba provést prostřednictvím instance typu odvozené třídy nebo typu třídy zkonstruovat z něj. Toto omezení zabraňuje v přístupu k chráněným členům jiné odvozené třídy i v případě, že členové jsou zděděny ze stejné základní třídy jedné odvozené třídy.

Umožní `B` být základní třídu, která deklaruje člen chráněnou instanci `M`a nechat `D` být třída odvozená z `B`. V rámci *class_body* z `D`, přístup k `M` můžete provést jednu z následujících forem:

*  Neúplného *type_name* nebo *primary_expression* formuláře `M`.
*  A *primary_expression* formuláře `E.M`, zadaný typ `E` je `T` nebo třída odvozená z `T`, kde `T` je typ třídy `D`, nebo typ třídy. zkonstruovat z `D`
*  A *primary_expression* formuláře `base.M`.

Kromě těchto forem přístup odvozené třídy přístup k chráněné instance konstruktoru základní třídy v *constructor_initializer* ([inicializátory konstruktoru](classes.md#constructor-initializers)).

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
v rámci `A`, je možné přístup `x` prostřednictvím instance obou `A` a `B`, protože v obou případech přístup probíhá přes instanci `A` nebo třída odvozená z `A`. Ale v rámci `B`, není možné přistupovat k `x` prostřednictvím instance `A`, protože `A` není odvozen od `B`.

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
tři přiřazení `x` se nepovoluje, protože všechny probíhat přes instance typů tříd, které jsou zhotoveny z obecného typu.

### <a name="accessibility-constraints"></a>Omezení přístupnosti

Několik konstrukce v jazyce C# vyžaduje typ být ***přinejmenším stejně dostupná jako*** člena nebo jiného typu. Typ `T` říká, že je dostupná jako člen nebo typ `M` Pokud doména přístupnosti člena `T` je nadstavbou jazyka tak doména přístupnosti člena `M`. Jinými slovy `T` je přinejmenším stejně dostupná jako `M` Pokud `T` je dostupný ve všech kontextech, ve kterém `M` přístupný.

Existují následující omezení pro usnadnění:

*  Přímou základní třídu typu třídy musí být přinejmenším stejně dostupná jako typ třídy.
*  Explicitní základní rozhraní typu rozhraní musí být přinejmenším stejně dostupná jako samotného typu rozhraní.
*  Návratový typ a typy parametrů typu delegáta musí být přinejmenším stejně dostupná jako samotného typu delegáta.
*  Typ konstanty musí být přinejmenším stejně dostupná jako konstanta samotný.
*  Typ pole musí být přinejmenším stejně dostupná jako vlastní pole.
*  Návratový typ a typy parametrů metody musí být přinejmenším stejně dostupná jako metoda sama.
*  Typ vlastnosti musí být přinejmenším stejně dostupná jako samotné vlastnosti.
*  Typ události musí být přinejmenším stejně dostupná jako samotné události.
*  Typ a parametrem typy indexer musí být přinejmenším stejně dostupná jako samotný indexeru.
*  Návratový typ a typy parametrů operátoru musí být přinejmenším stejně dostupná jako operátor.
*  Typy parametrů konstruktoru instance musí být přinejmenším stejně dostupná jako konstruktor instance, samotného.

V příkladu
```csharp
class A {...}

public class B: A {...}
```
`B` třídy výsledkem chyba kompilace, protože `A` není dostupné jako `B`.

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
`H` metoda `B` výsledkem chyba kompilace, protože návratový typ `A` není dostupný jako metodu.

## <a name="signatures-and-overloading"></a>Podpisy a přetížení

Metody, konstruktory instancí, indexery a operátory jsou charakteristické jejich ***podpisy***:

*  Podpis metody se skládá z názvu metody, počet parametrů typu a typ a typ (hodnota, odkaz nebo výstup) každý z jejích formálních parametrů v pořadí zleva doprava. Pro tyto účely je identifikován libovolný typ parametru metody, nacházející se v typu formálního parametru není podle názvu, ale podle jeho pořadové číslo pozice v seznamu argumentů typu metody. Podpis metody konkrétně nezahrnuje návratovým typem, `params` modifikátor, který může být zadáno pro parametr úplně vpravo ani omezení parametru typu volitelné.
*  Podpis konstruktoru instance se skládá z typu a druh (hodnota, odkaz nebo výstup) každý z jejích formálních parametrů v pořadí zleva doprava. Podpis konstruktoru instance výslovně nezahrnete `params` modifikátor, který může být zadáno pro parametr úplně vpravo.
*  Podpis indexeru se skládá z typu každý z jejích formálních parametrů v pořadí zleva doprava. Podpis indexer konkrétně neobsahuje typ elementu ani obsahuje `params` modifikátor, který může být zadáno pro parametr úplně vpravo.
*  Podpis operátora se skládá z názvu operátoru a typu každý z jejích formálních parametrů v pořadí zleva doprava. Podpis operátora konkrétně neobsahuje typ výsledku.

Podpisy jsou povolení mechanismus pro ***přetížení*** členů třídy, struktury a rozhraní:

*  Přetěžování metod povoluje třídy, struktury nebo rozhraní, chcete-li deklarovat více metod se stejným názvem, poskytuje jejich podpisy musí být jedinečné v rámci této třídy, struktury nebo rozhraní.
*  Přetížení konstruktory instancí umožňuje třídě nebo struktuře, chcete-li deklarovat více instančních konstruktorech, pokud jejich podpisy jsou jedinečné v rámci této třídy nebo struktury.
*  Přetížení indexerů umožňuje třídy, struktury nebo rozhraní, chcete-li deklarovat několik indexerů, pokud jejich podpisy jsou jedinečné v rámci této třídy, struktury nebo rozhraní.
*  Přetížení operátorů umožňuje třídě nebo struktuře, chcete-li deklarovat více operátorů se stejným názvem, poskytuje jejich podpisy musí být jedinečné v rámci této třídy nebo struktury.

I když `out` a `ref` modifikátorech parametrů jsou považovány za součást podpisu, členy deklarované v jeden typ se nemůže lišit v podpisu výhradně nástrojem `ref` a `out`. Chyba kompilace nastane, pokud dva členy jsou deklarovány v rámci stejného typu s podpisy, které bude stejný, když všechny parametry v obou metodách s `out` modifikátory byly změněny na `ref` modifikátory. Pro jiné účely odpovídajících podpis (například skryjete nebo přepsání), `ref` a `out` jsou považovány za součást podpisu a navzájem neodpovídají. (Toto omezení je, aby byl C# programům snadno přeložit a spusťte na běžné Language infrastruktury (CLI), která neposkytuje způsob, jak definovat metody, které se liší pouze v `ref` a `out`.)

Pro účely podpisy, typy `object` a `dynamic` jsou považovány za totéž. Členy deklarované v jeden typ nelze proto se liší v podpisu výhradně nástrojem `object` a `dynamic`.

Následující příklad ukazuje sadu deklarace přetížené metody společně s jejich podpisy.
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

Všimněte si, že všechny `ref` a `out` modifikátorech parametrů ([parametry metody](classes.md#method-parameters)) jsou součástí podpis. Proto `F(int)` a `F(ref int)` jsou jedinečné podpisy. Ale `F(ref int)` a `F(out int)` nelze deklarovat v rámci stejné rozhraní, protože se liší jejich podpisy výhradně nástrojem `ref` a `out`. Všimněte si také, aby návratový typ a `params` modifikátor nejsou součástí podpis, takže ho není možné přetížení založené výhradně na návratový typ nebo zahrnutí nebo vyloučení `params` modifikátor. Jako takové deklarace metody `F(int)` a `F(params string[])` určené výše vyústí v chybu kompilace.

## <a name="scopes"></a>Obory

***Oboru*** názvu je oblast textu programu, ve kterém je možné k odkazování na entity deklarované podle názvu bez kvalifikace názvu. Může být obory ***vnořené***, a vnitřní obor může předeklarovat význam názvu z vnějšího oboru (to ale neodebere omezení stanovené [deklarace](basic-concepts.md#declarations) , že ve vnořeném bloku není možná pro deklarování místní proměnné se stejným názvem jako místní proměnnou v nadřízeném bloku). Název z vnějšího oboru je pak říká, že ***skryté*** v oblasti program text vztahuje na vnitřní a přístup k vnější název je možné jenom ručním kvalifikováním názvu.

*  Obor členem oboru názvů deklarována *namespace_member_declaration* ([Namespace členy](namespaces.md#namespace-members)) s žádný nadřazený *namespace_declaration* je celého programu text.
*  Obor členem oboru názvů deklarována *namespace_member_declaration* v rámci *namespace_declaration* jehož plně kvalifikovaný název je `N` je *namespace_body*  z každé *namespace_declaration* jehož plně kvalifikovaný název je `N` nebo začíná `N`, následované tečkou.
*  Obor názvu určené *extern_alias_directive* rozloženo *using_directive*s, *global_attributes* a *namespace_member_ deklarace*s jeho bezprostředně nadřazeného kompilace částí nebo těle oboru názvů. *Extern_alias_directive* není přispívat všemi novými členy do prostoru základní deklarace. Jinými slovy *extern_alias_directive* není přenosné, ale místo toho ovlivňuje pouze kompilaci jednotky nebo oboru názvů text ve kterém se vyskytuje.
*  Obor názvu definován ani importován podle *using_directive* ([direktiv Using](namespaces.md#using-directives)) rozšiřuje přes *namespace_member_declaration*s  *compilation_unit* nebo *namespace_body* ve kterém *using_directive* vyvolá. A *using_directive* zpřístupnit nula nebo více názvů, typ nebo člen názvy v rámci konkrétní *compilation_unit* nebo *namespace_body*, ale ne přispívat všemi novými členy do prostoru základní deklarace. Jinými slovy *using_directive* není přenosné, ale místo toho má vliv pouze *compilation_unit* nebo *namespace_body* ve kterém se vyskytuje.
*  Obor parametr typu deklarován *type_parameter_list* na *class_declaration* ([třídy deklarací](classes.md#class-declarations)) je *class_base*, *type_parameter_constraints_clause*s, a *class_body* tohoto *class_declaration*.
*  Obor parametr typu deklarován *type_parameter_list* na *struct_declaration* ([deklarace struktury](structs.md#struct-declarations)) je *struct_interfaces* , *type_parameter_constraints_clause*s, a *struct_body* tohoto *struct_declaration*.
*  Obor parametr typu deklarován *type_parameter_list* na *interface_declaration* ([rozhraní deklarace](interfaces.md#interface-declarations)) je *interface_base* , *type_parameter_constraints_clause*s, a *interface_body* tohoto *interface_declaration*.
*  Obor parametr typu deklarován *type_parameter_list* na *delegate_declaration* ([delegovat deklarace](delegates.md#delegate-declarations)) je *typ*, *formal_parameter_list*, a *type_parameter_constraints_clause*s, který *delegate_declaration*.
*  Obor člen deklarován *class_member_declaration* ([tělo třídy](classes.md#class-body)) je *class_body* ve kterém dochází k deklaraci. Kromě toho oboru členu třídy rozšiřuje na *class_body* z nich odvozené třídy, které jsou součástí tak doména přístupnosti ([usnadnění domén](basic-concepts.md#accessibility-domains)) člena.
*  Obor člen deklarován *struct_member_declaration* ([členy struktury](structs.md#struct-members)) je *struct_body* ve kterém dochází k deklaraci.
*  Obor člen deklarován *enum_member_declaration* ([členy výčtu](enums.md#enum-members)) je *enum_body* ve kterém dochází k deklaraci.
*  Obor parametr deklarovaný v *method_declaration* ([metody](classes.md#methods)) je *method_body* tohoto *method_declaration*.
*  Obor parametr deklarovaný v *indexer_declaration* ([indexery](classes.md#indexers)) je *accessor_declarations* tohoto *indexer_declaration*.
*  Obor parametr deklarovaný v *operator_declaration* ([operátory](classes.md#operators)) je *bloku* tohoto *operator_declaration*.
*  Obor parametr deklarovaný v *constructor_declaration* ([Instance konstruktory](classes.md#instance-constructors)) je *constructor_initializer* a *bloku* tohoto *constructor_declaration*.
*  Obor parametr deklarovaný v *lambda_expression* ([výrazy anonymní funkce](expressions.md#anonymous-function-expressions)) je *anonymous_function_body* tohoto *lambda_ výraz*
*  Obor parametr deklarovaný v *anonymous_method_expression* ([výrazy anonymní funkce](expressions.md#anonymous-function-expressions)) je *bloku* tohoto *anonymous_method _expression*.
*  Obor popisku deklarované v *labeled_statement* ([příkazy s popiskem](statements.md#labeled-statements)) je *bloku* ve kterém dochází k deklaraci.
*  Lokální proměnná deklarovaná v rozsahu *local_variable_declaration* ([místní deklarace proměnné](statements.md#local-variable-declarations)) je blok ve kterém dochází k deklaraci.
*  Lokální proměnná deklarovaná v rozsahu *switch_block* z `switch` – příkaz ([příkazu switch](statements.md#the-switch-statement)) je *switch_block*.
*  Lokální proměnná deklarovaná v rozsahu *for_initializer* z `for` – příkaz ([pro příkaz](statements.md#the-for-statement)) je *for_initializer*,  *for_condition*, *for_iterator*a uzavřeného *příkaz* z `for` příkazu.
*  Obor lokální konstanta deklarované v *local_constant_declaration* ([místní deklarace konstantní](statements.md#local-constant-declarations)) je blok ve kterém dochází k deklaraci. Jde chybu v době kompilace k odkazování na lokální konstanta v textové pozici, která předchází jeho *constant_declarator*.
*  Obor proměnné deklarované jako součást *foreach_statement*, *using_statement*, *lock_statement* nebo *query_expression* je Určuje rozšíření dané konstrukce.

V rámci oboru názvů, třídy, struktury nebo výčtu člen je možné odkazovat na člen v textové pozici, která předchází deklarace člena. Příklad
```csharp
class A
{
    void F() {
        i = 1;
    }

    int i = 0;
}
```
Tady je platný pro `F` k odkazování na `i` dříve, než je deklarována.

V rámci oboru místní proměnná, je chyba kompilace k odkazování na místní proměnnou v textové pozici, která předchází *local_variable_declarator* lokální proměnné. Příklad
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

V `F` výše uvedené metody, první přiřazení k `i` konkrétně neodkazuje na pole deklarovaná ve vnějším oboru. Místo toho se odkazuje na místní proměnné a je výsledkem chyba kompilace, protože textový předchází deklarace proměnné. V `G` metody, použití `j` v inicializátoru pro deklaraci `j` je platný, protože použití nepředchází *local_variable_declarator*. V `H` metody, následné *local_variable_declarator* správně odkazuje na místní proměnná deklarovaná v předchozím kroku *local_variable_declarator* v rámci stejného  *local_variable_declaration*.

Pravidla oboru místních proměnných slouží k zajištění, že význam názvu použít v kontextu výrazu je vždy stejné v rámci bloku. Pokud obor lokální proměnné můžete rozšířit jenom z jeho deklarace do konce bloku, v předchozím příkladu by první přiřazení přiřadit k proměnné instance a druhé přiřazení by přiřadit místní proměnnou, by mohl vést k chyby při kompilaci šlo novější změnu řazení příkazy bloku.

Význam názvu v rámci bloku se může lišit v závislosti na kontextu, ve kterém se používá název. V příkladu
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
název `A` se používá v kontextu výrazu k odkazování na místní proměnnou `A` a v kontextu typu odkazovat na třídu `A`.

### <a name="name-hiding"></a>Skrytí názvu

Obor entity obvykle zahrnuje další textem programu místa, než je deklarace entity. Zejména oboru entita může obsahovat deklarace, které zavádějí nové prostory deklarace obsahující entity se stejným názvem. Takové deklarace způsobit subjektem původní stane ***skryté***. Naopak entity se říká, že ***viditelné*** Pokud není skrytý.

Skrývání názvů nastane, pokud obory překrývat prostřednictvím vnoření a kdy obory překrývat prostřednictvím dědičnosti. Vlastnosti dva druhy skrytí jsou popsány v následujících částech.

#### <a name="hiding-through-nesting"></a>Skrytí prostřednictvím vnoření

Skrytí názvu prostřednictvím vnoření může nastat v důsledku vnořené obory názvů nebo typy v oborech názvů, v důsledku vnořené typy v rámci třídy nebo struktury a v důsledku parametr a místní deklarace proměnné.

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
v rámci `F` proměnná instance, metoda `i` je skrytý místní proměnná `i`, ale v rámci `G` metody `i` stále odkazuje na proměnnou instance.

Název ve vnitřním oboru je skryt název ve vnějším oboru, skryje všechny přetížené výskyty s tímto názvem. V příkladu
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
volání `F(1)` vyvolá `F` deklarované v `Inner` protože všechny vnější výskyty `F` skryty pomocí vnitřní deklarace. Ze stejného důvodu volání `F("Hello")` výsledkem chyba kompilace.

#### <a name="hiding-through-inheritance"></a>Skrytí prostřednictvím dědičnosti

Skrytí názvu prostřednictvím dědičnosti vyvolá se v případě třídy nebo struktury předeklarovat názvy, které byly zděděny ze základních tříd. Tento typ skrývání názvů má jednu z následujících forem:

*  – Konstanta, pole, vlastnosti, události nebo typ byl zaveden ve třídě nebo struktuře skryje všechny členy základní třídy se stejným názvem.
*  Metody zavedené ve třídě nebo struktuře skryje všechny členy jiné metody základní třídy se stejným názvem a všechny metody základní třídy se stejným podpisem (název metody a počet parametrů, modifikátory a typy).
*  Indexer zavedený ve třídě nebo struktuře skryje všechny základní třídy indexerů se stejným podpisem (počet parametrů a typů).

Pravidla pro deklarace operátoru ([operátory](classes.md#operators)) znemožnit odvozené třídy za účelem deklarovat operátor se stejným podpisem jako operátor v základní třídě. Proto operátory nikdy skrýt mezi sebou.

Rozporu s skrytí názvu z vnějšího oboru, skrytí přístupový název ze zděděných oboru způsobí, že upozornění, aby oznámený. V příkladu
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
deklarace `F` v `Derived` způsobí, že aby oznámený upozornění. Skrytí zděděného název není výslovně chybu, protože, který bude bránit samostatný vývoj základních tříd. Například výše situace může mít přijít, protože na novější verzi `Base` zavedená `F` metodu, která nebyla k dispozici ve starší verzi této třídy. Výše uvedené situace je chyba, pak všechny změny provedené na základní třídu v knihovně tříd samostatně vyvíjených může potenciálně způsobit odvozené třídy stanou neplatnými.

Toto upozornění způsobeno skrytí zděděného název lze odstranit pomocí `new` modifikátor:
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

`new` Modifikátor znamená, že `F` v `Derived` je "nové", a je ve skutečnosti určen ke skrytí zděděného člena.

Deklarace nový člen skrývá zděděný člen pouze v rámci oboru nového člena.

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

V příkladu výše, deklarace `F` v `Derived` skryje `F` , který byl zděděn od `Base`, ale od nové `F` v `Derived` má privátní přístup, jeho rozsah nerozšiřuje `MoreDerived` . Proto volání `F()` v `MoreDerived.G` je platný a vyvolá `Base.F`.

## <a name="namespace-and-type-names"></a>Namespace a zadejte názvy

V několika kontextech C# program vyžadovat *namespace_name* nebo *type_name* zadání.

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

A *namespace_name* je *namespace_or_type_name* , který odkazuje na obor názvů. Po řešení, jak je popsáno níže, *namespace_or_type_name* z *namespace_name* musí odkazovat na názvový prostor, nebo v opačném případě dojde k chybě kompilace. Žádné argumenty typu ([argumenty typu](types.md#type-arguments)) mohou být přítomny v *namespace_name* (pouze pro typy mohou mít argumenty typu).

A *type_name* je *namespace_or_type_name* odkazující k typu. Po řešení, jak je popsáno níže, *namespace_or_type_name* z *type_name* musí odkazovat na typ, nebo v opačném případě dojde k chybě kompilace.

Pokud *namespace_or_type_name* kvalifikovaný alias-členem jeho význam se, jak je popsáno v [Namespace alias kvalifikátory](namespaces.md#namespace-alias-qualifiers). V opačném případě *namespace_or_type_name* obsahuje jednu ze čtyř podob:

*  `I`
*  `I<A1, ..., Ak>`
*  `N.I`
*  `N.I<A1, ..., Ak>`

kde `I` je jediným identifikátorem `N` je *namespace_or_type_name* a `<A1, ..., Ak>` je volitelný *type_argument_list*. Pokud ne *type_argument_list* je zadán, vezměte v úvahu `k` být nula.

Význam *namespace_or_type_name* je stanoven následujícím způsobem:

*   Pokud *namespace_or_type_name* má formu `I` nebo formuláře `I<A1, ..., Ak>`:
    * Pokud `K` je nula a *namespace_or_type_name* se zobrazí v deklaraci obecné metody ([metody](classes.md#methods)) a pokud tato deklarace obsahuje parametr typu ([typu Parametry](classes.md#type-parameters)) s názvem `I`, pak bude *namespace_or_type_name* odkazuje na parametr typu.
    * Jinak, pokud *namespace_or_type_name* se zobrazí v rámci deklarace typu a potom pro každý typ instance `T` ([typ instance](classes.md#the-instance-type)) začíná s typem instance daného typu deklarace a budete pokračovat s typem instance deklaraci nadřazené třídu nebo strukturu (pokud existuje):
        * Pokud `K` je nula a deklarace `T` obsahuje parametr typu s názvem `I`, pak bude *namespace_or_type_name* odkazuje na parametr typu.
        * Jinak, pokud *namespace_or_type_name* se zobrazí v těle deklarace typu a `T` nebo jakýkoli z jeho základních typů obsahuje vnořené přístupné typu s názvem `I` a `K`  parametry typu, pak bude *namespace_or_type_name* odkazuje na tento typ vytvořený s argumenty daného typu. Pokud existuje více než jeden takový typ, je vybraný typ deklarovaný v rámci více odvozeného typu. Všimněte si, že členové bez typu (konstanty, pole, metody, vlastnosti, indexery, operátory, konstruktory instancí, destruktory a statické konstruktory) a členy typů s různým počtem parametrů typu jsou ignorovány při určování význam *namespace_or_type_name*.
    * Pokud byly úspěšné, pak pro každý obor názvů v předchozích krocích `N`začíná s oborem názvů, ve kterém *namespace_or_type_name* dojde, pokračujte v každé nadřazeného oboru názvů (pokud existuje) a končící globální obor názvů následující kroky jsou vyhodnocen, dokud se entita nachází:
        * Pokud `K` je nula a `I` je název oboru názvů v `N`, pak:
            * Pokud umístění ve kterém *namespace_or_type_name* dojde k není uzavřen v deklaraci oboru názvů pro `N` a obsahuje deklaraci oboru názvů *extern_alias_directive* nebo *using_alias_directive* , která přidruží název `I` s oborem názvů nebo typ, pak bude *namespace_or_type_name* je nejednoznačný a dojde k chybě kompilace.
            * V opačném případě *namespace_or_type_name* odkazuje na obor názvů s názvem `I` v `N`.
        * Jinak, pokud `N` obsahuje přístupného typu s názvem `I` a `K`  parametry typu, pak:
            * Pokud `K` je nula a umístění, kde *namespace_or_type_name* dojde k není uzavřen v deklaraci oboru názvů pro `N` a obsahuje deklaraci oboru názvů *extern_alias_directive*  nebo *using_alias_directive* , která přidruží název `I` s oborem názvů nebo typ, pak bude *namespace_or_type_name* je nejednoznačný a kompilace dojde k chybě.
            * V opačném případě *namespace_or_type_name* odkazuje na typ vytvořený s argumenty daného typu.
        * Jinak, pokud umístění ve kterém *namespace_or_type_name* dojde k není uzavřen v deklaraci oboru názvů pro `N`:
            * Pokud `K` je nula a obsahuje deklaraci oboru názvů *extern_alias_directive* nebo *using_alias_directive* , která přidruží název `I` s importovaným oborem názvů nebo typ, pak bude *namespace_or_type_name* odkazuje na tento obor názvů nebo typ.
            * Jinak, pokud deklarace oborů názvů a typ importované tímto seznamem *using_namespace_directive*s a *using_alias_directive*s deklarace oboru názvů obsahovat přesně jeden přístupný typ. s názvem `I` a `K`  parametry typu, pak bude *namespace_or_type_name* odkazuje na tento typ vytvořený s argumenty daného typu.
            * Jinak, pokud deklarace oborů názvů a typ importované tímto seznamem *using_namespace_directive*s a *using_alias_directive*s deklarace oboru názvů obsahovat více než jeden přístupný typ. s názvem `I` a `K`  parametry typu, pak bude *namespace_or_type_name* je nejednoznačný a dojde k chybě.
    * V opačném případě *namespace_or_type_name* je nedefinovaný a dojde k chybě kompilace.
*  V opačném případě *namespace_or_type_name* má formu `N.I` nebo formuláře `N.I<A1, ..., Ak>`. `N` jako první vyřeší *namespace_or_type_name*. Pokud se rozlišení `N` neproběhne úspěšně, dojde k chybě kompilace. V opačném případě `N.I` nebo `N.I<A1, ..., Ak>` vyřešen následujícím způsobem:
    * Pokud `K` je nula a `N` odkazuje na obor názvů a `N` obsahuje vnořené oboru názvů s názvem `I`, pak bude *namespace_or_type_name* odkazuje na tomto vnořené oboru názvů.
    * Jinak, pokud `N` odkazuje na obor názvů a `N` obsahuje přístupného typu s názvem `I` a `K`  parametry typu, pak bude *namespace_or_type_name* odkazuje k danému typu s argumenty daného typu.
    * Jinak, pokud `N` odkazuje na typ třídy nebo struktury (pravděpodobně konstruovaný) a `N` nebo některý z jeho základních tříd obsahovat vnořené přístupné typu s názvem `I` a `K`  zadejte parametry a potom *namespace_or_type_name* odkazuje na tento typ vytvořený s argumenty daného typu. Pokud existuje více než jeden takový typ, je vybraný typ deklarovaný v rámci více odvozeného typu. Všimněte si, že pokud význam `N.I` určen jako součást řešení specifikace základní třídy `N` pak přímé základní třídy `N` se považuje za objekt ([základních tříd](classes.md#base-classes)).
    * V opačném případě `N.I` je neplatný *namespace_or_type_name*, a dojde k chybě kompilace.

A *namespace_or_type_name* smí odkazovat na statickou třídu ([statické třídy](classes.md#static-classes)) pouze v případě

*  *Namespace_or_type_name* je `T` v *namespace_or_type_name* formuláře `T.I`, nebo
*  *Namespace_or_type_name* je `T` v *typeof_expression* ([seznamy argumentů](expressions.md#argument-lists)1) ve formátu `typeof(T)`.

### <a name="fully-qualified-names"></a>Plně kvalifikované názvy

Každý obor názvů a typ má ***plně kvalifikovaný název***, který jednoznačně identifikuje obor názvů nebo typ mimo všechny ostatní. Plně kvalifikovaný název oboru názvů nebo typ `N` je stanoven následujícím způsobem:

*  Pokud `N` patří do globálního oboru názvů a jeho plně kvalifikovaný název je `N`.
*  V opačném případě je jeho plně kvalifikovaný název `S.N`, kde `S` je plně kvalifikovaný název oboru názvů nebo typ, ve kterém `N` je deklarována.

Jinými slovy, plně kvalifikovaný název `N` je kompletní hierarchické cesty identifikátorů, které vedly k `N`od globálního oboru názvů. Protože každý člen obor názvů nebo typ musí mít jedinečný název, následuje plně kvalifikovaný název oboru názvů nebo typ je vždy jedinečný.

Následující příklad ukazuje několik deklarací oboru názvů a typ spolu s jejich přidružených plně kvalifikovaných názvů.
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

C# používá Automatická správa paměti, která vaši vývojáři z ručně přidělování a uvolňování paměti obsazena objekty. Zásady správy paměti automatické implementují ***systému uvolňování paměti***. Životní cyklus správy paměti objektu vypadá takto:

1. Při vytvoření objektu se pro něj přidělené paměti, spusťte konstruktoru a objekt považován za provozu.
2. Pokud objekt nebo libovolné části, nemůže mít přístup všechny možné pokračování provádění, než spuštění destruktory, objekt je považován za už používá a bude vhodné pro zničení. Kompilátor jazyka C# a systému uvolňování paměti můžou rozhodnout pro analýzu kódu k určení, které odkazuje na objekt lze v budoucnu. Například pokud místní proměnná, která je v oboru je pouze existující odkaz na objekt, ale tuto místní proměnnou se nikdy označuje všechny možné pokračování provádění aktuálního spuštění bodu v postupu, uvolňování může (ale není potřeba) považovat objektu jako už používá.
3. Po nárok zničení objektu na některé později tento parametr nezadáte čas destruktoru ([destruktory](classes.md#destructors)) (pokud existuje) pro spuštění objektu. Za normálních okolností destruktor objektu se spustí pouze jednou, ale specifický pro implementaci rozhraní API mohou povolit toto chování k přepsání.
4. Po spuštění destruktoru objektu, pokud daný objekt nebo libovolné části, není přístupná všechny možné pokračování provádění, včetně spuštění destruktory, objekt je považován za nedostupné a objekt se stane nárok na kolekci.
5. A konečně někdy po objekt stane nárok na kolekci, systému uvolňování paměti uvolní paměti spojený s tímto objektem.

Uvolňování paměti uchovává informace o použití objektu a využívá tyto informace, aby paměti rozhodnutích týkajících se správy, jako je například umístění v paměti vyhledejte nově vytvořený objekt, když se přesunout objekt a když objekt již není používán nebo nedostupný.

Stejně jako v jiných jazycích, které se předpokládá existenci systému uvolňování paměti C# je navržený tak, aby systému uvolňování paměti může implementovat řadu různých zásad správy paměti. Například C# nevyžaduje, aby spuštění destruktory nebo shromažďovat objekty jako přicházejí nebo, že destruktory spustit v libovolném pořadí nebo v libovolném konkrétním vlákně.

Řídí chování systému uvolňování paměti, do určité míry, prostřednictvím statických metod ve třídě `System.GC`. Tato třída slouží k vyžádání kolekce pravděpodobnější, destruktory spuštění (nebo nespuštěno) a tak dále.

Od systému uvolňování paměti může široké šířky při rozhodování, kdy se mají shromáždit objekty a spusťte destruktory, vyhovující implementace může vytvořit výstup, který se liší od, který je znázorněno v následujícím kódu. Program
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
vytvoří instanci třídy `A` a instance třídy `B`. Tyto objekty stane oprávněnými pro uvolnění při proměnné `b` je přiřazena hodnota `null`, protože se po uplynutí této doby je možné pro uživatelem zapsaný kód pro přístup k nim. Výstup může být buď
```
Destruct instance of A
Destruct instance of B
```
or
```
Destruct instance of B
Destruct instance of A
```
protože jazyk ukládá bez omezení na pořadí, ve kterém jsou objekty uvolněna z paměti.

V případech, malý může být důležité rozdíl mezi "nárok zničení" a "nárok na kolekci". Například
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

Ve výše uvedené program, pokud se rozhodne systému uvolňování paměti ke spuštění destruktor `A` před destruktor `B`, může to být výstup tohoto programu:
```
Destruct instance of A
Destruct instance of B
A.F
RefA is not null
```

Všimněte si, že i když instance `A` nebyl používán a `A`společnosti byl spuštěn destruktor, je stále možné metody `A` (v tomto případě `F`) k volání z jiné destruktor. Také si všimněte, že může způsobit spuštění destruktoru objektu autentický z hlavní program znovu. V takovém případě spouštění `B`je destruktor způsobila instance `A` , který byl dříve nečinnosti se zpřístupní z živého odkazu `Test.RefA`. Po volání `WaitForPendingFinalizers`, instanci `B` má nárok na kolekce, ale instance `A` není k dispozici, protože odkaz `Test.RefA`.

Abyste předešli zmatení a neočekávané chování, je obecně vhodné pro destruktory provádět čištění jenom s daty uloženými v jejich vlastní polím objektu a nechcete provádět všechny akce v odkazované objekty nebo statická pole.

Se o alternativu k použití destruktorů umožňuje implementovat třídu `System.IDisposable` rozhraní. To umožňuje klientovi k určení toho, kdy k uvolnění prostředků objektu, obvykle tak přístup k objektu jako prostředek v objektu `using` – příkaz ([pomocí příkazu](statements.md#the-using-statement)).

## <a name="execution-order"></a>Pořadí provádění

Spuštění programu v jazyce C# pokračuje tak, aby vedlejší účinky jednotlivých provádění vlákna jsou zachovány při provádění kritické body. A ***vedlejší účinek*** je definován jako čtení nebo zápisu pole s modifikátorem volatile, zápis stálé proměnné, zápis do externího prostředku a vyvolává výjimku. Spuštění nutné pro body, ve kterých musí být zachovány pořadí těchto vedlejší účinky jsou odkazy na pole s modifikátorem volatile ([pole s modifikátorem Volatile](classes.md#volatile-fields)), `lock` příkazy ([příkaz lock](statements.md#the-lock-statement)), a vytváření a ukončení vlákna. Prostředí pro spuštění je zdarma, chcete-li změnit pořadí provádění programu v C# v souladu s následujícími omezeními:

*  Závislost na data se zachovají v rámci vlákno provádění. To znamená hodnotu každé proměnné je vypočítán jako by všechny příkazy ve vlákně byly provedeny v původní pořadí programu.
*  Inicializace pořadí pravidel jsou zachovány ([inicializace pole](classes.md#field-initialization) a [proměnné inicializátory](classes.md#variable-initializers)).
*  Pořadí vedlejší účinky je zachováno s ohledem na volatile čtení a zápisy ([pole s modifikátorem Volatile](classes.md#volatile-fields)). Kromě toho prostředí pro spouštění nemusí vyhodnocení součástí výrazu, jestli můžete zjistit, že hodnota tohoto výrazu se nepoužívá a že žádné vedlejší účinky potřebné vytvářeny (včetně všech volání metody nebo přístup k pole s modifikátorem volatile). Když dojde k přerušení provádění programu asynchronní událostí (například výjimka vyvolaná objektem jiného vlákna), není zaručeno, že pozorovatelný vedlejší účinky jsou viditelné v původní pořadí programu.
