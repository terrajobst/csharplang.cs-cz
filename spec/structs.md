---
ms.openlocfilehash: 6dd1dde67597b2125de9a1aa2fab9144128d533f
ms.sourcegitcommit: 892af9016b3317a8fae12d195014dc38ba51cf16
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/01/2019
ms.locfileid: "71704032"
---
# <a name="structs"></a>Struktury

Struktury jsou podobné třídám, které představují datové struktury, které mohou obsahovat datové členy a členy funkce. Nicméně na rozdíl od tříd, struktury jsou typy hodnot a nevyžadují přidělení haldy. Proměnná typu struktury přímo obsahuje data struktury, zatímco proměnná typu třídy obsahuje odkaz na data, druhá je známá jako objekt.

Struktury jsou zvláště užitečné pro malé datové struktury, které mají sémantiku hodnot. Komplexní čísla, body v systému souřadnic nebo páry klíč-hodnota ve slovníku jsou všechny dobrými příklady struktur. Klíč k těmto datovým strukturám je, že mají několik datových členů, které nevyžadují použití dědičnosti nebo referenční identity, a že je lze pohodlně implementovat pomocí sémantiky hodnot, kde přiřazení kopíruje hodnotu namísto odkazu.

Jak je popsáno v [jednoduchých typech](types.md#simple-types), jednoduché typy poskytované C#, například `int`, `double` a `bool`, jsou ve skutečnosti všechny typy struktury. Stejně jako tyto předdefinované typy jsou struktury, lze také použít struktury a přetížení operátoru pro implementaci nových primitivních typů v C# jazyce. Dva příklady těchto typů jsou uvedeny na konci této kapitoly ([Příklady struktury](structs.md#struct-examples)).

## <a name="struct-declarations"></a>Deklarace struktury

*Struct_declaration* je *type_declaration* ([deklarace typu](namespaces.md#type-declarations)), který deklaruje novou strukturu:

```antlr
struct_declaration
    : attributes? struct_modifier* 'partial'? 'struct' identifier type_parameter_list?
      struct_interfaces? type_parameter_constraints_clause* struct_body ';'?
    ;
```

*Struct_declaration* se skládá z volitelné sady *atributů* ([atributů](attributes.md)), následované volitelnou sadou *struct_modifier*s ([modifikátory struktury](structs.md#struct-modifiers)) následovaným volitelným modifikátorem `partial` následovaným nastavením klíčové slovo `struct` a *identifikátor* , který název struktury, následované volitelnou specifikací *Type_parameter_list* ([parametry typu](classes.md#type-parameters)) následovaný volitelnou specifikací *struct_interfaces* ([částečný Modifikátor](structs.md#partial-modifier))) následovaný volitelnou specifikací *type_parameter_constraints_clause*s ([omezeními parametrů typu](classes.md#type-parameter-constraints)) následovaným *struct_body* ([tělo struktury](structs.md#struct-body)), volitelně následovaný středníkem.

### <a name="struct-modifiers"></a>Modifikátory struktury

*Struct_declaration* může volitelně zahrnovat posloupnost modifikátorů struktury:

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

Jedná se o chybu při kompilaci, aby se stejný modifikátor zobrazoval víckrát v deklaraci struktury.

Modifikátory deklarace struktury mají stejný význam jako deklarace třídy ([deklarace tříd](classes.md#class-declarations)).

### <a name="partial-modifier"></a>Částečný modifikátor

Modifikátor `partial` označuje, že toto *struct_declaration* je částečná deklarace typu. Vícenásobná deklarace částečné struktury se stejným názvem v rámci ohraničujícího oboru názvů nebo deklarace typu jsou zkombinovány o jednu deklaraci struktury za základě pravidel určených v [částečných typech](classes.md#partial-types).

### <a name="struct-interfaces"></a>Rozhraní struktury

Deklarace struktury může zahrnovat specifikaci *struct_interfaces* . v takovém případě je tato struktura určena k přímé implementaci daných typů rozhraní.

```antlr
struct_interfaces
    : ':' interface_type_list
    ;
```

Implementace rozhraní jsou podrobněji popsány v [implementacích rozhraní](interfaces.md#interface-implementations).

### <a name="struct-body"></a>Tělo struktury

*Struct_body* struktury definuje členy struktury.

```antlr
struct_body
    : '{' struct_member_declaration* '}'
    ;
```

## <a name="struct-members"></a>Členové struktury

Členy struktury se skládají ze členů zavedených jeho *struct_member_declaration*s a členy zděděnými z typu `System.ValueType`.

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

S výjimkou rozdílů uvedených v [rozdílech třídy a struktury](structs.md#class-and-struct-differences)jsou popisy členů třídy, které jsou zadány ve [členech třídy](classes.md#class-members) pomocí [iterátorů](classes.md#iterators) , použity také pro členy struktury.

## <a name="class-and-struct-differences"></a>Rozdíly třídy a struktury

Struktury se liší od tříd v několika důležitých způsobech:

*  Struktury jsou typy hodnot ([sémantika hodnot](structs.md#value-semantics)).
*  Všechny typy struktury implicitně dědí z třídy `System.ValueType` ([Dědičnost](structs.md#inheritance)).
*  Přiřazení k proměnné typu struktury vytvoří kopii přiřazené hodnoty ([přiřazení](structs.md#assignment)).
*  Výchozí hodnota struktury je hodnota vytvořená nastavením všech polí Typ hodnoty na jejich výchozí hodnotu a všechna pole s odkazem na `null` ([výchozí hodnoty](structs.md#default-values)).
*  Operace zabalení a rozbalení se používají k převodu mezi typem struktury a `object` (zabalení[a rozbalení](structs.md#boxing-and-unboxing)).
*  Význam `this` se pro struktury ([Tento přístup](expressions.md#this-access)) liší.
*  Deklarace polí instance pro strukturu nejsou povoleny pro zahrnutí inicializátorů proměnných ([Inicializátory pole](structs.md#field-initializers)).
*  Struktura není povolená pro deklaraci konstruktoru instance bez parametrů ([konstruktory](structs.md#constructors)).
*  Struktura není oprávněná deklarovat destruktor ([destruktory](structs.md#destructors)).

### <a name="value-semantics"></a>Sémantika hodnoty

Struktury jsou typy hodnot ([typy hodnot](types.md#value-types)) a jsou označeny jako sémantika hodnoty. Třídy jsou na druhé straně odkazy na typy ([odkazové typy](types.md#reference-types)) a jsou označeny jako referenční sémantika.

Proměnná typu struktury přímo obsahuje data struktury, zatímco proměnná typu třídy obsahuje odkaz na data, druhá je známá jako objekt. Pokud struktura `B` obsahuje pole instance typu `A` a `A` je typ struktury, jedná se o chybu při kompilaci pro `A` pro záviset na `B` nebo na typu vytvořeném z `B`. Struktura `X` ***přímo závisí na*** struktuře `Y`, pokud `X` obsahuje pole instance typu `Y`. V této definici je kompletní sada struktur, na které struktura závisí, ***přímý uzávěr přímo závislá na*** vztahu.  Například
```csharp
struct Node
{
    int data;
    Node next; // error, Node directly depends on itself
}
```
je chyba, protože `Node` obsahuje pole instance vlastního typu.  Jiný příklad
```csharp
struct A { B b; }

struct B { C c; }

struct C { A a; }
```
je chyba, protože každý z typů `A`, `B` a `C` závisí na sobě navzájem.

U tříd je možné, aby dvě proměnné odkazovaly na stejný objekt a bylo tak možné, aby operace na jedné proměnné ovlivnily objekt, na který je odkazováno jinou proměnnou. U struktur mají proměnné, které mají svou vlastní kopii dat (s výjimkou proměnných parametrů `ref` a `out`) a nejsou možné operace na jednom z nich ovlivnit. Vzhledem k tomu, že struktury neodkazují na typy, není možné, aby hodnoty typu struktury byly `null`.

Daná deklarace
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
Vytvoří výstup hodnoty `10`. Přiřazení `a` pro `b` vytvoří kopii hodnoty a `b` tak nebude ovlivněno přiřazením na `a.x`. Byl `Point` místo toho deklarován jako třída, výstup by byl `100`, protože `a` a `b` by odkazovaly na stejný objekt.

### <a name="inheritance"></a>Dědičnost

Všechny typy struktury implicitně dědí z třídy `System.ValueType`, což zase dědí z třídy `object`. Deklarace struktury může určovat seznam implementovaných rozhraní, ale není možné, aby deklarace struktury určovala základní třídu.

Typy struktury nejsou nikdy abstraktní a jsou vždy implicitně zapečetěné. Modifikátory `abstract` a `sealed` nejsou proto v deklaraci struktury povoleny.

Vzhledem k tomu, že dědičnost není pro struktury podporovaná, deklarovaná přístupnost člena struktury nemůže být `protected` nebo `protected internal`.

Členy funkce ve struktuře nelze `abstract` nebo `virtual` a modifikátor `override` je povolen pouze pro přepsání metod zděděných od `System.ValueType`.

### <a name="assignment"></a>Přiřazení

Přiřazení k proměnné typu struktury vytvoří kopii hodnoty, která je přiřazena. To se liší od přiřazení k proměnné typu třídy, které kopírují odkaz, ale nikoli objekt identifikovaný odkazem.

Podobně jako u přiřazení, pokud je struktura předána jako parametr hodnoty nebo vrácena jako výsledek členu funkce, je vytvořena kopie struktury. Struktura může být předána odkazem na člena funkce pomocí parametru `ref` nebo `out`.

Pokud je cílem přiřazení vlastnost nebo indexer struktury, výraz instance přidružený k vlastnosti nebo přístupu indexeru musí být klasifikován jako proměnná. Pokud je výraz instance klasifikován jako hodnota, dojde k chybě při kompilaci. Tato informace je podrobněji popsána v tématu [jednoduché přiřazení](expressions.md#simple-assignment).

### <a name="default-values"></a>Výchozí hodnoty

Jak je popsáno ve [výchozích hodnotách](variables.md#default-values), několik druhů proměnných se automaticky inicializuje na výchozí hodnotu při jejich vytvoření. U proměnných typů tříd a jiných typů odkazů je tato výchozí hodnota `null`. Nicméně vzhledem k tomu, že struktury jsou typy hodnot, které nemohou být `null`, výchozí hodnota struktury je hodnota vytvořená nastavením všech polí Typ hodnoty na jejich výchozí hodnotu a všechna pole typu odkaz na `null`.

V příkladu se odkazuje na strukturu @no__t 0, která je uvedená výše, příklad
```csharp
Point[] a = new Point[100];
```
Inicializuje každé `Point` v poli na hodnotu vytvořenou nastavením polí `x` a `y` na hodnotu nula.

Výchozí hodnota struktury odpovídá hodnotě vrácené výchozím konstruktorem struktury ([výchozí konstruktory](types.md#default-constructors)). Na rozdíl od třídy struktura není povolena k deklaraci konstruktoru instance bez parametrů. Místo toho má každá struktura implicitně konstruktor instance bez parametrů, který vždycky vrací hodnotu, která je výsledkem nastavení všech polí typu hodnoty na výchozí hodnotu a všechna pole odkazového typu na `null`.

Struktury by měly být navržené tak, aby braly výchozí stav inicializace na platný stav. V příkladu
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
uživatelsky definovaný konstruktor instance chrání proti hodnotám null pouze tam, kde je explicitně volána. V případech, kdy proměnná `KeyValuePair` podléhá inicializaci výchozí hodnoty, budou pole `key` a `value` null a struktura musí být připravená na zpracování tohoto stavu.

### <a name="boxing-and-unboxing"></a>Zabalení a rozbalení

Hodnota typu třídy může být převedena na typ `object` nebo na typ rozhraní, který je implementován třídou jednoduše tím, že v době kompilace považuje odkaz za jiný typ. Stejně tak hodnota typu `object` nebo hodnota typu rozhraní lze převést zpět na typ třídy beze změny odkazu (ale v tomto případě je vyžadována kontrolní rutina typu runtime).

Vzhledem k tomu, že struktury nejsou odkazy na typy, jsou tyto operace pro typy struktury implementovány jinak. Je-li hodnota typu struktury převedena na typ `object` nebo na typ rozhraní, který je implementován strukturou, dojde k operaci zabalení. Podobně platí, že pokud je hodnota typu `object` nebo hodnota typu rozhraní převedena zpět na typ struktury, bude provedena operace rozbalení. Klíčovým rozdílem ze stejných operací na typech tříd je, že zabalení a rozbalení zkopíruje hodnotu struktury buď do, nebo z zabalené instance. Proto se po zabalení nebo rozbalení operace změny provedené v nezabalené struktuře neprojeví v zabalené struktuře.

Když typ struktury přepíše virtuální metodu děděnou z `System.Object` (například `Equals`, `GetHashCode` nebo `ToString`), volání virtuální metody prostřednictvím instance typu struktury nezpůsobí, že dojde k zabalení. To platí i v případě, že se jako parametr typu používá struktura a k vyvolání dojde prostřednictvím instance typu parametru typu. Příklad:
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
```console
1
2
3
```

I když je špatný styl `ToString` pro vedlejší účinky, příklad ukazuje, že žádné zabalení neproběhlo pro tři vyvolání `x.ToString()`.

Podobně zabalení nikdy neproběhne implicitně při přístupu ke členu v parametru omezeného typu. Předpokládejme například, že rozhraní `ICounter` obsahuje metodu `Increment`, kterou lze použít k úpravě hodnoty. Pokud je jako omezení použito `ICounter`, implementace metody `Increment` je volána s odkazem na proměnnou, která byla volána `Increment`, nikdy nezabalená kopie.

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

První volání `Increment` upraví hodnotu v proměnné `x`. To není ekvivalentní druhému volání `Increment`, což upraví hodnotu v zabalené kopii `x`. Proto výstup programu je:
```console
0
1
1
```

Další podrobnosti o zabalení a rozbalení naleznete v tématu [zabalení a rozbalení](types.md#boxing-and-unboxing).

### <a name="meaning-of-this"></a>Význam tohoto

V rámci konstruktoru instance nebo členu funkce instance třídy je `this` klasifikován jako hodnota. Proto, zatímco `this` lze použít k odkazování na instanci, pro kterou byl člen funkce vyvolán, není možné přiřadit k `this` v členu funkce třídy.

V rámci konstruktoru instance struktury, `this` odpovídá parametru `out` typu struktury a v rámci členu funkce instance struktury, `this` odpovídá parametru `ref` typu struktury. V obou případech je `this` klasifikován jako proměnná a je možné upravit celou strukturu, pro kterou byl člen funkce vyvolán přiřazením do `this` nebo předáním jako parametru `ref` nebo `out`.

### <a name="field-initializers"></a>Inicializátory polí

Jak je popsáno ve [výchozích hodnotách](structs.md#default-values), výchozí hodnota struktury sestává z hodnoty, která je výsledkem nastavení všech polí typu hodnoty na výchozí hodnotu a všech polí typu odkazu na `null`. Z tohoto důvodu struktura nepovoluje deklarace pole instance pro zahrnutí inicializátorů proměnných. Toto omezení platí pouze pro pole instance. Statická pole struktury mají povolený zahrnutí inicializátorů proměnných.

Příklad
```csharp
struct Point
{
    public int x = 1;  // Error, initializer not permitted
    public int y = 1;  // Error, initializer not permitted
}
```
došlo k chybě, protože deklarace pole instance obsahují proměnné inicializátorů.

### <a name="constructors"></a>Konstruktory

Na rozdíl od třídy struktura není povolena k deklaraci konstruktoru instance bez parametrů. Místo toho má každá struktura implicitně konstruktor instance bez parametrů, který vždycky vrací hodnotu, která je výsledkem nastavení všech polí typu hodnoty na výchozí hodnotu a všechna pole odkazového typu na hodnotu null ([výchozí konstruktory](types.md#default-constructors)). Struktura může deklarovat konstruktory instancí s parametry. Například
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

S ohledem na výše uvedenou deklaraci jsou příkazy
```csharp
Point p1 = new Point();
Point p2 = new Point(0, 0);
```
Vytvoří `Point` s `x` a `y` inicializována na nulu.

Konstruktor instance struktury není oprávněn zahrnovat inicializátor konstruktoru formuláře `base(...)`.

Pokud konstruktor instance struktury neurčuje inicializátor konstruktoru, proměnná `this` odpovídá parametru `out` typu struktury a podobně jako parametr `out`, `this` se musí jednoznačně přiřadit ([jednoznačné přiřazení ](variables.md#definite-assignment)) na každém místě, kde se konstruktor vrátí. Pokud konstruktor instance struktury určuje inicializátor konstruktoru, proměnná `this` odpovídá parametru `ref` typu struktury a podobá se parametru `ref`, `this` je považována za jednoznačně přiřazenou pro položku v těle konstruktoru. . Zvažte následující implementaci konstruktoru instance:
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

Není možné volat žádnou členskou funkci instance (včetně přístupových objektů set pro vlastnosti `X` a `Y`), dokud nebudou přiřazena všechna pole strukturované struktury. Jediná výjimka zahrnuje automaticky implementované vlastnosti ([automaticky implementované vlastnosti](classes.md#automatically-implemented-properties)). Neomezená pravidla přiřazení ([jednoduché výrazy přiřazení](variables.md#simple-assignment-expressions)) specificky vyloučí přiřazení k automatické vlastnosti typu struktury v rámci konstruktoru instance daného typu struktury. takové přiřazení je považováno za jednoznačné přiřazení skryté pole pro zálohování automatické vlastnosti. Proto jsou povoleny následující:

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

Struktura není oprávněná deklarovat destruktor.

### <a name="static-constructors"></a>Statické konstruktory

Statické konstruktory pro struktury se řídí většinou stejných pravidel jako u tříd. Provedení statického konstruktoru pro typ struktury je aktivováno prvním z následujících událostí, které se mají provést v rámci domény aplikace:

*  Odkazuje se na statický člen typu struktury.
*  Je volán explicitně deklarovaný konstruktor typu struktury.

Vytváření výchozích hodnot ([výchozích hodnot](structs.md#default-values)) typů struktury neaktivuje statický konstruktor. (Příkladem je počáteční hodnota prvků v poli.)

## <a name="struct-examples"></a>Příklady struktury

Následující příklad ukazuje dva významné příklady použití `struct` typů k vytváření typů, které lze použít podobně jako předdefinované typy jazyka, ale s upravenou sémantikou.

### <a name="database-integer-type"></a>Celočíselný typ databáze

Níže uvedená struktura `DBInt` implementuje celočíselný typ, který může představovat úplnou sadu hodnot typu `int` a další stav, který označuje neznámou hodnotu. Typ s těmito charakteristikami se běžně používá v databázích.

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

### <a name="database-boolean-type"></a>Typ databáze typu Boolean

Struktura `DBBool` níže implementuje logický typ se třemi hodnotami. Možné hodnoty tohoto typu jsou `DBBool.True`, `DBBool.False` a `DBBool.Null`, kde člen `Null` označuje neznámou hodnotu. Tyto tři logické typy jsou běžně používány v databázích.

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
