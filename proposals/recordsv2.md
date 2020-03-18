---
ms.openlocfilehash: 5636157ba54e93587847d6f2f7aac2dc675f3112
ms.sourcegitcommit: af27912886f22cda9b98b7769447d85cd9732736
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/09/2019
ms.locfileid: "79484998"
---

# <a name="records-v2"></a>Záznamy v2

V minulosti jsme si mysleli, že se jedná o záznamy jako funkce umožňující práci s daty.

"Práce s daty" je velká skupina s několika omezujícími vlastnostmi, takže může být zajímavá, aby se každý z nich mohl podívat na izolaci. Začněte tím, že si vyhledáme příklad záznamů dnes a některé z jeho nevýhod.

Například jednoduchý záznam by měl být definován dnes takto

```C#
public class UserInfo
{
    public string Username { get; set; }
    public string Email { get; set; }
    public bool IsAdmin  { get; set; } = false;
}
```

a použití by se načetlo.

```C#
void M()
{
    var userInfo = new UserInfo() 
    {
        Username = "andy",
        Email = "angocke@microsoft.com",
        IsAdmin = true
    };
}
```

Tento kód má významné výhody:

1. Definice je odolná proti verzím, dají se snadno přidat nebo přesunout vlastnosti.
2. Vlastnosti se dají nastavit v libovolném pořadí a názvy v inicializaci se vždycky shodují s přístupovými objekty.
3. Vlastnosti s výchozími hodnotami se dají jednoduše přeskočit.

První chyba je, že vlastnosti musí být proměnlivé.

# <a name="mutability"></a>Proměnlivost

Co bychom C# chtěli poskytnout způsob, jak nastavit `readonly` člena v inicializátorech objektů.
Vzhledem k tomu, že některé typy nemusely být navržené s touto inicializací, chtěli bychom také vyjádřit výslovný souhlas.

Navrhované řešení je nový modifikátor, `initonly`, který lze použít pro vlastnosti a pole:

```C#
public class UserInfo
{
    public initonly string Username { get; }
    public initonly string Email { get; }
    public initonly bool IsAdmin { get; } = false;
}
```

CodeGen pro tuto hodnotu je překvapivě přímé dopřed: právě nastavíme pole jen pro čtení.
U nižších vlastností by to mělo vypadat takto:

```C#
public class UserInfo
{
    private readonly string <Backing>_username;
    public string get_Username() => <Backing>_username;
    [return: modreq(initonly)]
    public void set_Username(string value) { <Backing>_username = value; }
    ...
}
```

CLR považuje nastavení pole jen pro čtení za neověřitelné, ale ne nebezpečné. Pro podporu pokročilejšího ověřovatele je navrženo následující pravidlo: pole jen pro čtení lze změnit pouze uvnitř `initonly` metody nebo na nový objekt, který je v zásobníku CLR a nebyl publikován prostřednictvím úložiště nebo volání metody.

To by mělo vyřešit mnohé z problémů s proměnlivost v příkladu `UserInfo`, a přitom nevyžadují strategie složitosti nebo poměrně křehký Emit. Neměnnosti ale k dispozici nový problém: snadno se vytvoří objekt se změnami.

# <a name="with-ing"></a>S-Lo

Při programování s neměnnosti se provádí změny objektu vytvořením kopie se změnami namísto provedení změn přímo na objektu. Bohužel neexistuje pohodlný způsob, jak to provést v C#, a to ani pomocí záznamů aktuálního stylu. Dříve jsme navrhli, aby se pro záznamy, které implementují tyto funkce, zadal určitý druh automaticky generované metody with. Pokud máme takový mechanismus, je důležité, aby fungoval s `initonly` členy. Chcete-li toho dosáhnout, je navrženo, že přidáte výraz `with`, který je podobný inicializátoru objektu. Ukázka použití by vypadala takto:

```C#
var userInfo = new UserInfo() 
{
    Username = "andy",
    Email = "angocke@microsoft.com",
    IsAdmin = true
};
var newUserName = userInfo with { Username = "angocke" };
```

Výsledný objekt `newUserName` by představoval kopii `userInfo`s `Username` nastavenou na "angocke".
CodeGen ve výrazu `with` by byl také podobný inicializátoru objektu: je vytvořen nový objekt a poté `initonly` `Username` setter v těle metody.

Rozdíl je samozřejmě, že nově sestavený objekt není jednoduchým vytvořením objektu, jedná se o duplikát původního objektu. Chcete-li tuto funkci poskytnout, je nutné, aby objekt poskytoval "konstruktor", který poskytuje duplicitní objekt. Vzorový `With` konstruktor by vypadal jako:

```C#
class UserInfo
{
    ...
    [WithConstructor] // placeholder syntax, up for debate
    public UserInfo With()
    {
        return new UserInfo() { Username = this.Username, Email = this.Email, IsAdmin = this.IsAdmin };
    }
}
```

Výraz `with` například nastaví `initonly` členů, stejně jako inicializátor objektu, takže aby bylo možné podporovat ověřování, je nutné zajistit, aby objekt nebyl publikován před nastavením `initonly` členů. K vykonání atributu `WithConstructor` (nebo ekvivalentní syntaxe) vynutilo nové pravidlo pro metodu: všechny návratové příkazy musí přímo obsahovat výraz pro vytvoření objektu, pravděpodobně u inicializátoru objektu.

Pokud konstruktor `With` vyžaduje ověření, může uživatel zavést konstruktor, který toto ověření provede, např.

```C#
class UserInfo
{
    ...
    private UserInfo(UserInfo original)
    {
        // validation code
    }
    [WithConstructor]
    public UserInfo With() => new UserInfo(this);
}
```

Poslední část složitosti přidružená k `With` je dědění. Pokud je záznam rozšiřitelný, budete muset pro podtřídu zadat nový `With`. Toho lze dosáhnout následujícím způsobem:

```C#
class Base
{
    ...
    protected Base(Base original)
    {
        // validation
    }
    [WithConstructor]
    public virtual Base With() => new Base(this);
}
class Derived : Base
{
    ...
    protected Derived(Derived original)
    : base(original)
    {
        // validation
    }
    [WithConstructor]
    public override Derived With() => new Derived(this);
}
```

Poznamenejte si tu další složitost: aby bylo možné přepsat `With` konstruktor s odvozeným typem, bude jazyk také potřebovat podporu kovariantních návratových typů v potlačení.
[Zde](https://github.com/dotnet/csharplang/blob/725763343ad44a9251b03814e6897d87fe553769/proposals/covariant-returns.md)už existuje samostatný návrh této funkce.

**Nevýhody**

- Provedení všech návratových příkazů ve `WithConstructor`s obsahuje nové výrazy objektu jsou omezující.
  To může být možné zmírnit analýzou toku, která zajišťuje, že nový objekt neřídí metodu.
- Podpora variance v přepsáních pomocí štychů kompilátoru bude vyžadovat zástupné metody, které se budou postupně zvětšovat s hloubkou dědičnosti. Nutnost pro metodu zástupné procedury je způsobena požadavkem na modul runtime, který přesně Přepisuje signatury. Pokud byl požadavek na modul runtime vydaný, nemusíte vůbec vyžadovat metody stub.
- Použití zřetězených konstruktorů ve formuláři `Type(Type original)` efektivně vyhrazuje tento konstruktor pro použití vzoru. Vzhledem k tomu, že konstruktory mají jedinečnou sémantiku a nelze je znovu pojmenovat, může to být omezení.


## <a name="wrapping-it-all-up-records"></a>Vše zabalením: záznamů

Výše uvedené funkce umožňují styl programování, které bylo velmi obtížné. Ale i s novými funkcemi by mohly být poměrně podrobné a náchylné k chybám, aby se všechno daly opatřit poznámkami. K dispozici je také několik položek, jako je rovná se a GetHashCode, které již mohou být zapsány dnes, stačí pracné.
Významná Chyba při implementaci rovnosti na těchto nových primitivních objektech je navíc taková, že strukturální rovnost je něco, co by se mělo změnit s datovým typem při přidávání nových dat, ale při ručním zpracování je pravděpodobnější, že se tyto věci můžou dostat mimo synchronizaci.

Proto je navržena tak, C# aby podporovala novou syntaxi záznamů, neposkytovala nové funkce, ale pro nastavení výchozích hodnot a generování kódu navrženého pro použití v záznamech. Příklad syntaxe by vypadala takto

```C#
data class UserInfo
{
    public string Username { get; }
    public string Email { get; }
    public bool IsAdmin { get; } = false;
}
```

Generovaný kód pro tuto třídu bude považovat všechna veřejná pole a automatické vlastnosti jako strukturální členy záznamu. Členy záznamu mohou být přizpůsobeny pomocí nového atributu `RecordMember(bool)`, který lze použít buď k zahrnutí nebo vyloučení členů. Členové záznamu by byli `initonly` ve výchozím nastavení a pro třídu založenou na členech záznamu by byla automaticky vygenerována rovnost. V jakémkoli bodě by chování těchto členů bylo možné přizpůsobit jednoduše tím, že je deklarujete ve zdroji. Implementace vytvořená uživatelem by nahradila výchozí implementaci ve všech použitích vzorů.

Všimněte si, že rovnost na straně dědičnosti je složitá, ale zdá se, že je v [návrhu ostatních záznamů](records.md)vhodně vyřešená.

## <a name="primary-constructors"></a>Primární konstruktory

Předchozí návrh záznamu obsahuje také novou syntaxi pro seznam parametrů na samotném typu, např.

```C#
class Point(int X, int Y);
```

V novém návrhu by seznam parametrů byl kolmou C# funkcí, která by mohla být čistě integrovaná se záznamy. Pokud je primární konstruktor součástí záznamu, má nové výchozí hodnoty, stejně jako veřejná pole a automatické vlastnosti: parametry v primárním konstruktoru by se měly použít ke generování veřejných vlastností člena záznamu se stejným názvem. Kromě toho by se teď mohl použít primární konstruktor k automatickému vygenerování dekonstruktoru.

Například následující záznam s primárním konstruktorem

```C#
data class Point(int X, int Y);
```

by byl ekvivalentní

```C#
data class Point
{
    public int X { get; }
    public int Y { get; }

    public Point(int x, int y)
    {
        X = x;
        Y = y;
    }

    public void Deconstruct(out int X, out int Y)
    {
        X = this.X;
        Y = this.Y;
    }
}
```

a konečná generace výše by byla

```C#
class Point
{
    public initonly int X { get; }
    public initonly int Y { get; }

    public Point(int x, int y)
    {
        X = x;
        Y = y;
    }

    protected Point(Point other)
    : this(other.X, other.Y)
    { }

    [WithConstructor]
    public virtual Point With() => new Point(this);

    public void Deconstruct(out int X, out int Y)
    {
        X = this.X;
        Y = this.Y;
    }

    // Generated equality
}
```

Všimněte si, že pro datovou třídu s primárním konstruktorem je třeba vzít v úvahu další informace: místo nastavení primárních polí uvnitř generovaného chráněného konstruktoru je delegován na primární konstruktor. Pokud má Třída Point jiný než primární člen záznamu, např.

```C#
data class Point(int X, int Y)
{
    public int Z { get; }
}
```

následně by se takto vygenerovaný chráněný konstruktor změnil:

```C#
class Point
{
    // ...
    protected Point(Point other)
    : this(other.X, other.Y)
    {
        Z = other.Z;
    }
    // ...
}
```

Zejména to nereaguje na to, co dělat v souvislosti s děděním záznamů s primárními konstruktory. Například

```C#
data class A(int X, int Y);
data class B(int X, int Y, int Z) : A;
```

Místo toho, aby bylo možné nějakým způsobem vyřešit, může explicitní přístup vyžadovat, aby seznam parametrů byl poskytnut se základním seznamem, např.

```C#
data class A(int X, int Y);
data class B(int X, int Y, int Z) : A(X, Y);
```

Seznam parametrů v základním seznamu pak bude použit pro volání `base` v generovaném primárním konstruktoru:

```C#
class B
{
    // ..
    public B(int x, int y, int z)
    : base(x, y)
    // ..
}
```

Jak by primární konstruktor mohl znamenat mimo záznam, který je stále otevřený pro další návrh.
