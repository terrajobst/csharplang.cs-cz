# <a name="namespaces"></a>Jmenné prostory

Programy jazyka C# jsou uspořádané pomocí oborů názvů. Obory názvů slouží jako systém "internal" organizace pro program i jako systém "externí" organizace, způsob prezentace prvky programu, které jsou vystaveny do jiných programů.

Direktivy using ([direktiv Using](namespaces.md#using-directives)) jsou k dispozici pro usnadnění použití oborů názvů.

## <a name="compilation-units"></a>Kompilačních jednotek

A *compilation_unit* definuje strukturu celkové zdrojového souboru. Jednotka kompilace se skládá z nula nebo více *using_directive*s následovat nula nebo více *global_attributes* následovat nula nebo více *namespace_member_declaration*s .

```antlr
compilation_unit
    : extern_alias_directive* using_directive* global_attributes? namespace_member_declaration*
    ;
```

Se skládá z jedné nebo více jednotek kompilace programu v jazyce C#, každý obsažené v samostatném zdrojovém souboru. Při kompilaci programu v jazyce C# kompilačních jednotek jsou zpracovány všechny najednou. Proto kompilačních jednotek můžete jsou vzájemně závislé, může být cyklické způsobem.

*Using_directive*s vliv jednotku kompilace *global_attributes* a *namespace_member_declaration*s z kompilační jednotky, ale nemají žádný vliv jiné jednotky kompilace.

*Global_attributes* ([atributy](attributes.md)) z kompilační jednotky povolit specifikaci atributy pro cílové sestavení a modul. Sestavení a moduly fungují jako fyzické kontejnery pro typy. Sestavení může obsahovat několik fyzicky oddělená modulů.

*Namespace_member_declaration*s každou jednotku kompilace programu přispívají členové jediné deklaraci místo nazývané globální obor názvů. Příklad:

Soubor `A.cs`:
```csharp
class A {}
```

Soubor `B.cs`:
```csharp
class B {}
```

Dva kompilačních jednotek přispívat do jednoho globálního oboru názvů, v tomto případě deklarovat dvě třídy s plně kvalifikované názvy `A` a `B`. Protože dva kompilačních jednotek přispívat na stejné místo prohlášení, bylo by chybě pokud každá obsahovala deklarace člena se stejným názvem.

## <a name="namespace-declarations"></a>Deklarace Namespace

A *namespace_declaration* se skládá z klíčového slova `namespace`, za nímž následuje název oboru názvů a text, může volitelně následovat středník.

```antlr
namespace_declaration
    : 'namespace' qualified_identifier namespace_body ';'?
    ;

qualified_identifier
    : identifier ('.' identifier)*
    ;

namespace_body
    : '{' extern_alias_directive* using_directive* namespace_member_declaration* '}'
    ;
```

A *namespace_declaration* může dojít k jako deklaraci nejvyšší úrovně v *compilation_unit* nebo jako člen deklarace v rámci jiného *namespace_declaration*. Když *namespace_declaration* vyskytuje se jako deklaraci nejvyšší úrovně v *compilation_unit*, obor názvů, stane se členem globální obor názvů. Když *namespace_declaration* dochází v rámci jiného *namespace_declaration*, vnitřní obor názvů, stane se členem vnější obor názvů. V obou případech se název oboru názvů musí být jedinečný v rámci nadřazeného oboru názvů.

Obory názvů jsou implicitně `public` a deklarace oboru názvů nesmí obsahovat žádné modifikátory přístupu.

V rámci *namespace_body*, volitelný *using_directive*importovat s názvy jiných obory názvů, typy a členy, což jim umožní být odkazovaných přímo namísto prostřednictvím kvalifikované názvy. Volitelný *namespace_member_declaration*s přispívat členy do prostoru deklarace oboru názvů. Všimněte si, že všechny *using_directive*s musí být uvedena před všemi deklaracemi členů.

*Qualified_identifier* z *namespace_declaration* může být jednoho identifikátoru nebo posloupnost identifikátorů oddělených "`.`" tokeny. Druhý formulář umožňuje programu definovat vnořené oboru názvů bez vnoření lexikálně několik deklarace oboru názvů. Například

```csharp
namespace N1.N2
{
    class A {}

    class B {}
}
```
je sémantické ekvivalenty
```csharp
namespace N1
{
    namespace N2
    {
        class A {}

        class B {}
    }
}
```

Obory názvů jsou otevřený a dvě deklarace oboru názvů se stejným plně kvalifikovaným názvem přispívat na stejné místo prohlášení ([deklarace](basic-concepts.md#declarations)). V příkladu
```csharp
namespace N1.N2
{
    class A {}
}

namespace N1.N2
{
    class B {}
}
```
výše uvedené dvě deklarace oboru názvů přispívat na stejné místo prohlášení, v tomto případě deklarovat dvě třídy s plně kvalifikované názvy `N1.N2.A` a `N1.N2.B`. Vzhledem k tomu, že dvě deklarace přispívat na stejné místo prohlášení, by bylo, že se chybu, pokud každá obsahovala deklarace člena se stejným názvem.

## <a name="extern-aliases"></a>Aliasy extern

*Extern_alias_directive* zavádí identifikátor, který slouží jako alias pro obor názvů. Specifikace oboru názvů s aliasem je externí ke zdrojovému kódu programu a platí také pro vnořené obory názvů s aliasem oboru názvů.

```antlr
extern_alias_directive
    : 'extern' 'alias' identifier ';'
    ;
```

Rozsah *extern_alias_directive* rozloženo *using_directive*s, *global_attributes* a *namespace_member_declaration*s jeho bezprostředně nadřazeného kompilace částí nebo těle oboru názvů.

V rámci kompilace částí nebo těle oboru názvů, který obsahuje *extern_alias_directive*, identifikátor zavedené *extern_alias_directive* slouží k odkazu na obor názvů s aliasem. Je chyba kompilace pro *identifikátor* být slovo `global`.

*Extern_alias_directive* zpřístupní alias v rámci konkrétní kompilace jednotky nebo oboru názvů tělo, ale to není přispívat všemi novými členy do prostoru základní deklarace. Jinými slovy *extern_alias_directive* není přenosné, ale místo toho ovlivňuje pouze kompilaci jednotky nebo oboru názvů text ve kterém se vyskytuje.

Následující program deklaruje a používá dva extern aliasy `X` a `Y`, každý z které představují nejnižší úrovni hierarchie jedinečných názvů:
```csharp
extern alias X;
extern alias Y;

class Test
{
    X::N.A a;
    X::N.B b1;
    Y::N.B b2;
    Y::N.C c;
}
```

Program deklaruje existenci externích aliasů `X` a `Y`, ale skutečné definice aliasy jsou externí vzhledem k programu. Identicky pojmenovanou `N.B` třídy může být odkazováno nyní jako `X.N.B` a `Y.N.B`, nebo pomocí kvalifikátor aliasu oboru názvů `X::N.B` a `Y::N.B`. Pokud program deklaruje externí alias pro kterou je k dispozici žádná externí definice dojde k chybě.

## <a name="using-directives"></a>direktivy using

***Direktivy using*** usnadnění používání oborů názvů a typy definované v jiných oborech názvů. Pomocí direktivy vliv proces překladu IP adres z *namespace_or_type_name*s ([Namespace a zadejte názvy](basic-concepts.md#namespace-and-type-names)) a *simple_name*s ([jednoduché názvy ](expressions.md#simple-names)), ale na rozdíl od deklarace pomocí direktivy nepočítají nové členy do základní deklaraci prostorů kompilačních jednotek nebo obory názvů, ve kterém se používají.

```antlr
using_directive
    : using_alias_directive
    | using_namespace_directive
    | using_static_directive
    ;
```

A *using_alias_directive* ([alias direktiv Using](namespaces.md#using-alias-directives)) představuje alias pro obor názvů nebo typ.

A *using_namespace_directive* ([pomocí direktivy oboru názvů](namespaces.md#using-namespace-directives)) Importuje členy typu oboru názvů.

A *using_static_directive* ([statické direktiv Using](namespaces.md#using-static-directives)) Importuje vnořené typy a statické členy typu.

Rozsah *using_directive* rozloženo *namespace_member_declaration*s jeho bezprostředně nadřazeného kompilace částí nebo těle oboru názvů. Rozsah *using_directive* konkrétně nezahrnuje jeho peer *using_directive*s. Díky tomu se vytvoření partnerského vztahu *using_directive*s nemají vliv na sobě navzájem, a pořadí, ve kterém jsou zapsány je neplatné.

### <a name="using-alias-directives"></a>Alias direktivy using

A *using_alias_directive* zavádí identifikátor, který slouží jako alias pro obor názvů nebo typ těla bezprostředně vložená kompilace jednotky nebo oboru názvů.

```antlr
using_alias_directive
    : 'using' identifier '=' namespace_or_type_name ';'
    ;
```

V rámci deklarace členů v kompilaci jednotky nebo oboru názvů subjektu, který obsahuje *using_alias_directive*, identifikátor zavedené *using_alias_directive* lze použít k odkazu daný obor názvů nebo typ. Příklad:
```csharp
namespace N1.N2
{
    class A {}
}

namespace N3
{
    using A = N1.N2.A;

    class B: A {}
}
```

Výše uvedené, v rámci deklarace členů v `N3` obor názvů, `A` je alias pro `N1.N2.A`a proto třídy `N3.B` je odvozena z třídy `N1.N2.A`. Stejného výsledku lze získat tak, že vytvoříte alias `R` pro `N1.N2` a potom odkazování na ně `R.A`:
```csharp
namespace N3
{
    using R = N1.N2;

    class B: R.A {}
}
```

*Identifikátor* z *using_alias_directive* musí být jedinečný v rámci deklarace prostoru kompilační jednotky nebo oboru názvů, který obsahuje okamžitě *using_alias_directive* . Příklad:
```csharp
namespace N3
{
    class A {}
}

namespace N3
{
    using A = N1.N2.A;        // Error, A already exists
}
```

Výše uvedené, `N3` již obsahuje člena `A`, takže se jedná o chybu kompilace pro *using_alias_directive* používat tento identifikátor. Podobně je chyba kompilace pro dvě nebo více *using_alias_directive*ve stejné kompilaci jednotky nebo oboru názvů tělo pro deklaraci aliasy se stejným názvem.

A *using_alias_directive* zpřístupní alias v rámci konkrétní kompilace jednotky nebo oboru názvů tělo, ale to není přispívat všemi novými členy do prostoru základní deklarace. Jinými slovy *using_alias_directive* není přenosné, ale spíše ovlivňuje pouze kompilaci jednotky nebo oboru názvů text ve kterém se vyskytuje. V příkladu
```csharp
namespace N3
{
    using R = N1.N2;
}

namespace N3
{
    class B: R.A {}            // Error, R unknown
}
```
rozsah *using_alias_directive* , který představuje `R` rozšiřuje pouze na deklarace členů v těle oboru názvů, ve kterém je obsažená, takže `R` není známý v druhé deklarace oboru názvů. Ale, že umístíte *using_alias_directive* obsahující kompilace částí způsobí, že alias k dispozici v rámci obě deklarace oboru názvů:
```csharp
using R = N1.N2;

namespace N3
{
    class B: R.A {}
}

namespace N3
{
    class C: R.A {}
}
```

Stejně jako běžné členy, názvy zavedené *using_alias_directive*s jsou skryté členy s podobným názvem ve vnořené obory. V příkladu
```csharp
using R = N1.N2;

namespace N3
{
    class R {}

    class B: R.A {}        // Error, R has no member A
}
```
odkaz na `R.A` v deklaraci `B` způsobí chybu kompilace, protože `R` odkazuje na `N3.R`, nikoli `N1.N2`.

Pořadí, ve kterém *using_alias_directive*s jsou zapsány nemá žádný význam a rozlišení *namespace_or_type_name* odkazuje *using_alias_directive*nemá vliv *using_alias_directive* samotné nebo jiná *using_directive*ve okamžitě obsahující text jednotky nebo oboru názvů kompilace. Jinými slovy *namespace_or_type_name* z *using_alias_directive* vyřeší, jako kdyby měl okamžitě obsahující text jednotky nebo oboru názvů kompilace bez *using_directive*s. A *using_alias_directive* však může být ovlivněn *extern_alias_directive*ve okamžitě obsahující text jednotky nebo oboru názvů kompilace. V příkladu
```csharp
namespace N1.N2 {}

namespace N3
{
    extern alias E;

    using R1 = E.N;        // OK

    using R2 = N1;         // OK

    using R3 = N1.N2;      // OK

    using R4 = R2.N2;      // Error, R2 unknown
}
```
poslední *using_alias_directive* výsledkem chyba kompilace, protože nemá vliv první *using_alias_directive*. První *using_alias_directive* nemá za následek chybu od oboru extern alias `E` zahrnuje *using_alias_directive*.

A *using_alias_directive* můžete vytvořit alias pro obor názvů nebo typ, včetně oboru názvů, ve kterém se zobrazí a jakékoli obor názvů nebo typ vnořené v daném oboru názvů.

Přístup k oboru názvů nebo typ prostřednictvím alias vrací stejný výsledek jako přístupu tohoto oboru názvů nebo typ s využitím názvu deklarovaný. Mějme například
```csharp
namespace N1.N2
{
    class A {}
}

namespace N3
{
    using R1 = N1;
    using R2 = N1.N2;

    class B
    {
        N1.N2.A a;            // refers to N1.N2.A
        R1.N2.A b;            // refers to N1.N2.A
        R2.A c;               // refers to N1.N2.A
    }
}
```
názvy `N1.N2.A`, `R1.N2.A`, a `R2.A` jsou ekvivalentní a všechny odkazovat na třídu, jejíž plně kvalifikovaný název je `N1.N2.A`.

Aliasy Using název uzavřený konstruovaný typ. může, ale nikoli název deklarace nevázaný parametr generického typu bez zadávání argumentů typu. Příklad:
```csharp
namespace N1
{
    class A<T>
    {
        class B {}
    }
}

namespace N2
{
    using W = N1.A;          // Error, cannot name unbound generic type

    using X = N1.A.B;        // Error, cannot name unbound generic type

    using Y = N1.A<int>;     // Ok, can name closed constructed type

    using Z<T> = N1.A<T>;    // Error, using alias cannot have type parameters
}
```

### <a name="using-namespace-directives"></a>Pomocí direktivy oboru názvů

A *using_namespace_directive* importuje typy obsažené v oboru názvů do bezprostředně vložená kompilace částí nebo těle oboru názvů, povolení identifikátor každého typu bez kvalifikace lze používat.

```antlr
using_namespace_directive
    : 'using' namespace_name ';'
    ;
```

V rámci deklarace členů v kompilaci jednotky nebo oboru názvů subjektu, který obsahuje *using_namespace_directive*, typy obsažené v daném oboru názvů může být odkazováno přímo. Příklad:
```csharp
namespace N1.N2
{
    class A {}
}

namespace N3
{
    using N1.N2;

    class B: A {}
}
```

Výše uvedené, v rámci deklarace členů v `N3` typ členů z oboru názvů `N1.N2` se přímo k dispozici a proto třídy `N3.B` je odvozena z třídy `N1.N2.A`.

A *using_namespace_directive* importuje typy obsažené v daném oboru názvů, ale specificky neimportuje vnořené obory názvů. V příkladu
```csharp
namespace N1.N2
{
    class A {}
}

namespace N3
{
    using N1;

    class B: N2.A {}        // Error, N2 unknown
}
```
*using_namespace_directive* importuje typy obsažené v `N1`, ale ne obory názvů vnořené v `N1`. Díky tomu se odkaz na `N2.A` v deklaraci `B` výsledkem chyba kompilace, protože žádné členy s názvem `N2` nacházejí v oboru.

Na rozdíl od *using_alias_directive*, *using_namespace_directive* mohou importovat typy, jejichž identifikátory jsou již definovány v rámci nadřazeného kompilace jednotky nebo oboru názvů subjektu. V důsledku toho názvů importované tímto seznamem *using_namespace_directive* jsou skryté členy s podobným názvem v nadřazeném kompilace jednotky nebo oboru názvů textu. Příklad:
```csharp
namespace N1.N2
{
    class A {}

    class B {}
}

namespace N3
{
    using N1.N2;

    class A {}
}
```

Tady v rámci deklarace členů v `N3` obor názvů, `A` odkazuje na `N3.A` spíše než `N1.N2.A`.

Když se více než jeden obor názvů nebo typ importované tímto seznamem *using_namespace_directive*s nebo *using_static_directive*ve stejné kompilaci tělo jednotky nebo oboru názvů obsahují typy se stejným názvem odkazy na Tento název jako *type_name* jsou považovány za nejednoznačné. V příkladu
```csharp
namespace N1
{
    class A {}
}

namespace N2
{
    class A {}
}

namespace N3
{
    using N1;

    using N2;

    class B: A {}                // Error, A is ambiguous
}
```
obě `N1` a `N2` obsahovat člena `A`a protože `N3` importuje obě, odkazující na `A` v `N3` je chyba kompilace. V takovém případě může být konflikt vyřešit buď prostřednictvím kvalifikace odkazy na `A`, nebo zavedením *using_alias_directive* , který vybere konkrétní `A`. Příklad:
```csharp
namespace N3
{
    using N1;

    using N2;

    using A = N1.A;

    class B: A {}                // A means N1.A
}
```

Kromě toho, kdy více než jeden obor názvů nebo typ importované tímto seznamem *using_namespace_directive*s nebo *using_static_directive*ve stejné kompilaci tělo jednotky nebo oboru názvů obsahují typy nebo členy podle stejný název, odkazuje na tento název jako *simple_name* jsou považovány za nejednoznačné. V příkladu
```csharp
namespace N1
{
    class A {}
}

class C
{
    public static int A;
}

namespace N2
{
    using N1;
    using static C;

    class B
    {
        void M() 
        { 
            A a = new A();   // Ok, A is unambiguous as a type-name
            A.Equals(2);     // Error, A is ambiguous as a simple-name
        }
    }
}
```
`N1` obsahuje člen typu `A`, a `C` obsahuje statickou metodu `A`a protože `N2` importuje obě, odkazující na `A` jako *simple_name* je nejednoznačný a kompilace došlo k chybě. 

Podobně jako *using_alias_directive*, *using_namespace_directive* není přispívat všemi novými členy základní deklaraci prostoru kompilační jednotky nebo oboru názvů, ale místo toho má vliv pouze kompilace částí nebo těle oboru názvů ve kterém se zobrazí.

*Namespace_name* odkazovaná *using_namespace_directive* vyřeší stejným způsobem jako *namespace_or_type_name* odkazuje  *using_alias_directive*. Proto *using_namespace_directive*ve stejné kompilaci jednotky nebo oboru názvů tělo nemají vliv na sebe navzájem a mohou být napsány v libovolném pořadí.

### <a name="using-static-directives"></a>Statické direktivy using

A *using_static_directive* importuje vnořené typy a statické členy přímo součástí deklarace typu do bezprostředně vložená kompilace částí nebo těle oboru názvů, povolení identifikátor každého člena a typ být použít bez kvalifikace.

```antlr
using_static_directive
    : 'using' 'static' type_name ';'
    ;
```

V rámci deklarace členů v kompilaci jednotky nebo oboru názvů subjektu, který obsahuje *using_static_directive*, přístupný vnořené typy a statické členy (s výjimkou metody rozšíření) součástí deklarace daný typ může být odkazováno přímo. Příklad:

```csharp
namespace N1
{
    class A 
    {
        public class B{}
        public static B M(){ return new B(); }
    }
}

namespace N2
{
    using static N1.A;
    class C
    {
        void N() { B b = M(); }
    }
}
```

Výše uvedené, v rámci deklarace členů v `N2` obor názvů, statické členy a vnořené typy `N1.A` jsou přímo k dispozici a tedy metodu `N` může odkazovat i `B` a `M` členy `N1.A`.

A *using_static_directive* konkrétně neimportuje rozšiřující metody přímo jako statické metody, ale zpřístupňuje je pro volání metody rozšíření ([volání metod rozšíření](expressions.md#extension-method-invocations)). V příkladu

```csharp
namespace N1 
{
    static class A 
    {
        public static void M(this string s){}
    }
}

namespace N2
{
    using static N1.A;

    class B
    {
        void N() 
        {
            M("A");      // Error, M unknown
            "B".M();     // Ok, M known as extension method
            N1.A.M("C"); // Ok, fully qualified
        }
    }
}
```
*using_static_directive* importuje rozšiřující metoda `M` součástí `N1.A`, ale pouze jako metody rozšíření. Proto první odkaz na `M` v těle `B.N` výsledkem chyba kompilace, protože žádné členy s názvem `M` nacházejí v oboru.

A *using_static_directive* importuje jenom členy a typy deklarované přímo do daného typu, ne členy a typy deklarované v základních tříd.

TODO: Příklad

Nejednoznačnosti mezi několika *using_namespace_directives* a *using_static_directives* jsou popsány v [pomocí direktivy oboru názvů](namespaces.md#using-namespace-directives).

## <a name="namespace-members"></a>Namespace členy

A *namespace_member_declaration* je buď *namespace_declaration* ([deklarací Namespace](namespaces.md#namespace-declarations)) nebo *type_declaration* () [Typ deklarace](namespaces.md#type-declarations)).

```antlr
namespace_member_declaration
    : namespace_declaration
    | type_declaration
    ;
```

Jednotka kompilace nebo těle oboru názvů může obsahovat *namespace_member_declaration*s a takové deklarace přispívat nové členy do prostoru základní deklarace obsahující text jednotky nebo oboru názvů kompilace.

## <a name="type-declarations"></a>Deklarace typu

A *type_declaration* je *class_declaration* ([třídy deklarací](classes.md#class-declarations)), *struct_declaration* ([– struktura deklarace](structs.md#struct-declarations)), *interface_declaration* ([rozhraní deklarace](interfaces.md#interface-declarations)), *enum_declaration* ([výčtu deklarace](enums.md#enum-declarations)), nebo *delegate_declaration* ([delegovat deklarace](delegates.md#delegate-declarations)).

```antlr
type_declaration
    : class_declaration
    | struct_declaration
    | interface_declaration
    | enum_declaration
    | delegate_declaration
    ;
```

A *type_declaration* situace může nastat jako deklaraci nejvyšší úrovně v jednotce kompilace nebo jako člen deklarace v rámci oboru názvů, třídy nebo struktury.

Při deklaraci typu pro typ `T` vyskytuje se jako deklaraci nejvyšší úrovně v jednotce kompilace, plně kvalifikovaný název nově deklarovaného typu je jednoduše `T`. Při deklaraci typu pro typ `T` dochází v rámci oboru názvů, třídy nebo struktury, plně kvalifikovaný název nově deklarovaného typu je `N.T`, kde `N` je plně kvalifikovaný název obsahuje obor názvů, třídy nebo struktury.

Typ deklarovaný v rámci třídy nebo struktury se nazývá vnořený typ. ([vnořené typy](classes.md#nested-types)).

Povolené přístupu modifikátory přístupu a přístup k výchozím pro deklaraci typu závisí na kontextu, ve kterém probíhá deklarace ([deklarovaná přístupnost](basic-concepts.md#declared-accessibility)):

*  Typy deklarované v jednotkách kompilace nebo obory názvů můžou mít `public` nebo `internal` přístup. Výchozí hodnota je `internal` přístup.
*  Může mít typy deklarované v třídách `public`, `protected internal`, `protected`, `internal`, nebo `private` přístup. Výchozí hodnota je `private` přístup.
*  Může mít typy deklarované ve strukturách `public`, `internal`, nebo `private` přístup. Výchozí hodnota je `private` přístup.

## <a name="namespace-alias-qualifiers"></a>Kvalifikátory alias Namespace

***Kvalifikátor aliasu oboru názvů*** `::` díky tomu je možné zaručit, že název prohledávání typů, které nejsou ovlivněny zavedení nové typy a členy. Kvalifikátor aliasu oboru názvů se vždy zobrazuje mezi dva identifikátory, které jsou uvedené jako identifikátory levé a pravé. Na rozdíl od standardní `.` kvalifikátor, vlevo identifikátor `::` kvalifikátor se hledá nahoru pouze jako externího prvku nebo using alias.

A *qualified_alias_member* je definovaná následujícím způsobem:

```antlr
qualified_alias_member
    : identifier '::' identifier type_argument_list?
    ;
```

A *qualified_alias_member* může sloužit jako *namespace_or_type_name* ([Namespace a zadejte názvy](basic-concepts.md#namespace-and-type-names)) nebo jako levý operand v *member_access* ([Přístup ke členu](expressions.md#member-access)).

A *qualified_alias_member* má jednu z těchto dvou tvarů:

*  `N::I<A1, ..., Ak>`, kde `N` a `I` představují identifikátory, a `<A1, ..., Ak>` je seznam argumentů typu. (`K` je vždy alespoň jednou.)
*  `N::I`, kde `N` a `I` představují identifikátory. (V tomto případě `K` je považován za nulu.)

Použití tohoto zápisu význam *qualified_alias_member* je stanoven následujícím způsobem:

*  Pokud `N` je identifikátor `global`, pak se hledá globální obor názvů `I`:
   * Pokud globální obor názvů obsahuje obor názvů s názvem `I` a `K` je nula, pak bude *qualified_alias_member* odkazuje na tento obor názvů.
   * Jinak, pokud je globální obor názvů obsahuje neobecný typ s názvem `I` a `K` je nula, pak bude *qualified_alias_member* odkazuje na typu.
   * Jinak, pokud je globální obor názvů obsahuje typ s názvem `I` , který má `K`  parametry typu, pak bude *qualified_alias_member* odkazuje na tento typ vytvořený s argumenty daného typu.
   * V opačném případě *qualified_alias_member* je nedefinovaný a dojde k chybě kompilace.

*  V opačném případě od deklarace oboru názvů ([deklarací Namespace](namespaces.md#namespace-declarations)) okamžitě obsahující *qualified_alias_member* (pokud existuje), pokračování s každou ohraničující deklarace oboru názvů (pokud existuje) a konče jednotku kompilace obsahující *qualified_alias_member*, následující kroky jsou vyhodnocen, dokud se entita nachází:

   * Pokud obsahuje obor názvů deklarace nebo kompilace částí *using_alias_directive* , která přidruží `N` s typem, pak bude *qualified_alias_member* není definována a za kompilace dojde k chybě.
   * Jinak, pokud obsahuje obor názvů deklarace nebo kompilace částí *extern_alias_directive* nebo *using_alias_directive* , která přidruží `N` s oborem názvů, pak:
     * Pokud přidružené k oboru názvů `N` obsahuje obor názvů s názvem `I` a `K` je nula, pak bude *qualified_alias_member* odkazuje na tento obor názvů.
     * Jinak, pokud přidružené k oboru názvů `N` obsahuje neobecný typ s názvem `I` a `K` je nula, pak bude *qualified_alias_member* odkazuje na typu.
     * Jinak, pokud přidružené k oboru názvů `N` obsahuje typ s názvem `I` , který má `K`  parametry typu, pak bude *qualified_alias_member* označuje, že typ zkonstruován pomocí argumenty daného typu.
     * V opačném případě *qualified_alias_member* je nedefinovaný a dojde k chybě kompilace.
*  V opačném případě *qualified_alias_member* je nedefinovaný a dojde k chybě kompilace.

Všimněte si, že pomocí kvalifikátor aliasu oboru názvů s aliasem, který odkazuje na typ způsobí chybu kompilace. Všimněte si také, že pokud identifikátor `N` je `global`, pak vyhledávání se provádí v globálním oboru názvů, i v případě použití alias přidružení `global` s typem nebo oboru názvů.

### <a name="uniqueness-of-aliases"></a>Jedinečnost aliasy

Každý kompilace částí a obor názvů subjekt má samostatné prohlášení místa pro aliasy extern a aliasy using. Proto i když název externí alias nebo using alias musí být jedinečný v rámci sady aliasy extern a aliasy using deklarované v okamžitě obsahující text jednotky nebo oboru názvů kompilace, alias smí obsahovat mít stejný název jako typ nebo obor názvů, pokud mám t se používá jenom s `::` kvalifikátoru.

V příkladu
```csharp
namespace N
{
    public class A {}

    public class B {}
}

namespace N
{
    using A = System.IO;

    class X
    {
        A.Stream s1;            // Error, A is ambiguous

        A::Stream s2;           // Ok
    }
}
```
název `A` má dvě možných významů v těle druhého oboru názvů, protože obě třídy `A` a using alias `A` nacházejí v oboru. Z tohoto důvodu použití `A` v kvalifikovaném názvu `A.Stream` je nejednoznačný a způsobí chybu kompilace dojde k. Nicméně použití `A` s `::` kvalifikátor není chyba protože `A` se hledá jenom jako alias oboru názvů.
