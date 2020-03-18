---
ms.openlocfilehash: 49720d62c496ff904eacacb31ae29ef97a5aefaa
ms.sourcegitcommit: 8152182f0a477cb3082e625b607262cc459a17f3
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/23/2019
ms.locfileid: "79484942"
---
# <a name="records"></a>záznamy

* Navrženo [x]
* [] Prototyp: [dokončeno](https://github.com/PROTOTYPE_OWNER/roslyn/BRANCH_NAME)
* [] [Implementace: probíhá](https://github.com/dotnet/roslyn/BRANCH_NAME)
* [] Specifikace: specifikace konceptu je uzavřená.

## <a name="summary"></a>Souhrn
[summary]: #summary

Záznamy jsou nové, zjednodušené formuláře deklarace pro C# typy třídy a struktury, které kombinují výhody řady jednodušších funkcí. Popisujeme nové funkce (parametry volajícího a *s-Expression*s), dávají syntaxi a sémantiku pro deklarace záznamů a pak poskytují několik příkladů.


## <a name="motivation"></a>Motivační
[motivation]: #motivation

Významný počet deklarací typu v C# nástroji je trochu více než agregované kolekce typových dat. Deklarace takových typů bohužel vyžaduje Skvělé využívání často používaného kódu. *Záznamy* poskytují mechanismus pro deklaraci datového typu tím, že popisují členy agregace spolu s dodatečným kódem nebo odchylkami od obvyklého často používaného textu, pokud existují.

Viz [Příklady](#examples)níže.

## <a name="detailed-design"></a>Podrobný návrh
[design]: #detailed-design

### <a name="caller-receiver-parameters"></a>parametry přijímače volajícího

V současné době musí být *výchozí argument* parametru metody 
- *konstantní výraz*; ani
- výraz formuláře `new S()`, kde `S` je hodnotový typ; ani
- výraz formuláře `default(S)`, kde `S` je hodnotový typ

Rozšíříme to tak, aby přidalo následující
- výraz `this.Identifier` formuláře

Tento nový formulář se nazývá *výchozí argument typu volající přijímač*a je povolený jenom v případě, že jsou splněné všechny následující podmínky.
- Metoda, ve které se vyskytuje, je metoda instance; ani
- Výraz `this.Identifier` váže ke členu instance ohraničujícího typu, který musí být buď pole, nebo vlastnost; ani
- Člen, ke kterému se váže (a přístupový objekt `get`, pokud se jedná o vlastnost) je alespoň tak přístupný jako metoda; ani
- Typ `this.Identifier` je implicitně převoditelný pomocí identity nebo konverze s možnou hodnotou null na typ parametru (Toto je stávající omezení ve *výchozím argumentu*).

Pokud je argument vynechán od vyvolání člena funkce pro odpovídající volitelný parametr s *výchozím argumentem volajícího přijímače*, hodnota člena příjemce je implicitně předána. 

> **Poznámky k návrhu**: hlavní důvod parametru volajícího přijímače je podpora *výrazu with-Expression*. Nápad je, že můžete deklarovat metodu jako tuto
> ```cs
> class Point
> {
>     public readonly int X;
>     public readonly int Y;
>     public Point With(int x = this.X, int y = this.Y) => new Point(x, y);
>     // etc
> }
> ```
> a pak ho použijte takto
> ```cs
>     Point p = new Point(3, 4);
>     p = p.With(x: 1);
> ```
> Chcete-li vytvořit novou `Point` stejným způsobem jako stávající `Point` ale s hodnotou `X` změněno.
> 
> Jedná se o otevřenou otázku bez ohledu na to, jestli je syntaktická forma *with-Expression* přidávána až po podpoře parametrů volajícího přijímače, takže by to bylo možné, takže by to mělo být *místo* *kromě* *výrazu with-Expression*.

- [] **Otevření problému**: Jaké je pořadí, ve kterém je *výchozí argument volajícího přijímače* vyhodnocen s ohledem na jiné argumenty? Řekněme, že není určen?

### <a name="with-expressions"></a>výrazy with

Navrhuje se nový formulář výrazu:

```antlr
primary_expression
    : with_expression
    ;

with_expression
    : primary_expression 'with' '{' with_initializer_list '}'
    ;

with_initializer_list
    : with_initializer
    | with_initializer ',' with_initializer_list
    ;

with_initializer
    : identifier '=' expression
    ;
```

Token `with` je nové klíčové slovo závislé na kontextu.

Každý *identifikátor* nalevo od *with_initializer* musí být svázán s polem nebo vlastností přístupné instance typu *primary_expression* *with_expression*. Mezi těmito identifikátory daného *with_expression*nesmí existovat žádný duplicitní název.

*With_expression* formuláře

> `with` `{` *identifikátoru* *E1* = *e2*,... `}`

se považuje za vyvolání formuláře.

> *e1*`.With(`*identifier2*`:` *e2*,...`)`

Kde, pro každou metodu s názvem `With`, která je přístupným členem instance *E1*, vybíráme *identifier2* jako název prvního parametru v této metodě, který má parametr volajícího přijímače, který je stejný člen jako pole instance nebo vlastnost vázaná na *identifikátor*. V případě, že žádný takový parametr nelze identifikovat, je-li metoda vyloučena z úvahy. Metoda, která má být vyvolána, je vybrána z zbývajících kandidátů podle řešení přetížení.

> **Poznámky k návrhu**: zadané parametry přijímače volajícího, mnoho výhod *výrazu with* je k dispozici bez tohoto speciálního formátu syntaxe. Proto zvažujeme, jestli je to potřeba. Hlavní výhodou je umožnění jednoho na program v podobě názvů polí a vlastností, nikoli z podmínek názvů parametrů. Tímto způsobem vylepšíme čitelnost i kvalitu nástrojů (např. jít na definici na identifikátoru *with_expression* by se místo parametru metody mohly přejít na vlastnost).

- [] **Otevření problému**: Tento popis by měl být upravený tak, aby podporoval metody rozšíření.

### <a name="pattern-matching"></a>shoda vzoru

Pro specifikaci `Deconstruct` a jeho vztahu k porovnávání vzorů se podívejte na [specifikaci porovnávání vzorů](csharp-8.0/patterns.md#positional-pattern) .

> **Poznámky k návrhu**: na základě `Deconstruct` vygenerovaného kompilátorem jak je uvedeno v tomto dokumentu a specifikace pro porovnávání vzorů, deklarace záznamů
> ```cs
> public class Point(int X, int Y);
> ```
> bude podporovat poziční porovnávání vzorů následujícím způsobem.
> ```cs
> Point p = new Point(3, 4);
> if (p is Point(3, var y)) { // if X is 3
>     Console.WriteLine(y);   // print Y
> }
> ```

### <a name="record-type-declarations"></a>deklarace typu záznamu

Syntaxe pro `class` nebo deklarace `struct` je rozšířena na parametry hodnot, které jsou podporovány. parametry se stanou vlastnostmi tohoto typu:

```antlr
class_declaration
    : attributes? class_modifiers? 'partial'? 'class' identifier type_parameter_list?
      record_parameters? record_class_base? type_parameter_constraints_clauses? class_body
    ;

struct_declaration
    : attributes? struct_modifiers? 'partial'? 'struct' identifier type_parameter_list?
      record_parameters? struct_interfaces? type_parameter_constraints_clauses? struct_body
    ;

record_class_base
    : class_type record_base_arguments?
    | interface_type_list
    | class_type record_base_arguments? ',' interface_type_list
    ;

record_base_arguments
    : '(' argument_list? ')'
    ;

record_parameters
    : '(' record_parameter_list? ')'
    ;

record_parameter_list
    : record_parameter
    | record_parameter ',' record_parameter_list
    ;

record_parameter
    : attributes? type identifier record_property_name? default_argument?
    ;

record_property_name
    : ':' identifier
    ;

class_body
    : '{' class_member_declarations? '}'
    | ';'
    ;

struct_body
    : '{' struct_members_declarations? '}'
    | ';'
    ;
```

> **Poznámky k návrhu**: vzhledem k tomu, že typy záznamů jsou často užitečné, aniž by museli být členy explicitně deklarovány v těle třídy, upravujeme syntaxi deklarace, aby tělo bylo možné jednoduše použít jako středník.

Třída (struct) deklarovaná pomocí *parametrů Record* se nazývá *Třída záznamu* (*Struktura záznamu*), která je *typu záznamu*.

- [] **Otevření problému**: musíme do gramatiky zahrnout *primary_constructor_body* , aby se mohl objevit v rámci deklarace typu záznamu.
- [] **Otevření problému**: jaká jsou pravidla konfliktů názvů pro názvy parametrů? Pravděpodobně jedna z nich není povolena v konfliktu s parametrem typu nebo jiným *parametrem záznamu*.
- [] **Otevření problému**: musíme zadat rozsah parametrů záznamu. Kde je lze použít? V rámci inicializátorů polí instance a *primary_constructor_body* nejméně.
- [] **Otevření problému**: může být deklarace typu záznamu částečná? Pokud ano, musíte parametry opakovat v každé části?

#### <a name="members-of-a-record-type"></a>Členové typu záznamu

Kromě členů deklarovaných v *těle třídy*má typ záznamu následující další členy:

##### <a name="primary-constructor"></a>Primární konstruktor

Typ záznamu má konstruktor `public`, jehož signatura odpovídá parametrům hodnoty deklarace typu. Označuje se jako *primární konstruktor* pro daný typ a způsobí, že implicitně deklarovaný *výchozí konstruktor* bude potlačen.

Za běhu primárního konstruktoru

* Inicializuje pole zálohování generované kompilátorem pro vlastnosti odpovídající parametrům hodnoty (pokud jsou tyto vlastnosti poskytovány kompilátorem; [viz 1.1.2](#1.1.2)); Stisknutím
* spustí Inicializátory pole instance uvedené v *těle třídy*; a potom
* vyvolá konstruktor základní třídy:
    * Pokud jsou v *record_base_arguments*argumenty, je vyvolán základní konstruktor vybraný pomocí řešení přetížení s těmito argumenty;
    * V opačném případě je základní konstruktor vyvolán bez argumentů.
* spustí tělo každé *primary_constructor_body*, pokud existuje, ve zdrojové objednávce.

- [] **Otevření problému**: musíme specifikovat toto pořadí, zejména u všech kompilačních jednotek pro částečné.
- [] **Otevření problému**: musíme specifikovat, že každý explicitně deklarovaný konstruktor musí být zřetězený s primárním konstruktorem.
- [] **Otevření problému**: by mělo být povoleno změnit modifikátor přístupu u primárního konstruktoru?
- [] **Otevření problému**: v struktuře záznamu je chyba, že neexistují žádné parametry záznamu?

##### <a name="primary-constructor-body"></a>Tělo primárního konstruktoru

```antlr
primary_constructor_body
    : attributes? constructor_modifiers? identifier block
    ;
```

*Primary_constructor_body* lze použít pouze v rámci deklarace typu záznamu. *Identifikátor* *primary_constructor_body* musí pojmenovat typ záznamu, ve kterém je deklarován.

*Primary_constructor_body* sám nedeklaruje člena, ale je to způsob, jak programátorovi poskytnout atributy pro a zadat přístup k primárnímu konstruktoru typu záznamu. Umožňuje také programátorovi poskytnout další kód, který se spustí, když je vytvořena instance typu záznamu.

- [] **Otevření problému**: Nezapomeňte si uvědomit, že výchozí konstruktor struktury ho přeskočí.
- [] **Otevření problému**: je potřeba zadat pořadí spouštění inicializace.
- [] **Otevření problému**: Pokud máme v deklaraci typu nezáznamový typ něco jako *primary_constructor_body* (předpokládá se, že neexistují atributy a modifikátory), a považujeme za to, že by to byl kód inicializátoru pole instance?

##### <a name="properties"></a>Vlastnosti

Pro každý parametr záznamu deklarace typu záznamu existuje odpovídající člen vlastnosti `public`, jehož název a typ jsou převedené z deklarace parametru value. Jeho název je *identifikátor* *record_property_name*, pokud je k dispozici, nebo *identifikátor* *record_parameter* jinak. Pokud žádná konkrétní (tj. neabstraktní) veřejná vlastnost s přistupujícím objektem `get` a s tímto názvem a typem je explicitně deklarována nebo zděděna, je vytvořena kompilátorem následujícím způsobem:

* Pro *strukturu záznamu* nebo *třídu záznamu*`sealed`:
 * `private` `readonly` pole je vytvořeno jako pole zálohování pro vlastnost `readonly`. Jeho hodnota je inicializována během konstrukce s hodnotou odpovídajícího primárního parametru konstruktoru.
 * Vlastnost přístupového objektu `get` je implementovaná pro návratovou hodnotu pole pro zálohování.
 * Všechny "párové" zděděné přístupové objekty `get` virtuální vlastnosti jsou přepsány.

> **Poznámky k návrhu**: jinými slovy Pokud rozšiřujete základní třídu nebo implementujete rozhraní, které deklaruje veřejnou abstraktní vlastnost se stejným názvem a typem jako parametr záznamu, je tato vlastnost přepsána nebo implementována.

- [] **Otevření problému**: je možné změnit modifikátor přístupu u vlastnosti, pokud je explicitně deklarována?
- [] **Otevření problému**: mělo by být možné nahradit pole pro vlastnost?

##### <a name="object-methods"></a>Metody objektů

V případě *struktury záznamu* nebo *třídy záznamu*`sealed` jsou implementace metod `object.GetHashCode()` a `object.Equals(object)` vytvářeny kompilátorem, pokud jej neposkytuje uživatel.

- [] **Otevření problému**: doporučujeme přesně zadat implementaci.
- [] **Otevření problému**: Doporučujeme také přidat rozhraní `IEquatable<T>` pro daný typ záznamu a určit, že implementace jsou k dispozici.
- [] **Otevření problému**: Doporučujeme také určit, že implementujeme všechny `IEquatable<T>.Equals`.
- [] **Otevření problému**: doporučujeme přesně zadat, jak řešíme problém rovnosti záznamů na začátku dědičnosti záznamu: konkrétně způsob, jakým generujeme metody rovnosti tak, aby byly symetrické, přenositelné, reflexivní atd.
- [] **Otevření problému**: bylo navrženo, že pro typy záznamů implementujeme `operator ==` a `operator !=`.
- [] **Otevření problému**: máme automaticky vygenerovat implementaci `object.ToString`?

##### `Deconstruct`

Typ záznamu obsahuje metodu `public` generovanou kompilátorem `void Deconstruct`, pokud uživatel nezadá nějaký podpis. Každý parametr je `out` parametr se stejným názvem a typem jako odpovídající parametr typu záznamu. Implementace této metody pomocí kompilátoru přiřadí každému `out` parametru hodnotu odpovídající vlastnosti.

Prohlédněte [si specifikaci porovnávání vzorů](csharp-8.0/patterns.md#positional-pattern) pro sémantiku `Deconstruct`.

##### <a name="with-method"></a>`With` – metoda

Pokud není k dispozici uživatelsky deklarovaný člen s názvem `With` deklarované, typ záznamu má metodu poskytnutou kompilátorem s názvem `With`, jejíž návratový typ je samotný typ záznamu a obsahuje jeden parametr hodnoty odpovídající každému *parametru záznamu* ve stejném pořadí, že tyto parametry jsou uvedeny v deklaraci typu záznamu. Každý parametr musí mít *výchozí argument typu volajícího přijímače* odpovídající vlastnosti.

V třídě `abstract` záznamu je metoda `With` poskytnutá kompilátorem abstraktní. V rámci struktury záznamu nebo zapečetěné třídy záznamů je `With` metoda poskytnutá kompilátorem `sealed`. V opačném případě `With` metoda, která je součástí kompilátoru, je virtuální a její implementace vrátí novou instanci vytvořenou vyvoláním primárního konstruktoru s parametry jako argumenty pro vytvoření nové instance z parametrů a vrácení této nové instance.

- [] **Otevření problému**: Doporučujeme také určit, za jakých podmínek přepíšeme nebo implementujete zděděné metody Virtual `With` nebo `With` metody z implementovaných rozhraní.
- [] **Otevření problému**: máme to říci, co se stane, když zdědíme nevirtuální `With` metodu.

> **Poznámky k návrhu**: vzhledem k tomu, že typy záznamů jsou ve výchozím nastavení neměnné, poskytuje metoda `With` způsob vytvoření nové instance, která je stejná jako existující instance, ale s vybranými vlastnostmi pro nové hodnoty. Například zadaný
> ```cs
> public class Point(int X, int Y);
> ```
> k dispozici je člen poskytnutý kompilátorem.
> ```cs
>     public virtual Point With(int X = this.X, int Y = this.Y) => new Point(X, Y);
> ```
> Který povoluje proměnnou typu záznamu
> ```cs
> var p = new Point(3, 4);
> ```
> má být nahrazena instancí, která má jednu nebo více různých vlastností.
> ```cs
>     p = p.With(X: 5);
> ```
> To se dá také vyjádřit pomocí *with_expression*:
> ```cs
>     p = p with { X = 5 };
> ```

### <a name="examples"></a>Příklady

#### <a name="compatibility-of-record-types"></a>Kompatibilita typů záznamů

Vzhledem k tomu, že programátor může přidat členy do deklarace typu záznamu, je často možné změnit sadu prvků záznamu, aniž by to ovlivnilo stávající klienty. Například s ohledem na počáteční verzi typu záznamu

```cs
// v1
public class Person(string Name, DateTime DateOfBirth);
```

Nový prvek typu záznamu lze compatibly přidat do další revize typu, aniž by to ovlivnilo binární nebo zdrojovou kompatibilitu:

```cs
// v2
public class Person(string Name, DateTime DateOfBirth, string HomeTown)
{
    // Note: below operations added to retain binary compatibility with v1
    public Person(string Name, DateTime DateOfBirth) : this(Name, DateOfBirth, string.Empty) {}
    public static void operator is(Person self, out string Name, out DateTime DateOfBirth)
        { Name = self.Name; DateOfBirth = self.DateOfBirth; }
    public Person With(string Name, DateTime DateOfBirth) => new Person(Name, DateOfBirth);
}
```

#### <a name="record-struct-example"></a>Příklad struktury záznamu

Tato struktura záznamu

```cs
public struct Pair(object First, object Second);
```

je přeloženo na tento kód

```cs
public struct Pair : IEquatable<Pair>
{
    public object First { get; }
    public object Second { get; }
    public Pair(object First, object Second)
    {
        this.First = First;
        this.Second = Second;
    }
    public bool Equals(Pair other) // for IEquatable<Pair>
    {
        return Equals(First, other.First) && Equals(Second, other.Second);
    }
    public override bool Equals(object other)
    {
        return (other as Pair)?.Equals(this) == true;
    }
    public override int GetHashCode()
    {
        return (First?.GetHashCode()*17 + Second?.GetHashCode()).GetValueOrDefault();
    }
    public Pair With(object First = this.First, object Second = this.Second) => new Pair(First, Second);
    public void Deconstruct(out object First, out object Second)
    {
        First = self.First;
        Second = self.Second;
    }
}
```

- [] **Otevření problému**: má-li být implementace rovnosti (dvojice jiných) veřejným členem dvojice?
- [] **Otevření problému**: tato implementace `Equals` není symetrická na tváři dědičnosti.
- [] **Otevření problému**: by měl záznam deklarovat `operator ==` a `operator !=`?

> **Poznámky k návrhu**: vzhledem k tomu, že jeden typ záznamu může dědit z jiné a tato implementace `Equals` by v takovém případě nebyla v takovém případě symetrická, není správná. Navrhli jsme implementovat rovnost tímto způsobem:
> ```cs
>     public bool Equals(Pair other) // for IEquatable<Pair>
>     {
>         return other != null && EqualityContract == other.EqualityContract &&
>             Equals(First, other.First) && Equals(Second, other.Second);
>     }
>     protected virtual Type EqualityContract => typeof(Pair);
> ```
> Odvozené záznamy by `override EqualityContract`. Méně atraktivní alternativou je omezení dědičnosti.

#### <a name="sealed-record-example"></a>Příklad zapečetěného záznamu

Tato zapečetěná třída záznamu

```cs
public sealed class Student(string Name, decimal Gpa);
```

se převede do tohoto kódu.

```cs
public sealed class Student : IEquatable<Student>
{
    public string Name { get; }
    public decimal Gpa { get; }
    public Student(string Name, decimal Gpa)
    {
        this.Name = Name;
        this.Gpa = Gpa;
    }
    public bool Equals(Student other) // for IEquatable<Student>
    {
        return other != null && Equals(Name, other.Name) && Equals(Gpa, other.Gpa);
    }
    public override bool Equals(object other)
    {
        return this.Equals(other as Student);
    }
    public override int GetHashCode()
    {
        return (Name?.GetHashCode()*17 + Gpa?.GetHashCode()).GetValueOrDefault();
    }
    public Student With(string Name = this.Name, decimal Gpa = this.Gpa) => new Student(Name, Gpa);
    public void Deconstruct(out string Name, out decimal Gpa)
    {
        Name = self.Name;
        Gpa = self.Gpa;
    }
}
```

#### <a name="abstract-record-class-example"></a>příklad třídy abstraktního záznamu

Tato abstraktní třída záznamů

```cs
public abstract class Person(string Name);
```

se převede do tohoto kódu.

```cs
public abstract class Person : IEquatable<Person>
{
    public string Name { get; }
    public Person(string Name)
    {
        this.Name = Name;
    }
    public bool Equals(Person other)
    {
        return other != null && Equals(Name, other.Name);
    }
    public override bool Equals(object other)
    {
        return Equals(other as Person);
    }
    public override int GetHashCode()
    {
        return (Name?.GetHashCode()).GetValueOrDefault();
    }
    public abstract Person With(string Name = this.Name);
    public void Deconstruct(Person self, out string Name)
    {
        Name = self.Name;
    }
}
```

#### <a name="combining-abstract-and-sealed-records"></a>kombinování abstraktních a zapečetěných záznamů

Byla zadána třída abstraktního záznamu `Person` výše, tato třída zapečetěného záznamu

```cs
public sealed class Student(string Name, decimal Gpa) : Person(Name);
```

se převede do tohoto kódu.

```cs
public sealed class Student : Person, IEquatable<Student>
{
    public override string Name { get; }
    public decimal Gpa { get; }
    public Student(string Name, decimal Gpa) : base(Name)
    {
        this.Name = Name;
        this.Gpa = Gpa;
    }
    public override bool Equals(Student other) // for IEquatable<Student>
    {
        return Equals(Name, other.Name) && Equals(Gpa, other.Gpa);
    }
    public bool Equals(Person other) // for IEquatable<Person>
    {
        return (other as Student)?.Equals(this) == true;
    }
    public override bool Equals(object other)
    {
        return (other as Student)?.Equals(this) == true;
    }
    public override int GetHashCode()
    {
        return (Name?.GetHashCode()*17 + Gpa.GetHashCode()).GetValueOrDefault();
    }
    public Student With(string Name = this.Name, decimal Gpa = this.Gpa) => new Student(Name, Gpa);
    public override Person With(string Name = this.Name) => new Student(Name, Gpa);
    public void Deconstruct(Student self, out string Name, out decimal Gpa)
    {
        Name = self.Name;
        Gpa = self.Gpa;
    }
}
```

## <a name="drawbacks"></a>Nevýhody
[drawbacks]: #drawbacks

Stejně jako u libovolné jazykové funkce musíme dotazovat, jestli se další složitost tohoto jazyka znovu vyplácí v další čistotě, která je k dispozici pro tělo C# programů, které by mohly využít výhod této funkce.

## <a name="alternatives"></a>Alternativy
[alternatives]: #alternatives

Za 6 se C# považujeme za přidání *primárních konstruktorů* . I když zabírají stejnou syntaktickou plochu jako tento návrh, zjistili jsme, že se jedná o krátké výhody nabízené záznamy.

## <a name="unresolved-questions"></a>Nevyřešené dotazy
[unresolved]: #unresolved-questions

Otevřené otázky se zobrazí v těle návrhu.

## <a name="design-meetings"></a>Schůzky pro návrh

Bude doplněno
