---
ms.openlocfilehash: 0a09585f4f885647230354c66a2449abb7ef1f44
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/25/2019
ms.locfileid: "64488775"
---
# <a name="interfaces"></a>Rozhraní

Rozhraní definuje kontrakt. Třída nebo struktura, která implementuje rozhraní musí dodržovat jeho kontrakt. Rozhraní může dědit z více základních rozhraní a třídy nebo struktury může implementovat více rozhraní.

Rozhraní může obsahovat metody, vlastnosti, události a indexery. Rozhraní samotné neposkytuje implementace jeho členů, které definuje. Rozhraní určuje pouze členy, které je třeba dodat ze třídy nebo struktury, které implementují rozhraní.

## <a name="interface-declarations"></a>Deklarace rozhraní.

*Interface_declaration* je *type_declaration* ([typ deklarace](namespaces.md#type-declarations)), který deklaruje nový typ rozhraní.

```antlr
interface_declaration
    : attributes? interface_modifier* 'partial'? 'interface'
      identifier variant_type_parameter_list? interface_base?
      type_parameter_constraints_clause* interface_body ';'?
    ;
```

*Interface_declaration* se skládá z volitelné sadu *atributy* ([atributy](attributes.md)) následovaný volitelná sada *interface_modifier*s ([rozhraní modifikátory](interfaces.md#interface-modifiers)), následovaným volitelnou `partial` modifikátor, za nímž následuje klíčové slovo `interface` a *identifikátor* , názvy rozhraní, za nímž následuje volitelným *variant_type_parameter_list* specifikace ([seznamy parametru typu Variant](interfaces.md#variant-type-parameter-lists)), následovaným volitelnou *interface_base* specifikace ([základní rozhraní](interfaces.md#base-interfaces)), následovaným volitelnou *type_parameter_constraints_clause*s specifikace ([omezení parametru typu](classes.md#type-parameter-constraints)) , za nímž následuje *interface_body* ([rozhraní text](interfaces.md#interface-body)), volitelně za nímž následuje středníkem.

### <a name="interface-modifiers"></a>Modifikátory rozhraní

*Interface_declaration* může volitelně zahrnovat posloupnost modifikátory rozhraní:

```antlr
interface_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    | interface_modifier_unsafe
    ;
```

Je chyba kompilace pro stejný modifikátor se zobrazí více než jednou v deklaraci rozhraní.

`new` Modifikátor je povolené jenom u rozhraní definované v rámci třídy. Určuje, že rozhraní skrývá zděděného člena se stejným názvem, jak je popsáno v [new – modifikátor](classes.md#the-new-modifier).

`public`, `protected`, `internal`, A `private` řídit modifikátory přístupnosti rozhraní. V závislosti na kontextu, ve kterém dochází k deklaraci rozhraní, může být povoleno pouze některé tyto modifikátory ([deklarovaná přístupnost](basic-concepts.md#declared-accessibility)).

### <a name="partial-modifier"></a>Částečný modifikátor

`partial` Modifikátor znamená, že to *interface_declaration* je částečný typ deklarace. Více deklarací částečné rozhraní se stejným názvem v rámci nadřazeného oboru názvů nebo typ deklarace se dá formuláře jedno rozhraní prohlášení, dle pravidel uvedených v [částečné typy](classes.md#partial-types).

### <a name="variant-type-parameter-lists"></a>Seznamy parametrů typ Variant

Seznamy parametrů variantního typu může dojít pouze na typy rozhraní a delegátů. Rozdíl od běžných *type_parameter_list*s je volitelný *variance_annotation* na každý z parametrů typu.

```antlr
variant_type_parameter_list
    : '<' variant_type_parameters '>'
    ;

variant_type_parameters
    : attributes? variance_annotation? type_parameter
    | variant_type_parameters ',' attributes? variance_annotation? type_parameter
    ;

variance_annotation
    : 'in'
    | 'out'
    ;
```

Pokud je anotaci variance `out`, typ parametru je označen jako ***kovariantní***. Pokud je anotaci variance `in`, typ parametru je označen jako ***kontravariantní***. Pokud neexistuje žádný anotaci variance, typ parametru je označen jako ***invariantní***.

V příkladu
```csharp
interface C<out X, in Y, Z> 
{
  X M(Y y);
  Z P { get; set; }
}
```
`X` je kovariantní `Y` je kontravariantní a `Z` je neutrální.

#### <a name="variance-safety"></a>Bezpečný přístup z více odchylek

Výskyt anotace variance v seznamu parametrů typu typu omezuje na místech, kde může dojít k typům v deklaraci typu.

Typ `T` je ***výstup unsafe*** pokud obsahuje jeden z následujících akcí:

*  `T` je parametr kontravariantního typu
*  `T` je typ pole s typem výstupu unsafe – element
*  `T` je typ rozhraní nebo delegát `S<A1,...,Ak>` vytvořený z obecného typu `S<X1,...,Xk>` where pro nejméně jednu `Ai` obsahuje jeden z následujících akcí:
   * `Xi` je kovariantní nebo neutrální a `Ai` není bezpečné pro výstup.
   * `Xi` je kontravariantní nebo invariantní a `Ai` je bezpečná pro vstup.
   
Typ `T` je ***vstup nebezpečné*** pokud obsahuje jeden z následujících akcí:

*  `T` je parametr kovariantního typu
*  `T` je typ pole s typem vstupu unsafe – element
*  `T` je typ rozhraní nebo delegát `S<A1,...,Ak>` vytvořený z obecného typu `S<X1,...,Xk>` where pro nejméně jednu `Ai` obsahuje jeden z následujících akcí:
   * `Xi` je kovariantní nebo neutrální a `Ai` není bezpečné pro vstup.
   * `Xi` je kontravariantní nebo invariantní a `Ai` není bezpečné pro výstup.

Intuitivně nebezpečné výstupní typ je zakázáno v poloze výstup a vstup nezabezpečený typ je zakázáno na vstupní pozici.

Typ je ***bezpečný výstup*** Pokud není výstupní nebezpečné, a ***vstup typově bezpečný*** nesplnění vstup unsafe.

#### <a name="variance-conversion"></a>Převod odchylka

Účelem anotace variance je zadání mírnější (ale pořád typově bezpečný) převody na typy rozhraní a delegátů. Tím účelem definice implicitní ([implicitních převodů](conversions.md#implicit-conversions)) a explicitní převody ([explicitních převodů](conversions.md#explicit-conversions)) ujistěte se, použijte pojem variance převoditelnosti, která je definovaná následujícím způsobem:

Typ `T<A1,...,An>` je odchylka převoditelné na typ `T<B1,...,Bn>` Pokud `T` rozhraní nebo typ delegáta deklarovat s parametry variantního typu `T<X1,...,Xn>`a pro každý parametr variantního typu `Xi` jednu z následujících obsahuje:

*  `Xi` je kovariantní a implicitní převod odkazu nebo identity `Ai` do `Bi`
*  `Xi` je kontravariantní a implicitní odkaz nebo převod identity `Bi` do `Ai`
*  `Xi` invariantní a identitu, která existuje převod z `Ai` do `Bi`

### <a name="base-interfaces"></a>Základní rozhraní

Rozhraní může dědit z nuly nebo více typů rozhraní, které se volají ***explicitní základních rozhraní*** rozhraní. Když rozhraní obsahuje jedno nebo více explicitní základní rozhraní, pak v deklaraci rozhraní, identifikátor rozhraní následuje dvojtečka a čárkou oddělený seznam typů základní rozhraní.

```antlr
interface_base
    : ':' interface_type_list
    ;
```

Pro typ vybudované rozhraní explicitní základních rozhraní jsou tvořeny tak explicitní základní rozhraní deklarací na deklarace obecného typu a nahrazování pro každou *type_parameter* v základní rozhraní deklarace, odpovídající *type_argument* sestaveného typu.

Explicitní základní rozhraní rozhraní musí být přinejmenším stejně dostupná jako rozhraní samotného ([usnadnění omezení](basic-concepts.md#accessibility-constraints)). Například je chyba kompilace k určení `private` nebo `internal` rozhraní v *interface_base* z `public` rozhraní.

Je chyba kompilace pro rozhraní přímo nebo nepřímo dědí sám od sebe.

***Základní rozhraní*** rozhraní jsou explicitní základní rozhraní a jejich základní rozhraní. Jinými slovy sadu základních rozhraní je kompletní přechodné uzavření explicitní základní rozhraní, jejich explicitní základní rozhraní a tak dále. Rozhraní zdědí všechny členy své základní rozhraní. V příkladu
```csharp
interface IControl
{
    void Paint();
}

interface ITextBox: IControl
{
    void SetText(string text);
}

interface IListBox: IControl
{
    void SetItems(string[] items);
}

interface IComboBox: ITextBox, IListBox {}
```
základní rozhraní `IComboBox` jsou `IControl`, `ITextBox`, a `IListBox`.

Jinými slovy `IComboBox` rozhraní výše dědí členy `SetText` a `SetItems` stejně jako `Paint`.

Každé základní rozhraní rozhraní musí být bezpečná pro výstup ([Variance bezpečnosti](interfaces.md#variance-safety)). Třída nebo struktura, která implementuje rozhraní také implicitně implementuje všechna rozhraní základní rozhraní.

### <a name="interface-body"></a>Rozhraní text

*Interface_body* rozhraní definuje členy rozhraní.

```antlr
interface_body
    : '{' interface_member_declaration* '}'
    ;
```

## <a name="interface-members"></a>Členy rozhraní

Členové rozhraní jsou členy zděděné ze základní rozhraní a členy deklarované pomocí samotným rozhraním.

```antlr
interface_member_declaration
    : interface_method_declaration
    | interface_property_declaration
    | interface_event_declaration
    | interface_indexer_declaration
    ;
```

Deklaraci rozhraní může deklarovat nula nebo více členů. Členy rozhraní musí být metody, vlastnosti, události nebo indexery. Rozhraní nemohou obsahovat konstanty, pole, operátory, konstruktory instancí, destruktory nebo typy a rozhraní může obsahovat statické členy libovolného typu.

Všechny členy rozhraní implicitně mít přístup public. Je chyba kompilace pro deklarace člena rozhraní zahrnout všechny modifikátory. Konkrétně se členy rozhraní nelze použít deklaraci modifikátory `abstract`, `public`, `protected`, `internal`, `private`, `virtual`, `override`, nebo `static`.

V příkladu
```csharp
public delegate void StringListEvent(IStringList sender);

public interface IStringList
{
    void Add(string s);
    int Count { get; }
    event StringListEvent Changed;
    string this[int index] { get; set; }
}
```
deklaruje rozhraní, které obsahuje jeden z možných druhy členů: Metody, vlastnosti, události a indexer.

*Interface_declaration* vytvoří nové prohlášení prostor ([deklarace](basic-concepts.md#declarations)) a *interface_member_declaration*s okamžitě obsažených *interface_declaration* zavést nové členy do tohoto prostoru deklarace. Následující pravidla platí pro *interface_member_declaration*s:

*  Název metody se musí lišit od názvů všech vlastnosti a události deklarované ve stejné rozhraní. Kromě toho, podpis ([podpisy a přetížení](basic-concepts.md#signatures-and-overloading)) z metody se musí lišit od podpisů všechny ostatní metody s deklarací ve stejné rozhraní, a dvě metody s deklarací ve stejné rozhraní nemusí máte podpisů, který se liší pouze `ref` a `out`.
*  Název vlastnosti nebo události se musí lišit od názvů ostatních členů deklarovaná ve stejné rozhraní.
*  Podpis indexeru se musí lišit od podpisů ze všech indexerů je deklarována v stejné rozhraní.

Zděděné členy rozhraní jsou speciálně není součástí místa deklarace rozhraní. Proto rozhraní může deklarovat člena se stejným názvem a signaturou jako zděděného člena. Pokud k tomu dojde, se říká, že člen odvozené rozhraní skrýt člen základního rozhraní. Skrytí zděděného člena není považováno za chybu, ale to způsobit, že kompilátor vydat upozornění. Potlačit upozornění, musí obsahovat deklarace člena odvozené rozhraní `new` modifikátor k označení, že odvozené člen má skrýt základního člena. V tomto tématu je popsán dále v [skrytí prostřednictvím dědičnosti](basic-concepts.md#hiding-through-inheritance).

Pokud `new` Modifikátor je součástí deklarace, která neskryje zděděného člena, objeví se upozornění o tom. Toto upozornění je potlačeno odebráním `new` modifikátor.

Všimněte si, že členy ve třídě `object` nejsou, přísně vzato členy libovolné rozhraní ([členy rozhraní](interfaces.md#interface-members)). Ale členy ve třídě `object` jsou k dispozici prostřednictvím člen vyhledávání v libovolný typ rozhraní ([člen vyhledávání](expressions.md#member-lookup)).

### <a name="interface-methods"></a>Metody rozhraní

Metody rozhraní jsou deklarovány pomocí *interface_method_declaration*s:

```antlr
interface_method_declaration
    : attributes? 'new'? return_type identifier type_parameter_list
      '(' formal_parameter_list? ')' type_parameter_constraints_clause* ';'
    ;
```

*Atributy*, *typ*, *identifikátor*, a *formal_parameter_list* deklarace metody rozhraní mají stejné To znamená jako deklarace metody ve třídě ([metody](classes.md#methods)). Deklaraci metody rozhraní není povolené zadat těla metody a deklarace proto vždy končí středníkem.

Každý formální parametr typu metody rozhraní musí být bezpečná pro vstup ([Variance bezpečnosti](interfaces.md#variance-safety)), a návratový typ musí být buď `void` nebo výstup typově bezpečné. Kromě toho každé omezení typu třídy, rozhraní typu omezení a omezení parametru typu na libovolný typ parametru metody musí být bezpečná pro vstup.

Tato pravidla ujistěte se, že všechny kovariantní nebo kontravariantní využití rozhraní zůstává bezpečnost typů. Například
```csharp
interface I<out T> { void M<U>() where U : T; }
```
je neplatné. protože použití `T` jako omezení parametru typu na `U` není bezpečná pro vstup.

Bylo toto omezení není použito by bylo možné porušení bezpečnosti typů následujícím způsobem:
```csharp
class B {}
class D : B{}
class E : B {}
class C : I<D> { public void M<U>() {...} }
...
I<B> b = new C();
b.M<E>();
```
Toto je ve skutečnosti volání `C.M<E>`. Ale toto volání vyžaduje, aby `E` odvozovat `D`, takže bezpečnost typů by se tady došlo k porušení.

### <a name="interface-properties"></a>Vlastnosti rozhraní

Vlastnosti rozhraní jsou deklarovány pomocí *interface_property_declaration*s:

```antlr
interface_property_declaration
    : attributes? 'new'? type identifier '{' interface_accessors '}'
    ;

interface_accessors
    : attributes? 'get' ';'
    | attributes? 'set' ';'
    | attributes? 'get' ';' attributes? 'set' ';'
    | attributes? 'set' ';' attributes? 'get' ';'
    ;
```

*Atributy*, *typ*, a *identifikátor* deklaraci vlastnosti rozhraní mají stejný význam jako deklarace vlastnosti ve třídě ([ Vlastnosti](classes.md#properties)).

Přistupující objekty deklaraci vlastnosti rozhraní odpovídají přistupující objekty vlastnosti deklaraci třídy ([přistupující objekty](classes.md#accessors)), s tím rozdílem, že tělo přístupový objekt musí být vždy středník. Proto přístupové objekty jednoduše označí, zda je vlastnost pro čtení i zápis, jen pro čtení nebo jen pro zápis.

Výstup typově bezpečný, pokud existuje přístupový objekt get musí být typu vlastnost rozhraní a musí být vstup typově bezpečný, pokud existuje přístupový objekt set.

### <a name="interface-events"></a>Události rozhraní

Události rozhraní jsou deklarovány pomocí *interface_event_declaration*s:

```antlr
interface_event_declaration
    : attributes? 'new'? 'event' type identifier ';'
    ;
```

*Atributy*, *typ*, a *identifikátor* deklaraci události rozhraní mají stejný význam jako deklaraci události ve třídě ([události ](classes.md#events)).

Bezpečná pro vstup musí být typu události rozhraní.

### <a name="interface-indexers"></a>Indexery v rozhraní

Indexery v rozhraní jsou deklarovány pomocí *interface_indexer_declaration*s:

```antlr
interface_indexer_declaration
    : attributes? 'new'? type 'this' '[' formal_parameter_list ']' '{' interface_accessors '}'
    ;
```

*Atributy*, *typ*, a *formal_parameter_list* indexer deklarace rozhraní mají stejný význam jako deklarace indexeru ve třídě ([ Indexery](classes.md#indexers)).

Přistupující objekty deklaraci rozhraní indexeru odpovídají přistupující objekty indexer deklaraci třídy ([indexery](classes.md#indexers)), s tím rozdílem, že tělo přístupový objekt musí být vždy středník. Proto přístupové objekty jednoduše označí, zda je indexeru pro čtení i zápis, jen pro čtení nebo jen pro zápis.

Všechny typy formálních parametrů indexer rozhraní musí být bezpečná pro vstup. Kromě toho všechny `out` nebo `ref` typy formálních parametrů musí být také výstup typově bezpečné. Poznámka: to dokonce i `out` parametry musejí být vstup typově bezpečný, kvůli omezením platformy pro základní spuštění.

Výstup typově bezpečný, pokud existuje přístupový objekt get musí být typu rozhraní indexeru a musí být vstup typově bezpečný, pokud existuje přístupový objekt set.

### <a name="interface-member-access"></a>Přístup ke členu rozhraní

Rozhraní členové budou přístupní prostřednictvím přístupu ke členu ([přístup ke členu](expressions.md#member-access)) a přístup indexeru ([přístup indexeru](expressions.md#indexer-access)) výrazů formuláře `I.M` a `I[A]`, kde `I` je typu rozhraní, `M` je metoda, vlastnost nebo událost typu rozhraní, a `A` je seznam argumentů indexeru.

Pro rozhraní, které se týkají výhradně jednoduchou dědičností (přesně žádnou nebo jednu přímou základní rozhraní každého rozhraní v řetězu dědičnosti má), důsledky člena vyhledávání ([člen vyhledávání](expressions.md#member-lookup)), volání metody ([ Volání metod](expressions.md#method-invocations)) a přístup indexeru ([přístup indexeru](expressions.md#indexer-access)) pravidla jsou stejné jako u třídy a struktury: Skrýt členy více odvozený méně odvozené členy se stejným názvem a signaturou. Pro rozhraní vícenásobné dědičnosti nejednoznačnosti může dojít, když dva nebo víc nesouvisejících základních rozhraní deklarovat členy s totožným názvem a signaturou. Tato část ukazuje několik příkladů takových situacích. Ve všech případech je možné přeložit nejednoznačnosti explicitní přetypování.

V příkladu
```csharp
interface IList
{
    int Count { get; set; }
}

interface ICounter
{
    void Count(int i);
}

interface IListCounter: IList, ICounter {}

class C
{
    void Test(IListCounter x) {
        x.Count(1);                  // Error
        x.Count = 1;                 // Error
        ((IList)x).Count = 1;        // Ok, invokes IList.Count.set
        ((ICounter)x).Count(1);      // Ok, invokes ICounter.Count
    }
}
```
první dva příkazy způsobit chyby při kompilaci, protože člen vyhledávání ([člen vyhledávání](expressions.md#member-lookup)) z `Count` v `IListCounter` je nejednoznačný. Jak je znázorněno v příkladu, přetypování se vyřešit nejednoznačnost `x` na typ odpovídající základní rozhraní. Tyto položky CAST mít žádné náklady za běhu – pouze sestávají z následujících zobrazení instance jako méně odvozený typ v době kompilace.

V příkladu
```csharp
interface IInteger
{
    void Add(int i);
}

interface IDouble
{
    void Add(double d);
}

interface INumber: IInteger, IDouble {}

class C
{
    void Test(INumber n) {
        n.Add(1);                // Invokes IInteger.Add
        n.Add(1.0);              // Only IDouble.Add is applicable
        ((IInteger)n).Add(1);    // Only IInteger.Add is a candidate
        ((IDouble)n).Add(1);     // Only IDouble.Add is a candidate
    }
}
```
vyvolání `n.Add(1)` vybere `IInteger.Add` použitím pravidla rozlišení přetížení [rozlišení přetěžování](expressions.md#overload-resolution). Podobně při vyvolání `n.Add(1.0)` vybere `IDouble.Add`. Po vložení explicitní přetypování, je pouze jeden Release candidate metoda a proto nejednoznačnosti.

V příkladu
```csharp
interface IBase
{
    void F(int i);
}

interface ILeft: IBase
{
    new void F(int i);
}

interface IRight: IBase
{
    void G();
}

interface IDerived: ILeft, IRight {}

class A
{
    void Test(IDerived d) {
        d.F(1);                 // Invokes ILeft.F
        ((IBase)d).F(1);        // Invokes IBase.F
        ((ILeft)d).F(1);        // Invokes ILeft.F
        ((IRight)d).F(1);       // Invokes IBase.F
    }
}
```
`IBase.F` skryl člen `ILeft.F` člena. Vyvolání `d.F(1)` takto vybere `ILeft.F`, i když `IBase.F` se nebudou skryté v přístupové cestě, která provede `IRight`.

Intuitivní pravidlo pro skrytí v rozhraních vícenásobné dědičnosti je jednoduše toto: Pokud je člen skrytý v jakékoli cestě přístup, je skrytý ve všech cestách přístup. Protože cesta pro přístup z `IDerived` k `ILeft` k `IBase` skryje `IBase.F`, člen také skrytý v přístupové cestě z `IDerived` k `IRight` k `IBase`.

## <a name="fully-qualified-interface-member-names"></a>Rozhraní plně kvalifikované názvy členů

Člen rozhraní se někdy označuje podle jeho ***plně kvalifikovaný název***. Plně kvalifikovaný název člena rozhraní se skládá z názvu rozhraní, ve kterém je deklarována, následované tečkou, za nímž následuje název člena člena. Plně kvalifikovaný název členu odkazuje na rozhraní, ve kterém člen je deklarovaný. Mějme například deklarace
```csharp
interface IControl
{
    void Paint();
}

interface ITextBox: IControl
{
    void SetText(string text);
}
```
plně kvalifikovaný název `Paint` je `IControl.Paint` a plně kvalifikovaný název `SetText` je `ITextBox.SetText`.

V příkladu výše, není možné odkazovat na `Paint` jako `ITextBox.Paint`.

Pokud rozhraní je součástí oboru názvů, plně kvalifikovaný název člena rozhraní obsahuje název oboru názvů. Příklad
```csharp
namespace System
{
    public interface ICloneable
    {
        object Clone();
    }
}
```

Tady, plně kvalifikovaný název `Clone` je metoda `System.ICloneable.Clone`.

## <a name="interface-implementations"></a>Implementace rozhraní

Pomocí třídy a struktury mohou implementovat rozhraní. K označení, že třídy nebo struktury přímo implementuje rozhraní, je identifikátor rozhraní součástí seznamu třídy základní třídy nebo struktury. Příklad:
```csharp
interface ICloneable
{
    object Clone();
}

interface IComparable
{
    int CompareTo(object other);
}

class ListEntry: ICloneable, IComparable
{
    public object Clone() {...}
    public int CompareTo(object other) {...}
}
```

Třída nebo struktura, která implementuje rozhraní přímo také přímo implementuje všechna rozhraní základní rozhraní implicitně. To platí i v případě, že dané třídy nebo struktury neobsahuje explicitně všech základních rozhraní v seznamu základních tříd. Příklad:
```csharp
interface IControl
{
    void Paint();
}

interface ITextBox: IControl
{
    void SetText(string text);
}

class TextBox: ITextBox
{
    public void Paint() {...}
    public void SetText(string text) {...}
}
```

Tady třídy `TextBox` implementuje oba `IControl` a `ITextBox`.

Pokud třída `C` přímo implementuje rozhraní, všechny třídy odvozené z jazyka C implicitně také implementovat rozhraní. Základní rozhraní zadané v deklaraci třídy mohou být typy vybudované rozhraní ([vytvořený typy](types.md#constructed-types)). Základní rozhraní nemůže být parametr typu sama o sobě, i když to může zahrnovat parametry typu, které jsou v oboru. Následující kód ukazuje, jak implementovat a rozšířit sestavené typy třídy:
```csharp
class C<U,V> {}

interface I1<V> {}

class D: C<string,int>, I1<string> {}

class E<T>: C<int,T>, I1<T> {}
```

Základní rozhraní deklaraci obecné třídy musí splňovat pravidla jedinečnost je popsáno v [jedinečnost implementovaná rozhraní](interfaces.md#uniqueness-of-implemented-interfaces).

### <a name="explicit-interface-member-implementations"></a>Člen implementace explicitního rozhraní

Pro účely provádění rozhraní, třídy nebo struktury může deklarovat ***implementace explicitního rozhraní členských***. Implementace explicitního rozhraní člen je deklarace metody, vlastnosti, události nebo indexeru, který odkazuje na rozhraní plně kvalifikovaný název členu. Příklad
```csharp
interface IList<T>
{
    T[] GetElements();
}

interface IDictionary<K,V>
{
    V this[K key];
    void Add(K key, V value);
}

class List<T>: IList<T>, IDictionary<int,T>
{
    T[] IList<T>.GetElements() {...}
    T IDictionary<int,T>.this[int index] {...}
    void IDictionary<int,T>.Add(int index, T value) {...}
}
```

Tady `IDictionary<int,T>.this` a `IDictionary<int,T>.Add` jsou člen implementace explicitního rozhraní.

V některých případech nemusí být vhodný pro implementující třídu, ve kterém může být případ tohoto člena rozhraní implementované pomocí explicitní implementace členu rozhraní název člena rozhraní. Třída implementace souborovou abstrakci, například by pravděpodobně implementovat `Close` členskou funkci, která má za následek uvolnění souboru prostředků a implementovat `Dispose` metodu `IDisposable` rozhraní pomocí explicitního rozhraní implementace člena:
```csharp
interface IDisposable
{
    void Dispose();
}

class MyFile: IDisposable
{
    void IDisposable.Dispose() {
        Close();
    }

    public void Close() {
        // Do what's necessary to close the file
        System.GC.SuppressFinalize(this);
    }
}
```

Není možné přistupovat explicitní implementace členu rozhraní prostřednictvím jeho plně kvalifikovaný název volání metody, přístup k vlastnosti nebo indexeru přístup. Implementace explicitního rozhraní člen je přístupný pouze prostřednictvím instance rozhraní a se odkazuje v tomto případě jednoduše tak, že její název člena.

Je chyba kompilace pro člen implementace explicitního rozhraní zahrnout modifikátory přístupu a je chyba kompilace zahrnout modifikátory `abstract`, `virtual`, `override`, nebo `static`.

Implementace explicitního rozhraní členských mají různou přístupností. vlastnosti než ostatní členy. Protože jsou implementace explicitního rozhraní členských nikdy přístupné prostřednictvím jejich plně kvalifikovaný název metody vyvolání nebo přístup k vlastnosti, jsou ve smyslu privátní. Nicméně protože jsou dostupné prostřednictvím instance rozhraní, jsou ve smyslu také veřejné.

Implementace explicitního rozhraní členských mají dva primární účely:

*  Protože implementace explicitního rozhraní člen není přístupný prostřednictvím instance třídy nebo struktury, umožňují implementací rozhraní, které se mají vyloučit z veřejného rozhraní, třídy nebo struktury. To je zvlášť užitečné, když třída nebo struktura implementuje interní rozhraní, které nejsou důležité pro příjemce této třídy nebo struktury.
*  Implementace explicitního rozhraní členských Povolit odstraňování mnohoznačnosti rozhraní členů se stejným podpisem. Bez implementace explicitního rozhraní členských to by jinak nebylo možné pro třídu nebo strukturu mít různé implementace rozhraní členy se stejným podpisem a návratový typ, jako by ji nebylo možné pro třídu nebo strukturu mít žádnou implementaci všechny členy rozhraní se stejným podpisem ale s různými návratové typy.

Implementace explicitního rozhraní člen platný třídy nebo struktury název rozhraní v její základní třídy seznamu, který obsahuje člena, jehož plně kvalifikovaný název, typ a typy parametrů přesně odpovídat názvům členů explicitního rozhraní implementace. Díky tomu se v následující třídy
```csharp
class Shape: ICloneable
{
    object ICloneable.Clone() {...}
    int IComparable.CompareTo(object other) {...}    // invalid
}
```
deklarace `IComparable.CompareTo` výsledkem chyba kompilace, protože `IComparable` není uvedena v seznamu základních tříd z `Shape` a není základní rozhraní `ICloneable`. Stejně tak v deklaracích
```csharp
class Shape: ICloneable
{
    object ICloneable.Clone() {...}
}

class Ellipse: Shape
{
    object ICloneable.Clone() {...}    // invalid
}
```
deklarace `ICloneable.Clone` v `Ellipse` výsledkem chyba kompilace, protože `ICloneable` není výslovně uvedené v seznamu základních tříd z `Ellipse`.

Plně kvalifikovaný název člena rozhraní musí odkazovat na rozhraní, ve kterém byl deklarován člen. Proto v deklaracích
```csharp
interface IControl
{
    void Paint();
}

interface ITextBox: IControl
{
    void SetText(string text);
}

class TextBox: ITextBox
{
    void IControl.Paint() {...}
    void ITextBox.SetText(string text) {...}
}
```
implementace explicitního rozhraní člen `Paint` musí být napsaná jako `IControl.Paint`.

### <a name="uniqueness-of-implemented-interfaces"></a>Jedinečnost implementovaného rozhraní

Rozhraní implementovaná deklarace obecného typu musí zůstat jedinečné pro všechny možné sestavené typy. Bez tohoto pravidla by bylo možné určit správnou metodu pro volání pro určité sestavené typy. Předpokládejme například, deklaraci obecné třídy byly převody povoleny k zapsání následujícím způsobem:
```csharp
interface I<T>
{
    void F();
}

class X<U,V>: I<U>, I<V>                    // Error: I<U> and I<V> conflict
{
    void I<U>.F() {...}
    void I<V>.F() {...}
}
```

Byla to povolené, by bylo možné určit, jaký kód ke spuštění v následujících případech:
```csharp
I<int> x = new X<int,int>();
x.F();
```

Pokud chcete zjistit, pokud rozhraní seznamu deklarace obecného typu je platný, jsou prováděny následovně:

*  Umožní `L` být seznam rozhraní přímo zadaný v deklaraci rozhraní, struktury nebo obecná třída `C`.
*  Přidat do `L` některé základní rozhraní již v rozhraní `L`.
*  Odebrat duplicity z `L`.
*  Pokud všechny možné konstruovaný typ vytvořený z `C` by po argumenty typu jsou nahrazeny do `L`, způsobit, že dvě rozhraní v `L` shodovat, pak deklarace `C` je neplatný. Deklarace omezení nejsou považovány za při určování všechny možné sestavené typy.

V deklaraci třídy `X` nad seznamem rozhraní `L` se skládá z `I<U>` a `I<V>`. Deklarace není platná, protože některé konstruovaný typ s `U` a `V` stejným typem by způsobila tato dvě rozhraní být identické typy.

Je možné, určených na úrovních různé dědičnosti sjednotit rozhraní:
```csharp
interface I<T>
{
    void F();
}

class Base<U>: I<U>
{
    void I<U>.F() {...}
}

class Derived<U,V>: Base<U>, I<V>    // Ok
{
    void I<V>.F() {...}
}
```

Tento kód je platný i v případě `Derived<U,V>` implementuje oba `I<U>` a `I<V>`. Kód
```csharp
I<int> x = new Derived<int,int>();
x.F();
```
vyvolá metodu v `Derived`, protože `Derived<int,int>` efektivně implementuje znovu `I<int>` ([opětovná implementace rozhraní](interfaces.md#interface-re-implementation)).

### <a name="implementation-of-generic-methods"></a>Provádění obecné metody

Při obecné metody implicitně implementuje metodu rozhraní omezení uvedená pro každý parametr typu metody musí být v obou deklarace ekvivalentní (po libovolný typ rozhraní parametry nahrazeny příslušnými argumenty typu), kde – Metoda parametry typu jsou označeny ordinální Pozice zleva doprava.

Při obecné metody explicitně implementuje metodu rozhraní, ale bez omezení jsou povoleny v implementaci metody. Místo toho se omezení dědí z rozhraní – metoda

```csharp
interface I<A,B,C>
{
    void F<T>(T t) where T: A;
    void G<T>(T t) where T: B;
    void H<T>(T t) where T: C;
}

class C: I<object,C,string>
{
    public void F<T>(T t) {...}                    // Ok
    public void G<T>(T t) where T: C {...}         // Ok
    public void H<T>(T t) where T: string {...}    // Error
}
```

Metoda `C.F<T>` implicitně implementuje `I<object,C,string>.F<T>`. V takovém případě `C.F<T>` není požadováno (ani povolené) k určení omezení `T:object` od `object` je implicitní omezení pro všechny parametry typu. Metoda `C.G<T>` implicitně implementuje `I<object,C,string>.G<T>` vzhledem k tomu, že omezení shodovat s hodnotami v rozhraní, po nahrazení parametrů typu rozhraní s odpovídající argumenty typu. Omezení pro metodu `C.H<T>` se o chybu, protože zapečetěné typy (`string` v tomto případě) nejde použít jako omezení. Vynechání omezení by také být chybu, protože omezení implementace metody implicitní rozhraní jsou vyžadovaný pro shodu. Proto není možné implicitně implementovat `I<object,C,string>.H<T>`. Tato metoda rozhraní je možné implementovat pouze pomocí implementace explicitního rozhraní člena:
```csharp
class C: I<object,C,string>
{
    ...

    public void H<U>(U u) where U: class {...}

    void I<object,C,string>.H<T>(T t) {
        string s = t;    // Ok
        H<T>(t);
    }
}
```

V tomto příkladu volá člen implementace explicitního rozhraní veřejnou metodu s výhradně slabšími omezeními. Všimněte si, že přiřazení z `t` k `s` je platný od `T` dědí omezení `T:string`, i když toto omezení není anyAttribute ve zdrojovém kódu.

### <a name="interface-mapping"></a>Mapování rozhraní

Třídy nebo struktury, musíte zadat implementace členů rozhraní, které jsou uvedeny v seznamu základních tříd z dané třídy nebo struktury. Proces vyhledávání v implementující třída nebo struktura implementuje členy daného rozhraní se označuje jako ***mapování rozhraní***.

Mapování rozhraní třídy nebo struktury `C` vyhledá implementace pro každého člena každé rozhraní, udávaná v seznamu základních tříd z `C`. Implementace členu rozhraní konkrétní `I.M`, kde `I` rozhraní, ve kterém je člen `M` je deklarována, je určen tím, že kontroluje každá třída nebo struktura `S`počínaje `C` a opakující se pro každou po sobě jdoucích základní třídu `C`, dokud nebude nalezen odpovídající:

*  Pokud `S` obsahuje prohlášení o implementace explicitního rozhraní člena, která odpovídá `I` a `M`, pak tento člen je provádění `I.M`.
*  Jinak, pokud `S` obsahuje prohlášení o nestatická veřejný člen, který odpovídá `M`, pak tento člen je provádění `I.M`. Pokud je více než jeden člen shody neurčené který člen je provádění `I.M`. Tato situace může nastat, pouze pokud `S` je konstruovaný typ. Pokud dva členy, jak je deklarován v obecném typu mít různé podpisy, ale argumenty typu provádět jejich podpisy identické.

Chyba kompilace v případě implementace nejde najít pro všechny členy všechna rozhraní zadané v seznamu základních tříd z `C`. Mějte na paměti, že členy rozhraní zahrnout tyto členy, které jsou zděděny ze základních rozhraní.

Pro účely mapování rozhraní, člen třídy `A` odpovídá člena rozhraní `B` při:

*  `A` a `B` jsou metody a název, typ, a seznam formálních parametrů `A` a `B` jsou identické.
*  `A` a `B` jsou vlastnosti, název a typ `A` a `B` jsou identické, a `A` má stejné přístupové objekty jako `B` (`A` smí mít přistupující objekty další, pokud se nejedná o explicitní rozhraní implementace člena).
*  `A` a `B` jsou události a název a typ `A` a `B` jsou identické.
*  `A` a `B` indexery, je typ a formální parametr seznam `A` a `B` jsou identické, a `A` má stejné přístupové objekty jako `B` (`A` smí mít další přístupových objektů, pokud se nejedná explicitní implementace členu rozhraní).

Významné důsledky algoritmu mapování rozhraní jsou následující:

*  Implementace explicitního rozhraní členských modulů má přednost před ostatní členové ve stejné třídě nebo struktuře při určování člen třídy nebo struktury, který implementuje člena rozhraní.
*  Neveřejné ani statické členy účastnit mapování rozhraní.

V příkladu
```csharp
interface ICloneable
{
    object Clone();
}

class C: ICloneable
{
    object ICloneable.Clone() {...}
    public object Clone() {...}
}
```
`ICloneable.Clone` členem `C` stane provádění `Clone` v `ICloneable` vzhledem k tomu, že člen implementace explicitního rozhraní přednost před ostatními členy.

Pokud třída nebo struktura implementuje dvě nebo více rozhraní, který obsahuje člena se stejným názvem, typem a typy parametrů, je možné mapovat každou z těchto členů rozhraní do jednoho člena třídy nebo struktury. Příklad
```csharp
interface IControl
{
    void Paint();
}

interface IForm
{
    void Paint();
}

class Page: IControl, IForm
{
    public void Paint() {...}
}
```

Tady `Paint` obě metody `IControl` a `IForm` jsou mapovány na `Paint` metoda ve `Page`. To je samozřejmě také možné mít samostatné explicitní člen implementace rozhraní pro tyto dvě metody.

Pokud třída nebo struktura implementuje rozhraní, které obsahuje skryté členy, musí být některé členy nutně implementované prostřednictvím implementace explicitního rozhraní člena. Příklad
```csharp
interface IBase
{
    int P { get; }
}

interface IDerived: IBase
{
    new int P();
}
```

Implementace tohoto rozhraní by vyžadovaly alespoň jeden explicitní implementace členu rozhraní a by proveďte jednu z následujících forem
```csharp
class C: IDerived
{
    int IBase.P { get {...} }
    int IDerived.P() {...}
}

class C: IDerived
{
    public int P { get {...} }
    int IDerived.P() {...}
}

class C: IDerived
{
    int IBase.P { get {...} }
    public int P() {...}
}
```

Pokud třída implementuje více rozhraní, které mají stejný základní rozhraní, může být pouze jedna implementace základní rozhraní. V příkladu
```csharp
interface IControl
{
    void Paint();
}

interface ITextBox: IControl
{
    void SetText(string text);
}

interface IListBox: IControl
{
    void SetItems(string[] items);
}

class ComboBox: IControl, ITextBox, IListBox
{
    void IControl.Paint() {...}
    void ITextBox.SetText(string text) {...}
    void IListBox.SetItems(string[] items) {...}
}
```
není možné mít odlišnou implementaci pro `IControl` s názvem v seznamu základních tříd, `IControl` děděné `ITextBox`a `IControl` děděné `IListBox`. Ve skutečnosti není potuchy samostatné identity pro tato rozhraní. Místo toho implementace `ITextBox` a `IListBox` sdílet stejné provádění `IControl`, a `ComboBox` považován za jednoduše implementovat tři rozhraní `IControl`, `ITextBox`, a `IListBox`.

Členy základní třídy součástí mapování rozhraní. V příkladu
```csharp
interface Interface1
{
    void F();
}

class Class1
{
    public void F() {}
    public void G() {}
}

class Class2: Class1, Interface1
{
    new public void G() {}
}
```
Metoda `F` v `Class1` se používá v `Class2`vaší implementace `Interface1`.

### <a name="interface-implementation-inheritance"></a>Dědičnost implementace rozhraní

Třída dědí všechny implementace rozhraní poskytovaných jejích základních tříd.

Bez explicitně ***pokaždé znova implementovány*** rozhraní, odvozené třídy nelze žádným způsobem změnit mapování rozhraní dědí z jejích základních tříd. Například v deklaracích
```csharp
interface IControl
{
    void Paint();
}

class Control: IControl
{
    public void Paint() {...}
}

class TextBox: Control
{
    new public void Paint() {...}
}
```
`Paint` metoda ve `TextBox` skryje `Paint` metoda ve `Control`, ale nezmění mapování `Control.Paint` do `IControl.Paint`a volání `Paint` prostřednictvím třídy instance a interface instance bude mít následujících efektů
```csharp
Control c = new Control();
TextBox t = new TextBox();
IControl ic = c;
IControl it = t;
c.Paint();            // invokes Control.Paint();
t.Paint();            // invokes TextBox.Paint();
ic.Paint();           // invokes Control.Paint();
it.Paint();           // invokes Control.Paint();
```

Ale když metody rozhraní je mapována na virtuální metodu ve třídě, je možné pro odvozeným třídám přepsat virtuální metodu a změňte implementaci rozhraní. Například přepsání deklarace výše na
```csharp
interface IControl
{
    void Paint();
}

class Control: IControl
{
    public virtual void Paint() {...}
}

class TextBox: Control
{
    public override void Paint() {...}
}
```
se teď mělo být dodržen následujících efektů
```csharp
Control c = new Control();
TextBox t = new TextBox();
IControl ic = c;
IControl it = t;
c.Paint();            // invokes Control.Paint();
t.Paint();            // invokes TextBox.Paint();
ic.Paint();           // invokes Control.Paint();
it.Paint();           // invokes TextBox.Paint();
```

Vzhledem k tomu, že člen implementace explicitního rozhraní nejde použít deklaraci virtuální, není možné přepsat explicitní implementace členu rozhraní. Ale je zcela platná pro člena implementace explicitního rozhraní volat jinou metodu, a, jiné metody mohou být deklarovány virtuální umožňující odvozených třídách přepsat. Příklad
```csharp
interface IControl
{
    void Paint();
}

class Control: IControl
{
    void IControl.Paint() { PaintControl(); }
    protected virtual void PaintControl() {...}
}

class TextBox: Control
{
    protected override void PaintControl() {...}
}
```

Tady třídy odvozené z `Control` můžete specialize provádění `IControl.Paint` tak, že přepíšete `PaintControl` metody.

### <a name="interface-re-implementation"></a>Opětovná implementace rozhraní

Třída, která dědí implementaci rozhraní povoleno ***znovu implementovat*** rozhraní zahrnutím v seznamu základních tříd.

Opětovná implementace rozhraní se řídí přesně stejná rozhraní mapování pravidla, počáteční implementace rozhraní. Zděděné rozhraní mapování tedy nemá žádný vliv na mapování rozhraní stanovit opětovná implementace rozhraní. Například v deklaracích
```csharp
interface IControl
{
    void Paint();
}

class Control: IControl
{
    void IControl.Paint() {...}
}

class MyControl: Control, IControl
{
    public void Paint() {}
}
```
fakt, který `Control` mapuje `IControl.Paint` do `Control.IControl.Paint` nemá vliv na opětovná implementace v `MyControl`, namapovaná `IControl.Paint` na `MyControl.Paint`.

Zděděná deklarace veřejného člena a člen zděděný explicitní rozhraní, které deklarace účastnit procesu rozhraní mapování pro znovu implementovaných rozhraní. Příklad
```csharp
interface IMethods
{
    void F();
    void G();
    void H();
    void I();
}

class Base: IMethods
{
    void IMethods.F() {}
    void IMethods.G() {}
    public void H() {}
    public void I() {}
}

class Derived: Base, IMethods
{
    public void F() {}
    void IMethods.H() {}
}
```

Tady, provádění `IMethods` v `Derived` mapuje metody rozhraní do `Derived.F`, `Base.IMethods.G`, `Derived.IMethods.H`, a `Base.I`.

Pokud třída implementuje rozhraní, je implicitně také implementuje všechna rozhraní základní rozhraní. Obdobně opětovná implementace rozhraní je také implicitně opětovná implementace všech základních rozhraní rozhraní. Příklad
```csharp
interface IBase
{
    void F();
}

interface IDerived: IBase
{
    void G();
}

class C: IDerived
{
    void IBase.F() {...}
    void IDerived.G() {...}
}

class D: C, IDerived
{
    public void F() {...}
    public void G() {...}
}
```

Tady, opětovná implementace `IDerived` také znovu implementuje `IBase`, mapování `IBase.F` na `D.F`.

### <a name="abstract-classes-and-interfaces"></a>Abstraktní třídy a rozhraní

Stejně jako neabstraktní třídy musíte zadat abstraktní třída implementace všech členů, které jsou uvedeny v seznamu základních tříd třídy rozhraní. Abstraktní třídy je však povoleno mapování rozhraní metod na abstraktní metody. Příklad
```csharp
interface IMethods
{
    void F();
    void G();
}

abstract class C: IMethods
{
    public abstract void F();
    public abstract void G();
}
```

Tady, provádění `IMethods` mapuje `F` a `G` na abstraktní metody, které musí být přepsána nastaveními v neabstraktní třídy, které jsou odvozeny z `C`.

Všimněte si, že člen implementace explicitního rozhraní nemohou být abstraktní, ale implementace explicitního rozhraní členských samozřejmě povoleno volat abstraktní metody. Příklad
```csharp
interface IMethods
{
    void F();
    void G();
}

abstract class C: IMethods
{
    void IMethods.F() { FF(); }
    void IMethods.G() { GG(); }
    protected abstract void FF();
    protected abstract void GG();
}
```

Tady, neabstraktní třídy, které jsou odvozeny z `C` by bylo zapotřebí k přepsání `FF` a `GG`, díky tomu Skutečná implementace `IMethods`.
