# <a name="namespaces"></a><span data-ttu-id="4ac72-101">Jmenné prostory</span><span class="sxs-lookup"><span data-stu-id="4ac72-101">Namespaces</span></span>

<span data-ttu-id="4ac72-102">Programy jazyka C# jsou uspořádané pomocí oborů názvů.</span><span class="sxs-lookup"><span data-stu-id="4ac72-102">C# programs are organized using namespaces.</span></span> <span data-ttu-id="4ac72-103">Obory názvů slouží jako systém "internal" organizace pro program i jako systém "externí" organizace, způsob prezentace prvky programu, které jsou vystaveny do jiných programů.</span><span class="sxs-lookup"><span data-stu-id="4ac72-103">Namespaces are used both as an "internal" organization system for a program, and as an "external" organization system—a way of presenting program elements that are exposed to other programs.</span></span>

<span data-ttu-id="4ac72-104">Direktivy using ([direktiv Using](namespaces.md#using-directives)) jsou k dispozici pro usnadnění použití oborů názvů.</span><span class="sxs-lookup"><span data-stu-id="4ac72-104">Using directives ([Using directives](namespaces.md#using-directives)) are provided to facilitate the use of namespaces.</span></span>

## <a name="compilation-units"></a><span data-ttu-id="4ac72-105">Kompilačních jednotek</span><span class="sxs-lookup"><span data-stu-id="4ac72-105">Compilation units</span></span>

<span data-ttu-id="4ac72-106">A *compilation_unit* definuje strukturu celkové zdrojového souboru.</span><span class="sxs-lookup"><span data-stu-id="4ac72-106">A *compilation_unit* defines the overall structure of a source file.</span></span> <span data-ttu-id="4ac72-107">Jednotka kompilace se skládá z nula nebo více *using_directive*s následovat nula nebo více *global_attributes* následovat nula nebo více *namespace_member_declaration*s .</span><span class="sxs-lookup"><span data-stu-id="4ac72-107">A compilation unit consists of zero or more *using_directive*s followed by zero or more *global_attributes* followed by zero or more *namespace_member_declaration*s.</span></span>

```antlr
compilation_unit
    : extern_alias_directive* using_directive* global_attributes? namespace_member_declaration*
    ;
```

<span data-ttu-id="4ac72-108">Se skládá z jedné nebo více jednotek kompilace programu v jazyce C#, každý obsažené v samostatném zdrojovém souboru.</span><span class="sxs-lookup"><span data-stu-id="4ac72-108">A C# program consists of one or more compilation units, each contained in a separate source file.</span></span> <span data-ttu-id="4ac72-109">Při kompilaci programu v jazyce C# kompilačních jednotek jsou zpracovány všechny najednou.</span><span class="sxs-lookup"><span data-stu-id="4ac72-109">When a C# program is compiled, all of the compilation units are processed together.</span></span> <span data-ttu-id="4ac72-110">Proto kompilačních jednotek můžete jsou vzájemně závislé, může být cyklické způsobem.</span><span class="sxs-lookup"><span data-stu-id="4ac72-110">Thus, compilation units can depend on each other, possibly in a circular fashion.</span></span>

<span data-ttu-id="4ac72-111">*Using_directive*s vliv jednotku kompilace *global_attributes* a *namespace_member_declaration*s z kompilační jednotky, ale nemají žádný vliv jiné jednotky kompilace.</span><span class="sxs-lookup"><span data-stu-id="4ac72-111">The *using_directive*s of a compilation unit affect the *global_attributes* and *namespace_member_declaration*s of that compilation unit, but have no effect on other compilation units.</span></span>

<span data-ttu-id="4ac72-112">*Global_attributes* ([atributy](attributes.md)) z kompilační jednotky povolit specifikaci atributy pro cílové sestavení a modul.</span><span class="sxs-lookup"><span data-stu-id="4ac72-112">The *global_attributes* ([Attributes](attributes.md)) of a compilation unit permit the specification of attributes for the target assembly and module.</span></span> <span data-ttu-id="4ac72-113">Sestavení a moduly fungují jako fyzické kontejnery pro typy.</span><span class="sxs-lookup"><span data-stu-id="4ac72-113">Assemblies and modules act as physical containers for types.</span></span> <span data-ttu-id="4ac72-114">Sestavení může obsahovat několik fyzicky oddělená modulů.</span><span class="sxs-lookup"><span data-stu-id="4ac72-114">An assembly may consist of several physically separate modules.</span></span>

<span data-ttu-id="4ac72-115">*Namespace_member_declaration*s každou jednotku kompilace programu přispívají členové jediné deklaraci místo nazývané globální obor názvů.</span><span class="sxs-lookup"><span data-stu-id="4ac72-115">The *namespace_member_declaration*s of each compilation unit of a program contribute members to a single declaration space called the global namespace.</span></span> <span data-ttu-id="4ac72-116">Příklad:</span><span class="sxs-lookup"><span data-stu-id="4ac72-116">For example:</span></span>

<span data-ttu-id="4ac72-117">Soubor `A.cs`:</span><span class="sxs-lookup"><span data-stu-id="4ac72-117">File `A.cs`:</span></span>
```csharp
class A {}
```

<span data-ttu-id="4ac72-118">Soubor `B.cs`:</span><span class="sxs-lookup"><span data-stu-id="4ac72-118">File `B.cs`:</span></span>
```csharp
class B {}
```

<span data-ttu-id="4ac72-119">Dva kompilačních jednotek přispívat do jednoho globálního oboru názvů, v tomto případě deklarovat dvě třídy s plně kvalifikované názvy `A` a `B`.</span><span class="sxs-lookup"><span data-stu-id="4ac72-119">The two compilation units contribute to the single global namespace, in this case declaring two classes with the fully qualified names `A` and `B`.</span></span> <span data-ttu-id="4ac72-120">Protože dva kompilačních jednotek přispívat na stejné místo prohlášení, bylo by chybě pokud každá obsahovala deklarace člena se stejným názvem.</span><span class="sxs-lookup"><span data-stu-id="4ac72-120">Because the two compilation units contribute to the same declaration space, it would have been an error if each contained a declaration of a member with the same name.</span></span>

## <a name="namespace-declarations"></a><span data-ttu-id="4ac72-121">Deklarace Namespace</span><span class="sxs-lookup"><span data-stu-id="4ac72-121">Namespace declarations</span></span>

<span data-ttu-id="4ac72-122">A *namespace_declaration* se skládá z klíčového slova `namespace`, za nímž následuje název oboru názvů a text, může volitelně následovat středník.</span><span class="sxs-lookup"><span data-stu-id="4ac72-122">A *namespace_declaration* consists of the keyword `namespace`, followed by a namespace name and body, optionally followed by a semicolon.</span></span>

```antlr
namespace_declaration
    : 'namespace' qualified_identifier namespace_body ';'?
    ;

qualified_identifier
    : identifier ('.' identifier)*
    ;

namespace_body
    : '{' extern_alias_directive* using_directive* namespace_member_declaration* '}'
    ;
```

<span data-ttu-id="4ac72-123">A *namespace_declaration* může dojít k jako deklaraci nejvyšší úrovně v *compilation_unit* nebo jako člen deklarace v rámci jiného *namespace_declaration*.</span><span class="sxs-lookup"><span data-stu-id="4ac72-123">A *namespace_declaration* may occur as a top-level declaration in a *compilation_unit* or as a member declaration within another *namespace_declaration*.</span></span> <span data-ttu-id="4ac72-124">Když *namespace_declaration* vyskytuje se jako deklaraci nejvyšší úrovně v *compilation_unit*, obor názvů, stane se členem globální obor názvů.</span><span class="sxs-lookup"><span data-stu-id="4ac72-124">When a *namespace_declaration* occurs as a top-level declaration in a *compilation_unit*, the namespace becomes a member of the global namespace.</span></span> <span data-ttu-id="4ac72-125">Když *namespace_declaration* dochází v rámci jiného *namespace_declaration*, vnitřní obor názvů, stane se členem vnější obor názvů.</span><span class="sxs-lookup"><span data-stu-id="4ac72-125">When a *namespace_declaration* occurs within another *namespace_declaration*, the inner namespace becomes a member of the outer namespace.</span></span> <span data-ttu-id="4ac72-126">V obou případech se název oboru názvů musí být jedinečný v rámci nadřazeného oboru názvů.</span><span class="sxs-lookup"><span data-stu-id="4ac72-126">In either case, the name of a namespace must be unique within the containing namespace.</span></span>

<span data-ttu-id="4ac72-127">Obory názvů jsou implicitně `public` a deklarace oboru názvů nesmí obsahovat žádné modifikátory přístupu.</span><span class="sxs-lookup"><span data-stu-id="4ac72-127">Namespaces are implicitly `public` and the declaration of a namespace cannot include any access modifiers.</span></span>

<span data-ttu-id="4ac72-128">V rámci *namespace_body*, volitelný *using_directive*importovat s názvy jiných obory názvů, typy a členy, což jim umožní být odkazovaných přímo namísto prostřednictvím kvalifikované názvy.</span><span class="sxs-lookup"><span data-stu-id="4ac72-128">Within a *namespace_body*, the optional *using_directive*s import the names of other namespaces, types and members, allowing them to be referenced directly instead of through qualified names.</span></span> <span data-ttu-id="4ac72-129">Volitelný *namespace_member_declaration*s přispívat členy do prostoru deklarace oboru názvů.</span><span class="sxs-lookup"><span data-stu-id="4ac72-129">The optional *namespace_member_declaration*s contribute members to the declaration space of the namespace.</span></span> <span data-ttu-id="4ac72-130">Všimněte si, že všechny *using_directive*s musí být uvedena před všemi deklaracemi členů.</span><span class="sxs-lookup"><span data-stu-id="4ac72-130">Note that all *using_directive*s must appear before any member declarations.</span></span>

<span data-ttu-id="4ac72-131">*Qualified_identifier* z *namespace_declaration* může být jednoho identifikátoru nebo posloupnost identifikátorů oddělených "`.`" tokeny.</span><span class="sxs-lookup"><span data-stu-id="4ac72-131">The *qualified_identifier* of a *namespace_declaration* may be a single identifier or a sequence of identifiers separated by "`.`" tokens.</span></span> <span data-ttu-id="4ac72-132">Druhý formulář umožňuje programu definovat vnořené oboru názvů bez vnoření lexikálně několik deklarace oboru názvů.</span><span class="sxs-lookup"><span data-stu-id="4ac72-132">The latter form permits a program to define a nested namespace without lexically nesting several namespace declarations.</span></span> <span data-ttu-id="4ac72-133">Například</span><span class="sxs-lookup"><span data-stu-id="4ac72-133">For example,</span></span>

```csharp
namespace N1.N2
{
    class A {}

    class B {}
}
```
<span data-ttu-id="4ac72-134">je sémantické ekvivalenty</span><span class="sxs-lookup"><span data-stu-id="4ac72-134">is semantically equivalent to</span></span>
```csharp
namespace N1
{
    namespace N2
    {
        class A {}

        class B {}
    }
}
```

<span data-ttu-id="4ac72-135">Obory názvů jsou otevřený a dvě deklarace oboru názvů se stejným plně kvalifikovaným názvem přispívat na stejné místo prohlášení ([deklarace](basic-concepts.md#declarations)).</span><span class="sxs-lookup"><span data-stu-id="4ac72-135">Namespaces are open-ended, and two namespace declarations with the same fully qualified name contribute to the same declaration space ([Declarations](basic-concepts.md#declarations)).</span></span> <span data-ttu-id="4ac72-136">V příkladu</span><span class="sxs-lookup"><span data-stu-id="4ac72-136">In the example</span></span>
```csharp
namespace N1.N2
{
    class A {}
}

namespace N1.N2
{
    class B {}
}
```
<span data-ttu-id="4ac72-137">výše uvedené dvě deklarace oboru názvů přispívat na stejné místo prohlášení, v tomto případě deklarovat dvě třídy s plně kvalifikované názvy `N1.N2.A` a `N1.N2.B`.</span><span class="sxs-lookup"><span data-stu-id="4ac72-137">the two namespace declarations above contribute to the same declaration space, in this case declaring two classes with the fully qualified names `N1.N2.A` and `N1.N2.B`.</span></span> <span data-ttu-id="4ac72-138">Vzhledem k tomu, že dvě deklarace přispívat na stejné místo prohlášení, by bylo, že se chybu, pokud každá obsahovala deklarace člena se stejným názvem.</span><span class="sxs-lookup"><span data-stu-id="4ac72-138">Because the two declarations contribute to the same declaration space, it would have been an error if each contained a declaration of a member with the same name.</span></span>

## <a name="extern-aliases"></a><span data-ttu-id="4ac72-139">Aliasy extern</span><span class="sxs-lookup"><span data-stu-id="4ac72-139">Extern aliases</span></span>

<span data-ttu-id="4ac72-140">*Extern_alias_directive* zavádí identifikátor, který slouží jako alias pro obor názvů.</span><span class="sxs-lookup"><span data-stu-id="4ac72-140">An *extern_alias_directive* introduces an identifier that serves as an alias for a namespace.</span></span> <span data-ttu-id="4ac72-141">Specifikace oboru názvů s aliasem je externí ke zdrojovému kódu programu a platí také pro vnořené obory názvů s aliasem oboru názvů.</span><span class="sxs-lookup"><span data-stu-id="4ac72-141">The specification of the aliased namespace is external to the source code of the program and applies also to nested namespaces of the aliased namespace.</span></span>

```antlr
extern_alias_directive
    : 'extern' 'alias' identifier ';'
    ;
```

<span data-ttu-id="4ac72-142">Rozsah *extern_alias_directive* rozloženo *using_directive*s, *global_attributes* a *namespace_member_declaration*s jeho bezprostředně nadřazeného kompilace částí nebo těle oboru názvů.</span><span class="sxs-lookup"><span data-stu-id="4ac72-142">The scope of an *extern_alias_directive* extends over the *using_directive*s, *global_attributes* and *namespace_member_declaration*s of its immediately containing compilation unit or namespace body.</span></span>

<span data-ttu-id="4ac72-143">V rámci kompilace částí nebo těle oboru názvů, který obsahuje *extern_alias_directive*, identifikátor zavedené *extern_alias_directive* slouží k odkazu na obor názvů s aliasem.</span><span class="sxs-lookup"><span data-stu-id="4ac72-143">Within a compilation unit or namespace body that contains an *extern_alias_directive*, the identifier introduced by the *extern_alias_directive* can be used to reference the aliased namespace.</span></span> <span data-ttu-id="4ac72-144">Je chyba kompilace pro *identifikátor* být slovo `global`.</span><span class="sxs-lookup"><span data-stu-id="4ac72-144">It is a compile-time error for the *identifier* to be the word `global`.</span></span>

<span data-ttu-id="4ac72-145">*Extern_alias_directive* zpřístupní alias v rámci konkrétní kompilace jednotky nebo oboru názvů tělo, ale to není přispívat všemi novými členy do prostoru základní deklarace.</span><span class="sxs-lookup"><span data-stu-id="4ac72-145">An *extern_alias_directive* makes an alias available within a particular compilation unit or namespace body, but it does not contribute any new members to the underlying declaration space.</span></span> <span data-ttu-id="4ac72-146">Jinými slovy *extern_alias_directive* není přenosné, ale místo toho ovlivňuje pouze kompilaci jednotky nebo oboru názvů text ve kterém se vyskytuje.</span><span class="sxs-lookup"><span data-stu-id="4ac72-146">In other words, an *extern_alias_directive* is not transitive, but, rather, affects only the compilation unit or namespace body in which it occurs.</span></span>

<span data-ttu-id="4ac72-147">Následující program deklaruje a používá dva extern aliasy `X` a `Y`, každý z které představují nejnižší úrovni hierarchie jedinečných názvů:</span><span class="sxs-lookup"><span data-stu-id="4ac72-147">The following program declares and uses two extern aliases, `X` and `Y`, each of which represent the root of a distinct namespace hierarchy:</span></span>
```csharp
extern alias X;
extern alias Y;

class Test
{
    X::N.A a;
    X::N.B b1;
    Y::N.B b2;
    Y::N.C c;
}
```

<span data-ttu-id="4ac72-148">Program deklaruje existenci externích aliasů `X` a `Y`, ale skutečné definice aliasy jsou externí vzhledem k programu.</span><span class="sxs-lookup"><span data-stu-id="4ac72-148">The program declares the existence of the extern aliases `X` and `Y`, but the actual definitions of the aliases are external to the program.</span></span> <span data-ttu-id="4ac72-149">Identicky pojmenovanou `N.B` třídy může být odkazováno nyní jako `X.N.B` a `Y.N.B`, nebo pomocí kvalifikátor aliasu oboru názvů `X::N.B` a `Y::N.B`.</span><span class="sxs-lookup"><span data-stu-id="4ac72-149">The identically named `N.B` classes can now be referenced as `X.N.B` and `Y.N.B`, or, using the namespace alias qualifier, `X::N.B` and `Y::N.B`.</span></span> <span data-ttu-id="4ac72-150">Pokud program deklaruje externí alias pro kterou je k dispozici žádná externí definice dojde k chybě.</span><span class="sxs-lookup"><span data-stu-id="4ac72-150">An error occurs if a program declares an extern alias for which no external definition is provided.</span></span>

## <a name="using-directives"></a><span data-ttu-id="4ac72-151">direktivy using</span><span class="sxs-lookup"><span data-stu-id="4ac72-151">Using directives</span></span>

<span data-ttu-id="4ac72-152">***Direktivy using*** usnadnění používání oborů názvů a typy definované v jiných oborech názvů.</span><span class="sxs-lookup"><span data-stu-id="4ac72-152">***Using directives*** facilitate the use of namespaces and types defined in other namespaces.</span></span> <span data-ttu-id="4ac72-153">Pomocí direktivy vliv proces překladu IP adres z *namespace_or_type_name*s ([Namespace a zadejte názvy](basic-concepts.md#namespace-and-type-names)) a *simple_name*s ([jednoduché názvy ](expressions.md#simple-names)), ale na rozdíl od deklarace pomocí direktivy nepočítají nové členy do základní deklaraci prostorů kompilačních jednotek nebo obory názvů, ve kterém se používají.</span><span class="sxs-lookup"><span data-stu-id="4ac72-153">Using directives impact the name resolution process of *namespace_or_type_name*s ([Namespace and type names](basic-concepts.md#namespace-and-type-names)) and *simple_name*s ([Simple names](expressions.md#simple-names)), but unlike declarations, using directives do not contribute new members to the underlying declaration spaces of the compilation units or namespaces within which they are used.</span></span>

```antlr
using_directive
    : using_alias_directive
    | using_namespace_directive
    | using_static_directive
    ;
```

<span data-ttu-id="4ac72-154">A *using_alias_directive* ([alias direktiv Using](namespaces.md#using-alias-directives)) představuje alias pro obor názvů nebo typ.</span><span class="sxs-lookup"><span data-stu-id="4ac72-154">A *using_alias_directive* ([Using alias directives](namespaces.md#using-alias-directives)) introduces an alias for a namespace or type.</span></span>

<span data-ttu-id="4ac72-155">A *using_namespace_directive* ([pomocí direktivy oboru názvů](namespaces.md#using-namespace-directives)) Importuje členy typu oboru názvů.</span><span class="sxs-lookup"><span data-stu-id="4ac72-155">A *using_namespace_directive* ([Using namespace directives](namespaces.md#using-namespace-directives)) imports the type members of a namespace.</span></span>

<span data-ttu-id="4ac72-156">A *using_static_directive* ([statické direktiv Using](namespaces.md#using-static-directives)) Importuje vnořené typy a statické členy typu.</span><span class="sxs-lookup"><span data-stu-id="4ac72-156">A *using_static_directive* ([Using static directives](namespaces.md#using-static-directives)) imports the nested types and static members of a type.</span></span>

<span data-ttu-id="4ac72-157">Rozsah *using_directive* rozloženo *namespace_member_declaration*s jeho bezprostředně nadřazeného kompilace částí nebo těle oboru názvů.</span><span class="sxs-lookup"><span data-stu-id="4ac72-157">The scope of a *using_directive* extends over the *namespace_member_declaration*s of its immediately containing compilation unit or namespace body.</span></span> <span data-ttu-id="4ac72-158">Rozsah *using_directive* konkrétně nezahrnuje jeho peer *using_directive*s.</span><span class="sxs-lookup"><span data-stu-id="4ac72-158">The scope of a *using_directive* specifically does not include its peer *using_directive*s.</span></span> <span data-ttu-id="4ac72-159">Díky tomu se vytvoření partnerského vztahu *using_directive*s nemají vliv na sobě navzájem, a pořadí, ve kterém jsou zapsány je neplatné.</span><span class="sxs-lookup"><span data-stu-id="4ac72-159">Thus, peer *using_directive*s do not affect each other, and the order in which they are written is insignificant.</span></span>

### <a name="using-alias-directives"></a><span data-ttu-id="4ac72-160">Alias direktivy using</span><span class="sxs-lookup"><span data-stu-id="4ac72-160">Using alias directives</span></span>

<span data-ttu-id="4ac72-161">A *using_alias_directive* zavádí identifikátor, který slouží jako alias pro obor názvů nebo typ těla bezprostředně vložená kompilace jednotky nebo oboru názvů.</span><span class="sxs-lookup"><span data-stu-id="4ac72-161">A *using_alias_directive* introduces an identifier that serves as an alias for a namespace or type within the immediately enclosing compilation unit or namespace body.</span></span>

```antlr
using_alias_directive
    : 'using' identifier '=' namespace_or_type_name ';'
    ;
```

<span data-ttu-id="4ac72-162">V rámci deklarace členů v kompilaci jednotky nebo oboru názvů subjektu, který obsahuje *using_alias_directive*, identifikátor zavedené *using_alias_directive* lze použít k odkazu daný obor názvů nebo typ.</span><span class="sxs-lookup"><span data-stu-id="4ac72-162">Within member declarations in a compilation unit or namespace body that contains a *using_alias_directive*, the identifier introduced by the *using_alias_directive* can be used to reference the given namespace or type.</span></span> <span data-ttu-id="4ac72-163">Příklad:</span><span class="sxs-lookup"><span data-stu-id="4ac72-163">For example:</span></span>
```csharp
namespace N1.N2
{
    class A {}
}

namespace N3
{
    using A = N1.N2.A;

    class B: A {}
}
```

<span data-ttu-id="4ac72-164">Výše uvedené, v rámci deklarace členů v `N3` obor názvů, `A` je alias pro `N1.N2.A`a proto třídy `N3.B` je odvozena z třídy `N1.N2.A`.</span><span class="sxs-lookup"><span data-stu-id="4ac72-164">Above, within member declarations in the `N3` namespace, `A` is an alias for `N1.N2.A`, and thus class `N3.B` derives from class `N1.N2.A`.</span></span> <span data-ttu-id="4ac72-165">Stejného výsledku lze získat tak, že vytvoříte alias `R` pro `N1.N2` a potom odkazování na ně `R.A`:</span><span class="sxs-lookup"><span data-stu-id="4ac72-165">The same effect can be obtained by creating an alias `R` for `N1.N2` and then referencing `R.A`:</span></span>
```csharp
namespace N3
{
    using R = N1.N2;

    class B: R.A {}
}
```

<span data-ttu-id="4ac72-166">*Identifikátor* z *using_alias_directive* musí být jedinečný v rámci deklarace prostoru kompilační jednotky nebo oboru názvů, který obsahuje okamžitě *using_alias_directive* .</span><span class="sxs-lookup"><span data-stu-id="4ac72-166">The *identifier* of a *using_alias_directive* must be unique within the declaration space of the compilation unit or namespace that immediately contains the *using_alias_directive*.</span></span> <span data-ttu-id="4ac72-167">Příklad:</span><span class="sxs-lookup"><span data-stu-id="4ac72-167">For example:</span></span>
```csharp
namespace N3
{
    class A {}
}

namespace N3
{
    using A = N1.N2.A;        // Error, A already exists
}
```

<span data-ttu-id="4ac72-168">Výše uvedené, `N3` již obsahuje člena `A`, takže se jedná o chybu kompilace pro *using_alias_directive* používat tento identifikátor.</span><span class="sxs-lookup"><span data-stu-id="4ac72-168">Above, `N3` already contains a member `A`, so it is a compile-time error for a *using_alias_directive* to use that identifier.</span></span> <span data-ttu-id="4ac72-169">Podobně je chyba kompilace pro dvě nebo více *using_alias_directive*ve stejné kompilaci jednotky nebo oboru názvů tělo pro deklaraci aliasy se stejným názvem.</span><span class="sxs-lookup"><span data-stu-id="4ac72-169">Likewise, it is a compile-time error for two or more *using_alias_directive*s in the same compilation unit or namespace body to declare aliases by the same name.</span></span>

<span data-ttu-id="4ac72-170">A *using_alias_directive* zpřístupní alias v rámci konkrétní kompilace jednotky nebo oboru názvů tělo, ale to není přispívat všemi novými členy do prostoru základní deklarace.</span><span class="sxs-lookup"><span data-stu-id="4ac72-170">A *using_alias_directive* makes an alias available within a particular compilation unit or namespace body, but it does not contribute any new members to the underlying declaration space.</span></span> <span data-ttu-id="4ac72-171">Jinými slovy *using_alias_directive* není přenosné, ale spíše ovlivňuje pouze kompilaci jednotky nebo oboru názvů text ve kterém se vyskytuje.</span><span class="sxs-lookup"><span data-stu-id="4ac72-171">In other words, a *using_alias_directive* is not transitive but rather affects only the compilation unit or namespace body in which it occurs.</span></span> <span data-ttu-id="4ac72-172">V příkladu</span><span class="sxs-lookup"><span data-stu-id="4ac72-172">In the example</span></span>
```csharp
namespace N3
{
    using R = N1.N2;
}

namespace N3
{
    class B: R.A {}            // Error, R unknown
}
```
<span data-ttu-id="4ac72-173">rozsah *using_alias_directive* , který představuje `R` rozšiřuje pouze na deklarace členů v těle oboru názvů, ve kterém je obsažená, takže `R` není známý v druhé deklarace oboru názvů.</span><span class="sxs-lookup"><span data-stu-id="4ac72-173">the scope of the *using_alias_directive* that introduces `R` only extends to member declarations in the namespace body in which it is contained, so `R` is unknown in the second namespace declaration.</span></span> <span data-ttu-id="4ac72-174">Ale, že umístíte *using_alias_directive* obsahující kompilace částí způsobí, že alias k dispozici v rámci obě deklarace oboru názvů:</span><span class="sxs-lookup"><span data-stu-id="4ac72-174">However, placing the *using_alias_directive* in the containing compilation unit causes the alias to become available within both namespace declarations:</span></span>
```csharp
using R = N1.N2;

namespace N3
{
    class B: R.A {}
}

namespace N3
{
    class C: R.A {}
}
```

<span data-ttu-id="4ac72-175">Stejně jako běžné členy, názvy zavedené *using_alias_directive*s jsou skryté členy s podobným názvem ve vnořené obory.</span><span class="sxs-lookup"><span data-stu-id="4ac72-175">Just like regular members, names introduced by *using_alias_directive*s are hidden by similarly named members in nested scopes.</span></span> <span data-ttu-id="4ac72-176">V příkladu</span><span class="sxs-lookup"><span data-stu-id="4ac72-176">In the example</span></span>
```csharp
using R = N1.N2;

namespace N3
{
    class R {}

    class B: R.A {}        // Error, R has no member A
}
```
<span data-ttu-id="4ac72-177">odkaz na `R.A` v deklaraci `B` způsobí chybu kompilace, protože `R` odkazuje na `N3.R`, nikoli `N1.N2`.</span><span class="sxs-lookup"><span data-stu-id="4ac72-177">the reference to `R.A` in the declaration of `B` causes a compile-time error because `R` refers to `N3.R`, not `N1.N2`.</span></span>

<span data-ttu-id="4ac72-178">Pořadí, ve kterém *using_alias_directive*s jsou zapsány nemá žádný význam a rozlišení *namespace_or_type_name* odkazuje *using_alias_directive*nemá vliv *using_alias_directive* samotné nebo jiná *using_directive*ve okamžitě obsahující text jednotky nebo oboru názvů kompilace.</span><span class="sxs-lookup"><span data-stu-id="4ac72-178">The order in which *using_alias_directive*s are written has no significance, and resolution of the *namespace_or_type_name* referenced by a *using_alias_directive* is not affected by the *using_alias_directive* itself or by other *using_directive*s in the immediately containing compilation unit or namespace body.</span></span> <span data-ttu-id="4ac72-179">Jinými slovy *namespace_or_type_name* z *using_alias_directive* vyřeší, jako kdyby měl okamžitě obsahující text jednotky nebo oboru názvů kompilace bez *using_directive*s.</span><span class="sxs-lookup"><span data-stu-id="4ac72-179">In other words, the *namespace_or_type_name* of a *using_alias_directive* is resolved as if the immediately containing compilation unit or namespace body had no *using_directive*s.</span></span> <span data-ttu-id="4ac72-180">A *using_alias_directive* však může být ovlivněn *extern_alias_directive*ve okamžitě obsahující text jednotky nebo oboru názvů kompilace.</span><span class="sxs-lookup"><span data-stu-id="4ac72-180">A *using_alias_directive* may however be affected by *extern_alias_directive*s in the immediately containing compilation unit or namespace body.</span></span> <span data-ttu-id="4ac72-181">V příkladu</span><span class="sxs-lookup"><span data-stu-id="4ac72-181">In the example</span></span>
```csharp
namespace N1.N2 {}

namespace N3
{
    extern alias E;

    using R1 = E.N;        // OK

    using R2 = N1;         // OK

    using R3 = N1.N2;      // OK

    using R4 = R2.N2;      // Error, R2 unknown
}
```
<span data-ttu-id="4ac72-182">poslední *using_alias_directive* výsledkem chyba kompilace, protože nemá vliv první *using_alias_directive*.</span><span class="sxs-lookup"><span data-stu-id="4ac72-182">the last *using_alias_directive* results in a compile-time error because it is not affected by the first *using_alias_directive*.</span></span> <span data-ttu-id="4ac72-183">První *using_alias_directive* nemá za následek chybu od oboru extern alias `E` zahrnuje *using_alias_directive*.</span><span class="sxs-lookup"><span data-stu-id="4ac72-183">The first *using_alias_directive* does not result in an error since the scope of the extern alias `E` includes the *using_alias_directive*.</span></span>

<span data-ttu-id="4ac72-184">A *using_alias_directive* můžete vytvořit alias pro obor názvů nebo typ, včetně oboru názvů, ve kterém se zobrazí a jakékoli obor názvů nebo typ vnořené v daném oboru názvů.</span><span class="sxs-lookup"><span data-stu-id="4ac72-184">A *using_alias_directive* can create an alias for any namespace or type, including the namespace within which it appears and any namespace or type nested within that namespace.</span></span>

<span data-ttu-id="4ac72-185">Přístup k oboru názvů nebo typ prostřednictvím alias vrací stejný výsledek jako přístupu tohoto oboru názvů nebo typ s využitím názvu deklarovaný.</span><span class="sxs-lookup"><span data-stu-id="4ac72-185">Accessing a namespace or type through an alias yields exactly the same result as accessing that namespace or type through its declared name.</span></span> <span data-ttu-id="4ac72-186">Mějme například</span><span class="sxs-lookup"><span data-stu-id="4ac72-186">For example, given</span></span>
```csharp
namespace N1.N2
{
    class A {}
}

namespace N3
{
    using R1 = N1;
    using R2 = N1.N2;

    class B
    {
        N1.N2.A a;            // refers to N1.N2.A
        R1.N2.A b;            // refers to N1.N2.A
        R2.A c;               // refers to N1.N2.A
    }
}
```
<span data-ttu-id="4ac72-187">názvy `N1.N2.A`, `R1.N2.A`, a `R2.A` jsou ekvivalentní a všechny odkazovat na třídu, jejíž plně kvalifikovaný název je `N1.N2.A`.</span><span class="sxs-lookup"><span data-stu-id="4ac72-187">the names `N1.N2.A`, `R1.N2.A`, and `R2.A` are equivalent and all refer to the class whose fully qualified name is `N1.N2.A`.</span></span>

<span data-ttu-id="4ac72-188">Aliasy Using název uzavřený konstruovaný typ. může, ale nikoli název deklarace nevázaný parametr generického typu bez zadávání argumentů typu.</span><span class="sxs-lookup"><span data-stu-id="4ac72-188">Using aliases can name a closed constructed type, but cannot name an unbound generic type declaration without supplying type arguments.</span></span> <span data-ttu-id="4ac72-189">Příklad:</span><span class="sxs-lookup"><span data-stu-id="4ac72-189">For example:</span></span>
```csharp
namespace N1
{
    class A<T>
    {
        class B {}
    }
}

namespace N2
{
    using W = N1.A;          // Error, cannot name unbound generic type

    using X = N1.A.B;        // Error, cannot name unbound generic type

    using Y = N1.A<int>;     // Ok, can name closed constructed type

    using Z<T> = N1.A<T>;    // Error, using alias cannot have type parameters
}
```

### <a name="using-namespace-directives"></a><span data-ttu-id="4ac72-190">Pomocí direktivy oboru názvů</span><span class="sxs-lookup"><span data-stu-id="4ac72-190">Using namespace directives</span></span>

<span data-ttu-id="4ac72-191">A *using_namespace_directive* importuje typy obsažené v oboru názvů do bezprostředně vložená kompilace částí nebo těle oboru názvů, povolení identifikátor každého typu bez kvalifikace lze používat.</span><span class="sxs-lookup"><span data-stu-id="4ac72-191">A *using_namespace_directive* imports the types contained in a namespace into the immediately enclosing compilation unit or namespace body, enabling the identifier of each type to be used without qualification.</span></span>

```antlr
using_namespace_directive
    : 'using' namespace_name ';'
    ;
```

<span data-ttu-id="4ac72-192">V rámci deklarace členů v kompilaci jednotky nebo oboru názvů subjektu, který obsahuje *using_namespace_directive*, typy obsažené v daném oboru názvů může být odkazováno přímo.</span><span class="sxs-lookup"><span data-stu-id="4ac72-192">Within member declarations in a compilation unit or namespace body that contains a *using_namespace_directive*, the types contained in the given namespace can be referenced directly.</span></span> <span data-ttu-id="4ac72-193">Příklad:</span><span class="sxs-lookup"><span data-stu-id="4ac72-193">For example:</span></span>
```csharp
namespace N1.N2
{
    class A {}
}

namespace N3
{
    using N1.N2;

    class B: A {}
}
```

<span data-ttu-id="4ac72-194">Výše uvedené, v rámci deklarace členů v `N3` typ členů z oboru názvů `N1.N2` se přímo k dispozici a proto třídy `N3.B` je odvozena z třídy `N1.N2.A`.</span><span class="sxs-lookup"><span data-stu-id="4ac72-194">Above, within member declarations in the `N3` namespace, the type members of `N1.N2` are directly available, and thus class `N3.B` derives from class `N1.N2.A`.</span></span>

<span data-ttu-id="4ac72-195">A *using_namespace_directive* importuje typy obsažené v daném oboru názvů, ale specificky neimportuje vnořené obory názvů.</span><span class="sxs-lookup"><span data-stu-id="4ac72-195">A *using_namespace_directive* imports the types contained in the given namespace, but specifically does not import nested namespaces.</span></span> <span data-ttu-id="4ac72-196">V příkladu</span><span class="sxs-lookup"><span data-stu-id="4ac72-196">In the example</span></span>
```csharp
namespace N1.N2
{
    class A {}
}

namespace N3
{
    using N1;

    class B: N2.A {}        // Error, N2 unknown
}
```
<span data-ttu-id="4ac72-197">*using_namespace_directive* importuje typy obsažené v `N1`, ale ne obory názvů vnořené v `N1`.</span><span class="sxs-lookup"><span data-stu-id="4ac72-197">the *using_namespace_directive* imports the types contained in `N1`, but not the namespaces nested in `N1`.</span></span> <span data-ttu-id="4ac72-198">Díky tomu se odkaz na `N2.A` v deklaraci `B` výsledkem chyba kompilace, protože žádné členy s názvem `N2` nacházejí v oboru.</span><span class="sxs-lookup"><span data-stu-id="4ac72-198">Thus, the reference to `N2.A` in the declaration of `B` results in a compile-time error because no members named `N2` are in scope.</span></span>

<span data-ttu-id="4ac72-199">Na rozdíl od *using_alias_directive*, *using_namespace_directive* mohou importovat typy, jejichž identifikátory jsou již definovány v rámci nadřazeného kompilace jednotky nebo oboru názvů subjektu.</span><span class="sxs-lookup"><span data-stu-id="4ac72-199">Unlike a *using_alias_directive*, a *using_namespace_directive* may import types whose identifiers are already defined within the enclosing compilation unit or namespace body.</span></span> <span data-ttu-id="4ac72-200">V důsledku toho názvů importované tímto seznamem *using_namespace_directive* jsou skryté členy s podobným názvem v nadřazeném kompilace jednotky nebo oboru názvů textu.</span><span class="sxs-lookup"><span data-stu-id="4ac72-200">In effect, names imported by a *using_namespace_directive* are hidden by similarly named members in the enclosing compilation unit or namespace body.</span></span> <span data-ttu-id="4ac72-201">Příklad:</span><span class="sxs-lookup"><span data-stu-id="4ac72-201">For example:</span></span>
```csharp
namespace N1.N2
{
    class A {}

    class B {}
}

namespace N3
{
    using N1.N2;

    class A {}
}
```

<span data-ttu-id="4ac72-202">Tady v rámci deklarace členů v `N3` obor názvů, `A` odkazuje na `N3.A` spíše než `N1.N2.A`.</span><span class="sxs-lookup"><span data-stu-id="4ac72-202">Here, within member declarations in the `N3` namespace, `A` refers to `N3.A` rather than `N1.N2.A`.</span></span>

<span data-ttu-id="4ac72-203">Když se více než jeden obor názvů nebo typ importované tímto seznamem *using_namespace_directive*s nebo *using_static_directive*ve stejné kompilaci tělo jednotky nebo oboru názvů obsahují typy se stejným názvem odkazy na Tento název jako *type_name* jsou považovány za nejednoznačné.</span><span class="sxs-lookup"><span data-stu-id="4ac72-203">When more than one namespace or type imported by *using_namespace_directive*s or *using_static_directive*s in the same compilation unit or namespace body contain types by the same name, references to that name as a *type_name* are considered ambiguous.</span></span> <span data-ttu-id="4ac72-204">V příkladu</span><span class="sxs-lookup"><span data-stu-id="4ac72-204">In the example</span></span>
```csharp
namespace N1
{
    class A {}
}

namespace N2
{
    class A {}
}

namespace N3
{
    using N1;

    using N2;

    class B: A {}                // Error, A is ambiguous
}
```
<span data-ttu-id="4ac72-205">obě `N1` a `N2` obsahovat člena `A`a protože `N3` importuje obě, odkazující na `A` v `N3` je chyba kompilace.</span><span class="sxs-lookup"><span data-stu-id="4ac72-205">both `N1` and `N2` contain a member `A`, and because `N3` imports both, referencing `A` in `N3` is a compile-time error.</span></span> <span data-ttu-id="4ac72-206">V takovém případě může být konflikt vyřešit buď prostřednictvím kvalifikace odkazy na `A`, nebo zavedením *using_alias_directive* , který vybere konkrétní `A`.</span><span class="sxs-lookup"><span data-stu-id="4ac72-206">In this situation, the conflict can be resolved either through qualification of references to `A`, or by introducing a *using_alias_directive* that picks a particular `A`.</span></span> <span data-ttu-id="4ac72-207">Příklad:</span><span class="sxs-lookup"><span data-stu-id="4ac72-207">For example:</span></span>
```csharp
namespace N3
{
    using N1;

    using N2;

    using A = N1.A;

    class B: A {}                // A means N1.A
}
```

<span data-ttu-id="4ac72-208">Kromě toho, kdy více než jeden obor názvů nebo typ importované tímto seznamem *using_namespace_directive*s nebo *using_static_directive*ve stejné kompilaci tělo jednotky nebo oboru názvů obsahují typy nebo členy podle stejný název, odkazuje na tento název jako *simple_name* jsou považovány za nejednoznačné.</span><span class="sxs-lookup"><span data-stu-id="4ac72-208">Furthermore, when more than one namespace or type imported by *using_namespace_directive*s or *using_static_directive*s in the same compilation unit or namespace body contain types or members by the same name, references to that name as a *simple_name* are considered ambiguous.</span></span> <span data-ttu-id="4ac72-209">V příkladu</span><span class="sxs-lookup"><span data-stu-id="4ac72-209">In the example</span></span>
```csharp
namespace N1
{
    class A {}
}

class C
{
    public static int A;
}

namespace N2
{
    using N1;
    using static C;

    class B
    {
        void M() 
        { 
            A a = new A();   // Ok, A is unambiguous as a type-name
            A.Equals(2);     // Error, A is ambiguous as a simple-name
        }
    }
}
```
<span data-ttu-id="4ac72-210">`N1` obsahuje člen typu `A`, a `C` obsahuje statickou metodu `A`a protože `N2` importuje obě, odkazující na `A` jako *simple_name* je nejednoznačný a kompilace došlo k chybě.</span><span class="sxs-lookup"><span data-stu-id="4ac72-210">`N1` contains a type member `A`, and `C` contains a static method `A`, and because `N2` imports both, referencing `A` as a *simple_name* is ambiguous and a compile-time error.</span></span> 

<span data-ttu-id="4ac72-211">Podobně jako *using_alias_directive*, *using_namespace_directive* není přispívat všemi novými členy základní deklaraci prostoru kompilační jednotky nebo oboru názvů, ale místo toho má vliv pouze kompilace částí nebo těle oboru názvů ve kterém se zobrazí.</span><span class="sxs-lookup"><span data-stu-id="4ac72-211">Like a *using_alias_directive*, a *using_namespace_directive* does not contribute any new members to the underlying declaration space of the compilation unit or namespace, but rather affects only the compilation unit or namespace body in which it appears.</span></span>

<span data-ttu-id="4ac72-212">*Namespace_name* odkazovaná *using_namespace_directive* vyřeší stejným způsobem jako *namespace_or_type_name* odkazuje  *using_alias_directive*.</span><span class="sxs-lookup"><span data-stu-id="4ac72-212">The *namespace_name* referenced by a *using_namespace_directive* is resolved in the same way as the *namespace_or_type_name* referenced by a *using_alias_directive*.</span></span> <span data-ttu-id="4ac72-213">Proto *using_namespace_directive*ve stejné kompilaci jednotky nebo oboru názvů tělo nemají vliv na sebe navzájem a mohou být napsány v libovolném pořadí.</span><span class="sxs-lookup"><span data-stu-id="4ac72-213">Thus, *using_namespace_directive*s in the same compilation unit or namespace body do not affect each other and can be written in any order.</span></span>

### <a name="using-static-directives"></a><span data-ttu-id="4ac72-214">Statické direktivy using</span><span class="sxs-lookup"><span data-stu-id="4ac72-214">Using static directives</span></span>

<span data-ttu-id="4ac72-215">A *using_static_directive* importuje vnořené typy a statické členy přímo součástí deklarace typu do bezprostředně vložená kompilace částí nebo těle oboru názvů, povolení identifikátor každého člena a typ být použít bez kvalifikace.</span><span class="sxs-lookup"><span data-stu-id="4ac72-215">A *using_static_directive* imports the nested types and static members contained directly in a type declaration into the immediately enclosing compilation unit or namespace body, enabling the identifier of each member and type to be used without qualification.</span></span>

```antlr
using_static_directive
    : 'using' 'static' type_name ';'
    ;
```

<span data-ttu-id="4ac72-216">V rámci deklarace členů v kompilaci jednotky nebo oboru názvů subjektu, který obsahuje *using_static_directive*, přístupný vnořené typy a statické členy (s výjimkou metody rozšíření) součástí deklarace daný typ může být odkazováno přímo.</span><span class="sxs-lookup"><span data-stu-id="4ac72-216">Within member declarations in a compilation unit or namespace body that contains a *using_static_directive*, the accessible nested types and static members (except extension methods) contained directly in the declaration of the given type can be referenced directly.</span></span> <span data-ttu-id="4ac72-217">Příklad:</span><span class="sxs-lookup"><span data-stu-id="4ac72-217">For example:</span></span>

```csharp
namespace N1
{
    class A 
    {
        public class B{}
        public static B M(){ return new B(); }
    }
}

namespace N2
{
    using static N1.A;
    class C
    {
        void N() { B b = M(); }
    }
}
```

<span data-ttu-id="4ac72-218">Výše uvedené, v rámci deklarace členů v `N2` obor názvů, statické členy a vnořené typy `N1.A` jsou přímo k dispozici a tedy metodu `N` může odkazovat i `B` a `M` členy `N1.A`.</span><span class="sxs-lookup"><span data-stu-id="4ac72-218">Above, within member declarations in the `N2` namespace, the static members and nested types of `N1.A` are directly available, and thus the method `N` is able to reference both the `B` and `M` members of `N1.A`.</span></span>

<span data-ttu-id="4ac72-219">A *using_static_directive* konkrétně neimportuje rozšiřující metody přímo jako statické metody, ale zpřístupňuje je pro volání metody rozšíření ([volání metod rozšíření](expressions.md#extension-method-invocations)).</span><span class="sxs-lookup"><span data-stu-id="4ac72-219">A *using_static_directive* specifically does not import extension methods directly as static methods, but makes them available for extension method invocation ([Extension method invocations](expressions.md#extension-method-invocations)).</span></span> <span data-ttu-id="4ac72-220">V příkladu</span><span class="sxs-lookup"><span data-stu-id="4ac72-220">In the example</span></span>

```csharp
namespace N1 
{
    static class A 
    {
        public static void M(this string s){}
    }
}

namespace N2
{
    using static N1.A;

    class B
    {
        void N() 
        {
            M("A");      // Error, M unknown
            "B".M();     // Ok, M known as extension method
            N1.A.M("C"); // Ok, fully qualified
        }
    }
}
```
<span data-ttu-id="4ac72-221">*using_static_directive* importuje rozšiřující metoda `M` součástí `N1.A`, ale pouze jako metody rozšíření.</span><span class="sxs-lookup"><span data-stu-id="4ac72-221">the *using_static_directive* imports the extension method `M` contained in `N1.A`, but only as an extension method.</span></span> <span data-ttu-id="4ac72-222">Proto první odkaz na `M` v těle `B.N` výsledkem chyba kompilace, protože žádné členy s názvem `M` nacházejí v oboru.</span><span class="sxs-lookup"><span data-stu-id="4ac72-222">Thus, the first reference to `M` in the body of `B.N` results in a compile-time error because no members named `M` are in scope.</span></span>

<span data-ttu-id="4ac72-223">A *using_static_directive* importuje jenom členy a typy deklarované přímo do daného typu, ne členy a typy deklarované v základních tříd.</span><span class="sxs-lookup"><span data-stu-id="4ac72-223">A *using_static_directive* only imports members and types declared directly in the given type, not members and types declared in base classes.</span></span>

<span data-ttu-id="4ac72-224">TODO: Příklad</span><span class="sxs-lookup"><span data-stu-id="4ac72-224">TODO: Example</span></span>

<span data-ttu-id="4ac72-225">Nejednoznačnosti mezi několika *using_namespace_directives* a *using_static_directives* jsou popsány v [pomocí direktivy oboru názvů](namespaces.md#using-namespace-directives).</span><span class="sxs-lookup"><span data-stu-id="4ac72-225">Ambiguities between multiple *using_namespace_directives* and *using_static_directives* are discussed in [Using namespace directives](namespaces.md#using-namespace-directives).</span></span>

## <a name="namespace-members"></a><span data-ttu-id="4ac72-226">Namespace členy</span><span class="sxs-lookup"><span data-stu-id="4ac72-226">Namespace members</span></span>

<span data-ttu-id="4ac72-227">A *namespace_member_declaration* je buď *namespace_declaration* ([deklarací Namespace](namespaces.md#namespace-declarations)) nebo *type_declaration* () [Typ deklarace](namespaces.md#type-declarations)).</span><span class="sxs-lookup"><span data-stu-id="4ac72-227">A *namespace_member_declaration* is either a *namespace_declaration* ([Namespace declarations](namespaces.md#namespace-declarations)) or a *type_declaration* ([Type declarations](namespaces.md#type-declarations)).</span></span>

```antlr
namespace_member_declaration
    : namespace_declaration
    | type_declaration
    ;
```

<span data-ttu-id="4ac72-228">Jednotka kompilace nebo těle oboru názvů může obsahovat *namespace_member_declaration*s a takové deklarace přispívat nové členy do prostoru základní deklarace obsahující text jednotky nebo oboru názvů kompilace.</span><span class="sxs-lookup"><span data-stu-id="4ac72-228">A compilation unit or a namespace body can contain *namespace_member_declaration*s, and such declarations contribute new members to the underlying declaration space of the containing compilation unit or namespace body.</span></span>

## <a name="type-declarations"></a><span data-ttu-id="4ac72-229">Deklarace typu</span><span class="sxs-lookup"><span data-stu-id="4ac72-229">Type declarations</span></span>

<span data-ttu-id="4ac72-230">A *type_declaration* je *class_declaration* ([třídy deklarací](classes.md#class-declarations)), *struct_declaration* ([– struktura deklarace](structs.md#struct-declarations)), *interface_declaration* ([rozhraní deklarace](interfaces.md#interface-declarations)), *enum_declaration* ([výčtu deklarace](enums.md#enum-declarations)), nebo *delegate_declaration* ([delegovat deklarace](delegates.md#delegate-declarations)).</span><span class="sxs-lookup"><span data-stu-id="4ac72-230">A *type_declaration* is a *class_declaration* ([Class declarations](classes.md#class-declarations)), a *struct_declaration* ([Struct declarations](structs.md#struct-declarations)), an *interface_declaration* ([Interface declarations](interfaces.md#interface-declarations)), an *enum_declaration* ([Enum declarations](enums.md#enum-declarations)), or a *delegate_declaration* ([Delegate declarations](delegates.md#delegate-declarations)).</span></span>

```antlr
type_declaration
    : class_declaration
    | struct_declaration
    | interface_declaration
    | enum_declaration
    | delegate_declaration
    ;
```

<span data-ttu-id="4ac72-231">A *type_declaration* situace může nastat jako deklaraci nejvyšší úrovně v jednotce kompilace nebo jako člen deklarace v rámci oboru názvů, třídy nebo struktury.</span><span class="sxs-lookup"><span data-stu-id="4ac72-231">A *type_declaration* can occur as a top-level declaration in a compilation unit or as a member declaration within a namespace, class, or struct.</span></span>

<span data-ttu-id="4ac72-232">Při deklaraci typu pro typ `T` vyskytuje se jako deklaraci nejvyšší úrovně v jednotce kompilace, plně kvalifikovaný název nově deklarovaného typu je jednoduše `T`.</span><span class="sxs-lookup"><span data-stu-id="4ac72-232">When a type declaration for a type `T` occurs as a top-level declaration in a compilation unit, the fully qualified name of the newly declared type is simply `T`.</span></span> <span data-ttu-id="4ac72-233">Při deklaraci typu pro typ `T` dochází v rámci oboru názvů, třídy nebo struktury, plně kvalifikovaný název nově deklarovaného typu je `N.T`, kde `N` je plně kvalifikovaný název obsahuje obor názvů, třídy nebo struktury.</span><span class="sxs-lookup"><span data-stu-id="4ac72-233">When a type declaration for a type `T` occurs within a namespace, class, or struct, the fully qualified name of the newly declared type is `N.T`, where `N` is the fully qualified name of the containing namespace, class, or struct.</span></span>

<span data-ttu-id="4ac72-234">Typ deklarovaný v rámci třídy nebo struktury se nazývá vnořený typ. ([vnořené typy](classes.md#nested-types)).</span><span class="sxs-lookup"><span data-stu-id="4ac72-234">A type declared within a class or struct is called a nested type ([Nested types](classes.md#nested-types)).</span></span>

<span data-ttu-id="4ac72-235">Povolené přístupu modifikátory přístupu a přístup k výchozím pro deklaraci typu závisí na kontextu, ve kterém probíhá deklarace ([deklarovaná přístupnost](basic-concepts.md#declared-accessibility)):</span><span class="sxs-lookup"><span data-stu-id="4ac72-235">The permitted access modifiers and the default access for a type declaration depend on the context in which the declaration takes place ([Declared accessibility](basic-concepts.md#declared-accessibility)):</span></span>

*  <span data-ttu-id="4ac72-236">Typy deklarované v jednotkách kompilace nebo obory názvů můžou mít `public` nebo `internal` přístup.</span><span class="sxs-lookup"><span data-stu-id="4ac72-236">Types declared in compilation units or namespaces can have `public` or `internal` access.</span></span> <span data-ttu-id="4ac72-237">Výchozí hodnota je `internal` přístup.</span><span class="sxs-lookup"><span data-stu-id="4ac72-237">The default is `internal` access.</span></span>
*  <span data-ttu-id="4ac72-238">Může mít typy deklarované v třídách `public`, `protected internal`, `protected`, `internal`, nebo `private` přístup.</span><span class="sxs-lookup"><span data-stu-id="4ac72-238">Types declared in classes can have `public`, `protected internal`, `protected`, `internal`, or `private` access.</span></span> <span data-ttu-id="4ac72-239">Výchozí hodnota je `private` přístup.</span><span class="sxs-lookup"><span data-stu-id="4ac72-239">The default is `private` access.</span></span>
*  <span data-ttu-id="4ac72-240">Může mít typy deklarované ve strukturách `public`, `internal`, nebo `private` přístup.</span><span class="sxs-lookup"><span data-stu-id="4ac72-240">Types declared in structs can have `public`, `internal`, or `private` access.</span></span> <span data-ttu-id="4ac72-241">Výchozí hodnota je `private` přístup.</span><span class="sxs-lookup"><span data-stu-id="4ac72-241">The default is `private` access.</span></span>

## <a name="namespace-alias-qualifiers"></a><span data-ttu-id="4ac72-242">Kvalifikátory alias Namespace</span><span class="sxs-lookup"><span data-stu-id="4ac72-242">Namespace alias qualifiers</span></span>

<span data-ttu-id="4ac72-243">***Kvalifikátor aliasu oboru názvů*** `::` díky tomu je možné zaručit, že název prohledávání typů, které nejsou ovlivněny zavedení nové typy a členy.</span><span class="sxs-lookup"><span data-stu-id="4ac72-243">The ***namespace alias qualifier*** `::` makes it possible to guarantee that type name lookups are unaffected by the introduction of new types and members.</span></span> <span data-ttu-id="4ac72-244">Kvalifikátor aliasu oboru názvů se vždy zobrazuje mezi dva identifikátory, které jsou uvedené jako identifikátory levé a pravé.</span><span class="sxs-lookup"><span data-stu-id="4ac72-244">The namespace alias qualifier always appears between two identifiers referred to as the left-hand and right-hand identifiers.</span></span> <span data-ttu-id="4ac72-245">Na rozdíl od standardní `.` kvalifikátor, vlevo identifikátor `::` kvalifikátor se hledá nahoru pouze jako externího prvku nebo using alias.</span><span class="sxs-lookup"><span data-stu-id="4ac72-245">Unlike the regular `.` qualifier, the left-hand identifier of the `::` qualifier is looked up only as an extern or using alias.</span></span>

<span data-ttu-id="4ac72-246">A *qualified_alias_member* je definovaná následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="4ac72-246">A *qualified_alias_member* is defined as follows:</span></span>

```antlr
qualified_alias_member
    : identifier '::' identifier type_argument_list?
    ;
```

<span data-ttu-id="4ac72-247">A *qualified_alias_member* může sloužit jako *namespace_or_type_name* ([Namespace a zadejte názvy](basic-concepts.md#namespace-and-type-names)) nebo jako levý operand v *member_access* ([Přístup ke členu](expressions.md#member-access)).</span><span class="sxs-lookup"><span data-stu-id="4ac72-247">A *qualified_alias_member* can be used as a *namespace_or_type_name* ([Namespace and type names](basic-concepts.md#namespace-and-type-names)) or as the left operand in a *member_access* ([Member access](expressions.md#member-access)).</span></span>

<span data-ttu-id="4ac72-248">A *qualified_alias_member* má jednu z těchto dvou tvarů:</span><span class="sxs-lookup"><span data-stu-id="4ac72-248">A *qualified_alias_member* has one of two forms:</span></span>

*  <span data-ttu-id="4ac72-249">`N::I<A1, ..., Ak>`, kde `N` a `I` představují identifikátory, a `<A1, ..., Ak>` je seznam argumentů typu.</span><span class="sxs-lookup"><span data-stu-id="4ac72-249">`N::I<A1, ..., Ak>`, where `N` and `I` represent identifiers, and `<A1, ..., Ak>` is a type argument list.</span></span> <span data-ttu-id="4ac72-250">(`K` je vždy alespoň jednou.)</span><span class="sxs-lookup"><span data-stu-id="4ac72-250">(`K` is always at least one.)</span></span>
*  <span data-ttu-id="4ac72-251">`N::I`, kde `N` a `I` představují identifikátory.</span><span class="sxs-lookup"><span data-stu-id="4ac72-251">`N::I`, where `N` and `I` represent identifiers.</span></span> <span data-ttu-id="4ac72-252">(V tomto případě `K` je považován za nulu.)</span><span class="sxs-lookup"><span data-stu-id="4ac72-252">(In this case, `K` is considered to be zero.)</span></span>

<span data-ttu-id="4ac72-253">Použití tohoto zápisu význam *qualified_alias_member* je stanoven následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="4ac72-253">Using this notation, the meaning of a *qualified_alias_member* is determined as follows:</span></span>

*  <span data-ttu-id="4ac72-254">Pokud `N` je identifikátor `global`, pak se hledá globální obor názvů `I`:</span><span class="sxs-lookup"><span data-stu-id="4ac72-254">If `N` is the identifier `global`, then the global namespace is searched for `I`:</span></span>
   * <span data-ttu-id="4ac72-255">Pokud globální obor názvů obsahuje obor názvů s názvem `I` a `K` je nula, pak bude *qualified_alias_member* odkazuje na tento obor názvů.</span><span class="sxs-lookup"><span data-stu-id="4ac72-255">If the global namespace contains a namespace named `I` and `K` is zero, then the *qualified_alias_member* refers to that namespace.</span></span>
   * <span data-ttu-id="4ac72-256">Jinak, pokud je globální obor názvů obsahuje neobecný typ s názvem `I` a `K` je nula, pak bude *qualified_alias_member* odkazuje na typu.</span><span class="sxs-lookup"><span data-stu-id="4ac72-256">Otherwise, if the global namespace contains a non-generic type named `I` and `K` is zero, then the *qualified_alias_member* refers to that type.</span></span>
   * <span data-ttu-id="4ac72-257">Jinak, pokud je globální obor názvů obsahuje typ s názvem `I` , který má `K`  parametry typu, pak bude *qualified_alias_member* odkazuje na tento typ vytvořený s argumenty daného typu.</span><span class="sxs-lookup"><span data-stu-id="4ac72-257">Otherwise, if the global namespace contains a type named `I` that has `K` type parameters, then the *qualified_alias_member* refers to that type constructed with the given type arguments.</span></span>
   * <span data-ttu-id="4ac72-258">V opačném případě *qualified_alias_member* je nedefinovaný a dojde k chybě kompilace.</span><span class="sxs-lookup"><span data-stu-id="4ac72-258">Otherwise, the *qualified_alias_member* is undefined and a compile-time error occurs.</span></span>

*  <span data-ttu-id="4ac72-259">V opačném případě od deklarace oboru názvů ([deklarací Namespace](namespaces.md#namespace-declarations)) okamžitě obsahující *qualified_alias_member* (pokud existuje), pokračování s každou ohraničující deklarace oboru názvů (pokud existuje) a konče jednotku kompilace obsahující *qualified_alias_member*, následující kroky jsou vyhodnocen, dokud se entita nachází:</span><span class="sxs-lookup"><span data-stu-id="4ac72-259">Otherwise, starting with the namespace declaration ([Namespace declarations](namespaces.md#namespace-declarations)) immediately containing the *qualified_alias_member* (if any), continuing with each enclosing namespace declaration (if any), and ending with the compilation unit containing the *qualified_alias_member*, the following steps are evaluated until an entity is located:</span></span>

   * <span data-ttu-id="4ac72-260">Pokud obsahuje obor názvů deklarace nebo kompilace částí *using_alias_directive* , která přidruží `N` s typem, pak bude *qualified_alias_member* není definována a za kompilace dojde k chybě.</span><span class="sxs-lookup"><span data-stu-id="4ac72-260">If the namespace declaration or compilation unit contains a *using_alias_directive* that associates `N` with a type, then the *qualified_alias_member* is undefined and a compile-time error occurs.</span></span>
   * <span data-ttu-id="4ac72-261">Jinak, pokud obsahuje obor názvů deklarace nebo kompilace částí *extern_alias_directive* nebo *using_alias_directive* , která přidruží `N` s oborem názvů, pak:</span><span class="sxs-lookup"><span data-stu-id="4ac72-261">Otherwise, if the namespace declaration or compilation unit contains an *extern_alias_directive* or *using_alias_directive* that associates `N` with a namespace, then:</span></span>
     * <span data-ttu-id="4ac72-262">Pokud přidružené k oboru názvů `N` obsahuje obor názvů s názvem `I` a `K` je nula, pak bude *qualified_alias_member* odkazuje na tento obor názvů.</span><span class="sxs-lookup"><span data-stu-id="4ac72-262">If the namespace associated with `N` contains a namespace named `I` and `K` is zero, then the *qualified_alias_member* refers to that namespace.</span></span>
     * <span data-ttu-id="4ac72-263">Jinak, pokud přidružené k oboru názvů `N` obsahuje neobecný typ s názvem `I` a `K` je nula, pak bude *qualified_alias_member* odkazuje na typu.</span><span class="sxs-lookup"><span data-stu-id="4ac72-263">Otherwise, if the namespace associated with `N` contains a non-generic type named `I` and `K` is zero, then the *qualified_alias_member* refers to that type.</span></span>
     * <span data-ttu-id="4ac72-264">Jinak, pokud přidružené k oboru názvů `N` obsahuje typ s názvem `I` , který má `K`  parametry typu, pak bude *qualified_alias_member* označuje, že typ zkonstruován pomocí argumenty daného typu.</span><span class="sxs-lookup"><span data-stu-id="4ac72-264">Otherwise, if the namespace associated with `N` contains a type named `I` that has `K` type parameters, then the *qualified_alias_member* refers to that type constructed with the given type arguments.</span></span>
     * <span data-ttu-id="4ac72-265">V opačném případě *qualified_alias_member* je nedefinovaný a dojde k chybě kompilace.</span><span class="sxs-lookup"><span data-stu-id="4ac72-265">Otherwise, the *qualified_alias_member* is undefined and a compile-time error occurs.</span></span>
*  <span data-ttu-id="4ac72-266">V opačném případě *qualified_alias_member* je nedefinovaný a dojde k chybě kompilace.</span><span class="sxs-lookup"><span data-stu-id="4ac72-266">Otherwise, the *qualified_alias_member* is undefined and a compile-time error occurs.</span></span>

<span data-ttu-id="4ac72-267">Všimněte si, že pomocí kvalifikátor aliasu oboru názvů s aliasem, který odkazuje na typ způsobí chybu kompilace.</span><span class="sxs-lookup"><span data-stu-id="4ac72-267">Note that using the namespace alias qualifier with an alias that references a type causes a compile-time error.</span></span> <span data-ttu-id="4ac72-268">Všimněte si také, že pokud identifikátor `N` je `global`, pak vyhledávání se provádí v globálním oboru názvů, i v případě použití alias přidružení `global` s typem nebo oboru názvů.</span><span class="sxs-lookup"><span data-stu-id="4ac72-268">Also note that if the identifier `N` is `global`, then lookup is performed in the global namespace, even if there is a using alias associating `global` with a type or namespace.</span></span>

### <a name="uniqueness-of-aliases"></a><span data-ttu-id="4ac72-269">Jedinečnost aliasy</span><span class="sxs-lookup"><span data-stu-id="4ac72-269">Uniqueness of aliases</span></span>

<span data-ttu-id="4ac72-270">Každý kompilace částí a obor názvů subjekt má samostatné prohlášení místa pro aliasy extern a aliasy using.</span><span class="sxs-lookup"><span data-stu-id="4ac72-270">Each compilation unit and namespace body has a separate declaration space for extern aliases and using aliases.</span></span> <span data-ttu-id="4ac72-271">Proto i když název externí alias nebo using alias musí být jedinečný v rámci sady aliasy extern a aliasy using deklarované v okamžitě obsahující text jednotky nebo oboru názvů kompilace, alias smí obsahovat mít stejný název jako typ nebo obor názvů, pokud mám t se používá jenom s `::` kvalifikátoru.</span><span class="sxs-lookup"><span data-stu-id="4ac72-271">Thus, while the name of an extern alias or using alias must be unique within the set of extern aliases and using aliases declared in the immediately containing compilation unit or namespace body, an alias is permitted to have the same name as a type or namespace as long as it is used only with the `::` qualifier.</span></span>

<span data-ttu-id="4ac72-272">V příkladu</span><span class="sxs-lookup"><span data-stu-id="4ac72-272">In the example</span></span>
```csharp
namespace N
{
    public class A {}

    public class B {}
}

namespace N
{
    using A = System.IO;

    class X
    {
        A.Stream s1;            // Error, A is ambiguous

        A::Stream s2;           // Ok
    }
}
```
<span data-ttu-id="4ac72-273">název `A` má dvě možných významů v těle druhého oboru názvů, protože obě třídy `A` a using alias `A` nacházejí v oboru.</span><span class="sxs-lookup"><span data-stu-id="4ac72-273">the name `A` has two possible meanings in the second namespace body because both the class `A` and the using alias `A` are in scope.</span></span> <span data-ttu-id="4ac72-274">Z tohoto důvodu použití `A` v kvalifikovaném názvu `A.Stream` je nejednoznačný a způsobí chybu kompilace dojde k.</span><span class="sxs-lookup"><span data-stu-id="4ac72-274">For this reason, use of `A` in the qualified name `A.Stream` is ambiguous and causes a compile-time error to occur.</span></span> <span data-ttu-id="4ac72-275">Nicméně použití `A` s `::` kvalifikátor není chyba protože `A` se hledá jenom jako alias oboru názvů.</span><span class="sxs-lookup"><span data-stu-id="4ac72-275">However, use of `A` with the `::` qualifier is not an error because `A` is looked up only as a namespace alias.</span></span>
