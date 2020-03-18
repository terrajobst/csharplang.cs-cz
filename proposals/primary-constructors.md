---
ms.openlocfilehash: 77c9ffda3a59cc5f3dcc3ec9848600c6c5e03eed
ms.sourcegitcommit: 27487fa0294f4cdb7e5f2478884149e05314fd8a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/23/2019
ms.locfileid: "79484970"
---
# <a name="primary-constructors"></a><span data-ttu-id="92eea-101">Primární konstruktory</span><span class="sxs-lookup"><span data-stu-id="92eea-101">Primary constructors</span></span>

* <span data-ttu-id="92eea-102">Navrženo [x]</span><span class="sxs-lookup"><span data-stu-id="92eea-102">[x] Proposed</span></span>
* <span data-ttu-id="92eea-103">[] Prototyp: Nezahájeno</span><span class="sxs-lookup"><span data-stu-id="92eea-103">[ ] Prototype: Not started</span></span>
* <span data-ttu-id="92eea-104">[] Implementace: Nezahájeno</span><span class="sxs-lookup"><span data-stu-id="92eea-104">[ ] Implementation: Not started</span></span>
* <span data-ttu-id="92eea-105">[] Specifikace: Nezahájeno</span><span class="sxs-lookup"><span data-stu-id="92eea-105">[ ] Specification: Not started</span></span>

## <a name="summary"></a><span data-ttu-id="92eea-106">Souhrn</span><span class="sxs-lookup"><span data-stu-id="92eea-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="92eea-107">Třídy mohou mít seznam parametrů a v případě, že mají, jejich specifikace základní třídy může mít seznam argumentů.</span><span class="sxs-lookup"><span data-stu-id="92eea-107">Classes can have a parameter list, and when they do, their base class specification can have an argument list.</span></span>
<span data-ttu-id="92eea-108">Parametry primárního konstruktoru jsou v oboru v celé deklaraci třídy, a pokud jsou zachyceny členem funkce nebo anonymní funkcí, jsou uloženy jako soukromá pole ve třídě.</span><span class="sxs-lookup"><span data-stu-id="92eea-108">Primary constructor parameters are in scope throughout the class declaration, and if they are captured by a function member or anonymous function, they are stored as private fields in the class.</span></span>

## <a name="motivation"></a><span data-ttu-id="92eea-109">Motivační</span><span class="sxs-lookup"><span data-stu-id="92eea-109">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="92eea-110">V kódu inicializace programu je běžné mít spoustu často.</span><span class="sxs-lookup"><span data-stu-id="92eea-110">It is common to have a lot of boilerplate in program initialization code.</span></span> <span data-ttu-id="92eea-111">V obecném případě je konkrétní část dat `x` uvedena mnohokrát:</span><span class="sxs-lookup"><span data-stu-id="92eea-111">In the general case, a given piece of data `x` is mentioned many times:</span></span>

- <span data-ttu-id="92eea-112">Jako soukromé pole `_x`</span><span class="sxs-lookup"><span data-stu-id="92eea-112">As a private field `_x`</span></span>
- <span data-ttu-id="92eea-113">Jako parametr `x` do konstruktoru</span><span class="sxs-lookup"><span data-stu-id="92eea-113">As a parameter `x` to a constructor</span></span>
- <span data-ttu-id="92eea-114">V `_x = x;` přiřazení pole z parametru v konstruktoru</span><span class="sxs-lookup"><span data-stu-id="92eea-114">In an assignment `_x = x;` of the field from the parameter in the constructor</span></span>
- <span data-ttu-id="92eea-115">Jako vlastnost `X`</span><span class="sxs-lookup"><span data-stu-id="92eea-115">As a property `X`</span></span>
- <span data-ttu-id="92eea-116">V `x = value;` setter vlastnosti</span><span class="sxs-lookup"><span data-stu-id="92eea-116">In the property setter `x = value;`</span></span>
- <span data-ttu-id="92eea-117">Ve vlastnosti getter `return x;`</span><span class="sxs-lookup"><span data-stu-id="92eea-117">In the property getter `return x;`</span></span>

``` c#
class C
{
    private string _x;
    
    public C(string x)
    {
        _x = x;
    }
    public string X
    {
        get => _x;
        set { if (value == null) throw new NullArgumentException(nameof(X)); _x = value; }
    }
}
```

<span data-ttu-id="92eea-118">Pro vlastnosti, které nevyžadují ověřování nebo výpočet, se únavných úkolů dá snížit pomocí automatických vlastností, takže je potřeba deklarovat explicitní pole zálohování pro vlastnost.</span><span class="sxs-lookup"><span data-stu-id="92eea-118">For properties that don't require validation or computation, the tedium can be reduced using auto-properties, thus cutting out the need to declare an explicit backing field for the property.</span></span> <span data-ttu-id="92eea-119">Pokud ale vaše vlastnost vyžaduje jakýkoli druh logiky nad rámec toho, co poskytuje Automatická vlastnost, je výše uvedená nejlepší.</span><span class="sxs-lookup"><span data-stu-id="92eea-119">But if your property requires any sort of logic beyond what an auto-property provides, the above is the best you an do.</span></span>

<span data-ttu-id="92eea-120">Primární konstruktory místo toho snižují režii tím, že zadáte argumenty konstruktoru přímo v rozsahu celé třídy a znovu už nemusí komplikovaně nutnost explicitně deklarovat pole pro zálohování.</span><span class="sxs-lookup"><span data-stu-id="92eea-120">Primary constructors instead reduce the overhead by putting constructor arguments directly in scope throughout the class, again obviating the need to explicitly declare a backing field.</span></span> <span data-ttu-id="92eea-121">Proto by byl výše uvedený příklad následující:</span><span class="sxs-lookup"><span data-stu-id="92eea-121">Thus, the above example would become:</span></span>

``` c#
class C(string x)
{
    public string X
    {
        get => x;
        set { if (value == null) throw new NullArgumentException(nameof(X)); x = value; }
    }
}
```

<span data-ttu-id="92eea-122">V tomto příkladu primární konstruktor snižuje počet pojmenovaných entit pro `x` od tří do dvou a už nemusí komplikovaně pole `_x` zálohování.</span><span class="sxs-lookup"><span data-stu-id="92eea-122">In this example, the primary constructor reduces the number of named entities for `x` from three to two, obviating the `_x` backing field.</span></span> <span data-ttu-id="92eea-123">Odstraní dvě ze tří členských deklarací (zachovají pouze deklaraci vlastnosti) a sníží celkový počet zmínk `x`/`_x`/`X` od osmi do pěti.</span><span class="sxs-lookup"><span data-stu-id="92eea-123">It removes two out of three member declarations (keeping only the property declaration itself), and reduces the total number of mentions of `x`/`_x`/`X` from eight to five.</span></span>


## <a name="detailed-design"></a><span data-ttu-id="92eea-124">Podrobný návrh</span><span class="sxs-lookup"><span data-stu-id="92eea-124">Detailed design</span></span>
[design]: #detailed-design

<span data-ttu-id="92eea-125">Třídy mohou mít seznam parametrů:</span><span class="sxs-lookup"><span data-stu-id="92eea-125">Classes can have a parameter list:</span></span>

``` c#
public class C(int i, string s)
{
    ...
}
```

<span data-ttu-id="92eea-126">Seznam parametrů způsobí, že konstruktor bude implicitně deklarován pro třídu, se stejnou přístupností jako samotná třída.</span><span class="sxs-lookup"><span data-stu-id="92eea-126">The parameter list causes a constructor to be implicitly declared for the class, with the same accessibility as the class itself.</span></span>

``` c#
new C(5, "Hello");
```

<span data-ttu-id="92eea-127">Parametry primárního konstruktoru jsou v oboru celé tělo třídy.</span><span class="sxs-lookup"><span data-stu-id="92eea-127">Primary constructor parameters are in scope throughout the class body.</span></span> <span data-ttu-id="92eea-128">Pokud jsou zachyceny členem funkce nebo anonymní funkcí, budou uloženy jako soukromá pole ve třídě.</span><span class="sxs-lookup"><span data-stu-id="92eea-128">If they are captured by a function member or anonymous function, they become stored as private fields in the class.</span></span> <span data-ttu-id="92eea-129">Pokud jsou používány pouze během inicializace, nebudou uloženy v objektu.</span><span class="sxs-lookup"><span data-stu-id="92eea-129">If they are only used during initialization they will not be stored in the object.</span></span>

``` c#
public class C(int i, string s)
{
    int[] a = new int[i]; // i not captured
    public int S => s;    // s captured
}
```

<span data-ttu-id="92eea-130">Pokud třída s primárním konstruktorem má specifikaci základní třídy, může mít jeden seznam argumentů.</span><span class="sxs-lookup"><span data-stu-id="92eea-130">If a class with a primary constructor has a base class specification, that one can have an argument list.</span></span> <span data-ttu-id="92eea-131">Slouží jako seznam argumentů pro `base(...)` inicializátor implicitně deklarovaného konstruktoru.</span><span class="sxs-lookup"><span data-stu-id="92eea-131">This serves as the argument list to a `base(...)` initializer of the implicitly declared constructor.</span></span> <span data-ttu-id="92eea-132">Pokud není zadaný žádný seznam argumentů, předpokládá se prázdný.</span><span class="sxs-lookup"><span data-stu-id="92eea-132">If no argument list is provided, an empty one is assumed.</span></span>

``` c#
public class C(int i, string s) : B(s)
{
    ...
}
```
<span data-ttu-id="92eea-133">Třída může mít explicitně definované konstruktory, ale všechny musí používat inicializátor `this(...)`.</span><span class="sxs-lookup"><span data-stu-id="92eea-133">The class can have explicitly defined constructors as well, but those all have to use a `this(...)` initializer.</span></span> <span data-ttu-id="92eea-134">Tím je zajištěno, že primární konstruktor je vždy volán při vytvoření nové instance.</span><span class="sxs-lookup"><span data-stu-id="92eea-134">This ensures that the primary constructor is always called when a new instance is constructed.</span></span>

<span data-ttu-id="92eea-135">Všechny Inicializátory v těle třídy se stanou přiřazeními ve vygenerovaném konstruktoru.</span><span class="sxs-lookup"><span data-stu-id="92eea-135">All initializers in the class body will become assignments in the generated constructor.</span></span> <span data-ttu-id="92eea-136">To znamená, že na rozdíl od jiných tříd se Inicializátory spustí *po* vyvolání základního konstruktoru, ne před.</span><span class="sxs-lookup"><span data-stu-id="92eea-136">This means that, unlike other classes, initializers will run *after* the base constructor has been invoked, not before.</span></span> <span data-ttu-id="92eea-137">Kromě toho generovaná třída bude obsahovat přiřazení pro inicializaci všech privátních polí, která byla vygenerována pro uložení parametrů primárního konstruktoru, které byly zachyceny členy.</span><span class="sxs-lookup"><span data-stu-id="92eea-137">In addition, the generated class will contain assignments to initialize any private fields that were generated to store primary constructor parameters that were captured by members.</span></span> <span data-ttu-id="92eea-138">Tyto členy se přepíší pro použití privátního pole namísto parametru způsobem podobným uzavření pro výrazy lambda.</span><span class="sxs-lookup"><span data-stu-id="92eea-138">Those members are rewritten to use the private field instead of the parameter in a manner similar to closures for lambda expressions.</span></span> <span data-ttu-id="92eea-139">Vygenerovaná primární pole jsou inicializována jako první a poté jsou vygenerovaná Inicializátory spouštěny v pořadí podle vzhledu třídy.</span><span class="sxs-lookup"><span data-stu-id="92eea-139">The generated primary fields are initialized first, and then the initializer-generated assignments are executed in the order of appearance in the class.</span></span>

<span data-ttu-id="92eea-140">V předchozím příkladu je účinek deklarace třídy podobný jako při přepsání takto:</span><span class="sxs-lookup"><span data-stu-id="92eea-140">For the above example, the effect of the class declaration is as if rewritten like this:</span></span>

``` c#
public class C : B
{
    public C(int i, string s) : base(s)
    {
        __s = s;        // store parameter s for captured use
        a = new int[i]; // initialize a
    }
    int __s; // generated field for capture of s
    
    int[] a;
    public int S => __s; // s replaced with captured __s
}
```

<span data-ttu-id="92eea-141">Zachycení má podobná omezení pro zachycení místních proměnných pomocí výrazů lambda.</span><span class="sxs-lookup"><span data-stu-id="92eea-141">The capture has similar restrictions to the capture of local variables by lambda expressions.</span></span> <span data-ttu-id="92eea-142">Například parametry `ref` a `out` jsou povoleny v primárních konstruktorech, ale nelze je zachytit na své členské subjekty.</span><span class="sxs-lookup"><span data-stu-id="92eea-142">For instance, `ref` and `out` parameters are allowed in primary constructors, but cannot be captured my member bodies.</span></span>


## <a name="drawbacks"></a><span data-ttu-id="92eea-143">Nevýhody</span><span class="sxs-lookup"><span data-stu-id="92eea-143">Drawbacks</span></span>
[drawbacks]: #drawbacks

<span data-ttu-id="92eea-144">V hrubém pořadí podle významnosti.</span><span class="sxs-lookup"><span data-stu-id="92eea-144">In rough order of significance.</span></span>

* <span data-ttu-id="92eea-145">Návrh používá syntaxi, která byla navržena také pro poziční záznamy.</span><span class="sxs-lookup"><span data-stu-id="92eea-145">The proposal uses syntax that has also been proposed for positional records.</span></span> <span data-ttu-id="92eea-146">Pokud si vyžádali obě funkce, je nutné mít nějaká ubytování.</span><span class="sxs-lookup"><span data-stu-id="92eea-146">If we desire both features, some accommodation is required.</span></span> <span data-ttu-id="92eea-147">Například</span><span class="sxs-lookup"><span data-stu-id="92eea-147">E.g.</span></span> <span data-ttu-id="92eea-148">byl navržený modifikátor `data` u záznamů.</span><span class="sxs-lookup"><span data-stu-id="92eea-148">a `data` modifier on records has been proposed.</span></span>
* <span data-ttu-id="92eea-149">Velikost alokace konstruovaných objektů je méně zřejmá, protože kompilátor určuje, zda se má přidělit pole pro parametr primárního konstruktoru na základě úplného textu třídy.</span><span class="sxs-lookup"><span data-stu-id="92eea-149">The allocation size of constructed objects is less obvious, as the compiler determines whether to allocate a field for a primary constructor parameter based on the full text of the class.</span></span> <span data-ttu-id="92eea-150">Toto riziko je podobné implicitnímu zachycení proměnných pomocí výrazů lambda.</span><span class="sxs-lookup"><span data-stu-id="92eea-150">This risk is similar to the implicit capture of variables by lambda expressions.</span></span>
* <span data-ttu-id="92eea-151">Běžný pokušení (nebo nechtěného vzoru) může být zachytit "stejný" parametr na více úrovních dědičnosti, protože je předán řetězu konstruktoru namísto explicitního allotting IT chráněného pole v základní třídě, což vede k duplicitním přidělením. pro stejná data v objektech.</span><span class="sxs-lookup"><span data-stu-id="92eea-151">A common temptation (or accidental pattern) might be to capture the "same" parameter at multiple levels of inheritance as it is passed up the constructor chain instead of explicitly allotting it a protected field at the base class, leading to duplicated allocations for the same data in objects.</span></span> <span data-ttu-id="92eea-152">To je velmi podobné jako dnešní riziko přepsání automatických vlastností pomocí automatických vlastností.</span><span class="sxs-lookup"><span data-stu-id="92eea-152">This is very similar to today's risk of overriding auto-properties with auto-properties.</span></span> 
* <span data-ttu-id="92eea-153">Jak bylo navrženo výše, neexistuje žádné místo pro další logiku, která by mohla být obvykle vyjádřena v podinstitucích konstruktoru.</span><span class="sxs-lookup"><span data-stu-id="92eea-153">As proposed above, there is no place for additional logic that might usually expressed in constructor bodies.</span></span> <span data-ttu-id="92eea-154">Přípona "primárního konstruktoru" níže uvedená adresa.</span><span class="sxs-lookup"><span data-stu-id="92eea-154">The "Primary constructor bodies" extension below addresses that.</span></span>
* <span data-ttu-id="92eea-155">Jak je navrženo, sémantika pořadí spouštění se neliší od běžných konstruktorů.</span><span class="sxs-lookup"><span data-stu-id="92eea-155">As proposed, execution order semantics are subtly different than with ordinary constructors.</span></span> <span data-ttu-id="92eea-156">To může být způsobeno tím, že je to u některých návrhů rozšíření (zejména "primárních" institucí "primárních konstruktorů") náklady.</span><span class="sxs-lookup"><span data-stu-id="92eea-156">This could probably be remedied, but at the cost of some of the extension proposals (notably "Primary constructor bodies").</span></span>
* <span data-ttu-id="92eea-157">Návrh funguje pouze v případě, že jeden konstruktor může být určen jako primární.</span><span class="sxs-lookup"><span data-stu-id="92eea-157">The proposal only works when a single constructor can be designated primary.</span></span>
* <span data-ttu-id="92eea-158">Neexistuje žádný způsob, jak mít samostatnou přístupnost třídy a primárního konstruktoru.</span><span class="sxs-lookup"><span data-stu-id="92eea-158">There is no way to have separate accessibility of the class and the primary constructor.</span></span> <span data-ttu-id="92eea-159">Například v situacích, kdy veřejné konstruktory jsou delegovány na jeden privátní konstruktor "Build-All", který by byl potřeba.</span><span class="sxs-lookup"><span data-stu-id="92eea-159">For instance, in situations where public constructors all delegate to one private "build-it-all" constructor that would be needed.</span></span> <span data-ttu-id="92eea-160">V případě potřeby může být pro vás navržena syntaxe později.</span><span class="sxs-lookup"><span data-stu-id="92eea-160">If necessary, syntax could be proposed for that later.</span></span>


## <a name="alternatives"></a><span data-ttu-id="92eea-161">Alternativy</span><span class="sxs-lookup"><span data-stu-id="92eea-161">Alternatives</span></span>
[alternatives]: #alternatives

<span data-ttu-id="92eea-162">Fulltextové záznamy mohou být alternativou nebo mohou koexistovat s primárními konstruktory v závislosti na konkrétních událostech.</span><span class="sxs-lookup"><span data-stu-id="92eea-162">Full-on positional records may be an alternative, or may coexist with primary constructors, depending on the specifics.</span></span> <span data-ttu-id="92eea-163">Umožňují *více* zkratky v *menším* počtu scénářů.</span><span class="sxs-lookup"><span data-stu-id="92eea-163">They would allow for *more* abbreviation in a *smaller* number of scenarios.</span></span> <span data-ttu-id="92eea-164">Takže obě jsou potenciálně užitečné, ale obě můžou být přehnaně důkladné, pokud je nemůžete trochu snadno integrovat.</span><span class="sxs-lookup"><span data-stu-id="92eea-164">So both are potentially useful, but having both may be overkill, unless they can be somewhat neatly integrated with each other.</span></span>


## <a name="possible-extensions"></a><span data-ttu-id="92eea-165">Možná rozšíření</span><span class="sxs-lookup"><span data-stu-id="92eea-165">Possible extensions</span></span>
[extensions]: #possible-extensions

<span data-ttu-id="92eea-166">Jedná se o variace nebo doplňky základního návrhu, které mohou být zváženy ve spojení s ní, nebo v pozdější fázi, pokud je to považováno za užitečné.</span><span class="sxs-lookup"><span data-stu-id="92eea-166">These are variations or additions to the core proposal that may be considered in conjunction with it, or at a later stage if deemed useful.</span></span>

### <a name="primary-constructor-bodies"></a><span data-ttu-id="92eea-167">Základní subjekty konstruktoru</span><span class="sxs-lookup"><span data-stu-id="92eea-167">Primary constructor bodies</span></span>

<span data-ttu-id="92eea-168">Samotné konstruktory často obsahují logiku ověřování parametrů nebo jiný kód inicializace netriviální, který nelze vyjádřit jako Inicializátory.</span><span class="sxs-lookup"><span data-stu-id="92eea-168">Constructors themselves often contain parameter validation logic or other nontrivial initialization code that cannot be expressed as initializers.</span></span>

<span data-ttu-id="92eea-169">Primární konstruktory mohou být rozšířeny, aby bylo možné zobrazit bloky příkazů přímo v těle třídy.</span><span class="sxs-lookup"><span data-stu-id="92eea-169">Primary constructors could be extended to allow statement blocks to appear directly in the class body.</span></span> <span data-ttu-id="92eea-170">Tyto příkazy by byly vloženy do generovaného konstruktoru v místě, kde se zobrazují mezi inicializací přiřazení, a by tedy byly provedeny s použitím inicializátorů.</span><span class="sxs-lookup"><span data-stu-id="92eea-170">Those statements would be inserted in the generated constructor at the point where they appear between initializing assignments, and would thus be executed interspersed with initializers.</span></span> <span data-ttu-id="92eea-171">Příklad:</span><span class="sxs-lookup"><span data-stu-id="92eea-171">For instance:</span></span>

``` c#
public class C(int i, string s) : B(s)
{
    {
        if (i < 0) throw new ArgumentOutOfRangeException(nameof(i));
    }
    int[] a = new int[i];
    public int S => s;
}
```

### <a name="initializer-fields-and-initializer-functions"></a><span data-ttu-id="92eea-172">Inicializační pole a inicializační funkce</span><span class="sxs-lookup"><span data-stu-id="92eea-172">Initializer fields and initializer functions</span></span>

<span data-ttu-id="92eea-173">Ve třídě s primárním konstruktorem můžeme vzít v úvahu deklarace polí a metod bez modifikátorů dostupnosti tak, aby byly větší jako lokální proměnné a místní funkce:</span><span class="sxs-lookup"><span data-stu-id="92eea-173">In a class with a primary constructor we could consider field and method declarations without accessibility modifiers to be more like local variables and local functions:</span></span>

* <span data-ttu-id="92eea-174">Stejně jako u parametrů primárního konstruktoru je "inicializační pole" zachycena pouze v samotném soukromém poli, pokud byly použity v členech Functions.</span><span class="sxs-lookup"><span data-stu-id="92eea-174">Just like primary constructor parameters the "initializer fields" would only be captured into an actual private field if they were used in function members.</span></span>
* <span data-ttu-id="92eea-175">"Inicializační funkce" by se měly považovat za zachycení parametrů primárního konstruktoru a polí inicializátoru, pokud byly použity sami v jiných členech funkce.</span><span class="sxs-lookup"><span data-stu-id="92eea-175">The "initializer functions" would only be considered to capture primary constructor parameters and initializer fields if they were themselves used in other function members.</span></span> <span data-ttu-id="92eea-176">Pokud není zachycena, mohou být vygenerována lépe, například místními funkcemi.</span><span class="sxs-lookup"><span data-stu-id="92eea-176">If not captured, they could be generated in a more optimal fashion, like local functions.</span></span>
* <span data-ttu-id="92eea-177">Stejně jako parametry primárního konstruktoru by nebyly k dispozici prostřednictvím přístupu členů, ale pouze jako jednoduchý název.</span><span class="sxs-lookup"><span data-stu-id="92eea-177">Just like primary constructor parameters they would not be available via member access, but only as a simple name.</span></span>

<span data-ttu-id="92eea-178">To lze použít pro dočasné proměnné a pomocné funkce, které jsou relevantní pouze pro inicializaci:</span><span class="sxs-lookup"><span data-stu-id="92eea-178">This could be used for temporary variables and helper functions that are only relevant to initialization:</span></span>

``` c#
public class C(string s)
{
    int size = s.Length;             // not captured
    int[] Create() => new int[size]; // not captured
    int[] a = Create();
    ...
}
```

<span data-ttu-id="92eea-179">To může být příliš jemné, zejména protože absence modifikátorů dostupnosti jinde znamená `private`.</span><span class="sxs-lookup"><span data-stu-id="92eea-179">This may be too subtle, especially since the absence of accessibility modifiers elsewhere simply means `private`.</span></span> 

### <a name="initializer-statements"></a><span data-ttu-id="92eea-180">Příkazy inicializátoru</span><span class="sxs-lookup"><span data-stu-id="92eea-180">Initializer statements</span></span>

<span data-ttu-id="92eea-181">Kombinace odmocniny výše uvedeného do rozšíření by mohla jednoduše dovolit příkazy přímo v těle třídy.</span><span class="sxs-lookup"><span data-stu-id="92eea-181">A radical combination of the above to extensions would be to simply allow statements directly in the class body.</span></span> <span data-ttu-id="92eea-182">Tyto příkazy jsou přesně stejné jako výše navržené tělo konstruktoru, s tím rozdílem, že není nutné je uzavřít do `{ }`.</span><span class="sxs-lookup"><span data-stu-id="92eea-182">Such statements are exactly as the interspersed constructor bodies proposed above, except they don't need to be enclosed in `{ }`.</span></span> <span data-ttu-id="92eea-183">Aby byla tato možnost dostatečně užitečná, "místní" proměnné a pomocné funkce by musel být také vyhodnotit na nejvyšší úrovni třídy, způsobem Prozkoumávám v části rozšíření "inicializační pole a inicializační funkce" výše.</span><span class="sxs-lookup"><span data-stu-id="92eea-183">For this to be sufficiently useful, "local" variables and helper functions would need to also be expressible at the top level of the class, in the manner explored in the extension "Initializer fields and initializer functions" above.</span></span>


### <a name="member-access"></a><span data-ttu-id="92eea-184">Přístup ke členům</span><span class="sxs-lookup"><span data-stu-id="92eea-184">Member access</span></span>

<span data-ttu-id="92eea-185">Základní návrh zpracovává parametry primárního konstruktoru jako parametry, které mohou být pouze označovány jako jednoduché názvy.</span><span class="sxs-lookup"><span data-stu-id="92eea-185">The core proposal treats primary constructor parameters as parameters that can only be referred as simple names.</span></span> <span data-ttu-id="92eea-186">Alternativou je umožnění odkazů, jako by se jednalo o soukromá pole, tj. s přístupem členů, a *to i* v případě, že se někdy negenerují jako pole.</span><span class="sxs-lookup"><span data-stu-id="92eea-186">An alternative is to allow them to be referenced as if they were private fields, i.e. with a member access, *even* if they are sometimes not generated as fields.</span></span> <span data-ttu-id="92eea-187">To by jim mohlo být odkazováno jako `this.x`, když jsou Stínově vytvořeny pomocí místních proměnných a jsou k nim přistupované z jiné instance jako `other.x`.</span><span class="sxs-lookup"><span data-stu-id="92eea-187">This would allow them to be referenced as `this.x` when shadowed by local variables, and accessed from a different instance as `other.x`.</span></span>

<span data-ttu-id="92eea-188">Pokud se použije na rozšíření "pole inicializátoru a inicializační funkce", tím se také sníží míra, se kterou se tyto hodnoty lišily od běžných privátních členů.</span><span class="sxs-lookup"><span data-stu-id="92eea-188">If applied to the "initializer fields and initializer functions" extension this would also reduce the degree to which those were different from ordinary private members.</span></span> <span data-ttu-id="92eea-189">Jediným rozdílem by pak bylo, že kompilátor je volné, aby je Elide z objektu, pokud se používá pouze během inicializace.</span><span class="sxs-lookup"><span data-stu-id="92eea-189">The only difference would then be that the compiler is free to elide them from the object if only used during initialization.</span></span>

