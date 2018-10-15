# <a name="basic-concepts"></a><span data-ttu-id="42142-101">Základní koncepty</span><span class="sxs-lookup"><span data-stu-id="42142-101">Basic concepts</span></span>

## <a name="application-startup"></a><span data-ttu-id="42142-102">Spuštění aplikace</span><span class="sxs-lookup"><span data-stu-id="42142-102">Application Startup</span></span>

<span data-ttu-id="42142-103">Sestavení, který má ***vstupní bod*** je volána ***aplikace***.</span><span class="sxs-lookup"><span data-stu-id="42142-103">An assembly that has an ***entry point*** is called an ***application***.</span></span> <span data-ttu-id="42142-104">Pokud je aplikace spustit nový ***aplikační domény*** se vytvoří.</span><span class="sxs-lookup"><span data-stu-id="42142-104">When an application is run, a new ***application domain*** is created.</span></span> <span data-ttu-id="42142-105">Několik různých instancí aplikace může existovat ve stejném počítači ve stejnou dobu, a každý má své vlastní domény aplikace.</span><span class="sxs-lookup"><span data-stu-id="42142-105">Several different instantiations of an application may exist on the same machine at the same time, and each has its own application domain.</span></span>

<span data-ttu-id="42142-106">Domény aplikace umožňuje izolace aplikací tak, že funguje jako kontejner pro stav aplikace.</span><span class="sxs-lookup"><span data-stu-id="42142-106">An application domain enables application isolation by acting as a container for application state.</span></span> <span data-ttu-id="42142-107">Domény aplikace funguje jako kontejner a hranice pro typy definované v aplikaci a knihovny tříd, které používá.</span><span class="sxs-lookup"><span data-stu-id="42142-107">An application domain acts as a container and boundary for the types defined in the application and the class libraries it uses.</span></span> <span data-ttu-id="42142-108">Typy načtena do jedné domény aplikace se liší od stejného typu načtena do jiné domény aplikace a instance objektů nejsou sdíleny přímo mezi doménami aplikace.</span><span class="sxs-lookup"><span data-stu-id="42142-108">Types loaded into one application domain are distinct from the same type loaded into another application domain, and instances of objects are not directly shared between application domains.</span></span> <span data-ttu-id="42142-109">Například každá doména aplikace má vlastní kopii statické proměnné pro tyto typy a statický konstruktor pro typ je spustit maximálně jednou pro každé aplikační doméně.</span><span class="sxs-lookup"><span data-stu-id="42142-109">For instance, each application domain has its own copy of static variables for these types, and a static constructor for a type is run at most once per application domain.</span></span> <span data-ttu-id="42142-110">Implementace je zdarma k poskytování specifický pro implementaci zásad nebo mechanismy pro vytváření a ničení aplikačních domén.</span><span class="sxs-lookup"><span data-stu-id="42142-110">Implementations are free to provide implementation-specific policy or mechanisms for the creation and destruction of application domains.</span></span>

<span data-ttu-id="42142-111">***Spuštění aplikace*** nastane, pokud prostředí provádění volání určené metody, která se označuje jako vstupní bod aplikace.</span><span class="sxs-lookup"><span data-stu-id="42142-111">***Application startup*** occurs when the execution environment calls a designated method, which is referred to as the application's entry point.</span></span> <span data-ttu-id="42142-112">Tuto metodu vstupního bodu je vždy pojmenováno `Main`a může mít jednu z následujících podpisech:</span><span class="sxs-lookup"><span data-stu-id="42142-112">This entry point method is always named `Main`, and can have one of the following signatures:</span></span>

```csharp
static void Main() {...}

static void Main(string[] args) {...}

static int Main() {...}

static int Main(string[] args) {...}
```

<span data-ttu-id="42142-113">Jak je znázorněno, může volitelně vrátit vstupním bodem `int` hodnotu.</span><span class="sxs-lookup"><span data-stu-id="42142-113">As shown, the entry point may optionally return an `int` value.</span></span> <span data-ttu-id="42142-114">To vrátit hodnota se používá v ukončení aplikace ([ukončení aplikace](basic-concepts.md#application-termination)).</span><span class="sxs-lookup"><span data-stu-id="42142-114">This return value is used in application termination ([Application termination](basic-concepts.md#application-termination)).</span></span>

<span data-ttu-id="42142-115">Vstupní bod může mít jeden formální parametr.</span><span class="sxs-lookup"><span data-stu-id="42142-115">The entry point may optionally have one formal parameter.</span></span> <span data-ttu-id="42142-116">Tento parametr může mít libovolný název, ale musí být typu parametru `string[]`.</span><span class="sxs-lookup"><span data-stu-id="42142-116">The parameter may have any name, but the type of the parameter must be `string[]`.</span></span> <span data-ttu-id="42142-117">Pokud je k dispozici formální parametr, vytvoří prostředí pro spouštění a předá `string[]` argument obsahující argumenty příkazového řádku, které bylo zadané při spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="42142-117">If the formal parameter is present, the execution environment creates and passes a `string[]` argument containing the command-line arguments that were specified when the application was started.</span></span> <span data-ttu-id="42142-118">`string[]` Nikdy argument má hodnotu null, ale může mít nulovou délku, pokud nebyly zadány žádné argumenty příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="42142-118">The `string[]` argument is never null, but it may have a length of zero if no command-line arguments were specified.</span></span>

<span data-ttu-id="42142-119">Od C# podporuje metodu přetížení, třídy nebo struktury může obsahovat několik definic některé metody, pokud každý má jiný podpis.</span><span class="sxs-lookup"><span data-stu-id="42142-119">Since C# supports method overloading, a class or struct may contain multiple definitions of some method, provided each has a different signature.</span></span> <span data-ttu-id="42142-120">Ale v jediném programu, žádné třídy nebo struktury může obsahovat více než jednu metodu s názvem `Main` jehož definice kvalifikuje to má být použit jako vstupní bod aplikace.</span><span class="sxs-lookup"><span data-stu-id="42142-120">However, within a single program, no class or struct may contain more than one method called `Main` whose definition qualifies it to be used as an application entry point.</span></span> <span data-ttu-id="42142-121">Další přetížené verze `Main` jsou povolené, ale pokud mají více než jeden parametr nebo jejich jediným parametrem je jiné než typu `string[]`.</span><span class="sxs-lookup"><span data-stu-id="42142-121">Other overloaded versions of `Main` are permitted, however, provided they have more than one parameter, or their only parameter is other than type `string[]`.</span></span>

<span data-ttu-id="42142-122">Aplikace lze vytvořit více tříd nebo struktur.</span><span class="sxs-lookup"><span data-stu-id="42142-122">An application can be made up of multiple classes or structs.</span></span> <span data-ttu-id="42142-123">Je možné pro více než jeden z těchto tříd nebo struktur obsahuje metodu nazvanou `Main` jehož definice kvalifikuje to má být použit jako vstupní bod aplikace.</span><span class="sxs-lookup"><span data-stu-id="42142-123">It is possible for more than one of these classes or structs to contain a method called `Main` whose definition qualifies it to be used as an application entry point.</span></span> <span data-ttu-id="42142-124">V takových případech musíte použít externího mechanismu (jako je například možnost příkazového řádku kompilátoru) vyberte jednu z těchto `Main` metody jako vstupní bod.</span><span class="sxs-lookup"><span data-stu-id="42142-124">In such cases, an external mechanism (such as a command-line compiler option) must be used to select one of these `Main` methods as the entry point.</span></span>

<span data-ttu-id="42142-125">V jazyce C# každé metody musí být definován jako člen třídy nebo struktury.</span><span class="sxs-lookup"><span data-stu-id="42142-125">In C#, every method must be defined as a member of a class or struct.</span></span> <span data-ttu-id="42142-126">Obvykle je deklarovaná přístupnost ([deklarovaná přístupnost](basic-concepts.md#declared-accessibility)) metody je určeno modifikátory přístupu ([modifikátorů přístupu](classes.md#access-modifiers)) zadanou v jeho deklaraci a podobně deklarovaný Přístupnost typu se určuje podle přístupu modifikátory přístupu zadaná v jeho deklaraci.</span><span class="sxs-lookup"><span data-stu-id="42142-126">Ordinarily, the declared accessibility ([Declared accessibility](basic-concepts.md#declared-accessibility)) of a method is determined by the access modifiers ([Access modifiers](classes.md#access-modifiers)) specified in its declaration, and similarly the declared accessibility of a type is determined by the access modifiers specified in its declaration.</span></span> <span data-ttu-id="42142-127">Aby dané metody daného typu bude volat musí být typu a členu přístupné.</span><span class="sxs-lookup"><span data-stu-id="42142-127">In order for a given method of a given type to be callable, both the type and the member must be accessible.</span></span> <span data-ttu-id="42142-128">Vstupní bod aplikace, ale je zvláštní případ.</span><span class="sxs-lookup"><span data-stu-id="42142-128">However, the application entry point is a special case.</span></span> <span data-ttu-id="42142-129">Konkrétně spouštěcí prostředí mají přístup k vstupní bod aplikace bez ohledu na jeho deklarovaná přístupnost a bez ohledu na to je deklarovaná přístupnost její nadřazený typ deklarace.</span><span class="sxs-lookup"><span data-stu-id="42142-129">Specifically, the execution environment can access the application's entry point regardless of its declared accessibility and regardless of the declared accessibility of its enclosing type declarations.</span></span>

<span data-ttu-id="42142-130">Metodu vstupního bodu aplikace nemusí být v deklaraci obecné třídy.</span><span class="sxs-lookup"><span data-stu-id="42142-130">The application entry point method may not be in a generic class declaration.</span></span>

<span data-ttu-id="42142-131">V ostatních ohledech metody vstupního bodu chovají jako ty, které nejsou vstupní body.</span><span class="sxs-lookup"><span data-stu-id="42142-131">In all other respects, entry point methods behave like those that are not entry points.</span></span>

## <a name="application-termination"></a><span data-ttu-id="42142-132">Ukončení aplikace</span><span class="sxs-lookup"><span data-stu-id="42142-132">Application termination</span></span>

<span data-ttu-id="42142-133">***Ukončení aplikace*** vrátí řízení do prostředí pro spuštění.</span><span class="sxs-lookup"><span data-stu-id="42142-133">***Application termination*** returns control to the execution environment.</span></span>

<span data-ttu-id="42142-134">Pokud návratový typ aplikace ***vstupní bod*** je metoda `int`, vrácená hodnota slouží jako aplikace ***stavový kód ukončení***.</span><span class="sxs-lookup"><span data-stu-id="42142-134">If the return type of the application's ***entry point*** method is `int`, the value returned serves as the application's ***termination status code***.</span></span> <span data-ttu-id="42142-135">Účelem tohoto kódu je umožnit komunikaci úspěšné či neúspěšné spuštění prostředí.</span><span class="sxs-lookup"><span data-stu-id="42142-135">The purpose of this code is to allow communication of success or failure to the execution environment.</span></span>

<span data-ttu-id="42142-136">Pokud je návratový typ metodu vstupního bodu `void`, dosáhnout pravou složenou závorku (`}`), který ukončí metody nebo provádění `return` příkaz, který nemá žádný výraz výsledkem stavový kód ukončení `0`.</span><span class="sxs-lookup"><span data-stu-id="42142-136">If the return type of the entry point method is `void`, reaching the right brace (`}`) which terminates that method, or executing a `return` statement that has no expression, results in a termination status code of `0`.</span></span>

<span data-ttu-id="42142-137">Před ukončení aplikace, jsou volány destruktory všech objektů, které ještě nebyly vynuceno uvolnění paměti, pokud takové vyčistit došlo k potlačení (voláním metody knihovny `GC.SuppressFinalize`, například).</span><span class="sxs-lookup"><span data-stu-id="42142-137">Prior to an application's termination, destructors for all of its objects that have not yet been garbage collected are called, unless such cleanup has been suppressed (by a call to the library method `GC.SuppressFinalize`, for example).</span></span>

## <a name="declarations"></a><span data-ttu-id="42142-138">Deklarace</span><span class="sxs-lookup"><span data-stu-id="42142-138">Declarations</span></span>

<span data-ttu-id="42142-139">Deklarace v programu v jazyce C# definuje rozložení na základní prvky programu.</span><span class="sxs-lookup"><span data-stu-id="42142-139">Declarations in a C# program define the constituent elements of the program.</span></span> <span data-ttu-id="42142-140">Programy jazyka C# jsou uspořádané pomocí oborů názvů ([obory názvů](namespaces.md)), který může obsahovat typ deklarace a deklarace vnořené oboru názvů.</span><span class="sxs-lookup"><span data-stu-id="42142-140">C# programs are organized using namespaces ([Namespaces](namespaces.md)), which can contain type declarations and nested namespace declarations.</span></span> <span data-ttu-id="42142-141">Zadejte prohlášení ([typ deklarace](namespaces.md#type-declarations)) se používají k definování tříd ([třídy](classes.md)), struktur ([struktury](structs.md)), rozhraní ([rozhraní](interfaces.md) ), výčtů ([výčty](enums.md)) a delegáti ([delegáti](delegates.md)).</span><span class="sxs-lookup"><span data-stu-id="42142-141">Type declarations ([Type declarations](namespaces.md#type-declarations)) are used to define classes ([Classes](classes.md)), structs ([Structs](structs.md)), interfaces ([Interfaces](interfaces.md)), enums ([Enums](enums.md)), and delegates ([Delegates](delegates.md)).</span></span> <span data-ttu-id="42142-142">Seznam typů členů v deklaraci typu závisí na formuláři deklaraci typu.</span><span class="sxs-lookup"><span data-stu-id="42142-142">The kinds of members permitted in a type declaration depend on the form of the type declaration.</span></span> <span data-ttu-id="42142-143">Například, deklarace tříd může obsahovat deklarace konstanty ([konstanty](classes.md#constants)), pole ([pole](classes.md#fields)), metody ([metody](classes.md#methods)), vlastností ([ Vlastnosti](classes.md#properties)), události ([události](classes.md#events)), indexery ([indexery](classes.md#indexers)), operátory ([operátory](classes.md#operators)), konstruktory instancí ([ Konstruktory instancí](classes.md#instance-constructors)), statické konstruktory ([statické konstruktory](classes.md#static-constructors)), destruktory ([destruktory](classes.md#destructors)) a vnořené typy ([vnořené typy](classes.md#nested-types)).</span><span class="sxs-lookup"><span data-stu-id="42142-143">For instance, class declarations can contain declarations for constants ([Constants](classes.md#constants)), fields ([Fields](classes.md#fields)), methods ([Methods](classes.md#methods)), properties ([Properties](classes.md#properties)), events ([Events](classes.md#events)), indexers ([Indexers](classes.md#indexers)), operators ([Operators](classes.md#operators)), instance constructors ([Instance constructors](classes.md#instance-constructors)), static constructors ([Static constructors](classes.md#static-constructors)), destructors ([Destructors](classes.md#destructors)), and nested types ([Nested types](classes.md#nested-types)).</span></span>

<span data-ttu-id="42142-144">Definuje název v deklaraci ***místa deklarace*** , ke které patří deklarace.</span><span class="sxs-lookup"><span data-stu-id="42142-144">A declaration defines a name in the ***declaration space*** to which the declaration belongs.</span></span> <span data-ttu-id="42142-145">S výjimkou přetížení členů ([podpisy a přetížení](basic-concepts.md#signatures-and-overloading)), je chyba kompilace mít dva nebo více deklarací, které zavádějí členy se stejným názvem v prostoru deklarace.</span><span class="sxs-lookup"><span data-stu-id="42142-145">Except for overloaded members ([Signatures and overloading](basic-concepts.md#signatures-and-overloading)), it is a compile-time error to have two or more declarations that introduce members with the same name in a declaration space.</span></span> <span data-ttu-id="42142-146">Nikdy je možné místo deklarace obsahující různé druhy členů se stejným názvem.</span><span class="sxs-lookup"><span data-stu-id="42142-146">It is never possible for a declaration space to contain different kinds of members with the same name.</span></span> <span data-ttu-id="42142-147">Například místo deklarace můžete, nikdy neobsahuje pole a metody se stejným názvem.</span><span class="sxs-lookup"><span data-stu-id="42142-147">For example, a declaration space can never contain a field and a method by the same name.</span></span>

<span data-ttu-id="42142-148">Existuje několik různých typů prostorů prohlášení, jak je popsáno v následující.</span><span class="sxs-lookup"><span data-stu-id="42142-148">There are several different types of declaration spaces, as described in the following.</span></span>

*  <span data-ttu-id="42142-149">V rámci všech zdrojových souborů programu *namespace_member_declaration*s s uzavíracím žádné *namespace_declaration* členem jedné kombinované deklaraci místo volána ***globální deklarace místo***.</span><span class="sxs-lookup"><span data-stu-id="42142-149">Within all source files of a program, *namespace_member_declaration*s with no enclosing *namespace_declaration* are members of a single combined declaration space called the ***global declaration space***.</span></span>
*  <span data-ttu-id="42142-150">V rámci všech zdrojových souborů programu *namespace_member_declaration*s v rámci *namespace_declaration*, které mají stejný plně kvalifikovaný obor názvů jsou členy jedné kombinované deklaraci místo.</span><span class="sxs-lookup"><span data-stu-id="42142-150">Within all source files of a program, *namespace_member_declaration*s within *namespace_declaration*s that have the same fully qualified namespace name are members of a single combined declaration space.</span></span>
*  <span data-ttu-id="42142-151">Každou deklaraci rozhraní, třídy nebo struktury vytvoří nové prohlášení prostor.</span><span class="sxs-lookup"><span data-stu-id="42142-151">Each class, struct, or interface declaration creates a new declaration space.</span></span> <span data-ttu-id="42142-152">Názvy jsou zavedeny do tohoto prostoru deklarace prostřednictvím *class_member_declaration*s, *struct_member_declaration*s, *interface_member_declaration*s, nebo *type_parameter*s.</span><span class="sxs-lookup"><span data-stu-id="42142-152">Names are introduced into this declaration space through *class_member_declaration*s, *struct_member_declaration*s, *interface_member_declaration*s, or *type_parameter*s.</span></span> <span data-ttu-id="42142-153">S výjimkou konstruktor instance přetížené deklarace a statický konstruktor deklarace, třídy nebo struktury nemohou obsahovat deklarace člena se stejným názvem jako třída nebo struktura.</span><span class="sxs-lookup"><span data-stu-id="42142-153">Except for overloaded instance constructor declarations and static constructor declarations, a class or struct cannot contain a member declaration with the same name as the class or struct.</span></span> <span data-ttu-id="42142-154">Třídy, struktury nebo rozhraní umožňuje deklaraci přetížené metody, indexery.</span><span class="sxs-lookup"><span data-stu-id="42142-154">A class, struct, or interface permits the declaration of overloaded methods and indexers.</span></span> <span data-ttu-id="42142-155">Kromě toho třídě nebo struktuře umožňuje deklaraci instance přetížené konstruktory a operátory.</span><span class="sxs-lookup"><span data-stu-id="42142-155">Furthermore, a class or struct permits the declaration of overloaded instance constructors and operators.</span></span> <span data-ttu-id="42142-156">Například třídy, struktury nebo rozhraní může obsahovat více deklarace metod se stejným názvem, pokud tyto deklarace metody se liší v jejich podpisu ([podpisy a přetížení](basic-concepts.md#signatures-and-overloading)).</span><span class="sxs-lookup"><span data-stu-id="42142-156">For example, a class, struct, or interface may contain multiple method declarations with the same name, provided these method declarations differ in their signature ([Signatures and overloading](basic-concepts.md#signatures-and-overloading)).</span></span> <span data-ttu-id="42142-157">Všimněte si, že základní třídy se nepočítají do místa deklarace třídy a základních rozhraní nepočítají do místa deklarace rozhraní.</span><span class="sxs-lookup"><span data-stu-id="42142-157">Note that base classes do not contribute to the declaration space of a class, and base interfaces do not contribute to the declaration space of an interface.</span></span> <span data-ttu-id="42142-158">Proto odvozené třídy nebo rozhraní může deklarovat člena se stejným názvem jako zděděného člena.</span><span class="sxs-lookup"><span data-stu-id="42142-158">Thus, a derived class or interface is allowed to declare a member with the same name as an inherited member.</span></span> <span data-ttu-id="42142-159">Tento člen se říká, že ***skrýt*** zděděný člen.</span><span class="sxs-lookup"><span data-stu-id="42142-159">Such a member is said to ***hide*** the inherited member.</span></span>
*  <span data-ttu-id="42142-160">Každou deklaraci delegáta vytvoří nové prohlášení prostor.</span><span class="sxs-lookup"><span data-stu-id="42142-160">Each delegate declaration creates a new declaration space.</span></span> <span data-ttu-id="42142-161">Názvy jsou zavedeny do tohoto prostoru deklarace prostřednictvím formální parametry (*fixed_parameter*s a *parameter_array*s) a *type_parameter*s.</span><span class="sxs-lookup"><span data-stu-id="42142-161">Names are introduced into this declaration space through formal parameters (*fixed_parameter*s and *parameter_array*s) and *type_parameter*s.</span></span>
*  <span data-ttu-id="42142-162">Každou deklaraci výčtu vytvoří nové prohlášení prostor.</span><span class="sxs-lookup"><span data-stu-id="42142-162">Each enumeration declaration creates a new declaration space.</span></span> <span data-ttu-id="42142-163">Názvy jsou zavedeny do tohoto prostoru deklarace prostřednictvím *enum_member_declarations*.</span><span class="sxs-lookup"><span data-stu-id="42142-163">Names are introduced into this declaration space through *enum_member_declarations*.</span></span>
*  <span data-ttu-id="42142-164">Každou deklaraci metody, indexer deklarace, operátor, deklarace konstruktoru instance a anonymní funkce vytvoří nový prostor deklarace volá ***místa deklarace lokální proměnné***.</span><span class="sxs-lookup"><span data-stu-id="42142-164">Each method declaration, indexer declaration, operator declaration, instance constructor declaration and anonymous function creates a new declaration space called a ***local variable declaration space***.</span></span> <span data-ttu-id="42142-165">Názvy jsou zavedeny do tohoto prostoru deklarace prostřednictvím formální parametry (*fixed_parameter*s a *parameter_array*s) a *type_parameter*s.</span><span class="sxs-lookup"><span data-stu-id="42142-165">Names are introduced into this declaration space through formal parameters (*fixed_parameter*s and *parameter_array*s) and *type_parameter*s.</span></span> <span data-ttu-id="42142-166">Tělo funkce člena nebo anonymní funkce, pokud existuje, se považuje za být vnořen v rámci deklarace lokální proměnné místa.</span><span class="sxs-lookup"><span data-stu-id="42142-166">The body of the function member or anonymous function, if any, is considered to be nested within the local variable declaration space.</span></span> <span data-ttu-id="42142-167">Jedná se o chybu pro prostor deklarace lokální proměnné a prostor vnořené deklarace lokální proměnné tak, aby obsahovala elementy se stejným názvem.</span><span class="sxs-lookup"><span data-stu-id="42142-167">It is an error for a local variable declaration space and a nested local variable declaration space to contain elements with the same name.</span></span> <span data-ttu-id="42142-168">Díky tomu se v rámci oboru vnořené deklarace není možné k deklarování místní proměnné nebo konstantní se stejným názvem jako místní proměnná nebo konstantní v nadřazeném místa deklarace.</span><span class="sxs-lookup"><span data-stu-id="42142-168">Thus, within a nested declaration space it is not possible to declare a local variable or constant with the same name as a local variable or constant in an enclosing declaration space.</span></span> <span data-ttu-id="42142-169">Je možné pro dvě deklarace mezery obsahovat elementy se stejným názvem jako ani jedna deklarace prostor obsahuje druhý.</span><span class="sxs-lookup"><span data-stu-id="42142-169">It is possible for two declaration spaces to contain elements with the same name as long as neither declaration space contains the other.</span></span>
*  <span data-ttu-id="42142-170">Každý *bloku* nebo *switch_block* , a také *pro*, *foreach* a *pomocí* vytvoří příkaz, deklarace lokální proměnné prostoru pro místní proměnné a místní konstanty.</span><span class="sxs-lookup"><span data-stu-id="42142-170">Each *block* or *switch_block* , as well as a *for*, *foreach* and *using* statement, creates a local variable declaration space for local variables and local constants .</span></span> <span data-ttu-id="42142-171">Názvy jsou zavedeny do tohoto prostoru deklarace prostřednictvím *local_variable_declaration*s a *local_constant_declaration*s.</span><span class="sxs-lookup"><span data-stu-id="42142-171">Names are introduced into this declaration space through *local_variable_declaration*s and *local_constant_declaration*s.</span></span> <span data-ttu-id="42142-172">Všimněte si, že jsou vnořené bloky, ke kterým dochází jako nebo v rámci těla členské funkce nebo anonymní funkce v rámci deklarace lokální proměnné prostoru deklarované pomocí těchto funkcí pro své parametry.</span><span class="sxs-lookup"><span data-stu-id="42142-172">Note that blocks that occur as or within the body of a function member or anonymous function are nested within the local variable declaration space declared by those functions for their parameters.</span></span> <span data-ttu-id="42142-173">Proto se bude chyba, která máte třeba metodu s místní proměnné a parametr se stejným názvem.</span><span class="sxs-lookup"><span data-stu-id="42142-173">Thus it is an error to have e.g. a method with a local variable and a parameter of the same name.</span></span>
*  <span data-ttu-id="42142-174">Každý *bloku* nebo *switch_block* vytvoří samostatné prohlášení prostor pro popisky.</span><span class="sxs-lookup"><span data-stu-id="42142-174">Each *block* or *switch_block* creates a separate declaration space for labels.</span></span> <span data-ttu-id="42142-175">Názvy jsou zavedeny do tohoto prostoru deklarace prostřednictvím *labeled_statement*s a názvy jsou odkazovány prostřednictvím *goto_statement*s.</span><span class="sxs-lookup"><span data-stu-id="42142-175">Names are introduced into this declaration space through *labeled_statement*s, and the names are referenced through *goto_statement*s.</span></span> <span data-ttu-id="42142-176">***Popisek místa deklarace*** bloku zahrnuje jakékoli vnořené bloky.</span><span class="sxs-lookup"><span data-stu-id="42142-176">The ***label declaration space*** of a block includes any nested blocks.</span></span> <span data-ttu-id="42142-177">Proto ve vnořeném bloku není možné deklarovat popisek se stejným názvem jako popisek z vnějšího bloku.</span><span class="sxs-lookup"><span data-stu-id="42142-177">Thus, within a nested block it is not possible to declare a label with the same name as a label in an enclosing block.</span></span>

<span data-ttu-id="42142-178">Textové pořadí, ve kterém jsou deklarovány názvy je obecně žádný význam.</span><span class="sxs-lookup"><span data-stu-id="42142-178">The textual order in which names are declared is generally of no significance.</span></span> <span data-ttu-id="42142-179">Zejména textové pořadí není důležité pro deklaraci a použití oborů názvů, konstanty, metody, vlastnosti, události, indexery, operátory, konstruktory instancí, destruktory, statické konstruktory a typy.</span><span class="sxs-lookup"><span data-stu-id="42142-179">In particular, textual order is not significant for the declaration and use of namespaces, constants, methods, properties, events, indexers, operators, instance constructors, destructors, static constructors, and types.</span></span> <span data-ttu-id="42142-180">Deklarace záleží na pořadí následujícími způsoby:</span><span class="sxs-lookup"><span data-stu-id="42142-180">Declaration order is significant in the following ways:</span></span>

*  <span data-ttu-id="42142-181">Pořadí deklarace pro pole deklarace a místní deklarace proměnné určuje pořadí, ve kterém jsou prováděna jejich inicializátory (pokud existuje).</span><span class="sxs-lookup"><span data-stu-id="42142-181">Declaration order for field declarations and local variable declarations determines the order in which their initializers (if any) are executed.</span></span>
*  <span data-ttu-id="42142-182">Lokální proměnné musí být definovány před jejich použitím ([obory](basic-concepts.md#scopes)).</span><span class="sxs-lookup"><span data-stu-id="42142-182">Local variables must be defined before they are used ([Scopes](basic-concepts.md#scopes)).</span></span>
*  <span data-ttu-id="42142-183">Deklarace pořadí deklarací členů výčtu ([členy výčtu](enums.md#enum-members)) je důležité, když *constant_expression* hodnoty jsou vynechány.</span><span class="sxs-lookup"><span data-stu-id="42142-183">Declaration order for enum member declarations ([Enum members](enums.md#enum-members)) is significant when *constant_expression* values are omitted.</span></span>

<span data-ttu-id="42142-184">Místo deklarace oboru názvů je "Otevřít zakončeno" a dvěma obor názvů, které deklarace se stejným plně kvalifikovaným názvem přispívat na stejné místo prohlášení.</span><span class="sxs-lookup"><span data-stu-id="42142-184">The declaration space of a namespace is "open ended", and two namespace declarations with the same fully qualified name contribute to the same declaration space.</span></span> <span data-ttu-id="42142-185">Příklad</span><span class="sxs-lookup"><span data-stu-id="42142-185">For example</span></span>
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

<span data-ttu-id="42142-186">výše uvedené dvě deklarace oboru názvů přispívat na stejné místo prohlášení, v tomto případě deklarovat dvě třídy s plně kvalifikované názvy `Megacorp.Data.Customer` a `Megacorp.Data.Order`.</span><span class="sxs-lookup"><span data-stu-id="42142-186">The two namespace declarations above contribute to the same declaration space, in this case declaring two classes with the fully qualified names `Megacorp.Data.Customer` and `Megacorp.Data.Order`.</span></span> <span data-ttu-id="42142-187">Vzhledem k tomu, že dvě deklarace přispívat na stejné místo prohlášení, to by způsobila Chyba kompilace pokud každá obsahovala deklaraci třídy se stejným názvem.</span><span class="sxs-lookup"><span data-stu-id="42142-187">Because the two declarations contribute to the same declaration space, it would have caused a compile-time error if each contained a declaration of a class with the same name.</span></span>

<span data-ttu-id="42142-188">Jak je uvedeno výše místo prohlášení bloku zahrnuje jakékoli vnořené bloky.</span><span class="sxs-lookup"><span data-stu-id="42142-188">As specified above, the declaration space of a block includes any nested blocks.</span></span> <span data-ttu-id="42142-189">Díky tomu se v následujícím příkladu `F` a `G` metody způsobit chybu kompilace, protože název `i` je deklarovaný ve vnějším bloku a nejde deklarovat ve vnitřním bloku.</span><span class="sxs-lookup"><span data-stu-id="42142-189">Thus, in the following example, the `F` and `G` methods result in a compile-time error because the name `i` is declared in the outer block and cannot be redeclared in the inner block.</span></span> <span data-ttu-id="42142-190">Ale `H` a `I` metod jsou platné od obou `i`společnosti jsou deklarovány v samostatných blocích-nested.</span><span class="sxs-lookup"><span data-stu-id="42142-190">However, the `H` and `I` methods are valid since the two `i`'s are declared in separate non-nested blocks.</span></span>

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

## <a name="members"></a><span data-ttu-id="42142-191">Členové</span><span class="sxs-lookup"><span data-stu-id="42142-191">Members</span></span>

<span data-ttu-id="42142-192">Obory názvů a typy mají ***členy***.</span><span class="sxs-lookup"><span data-stu-id="42142-192">Namespaces and types have ***members***.</span></span> <span data-ttu-id="42142-193">Členy entity jsou obecně dostupné prostřednictvím úplný název, který začíná na odkaz na entitu, za nímž následuje "`.`" token, následovaný název člena.</span><span class="sxs-lookup"><span data-stu-id="42142-193">The members of an entity are generally available through the use of a qualified name that starts with a reference to the entity, followed by a "`.`" token, followed by the name of the member.</span></span>

<span data-ttu-id="42142-194">Členy typu jsou buď deklarovány v deklaraci typu nebo ***zděděné*** ze základní třídy typu.</span><span class="sxs-lookup"><span data-stu-id="42142-194">Members of a type are either declared in the type declaration or ***inherited*** from the base class of the type.</span></span> <span data-ttu-id="42142-195">Pokud typ dědí ze základní třídy, všechny členy základní třídy, s výjimkou instance konstruktory, destruktory a statické konstruktory stanou členy odvozeného typu.</span><span class="sxs-lookup"><span data-stu-id="42142-195">When a type inherits from a base class, all members of the base class, except instance constructors, destructors and static constructors, become members of the derived type.</span></span> <span data-ttu-id="42142-196">Je deklarovaná přístupnost člena základní třídy neřídí, zda člen zděděný – dědičnosti rozšiřuje do jakéhokoli členu, který není konstruktor instance, statický konstruktor nebo destruktor.</span><span class="sxs-lookup"><span data-stu-id="42142-196">The declared accessibility of a base class member does not control whether the member is inherited—inheritance extends to any member that isn't an instance constructor, static constructor, or destructor.</span></span> <span data-ttu-id="42142-197">Ale nemusí být přístupné v odvozeném typu, buď z důvodu jeho deklarovaná přístupnost zděděného člena ([deklarovaná přístupnost](basic-concepts.md#declared-accessibility)) nebo protože je skrytá deklarací v samotném typu ([skrytí prostřednictvím dědičnost](basic-concepts.md#hiding-through-inheritance)).</span><span class="sxs-lookup"><span data-stu-id="42142-197">However, an inherited member may not be accessible in a derived type, either because of its declared accessibility ([Declared accessibility](basic-concepts.md#declared-accessibility)) or because it is hidden by a declaration in the type itself ([Hiding through inheritance](basic-concepts.md#hiding-through-inheritance)).</span></span>

### <a name="namespace-members"></a><span data-ttu-id="42142-198">Namespace členy</span><span class="sxs-lookup"><span data-stu-id="42142-198">Namespace members</span></span>

<span data-ttu-id="42142-199">Obory názvů a typy, které mají žádný nadřazený obor názvů jsou členy ***globální obor názvů***.</span><span class="sxs-lookup"><span data-stu-id="42142-199">Namespaces and types that have no enclosing namespace are members of the ***global namespace***.</span></span> <span data-ttu-id="42142-200">To odpovídá přímo názvy deklarované v deklaraci globálního místa.</span><span class="sxs-lookup"><span data-stu-id="42142-200">This corresponds directly to the names declared in the global declaration space.</span></span>

<span data-ttu-id="42142-201">Obory názvů a typy deklarované v rámci oboru názvů jsou členy tohoto oboru názvů.</span><span class="sxs-lookup"><span data-stu-id="42142-201">Namespaces and types declared within a namespace are members of that namespace.</span></span> <span data-ttu-id="42142-202">To odpovídá přímo názvy deklarované v prostoru deklarace oboru názvů.</span><span class="sxs-lookup"><span data-stu-id="42142-202">This corresponds directly to the names declared in the declaration space of the namespace.</span></span>

<span data-ttu-id="42142-203">Obory názvů nemá žádné omezení přístupu.</span><span class="sxs-lookup"><span data-stu-id="42142-203">Namespaces have no access restrictions.</span></span> <span data-ttu-id="42142-204">Není možné deklarovat privátní, chráněné nebo interní oborů názvů a názvy oborů názvů jsou vždy veřejně přístupná.</span><span class="sxs-lookup"><span data-stu-id="42142-204">It is not possible to declare private, protected, or internal namespaces, and namespace names are always publicly accessible.</span></span>

### <a name="struct-members"></a><span data-ttu-id="42142-205">Členy struktury</span><span class="sxs-lookup"><span data-stu-id="42142-205">Struct members</span></span>

<span data-ttu-id="42142-206">Členy struktury jsou členy deklarované ve struktuře a členy zděděné od přímé základní třídy struktura, obsahovat `System.ValueType` a nepřímá základní třída `object`.</span><span class="sxs-lookup"><span data-stu-id="42142-206">The members of a struct are the members declared in the struct and the members inherited from the struct's direct base class `System.ValueType` and the indirect base class `object`.</span></span>

<span data-ttu-id="42142-207">Členové jednoduchý typ. odpovídají přímo na členy struktury alias typu pomocí jednoduchého typu:</span><span class="sxs-lookup"><span data-stu-id="42142-207">The members of a simple type correspond directly to the members of the struct type aliased by the simple type:</span></span>

*  <span data-ttu-id="42142-208">Členové `sbyte` jsou členy `System.SByte` struktury.</span><span class="sxs-lookup"><span data-stu-id="42142-208">The members of `sbyte` are the members of the `System.SByte` struct.</span></span>
*  <span data-ttu-id="42142-209">Členové `byte` jsou členy `System.Byte` struktury.</span><span class="sxs-lookup"><span data-stu-id="42142-209">The members of `byte` are the members of the `System.Byte` struct.</span></span>
*  <span data-ttu-id="42142-210">Členové `short` jsou členy `System.Int16` struktury.</span><span class="sxs-lookup"><span data-stu-id="42142-210">The members of `short` are the members of the `System.Int16` struct.</span></span>
*  <span data-ttu-id="42142-211">Členové `ushort` jsou členy `System.UInt16` struktury.</span><span class="sxs-lookup"><span data-stu-id="42142-211">The members of `ushort` are the members of the `System.UInt16` struct.</span></span>
*  <span data-ttu-id="42142-212">Členové `int` jsou členy `System.Int32` struktury.</span><span class="sxs-lookup"><span data-stu-id="42142-212">The members of `int` are the members of the `System.Int32` struct.</span></span>
*  <span data-ttu-id="42142-213">Členové `uint` jsou členy `System.UInt32` struktury.</span><span class="sxs-lookup"><span data-stu-id="42142-213">The members of `uint` are the members of the `System.UInt32` struct.</span></span>
*  <span data-ttu-id="42142-214">Členové `long` jsou členy `System.Int64` struktury.</span><span class="sxs-lookup"><span data-stu-id="42142-214">The members of `long` are the members of the `System.Int64` struct.</span></span>
*  <span data-ttu-id="42142-215">Členové `ulong` jsou členy `System.UInt64` struktury.</span><span class="sxs-lookup"><span data-stu-id="42142-215">The members of `ulong` are the members of the `System.UInt64` struct.</span></span>
*  <span data-ttu-id="42142-216">Členové `char` jsou členy `System.Char` struktury.</span><span class="sxs-lookup"><span data-stu-id="42142-216">The members of `char` are the members of the `System.Char` struct.</span></span>
*  <span data-ttu-id="42142-217">Členové `float` jsou členy `System.Single` struktury.</span><span class="sxs-lookup"><span data-stu-id="42142-217">The members of `float` are the members of the `System.Single` struct.</span></span>
*  <span data-ttu-id="42142-218">Členové `double` jsou členy `System.Double` struktury.</span><span class="sxs-lookup"><span data-stu-id="42142-218">The members of `double` are the members of the `System.Double` struct.</span></span>
*  <span data-ttu-id="42142-219">Členové `decimal` jsou členy `System.Decimal` struktury.</span><span class="sxs-lookup"><span data-stu-id="42142-219">The members of `decimal` are the members of the `System.Decimal` struct.</span></span>
*  <span data-ttu-id="42142-220">Členové `bool` jsou členy `System.Boolean` struktury.</span><span class="sxs-lookup"><span data-stu-id="42142-220">The members of `bool` are the members of the `System.Boolean` struct.</span></span>

### <a name="enumeration-members"></a><span data-ttu-id="42142-221">Členy výčtu</span><span class="sxs-lookup"><span data-stu-id="42142-221">Enumeration members</span></span>

<span data-ttu-id="42142-222">Členy výčtu jsou deklarovány ve výčtu konstant a členy zděděné od přímé základní třídy výčtu `System.Enum` a nepřímé třídy base `System.ValueType` a `object`.</span><span class="sxs-lookup"><span data-stu-id="42142-222">The members of an enumeration are the constants declared in the enumeration and the members inherited from the enumeration's direct base class `System.Enum` and the indirect base classes `System.ValueType` and `object`.</span></span>

### <a name="class-members"></a><span data-ttu-id="42142-223">Členy třídy</span><span class="sxs-lookup"><span data-stu-id="42142-223">Class members</span></span>

<span data-ttu-id="42142-224">Členy třídy jsou členy deklarované ve třídě a členy zděděné ze základní třídy (s výjimkou třídy `object` který nemá žádnou základní třídu).</span><span class="sxs-lookup"><span data-stu-id="42142-224">The members of a class are the members declared in the class and the members inherited from the base class (except for class `object` which has no base class).</span></span> <span data-ttu-id="42142-225">Členy zděděné ze základní třídy zahrnují konstanty, pole, metody, vlastnosti, události, indexery, operátory a typy základní třídy, ale není instance konstruktory, destruktory a statické konstruktory základní třídy.</span><span class="sxs-lookup"><span data-stu-id="42142-225">The members inherited from the base class include the constants, fields, methods, properties, events, indexers, operators, and types of the base class, but not the instance constructors, destructors and static constructors of the base class.</span></span> <span data-ttu-id="42142-226">Bez ohledu na jejich přístupnost jsou zděděné členy základní třídy.</span><span class="sxs-lookup"><span data-stu-id="42142-226">Base class members are inherited without regard to their accessibility.</span></span>

<span data-ttu-id="42142-227">Deklarace třídy mohou obsahovat deklarace konstanty, pole, metody, vlastnosti, události, indexery, operátory, konstruktory instancí, destruktory, statické konstruktory a typy.</span><span class="sxs-lookup"><span data-stu-id="42142-227">A class declaration may contain declarations of constants, fields, methods, properties, events, indexers, operators, instance constructors, destructors, static constructors and types.</span></span>

<span data-ttu-id="42142-228">Členové `object` a `string` odpovídají přímo členy typů tříd, alias:</span><span class="sxs-lookup"><span data-stu-id="42142-228">The members of `object` and `string` correspond directly to the members of the class types they alias:</span></span>

*  <span data-ttu-id="42142-229">Členové `object` jsou členy `System.Object` třídy.</span><span class="sxs-lookup"><span data-stu-id="42142-229">The members of `object` are the members of the `System.Object` class.</span></span>
*  <span data-ttu-id="42142-230">Členové `string` jsou členy `System.String` třídy.</span><span class="sxs-lookup"><span data-stu-id="42142-230">The members of `string` are the members of the `System.String` class.</span></span>

### <a name="interface-members"></a><span data-ttu-id="42142-231">Členy rozhraní</span><span class="sxs-lookup"><span data-stu-id="42142-231">Interface members</span></span>

<span data-ttu-id="42142-232">Členy rozhraní jsou členy deklarované v rozhraní a ve všech základních rozhraní rozhraní.</span><span class="sxs-lookup"><span data-stu-id="42142-232">The members of an interface are the members declared in the interface and in all base interfaces of the interface.</span></span> <span data-ttu-id="42142-233">Členy ve třídě `object` nejsou, přísně vzato členy libovolné rozhraní ([členy rozhraní](interfaces.md#interface-members)).</span><span class="sxs-lookup"><span data-stu-id="42142-233">The members in class `object` are not, strictly speaking, members of any interface ([Interface members](interfaces.md#interface-members)).</span></span> <span data-ttu-id="42142-234">Ale členy ve třídě `object` jsou k dispozici prostřednictvím člen vyhledávání v libovolný typ rozhraní ([člen vyhledávání](expressions.md#member-lookup)).</span><span class="sxs-lookup"><span data-stu-id="42142-234">However, the members in class `object` are available via member lookup in any interface type ([Member lookup](expressions.md#member-lookup)).</span></span>

### <a name="array-members"></a><span data-ttu-id="42142-235">Členy pole</span><span class="sxs-lookup"><span data-stu-id="42142-235">Array members</span></span>

<span data-ttu-id="42142-236">Členy pole jsou členy zděděné z třídy `System.Array`.</span><span class="sxs-lookup"><span data-stu-id="42142-236">The members of an array are the members inherited from class `System.Array`.</span></span>

### <a name="delegate-members"></a><span data-ttu-id="42142-237">Delegát členy</span><span class="sxs-lookup"><span data-stu-id="42142-237">Delegate members</span></span>

<span data-ttu-id="42142-238">Členové delegáta jsou členy zděděné z třídy `System.Delegate`.</span><span class="sxs-lookup"><span data-stu-id="42142-238">The members of a delegate are the members inherited from class `System.Delegate`.</span></span>

## <a name="member-access"></a><span data-ttu-id="42142-239">Přístup ke členům</span><span class="sxs-lookup"><span data-stu-id="42142-239">Member access</span></span>

<span data-ttu-id="42142-240">Deklarace členů umožňují řídit přístup ke členu.</span><span class="sxs-lookup"><span data-stu-id="42142-240">Declarations of members allow control over member access.</span></span> <span data-ttu-id="42142-241">Usnadnění přístupu člena je vytvořeno je deklarovaná přístupnost ([deklarovaná přístupnost](basic-concepts.md#declared-accessibility)) člena kombinovat s přístupnosti bezprostředně nadřazeného typu, pokud existuje.</span><span class="sxs-lookup"><span data-stu-id="42142-241">The accessibility of a member is established by the declared accessibility ([Declared accessibility](basic-concepts.md#declared-accessibility)) of the member combined with the accessibility of the immediately containing type, if any.</span></span>

<span data-ttu-id="42142-242">Když je povolen přístup k určitým členem, člen se říká, že ***přístupné***.</span><span class="sxs-lookup"><span data-stu-id="42142-242">When access to a particular member is allowed, the member is said to be ***accessible***.</span></span> <span data-ttu-id="42142-243">Naopak při přístupu k určitým členem je zakázané, člen se říká, že ***nepřístupný***.</span><span class="sxs-lookup"><span data-stu-id="42142-243">Conversely, when access to a particular member is disallowed, the member is said to be ***inaccessible***.</span></span> <span data-ttu-id="42142-244">Je povolen přístup ke členovi textové umístění, ve kterém probíhá přístup je obsažen v tak doména přístupnosti ([usnadnění domén](basic-concepts.md#accessibility-domains)) člena.</span><span class="sxs-lookup"><span data-stu-id="42142-244">Access to a member is permitted when the textual location in which the access takes place is included in the accessibility domain ([Accessibility domains](basic-concepts.md#accessibility-domains)) of the member.</span></span>

### <a name="declared-accessibility"></a><span data-ttu-id="42142-245">Deklarovaná přístupnost</span><span class="sxs-lookup"><span data-stu-id="42142-245">Declared accessibility</span></span>

<span data-ttu-id="42142-246">***Deklarovaná přístupnost*** člena může být jedna z následujících akcí:</span><span class="sxs-lookup"><span data-stu-id="42142-246">The ***declared accessibility*** of a member can be one of the following:</span></span>

*  <span data-ttu-id="42142-247">Public, který je vybrán zahrnutím `public` modifikátor v deklaraci člena.</span><span class="sxs-lookup"><span data-stu-id="42142-247">Public, which is selected by including a `public` modifier in the member declaration.</span></span> <span data-ttu-id="42142-248">Intuitivní význam `public` je "přístup mimo jiné".</span><span class="sxs-lookup"><span data-stu-id="42142-248">The intuitive meaning of `public` is "access not limited".</span></span>
*  <span data-ttu-id="42142-249">Chráněný, což je vybraná zahrnutím `protected` modifikátor v deklaraci člena.</span><span class="sxs-lookup"><span data-stu-id="42142-249">Protected, which is selected by including a `protected` modifier in the member declaration.</span></span> <span data-ttu-id="42142-250">Intuitivní význam `protected` je "přístup omezen na obsahující třídu nebo typy odvozené ze třídy obsahující".</span><span class="sxs-lookup"><span data-stu-id="42142-250">The intuitive meaning of `protected` is "access limited to the containing class or types derived from the containing class".</span></span>
*  <span data-ttu-id="42142-251">Interní, který je vybrán zahrnutím `internal` modifikátor v deklaraci člena.</span><span class="sxs-lookup"><span data-stu-id="42142-251">Internal, which is selected by including an `internal` modifier in the member declaration.</span></span> <span data-ttu-id="42142-252">Intuitivní význam `internal` je "přístup omezen na tento program".</span><span class="sxs-lookup"><span data-stu-id="42142-252">The intuitive meaning of `internal` is "access limited to this program".</span></span>
*  <span data-ttu-id="42142-253">Chráněné vnitřní (tj. chráněné nebo interní), která se zvolila včetně `protected` a `internal` modifikátor v deklaraci člena.</span><span class="sxs-lookup"><span data-stu-id="42142-253">Protected internal (meaning protected or internal), which is selected by including both a `protected` and an `internal` modifier in the member declaration.</span></span> <span data-ttu-id="42142-254">Intuitivní význam `protected internal` "přístup omezen na tento program nebo typy odvozené od třídy obsahující".</span><span class="sxs-lookup"><span data-stu-id="42142-254">The intuitive meaning of `protected internal` is "access limited to this program or types derived from the containing class".</span></span>
*  <span data-ttu-id="42142-255">Privátní, který je vybrán zahrnutím `private` modifikátor v deklaraci člena.</span><span class="sxs-lookup"><span data-stu-id="42142-255">Private, which is selected by including a `private` modifier in the member declaration.</span></span> <span data-ttu-id="42142-256">Intuitivní význam `private` je "přístup omezen na obsahující typ".</span><span class="sxs-lookup"><span data-stu-id="42142-256">The intuitive meaning of `private` is "access limited to the containing type".</span></span>

<span data-ttu-id="42142-257">V závislosti na kontextu, ve kterém přijímá deklarace člena umístíte, jsou povoleny pouze určité typy deklarovaná přístupnost.</span><span class="sxs-lookup"><span data-stu-id="42142-257">Depending on the context in which a member declaration takes place, only certain types of declared accessibility are permitted.</span></span> <span data-ttu-id="42142-258">Kromě toho při deklaraci členské neobsahuje žádné modifikátory přístupu, kontext, ve kterém probíhá deklarace Určuje výchozí deklarovaná přístupnost.</span><span class="sxs-lookup"><span data-stu-id="42142-258">Furthermore, when a member declaration does not include any access modifiers, the context in which the declaration takes place determines the default declared accessibility.</span></span>

*  <span data-ttu-id="42142-259">Obory názvů mají implicitně `public` deklarovaná přístupnost.</span><span class="sxs-lookup"><span data-stu-id="42142-259">Namespaces implicitly have `public` declared accessibility.</span></span> <span data-ttu-id="42142-260">Deklarace oboru názvů jsou povoleny žádné modifikátory přístupu.</span><span class="sxs-lookup"><span data-stu-id="42142-260">No access modifiers are allowed on namespace declarations.</span></span>
*  <span data-ttu-id="42142-261">Typy deklarované v jednotkách kompilace nebo obory názvů můžou mít `public` nebo `internal` deklarované usnadnění přístupu a výchozí `internal` deklarovaná přístupnost.</span><span class="sxs-lookup"><span data-stu-id="42142-261">Types declared in compilation units or namespaces can have `public` or `internal` declared accessibility and default to `internal` declared accessibility.</span></span>
*  <span data-ttu-id="42142-262">Členy třídy lze mít pět typů deklarovaná přístupnost a ve výchozím nastavení `private` deklarovaná přístupnost.</span><span class="sxs-lookup"><span data-stu-id="42142-262">Class members can have any of the five kinds of declared accessibility and default to `private` declared accessibility.</span></span> <span data-ttu-id="42142-263">(Všimněte si, že typ deklarované jako člen třídy, může mít některý z pěti typů deklarovaná přístupnost, že typ deklarovaný s členem oboru názvů můžou mít jenom `public` nebo `internal` deklarovaná přístupnost.)</span><span class="sxs-lookup"><span data-stu-id="42142-263">(Note that a type declared as a member of a class can have any of the five kinds of declared accessibility, whereas a type declared as a member of a namespace can have only `public` or `internal` declared accessibility.)</span></span>
*  <span data-ttu-id="42142-264">Členy struktury mohou mít `public`, `internal`, nebo `private` deklarované usnadnění přístupu a výchozí `private` deklaraci přístupnost, protože jsou implicitně zapečetěné struktury.</span><span class="sxs-lookup"><span data-stu-id="42142-264">Struct members can have `public`, `internal`, or `private` declared accessibility and default to `private` declared accessibility because structs are implicitly sealed.</span></span> <span data-ttu-id="42142-265">Členy struktury zavedený – struktura (který je, nejsou děděna této struktury) nemůže mít `protected` nebo `protected internal` deklarovaná přístupnost.</span><span class="sxs-lookup"><span data-stu-id="42142-265">Struct members introduced in a struct (that is, not inherited by that struct) cannot have `protected` or `protected internal` declared accessibility.</span></span> <span data-ttu-id="42142-266">(Všimněte si, že typ deklarovaný jako člen struktury mohou mít `public`, `internal`, nebo `private` deklarovaná přístupnost, že typ deklarovaný s členem oboru názvů můžou mít jenom `public` nebo `internal` deklarovaná přístupnost.)</span><span class="sxs-lookup"><span data-stu-id="42142-266">(Note that a type declared as a member of a struct can have `public`, `internal`, or `private` declared accessibility, whereas a type declared as a member of a namespace can have only `public` or `internal` declared accessibility.)</span></span>
*  <span data-ttu-id="42142-267">Členy rozhraní mají implicitně `public` deklarovaná přístupnost.</span><span class="sxs-lookup"><span data-stu-id="42142-267">Interface members implicitly have `public` declared accessibility.</span></span> <span data-ttu-id="42142-268">Deklarace člena rozhraní jsou povoleny žádné modifikátory přístupu.</span><span class="sxs-lookup"><span data-stu-id="42142-268">No access modifiers are allowed on interface member declarations.</span></span>
*  <span data-ttu-id="42142-269">Členy výčtu mají implicitně `public` deklarovaná přístupnost.</span><span class="sxs-lookup"><span data-stu-id="42142-269">Enumeration members implicitly have `public` declared accessibility.</span></span> <span data-ttu-id="42142-270">U člena deklarací výčtu jsou povoleny žádné modifikátory přístupu.</span><span class="sxs-lookup"><span data-stu-id="42142-270">No access modifiers are allowed on enumeration member declarations.</span></span>

### <a name="accessibility-domains"></a><span data-ttu-id="42142-271">Usnadnění domén</span><span class="sxs-lookup"><span data-stu-id="42142-271">Accessibility domains</span></span>

<span data-ttu-id="42142-272">***Doména přístupnosti*** člena se skládá z (pravděpodobně nesouvislý) části textu programu, ve kterém je povolen přístup ke členu.</span><span class="sxs-lookup"><span data-stu-id="42142-272">The ***accessibility domain*** of a member consists of the (possibly disjoint) sections of program text in which access to the member is permitted.</span></span> <span data-ttu-id="42142-273">Pro účely definování tak doména přístupnosti člena, je označen jako člen ***nejvyšší úrovně*** Pokud není deklarován v rámci typu, a je označen jako člen ***vnořené*** případě, že je deklarována v rámci jiného typu.</span><span class="sxs-lookup"><span data-stu-id="42142-273">For purposes of defining the accessibility domain of a member, a member is said to be ***top-level*** if it is not declared within a type, and a member is said to be ***nested*** if it is declared within another type.</span></span> <span data-ttu-id="42142-274">Kromě toho ***programu text*** programu je definován tak, že všechny programu text obsažen ve všech zdrojových souborech programu a textem programu typ je definován tak, že všechny obsažené v textu programu *type_declaration*s tohoto typu (včetně, případně také typy, které jsou vnořené v rámci typu).</span><span class="sxs-lookup"><span data-stu-id="42142-274">Furthermore, the ***program text*** of a program is defined as all program text contained in all source files of the program, and the program text of a type is defined as all program text contained in the *type_declaration*s of that type (including, possibly, types that are nested within the type).</span></span>

<span data-ttu-id="42142-275">Doména přístupnosti člena předdefinovaný typ (například `object`, `int`, nebo `double`) neomezený.</span><span class="sxs-lookup"><span data-stu-id="42142-275">The accessibility domain of a predefined type (such as `object`, `int`, or `double`) is unlimited.</span></span>

<span data-ttu-id="42142-276">Doména přístupnosti člena na nejvyšší úrovni nevázaný typ `T` ([vázaný a nevázaných typy](types.md#bound-and-unbound-types)), který je deklarován v programu `P` je definovaná následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="42142-276">The accessibility domain of a top-level unbound type `T` ([Bound and unbound types](types.md#bound-and-unbound-types)) that is declared in a program `P` is defined as follows:</span></span>

*  <span data-ttu-id="42142-277">Pokud je deklarovaná přístupnost člena `T` je `public`, tak doména přístupnosti člena `T` je text programu `P` a libovolného programu, který odkazuje na `P`.</span><span class="sxs-lookup"><span data-stu-id="42142-277">If the declared accessibility of `T` is `public`, the accessibility domain of `T` is the program text of `P` and any program that references `P`.</span></span>
*  <span data-ttu-id="42142-278">Pokud je deklarovaná přístupnost člena `T` je `internal`, tak doména přístupnosti člena `T` je text programu `P`.</span><span class="sxs-lookup"><span data-stu-id="42142-278">If the declared accessibility of `T` is `internal`, the accessibility domain of `T` is the program text of `P`.</span></span>

<span data-ttu-id="42142-279">Z těchto definic následuje, tak doména přístupnosti člena nejvyšší úrovně nevázaný typ je vždy alespoň textem programu program, který typ je deklarován.</span><span class="sxs-lookup"><span data-stu-id="42142-279">From these definitions it follows that the accessibility domain of a top-level unbound type is always at least the program text of the program in which that type is declared.</span></span>

<span data-ttu-id="42142-280">Doména přístupnosti pro konstruovaný typ. `T<A1, ..., An>` je průsečík domény přístupnosti typu nevázaný parametr generického typu `T` a usnadnění domény argumentů typu `A1, ..., An`.</span><span class="sxs-lookup"><span data-stu-id="42142-280">The accessibility domain for a constructed type `T<A1, ..., An>` is the intersection of the accessibility domain of the unbound generic type `T` and the accessibility domains of the type arguments `A1, ..., An`.</span></span>

<span data-ttu-id="42142-281">Doména přístupnosti vnořeného člena `M` deklarovaného v typu `T` v rámci programu `P` je definována takto (konstatujme, že `M` pravděpodobně sám může být typ):</span><span class="sxs-lookup"><span data-stu-id="42142-281">The accessibility domain of a nested member `M` declared in a type `T` within a program `P` is defined as follows (noting that `M` itself may possibly be a type):</span></span>

*  <span data-ttu-id="42142-282">Pokud je deklarovaná přístupnost člena `M` je `public`, tak doména přístupnosti člena `M` je doména přístupnosti člena `T`.</span><span class="sxs-lookup"><span data-stu-id="42142-282">If the declared accessibility of `M` is `public`, the accessibility domain of `M` is the accessibility domain of `T`.</span></span>
*  <span data-ttu-id="42142-283">Pokud je deklarovaná přístupnost člena `M` je `protected internal`, umožněte `D` být sjednocení textem programu `P` a textem programu libovolného typu odvozené z `T`, který je deklarován mimo `P`.</span><span class="sxs-lookup"><span data-stu-id="42142-283">If the declared accessibility of `M` is `protected internal`, let `D` be the union of the program text of `P` and the program text of any type derived from `T`, which is declared outside `P`.</span></span> <span data-ttu-id="42142-284">Doména přístupnosti člena `M` je průsečík domény přístupnosti typu `T` s `D`.</span><span class="sxs-lookup"><span data-stu-id="42142-284">The accessibility domain of `M` is the intersection of the accessibility domain of `T` with `D`.</span></span>
*  <span data-ttu-id="42142-285">Pokud je deklarovaná přístupnost člena `M` je `protected`, umožněte `D` být sjednocení textem programu `T` a textem programu libovolného typu odvozené z `T`.</span><span class="sxs-lookup"><span data-stu-id="42142-285">If the declared accessibility of `M` is `protected`, let `D` be the union of the program text of `T` and the program text of any type derived from `T`.</span></span> <span data-ttu-id="42142-286">Doména přístupnosti člena `M` je průsečík domény přístupnosti typu `T` s `D`.</span><span class="sxs-lookup"><span data-stu-id="42142-286">The accessibility domain of `M` is the intersection of the accessibility domain of `T` with `D`.</span></span>
*  <span data-ttu-id="42142-287">Pokud je deklarovaná přístupnost člena `M` je `internal`, tak doména přístupnosti člena `M` je průsečík domény přístupnosti typu `T` s textem programu `P`.</span><span class="sxs-lookup"><span data-stu-id="42142-287">If the declared accessibility of `M` is `internal`, the accessibility domain of `M` is the intersection of the accessibility domain of `T` with the program text of `P`.</span></span>
*  <span data-ttu-id="42142-288">Pokud je deklarovaná přístupnost člena `M` je `private`, tak doména přístupnosti člena `M` je text programu `T`.</span><span class="sxs-lookup"><span data-stu-id="42142-288">If the declared accessibility of `M` is `private`, the accessibility domain of `M` is the program text of `T`.</span></span>

<span data-ttu-id="42142-289">Z tyto definice vyplývá, že doména přístupnosti vnořeného člena je vždy alespoň textem programu typu, ve kterém člen je deklarovaný.</span><span class="sxs-lookup"><span data-stu-id="42142-289">From these definitions it follows that the accessibility domain of a nested member is always at least the program text of the type in which the member is declared.</span></span> <span data-ttu-id="42142-290">Kromě toho vyplývá, tak doména přístupnosti člena se nikdy výstižnějších než doména přístupnosti typu, ve kterém člen je deklarovaný.</span><span class="sxs-lookup"><span data-stu-id="42142-290">Furthermore, it follows that the accessibility domain of a member is never more inclusive than the accessibility domain of the type in which the member is declared.</span></span>

<span data-ttu-id="42142-291">Intuitivní řečeno pokud typ nebo člen `M` je přístup, následující kroky jsou vyhodnoceny Ujistěte se, že je povolen přístup:</span><span class="sxs-lookup"><span data-stu-id="42142-291">In intuitive terms, when a type or member `M` is accessed, the following steps are evaluated to ensure that the access is permitted:</span></span>

*  <span data-ttu-id="42142-292">První, pokud `M` je deklarována v rámci typu (oproti kompilační jednotky nebo oboru názvů), Chyba kompilace nastane, pokud tento typ není dostupný.</span><span class="sxs-lookup"><span data-stu-id="42142-292">First, if `M` is declared within a type (as opposed to a compilation unit or a namespace), a compile-time error occurs if that type is not accessible.</span></span>
*  <span data-ttu-id="42142-293">Když se poté `M` je `public`, je povolen přístup.</span><span class="sxs-lookup"><span data-stu-id="42142-293">Then, if `M` is `public`, the access is permitted.</span></span>
*  <span data-ttu-id="42142-294">Jinak, pokud `M` je `protected internal`, přístup je povolený, pokud se vyskytuje v programu, ve kterém `M` je teď deklarována, nebo pokud dojde k v rámci třídy odvozené z třídy, ve které `M` je deklarovaný a probíhá prostřednictvím odvozené Typ třídy ([chráněného přístupu pro instanci členy](basic-concepts.md#protected-access-for-instance-members)).</span><span class="sxs-lookup"><span data-stu-id="42142-294">Otherwise, if `M` is `protected internal`, the access is permitted if it occurs within the program in which `M` is declared, or if it occurs within a class derived from the class in which `M` is declared and takes place through the derived class type ([Protected access for instance members](basic-concepts.md#protected-access-for-instance-members)).</span></span>
*  <span data-ttu-id="42142-295">Jinak, pokud `M` je `protected`, přístup je povolený, pokud k němu dojde v rámci třídy, ve které `M` je teď deklarována, nebo pokud dojde k v rámci třídy odvozené z třídy, ve které `M` je deklarovaný a probíhá prostřednictvím odvozené Typ třídy ([chráněného přístupu pro instanci členy](basic-concepts.md#protected-access-for-instance-members)).</span><span class="sxs-lookup"><span data-stu-id="42142-295">Otherwise, if `M` is `protected`, the access is permitted if it occurs within the class in which `M` is declared, or if it occurs within a class derived from the class in which `M` is declared and takes place through the derived class type ([Protected access for instance members](basic-concepts.md#protected-access-for-instance-members)).</span></span>
*  <span data-ttu-id="42142-296">Jinak, pokud `M` je `internal`, pokud se vyskytuje v programu, ve kterém smí obsahovat přístup `M` je deklarována.</span><span class="sxs-lookup"><span data-stu-id="42142-296">Otherwise, if `M` is `internal`, the access is permitted if it occurs within the program in which `M` is declared.</span></span>
*  <span data-ttu-id="42142-297">Jinak, pokud `M` je `private`, pokud k němu dojde v rámci typu, ve kterém smí obsahovat přístup `M` je deklarována.</span><span class="sxs-lookup"><span data-stu-id="42142-297">Otherwise, if `M` is `private`, the access is permitted if it occurs within the type in which `M` is declared.</span></span>
*  <span data-ttu-id="42142-298">V opačném případě tento typ nebo člen je přístupný a dojde k chybě kompilace.</span><span class="sxs-lookup"><span data-stu-id="42142-298">Otherwise, the type or member is inaccessible, and a compile-time error occurs.</span></span>

<span data-ttu-id="42142-299">V příkladu</span><span class="sxs-lookup"><span data-stu-id="42142-299">In the example</span></span>
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
<span data-ttu-id="42142-300">třídy a členy mají následující usnadnění domén:</span><span class="sxs-lookup"><span data-stu-id="42142-300">the classes and members have the following accessibility domains:</span></span>

*  <span data-ttu-id="42142-301">Doména přístupnosti člena `A` a `A.X` neomezený.</span><span class="sxs-lookup"><span data-stu-id="42142-301">The accessibility domain of `A` and `A.X` is unlimited.</span></span>
*  <span data-ttu-id="42142-302">Doména přístupnosti člena `A.Y`, `B`, `B.X`, `B.Y`, `B.C`, `B.C.X`, a `B.C.Y` je text programu obsahující programu.</span><span class="sxs-lookup"><span data-stu-id="42142-302">The accessibility domain of `A.Y`, `B`, `B.X`, `B.Y`, `B.C`, `B.C.X`, and `B.C.Y` is the program text of the containing program.</span></span>
*  <span data-ttu-id="42142-303">Doména přístupnosti člena `A.Z` je text programu `A`.</span><span class="sxs-lookup"><span data-stu-id="42142-303">The accessibility domain of `A.Z` is the program text of `A`.</span></span>
*  <span data-ttu-id="42142-304">Doména přístupnosti člena `B.Z` a `B.D` je text programu `B`, včetně textem programu `B.C` a `B.D`.</span><span class="sxs-lookup"><span data-stu-id="42142-304">The accessibility domain of `B.Z` and `B.D` is the program text of `B`, including the program text of `B.C` and `B.D`.</span></span>
*  <span data-ttu-id="42142-305">Doména přístupnosti člena `B.C.Z` je text programu `B.C`.</span><span class="sxs-lookup"><span data-stu-id="42142-305">The accessibility domain of `B.C.Z` is the program text of `B.C`.</span></span>
*  <span data-ttu-id="42142-306">Doména přístupnosti člena `B.D.X` a `B.D.Y` je text programu `B`, včetně textem programu `B.C` a `B.D`.</span><span class="sxs-lookup"><span data-stu-id="42142-306">The accessibility domain of `B.D.X` and `B.D.Y` is the program text of `B`, including the program text of `B.C` and `B.D`.</span></span>
*  <span data-ttu-id="42142-307">Doména přístupnosti člena `B.D.Z` je text programu `B.D`.</span><span class="sxs-lookup"><span data-stu-id="42142-307">The accessibility domain of `B.D.Z` is the program text of `B.D`.</span></span>

<span data-ttu-id="42142-308">Jak ukazuje příklad, tak doména přístupnosti člena se nikdy větší než u nadřazeného typu.</span><span class="sxs-lookup"><span data-stu-id="42142-308">As the example illustrates, the accessibility domain of a member is never larger than that of a containing type.</span></span> <span data-ttu-id="42142-309">Například i když všechny `X` členové mají veřejné deklarovaná přístupnost, vše kromě `A.X` usnadnění domén, které jsou omezeny nadřazeného typu.</span><span class="sxs-lookup"><span data-stu-id="42142-309">For example, even though all `X` members have public declared accessibility, all but `A.X` have accessibility domains that are constrained by a containing type.</span></span>

<span data-ttu-id="42142-310">Jak je popsáno v [členy](basic-concepts.md#members), odvozené typy dědí všechny členy základní třídy, s výjimkou pro instanci konstruktory, destruktory a statické konstruktory.</span><span class="sxs-lookup"><span data-stu-id="42142-310">As described in [Members](basic-concepts.md#members), all members of a base class, except for instance constructors, destructors and static constructors, are inherited by derived types.</span></span> <span data-ttu-id="42142-311">To zahrnuje i soukromé členy základní třídy.</span><span class="sxs-lookup"><span data-stu-id="42142-311">This includes even private members of a base class.</span></span> <span data-ttu-id="42142-312">Doména přístupnosti člena soukromý člen ale obsahuje pouze textem programu typu, ve kterém člen je deklarovaný.</span><span class="sxs-lookup"><span data-stu-id="42142-312">However, the accessibility domain of a private member includes only the program text of the type in which the member is declared.</span></span> <span data-ttu-id="42142-313">V příkladu</span><span class="sxs-lookup"><span data-stu-id="42142-313">In the example</span></span>
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
<span data-ttu-id="42142-314">`B` třída dědí soukromému členu `x` z `A` třídy.</span><span class="sxs-lookup"><span data-stu-id="42142-314">the `B` class inherits the private member `x` from the `A` class.</span></span> <span data-ttu-id="42142-315">Protože je privátní člen, je dostupná jenom v rámci *class_body* z `A`.</span><span class="sxs-lookup"><span data-stu-id="42142-315">Because the member is private, it is only accessible within the *class_body* of `A`.</span></span> <span data-ttu-id="42142-316">Proto přístup k `b.x` orientovaný `A.F` metody, ale selže v `B.F` metody.</span><span class="sxs-lookup"><span data-stu-id="42142-316">Thus, the access to `b.x` succeeds in the `A.F` method, but fails in the `B.F` method.</span></span>

### <a name="protected-access-for-instance-members"></a><span data-ttu-id="42142-317">Chráněného přístupu pro členy instance</span><span class="sxs-lookup"><span data-stu-id="42142-317">Protected access for instance members</span></span>

<span data-ttu-id="42142-318">Při `protected` člena instance je přistupováno mimo textem programu třídy, ve kterém je deklarována, a kdy `protected internal` člena instance je přístupná mimo textem programu program, ve kterém je deklarována, přístup musí proběhnout v rámci deklarace třídy, která je odvozena z třídy, ve kterém je deklarována.</span><span class="sxs-lookup"><span data-stu-id="42142-318">When a `protected` instance member is accessed outside the program text of the class in which it is declared, and when a `protected internal` instance member is accessed outside the program text of the program in which it is declared, the access must take place within a class declaration that derives from the class in which it is declared.</span></span> <span data-ttu-id="42142-319">Kromě toho přístup je potřeba provést prostřednictvím instance typu odvozené třídy nebo typu třídy zkonstruovat z něj.</span><span class="sxs-lookup"><span data-stu-id="42142-319">Furthermore, the access is required to take place through an instance of that derived class type or a class type constructed from it.</span></span> <span data-ttu-id="42142-320">Toto omezení zabraňuje v přístupu k chráněným členům jiné odvozené třídy i v případě, že členové jsou zděděny ze stejné základní třídy jedné odvozené třídy.</span><span class="sxs-lookup"><span data-stu-id="42142-320">This restriction prevents one derived class from accessing protected members of other derived classes, even when the members are inherited from the same base class.</span></span>

<span data-ttu-id="42142-321">Umožní `B` být základní třídu, která deklaruje člen chráněnou instanci `M`a nechat `D` být třída odvozená z `B`.</span><span class="sxs-lookup"><span data-stu-id="42142-321">Let `B` be a base class that declares a protected instance member `M`, and let `D` be a class that derives from `B`.</span></span> <span data-ttu-id="42142-322">V rámci *class_body* z `D`, přístup k `M` můžete provést jednu z následujících forem:</span><span class="sxs-lookup"><span data-stu-id="42142-322">Within the *class_body* of `D`, access to `M` can take one of the following forms:</span></span>

*  <span data-ttu-id="42142-323">Neúplného *type_name* nebo *primary_expression* formuláře `M`.</span><span class="sxs-lookup"><span data-stu-id="42142-323">An unqualified *type_name* or *primary_expression* of the form `M`.</span></span>
*  <span data-ttu-id="42142-324">A *primary_expression* formuláře `E.M`, zadaný typ `E` je `T` nebo třída odvozená z `T`, kde `T` je typ třídy `D`, nebo typ třídy. zkonstruovat z `D`</span><span class="sxs-lookup"><span data-stu-id="42142-324">A *primary_expression* of the form `E.M`, provided the type of `E` is `T` or a class derived from `T`, where `T` is the class type `D`, or a class type constructed from `D`</span></span>
*  <span data-ttu-id="42142-325">A *primary_expression* formuláře `base.M`.</span><span class="sxs-lookup"><span data-stu-id="42142-325">A *primary_expression* of the form `base.M`.</span></span>

<span data-ttu-id="42142-326">Kromě těchto forem přístup odvozené třídy přístup k chráněné instance konstruktoru základní třídy v *constructor_initializer* ([inicializátory konstruktoru](classes.md#constructor-initializers)).</span><span class="sxs-lookup"><span data-stu-id="42142-326">In addition to these forms of access, a derived class can access a protected instance constructor of a base class in a *constructor_initializer* ([Constructor initializers](classes.md#constructor-initializers)).</span></span>

<span data-ttu-id="42142-327">V příkladu</span><span class="sxs-lookup"><span data-stu-id="42142-327">In the example</span></span>
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
<span data-ttu-id="42142-328">v rámci `A`, je možné přístup `x` prostřednictvím instance obou `A` a `B`, protože v obou případech přístup probíhá přes instanci `A` nebo třída odvozená z `A`.</span><span class="sxs-lookup"><span data-stu-id="42142-328">within `A`, it is possible to access `x` through instances of both `A` and `B`, since in either case the access takes place through an instance of `A` or a class derived from `A`.</span></span> <span data-ttu-id="42142-329">Ale v rámci `B`, není možné přistupovat k `x` prostřednictvím instance `A`, protože `A` není odvozen od `B`.</span><span class="sxs-lookup"><span data-stu-id="42142-329">However, within `B`, it is not possible to access `x` through an instance of `A`, since `A` does not derive from `B`.</span></span>

<span data-ttu-id="42142-330">V příkladu</span><span class="sxs-lookup"><span data-stu-id="42142-330">In the example</span></span>
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
<span data-ttu-id="42142-331">tři přiřazení `x` se nepovoluje, protože všechny probíhat přes instance typů tříd, které jsou zhotoveny z obecného typu.</span><span class="sxs-lookup"><span data-stu-id="42142-331">the three assignments to `x` are permitted because they all take place through instances of class types constructed from the generic type.</span></span>

### <a name="accessibility-constraints"></a><span data-ttu-id="42142-332">Omezení přístupnosti</span><span class="sxs-lookup"><span data-stu-id="42142-332">Accessibility constraints</span></span>

<span data-ttu-id="42142-333">Několik konstrukce v jazyce C# vyžaduje typ být ***přinejmenším stejně dostupná jako*** člena nebo jiného typu.</span><span class="sxs-lookup"><span data-stu-id="42142-333">Several constructs in the C# language require a type to be ***at least as accessible as*** a member or another type.</span></span> <span data-ttu-id="42142-334">Typ `T` říká, že je dostupná jako člen nebo typ `M` Pokud doména přístupnosti člena `T` je nadstavbou jazyka tak doména přístupnosti člena `M`.</span><span class="sxs-lookup"><span data-stu-id="42142-334">A type `T` is said to be at least as accessible as a member or type `M` if the accessibility domain of `T` is a superset of the accessibility domain of `M`.</span></span> <span data-ttu-id="42142-335">Jinými slovy `T` je přinejmenším stejně dostupná jako `M` Pokud `T` je dostupný ve všech kontextech, ve kterém `M` přístupný.</span><span class="sxs-lookup"><span data-stu-id="42142-335">In other words, `T` is at least as accessible as `M` if `T` is accessible in all contexts in which `M` is accessible.</span></span>

<span data-ttu-id="42142-336">Existují následující omezení pro usnadnění:</span><span class="sxs-lookup"><span data-stu-id="42142-336">The following accessibility constraints exist:</span></span>

*  <span data-ttu-id="42142-337">Přímou základní třídu typu třídy musí být přinejmenším stejně dostupná jako typ třídy.</span><span class="sxs-lookup"><span data-stu-id="42142-337">The direct base class of a class type must be at least as accessible as the class type itself.</span></span>
*  <span data-ttu-id="42142-338">Explicitní základní rozhraní typu rozhraní musí být přinejmenším stejně dostupná jako samotného typu rozhraní.</span><span class="sxs-lookup"><span data-stu-id="42142-338">The explicit base interfaces of an interface type must be at least as accessible as the interface type itself.</span></span>
*  <span data-ttu-id="42142-339">Návratový typ a typy parametrů typu delegáta musí být přinejmenším stejně dostupná jako samotného typu delegáta.</span><span class="sxs-lookup"><span data-stu-id="42142-339">The return type and parameter types of a delegate type must be at least as accessible as the delegate type itself.</span></span>
*  <span data-ttu-id="42142-340">Typ konstanty musí být přinejmenším stejně dostupná jako konstanta samotný.</span><span class="sxs-lookup"><span data-stu-id="42142-340">The type of a constant must be at least as accessible as the constant itself.</span></span>
*  <span data-ttu-id="42142-341">Typ pole musí být přinejmenším stejně dostupná jako vlastní pole.</span><span class="sxs-lookup"><span data-stu-id="42142-341">The type of a field must be at least as accessible as the field itself.</span></span>
*  <span data-ttu-id="42142-342">Návratový typ a typy parametrů metody musí být přinejmenším stejně dostupná jako metoda sama.</span><span class="sxs-lookup"><span data-stu-id="42142-342">The return type and parameter types of a method must be at least as accessible as the method itself.</span></span>
*  <span data-ttu-id="42142-343">Typ vlastnosti musí být přinejmenším stejně dostupná jako samotné vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="42142-343">The type of a property must be at least as accessible as the property itself.</span></span>
*  <span data-ttu-id="42142-344">Typ události musí být přinejmenším stejně dostupná jako samotné události.</span><span class="sxs-lookup"><span data-stu-id="42142-344">The type of an event must be at least as accessible as the event itself.</span></span>
*  <span data-ttu-id="42142-345">Typ a parametrem typy indexer musí být přinejmenším stejně dostupná jako samotný indexeru.</span><span class="sxs-lookup"><span data-stu-id="42142-345">The type and parameter types of an indexer must be at least as accessible as the indexer itself.</span></span>
*  <span data-ttu-id="42142-346">Návratový typ a typy parametrů operátoru musí být přinejmenším stejně dostupná jako operátor.</span><span class="sxs-lookup"><span data-stu-id="42142-346">The return type and parameter types of an operator must be at least as accessible as the operator itself.</span></span>
*  <span data-ttu-id="42142-347">Typy parametrů konstruktoru instance musí být přinejmenším stejně dostupná jako konstruktor instance, samotného.</span><span class="sxs-lookup"><span data-stu-id="42142-347">The parameter types of an instance constructor must be at least as accessible as the instance constructor itself.</span></span>

<span data-ttu-id="42142-348">V příkladu</span><span class="sxs-lookup"><span data-stu-id="42142-348">In the example</span></span>
```csharp
class A {...}

public class B: A {...}
```
<span data-ttu-id="42142-349">`B` třídy výsledkem chyba kompilace, protože `A` není dostupné jako `B`.</span><span class="sxs-lookup"><span data-stu-id="42142-349">the `B` class results in a compile-time error because `A` is not at least as accessible as `B`.</span></span>

<span data-ttu-id="42142-350">Podobně v příkladu</span><span class="sxs-lookup"><span data-stu-id="42142-350">Likewise, in the example</span></span>
```csharp
class A {...}

public class B
{
    A F() {...}

    internal A G() {...}

    public A H() {...}
}
```
<span data-ttu-id="42142-351">`H` metoda `B` výsledkem chyba kompilace, protože návratový typ `A` není dostupný jako metodu.</span><span class="sxs-lookup"><span data-stu-id="42142-351">the `H` method in `B` results in a compile-time error because the return type `A` is not at least as accessible as the method.</span></span>

## <a name="signatures-and-overloading"></a><span data-ttu-id="42142-352">Podpisy a přetížení</span><span class="sxs-lookup"><span data-stu-id="42142-352">Signatures and overloading</span></span>

<span data-ttu-id="42142-353">Metody, konstruktory instancí, indexery a operátory jsou charakteristické jejich ***podpisy***:</span><span class="sxs-lookup"><span data-stu-id="42142-353">Methods, instance constructors, indexers, and operators are characterized by their ***signatures***:</span></span>

*  <span data-ttu-id="42142-354">Podpis metody se skládá z názvu metody, počet parametrů typu a typ a typ (hodnota, odkaz nebo výstup) každý z jejích formálních parametrů v pořadí zleva doprava.</span><span class="sxs-lookup"><span data-stu-id="42142-354">The signature of a method consists of the name of the method, the number of type parameters and the type and kind (value, reference, or output) of each of its formal parameters, considered in the order left to right.</span></span> <span data-ttu-id="42142-355">Pro tyto účely je identifikován libovolný typ parametru metody, nacházející se v typu formálního parametru není podle názvu, ale podle jeho pořadové číslo pozice v seznamu argumentů typu metody.</span><span class="sxs-lookup"><span data-stu-id="42142-355">For these purposes, any type parameter of the method that occurs in the type of a formal parameter is identified not by its name, but by its ordinal position in the type argument list of the method.</span></span> <span data-ttu-id="42142-356">Podpis metody konkrétně nezahrnuje návratovým typem, `params` modifikátor, který může být zadáno pro parametr úplně vpravo ani omezení parametru typu volitelné.</span><span class="sxs-lookup"><span data-stu-id="42142-356">The signature of a method specifically does not include the return type, the `params` modifier that may be specified for the right-most parameter, nor the optional type parameter constraints.</span></span>
*  <span data-ttu-id="42142-357">Podpis konstruktoru instance se skládá z typu a druh (hodnota, odkaz nebo výstup) každý z jejích formálních parametrů v pořadí zleva doprava.</span><span class="sxs-lookup"><span data-stu-id="42142-357">The signature of an instance constructor consists of the type and kind (value, reference, or output) of each of its formal parameters, considered in the order left to right.</span></span> <span data-ttu-id="42142-358">Podpis konstruktoru instance výslovně nezahrnete `params` modifikátor, který může být zadáno pro parametr úplně vpravo.</span><span class="sxs-lookup"><span data-stu-id="42142-358">The signature of an instance constructor specifically does not include the `params` modifier that may be specified for the right-most parameter.</span></span>
*  <span data-ttu-id="42142-359">Podpis indexeru se skládá z typu každý z jejích formálních parametrů v pořadí zleva doprava.</span><span class="sxs-lookup"><span data-stu-id="42142-359">The signature of an indexer consists of the type of each of its formal parameters, considered in the order left to right.</span></span> <span data-ttu-id="42142-360">Podpis indexer konkrétně neobsahuje typ elementu ani obsahuje `params` modifikátor, který může být zadáno pro parametr úplně vpravo.</span><span class="sxs-lookup"><span data-stu-id="42142-360">The signature of an indexer specifically does not include the element type, nor does it include the `params` modifier that may be specified for the right-most parameter.</span></span>
*  <span data-ttu-id="42142-361">Podpis operátora se skládá z názvu operátoru a typu každý z jejích formálních parametrů v pořadí zleva doprava.</span><span class="sxs-lookup"><span data-stu-id="42142-361">The signature of an operator consists of the name of the operator and the type of each of its formal parameters, considered in the order left to right.</span></span> <span data-ttu-id="42142-362">Podpis operátora konkrétně neobsahuje typ výsledku.</span><span class="sxs-lookup"><span data-stu-id="42142-362">The signature of an operator specifically does not include the result type.</span></span>

<span data-ttu-id="42142-363">Podpisy jsou povolení mechanismus pro ***přetížení*** členů třídy, struktury a rozhraní:</span><span class="sxs-lookup"><span data-stu-id="42142-363">Signatures are the enabling mechanism for ***overloading*** of members in classes, structs, and interfaces:</span></span>

*  <span data-ttu-id="42142-364">Přetěžování metod povoluje třídy, struktury nebo rozhraní, chcete-li deklarovat více metod se stejným názvem, poskytuje jejich podpisy musí být jedinečné v rámci této třídy, struktury nebo rozhraní.</span><span class="sxs-lookup"><span data-stu-id="42142-364">Overloading of methods permits a class, struct, or interface to declare multiple methods with the same name, provided their signatures are unique within that class, struct, or interface.</span></span>
*  <span data-ttu-id="42142-365">Přetížení konstruktory instancí umožňuje třídě nebo struktuře, chcete-li deklarovat více instančních konstruktorech, pokud jejich podpisy jsou jedinečné v rámci této třídy nebo struktury.</span><span class="sxs-lookup"><span data-stu-id="42142-365">Overloading of instance constructors permits a class or struct to declare multiple instance constructors, provided their signatures are unique within that class or struct.</span></span>
*  <span data-ttu-id="42142-366">Přetížení indexerů umožňuje třídy, struktury nebo rozhraní, chcete-li deklarovat několik indexerů, pokud jejich podpisy jsou jedinečné v rámci této třídy, struktury nebo rozhraní.</span><span class="sxs-lookup"><span data-stu-id="42142-366">Overloading of indexers permits a class, struct, or interface to declare multiple indexers, provided their signatures are unique within that class, struct, or interface.</span></span>
*  <span data-ttu-id="42142-367">Přetížení operátorů umožňuje třídě nebo struktuře, chcete-li deklarovat více operátorů se stejným názvem, poskytuje jejich podpisy musí být jedinečné v rámci této třídy nebo struktury.</span><span class="sxs-lookup"><span data-stu-id="42142-367">Overloading of operators permits a class or struct to declare multiple operators with the same name, provided their signatures are unique within that class or struct.</span></span>

<span data-ttu-id="42142-368">I když `out` a `ref` modifikátorech parametrů jsou považovány za součást podpisu, členy deklarované v jeden typ se nemůže lišit v podpisu výhradně nástrojem `ref` a `out`.</span><span class="sxs-lookup"><span data-stu-id="42142-368">Although `out` and `ref` parameter modifiers are considered part of a signature, members declared in a single type cannot differ in signature solely by `ref` and `out`.</span></span> <span data-ttu-id="42142-369">Chyba kompilace nastane, pokud dva členy jsou deklarovány v rámci stejného typu s podpisy, které bude stejný, když všechny parametry v obou metodách s `out` modifikátory byly změněny na `ref` modifikátory.</span><span class="sxs-lookup"><span data-stu-id="42142-369">A compile-time error occurs if two members are declared in the same type with signatures that would be the same if all parameters in both methods with `out` modifiers were changed to `ref` modifiers.</span></span> <span data-ttu-id="42142-370">Pro jiné účely odpovídajících podpis (například skryjete nebo přepsání), `ref` a `out` jsou považovány za součást podpisu a navzájem neodpovídají.</span><span class="sxs-lookup"><span data-stu-id="42142-370">For other purposes of signature matching (e.g., hiding or overriding), `ref` and `out` are considered part of the signature and do not match each other.</span></span> <span data-ttu-id="42142-371">(Toto omezení je umožňuje programům C# a spusťte na běžné Language infrastruktury (CLI), která neposkytuje způsob, jak definovat metody, které se liší pouze v snadno převést `ref` a `out`.)</span><span class="sxs-lookup"><span data-stu-id="42142-371">(This restriction is to allow C#  programs to be easily translated to run on the Common Language Infrastructure (CLI), which does not provide a way to define methods that differ solely in `ref` and `out`.)</span></span>

<span data-ttu-id="42142-372">Pro účely podpisy, typy `object` a `dynamic` jsou považovány za totéž.</span><span class="sxs-lookup"><span data-stu-id="42142-372">For the purposes of signatures, the types `object` and `dynamic` are considered the same.</span></span> <span data-ttu-id="42142-373">Členy deklarované v jeden typ nelze proto se liší v podpisu výhradně nástrojem `object` a `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="42142-373">Members declared in a single type can therefore not differ in signature solely by `object` and `dynamic`.</span></span>

<span data-ttu-id="42142-374">Následující příklad ukazuje sadu deklarace přetížené metody společně s jejich podpisy.</span><span class="sxs-lookup"><span data-stu-id="42142-374">The following example shows a set of overloaded method declarations along with their signatures.</span></span>
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

<span data-ttu-id="42142-375">Všimněte si, že všechny `ref` a `out` modifikátorech parametrů ([parametry metody](classes.md#method-parameters)) jsou součástí podpis.</span><span class="sxs-lookup"><span data-stu-id="42142-375">Note that any `ref` and `out` parameter modifiers ([Method parameters](classes.md#method-parameters)) are part of a signature.</span></span> <span data-ttu-id="42142-376">Proto `F(int)` a `F(ref int)` jsou jedinečné podpisy.</span><span class="sxs-lookup"><span data-stu-id="42142-376">Thus, `F(int)` and `F(ref int)` are unique signatures.</span></span> <span data-ttu-id="42142-377">Ale `F(ref int)` a `F(out int)` nelze deklarovat v rámci stejné rozhraní, protože se liší jejich podpisy výhradně nástrojem `ref` a `out`.</span><span class="sxs-lookup"><span data-stu-id="42142-377">However, `F(ref int)` and `F(out int)` cannot be declared within the same interface because their signatures differ solely by `ref` and `out`.</span></span> <span data-ttu-id="42142-378">Všimněte si také, aby návratový typ a `params` modifikátor nejsou součástí podpis, takže ho není možné přetížení založené výhradně na návratový typ nebo zahrnutí nebo vyloučení `params` modifikátor.</span><span class="sxs-lookup"><span data-stu-id="42142-378">Also, note that the return type and the `params` modifier are not part of a signature, so it is not possible to overload solely based on return type or on the inclusion or exclusion of the `params` modifier.</span></span> <span data-ttu-id="42142-379">Jako takové deklarace metody `F(int)` a `F(params string[])` určené výše vyústí v chybu kompilace.</span><span class="sxs-lookup"><span data-stu-id="42142-379">As such, the declarations of the methods `F(int)` and `F(params string[])` identified above result in a compile-time error.</span></span>

## <a name="scopes"></a><span data-ttu-id="42142-380">Obory</span><span class="sxs-lookup"><span data-stu-id="42142-380">Scopes</span></span>

<span data-ttu-id="42142-381">***Oboru*** názvu je oblast textu programu, ve kterém je možné k odkazování na entity deklarované podle názvu bez kvalifikace názvu.</span><span class="sxs-lookup"><span data-stu-id="42142-381">The ***scope*** of a name is the region of program text within which it is possible to refer to the entity declared by the name without qualification of the name.</span></span> <span data-ttu-id="42142-382">Může být obory ***vnořené***, a vnitřní obor může předeklarovat význam názvu z vnějšího oboru (to ale neodebere omezení stanovené [deklarace](basic-concepts.md#declarations) , že ve vnořeném bloku není možná pro deklarování místní proměnné se stejným názvem jako místní proměnnou v nadřízeném bloku).</span><span class="sxs-lookup"><span data-stu-id="42142-382">Scopes can be ***nested***, and an inner scope may redeclare the meaning of a name from an outer scope (this does not, however, remove the restriction imposed by [Declarations](basic-concepts.md#declarations) that within a nested block it is not possible to declare a local variable with the same name as a local variable in an enclosing block).</span></span> <span data-ttu-id="42142-383">Název z vnějšího oboru je pak říká, že ***skryté*** v oblasti program text vztahuje na vnitřní a přístup k vnější název je možné jenom ručním kvalifikováním názvu.</span><span class="sxs-lookup"><span data-stu-id="42142-383">The name from the outer scope is then said to be ***hidden*** in the region of program text covered by the inner scope, and access to the outer name is only possible by qualifying the name.</span></span>

*  <span data-ttu-id="42142-384">Obor členem oboru názvů deklarována *namespace_member_declaration* ([Namespace členy](namespaces.md#namespace-members)) s žádný nadřazený *namespace_declaration* je celého programu text.</span><span class="sxs-lookup"><span data-stu-id="42142-384">The scope of a namespace member declared by a *namespace_member_declaration* ([Namespace members](namespaces.md#namespace-members)) with no enclosing *namespace_declaration* is the entire program text.</span></span>
*  <span data-ttu-id="42142-385">Obor členem oboru názvů deklarována *namespace_member_declaration* v rámci *namespace_declaration* jehož plně kvalifikovaný název je `N` je *namespace_body*  z každé *namespace_declaration* jehož plně kvalifikovaný název je `N` nebo začíná `N`, následované tečkou.</span><span class="sxs-lookup"><span data-stu-id="42142-385">The scope of a namespace member declared by a *namespace_member_declaration* within a *namespace_declaration* whose fully qualified name is `N` is the *namespace_body* of every *namespace_declaration* whose fully qualified name is `N` or starts with `N`, followed by a period.</span></span>
*  <span data-ttu-id="42142-386">Obor názvu určené *extern_alias_directive* rozloženo *using_directive*s, *global_attributes* a *namespace_member_ deklarace*s jeho bezprostředně nadřazeného kompilace částí nebo těle oboru názvů.</span><span class="sxs-lookup"><span data-stu-id="42142-386">The scope of name defined by an *extern_alias_directive* extends over the *using_directive*s, *global_attributes* and *namespace_member_declaration*s of its immediately containing compilation unit or namespace body.</span></span> <span data-ttu-id="42142-387">*Extern_alias_directive* není přispívat všemi novými členy do prostoru základní deklarace.</span><span class="sxs-lookup"><span data-stu-id="42142-387">An *extern_alias_directive* does not contribute any new members to the underlying declaration space.</span></span> <span data-ttu-id="42142-388">Jinými slovy *extern_alias_directive* není přenosné, ale místo toho ovlivňuje pouze kompilaci jednotky nebo oboru názvů text ve kterém se vyskytuje.</span><span class="sxs-lookup"><span data-stu-id="42142-388">In other words, an *extern_alias_directive* is not transitive, but, rather, affects only the compilation unit or namespace body in which it occurs.</span></span>
*  <span data-ttu-id="42142-389">Obor názvu definován ani importován podle *using_directive* ([direktiv Using](namespaces.md#using-directives)) rozšiřuje přes *namespace_member_declaration*s  *compilation_unit* nebo *namespace_body* ve kterém *using_directive* vyvolá.</span><span class="sxs-lookup"><span data-stu-id="42142-389">The scope of a name defined or imported by a *using_directive* ([Using directives](namespaces.md#using-directives)) extends over the *namespace_member_declaration*s of the *compilation_unit* or *namespace_body* in which the *using_directive* occurs.</span></span> <span data-ttu-id="42142-390">A *using_directive* zpřístupnit nula nebo více názvů, typ nebo člen názvy v rámci konkrétní *compilation_unit* nebo *namespace_body*, ale ne přispívat všemi novými členy do prostoru základní deklarace.</span><span class="sxs-lookup"><span data-stu-id="42142-390">A *using_directive* may make zero or more namespace, type or member names available within a particular *compilation_unit* or *namespace_body*, but does not contribute any new members to the underlying declaration space.</span></span> <span data-ttu-id="42142-391">Jinými slovy *using_directive* není přenosné, ale místo toho má vliv pouze *compilation_unit* nebo *namespace_body* ve kterém se vyskytuje.</span><span class="sxs-lookup"><span data-stu-id="42142-391">In other words, a *using_directive* is not transitive but rather affects only the *compilation_unit* or *namespace_body* in which it occurs.</span></span>
*  <span data-ttu-id="42142-392">Obor parametr typu deklarován *type_parameter_list* na *class_declaration* ([třídy deklarací](classes.md#class-declarations)) je *class_base*, *type_parameter_constraints_clause*s, a *class_body* tohoto *class_declaration*.</span><span class="sxs-lookup"><span data-stu-id="42142-392">The scope of a type parameter declared by a *type_parameter_list* on a *class_declaration* ([Class declarations](classes.md#class-declarations)) is the *class_base*, *type_parameter_constraints_clause*s, and *class_body* of that *class_declaration*.</span></span>
*  <span data-ttu-id="42142-393">Obor parametr typu deklarován *type_parameter_list* na *struct_declaration* ([deklarace struktury](structs.md#struct-declarations)) je *struct_interfaces* , *type_parameter_constraints_clause*s, a *struct_body* tohoto *struct_declaration*.</span><span class="sxs-lookup"><span data-stu-id="42142-393">The scope of a type parameter declared by a *type_parameter_list* on a *struct_declaration* ([Struct declarations](structs.md#struct-declarations)) is the *struct_interfaces*, *type_parameter_constraints_clause*s, and *struct_body* of that *struct_declaration*.</span></span>
*  <span data-ttu-id="42142-394">Obor parametr typu deklarován *type_parameter_list* na *interface_declaration* ([rozhraní deklarace](interfaces.md#interface-declarations)) je *interface_base* , *type_parameter_constraints_clause*s, a *interface_body* tohoto *interface_declaration*.</span><span class="sxs-lookup"><span data-stu-id="42142-394">The scope of a type parameter declared by a *type_parameter_list* on an *interface_declaration* ([Interface declarations](interfaces.md#interface-declarations)) is the *interface_base*, *type_parameter_constraints_clause*s, and *interface_body* of that *interface_declaration*.</span></span>
*  <span data-ttu-id="42142-395">Obor parametr typu deklarován *type_parameter_list* na *delegate_declaration* ([delegovat deklarace](delegates.md#delegate-declarations)) je *typ*, *formal_parameter_list*, a *type_parameter_constraints_clause*s, který *delegate_declaration*.</span><span class="sxs-lookup"><span data-stu-id="42142-395">The scope of a type parameter declared by a *type_parameter_list* on a *delegate_declaration* ([Delegate declarations](delegates.md#delegate-declarations)) is the *return_type*, *formal_parameter_list*, and *type_parameter_constraints_clause*s of that *delegate_declaration*.</span></span>
*  <span data-ttu-id="42142-396">Obor člen deklarován *class_member_declaration* ([tělo třídy](classes.md#class-body)) je *class_body* ve kterém dochází k deklaraci.</span><span class="sxs-lookup"><span data-stu-id="42142-396">The scope of a member declared by a *class_member_declaration* ([Class body](classes.md#class-body)) is the *class_body* in which the declaration occurs.</span></span> <span data-ttu-id="42142-397">Kromě toho oboru členu třídy rozšiřuje na *class_body* z nich odvozené třídy, které jsou součástí tak doména přístupnosti ([usnadnění domén](basic-concepts.md#accessibility-domains)) člena.</span><span class="sxs-lookup"><span data-stu-id="42142-397">In addition, the scope of a class member extends to the *class_body* of those derived classes that are included in the accessibility domain ([Accessibility domains](basic-concepts.md#accessibility-domains)) of the member.</span></span>
*  <span data-ttu-id="42142-398">Obor člen deklarován *struct_member_declaration* ([členy struktury](structs.md#struct-members)) je *struct_body* ve kterém dochází k deklaraci.</span><span class="sxs-lookup"><span data-stu-id="42142-398">The scope of a member declared by a *struct_member_declaration* ([Struct members](structs.md#struct-members)) is the *struct_body* in which the declaration occurs.</span></span>
*  <span data-ttu-id="42142-399">Obor člen deklarován *enum_member_declaration* ([členy výčtu](enums.md#enum-members)) je *enum_body* ve kterém dochází k deklaraci.</span><span class="sxs-lookup"><span data-stu-id="42142-399">The scope of a member declared by an *enum_member_declaration*  ([Enum members](enums.md#enum-members)) is the *enum_body* in which the declaration occurs.</span></span>
*  <span data-ttu-id="42142-400">Obor parametr deklarovaný v *method_declaration* ([metody](classes.md#methods)) je *method_body* tohoto *method_declaration*.</span><span class="sxs-lookup"><span data-stu-id="42142-400">The scope of a parameter declared in a *method_declaration* ([Methods](classes.md#methods)) is the *method_body* of that *method_declaration*.</span></span>
*  <span data-ttu-id="42142-401">Obor parametr deklarovaný v *indexer_declaration* ([indexery](classes.md#indexers)) je *accessor_declarations* tohoto *indexer_declaration*.</span><span class="sxs-lookup"><span data-stu-id="42142-401">The scope of a parameter declared in an *indexer_declaration* ([Indexers](classes.md#indexers)) is the *accessor_declarations* of that *indexer_declaration*.</span></span>
*  <span data-ttu-id="42142-402">Obor parametr deklarovaný v *operator_declaration* ([operátory](classes.md#operators)) je *bloku* tohoto *operator_declaration*.</span><span class="sxs-lookup"><span data-stu-id="42142-402">The scope of a parameter declared in an *operator_declaration* ([Operators](classes.md#operators)) is the *block* of that *operator_declaration*.</span></span>
*  <span data-ttu-id="42142-403">Obor parametr deklarovaný v *constructor_declaration* ([Instance konstruktory](classes.md#instance-constructors)) je *constructor_initializer* a *bloku* tohoto *constructor_declaration*.</span><span class="sxs-lookup"><span data-stu-id="42142-403">The scope of a parameter declared in a *constructor_declaration* ([Instance constructors](classes.md#instance-constructors)) is the *constructor_initializer* and *block* of that *constructor_declaration*.</span></span>
*  <span data-ttu-id="42142-404">Obor parametr deklarovaný v *lambda_expression* ([výrazy anonymní funkce](expressions.md#anonymous-function-expressions)) je *anonymous_function_body* tohoto *lambda_ výraz*</span><span class="sxs-lookup"><span data-stu-id="42142-404">The scope of a parameter declared in a *lambda_expression* ([Anonymous function expressions](expressions.md#anonymous-function-expressions)) is the *anonymous_function_body* of that *lambda_expression*</span></span>
*  <span data-ttu-id="42142-405">Obor parametr deklarovaný v *anonymous_method_expression* ([výrazy anonymní funkce](expressions.md#anonymous-function-expressions)) je *bloku* tohoto *anonymous_method _expression*.</span><span class="sxs-lookup"><span data-stu-id="42142-405">The scope of a parameter declared in an *anonymous_method_expression* ([Anonymous function expressions](expressions.md#anonymous-function-expressions)) is the *block* of that *anonymous_method_expression*.</span></span>
*  <span data-ttu-id="42142-406">Obor popisku deklarované v *labeled_statement* ([příkazy s popiskem](statements.md#labeled-statements)) je *bloku* ve kterém dochází k deklaraci.</span><span class="sxs-lookup"><span data-stu-id="42142-406">The scope of a label declared in a *labeled_statement* ([Labeled statements](statements.md#labeled-statements)) is the *block* in which the declaration occurs.</span></span>
*  <span data-ttu-id="42142-407">Lokální proměnná deklarovaná v rozsahu *local_variable_declaration* ([místní deklarace proměnné](statements.md#local-variable-declarations)) je blok ve kterém dochází k deklaraci.</span><span class="sxs-lookup"><span data-stu-id="42142-407">The scope of a local variable declared in a *local_variable_declaration* ([Local variable declarations](statements.md#local-variable-declarations)) is the block in which the declaration occurs.</span></span>
*  <span data-ttu-id="42142-408">Lokální proměnná deklarovaná v rozsahu *switch_block* z `switch` – příkaz ([příkazu switch](statements.md#the-switch-statement)) je *switch_block*.</span><span class="sxs-lookup"><span data-stu-id="42142-408">The scope of a local variable declared in a *switch_block* of a `switch` statement ([The switch statement](statements.md#the-switch-statement)) is the *switch_block*.</span></span>
*  <span data-ttu-id="42142-409">Lokální proměnná deklarovaná v rozsahu *for_initializer* z `for` – příkaz ([pro příkaz](statements.md#the-for-statement)) je *for_initializer*,  *for_condition*, *for_iterator*a uzavřeného *příkaz* z `for` příkazu.</span><span class="sxs-lookup"><span data-stu-id="42142-409">The scope of a local variable declared in a *for_initializer* of a `for` statement ([The for statement](statements.md#the-for-statement)) is the *for_initializer*, the *for_condition*, the *for_iterator*, and the contained *statement* of the `for` statement.</span></span>
*  <span data-ttu-id="42142-410">Obor lokální konstanta deklarované v *local_constant_declaration* ([místní deklarace konstantní](statements.md#local-constant-declarations)) je blok ve kterém dochází k deklaraci.</span><span class="sxs-lookup"><span data-stu-id="42142-410">The scope of a local constant declared in a *local_constant_declaration* ([Local constant declarations](statements.md#local-constant-declarations)) is the block in which the declaration occurs.</span></span> <span data-ttu-id="42142-411">Jde chybu v době kompilace k odkazování na lokální konstanta v textové pozici, která předchází jeho *constant_declarator*.</span><span class="sxs-lookup"><span data-stu-id="42142-411">It is a compile-time error to refer to a local constant in a textual position that precedes its *constant_declarator*.</span></span>
*  <span data-ttu-id="42142-412">Obor proměnné deklarované jako součást *foreach_statement*, *using_statement*, *lock_statement* nebo *query_expression* je Určuje rozšíření dané konstrukce.</span><span class="sxs-lookup"><span data-stu-id="42142-412">The scope of a variable declared as part of a *foreach_statement*, *using_statement*, *lock_statement* or *query_expression* is determined by the expansion of the given construct.</span></span>

<span data-ttu-id="42142-413">V rámci oboru názvů, třídy, struktury nebo výčtu člen je možné odkazovat na člen v textové pozici, která předchází deklarace člena.</span><span class="sxs-lookup"><span data-stu-id="42142-413">Within the scope of a namespace, class, struct, or enumeration member it is possible to refer to the member in a textual position that precedes the declaration of the member.</span></span> <span data-ttu-id="42142-414">Příklad</span><span class="sxs-lookup"><span data-stu-id="42142-414">For example</span></span>
```csharp
class A
{
    void F() {
        i = 1;
    }

    int i = 0;
}
```
<span data-ttu-id="42142-415">Tady je platný pro `F` k odkazování na `i` dříve, než je deklarována.</span><span class="sxs-lookup"><span data-stu-id="42142-415">Here, it is valid for `F` to refer to `i` before it is declared.</span></span>

<span data-ttu-id="42142-416">V rámci oboru místní proměnná, je chyba kompilace k odkazování na místní proměnnou v textové pozici, která předchází *local_variable_declarator* lokální proměnné.</span><span class="sxs-lookup"><span data-stu-id="42142-416">Within the scope of a local variable, it is a compile-time error to refer to the local variable in a textual position that precedes the *local_variable_declarator* of the local variable.</span></span> <span data-ttu-id="42142-417">Příklad</span><span class="sxs-lookup"><span data-stu-id="42142-417">For example</span></span>
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

<span data-ttu-id="42142-418">V `F` výše uvedené metody, první přiřazení k `i` konkrétně neodkazuje na pole deklarovaná ve vnějším oboru.</span><span class="sxs-lookup"><span data-stu-id="42142-418">In the `F` method above, the first assignment to `i` specifically does not refer to the field declared in the outer scope.</span></span> <span data-ttu-id="42142-419">Místo toho se odkazuje na místní proměnné a je výsledkem chyba kompilace, protože textový předchází deklarace proměnné.</span><span class="sxs-lookup"><span data-stu-id="42142-419">Rather, it refers to the local variable and it results in a compile-time error because it textually precedes the declaration of the variable.</span></span> <span data-ttu-id="42142-420">V `G` metody, použití `j` v inicializátoru pro deklaraci `j` je platný, protože použití nepředchází *local_variable_declarator*.</span><span class="sxs-lookup"><span data-stu-id="42142-420">In the `G` method, the use of `j` in the initializer for the declaration of `j` is valid because the use does not precede the *local_variable_declarator*.</span></span> <span data-ttu-id="42142-421">V `H` metody, následné *local_variable_declarator* správně odkazuje na místní proměnná deklarovaná v předchozím kroku *local_variable_declarator* v rámci stejného  *local_variable_declaration*.</span><span class="sxs-lookup"><span data-stu-id="42142-421">In the `H` method, a subsequent *local_variable_declarator* correctly refers to a local variable declared in an earlier *local_variable_declarator* within the same *local_variable_declaration*.</span></span>

<span data-ttu-id="42142-422">Pravidla oboru místních proměnných slouží k zajištění, že význam názvu použít v kontextu výrazu je vždy stejné v rámci bloku.</span><span class="sxs-lookup"><span data-stu-id="42142-422">The scoping rules for local variables are designed to guarantee that the meaning of a name used in an expression context is always the same within a block.</span></span> <span data-ttu-id="42142-423">Pokud obor lokální proměnné můžete rozšířit jenom z jeho deklarace do konce bloku, v předchozím příkladu by první přiřazení přiřadit k proměnné instance a druhé přiřazení by přiřadit místní proměnnou, by mohl vést k chyby při kompilaci šlo novější změnu řazení příkazy bloku.</span><span class="sxs-lookup"><span data-stu-id="42142-423">If the scope of a local variable were to extend only from its declaration to the end of the block, then in the example above, the first assignment would assign to the instance variable and the second assignment would assign to the local variable, possibly leading to compile-time errors if the statements of the block were later to be rearranged.</span></span>

<span data-ttu-id="42142-424">Význam názvu v rámci bloku se může lišit v závislosti na kontextu, ve kterém se používá název.</span><span class="sxs-lookup"><span data-stu-id="42142-424">The meaning of a name within a block may differ based on the context in which the name is used.</span></span> <span data-ttu-id="42142-425">V příkladu</span><span class="sxs-lookup"><span data-stu-id="42142-425">In the example</span></span>
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
<span data-ttu-id="42142-426">název `A` se používá v kontextu výrazu k odkazování na místní proměnnou `A` a v kontextu typu odkazovat na třídu `A`.</span><span class="sxs-lookup"><span data-stu-id="42142-426">the name `A` is used in an expression context to refer to the local variable `A` and in a type context to refer to the class `A`.</span></span>

### <a name="name-hiding"></a><span data-ttu-id="42142-427">Skrytí názvu</span><span class="sxs-lookup"><span data-stu-id="42142-427">Name hiding</span></span>

<span data-ttu-id="42142-428">Obor entity obvykle zahrnuje další textem programu místa, než je deklarace entity.</span><span class="sxs-lookup"><span data-stu-id="42142-428">The scope of an entity typically encompasses more program text than the declaration space of the entity.</span></span> <span data-ttu-id="42142-429">Zejména oboru entita může obsahovat deklarace, které zavádějí nové prostory deklarace obsahující entity se stejným názvem.</span><span class="sxs-lookup"><span data-stu-id="42142-429">In particular, the scope of an entity may include declarations that introduce new declaration spaces containing entities of the same name.</span></span> <span data-ttu-id="42142-430">Takové deklarace způsobit subjektem původní stane ***skryté***.</span><span class="sxs-lookup"><span data-stu-id="42142-430">Such declarations cause the original entity to become ***hidden***.</span></span> <span data-ttu-id="42142-431">Naopak entity se říká, že ***viditelné*** Pokud není skrytý.</span><span class="sxs-lookup"><span data-stu-id="42142-431">Conversely, an entity is said to be ***visible*** when it is not hidden.</span></span>

<span data-ttu-id="42142-432">Skrývání názvů nastane, pokud obory překrývat prostřednictvím vnoření a kdy obory překrývat prostřednictvím dědičnosti.</span><span class="sxs-lookup"><span data-stu-id="42142-432">Name hiding occurs when scopes overlap through nesting and when scopes overlap through inheritance.</span></span> <span data-ttu-id="42142-433">Vlastnosti dva druhy skrytí jsou popsány v následujících částech.</span><span class="sxs-lookup"><span data-stu-id="42142-433">The characteristics of the two types of hiding are described in the following sections.</span></span>

#### <a name="hiding-through-nesting"></a><span data-ttu-id="42142-434">Skrytí prostřednictvím vnoření</span><span class="sxs-lookup"><span data-stu-id="42142-434">Hiding through nesting</span></span>

<span data-ttu-id="42142-435">Skrytí názvu prostřednictvím vnoření může nastat v důsledku vnořené obory názvů nebo typy v oborech názvů, v důsledku vnořené typy v rámci třídy nebo struktury a v důsledku parametr a místní deklarace proměnné.</span><span class="sxs-lookup"><span data-stu-id="42142-435">Name hiding through nesting can occur as a result of nesting namespaces or types within namespaces, as a result of nesting types within classes or structs, and as a result of parameter and local variable declarations.</span></span>

<span data-ttu-id="42142-436">V příkladu</span><span class="sxs-lookup"><span data-stu-id="42142-436">In the example</span></span>
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
<span data-ttu-id="42142-437">v rámci `F` proměnná instance, metoda `i` je skrytý místní proměnná `i`, ale v rámci `G` metody `i` stále odkazuje na proměnnou instance.</span><span class="sxs-lookup"><span data-stu-id="42142-437">within the `F` method, the instance variable `i` is hidden by the local variable `i`, but within the `G` method, `i` still refers to the instance variable.</span></span>

<span data-ttu-id="42142-438">Název ve vnitřním oboru je skryt název ve vnějším oboru, skryje všechny přetížené výskyty s tímto názvem.</span><span class="sxs-lookup"><span data-stu-id="42142-438">When a name in an inner scope hides a name in an outer scope, it hides all overloaded occurrences of that name.</span></span> <span data-ttu-id="42142-439">V příkladu</span><span class="sxs-lookup"><span data-stu-id="42142-439">In the example</span></span>
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
<span data-ttu-id="42142-440">volání `F(1)` vyvolá `F` deklarované v `Inner` protože všechny vnější výskyty `F` skryty pomocí vnitřní deklarace.</span><span class="sxs-lookup"><span data-stu-id="42142-440">the call `F(1)` invokes the `F` declared in `Inner` because all outer occurrences of `F` are hidden by the inner declaration.</span></span> <span data-ttu-id="42142-441">Ze stejného důvodu volání `F("Hello")` výsledkem chyba kompilace.</span><span class="sxs-lookup"><span data-stu-id="42142-441">For the same reason, the call `F("Hello")` results in a compile-time error.</span></span>

#### <a name="hiding-through-inheritance"></a><span data-ttu-id="42142-442">Skrytí prostřednictvím dědičnosti</span><span class="sxs-lookup"><span data-stu-id="42142-442">Hiding through inheritance</span></span>

<span data-ttu-id="42142-443">Skrytí názvu prostřednictvím dědičnosti vyvolá se v případě třídy nebo struktury předeklarovat názvy, které byly zděděny ze základních tříd.</span><span class="sxs-lookup"><span data-stu-id="42142-443">Name hiding through inheritance occurs when classes or structs redeclare names that were inherited from base classes.</span></span> <span data-ttu-id="42142-444">Tento typ skrývání názvů má jednu z následujících forem:</span><span class="sxs-lookup"><span data-stu-id="42142-444">This type of name hiding takes one of the following forms:</span></span>

*  <span data-ttu-id="42142-445">– Konstanta, pole, vlastnosti, události nebo typ byl zaveden ve třídě nebo struktuře skryje všechny členy základní třídy se stejným názvem.</span><span class="sxs-lookup"><span data-stu-id="42142-445">A constant, field, property, event, or type introduced in a class or struct hides all base class members with the same name.</span></span>
*  <span data-ttu-id="42142-446">Metody zavedené ve třídě nebo struktuře skryje všechny členy jiné metody základní třídy se stejným názvem a všechny metody základní třídy se stejným podpisem (název metody a počet parametrů, modifikátory a typy).</span><span class="sxs-lookup"><span data-stu-id="42142-446">A method introduced in a class or struct hides all non-method base class members with the same name, and all base class methods with the same signature (method name and parameter count, modifiers, and types).</span></span>
*  <span data-ttu-id="42142-447">Indexer zavedený ve třídě nebo struktuře skryje všechny základní třídy indexerů se stejným podpisem (počet parametrů a typů).</span><span class="sxs-lookup"><span data-stu-id="42142-447">An indexer introduced in a class or struct hides all base class indexers with the same signature (parameter count and types).</span></span>

<span data-ttu-id="42142-448">Pravidla pro deklarace operátoru ([operátory](classes.md#operators)) znemožnit odvozené třídy za účelem deklarovat operátor se stejným podpisem jako operátor v základní třídě.</span><span class="sxs-lookup"><span data-stu-id="42142-448">The rules governing operator declarations ([Operators](classes.md#operators)) make it impossible for a derived class to declare an operator with the same signature as an operator in a base class.</span></span> <span data-ttu-id="42142-449">Proto operátory nikdy skrýt mezi sebou.</span><span class="sxs-lookup"><span data-stu-id="42142-449">Thus, operators never hide one another.</span></span>

<span data-ttu-id="42142-450">Rozporu s skrytí názvu z vnějšího oboru, skrytí přístupový název ze zděděných oboru způsobí, že upozornění, aby oznámený.</span><span class="sxs-lookup"><span data-stu-id="42142-450">Contrary to hiding a name from an outer scope, hiding an accessible name from an inherited scope causes a warning to be reported.</span></span> <span data-ttu-id="42142-451">V příkladu</span><span class="sxs-lookup"><span data-stu-id="42142-451">In the example</span></span>
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
<span data-ttu-id="42142-452">deklarace `F` v `Derived` způsobí, že aby oznámený upozornění.</span><span class="sxs-lookup"><span data-stu-id="42142-452">the declaration of `F` in `Derived` causes a warning to be reported.</span></span> <span data-ttu-id="42142-453">Skrytí zděděného název není výslovně chybu, protože, který bude bránit samostatný vývoj základních tříd.</span><span class="sxs-lookup"><span data-stu-id="42142-453">Hiding an inherited name is specifically not an error, since that would preclude separate evolution of base classes.</span></span> <span data-ttu-id="42142-454">Například výše situace může mít přijít, protože na novější verzi `Base` zavedená `F` metodu, která nebyla k dispozici ve starší verzi této třídy.</span><span class="sxs-lookup"><span data-stu-id="42142-454">For example, the above situation might have come about because a later version of `Base` introduced an `F` method that wasn't present in an earlier version of the class.</span></span> <span data-ttu-id="42142-455">Výše uvedené situace je chyba, pak všechny změny provedené na základní třídu v knihovně tříd samostatně vyvíjených může potenciálně způsobit odvozené třídy stanou neplatnými.</span><span class="sxs-lookup"><span data-stu-id="42142-455">Had the above situation been an error, then any change made to a base class in a separately versioned class library could potentially cause derived classes to become invalid.</span></span>

<span data-ttu-id="42142-456">Toto upozornění způsobeno skrytí zděděného název lze odstranit pomocí `new` modifikátor:</span><span class="sxs-lookup"><span data-stu-id="42142-456">The warning caused by hiding an inherited name can be eliminated through use of the `new` modifier:</span></span>
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

<span data-ttu-id="42142-457">`new` Modifikátor znamená, že `F` v `Derived` je "nové", a je ve skutečnosti určen ke skrytí zděděného člena.</span><span class="sxs-lookup"><span data-stu-id="42142-457">The `new` modifier indicates that the `F` in `Derived` is "new", and that it is indeed intended to hide the inherited member.</span></span>

<span data-ttu-id="42142-458">Deklarace nový člen skrývá zděděný člen pouze v rámci oboru nového člena.</span><span class="sxs-lookup"><span data-stu-id="42142-458">A declaration of a new member hides an inherited member only within the scope of the new member.</span></span>

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

<span data-ttu-id="42142-459">V příkladu výše, deklarace `F` v `Derived` skryje `F` , který byl zděděn od `Base`, ale od nové `F` v `Derived` má privátní přístup, jeho rozsah nerozšiřuje `MoreDerived` .</span><span class="sxs-lookup"><span data-stu-id="42142-459">In the example above, the declaration of `F` in `Derived` hides the `F` that was inherited from `Base`, but since the new `F` in `Derived` has private access, its scope does not extend to `MoreDerived`.</span></span> <span data-ttu-id="42142-460">Proto volání `F()` v `MoreDerived.G` je platný a vyvolá `Base.F`.</span><span class="sxs-lookup"><span data-stu-id="42142-460">Thus, the call `F()` in `MoreDerived.G` is valid and will invoke `Base.F`.</span></span>

## <a name="namespace-and-type-names"></a><span data-ttu-id="42142-461">Namespace a zadejte názvy</span><span class="sxs-lookup"><span data-stu-id="42142-461">Namespace and type names</span></span>

<span data-ttu-id="42142-462">Vyžadovat v několika kontextech v programu v jazyce C# *namespace_name* nebo *type_name* zadání.</span><span class="sxs-lookup"><span data-stu-id="42142-462">Several contexts in a C# program require a *namespace_name* or a *type_name* to be specified.</span></span>

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

<span data-ttu-id="42142-463">A *namespace_name* je *namespace_or_type_name* , který odkazuje na obor názvů.</span><span class="sxs-lookup"><span data-stu-id="42142-463">A *namespace_name* is a *namespace_or_type_name* that refers to a namespace.</span></span> <span data-ttu-id="42142-464">Po řešení, jak je popsáno níže, *namespace_or_type_name* z *namespace_name* musí odkazovat na názvový prostor, nebo v opačném případě dojde k chybě kompilace.</span><span class="sxs-lookup"><span data-stu-id="42142-464">Following resolution as described below, the *namespace_or_type_name* of a *namespace_name* must refer to a namespace, or otherwise a compile-time error occurs.</span></span> <span data-ttu-id="42142-465">Žádné argumenty typu ([argumenty typu](types.md#type-arguments)) mohou být přítomny v *namespace_name* (pouze pro typy mohou mít argumenty typu).</span><span class="sxs-lookup"><span data-stu-id="42142-465">No type arguments ([Type arguments](types.md#type-arguments)) can be present in a *namespace_name* (only types can have type arguments).</span></span>

<span data-ttu-id="42142-466">A *type_name* je *namespace_or_type_name* odkazující k typu.</span><span class="sxs-lookup"><span data-stu-id="42142-466">A *type_name* is a *namespace_or_type_name* that refers to a type.</span></span> <span data-ttu-id="42142-467">Po řešení, jak je popsáno níže, *namespace_or_type_name* z *type_name* musí odkazovat na typ, nebo v opačném případě dojde k chybě kompilace.</span><span class="sxs-lookup"><span data-stu-id="42142-467">Following resolution as described below, the *namespace_or_type_name* of a *type_name* must refer to a type, or otherwise a compile-time error occurs.</span></span>

<span data-ttu-id="42142-468">Pokud *namespace_or_type_name* kvalifikovaný alias-členem jeho význam se, jak je popsáno v [Namespace alias kvalifikátory](namespaces.md#namespace-alias-qualifiers).</span><span class="sxs-lookup"><span data-stu-id="42142-468">If the *namespace_or_type_name* is a qualified-alias-member its meaning is as described in [Namespace alias qualifiers](namespaces.md#namespace-alias-qualifiers).</span></span> <span data-ttu-id="42142-469">V opačném případě *namespace_or_type_name* obsahuje jednu ze čtyř podob:</span><span class="sxs-lookup"><span data-stu-id="42142-469">Otherwise, a *namespace_or_type_name* has one of four forms:</span></span>

*  `I`
*  `I<A1, ..., Ak>`
*  `N.I`
*  `N.I<A1, ..., Ak>`

<span data-ttu-id="42142-470">kde `I` je jediným identifikátorem `N` je *namespace_or_type_name* a `<A1, ..., Ak>` je volitelný *type_argument_list*.</span><span class="sxs-lookup"><span data-stu-id="42142-470">where `I` is a single identifier, `N` is a *namespace_or_type_name* and `<A1, ..., Ak>` is an optional *type_argument_list*.</span></span> <span data-ttu-id="42142-471">Pokud ne *type_argument_list* je zadán, vezměte v úvahu `k` být nula.</span><span class="sxs-lookup"><span data-stu-id="42142-471">When no *type_argument_list* is specified, consider `k` to be zero.</span></span>

<span data-ttu-id="42142-472">Význam *namespace_or_type_name* je stanoven následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="42142-472">The meaning of a *namespace_or_type_name* is determined as follows:</span></span>

*   <span data-ttu-id="42142-473">Pokud *namespace_or_type_name* má formu `I` nebo formuláře `I<A1, ..., Ak>`:</span><span class="sxs-lookup"><span data-stu-id="42142-473">If the *namespace_or_type_name* is of the form `I` or of the form `I<A1, ..., Ak>`:</span></span>
    * <span data-ttu-id="42142-474">Pokud `K` je nula a *namespace_or_type_name* se zobrazí v deklaraci obecné metody ([metody](classes.md#methods)) a pokud tato deklarace obsahuje parametr typu ([typu Parametry](classes.md#type-parameters)) s názvem `I`, pak bude *namespace_or_type_name* odkazuje na parametr typu.</span><span class="sxs-lookup"><span data-stu-id="42142-474">If `K` is zero and the *namespace_or_type_name* appears within a generic method declaration ([Methods](classes.md#methods)) and if that declaration includes a type parameter ([Type parameters](classes.md#type-parameters)) with name `I`, then the *namespace_or_type_name* refers to that type parameter.</span></span>
    * <span data-ttu-id="42142-475">Jinak, pokud *namespace_or_type_name* se zobrazí v rámci deklarace typu a potom pro každý typ instance `T` ([typ instance](classes.md#the-instance-type)) začíná s typem instance daného typu deklarace a budete pokračovat s typem instance deklaraci nadřazené třídu nebo strukturu (pokud existuje):</span><span class="sxs-lookup"><span data-stu-id="42142-475">Otherwise, if the *namespace_or_type_name* appears within a type declaration, then for each instance type `T` ([The instance type](classes.md#the-instance-type)), starting with the instance type of that type declaration and continuing with the instance type of each enclosing class or struct declaration (if any):</span></span>
        * <span data-ttu-id="42142-476">Pokud `K` je nula a deklarace `T` obsahuje parametr typu s názvem `I`, pak bude *namespace_or_type_name* odkazuje na parametr typu.</span><span class="sxs-lookup"><span data-stu-id="42142-476">If `K` is zero and the declaration of `T` includes a type parameter with name `I`, then the *namespace_or_type_name* refers to that type parameter.</span></span>
        * <span data-ttu-id="42142-477">Jinak, pokud *namespace_or_type_name* se zobrazí v těle deklarace typu a `T` nebo jakýkoli z jeho základních typů obsahuje vnořené přístupné typu s názvem `I` a `K` parametry typu , pak bude *namespace_or_type_name* odkazuje na tento typ vytvořený s argumenty daného typu.</span><span class="sxs-lookup"><span data-stu-id="42142-477">Otherwise, if the *namespace_or_type_name* appears within the body of the type declaration, and `T` or any of its base types contain a nested accessible type having name `I` and `K` type parameters, then the *namespace_or_type_name* refers to that type constructed with the given type arguments.</span></span> <span data-ttu-id="42142-478">Pokud existuje více než jeden takový typ, je vybraný typ deklarovaný v rámci více odvozeného typu.</span><span class="sxs-lookup"><span data-stu-id="42142-478">If there is more than one such type, the type declared within the more derived type is selected.</span></span> <span data-ttu-id="42142-479">Všimněte si, že členové bez typu (konstanty, pole, metody, vlastnosti, indexery, operátory, konstruktory instancí, destruktory a statické konstruktory) a členy typů s různým počtem parametrů typu jsou ignorovány při určování význam *namespace_or_type_name*.</span><span class="sxs-lookup"><span data-stu-id="42142-479">Note that non-type members (constants, fields, methods, properties, indexers, operators, instance constructors, destructors, and static constructors) and type members with a different number of type parameters are ignored when determining the meaning of the *namespace_or_type_name*.</span></span>
    * <span data-ttu-id="42142-480">Pokud byly úspěšné, pak pro každý obor názvů v předchozích krocích `N`začíná s oborem názvů, ve kterém *namespace_or_type_name* dojde, pokračujte v každé nadřazeného oboru názvů (pokud existuje) a končící globální obor názvů následující kroky jsou vyhodnocen, dokud se entita nachází:</span><span class="sxs-lookup"><span data-stu-id="42142-480">If the previous steps were unsuccessful then, for each namespace `N`, starting with the namespace in which the *namespace_or_type_name* occurs, continuing with each enclosing namespace (if any), and ending with the global namespace, the following steps are evaluated until an entity is located:</span></span>
        * <span data-ttu-id="42142-481">Pokud `K` je nula a `I` je název oboru názvů v `N`, pak:</span><span class="sxs-lookup"><span data-stu-id="42142-481">If `K` is zero and `I` is the name of a namespace in `N`, then:</span></span>
            * <span data-ttu-id="42142-482">Pokud umístění ve kterém *namespace_or_type_name* dojde k není uzavřen v deklaraci oboru názvů pro `N` a obsahuje deklaraci oboru názvů *extern_alias_directive* nebo *using_alias_directive* , která přidruží název `I` s oborem názvů nebo typ, pak bude *namespace_or_type_name* je nejednoznačný a dojde k chybě kompilace.</span><span class="sxs-lookup"><span data-stu-id="42142-482">If the location where the *namespace_or_type_name* occurs is enclosed by a namespace declaration for `N` and the namespace declaration contains an *extern_alias_directive* or *using_alias_directive* that associates the name `I` with a namespace or type, then the *namespace_or_type_name* is ambiguous and a compile-time error occurs.</span></span>
            * <span data-ttu-id="42142-483">V opačném případě *namespace_or_type_name* odkazuje na obor názvů s názvem `I` v `N`.</span><span class="sxs-lookup"><span data-stu-id="42142-483">Otherwise, the *namespace_or_type_name* refers to the namespace named `I` in `N`.</span></span>
        * <span data-ttu-id="42142-484">Jinak, pokud `N` obsahuje přístupného typu s názvem `I` a `K` parametry typu, pak:</span><span class="sxs-lookup"><span data-stu-id="42142-484">Otherwise, if `N` contains an accessible type having name `I` and `K` type parameters, then:</span></span>
            * <span data-ttu-id="42142-485">Pokud `K` je nula a umístění, kde *namespace_or_type_name* dojde k není uzavřen v deklaraci oboru názvů pro `N` a obsahuje deklaraci oboru názvů *extern_alias_directive*  nebo *using_alias_directive* , která přidruží název `I` s oborem názvů nebo typ, pak bude *namespace_or_type_name* je nejednoznačný a kompilace dojde k chybě.</span><span class="sxs-lookup"><span data-stu-id="42142-485">If `K` is zero and the location where the *namespace_or_type_name* occurs is enclosed by a namespace declaration for `N` and the namespace declaration contains an *extern_alias_directive* or *using_alias_directive* that associates the name `I` with a namespace or type, then the *namespace_or_type_name* is ambiguous and a compile-time error occurs.</span></span>
            * <span data-ttu-id="42142-486">V opačném případě *namespace_or_type_name* odkazuje na typ vytvořený s argumenty daného typu.</span><span class="sxs-lookup"><span data-stu-id="42142-486">Otherwise, the *namespace_or_type_name* refers to the type constructed with the given type arguments.</span></span>
        * <span data-ttu-id="42142-487">Jinak, pokud umístění ve kterém *namespace_or_type_name* dojde k není uzavřen v deklaraci oboru názvů pro `N`:</span><span class="sxs-lookup"><span data-stu-id="42142-487">Otherwise, if the location where the *namespace_or_type_name* occurs is enclosed by a namespace declaration for `N`:</span></span>
            * <span data-ttu-id="42142-488">Pokud `K` je nula a obsahuje deklaraci oboru názvů *extern_alias_directive* nebo *using_alias_directive* , která přidruží název `I` s importovaným oborem názvů nebo typ, pak bude *namespace_or_type_name* odkazuje na tento obor názvů nebo typ.</span><span class="sxs-lookup"><span data-stu-id="42142-488">If `K` is zero and the namespace declaration contains an *extern_alias_directive* or *using_alias_directive* that associates the name `I` with an imported namespace or type, then the *namespace_or_type_name* refers to that namespace or type.</span></span>
            * <span data-ttu-id="42142-489">Jinak, pokud deklarace oborů názvů a typ importované tímto seznamem *using_namespace_directive*s a *using_alias_directive*s deklarace oboru názvů obsahovat přesně jeden přístupný typ. s názvem `I` a `K` parametry typu, pak bude *namespace_or_type_name* odkazuje na tento typ vytvořený s argumenty daného typu.</span><span class="sxs-lookup"><span data-stu-id="42142-489">Otherwise, if the namespaces and type declarations imported by the *using_namespace_directive*s and *using_alias_directive*s of the namespace declaration contain exactly one accessible type having name `I` and `K` type parameters, then the *namespace_or_type_name* refers to that type constructed with the given type arguments.</span></span>
            * <span data-ttu-id="42142-490">Jinak, pokud deklarace oborů názvů a typ importované tímto seznamem *using_namespace_directive*s a *using_alias_directive*s deklarace oboru názvů obsahovat více než jeden přístupný typ. s názvem `I` a `K` parametry typu, pak bude *namespace_or_type_name* je nejednoznačný a dojde k chybě.</span><span class="sxs-lookup"><span data-stu-id="42142-490">Otherwise, if the namespaces and type declarations imported by the *using_namespace_directive*s and *using_alias_directive*s of the namespace declaration contain more than one accessible type having name `I` and `K` type parameters, then the *namespace_or_type_name* is ambiguous and an error occurs.</span></span>
    * <span data-ttu-id="42142-491">V opačném případě *namespace_or_type_name* je nedefinovaný a dojde k chybě kompilace.</span><span class="sxs-lookup"><span data-stu-id="42142-491">Otherwise, the *namespace_or_type_name* is undefined and a compile-time error occurs.</span></span>
*  <span data-ttu-id="42142-492">V opačném případě *namespace_or_type_name* má formu `N.I` nebo formuláře `N.I<A1, ..., Ak>`.</span><span class="sxs-lookup"><span data-stu-id="42142-492">Otherwise, the *namespace_or_type_name* is of the form `N.I` or of the form `N.I<A1, ..., Ak>`.</span></span> <span data-ttu-id="42142-493">`N` jako první vyřeší *namespace_or_type_name*.</span><span class="sxs-lookup"><span data-stu-id="42142-493">`N` is first resolved as a *namespace_or_type_name*.</span></span> <span data-ttu-id="42142-494">Pokud se rozlišení `N` neproběhne úspěšně, dojde k chybě kompilace.</span><span class="sxs-lookup"><span data-stu-id="42142-494">If the resolution of `N` is not successful, a compile-time error occurs.</span></span> <span data-ttu-id="42142-495">V opačném případě `N.I` nebo `N.I<A1, ..., Ak>` vyřešen následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="42142-495">Otherwise, `N.I` or `N.I<A1, ..., Ak>` is resolved as follows:</span></span>
    * <span data-ttu-id="42142-496">Pokud `K` je nula a `N` odkazuje na obor názvů a `N` obsahuje vnořené oboru názvů s názvem `I`, pak bude *namespace_or_type_name* odkazuje na tomto vnořené oboru názvů.</span><span class="sxs-lookup"><span data-stu-id="42142-496">If `K` is zero and `N` refers to a namespace and `N` contains a nested namespace with name `I`, then the *namespace_or_type_name* refers to that nested namespace.</span></span>
    * <span data-ttu-id="42142-497">Jinak, pokud `N` odkazuje na obor názvů a `N` obsahuje přístupného typu s názvem `I` a `K` parametry typu, pak bude *namespace_or_type_name* odkazuje na tento typ. sestavit s argumenty daného typu.</span><span class="sxs-lookup"><span data-stu-id="42142-497">Otherwise, if `N` refers to a namespace and `N` contains an accessible type having name `I` and `K` type parameters, then the *namespace_or_type_name* refers to that type constructed with the given type arguments.</span></span>
    * <span data-ttu-id="42142-498">Jinak, pokud `N` odkazuje na typ třídy nebo struktury (pravděpodobně konstruovaný) a `N` nebo některý z jeho základních tříd obsahovat vnořené přístupné typu s názvem `I` a `K` parametry typu, pak bude *obor názvů _or_type_name* odkazuje na tento typ vytvořený s argumenty daného typu.</span><span class="sxs-lookup"><span data-stu-id="42142-498">Otherwise, if `N` refers to a (possibly constructed) class or struct type and `N` or any of its base classes contain a nested accessible type having name `I` and `K` type parameters, then the *namespace_or_type_name* refers to that type constructed with the given type arguments.</span></span> <span data-ttu-id="42142-499">Pokud existuje více než jeden takový typ, je vybraný typ deklarovaný v rámci více odvozeného typu.</span><span class="sxs-lookup"><span data-stu-id="42142-499">If there is more than one such type, the type declared within the more derived type is selected.</span></span> <span data-ttu-id="42142-500">Všimněte si, že pokud význam `N.I` určen jako součást řešení specifikace základní třídy `N` pak přímé základní třídy `N` se považuje za objekt ([základních tříd](classes.md#base-classes)).</span><span class="sxs-lookup"><span data-stu-id="42142-500">Note that if the meaning of `N.I` is being determined as part of resolving the base class specification of `N` then the direct base class of `N` is considered to be object ([Base classes](classes.md#base-classes)).</span></span>
    * <span data-ttu-id="42142-501">V opačném případě `N.I` je neplatný *namespace_or_type_name*, a dojde k chybě kompilace.</span><span class="sxs-lookup"><span data-stu-id="42142-501">Otherwise, `N.I` is an invalid *namespace_or_type_name*, and a compile-time error occurs.</span></span>

<span data-ttu-id="42142-502">A *namespace_or_type_name* smí odkazovat na statickou třídu ([statické třídy](classes.md#static-classes)) pouze v případě</span><span class="sxs-lookup"><span data-stu-id="42142-502">A *namespace_or_type_name* is permitted to reference a static class ([Static classes](classes.md#static-classes)) only if</span></span>

*  <span data-ttu-id="42142-503">*Namespace_or_type_name* je `T` v *namespace_or_type_name* formuláře `T.I`, nebo</span><span class="sxs-lookup"><span data-stu-id="42142-503">The *namespace_or_type_name* is the `T` in a *namespace_or_type_name* of the form `T.I`, or</span></span>
*  <span data-ttu-id="42142-504">*Namespace_or_type_name* je `T` v *typeof_expression* ([seznamy argumentů](expressions.md#argument-lists)1) ve formátu `typeof(T)`.</span><span class="sxs-lookup"><span data-stu-id="42142-504">The *namespace_or_type_name* is the `T` in a *typeof_expression* ([Argument lists](expressions.md#argument-lists)1) of the form `typeof(T)`.</span></span>

### <a name="fully-qualified-names"></a><span data-ttu-id="42142-505">Plně kvalifikované názvy</span><span class="sxs-lookup"><span data-stu-id="42142-505">Fully qualified names</span></span>

<span data-ttu-id="42142-506">Každý obor názvů a typ má ***plně kvalifikovaný název***, který jednoznačně identifikuje obor názvů nebo typ mimo všechny ostatní.</span><span class="sxs-lookup"><span data-stu-id="42142-506">Every namespace and type has a ***fully qualified name***, which uniquely identifies the namespace or type amongst all others.</span></span> <span data-ttu-id="42142-507">Plně kvalifikovaný název oboru názvů nebo typ `N` je stanoven následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="42142-507">The fully qualified name of a namespace or type `N` is determined as follows:</span></span>

*  <span data-ttu-id="42142-508">Pokud `N` patří do globálního oboru názvů a jeho plně kvalifikovaný název je `N`.</span><span class="sxs-lookup"><span data-stu-id="42142-508">If `N` is a member of the global namespace, its fully qualified name is `N`.</span></span>
*  <span data-ttu-id="42142-509">V opačném případě je jeho plně kvalifikovaný název `S.N`, kde `S` je plně kvalifikovaný název oboru názvů nebo typ, ve kterém `N` je deklarována.</span><span class="sxs-lookup"><span data-stu-id="42142-509">Otherwise, its fully qualified name is `S.N`, where `S` is the fully qualified name of the namespace or type in which `N` is declared.</span></span>

<span data-ttu-id="42142-510">Jinými slovy, plně kvalifikovaný název `N` je kompletní hierarchické cesty identifikátorů, které vedly k `N`od globálního oboru názvů.</span><span class="sxs-lookup"><span data-stu-id="42142-510">In other words, the fully qualified name of `N` is the complete hierarchical path of identifiers that lead to `N`, starting from the global namespace.</span></span> <span data-ttu-id="42142-511">Protože každý člen obor názvů nebo typ musí mít jedinečný název, následuje plně kvalifikovaný název oboru názvů nebo typ je vždy jedinečný.</span><span class="sxs-lookup"><span data-stu-id="42142-511">Because every member of a namespace or type must have a unique name, it follows that the fully qualified name of a namespace or type is always unique.</span></span>

<span data-ttu-id="42142-512">Následující příklad ukazuje několik deklarací oboru názvů a typ spolu s jejich přidružených plně kvalifikovaných názvů.</span><span class="sxs-lookup"><span data-stu-id="42142-512">The example below shows several namespace and type declarations along with their associated fully qualified names.</span></span>
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

## <a name="automatic-memory-management"></a><span data-ttu-id="42142-513">Automatická správa paměti</span><span class="sxs-lookup"><span data-stu-id="42142-513">Automatic memory management</span></span>

<span data-ttu-id="42142-514">C# používá Automatická správa paměti, která vaši vývojáři z ručně přidělování a uvolňování paměti obsazena objekty.</span><span class="sxs-lookup"><span data-stu-id="42142-514">C# employs automatic memory management, which frees developers from manually allocating and freeing the memory occupied by objects.</span></span> <span data-ttu-id="42142-515">Zásady správy paměti automatické implementují ***systému uvolňování paměti***.</span><span class="sxs-lookup"><span data-stu-id="42142-515">Automatic memory management policies are implemented by a ***garbage collector***.</span></span> <span data-ttu-id="42142-516">Životní cyklus správy paměti objektu vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="42142-516">The memory management life cycle of an object is as follows:</span></span>

1. <span data-ttu-id="42142-517">Při vytvoření objektu se pro něj přidělené paměti, spusťte konstruktoru a objekt považován za provozu.</span><span class="sxs-lookup"><span data-stu-id="42142-517">When the object is created, memory is allocated for it, the constructor is run, and the object is considered live.</span></span>
2. <span data-ttu-id="42142-518">Pokud objekt nebo libovolné části, nemůže mít přístup všechny možné pokračování provádění, než spuštění destruktory, objekt je považován za už používá a bude vhodné pro zničení.</span><span class="sxs-lookup"><span data-stu-id="42142-518">If the object, or any part of it, cannot be accessed by any possible continuation of execution, other than the running of destructors, the object is considered no longer in use, and it becomes eligible for destruction.</span></span> <span data-ttu-id="42142-519">Kompilátor jazyka C# a systému uvolňování paměti můžou rozhodnout pro analýzu kódu k určení, které odkazuje na objekt lze v budoucnu.</span><span class="sxs-lookup"><span data-stu-id="42142-519">The C# compiler and the garbage collector may choose to analyze code to determine which references to an object may be used in the future.</span></span> <span data-ttu-id="42142-520">Například pokud místní proměnná, která je v oboru je pouze existující odkaz na objekt, ale tuto místní proměnnou se nikdy označuje všechny možné pokračování provádění aktuálního spuštění bodu v postupu, uvolňování může (ale není potřeba) považovat objektu jako už používá.</span><span class="sxs-lookup"><span data-stu-id="42142-520">For instance, if a local variable that is in scope is the only existing reference to an object, but that local variable is never referred to in any possible continuation of execution from the current execution point in the procedure, the garbage collector may (but is not required to) treat the object as no longer in use.</span></span>
3. <span data-ttu-id="42142-521">Po nárok zničení objektu na některé později tento parametr nezadáte čas destruktoru ([destruktory](classes.md#destructors)) (pokud existuje) pro spuštění objektu.</span><span class="sxs-lookup"><span data-stu-id="42142-521">Once the object is eligible for destruction, at some unspecified later time the destructor ([Destructors](classes.md#destructors)) (if any) for the object is run.</span></span> <span data-ttu-id="42142-522">Za normálních okolností destruktor objektu se spustí pouze jednou, ale specifický pro implementaci rozhraní API mohou povolit toto chování k přepsání.</span><span class="sxs-lookup"><span data-stu-id="42142-522">Under normal circumstances the destructor for the object is run once only, though implementation-specific APIs may allow this behavior to be overridden.</span></span>
4. <span data-ttu-id="42142-523">Po spuštění destruktoru objektu, pokud daný objekt nebo libovolné části, není přístupná všechny možné pokračování provádění, včetně spuštění destruktory, objekt je považován za nedostupné a objekt se stane nárok na kolekci.</span><span class="sxs-lookup"><span data-stu-id="42142-523">Once the destructor for an object is run, if that object, or any part of it, cannot be accessed by any possible continuation of execution, including the running of destructors, the object is considered inaccessible and the object becomes eligible for collection.</span></span>
5. <span data-ttu-id="42142-524">A konečně někdy po objekt stane nárok na kolekci, systému uvolňování paměti uvolní paměti spojený s tímto objektem.</span><span class="sxs-lookup"><span data-stu-id="42142-524">Finally, at some time after the object becomes eligible for collection, the garbage collector frees the memory associated with that object.</span></span>

<span data-ttu-id="42142-525">Uvolňování paměti uchovává informace o použití objektu a využívá tyto informace, aby paměti rozhodnutích týkajících se správy, jako je například umístění v paměti vyhledejte nově vytvořený objekt, když se přesunout objekt a když objekt již není používán nebo nedostupný.</span><span class="sxs-lookup"><span data-stu-id="42142-525">The garbage collector maintains information about object usage, and uses this information to make memory management decisions, such as where in memory to locate a newly created object, when to relocate an object, and when an object is no longer in use or inaccessible.</span></span>

<span data-ttu-id="42142-526">Stejně jako v jiných jazycích, které se předpokládá existenci systému uvolňování paměti C# je navržený tak, aby systému uvolňování paměti může implementovat řadu různých zásad správy paměti.</span><span class="sxs-lookup"><span data-stu-id="42142-526">Like other languages that assume the existence of a garbage collector, C# is designed so that the garbage collector may implement a wide range of memory management policies.</span></span> <span data-ttu-id="42142-527">Například C# nevyžaduje, aby spuštění destruktory nebo shromažďovat objekty jako přicházejí nebo, že destruktory spustit v libovolném pořadí nebo v libovolném konkrétním vlákně.</span><span class="sxs-lookup"><span data-stu-id="42142-527">For instance, C# does not require that destructors be run or that objects be collected as soon as they are eligible, or that destructors be run in any particular order, or on any particular thread.</span></span>

<span data-ttu-id="42142-528">Řídí chování systému uvolňování paměti, do určité míry, prostřednictvím statických metod ve třídě `System.GC`.</span><span class="sxs-lookup"><span data-stu-id="42142-528">The behavior of the garbage collector can be controlled, to some degree, via static methods on the class `System.GC`.</span></span> <span data-ttu-id="42142-529">Tato třída slouží k vyžádání kolekce pravděpodobnější, destruktory spuštění (nebo nespuštěno) a tak dále.</span><span class="sxs-lookup"><span data-stu-id="42142-529">This class can be used to request a collection to occur, destructors to be run (or not run), and so forth.</span></span>

<span data-ttu-id="42142-530">Od systému uvolňování paměti může široké šířky při rozhodování, kdy se mají shromáždit objekty a spusťte destruktory, vyhovující implementace může vytvořit výstup, který se liší od, který je znázorněno v následujícím kódu.</span><span class="sxs-lookup"><span data-stu-id="42142-530">Since the garbage collector is allowed wide latitude in deciding when to collect objects and run destructors, a conforming implementation may produce output that differs from that shown by the following code.</span></span> <span data-ttu-id="42142-531">Program</span><span class="sxs-lookup"><span data-stu-id="42142-531">The program</span></span>
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
<span data-ttu-id="42142-532">vytvoří instanci třídy `A` a instance třídy `B`.</span><span class="sxs-lookup"><span data-stu-id="42142-532">creates an instance of class `A` and an instance of class `B`.</span></span> <span data-ttu-id="42142-533">Tyto objekty stane oprávněnými pro uvolnění při proměnné `b` je přiřazena hodnota `null`, protože se po uplynutí této doby je možné pro uživatelem zapsaný kód pro přístup k nim.</span><span class="sxs-lookup"><span data-stu-id="42142-533">These objects become eligible for garbage collection when the variable `b` is assigned the value `null`, since after this time it is impossible for any user-written code to access them.</span></span> <span data-ttu-id="42142-534">Výstup může být buď</span><span class="sxs-lookup"><span data-stu-id="42142-534">The output could be either</span></span>
```
Destruct instance of A
Destruct instance of B
```
<span data-ttu-id="42142-535">or</span><span class="sxs-lookup"><span data-stu-id="42142-535">or</span></span>
```
Destruct instance of B
Destruct instance of A
```
<span data-ttu-id="42142-536">protože jazyk ukládá bez omezení na pořadí, ve kterém jsou objekty uvolněna z paměti.</span><span class="sxs-lookup"><span data-stu-id="42142-536">because the language imposes no constraints on the order in which objects are garbage collected.</span></span>

<span data-ttu-id="42142-537">V případech, malý může být důležité rozdíl mezi "nárok zničení" a "nárok na kolekci".</span><span class="sxs-lookup"><span data-stu-id="42142-537">In subtle cases, the distinction between "eligible for destruction" and "eligible for collection" can be important.</span></span> <span data-ttu-id="42142-538">Například</span><span class="sxs-lookup"><span data-stu-id="42142-538">For example,</span></span>
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

<span data-ttu-id="42142-539">Ve výše uvedené program, pokud se rozhodne systému uvolňování paměti ke spuštění destruktor `A` před destruktor `B`, může to být výstup tohoto programu:</span><span class="sxs-lookup"><span data-stu-id="42142-539">In the above program, if the garbage collector chooses to run the destructor of `A` before the destructor of `B`, then the output of this program might be:</span></span>
```
Destruct instance of A
Destruct instance of B
A.F
RefA is not null
```

<span data-ttu-id="42142-540">Všimněte si, že i když instance `A` nebyl používán a `A`společnosti byl spuštěn destruktor, je stále možné metody `A` (v tomto případě `F`) k volání z jiné destruktor.</span><span class="sxs-lookup"><span data-stu-id="42142-540">Note that although the instance of `A` was not in use and `A`'s destructor was run, it is still possible for methods of `A` (in this case, `F`) to be called from another destructor.</span></span> <span data-ttu-id="42142-541">Také si všimněte, že může způsobit spuštění destruktoru objektu autentický z hlavní program znovu.</span><span class="sxs-lookup"><span data-stu-id="42142-541">Also, note that running of a destructor may cause an object to become usable from the mainline program again.</span></span> <span data-ttu-id="42142-542">V takovém případě spouštění `B`je destruktor způsobila instance `A` , který byl dříve nečinnosti se zpřístupní z živého odkazu `Test.RefA`.</span><span class="sxs-lookup"><span data-stu-id="42142-542">In this case, the running of `B`'s destructor caused an instance of `A` that was previously not in use to become accessible from the live reference `Test.RefA`.</span></span> <span data-ttu-id="42142-543">Po volání `WaitForPendingFinalizers`, instanci `B` má nárok na kolekce, ale instance `A` není k dispozici, protože odkaz `Test.RefA`.</span><span class="sxs-lookup"><span data-stu-id="42142-543">After the call to `WaitForPendingFinalizers`, the instance of `B` is eligible for collection, but the instance of `A` is not, because of the reference `Test.RefA`.</span></span>

<span data-ttu-id="42142-544">Abyste předešli zmatení a neočekávané chování, je obecně vhodné pro destruktory provádět čištění jenom s daty uloženými v jejich vlastní polím objektu a nechcete provádět všechny akce v odkazované objekty nebo statická pole.</span><span class="sxs-lookup"><span data-stu-id="42142-544">To avoid confusion and unexpected behavior, it is generally a good idea for destructors to only perform cleanup on data stored in their object's own fields, and not to perform any actions on referenced objects or static fields.</span></span>

<span data-ttu-id="42142-545">Se o alternativu k použití destruktorů umožňuje implementovat třídu `System.IDisposable` rozhraní.</span><span class="sxs-lookup"><span data-stu-id="42142-545">An alternative to using destructors is to let a class implement the `System.IDisposable` interface.</span></span> <span data-ttu-id="42142-546">To umožňuje klientovi k určení toho, kdy k uvolnění prostředků objektu, obvykle tak přístup k objektu jako prostředek v objektu `using` – příkaz ([pomocí příkazu](statements.md#the-using-statement)).</span><span class="sxs-lookup"><span data-stu-id="42142-546">This allows the client of the object to determine when to release the resources of the object, typically by accessing the object as a resource in a `using` statement ([The using statement](statements.md#the-using-statement)).</span></span>

## <a name="execution-order"></a><span data-ttu-id="42142-547">Pořadí provádění</span><span class="sxs-lookup"><span data-stu-id="42142-547">Execution order</span></span>

<span data-ttu-id="42142-548">Spuštění programu v jazyce C# pokračuje tak, aby vedlejší účinky jednotlivých provádění vlákna jsou zachovány při provádění kritické body.</span><span class="sxs-lookup"><span data-stu-id="42142-548">Execution of a C# program proceeds such that the side effects of each executing thread are preserved at critical execution points.</span></span> <span data-ttu-id="42142-549">A ***vedlejší účinek*** je definován jako čtení nebo zápisu pole s modifikátorem volatile, zápis stálé proměnné, zápis do externího prostředku a vyvolává výjimku.</span><span class="sxs-lookup"><span data-stu-id="42142-549">A ***side effect*** is defined as a read or write of a volatile field, a write to a non-volatile variable, a write to an external resource, and the throwing of an exception.</span></span> <span data-ttu-id="42142-550">Spuštění nutné pro body, ve kterých musí být zachovány pořadí těchto vedlejší účinky jsou odkazy na pole s modifikátorem volatile ([pole s modifikátorem Volatile](classes.md#volatile-fields)), `lock` příkazy ([příkaz lock](statements.md#the-lock-statement)), a vytváření a ukončení vlákna.</span><span class="sxs-lookup"><span data-stu-id="42142-550">The critical execution points at which the order of these side effects must be preserved are references to volatile fields ([Volatile fields](classes.md#volatile-fields)), `lock` statements ([The lock statement](statements.md#the-lock-statement)), and thread creation and termination.</span></span> <span data-ttu-id="42142-551">Prostředí pro spuštění je zdarma, chcete-li změnit pořadí provádění programu v C# v souladu s následujícími omezeními:</span><span class="sxs-lookup"><span data-stu-id="42142-551">The execution environment is free to change the order of execution of a C# program, subject to the following constraints:</span></span>

*  <span data-ttu-id="42142-552">Závislost na data se zachovají v rámci vlákno provádění.</span><span class="sxs-lookup"><span data-stu-id="42142-552">Data dependence is preserved within a thread of execution.</span></span> <span data-ttu-id="42142-553">To znamená hodnotu každé proměnné je vypočítán jako by všechny příkazy ve vlákně byly provedeny v původní pořadí programu.</span><span class="sxs-lookup"><span data-stu-id="42142-553">That is, the value of each variable is computed as if all statements in the thread were executed in original program order.</span></span>
*  <span data-ttu-id="42142-554">Inicializace pořadí pravidel jsou zachovány ([inicializace pole](classes.md#field-initialization) a [proměnné inicializátory](classes.md#variable-initializers)).</span><span class="sxs-lookup"><span data-stu-id="42142-554">Initialization ordering rules are preserved ([Field initialization](classes.md#field-initialization) and [Variable initializers](classes.md#variable-initializers)).</span></span>
*  <span data-ttu-id="42142-555">Pořadí vedlejší účinky je zachováno s ohledem na volatile čtení a zápisy ([pole s modifikátorem Volatile](classes.md#volatile-fields)).</span><span class="sxs-lookup"><span data-stu-id="42142-555">The ordering of side effects is preserved with respect to volatile reads and writes ([Volatile fields](classes.md#volatile-fields)).</span></span> <span data-ttu-id="42142-556">Kromě toho prostředí pro spouštění nemusí vyhodnocení součástí výrazu, jestli můžete zjistit, že hodnota tohoto výrazu se nepoužívá a že žádné vedlejší účinky potřebné vytvářeny (včetně všech volání metody nebo přístup k pole s modifikátorem volatile).</span><span class="sxs-lookup"><span data-stu-id="42142-556">Additionally, the execution environment need not evaluate part of an expression if it can deduce that that expression's value is not used and that no needed side effects are produced (including any caused by calling a method or accessing a volatile field).</span></span> <span data-ttu-id="42142-557">Když dojde k přerušení provádění programu asynchronní událostí (například výjimka vyvolaná objektem jiného vlákna), není zaručeno, že pozorovatelný vedlejší účinky jsou viditelné v původní pořadí programu.</span><span class="sxs-lookup"><span data-stu-id="42142-557">When program execution is interrupted by an asynchronous event (such as an exception thrown by another thread), it is not guaranteed that the observable side effects are visible in the original program order.</span></span>
