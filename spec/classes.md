---
ms.openlocfilehash: e0def754174ab8646f9b849abe86d2c375c958b6
ms.sourcegitcommit: 892af9016b3317a8fae12d195014dc38ba51cf16
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/01/2019
ms.locfileid: "71703986"
---
# <a name="classes"></a>Třídy

Třída je datová struktura, která může obsahovat datové členy (konstanty a pole), členy funkce (metody, vlastnosti, události, indexery, operátory, konstruktory instancí, destruktory a statické konstruktory) a vnořené typy. Typy tříd podporují dědičnost, mechanismus, který může odvozená třída zvětšit a specializovat základní třídu.

## <a name="class-declarations"></a>Deklarace tříd

*Class_declaration* je *type_declaration* ([deklarace typu](namespaces.md#type-declarations)), který deklaruje novou třídu.

```antlr
class_declaration
    : attributes? class_modifier* 'partial'? 'class' identifier type_parameter_list?
      class_base? type_parameter_constraints_clause* class_body ';'?
    ;
```

*Class_declaration* se skládá z volitelné sady *atributů* ([atributů](attributes.md)) následovaný volitelnou sadou *class_modifier*s ([modifikátory třídy](classes.md#class-modifiers)) následovaným volitelným modifikátorem `partial` následovaným klíčovým slovem. `class` a *identifikátor* , který vyjmenovává třídu následovaný nepovinným *type_parameter_list* ([parametry typu](classes.md#type-parameters)) následovaný nepovinnou specifikací *class_base* ([základní specifikace třídy](classes.md#class-base-specification)) následovaný volitelná sada *type_parameter_constraints_clause*s ([omezení parametrů typu](classes.md#type-parameter-constraints)) následovaná *class_body* ([tělo třídy](classes.md#class-body)), volitelně následovaný středníkem.

Deklarace třídy nemůže poskytovat *type_parameter_constraints_clause*s, pokud zároveň neposkytuje *type_parameter_list*.

Deklarace třídy, která poskytuje *type_parameter_list* , je ***Obecná deklarace třídy***. Kromě toho jakákoli třída vnořená uvnitř deklarace obecné třídy nebo deklarace obecné struktury je sám deklarací obecné třídy, protože k vytvoření vytvořeného typu musí být zadány parametry typu pro nadřazený typ.

### <a name="class-modifiers"></a>Modifikátory třídy

*Class_declaration* může volitelně zahrnovat posloupnost modifikátorů třídy:

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

Jedná se o chybu při kompilaci, aby se stejný modifikátor zobrazoval víckrát v deklaraci třídy.

`new` Modifikátor je povolen pro vnořené třídy. Určuje, že Třída skrývá zděděného člena se stejným názvem, jak je popsáno v [novém modifikátoru](classes.md#the-new-modifier). Jedná se o chybu `new` při kompilaci, aby se modifikátor zobrazoval v deklaraci třídy, která není vnořená deklarace třídy.

Modifikátory `protected` ,,`internal` a`private`řídípřístupnosttřídy. `public` V závislosti na kontextu, ve kterém se vyskytuje deklarace třídy, nemusí být některé z těchto modifikátorů povoleny ([deklarovaná přístupnost](basic-concepts.md#declared-accessibility)).

Modifikátory a`static`jsou popsány v následujících částech. `sealed` `abstract`

#### <a name="abstract-classes"></a>Abstraktní třídy

`abstract` Modifikátor slouží k označení toho, že třída je nekompletní a že je určena pro použití pouze jako základní třída. Abstraktní třída se od neabstraktní třídy liší následujícími způsoby:

*  Nelze vytvořit instanci abstraktní třídy přímo a jedná se o chybu při kompilaci pro použití `new` operátoru pro abstraktní třídu. I když je možné mít proměnné a hodnoty, jejichž typy kompilace jsou abstraktní, takové proměnné a hodnoty budou nutně buď `null` nebo obsahovat odkazy na instance neabstraktních tříd odvozených z abstraktních typů.
*  Abstraktní třída je povolena (ale není vyžadována), aby obsahovala abstraktní členy.
*  Abstraktní třída nemůže být zapečetěná.

Pokud je Neabstraktní třída odvozena z abstraktní třídy, Neabstraktní třída musí zahrnovat skutečné implementace všech zděděných abstraktních členů, čímž se přepsaly tyto abstraktní členy. V příkladu
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
abstraktní třída `A` zavádí abstraktní metodu `F`. Třída `B` zavádí další metodu `G`, ale vzhledem k tomu, že neposkytuje implementaci `F`, `B` musí být také deklarována jako abstraktní. `C` Přepisuje`F` třídu a poskytuje skutečnou implementaci. Vzhledem k tomu, že v `C`, nejsou k dispozici žádní abstraktní členové, `C` je povolen (ale není vyžadován) pro neabstraktní.

#### <a name="sealed-classes"></a>Zapečetěné třídy

`sealed` Modifikátor slouží k zabránění odvození z třídy. K chybě při kompilaci dojde, pokud je zapečetěná třída zadána jako základní třída jiné třídy.

Zapečetěná třída nemůže být také abstraktní třída.

`sealed` Modifikátor se používá hlavně k zabránění nezamýšlenému odvození, ale také umožňuje určité optimalizace za běhu. Konkrétně vzhledem k tomu, že zapečetěná třída je známa tak, že nikdy nemá žádné odvozené třídy, je možné transformovat vyvolání členů virtuální funkce na zapečetěných instancích třídy na nevirtuální vyvolání.

#### <a name="static-classes"></a>Statické třídy

Modifikátor slouží k označení třídy deklarované jako ***statické třídy.*** `static` Nelze vytvořit instanci statické třídy, nelze ji použít jako typ a může obsahovat pouze statické členy. Pouze statická třída může obsahovat deklarace rozšiřujících metod ([metod rozšíření](classes.md#extension-methods)).

Deklarace statické třídy podléhá následujícím omezením:

*  Statická třída nesmí obsahovat `sealed` modifikátor or. `abstract` Upozorňujeme však, že vzhledem k tomu, že statická třída nemůže být vytvořena nebo odvozena z, se chová, jako by byla zapečetěná i abstraktní.
*  Statická třída nesmí obsahovat specifikaci *class_base* ([základní specifikace třídy](classes.md#class-base-specification)) a nemůže explicitně zadat základní třídu nebo seznam implementovaných rozhraní. Statická třída implicitně dědí z typu `object`.
*  Statická třída může obsahovat pouze statické členy ([statické a členy instance](classes.md#static-and-instance-members)). Všimněte si, že konstanty a vnořené typy jsou klasifikovány jako statické členy.
*  Statická třída nemůže mít členy s `protected` nebo `protected internal` deklarovanou přístupností.

Jedná se o chybu při kompilaci, která porušuje některá z těchto omezení.

Statická třída nemá žádné konstruktory instancí. Není možné deklarovat konstruktor instance ve statické třídě a není k dispozici žádný výchozí konstruktor instance ([výchozí konstruktory](classes.md#default-constructors)) pro statickou třídu.

Členové statické třídy nejsou automaticky statický a deklarace členů musí explicitně obsahovat `static` modifikátor (s výjimkou konstant a vnořených typů). Pokud je třída vnořena do statické vnější třídy, vnořená třída není statickou třídou, pokud explicitně neobsahuje `static` modifikátor.

__Odkazování na typy statických tříd__

*Namespace_or_type_name* ([obor názvů a názvy typů](basic-concepts.md#namespace-and-type-names)) je povolený odkazování na statickou třídu, pokud

*  *Namespace_or_type_name* je `T` ve *namespace_or_type_name* ve formě `T.I` nebo
*  *Namespace_or_type_name* je `T` v *typeof_expression* ([seznam argumentů](expressions.md#argument-lists)1) formuláře `typeof(T)`.

*Primary_expression* ([Členové funkce](expressions.md#function-members)) mají povolen odkaz na statickou třídu, pokud

*  *Primary_expression* je `E` v *member_access* ([Kontrola doby kompilace dynamického přetěžování](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) formuláře `E.I`.

V jakémkoli jiném kontextu se jedná o chybu při kompilaci, která odkazuje na statickou třídu. Například se jedná o chybu pro statickou třídu, která bude použita jako základní třída, typ prvku ([vnořené typy](classes.md#nested-types)) člena, argument obecného typu nebo omezení parametru typu. Stejně tak statická třída nemůže být použita v typu pole, typu ukazatele `new` , výrazu, výrazu přetypování `is` , výrazu `sizeof` , `as` výrazu, výrazu nebo výrazu výchozí hodnoty.

### <a name="partial-modifier"></a>Částečný modifikátor

Modifikátor `partial` slouží k označení, že toto *class_declaration* je částečná deklarace typu. Vícenásobná deklarace částečného typu se stejným názvem v rámci ohraničujícího oboru názvů nebo deklarace typu jsou zkombinovány o jednu deklaraci typu podle pravidel zadaných v [částečných typech](classes.md#partial-types).

Deklarace třídy distribuované přes samostatné segmenty textu programu může být užitečná, pokud jsou tyto segmenty vyráběny nebo udržovány v různých kontextech. Například jedna část deklarace třídy může být vygenerována počítačem, zatímco druhá je ručně vytvořen. Textové oddělení těchto dvou brání aktualizacím, které jsou v konfliktu s aktualizacemi.

### <a name="type-parameters"></a>Parametry typu

Parametr typu je jednoduchý identifikátor, který označuje zástupný symbol pro zadaný argument typu pro vytvoření konstruovaného typu. Parametr typu je formální zástupný symbol pro typ, který bude dodán později. Naopak argument typu ([argumenty typu](types.md#type-arguments)) je skutečný typ, který je nahrazen parametrem typu při vytvoření vytvořeného typu.

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

Každý parametr typu v deklaraci třídy definuje název v prostoru deklarací ([deklarace](basic-concepts.md#declarations)) této třídy. Proto nemůže mít stejný název jako jiný parametr typu nebo člen deklarovaný v této třídě. Parametr typu nemůže mít stejný název jako samotný typ.

### <a name="class-base-specification"></a>Základní specifikace třídy

Deklarace třídy může obsahovat specifikaci *class_base* , která definuje přímou základní třídu třídy a rozhraní ([rozhraní](interfaces.md)) přímo implementované třídou.

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

Základní třída zadaná v deklaraci třídy může být typ konstruované třídy ([vytvořené typy](types.md#constructed-types)). Základní třída nemůže být parametrem typu sama o sobě, i když může zahrnovat parametry typu, které jsou v oboru.

```csharp
class Extend<V>: V {}            // Error, type parameter used as base class
```

#### <a name="base-classes"></a>Základní třídy

Pokud je *class_type* obsažen v *class_base*, určuje přímou základní třídu deklarované třídy. Pokud deklarace třídy nemá *class_base*nebo pokud *class_base* uvádí pouze typy rozhraní, předpokládá se přímá základní třída `object`. Třída dědí členy ze své přímé základní třídy, jak je popsáno v tématu [Dědičnost](classes.md#inheritance).

V příkladu
```csharp
class A {}

class B: A {}
```
Třída `A` je označována jako přímá základní `B`třída a `B` je označována jako odvozena z `A`. Vzhledem `A` k tomu, že explicitně nespecifikuje přímou základní třídu, její Přímá základní třída `object`je implicitně.

V případě konstruovaného typu třídy, pokud je v deklaraci obecné třídy specifikována základní třída, je základní třída konstruovaného typu získána nahrazením, pro každý *type_parameter* v deklaraci základní třídy, odpovídající *type_argument* z konstruovaného typu. S ohledem na deklarace obecných tříd
```csharp
class B<U,V> {...}

class G<T>: B<string,T[]> {...}
```
základní třída konstruovaného typu `G<int>` by byla. `B<string,int[]>`

Přímá základní třída typu třídy musí být alespoň dostupná jako typ samotné třídy ([domény pro usnadnění](basic-concepts.md#accessibility-domains)). Například se jedná o chybu `public` při kompilaci třídy, která má být odvozena `private` z třídy nebo `internal` .

Přímá základní třída typu třídy nesmí být žádné z následujících typů: `System.Array`, `System.Delegate`, `System.MulticastDelegate`, `System.Enum`nebo `System.ValueType`. Kromě toho deklarace obecné třídy nemůže používat `System.Attribute` jako přímou nebo nepřímou základní třídu.

Při určování významu přímé základní třídy `A` specifikace třídy `B`se dočasně předpokládá, `B` že přímá základní třída je dočasně považována `object`za. Intuitivní to zajistí, že význam specifikace základní třídy nemůže rekurzivně záviset sám na sobě. Příklad:
```csharp
class A<T> {
   public class B {}
}

class C : A<C.B> {}
```
`A<C.B>` došlo k chybě `object`, protože v základní třídě `C` je určena Přímá základní třída, která je považována za, a proto (podle pravidel [názvů a názvů typů](basic-concepts.md#namespace-and-type-names)) `C` není považována za člena. `B`.

Základní třídy typu třídy jsou přímá základní třída a její základní třídy. Jinými slovy, sada základních tříd je přenosný uzávěr vztahu přímé základní třídy. Odkazování na výše uvedený příklad jsou `B` `A` základní třídy pro a `object`. V příkladu
```csharp
class A {...}

class B<T>: A {...}

class C<T>: B<IComparable<T>> {...}

class D<T>: C<T[]> {...}
```
základní `D<int>` třídy pro jsou `B<IComparable<int[]>>` ,,`A`a .`object` `C<int[]>`

S výjimkou `object`třídy každý typ třídy obsahuje přesně jednu přímou základní třídu. `object` Třída nemá žádnou přímou základní třídu a jedná se o konečnou základní třídu všech ostatních tříd.

Je-li `B` Třída odvozena z třídy `A`, jedná se o chybu při `A` kompilaci, která závisí na `B`. Třída ***přímo závisí na*** své přímé základní třídě (pokud existuje) a ***přímo závisí na*** třídě, ve které je okamžitě vnořena (pokud existuje). V rámci této definice je kompletní sada tříd, na kterých je třída závislá, reflexivní a přenosný uzávěr ***přímo závislá na*** vztahu.

Příklad
```csharp
class A: A {}
```
je chybné, protože třída závisí sám na sobě. Podobně příklad
```csharp
class A: B {}
class B: C {}
class C: A {}
```
došlo k chybě, protože třídy jsou cyklicky závislé. Nakonec příklad
```csharp
class A: B.C {}

class B: A
{
    public class C {}
}
```
má za následek chybu při kompilaci, protože `A` závisí na `B.C` (její přímé základní třídě), která závisí na `B` (její bezprostředně ohraničující třídu), která je cyklicky závislá na `A`.

Všimněte si, že třída nezávisí na třídách, které jsou v ní vnořené. V příkladu
```csharp
class A
{
    class B: A {}
}
```
`B`závisí na `A` (protože `A` je to přímá základní třída a její bezprostředně ohraničující třída), ale `A` nezávisí na `B` (protože `B` není ani základní ani třída nadřazené třídy. `A`). Proto je příklad platný.

Není možné odvozovat od `sealed` třídy. V příkladu
```csharp
sealed class A {}

class B: A {}            // Error, cannot derive from a sealed class
```
Třída `B` je v chybě, protože se pokouší odvozovat `sealed` od třídy `A`.

#### <a name="interface-implementations"></a>Implementace rozhraní

Specifikace *class_base* může obsahovat seznam typů rozhraní. v takovém případě třída je označována jako přímá implementace daných typů rozhraní. Implementace rozhraní jsou podrobněji popsány v [implementacích rozhraní](interfaces.md#interface-implementations).

### <a name="type-parameter-constraints"></a>Omezení parametrů typu

Deklarace obecného typu a metody mohou volitelně určovat omezení parametrů typu zahrnutím *type_parameter_constraints_clause*s.

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

Každý *type_parameter_constraints_clause* se skládá z tokenu `where`, za nímž následuje název parametru typu, následovaný dvojtečkou a seznamem omezení pro tento parametr typu. Pro každý parametr typu může existovat `where` maximálně jedna klauzule `where` a klauzule mohou být uvedeny v libovolném pořadí. Podobně jako tokeny `set` `where` a v přistupujícím objektu vlastnosti token není klíčové slovo. `get`

Seznam omezení uvedených v `where` klauzuli může zahrnovat kteroukoli z následujících součástí v tomto pořadí: jedno primární omezení, jedno nebo více sekundárních omezení a `new()`omezení konstruktoru.

`class` Primárním omezením může být typ třídy nebo ***omezení typu odkazu*** nebo ***omezení*** `struct`typu hodnoty. Sekundární omezení může být *type_parameter* nebo *INTERFACE_TYPE*.

Omezení typu odkazu určuje, že argument typu použitý pro parametr typu musí být odkazový typ. Toto omezení neplatí pro všechny typy tříd, typy rozhraní, typy delegátů, typy polí a parametry typu, které jsou známé jako typ odkazu (jak je definováno níže).

Omezení typu hodnoty určuje, že argument typu použitý pro parametr typu musí být typ hodnoty, která není null. Toto omezení neodpovídají všem typům struktury, které nejsou null, výčtové typy a parametry typu s omezením typu hodnoty. Všimněte si, že i když typ hodnoty, typ s povolenou hodnotou null ([typy s možnou hodnotou null](types.md#nullable-types)) nevyhovuje omezení hodnotového typu. Parametr typu s omezením typu hodnoty nemůže mít také *constructor_constraint*.

Typy ukazatelů nikdy nemůžou být argumenty typu a nepovažují se za nevyhovující buď omezením typu odkazu nebo typu hodnoty.

Pokud je omezení typ třídy, typ rozhraní nebo parametr typu, tento typ Určuje minimální "základní typ", který každý argument typu použitý pro tento parametr typu musí podporovat. Vždy, když je použit konstruovaný typ nebo obecná metoda, je argument typu zkontrolován proti omezením parametru typu v době kompilace. Zadaný argument typu musí splňovat podmínky popsané v tématu [vyhovujícím omezením](types.md#satisfying-constraints).

Omezení *class_type* musí splňovat následující pravidla:

*  Typ musí být typ třídy.
*  Typ nesmí být `sealed`.
*  Typ nesmí být jeden z následujících typů `System.Array`:, `System.Delegate`, `System.Enum`nebo `System.ValueType`.
*  Typ nesmí být `object`. Vzhledem k tomu, že `object`všechny typy jsou odvozeny z, takové omezení by nebylo nijak ovlivněno, pokud bylo povoleno.
*  Pouze jedno omezení pro daný parametr typu může být typem třídy.

Typ zadaný jako omezení *INTERFACE_TYPE* musí splňovat následující pravidla:

*  Typ musí být typ rozhraní.
*  Typ nesmí být v dané `where` klauzuli uveden více než jednou.

V obou případech omezení může zahrnovat jakékoli parametry typu asociovaného typu nebo deklarace metody jako součást konstruovaného typu a může zahrnovat deklarovaný typ.

Jakýkoli typ třídy nebo rozhraní zadaný jako omezení parametru typu musí být alespoň přístupná ([Omezení přístupnosti](basic-concepts.md#accessibility-constraints)) jako obecný typ nebo metoda, která je deklarována.

Typ zadaný jako omezení *type_parameter* musí splňovat následující pravidla:

*  Typ musí být parametr typu.
*  Typ nesmí být v dané `where` klauzuli uveden více než jednou.

Kromě toho nesmí být v grafu závislostí parametrů typu žádné cykly, kde závislost je přenosný vztah definovaný pomocí:

*  Pokud `T` je parametr typu použit jako omezení pro `S` parametr `S` typu, ***závisí na*** `T`.
*  Pokud parametr `S` typu závisí na parametru `T` typu a závisí na `T` parametru `U` typu, pak `S` ***závisí na*** `U`.

Vzhledem k tomuto vztahu se jedná o chybu při kompilaci pro parametr typu, který závisí sám na sobě (přímo nebo nepřímo).

Všechna omezení musí být konzistentní mezi parametry závislého typu. Pokud parametr `S` typu závisí na parametru `T` typu, pak:

*  `T`nesmí mít omezení typu hodnoty. V opačném případě `S` `T`je to efektivně zapečetěné, takže by bylo nutné, aby bylo stejného typu, jako s vyloučením nutnosti dvou parametrů typu. `T`
*  Pokud má `S` omezení typu hodnoty, pak `T` nesmí mít omezení *class_type* .
*  Pokud má `S` omezení *class_type* `A` a `T` má omezení *class_type* `B`, musí existovat převod identity nebo implicitní převod odkazu z `A` na `B` nebo implicitní převod odkazu z `B` do `A`.
*  Pokud `S` také závisí na parametru typu `U` a `U` má omezení *class_type* `A` a `T` má omezení *class_type* `B`, musí existovat převod identity nebo implicitní převod odkazu z `A`. `B` nebo implicitní převod odkazu z 0 na 1.

Je platný pro `S` , že má omezení typu hodnoty a `T` má omezení typu odkazu. Toto omezení `T` je efektivní pro typy `System.Object`, `System.ValueType`, `System.Enum`a libovolný typ rozhraní.

Pokud klauzule pro parametr typu obsahuje omezení konstruktoru (které má formulář `new()`), `new` je možné použít operátor k vytvoření instancí typu ([výrazy vytváření objektů](expressions.md#object-creation-expressions)). `where` Jakýkoli argument typu, který se používá pro parametr typu s omezením konstruktoru, musí mít veřejný konstruktor bez parametrů (Tento konstruktor má implicitně existovat pro libovolný typ hodnoty) nebo musí být parametr typu s omezením nebo omezením hodnotového typu (viz [Omezení parametru typu](classes.md#type-parameter-constraints) pro podrobnosti).

Níže jsou uvedeny příklady omezení:
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

Následující příklad je chyba, protože způsobuje cyklické vazby v grafu závislostí parametrů typu:
```csharp
class Circular<S,T>
    where S: T
    where T: S                // Error, circularity in dependency graph
{
    ...
}
```

Následující příklady ilustrují další neplatnou situaci:
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

***Efektivní základní třída*** parametru `T` typu je definována takto:

*  Pokud `T` nemá omezení primárních omezení nebo parametrů typu, jeho efektivní základní třída je `object`.
*  Pokud `T` má omezení typu hodnoty, jeho efektivní základní třída je `System.ValueType`.
*  Pokud má `T` omezení *class_type* `C`, ale žádné omezení *type_parameter* , jeho platná základní třída je `C`.
*  Pokud `T` nemá žádné omezení *class_type* , ale má nejméně jedno omezení *type_parameter* , jeho efektivní základní třída je[nejvýšený](conversions.md#lifted-conversion-operators)typ (přenesené operátory převodu) v sadě efektivních základních tříd jeho *type_ omezení parametru* . Pravidla konzistence zajišťují, že existuje takový typ, který nejlépe zahrnuje.
*  Pokud má `T` omezení *class_type* a jedno nebo více *type_parameter* omezení, jeho efektivní základní třída je[nejvýšený](conversions.md#lifted-conversion-operators)typ (přenesené operátory převodu) v množině sestávající z *class_type* omezení `T` a efektivní základní třídy jeho omezení *type_parameter* . Pravidla konzistence zajišťují, že existuje takový typ, který nejlépe zahrnuje.
*  Pokud má `T` omezení typu reference, ale žádná omezení *class_type* , jeho účinná základní třída je `object`.

Pro účely těchto pravidel platí, že pokud má T omezení `V`, což je *value_type*, použijte místo nejpřesnější základní typ `V`, který je *class_type*. K tomu nemůže nikdy dojít v explicitně daném omezení, ale může dojít v případě, že jsou omezení obecné metody implicitně zděděna přepsáním deklarace metody nebo explicitní implementací metody rozhraní.

Tato pravidla zajišťují, že účinná základní třída je vždy *class_type*.

***Efektivní sada rozhraní*** parametru `T` typu je definována takto:

*  Pokud `T` nemá žádné *secondary_constraints*, je jeho efektivní sada rozhraní prázdná.
*  Pokud má `T` omezení *INTERFACE_TYPE* , ale žádné omezení *type_parameter* , jeho efektivní sada rozhraní je svou sadou omezení *INTERFACE_TYPE* .
*  Pokud `T` nemá žádná omezení *INTERFACE_TYPE* , ale má omezení *type_parameter* , jeho efektivní sada rozhraní je sjednocení platných sad rozhraní svých omezení *type_parameter* .
*  Pokud `T` má omezení *INTERFACE_TYPE* i omezení *type_parameter* , jeho efektivní sada rozhraní je sjednocení své sady omezení *INTERFACE_TYPE* a efektivní sady rozhraní své *type_parameter* jednotlivým.

Parametr typu je ***známý jako odkazový typ*** , pokud má omezení typu odkazu nebo jeho efektivní základní třídu `object` není nebo. `System.ValueType`

Hodnoty typu parametru omezeného typu lze použít pro přístup ke členům instance, které jsou odvozeny omezeními. V příkladu
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
metody `IPrintable` lze vyvolat přímo na `x` , protože `T` je omezeno na vždy implementovat `IPrintable`.

### <a name="class-body"></a>Tělo třídy

*Class_body* třídy definuje členy této třídy.

```antlr
class_body
    : '{' class_member_declaration* '}'
    ;
```

## <a name="partial-types"></a>Částečné typy

Deklarace typu může být rozdělená mezi několik ***deklarací částečného typu***. Deklarace typu je vytvořena z jeho částí podle pravidel v této části, přičemž je považována za jednu deklaraci během doby kompilace programu a za běhu.

*Class_declaration*, *struct_declaration* nebo *interface_declaration* představuje deklaraci částečného typu, pokud obsahuje modifikátor `partial`. `partial`není klíčové slovo a funguje pouze jako modifikátor, pokud se zobrazí bezprostředně před `class`jedním z klíčových slov `struct` nebo `interface` v deklaraci typu nebo před typem `void` v deklaraci metody. V jiných kontextech je možné ji použít jako běžný identifikátor.

Každá část deklarace částečného typu musí obsahovat `partial` modifikátor. Musí mít stejný název a být deklarován ve stejném oboru názvů nebo deklaraci typu jako ostatní části. Modifikátor označuje, že další části deklarace typu mohou existovat jinde, ale existence těchto dalších částí není požadavkem; je platná pro typ s jedinou deklarací pro `partial` zahrnutí modifikátoru. `partial`

Všechny části částečného typu musí být kompilovány dohromady tak, aby mohly být součásti sloučeny v době kompilace do jediné deklarace typu. Částečné typy specificky neumožňují rozšíření již kompilovaných typů.

Vnořené typy mohou být deklarovány ve více částech pomocí `partial` modifikátoru. Nadřazený typ je obvykle deklarován pomocí `partial` i a každá část vnořeného typu je deklarována v jiné části nadřazeného typu.

`partial` Modifikátor není povolený pro deklarace delegáta nebo výčtu.

### <a name="attributes"></a>Atributy

Atributy částečného typu jsou určeny kombinováním v nespecifikovaném pořadí, atributů každé části. Pokud je atribut umístěn na více částech, je ekvivalentní určení atributu vícekrát pro daný typ. Například dvě části:

```csharp
[Attr1, Attr2("hello")]
partial class A {}

[Attr3, Attr2("goodbye")]
partial class A {}
```
jsou ekvivalentní k deklaraci, jako například:
```csharp
[Attr1, Attr2("hello"), Attr3, Attr2("goodbye")]
class A {}
```

Atributy u parametrů typu jsou kombinovány podobným způsobem.

### <a name="modifiers"></a>Modifikátory

Pokud částečná deklarace typu zahrnuje specifikaci přístupnosti ( `public`, `protected`, `internal`a `private` modifikátory), musí souhlasit se všemi ostatními částmi, které obsahují specifikaci přístupnosti. Pokud žádná část částečného typu neobsahuje specifikaci přístupnosti, typ se udělí příslušnému výchozímu usnadnění ([deklarovaný přístup](basic-concepts.md#declared-accessibility)).

Pokud jeden nebo více částečných deklarací vnořeného typu obsahuje `new` modifikátor, není ohlášeno žádné upozornění, pokud vnořený typ skryje zděděný člen ([skrytím prostřednictvím dědičnosti](basic-concepts.md#hiding-through-inheritance)).

Pokud jedna nebo více částečných deklarací třídy obsahuje `abstract` modifikátor, je třída považována za abstraktní ([abstraktní třídy](classes.md#abstract-classes)). V opačném případě se třída považuje za neabstraktní.

Pokud jedna nebo více částečných deklarací třídy obsahuje `sealed` modifikátor, třída je považována za zapečetěnou ([zapečetěné třídy](classes.md#sealed-classes)). V opačném případě je třída považována za nezapečetěnou.

Všimněte si, že třída nemůže být abstraktní i zapečetěná.

Pokud je modifikátor použit v deklaraci částečného typu, pouze tato konkrétní část je považována za nezabezpečený kontext (nebezpečné kontexty).[](unsafe-code.md#unsafe-contexts) `unsafe`

### <a name="type-parameters-and-constraints"></a>Parametry a omezení typu

Je-li obecný typ deklarován ve více částech, musí každá část uvádět parametry typu. Každá část musí mít stejný počet parametrů typu a stejný název pro každý parametr typu v daném pořadí.

Pokud částečná deklarace obecného typu obsahuje omezení (`where` klauzule), musí tato omezení souhlasit se všemi ostatními částmi, které zahrnují omezení. Konkrétně každá část, která obsahuje omezení, musí mít omezení pro stejnou sadu parametrů typu a pro každý parametr typu musí být sady omezení primárního, sekundárního a konstruktoru ekvivalentní. Dvě sady omezení jsou ekvivalentní, pokud obsahují stejné členy. Pokud žádná část částečného obecného typu neurčuje omezení parametru typu, parametry typu jsou považovány za neomezeno.

Příklad
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
je správné, protože tyto části, které obsahují omezení (první dva) efektivně určují stejnou sadu omezení PRIMARY, sekundární a konstruktor pro stejnou sadu parametrů typu, v uvedeném pořadí.

### <a name="base-class"></a>Základní třída

Když deklarace částečné třídy obsahuje specifikaci základní třídy, musí souhlasit se všemi ostatními částmi, které obsahují specifikaci základní třídy. Pokud žádná část částečné třídy neobsahuje specifikaci základní třídy, bude základní třída `System.Object` ([základní třídy](classes.md#base-classes)).

### <a name="base-interfaces"></a>Základní rozhraní

Sada základních rozhraní pro typ deklarovaných ve více částech je sjednocení základních rozhraní specifikovaných v každé části. Konkrétní základní rozhraní může být v každé části pojmenováno pouze jednou, ale je povoleno pro více částí názvů stejných základních rozhraní. Musí existovat pouze jedna implementace členů kteréhokoli daného základního rozhraní.

V příkladu
```csharp
partial class C: IA, IB {...}

partial class C: IC {...}

partial class C: IA, IB {...}
```
sada základních rozhraní pro `C` třídu je `IA`, `IB`a `IC`.

Každá část obvykle poskytuje implementaci rozhraní deklarovaných v této části; Nejedná se však o požadavek. Část může poskytovat implementaci rozhraní deklarovaného v jiné části:
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

### <a name="members"></a>Members

S výjimkou částečných metod ([částečné metody](classes.md#partial-methods)) je množina členů typu deklarovaného ve více částech jednoduše sjednocením sady členů deklarované v každé části. Těla všech částí deklarace typu sdílejí stejné místo deklarace ([deklarace](basic-concepts.md#declarations)) a rozsah jednotlivých členů ([oborů](basic-concepts.md#scopes)) se rozšiřuje na tělo všech částí. Doména přístupnosti libovolného člena vždy obsahuje všechny části ohraničujícího typu; `private` člen deklarovaný v jedné části je volně dostupný z jiné části. Jedná se o chybu při kompilaci, která deklaruje stejného člena ve více než jedné části typu, pokud tento člen není typu s `partial` modifikátorem.

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

Řazení členů v rámci typu je zřídka významné pro C# kód, ale může být významné při propojení s jinými jazyky a prostředími. V těchto případech není definováno řazení členů v rámci typu deklarovaného ve více částech.

### <a name="partial-methods"></a>Částečné metody

Částečné metody mohou být definovány v jedné části deklarace typu a implementovány v jiném. Implementace je volitelná; Pokud žádná část neimplementuje částečnou metodu, deklarace částečné metody a všechna volání jsou z deklarace typu vycházející z kombinace částí.

Částečné metody nemůžou definovat modifikátory přístupu, ale jsou implicitně `private`. Návratový typ musí být `void`a jejich parametry nemohou `out` mít modifikátor. Identifikátor `partial` je rozpoznán jako speciální klíčové slovo v deklaraci metody pouze v případě, že se zobrazuje přímo `void` před typem. v opačném případě lze použít jako normální identifikátor. Částečná metoda nemůže explicitně implementovat metody rozhraní.

Existují dva druhy deklarací částečné metody: Je-li tělo deklarace metody středníkem, deklarace je označována jako ***definice částečné deklarace metody***. Pokud je text uveden jako *blok*, deklarace je označována jako ***implementace částečné deklarace metody***. V rámci deklarace typu může být pouze jedna třída definující deklaraci částečné metody s daným podpisem a může existovat pouze jedna implementace částečné deklarace metody s daným podpisem. Pokud je poskytnuta deklarace částečné metody, musí existovat odpovídající definující deklarace částečné metody a deklarace se musí shodovat, jak je uvedeno v následujícím seznamu:

* Deklarace musí mít stejné modifikátory (i když ne nutně ve stejném pořadí), název metody, počet parametrů typu a počet parametrů.
* Odpovídající parametry v deklaracích musí mít stejné modifikátory (i když ne nutně ve stejném pořadí) a stejné typy (rozdíly modulo v názvech parametrů typu).
* Odpovídající parametry typu v deklaracích musí mít stejná omezení (rozdíly modulo v názvech parametrů typu).

Implementace částečné deklarace metody se může vyskytovat ve stejné části jako odpovídající deklarace částečné metody.

V rámci řešení přetížení se účastní pouze definující částečná metoda. Proto bez ohledu na to, zda je deklarace implementována, mohou být výrazy volání přeloženy na vyvolání částečné metody. Vzhledem k tomu, že částečná metoda vždy vrátí hodnotu `void`, takové výrazy vyvolání budou vždy příkazy výrazu. Vzhledem k tomu, že částečná metoda je `private`implicitně implicitní, takové příkazy budou vždy provedeny v rámci jedné z částí deklarace typu, v rámci které je deklarována částečná metoda.

Pokud žádná část deklarace částečného typu neobsahuje implementaci deklarace pro danou částečnou metodu, jakýkoli příkaz výrazu, který je vyvolán, je jednoduše odebrán z deklarace kombinovaného typu. Proto výraz vyvolání, včetně všech základních výrazů, nemá žádný vliv na dobu běhu. Částečná metoda je také odebrána a nebude členem kombinované deklarace typu.

Pokud pro danou částečnou metodu existuje implementovaná deklarace, jsou zachovány volání částečných metod. Částečná metoda dává deklaraci metody podobně jako implementace částečné metody deklarace, s výjimkou následujících:

* `partial` Modifikátor není zahrnutý.
* Atributy v deklaraci výsledné metody jsou kombinované atributy definující a implementující deklaraci částečné metody v nespecifikovaném pořadí. Duplicity nejsou odebrány.
* Atributy parametrů výsledné deklarace metody jsou kombinované atributy odpovídajících parametrů definující a implementující deklaraci částečné metody v nespecifikovaném pořadí. Duplicity nejsou odebrány.

Pokud je udělena deklarace definující deklaraci, ale nikoli implementující deklaraci, platí následující omezení:

* Jedná se o chybu při kompilaci, která umožňuje vytvořit delegáta metody ([výrazy vytváření delegátů](expressions.md#delegate-creation-expressions)).
* Jedná se o chybu při kompilaci, na kterou se `M` odkazuje uvnitř anonymní funkce, která je převedena na typ stromu výrazu ([vyhodnocení anonymních převodů funkcí na typy stromu výrazů](conversions.md#evaluation-of-anonymous-function-conversions-to-expression-tree-types)).
* Výrazy, které se vyskytují jako součást vyvolání, `M` nemají vliv na určitý stav přiřazení ([jednoznačné přiřazení](variables.md#definite-assignment)), což může potenciálně vést k chybám při kompilaci.
* `M`nemůže být vstupním bodem aplikace ([spuštění aplikace](basic-concepts.md#application-startup)).

Částečné metody jsou užitečné pro umožnění jedné části deklarace typu k přizpůsobení chování jiné části, například, který je generován nástrojem. Zvažte následující deklaraci částečné třídy:
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

Pokud je tato třída zkompilována bez jakýchkoli jiných částí, definice deklarací částečné metody a jejich vyvolání budou odebrány a výsledná kombinovaná deklarace třídy bude odpovídat následujícímu:
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

Předpokládejme, že je k dispozici další část, která poskytuje implementaci deklarace částečných metod:
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

Nakonec bude výsledná kombinovaná deklarace třídy ekvivalentní následujícímu:
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

### <a name="name-binding"></a>Vazba názvu

Přestože každá část rozšiřitelného typu musí být deklarována v rámci stejného oboru názvů, části jsou obvykle zapisovány v rámci různých deklarací oboru názvů. Proto mohou být `using` pro každou část k dispozici různé direktivy ([direktivy using](namespaces.md#using-directives)). Při interpretaci jednoduchých názvů ([odvození typu](expressions.md#type-inference)) v rámci jedné součásti se považují pouze `using` direktivy oboru názvů, které obklopují tuto část. To může mít za následek, že stejný identifikátor má různé významy v různých částech:
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

## <a name="class-members"></a>Členové třídy

Členy třídy se skládají ze členů zavedených jeho *class_member_declaration*s a členy zděděných z přímé základní třídy.

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

Členy typu třídy jsou rozděleny do následujících kategorií:

*  Konstanty, které reprezentují konstantní hodnoty přidružené ke třídě ([konstanty](classes.md#constants)).
*  Pole, která jsou proměnné třídy ([pole](classes.md#fields)).
*  Metody, které implementují výpočty a akce, které mohou být provedeny třídou ([metody](classes.md#methods)).
*  Vlastnosti, které definují pojmenované vlastnosti a akce spojené s čtením a zápisem těchto vlastností ([vlastností](classes.md#properties)).
*  Události, které definují oznámení, která mohou být vygenerována třídou ([událostmi](classes.md#events)).
*  Indexery, které umožňují indexování instancí třídy stejným způsobem (syntakticky) jako pole ([indexery](classes.md#indexers)).
*  Operátory, které definují operátory výrazů, které mohou být aplikovány na instance třídy ([Operators](classes.md#operators)).
*  Konstruktory instancí, které implementují akce vyžadované k inicializaci instancí třídy ([konstruktory instancí](classes.md#instance-constructors))
*  Destruktory, které implementují akce, které mají být provedeny před tím, než dojde k trvalému zahození instancí třídy ([destruktory](classes.md#destructors)).
*  Statické konstruktory, které implementují akce vyžadované k inicializaci samotné třídy ([statické konstruktory](classes.md#static-constructors)).
*  Typy, které představují typy, které jsou místní pro třídu ([vnořené typy](classes.md#nested-types)).

Členy, které mohou obsahovat spustitelný kód, jsou souhrnně označovány jako *Členové funkce* typu třídy. Členy funkce typu třídy jsou metody, vlastnosti, události, indexery, operátory, konstruktory instancí, destruktory a statické konstruktory daného typu třídy.

*Class_declaration* vytvoří nové místo deklarace ([deklarace](basic-concepts.md#declarations)) a *class_member_declarationy*, které jsou okamžitě obsaženy v *class_declaration* , zavádí nové členy do tohoto prostoru deklarací. Následující pravidla platí pro *class_member_declaration*s:

*  Konstruktory instancí, destruktory a statické konstruktory musí mít stejný název jako bezprostředně ohraničující třída. Všichni ostatní členové musí mít názvy, které se liší od názvu bezprostředně ohraničující třídy.
*  Název konstanty, pole, vlastnosti, události nebo typu se musí lišit od názvů všech ostatních členů deklarovaných ve stejné třídě.
*  Název metody se musí lišit od názvů všech ostatních neodpovídajících metod deklarovaných ve stejné třídě. Kromě toho signatura ([signatury a přetížení](basic-concepts.md#signatures-and-overloading)) metody se musí lišit od signatur všech ostatních metod deklarovaných ve stejné třídě a dvě metody deklarované ve stejné třídě nemusí mít signatury, které se liší výhradně pomocí `ref` a. `out`.
*  Signatura konstruktoru instance se musí lišit od signatur všech ostatních konstruktorů instancí deklarovaných ve stejné třídě a dva konstruktory deklarované ve stejné třídě nemusí mít signatury, které se liší výhradně pomocí `ref` a. `out`
*  Signatura indexeru se musí lišit od signatur všech ostatních indexerů deklarovaných ve stejné třídě.
*  Signatura operátoru se musí lišit od signatur všech ostatních operátorů deklarovaných ve stejné třídě.

Zděděné členy typu třídy ([Dědičnost](classes.md#inheritance)) nejsou součástí prostoru deklarací třídy. Odvozená třída proto může deklarovat člen se stejným názvem nebo signaturou jako zděděný člen (což v důsledku toho skrývá zděděný člen).

### <a name="the-instance-type"></a>Typ instance

Každá deklarace třídy má přidružený vázaný typ ([vázané a nevázané typy](types.md#bound-and-unbound-types)), ***typ instance***. Pro deklaraci obecné třídy je typ instance vytvořen vytvořením konstruovaného typu ([vytvořené typy](types.md#constructed-types)) z deklarace typu, přičemž každý ze zadaných argumentů typu je odpovídajícím parametrem typu. Vzhledem k tomu, že typ instance používá parametry typu, lze použít pouze v případě, že jsou parametry typu v oboru; To je uvnitř deklarace třídy. Typ instance je typ `this` pro kód napsaný uvnitř deklarace třídy. Pro jiné než obecné třídy je typ instance jednoduše deklarovanou třídou. Následující znázorňuje několik deklarací třídy spolu s jejich typy instancí: 
```csharp
class A<T>                           // instance type: A<T>
{
    class B {}                       // instance type: A<T>.B
    class C<U> {}                    // instance type: A<T>.C<U>
}

class D {}                           // instance type: D
```

### <a name="members-of-constructed-types"></a>Členové konstruovaných typů

Nezděděné členy konstruovaného typu jsou získány nahrazením, pro každý *type_parameter* v deklaraci členu, odpovídající *type_argument* konstruovaného typu. Proces nahrazení je založen na sémantickém významu deklarace typu a není pouhou náhradou textu.

Například s ohledem na obecnou deklaraci třídy
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

Typ členu `a` v deklaraci `Gen` obecné třídy je " `T`dvojrozměrné pole", takže typ člena `a` v konstruovaném typu výše je "dvojrozměrné pole jednorozměrného pole ".`int`", nebo `int[,][]`.

V rámci členů funkce instance `this` je typem instance typ ([typ instance](classes.md#the-instance-type)) obsahující deklaraci.

Všichni členové obecné třídy mohou používat parametry typu z jakékoliv ohraničující třídy, a to buď přímo, nebo jako součást konstruovaného typu. Pokud je v době běhu použit konkrétní uzavřený konstruovaný typ ([otevřené a uzavřené typy](types.md#open-and-closed-types)), je každé použití parametru typu nahrazeno skutečným argumentem typu zadaným pro konstruovaný typ. Příklad:
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

Třída ***dědí*** členy svého typu přímé základní třídy. Dědičnost znamená, že třída implicitně obsahuje všechny členy svého typu přímé základní třídy, s výjimkou konstruktorů instancí, destruktorů a statických konstruktorů základní třídy. Mezi důležité aspekty dědičnosti patří:

*  Dědičnost je tranzitivní. Pokud `C` je odvozen z `B`a `B` je odvozen z `A`, pak `C` dědí členy deklarované v `B` a také členy deklarované v `A`.
*  Odvozená třída rozšiřuje svou přímou základní třídu. Odvozená třída může přidat nové členy do těch, které dědí, ale nemůže odebrat definici zděděného člena.
*  Konstruktory instancí, destruktory a statické konstruktory nejsou děděny, ale všichni ostatní členové jsou bez ohledu na deklaraci přístupu ([přístupu členů](basic-concepts.md#member-access)). V závislosti na deklaraci přístupnosti ale nemusí být zděděné členy přístupné v odvozené třídě.
*  Odvozená třída může ***Skrýt*** ([skrytím prostřednictvím dědičnosti](basic-concepts.md#hiding-through-inheritance)) zděděné členy deklarováním nových členů se stejným názvem nebo signaturou. Všimněte si, že při skrývání zděděného člena nedojde k odebrání tohoto člena – pouze k tomu, aby byl tento člen přístupný přímo prostřednictvím odvozené třídy.
*  Instance třídy obsahuje sadu všech polí instance deklarované ve třídě a jejích základních třídách a implicitní převod ([implicitní převody odkazů](conversions.md#implicit-reference-conversions)) existují z odvozené třídy typu na libovolný z jeho základních typů tříd. Proto odkaz na instanci některé odvozené třídy může být zpracován jako odkaz na instanci libovolné z jeho základních tříd.
*  Třída může deklarovat virtuální metody, vlastnosti a indexery a odvozené třídy mohou přepsat implementaci těchto členů funkce. To umožňuje třídám navést k polymorfnímu chování při zaznamenání akcí prováděných vyvoláním členu funkce se liší v závislosti na typu běhu instance, jejímž prostřednictvím je tento člen funkce vyvolán.

Zděděný člen typu konstruované třídy jsou členy okamžitého typu základní třídy ([základní třídy](classes.md#base-classes)), které jsou nalezeny nahrazením argumentů typu konstruovaného typu pro každý výskyt odpovídajících parametrů typu v  *specifikace class_base* Tyto členy jsou následně transformovány nahrazením, pro každý *type_parameter* v deklaraci členu, odpovídající *type_argument* specifikace *class_base* .

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

Ve výše uvedeném příkladu `D<int>` má konstruovaný typ nezděděný člen `public int G(string s)` získaný nahrazením argumentu `int` typu pro parametr `T`typu. `D<int>`má také zděděného člena z deklarace `B`třídy. Tento zděděný člen `B<int[]>` je určen prvním určením typu `D<int>` základní třídy nahrazením `int` pro `T` specifikaci `B<T[]>`základní třídy. Pak jako argument `B`typu pro `int[]` je nahrazeno `U` v v `public U F(long index)`, pokud chcete převzít zděděného člena `public int[] F(long index)`.

### <a name="the-new-modifier"></a>Nový modifikátor

*Class_member_declaration* má oprávnění deklarovat člena se stejným názvem nebo signaturou jako zděděný člen. V případě, že k tomu dojde, člen odvozené třídy je uveden ke ***skrytí*** člena základní třídy. Skrytí zděděného člena není považováno za chybu, ale způsobí, že kompilátor vydá upozornění. Chcete-li potlačit upozornění, deklaraci členu odvozené třídy může obsahovat `new` modifikátor, který označuje, že odvozený člen je určen pro skrytí základního člena. Toto téma se podrobněji popisuje [skrytím dědičnosti](basic-concepts.md#hiding-through-inheritance).

Pokud je v deklaraci, která neskrývá zděděný člen, zahrnut modifikátor,zobrazíseupozorněnínatentoefekt.`new` Toto upozornění se potlačí odebráním `new` modifikátoru.

### <a name="access-modifiers"></a>Modifikátory přístupu

*Class_member_declaration* může mít jeden z pěti možných typů deklarovaného usnadnění ([deklarovaný přístup](basic-concepts.md#declared-accessibility)): `public`, `protected internal`, `protected`, `internal` nebo `private`. S výjimkou `protected internal` kombinace se jedná o chybu při kompilaci k určení více než jednoho modifikátoru přístupu. Pokud *class_member_declaration* neobsahuje žádné modifikátory přístupu, předpokládá se `private`.

### <a name="constituent-types"></a>Typy prvků

Typy, které se používají v deklaraci členu, se nazývají typy prvků daného člena. Možné typy prvků jsou typ konstanty, pole, vlastnost, událost nebo indexer, návratový typ metody nebo operátoru a typy parametrů metody, indexeru, operátoru nebo konstruktoru instance. Typy prvků členu musí být alespoň tak přístupné jako samotný člen ([Omezení přístupnosti](basic-concepts.md#accessibility-constraints)).

### <a name="static-and-instance-members"></a>Statické členy a členové instancí

Členy třídy jsou buď ***statické členy*** , nebo ***členy instance***. Obecně řečeno, je vhodné si představit statické členy jako patřící do typů tříd a členů instancí jako patřící do objektů (instance typů tříd).

Pokud pole, metoda, vlastnost, událost, operátor nebo deklarace konstruktoru obsahují `static` modifikátor, deklaruje statický člen. Kromě toho deklarace konstanty nebo typu implicitně deklaruje statický člen. Statické členy mají následující vlastnosti:

*  Pokud je statický člen `M` odkazován v *member_access* ([členský přístup](expressions.md#member-access)) formuláře `E.M`, `E` musí poznamenat typ obsahující `M`. Jedná se o chybu při kompilaci pro `E` označení instance.
*  Statické pole identifikuje přesně jedno umístění úložiště, které se má sdílet všemi instancemi daného typu uzavřené třídy. Bez ohledu na to, kolik instancí daného typu uzavřené třídy je vytvořeno, existuje pouze jedna kopie statického pole.
*  Člen statické funkce (metoda, vlastnost, událost, operátor nebo konstruktor) nepracuje na konkrétní instanci a jedná se o chybu při kompilaci, na kterou se odkazuje `this` v takovém členu funkce.

V případě, že deklarace `static` pole, metody, vlastnosti, události, indexer, konstruktoru nebo destruktoru neobsahuje modifikátor, deklaruje člen instance. (Člen instance se někdy označuje jako nestatický člen.) Členové instance mají následující vlastnosti:

*  Pokud je člen instance `M` odkazován v *member_access* ([členský přístup](expressions.md#member-access)) formuláře `E.M`, `E` musí poznamenat instanci typu obsahujícího `M`. Jedná se o chybu při vazbě pro `E` zaznamenání typu.
*  Každá instance třídy obsahuje samostatnou sadu všech polí instance třídy.
*  Člen funkce instance (metoda, vlastnost, indexer, konstruktor instance nebo destruktor) pracuje na dané instanci třídy a k této instanci může přistupovat jako `this` ([Tento přístup](expressions.md#this-access)).

Následující příklad ukazuje pravidla pro přístup ke statickým a instancím členů:
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

Metoda `F` ukazuje, že v členu funkce instance lze použít *simple_name* ([jednoduché názvy](expressions.md#simple-names)) pro přístup ke členům instance i ke statickým členům. Metoda `G` ukazuje, že ve statickém členovi funkce se jedná o chybu při kompilaci pro přístup k členu instance prostřednictvím *simple_name*. Metoda `Main` ukazuje, *že ke členům* instance musí[být přístup prostřednictvím](expressions.md#member-access)instancí a ke statickým členům musí přistupovat prostřednictvím typů.

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
Třída `B` je vnořený typ, protože je deklarován v rámci `A`třídy a třída `A` je nevnořený typ, protože je deklarována v rámci kompilační jednotky.

#### <a name="fully-qualified-name"></a>Plně kvalifikovaný název

Plně kvalifikovaný název ([plně kvalifikované názvy](basic-concepts.md#fully-qualified-names)) pro vnořený typ je `S.N` tam, kde `S` je plně kvalifikovaný název typu, ve kterém je deklarován `N` typ.

#### <a name="declared-accessibility"></a>Deklarovaná přístupnost

Nevnořené typy mohou mít `public` nebo `internal` být deklarované přístupnosti `internal` a mít ve výchozím nastavení deklaraci přístupnosti. Vnořené typy mohou mít tyto formuláře deklarovaného přístupnosti a navíc jednu nebo více dalších forem deklarovaného usnadnění, v závislosti na tom, zda je obsažený typ třída nebo struktura:

*  Vnořený typ deklarovaný ve třídě může mít některou z pěti forem deklarovaného přístupnosti (`public`, `protected` `internal` `protected internal`,, `private` nebo `private`) a, podobně jako ostatní členy třídy, je použita výchozí deklarace. přístup.
*  Vnořený typ, který je deklarován ve struktuře, může mít jakékoli tři formy deklarovaného přístupnosti `internal`(`public`, `private`, nebo) a podobně jako ostatní členy struktury, `private` přičemž výchozí hodnota je deklarovaná přístupnost.

Příklad
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
deklaruje soukromou vnořenou třídu `Node`.

#### <a name="hiding"></a>Skrytí

Vnořený typ může skrýt ([Skrytí názvu](basic-concepts.md#name-hiding)) základního člena. `new` Modifikátor je povolen pro deklarace vnořeného typu, takže skrytí lze vyjádřit explicitně. Příklad
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
zobrazuje vnořenou třídu `M` , která skrývá metodu `M` definovanou v `Base`.

#### <a name="this-access"></a>Tento přístup

Vnořený typ a jeho obsahující typ nemají zvláštní vztah s ohledem na *This_Access* ([Tento přístup](expressions.md#this-access)). Konkrétně se `this` v rámci vnořeného typu nedá použít k odkazování na instance obsaženého typu. V případech, kdy vnořený typ potřebuje přístup k členům instance svého nadřazeného typu, je možné přístup poskytnout poskytnutím `this` pro instanci nadřazeného typu jako argument konstruktoru pro vnořený typ. Následující příklad
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
ukazuje tuto techniku. `C` Instance třídy `C`vytvoří instanci `Nested` třídy apředá`this` vlastní konstruktorkonstruktoru,abybylomožnéposkytnoutnáslednémupřístupučlenyinstance.`Nested`

#### <a name="access-to-private-and-protected-members-of-the-containing-type"></a>Přístup k soukromým a chráněným členům nadřazeného typu

Vnořený typ má přístup ke všem členům, které mají přístup k jeho nadřazenému typu, včetně členů nadřazeného typu, který má `private` a `protected` deklarované přístupnosti. Příklad
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
zobrazuje třídu `C` , která obsahuje vnořenou třídu `Nested`. V `Nested`rámci, metoda `G` volá statickou `F` metodudefinovanou`C`v a máprivátnídeklaracipřístupnosti.`F`

Vnořený typ také může přistupovat k chráněným členům definovaným v základním typu jeho nadřazeného typu. V příkladu
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
vnořená třída `Derived.Nested` přistupuje k chráněné metodě `F` definované v `Derived`základní třídě `Derived`, `Base`pomocí volání prostřednictvím instance.

#### <a name="nested-types-in-generic-classes"></a>Vnořené typy v obecných třídách

Deklarace obecné třídy může obsahovat deklarace vnořeného typu. V rámci vnořených typů lze použít parametry typu ohraničující třídy. Vnořená deklarace typu může obsahovat další parametry typu, které platí pouze pro vnořený typ.

Každá deklarace typu obsažená v deklaraci obecné třídy je implicitně deklarace obecného typu. Při psaní odkazu na typ vnořený v rámci obecného typu musí být obsažený konstruovaný typ, včetně jeho argumentů typu, pojmenován. V rámci vnější třídy však lze vnořený typ použít bez kvalifikace; typ instance vnější třídy lze implicitně použít při vytváření vnořeného typu. Následující příklad ukazuje tři různé správné způsoby, jak odkazovat na konstruovaný typ vytvořený z `Inner`; první dvě jsou ekvivalentní:
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

Přestože se jedná o špatný styl programování, parametr typu ve vnořeném typu může skrýt člen nebo parametr typu deklarovaný ve vnějším typu:
```csharp
class Outer<T>
{
    class Inner<T>        // Valid, hides Outer's T
    {
        public T t;       // Refers to Inner's T
    }
}
```

### <a name="reserved-member-names"></a>Rezervované názvy členů

Aby bylo možné zajistit C# podkladovou implementaci za běhu, pro každou deklaraci zdrojového člena, který je vlastností, událostí nebo indexerem, musí implementace vyhradit dva signatury metod na základě druhu deklarace člena, jeho názvu a typu. Jedná se o chybu v době kompilace pro program, který deklaruje člena, jehož signatura odpovídá jednomu z těchto vyhrazených signatur, i když základní implementace modulu runtime nepoužívá tyto rezervace.

Rezervované názvy nezavádí deklarace, takže se nepodílejí na vyhledávání členů. Přidružené signatury rezervovaných metod deklarace se ale účastní dědičnosti ([dědičnosti](classes.md#inheritance)) a můžou být skryté pomocí `new` modifikátoru ([nový modifikátor](classes.md#the-new-modifier)).

Rezervace těchto názvů slouží jako tři účely:

*  Pokud chcete, aby základní implementace používala běžný identifikátor jako název metody pro získání nebo nastavení přístupu k funkci C# Language.
*  Aby mohly jiné jazyky spolupracovat s použitím běžného identifikátoru jako názvu metody pro získání nebo nastavení přístupu k funkci C# jazyka.
*  Aby bylo zajištěno, že zdroj přijatý jedním vyhovujícím kompilátorem, je přijímán jiným, tím, že jsou specifické názvy rezervovaných členů konzistentní napříč všemi C# implementacemi.

Deklarace destruktoru ([destruktorů](classes.md#destructors)) také způsobí, že signatura bude rezervována ([názvy členů vyhrazené pro destruktory](classes.md#member-names-reserved-for-destructors)).

#### <a name="member-names-reserved-for-properties"></a>Názvy členů rezervované pro vlastnosti

U vlastnosti `P` ([vlastností](classes.md#properties)) typu `T`jsou rezervovány následující signatury:

```csharp
T get_P();
void set_P(T value);
```

Oba signatury jsou rezervované, a to i v případě, že je vlastnost jen pro čtení nebo jen pro zápis.

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
Třída `A` definuje vlastnost `P`, která je jen pro čtení, a zachovává tak `get_P` signatury pro metody a `set_P` . Třída `B` je odvozena z `A` a skrývá oba tyto vyhrazené signatury. Příklad vytvoří výstup:
```console
123
123
456
```

#### <a name="member-names-reserved-for-events"></a>Názvy členů rezervované pro události

Pro událost `E` ([události](classes.md#events)) typu `T`delegáta jsou vyhrazena následující signatury:
```csharp
void add_E(T handler);
void remove_E(T handler);
```

#### <a name="member-names-reserved-for-indexers"></a>Názvy členů rezervované pro indexery

Pro indexer ([indexery](classes.md#indexers)) typu `T` se seznamem `L`parametrů jsou rezervované následující signatury:
```csharp
T get_Item(L);
void set_Item(L, T value);
```

Oba signatury jsou rezervované, i když je indexer jen pro čtení nebo jen pro zápis.

Název `Item` člena je navíc rezervovaný.

#### <a name="member-names-reserved-for-destructors"></a>Názvy členů rezervované pro destruktory

Pro třídu obsahující destruktor ([destruktory](classes.md#destructors)) je následující signatura vyhrazena:
```csharp
void Finalize();
```

## <a name="constants"></a>Konstanty

***Konstanta*** je člen třídy, který představuje konstantní hodnotu: hodnotu, kterou lze vypočítat v době kompilace. *Constant_declaration* zavádí jednu nebo více konstant daného typu.

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

*Constant_declaration* může zahrnovat sadu *atributů* ([atributů](attributes.md)), modifikátor `new` ([nový modifikátor](classes.md#the-new-modifier)) a platnou kombinaci čtyř modifikátorů přístupu ([modifikátory přístupu](classes.md#access-modifiers)). Atributy a modifikátory se vztahují na všechny členy deklarované *constant_declaration*. I když jsou konstanty považovány za statické členy, *constant_declaration* ani nepožaduje ani neumožňuje modifikátor `static`. Je-li stejný modifikátor v deklaraci konstanty uveden několikrát, jedná se o chybu.

*Typ* *constant_declaration* určuje typ členů zavedených deklarací. Po typu následuje seznam *constant_declarator*, z nichž každý zavádí nového člena. *Constant_declarator* sestává z *identifikátoru* , který název členu následovaný tokenem "`=`" následovaný *constant_expression* ([konstantními výrazy](expressions.md#constant-expressions)), které předává hodnotu člena.

*Typ* zadaný v deklaraci konstanty musí být `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, 0, 1, 2, 3, 4, a *enum_type*nebo reference_.  *Zadejte*. Každý *constant_expression* musí vracet hodnotu cílového typu nebo typu, který lze převést na cílový typ pomocí implicitního převodu ([implicitní převody](conversions.md#implicit-conversions)).

*Typ* konstanty musí být alespoň tak přístupný jako konstanta sama ([Omezení přístupnosti](basic-concepts.md#accessibility-constraints)).

Hodnota konstanty je získána ve výrazu pomocí *simple_name* ([jednoduché názvy](expressions.md#simple-names)) nebo *member_access* ([přístup ke členu](expressions.md#member-access)).

Konstanta se může účastnit *constant_expression*. Proto může být konstanta použita v jakékoli konstrukci, která vyžaduje *constant_expression*. Příklady takových konstrukcí zahrnují `case` popisky, `goto case` příkazy, `enum` deklarace členů, atributy a další konstantní deklarace.

Jak je popsáno v [konstantních výrazech](expressions.md#constant-expressions), *constant_expression* je výraz, který lze plně vyhodnotit v době kompilace. Vzhledem k tomu, že jediný způsob, jak vytvořit *hodnotu jinou než* null, než je `string`, je použití operátoru `new` a protože operátor `new` není v *constant_expression*povolený, jedinou možnou hodnotou konstanty  *reference_type*s jiné než `string` je `null`.

Když je požadován symbolický název hodnoty konstanty, ale pokud typ této hodnoty není v deklaraci konstanty povolen, nebo pokud hodnotu nelze vypočítat v době kompilace *constant_expression*, pole `readonly` ([pole jen pro čtení](classes.md#readonly-fields)) může místo toho použít.

Deklarace konstanty, která deklaruje více konstant je ekvivalentní více deklaracím s jedním konstantou se stejnými atributy, modifikátory a typem. Například
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

Konstanty jsou povoleny v závislosti na jiných konstantách v rámci stejného programu, pokud závislosti nejsou cyklického charakteru. Kompilátor automaticky uspořádá pro vyhodnocení konstantních deklarací v příslušném pořadí. V příkladu
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
Kompilátor nejprve vyhodnotí `A.Y`a poté `B.Z`vyhodnotí a nakonec `A.X`vyhodnotí, a vrátí `10`hodnoty `11`, a `12`. Konstantní deklarace mohou záviset na konstantách z jiných programů, ale tyto závislosti jsou možné pouze v jednom směru. Odkazy na výše uvedený příklad, pokud `A` a `B` byly deklarovány v samostatných programech, by mohly být závislé `A.X` na `B.Z`, ale `B.Z` mohou být pak nezávislá `A.Y`na.

## <a name="fields"></a>Fields (Pole)

***Pole*** je člen, který představuje proměnnou přidruženou k objektu nebo třídě. *Field_declaration* zavádí jedno nebo více polí daného typu.

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

*Field_declaration* může zahrnovat sadu *atributů* ([atributů](attributes.md)), modifikátor `new` ([nový modifikátor](classes.md#the-new-modifier)), platnou kombinaci modifikátorů čtyř přístupu ([modifikátory přístupu](classes.md#access-modifiers)) a modifikátor `static` ([ Statická pole a pole instance](classes.md#static-and-instance-fields)). Kromě toho může *field_declaration* obsahovat modifikátor `readonly` ([pole jen pro čtení](classes.md#readonly-fields)) nebo modifikátor `volatile` (pole s[nestálou](classes.md#volatile-fields)výjimkou), ale ne obojí. Atributy a modifikátory se vztahují na všechny členy deklarované *field_declaration*. Je-li stejný modifikátor v deklaraci pole uveden několikrát, jedná se o chybu.

*Typ* *field_declaration* určuje typ členů zavedených deklarací. Po typu následuje seznam *variable_declarator*, z nichž každý zavádí nového člena. *Variable_declarator* se skládá z *identifikátoru* , který má za člena název, volitelně následovaný tokenem "`=`" a *variable_initializer* ([Inicializátory proměnných](classes.md#variable-initializers)), který poskytuje počáteční hodnotu tohoto člena.

*Typ* pole musí být alespoň přístupný jako pole samotné ([Omezení přístupnosti](basic-concepts.md#accessibility-constraints)).

Hodnota pole se získá ve výrazu s použitím *simple_name* ([jednoduché názvy](expressions.md#simple-names)) nebo *member_access* ([přístup ke členu](expressions.md#member-access)). Hodnota pole, které není jen pro čtení, je upravena pomocí *přiřazení* ([operátor přiřazení](expressions.md#assignment-operators)). Hodnota pole, které není jen pro čtení, může být získána i upravena pomocí operátorů přírůstku[a snížení](expressions.md#postfix-increment-and-decrement-operators)předpony a operátorů přírůstku a snížení předpony ([zvýšení a snížení předpony). Operators](expressions.md#prefix-increment-and-decrement-operators)).

Deklarace pole, která deklaruje více polí, je ekvivalentní více deklaracím jednoho pole se stejnými atributy, modifikátory a typem. Například
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

### <a name="static-and-instance-fields"></a>Statická pole a pole instance

Když deklarace pole obsahuje `static` modifikátor, pole zavedená deklarací jsou ***statická pole***. Pokud není `static` k dispozici žádný modifikátor, pole zavedená deklarací jsou ***pole instance***. Pole statických polí a instancí jsou dva z několika druhů proměnných ([proměnných](variables.md)) podporovaných C#a v době, kdy jsou označovány jako ***statické proměnné*** a ***proměnné instancí***v uvedeném pořadí.

Statické pole není součástí určité instance. místo toho je sdíleno mezi všemi instancemi uzavřeného typu ([otevřené a uzavřené typy](types.md#open-and-closed-types)). Bez ohledu na to, kolik instancí uzavřeného typu třídy je vytvořeno, existuje pouze jedna kopie statického pole pro přidruženou doménu aplikace.

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

Pole instance patří do instance. Konkrétně každá instance třídy obsahuje samostatnou sadu všech polí instance této třídy.

Když je na pole odkazováno v *member_access* ([členský přístup](expressions.md#member-access)) formuláře `E.M`, pokud `M` je statické pole, `E` musí znamenat typ obsahující `M` a pokud `M` je pole instance, musí být E-mailová instance typu, který obsahuje `M`.

Rozdíly mezi členy static a instance jsou podrobněji popsány ve [statických a instancích členů](classes.md#static-and-instance-members).

### <a name="readonly-fields"></a>Pole jen pro čtení

Pokud *field_declaration* obsahuje modifikátor `readonly`, pole zavedená deklarací jsou ***pole jen pro čtení***. Přímá přiřazení k polím jen pro čtení se můžou vyskytovat jenom jako součást této deklarace nebo konstruktoru instance nebo statického konstruktoru ve stejné třídě. (Pole jen pro čtení lze v těchto kontextech přiřadit vícekrát.) Konkrétně Přímá přiřazení k `readonly` poli jsou povolena pouze v následujících kontextech:

*  V *variable_declarator* , který zavádí pole (včetně *variable_initializer* v deklaraci).
*  Pro pole instance v konstruktorech instancí třídy, která obsahuje deklaraci pole; pro statické pole ve statickém konstruktoru třídy, která obsahuje deklaraci pole. Jsou to také pouze kontexty, ve kterých je platný pro předání `readonly` pole `out` jako parametru nebo `ref` .

Pokus o přiřazení k `readonly` poli nebo jeho předání `out` jako parametr nebo `ref` v jakémkoli jiném kontextu je chyba při kompilaci.

#### <a name="using-static-readonly-fields-for-constants"></a>Použití statických polí jen pro čtení pro konstanty

Pole je užitečné, pokud je požadován symbolický název hodnoty konstanty, ale pokud typ hodnoty není `const` v deklaraci povolený, nebo když hodnotu nelze vypočítat v době kompilace. `static readonly` V příkladu
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
`Black`členy, `White`, ,`Red`anelzedeklarovatjako členy,protožejejichhodnotynelzevypočítatvdoběkompilace.`const` `Green` `Blue` `static readonly` Namísto toho je ale deklarujete stejně jako stejný efekt.

#### <a name="versioning-of-constants-and-static-readonly-fields"></a>Správa verzí konstant a statických polí jen pro čtení

Konstanty a pole ReadOnly mají různé sémantiky binárních verzí. Pokud výraz odkazuje na konstantu, je hodnota konstanty získána v době kompilace, ale pokud výraz odkazuje na pole jen pro čtení, hodnota pole se nezíská až do doby běhu. Vezměte v úvahu aplikaci, která se skládá ze dvou samostatných programů:
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

Obory `Program2` názvů aoznačujídvaprogramy,kteréjsoukompiloványsamostatně.`Program1` Vzhledem `Program1.Utils.X` k tomu, že je deklarován jako statické pole jen pro čtení, `Console.WriteLine` není výstup hodnoty příkazu znám v době kompilace, ale je spíše získán za běhu. Proto `X` Pokud je hodnota změněna a `Program1` znovu zkompilována, `Console.WriteLine` příkaz bude výstupem nové hodnoty, i když `Program2` není znovu zkompilován. Nicméně existovala `X` konstanta, `X` hodnota by byla získána v době `Program2` kompilace a by zůstala neovlivněná změnami v `Program1` , dokud `Program2` není znovu zkompilována.

### <a name="volatile-fields"></a>Pole s modifikátorem volatile

Pokud *field_declaration* obsahuje modifikátor `volatile`, pole zavedená touto deklarací jsou pole typu ***volatile***.

Pro nestálá pole mohou optimalizační techniky, které mění pořadí instrukcí, vést k neočekávaným a nepředvídatelným výsledkům v aplikacích s více vlákny, které přistupují k polím bez synchronizace,[jako je například lock_statement ( Lock – příkaz](statements.md#the-lock-statement)). Tyto optimalizace mohou být provedeny kompilátorem, systémem za běhu nebo podle hardwaru. U polí typu volatile jsou tyto optimalizace pro změnu pořadí omezené:

*  Čtení pole s modifikátorem volatile se označuje jako ***nestálé čtení***. Nestálá čtení má "získání sémantiky"; To znamená, že před jakýmikoli odkazy na paměť, ke které dojde v sekvenci instrukcí, je zaručena Tato situace.
*  Zápis pole s modifikátorem volatile se nazývá ***zápis typu volatile***. Volatile zápis má "sémantiku vydání"; To znamená, že je zaručeno, že nastane po odkazech na paměť před instrukcí zápisu v sekvenci instrukcí.

Tato omezení zajistí, že všechna vlákna budou sledovat nestálá zápisy provedené jakýmkoli jiným vláknem v pořadí, ve kterém byly provedeny. Vyhovující implementace není nutná k poskytnutí jediného celkového řazení pro nestálá zápisy, který je vidět ze všech vláken provádění. Typ pole s modifikátorem volatile musí být jeden z následujících:

*  *Reference_type*.
*  `byte`Typ ,`char`,, ,`int`, ,,`bool`,,, nebo .`System.UIntPtr` `float` `ushort` `short` `sbyte` `uint` `System.IntPtr`
*  *Enum_type* , který má základní typ výčtu `byte`, `sbyte`, `short`, `ushort`, `int` nebo `uint`.

Příklad
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
```console
result = 143
```

V tomto příkladu metoda `Main` spustí nové vlákno, které spouští metodu. `Thread2` Tato metoda ukládá hodnotu do nestálého pole s názvem `result`a pak je uloženo `true` v poli `finished`volatile. Hlavní vlákno počká, až bude pole `finished` nastaveno na `true`, a pak přečte pole `result`. Vzhledem `finished` k tomu, `volatile`že bylo deklarováno, musí hlavní vlákno `143` načíst hodnotu z `result`pole. Pokud nebylo `finished` deklarováno `volatile`pole, bylo by `result` povoleno, aby bylo úložiště viditelné pro hlavní vlákno po uložení do `finished`, a proto hlavní vlákno přečte hodnotu `0` z pole `result`. `finished` Deklarace`volatile` jako pole zabrání v takové nekonzistenci.

### <a name="field-initialization"></a>Inicializace pole

Počáteční hodnota pole, zda se jedná o statické pole nebo pole instance, je výchozí hodnota ([výchozí hodnoty](variables.md#default-values)) typu pole. Není možné sledovat hodnotu pole před touto výchozí inicializací a pole není tedy nikdy "uninicializovaný". Příklad
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
```console
b = False, i = 0
```
vzhledem `b` k `i` tomu, že jsou automaticky inicializovány výchozí hodnoty.

### <a name="variable-initializers"></a>Inicializátory proměnných

Deklarace polí můžou zahrnovat *variable_initializer*s. Pro statická pole, Inicializátory proměnných odpovídají příkazům přiřazení provedeným během inicializace třídy. Pro pole instancí odpovídají Inicializátory proměnných příkazy přiřazení, které jsou spuštěny při vytvoření instance třídy.

Příklad
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
```console
x = 1.4142135623731, i = 100, s = Hello
```
vzhledem k `x` tomu, že k přiřazení dojde v případě, že se spustí `i` Inicializátory statických polí a dojde k přiřazení a `s` dojde k provedení inicializátorů pole instance.

Výchozí inicializace hodnoty popsané v části [inicializace pole](classes.md#field-initialization) probíhá pro všechna pole, včetně polí, která mají Inicializátory proměnných. Proto při inicializaci třídy jsou všechna statická pole v této třídě nejprve inicializována na jejich výchozí hodnoty a poté jsou provedeny Inicializátory statického pole v textovém pořadí. Podobně platí, že když je vytvořena instance třídy, všechna pole instance v této instanci jsou nejprve inicializována na jejich výchozí hodnoty a poté jsou v textovém pořadí provedeny Inicializátory pole instance.

Je možné, že statická pole s Inicializátory proměnných budou pozorována ve svém výchozím stavu. Nicméně to se důrazně nedoporučuje jako u stylu. Příklad
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
vykazuje toto chování. Bez ohledu na cyklické definice a a b je program platný. Výsledkem bude výstup
```console
a = 1, b = 2
```
vzhledem k tomu, `a` že `b` statická pole a `0` jsou inicializována na ( `int`výchozí hodnota pro) před spuštěním jejich inicializátorů. Když je inicializátor pro `a` spuštění, `b` hodnota je nula, a proto `a` je inicializována na `1`. V případě inicializátoru `b` pro spuštění je `a` hodnota již `1`, a proto `b` je inicializována na `2`.

#### <a name="static-field-initialization"></a>Inicializace statických polí

Inicializátory proměnných statického pole třídy odpovídají sekvenci přiřazení, která je spuštěna v textovém pořadí, ve kterém se nacházejí v deklaraci třídy. Pokud ve třídě existuje statický konstruktor ([statické konstruktory](classes.md#static-constructors)), provádění statických inicializátorů polí probíhá bezprostředně před spuštěním tohoto statického konstruktoru. V opačném případě jsou Inicializátory statických polí spouštěny v době závislé na implementaci před prvním použitím statického pole této třídy. Příklad
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
může způsobit výstup:
```console
Init A
Init B
1 1
```
nebo výstup:
```console
Init B
Init A
1 1
```
vzhledem k tomu, `X`že spuštění inicializátoru inicializátoru a `Y`inicializátoru může probíhat v obou objednávkách, jsou omezené jenom na to, aby se nastaly před odkazy na tato pole. V tomto příkladu je však:
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
výstup musí být:
```console
Init B
Init A
1 1
```
vzhledem k tomu, `B`že pravidla pro použití statických konstruktorů (jak je definováno ve [statických konstruktorech](classes.md#static-constructors)) poskytují statické konstruktory (a proto `B`statické inicializátory pole) `A`musí být spuštěny před statickým Inicializátory konstruktoru a pole.

#### <a name="instance-field-initialization"></a>Inicializace pole instance

Inicializátory proměnných pole instance třídy odpovídají sekvenci přiřazení, která jsou spouštěna ihned po zadání některého z konstruktorů instance ([Inicializátory konstruktoru](classes.md#constructor-initializers)) dané třídy. Inicializátory proměnných jsou spouštěny v textovém pořadí, ve kterém jsou uvedeny v deklaraci třídy. Proces vytvoření a inicializace instance třídy je podrobněji popsán v části [konstruktory instancí](classes.md#instance-constructors).

Inicializátor proměnné pro pole instance nemůže odkazovat na vytvořenou instanci. Proto se jedná o chybu při kompilaci na odkaz `this` v inicializátoru proměnné, protože se jedná o chybu při kompilaci pro inicializátor proměnné, aby odkazoval na libovolný člen instance prostřednictvím *simple_name*. V příkladu
```csharp
class A
{
    int x = 1;
    int y = x + 1;        // Error, reference to instance member of this
}
```
Proměnná inicializátoru pro `y` výsledky v době kompilace má za následek chybu při kompilaci, protože odkazuje na člen vytvářené instance.

## <a name="methods"></a>Metody

***Metoda*** je člen, který implementuje výpočet nebo akci, kterou lze provést pomocí objektu nebo třídy. Metody jsou deklarovány pomocí *method_declaration*s:

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

*Method_declaration* může zahrnovat sadu *atributů* ([atributů](attributes.md)) a platnou kombinaci modifikátorů čtyř přístupu ([modifikátory přístupu](classes.md#access-modifiers)), `new` ([nový modifikátor](classes.md#the-new-modifier)), `static` ([static a instance). metody](classes.md#static-and-instance-methods)), `virtual` ([virtuální metody](classes.md#virtual-methods)), 0 ([metody přepisu](classes.md#override-methods)), 2 ([zapečetěné metody](classes.md#sealed-methods)), 4 ([abstraktní metody](classes.md#abstract-methods)) a 6 ([externí metody](classes.md#external-methods)) modifikátory.

Deklarace má platnou kombinaci modifikátorů, pokud jsou splněny všechny následující podmínky:

*  Deklarace zahrnuje platnou kombinaci modifikátorů přístupu ([modifikátory přístupu](classes.md#access-modifiers)).
*  Deklarace nezahrnuje stejný modifikátor víckrát.
*  Deklarace zahrnuje maximálně jeden z následujících modifikátorů: `static`, `virtual`, a `override`.
*  Deklarace zahrnuje maximálně jeden z následujících modifikátorů: `new` a. `override`
*  `abstract` Pokud deklarace obsahuje modifikátor, pak deklarace neobsahuje žádné následující modifikátory: `virtual` `static`, `sealed` nebo `extern`.
*  Pokud deklarace obsahuje `private` modifikátor, pak deklarace neobsahuje žádné následující modifikátory: `virtual`, `override`nebo `abstract`.
*  Pokud deklarace obsahuje `sealed` modifikátor, pak deklarace `override` obsahuje také modifikátor.
*  Pokud deklarace obsahuje `partial` modifikátor, pak nezahrnuje žádné z následujících modifikátorů: `new`, `public`, `protected`, `internal`, `private`, `virtual`,, `override` `sealed` , `abstract`nebo .`extern`

Metoda, která má `async` modifikátor, je asynchronní funkce a postupuje podle pravidel popsaných v tématu [asynchronní funkce](classes.md#async-functions).

Typ *deklarace* metody určuje typ počítané hodnoty a vrácenou metodou. *Typ* je `void`, pokud metoda nevrací hodnotu. Pokud deklarace obsahuje `partial` modifikátor, pak návratový typ musí být `void`.

*MEMBER_NAME* Určuje název metody. Pokud metoda není explicitní implementací člena rozhraní ([explicitní implementace členů rozhraní](interfaces.md#explicit-interface-member-implementations)), *MEMBER_NAME* je pouze *identifikátor*. Pro explicitní implementaci člena rozhraní se *MEMBER_NAME* skládá z *INTERFACE_TYPE* následovaných "`.`" a *identifikátorem*.

Volitelné *type_parameter_list* Určuje parametry typu metody ([parametry typu](classes.md#type-parameters)). Pokud je určena *type_parameter_list* , metoda je ***Obecná metoda***. Pokud má metoda modifikátor `extern`, nelze zadat *type_parameter_list* .

Volitelné *formal_parameter_list* Určuje parametry metody ([parametry metody](classes.md#method-parameters)).

Volitelná *type_parameter_constraints_clause*s určují omezení pro jednotlivé parametry typu ([omezení parametrů typu](classes.md#type-parameter-constraints)) a mohou být zadána pouze v případě, že je zadán také parametr *type_parameter_list* a metoda nemá Modifikátor `override`.

*Typ* a každý z typů, na které se odkazuje v *formal_parameter_list* metody, musí být alespoň tak přístupný jako samotná metoda ([Omezení přístupnosti](basic-concepts.md#accessibility-constraints)).

*Method_body* je buď středník, ***text příkazu*** nebo ***tělo výrazu***. Tělo příkazu se skládá z *bloku*, který určuje příkazy, které mají být provedeny při volání metody. Tělo výrazu se skládá `=>` za následováním *výrazu* a středníkem a označuje jeden výraz, který má být proveden při vyvolání metody. 

Pro metody `abstract` a `extern` se *method_body* skládá jednoduše středník. Pro metody `partial` může *method_body* obsahovat buď středník, blok těla nebo tělo výrazu. Pro všechny ostatní metody je *method_body* buď tělo bloku, nebo tělo výrazu.

Pokud se *method_body* skládá ze středníku, nesmí deklarace obsahovat modifikátor `async`.

Název, seznam parametrů typu a seznam formálních parametrů metody definují podpis ([signatury a přetížení](basic-concepts.md#signatures-and-overloading)) metody. Konkrétně signatura metody se skládá z jejího názvu, počtu parametrů typu a počtu, modifikátorů a typů jeho formálních parametrů. Pro tyto účely je jakýkoli parametr typu metody, která se vyskytuje v typu formálního parametru, identifikován jako není jeho názvem, ale podle jeho ordinální pozice v seznamu argumentů typu metody. Návratový typ není součástí signatury metody, ani se nejedná o názvy parametrů typu nebo formální parametry.

Název metody se musí lišit od názvů všech ostatních neodpovídajících metod deklarovaných ve stejné třídě. Kromě toho signatura metody se musí lišit od signatur všech ostatních metod deklarovaných ve stejné třídě a dvě metody deklarované ve stejné třídě nemusí mít signatury, které se liší výhradně pomocí `ref` a. `out`

*Type_parameter*metody jsou v oboru v rámci *method_declaration*a lze je použít k vytvoření typů v celém oboru v typu *typ*, *method_body*a *type_parameter_constraints_clause*s, ale ne v *atributech*.

Všechny formální parametry a parametry typu musí mít jiné názvy.

### <a name="method-parameters"></a>Parametry metody

Parametry metody, pokud existuje, jsou deklarovány metodou *formal_parameter_list*metody.

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

Seznam formálních parametrů se skládá z jednoho nebo více parametrů oddělených čárkami, jejichž *parameter_array*může být pouze poslední.

*Fixed_parameter* se skládá z volitelné sady *atributů* ([atributů](attributes.md)), volitelného modifikátoru `ref`, `out` nebo `this`, *typu*, *identifikátoru* a volitelného *default_argument*. Každý *fixed_parameter* deklaruje parametr daného typu se zadaným názvem. `this` Modifikátor označuje metodu jako metodu rozšíření a je povolen pouze pro první parametr statické metody. Rozšiřující metody jsou dále popsány v tématu [metody rozšíření](classes.md#extension-methods).

*Fixed_parameter* s *default_argument* je označován jako ***volitelný parametr***, zatímco *fixed_parameter* bez *default_argument* je ***povinný parametr***. Požadovaný parametr se nesmí nacházet po volitelném parametru v *formal_parameter_list*.

Parametr `ref` nebo `out` nemůže mít *default_argument*. *Výraz* v *default_argument* musí být jedna z následujících:

*  a *constant_expression*
*  výraz formuláře `new S()` , kde `S` je hodnotový typ
*  výraz formuláře `default(S)` , kde `S` je hodnotový typ

*Výraz* musí být implicitně převoditelný pomocí identity nebo konverze s možnou hodnotou null na typ parametru.

Pokud dojde k volitelným parametrům v deklaraci částečné metody ([částečné metody](classes.md#partial-methods)), explicitní implementace člena rozhraní ([explicitní implementace členů rozhraní](interfaces.md#explicit-interface-member-implementations)) nebo v deklaraci indexeru s jedním parametrem ([ Indexery](classes.md#indexers)) kompilátor by měl poskytnout upozornění, protože tito členové nemohou být nikdy vyvoláni způsobem, který umožňuje vynechat argumenty.

*Parameter_array* se skládá z volitelné sady *atributů* ([atributů](attributes.md)), modifikátoru `params`, *array_type*a *identifikátoru*. Pole parametrů deklaruje jeden parametr daného typu pole se zadaným názvem. *Array_type* pole parametrů musí být jednorozměrného typu pole ([typy polí](arrays.md#array-types)). V volání metody umožňuje pole parametrů zadat buď jeden argument daného typu pole, nebo umožňuje zadat nula nebo více argumentů typu elementu pole. Pole parametrů jsou podrobněji popsány v [polích parametrů](classes.md#parameter-arrays).

K *parameter_array* může dojít po volitelném parametru, ale nemůže mít výchozí hodnotu – vynechání argumentů pro *parameter_array* by vedlo k vytvoření prázdného pole.

Následující příklad znázorňuje různé druhy parametrů:
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

V *formal_parameter_list* pro `M` `i` je povinný parametr ref, `d` je povinný parametr hodnoty `b`, `s`, `o` a `t` jsou volitelné parametry hodnoty a `a` je pole parametrů.

Deklarace metody vytvoří samostatný prostor deklarace pro parametry, parametry typu a místní proměnné. Názvy jsou představeny do tohoto prostoru deklarace seznamem parametrů typu a seznamem formálních parametrů metody a deklarací místní proměnné v *bloku* metody. Jedná se o chybu, pokud dva členy prostoru deklarace metod mají stejný název. Jedná se o chybu pro místo deklarace metody a místní prostor deklarace proměnné vnořeného prostoru deklarace, který obsahuje prvky se stejným názvem.

Vyvolání metody ([vyvolání metod](expressions.md#method-invocations)) vytvoří kopii, která je specifická pro toto vyvolání, formální parametry a lokální proměnné metody a seznam argumentů vyvolání přiřadí k nově vytvořenému formálnímu seznamu hodnoty nebo odkazy na proměnné. ukazatelů. V rámci *bloku* metody mohou být formální parametry odkazovány pomocí jejich identifikátorů ve výrazech *simple_name* ([jednoduché názvy](expressions.md#simple-names)).

Existují čtyři druhy formálních parametrů:

*  Parametry hodnot, které jsou deklarovány bez modifikátorů.
*  Referenční parametry, které jsou deklarovány s `ref` modifikátorem.
*  Výstupní parametry, které jsou deklarovány s `out` modifikátorem.
*  Pole parametrů, která jsou deklarována s `params` modifikátorem.

Jak je popsáno v části [signatury a přetížení](basic-concepts.md#signatures-and-overloading), `ref` jsou `out` modifikátory a součástí signatury metody, ale `params` modifikátor ne.

#### <a name="value-parameters"></a>Parametry hodnoty

Parametr deklarovaný bez modifikátorů je parametrem hodnoty. Parametr hodnoty odpovídá místní proměnné, která vrací počáteční hodnotu z odpovídajícího argumentu zadaného ve volání metody.

Pokud je formální parametr parametr hodnoty, odpovídající argument ve volání metody musí být výraz, který je implicitně konvertibilní ([implicitní převody](conversions.md#implicit-conversions)) na formální typ parametru.

Metoda je povolena k přiřazení nových hodnot parametru hodnoty. Tato přiřazení mají vliv jenom na umístění místního úložiště reprezentované parametrem value – nemají žádný vliv na skutečný argument uvedený ve volání metody.

#### <a name="reference-parameters"></a>Parametry odkazu

Parametr deklarovaný s `ref` modifikátorem je parametr odkazu. Na rozdíl od parametru hodnoty nevytvoří parametr reference nové umístění úložiště. Místo toho parametr reference představuje stejné umístění úložiště jako proměnná zadaná jako argument ve volání metody.

Pokud je formální parametr referenčním parametrem, odpovídající argument ve volání metody musí obsahovat klíčové slovo `ref` následovaný *variable_reference* ([přesné pravidlo pro určení jednoznačného přiřazení](variables.md#precise-rules-for-determining-definite-assignment)) stejného typu jako formální parametr Proměnná musí být jednoznačně přiřazena dříve, než může být předána jako parametr reference.

V rámci metody je referenční parametr vždy považován za jednoznačně přiřazený.

Metoda deklarovaná jako iterátor ([iterátory](classes.md#iterators)) nemůže mít parametry odkazu.

Příklad
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
```console
i = 2, j = 1
```

Pro `Swap` vyvolání v `Main`, `x` představuje a`i` představuje`j`. `y` Proto má vyvolání účinek na záměnu hodnot `i` a. `j`

V metodě, která přebírá parametry odkazu, je možné, že více názvů představuje stejné umístění úložiště. V příkladu
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
`F` vyvolání v `s` rámcipředává`b`odkaz na pro i `a`. `G` Pro toto vyvolání se tedy `s`názvy, `a`a `b` všechny odkazují na stejné umístění úložiště a tři přiřazení všechny upraví pole `s`instance.

#### <a name="output-parameters"></a>Výstupní parametry

Parametr deklarovaný s `out` modifikátorem je výstupní parametr. Podobně jako parametr reference nevytváří výstupní parametr nové umístění úložiště. Namísto toho výstupní parametr představuje stejné umístění úložiště jako proměnná zadaná jako argument ve volání metody.

Pokud je formálním parametrem výstupní parametr, odpovídající argument ve volání metody musí obsahovat klíčové slovo `out` následovaný *variable_reference* ([přesné pravidlo pro určení jednoznačného přiřazení](variables.md#precise-rules-for-determining-definite-assignment)) stejného typu jako formální parametr Proměnná nemusí být jednoznačně přiřazena, než může být předána jako výstupní parametr, ale po vyvolání, kde byla proměnná předána jako výstupní parametr, je proměnná považována za jednoznačně přiřazenou.

V rámci metody, stejně jako místní proměnná, je výstupní parametr zpočátku považován za nepřiřazený a musí být jednoznačně přiřazen před použitím jeho hodnoty.

Každý výstupní parametr metody musí být jednoznačně přiřazen předtím, než se metoda vrátí.

Metoda deklarovaná jako částečná metoda ([částečné metody](classes.md#partial-methods)) nebo iterátory ([iterátory](classes.md#iterators)) nemůže mít výstupní parametry.

Výstupní parametry jsou obvykle používány v metodách, které vytváří více návratových hodnot. Příklad:
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

Příklad vytvoří výstup:
```console
c:\Windows\System\
hello.txt
```

Všimněte si, `dir` že `name` proměnné a lze odřadit `SplitPath`před jejich předáním a že jsou považovány za jednoznačně přiřazené po volání.

#### <a name="parameter-arrays"></a>Pole parametrů

Parametr deklarovaný s `params` modifikátorem je pole parametrů. Pokud seznam formálních parametrů obsahuje pole parametrů, musí být posledním parametrem v seznamu a musí být jednorozměrného typu pole. Typy `string[]` `string[,]` a `string[][]` lze například použít jako typ pole parametrů, ale typ ale nemůže. `params` Modifikátor nemůžete kombinovat s `ref` modifikátory a `out`.

Pole parametrů povoluje, aby byly argumenty zadány jedním ze dvou způsobů volání metody:

*  Argument zadaný pro pole parametrů může být jeden výraz, který je implicitně převoditelný ([implicitní převod](conversions.md#implicit-conversions)) na typ pole parametrů. V tomto případě pole parametrů funguje přesně jako parametr hodnoty.
*  Alternativně může vyvolání zadat nula nebo více argumentů pro pole parametrů, kde každý argument je výraz, který je implicitně převoditelný ([implicitní převod](conversions.md#implicit-conversions)) na typ prvku pole parametrů. V tomto případě vyvolání vytvoří instanci typu pole parametru s délkou odpovídající počtu argumentů, inicializuje prvky instance pole pomocí daných hodnot argumentů a použije nově vytvořenou instanci pole jako aktuální. Argument.

S výjimkou povolení proměnlivého počtu argumentů ve volání je pole parametru přesně ekvivalentní parametrům hodnot (parametrům[hodnot](classes.md#value-parameters)) stejného typu.

Příklad
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
```console
Array contains 3 elements: 1 2 3
Array contains 4 elements: 10 20 30 40
Array contains 0 elements:
```

První vyvolání `F` jednoduše předává pole `a` jako parametr hodnoty. Druhé vyvolání `F` automaticky vytvoří čtyři prvky `int[]` s danými hodnotami elementu a předá tuto instanci pole jako parametr hodnoty. Podobně třetí vyvolání `F` vytvoří nulový element `int[]` a předá tuto instanci jako parametr hodnoty. Druhé a třetí volání jsou přesně ekvivalentní zápisu:
```csharp
F(new int[] {10, 20, 30, 40});
F(new int[] {});
```

Při provádění řešení přetížení může být metoda s polem parametrů platná buď v normálním tvaru, nebo v rozbaleném formátu ([příslušný člen funkce](expressions.md#applicable-function-member)). Rozbalená forma metody je k dispozici pouze v případě, že normální forma metody není platná a pouze pokud příslušná metoda se stejným podpisem jako rozšířený formulář již není deklarována ve stejném typu.

Příklad
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
```console
F();
F(object[]);
F(object,object);
F(object[]);
F(object[]);
```

V příkladu jsou jako běžné metody již do třídy zahrnuty dvě z možných rozšířených forem metody s polem parametrů. Tyto rozšířené formuláře se proto neberou v úvahu při provádění řešení přetížení a první a třetí metoda vyvolání tak vybere běžné metody. Když třída deklaruje metodu s polem parametrů, není neobvyklá a také obsahuje některé z rozšířených formulářů jako běžné metody. Díky tomu je možné se vyhnout přidělení instance pole, která nastane, když je vyvolána rozšířená forma metody s polem parametru.

Když je `object[]`typ pole parametru, dojde k potenciální nejednoznačnosti mezi obvyklou formou metody a vynaloženým formulářem pro jeden `object` parametr. Důvodem nejednoznačnosti je, že je sama `object[]` o sobě implicitně převoditelná na `object`typ. Nejednoznačnost nepředstavuje žádný problém, ale vzhledem k tomu, že je možné ho vyřešit vložením přetypování, pokud je to potřeba.

Příklad
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
```console
System.Int32 System.String System.Double
System.Object[]
System.Object[]
System.Int32 System.String System.Double
```

V prvním a posledním volání aplikace `F`je normální forma pro `F` platná, protože implicitní převod existuje z typu argumentu na typ parametru (oba jsou typu `object[]`). Proto rozlišení přetížení vybírá normální formu `F`a argument je předán jako parametr regulární hodnoty. V druhém a třetím vyvolání není platná normální forma `F` , protože z typu argumentu neexistuje implicitní převod na typ parametru (typ `object` nelze implicitně převést na typ `object[]`). Nicméně rozbalená forma pro `F` je, takže je vybrána při řešení přetížení. Výsledkem je, že vyvolání jednoho prvku `object[]` je vytvořeno voláním a jeden prvek pole je inicializován s danou hodnotou argumentu (což je sám odkaz `object[]`na).

### <a name="static-and-instance-methods"></a>Statické a instanční metody

Pokud deklarace metody obsahuje `static` modifikátor, tato metoda je označována jako statická metoda. Pokud není `static` k dispozici žádný modifikátor, metoda je označována jako metoda instance.

Statická metoda nepracuje na konkrétní instanci a jedná se o chybu při kompilaci, na kterou `this` se odkazuje ve statické metodě.

Metoda instance pracuje na dané instanci třídy a k této instanci může přistupovat jako `this` ([Tento přístup](expressions.md#this-access)).

Když se na metodu odkazuje *member_access* ([členský přístup](expressions.md#member-access)) formuláře `E.M`, pokud `M` je statická metoda, `E` musí znamenat typ obsahující `M` a pokud `M` je metoda instance, `E` musí poznamenat instanci typu. obsahující `M`.

Rozdíly mezi členy static a instance jsou podrobněji popsány ve [statických a instancích členů](classes.md#static-and-instance-members).

### <a name="virtual-methods"></a>Virtuální metody

Pokud deklarace metody instance obsahuje `virtual` modifikátor, tato metoda je označována jako virtuální metoda. Pokud není `virtual` k dispozici žádný modifikátor, metoda je označována jako nevirtuální metoda.

Implementace nevirtuální metody je invariantní: Implementace je stejná, bez ohledu na to, zda je metoda vyvolána na instanci třídy, ve které je deklarována, nebo instanci odvozené třídy. Naproti tomu implementace virtuální metody může být nahrazena odvozenými třídami. Proces nahrazení implementace zděděné virtuální metody je označován jako ***přepsání*** této metody ([metody přepisu](classes.md#override-methods)).

V volání virtuální metody určuje ***Typ spuštění*** instance, pro kterou se volání provádí, způsob, jakým je implementace samotné metody vyvolána. V nevirtuálním volání metody je ***Typ doby kompilace*** instance určujícím faktorem. V přesném termínu, když je `N` vyvolána metoda s názvem seznamem `A` argumentů v instanci s typem `C` pro dobu kompilace a typem `R` za běhu (kde `R` je buď `C` nebo odvozenou třídou z `C`) se volání zpracovává takto:

*  Nejprve je rozlišení přetěžování použito pro `C`, `N`a `A`, pro výběr konkrétní metody `M` ze sady metod deklarovaných a zděděných pomocí `C`. Tento postup je popsaný v tématu [vyvolání metod](expressions.md#method-invocations).
*  V případě `M` , že je metoda jiná než virtuální, `M` je vyvolána.
*  V opačném případě `M` `R` je virtuální metoda a je vyvolána největší odvozená implementace s ohledem na. `M`

Pro každou virtuální metodu deklarovanou nebo zděděnou třídou existuje ***největší odvozená implementace*** metody s ohledem na tuto třídu. Největší odvozená implementace virtuální metody `M` s ohledem na třídu `R` je určena následujícím způsobem:

*  Pokud `R` obsahuje úvodní `virtual` deklaraci `M`, `M`pak toto je největší odvozená implementace.
*  V opačném `R` případě, `override` Pokud `M`obsahuje, je toto největší odvozená implementace `M`.
*  V opačném případě je nejvíce odvozená implementace `M` s ohledem na `R` stejná jako největší odvozená implementace `M` `R`s ohledem na přímou základní třídu.

Následující příklad znázorňuje rozdíly mezi virtuálními a nevirtuálními metodami:
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

V příkladu `A` představuje `F` nevirtuální a virtuální metodu `G`. Třída `B` zavádí novou nevirtuální metodu `F`, takže Skryje zděděné `F`a také přepíše zděděnou metodu `G`. Příklad vytvoří výstup:
```console
A.F
B.F
B.G
B.G
```

Všimněte si, že `a.G()` příkaz `B.G`vyvolá, nikoli `A.G`. Důvodem je, že typ modulu runtime instance (což je `B`), nikoli typ doby kompilace instance (což je `A`), Určuje skutečnou implementaci metody, která má být vyvolána.

Vzhledem k tomu, že metody mohou skrýt zděděné metody, je možné, že třída bude obsahovat několik virtuálních metod se stejnou signaturou. To nepředstavuje problém, protože všechny kromě nejvíce odvozené metody jsou skryté. V příkladu
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
třídy `C` a`D` obsahují dvě virtuální metody se stejnou signaturou: Ten, který představil `A` , a ten, který `C`představil. Metoda, kterou zavedla `C` , skrývá metodu děděnou z. `A` Proto `D` deklarace override v přepisuje metodu `C`, kterou zavádí, a není možné `D` přepsat metodu zavedenou pomocí `A`. Příklad vytvoří výstup:
```console
B.F
B.F
D.F
D.F
```

Všimněte si, že je možné vyvolat skrytou virtuální metodu přístupem k instanci `D` prostřednictvím méně odvozeného typu, ve kterém není metoda skrytá.

### <a name="override-methods"></a>Metody přepsání

Pokud deklarace metody instance obsahuje `override` modifikátor, metoda je označována jako ***Metoda přepsání***. Metoda override přepíše zděděnou virtuální metodu se stejnou signaturou. Zatímco deklarace virtuální metody zavádí novou metodu, deklarace metody přepsání specializuje existující zděděnou virtuální metodu tím, že poskytuje novou implementaci této metody.

Metoda přepsaná `override` deklarací je označována jako ***přepsaná základní metoda***. Pro metodu `M` přepsání deklarovanou ve třídě `C`je určena přepsaná základní metoda prozkoumáním `C`každého typu základní třídy, `C` počínaje přímým typem základní třídy a pokračováním každého po sobě. typ přímé základní třídy, dokud v daném typu základní třídy, je umístěna alespoň jedna přístupná metoda, která má stejný podpis jako `M` po nahrazení argumentů typu. Pro účely nalezení přepsané základní metody `public`je metoda považována za dostupnou `protected` `protected internal`, pokud je, pokud je, pokud je, pokud je, nebo pokud `internal` je, a deklarována ve stejném programu jako `C`.

Dojde k chybě při kompilaci, pokud pro deklaraci přepsání nejsou splněné všechny následující podmínky:

*  Přepsaná základní metoda může být umístěna, jak je popsáno výše.
*  Právě jedna taková přepsaná základní metoda existuje. Toto omezení má vliv pouze v případě, že typ základní třídy je konstruovaný typ, kde nahrazení argumentů typu provede stejnou signaturu dvou metod.
*  Přepsaná základní metoda je metoda virtuální, abstraktní nebo potlačení. Jinými slovy, přepsaná základní metoda nemůže být statická nebo není virtuální.
*  Přepsaná základní metoda není zapečetěná metoda.
*  Metoda override a přepsaná základní metoda mají stejný návratový typ.
*  Deklarace přepisu a přepsaná základní metoda mají stejné deklarované přístupnosti. Jinými slovy deklarace přepisu nemůže změnit přístupnost virtuální metody. Nicméně, pokud je přepsaná základní metoda chráněna jako interní a je deklarována v jiném sestavení než sestavení obsahující metodu přepsání, musí být pro metodu override deklarovaná přístupnost chráněná.
*  Deklarace override neurčuje Type-Parameter-Constraint-klauzule. Místo toho jsou omezení děděna z přepsané základní metody. Všimněte si, že omezení, která jsou parametry typu v přepsané metodě, mohou být nahrazena argumenty typu ve zděděném omezení. To může vést k omezením, která nejsou platná v případě explicitního určení, jako jsou například typy hodnot nebo zapečetěné typy.

Následující příklad ukazuje, jak přepisování pravidel funguje pro obecné třídy:
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

Deklarace přepsání má přístup k přepsané základní metodě pomocí *base_access* ([základní přístup](expressions.md#base-access)). V příkladu
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
`A`vyvolání v `B` vyvolá`PrintFields`metodudeklarovanouv. `base.PrintFields()` *Base_access* zakáže mechanismus virtuálního vyvolání a jednoduše zachází se základní metodou jako nevirtuální metoda. Bylo vyvolání `B` volání v napsáno `((A)this).PrintFields()`, `PrintFields` by rekurzivně vyvolalo metodu deklarovanou v `B`, nikoli deklaraci, která `A`je deklarována v, protože `PrintFields` je virtuální a běhový typ .`((A)this)` je .`B`

Pouze zahrnutím `override` modifikátoru může metoda přepsat jinou metodu. Ve všech ostatních případech metoda se stejnou signaturou jako zděděná metoda jednoduše skrývá zděděnou metodu. V příkladu
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
`F` `A`metoda v `B` nezahrnuje`override` modifikátor, proto nepřepisuje metodu v. `F` Místo toho `F` metoda v `B` skrývá metodu v `A`a upozornění je `new` hlášeno, protože deklarace neobsahuje modifikátor.

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
`F` metoda v `A`skrývá virtuální `F` metodu `B` děděnou z. Vzhledem k tomu `F` , `B` že nový v nástroji má privátní přístup, jeho obor `B` obsahuje pouze tělo třídy a nerozšiřuje `C`jej na. Proto deklarace `F` v `C` jeoprávněna`A`přepsat zděděnéz.`F`

### <a name="sealed-methods"></a>Zapečetěné metody

Pokud deklarace metody instance obsahuje `sealed` modifikátor, tato metoda je označována jako ***zapečetěná metoda***. Pokud deklarace metody instance obsahuje `sealed` modifikátor, musí také `override` obsahovat modifikátor. `sealed` Použití modifikátoru zabrání odvození třídy z dalšího přepsání metody.

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
Třída `B` poskytuje dvě metody přepisu `F` : metoda, která `G` má `sealed` modifikátor a metodu, která není. `B`použití zapečetění `modifier` zabrání `C` dalšímu přepsání `F`.

### <a name="abstract-methods"></a>Abstraktní metody

Pokud deklarace metody instance obsahuje `abstract` modifikátor, tato metoda je označována jako ***abstraktní metoda***. I když je abstraktní metoda implicitně také virtuální metodou, nemůže mít modifikátor `virtual`.

Deklarace abstraktní metody zavádí novou virtuální metodu, ale neposkytuje implementaci této metody. Místo toho jsou vyžadovány neabstraktní odvozené třídy k poskytnutí vlastní implementace přepsáním této metody. Vzhledem k tomu, že abstraktní metoda neposkytuje žádnou skutečnou implementaci, *method_body* abstraktní metody se jednoduše skládá ze střední hodnoty.

Deklarace abstraktní metody jsou povoleny pouze v abstraktních třídách ([abstraktní třídy](classes.md#abstract-classes)).

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
`Shape` třída definuje abstraktní pojem objektu geometrického tvaru, který může malovat sám. Metoda `Paint` je abstraktní, protože není k dispozici žádná smysluplná výchozí implementace. Třídy `Ellipse` a `Box` jsou konkrétní`Shape` implementace. Vzhledem k tomu, že tyto třídy jsou neabstraktní, jsou vyžadovány `Paint` pro přepsání metody a poskytnutí skutečné implementace.

Jedná se o chybu při kompilaci pro *base_access* ([základní přístup](expressions.md#base-access)) pro odkazování na abstraktní metodu. V příkladu
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
pro `base.F()` vyvolání je hlášena chyba při kompilaci, protože odkazuje na abstraktní metodu.

Deklarace abstraktní metody je povolena pro přepsání virtuální metody. To umožňuje abstraktní třídě vynutit znovu implementaci metody v odvozených třídách a provede původní implementaci metody jako nedostupné. V příkladu
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
Třída `A` deklaruje virtuální metodu, třída `B` Přepisuje tuto metodu abstraktní metodou a třída `C` Přepisuje abstraktní metodu k poskytnutí vlastní implementace.

### <a name="external-methods"></a>Externí metody

Pokud deklarace metody obsahuje `extern` modifikátor, tato metoda je označována jako ***externí metoda***. Externí metody jsou implementovány externě, obvykle v jiném jazyce než C#. Vzhledem k tomu, že deklarace externí metody neposkytuje žádnou skutečnou implementaci, *method_body* vnější metody se skládá středníkem. Externí metoda nemůže být obecná.

Modifikátor se obvykle používá ve spojení `DllImport` s atributem ([součinností s komponentami com a Win32](attributes.md#interoperation-with-com-and-win32-components)), což umožňuje implementovat externí metody pomocí knihoven DLL (knihovny dynamického propojení). `extern` Spouštěcí prostředí může podporovat jiné mechanismy, při kterých lze zadat implementace externích metod.

Pokud externí metoda obsahuje `DllImport` atribut, deklarace metody musí také `static` obsahovat modifikátor. Tento příklad ukazuje použití `extern` modifikátoru `DllImport` a atributu:
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

Pokud deklarace metody obsahuje `partial` modifikátor, tato metoda je označována jako ***částečná metoda***. Částečné metody lze deklarovat pouze jako členy částečných typů ([částečné typy](classes.md#partial-types)) a jsou předmětem řady omezení. Částečné metody jsou podrobněji popsány v [části částečné metody](classes.md#partial-methods).

### <a name="extension-methods"></a>Metody rozšíření

Pokud první parametr metody obsahuje `this` modifikátor, tato metoda je označována jako ***metoda rozšíření***. Metody rozšíření lze deklarovat pouze v neobecných statických třídách, které nejsou vnořené. První parametr metody rozšíření nemůže mít modifikátory jiné než `this`a typ parametru nemůže být typu ukazatel.

Následuje příklad statické třídy, která deklaruje dvě metody rozšíření:
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

Metoda rozšíření je běžnou statickou metodou. Kromě toho, kde je jeho ohraničující statická třída v oboru, lze metodu rozšíření vyvolat pomocí syntaxe volání metody instance ([vyvolání metody rozšíření](expressions.md#extension-method-invocations)) pomocí výrazu příjemce jako prvního argumentu.

Následující program používá metody rozšíření deklarované výše:
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

Metoda je k dispozici `string[]`v `ToInt32` a metoda je k dispozici na `string`, protože byla deklarována jako metody rozšíření. `Slice` Význam tohoto programu je stejný jako následující při použití běžných volání statických metod:
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

*Method_body* deklarace metody se skládá buď z těla bloku, textu výrazu, nebo od středníku.

***Výsledný typ*** metody je `void` , pokud je `void`návratový typ, nebo pokud je metoda asynchronní a návratový typ je `System.Threading.Tasks.Task`. V opačném případě typ výsledku neasynchronní metody je jeho návratový typ a výsledný typ asynchronní metody s návratovým typem `System.Threading.Tasks.Task<T>` je. `T`

V případě, že metoda `void` má typ výsledku a tělo bloku, `return` příkazy ([příkaz return](statements.md#the-return-statement)) v bloku nepovolují zadání výrazu. Pokud se provádění bloku metody void dokončí normálně (to znamená, že řízení projde na konci těla metody), tato metoda jednoduše vrátí na aktuálního volajícího.
    
Když metoda obsahuje výsledek `void` a tělo výrazu, výraz `E` musí být *statement_expression*a tělo je přesně stejné jako blokové tělo formuláře `{ E; }`.
    
Pokud má metoda typ výsledku, který není typu void, a tělo bloku, každý `return` příkaz v bloku musí specifikovat výraz, který je implicitně převeden na typ výsledku. Koncový bod těla bloku metody vracející hodnoty nesmí být dosažitelný. Jinými slovy, v metodě vracející hodnoty s tělo bloku není ovládací prvek povolen tok na konec těla metody.
    
Pokud má metoda typ výsledku, který není typu void, a tělo výrazu, musí být výraz implicitně převeden na výsledný typ a tělo je přesně ekvivalentní tělo bloku ve formuláři `{ return E; }`.
    
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
metoda vracející `F` hodnoty má za následek chybu při kompilaci, protože řízení může přesměrovat konec těla metody. Metody `G` a`H` jsou správné, protože všechny možné cesty provádění končí v příkazu return, který určuje návratovou hodnotu. `I` Metoda je správná, protože její tělo je ekvivalentní bloku příkazu s pouze jediným příkazem Return.

### <a name="method-overloading"></a>Přetížení metody

Pravidla řešení přetížení metod jsou popsána v tématu [odvození typu](expressions.md#type-inference).

## <a name="properties"></a>properties

***Vlastnost*** je člen, který poskytuje přístup k vlastnosti objektu nebo třídy. Mezi příklady vlastností patří délka řetězce, velikost písma, titulek okna, jméno zákazníka atd.... Vlastnosti jsou přirozené rozšíření polí – oba se nazývají členy s přidruženými typy a syntaxe pro přístup k polím a vlastnostem je stejná. Na rozdíl od polí ale vlastnosti neoznačují umístění úložiště. Místo toho mají vlastnosti ***přistupující objekty*** , které určují příkazy, které mají být provedeny, když jsou jejich hodnoty čteny nebo zapsány. Vlastnosti tak poskytují mechanismus pro přidružení akcí ke čtení a zápisu atributů objektu; Kromě toho povolují, aby tyto atributy byly vypočítány.

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

*Property_declaration* může zahrnovat sadu *atributů* ([atributů](attributes.md)) a platnou kombinaci modifikátorů čtyř přístupu ([modifikátory přístupu](classes.md#access-modifiers)), `new` ([nový modifikátor](classes.md#the-new-modifier)), `static` ([static a instance). metody](classes.md#static-and-instance-methods)), `virtual` ([virtuální metody](classes.md#virtual-methods)), 0 ([metody přepisu](classes.md#override-methods)), 2 ([zapečetěné metody](classes.md#sealed-methods)), 4 ([abstraktní metody](classes.md#abstract-methods)) a 6 ([externí metody](classes.md#external-methods)) modifikátory.

Deklarace vlastností jsou v souladu se stejnými pravidly jako deklarace metod ([metody](classes.md#methods)) s ohledem na platné kombinace modifikátorů.

*Typ* deklarace vlastnosti určuje typ vlastnosti zavedené deklarací a *MEMBER_NAME* Určuje název vlastnosti. Pokud vlastnost není explicitní implementací člena rozhraní, *MEMBER_NAME* je pouze *identifikátor*. Pro explicitní implementaci člena rozhraní ([explicitní implementace členů rozhraní](interfaces.md#explicit-interface-member-implementations)) se *MEMBER_NAME* skládá z *INTERFACE_TYPE* následovaných "`.`" a *identifikátorem*.

*Typ* vlastnosti musí být alespoň tak přístupný jako samotná vlastnost ([Omezení přístupnosti](basic-concepts.md#accessibility-constraints)).

*Property_body* může buď sestávat z ***těla přistupujícího objektu*** nebo ***textu výrazu***. V těle přístupového objektu *accessor_declarations*, který musí být uzavřen v tokenech "`{`" a "`}`", deklarujte přistupující objekty ([přístupové objekty](classes.md#accessors)) vlastnosti. Přistupující objekty určují spustitelné příkazy spojené s čtením a zápisem vlastnosti.

Tělo `=>` výrazu sestávající z *výrazu následovaný výrazem* `E` a středníkem je přesně ekvivalentní tělo `{ get { return E; } }`příkazu a lze jej proto použít pouze k zadání vlastností pouze pro getter, kde výsledek metoda getter je dána jediným výrazem.

*Property_initializer* se může předávat jenom pro automaticky implementované vlastnosti ([automaticky implementované vlastnosti](classes.md#automatically-implemented-properties)) a vyvolá inicializaci podkladového pole těchto vlastností s hodnotou zadanou *výrazem.* .

I když syntaxe pro přístup k vlastnosti je stejná jako u pole, vlastnost není klasifikována jako proměnná. Proto není možné předat vlastnost jako `ref` argument or. `out`

Pokud deklarace vlastnosti obsahuje `extern` modifikátor, vlastnost je označována jako ***externí vlastnost***. Vzhledem k tomu, že deklarace externí vlastnosti neposkytuje žádnou skutečnou implementaci, každá z jeho *accessor_declarations* se skládá ze střední hodnoty.

### <a name="static-and-instance-properties"></a>Statické a vlastnosti instance

Pokud deklarace vlastnosti obsahuje `static` modifikátor, vlastnost je označována jako ***statická vlastnost***. Pokud není `static` k dispozici žádný modifikátor, vlastnost je označována jako ***vlastnost instance***.

Statická vlastnost není přidružena k určité instanci a jedná se o chybu při kompilaci, na kterou se odkazuje `this` v přístupových objektech statické vlastnosti.

Vlastnost instance je přidružena k dané instanci třídy a k této instanci lze v přístupových objektech této `this` vlastnosti přistupovat jako ([Tento přístup](expressions.md#this-access)).

Když se na vlastnost odkazuje v *member_access* ([členský přístup](expressions.md#member-access)) formuláře `E.M`, pokud `M` je statická vlastnost, `E` musí znamenat typ obsahující `M` a pokud `M` je vlastnost instance, musí být E-mailová instance typu. obsahující `M`.

Rozdíly mezi členy static a instance jsou podrobněji popsány ve [statických a instancích členů](classes.md#static-and-instance-members).

### <a name="accessors"></a>Přístupové objekty

*Accessor_declarations* vlastnosti určují spustitelné příkazy přidružené ke čtení a zápisu této vlastnosti.

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

Deklarace přistupujícího objektu se skládají z *get_accessor_declaration*, *set_accessor_declaration*nebo obou. Každá deklarace přistupujícího objektu se skládá z tokenu `get` nebo `set` následovaného nepovinným *accessor_modifier* a *accessor_body*.

Použití *accessor_modifier*s se řídí následujícími omezeními:

*  *Accessor_modifier* se nesmí použít v rozhraní nebo v explicitní implementaci člena rozhraní.
*  U vlastnosti nebo indexeru, který nemá modifikátor `override`, je povolený *accessor_modifier* jenom v případě, že vlastnost nebo indexer má přístupový objekt `get` i `set` a pak je povolený jenom pro jeden z těchto přístupových objektů.
*  U vlastnosti nebo indexeru, který obsahuje modifikátor `override`, musí přistupující objekt odpovídat *accessor_modifier*(pokud existuje) přepsaného objektu.
*  *Accessor_modifier* musí deklarovat přístupnost, která je striktně přísnější než deklarovaná přístupnost vlastnosti nebo samotného indexeru. Bude přesný:
   * Pokud má vlastnost nebo indexeru deklarovanou přístupnost `public`, může být *accessor_modifier* buď `protected internal`, `internal`, `protected` nebo `private`.
   * Pokud má vlastnost nebo indexeru deklaraci přístupnosti `protected internal`, může být *accessor_modifier* buď `internal`, `protected` nebo `private`.
   * Pokud má vlastnost nebo indexeru deklaraci přístupnosti `internal` nebo `protected`, *accessor_modifier* musí být `private`.
   * Pokud vlastnost nebo indexovací člen má deklaraci přístupnosti `private`, nemůžete použít žádný *accessor_modifier* .

U vlastností `abstract` a `extern` jsou *accessor_body* pro každý přistupující objekt jednoduše středníkem. Neabstraktní vlastnost, která není typu extern, může mít každý *accessor_body* středníkem, v takovém případě se jedná o ***automaticky implementovanou vlastnost*** ([automaticky implementované vlastnosti](classes.md#automatically-implemented-properties)). Automaticky implementovaná vlastnost musí mít alespoň přistupující objekt get. Pro přistupující objekty jakékoli jiné neabstraktní nebo neexterné vlastnosti je *accessor_body* *blok* , který určuje příkazy, které mají být provedeny, když je vyvolán odpovídající přistupující objekt.

`get` Přistupující objekt odpovídá metodě bez parametrů s návratovou hodnotou typu vlastnosti. S výjimkou cíle přiřazení, při odkazování na vlastnost ve výrazu, `get` je objekt přistupující k vlastnosti vyvolán pro výpočet hodnoty vlastnosti ([hodnoty výrazů](expressions.md#values-of-expressions)). Tělo `get` přístupového objektu musí splňovat pravidla pro metody vracející hodnoty popsané v [těle metody](classes.md#method-body). Konkrétně všechny `return` příkazy v těle `get` přístupového objektu musí určovat výraz, který lze implicitně převést na typ vlastnosti. Kromě toho nesmí být koncový bod `get` přístupového objektu dostupný.

Přistupující objekt odpovídá metodě s parametrem s jednou hodnotou typu vlastnosti `void` a návratovým typem. `set` Implicitní parametr `set` přístupového objektu je vždy pojmenován `value`. Je-li na vlastnost odkazováno jako na cíl přiřazení ([operátor přiřazení](expressions.md#assignment-operators)) `++` nebo jako Operand operátoru `--` nebo (operátory[přírůstku a snížení](expressions.md#postfix-increment-and-decrement-operators) [předpony, modifikátor přírůstku a snížení předpony](expressions.md#prefix-increment-and-decrement-operators)), přistupující objekt je vyvolán s argumentem (jehož hodnota je pravá strana přiřazení nebo operand `++` operátoru OR `--` ), který poskytuje novou hodnotu ([jednoduché přiřazení](expressions.md#simple-assignment)). `set` Tělo `set` přístupového objektu musí splňovat pravidla pro `void` metody popsané v [těle metody](classes.md#method-body). Konkrétně `return` příkazy`set` v těle přístupového objektu nejsou povoleny pro zadání výrazu. Vzhledem k tomu, že `value` `set` objektpropřístupimplicitněobsahujeparametrsnázvem,jednáseochybupřikompilacimístníproměnnénebokonstantnídeklaracevpřístupovémobjektu,aby`set` měl tento název.

Na základě přítomnosti nebo nepřítomnosti `get` přístupových objektů a `set` je vlastnost klasifikována takto:

*  Vlastnost, která obsahuje `get` přístupový objekt `set` i přistupující objekt, je označována jako vlastnost ***pro čtení i zápis*** .
*  Vlastnost, která má pouze `get` přistupující objekt, je označována jako vlastnost ***jen pro čtení*** . Jedná se o chybu při kompilaci pro vlastnost, která je jen pro čtení, aby byla cílem přiřazení.
*  Vlastnost, která má pouze `set` přistupující objekt, je označována jako vlastnost pouze pro ***zápis*** . S výjimkou cíle přiřazení se jedná o chybu při kompilaci pro odkazování na vlastnost pouze pro zápis ve výrazu.

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
ovládací prvek deklaruje veřejnou `Caption` vlastnost. `Button` Přistupující objekt `Caption` vlastnosti vrátí řetězec uložený v soukromém `caption` poli. `get` `set` Přístupový objekt kontroluje, zda je nová hodnota odlišná od aktuální hodnoty, a pokud ano, uloží novou hodnotu a znovu vykreslí ovládací prvek. Vlastnosti často postupují podle výše uvedeného vzoru: Přístupový objekt jednoduše vrátí hodnotu uloženou v soukromém poli `set` a přistupující objekt změní toto soukromé pole a pak provede jakékoli další akce, které jsou vyžadovány k úplné aktualizaci stavu objektu. `get`

Podle výše uvedené `Caption` třídyjepříkladem`Button` použití vlastnosti:
```csharp
Button okButton = new Button();
okButton.Caption = "OK";            // Invokes set accessor
string s = okButton.Caption;        // Invokes get accessor
```

V `set` tomto případě je přístup k objektu vyvolán přiřazením hodnoty vlastnosti `get` a přístup je vyvolán odkazem na vlastnost ve výrazu.

Přístupové objekty `set` a vlastnosti nejsou jedinečné, a není možné deklarovat přístupové objekty pro vlastnost samostatně. `get` V takovém případě není možné, aby dva přistupující objekty vlastnosti pro čtení a zápis měly jinou přístupnost. Příklad
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
nedeklaruje jednu vlastnost pro čtení i zápis. Místo toho deklaruje dvě vlastnosti se stejným názvem, jeden jen pro čtení a jeden jen pro zápis. Vzhledem k tomu, že dva členy deklarované ve stejné třídě nemohou mít stejný název, v příkladu dojde k chybě při kompilaci.

Když odvozená třída deklaruje vlastnost se stejným názvem jako zděděné vlastnosti, skryje odvozená vlastnost děděné vlastnosti s ohledem na čtení i zápis. V příkladu
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
vlastnost v `B` nástroji skrývá`P` vlastnost v`A` s ohledem na čtení i zápis. `P` Proto v příkazech
```csharp
B b = new B();
b.P = 1;          // Error, B.P is read-only
((A)b).P = 1;     // Ok, reference to A.P
```
přiřazení, které `b.P` způsobí nahlášení chyby při kompilaci, vzhledem k tomu, že `P` vlastnost jen pro čtení v `B` nástroji skrývá vlastnost pouze `P` pro zápis v `A`. Upozorňujeme však, že k přístupu k skryté `P` vlastnosti lze použít přetypování.

Na rozdíl od veřejných polí vlastnosti poskytují oddělení vnitřní stav objektu a jeho veřejné rozhraní. Vezměte v úvahu příklad:
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

Tady třída používá dvě `int` pole, `x` a `y`k uložení jeho umístění. `Label` Umístění je veřejně `X` vystaveno jako `Y` a vlastnost i jako `Location` vlastnost typu `Point`. Pokud v budoucí verzi `Label`nástroje, je vhodnější Uložit umístění `Point` jako interní, může být změna provedena bez vlivu na veřejné rozhraní třídy:
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

Bylo `x` a `y` místo toho `public readonly` bylo možnéprovést`Label` takovou změnu třídy.

Vystavení stavu prostřednictvím vlastností není nutně efektivní než vystavení polí přímo. Zejména pokud je vlastnost nevirtuální a obsahuje pouze malý objem kódu, může spouštěcí prostředí nahradit volání přístupových objektů skutečným kódem přístupových objektů. Tento proces se označuje jako a dává přístup k vlastnostem jako účinný přístup k ***polím, ale***zachovává zvýšenou flexibilitu vlastností.

Vzhledem k tomu, `get` že vyvolání přístupového objektu je v koncepčním ekvivalentu pro čtení hodnoty pole, je považováno za `get` špatný programovací styl pro přistupující objekty, aby měly pozorovatelící vedlejší účinky. V příkladu
```csharp
class Counter
{
    private int next;

    public int Next {
        get { return next++; }
    }
}
```
hodnota `Next` vlastnosti závisí na počtu dříve přístupů k vlastnosti. Proto přístup k vlastnosti vytvoří pozorovatelný vedlejší efekt a vlastnost by měla být namísto toho implementována jako metoda.

Konvence žádné vedlejší účinky pro `get` přístupové objekty neznamená, že přistupující objekty by měly být vždy zapisovány, `get` aby jednoduše vracely hodnoty uložené v polích. Přistupující objekty `get` budou často počítat hodnotu vlastnosti tím, že získávají přístup k více polím nebo vyvolání metod. Správně navržený `get` přistupující objekt ale neprovede žádné akce, které způsobují pozorovatelné změny stavu objektu.

Vlastnosti lze použít ke zpoždění inicializace prostředku až do okamžiku, kdy se na něj poprvé odkazuje. Příklad:
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

Třída obsahuje tři vlastnosti `In` `Error`,, a, které reprezentují standardní vstupní, výstupní a chybové zařízení. `Out` `Console` Zveřejněním těchto členů jako vlastností může `Console` třída zpozdit svou inicializaci, dokud nebudou skutečně použity. Například při prvním odkazování na `Out` vlastnost, jako v
```csharp
Console.Out.WriteLine("hello, world");
```
Vytvoří se `TextWriter` podklad pro výstupní zařízení. Pokud ale aplikace nevytváří žádné odkazy na `In` vlastnosti a `Error` , nevytvoří se pro tato zařízení žádné objekty.

### <a name="automatically-implemented-properties"></a>Automaticky implementované vlastnosti

Automaticky implementovaná vlastnost (nebo ***Automatická vlastnost*** pro krátkou hodnotu) je neabstraktní neexterní vlastnost s tělem přístupového objektu jenom středníkem. K automatickým vlastnostem musí mít přistupující objekt get a volitelně může mít přístupový objekt set.

Je-li vlastnost zadána jako automaticky implementovaná vlastnost, je pro vlastnost automaticky k dispozici skryté pole zálohování a přistupující objekty jsou implementovány pro čtení a zápis do daného pole pro zálohování. Pokud automatická vlastnost nemá žádný přistupující objekt set, je pole zálohování považováno za `readonly` ([pole jen pro čtení](classes.md#readonly-fields)). Stejně jako `readonly` pole lze také přiřadit automatickou vlastnost pouze pro getter do těla konstruktoru ohraničující třídy. Takové přiřazení přiřadí přímo k poli pro zálohování jen pro čtení ve vlastnosti.

Automatická vlastnost může volitelně mít *property_initializer*, který se aplikuje přímo na pole pro zálohování jako *variable_initializer* ([Inicializátory proměnných](classes.md#variable-initializers)).

Následující příklad:
```csharp
public class Point {
    public int X { get; set; } = 0;
    public int Y { get; set; } = 0;
}
```
je ekvivalentní následující deklaraci:
```csharp
public class Point {
    private int __x = 0;
    private int __y = 0;
    public int X { get { return __x; } set { __x = value; } }
    public int Y { get { return __y; } set { __y = value; } }
}
```

Následující příklad:
```csharp
public class ReadOnlyPoint
{
    public int X { get; }
    public int Y { get; }
    public ReadOnlyPoint(int x, int y) { X = x; Y = y; }
}
```
je ekvivalentní následující deklaraci:
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

Všimněte si, že přiřazení k poli jen pro čtení jsou zákonná, protože se vyskytují v rámci konstruktoru.


### <a name="accessibility"></a>Přístupnost

Pokud má přistupující objekt *accessor_modifier*, doména přístupnosti ([domény přístupnosti](basic-concepts.md#accessibility-domains)) je určena pomocí deklarovaného přístupnosti *accessor_modifier*. Pokud přístupový objekt nemá *accessor_modifier*, je doména přístupnosti přistupujícího objektu určena z deklarovaného přístupnosti vlastnosti nebo indexeru.

Přítomnost *accessor_modifier* nikdy nemá vliv na vyhledávání členů ([Operators](expressions.md#operators)) nebo rozlišení přetěžování ([řešení přetížení](expressions.md#overload-resolution)). Modifikátory u vlastnosti nebo indexeru vždy určují, na kterou vlastnost nebo indexer je svázán, bez ohledu na kontext přístupu.

Po výběru konkrétní vlastnosti nebo indexeru se k určení, jestli je toto použití platné, použijí domény přístupnosti konkrétních přístupových objektů:

*  Pokud je použití jako hodnota ([hodnoty výrazů](expressions.md#values-of-expressions)), `get` přistupující objekt musí existovat a být přístupný.
*  Pokud je použití jako cíl jednoduchého přiřazení ([jednoduché přiřazení](expressions.md#simple-assignment)), `set` přístup musí existovat a být přístupný.
*  Pokud je použití jako cíl složeného přiřazení ([složené přiřazení](expressions.md#compound-assignment)) nebo `++` jako cíl operátorů nebo `--` ( `get` [Členové funkce](expressions.md#function-members).9, [výrazy vyvolání](expressions.md#invocation-expressions)), oba přistupující objekty a `set` přístupový objekt musí existovat a být přístupný.

V následujícím příkladu je vlastnost `A.Text` skrytá vlastností `B.Text`, a to i v `set` kontextech, kde je volána pouze přístupový objekt. Naproti tomu vlastnost `B.Count` není přístupná třídě `M`, takže se místo toho použije vlastnost `A.Count` přístupná.

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

Přistupující objekt, který se používá k implementaci rozhraní, nesmí mít objekt *accessor_modifier*. Pokud je k implementaci rozhraní použit pouze jeden přistupující objekt, druhý přistupující objekt může být deklarován s *accessor_modifier*:
```csharp
public interface I
{
    string Prop { get; }
}

public class C: I
{
    public string Prop {
        get { return "April"; }       // Must not have a modifier here
        internal set {...}            // Ok, because I.Prop has no set accessor
    }
}
```

### <a name="virtual-sealed-override-and-abstract-property-accessors"></a>Virtuální, zapečetěné, přepsané a abstraktní přístupové objekty vlastnosti

Deklarace `virtual` vlastnosti určuje, že přistupující objekty vlastnosti jsou virtuální. `virtual` Modifikátor platí pro přistupující objekty vlastnosti pro čtení i zápis – není možné pouze jeden přistupující objekt vlastnosti pro čtení i zápis, který by měl být virtuální.

Deklarace `abstract` vlastnosti určuje, že přistupující objekty vlastnosti jsou virtuální, ale neposkytuje skutečnou implementaci přistupujících objektů. Místo toho jsou vyžadovány neabstraktní odvozené třídy pro zajištění vlastní implementace přístupových objektů přepsáním vlastnosti. Vzhledem k tomu, že přistupující objekt pro deklaraci abstraktní vlastnosti neposkytuje žádnou skutečnou implementaci, jeho *accessor_body* se jednoduše skládá ze střední hodnoty.

Deklarace vlastnosti, která zahrnuje `abstract` modifikátory a `override` , určuje, že vlastnost je abstraktní a Přepisuje základní vlastnost. Přístupové objekty takové vlastnosti jsou také abstraktní.

Deklarace abstraktních vlastností jsou povolené jenom v abstraktních třídách ([abstraktní třídy](classes.md#abstract-classes)). Přistupující objekty zděděné virtuální vlastnosti mohou být přepsány v odvozené třídě zahrnutím deklarace vlastnosti, která určuje `override` direktivu. Toto je známo jako ***přepsání deklarace vlastnosti***. Přepsání deklarace vlastnosti nedeklaruje novou vlastnost. Místo toho se pouze specializují implementace přístupových objektů existující virtuální vlastnosti.

Přepisující deklaraci vlastnosti musí určovat přesně stejné Modifikátory dostupnosti, typ a název jako zděděnou vlastnost. Pokud má zděděná vlastnost pouze jeden přistupující objekt (tj. Pokud je zděděná vlastnost jen pro čtení nebo jen pro zápis), překrytá vlastnost musí zahrnovat pouze tento přístupový objekt. Pokud zděděná vlastnost zahrnuje přístupové objekty (tj. Pokud je zděděná vlastnost určena pro čtení i zápis), přepsání vlastnosti může zahrnovat buď jeden přistupující objekt, nebo oba přístupové objekty.

Přepisující deklaraci vlastnosti může obsahovat `sealed` modifikátor. Použití tohoto modifikátoru zabrání odvození třídy z dalšího přepsání vlastnosti. Přístupové objekty zapečetěné vlastnosti jsou také zapečetěné.

S výjimkou rozdílů v deklaraci a syntaxi vyvolání, virtuálních, zapečetěných, přepsání a abstraktních přístupových objektů se chovají stejně jako virtuální, zapečetěné, přepisování a abstraktní metody. Konkrétně pravidla, která jsou popsána v tématu [virtuální metody](classes.md#virtual-methods), [metody přepsání](classes.md#override-methods), [zapečetěné metody](classes.md#sealed-methods)a [abstraktní metody](classes.md#abstract-methods) , se použijí, jako by přistupující objekty byly metody odpovídající formy:

*  `get` Přistupující objekt odpovídá metodě bez parametrů s návratovou hodnotou typu vlastnosti a stejnými modifikátory jako obsahující vlastnosti.
*  Přístupový objekt odpovídá metodě s parametrem s jednou hodnotou typu vlastnosti `void` , návratovým typem a stejnými modifikátory jako obsahující vlastnosti. `set`

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
`X`je virtuální vlastnost jen pro čtení, `Y` je virtuální vlastností pro čtení i zápis a `Z` je abstraktní vlastnost pro čtení i zápis. Protože `Z` je abstraktní, obsažená třída `A` musí být také deklarována jako abstraktní.

Třída, která je odvozena `A` z, je zobrazena níže:
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

Tady, deklarace `X`, `Y`a `Z` přepisují deklarace vlastností. Každá deklarace vlastnosti přesně odpovídá modifikátorům přístupnosti, typu a názvu odpovídající zděděné vlastnosti. `get` `Y` Přístupovýobjekt`base` objektu a`set`přistupující objekt k použití klíčového slova pro přístup k zděděným přístupovým objektům. `X` Deklarace `Z` přepíše jak abstraktní přistupující objekty, takže neexistují žádné zbývající členy abstraktní funkce v `B` `B` a je povoleno být Neabstraktní třída.

Je-li vlastnost deklarována jako `override`, musí být všechny přepsané přistupující objekty přístupné přepsání kódu. Kromě toho musí být deklarované přístupnosti samotné vlastnosti i indexeru a přistupující objekty shodné s tím, že přepsané členy a přistupující objekty. Příklad:
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

## <a name="events"></a>Duration

***Událost*** je člen, který umožňuje objektu nebo třídě poskytnout oznámení. Klienti mohou připojit spustitelný kód pro události tím, že dodávají ***obslužné rutiny událostí***.

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

*Event_declaration* může zahrnovat sadu *atributů* ([atributů](attributes.md)) a platnou kombinaci modifikátorů čtyř přístupu ([modifikátory přístupu](classes.md#access-modifiers)), `new` ([nový modifikátor](classes.md#the-new-modifier)), `static` ([static a instance). metody](classes.md#static-and-instance-methods)), `virtual` ([virtuální metody](classes.md#virtual-methods)), 0 ([metody přepisu](classes.md#override-methods)), 2 ([zapečetěné metody](classes.md#sealed-methods)), 4 ([abstraktní metody](classes.md#abstract-methods)) a 6 ([externí metody](classes.md#external-methods)) modifikátory.

Deklarace událostí podléhají stejným pravidlům jako deklarace metod ([metody](classes.md#methods)) s ohledem na platné kombinace modifikátorů.

Typ deklarace události musí být *typu* *delegate_type* ([odkazové typy](types.md#reference-types)) a tento *delegate_type* musí být alespoň tak přístupný jako samotná událost ([Omezení přístupnosti](basic-concepts.md#accessibility-constraints)).

Deklarace události může zahrnovat *event_accessor_declarations*. Nicméně pokud to neplatí pro neexterní, neabstraktní události, kompilátor je automaticky dodá ([události podobné poli](classes.md#field-like-events)); pro externí události jsou přístupové objekty poskytovány externě.

Deklarace události, která vynechává *event_accessor_declarations* , definuje jednu nebo více událostí – jeden pro každou *variable_declarator*. Atributy a modifikátory se vztahují na všechny členy deklarované tímto *event_declaration*.

Jedná se o chybu při kompilaci, kterou může *event_declaration* zahrnovat modifikátor `abstract` a *event_accessor_declarations*závorky s oddělovači.

Pokud deklarace události obsahuje `extern` modifikátor, událost je označována jako ***externí událost***. Vzhledem k tomu, že vnější deklarace události neposkytuje žádnou skutečnou implementaci, jedná se o chybu, aby mohla zahrnovat modifikátor `extern` a *event_accessor_declarations*.

Jedná se o chybu při kompilaci pro *variable_declarator* deklarace události s modifikátorem `abstract` nebo `external` pro zahrnutí *variable_initializer*.

Událost lze použít jako levý operand `+=` operátorů a `-=` ([přiřazení události](expressions.md#event-assignment)). Tyto operátory se používají k připojení obslužných rutin událostí k nebo k odebrání obslužných rutin událostí z události a modifikátorů přístupu ovládacího prvku události, ve kterém jsou tyto operace povoleny.

Vzhledem `+=` k `-=` tomu, že a jsou pouze operace, které jsou povoleny pro událost mimo typ, který deklaruje událost, může externí kód přidat a odebrat obslužné rutiny pro událost, ale nemůže žádným jiným způsobem získat nebo změnit základní seznam události. žádostí.

V `x += y` operaci formuláře `void` nebo `x -= y`, když `x` je událost a odkaz probíhá `x`mimo typ, který obsahuje deklaraci, výsledek operace je typu (na rozdíl od typ `x` s`x` hodnotou po přiřazení). Toto pravidlo zabrání externímu kódu v nepřímém zkoumání základního delegáta události.

Následující příklad ukazuje, jak jsou obslužné rutiny událostí připojeny k instancím `Button` třídy:
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

Zde konstruktor `Button` instance vytvoří dvě instance a připojí obslužné rutiny události k `Click` událostem. `LoginDialog`

### <a name="field-like-events"></a>Události podobné poli

V textu programu třídy nebo struktury, která obsahuje deklaraci události, mohou být některé události použity jako pole. Pro použití tímto způsobem nesmí být událost `abstract` nebo `extern` a nesmí explicitně zahrnovat *event_accessor_declarations*. Taková událost se dá použít v jakémkoli kontextu, který povoluje pole. Pole obsahuje delegáta ([Delegáti](delegates.md)), který odkazuje na seznam obslužných rutin událostí přidaných do události. Pokud nebyly přidány žádné obslužné rutiny událostí, pole obsahuje `null`.

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
`Click`slouží jako pole v rámci `Button` třídy. Jak ukazuje příklad, pole lze prozkoumat, upravit a použít ve výrazech volání delegátů. `OnClick` Metoda`Button` ve`Click` třídě vyvolá událost. Pojem vyvolání události je přesně shodný s voláním delegáta reprezentovaného událostí, takže neexistují žádné speciální jazykové konstrukce pro vyvolání událostí. Všimněte si, že volání delegáta předchází kontrolu, která zajišťuje, že delegát je jiný než null.

Mimo deklaraci `Button` třídy `Click` může být člen použit pouze na levé straně `+=` operátoru and `-=` , jako v
```csharp
b.Click += new EventHandler(...);
```
který připojí delegáta k seznamu `Click` vyvolání události a
```csharp
b.Click -= new EventHandler(...);
```
Tím se odebere delegát ze seznamu `Click` vyvolání události.

Při kompilování události podobné poli kompilátor automaticky vytvoří úložiště pro blokování delegáta a vytvoří přistupující objekty pro událost, která přidá nebo odebere obslužné rutiny události do pole delegát. Operace sčítání a odebírání jsou bezpečné pro přístup z více vláken a mohou (ale nemusí) být provedeny, když držíte zámek ([příkaz Lock](statements.md#the-lock-statement)) na obsahujícím objektu události instance nebo objekt typu ([výrazy vytváření anonymních objektů](expressions.md#anonymous-object-creation-expressions)). pro statickou událost.

Proto deklarace události instance formuláře:
```csharp
class X
{
    public event D Ev;
}
```
bude zkompilována do podobného:
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
V rámci třídy `X` `Ev` odkazy na levou stranu `+=` operátoru and `-=` způsobí vyvolání přístupových objektů Add a Remove. Všechny ostatní odkazy na `Ev` jsou kompilovány tak, aby odkazovaly na skryté pole `__Ev` namísto ([přístup k přístupu členů](expressions.md#member-access)). Název "`__Ev`" je libovolný. skryté pole může mít libovolný název nebo žádný název.

### <a name="event-accessors"></a>Přístupové objekty událostí

Deklarace událostí typicky vynechávají *event_accessor_declarations*, jak je uvedeno výše v příkladu `Button`. Jedna z těchto situací zahrnuje případ, ve kterém není přijatelné náklady na úložiště jednoho pole na každou událost. V takových případech může třída zahrnovat *event_accessor_declarations* a používat privátní mechanizmus pro ukládání seznamu obslužných rutin událostí.

*Event_accessor_declarations* události specifikuje spustitelné příkazy přidružené k přidávání a odebírání obslužných rutin událostí.

Deklarace přistupujícího objektu se skládají z *add_accessor_declaration* a *remove_accessor_declaration*. Každá deklarace přistupujícího objektu se `add` skládá `remove` z tokenu nebo následovaný *blokem*. *Blok* přidružený k *add_accessor_declaration* Určuje příkazy, které mají být provedeny při přidání obslužné rutiny události, a *blok* přidružený k *remove_accessor_declaration* Určuje příkazy, které mají být provedeny. Při odebrání obslužné rutiny události.

Každý *add_accessor_declaration* a *remove_accessor_declaration* odpovídá metodě s parametrem s jednou hodnotou typu události a návratovým typem `void`. Implicitní parametr přístupového objektu události má název `value`. V případě, že se v přiřazení události používá událost, použije se odpovídající přístupový objekt události. Konkrétně, je-li operátor `+=` přiřazení použit, je použit přistupující objekt Add a je-li `-=` operátor přiřazení, je použit přístupový objekt Remove. V obou případech je pravý operand operátoru přiřazení použit jako argument přístupového objektu události. Blok *add_accessor_declaration* nebo *remove_accessor_declaration* musí splňovat pravidla pro metody `void` popsané v [těle metody](classes.md#method-body). Konkrétně `return` příkazy v takovém bloku nejsou povoleny pro určení výrazu.

Vzhledem k tomu, že objekt pro přístup k události `value`má implicitně parametr s názvem, jedná se o chybu při kompilaci pro místní proměnnou nebo konstantu deklarovanou v přístupovém objektu události, aby měl tento název.

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
`Control` třída implementuje interní mechanismus úložiště pro události. Metoda přidruží hodnotu delegáta k klíči `GetEventHandler` , metoda vrátí delegáta, který je aktuálně přidružen k klíči, a `RemoveEventHandler` metoda odebere delegáta jako obslužnou rutinu události pro zadanou událost. `AddEventHandler` Proto je podkladový mechanismus úložiště navržený tak, že se neúčtují žádné náklady na přidružení `null` hodnoty delegáta k klíči, takže nezpracované události nevyužívají žádné úložiště.

### <a name="static-and-instance-events"></a>Statické a instance události

Pokud deklarace události obsahuje `static` modifikátor, událost je označována jako ***statická událost***. Pokud není `static` k dispozici žádný modifikátor, událost je označována jako ***událost instance***.

Statická událost není přidružena k určité instanci a jedná se o chybu při kompilaci, na kterou se odkazuje `this` v přístupových objektech statické události.

Událost instance je přidružena k dané instanci třídy a k této instanci lze přistupovat jako `this` ([přístup](expressions.md#this-access)) v přístupových objektech této události.

Pokud se na událost odkazuje v *member_access* ([členský přístup](expressions.md#member-access)) formuláře `E.M`, pokud `M` je statická událost, `E` musí znamenat typ obsahující `M`, a pokud `M` je instance událost, E musí napřed znamenat instanci typu obsahujícího `M`.

Rozdíly mezi členy static a instance jsou podrobněji popsány ve [statických a instancích členů](classes.md#static-and-instance-members).

### <a name="virtual-sealed-override-and-abstract-event-accessors"></a>Virtuální, zapečetěné, přepsání a abstraktní přístupové objekty událostí

Deklarace `virtual` události Určuje, že přistupující objekty této události jsou virtuální. `virtual` Modifikátor platí pro přistupující objekty události.

Deklarace `abstract` události Určuje, že přistupující objekty události jsou virtuální, ale neposkytuje skutečnou implementaci přistupujících objektů. Místo toho jsou vyžadovány neabstraktní odvozené třídy pro zajištění vlastní implementace přístupových objektů přepsáním události. Vzhledem k tomu, že deklarace abstraktní události neposkytuje žádnou skutečnou implementaci, nemůže poskytnout *event_accessor_declarations*oddělený závorkami.

Deklarace události, která zahrnuje `abstract` modifikátory a `override` , určuje, že událost je abstraktní a Přepisuje základní událost. Přístupové objekty takové události jsou také abstraktní.

Abstraktní deklarace událostí jsou povolené jenom v abstraktních třídách ([abstraktní třídy](classes.md#abstract-classes)).

Přistupující objekty zděděné virtuální události lze přepsat v odvozené třídě zahrnutím deklarace události, která určuje `override` modifikátor. To se označuje jako ***přepsání deklarace události***. Přepsání deklarace události nedeklaruje novou událost. Místo toho se pouze specializují implementace přístupových objektů existující virtuální události.

Přepisující deklaraci události musí určovat přesně stejné Modifikátory dostupnosti, typ a název jako přepsanou událost.

Přepisující deklaraci události může obsahovat `sealed` modifikátor. Použití tohoto modifikátoru zabrání odvození třídy v dalším přepsání události. Přístupové objekty zapečetěné události jsou také zapečetěné.

Jedná se o chybu při kompilaci, která přepisuje deklaraci události, aby zahrnovala `new` modifikátor.

S výjimkou rozdílů v deklaraci a syntaxi vyvolání, virtuálních, zapečetěných, přepsání a abstraktních přístupových objektů se chovají stejně jako virtuální, zapečetěné, přepisování a abstraktní metody. Konkrétně pravidla, která jsou popsána v tématu [virtuální metody](classes.md#virtual-methods), [metody přepisu](classes.md#override-methods), [zapečetěné metody](classes.md#sealed-methods)a [abstraktní metody](classes.md#abstract-methods) , se použijí, jako přistupující objekty byly metody odpovídající formy. Každý přistupující objekt odpovídá metodě s parametrem s jednou hodnotou typu události, `void` návratový typ a stejné modifikátory jako obsažená událost.

## <a name="indexers"></a>Indexery

***Indexer*** je člen, který umožňuje indexování objektu stejným způsobem jako pole. Indexery jsou deklarovány pomocí *indexer_declaration*s:

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

*Indexer_declaration* může zahrnovat sadu *atributů* ([atributů](attributes.md)) a platnou kombinaci modifikátorů čtyř přístupu ([modifikátory přístupu](classes.md#access-modifiers)), `new` ([nový modifikátor](classes.md#the-new-modifier)), `virtual` ([virtuální metody ](classes.md#virtual-methods)), `override` ([metody přepisu](classes.md#override-methods)), 0 ([zapečetěné metody](classes.md#sealed-methods)), 2 ([abstraktní metody](classes.md#abstract-methods)) a modifikátory 4 ([externí metody](classes.md#external-methods)).

Deklarace indexeru podléhají stejným pravidlům jako deklarace metod ([metody](classes.md#methods)) s ohledem na platné kombinace modifikátorů, s jednou výjimkou, že statický modifikátor není povolený u deklarace indexeru.

Modifikátory `virtual`, `override`a `abstract` se vzájemně vylučují, s výjimkou jednoho případu. Modifikátory `override` a mohou být použity společně, aby abstraktní indexer mohl přepsat virtuální. `abstract`

*Typ* deklarace indexer určuje typ elementu indexeru zavedeného deklarací. Pokud indexer není explicitní implementací člena rozhraní, pak je *typ* následován klíčovým slovem `this`. Pro explicitní implementaci člena rozhraní je *typu* následováno *interface_type*, "`.`" a klíčovým slovem `this`. Na rozdíl od jiných členů indexery nemají uživatelsky definované názvy.

*Formal_parameter_list* Určuje parametry indexeru. Seznam formálních parametrů indexeru odpovídá metodě ([parametrům metody](classes.md#method-parameters)), s tím rozdílem, že je nutné zadat alespoň jeden parametr a zda `ref` nejsou povoleny modifikátory parametrů a. `out`

*Typ* indexeru a každý z typů odkazovaných v *formal_parameter_list* musí být alespoň tak přístupný jako indexer samotný ([Omezení přístupnosti](basic-concepts.md#accessibility-constraints)).

*Indexer_body* může buď sestávat z ***těla přistupujícího objektu*** nebo ***textu výrazu***. V těle přístupového objektu *accessor_declarations*, který musí být uzavřen v tokenech "`{`" a "`}`", deklarujte přistupující objekty ([přístupové objekty](classes.md#accessors)) vlastnosti. Přistupující objekty určují spustitelné příkazy spojené s čtením a zápisem vlastnosti.

Tělo výrazu sestávající z "`=>`" následovaný výrazem `E` a středníkem je přesně ekvivalentní tělo `{ get { return E; } }`příkazu, a lze jej proto použít pouze k zadání indexerů pouze pro getter, kde je výsledek getter předána jediným výrazem.

I když je syntaxe pro přístup k prvku indexeru stejná jako u prvku pole, element indexeru není klasifikován jako proměnná. Proto není možné předat element indexeru jako `ref` argument or. `out`

Seznam formálních parametrů indexeru definuje podpis ([signatury a přetížení](basic-concepts.md#signatures-and-overloading)) indexeru. Konkrétně signatura indexeru se skládá z počtu a typů jeho formálních parametrů. Typ elementu a názvy formálních parametrů nejsou součástí signatury indexeru.

Signatura indexeru se musí lišit od signatur všech ostatních indexerů deklarovaných ve stejné třídě.

Indexery a vlastnosti jsou v konceptu velmi podobné, ale liší se následujícími způsoby:

*  Vlastnost je identifikována názvem, zatímco indexer je identifikován podle jeho signatury.
*  Vlastnost je přístupná prostřednictvím *simple_name* ([jednoduchých názvů](expressions.md#simple-names)) nebo *member_access* ([přístup členů](expressions.md#member-access)), zatímco element indexeru je přístupný prostřednictvím *element_access* ([přístup indexerem](expressions.md#indexer-access)).
*  Vlastnost může být `static` členem, zatímco indexer je vždy členem instance.
*  Přistupující objekt vlastnosti odpovídá metodě bez parametrů, `get` zatímco přistupující objekt indexeru odpovídá metodě se stejným formálním seznamem parametrů jako indexer. `get`
*  Přistupující objekt vlastnosti odpovídá metodě s jediným parametrem s názvem `value`, zatímco `set` přistupující objekt indexeru odpovídá metodě se stejným formálním seznamem parametrů jako indexer a další parametr. `set` název `value`.
*  Jedná se o chybu při kompilaci pro přistupující objekt indexeru pro deklaraci místní proměnné se stejným názvem jako parametr indexeru.
*  V deklaraci přepsání vlastnosti je zděděná vlastnost k ní přistupovaná `base.P`pomocí syntaxe `P` , kde je název vlastnosti. V rámci přepsání deklarace indexeru se zděděnému indexeru přistupuje pomocí syntaxe `base[E]`, kde `E` je seznam výrazů oddělených čárkami.
*  Neexistuje žádný koncept "automaticky implementovaného indexeru". Není k dispozici chyba pro neabstraktního neexterního indexeru s přístupovými objekty středník.

Kromě těchto rozdílů se všechna pravidla definovaná v [přístupových](classes.md#accessors) objektů a [automaticky implementované vlastnosti](classes.md#automatically-implemented-properties) vztahují na přistupující objekty indexeru i na přistupující objekty vlastností.

Pokud deklarace indexeru obsahuje `extern` modifikátor, indexer je označován jako ***externí indexer***. Vzhledem k tomu, že externí deklarace indexeru neposkytuje žádnou skutečnou implementaci, každá z jeho *accessor_declarations* se skládá ze střední hodnoty.

Následující příklad deklaruje `BitArray` třídu, která implementuje indexer pro přístup k jednotlivým bitům v bitovém poli.
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

Instance `BitArray` třídy spotřebovává podstatně méně paměti než odpovídající `bool[]` (vzhledem k tomu, že každá hodnota bývalého zabírá pouze jeden bit namísto jeho jednoho bajtu), ale umožňuje stejné operace jako `bool[]`.

Následující `CountPrimes` Třída`BitArray` používá k výpočtu počtu primárních operací mezi 1 a daným maximem klasický algoritmus "síto":
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

Všimněte si, že syntaxe pro přístup k elementům `BitArray` je přesně stejná jako `bool[]`u pro.

Následující příklad ukazuje třídu mřížky o 26 * 10, která má indexer se dvěma parametry. První parametr je povinný jako velké nebo malé písmeno v rozsahu A – Z a druhý musí být celé číslo v rozsahu 0-9.

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

### <a name="indexer-overloading"></a>Přetížení indexeru

Pravidla rozlišení přetížení indexeru jsou popsána v tématu [odvození typu](expressions.md#type-inference).

## <a name="operators"></a>Operátory

***Operátor*** je člen, který definuje význam operátoru výrazu, který lze použít na instance třídy. Operátory jsou deklarovány pomocí *operator_declaration*s:

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

Existují tři kategorie přetížených operátorů: Unární operátory ([unární operátory](classes.md#unary-operators)), binární operátory ([binární operátory](classes.md#binary-operators)) a operátory převodu ([operátory převodu](classes.md#conversion-operators)).

*Operator_body* je buď středník, ***text příkazu*** nebo ***tělo výrazu***. Tělo příkazu se skládá z *bloku*, který určuje příkazy, které mají být provedeny při vyvolání operátoru. *Blok* musí odpovídat pravidlům pro metody vracející hodnoty popsané v [těle metody](classes.md#method-body). Tělo výrazu se skládá `=>` za následováním výrazu a středníkem a označuje jeden výraz, který má být proveden při vyvolání operátoru.

Pro operátory `extern` se *operator_body* skládá jednoduše středník. Pro všechny ostatní operátory je *operator_body* buď blokové tělo, nebo tělo výrazu.

Následující pravidla platí pro všechny deklarace operátorů:

*  Deklarace operátoru musí zahrnovat jak `public` `static` a, tak i modifikátor.
*  Parametr (y) operátoru musí být parametry hodnoty ([parametry hodnoty](variables.md#value-parameters)). Jedná se o chybu při kompilaci pro deklaraci operátora k určení `ref` nebo `out` parametrů.
*  Signatura operátoru ([unární operátory](classes.md#unary-operators), [binární operátory](classes.md#binary-operators), [operátory převodu](classes.md#conversion-operators)) se musí lišit od signatur všech ostatních operátorů deklarovaných ve stejné třídě.
*  Všechny typy, na které je odkazováno v deklaraci operátoru, musí být alespoň tak přístupné jako samotný operátor ([Omezení přístupnosti](basic-concepts.md#accessibility-constraints)).
*  Je-li stejný modifikátor v deklaraci operátoru uveden několikrát, jedná se o chybu.

Každá kategorie operátoru ukládá další omezení, jak je popsáno v následujících částech.

Podobně jako ostatní členové jsou operátory deklarované v základní třídě děděny odvozenými třídami. Vzhledem k tomu, že deklarace operátorů vždy vyžadují třídu nebo strukturu, ve které je operátor deklarován pro účast v Signature operátoru, není možné použít operátor deklarovaný v odvozené třídě pro skrytí operátoru deklarovaného v základní třídě. `new` Proto modifikátor není nikdy vyžadován a v deklaraci operátoru není nikdy povoleno.

Další informace o unárních a binárních operátorech lze nalézt v [operátorech](expressions.md#operators).

Další informace o operátorech převodu lze nalézt v [uživatelsky definovaných převodech](conversions.md#user-defined-conversions).

### <a name="unary-operators"></a>Unární operátory

Následující pravidla platí pro deklarace unárních operátorů, `T` kde označuje typ instance třídy nebo struktury, která obsahuje deklaraci operátoru:

*  Unární `+`operátor `T` , `-`, `!` `T?` nebo musímítjedenparametrtypunebomůževracetlibovolnýtyp.`~`
*  Unární `++` operátor OR `--` musí mít jeden parametr typu `T` nebo `T?` a musí vracet stejný typ nebo z něj odvozený typ.
*  Unární `true` operátor OR `false` musí mít jeden parametr typu `T` nebo `T?` musí vracet typ `bool`.

Signatura unárního operátoru se skládá z tokenu operátoru `-`( `!``+`, `~`, `++` `--`,, `true`,, `false`nebo) a typu jednoho formálního parametru. Návratový typ není součástí signatury unárního operátoru, ani se nejedná o název formálního parametru.

Unární operátory `false` a vyžadují deklaraci párového páru. `true` K chybě v době kompilace dojde, pokud třída deklaruje jeden z těchto operátorů bez nutnosti deklarovat i druhý. Operátory `true` a`false` jsou podrobněji popsány v [uživatelsky definovaných podmíněných logických operátorech](expressions.md#user-defined-conditional-logical-operators) a [logických výrazech](expressions.md#boolean-expressions).

Následující příklad ukazuje implementaci a následné použití `operator ++` pro třídu celočíselného vektoru:
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

Všimněte si, jak metoda operátora vrací hodnotu vytvořenou přidáním 1 k operandu, stejně jako operátory přírůstku a snížení předpony (operátory přírůstku[a](expressions.md#postfix-increment-and-decrement-operators)snížení předpony) a operátory přírůstku a snížení předpony ([předpona operátory přírůstku a snížení](expressions.md#prefix-increment-and-decrement-operators)). Na rozdíl od C++, tato metoda nemusí změnit hodnotu svého operandu přímo. Ve skutečnosti Změna hodnoty operandu by narušila standardní sémantiku operátoru přírůstku.

### <a name="binary-operators"></a>Binární operátory

Následující pravidla platí pro deklarace binárních operátorů, `T` kde označuje typ instance třídy nebo struktury, která obsahuje deklaraci operátoru:

*  Binární operátor bez posunutí musí mít dva parametry, minimálně jeden z nich musí mít typ `T` nebo `T?`a může vracet libovolný typ.
*  `<<` Binární nebo `T?` `T` `int?` `int` operátor musí mít dva parametry, první z nich musí mít typ nebo a druhý z nich musí mít typ nebo a může vracet libovolný typ. `>>`

Signatura binárního operátoru se skládá z tokenu operátoru `-`( `*``+`, `/`, `%`, `&`, `|`, `^`, `<<`, `>>`,,, `==`, `!=`, ,,`>`nebo )atypydvou`<=`formálníchparametrů. `<` `>=` Návratový typ a názvy formálních parametrů nejsou součástí signatury binárního operátoru.

Některé binární operátory vyžadují párové deklarace. Pro každou deklaraci každého operátoru páru musí existovat deklarace, která odpovídá druhému operátoru. Dvě deklarace operátoru se shodují, pokud mají stejný návratový typ a stejný typ pro každý parametr. Následující operátory vyžadují párové deklarace:

*  `operator ==` a `operator !=`
*  `operator >` a `operator <`
*  `operator >=` a `operator <=`

### <a name="conversion-operators"></a>Operátory převodu

Deklarace operátoru převodu zavádí ***uživatelsky definovaný převod*** ([uživatelsky definované převody](conversions.md#user-defined-conversions)), které rozšiřují předdefinované implicitní a explicitní převody.

Deklarace operátoru převodu, která zahrnuje `implicit` klíčové slovo zavádí uživatelem definovaný implicitní převod. Implicitní převody mohou nastat v různých situacích, včetně vyvolání členů funkce, výrazů přetypování a přiřazení. Tento postup je popsán dále v tématu [implicitní převody](conversions.md#implicit-conversions).

Deklarace operátoru převodu, která zahrnuje `explicit` klíčové slovo zavádí uživatelem definovaný explicitní převod. Explicitní převody mohou nastat ve výrazech přetypování a jsou popsány dále v tématu [explicitní převody](conversions.md#explicit-conversions).

Operátor převodu se převede ze zdrojového typu určeného parametrem typu operátoru převodu na cílový typ, který je označen návratovým typem operátoru převodu.

Pro `S` daný typ zdroje a cílový typ `T`, pokud `S` nebo `T` jsou typy s možnou hodnotou `S0` null `T0` , umožňují a odkazují na jejich základní `S0` typy `T0` , jinak a je rovno `T`avuvedenémpořadí `S` . Třída nebo struktura je oprávněna deklarovat převod ze zdrojového typu `S` na cílový typ `T` pouze v případě, že jsou splněny všechny následující podmínky:

*  `S0`a `T0` jsou různé typy.
*  Buď `S0` nebo`T0` je typem třídy nebo struktury, ve které se provádí deklarace operátoru.
*  Ani `S0` ani `T0` není *INTERFACE_TYPE*.
*  S výjimkou uživatelem `S` definovaných převodů neexistuje převod z `T` na nebo z `T` na `S`.

Pro účely těchto pravidel jsou všechny parametry typu přidružené `S` k nebo `T` považovány za jedinečné typy, které nemají žádný vztah dědičnosti s jinými typy, a jakákoli omezení u těchto parametrů typu jsou ignorována.

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
první dva deklarace operátoru jsou povoleny, protože pro účely [indexerů](classes.md#indexers).3 `T` a `int` `string` v je považovány za jedinečné typy bez vztahu. Třetí operátor je však chybou, protože `C<T>` je základní `D<T>`třídou třídy.

Od druhého pravidla následuje, že operátor převodu musí převést buď na nebo z třídy nebo typu struktury, ve které je operátor deklarován. Například je `C` možné třídu nebo typ struktury definovat převod z `C` na `int` a z `int` na `C`, ale ne z `int` na `bool`.

Není možné přímo znovu definovat předem definovaný převod. Proto operátory převodu nepovolují převod z nebo na `object` , protože implicitní a explicitní převody již existují mezi `object` a všemi ostatními typy. Stejně tak, ani zdrojový ani cílový typ převodu může být základní typ druhého, protože převod by pak již existoval.

Je však možné deklarovat operátory u obecných typů, které pro konkrétní argumenty typu určují převody, které již existují jako předdefinované převody. V příkladu
```csharp
struct Convertible<T>
{
    public static implicit operator Convertible<T>(T value) {...}
    public static explicit operator T(Convertible<T> value) {...}
}
```
Pokud je `object` typ zadán jako argument typu pro `T`, druhý operátor deklaruje převod, který již existuje (implicitní, a tedy také explicitní, převod existuje z libovolného typu na typ `object`).

V případech, kdy existuje předem definovaný převod mezi dvěma typy, jsou ignorovány uživatelsky definované převody mezi těmito typy. Určen

*  Pokud existuje předem definovaný implicitní převod ([implicitní převody](conversions.md#implicit-conversions)) `S` z typu na typ `T`, všechny uživatelsky definované převody (implicitní nebo explicitní) z `S` na `T` jsou ignorovány.
*  Pokud existuje předem definovaný explicitní převod ([explicitní převody](conversions.md#explicit-conversions)) `S` z typu na typ `T`, všechny uživatelem definované explicitní převody z `S` na `T` jsou ignorovány. Mimoto

Pokud `T` je typ rozhraní, uživatelem definované implicitní převody z `S` na `T` jsou ignorovány.

V opačném případě uživatelem definované implicitní převody `S` z `T` na na jsou stále považovány za.

Pro všechny typy, `object`ale operátory deklarované `Convertible<T>` výše uvedeným typem nejsou v konfliktu s předem definovanými převody. Příklad:
```csharp
void F(int i, Convertible<int> n) {
    i = n;                          // Error
    i = (int)n;                     // User-defined explicit conversion
    n = i;                          // User-defined implicit conversion
    n = (Convertible<int>)i;        // User-defined implicit conversion
}
```

Nicméně u typů `object`, předem definovaných převodů skryjte uživatelsky definované převody ve všech případech, ale jednu z těchto hodnot:

```csharp
void F(object o, Convertible<object> n) {
    o = n;                         // Pre-defined boxing conversion
    o = (object)n;                 // Pre-defined boxing conversion
    n = o;                         // User-defined implicit conversion
    n = (Convertible<object>)o;    // Pre-defined unboxing conversion
}
```

Uživatelem definované převody nejsou povoleny pro převod z nebo na *INTERFACE_TYPE*s. Konkrétně toto omezení zajistí, že při převodu na *INTERFACE_TYPE*nedochází k žádným uživatelsky definovaným transformacím a převod na *INTERFACE_TYPE* se zdaří jenom v případě, že převedený objekt ve skutečnosti implementuje. zadaný *INTERFACE_TYPE*.

Signatura operátoru převodu se skládá ze zdrojového typu a cílového typu. (Všimněte si, že se jedná o jedinou formu člena, pro který se v signatuře účastní návratový typ.) Klasifikace `implicit` nebo`explicit` operátor převodu není součástí signatury operátoru. Třída nebo struktura proto nemůže deklarovat `implicit` operátor `explicit` převodu a operátor převodu se stejnými zdrojovými a cílovými typy.

Obecně platí, že uživatelsky definované implicitní převody by měly být navržené tak, aby nikdy nevolaly výjimky a nikdy neztratily informace. Pokud uživatelsky definovaný převod může vést k výjimkám (například protože zdrojový argument je mimo rozsah) nebo ztratil informace (například zahození vysokého počtu bitů), pak tento převod by měl být definován jako explicitní převod.

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
`Digit` převod z `Digit` na `byte` je implicitní, protože nikdy nevyvolává výjimky nebo ztratí informace, ale převod z `byte` na `Digit` je explicitní, protože může představovat pouze podmnožinu možných `byte`hodnoty.

## <a name="instance-constructors"></a>Konstruktory instancí

***Konstruktor instance*** je člen, který implementuje akce vyžadované pro inicializaci instance třídy. Konstruktory instancí jsou deklarovány pomocí *constructor_declaration*s:

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

*Constructor_declaration* může zahrnovat sadu *atributů* ([atributů](attributes.md)), platnou kombinaci modifikátorů přístupu ([modifikátory přístupu](classes.md#access-modifiers)) a modifikátor `extern` ([externí metody](classes.md#external-methods)). Deklarace konstruktoru nemůže zahrnovat stejný modifikátor víckrát.

*Identifikátor* *constructor_declarator* musí pojmenovat třídu, ve které je deklarován konstruktor instance. Pokud je zadán jiný název, dojde k chybě při kompilaci.

Volitelná *formal_parameter_list* konstruktoru instance podléhá stejným pravidlům jako *Formal_parameter_list* metody ([metody](classes.md#methods)). Seznam formálních parametrů definuje podpis ([signatury a přetížení](basic-concepts.md#signatures-and-overloading)) konstruktoru instance a řídí proces, při kterém řešení přetížení ([odvození typu](expressions.md#type-inference)) vybere konkrétní konstruktor instance ve volání.

Každý typ odkazovaný v *formal_parameter_list* konstruktoru instance musí být alespoň tak přístupný jako samotný konstruktor ([Omezení přístupnosti](basic-concepts.md#accessibility-constraints)).

Nepovinná *constructor_initializer* určuje jiný konstruktor instance, který se má vyvolat před spuštěním příkazů uvedených v *constructor_body* tohoto konstruktoru instance. Tento postup je popsán dále v [inicializátorech konstruktoru](classes.md#constructor-initializers).

Když deklarace konstruktoru obsahuje `extern` modifikátor, konstruktor je označován jako ***externí konstruktor***. Vzhledem k tomu, že deklarace externího konstruktoru neposkytuje žádnou skutečnou implementaci, její *constructor_body* se skládá ze střední hodnoty. Pro všechny ostatní konstruktory se *constructor_body* skládá z *bloku* , který určuje příkazy pro inicializaci nové instance třídy. To odpovídá přesně *bloku* metody instance s `void` návratovým typem ([tělo metody](classes.md#method-body)).

Konstruktory instancí nejsou děděny. Třída proto nemá žádné konstruktory instancí jiné než ty, které jsou ve skutečnosti deklarovány ve třídě. Pokud třída neobsahuje deklarace konstruktoru instance, je automaticky poskytnut výchozí konstruktor instance ([výchozí konstruktory](classes.md#default-constructors)).

Konstruktory instancí jsou vyvolány pomocí *object_creation_expression*s ([výrazy vytváření objektů](expressions.md#object-creation-expressions)) a prostřednictvím *constructor_initializer*s.

### <a name="constructor-initializers"></a>Inicializátory konstruktoru

Všechny konstruktory instancí (kromě těch pro třídu `object`) implicitně zahrnují vyvolání jiného konstruktoru instance těsně před *constructor_body*. Konstruktor pro implicitní vyvolání je určen *constructor_initializer*:

*  Inicializátor konstruktoru instance formuláře `base(argument_list)` nebo `base()` způsobí vyvolání konstruktoru instance z přímé základní třídy. Tento konstruktor je vybrán pomocí *argument_list* , pokud je k dispozici, a pravidla rozlišení přetížení pro [rozlišení přetěžování](expressions.md#overload-resolution). Sada konstruktorů instance kandidátů se skládá ze všech přístupných konstruktorů instancí obsažených v přímé základní třídě nebo výchozího konstruktoru ([výchozí konstruktory](classes.md#default-constructors)), pokud nejsou deklarovány žádné konstruktory instancí v přímé základní třídě. Pokud je tato sada prázdná nebo pokud nelze identifikovat jeden nejlepší konstruktor instance, dojde k chybě při kompilaci.
*  Inicializátor konstruktoru instance formuláře `this(argument-list)` nebo `this()` způsobí vyvolání konstruktoru instance ze samotné třídy. Konstruktor je vybrán pomocí *argument_list* , pokud je k dispozici, a pravidla rozlišení přetížení pro [rozlišení přetěžování](expressions.md#overload-resolution). Sada konstruktorů instancí kandidátů se skládá ze všech přístupných konstruktorů instancí deklarovaných ve třídě samotné. Pokud je tato sada prázdná nebo pokud nelze identifikovat jeden nejlepší konstruktor instance, dojde k chybě při kompilaci. Pokud deklarace konstruktoru instance obsahuje inicializátor konstruktoru, který vyvolá samotný konstruktor, dojde k chybě při kompilaci.

Pokud konstruktor instance nemá inicializátor konstruktoru, implicitně se poskytne inicializátor konstruktoru formuláře `base()` . Proto deklarace konstruktoru instance formuláře
```csharp
C(...) {...}
```
je přesně stejný jako
```csharp
C(...): base() {...}
```

Rozsah parametrů předaných *formal_parameter_list* deklarace konstruktoru instance zahrnuje inicializátor konstruktoru této deklarace. Proto inicializátor konstruktoru má povolen přístup k parametrům konstruktoru. Příklad:
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

Inicializátor konstruktoru instance nemá přístup k instanci, kterou vytváříte. Proto se jedná o chybu při kompilaci na odkaz `this` ve výrazu argumentu inicializátoru konstruktoru, jako je chyba při kompilaci pro výraz argumentu na odkazování na člen instance prostřednictvím *simple_name*.

### <a name="instance-variable-initializers"></a>Inicializátory proměnných instance

Pokud konstruktor instance nemá inicializátor konstruktoru, nebo má inicializátor konstruktoru ve formátu `base(...)`, tento konstruktor implicitně provede inicializace určené v *variable_initializer*s polí instance, která jsou deklarována v jeho třída. To odpovídá sekvenci přiřazení, která je provedena ihned po vstupu do konstruktoru a před implicitním voláním konstruktoru přímé základní třídy. Inicializátory proměnných jsou spouštěny v textovém pořadí, ve kterém jsou uvedeny v deklaraci třídy.

### <a name="constructor-execution"></a>Spuštění konstruktoru

Inicializátory proměnných jsou transformovány na příkazy přiřazení a tyto příkazy přiřazení jsou spuštěny před voláním konstruktoru instance základní třídy. Toto řazení zajišťuje, aby všechna pole instance byla inicializována svými Inicializátory proměnných před všemi příkazy, které mají přístup k této instanci.

Uvedený příklad
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
Když `new B()` se používá k vytvoření `B`instance, je vytvořen následující výstup:
```console
x = 1, y = 0
```

Hodnota `x` je 1, protože inicializátor proměnné je proveden před vyvoláním konstruktoru instance základní třídy. Hodnota `y` je však 0 (výchozí hodnota `int`), protože přiřazení k `y` není provedeno až po návratu konstruktoru základní třídy.

Je vhodné si představit Inicializátory proměnných instancí a Inicializátory konstruktoru jako příkazy, které jsou automaticky vloženy před *constructor_body*. Příklad
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
obsahuje několik inicializátorů proměnných; obsahuje také Inicializátory konstruktoru obou tvarů (`base` a `this`). Příklad odpovídá kódu uvedenému níže, kde každý komentář indikuje automaticky vložený příkaz (syntaxe použitá pro automaticky vložená volání konstruktoru není platná, ale slouží pouze k ilustraci mechanismu).

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

Pokud třída neobsahuje deklarace konstruktoru instance, je automaticky poskytnut výchozí konstruktor instance. Tento výchozí konstruktor jednoduše vyvolá konstruktor bez parametrů přímé základní třídy. Je-li třída abstraktní, je deklarovaná přístupnost pro výchozí konstruktor chráněna. V opačném případě je deklarovaná přístupnost pro výchozí konstruktor veřejná. Proto výchozí konstruktor je vždy ve tvaru

```csharp
protected C(): base() {}
```
nebo
```csharp
public C(): base() {}
```
kde `C` je název třídy. Pokud řešení přetížení nemůže určit jedinečný nejlepší kandidát pro inicializátor konstruktoru základní třídy, dojde k chybě při kompilaci.

V příkladu
```csharp
class Message
{
    object sender;
    string text;
}
```
k dispozici je výchozí konstruktor, protože třída neobsahuje žádné deklarace konstruktoru instance. Proto je příklad přesně ekvivalentní
```csharp
class Message
{
    object sender;
    string text;

    public Message(): base() {}
}
```

### <a name="private-constructors"></a>Soukromé konstruktory

Když třída `T` deklaruje pouze konstruktory privátní instance, není možné použít třídy mimo `T` text programu pro odvození z `T` nebo přímo vytvářet instance `T`. Proto pokud třída obsahuje pouze statické členy a není určena pro vytvoření instance, přidání prázdného konstruktoru soukromé instance zabrání vytvoření instance. Příklad:
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

`Trig` Třídy seskupují související metody a konstanty, ale nejsou určeny pro vytvoření instance. Proto deklaruje jeden prázdný konstruktor privátní instance. Pro potlačení automatické generace výchozího konstruktoru musí být deklarován alespoň jeden konstruktor instance.

### <a name="optional-instance-constructor-parameters"></a>Volitelné parametry konstruktoru instance

`this(...)` Tvar inicializátoru konstruktoru se obvykle používá ve spojení s přetížením pro implementaci volitelných parametrů konstruktoru instance. V příkladu
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
první dva konstruktory instance pouze poskytují výchozí hodnoty pro chybějící argumenty. Oba používají `this(...)` inicializátor konstruktoru k vyvolání třetího konstruktoru instance, který ve skutečnosti provádí inicializaci nové instance. Výsledkem je, že volitelné parametry konstruktoru:
```csharp
Text t1 = new Text();                    // Same as Text(0, 0, null)
Text t2 = new Text(5, 10);               // Same as Text(5, 10, null)
Text t3 = new Text(5, 20, "Hello");
```

## <a name="static-constructors"></a>Statické konstruktory

***Statický konstruktor*** je člen, který implementuje akce vyžadované k inicializaci typu uzavřené třídy. Statické konstruktory jsou deklarovány pomocí *static_constructor_declaration*s:

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

*Static_constructor_declaration* může obsahovat sadu *atributů* ([atributů](attributes.md)) a modifikátor `extern` ([externí metody](classes.md#external-methods)).

*Identifikátor* *static_constructor_declaration* musí pojmenovat třídu, ve které je deklarován statický konstruktor. Pokud je zadán jiný název, dojde k chybě při kompilaci.

Pokud deklarace statického konstruktoru obsahuje `extern` modifikátor, statický konstruktor je označován jako ***externí statický konstruktor***. Vzhledem k tomu, že deklarace externích statických konstruktorů neposkytuje žádnou skutečnou implementaci, její *static_constructor_body* se skládá ze střední hodnoty. Pro všechny ostatní deklarace statického konstruktoru se *static_constructor_body* skládá z *bloku* , který určuje příkazy, které mají být provedeny, aby bylo možné třídu inicializovat. To odpovídá přesně *method_body* statické metody s návratovým typem `void` ([tělo metody](classes.md#method-body)).

Statické konstruktory nejsou zděděné a nelze je volat přímo.

Statický konstruktor pro uzavřený typ třídy se v dané doméně aplikace provede maximálně jednou. Provedení statického konstruktoru je aktivováno prvním z následujících událostí, které se mají provést v rámci domény aplikace:

*  Vytvoří se instance typu třídy.
*  Je odkazováno na všechny statické členy typu třídy.

Pokud třída obsahuje `Main` metodu ([spuštění aplikace](basic-concepts.md#application-startup)), ve které začíná spuštění, statický konstruktor pro tuto `Main` třídu se spustí před voláním metody.

Pro inicializaci nového typu uzavřené třídy je nejprve vytvořena nová sada statických polí ([statická a pole instance](classes.md#static-and-instance-fields)) pro tento konkrétní uzavřený typ. Každé statické pole je inicializováno na výchozí hodnotu ([výchozí hodnoty](variables.md#default-values)). Dále jsou pro Tato statická pole spouštěny Inicializátory statických polí ([inicializace statických polí](classes.md#static-field-initialization)). Nakonec se spustí statický konstruktor.

Příklad
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
musí mít výstup:
```console
Init A
A.F
Init B
B.F
```
vzhledem `A`k tomu `A.F` ,že`B.F`provádění statického konstruktoru je aktivováno voláním a spuštění statickéhokonstruktorujeaktivovánovolánímmetody.`B`

Je možné vytvořit kruhové závislosti, které umožňují, aby statická pole s Inicializátory proměnných byla pozorována ve svém výchozím stavu.

Příklad
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
```console
X = 1, Y = 2
```

Chcete-li `Main` spustit metodu, systém nejprve spustí inicializátor pro `B.Y`, před statickým konstruktorem třídy `B`. `Y`způsobují `A`, že se má spustit statický konstruktor inicializátoru, protože se `A.X` na něj odkazuje hodnota. Statický konstruktor třídy `A` dále pokračuje na výpočet `X`hodnoty a při tom načítá výchozí hodnotu `Y`, která je nula. `A.X`je tedy inicializována na hodnotu 1. Proces spuštění `A`inicializátorů statických polí a statického konstruktoru se pak dokončí a vrátí se do výpočtu počáteční `Y`hodnoty, výsledek, který se bude 2.

Vzhledem k tomu, že je statický konstruktor proveden právě jednou pro každý uzavřený typ třídy, je to vhodné místo pro vynutit kontroly za běhu u parametru typu, který nelze zkontrolovat v době kompilace prostřednictvím omezení ([omezení parametrů typu](classes.md#type-parameter-constraints)). . Například následující typ používá statický konstruktor k vymáhání, že argument typu je výčet:
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

***Destruktor*** je člen, který implementuje akce vyžadované k destrukcií instance třídy. Destruktor je deklarovaný pomocí *destructor_declaration*:

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

*Destructor_declaration* může obsahovat sadu *atributů* ([atributů](attributes.md)).

*Identifikátor* *destructor_declaration* musí pojmenovat třídu, ve které je destruktor deklarovaný. Pokud je zadán jiný název, dojde k chybě při kompilaci.

Když deklarace destruktoru obsahuje `extern` modifikátor, destruktor je označován jako ***externí destruktor***. Vzhledem k tomu, že deklarace externího destruktoru neposkytuje žádnou skutečnou implementaci, její *destructor_body* se skládá ze střední hodnoty. Pro všechny ostatní destruktory se *destructor_body* skládá z *bloku* , který určuje příkazy, které mají být provedeny, aby destrukci instanci třídy. *Destructor_body* odpovídá přesně *method_body* metody instance s návratovým typem `void` ([tělo metody](classes.md#method-body)).

Destruktory nejsou děděny. Třída proto nemá žádné destruktory jiné než ta, která může být deklarována v této třídě.

Vzhledem k tomu, že destruktor musí mít žádné parametry, nemůže být přetížený, takže třída může mít maximálně jeden destruktor.

Destruktory jsou vyvolány automaticky a nelze je vyvolat explicitně. Instance se stane oprávněným zničením, pokud již není možné pro použití této instance žádným kódem. Spuštění destruktoru pro instanci může nastat kdykoli poté, co se instance stává nárokem na zničení. Když je instance destrukturovaná, jsou volány destruktory v řetězu dědičnosti dané instance, od největší odvozené k nejméně odvozené. Destruktor může být proveden na jakémkoli vlákně. Další diskuzi o pravidlech, která určují, kdy a jak se má destruktor spustit, najdete v tématu [Automatická správa paměti](basic-concepts.md#automatic-memory-management).

Výstup příkladu
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
vzhledem k tomu, že destruktory v řetězu dědičnosti jsou volány v pořadí, od největší odvozené k nejméně odvozenému.

Destruktory jsou implementovány přepsáním virtuální metody `Finalize` na `System.Object`. C#programy nemají oprávnění přepsat tuto metodu nebo je volat (nebo potlačením) přímo. Např. program
```csharp
class A 
{
    override protected void Finalize() {}    // error

    public void F() {
        this.Finalize();                     // error
    }
}
```
obsahuje dvě chyby.

Kompilátor se chová, jako by tato metoda a přepsání neexistovaly vůbec. Proto tento program:
```csharp
class A 
{
    void Finalize() {}                            // permitted
}
```
je platný a metoda zobrazuje skryté `System.Object` `Finalize` metody.

Diskuzi o chování při vyvolání výjimky z destruktoru naleznete v tématu [jak jsou zpracovávány výjimky](exceptions.md#how-exceptions-are-handled).

## <a name="iterators"></a>Iterátory

Člen funkce ([členy funkce](expressions.md#function-members)) implementovaný pomocí bloku iterátoru ([bloky](statements.md#blocks)) se nazývá ***iterátor***.

Blok iterátoru lze použít jako tělo členu funkce, pokud je návratový typ odpovídajícího členu funkce jedním z rozhraní enumerátoru ([rozhraní enumerátorů](classes.md#enumerator-interfaces)) nebo jednoho ze vyčíslitelné rozhraní ([vyčíslitelné](classes.md#enumerable-interfaces)rozhraní). . Může dojít jako *method_body*, *operator_body* nebo *accessor_body*, zatímco události, konstruktory instancí, statické konstruktory a destruktory nelze implementovat jako iterátory.

Pokud je člen funkce implementován pomocí bloku iterátoru, jedná se o chybu při kompilaci pro seznam formálních parametrů člena funkce k určení `ref` parametrů nebo. `out`

### <a name="enumerator-interfaces"></a>Rozhraní enumerátorů

***Rozhraní enumerátoru*** jsou neobecné rozhraní `System.Collections.IEnumerator` a všechny instance obecného rozhraní. `System.Collections.Generic.IEnumerator<T>` V zkrácení v této kapitole jsou tato rozhraní odkazována jako `IEnumerator` a `IEnumerator<T>`v uvedeném pořadí.

### <a name="enumerable-interfaces"></a>Výčtová rozhraní

***Výčtová rozhraní*** jsou neobecné rozhraní `System.Collections.IEnumerable` a všechny instance obecného rozhraní `System.Collections.Generic.IEnumerable<T>`. V zkrácení v této kapitole jsou tato rozhraní odkazována jako `IEnumerable` a `IEnumerable<T>`v uvedeném pořadí.

### <a name="yield-type"></a>Typ yield

Iterátor vytvoří sekvenci hodnot, všechny stejného typu. Tento typ se nazývá ***typ yield*** iterátoru.

*  Typ yield iterátoru, který vrací `IEnumerator` nebo `IEnumerable` je `object`.
*  Typ yield iterátoru, který vrací `IEnumerator<T>` nebo `IEnumerable<T>` je `T`.

### <a name="enumerator-objects"></a>Čítače objektů

Když člen funkce vracející typ rozhraní enumerátoru je implementován pomocí bloku iterátoru, volání členu funkce nespustí okamžitě kód v bloku iterátoru. Místo toho se vytvoří a vrátí ***objekt enumerátoru*** . Tento objekt zapouzdřuje kód zadaný v bloku iterátoru a spuštění kódu v bloku iterátoru nastane, pokud je vyvolána `MoveNext` metoda objektu Enumerator. Objekt enumerátoru má následující vlastnosti:

*  Implementuje `IEnumerator` a `IEnumerator<T>`, kde`T` je typ yield iterátoru.
*  Implementuje `System.IDisposable`.
*  Inicializuje se pomocí kopie hodnot argumentů (pokud existuje) a hodnoty instance předané členu funkce.
*  Má čtyři možné stavy, ***před***, po ***spuštění***, ***pozastavení***a ***po***a je zpočátku ve stavu ***před*** .

Objekt enumerátor je obvykle instancí třídy výčtu generované kompilátorem, která zapouzdřuje kód v bloku iterátoru a implementuje rozhraní enumerátorů, ale je možné použít i jiné metody implementace. Pokud je třída enumerátoru generována kompilátorem, tato třída bude vnořena, přímo nebo nepřímo, ve třídě obsahující člena funkce, bude mít privátní přístupnost a bude mít název vyhrazený pro použití kompilátorem ([identifikátory](lexical-structure.md#identifiers)).

Objekt enumerátoru může implementovat více rozhraní, než je uvedeno výše.

V následujících částech jsou popsány přesné chování `MoveNext`, `Current`a v `Dispose` implementacích rozhraní `IEnumerable` a `IEnumerable<T>` , které jsou k dispozici v objektu Enumerator.

Všimněte si, že objekty enumerátoru tuto `IEnumerator.Reset` metodu nepodporují. Vyvolání této metody způsobí, že `System.NotSupportedException` bude vyvolána výjimka.

#### <a name="the-movenext-method"></a>Metoda MoveNext

`MoveNext` Metoda objektu Enumerator zapouzdřuje kód bloku iterátoru. Vyvolání `MoveNext` metody spustí kód v bloku iterátoru a podle potřeby `Current` nastaví vlastnost objektu Enumerator. Přesná akce prováděná `MoveNext` v závislosti na stavu objektu enumerátoru, když `MoveNext` je vyvolána:

*  Pokud je stav objektu enumerátoru ***před***, vyvolání `MoveNext`:
   * Změní stav na ***spuštěno***.
   * Inicializuje parametry (včetně `this`) bloku iterátoru na hodnoty argumentů a hodnotu instance uloženou při inicializaci objektu Enumerator.
   * Spustí blok iterátoru od začátku až do přerušení provádění (jak je popsáno níže).
*  Pokud je ***spuštěn***stav objektu Enumerator, není výsledek vyvolání `MoveNext` uveden.
*  Pokud je stav objektu enumerátoru ***pozastaveno***, volání `MoveNext`:
   * Změní stav na ***spuštěno***.
   * Obnoví hodnoty všech místních proměnných a parametrů (včetně tohoto) do hodnot uložených při posledním pozastavení běhu bloku iterátoru. Všimněte si, že obsah všech objektů, na které tyto proměnné odkazovaly, se mohou od předchozího volání metody MoveNext změnit.
   * Pokračuje v provádění bloku iterátoru hned po `yield return` příkazu, který způsobil pozastavení provádění, a pokračuje, dokud nebude provádění přerušeno (jak je popsáno níže).
*  Pokud je stav objektu enumerátor ***po***, volání `MoveNext` funkce vrátí `false`.


Když `MoveNext` se spustí blok iterátoru, provádění se dá přerušit čtyřmi způsoby: `yield return` Pomocí příkazu `yield break` , pomocí příkazu, narazí na konec bloku iterátoru a vyvolala se výjimka a rozšíří se z bloku iterátoru.

*  Při zjištění příkazu ([příkaz yield):](statements.md#the-yield-statement) `yield return`
   * Výraz uvedený v příkazu je vyhodnocen, implicitně převeden na typ yield a přiřazený `Current` vlastnosti objektu enumerátor.
   * Provádění textu iterátoru je pozastaveno. Hodnoty všech místních proměnných a parametrů (včetně `this`) jsou uloženy, stejně jako umístění tohoto `yield return` příkazu. Pokud je `try` `finally` příkaz v jednom nebo více blocích, přidružené bloky nejsou v tuto chvíli spuštěny. `yield return`
   * Stav objektu enumerátoru je změněn na ***pozastaveno***.
   * Metoda se vrátí `true` volajícímu, což značí, že iteraci se úspěšně pokročila na další hodnotu. `MoveNext`
*  Při zjištění příkazu ([příkaz yield):](statements.md#the-yield-statement) `yield break`
   * Pokud je `try` `finally` příkaz v jednom nebo více blocích, jsou spuštěny přidružené bloky. `yield break`
   * Stav objektu enumerátoru se změní na ***po***.
   * Metoda se vrátí `false` volajícímu, což značí, že je iterace dokončená. `MoveNext`
*  Při zjištění konce textu iterátoru:
   * Stav objektu enumerátoru se změní na ***po***.
   * Metoda se vrátí `false` volajícímu, což značí, že je iterace dokončená. `MoveNext`
*  Když je vyvolána výjimka a šířena mimo blok iterátoru:
   * Příslušné `finally` bloky v těle iterátoru budou provedeny při šíření výjimky.
   * Stav objektu enumerátoru se změní na ***po***.
   * Šíření výjimky pokračuje volajícím `MoveNext` metody.

#### <a name="the-current-property"></a>Aktuální vlastnost

`Current` Vlastnost objektu enumerátoru je `yield return` ovlivněna příkazy v bloku iterátoru.

Když je objekt enumerátoru v ***pozastaveném*** stavu, hodnota `Current` je hodnota nastavená předchozím voláním metody `MoveNext`. Pokud je objekt enumerátoru v ***před***, ***spuštěný***nebo ***po*** stavech, není výsledek přístupu `Current` uveden.

Pro iterátory s typem yield, který je `object`jiný než, výsledek přístupu `Current` `IEnumerable` přes implementaci objektu enumerátoru odpovídá přístupu `Current` prostřednictvím objektu `IEnumerator<T>` enumerátoru. implementace a přetypování výsledku do `object`.

#### <a name="the-dispose-method"></a>Metoda Dispose

Metoda slouží k vyčištění iterace tím, že se objekt enumerátoru ***po stavu po.*** `Dispose`

*  Pokud je stav objektu enumerátoru ***před***, volání `Dispose` změní stav na ***po***.
*  Pokud je ***spuštěn***stav objektu Enumerator, není výsledek vyvolání `Dispose` uveden.
*  Pokud je stav objektu enumerátoru ***pozastaveno***, volání `Dispose`:
   * Změní stav na ***spuštěno***.
   * Provede všechny bloky finally, jako by byl příkaz `yield return` naposledy spuštěn. `yield break` V případě, že dojde k výjimce a šíření výjimky z těla iterátoru, je stav objektu enumerátoru nastaven na hodnotu ***po*** a výjimka je šířena volajícímu `Dispose` metody.
   * Změní stav na ***po***.
*  Pokud je stav objektu enumerátor ***po***, vyvolání `Dispose` nemá žádný vliv.

### <a name="enumerable-objects"></a>Vyčíslitelné objekty

Když člen funkce vracející Výčtový typ rozhraní je implementován pomocí bloku iterátoru, volání členu funkce nespustí okamžitě kód v bloku iterátoru. Místo toho je vytvořen a vrácen ***vyčíslitelné objekt*** . `GetEnumerator` Metoda vyčíslitelného objektu vrátí objekt enumerátoru, který zapouzdřuje kód zadaný v bloku iterátoru, a spuštění kódu v bloku iterátoru nastane, když je vyvolána `MoveNext` metoda objektu Enumerator. Vyčíslitelné objekty mají následující vlastnosti:

*  Implementuje `IEnumerable` a `IEnumerable<T>`, kde`T` je typ yield iterátoru.
*  Inicializuje se pomocí kopie hodnot argumentů (pokud existuje) a hodnoty instance předané členu funkce.

Vyčíslitelné objekt je obvykle instancí výčtové třídy generované kompilátorem, která zapouzdřuje kód v bloku iterátoru a implementuje Výčtová rozhraní, ale mohou být k dispozici jiné metody implementace. Pokud je Výčtová třída generována kompilátorem, tato třída bude vnořena, přímo nebo nepřímo, ve třídě obsahující člena funkce, bude mít privátní přístupnost a bude mít název vyhrazený pro použití kompilátorem ([identifikátory](lexical-structure.md#identifiers)).

Vyčíslitelné objekty mohou implementovat více rozhraní, než je uvedeno výše. Konkrétně může implementovat `IEnumerator` také vyčíslitelné objekt a `IEnumerator<T>`jeho povolení sloužit jako vyčíslitelné i enumerátor. V tomto typu implementace při prvním vyvolání `GetEnumerator` metody vyčíslitelného objektu je vrácen vyčíslitelné samotný objekt. Následná vyvolání vyčíslitelného objektu `GetEnumerator`, pokud nějaký objekt, vrátí kopii vyčíslitelného objektu. Proto každý vrácený enumerátor má svůj vlastní stav a změny v jednom enumerátoru nebudou mít vliv na další.

#### <a name="the-getenumerator-method"></a>Metoda GetEnumerator

Vyčíslitelné objekty poskytují implementaci `GetEnumerator` metod `IEnumerable` rozhraní a `IEnumerable<T>` . Tyto dvě `GetEnumerator` metody sdílejí společnou implementaci, která získává a vrací dostupný objekt enumerátoru. Objekt enumerátor je inicializován s hodnotami argumentů a hodnotou instance uloženou při inicializaci vyčíslitelného objektu, ale v opačném případě funkce objektu Enumerator funguje tak, jak je popsáno v tématu [čítače objektů](classes.md#enumerator-objects).

### <a name="implementation-example"></a>Příklad implementace

Tato část popisuje možnou implementaci iterátorů z pohledu standardních C# konstrukcí. Zde popsaná implementace je založena na stejných zásadách, které používá kompilátor společnosti C# Microsoft, ale nejedná se o udělenou implementaci nebo o jedinou možnost.

Následující `Stack<T>` třída implementuje svou `GetEnumerator` metodu pomocí iterátoru. Iterátor vytvoří výčet prvků zásobníku v horním pořadí.

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

`GetEnumerator` Metodu lze přeložit do instance třídy enumerátor generovaných kompilátorem, která zapouzdřuje kód do bloku iterátoru, jak je znázorněno v následujícím příkladu.

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

V předchozím překladu je kód v bloku iterátoru převeden na Stavový počítač a umístěn do `MoveNext` metody třídy Enumerator. Kromě toho je místní proměnná `i` převedena do pole v objektu Enumerator, aby mohla nadále existovat ve voláních `MoveNext`.

V následujícím příkladu je vytištěna Jednoduchá tabulka násobení celých čísel od 1 do 10. `FromTo` Metoda v příkladu vrátí vyčíslitelného objektu a je implementována pomocí iterátoru.

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

`FromTo` Metodu lze přeložit do instance výčtové třídy generované kompilátorem, která zapouzdřuje kód do bloku iterátoru, jak je znázorněno v následujícím příkladu.

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

Vyčíslitelné třída implementuje vyčíslitelné rozhraní i rozhraní enumerátoru a umožňuje tak, aby sloužila jako vyčíslitelné i enumerátor. Při prvním `GetEnumerator` vyvolání metody se vrátí vyčíslitelné samotný objekt. Následná vyvolání vyčíslitelného objektu `GetEnumerator`, pokud nějaký objekt, vrátí kopii vyčíslitelného objektu. Proto každý vrácený enumerátor má svůj vlastní stav a změny v jednom enumerátoru nebudou mít vliv na další. `Interlocked.CompareExchange` Metoda se používá k zajištění operace bezpečné pro přístup z více vláken.

Parametry `from` a`to` jsou přeměněny na pole v vyčíslitelné třídě. Vzhledem `from` k tomu, že je v bloku iterátoru `__from` upraveno, je zavedeno další pole, `from` které bude uchovávat počáteční hodnotu zadanou v každém enumerátoru.

Metoda vyvolá výjimku `InvalidOperationException` , pokud je volána, když `__state` je `0`. `MoveNext` To chrání před použitím vyčíslitelného objektu jako objektu enumerátoru bez prvního volání `GetEnumerator`.

Následující příklad ukazuje jednoduchou stromovou třídu. Třída implementuje svou `GetEnumerator` metodu pomocí iterátoru. `Tree<T>` Iterátor vytvoří výčet prvků stromu v vpony pořadí.

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

`GetEnumerator` Metodu lze přeložit do instance třídy enumerátor generovaných kompilátorem, která zapouzdřuje kód do bloku iterátoru, jak je znázorněno v následujícím příkladu.

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

Kompilátor generovaný dočasné objekty použitý v `foreach` příkazech se přenese `__left` do polí a `__right` objektu Enumerator. Pole objektu Enumerator je pečlivě aktualizováno, aby se správná `Dispose()` metoda volala správně, pokud je vyvolána výjimka. `__state` Všimněte si, že není možné zapisovat přeložený kód pomocí jednoduchých `foreach` příkazů.

## <a name="async-functions"></a>Asynchronní funkce

Metoda ([metody](classes.md#methods)) nebo anonymní funkce ([výrazy anonymní funkce](expressions.md#anonymous-function-expressions) `async` ) s modifikátorem se nazývá ***asynchronní funkce***. Obecně se pojem ***Async*** používá k popisu libovolného druhu funkce, která má `async` modifikátor.

Jedná se o chybu při kompilaci pro seznam formálních parametrů asynchronní funkce pro určení `ref` parametrů nebo. `out`

Typ asynchronní metody musí být buď `void`, nebo ***typ úlohy***. Typy úloh jsou `System.Threading.Tasks.Task` a typy vytvořené z `System.Threading.Tasks.Task<T>`. Pro účely zkrácení je v této kapitole odkazováno `Task` na tyto typy, a `Task<T>`to v uvedeném pořadí. Asynchronní metoda, která vrací typ úkolu, je označována jako vracející úlohy.

Přesná definice typů úloh je definovaná implementace, ale v bodě zobrazení je typ úlohy v jednom ze stavů neúplný, úspěšný nebo chybný. Úloha s chybou zaznamená určitou výjimku. Výsledkem úspěšného `Task<T>` záznamu je výsledek typu `T`. Typy úloh jsou await, a proto mohou být operandy výrazů await ([výrazy await](expressions.md#await-expressions)).

Asynchronní vyvolání funkce má schopnost pozastavit vyhodnocení pomocí výrazů await ([výrazy await](expressions.md#await-expressions)) ve svém těle. Vyhodnocení může být později obnoveno v okamžiku pozastavení výrazu await pomocí ***delegáta pokračování***. Delegát pokračování je typu `System.Action`a při jeho vyvolání dojde k vyhodnocení vyvolání asynchronní funkce z výrazu await, kde skončila. ***Aktuální volající*** asynchronní volání funkce je původní volající, pokud se volání funkce nikdy pozastavilo nebo Poslední volající delegáta pokračování v opačném případě.

### <a name="evaluation-of-a-task-returning-async-function"></a>Vyhodnocení asynchronní funkce vracející úlohu

Volání asynchronní funkce vracející úlohu způsobí, že se vygeneruje instance vráceného typu úkolu. To se označuje jako ***návratová úloha*** asynchronní funkce. Úloha je zpočátku v neúplném stavu.

Text asynchronní funkce se pak vyhodnotí, dokud se buď pozastaví (tím, že se dokončí výraz await), nebo se ukončí, když se ovládací prvek, na který se vrací, vrátí volajícímu, spolu s návratovou úlohou.

Po ukončení textu asynchronní funkce se úloha vrácení přemístí mimo nekompletní stav:

*  Pokud tělo funkce skončí jako výsledek dosažení návratového příkazu nebo konce těla, je do návratové úlohy zaznamenána jakákoli výsledná hodnota, která je vložena do úspěšného stavu.
*  Pokud tělo funkce skončí jako výsledek nezachycené výjimky ([příkaz throw](statements.md#the-throw-statement)), je výjimka zaznamenána v úloze vrácení, která je umístěna do chybového stavu.

### <a name="evaluation-of-a-void-returning-async-function"></a>Vyhodnocení asynchronní funkce vracející typ void

Pokud je `void`návratový typ asynchronní funkce, vyhodnocování se liší od výše uvedeného následujícím způsobem: Vzhledem k tomu, že není vrácen žádný úkol, funkce místo toho komunikuje s dokončováním a výjimkou do ***kontextu synchronizace***aktuálního vlákna. Přesná definice synchronizačního kontextu je závislá na implementaci, ale je reprezentace "Where", na kterém je spuštěno aktuální vlákno. Kontext synchronizace je upozorněn, když je zahájeno vyhodnocení asynchronní funkce vracející anulování, úspěšné dokončení nebo způsobí vyvolání nezachycené výjimky.

Díky tomu může kontext sledovat, kolik asynchronních funkcí vracejících se změnami je spuštěných v rámci něj, a rozhodnout se, jak šířit výjimky, které z nich přicházejí.
