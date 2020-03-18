---
ms.openlocfilehash: 5636157ba54e93587847d6f2f7aac2dc675f3112
ms.sourcegitcommit: af27912886f22cda9b98b7769447d85cd9732736
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/09/2019
ms.locfileid: "79484998"
---

# <a name="records-v2"></a><span data-ttu-id="90492-101">Záznamy v2</span><span class="sxs-lookup"><span data-stu-id="90492-101">Records v2</span></span>

<span data-ttu-id="90492-102">V minulosti jsme si mysleli, že se jedná o záznamy jako funkce umožňující práci s daty.</span><span class="sxs-lookup"><span data-stu-id="90492-102">In the past we've thought about records as a feature to enable working with data.</span></span>

<span data-ttu-id="90492-103">"Práce s daty" je velká skupina s několika omezujícími vlastnostmi, takže může být zajímavá, aby se každý z nich mohl podívat na izolaci.</span><span class="sxs-lookup"><span data-stu-id="90492-103">"Working with data" is a big group with a number of facets, so it may be interesting to look at each in isolation.</span></span> <span data-ttu-id="90492-104">Začněte tím, že si vyhledáme příklad záznamů dnes a některé z jeho nevýhod.</span><span class="sxs-lookup"><span data-stu-id="90492-104">Let's start by looking at an example of records today and some of its drawbacks.</span></span>

<span data-ttu-id="90492-105">Například jednoduchý záznam by měl být definován dnes takto</span><span class="sxs-lookup"><span data-stu-id="90492-105">For instance, a simple record would be defined today as follows</span></span>

```C#
public class UserInfo
{
    public string Username { get; set; }
    public string Email { get; set; }
    public bool IsAdmin  { get; set; } = false;
}
```

<span data-ttu-id="90492-106">a použití by se načetlo.</span><span class="sxs-lookup"><span data-stu-id="90492-106">and the usage would read</span></span>

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

<span data-ttu-id="90492-107">Tento kód má významné výhody:</span><span class="sxs-lookup"><span data-stu-id="90492-107">There are significant advantages in this code:</span></span>

1. <span data-ttu-id="90492-108">Definice je odolná proti verzím, dají se snadno přidat nebo přesunout vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="90492-108">The definition is version resilient, properties can easily be added or moved</span></span>
2. <span data-ttu-id="90492-109">Vlastnosti se dají nastavit v libovolném pořadí a názvy v inicializaci se vždycky shodují s přístupovými objekty.</span><span class="sxs-lookup"><span data-stu-id="90492-109">Properties can be set in any order, and the names in the initialization always match the accessors</span></span>
3. <span data-ttu-id="90492-110">Vlastnosti s výchozími hodnotami se dají jednoduše přeskočit.</span><span class="sxs-lookup"><span data-stu-id="90492-110">Properties with default values can simply be skipped</span></span>

<span data-ttu-id="90492-111">První chyba je, že vlastnosti musí být proměnlivé.</span><span class="sxs-lookup"><span data-stu-id="90492-111">The first flaw is that the properties must now be mutable.</span></span>

# <a name="mutability"></a><span data-ttu-id="90492-112">Proměnlivost</span><span class="sxs-lookup"><span data-stu-id="90492-112">Mutability</span></span>

<span data-ttu-id="90492-113">Co bychom C# chtěli poskytnout způsob, jak nastavit `readonly` člena v inicializátorech objektů.</span><span class="sxs-lookup"><span data-stu-id="90492-113">What we'd like is for C# to provide a way to set a `readonly` member in object initializers.</span></span>
<span data-ttu-id="90492-114">Vzhledem k tomu, že některé typy nemusely být navržené s touto inicializací, chtěli bychom také vyjádřit výslovný souhlas.</span><span class="sxs-lookup"><span data-stu-id="90492-114">Since some types may not have been designed with this initialization in mind, we'd also like it to be opt-in.</span></span>

<span data-ttu-id="90492-115">Navrhované řešení je nový modifikátor, `initonly`, který lze použít pro vlastnosti a pole:</span><span class="sxs-lookup"><span data-stu-id="90492-115">The proposed solution is a new modifier, `initonly`, that can be applied to properties and fields:</span></span>

```C#
public class UserInfo
{
    public initonly string Username { get; }
    public initonly string Email { get; }
    public initonly bool IsAdmin { get; } = false;
}
```

<span data-ttu-id="90492-116">CodeGen pro tuto hodnotu je překvapivě přímé dopřed: právě nastavíme pole jen pro čtení.</span><span class="sxs-lookup"><span data-stu-id="90492-116">The codegen for this is surprisingly straight forward: we just set the readonly field.</span></span>
<span data-ttu-id="90492-117">U nižších vlastností by to mělo vypadat takto:</span><span class="sxs-lookup"><span data-stu-id="90492-117">Specifically, the lowered properties would look like:</span></span>

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

<span data-ttu-id="90492-118">CLR považuje nastavení pole jen pro čtení za neověřitelné, ale ne nebezpečné.</span><span class="sxs-lookup"><span data-stu-id="90492-118">The CLR considers setting readonly fields to be unverifiable, but not unsafe.</span></span> <span data-ttu-id="90492-119">Pro podporu pokročilejšího ověřovatele je navrženo následující pravidlo: pole jen pro čtení lze změnit pouze uvnitř `initonly` metody nebo na nový objekt, který je v zásobníku CLR a nebyl publikován prostřednictvím úložiště nebo volání metody.</span><span class="sxs-lookup"><span data-stu-id="90492-119">To support a more advanced verifier, the following rule is proposed: a readonly field can be modified only inside `initonly` methods, or on a new object that is on the CLR stack and has not been published via a store or method call.</span></span>

<span data-ttu-id="90492-120">To by mělo vyřešit mnohé z problémů s proměnlivost v příkladu `UserInfo`, a přitom nevyžadují strategie složitosti nebo poměrně křehký Emit.</span><span class="sxs-lookup"><span data-stu-id="90492-120">This should solve many of the problems with mutability in the `UserInfo` example, while not requiring complicated or brittle emit strategies.</span></span> <span data-ttu-id="90492-121">Neměnnosti ale k dispozici nový problém: snadno se vytvoří objekt se změnami.</span><span class="sxs-lookup"><span data-stu-id="90492-121">However, immutability does present a new problem: easily constructing an object with changes.</span></span>

# <a name="with-ing"></a><span data-ttu-id="90492-122">S-Lo</span><span class="sxs-lookup"><span data-stu-id="90492-122">With-ing</span></span>

<span data-ttu-id="90492-123">Při programování s neměnnosti se provádí změny objektu vytvořením kopie se změnami namísto provedení změn přímo na objektu.</span><span class="sxs-lookup"><span data-stu-id="90492-123">When programming with immutability, making changes to an object is done by constructing a copy with changes instead of making the changes directly on the object.</span></span> <span data-ttu-id="90492-124">Bohužel neexistuje pohodlný způsob, jak to provést v C#, a to ani pomocí záznamů aktuálního stylu.</span><span class="sxs-lookup"><span data-stu-id="90492-124">Unfortunately, there's no convenient way to do this in C#, even with current-style records.</span></span> <span data-ttu-id="90492-125">Dříve jsme navrhli, aby se pro záznamy, které implementují tyto funkce, zadal určitý druh automaticky generované metody with.</span><span class="sxs-lookup"><span data-stu-id="90492-125">It's been previously proposed that some kind of autogenerated "With" method be provided for records that implements that functionality.</span></span> <span data-ttu-id="90492-126">Pokud máme takový mechanismus, je důležité, aby fungoval s `initonly` členy.</span><span class="sxs-lookup"><span data-stu-id="90492-126">If we have such a mechanism, it's important that it work with `initonly` members.</span></span> <span data-ttu-id="90492-127">Chcete-li toho dosáhnout, je navrženo, že přidáte výraz `with`, který je podobný inicializátoru objektu.</span><span class="sxs-lookup"><span data-stu-id="90492-127">To achieve this, it's proposed that we add a `with` expression, analogous to an object initializer.</span></span> <span data-ttu-id="90492-128">Ukázka použití by vypadala takto:</span><span class="sxs-lookup"><span data-stu-id="90492-128">Sample usage would be as follows:</span></span>

```C#
var userInfo = new UserInfo() 
{
    Username = "andy",
    Email = "angocke@microsoft.com",
    IsAdmin = true
};
var newUserName = userInfo with { Username = "angocke" };
```

<span data-ttu-id="90492-129">Výsledný objekt `newUserName` by představoval kopii `userInfo`s `Username` nastavenou na "angocke".</span><span class="sxs-lookup"><span data-stu-id="90492-129">The resulting `newUserName` object would be a copy of `userInfo`, with `Username` set to "angocke".</span></span>
<span data-ttu-id="90492-130">CodeGen ve výrazu `with` by byl také podobný inicializátoru objektu: je vytvořen nový objekt a poté `initonly` `Username` setter v těle metody.</span><span class="sxs-lookup"><span data-stu-id="90492-130">The codegen on the `with` expression would also be similar to the object initializer: a new object is constructed, and then the `initonly` `Username` setter would be called in the method body.</span></span>

<span data-ttu-id="90492-131">Rozdíl je samozřejmě, že nově sestavený objekt není jednoduchým vytvořením objektu, jedná se o duplikát původního objektu.</span><span class="sxs-lookup"><span data-stu-id="90492-131">Of course, the difference here is that the new object being constructed is not a simple new object creation, it is a duplicate of the original object.</span></span> <span data-ttu-id="90492-132">Chcete-li tuto funkci poskytnout, je nutné, aby objekt poskytoval "konstruktor", který poskytuje duplicitní objekt.</span><span class="sxs-lookup"><span data-stu-id="90492-132">To provide this functionality, we require that the object provide a "With constructor" that provides a duplicate object.</span></span> <span data-ttu-id="90492-133">Vzorový `With` konstruktor by vypadal jako:</span><span class="sxs-lookup"><span data-stu-id="90492-133">A sample `With` constructor would look like:</span></span>

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

<span data-ttu-id="90492-134">Výraz `with` například nastaví `initonly` členů, stejně jako inicializátor objektu, takže aby bylo možné podporovat ověřování, je nutné zajistit, aby objekt nebyl publikován před nastavením `initonly` členů.</span><span class="sxs-lookup"><span data-stu-id="90492-134">Notably, the `with` expression will set `initonly` members, just like the object initializer, so to support verification we must ensure that the object cannot have been published before the `initonly` members are set.</span></span> <span data-ttu-id="90492-135">K vykonání atributu `WithConstructor` (nebo ekvivalentní syntaxe) vynutilo nové pravidlo pro metodu: všechny návratové příkazy musí přímo obsahovat výraz pro vytvoření objektu, pravděpodobně u inicializátoru objektu.</span><span class="sxs-lookup"><span data-stu-id="90492-135">To enforce this, the `WithConstructor` attribute (or equivalent syntax) will enforce a new rule for the method: all return statements must directly contain an object creation expression, possibly with an object initializer.</span></span>

<span data-ttu-id="90492-136">Pokud konstruktor `With` vyžaduje ověření, může uživatel zavést konstruktor, který toto ověření provede, např.</span><span class="sxs-lookup"><span data-stu-id="90492-136">If the `With` constructor requires validation, the user may introduce a constructor to do that validation, e.g.</span></span>

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

<span data-ttu-id="90492-137">Poslední část složitosti přidružená k `With` je dědění.</span><span class="sxs-lookup"><span data-stu-id="90492-137">The last piece of complexity associated with `With` is inheritance.</span></span> <span data-ttu-id="90492-138">Pokud je záznam rozšiřitelný, budete muset pro podtřídu zadat nový `With`.</span><span class="sxs-lookup"><span data-stu-id="90492-138">If your record is extensible, you will need to provide a new `With` for the subclass.</span></span> <span data-ttu-id="90492-139">Toho lze dosáhnout následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="90492-139">This can be achieved as follows:</span></span>

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

<span data-ttu-id="90492-140">Poznamenejte si tu další složitost: aby bylo možné přepsat `With` konstruktor s odvozeným typem, bude jazyk také potřebovat podporu kovariantních návratových typů v potlačení.</span><span class="sxs-lookup"><span data-stu-id="90492-140">Note one additional piece of complexity here: in order to override the `With` constructor with the derived type the language will also need to support covariant return types in overrides.</span></span>
<span data-ttu-id="90492-141">[Zde](https://github.com/dotnet/csharplang/blob/725763343ad44a9251b03814e6897d87fe553769/proposals/covariant-returns.md)už existuje samostatný návrh této funkce.</span><span class="sxs-lookup"><span data-stu-id="90492-141">There is already a separate proposal for this feature [here](https://github.com/dotnet/csharplang/blob/725763343ad44a9251b03814e6897d87fe553769/proposals/covariant-returns.md).</span></span>

<span data-ttu-id="90492-142">**Nevýhody**</span><span class="sxs-lookup"><span data-stu-id="90492-142">**Drawbacks**</span></span>

- <span data-ttu-id="90492-143">Provedení všech návratových příkazů ve `WithConstructor`s obsahuje nové výrazy objektu jsou omezující.</span><span class="sxs-lookup"><span data-stu-id="90492-143">Making all return statements in `WithConstructor`s contain new object expressions is restrictive.</span></span>
  <span data-ttu-id="90492-144">To může být možné zmírnit analýzou toku, která zajišťuje, že nový objekt neřídí metodu.</span><span class="sxs-lookup"><span data-stu-id="90492-144">This could be possibly be mitigated by flow analysis that ensures the new object doesn't escape the method</span></span>
- <span data-ttu-id="90492-145">Podpora variance v přepsáních pomocí štychů kompilátoru bude vyžadovat zástupné metody, které se budou postupně zvětšovat s hloubkou dědičnosti.</span><span class="sxs-lookup"><span data-stu-id="90492-145">Supporting variance in overrides through compiler tricks will require stub methods, which will grow quadratically with the inheritance depth.</span></span> <span data-ttu-id="90492-146">Nutnost pro metodu zástupné procedury je způsobena požadavkem na modul runtime, který přesně Přepisuje signatury.</span><span class="sxs-lookup"><span data-stu-id="90492-146">The need for a stub method is due to a runtime requirement that override signatures match exactly.</span></span> <span data-ttu-id="90492-147">Pokud byl požadavek na modul runtime vydaný, nemusíte vůbec vyžadovat metody stub.</span><span class="sxs-lookup"><span data-stu-id="90492-147">If the runtime requirement were loosened, the stub methods would not be required at all.</span></span>
- <span data-ttu-id="90492-148">Použití zřetězených konstruktorů ve formuláři `Type(Type original)` efektivně vyhrazuje tento konstruktor pro použití vzoru.</span><span class="sxs-lookup"><span data-stu-id="90492-148">Using chained constructors of the form `Type(Type original)` effectively reserves that constructor for the use of the pattern.</span></span> <span data-ttu-id="90492-149">Vzhledem k tomu, že konstruktory mají jedinečnou sémantiku a nelze je znovu pojmenovat, může to být omezení.</span><span class="sxs-lookup"><span data-stu-id="90492-149">Since constructors have unique semantics and cannot be re-named this could be limiting.</span></span>


## <a name="wrapping-it-all-up-records"></a><span data-ttu-id="90492-150">Vše zabalením: záznamů</span><span class="sxs-lookup"><span data-stu-id="90492-150">Wrapping it all up: Records</span></span>

<span data-ttu-id="90492-151">Výše uvedené funkce umožňují styl programování, které bylo velmi obtížné.</span><span class="sxs-lookup"><span data-stu-id="90492-151">The above features enable a style of programming that was very difficult before.</span></span> <span data-ttu-id="90492-152">Ale i s novými funkcemi by mohly být poměrně podrobné a náchylné k chybám, aby se všechno daly opatřit poznámkami.</span><span class="sxs-lookup"><span data-stu-id="90492-152">But even with the new features it could be quite verbose and error prone to annotate everything yourself.</span></span> <span data-ttu-id="90492-153">K dispozici je také několik položek, jako je rovná se a GetHashCode, které již mohou být zapsány dnes, stačí pracné.</span><span class="sxs-lookup"><span data-stu-id="90492-153">There are also a few items, like Equals and GetHashCode, which can already be written today, it's just laborious.</span></span>
<span data-ttu-id="90492-154">Významná Chyba při implementaci rovnosti na těchto nových primitivních objektech je navíc taková, že strukturální rovnost je něco, co by se mělo změnit s datovým typem při přidávání nových dat, ale při ručním zpracování je pravděpodobnější, že se tyto věci můžou dostat mimo synchronizaci.</span><span class="sxs-lookup"><span data-stu-id="90492-154">Moreover, a significant flaw in implementing equality on top of these new primitives is that structural equality is something that should change with your data type as new data is added, but when handling it manually it is likely that these things can get out of sync.</span></span>

<span data-ttu-id="90492-155">Proto je navržena tak, C# aby podporovala novou syntaxi záznamů, neposkytovala nové funkce, ale pro nastavení výchozích hodnot a generování kódu navrženého pro použití v záznamech.</span><span class="sxs-lookup"><span data-stu-id="90492-155">Therefore, it is proposed that C# support new syntax for records, not for providing new features, but for setting defaults and generating code designed for use in records.</span></span> <span data-ttu-id="90492-156">Příklad syntaxe by vypadala takto</span><span class="sxs-lookup"><span data-stu-id="90492-156">Example syntax would look like</span></span>

```C#
data class UserInfo
{
    public string Username { get; }
    public string Email { get; }
    public bool IsAdmin { get; } = false;
}
```

<span data-ttu-id="90492-157">Generovaný kód pro tuto třídu bude považovat všechna veřejná pole a automatické vlastnosti jako strukturální členy záznamu.</span><span class="sxs-lookup"><span data-stu-id="90492-157">The generated code for this class would regard all public fields and auto-properties as structural members of the record.</span></span> <span data-ttu-id="90492-158">Členy záznamu mohou být přizpůsobeny pomocí nového atributu `RecordMember(bool)`, který lze použít buď k zahrnutí nebo vyloučení členů.</span><span class="sxs-lookup"><span data-stu-id="90492-158">Record members could be customized using a new `RecordMember(bool)` attribute that could be used to either include or exclude members.</span></span> <span data-ttu-id="90492-159">Členové záznamu by byli `initonly` ve výchozím nastavení a pro třídu založenou na členech záznamu by byla automaticky vygenerována rovnost.</span><span class="sxs-lookup"><span data-stu-id="90492-159">Record members would be `initonly` by default and equality would be autogenerated for the class based on the record members.</span></span> <span data-ttu-id="90492-160">V jakémkoli bodě by chování těchto členů bylo možné přizpůsobit jednoduše tím, že je deklarujete ve zdroji.</span><span class="sxs-lookup"><span data-stu-id="90492-160">At any point the behavior of these members could be customized simply by declaring them in source.</span></span> <span data-ttu-id="90492-161">Implementace vytvořená uživatelem by nahradila výchozí implementaci ve všech použitích vzorů.</span><span class="sxs-lookup"><span data-stu-id="90492-161">The user-written implementation would replace the default implementation in all pattern usage.</span></span>

<span data-ttu-id="90492-162">Všimněte si, že rovnost na straně dědičnosti je složitá, ale zdá se, že je v [návrhu ostatních záznamů](records.md)vhodně vyřešená.</span><span class="sxs-lookup"><span data-stu-id="90492-162">Note that equality in the face of inheritance is complex, but seems to have been adequately solved in the [other records proposal](records.md).</span></span>

## <a name="primary-constructors"></a><span data-ttu-id="90492-163">Primární konstruktory</span><span class="sxs-lookup"><span data-stu-id="90492-163">Primary constructors</span></span>

<span data-ttu-id="90492-164">Předchozí návrh záznamu obsahuje také novou syntaxi pro seznam parametrů na samotném typu, např.</span><span class="sxs-lookup"><span data-stu-id="90492-164">Previous record proposal have also included a new syntax for a parameter list on the type itself, e.g.</span></span>

```C#
class Point(int X, int Y);
```

<span data-ttu-id="90492-165">V novém návrhu by seznam parametrů byl kolmou C# funkcí, která by mohla být čistě integrovaná se záznamy.</span><span class="sxs-lookup"><span data-stu-id="90492-165">In the new design, the parameter list would be an orthogonal C# feature, which could be cleanly integrated with records.</span></span> <span data-ttu-id="90492-166">Pokud je primární konstruktor součástí záznamu, má nové výchozí hodnoty, stejně jako veřejná pole a automatické vlastnosti: parametry v primárním konstruktoru by se měly použít ke generování veřejných vlastností člena záznamu se stejným názvem.</span><span class="sxs-lookup"><span data-stu-id="90492-166">If a primary constructor is included in a record, it would have new defaults, just like public fields and auto-properties: the parameters in the primary constructor would be used to generate public record-member properties with the same name.</span></span> <span data-ttu-id="90492-167">Kromě toho by se teď mohl použít primární konstruktor k automatickému vygenerování dekonstruktoru.</span><span class="sxs-lookup"><span data-stu-id="90492-167">In addition, the primary constructor could now be used to auto-generate a deconstructor.</span></span>

<span data-ttu-id="90492-168">Například následující záznam s primárním konstruktorem</span><span class="sxs-lookup"><span data-stu-id="90492-168">For example, the following record with a primary constructor</span></span>

```C#
data class Point(int X, int Y);
```

<span data-ttu-id="90492-169">by byl ekvivalentní</span><span class="sxs-lookup"><span data-stu-id="90492-169">would be equivalent to</span></span>

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

<span data-ttu-id="90492-170">a konečná generace výše by byla</span><span class="sxs-lookup"><span data-stu-id="90492-170">and the final generation of the above would be</span></span>

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

<span data-ttu-id="90492-171">Všimněte si, že pro datovou třídu s primárním konstruktorem je třeba vzít v úvahu další informace: místo nastavení primárních polí uvnitř generovaného chráněného konstruktoru je delegován na primární konstruktor.</span><span class="sxs-lookup"><span data-stu-id="90492-171">Note that we've taken one other piece of information into account for a data class with a primary constructor: instead of setting the primary fields inside the generated protected constructor, we delegate to the primary constructor.</span></span> <span data-ttu-id="90492-172">Pokud má Třída Point jiný než primární člen záznamu, např.</span><span class="sxs-lookup"><span data-stu-id="90492-172">If the Point class had another non-primary record member, e.g.</span></span>

```C#
data class Point(int X, int Y)
{
    public int Z { get; }
}
```

<span data-ttu-id="90492-173">následně by se takto vygenerovaný chráněný konstruktor změnil:</span><span class="sxs-lookup"><span data-stu-id="90492-173">then that would change the generated protected constructor as follows:</span></span>

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

<span data-ttu-id="90492-174">Zejména to nereaguje na to, co dělat v souvislosti s děděním záznamů s primárními konstruktory.</span><span class="sxs-lookup"><span data-stu-id="90492-174">Notably, this doesn't answer what to do about inheritance of records with primary constructors.</span></span> <span data-ttu-id="90492-175">Například</span><span class="sxs-lookup"><span data-stu-id="90492-175">For instance,</span></span>

```C#
data class A(int X, int Y);
data class B(int X, int Y, int Z) : A;
```

<span data-ttu-id="90492-176">Místo toho, aby bylo možné nějakým způsobem vyřešit, může explicitní přístup vyžadovat, aby seznam parametrů byl poskytnut se základním seznamem, např.</span><span class="sxs-lookup"><span data-stu-id="90492-176">Rather than resolving in an arbitrary manner, a more explicit approach could require that a parameter list be provided with the base list, e.g.</span></span>

```C#
data class A(int X, int Y);
data class B(int X, int Y, int Z) : A(X, Y);
```

<span data-ttu-id="90492-177">Seznam parametrů v základním seznamu pak bude použit pro volání `base` v generovaném primárním konstruktoru:</span><span class="sxs-lookup"><span data-stu-id="90492-177">The parameter list in the base list would then be applied to a `base` call in the generated primary constructor:</span></span>

```C#
class B
{
    // ..
    public B(int x, int y, int z)
    : base(x, y)
    // ..
}
```

<span data-ttu-id="90492-178">Jak by primární konstruktor mohl znamenat mimo záznam, který je stále otevřený pro další návrh.</span><span class="sxs-lookup"><span data-stu-id="90492-178">As for what a primary constructor could mean outside of a record, that is still open to further proposal.</span></span>
