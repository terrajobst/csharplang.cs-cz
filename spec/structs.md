# <a name="structs"></a>Struktury

Struktury jsou podobné třídy, které představují datové struktury, které mohou obsahovat datové členy a funkční členy. Na rozdíl od tříd však struktury jsou typy hodnot a nevyžadují přidělení haldy. Proměnné typu Struktura přímo obsahuje datové struktury, že proměnné typu třídy obsahuje odkaz na data, druhá možnost známé jako objekt.

Struktury jsou zvláště užitečná pro malé datové struktury, které mají hodnotu sémantiku. Komplexní čísla, body v souřadnicovém systému nebo páry klíč hodnota do slovníku jsou všechny dobrým příkladem struktury. Je klíč pro tyto datové struktury, ke kterým mají několik datových členů, nevyžadují použití dědičnosti nebo referenční identity a že můžete pohodlně prováděny pomocí sémantiky hodnota, kde přiřazení kopíruje hodnotu namísto odkazu.

Jak je popsáno v [jednoduché typy](types.md#simple-types), jednoduché typy, které jsou k dispozici v jazyce C#, jako například `int`, `double`, a `bool`, jsou ve skutečnosti všechny typy struktury. Stejně jako tyto předdefinované typy jsou struktury, je také možné použít struktury a přetěžování pro implementaci nový "základní" typy v jazyce C#. Na konci této kapitole jsou uvedeny dva příklady těchto typů ([struktura příklady](structs.md#struct-examples)).

## <a name="struct-declarations"></a>Deklarace struktury

A *struct_declaration* je *type_declaration* ([typ deklarace](namespaces.md#type-declarations)), který deklaruje novou strukturu:

```antlr
struct_declaration
    : attributes? struct_modifier* 'partial'? 'struct' identifier type_parameter_list?
      struct_interfaces? type_parameter_constraints_clause* struct_body ';'?
    ;
```

A *struct_declaration* se skládá z volitelné sadu *atributy* ([atributy](attributes.md)) následovaný volitelná sada *struct_modifier*s ([struktura modifikátory](structs.md#struct-modifiers)), následovaným volitelnou `partial` modifikátor, za nímž následuje klíčové slovo `struct` a *identifikátor* , která pojmenuje struktury, za nímž následuje volitelné *type_parameter_list* specifikace ([parametry typu](classes.md#type-parameters)), následovaným volitelnou *struct_interfaces* specifikace ([Částečný modifikátor](structs.md#partial-modifier))), následovaným volitelnou *type_parameter_constraints_clause*s specifikace ([omezení parametru typu](classes.md#type-parameter-constraints)) a po něm *struct_body* ([struktury textu](structs.md#struct-body)), volitelně za nímž následuje středníkem.

### <a name="struct-modifiers"></a>Modifikátory – struktura

A *struct_declaration* může volitelně zahrnovat posloupnost modifikátory struktury:

```antlr
struct_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    | struct_modifier_unsafe
    ;
```

Je chyba kompilace pro stejný modifikátor objevit více než jednou v deklaraci struktury.

Modifikátory deklarace struktury mají stejný význam jako deklarace třídy ([třídy deklarací](classes.md#class-declarations)).

### <a name="partial-modifier"></a>Částečný modifikátor

`partial` Modifikátor znamená, že to *struct_declaration* je částečný typ deklarace. Více deklaracích částečné struktury se stejným názvem v rámci nadřazeného oboru názvů nebo typ deklarace se dá tvoří jednu deklaraci struktury, dle pravidel uvedených v [částečné typy](classes.md#partial-types).

### <a name="struct-interfaces"></a>Struktura rozhraní

Může obsahovat deklaraci struktury *struct_interfaces* specifikace, ve kterém případ struct se říká, že přímo implementaci typů dané rozhraní.

```antlr
struct_interfaces
    : ':' interface_type_list
    ;
```

Implementace rozhraní jsou popsány dále v [rozhraní implementace](interfaces.md#interface-implementations).

### <a name="struct-body"></a>Text – struktura

*Struct_body* struktury definuje členy struktury.

```antlr
struct_body
    : '{' struct_member_declaration* '}'
    ;
```

## <a name="struct-members"></a>Členy struktury

Členové struktury obsahovat členy zavedených v jeho *struct_member_declaration*s a členy zděděné z typu `System.ValueType`.

```antlr
struct_member_declaration
    : constant_declaration
    | field_declaration
    | method_declaration
    | property_declaration
    | event_declaration
    | indexer_declaration
    | operator_declaration
    | constructor_declaration
    | static_constructor_declaration
    | type_declaration
    | struct_member_declaration_unsafe
    ;
```

S výjimkou rozdílů, které jste si poznamenali v [třídou a strukturou rozdíly](structs.md#class-and-struct-differences), popisy členy třídy, které jsou součástí [členy třídy](classes.md#class-members) prostřednictvím [iterátory](classes.md#iterators) použít – struktura také členy.

## <a name="class-and-struct-differences"></a>Třídy a struktury rozdíly

Struktury se liší od tříd v několika důležitých směrech –:

*  Struktury jsou typy hodnot ([hodnota sémantiku](structs.md#value-semantics)).
*  Všechny typy struktury implicitně dědí z třídy `System.ValueType` ([dědičnosti](structs.md#inheritance)).
*  Přiřazení proměnné typu Struktura se vytvoří kopie přiřazené hodnoty ([přiřazení](structs.md#assignment)).
*  Výchozí hodnota struktury je hodnotu vytvořenou testovaným nastavení všechna pole typu hodnota na výchozí hodnoty a referenční dokumentace všech polí typu `null` ([výchozí hodnoty](structs.md#default-values)).
*  Operace zabalení a rozbalení se používají pro převod mezi typy struktury a `object` ([zabalení a rozbalení](structs.md#boxing-and-unboxing)).
*  Význam `this` se liší pro struktury ([tento přístup](expressions.md#this-access)).
*  Zahrnout proměnné inicializátory nejsou povolené instance pole deklarace pro strukturu ([Inicializátory pole](structs.md#field-initializers)).
*  Struktura není povolená pro deklaraci konstruktor instance bez parametrů ([konstruktory](structs.md#constructors)).
*  Chcete-li deklarovat destruktor není povolená struktury ([destruktory](structs.md#destructors)).

### <a name="value-semantics"></a>Sémantika hodnoty

Struktury jsou typy hodnot ([typů hodnot](types.md#value-types)) a se říká, že mají hodnotu sémantiku. Třídy, na druhé straně jsou odkazové typy ([referenční typy](types.md#reference-types)) a mají často odkazové sémantiky.

Proměnné typu Struktura přímo obsahuje datové struktury, že proměnné typu třídy obsahuje odkaz na data, druhá možnost známé jako objekt. Pokud struktura `B` obsahuje pole instance typu `A` a `A` je typ struktury je chyba kompilace pro `A` závisí na `B` nebo typ vytvořený z `B`. Struktura `X` ***přímo závisí na*** struktura `Y` Pokud `X` obsahuje pole instance typu `Y`. Při této definici, kompletní sadu struktury, na kterém závisí struktura je přenositelný uzavření ***přímo závisí na*** vztah.  Příklad
```csharp
struct Node
{
    int data;
    Node next; // error, Node directly depends on itself
}
```
se o chybu, protože `Node` obsahuje pole instance vlastního typu.  Další příklad
```csharp
struct A { B b; }

struct B { C c; }

struct C { A a; }
```
se o chybu, protože jednotlivé typy `A`, `B`, a `C` závisí na sebe navzájem.

Pomocí třídy je možné pro dvě proměnné odkazovat na stejný objekt a proto možná pro operace v rámci jedné proměnné ovlivňovat objekt odkazovaný jinou proměnnou. Struktury, proměnné každý mají své vlastní kopii dat (s výjimkou v případě třídy `ref` a `out` proměnných parametrů), a není možné pro operace se na nich se má vliv na jinou. Navíc vzhledem k tomu struktury nejsou typy odkazů, není možné pro hodnoty typu struktura bude `null`.

Zadané deklarace
```csharp
struct Point
{
    public int x, y;

    public Point(int x, int y) {
        this.x = x;
        this.y = y;
    }
}
```
fragment kódu
```csharp
Point a = new Point(10, 10);
Point b = a;
a.x = 100;
System.Console.WriteLine(b.x);
```
Vypíše hodnotu `10`. Přiřazení `a` k `b` vytvoří kopii hodnoty, a `b` je tedy nebudou výpadkem ovlivněny přiřazení `a.x`. Měl `Point` místo toho byla deklarována jako třída, výstup by měl `100` protože `a` a `b` by odkazovat na stejný objekt.

### <a name="inheritance"></a>Dědičnost

Všechny typy struktury implicitně dědí z třídy `System.ValueType`, který zase dědí z třídy `object`. Deklarace struktury mohou zadat seznamu implementovaných rozhraní, ale není možné určit základní třídu pro deklaraci struktury.

Typy struktury jsou nikdy abstraktní a jsou implicitně jsou vždycky zapečetěné. `abstract` a `sealed` proto nejsou povolené modifikátory. v deklaraci struktury.

Protože dědičnost se nepodporuje pro struktury, nemůže být deklarovaná přístupnost člena struktury `protected` nebo `protected internal`.

Funkce členy struktury nemůžou být `abstract` nebo `virtual`a `override` Modifikátor je povolen pouze k přepsání metod zděděných z `System.ValueType`.

### <a name="assignment"></a>Přiřazení

Přiřazení proměnné typu Struktura vytvoří kopii přiřazené hodnoty. Tím se liší od přiřazení k proměnné typu třídy, která kopíruje odkaz, ale ne identifikovaný odkaz na objekt.

Podobně jako přiřazení, pokud struktury je předán jako parametr hodnoty nebo vrátí jako výsledek členské funkce, je vytvořena kopie struktury. Struktura může být předány podle odkazu na člen funkce pomocí `ref` nebo `out` parametru.

Pokud vlastnost nebo indexovací člen struktury je cílem přiřazení, musí být výraz instance spojené s přístupem k vlastnosti nebo indexeru zařazeny jako proměnnou. Pokud výraz instance je klasifikován tak hodnotu, dojde k chybě v době kompilace. To je podrobně popsán dále v [jednoduché přiřazení](expressions.md#simple-assignment).

### <a name="default-values"></a>Výchozí hodnoty

Jak je popsáno v [výchozí hodnoty](variables.md#default-values), několik druhů proměnné jsou automaticky inicializovány na jejich výchozí hodnota při jejich vytváření. Pro proměnné typů třídy a další typy odkazů, tato výchozí hodnota je `null`. Ale protože struktury jsou typy hodnot, které nelze `null`, výchozí hodnota struktury je hodnotu vytvořenou testovaným nastavení všechna pole typu hodnota na výchozí hodnoty a referenční dokumentace všech polí typu `null`.

Odkazující na `Point` struktury deklarovaného výše v příkladu
```csharp
Point[] a = new Point[100];
```
Inicializuje každý `Point` jako pole k hodnotě vytvořený tak, že nastavíte `x` a `y` pole na hodnotu nula.

Výchozí hodnota struktury odpovídá hodnotě vrácené výchozí konstruktor třídy struktury ([výchozí konstruktory](types.md#default-constructors)). Na rozdíl od třídy struktury není oprávněn deklarovat konstruktor instance bez parametrů. Místo toho každých struktura má implicitně konstruktor instance bez parametrů, která vždy vrátí hodnotu, která je výsledkem nastavení všechna pole typu hodnota na výchozí hodnoty a referenční dokumentace všech polí typu `null`.

Struktury by se měly navrhovat vzít v úvahu výchozího stavu inicializace platném stavu. V příkladu
```csharp
using System;

struct KeyValuePair
{
    string key;
    string value;

    public KeyValuePair(string key, string value) {
        if (key == null || value == null) throw new ArgumentException();
        this.key = key;
        this.value = value;
    }
}
```
uživatelské instance konstruktoru chrání proti hodnoty null, pouze pokud je explicitně volána. V případech, kde `KeyValuePair` proměnná je v souladu s výchozí hodnotou inicializace `key` a `value` pole bude mít hodnotu null a struct musí být připravena ke zpracování tohoto stavu.

### <a name="boxing-and-unboxing"></a>Zabalení a rozbalení

Hodnotu typu třídy lze převést na typ `object` nebo k typu rozhraní, která je implementována ve třídě jednoduše díky tomu, že odkaz na jiný typ v době kompilace. Obdobně hodnotu typu `object` nebo hodnotu rozhraní typu lze převést zpět na typ třídy beze změny odkaz (ale samozřejmě typu modulu runtime je vyžadována kontrola v tomto případě).

Protože struktury nejsou typy odkazů, jsou tyto operace pro typy struktury implementováno jinak. Pokud je hodnota typu Struktura převést na typ `object` nebo na typ rozhraní implementovaný struktury zabalení operace probíhá. Podobně když je hodnota typu `object` nebo hodnotu typu rozhraní je převést zpět na typu Struktura, probíhá operace rozbalení. Klíčovým rozdílem mezi ze stejné operace na typy tříd je, že zabalení a rozbalení zkopírujete příslušnou hodnotu struktury do nebo z pevně určené instance. Proto po operaci zabalení a rozbalení provedené změny nezabalené struktury se neprojeví v zabalený struktury.

Při přepsání typu Struktura virtuální metody zděděné z `System.Object` (například `Equals`, `GetHashCode`, nebo `ToString`), volání virtuální metody prostřednictvím instance typu Struktura nezpůsobí zabalení dochází. To platí i v případě, že struktura slouží jako parametr typu a dojde k vyvolání prostřednictvím instance typu parametru typu. Příklad:
```csharp
using System;

struct Counter
{
    int value;

    public override string ToString() {
        value++;
        return value.ToString();
    }
}

class Program
{
    static void Test<T>() where T: new() {
        T x = new T();
        Console.WriteLine(x.ToString());
        Console.WriteLine(x.ToString());
        Console.WriteLine(x.ToString());
    }

    static void Main() {
        Test<Counter>();
    }
}
```

Výstup programu je:
```
1
2
3
```

I když je špatný styl `ToString` mít vedlejší účinky, příklad ukazuje, že došlo k žádné zabalení pro tři volání `x.ToString()`.

Podobně nikdy implicitně zabalení dochází při přístupu k členovi na parametr typu s omezením. Předpokládejme například, že rozhraní `ICounter` obsahuje metodu `Increment` který můžete použít třeba hodnotu změnit. Pokud `ICounter` je použitý jako omezení, provádění `Increment` metoda je volána s odkazem na proměnnou, která `Increment` byla volána pro nikdy zabalený kopírování.

```csharp
using System;

interface ICounter
{
    void Increment();
}

struct Counter: ICounter
{
    int value;

    public override string ToString() {
        return value.ToString();
    }

    void ICounter.Increment() {
        value++;
    }
}

class Program
{
    static void Test<T>() where T: ICounter, new() {
        T x = new T();
        Console.WriteLine(x);
        x.Increment();                    // Modify x
        Console.WriteLine(x);
        ((ICounter)x).Increment();        // Modify boxed copy of x
        Console.WriteLine(x);
    }

    static void Main() {
        Test<Counter>();
    }
}
```

První volání `Increment` změní hodnoty v proměnné `x`. Toto není ekvivalentní k druhé volání `Increment`, které mění hodnotu v zabalený kopie `x`. Proto je výstup programu:
```
0
1
1
```

Další podrobnosti o zabalení a rozbalení najdete [zabalení a rozbalení](types.md#boxing-and-unboxing).

### <a name="meaning-of-this"></a>Význam tohoto objektu

V rámci konstruktor instance nebo instance funkce člena třídy `this` klasifikovaný jako hodnotu. Proto při `this` slouží k odkazování na instanci pro který byla vyvolána členské funkce, není možné přiřadit `this` v členské funkce třídy.

V rámci konstruktoru instance struktury `this` odpovídá `out` parametr typu Struktura a v rámci funkce členem instance struktury, `this` odpovídá `ref` parametr typu Struktura. V obou případech `this` je klasifikován jako proměnnou, a je možné změnit celé struktury, pro kterou členské funkce se vyvolala přiřazením `this` nebo předáním jako `ref` nebo `out` parametru.

### <a name="field-initializers"></a>Inicializátory pole

Jak je popsáno v [výchozí hodnoty](structs.md#default-values), výchozí hodnota struktury se skládá z hodnotu, která je výsledkem nastavení všechna pole typu hodnota na výchozí hodnoty a referenční dokumentace všech polí typu `null`. Z tohoto důvodu struktura neumožňuje deklarace pole instance na obsahovat inicializátory proměnné. Toto omezení platí pouze pro pole instance. Statická pole struktury mohou obsahovat inicializátory proměnné.

V příkladu
```csharp
struct Point
{
    public int x = 1;  // Error, initializer not permitted
    public int y = 1;  // Error, initializer not permitted
}
```
je v chybě, protože deklarace pole instance obsahovat inicializátory proměnné.

### <a name="constructors"></a>Konstruktory

Na rozdíl od třídy struktury není oprávněn deklarovat konstruktor instance bez parametrů. Místo toho každých struktury implicitně obsahuje konstruktor instance bez parametrů, která vždy vrátí hodnotu, která je výsledkem nastavení všechna pole na výchozí hodnoty a referenční dokumentace všech typ pole na hodnotu null ([výchozí konstruktory](types.md#default-constructors)). Struktury můžete deklarovat konstruktory instancí s parametry. Příklad
```csharp
struct Point
{
    int x, y;

    public Point(int x, int y) {
        this.x = x;
        this.y = y;
    }
}
```

Uvedené výše uvedené prohlášení, příkazy
```csharp
Point p1 = new Point();
Point p2 = new Point(0, 0);
```
obě vytváří `Point` s `x` a `y` inicializována na nulovou hodnotu.

Konstruktor instance struktury není dovoleno zahrnují inicializátoru konstruktoru formuláře `base(...)`.

Pokud konstruktor instance struktury neurčuje inicializátoru konstruktoru `this` odpovídá proměnné `out` parametr typu struktury a podobně jako `out` parametr, `this` musí být jednoznačně přiřazena () [Jednoznačného přiřazení](variables.md#definite-assignment)) na jakémkoliv místě, kde konstruktor vrátí. Pokud konstruktor instance struktury určuje inicializátoru konstruktoru `this` odpovídá proměnné `ref` parametr typu struktury a podobně jako `ref` parametr, `this` se považuje za jednoznačně přiřazené v vstupem do těla konstruktoru. Zvažte implementaci konstruktoru instance níže:
```csharp
struct Point
{
    int x, y;

    public int X {
        set { x = value; }
    }

    public int Y {
        set { y = value; }
    }

    public Point(int x, int y) {
        X = x;        // error, this is not yet definitely assigned
        Y = y;        // error, this is not yet definitely assigned
    }
}
```

Žádné členskou funkci instance (včetně přístupové objekty set vlastnosti `X` a `Y`) nelze volat, dokud všechna pole struktury vytváří jednoznačně přiřazena. Jedinou výjimkou zahrnuje automaticky implementované vlastnosti ([automaticky implementované vlastnosti](classes.md#automatically-implemented-properties)). Pravidla jednoznačného přiřazení ([jednoduché přiřazení výrazy](variables.md#simple-assignment-expressions)) konkrétně s výjimkou přiřazení na automatickou vlastnost typu struktury v rámci konstruktoru instance tohoto typu struktury: taková přiřazení se považuje za jednoznačného přiřazení skryté pomocným polem vlastnosti automaticky. Proto následující může:

```csharp
struct Point
{
    public int X { get; set; }
    public int Y { get; set; }

    public Point(int x, int y) {
        X = x;      // allowed, definitely assigns backing field
        Y = y;      // allowed, definitely assigns backing field
    }
```

### <a name="destructors"></a>Destruktory

Chcete-li deklarovat destruktor není povolena struktury.

### <a name="static-constructors"></a>Statické konstruktory

Statické konstruktory pro struktury pomocí většiny stejná pravidla jako u třídy. Provedení statický konstruktor pro typ struktury se aktivuje první z následujících událostí v rámci domény aplikace:

*  Statický člen typu Struktura se odkazuje.
*  Explicitně deklarované konstruktoru typu Struktura se nazývá.

Vytváření výchozích hodnot ([výchozí hodnoty](structs.md#default-values)) struktury typy neaktivuje statický konstruktor. (Příkladem je počáteční hodnota elementů v matici.)

## <a name="struct-examples"></a>Příklady – struktura

Následující příklad zobrazuje dvě důležité příklady použití `struct` typy a vytvoří typy, které je možné podobně předdefinované typy jazyka, ale s upravenou sémantiku.

### <a name="database-integer-type"></a>Celočíselný typ databáze

`DBInt` Celočíselného typu, který může představovat úplnou sadu hodnot, které implementuje strukturu níže `int` typu a navíc další stav, který určuje neznámou hodnotu. Typ s těmito charakteristikami se běžně používá v databázích.

```csharp
using System;

public struct DBInt
{
    // The Null member represents an unknown DBInt value.

    public static readonly DBInt Null = new DBInt();

    // When the defined field is true, this DBInt represents a known value
    // which is stored in the value field. When the defined field is false,
    // this DBInt represents an unknown value, and the value field is 0.

    int value;
    bool defined;

    // Private instance constructor. Creates a DBInt with a known value.

    DBInt(int value) {
        this.value = value;
        this.defined = true;
    }

    // The IsNull property is true if this DBInt represents an unknown value.

    public bool IsNull { get { return !defined; } }

    // The Value property is the known value of this DBInt, or 0 if this
    // DBInt represents an unknown value.

    public int Value { get { return value; } }

    // Implicit conversion from int to DBInt.

    public static implicit operator DBInt(int x) {
        return new DBInt(x);
    }

    // Explicit conversion from DBInt to int. Throws an exception if the
    // given DBInt represents an unknown value.

    public static explicit operator int(DBInt x) {
        if (!x.defined) throw new InvalidOperationException();
        return x.value;
    }

    public static DBInt operator +(DBInt x) {
        return x;
    }

    public static DBInt operator -(DBInt x) {
        return x.defined ? -x.value : Null;
    }

    public static DBInt operator +(DBInt x, DBInt y) {
        return x.defined && y.defined? x.value + y.value: Null;
    }

    public static DBInt operator -(DBInt x, DBInt y) {
        return x.defined && y.defined? x.value - y.value: Null;
    }

    public static DBInt operator *(DBInt x, DBInt y) {
        return x.defined && y.defined? x.value * y.value: Null;
    }

    public static DBInt operator /(DBInt x, DBInt y) {
        return x.defined && y.defined? x.value / y.value: Null;
    }

    public static DBInt operator %(DBInt x, DBInt y) {
        return x.defined && y.defined? x.value % y.value: Null;
    }

    public static DBBool operator ==(DBInt x, DBInt y) {
        return x.defined && y.defined? x.value == y.value: DBBool.Null;
    }

    public static DBBool operator !=(DBInt x, DBInt y) {
        return x.defined && y.defined? x.value != y.value: DBBool.Null;
    }

    public static DBBool operator >(DBInt x, DBInt y) {
        return x.defined && y.defined? x.value > y.value: DBBool.Null;
    }

    public static DBBool operator <(DBInt x, DBInt y) {
        return x.defined && y.defined? x.value < y.value: DBBool.Null;
    }

    public static DBBool operator >=(DBInt x, DBInt y) {
        return x.defined && y.defined? x.value >= y.value: DBBool.Null;
    }

    public static DBBool operator <=(DBInt x, DBInt y) {
        return x.defined && y.defined? x.value <= y.value: DBBool.Null;
    }

    public override bool Equals(object obj) {
        if (!(obj is DBInt)) return false;
        DBInt x = (DBInt)obj;
        return value == x.value && defined == x.defined;
    }

    public override int GetHashCode() {
        return value;
    }

    public override string ToString() {
        return defined? value.ToString(): "DBInt.Null";
    }
}
```

### <a name="database-boolean-type"></a>Logický typ databáze

`DBBool` Níže struktura implementuje logický typ s hodnotou tři. Možné hodnoty tohoto typu jsou `DBBool.True`, `DBBool.False`, a `DBBool.Null`, kde `Null` člen určuje neznámou hodnotu. Tyto tři vracející logické typy se běžně používají v databázích.

```csharp
using System;

public struct DBBool
{
    // The three possible DBBool values.

    public static readonly DBBool Null = new DBBool(0);
    public static readonly DBBool False = new DBBool(-1);
    public static readonly DBBool True = new DBBool(1);

    // Private field that stores -1, 0, 1 for False, Null, True.

    sbyte value;

    // Private instance constructor. The value parameter must be -1, 0, or 1.

    DBBool(int value) {
        this.value = (sbyte)value;
    }

    // Properties to examine the value of a DBBool. Return true if this
    // DBBool has the given value, false otherwise.

    public bool IsNull { get { return value == 0; } }

    public bool IsFalse { get { return value < 0; } }

    public bool IsTrue { get { return value > 0; } }

    // Implicit conversion from bool to DBBool. Maps true to DBBool.True and
    // false to DBBool.False.

    public static implicit operator DBBool(bool x) {
        return x? True: False;
    }

    // Explicit conversion from DBBool to bool. Throws an exception if the
    // given DBBool is Null, otherwise returns true or false.

    public static explicit operator bool(DBBool x) {
        if (x.value == 0) throw new InvalidOperationException();
        return x.value > 0;
    }

    // Equality operator. Returns Null if either operand is Null, otherwise
    // returns True or False.

    public static DBBool operator ==(DBBool x, DBBool y) {
        if (x.value == 0 || y.value == 0) return Null;
        return x.value == y.value? True: False;
    }

    // Inequality operator. Returns Null if either operand is Null, otherwise
    // returns True or False.

    public static DBBool operator !=(DBBool x, DBBool y) {
        if (x.value == 0 || y.value == 0) return Null;
        return x.value != y.value? True: False;
    }

    // Logical negation operator. Returns True if the operand is False, Null
    // if the operand is Null, or False if the operand is True.

    public static DBBool operator !(DBBool x) {
        return new DBBool(-x.value);
    }

    // Logical AND operator. Returns False if either operand is False,
    // otherwise Null if either operand is Null, otherwise True.

    public static DBBool operator &(DBBool x, DBBool y) {
        return new DBBool(x.value < y.value? x.value: y.value);
    }

    // Logical OR operator. Returns True if either operand is True, otherwise
    // Null if either operand is Null, otherwise False.

    public static DBBool operator |(DBBool x, DBBool y) {
        return new DBBool(x.value > y.value? x.value: y.value);
    }

    // Definitely true operator. Returns true if the operand is True, false
    // otherwise.

    public static bool operator true(DBBool x) {
        return x.value > 0;
    }

    // Definitely false operator. Returns true if the operand is False, false
    // otherwise.

    public static bool operator false(DBBool x) {
        return x.value < 0;
    }

    public override bool Equals(object obj) {
        if (!(obj is DBBool)) return false;
        return value == ((DBBool)obj).value;
    }

    public override int GetHashCode() {
        return value;
    }

    public override string ToString() {
        if (value > 0) return "DBBool.True";
        if (value < 0) return "DBBool.False";
        return "DBBool.Null";
    }
}
```
