---
ms.openlocfilehash: ff31585520c9090ad92893a930327112743c8e77
ms.sourcegitcommit: 892af9016b3317a8fae12d195014dc38ba51cf16
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/01/2019
ms.locfileid: "71704010"
---
# <a name="basic-concepts"></a><span data-ttu-id="5b04b-101">Základní koncepty</span><span class="sxs-lookup"><span data-stu-id="5b04b-101">Basic concepts</span></span>

## <a name="application-startup"></a><span data-ttu-id="5b04b-102">Spuštění aplikace</span><span class="sxs-lookup"><span data-stu-id="5b04b-102">Application Startup</span></span>

<span data-ttu-id="5b04b-103">Sestavení, které má ***vstupní bod*** , se nazývá ***aplikace***.</span><span class="sxs-lookup"><span data-stu-id="5b04b-103">An assembly that has an ***entry point*** is called an ***application***.</span></span> <span data-ttu-id="5b04b-104">Při spuštění aplikace se vytvoří nová ***doména aplikace*** .</span><span class="sxs-lookup"><span data-stu-id="5b04b-104">When an application is run, a new ***application domain*** is created.</span></span> <span data-ttu-id="5b04b-105">V jednom počítači může existovat několik různých instancí aplikace současně a každá z nich má svou vlastní doménu aplikace.</span><span class="sxs-lookup"><span data-stu-id="5b04b-105">Several different instantiations of an application may exist on the same machine at the same time, and each has its own application domain.</span></span>

<span data-ttu-id="5b04b-106">Doména aplikace umožňuje izolaci aplikace působit jako kontejner pro stav aplikace.</span><span class="sxs-lookup"><span data-stu-id="5b04b-106">An application domain enables application isolation by acting as a container for application state.</span></span> <span data-ttu-id="5b04b-107">Doména aplikace funguje jako kontejner a hranice pro typy definované v aplikaci a knihovny tříd, které používá.</span><span class="sxs-lookup"><span data-stu-id="5b04b-107">An application domain acts as a container and boundary for the types defined in the application and the class libraries it uses.</span></span> <span data-ttu-id="5b04b-108">Typy načtené do jedné domény aplikace se liší od stejného typu načteného do jiné domény aplikace a instance objektů nejsou přímo sdíleny mezi doménami aplikace.</span><span class="sxs-lookup"><span data-stu-id="5b04b-108">Types loaded into one application domain are distinct from the same type loaded into another application domain, and instances of objects are not directly shared between application domains.</span></span> <span data-ttu-id="5b04b-109">Každý aplikační doména má například vlastní kopii statických proměnných pro tyto typy a statický konstruktor pro typ je spuštěn nejvýše jednou pro doménu aplikace.</span><span class="sxs-lookup"><span data-stu-id="5b04b-109">For instance, each application domain has its own copy of static variables for these types, and a static constructor for a type is run at most once per application domain.</span></span> <span data-ttu-id="5b04b-110">Implementace jsou bezplatné pro poskytování zásad nebo mechanismů specifických pro implementaci pro vytváření a zničení aplikačních domén.</span><span class="sxs-lookup"><span data-stu-id="5b04b-110">Implementations are free to provide implementation-specific policy or mechanisms for the creation and destruction of application domains.</span></span>

<span data-ttu-id="5b04b-111">***Spuštění aplikace*** nastane, pokud spouštěcí prostředí volá určenou metodu, která je označována jako vstupní bod aplikace.</span><span class="sxs-lookup"><span data-stu-id="5b04b-111">***Application startup*** occurs when the execution environment calls a designated method, which is referred to as the application's entry point.</span></span> <span data-ttu-id="5b04b-112">Tato metoda vstupního bodu je vždy pojmenována `Main`a může mít jednu z následujících signatur:</span><span class="sxs-lookup"><span data-stu-id="5b04b-112">This entry point method is always named `Main`, and can have one of the following signatures:</span></span>

```csharp
static void Main() {...}

static void Main(string[] args) {...}

static int Main() {...}

static int Main(string[] args) {...}
```

<span data-ttu-id="5b04b-113">Jak je znázorněno, může vstupní bod volitelně vracet `int` hodnotu.</span><span class="sxs-lookup"><span data-stu-id="5b04b-113">As shown, the entry point may optionally return an `int` value.</span></span> <span data-ttu-id="5b04b-114">Tato návratová hodnota se používá při ukončení aplikace ([ukončení aplikace](basic-concepts.md#application-termination)).</span><span class="sxs-lookup"><span data-stu-id="5b04b-114">This return value is used in application termination ([Application termination](basic-concepts.md#application-termination)).</span></span>

<span data-ttu-id="5b04b-115">Vstupní bod může volitelně mít jeden formální parametr.</span><span class="sxs-lookup"><span data-stu-id="5b04b-115">The entry point may optionally have one formal parameter.</span></span> <span data-ttu-id="5b04b-116">Parametr může mít libovolný název, ale typ parametru musí být `string[]`.</span><span class="sxs-lookup"><span data-stu-id="5b04b-116">The parameter may have any name, but the type of the parameter must be `string[]`.</span></span> <span data-ttu-id="5b04b-117">Pokud je k dispozici formální parametr, spouštěcí prostředí vytvoří a předává `string[]` argument obsahující argumenty příkazového řádku, které byly zadány při spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="5b04b-117">If the formal parameter is present, the execution environment creates and passes a `string[]` argument containing the command-line arguments that were specified when the application was started.</span></span> <span data-ttu-id="5b04b-118">Argument `string[]` nemá nikdy hodnotu null, ale v případě, že nebyly zadány žádné argumenty příkazového řádku, může mít nulovou délku.</span><span class="sxs-lookup"><span data-stu-id="5b04b-118">The `string[]` argument is never null, but it may have a length of zero if no command-line arguments were specified.</span></span>

<span data-ttu-id="5b04b-119">Vzhledem C# k tomu, že podporuje přetížení metody, třída nebo struktura může obsahovat více definicí některé metody, pokud každý má jiný podpis.</span><span class="sxs-lookup"><span data-stu-id="5b04b-119">Since C# supports method overloading, a class or struct may contain multiple definitions of some method, provided each has a different signature.</span></span> <span data-ttu-id="5b04b-120">V rámci jednoho programu však žádná třída ani struktura nesmí obsahovat více než jednu metodu nazvanou `Main` jejíž definice je pro použití jako vstupní bod aplikace.</span><span class="sxs-lookup"><span data-stu-id="5b04b-120">However, within a single program, no class or struct may contain more than one method called `Main` whose definition qualifies it to be used as an application entry point.</span></span> <span data-ttu-id="5b04b-121">Další přetížené verze `Main` jsou povoleny, avšak za předpokladu, že mají více než jeden parametr, nebo je jejich jediný parametr jiný než typ `string[]`.</span><span class="sxs-lookup"><span data-stu-id="5b04b-121">Other overloaded versions of `Main` are permitted, however, provided they have more than one parameter, or their only parameter is other than type `string[]`.</span></span>

<span data-ttu-id="5b04b-122">Aplikace může být tvořena více třídami nebo strukturami.</span><span class="sxs-lookup"><span data-stu-id="5b04b-122">An application can be made up of multiple classes or structs.</span></span> <span data-ttu-id="5b04b-123">Je možné, aby více než jedna z těchto tříd nebo struktur obsahovala metodu nazvanou `Main` jejíž definice je určena k použití jako vstupní bod aplikace.</span><span class="sxs-lookup"><span data-stu-id="5b04b-123">It is possible for more than one of these classes or structs to contain a method called `Main` whose definition qualifies it to be used as an application entry point.</span></span> <span data-ttu-id="5b04b-124">V takových případech je nutné použít externí mechanismus (například možnost kompilátoru příkazového řádku) k výběru jedné z následujících metod `Main` jako vstupní bod.</span><span class="sxs-lookup"><span data-stu-id="5b04b-124">In such cases, an external mechanism (such as a command-line compiler option) must be used to select one of these `Main` methods as the entry point.</span></span>

<span data-ttu-id="5b04b-125">V C#nástroji musí být každá metoda definována jako člen třídy nebo struktury.</span><span class="sxs-lookup"><span data-stu-id="5b04b-125">In C#, every method must be defined as a member of a class or struct.</span></span> <span data-ttu-id="5b04b-126">Obecně deklarovaná přístupnost ([deklarovaná přístupnost](basic-concepts.md#declared-accessibility)) metody je určena modifikátory přístupu ([modifikátory přístupu](classes.md#access-modifiers)) určenými v deklaraci a podobně deklarovaná přístupnost typu je určena modifikátory přístupu určenými v deklaraci.</span><span class="sxs-lookup"><span data-stu-id="5b04b-126">Ordinarily, the declared accessibility ([Declared accessibility](basic-concepts.md#declared-accessibility)) of a method is determined by the access modifiers ([Access modifiers](classes.md#access-modifiers)) specified in its declaration, and similarly the declared accessibility of a type is determined by the access modifiers specified in its declaration.</span></span> <span data-ttu-id="5b04b-127">Aby bylo možné volat danou metodu daného typu, musí být typ i člen přístupný.</span><span class="sxs-lookup"><span data-stu-id="5b04b-127">In order for a given method of a given type to be callable, both the type and the member must be accessible.</span></span> <span data-ttu-id="5b04b-128">Vstupní bod aplikace je však zvláštní případ.</span><span class="sxs-lookup"><span data-stu-id="5b04b-128">However, the application entry point is a special case.</span></span> <span data-ttu-id="5b04b-129">Konkrétně spouštěcí prostředí má přístup ke vstupnímu bodu aplikace bez ohledu na jeho deklaraci, a to bez ohledu na deklaraci přístupnosti jejich nadřazených typů deklarací.</span><span class="sxs-lookup"><span data-stu-id="5b04b-129">Specifically, the execution environment can access the application's entry point regardless of its declared accessibility and regardless of the declared accessibility of its enclosing type declarations.</span></span>

<span data-ttu-id="5b04b-130">Metoda vstupního bodu aplikace nemůže být v deklaraci obecné třídy.</span><span class="sxs-lookup"><span data-stu-id="5b04b-130">The application entry point method may not be in a generic class declaration.</span></span>

<span data-ttu-id="5b04b-131">Ve všech ostatních ohledech se metody vstupního bodu chovají stejně jako ty, které nejsou vstupními body.</span><span class="sxs-lookup"><span data-stu-id="5b04b-131">In all other respects, entry point methods behave like those that are not entry points.</span></span>

## <a name="application-termination"></a><span data-ttu-id="5b04b-132">Ukončení aplikace</span><span class="sxs-lookup"><span data-stu-id="5b04b-132">Application termination</span></span>

<span data-ttu-id="5b04b-133">***Ukončení aplikace*** vrátí řízení spouštěcímu prostředí.</span><span class="sxs-lookup"><span data-stu-id="5b04b-133">***Application termination*** returns control to the execution environment.</span></span>

<span data-ttu-id="5b04b-134">Pokud je návratový typ metody ***vstupního bodu*** aplikace `int`, vrácená hodnota slouží jako ***kód stavu ukončení***aplikace.</span><span class="sxs-lookup"><span data-stu-id="5b04b-134">If the return type of the application's ***entry point*** method is `int`, the value returned serves as the application's ***termination status code***.</span></span> <span data-ttu-id="5b04b-135">Účelem tohoto kódu je umožnění komunikace s úspěchem nebo selháním spouštěcího prostředí.</span><span class="sxs-lookup"><span data-stu-id="5b04b-135">The purpose of this code is to allow communication of success or failure to the execution environment.</span></span>

<span data-ttu-id="5b04b-136">Pokud je návratový typ metody vstupního bodu `void`, dosáhnete pravé složené závorky (`}`), která tuto metodu ukončí, nebo provedením příkazu `return`, který nemá žádný výraz, výsledkem je stavový kód ukončení `0`.</span><span class="sxs-lookup"><span data-stu-id="5b04b-136">If the return type of the entry point method is `void`, reaching the right brace (`}`) which terminates that method, or executing a `return` statement that has no expression, results in a termination status code of `0`.</span></span>

<span data-ttu-id="5b04b-137">Před ukončením aplikace jsou destruktory pro všechny své objekty, které ještě nebyly shromažďovány do paměti, volány, pokud takové vyčištění nebylo potlačeno (voláním metody knihovny `GC.SuppressFinalize`, například).</span><span class="sxs-lookup"><span data-stu-id="5b04b-137">Prior to an application's termination, destructors for all of its objects that have not yet been garbage collected are called, unless such cleanup has been suppressed (by a call to the library method `GC.SuppressFinalize`, for example).</span></span>

## <a name="declarations"></a><span data-ttu-id="5b04b-138">Deklarace</span><span class="sxs-lookup"><span data-stu-id="5b04b-138">Declarations</span></span>

<span data-ttu-id="5b04b-139">Deklarace v C# programu definují prvky prvku programu.</span><span class="sxs-lookup"><span data-stu-id="5b04b-139">Declarations in a C# program define the constituent elements of the program.</span></span> <span data-ttu-id="5b04b-140">C#programy jsou uspořádány pomocí oborů názvů ([obory](namespaces.md)názvů), které mohou obsahovat deklarace typu a vnořené deklarace oboru názvů.</span><span class="sxs-lookup"><span data-stu-id="5b04b-140">C# programs are organized using namespaces ([Namespaces](namespaces.md)), which can contain type declarations and nested namespace declarations.</span></span> <span data-ttu-id="5b04b-141">Deklarace typu ([deklarace typu](namespaces.md#type-declarations)) se používají k definování tříd ([tříd](classes.md)), struktur ([struktur](structs.md)), rozhraní ([rozhraní](interfaces.md)), výčtů ([výčtů](enums.md)) a delegátů ([delegátů](delegates.md)).</span><span class="sxs-lookup"><span data-stu-id="5b04b-141">Type declarations ([Type declarations](namespaces.md#type-declarations)) are used to define classes ([Classes](classes.md)), structs ([Structs](structs.md)), interfaces ([Interfaces](interfaces.md)), enums ([Enums](enums.md)), and delegates ([Delegates](delegates.md)).</span></span> <span data-ttu-id="5b04b-142">Typy členů povolených v deklaraci typu závisí na formuláři deklarace typu.</span><span class="sxs-lookup"><span data-stu-id="5b04b-142">The kinds of members permitted in a type declaration depend on the form of the type declaration.</span></span> <span data-ttu-id="5b04b-143">Například deklarace třídy mohou obsahovat deklarace pro konstanty ([konstanty](classes.md#constants)), pole ([pole](classes.md#fields)), metody ([metody](classes.md#methods)), vlastnosti ([vlastnosti](classes.md#properties)), události ([události](classes.md#events)), indexery ([indexery](classes.md#indexers)), operátory ([operátory](classes.md#operators)), konstruktory instancí ([konstruktory instancí](classes.md#instance-constructors)), statické konstruktory ([statické konstruktory](classes.md#static-constructors)), destruktory ([destruktory](classes.md#destructors)) a vnořené typy ([vnořené typy](classes.md#nested-types)).</span><span class="sxs-lookup"><span data-stu-id="5b04b-143">For instance, class declarations can contain declarations for constants ([Constants](classes.md#constants)), fields ([Fields](classes.md#fields)), methods ([Methods](classes.md#methods)), properties ([Properties](classes.md#properties)), events ([Events](classes.md#events)), indexers ([Indexers](classes.md#indexers)), operators ([Operators](classes.md#operators)), instance constructors ([Instance constructors](classes.md#instance-constructors)), static constructors ([Static constructors](classes.md#static-constructors)), destructors ([Destructors](classes.md#destructors)), and nested types ([Nested types](classes.md#nested-types)).</span></span>

<span data-ttu-id="5b04b-144">Deklarace definuje název v ***prostoru deklarace*** , ke kterému patří deklarace.</span><span class="sxs-lookup"><span data-stu-id="5b04b-144">A declaration defines a name in the ***declaration space*** to which the declaration belongs.</span></span> <span data-ttu-id="5b04b-145">S výjimkou přetížených členů ([signatury a přetížení](basic-concepts.md#signatures-and-overloading)) se jedná o chybu při kompilaci, která má dvě nebo více deklarací, které zavádějí členy se stejným názvem v prostoru deklarací.</span><span class="sxs-lookup"><span data-stu-id="5b04b-145">Except for overloaded members ([Signatures and overloading](basic-concepts.md#signatures-and-overloading)), it is a compile-time error to have two or more declarations that introduce members with the same name in a declaration space.</span></span> <span data-ttu-id="5b04b-146">V prostoru deklarací není nikdy možné obsahovat různé druhy členů se stejným názvem.</span><span class="sxs-lookup"><span data-stu-id="5b04b-146">It is never possible for a declaration space to contain different kinds of members with the same name.</span></span> <span data-ttu-id="5b04b-147">Například prostor deklarace nesmí nikdy obsahovat pole a metodu se stejným názvem.</span><span class="sxs-lookup"><span data-stu-id="5b04b-147">For example, a declaration space can never contain a field and a method by the same name.</span></span>

<span data-ttu-id="5b04b-148">Existuje několik různých typů prostorů deklarací, jak je popsáno v následujícím tématu.</span><span class="sxs-lookup"><span data-stu-id="5b04b-148">There are several different types of declaration spaces, as described in the following.</span></span>

*  <span data-ttu-id="5b04b-149">V rámci všech zdrojových souborů programu *namespace_member_declaration*s bez uzavíracích *namespace_declaration* členy jednoho kombinovaného prostoru deklarací, který se nazývá ***globální místo deklarace***.</span><span class="sxs-lookup"><span data-stu-id="5b04b-149">Within all source files of a program, *namespace_member_declaration*s with no enclosing *namespace_declaration* are members of a single combined declaration space called the ***global declaration space***.</span></span>
*  <span data-ttu-id="5b04b-150">V rámci všech zdrojových souborů programu *namespace_member_declaration*s v *namespace_declaration*s, které mají stejný plně kvalifikovaný obor názvů, jsou členy jednoho kombinovaného prostoru deklarací.</span><span class="sxs-lookup"><span data-stu-id="5b04b-150">Within all source files of a program, *namespace_member_declaration*s within *namespace_declaration*s that have the same fully qualified namespace name are members of a single combined declaration space.</span></span>
*  <span data-ttu-id="5b04b-151">Každá deklarace třídy, struktury nebo rozhraní vytvoří nové místo deklarace.</span><span class="sxs-lookup"><span data-stu-id="5b04b-151">Each class, struct, or interface declaration creates a new declaration space.</span></span> <span data-ttu-id="5b04b-152">Názvy se do tohoto prostoru deklarace přivádějí prostřednictvím *class_member_declaration*s, *struct_member_declaration*s, *interface_member_declaration*s nebo *type_parameter*s.</span><span class="sxs-lookup"><span data-stu-id="5b04b-152">Names are introduced into this declaration space through *class_member_declaration*s, *struct_member_declaration*s, *interface_member_declaration*s, or *type_parameter*s.</span></span> <span data-ttu-id="5b04b-153">Výjimkou jsou deklarace konstruktoru přetížené instance a deklarace statických konstruktorů, třídy nebo struktury nemůžou obsahovat deklaraci členu se stejným názvem jako třída nebo struktura.</span><span class="sxs-lookup"><span data-stu-id="5b04b-153">Except for overloaded instance constructor declarations and static constructor declarations, a class or struct cannot contain a member declaration with the same name as the class or struct.</span></span> <span data-ttu-id="5b04b-154">Třída, struktura nebo rozhraní povolují deklaraci přetížených metod a indexerů.</span><span class="sxs-lookup"><span data-stu-id="5b04b-154">A class, struct, or interface permits the declaration of overloaded methods and indexers.</span></span> <span data-ttu-id="5b04b-155">Třída nebo struktura navíc umožňuje deklaraci přetížených konstruktorů instancí a operátorů.</span><span class="sxs-lookup"><span data-stu-id="5b04b-155">Furthermore, a class or struct permits the declaration of overloaded instance constructors and operators.</span></span> <span data-ttu-id="5b04b-156">Například třída, struktura nebo rozhraní mohou obsahovat více deklarací metod se stejným názvem, za předpokladu, že se tyto deklarace metod liší v podpisu ([signatury a přetížení](basic-concepts.md#signatures-and-overloading)).</span><span class="sxs-lookup"><span data-stu-id="5b04b-156">For example, a class, struct, or interface may contain multiple method declarations with the same name, provided these method declarations differ in their signature ([Signatures and overloading](basic-concepts.md#signatures-and-overloading)).</span></span> <span data-ttu-id="5b04b-157">Všimněte si, že základní třídy nepřispívají k prostoru deklarací třídy a základní rozhraní nepřispívají k prostoru deklarací rozhraní.</span><span class="sxs-lookup"><span data-stu-id="5b04b-157">Note that base classes do not contribute to the declaration space of a class, and base interfaces do not contribute to the declaration space of an interface.</span></span> <span data-ttu-id="5b04b-158">Proto odvozená třída nebo rozhraní může deklarovat člen se stejným názvem jako zděděný člen.</span><span class="sxs-lookup"><span data-stu-id="5b04b-158">Thus, a derived class or interface is allowed to declare a member with the same name as an inherited member.</span></span> <span data-ttu-id="5b04b-159">Tento člen je označován za účelem ***skrytí*** zděděného člena.</span><span class="sxs-lookup"><span data-stu-id="5b04b-159">Such a member is said to ***hide*** the inherited member.</span></span>
*  <span data-ttu-id="5b04b-160">Každá deklarace delegáta vytvoří nové místo deklarace.</span><span class="sxs-lookup"><span data-stu-id="5b04b-160">Each delegate declaration creates a new declaration space.</span></span> <span data-ttu-id="5b04b-161">Názvy jsou představeny do tohoto prostoru deklarací prostřednictvím formálních parametrů (*fixed_parameter*s a *parameter_array*) a *type_parameter*s.</span><span class="sxs-lookup"><span data-stu-id="5b04b-161">Names are introduced into this declaration space through formal parameters (*fixed_parameter*s and *parameter_array*s) and *type_parameter*s.</span></span>
*  <span data-ttu-id="5b04b-162">Každá deklarace výčtu vytvoří nové místo deklarace.</span><span class="sxs-lookup"><span data-stu-id="5b04b-162">Each enumeration declaration creates a new declaration space.</span></span> <span data-ttu-id="5b04b-163">Názvy jsou představeny do tohoto prostoru deklarací prostřednictvím *enum_member_declarations*.</span><span class="sxs-lookup"><span data-stu-id="5b04b-163">Names are introduced into this declaration space through *enum_member_declarations*.</span></span>
*  <span data-ttu-id="5b04b-164">Každá deklarace metody, deklarace indexeru, deklarace operátoru, deklarace konstruktoru instance a anonymní funkce vytvoří nové místo deklarace s názvem ***místní proměnná proměnné Space***.</span><span class="sxs-lookup"><span data-stu-id="5b04b-164">Each method declaration, indexer declaration, operator declaration, instance constructor declaration and anonymous function creates a new declaration space called a ***local variable declaration space***.</span></span> <span data-ttu-id="5b04b-165">Názvy jsou představeny do tohoto prostoru deklarací prostřednictvím formálních parametrů (*fixed_parameter*s a *parameter_array*) a *type_parameter*s.</span><span class="sxs-lookup"><span data-stu-id="5b04b-165">Names are introduced into this declaration space through formal parameters (*fixed_parameter*s and *parameter_array*s) and *type_parameter*s.</span></span> <span data-ttu-id="5b04b-166">Tělo členu funkce nebo anonymní funkce, pokud existuje, je považováno za vnořené v rámci prostoru deklarace místní proměnné.</span><span class="sxs-lookup"><span data-stu-id="5b04b-166">The body of the function member or anonymous function, if any, is considered to be nested within the local variable declaration space.</span></span> <span data-ttu-id="5b04b-167">Jedná se o chybu pro místní prostor deklarace proměnné a vnořené místo deklarace místní proměnné, které obsahuje prvky se stejným názvem.</span><span class="sxs-lookup"><span data-stu-id="5b04b-167">It is an error for a local variable declaration space and a nested local variable declaration space to contain elements with the same name.</span></span> <span data-ttu-id="5b04b-168">Proto v rámci vnořeného prostoru deklarací není možné deklarovat místní proměnnou nebo konstantu se stejným názvem jako místní proměnná nebo konstanta v ohraničujícím prostoru deklarace.</span><span class="sxs-lookup"><span data-stu-id="5b04b-168">Thus, within a nested declaration space it is not possible to declare a local variable or constant with the same name as a local variable or constant in an enclosing declaration space.</span></span> <span data-ttu-id="5b04b-169">Je možné, že dva prostory deklarace obsahují prvky se stejným názvem, pokud ani prostor deklarace neobsahuje.</span><span class="sxs-lookup"><span data-stu-id="5b04b-169">It is possible for two declaration spaces to contain elements with the same name as long as neither declaration space contains the other.</span></span>
*  <span data-ttu-id="5b04b-170">Každý *blok* nebo *switch_block* , stejně jako příkaz *for*, *foreach* a *using* , vytvoří místní prostor deklarací proměnných pro lokální proměnné a místní konstanty.</span><span class="sxs-lookup"><span data-stu-id="5b04b-170">Each *block* or *switch_block* , as well as a *for*, *foreach* and *using* statement, creates a local variable declaration space for local variables and local constants .</span></span> <span data-ttu-id="5b04b-171">Názvy jsou představeny do tohoto prostoru deklarací prostřednictvím *local_variable_declaration*s a *local_constant_declaration*s.</span><span class="sxs-lookup"><span data-stu-id="5b04b-171">Names are introduced into this declaration space through *local_variable_declaration*s and *local_constant_declaration*s.</span></span> <span data-ttu-id="5b04b-172">Všimněte si, že bloky, ke kterým dochází jako nebo v těle členu funkce nebo anonymní funkce, jsou vnořené v rámci prostoru deklarací místní proměnné deklarované pomocí těchto funkcí pro své parametry.</span><span class="sxs-lookup"><span data-stu-id="5b04b-172">Note that blocks that occur as or within the body of a function member or anonymous function are nested within the local variable declaration space declared by those functions for their parameters.</span></span> <span data-ttu-id="5b04b-173">Proto se jedná o chybu, například metoda s místní proměnnou a parametr se stejným názvem.</span><span class="sxs-lookup"><span data-stu-id="5b04b-173">Thus it is an error to have e.g. a method with a local variable and a parameter of the same name.</span></span>
*  <span data-ttu-id="5b04b-174">Každý *blok* nebo *switch_block* vytvoří pro popisky samostatný prostor deklarace.</span><span class="sxs-lookup"><span data-stu-id="5b04b-174">Each *block* or *switch_block* creates a separate declaration space for labels.</span></span> <span data-ttu-id="5b04b-175">Názvy jsou představeny do tohoto prostoru deklarací prostřednictvím *labeled_statement*s a názvy jsou odkazovány prostřednictvím *goto_statement*s.</span><span class="sxs-lookup"><span data-stu-id="5b04b-175">Names are introduced into this declaration space through *labeled_statement*s, and the names are referenced through *goto_statement*s.</span></span> <span data-ttu-id="5b04b-176">***Prostor deklarace popisku*** bloku obsahuje všechny vnořené bloky.</span><span class="sxs-lookup"><span data-stu-id="5b04b-176">The ***label declaration space*** of a block includes any nested blocks.</span></span> <span data-ttu-id="5b04b-177">Proto v rámci vnořeného bloku není možné deklarovat popisek se stejným názvem jako popisek v ohraničujícím bloku.</span><span class="sxs-lookup"><span data-stu-id="5b04b-177">Thus, within a nested block it is not possible to declare a label with the same name as a label in an enclosing block.</span></span>

<span data-ttu-id="5b04b-178">Textové pořadí, ve kterém jsou názvy deklarovány, je obecně bez významu.</span><span class="sxs-lookup"><span data-stu-id="5b04b-178">The textual order in which names are declared is generally of no significance.</span></span> <span data-ttu-id="5b04b-179">Konkrétně textové pořadí není významné pro deklaraci a použití oborů názvů, konstant, metod, vlastností, událostí, indexerů, operátorů, konstruktorů instancí, destruktorů, statických konstruktorů a typů.</span><span class="sxs-lookup"><span data-stu-id="5b04b-179">In particular, textual order is not significant for the declaration and use of namespaces, constants, methods, properties, events, indexers, operators, instance constructors, destructors, static constructors, and types.</span></span> <span data-ttu-id="5b04b-180">Pořadí deklarací je významné v následujících ohledech:</span><span class="sxs-lookup"><span data-stu-id="5b04b-180">Declaration order is significant in the following ways:</span></span>

*  <span data-ttu-id="5b04b-181">Pořadí deklarací pro deklarace polí a deklarace místních proměnných Určuje pořadí, ve kterém jsou provedeny jejich Inicializátory (pokud existují).</span><span class="sxs-lookup"><span data-stu-id="5b04b-181">Declaration order for field declarations and local variable declarations determines the order in which their initializers (if any) are executed.</span></span>
*  <span data-ttu-id="5b04b-182">Lokální proměnné musí být definovány předtím, než se použijí ([rozsahy](basic-concepts.md#scopes)).</span><span class="sxs-lookup"><span data-stu-id="5b04b-182">Local variables must be defined before they are used ([Scopes](basic-concepts.md#scopes)).</span></span>
*  <span data-ttu-id="5b04b-183">Pořadí deklarací pro deklarace členů výčtu ([členy výčtu](enums.md#enum-members)) je důležité, pokud jsou vynechány *constant_expression* hodnoty.</span><span class="sxs-lookup"><span data-stu-id="5b04b-183">Declaration order for enum member declarations ([Enum members](enums.md#enum-members)) is significant when *constant_expression* values are omitted.</span></span>

<span data-ttu-id="5b04b-184">Prostor deklarací oboru názvů je "otevřený konec" a dvě deklarace oboru názvů se stejným plně kvalifikovaným názvem přispívají do stejného prostoru deklarace.</span><span class="sxs-lookup"><span data-stu-id="5b04b-184">The declaration space of a namespace is "open ended", and two namespace declarations with the same fully qualified name contribute to the same declaration space.</span></span> <span data-ttu-id="5b04b-185">Například</span><span class="sxs-lookup"><span data-stu-id="5b04b-185">For example</span></span>
```csharp
namespace Megacorp.Data
{
    class Customer
    {
        ...
    }
}

namespace Megacorp.Data
{
    class Order
    {
        ...
    }
}
```

<span data-ttu-id="5b04b-186">Dvě deklarace oboru názvů výše přispívají do stejného prostoru deklarace. v tomto případě deklaruje dvě třídy s plně kvalifikovanými názvy `Megacorp.Data.Customer` a `Megacorp.Data.Order`.</span><span class="sxs-lookup"><span data-stu-id="5b04b-186">The two namespace declarations above contribute to the same declaration space, in this case declaring two classes with the fully qualified names `Megacorp.Data.Customer` and `Megacorp.Data.Order`.</span></span> <span data-ttu-id="5b04b-187">Vzhledem k tomu, že dvě deklarace přispívají ke stejnému prostoru deklarace, by to vedlo k chybě při kompilaci, pokud každá z nich obsahuje deklaraci třídy se stejným názvem.</span><span class="sxs-lookup"><span data-stu-id="5b04b-187">Because the two declarations contribute to the same declaration space, it would have caused a compile-time error if each contained a declaration of a class with the same name.</span></span>

<span data-ttu-id="5b04b-188">Jak je uvedeno výše, místo deklarace bloku obsahuje všechny vnořené bloky.</span><span class="sxs-lookup"><span data-stu-id="5b04b-188">As specified above, the declaration space of a block includes any nested blocks.</span></span> <span data-ttu-id="5b04b-189">Proto v následujícím příkladu metody `F` a `G` způsobí chybu při kompilaci, protože název `i` je deklarován ve vnějším bloku a nemůže být znovu deklarován ve vnitřním bloku.</span><span class="sxs-lookup"><span data-stu-id="5b04b-189">Thus, in the following example, the `F` and `G` methods result in a compile-time error because the name `i` is declared in the outer block and cannot be redeclared in the inner block.</span></span> <span data-ttu-id="5b04b-190">Nicméně metody `H` a `I` jsou platné, protože dvě `i`jsou deklarovány v samostatných nevnořených blocích.</span><span class="sxs-lookup"><span data-stu-id="5b04b-190">However, the `H` and `I` methods are valid since the two `i`'s are declared in separate non-nested blocks.</span></span>

```csharp
class A
{
    void F() {
        int i = 0;
        if (true) {
            int i = 1;            
        }
    }

    void G() {
        if (true) {
            int i = 0;
        }
        int i = 1;                
    }

    void H() {
        if (true) {
            int i = 0;
        }
        if (true) {
            int i = 1;
        }
    }

    void I() {
        for (int i = 0; i < 10; i++)
            H();
        for (int i = 0; i < 10; i++)
            H();
    }
}
```

## <a name="members"></a><span data-ttu-id="5b04b-191">Členové</span><span class="sxs-lookup"><span data-stu-id="5b04b-191">Members</span></span>

<span data-ttu-id="5b04b-192">Obory názvů a typy mají ***členy***.</span><span class="sxs-lookup"><span data-stu-id="5b04b-192">Namespaces and types have ***members***.</span></span> <span data-ttu-id="5b04b-193">Členové entity jsou všeobecně k dispozici prostřednictvím kvalifikovaného názvu, který začíná odkazem na entitu a následuje tokenem "`.`" následovaným názvem člena.</span><span class="sxs-lookup"><span data-stu-id="5b04b-193">The members of an entity are generally available through the use of a qualified name that starts with a reference to the entity, followed by a "`.`" token, followed by the name of the member.</span></span>

<span data-ttu-id="5b04b-194">Členy typu jsou deklarovány buď v deklaraci typu, nebo ***zděděny*** ze základní třídy typu.</span><span class="sxs-lookup"><span data-stu-id="5b04b-194">Members of a type are either declared in the type declaration or ***inherited*** from the base class of the type.</span></span> <span data-ttu-id="5b04b-195">Když typ dědí ze základní třídy, všichni členové základní třídy, s výjimkou konstruktorů instancí, destruktorů a statických konstruktorů, se stanou členy odvozeného typu.</span><span class="sxs-lookup"><span data-stu-id="5b04b-195">When a type inherits from a base class, all members of the base class, except instance constructors, destructors and static constructors, become members of the derived type.</span></span> <span data-ttu-id="5b04b-196">Deklarovaná přístupnost člena základní třídy neurčuje, jestli je člen zděděný – dědičnost rozšiřuje na jakýkoliv člen, který není konstruktorem instance, statický konstruktor nebo destruktor.</span><span class="sxs-lookup"><span data-stu-id="5b04b-196">The declared accessibility of a base class member does not control whether the member is inherited—inheritance extends to any member that isn't an instance constructor, static constructor, or destructor.</span></span> <span data-ttu-id="5b04b-197">Zděděný člen však nemusí být přístupný v odvozeném typu, buď z důvodu deklarovaného přístupnosti ([deklarovaného přístupnosti](basic-concepts.md#declared-accessibility)), nebo vzhledem k tomu, že je skrytý deklarací v samotném typu ([skrytím prostřednictvím dědičnosti](basic-concepts.md#hiding-through-inheritance)).</span><span class="sxs-lookup"><span data-stu-id="5b04b-197">However, an inherited member may not be accessible in a derived type, either because of its declared accessibility ([Declared accessibility](basic-concepts.md#declared-accessibility)) or because it is hidden by a declaration in the type itself ([Hiding through inheritance](basic-concepts.md#hiding-through-inheritance)).</span></span>

### <a name="namespace-members"></a><span data-ttu-id="5b04b-198">Členové oboru názvů</span><span class="sxs-lookup"><span data-stu-id="5b04b-198">Namespace members</span></span>

<span data-ttu-id="5b04b-199">Obory názvů a typy, které nemají obor názvů nadřazených, jsou členy ***globálního oboru názvů***.</span><span class="sxs-lookup"><span data-stu-id="5b04b-199">Namespaces and types that have no enclosing namespace are members of the ***global namespace***.</span></span> <span data-ttu-id="5b04b-200">To odpovídá přímo názvům deklarovaným v globálním prostoru deklarací.</span><span class="sxs-lookup"><span data-stu-id="5b04b-200">This corresponds directly to the names declared in the global declaration space.</span></span>

<span data-ttu-id="5b04b-201">Obory názvů a typy deklarované v rámci oboru názvů jsou členy tohoto oboru názvů.</span><span class="sxs-lookup"><span data-stu-id="5b04b-201">Namespaces and types declared within a namespace are members of that namespace.</span></span> <span data-ttu-id="5b04b-202">To odpovídá přímo názvům deklarovaným v prostoru deklarací oboru názvů.</span><span class="sxs-lookup"><span data-stu-id="5b04b-202">This corresponds directly to the names declared in the declaration space of the namespace.</span></span>

<span data-ttu-id="5b04b-203">Obory názvů nemají žádná omezení přístupu.</span><span class="sxs-lookup"><span data-stu-id="5b04b-203">Namespaces have no access restrictions.</span></span> <span data-ttu-id="5b04b-204">Není možné deklarovat privátní, chráněné nebo interní obory názvů a názvy oborů názvů jsou vždy veřejně přístupné.</span><span class="sxs-lookup"><span data-stu-id="5b04b-204">It is not possible to declare private, protected, or internal namespaces, and namespace names are always publicly accessible.</span></span>

### <a name="struct-members"></a><span data-ttu-id="5b04b-205">Členové struktury</span><span class="sxs-lookup"><span data-stu-id="5b04b-205">Struct members</span></span>

<span data-ttu-id="5b04b-206">Členy struktury jsou členy deklarované ve struktuře a členy zděděné z přímé základní třídy struktury `System.ValueType` a nepřímou základní třídou `object`.</span><span class="sxs-lookup"><span data-stu-id="5b04b-206">The members of a struct are the members declared in the struct and the members inherited from the struct's direct base class `System.ValueType` and the indirect base class `object`.</span></span>

<span data-ttu-id="5b04b-207">Členové jednoduchého typu odpovídají přímo členům typu struktury s aliasem jednoduchý typ:</span><span class="sxs-lookup"><span data-stu-id="5b04b-207">The members of a simple type correspond directly to the members of the struct type aliased by the simple type:</span></span>

*  <span data-ttu-id="5b04b-208">Členové `sbyte` jsou členy struktury `System.SByte`.</span><span class="sxs-lookup"><span data-stu-id="5b04b-208">The members of `sbyte` are the members of the `System.SByte` struct.</span></span>
*  <span data-ttu-id="5b04b-209">Členové `byte` jsou členy struktury `System.Byte`.</span><span class="sxs-lookup"><span data-stu-id="5b04b-209">The members of `byte` are the members of the `System.Byte` struct.</span></span>
*  <span data-ttu-id="5b04b-210">Členové `short` jsou členy struktury `System.Int16`.</span><span class="sxs-lookup"><span data-stu-id="5b04b-210">The members of `short` are the members of the `System.Int16` struct.</span></span>
*  <span data-ttu-id="5b04b-211">Členové `ushort` jsou členy struktury `System.UInt16`.</span><span class="sxs-lookup"><span data-stu-id="5b04b-211">The members of `ushort` are the members of the `System.UInt16` struct.</span></span>
*  <span data-ttu-id="5b04b-212">Členové `int` jsou členy struktury `System.Int32`.</span><span class="sxs-lookup"><span data-stu-id="5b04b-212">The members of `int` are the members of the `System.Int32` struct.</span></span>
*  <span data-ttu-id="5b04b-213">Členové `uint` jsou členy struktury `System.UInt32`.</span><span class="sxs-lookup"><span data-stu-id="5b04b-213">The members of `uint` are the members of the `System.UInt32` struct.</span></span>
*  <span data-ttu-id="5b04b-214">Členové `long` jsou členy struktury `System.Int64`.</span><span class="sxs-lookup"><span data-stu-id="5b04b-214">The members of `long` are the members of the `System.Int64` struct.</span></span>
*  <span data-ttu-id="5b04b-215">Členové `ulong` jsou členy struktury `System.UInt64`.</span><span class="sxs-lookup"><span data-stu-id="5b04b-215">The members of `ulong` are the members of the `System.UInt64` struct.</span></span>
*  <span data-ttu-id="5b04b-216">Členové `char` jsou členy struktury `System.Char`.</span><span class="sxs-lookup"><span data-stu-id="5b04b-216">The members of `char` are the members of the `System.Char` struct.</span></span>
*  <span data-ttu-id="5b04b-217">Členové `float` jsou členy struktury `System.Single`.</span><span class="sxs-lookup"><span data-stu-id="5b04b-217">The members of `float` are the members of the `System.Single` struct.</span></span>
*  <span data-ttu-id="5b04b-218">Členové `double` jsou členy struktury `System.Double`.</span><span class="sxs-lookup"><span data-stu-id="5b04b-218">The members of `double` are the members of the `System.Double` struct.</span></span>
*  <span data-ttu-id="5b04b-219">Členové `decimal` jsou členy struktury `System.Decimal`.</span><span class="sxs-lookup"><span data-stu-id="5b04b-219">The members of `decimal` are the members of the `System.Decimal` struct.</span></span>
*  <span data-ttu-id="5b04b-220">Členové `bool` jsou členy struktury `System.Boolean`.</span><span class="sxs-lookup"><span data-stu-id="5b04b-220">The members of `bool` are the members of the `System.Boolean` struct.</span></span>

### <a name="enumeration-members"></a><span data-ttu-id="5b04b-221">Členové výčtu</span><span class="sxs-lookup"><span data-stu-id="5b04b-221">Enumeration members</span></span>

<span data-ttu-id="5b04b-222">Členy výčtu jsou konstanty deklarované ve výčtu a členy zděděné z přímé základní třídy výčtu `System.Enum` a nepřímé základní třídy `System.ValueType` a `object`.</span><span class="sxs-lookup"><span data-stu-id="5b04b-222">The members of an enumeration are the constants declared in the enumeration and the members inherited from the enumeration's direct base class `System.Enum` and the indirect base classes `System.ValueType` and `object`.</span></span>

### <a name="class-members"></a><span data-ttu-id="5b04b-223">Členové třídy</span><span class="sxs-lookup"><span data-stu-id="5b04b-223">Class members</span></span>

<span data-ttu-id="5b04b-224">Členy třídy jsou členy deklarované ve třídě a členy zděděné ze základní třídy (kromě třídy `object`, která nemá žádnou základní třídu).</span><span class="sxs-lookup"><span data-stu-id="5b04b-224">The members of a class are the members declared in the class and the members inherited from the base class (except for class `object` which has no base class).</span></span> <span data-ttu-id="5b04b-225">Členy zděděné ze základní třídy zahrnují konstanty, pole, metody, vlastnosti, události, indexery, operátory a typy základní třídy, ale ne konstruktory instancí, destruktory a statické konstruktory základní třídy.</span><span class="sxs-lookup"><span data-stu-id="5b04b-225">The members inherited from the base class include the constants, fields, methods, properties, events, indexers, operators, and types of the base class, but not the instance constructors, destructors and static constructors of the base class.</span></span> <span data-ttu-id="5b04b-226">Členové základní třídy jsou děděni bez ohledu na jejich přístupnost.</span><span class="sxs-lookup"><span data-stu-id="5b04b-226">Base class members are inherited without regard to their accessibility.</span></span>

<span data-ttu-id="5b04b-227">Deklarace třídy může obsahovat deklarace konstant, pole, metody, vlastnosti, události, indexery, operátory, konstruktory instancí, destruktorů, statických konstruktorů a typů.</span><span class="sxs-lookup"><span data-stu-id="5b04b-227">A class declaration may contain declarations of constants, fields, methods, properties, events, indexers, operators, instance constructors, destructors, static constructors and types.</span></span>

<span data-ttu-id="5b04b-228">Členy `object` a `string` odpovídají přímo členům typů tříd, které aliasují:</span><span class="sxs-lookup"><span data-stu-id="5b04b-228">The members of `object` and `string` correspond directly to the members of the class types they alias:</span></span>

*  <span data-ttu-id="5b04b-229">Členy `object` jsou členy třídy `System.Object`.</span><span class="sxs-lookup"><span data-stu-id="5b04b-229">The members of `object` are the members of the `System.Object` class.</span></span>
*  <span data-ttu-id="5b04b-230">Členy `string` jsou členy třídy `System.String`.</span><span class="sxs-lookup"><span data-stu-id="5b04b-230">The members of `string` are the members of the `System.String` class.</span></span>

### <a name="interface-members"></a><span data-ttu-id="5b04b-231">Členy rozhraní</span><span class="sxs-lookup"><span data-stu-id="5b04b-231">Interface members</span></span>

<span data-ttu-id="5b04b-232">Členy rozhraní jsou členy deklarované v rozhraní a ve všech základních rozhraních rozhraní.</span><span class="sxs-lookup"><span data-stu-id="5b04b-232">The members of an interface are the members declared in the interface and in all base interfaces of the interface.</span></span> <span data-ttu-id="5b04b-233">Členy ve třídě `object` nejsou, výhradně řečeno, členové libovolného rozhraní ([Členové rozhraní](interfaces.md#interface-members)).</span><span class="sxs-lookup"><span data-stu-id="5b04b-233">The members in class `object` are not, strictly speaking, members of any interface ([Interface members](interfaces.md#interface-members)).</span></span> <span data-ttu-id="5b04b-234">Nicméně členové ve třídě `object` jsou k dispozici prostřednictvím vyhledávání členů v jakémkoli typu rozhraní ([vyhledávání členů](expressions.md#member-lookup)).</span><span class="sxs-lookup"><span data-stu-id="5b04b-234">However, the members in class `object` are available via member lookup in any interface type ([Member lookup](expressions.md#member-lookup)).</span></span>

### <a name="array-members"></a><span data-ttu-id="5b04b-235">Členové pole</span><span class="sxs-lookup"><span data-stu-id="5b04b-235">Array members</span></span>

<span data-ttu-id="5b04b-236">Členy pole jsou členy děděny ze třídy `System.Array`.</span><span class="sxs-lookup"><span data-stu-id="5b04b-236">The members of an array are the members inherited from class `System.Array`.</span></span>

### <a name="delegate-members"></a><span data-ttu-id="5b04b-237">Členové delegáta</span><span class="sxs-lookup"><span data-stu-id="5b04b-237">Delegate members</span></span>

<span data-ttu-id="5b04b-238">Členové delegáta jsou členy dědění ze třídy `System.Delegate`.</span><span class="sxs-lookup"><span data-stu-id="5b04b-238">The members of a delegate are the members inherited from class `System.Delegate`.</span></span>

## <a name="member-access"></a><span data-ttu-id="5b04b-239">Přístup ke členům</span><span class="sxs-lookup"><span data-stu-id="5b04b-239">Member access</span></span>

<span data-ttu-id="5b04b-240">Deklarace členů umožňují kontrolu nad přístupem členů.</span><span class="sxs-lookup"><span data-stu-id="5b04b-240">Declarations of members allow control over member access.</span></span> <span data-ttu-id="5b04b-241">Přístupnost člena je založena na deklaraci přístupnosti ([deklarovaný přístup](basic-concepts.md#declared-accessibility)) člena v kombinaci s přístupností bezprostředně obsahujícího typu, pokud existuje.</span><span class="sxs-lookup"><span data-stu-id="5b04b-241">The accessibility of a member is established by the declared accessibility ([Declared accessibility](basic-concepts.md#declared-accessibility)) of the member combined with the accessibility of the immediately containing type, if any.</span></span>

<span data-ttu-id="5b04b-242">Když je povolený přístup ke konkrétnímu členu, bude se jednat o člena, který je ***přístupný***.</span><span class="sxs-lookup"><span data-stu-id="5b04b-242">When access to a particular member is allowed, the member is said to be ***accessible***.</span></span> <span data-ttu-id="5b04b-243">Naopak, pokud je zakázaný přístup ke konkrétnímu členovi, říká se, že je ***nepřístupný***.</span><span class="sxs-lookup"><span data-stu-id="5b04b-243">Conversely, when access to a particular member is disallowed, the member is said to be ***inaccessible***.</span></span> <span data-ttu-id="5b04b-244">Přístup ke členu je povolený, když je textové umístění, ve kterém se přístup provádí, zahrnuté v doméně přístupnosti ([domény přístupnosti](basic-concepts.md#accessibility-domains)) člena.</span><span class="sxs-lookup"><span data-stu-id="5b04b-244">Access to a member is permitted when the textual location in which the access takes place is included in the accessibility domain ([Accessibility domains](basic-concepts.md#accessibility-domains)) of the member.</span></span>

### <a name="declared-accessibility"></a><span data-ttu-id="5b04b-245">Deklarovaná přístupnost</span><span class="sxs-lookup"><span data-stu-id="5b04b-245">Declared accessibility</span></span>

<span data-ttu-id="5b04b-246">***Deklarovaná přístupnost*** člena může být jedna z následujících:</span><span class="sxs-lookup"><span data-stu-id="5b04b-246">The ***declared accessibility*** of a member can be one of the following:</span></span>

*  <span data-ttu-id="5b04b-247">Public, který je vybrán zahrnutím modifikátoru `public` v deklaraci členu.</span><span class="sxs-lookup"><span data-stu-id="5b04b-247">Public, which is selected by including a `public` modifier in the member declaration.</span></span> <span data-ttu-id="5b04b-248">Intuitivní význam `public` je "Access not Unlimited".</span><span class="sxs-lookup"><span data-stu-id="5b04b-248">The intuitive meaning of `public` is "access not limited".</span></span>
*  <span data-ttu-id="5b04b-249">Protected, který je vybrán zahrnutím modifikátoru `protected` v deklaraci členu.</span><span class="sxs-lookup"><span data-stu-id="5b04b-249">Protected, which is selected by including a `protected` modifier in the member declaration.</span></span> <span data-ttu-id="5b04b-250">Intuitivní význam `protected` je "přístup omezený na obsahující třídu nebo typy odvozené z nadřazené třídy".</span><span class="sxs-lookup"><span data-stu-id="5b04b-250">The intuitive meaning of `protected` is "access limited to the containing class or types derived from the containing class".</span></span>
*  <span data-ttu-id="5b04b-251">Interní, který je vybrán zahrnutím modifikátoru `internal` v deklaraci členu.</span><span class="sxs-lookup"><span data-stu-id="5b04b-251">Internal, which is selected by including an `internal` modifier in the member declaration.</span></span> <span data-ttu-id="5b04b-252">Intuitivní význam `internal` je "přístup omezený na tento program".</span><span class="sxs-lookup"><span data-stu-id="5b04b-252">The intuitive meaning of `internal` is "access limited to this program".</span></span>
*  <span data-ttu-id="5b04b-253">Chráněn interní (tj. chráněný nebo interní), který je vybrán zahrnutím `protected` a modifikátorem `internal` v deklaraci členu.</span><span class="sxs-lookup"><span data-stu-id="5b04b-253">Protected internal (meaning protected or internal), which is selected by including both a `protected` and an `internal` modifier in the member declaration.</span></span> <span data-ttu-id="5b04b-254">Intuitivní význam `protected internal` je "přístup omezený na tento program nebo typy odvozené z nadřazené třídy".</span><span class="sxs-lookup"><span data-stu-id="5b04b-254">The intuitive meaning of `protected internal` is "access limited to this program or types derived from the containing class".</span></span>
*  <span data-ttu-id="5b04b-255">Private, který je vybrán zahrnutím modifikátoru `private` v deklaraci členu.</span><span class="sxs-lookup"><span data-stu-id="5b04b-255">Private, which is selected by including a `private` modifier in the member declaration.</span></span> <span data-ttu-id="5b04b-256">Intuitivní význam `private` je "přístup omezený na obsažený typ".</span><span class="sxs-lookup"><span data-stu-id="5b04b-256">The intuitive meaning of `private` is "access limited to the containing type".</span></span>

<span data-ttu-id="5b04b-257">V závislosti na kontextu, ve kterém je deklarace členů provedena, jsou povoleny pouze určité typy deklarovaného usnadnění.</span><span class="sxs-lookup"><span data-stu-id="5b04b-257">Depending on the context in which a member declaration takes place, only certain types of declared accessibility are permitted.</span></span> <span data-ttu-id="5b04b-258">Kromě toho, když deklarace člena neobsahuje žádné modifikátory přístupu, určuje kontext, ve kterém se deklarace provede, výchozí deklarovaná přístupnost.</span><span class="sxs-lookup"><span data-stu-id="5b04b-258">Furthermore, when a member declaration does not include any access modifiers, the context in which the declaration takes place determines the default declared accessibility.</span></span>

*  <span data-ttu-id="5b04b-259">Obory názvů mají implicitně `public` deklarovaný přístup.</span><span class="sxs-lookup"><span data-stu-id="5b04b-259">Namespaces implicitly have `public` declared accessibility.</span></span> <span data-ttu-id="5b04b-260">U deklarací oboru názvů nejsou povoleny žádné modifikátory přístupu.</span><span class="sxs-lookup"><span data-stu-id="5b04b-260">No access modifiers are allowed on namespace declarations.</span></span>
*  <span data-ttu-id="5b04b-261">Typy deklarované v jednotkách kompilace nebo oborech názvů můžou mít `public` nebo `internal` deklarované přístupnosti a výchozí hodnoty pro `internal` deklarovaného přístupu.</span><span class="sxs-lookup"><span data-stu-id="5b04b-261">Types declared in compilation units or namespaces can have `public` or `internal` declared accessibility and default to `internal` declared accessibility.</span></span>
*  <span data-ttu-id="5b04b-262">Členové třídy mohou mít kterýkoli z pěti druhů deklarovaného přístupnosti a výchozích možností, aby `private` deklarovaného přístupnosti.</span><span class="sxs-lookup"><span data-stu-id="5b04b-262">Class members can have any of the five kinds of declared accessibility and default to `private` declared accessibility.</span></span> <span data-ttu-id="5b04b-263">(Všimněte si, že typ deklarovaný jako člen třídy může mít kterýkoli z pěti druhů deklarovaného přístupnosti, zatímco typ deklarovaný jako člen oboru názvů může mít pouze `public` nebo `internal` deklarovaného přístupnosti.)</span><span class="sxs-lookup"><span data-stu-id="5b04b-263">(Note that a type declared as a member of a class can have any of the five kinds of declared accessibility, whereas a type declared as a member of a namespace can have only `public` or `internal` declared accessibility.)</span></span>
*  <span data-ttu-id="5b04b-264">Členové struktury můžou mít `public`, `internal`nebo `private` deklarovaného přístupnosti a výchozí hodnoty pro `private` deklarovaná přístupnost, protože struktury jsou implicitně zapečetěné.</span><span class="sxs-lookup"><span data-stu-id="5b04b-264">Struct members can have `public`, `internal`, or `private` declared accessibility and default to `private` declared accessibility because structs are implicitly sealed.</span></span> <span data-ttu-id="5b04b-265">Členy struktury představené ve struktuře (to znamená, že není zděděná touto strukturou) nemohou mít deklaraci Accessibility `protected` ani `protected internal`.</span><span class="sxs-lookup"><span data-stu-id="5b04b-265">Struct members introduced in a struct (that is, not inherited by that struct) cannot have `protected` or `protected internal` declared accessibility.</span></span> <span data-ttu-id="5b04b-266">(Všimněte si, že typ deklarovaný jako člen struktury může mít `public`, `internal`nebo `private` deklarovaného usnadnění, zatímco typ deklarovaný jako člen oboru názvů může mít pouze `public` nebo `internal` deklarovaný přístup.)</span><span class="sxs-lookup"><span data-stu-id="5b04b-266">(Note that a type declared as a member of a struct can have `public`, `internal`, or `private` declared accessibility, whereas a type declared as a member of a namespace can have only `public` or `internal` declared accessibility.)</span></span>
*  <span data-ttu-id="5b04b-267">Členy rozhraní mají implicitně `public` deklarovaný přístup.</span><span class="sxs-lookup"><span data-stu-id="5b04b-267">Interface members implicitly have `public` declared accessibility.</span></span> <span data-ttu-id="5b04b-268">V deklaracích členů rozhraní nejsou povoleny žádné modifikátory přístupu.</span><span class="sxs-lookup"><span data-stu-id="5b04b-268">No access modifiers are allowed on interface member declarations.</span></span>
*  <span data-ttu-id="5b04b-269">Členové výčtu mají implicitně `public` deklarovaný přístup.</span><span class="sxs-lookup"><span data-stu-id="5b04b-269">Enumeration members implicitly have `public` declared accessibility.</span></span> <span data-ttu-id="5b04b-270">Pro deklarace členů výčtu nejsou povoleny žádné modifikátory přístupu.</span><span class="sxs-lookup"><span data-stu-id="5b04b-270">No access modifiers are allowed on enumeration member declarations.</span></span>

### <a name="accessibility-domains"></a><span data-ttu-id="5b04b-271">Domény přístupnosti</span><span class="sxs-lookup"><span data-stu-id="5b04b-271">Accessibility domains</span></span>

<span data-ttu-id="5b04b-272">***Doména přístupnosti*** člena se skládá z (případně nesouvislých) oddílů textu programu, ve kterém je povolený přístup ke členu.</span><span class="sxs-lookup"><span data-stu-id="5b04b-272">The ***accessibility domain*** of a member consists of the (possibly disjoint) sections of program text in which access to the member is permitted.</span></span> <span data-ttu-id="5b04b-273">Pro účely definování domény přístupnosti člena je člen označován jako ***nejvyšší úroveň*** , pokud není deklarován v rámci typu, a člen je označován jako ***vnořený*** v případě, že je deklarován v rámci jiného typu.</span><span class="sxs-lookup"><span data-stu-id="5b04b-273">For purposes of defining the accessibility domain of a member, a member is said to be ***top-level*** if it is not declared within a type, and a member is said to be ***nested*** if it is declared within another type.</span></span> <span data-ttu-id="5b04b-274">Kromě toho ***text*** programu programu je definován jako veškerý text programu obsažený ve všech zdrojových souborech programu a text programu typu je definován jako veškerý text programu obsažený v *type_declaration*s daného typu (včetně, případně typů, které jsou vnořeny do typu).</span><span class="sxs-lookup"><span data-stu-id="5b04b-274">Furthermore, the ***program text*** of a program is defined as all program text contained in all source files of the program, and the program text of a type is defined as all program text contained in the *type_declaration*s of that type (including, possibly, types that are nested within the type).</span></span>

<span data-ttu-id="5b04b-275">Doména přístupnosti předdefinovaného typu (například `object`, `int`nebo `double`) je neomezená.</span><span class="sxs-lookup"><span data-stu-id="5b04b-275">The accessibility domain of a predefined type (such as `object`, `int`, or `double`) is unlimited.</span></span>

<span data-ttu-id="5b04b-276">Doména přístupnosti `T` nevázaného typu na nejvyšší úrovni ([vázané a nevázané typy](types.md#bound-and-unbound-types)), která je deklarována v programu `P` je definována takto:</span><span class="sxs-lookup"><span data-stu-id="5b04b-276">The accessibility domain of a top-level unbound type `T` ([Bound and unbound types](types.md#bound-and-unbound-types)) that is declared in a program `P` is defined as follows:</span></span>

*  <span data-ttu-id="5b04b-277">Pokud je deklarovaná přístupnost `T` `public`, doména přístupnosti `T` je text programu `P` a jakýkoliv program, který odkazuje na `P`.</span><span class="sxs-lookup"><span data-stu-id="5b04b-277">If the declared accessibility of `T` is `public`, the accessibility domain of `T` is the program text of `P` and any program that references `P`.</span></span>
*  <span data-ttu-id="5b04b-278">Pokud je deklarovaná přístupnost `T` `internal`, doména přístupnosti `T` je text programu `P`.</span><span class="sxs-lookup"><span data-stu-id="5b04b-278">If the declared accessibility of `T` is `internal`, the accessibility domain of `T` is the program text of `P`.</span></span>

<span data-ttu-id="5b04b-279">Z těchto definic se řídí tím, že doména přístupnosti typu bez vazby na nejvyšší úrovni je vždy alespoň text programu programu, ve kterém je tento typ deklarován.</span><span class="sxs-lookup"><span data-stu-id="5b04b-279">From these definitions it follows that the accessibility domain of a top-level unbound type is always at least the program text of the program in which that type is declared.</span></span>

<span data-ttu-id="5b04b-280">Doména přístupnosti pro konstruovaný typ `T<A1, ..., An>` je průsečík domény přístupnosti nevázaného obecného typu `T` a domén přístupnosti argumentů typu `A1, ..., An`.</span><span class="sxs-lookup"><span data-stu-id="5b04b-280">The accessibility domain for a constructed type `T<A1, ..., An>` is the intersection of the accessibility domain of the unbound generic type `T` and the accessibility domains of the type arguments `A1, ..., An`.</span></span>

<span data-ttu-id="5b04b-281">Doména přístupnosti vnořeného člena `M` deklarovaného v `T` typu v rámci programu `P` je definována takto (s označením, že by mohl být typ pravděpodobně sám `M`):</span><span class="sxs-lookup"><span data-stu-id="5b04b-281">The accessibility domain of a nested member `M` declared in a type `T` within a program `P` is defined as follows (noting that `M` itself may possibly be a type):</span></span>

*  <span data-ttu-id="5b04b-282">Pokud je deklarovaná přístupnost `M` `public`, doména přístupnosti `M` je doména přístupnosti `T`.</span><span class="sxs-lookup"><span data-stu-id="5b04b-282">If the declared accessibility of `M` is `public`, the accessibility domain of `M` is the accessibility domain of `T`.</span></span>
*  <span data-ttu-id="5b04b-283">Pokud je deklarovaná přístupnost `M` `protected internal`, nechte `D` být sjednocením textu programu `P` a text programu libovolného typu odvozeného z `T`, který je deklarovaný mimo `P`.</span><span class="sxs-lookup"><span data-stu-id="5b04b-283">If the declared accessibility of `M` is `protected internal`, let `D` be the union of the program text of `P` and the program text of any type derived from `T`, which is declared outside `P`.</span></span> <span data-ttu-id="5b04b-284">Doména přístupnosti `M` je průnik domény přístupnosti `T` s `D`.</span><span class="sxs-lookup"><span data-stu-id="5b04b-284">The accessibility domain of `M` is the intersection of the accessibility domain of `T` with `D`.</span></span>
*  <span data-ttu-id="5b04b-285">Pokud je deklarovaná přístupnost `M` `protected`, nechte `D` být sjednocením textu programu `T` a textem programu libovolného typu odvozeného z `T`.</span><span class="sxs-lookup"><span data-stu-id="5b04b-285">If the declared accessibility of `M` is `protected`, let `D` be the union of the program text of `T` and the program text of any type derived from `T`.</span></span> <span data-ttu-id="5b04b-286">Doména přístupnosti `M` je průnik domény přístupnosti `T` s `D`.</span><span class="sxs-lookup"><span data-stu-id="5b04b-286">The accessibility domain of `M` is the intersection of the accessibility domain of `T` with `D`.</span></span>
*  <span data-ttu-id="5b04b-287">Pokud je deklarovaná přístupnost `M` `internal`, doména přístupnosti `M` je průnik domény přístupnosti `T` s textem programu `P`.</span><span class="sxs-lookup"><span data-stu-id="5b04b-287">If the declared accessibility of `M` is `internal`, the accessibility domain of `M` is the intersection of the accessibility domain of `T` with the program text of `P`.</span></span>
*  <span data-ttu-id="5b04b-288">Pokud je deklarovaná přístupnost `M` `private`, doména přístupnosti `M` je text programu `T`.</span><span class="sxs-lookup"><span data-stu-id="5b04b-288">If the declared accessibility of `M` is `private`, the accessibility domain of `M` is the program text of `T`.</span></span>

<span data-ttu-id="5b04b-289">Z těchto definic následuje, že doména přístupnosti vnořeného člena je vždy alespoň text programu typu, ve kterém je člen deklarován.</span><span class="sxs-lookup"><span data-stu-id="5b04b-289">From these definitions it follows that the accessibility domain of a nested member is always at least the program text of the type in which the member is declared.</span></span> <span data-ttu-id="5b04b-290">Navíc následuje za tím, že doména přístupnosti člena není nikdy více zahrnutá než doména přístupnosti typu, ve kterém je člen deklarovaný.</span><span class="sxs-lookup"><span data-stu-id="5b04b-290">Furthermore, it follows that the accessibility domain of a member is never more inclusive than the accessibility domain of the type in which the member is declared.</span></span>

<span data-ttu-id="5b04b-291">V intuitivních případech platí, že při přístupu k typu nebo členu `M` je vyhodnocen následující postup, který zajistí, že přístup je povolen:</span><span class="sxs-lookup"><span data-stu-id="5b04b-291">In intuitive terms, when a type or member `M` is accessed, the following steps are evaluated to ensure that the access is permitted:</span></span>

*  <span data-ttu-id="5b04b-292">První, pokud je `M` deklarována v rámci typu (na rozdíl od kompilační jednotky nebo oboru názvů), dojde k chybě při kompilaci, pokud tento typ není přístupný.</span><span class="sxs-lookup"><span data-stu-id="5b04b-292">First, if `M` is declared within a type (as opposed to a compilation unit or a namespace), a compile-time error occurs if that type is not accessible.</span></span>
*  <span data-ttu-id="5b04b-293">Pokud je pak `M` `public`, povolený přístup.</span><span class="sxs-lookup"><span data-stu-id="5b04b-293">Then, if `M` is `public`, the access is permitted.</span></span>
*  <span data-ttu-id="5b04b-294">V opačném případě, pokud je `M` `protected internal`, je přístup povolen, pokud k tomu dojde v rámci programu, ve kterém je deklarována `M`, nebo pokud k ní dojde v rámci třídy odvozené od třídy, ve které je `M` deklarována a provedena prostřednictvím odvozené třídy ([chráněný přístup pro členy instance](basic-concepts.md#protected-access-for-instance-members)).</span><span class="sxs-lookup"><span data-stu-id="5b04b-294">Otherwise, if `M` is `protected internal`, the access is permitted if it occurs within the program in which `M` is declared, or if it occurs within a class derived from the class in which `M` is declared and takes place through the derived class type ([Protected access for instance members](basic-concepts.md#protected-access-for-instance-members)).</span></span>
*  <span data-ttu-id="5b04b-295">V opačném případě, pokud je `M` `protected`, je přístup povolen, pokud se vyskytuje v rámci třídy, ve které je deklarován `M`, nebo pokud k ní dojde v rámci třídy odvozené od třídy, ve které `M` deklarována a provedena prostřednictvím odvozené třídy ([chráněný přístup pro členy instance](basic-concepts.md#protected-access-for-instance-members)).</span><span class="sxs-lookup"><span data-stu-id="5b04b-295">Otherwise, if `M` is `protected`, the access is permitted if it occurs within the class in which `M` is declared, or if it occurs within a class derived from the class in which `M` is declared and takes place through the derived class type ([Protected access for instance members](basic-concepts.md#protected-access-for-instance-members)).</span></span>
*  <span data-ttu-id="5b04b-296">V opačném případě, pokud je `M` `internal`, je přístup povolen, pokud k němu dojde v programu, ve kterém je deklarován `M`.</span><span class="sxs-lookup"><span data-stu-id="5b04b-296">Otherwise, if `M` is `internal`, the access is permitted if it occurs within the program in which `M` is declared.</span></span>
*  <span data-ttu-id="5b04b-297">V opačném případě, pokud je `M` `private`, je přístup povolen, pokud k němu dojde v rámci typu, ve kterém je deklarováno `M`.</span><span class="sxs-lookup"><span data-stu-id="5b04b-297">Otherwise, if `M` is `private`, the access is permitted if it occurs within the type in which `M` is declared.</span></span>
*  <span data-ttu-id="5b04b-298">V opačném případě je typ nebo člen nepřístupný a dojde k chybě při kompilaci.</span><span class="sxs-lookup"><span data-stu-id="5b04b-298">Otherwise, the type or member is inaccessible, and a compile-time error occurs.</span></span>

<span data-ttu-id="5b04b-299">V příkladu</span><span class="sxs-lookup"><span data-stu-id="5b04b-299">In the example</span></span>
```csharp
public class A
{
    public static int X;
    internal static int Y;
    private static int Z;
}

internal class B
{
    public static int X;
    internal static int Y;
    private static int Z;

    public class C
    {
        public static int X;
        internal static int Y;
        private static int Z;
    }

    private class D
    {
        public static int X;
        internal static int Y;
        private static int Z;
    }
}
```
<span data-ttu-id="5b04b-300">třídy a členové mají následující domény přístupnosti:</span><span class="sxs-lookup"><span data-stu-id="5b04b-300">the classes and members have the following accessibility domains:</span></span>

*  <span data-ttu-id="5b04b-301">Doména přístupnosti `A` a `A.X` je neomezená.</span><span class="sxs-lookup"><span data-stu-id="5b04b-301">The accessibility domain of `A` and `A.X` is unlimited.</span></span>
*  <span data-ttu-id="5b04b-302">Doména přístupnosti `A.Y`, `B`, `B.X`, `B.Y`, `B.C`, `B.C.X`a `B.C.Y` je text programu obsahujícího programu.</span><span class="sxs-lookup"><span data-stu-id="5b04b-302">The accessibility domain of `A.Y`, `B`, `B.X`, `B.Y`, `B.C`, `B.C.X`, and `B.C.Y` is the program text of the containing program.</span></span>
*  <span data-ttu-id="5b04b-303">Doména přístupnosti `A.Z` je text programu `A`.</span><span class="sxs-lookup"><span data-stu-id="5b04b-303">The accessibility domain of `A.Z` is the program text of `A`.</span></span>
*  <span data-ttu-id="5b04b-304">Doména přístupnosti `B.Z` a `B.D` je text programu `B`, včetně textu programu `B.C` a `B.D`.</span><span class="sxs-lookup"><span data-stu-id="5b04b-304">The accessibility domain of `B.Z` and `B.D` is the program text of `B`, including the program text of `B.C` and `B.D`.</span></span>
*  <span data-ttu-id="5b04b-305">Doména přístupnosti `B.C.Z` je text programu `B.C`.</span><span class="sxs-lookup"><span data-stu-id="5b04b-305">The accessibility domain of `B.C.Z` is the program text of `B.C`.</span></span>
*  <span data-ttu-id="5b04b-306">Doména přístupnosti `B.D.X` a `B.D.Y` je text programu `B`, včetně textu programu `B.C` a `B.D`.</span><span class="sxs-lookup"><span data-stu-id="5b04b-306">The accessibility domain of `B.D.X` and `B.D.Y` is the program text of `B`, including the program text of `B.C` and `B.D`.</span></span>
*  <span data-ttu-id="5b04b-307">Doména přístupnosti `B.D.Z` je text programu `B.D`.</span><span class="sxs-lookup"><span data-stu-id="5b04b-307">The accessibility domain of `B.D.Z` is the program text of `B.D`.</span></span>

<span data-ttu-id="5b04b-308">Jak ukazuje příklad, doména přístupnosti člena není nikdy větší než v nadřazeném typu.</span><span class="sxs-lookup"><span data-stu-id="5b04b-308">As the example illustrates, the accessibility domain of a member is never larger than that of a containing type.</span></span> <span data-ttu-id="5b04b-309">Například i v případě, že všichni `X` členové mají veřejnou deklaraci přístupnosti, všechny, ale `A.X` mají domény přístupnosti, které jsou omezeny nadřazeným typem.</span><span class="sxs-lookup"><span data-stu-id="5b04b-309">For example, even though all `X` members have public declared accessibility, all but `A.X` have accessibility domains that are constrained by a containing type.</span></span>

<span data-ttu-id="5b04b-310">Jak je popsáno v tématu [Členové](basic-concepts.md#members), jsou všechny členy základní třídy, s výjimkou konstruktorů instancí, destruktorů a statických konstruktorů, děděny odvozenými typy.</span><span class="sxs-lookup"><span data-stu-id="5b04b-310">As described in [Members](basic-concepts.md#members), all members of a base class, except for instance constructors, destructors and static constructors, are inherited by derived types.</span></span> <span data-ttu-id="5b04b-311">To zahrnuje i soukromé členy základní třídy.</span><span class="sxs-lookup"><span data-stu-id="5b04b-311">This includes even private members of a base class.</span></span> <span data-ttu-id="5b04b-312">Doména přístupnosti privátního člena však obsahuje pouze text programu typu, ve kterém je člen deklarován.</span><span class="sxs-lookup"><span data-stu-id="5b04b-312">However, the accessibility domain of a private member includes only the program text of the type in which the member is declared.</span></span> <span data-ttu-id="5b04b-313">V příkladu</span><span class="sxs-lookup"><span data-stu-id="5b04b-313">In the example</span></span>
```csharp
class A
{
    int x;

    static void F(B b) {
        b.x = 1;        // Ok
    }
}

class B: A
{
    static void F(B b) {
        b.x = 1;        // Error, x not accessible
    }
}
```
<span data-ttu-id="5b04b-314">Třída `B` dědí soukromý člen `x` z třídy `A`.</span><span class="sxs-lookup"><span data-stu-id="5b04b-314">the `B` class inherits the private member `x` from the `A` class.</span></span> <span data-ttu-id="5b04b-315">Vzhledem k tomu, že je člen soukromý, je přístupný pouze v rámci *class_body* `A`.</span><span class="sxs-lookup"><span data-stu-id="5b04b-315">Because the member is private, it is only accessible within the *class_body* of `A`.</span></span> <span data-ttu-id="5b04b-316">Proto je přístup k `b.x` v metodě `A.F` úspěšný, ale v metodě `B.F` selže.</span><span class="sxs-lookup"><span data-stu-id="5b04b-316">Thus, the access to `b.x` succeeds in the `A.F` method, but fails in the `B.F` method.</span></span>

### <a name="protected-access-for-instance-members"></a><span data-ttu-id="5b04b-317">Chráněný přístup pro členy instance</span><span class="sxs-lookup"><span data-stu-id="5b04b-317">Protected access for instance members</span></span>

<span data-ttu-id="5b04b-318">Pokud je člen instance `protected` přístupný mimo text programu třídy, ve které je deklarována, a pokud je člen instance `protected internal` přístupný mimo text programu programu, ve kterém je deklarován, přístup musí probíhat v rámci deklarace třídy, která je odvozena od třídy, ve které je deklarována.</span><span class="sxs-lookup"><span data-stu-id="5b04b-318">When a `protected` instance member is accessed outside the program text of the class in which it is declared, and when a `protected internal` instance member is accessed outside the program text of the program in which it is declared, the access must take place within a class declaration that derives from the class in which it is declared.</span></span> <span data-ttu-id="5b04b-319">Kromě toho je nutné přístup provést prostřednictvím instance daného typu odvozené třídy nebo z něj vytvořeného typu třídy.</span><span class="sxs-lookup"><span data-stu-id="5b04b-319">Furthermore, the access is required to take place through an instance of that derived class type or a class type constructed from it.</span></span> <span data-ttu-id="5b04b-320">Toto omezení zabrání jedné odvozené třídě v přístupu k chráněným členům jiných odvozených tříd, a to i v případě, že jsou členové děděni ze stejné základní třídy.</span><span class="sxs-lookup"><span data-stu-id="5b04b-320">This restriction prevents one derived class from accessing protected members of other derived classes, even when the members are inherited from the same base class.</span></span>

<span data-ttu-id="5b04b-321">Nechejte `B` být základní třídou, která deklaruje člena chráněné instance `M`a nechat `D` být třída, která je odvozena od `B`.</span><span class="sxs-lookup"><span data-stu-id="5b04b-321">Let `B` be a base class that declares a protected instance member `M`, and let `D` be a class that derives from `B`.</span></span> <span data-ttu-id="5b04b-322">V rámci *class_body* `D`může přístup k `M` mít jednu z následujících forem:</span><span class="sxs-lookup"><span data-stu-id="5b04b-322">Within the *class_body* of `D`, access to `M` can take one of the following forms:</span></span>

*  <span data-ttu-id="5b04b-323">Nekvalifikovaný *TYPE_NAME* nebo *primary_expression* `M`formuláře.</span><span class="sxs-lookup"><span data-stu-id="5b04b-323">An unqualified *type_name* or *primary_expression* of the form `M`.</span></span>
*  <span data-ttu-id="5b04b-324">*Primary_expression* `E.M`formuláře za předpokladu, že typ `E` je `T` nebo třída odvozená od `T`, kde `T` je typ třídy `D`nebo typ třídy vytvořený z `D`</span><span class="sxs-lookup"><span data-stu-id="5b04b-324">A *primary_expression* of the form `E.M`, provided the type of `E` is `T` or a class derived from `T`, where `T` is the class type `D`, or a class type constructed from `D`</span></span>
*  <span data-ttu-id="5b04b-325">`base.M`*primary_expression* formuláře.</span><span class="sxs-lookup"><span data-stu-id="5b04b-325">A *primary_expression* of the form `base.M`.</span></span>

<span data-ttu-id="5b04b-326">Kromě těchto forem přístupu může odvozená třída přistupovat k konstruktoru chráněné instance základní třídy v *constructor_initializer* ([Inicializátory konstruktoru](classes.md#constructor-initializers)).</span><span class="sxs-lookup"><span data-stu-id="5b04b-326">In addition to these forms of access, a derived class can access a protected instance constructor of a base class in a *constructor_initializer* ([Constructor initializers](classes.md#constructor-initializers)).</span></span>

<span data-ttu-id="5b04b-327">V příkladu</span><span class="sxs-lookup"><span data-stu-id="5b04b-327">In the example</span></span>
```csharp
public class A
{
    protected int x;

    static void F(A a, B b) {
        a.x = 1;        // Ok
        b.x = 1;        // Ok
    }
}

public class B: A
{
    static void F(A a, B b) {
        a.x = 1;        // Error, must access through instance of B
        b.x = 1;        // Ok
    }
}
```
<span data-ttu-id="5b04b-328">v rámci `A`je možné přistupovat `x` prostřednictvím instancí obou `A` a `B`, protože v obou případech dojde k přístupu prostřednictvím instance `A` nebo třídy odvozené od `A`.</span><span class="sxs-lookup"><span data-stu-id="5b04b-328">within `A`, it is possible to access `x` through instances of both `A` and `B`, since in either case the access takes place through an instance of `A` or a class derived from `A`.</span></span> <span data-ttu-id="5b04b-329">V rámci `B`ale není možné získat přístup `x` prostřednictvím instance `A`, protože `A` neodvozuje z `B`.</span><span class="sxs-lookup"><span data-stu-id="5b04b-329">However, within `B`, it is not possible to access `x` through an instance of `A`, since `A` does not derive from `B`.</span></span>

<span data-ttu-id="5b04b-330">V příkladu</span><span class="sxs-lookup"><span data-stu-id="5b04b-330">In the example</span></span>
```csharp
class C<T>
{
    protected T x;
}

class D<T>: C<T>
{
    static void F() {
        D<T> dt = new D<T>();
        D<int> di = new D<int>();
        D<string> ds = new D<string>();
        dt.x = default(T);
        di.x = 123;
        ds.x = "test";
    }
}
```
<span data-ttu-id="5b04b-331">tři přiřazení k `x` jsou povolena, protože všechna probíhají prostřednictvím instancí typů třídy vytvořených z obecného typu.</span><span class="sxs-lookup"><span data-stu-id="5b04b-331">the three assignments to `x` are permitted because they all take place through instances of class types constructed from the generic type.</span></span>

### <a name="accessibility-constraints"></a><span data-ttu-id="5b04b-332">Omezení přístupnosti</span><span class="sxs-lookup"><span data-stu-id="5b04b-332">Accessibility constraints</span></span>

<span data-ttu-id="5b04b-333">Několik konstrukcí v C# jazyce vyžaduje, aby byl typ ***aspoň dostupný jako*** člen nebo jiný typ.</span><span class="sxs-lookup"><span data-stu-id="5b04b-333">Several constructs in the C# language require a type to be ***at least as accessible as*** a member or another type.</span></span> <span data-ttu-id="5b04b-334">Typ `T` se říká, že má být nejméně tak přístupný jako člen nebo typ `M`, pokud je doména přístupnosti `T` nadmnožinou domény přístupnosti `M`.</span><span class="sxs-lookup"><span data-stu-id="5b04b-334">A type `T` is said to be at least as accessible as a member or type `M` if the accessibility domain of `T` is a superset of the accessibility domain of `M`.</span></span> <span data-ttu-id="5b04b-335">Jinými slovy, `T` je alespoň tak přístupný jako `M`, pokud je `T` přístupná ve všech kontextech, ve kterých `M` k dispozici.</span><span class="sxs-lookup"><span data-stu-id="5b04b-335">In other words, `T` is at least as accessible as `M` if `T` is accessible in all contexts in which `M` is accessible.</span></span>

<span data-ttu-id="5b04b-336">Existují následující omezení přístupnosti:</span><span class="sxs-lookup"><span data-stu-id="5b04b-336">The following accessibility constraints exist:</span></span>

*  <span data-ttu-id="5b04b-337">Přímá základní třída typu třídy musí být alespoň tak přístupná jako typ třídy samotné.</span><span class="sxs-lookup"><span data-stu-id="5b04b-337">The direct base class of a class type must be at least as accessible as the class type itself.</span></span>
*  <span data-ttu-id="5b04b-338">Explicitní základní rozhraní typu rozhraní musí být alespoň tak přístupná jako typ rozhraní.</span><span class="sxs-lookup"><span data-stu-id="5b04b-338">The explicit base interfaces of an interface type must be at least as accessible as the interface type itself.</span></span>
*  <span data-ttu-id="5b04b-339">Návratový typ a typy parametrů typu delegáta musí být k dispozici alespoň jako typ delegáta.</span><span class="sxs-lookup"><span data-stu-id="5b04b-339">The return type and parameter types of a delegate type must be at least as accessible as the delegate type itself.</span></span>
*  <span data-ttu-id="5b04b-340">Typ konstanty musí být alespoň tak přístupný jako konstanta sama.</span><span class="sxs-lookup"><span data-stu-id="5b04b-340">The type of a constant must be at least as accessible as the constant itself.</span></span>
*  <span data-ttu-id="5b04b-341">Typ pole musí být alespoň tak přístupný jako pole samotné.</span><span class="sxs-lookup"><span data-stu-id="5b04b-341">The type of a field must be at least as accessible as the field itself.</span></span>
*  <span data-ttu-id="5b04b-342">Návratový typ a typy parametrů metody musí být k dispozici alespoň jako metoda samotná.</span><span class="sxs-lookup"><span data-stu-id="5b04b-342">The return type and parameter types of a method must be at least as accessible as the method itself.</span></span>
*  <span data-ttu-id="5b04b-343">Typ vlastnosti musí být alespoň tak přístupný jako vlastnost sama.</span><span class="sxs-lookup"><span data-stu-id="5b04b-343">The type of a property must be at least as accessible as the property itself.</span></span>
*  <span data-ttu-id="5b04b-344">Typ události musí být alespoň tak přístupný jako událost samotná.</span><span class="sxs-lookup"><span data-stu-id="5b04b-344">The type of an event must be at least as accessible as the event itself.</span></span>
*  <span data-ttu-id="5b04b-345">Typy a parametry indexeru musí být alespoň tak přístupné jako indexer samotný.</span><span class="sxs-lookup"><span data-stu-id="5b04b-345">The type and parameter types of an indexer must be at least as accessible as the indexer itself.</span></span>
*  <span data-ttu-id="5b04b-346">Návratový typ a typy parametrů operátoru musí být k dispozici alespoň jako samotný operátor.</span><span class="sxs-lookup"><span data-stu-id="5b04b-346">The return type and parameter types of an operator must be at least as accessible as the operator itself.</span></span>
*  <span data-ttu-id="5b04b-347">Typy parametrů konstruktoru instance musí být alespoň tak přístupné jako samotný konstruktor instance.</span><span class="sxs-lookup"><span data-stu-id="5b04b-347">The parameter types of an instance constructor must be at least as accessible as the instance constructor itself.</span></span>

<span data-ttu-id="5b04b-348">V příkladu</span><span class="sxs-lookup"><span data-stu-id="5b04b-348">In the example</span></span>
```csharp
class A {...}

public class B: A {...}
```
<span data-ttu-id="5b04b-349">u `B` třídy dojde k chybě při kompilaci, protože `A` není alespoň tak přístupná jako `B`.</span><span class="sxs-lookup"><span data-stu-id="5b04b-349">the `B` class results in a compile-time error because `A` is not at least as accessible as `B`.</span></span>

<span data-ttu-id="5b04b-350">Podobně v příkladu</span><span class="sxs-lookup"><span data-stu-id="5b04b-350">Likewise, in the example</span></span>
```csharp
class A {...}

public class B
{
    A F() {...}

    internal A G() {...}

    public A H() {...}
}
```
<span data-ttu-id="5b04b-351">Metoda `H` v `B` má za následek chybu při kompilaci, protože návratový typ `A` není k dispozici jako metoda.</span><span class="sxs-lookup"><span data-stu-id="5b04b-351">the `H` method in `B` results in a compile-time error because the return type `A` is not at least as accessible as the method.</span></span>

## <a name="signatures-and-overloading"></a><span data-ttu-id="5b04b-352">Signatury a přetížení</span><span class="sxs-lookup"><span data-stu-id="5b04b-352">Signatures and overloading</span></span>

<span data-ttu-id="5b04b-353">Metody, konstruktory instancí, indexery a operátory jsou charakterizovány svými ***signaturami***:</span><span class="sxs-lookup"><span data-stu-id="5b04b-353">Methods, instance constructors, indexers, and operators are characterized by their ***signatures***:</span></span>

*  <span data-ttu-id="5b04b-354">Signatura metody se skládá z názvu metody, počtu parametrů typu a typu a druhu (hodnota, odkaz nebo výstup) každého jeho formálního parametru, který je považován za v pořadí zleva doprava.</span><span class="sxs-lookup"><span data-stu-id="5b04b-354">The signature of a method consists of the name of the method, the number of type parameters and the type and kind (value, reference, or output) of each of its formal parameters, considered in the order left to right.</span></span> <span data-ttu-id="5b04b-355">Pro tyto účely je jakýkoli parametr typu metody, která se vyskytuje v typu formálního parametru, identifikován jako není jeho názvem, ale podle jeho ordinální pozice v seznamu argumentů typu metody.</span><span class="sxs-lookup"><span data-stu-id="5b04b-355">For these purposes, any type parameter of the method that occurs in the type of a formal parameter is identified not by its name, but by its ordinal position in the type argument list of the method.</span></span> <span data-ttu-id="5b04b-356">Signatura metody konkrétně neobsahuje návratový typ, modifikátor `params`, který může být zadán pro parametr Right-nejvíc, ani volitelná omezení parametrů typu.</span><span class="sxs-lookup"><span data-stu-id="5b04b-356">The signature of a method specifically does not include the return type, the `params` modifier that may be specified for the right-most parameter, nor the optional type parameter constraints.</span></span>
*  <span data-ttu-id="5b04b-357">Signatura konstruktoru instance se skládá z typu a druhu (hodnota, odkaz nebo výstup) každého jeho formálního parametru, který je považován za v pořadí zleva doprava.</span><span class="sxs-lookup"><span data-stu-id="5b04b-357">The signature of an instance constructor consists of the type and kind (value, reference, or output) of each of its formal parameters, considered in the order left to right.</span></span> <span data-ttu-id="5b04b-358">Signatura konstruktoru konstruktoru konkrétně neobsahuje modifikátor `params`, který může být zadán pro parametr Right-nejvíc.</span><span class="sxs-lookup"><span data-stu-id="5b04b-358">The signature of an instance constructor specifically does not include the `params` modifier that may be specified for the right-most parameter.</span></span>
*  <span data-ttu-id="5b04b-359">Signatura indexeru se skládá z typu každého ze svých formálních parametrů, které se považují za v pořadí zleva doprava.</span><span class="sxs-lookup"><span data-stu-id="5b04b-359">The signature of an indexer consists of the type of each of its formal parameters, considered in the order left to right.</span></span> <span data-ttu-id="5b04b-360">Podpis indexeru konkrétně nezahrnuje typ elementu, ani neobsahuje modifikátor `params`, který může být zadán pro parametr Right-nejvíc.</span><span class="sxs-lookup"><span data-stu-id="5b04b-360">The signature of an indexer specifically does not include the element type, nor does it include the `params` modifier that may be specified for the right-most parameter.</span></span>
*  <span data-ttu-id="5b04b-361">Signatura operátoru se skládá z názvu operátoru a typu každého z jeho formálních parametrů, které se považují za v pořadí zleva doprava.</span><span class="sxs-lookup"><span data-stu-id="5b04b-361">The signature of an operator consists of the name of the operator and the type of each of its formal parameters, considered in the order left to right.</span></span> <span data-ttu-id="5b04b-362">Podpis operátora konkrétně neobsahuje typ výsledku.</span><span class="sxs-lookup"><span data-stu-id="5b04b-362">The signature of an operator specifically does not include the result type.</span></span>

<span data-ttu-id="5b04b-363">Signatury jsou mechanismy povolování pro ***přetížení*** členů ve třídách, strukturách a rozhraních:</span><span class="sxs-lookup"><span data-stu-id="5b04b-363">Signatures are the enabling mechanism for ***overloading*** of members in classes, structs, and interfaces:</span></span>

*  <span data-ttu-id="5b04b-364">Přetížení metod umožňuje třídy, struktury nebo rozhraní deklarovat více metod se stejným názvem, pokud jsou jejich podpisy v rámci této třídy, struktury nebo rozhraní jedinečné.</span><span class="sxs-lookup"><span data-stu-id="5b04b-364">Overloading of methods permits a class, struct, or interface to declare multiple methods with the same name, provided their signatures are unique within that class, struct, or interface.</span></span>
*  <span data-ttu-id="5b04b-365">Přetížení konstruktorů instancí umožňuje třídě nebo struktuře deklarovat více konstruktorů instancí za předpokladu, že jejich signatury jsou v rámci této třídy nebo struktury jedinečné.</span><span class="sxs-lookup"><span data-stu-id="5b04b-365">Overloading of instance constructors permits a class or struct to declare multiple instance constructors, provided their signatures are unique within that class or struct.</span></span>
*  <span data-ttu-id="5b04b-366">Přetížení indexerů umožňuje třídy, struktury nebo rozhraní deklarovat více indexerů za předpokladu, že jejich signatury jsou v rámci této třídy, struktury nebo rozhraní jedinečné.</span><span class="sxs-lookup"><span data-stu-id="5b04b-366">Overloading of indexers permits a class, struct, or interface to declare multiple indexers, provided their signatures are unique within that class, struct, or interface.</span></span>
*  <span data-ttu-id="5b04b-367">Přetížení operátorů umožňuje třídě nebo struktuře deklarovat více operátorů se stejným názvem, pokud jsou jejich podpisy v rámci této třídy nebo struktury jedinečné.</span><span class="sxs-lookup"><span data-stu-id="5b04b-367">Overloading of operators permits a class or struct to declare multiple operators with the same name, provided their signatures are unique within that class or struct.</span></span>

<span data-ttu-id="5b04b-368">I když jsou modifikátory parametrů `out` a `ref` považovány za součást signatury, členy deklarované v jednom typu se nemohou lišit v signatuře pouze pomocí `ref` a `out`.</span><span class="sxs-lookup"><span data-stu-id="5b04b-368">Although `out` and `ref` parameter modifiers are considered part of a signature, members declared in a single type cannot differ in signature solely by `ref` and `out`.</span></span> <span data-ttu-id="5b04b-369">K chybě v době kompilace dojde v případě, že dva členy jsou deklarovány ve stejném typu s signaturami, které by byly stejné, pokud se všechny parametry v obou metodách s modifikátory `out` změnily na `ref` modifikátory.</span><span class="sxs-lookup"><span data-stu-id="5b04b-369">A compile-time error occurs if two members are declared in the same type with signatures that would be the same if all parameters in both methods with `out` modifiers were changed to `ref` modifiers.</span></span> <span data-ttu-id="5b04b-370">Pro jiné účely porovnávání signatur (například skrytí nebo přepsání) `ref` a `out` se považují za součást signatury a vzájemně se neshodují.</span><span class="sxs-lookup"><span data-stu-id="5b04b-370">For other purposes of signature matching (e.g., hiding or overriding), `ref` and `out` are considered part of the signature and do not match each other.</span></span> <span data-ttu-id="5b04b-371">(Toto omezení je umožněno C# snadné překladu programů na Common Language Infrastructure (CLI), což neposkytuje způsob, jak definovat metody, které se liší pouze v `ref` a `out`.)</span><span class="sxs-lookup"><span data-stu-id="5b04b-371">(This restriction is to allow C#  programs to be easily translated to run on the Common Language Infrastructure (CLI), which does not provide a way to define methods that differ solely in `ref` and `out`.)</span></span>

<span data-ttu-id="5b04b-372">Pro účely signatur se typy `object` a `dynamic` považují za stejné.</span><span class="sxs-lookup"><span data-stu-id="5b04b-372">For the purposes of signatures, the types `object` and `dynamic` are considered the same.</span></span> <span data-ttu-id="5b04b-373">Členy deklarované v jednom typu se proto nemohou lišit v signatuře pouze pomocí `object` a `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="5b04b-373">Members declared in a single type can therefore not differ in signature solely by `object` and `dynamic`.</span></span>

<span data-ttu-id="5b04b-374">Následující příklad ukazuje sadu přetížených deklarací metod spolu s jejich podpisy.</span><span class="sxs-lookup"><span data-stu-id="5b04b-374">The following example shows a set of overloaded method declarations along with their signatures.</span></span>
```csharp
interface ITest
{
    void F();                        // F()

    void F(int x);                   // F(int)

    void F(ref int x);               // F(ref int)

    void F(out int x);               // F(out int)      error

    void F(int x, int y);            // F(int, int)

    int F(string s);                 // F(string)

    int F(int x);                    // F(int)          error

    void F(string[] a);              // F(string[])

    void F(params string[] a);       // F(string[])     error
}
```

<span data-ttu-id="5b04b-375">Všimněte si, že všechny modifikátory parametrů `ref` a `out` ([parametry metody](classes.md#method-parameters)) jsou součástí signatury.</span><span class="sxs-lookup"><span data-stu-id="5b04b-375">Note that any `ref` and `out` parameter modifiers ([Method parameters](classes.md#method-parameters)) are part of a signature.</span></span> <span data-ttu-id="5b04b-376">Proto jsou `F(int)` a `F(ref int)` jedinečnými podpisy.</span><span class="sxs-lookup"><span data-stu-id="5b04b-376">Thus, `F(int)` and `F(ref int)` are unique signatures.</span></span> <span data-ttu-id="5b04b-377">`F(ref int)` a `F(out int)` však nelze deklarovat v rámci stejného rozhraní, protože jejich signatury se liší výhradně pomocí `ref` a `out`.</span><span class="sxs-lookup"><span data-stu-id="5b04b-377">However, `F(ref int)` and `F(out int)` cannot be declared within the same interface because their signatures differ solely by `ref` and `out`.</span></span> <span data-ttu-id="5b04b-378">Všimněte si také, že návratový typ a modifikátor `params` nejsou součástí signatury, takže není možné přetížit pouze na základě návratového typu nebo při zahrnutí nebo vyloučení modifikátoru `params`.</span><span class="sxs-lookup"><span data-stu-id="5b04b-378">Also, note that the return type and the `params` modifier are not part of a signature, so it is not possible to overload solely based on return type or on the inclusion or exclusion of the `params` modifier.</span></span> <span data-ttu-id="5b04b-379">V takovém případě deklarace metod `F(int)` a `F(params string[])` zjistily výše uvedený výsledek při kompilaci.</span><span class="sxs-lookup"><span data-stu-id="5b04b-379">As such, the declarations of the methods `F(int)` and `F(params string[])` identified above result in a compile-time error.</span></span>

## <a name="scopes"></a><span data-ttu-id="5b04b-380">Oboru</span><span class="sxs-lookup"><span data-stu-id="5b04b-380">Scopes</span></span>

<span data-ttu-id="5b04b-381">***Rozsah*** názvu je oblast textu programu, ve které je možné odkazovat na entitu deklarované názvem bez kvalifikace názvu.</span><span class="sxs-lookup"><span data-stu-id="5b04b-381">The ***scope*** of a name is the region of program text within which it is possible to refer to the entity declared by the name without qualification of the name.</span></span> <span data-ttu-id="5b04b-382">Obory můžou být ***vnořené***a vnitřní rozsah může znovu deklarovat význam názvu z vnějšího oboru (to ale neodebere omezení vyplývající z [deklarací](basic-concepts.md#declarations) , které v rámci vnořeného bloku není možné deklarovat místní proměnnou se stejným názvem jako místní proměnná v ohraničujícím bloku.)</span><span class="sxs-lookup"><span data-stu-id="5b04b-382">Scopes can be ***nested***, and an inner scope may redeclare the meaning of a name from an outer scope (this does not, however, remove the restriction imposed by [Declarations](basic-concepts.md#declarations) that within a nested block it is not possible to declare a local variable with the same name as a local variable in an enclosing block).</span></span> <span data-ttu-id="5b04b-383">Název z vnějšího oboru se pak bude považovat za ***skrytý*** v oblasti textu programu, který je pokrytý vnitřním rozsahem, a přístup k vnějšímu názvu je možný jenom pomocí kvalifikovaného názvu.</span><span class="sxs-lookup"><span data-stu-id="5b04b-383">The name from the outer scope is then said to be ***hidden*** in the region of program text covered by the inner scope, and access to the outer name is only possible by qualifying the name.</span></span>

*  <span data-ttu-id="5b04b-384">Rozsah člena oboru názvů deklarovaného *namespace_member_declaration* ([členy oboru názvů](namespaces.md#namespace-members)) bez uzavírací *namespace_declaration* je celý text programu.</span><span class="sxs-lookup"><span data-stu-id="5b04b-384">The scope of a namespace member declared by a *namespace_member_declaration* ([Namespace members](namespaces.md#namespace-members)) with no enclosing *namespace_declaration* is the entire program text.</span></span>
*  <span data-ttu-id="5b04b-385">Rozsah člena oboru názvů deklarovaného *namespace_member_declaration* v rámci *namespace_declaration* , jehož plně kvalifikovaný název je `N` *namespace_body* všech *namespace_declaration* , jejichž plně kvalifikovaný název je `N` nebo začíná na `N`a za tečkou.</span><span class="sxs-lookup"><span data-stu-id="5b04b-385">The scope of a namespace member declared by a *namespace_member_declaration* within a *namespace_declaration* whose fully qualified name is `N` is the *namespace_body* of every *namespace_declaration* whose fully qualified name is `N` or starts with `N`, followed by a period.</span></span>
*  <span data-ttu-id="5b04b-386">Rozsah názvu definovaného *extern_alias_directive* se rozšíří přes *using_directive*s, *global_attributes* a *namespace_member_declaration*s jeho okamžitou součástí, který obsahuje kompilační jednotka nebo tělo oboru názvů.</span><span class="sxs-lookup"><span data-stu-id="5b04b-386">The scope of name defined by an *extern_alias_directive* extends over the *using_directive*s, *global_attributes* and *namespace_member_declaration*s of its immediately containing compilation unit or namespace body.</span></span> <span data-ttu-id="5b04b-387">*Extern_alias_directive* nepřispívají žádné nové členy do podkladového prostoru deklarací.</span><span class="sxs-lookup"><span data-stu-id="5b04b-387">An *extern_alias_directive* does not contribute any new members to the underlying declaration space.</span></span> <span data-ttu-id="5b04b-388">Jinými slovy *extern_alias_directive* není přenositelné, ale místo toho ovlivňuje pouze kompilační jednotku nebo tělo oboru názvů, ve kterém se vyskytuje.</span><span class="sxs-lookup"><span data-stu-id="5b04b-388">In other words, an *extern_alias_directive* is not transitive, but, rather, affects only the compilation unit or namespace body in which it occurs.</span></span>
*  <span data-ttu-id="5b04b-389">Rozsah názvu definovaného nebo importovaného pomocí *using_directive* ([direktivy using](namespaces.md#using-directives)) se rozšíří přes *namespace_member_declaration*s *compilation_unit* nebo *namespace_body* , ve kterém se *using_directive* vyskytuje.</span><span class="sxs-lookup"><span data-stu-id="5b04b-389">The scope of a name defined or imported by a *using_directive* ([Using directives](namespaces.md#using-directives)) extends over the *namespace_member_declaration*s of the *compilation_unit* or *namespace_body* in which the *using_directive* occurs.</span></span> <span data-ttu-id="5b04b-390">*Using_directive* může v rámci konkrétní *compilation_unit* nebo *namespace_body*vydávat nula nebo více názvů, typ nebo názvy členů, ale nepřispívají žádné nové členy do podkladového prostoru deklarací.</span><span class="sxs-lookup"><span data-stu-id="5b04b-390">A *using_directive* may make zero or more namespace, type or member names available within a particular *compilation_unit* or *namespace_body*, but does not contribute any new members to the underlying declaration space.</span></span> <span data-ttu-id="5b04b-391">Jinými slovy *using_directive* není přenosný, ale má vliv pouze na *compilation_unit* nebo *namespace_body* , ve kterém se vyskytuje.</span><span class="sxs-lookup"><span data-stu-id="5b04b-391">In other words, a *using_directive* is not transitive but rather affects only the *compilation_unit* or *namespace_body* in which it occurs.</span></span>
*  <span data-ttu-id="5b04b-392">Rozsah parametru typu deklarovaného *type_parameter_list* v *class_declaration* ([deklarace tříd](classes.md#class-declarations)) je *class_base*, *type_parameter_constraints_clause*s a *class_body* tohoto *class_declaration*.</span><span class="sxs-lookup"><span data-stu-id="5b04b-392">The scope of a type parameter declared by a *type_parameter_list* on a *class_declaration* ([Class declarations](classes.md#class-declarations)) is the *class_base*, *type_parameter_constraints_clause*s, and *class_body* of that *class_declaration*.</span></span>
*  <span data-ttu-id="5b04b-393">Rozsah parametru typu deklarovaného *type_parameter_list* v *struct_declaration* ([deklarace struktury](structs.md#struct-declarations)) je *struct_interfaces*, *type_parameter_constraints_clause*s a *struct_body* tohoto *struct_declaration*.</span><span class="sxs-lookup"><span data-stu-id="5b04b-393">The scope of a type parameter declared by a *type_parameter_list* on a *struct_declaration* ([Struct declarations](structs.md#struct-declarations)) is the *struct_interfaces*, *type_parameter_constraints_clause*s, and *struct_body* of that *struct_declaration*.</span></span>
*  <span data-ttu-id="5b04b-394">Rozsah parametru typu deklarovaného *type_parameter_list* v *interface_declaration* ([deklarace rozhraní](interfaces.md#interface-declarations)) je *interface_base*, *type_parameter_constraints_clause*s a *interface_body* tohoto *interface_declaration*.</span><span class="sxs-lookup"><span data-stu-id="5b04b-394">The scope of a type parameter declared by a *type_parameter_list* on an *interface_declaration* ([Interface declarations](interfaces.md#interface-declarations)) is the *interface_base*, *type_parameter_constraints_clause*s, and *interface_body* of that *interface_declaration*.</span></span>
*  <span data-ttu-id="5b04b-395">Rozsah parametru typu deklarovaného *type_parameter_list* v *delegate_declaration* ([deklarace delegátů](delegates.md#delegate-declarations)) je *return_type*, *formal_parameter_list*a *type_parameter_constraints_clause*s daného *delegate_declaration*.</span><span class="sxs-lookup"><span data-stu-id="5b04b-395">The scope of a type parameter declared by a *type_parameter_list* on a *delegate_declaration* ([Delegate declarations](delegates.md#delegate-declarations)) is the *return_type*, *formal_parameter_list*, and *type_parameter_constraints_clause*s of that *delegate_declaration*.</span></span>
*  <span data-ttu-id="5b04b-396">Rozsah člena deklarovaného *class_member_declaration* ([tělo třídy](classes.md#class-body)) je *class_body* , ve kterém k deklaraci dojde.</span><span class="sxs-lookup"><span data-stu-id="5b04b-396">The scope of a member declared by a *class_member_declaration* ([Class body](classes.md#class-body)) is the *class_body* in which the declaration occurs.</span></span> <span data-ttu-id="5b04b-397">Kromě toho rozsah člena třídy rozšiřuje na *class_body* těch odvozených tříd, které jsou zahrnuty v doméně přístupnosti ([domény](basic-concepts.md#accessibility-domains)přístupnosti) člena.</span><span class="sxs-lookup"><span data-stu-id="5b04b-397">In addition, the scope of a class member extends to the *class_body* of those derived classes that are included in the accessibility domain ([Accessibility domains](basic-concepts.md#accessibility-domains)) of the member.</span></span>
*  <span data-ttu-id="5b04b-398">Rozsah člena deklarovaného *struct_member_declaration* ([Členové struktury](structs.md#struct-members)) je *struct_body* , ve kterém k deklaraci dojde.</span><span class="sxs-lookup"><span data-stu-id="5b04b-398">The scope of a member declared by a *struct_member_declaration* ([Struct members](structs.md#struct-members)) is the *struct_body* in which the declaration occurs.</span></span>
*  <span data-ttu-id="5b04b-399">Rozsah člena deklarovaného *enum_member_declaration* ([členy výčtu](enums.md#enum-members)) je *enum_body* , ve kterém k deklaraci dojde.</span><span class="sxs-lookup"><span data-stu-id="5b04b-399">The scope of a member declared by an *enum_member_declaration*  ([Enum members](enums.md#enum-members)) is the *enum_body* in which the declaration occurs.</span></span>
*  <span data-ttu-id="5b04b-400">Rozsah parametru deklarovaného v *method_declaration* ([metody](classes.md#methods)) je *method_body* tohoto *method_declaration*.</span><span class="sxs-lookup"><span data-stu-id="5b04b-400">The scope of a parameter declared in a *method_declaration* ([Methods](classes.md#methods)) is the *method_body* of that *method_declaration*.</span></span>
*  <span data-ttu-id="5b04b-401">Rozsah parametru deklarovaného v *indexer_declaration* ([indexery](classes.md#indexers)) je *accessor_declarations* tohoto *indexer_declaration*.</span><span class="sxs-lookup"><span data-stu-id="5b04b-401">The scope of a parameter declared in an *indexer_declaration* ([Indexers](classes.md#indexers)) is the *accessor_declarations* of that *indexer_declaration*.</span></span>
*  <span data-ttu-id="5b04b-402">Rozsah parametru deklarovaného v *operator_declaration* ([Operators](classes.md#operators)) je *blok* tohoto *operator_declaration*.</span><span class="sxs-lookup"><span data-stu-id="5b04b-402">The scope of a parameter declared in an *operator_declaration* ([Operators](classes.md#operators)) is the *block* of that *operator_declaration*.</span></span>
*  <span data-ttu-id="5b04b-403">Rozsah parametru deklarovaného v *constructor_declaration* ([konstruktory Instance](classes.md#instance-constructors)) je *constructor_initializer* a *blok* tohoto *constructor_declaration*.</span><span class="sxs-lookup"><span data-stu-id="5b04b-403">The scope of a parameter declared in a *constructor_declaration* ([Instance constructors](classes.md#instance-constructors)) is the *constructor_initializer* and *block* of that *constructor_declaration*.</span></span>
*  <span data-ttu-id="5b04b-404">Rozsah parametru deklarovaného v *lambda_expression* ([anonymní výrazy funkce](expressions.md#anonymous-function-expressions)) je *anonymous_function_body* tohoto *lambda_expression*</span><span class="sxs-lookup"><span data-stu-id="5b04b-404">The scope of a parameter declared in a *lambda_expression* ([Anonymous function expressions](expressions.md#anonymous-function-expressions)) is the *anonymous_function_body* of that *lambda_expression*</span></span>
*  <span data-ttu-id="5b04b-405">Rozsah parametru deklarovaného v *anonymous_method_expression* ([anonymní výrazy funkce](expressions.md#anonymous-function-expressions)) je *blok* tohoto *anonymous_method_expression*.</span><span class="sxs-lookup"><span data-stu-id="5b04b-405">The scope of a parameter declared in an *anonymous_method_expression* ([Anonymous function expressions](expressions.md#anonymous-function-expressions)) is the *block* of that *anonymous_method_expression*.</span></span>
*  <span data-ttu-id="5b04b-406">Rozsah popisku deklarovaného v *labeled_statement* ([příkazy s popiskem](statements.md#labeled-statements)) je *blok* , ve kterém k deklaraci dojde.</span><span class="sxs-lookup"><span data-stu-id="5b04b-406">The scope of a label declared in a *labeled_statement* ([Labeled statements](statements.md#labeled-statements)) is the *block* in which the declaration occurs.</span></span>
*  <span data-ttu-id="5b04b-407">Rozsah místní proměnné deklarované v *local_variable_declaration* ([deklarace místní proměnné](statements.md#local-variable-declarations)) je blok, ve kterém se deklarace vyskytuje.</span><span class="sxs-lookup"><span data-stu-id="5b04b-407">The scope of a local variable declared in a *local_variable_declaration* ([Local variable declarations](statements.md#local-variable-declarations)) is the block in which the declaration occurs.</span></span>
*  <span data-ttu-id="5b04b-408">Rozsah místní proměnné deklarované v *switch_block* příkazu `switch` ([příkaz switch](statements.md#the-switch-statement)) je *switch_block*.</span><span class="sxs-lookup"><span data-stu-id="5b04b-408">The scope of a local variable declared in a *switch_block* of a `switch` statement ([The switch statement](statements.md#the-switch-statement)) is the *switch_block*.</span></span>
*  <span data-ttu-id="5b04b-409">Rozsah místní proměnné deklarované v *for_initializer* příkazu `for` ([pro příkaz for](statements.md#the-for-statement)) je *for_initializer*, *for_condition*, *for_iterator*a obsažený *příkaz* `for`.</span><span class="sxs-lookup"><span data-stu-id="5b04b-409">The scope of a local variable declared in a *for_initializer* of a `for` statement ([The for statement](statements.md#the-for-statement)) is the *for_initializer*, the *for_condition*, the *for_iterator*, and the contained *statement* of the `for` statement.</span></span>
*  <span data-ttu-id="5b04b-410">Rozsah místní konstanty deklarované v *local_constant_declaration* ([místní konstanta deklarace](statements.md#local-constant-declarations)) je blok, ve kterém k deklaraci dojde.</span><span class="sxs-lookup"><span data-stu-id="5b04b-410">The scope of a local constant declared in a *local_constant_declaration* ([Local constant declarations](statements.md#local-constant-declarations)) is the block in which the declaration occurs.</span></span> <span data-ttu-id="5b04b-411">Jedná se o chybu při kompilaci, která odkazuje na místní konstantu v textové pozici, která předchází jeho *constant_declarator*.</span><span class="sxs-lookup"><span data-stu-id="5b04b-411">It is a compile-time error to refer to a local constant in a textual position that precedes its *constant_declarator*.</span></span>
*  <span data-ttu-id="5b04b-412">Rozsah proměnné deklarované jako součást *foreach_statement*, *using_statement*, *lock_statement* nebo *query_expression* je určen rozšířením dané konstrukce.</span><span class="sxs-lookup"><span data-stu-id="5b04b-412">The scope of a variable declared as part of a *foreach_statement*, *using_statement*, *lock_statement* or *query_expression* is determined by the expansion of the given construct.</span></span>

<span data-ttu-id="5b04b-413">V rámci rozsahu oboru názvů, třídy, struktury nebo člena výčtu je možné odkazovat na člen v textové pozici, která předchází deklaraci členu.</span><span class="sxs-lookup"><span data-stu-id="5b04b-413">Within the scope of a namespace, class, struct, or enumeration member it is possible to refer to the member in a textual position that precedes the declaration of the member.</span></span> <span data-ttu-id="5b04b-414">Například</span><span class="sxs-lookup"><span data-stu-id="5b04b-414">For example</span></span>
```csharp
class A
{
    void F() {
        i = 1;
    }

    int i = 0;
}
```
<span data-ttu-id="5b04b-415">Tady je platný, aby `F` odkazoval na `i` předtím, než se deklaruje.</span><span class="sxs-lookup"><span data-stu-id="5b04b-415">Here, it is valid for `F` to refer to `i` before it is declared.</span></span>

<span data-ttu-id="5b04b-416">V rámci rozsahu místní proměnné se jedná o chybu při kompilaci, která odkazuje na místní proměnnou na textové pozici, která předchází *local_variable_declarator* místní proměnné.</span><span class="sxs-lookup"><span data-stu-id="5b04b-416">Within the scope of a local variable, it is a compile-time error to refer to the local variable in a textual position that precedes the *local_variable_declarator* of the local variable.</span></span> <span data-ttu-id="5b04b-417">Například</span><span class="sxs-lookup"><span data-stu-id="5b04b-417">For example</span></span>
```csharp
class A
{
    int i = 0;

    void F() {
        i = 1;                  // Error, use precedes declaration
        int i;
        i = 2;
    }

    void G() {
        int j = (j = 1);        // Valid
    }

    void H() {
        int a = 1, b = ++a;    // Valid
    }
}
```

<span data-ttu-id="5b04b-418">V metodě `F` výše první přiřazení `i` výslovně neodkazuje na pole deklarované ve vnějším oboru.</span><span class="sxs-lookup"><span data-stu-id="5b04b-418">In the `F` method above, the first assignment to `i` specifically does not refer to the field declared in the outer scope.</span></span> <span data-ttu-id="5b04b-419">Místo toho odkazuje na místní proměnnou a má za následek chybu při kompilaci, protože předá deklaraci proměnné.</span><span class="sxs-lookup"><span data-stu-id="5b04b-419">Rather, it refers to the local variable and it results in a compile-time error because it textually precedes the declaration of the variable.</span></span> <span data-ttu-id="5b04b-420">V metodě `G` je použití `j` v inicializátoru pro deklaraci `j` platné, protože použití nepředchází *local_variable_declarator*.</span><span class="sxs-lookup"><span data-stu-id="5b04b-420">In the `G` method, the use of `j` in the initializer for the declaration of `j` is valid because the use does not precede the *local_variable_declarator*.</span></span> <span data-ttu-id="5b04b-421">V metodě `H` následující *local_variable_declarator* správně odkazuje na místní proměnnou deklarovanou v dřívějším *local_variable_declarator* v rámci stejného *local_variable_declaration*.</span><span class="sxs-lookup"><span data-stu-id="5b04b-421">In the `H` method, a subsequent *local_variable_declarator* correctly refers to a local variable declared in an earlier *local_variable_declarator* within the same *local_variable_declaration*.</span></span>

<span data-ttu-id="5b04b-422">Pravidla oboru pro lokální proměnné jsou navržena tak, aby zaručila, že význam názvu používaného v kontextu výrazu je vždy stejný v rámci bloku.</span><span class="sxs-lookup"><span data-stu-id="5b04b-422">The scoping rules for local variables are designed to guarantee that the meaning of a name used in an expression context is always the same within a block.</span></span> <span data-ttu-id="5b04b-423">Pokud byl rozsah místní proměnné rozšířen pouze z deklarace na konec bloku, pak v příkladu výše se první přiřazení přiřadí proměnné instance a druhé přiřazení by se mohlo přiřazovat k místní proměnné, což může vést k chyby při kompilaci, pokud byly příkazy bloku později přeuspořádány.</span><span class="sxs-lookup"><span data-stu-id="5b04b-423">If the scope of a local variable were to extend only from its declaration to the end of the block, then in the example above, the first assignment would assign to the instance variable and the second assignment would assign to the local variable, possibly leading to compile-time errors if the statements of the block were later to be rearranged.</span></span>

<span data-ttu-id="5b04b-424">Význam názvu v rámci bloku se může lišit v závislosti na kontextu, ve kterém je název použit.</span><span class="sxs-lookup"><span data-stu-id="5b04b-424">The meaning of a name within a block may differ based on the context in which the name is used.</span></span> <span data-ttu-id="5b04b-425">V příkladu</span><span class="sxs-lookup"><span data-stu-id="5b04b-425">In the example</span></span>
```csharp
using System;

class A {}

class Test
{
    static void Main() {
        string A = "hello, world";
        string s = A;                            // expression context

        Type t = typeof(A);                      // type context

        Console.WriteLine(s);                    // writes "hello, world"
        Console.WriteLine(t);                    // writes "A"
    }
}
```
<span data-ttu-id="5b04b-426">název `A` se používá v kontextu výrazu pro odkazování na místní proměnnou `A` a v kontextu typu pro odkazování na `A`třídy.</span><span class="sxs-lookup"><span data-stu-id="5b04b-426">the name `A` is used in an expression context to refer to the local variable `A` and in a type context to refer to the class `A`.</span></span>

### <a name="name-hiding"></a><span data-ttu-id="5b04b-427">Skrytí názvu</span><span class="sxs-lookup"><span data-stu-id="5b04b-427">Name hiding</span></span>

<span data-ttu-id="5b04b-428">Rozsah entity obvykle zahrnuje větší text programu než místo deklarace entity.</span><span class="sxs-lookup"><span data-stu-id="5b04b-428">The scope of an entity typically encompasses more program text than the declaration space of the entity.</span></span> <span data-ttu-id="5b04b-429">Konkrétně rozsah entity může zahrnovat deklarace, které zavádějí nové prostory deklarací obsahující entity se stejným názvem.</span><span class="sxs-lookup"><span data-stu-id="5b04b-429">In particular, the scope of an entity may include declarations that introduce new declaration spaces containing entities of the same name.</span></span> <span data-ttu-id="5b04b-430">Tyto deklarace způsobí, že původní entita bude ***Skrytá***.</span><span class="sxs-lookup"><span data-stu-id="5b04b-430">Such declarations cause the original entity to become ***hidden***.</span></span> <span data-ttu-id="5b04b-431">Naopak entita je označována jako ***viditelná*** , pokud není skrytá.</span><span class="sxs-lookup"><span data-stu-id="5b04b-431">Conversely, an entity is said to be ***visible*** when it is not hidden.</span></span>

<span data-ttu-id="5b04b-432">K skrývání názvů dojde, když se obory překrývají prostřednictvím vnořování a když se obory překrývají prostřednictvím dědičnosti.</span><span class="sxs-lookup"><span data-stu-id="5b04b-432">Name hiding occurs when scopes overlap through nesting and when scopes overlap through inheritance.</span></span> <span data-ttu-id="5b04b-433">Vlastnosti dvou typů skrytí jsou popsány v následujících částech.</span><span class="sxs-lookup"><span data-stu-id="5b04b-433">The characteristics of the two types of hiding are described in the following sections.</span></span>

#### <a name="hiding-through-nesting"></a><span data-ttu-id="5b04b-434">Skrytí prostřednictvím vnořování</span><span class="sxs-lookup"><span data-stu-id="5b04b-434">Hiding through nesting</span></span>

<span data-ttu-id="5b04b-435">Název, který se skrývá prostřednictvím vnořování, může nastat jako výsledek vnořování oborů názvů nebo typů v oborech názvů jako výsledek vnořování typů v rámci tříd nebo struktur, a v důsledku deklarací parametrů a místních proměnných.</span><span class="sxs-lookup"><span data-stu-id="5b04b-435">Name hiding through nesting can occur as a result of nesting namespaces or types within namespaces, as a result of nesting types within classes or structs, and as a result of parameter and local variable declarations.</span></span>

<span data-ttu-id="5b04b-436">V příkladu</span><span class="sxs-lookup"><span data-stu-id="5b04b-436">In the example</span></span>
```csharp
class A
{
    int i = 0;

    void F() {
        int i = 1;
    }

    void G() {
        i = 1;
    }
}
```
<span data-ttu-id="5b04b-437">v rámci metody `F` je proměnná instance `i` skrytá místní proměnnou `i`, ale v rámci metody `G` `i` stále odkazuje na proměnnou instance.</span><span class="sxs-lookup"><span data-stu-id="5b04b-437">within the `F` method, the instance variable `i` is hidden by the local variable `i`, but within the `G` method, `i` still refers to the instance variable.</span></span>

<span data-ttu-id="5b04b-438">Když název ve vnitřním oboru skrývá název ve vnějším oboru, skryje všechny přetížené výskyty daného názvu.</span><span class="sxs-lookup"><span data-stu-id="5b04b-438">When a name in an inner scope hides a name in an outer scope, it hides all overloaded occurrences of that name.</span></span> <span data-ttu-id="5b04b-439">V příkladu</span><span class="sxs-lookup"><span data-stu-id="5b04b-439">In the example</span></span>
```csharp
class Outer
{
    static void F(int i) {}

    static void F(string s) {}

    class Inner
    {
        void G() {
            F(1);              // Invokes Outer.Inner.F
            F("Hello");        // Error
        }

        static void F(long l) {}
    }
}
```
<span data-ttu-id="5b04b-440">volání `F(1)` vyvolá `F` deklarované v `Inner`, protože vnitřní deklarace má skryté všechny vnější výskyty `F`.</span><span class="sxs-lookup"><span data-stu-id="5b04b-440">the call `F(1)` invokes the `F` declared in `Inner` because all outer occurrences of `F` are hidden by the inner declaration.</span></span> <span data-ttu-id="5b04b-441">Ze stejného důvodu `F("Hello")` volání způsobí chybu při kompilaci.</span><span class="sxs-lookup"><span data-stu-id="5b04b-441">For the same reason, the call `F("Hello")` results in a compile-time error.</span></span>

#### <a name="hiding-through-inheritance"></a><span data-ttu-id="5b04b-442">Skrytí prostřednictvím dědičnosti</span><span class="sxs-lookup"><span data-stu-id="5b04b-442">Hiding through inheritance</span></span>

<span data-ttu-id="5b04b-443">K skrývání názvů prostřednictvím dědičnosti dojde, když třídy nebo struktury znovu deklaruje názvy, které byly děděny ze základních tříd.</span><span class="sxs-lookup"><span data-stu-id="5b04b-443">Name hiding through inheritance occurs when classes or structs redeclare names that were inherited from base classes.</span></span> <span data-ttu-id="5b04b-444">Tento typ názvu skrývání má jednu z následujících forem:</span><span class="sxs-lookup"><span data-stu-id="5b04b-444">This type of name hiding takes one of the following forms:</span></span>

*  <span data-ttu-id="5b04b-445">Konstanta, pole, vlastnost, událost nebo typ zavedený ve třídě nebo struktuře skrývá všechny členy základní třídy se stejným názvem.</span><span class="sxs-lookup"><span data-stu-id="5b04b-445">A constant, field, property, event, or type introduced in a class or struct hides all base class members with the same name.</span></span>
*  <span data-ttu-id="5b04b-446">Metoda zavedená ve třídě nebo struktuře skrývá všechny členy základní třídy bez metod se stejným názvem a všechny metody základní třídy se stejným podpisem (název metody a počet parametrů, modifikátory a typy).</span><span class="sxs-lookup"><span data-stu-id="5b04b-446">A method introduced in a class or struct hides all non-method base class members with the same name, and all base class methods with the same signature (method name and parameter count, modifiers, and types).</span></span>
*  <span data-ttu-id="5b04b-447">Indexer zavedený ve třídě nebo struktuře skrývá všechny základní třídy indexery se stejnou signaturou (počet parametrů a typy).</span><span class="sxs-lookup"><span data-stu-id="5b04b-447">An indexer introduced in a class or struct hides all base class indexers with the same signature (parameter count and types).</span></span>

<span data-ttu-id="5b04b-448">Pravidla řídící deklarace operátorů ([operátory](classes.md#operators)) znemožňují, aby odvozená třída deklarovala operátor se stejnou signaturou jako operátor v základní třídě.</span><span class="sxs-lookup"><span data-stu-id="5b04b-448">The rules governing operator declarations ([Operators](classes.md#operators)) make it impossible for a derived class to declare an operator with the same signature as an operator in a base class.</span></span> <span data-ttu-id="5b04b-449">Proto operátory nikdy neskrývají.</span><span class="sxs-lookup"><span data-stu-id="5b04b-449">Thus, operators never hide one another.</span></span>

<span data-ttu-id="5b04b-450">V rozporu s skrytím názvu z vnějšího oboru způsobí skrytí přístupového názvu z zděděného oboru upozornění.</span><span class="sxs-lookup"><span data-stu-id="5b04b-450">Contrary to hiding a name from an outer scope, hiding an accessible name from an inherited scope causes a warning to be reported.</span></span> <span data-ttu-id="5b04b-451">V příkladu</span><span class="sxs-lookup"><span data-stu-id="5b04b-451">In the example</span></span>
```csharp
class Base
{
    public void F() {}
}

class Derived: Base
{
    public void F() {}        // Warning, hiding an inherited name
}
```
<span data-ttu-id="5b04b-452">deklarace `F` v `Derived` způsobí, že bude Hlášeno upozornění.</span><span class="sxs-lookup"><span data-stu-id="5b04b-452">the declaration of `F` in `Derived` causes a warning to be reported.</span></span> <span data-ttu-id="5b04b-453">Skrytí zděděného názvu není specificky chyba, protože by to vedlo k samostatnému vývoji základních tříd.</span><span class="sxs-lookup"><span data-stu-id="5b04b-453">Hiding an inherited name is specifically not an error, since that would preclude separate evolution of base classes.</span></span> <span data-ttu-id="5b04b-454">Výše uvedená situace mohla například proniknout, protože novější verze `Base` představila `F` metodu, která nebyla přítomna v dřívější verzi třídy.</span><span class="sxs-lookup"><span data-stu-id="5b04b-454">For example, the above situation might have come about because a later version of `Base` introduced an `F` method that wasn't present in an earlier version of the class.</span></span> <span data-ttu-id="5b04b-455">V výše uvedené situaci došlo k chybě, takže jakákoli změna základní třídy v knihovně tříd oddělené verze by mohla potenciálně způsobit, že odvozené třídy stanou být neplatné.</span><span class="sxs-lookup"><span data-stu-id="5b04b-455">Had the above situation been an error, then any change made to a base class in a separately versioned class library could potentially cause derived classes to become invalid.</span></span>

<span data-ttu-id="5b04b-456">Upozornění způsobené skrytím zděděného názvu lze odstranit pomocí modifikátoru `new`:</span><span class="sxs-lookup"><span data-stu-id="5b04b-456">The warning caused by hiding an inherited name can be eliminated through use of the `new` modifier:</span></span>
```csharp
class Base
{
    public void F() {}
}

class Derived: Base
{
    new public void F() {}
}
```

<span data-ttu-id="5b04b-457">Modifikátor `new` označuje, že `F` v `Derived` je "nové" a že je skutečně určena pro skrytí zděděného člena.</span><span class="sxs-lookup"><span data-stu-id="5b04b-457">The `new` modifier indicates that the `F` in `Derived` is "new", and that it is indeed intended to hide the inherited member.</span></span>

<span data-ttu-id="5b04b-458">Deklarace nového člena skrývá zděděného člena pouze v rámci oboru nového člena.</span><span class="sxs-lookup"><span data-stu-id="5b04b-458">A declaration of a new member hides an inherited member only within the scope of the new member.</span></span>

```csharp
class Base
{
    public static void F() {}
}

class Derived: Base
{
    new private static void F() {}    // Hides Base.F in Derived only
}

class MoreDerived: Derived
{
    static void G() { F(); }          // Invokes Base.F
}
```

<span data-ttu-id="5b04b-459">V předchozím příkladu deklarace `F` v `Derived` skrývá `F`, která byla zděděná od `Base`, ale vzhledem k tomu, že nový `F` `Derived` má privátní přístup, jeho obor se nerozšiřuje na `MoreDerived`.</span><span class="sxs-lookup"><span data-stu-id="5b04b-459">In the example above, the declaration of `F` in `Derived` hides the `F` that was inherited from `Base`, but since the new `F` in `Derived` has private access, its scope does not extend to `MoreDerived`.</span></span> <span data-ttu-id="5b04b-460">Proto je volání `F()` v `MoreDerived.G` platné a vyvolá `Base.F`.</span><span class="sxs-lookup"><span data-stu-id="5b04b-460">Thus, the call `F()` in `MoreDerived.G` is valid and will invoke `Base.F`.</span></span>

## <a name="namespace-and-type-names"></a><span data-ttu-id="5b04b-461">Obor názvů a názvy typů</span><span class="sxs-lookup"><span data-stu-id="5b04b-461">Namespace and type names</span></span>

<span data-ttu-id="5b04b-462">Několik kontextů v C# programu vyžaduje zadání *namespace_name* nebo *TYPE_NAME* .</span><span class="sxs-lookup"><span data-stu-id="5b04b-462">Several contexts in a C# program require a *namespace_name* or a *type_name* to be specified.</span></span>

```antlr
namespace_name
    : namespace_or_type_name
    ;

type_name
    : namespace_or_type_name
    ;

namespace_or_type_name
    : identifier type_argument_list?
    | namespace_or_type_name '.' identifier type_argument_list?
    | qualified_alias_member
    ;
```

<span data-ttu-id="5b04b-463">*Namespace_name* je *namespace_or_type_name* , který odkazuje na obor názvů.</span><span class="sxs-lookup"><span data-stu-id="5b04b-463">A *namespace_name* is a *namespace_or_type_name* that refers to a namespace.</span></span> <span data-ttu-id="5b04b-464">Následující řešení, jak je popsáno níže, *namespace_or_type_name* *namespace_name* musí odkazovat na obor názvů nebo v případě chyby při kompilaci.</span><span class="sxs-lookup"><span data-stu-id="5b04b-464">Following resolution as described below, the *namespace_or_type_name* of a *namespace_name* must refer to a namespace, or otherwise a compile-time error occurs.</span></span> <span data-ttu-id="5b04b-465">V *namespace_name* se nesmí vyskytovat žádné argumenty typu ([argumenty typu](types.md#type-arguments)) (argumenty typu můžou mít jenom typy).</span><span class="sxs-lookup"><span data-stu-id="5b04b-465">No type arguments ([Type arguments](types.md#type-arguments)) can be present in a *namespace_name* (only types can have type arguments).</span></span>

<span data-ttu-id="5b04b-466">*TYPE_NAME* je *namespace_or_type_name* , který odkazuje na typ.</span><span class="sxs-lookup"><span data-stu-id="5b04b-466">A *type_name* is a *namespace_or_type_name* that refers to a type.</span></span> <span data-ttu-id="5b04b-467">Následující řešení, jak je popsáno níže, *namespace_or_type_name* *TYPE_NAME* musí odkazovat na typ nebo jinak dojde k chybě při kompilaci.</span><span class="sxs-lookup"><span data-stu-id="5b04b-467">Following resolution as described below, the *namespace_or_type_name* of a *type_name* must refer to a type, or otherwise a compile-time error occurs.</span></span>

<span data-ttu-id="5b04b-468">Pokud *namespace_or_type_name* je kvalifikovaný alias členu, jak je popsáno v [kvalifikátorech aliasu oboru názvů](namespaces.md#namespace-alias-qualifiers).</span><span class="sxs-lookup"><span data-stu-id="5b04b-468">If the *namespace_or_type_name* is a qualified-alias-member its meaning is as described in [Namespace alias qualifiers](namespaces.md#namespace-alias-qualifiers).</span></span> <span data-ttu-id="5b04b-469">V opačném případě *namespace_or_type_name* má jednu ze čtyř forem:</span><span class="sxs-lookup"><span data-stu-id="5b04b-469">Otherwise, a *namespace_or_type_name* has one of four forms:</span></span>

*  `I`
*  `I<A1, ..., Ak>`
*  `N.I`
*  `N.I<A1, ..., Ak>`

<span data-ttu-id="5b04b-470">Pokud je `I` jediným identifikátorem, `N` je *namespace_or_type_name* a `<A1, ..., Ak>` je volitelná *type_argument_list*.</span><span class="sxs-lookup"><span data-stu-id="5b04b-470">where `I` is a single identifier, `N` is a *namespace_or_type_name* and `<A1, ..., Ak>` is an optional *type_argument_list*.</span></span> <span data-ttu-id="5b04b-471">Pokud není zadaný žádný *type_argument_list* , zvažte `k` na nulu.</span><span class="sxs-lookup"><span data-stu-id="5b04b-471">When no *type_argument_list* is specified, consider `k` to be zero.</span></span>

<span data-ttu-id="5b04b-472">Význam *namespace_or_type_name* se určuje takto:</span><span class="sxs-lookup"><span data-stu-id="5b04b-472">The meaning of a *namespace_or_type_name* is determined as follows:</span></span>

*   <span data-ttu-id="5b04b-473">Pokud má *namespace_or_type_name* `I` nebo formuláře `I<A1, ..., Ak>`:</span><span class="sxs-lookup"><span data-stu-id="5b04b-473">If the *namespace_or_type_name* is of the form `I` or of the form `I<A1, ..., Ak>`:</span></span>
    * <span data-ttu-id="5b04b-474">Pokud je `K` nula a *namespace_or_type_name* se objeví v rámci deklarace obecné metody ([metody](classes.md#methods)) a pokud tato deklarace obsahuje parametr typu ([parametry typu](classes.md#type-parameters)) s názvem `I`, pak *namespace_or_type_name* odkazuje na tento parametr typu.</span><span class="sxs-lookup"><span data-stu-id="5b04b-474">If `K` is zero and the *namespace_or_type_name* appears within a generic method declaration ([Methods](classes.md#methods)) and if that declaration includes a type parameter ([Type parameters](classes.md#type-parameters)) with name `I`, then the *namespace_or_type_name* refers to that type parameter.</span></span>
    * <span data-ttu-id="5b04b-475">V opačném případě, pokud se *namespace_or_type_name* objeví v rámci deklarace typu, pak pro každý typ instance `T` ([typ instance](classes.md#the-instance-type)), počínaje typem instance této deklarace a pokračování s typem instance každé nadřazené třídy nebo deklarace struktury (pokud existuje):</span><span class="sxs-lookup"><span data-stu-id="5b04b-475">Otherwise, if the *namespace_or_type_name* appears within a type declaration, then for each instance type `T` ([The instance type](classes.md#the-instance-type)), starting with the instance type of that type declaration and continuing with the instance type of each enclosing class or struct declaration (if any):</span></span>
        * <span data-ttu-id="5b04b-476">Pokud je `K` nula a deklarace `T` obsahuje parametr typu s názvem `I`, pak *namespace_or_type_name* odkazuje na tento parametr typu.</span><span class="sxs-lookup"><span data-stu-id="5b04b-476">If `K` is zero and the declaration of `T` includes a type parameter with name `I`, then the *namespace_or_type_name* refers to that type parameter.</span></span>
        * <span data-ttu-id="5b04b-477">V opačném případě, pokud se *namespace_or_type_name* objeví v těle deklarace typu a `T` nebo některý z jeho základních typů obsahuje vnořený přístupný typ, který má název `I` a `K` parametry typu, pak *namespace_or_type_name* odkazuje na tento typ sestavený pomocí daných argumentů typu.</span><span class="sxs-lookup"><span data-stu-id="5b04b-477">Otherwise, if the *namespace_or_type_name* appears within the body of the type declaration, and `T` or any of its base types contain a nested accessible type having name `I` and `K` type parameters, then the *namespace_or_type_name* refers to that type constructed with the given type arguments.</span></span> <span data-ttu-id="5b04b-478">Pokud existuje více než jeden takový typ, je vybrán typ deklarovaný v rámci více odvozeného typu.</span><span class="sxs-lookup"><span data-stu-id="5b04b-478">If there is more than one such type, the type declared within the more derived type is selected.</span></span> <span data-ttu-id="5b04b-479">Všimněte si, že netypové členy (konstanty, pole, metody, vlastnosti, indexery, operátory, konstruktory instancí, destruktory a statické konstruktory) a členy typu s jiným počtem parametrů typu jsou při určování významu *namespace_or_type_name*ignorovány.</span><span class="sxs-lookup"><span data-stu-id="5b04b-479">Note that non-type members (constants, fields, methods, properties, indexers, operators, instance constructors, destructors, and static constructors) and type members with a different number of type parameters are ignored when determining the meaning of the *namespace_or_type_name*.</span></span>
    * <span data-ttu-id="5b04b-480">Pokud předchozí kroky byly neúspěšné, potom pro každý `N`oboru názvů počínaje oborem názvů, ve kterém *namespace_or_type_name* dochází, pokračuje s každým ohraničujícím oborem názvů (pokud existuje) a končí globálním oborem názvů, jsou vyhodnoceny následující kroky, dokud není nalezena entita:</span><span class="sxs-lookup"><span data-stu-id="5b04b-480">If the previous steps were unsuccessful then, for each namespace `N`, starting with the namespace in which the *namespace_or_type_name* occurs, continuing with each enclosing namespace (if any), and ending with the global namespace, the following steps are evaluated until an entity is located:</span></span>
        * <span data-ttu-id="5b04b-481">Pokud je `K` nula a `I` je název oboru názvů v `N`, pak:</span><span class="sxs-lookup"><span data-stu-id="5b04b-481">If `K` is zero and `I` is the name of a namespace in `N`, then:</span></span>
            * <span data-ttu-id="5b04b-482">Pokud se umístění, kde *namespace_or_type_name* dochází, je uzavřeno deklarací oboru názvů pro `N` a deklarace oboru názvů obsahuje *extern_alias_directive* nebo *using_alias_directive* , které přidruží název `I` k oboru názvů nebo typu, *namespace_or_type_name* je nejednoznačný a dojde k chybě při kompilaci.</span><span class="sxs-lookup"><span data-stu-id="5b04b-482">If the location where the *namespace_or_type_name* occurs is enclosed by a namespace declaration for `N` and the namespace declaration contains an *extern_alias_directive* or *using_alias_directive* that associates the name `I` with a namespace or type, then the *namespace_or_type_name* is ambiguous and a compile-time error occurs.</span></span>
            * <span data-ttu-id="5b04b-483">V opačném případě *namespace_or_type_name* odkazuje na obor názvů s názvem `I` v `N`.</span><span class="sxs-lookup"><span data-stu-id="5b04b-483">Otherwise, the *namespace_or_type_name* refers to the namespace named `I` in `N`.</span></span>
        * <span data-ttu-id="5b04b-484">Jinak, pokud `N` obsahuje přístupný typ s názvem `I` a `K` parametry typu, pak:</span><span class="sxs-lookup"><span data-stu-id="5b04b-484">Otherwise, if `N` contains an accessible type having name `I` and `K` type parameters, then:</span></span>
            * <span data-ttu-id="5b04b-485">Pokud je `K` nula a umístění, kde se *namespace_or_type_name* vyskytuje, je uzavřeno deklarací oboru názvů pro `N` a deklarace oboru názvů obsahuje *extern_alias_directive* nebo *using_alias_directive* , který přidruží název `I` k oboru názvů nebo typu, *namespace_or_type_name* je nejednoznačný a dojde k chybě při kompilaci.</span><span class="sxs-lookup"><span data-stu-id="5b04b-485">If `K` is zero and the location where the *namespace_or_type_name* occurs is enclosed by a namespace declaration for `N` and the namespace declaration contains an *extern_alias_directive* or *using_alias_directive* that associates the name `I` with a namespace or type, then the *namespace_or_type_name* is ambiguous and a compile-time error occurs.</span></span>
            * <span data-ttu-id="5b04b-486">V opačném případě *namespace_or_type_name* odkazuje na typ sestavený pomocí daných argumentů typu.</span><span class="sxs-lookup"><span data-stu-id="5b04b-486">Otherwise, the *namespace_or_type_name* refers to the type constructed with the given type arguments.</span></span>
        * <span data-ttu-id="5b04b-487">V opačném případě, pokud je umístění, kde *namespace_or_type_name* dojde, uzavřeno deklarací oboru názvů pro `N`:</span><span class="sxs-lookup"><span data-stu-id="5b04b-487">Otherwise, if the location where the *namespace_or_type_name* occurs is enclosed by a namespace declaration for `N`:</span></span>
            * <span data-ttu-id="5b04b-488">Pokud je `K` nula a deklarace oboru názvů obsahuje *extern_alias_directive* nebo *using_alias_directive* , které přidruží název `I` k importovanému oboru názvů nebo typu, pak *namespace_or_type_name* odkazuje na tento obor názvů nebo typ.</span><span class="sxs-lookup"><span data-stu-id="5b04b-488">If `K` is zero and the namespace declaration contains an *extern_alias_directive* or *using_alias_directive* that associates the name `I` with an imported namespace or type, then the *namespace_or_type_name* refers to that namespace or type.</span></span>
            * <span data-ttu-id="5b04b-489">V opačném případě, pokud deklarace oborů názvů a typů importované pomocí *using_namespace_directive*s a *using_alias_directive*s deklarací oboru názvů obsahují přesně jeden přístupný typ s názvem `I` a `K` parametry typu, pak *namespace_or_type_name* odkazuje na tento typ sestavený pomocí daných argumentů typu.</span><span class="sxs-lookup"><span data-stu-id="5b04b-489">Otherwise, if the namespaces and type declarations imported by the *using_namespace_directive*s and *using_alias_directive*s of the namespace declaration contain exactly one accessible type having name `I` and `K` type parameters, then the *namespace_or_type_name* refers to that type constructed with the given type arguments.</span></span>
            * <span data-ttu-id="5b04b-490">V opačném případě, pokud deklarace oborů názvů a typů importované pomocí *using_namespace_directive*s a *using_alias_directive*s deklarací oboru názvů obsahují více než jeden přístupný typ s názvem `I` a `K` parametry typu, *namespace_or_type_name* je nejednoznačný a dojde k chybě.</span><span class="sxs-lookup"><span data-stu-id="5b04b-490">Otherwise, if the namespaces and type declarations imported by the *using_namespace_directive*s and *using_alias_directive*s of the namespace declaration contain more than one accessible type having name `I` and `K` type parameters, then the *namespace_or_type_name* is ambiguous and an error occurs.</span></span>
    * <span data-ttu-id="5b04b-491">V opačném případě *namespace_or_type_name* není definován a dojde k chybě při kompilaci.</span><span class="sxs-lookup"><span data-stu-id="5b04b-491">Otherwise, the *namespace_or_type_name* is undefined and a compile-time error occurs.</span></span>
*  <span data-ttu-id="5b04b-492">V opačném případě je *namespace_or_type_name* `N.I` formuláře nebo formuláře `N.I<A1, ..., Ak>`.</span><span class="sxs-lookup"><span data-stu-id="5b04b-492">Otherwise, the *namespace_or_type_name* is of the form `N.I` or of the form `N.I<A1, ..., Ak>`.</span></span> <span data-ttu-id="5b04b-493">`N` je nejprve vyřešen jako *namespace_or_type_name*.</span><span class="sxs-lookup"><span data-stu-id="5b04b-493">`N` is first resolved as a *namespace_or_type_name*.</span></span> <span data-ttu-id="5b04b-494">Pokud řešení `N` neproběhne úspěšně, dojde k chybě při kompilaci.</span><span class="sxs-lookup"><span data-stu-id="5b04b-494">If the resolution of `N` is not successful, a compile-time error occurs.</span></span> <span data-ttu-id="5b04b-495">V opačném případě `N.I` nebo `N.I<A1, ..., Ak>` vyřešeny následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="5b04b-495">Otherwise, `N.I` or `N.I<A1, ..., Ak>` is resolved as follows:</span></span>
    * <span data-ttu-id="5b04b-496">Pokud je `K` nula a `N` odkazuje na obor názvů a `N` obsahuje vnořený obor názvů s názvem `I`, pak *namespace_or_type_name* odkazuje na tento vnořený obor názvů.</span><span class="sxs-lookup"><span data-stu-id="5b04b-496">If `K` is zero and `N` refers to a namespace and `N` contains a nested namespace with name `I`, then the *namespace_or_type_name* refers to that nested namespace.</span></span>
    * <span data-ttu-id="5b04b-497">V opačném případě, pokud `N` odkazuje na obor názvů a `N` obsahuje přístupný typ, který má název `I` a `K` parametry typu, pak *namespace_or_type_name* odkazuje na tento typ sestavený pomocí daných argumentů typu.</span><span class="sxs-lookup"><span data-stu-id="5b04b-497">Otherwise, if `N` refers to a namespace and `N` contains an accessible type having name `I` and `K` type parameters, then the *namespace_or_type_name* refers to that type constructed with the given type arguments.</span></span>
    * <span data-ttu-id="5b04b-498">V opačném případě, pokud `N` odkazuje na (případně konstruovaný) třídu nebo typ struktury a `N` nebo některá z jeho základních tříd obsahuje vnořený přístupný typ, který má název `I` a `K` parametry typu, pak *namespace_or_type_name* odkazuje na tento typ vytvořený pomocí daných argumentů typu.</span><span class="sxs-lookup"><span data-stu-id="5b04b-498">Otherwise, if `N` refers to a (possibly constructed) class or struct type and `N` or any of its base classes contain a nested accessible type having name `I` and `K` type parameters, then the *namespace_or_type_name* refers to that type constructed with the given type arguments.</span></span> <span data-ttu-id="5b04b-499">Pokud existuje více než jeden takový typ, je vybrán typ deklarovaný v rámci více odvozeného typu.</span><span class="sxs-lookup"><span data-stu-id="5b04b-499">If there is more than one such type, the type declared within the more derived type is selected.</span></span> <span data-ttu-id="5b04b-500">Všimněte si, že pokud je význam `N.I` určován jako součást překladu specifikace základní třídy `N` pak je přímá základní třída `N` považována za Object ([základní třídy](classes.md#base-classes)).</span><span class="sxs-lookup"><span data-stu-id="5b04b-500">Note that if the meaning of `N.I` is being determined as part of resolving the base class specification of `N` then the direct base class of `N` is considered to be object ([Base classes](classes.md#base-classes)).</span></span>
    * <span data-ttu-id="5b04b-501">V opačném případě je `N.I` neplatný *namespace_or_type_name*a dojde k chybě při kompilaci.</span><span class="sxs-lookup"><span data-stu-id="5b04b-501">Otherwise, `N.I` is an invalid *namespace_or_type_name*, and a compile-time error occurs.</span></span>

<span data-ttu-id="5b04b-502">*Namespace_or_type_name* je povolen odkazování na statickou třídu ([statické třídy](classes.md#static-classes)) pouze v případě, že</span><span class="sxs-lookup"><span data-stu-id="5b04b-502">A *namespace_or_type_name* is permitted to reference a static class ([Static classes](classes.md#static-classes)) only if</span></span>

*  <span data-ttu-id="5b04b-503">*Namespace_or_type_name* je `T` v *namespace_or_type_name* formuláře `T.I`nebo</span><span class="sxs-lookup"><span data-stu-id="5b04b-503">The *namespace_or_type_name* is the `T` in a *namespace_or_type_name* of the form `T.I`, or</span></span>
*  <span data-ttu-id="5b04b-504">*Namespace_or_type_name* je `T` ve *typeof_expression* ([seznam argumentů](expressions.md#argument-lists)1) `typeof(T)`formuláře.</span><span class="sxs-lookup"><span data-stu-id="5b04b-504">The *namespace_or_type_name* is the `T` in a *typeof_expression* ([Argument lists](expressions.md#argument-lists)1) of the form `typeof(T)`.</span></span>

### <a name="fully-qualified-names"></a><span data-ttu-id="5b04b-505">Plně kvalifikované názvy</span><span class="sxs-lookup"><span data-stu-id="5b04b-505">Fully qualified names</span></span>

<span data-ttu-id="5b04b-506">Každý obor názvů a typ má ***plně kvalifikovaný název***, který jednoznačně identifikuje obor názvů nebo typ mezi všemi ostatními.</span><span class="sxs-lookup"><span data-stu-id="5b04b-506">Every namespace and type has a ***fully qualified name***, which uniquely identifies the namespace or type amongst all others.</span></span> <span data-ttu-id="5b04b-507">Plně kvalifikovaný název oboru názvů nebo typu `N` je určen následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="5b04b-507">The fully qualified name of a namespace or type `N` is determined as follows:</span></span>

*  <span data-ttu-id="5b04b-508">Pokud je `N` členem globálního oboru názvů, je jeho plně kvalifikovaný název `N`.</span><span class="sxs-lookup"><span data-stu-id="5b04b-508">If `N` is a member of the global namespace, its fully qualified name is `N`.</span></span>
*  <span data-ttu-id="5b04b-509">V opačném případě je jeho plně kvalifikovaný název `S.N`, kde `S` je plně kvalifikovaný název oboru názvů nebo typu, ve kterém je deklarován `N`.</span><span class="sxs-lookup"><span data-stu-id="5b04b-509">Otherwise, its fully qualified name is `S.N`, where `S` is the fully qualified name of the namespace or type in which `N` is declared.</span></span>

<span data-ttu-id="5b04b-510">Jinými slovy je plně kvalifikovaný název `N` úplnou hierarchickou cestu identifikátorů, které vedou k `N`, počínaje globálním oborem názvů.</span><span class="sxs-lookup"><span data-stu-id="5b04b-510">In other words, the fully qualified name of `N` is the complete hierarchical path of identifiers that lead to `N`, starting from the global namespace.</span></span> <span data-ttu-id="5b04b-511">Vzhledem k tomu, že každý člen oboru názvů nebo typu musí mít jedinečný název, následuje za tím, že plně kvalifikovaný název oboru názvů nebo typu je vždycky jedinečný.</span><span class="sxs-lookup"><span data-stu-id="5b04b-511">Because every member of a namespace or type must have a unique name, it follows that the fully qualified name of a namespace or type is always unique.</span></span>

<span data-ttu-id="5b04b-512">Následující příklad ukazuje několik deklarací obor názvů a typů spolu s přidruženými plně kvalifikovanými názvy.</span><span class="sxs-lookup"><span data-stu-id="5b04b-512">The example below shows several namespace and type declarations along with their associated fully qualified names.</span></span>
```csharp
class A {}                // A

namespace X               // X
{
    class B               // X.B
    {
        class C {}        // X.B.C
    }

    namespace Y           // X.Y
    {
        class D {}        // X.Y.D
    }
}

namespace X.Y             // X.Y
{
    class E {}            // X.Y.E
}
```

## <a name="automatic-memory-management"></a><span data-ttu-id="5b04b-513">Automatická správa paměti</span><span class="sxs-lookup"><span data-stu-id="5b04b-513">Automatic memory management</span></span>

<span data-ttu-id="5b04b-514">C#využívá automatickou správu paměti, která vývojářům uvolňuje v ručním přidělování a uvolňování paměti obsazené objekty.</span><span class="sxs-lookup"><span data-stu-id="5b04b-514">C# employs automatic memory management, which frees developers from manually allocating and freeing the memory occupied by objects.</span></span> <span data-ttu-id="5b04b-515">Automatické zásady správy paměti jsou implementované systémem ***uvolňování***paměti.</span><span class="sxs-lookup"><span data-stu-id="5b04b-515">Automatic memory management policies are implemented by a ***garbage collector***.</span></span> <span data-ttu-id="5b04b-516">Životní cyklus správy paměti objektu je následující:</span><span class="sxs-lookup"><span data-stu-id="5b04b-516">The memory management life cycle of an object is as follows:</span></span>

1. <span data-ttu-id="5b04b-517">Když je objekt vytvořen, je pro něj přidělena paměť, konstruktor je spuštěn a objekt je považován za dynamický.</span><span class="sxs-lookup"><span data-stu-id="5b04b-517">When the object is created, memory is allocated for it, the constructor is run, and the object is considered live.</span></span>
2. <span data-ttu-id="5b04b-518">V případě, že k objektu nebo jakékoli jeho části nelze mít k dispozici žádné možné pokračování v provádění, jiné než spuštění destruktorů, objekt je považován za již nepoužit a bude způsobilý k zničení.</span><span class="sxs-lookup"><span data-stu-id="5b04b-518">If the object, or any part of it, cannot be accessed by any possible continuation of execution, other than the running of destructors, the object is considered no longer in use, and it becomes eligible for destruction.</span></span> <span data-ttu-id="5b04b-519">C# Kompilátor a systém uvolňování paměti se můžou rozhodnout analyzovat kód a určit, které odkazy na objekt se můžou v budoucnu použít.</span><span class="sxs-lookup"><span data-stu-id="5b04b-519">The C# compiler and the garbage collector may choose to analyze code to determine which references to an object may be used in the future.</span></span> <span data-ttu-id="5b04b-520">Například pokud místní proměnná, která je v oboru, je jediným existujícím odkazem na objekt, ale tato místní proměnná nikdy neodkazuje na jakékoli možné pokračování spuštění z aktuálního bodu spuštění v proceduře, může být systém uvolňování paměti (ale není vyžadováno pro), aby byl objekt považován za již nepoužitý.</span><span class="sxs-lookup"><span data-stu-id="5b04b-520">For instance, if a local variable that is in scope is the only existing reference to an object, but that local variable is never referred to in any possible continuation of execution from the current execution point in the procedure, the garbage collector may (but is not required to) treat the object as no longer in use.</span></span>
3. <span data-ttu-id="5b04b-521">Jakmile je objekt způsobilý k zničení, v některých nespecifikovaných pozdějších případech je spuštěn[destruktor (pokud existuje) pro](classes.md#destructors)daný objekt.</span><span class="sxs-lookup"><span data-stu-id="5b04b-521">Once the object is eligible for destruction, at some unspecified later time the destructor ([Destructors](classes.md#destructors)) (if any) for the object is run.</span></span> <span data-ttu-id="5b04b-522">Za normálních okolností je destruktor pro objekt spuštěn pouze jednou, i když rozhraní API specifická pro implementaci mohou toto chování přepsat.</span><span class="sxs-lookup"><span data-stu-id="5b04b-522">Under normal circumstances the destructor for the object is run once only, though implementation-specific APIs may allow this behavior to be overridden.</span></span>
4. <span data-ttu-id="5b04b-523">Po spuštění destruktoru pro objekt, pokud je k tomuto objektu nebo jakékoli části k němu nelze získat přístup jakýmkoli možným pokračováním v provádění, včetně spuštění destruktorů, je objekt považován za nedostupný a objekt se bude jednat o oprávněný pro kolekci.</span><span class="sxs-lookup"><span data-stu-id="5b04b-523">Once the destructor for an object is run, if that object, or any part of it, cannot be accessed by any possible continuation of execution, including the running of destructors, the object is considered inaccessible and the object becomes eligible for collection.</span></span>
5. <span data-ttu-id="5b04b-524">Nakonec, v některých případech, kdy se objekt bude oprávněn pro shromažďování, uvolní uvolňování paměti paměť přidruženou k tomuto objektu.</span><span class="sxs-lookup"><span data-stu-id="5b04b-524">Finally, at some time after the object becomes eligible for collection, the garbage collector frees the memory associated with that object.</span></span>

<span data-ttu-id="5b04b-525">Systém uvolňování paměti uchovává informace o použití objektů a používá tyto informace k tomu, aby bylo možné provádět rozhodnutí týkající se správy paměti, například kde v paměti vyhledat nově vytvořený objekt, kdy se má objekt přemístit, a když se objekt již nepoužívá nebo není přístupný.</span><span class="sxs-lookup"><span data-stu-id="5b04b-525">The garbage collector maintains information about object usage, and uses this information to make memory management decisions, such as where in memory to locate a newly created object, when to relocate an object, and when an object is no longer in use or inaccessible.</span></span>

<span data-ttu-id="5b04b-526">Podobně jako u jiných jazyků, které předpokládají existence systému uvolňování C# paměti, je navrženo tak, aby systém uvolňování paměti mohl implementovat široké spektrum zásad správy paměti.</span><span class="sxs-lookup"><span data-stu-id="5b04b-526">Like other languages that assume the existence of a garbage collector, C# is designed so that the garbage collector may implement a wide range of memory management policies.</span></span> <span data-ttu-id="5b04b-527">C# Například nevyžaduje, aby byly destruktory spuštěny, nebo aby byly shromažďovány, jakmile jsou způsobilé, nebo že destruktory jsou spouštěny v libovolném konkrétním pořadí nebo v jakémkoli konkrétním vlákně.</span><span class="sxs-lookup"><span data-stu-id="5b04b-527">For instance, C# does not require that destructors be run or that objects be collected as soon as they are eligible, or that destructors be run in any particular order, or on any particular thread.</span></span>

<span data-ttu-id="5b04b-528">Chování systému uvolňování paměti lze na určitou míru ovládat prostřednictvím statických metod třídy `System.GC`.</span><span class="sxs-lookup"><span data-stu-id="5b04b-528">The behavior of the garbage collector can be controlled, to some degree, via static methods on the class `System.GC`.</span></span> <span data-ttu-id="5b04b-529">Tato třída se dá použít k podání žádosti o kolekci, destruktory, které se mají spustit (nebo nespouštět), a tak dále.</span><span class="sxs-lookup"><span data-stu-id="5b04b-529">This class can be used to request a collection to occur, destructors to be run (or not run), and so forth.</span></span>

<span data-ttu-id="5b04b-530">Vzhledem k tomu, že systém uvolňování paměti je povolený rozsáhlá Zeměpisná šířka při rozhodování o shromažďování objektů a spouštění destruktorů, může vyhovující implementace způsobit výstup, který se liší od toho, který je zobrazen v následujícím kódu.</span><span class="sxs-lookup"><span data-stu-id="5b04b-530">Since the garbage collector is allowed wide latitude in deciding when to collect objects and run destructors, a conforming implementation may produce output that differs from that shown by the following code.</span></span> <span data-ttu-id="5b04b-531">Program</span><span class="sxs-lookup"><span data-stu-id="5b04b-531">The program</span></span>
```csharp
using System;

class A
{
    ~A() {
        Console.WriteLine("Destruct instance of A");
    }
}

class B
{
    object Ref;

    public B(object o) {
        Ref = o;
    }

    ~B() {
        Console.WriteLine("Destruct instance of B");
    }
}

class Test
{
    static void Main() {
        B b = new B(new A());
        b = null;
        GC.Collect();
        GC.WaitForPendingFinalizers();
    }
}
```
<span data-ttu-id="5b04b-532">Vytvoří instanci třídy `A` a instanci `B`třídy.</span><span class="sxs-lookup"><span data-stu-id="5b04b-532">creates an instance of class `A` and an instance of class `B`.</span></span> <span data-ttu-id="5b04b-533">Tyto objekty jsou způsobilé pro uvolňování paměti, když je proměnné `b` přiřazena hodnota `null`, protože po této době není možné, aby k nim měl přístup žádný kód, který by měl být vytvořen uživatelem.</span><span class="sxs-lookup"><span data-stu-id="5b04b-533">These objects become eligible for garbage collection when the variable `b` is assigned the value `null`, since after this time it is impossible for any user-written code to access them.</span></span> <span data-ttu-id="5b04b-534">Výstup může být buď</span><span class="sxs-lookup"><span data-stu-id="5b04b-534">The output could be either</span></span>

```console
Destruct instance of A
Destruct instance of B
```
<span data-ttu-id="5b04b-535">nebo</span><span class="sxs-lookup"><span data-stu-id="5b04b-535">or</span></span>
```console
Destruct instance of B
Destruct instance of A
```
<span data-ttu-id="5b04b-536">vzhledem k tomu, že jazyk ukládá žádná omezení v pořadí, ve kterém jsou objekty uvolněny z paměti.</span><span class="sxs-lookup"><span data-stu-id="5b04b-536">because the language imposes no constraints on the order in which objects are garbage collected.</span></span>

<span data-ttu-id="5b04b-537">V nejemných případech může být rozdíl mezi "způsobilým zničením" a "způsobilou pro kolekci" důležitý.</span><span class="sxs-lookup"><span data-stu-id="5b04b-537">In subtle cases, the distinction between "eligible for destruction" and "eligible for collection" can be important.</span></span> <span data-ttu-id="5b04b-538">Například</span><span class="sxs-lookup"><span data-stu-id="5b04b-538">For example,</span></span>
```csharp
using System;

class A
{
    ~A() {
        Console.WriteLine("Destruct instance of A");
    }

    public void F() {
        Console.WriteLine("A.F");
        Test.RefA = this;
    }
}

class B
{
    public A Ref;

    ~B() {
        Console.WriteLine("Destruct instance of B");
        Ref.F();
    }
}

class Test
{
    public static A RefA;
    public static B RefB;

    static void Main() {
        RefB = new B();
        RefA = new A();
        RefB.Ref = RefA;
        RefB = null;
        RefA = null;

        // A and B now eligible for destruction
        GC.Collect();
        GC.WaitForPendingFinalizers();

        // B now eligible for collection, but A is not
        if (RefA != null)
            Console.WriteLine("RefA is not null");
    }
}
```

<span data-ttu-id="5b04b-539">Pokud se ve výše uvedeném programu systém uvolňování paměti rozhodne spustit destruktor `A` před Destruktorem `B`, může být výstup tohoto programu následující:</span><span class="sxs-lookup"><span data-stu-id="5b04b-539">In the above program, if the garbage collector chooses to run the destructor of `A` before the destructor of `B`, then the output of this program might be:</span></span>
```console
Destruct instance of A
Destruct instance of B
A.F
RefA is not null
```

<span data-ttu-id="5b04b-540">Všimněte si, že i když se instance `A` nepoužívá a byl spuštěný destruktor `A`, je stále možné používat metody `A` (v tomto případě `F`), které mají být volány z jiného destruktoru.</span><span class="sxs-lookup"><span data-stu-id="5b04b-540">Note that although the instance of `A` was not in use and `A`'s destructor was run, it is still possible for methods of `A` (in this case, `F`) to be called from another destructor.</span></span> <span data-ttu-id="5b04b-541">Všimněte si také, že při spuštění destruktoru může dojít k opakovanému použití objektu v programu hlavní.</span><span class="sxs-lookup"><span data-stu-id="5b04b-541">Also, note that running of a destructor may cause an object to become usable from the mainline program again.</span></span> <span data-ttu-id="5b04b-542">V tomto případě spuštění destruktoru `B`způsobilo, že se instance `A`, která se dřív nepoužila k zpřístupnění z živého odkazu `Test.RefA`.</span><span class="sxs-lookup"><span data-stu-id="5b04b-542">In this case, the running of `B`'s destructor caused an instance of `A` that was previously not in use to become accessible from the live reference `Test.RefA`.</span></span> <span data-ttu-id="5b04b-543">Po volání `WaitForPendingFinalizers`má instance `B` nárok na kolekci, ale instance `A` není z důvodu referenčního `Test.RefA`.</span><span class="sxs-lookup"><span data-stu-id="5b04b-543">After the call to `WaitForPendingFinalizers`, the instance of `B` is eligible for collection, but the instance of `A` is not, because of the reference `Test.RefA`.</span></span>

<span data-ttu-id="5b04b-544">Aby nedošlo k nejasnostem a neočekávanému chování, je obecně vhodné, aby destruktory prováděly pouze čištění dat uložených ve vlastních polích objektu a aby neprováděly žádné akce s odkazovanými objekty nebo statickými poli.</span><span class="sxs-lookup"><span data-stu-id="5b04b-544">To avoid confusion and unexpected behavior, it is generally a good idea for destructors to only perform cleanup on data stored in their object's own fields, and not to perform any actions on referenced objects or static fields.</span></span>

<span data-ttu-id="5b04b-545">Alternativou k použití destruktorů je umožnit třídě implementovat rozhraní `System.IDisposable`.</span><span class="sxs-lookup"><span data-stu-id="5b04b-545">An alternative to using destructors is to let a class implement the `System.IDisposable` interface.</span></span> <span data-ttu-id="5b04b-546">To umožňuje klientovi objektu určit, kdy uvolnit prostředky objektu, obvykle přístupem k objektu jako prostředku v příkazu `using` ([příkaz using](statements.md#the-using-statement)).</span><span class="sxs-lookup"><span data-stu-id="5b04b-546">This allows the client of the object to determine when to release the resources of the object, typically by accessing the object as a resource in a `using` statement ([The using statement](statements.md#the-using-statement)).</span></span>

## <a name="execution-order"></a><span data-ttu-id="5b04b-547">Pořadí spouštění</span><span class="sxs-lookup"><span data-stu-id="5b04b-547">Execution order</span></span>

<span data-ttu-id="5b04b-548">Spuštění C# programu pokračuje tak, že vedlejší účinky každého zpracovávaného vlákna jsou zachovány v kritických bodech provádění.</span><span class="sxs-lookup"><span data-stu-id="5b04b-548">Execution of a C# program proceeds such that the side effects of each executing thread are preserved at critical execution points.</span></span> <span data-ttu-id="5b04b-549">***Vedlejší efekt*** je definován jako čtení nebo zápis nestálého pole, zápis do nestálé proměnné, zápis do externího prostředku a vyvolání výjimky.</span><span class="sxs-lookup"><span data-stu-id="5b04b-549">A ***side effect*** is defined as a read or write of a volatile field, a write to a non-volatile variable, a write to an external resource, and the throwing of an exception.</span></span> <span data-ttu-id="5b04b-550">Kritické body provádění, při kterých je třeba zachovat pořadí těchto vedlejších účinků, jsou odkazy na nestálá pole ([pole s modifikátorem volatile](classes.md#volatile-fields)), `lock` příkazy ([příkaz Lock](statements.md#the-lock-statement)) a vytvoření a ukončení vlákna.</span><span class="sxs-lookup"><span data-stu-id="5b04b-550">The critical execution points at which the order of these side effects must be preserved are references to volatile fields ([Volatile fields](classes.md#volatile-fields)), `lock` statements ([The lock statement](statements.md#the-lock-statement)), and thread creation and termination.</span></span> <span data-ttu-id="5b04b-551">Spouštěcí prostředí je zdarma, aby bylo možné změnit pořadí provádění C# programu, a to s těmito omezeními:</span><span class="sxs-lookup"><span data-stu-id="5b04b-551">The execution environment is free to change the order of execution of a C# program, subject to the following constraints:</span></span>

*  <span data-ttu-id="5b04b-552">Závislost dat se zachovává v rámci vlákna provádění.</span><span class="sxs-lookup"><span data-stu-id="5b04b-552">Data dependence is preserved within a thread of execution.</span></span> <span data-ttu-id="5b04b-553">To znamená, že hodnota každé proměnné je vypočítána jako v případě, že všechny příkazy ve vlákně byly provedeny v původní pořadí programu.</span><span class="sxs-lookup"><span data-stu-id="5b04b-553">That is, the value of each variable is computed as if all statements in the thread were executed in original program order.</span></span>
*  <span data-ttu-id="5b04b-554">Pravidla pro pořadí inicializace jsou zachována ([inicializace polí](classes.md#field-initialization) a [Inicializátory proměnných](classes.md#variable-initializers)).</span><span class="sxs-lookup"><span data-stu-id="5b04b-554">Initialization ordering rules are preserved ([Field initialization](classes.md#field-initialization) and [Variable initializers](classes.md#variable-initializers)).</span></span>
*  <span data-ttu-id="5b04b-555">Pořadí vedlejších účinků je zachováno s ohledem na nestálá čtení a zápisy ([pole s stálým](classes.md#volatile-fields)počtem).</span><span class="sxs-lookup"><span data-stu-id="5b04b-555">The ordering of side effects is preserved with respect to volatile reads and writes ([Volatile fields](classes.md#volatile-fields)).</span></span> <span data-ttu-id="5b04b-556">Kromě toho prostředí pro spuštění nemusí vyhodnotit část výrazu, pokud může odvodit, že hodnota tohoto výrazu není použita a že nejsou vytvořeny žádné požadované vedlejší účinky (včetně jakékoli příčiny způsobené voláním metody nebo přístupu k nestálému poli).</span><span class="sxs-lookup"><span data-stu-id="5b04b-556">Additionally, the execution environment need not evaluate part of an expression if it can deduce that that expression's value is not used and that no needed side effects are produced (including any caused by calling a method or accessing a volatile field).</span></span> <span data-ttu-id="5b04b-557">Když je provádění programu přerušeno asynchronní událostí (například výjimka vyvolaná jiným vláknem), není zaručeno, že pozorovatelé vedlejší účinky jsou viditelné v původním pořadí programu.</span><span class="sxs-lookup"><span data-stu-id="5b04b-557">When program execution is interrupted by an asynchronous event (such as an exception thrown by another thread), it is not guaranteed that the observable side effects are visible in the original program order.</span></span>
