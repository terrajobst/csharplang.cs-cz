# <a name="attributes"></a>Atributy

Velká část jazyka C# umožňuje programátorovi, aby zadejte deklarativní informace o entitách, které jsou definovány v programu. Například je určená pro usnadnění metody ve třídě upravení s *method_modifier*s `public`, `protected`, `internal`, a `private`.

C# umožňuje programátorům vytvářet nové typy deklarativní informací nazývaných ***atributy***. Programátoři pak můžete připojit atributy na různé entity programu a načíst informace o atributu v prostředí za běhu. Například může definovat rozhraní `HelpAttribute` atribut, který může být umístěn v určitých prvků programu (například třídy a metody) k poskytování mapování z těchto prvků programu do jejich dokumentaci.

Atributy jsou definované pomocí deklarace třídy atributů ([atribut třídy](attributes.md#attribute-classes)), který může mít poziční a pojmenované parametry ([Positional a pojmenované parametry](attributes.md#positional-and-named-parameters)). Atributy jsou připojeny k entitám v programu v C# pomocí atributů specifikací ([specifikace atributu](attributes.md#attribute-specification)) a mohou být načteny při spuštění jako instance atributu ([instance atributu do mezipaměti](attributes.md#attribute-instances)).

## <a name="attribute-classes"></a>Třídy atributů

Třída, která dědí z abstraktní třídy `System.Attribute`, ať už přímo či nepřímo, je ***třídy atributů***. Deklarace třídy atributu definuje nový druh ***atribut*** , který je možné použít v deklaraci. Podle konvence jsou pojmenovány třídy atributů s příponu `Attribute`. Použití atributu mohou zahrnout nebo vynechejte tuto příponu.

### <a name="attribute-usage"></a>Použití atributu

Atribut `AttributeUsage` ([atribut The AttributeUsage](attributes.md#the-attributeusage-attribute)) se používá k popisu použití třídy atributu.

`AttributeUsage` má poziční parametr ([Positional a pojmenované parametry](attributes.md#positional-and-named-parameters)), která umožňuje třídu atributu k určení typů deklarací, na kterých je možné. V příkladu

```csharp
using System;

[AttributeUsage(AttributeTargets.Class | AttributeTargets.Interface)]
public class SimpleAttribute: Attribute 
{
    ...
}
```

definuje třídu atributu s názvem `SimpleAttribute` , který můžete použít u *class_declaration*s a *interface_declaration*pouze s. V příkladu

```csharp
[Simple] class Class1 {...}

[Simple] interface Interface1 {...}
```

ukazuje použití několika `Simple` atribut. I když tento atribut je definován s názvem `SimpleAttribute`, když tento atribut se používá, `Attribute` přípona může být vynechán, výsledkem je krátký název `Simple`. Výše uvedený příklad je proto sémanticky ekvivalentní následujícímu zápisu:

```csharp
[SimpleAttribute] class Class1 {...}

[SimpleAttribute] interface Interface1 {...}
```

`AttributeUsage` má pojmenovaným parametrem ([Positional a pojmenované parametry](attributes.md#positional-and-named-parameters)) volá `AllowMultiple`, která udává, zda atribut lze zadat více než jednou pro danou entitu. Pokud `AllowMultiple` atribut třídy má hodnotu true, pak je danou třídu atributů ***více použít atribut třídy***a je možné zadat více než jednou entitou. Pokud `AllowMultiple` atributu třída má hodnotu false nebo neurčená a potom je danou třídu atributů ***jedno použití třídy atributů***a je možné zadat maximálně jednou entitou.

V příkladu

```csharp
using System;

[AttributeUsage(AttributeTargets.Class, AllowMultiple = true)]
public class AuthorAttribute: Attribute
{
    private string name;

    public AuthorAttribute(string name) {
        this.name = name;
    }

    public string Name {
        get { return name; }
    }
}
```

definuje třídu více použít atribut s názvem `AuthorAttribute`. V příkladu

```csharp
[Author("Brian Kernighan"), Author("Dennis Ritchie")] 
class Class1
{
    ...
}
```

ukazuje deklaraci třídy s dvěma způsoby použití `Author` atribut.

`AttributeUsage` má jiné pojmenovaný parametr s názvem `Inherited`, což znamená, zda atribut, pokud zadaný na základní třídu, je také děděné třídy, které jsou odvozeny z této základní třídy. Pokud `Inherited` atribut třídy má hodnotu true, pak tento atribut pochází. Pokud `Inherited` atributu třída má hodnotu false, pak tento atribut není zděděno. Pokud neurčená, jeho výchozí hodnota je true.

Třídu atributu `X` nemají `AttributeUsage` atribut připojena k němu, jako v

```csharp
using System;

class X: Attribute {...}
```

Je ekvivalentní následujícímu zápisu:

```csharp
using System;

[AttributeUsage(
    AttributeTargets.All,
    AllowMultiple = false,
    Inherited = true)
]
class X: Attribute {...}
```

### <a name="positional-and-named-parameters"></a>Pojmenované a poziční parametry

Atribut třídy mohou mít ***poziční parametry*** a ***pojmenované parametry***. Každý veřejný konstruktor instance pro třídu atributu definuje platné posloupnost poziční parametry pro danou třídu atributů. Každé nestatické pole veřejné čtení a zápis a vlastnosti pro třídu atributu definuje pojmenovaný parametr pro třídu atributu.

V příkladu

```csharp
using System;

[AttributeUsage(AttributeTargets.Class)]
public class HelpAttribute: Attribute
{
    public HelpAttribute(string url) {        // Positional parameter
        ...
    }

    public string Topic {                     // Named parameter
        get {...}
        set {...}
    }

    public string Url {
        get {...}
    }
}
```

definuje třídu atributu s názvem `HelpAttribute` , který má jeden pozičních parametrů více dopředu, `url`a jednu s názvem parametru `Topic`. Přestože jde o nestatická a public, vlastnost `Url` nedefinuje parametr s názvem, protože se nejedná o čtení a zápis.

Tato třída atributu může být použit takto:

```csharp
[Help("http://www.mycompany.com/.../Class1.htm")]
class Class1
{
    ...
}

[Help("http://www.mycompany.com/.../Misc.htm", Topic = "Class2")]
class Class2
{
    ...
}
```

### <a name="attribute-parameter-types"></a>Typy parametrů atributů

Typy poziční a pojmenované parametry atributu třídy jsou omezené na ***typy parametrů atributů***, které jsou:

*  Jedním z následujících typů: `bool`, `byte`, `char`, `double`, `float`, `int`, `long`, `sbyte`, `short`, `string`, `uint`, `ulong`, `ushort`.
*  Typ `object`.
*  Typ `System.Type`.
*  Zadaný typ výčtu, nemá přístupnost public a typy, ve kterých je vnořené (pokud existuje) je navíc přístupnost public ([specifikace atributu](attributes.md#attribute-specification)).
*  Jednorozměrná pole z výše uvedených typů.
*  Argument konstruktoru nebo veřejné pole, která nemá jeden z těchto typů nelze použít jako parametr poziční nebo pojmenované ve specifikaci atributu.

## <a name="attribute-specification"></a>Specifikace atributu

***Specifikace atributu*** je aplikace dříve definovaný atribut deklarace. Atribut je deklarativní informace, které je určená pro deklaraci. Atributy lze zadat v globálním oboru (k určení atributů na obsahující sestavení nebo modul) a pro *type_declaration*s ([typ deklarace](namespaces.md#type-declarations)), *class_member_declaration* s ([omezení parametru typu](classes.md#type-parameter-constraints)), *interface_member_declaration*s ([členy rozhraní](interfaces.md#interface-members)), *struct_member _declaration*s ([členy struktury](structs.md#struct-members)), *enum_member_declaration*s ([členy výčtu](enums.md#enum-members)), *accessor_declarations*  ([Přistupující objekty](classes.md#accessors)), *event_accessor_declarations* ([události podobné poli](classes.md#field-like-events)), a *formal_parameter_list*s ([parametry metody](classes.md#method-parameters)).

Atributy jsou určené v ***atribut oddíly***. Oddíl atribut se skládá z dvojice hranaté závorky, které před a za čárkou oddělený seznam jednoho nebo více atributů. Pořadí, ve kterém jsou zadané atributy do tohoto seznamu, a pořadí, ve kterém oddíly připojené do stejné entity programu jsou uspořádané, není důležité. Například atributů specifikací `[A][B]`, `[B][A]`, `[A,B]`, a `[B,A]` jsou ekvivalentní.

```antlr
global_attributes
    : global_attribute_section+
    ;

global_attribute_section
    : '[' global_attribute_target_specifier attribute_list ']'
    | '[' global_attribute_target_specifier attribute_list ',' ']'
    ;

global_attribute_target_specifier
    : global_attribute_target ':'
    ;

global_attribute_target
    : 'assembly'
    | 'module'
    ;

attributes
    : attribute_section+
    ;

attribute_section
    : '[' attribute_target_specifier? attribute_list ']'
    | '[' attribute_target_specifier? attribute_list ',' ']'
    ;

attribute_target_specifier
    : attribute_target ':'
    ;

attribute_target
    : 'field'
    | 'event'
    | 'method'
    | 'param'
    | 'property'
    | 'return'
    | 'type'
    ;

attribute_list
    : attribute (',' attribute)*
    ;

attribute
    : attribute_name attribute_arguments?
    ;

attribute_name
    : type_name
    ;

attribute_arguments
    : '(' positional_argument_list? ')'
    | '(' positional_argument_list ',' named_argument_list ')'
    | '(' named_argument_list ')'
    ;

positional_argument_list
    : positional_argument (',' positional_argument)*
    ;

positional_argument
    : attribute_argument_expression
    ;

named_argument_list
    : named_argument (','  named_argument)*
    ;

named_argument
    : identifier '=' attribute_argument_expression
    ;

attribute_argument_expression
    : expression
    ;
```

Atribut se skládá ze *attribute_name v relaci* a volitelný seznam poziční a pojmenované argumenty. Poziční argumenty (pokud existuje) předcházet pojmenované argumenty. Poziční argument se skládá ze *attribute_argument_expression*; pojmenovaný argument se skládá z názvu, za nímž následuje shodné znaménko, za nímž následuje *attribute_argument_expression*, který společně , jsou omezena stejnými pravidly jako jednoduchého přiřazení. Pojmenované argumenty pořadí není důležité.

*Attribute_name v relaci* Určuje třídu atributu. Pokud formu *attribute_name v relaci* je *type_name* potom tento název musí odkazovat na třídu atributu. V opačném případě dojde k chybě v době kompilace. V příkladu

```csharp
class Class1 {}

[Class1] class Class2 {}    // Error
```

výsledkem chyba kompilace, protože se pokouší použít `Class1` jako atribut třídu při `Class1` není třída atributu.

Určitých kontextech povolit specifikaci atributu na více než jeden cíl. Program můžete explicitně určit cílový zahrnutím *attribute_target_specifier*. Pokud atribut je umístěn na globální úrovni, *global_attribute_target_specifier* je povinný. V jiných umístěních, se použije přiměřené výchozí hodnoty, ale *attribute_target_specifier* lze potvrdit nebo potlačit výchozí v některých případech nejednoznačný (nebo pouze potvrzují výchozí v-dvojznačné případy). Proto, obvykle *attribute_target_specifier*s lze vynechat s výjimkou na globální úrovni. Potenciálně nejednoznačný kontexty jsou vyřešeny následujícím způsobem:

*  Atribut zadaný u globálního rozsahu můžete použít buď do cílového sestavení nebo modulu cíl. Neexistuje žádná výchozí hodnota pro tento kontext, tak *attribute_target_specifier* je vždy vyžadován v tomto kontextu. Přítomnost `assembly` *attribute_target_specifier* naznačuje, že atribut používá k cíli sestavení; přítomnost `module` *attribute_target_specifier* Označuje, že platí atribut pro cílový modul.
*  Na delegáta, který byl deklarován nebo na jeho návratovou hodnotu, můžete použít atribut zadaný v deklaraci delegáta. Chybí *attribute_target_specifier*, platí atribut pro delegáta. Přítomnost `type` *attribute_target_specifier* naznačuje, že atribut používá k delegování; přítomnost `return` *attribute_target_specifier* Označuje, že použije atribut návratovou hodnotu.
*  Atribut zadaný v deklaraci metody můžete použít metody deklarované nebo jeho návratovou hodnotu. Chybí *attribute_target_specifier*, platí atribut pro metodu. Přítomnost `method` *attribute_target_specifier* naznačuje, že atribut používá k metodě; přítomnost `return` *attribute_target_specifier* označuje že platí atribut pro návratovou hodnotu.
*  Atribut zadaný u deklarace operátoru lze použít operátor, který byl deklarován nebo jeho návratovou hodnotu. Chybí *attribute_target_specifier*, platí atribut pro operátor. Přítomnost `method` *attribute_target_specifier* naznačuje, že atribut používá operátor; přítomnost `return` *attribute_target_specifier* Označuje, že použije atribut návratovou hodnotu.
*  Atribut zadaný v deklaraci události, která vynechává přístupových objektů událostí můžete použít pro události deklarované, související pole (Pokud je událost není abstraktní) nebo pro související přidání a odebrání metody. Chybí *attribute_target_specifier*, platí atribut pro událost. Přítomnost `event` *attribute_target_specifier* naznačuje, že atribut používá k události; přítomnost `field` *attribute_target_specifier* označuje platí atribut pro pole. a přítomnost `method` *attribute_target_specifier* naznačuje, že používá atribut do metody.
*  Atribut zadaný u deklarace přistupující objekt get pro vlastnost nebo indexovací člen deklarace můžete použít metodu přidružené nebo jeho návratovou hodnotu. Chybí *attribute_target_specifier*, platí atribut pro metodu. Přítomnost `method` *attribute_target_specifier* naznačuje, že atribut používá k metodě; přítomnost `return` *attribute_target_specifier* označuje že platí atribut pro návratovou hodnotu.
*  Atribut na přístupový objekt set pro vlastnost nebo indexovací člen deklarace můžete použít na přidruženou metodu nebo na jeho jedinou implicitní parametr. Chybí *attribute_target_specifier*, platí atribut pro metodu. Přítomnost `method` *attribute_target_specifier* naznačuje, že atribut používá k metodě; přítomnost `param` *attribute_target_specifier* označuje platí atribut pro parametr; přítomnost `return` *attribute_target_specifier* označuje, že použije atribut návratovou hodnotu.
*  Atribut zadaný v deklaraci přístupový objekt add nebo remove pro deklaraci události můžete použít buď na přidruženou metodu, nebo jeho jedinou parametru. Chybí *attribute_target_specifier*, platí atribut pro metodu. Přítomnost `method` *attribute_target_specifier* naznačuje, že atribut používá k metodě; přítomnost `param` *attribute_target_specifier* označuje platí atribut pro parametr; přítomnost `return` *attribute_target_specifier* označuje, že použije atribut návratovou hodnotu.

V jiných kontextech, zahrnutí *attribute_target_specifier* je povolené, ale zbytečné. Například deklaraci třídy může obsahovat nebo vynechat specifikátor `type`:

```csharp
[type: Author("Brian Kernighan")]
class Class1 {}

[Author("Dennis Ritchie")]
class Class2 {}
```

Jedná se o chybu, chcete-li určit neplatný *attribute_target_specifier*. Například specifikátor `param` nelze použít v deklaraci třídy:

```csharp
[param: Author("Brian Kernighan")]        // Error
class Class1 {}
```

Podle konvence jsou pojmenovány třídy atributů s příponu `Attribute`. *Attribute_name v relaci* formuláře *type_name* může zahrnovat nebo vynechejte tuto příponu. Pokud se najde třídu atributu s i bez této přípony, existuje nejednoznačnost a způsobí chybu kompilace. Pokud *attribute_name v relaci* je napsán tak, aby jeho krajním *identifikátor* Doslovný identifikátor ([identifikátory](lexical-structure.md#identifiers)), pak pouze atribut bez přípony je nalezena shoda, což umožní tato nejednoznačnost vyřešit. V příkladu

```csharp
using System;

[AttributeUsage(AttributeTargets.All)]
public class X: Attribute
{}

[AttributeUsage(AttributeTargets.All)]
public class XAttribute: Attribute
{}

[X]                     // Error: ambiguity
class Class1 {}

[XAttribute]            // Refers to XAttribute
class Class2 {}

[@X]                    // Refers to X
class Class3 {}

[@XAttribute]           // Refers to XAttribute
class Class4 {}
```

ukazuje dvě atribut s názvem třídy `X` a `XAttribute`. Atribut `[X]` je nejednoznačný, protože by mohla odkazovat buď `X` nebo `XAttribute`. Použití Doslovný identifikátor umožňuje přesnou záměr zadání v těchto výjimečných případech. Atribut `[XAttribute]` není nejednoznačný (i když by být, pokud byla třída atributu s názvem `XAttributeAttribute`!). Pokud deklarace třídy `X` Odebereme, pak oba atributy odkazovat na třídu atributu s názvem `XAttribute`, následujícím způsobem:

```csharp
using System;

[AttributeUsage(AttributeTargets.All)]
public class XAttribute: Attribute
{}

[X]                     // Refers to XAttribute
class Class1 {}

[XAttribute]            // Refers to XAttribute
class Class2 {}

[@X]                    // Error: no attribute named "X"
class Class3 {}
```

Je chyba kompilace použít třídu atributů jedno použití více než jednou na stejné entity. V příkladu

```csharp
using System;

[AttributeUsage(AttributeTargets.Class)]
public class HelpStringAttribute: Attribute
{
    string value;

    public HelpStringAttribute(string value) {
        this.value = value;
    }

    public string Value {
        get {...}
    }
}

[HelpString("Description of Class1")]
[HelpString("Another description of Class1")]
public class Class1 {}
```

výsledkem chyba kompilace, protože se pokouší použít `HelpString`, tedy třídu atributu na jedno použití více než jednou v deklaraci `Class1`.

Výraz `E` je *attribute_argument_expression* Pokud jsou splněny všechny následující příkazy:

*  Typ `E` je typ parametru atributu ([typy parametrů atributů](attributes.md#attribute-parameter-types)).
*  V době kompilace, hodnota `E` bylo možné přeložit na jednu z následujících akcí:
   * Konstantní hodnotu.
   * A `System.Type` objektu.
   * Jednorozměrné pole *attribute_argument_expression*s.

Příklad:

```csharp
using System;

[AttributeUsage(AttributeTargets.Class)]
public class TestAttribute: Attribute
{
    public int P1 {
        get {...}
        set {...}
    }

    public Type P2 {
        get {...}
        set {...}
    }

    public object P3 {
        get {...}
        set {...}
    }
}

[Test(P1 = 1234, P3 = new int[] {1, 3, 5}, P2 = typeof(float))]
class MyClass {}
```

A *typeof_expression* ([operátor typeof](expressions.md#the-typeof-operator)) použít jako výraz argumentu atributu může odkazovat neobecný typ, uzavřený konstruovaný typ. nebo nevázaný parametr generického typu, ale nemůže odkazovat Otevřete typu. Tím je zajištěno, že výraz lze vyřešit v době kompilace.

```csharp
class A: Attribute
{
    public A(Type t) {...}
}

class G<T>
{
    [A(typeof(T))] T t;                  // Error, open type in attribute
}

class X
{
    [A(typeof(List<int>))] int x;        // Ok, closed constructed type
    [A(typeof(List<>))] int y;           // Ok, unbound generic type
}
```

## <a name="attribute-instances"></a>Instance atributu

***Instance atributu*** je instanci, která představuje atribut v době běhu. Atribut je definován pomocí třídy atributu, poziční argumenty a pojmenované argumenty. Instance atributu je instancí třídy atributu, který je inicializován s poziční a pojmenované argumenty.

Načtení instance atributu zahrnuje kompilace a spuštění zpracování, jak je popsáno v následujících částech.

### <a name="compilation-of-an-attribute"></a>Kompilace atributu

Kompilace *atribut* třídou atributu `T`, *positional_argument_list* `P` a *named_argument_list* `N`, se skládá z následujících kroků:

*  Postupujte podle kroků zpracování kompilace pro kompilaci *object_creation_expression* formuláře `new T(P)`. Tyto kroky vyústí v chybu v době kompilace, nebo určit konstruktor instance `C` na `T` , který může být vyvolána v době běhu.
*  Pokud `C` nemá přístupnost public, dojde k chybě v době kompilace.
*  Pro každou *named_argument* `Arg` v `N`:
   * Umožní `Name` být *identifikátor* z *named_argument* `Arg`.
   * `Name` musíte určit o nestatická čtení a zápis veřejné pole ani vlastnost na `T`. Pokud `T` nemá žádný odpovídající pole nebo vlastnost, dojde k chybě v době kompilace.
*  Následující informace pro vytvoření instance za běhu atributu: třída atributů `T`, konstruktor instance `C` na `T`, *positional_argument_list* `P` a *named_argument_list* `N`.

### <a name="run-time-retrieval-of-an-attribute-instance"></a>Za běhu načítání instance atributu

Kompilace *atribut* vrací třídu atributu `T`, konstruktor instance `C` na `T`, *positional_argument_list* `P`a *named_argument_list* `N`. Tyto informace zadané, je možné načíst instanci atributu v době běhu pomocí následujících kroků:

*  Postupujte podle kroků zpracování za běhu pro spuštění *object_creation_expression* formuláře `new T(P)`, pomocí konstruktoru instance `C` počítáno v době kompilace. Tyto kroky za následek výjimku, nebo vytvořit instanci `O` z `T`.
*  Pro každou *named_argument* `Arg` v `N`, v pořadí:
   * Umožní `Name` být *identifikátor* z *named_argument* `Arg`. Pokud `Name` neidentifikuje nestatické veřejné čtení a zápis pole nebo vlastnost na `O`, pak je vyvolána výjimka.
   * Umožní `Value` být výsledek vyhodnocení výrazu *attribute_argument_expression* z `Arg`.
   * Pokud `Name` identifikuje pole na `O`, nastavte toto pole na `Value`.
   * V opačném případě `Name` identifikuje vlastnosti v `O`. Tuto vlastnost nastavte na `Value`.
   * Výsledkem je `O`, instance třídy atributu `T` , který byl inicializován s *positional_argument_list* `P` a *named_argument_list* `N`.

## <a name="reserved-attributes"></a>Vyhrazené atributy

Malý počet atributy ovlivňují jazyk nějakým způsobem. Tyto atributy zahrnout:

*  `System.AttributeUsageAttribute` ([Atribut the AttributeUsage](attributes.md#the-attributeusage-attribute)), který se používá k popisu způsoby, ve kterém můžete použít třídu atributu.
*  `System.Diagnostics.ConditionalAttribute` ([The podmíněný atribut](attributes.md#the-conditional-attribute)), který se používá k definování podmíněné metody.
*  `System.ObsoleteAttribute` ([The zastaralé atribut](attributes.md#the-obsolete-attribute)), který se používá k označení člena jako zastaralé.
*  `System.Runtime.CompilerServices.CallerLineNumberAttribute`, `System.Runtime.CompilerServices.CallerFilePathAttribute` a `System.Runtime.CompilerServices.CallerMemberNameAttribute` ([atributy informace o volajícím](attributes.md#caller-info-attributes)), které se používají k zadání informací o volání kontextu na volitelné parametry.

### <a name="the-attributeusage-attribute"></a>Atribut AttributeUsage

Atribut `AttributeUsage` se používá k popisu způsobu, ve kterém lze použít na třídu atributu.

Třída, která je upravena pomocí `AttributeUsage` atribut musí být odvozen od `System.Attribute`, buď přímo nebo nepřímo. V opačném případě dojde k chybě v době kompilace.

```csharp
namespace System
{
    [AttributeUsage(AttributeTargets.Class)]
    public class AttributeUsageAttribute: Attribute
    {
        public AttributeUsageAttribute(AttributeTargets validOn) {...}
        public virtual bool AllowMultiple { get {...} set {...} }
        public virtual bool Inherited { get {...} set {...} }
        public virtual AttributeTargets ValidOn { get {...} }
    }

    public enum AttributeTargets
    {
        Assembly     = 0x0001,
        Module       = 0x0002,
        Class        = 0x0004,
        Struct       = 0x0008,
        Enum         = 0x0010,
        Constructor  = 0x0020,
        Method       = 0x0040,
        Property     = 0x0080,
        Field        = 0x0100,
        Event        = 0x0200,
        Interface    = 0x0400,
        Parameter    = 0x0800,
        Delegate     = 0x1000,
        ReturnValue  = 0x2000,

        All = Assembly | Module | Class | Struct | Enum | Constructor | 
            Method | Property | Field | Event | Interface | Parameter | 
            Delegate | ReturnValue
    }
}
```

### <a name="the-conditional-attribute"></a>Atribut Conditional.

Atribut `Conditional` umožňuje definici ***podmíněné metody*** a ***atribut conditional třídy***.

```csharp
namespace System.Diagnostics
{
    [AttributeUsage(AttributeTargets.Method | AttributeTargets.Class, AllowMultiple = true)]
    public class ConditionalAttribute: Attribute
    {
        public ConditionalAttribute(string conditionString) {...}
        public string ConditionString { get {...} }
    }
}
```

#### <a name="conditional-methods"></a>Podmíněné metody.

Metody upravené pomocí `Conditional` atribut je podmíněná metoda. `Conditional` Atribut označuje podmínku otestováním symbol podmíněné kompilace. Volání podmíněné metody jsou zahrnuty nebo v závislosti na tom, zda je tento symbol definován místě volání vynechán. Pokud je definován symbol, je součástí; volání v opačném případě je vynechána, volání (včetně hodnocení příjemce a parametry volání).

Podmíněná metoda je v souladu s následujícími omezeními:

*  Podmíněná metoda musí být metoda ve *class_declaration* nebo *struct_declaration*. Pokud dojde k chybě kompilace `Conditional` je zadán atribut pro metodu v deklaraci rozhraní.
*  Podmíněná metoda musí mít typ vrácené hodnoty `void`.
*  Podmíněná metoda nesmí být označené `override` modifikátor. Podmíněná metoda může být označena s `virtual` modifikátor, ale. Přepsání metody jsou implicitně podmíněné a nesmí být označené explicitně `Conditional` atribut.
*  Podmíněná metoda nesmí být implementace metody rozhraní. V opačném případě dojde k chybě v době kompilace.

Kromě toho dojde k chybě kompilace, pokud podmíněná metoda se používá v *delegate_creation_expression*. V příkladu

```csharp
#define DEBUG

using System;
using System.Diagnostics;

class Class1 
{
    [Conditional("DEBUG")]
    public static void M() {
        Console.WriteLine("Executed Class1.M");
    }
}

class Class2
{
    public static void Test() {
        Class1.M();
    }
}
```

deklaruje `Class1.M` jako podmíněná metoda. `Class2`společnosti `Test` metoda volá tuto metodu. Protože podmíněné kompilace symbol `DEBUG` je definováno, pokud `Class2.Test` je volána, bude se volat `M`. Pokud se symbol `DEBUG` kdyby byla definována, pak `Class2.Test` by volat `Class1.M`.

Je důležité si uvědomit, že zahrnutí nebo vyloučení volání podmíněné metody se řídí symboly podmíněné kompilace místě volání. V příkladu

Soubor `class1.cs`:

```csharp
using System.Diagnostics;

class Class1 
{
    [Conditional("DEBUG")]
    public static void F() {
        Console.WriteLine("Executed Class1.F");
    }
}
```

Soubor `class2.cs`:

```csharp
#define DEBUG

class Class2
{
    public static void G() {
        Class1.F();                // F is called
    }
}
```

Soubor `class3.cs`:

```csharp
#undef DEBUG

class Class3
{
    public static void H() {
        Class1.F();                // F is not called
    }
}
```

třídy `Class2` a `Class3` každý obsahuje volání metodě podmíněného `Class1.F`, což je podmíněné podle, jestli `DEBUG` je definována. Protože je tento symbol definovaný v rámci `Class2` , ale ne `Class3`, volání `F` v `Class2` je zahrnuta, při volání `F` v `Class3` je vynechán.

Použití podmíněné metody v řetěz dědičnosti může být matoucí. Volání podmíněná metoda prostřednictvím `base`, formuláře `base.M`, jsou v souladu s pravidly volání normální podmíněná metoda. V příkladu

Soubor `class1.cs`:

```csharp
using System;
using System.Diagnostics;

class Class1 
{
    [Conditional("DEBUG")]
    public virtual void M() {
        Console.WriteLine("Class1.M executed");
    }
}
```

Soubor `class2.cs`:

```csharp
using System;

class Class2: Class1
{
    public override void M() {
        Console.WriteLine("Class2.M executed");
        base.M();                        // base.M is not called!
    }
}
```

Soubor `class3.cs`:

```csharp
#define DEBUG

using System;

class Class3
{
    public static void Test() {
        Class2 c = new Class2();
        c.M();                            // M is called
    }
}
```

`Class2` obsahuje volání `M` definované v její základní třídě. Toto volání je vynechána, protože podmíněné na základě přítomnosti symbolu je základní metoda `DEBUG`, který není definován. Díky tomu se metoda zapisuje do konzoly "`Class2.M executed`" pouze. Použití rozumné *pp_declaration*s můžete tyto problémy eliminovat.

#### <a name="conditional-attribute-classes"></a>Atribut Conditional třídy

Třídu atributu ([atribut třídy](attributes.md#attribute-classes)) dekorovaných s jedním nebo více `Conditional` je atributy ***třídě atribut conditional***. Třída atribut conditional je tedy přidružené symboly podmíněné kompilace deklarované v jeho `Conditional` atributy. V tomto příkladu:

```csharp
using System;
using System.Diagnostics;
[Conditional("ALPHA")]
[Conditional("BETA")]
public class TestAttribute : Attribute {}
```

deklaruje `TestAttribute` jako atribut conditional třídy přidružené symboly podmíněné kompilace `ALPHA` a `BETA`.

Atribut specifikace ([specifikace atributu](attributes.md#attribute-specification)) podmíněné atributu jsou zahrnuty, pokud jeden nebo více jeho symboly podmíněné kompilace přidružené je definována místě specifikace, jinak atribut specifikace je vynechán.

Je důležité si uvědomit, že zahrnutí nebo vyloučení specifikaci atributu třídy atribut conditional je řízen symboly podmíněné kompilace místě specifikaci. V příkladu

Soubor `test.cs`:

```csharp
using System;
using System.Diagnostics;

[Conditional("DEBUG")]

public class TestAttribute : Attribute {}
```

Soubor `class1.cs`:

```csharp
#define DEBUG

[Test]                // TestAttribute is specified

class Class1 {}
```

Soubor `class2.cs`:

```csharp
#undef DEBUG

[Test]                 // TestAttribute is not specified

class Class2 {}
```

třídy `Class1` a `Class2` jsou každý upravené pomocí atributu `Test`, což je podmíněné podle, jestli `DEBUG` je definována. Protože je tento symbol definovaný v rámci `Class1` , ale ne `Class2`, specifikaci `Test` atribut na `Class1` je zahrnuta při určení `Test` atribut na `Class2` je vynechán.

### <a name="the-obsolete-attribute"></a>Atribut zastaralé

Atribut `Obsolete` slouží k označení typy a členy typů, které by se už nebude používat.

```csharp
namespace System
{
    [AttributeUsage(
        AttributeTargets.Class | 
        AttributeTargets.Struct |
        AttributeTargets.Enum | 
        AttributeTargets.Interface | 
        AttributeTargets.Delegate |
        AttributeTargets.Method | 
        AttributeTargets.Constructor |
        AttributeTargets.Property | 
        AttributeTargets.Field |
        AttributeTargets.Event,
        Inherited = false)
    ]
    public class ObsoleteAttribute: Attribute
    {
        public ObsoleteAttribute() {...}
        public ObsoleteAttribute(string message) {...}
        public ObsoleteAttribute(string message, bool error) {...}
        public string Message { get {...} }
        public bool IsError { get {...} }
    }
}
```

Pokud program používá typ nebo člen, který je doplněn `Obsolete` atribut, kompilátor vyvolá upozornění nebo chybu. Konkrétně, kompilátor vyvolá upozornění-li zadán žádný parametr error, nebo pokud parametr error je k dispozici a má hodnotu `false`. Kompilátor vyvolá chybu, pokud je zadán parametr error a má hodnotu `true`.

V příkladu

```csharp
[Obsolete("This class is obsolete; use class B instead")]
class A
{
    public void F() {}
}

class B
{
    public void F() {}
}

class Test
{
    static void Main() {
        A a = new A();         // Warning
        a.F();
    }
}
```

Třída `A` je doplněn `Obsolete` atribut. Každé použití klíčového `A` v `Main` výsledky upozornění, která zahrnuje zadaná zpráva "Tato třída je zastaralá; Třída B místo toho použijte."

### <a name="caller-info-attributes"></a>Atributy informace o volajícím

Pro účely, jako je protokolování a vytváření sestav je někdy užitečné pro členské funkce určité kompilace informace o volajícím kódu. Atributy informace o volajícím poskytují způsob, jak transparentně předejte tyto informace.

Pokud volitelný parametr, je opatřen poznámkou jeden z atributů informace o volajícím, vynechání odpovídající argument ve volání nezpůsobí nutně výchozí hodnotu parametru nahrazena. Místo toho pokud je zadaných informací o volání kontextu k dispozici, tyto informace se předá jako hodnotu argumentu.

Příklad:

```csharp
using System.Runtime.CompilerServices

...

public void Log(
    [CallerLineNumber] int line = -1,
    [CallerFilePath]   string path = null,
    [CallerMemberName] string name = null
)
{
    Console.WriteLine((line < 0) ? "No line" : "Line "+ line);
    Console.WriteLine((path == null) ? "No file path" : path);
    Console.WriteLine((name == null) ? "No member name" : name);
}
```

Volání `Log()` bez argumentů by tisk řádku číslo a cestu k souboru volání, stejně jako název člena, ve kterém došlo k volání.

Atributy informace o volajícím se může vyskytnout u volitelné parametry kdekoli, včetně v deklaraci delegáta. Ale atributy informace o konkrétní volající mají omezení na typy parametrů, které se může atribut, tak, aby se vždy být proveden implicitní převod z nahrazenou hodnotu na typ parametru.

Jedná se o chybu v parametru definování a provádění části deklarace částečné metody nastavili stejný atribut informace o volajícím. Použijí se pouze atributy volající informace v části definující, vzhledem k tomu dochází pouze v části implementující atributy informace o volajícím jsou ignorovány.

Informace o subjektu volajícím nemá vliv na řešení přetížení. Jak s atributy volitelné parametry jsou vynechány stále ze zdrojového kódu volajícího, rozlišení přetížení ignoruje tyto parametry stejně, jako je ignorován jiné vynechaný volitelné parametry ([rozlišení přetěžování](expressions.md#overload-resolution)).

Informace o subjektu volajícím nahrazuje pouze při vyvolání funkce explicitně ve zdrojovém kódu. Implicitní vyvolání například volání konstruktoru nadřazené implicitní nemají zdrojové umístění a nebude nahradit informace o volajícím. Navíc nebudou volání, které jsou vázány dynamicky nahradit informace o volajícím. Informace o volajícím s atributy parametr se vynechá v takových případech, když se místo toho používá zadanou výchozí hodnotu parametru.

Jedinou výjimkou je výrazy dotazu. Tyto adresy jsou považované za syntaktické rozšíření, a pokud volání, rozbalte Pokud chcete vynechat, nechte volitelné parametry s atributy informace o volajícím, informace o subjektu volajícím bude nahrazena. Umístění používá je klauzule dotazu, který byl vytvořen volání.

Pokud na daný parametr není zadán více než jeden atribut informace o volajícím, jsou upřednostňovány v následujícím pořadí: `CallerLineNumber`, `CallerFilePath`, `CallerMemberName`.

#### <a name="the-callerlinenumber-attribute"></a>Atribut CallerLineNumber

`System.Runtime.CompilerServices.CallerLineNumberAttribute` Může po standardní implicitní převod na volitelné parametry ([standardní implicitní převody](conversions.md#standard-implicit-conversions)) z konstantní hodnoty `int.MaxValue` na typ parametru. Tím se zajistí, že jakékoli číslo řádku záporná až tuto hodnotu lze předat bez chyb.

Pokud volitelný parametr se vynechá volání funkce z umístění ve zdrojovém kódu `CallerLineNumberAttribute`, pak číselný literál, který představuje číslo řádku v tomto umístění se používá jako argument k vyvolání místo výchozí hodnotu parametru.

V případě vyvolání pokrývá více řádků, řádek zvolili je závislý na implementaci.

Všimněte si, že mohou být ovlivněny číslo řádku `#line` direktivy ([řádek direktivy](lexical-structure.md#line-directives)).

#### <a name="the-callerfilepath-attribute"></a>Atribut CallerFilePath

`System.Runtime.CompilerServices.CallerFilePathAttribute` Může po standardní implicitní převod na volitelné parametry ([standardní implicitní převody](conversions.md#standard-implicit-conversions)) z `string` na typ parametru.

Pokud volitelný parametr se vynechá volání funkce z umístění ve zdrojovém kódu `CallerFilePathAttribute`, pak Textový literál představuje cestu k souboru tohoto umístění se používá jako argument k vyvolání místo výchozí hodnotu parametru.

Formát cesty k souboru je závislý na implementaci.

Všimněte si, že cesta k souboru může být ovlivněno `#line` direktivy ([řádek direktivy](lexical-structure.md#line-directives)).

#### <a name="the-callermembername-attribute"></a>Atribut CallerMemberName

`System.Runtime.CompilerServices.CallerMemberNameAttribute` Může po standardní implicitní převod na volitelné parametry ([standardní implicitní převody](conversions.md#standard-implicit-conversions)) z `string` na typ parametru.

Pokud volání funkce z umístění v rámci těla členské funkce nebo v rámci atributu použitý pro členské funkce samotný nebo jeho návratový typ, parametry nebo parametry typu v vynechá zdrojového kódu s volitelným parametrem `CallerMemberNameAttribute`, o řetězcový literál představující název, se kterou se používá jako argument k vyvolání místo výchozí hodnotu parametru.

Pro volání, ke kterým dochází v rámci obecných metod se používá pouze samotný název metody bez seznamu parametrů typu.

Pro volání, ke kterým dochází v rámci implementace explicitního rozhraní člen se používá pouze samotný název metody bez kvalifikace předchozí rozhraní.

Pro vyvolání, ke kterým dochází v rámci přistupující objekty vlastnosti nebo události je použít název člena, vlastnosti nebo samotné události.

Pro volání, ke kterým dochází v rámci přístupových objektů indexer, použít název člena je dodaného `IndexerNameAttribute` ([atribut The IndexerName](attributes.md#the-indexername-attribute)) na člen indexer, pokud jsou k dispozici, nebo výchozí název `Item` jinak.

Název, který slouží pro vyvolání, ke kterým dochází v rámci deklarace konstruktory instancí, statické konstruktory, destruktory a operátory člen je závislý na implementaci.

## <a name="attributes-for-interoperation"></a>Atributy pro spolupráci

Poznámka: Tato část se vztahuje pouze na implementaci rozhraní Microsoft .NET C#.

### <a name="interoperation-with-com-and-win32-components"></a>Vzájemná spolupráce s komponentami COM a Win32

.NET runtime obsahuje velký počet atributů, které programu povolit programy jazyka C# pro spolupráci s komponenty napsané s využitím modelu COM a knihovny DLL systému Win32. Například `DllImport` atribut lze použít na `static extern` indikace, že implementace metody se nachází v knihovně DLL systému Win32. Tyto atributy jsou součástí `System.Runtime.InteropServices` obor názvů a podrobnou dokumentaci pro tyto atributy se nachází v dokumentaci k modulu runtime .NET.

### <a name="interoperation-with-other-net-languages"></a>Vzájemná spolupráce s jinými jazyky rozhraní .NET

#### <a name="the-indexername-attribute"></a>Atribut IndexerName

Indexery jsou implementovány v .NET pomocí indexované vlastnosti a mít název v metadatech .NET. Pokud ne `IndexerName` atribut je k dispozici pro indexer a pak název `Item` se používá ve výchozím nastavení. `IndexerName` Atribut umožňuje vývojář, abyste mohli přepsat toto výchozí nastavení a zadejte jiný název.

```csharp
namespace System.Runtime.CompilerServices.CSharp
{
    [AttributeUsage(AttributeTargets.Property)]
    public class IndexerNameAttribute: Attribute
    {
        public IndexerNameAttribute(string indexerName) {...}
        public string Value { get {...} } 
    }
}
```
