---
ms.openlocfilehash: af7af574814dc04ee3ece0396b7ae5f86b3ec8eb
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/25/2019
ms.locfileid: "64488905"
---
# <a name="classes"></a>Třídy

Třída je datová struktura, která mohou obsahovat datové členy (konstant a polí), funkce členy (metody, vlastnosti, události, indexery, operátory, konstruktory instancí, destruktory a statické konstruktory) a vnořené typy. Typy tříd podporují dědičnost, mechanismus, kterým odvozené třídy můžete rozšířit a specialize základní třídy.

## <a name="class-declarations"></a>Deklarace tříd

A *class_declaration* je *type_declaration* ([typ deklarace](namespaces.md#type-declarations)), který deklaruje novou třídu.

```antlr
class_declaration
    : attributes? class_modifier* 'partial'? 'class' identifier type_parameter_list?
      class_base? type_parameter_constraints_clause* class_body ';'?
    ;
```

A *class_declaration* se skládá z volitelné sadu *atributy* ([atributy](attributes.md)) následovaný volitelná sada *class_modifier*s ([třídy modifikátory](classes.md#class-modifiers)), následovaným volitelnou `partial` modifikátor, za nímž následuje klíčové slovo `class` a *identifikátor* , názvy tříd, za nímž následuje volitelné *type_parameter_list* ([parametry typu](classes.md#type-parameters)), následovaným volitelnou *class_base* specifikace ([třídy base specifikace](classes.md#class-base-specification)) následovaný volitelná sada *type_parameter_constraints_clause*s ([omezení parametru typu](classes.md#type-parameter-constraints)) a po něm *class_body*  ([Tělo třídy](classes.md#class-body)), volitelně za nímž následuje středníkem.

Deklarace třídy nelze zadat *type_parameter_constraints_clause*s Pokud také poskytuje *type_parameter_list*.

Deklarace třídy, která se dodává *type_parameter_list* je ***deklaraci obecné třídy***. Kromě toho všechny třídy vnořené v deklaraci obecné třídy nebo obecnou strukturu prohlášení je samotný deklaraci obecné třídy protože parametry typu pro nadřazený typ musí být poskytnuty vytvořit konstruovaný typ.

### <a name="class-modifiers"></a>Modifikátory třídy

A *class_declaration* může volitelně zahrnovat posloupnost modifikátory třídy:

```antlr
class_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    | 'abstract'
    | 'sealed'
    | 'static'
    | class_modifier_unsafe
    ;
```

Je chyba kompilace pro stejný modifikátor objevit více než jednou v deklaraci třídy.

`new` Smí obsahovat modifikátor u vnořených tříd. Určuje, že třída skrývá zděděného člena se stejným názvem, jak je popsáno v [new – modifikátor](classes.md#the-new-modifier). Je chyba kompilace pro `new` modifikátor se zobrazí v deklaraci třídy, která není deklarace vnořené třídy.

`public`, `protected`, `internal`, A `private` modifikátory řídit přístupnost člena třídy. V závislosti na kontextu, ve kterém dochází k deklaraci třídy, některé z těchto modifikátorů nemusí být povoleno ([deklarovaná přístupnost](basic-concepts.md#declared-accessibility)).

`abstract`, `sealed` a `static` modifikátory jsou popsány v následujících částech.

#### <a name="abstract-classes"></a>Abstraktní třídy

`abstract` Modifikátor se používá k označení, že třída je neúplný a že je určena pro použití pouze jako základní třídu. Abstraktní třída se liší od neabstraktní třídy následujícími způsoby:

*  Abstraktní třídu nelze přímo vytvořit instanci a je chyba kompilace pro použití `new` operátor u abstraktní třídy. I když je možné mít proměnných a hodnot, jejichž typy za kompilace, je abstraktních, tyto proměnné a jejich hodnoty nutně bude buď `null` nebo obsahovat odkazy na instance neabstraktní třídy odvozené od abstraktní typy.
*  Abstraktní třídy je povoleno (ale nevyžaduje) tak, aby obsahovala abstraktní členy.
*  Abstraktní třída nemůže být zapečetěný.

Když neabstraktní třídy je odvozen z abstraktní třídy, Neabstraktní třída musí obsahovat Skutečná implementace všechny zděděné abstraktní členy přepíše abstraktní členy. V příkladu
```csharp
abstract class A
{
    public abstract void F();
}

abstract class B: A
{
    public void G() {}
}

class C: B
{
    public override void F() {
        // actual implementation of F
    }
}
```
abstraktní třída `A` představuje abstraktní metody `F`. Třída `B` zavádí další metodu `G`, ale protože neposkytuje implementaci `F`, `B` také musí být deklarovaná jako abstraktní. Třída `C` přepíše `F` a poskytuje skutečné implementaci. Vzhledem k tomu, že neexistují žádní abstraktní členové v `C`, `C` je povoleno (ale nevyžaduje) být neabstraktní.

#### <a name="sealed-classes"></a>Zapečetěné třídy

`sealed` Modifikátor se používá při prevenci odvozování z třídy. Chyba kompilace nastane, pokud zapečetěné třídě je zadán jako základní třídou jiné třídy.

Zapečetěná třída nemůže být také abstraktní třídu.

`sealed` Modifikátor se používá především zabránit neúmyslnému odvození, ale umožňuje také některé optimalizace za běhu. Zejména protože zapečetěná třída se ví, nikdy nemůžete mít všechny odvozené třídy, je možné transformovat volání virtuální funkce člen v zapečetěné třídě instancí na nevirtuální volání.

#### <a name="static-classes"></a>Statické třídy

`static` Modifikátor se používá k označení třídy deklarované jako ***statické třídy***. Statická třída se nedá vytvořit instance, nelze použít jako typ a může obsahovat pouze statické členy. Pouze statické třídy mohou obsahovat deklarace metody rozšíření ([rozšiřující metody](classes.md#extension-methods)).

Statická třída deklarace je v souladu s následujícími omezeními:

*  Nesmí obsahovat statické třídy `sealed` nebo `abstract` modifikátor. Všimněte si však, že od statické třídy nelze vytvořit instanci nebo odvozený od, se chová jako by byl uzavřený a abstraktní.
*  Nesmí obsahovat statické třídy *class_base* specifikace ([specifikace základní třídy](classes.md#class-base-specification)) a nelze explicitně zadat základní třídu nebo seznamu implementovaných rozhraní. Statické třídy implicitně dědí z typu `object`.
*  Statická třída může obsahovat pouze statické členy ([statické a instance členy](classes.md#static-and-instance-members)). Všimněte si, že konstanty a vnořené typy jsou klasifikovány jako statické členy.
*  Statické třídy nemohou obsahovat členy s `protected` nebo `protected internal` deklarovaná přístupnost.

Je chyba kompilace na kterékoli z těchto omezení porušují.

Statická třída nemá žádné konstruktory instancí. Není možné deklarovat konstruktor instance ve statické třídě a žádný výchozí konstruktor instance ([výchozí konstruktory](classes.md#default-constructors)) je k dispozici pro statické třídě.

Členové statické třídy nejsou automaticky statické, a explicitně musí obsahovat deklarace členů `static` modifikátor (s výjimkou konstanty a vnořené typy). Když v rámci statických vnější třídy je vnořená třída, vnořené třídy není statická třída Pokud explicitně zahrnuje `static` modifikátor.

__Odkazování na typy statických tříd__

A *namespace_or_type_name* ([Namespace a zadejte názvy](basic-concepts.md#namespace-and-type-names)) odkazu statické třídy, pokud je povoleno

*  *Namespace_or_type_name* je `T` v *namespace_or_type_name* formuláře `T.I`, nebo
*  *Namespace_or_type_name* je `T` v *typeof_expression* ([seznamy argumentů](expressions.md#argument-lists)1) ve formátu `typeof(T)`.

A *primary_expression* ([funkce členy](expressions.md#function-members)) odkazu statické třídy, pokud je povoleno

*  *Primary_expression* je `E` v *member_access* ([kompilace kontrolu dynamické přetížení](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) ve formátu `E.I`.

V libovolném kontextu je chyba kompilace odkazovat statické třídě. Například, jedná se o chybu pro statické třídy má být použit jako základní třídu, základní typ ([vnořené typy](classes.md#nested-types)) člena, argument obecného typu nebo omezení parametru typu. Obdobně statické třídy nelze použít v typu pole, typ ukazatele, `new` výraz, výraz přetypování `is` výrazu, `as` výrazu, `sizeof` výraz nebo výraz výchozí hodnoty.

### <a name="partial-modifier"></a>Částečný modifikátor

`partial` Modifikátor se používá k označení, že tento *class_declaration* je částečný typ deklarace. Více deklaracích částečné typu se stejným názvem v rámci nadřazeného oboru názvů nebo typ deklarace se dá formuláře jeden typ deklarace, dle pravidel uvedených v [částečné typy](classes.md#partial-types).

S deklarace třídy distribuované přes oddělených segmentech textem programu může být užitečné, pokud jsou tyto segmenty vytvořeného nebo udržovat v různých kontextech. Jednou ze součástí sady deklaraci třídy, může být počítače, generovány, zatímco druhá je ručně vytvořená. Textové oddělení dvě zakáže aktualizace jednou v konfliktu s aktualizacemi jiným.

### <a name="type-parameters"></a>Parametry typu

Parametr typu je jednoduchý identifikátor, který označuje zástupný symbol pro argument typu zadaný konstruovaný typ vytvoření. Parametr typu je formální zástupný symbol pro typ, který budou poskytnuty později. Naopak argument typu ([argumenty typu](types.md#type-arguments)) je skutečný typ, který nahrazuje pro parametr typu, když se vytvoří konstruovaný typ.

```antlr
type_parameter_list
    : '<' type_parameters '>'
    ;

type_parameters
    : attributes? type_parameter
    | type_parameters ',' attributes? type_parameter
    ;

type_parameter
    : identifier
    ;
```

Každý z parametrů typu v deklaraci třídy definuje název v deklaraci prostoru ([deklarace](basic-concepts.md#declarations)) této třídy. Proto nemůže mít stejný název jako jiný typ parametru nebo člen deklarovaný v dané třídě. Parametr typu nemůže mít stejný název jako samotného typu.

### <a name="class-base-specification"></a>Specifikace základní třídy

Může obsahovat deklaraci třídy *class_base* specifikace, která definuje přímou základní třídu třídy a rozhraní ([rozhraní](interfaces.md)) přímo implementováno třídou.

```antlr
class_base
    : ':' class_type
    | ':' interface_type_list
    | ':' class_type ',' interface_type_list
    ;

interface_type_list
    : interface_type (',' interface_type)*
    ;
```

Základní třída zadaná v deklaraci třídy může být typem konstruovaný třídy ([vytvořený typy](types.md#constructed-types)). Základní třída nemůže být parametr typu sama o sobě, i když to může zahrnovat parametry typu, které jsou v oboru.

```csharp
class Extend<V>: V {}            // Error, type parameter used as base class
```

#### <a name="base-classes"></a>Základní třídy

Když *class_type* je součástí *class_base*, určuje přímou základní třídu třídy deklarované. Pokud deklarace třídy nemá žádné *class_base*, nebo pokud *class_base* seznamy pouze typy rozhraní, přímou základní třídu je považován za `object`. Třída dědí členy z její přímé základní třídy, jak je popsáno v [dědičnosti](classes.md#inheritance).

V příkladu
```csharp
class A {}

class B: A {}
```
Třída `A` je označen jako přímé základní třídy `B`, a `B` se říká, že nelze odvodit z `A`. Protože `A` nemá explicitně zadat přímou základní třídu, přímou základní třídu je implicitně `object`.

Pro typ vytvořeného třídy, pokud je zadaná základní třída v deklaraci obecné třídy základní třídy pro konstruovaný typ. se získá nahrazování pro každou *type_parameter* v deklaraci základní třídy, odpovídající *type_argument* sestaveného typu. Zadané deklarace obecných tříd
```csharp
class B<U,V> {...}

class G<T>: B<string,T[]> {...}
```
Základní třída konstruovaný typ `G<int>` by `B<string,int[]>`.

Přímou základní třídu typu třídy musí být přinejmenším stejně dostupná jako typ třídy ([usnadnění domén](basic-concepts.md#accessibility-domains)). Například je chyba kompilace pro `public` odvodit z třídy `private` nebo `internal` třídy.

Přímou základní třídu typu třídy nesmí být některý z následujících typů: `System.Array`, `System.Delegate`, `System.MulticastDelegate`, `System.Enum`, nebo `System.ValueType`. Kromě toho nelze použít deklaraci obecné třídy `System.Attribute` jako přímou nebo nepřímou základní třídy.

Při určování význam specifikaci přímou základní třídu `A` třídy `B`, přímé základní třídy `B` je dočasně předpokládá se, že `object`. Intuitivně tím se zajistí, že význam specifikace základní třídy nemůže rekurzivně závisí sám na sobě. Příklad:
```csharp
class A<T> {
   public class B {}
}

class C : A<C.B> {}
```
došlo k chybě od specifikace základní třídy `A<C.B>` přímé základní třídy `C` se považuje za `object`a proto (podle pravidel objektů [Namespace a zadejte názvy](basic-concepts.md#namespace-and-type-names)) `C` není považována za jste členem `B`.

Základní třídy typu třídy jsou přímou základní třídu a její základní třídy. Jinými slovy sadu základních tříd je přenositelný uzavření relace přímou základní třídu. V příkladu výše, základní třídy odkazující `B` jsou `A` a `object`. V příkladu
```csharp
class A {...}

class B<T>: A {...}

class C<T>: B<IComparable<T>> {...}

class D<T>: C<T[]> {...}
```
základní třídy `D<int>` jsou `C<int[]>`, `B<IComparable<int[]>>`, `A`, a `object`.

S výjimkou třídy `object`, každý typ třída nemá přesně jednu přímou základní třídu. `object` Třída nemá žádné přímé základní třídy a je ultimate základní třídou jiné třídy.

Pokud třída `B` je odvozena z třídy `A`, je chyba kompilace pro `A` závisí na `B`. Třída ***přímo závisí na*** své přímé základní třídy (pokud existuje) a ***přímo závisí na*** třídy, ve kterém je okamžitě vnořený (pokud existuje). Při této definici, kompletní sadu tříd, na kterých závisí třída je uzavření reflexivní a přenosné ***přímo závisí na*** vztah.

V příkladu
```csharp
class A: A {}
```
protože třída závisí sám na sobě je chybné. Podobně příklad
```csharp
class A: B {}
class B: C {}
class C: A {}
```
je v chybě, protože třídy záviset sama na sebe záviset samy na sobě. Nakonec příklad
```csharp
class A: B.C {}

class B: A
{
    public class C {}
}
```
výsledkem chyba kompilace, protože `A` závisí na `B.C` (své přímé základní třídy), závisí na `B` (jeho okamžitě ohraničující třídy), který odkazuje cyklicky sám závisí na `A`.

Všimněte si, že třída není závislý na třídy, které jsou v něm vnořený. V příkladu
```csharp
class A
{
    class B: A {}
}
```
`B` závisí na `A` (protože `A` přímou základní třídu a její okamžitě ohraničující třídy), ale `A` nezávisí na `B` (protože `B` není základní třídu ani nadřazené třídu `A` ). V příkladu je proto platný.

Není možné odvodit z `sealed` třídy. V příkladu
```csharp
sealed class A {}

class B: A {}            // Error, cannot derive from a sealed class
```
Třída `B` je v chybě, protože se pokusí o odvozovat `sealed` třídy `A`.

#### <a name="interface-implementations"></a>Implementace rozhraní

A *class_base* specifikace může obsahovat seznam typů rozhraní, ve kterých případ třídu se říká, že přímo implementaci typů dané rozhraní. Implementace rozhraní jsou popsány dále v [rozhraní implementace](interfaces.md#interface-implementations).

### <a name="type-parameter-constraints"></a>Omezení parametru typu

Obecná deklarace typů a metod, můžete volitelně zadat omezení parametru typu zahrnutím *type_parameter_constraints_clause*s.

```antlr
type_parameter_constraints_clause
    : 'where' type_parameter ':' type_parameter_constraints
    ;

type_parameter_constraints
    : primary_constraint
    | secondary_constraints
    | constructor_constraint
    | primary_constraint ',' secondary_constraints
    | primary_constraint ',' constructor_constraint
    | secondary_constraints ',' constructor_constraint
    | primary_constraint ',' secondary_constraints ',' constructor_constraint
    ;

primary_constraint
    : class_type
    | 'class'
    | 'struct'
    ;

secondary_constraints
    : interface_type
    | type_parameter
    | secondary_constraints ',' interface_type
    | secondary_constraints ',' type_parameter
    ;

constructor_constraint
    : 'new' '(' ')'
    ;
```

Každý *type_parameter_constraints_clause* se skládá z tokenu `where`, za nímž následuje název parametru typu, za nímž následuje dvojtečka a seznam omezení za parametr daného typu. Může existovat maximálně jeden `where` klauzuli pro každý parametr typu a `where` klauzule výpis je možný v libovolném pořadí. Podobně jako `get` a `set` tokeny v přistupující objekt vlastnosti `where` token není klíčové slovo.

Omezení uvedená v seznamu `where` klauzule můžete zahrnout jakýkoli z následujících komponent, které v tomto pořadí: jen jedno omezení primárního, jeden nebo více sekundárních omezení a omezení konstruktoru `new()`.

Omezení primárního může být typem třídy nebo ***referenční omezení typu*** `class` nebo ***hodnotu omezení typu*** `struct`. Sekundární omezení může být *type_parameter* nebo *interface_type*.

Omezení typu odkazu Určuje, že typ argumentu použitý pro parametr typu musí být typ odkazu. Všechny typy tříd, typy rozhraní, delegáta typy, typy polí a parametry typu, které jsou známé jako typ odkazu (jak je definována níže) splňovat toto omezení.

Omezení typu hodnota určuje, že typ argumentu použitý pro parametr typu musí být typu hodnotu Null. Všechny typy neumožňující hodnotu struktury, výčtové typy a parametry typu s omezení typu hodnota splňovat toto omezení. Všimněte si, že i když jsou klasifikovány jako typ hodnoty, typ připouštějící hodnotu Null ([typy připouštějící hodnotu Null](types.md#nullable-types)) nevyhovuje omezení typu hodnoty. Parametr typu s omezení typu hodnota nemůže mít také *constructor_constraint*.

Typy ukazatelů nikdy mohou být argumenty typu a nejsou považovány za splňovat buď odkaz na typ nebo hodnota typu omezení.

Pokud omezení typu třídy, typ rozhraní nebo parametr typu, typu Určuje minimální "základní typ", který musí podporovat každý typ argumentu použitý pro parametr typu. Pokaždé, když se používá vytvořeného typu nebo obecné metody, typ argumentu je porovnávána s omezeními u parametru typu v době kompilace. Zadaný argument typu musí splňovat požadavky popsané v [neodpovídajících omezení](types.md#satisfying-constraints).

A *class_type* omezení musí splňovat následující pravidla:

*  Typ musí být typu třídy.
*  Typ nesmí být `sealed`.
*  Typ nesmí být jedním z následujících typů: `System.Array`, `System.Delegate`, `System.Enum`, nebo `System.ValueType`.
*  Typ nesmí být `object`. Protože všechny typy jsou odvozeny z `object`, taková omezení by neměl žádný vliv, pokud byly převody povoleny.
*  Typ třídy může být maximálně jedno omezení pro parametr daného typu.

Typ zadaný jako *interface_type* omezení musí splňovat následující pravidla:

*  Typ musí být typem rozhraní.
*  Typ nesmí být zadáno více než jednou v danou `where` klauzuli.

V obou případech omezení může zahrnovat parametry typu přidruženého typu nebo deklarace metody jako součást konstruovaný typ a může zahrnovat typ byl deklarován.

Všechny třídy nebo rozhraní typ zadaný jako omezení parametru typu musí být přinejmenším stejně dostupná ([usnadnění omezení](basic-concepts.md#accessibility-constraints)) jako obecného typu nebo metody deklarované.

Typ zadaný jako *type_parameter* omezení musí splňovat následující pravidla:

*  Typ musí být parametry typu.
*  Typ nesmí být zadáno více než jednou v danou `where` klauzuli.

Kromě toho musí existovat žádné cyklické v grafu závislostí parametrů typu, kde závislost je přenositelný vztah určené:

*  Pokud parametr typu `T` je použitý jako omezení pro parametr typu `S` pak `S` ***závisí na*** `T`.
*  Pokud parametr typu `S` závisí na parametr typu `T` a `T` závisí na parametr typu `U` pak `S` ***závisí na*** `U`.

Zadaný tento vztah, je chyba kompilace pro parametr typu závisí sám na sobě (přímo nebo nepřímo).

Žádná omezení musí být konzistentní mezi parametry závislého typu. Pokud zadáte parametr `S` závisí na typu parametru `T` pak:

*  `T` nesmí mít hodnotu omezení typu. V opačném případě `T` zapečetěn efektivně tak `S` by se vynutilo být stejného typu jako `T`, takže odpadá potřeba dva parametry typu.
*  Pokud `S` má omezení hodnoty typu pak `T` nesmí mít *class_type* omezení.
*  Pokud `S` má *class_type* omezení `A` a `T` má *class_type* omezení `B` musí být konverzi identity nebo implicitní referenční převod z `A` k `B` nebo implicitní referenční převod z `B` k `A`.
*  Pokud `S` závisí také na parametr typu `U` a `U` má *class_type* omezení `A` a `T` má *class_type* omezení `B` pak musíte být správcem nebo implicitní referenční převod z identity převod `A` k `B` nebo implicitní referenční převod z `B` k `A`.

Je platný pro `S` mít omezení typu hodnoty a `T` mít omezení typu odkazu. Efektivně to omezuje `T` typům `System.Object`, `System.ValueType`, `System.Enum`a libovolný typ rozhraní.

Pokud `where` klauzule pro parametr typu obsahuje omezení konstruktoru (která má tvar `new()`), je možné použít `new` operátoru pro vytvoření instancí typu ([vytváření výrazyobjektu](expressions.md#object-creation-expressions)). Jakýkoli typ argument použitý pro parametr typu s omezením konstruktor musí mít veřejný konstruktor bez parametrů (Tento konstruktor implicitně existuje pro libovolný typ hodnoty), nebo jako parametr typu s hodnotou typu omezení nebo omezení konstruktoru (viz [Omezení parametru typu](classes.md#type-parameter-constraints) podrobnosti).

Následují příklady omezení:
```csharp
interface IPrintable
{
    void Print();
}

interface IComparable<T>
{
    int CompareTo(T value);
}

interface IKeyProvider<T>
{
    T GetKey();
}

class Printer<T> where T: IPrintable {...}

class SortedList<T> where T: IComparable<T> {...}

class Dictionary<K,V>
    where K: IComparable<K>
    where V: IPrintable, IKeyProvider<K>, new()
{
    ...
}
```

V následujícím příkladu je v chybě, protože to způsobí, že Cykličnost v grafu závislostí parametrů typu:
```csharp
class Circular<S,T>
    where S: T
    where T: S                // Error, circularity in dependency graph
{
    ...
}
```

Následující příklady znázorňují neplatný několika dalších situacích:
```csharp
class Sealed<S,T>
    where S: T
    where T: struct        // Error, T is sealed
{
    ...
}

class A {...}

class B {...}

class Incompat<S,T>
    where S: A, T
    where T: B                // Error, incompatible class-type constraints
{
    ...
}

class StructWithClass<S,T,U>
    where S: struct, T
    where T: U
    where U: A                // Error, A incompatible with struct
{
    ...
}
```

***Efektivní základní třídy*** parametru typu `T` je definovaná následujícím způsobem:

*  Pokud `T` nemá žádné omezení primárního nebo omezení parametru typu, její základní třída je `object`.
*  Pokud `T` má omezení typu hodnoty, je její základní třída `System.ValueType`.
*  Pokud `T` má *class_type* omezení `C` ale žádné *type_parameter* omezení, její základní třída je `C`.
*  Pokud `T` nemá žádné *class_type* omezení, ale má jeden nebo více *type_parameter* omezení, její základní třída je nejvíce encompassed typ ([zrušit převod operátory](conversions.md#lifted-conversion-operators)) v sadě efektivní základní třídy jeho *type_parameter* omezení. Pravidla konzistence Ujistěte se, že většina encompassed typ existuje.
*  Pokud `T` jsou obě *class_type* omezení a jeden nebo více *type_parameter* omezení, její základní třída je nejvíce encompassed typ ([zrušit převod operátory](conversions.md#lifted-conversion-operators)) v sadě, který se skládá z *class_type* omezení `T` a efektivní základní třídy jeho *type_parameter* omezení. Pravidla konzistence Ujistěte se, že většina encompassed typ existuje.
*  Pokud `T` má omezení typu odkaz, ale ne *class_type* omezení, její základní třída je `object`.

Pro účely těchto pravidel, pokud T má omezení `V` , který je *value_type*, použijte místo toho nejspecifičtější základního typu `V` , který je *class_type*. To může nikdy nemělo nastat v explicitně dané omezení, ale může dojít, pokud omezení obecné metody jsou implicitně dědí přepsání deklarace metody nebo explicitní implementaci metody rozhraní.

Tato pravidla ujistěte se, že efektivní základní třídy je vždy *class_type*.

***Rozhraní efektivní sada*** parametru typu `T` je definovaná následujícím způsobem:

*  Pokud `T` nemá žádné *secondary_constraints*, jeho rozhraní efektivní sada byla prázdná.
*  Pokud `T` má *interface_type* omezení, ale ne *type_parameter* omezení, jeho sady efektivní rozhraní je sada *interface_type* omezení.
*  Pokud `T` nemá žádné *interface_type* ale má omezení *type_parameter* omezení, je jeho efektivní rozhraní sady sjednocení sady efektivní rozhraní jeho *type_ Parametr* omezení.
*  Pokud `T` jsou obě *interface_type* omezení a *type_parameter* omezení, je jeho efektivní rozhraní sady sjednocení svou sadu *interface_type* omezení a efektivní rozhraní sady jeho *type_parameter* omezení.

Parametr typu je ***známé jako typ odkazu*** Pokud má omezení typu odkazu nebo její základní třída není `object` nebo `System.ValueType`.

Hodnoty typu parametru omezeného typu lze použít pro přístup ke členům instance odvozené od omezení. V příkladu
```csharp
interface IPrintable
{
    void Print();
}

class Printer<T> where T: IPrintable
{
    void PrintOne(T x) {
        x.Print();
    }
}
```
metody `IPrintable` lze vyvolat přímo na `x` protože `T` je omezen na vždy implementovat `IPrintable`.

### <a name="class-body"></a>Tělo třídy

*Class_body* třídy definuje členy této třídy.

```antlr
class_body
    : '{' class_member_declaration* '}'
    ;
```

## <a name="partial-types"></a>Částečné typy

Deklarace typu je možné rozdělit mezi několik ***částečného typu deklarace***. Deklarace typu je vytvořen z jejích částí pomocí následujících pravidel v této části, přičemž je považován za jednu deklaraci během zbývající zpracování kompilace a za běhu programu.

A *class_declaration*, *struct_declaration* nebo *interface_declaration* představuje částečný typ deklarace, obsahuje-li `partial` modifikátor. `partial` není klíčové slovo a pouze pokud se zobrazí jedna z klíčových slov bezprostředně před funguje jako modifikátor `class`, `struct` nebo `interface` v deklaraci typu nebo před typem `void` v deklaraci metody. V jiných kontextech můžete použít jako normální identifikátor.

Každá část deklarace částečný typ jsou povinné `partial` modifikátor. Musí mít stejný název a být deklarované v stejný obor názvů nebo typ deklarace jako jiné části. `partial` Modifikátor vyplývá, že další části deklarace typu může existovat někde jinde, ale existenci těchto dalších částí není povinné, je platný pro typ s deklarací jedné zahrnout `partial` modifikátor.

Všechny části částečného typu musí být kompilovány společně tak, aby části lze sloučit v době kompilace do jedinou deklaraci typu. Částečné typy výslovně neumožňují již kompilované typy prodloužit.

Vnořené typy lze deklarovat v více částí s použitím `partial` modifikátor. Obvykle obsahující typ je deklarován pomocí `partial` , jak dobře a každá část vnořený typ je deklarovaný v jiné části nadřazeného typu.

`partial` Modifikátoru není povolena u deklarace delegáta nebo výčtu.

### <a name="attributes"></a>Atributy

Atributy částečný typ jsou určeny v nespecifikovaném pořadí kombinaci atributů jednotlivých součástí. Pokud atribut je umístěn na více částí, je ekvivalentní se zadáním atribut více než jednou na typu. Například dvě části:

```csharp
[Attr1, Attr2("hello")]
partial class A {}

[Attr3, Attr2("goodbye")]
partial class A {}
```
jsou ekvivalentní deklarace, jako:
```csharp
[Attr1, Attr2("hello"), Attr3, Attr2("goodbye")]
class A {}
```

Atributy u parametrů typu kombinovat podobným způsobem.

### <a name="modifiers"></a>Modifikátory

Při deklaraci částečného typu zahrnuje specifikaci přístupnosti ( `public`, `protected`, `internal`, a `private` modifikátory) musíte souhlasit s všechny ostatní součásti, které zahrnují specifikaci usnadnění přístupu. Pokud žádná část částečného typu obsahuje specifikaci usnadnění přístupu, typ je předána odpovídající výchozí dostupnost ([deklarovaná přístupnost](basic-concepts.md#declared-accessibility)).

Pokud jeden nebo více částečné deklarace vnořeného typu `new` modifikátor, bez upozornění bude nahlášena, pokud vnořeného typu skrývá zděděný člen ([skrytí prostřednictvím dědičnosti](basic-concepts.md#hiding-through-inheritance)).

Pokud jeden nebo více částečné deklarace třídy zahrnují `abstract` modifikátor, třída je považována za abstraktní ([abstraktní třídy](classes.md#abstract-classes)). Třídy v opačném případě se považuje za neabstraktní.

Pokud jeden nebo více částečné deklarace třídy `sealed` modifikátor, třída je považován za zapečetěné ([zapečetěné třídy](classes.md#sealed-classes)). V opačném případě se považuje za třídu nezapečetěné.

Všimněte si, že třída nemůže být extern i sealed.

Když `unsafe` modifikátor se používá v deklaraci částečné typu jen pro konkrétní části je považován za kontextu unsafe ([nezabezpečený kontext](unsafe-code.md#unsafe-contexts)).

### <a name="type-parameters-and-constraints"></a>Parametry typu a omezení

Pokud obecný typ je deklarován ve více částí, musí každá část stavu parametry typu. Každá část musí mít stejný počet parametrů typu a stejný název pro každý parametr typu v pořadí.

Pokud obsahuje deklaraci částečný obecný typ omezení (`where` klauzule), omezení musíte souhlasit s jinými částmi, které zahrnují omezení. Konkrétně jednotlivé části, která zahrnuje omezení musí mít omezení pro stejnou sadu parametrů typu, a pro každý parametr typu sady primární, sekundární a omezení konstruktoru musí být ekvivalentní. Dvě sady omezení jsou rovnocenné, pokud obsahují stejné členy. Pokud žádná část částečný obecný typ omezení parametru typu, typu, který parametry se považují za vstupy bez omezení.

V příkladu
```csharp
partial class Dictionary<K,V>
    where K: IComparable<K>
    where V: IKeyProvider<K>, IPersistable
{
    ...
}

partial class Dictionary<K,V>
    where V: IPersistable, IKeyProvider<K>
    where K: IComparable<K>
{
    ...
}

partial class Dictionary<K,V>
{
    ...
}
```
správnost vzhledem k tomu, že tyto součásti, které zahrnují omezení (první dva) efektivně zadejte stejnou sadu primární, sekundární a omezení konstruktoru pro stejnou sadu parametrů typu v uvedeném pořadí.

### <a name="base-class"></a>Základní třída

Při deklaraci částečné třídy obsahuje specifikace základní třídy musíte souhlasit s jinými částmi, které obsahují specifikace základní třídy. Pokud žádná část částečné třídy obsahuje specifikace základní třídy, stane se základní třídy `System.Object` ([základních tříd](classes.md#base-classes)).

### <a name="base-interfaces"></a>Základní rozhraní

Sada základních rozhraní pro typ deklarovaný v několika částí je sjednocení základních rozhraní zadané u každé části. Určité základní rozhraní může mít pouze jednou název na jednotlivé části, ale je povoleno více částí název stejné základních rozhraní. Musí existovat pouze jedna implementace členů jakékoli dané základní rozhraní.

V příkladu
```csharp
partial class C: IA, IB {...}

partial class C: IC {...}

partial class C: IA, IB {...}
```
Sada základních rozhraní pro třídu `C` je `IA`, `IB`, a `IC`.

Obvykle každá část poskytuje implementaci rozhraní deklarované na takovou stranou, Nicméně toto není povinné. Část může poskytnout implementaci pro deklarované na jinou část rozhraní:
```csharp
partial class X
{
    int IComparable.CompareTo(object o) {...}
}

partial class X: IComparable
{
    ...
}
```

### <a name="members"></a>Členové

S výjimkou částečné metody ([částečné metody](classes.md#partial-methods)), sadu členů z typu deklarovaného ve více částí je jednoduše sjednocení sadu členy s deklarací v každé části. Subjekty všech částí deklarace typu sdílejí stejný obor deklarace ([deklarace](basic-concepts.md#declarations)) a obor každého člena ([obory](basic-concepts.md#scopes)) rozšiřuje do těla všechny části. Doména přístupnosti člena vždy obsahuje všechny části nadřazeného typu.; `private` člen deklarovaný v jedné části volně přístupná z jiné části. Je chyba kompilace pro deklaraci na stejný člen ve více než jednu část typu, pokud je daný člen typu s `partial` modifikátor.

```csharp
partial class A
{
    int x;                     // Error, cannot declare x more than once

    partial class Inner        // Ok, Inner is a partial type
    {
        int y;
    }
}

partial class A
{
    int x;                     // Error, cannot declare x more than once

    partial class Inner        // Ok, Inner is a partial type
    {
        int z;
    }
}
```

Řazení členů v rámci typu je zřídka důležité pro kód jazyka C#, ale může být důležité při propojování s jinými jazyky a prostředí. V těchto případech není definováno pořadí členů v rámci typu deklarovaného ve více částí.

### <a name="partial-methods"></a>Částečné metody

Částečné metody můžete definovanou v jedné části deklarace typu a implementovat v jiném. Implementace je volitelná. Pokud žádná část implementuje částečné metody, deklaraci částečné metody a všechna volání se odeberou z deklarace typu, který je výsledkem kombinace částí.

Částečné metody nelze definovat modifikátory přístupu, ale jsou implicitně `private`. Jejich návratový typ musí být `void`, a jejich parametrů nemůže mít `out` modifikátor. Identifikátor `partial` je rozpoznán jako speciální – klíčové slovo v deklaraci metody pouze v případě, že se zobrazí přímo před `void` typ; jinak mohou být použity jako normální identifikátor. Částečná metoda nesmí explicitně implementovat metody rozhraní.

Existují dva druhy deklarace částečné metody: Pokud text v deklaraci metody je středník, deklarace se říká, že ***definující deklarace částečné metody***. Pokud text je zadána jako *bloku*, deklarace se říká, že ***implementující deklaraci částečné metody***. V části deklarace typu může existovat pouze jedna definice deklarace částečné metody s daným podpisem. a může existovat pouze jeden implementující deklaraci částečné metody s daným podpisem. Pokud je zadána implementující deklaraci částečné metody, odpovídající definující deklarace částečné metody musí existovat a deklarace musí odpovídat jako zadaný v následujících tématech:

* Deklarace musí mít stejné modifikátory (i když nemusí ve stejném pořadí), název metody, počet parametrů typu a počtu parametrů.
* Odpovídajícím parametrům in v prohlášení musí mít stejné modifikátory (i když nemusí ve stejném pořadí) a stejnými typy (modulo rozdíly v názvy parametrů typů).
* Odpovídající parametry typu v prohlášení musí mít stejné omezení (modulo rozdíly v názvy parametrů typů).

Implementující deklaraci částečné metody se může zobrazit v části stejné jako odpovídající definující deklarace částečné metody.

Pouze definuje částečné metody se účastní řešení přetížení. To znamená zda implementující deklarace není uveden, výrazy typu invocation může vyřešit volání částečné metody. Protože je částečná metoda vždy vrátí `void`, tyto výrazy typu invocation bude vždy příkazy výrazů. Navíc protože částečné metody je implicitně `private`, tyto příkazy se vždy objevují v rámci jedna z částí deklarace typu, ve kterém je deklarována jako částečná metoda.

Pokud žádná z částí deklarace částečného typu obsahuje implementující deklarace pro dané částečné metody, libovolný příkaz výrazu vyvolání jednoduše odebrán kombinované typ deklarace. Proto volání výrazu, včetně všech základních výrazů nemá žádný vliv za běhu. Částečná metoda sama se také odebere a nesmí být členem skupiny kombinované typ deklarace.

Pokud implementující deklarace pro dané částečné metody vyvolání částečné metody se zachovají. Částečné metody vede k deklaraci metody podobné pro implementující deklaraci částečné metody s těmito výjimkami:

* `partial` Modifikátor není součástí
* Atributy ve výsledné deklarace metody jsou kombinované atributy definování a implementující deklaraci částečné metody v nespecifikovaném pořadí. Duplicitní položky se neodeberou.
* Atributy pro parametry výsledný deklarace metody jsou kombinované atributů odpovídajících parametrů definování a implementující deklaraci částečné metody v nespecifikovaném pořadí. Duplicitní položky se neodeberou.

Pokud definující deklarací, ale ne implementující deklarace částečné metody M, platí následující omezení:

* Jde chybu v době kompilace k vytvoření delegáta metodě ([delegovat vytváření výrazů](expressions.md#delegate-creation-expressions)).
* Jde chybu v době kompilace k odkazování na `M` uvnitř anonymní funkce převedená na typu stromu výrazu ([vyhodnocení anonymní funkce převody na typy stromu výrazů](conversions.md#evaluation-of-anonymous-function-conversions-to-expression-tree-types)).
* Výrazy, ke kterým dochází v rámci vyvolání `M` nemají vliv na stav jednoznačného přiřazení ([jednoznačného přiřazení](variables.md#definite-assignment)), který může potenciálně vést k chybám kompilace.
* `M` nemůže být vstupním bodem pro aplikaci ([spuštění aplikace](basic-concepts.md#application-startup)).

Částečné metody jsou užitečné pro povolení jednu část deklarace typu k přizpůsobení chování jiné části, například takový, který je vygenerován pomocí nástroje. Vezměte v úvahu následující deklarace částečné třídy:
```csharp
partial class Customer
{
    string name;

    public string Name {
        get { return name; }
        set {
            OnNameChanging(value);
            name = value;
            OnNameChanged();
        }

    }

    partial void OnNameChanging(string newName);

    partial void OnNameChanged();
}
```

Pokud tato třída je kompilována bez dalších částí, definující deklarace částečné metody a jejich volání se odeberou a výsledné deklarace kombinované třídy budou ekvivalentní následujícímu zápisu:
```csharp
class Customer
{
    string name;

    public string Name {
        get { return name; }
        set { name = value; }
    }
}
```

Předpokládejme, že jiné části je uveden, ale poskytující implementující deklaraci částečné metody:
```csharp
partial class Customer
{
    partial void OnNameChanging(string newName)
    {
        Console.WriteLine("Changing " + name + " to " + newName);
    }

    partial void OnNameChanged()
    {
        Console.WriteLine("Changed to " + name);
    }
}
```

Pak bude výsledná deklarace kombinované třídy ekvivalentní následujícímu zápisu:
```csharp
class Customer
{
    string name;

    public string Name {
        get { return name; }
        set {
            OnNameChanging(value);
            name = value;
            OnNameChanged();
        }

    }

    void OnNameChanging(string newName)
    {
        Console.WriteLine("Changing " + name + " to " + newName);
    }

    void OnNameChanged()
    {
        Console.WriteLine("Changed to " + name);
    }
}
```

### <a name="name-binding"></a>Název vazby

I když každá část rozšiřitelný typ musí být deklarovány v rámci stejného oboru názvů, části jsou obvykle napsány v rámci jiný obor názvů deklarace. Díky tomu se různé `using` direktivy ([direktiv Using](namespaces.md#using-directives)) může být k dispozici pro každou část. Při interpretaci jednoduché názvy ([odvození typu](expressions.md#type-inference)) v rámci jedné části pouze `using` direktivy deklarace oboru názvů uzavírající této části jsou považovány za. Výsledkem může být stejný identifikátor mají různý význam v různých částech:
```csharp
namespace N
{
    using List = System.Collections.ArrayList;

    partial class A
    {
        List x;                // x has type System.Collections.ArrayList
    }
}

namespace N
{
    using List = Widgets.LinkedList;

    partial class A
    {
        List y;                // y has type Widgets.LinkedList
    }
}
```

## <a name="class-members"></a>Členy třídy

Členy třídy se skládá z členů zavedených v jeho *class_member_declaration*s a členy zděděné od přímé základní třídy.

```antlr
class_member_declaration
    : constant_declaration
    | field_declaration
    | method_declaration
    | property_declaration
    | event_declaration
    | indexer_declaration
    | operator_declaration
    | constructor_declaration
    | destructor_declaration
    | static_constructor_declaration
    | type_declaration
    ;
```

Členy typu třídy jsou rozdělené do následujících kategorií:

*  Konstanty, které představují konstanty, které jsou přidružené k třídě ([konstanty](classes.md#constants)).
*  Pole, které jsou proměnné třídy ([pole](classes.md#fields)).
*  Metody, které implementují výpočtů a akcí, které lze provést pomocí třídy ([metody](classes.md#methods)).
*  Vlastnosti, které definují pojmenované vlastnosti a akce přidružené k čtení a zápis těchto vlastností ([vlastnosti](classes.md#properties)).
*  Události, které definují oznámení, která mohou být generovány třídu ([události](classes.md#events)).
*  Indexery, které povolují instance třídy, které mají být indexovány v stejným způsobem (syntakticky) jako pole ([indexery](classes.md#indexers)).
*  Operátory, které definují operátory výrazů, které lze použít u instance třídy ([operátory](classes.md#operators)).
*  Instance konstruktory, které implementují akce potřebné k inicializaci instance třídy ([Instance konstruktory](classes.md#instance-constructors))
*  Destruktory, které implementují akce, jež mají být provedeny před instancí třídy budou trvale odstraněny ([destruktory](classes.md#destructors)).
*  Statické konstruktory, které implementují akce potřebné k inicializaci vlastní třídy ([statické konstruktory](classes.md#static-constructors)).
*  Typy, které představují typy, které jsou místní pro třídu ([vnořené typy](classes.md#nested-types)).

Členy, které mohou obsahovat spustitelného kódu jsou souhrnně označovány jako *funkce členy* typu třídy. Funkce členy typu třídy jsou metody, vlastnosti, události, indexery, operátory, konstruktory instancí, destruktory a statických konstruktorů tohoto typu třídy.

A *class_declaration* vytvoří nové prohlášení prostor ([deklarace](basic-concepts.md#declarations)) a *class_member_declaration*s okamžitě obsažených *třídy _declaration* zavést nové členy do tohoto prostoru deklarace. Následující pravidla platí pro *class_member_declaration*s:

*  Instance konstruktory, destruktory a statické konstruktory musí mít stejný název jako okamžitě nadřazené třídy. Všechny ostatní členové musí mít názvy, které se liší od názvu okamžitě nadřazené třídy.
*  Název – konstanta, pole, vlastnosti, události nebo typ se musí lišit od názvů ostatních členů deklarované ve stejné třídě.
*  Název metody se musí lišit od názvů ostatních bez metody s deklarací ve stejné třídě. Kromě toho, podpis ([podpisy a přetížení](basic-concepts.md#signatures-and-overloading)) z metody se musí lišit od podpisů všechny ostatní metody s deklarací ve stejné třídě, a dvě metody s deklarací ve stejné třídě, nemusí mít podpisy, které se liší pouze podle `ref` a `out`.
*  Podpis konstruktoru instance se musí lišit od podpisů všechny ostatní instance konstruktory deklarované ve stejné třídě, a dva konstruktory deklarované ve stejné třídě, nemusí mít podpisy, které se liší pouze `ref` a `out`.
*  Podpis indexeru se musí lišit od podpisů ze všech indexerů deklarované ve stejné třídě.
*  Podpis operátora se musí lišit od podpisů všechny ostatní operátory, které jsou deklarovány ve stejné třídě.

Zděděné členy typu třídy ([dědičnosti](classes.md#inheritance)) nejsou součástí místa deklarace třídy. Proto odvozená třída může deklarovat člena se stejným názvem a signaturou jako zděděného člena (což ve skutečnosti skrývá zděděný člen).

### <a name="the-instance-type"></a>Typ instance

Každou deklaraci třídy je přidružen vázaný typ ([vázaný a nevázaných typy](types.md#bound-and-unbound-types)), ***typ instance***. Pro deklaraci obecné třídy, je vytvořen typ instance tak, že vytvoříte konstruovaný typ ([vytvořený typy](types.md#constructed-types)) z deklarace typu pro každý zadaný typ argumentů se odpovídající parametr typu. Protože typ instance používá parametry typu, ho jde použít jenom ve kterém jsou parametry typu v oboru, To znamená uvnitř deklarace třídy. Typ instance je typu `this` pro kód napsaný v deklaraci třídy. Typ instance pro obecné třídy, je jednoduše deklarované třídy. Následuje několik deklarace tříd spolu s jejich typy instancí: 
```csharp
class A<T>                           // instance type: A<T>
{
    class B {}                       // instance type: A<T>.B
    class C<U> {}                    // instance type: A<T>.C<U>
}

class D {}                           // instance type: D
```

### <a name="members-of-constructed-types"></a>Členové sestavené typy

Získávají se-zděděné členy konstruovaný typ nahrazením pro každou *type_parameter* v deklaraci člena odpovídajícího *type_argument* sestaveného typu. Proces nahrazení vychází sémantický význam deklarace typů a není jednoduše textové nahrazení.

Mějme například deklaraci obecné třídy
```csharp
class Gen<T,U>
{
    public T[,] a;
    public void G(int i, T t, Gen<U,T> gt) {...}
    public U Prop { get {...} set {...} }
    public int H(double d) {...}
}
```
konstruovaný typ `Gen<int[],IComparable<string>>` má následující členy:
```csharp
public int[,][] a;
public void G(int i, int[] t, Gen<IComparable<string>,int[]> gt) {...}
public IComparable<string> Prop { get {...} set {...} }
public int H(double d) {...}
```

Typ člena `a` v deklaraci obecné třídy `Gen` je "dvourozměrné pole `T`", protože typ člena `a` ve výše uvedené konstruovaný typ je "dvourozměrné pole jednorozměrné pole `int`", nebo `int[,][]`.

V rámci instance funkce členy typu `this` je typu instance ([typ instance](classes.md#the-instance-type)) obsahující deklarace.

Všichni členové obecné třídy můžete použít parametry typu z nadřazené třídy, buď přímo, nebo jako součást konstruovaný typ. Když konkrétní uzavřený konstruovaný typ. ([otevřené a uzavřené typy](types.md#open-and-closed-types)) se používá v době běhu, každé použití klíčového parametr typu je nahrazen skutečným typem argument zadaný pro konstruovaný typ. Příklad:
```csharp
class C<V>
{
    public V f1;
    public C<V> f2 = null;

    public C(V x) {
        this.f1 = x;
        this.f2 = this;
    }
}

class Application
{
    static void Main() {
        C<int> x1 = new C<int>(1);
        Console.WriteLine(x1.f1);        // Prints 1

        C<double> x2 = new C<double>(3.1415);
        Console.WriteLine(x2.f1);        // Prints 3.1415
    }
}
```

### <a name="inheritance"></a>Dědičnost

Třída ***dědí*** členy jeho přímou základní třídu typu. Dědičnost znamená, že třídy implicitně obsahuje všechny členy jeho přímou základní třídu typu s výjimkou instance konstruktory, destruktory a statické konstruktory základní třídy. Jsou některé důležité aspekty dědičnosti:

*  Dědičnost je přenosné. Pokud `C` je odvozen z `B`, a `B` je odvozen z `A`, pak `C` dědí členy deklarované v `B` a také členy deklarované v `A`.
*  Odvozená třída rozšiřuje své přímé základní třídy. Odvozené třídy můžete přidat nové členy pro ty, které se dědí, ale nemůže odstranit definici zděděného člena.
*  Instance konstruktory, destruktory a statické konstruktory nejsou děděna, ale ostatní členové jsou, bez ohledu na jejich deklarovaná přístupnost ([přístup ke členu](basic-concepts.md#member-access)). V závislosti na jejich deklarovaná přístupnost, nemusí být přístupné v odvozené třídě zděděných členů.
*  Odvozená třída může ***skrýt*** ([skrytí prostřednictvím dědičnosti](basic-concepts.md#hiding-through-inheritance)) zděděné členy deklarováním nové členy s totožným názvem a signaturou. Nezapomeňte ale, že skrytím zděděného člena neodebere, se kterou – umožňuje pouze tento člen přístupný přímo prostřednictvím odvozené třídy.
*  Instance třídy obsahuje sadu všechna pole instancí, které jsou deklarovány ve třídě a jejích základních tříd a implicitní převod ([odkaz na implicitní převody](conversions.md#implicit-reference-conversions)) existuje z typu odvozené třídy pro některé typy jejího základní třídy. Díky tomu se odkaz na instanci některé z odvozených tříd lze považovat za odkaz na instanci některý z jeho základních tříd.
*  Třídy lze deklarovat virtuální metody, vlastnostmi a indexery a odvozených tříd může přepsat implementaci těchto funkcí členů. To umožňuje polymorfní chování, ve které akce prováděné při volání členské funkce se liší v závislosti na run-time typu instance, jejímž prostřednictvím je vyvolána funkce člena třídy.

Zděděný člen typu vytvořeného třídy jsou členy typu přímé základní třídy ([základních tříd](classes.md#base-classes)), která byla nalezena nahrazením argumentů typu sestaveného typu pro každý výskyt odpovídající typ v parametrech *class_base* specifikace. Tyto členy, pak jsou transformovány sadou nahrazování pro každou *type_parameter* v deklaraci člena odpovídajícího *type_argument* z *class_base* specifikace.

```csharp
class B<U>
{
    public U F(long index) {...}
}

class D<T>: B<T[]>
{
    public T G(string s) {...}
}
```

V příkladu výše, konstruovaný typ `D<int>` má člen zděděné `public int G(string s)` získala při nahrazování argument typu `int` pro parametr typu `T`. `D<int>` má také zděděného členu v deklaraci třídy `B`. Tento člen zděděný je určeno první typ základní třídy určující `B<int[]>` z `D<int>` nahrazením `int` pro `T` ve specifikaci základní třídy `B<T[]>`. Potom jako argument typu pro `B`, `int[]` nahrazuje `U` v `public U F(long index)`, což má za následek zděděný člen `public int[] F(long index)`.

### <a name="the-new-modifier"></a>New – modifikátor

A *class_member_declaration* může deklarovat člena se stejným názvem a signaturou jako zděděného člena. V tomto případě člena odvozené třídy se říká, že ***skrýt*** členu základní třídy. Skrytí zděděného člena není považováno za chybu, ale to způsobit, že kompilátor vydat upozornění. Potlačit upozornění, může obsahovat deklarace člena odvozené třídy `new` modifikátor k označení, že odvozené člen má skrýt základního člena. V tomto tématu je popsán dále v [skrytí prostřednictvím dědičnosti](basic-concepts.md#hiding-through-inheritance).

Pokud `new` Modifikátor je součástí deklarace, která neskryje zděděného člena, příslušná objeví se upozornění. Toto upozornění je potlačeno odebráním `new` modifikátor.

### <a name="access-modifiers"></a>Modifikátory přístupu

A *class_member_declaration* může mít některou z pěti druhy deklarovaná přístupnost ([deklarovaná přístupnost](basic-concepts.md#declared-accessibility)): `public`, `protected internal`, `protected`, `internal` , nebo `private`. S výjimkou `protected internal` kombinaci, je chyba kompilace určit více než jeden modifikátor přístupu. Když *class_member_declaration* nezahrnuje žádné modifikátory přístupu `private` se předpokládá, že.

### <a name="constituent-types"></a>Základní typy

Typy, které se používají v deklaraci člena jsou volány základní typy daného člena. Je to možné základní typy jsou typ konstanty, pole, vlastnosti, události, nebo indexeru, návratový typ metody nebo operátor a typy parametrů metody, indexer, operátor nebo konstruktor instance. Základní typy člena musí být přinejmenším stejně dostupná jako samotný člen ([usnadnění omezení](basic-concepts.md#accessibility-constraints)).

### <a name="static-and-instance-members"></a>Členy statických a instance

Členy třídy jsou buď ***statické členy*** nebo ***členy instance***. Obecně je vhodné popřemýšlet o statické členy jako patřící do typů tříd a členů instance jako patřící do objektů (instance typů tříd).

Při deklaraci pole, metoda, vlastnost, událost, operátor nebo konstruktor obsahuje `static` modifikátor, deklaruje statický člen. Kromě toho deklaraci konstanty nebo typu implicitně deklaruje název statického člena. Statické členy mají následující vlastnosti:

*  Při statického člena, který `M` odkazuje *member_access* ([přístup ke členu](expressions.md#member-access)) ve formátu `E.M`, `E` musí označují typ obsahující `M`. Je chyba kompilace pro `E` k označení instance.
*  Statické pole identifikuje přesně jednoho umístění úložiště do sdíleného všemi instancemi typu dané uzavřené třídy. Bez ohledu na to, kolik instancí typu dané uzavřené třídy jsou vytvořeny je někdy pouze jednu kopii tohoto statické pole.
*  Statická funkce člena (metoda, vlastnost, událost, operátor nebo konstruktor) nefunguje na konkrétní instanci a je chyba kompilace k odkazování na `this` v členské funkce.

Při deklaraci pole, metoda, vlastnost, událost, indexer, konstruktor nebo destruktor nezahrnuje `static` modifikátor, deklaruje člen instance. (Člen instance se někdy nazývá nestatického člena.) Členy instance mají následující vlastnosti:

*  Při členem instance `M` odkazuje *member_access* ([přístup ke členu](expressions.md#member-access)) formuláře `E.M`, `E` musí označit instanci typu, který obsahuje `M`. Jedná se chybu doba vazby pro `E` k označení typu.
*  Každá instance třídy obsahuje samostatnou sadu všechna pole instancí třídy.
*  Funkce člena instance (metoda, vlastnost, indexer, konstruktor instance nebo destruktor) funguje v dané instanci třídy a tato instance je přístupná jako `this` ([tento přístup](expressions.md#this-access)).

Následující příklad znázorňuje pravidla pro přístup k statické a členy instance:
```csharp
class Test
{
    int x;
    static int y;

    void F() {
        x = 1;            // Ok, same as this.x = 1
        y = 1;            // Ok, same as Test.y = 1
    }

    static void G() {
        x = 1;            // Error, cannot access this.x
        y = 1;            // Ok, same as Test.y = 1
    }

    static void Main() {
        Test t = new Test();
        t.x = 1;          // Ok
        t.y = 1;          // Error, cannot access static member through instance
        Test.x = 1;       // Error, cannot access instance member through type
        Test.y = 1;       // Ok
    }
}
```

`F` Metoda ukazuje, že ve funkci členem instance *simple_name* ([jednoduché názvy](expressions.md#simple-names)) lze použít pro přístup k instanci členy a statické členy. `G` Metoda ukazuje, že v statická funkce členu, je chyba kompilace pro přístup k instanci člena prostřednictvím *simple_name*. `Main` Metoda ukazuje, že *member_access* ([přístup ke členu](expressions.md#member-access)) členy instance je možný přes instancí a statické členy je možný prostřednictvím typů.

### <a name="nested-types"></a>Vnořené typy

Typ deklarovaný v rámci deklarace třídy nebo struktury se nazývá ***vnořený typ***. Typ, který je deklarován v rámci kompilační jednotky nebo oboru názvů je volána ***nevnořený typ***.

V příkladu
```csharp
using System;

class A
{
    class B
    {
        static void F() {
            Console.WriteLine("A.B.F");
        }
    }
}
```
Třída `B` je vnořený typ, protože je deklarována v rámci třídy `A`a třídy `A` je nevnořeném typu, protože je deklarována v rámci kompilační jednotky.

#### <a name="fully-qualified-name"></a>Plně kvalifikovaný název

Plně kvalifikovaný název ([plně kvalifikované názvy](basic-concepts.md#fully-qualified-names)) pro vnořený typ je `S.N` kde `S` je plně kvalifikovaný název typu, v jakém typu `N` je deklarována.

#### <a name="declared-accessibility"></a>Deklarovaná přístupnost

Může mít nevnořených typů `public` nebo `internal` deklarován usnadnění a mít `internal` deklarovaná přístupnost ve výchozím nastavení. Vnořené typy mohou mít tyto formy deklarovaná přístupnost, plus nejmíň jeden další formy deklarovaná přístupnost, v závislosti na tom, zda je obsahující typ třídy nebo struktury:

*  Vnořený typ, který je deklarován ve třídě může mít některý z pěti formy deklarovaná přístupnost (`public`, `protected internal`, `protected`, `internal`, nebo `private`) a stejně jako ostatní členové třídy, výchozí hodnota je `private` deklarována usnadnění přístupu.
*  Vnořený typ, který je deklarován v struktury mohou mít tři formy deklarovaná přístupnost (`public`, `internal`, nebo `private`) a stejně jako ostatní členové struktury výchozí `private` deklarovaná přístupnost.

V příkladu
```csharp
public class List
{
    // Private data structure
    private class Node
    { 
        public object Data;
        public Node Next;

        public Node(object data, Node next) {
            this.Data = data;
            this.Next = next;
        }
    }

    private Node first = null;
    private Node last = null;

    // Public interface
    public void AddToFront(object o) {...}
    public void AddToBack(object o) {...}
    public object RemoveFromFront() {...}
    public object RemoveFromBack() {...}
    public int Count { get {...} }
}
```
deklaruje privátní vnořené třídy `Node`.

#### <a name="hiding"></a>Skrytí

Vnořený typ může skrýt ([skrytí názvu](basic-concepts.md#name-hiding)) základního člena. `new` Modifikátor můžou běžet na deklarace vnořených typů tak, aby skrytí lze vyjádřit explicitně. V příkladu
```csharp
using System;

class Base
{
    public static void M() {
        Console.WriteLine("Base.M");
    }
}

class Derived: Base 
{
    new public class M 
    {
        public static void F() {
            Console.WriteLine("Derived.M.F");
        }
    }
}

class Test 
{
    static void Main() {
        Derived.M.F();
    }
}
```
ukazuje, vnořené třídy `M` , která skrývá metodu `M` definované v `Base`.

#### <a name="this-access"></a>Tento přístup

Vnořený typ a jeho nadřazeného typu nemají zvláštní vztah s ohledem na *this_access* ([tento přístup](expressions.md#this-access)). Konkrétně `this` uvnitř vnořeného typu nelze použít k odkazování na členy instance nadřazeného typu. V případech, kdy je vnořený typ přístupu ke členům instance jeho nadřazeného typu, lze zadat přístup tím, že poskytuje `this` pro instanci jako argument konstruktoru pro vnořeného typu nadřazeného typu. Následující příklad
```csharp
using System;

class C
{
    int i = 123;

    public void F() {
        Nested n = new Nested(this);
        n.G();
    }

    public class Nested
    {
        C this_c;

        public Nested(C c) {
            this_c = c;
        }

        public void G() {
            Console.WriteLine(this_c.i);
        }
    }
}

class Test
{
    static void Main() {
        C c = new C();
        c.F();
    }
}
```
Zobrazí tuto techniku. Instance `C` vytvoří instanci `Nested` a předává své vlastní `this` k `Nested`pro konstruktor, aby bylo možné poskytnout další přístup k `C`na členy instance.

#### <a name="access-to-private-and-protected-members-of-the-containing-type"></a>Přístup k soukromým a chráněným členům nadřazeného typu.

Vnořený typ má přístup ke všem členům, kteří jsou k dispozici k jeho nadřazeného typu, včetně členů nadřazeného typu, které mají `private` a `protected` deklarovaná přístupnost. V příkladu
```csharp
using System;

class C 
{
    private static void F() {
        Console.WriteLine("C.F");
    }

    public class Nested 
    {
        public static void G() {
            F();
        }
    }
}

class Test 
{
    static void Main() {
        C.Nested.G();
    }
}
```
ukazuje třídy `C` , která obsahuje vnořené třídy `Nested`. V rámci `Nested`, metoda `G` zavolat statickou metodu `F` definované v `C`, a `F` privátní je deklarovaná přístupnost.

Vnořený typ také mohou přistupovat k chráněným členům definované v základním typu z jeho nadřazeného typu. V příkladu
```csharp
using System;

class Base 
{
    protected void F() {
        Console.WriteLine("Base.F");
    }
}

class Derived: Base 
{
    public class Nested 
    {
        public void G() {
            Derived d = new Derived();
            d.F();        // ok
        }
    }
}

class Test 
{
    static void Main() {
        Derived.Nested n = new Derived.Nested();
        n.G();
    }
}
```
vnořené třídy `Derived.Nested` přistupuje k chráněná metoda `F` definované v `Derived`od základní třídy, `Base`, volání prostřednictvím instance `Derived`.

#### <a name="nested-types-in-generic-classes"></a>Vnořené typy obecných tříd

Deklaraci obecné třídy mohou obsahovat deklarace vnořených typů. Parametry typu nadřazené třídy je možné v rámci vnořené typy. Deklarace vnořeného typu. může obsahovat další typové parametry, které se vztahují pouze k vnořeného typu.

Každý typ deklarace obsažené v deklaraci obecné třídy je implicitně deklarace obecného typu. Při psaní odkaz na typ vnořené do obecného typu, musí mít název obsahující konstruovaný typ, včetně jeho argumentů typu. Však z vnější třídy vnořeného typu můžete bez kvalifikace lze používat; Typ instance z vnější třídy lze implicitně používá při vytváření vnořeného typu. Následující příklad ukazuje tři různé správné způsoby, jak odkazovat na konstruovaný typ vytvořený z `Inner`; první dvě jsou ekvivalentní:
```csharp
class Outer<T>
{
    class Inner<U>
    {
        public static void F(T t, U u) {...}
    }

    static void F(T t) {
        Outer<T>.Inner<string>.F(t, "abc");      // These two statements have
        Inner<string>.F(t, "abc");               // the same effect

        Outer<int>.Inner<string>.F(3, "abc");    // This type is different

        Outer.Inner<string>.F(t, "abc");         // Error, Outer needs type arg
    }
}
```

I když je chybný programování styl, parametr typu v vnořeného typu můžete skrýt člena nebo parametr deklarovaný v typu vnějšího typu:
```csharp
class Outer<T>
{
    class Inner<T>        // Valid, hides Outer's T
    {
        public T t;       // Refers to Inner's T
    }
}
```

### <a name="reserved-member-names"></a>Názvy vyhrazené členů

K usnadnění základního jazyka C# za běhu, pro každou deklaraci člena zdroj, který je vlastnost, událost nebo indexeru, musíte rezervovat implementace dvou podpisy metod v závislosti na charakteru deklarace člena, jeho název a jeho typ. Je chyba kompilace programu k deklarování člena, jehož předpis odpovídá jednomu z těchto vyhrazená podpisy, i v případě, že se základní implementací za běhu neprovede využívání tyto rezervace.

Vyhrazené názvy nezavádí deklarace, proto není účast ve vyhledávání člena. Však deklarace přidružené k vyhrazené metoda podpisy zapojují do dědičnosti ([dědičnosti](classes.md#inheritance)) a může být skrytá s `new` modifikátor ([new – modifikátor](classes.md#the-new-modifier)).

Rezervace tyto názvy slouží tři účelům:

*  Aby se základní implementací běžných identifikátor použít jako název metody pro získání nebo nastavení přístupu k funkci jazyka C#.
*  Pro povolení jazyků pro spolupráci, pomocí běžných identifikátor jako název metody pro získání nebo nastavení přístupu k funkci jazyka C#.
*  Pomáhá zajistit, že zdroj přijal jedna vyhovující kompilátoru přijme, tak, že specifika vyhrazeným členem názvy konzistentní napříč všemi implementacemi jazyka C#.

Deklarace destruktoru ([destruktory](classes.md#destructors)) navíc způsobí, že podpis, které budou rezervovány ([názvy členů, které jsou vyhrazené pro destruktory](classes.md#member-names-reserved-for-destructors)).

#### <a name="member-names-reserved-for-properties"></a>Názvy členů rezervované vlastnosti

Pro vlastnost `P` ([vlastnosti](classes.md#properties)) typu `T`, následující podpisy jsou vyhrazené:

```csharp
T get_P();
void set_P(T value);
```

Obě podpisy jsou vyhrazené, i v případě, že vlastnost je jen pro čtení nebo jen pro zápis.

V příkladu
```csharp
using System;

class A
{
    public int P {
        get { return 123; }
    }
}

class B: A
{
    new public int get_P() {
        return 456;
    }

    new public void set_P(int value) {
    }
}

class Test
{
    static void Main() {
        B b = new B();
        A a = b;
        Console.WriteLine(a.P);
        Console.WriteLine(b.P);
        Console.WriteLine(b.get_P());
    }
}
```
Třída `A` definuje vlastnost jen pro čtení `P`, tedy rezervace podpisy pro `get_P` a `set_P` metody. Třída `B` je odvozena z `A` a skryje obě tyto vyhrazená podpisy. Vzorové postupy výstupu:
```
123
123
456
```

#### <a name="member-names-reserved-for-events"></a>Názvy členů vyhrazené pro události

Pro událost `E` ([události](classes.md#events)) typu delegáta `T`, následující podpisy jsou vyhrazené:
```csharp
void add_E(T handler);
void remove_E(T handler);
```

#### <a name="member-names-reserved-for-indexers"></a>Názvy členů vyhrazené pro indexery

Pro indexer ([indexery](classes.md#indexers)) typu `T` se seznamem parametrů `L`, následující podpisy jsou vyhrazené:
```csharp
T get_Item(L);
void set_Item(L, T value);
```

Obě podpisy jsou vyhrazené, i v případě indexeru je jen pro čtení nebo jen pro zápis.

Kromě názvu členu `Item` je vyhrazená.

#### <a name="member-names-reserved-for-destructors"></a>Názvy členů vyhrazené pro destruktory

Pro třídu obsahující destruktor ([destruktory](classes.md#destructors)), je vyhrazený následující podpis:
```csharp
void Finalize();
```

## <a name="constants"></a>Konstanty

A ***konstantní*** je členem třídy reprezentující konstantní hodnotu: hodnotu, která lze vypočítat v době kompilace. A *constant_declaration* představuje jeden nebo více konstant daného typu.

```antlr
constant_declaration
    : attributes? constant_modifier* 'const' type constant_declarators ';'
    ;

constant_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    ;

constant_declarators
    : constant_declarator (',' constant_declarator)*
    ;

constant_declarator
    : identifier '=' constant_expression
    ;
```

A *constant_declaration* může zahrnovat sadu *atributy* ([atributy](attributes.md)), `new` modifikátor ([new – modifikátor](classes.md#the-new-modifier)), a platnou kombinaci čtyři přístupu modifikátory přístupu ([modifikátorů přístupu](classes.md#access-modifiers)). Atributy a modifikátory platí pro všechny členy deklarované pomocí *constant_declaration*. I když konstanty jsou považovány za statické členy *constant_declaration* ani vyžaduje ani umožňuje `static` modifikátor. Jedná se o chybu pro stejný modifikátor se zobrazí více než jednou v deklarace konstanty.

*Typ* z *constant_declaration* Určuje typ členů zavedeným deklarací. Typ následuje seznam *constant_declarator*s, z nichž každý představuje nového člena. A *constant_declarator* se skládá ze *identifikátor* , který pojmenovává člen, za nímž následuje "`=`" token a po něm *constant_expression* ([ Výrazy konstant](expressions.md#constant-expressions)), který obsahuje hodnotu člena.

*Typ* podle konstanty musí být deklarace `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, `decimal`, `bool`, `string`, *enum_type*, nebo *reference_type*. Každý *constant_expression* musí poskytovat hodnotu cílového typu nebo typu, který lze převést na typ cíle podle implicitní převod ([implicitních převodů](conversions.md#implicit-conversions)).

*Typ* konstanty musí být přinejmenším stejně dostupná jako konstanta samotného ([usnadnění omezení](basic-concepts.md#accessibility-constraints)).

Pomocí výrazu se získá hodnota konstanty *simple_name* ([jednoduché názvy](expressions.md#simple-names)) nebo *member_access* ([přístup ke členu](expressions.md#member-access)).

Samotný konstantu mohl podílet na *constant_expression*. Proto lze v žádné konstrukci, která vyžaduje konstantu *constant_expression*. Příkladem takových konstrukce `case` popisky, `goto case` příkazů `enum` deklarací členů, atributy a jiné deklarace konstanty.

Jak je popsáno v [konstantní výrazy](expressions.md#constant-expressions), *constant_expression* je výraz, který může být plně vyhodnocen v době kompilace. Od jediný způsob, jak vytvořit nenulovou hodnotu *reference_type* jiné než `string` použijte `new` operátorů a od `new` operátor není povolený v *constant_ výraz*, jediná možná hodnota konstanty *reference_type*s jiným než `string` je `null`.

Symbolický název pro konstantní hodnotu, pokud se požaduje, ale když typ této hodnoty není povolen v deklarace konstanty nebo když hodnotu nelze vypočítat v době kompilace pomocí *constant_expression*, `readonly` pole () [Pole jen pro čtení](classes.md#readonly-fields)) mohou být použity místo toho.

Je ekvivalentní více deklarací jedné konstanty se stejnými atributy, modifikátory a typ deklarace konstanty, který deklaruje více konstant. Příklad
```csharp
class A
{
    public const double X = 1.0, Y = 2.0, Z = 3.0;
}
```
je ekvivalentem
```csharp
class A
{
    public const double X = 1.0;
    public const double Y = 2.0;
    public const double Z = 3.0;
}
```

Konstanty jsou povolené závislé na dalších konstant ve stejném programu, za předpokladu, závislosti nejsou cyklické povahy. Kompilátor automaticky uspořádá k vyhodnocení konstantní deklarace ve správném pořadí. V příkladu
```csharp
class A
{
    public const int X = B.Z + 1;
    public const int Y = 10;
}

class B
{
    public const int Z = A.Y + 1;
}
```
Kompilátor nejprve vyhodnotí `A.Y`, pak vyhodnotí `B.Z`a nakonec vyhodnotí `A.X`, vytváření hodnoty `10`, `11`, a `12`. Deklarace konstanty může záviset na konstanty z jiných aplikací, ale tyto závislosti jsou možné pouze v jednom směru. Odkazující na výše uvedeném příkladu, pokud `A` a `B` byly deklarovány v samostatných programech, by bylo možné pro `A.X` závisí na `B.Z`, ale `B.Z` může potom závisí nejsou současně `A.Y`.

## <a name="fields"></a>Pole

A ***pole*** je člen, který představuje proměnnou přidruženou k objektu nebo třídě. A *field_declaration* představuje nejméně jedno pole daného typu.

```antlr
field_declaration
    : attributes? field_modifier* type variable_declarators ';'
    ;

field_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    | 'static'
    | 'readonly'
    | 'volatile'
    | field_modifier_unsafe
    ;

variable_declarators
    : variable_declarator (',' variable_declarator)*
    ;

variable_declarator
    : identifier ('=' variable_initializer)?
    ;

variable_initializer
    : expression
    | array_initializer
    ;
```

A *field_declaration* může zahrnovat sadu *atributy* ([atributy](attributes.md)), `new` modifikátor ([new – modifikátor](classes.md#the-new-modifier)), platnou kombinaci čtyři přístupu modifikátory přístupu ([modifikátorů přístupu](classes.md#access-modifiers)) a `static` modifikátor ([statické a instance pole](classes.md#static-and-instance-fields)). Kromě toho *field_declaration* mohou zahrnovat `readonly` modifikátor ([pole jen pro čtení](classes.md#readonly-fields)) nebo `volatile` modifikátor ([pole s modifikátorem Volatile](classes.md#volatile-fields)), ale ne obojí. Atributy a modifikátory platí pro všechny členy deklarované pomocí *field_declaration*. Jedná se o chybu pro stejný modifikátor objevit více než jednou v deklaraci pole.

*Typ* z *field_declaration* Určuje typ členů zavedeným deklarací. Typ následuje seznam *variable_declarator*s, z nichž každý představuje nového člena. A *variable_declarator* se skládá ze *identifikátor* názvů, který tohoto člena, může volitelně následovat "`=`" token a *variable_initializer* ([ Proměnné inicializátory](classes.md#variable-initializers)), která obsahuje počáteční hodnotu daného člena.

*Typ* pole musí být přinejmenším stejně dostupná jako samotného pole ([usnadnění omezení](basic-concepts.md#accessibility-constraints)).

Hodnota pole se získá ve výrazu pomocí *simple_name* ([jednoduché názvy](expressions.md#simple-names)) nebo *member_access* ([přístup ke členu](expressions.md#member-access)). Hodnota pole není jen pro čtení je upravit pomocí *přiřazení* ([operátory přiřazení](expressions.md#assignment-operators)). Hodnota pole není jen pro čtení může být získat a upravit pomocí zvýšení příponového operátora i operátory snížení ([Příponové operátory Inkrementace a dekrementace operátory](expressions.md#postfix-increment-and-decrement-operators)) a předpony Inkrementace a dekrementace operátory ([předpona operátory přírůstku a snížení](expressions.md#prefix-increment-and-decrement-operators)).

Deklarace pole, která deklaruje několik polí je ekvivalentní více deklarací z jednoho pole se stejnými atributy, modifikátory a typu. Příklad
```csharp
class A
{
    public static int X = 1, Y, Z = 100;
}
```
je ekvivalentem
```csharp
class A
{
    public static int X = 1;
    public static int Y;
    public static int Z = 100;
}
```

### <a name="static-and-instance-fields"></a>Statické a instance pole

Při deklaraci pole zahrnuje `static` modifikátor, polím zavedeným deklarací jsou ***statická pole***. Pokud ne `static` Modifikátor je k dispozici, jsou polím zavedeným deklarací ***instance pole***. Statická pole a pole instancí jsou dvě z několika druhů proměnné ([proměnné](variables.md)) podporují C# a v některých případech se označují jako ***statické proměnné*** a ***instance proměnné*** v uvedeném pořadí.

Statické pole není součástí konkrétní instanci; Místo toho se sdílí mezi všechny instance uzavřených typu ([otevřené a uzavřené typy](types.md#open-and-closed-types)). Bez ohledu na to, kolik instancí typu uzavřené třídy jsou vytvořeny je pouze někdy jednu kopii statické pole pro doménu přidružené aplikace.

Příklad:
```csharp
class C<V>
{
    static int count = 0;

    public C() {
        count++;
    }

    public static int Count {
        get { return count; }
    }
}

class Application
{
    static void Main() {
        C<int> x1 = new C<int>();
        Console.WriteLine(C<int>.Count);        // Prints 1

        C<double> x2 = new C<double>();
        Console.WriteLine(C<int>.Count);        // Prints 1

        C<int> x3 = new C<int>();
        Console.WriteLine(C<int>.Count);        // Prints 2
    }
}
```

Pole instance patří k instanci. Konkrétně každá instance třídy obsahuje samostatnou sadu všechna pole instancí této třídy.

Když odkazuje pole *member_access* ([přístup ke členu](expressions.md#member-access)) formuláře `E.M`, pokud `M` je statická pole, `E` musí označují typ obsahující `M` a pokud `M` je polem instance E musí označit instanci typu, který obsahuje `M`.

Rozdíly mezi statické a členy instance jsou popsány dále v [statické a instance členy](classes.md#static-and-instance-members).

### <a name="readonly-fields"></a>Pole určené jen pro čtení

Když *field_declaration* zahrnuje `readonly` modifikátor, polím zavedeným deklarací se ***pole jen pro čtení***. Přímá přiřazení k polím jen pro čtení může dojít pouze jako součást tohoto prohlášení nebo v konstruktoru instance nebo statický konstruktor ve stejné třídě. (Pole jen pro čtení může být přiřazeno více než jednou v těchto kontextech.) Konkrétně přímého přiřazení `readonly` pole jsou povoleny pouze v následujících kontextů:

*  V *variable_declarator* , který představuje pole (včetně *variable_initializer* v deklaraci).
*  Pro pole instance, v konstruktorech instancí třídy, která obsahuje deklaraci pole; pro statické pole v statický konstruktor třídy, která obsahuje deklaraci pole. Toto jsou také pouze kontextech, ve kterých je možné předat `readonly` pole jako `out` nebo `ref` parametru.

Pokus o přiřazení `readonly` pole nebo předat ji jako `out` nebo `ref` parametr v libovolném kontextu je chyba kompilace.

#### <a name="using-static-readonly-fields-for-constants"></a>Pomocí statického pole pro konstanty

A `static readonly` pole je užitečné, když je žádoucí symbolický název pro konstantní hodnotu, ale když typ hodnoty není povolen v `const` prohlášení, nebo když hodnotu nelze vypočítat v době kompilace. V příkladu
```csharp
public class Color
{
    public static readonly Color Black = new Color(0, 0, 0);
    public static readonly Color White = new Color(255, 255, 255);
    public static readonly Color Red = new Color(255, 0, 0);
    public static readonly Color Green = new Color(0, 255, 0);
    public static readonly Color Blue = new Color(0, 0, 255);

    private byte red, green, blue;

    public Color(byte r, byte g, byte b) {
        red = r;
        green = g;
        blue = b;
    }
}
```
`Black`, `White`, `Red`, `Green`, a `Blue` členy nelze deklarovat jako `const` členy vzhledem k tomu, že jejich hodnoty nelze vypočítat v době kompilace. Je však deklarovat `static readonly` místo toho má mnohem stejný účinek.

#### <a name="versioning-of-constants-and-static-readonly-fields"></a>Správa verzí statického polí a konstanty

Konstanty a pole jen pro čtení mají sémantiku jiný binární správy verzí. Pokud výraz odkazuje na konstantu, je hodnotou konstanty získané v době kompilace, ale pokud výraz odkazuje na pole jen pro čtení, hodnota pole není dosaženo až za běhu. Vezměte v úvahu aplikace, která se skládá ze dvou samostatných programech:
```csharp
using System;

namespace Program1
{
    public class Utils
    {
        public static readonly int X = 1;
    }
}

namespace Program2
{
    class Test
    {
        static void Main() {
            Console.WriteLine(Program1.Utils.X);
        }
    }
}
```

`Program1` a `Program2` obory názvů označení dva programy, které jsou kompilovány samostatně. Protože `Program1.Utils.X` je deklarován jako statického pole hodnotu výstupu podle `Console.WriteLine` příkaz není znám v době kompilace, ale raději získali v době běhu. Proto pokud hodnota `X` se změní a `Program1` přepsán, `Console.WriteLine` příkaz bude výstup i když hodnota `Program2` není znovu zkompilován. Ale měli `X` byla konstantu, hodnota `X` by byly získány v době `Program2` byl zkompilován a zůstane nebudou výpadkem ovlivněny změnami `Program1` dokud `Program2` přepsán.

### <a name="volatile-fields"></a>Pole s modifikátorem volatile

Když *field_declaration* zahrnuje `volatile` modifikátor, polím zavedeným tuto deklaraci jsou ***pole s modifikátorem volatile***.

Pro pole stálé techniky optimalizace, které Změna pořadí pokyny může vést k neočekávané a nepředvídatelné výsledky ve vícevláknových programů, které přístup k polím bez synchronizace, jako je například poskytuje *lock_statement*  ([Příkaz lock](statements.md#the-lock-statement)). Tyto optimalizace lze provést pomocí kompilátoru, za běhu systému nebo hardwarem. Pro pole s modifikátorem volatile jsou omezeny takový způsob optimalizace:

*  Čtení pole s modifikátorem volatile se volá ***volatile čtení***. Volatile pro čtení má "získat sémantika"; To znamená, že je zaručeno, že před všechny odkazy na paměti, ke kterým dochází za něj postupně instrukce.
*  Zápis pole s modifikátorem volatile se volá ***volatile zápisu***. Volatile zápisu je "vydané verze sémantika"; To znamená, že je zaručeno, že tomu může dojít po všechny odkazy na paměť než instrukce zápis v pořadí instrukce.

Tato omezení Ujistěte se, že všechna vlákna budou sledovat volatile zápisy provádí ostatní vlákna v pořadí, ve kterém byly provedeny. Vyhovující implementace není potřeba poskytovat, jeden celkový řazení volatile zápisů pohledu ze všech vláken, která. Typ pole s modifikátorem volatile musí být jeden z následujících akcí:

*  A *reference_type*.
*  Typ `byte`, `sbyte`, `short`, `ushort`, `int`, `uint`, `char`, `float`, `bool`, `System.IntPtr`, nebo` System.UIntPtr`.
*  *Enum_type* základního typu výčtu `byte`, `sbyte`, `short`, `ushort`, `int`, nebo `uint`.

V příkladu
```csharp
using System;
using System.Threading;

class Test
{
    public static int result;   
    public static volatile bool finished;

    static void Thread2() {
        result = 143;    
        finished = true; 
    }

    static void Main() {
        finished = false;

        // Run Thread2() in a new thread
        new Thread(new ThreadStart(Thread2)).Start();

        // Wait for Thread2 to signal that it has a result by setting
        // finished to true.
        for (;;) {
            if (finished) {
                Console.WriteLine("result = {0}", result);
                return;
            }
        }
    }
}
```
Vytvoří výstup:
```
result = 143
```

V tomto příkladu metoda `Main` začíná nové vlákno, na kterém běží metodu `Thread2`. Tato metoda ukládá hodnotu do pole stálé s názvem `result`, pak uloží `true` v pole s modifikátorem volatile `finished`. Hlavní podproces čeká pole `finished` nastavit `true`, pak načte pole `result`. Protože `finished` byla deklarována `volatile`, hlavního vlákna musí načíst hodnotu `143` z pole `result`. Pokud pole `finished` kdyby byly deklarovány `volatile`, pak by přípustné úložiště `result` uvidí hlavního vlákna po úložiště `finished`a proto pro hlavní vlákno načíst hodnotu `0` z pole `result`. Deklarování `finished` jako `volatile` pole zabraňuje Tato nekonzistence.

### <a name="field-initialization"></a>Inicializace pole

Počáteční hodnota pole, ať to statické pole nebo pole instance, je výchozí hodnota ([výchozí hodnoty](variables.md#default-values)) z typu pole. Není možné předtím, než tato výchozí inicializace došlo k chybě, a pole proto nikdy "neinicializované" zachovávají hodnotu pole. V příkladu
```csharp
using System;

class Test
{
    static bool b;
    int i;

    static void Main() {
        Test t = new Test();
        Console.WriteLine("b = {0}, i = {1}", b, t.i);
    }
}
```
Vytvoří výstup
```
b = False, i = 0
```
protože `b` a `i` obě automaticky inicializovány na výchozí hodnoty.

### <a name="variable-initializers"></a>Inicializátory proměnné

Deklarace pole může obsahovat *variable_initializer*s. Statická pole Proměnná inicializátory odpovídají přiřazovací příkazy, které jsou spouštěny během inicializace třídy. Například pole, proměnná inicializátory odpovídají přiřazovací příkazy, které jsou spouštěny, když je vytvořena instance třídy.

V příkladu
```csharp
using System;

class Test
{
    static double x = Math.Sqrt(2.0);
    int i = 100;
    string s = "Hello";

    static void Main() {
        Test a = new Test();
        Console.WriteLine("x = {0}, i = {1}, s = {2}", x, a.i, a.s);
    }
}
```
Vytvoří výstup
```
x = 1.4142135623731, i = 100, s = Hello
```
protože přiřazení k `x` vyvolá se při spuštění statickými Inicializátory pole a přiřazení `i` a `s` dojít, když Inicializátory polí instance spustit.

Výchozí hodnota inicializace je popsáno v [inicializace pole](classes.md#field-initialization) dochází u všech polí, včetně polí, které mají variabilní inicializátory. Proto při inicializaci třídy všechny statické pole v dané třídě jsou nejprve inicializovat na výchozí hodnoty a pak statickými Inicializátory pole jsou provedeny v textové pořadí. Podobně když je vytvořena instance třídy, všechna pole instancí v dané instanci jsou nejprve inicializovat na výchozí hodnoty a pak Inicializátory polí instance jsou provedeny v textové pořadí.

Je možné pro statické pole s proměnnou inicializátory má být dodržen v jejich výchozí hodnoty stavu. Je to ale jak styl důrazně nedoporučuje. V příkladu
```csharp
using System;

class Test
{
    static int a = b + 1;
    static int b = a + 1;

    static void Main() {
        Console.WriteLine("a = {0}, b = {1}", a, b);
    }
}
```
je třeba toto chování. Bez ohledu na cyklické definice a b, program je platný. Výsledkem výstupu
```
a = 1, b = 2
```
protože statická pole `a` a `b` jsou inicializovány na hodnotu `0` (výchozí hodnota pro `int`) předtím, než se spustí jejich inicializátory. Při inicializátor pro `a` spustí hodnotu `b` je nula a proto `a` je inicializován na `1`. Při inicializátor pro `b` spustí, hodnotu `a` již `1`a proto `b` je inicializován na `2`.

#### <a name="static-field-initialization"></a>Inicializace statických polí

Inicializátory proměnné statického pole třídy odpovídají sekvenci úlohy, které jsou spouštěny v textové pořadí, v jakém jsou uvedeny v deklaraci třídy. Pokud statický konstruktor ([statické konstruktory](classes.md#static-constructors)) existuje ve třídě, provádění statickými Inicializátory pole nastane bezprostředně před spuštěním této statický konstruktor. V opačném případě statickými Inicializátory pole jsou spuštěny v závislý na implementaci dobu před prvním použitím statické pole této třídy. V příkladu
```csharp
using System;

class Test 
{ 
    static void Main() {
        Console.WriteLine("{0} {1}", B.Y, A.X);
    }

    public static int F(string s) {
        Console.WriteLine(s);
        return 1;
    }
}

class A
{
    public static int X = Test.F("Init A");
}

class B
{
    public static int Y = Test.F("Init B");
}
```
může generovat buď výstup:
```
Init A
Init B
1 1
```
nebo výstup:
```
Init B
Init A
1 1
```
protože provádění `X`společnosti inicializátor a `Y`společnosti inicializátor mohlo by dojít k buď popořadě; jsou pouze omezené před odkazy na tato pole. Ale v tomto příkladu:
```csharp
using System;

class Test
{
    static void Main() {
        Console.WriteLine("{0} {1}", B.Y, A.X);
    }

    public static int F(string s) {
        Console.WriteLine(s);
        return 1;
    }
}

class A
{
    static A() {}

    public static int X = Test.F("Init A");
}

class B
{
    static B() {}

    public static int Y = Test.F("Init B");
}
```
musí být výstup:
```
Init B
Init A
1 1
```
protože pravidla pro při spuštění statické konstruktory (jak jsou definovány v [statické konstruktory](classes.md#static-constructors)) stanoví, že `B`společnosti statického konstruktoru (a proto `B`společnosti statickými Inicializátory pole) musí spustit před `A`na statický konstruktor a Inicializátory polí.

#### <a name="instance-field-initialization"></a>Inicializace pole instance

Proměnné Inicializátory polí instance třídy, které odpovídají posloupnost přiřazení, která jsou spuštěna ihned po vstupu do některého z konstruktory instancí ([inicializátory konstruktoru](classes.md#constructor-initializers)) této třídy. Proměnné inicializátory jsou provedeny v textové pořadí, v jakém jsou uvedeny v deklaraci třídy. Proces vytváření a Inicializace instance třídy je popsány dále v [Instance konstruktory](classes.md#instance-constructors).

Proměnná inicializátor pro pole instance nemůže odkazovat na právě vytvořenou instanci. Proto je chyba kompilace odkazovat `this` v inicializátoru proměnné, protože je chyba kompilace pro inicializátoru proměnné tak, aby odkazovaly kterýkoli člen instance prostřednictvím *simple_name*. V příkladu
```csharp
class A
{
    int x = 1;
    int y = x + 1;        // Error, reference to instance member of this
}
```
Proměnná inicializátor pro `y` výsledkem chyba kompilace, protože odkazuje na člen právě vytvořenou instanci.

## <a name="methods"></a>Metody

A ***metoda*** je člen, který implementuje výpočtu nebo akce, která může provádět k objektu nebo třídě. Metody jsou deklarovány pomocí *method_declaration*s:

```antlr
method_declaration
    : method_header method_body
    ;

method_header
    : attributes? method_modifier* 'partial'? return_type member_name type_parameter_list?
      '(' formal_parameter_list? ')' type_parameter_constraints_clause*
    ;

method_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    | 'static'
    | 'virtual'
    | 'sealed'
    | 'override'
    | 'abstract'
    | 'extern'
    | 'async'
    | method_modifier_unsafe
    ;

return_type
    : type
    | 'void'
    ;

member_name
    : identifier
    | interface_type '.' identifier
    ;

method_body
    : block
    | '=>' expression ';'
    | ';'
    ;
```

A *method_declaration* může zahrnovat sadu *atributy* ([atributy](attributes.md)) a platnou kombinaci čtyři přístupu modifikátory přístupu ([modifikátorů přístupu ](classes.md#access-modifiers)), `new` ([new – modifikátor](classes.md#the-new-modifier)), `static` ([statická a instanci metody](classes.md#static-and-instance-methods)), `virtual` ([virtuální metody](classes.md#virtual-methods)), `override` ([Přepište metody](classes.md#override-methods)), `sealed` ([zapečetěné metody](classes.md#sealed-methods)), `abstract` ([abstraktní metody](classes.md#abstract-methods)), a `extern` ([Externí metody](classes.md#external-methods)) modifikátory.

Deklarace má platnou kombinaci modifikátory, pokud jsou splněny všechny z následujících akcí:

*  Deklarace obsahuje platnou kombinaci modifikátory přístupu ([modifikátorů přístupu](classes.md#access-modifiers)).
*  Deklarace nezahrnuje stejný modifikátor více než jednou.
*  Deklarace obsahuje jenom jedna z následujících parametrů: `static`, `virtual`, a `override`.
*  Deklarace obsahuje jenom jedna z následujících parametrů: `new` a `override`.
*  Pokud deklarace obsahuje `abstract` modifikátoru deklarace a nesmí obsahovat žádný z následujících parametrů: `static`, `virtual`, `sealed` nebo `extern`.
*  Pokud deklarace obsahuje `private` modifikátoru deklarace a nesmí obsahovat žádný z následujících parametrů: `virtual`, `override`, nebo `abstract`.
*  Pokud deklarace obsahuje `sealed` modifikátoru deklarace a také `override` modifikátor.
*  Pokud deklarace obsahuje `partial` modifikátor, pak nesmí obsahovat žádný z následujících parametrů: `new`, `public`, `protected`, `internal`, `private`, `virtual`, `sealed`, `override` , `abstract`, nebo `extern`.

Metodu, která má `async` Modifikátor je asynchronní funkci a postupuje pravidel popsaných v [asynchronních funkcí](classes.md#async-functions).

*Typ* deklarace metody Určuje typ hodnoty vypočítané a vrácen touto metodou. *Typ* je `void` Pokud metoda nevrací hodnotu. Pokud deklarace obsahuje `partial` modifikátor, pak návratový typ musí být `void`.

*Member_name* Určuje název metody. Pokud metoda není člen implementace explicitního rozhraní ([implementace explicitního rozhraní členských](interfaces.md#explicit-interface-member-implementations)), *member_name* je jednoduše k *identifikátor*. Člen implementace explicitního rozhraní naleznete *member_name* se skládá ze *interface_type* následovaný "`.`" a *identifikátor*.

Volitelný *type_parameter_list* Určuje typ parametry metody ([parametry typu](classes.md#type-parameters)). Pokud *type_parameter_list* určena metoda je ***obecnou metodu***. Pokud tato metoda má `extern` modifikátor, *type_parameter_list* nelze zadat.

Volitelný *formal_parameter_list* Určuje parametry metody ([parametry metody](classes.md#method-parameters)).

Volitelný *type_parameter_constraints_clause*s zadat omezení parametrů jednotlivých typů ([omezení parametru typu](classes.md#type-parameter-constraints)) a může být určen pouze pokud *type_parameter_ seznam* je rovněž dodán, a nemá metodu `override` modifikátor.

*Typ* a každý z typů odkazuje *formal_parameter_list* metody musí být přinejmenším stejně dostupná jako metoda sama ([usnadnění omezení](basic-concepts.md#accessibility-constraints)).

*Method_body* je buď středník ***tělo s příkazy*** nebo ***tělo výrazu***. Tělo s příkazy se skládá z *bloku*, která určuje příkazy ke spuštění při vyvolání metody. Tělo výrazu se skládá z `=>` následovaný *výraz* a středník a označuje jeden výraz k provádění při vyvolání metody. 

Pro `abstract` a `extern` metody, *method_body* se skládá pouze ze středníku. Pro `partial` metody *method_body* může skládat ze středníku, blok textu nebo tělo výrazu. Pro všechny ostatní metody *method_body* je blok textu nebo tělo výrazu.

Pokud *method_body* se skládá ze středníku, pak nemusí obsahovat deklaraci `async` modifikátor.

Název, seznam parametrů typu a seznam formálních parametrů metody definování signatury ([podpisy a přetížení](basic-concepts.md#signatures-and-overloading)) metody. Konkrétně podpis metody se skládá z názvu, počet parametrů typu a číslo, modifikátory a typy formálních parametrů. Pro tyto účely je identifikován libovolný typ parametru metody, nacházející se v typu formálního parametru není podle názvu, ale podle jeho pořadové číslo pozice v seznamu argumentů typu metody. Návratový typ není součástí signatury metody, ani se o názvy formální parametry ani parametry typu.

Název metody se musí lišit od názvů ostatních bez metody s deklarací ve stejné třídě. Kromě toho podpis metody se musí lišit od podpisů všechny ostatní metody s deklarací ve stejné třídě, a dvě metody s deklarací ve stejné třídě, nemusí mít podpisy, které se liší pouze `ref` a `out`.

Metody *type_parameter*s jsou v oboru v celém *method_declaration*a je možné typy formulářů v daném oboru v celém *typ*, *method_body*, a *type_parameter_constraints_clause*s, ale ne v *atributy*.

Všechny formálních parametrů a parametrů typu musí mít jedinečné názvy.

### <a name="method-parameters"></a>Parametry metody

Parametry metody, pokud existují, jsou deklarovány pomocí metody *formal_parameter_list*.

```antlr
formal_parameter_list
    : fixed_parameters
    | fixed_parameters ',' parameter_array
    | parameter_array
    ;

fixed_parameters
    : fixed_parameter (',' fixed_parameter)*
    ;

fixed_parameter
    : attributes? parameter_modifier? type identifier default_argument?
    ;

default_argument
    : '=' expression
    ;

parameter_modifier
    : 'ref'
    | 'out'
    | 'this'
    ;

parameter_array
    : attributes? 'params' array_type identifier
    ;
```

Seznam formálních parametrů se skládá z jednoho nebo více oddělených čárkou parametrů z nichž může být pouze poslední *parameter_array*.

A *fixed_parameter* se skládá z volitelné sadu *atributy* ([atributy](attributes.md)), volitelně `ref`, `out` nebo `this` modifikátor, *typ*, *identifikátor* a volitelně *default_argument*. Každý *fixed_parameter* deklaruje parametr daného typu s daným názvem. `this` Modifikátor označí metody jako metody rozšíření a je povolený jenom u první parametr statické metody. Rozšiřující metody jsou popsané v [rozšiřující metody](classes.md#extension-methods).

A *fixed_parameter* s *default_argument* se označuje jako ***volitelný parametr***, že *fixed_parameter* bez *default_argument* je ***požadovaný parametr***. Povinný parametr se nemusí zobrazit po volitelný parametr *formal_parameter_list*.

A `ref` nebo `out` parametr nemůže mít *default_argument*. *Výraz* v *default_argument* musí být jedna z následujících akcí:

*  a *constant_expression*
*  výraz ve tvaru `new S()` kde `S` je typ hodnoty
*  výraz ve tvaru `default(S)` kde `S` je typ hodnoty

*Výraz* musí být implicitně převeditelný identity nebo s povolenou hodnotou Null převod na typ parametru.

Pokud dojde k volitelné parametry v implementující deklaraci částečné metody ([částečné metody](classes.md#partial-methods)), člen implementace explicitního rozhraní ([implementace explicitního rozhraní členských](interfaces.md#explicit-interface-member-implementations)) nebo indexer jedním parametrem deklarace ([indexery](classes.md#indexers)) kompilátor měl dát upozornění, protože tyto členy můžete nelze nikdy vyvolat způsobem, který umožňuje argumenty, které mají být vynechán.

A *parameter_array* se skládá z volitelné sadu *atributy* ([atributy](attributes.md)), `params` modifikátor, *array_type*, a *identifikátor*. Pole parametrů deklaruje jeden parametr typu daného pole s daným názvem. *Array_type* parametru pole musí být typu jednorozměrná pole ([pole typů](arrays.md#array-types)). Ve volání metody pole parametrů povoluje buď jeden argument typu daného pole zadat nebo it specialistovi nuly nebo více argumentů typ elementu pole zadat. Pole parametrů jsou popsány dále v [pole parametrů](classes.md#parameter-arrays).

A *parameter_array* může vzniknout po volitelný parametr, ale nemůže mít výchozí hodnotu--vynechání argumenty *parameter_array* místo toho způsobí vytvoření prázdné pole.

Následující příklad ukazuje různé druhy parametry:
```csharp
public void M(
    ref int      i,
    decimal      d,
    bool         b = false,
    bool?        n = false,
    string       s = "Hello",
    object       o = null,
    T            t = default(T),
    params int[] a
) { }
```

V *formal_parameter_list* pro `M`, `i` je parametr povinný ref `d` je povinná hodnota parametr `b`, `s`, `o` a `t` jsou nepovinné hodnoty parametrů a `a` je pole parametrů.

Deklarace metody vytvoří samostatné prohlášení prostor pro parametry, parametry typu a místní proměnné. Názvy jsou zavedeny do tohoto prostoru deklarace seznamu parametrů typu a seznamu formálních parametrů metody a místní deklarace proměnné v *bloku* metody. Jedná se o chybu pro dva členy místa deklarace metoda má stejný název. Jedná se o chybu místa deklarace metody a deklarace lokální proměnné místo prostoru vnořené deklarace tak, aby obsahovala elementy se stejným názvem.

Volání metody ([volání metod](expressions.md#method-invocations)) vytvoří kopii specifické pro tuto vyvolání, formálních parametrů a lokálních proměnných metody a seznamu argumentů volání přiřadí hodnoty nebo proměnné odkazů na nově vytvořené formální parametry. V rámci *bloku* metody, formální parametry lze odkazovat pomocí jejich identifikátorů v *simple_name* výrazy ([jednoduché názvy](expressions.md#simple-names)).

Existují čtyři typy formálních parametrů:

*  Hodnoty parametrů, které jsou deklarovány bez jakékoli modifikátory.
*  Odkazovat na parametry, které jsou deklarovány pomocí `ref` modifikátor.
*  Výstupní parametry, které jsou deklarovány pomocí `out` modifikátor.
*  Pole parametrů, které jsou deklarovány pomocí `params` modifikátor.

Jak je popsáno v [podpisy a přetížení](basic-concepts.md#signatures-and-overloading), `ref` a `out` modifikátory jsou součástí signatury metody, ale `params` modifikátor není.

#### <a name="value-parameters"></a>Parametry s hodnotou

Parametr deklarovány žádné modifikátory přístupu se parametr hodnoty. Hodnota parametru odpovídá místní proměnná, která získá počáteční hodnoty z na odpovídající argument zadaný ve volání metody.

Po formální parametr se parametr hodnoty odpovídající argument ve volání metody musí být výraz, který je implicitně převést ([implicitních převodů](conversions.md#implicit-conversions)) na typ formálního parametru.

Metoda je povolený pro přiřazení nové hodnoty pro parametr hodnoty. Taková přiřazení ovlivní pouze místní úložiště reprezentovaný hodnotou parametru – nemají žádný účinek na skutečný argument předaný ve volání metody.

#### <a name="reference-parameters"></a>Parametry odkazu

Parametr deklarovaný s `ref` Modifikátor je odkaz na parametr. Na rozdíl od parametru hodnoty parametr odkazu vytváření nového umístění úložiště. Místo toho referenční parametr představuje stejné úložiště jako proměnná, zadaný jako argument ve volání metody.

Parametr odkazu po formálním parametrem odpovídající argument ve volání metody se musí skládat z klíčového slova `ref` následovaný *variable_reference* ([přesné pravidla pro určování jednoznačného přiřazení](variables.md#precise-rules-for-determining-definite-assignment)) stejného typu jako formálních parametrů. Proměnná musí být jednoznačně přiřazena dříve, než může být předán jako parametr odkazu.

V rámci metody referenční parametr je vždy považován za jednoznačně přiřazené.

Metody deklarované jako iterátor ([iterátory](classes.md#iterators)) nemůže mít parametry odkazů.

V příkladu
```csharp
using System;

class Test
{
    static void Swap(ref int x, ref int y) {
        int temp = x;
        x = y;
        y = temp;
    }

    static void Main() {
        int i = 1, j = 2;
        Swap(ref i, ref j);
        Console.WriteLine("i = {0}, j = {1}", i, j);
    }
}
```
Vytvoří výstup
```
i = 2, j = 1
```

Pro vyvolání `Swap` v `Main`, `x` představuje `i` a `y` představuje `j`. Proto volání má vliv na prohození hodnoty `i` a `j`.

V metodě, která přebírá parametry odkazů je možné pro více názvy, které představují stejné umístění úložiště. V příkladu
```csharp
class A
{
    string s;

    void F(ref string a, ref string b) {
        s = "One";
        a = "Two";
        b = "Three";
    }

    void G() {
        F(ref s, ref s);
    }
}
```
vyvolání `F` v `G` předává odkazem na `s` pro obě `a` a `b`. Proto pro vyvolání, názvy `s`, `a`, a `b` všechny odkazovat do stejného umístění úložiště, a upravte všechny tři přiřazení pole instance `s`.

#### <a name="output-parameters"></a>Výstupní parametry

Parametr deklarovaný pomocí `out` Modifikátor je výstupní parametr. Podobně jako parametr odkazu, výstupní parametr nevytváří nového umístění úložiště. Místo toho výstupní parametr představuje stejné úložiště jako proměnná, zadaný jako argument ve volání metody.

Při formální parametr je výstupní parametr, odpovídající argument ve volání metody musí obsahovat klíčové slovo `out` následovaný *variable_reference* ([přesné pravidla pro určování jednoznačného přiřazení](variables.md#precise-rules-for-determining-definite-assignment)) stejného typu jako formálních parametrů. Proměnná není jednoznačně přiřazena dříve, než může být předán jako výstupní parametr, ale po vyvolání, kde byla předána proměnná jako výstupní parametr, proměnná je považován za jednoznačně přiřazené.

V rámci metody, stejně jako místní proměnná, výstupní parametr je zpočátku považován za nepřiřazené a musí být jednoznačně přiřazena dříve, než se jeho hodnota se používá.

Každou výstupní parametr metody musí být jednoznačně přiřazena dříve, než metoda vrátí.

Metoda deklarována jako částečná metoda ([částečné metody](classes.md#partial-methods)) nebo iterátor ([iterátory](classes.md#iterators)) nemůže mít výstupní parametry.

Výstupní parametry jsou obvykle používány v metodách, které vytvářejí více návratových hodnot. Příklad:
```csharp
using System;

class Test
{
    static void SplitPath(string path, out string dir, out string name) {
        int i = path.Length;
        while (i > 0) {
            char ch = path[i - 1];
            if (ch == '\\' || ch == '/' || ch == ':') break;
            i--;
        }
        dir = path.Substring(0, i);
        name = path.Substring(i);
    }

    static void Main() {
        string dir, name;
        SplitPath("c:\\Windows\\System\\hello.txt", out dir, out name);
        Console.WriteLine(dir);
        Console.WriteLine(name);
    }
}
```

Vzorové postupy výstupu:
```
c:\Windows\System\
hello.txt
```

Všimněte si, že `dir` a `name` proměnné může nepřiřazené, než jsou předány `SplitPath`, a že jsou považovány za jednoznačně přiřazené následující po volání.

#### <a name="parameter-arrays"></a>Pole parametrů

Parametr deklarovaný s `params` se modifikátor pole parametrů. Pokud seznam formálních parametrů obsahuje pole parametrů, musí být posledním parametrem v seznamu a musí být jednorozměrné pole typu. Například typy `string[]` a `string[][]` lze použít jako typ pole parametrů, ale typ `string[,]` nemůže. Není možné kombinovat `params` modifikátor modifikátory `ref` a `out`.

Pole parametrů povoluje argumenty, které mají být zadány v některém ze dvou způsobů ve volání metody:

*  Argument zadaný pro parametr pole může být jediný výraz, který je implicitně převést ([implicitních převodů](conversions.md#implicit-conversions)) na typ pole parametrů. V tomto případě pole parametrů funguje přesně jako hodnotu parametru.
*  Alternativně můžete vyvolání zadat nuly nebo více argumentů pro pole parametrů, kde každý argument je výraz, který je implicitně převést ([implicitních převodů](conversions.md#implicit-conversions)) na typ elementu pole parametrů. V takovém případě vyvolání vytvoří instanci typu parametru pole s délkou odpovídající počet argumentů, inicializuje prvky instance pole s hodnotami daný argument a používá instanci nově vytvořené pole jako skutečný argument.

Kromě povolení proměnný počet argumentů ve vyvolání, je přesně odpovídá parametru hodnoty pole parametrů ([hodnoty parametrů](classes.md#value-parameters)) stejného typu.

V příkladu
```csharp
using System;

class Test
{
    static void F(params int[] args) {
        Console.Write("Array contains {0} elements:", args.Length);
        foreach (int i in args) 
            Console.Write(" {0}", i);
        Console.WriteLine();
    }

    static void Main() {
        int[] arr = {1, 2, 3};
        F(arr);
        F(10, 20, 30, 40);
        F();
    }
}
```
Vytvoří výstup
```
Array contains 3 elements: 1 2 3
Array contains 4 elements: 10 20 30 40
Array contains 0 elements:
```

Před prvním vyvoláním služby `F` jednoduše předává pole `a` jako hodnotu parametru. Druhé volání `F` automaticky vytvoří čtyřech prvcích `int[]` s hodnotami daného elementu a předá, které pole instance jako hodnotu parametru. Podobně, třetí vyvolání `F` vytvoří element nula `int[]` a předá jako parametr hodnoty této instance. Druhý a třetí volání přesně odpovídají na zápis:
```csharp
F(new int[] {10, 20, 30, 40});
F(new int[] {});
```

Při překladu přetížení metody s polem parametrů lze použít v běžné formě nebo v podobě rozšířené ([použít funkční člen](expressions.md#applicable-function-member)). Rozbalené metody je k dispozici pouze v případě, že formuláři normální metody se nedá použít, a pouze v případě použitelnou metodu se stejnou signaturou, jako rozšířené formulář není již deklarovány v rámci stejného typu.

V příkladu
```csharp
using System;

class Test
{
    static void F(params object[] a) {
        Console.WriteLine("F(object[])");
    }

    static void F() {
        Console.WriteLine("F()");
    }

    static void F(object a0, object a1) {
        Console.WriteLine("F(object,object)");
    }

    static void Main() {
        F();
        F(1);
        F(1, 2);
        F(1, 2, 3);
        F(1, 2, 3, 4);
    }
}
```
Vytvoří výstup
```
F();
F(object[]);
F(object,object);
F(object[]);
F(object[]);
```

V tomto příkladu dvě formy možné rozšířené metody s polem parametrů jsou již zahrnuty v třídě jako běžné metody. Tyto rozšířené formuláře proto ohled při provádění rozlišení přetížení a volání metod první a třetí proto vyberte běžné metody. Pokud třída deklaruje metodu s polem parametrů, není nic neobvyklého, také obsahovat některé rozšířené formuláře jako běžné metody. Tímto způsobem, takže je možné, aby se zabránilo přidělení pole instance, která nastane, pokud rozbalené metody s polem parametrů vyvolání.

Pokud je typ pole parametrů `object[]`, potenciální nejednoznačností dochází mezi formuláři normální metody a expended formuláře pro jeden `object` parametru. Důvod nejednoznačnost je, že `object[]` je sama o sobě implicitně převést na typ `object`. Nejednoznačnosti uvede není problém, ale vzhledem k tomu, že se dá vyřešit tak, že v případě potřeby vloží přetypování.

V příkladu
```csharp
using System;

class Test
{
    static void F(params object[] args) {
        foreach (object o in args) {
            Console.Write(o.GetType().FullName);
            Console.Write(" ");
        }
        Console.WriteLine();
    }

    static void Main() {
        object[] a = {1, "Hello", 123.456};
        object o = a;
        F(a);
        F((object)a);
        F(o);
        F((object[])o);
    }
}
```
Vytvoří výstup
```
System.Int32 System.String System.Double
System.Object[]
System.Object[]
System.Int32 System.String System.Double
```

V prvním a posledním volání `F`, běžné formě `F` platí, protože existuje implicitní převod z typu argumentu na typ parametru (obojí jsou typu `object[]`). Díky tomu se řešení přetížení vybere běžné formě `F`, a argument je předán jako parametr regulární hodnoty. V druhé a třetí volání, běžné formě `F` se nedá použít, protože neexistuje žádný implicitní převod z typu argumentu na typ parametru (typ `object` nejde implicitně převést na typ `object[]`). Ale rozšířené formu `F` se vztahuje, takže se zvolila řešení přetížení. V důsledku toho prvek jedním `object[]` vytvoří při vyvolání, a jeden prvek pole je inicializován s hodnotou daný argument (které sám o sobě představuje odkaz na `object[]`).

### <a name="static-and-instance-methods"></a>Statické a instance metody

Pokud deklarace metody obsahuje `static` modifikátor, že metoda je označen jako statickou metodu. Pokud ne `static` Modifikátor je k dispozici, metodu je označen jako metoda instance.

Statická metoda nefunguje na konkrétní instanci a je chyba kompilace k odkazování na `this` uvnitř statické metody.

Metoda instance pracuje instanci dané třídy, a tato instance je přístupná jako `this` ([tento přístup](expressions.md#this-access)).

Když metoda odkazuje *member_access* ([přístup ke členu](expressions.md#member-access)) formuláře `E.M`, pokud `M` je statická metoda, `E` musí označují typ obsahující `M`a pokud `M` je metoda instance, `E` musí označit instanci typu, který obsahuje `M`.

Rozdíly mezi statické a členy instance jsou popsány dále v [statické a instance členy](classes.md#static-and-instance-members).

### <a name="virtual-methods"></a>Virtuální metody

Pokud obsahuje deklaraci metody instance `virtual` modifikátor, že metoda je označen jako virtuální metody. Pokud ne `virtual` Modifikátor je k dispozici, metodu se říká, že jako nevirtuální metodu.

Implementace nevirtuální metody je invariantní: Implementace je stejný, ať vyvolání metody instance třídy, ve kterém je deklarována nebo instanci odvozené třídy. Naproti tomu implementace virtuální metoda může být potlačeno z odvozených tříd. Nahrazování provádění zděděnou virtuální metodu proces se označuje jako ***přepsání*** metody ([přepište metody](classes.md#override-methods)).

Ve volání virtuální metody ***run-time typu*** instance, pro který přebírá tento vyvolání místo určuje implementaci skutečné metoda k vyvolání. Ve volání nevirtuální metody ***typu v době kompilace*** instance je určujícím faktorem. Přesně řečeno, pokud metodu s názvem `N` vyvolání se seznamem argumentů `A` instance s typem kompilace `C` a run-time typu `R` (kde `R` je buď `C` nebo třída odvozená z `C`), volání se zpracovává následujícím způsobem:

*  Nejprve řešení přetížení se použije pro `C`, `N`, a `A`, vyberte konkrétní metody `M` ze sady metody deklarované v a dědí `C`. To je popsáno v [volání metod](expressions.md#method-invocations).
*  Když se poté `M` nevirtuální metoda `M` je vyvolána.
*  V opačném případě `M` virtuální metody a implementace Většina odvozených z `M` s ohledem na `R` je vyvolána.

Pro každý virtuální metody deklarované v nebo zděděná z třídy existuje ***nejvíce odvozenému implementace*** metody s ohledem na tuto třídu. Implementace Většina odvozených virtuální metody `M` s ohledem na třídu `R` je stanoven následujícím způsobem:

*  Pokud `R` obsahuje umístění `virtual` deklarace `M`, pak toto je implementace Většina odvozených z `M`.
*  Jinak, pokud `R` obsahuje `override` z `M`, pak toto je implementace Většina odvozených z `M`.
*  V opačném případě nejvíce odvozené provádění `M` s ohledem na `R` je stejná jako implementace Většina odvozených z `M` s ohledem na přímé základní třídy `R`.

Následující příklad ukazuje rozdíly mezi virtuální a nevirtuální metody:
```csharp
using System;

class A
{
    public void F() { Console.WriteLine("A.F"); }

    public virtual void G() { Console.WriteLine("A.G"); }
}

class B: A
{
    new public void F() { Console.WriteLine("B.F"); }

    public override void G() { Console.WriteLine("B.G"); }
}

class Test
{
    static void Main() {
        B b = new B();
        A a = b;
        a.F();
        b.F();
        a.G();
        b.G();
    }
}
```

V tomto příkladu `A` zavádí nevirtuální metoda `F` a virtuální metody `G`. Třída `B` představuje nový způsob nevirtuální `F`, proto děděné skrytí `F`a také přepsání zděděné metody `G`. Vzorové postupy výstupu:
```
A.F
B.F
B.G
B.G
```

Všimněte si, že příkaz `a.G()` vyvolá `B.G`, nikoli `A.G`. Důvodem je, že run-time typu instance (což je `B`), ne za kompilace typ instance (což je `A`), určuje implementaci skutečné metoda k vyvolání.

Protože jsou povoleny metody ke skrytí zděděné metody, je možné pro třídu obsahuje několik virtuálních metod se stejným podpisem. Tím nedojde k žádnému kvůli problému nejednoznačnost od všech polí kromě nejvíce Odvozená metoda jsou skryté. V příkladu
```csharp
using System;

class A
{
    public virtual void F() { Console.WriteLine("A.F"); }
}

class B: A
{
    public override void F() { Console.WriteLine("B.F"); }
}

class C: B
{
    new public virtual void F() { Console.WriteLine("C.F"); }
}

class D: C
{
    public override void F() { Console.WriteLine("D.F"); }
}

class Test
{
    static void Main() {
        D d = new D();
        A a = d;
        B b = d;
        C c = d;
        a.F();
        b.F();
        c.F();
        d.F();
    }
}
```
`C` a `D` třídy obsahovat dvě virtuální metody se stejnou signaturou: Je zavedený `A` a ten zavedené `C`. Metody zavedené `C` skryje metody zděděné z `A`. Proto přepsání deklarace v `D` přepisuje deklaraci metody zavedené `C`, a není možné pro `D` přepsat metodu zavedené `A`. Vzorové postupy výstupu:
```
B.F
B.F
D.F
D.F
```

Všimněte si, že je možné vyvolat skryté virtuální metody, přístup k instanci `D` prostřednictvím méně odvozený typ, ve kterém není skrytý metodu.

### <a name="override-methods"></a>Přepište metody

Při deklaraci instance metody obsahuje `override` modifikátor, metoda se říká, že ***přepsat metodu***. To metoda override přepsání zděděné virtuální metody se stejným podpisem. Vzhledem k tomu virtuální metoda deklarace zavádí nové metody, specializuje deklaraci metody přepsání existující zděděnou virtuální metodu tím, že poskytuje novou implementaci této metody.

Metoda přepsat `override` prohlášení je označován jako ***přepsat základní metodu***. Pro to metoda override `M` deklarovat v třídě `C`, přepsané základní metoda je určen tím, že kontroluje každý typ základní třídy `C`začíná s přímou základní třídu typu `C` a pokračuje s každou po sobě jdoucích Typ přímé základní třídy, do dané základní třídy typu alespoň jedna je přístupná metoda nachází, který má stejnou signaturu jako `M` po nahrazení argumentů typu. Pro účely vyhledávání přepsané základní metody, metoda se považuje za dostupné v případě, že je `public`, pokud je `protected`, pokud je `protected internal`, nebo je-li `internal` a deklarovat ve stejném programu jako `C`.

Chyba kompilace nastane, pokud jsou splněny pro deklaraci přepsat všechny z následujících akcí:

*  Přepsané základní metoda může být umístěn, jak je popsáno výše.
*  Existuje přesně jeden takový přepsané základní metodě. Toto omezení nemá vliv, pouze v případě, že typ základní třídy je konstruovaný typ. Pokud nahrazení argumentů typu díky podpis dvě metody stejné.
*  Přepsané základní metoda je virtuální, abstraktní nebo přepište metodu. Jinými slovy přepsané základní metoda nemůže být statická nebo nevirtuální.
*  Přepsané základní metoda není zapečetěné metody.
*  Metoda přepsání a přepsané základní metody mají vracet hodnotu stejného typu.
*  Přepsání deklarace a přepsané základní metody mají stejné deklarovaná přístupnost. Jinými slovy přepsání deklarace nemůže změnit přístupnost virtuální metody. Ale pokud přepsané základní metoda je interní chráněné a je deklarována v jiném sestavení než sestavení obsahující metodu přepsání metody pak přepsání metody deklarované usnadnění musí být chráněné.
*  Přepsání deklarace není určen typ parametru omezení klauzule. Místo toho omezení dědí z přepsané základní metodě. Všimněte si, že omezení, která jsou parametry typu v metodě přepsané mohou být nahrazeny argumenty typu v zděděné omezení. To může vést k omezení, která nejsou povoleny, pokud explicitně zadán, jako jsou typy hodnot nebo zapečetěné typy.

Následující příklad ukazuje, jak fungují přepsání pravidel pro obecné třídy:
```csharp
abstract class C<T>
{
    public virtual T F() {...}
    public virtual C<T> G() {...}
    public virtual void H(C<T> x) {...}
}

class D: C<string>
{
    public override string F() {...}            // Ok
    public override C<string> G() {...}         // Ok
    public override void H(C<T> x) {...}        // Error, should be C<string>
}

class E<T,U>: C<U>
{
    public override U F() {...}                 // Ok
    public override C<U> G() {...}              // Ok
    public override void H(C<T> x) {...}        // Error, should be C<U>
}
```

Deklarace přepsání můžete přistupovat, pomocí přepsané základní metody *base_access* ([základní přístup](expressions.md#base-access)). V příkladu
```csharp
class A
{
    int x;

    public virtual void PrintFields() {
        Console.WriteLine("x = {0}", x);
    }
}

class B: A
{
    int y;

    public override void PrintFields() {
        base.PrintFields();
        Console.WriteLine("y = {0}", y);
    }
}
```
`base.PrintFields()` volání `B` vyvolá `PrintFields` metody deklarované v `A`. A *base_access* zakáže mechanismus volání virtuální a základní metoda jednoduše považuje za nevirtuální metody. Došlo při vyvolání v `B` byla zapsána `((A)this).PrintFields()`, by rekurzivně vyvolat `PrintFields` metody deklarované v `B`, není ten deklarované v `A`, protože `PrintFields` je virtuální a za běhu typu `((A)this)` je `B`.

Pouze zahrnutím `override` může Modifikátor metody přepsání jinou metodu. Ve všech ostatních případech metodu se stejnou signaturou jako zděděnou metodu jednoduše skryje zděděné metody. V příkladu
```csharp
class A
{
    public virtual void F() {}
}

class B: A
{
    public virtual void F() {}        // Warning, hiding inherited F()
}
```
`F` metoda ve `B` nezahrnuje `override` modifikátor, takže nedojde k přepsání `F` metoda ve `A`. Místo toho `F` metoda `B` skrývá metodu v `A`, a upozornění je hlášeno, protože neobsahuje deklaraci `new` modifikátor.

V příkladu
```csharp
class A
{
    public virtual void F() {}
}

class B: A
{
    new private void F() {}        // Hides A.F within body of B
}

class C: B
{
    public override void F() {}    // Ok, overrides A.F
}
```
`F` metoda `B` skryje virtuální `F` metody zděděné z `A`. Od nové `F` v `B` má privátní přístup, její obor pouze obsahuje tělo třídy `B` a nerozšiřuje `C`. Proto deklarace `F` v `C` smí přepsat `F` zděděno z `A`.

### <a name="sealed-methods"></a>Zapečetěné metody

Při deklaraci instance metody obsahuje `sealed` modifikátor, že metoda je říká, že ***zapečetěné metody***. Pokud obsahuje deklaraci metody instance `sealed` modifikátor, musí také zahrnovat `override` modifikátor. Použití `sealed` modifikátor zabrání další přetížení metody odvozené třídy.

V příkladu
```csharp
using System;

class A
{
    public virtual void F() {
        Console.WriteLine("A.F");
    }

    public virtual void G() {
        Console.WriteLine("A.G");
    }
}

class B: A
{
    sealed override public void F() {
        Console.WriteLine("B.F");
    } 

    override public void G() {
        Console.WriteLine("B.G");
    } 
}

class C: B
{
    override public void G() {
        Console.WriteLine("C.G");
    } 
}
```
třídy `B` poskytuje dvě přepište metody: `F` metodu, která má `sealed` modifikátor a `G` metoda, která neexistuje. `B`využití zapečetěná `modifier` brání `C` další možnost obejít `F`.

### <a name="abstract-methods"></a>Abstraktní metody

Při deklaraci instance metody obsahuje `abstract` modifikátor, že metoda je říká, že ***abstraktní metoda***. I když abstraktní metoda je implicitně také virtuální metody, nemůže mít modifikátor `virtual`.

Deklarace abstraktní metody zavádí novou virtuální metodou, ale neposkytuje implementaci této metody. Odvozené neabstraktní třídy místo toho je potřeba poskytli vlastní implementaci přepsáním této metody. Protože abstraktní metody poskytuje skutečné implementaci, *method_body* abstraktní metody jednoduše se skládá ze středníku.

Deklarace abstraktní metody jsou povolené jenom v abstraktní třídy ([abstraktní třídy](classes.md#abstract-classes)).

V příkladu
```csharp
public abstract class Shape
{
    public abstract void Paint(Graphics g, Rectangle r);
}

public class Ellipse: Shape
{
    public override void Paint(Graphics g, Rectangle r) {
        g.DrawEllipse(r);
    }
}

public class Box: Shape
{
    public override void Paint(Graphics g, Rectangle r) {
        g.DrawRect(r);
    }
}
```
`Shape` třída používá koncept abstraktní geometrický tvar objektu, který vlastní vykreslení. `Paint` Metoda je abstraktní, protože neexistuje žádné smysluplné výchozí implementaci. `Ellipse` a `Box` třídy jsou konkrétní `Shape` implementace. Protože tyto třídy jsou neabstraktní, jsou nutné k přepsání `Paint` metoda a poskytnout skutečné implementaci.

Je chyba kompilace pro *base_access* ([základní přístup](expressions.md#base-access)) tak, aby odkazovaly abstraktní metody. V příkladu
```csharp
abstract class A
{
    public abstract void F();
}

class B: A
{
    public override void F() {
        base.F();                        // Error, base.F is abstract
    }
}
```
oznámí se chyba kompilace pro `base.F()` vyvolání protože odkazuje na abstraktní metody.

Pokud chcete přepsat virtuální metodu smí obsahovat deklarace abstraktní metody. Tato možnost povoluje abstraktní třídu pro vynucení opětovná implementace metody v odvozených třídách a znepřístupní původní implementaci metody. V příkladu
```csharp
using System;

class A
{
    public virtual void F() {
        Console.WriteLine("A.F");
    }
}

abstract class B: A
{
    public abstract override void F();
}

class C: B
{
    public override void F() {
        Console.WriteLine("C.F");
    }
}
```
Třída `A` deklaruje virtuální metody, třídy `B` přepíše tuto metodu pomocí abstraktní metody a třídy `C` přepíše abstraktní metodu a zadejte vlastní implementaci.

### <a name="external-methods"></a>Externí metody

Pokud deklarace metody obsahuje `extern` modifikátor, že metoda je říká, že ***externí metoda***. Externí metody jsou implementovány externě, obvykle pomocí jiného jazyka než C#. Protože deklaraci externí metody poskytuje skutečné implementaci, *method_body* externí metody jednoduše se skládá ze středníku. Externí metoda nemusí být obecný.

`extern` Modifikátor se obvykle používá ve spojení s `DllImport` atribut ([vzájemná spolupráce s komponentami COM a Win32](attributes.md#interoperation-with-com-and-win32-components)), což externí metody k implementaci knihovny DLL (Dynamic Link Libraries). Spouštěcí prostředí můžou podporovat jiné mechanismy, které je možné poskytnout implementace metod externí.

Pokud obsahuje externí metody `DllImport` atribut deklarace metody musí také zahrnovat `static` modifikátor. Tento příklad ukazuje použití `extern` modifikátor a `DllImport` atribut:
```csharp
using System.Text;
using System.Security.Permissions;
using System.Runtime.InteropServices;

class Path
{
    [DllImport("kernel32", SetLastError=true)]
    static extern bool CreateDirectory(string name, SecurityAttribute sa);

    [DllImport("kernel32", SetLastError=true)]
    static extern bool RemoveDirectory(string name);

    [DllImport("kernel32", SetLastError=true)]
    static extern int GetCurrentDirectory(int bufSize, StringBuilder buf);

    [DllImport("kernel32", SetLastError=true)]
    static extern bool SetCurrentDirectory(string name);
}
```

### <a name="partial-methods-recap"></a>Částečné metody (rekapitulace)

Pokud deklarace metody obsahuje `partial` modifikátor, že metoda je říká, že ***částečná metoda***. Částečné metody se dají deklarovat jenom jako členy částečné typy ([částečné typy](classes.md#partial-types)) a můžou se několik omezení. Částečné metody jsou popsané v [částečné metody](classes.md#partial-methods).

### <a name="extension-methods"></a>Rozšiřující metody

Pokud je první parametr metody obsahuje `this` modifikátor, že metoda je říká, že ***– metoda rozšíření***. Metody rozšíření lze deklarovat pouze v obecné, bez vnoření statické třídy. První parametr metody rozšíření může mít jiné než žádné modifikátory `this`, a typ parametrů nemůže mít typ ukazatele.

Následuje příklad statické třídy, která deklaruje dvě rozšiřující metody:
```csharp
public static class Extensions
{
    public static int ToInt32(this string s) {
        return Int32.Parse(s);
    }

    public static T[] Slice<T>(this T[] source, int index, int count) {
        if (index < 0 || count < 0 || source.Length - index < count)
            throw new ArgumentException();
        T[] result = new T[count];
        Array.Copy(source, index, result, 0, count);
        return result;
    }
}
```

Metody rozšíření je regulární statickou metodu. Kromě toho, kde je její nadřazené statické třídy v oboru, metody rozšíření lze vyvolat pomocí syntaxe volání metody instance ([volání metod rozšíření](expressions.md#extension-method-invocations)), pomocí výrazu příjemce jako první argument.

Následující program používá rozšiřující metody deklarované výše:
```csharp
static class Program
{
    static void Main() {
        string[] strings = { "1", "22", "333", "4444" };
        foreach (string s in strings.Slice(1, 2)) {
            Console.WriteLine(s.ToInt32());
        }
    }
}
```

`Slice` Metoda je k dispozici na `string[]`a `ToInt32` metoda je k dispozici na `string`, protože deklarovány jako metody rozšíření. Význam tohoto programu je stejný jako běžný statická metoda volání následující za použití:
```csharp
static class Program
{
    static void Main() {
        string[] strings = { "1", "22", "333", "4444" };
        foreach (string s in Extensions.Slice(strings, 1, 2)) {
            Console.WriteLine(Extensions.ToInt32(s));
        }
    }
}
```

### <a name="method-body"></a>Tělo metody

*Method_body* metody deklarací se skládá z bloku textu, tělo výrazu nebo středníkem.

***Výsledek typu*** metody je `void` návratový typ je `void`, nebo pokud se tato metoda je asynchronní a návratový typ `System.Threading.Tasks.Task`. V opačném případě je typ výsledku jiné asynchronní metody jejího návratového typu a výsledný typ asynchronní metody s návratovým typem `System.Threading.Tasks.Task<T>` je `T`.

Pokud má metodu `void` výsledek typu a blok textu, `return` příkazy ([příkaz return](statements.md#the-return-statement)) v bloku nejsou povolené. Chcete-li zadat výraz. Pokud spuštění bloku void metoda obvykle hotové (to znamená, určují toky vypnout na konec těla metody), tato metoda jednoduše vrátí aktuální volajícímu.
    
Pokud má metodu `void` výsledek a tělo výrazu, výrazu `E` musí být *statement_expression*, a přesně odpovídá do bloku textu ve formátu textu `{ E; }`.
    
Když metoda má typ jiný než void výsledku a blok textu, každý `return` příkazu v bloku musejí zadat výraz, který je implicitně převést na typ výsledku. Koncový bod bloku těla metody vracející hodnotu nesmí být dosažitelný. Jinými slovy v metodě vrací hodnotu s tělem bloku, ovládací prvek není oprávněn tok koncem tělo metody.
    
Když metoda má typ jiný než void výsledku a tělo výrazu, výraz musí být implicitně převoditelná na typ výsledku a text se do bloku textu formuláře `{ return E; }`.
    
V příkladu
```csharp
class A
{
    public int F() {}            // Error, return value required

    public int G() {
        return 1;
    }

    public int H(bool b) {
        if (b) {
            return 1;
        }
        else {
            return 0;
        }
    }

    public int I(bool b) => b ? 1 : 0;
}
```
vrací hodnotu `F` metodu vede k chybě kompilace vzhledem k tomu, že lze vypnout na konec těla metody tok řízení. `G` a `H` metody jsou správné, protože se všemi možnými cestami spuštění ukončení v příkazu return, který určuje návratovou hodnotu. `I` – Metoda je správný, protože jeho text je ekvivalentní k blok příkazů, který se právě jedním návratovým příkazem. v něm.

### <a name="method-overloading"></a>Přetěžování metody

Pravidla pro řešení přetížení metody jsou popsané v [odvození typu](expressions.md#type-inference).

## <a name="properties"></a>Vlastnosti

A ***vlastnost*** je člen, který poskytuje přístup k typické pro objekt nebo třídu. Příkladem vlastnosti zahrnují délku řetězce, velikost písma, titulek okna, název zákazníka a tak dále. Vlastnosti jsou přirozené rozšíření polí – obě jsou pojmenované členy s přidružené typy a syntaxe pro přístup k vlastnosti a pole je stejná. Na rozdíl od pole, ale vlastnosti nejsou označení umístění úložiště. Místo toho mají vlastnosti ***přistupující objekty*** , které určují příkazy ke se spustí při jejich hodnoty jsou číst nebo zapisovat. Vlastnosti tedy poskytují mechanismus pro přidružení k čtení a zápis atributy objektu; akce navíc umožňují těchto atributů, které mají být vypočteny.

Vlastnosti jsou deklarovány pomocí *property_declaration*s:

```antlr
property_declaration
    : attributes? property_modifier* type member_name property_body
    ;

property_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    | 'static'
    | 'virtual'
    | 'sealed'
    | 'override'
    | 'abstract'
    | 'extern'
    | property_modifier_unsafe
    ;

property_body
    : '{' accessor_declarations '}' property_initializer?
    | '=>' expression ';'
    ;

property_initializer
    : '=' variable_initializer ';'
    ;
```

A *property_declaration* může zahrnovat sadu *atributy* ([atributy](attributes.md)) a platnou kombinaci čtyři přístupu modifikátory přístupu ([modifikátorů přístupu ](classes.md#access-modifiers)), `new` ([new – modifikátor](classes.md#the-new-modifier)), `static` ([statická a instanci metody](classes.md#static-and-instance-methods)), `virtual` ([virtuální metody](classes.md#virtual-methods)), `override` ([Přepište metody](classes.md#override-methods)), `sealed` ([zapečetěné metody](classes.md#sealed-methods)), `abstract` ([abstraktní metody](classes.md#abstract-methods)), a `extern` ([Externí metody](classes.md#external-methods)) modifikátory.

Deklarace vlastnosti se vztahují stejná pravidla jako deklarace metody ([metody](classes.md#methods)) s ohledem na platné kombinace modifikátorů.

*Typ* vlastnosti deklarace Určuje typ vlastnosti zavedeným deklarací a *member_name* Určuje název vlastnosti. Pokud není vlastnost člena implementace explicitního rozhraní, *member_name* je jednoduše k *identifikátor*. Člen implementace explicitního rozhraní ([implementace explicitního rozhraní členských](interfaces.md#explicit-interface-member-implementations)), *member_name* se skládá ze *interface_type* za nímž následuje " `.`"a *identifikátor*.

*Typ* vlastnosti musí být přístupné jako vlastnost vlastní ([usnadnění omezení](basic-concepts.md#accessibility-constraints)).

A *property_body* může buď skládat ze ***přistupující objekt textu*** nebo ***tělo výrazu***. V textu přístupový objekt *accessor_declarations*, musí být uzavřen v "`{`"a"`}`" tokeny, deklarujte přístupových objektů ([přistupující objekty](classes.md#accessors)) vlastnosti. Přístupové objekty zadejte spustitelné příkazy, které jsou přidružené k čtení a zápisu vlastnosti.

Tělo výrazu skládající se z `=>` následovaný *výraz* `E` a středník se na základní text příkazu `{ get { return E; } }`a proto jde použít jenom k určení jen pro funkci getter vlastnosti, ve kterém je výsledek getter dán jeden výraz.

A *property_initializer* nejde zadávat jenom automaticky implementované vlastnosti ([automaticky implementované vlastnosti](classes.md#automatically-implemented-properties)) a způsobí, že inicializace podkladové pole těchto vlastnosti s hodnotou dána *výraz*.

I když syntaxi pro přístup k vlastnosti je stejné jako pro pole, vlastnost není klasifikováno jako proměnnou. Proto není možné předat vlastnost jako `ref` nebo `out` argument.

Pokud obsahuje deklaraci vlastnosti `extern` modifikátor, je označen jako vlastnost ***vlastnost external***. Protože deklaraci vlastnost external poskytuje skutečné implementaci, každá z jeho *accessor_declarations* skládá se ze středníku.

### <a name="static-and-instance-properties"></a>Statické a instance vlastnosti

Pokud obsahuje deklaraci vlastnosti `static` modifikátor, vlastnost se říká, že ***statickou vlastnost***. Pokud ne `static` Modifikátor je k dispozici, je označen jako vlastnost ***vlastnost instance***.

Statická vlastnost není přidružena k určité instanci a je chyba kompilace k odkazování na `this` v přistupující objekty statickou vlastnost.

Vlastnost instance je přidružený k instanci dané třídy a tato instance je přístupná jako `this` ([tento přístup](expressions.md#this-access)) v přistupující objekty vlastnosti.

Pokud vlastnost se odkazuje v *member_access* ([přístup ke členu](expressions.md#member-access)) ve formátu `E.M`, pokud `M` je statická vlastnost `E` musí označují typ obsahující `M`a pokud `M` je vlastnost instance, E musí označit instanci typu, který obsahuje `M`.

Rozdíly mezi statické a členy instance jsou popsány dále v [statické a instance členy](classes.md#static-and-instance-members).

### <a name="accessors"></a>Přístupové objekty

*Accessor_declarations* vlastnosti zadejte spustitelné příkazy spojená se čtením a zápisem tuto vlastnost.

```antlr
accessor_declarations
    : get_accessor_declaration set_accessor_declaration?
    | set_accessor_declaration get_accessor_declaration?
    ;

get_accessor_declaration
    : attributes? accessor_modifier? 'get' accessor_body
    ;

set_accessor_declaration
    : attributes? accessor_modifier? 'set' accessor_body
    ;

accessor_modifier
    : 'protected'
    | 'internal'
    | 'private'
    | 'protected' 'internal'
    | 'internal' 'protected'
    ;

accessor_body
    : block
    | ';'
    ;
```

Deklarace přistupujícího objektu obsahovat *get_accessor_declaration*, *set_accessor_declaration*, nebo obojí. Každá deklarace přistupujícího objektu se skládá z tokenu `get` nebo `set` a volitelně *accessor_modifier* a *accessor_body*.

Použití *accessor_modifier*s se vztahují následující omezení:

*  *Accessor_modifier* nelze použít v rozhraní nebo člen implementace explicitního rozhraní.
*  Pro vlastnost nebo indexovací člen, který nemá žádné `override` modifikátor, *accessor_modifier* smí obsahovat pouze v případě, že vlastnost nebo indexovací člen jsou obě `get` a `set` přístupový objekt a pak je povolené jenom na jednu z těchto přístupové objekty.
*  Pro vlastnost nebo indexovací člen, který zahrnuje `override` modifikátor, přístupový objekt musí odpovídat *accessor_modifier*(pokud existuje) přistupujícího objektu přepsání.
*  *Accessor_modifier* musí deklarovat dostupnost, která je výhradně více omezující než je deklarovaná přístupnost člena vlastnost nebo indexovací člen, samotného. Abychom byli přesní:
   * Pokud vlastnost nebo indexer je deklarovaná přístupnost člena `public`, *accessor_modifier* může být buď `protected internal`, `internal`, `protected`, nebo `private`.
   * Pokud vlastnost nebo indexer je deklarovaná přístupnost člena `protected internal`, *accessor_modifier* může být buď `internal`, `protected`, nebo `private`.
   * Pokud vlastnost nebo indexer je deklarovaná přístupnost člena `internal` nebo `protected`, *accessor_modifier* musí být `private`.
   * Pokud vlastnost nebo indexer je deklarovaná přístupnost člena `private`, ne *accessor_modifier* mohou být použity.

Pro `abstract` a `extern` vlastnosti, *accessor_body* pro každý přistupující objekt zadán, je jednoduše středníkem. Neabstraktní, není externí vlastnost může mít každý *accessor_body* se středníkem, v takovém případě je ***automaticky implementovaná vlastnost*** ([automaticky implementované vlastnosti ](classes.md#automatically-implemented-properties)). Automaticky implementované vlastnosti musí mít alespoň přistupující objekt get. Pro přistupující objekty vlastnosti žádné jiné neabstraktní, není externí *accessor_body* je *bloku* který určuje příkazy ke se spustí při vyvolání odpovídající přistupujícího objektu.

A `get` přistupující objekt odpovídající konstruktor bez parametrů metody s návratovou hodnotou typu vlastnosti. Při odkazování na vlastnost ve výrazu, s výjimkou případů cílem přiřazení, `get` přistupující objekt vlastnosti je vyvolána k výpočtu hodnoty vlastnosti ([hodnot výrazů](expressions.md#values-of-expressions)). Text `get` přístupového objektu musí splňovat pravidla pro vrácení hodnoty metod popsaných v [tělo metody](classes.md#method-body). Konkrétně se všechny `return` příkazy v těle `get` přistupující objekt musejí zadat výraz, který je implicitně převést na typ vlastnosti. Kromě toho koncový bod `get` přistupující objekt nesmí být dosažitelný.

A `set` přistupující objekt odpovídající metodu s jednou hodnotou parametrem typu vlastnosti a `void` návratového typu. Implicitní parametr `set` přístupový objekt je vždy pojmenováno `value`. Když se odkazuje vlastnost jako cíl přiřazení ([operátory přiřazení](expressions.md#assignment-operators)), nebo jako operand `++` nebo `--` ([Příponové operátory Inkrementace a dekrementace operátory](expressions.md#postfix-increment-and-decrement-operators), [ Předpona Inkrementace a dekrementace operátory](expressions.md#prefix-increment-and-decrement-operators)), `set` přístupového objektu je volána s argumentem (jehož hodnota je pravá strana přiřazení nebo operand `++` nebo `--` operátor), který poskytuje novou hodnotu ([jednoduché přiřazení](expressions.md#simple-assignment)). Text `set` přístupového objektu musí splňovat pravidla pro `void` metod popsaných v [tělo metody](classes.md#method-body). Zejména `return` příkazů v `set` přistupující objekt textu nejsou povolené. Chcete-li zadat výraz. Protože `set` přístupového objektu má implicitně parametr s názvem `value`, je chyba kompilace pro deklarace lokální proměnné nebo konstantní v `set` přístupový objekt s tímto názvem.

Záleží na přítomnosti nebo absenci `get` a `set` přistupující objekty vlastnosti klasifikovaný následujícím způsobem:

*  Vlastnost, která obsahuje oba `get` přístupového objektu a `set` přístupového objektu se říká, že ***čtení a zápis*** vlastnost.
*  Vlastnost, která má pouze `get` přístupového objektu se říká, že ***jen pro čtení*** vlastnost. Je chyba kompilace pro vlastnosti jen pro čtení a být cílem přiřazení.
*  Vlastnost, která má pouze `set` přístupového objektu se říká, že ***jen pro zápis*** vlastnost. S výjimkou jako cílem přiřazení, je chyba kompilace k odkazování na vlastnost jen pro zápis ve výrazu.

V příkladu
```csharp
public class Button: Control
{
    private string caption;

    public string Caption {
        get {
            return caption;
        }
        set {
            if (caption != value) {
                caption = value;
                Repaint();
            }
        }
    }

    public override void Paint(Graphics g, Rectangle r) {
        // Painting code goes here
    }
}
```
`Button` ovládací prvek deklaruje veřejnou `Caption` vlastnost. `get` Přistupující objekt `Caption` vlastnost vrací řetězec uložený v privátní `caption` pole. `set` Přistupující objekt kontroluje, jestli je nová hodnota se liší od aktuální hodnoty a pokud ano, uloží novou hodnotu a překreslí ovládacího prvku. Vlastnosti často mají tvar uvedené výše: `get` Přistupující objekt jednoduše vrací hodnotu uloženou v soukromé pole a `set` přistupující objekt upravuje privátní pole a potom provede žádné další akce pro úplnou aktualizaci stavu objektu.

Zadaný `Button` třídy výše, následuje příklad použití `Caption` vlastnost:
```csharp
Button okButton = new Button();
okButton.Caption = "OK";            // Invokes set accessor
string s = okButton.Caption;        // Invokes get accessor
```

Tady `set` přístupového objektu je vyvolán přiřazení hodnoty k vlastnosti a `get` přístupového objektu je vyvolán pomocí odkazu na vlastnost ve výrazu.

`get` a `set` přistupující objekty vlastnosti nejsou odlišné členy a není možné deklarovat přistupující objekty vlastnosti samostatně. V důsledku toho není možné pro dva přistupující objekty vlastnosti pro čtení i zápis mít různou přístupností. V příkladu
```csharp
class A
{
    private string name;

    public string Name {                // Error, duplicate member name
        get { return name; }
    }

    public string Name {                // Error, duplicate member name
        set { name = value; }
    }
}
```
nedeklaruje jedné vlastnosti pro čtení i zápis. Místo toho deklaruje dvě vlastnosti se stejným názvem, jeden jen pro čtení a jednu pouze pro zápis. Protože dva členy deklarované ve stejné třídě, nemůže mít stejný název, příklad způsobí chybu kompilace dojde k.

Pokud odvozená třída deklaruje vlastnost se stejným názvem jako o zděděnou vlastnost, vlastnost odvozené skryje zděděné vlastnosti s ohledem na čtení i zápis. V příkladu
```csharp
class A
{
    public int P {
        set {...}
    }
}

class B: A
{
    new public int P {
        get {...}
    }
}
```
`P` vlastnost `B` skryje `P` vlastnost `A` s ohledem na čtení i zápis. Díky tomu se v příkazech
```csharp
B b = new B();
b.P = 1;          // Error, B.P is read-only
((A)b).P = 1;     // Ok, reference to A.P
```
přiřazení `b.P` způsobí chybu kompilace má být hlášen, protože jen pro čtení `P` vlastnost `B` skryje jen pro zápis `P` vlastnost v `A`. Upozorňujeme, že přetypování lze použít pro přístup k skrytého `P` vlastnost.

Na rozdíl od veřejné pole vlastnosti poskytují oddělení mezi vnitřní stav objektu a jeho veřejné rozhraní. Podívejte se na příklad:
```csharp
class Label
{
    private int x, y;
    private string caption;

    public Label(int x, int y, string caption) {
        this.x = x;
        this.y = y;
        this.caption = caption;
    }

    public int X {
        get { return x; }
    }

    public int Y {
        get { return y; }
    }

    public Point Location {
        get { return new Point(x, y); }
    }

    public string Caption {
        get { return caption; }
    }
}
```

Tady `Label` používá třídy, dvě `int` pole, `x` a `y`, k jeho umístění úložiště. Umístění je veřejně vystavené jako `X` a `Y` vlastnost a jako `Location` vlastnost typu `Point`. Pokud v budoucí verzi systému `Label`, bude pohodlnější umístění jako úložiště `Point` interně, může být změnu provést, aniž by to ovlivnilo veřejné rozhraní třídy:
```csharp
class Label
{
    private Point location;
    private string caption;

    public Label(int x, int y, string caption) {
        this.location = new Point(x, y);
        this.caption = caption;
    }

    public int X {
        get { return location.x; }
    }

    public int Y {
        get { return location.y; }
    }

    public Point Location {
        get { return location; }
    }

    public string Caption {
        get { return caption; }
    }
}
```

Měl `x` a `y` místo toho byla `public readonly` pole, bylo by možné provést změnu na `Label` třídy.

Vystavení stav prostřednictvím vlastnosti není nutně všechny méně efektivní než zveřejnění pole přímo. Zejména při vlastnost nevirtuální a obsahuje pouze malé množství kódu, prostředí pro spuštění mohou nahradit volání přistupující objekty skutečný kód přístupové objekty. Tento proces se označuje jako ***vkládání***, a umožňuje tak efektivní jako přístup k poli přístup k vlastnostem, ale zachová se zvýší flexibilita při vlastnosti.

Od volání `get` přístupový objekt je koncepčním ekvivalentem hodnoty pole pro čtení, bude považován za špatný styl programování pro `get` přistupující objekty pozorovatelný vedlejší účinky. V příkladu
```csharp
class Counter
{
    private int next;

    public int Next {
        get { return next++; }
    }
}
```
Hodnota `Next` vlastnost závisí na počet, kolikrát vlastnost má dříve navštívili. Proto přístup k vlastnosti vytváří pozorovatelný vedlejší efekt, a vlastnost by měla být implementována jako metoda místo.

"Žádné vedlejší účinky" konvence pro `get` přistupující objekty neznamená, že `get` přístupových objektů by měl vždy být zapsán jednoduše vrátit hodnoty uložené v polích. Ve skutečnosti `get` přistupující objekty často výpočtu hodnoty vlastnosti přístup k více polí nebo volání metod. Nicméně správně navržená `get` přístupového objektu provede žádná akce, které způsobují pozorovatelných změny ve stavu objektu.

Vlastnosti lze použít k odložení inicializace prostředku až do okamžiku, kdy se nejprve odkazuje. Příklad:
```csharp
using System.IO;

public class Console
{
    private static TextReader reader;
    private static TextWriter writer;
    private static TextWriter error;

    public static TextReader In {
        get {
            if (reader == null) {
                reader = new StreamReader(Console.OpenStandardInput());
            }
            return reader;
        }
    }

    public static TextWriter Out {
        get {
            if (writer == null) {
                writer = new StreamWriter(Console.OpenStandardOutput());
            }
            return writer;
        }
    }

    public static TextWriter Error {
        get {
            if (error == null) {
                error = new StreamWriter(Console.OpenStandardError());
            }
            return error;
        }
    }
}
```

`Console` Třída obsahuje tři vlastnosti `In`, `Out`, a `Error`, které představují standardní vstup, výstup a chyby zařízení. Zveřejněním tyto členy jako vlastnosti `Console` třídy můžete zpoždění inicializační, dokud skutečně spotřebujete. Například při prvním odkazující `Out` vlastnosti, jako v
```csharp
Console.Out.WriteLine("hello, world");
```
základní `TextWriter` pro vytvoření výstupního zařízení. Ale pokud žádný odkaz na aplikaci `In` a `Error` vlastnosti a pak žádné objekty jsou vytvořeny pro tato zařízení.

### <a name="automatically-implemented-properties"></a>Automaticky implementované vlastnosti

Automaticky implementované vlastnosti (nebo ***automatickou vlastnost*** zkráceně), je vlastnost bez extern neabstraktní s orgány pouze středník. Automatické vlastnosti musí mít přístupový objekt get a můžou mít přistupující objekt set.

Je-li vlastnost zadána jako automaticky implementovanou vlastnost, pole Skrytá zálohování je automaticky dostupný pro vlastnost a přístupové objekty jsou implementovány číst z a zapisovat do tohoto pole zálohování. Pokud automatickou vlastnost nemá přístupový objekt set, je považováno za pomocné pole `readonly` ([pole jen pro čtení](classes.md#readonly-fields)). Stejně jako `readonly` pole, jen pro funkci getter automatickou vlastnost může být přiřazena také v těle konstruktoru nadřazené třídy. Taková přiřazení se přiřadí přímo pomocné pole jen pro čtení vlastnosti.

Automatické vlastnosti můžou mít *property_initializer*, které platí přímo do pole zálohování jako *variable_initializer* ([proměnné inicializátory](classes.md#variable-initializers)) .

V následujícím příkladu:
```csharp
public class Point {
    public int X { get; set; } = 0;
    public int Y { get; set; } = 0;
}
```
je ekvivalentní následující deklarace:
```csharp
public class Point {
    private int __x = 0;
    private int __y = 0;
    public int X { get { return __x; } set { __x = value; } }
    public int Y { get { return __y; } set { __y = value; } }
}
```

V následujícím příkladu:
```csharp
public class ReadOnlyPoint
{
    public int X { get; }
    public int Y { get; }
    public ReadOnlyPoint(int x, int y) { X = x; Y = y; }
}
```
je ekvivalentní následující deklarace:
```csharp
public class ReadOnlyPoint
{
    private readonly int __x;
    private readonly int __y;
    public int X { get { return __x; } }
    public int Y { get { return __y; } }
    public ReadOnlyPoint(int x, int y) { __x = x; __y = y; }
}
```

Všimněte si, že přiřazení pole jen pro čtení jsou platné, protože k nim dojde v rámci konstruktoru.


### <a name="accessibility"></a>Usnadnění

Pokud má přistupující objekt *accessor_modifier*, tak doména přístupnosti ([usnadnění domén](basic-concepts.md#accessibility-domains)) přístupového objektu je určen pomocí deklarovaná přístupnost člena *accessor_modifier* . Pokud nemá přistupující objekt *accessor_modifier*, tak doména přístupnosti přístupového objektu se určí na základě deklarovaná přístupnost člena vlastnost nebo indexer.

Přítomnost *accessor_modifier* nikdy ovlivňuje člen vyhledávání ([operátory](expressions.md#operators)) nebo rozlišení přetěžování ([rozlišení přetěžování](expressions.md#overload-resolution)). Modifikátory pro vlastnost nebo indexovací člen vždy určit, která vlastnost nebo indexer je vázán na, bez ohledu na místní přístup.

Jakmile byl vybrán konkrétní vlastnost nebo indexovací člen, usnadnění domén konkrétní přistupující objekty používané slouží k určení platnost tohoto využití:

*  Pokud jako hodnotu ([hodnot výrazů](expressions.md#values-of-expressions)), `get` přístupového objektu musí existovat a být přístupná.
*  Pokud jako cíl jednoduché přiřazení ([jednoduché přiřazení](expressions.md#simple-assignment)), `set` přístupového objektu musí existovat a být přístupná.
*  Při použití jako cíl složeného přiřazení ([složené přiřazení](expressions.md#compound-assignment)), nebo jako cíl `++` nebo `--` operátory ([funkce členy](expressions.md#function-members).9, [ Výrazy typu Invocation](expressions.md#invocation-expressions)), i `get` přístupové objekty a `set` přístupového objektu musí existovat a být přístupná.

V následujícím příkladu, vlastnost `A.Text` byl skrytý za vlastnost `B.Text`, i v kontextech, kde je to jenom `set` přístupového objektu je volána. Naproti tomu, vlastnost `B.Count` není přístupná pro třídy `M`, takže vlastnost přístupné `A.Count` místo ní se použije.

```csharp
class A
{
    public string Text {
        get { return "hello"; }
        set { }
    }

    public int Count {
        get { return 5; }
        set { }
    }
}

class B: A
{
    private string text = "goodbye"; 
    private int count = 0;

    new public string Text {
        get { return text; }
        protected set { text = value; }
    }

    new protected int Count { 
        get { return count; }
        set { count = value; }
    }
}

class M
{
    static void Main() {
        B b = new B();
        b.Count = 12;             // Calls A.Count set accessor
        int i = b.Count;          // Calls A.Count get accessor
        b.Text = "howdy";         // Error, B.Text set accessor not accessible
        string s = b.Text;        // Calls B.Text get accessor
    }
}
```

Přístupový objekt, který se používá k implementaci rozhraní nemusí mít *accessor_modifier*. Pokud pouze jeden přistupující objekt se používá k implementaci rozhraní, další přístupový objekt mohou být deklarovány pomocí *accessor_modifier*:
```csharp
public interface I
{
    string Prop { get; }
}

public class C: I
{
    public Prop {
        get { return "April"; }       // Must not have a modifier here
        internal set {...}            // Ok, because I.Prop has no set accessor
    }
}
```

### <a name="virtual-sealed-override-and-abstract-property-accessors"></a>Virtuální zapečetěná, přepsání a přístupové objekty abstraktní vlastnosti

A `virtual` deklaraci vlastnosti určuje, že jsou virtuální přistupující objekty vlastnosti. `virtual` Modifikátor se vztahuje na oba přistupující objekty vlastnosti pro čtení i zápis, není možné, pouze jeden přistupující objekt vlastnosti pro čtení a zápis na virtuální.

`abstract` Deklaraci vlastnosti určuje, že jsou virtuální přistupující objekty vlastnosti, ale neposkytuje Skutečná implementace přístupové objekty. Odvozené neabstraktní třídy místo toho je potřeba poskytli vlastní implementaci pro přístupové objekty tak, že přepíšete vlastnost. Protože přístupový objekt pro deklaraci abstraktní vlastnosti poskytuje skutečné implementaci, jeho *accessor_body* jednoduše se skládá ze středníku.

Deklarace vlastnosti, která zahrnuje i `abstract` a `override` modifikátory Určuje, že vlastnost je abstraktní a přepisuje základní vlastnost. Přístupové objekty tyto vlastnosti jsou také abstraktní.

Abstraktní vlastnost deklarace jsou povolené jenom v abstraktní třídy ([abstraktní třídy](classes.md#abstract-classes)). Přistupující objekty vlastnosti zděděné virtuální lze přepsat v odvozené třídě včetně deklaraci vlastnosti, která určuje, `override` směrnice. Jedná se ***přepsání deklarace vlastnosti***. Přepsání deklarace vlastnosti nedeklaruje novou vlastnost. Místo toho ji jednoduše se specializuje implementace přistupující objekty existující virtuální vlastnosti.

Přepsání deklarace vlastnost musíte zadat přesně stejné modifikátory, typ a název jako zděděné vlastnosti. Pokud zděděnou vlastnost má pouze jeden přistupující objekt (například pokud zděděnou vlastnost je jen pro čtení nebo jen pro zápis), vlastnost přepsání musí obsahovat pouze přistupujícího objektu. Pokud zděděné vlastnosti obsahuje oba přistupující objekty (tj. Pokud zděděné vlastnosti je čtení a zápis), vlastnost přepsání může obsahovat jeden přistupující objekt nebo oba přistupující objekty.

Může obsahovat deklaraci vlastnosti přepsání `sealed` modifikátor. Použití tohoto modifikátoru zabrání další přepisuje vlastnost odvozené třídy. Přístupové objekty zapečetěné vlastnosti jsou také zapečetěné.

Až na pár rozdílů v deklarace a volání syntaxe, virtuální, zapečetěné, přepsání a abstraktní přistupující objekty chovají stejně jako virtuální, zapečetěné, přepsání a abstraktní metody. Konkrétně pravidel popsaných v [virtuální metody](classes.md#virtual-methods), [přepište metody](classes.md#override-methods), [zapečetěné metody](classes.md#sealed-methods), a [abstraktní metody](classes.md#abstract-methods) použít jako přístupové objekty byly metody odpovídající formuláře:

*  A `get` přistupující objekt odpovídající konstruktor bez parametrů metody s návratovou hodnotou vlastnosti typu a stejného modifikátory jako příslušná vlastnost.
*  A `set` přistupující objekt odpovídající metodu s jednou hodnotou parametrem typu vlastnosti `void` návratový typ a modifikátory stejné jako příslušná vlastnost.

V příkladu
```csharp
abstract class A
{
    int y;

    public virtual int X {
        get { return 0; }
    }

    public virtual int Y {
        get { return y; }
        set { y = value; }
    }

    public abstract int Z { get; set; }
}
```
`X` je virtuální vlastnost jen pro čtení, `Y` je virtuální vlastnost pro čtení i zápis, a `Z` je abstraktní vlastnost pro čtení i zápis. Protože `Z` je abstraktní třídu obsahující `A` také musí být deklarovaná jako abstraktní.

Třída, která je odvozena z `A` se zobrazuje níže:
```csharp
class B: A
{
    int z;

    public override int X {
        get { return base.X + 1; }
    }

    public override int Y {
        set { base.Y = value < 0? 0: value; }
    }

    public override int Z {
        get { return z; }
        set { z = value; }
    }
}
```

Tady, prohlášení o `X`, `Y`, a `Z` jsou přepsání deklarace vlastností. Každá vlastnost deklarace přesně shoduje se modifikátory, typ a název odpovídající zděděné vlastnosti. `get` Přistupující objekt `X` a `set` přistupující objekt `Y` použít `base` – klíčové slovo pro přístup k zděděné přistupující objekty. Deklarace `Z` přepíše přistupující objekty jak abstraktní – proto nejsou žádní členové nezpracovaných abstraktní funkce v `B`, a `B` smí být neabstraktní třída.

Když je vlastnost deklarován jako `override`, všechny přistupující objekty přepsané musí být přístupná pro přepsání kódu. Kromě toho je deklarovaná přístupnost vlastnosti nebo indexeru, samotného a přistupujících objektů, se musí shodovat s přepsanému členu a přístupové objekty. Příklad:
```csharp
public class B
{
    public virtual int P {
        protected set {...}
        get {...}
    }
}

public class D: B
{
    public override int P {
        protected set {...}            // Must specify protected here
        get {...}                      // Must not have a modifier here
    }
}
```

## <a name="events"></a>Události

***Události*** je člen, který umožňuje objektu nebo třídě poskytnout oznámení. Klienti mohou připojit spustitelný kód pro události zadáním ***obslužné rutiny událostí***.

Události jsou deklarovány pomocí *event_declaration*s:

```antlr
event_declaration
    : attributes? event_modifier* 'event' type variable_declarators ';'
    | attributes? event_modifier* 'event' type member_name '{' event_accessor_declarations '}'
    ;

event_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    | 'static'
    | 'virtual'
    | 'sealed'
    | 'override'
    | 'abstract'
    | 'extern'
    | event_modifier_unsafe
    ;

event_accessor_declarations
    : add_accessor_declaration remove_accessor_declaration
    | remove_accessor_declaration add_accessor_declaration
    ;

add_accessor_declaration
    : attributes? 'add' block
    ;

remove_accessor_declaration
    : attributes? 'remove' block
    ;
```

*Event_declaration* může zahrnovat sadu *atributy* ([atributy](attributes.md)) a platnou kombinaci čtyři přístupu modifikátory přístupu ([modifikátorů přístupu ](classes.md#access-modifiers)), `new` ([new – modifikátor](classes.md#the-new-modifier)), `static` ([statická a instanci metody](classes.md#static-and-instance-methods)), `virtual` ([virtuální metody](classes.md#virtual-methods)), `override` ([Přepište metody](classes.md#override-methods)), `sealed` ([zapečetěné metody](classes.md#sealed-methods)), `abstract` ([abstraktní metody](classes.md#abstract-methods)), a `extern` ([Externí metody](classes.md#external-methods)) modifikátory.

Deklarace událostí se vztahují stejná pravidla jako deklarace metody ([metody](classes.md#methods)) s ohledem na platné kombinace modifikátorů.

*Typ* události musí být *delegate_type* ([referenční typy](types.md#reference-types)) a že *delegate_type* musí být dlouhý aspoň jako dostupné jako samotné události ([usnadnění omezení](basic-concepts.md#accessibility-constraints)).

Deklaraci události může zahrnovat *event_accessor_declarations*. Nicméně pokud tomu tak není, pro jiné externí, neabstraktní událostí, kompilátor poskytuje je automaticky ([události podobné poli](classes.md#field-like-events)); pro externí události přístupové objekty jsou k dispozici externě.

Deklaraci události, která vynechává *event_accessor_declarations* definuje jeden nebo více událostí – jeden pro každou z *variable_declarator*s. Atributy a modifikátory platí pro všechny členy deklarované pomocí takové *event_declaration*.

Je chyba kompilace pro *event_declaration* oba `abstract` modifikátor a oddělené složenou závorku *event_accessor_declarations*.

Pokud obsahuje deklaraci události `extern` modifikátor, události se říká, že ***externí událost***. Protože deklaraci externí události poskytuje skutečné implementaci, jedná se o chybu, aby se oba `extern` modifikátor a *event_accessor_declarations*.

Je chyba kompilace pro *variable_declarator* deklarace událostí pomocí `abstract` nebo `external` modifikátor zahrnout *variable_initializer*.

Událost lze použít jako operand na levé straně `+=` a `-=` operátory ([události přiřazení](expressions.md#event-assignment)). Tyto operátory používají, připojit obslužných rutin událostí k a odebrání obslužné rutiny události z události, a řízení přístupu modifikátory přístupu události kontextech, ve kterých jsou povolené tyto operace.

Protože `+=` a `-=` jsou jediné operace, které jsou povoleny pro událost mimo typ, který deklaruje událost, externí kód můžete přidat a odebrat obslužné rutiny události, ale nelze žádným způsobem získat nebo upravit základní seznam událostí obslužné rutiny.

V operaci formuláře `x += y` nebo `x -= y`, když `x` je událost a odkaz na něho mimo typ, který obsahuje deklaraci `x`, výsledek operace má typ `void` (na rozdíl od nutnosti typ `x`, s hodnotou `x` po přiřazení). Toto pravidlo zakazuje externí kód nepřímo zkoumání základního delegáta události.

Následující příklad ukazuje, jak obslužné rutiny událostí jsou připojené k instancím typu `Button` třídy:
```csharp
public delegate void EventHandler(object sender, EventArgs e);

public class Button: Control
{
    public event EventHandler Click;
}

public class LoginDialog: Form
{
    Button OkButton;
    Button CancelButton;

    public LoginDialog() {
        OkButton = new Button(...);
        OkButton.Click += new EventHandler(OkButtonClick);
        CancelButton = new Button(...);
        CancelButton.Click += new EventHandler(CancelButtonClick);
    }

    void OkButtonClick(object sender, EventArgs e) {
        // Handle OkButton.Click event
    }

    void CancelButtonClick(object sender, EventArgs e) {
        // Handle CancelButton.Click event
    }
}
```

Tady `LoginDialog` konstruktor instance vytvoří dva `Button` instance a připojí obslužných rutin událostí k `Click` události.

### <a name="field-like-events"></a>Události podobné poli

V rámci programu textu dané třídy nebo struktury, která obsahuje deklaraci události některé události lze použít jako pole. Použije tímto způsobem, nesmí být událost `abstract` nebo `extern`a nesmějí obsahovat explicitně *event_accessor_declarations*. Tato událost lze použít v libovolném kontextu, který umožňuje pole. Toto pole obsahuje delegáta ([delegáti](delegates.md)) odkazuje na seznam obslužných rutin událostí, které byly přidány k této události. Pokud nebyly přidány žádné obslužné rutiny událostí, bude pole obsahovat `null`.

V příkladu
```csharp
public delegate void EventHandler(object sender, EventArgs e);

public class Button: Control
{
    public event EventHandler Click;

    protected void OnClick(EventArgs e) {
        if (Click != null) Click(this, e);
    }

    public void Reset() {
        Click = null;
    }
}
```
`Click` slouží jako pole v rámci `Button` třídy. Jak ukazuje příklad, pole může být prověřit, upravit a použít ve výrazech vyvolání delegáta. `OnClick` Metodu `Button` třídy "vyvolá" `Click` událostí. Pojem vyvolání události je přesně ekvivalentní k vyvolání delegáta představované událostí – to znamená, nejsou žádné zvláštní jazykovým konstrukcím pro vyvolání události. Všimněte si, že při vyvolání delegáta předchází kontrolou, která zajistí, že delegát je jiná než null.

Mimo deklaraci `Button` třídy, `Click` člena lze použít pouze na levé straně `+=` a `-=` operátory, jako v
```csharp
b.Click += new EventHandler(...);
```
která připojí do seznamu vyvolání delegáta `Click` události, a
```csharp
b.Click -= new EventHandler(...);
```
které odebere ze seznamu vyvolání delegáta `Click` událostí.

Při kompilaci pole podobných událostí, kompilátor automaticky vytvoří úložiště pro uložení delegáta a vytvoří přistupující objekty události, které přidání nebo odebrání obslužných rutin událostí k pole delegáta. Operace přidání a odebrání jsou bezpečná a může (ale nemusíte) být neučinili při držení zámku ([příkaz lock](statements.md#the-lock-statement)) na objekt obsahující instanci události, nebo objekt typu ([anonymní výrazy vytvoření objektu](expressions.md#anonymous-object-creation-expressions)) pro statické události.

Proto deklaraci události instance formuláře:
```csharp
class X
{
    public event D Ev;
}
```
bude zkompilována do něco ekvivalentní:
```csharp
class X
{
    private D __Ev;  // field to hold the delegate

    public event D Ev {
        add {
            /* add the delegate in a thread safe way */
        }

        remove {
            /* remove the delegate in a thread safe way */
        }
    }
}
```
V rámci třídy `X`, odkazy na `Ev` na levé straně `+=` a `-=` operátory způsobit přidat a odebrat přístupové objekty, které má být volána. Všechny odkazy na `Ev` jsou kompilovány do odkazu skryté pole `__Ev` místo ([přístup ke členu](expressions.md#member-access)). Název "`__Ev`" libovolný; skryté pole může mít libovolný název nebo není název vůbec.

### <a name="event-accessors"></a>Přístupové objekty událostí

Deklarace událostí obvykle vynechat *event_accessor_declarations*, například `Button` výše uvedený příklad. Jedna situace to tedy zahrnuje případ, ve kterém není přijatelná náklady na uložení jednoho pole na událost. V takových případech může obsahovat třídu *event_accessor_declarations* a použít privátní mechanismus pro ukládání seznamu obslužných rutin událostí.

*Event_accessor_declarations* události zadejte spustitelné příkazy, které jsou přidružené k přidávání a odebírání obslužných rutin událostí.

Deklarace přistupujícího objektu se skládají z *add_accessor_declaration* a *remove_accessor_declaration*. Každá deklarace přistupujícího objektu se skládá z tokenu `add` nebo `remove` následovaný *bloku*. *Bloku* přidružené *add_accessor_declaration* určuje příkazy ke spuštění při přidání obslužné rutiny události a *bloku* přidružené *remove_accessor_declaration* určuje příkazy ke spuštění při odebrání obslužné rutiny události.

Každý *add_accessor_declaration* a *remove_accessor_declaration* odpovídá metodu s jednou hodnotou parametru typu události a `void` návratového typu. Implicitní parametr přístupový objekt události je pojmenován `value`. Při použití události v úloze událostí se používá přístupového objektu příslušné události. Konkrétně Pokud operátor přiřazení je `+=` použije přístupový objekt add a operátor přiřazení je `-=` použije přístupový objekt remove. V obou případech zpracovával pravý operand operátoru přiřazení slouží jako argument přístupového objektu události. Blok *add_accessor_declaration* nebo *remove_accessor_declaration* musí splňovat pravidla pro `void` metod popsaných v [tělo metody](classes.md#method-body). Zejména `return` příkazy v těchto bloku není povoleno zadat výraz.

Protože přístupový objekt události má parametr s názvem implicitně `value`, je chyba kompilace pro místní proměnné nebo konstantní deklarované v přístupový objekt události s tímto názvem.

V příkladu
```csharp
class Control: Component
{
    // Unique keys for events
    static readonly object mouseDownEventKey = new object();
    static readonly object mouseUpEventKey = new object();

    // Return event handler associated with key
    protected Delegate GetEventHandler(object key) {...}

    // Add event handler associated with key
    protected void AddEventHandler(object key, Delegate handler) {...}

    // Remove event handler associated with key
    protected void RemoveEventHandler(object key, Delegate handler) {...}

    // MouseDown event
    public event MouseEventHandler MouseDown {
        add { AddEventHandler(mouseDownEventKey, value); }
        remove { RemoveEventHandler(mouseDownEventKey, value); }
    }

    // MouseUp event
    public event MouseEventHandler MouseUp {
        add { AddEventHandler(mouseUpEventKey, value); }
        remove { RemoveEventHandler(mouseUpEventKey, value); }
    }

    // Invoke the MouseUp event
    protected void OnMouseUp(MouseEventArgs args) {
        MouseEventHandler handler; 
        handler = (MouseEventHandler)GetEventHandler(mouseUpEventKey);
        if (handler != null)
            handler(this, args);
    }
}
```
`Control` třída implementuje mechanismus interní úložiště pro události. `AddEventHandler` Metoda přidruží delegáta hodnotu s klíčem, `GetEventHandler` delegáta aktuálně přidružené s klíčem, vrátí metoda hodnotu a `RemoveEventHandler` metoda odstraní delegáta jako obslužnou rutinu události pro zadané události. Pravděpodobně základní mechanismus úložiště je navržená tak, aby se neúčtují žádné poplatky pro přidružení `null` delegovat hodnotu s klíčem, a proto neošetřené události využívat žádné úložiště.

### <a name="static-and-instance-events"></a>Statické a instance události

Pokud obsahuje deklaraci události `static` modifikátor, události se říká, že ***statické události***. Pokud ne `static` Modifikátor je k dispozici, události se říká, že ***událost instance***.

Statické události není přidružena k určité instanci a je chyba kompilace k odkazování na `this` v přístupových objektů statické události.

Instance události je přidružen k dané instanci třídy a tato instance je přístupná jako `this` ([tento přístup](expressions.md#this-access)) v přístupové objekty z této události.

Kdy se událost odkazuje v *member_access* ([přístup ke členu](expressions.md#member-access)) ve formátu `E.M`, pokud `M` je statická, `E` musí označují typ obsahující `M`a pokud `M` je událost instance E musí označit instanci typu, který obsahuje `M`.

Rozdíly mezi statické a členy instance jsou popsány dále v [statické a instance členy](classes.md#static-and-instance-members).

### <a name="virtual-sealed-override-and-abstract-event-accessors"></a>Virtuální zapečetěná, přepsání a přístupových objektů událostí abstraktní

A `virtual` deklaraci události Určuje, že přístupové objekty z této události je virtuální. `virtual` Modifikátor se vztahuje na oba přistupující objekty události.

`abstract` Deklaraci události Určuje, že jsou virtuální přistupující objekty události, ale neposkytuje Skutečná implementace přístupové objekty. Odvozené neabstraktní třídy místo toho je potřeba poskytli vlastní implementaci pro přístupové objekty tak, že přepíšete události. Protože deklarace abstraktní událost poskytuje skutečné implementaci, nemůže poskytnout oddělených složenou závorku *event_accessor_declarations*.

Deklaraci události, která zahrnuje i `abstract` a `override` modifikátory Určuje, že událost je abstraktní a přepisuje základní událost. Přístupové objekty tyto události jsou také abstraktní.

Deklarace abstraktních událostí jsou povolené jenom v abstraktní třídy ([abstraktní třídy](classes.md#abstract-classes)).

Přistupující objekty zděděné virtuální událost lze přepsat v odvozené třídě včetně deklaraci události, která určuje, `override` modifikátor. Jedná se ***přepsání deklarace události***. Přepsání deklarace události nedeklaruje novou událost. Místo toho ji jednoduše se specializuje implementace přistupující objekty existující virtuální událost.

Přepsání deklarace události musíte zadat přesně stejné modifikátory, typ a název jako přepsané událost.

Přepsání deklarace události může zahrnovat `sealed` modifikátor. Použití tohoto modifikátoru zabrání přepsání další události odvozené třídy. Přístupové objekty zapečetěné události jsou také zapečetěné.

Je chyba kompilace pro přepsání deklarace události mají zahrnout `new` modifikátor.

Až na pár rozdílů v deklarace a volání syntaxe, virtuální, zapečetěné, přepsání a abstraktní přistupující objekty chovají stejně jako virtuální, zapečetěné, přepsání a abstraktní metody. Konkrétně pravidel popsaných v [virtuální metody](classes.md#virtual-methods), [přepište metody](classes.md#override-methods), [zapečetěné metody](classes.md#sealed-methods), a [abstraktní metody](classes.md#abstract-methods) použít jako přístupové objekty byly metody odpovídající formuláře. Odpovídá každé přístupový objekt pro metodu s jednou hodnotou parametru typu události, `void` návratový typ a stejné modifikátory jako obsahující události.

## <a name="indexers"></a>Indexery

***Indexer*** je člen, který umožňuje objektu indexovat stejným způsobem jako pole. Indexery jsou deklarovány pomocí *indexer_declaration*s:

```antlr
indexer_declaration
    : attributes? indexer_modifier* indexer_declarator indexer_body
    ;

indexer_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    | 'virtual'
    | 'sealed'
    | 'override'
    | 'abstract'
    | 'extern'
    | indexer_modifier_unsafe
    ;

indexer_declarator
    : type 'this' '[' formal_parameter_list ']'
    | type interface_type '.' 'this' '[' formal_parameter_list ']'
    ;

indexer_body
    : '{' accessor_declarations '}' 
    | '=>' expression ';'
    ;
```

*Indexer_declaration* může zahrnovat sadu *atributy* ([atributy](attributes.md)) a platnou kombinaci čtyři přístupu modifikátory přístupu ([modifikátorů přístupu ](classes.md#access-modifiers)), `new` ([new – modifikátor](classes.md#the-new-modifier)), `virtual` ([virtuální metody](classes.md#virtual-methods)), `override` ([přepište metody](classes.md#override-methods) ), `sealed` ([Zapečetěné metody](classes.md#sealed-methods)), `abstract` ([abstraktní metody](classes.md#abstract-methods)), a `extern` ([externí metody](classes.md#external-methods)) modifikátory.

Indexer deklarace se vztahují stejná pravidla jako deklarace metody ([metody](classes.md#methods)) s ohledem na platné kombinace modifikátorů s jednou výjimkou, která se statickém modifikátoru není povolena v deklaraci indexeru.

Modifikátory `virtual`, `override`, a `abstract` se navzájem vylučují s výjimkou v jednom případě. `abstract` a `override` modifikátory lze použít společně tak, aby abstraktní indexeru můžete přepsat virtuální jeden.

*Typ* deklarace Určuje typ elementu indexeru zavedeným deklarací z indexeru. Indexer se nejedná explicitní implementace členu rozhraní, *typ* postupuje podle klíčového slova `this`. Člen implementace explicitního rozhraní naleznete *typ* následuje *interface_type*, "`.`" a klíčové slovo `this`. Na rozdíl od jiných členů nemají indexery uživatelského jména.

*Formal_parameter_list* Určuje parametry indexeru. Seznam formálních parametrů indexer odpovídá signatuře metody ([parametry metody](classes.md#method-parameters)), s tím rozdílem, že musí být zadán minimálně jeden parametr a že `ref` a `out` nejsou povolené modifikátory parametrů .

*Typ* indexeru a každý z typů odkazuje *formal_parameter_list* musí být přinejmenším stejně dostupná jako samotný indexeru ([usnadnění omezení](basic-concepts.md#accessibility-constraints)).

*Indexer_body* může buď skládat ze ***přistupující objekt textu*** nebo ***tělo výrazu***. V textu přístupový objekt *accessor_declarations*, musí být uzavřen v "`{`"a"`}`" tokeny, deklarujte přístupových objektů ([přistupující objekty](classes.md#accessors)) vlastnosti. Přístupové objekty zadejte spustitelné příkazy, které jsou přidružené k čtení a zápisu vlastnosti.

Tělo výrazu skládající se z "`=>`" následovaného výrazem `E` a středník se na základní text příkazu `{ get { return E; } }`a proto jde použít jenom k určení indexery jen pro funkci getter kde je výsledek getter Dal jeden výraz.

I když syntaxi pro přístup k elementu indexeru je stejné jako pro prvek pole, není indexer element klasifikovaná jako proměnnou. Proto není možné předat jako element indexeru `ref` nebo `out` argument.

Seznam formálních parametrů indexer definuje podpis ([podpisy a přetížení](basic-concepts.md#signatures-and-overloading)) indexeru. Konkrétně podpis indexeru se skládá z počet a typy formálních parametrů. Typ elementu a názvy formálních parametrů nejsou součástí indexer podpis.

Podpis indexeru se musí lišit od podpisů ze všech indexerů deklarované ve stejné třídě.

Indexery a vlastnosti je v principu velmi podobné, ale liší následujícími způsoby:

*  Vlastnost je identifikován názvem, že indexer je identifikován jeho podpis.
*  Vlastnost je přístupný prostřednictvím *simple_name* ([jednoduché názvy](expressions.md#simple-names)) nebo *member_access* ([přístup ke členu](expressions.md#member-access)), že indexer Element je přístupný prostřednictvím *element_access* ([přístup indexeru](expressions.md#indexer-access)).
*  Vlastnost může být `static` člen, že indexer je vždy člena instance.
*  A `get` přistupující objekt vlastnosti odpovídající metody bez vstupních parametrů, že `get` přistupující objekt indexer odpovídá metodu s stejného seznamu formálních parametrů jako indexeru.
*  A `set` odpovídá přistupující objekt vlastnosti na metodu s jedním parametrem s názvem `value`, že `set` přistupující objekt indexer odpovídá metodu s stejného seznamu formálních parametrů jako indexeru a navíc další parametr s názvem `value`.
*  Je chyba kompilace pro indexer přístupový objekt pro deklarování místní proměnné se stejným názvem jako parametr indexeru.
*  V deklaraci vlastnosti přepsání zděděné vlastnosti přistupuje pomocí syntaxe `base.P`, kde `P` je název vlastnosti. V deklaraci indexer přepsání zděděné indexer přistupuje pomocí syntaxe `base[E]`, kde `E` je seznam výrazů oddělených čárkami.
*  Neexistuje koncept "automaticky implementované indexer". Jedná se o chybu, aby neabstraktní, neexterní indexer s průvodcem středník.

Kromě těchto rozdílů všechna pravidla definovaná v [přistupující objekty](classes.md#accessors) a [automaticky implementované vlastnosti](classes.md#automatically-implemented-properties) platí pro indexer přistupující objekty také jako přístupové objekty vlastnosti.

Pokud obsahuje deklaraci indexer `extern` modifikátor, indexeru se říká, že ***externí indexer***. Protože deklaraci externí indexer poskytuje skutečné implementaci, každá z jeho *accessor_declarations* skládá se ze středníku.

Následující příklad deklaruje `BitArray` třídu, která implementuje indexeru pro přístup k jednotlivým bitů v bitové pole.
```csharp
using System;

class BitArray
{
    int[] bits;
    int length;

    public BitArray(int length) {
        if (length < 0) throw new ArgumentException();
        bits = new int[((length - 1) >> 5) + 1];
        this.length = length;
    }

    public int Length {
        get { return length; }
    }

    public bool this[int index] {
        get {
            if (index < 0 || index >= length) {
                throw new IndexOutOfRangeException();
            }
            return (bits[index >> 5] & 1 << index) != 0;
        }
        set {
            if (index < 0 || index >= length) {
                throw new IndexOutOfRangeException();
            }
            if (value) {
                bits[index >> 5] |= 1 << index;
            }
            else {
                bits[index >> 5] &= ~(1 << index);
            }
        }
    }
}
```

Instance `BitArray` třídy spotřebovává podstatně menší paměti než odpovídající `bool[]` (protože každá hodnota bývalé zabírá místo pouze jeden bit odkazující na jeden bajt), ale umožňuje stejné operace jako `bool[]`.

Následující `CountPrimes` třídy používá `BitArray` a klasického "x" algoritmus vypočítat počet základen mezi 1 a daný maximální:
```csharp
class CountPrimes
{
    static int Count(int max) {
        BitArray flags = new BitArray(max + 1);
        int count = 1;
        for (int i = 2; i <= max; i++) {
            if (!flags[i]) {
                for (int j = i * 2; j <= max; j += i) flags[j] = true;
                count++;
            }
        }
        return count;
    }

    static void Main(string[] args) {
        int max = int.Parse(args[0]);
        int count = Count(max);
        Console.WriteLine("Found {0} primes between 1 and {1}", count, max);
    }
}
```

Všimněte si, že syntaxe pro přístup k prvků `BitArray` je přesně stejné jako v případě `bool[]`.

Následující příklad ukazuje třídu 26 * 10 mřížky, která má indexer se dvěma parametry. První parametr musí být velké nebo malé písmeno v rozsahu A – Z, a druhý musí být celé číslo v rozsahu 0 – 9.

```csharp
using System;

class Grid
{
    const int NumRows = 26;
    const int NumCols = 10;

    int[,] cells = new int[NumRows, NumCols];

    public int this[char c, int col] {
        get {
            c = Char.ToUpper(c);
            if (c < 'A' || c > 'Z') {
                throw new ArgumentException();
            }
            if (col < 0 || col >= NumCols) {
                throw new IndexOutOfRangeException();
            }
            return cells[c - 'A', col];
        }

        set {
            c = Char.ToUpper(c);
            if (c < 'A' || c > 'Z') {
                throw new ArgumentException();
            }
            if (col < 0 || col >= NumCols) {
                throw new IndexOutOfRangeException();
            }
            cells[c - 'A', col] = value;
        }
    }
}
```

### <a name="indexer-overloading"></a>Indexer přetížení

Pravidla pro řešení přetížení indexeru jsou popsány v [odvození typu](expressions.md#type-inference).

## <a name="operators"></a>Operátory

***Operátor*** je člen, který definuje význam výrazu operátor, který lze použít u instance třídy. Operátory jsou deklarovány pomocí *operator_declaration*s:

```antlr
operator_declaration
    : attributes? operator_modifier+ operator_declarator operator_body
    ;

operator_modifier
    : 'public'
    | 'static'
    | 'extern'
    | operator_modifier_unsafe
    ;

operator_declarator
    : unary_operator_declarator
    | binary_operator_declarator
    | conversion_operator_declarator
    ;

unary_operator_declarator
    : type 'operator' overloadable_unary_operator '(' type identifier ')'
    ;

overloadable_unary_operator
    : '+' | '-' | '!' | '~' | '++' | '--' | 'true' | 'false'
    ;

binary_operator_declarator
    : type 'operator' overloadable_binary_operator '(' type identifier ',' type identifier ')'
    ;

overloadable_binary_operator
    : '+'   | '-'   | '*'   | '/'   | '%'   | '&'   | '|'   | '^'   | '<<'
    | right_shift | '=='  | '!='  | '>'   | '<'   | '>='  | '<='
    ;

conversion_operator_declarator
    : 'implicit' 'operator' type '(' type identifier ')'
    | 'explicit' 'operator' type '(' type identifier ')'
    ;

operator_body
    : block
    | '=>' expression ';'
    | ';'
    ;
```

Existují tři kategorie přetížitelné operátory: Unární operátory ([unárních operátorů](classes.md#unary-operators)), binární operátory ([binární operátory](classes.md#binary-operators)) a operátory převodu ([operátory převodu](classes.md#conversion-operators)).

*Operator_body* je buď středník ***tělo s příkazy*** nebo ***tělo výrazu***. Tělo s příkazy se skládá z *bloku*, která určuje příkazy ke spuštění při vyvolání operátor. *Bloku* musí splňovat pravidla pro vrácení hodnoty metod popsaných v [tělo metody](classes.md#method-body). Tělo výrazu se skládá z `=>` následovat výraz a středník a označuje jeden výraz k provádění při vyvolání operátor.

Pro `extern` operátory, *operator_body* se skládá pouze ze středníku. Pro všechny ostatní operátory *operator_body* je blok textu nebo tělo výrazu.

Následující pravidla platí pro všechny deklarace operátoru:

*  Musí obsahovat deklarace operátoru `public` a `static` modifikátor.
*  Parametry operátoru musí být parametry s hodnotou ([hodnoty parametrů](variables.md#value-parameters)). Je chyba kompilace pro deklaraci operátor k určení `ref` nebo `out` parametry.
*  Podpis operátora ([unárních operátorů](classes.md#unary-operators), [binární operátory](classes.md#binary-operators), [operátory převodu](classes.md#conversion-operators)) se musí lišit od podpisů všechny ostatní operátory, které jsou deklarované v stejné třídy.
*  Všechny typy odkazované v deklarace operátoru musí být přinejmenším stejně dostupná jako operátor ([usnadnění omezení](basic-concepts.md#accessibility-constraints)).
*  Jedná se o chybu pro stejný modifikátor objevit více než jednou v deklarace operátoru.

Každá kategorie operátorů ukládá další omezení, jak je popsáno v následujících částech.

Stejně jako ostatní členové v základní třídě je deklarovaný operátory jsou zděděny z odvozených tříd. Deklarace operátoru vždy vyžadují dané třídy nebo struktury, ve kterém je operátor deklarován k účasti v signatuře operátoru, a proto není možné operátor deklarovaná v odvozené třídě skrýt operátor v základní třídě je deklarovaný. Proto `new` modifikátor se nikdy vyžaduje a proto nikdy povoleno, v deklarace operátoru.

Další informace o jednočlenné a binární operátory lze nalézt v [operátory](expressions.md#operators).

Další informace o operátory převodu lze nalézt v [uživatelem definované převody](conversions.md#user-defined-conversions).

### <a name="unary-operators"></a>Unární operátory

Následující pravidla platí pro unární operátor prohlášení, kde `T` označuje typ instance dané třídy nebo struktury, která obsahuje deklaraci operátor:

*  Unární operátor `+`, `-`, `!`, nebo `~` operátor musí vzít jeden parametr typu `T` nebo `T?` a může vrátit libovolného typu.
*  Unární operátor `++` nebo `--` operátor musí vzít jeden parametr typu `T` nebo `T?` a musí vrátit, že stejný typ nebo typ odvozený z něj.
*  Unární operátor `true` nebo `false` operátor musí vzít jeden parametr typu `T` nebo `T?` a musí vrátit typ `bool`.

Podpis unární operátor se skládá z tokenu – operátor (`+`, `-`, `!`, `~`, `++`, `--`, `true`, nebo `false`) a jeden formální parametr typu. Návratový typ není součástí unární operátor podpis, ani není název formálních parametrů.

`true` a `false` unárních operátorů vyžadují pair-wise deklarace. Chyba kompilace nastane, pokud jeden z těchto operátorů třída deklaruje bez také druhý deklarace. `true` a `false` operátory jsou popsány dále v [podmíněné logické operátory definované uživatelem](expressions.md#user-defined-conditional-logical-operators) a [logické výrazy](expressions.md#boolean-expressions).

Následující příklad ukazuje implementaci a následné použití `operator ++` pro třídu vector celé číslo:
```csharp
public class IntVector
{
    public IntVector(int length) {...}

    public int Length {...}                 // read-only property

    public int this[int index] {...}        // read-write indexer

    public static IntVector operator ++(IntVector iv) {
        IntVector temp = new IntVector(iv.Length);
        for (int i = 0; i < iv.Length; i++)
            temp[i] = iv[i] + 1;
        return temp;
    }
}

class Test
{
    static void Main() {
        IntVector iv1 = new IntVector(4);    // vector of 4 x 0
        IntVector iv2;

        iv2 = iv1++;    // iv2 contains 4 x 0, iv1 contains 4 x 1
        iv2 = ++iv1;    // iv2 contains 4 x 2, iv1 contains 4 x 2
    }
}
```

Všimněte si, jak metoda – operátor vrátí hodnotu vytvořený tak, že přidáte 1 s operandem, stejně jako zvýšení přípony a operátory snížení ([Příponové operátory Inkrementace a dekrementace operátory](expressions.md#postfix-increment-and-decrement-operators)) a předponu Inkrementace a dekrementace operátory ([předpony Inkrementace a dekrementace operátory](expressions.md#prefix-increment-and-decrement-operators)). Na rozdíl od v jazyce C++, tato metoda nemusí upravovat hodnota jeho operandu přímo. Ve skutečnosti Úprava hodnoty operandu by mohla narušit standardní sémantiku příponového operátoru Inkrementace.

### <a name="binary-operators"></a>Binární operátory

Následující pravidla platí pro binární operátor prohlášení, kde `T` označuje typ instance dané třídy nebo struktury, která obsahuje deklaraci operátor:

*  Binární operátor bez posunutí musí přebírají dva parametry, nejméně jeden z nich musí mít typ `T` nebo `T?`a může vrátit libovolného typu.
*  Binární soubor `<<` nebo `>>` operátor musí mít dva parametry, první z nich musí mít typ `T` nebo `T?` a druhý z nich musí mít typ `int` nebo `int?`a může vrátit libovolného typu.

Podpis binárního operátoru se skládá z tokenu – operátor (`+`, `-`, `*`, `/`, `%`, `&`, `|`, `^`, `<<`, `>>`, `==`, `!=`, `>`, `<`, `>=`, nebo `<=`) a typy dva formální parametry. Názvy formálních parametrů a návratový typ nejsou součástí podpis binárního operátoru.

Některé binární operátory vyžadují pair-wise deklarace. Pro každou deklaraci buď operátor dvojice musí být odpovídající deklarace operátoru které odpovídá páru licencí. Dvě deklarace operátoru případy, kdy se mají vracet hodnotu stejného typu a stejného typu pro každý parametr. Tyto operátory vyžadují pair-wise deklarace:

*  `operator ==` a `operator !=`
*  `operator >` a `operator <`
*  `operator >=` a `operator <=`

### <a name="conversion-operators"></a>operátory převodu

Zavádí deklaraci operátor převodu ***uživatelsky definovaný převod*** ([uživatelem definované převody](conversions.md#user-defined-conversions)) která posiluje předem definovaných implicitní a explicitní převody.

Deklarace operátor převodu, která zahrnuje `implicit` – klíčové slovo představuje uživatelem definovaný implicitní převod. Implicitní převod může dojít v různých situacích, včetně volání členské funkce, výrazy přetypování a přiřazení. Toto je popsáno dále v [implicitních převodů](conversions.md#implicit-conversions).

Deklarace operátor převodu, která zahrnuje `explicit` – klíčové slovo představuje uživatelem definovaný explicitní převod. Může dojít v výrazy přetypování explicitních převodů a jsou popsány dále v [explicitních převodů](conversions.md#explicit-conversions).

Operátor převodu převede od zdrojového typu, který je označen typ parametru operátorů převodu s cílovým typem indikován návratového typu operátoru převodu.

Pro typ daného zdroje `S` a cílový typ `T`, pokud `S` nebo `T` jsou typy s možnou hodnotou Null, umožní `S0` a `T0` odkazovat na základní typy, jinak `S0` a `T0` jsou rovno `S` a `T` v uvedeném pořadí. Třídy nebo struktury je povolené pro deklaraci převod z typu zdrojové `S` s cílovým typem `T` pouze v případě, že jsou splněny všechny z následujících akcí:

*  `S0` a `T0` jsou různé typy.
*  Buď `S0` nebo `T0` je typ třídy nebo struktury, ve kterém probíhá deklarace operátoru.
*  Ani `S0` ani `T0` je *interface_type*.
*  S výjimkou uživatelem definované převody neexistuje převod z `S` k `T` nebo z `T` k `S`.

Pro účely těchto pravidel se žádné parametry přidružené k typu `S` nebo `T` jsou považovány za jedinečné typy, které mají vztah dědičnosti s jinými typy a jakákoliv omezení u těchto typů parametry budou ignorovány.

V příkladu
```csharp
class C<T> {...}

class D<T>: C<T>
{
    public static implicit operator C<int>(D<T> value) {...}      // Ok
    public static implicit operator C<string>(D<T> value) {...}   // Ok
    public static implicit operator C<T>(D<T> value) {...}        // Error
}
```
první dva operátor deklarace jsou povolené, protože pro účely [indexery](classes.md#indexers).3, `T` a `int` a `string` v uvedeném pořadí jsou považovány za jedinečné typy se žádné relace. Ale třetí operátor se o chybu, protože `C<T>` je základní třída `D<T>`.

Z druhé pravidlo, že vyplývá, že operátor převodu musíte převést na nebo z typu třídy nebo struktury, ve kterém je operátor deklarován. Například je možné pro typ třídy nebo struktury `C` definovat převod z `C` k `int` a z `int` k `C`, ale ne z `int` k `bool`.

Není možné přímo předefinovat předem definovaný převod. Proto nejsou povoleny operátory převodu k převedení z nebo do `object` protože implicitní a explicitní převody již neexistuje mezi `object` a všechny ostatní typy. Podobně může být zdrojové ani cílové typy převodu základního typu, protože převod by pak již existuje.

Je však možné deklarovat operátory v obecných typech, které pro určitý typ argumentů, zadejte převody, které již existují jako předem definované převody. V příkladu
```csharp
struct Convertible<T>
{
    public static implicit operator Convertible<T>(T value) {...}
    public static explicit operator T(Convertible<T> value) {...}
}
```
Když zadejte `object` je zadaný jako argument typu pro `T`, druhý operátor deklaruje převod, který již existuje (implicitní a proto také explicitně, existuje převod z libovolný typ na typ `object`).

V případech, kde existuje předem definovaný převod mezi dvěma typy jsou ignorovány všechny uživatelem definované převody mezi typy. Konkrétně:

*  Pokud předdefinované implicitní převod ([implicitních převodů](conversions.md#implicit-conversions)) z typu `S` na typ `T`, všechny uživatelem definované převody (implicitní nebo explicitní) z `S` k `T` jsou ignorovány.
*  Pokud předem definované explicitní převod ([explicitních převodů](conversions.md#explicit-conversions)) z typu `S` na typ `T`, žádné uživatelem definované explicitní převody z `S` k `T` jsou ignorovány. Dále:

Pokud `T` je typu rozhraní, uživatelem definované implicitní převody z `S` k `T` jsou ignorovány.

Jinak, uživatelem definované implicitní převody z `S` k `T` jsou stále považovány za.

Pro všechny typy, ale `object`, operátory deklaroval `Convertible<T>` výše uvedených typů nejsou v rozporu s předem definované převody. Příklad:
```csharp
void F(int i, Convertible<int> n) {
    i = n;                          // Error
    i = (int)n;                     // User-defined explicit conversion
    n = i;                          // User-defined implicit conversion
    n = (Convertible<int>)i;        // User-defined implicit conversion
}
```

Ale pro typ `object`, uživatelem definované převody ve všech případech ale jeden skrýt předem definované převody:

```csharp
void F(object o, Convertible<object> n) {
    o = n;                         // Pre-defined boxing conversion
    o = (object)n;                 // Pre-defined boxing conversion
    n = o;                         // User-defined implicit conversion
    n = (Convertible<object>)o;    // Pre-defined unboxing conversion
}
```

Uživatelem definované převody nejsou povoleny pro převod z nebo do *interface_type*s. Konkrétně se toto omezení zajišťuje, že žádné uživatelem definované dochází při převodu na *interface_type*a že převod na *interface_type* proběhne úspěšně pouze pokud objekt Převádí se ve skutečnosti implementuje zadaný *interface_type*.

Podpis operátora převodu se skládá z zdrojového a cílového typu. (Všimněte si, že se jedná o jedinou formou člena, u kterého návratový typ se účastní signatura.) `implicit` Nebo `explicit` klasifikace operátor převodu není součást podpisu obsluhy. Proto třídy nebo struktury nelze deklarovat obě `implicit` a `explicit` operátor převodu se stejnými typy zdroj a cíl.

Obecně platí uživatelem definované implicitní převody by se měly navrhovat nikdy nevyvolají výjimky a nikdy dojít ke ztrátě informací. Je-li uživatelem definovaný převod může mít za následek výjimky (například, protože zdroj argument je mimo rozsah) nebo ke ztrátě informací (například se zahodí implikovaný bits) a pak tento převod musí být definován jako explicitní převod.

V příkladu
```csharp
using System;

public struct Digit
{
    byte value;

    public Digit(byte value) {
        if (value < 0 || value > 9) throw new ArgumentException();
        this.value = value;
    }

    public static implicit operator byte(Digit d) {
        return d.value;
    }

    public static explicit operator Digit(byte b) {
        return new Digit(b);
    }
}
```
Převod z `Digit` k `byte` je implicitní, protože nikdy vyvolá výjimky nebo ztratí informace, ale převod z `byte` k `Digit` je explicitní od `Digit` mohou představovat jenom podmnožinu možné hodnoty `byte`.

## <a name="instance-constructors"></a>Konstruktory instancí

***Konstruktor instance*** je člen, který implementuje akce potřebné k inicializaci instance třídy. Konstruktory instancí jsou deklarovány pomocí *constructor_declaration*s:

```antlr
constructor_declaration
    : attributes? constructor_modifier* constructor_declarator constructor_body
    ;

constructor_modifier
    : 'public'
    | 'protected'
    | 'internal'
    | 'private'
    | 'extern'
    | constructor_modifier_unsafe
    ;

constructor_declarator
    : identifier '(' formal_parameter_list? ')' constructor_initializer?
    ;

constructor_initializer
    : ':' 'base' '(' argument_list? ')'
    | ':' 'this' '(' argument_list? ')'
    ;

constructor_body
    : block
    | ';'
    ;
```

A *constructor_declaration* může zahrnovat sadu *atributy* ([atributy](attributes.md)), platnou kombinaci čtyři přístupu modifikátory přístupu ([modifikátorů přístupu ](classes.md#access-modifiers)) a `extern` ([externí metody](classes.md#external-methods)) modifikátor. Deklarace konstruktoru se nepovoluje patří stejné modifikátor více než jednou.

*Identifikátor* z *constructor_declarator* název musí být třída, ve kterém je deklarována konstruktor instance. Pokud je zadán žádným jiným názvem, dojde k chybě kompilace.

Volitelný *formal_parameter_list* instance konstruktor je v souladu s stejná pravidla jako *formal_parameter_list* metody ([metody](classes.md#methods)). Seznam formálních parametrů definuje podpis ([podpisy a přetížení](basic-concepts.md#signatures-and-overloading)) z konstruktoru instance a kterým se řídí proces rozlišení přetěžování ([odvození typu](expressions.md#type-inference)) vybere konkrétní konstruktor instance vyvolání.

Každý z typů odkazuje *formal_parameter_list* instance konstruktor musí být přinejmenším stejně dostupná jako konstruktor samotný ([usnadnění omezení](basic-concepts.md#accessibility-constraints)).

Volitelný *constructor_initializer* určuje jiný konstruktor instance, který má být vyvolán před spuštěním příkazů uvedených v *constructor_body* tento konstruktor instance. Toto je popsáno dále v [inicializátory konstruktoru](classes.md#constructor-initializers).

Pokud obsahuje deklaraci konstruktoru `extern` modifikátor, konstruktor se říká, že ***externí konstruktor***. Protože deklaraci externí konstruktor poskytuje skutečné implementaci, jeho *constructor_body* skládá se ze středníku. Pro všechny ostatní konstruktory *constructor_body* se skládá z *bloku* který určuje příkazy ke inicializuje novou instanci třídy. To přesně odpovídá *bloku* metody instance s `void` návratový typ ([tělo metody](classes.md#method-body)).

Konstruktory instancí nedědí. Proto třída nemá žádné konstruktory instancí než ty, které skutečně deklarovaná ve třídě. Pokud třída obsahuje žádné deklarace konstruktoru instance, se automaticky poskytuje výchozí konstruktor instance ([výchozí konstruktory](classes.md#default-constructors)).

Jsou vyvolány konstruktory instancí *object_creation_expression*s ([výrazy vytvoření objektu](expressions.md#object-creation-expressions)) a přes *constructor_initializer*s.

### <a name="constructor-initializers"></a>Inicializátory konstruktoru

Všechny konstruktory instancí (s výjimkou souborů pro třídu `object`) implicitně bezprostředně před patří volání jiného konstruktoru instance *constructor_body*. Konstruktoru implicitně závisí *constructor_initializer*:

*  Inicializátor konstruktoru instance formuláře `base(argument_list)` nebo `base()` způsobí, že od přímé základní třídy má být volána konstruktor instance. Tento konstruktor je zaškrtnuto, pomocí *argument_list* -li k dispozici a pravidla rozlišení přetížení [rozlišení přetěžování](expressions.md#overload-resolution). Sada konstruktory instancí Release candidate zahrnuje všechny dostupné instance konstruktory obsažené v přímé základní třídy nebo výchozího konstruktoru ([výchozí konstruktory](classes.md#default-constructors)), pokud jsou v deklarovány žádné konstruktory instancí přímé základní třídy. Pokud tato sada je prázdný nebo jediný konstruktor instance nejlepší nelze identifikovat, dojde k chybě kompilace.
*  Inicializátor konstruktoru instance formuláře `this(argument-list)` nebo `this()` způsobí, že ze třídy samotný má být volána konstruktor instance. Konstruktor je zaškrtnuto, pomocí *argument_list* -li k dispozici a pravidla rozlišení přetížení [rozlišení přetěžování](expressions.md#overload-resolution). Sada konstruktory instancí Release candidate zahrnuje všechny dostupné instance konstruktory deklarované v samotné třídě. Pokud tato sada je prázdný nebo jediný konstruktor instance nejlepší nelze identifikovat, dojde k chybě kompilace. Pokud deklarace konstruktoru instance obsahuje inicializátor konstruktoru, který volá konstruktor samotný, dojde k chybě kompilace.

Pokud konstruktor instance nemá žádný inicializátor konstruktoru, inicializátoru konstruktoru formuláře `base()` implicitně neposkytujeme. Díky tomu se instanci deklarace konstruktoru formuláře
```csharp
C(...) {...}
```
přesně odpovídá
```csharp
C(...): base() {...}
```

Obor parametry dána *formal_parameter_list* deklarace obsahuje konstruktor instance inicializátoru konstruktoru tohoto prohlášení. Inicializátor konstruktoru. proto je povolený přístup k parametrů volanému konstruktoru. Příklad:
```csharp
class A
{
    public A(int x, int y) {}
}

class B: A
{
    public B(int x, int y): base(x + y, x - y) {}
}
```

Inicializátor konstruktoru instance nelze získat přístup k instanci vytváří. Proto je chyba kompilace odkazovat `this` v argumentu výrazu inicializátoru konstruktoru, jako je chyba kompilace pro výraz argumentu k odkazování kterýkoli člen instance prostřednictvím *simple_name*.

### <a name="instance-variable-initializers"></a>Inicializátory proměnné instance

Pokud konstruktor instance nemá žádný inicializátor konstruktoru, nebo má inicializátoru konstruktoru formuláře `base(...)`, tento konstruktor provádí implicitně inicializace určené *variable_initializer*s pole instancí deklarované ve své třídě. To odpovídá sekvenci přiřazení, která jsou spuštěna ihned po vstupu do konstruktoru a před implicitní volání konstruktoru přímou základní třídu. Proměnné inicializátory jsou provedeny v textové pořadí, v jakém jsou uvedeny v deklaraci třídy.

### <a name="constructor-execution"></a>Provádění konstruktoru

Inicializátory proměnné se transformují na přiřazovací příkazy a tyto přiřazovací příkazy jsou spouštěny před vyvoláním konstruktoru instance základní třídy. Toto uspořádání zajistí, že všechna pole instancí jsou inicializovány jejich proměnné inicializátory předtím, než se spustí všechny příkazy, které mají přístup k této instanci.

Daný příklad
```csharp
using System;

class A
{
    public A() {
        PrintFields();
    }

    public virtual void PrintFields() {}
}

class B: A
{
    int x = 1;
    int y;

    public B() {
        y = -1;
    }

    public override void PrintFields() {
        Console.WriteLine("x = {0}, y = {1}", x, y);
    }
}
```
Když `new B()` slouží k vytvoření instance `B`, vytváří následující výstup:
```
x = 1, y = 0
```

Hodnota `x` je 1, protože proměnná inicializátor, je proveden před vyvoláním konstruktoru instance základní třídy. Však hodnoty `y` je 0 (výchozí hodnota `int`) protože přiřazení `y` není spuštěn až poté, co vrátí konstruktor základní třídy.

Je vhodné popřemýšlet instance proměnné inicializátory a inicializátory konstruktoru jako příkazy, které jsou před něj vloženo automaticky *constructor_body*. V příkladu
```csharp
using System;
using System.Collections;

class A
{
    int x = 1, y = -1, count;

    public A() {
        count = 0;
    }

    public A(int n) {
        count = n;
    }
}

class B: A
{
    double sqrt2 = Math.Sqrt(2.0);
    ArrayList items = new ArrayList(100);
    int max;

    public B(): this(100) {
        items.Add("default");
    }

    public B(int n): base(n - 1) {
        max = n;
    }
}
```
obsahuje několik proměnných inicializátory; také obsahuje konstruktor inicializátory obě formy (`base` a `this`). V příkladu odpovídá kódu je uvedeno níže, kde každý komentář označuje automaticky vložené – příkaz (syntaxi pro volání automaticky vložené konstruktoru není platný, ale slouží jenom ke znázornění mechanismu).

```csharp
using System.Collections;

class A
{
    int x, y, count;

    public A() {
        x = 1;                       // Variable initializer
        y = -1;                      // Variable initializer
        object();                    // Invoke object() constructor
        count = 0;
    }

    public A(int n) {
        x = 1;                       // Variable initializer
        y = -1;                      // Variable initializer
        object();                    // Invoke object() constructor
        count = n;
    }
}

class B: A
{
    double sqrt2;
    ArrayList items;
    int max;

    public B(): this(100) {
        B(100);                      // Invoke B(int) constructor
        items.Add("default");
    }

    public B(int n): base(n - 1) {
        sqrt2 = Math.Sqrt(2.0);      // Variable initializer
        items = new ArrayList(100);  // Variable initializer
        A(n - 1);                    // Invoke A(int) constructor
        max = n;
    }
}
```

### <a name="default-constructors"></a>Výchozí konstruktory

Pokud třída obsahuje žádné deklarace konstruktoru instance, se automaticky poskytuje výchozí konstruktor instance. Tento výchozí konstruktor jednoduše volá konstruktor základní třídy s přímým přístupem. Pokud je abstraktní třída je deklarovaná přístupnost pro výchozí konstruktor je chráněný. V opačném případě je deklarovaná přístupnost pro výchozí konstruktor je veřejný. Proto výchozí konstruktor je vždy formuláře

```csharp
protected C(): base() {}
```
or
```csharp
public C(): base() {}
```
kde `C` je název třídy. Pokud řešení přetížení není schopen určit jedinečné nejlepší kandidát pro inicializátoru konstruktoru základní třídy, dojde k chybě v době kompilace.

V příkladu
```csharp
class Message
{
    object sender;
    string text;
}
```
protože třída obsahuje žádné deklarace konstruktoru instance je k dispozici výchozí konstruktor. Proto je v příkladu přesně odpovídá
```csharp
class Message
{
    object sender;
    string text;

    public Message(): base() {}
}
```

### <a name="private-constructors"></a>Soukromé konstruktory

Pokud třída `T` deklaruje pouze privátní instanci konstruktory, není možné pro třídy mimo textem programu `T` odvodit z `T` nebo přímo vytvářet instance `T`. Proto pokud třída obsahuje pouze statické členy a není určen má být vytvořena, přidáním konstruktoru instance prázdný privátní brání vytváření instancí. Příklad:
```csharp
public class Trig
{
    private Trig() {}        // Prevent instantiation

    public const double PI = 3.14159265358979323846;

    public static double Sin(double x) {...}
    public static double Cos(double x) {...}
    public static double Tan(double x) {...}
}
```

`Trig` Třídy skupiny s ní související metody a konstanty, ale není by se měly vytvořit. Proto deklaruje jednu instanci privátní prázdný konstruktor. Potlačit automatické generování výchozího konstruktoru musí být deklarován nejméně jedna instance konstruktoru.

### <a name="optional-instance-constructor-parameters"></a>Parametry konstruktoru volitelné instance

`this(...)` Formu inicializátoru konstruktoru se běžně používá v společně s přetížení k implementaci instance volitelné parametry konstruktoru. V příkladu
```csharp
class Text
{
    public Text(): this(0, 0, null) {}

    public Text(int x, int y): this(x, y, null) {}

    public Text(int x, int y, string s) {
        // Actual constructor implementation
    }
}
```
první dvě instance konstruktory pouze zadat výchozí hodnoty pro chybějící argumenty. Používejte `this(...)` inicializátoru konstruktoru, který má být vyvolán třetí konstruktor instance, která ve skutečnosti provádí inicializuje novou instanci. Efekt je, volitelný konstruktor parametry:
```csharp
Text t1 = new Text();                    // Same as Text(0, 0, null)
Text t2 = new Text(5, 10);               // Same as Text(5, 10, null)
Text t3 = new Text(5, 20, "Hello");
```

## <a name="static-constructors"></a>Statické konstruktory

A ***statický konstruktor*** je člen, který implementuje akce potřebné k inicializaci typu uzavřené třídy. Statické konstruktory jsou deklarovány pomocí *static_constructor_declaration*s:

```antlr
static_constructor_declaration
    : attributes? static_constructor_modifiers identifier '(' ')' static_constructor_body
    ;

static_constructor_modifiers
    : 'extern'? 'static'
    | 'static' 'extern'?
    | static_constructor_modifiers_unsafe
    ;

static_constructor_body
    : block
    | ';'
    ;
```

A *static_constructor_declaration* může zahrnovat sadu *atributy* ([atributy](attributes.md)) a `extern` modifikátor ([externí metody](classes.md#external-methods)).

*Identifikátor* z *static_constructor_declaration* název musí být třída, ve kterém je deklarována statický konstruktor. Pokud je zadán žádným jiným názvem, dojde k chybě kompilace.

Pokud deklarace statického konstruktoru obsahuje `extern` modifikátor, statický konstruktor se říká, že ***externí statický konstruktor***. Protože deklaraci externí statický konstruktor poskytuje skutečné implementaci, jeho *static_constructor_body* skládá se ze středníku. Pro všechny ostatní deklarace statického konstruktoru *static_constructor_body* se skládá z *bloku* který určuje příkazy ke spuštění, aby bylo možné inicializovat třídu. To přesně odpovídá *method_body* statické metody s `void` návratový typ ([tělo metody](classes.md#method-body)).

Statické konstruktory nejsou děděna a nelze ji vyvolat přímo.

Statický konstruktor pro typ třídy uzavřené spouští nejvýše jednou v doméně aplikace. Spuštění statický konstruktor se aktivuje při prvním z následujících událostí v rámci domény aplikace:

*  Je vytvořena instance typu třídy.
*  Žádné statické členy typu třídy jsou odkazovány.

Pokud třída obsahuje `Main` – metoda ([spuštění aplikace](basic-concepts.md#application-startup)) v které provádění začne, statický konstruktor pro danou třídu provede před `Main` metoda je volána.

Inicializace nového typu třídy uzavřené, nejprve novou sadu statických polí ([statické a instance pole](classes.md#static-and-instance-fields)) pro daný typ uzavřené se vytvoří. Každé statické pole se inicializuje na výchozí hodnotu ([výchozí hodnoty](variables.md#default-values)). Další, statickými Inicializátory pole ([statické pole inicializace](classes.md#static-field-initialization)) jsou spouštěny pro tyto statická pole. Nakonec je spuštěn statický konstruktor.

V příkladu
```csharp
using System;

class Test
{
    static void Main() {
        A.F();
        B.F();
    }
}

class A
{
    static A() {
        Console.WriteLine("Init A");
    }
    public static void F() {
        Console.WriteLine("A.F");
    }
}

class B
{
    static B() {
        Console.WriteLine("Init B");
    }
    public static void F() {
        Console.WriteLine("B.F");
    }
}
```
musíte vytvořit výstup:
```
Init A
A.F
Init B
B.F
```
protože provádění `A`na statický konstruktor se aktivuje při volání `A.F`a provádění `B`na statický konstruktor se aktivuje při volání `B.F`.

Je možné k vytvoření cyklické závislosti, které umožňují statická pole s proměnnou inicializátory má být dodržen v jejich výchozí hodnoty stavu.

V příkladu
```csharp
using System;

class A
{
    public static int X;

    static A() {
        X = B.Y + 1;
    }
}

class B
{
    public static int Y = A.X + 1;

    static B() {}

    static void Main() {
        Console.WriteLine("X = {0}, Y = {1}", A.X, B.Y);
    }
}
```
Vytvoří výstup
```
X = 1, Y = 2
```

Ke spuštění `Main` , systém nejprve spuštěním metody inicializátor pro `B.Y`, před třídy `B`na statický konstruktor. `Y`pro inicializátor způsobí, že `A`na statický konstruktor, chcete-li spustit, protože hodnota `A.X` odkazuje. Statický konstruktor třídy `A` pak pokračuje k výpočtu hodnoty `X`a přitom tak načte výchozí hodnotu `Y`, což je nula. `A.X` Proto je inicializován na hodnotu 1. Proces spuštění `A`statickými Inicializátory pole a statický konstruktor pak dokončí, vrácení k výpočtu počáteční hodnota `Y`, výsledkem bude 2.

Protože statický konstruktor provádí právě jednou pro každou uzavřený konstruovaný třídy typu, je vhodné místo pro vynutit kontroly za běhu na parametr typu, který nelze zaregistrovat v době kompilace prostřednictvím omezení ([parametr typu omezení](classes.md#type-parameter-constraints)). Například následující typ používá statický konstruktor k vynucení, že je argument typu výčtu:
```csharp
class Gen<T> where T: struct
{
    static Gen() {
        if (!typeof(T).IsEnum) {
            throw new ArgumentException("T must be an enum");
        }
    }
}
```

## <a name="destructors"></a>Destruktory

A ***destruktor*** je člen, který implementuje akce potřebné k destrukci instance třídy. Destruktor je deklarován pomocí *destructor_declaration*:

```antlr
destructor_declaration
    : attributes? 'extern'? '~' identifier '(' ')' destructor_body
    | destructor_declaration_unsafe
    ;

destructor_body
    : block
    | ';'
    ;
```

A *destructor_declaration* může zahrnovat sadu *atributy* ([atributy](attributes.md)).

*Identifikátor* z *destructor_declaration* název musí být třída, ve kterém je deklarována destruktor. Pokud je zadán žádným jiným názvem, dojde k chybě kompilace.

Pokud deklarace destruktoru obsahuje `extern` modifikátor, destruktor se říká, že ***externí destruktor***. Protože deklaraci externí destruktor poskytuje skutečné implementaci, jeho *destructor_body* skládá se ze středníku. Pro všechny ostatní destruktory *destructor_body* se skládá z *bloku* který určuje příkazy provést, aby destrukce instance třídy. A *destructor_body* přesně odpovídá *method_body* metody instance s `void` návratový typ ([tělo metody](classes.md#method-body)).

Destruktory, nejsou děděna. Proto třída nemá žádné destruktory než ten, který může být deklarován v dané třídě.

Protože destruktor je nemusíte mít žádné parametry, nemůže být přetížená, takže třída může mít, maximálně jeden destruktor.

Destruktory jsou vyvolány automaticky a nelze ji vyvolat explicitně. Když už není možné použít tuto instanci žádný kód, bude vhodné pro odstranění instance. V okamžiku, jakmile bude instance nárok na odstranění může dojít k spuštění destruktoru pro instanci. Při destrukci instancí jsou volány destruktory v řetězu dědičnosti této instance, v pořadí od těch s nejvíce odvozené nejméně odvozený. Destruktor může být spuštěna v libovolném vlákně. Další informace o pravidlech, která určují, kdy a jak je spuštěn destruktor, naleznete v tématu [Automatická správa paměti](basic-concepts.md#automatic-memory-management).

Výstup v příkladu
```csharp
using System;

class A
{
    ~A() {
        Console.WriteLine("A's destructor");
    }
}

class B: A
{
    ~B() {
        Console.WriteLine("B's destructor");
    }
}

class Test
{
   static void Main() {
        B b = new B();
        b = null;
        GC.Collect();
        GC.WaitForPendingFinalizers();
   }
}
```
is
```
B's destructor
A's destructor
```
protože destruktory v řetěz dědičnosti jsou volány v pořadí od těch s nejvíce odvozené nejméně odvozený.

Destruktory jsou implementované přepisování virtuální metody `Finalize` na `System.Object`. Potlačí tuto metodu nebo volání (nebo jeho přepsání) nejsou povoleny programy jazyka C# přímo. Například program
```csharp
class A 
{
    override protected void Finalize() {}    // error

    public void F() {
        this.Finalize();                     // error
    }
}
```
obsahuje dva chyby.

Kompilátor se chová jako by tuto metodu a přepsání je vůbec neexistovat. Proto tento program:
```csharp
class A 
{
    void Finalize() {}                            // permitted
}
```
je platný, a skryje zobrazují metodu `System.Object`společnosti `Finalize` metody.

Informace o chování při z destruktoru je vyvolána výjimka, najdete v článku [způsob zpracování výjimek](exceptions.md#how-exceptions-are-handled).

## <a name="iterators"></a>Iterátory

Členské funkce ([funkce členy](expressions.md#function-members)) implementované pomocí blok iterátoru ([bloky](statements.md#blocks)) je volána ***iterátoru***.

Blok iterátoru, mohou být použity jako tělo funkce člena jako návratový typ odpovídající členské funkce je součástí rozhraní pro výčty ([rozhraní pro výčty](classes.md#enumerator-interfaces)) nebo jedna z vyčíslitelná rozhraní ([Vyčíslitelná rozhraní](classes.md#enumerable-interfaces)). To může nastat, jako *method_body*, *operator_body* nebo *accessor_body*, zatímco události, konstruktory instancí, statické konstruktory a destruktory nelze implementovat jako iterátory.

Je-li členské funkce je implementována pomocí blok iterátoru, je chyba kompilace pro seznam formálních parametrů členské funkce lze zadat `ref` nebo `out` parametry.

### <a name="enumerator-interfaces"></a>Rozhraní pro výčty

***Rozhraní pro výčty*** jsou obecné rozhraní `System.Collections.IEnumerator` a všech instancí obecného rozhraní `System.Collections.Generic.IEnumerator<T>`. Pro účely jako stručný výtah v této kapitole tato rozhraní jsou odkazovány jako `IEnumerator` a `IEnumerator<T>`v uvedeném pořadí.

### <a name="enumerable-interfaces"></a>Vyčíslitelná rozhraní

***Vyčíslitelná rozhraní*** jsou obecné rozhraní `System.Collections.IEnumerable` a všech instancí obecného rozhraní `System.Collections.Generic.IEnumerable<T>`. Pro účely jako stručný výtah v této kapitole tato rozhraní jsou odkazovány jako `IEnumerable` a `IEnumerable<T>`v uvedeném pořadí.

### <a name="yield-type"></a>Typ příkazu yield

Iterátor vytvoří posloupnost hodnot, všechny stejného typu. Tento typ se nazývá ***yield typ*** iterátoru.

*  Yield typ iterátoru, který vrací `IEnumerator` nebo `IEnumerable` je `object`.
*  Yield typ iterátoru, který vrací `IEnumerator<T>` nebo `IEnumerable<T>` je `T`.

### <a name="enumerator-objects"></a>Výčet objektů

Pokud člen funkce vrací enumerátor typ rozhraní je implementováno pomocí blok iterátoru, volání členské funkce nespustí ihned kód v bloku iterátoru. Místo toho ***objekt enumerator*** je vytvořena a vrácena. Tento objekt zapouzdřuje kód určený v bloku iterátoru a dojde k provádění kódu v bloku iterátoru při objekt enumerator `MoveNext` vyvolání metody. Objekt enumerátoru má následující vlastnosti:

*  Implementuje `IEnumerator` a `IEnumerator<T>`, kde `T` yield typu iterátoru.
*  Implementuje `System.IDisposable`.
*  Je inicializována s kopii hodnoty argumentů (pokud existuje) a byla předána hodnota instanci členské funkce.
*  Má čtyři možných stavů ***před***, ***systémem***, ***pozastaveno***, a ***po***a je zpočátku v ***před***  stavu.

Objekt enumerátoru je obvykle instance třídy generované kompilátorem enumerátor, který zapouzdřuje kód v bloku iterátoru a implementuje rozhraní čítače, ale jiné metody implementace jsou možné. Pokud třídu enumerátor je generovaný kompilátorem, se vnoří dané třídy, přímo nebo nepřímo, třídy obsahující členské funkce bude mít přístupnost private a bude mít název vyhrazený pro použití kompilátoru ([identifikátory ](lexical-structure.md#identifiers)).

Objekt enumerátoru může implementovat více rozhraní než uvedené výše.

Následující části popisují přesné chování `MoveNext`, `Current`, a `Dispose` členy `IEnumerable` a `IEnumerable<T>` rozhraní implementace poskytované objekt enumerátoru.

Všimněte si, že objekty enumerátor nepodporuje `IEnumerator.Reset` metody. Volání této metody způsobí, že `System.NotSupportedException` vyvolání.

#### <a name="the-movenext-method"></a>MoveNext – metoda

`MoveNext` Metoda objekt enumerátoru zapouzdřuje kód blok iterátoru. Volání `MoveNext` metody spustí kód v bloku iterátoru a nastaví `Current` vlastnost objekt enumerator podle potřeby. Přesné akce prováděné `MoveNext` závisí na stavu objekt enumerator při `MoveNext` vyvolání:

*  Pokud je stav objektu enumerátor ***před***, vyvolání `MoveNext`:
   * Změní stav, který má ***systémem***.
   * Inicializuje parametry (včetně `this`) na hodnoty argumentu a hodnotu instance uložily v době, kdy byl inicializován objekt enumerator bloku iterátoru.
   * Opakuje blok iterátoru od začátku, dokud se provádění dojde k přerušení (jak je popsáno níže).
*  Pokud státu objekt enumerator ***systémem***, výsledek vyvolání `MoveNext` není zadána.
*  Pokud je stav objektu enumerátor ***pozastaveno***, vyvolání `MoveNext`:
   * Změní stav, který má ***systémem***.
   * Obnoví hodnoty uloženy při provedení bloku iterátoru posledního pozastavení hodnoty všech místních proměnných a parametry (včetně to). Všimněte si, že obsah ze všech objektů, které odkazují tyto proměnné mohou změnily od předchozího volání k MoveNext.
   * Pokračuje v provádění bezprostředně po bloku iterátoru `yield return` příkaz, který způsobil pozastavení provádění a pokračuje, dokud provádění dojde k přerušení (jak je popsáno níže).
*  Pokud je stav objektu enumerátor ***po***, vyvolání `MoveNext` vrátí `false`.


Když `MoveNext` provede blok iterátoru, provedení může být přerušeno čtyři způsoby: Podle `yield return` příkaz, `yield break` prohlášení, prostřednictvím zjištění konce bloku iterátoru a výjimku vyvolána a rozšířena z bloku iterátoru.

*  Když `yield return` zjistil – příkaz ([příkaz yield](statements.md#the-yield-statement)):
   * Výraz zadaný v příkazu je vyhodnocen, implicitně převeden na typ yield a přiřazená `Current` vlastnost v objektu enumerátor.
   * Spuštění tělo iterátoru je pozastaveno. Hodnoty všech místních proměnných a parametry (včetně `this`) jsou uloženy, protože je to umístění `yield return` příkazu. Pokud `yield return` příkaz je v jedné nebo více `try` blokuje, přidružené `finally` v tuto chvíli nejsou provedeny bloky.
   * Stav enumerátoru objektu se změní na ***pozastaveno***.
   * `MoveNext` Vrátí metoda `true` volajícímu, označující, že byly úspěšně posunuty iterace na nejbližší hodnotu.
*  Když `yield break` zjistil – příkaz ([příkaz yield](statements.md#the-yield-statement)):
   * Pokud `yield break` příkaz je v jedné nebo více `try` blokuje, přidružené `finally` bloky provádějí.
   * Stav enumerátoru objektu se změní na ***po***.
   * `MoveNext` Vrátí metoda `false` volajícímu, označující, že je k dokončení iterace.
*  Pokud je nalezen konec tělo iterátoru:
   * Stav enumerátoru objektu se změní na ***po***.
   * `MoveNext` Vrátí metoda `false` volajícímu, označující, že je k dokončení iterace.
*  Když výjimka je výjimka vyvolána a rozšířena z bloku iterátoru:
   * Odpovídající `finally` bloky v tělo iterátoru bude proveden šíření výjimek.
   * Stav enumerátoru objektu se změní na ***po***.
   * Šíření výjimek pokračuje do volajícího `MoveNext` metody.

#### <a name="the-current-property"></a>Vlastnost Current

Objekt enumerátoru `Current` vlastnost je ovlivňován `yield return` příkazy v bloku iterátoru.

Pokud je objekt enumerátoru v ***pozastaveno*** stavu, hodnota `Current` se hodnoty nastavené v předchozím volání `MoveNext`. Pokud je objekt enumerátoru v ***před***, ***systémem***, nebo ***po*** stavy, výsledek přístup k `Current` není zadána.

Iterátor s výtěžností typu jiného než `object`, výsledek přístup k `Current` prostřednictvím objekt enumerator `IEnumerable` odpovídá přístup k implementaci `Current` prostřednictvím objekt enumerator `IEnumerator<T>` implementace a přetypování výsledek, který má `object`.

#### <a name="the-dispose-method"></a>Dispose – metoda

`Dispose` Metoda se používá k vyčištění iterace přeneste objekt enumerator do ***po*** stavu.

*  Pokud je stav objektu enumerátor ***před***, vyvolání `Dispose` změní stav, který má ***po***.
*  Pokud státu objekt enumerator ***systémem***, výsledek vyvolání `Dispose` není zadána.
*  Pokud je stav objektu enumerátor ***pozastaveno***, vyvolání `Dispose`:
   * Změní stav, který má ***systémem***.
   * Provede všechny nakonec bloky jakoby posledního spuštěného `yield return` příkaz byly `yield break` příkazu. Pokud to způsobí, že výjimka bude vyvolána a rozšíří mimo tělo iterátoru, stav objektu enumerátor je nastavený na ***po*** a výjimka se šíří do volajícího `Dispose` metody.
   * Změní stav, který má ***po***.
*  Pokud je stav objektu enumerátor ***po***, vyvolání `Dispose` nemá žádný vliv.

### <a name="enumerable-objects"></a>Vyčíslitelná objekty

Pokud člen funkce vracející typ vyčíslitelná rozhraní je implementováno pomocí blok iterátoru, volání členské funkce nespustí ihned kód v bloku iterátoru. Místo toho ***vyčíslitelný objekt*** je vytvořena a vrácena. Vyčíslitelný objekt `GetEnumerator` metoda vrací zadaný objekt enumerátoru, který zapouzdřuje kód v bloku iterátoru a dojde k provádění kódu v bloku iterátoru při objekt enumerator `MoveNext` vyvolání metody. Vyčíslitelný objekt má následující vlastnosti:

*  Implementuje `IEnumerable` a `IEnumerable<T>`, kde `T` yield typu iterátoru.
*  Je inicializována s kopii hodnoty argumentů (pokud existuje) a byla předána hodnota instanci členské funkce.

Vyčíslitelný objekt, je obvykle instance generované kompilátorem vyčíslitelné třídu, která zapouzdřuje kód v bloku iterátoru a implementuje vyčíslitelná rozhraní, ale jiné metody implementace jsou možné. Pokud vyčíslitelné třídy je generovaný kompilátorem, se vnoří dané třídy, přímo nebo nepřímo, třídy obsahující členské funkce bude mít přístupnost private a bude mít název vyhrazený pro použití kompilátoru ([identifikátory ](lexical-structure.md#identifiers)).

Vyčíslitelný objekt může implementovat více rozhraní než uvedené výše. Zejména může také implementovat vyčíslitelný objekt `IEnumerator` a `IEnumerator<T>`, díky tomu se může sloužit jako Výčtový objekt a enumerátor. V tomto typu implementace nevyskytne první vyčíslitelný objekt `GetEnumerator` je vyvolána metoda, je vrácena sám vyčíslitelným objektem. Následné volání vyčíslitelný objekt `GetEnumerator`, pokud existuje, vrátí kopii vyčíslitelným objektem. Díky tomu se každá vrácená čítače mají svůj vlastní stav a změny v jedné enumerátor nebude mít vliv na jiné.

#### <a name="the-getenumerator-method"></a>GetEnumerator – metoda

Vyčíslitelný objekt poskytuje implementaci `GetEnumerator` metody `IEnumerable` a `IEnumerable<T>` rozhraní. Dva `GetEnumerator` metody sdílejí běžnou implementaci, která získá a vrátí objekt k dispozici enumerátoru. Objekt enumerator inicializovat pomocí hodnoty argumentů a instanci hodnotu uložily v době, kdy vyčíslitelný objekt byl inicializován, ale jinak funkce objekt enumerátoru, jak je popsáno v [enumerátor objekty](classes.md#enumerator-objects).

### <a name="implementation-example"></a>Příklad implementace

Tato část popisuje možnou implementaci iterátorů, co se týče standardní konstrukce jazyka C#. Implementace je zde popsáno, je založená na stejné zásady použít kompilátor jazyka Microsoft C#, ale v žádném smyslu mandátem implementace nebo pouze jeden možný.

Následující `Stack<T>` implementuje třída jeho `GetEnumerator` metodu pomocí iterátoru. Iterátor uvádí prvky zásobníku v horní části pořadí.

```csharp
using System;
using System.Collections;
using System.Collections.Generic;

class Stack<T>: IEnumerable<T>
{
    T[] items;
    int count;

    public void Push(T item) {
        if (items == null) {
            items = new T[4];
        }
        else if (items.Length == count) {
            T[] newItems = new T[count * 2];
            Array.Copy(items, 0, newItems, 0, count);
            items = newItems;
        }
        items[count++] = item;
    }

    public T Pop() {
        T result = items[--count];
        items[count] = default(T);
        return result;
    }

    public IEnumerator<T> GetEnumerator() {
        for (int i = count - 1; i >= 0; --i) yield return items[i];
    }
}
```

`GetEnumerator` Metody lze přeložit do instance třídy generované kompilátorem enumerátor, který zapouzdřuje kód v bloku iterátoru, jak je znázorněno v následujícím.

```csharp
class Stack<T>: IEnumerable<T>
{
    ...

    public IEnumerator<T> GetEnumerator() {
        return new __Enumerator1(this);
    }

    class __Enumerator1: IEnumerator<T>, IEnumerator
    {
        int __state;
        T __current;
        Stack<T> __this;
        int i;

        public __Enumerator1(Stack<T> __this) {
            this.__this = __this;
        }

        public T Current {
            get { return __current; }
        }

        object IEnumerator.Current {
            get { return __current; }
        }

        public bool MoveNext() {
            switch (__state) {
                case 1: goto __state1;
                case 2: goto __state2;
            }
            i = __this.count - 1;
        __loop:
            if (i < 0) goto __state2;
            __current = __this.items[i];
            __state = 1;
            return true;
        __state1:
            --i;
            goto __loop;
        __state2:
            __state = 2;
            return false;
        }

        public void Dispose() {
            __state = 2;
        }

        void IEnumerator.Reset() {
            throw new NotSupportedException();
        }
    }
}
```

V předchozím překladu kód v bloku iterátoru převedena na stavu počítače a umístí do `MoveNext` metoda třídy výčtu. Kromě toho místní proměnná `i` bude převedena na pole v objekt enumerator, takže můžete dál existovat mezi voláními `MoveNext`.

Následující příklad vytiskne jednoduchou tabulku násobení celých čísel 1 až 10. `FromTo` Metoda v tomto příkladu vrácen vyčíslitelný objekt a je implementována pomocí iterátoru.

```csharp
using System;
using System.Collections.Generic;

class Test
{
    static IEnumerable<int> FromTo(int from, int to) {
        while (from <= to) yield return from++;
    }

    static void Main() {
        IEnumerable<int> e = FromTo(1, 10);
        foreach (int x in e) {
            foreach (int y in e) {
                Console.Write("{0,3} ", x * y);
            }
            Console.WriteLine();
        }
    }
}
```

`FromTo` Metody lze přeložit do instance třídy generované kompilátorem vyčíslitelné, který zapouzdřuje kód v bloku iterátoru, jak je znázorněno v následujícím.

```csharp
using System;
using System.Threading;
using System.Collections;
using System.Collections.Generic;

class Test
{
    ...

    static IEnumerable<int> FromTo(int from, int to) {
        return new __Enumerable1(from, to);
    }

    class __Enumerable1:
        IEnumerable<int>, IEnumerable,
        IEnumerator<int>, IEnumerator
    {
        int __state;
        int __current;
        int __from;
        int from;
        int to;
        int i;

        public __Enumerable1(int __from, int to) {
            this.__from = __from;
            this.to = to;
        }

        public IEnumerator<int> GetEnumerator() {
            __Enumerable1 result = this;
            if (Interlocked.CompareExchange(ref __state, 1, 0) != 0) {
                result = new __Enumerable1(__from, to);
                result.__state = 1;
            }
            result.from = result.__from;
            return result;
        }

        IEnumerator IEnumerable.GetEnumerator() {
            return (IEnumerator)GetEnumerator();
        }

        public int Current {
            get { return __current; }
        }

        object IEnumerator.Current {
            get { return __current; }
        }

        public bool MoveNext() {
            switch (__state) {
            case 1:
                if (from > to) goto case 2;
                __current = from++;
                __state = 1;
                return true;
            case 2:
                __state = 2;
                return false;
            default:
                throw new InvalidOperationException();
            }
        }

        public void Dispose() {
            __state = 2;
        }

        void IEnumerator.Reset() {
            throw new NotSupportedException();
        }
    }
}
```

Vyčíslitelná třída implementuje vyčíslitelná rozhraní a rozhraní čítače, díky tomu se může sloužit jako Výčtový objekt a enumerátor. Prvním `GetEnumerator` je vyvolána metoda, je vrácena sám vyčíslitelným objektem. Následné volání vyčíslitelný objekt `GetEnumerator`, pokud existuje, vrátí kopii vyčíslitelným objektem. Díky tomu se každá vrácená čítače mají svůj vlastní stav a změny v jedné enumerátor nebude mít vliv na jiné. `Interlocked.CompareExchange` Metoda se používá k zajištění operace bezpečné pro vlákna.

`from` a `to` parametry jsou převedena na pole v třídě vyčíslitelný. Protože `from` se mění v bloku iterátoru, zobrazí se další `__from` pole se používá k uchování na počáteční hodnoty `from` v každé enumerátor.

`MoveNext` Vyvolá metoda výjimku `InvalidOperationException` Pokud je volána, když `__state` je `0`. Je to ochrana proti použití vyčíslitelný objekt jako objekt enumerátoru bez první volání `GetEnumerator`.

Následující příklad ukazuje jednoduchý stromu tříd. `Tree<T>` Implementuje třída jeho `GetEnumerator` metodu pomocí iterátoru. Iterátor vytvoří výčet elementy stromu v pořadí infix.

```csharp
using System;
using System.Collections.Generic;

class Tree<T>: IEnumerable<T>
{
    T value;
    Tree<T> left;
    Tree<T> right;

    public Tree(T value, Tree<T> left, Tree<T> right) {
        this.value = value;
        this.left = left;
        this.right = right;
    }

    public IEnumerator<T> GetEnumerator() {
        if (left != null) foreach (T x in left) yield x;
        yield value;
        if (right != null) foreach (T x in right) yield x;
    }
}

class Program
{
    static Tree<T> MakeTree<T>(T[] items, int left, int right) {
        if (left > right) return null;
        int i = (left + right) / 2;
        return new Tree<T>(items[i], 
            MakeTree(items, left, i - 1),
            MakeTree(items, i + 1, right));
    }

    static Tree<T> MakeTree<T>(params T[] items) {
        return MakeTree(items, 0, items.Length - 1);
    }

    // The output of the program is:
    // 1 2 3 4 5 6 7 8 9
    // Mon Tue Wed Thu Fri Sat Sun

    static void Main() {
        Tree<int> ints = MakeTree(1, 2, 3, 4, 5, 6, 7, 8, 9);
        foreach (int i in ints) Console.Write("{0} ", i);
        Console.WriteLine();

        Tree<string> strings = MakeTree(
            "Mon", "Tue", "Wed", "Thu", "Fri", "Sat", "Sun");
        foreach (string s in strings) Console.Write("{0} ", s);
        Console.WriteLine();
    }
}
```

`GetEnumerator` Metody lze přeložit do instance třídy generované kompilátorem enumerátor, který zapouzdřuje kód v bloku iterátoru, jak je znázorněno v následujícím.

```csharp
class Tree<T>: IEnumerable<T>
{
    ...

    public IEnumerator<T> GetEnumerator() {
        return new __Enumerator1(this);
    }

    class __Enumerator1 : IEnumerator<T>, IEnumerator
    {
        Node<T> __this;
        IEnumerator<T> __left, __right;
        int __state;
        T __current;

        public __Enumerator1(Node<T> __this) {
            this.__this = __this;
        }

        public T Current {
            get { return __current; }
        }

        object IEnumerator.Current {
            get { return __current; }
        }

        public bool MoveNext() {
            try {
                switch (__state) {

                case 0:
                    __state = -1;
                    if (__this.left == null) goto __yield_value;
                    __left = __this.left.GetEnumerator();
                    goto case 1;

                case 1:
                    __state = -2;
                    if (!__left.MoveNext()) goto __left_dispose;
                    __current = __left.Current;
                    __state = 1;
                    return true;

                __left_dispose:
                    __state = -1;
                    __left.Dispose();

                __yield_value:
                    __current = __this.value;
                    __state = 2;
                    return true;

                case 2:
                    __state = -1;
                    if (__this.right == null) goto __end;
                    __right = __this.right.GetEnumerator();
                    goto case 3;

                case 3:
                    __state = -3;
                    if (!__right.MoveNext()) goto __right_dispose;
                    __current = __right.Current;
                    __state = 3;
                    return true;

                __right_dispose:
                    __state = -1;
                    __right.Dispose();

                __end:
                    __state = 4;
                    break;

                }
            }
            finally {
                if (__state < 0) Dispose();
            }
            return false;
        }

        public void Dispose() {
            try {
                switch (__state) {

                case 1:
                case -2:
                    __left.Dispose();
                    break;

                case 3:
                case -3:
                    __right.Dispose();
                    break;

                }
            }
            finally {
                __state = 4;
            }
        }

        void IEnumerator.Reset() {
            throw new NotSupportedException();
        }
    }
}
```

Dočasné objekty generovaný kompilátorem, používané `foreach` příkazy jsou zrušeno vs. do `__left` a `__right` polí objekt enumerátoru. `__state` Pole objektu enumerátor se pečlivě aktualizuje tak, aby správné `Dispose()` metoda volána správně, pokud je vyvolána výjimka. Všimněte si, že není možné psát přeložený kód v jednoduchém `foreach` příkazy.

## <a name="async-functions"></a>Asynchronní funkce

Metody ([metody](classes.md#methods)) nebo anonymní funkce ([výrazy anonymní funkce](expressions.md#anonymous-function-expressions)) s `async` Modifikátor je volána ***asynchronní funkci***. Obecně platí termín ***asynchronní*** se používá k popisu jakýkoli druh funkce, která má `async` modifikátor.

Chyba kompilace pro asynchronní funkce lze zadat seznam formálních parametrů je `ref` nebo `out` parametry.

*Typ* asynchronní metody musí být buď `void` nebo ***typ úkolu***. Typy úloh jsou `System.Threading.Tasks.Task` a typy vytvářejí na základě `System.Threading.Tasks.Task<T>`. Pro účely jako stručný výtah v této kapitole tyto typy jsou odkazovány jako `Task` a `Task<T>`v uvedeném pořadí. Asynchronní metody vracející typ úloh se říká, že vracejí task.

Přesné definice typů úloh je definován implementací, ale z hlediska tohoto jazyka je typ úkolu v jednom ze stavů nekompletní, bylo úspěšné nebo došlo k chybě. Úlohu chybnou zaznamenává relevantní výjimka. Úspěšném `Task<T>` zaznamenává výsledek typu `T`. Typy úloh jsou očekávatelný a může proto být operandy výrazech await ([výrazech Await](expressions.md#await-expressions)).

Asynchronní vyvolání funkce má schopnost pozastavit vyhodnocení prostřednictvím výrazech await ([výrazech Await](expressions.md#await-expressions)) v jeho textu. Vyhodnocení může později obnovit místě pozastavení operátor await výraz prostřednictvím ***delegát pokračování***. Obnovení delegáta je typu `System.Action`, a když je vyvolán, vyhodnocení volání asynchronní funkce bude pokračovat z výrazu await, kde přestalo. ***Aktuální volající*** asynchronní funkce volání je původní volajícího, pokud volání funkce nikdy byl pozastaven nebo nejnovější volající delegát pokračování jinak.

### <a name="evaluation-of-a-task-returning-async-function"></a>Hodnocení produktu asynchronní funkci vracející úlohy

Volání asynchronní funkci vracející úlohy způsobí, že instance typu vrácené úlohy budou vygenerovány. Tento postup se nazývá ***návratová hodnota task*** asynchronní funkce. Úloha je zpočátku nekompletnímu stavu.

Tělo funkce asynchronní je pak vyhodnocen, dokud je buď pozastaveno (díky tomu, že výraz await) nebo ukončí, ve kterém ovládací prvek bod se vrátí volajícímu spolu návratová hodnota task.

Při ukončení těla asynchronní funkce, vrácené úlohy je vyřadí z nekompletnímu stavu:

*  Pokud tělo funkce končí v důsledku dosáhnout návratový příkaz nebo konec textu, libovolná hodnota výsledku je zaznamenán v vrácené úlohou, které přejde do stavu úspěšné.
*  Pokud tělo funkce končí v důsledku vydá nezachycenou výjimku ([příkazu throw](statements.md#the-throw-statement)) výjimky je zaznamenán v vrácené úlohou, které je umístěn v chybovém stavu.

### <a name="evaluation-of-a-void-returning-async-function"></a>Hodnocení produktu asynchronní funkci vracející typ void

Pokud je návratový typ asynchronní funkce `void`, vyhodnocení se liší od výše uvedených následujícím způsobem: Protože nevrátí žádná úloha, funkci místo toho komunikuje dokončení a výjimky pro aktuální vlákno ***kontext synchronizace***. Přesné definici kontext synchronizace je závislý na implementaci, ale je reprezentace "kde" spuštění aktuálního vlákna. Kontext synchronizace zasláno oznámení, když vyhodnocení asynchronní funkci vracející hodnotu void začíná, dokončíte úspěšně nebo způsobí, že vydá nezachycenou výjimku, která je vyvolána.

To umožňuje sledovat, kolik asynchronní funkce vracející hodnotu void běží pod ním a rozhodování o způsobu šíření výjimky z nich kontextu.
