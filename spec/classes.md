# <a name="classes"></a><span data-ttu-id="56f2d-101">Třídy</span><span class="sxs-lookup"><span data-stu-id="56f2d-101">Classes</span></span>

<span data-ttu-id="56f2d-102">Třída je datová struktura, která mohou obsahovat datové členy (konstant a polí), funkce členy (metody, vlastnosti, události, indexery, operátory, konstruktory instancí, destruktory a statické konstruktory) a vnořené typy.</span><span class="sxs-lookup"><span data-stu-id="56f2d-102">A class is a data structure that may contain data members (constants and fields), function members (methods, properties, events, indexers, operators, instance constructors, destructors and static constructors), and nested types.</span></span> <span data-ttu-id="56f2d-103">Typy tříd podporují dědičnost, mechanismus, kterým odvozené třídy můžete rozšířit a specialize základní třídy.</span><span class="sxs-lookup"><span data-stu-id="56f2d-103">Class types support inheritance, a mechanism whereby a derived class can extend and specialize a base class.</span></span>

## <a name="class-declarations"></a><span data-ttu-id="56f2d-104">Deklarace tříd</span><span class="sxs-lookup"><span data-stu-id="56f2d-104">Class declarations</span></span>

<span data-ttu-id="56f2d-105">A *class_declaration* je *type_declaration* ([typ deklarace](namespaces.md#type-declarations)), který deklaruje novou třídu.</span><span class="sxs-lookup"><span data-stu-id="56f2d-105">A *class_declaration* is a *type_declaration* ([Type declarations](namespaces.md#type-declarations)) that declares a new class.</span></span>

```antlr
class_declaration
    : attributes? class_modifier* 'partial'? 'class' identifier type_parameter_list?
      class_base? type_parameter_constraints_clause* class_body ';'?
    ;
```

<span data-ttu-id="56f2d-106">A *class_declaration* se skládá z volitelné sadu *atributy* ([atributy](attributes.md)) následovaný volitelná sada *class_modifier*s ([třídy modifikátory](classes.md#class-modifiers)), následovaným volitelnou `partial` modifikátor, za nímž následuje klíčové slovo `class` a *identifikátor* , názvy tříd, za nímž následuje volitelné *type_parameter_list* ([parametry typu](classes.md#type-parameters)), následovaným volitelnou *class_base* specifikace ([třídy base specifikace](classes.md#class-base-specification)) následovaný volitelná sada *type_parameter_constraints_clause*s ([omezení parametru typu](classes.md#type-parameter-constraints)) a po něm *class_body*  ([Tělo třídy](classes.md#class-body)), volitelně za nímž následuje středníkem.</span><span class="sxs-lookup"><span data-stu-id="56f2d-106">A *class_declaration* consists of an optional set of *attributes* ([Attributes](attributes.md)), followed by an optional set of *class_modifier*s ([Class modifiers](classes.md#class-modifiers)), followed by an optional `partial` modifier, followed by the keyword `class` and an *identifier* that names the class, followed by an optional *type_parameter_list* ([Type parameters](classes.md#type-parameters)), followed by an optional *class_base* specification ([Class base specification](classes.md#class-base-specification)) , followed by an optional set of *type_parameter_constraints_clause*s ([Type parameter constraints](classes.md#type-parameter-constraints)), followed by a *class_body* ([Class body](classes.md#class-body)), optionally followed by a semicolon.</span></span>

<span data-ttu-id="56f2d-107">Deklarace třídy nelze zadat *type_parameter_constraints_clause*s Pokud také poskytuje *type_parameter_list*.</span><span class="sxs-lookup"><span data-stu-id="56f2d-107">A class declaration cannot supply *type_parameter_constraints_clause*s unless it also supplies a *type_parameter_list*.</span></span>

<span data-ttu-id="56f2d-108">Deklarace třídy, která se dodává *type_parameter_list* je ***deklaraci obecné třídy***.</span><span class="sxs-lookup"><span data-stu-id="56f2d-108">A class declaration that supplies a *type_parameter_list* is a ***generic class declaration***.</span></span> <span data-ttu-id="56f2d-109">Kromě toho všechny třídy vnořené v deklaraci obecné třídy nebo obecnou strukturu prohlášení je samotný deklaraci obecné třídy protože parametry typu pro nadřazený typ musí být poskytnuty vytvořit konstruovaný typ.</span><span class="sxs-lookup"><span data-stu-id="56f2d-109">Additionally, any class nested inside a generic class declaration or a generic struct declaration is itself a generic class declaration, since type parameters for the containing type must be supplied to create a constructed type.</span></span>

### <a name="class-modifiers"></a><span data-ttu-id="56f2d-110">Modifikátory třídy</span><span class="sxs-lookup"><span data-stu-id="56f2d-110">Class modifiers</span></span>

<span data-ttu-id="56f2d-111">A *class_declaration* může volitelně zahrnovat posloupnost modifikátory třídy:</span><span class="sxs-lookup"><span data-stu-id="56f2d-111">A *class_declaration* may optionally include a sequence of class modifiers:</span></span>

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

<span data-ttu-id="56f2d-112">Je chyba kompilace pro stejný modifikátor objevit více než jednou v deklaraci třídy.</span><span class="sxs-lookup"><span data-stu-id="56f2d-112">It is a compile-time error for the same modifier to appear multiple times in a class declaration.</span></span>

<span data-ttu-id="56f2d-113">`new` Smí obsahovat modifikátor u vnořených tříd.</span><span class="sxs-lookup"><span data-stu-id="56f2d-113">The `new` modifier is permitted on nested classes.</span></span> <span data-ttu-id="56f2d-114">Určuje, že třída skrývá zděděného člena se stejným názvem, jak je popsáno v [new – modifikátor](classes.md#the-new-modifier).</span><span class="sxs-lookup"><span data-stu-id="56f2d-114">It specifies that the class hides an inherited member by the same name, as described in [The new modifier](classes.md#the-new-modifier).</span></span> <span data-ttu-id="56f2d-115">Je chyba kompilace pro `new` modifikátor se zobrazí v deklaraci třídy, která není deklarace vnořené třídy.</span><span class="sxs-lookup"><span data-stu-id="56f2d-115">It is a compile-time error for the `new` modifier to appear on a class declaration that is not a nested class declaration.</span></span>

<span data-ttu-id="56f2d-116">`public`, `protected`, `internal`, A `private` modifikátory řídit přístupnost člena třídy.</span><span class="sxs-lookup"><span data-stu-id="56f2d-116">The `public`, `protected`, `internal`, and `private` modifiers control the accessibility of the class.</span></span> <span data-ttu-id="56f2d-117">V závislosti na kontextu, ve kterém dochází k deklaraci třídy, některé z těchto modifikátorů nemusí být povoleno ([deklarovaná přístupnost](basic-concepts.md#declared-accessibility)).</span><span class="sxs-lookup"><span data-stu-id="56f2d-117">Depending on the context in which the class declaration occurs, some of these modifiers may not be permitted ([Declared accessibility](basic-concepts.md#declared-accessibility)).</span></span>

<span data-ttu-id="56f2d-118">`abstract`, `sealed` a `static` modifikátory jsou popsány v následujících částech.</span><span class="sxs-lookup"><span data-stu-id="56f2d-118">The `abstract`, `sealed` and `static` modifiers are discussed in the following sections.</span></span>

#### <a name="abstract-classes"></a><span data-ttu-id="56f2d-119">Abstraktní třídy</span><span class="sxs-lookup"><span data-stu-id="56f2d-119">Abstract classes</span></span>

<span data-ttu-id="56f2d-120">`abstract` Modifikátor se používá k označení, že třída je neúplný a že je určena pro použití pouze jako základní třídu.</span><span class="sxs-lookup"><span data-stu-id="56f2d-120">The `abstract` modifier is used to indicate that a class is incomplete and that it is intended to be used only as a base class.</span></span> <span data-ttu-id="56f2d-121">Abstraktní třída se liší od neabstraktní třídy následujícími způsoby:</span><span class="sxs-lookup"><span data-stu-id="56f2d-121">An abstract class differs from a non-abstract class in the following ways:</span></span>

*  <span data-ttu-id="56f2d-122">Abstraktní třídu nelze přímo vytvořit instanci a je chyba kompilace pro použití `new` operátor u abstraktní třídy.</span><span class="sxs-lookup"><span data-stu-id="56f2d-122">An abstract class cannot be instantiated directly, and it is a compile-time error to use the `new` operator on an abstract class.</span></span> <span data-ttu-id="56f2d-123">I když je možné mít proměnných a hodnot, jejichž typy za kompilace, je abstraktních, tyto proměnné a jejich hodnoty nutně bude buď `null` nebo obsahovat odkazy na instance neabstraktní třídy odvozené od abstraktní typy.</span><span class="sxs-lookup"><span data-stu-id="56f2d-123">While it is possible to have variables and values whose compile-time types are abstract, such variables and values will necessarily either be `null` or contain references to instances of non-abstract classes derived from the abstract types.</span></span>
*  <span data-ttu-id="56f2d-124">Abstraktní třídy je povoleno (ale nevyžaduje) tak, aby obsahovala abstraktní členy.</span><span class="sxs-lookup"><span data-stu-id="56f2d-124">An abstract class is permitted (but not required) to contain abstract members.</span></span>
*  <span data-ttu-id="56f2d-125">Abstraktní třída nemůže být zapečetěný.</span><span class="sxs-lookup"><span data-stu-id="56f2d-125">An abstract class cannot be sealed.</span></span>

<span data-ttu-id="56f2d-126">Když neabstraktní třídy je odvozen z abstraktní třídy, Neabstraktní třída musí obsahovat Skutečná implementace všechny zděděné abstraktní členy přepíše abstraktní členy.</span><span class="sxs-lookup"><span data-stu-id="56f2d-126">When a non-abstract class is derived from an abstract class, the non-abstract class must include actual implementations of all inherited abstract members, thereby overriding those abstract members.</span></span> <span data-ttu-id="56f2d-127">V příkladu</span><span class="sxs-lookup"><span data-stu-id="56f2d-127">In the example</span></span>
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
<span data-ttu-id="56f2d-128">abstraktní třída `A` představuje abstraktní metody `F`.</span><span class="sxs-lookup"><span data-stu-id="56f2d-128">the abstract class `A` introduces an abstract method `F`.</span></span> <span data-ttu-id="56f2d-129">Třída `B` zavádí další metodu `G`, ale protože neposkytuje implementaci `F`, `B` také musí být deklarovaná jako abstraktní.</span><span class="sxs-lookup"><span data-stu-id="56f2d-129">Class `B` introduces an additional method `G`, but since it doesn't provide an implementation of `F`, `B` must also be declared abstract.</span></span> <span data-ttu-id="56f2d-130">Třída `C` přepíše `F` a poskytuje skutečné implementaci.</span><span class="sxs-lookup"><span data-stu-id="56f2d-130">Class `C` overrides `F` and provides an actual implementation.</span></span> <span data-ttu-id="56f2d-131">Vzhledem k tomu, že neexistují žádní abstraktní členové v `C`, `C` je povoleno (ale nevyžaduje) být neabstraktní.</span><span class="sxs-lookup"><span data-stu-id="56f2d-131">Since there are no abstract members in `C`, `C` is permitted (but not required) to be non-abstract.</span></span>

#### <a name="sealed-classes"></a><span data-ttu-id="56f2d-132">Zapečetěné třídy</span><span class="sxs-lookup"><span data-stu-id="56f2d-132">Sealed classes</span></span>

<span data-ttu-id="56f2d-133">`sealed` Modifikátor se používá při prevenci odvozování z třídy.</span><span class="sxs-lookup"><span data-stu-id="56f2d-133">The `sealed` modifier is used to prevent derivation from a class.</span></span> <span data-ttu-id="56f2d-134">Chyba kompilace nastane, pokud zapečetěné třídě je zadán jako základní třídou jiné třídy.</span><span class="sxs-lookup"><span data-stu-id="56f2d-134">A compile-time error occurs if a sealed class is specified as the base class of another class.</span></span>

<span data-ttu-id="56f2d-135">Zapečetěná třída nemůže být také abstraktní třídu.</span><span class="sxs-lookup"><span data-stu-id="56f2d-135">A sealed class cannot also be an abstract class.</span></span>

<span data-ttu-id="56f2d-136">`sealed` Modifikátor se používá především zabránit neúmyslnému odvození, ale umožňuje také některé optimalizace za běhu.</span><span class="sxs-lookup"><span data-stu-id="56f2d-136">The `sealed` modifier is primarily used to prevent unintended derivation, but it also enables certain run-time optimizations.</span></span> <span data-ttu-id="56f2d-137">Zejména protože zapečetěná třída se ví, nikdy nemůžete mít všechny odvozené třídy, je možné transformovat volání virtuální funkce člen v zapečetěné třídě instancí na nevirtuální volání.</span><span class="sxs-lookup"><span data-stu-id="56f2d-137">In particular, because a sealed class is known to never have any derived classes, it is possible to transform virtual function member invocations on sealed class instances into non-virtual invocations.</span></span>

#### <a name="static-classes"></a><span data-ttu-id="56f2d-138">Statické třídy</span><span class="sxs-lookup"><span data-stu-id="56f2d-138">Static classes</span></span>

<span data-ttu-id="56f2d-139">`static` Modifikátor se používá k označení třídy deklarované jako ***statické třídy***.</span><span class="sxs-lookup"><span data-stu-id="56f2d-139">The `static` modifier is used to mark the class being declared as a ***static class***.</span></span> <span data-ttu-id="56f2d-140">Statická třída se nedá vytvořit instance, nelze použít jako typ a může obsahovat pouze statické členy.</span><span class="sxs-lookup"><span data-stu-id="56f2d-140">A static class cannot be instantiated, cannot be used as a type and can contain only static members.</span></span> <span data-ttu-id="56f2d-141">Pouze statické třídy mohou obsahovat deklarace metody rozšíření ([rozšiřující metody](classes.md#extension-methods)).</span><span class="sxs-lookup"><span data-stu-id="56f2d-141">Only a static class can contain declarations of extension methods ([Extension methods](classes.md#extension-methods)).</span></span>

<span data-ttu-id="56f2d-142">Statická třída deklarace je v souladu s následujícími omezeními:</span><span class="sxs-lookup"><span data-stu-id="56f2d-142">A static class declaration is subject to the following restrictions:</span></span>

*  <span data-ttu-id="56f2d-143">Nesmí obsahovat statické třídy `sealed` nebo `abstract` modifikátor.</span><span class="sxs-lookup"><span data-stu-id="56f2d-143">A static class may not include a `sealed` or `abstract` modifier.</span></span> <span data-ttu-id="56f2d-144">Všimněte si však, že od statické třídy nelze vytvořit instanci nebo odvozený od, se chová jako by byl uzavřený a abstraktní.</span><span class="sxs-lookup"><span data-stu-id="56f2d-144">Note, however, that since a static class cannot be instantiated or derived from, it behaves as if it was both sealed and abstract.</span></span>
*  <span data-ttu-id="56f2d-145">Nesmí obsahovat statické třídy *class_base* specifikace ([specifikace základní třídy](classes.md#class-base-specification)) a nelze explicitně zadat základní třídu nebo seznamu implementovaných rozhraní.</span><span class="sxs-lookup"><span data-stu-id="56f2d-145">A static class may not include a *class_base* specification ([Class base specification](classes.md#class-base-specification)) and cannot explicitly specify a base class or a list of implemented interfaces.</span></span> <span data-ttu-id="56f2d-146">Statické třídy implicitně dědí z typu `object`.</span><span class="sxs-lookup"><span data-stu-id="56f2d-146">A static class implicitly inherits from type `object`.</span></span>
*  <span data-ttu-id="56f2d-147">Statická třída může obsahovat pouze statické členy ([statické a instance členy](classes.md#static-and-instance-members)).</span><span class="sxs-lookup"><span data-stu-id="56f2d-147">A static class can only contain static members ([Static and instance members](classes.md#static-and-instance-members)).</span></span> <span data-ttu-id="56f2d-148">Všimněte si, že konstanty a vnořené typy jsou klasifikovány jako statické členy.</span><span class="sxs-lookup"><span data-stu-id="56f2d-148">Note that constants and nested types are classified as static members.</span></span>
*  <span data-ttu-id="56f2d-149">Statické třídy nemohou obsahovat členy s `protected` nebo `protected internal` deklarovaná přístupnost.</span><span class="sxs-lookup"><span data-stu-id="56f2d-149">A static class cannot have members with `protected` or `protected internal` declared accessibility.</span></span>

<span data-ttu-id="56f2d-150">Je chyba kompilace na kterékoli z těchto omezení porušují.</span><span class="sxs-lookup"><span data-stu-id="56f2d-150">It is a compile-time error to violate any of these restrictions.</span></span>

<span data-ttu-id="56f2d-151">Statická třída nemá žádné konstruktory instancí.</span><span class="sxs-lookup"><span data-stu-id="56f2d-151">A static class has no instance constructors.</span></span> <span data-ttu-id="56f2d-152">Není možné deklarovat konstruktor instance ve statické třídě a žádný výchozí konstruktor instance ([výchozí konstruktory](classes.md#default-constructors)) je k dispozici pro statické třídě.</span><span class="sxs-lookup"><span data-stu-id="56f2d-152">It is not possible to declare an instance constructor in a static class, and no default instance constructor ([Default constructors](classes.md#default-constructors)) is provided for a static class.</span></span>

<span data-ttu-id="56f2d-153">Členové statické třídy nejsou automaticky statické, a explicitně musí obsahovat deklarace členů `static` modifikátor (s výjimkou konstanty a vnořené typy).</span><span class="sxs-lookup"><span data-stu-id="56f2d-153">The members of a static class are not automatically static, and the member declarations must explicitly include a `static` modifier (except for constants and nested types).</span></span> <span data-ttu-id="56f2d-154">Když v rámci statických vnější třídy je vnořená třída, vnořené třídy není statická třída Pokud explicitně zahrnuje `static` modifikátor.</span><span class="sxs-lookup"><span data-stu-id="56f2d-154">When a class is nested within a static outer class, the nested class is not a static class unless it explicitly includes a `static` modifier.</span></span>

<span data-ttu-id="56f2d-155">__Odkazování na typy statických tříd__</span><span class="sxs-lookup"><span data-stu-id="56f2d-155">__Referencing static class types__</span></span>

<span data-ttu-id="56f2d-156">A *namespace_or_type_name* ([Namespace a zadejte názvy](basic-concepts.md#namespace-and-type-names)) odkazu statické třídy, pokud je povoleno</span><span class="sxs-lookup"><span data-stu-id="56f2d-156">A *namespace_or_type_name* ([Namespace and type names](basic-concepts.md#namespace-and-type-names)) is permitted to reference a static class if</span></span>

*  <span data-ttu-id="56f2d-157">*Namespace_or_type_name* je `T` v *namespace_or_type_name* formuláře `T.I`, nebo</span><span class="sxs-lookup"><span data-stu-id="56f2d-157">The *namespace_or_type_name* is the `T` in a *namespace_or_type_name* of the form `T.I`, or</span></span>
*  <span data-ttu-id="56f2d-158">*Namespace_or_type_name* je `T` v *typeof_expression* ([seznamy argumentů](expressions.md#argument-lists)1) ve formátu `typeof(T)`.</span><span class="sxs-lookup"><span data-stu-id="56f2d-158">The *namespace_or_type_name* is the `T` in a *typeof_expression* ([Argument lists](expressions.md#argument-lists)1) of the form `typeof(T)`.</span></span>

<span data-ttu-id="56f2d-159">A *primary_expression* ([funkce členy](expressions.md#function-members)) odkazu statické třídy, pokud je povoleno</span><span class="sxs-lookup"><span data-stu-id="56f2d-159">A *primary_expression* ([Function members](expressions.md#function-members)) is permitted to reference a static class if</span></span>

*  <span data-ttu-id="56f2d-160">*Primary_expression* je `E` v *member_access* ([kompilace kontrolu dynamické přetížení](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) ve formátu `E.I`.</span><span class="sxs-lookup"><span data-stu-id="56f2d-160">The *primary_expression* is the `E` in a *member_access* ([Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) of the form `E.I`.</span></span>

<span data-ttu-id="56f2d-161">V libovolném kontextu je chyba kompilace odkazovat statické třídě.</span><span class="sxs-lookup"><span data-stu-id="56f2d-161">In any other context it is a compile-time error to reference a static class.</span></span> <span data-ttu-id="56f2d-162">Například, jedná se o chybu pro statické třídy má být použit jako základní třídu, základní typ ([vnořené typy](classes.md#nested-types)) člena, argument obecného typu nebo omezení parametru typu.</span><span class="sxs-lookup"><span data-stu-id="56f2d-162">For example, it is an error for a static class to be used as a base class, a constituent type ([Nested types](classes.md#nested-types)) of a member, a generic type argument, or a type parameter constraint.</span></span> <span data-ttu-id="56f2d-163">Obdobně statické třídy nelze použít v typu pole, typ ukazatele, `new` výraz, výraz přetypování `is` výrazu, `as` výrazu, `sizeof` výraz nebo výraz výchozí hodnoty.</span><span class="sxs-lookup"><span data-stu-id="56f2d-163">Likewise, a static class cannot be used in an array type, a pointer type, a `new` expression, a cast expression, an `is` expression, an `as` expression, a `sizeof` expression, or a default value expression.</span></span>

### <a name="partial-modifier"></a><span data-ttu-id="56f2d-164">Částečný modifikátor</span><span class="sxs-lookup"><span data-stu-id="56f2d-164">Partial modifier</span></span>

<span data-ttu-id="56f2d-165">`partial` Modifikátor se používá k označení, že tento *class_declaration* je částečný typ deklarace.</span><span class="sxs-lookup"><span data-stu-id="56f2d-165">The `partial` modifier is used to indicate that this *class_declaration* is a partial type declaration.</span></span> <span data-ttu-id="56f2d-166">Více deklaracích částečné typu se stejným názvem v rámci nadřazeného oboru názvů nebo typ deklarace se dá formuláře jeden typ deklarace, dle pravidel uvedených v [částečné typy](classes.md#partial-types).</span><span class="sxs-lookup"><span data-stu-id="56f2d-166">Multiple partial type declarations with the same name within an enclosing namespace or type declaration combine to form one type declaration, following the rules specified in [Partial types](classes.md#partial-types).</span></span>

<span data-ttu-id="56f2d-167">S deklarace třídy distribuované přes oddělených segmentech textem programu může být užitečné, pokud jsou tyto segmenty vytvořeného nebo udržovat v různých kontextech.</span><span class="sxs-lookup"><span data-stu-id="56f2d-167">Having the declaration of a class distributed over separate segments of program text can be useful if these segments are produced or maintained in different contexts.</span></span> <span data-ttu-id="56f2d-168">Jednou ze součástí sady deklaraci třídy, může být počítače, generovány, zatímco druhá je ručně vytvořená.</span><span class="sxs-lookup"><span data-stu-id="56f2d-168">For instance, one part of a class declaration may be machine generated, whereas the other is manually authored.</span></span> <span data-ttu-id="56f2d-169">Textové oddělení dvě zakáže aktualizace jednou v konfliktu s aktualizacemi jiným.</span><span class="sxs-lookup"><span data-stu-id="56f2d-169">Textual separation of the two prevents updates by one from conflicting with updates by the other.</span></span>

### <a name="type-parameters"></a><span data-ttu-id="56f2d-170">Parametry typu</span><span class="sxs-lookup"><span data-stu-id="56f2d-170">Type parameters</span></span>

<span data-ttu-id="56f2d-171">Parametr typu je jednoduchý identifikátor, který označuje zástupný symbol pro argument typu zadaný konstruovaný typ vytvoření.</span><span class="sxs-lookup"><span data-stu-id="56f2d-171">A type parameter is a simple identifier that denotes a placeholder for a type argument supplied to create a constructed type.</span></span> <span data-ttu-id="56f2d-172">Parametr typu je formální zástupný symbol pro typ, který budou poskytnuty později.</span><span class="sxs-lookup"><span data-stu-id="56f2d-172">A type parameter is a formal placeholder for a type that will be supplied later.</span></span> <span data-ttu-id="56f2d-173">Naopak argument typu ([argumenty typu](types.md#type-arguments)) je skutečný typ, který nahrazuje pro parametr typu, když se vytvoří konstruovaný typ.</span><span class="sxs-lookup"><span data-stu-id="56f2d-173">By contrast, a type argument ([Type arguments](types.md#type-arguments)) is the actual type that is substituted for the type parameter when a constructed type is created.</span></span>

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

<span data-ttu-id="56f2d-174">Každý z parametrů typu v deklaraci třídy definuje název v deklaraci prostoru ([deklarace](basic-concepts.md#declarations)) této třídy.</span><span class="sxs-lookup"><span data-stu-id="56f2d-174">Each type parameter in a class declaration defines a name in the declaration space ([Declarations](basic-concepts.md#declarations)) of that class.</span></span> <span data-ttu-id="56f2d-175">Proto nemůže mít stejný název jako jiný typ parametru nebo člen deklarovaný v dané třídě.</span><span class="sxs-lookup"><span data-stu-id="56f2d-175">Thus, it cannot have the same name as another type parameter or a member declared in that class.</span></span> <span data-ttu-id="56f2d-176">Parametr typu nemůže mít stejný název jako samotného typu.</span><span class="sxs-lookup"><span data-stu-id="56f2d-176">A type parameter cannot have the same name as the type itself.</span></span>

### <a name="class-base-specification"></a><span data-ttu-id="56f2d-177">Specifikace základní třídy</span><span class="sxs-lookup"><span data-stu-id="56f2d-177">Class base specification</span></span>

<span data-ttu-id="56f2d-178">Může obsahovat deklaraci třídy *class_base* specifikace, která definuje přímou základní třídu třídy a rozhraní ([rozhraní](interfaces.md)) přímo implementováno třídou.</span><span class="sxs-lookup"><span data-stu-id="56f2d-178">A class declaration may include a *class_base* specification, which defines the direct base class of the class and the interfaces ([Interfaces](interfaces.md)) directly implemented by the class.</span></span>

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

<span data-ttu-id="56f2d-179">Základní třída zadaná v deklaraci třídy může být typem konstruovaný třídy ([vytvořený typy](types.md#constructed-types)).</span><span class="sxs-lookup"><span data-stu-id="56f2d-179">The base class specified in a class declaration can be a constructed class type ([Constructed types](types.md#constructed-types)).</span></span> <span data-ttu-id="56f2d-180">Základní třída nemůže být parametr typu sama o sobě, i když to může zahrnovat parametry typu, které jsou v oboru.</span><span class="sxs-lookup"><span data-stu-id="56f2d-180">A base class cannot be a type parameter on its own, though it can involve the type parameters that are in scope.</span></span>

```csharp
class Extend<V>: V {}            // Error, type parameter used as base class
```

#### <a name="base-classes"></a><span data-ttu-id="56f2d-181">Základní třídy</span><span class="sxs-lookup"><span data-stu-id="56f2d-181">Base classes</span></span>

<span data-ttu-id="56f2d-182">Když *class_type* je součástí *class_base*, určuje přímou základní třídu třídy deklarované.</span><span class="sxs-lookup"><span data-stu-id="56f2d-182">When a *class_type* is included in the *class_base*, it specifies the direct base class of the class being declared.</span></span> <span data-ttu-id="56f2d-183">Pokud deklarace třídy nemá žádné *class_base*, nebo pokud *class_base* seznamy pouze typy rozhraní, přímou základní třídu je považován za `object`.</span><span class="sxs-lookup"><span data-stu-id="56f2d-183">If a class declaration has no *class_base*, or if the *class_base* lists only interface types, the direct base class is assumed to be `object`.</span></span> <span data-ttu-id="56f2d-184">Třída dědí členy z její přímé základní třídy, jak je popsáno v [dědičnosti](classes.md#inheritance).</span><span class="sxs-lookup"><span data-stu-id="56f2d-184">A class inherits members from its direct base class, as described in [Inheritance](classes.md#inheritance).</span></span>

<span data-ttu-id="56f2d-185">V příkladu</span><span class="sxs-lookup"><span data-stu-id="56f2d-185">In the example</span></span>
```csharp
class A {}

class B: A {}
```
<span data-ttu-id="56f2d-186">Třída `A` je označen jako přímé základní třídy `B`, a `B` se říká, že nelze odvodit z `A`.</span><span class="sxs-lookup"><span data-stu-id="56f2d-186">class `A` is said to be the direct base class of `B`, and `B` is said to be derived from `A`.</span></span> <span data-ttu-id="56f2d-187">Protože `A` nemá explicitně zadat přímou základní třídu, přímou základní třídu je implicitně `object`.</span><span class="sxs-lookup"><span data-stu-id="56f2d-187">Since `A` does not explicitly specify a direct base class, its direct base class is implicitly `object`.</span></span>

<span data-ttu-id="56f2d-188">Pro typ vytvořeného třídy, pokud je zadaná základní třída v deklaraci obecné třídy základní třídy pro konstruovaný typ. se získá nahrazování pro každou *type_parameter* v deklaraci základní třídy, odpovídající *type_argument* sestaveného typu.</span><span class="sxs-lookup"><span data-stu-id="56f2d-188">For a constructed class type, if a base class is specified in the generic class declaration, the base class of the constructed type is obtained by substituting, for each *type_parameter* in the base class declaration, the corresponding *type_argument* of the constructed type.</span></span> <span data-ttu-id="56f2d-189">Zadané deklarace obecných tříd</span><span class="sxs-lookup"><span data-stu-id="56f2d-189">Given the generic class declarations</span></span>
```csharp
class B<U,V> {...}

class G<T>: B<string,T[]> {...}
```
<span data-ttu-id="56f2d-190">Základní třída konstruovaný typ `G<int>` by `B<string,int[]>`.</span><span class="sxs-lookup"><span data-stu-id="56f2d-190">the base class of the constructed type `G<int>` would be `B<string,int[]>`.</span></span>

<span data-ttu-id="56f2d-191">Přímou základní třídu typu třídy musí být přinejmenším stejně dostupná jako typ třídy ([usnadnění domén](basic-concepts.md#accessibility-domains)).</span><span class="sxs-lookup"><span data-stu-id="56f2d-191">The direct base class of a class type must be at least as accessible as the class type itself ([Accessibility domains](basic-concepts.md#accessibility-domains)).</span></span> <span data-ttu-id="56f2d-192">Například je chyba kompilace pro `public` odvodit z třídy `private` nebo `internal` třídy.</span><span class="sxs-lookup"><span data-stu-id="56f2d-192">For example, it is a compile-time error for a `public` class to derive from a `private` or `internal` class.</span></span>

<span data-ttu-id="56f2d-193">Přímou základní třídu typu třídy nesmí být některý z následujících typů: `System.Array`, `System.Delegate`, `System.MulticastDelegate`, `System.Enum`, nebo `System.ValueType`.</span><span class="sxs-lookup"><span data-stu-id="56f2d-193">The direct base class of a class type must not be any of the following types: `System.Array`, `System.Delegate`, `System.MulticastDelegate`, `System.Enum`, or `System.ValueType`.</span></span> <span data-ttu-id="56f2d-194">Kromě toho nelze použít deklaraci obecné třídy `System.Attribute` jako přímou nebo nepřímou základní třídy.</span><span class="sxs-lookup"><span data-stu-id="56f2d-194">Furthermore, a generic class declaration cannot use `System.Attribute` as a direct or indirect base class.</span></span>

<span data-ttu-id="56f2d-195">Při určování význam specifikaci přímou základní třídu `A` třídy `B`, přímé základní třídy `B` je dočasně předpokládá se, že `object`.</span><span class="sxs-lookup"><span data-stu-id="56f2d-195">While determining the meaning of the direct base class specification `A` of a class `B`, the direct base class of `B` is temporarily assumed to be `object`.</span></span> <span data-ttu-id="56f2d-196">Intuitivně tím se zajistí, že význam specifikace základní třídy nemůže rekurzivně závisí sám na sobě.</span><span class="sxs-lookup"><span data-stu-id="56f2d-196">Intuitively this ensures that the meaning of a base class specification cannot recursively depend on itself.</span></span> <span data-ttu-id="56f2d-197">Příklad:</span><span class="sxs-lookup"><span data-stu-id="56f2d-197">The example:</span></span>
```csharp
class A<T> {
   public class B {}
}

class C : A<C.B> {}
```
<span data-ttu-id="56f2d-198">došlo k chybě od specifikace základní třídy `A<C.B>` přímé základní třídy `C` se považuje za `object`a proto (podle pravidel objektů [Namespace a zadejte názvy](basic-concepts.md#namespace-and-type-names)) `C` není považována za jste členem `B`.</span><span class="sxs-lookup"><span data-stu-id="56f2d-198">is in error since in the base class specification `A<C.B>` the direct base class of `C` is considered to be `object`, and hence (by the rules of [Namespace and type names](basic-concepts.md#namespace-and-type-names))  `C` is not considered to have a member `B`.</span></span>

<span data-ttu-id="56f2d-199">Základní třídy typu třídy jsou přímou základní třídu a její základní třídy.</span><span class="sxs-lookup"><span data-stu-id="56f2d-199">The base classes of a class type are the direct base class and its base classes.</span></span> <span data-ttu-id="56f2d-200">Jinými slovy sadu základních tříd je přenositelný uzavření relace přímou základní třídu.</span><span class="sxs-lookup"><span data-stu-id="56f2d-200">In other words, the set of base classes is the transitive closure of the direct base class relationship.</span></span> <span data-ttu-id="56f2d-201">V příkladu výše, základní třídy odkazující `B` jsou `A` a `object`.</span><span class="sxs-lookup"><span data-stu-id="56f2d-201">Referring to the example above, the base classes of `B` are `A` and `object`.</span></span> <span data-ttu-id="56f2d-202">V příkladu</span><span class="sxs-lookup"><span data-stu-id="56f2d-202">In the example</span></span>
```csharp
class A {...}

class B<T>: A {...}

class C<T>: B<IComparable<T>> {...}

class D<T>: C<T[]> {...}
```
<span data-ttu-id="56f2d-203">základní třídy `D<int>` jsou `C<int[]>`, `B<IComparable<int[]>>`, `A`, a `object`.</span><span class="sxs-lookup"><span data-stu-id="56f2d-203">the base classes of `D<int>` are `C<int[]>`, `B<IComparable<int[]>>`, `A`, and `object`.</span></span>

<span data-ttu-id="56f2d-204">S výjimkou třídy `object`, každý typ třída nemá přesně jednu přímou základní třídu.</span><span class="sxs-lookup"><span data-stu-id="56f2d-204">Except for class `object`, every class type has exactly one direct base class.</span></span> <span data-ttu-id="56f2d-205">`object` Třída nemá žádné přímé základní třídy a je ultimate základní třídou jiné třídy.</span><span class="sxs-lookup"><span data-stu-id="56f2d-205">The `object` class has no direct base class and is the ultimate base class of all other classes.</span></span>

<span data-ttu-id="56f2d-206">Pokud třída `B` je odvozena z třídy `A`, je chyba kompilace pro `A` závisí na `B`.</span><span class="sxs-lookup"><span data-stu-id="56f2d-206">When a class `B` derives from a class `A`, it is a compile-time error for `A` to depend on `B`.</span></span> <span data-ttu-id="56f2d-207">Třída ***přímo závisí na*** své přímé základní třídy (pokud existuje) a ***přímo závisí na*** třídy, ve kterém je okamžitě vnořený (pokud existuje).</span><span class="sxs-lookup"><span data-stu-id="56f2d-207">A class ***directly depends on*** its direct base class (if any) and ***directly depends on*** the class within which it is immediately nested (if any).</span></span> <span data-ttu-id="56f2d-208">Při této definici, kompletní sadu tříd, na kterých závisí třída je uzavření reflexivní a přenosné ***přímo závisí na*** vztah.</span><span class="sxs-lookup"><span data-stu-id="56f2d-208">Given this definition, the complete set of classes upon which a class depends is the reflexive and transitive closure of the ***directly depends on*** relationship.</span></span>

<span data-ttu-id="56f2d-209">V příkladu</span><span class="sxs-lookup"><span data-stu-id="56f2d-209">The example</span></span>
```csharp
class A: A {}
```
<span data-ttu-id="56f2d-210">protože třída závisí sám na sobě je chybné.</span><span class="sxs-lookup"><span data-stu-id="56f2d-210">is erroneous because the class depends on itself.</span></span> <span data-ttu-id="56f2d-211">Podobně příklad</span><span class="sxs-lookup"><span data-stu-id="56f2d-211">Likewise, the example</span></span>
```csharp
class A: B {}
class B: C {}
class C: A {}
```
<span data-ttu-id="56f2d-212">je v chybě, protože třídy záviset sama na sebe záviset samy na sobě.</span><span class="sxs-lookup"><span data-stu-id="56f2d-212">is in error because the classes circularly depend on themselves.</span></span> <span data-ttu-id="56f2d-213">Nakonec příklad</span><span class="sxs-lookup"><span data-stu-id="56f2d-213">Finally, the example</span></span>
```csharp
class A: B.C {}

class B: A
{
    public class C {}
}
```
<span data-ttu-id="56f2d-214">výsledkem chyba kompilace, protože `A` závisí na `B.C` (své přímé základní třídy), závisí na `B` (jeho okamžitě ohraničující třídy), který odkazuje cyklicky sám závisí na `A`.</span><span class="sxs-lookup"><span data-stu-id="56f2d-214">results in a compile-time error because `A` depends on `B.C` (its direct base class), which depends on `B` (its immediately enclosing class), which circularly depends on `A`.</span></span>

<span data-ttu-id="56f2d-215">Všimněte si, že třída není závislý na třídy, které jsou v něm vnořený.</span><span class="sxs-lookup"><span data-stu-id="56f2d-215">Note that a class does not depend on the classes that are nested within it.</span></span> <span data-ttu-id="56f2d-216">V příkladu</span><span class="sxs-lookup"><span data-stu-id="56f2d-216">In the example</span></span>
```csharp
class A
{
    class B: A {}
}
```
<span data-ttu-id="56f2d-217">`B` závisí na `A` (protože `A` přímou základní třídu a její okamžitě ohraničující třídy), ale `A` nezávisí na `B` (protože `B` není základní třídu ani nadřazené třídu `A` ).</span><span class="sxs-lookup"><span data-stu-id="56f2d-217">`B` depends on `A` (because `A` is both its direct base class and its immediately enclosing class), but `A` does not depend on `B` (since `B` is neither a base class nor an enclosing class of `A`).</span></span> <span data-ttu-id="56f2d-218">V příkladu je proto platný.</span><span class="sxs-lookup"><span data-stu-id="56f2d-218">Thus, the example is valid.</span></span>

<span data-ttu-id="56f2d-219">Není možné odvodit z `sealed` třídy.</span><span class="sxs-lookup"><span data-stu-id="56f2d-219">It is not possible to derive from a `sealed` class.</span></span> <span data-ttu-id="56f2d-220">V příkladu</span><span class="sxs-lookup"><span data-stu-id="56f2d-220">In the example</span></span>
```csharp
sealed class A {}

class B: A {}            // Error, cannot derive from a sealed class
```
<span data-ttu-id="56f2d-221">Třída `B` je v chybě, protože se pokusí o odvozovat `sealed` třídy `A`.</span><span class="sxs-lookup"><span data-stu-id="56f2d-221">class `B` is in error because it attempts to derive from the `sealed` class `A`.</span></span>

#### <a name="interface-implementations"></a><span data-ttu-id="56f2d-222">Implementace rozhraní</span><span class="sxs-lookup"><span data-stu-id="56f2d-222">Interface implementations</span></span>

<span data-ttu-id="56f2d-223">A *class_base* specifikace může obsahovat seznam typů rozhraní, ve kterých případ třídu se říká, že přímo implementaci typů dané rozhraní.</span><span class="sxs-lookup"><span data-stu-id="56f2d-223">A *class_base* specification may include a list of interface types, in which case the class is said to directly implement the given interface types.</span></span> <span data-ttu-id="56f2d-224">Implementace rozhraní jsou popsány dále v [rozhraní implementace](interfaces.md#interface-implementations).</span><span class="sxs-lookup"><span data-stu-id="56f2d-224">Interface implementations are discussed further in [Interface implementations](interfaces.md#interface-implementations).</span></span>

### <a name="type-parameter-constraints"></a><span data-ttu-id="56f2d-225">Omezení parametru typu</span><span class="sxs-lookup"><span data-stu-id="56f2d-225">Type parameter constraints</span></span>

<span data-ttu-id="56f2d-226">Obecná deklarace typů a metod, můžete volitelně zadat omezení parametru typu zahrnutím *type_parameter_constraints_clause*s.</span><span class="sxs-lookup"><span data-stu-id="56f2d-226">Generic type and method declarations can optionally specify type parameter constraints by including *type_parameter_constraints_clause*s.</span></span>

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

<span data-ttu-id="56f2d-227">Každý *type_parameter_constraints_clause* se skládá z tokenu `where`, za nímž následuje název parametru typu, za nímž následuje dvojtečka a seznam omezení za parametr daného typu.</span><span class="sxs-lookup"><span data-stu-id="56f2d-227">Each *type_parameter_constraints_clause* consists of the token `where`, followed by the name of a type parameter, followed by a colon and the list of constraints for that type parameter.</span></span> <span data-ttu-id="56f2d-228">Může existovat maximálně jeden `where` klauzuli pro každý parametr typu a `where` klauzule výpis je možný v libovolném pořadí.</span><span class="sxs-lookup"><span data-stu-id="56f2d-228">There can be at most one `where` clause for each type parameter, and the `where` clauses can be listed in any order.</span></span> <span data-ttu-id="56f2d-229">Podobně jako `get` a `set` tokeny v přistupující objekt vlastnosti `where` token není klíčové slovo.</span><span class="sxs-lookup"><span data-stu-id="56f2d-229">Like the `get` and `set` tokens in a property accessor, the `where` token is not a keyword.</span></span>

<span data-ttu-id="56f2d-230">Omezení uvedená v seznamu `where` klauzule můžete zahrnout jakýkoli z následujících komponent, které v tomto pořadí: jen jedno omezení primárního, jeden nebo více sekundárních omezení a omezení konstruktoru `new()`.</span><span class="sxs-lookup"><span data-stu-id="56f2d-230">The list of constraints given in a `where` clause can include any of the following components, in this order: a single primary constraint, one or more secondary constraints, and the constructor constraint, `new()`.</span></span>

<span data-ttu-id="56f2d-231">Omezení primárního může být typem třídy nebo ***referenční omezení typu*** `class` nebo ***hodnotu omezení typu*** `struct`.</span><span class="sxs-lookup"><span data-stu-id="56f2d-231">A primary constraint can be a class type or the ***reference type constraint*** `class` or the ***value type constraint*** `struct`.</span></span> <span data-ttu-id="56f2d-232">Sekundární omezení může být *type_parameter* nebo *interface_type*.</span><span class="sxs-lookup"><span data-stu-id="56f2d-232">A secondary constraint can be a *type_parameter* or *interface_type*.</span></span>

<span data-ttu-id="56f2d-233">Omezení typu odkazu Určuje, že typ argumentu použitý pro parametr typu musí být typ odkazu.</span><span class="sxs-lookup"><span data-stu-id="56f2d-233">The reference type constraint specifies that a type argument used for the type parameter must be a reference type.</span></span> <span data-ttu-id="56f2d-234">Všechny typy tříd, typy rozhraní, delegáta typy, typy polí a parametry typu, které jsou známé jako typ odkazu (jak je definována níže) splňovat toto omezení.</span><span class="sxs-lookup"><span data-stu-id="56f2d-234">All class types, interface types, delegate types, array types, and type parameters known to be a reference type (as defined below) satisfy this constraint.</span></span>

<span data-ttu-id="56f2d-235">Omezení typu hodnota určuje, že typ argumentu použitý pro parametr typu musí být typu hodnotu Null.</span><span class="sxs-lookup"><span data-stu-id="56f2d-235">The value type constraint specifies that a type argument used for the type parameter must be a non-nullable value type.</span></span> <span data-ttu-id="56f2d-236">Všechny typy neumožňující hodnotu struktury, výčtové typy a parametry typu s omezení typu hodnota splňovat toto omezení.</span><span class="sxs-lookup"><span data-stu-id="56f2d-236">All non-nullable struct types, enum types, and type parameters having the value type constraint satisfy this constraint.</span></span> <span data-ttu-id="56f2d-237">Všimněte si, že i když jsou klasifikovány jako typ hodnoty, typ připouštějící hodnotu Null ([typy připouštějící hodnotu Null](types.md#nullable-types)) nevyhovuje omezení typu hodnoty.</span><span class="sxs-lookup"><span data-stu-id="56f2d-237">Note that although classified as a value type, a nullable type ([Nullable types](types.md#nullable-types)) does not satisfy the value type constraint.</span></span> <span data-ttu-id="56f2d-238">Parametr typu s omezení typu hodnota nemůže mít také *constructor_constraint*.</span><span class="sxs-lookup"><span data-stu-id="56f2d-238">A type parameter having the value type constraint cannot also have the *constructor_constraint*.</span></span>

<span data-ttu-id="56f2d-239">Typy ukazatelů nikdy mohou být argumenty typu a nejsou považovány za splňovat buď odkaz na typ nebo hodnota typu omezení.</span><span class="sxs-lookup"><span data-stu-id="56f2d-239">Pointer types are never allowed to be type arguments and are not considered to satisfy either the reference type or value type constraints.</span></span>

<span data-ttu-id="56f2d-240">Pokud omezení typu třídy, typ rozhraní nebo parametr typu, typu Určuje minimální "základní typ", který musí podporovat každý typ argumentu použitý pro parametr typu.</span><span class="sxs-lookup"><span data-stu-id="56f2d-240">If a constraint is a class type, an interface type, or a type parameter, that type specifies a minimal "base type" that every type argument used for that type parameter must support.</span></span> <span data-ttu-id="56f2d-241">Pokaždé, když se používá vytvořeného typu nebo obecné metody, typ argumentu je porovnávána s omezeními u parametru typu v době kompilace.</span><span class="sxs-lookup"><span data-stu-id="56f2d-241">Whenever a constructed type or generic method is used, the type argument is checked against the constraints on the type parameter at compile-time.</span></span> <span data-ttu-id="56f2d-242">Zadaný argument typu musí splňovat požadavky popsané v [neodpovídajících omezení](types.md#satisfying-constraints).</span><span class="sxs-lookup"><span data-stu-id="56f2d-242">The type argument supplied must satisfy the conditions described in [Satisfying constraints](types.md#satisfying-constraints).</span></span>

<span data-ttu-id="56f2d-243">A *class_type* omezení musí splňovat následující pravidla:</span><span class="sxs-lookup"><span data-stu-id="56f2d-243">A *class_type* constraint must satisfy the following rules:</span></span>

*  <span data-ttu-id="56f2d-244">Typ musí být typu třídy.</span><span class="sxs-lookup"><span data-stu-id="56f2d-244">The type must be a class type.</span></span>
*  <span data-ttu-id="56f2d-245">Typ nesmí být `sealed`.</span><span class="sxs-lookup"><span data-stu-id="56f2d-245">The type must not be `sealed`.</span></span>
*  <span data-ttu-id="56f2d-246">Typ nesmí být jedním z následujících typů: `System.Array`, `System.Delegate`, `System.Enum`, nebo `System.ValueType`.</span><span class="sxs-lookup"><span data-stu-id="56f2d-246">The type must not be one of the following types: `System.Array`, `System.Delegate`, `System.Enum`, or `System.ValueType`.</span></span>
*  <span data-ttu-id="56f2d-247">Typ nesmí být `object`.</span><span class="sxs-lookup"><span data-stu-id="56f2d-247">The type must not be `object`.</span></span> <span data-ttu-id="56f2d-248">Protože všechny typy jsou odvozeny z `object`, taková omezení by neměl žádný vliv, pokud byly převody povoleny.</span><span class="sxs-lookup"><span data-stu-id="56f2d-248">Because all types derive from `object`, such a constraint would have no effect if it were permitted.</span></span>
*  <span data-ttu-id="56f2d-249">Typ třídy může být maximálně jedno omezení pro parametr daného typu.</span><span class="sxs-lookup"><span data-stu-id="56f2d-249">At most one constraint for a given type parameter can be a class type.</span></span>

<span data-ttu-id="56f2d-250">Typ zadaný jako *interface_type* omezení musí splňovat následující pravidla:</span><span class="sxs-lookup"><span data-stu-id="56f2d-250">A type specified as an *interface_type* constraint must satisfy the following rules:</span></span>

*  <span data-ttu-id="56f2d-251">Typ musí být typem rozhraní.</span><span class="sxs-lookup"><span data-stu-id="56f2d-251">The type must be an interface type.</span></span>
*  <span data-ttu-id="56f2d-252">Typ nesmí být zadáno více než jednou v danou `where` klauzuli.</span><span class="sxs-lookup"><span data-stu-id="56f2d-252">A type must not be specified more than once in a given `where` clause.</span></span>

<span data-ttu-id="56f2d-253">V obou případech omezení může zahrnovat parametry typu přidruženého typu nebo deklarace metody jako součást konstruovaný typ a může zahrnovat typ byl deklarován.</span><span class="sxs-lookup"><span data-stu-id="56f2d-253">In either case, the constraint can involve any of the type parameters of the associated type or method declaration as part of a constructed type, and can involve the type being declared.</span></span>

<span data-ttu-id="56f2d-254">Všechny třídy nebo rozhraní typ zadaný jako omezení parametru typu musí být přinejmenším stejně dostupná ([usnadnění omezení](basic-concepts.md#accessibility-constraints)) jako obecného typu nebo metody deklarované.</span><span class="sxs-lookup"><span data-stu-id="56f2d-254">Any class or interface type specified as a type parameter constraint must be at least as accessible ([Accessibility constraints](basic-concepts.md#accessibility-constraints)) as the generic type or method being declared.</span></span>

<span data-ttu-id="56f2d-255">Typ zadaný jako *type_parameter* omezení musí splňovat následující pravidla:</span><span class="sxs-lookup"><span data-stu-id="56f2d-255">A type specified as a *type_parameter* constraint must satisfy the following rules:</span></span>

*  <span data-ttu-id="56f2d-256">Typ musí být parametry typu.</span><span class="sxs-lookup"><span data-stu-id="56f2d-256">The type must be a type parameter.</span></span>
*  <span data-ttu-id="56f2d-257">Typ nesmí být zadáno více než jednou v danou `where` klauzuli.</span><span class="sxs-lookup"><span data-stu-id="56f2d-257">A type must not be specified more than once in a given `where` clause.</span></span>

<span data-ttu-id="56f2d-258">Kromě toho musí existovat žádné cyklické v grafu závislostí parametrů typu, kde závislost je přenositelný vztah určené:</span><span class="sxs-lookup"><span data-stu-id="56f2d-258">In addition there must be no cycles in the dependency graph of type parameters, where dependency is a transitive relation defined by:</span></span>

*  <span data-ttu-id="56f2d-259">Pokud parametr typu `T` je použitý jako omezení pro parametr typu `S` pak `S` ***závisí na*** `T`.</span><span class="sxs-lookup"><span data-stu-id="56f2d-259">If a type parameter `T` is used as a constraint for type parameter `S` then `S` ***depends on*** `T`.</span></span>
*  <span data-ttu-id="56f2d-260">Pokud parametr typu `S` závisí na parametr typu `T` a `T` závisí na parametr typu `U` pak `S` ***závisí na*** `U`.</span><span class="sxs-lookup"><span data-stu-id="56f2d-260">If a type parameter `S` depends on a type parameter `T` and `T` depends on a type parameter `U` then `S` ***depends on*** `U`.</span></span>

<span data-ttu-id="56f2d-261">Zadaný tento vztah, je chyba kompilace pro parametr typu závisí sám na sobě (přímo nebo nepřímo).</span><span class="sxs-lookup"><span data-stu-id="56f2d-261">Given this relation, it is a compile-time error for a type parameter to depend on itself (directly or indirectly).</span></span>

<span data-ttu-id="56f2d-262">Žádná omezení musí být konzistentní mezi parametry závislého typu.</span><span class="sxs-lookup"><span data-stu-id="56f2d-262">Any constraints must be consistent among dependent type parameters.</span></span> <span data-ttu-id="56f2d-263">Pokud zadáte parametr `S` závisí na typu parametru `T` pak:</span><span class="sxs-lookup"><span data-stu-id="56f2d-263">If type parameter `S` depends on type parameter `T` then:</span></span>

*  <span data-ttu-id="56f2d-264">`T` nesmí mít hodnotu omezení typu.</span><span class="sxs-lookup"><span data-stu-id="56f2d-264">`T` must not have the value type constraint.</span></span> <span data-ttu-id="56f2d-265">V opačném případě `T` zapečetěn efektivně tak `S` by se vynutilo být stejného typu jako `T`, takže odpadá potřeba dva parametry typu.</span><span class="sxs-lookup"><span data-stu-id="56f2d-265">Otherwise, `T` is effectively sealed so `S` would be forced to be the same type as `T`, eliminating the need for two type parameters.</span></span>
*  <span data-ttu-id="56f2d-266">Pokud `S` má omezení hodnoty typu pak `T` nesmí mít *class_type* omezení.</span><span class="sxs-lookup"><span data-stu-id="56f2d-266">If `S` has the value type constraint then `T` must not have a *class_type* constraint.</span></span>
*  <span data-ttu-id="56f2d-267">Pokud `S` má *class_type* omezení `A` a `T` má *class_type* omezení `B` musí být konverzi identity nebo implicitní referenční převod z `A` k `B` nebo implicitní referenční převod z `B` k `A`.</span><span class="sxs-lookup"><span data-stu-id="56f2d-267">If `S` has a *class_type* constraint `A` and `T` has a *class_type* constraint `B` then there must be an identity conversion or implicit reference conversion from `A` to `B` or an implicit reference conversion from `B` to `A`.</span></span>
*  <span data-ttu-id="56f2d-268">Pokud `S` závisí také na parametr typu `U` a `U` má *class_type* omezení `A` a `T` má *class_type* omezení `B` pak musíte být správcem nebo implicitní referenční převod z identity převod `A` k `B` nebo implicitní referenční převod z `B` k `A`.</span><span class="sxs-lookup"><span data-stu-id="56f2d-268">If `S` also depends on type parameter `U` and `U` has a *class_type* constraint `A` and `T` has a *class_type* constraint `B` then there must be an identity conversion or implicit reference conversion from `A` to `B` or an implicit reference conversion from `B` to `A`.</span></span>

<span data-ttu-id="56f2d-269">Je platný pro `S` mít omezení typu hodnoty a `T` mít omezení typu odkazu.</span><span class="sxs-lookup"><span data-stu-id="56f2d-269">It is valid for `S` to have the value type constraint and `T` to have the reference type constraint.</span></span> <span data-ttu-id="56f2d-270">Efektivně to omezuje `T` typům `System.Object`, `System.ValueType`, `System.Enum`a libovolný typ rozhraní.</span><span class="sxs-lookup"><span data-stu-id="56f2d-270">Effectively this limits `T` to the types `System.Object`, `System.ValueType`, `System.Enum`, and any interface type.</span></span>

<span data-ttu-id="56f2d-271">Pokud `where` klauzule pro parametr typu obsahuje omezení konstruktoru (která má tvar `new()`), je možné použít `new` operátoru pro vytvoření instancí typu ([vytváření výrazyobjektu](expressions.md#object-creation-expressions)).</span><span class="sxs-lookup"><span data-stu-id="56f2d-271">If the `where` clause for a type parameter includes a constructor constraint (which has the form `new()`), it is possible to use the `new` operator to create instances of the type ([Object creation expressions](expressions.md#object-creation-expressions)).</span></span> <span data-ttu-id="56f2d-272">Jakýkoli typ argument použitý pro parametr typu s omezením konstruktor musí mít veřejný konstruktor bez parametrů (Tento konstruktor implicitně existuje pro libovolný typ hodnoty), nebo jako parametr typu s hodnotou typu omezení nebo omezení konstruktoru (viz [Omezení parametru typu](classes.md#type-parameter-constraints) podrobnosti).</span><span class="sxs-lookup"><span data-stu-id="56f2d-272">Any type argument used for a type parameter with a constructor constraint must have a public parameterless constructor (this constructor implicitly exists for any value type) or be a type parameter having the value type constraint or constructor constraint (see [Type parameter constraints](classes.md#type-parameter-constraints) for details).</span></span>

<span data-ttu-id="56f2d-273">Následují příklady omezení:</span><span class="sxs-lookup"><span data-stu-id="56f2d-273">The following are examples of constraints:</span></span>
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

<span data-ttu-id="56f2d-274">V následujícím příkladu je v chybě, protože to způsobí, že Cykličnost v grafu závislostí parametrů typu:</span><span class="sxs-lookup"><span data-stu-id="56f2d-274">The following example is in error because it causes a circularity in the dependency graph of the type parameters:</span></span>
```csharp
class Circular<S,T>
    where S: T
    where T: S                // Error, circularity in dependency graph
{
    ...
}
```

<span data-ttu-id="56f2d-275">Následující příklady znázorňují neplatný několika dalších situacích:</span><span class="sxs-lookup"><span data-stu-id="56f2d-275">The following examples illustrate additional invalid situations:</span></span>
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

<span data-ttu-id="56f2d-276">***Efektivní základní třídy*** parametru typu `T` je definovaná následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="56f2d-276">The ***effective base class*** of a type parameter `T` is defined as follows:</span></span>

*  <span data-ttu-id="56f2d-277">Pokud `T` nemá žádné omezení primárního nebo omezení parametru typu, její základní třída je `object`.</span><span class="sxs-lookup"><span data-stu-id="56f2d-277">If `T` has no primary constraints or type parameter constraints, its effective base class is `object`.</span></span>
*  <span data-ttu-id="56f2d-278">Pokud `T` má omezení typu hodnoty, je její základní třída `System.ValueType`.</span><span class="sxs-lookup"><span data-stu-id="56f2d-278">If `T` has the value type constraint, its effective base class is `System.ValueType`.</span></span>
*  <span data-ttu-id="56f2d-279">Pokud `T` má *class_type* omezení `C` ale žádné *type_parameter* omezení, její základní třída je `C`.</span><span class="sxs-lookup"><span data-stu-id="56f2d-279">If `T` has a *class_type* constraint `C` but no *type_parameter* constraints, its effective base class is `C`.</span></span>
*  <span data-ttu-id="56f2d-280">Pokud `T` nemá žádné *class_type* omezení, ale má jeden nebo více *type_parameter* omezení, její základní třída je nejvíce encompassed typ ([zrušit převod operátory](conversions.md#lifted-conversion-operators)) v sadě efektivní základní třídy jeho *type_parameter* omezení.</span><span class="sxs-lookup"><span data-stu-id="56f2d-280">If `T` has no *class_type* constraint but has one or more *type_parameter* constraints, its effective base class is the most encompassed type ([Lifted conversion operators](conversions.md#lifted-conversion-operators)) in the set of effective base classes of its *type_parameter* constraints.</span></span> <span data-ttu-id="56f2d-281">Pravidla konzistence Ujistěte se, že většina encompassed typ existuje.</span><span class="sxs-lookup"><span data-stu-id="56f2d-281">The consistency rules ensure that such a most encompassed type exists.</span></span>
*  <span data-ttu-id="56f2d-282">Pokud `T` jsou obě *class_type* omezení a jeden nebo více *type_parameter* omezení, její základní třída je nejvíce encompassed typ ([zrušit převod operátory](conversions.md#lifted-conversion-operators)) v sadě, který se skládá z *class_type* omezení `T` a efektivní základní třídy jeho *type_parameter* omezení.</span><span class="sxs-lookup"><span data-stu-id="56f2d-282">If `T` has both a *class_type* constraint and one or more *type_parameter* constraints, its effective base class is the most encompassed type ([Lifted conversion operators](conversions.md#lifted-conversion-operators)) in the set consisting of the *class_type* constraint of `T` and the effective base classes of its *type_parameter* constraints.</span></span> <span data-ttu-id="56f2d-283">Pravidla konzistence Ujistěte se, že většina encompassed typ existuje.</span><span class="sxs-lookup"><span data-stu-id="56f2d-283">The consistency rules ensure that such a most encompassed type exists.</span></span>
*  <span data-ttu-id="56f2d-284">Pokud `T` má omezení typu odkaz, ale ne *class_type* omezení, její základní třída je `object`.</span><span class="sxs-lookup"><span data-stu-id="56f2d-284">If `T` has the reference type constraint but no *class_type* constraints, its effective base class is `object`.</span></span>

<span data-ttu-id="56f2d-285">Pro účely těchto pravidel, pokud T má omezení `V` , který je *value_type*, použijte místo toho nejspecifičtější základního typu `V` , který je *class_type*.</span><span class="sxs-lookup"><span data-stu-id="56f2d-285">For the purpose of these rules, if T has a constraint `V` that is a *value_type*, use instead the most specific base type of `V` that is a *class_type*.</span></span> <span data-ttu-id="56f2d-286">To může nikdy nemělo nastat v explicitně dané omezení, ale může dojít, pokud omezení obecné metody jsou implicitně dědí přepsání deklarace metody nebo explicitní implementaci metody rozhraní.</span><span class="sxs-lookup"><span data-stu-id="56f2d-286">This can never happen in an explicitly given constraint, but may occur when the constraints of a generic method are implicitly inherited by an overriding method declaration or an explicit implementation of an interface method.</span></span>

<span data-ttu-id="56f2d-287">Tato pravidla ujistěte se, že efektivní základní třídy je vždy *class_type*.</span><span class="sxs-lookup"><span data-stu-id="56f2d-287">These rules ensure that the effective base class is always a *class_type*.</span></span>

<span data-ttu-id="56f2d-288">***Rozhraní efektivní sada*** parametru typu `T` je definovaná následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="56f2d-288">The ***effective interface set*** of a type parameter `T` is defined as follows:</span></span>

*  <span data-ttu-id="56f2d-289">Pokud `T` nemá žádné *secondary_constraints*, jeho rozhraní efektivní sada byla prázdná.</span><span class="sxs-lookup"><span data-stu-id="56f2d-289">If `T` has no *secondary_constraints*, its effective interface set is empty.</span></span>
*  <span data-ttu-id="56f2d-290">Pokud `T` má *interface_type* omezení, ale ne *type_parameter* omezení, jeho sady efektivní rozhraní je sada *interface_type* omezení.</span><span class="sxs-lookup"><span data-stu-id="56f2d-290">If `T` has *interface_type* constraints but no *type_parameter* constraints, its effective interface set is its set of *interface_type* constraints.</span></span>
*  <span data-ttu-id="56f2d-291">Pokud `T` nemá žádné *interface_type* ale má omezení *type_parameter* omezení, je jeho efektivní rozhraní sady sjednocení sady efektivní rozhraní jeho *type_ Parametr* omezení.</span><span class="sxs-lookup"><span data-stu-id="56f2d-291">If `T` has no *interface_type* constraints but has *type_parameter* constraints, its effective interface set is the union of the effective interface sets of its *type_parameter* constraints.</span></span>
*  <span data-ttu-id="56f2d-292">Pokud `T` jsou obě *interface_type* omezení a *type_parameter* omezení, je jeho efektivní rozhraní sady sjednocení svou sadu *interface_type* omezení a efektivní rozhraní sady jeho *type_parameter* omezení.</span><span class="sxs-lookup"><span data-stu-id="56f2d-292">If `T` has both *interface_type* constraints and *type_parameter* constraints, its effective interface set is the union of its set of *interface_type* constraints and the effective interface sets of its *type_parameter* constraints.</span></span>

<span data-ttu-id="56f2d-293">Parametr typu je ***známé jako typ odkazu*** Pokud má omezení typu odkazu nebo její základní třída není `object` nebo `System.ValueType`.</span><span class="sxs-lookup"><span data-stu-id="56f2d-293">A type parameter is ***known to be a reference type*** if it has the reference type constraint or its effective base class is not `object` or `System.ValueType`.</span></span>

<span data-ttu-id="56f2d-294">Hodnoty typu parametru omezeného typu lze použít pro přístup ke členům instance odvozené od omezení.</span><span class="sxs-lookup"><span data-stu-id="56f2d-294">Values of a constrained type parameter type can be used to access the instance members implied by the constraints.</span></span> <span data-ttu-id="56f2d-295">V příkladu</span><span class="sxs-lookup"><span data-stu-id="56f2d-295">In the example</span></span>
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
<span data-ttu-id="56f2d-296">metody `IPrintable` lze vyvolat přímo na `x` protože `T` je omezen na vždy implementovat `IPrintable`.</span><span class="sxs-lookup"><span data-stu-id="56f2d-296">the methods of `IPrintable` can be invoked directly on `x` because `T` is constrained to always implement `IPrintable`.</span></span>

### <a name="class-body"></a><span data-ttu-id="56f2d-297">Tělo třídy</span><span class="sxs-lookup"><span data-stu-id="56f2d-297">Class body</span></span>

<span data-ttu-id="56f2d-298">*Class_body* třídy definuje členy této třídy.</span><span class="sxs-lookup"><span data-stu-id="56f2d-298">The *class_body* of a class defines the members of that class.</span></span>

```antlr
class_body
    : '{' class_member_declaration* '}'
    ;
```

## <a name="partial-types"></a><span data-ttu-id="56f2d-299">Částečné typy</span><span class="sxs-lookup"><span data-stu-id="56f2d-299">Partial types</span></span>

<span data-ttu-id="56f2d-300">Deklarace typu je možné rozdělit mezi několik ***částečného typu deklarace***.</span><span class="sxs-lookup"><span data-stu-id="56f2d-300">A type declaration can be split across multiple ***partial type declarations***.</span></span> <span data-ttu-id="56f2d-301">Deklarace typu je vytvořen z jejích částí pomocí následujících pravidel v této části, přičemž je považován za jednu deklaraci během zbývající zpracování kompilace a za běhu programu.</span><span class="sxs-lookup"><span data-stu-id="56f2d-301">The type declaration is constructed from its parts by following the rules in this section, whereupon it is treated as a single declaration during the remainder of the compile-time and run-time processing of the program.</span></span>

<span data-ttu-id="56f2d-302">A *class_declaration*, *struct_declaration* nebo *interface_declaration* představuje částečný typ deklarace, obsahuje-li `partial` modifikátor.</span><span class="sxs-lookup"><span data-stu-id="56f2d-302">A *class_declaration*, *struct_declaration* or *interface_declaration* represents a partial type declaration if it includes a `partial` modifier.</span></span> <span data-ttu-id="56f2d-303">`partial` není klíčové slovo a pouze pokud se zobrazí jedna z klíčových slov bezprostředně před funguje jako modifikátor `class`, `struct` nebo `interface` v deklaraci typu nebo před typem `void` v deklaraci metody.</span><span class="sxs-lookup"><span data-stu-id="56f2d-303">`partial` is not a keyword, and only acts as a modifier if it appears immediately before one of the keywords `class`, `struct` or `interface` in a type declaration, or before the type `void` in a method declaration.</span></span> <span data-ttu-id="56f2d-304">V jiných kontextech můžete použít jako normální identifikátor.</span><span class="sxs-lookup"><span data-stu-id="56f2d-304">In other contexts it can be used as a normal identifier.</span></span>

<span data-ttu-id="56f2d-305">Každá část deklarace částečný typ jsou povinné `partial` modifikátor.</span><span class="sxs-lookup"><span data-stu-id="56f2d-305">Each part of a partial type declaration must include a `partial` modifier.</span></span> <span data-ttu-id="56f2d-306">Musí mít stejný název a být deklarované v stejný obor názvů nebo typ deklarace jako jiné části.</span><span class="sxs-lookup"><span data-stu-id="56f2d-306">It must have the same name  and be declared in the same namespace or type declaration as the other parts.</span></span> <span data-ttu-id="56f2d-307">`partial` Modifikátor vyplývá, že další části deklarace typu může existovat někde jinde, ale existenci těchto dalších částí není povinné, je platný pro typ s deklarací jedné zahrnout `partial` modifikátor.</span><span class="sxs-lookup"><span data-stu-id="56f2d-307">The `partial` modifier indicates that additional parts of the type declaration may exist elsewhere, but the existence of such additional parts is not a requirement; it is valid for a type with a single declaration to include the `partial` modifier.</span></span>

<span data-ttu-id="56f2d-308">Všechny části částečného typu musí být kompilovány společně tak, aby části lze sloučit v době kompilace do jedinou deklaraci typu.</span><span class="sxs-lookup"><span data-stu-id="56f2d-308">All parts of a partial type must be compiled together such that the parts can be merged at compile-time into a single type declaration.</span></span> <span data-ttu-id="56f2d-309">Částečné typy výslovně neumožňují již kompilované typy prodloužit.</span><span class="sxs-lookup"><span data-stu-id="56f2d-309">Partial types specifically do not allow already compiled types to be extended.</span></span>

<span data-ttu-id="56f2d-310">Vnořené typy lze deklarovat v více částí s použitím `partial` modifikátor.</span><span class="sxs-lookup"><span data-stu-id="56f2d-310">Nested types may be declared in multiple parts by using the `partial` modifier.</span></span> <span data-ttu-id="56f2d-311">Obvykle obsahující typ je deklarován pomocí `partial` , jak dobře a každá část vnořený typ je deklarovaný v jiné části nadřazeného typu.</span><span class="sxs-lookup"><span data-stu-id="56f2d-311">Typically, the containing type is declared using `partial` as well, and each part of the nested type is declared in a different part of the containing type.</span></span>

<span data-ttu-id="56f2d-312">`partial` Modifikátoru není povolena u deklarace delegáta nebo výčtu.</span><span class="sxs-lookup"><span data-stu-id="56f2d-312">The `partial` modifier is not permitted on delegate or enum declarations.</span></span>

### <a name="attributes"></a><span data-ttu-id="56f2d-313">Atributy</span><span class="sxs-lookup"><span data-stu-id="56f2d-313">Attributes</span></span>

<span data-ttu-id="56f2d-314">Atributy částečný typ jsou určeny v nespecifikovaném pořadí kombinaci atributů jednotlivých součástí.</span><span class="sxs-lookup"><span data-stu-id="56f2d-314">The attributes of a partial type are determined by combining, in an unspecified order, the attributes of each of the parts.</span></span> <span data-ttu-id="56f2d-315">Pokud atribut je umístěn na více částí, je ekvivalentní se zadáním atribut více než jednou na typu.</span><span class="sxs-lookup"><span data-stu-id="56f2d-315">If an attribute is placed on multiple parts, it is equivalent to specifying the attribute multiple times on the type.</span></span> <span data-ttu-id="56f2d-316">Například dvě části:</span><span class="sxs-lookup"><span data-stu-id="56f2d-316">For example, the two parts:</span></span>

```csharp
[Attr1, Attr2("hello")]
partial class A {}

[Attr3, Attr2("goodbye")]
partial class A {}
```
<span data-ttu-id="56f2d-317">jsou ekvivalentní deklarace, jako:</span><span class="sxs-lookup"><span data-stu-id="56f2d-317">are equivalent to a declaration such as:</span></span>
```csharp
[Attr1, Attr2("hello"), Attr3, Attr2("goodbye")]
class A {}
```

<span data-ttu-id="56f2d-318">Atributy u parametrů typu kombinovat podobným způsobem.</span><span class="sxs-lookup"><span data-stu-id="56f2d-318">Attributes on type parameters combine in a similar fashion.</span></span>

### <a name="modifiers"></a><span data-ttu-id="56f2d-319">Modifikátory</span><span class="sxs-lookup"><span data-stu-id="56f2d-319">Modifiers</span></span>

<span data-ttu-id="56f2d-320">Při deklaraci částečného typu zahrnuje specifikaci přístupnosti ( `public`, `protected`, `internal`, a `private` modifikátory) musíte souhlasit s všechny ostatní součásti, které zahrnují specifikaci usnadnění přístupu.</span><span class="sxs-lookup"><span data-stu-id="56f2d-320">When a partial type declaration includes an accessibility specification (the `public`, `protected`, `internal`, and `private` modifiers) it must agree with all other parts that include an accessibility specification.</span></span> <span data-ttu-id="56f2d-321">Pokud žádná část částečného typu obsahuje specifikaci usnadnění přístupu, typ je předána odpovídající výchozí dostupnost ([deklarovaná přístupnost](basic-concepts.md#declared-accessibility)).</span><span class="sxs-lookup"><span data-stu-id="56f2d-321">If no part of a partial type includes an accessibility specification, the type is given the appropriate default accessibility ([Declared accessibility](basic-concepts.md#declared-accessibility)).</span></span>

<span data-ttu-id="56f2d-322">Pokud jeden nebo více částečné deklarace vnořeného typu `new` modifikátor, bez upozornění bude nahlášena, pokud vnořeného typu skrývá zděděný člen ([skrytí prostřednictvím dědičnosti](basic-concepts.md#hiding-through-inheritance)).</span><span class="sxs-lookup"><span data-stu-id="56f2d-322">If one or more partial declarations of a nested type include a `new` modifier, no warning is reported if the nested type hides an inherited member ([Hiding through inheritance](basic-concepts.md#hiding-through-inheritance)).</span></span>

<span data-ttu-id="56f2d-323">Pokud jeden nebo více částečné deklarace třídy zahrnují `abstract` modifikátor, třída je považována za abstraktní ([abstraktní třídy](classes.md#abstract-classes)).</span><span class="sxs-lookup"><span data-stu-id="56f2d-323">If one or more partial declarations of a class include an `abstract` modifier, the class is considered abstract ([Abstract classes](classes.md#abstract-classes)).</span></span> <span data-ttu-id="56f2d-324">Třídy v opačném případě se považuje za neabstraktní.</span><span class="sxs-lookup"><span data-stu-id="56f2d-324">Otherwise, the class is considered non-abstract.</span></span>

<span data-ttu-id="56f2d-325">Pokud jeden nebo více částečné deklarace třídy `sealed` modifikátor, třída je považován za zapečetěné ([zapečetěné třídy](classes.md#sealed-classes)).</span><span class="sxs-lookup"><span data-stu-id="56f2d-325">If one or more partial declarations of a class include a `sealed` modifier, the class is considered sealed ([Sealed classes](classes.md#sealed-classes)).</span></span> <span data-ttu-id="56f2d-326">V opačném případě se považuje za třídu nezapečetěné.</span><span class="sxs-lookup"><span data-stu-id="56f2d-326">Otherwise, the class is considered unsealed.</span></span>

<span data-ttu-id="56f2d-327">Všimněte si, že třída nemůže být extern i sealed.</span><span class="sxs-lookup"><span data-stu-id="56f2d-327">Note that a class cannot be both abstract and sealed.</span></span>

<span data-ttu-id="56f2d-328">Když `unsafe` modifikátor se používá v deklaraci částečné typu jen pro konkrétní části je považován za kontextu unsafe ([nezabezpečený kontext](unsafe-code.md#unsafe-contexts)).</span><span class="sxs-lookup"><span data-stu-id="56f2d-328">When the `unsafe` modifier is used on a partial type declaration, only that particular part is considered an unsafe context ([Unsafe contexts](unsafe-code.md#unsafe-contexts)).</span></span>

### <a name="type-parameters-and-constraints"></a><span data-ttu-id="56f2d-329">Parametry typu a omezení</span><span class="sxs-lookup"><span data-stu-id="56f2d-329">Type parameters and constraints</span></span>

<span data-ttu-id="56f2d-330">Pokud obecný typ je deklarován ve více částí, musí každá část stavu parametry typu.</span><span class="sxs-lookup"><span data-stu-id="56f2d-330">If a generic type is declared in multiple parts, each part must state the type parameters.</span></span> <span data-ttu-id="56f2d-331">Každá část musí mít stejný počet parametrů typu a stejný název pro každý parametr typu v pořadí.</span><span class="sxs-lookup"><span data-stu-id="56f2d-331">Each part must have the same number of type parameters, and the same name for each type parameter, in order.</span></span>

<span data-ttu-id="56f2d-332">Pokud obsahuje deklaraci částečný obecný typ omezení (`where` klauzule), omezení musíte souhlasit s jinými částmi, které zahrnují omezení.</span><span class="sxs-lookup"><span data-stu-id="56f2d-332">When a partial generic type declaration includes constraints (`where` clauses), the constraints must agree with all other parts that include constraints.</span></span> <span data-ttu-id="56f2d-333">Konkrétně jednotlivé části, která zahrnuje omezení musí mít omezení pro stejnou sadu parametrů typu, a pro každý parametr typu sady primární, sekundární a omezení konstruktoru musí být ekvivalentní.</span><span class="sxs-lookup"><span data-stu-id="56f2d-333">Specifically, each part that includes constraints must have constraints for the same set of type parameters, and for each type parameter the sets of primary, secondary, and constructor constraints must be equivalent.</span></span> <span data-ttu-id="56f2d-334">Dvě sady omezení jsou rovnocenné, pokud obsahují stejné členy.</span><span class="sxs-lookup"><span data-stu-id="56f2d-334">Two sets of constraints are equivalent if they contain the same members.</span></span> <span data-ttu-id="56f2d-335">Pokud žádná část částečný obecný typ omezení parametru typu, typu, který parametry se považují za vstupy bez omezení.</span><span class="sxs-lookup"><span data-stu-id="56f2d-335">If no part of a partial generic type specifies type parameter constraints, the type parameters are considered unconstrained.</span></span>

<span data-ttu-id="56f2d-336">V příkladu</span><span class="sxs-lookup"><span data-stu-id="56f2d-336">The example</span></span>
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
<span data-ttu-id="56f2d-337">správnost vzhledem k tomu, že tyto součásti, které zahrnují omezení (první dva) efektivně zadejte stejnou sadu primární, sekundární a omezení konstruktoru pro stejnou sadu parametrů typu v uvedeném pořadí.</span><span class="sxs-lookup"><span data-stu-id="56f2d-337">is correct because those parts that include constraints (the first two) effectively specify the same set of primary, secondary, and constructor constraints for the same set of type parameters, respectively.</span></span>

### <a name="base-class"></a><span data-ttu-id="56f2d-338">Základní třída</span><span class="sxs-lookup"><span data-stu-id="56f2d-338">Base class</span></span>

<span data-ttu-id="56f2d-339">Při deklaraci částečné třídy obsahuje specifikace základní třídy musíte souhlasit s jinými částmi, které obsahují specifikace základní třídy.</span><span class="sxs-lookup"><span data-stu-id="56f2d-339">When a partial class declaration includes a base class specification it must agree with all other parts that include a base class specification.</span></span> <span data-ttu-id="56f2d-340">Pokud žádná část částečné třídy obsahuje specifikace základní třídy, stane se základní třídy `System.Object` ([základních tříd](classes.md#base-classes)).</span><span class="sxs-lookup"><span data-stu-id="56f2d-340">If no part of a partial class includes a base class specification, the base class becomes `System.Object` ([Base classes](classes.md#base-classes)).</span></span>

### <a name="base-interfaces"></a><span data-ttu-id="56f2d-341">Základní rozhraní</span><span class="sxs-lookup"><span data-stu-id="56f2d-341">Base interfaces</span></span>

<span data-ttu-id="56f2d-342">Sada základních rozhraní pro typ deklarovaný v několika částí je sjednocení základních rozhraní zadané u každé části.</span><span class="sxs-lookup"><span data-stu-id="56f2d-342">The set of base interfaces for a type declared in multiple parts is the union of the base interfaces specified on each part.</span></span> <span data-ttu-id="56f2d-343">Určité základní rozhraní může mít pouze jednou název na jednotlivé části, ale je povoleno více částí název stejné základních rozhraní.</span><span class="sxs-lookup"><span data-stu-id="56f2d-343">A particular base interface may only be named once on each part, but it is permitted for multiple parts to name the same base interface(s).</span></span> <span data-ttu-id="56f2d-344">Musí existovat pouze jedna implementace členů jakékoli dané základní rozhraní.</span><span class="sxs-lookup"><span data-stu-id="56f2d-344">There must only be one implementation of the members of any given base interface.</span></span>

<span data-ttu-id="56f2d-345">V příkladu</span><span class="sxs-lookup"><span data-stu-id="56f2d-345">In the example</span></span>
```csharp
partial class C: IA, IB {...}

partial class C: IC {...}

partial class C: IA, IB {...}
```
<span data-ttu-id="56f2d-346">Sada základních rozhraní pro třídu `C` je `IA`, `IB`, a `IC`.</span><span class="sxs-lookup"><span data-stu-id="56f2d-346">the set of base interfaces for class `C` is `IA`, `IB`, and `IC`.</span></span>

<span data-ttu-id="56f2d-347">Obvykle každá část poskytuje implementaci rozhraní deklarované na takovou stranou, Nicméně toto není povinné.</span><span class="sxs-lookup"><span data-stu-id="56f2d-347">Typically, each part provides an implementation of the interface(s) declared on that part; however, this is not a requirement.</span></span> <span data-ttu-id="56f2d-348">Část může poskytnout implementaci pro deklarované na jinou část rozhraní:</span><span class="sxs-lookup"><span data-stu-id="56f2d-348">A part may provide the implementation for an interface declared on a different part:</span></span>
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

### <a name="members"></a><span data-ttu-id="56f2d-349">Členové</span><span class="sxs-lookup"><span data-stu-id="56f2d-349">Members</span></span>

<span data-ttu-id="56f2d-350">S výjimkou částečné metody ([částečné metody](classes.md#partial-methods)), sadu členů z typu deklarovaného ve více částí je jednoduše sjednocení sadu členy s deklarací v každé části.</span><span class="sxs-lookup"><span data-stu-id="56f2d-350">With the exception of partial methods ([Partial methods](classes.md#partial-methods)), the set of members of a type declared in multiple parts is simply the union of the set of members declared in each part.</span></span> <span data-ttu-id="56f2d-351">Subjekty všech částí deklarace typu sdílejí stejný obor deklarace ([deklarace](basic-concepts.md#declarations)) a obor každého člena ([obory](basic-concepts.md#scopes)) rozšiřuje do těla všechny části.</span><span class="sxs-lookup"><span data-stu-id="56f2d-351">The bodies of all parts of the type declaration share the same declaration space ([Declarations](basic-concepts.md#declarations)), and the scope of each member ([Scopes](basic-concepts.md#scopes)) extends to the bodies of all the parts.</span></span> <span data-ttu-id="56f2d-352">Doména přístupnosti člena vždy obsahuje všechny části nadřazeného typu.; `private` člen deklarovaný v jedné části volně přístupná z jiné části.</span><span class="sxs-lookup"><span data-stu-id="56f2d-352">The accessibility domain of any member always includes all the parts of the enclosing type; a `private` member declared in one part is freely accessible from another part.</span></span> <span data-ttu-id="56f2d-353">Je chyba kompilace pro deklaraci na stejný člen ve více než jednu část typu, pokud je daný člen typu s `partial` modifikátor.</span><span class="sxs-lookup"><span data-stu-id="56f2d-353">It is a compile-time error to declare the same member in more than one part of the type, unless that member is a type with the `partial` modifier.</span></span>

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

<span data-ttu-id="56f2d-354">Řazení členů v rámci typu je zřídka důležité pro kód jazyka C#, ale může být důležité při propojování s jinými jazyky a prostředí.</span><span class="sxs-lookup"><span data-stu-id="56f2d-354">The ordering of members within a type is rarely significant to C# code, but may be significant when interfacing with other languages and environments.</span></span> <span data-ttu-id="56f2d-355">V těchto případech není definováno pořadí členů v rámci typu deklarovaného ve více částí.</span><span class="sxs-lookup"><span data-stu-id="56f2d-355">In these cases, the ordering of members within a type declared in multiple parts is undefined.</span></span>

### <a name="partial-methods"></a><span data-ttu-id="56f2d-356">Částečné metody</span><span class="sxs-lookup"><span data-stu-id="56f2d-356">Partial methods</span></span>

<span data-ttu-id="56f2d-357">Částečné metody můžete definovanou v jedné části deklarace typu a implementovat v jiném.</span><span class="sxs-lookup"><span data-stu-id="56f2d-357">Partial methods can be defined in one part of a type declaration and implemented in another.</span></span> <span data-ttu-id="56f2d-358">Implementace je volitelná. Pokud žádná část implementuje částečné metody, deklaraci částečné metody a všechna volání se odeberou z deklarace typu, který je výsledkem kombinace částí.</span><span class="sxs-lookup"><span data-stu-id="56f2d-358">The implementation is optional; if no part implements the partial method, the partial method declaration and all calls to it are removed from the type declaration resulting from the combination of the parts.</span></span>

<span data-ttu-id="56f2d-359">Částečné metody nelze definovat modifikátory přístupu, ale jsou implicitně `private`.</span><span class="sxs-lookup"><span data-stu-id="56f2d-359">Partial methods cannot define access modifiers, but are implicitly `private`.</span></span> <span data-ttu-id="56f2d-360">Jejich návratový typ musí být `void`, a jejich parametrů nemůže mít `out` modifikátor.</span><span class="sxs-lookup"><span data-stu-id="56f2d-360">Their return type must be `void`, and their parameters cannot have the `out` modifier.</span></span> <span data-ttu-id="56f2d-361">Identifikátor `partial` je rozpoznán jako speciální – klíčové slovo v deklaraci metody pouze v případě, že se zobrazí přímo před `void` typ; jinak mohou být použity jako normální identifikátor.</span><span class="sxs-lookup"><span data-stu-id="56f2d-361">The identifier `partial` is recognized as a special keyword in a method declaration only if it appears right before the `void` type; otherwise it can be used as a normal identifier.</span></span> <span data-ttu-id="56f2d-362">Částečná metoda nesmí explicitně implementovat metody rozhraní.</span><span class="sxs-lookup"><span data-stu-id="56f2d-362">A partial method cannot explicitly implement interface methods.</span></span>

<span data-ttu-id="56f2d-363">Existují dva typy deklarace částečné metody: Pokud text v deklaraci metody je středník, deklarace se říká, že ***definující deklarace částečné metody***.</span><span class="sxs-lookup"><span data-stu-id="56f2d-363">There are two kinds of partial method declarations: If the body of the method declaration is a semicolon, the declaration is said to be a ***defining partial method declaration***.</span></span> <span data-ttu-id="56f2d-364">Pokud text je zadána jako *bloku*, deklarace se říká, že ***implementující deklaraci částečné metody***.</span><span class="sxs-lookup"><span data-stu-id="56f2d-364">If the body is given as a *block*, the declaration is said to be an ***implementing partial method declaration***.</span></span> <span data-ttu-id="56f2d-365">V části deklarace typu může existovat pouze jedna definice deklarace částečné metody s daným podpisem. a může existovat pouze jeden implementující deklaraci částečné metody s daným podpisem.</span><span class="sxs-lookup"><span data-stu-id="56f2d-365">Across the parts of a type declaration there can be only one defining partial method declaration with a given signature, and there can be only one implementing partial method declaration with a given signature.</span></span> <span data-ttu-id="56f2d-366">Pokud je zadána implementující deklaraci částečné metody, odpovídající definující deklarace částečné metody musí existovat a deklarace musí odpovídat jako zadaný v následujících tématech:</span><span class="sxs-lookup"><span data-stu-id="56f2d-366">If an implementing partial method declaration is given, a corresponding defining partial method declaration must exist, and the declarations must match as specified in the following:</span></span>

* <span data-ttu-id="56f2d-367">Deklarace musí mít stejné modifikátory (i když nemusí ve stejném pořadí), název metody, počet parametrů typu a počtu parametrů.</span><span class="sxs-lookup"><span data-stu-id="56f2d-367">The declarations must have the same modifiers (although not necessarily in the same order), method name, number of type parameters and number of parameters.</span></span>
* <span data-ttu-id="56f2d-368">Odpovídajícím parametrům in v prohlášení musí mít stejné modifikátory (i když nemusí ve stejném pořadí) a stejnými typy (modulo rozdíly v názvy parametrů typů).</span><span class="sxs-lookup"><span data-stu-id="56f2d-368">Corresponding parameters in the declarations must have the same modifiers (although not necessarily in the same order) and the same types (modulo differences in type parameter names).</span></span>
* <span data-ttu-id="56f2d-369">Odpovídající parametry typu v prohlášení musí mít stejné omezení (modulo rozdíly v názvy parametrů typů).</span><span class="sxs-lookup"><span data-stu-id="56f2d-369">Corresponding type parameters in the declarations must have the same constraints (modulo differences in type parameter names).</span></span>

<span data-ttu-id="56f2d-370">Implementující deklaraci částečné metody se může zobrazit v části stejné jako odpovídající definující deklarace částečné metody.</span><span class="sxs-lookup"><span data-stu-id="56f2d-370">An implementing partial method declaration can appear in the same part as the corresponding defining partial method declaration.</span></span>

<span data-ttu-id="56f2d-371">Pouze definuje částečné metody se účastní řešení přetížení.</span><span class="sxs-lookup"><span data-stu-id="56f2d-371">Only a defining partial method participates in overload resolution.</span></span> <span data-ttu-id="56f2d-372">To znamená zda implementující deklarace není uveden, výrazy typu invocation může vyřešit volání částečné metody.</span><span class="sxs-lookup"><span data-stu-id="56f2d-372">Thus, whether or not an implementing declaration is given, invocation expressions may resolve to invocations of the partial method.</span></span> <span data-ttu-id="56f2d-373">Protože je částečná metoda vždy vrátí `void`, tyto výrazy typu invocation bude vždy příkazy výrazů.</span><span class="sxs-lookup"><span data-stu-id="56f2d-373">Because a partial method always returns `void`, such invocation expressions will always be expression statements.</span></span> <span data-ttu-id="56f2d-374">Navíc protože částečné metody je implicitně `private`, tyto příkazy se vždy objevují v rámci jedna z částí deklarace typu, ve kterém je deklarována jako částečná metoda.</span><span class="sxs-lookup"><span data-stu-id="56f2d-374">Furthermore, because a partial method is implicitly `private`, such statements will always occur within one of the parts of the type declaration within which the partial method is declared.</span></span>

<span data-ttu-id="56f2d-375">Pokud žádná z částí deklarace částečného typu obsahuje implementující deklarace pro dané částečné metody, libovolný příkaz výrazu vyvolání jednoduše odebrán kombinované typ deklarace.</span><span class="sxs-lookup"><span data-stu-id="56f2d-375">If no part of a partial type declaration contains an implementing declaration for a given partial method, any expression statement invoking it is simply removed from the combined type declaration.</span></span> <span data-ttu-id="56f2d-376">Proto volání výrazu, včetně všech základních výrazů nemá žádný vliv za běhu.</span><span class="sxs-lookup"><span data-stu-id="56f2d-376">Thus the invocation expression, including any constituent expressions, has no effect at run-time.</span></span> <span data-ttu-id="56f2d-377">Částečná metoda sama se také odebere a nesmí být členem skupiny kombinované typ deklarace.</span><span class="sxs-lookup"><span data-stu-id="56f2d-377">The partial method itself is also removed and will not be a member of the combined type declaration.</span></span>

<span data-ttu-id="56f2d-378">Pokud implementující deklarace pro dané částečné metody vyvolání částečné metody se zachovají.</span><span class="sxs-lookup"><span data-stu-id="56f2d-378">If an implementing declaration exist for a given partial method, the invocations of the partial methods are retained.</span></span> <span data-ttu-id="56f2d-379">Částečné metody vede k deklaraci metody podobné pro implementující deklaraci částečné metody s těmito výjimkami:</span><span class="sxs-lookup"><span data-stu-id="56f2d-379">The partial method gives rise to a method declaration similar to the implementing partial method declaration except for the following:</span></span>

* <span data-ttu-id="56f2d-380">`partial` Modifikátor není součástí</span><span class="sxs-lookup"><span data-stu-id="56f2d-380">The `partial` modifier is not included</span></span>
* <span data-ttu-id="56f2d-381">Atributy ve výsledné deklarace metody jsou kombinované atributy definování a implementující deklaraci částečné metody v nespecifikovaném pořadí.</span><span class="sxs-lookup"><span data-stu-id="56f2d-381">The attributes in the resulting method declaration are the combined attributes of the defining and the implementing partial method declaration in unspecified order.</span></span> <span data-ttu-id="56f2d-382">Duplicitní položky se neodeberou.</span><span class="sxs-lookup"><span data-stu-id="56f2d-382">Duplicates are not removed.</span></span>
* <span data-ttu-id="56f2d-383">Atributy pro parametry výsledný deklarace metody jsou kombinované atributů odpovídajících parametrů definování a implementující deklaraci částečné metody v nespecifikovaném pořadí.</span><span class="sxs-lookup"><span data-stu-id="56f2d-383">The attributes on the parameters of the resulting method declaration are the combined attributes of the corresponding parameters of the defining and the implementing partial method declaration in unspecified order.</span></span> <span data-ttu-id="56f2d-384">Duplicitní položky se neodeberou.</span><span class="sxs-lookup"><span data-stu-id="56f2d-384">Duplicates are not removed.</span></span>

<span data-ttu-id="56f2d-385">Pokud definující deklarací, ale ne implementující deklarace částečné metody M, platí následující omezení:</span><span class="sxs-lookup"><span data-stu-id="56f2d-385">If a defining declaration but not an implementing declaration is given for a partial method M, the following restrictions apply:</span></span>

* <span data-ttu-id="56f2d-386">Jde chybu v době kompilace k vytvoření delegáta metodě ([delegovat vytváření výrazů](expressions.md#delegate-creation-expressions)).</span><span class="sxs-lookup"><span data-stu-id="56f2d-386">It is a compile-time error to create a delegate to method ([Delegate creation expressions](expressions.md#delegate-creation-expressions)).</span></span>
* <span data-ttu-id="56f2d-387">Jde chybu v době kompilace k odkazování na `M` uvnitř anonymní funkce převedená na typu stromu výrazu ([vyhodnocení anonymní funkce převody na typy stromu výrazů](conversions.md#evaluation-of-anonymous-function-conversions-to-expression-tree-types)).</span><span class="sxs-lookup"><span data-stu-id="56f2d-387">It is a compile-time error to refer to `M` inside an anonymous function that is converted to an expression tree type ([Evaluation of anonymous function conversions to expression tree types](conversions.md#evaluation-of-anonymous-function-conversions-to-expression-tree-types)).</span></span>
* <span data-ttu-id="56f2d-388">Výrazy, ke kterým dochází v rámci vyvolání `M` nemají vliv na stav jednoznačného přiřazení ([jednoznačného přiřazení](variables.md#definite-assignment)), který může potenciálně vést k chybám kompilace.</span><span class="sxs-lookup"><span data-stu-id="56f2d-388">Expressions occurring as part of an invocation of `M` do not affect the definite assignment state ([Definite assignment](variables.md#definite-assignment)), which can potentially lead to compile-time errors.</span></span>
* <span data-ttu-id="56f2d-389">`M` nemůže být vstupním bodem pro aplikaci ([spuštění aplikace](basic-concepts.md#application-startup)).</span><span class="sxs-lookup"><span data-stu-id="56f2d-389">`M` cannot be the entry point for an application ([Application Startup](basic-concepts.md#application-startup)).</span></span>

<span data-ttu-id="56f2d-390">Částečné metody jsou užitečné pro povolení jednu část deklarace typu k přizpůsobení chování jiné části, například takový, který je vygenerován pomocí nástroje.</span><span class="sxs-lookup"><span data-stu-id="56f2d-390">Partial methods are useful for allowing one part of a type declaration to customize the behavior of another part, e.g., one that is generated by a tool.</span></span> <span data-ttu-id="56f2d-391">Vezměte v úvahu následující deklarace částečné třídy:</span><span class="sxs-lookup"><span data-stu-id="56f2d-391">Consider the following partial class declaration:</span></span>
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

<span data-ttu-id="56f2d-392">Pokud tato třída je kompilována bez dalších částí, definující deklarace částečné metody a jejich volání se odeberou a výsledné deklarace kombinované třídy budou ekvivalentní následujícímu zápisu:</span><span class="sxs-lookup"><span data-stu-id="56f2d-392">If this class is compiled without any other parts, the defining partial method declarations and their invocations will be removed, and the resulting combined class declaration will be equivalent to the following:</span></span>
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

<span data-ttu-id="56f2d-393">Předpokládejme, že jiné části je uveden, ale poskytující implementující deklaraci částečné metody:</span><span class="sxs-lookup"><span data-stu-id="56f2d-393">Assume that another part is given, however, which provides implementing declarations of the partial methods:</span></span>
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

<span data-ttu-id="56f2d-394">Pak bude výsledná deklarace kombinované třídy ekvivalentní následujícímu zápisu:</span><span class="sxs-lookup"><span data-stu-id="56f2d-394">Then the resulting combined class declaration will be equivalent to the following:</span></span>
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

### <a name="name-binding"></a><span data-ttu-id="56f2d-395">Název vazby</span><span class="sxs-lookup"><span data-stu-id="56f2d-395">Name binding</span></span>

<span data-ttu-id="56f2d-396">I když každá část rozšiřitelný typ musí být deklarovány v rámci stejného oboru názvů, části jsou obvykle napsány v rámci jiný obor názvů deklarace.</span><span class="sxs-lookup"><span data-stu-id="56f2d-396">Although each part of an extensible type must be declared within the same namespace, the parts are typically written within different namespace declarations.</span></span> <span data-ttu-id="56f2d-397">Díky tomu se různé `using` direktivy ([direktiv Using](namespaces.md#using-directives)) může být k dispozici pro každou část.</span><span class="sxs-lookup"><span data-stu-id="56f2d-397">Thus, different `using` directives ([Using directives](namespaces.md#using-directives)) may be present for each part.</span></span> <span data-ttu-id="56f2d-398">Při interpretaci jednoduché názvy ([odvození typu](expressions.md#type-inference)) v rámci jedné části pouze `using` direktivy deklarace oboru názvů uzavírající této části jsou považovány za.</span><span class="sxs-lookup"><span data-stu-id="56f2d-398">When interpreting simple names ([Type inference](expressions.md#type-inference)) within one part, only the `using` directives of the namespace declaration(s) enclosing that part are considered.</span></span> <span data-ttu-id="56f2d-399">Výsledkem může být stejný identifikátor mají různý význam v různých částech:</span><span class="sxs-lookup"><span data-stu-id="56f2d-399">This may result in the same identifier having different meanings in different parts:</span></span>
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

## <a name="class-members"></a><span data-ttu-id="56f2d-400">Členy třídy</span><span class="sxs-lookup"><span data-stu-id="56f2d-400">Class members</span></span>

<span data-ttu-id="56f2d-401">Členy třídy se skládá z členů zavedených v jeho *class_member_declaration*s a členy zděděné od přímé základní třídy.</span><span class="sxs-lookup"><span data-stu-id="56f2d-401">The members of a class consist of the members introduced by its *class_member_declaration*s and the members inherited from the direct base class.</span></span>

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

<span data-ttu-id="56f2d-402">Členy typu třídy jsou rozdělené do následujících kategorií:</span><span class="sxs-lookup"><span data-stu-id="56f2d-402">The members of a class type are divided into the following categories:</span></span>

*  <span data-ttu-id="56f2d-403">Konstanty, které představují konstanty, které jsou přidružené k třídě ([konstanty](classes.md#constants)).</span><span class="sxs-lookup"><span data-stu-id="56f2d-403">Constants, which represent constant values associated with the class ([Constants](classes.md#constants)).</span></span>
*  <span data-ttu-id="56f2d-404">Pole, které jsou proměnné třídy ([pole](classes.md#fields)).</span><span class="sxs-lookup"><span data-stu-id="56f2d-404">Fields, which are the variables of the class ([Fields](classes.md#fields)).</span></span>
*  <span data-ttu-id="56f2d-405">Metody, které implementují výpočtů a akcí, které lze provést pomocí třídy ([metody](classes.md#methods)).</span><span class="sxs-lookup"><span data-stu-id="56f2d-405">Methods, which implement the computations and actions that can be performed by the class ([Methods](classes.md#methods)).</span></span>
*  <span data-ttu-id="56f2d-406">Vlastnosti, které definují pojmenované vlastnosti a akce přidružené k čtení a zápis těchto vlastností ([vlastnosti](classes.md#properties)).</span><span class="sxs-lookup"><span data-stu-id="56f2d-406">Properties, which define named characteristics and the actions associated with reading and writing those characteristics ([Properties](classes.md#properties)).</span></span>
*  <span data-ttu-id="56f2d-407">Události, které definují oznámení, která mohou být generovány třídu ([události](classes.md#events)).</span><span class="sxs-lookup"><span data-stu-id="56f2d-407">Events, which define notifications that can be generated by the class ([Events](classes.md#events)).</span></span>
*  <span data-ttu-id="56f2d-408">Indexery, které povolují instance třídy, které mají být indexovány v stejným způsobem (syntakticky) jako pole ([indexery](classes.md#indexers)).</span><span class="sxs-lookup"><span data-stu-id="56f2d-408">Indexers, which permit instances of the class to be indexed in the same way (syntactically) as arrays ([Indexers](classes.md#indexers)).</span></span>
*  <span data-ttu-id="56f2d-409">Operátory, které definují operátory výrazů, které lze použít u instance třídy ([operátory](classes.md#operators)).</span><span class="sxs-lookup"><span data-stu-id="56f2d-409">Operators, which define the expression operators that can be applied to instances of the class ([Operators](classes.md#operators)).</span></span>
*  <span data-ttu-id="56f2d-410">Instance konstruktory, které implementují akce potřebné k inicializaci instance třídy ([Instance konstruktory](classes.md#instance-constructors))</span><span class="sxs-lookup"><span data-stu-id="56f2d-410">Instance constructors, which implement the actions required to initialize instances of the class ([Instance constructors](classes.md#instance-constructors))</span></span>
*  <span data-ttu-id="56f2d-411">Destruktory, které implementují akce, jež mají být provedeny před instancí třídy budou trvale odstraněny ([destruktory](classes.md#destructors)).</span><span class="sxs-lookup"><span data-stu-id="56f2d-411">Destructors, which implement the actions to be performed before instances of the class are permanently discarded ([Destructors](classes.md#destructors)).</span></span>
*  <span data-ttu-id="56f2d-412">Statické konstruktory, které implementují akce potřebné k inicializaci vlastní třídy ([statické konstruktory](classes.md#static-constructors)).</span><span class="sxs-lookup"><span data-stu-id="56f2d-412">Static constructors, which implement the actions required to initialize the class itself ([Static constructors](classes.md#static-constructors)).</span></span>
*  <span data-ttu-id="56f2d-413">Typy, které představují typy, které jsou místní pro třídu ([vnořené typy](classes.md#nested-types)).</span><span class="sxs-lookup"><span data-stu-id="56f2d-413">Types, which represent the types that are local to the class ([Nested types](classes.md#nested-types)).</span></span>

<span data-ttu-id="56f2d-414">Členy, které mohou obsahovat spustitelného kódu jsou souhrnně označovány jako *funkce členy* typu třídy.</span><span class="sxs-lookup"><span data-stu-id="56f2d-414">Members that can contain executable code are collectively known as the *function members* of the class type.</span></span> <span data-ttu-id="56f2d-415">Funkce členy typu třídy jsou metody, vlastnosti, události, indexery, operátory, konstruktory instancí, destruktory a statických konstruktorů tohoto typu třídy.</span><span class="sxs-lookup"><span data-stu-id="56f2d-415">The function members of a class type are the methods, properties, events, indexers, operators, instance constructors,  destructors, and static constructors of that class type.</span></span>

<span data-ttu-id="56f2d-416">A *class_declaration* vytvoří nové prohlášení prostor ([deklarace](basic-concepts.md#declarations)) a *class_member_declaration*s okamžitě obsažených *třídy _declaration* zavést nové členy do tohoto prostoru deklarace.</span><span class="sxs-lookup"><span data-stu-id="56f2d-416">A *class_declaration* creates a new declaration space ([Declarations](basic-concepts.md#declarations)), and the *class_member_declaration*s immediately contained by the *class_declaration* introduce new members into this declaration space.</span></span> <span data-ttu-id="56f2d-417">Následující pravidla platí pro *class_member_declaration*s:</span><span class="sxs-lookup"><span data-stu-id="56f2d-417">The following rules apply to *class_member_declaration*s:</span></span>

*  <span data-ttu-id="56f2d-418">Instance konstruktory, destruktory a statické konstruktory musí mít stejný název jako okamžitě nadřazené třídy.</span><span class="sxs-lookup"><span data-stu-id="56f2d-418">Instance constructors, destructors and static constructors must have the same name as the immediately enclosing class.</span></span> <span data-ttu-id="56f2d-419">Všechny ostatní členové musí mít názvy, které se liší od názvu okamžitě nadřazené třídy.</span><span class="sxs-lookup"><span data-stu-id="56f2d-419">All other members must have names that differ from the name of the immediately enclosing class.</span></span>
*  <span data-ttu-id="56f2d-420">Název – konstanta, pole, vlastnosti, události nebo typ se musí lišit od názvů ostatních členů deklarované ve stejné třídě.</span><span class="sxs-lookup"><span data-stu-id="56f2d-420">The name of a constant, field, property, event, or type must differ from the names of all other members declared in the same class.</span></span>
*  <span data-ttu-id="56f2d-421">Název metody se musí lišit od názvů ostatních bez metody s deklarací ve stejné třídě.</span><span class="sxs-lookup"><span data-stu-id="56f2d-421">The name of a method must differ from the names of all other non-methods declared in the same class.</span></span> <span data-ttu-id="56f2d-422">Kromě toho, podpis ([podpisy a přetížení](basic-concepts.md#signatures-and-overloading)) z metody se musí lišit od podpisů všechny ostatní metody s deklarací ve stejné třídě, a dvě metody s deklarací ve stejné třídě, nemusí mít podpisy, které se liší pouze podle `ref` a `out`.</span><span class="sxs-lookup"><span data-stu-id="56f2d-422">In addition, the signature ([Signatures and overloading](basic-concepts.md#signatures-and-overloading)) of a method must differ from the signatures of all other methods declared in the same class, and two methods declared in the same class may not have signatures that differ solely by `ref` and `out`.</span></span>
*  <span data-ttu-id="56f2d-423">Podpis konstruktoru instance se musí lišit od podpisů všechny ostatní instance konstruktory deklarované ve stejné třídě, a dva konstruktory deklarované ve stejné třídě, nemusí mít podpisy, které se liší pouze `ref` a `out`.</span><span class="sxs-lookup"><span data-stu-id="56f2d-423">The signature of an instance constructor must differ from the signatures of all other instance constructors declared in the same class, and two constructors declared in the same class may not have signatures that differ solely by `ref` and `out`.</span></span>
*  <span data-ttu-id="56f2d-424">Podpis indexeru se musí lišit od podpisů ze všech indexerů deklarované ve stejné třídě.</span><span class="sxs-lookup"><span data-stu-id="56f2d-424">The signature of an indexer must differ from the signatures of all other indexers declared in the same class.</span></span>
*  <span data-ttu-id="56f2d-425">Podpis operátora se musí lišit od podpisů všechny ostatní operátory, které jsou deklarovány ve stejné třídě.</span><span class="sxs-lookup"><span data-stu-id="56f2d-425">The signature of an operator must differ from the signatures of all other operators declared in the same class.</span></span>

<span data-ttu-id="56f2d-426">Zděděné členy typu třídy ([dědičnosti](classes.md#inheritance)) nejsou součástí místa deklarace třídy.</span><span class="sxs-lookup"><span data-stu-id="56f2d-426">The inherited members of a class type ([Inheritance](classes.md#inheritance)) are not part of the declaration space of a class.</span></span> <span data-ttu-id="56f2d-427">Proto odvozená třída může deklarovat člena se stejným názvem a signaturou jako zděděného člena (což ve skutečnosti skrývá zděděný člen).</span><span class="sxs-lookup"><span data-stu-id="56f2d-427">Thus, a derived class is allowed to declare a member with the same name or signature as an inherited member (which in effect hides the inherited member).</span></span>

### <a name="the-instance-type"></a><span data-ttu-id="56f2d-428">Typ instance</span><span class="sxs-lookup"><span data-stu-id="56f2d-428">The instance type</span></span>

<span data-ttu-id="56f2d-429">Každou deklaraci třídy je přidružen vázaný typ ([vázaný a nevázaných typy](types.md#bound-and-unbound-types)), ***typ instance***.</span><span class="sxs-lookup"><span data-stu-id="56f2d-429">Each class declaration has an associated bound type ([Bound and unbound types](types.md#bound-and-unbound-types)), the ***instance type***.</span></span> <span data-ttu-id="56f2d-430">Pro deklaraci obecné třídy, je vytvořen typ instance tak, že vytvoříte konstruovaný typ ([vytvořený typy](types.md#constructed-types)) z deklarace typu pro každý zadaný typ argumentů se odpovídající parametr typu.</span><span class="sxs-lookup"><span data-stu-id="56f2d-430">For a generic class declaration, the instance type is formed by creating a constructed type ([Constructed types](types.md#constructed-types)) from the type declaration, with each of the supplied type arguments being the corresponding type parameter.</span></span> <span data-ttu-id="56f2d-431">Protože typ instance používá parametry typu, ho jde použít jenom ve kterém jsou parametry typu v oboru, To znamená uvnitř deklarace třídy.</span><span class="sxs-lookup"><span data-stu-id="56f2d-431">Since the instance type uses the type parameters, it can only be used where the type parameters are in scope; that is, inside the class declaration.</span></span> <span data-ttu-id="56f2d-432">Typ instance je typu `this` pro kód napsaný v deklaraci třídy.</span><span class="sxs-lookup"><span data-stu-id="56f2d-432">The instance type is the type of `this` for code written inside the class declaration.</span></span> <span data-ttu-id="56f2d-433">Typ instance pro obecné třídy, je jednoduše deklarované třídy.</span><span class="sxs-lookup"><span data-stu-id="56f2d-433">For non-generic classes, the instance type is simply the declared class.</span></span> <span data-ttu-id="56f2d-434">Následuje několik deklarace tříd spolu s jejich typy instancí:</span><span class="sxs-lookup"><span data-stu-id="56f2d-434">The following shows several class declarations along with their instance types:</span></span> 
```csharp
class A<T>                           // instance type: A<T>
{
    class B {}                       // instance type: A<T>.B
    class C<U> {}                    // instance type: A<T>.C<U>
}

class D {}                           // instance type: D
```

### <a name="members-of-constructed-types"></a><span data-ttu-id="56f2d-435">Členové sestavené typy</span><span class="sxs-lookup"><span data-stu-id="56f2d-435">Members of constructed types</span></span>

<span data-ttu-id="56f2d-436">Získávají se-zděděné členy konstruovaný typ nahrazením pro každou *type_parameter* v deklaraci člena odpovídajícího *type_argument* sestaveného typu.</span><span class="sxs-lookup"><span data-stu-id="56f2d-436">The non-inherited members of a constructed type are obtained by substituting, for each *type_parameter* in the member declaration, the corresponding *type_argument* of the constructed type.</span></span> <span data-ttu-id="56f2d-437">Proces nahrazení vychází sémantický význam deklarace typů a není jednoduše textové nahrazení.</span><span class="sxs-lookup"><span data-stu-id="56f2d-437">The substitution process is based on the semantic meaning of type declarations, and is not simply textual substitution.</span></span>

<span data-ttu-id="56f2d-438">Mějme například deklaraci obecné třídy</span><span class="sxs-lookup"><span data-stu-id="56f2d-438">For example, given the generic class declaration</span></span>
```csharp
class Gen<T,U>
{
    public T[,] a;
    public void G(int i, T t, Gen<U,T> gt) {...}
    public U Prop { get {...} set {...} }
    public int H(double d) {...}
}
```
<span data-ttu-id="56f2d-439">konstruovaný typ `Gen<int[],IComparable<string>>` má následující členy:</span><span class="sxs-lookup"><span data-stu-id="56f2d-439">the constructed type `Gen<int[],IComparable<string>>` has the following members:</span></span>
```csharp
public int[,][] a;
public void G(int i, int[] t, Gen<IComparable<string>,int[]> gt) {...}
public IComparable<string> Prop { get {...} set {...} }
public int H(double d) {...}
```

<span data-ttu-id="56f2d-440">Typ člena `a` v deklaraci obecné třídy `Gen` je "dvourozměrné pole `T`", protože typ člena `a` ve výše uvedené konstruovaný typ je "dvourozměrné pole jednorozměrné pole `int`", nebo `int[,][]`.</span><span class="sxs-lookup"><span data-stu-id="56f2d-440">The type of the member `a` in the generic class declaration `Gen` is "two-dimensional array of `T`", so the type of the member `a` in the constructed type above is "two-dimensional array of one-dimensional array of `int`", or `int[,][]`.</span></span>

<span data-ttu-id="56f2d-441">V rámci instance funkce členy typu `this` je typu instance ([typ instance](classes.md#the-instance-type)) obsahující deklarace.</span><span class="sxs-lookup"><span data-stu-id="56f2d-441">Within instance function members, the type of `this` is the instance type ([The instance type](classes.md#the-instance-type)) of the containing declaration.</span></span>

<span data-ttu-id="56f2d-442">Všichni členové obecné třídy můžete použít parametry typu z nadřazené třídy, buď přímo, nebo jako součást konstruovaný typ.</span><span class="sxs-lookup"><span data-stu-id="56f2d-442">All members of a generic class can use type parameters from any enclosing class, either directly or as part of a constructed type.</span></span> <span data-ttu-id="56f2d-443">Když konkrétní uzavřený konstruovaný typ. ([otevřené a uzavřené typy](types.md#open-and-closed-types)) se používá v době běhu, každé použití klíčového parametr typu je nahrazen skutečným typem argument zadaný pro konstruovaný typ.</span><span class="sxs-lookup"><span data-stu-id="56f2d-443">When a particular closed constructed type ([Open and closed types](types.md#open-and-closed-types)) is used at run-time, each use of a type parameter is replaced with the actual type argument supplied to the constructed type.</span></span> <span data-ttu-id="56f2d-444">Příklad:</span><span class="sxs-lookup"><span data-stu-id="56f2d-444">For example:</span></span>
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

### <a name="inheritance"></a><span data-ttu-id="56f2d-445">Dědičnost</span><span class="sxs-lookup"><span data-stu-id="56f2d-445">Inheritance</span></span>

<span data-ttu-id="56f2d-446">Třída ***dědí*** členy jeho přímou základní třídu typu.</span><span class="sxs-lookup"><span data-stu-id="56f2d-446">A class ***inherits*** the members of its direct base class type.</span></span> <span data-ttu-id="56f2d-447">Dědičnost znamená, že třídy implicitně obsahuje všechny členy jeho přímou základní třídu typu s výjimkou instance konstruktory, destruktory a statické konstruktory základní třídy.</span><span class="sxs-lookup"><span data-stu-id="56f2d-447">Inheritance means that a class implicitly contains all members of its direct base class type, except for the instance constructors, destructors and static constructors of the base class.</span></span> <span data-ttu-id="56f2d-448">Jsou některé důležité aspekty dědičnosti:</span><span class="sxs-lookup"><span data-stu-id="56f2d-448">Some important aspects of inheritance are:</span></span>

*  <span data-ttu-id="56f2d-449">Dědičnost je přenosné.</span><span class="sxs-lookup"><span data-stu-id="56f2d-449">Inheritance is transitive.</span></span> <span data-ttu-id="56f2d-450">Pokud `C` je odvozen z `B`, a `B` je odvozen z `A`, pak `C` dědí členy deklarované v `B` a také členy deklarované v `A`.</span><span class="sxs-lookup"><span data-stu-id="56f2d-450">If `C` is derived from `B`, and `B` is derived from `A`, then `C` inherits the members declared in `B` as well as the members declared in `A`.</span></span>
*  <span data-ttu-id="56f2d-451">Odvozená třída rozšiřuje své přímé základní třídy.</span><span class="sxs-lookup"><span data-stu-id="56f2d-451">A derived class extends its direct base class.</span></span> <span data-ttu-id="56f2d-452">Odvozené třídy můžete přidat nové členy pro ty, které se dědí, ale nemůže odstranit definici zděděného člena.</span><span class="sxs-lookup"><span data-stu-id="56f2d-452">A derived class can add new members to those it inherits, but it cannot remove the definition of an inherited member.</span></span>
*  <span data-ttu-id="56f2d-453">Instance konstruktory, destruktory a statické konstruktory nejsou děděna, ale ostatní členové jsou, bez ohledu na jejich deklarovaná přístupnost ([přístup ke členu](basic-concepts.md#member-access)).</span><span class="sxs-lookup"><span data-stu-id="56f2d-453">Instance constructors, destructors, and static constructors are not inherited, but all other members are, regardless of their declared accessibility ([Member access](basic-concepts.md#member-access)).</span></span> <span data-ttu-id="56f2d-454">V závislosti na jejich deklarovaná přístupnost, nemusí být přístupné v odvozené třídě zděděných členů.</span><span class="sxs-lookup"><span data-stu-id="56f2d-454">However, depending on their declared accessibility, inherited members might not be accessible in a derived class.</span></span>
*  <span data-ttu-id="56f2d-455">Odvozená třída může ***skrýt*** ([skrytí prostřednictvím dědičnosti](basic-concepts.md#hiding-through-inheritance)) zděděné členy deklarováním nové členy s totožným názvem a signaturou.</span><span class="sxs-lookup"><span data-stu-id="56f2d-455">A derived class can ***hide*** ([Hiding through inheritance](basic-concepts.md#hiding-through-inheritance)) inherited members by declaring new members with the same name or signature.</span></span> <span data-ttu-id="56f2d-456">Nezapomeňte ale, že skrytím zděděného člena neodebere, se kterou – umožňuje pouze tento člen přístupný přímo prostřednictvím odvozené třídy.</span><span class="sxs-lookup"><span data-stu-id="56f2d-456">Note however that hiding an inherited member does not remove that member—it merely makes that member inaccessible directly through the derived class.</span></span>
*  <span data-ttu-id="56f2d-457">Instance třídy obsahuje sadu všechna pole instancí, které jsou deklarovány ve třídě a jejích základních tříd a implicitní převod ([odkaz na implicitní převody](conversions.md#implicit-reference-conversions)) existuje z typu odvozené třídy pro některé typy jejího základní třídy.</span><span class="sxs-lookup"><span data-stu-id="56f2d-457">An instance of a class contains a set of all instance fields declared in the class and its base classes, and an implicit conversion ([Implicit reference conversions](conversions.md#implicit-reference-conversions)) exists from a derived class type to any of its base class types.</span></span> <span data-ttu-id="56f2d-458">Díky tomu se odkaz na instanci některé z odvozených tříd lze považovat za odkaz na instanci některý z jeho základních tříd.</span><span class="sxs-lookup"><span data-stu-id="56f2d-458">Thus, a reference to an instance of some derived class can be treated as a reference to an instance of any of its base classes.</span></span>
*  <span data-ttu-id="56f2d-459">Třídy lze deklarovat virtuální metody, vlastnostmi a indexery a odvozených tříd může přepsat implementaci těchto funkcí členů.</span><span class="sxs-lookup"><span data-stu-id="56f2d-459">A class can declare virtual methods, properties, and indexers, and derived classes can override the implementation of these function members.</span></span> <span data-ttu-id="56f2d-460">To umožňuje polymorfní chování, ve které akce prováděné při volání členské funkce se liší v závislosti na run-time typu instance, jejímž prostřednictvím je vyvolána funkce člena třídy.</span><span class="sxs-lookup"><span data-stu-id="56f2d-460">This enables classes to exhibit polymorphic behavior wherein the actions performed by a function member invocation varies depending on the run-time type of the instance through which that function member is invoked.</span></span>

<span data-ttu-id="56f2d-461">Zděděný člen typu vytvořeného třídy jsou členy typu přímé základní třídy ([základních tříd](classes.md#base-classes)), která byla nalezena nahrazením argumentů typu sestaveného typu pro každý výskyt odpovídající typ v parametrech *class_base* specifikace.</span><span class="sxs-lookup"><span data-stu-id="56f2d-461">The inherited member of a constructed class type are the members of the immediate base class type ([Base classes](classes.md#base-classes)), which is found by substituting the type arguments of the constructed type for each occurrence of the corresponding type parameters in the *class_base* specification.</span></span> <span data-ttu-id="56f2d-462">Tyto členy, pak jsou transformovány sadou nahrazování pro každou *type_parameter* v deklaraci člena odpovídajícího *type_argument* z *class_base* specifikace.</span><span class="sxs-lookup"><span data-stu-id="56f2d-462">These members, in turn, are transformed by substituting, for each *type_parameter* in the member declaration, the corresponding *type_argument* of the *class_base* specification.</span></span>

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

<span data-ttu-id="56f2d-463">V příkladu výše, konstruovaný typ `D<int>` má člen zděděné `public int G(string s)` získala při nahrazování argument typu `int` pro parametr typu `T`.</span><span class="sxs-lookup"><span data-stu-id="56f2d-463">In the above example, the constructed type `D<int>` has a non-inherited member `public int G(string s)` obtained by substituting the type argument `int` for the type parameter `T`.</span></span> <span data-ttu-id="56f2d-464">`D<int>` má také zděděného členu v deklaraci třídy `B`.</span><span class="sxs-lookup"><span data-stu-id="56f2d-464">`D<int>` also has an inherited member from the class declaration `B`.</span></span> <span data-ttu-id="56f2d-465">Tento člen zděděný je určeno první typ základní třídy určující `B<int[]>` z `D<int>` nahrazením `int` pro `T` ve specifikaci základní třídy `B<T[]>`.</span><span class="sxs-lookup"><span data-stu-id="56f2d-465">This inherited member is determined by first determining the base class type `B<int[]>` of `D<int>` by substituting `int` for `T` in the base class specification `B<T[]>`.</span></span> <span data-ttu-id="56f2d-466">Potom jako argument typu pro `B`, `int[]` nahrazuje `U` v `public U F(long index)`, což má za následek zděděný člen `public int[] F(long index)`.</span><span class="sxs-lookup"><span data-stu-id="56f2d-466">Then, as a type argument to `B`, `int[]` is substituted for `U` in `public U F(long index)`, yielding the inherited member `public int[] F(long index)`.</span></span>

### <a name="the-new-modifier"></a><span data-ttu-id="56f2d-467">New – modifikátor</span><span class="sxs-lookup"><span data-stu-id="56f2d-467">The new modifier</span></span>

<span data-ttu-id="56f2d-468">A *class_member_declaration* může deklarovat člena se stejným názvem a signaturou jako zděděného člena.</span><span class="sxs-lookup"><span data-stu-id="56f2d-468">A *class_member_declaration* is permitted to declare a member with the same name or signature as an inherited member.</span></span> <span data-ttu-id="56f2d-469">V tomto případě člena odvozené třídy se říká, že ***skrýt*** členu základní třídy.</span><span class="sxs-lookup"><span data-stu-id="56f2d-469">When this occurs, the derived class member is said to ***hide*** the base class member.</span></span> <span data-ttu-id="56f2d-470">Skrytí zděděného člena není považováno za chybu, ale to způsobit, že kompilátor vydat upozornění.</span><span class="sxs-lookup"><span data-stu-id="56f2d-470">Hiding an inherited member is not considered an error, but it does cause the compiler to issue a warning.</span></span> <span data-ttu-id="56f2d-471">Potlačit upozornění, může obsahovat deklarace člena odvozené třídy `new` modifikátor k označení, že odvozené člen má skrýt základního člena.</span><span class="sxs-lookup"><span data-stu-id="56f2d-471">To suppress the warning, the declaration of the derived class member can include a `new` modifier to indicate that the derived member is intended to hide the base member.</span></span> <span data-ttu-id="56f2d-472">V tomto tématu je popsán dále v [skrytí prostřednictvím dědičnosti](basic-concepts.md#hiding-through-inheritance).</span><span class="sxs-lookup"><span data-stu-id="56f2d-472">This topic is discussed further in [Hiding through inheritance](basic-concepts.md#hiding-through-inheritance).</span></span>

<span data-ttu-id="56f2d-473">Pokud `new` Modifikátor je součástí deklarace, která neskryje zděděného člena, příslušná objeví se upozornění.</span><span class="sxs-lookup"><span data-stu-id="56f2d-473">If a `new` modifier is included in a declaration that doesn't hide an inherited member, a warning to that effect is issued.</span></span> <span data-ttu-id="56f2d-474">Toto upozornění je potlačeno odebráním `new` modifikátor.</span><span class="sxs-lookup"><span data-stu-id="56f2d-474">This warning is suppressed by removing the `new` modifier.</span></span>

### <a name="access-modifiers"></a><span data-ttu-id="56f2d-475">Modifikátory přístupu</span><span class="sxs-lookup"><span data-stu-id="56f2d-475">Access modifiers</span></span>

<span data-ttu-id="56f2d-476">A *class_member_declaration* může mít některou z pěti druhy deklarovaná přístupnost ([deklarovaná přístupnost](basic-concepts.md#declared-accessibility)): `public`, `protected internal`, `protected`, `internal` , nebo `private`.</span><span class="sxs-lookup"><span data-stu-id="56f2d-476">A *class_member_declaration* can have any one of the five possible kinds of declared accessibility ([Declared accessibility](basic-concepts.md#declared-accessibility)): `public`, `protected internal`, `protected`, `internal`, or `private`.</span></span> <span data-ttu-id="56f2d-477">S výjimkou `protected internal` kombinaci, je chyba kompilace určit více než jeden modifikátor přístupu.</span><span class="sxs-lookup"><span data-stu-id="56f2d-477">Except for the `protected internal` combination, it is a compile-time error to specify more than one access modifier.</span></span> <span data-ttu-id="56f2d-478">Když *class_member_declaration* nezahrnuje žádné modifikátory přístupu `private` se předpokládá, že.</span><span class="sxs-lookup"><span data-stu-id="56f2d-478">When a *class_member_declaration* does not include any access modifiers, `private` is assumed.</span></span>

### <a name="constituent-types"></a><span data-ttu-id="56f2d-479">Základní typy</span><span class="sxs-lookup"><span data-stu-id="56f2d-479">Constituent types</span></span>

<span data-ttu-id="56f2d-480">Typy, které se používají v deklaraci člena jsou volány základní typy daného člena.</span><span class="sxs-lookup"><span data-stu-id="56f2d-480">Types that are used in the declaration of a member are called the constituent types of that member.</span></span> <span data-ttu-id="56f2d-481">Je to možné základní typy jsou typ konstanty, pole, vlastnosti, události, nebo indexeru, návratový typ metody nebo operátor a typy parametrů metody, indexer, operátor nebo konstruktor instance.</span><span class="sxs-lookup"><span data-stu-id="56f2d-481">Possible constituent types are the type of a constant, field, property, event, or indexer, the return type of a method or operator, and the parameter types of a method, indexer, operator, or instance constructor.</span></span> <span data-ttu-id="56f2d-482">Základní typy člena musí být přinejmenším stejně dostupná jako samotný člen ([usnadnění omezení](basic-concepts.md#accessibility-constraints)).</span><span class="sxs-lookup"><span data-stu-id="56f2d-482">The constituent types of a member must be at least as accessible as that member itself ([Accessibility constraints](basic-concepts.md#accessibility-constraints)).</span></span>

### <a name="static-and-instance-members"></a><span data-ttu-id="56f2d-483">Členy statických a instance</span><span class="sxs-lookup"><span data-stu-id="56f2d-483">Static and instance members</span></span>

<span data-ttu-id="56f2d-484">Členy třídy jsou buď ***statické členy*** nebo ***členy instance***.</span><span class="sxs-lookup"><span data-stu-id="56f2d-484">Members of a class are either ***static members*** or ***instance members***.</span></span> <span data-ttu-id="56f2d-485">Obecně je vhodné popřemýšlet o statické členy jako patřící do typů tříd a členů instance jako patřící do objektů (instance typů tříd).</span><span class="sxs-lookup"><span data-stu-id="56f2d-485">Generally speaking, it is useful to think of static members as belonging to class types and instance members as belonging to objects (instances of class types).</span></span>

<span data-ttu-id="56f2d-486">Při deklaraci pole, metoda, vlastnost, událost, operátor nebo konstruktor obsahuje `static` modifikátor, deklaruje statický člen.</span><span class="sxs-lookup"><span data-stu-id="56f2d-486">When a field, method, property, event, operator, or constructor declaration includes a `static` modifier, it declares a static member.</span></span> <span data-ttu-id="56f2d-487">Kromě toho deklaraci konstanty nebo typu implicitně deklaruje název statického člena.</span><span class="sxs-lookup"><span data-stu-id="56f2d-487">In addition, a constant or type declaration implicitly declares a static member.</span></span> <span data-ttu-id="56f2d-488">Statické členy mají následující vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="56f2d-488">Static members have the following characteristics:</span></span>

*  <span data-ttu-id="56f2d-489">Při statického člena, který `M` odkazuje *member_access* ([přístup ke členu](expressions.md#member-access)) ve formátu `E.M`, `E` musí označují typ obsahující `M`.</span><span class="sxs-lookup"><span data-stu-id="56f2d-489">When a static member `M` is referenced in a *member_access* ([Member access](expressions.md#member-access)) of the form `E.M`, `E` must denote a type containing `M`.</span></span> <span data-ttu-id="56f2d-490">Je chyba kompilace pro `E` k označení instance.</span><span class="sxs-lookup"><span data-stu-id="56f2d-490">It is a compile-time error for `E` to denote an instance.</span></span>
*  <span data-ttu-id="56f2d-491">Statické pole identifikuje přesně jednoho umístění úložiště do sdíleného všemi instancemi typu dané uzavřené třídy.</span><span class="sxs-lookup"><span data-stu-id="56f2d-491">A static field identifies exactly one storage location to be shared by all instances of a given closed class type.</span></span> <span data-ttu-id="56f2d-492">Bez ohledu na to, kolik instancí typu dané uzavřené třídy jsou vytvořeny je někdy pouze jednu kopii tohoto statické pole.</span><span class="sxs-lookup"><span data-stu-id="56f2d-492">No matter how many instances of a given closed class type are created, there is only ever one copy of a static field.</span></span>
*  <span data-ttu-id="56f2d-493">Statická funkce člena (metoda, vlastnost, událost, operátor nebo konstruktor) nefunguje na konkrétní instanci a je chyba kompilace k odkazování na `this` v členské funkce.</span><span class="sxs-lookup"><span data-stu-id="56f2d-493">A static function member (method, property, event, operator, or constructor) does not operate on a specific instance, and it is a compile-time error to refer to `this` in such a function member.</span></span>

<span data-ttu-id="56f2d-494">Při deklaraci pole, metoda, vlastnost, událost, indexer, konstruktor nebo destruktor nezahrnuje `static` modifikátor, deklaruje člen instance.</span><span class="sxs-lookup"><span data-stu-id="56f2d-494">When a field, method, property, event, indexer, constructor, or destructor declaration does not include a `static` modifier, it declares an instance member.</span></span> <span data-ttu-id="56f2d-495">(Člen instance se někdy nazývá nestatického člena.) Členy instance mají následující vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="56f2d-495">(An instance member is sometimes called a non-static member.) Instance members have the following characteristics:</span></span>

*  <span data-ttu-id="56f2d-496">Při členem instance `M` odkazuje *member_access* ([přístup ke členu](expressions.md#member-access)) formuláře `E.M`, `E` musí označit instanci typu, který obsahuje `M`.</span><span class="sxs-lookup"><span data-stu-id="56f2d-496">When an instance member `M` is referenced in a *member_access* ([Member access](expressions.md#member-access)) of the form `E.M`, `E` must denote an instance of a type containing `M`.</span></span> <span data-ttu-id="56f2d-497">Jedná se chybu doba vazby pro `E` k označení typu.</span><span class="sxs-lookup"><span data-stu-id="56f2d-497">It is a binding-time error for `E` to denote a type.</span></span>
*  <span data-ttu-id="56f2d-498">Každá instance třídy obsahuje samostatnou sadu všechna pole instancí třídy.</span><span class="sxs-lookup"><span data-stu-id="56f2d-498">Every instance of a class contains a separate set of all instance fields of the class.</span></span>
*  <span data-ttu-id="56f2d-499">Funkce člena instance (metoda, vlastnost, indexer, konstruktor instance nebo destruktor) funguje v dané instanci třídy a tato instance je přístupná jako `this` ([tento přístup](expressions.md#this-access)).</span><span class="sxs-lookup"><span data-stu-id="56f2d-499">An instance function member (method, property, indexer, instance constructor, or destructor) operates on a given instance of the class, and this instance can be accessed as `this` ([This access](expressions.md#this-access)).</span></span>

<span data-ttu-id="56f2d-500">Následující příklad znázorňuje pravidla pro přístup k statické a členy instance:</span><span class="sxs-lookup"><span data-stu-id="56f2d-500">The following example illustrates the rules for accessing static and instance members:</span></span>
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

<span data-ttu-id="56f2d-501">`F` Metoda ukazuje, že ve funkci členem instance *simple_name* ([jednoduché názvy](expressions.md#simple-names)) lze použít pro přístup k instanci členy a statické členy.</span><span class="sxs-lookup"><span data-stu-id="56f2d-501">The `F` method shows that in an instance function member, a *simple_name* ([Simple names](expressions.md#simple-names)) can be used to access both instance members and static members.</span></span> <span data-ttu-id="56f2d-502">`G` Metoda ukazuje, že v statická funkce členu, je chyba kompilace pro přístup k instanci člena prostřednictvím *simple_name*.</span><span class="sxs-lookup"><span data-stu-id="56f2d-502">The `G` method shows that in a static function member, it is a compile-time error to access an instance member through a *simple_name*.</span></span> <span data-ttu-id="56f2d-503">`Main` Metoda ukazuje, že *member_access* ([přístup ke členu](expressions.md#member-access)) členy instance je možný přes instancí a statické členy je možný prostřednictvím typů.</span><span class="sxs-lookup"><span data-stu-id="56f2d-503">The `Main` method shows that in a *member_access* ([Member access](expressions.md#member-access)), instance members must be accessed through instances, and static members must be accessed through types.</span></span>

### <a name="nested-types"></a><span data-ttu-id="56f2d-504">Vnořené typy</span><span class="sxs-lookup"><span data-stu-id="56f2d-504">Nested types</span></span>

<span data-ttu-id="56f2d-505">Typ deklarovaný v rámci deklarace třídy nebo struktury se nazývá ***vnořený typ***.</span><span class="sxs-lookup"><span data-stu-id="56f2d-505">A type declared within a class or struct declaration is called a ***nested type***.</span></span> <span data-ttu-id="56f2d-506">Typ, který je deklarován v rámci kompilační jednotky nebo oboru názvů je volána ***nevnořený typ.***.</span><span class="sxs-lookup"><span data-stu-id="56f2d-506">A type that is declared within a compilation unit or namespace is called a ***non-nested type***.</span></span>

<span data-ttu-id="56f2d-507">V příkladu</span><span class="sxs-lookup"><span data-stu-id="56f2d-507">In the example</span></span>
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
<span data-ttu-id="56f2d-508">Třída `B` je vnořený typ, protože je deklarována v rámci třídy `A`a třídy `A` je nevnořeném typu, protože je deklarována v rámci kompilační jednotky.</span><span class="sxs-lookup"><span data-stu-id="56f2d-508">class `B` is a nested type because it is declared within class `A`, and class `A` is a non-nested type because it is declared within a compilation unit.</span></span>

#### <a name="fully-qualified-name"></a><span data-ttu-id="56f2d-509">Plně kvalifikovaný název</span><span class="sxs-lookup"><span data-stu-id="56f2d-509">Fully qualified name</span></span>

<span data-ttu-id="56f2d-510">Plně kvalifikovaný název ([plně kvalifikované názvy](basic-concepts.md#fully-qualified-names)) pro vnořený typ je `S.N` kde `S` je plně kvalifikovaný název typu, v jakém typu `N` je deklarována.</span><span class="sxs-lookup"><span data-stu-id="56f2d-510">The fully qualified name ([Fully qualified names](basic-concepts.md#fully-qualified-names)) for a nested type is `S.N` where `S` is the fully qualified name of the type in which type `N` is declared.</span></span>

#### <a name="declared-accessibility"></a><span data-ttu-id="56f2d-511">Deklarovaná přístupnost</span><span class="sxs-lookup"><span data-stu-id="56f2d-511">Declared accessibility</span></span>

<span data-ttu-id="56f2d-512">Může mít nevnořených typů `public` nebo `internal` deklarován usnadnění a mít `internal` deklarovaná přístupnost ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="56f2d-512">Non-nested types can have `public` or `internal` declared accessibility and have `internal` declared accessibility by default.</span></span> <span data-ttu-id="56f2d-513">Vnořené typy mohou mít tyto formy deklarovaná přístupnost, plus nejmíň jeden další formy deklarovaná přístupnost, v závislosti na tom, zda je obsahující typ třídy nebo struktury:</span><span class="sxs-lookup"><span data-stu-id="56f2d-513">Nested types can have these forms of declared accessibility too, plus one or more additional forms of declared accessibility, depending on whether the containing type is a class or struct:</span></span>

*  <span data-ttu-id="56f2d-514">Vnořený typ, který je deklarován ve třídě může mít některý z pěti formy deklarovaná přístupnost (`public`, `protected internal`, `protected`, `internal`, nebo `private`) a stejně jako ostatní členové třídy, výchozí hodnota je `private` deklarována usnadnění přístupu.</span><span class="sxs-lookup"><span data-stu-id="56f2d-514">A nested type that is declared in a class can have any of five forms of declared accessibility (`public`, `protected internal`, `protected`, `internal`, or `private`) and, like other class members, defaults to `private` declared accessibility.</span></span>
*  <span data-ttu-id="56f2d-515">Vnořený typ, který je deklarován v struktury mohou mít tři formy deklarovaná přístupnost (`public`, `internal`, nebo `private`) a stejně jako ostatní členové struktury výchozí `private` deklarovaná přístupnost.</span><span class="sxs-lookup"><span data-stu-id="56f2d-515">A nested type that is declared in a struct can have any of three forms of declared accessibility (`public`, `internal`, or `private`) and, like other struct members, defaults to `private` declared accessibility.</span></span>

<span data-ttu-id="56f2d-516">V příkladu</span><span class="sxs-lookup"><span data-stu-id="56f2d-516">The example</span></span>
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
<span data-ttu-id="56f2d-517">deklaruje privátní vnořené třídy `Node`.</span><span class="sxs-lookup"><span data-stu-id="56f2d-517">declares a private nested class `Node`.</span></span>

#### <a name="hiding"></a><span data-ttu-id="56f2d-518">Skrytí</span><span class="sxs-lookup"><span data-stu-id="56f2d-518">Hiding</span></span>

<span data-ttu-id="56f2d-519">Vnořený typ může skrýt ([skrytí názvu](basic-concepts.md#name-hiding)) základního člena.</span><span class="sxs-lookup"><span data-stu-id="56f2d-519">A nested type may hide ([Name hiding](basic-concepts.md#name-hiding)) a base member.</span></span> <span data-ttu-id="56f2d-520">`new` Modifikátor můžou běžet na deklarace vnořených typů tak, aby skrytí lze vyjádřit explicitně.</span><span class="sxs-lookup"><span data-stu-id="56f2d-520">The `new` modifier is permitted on nested type declarations so that hiding can be expressed explicitly.</span></span> <span data-ttu-id="56f2d-521">V příkladu</span><span class="sxs-lookup"><span data-stu-id="56f2d-521">The example</span></span>
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
<span data-ttu-id="56f2d-522">ukazuje, vnořené třídy `M` , která skrývá metodu `M` definované v `Base`.</span><span class="sxs-lookup"><span data-stu-id="56f2d-522">shows a nested class `M` that hides the method `M` defined in `Base`.</span></span>

#### <a name="this-access"></a><span data-ttu-id="56f2d-523">Tento přístup</span><span class="sxs-lookup"><span data-stu-id="56f2d-523">this access</span></span>

<span data-ttu-id="56f2d-524">Vnořený typ a jeho nadřazeného typu nemají zvláštní vztah s ohledem na *this_access* ([tento přístup](expressions.md#this-access)).</span><span class="sxs-lookup"><span data-stu-id="56f2d-524">A nested type and its containing type do not have a special relationship with regard to *this_access* ([This access](expressions.md#this-access)).</span></span> <span data-ttu-id="56f2d-525">Konkrétně `this` uvnitř vnořeného typu nelze použít k odkazování na členy instance nadřazeného typu.</span><span class="sxs-lookup"><span data-stu-id="56f2d-525">Specifically, `this` within a nested type cannot be used to refer to instance members of the containing type.</span></span> <span data-ttu-id="56f2d-526">V případech, kdy je vnořený typ přístupu ke členům instance jeho nadřazeného typu, lze zadat přístup tím, že poskytuje `this` pro instanci jako argument konstruktoru pro vnořeného typu nadřazeného typu.</span><span class="sxs-lookup"><span data-stu-id="56f2d-526">In cases where a nested type needs access to the instance members of its containing type, access can be provided by providing the `this` for the instance of the containing type as a constructor argument for the nested type.</span></span> <span data-ttu-id="56f2d-527">Následující příklad</span><span class="sxs-lookup"><span data-stu-id="56f2d-527">The following example</span></span>
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
<span data-ttu-id="56f2d-528">Zobrazí tuto techniku.</span><span class="sxs-lookup"><span data-stu-id="56f2d-528">shows this technique.</span></span> <span data-ttu-id="56f2d-529">Instance `C` vytvoří instanci `Nested` a předává své vlastní `this` k `Nested`pro konstruktor, aby bylo možné poskytnout další přístup k `C`na členy instance.</span><span class="sxs-lookup"><span data-stu-id="56f2d-529">An instance of `C` creates an instance of `Nested` and passes its own `this` to `Nested`'s constructor in order to provide subsequent access to `C`'s instance members.</span></span>

#### <a name="access-to-private-and-protected-members-of-the-containing-type"></a><span data-ttu-id="56f2d-530">Přístup k soukromým a chráněným členům nadřazeného typu.</span><span class="sxs-lookup"><span data-stu-id="56f2d-530">Access to private and protected members of the containing type</span></span>

<span data-ttu-id="56f2d-531">Vnořený typ má přístup ke všem členům, kteří jsou k dispozici k jeho nadřazeného typu, včetně členů nadřazeného typu, které mají `private` a `protected` deklarovaná přístupnost.</span><span class="sxs-lookup"><span data-stu-id="56f2d-531">A nested type has access to all of the members that are accessible to its containing type, including members of the containing type that have `private` and `protected` declared accessibility.</span></span> <span data-ttu-id="56f2d-532">V příkladu</span><span class="sxs-lookup"><span data-stu-id="56f2d-532">The example</span></span>
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
<span data-ttu-id="56f2d-533">ukazuje třídy `C` , která obsahuje vnořené třídy `Nested`.</span><span class="sxs-lookup"><span data-stu-id="56f2d-533">shows a class `C` that contains a nested class `Nested`.</span></span> <span data-ttu-id="56f2d-534">V rámci `Nested`, metoda `G` zavolat statickou metodu `F` definované v `C`, a `F` privátní je deklarovaná přístupnost.</span><span class="sxs-lookup"><span data-stu-id="56f2d-534">Within `Nested`, the method `G` calls the static method `F` defined in `C`, and `F` has private declared accessibility.</span></span>

<span data-ttu-id="56f2d-535">Vnořený typ také mohou přistupovat k chráněným členům definované v základním typu z jeho nadřazeného typu.</span><span class="sxs-lookup"><span data-stu-id="56f2d-535">A nested type also may access protected members defined in a base type of its containing type.</span></span> <span data-ttu-id="56f2d-536">V příkladu</span><span class="sxs-lookup"><span data-stu-id="56f2d-536">In the example</span></span>
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
<span data-ttu-id="56f2d-537">vnořené třídy `Derived.Nested` přistupuje k chráněná metoda `F` definované v `Derived`od základní třídy, `Base`, volání prostřednictvím instance `Derived`.</span><span class="sxs-lookup"><span data-stu-id="56f2d-537">the nested class `Derived.Nested` accesses the protected method `F` defined in `Derived`'s base class, `Base`, by calling through an instance of `Derived`.</span></span>

#### <a name="nested-types-in-generic-classes"></a><span data-ttu-id="56f2d-538">Vnořené typy obecných tříd</span><span class="sxs-lookup"><span data-stu-id="56f2d-538">Nested types in generic classes</span></span>

<span data-ttu-id="56f2d-539">Deklaraci obecné třídy mohou obsahovat deklarace vnořených typů.</span><span class="sxs-lookup"><span data-stu-id="56f2d-539">A generic class declaration can contain nested type declarations.</span></span> <span data-ttu-id="56f2d-540">Parametry typu nadřazené třídy je možné v rámci vnořené typy.</span><span class="sxs-lookup"><span data-stu-id="56f2d-540">The type parameters of the enclosing class can be used within the nested types.</span></span> <span data-ttu-id="56f2d-541">Deklarace vnořeného typu. může obsahovat další typové parametry, které se vztahují pouze k vnořeného typu.</span><span class="sxs-lookup"><span data-stu-id="56f2d-541">A nested type declaration can contain additional type parameters that apply only to the nested type.</span></span>

<span data-ttu-id="56f2d-542">Každý typ deklarace obsažené v deklaraci obecné třídy je implicitně deklarace obecného typu.</span><span class="sxs-lookup"><span data-stu-id="56f2d-542">Every type declaration contained within a generic class declaration is implicitly a generic type declaration.</span></span> <span data-ttu-id="56f2d-543">Při psaní odkaz na typ vnořené do obecného typu, musí mít název obsahující konstruovaný typ, včetně jeho argumentů typu.</span><span class="sxs-lookup"><span data-stu-id="56f2d-543">When writing a reference to a type nested within a generic type, the containing constructed type, including its type arguments, must be named.</span></span> <span data-ttu-id="56f2d-544">Však z vnější třídy vnořeného typu můžete bez kvalifikace lze používat; Typ instance z vnější třídy lze implicitně používá při vytváření vnořeného typu.</span><span class="sxs-lookup"><span data-stu-id="56f2d-544">However, from within the outer class, the nested type can be used without qualification; the instance type of the outer class can be implicitly used when constructing the nested type.</span></span> <span data-ttu-id="56f2d-545">Následující příklad ukazuje tři různé správné způsoby, jak odkazovat na konstruovaný typ vytvořený z `Inner`; první dvě jsou ekvivalentní:</span><span class="sxs-lookup"><span data-stu-id="56f2d-545">The following example shows three different correct ways to refer to a constructed type created from `Inner`; the first two are equivalent:</span></span>
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

<span data-ttu-id="56f2d-546">I když je chybný programování styl, parametr typu v vnořeného typu můžete skrýt člena nebo parametr deklarovaný v typu vnějšího typu:</span><span class="sxs-lookup"><span data-stu-id="56f2d-546">Although it is bad programming style, a type parameter in a nested type can hide a member or type parameter declared in the outer type:</span></span>
```csharp
class Outer<T>
{
    class Inner<T>        // Valid, hides Outer's T
    {
        public T t;       // Refers to Inner's T
    }
}
```

### <a name="reserved-member-names"></a><span data-ttu-id="56f2d-547">Názvy vyhrazené členů</span><span class="sxs-lookup"><span data-stu-id="56f2d-547">Reserved member names</span></span>

<span data-ttu-id="56f2d-548">K usnadnění základního jazyka C# za běhu, pro každou deklaraci člena zdroj, který je vlastnost, událost nebo indexeru, musíte rezervovat implementace dvou podpisy metod v závislosti na charakteru deklarace člena, jeho název a jeho typ.</span><span class="sxs-lookup"><span data-stu-id="56f2d-548">To facilitate the underlying C# run-time implementation, for each source member declaration that is a property, event, or indexer, the implementation must reserve two method signatures based on the kind of the member declaration, its name, and its type.</span></span> <span data-ttu-id="56f2d-549">Je chyba kompilace programu k deklarování člena, jehož předpis odpovídá jednomu z těchto vyhrazená podpisy, i v případě, že se základní implementací za běhu neprovede využívání tyto rezervace.</span><span class="sxs-lookup"><span data-stu-id="56f2d-549">It is a compile-time error for a program to declare a member whose signature matches one of these reserved signatures, even if the underlying run-time implementation does not make use of these reservations.</span></span>

<span data-ttu-id="56f2d-550">Vyhrazené názvy nezavádí deklarace, proto není účast ve vyhledávání člena.</span><span class="sxs-lookup"><span data-stu-id="56f2d-550">The reserved names do not introduce declarations, thus they do not participate in member lookup.</span></span> <span data-ttu-id="56f2d-551">Však deklarace přidružené k vyhrazené metoda podpisy zapojují do dědičnosti ([dědičnosti](classes.md#inheritance)) a může být skrytá s `new` modifikátor ([new – modifikátor](classes.md#the-new-modifier)).</span><span class="sxs-lookup"><span data-stu-id="56f2d-551">However, a declaration's associated reserved method signatures do participate in inheritance ([Inheritance](classes.md#inheritance)), and can be hidden with the `new` modifier ([The new modifier](classes.md#the-new-modifier)).</span></span>

<span data-ttu-id="56f2d-552">Rezervace tyto názvy slouží tři účelům:</span><span class="sxs-lookup"><span data-stu-id="56f2d-552">The reservation of these names serves three purposes:</span></span>

*  <span data-ttu-id="56f2d-553">Aby se základní implementací běžných identifikátor použít jako název metody pro získání nebo nastavení přístupu k funkci jazyka C#.</span><span class="sxs-lookup"><span data-stu-id="56f2d-553">To allow the underlying implementation to use an ordinary identifier as a method name for get or set access to the C# language feature.</span></span>
*  <span data-ttu-id="56f2d-554">Pro povolení jazyků pro spolupráci, pomocí běžných identifikátor jako název metody pro získání nebo nastavení přístupu k funkci jazyka C#.</span><span class="sxs-lookup"><span data-stu-id="56f2d-554">To allow other languages to interoperate using an ordinary identifier as a method name for get or set access to the C# language feature.</span></span>
*  <span data-ttu-id="56f2d-555">Pomáhá zajistit, že zdroj přijal jedna vyhovující kompilátoru přijme, tak, že specifika vyhrazeným členem názvy konzistentní napříč všemi implementacemi jazyka C#.</span><span class="sxs-lookup"><span data-stu-id="56f2d-555">To help ensure that the source accepted by one conforming compiler is accepted by another, by making the specifics of reserved member names consistent across all C# implementations.</span></span>

<span data-ttu-id="56f2d-556">Deklarace destruktoru ([destruktory](classes.md#destructors)) navíc způsobí, že podpis, které budou rezervovány ([názvy členů, které jsou vyhrazené pro destruktory](classes.md#member-names-reserved-for-destructors)).</span><span class="sxs-lookup"><span data-stu-id="56f2d-556">The declaration of a destructor ([Destructors](classes.md#destructors)) also causes a signature to be reserved ([Member names reserved for destructors](classes.md#member-names-reserved-for-destructors)).</span></span>

#### <a name="member-names-reserved-for-properties"></a><span data-ttu-id="56f2d-557">Názvy členů rezervované vlastnosti</span><span class="sxs-lookup"><span data-stu-id="56f2d-557">Member names reserved for properties</span></span>

<span data-ttu-id="56f2d-558">Pro vlastnost `P` ([vlastnosti](classes.md#properties)) typu `T`, následující podpisy jsou vyhrazené:</span><span class="sxs-lookup"><span data-stu-id="56f2d-558">For a property `P` ([Properties](classes.md#properties)) of type `T`, the following signatures are reserved:</span></span>

```csharp
T get_P();
void set_P(T value);
```

<span data-ttu-id="56f2d-559">Obě podpisy jsou vyhrazené, i v případě, že vlastnost je jen pro čtení nebo jen pro zápis.</span><span class="sxs-lookup"><span data-stu-id="56f2d-559">Both signatures are reserved, even if the property is read-only or write-only.</span></span>

<span data-ttu-id="56f2d-560">V příkladu</span><span class="sxs-lookup"><span data-stu-id="56f2d-560">In the example</span></span>
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
<span data-ttu-id="56f2d-561">Třída `A` definuje vlastnost jen pro čtení `P`, tedy rezervace podpisy pro `get_P` a `set_P` metody.</span><span class="sxs-lookup"><span data-stu-id="56f2d-561">a class `A` defines a read-only property `P`, thus reserving signatures for `get_P` and `set_P` methods.</span></span> <span data-ttu-id="56f2d-562">Třída `B` je odvozena z `A` a skryje obě tyto vyhrazená podpisy.</span><span class="sxs-lookup"><span data-stu-id="56f2d-562">A class `B` derives from `A` and hides both of these reserved signatures.</span></span> <span data-ttu-id="56f2d-563">Vzorové postupy výstupu:</span><span class="sxs-lookup"><span data-stu-id="56f2d-563">The example produces the output:</span></span>
```
123
123
456
```

#### <a name="member-names-reserved-for-events"></a><span data-ttu-id="56f2d-564">Názvy členů vyhrazené pro události</span><span class="sxs-lookup"><span data-stu-id="56f2d-564">Member names reserved for events</span></span>

<span data-ttu-id="56f2d-565">Pro událost `E` ([události](classes.md#events)) typu delegáta `T`, následující podpisy jsou vyhrazené:</span><span class="sxs-lookup"><span data-stu-id="56f2d-565">For an event `E` ([Events](classes.md#events)) of delegate type `T`, the following signatures are reserved:</span></span>
```csharp
void add_E(T handler);
void remove_E(T handler);
```

#### <a name="member-names-reserved-for-indexers"></a><span data-ttu-id="56f2d-566">Názvy členů vyhrazené pro indexery</span><span class="sxs-lookup"><span data-stu-id="56f2d-566">Member names reserved for indexers</span></span>

<span data-ttu-id="56f2d-567">Pro indexer ([indexery](classes.md#indexers)) typu `T` se seznamem parametrů `L`, následující podpisy jsou vyhrazené:</span><span class="sxs-lookup"><span data-stu-id="56f2d-567">For an indexer ([Indexers](classes.md#indexers)) of type `T` with parameter-list `L`, the following signatures are reserved:</span></span>
```csharp
T get_Item(L);
void set_Item(L, T value);
```

<span data-ttu-id="56f2d-568">Obě podpisy jsou vyhrazené, i v případě indexeru je jen pro čtení nebo jen pro zápis.</span><span class="sxs-lookup"><span data-stu-id="56f2d-568">Both signatures are reserved, even if the indexer is read-only or write-only.</span></span>

<span data-ttu-id="56f2d-569">Kromě názvu členu `Item` je vyhrazená.</span><span class="sxs-lookup"><span data-stu-id="56f2d-569">Furthermore the member name `Item` is reserved.</span></span>

#### <a name="member-names-reserved-for-destructors"></a><span data-ttu-id="56f2d-570">Názvy členů vyhrazené pro destruktory</span><span class="sxs-lookup"><span data-stu-id="56f2d-570">Member names reserved for destructors</span></span>

<span data-ttu-id="56f2d-571">Pro třídu obsahující destruktor ([destruktory](classes.md#destructors)), je vyhrazený následující podpis:</span><span class="sxs-lookup"><span data-stu-id="56f2d-571">For a class containing a destructor ([Destructors](classes.md#destructors)), the following signature is reserved:</span></span>
```csharp
void Finalize();
```

## <a name="constants"></a><span data-ttu-id="56f2d-572">Konstanty</span><span class="sxs-lookup"><span data-stu-id="56f2d-572">Constants</span></span>

<span data-ttu-id="56f2d-573">A ***konstantní*** je členem třídy reprezentující konstantní hodnotu: hodnotu, která lze vypočítat v době kompilace.</span><span class="sxs-lookup"><span data-stu-id="56f2d-573">A ***constant*** is a class member that represents a constant value: a value that can be computed at compile-time.</span></span> <span data-ttu-id="56f2d-574">A *constant_declaration* představuje jeden nebo více konstant daného typu.</span><span class="sxs-lookup"><span data-stu-id="56f2d-574">A *constant_declaration* introduces one or more constants of a given type.</span></span>

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

<span data-ttu-id="56f2d-575">A *constant_declaration* může zahrnovat sadu *atributy* ([atributy](attributes.md)), `new` modifikátor ([new – modifikátor](classes.md#the-new-modifier)), a platnou kombinaci čtyři přístupu modifikátory přístupu ([modifikátorů přístupu](classes.md#access-modifiers)).</span><span class="sxs-lookup"><span data-stu-id="56f2d-575">A *constant_declaration* may include a set of *attributes* ([Attributes](attributes.md)), a `new` modifier ([The new modifier](classes.md#the-new-modifier)), and a valid combination of the four access modifiers ([Access modifiers](classes.md#access-modifiers)).</span></span> <span data-ttu-id="56f2d-576">Atributy a modifikátory platí pro všechny členy deklarované pomocí *constant_declaration*.</span><span class="sxs-lookup"><span data-stu-id="56f2d-576">The attributes and modifiers apply to all of the members declared by the *constant_declaration*.</span></span> <span data-ttu-id="56f2d-577">I když konstanty jsou považovány za statické členy *constant_declaration* ani vyžaduje ani umožňuje `static` modifikátor.</span><span class="sxs-lookup"><span data-stu-id="56f2d-577">Even though constants are considered static members, a *constant_declaration* neither requires nor allows a `static` modifier.</span></span> <span data-ttu-id="56f2d-578">Jedná se o chybu pro stejný modifikátor se zobrazí více než jednou v deklarace konstanty.</span><span class="sxs-lookup"><span data-stu-id="56f2d-578">It is an error for the same modifier to appear multiple times in a constant declaration.</span></span>

<span data-ttu-id="56f2d-579">*Typ* z *constant_declaration* Určuje typ členů zavedeným deklarací.</span><span class="sxs-lookup"><span data-stu-id="56f2d-579">The *type* of a *constant_declaration* specifies the type of the members introduced by the declaration.</span></span> <span data-ttu-id="56f2d-580">Typ následuje seznam *constant_declarator*s, z nichž každý představuje nového člena.</span><span class="sxs-lookup"><span data-stu-id="56f2d-580">The type is followed by a list of *constant_declarator*s, each of which introduces a new member.</span></span> <span data-ttu-id="56f2d-581">A *constant_declarator* se skládá ze *identifikátor* , který pojmenovává člen, za nímž následuje "`=`" token a po něm *constant_expression* ([ Výrazy konstant](expressions.md#constant-expressions)), který obsahuje hodnotu člena.</span><span class="sxs-lookup"><span data-stu-id="56f2d-581">A *constant_declarator* consists of an *identifier* that names the member, followed by an "`=`" token, followed by a *constant_expression* ([Constant expressions](expressions.md#constant-expressions)) that gives the value of the member.</span></span>

<span data-ttu-id="56f2d-582">*Typ* podle konstanty musí být deklarace `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, `decimal`, `bool`, `string`, *enum_type*, nebo *reference_type*.</span><span class="sxs-lookup"><span data-stu-id="56f2d-582">The *type* specified in a constant declaration must be `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, `decimal`, `bool`, `string`, an *enum_type*, or a *reference_type*.</span></span> <span data-ttu-id="56f2d-583">Každý *constant_expression* musí poskytovat hodnotu cílového typu nebo typu, který lze převést na typ cíle podle implicitní převod ([implicitních převodů](conversions.md#implicit-conversions)).</span><span class="sxs-lookup"><span data-stu-id="56f2d-583">Each *constant_expression* must yield a value of the target type or of a type that can be converted to the target type by an implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)).</span></span>

<span data-ttu-id="56f2d-584">*Typ* konstanty musí být přinejmenším stejně dostupná jako konstanta samotného ([usnadnění omezení](basic-concepts.md#accessibility-constraints)).</span><span class="sxs-lookup"><span data-stu-id="56f2d-584">The *type* of a constant must be at least as accessible as the constant itself ([Accessibility constraints](basic-concepts.md#accessibility-constraints)).</span></span>

<span data-ttu-id="56f2d-585">Pomocí výrazu se získá hodnota konstanty *simple_name* ([jednoduché názvy](expressions.md#simple-names)) nebo *member_access* ([přístup ke členu](expressions.md#member-access)).</span><span class="sxs-lookup"><span data-stu-id="56f2d-585">The value of a constant is obtained in an expression using a *simple_name* ([Simple names](expressions.md#simple-names)) or a *member_access* ([Member access](expressions.md#member-access)).</span></span>

<span data-ttu-id="56f2d-586">Samotný konstantu mohl podílet na *constant_expression*.</span><span class="sxs-lookup"><span data-stu-id="56f2d-586">A constant can itself participate in a *constant_expression*.</span></span> <span data-ttu-id="56f2d-587">Proto lze v žádné konstrukci, která vyžaduje konstantu *constant_expression*.</span><span class="sxs-lookup"><span data-stu-id="56f2d-587">Thus, a constant may be used in any construct that requires a *constant_expression*.</span></span> <span data-ttu-id="56f2d-588">Příkladem takových konstrukce `case` popisky, `goto case` příkazů `enum` deklarací členů, atributy a jiné deklarace konstanty.</span><span class="sxs-lookup"><span data-stu-id="56f2d-588">Examples of such constructs include `case` labels, `goto case` statements, `enum` member declarations, attributes, and other constant declarations.</span></span>

<span data-ttu-id="56f2d-589">Jak je popsáno v [konstantní výrazy](expressions.md#constant-expressions), *constant_expression* je výraz, který může být plně vyhodnocen v době kompilace.</span><span class="sxs-lookup"><span data-stu-id="56f2d-589">As described in [Constant expressions](expressions.md#constant-expressions), a *constant_expression* is an expression that can be fully evaluated at compile-time.</span></span> <span data-ttu-id="56f2d-590">Od jediný způsob, jak vytvořit nenulovou hodnotu *reference_type* jiné než `string` použijte `new` operátorů a od `new` operátor není povolený v *constant_ výraz*, jediná možná hodnota konstanty *reference_type*s jiným než `string` je `null`.</span><span class="sxs-lookup"><span data-stu-id="56f2d-590">Since the only way to create a non-null value of a *reference_type* other than `string` is to apply the `new` operator, and since the `new` operator is not permitted in a *constant_expression*, the only possible value for constants of *reference_type*s other than `string` is `null`.</span></span>

<span data-ttu-id="56f2d-591">Symbolický název pro konstantní hodnotu, pokud se požaduje, ale když typ této hodnoty není povolen v deklarace konstanty nebo když hodnotu nelze vypočítat v době kompilace pomocí *constant_expression*, `readonly` pole () [Pole jen pro čtení](classes.md#readonly-fields)) mohou být použity místo toho.</span><span class="sxs-lookup"><span data-stu-id="56f2d-591">When a symbolic name for a constant value is desired, but when the type of that value is not permitted in a constant declaration, or when the value cannot be computed at compile-time by a *constant_expression*, a `readonly` field ([Readonly fields](classes.md#readonly-fields)) may be used instead.</span></span>

<span data-ttu-id="56f2d-592">Je ekvivalentní více deklarací jedné konstanty se stejnými atributy, modifikátory a typ deklarace konstanty, který deklaruje více konstant.</span><span class="sxs-lookup"><span data-stu-id="56f2d-592">A constant declaration that declares multiple constants is equivalent to multiple declarations of single constants with the same attributes, modifiers, and type.</span></span> <span data-ttu-id="56f2d-593">Příklad</span><span class="sxs-lookup"><span data-stu-id="56f2d-593">For example</span></span>
```csharp
class A
{
    public const double X = 1.0, Y = 2.0, Z = 3.0;
}
```
<span data-ttu-id="56f2d-594">je ekvivalentem</span><span class="sxs-lookup"><span data-stu-id="56f2d-594">is equivalent to</span></span>
```csharp
class A
{
    public const double X = 1.0;
    public const double Y = 2.0;
    public const double Z = 3.0;
}
```

<span data-ttu-id="56f2d-595">Konstanty jsou povolené závislé na dalších konstant ve stejném programu, za předpokladu, závislosti nejsou cyklické povahy.</span><span class="sxs-lookup"><span data-stu-id="56f2d-595">Constants are permitted to depend on other constants within the same program as long as the dependencies are not of a circular nature.</span></span> <span data-ttu-id="56f2d-596">Kompilátor automaticky uspořádá k vyhodnocení konstantní deklarace ve správném pořadí.</span><span class="sxs-lookup"><span data-stu-id="56f2d-596">The compiler automatically arranges to evaluate the constant declarations in the appropriate order.</span></span> <span data-ttu-id="56f2d-597">V příkladu</span><span class="sxs-lookup"><span data-stu-id="56f2d-597">In the example</span></span>
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
<span data-ttu-id="56f2d-598">Kompilátor nejprve vyhodnotí `A.Y`, pak vyhodnotí `B.Z`a nakonec vyhodnotí `A.X`, vytváření hodnoty `10`, `11`, a `12`.</span><span class="sxs-lookup"><span data-stu-id="56f2d-598">the compiler first evaluates `A.Y`, then evaluates `B.Z`, and finally evaluates `A.X`, producing the values `10`, `11`, and `12`.</span></span> <span data-ttu-id="56f2d-599">Deklarace konstanty může záviset na konstanty z jiných aplikací, ale tyto závislosti jsou možné pouze v jednom směru.</span><span class="sxs-lookup"><span data-stu-id="56f2d-599">Constant declarations may depend on constants from other programs, but such dependencies are only possible in one direction.</span></span> <span data-ttu-id="56f2d-600">Odkazující na výše uvedeném příkladu, pokud `A` a `B` byly deklarovány v samostatných programech, by bylo možné pro `A.X` závisí na `B.Z`, ale `B.Z` může potom závisí nejsou současně `A.Y`.</span><span class="sxs-lookup"><span data-stu-id="56f2d-600">Referring to the example above, if `A` and `B` were declared in separate programs, it would be possible for `A.X` to depend on `B.Z`, but `B.Z` could then not simultaneously depend on `A.Y`.</span></span>

## <a name="fields"></a><span data-ttu-id="56f2d-601">Pole</span><span class="sxs-lookup"><span data-stu-id="56f2d-601">Fields</span></span>

<span data-ttu-id="56f2d-602">A ***pole*** je člen, který představuje proměnnou přidruženou k objektu nebo třídě.</span><span class="sxs-lookup"><span data-stu-id="56f2d-602">A ***field*** is a member that represents a variable associated with an object or class.</span></span> <span data-ttu-id="56f2d-603">A *field_declaration* představuje nejméně jedno pole daného typu.</span><span class="sxs-lookup"><span data-stu-id="56f2d-603">A *field_declaration* introduces one or more fields of a given type.</span></span>

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

<span data-ttu-id="56f2d-604">A *field_declaration* může zahrnovat sadu *atributy* ([atributy](attributes.md)), `new` modifikátor ([new – modifikátor](classes.md#the-new-modifier)), platnou kombinaci čtyři přístupu modifikátory přístupu ([modifikátorů přístupu](classes.md#access-modifiers)) a `static` modifikátor ([statické a instance pole](classes.md#static-and-instance-fields)).</span><span class="sxs-lookup"><span data-stu-id="56f2d-604">A *field_declaration* may include a set of *attributes* ([Attributes](attributes.md)), a `new` modifier ([The new modifier](classes.md#the-new-modifier)), a valid combination of the four access modifiers ([Access modifiers](classes.md#access-modifiers)), and a `static` modifier ([Static and instance fields](classes.md#static-and-instance-fields)).</span></span> <span data-ttu-id="56f2d-605">Kromě toho *field_declaration* mohou zahrnovat `readonly` modifikátor ([pole jen pro čtení](classes.md#readonly-fields)) nebo `volatile` modifikátor ([pole s modifikátorem Volatile](classes.md#volatile-fields)), ale ne obojí.</span><span class="sxs-lookup"><span data-stu-id="56f2d-605">In addition, a *field_declaration* may include a `readonly` modifier ([Readonly fields](classes.md#readonly-fields)) or a `volatile` modifier ([Volatile fields](classes.md#volatile-fields)) but not both.</span></span> <span data-ttu-id="56f2d-606">Atributy a modifikátory platí pro všechny členy deklarované pomocí *field_declaration*.</span><span class="sxs-lookup"><span data-stu-id="56f2d-606">The attributes and modifiers apply to all of the members declared by the *field_declaration*.</span></span> <span data-ttu-id="56f2d-607">Jedná se o chybu pro stejný modifikátor objevit více než jednou v deklaraci pole.</span><span class="sxs-lookup"><span data-stu-id="56f2d-607">It is an error for the same modifier to appear multiple times in a field declaration.</span></span>

<span data-ttu-id="56f2d-608">*Typ* z *field_declaration* Určuje typ členů zavedeným deklarací.</span><span class="sxs-lookup"><span data-stu-id="56f2d-608">The *type* of a *field_declaration* specifies the type of the members introduced by the declaration.</span></span> <span data-ttu-id="56f2d-609">Typ následuje seznam *variable_declarator*s, z nichž každý představuje nového člena.</span><span class="sxs-lookup"><span data-stu-id="56f2d-609">The type is followed by a list of *variable_declarator*s, each of which introduces a new member.</span></span> <span data-ttu-id="56f2d-610">A *variable_declarator* se skládá ze *identifikátor* názvů, který tohoto člena, může volitelně následovat "`=`" token a *variable_initializer* ([ Proměnné inicializátory](classes.md#variable-initializers)), která obsahuje počáteční hodnotu daného člena.</span><span class="sxs-lookup"><span data-stu-id="56f2d-610">A *variable_declarator* consists of an *identifier* that names that member, optionally followed by an "`=`" token and a *variable_initializer* ([Variable initializers](classes.md#variable-initializers)) that gives the initial value of that member.</span></span>

<span data-ttu-id="56f2d-611">*Typ* pole musí být přinejmenším stejně dostupná jako samotného pole ([usnadnění omezení](basic-concepts.md#accessibility-constraints)).</span><span class="sxs-lookup"><span data-stu-id="56f2d-611">The *type* of a field must be at least as accessible as the field itself ([Accessibility constraints](basic-concepts.md#accessibility-constraints)).</span></span>

<span data-ttu-id="56f2d-612">Hodnota pole se získá ve výrazu pomocí *simple_name* ([jednoduché názvy](expressions.md#simple-names)) nebo *member_access* ([přístup ke členu](expressions.md#member-access)).</span><span class="sxs-lookup"><span data-stu-id="56f2d-612">The value of a field is obtained in an expression using a *simple_name* ([Simple names](expressions.md#simple-names)) or a *member_access* ([Member access](expressions.md#member-access)).</span></span> <span data-ttu-id="56f2d-613">Hodnota pole není jen pro čtení je upravit pomocí *přiřazení* ([operátory přiřazení](expressions.md#assignment-operators)).</span><span class="sxs-lookup"><span data-stu-id="56f2d-613">The value of a non-readonly field is modified using an *assignment* ([Assignment operators](expressions.md#assignment-operators)).</span></span> <span data-ttu-id="56f2d-614">Hodnota pole není jen pro čtení může být získat a upravit pomocí zvýšení příponového operátora i operátory snížení ([Příponové operátory Inkrementace a dekrementace operátory](expressions.md#postfix-increment-and-decrement-operators)) a předpony Inkrementace a dekrementace operátory ([předpona operátory přírůstku a snížení](expressions.md#prefix-increment-and-decrement-operators)).</span><span class="sxs-lookup"><span data-stu-id="56f2d-614">The value of a non-readonly field can be both obtained and modified using postfix increment and decrement operators ([Postfix increment and decrement operators](expressions.md#postfix-increment-and-decrement-operators)) and prefix increment and decrement operators ([Prefix increment and decrement operators](expressions.md#prefix-increment-and-decrement-operators)).</span></span>

<span data-ttu-id="56f2d-615">Deklarace pole, která deklaruje několik polí je ekvivalentní více deklarací z jednoho pole se stejnými atributy, modifikátory a typu.</span><span class="sxs-lookup"><span data-stu-id="56f2d-615">A field declaration that declares multiple fields is equivalent to multiple declarations of single fields with the same attributes, modifiers, and type.</span></span> <span data-ttu-id="56f2d-616">Příklad</span><span class="sxs-lookup"><span data-stu-id="56f2d-616">For example</span></span>
```csharp
class A
{
    public static int X = 1, Y, Z = 100;
}
```
<span data-ttu-id="56f2d-617">je ekvivalentem</span><span class="sxs-lookup"><span data-stu-id="56f2d-617">is equivalent to</span></span>
```csharp
class A
{
    public static int X = 1;
    public static int Y;
    public static int Z = 100;
}
```

### <a name="static-and-instance-fields"></a><span data-ttu-id="56f2d-618">Statické a instance pole</span><span class="sxs-lookup"><span data-stu-id="56f2d-618">Static and instance fields</span></span>

<span data-ttu-id="56f2d-619">Při deklaraci pole zahrnuje `static` modifikátor, polím zavedeným deklarací jsou ***statická pole***.</span><span class="sxs-lookup"><span data-stu-id="56f2d-619">When a field declaration includes a `static` modifier, the fields introduced by the declaration are ***static fields***.</span></span> <span data-ttu-id="56f2d-620">Pokud ne `static` Modifikátor je k dispozici, jsou polím zavedeným deklarací ***instance pole***.</span><span class="sxs-lookup"><span data-stu-id="56f2d-620">When no `static` modifier is present, the fields introduced by the declaration are ***instance fields***.</span></span> <span data-ttu-id="56f2d-621">Statická pole a pole instancí jsou dvě z několika druhů proměnné ([proměnné](variables.md)) podporují C# a v některých případech se označují jako ***statické proměnné*** a ***instance proměnné*** v uvedeném pořadí.</span><span class="sxs-lookup"><span data-stu-id="56f2d-621">Static fields and instance fields are two of the several kinds of variables ([Variables](variables.md)) supported by C#, and at times they are referred to as ***static variables*** and ***instance variables***, respectively.</span></span>

<span data-ttu-id="56f2d-622">Statické pole není součástí konkrétní instanci; Místo toho se sdílí mezi všechny instance uzavřených typu ([otevřené a uzavřené typy](types.md#open-and-closed-types)).</span><span class="sxs-lookup"><span data-stu-id="56f2d-622">A static field is not part of a specific instance; instead, it is shared amongst all instances of a closed type ([Open and closed types](types.md#open-and-closed-types)).</span></span> <span data-ttu-id="56f2d-623">Bez ohledu na to, kolik instancí typu uzavřené třídy jsou vytvořeny je pouze někdy jednu kopii statické pole pro doménu přidružené aplikace.</span><span class="sxs-lookup"><span data-stu-id="56f2d-623">No matter how many instances of a closed class type are created, there is only ever one copy of a static field for the associated application domain.</span></span>

<span data-ttu-id="56f2d-624">Příklad:</span><span class="sxs-lookup"><span data-stu-id="56f2d-624">For example:</span></span>
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

<span data-ttu-id="56f2d-625">Pole instance patří k instanci.</span><span class="sxs-lookup"><span data-stu-id="56f2d-625">An instance field belongs to an instance.</span></span> <span data-ttu-id="56f2d-626">Konkrétně každá instance třídy obsahuje samostatnou sadu všechna pole instancí této třídy.</span><span class="sxs-lookup"><span data-stu-id="56f2d-626">Specifically, every instance of a class contains a separate set of all the instance fields of that class.</span></span>

<span data-ttu-id="56f2d-627">Když odkazuje pole *member_access* ([přístup ke členu](expressions.md#member-access)) formuláře `E.M`, pokud `M` je statická pole, `E` musí označují typ obsahující `M` a pokud `M` je polem instance E musí označit instanci typu, který obsahuje `M`.</span><span class="sxs-lookup"><span data-stu-id="56f2d-627">When a field is referenced in a *member_access* ([Member access](expressions.md#member-access)) of the form `E.M`, if `M` is a static field, `E` must denote a type containing `M`, and if `M` is an instance field, E must denote an instance of a type containing `M`.</span></span>

<span data-ttu-id="56f2d-628">Rozdíly mezi statické a členy instance jsou popsány dále v [statické a instance členy](classes.md#static-and-instance-members).</span><span class="sxs-lookup"><span data-stu-id="56f2d-628">The differences between static and instance members are discussed further in [Static and instance members](classes.md#static-and-instance-members).</span></span>

### <a name="readonly-fields"></a><span data-ttu-id="56f2d-629">Pole určené jen pro čtení</span><span class="sxs-lookup"><span data-stu-id="56f2d-629">Readonly fields</span></span>

<span data-ttu-id="56f2d-630">Když *field_declaration* zahrnuje `readonly` modifikátor, polím zavedeným deklarací se ***pole jen pro čtení***.</span><span class="sxs-lookup"><span data-stu-id="56f2d-630">When a *field_declaration* includes a `readonly` modifier, the fields introduced by the declaration are ***readonly fields***.</span></span> <span data-ttu-id="56f2d-631">Přímá přiřazení k polím jen pro čtení může dojít pouze jako součást tohoto prohlášení nebo v konstruktoru instance nebo statický konstruktor ve stejné třídě.</span><span class="sxs-lookup"><span data-stu-id="56f2d-631">Direct assignments to readonly fields can only occur as part of that declaration or in an instance constructor or static constructor in the same class.</span></span> <span data-ttu-id="56f2d-632">(Pole jen pro čtení může být přiřazeno více než jednou v těchto kontextech.) Konkrétně přímého přiřazení `readonly` pole jsou povoleny pouze v následujících kontextů:</span><span class="sxs-lookup"><span data-stu-id="56f2d-632">(A readonly field can be assigned to multiple times in these contexts.) Specifically, direct assignments to a `readonly` field are permitted only in the following contexts:</span></span>

*  <span data-ttu-id="56f2d-633">V *variable_declarator* , který představuje pole (včetně *variable_initializer* v deklaraci).</span><span class="sxs-lookup"><span data-stu-id="56f2d-633">In the *variable_declarator* that introduces the field (by including a *variable_initializer* in the declaration).</span></span>
*  <span data-ttu-id="56f2d-634">Pro pole instance, v konstruktorech instancí třídy, která obsahuje deklaraci pole; pro statické pole v statický konstruktor třídy, která obsahuje deklaraci pole.</span><span class="sxs-lookup"><span data-stu-id="56f2d-634">For an instance field, in the instance constructors of the class that contains the field declaration; for a static field, in the static constructor of the class that contains the field declaration.</span></span> <span data-ttu-id="56f2d-635">Toto jsou také pouze kontextech, ve kterých je možné předat `readonly` pole jako `out` nebo `ref` parametru.</span><span class="sxs-lookup"><span data-stu-id="56f2d-635">These are also the only contexts in which it is valid to pass a `readonly` field as an `out` or `ref` parameter.</span></span>

<span data-ttu-id="56f2d-636">Pokus o přiřazení `readonly` pole nebo předat ji jako `out` nebo `ref` parametr v libovolném kontextu je chyba kompilace.</span><span class="sxs-lookup"><span data-stu-id="56f2d-636">Attempting to assign to a `readonly` field or pass it as an `out` or `ref` parameter in any other context is a compile-time error.</span></span>

#### <a name="using-static-readonly-fields-for-constants"></a><span data-ttu-id="56f2d-637">Pomocí statického pole pro konstanty</span><span class="sxs-lookup"><span data-stu-id="56f2d-637">Using static readonly fields for constants</span></span>

<span data-ttu-id="56f2d-638">A `static readonly` pole je užitečné, když je žádoucí symbolický název pro konstantní hodnotu, ale když typ hodnoty není povolen v `const` prohlášení, nebo když hodnotu nelze vypočítat v době kompilace.</span><span class="sxs-lookup"><span data-stu-id="56f2d-638">A `static readonly` field is useful when a symbolic name for a constant value is desired, but when the type of the value is not permitted in a `const` declaration, or when the value cannot be computed at compile-time.</span></span> <span data-ttu-id="56f2d-639">V příkladu</span><span class="sxs-lookup"><span data-stu-id="56f2d-639">In the example</span></span>
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
<span data-ttu-id="56f2d-640">`Black`, `White`, `Red`, `Green`, a `Blue` členy nelze deklarovat jako `const` členy vzhledem k tomu, že jejich hodnoty nelze vypočítat v době kompilace.</span><span class="sxs-lookup"><span data-stu-id="56f2d-640">the `Black`, `White`, `Red`, `Green`, and `Blue` members cannot be declared as `const` members because their values cannot be computed at compile-time.</span></span> <span data-ttu-id="56f2d-641">Je však deklarovat `static readonly` místo toho má mnohem stejný účinek.</span><span class="sxs-lookup"><span data-stu-id="56f2d-641">However, declaring them `static readonly` instead has much the same effect.</span></span>

#### <a name="versioning-of-constants-and-static-readonly-fields"></a><span data-ttu-id="56f2d-642">Správa verzí statického polí a konstanty</span><span class="sxs-lookup"><span data-stu-id="56f2d-642">Versioning of constants and static readonly fields</span></span>

<span data-ttu-id="56f2d-643">Konstanty a pole jen pro čtení mají sémantiku jiný binární správy verzí.</span><span class="sxs-lookup"><span data-stu-id="56f2d-643">Constants and readonly fields have different binary versioning semantics.</span></span> <span data-ttu-id="56f2d-644">Pokud výraz odkazuje na konstantu, je hodnotou konstanty získané v době kompilace, ale pokud výraz odkazuje na pole jen pro čtení, hodnota pole není dosaženo až za běhu.</span><span class="sxs-lookup"><span data-stu-id="56f2d-644">When an expression references a constant, the value of the constant is obtained at compile-time, but when an expression references a readonly field, the value of the field is not obtained until run-time.</span></span> <span data-ttu-id="56f2d-645">Vezměte v úvahu aplikace, která se skládá ze dvou samostatných programech:</span><span class="sxs-lookup"><span data-stu-id="56f2d-645">Consider an application that consists of two separate programs:</span></span>
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

<span data-ttu-id="56f2d-646">`Program1` a `Program2` obory názvů označení dva programy, které jsou kompilovány samostatně.</span><span class="sxs-lookup"><span data-stu-id="56f2d-646">The `Program1` and `Program2` namespaces denote two programs that are compiled separately.</span></span> <span data-ttu-id="56f2d-647">Protože `Program1.Utils.X` je deklarován jako statického pole hodnotu výstupu podle `Console.WriteLine` příkaz není znám v době kompilace, ale raději získali v době běhu.</span><span class="sxs-lookup"><span data-stu-id="56f2d-647">Because `Program1.Utils.X` is declared as a static readonly field, the value output by the `Console.WriteLine` statement is not known at compile-time, but rather is obtained at run-time.</span></span> <span data-ttu-id="56f2d-648">Proto pokud hodnota `X` se změní a `Program1` přepsán, `Console.WriteLine` příkaz bude výstup i když hodnota `Program2` není znovu zkompilován.</span><span class="sxs-lookup"><span data-stu-id="56f2d-648">Thus, if the value of `X` is changed and `Program1` is recompiled, the `Console.WriteLine` statement will output the new value even if `Program2` isn't recompiled.</span></span> <span data-ttu-id="56f2d-649">Ale měli `X` byla konstantu, hodnota `X` by byly získány v době `Program2` byl zkompilován a zůstane nebudou výpadkem ovlivněny změnami `Program1` dokud `Program2` přepsán.</span><span class="sxs-lookup"><span data-stu-id="56f2d-649">However, had `X` been a constant, the value of `X` would have been obtained at the time `Program2` was compiled, and would remain unaffected by changes in `Program1` until `Program2` is recompiled.</span></span>

### <a name="volatile-fields"></a><span data-ttu-id="56f2d-650">Pole s modifikátorem volatile</span><span class="sxs-lookup"><span data-stu-id="56f2d-650">Volatile fields</span></span>

<span data-ttu-id="56f2d-651">Když *field_declaration* zahrnuje `volatile` modifikátor, polím zavedeným tuto deklaraci jsou ***pole s modifikátorem volatile***.</span><span class="sxs-lookup"><span data-stu-id="56f2d-651">When a *field_declaration* includes a `volatile` modifier, the fields introduced by that declaration are ***volatile fields***.</span></span>

<span data-ttu-id="56f2d-652">Pro pole stálé techniky optimalizace, které Změna pořadí pokyny může vést k neočekávané a nepředvídatelné výsledky ve vícevláknových programů, které přístup k polím bez synchronizace, jako je například poskytuje *lock_statement*  ([Příkaz lock](statements.md#the-lock-statement)).</span><span class="sxs-lookup"><span data-stu-id="56f2d-652">For non-volatile fields, optimization techniques that reorder instructions can lead to unexpected and unpredictable results in multi-threaded programs that access fields without synchronization such as that provided by the *lock_statement* ([The lock statement](statements.md#the-lock-statement)).</span></span> <span data-ttu-id="56f2d-653">Tyto optimalizace lze provést pomocí kompilátoru, za běhu systému nebo hardwarem.</span><span class="sxs-lookup"><span data-stu-id="56f2d-653">These optimizations can be performed by the compiler, by the run-time system, or by hardware.</span></span> <span data-ttu-id="56f2d-654">Pro pole s modifikátorem volatile jsou omezeny takový způsob optimalizace:</span><span class="sxs-lookup"><span data-stu-id="56f2d-654">For volatile fields, such reordering optimizations are restricted:</span></span>

*  <span data-ttu-id="56f2d-655">Čtení pole s modifikátorem volatile se volá ***volatile čtení***.</span><span class="sxs-lookup"><span data-stu-id="56f2d-655">A read of a volatile field is called a ***volatile read***.</span></span> <span data-ttu-id="56f2d-656">Volatile pro čtení má "získat sémantika"; To znamená, že je zaručeno, že před všechny odkazy na paměti, ke kterým dochází za něj postupně instrukce.</span><span class="sxs-lookup"><span data-stu-id="56f2d-656">A volatile read has "acquire semantics"; that is, it is guaranteed to occur prior to any references to memory that occur after it in the instruction sequence.</span></span>
*  <span data-ttu-id="56f2d-657">Zápis pole s modifikátorem volatile se volá ***volatile zápisu***.</span><span class="sxs-lookup"><span data-stu-id="56f2d-657">A write of a volatile field is called a ***volatile write***.</span></span> <span data-ttu-id="56f2d-658">Volatile zápisu je "vydané verze sémantika"; To znamená, že je zaručeno, že tomu může dojít po všechny odkazy na paměť než instrukce zápis v pořadí instrukce.</span><span class="sxs-lookup"><span data-stu-id="56f2d-658">A volatile write has "release semantics"; that is, it is guaranteed to happen after any memory references prior to the write instruction in the instruction sequence.</span></span>

<span data-ttu-id="56f2d-659">Tato omezení Ujistěte se, že všechna vlákna budou sledovat volatile zápisy provádí ostatní vlákna v pořadí, ve kterém byly provedeny.</span><span class="sxs-lookup"><span data-stu-id="56f2d-659">These restrictions ensure that all threads will observe volatile writes performed by any other thread in the order in which they were performed.</span></span> <span data-ttu-id="56f2d-660">Vyhovující implementace není potřeba poskytovat, jeden celkový řazení volatile zápisů pohledu ze všech vláken, která.</span><span class="sxs-lookup"><span data-stu-id="56f2d-660">A conforming implementation is not required to provide a single total ordering of volatile writes as seen from all threads of execution.</span></span> <span data-ttu-id="56f2d-661">Typ pole s modifikátorem volatile musí být jeden z následujících akcí:</span><span class="sxs-lookup"><span data-stu-id="56f2d-661">The type of a volatile field must be one of the following:</span></span>

*  <span data-ttu-id="56f2d-662">A *reference_type*.</span><span class="sxs-lookup"><span data-stu-id="56f2d-662">A *reference_type*.</span></span>
*  <span data-ttu-id="56f2d-663">Typ `byte`, `sbyte`, `short`, `ushort`, `int`, `uint`, `char`, `float`, `bool`, `System.IntPtr`, nebo` System.UIntPtr`.</span><span class="sxs-lookup"><span data-stu-id="56f2d-663">The type `byte`, `sbyte`, `short`, `ushort`, `int`, `uint`, `char`, `float`, `bool`, `System.IntPtr`, or` System.UIntPtr`.</span></span>
*  <span data-ttu-id="56f2d-664">*Enum_type* základního typu výčtu `byte`, `sbyte`, `short`, `ushort`, `int`, nebo `uint`.</span><span class="sxs-lookup"><span data-stu-id="56f2d-664">An *enum_type* having an enum base type of `byte`, `sbyte`, `short`, `ushort`, `int`, or `uint`.</span></span>

<span data-ttu-id="56f2d-665">V příkladu</span><span class="sxs-lookup"><span data-stu-id="56f2d-665">The example</span></span>
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
<span data-ttu-id="56f2d-666">Vytvoří výstup:</span><span class="sxs-lookup"><span data-stu-id="56f2d-666">produces the output:</span></span>
```
result = 143
```

<span data-ttu-id="56f2d-667">V tomto příkladu metoda `Main` začíná nové vlákno, na kterém běží metodu `Thread2`.</span><span class="sxs-lookup"><span data-stu-id="56f2d-667">In this example, the method `Main` starts a new thread that runs the method `Thread2`.</span></span> <span data-ttu-id="56f2d-668">Tato metoda ukládá hodnotu do pole stálé s názvem `result`, pak uloží `true` v pole s modifikátorem volatile `finished`.</span><span class="sxs-lookup"><span data-stu-id="56f2d-668">This method stores a value into a non-volatile field called `result`, then stores `true` in the volatile field `finished`.</span></span> <span data-ttu-id="56f2d-669">Hlavní podproces čeká pole `finished` nastavit `true`, pak načte pole `result`.</span><span class="sxs-lookup"><span data-stu-id="56f2d-669">The main thread waits for the field `finished` to be set to `true`, then reads the field `result`.</span></span> <span data-ttu-id="56f2d-670">Protože `finished` byla deklarována `volatile`, hlavního vlákna musí načíst hodnotu `143` z pole `result`.</span><span class="sxs-lookup"><span data-stu-id="56f2d-670">Since `finished` has been declared `volatile`, the main thread must read the value `143` from the field `result`.</span></span> <span data-ttu-id="56f2d-671">Pokud pole `finished` kdyby byly deklarovány `volatile`, pak by přípustné úložiště `result` uvidí hlavního vlákna po úložiště `finished`a proto pro hlavní vlákno načíst hodnotu `0` z pole `result`.</span><span class="sxs-lookup"><span data-stu-id="56f2d-671">If the field `finished` had not been declared `volatile`, then it would be permissible for the store to `result` to be visible to the main thread after the store to `finished`, and hence for the main thread to read the value `0` from the field `result`.</span></span> <span data-ttu-id="56f2d-672">Deklarování `finished` jako `volatile` pole zabraňuje Tato nekonzistence.</span><span class="sxs-lookup"><span data-stu-id="56f2d-672">Declaring `finished` as a `volatile` field prevents any such inconsistency.</span></span>

### <a name="field-initialization"></a><span data-ttu-id="56f2d-673">Inicializace pole</span><span class="sxs-lookup"><span data-stu-id="56f2d-673">Field initialization</span></span>

<span data-ttu-id="56f2d-674">Počáteční hodnota pole, ať to statické pole nebo pole instance, je výchozí hodnota ([výchozí hodnoty](variables.md#default-values)) z typu pole.</span><span class="sxs-lookup"><span data-stu-id="56f2d-674">The initial value of a field, whether it be a static field or an instance field, is the default value ([Default values](variables.md#default-values)) of the field's type.</span></span> <span data-ttu-id="56f2d-675">Není možné předtím, než tato výchozí inicializace došlo k chybě, a pole proto nikdy "neinicializované" zachovávají hodnotu pole.</span><span class="sxs-lookup"><span data-stu-id="56f2d-675">It is not possible to observe the value of a field before this default initialization has occurred, and a field is thus never "uninitialized".</span></span> <span data-ttu-id="56f2d-676">V příkladu</span><span class="sxs-lookup"><span data-stu-id="56f2d-676">The example</span></span>
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
<span data-ttu-id="56f2d-677">Vytvoří výstup</span><span class="sxs-lookup"><span data-stu-id="56f2d-677">produces the output</span></span>
```
b = False, i = 0
```
<span data-ttu-id="56f2d-678">protože `b` a `i` obě automaticky inicializovány na výchozí hodnoty.</span><span class="sxs-lookup"><span data-stu-id="56f2d-678">because `b` and `i` are both automatically initialized to default values.</span></span>

### <a name="variable-initializers"></a><span data-ttu-id="56f2d-679">Inicializátory proměnné</span><span class="sxs-lookup"><span data-stu-id="56f2d-679">Variable initializers</span></span>

<span data-ttu-id="56f2d-680">Deklarace pole může obsahovat *variable_initializer*s.</span><span class="sxs-lookup"><span data-stu-id="56f2d-680">Field declarations may include *variable_initializer*s.</span></span> <span data-ttu-id="56f2d-681">Statická pole Proměnná inicializátory odpovídají přiřazovací příkazy, které jsou spouštěny během inicializace třídy.</span><span class="sxs-lookup"><span data-stu-id="56f2d-681">For static fields, variable initializers correspond to assignment statements that are executed during class initialization.</span></span> <span data-ttu-id="56f2d-682">Například pole, proměnná inicializátory odpovídají přiřazovací příkazy, které jsou spouštěny, když je vytvořena instance třídy.</span><span class="sxs-lookup"><span data-stu-id="56f2d-682">For instance fields, variable initializers correspond to assignment statements that are executed when an instance of the class is created.</span></span>

<span data-ttu-id="56f2d-683">V příkladu</span><span class="sxs-lookup"><span data-stu-id="56f2d-683">The example</span></span>
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
<span data-ttu-id="56f2d-684">Vytvoří výstup</span><span class="sxs-lookup"><span data-stu-id="56f2d-684">produces the output</span></span>
```
x = 1.4142135623731, i = 100, s = Hello
```
<span data-ttu-id="56f2d-685">protože přiřazení k `x` vyvolá se při spuštění statickými Inicializátory pole a přiřazení `i` a `s` dojít, když Inicializátory polí instance spustit.</span><span class="sxs-lookup"><span data-stu-id="56f2d-685">because an assignment to `x` occurs when static field initializers execute and assignments to `i` and `s` occur when the instance field initializers execute.</span></span>

<span data-ttu-id="56f2d-686">Výchozí hodnota inicializace je popsáno v [inicializace pole](classes.md#field-initialization) dochází u všech polí, včetně polí, které mají variabilní inicializátory.</span><span class="sxs-lookup"><span data-stu-id="56f2d-686">The default value initialization described in [Field initialization](classes.md#field-initialization) occurs for all fields, including fields that have variable initializers.</span></span> <span data-ttu-id="56f2d-687">Proto při inicializaci třídy všechny statické pole v dané třídě jsou nejprve inicializovat na výchozí hodnoty a pak statickými Inicializátory pole jsou provedeny v textové pořadí.</span><span class="sxs-lookup"><span data-stu-id="56f2d-687">Thus, when a class is initialized, all static fields in that class are first initialized to their default values, and then the static field initializers are executed in textual order.</span></span> <span data-ttu-id="56f2d-688">Podobně když je vytvořena instance třídy, všechna pole instancí v dané instanci jsou nejprve inicializovat na výchozí hodnoty a pak Inicializátory polí instance jsou provedeny v textové pořadí.</span><span class="sxs-lookup"><span data-stu-id="56f2d-688">Likewise, when an instance of a class is created, all instance fields in that instance are first initialized to their default values, and then the instance field initializers are executed in textual order.</span></span>

<span data-ttu-id="56f2d-689">Je možné pro statické pole s proměnnou inicializátory má být dodržen v jejich výchozí hodnoty stavu.</span><span class="sxs-lookup"><span data-stu-id="56f2d-689">It is possible for static fields with variable initializers to be observed in their default value state.</span></span> <span data-ttu-id="56f2d-690">Je to ale jak styl důrazně nedoporučuje.</span><span class="sxs-lookup"><span data-stu-id="56f2d-690">However, this is strongly discouraged as a matter of style.</span></span> <span data-ttu-id="56f2d-691">V příkladu</span><span class="sxs-lookup"><span data-stu-id="56f2d-691">The example</span></span>
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
<span data-ttu-id="56f2d-692">je třeba toto chování.</span><span class="sxs-lookup"><span data-stu-id="56f2d-692">exhibits this behavior.</span></span> <span data-ttu-id="56f2d-693">Bez ohledu na cyklické definice a b, program je platný.</span><span class="sxs-lookup"><span data-stu-id="56f2d-693">Despite the circular definitions of a and b, the program is valid.</span></span> <span data-ttu-id="56f2d-694">Výsledkem výstupu</span><span class="sxs-lookup"><span data-stu-id="56f2d-694">It results in the output</span></span>
```
a = 1, b = 2
```
<span data-ttu-id="56f2d-695">protože statická pole `a` a `b` jsou inicializovány na hodnotu `0` (výchozí hodnota pro `int`) předtím, než se spustí jejich inicializátory.</span><span class="sxs-lookup"><span data-stu-id="56f2d-695">because the static fields `a` and `b` are initialized to `0` (the default value for `int`) before their initializers are executed.</span></span> <span data-ttu-id="56f2d-696">Při inicializátor pro `a` spustí hodnotu `b` je nula a proto `a` je inicializován na `1`.</span><span class="sxs-lookup"><span data-stu-id="56f2d-696">When the initializer for `a` runs, the value of `b` is zero, and so `a` is initialized to `1`.</span></span> <span data-ttu-id="56f2d-697">Při inicializátor pro `b` spustí, hodnotu `a` již `1`a proto `b` je inicializován na `2`.</span><span class="sxs-lookup"><span data-stu-id="56f2d-697">When the initializer for `b` runs, the value of `a` is already `1`, and so `b` is initialized to `2`.</span></span>

#### <a name="static-field-initialization"></a><span data-ttu-id="56f2d-698">Inicializace statických polí</span><span class="sxs-lookup"><span data-stu-id="56f2d-698">Static field initialization</span></span>

<span data-ttu-id="56f2d-699">Inicializátory proměnné statického pole třídy odpovídají sekvenci úlohy, které jsou spouštěny v textové pořadí, v jakém jsou uvedeny v deklaraci třídy.</span><span class="sxs-lookup"><span data-stu-id="56f2d-699">The static field variable initializers of a class correspond to a sequence of assignments that are executed in the textual order in which they appear in the class declaration.</span></span> <span data-ttu-id="56f2d-700">Pokud statický konstruktor ([statické konstruktory](classes.md#static-constructors)) existuje ve třídě, provádění statickými Inicializátory pole nastane bezprostředně před spuštěním této statický konstruktor.</span><span class="sxs-lookup"><span data-stu-id="56f2d-700">If a static constructor ([Static constructors](classes.md#static-constructors)) exists in the class, execution of the static field initializers occurs immediately prior to executing that static constructor.</span></span> <span data-ttu-id="56f2d-701">V opačném případě statickými Inicializátory pole jsou spuštěny v závislý na implementaci dobu před prvním použitím statické pole této třídy.</span><span class="sxs-lookup"><span data-stu-id="56f2d-701">Otherwise, the static field initializers are executed at an implementation-dependent time prior to the first use of a static field of that class.</span></span> <span data-ttu-id="56f2d-702">V příkladu</span><span class="sxs-lookup"><span data-stu-id="56f2d-702">The example</span></span>
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
<span data-ttu-id="56f2d-703">může generovat buď výstup:</span><span class="sxs-lookup"><span data-stu-id="56f2d-703">might produce either the output:</span></span>
```
Init A
Init B
1 1
```
<span data-ttu-id="56f2d-704">nebo výstup:</span><span class="sxs-lookup"><span data-stu-id="56f2d-704">or the output:</span></span>
```
Init B
Init A
1 1
```
<span data-ttu-id="56f2d-705">protože provádění `X`společnosti inicializátor a `Y`společnosti inicializátor mohlo by dojít k buď popořadě; jsou pouze omezené před odkazy na tato pole.</span><span class="sxs-lookup"><span data-stu-id="56f2d-705">because the execution of `X`'s initializer and `Y`'s initializer could occur in either order; they are only constrained to occur before the references to those fields.</span></span> <span data-ttu-id="56f2d-706">Ale v tomto příkladu:</span><span class="sxs-lookup"><span data-stu-id="56f2d-706">However, in the example:</span></span>
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
<span data-ttu-id="56f2d-707">musí být výstup:</span><span class="sxs-lookup"><span data-stu-id="56f2d-707">the output must be:</span></span>
```
Init B
Init A
1 1
```
<span data-ttu-id="56f2d-708">protože pravidla pro při spuštění statické konstruktory (jak jsou definovány v [statické konstruktory](classes.md#static-constructors)) stanoví, že `B`společnosti statického konstruktoru (a proto `B`společnosti statickými Inicializátory pole) musí spustit před `A`na statický konstruktor a Inicializátory polí.</span><span class="sxs-lookup"><span data-stu-id="56f2d-708">because the rules for when static constructors execute (as defined in [Static constructors](classes.md#static-constructors)) provide that `B`'s static constructor (and hence `B`'s static field initializers) must run before `A`'s static constructor and field initializers.</span></span>

#### <a name="instance-field-initialization"></a><span data-ttu-id="56f2d-709">Inicializace pole instance</span><span class="sxs-lookup"><span data-stu-id="56f2d-709">Instance field initialization</span></span>

<span data-ttu-id="56f2d-710">Proměnné Inicializátory polí instance třídy, které odpovídají posloupnost přiřazení, která jsou spuštěna ihned po vstupu do některého z konstruktory instancí ([inicializátory konstruktoru](classes.md#constructor-initializers)) této třídy.</span><span class="sxs-lookup"><span data-stu-id="56f2d-710">The instance field variable initializers of a class correspond to a sequence of assignments that are executed immediately upon entry to any one of the instance constructors ([Constructor initializers](classes.md#constructor-initializers)) of that class.</span></span> <span data-ttu-id="56f2d-711">Proměnné inicializátory jsou provedeny v textové pořadí, v jakém jsou uvedeny v deklaraci třídy.</span><span class="sxs-lookup"><span data-stu-id="56f2d-711">The variable initializers are executed in the textual order in which they appear in the class declaration.</span></span> <span data-ttu-id="56f2d-712">Proces vytváření a Inicializace instance třídy je popsány dále v [Instance konstruktory](classes.md#instance-constructors).</span><span class="sxs-lookup"><span data-stu-id="56f2d-712">The class instance creation and initialization process is described further in [Instance constructors](classes.md#instance-constructors).</span></span>

<span data-ttu-id="56f2d-713">Proměnná inicializátor pro pole instance nemůže odkazovat na právě vytvořenou instanci.</span><span class="sxs-lookup"><span data-stu-id="56f2d-713">A variable initializer for an instance field cannot reference the instance being created.</span></span> <span data-ttu-id="56f2d-714">Proto je chyba kompilace odkazovat `this` v inicializátoru proměnné, protože je chyba kompilace pro inicializátoru proměnné tak, aby odkazovaly kterýkoli člen instance prostřednictvím *simple_name*.</span><span class="sxs-lookup"><span data-stu-id="56f2d-714">Thus, it is a compile-time error to reference `this` in a variable initializer, as it is a compile-time error for a variable initializer to reference any instance member through a *simple_name*.</span></span> <span data-ttu-id="56f2d-715">V příkladu</span><span class="sxs-lookup"><span data-stu-id="56f2d-715">In the example</span></span>
```csharp
class A
{
    int x = 1;
    int y = x + 1;        // Error, reference to instance member of this
}
```
<span data-ttu-id="56f2d-716">Proměnná inicializátor pro `y` výsledkem chyba kompilace, protože odkazuje na člen právě vytvořenou instanci.</span><span class="sxs-lookup"><span data-stu-id="56f2d-716">the variable initializer for `y` results in a compile-time error because it references a member of the instance being created.</span></span>

## <a name="methods"></a><span data-ttu-id="56f2d-717">Metody</span><span class="sxs-lookup"><span data-stu-id="56f2d-717">Methods</span></span>

<span data-ttu-id="56f2d-718">A ***metoda*** je člen, který implementuje výpočtu nebo akce, která může provádět k objektu nebo třídě.</span><span class="sxs-lookup"><span data-stu-id="56f2d-718">A ***method*** is a member that implements a computation or action that can be performed by an object or class.</span></span> <span data-ttu-id="56f2d-719">Metody jsou deklarovány pomocí *method_declaration*s:</span><span class="sxs-lookup"><span data-stu-id="56f2d-719">Methods are declared using *method_declaration*s:</span></span>

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

<span data-ttu-id="56f2d-720">A *method_declaration* může zahrnovat sadu *atributy* ([atributy](attributes.md)) a platnou kombinaci čtyři přístupu modifikátory přístupu ([modifikátorů přístupu ](classes.md#access-modifiers)), `new` ([new – modifikátor](classes.md#the-new-modifier)), `static` ([statická a instanci metody](classes.md#static-and-instance-methods)), `virtual` ([virtuální metody](classes.md#virtual-methods)), `override` ([Přepište metody](classes.md#override-methods)), `sealed` ([zapečetěné metody](classes.md#sealed-methods)), `abstract` ([abstraktní metody](classes.md#abstract-methods)), a `extern` ([Externí metody](classes.md#external-methods)) modifikátory.</span><span class="sxs-lookup"><span data-stu-id="56f2d-720">A *method_declaration* may include a set of *attributes* ([Attributes](attributes.md)) and a valid combination of the four access modifiers ([Access modifiers](classes.md#access-modifiers)), the `new` ([The new modifier](classes.md#the-new-modifier)),  `static` ([Static and instance methods](classes.md#static-and-instance-methods)), `virtual` ([Virtual methods](classes.md#virtual-methods)), `override` ([Override methods](classes.md#override-methods)), `sealed` ([Sealed methods](classes.md#sealed-methods)), `abstract` ([Abstract methods](classes.md#abstract-methods)), and `extern` ([External methods](classes.md#external-methods)) modifiers.</span></span>

<span data-ttu-id="56f2d-721">Deklarace má platnou kombinaci modifikátory, pokud jsou splněny všechny z následujících akcí:</span><span class="sxs-lookup"><span data-stu-id="56f2d-721">A declaration has a valid combination of modifiers if all of the following are true:</span></span>

*  <span data-ttu-id="56f2d-722">Deklarace obsahuje platnou kombinaci modifikátory přístupu ([modifikátorů přístupu](classes.md#access-modifiers)).</span><span class="sxs-lookup"><span data-stu-id="56f2d-722">The declaration includes a valid combination of access modifiers ([Access modifiers](classes.md#access-modifiers)).</span></span>
*  <span data-ttu-id="56f2d-723">Deklarace nezahrnuje stejný modifikátor více než jednou.</span><span class="sxs-lookup"><span data-stu-id="56f2d-723">The declaration does not include the same modifier multiple times.</span></span>
*  <span data-ttu-id="56f2d-724">Deklarace obsahuje jenom jedna z následujících parametrů: `static`, `virtual`, a `override`.</span><span class="sxs-lookup"><span data-stu-id="56f2d-724">The declaration includes at most one of the following modifiers: `static`, `virtual`, and `override`.</span></span>
*  <span data-ttu-id="56f2d-725">Deklarace obsahuje jenom jedna z následujících parametrů: `new` a `override`.</span><span class="sxs-lookup"><span data-stu-id="56f2d-725">The declaration includes at most one of the following modifiers: `new` and `override`.</span></span>
*  <span data-ttu-id="56f2d-726">Pokud deklarace obsahuje `abstract` modifikátoru deklarace a nesmí obsahovat žádný z následujících parametrů: `static`, `virtual`, `sealed` nebo `extern`.</span><span class="sxs-lookup"><span data-stu-id="56f2d-726">If the declaration includes the `abstract` modifier, then the declaration does not include any of the following modifiers: `static`, `virtual`, `sealed` or `extern`.</span></span>
*  <span data-ttu-id="56f2d-727">Pokud deklarace obsahuje `private` modifikátoru deklarace a nesmí obsahovat žádný z následujících parametrů: `virtual`, `override`, nebo `abstract`.</span><span class="sxs-lookup"><span data-stu-id="56f2d-727">If the declaration includes the `private` modifier, then the declaration does not include any of the following modifiers: `virtual`, `override`, or `abstract`.</span></span>
*  <span data-ttu-id="56f2d-728">Pokud deklarace obsahuje `sealed` modifikátoru deklarace a také `override` modifikátor.</span><span class="sxs-lookup"><span data-stu-id="56f2d-728">If the declaration includes the `sealed` modifier, then the declaration also includes the `override` modifier.</span></span>
*  <span data-ttu-id="56f2d-729">Pokud deklarace obsahuje `partial` modifikátor, pak nesmí obsahovat žádný z následujících parametrů: `new`, `public`, `protected`, `internal`, `private`, `virtual`, `sealed`, `override` , `abstract`, nebo `extern`.</span><span class="sxs-lookup"><span data-stu-id="56f2d-729">If the declaration includes the `partial` modifier, then it does not include any of the following modifiers: `new`, `public`, `protected`, `internal`, `private`, `virtual`, `sealed`, `override`, `abstract`, or `extern`.</span></span>

<span data-ttu-id="56f2d-730">Metodu, která má `async` Modifikátor je asynchronní funkci a postupuje pravidel popsaných v [asynchronních funkcí](classes.md#async-functions).</span><span class="sxs-lookup"><span data-stu-id="56f2d-730">A method that has the `async` modifier is an async function and follows the rules described in [Async functions](classes.md#async-functions).</span></span>

<span data-ttu-id="56f2d-731">*Typ* deklarace metody Určuje typ hodnoty vypočítané a vrácen touto metodou.</span><span class="sxs-lookup"><span data-stu-id="56f2d-731">The *return_type* of a method declaration specifies the type of the value computed and returned by the method.</span></span> <span data-ttu-id="56f2d-732">*Typ* je `void` Pokud metoda nevrací hodnotu.</span><span class="sxs-lookup"><span data-stu-id="56f2d-732">The *return_type* is `void` if the method does not return a value.</span></span> <span data-ttu-id="56f2d-733">Pokud deklarace obsahuje `partial` modifikátor, pak návratový typ musí být `void`.</span><span class="sxs-lookup"><span data-stu-id="56f2d-733">If the declaration includes the `partial` modifier, then the return type must be `void`.</span></span>

<span data-ttu-id="56f2d-734">*Member_name* Určuje název metody.</span><span class="sxs-lookup"><span data-stu-id="56f2d-734">The *member_name* specifies the name of the method.</span></span> <span data-ttu-id="56f2d-735">Pokud metoda není člen implementace explicitního rozhraní ([implementace explicitního rozhraní členských](interfaces.md#explicit-interface-member-implementations)), *member_name* je jednoduše k *identifikátor*.</span><span class="sxs-lookup"><span data-stu-id="56f2d-735">Unless the method is an explicit interface member implementation ([Explicit interface member implementations](interfaces.md#explicit-interface-member-implementations)), the *member_name* is simply an *identifier*.</span></span> <span data-ttu-id="56f2d-736">Člen implementace explicitního rozhraní naleznete *member_name* se skládá ze *interface_type* následovaný "`.`" a *identifikátor*.</span><span class="sxs-lookup"><span data-stu-id="56f2d-736">For an explicit interface member implementation, the *member_name* consists of an *interface_type* followed by a "`.`" and an *identifier*.</span></span>

<span data-ttu-id="56f2d-737">Volitelný *type_parameter_list* Určuje typ parametry metody ([parametry typu](classes.md#type-parameters)).</span><span class="sxs-lookup"><span data-stu-id="56f2d-737">The optional *type_parameter_list* specifies the type parameters of the method ([Type parameters](classes.md#type-parameters)).</span></span> <span data-ttu-id="56f2d-738">Pokud *type_parameter_list* určena metoda je ***obecnou metodu***.</span><span class="sxs-lookup"><span data-stu-id="56f2d-738">If a *type_parameter_list* is specified the method is a ***generic method***.</span></span> <span data-ttu-id="56f2d-739">Pokud tato metoda má `extern` modifikátor, *type_parameter_list* nelze zadat.</span><span class="sxs-lookup"><span data-stu-id="56f2d-739">If the method has an `extern` modifier, a *type_parameter_list* cannot be specified.</span></span>

<span data-ttu-id="56f2d-740">Volitelný *formal_parameter_list* Určuje parametry metody ([parametry metody](classes.md#method-parameters)).</span><span class="sxs-lookup"><span data-stu-id="56f2d-740">The optional *formal_parameter_list* specifies the parameters of the method ([Method parameters](classes.md#method-parameters)).</span></span>

<span data-ttu-id="56f2d-741">Volitelný *type_parameter_constraints_clause*s zadat omezení parametrů jednotlivých typů ([omezení parametru typu](classes.md#type-parameter-constraints)) a může být určen pouze pokud *type_parameter_ seznam* je rovněž dodán, a nemá metodu `override` modifikátor.</span><span class="sxs-lookup"><span data-stu-id="56f2d-741">The optional *type_parameter_constraints_clause*s specify constraints on individual type parameters ([Type parameter constraints](classes.md#type-parameter-constraints)) and may only be specified if a *type_parameter_list* is also supplied, and the method does not have an `override` modifier.</span></span>

<span data-ttu-id="56f2d-742">*Typ* a každý z typů odkazuje *formal_parameter_list* metody musí být přinejmenším stejně dostupná jako metoda sama ([usnadnění omezení](basic-concepts.md#accessibility-constraints)).</span><span class="sxs-lookup"><span data-stu-id="56f2d-742">The *return_type* and each of the types referenced in the *formal_parameter_list* of a method must be at least as accessible as the method itself ([Accessibility constraints](basic-concepts.md#accessibility-constraints)).</span></span>

<span data-ttu-id="56f2d-743">*Method_body* je buď středník ***tělo s příkazy*** nebo ***tělo výrazu***.</span><span class="sxs-lookup"><span data-stu-id="56f2d-743">The *method_body* is either a semicolon, a ***statement body*** or an ***expression body***.</span></span> <span data-ttu-id="56f2d-744">Tělo s příkazy se skládá z *bloku*, která určuje příkazy ke spuštění při vyvolání metody.</span><span class="sxs-lookup"><span data-stu-id="56f2d-744">A statement body consists of a *block*, which specifies the statements to execute when the method is invoked.</span></span> <span data-ttu-id="56f2d-745">Tělo výrazu se skládá z `=>` následovaný *výraz* a středník a označuje jeden výraz k provádění při vyvolání metody.</span><span class="sxs-lookup"><span data-stu-id="56f2d-745">An expression body consists of `=>` followed by an *expression* and a semicolon, and denotes a single expression to perform when the method is invoked.</span></span> 

<span data-ttu-id="56f2d-746">Pro `abstract` a `extern` metody, *method_body* se skládá pouze ze středníku.</span><span class="sxs-lookup"><span data-stu-id="56f2d-746">For `abstract` and `extern` methods, the *method_body* consists simply of a semicolon.</span></span> <span data-ttu-id="56f2d-747">Pro `partial` metody *method_body* může skládat ze středníku, blok textu nebo tělo výrazu.</span><span class="sxs-lookup"><span data-stu-id="56f2d-747">For `partial` methods the *method_body* may consist of either a semicolon, a block body or an expression body.</span></span> <span data-ttu-id="56f2d-748">Pro všechny ostatní metody *method_body* je blok textu nebo tělo výrazu.</span><span class="sxs-lookup"><span data-stu-id="56f2d-748">For all other methods, the *method_body* is either a block body or an expression body.</span></span>

<span data-ttu-id="56f2d-749">Pokud *method_body* se skládá ze středníku, pak nemusí obsahovat deklaraci `async` modifikátor.</span><span class="sxs-lookup"><span data-stu-id="56f2d-749">If the *method_body* consists of a semicolon, then the declaration may not include the `async` modifier.</span></span>

<span data-ttu-id="56f2d-750">Název, seznam parametrů typu a seznam formálních parametrů metody definování signatury ([podpisy a přetížení](basic-concepts.md#signatures-and-overloading)) metody.</span><span class="sxs-lookup"><span data-stu-id="56f2d-750">The name, the type parameter list and the formal parameter list of a method define the signature ([Signatures and overloading](basic-concepts.md#signatures-and-overloading)) of the method.</span></span> <span data-ttu-id="56f2d-751">Konkrétně podpis metody se skládá z názvu, počet parametrů typu a číslo, modifikátory a typy formálních parametrů.</span><span class="sxs-lookup"><span data-stu-id="56f2d-751">Specifically, the signature of a method consists of its name, the number of type parameters and the number, modifiers, and types of its formal parameters.</span></span> <span data-ttu-id="56f2d-752">Pro tyto účely je identifikován libovolný typ parametru metody, nacházející se v typu formálního parametru není podle názvu, ale podle jeho pořadové číslo pozice v seznamu argumentů typu metody. Návratový typ není součástí signatury metody, ani se o názvy formální parametry ani parametry typu.</span><span class="sxs-lookup"><span data-stu-id="56f2d-752">For these purposes, any type parameter of the method that occurs in the type of a formal parameter is identified not by its name, but by its ordinal position in the type argument list of the method.The return type is not part of a method's signature, nor are the names of the type parameters or the formal parameters.</span></span>

<span data-ttu-id="56f2d-753">Název metody se musí lišit od názvů ostatních bez metody s deklarací ve stejné třídě.</span><span class="sxs-lookup"><span data-stu-id="56f2d-753">The name of a method must differ from the names of all other non-methods declared in the same class.</span></span> <span data-ttu-id="56f2d-754">Kromě toho podpis metody se musí lišit od podpisů všechny ostatní metody s deklarací ve stejné třídě, a dvě metody s deklarací ve stejné třídě, nemusí mít podpisy, které se liší pouze `ref` a `out`.</span><span class="sxs-lookup"><span data-stu-id="56f2d-754">In addition, the signature of a method must differ from the signatures of all other methods declared in the same class, and two methods declared in the same class may not have signatures that differ solely by `ref` and `out`.</span></span>

<span data-ttu-id="56f2d-755">Metody *type_parameter*s jsou v oboru v celém *method_declaration*a je možné typy formulářů v daném oboru v celém *typ*, *method_body*, a *type_parameter_constraints_clause*s, ale ne v *atributy*.</span><span class="sxs-lookup"><span data-stu-id="56f2d-755">The method's *type_parameter*s are in scope throughout the *method_declaration*, and can be used to form types throughout that scope in *return_type*, *method_body*, and *type_parameter_constraints_clause*s but not in *attributes*.</span></span>

<span data-ttu-id="56f2d-756">Všechny formálních parametrů a parametrů typu musí mít jedinečné názvy.</span><span class="sxs-lookup"><span data-stu-id="56f2d-756">All formal parameters and type parameters must have different names.</span></span>

### <a name="method-parameters"></a><span data-ttu-id="56f2d-757">Parametry metody</span><span class="sxs-lookup"><span data-stu-id="56f2d-757">Method parameters</span></span>

<span data-ttu-id="56f2d-758">Parametry metody, pokud existují, jsou deklarovány pomocí metody *formal_parameter_list*.</span><span class="sxs-lookup"><span data-stu-id="56f2d-758">The parameters of a method, if any, are declared by the method's *formal_parameter_list*.</span></span>

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

<span data-ttu-id="56f2d-759">Seznam formálních parametrů se skládá z jednoho nebo více oddělených čárkou parametrů z nichž může být pouze poslední *parameter_array*.</span><span class="sxs-lookup"><span data-stu-id="56f2d-759">The formal parameter list consists of one or more comma-separated parameters of which only the last may be a *parameter_array*.</span></span>

<span data-ttu-id="56f2d-760">A *fixed_parameter* se skládá z volitelné sadu *atributy* ([atributy](attributes.md)), volitelně `ref`, `out` nebo `this` modifikátor, *typ*, *identifikátor* a volitelně *default_argument*.</span><span class="sxs-lookup"><span data-stu-id="56f2d-760">A *fixed_parameter* consists of an optional set of *attributes* ([Attributes](attributes.md)), an optional `ref`, `out` or `this` modifier, a *type*, an *identifier* and an optional *default_argument*.</span></span> <span data-ttu-id="56f2d-761">Každý *fixed_parameter* deklaruje parametr daného typu s daným názvem.</span><span class="sxs-lookup"><span data-stu-id="56f2d-761">Each *fixed_parameter* declares a parameter of the given type with the given name.</span></span> <span data-ttu-id="56f2d-762">`this` Modifikátor označí metody jako metody rozšíření a je povolený jenom u první parametr statické metody.</span><span class="sxs-lookup"><span data-stu-id="56f2d-762">The `this` modifier designates the method as an extension method and is only allowed on the first parameter of a static method.</span></span> <span data-ttu-id="56f2d-763">Rozšiřující metody jsou popsané v [rozšiřující metody](classes.md#extension-methods).</span><span class="sxs-lookup"><span data-stu-id="56f2d-763">Extension methods are further described in [Extension methods](classes.md#extension-methods).</span></span>

<span data-ttu-id="56f2d-764">A *fixed_parameter* s *default_argument* se označuje jako ***volitelný parametr***, že *fixed_parameter* bez *default_argument* je ***požadovaný parametr***.</span><span class="sxs-lookup"><span data-stu-id="56f2d-764">A *fixed_parameter* with a *default_argument* is known as an ***optional parameter***, whereas a *fixed_parameter* without a *default_argument* is a ***required parameter***.</span></span> <span data-ttu-id="56f2d-765">Povinný parametr se nemusí zobrazit po volitelný parametr *formal_parameter_list*.</span><span class="sxs-lookup"><span data-stu-id="56f2d-765">A required parameter may not appear after an optional parameter in a *formal_parameter_list*.</span></span>

<span data-ttu-id="56f2d-766">A `ref` nebo `out` parametr nemůže mít *default_argument*.</span><span class="sxs-lookup"><span data-stu-id="56f2d-766">A `ref` or `out` parameter cannot have a *default_argument*.</span></span> <span data-ttu-id="56f2d-767">*Výraz* v *default_argument* musí být jedna z následujících akcí:</span><span class="sxs-lookup"><span data-stu-id="56f2d-767">The *expression* in a *default_argument* must be one of the following:</span></span>

*  <span data-ttu-id="56f2d-768">*constant_expression*</span><span class="sxs-lookup"><span data-stu-id="56f2d-768">a *constant_expression*</span></span>
*  <span data-ttu-id="56f2d-769">výraz ve tvaru `new S()` kde `S` je typ hodnoty</span><span class="sxs-lookup"><span data-stu-id="56f2d-769">an expression of the form `new S()` where `S` is a value type</span></span>
*  <span data-ttu-id="56f2d-770">výraz ve tvaru `default(S)` kde `S` je typ hodnoty</span><span class="sxs-lookup"><span data-stu-id="56f2d-770">an expression of the form `default(S)` where `S` is a value type</span></span>

<span data-ttu-id="56f2d-771">*Výraz* musí být implicitně převeditelný identity nebo s povolenou hodnotou Null převod na typ parametru.</span><span class="sxs-lookup"><span data-stu-id="56f2d-771">The *expression* must be implicitly convertible by an identity or nullable conversion to the type of the parameter.</span></span>

<span data-ttu-id="56f2d-772">Pokud dojde k volitelné parametry v implementující deklaraci částečné metody ([částečné metody](classes.md#partial-methods)), člen implementace explicitního rozhraní ([implementace explicitního rozhraní členských](interfaces.md#explicit-interface-member-implementations)) nebo indexer jedním parametrem deklarace ([indexery](classes.md#indexers)) kompilátor měl dát upozornění, protože tyto členy můžete nelze nikdy vyvolat způsobem, který umožňuje argumenty, které mají být vynechán.</span><span class="sxs-lookup"><span data-stu-id="56f2d-772">If optional parameters occur in an implementing partial method declaration ([Partial methods](classes.md#partial-methods)) , an explicit interface member implementation ([Explicit interface member implementations](interfaces.md#explicit-interface-member-implementations)) or in a single-parameter indexer declaration ([Indexers](classes.md#indexers)) the compiler should give a warning, since these members can never be invoked in a way that permits arguments to be omitted.</span></span>

<span data-ttu-id="56f2d-773">A *parameter_array* se skládá z volitelné sadu *atributy* ([atributy](attributes.md)), `params` modifikátor, *array_type*, a *identifikátor*.</span><span class="sxs-lookup"><span data-stu-id="56f2d-773">A *parameter_array* consists of an optional set of *attributes* ([Attributes](attributes.md)), a `params` modifier, an *array_type*, and an *identifier*.</span></span> <span data-ttu-id="56f2d-774">Pole parametrů deklaruje jeden parametr typu daného pole s daným názvem.</span><span class="sxs-lookup"><span data-stu-id="56f2d-774">A parameter array declares a single parameter of the given array type with the given name.</span></span> <span data-ttu-id="56f2d-775">*Array_type* parametru pole musí být typu jednorozměrná pole ([pole typů](arrays.md#array-types)).</span><span class="sxs-lookup"><span data-stu-id="56f2d-775">The *array_type* of a parameter array must be a single-dimensional array type ([Array types](arrays.md#array-types)).</span></span> <span data-ttu-id="56f2d-776">Ve volání metody pole parametrů povoluje buď jeden argument typu daného pole zadat nebo it specialistovi nuly nebo více argumentů typ elementu pole zadat.</span><span class="sxs-lookup"><span data-stu-id="56f2d-776">In a method invocation, a parameter array permits either a single argument of the given array type to be specified, or it permits zero or more arguments of the array element type to be specified.</span></span> <span data-ttu-id="56f2d-777">Pole parametrů jsou popsány dále v [pole parametrů](classes.md#parameter-arrays).</span><span class="sxs-lookup"><span data-stu-id="56f2d-777">Parameter arrays are described further in [Parameter arrays](classes.md#parameter-arrays).</span></span>

<span data-ttu-id="56f2d-778">A *parameter_array* může vzniknout po volitelný parametr, ale nemůže mít výchozí hodnotu--vynechání argumenty *parameter_array* místo toho způsobí vytvoření prázdné pole.</span><span class="sxs-lookup"><span data-stu-id="56f2d-778">A *parameter_array* may occur after an optional parameter, but cannot have a default value -- the omission of arguments for a *parameter_array* would instead result in the creation of an empty array.</span></span>

<span data-ttu-id="56f2d-779">Následující příklad ukazuje různé druhy parametry:</span><span class="sxs-lookup"><span data-stu-id="56f2d-779">The following example illustrates different kinds of parameters:</span></span>
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

<span data-ttu-id="56f2d-780">V *formal_parameter_list* pro `M`, `i` je parametr povinný ref `d` je povinná hodnota parametr `b`, `s`, `o` a `t` jsou nepovinné hodnoty parametrů a `a` je pole parametrů.</span><span class="sxs-lookup"><span data-stu-id="56f2d-780">In the *formal_parameter_list* for `M`, `i` is a required ref parameter, `d` is a required value parameter, `b`, `s`, `o` and `t` are optional value parameters and `a` is a parameter array.</span></span>

<span data-ttu-id="56f2d-781">Deklarace metody vytvoří samostatné prohlášení prostor pro parametry, parametry typu a místní proměnné.</span><span class="sxs-lookup"><span data-stu-id="56f2d-781">A method declaration creates a separate declaration space for parameters, type parameters and local variables.</span></span> <span data-ttu-id="56f2d-782">Názvy jsou zavedeny do tohoto prostoru deklarace seznamu parametrů typu a seznamu formálních parametrů metody a místní deklarace proměnné v *bloku* metody.</span><span class="sxs-lookup"><span data-stu-id="56f2d-782">Names are introduced into this declaration space by the type parameter list and the formal parameter list of the method and by local variable declarations in the *block* of the method.</span></span> <span data-ttu-id="56f2d-783">Jedná se o chybu pro dva členy místa deklarace metoda má stejný název.</span><span class="sxs-lookup"><span data-stu-id="56f2d-783">It is an error for two members of a method declaration space to have the same name.</span></span> <span data-ttu-id="56f2d-784">Jedná se o chybu místa deklarace metody a deklarace lokální proměnné místo prostoru vnořené deklarace tak, aby obsahovala elementy se stejným názvem.</span><span class="sxs-lookup"><span data-stu-id="56f2d-784">It is an error for the method declaration space and the local variable declaration space of a nested declaration space to contain elements with the same name.</span></span>

<span data-ttu-id="56f2d-785">Volání metody ([volání metod](expressions.md#method-invocations)) vytvoří kopii specifické pro tuto vyvolání, formálních parametrů a lokálních proměnných metody a seznamu argumentů volání přiřadí hodnoty nebo proměnné odkazů na nově vytvořené formální parametry.</span><span class="sxs-lookup"><span data-stu-id="56f2d-785">A method invocation ([Method invocations](expressions.md#method-invocations)) creates a copy, specific to that invocation, of the formal parameters and local variables of the method, and the argument list of the invocation assigns values or variable references to the newly created formal parameters.</span></span> <span data-ttu-id="56f2d-786">V rámci *bloku* metody, formální parametry lze odkazovat pomocí jejich identifikátorů v *simple_name* výrazy ([jednoduché názvy](expressions.md#simple-names)).</span><span class="sxs-lookup"><span data-stu-id="56f2d-786">Within the *block* of a method, formal parameters can be referenced by their identifiers in *simple_name* expressions ([Simple names](expressions.md#simple-names)).</span></span>

<span data-ttu-id="56f2d-787">Existují čtyři typy formálních parametrů:</span><span class="sxs-lookup"><span data-stu-id="56f2d-787">There are four kinds of formal parameters:</span></span>

*  <span data-ttu-id="56f2d-788">Hodnoty parametrů, které jsou deklarovány bez jakékoli modifikátory.</span><span class="sxs-lookup"><span data-stu-id="56f2d-788">Value parameters, which are declared without any modifiers.</span></span>
*  <span data-ttu-id="56f2d-789">Odkazovat na parametry, které jsou deklarovány pomocí `ref` modifikátor.</span><span class="sxs-lookup"><span data-stu-id="56f2d-789">Reference parameters, which are declared with the `ref` modifier.</span></span>
*  <span data-ttu-id="56f2d-790">Výstupní parametry, které jsou deklarovány pomocí `out` modifikátor.</span><span class="sxs-lookup"><span data-stu-id="56f2d-790">Output parameters, which are declared with the `out` modifier.</span></span>
*  <span data-ttu-id="56f2d-791">Pole parametrů, které jsou deklarovány pomocí `params` modifikátor.</span><span class="sxs-lookup"><span data-stu-id="56f2d-791">Parameter arrays, which are declared with the `params` modifier.</span></span>

<span data-ttu-id="56f2d-792">Jak je popsáno v [podpisy a přetížení](basic-concepts.md#signatures-and-overloading), `ref` a `out` modifikátory jsou součástí signatury metody, ale `params` modifikátor není.</span><span class="sxs-lookup"><span data-stu-id="56f2d-792">As described in [Signatures and overloading](basic-concepts.md#signatures-and-overloading), the `ref` and `out` modifiers are part of a method's signature, but the `params` modifier is not.</span></span>

#### <a name="value-parameters"></a><span data-ttu-id="56f2d-793">Parametry s hodnotou</span><span class="sxs-lookup"><span data-stu-id="56f2d-793">Value parameters</span></span>

<span data-ttu-id="56f2d-794">Parametr deklarovány žádné modifikátory přístupu se parametr hodnoty.</span><span class="sxs-lookup"><span data-stu-id="56f2d-794">A parameter declared with no modifiers is a value parameter.</span></span> <span data-ttu-id="56f2d-795">Hodnota parametru odpovídá místní proměnná, která získá počáteční hodnoty z na odpovídající argument zadaný ve volání metody.</span><span class="sxs-lookup"><span data-stu-id="56f2d-795">A value parameter corresponds to a local variable that gets its initial value from the corresponding argument supplied in the method invocation.</span></span>

<span data-ttu-id="56f2d-796">Po formální parametr se parametr hodnoty odpovídající argument ve volání metody musí být výraz, který je implicitně převést ([implicitních převodů](conversions.md#implicit-conversions)) na typ formálního parametru.</span><span class="sxs-lookup"><span data-stu-id="56f2d-796">When a formal parameter is a value parameter, the corresponding argument in a method invocation must be an expression that is implicitly convertible ([Implicit conversions](conversions.md#implicit-conversions)) to the formal parameter type.</span></span>

<span data-ttu-id="56f2d-797">Metoda je povolený pro přiřazení nové hodnoty pro parametr hodnoty.</span><span class="sxs-lookup"><span data-stu-id="56f2d-797">A method is permitted to assign new values to a value parameter.</span></span> <span data-ttu-id="56f2d-798">Taková přiřazení ovlivní pouze místní úložiště reprezentovaný hodnotou parametru – nemají žádný účinek na skutečný argument předaný ve volání metody.</span><span class="sxs-lookup"><span data-stu-id="56f2d-798">Such assignments only affect the local storage location represented by the value parameter—they have no effect on the actual argument given in the method invocation.</span></span>

#### <a name="reference-parameters"></a><span data-ttu-id="56f2d-799">Parametry odkazu</span><span class="sxs-lookup"><span data-stu-id="56f2d-799">Reference parameters</span></span>

<span data-ttu-id="56f2d-800">Parametr deklarovaný s `ref` Modifikátor je odkaz na parametr.</span><span class="sxs-lookup"><span data-stu-id="56f2d-800">A parameter declared with a `ref` modifier is a reference parameter.</span></span> <span data-ttu-id="56f2d-801">Na rozdíl od parametru hodnoty parametr odkazu vytváření nového umístění úložiště.</span><span class="sxs-lookup"><span data-stu-id="56f2d-801">Unlike a value parameter, a reference parameter does not create a new storage location.</span></span> <span data-ttu-id="56f2d-802">Místo toho referenční parametr představuje stejné úložiště jako proměnná, zadaný jako argument ve volání metody.</span><span class="sxs-lookup"><span data-stu-id="56f2d-802">Instead, a reference parameter represents the same storage location as the variable given as the argument in the method invocation.</span></span>

<span data-ttu-id="56f2d-803">Parametr odkazu po formálním parametrem odpovídající argument ve volání metody se musí skládat z klíčového slova `ref` následovaný *variable_reference* ([přesné pravidla pro určování jednoznačného přiřazení](variables.md#precise-rules-for-determining-definite-assignment)) stejného typu jako formálních parametrů.</span><span class="sxs-lookup"><span data-stu-id="56f2d-803">When a formal parameter is a reference parameter, the corresponding argument in a method invocation must consist of the keyword `ref` followed by a *variable_reference* ([Precise rules for determining definite assignment](variables.md#precise-rules-for-determining-definite-assignment)) of the same type as the formal parameter.</span></span> <span data-ttu-id="56f2d-804">Proměnná musí být jednoznačně přiřazena dříve, než může být předán jako parametr odkazu.</span><span class="sxs-lookup"><span data-stu-id="56f2d-804">A variable must be definitely assigned before it can be passed as a reference parameter.</span></span>

<span data-ttu-id="56f2d-805">V rámci metody referenční parametr je vždy považován za jednoznačně přiřazené.</span><span class="sxs-lookup"><span data-stu-id="56f2d-805">Within a method, a reference parameter is always considered definitely assigned.</span></span>

<span data-ttu-id="56f2d-806">Metody deklarované jako iterátor ([iterátory](classes.md#iterators)) nemůže mít parametry odkazů.</span><span class="sxs-lookup"><span data-stu-id="56f2d-806">A method declared as an iterator ([Iterators](classes.md#iterators)) cannot have reference parameters.</span></span>

<span data-ttu-id="56f2d-807">V příkladu</span><span class="sxs-lookup"><span data-stu-id="56f2d-807">The example</span></span>
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
<span data-ttu-id="56f2d-808">Vytvoří výstup</span><span class="sxs-lookup"><span data-stu-id="56f2d-808">produces the output</span></span>
```
i = 2, j = 1
```

<span data-ttu-id="56f2d-809">Pro vyvolání `Swap` v `Main`, `x` představuje `i` a `y` představuje `j`.</span><span class="sxs-lookup"><span data-stu-id="56f2d-809">For the invocation of `Swap` in `Main`, `x` represents `i` and `y` represents `j`.</span></span> <span data-ttu-id="56f2d-810">Proto volání má vliv na prohození hodnoty `i` a `j`.</span><span class="sxs-lookup"><span data-stu-id="56f2d-810">Thus, the invocation has the effect of swapping the values of `i` and `j`.</span></span>

<span data-ttu-id="56f2d-811">V metodě, která přebírá parametry odkazů je možné pro více názvy, které představují stejné umístění úložiště.</span><span class="sxs-lookup"><span data-stu-id="56f2d-811">In a method that takes reference parameters it is possible for multiple names to represent the same storage location.</span></span> <span data-ttu-id="56f2d-812">V příkladu</span><span class="sxs-lookup"><span data-stu-id="56f2d-812">In the example</span></span>
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
<span data-ttu-id="56f2d-813">vyvolání `F` v `G` předává odkazem na `s` pro obě `a` a `b`.</span><span class="sxs-lookup"><span data-stu-id="56f2d-813">the invocation of `F` in `G` passes a reference to `s` for both `a` and `b`.</span></span> <span data-ttu-id="56f2d-814">Proto pro vyvolání, názvy `s`, `a`, a `b` všechny odkazovat do stejného umístění úložiště, a upravte všechny tři přiřazení pole instance `s`.</span><span class="sxs-lookup"><span data-stu-id="56f2d-814">Thus, for that invocation, the names `s`, `a`, and `b` all refer to the same storage location, and the three assignments all modify the instance field `s`.</span></span>

#### <a name="output-parameters"></a><span data-ttu-id="56f2d-815">Výstupní parametry</span><span class="sxs-lookup"><span data-stu-id="56f2d-815">Output parameters</span></span>

<span data-ttu-id="56f2d-816">Parametr deklarovaný pomocí `out` Modifikátor je výstupní parametr.</span><span class="sxs-lookup"><span data-stu-id="56f2d-816">A parameter declared with an `out` modifier is an output parameter.</span></span> <span data-ttu-id="56f2d-817">Podobně jako parametr odkazu, výstupní parametr nevytváří nového umístění úložiště.</span><span class="sxs-lookup"><span data-stu-id="56f2d-817">Similar to a reference parameter, an output parameter does not create a new storage location.</span></span> <span data-ttu-id="56f2d-818">Místo toho výstupní parametr představuje stejné úložiště jako proměnná, zadaný jako argument ve volání metody.</span><span class="sxs-lookup"><span data-stu-id="56f2d-818">Instead, an output parameter represents the same storage location as the variable given as the argument in the method invocation.</span></span>

<span data-ttu-id="56f2d-819">Při formální parametr je výstupní parametr, odpovídající argument ve volání metody musí obsahovat klíčové slovo `out` následovaný *variable_reference* ([přesné pravidla pro určování jednoznačného přiřazení](variables.md#precise-rules-for-determining-definite-assignment)) stejného typu jako formálních parametrů.</span><span class="sxs-lookup"><span data-stu-id="56f2d-819">When a formal parameter is an output parameter, the corresponding argument in a method invocation must consist of the keyword `out` followed by a *variable_reference* ([Precise rules for determining definite assignment](variables.md#precise-rules-for-determining-definite-assignment)) of the same type as the formal parameter.</span></span> <span data-ttu-id="56f2d-820">Proměnná není jednoznačně přiřazena dříve, než může být předán jako výstupní parametr, ale po vyvolání, kde byla předána proměnná jako výstupní parametr, proměnná je považován za jednoznačně přiřazené.</span><span class="sxs-lookup"><span data-stu-id="56f2d-820">A variable need not be definitely assigned before it can be passed as an output parameter, but following an invocation where a variable was passed as an output parameter, the variable is considered definitely assigned.</span></span>

<span data-ttu-id="56f2d-821">V rámci metody, stejně jako místní proměnná, výstupní parametr je zpočátku považován za nepřiřazené a musí být jednoznačně přiřazena dříve, než se jeho hodnota se používá.</span><span class="sxs-lookup"><span data-stu-id="56f2d-821">Within a method, just like a local variable, an output parameter is initially considered unassigned and must be definitely assigned before its value is used.</span></span>

<span data-ttu-id="56f2d-822">Každou výstupní parametr metody musí být jednoznačně přiřazena dříve, než metoda vrátí.</span><span class="sxs-lookup"><span data-stu-id="56f2d-822">Every output parameter of a method must be definitely assigned before the method returns.</span></span>

<span data-ttu-id="56f2d-823">Metoda deklarována jako částečná metoda ([částečné metody](classes.md#partial-methods)) nebo iterátor ([iterátory](classes.md#iterators)) nemůže mít výstupní parametry.</span><span class="sxs-lookup"><span data-stu-id="56f2d-823">A method declared as a partial method ([Partial methods](classes.md#partial-methods)) or an iterator ([Iterators](classes.md#iterators)) cannot have output parameters.</span></span>

<span data-ttu-id="56f2d-824">Výstupní parametry jsou obvykle používány v metodách, které vytvářejí více návratových hodnot.</span><span class="sxs-lookup"><span data-stu-id="56f2d-824">Output parameters are typically used in methods that produce multiple return values.</span></span> <span data-ttu-id="56f2d-825">Příklad:</span><span class="sxs-lookup"><span data-stu-id="56f2d-825">For example:</span></span>
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

<span data-ttu-id="56f2d-826">Vzorové postupy výstupu:</span><span class="sxs-lookup"><span data-stu-id="56f2d-826">The example produces the output:</span></span>
```
c:\Windows\System\
hello.txt
```

<span data-ttu-id="56f2d-827">Všimněte si, že `dir` a `name` proměnné může nepřiřazené, než jsou předány `SplitPath`, a že jsou považovány za jednoznačně přiřazené následující po volání.</span><span class="sxs-lookup"><span data-stu-id="56f2d-827">Note that the `dir` and `name` variables can be unassigned before they are passed to `SplitPath`, and that they are considered definitely assigned following the call.</span></span>

#### <a name="parameter-arrays"></a><span data-ttu-id="56f2d-828">Pole parametrů</span><span class="sxs-lookup"><span data-stu-id="56f2d-828">Parameter arrays</span></span>

<span data-ttu-id="56f2d-829">Parametr deklarovaný s `params` se modifikátor pole parametrů.</span><span class="sxs-lookup"><span data-stu-id="56f2d-829">A parameter declared with a `params` modifier is a parameter array.</span></span> <span data-ttu-id="56f2d-830">Pokud seznam formálních parametrů obsahuje pole parametrů, musí být posledním parametrem v seznamu a musí být jednorozměrné pole typu.</span><span class="sxs-lookup"><span data-stu-id="56f2d-830">If a formal parameter list includes a parameter array, it must be the last parameter in the list and it must be of a single-dimensional array type.</span></span> <span data-ttu-id="56f2d-831">Například typy `string[]` a `string[][]` lze použít jako typ pole parametrů, ale typ `string[,]` nemůže.</span><span class="sxs-lookup"><span data-stu-id="56f2d-831">For example, the types `string[]` and `string[][]` can be used as the type of a parameter array, but the type `string[,]` can not.</span></span> <span data-ttu-id="56f2d-832">Není možné kombinovat `params` modifikátor modifikátory `ref` a `out`.</span><span class="sxs-lookup"><span data-stu-id="56f2d-832">It is not possible to combine the `params` modifier with the modifiers `ref` and `out`.</span></span>

<span data-ttu-id="56f2d-833">Pole parametrů povoluje argumenty, které mají být zadány v některém ze dvou způsobů ve volání metody:</span><span class="sxs-lookup"><span data-stu-id="56f2d-833">A parameter array permits arguments to be specified in one of two ways in a method invocation:</span></span>

*  <span data-ttu-id="56f2d-834">Argument zadaný pro parametr pole může být jediný výraz, který je implicitně převést ([implicitních převodů](conversions.md#implicit-conversions)) na typ pole parametrů.</span><span class="sxs-lookup"><span data-stu-id="56f2d-834">The argument given for a parameter array can be a single expression that is implicitly convertible ([Implicit conversions](conversions.md#implicit-conversions)) to the parameter array type.</span></span> <span data-ttu-id="56f2d-835">V tomto případě pole parametrů funguje přesně jako hodnotu parametru.</span><span class="sxs-lookup"><span data-stu-id="56f2d-835">In this case, the parameter array acts precisely like a value parameter.</span></span>
*  <span data-ttu-id="56f2d-836">Alternativně můžete vyvolání zadat nuly nebo více argumentů pro pole parametrů, kde každý argument je výraz, který je implicitně převést ([implicitních převodů](conversions.md#implicit-conversions)) na typ elementu pole parametrů.</span><span class="sxs-lookup"><span data-stu-id="56f2d-836">Alternatively, the invocation can specify zero or more arguments for the parameter array, where each argument is an expression that is implicitly convertible ([Implicit conversions](conversions.md#implicit-conversions)) to the element type of the parameter array.</span></span> <span data-ttu-id="56f2d-837">V takovém případě vyvolání vytvoří instanci typu parametru pole s délkou odpovídající počet argumentů, inicializuje prvky instance pole s hodnotami daný argument a používá instanci nově vytvořené pole jako skutečný argument.</span><span class="sxs-lookup"><span data-stu-id="56f2d-837">In this case, the invocation creates an instance of the parameter array type with a length corresponding to the number of arguments, initializes the elements of the array instance with the given argument values, and uses the newly created array instance as the actual argument.</span></span>

<span data-ttu-id="56f2d-838">Kromě povolení proměnný počet argumentů ve vyvolání, je přesně odpovídá parametru hodnoty pole parametrů ([hodnoty parametrů](classes.md#value-parameters)) stejného typu.</span><span class="sxs-lookup"><span data-stu-id="56f2d-838">Except for allowing a variable number of arguments in an invocation, a parameter array is precisely equivalent to a value parameter ([Value parameters](classes.md#value-parameters)) of the same type.</span></span>

<span data-ttu-id="56f2d-839">V příkladu</span><span class="sxs-lookup"><span data-stu-id="56f2d-839">The example</span></span>
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
<span data-ttu-id="56f2d-840">Vytvoří výstup</span><span class="sxs-lookup"><span data-stu-id="56f2d-840">produces the output</span></span>
```
Array contains 3 elements: 1 2 3
Array contains 4 elements: 10 20 30 40
Array contains 0 elements:
```

<span data-ttu-id="56f2d-841">Před prvním vyvoláním služby `F` jednoduše předává pole `a` jako hodnotu parametru.</span><span class="sxs-lookup"><span data-stu-id="56f2d-841">The first invocation of `F` simply passes the array `a` as a value parameter.</span></span> <span data-ttu-id="56f2d-842">Druhé volání `F` automaticky vytvoří čtyřech prvcích `int[]` s hodnotami daného elementu a předá, které pole instance jako hodnotu parametru.</span><span class="sxs-lookup"><span data-stu-id="56f2d-842">The second invocation of `F` automatically creates a four-element `int[]` with the given element values and passes that array instance as a value parameter.</span></span> <span data-ttu-id="56f2d-843">Podobně, třetí vyvolání `F` vytvoří element nula `int[]` a předá jako parametr hodnoty této instance.</span><span class="sxs-lookup"><span data-stu-id="56f2d-843">Likewise, the third invocation of `F` creates a zero-element `int[]` and passes that instance as a value parameter.</span></span> <span data-ttu-id="56f2d-844">Druhý a třetí volání přesně odpovídají na zápis:</span><span class="sxs-lookup"><span data-stu-id="56f2d-844">The second and third invocations are precisely equivalent to writing:</span></span>
```csharp
F(new int[] {10, 20, 30, 40});
F(new int[] {});
```

<span data-ttu-id="56f2d-845">Při překladu přetížení metody s polem parametrů lze použít v běžné formě nebo v podobě rozšířené ([použít funkční člen](expressions.md#applicable-function-member)).</span><span class="sxs-lookup"><span data-stu-id="56f2d-845">When performing overload resolution, a method with a parameter array may be applicable either in its normal form or in its expanded form ([Applicable function member](expressions.md#applicable-function-member)).</span></span> <span data-ttu-id="56f2d-846">Rozbalené metody je k dispozici pouze v případě, že formuláři normální metody se nedá použít, a pouze v případě použitelnou metodu se stejnou signaturou, jako rozšířené formulář není již deklarovány v rámci stejného typu.</span><span class="sxs-lookup"><span data-stu-id="56f2d-846">The expanded form of a method is available only if the normal form of the method is not applicable and only if an applicable method with the same signature as the expanded form is not already declared in the same type.</span></span>

<span data-ttu-id="56f2d-847">V příkladu</span><span class="sxs-lookup"><span data-stu-id="56f2d-847">The example</span></span>
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
<span data-ttu-id="56f2d-848">Vytvoří výstup</span><span class="sxs-lookup"><span data-stu-id="56f2d-848">produces the output</span></span>
```
F();
F(object[]);
F(object,object);
F(object[]);
F(object[]);
```

<span data-ttu-id="56f2d-849">V tomto příkladu dvě formy možné rozšířené metody s polem parametrů jsou již zahrnuty v třídě jako běžné metody.</span><span class="sxs-lookup"><span data-stu-id="56f2d-849">In the example, two of the possible expanded forms of the method with a parameter array are already included in the class as regular methods.</span></span> <span data-ttu-id="56f2d-850">Tyto rozšířené formuláře proto ohled při provádění rozlišení přetížení a volání metod první a třetí proto vyberte běžné metody.</span><span class="sxs-lookup"><span data-stu-id="56f2d-850">These expanded forms are therefore not considered when performing overload resolution, and the first and third method invocations thus select the regular methods.</span></span> <span data-ttu-id="56f2d-851">Pokud třída deklaruje metodu s polem parametrů, není nic neobvyklého, také obsahovat některé rozšířené formuláře jako běžné metody.</span><span class="sxs-lookup"><span data-stu-id="56f2d-851">When a class declares a method with a parameter array, it is not uncommon to also include some of the expanded forms as regular methods.</span></span> <span data-ttu-id="56f2d-852">Tímto způsobem, takže je možné, aby se zabránilo přidělení pole instance, která nastane, pokud rozbalené metody s polem parametrů vyvolání.</span><span class="sxs-lookup"><span data-stu-id="56f2d-852">By doing so it is possible to avoid the allocation of an array instance that occurs when an expanded form of a method with a parameter array is invoked.</span></span>

<span data-ttu-id="56f2d-853">Pokud je typ pole parametrů `object[]`, potenciální nejednoznačností dochází mezi formuláři normální metody a expended formuláře pro jeden `object` parametru.</span><span class="sxs-lookup"><span data-stu-id="56f2d-853">When the type of a parameter array is `object[]`, a potential ambiguity arises between the normal form of the method and the expended form for a single `object` parameter.</span></span> <span data-ttu-id="56f2d-854">Důvod nejednoznačnost je, že `object[]` je sama o sobě implicitně převést na typ `object`.</span><span class="sxs-lookup"><span data-stu-id="56f2d-854">The reason for the ambiguity is that an `object[]` is itself implicitly convertible to type `object`.</span></span> <span data-ttu-id="56f2d-855">Nejednoznačnosti uvede není problém, ale vzhledem k tomu, že se dá vyřešit tak, že v případě potřeby vloží přetypování.</span><span class="sxs-lookup"><span data-stu-id="56f2d-855">The ambiguity presents no problem, however, since it can be resolved by inserting a cast if needed.</span></span>

<span data-ttu-id="56f2d-856">V příkladu</span><span class="sxs-lookup"><span data-stu-id="56f2d-856">The example</span></span>
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
<span data-ttu-id="56f2d-857">Vytvoří výstup</span><span class="sxs-lookup"><span data-stu-id="56f2d-857">produces the output</span></span>
```
System.Int32 System.String System.Double
System.Object[]
System.Object[]
System.Int32 System.String System.Double
```

<span data-ttu-id="56f2d-858">V prvním a posledním volání `F`, běžné formě `F` platí, protože existuje implicitní převod z typu argumentu na typ parametru (obojí jsou typu `object[]`).</span><span class="sxs-lookup"><span data-stu-id="56f2d-858">In the first and last invocations of `F`, the normal form of `F` is applicable because an implicit conversion exists from the argument type to the parameter type (both are of type `object[]`).</span></span> <span data-ttu-id="56f2d-859">Díky tomu se řešení přetížení vybere běžné formě `F`, a argument je předán jako parametr regulární hodnoty.</span><span class="sxs-lookup"><span data-stu-id="56f2d-859">Thus, overload resolution selects the normal form of `F`, and the argument is passed as a regular value parameter.</span></span> <span data-ttu-id="56f2d-860">V druhé a třetí volání, běžné formě `F` se nedá použít, protože neexistuje žádný implicitní převod z typu argumentu na typ parametru (typ `object` nejde implicitně převést na typ `object[]`).</span><span class="sxs-lookup"><span data-stu-id="56f2d-860">In the second and third invocations, the normal form of `F` is not applicable because no implicit conversion exists from the argument type to the parameter type (type `object` cannot be implicitly converted to type `object[]`).</span></span> <span data-ttu-id="56f2d-861">Ale rozšířené formu `F` se vztahuje, takže se zvolila řešení přetížení.</span><span class="sxs-lookup"><span data-stu-id="56f2d-861">However, the expanded form of `F` is applicable, so it is selected by overload resolution.</span></span> <span data-ttu-id="56f2d-862">V důsledku toho prvek jedním `object[]` vytvoří při vyvolání, a jeden prvek pole je inicializován s hodnotou daný argument (které sám o sobě představuje odkaz na `object[]`).</span><span class="sxs-lookup"><span data-stu-id="56f2d-862">As a result, a one-element `object[]` is created by the invocation, and the single element of the array is initialized with the given argument value (which itself is a reference to an `object[]`).</span></span>

### <a name="static-and-instance-methods"></a><span data-ttu-id="56f2d-863">Statické a instance metody</span><span class="sxs-lookup"><span data-stu-id="56f2d-863">Static and instance methods</span></span>

<span data-ttu-id="56f2d-864">Pokud deklarace metody obsahuje `static` modifikátor, že metoda je označen jako statickou metodu.</span><span class="sxs-lookup"><span data-stu-id="56f2d-864">When a method declaration includes a `static` modifier, that method is said to be a static method.</span></span> <span data-ttu-id="56f2d-865">Pokud ne `static` Modifikátor je k dispozici, metodu je označen jako metoda instance.</span><span class="sxs-lookup"><span data-stu-id="56f2d-865">When no `static` modifier is present, the method is said to be an instance method.</span></span>

<span data-ttu-id="56f2d-866">Statická metoda nefunguje na konkrétní instanci a je chyba kompilace k odkazování na `this` uvnitř statické metody.</span><span class="sxs-lookup"><span data-stu-id="56f2d-866">A static method does not operate on a specific instance, and it is a compile-time error to refer to `this` in a static method.</span></span>

<span data-ttu-id="56f2d-867">Metoda instance pracuje instanci dané třídy, a tato instance je přístupná jako `this` ([tento přístup](expressions.md#this-access)).</span><span class="sxs-lookup"><span data-stu-id="56f2d-867">An instance method operates on a given instance of a class, and that instance can be accessed as `this` ([This access](expressions.md#this-access)).</span></span>

<span data-ttu-id="56f2d-868">Když metoda odkazuje *member_access* ([přístup ke členu](expressions.md#member-access)) formuláře `E.M`, pokud `M` je statická metoda, `E` musí označují typ obsahující `M`a pokud `M` je metoda instance, `E` musí označit instanci typu, který obsahuje `M`.</span><span class="sxs-lookup"><span data-stu-id="56f2d-868">When a method is referenced in a *member_access* ([Member access](expressions.md#member-access)) of the form `E.M`, if `M` is a static method, `E` must denote a type containing `M`, and if `M` is an instance method, `E` must denote an instance of a type containing `M`.</span></span>

<span data-ttu-id="56f2d-869">Rozdíly mezi statické a členy instance jsou popsány dále v [statické a instance členy](classes.md#static-and-instance-members).</span><span class="sxs-lookup"><span data-stu-id="56f2d-869">The differences between static and instance members are discussed further in [Static and instance members](classes.md#static-and-instance-members).</span></span>

### <a name="virtual-methods"></a><span data-ttu-id="56f2d-870">Virtuální metody</span><span class="sxs-lookup"><span data-stu-id="56f2d-870">Virtual methods</span></span>

<span data-ttu-id="56f2d-871">Pokud obsahuje deklaraci metody instance `virtual` modifikátor, že metoda je označen jako virtuální metody.</span><span class="sxs-lookup"><span data-stu-id="56f2d-871">When an instance method declaration includes a `virtual` modifier, that method is said to be a virtual method.</span></span> <span data-ttu-id="56f2d-872">Pokud ne `virtual` Modifikátor je k dispozici, metodu se říká, že jako nevirtuální metodu.</span><span class="sxs-lookup"><span data-stu-id="56f2d-872">When no `virtual` modifier is present, the method is said to be a non-virtual method.</span></span>

<span data-ttu-id="56f2d-873">Implementace nevirtuální metody je invariantní: implementace je stejný, ať je metoda vyvolána na instanci třídy ve kterém je deklarována nebo instanci odvozené třídy.</span><span class="sxs-lookup"><span data-stu-id="56f2d-873">The implementation of a non-virtual method is invariant: The implementation is the same whether the method is invoked on an instance of the class in which it is declared or an instance of a derived class.</span></span> <span data-ttu-id="56f2d-874">Naproti tomu implementace virtuální metoda může být potlačeno z odvozených tříd.</span><span class="sxs-lookup"><span data-stu-id="56f2d-874">In contrast, the implementation of a virtual method can be superseded by derived classes.</span></span> <span data-ttu-id="56f2d-875">Nahrazování provádění zděděnou virtuální metodu proces se označuje jako ***přepsání*** metody ([přepište metody](classes.md#override-methods)).</span><span class="sxs-lookup"><span data-stu-id="56f2d-875">The process of superseding the implementation of an inherited virtual method is known as ***overriding*** that method ([Override methods](classes.md#override-methods)).</span></span>

<span data-ttu-id="56f2d-876">Ve volání virtuální metody ***run-time typu*** instance, pro který přebírá tento vyvolání místo určuje implementaci skutečné metoda k vyvolání.</span><span class="sxs-lookup"><span data-stu-id="56f2d-876">In a virtual method invocation, the ***run-time type*** of the instance for which that invocation takes place determines the actual method implementation to invoke.</span></span> <span data-ttu-id="56f2d-877">Ve volání nevirtuální metody ***typu v době kompilace*** instance je určujícím faktorem.</span><span class="sxs-lookup"><span data-stu-id="56f2d-877">In a non-virtual method invocation, the ***compile-time type*** of the instance is the determining factor.</span></span> <span data-ttu-id="56f2d-878">Přesně řečeno, pokud metodu s názvem `N` vyvolání se seznamem argumentů `A` instance s typem kompilace `C` a run-time typu `R` (kde `R` je buď `C` nebo třída odvozená z `C`), volání se zpracovává následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="56f2d-878">In precise terms, when a method named `N` is invoked with an argument list `A` on an instance with a compile-time type `C` and a run-time type `R` (where `R` is either `C` or a class derived from `C`), the invocation is processed as follows:</span></span>

*  <span data-ttu-id="56f2d-879">Nejprve řešení přetížení se použije pro `C`, `N`, a `A`, vyberte konkrétní metody `M` ze sady metody deklarované v a dědí `C`.</span><span class="sxs-lookup"><span data-stu-id="56f2d-879">First, overload resolution is applied to `C`, `N`, and `A`, to select a specific method `M` from the set of methods declared in and inherited by `C`.</span></span> <span data-ttu-id="56f2d-880">To je popsáno v [volání metod](expressions.md#method-invocations).</span><span class="sxs-lookup"><span data-stu-id="56f2d-880">This is described in [Method invocations](expressions.md#method-invocations).</span></span>
*  <span data-ttu-id="56f2d-881">Když se poté `M` nevirtuální metoda `M` je vyvolána.</span><span class="sxs-lookup"><span data-stu-id="56f2d-881">Then, if `M` is a non-virtual method, `M` is invoked.</span></span>
*  <span data-ttu-id="56f2d-882">V opačném případě `M` virtuální metody a implementace Většina odvozených z `M` s ohledem na `R` je vyvolána.</span><span class="sxs-lookup"><span data-stu-id="56f2d-882">Otherwise, `M` is a virtual method, and the most derived implementation of `M` with respect to `R` is invoked.</span></span>

<span data-ttu-id="56f2d-883">Pro každý virtuální metody deklarované v nebo zděděná z třídy existuje ***nejvíce odvozenému implementace*** metody s ohledem na tuto třídu.</span><span class="sxs-lookup"><span data-stu-id="56f2d-883">For every virtual method declared in or inherited by a class, there exists a ***most derived implementation*** of the method with respect to that class.</span></span> <span data-ttu-id="56f2d-884">Implementace Většina odvozených virtuální metody `M` s ohledem na třídu `R` je stanoven následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="56f2d-884">The most derived implementation of a virtual method `M` with respect to a class `R` is determined as follows:</span></span>

*  <span data-ttu-id="56f2d-885">Pokud `R` obsahuje umístění `virtual` deklarace `M`, pak toto je implementace Většina odvozených z `M`.</span><span class="sxs-lookup"><span data-stu-id="56f2d-885">If `R` contains the introducing `virtual` declaration of `M`, then this is the most derived implementation of `M`.</span></span>
*  <span data-ttu-id="56f2d-886">Jinak, pokud `R` obsahuje `override` z `M`, pak toto je implementace Většina odvozených z `M`.</span><span class="sxs-lookup"><span data-stu-id="56f2d-886">Otherwise, if `R` contains an `override` of `M`, then this is the most derived implementation of `M`.</span></span>
*  <span data-ttu-id="56f2d-887">V opačném případě nejvíce odvozené provádění `M` s ohledem na `R` je stejná jako implementace Většina odvozených z `M` s ohledem na přímé základní třídy `R`.</span><span class="sxs-lookup"><span data-stu-id="56f2d-887">Otherwise, the most derived implementation of `M` with respect to `R` is the same as the most derived implementation of `M` with respect to the direct base class of `R`.</span></span>

<span data-ttu-id="56f2d-888">Následující příklad ukazuje rozdíly mezi virtuální a nevirtuální metody:</span><span class="sxs-lookup"><span data-stu-id="56f2d-888">The following example illustrates the differences between virtual and non-virtual methods:</span></span>
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

<span data-ttu-id="56f2d-889">V tomto příkladu `A` zavádí nevirtuální metoda `F` a virtuální metody `G`.</span><span class="sxs-lookup"><span data-stu-id="56f2d-889">In the example, `A` introduces a non-virtual method `F` and a virtual method `G`.</span></span> <span data-ttu-id="56f2d-890">Třída `B` představuje nový způsob nevirtuální `F`, proto děděné skrytí `F`a také přepsání zděděné metody `G`.</span><span class="sxs-lookup"><span data-stu-id="56f2d-890">The class `B` introduces a new non-virtual method `F`, thus hiding the inherited `F`, and also overrides the inherited method `G`.</span></span> <span data-ttu-id="56f2d-891">Vzorové postupy výstupu:</span><span class="sxs-lookup"><span data-stu-id="56f2d-891">The example produces the output:</span></span>
```
A.F
B.F
B.G
B.G
```

<span data-ttu-id="56f2d-892">Všimněte si, že příkaz `a.G()` vyvolá `B.G`, nikoli `A.G`.</span><span class="sxs-lookup"><span data-stu-id="56f2d-892">Notice that the statement `a.G()` invokes `B.G`, not `A.G`.</span></span> <span data-ttu-id="56f2d-893">Důvodem je, že run-time typu instance (což je `B`), ne za kompilace typ instance (což je `A`), určuje implementaci skutečné metoda k vyvolání.</span><span class="sxs-lookup"><span data-stu-id="56f2d-893">This is because the run-time type of the instance (which is `B`), not the compile-time type of the instance (which is `A`), determines the actual method implementation to invoke.</span></span>

<span data-ttu-id="56f2d-894">Protože jsou povoleny metody ke skrytí zděděné metody, je možné pro třídu obsahuje několik virtuálních metod se stejným podpisem.</span><span class="sxs-lookup"><span data-stu-id="56f2d-894">Because methods are allowed to hide inherited methods, it is possible for a class to contain several virtual methods with the same signature.</span></span> <span data-ttu-id="56f2d-895">Tím nedojde k žádnému kvůli problému nejednoznačnost od všech polí kromě nejvíce Odvozená metoda jsou skryté.</span><span class="sxs-lookup"><span data-stu-id="56f2d-895">This does not present an ambiguity problem, since all but the most derived method are hidden.</span></span> <span data-ttu-id="56f2d-896">V příkladu</span><span class="sxs-lookup"><span data-stu-id="56f2d-896">In the example</span></span>
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
<span data-ttu-id="56f2d-897">`C` a `D` třídy obsahovat dvě virtuální metody se stejnou signaturou: je zavedený `A` a ten zavedené `C`.</span><span class="sxs-lookup"><span data-stu-id="56f2d-897">the `C` and `D` classes contain two virtual methods with the same signature: The one introduced by `A` and the one introduced by `C`.</span></span> <span data-ttu-id="56f2d-898">Metody zavedené `C` skryje metody zděděné z `A`.</span><span class="sxs-lookup"><span data-stu-id="56f2d-898">The method introduced by `C` hides the method inherited from `A`.</span></span> <span data-ttu-id="56f2d-899">Proto přepsání deklarace v `D` přepisuje deklaraci metody zavedené `C`, a není možné pro `D` přepsat metodu zavedené `A`.</span><span class="sxs-lookup"><span data-stu-id="56f2d-899">Thus, the override declaration in `D` overrides the method introduced by `C`, and it is not possible for `D` to override the method introduced by `A`.</span></span> <span data-ttu-id="56f2d-900">Vzorové postupy výstupu:</span><span class="sxs-lookup"><span data-stu-id="56f2d-900">The example produces the output:</span></span>
```
B.F
B.F
D.F
D.F
```

<span data-ttu-id="56f2d-901">Všimněte si, že je možné vyvolat skryté virtuální metody, přístup k instanci `D` prostřednictvím méně odvozený typ, ve kterém není skrytý metodu.</span><span class="sxs-lookup"><span data-stu-id="56f2d-901">Note that it is possible to invoke the hidden virtual method by accessing an instance of `D` through a less derived type in which the method is not hidden.</span></span>

### <a name="override-methods"></a><span data-ttu-id="56f2d-902">Přepište metody</span><span class="sxs-lookup"><span data-stu-id="56f2d-902">Override methods</span></span>

<span data-ttu-id="56f2d-903">Při deklaraci instance metody obsahuje `override` modifikátor, metoda se říká, že ***přepsat metodu***.</span><span class="sxs-lookup"><span data-stu-id="56f2d-903">When an instance method declaration includes an `override` modifier, the method is said to be an ***override method***.</span></span> <span data-ttu-id="56f2d-904">To metoda override přepsání zděděné virtuální metody se stejným podpisem.</span><span class="sxs-lookup"><span data-stu-id="56f2d-904">An override method overrides an inherited virtual method with the same signature.</span></span> <span data-ttu-id="56f2d-905">Vzhledem k tomu virtuální metoda deklarace zavádí nové metody, specializuje deklaraci metody přepsání existující zděděnou virtuální metodu tím, že poskytuje novou implementaci této metody.</span><span class="sxs-lookup"><span data-stu-id="56f2d-905">Whereas a virtual method declaration introduces a new method, an override method declaration specializes an existing inherited virtual method by providing a new implementation of that method.</span></span>

<span data-ttu-id="56f2d-906">Metoda přepsat `override` prohlášení je označován jako ***přepsat základní metodu***.</span><span class="sxs-lookup"><span data-stu-id="56f2d-906">The method overridden by an `override` declaration is known as the ***overridden base method***.</span></span> <span data-ttu-id="56f2d-907">Pro to metoda override `M` deklarovat v třídě `C`, přepsané základní metoda je určen tím, že kontroluje každý typ základní třídy `C`začíná s přímou základní třídu typu `C` a pokračuje s každou po sobě jdoucích Typ přímé základní třídy, do dané základní třídy typu alespoň jedna je přístupná metoda nachází, který má stejnou signaturu jako `M` po nahrazení argumentů typu.</span><span class="sxs-lookup"><span data-stu-id="56f2d-907">For an override method `M` declared in a class `C`, the overridden base method is determined by examining each base class type of `C`, starting with the direct base class type of `C` and continuing with each successive direct base class type, until in a given base class type at least one accessible method is located which has the same signature as `M` after substitution of type arguments.</span></span> <span data-ttu-id="56f2d-908">Pro účely vyhledávání přepsané základní metody, metoda se považuje za dostupné v případě, že je `public`, pokud je `protected`, pokud je `protected internal`, nebo je-li `internal` a deklarovat ve stejném programu jako `C`.</span><span class="sxs-lookup"><span data-stu-id="56f2d-908">For the purposes of locating the overridden base method, a method is considered accessible if it is `public`, if it is `protected`, if it is `protected internal`, or if it is `internal` and declared in the same program as `C`.</span></span>

<span data-ttu-id="56f2d-909">Chyba kompilace nastane, pokud jsou splněny pro deklaraci přepsat všechny z následujících akcí:</span><span class="sxs-lookup"><span data-stu-id="56f2d-909">A compile-time error occurs unless all of the following are true for an override declaration:</span></span>

*  <span data-ttu-id="56f2d-910">Přepsané základní metoda může být umístěn, jak je popsáno výše.</span><span class="sxs-lookup"><span data-stu-id="56f2d-910">An overridden base method can be located as described above.</span></span>
*  <span data-ttu-id="56f2d-911">Existuje přesně jeden takový přepsané základní metodě.</span><span class="sxs-lookup"><span data-stu-id="56f2d-911">There is exactly one such overridden base method.</span></span> <span data-ttu-id="56f2d-912">Toto omezení nemá vliv, pouze v případě, že typ základní třídy je konstruovaný typ. Pokud nahrazení argumentů typu díky podpis dvě metody stejné.</span><span class="sxs-lookup"><span data-stu-id="56f2d-912">This restriction has effect only if the base class type is a constructed type where the substitution of type arguments makes the signature of two methods the same.</span></span>
*  <span data-ttu-id="56f2d-913">Přepsané základní metoda je virtuální, abstraktní nebo přepište metodu.</span><span class="sxs-lookup"><span data-stu-id="56f2d-913">The overridden base method is a virtual, abstract, or override method.</span></span> <span data-ttu-id="56f2d-914">Jinými slovy přepsané základní metoda nemůže být statická nebo nevirtuální.</span><span class="sxs-lookup"><span data-stu-id="56f2d-914">In other words, the overridden base method cannot be static or non-virtual.</span></span>
*  <span data-ttu-id="56f2d-915">Přepsané základní metoda není zapečetěné metody.</span><span class="sxs-lookup"><span data-stu-id="56f2d-915">The overridden base method is not a sealed method.</span></span>
*  <span data-ttu-id="56f2d-916">Metoda přepsání a přepsané základní metody mají vracet hodnotu stejného typu.</span><span class="sxs-lookup"><span data-stu-id="56f2d-916">The override method and the overridden base method have the same return type.</span></span>
*  <span data-ttu-id="56f2d-917">Přepsání deklarace a přepsané základní metody mají stejné deklarovaná přístupnost.</span><span class="sxs-lookup"><span data-stu-id="56f2d-917">The override declaration and the overridden base method have the same declared accessibility.</span></span> <span data-ttu-id="56f2d-918">Jinými slovy přepsání deklarace nemůže změnit přístupnost virtuální metody.</span><span class="sxs-lookup"><span data-stu-id="56f2d-918">In other words, an override declaration cannot change the accessibility of the virtual method.</span></span> <span data-ttu-id="56f2d-919">Ale pokud přepsané základní metoda je interní chráněné a je deklarována v jiném sestavení než sestavení obsahující metodu přepsání metody pak přepsání metody deklarované usnadnění musí být chráněné.</span><span class="sxs-lookup"><span data-stu-id="56f2d-919">However, if the overridden base method is protected internal and it is declared in a different assembly than the assembly containing the override method then the override method's declared accessibility must be protected.</span></span>
*  <span data-ttu-id="56f2d-920">Přepsání deklarace není určen typ parametru omezení klauzule.</span><span class="sxs-lookup"><span data-stu-id="56f2d-920">The override declaration does not specify type-parameter-constraints-clauses.</span></span> <span data-ttu-id="56f2d-921">Místo toho omezení dědí z přepsané základní metodě.</span><span class="sxs-lookup"><span data-stu-id="56f2d-921">Instead the constraints are inherited from the overridden base method.</span></span> <span data-ttu-id="56f2d-922">Všimněte si, že omezení, která jsou parametry typu v metodě přepsané mohou být nahrazeny argumenty typu v zděděné omezení.</span><span class="sxs-lookup"><span data-stu-id="56f2d-922">Note that constraints that are type parameters in the overridden method may be replaced by type arguments in the inherited constraint.</span></span> <span data-ttu-id="56f2d-923">To může vést k omezení, která nejsou povoleny, pokud explicitně zadán, jako jsou typy hodnot nebo zapečetěné typy.</span><span class="sxs-lookup"><span data-stu-id="56f2d-923">This can lead to constraints that are not legal when explicitly specified, such as value types or sealed types.</span></span>

<span data-ttu-id="56f2d-924">Následující příklad ukazuje, jak fungují přepsání pravidel pro obecné třídy:</span><span class="sxs-lookup"><span data-stu-id="56f2d-924">The following example demonstrates how the overriding rules work for generic classes:</span></span>
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

<span data-ttu-id="56f2d-925">Deklarace přepsání můžete přistupovat, pomocí přepsané základní metody *base_access* ([základní přístup](expressions.md#base-access)).</span><span class="sxs-lookup"><span data-stu-id="56f2d-925">An override declaration can access the overridden base method using a *base_access* ([Base access](expressions.md#base-access)).</span></span> <span data-ttu-id="56f2d-926">V příkladu</span><span class="sxs-lookup"><span data-stu-id="56f2d-926">In the example</span></span>
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
<span data-ttu-id="56f2d-927">`base.PrintFields()` volání `B` vyvolá `PrintFields` metody deklarované v `A`.</span><span class="sxs-lookup"><span data-stu-id="56f2d-927">the `base.PrintFields()` invocation in `B` invokes the `PrintFields` method declared in `A`.</span></span> <span data-ttu-id="56f2d-928">A *base_access* zakáže mechanismus volání virtuální a základní metoda jednoduše považuje za nevirtuální metody.</span><span class="sxs-lookup"><span data-stu-id="56f2d-928">A *base_access* disables the virtual invocation mechanism and simply treats the base method as a non-virtual method.</span></span> <span data-ttu-id="56f2d-929">Došlo při vyvolání v `B` byla zapsána `((A)this).PrintFields()`, by rekurzivně vyvolat `PrintFields` metody deklarované v `B`, není ten deklarované v `A`, protože `PrintFields` je virtuální a za běhu typu `((A)this)` je `B`.</span><span class="sxs-lookup"><span data-stu-id="56f2d-929">Had the invocation in `B` been written `((A)this).PrintFields()`, it would recursively invoke the `PrintFields` method declared in `B`, not the one declared in `A`, since `PrintFields` is virtual and the run-time type of `((A)this)` is `B`.</span></span>

<span data-ttu-id="56f2d-930">Pouze zahrnutím `override` může Modifikátor metody přepsání jinou metodu.</span><span class="sxs-lookup"><span data-stu-id="56f2d-930">Only by including an `override` modifier can a method override another method.</span></span> <span data-ttu-id="56f2d-931">Ve všech ostatních případech metodu se stejnou signaturou jako zděděnou metodu jednoduše skryje zděděné metody.</span><span class="sxs-lookup"><span data-stu-id="56f2d-931">In all other cases, a method with the same signature as an inherited method simply hides the inherited method.</span></span> <span data-ttu-id="56f2d-932">V příkladu</span><span class="sxs-lookup"><span data-stu-id="56f2d-932">In the example</span></span>
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
<span data-ttu-id="56f2d-933">`F` metoda ve `B` nezahrnuje `override` modifikátor, takže nedojde k přepsání `F` metoda ve `A`.</span><span class="sxs-lookup"><span data-stu-id="56f2d-933">the `F` method in `B` does not include an `override` modifier and therefore does not override the `F` method in `A`.</span></span> <span data-ttu-id="56f2d-934">Místo toho `F` metoda `B` skrývá metodu v `A`, a upozornění je hlášeno, protože neobsahuje deklaraci `new` modifikátor.</span><span class="sxs-lookup"><span data-stu-id="56f2d-934">Rather, the `F` method in `B` hides the method in `A`, and a warning is reported because the declaration does not include a `new` modifier.</span></span>

<span data-ttu-id="56f2d-935">V příkladu</span><span class="sxs-lookup"><span data-stu-id="56f2d-935">In the example</span></span>
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
<span data-ttu-id="56f2d-936">`F` metoda `B` skryje virtuální `F` metody zděděné z `A`.</span><span class="sxs-lookup"><span data-stu-id="56f2d-936">the `F` method in `B` hides the virtual `F` method inherited from `A`.</span></span> <span data-ttu-id="56f2d-937">Od nové `F` v `B` má privátní přístup, její obor pouze obsahuje tělo třídy `B` a nerozšiřuje `C`.</span><span class="sxs-lookup"><span data-stu-id="56f2d-937">Since the new `F` in `B` has private access, its scope only includes the class body of `B` and does not extend to `C`.</span></span> <span data-ttu-id="56f2d-938">Proto deklarace `F` v `C` smí přepsat `F` zděděno z `A`.</span><span class="sxs-lookup"><span data-stu-id="56f2d-938">Therefore, the declaration of `F` in `C` is permitted to override the `F` inherited from `A`.</span></span>

### <a name="sealed-methods"></a><span data-ttu-id="56f2d-939">Zapečetěné metody</span><span class="sxs-lookup"><span data-stu-id="56f2d-939">Sealed methods</span></span>

<span data-ttu-id="56f2d-940">Při deklaraci instance metody obsahuje `sealed` modifikátor, že metoda je říká, že ***zapečetěné metody***.</span><span class="sxs-lookup"><span data-stu-id="56f2d-940">When an instance method declaration includes a `sealed` modifier, that method is said to be a ***sealed method***.</span></span> <span data-ttu-id="56f2d-941">Pokud obsahuje deklaraci metody instance `sealed` modifikátor, musí také zahrnovat `override` modifikátor.</span><span class="sxs-lookup"><span data-stu-id="56f2d-941">If an instance method declaration includes the  `sealed` modifier, it must also include the `override` modifier.</span></span> <span data-ttu-id="56f2d-942">Použití `sealed` modifikátor zabrání další přetížení metody odvozené třídy.</span><span class="sxs-lookup"><span data-stu-id="56f2d-942">Use of the `sealed` modifier prevents a derived class from further overriding the method.</span></span>

<span data-ttu-id="56f2d-943">V příkladu</span><span class="sxs-lookup"><span data-stu-id="56f2d-943">In the example</span></span>
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
<span data-ttu-id="56f2d-944">třídy `B` poskytuje dvě přepište metody: `F` metodu, která má `sealed` modifikátor a `G` metoda, která neexistuje.</span><span class="sxs-lookup"><span data-stu-id="56f2d-944">the class `B` provides two override methods: an `F` method that has the `sealed` modifier and a `G` method that does not.</span></span> <span data-ttu-id="56f2d-945">`B`využití zapečetěná `modifier` brání `C` další možnost obejít `F`.</span><span class="sxs-lookup"><span data-stu-id="56f2d-945">`B`'s use of the sealed `modifier` prevents `C` from further overriding `F`.</span></span>

### <a name="abstract-methods"></a><span data-ttu-id="56f2d-946">Abstraktní metody</span><span class="sxs-lookup"><span data-stu-id="56f2d-946">Abstract methods</span></span>

<span data-ttu-id="56f2d-947">Při deklaraci instance metody obsahuje `abstract` modifikátor, že metoda je říká, že ***abstraktní metoda***.</span><span class="sxs-lookup"><span data-stu-id="56f2d-947">When an instance method declaration includes an `abstract` modifier, that method is said to be an ***abstract method***.</span></span> <span data-ttu-id="56f2d-948">I když abstraktní metoda je implicitně také virtuální metody, nemůže mít modifikátor `virtual`.</span><span class="sxs-lookup"><span data-stu-id="56f2d-948">Although an abstract method is implicitly also a virtual method, it cannot have the modifier `virtual`.</span></span>

<span data-ttu-id="56f2d-949">Deklarace abstraktní metody zavádí novou virtuální metodou, ale neposkytuje implementaci této metody.</span><span class="sxs-lookup"><span data-stu-id="56f2d-949">An abstract method declaration introduces a new virtual method but does not provide an implementation of that method.</span></span> <span data-ttu-id="56f2d-950">Odvozené neabstraktní třídy místo toho je potřeba poskytli vlastní implementaci přepsáním této metody.</span><span class="sxs-lookup"><span data-stu-id="56f2d-950">Instead, non-abstract derived classes are required to provide their own implementation by overriding that method.</span></span> <span data-ttu-id="56f2d-951">Protože abstraktní metody poskytuje skutečné implementaci, *method_body* abstraktní metody jednoduše se skládá ze středníku.</span><span class="sxs-lookup"><span data-stu-id="56f2d-951">Because an abstract method provides no actual implementation, the *method_body* of an abstract method simply consists of a semicolon.</span></span>

<span data-ttu-id="56f2d-952">Deklarace abstraktní metody jsou povolené jenom v abstraktní třídy ([abstraktní třídy](classes.md#abstract-classes)).</span><span class="sxs-lookup"><span data-stu-id="56f2d-952">Abstract method declarations are only permitted in abstract classes ([Abstract classes](classes.md#abstract-classes)).</span></span>

<span data-ttu-id="56f2d-953">V příkladu</span><span class="sxs-lookup"><span data-stu-id="56f2d-953">In the example</span></span>
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
<span data-ttu-id="56f2d-954">`Shape` třída používá koncept abstraktní geometrický tvar objektu, který vlastní vykreslení.</span><span class="sxs-lookup"><span data-stu-id="56f2d-954">the `Shape` class defines the abstract notion of a geometrical shape object that can paint itself.</span></span> <span data-ttu-id="56f2d-955">`Paint` Metoda je abstraktní, protože neexistuje žádné smysluplné výchozí implementaci.</span><span class="sxs-lookup"><span data-stu-id="56f2d-955">The `Paint` method is abstract because there is no meaningful default implementation.</span></span> <span data-ttu-id="56f2d-956">`Ellipse` a `Box` třídy jsou konkrétní `Shape` implementace.</span><span class="sxs-lookup"><span data-stu-id="56f2d-956">The `Ellipse` and `Box` classes are concrete `Shape` implementations.</span></span> <span data-ttu-id="56f2d-957">Protože tyto třídy jsou neabstraktní, jsou nutné k přepsání `Paint` metoda a poskytnout skutečné implementaci.</span><span class="sxs-lookup"><span data-stu-id="56f2d-957">Because these classes are non-abstract, they are required to override the `Paint` method and provide an actual implementation.</span></span>

<span data-ttu-id="56f2d-958">Je chyba kompilace pro *base_access* ([základní přístup](expressions.md#base-access)) tak, aby odkazovaly abstraktní metody.</span><span class="sxs-lookup"><span data-stu-id="56f2d-958">It is a compile-time error for a *base_access* ([Base access](expressions.md#base-access)) to reference an abstract method.</span></span> <span data-ttu-id="56f2d-959">V příkladu</span><span class="sxs-lookup"><span data-stu-id="56f2d-959">In the example</span></span>
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
<span data-ttu-id="56f2d-960">oznámí se chyba kompilace pro `base.F()` vyvolání protože odkazuje na abstraktní metody.</span><span class="sxs-lookup"><span data-stu-id="56f2d-960">a compile-time error is reported for the `base.F()` invocation because it references an abstract method.</span></span>

<span data-ttu-id="56f2d-961">Pokud chcete přepsat virtuální metodu smí obsahovat deklarace abstraktní metody.</span><span class="sxs-lookup"><span data-stu-id="56f2d-961">An abstract method declaration is permitted to override a virtual method.</span></span> <span data-ttu-id="56f2d-962">Tato možnost povoluje abstraktní třídu pro vynucení opětovná implementace metody v odvozených třídách a znepřístupní původní implementaci metody.</span><span class="sxs-lookup"><span data-stu-id="56f2d-962">This allows an abstract class to force re-implementation of the method in derived classes, and makes the original implementation of the method unavailable.</span></span> <span data-ttu-id="56f2d-963">V příkladu</span><span class="sxs-lookup"><span data-stu-id="56f2d-963">In the example</span></span>
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
<span data-ttu-id="56f2d-964">Třída `A` deklaruje virtuální metody, třídy `B` přepíše tuto metodu pomocí abstraktní metody a třídy `C` přepíše abstraktní metodu a zadejte vlastní implementaci.</span><span class="sxs-lookup"><span data-stu-id="56f2d-964">class `A` declares a virtual method, class `B` overrides this method with an abstract method, and class `C` overrides the abstract method to provide its own implementation.</span></span>

### <a name="external-methods"></a><span data-ttu-id="56f2d-965">Externí metody</span><span class="sxs-lookup"><span data-stu-id="56f2d-965">External methods</span></span>

<span data-ttu-id="56f2d-966">Pokud deklarace metody obsahuje `extern` modifikátor, že metoda je říká, že ***externí metoda***.</span><span class="sxs-lookup"><span data-stu-id="56f2d-966">When a method declaration includes an `extern` modifier, that method is said to be an ***external method***.</span></span> <span data-ttu-id="56f2d-967">Externí metody jsou implementovány externě, obvykle pomocí jiného jazyka než C#.</span><span class="sxs-lookup"><span data-stu-id="56f2d-967">External methods are implemented externally, typically using a language other than C#.</span></span> <span data-ttu-id="56f2d-968">Protože deklaraci externí metody poskytuje skutečné implementaci, *method_body* externí metody jednoduše se skládá ze středníku.</span><span class="sxs-lookup"><span data-stu-id="56f2d-968">Because an external method declaration provides no actual implementation, the *method_body* of an external method simply consists of a semicolon.</span></span> <span data-ttu-id="56f2d-969">Externí metoda nemusí být obecný.</span><span class="sxs-lookup"><span data-stu-id="56f2d-969">An external method may not be generic.</span></span>

<span data-ttu-id="56f2d-970">`extern` Modifikátor se obvykle používá ve spojení s `DllImport` atribut ([vzájemná spolupráce s komponentami COM a Win32](attributes.md#interoperation-with-com-and-win32-components)), což externí metody k implementaci knihovny DLL (Dynamic Link Libraries).</span><span class="sxs-lookup"><span data-stu-id="56f2d-970">The `extern` modifier is typically used in conjunction with a `DllImport` attribute ([Interoperation with COM and Win32 components](attributes.md#interoperation-with-com-and-win32-components)), allowing external methods to be implemented by DLLs (Dynamic Link Libraries).</span></span> <span data-ttu-id="56f2d-971">Spouštěcí prostředí můžou podporovat jiné mechanismy, které je možné poskytnout implementace metod externí.</span><span class="sxs-lookup"><span data-stu-id="56f2d-971">The execution environment may support other mechanisms whereby implementations of external methods can be provided.</span></span>

<span data-ttu-id="56f2d-972">Pokud obsahuje externí metody `DllImport` atribut deklarace metody musí také zahrnovat `static` modifikátor.</span><span class="sxs-lookup"><span data-stu-id="56f2d-972">When an external method includes a `DllImport` attribute, the method declaration must also include a `static` modifier.</span></span> <span data-ttu-id="56f2d-973">Tento příklad ukazuje použití `extern` modifikátor a `DllImport` atribut:</span><span class="sxs-lookup"><span data-stu-id="56f2d-973">This example demonstrates the use of the `extern` modifier and the `DllImport` attribute:</span></span>
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

### <a name="partial-methods-recap"></a><span data-ttu-id="56f2d-974">Částečné metody (rekapitulace)</span><span class="sxs-lookup"><span data-stu-id="56f2d-974">Partial methods (recap)</span></span>

<span data-ttu-id="56f2d-975">Pokud deklarace metody obsahuje `partial` modifikátor, že metoda je říká, že ***částečná metoda***.</span><span class="sxs-lookup"><span data-stu-id="56f2d-975">When a method declaration includes a `partial` modifier, that method is said to be a ***partial method***.</span></span> <span data-ttu-id="56f2d-976">Částečné metody se dají deklarovat jenom jako členy částečné typy ([částečné typy](classes.md#partial-types)) a můžou se několik omezení.</span><span class="sxs-lookup"><span data-stu-id="56f2d-976">Partial methods can only be declared as members of partial types ([Partial types](classes.md#partial-types)), and are subject to a number of restrictions.</span></span> <span data-ttu-id="56f2d-977">Částečné metody jsou popsané v [částečné metody](classes.md#partial-methods).</span><span class="sxs-lookup"><span data-stu-id="56f2d-977">Partial methods are further described in [Partial methods](classes.md#partial-methods).</span></span>

### <a name="extension-methods"></a><span data-ttu-id="56f2d-978">Rozšiřující metody</span><span class="sxs-lookup"><span data-stu-id="56f2d-978">Extension methods</span></span>

<span data-ttu-id="56f2d-979">Pokud je první parametr metody obsahuje `this` modifikátor, že metoda je říká, že ***– metoda rozšíření***.</span><span class="sxs-lookup"><span data-stu-id="56f2d-979">When the first parameter of a method includes the `this` modifier, that method is said to be an ***extension method***.</span></span> <span data-ttu-id="56f2d-980">Metody rozšíření lze deklarovat pouze v obecné, bez vnoření statické třídy.</span><span class="sxs-lookup"><span data-stu-id="56f2d-980">Extension methods can only be declared in non-generic, non-nested static classes.</span></span> <span data-ttu-id="56f2d-981">První parametr metody rozšíření může mít jiné než žádné modifikátory `this`, a typ parametrů nemůže mít typ ukazatele.</span><span class="sxs-lookup"><span data-stu-id="56f2d-981">The first parameter of an extension method can have no modifiers other than `this`, and the parameter type cannot be a pointer type.</span></span>

<span data-ttu-id="56f2d-982">Následuje příklad statické třídy, která deklaruje dvě rozšiřující metody:</span><span class="sxs-lookup"><span data-stu-id="56f2d-982">The following is an example of a static class that declares two extension methods:</span></span>
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

<span data-ttu-id="56f2d-983">Metody rozšíření je regulární statickou metodu.</span><span class="sxs-lookup"><span data-stu-id="56f2d-983">An extension method is a regular static method.</span></span> <span data-ttu-id="56f2d-984">Kromě toho, kde je její nadřazené statické třídy v oboru, metody rozšíření lze vyvolat pomocí syntaxe volání metody instance ([volání metod rozšíření](expressions.md#extension-method-invocations)), pomocí výrazu příjemce jako první argument.</span><span class="sxs-lookup"><span data-stu-id="56f2d-984">In addition, where its enclosing static class is in scope, an extension method can be invoked using instance method invocation syntax ([Extension method invocations](expressions.md#extension-method-invocations)), using the receiver expression as the first argument.</span></span>

<span data-ttu-id="56f2d-985">Následující program používá rozšiřující metody deklarované výše:</span><span class="sxs-lookup"><span data-stu-id="56f2d-985">The following program uses the extension methods declared above:</span></span>
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

<span data-ttu-id="56f2d-986">`Slice` Metoda je k dispozici na `string[]`a `ToInt32` metoda je k dispozici na `string`, protože deklarovány jako metody rozšíření.</span><span class="sxs-lookup"><span data-stu-id="56f2d-986">The `Slice` method is available on the `string[]`, and the `ToInt32` method is available on `string`, because they have been declared as extension methods.</span></span> <span data-ttu-id="56f2d-987">Význam tohoto programu je stejný jako běžný statická metoda volání následující za použití:</span><span class="sxs-lookup"><span data-stu-id="56f2d-987">The meaning of the program is the same as the following, using ordinary static method calls:</span></span>
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

### <a name="method-body"></a><span data-ttu-id="56f2d-988">Tělo metody</span><span class="sxs-lookup"><span data-stu-id="56f2d-988">Method body</span></span>

<span data-ttu-id="56f2d-989">*Method_body* metody deklarací se skládá z bloku textu, tělo výrazu nebo středníkem.</span><span class="sxs-lookup"><span data-stu-id="56f2d-989">The *method_body* of a method declaration consists of either a block body, an expression body or a semicolon.</span></span>

<span data-ttu-id="56f2d-990">***Výsledek typu*** metody je `void` návratový typ je `void`, nebo pokud se tato metoda je asynchronní a návratový typ `System.Threading.Tasks.Task`.</span><span class="sxs-lookup"><span data-stu-id="56f2d-990">The ***result type*** of a method is `void` if the return type is `void`, or if the method is async and the return type is `System.Threading.Tasks.Task`.</span></span> <span data-ttu-id="56f2d-991">V opačném případě je typ výsledku jiné asynchronní metody jejího návratového typu a výsledný typ asynchronní metody s návratovým typem `System.Threading.Tasks.Task<T>` je `T`.</span><span class="sxs-lookup"><span data-stu-id="56f2d-991">Otherwise, the result type of a non-async method is its return type, and the result type of an async method with return type `System.Threading.Tasks.Task<T>` is `T`.</span></span>

<span data-ttu-id="56f2d-992">Pokud má metodu `void` výsledek typu a blok textu, `return` příkazy ([příkaz return](statements.md#the-return-statement)) v bloku nejsou povolené. Chcete-li zadat výraz.</span><span class="sxs-lookup"><span data-stu-id="56f2d-992">When a method has a `void` result type and a block body, `return` statements ([The return statement](statements.md#the-return-statement)) in the block are not permitted to specify an expression.</span></span> <span data-ttu-id="56f2d-993">Pokud spuštění bloku void metoda obvykle hotové (to znamená, určují toky vypnout na konec těla metody), tato metoda jednoduše vrátí aktuální volajícímu.</span><span class="sxs-lookup"><span data-stu-id="56f2d-993">If execution of the block of a void method completes normally (that is, control flows off the end of the method body), that method simply returns to its current caller.</span></span>
    
<span data-ttu-id="56f2d-994">Pokud má metodu `void` výsledek a tělo výrazu, výrazu `E` musí být *statement_expression*, a přesně odpovídá do bloku textu ve formátu textu `{ E; }`.</span><span class="sxs-lookup"><span data-stu-id="56f2d-994">When a method has a `void` result and an expression body, the expression `E` must be a *statement_expression*, and the body is exactly equivalent to a block body of the form `{ E; }`.</span></span>
    
<span data-ttu-id="56f2d-995">Když metoda má typ jiný než void výsledku a blok textu, každý `return` příkazu v bloku musejí zadat výraz, který je implicitně převést na typ výsledku.</span><span class="sxs-lookup"><span data-stu-id="56f2d-995">When a method has a non-void result type and a block body, each `return` statement in the block must specify an expression that is implicitly convertible to the result type.</span></span> <span data-ttu-id="56f2d-996">Koncový bod bloku těla metody vracející hodnotu nesmí být dosažitelný.</span><span class="sxs-lookup"><span data-stu-id="56f2d-996">The endpoint of a block body of a value-returning method must not be reachable.</span></span> <span data-ttu-id="56f2d-997">Jinými slovy v metodě vrací hodnotu s tělem bloku, ovládací prvek není oprávněn tok koncem tělo metody.</span><span class="sxs-lookup"><span data-stu-id="56f2d-997">In other words, in a value-returning method with a block body, control is not permitted to flow off the end of the method body.</span></span>
    
<span data-ttu-id="56f2d-998">Když metoda má typ jiný než void výsledku a tělo výrazu, výraz musí být implicitně převoditelná na typ výsledku a text se do bloku textu formuláře `{ return E; }`.</span><span class="sxs-lookup"><span data-stu-id="56f2d-998">When a method has a non-void result type and an expression body, the expression must be implicitly convertible to the result type, and the body is exactly equivalent to a block body of the form `{ return E; }`.</span></span>
    
<span data-ttu-id="56f2d-999">V příkladu</span><span class="sxs-lookup"><span data-stu-id="56f2d-999">In the example</span></span>
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
<span data-ttu-id="56f2d-1000">vrací hodnotu `F` metodu vede k chybě kompilace vzhledem k tomu, že lze vypnout na konec těla metody tok řízení.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1000">the value-returning `F` method results in a compile-time error because control can flow off the end of the method body.</span></span> <span data-ttu-id="56f2d-1001">`G` a `H` metody jsou správné, protože se všemi možnými cestami spuštění ukončení v příkazu return, který určuje návratovou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1001">The `G` and `H` methods are correct because all possible execution paths end in a return statement that specifies a return value.</span></span> <span data-ttu-id="56f2d-1002">`I` – Metoda je správný, protože jeho text je ekvivalentní k blok příkazů, který se právě jedním návratovým příkazem. v něm.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1002">The `I` method is correct, because its body is equivalent to a statement block with just a single return statement in it.</span></span>

### <a name="method-overloading"></a><span data-ttu-id="56f2d-1003">Přetěžování metody</span><span class="sxs-lookup"><span data-stu-id="56f2d-1003">Method overloading</span></span>

<span data-ttu-id="56f2d-1004">Pravidla pro řešení přetížení metody jsou popsané v [odvození typu](expressions.md#type-inference).</span><span class="sxs-lookup"><span data-stu-id="56f2d-1004">The method overload resolution rules are described in [Type inference](expressions.md#type-inference).</span></span>

## <a name="properties"></a><span data-ttu-id="56f2d-1005">Vlastnosti</span><span class="sxs-lookup"><span data-stu-id="56f2d-1005">Properties</span></span>

<span data-ttu-id="56f2d-1006">A ***vlastnost*** je člen, který poskytuje přístup k typické pro objekt nebo třídu.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1006">A ***property*** is a member that provides access to a characteristic of an object or a class.</span></span> <span data-ttu-id="56f2d-1007">Příkladem vlastnosti zahrnují délku řetězce, velikost písma, titulek okna, název zákazníka a tak dále.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1007">Examples of properties include the length of a string, the size of a font, the caption of a window, the name of a customer, and so on.</span></span> <span data-ttu-id="56f2d-1008">Vlastnosti jsou přirozené rozšíření polí – obě jsou pojmenované členy s přidružené typy a syntaxe pro přístup k vlastnosti a pole je stejná.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1008">Properties are a natural extension of fields—both are named members with associated types, and the syntax for accessing fields and properties is the same.</span></span> <span data-ttu-id="56f2d-1009">Na rozdíl od pole, ale vlastnosti nejsou označení umístění úložiště.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1009">However, unlike fields, properties do not denote storage locations.</span></span> <span data-ttu-id="56f2d-1010">Místo toho mají vlastnosti ***přistupující objekty*** , které určují příkazy ke se spustí při jejich hodnoty jsou číst nebo zapisovat.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1010">Instead, properties have ***accessors*** that specify the statements to be executed when their values are read or written.</span></span> <span data-ttu-id="56f2d-1011">Vlastnosti tedy poskytují mechanismus pro přidružení k čtení a zápis atributy objektu; akce navíc umožňují těchto atributů, které mají být vypočteny.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1011">Properties thus provide a mechanism for associating actions with the reading and writing of an object's attributes; furthermore, they permit such attributes to be computed.</span></span>

<span data-ttu-id="56f2d-1012">Vlastnosti jsou deklarovány pomocí *property_declaration*s:</span><span class="sxs-lookup"><span data-stu-id="56f2d-1012">Properties are declared using *property_declaration*s:</span></span>

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

<span data-ttu-id="56f2d-1013">A *property_declaration* může zahrnovat sadu *atributy* ([atributy](attributes.md)) a platnou kombinaci čtyři přístupu modifikátory přístupu ([modifikátorů přístupu ](classes.md#access-modifiers)), `new` ([new – modifikátor](classes.md#the-new-modifier)), `static` ([statická a instanci metody](classes.md#static-and-instance-methods)), `virtual` ([virtuální metody](classes.md#virtual-methods)), `override` ([Přepište metody](classes.md#override-methods)), `sealed` ([zapečetěné metody](classes.md#sealed-methods)), `abstract` ([abstraktní metody](classes.md#abstract-methods)), a `extern` ([Externí metody](classes.md#external-methods)) modifikátory.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1013">A *property_declaration* may include a set of *attributes* ([Attributes](attributes.md)) and a valid combination of the four access modifiers ([Access modifiers](classes.md#access-modifiers)), the `new` ([The new modifier](classes.md#the-new-modifier)),  `static` ([Static and instance methods](classes.md#static-and-instance-methods)), `virtual` ([Virtual methods](classes.md#virtual-methods)), `override` ([Override methods](classes.md#override-methods)), `sealed` ([Sealed methods](classes.md#sealed-methods)), `abstract` ([Abstract methods](classes.md#abstract-methods)), and `extern` ([External methods](classes.md#external-methods)) modifiers.</span></span>

<span data-ttu-id="56f2d-1014">Deklarace vlastnosti se vztahují stejná pravidla jako deklarace metody ([metody](classes.md#methods)) s ohledem na platné kombinace modifikátorů.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1014">Property declarations are subject to the same rules as method declarations ([Methods](classes.md#methods)) with regard to valid combinations of modifiers.</span></span>

<span data-ttu-id="56f2d-1015">*Typ* vlastnosti deklarace Určuje typ vlastnosti zavedeným deklarací a *member_name* Určuje název vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1015">The *type* of a property declaration specifies the type of the property introduced by the declaration, and the *member_name* specifies the name of the property.</span></span> <span data-ttu-id="56f2d-1016">Pokud není vlastnost člena implementace explicitního rozhraní, *member_name* je jednoduše k *identifikátor*.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1016">Unless the property is an explicit interface member implementation, the *member_name* is simply an *identifier*.</span></span> <span data-ttu-id="56f2d-1017">Člen implementace explicitního rozhraní ([implementace explicitního rozhraní členských](interfaces.md#explicit-interface-member-implementations)), *member_name* se skládá ze *interface_type* za nímž následuje " `.`"a *identifikátor*.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1017">For an explicit interface member implementation ([Explicit interface member implementations](interfaces.md#explicit-interface-member-implementations)), the *member_name* consists of an *interface_type* followed by a "`.`" and an *identifier*.</span></span>

<span data-ttu-id="56f2d-1018">*Typ* vlastnosti musí být přístupné jako vlastnost vlastní ([usnadnění omezení](basic-concepts.md#accessibility-constraints)).</span><span class="sxs-lookup"><span data-stu-id="56f2d-1018">The *type* of a property must be at least as accessible as the property itself ([Accessibility constraints](basic-concepts.md#accessibility-constraints)).</span></span>

<span data-ttu-id="56f2d-1019">A *property_body* může buď skládat ze ***přistupující objekt textu*** nebo ***tělo výrazu***.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1019">A *property_body* may either consist of an ***accessor body*** or an ***expression body***.</span></span> <span data-ttu-id="56f2d-1020">V textu přístupový objekt *accessor_declarations*, musí být uzavřen v "`{`"a"`}`" tokeny, deklarujte přístupových objektů ([přistupující objekty](classes.md#accessors)) vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1020">In an accessor body,  *accessor_declarations*, which must be enclosed in "`{`" and "`}`" tokens, declare the accessors ([Accessors](classes.md#accessors)) of the property.</span></span> <span data-ttu-id="56f2d-1021">Přístupové objekty zadejte spustitelné příkazy, které jsou přidružené k čtení a zápisu vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1021">The accessors specify the executable statements associated with reading and writing the property.</span></span>

<span data-ttu-id="56f2d-1022">Tělo výrazu skládající se z `=>` následovaný *výraz* `E` a středník se na základní text příkazu `{ get { return E; } }`a proto jde použít jenom k určení jen pro funkci getter vlastnosti, ve kterém je výsledek getter dán jeden výraz.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1022">An expression body consisting of `=>` followed by an *expression* `E` and a semicolon is exactly equivalent to the statement body `{ get { return E; } }`, and can therefore only be used to specify getter-only properties where the result of the getter is given by a single expression.</span></span>

<span data-ttu-id="56f2d-1023">A *property_initializer* nejde zadávat jenom automaticky implementované vlastnosti ([automaticky implementované vlastnosti](classes.md#automatically-implemented-properties)) a způsobí, že inicializace podkladové pole těchto vlastnosti s hodnotou dána *výraz*.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1023">A *property_initializer* may only be given for an automatically implemented property ([Automatically implemented properties](classes.md#automatically-implemented-properties)), and causes the initialization of the underlying field of such properties with the value given by the *expression*.</span></span>

<span data-ttu-id="56f2d-1024">I když syntaxi pro přístup k vlastnosti je stejné jako pro pole, vlastnost není klasifikováno jako proměnnou.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1024">Even though the syntax for accessing a property is the same as that for a field, a property is not classified as a variable.</span></span> <span data-ttu-id="56f2d-1025">Proto není možné předat vlastnost jako `ref` nebo `out` argument.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1025">Thus, it is not possible to pass a property as a `ref` or `out` argument.</span></span>

<span data-ttu-id="56f2d-1026">Pokud obsahuje deklaraci vlastnosti `extern` modifikátor, je označen jako vlastnost ***vlastnost external***.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1026">When a property declaration includes an `extern` modifier, the property is said to be an ***external property***.</span></span> <span data-ttu-id="56f2d-1027">Protože deklaraci vlastnost external poskytuje skutečné implementaci, každá z jeho *accessor_declarations* skládá se ze středníku.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1027">Because an external property declaration provides no actual implementation, each of its *accessor_declarations* consists of a semicolon.</span></span>

### <a name="static-and-instance-properties"></a><span data-ttu-id="56f2d-1028">Statické a instance vlastnosti</span><span class="sxs-lookup"><span data-stu-id="56f2d-1028">Static and instance properties</span></span>

<span data-ttu-id="56f2d-1029">Pokud obsahuje deklaraci vlastnosti `static` modifikátor, vlastnost se říká, že ***statickou vlastnost***.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1029">When a property declaration includes a `static` modifier, the property is said to be a ***static property***.</span></span> <span data-ttu-id="56f2d-1030">Pokud ne `static` Modifikátor je k dispozici, je označen jako vlastnost ***vlastnost instance***.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1030">When no `static` modifier is present, the property is said to be an ***instance property***.</span></span>

<span data-ttu-id="56f2d-1031">Statická vlastnost není přidružena k určité instanci a je chyba kompilace k odkazování na `this` v přistupující objekty statickou vlastnost.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1031">A static property is not associated with a specific instance, and it is a compile-time error to refer to `this` in the accessors of a static property.</span></span>

<span data-ttu-id="56f2d-1032">Vlastnost instance je přidružený k instanci dané třídy a tato instance je přístupná jako `this` ([tento přístup](expressions.md#this-access)) v přistupující objekty vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1032">An instance property is associated with a given instance of a class, and that instance can be accessed as `this` ([This access](expressions.md#this-access)) in the accessors of that property.</span></span>

<span data-ttu-id="56f2d-1033">Pokud vlastnost se odkazuje v *member_access* ([přístup ke členu](expressions.md#member-access)) ve formátu `E.M`, pokud `M` je statická vlastnost `E` musí označují typ obsahující `M`a pokud `M` je vlastnost instance, E musí označit instanci typu, který obsahuje `M`.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1033">When a property is referenced in a *member_access* ([Member access](expressions.md#member-access)) of the form `E.M`, if `M` is a static property, `E` must denote a type containing `M`, and if `M` is an instance property, E must denote an instance of a type containing `M`.</span></span>

<span data-ttu-id="56f2d-1034">Rozdíly mezi statické a členy instance jsou popsány dále v [statické a instance členy](classes.md#static-and-instance-members).</span><span class="sxs-lookup"><span data-stu-id="56f2d-1034">The differences between static and instance members are discussed further in [Static and instance members](classes.md#static-and-instance-members).</span></span>

### <a name="accessors"></a><span data-ttu-id="56f2d-1035">Přístupové objekty</span><span class="sxs-lookup"><span data-stu-id="56f2d-1035">Accessors</span></span>

<span data-ttu-id="56f2d-1036">*Accessor_declarations* vlastnosti zadejte spustitelné příkazy spojená se čtením a zápisem tuto vlastnost.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1036">The *accessor_declarations* of a property specify the executable statements associated with reading and writing that property.</span></span>

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

<span data-ttu-id="56f2d-1037">Deklarace přistupujícího objektu obsahovat *get_accessor_declaration*, *set_accessor_declaration*, nebo obojí.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1037">The accessor declarations consist of a *get_accessor_declaration*, a *set_accessor_declaration*, or both.</span></span> <span data-ttu-id="56f2d-1038">Každá deklarace přistupujícího objektu se skládá z tokenu `get` nebo `set` a volitelně *accessor_modifier* a *accessor_body*.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1038">Each accessor declaration consists of the token `get` or `set` followed by an optional *accessor_modifier* and an *accessor_body*.</span></span>

<span data-ttu-id="56f2d-1039">Použití *accessor_modifier*s se vztahují následující omezení:</span><span class="sxs-lookup"><span data-stu-id="56f2d-1039">The use of *accessor_modifier*s is governed by the following restrictions:</span></span>

*  <span data-ttu-id="56f2d-1040">*Accessor_modifier* nelze použít v rozhraní nebo člen implementace explicitního rozhraní.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1040">An *accessor_modifier* may not be used in an interface or in an explicit interface member implementation.</span></span>
*  <span data-ttu-id="56f2d-1041">Pro vlastnost nebo indexovací člen, který nemá žádné `override` modifikátor, *accessor_modifier* smí obsahovat pouze v případě, že vlastnost nebo indexovací člen jsou obě `get` a `set` přístupový objekt a pak je povolené jenom na jednu z těchto přístupové objekty.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1041">For a property or indexer that has no `override` modifier, an *accessor_modifier* is permitted only if the property or indexer has both a `get` and `set` accessor, and then is permitted only on one of those accessors.</span></span>
*  <span data-ttu-id="56f2d-1042">Pro vlastnost nebo indexovací člen, který zahrnuje `override` modifikátor, přístupový objekt musí odpovídat *accessor_modifier*(pokud existuje) přistupujícího objektu přepsání.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1042">For a property or indexer that includes an `override` modifier, an accessor must match the *accessor_modifier*, if any, of the accessor being overridden.</span></span>
*  <span data-ttu-id="56f2d-1043">*Accessor_modifier* musí deklarovat dostupnost, která je výhradně více omezující než je deklarovaná přístupnost člena vlastnost nebo indexovací člen, samotného.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1043">The *accessor_modifier* must declare an accessibility that is strictly more restrictive than the declared accessibility of the property or indexer itself.</span></span> <span data-ttu-id="56f2d-1044">Abychom byli přesní:</span><span class="sxs-lookup"><span data-stu-id="56f2d-1044">To be precise:</span></span>
   * <span data-ttu-id="56f2d-1045">Pokud vlastnost nebo indexer je deklarovaná přístupnost člena `public`, *accessor_modifier* může být buď `protected internal`, `internal`, `protected`, nebo `private`.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1045">If the property or indexer has a declared accessibility of `public`, the *accessor_modifier* may be either `protected internal`, `internal`, `protected`, or `private`.</span></span>
   * <span data-ttu-id="56f2d-1046">Pokud vlastnost nebo indexer je deklarovaná přístupnost člena `protected internal`, *accessor_modifier* může být buď `internal`, `protected`, nebo `private`.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1046">If the property or indexer has a declared accessibility of `protected internal`, the *accessor_modifier* may be either `internal`, `protected`, or `private`.</span></span>
   * <span data-ttu-id="56f2d-1047">Pokud vlastnost nebo indexer je deklarovaná přístupnost člena `internal` nebo `protected`, *accessor_modifier* musí být `private`.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1047">If the property or indexer has a declared accessibility of `internal` or `protected`, the *accessor_modifier* must be `private`.</span></span>
   * <span data-ttu-id="56f2d-1048">Pokud vlastnost nebo indexer je deklarovaná přístupnost člena `private`, ne *accessor_modifier* mohou být použity.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1048">If the property or indexer has a declared accessibility of `private`, no *accessor_modifier* may be used.</span></span>

<span data-ttu-id="56f2d-1049">Pro `abstract` a `extern` vlastnosti, *accessor_body* pro každý přistupující objekt zadán, je jednoduše středníkem.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1049">For `abstract` and `extern` properties, the *accessor_body* for each accessor specified is simply a semicolon.</span></span> <span data-ttu-id="56f2d-1050">Neabstraktní, není externí vlastnost může mít každý *accessor_body* se středníkem, v takovém případě je ***automaticky implementovaná vlastnost*** ([automaticky implementované vlastnosti ](classes.md#automatically-implemented-properties)).</span><span class="sxs-lookup"><span data-stu-id="56f2d-1050">A non-abstract, non-extern property may have each *accessor_body* be a semicolon, in which case it is an ***automatically implemented property*** ([Automatically implemented properties](classes.md#automatically-implemented-properties)).</span></span> <span data-ttu-id="56f2d-1051">Automaticky implementované vlastnosti musí mít alespoň přistupující objekt get.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1051">An automatically implemented property must have at least a get accessor.</span></span> <span data-ttu-id="56f2d-1052">Pro přistupující objekty vlastnosti žádné jiné neabstraktní, není externí *accessor_body* je *bloku* který určuje příkazy ke se spustí při vyvolání odpovídající přistupujícího objektu.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1052">For the accessors of any other non-abstract, non-extern property, the *accessor_body* is a *block* which specifies the statements to be executed when the corresponding accessor is invoked.</span></span>

<span data-ttu-id="56f2d-1053">A `get` přistupující objekt odpovídající konstruktor bez parametrů metody s návratovou hodnotou typu vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1053">A `get` accessor corresponds to a parameterless method with a return value of the property type.</span></span> <span data-ttu-id="56f2d-1054">Při odkazování na vlastnost ve výrazu, s výjimkou případů cílem přiřazení, `get` přistupující objekt vlastnosti je vyvolána k výpočtu hodnoty vlastnosti ([hodnot výrazů](expressions.md#values-of-expressions)).</span><span class="sxs-lookup"><span data-stu-id="56f2d-1054">Except as the target of an assignment, when a property is referenced in an expression, the `get` accessor of the property is invoked to compute the value of the property ([Values of expressions](expressions.md#values-of-expressions)).</span></span> <span data-ttu-id="56f2d-1055">Text `get` přístupového objektu musí splňovat pravidla pro vrácení hodnoty metod popsaných v [tělo metody](classes.md#method-body).</span><span class="sxs-lookup"><span data-stu-id="56f2d-1055">The body of a `get` accessor must conform to the rules for value-returning methods described in [Method body](classes.md#method-body).</span></span> <span data-ttu-id="56f2d-1056">Konkrétně se všechny `return` příkazy v těle `get` přistupující objekt musejí zadat výraz, který je implicitně převést na typ vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1056">In particular, all `return` statements in the body of a `get` accessor must specify an expression that is implicitly convertible to the property type.</span></span> <span data-ttu-id="56f2d-1057">Kromě toho koncový bod `get` přistupující objekt nesmí být dosažitelný.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1057">Furthermore, the endpoint of a `get` accessor must not be reachable.</span></span>

<span data-ttu-id="56f2d-1058">A `set` přistupující objekt odpovídající metodu s jednou hodnotou parametrem typu vlastnosti a `void` návratového typu.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1058">A `set` accessor corresponds to a method with a single value parameter of the property type and a `void` return type.</span></span> <span data-ttu-id="56f2d-1059">Implicitní parametr `set` přístupový objekt je vždy pojmenováno `value`.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1059">The implicit parameter of a `set` accessor is always named `value`.</span></span> <span data-ttu-id="56f2d-1060">Když se odkazuje vlastnost jako cíl přiřazení ([operátory přiřazení](expressions.md#assignment-operators)), nebo jako operand `++` nebo `--` ([Příponové operátory Inkrementace a dekrementace operátory](expressions.md#postfix-increment-and-decrement-operators), [ Předpona Inkrementace a dekrementace operátory](expressions.md#prefix-increment-and-decrement-operators)), `set` přístupového objektu je volána s argumentem (jehož hodnota je pravá strana přiřazení nebo operand `++` nebo `--` operátor), který poskytuje novou hodnotu ([jednoduché přiřazení](expressions.md#simple-assignment)).</span><span class="sxs-lookup"><span data-stu-id="56f2d-1060">When a property is referenced as the target of an assignment ([Assignment operators](expressions.md#assignment-operators)), or as the operand of `++` or `--` ([Postfix increment and decrement operators](expressions.md#postfix-increment-and-decrement-operators), [Prefix increment and decrement operators](expressions.md#prefix-increment-and-decrement-operators)), the `set` accessor is invoked with an argument (whose value is that of the right-hand side of the assignment or the operand of the `++` or `--` operator) that provides the new value ([Simple assignment](expressions.md#simple-assignment)).</span></span> <span data-ttu-id="56f2d-1061">Text `set` přístupového objektu musí splňovat pravidla pro `void` metod popsaných v [tělo metody](classes.md#method-body).</span><span class="sxs-lookup"><span data-stu-id="56f2d-1061">The body of a `set` accessor must conform to the rules for `void` methods described in [Method body](classes.md#method-body).</span></span> <span data-ttu-id="56f2d-1062">Zejména `return` příkazů v `set` přistupující objekt textu nejsou povolené. Chcete-li zadat výraz.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1062">In particular, `return` statements in the `set` accessor body are not permitted to specify an expression.</span></span> <span data-ttu-id="56f2d-1063">Protože `set` přístupového objektu má implicitně parametr s názvem `value`, je chyba kompilace pro deklarace lokální proměnné nebo konstantní v `set` přístupový objekt s tímto názvem.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1063">Since a `set` accessor implicitly has a parameter named `value`, it is a compile-time error for a local variable or constant declaration in a `set` accessor to have that name.</span></span>

<span data-ttu-id="56f2d-1064">Záleží na přítomnosti nebo absenci `get` a `set` přistupující objekty vlastnosti klasifikovaný následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="56f2d-1064">Based on the presence or absence of the `get` and `set` accessors, a property is classified as follows:</span></span>

*  <span data-ttu-id="56f2d-1065">Vlastnost, která obsahuje oba `get` přístupového objektu a `set` přístupového objektu se říká, že ***čtení a zápis*** vlastnost.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1065">A property that includes both a `get` accessor and a `set` accessor is said to be a ***read-write*** property.</span></span>
*  <span data-ttu-id="56f2d-1066">Vlastnost, která má pouze `get` přístupového objektu se říká, že ***jen pro čtení*** vlastnost.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1066">A property that has only a `get` accessor is said to be a ***read-only*** property.</span></span> <span data-ttu-id="56f2d-1067">Je chyba kompilace pro vlastnosti jen pro čtení a být cílem přiřazení.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1067">It is a compile-time error for a read-only property to be the target of an assignment.</span></span>
*  <span data-ttu-id="56f2d-1068">Vlastnost, která má pouze `set` přístupového objektu se říká, že ***jen pro zápis*** vlastnost.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1068">A property that has only a `set` accessor is said to be a ***write-only*** property.</span></span> <span data-ttu-id="56f2d-1069">S výjimkou jako cílem přiřazení, je chyba kompilace k odkazování na vlastnost jen pro zápis ve výrazu.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1069">Except as the target of an assignment, it is a compile-time error to reference a write-only property in an expression.</span></span>

<span data-ttu-id="56f2d-1070">V příkladu</span><span class="sxs-lookup"><span data-stu-id="56f2d-1070">In the example</span></span>
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
<span data-ttu-id="56f2d-1071">`Button` ovládací prvek deklaruje veřejnou `Caption` vlastnost.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1071">the `Button` control declares a public `Caption` property.</span></span> <span data-ttu-id="56f2d-1072">`get` Přistupující objekt `Caption` vlastnost vrací řetězec uložený v privátní `caption` pole.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1072">The `get` accessor of the `Caption` property returns the string stored in the private `caption` field.</span></span> <span data-ttu-id="56f2d-1073">`set` Přistupující objekt kontroluje, jestli je nová hodnota se liší od aktuální hodnoty a pokud ano, uloží novou hodnotu a překreslí ovládacího prvku.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1073">The `set` accessor checks if the new value is different from the current value, and if so, it stores the new value and repaints the control.</span></span> <span data-ttu-id="56f2d-1074">Vlastnosti často mají tvar uvedené nahoře: `get` přistupující objekt jednoduše vrací hodnotu uloženou v soukromé pole a `set` přistupující objekt upravuje privátní pole a potom provede žádné další akce pro úplnou aktualizaci stavu objektu.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1074">Properties often follow the pattern shown above: The `get` accessor simply returns a value stored in a private field, and the `set` accessor modifies that private field and then performs any additional actions required to fully update the state of the object.</span></span>

<span data-ttu-id="56f2d-1075">Zadaný `Button` třídy výše, následuje příklad použití `Caption` vlastnost:</span><span class="sxs-lookup"><span data-stu-id="56f2d-1075">Given the `Button` class above, the following is an example of use of the `Caption` property:</span></span>
```csharp
Button okButton = new Button();
okButton.Caption = "OK";            // Invokes set accessor
string s = okButton.Caption;        // Invokes get accessor
```

<span data-ttu-id="56f2d-1076">Tady `set` přístupového objektu je vyvolán přiřazení hodnoty k vlastnosti a `get` přístupového objektu je vyvolán pomocí odkazu na vlastnost ve výrazu.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1076">Here, the `set` accessor is invoked by assigning a value to the property, and the `get` accessor is invoked by referencing the property in an expression.</span></span>

<span data-ttu-id="56f2d-1077">`get` a `set` přistupující objekty vlastnosti nejsou odlišné členy a není možné deklarovat přistupující objekty vlastnosti samostatně.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1077">The `get` and `set` accessors of a property are not distinct members, and it is not possible to declare the accessors of a property separately.</span></span> <span data-ttu-id="56f2d-1078">V důsledku toho není možné pro dva přistupující objekty vlastnosti pro čtení i zápis mít různou přístupností.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1078">As such, it is not possible for the two accessors of a read-write property to have different accessibility.</span></span> <span data-ttu-id="56f2d-1079">V příkladu</span><span class="sxs-lookup"><span data-stu-id="56f2d-1079">The example</span></span>
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
<span data-ttu-id="56f2d-1080">nedeklaruje jedné vlastnosti pro čtení i zápis.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1080">does not declare a single read-write property.</span></span> <span data-ttu-id="56f2d-1081">Místo toho deklaruje dvě vlastnosti se stejným názvem, jeden jen pro čtení a jednu pouze pro zápis.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1081">Rather, it declares two properties with the same name, one read-only and one write-only.</span></span> <span data-ttu-id="56f2d-1082">Protože dva členy deklarované ve stejné třídě, nemůže mít stejný název, příklad způsobí chybu kompilace dojde k.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1082">Since two members declared in the same class cannot have the same name, the example causes a compile-time error to occur.</span></span>

<span data-ttu-id="56f2d-1083">Pokud odvozená třída deklaruje vlastnost se stejným názvem jako o zděděnou vlastnost, vlastnost odvozené skryje zděděné vlastnosti s ohledem na čtení i zápis.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1083">When a derived class declares a property by the same name as an inherited property, the derived property hides the inherited property with respect to both reading and writing.</span></span> <span data-ttu-id="56f2d-1084">V příkladu</span><span class="sxs-lookup"><span data-stu-id="56f2d-1084">In the example</span></span>
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
<span data-ttu-id="56f2d-1085">`P` vlastnost `B` skryje `P` vlastnost `A` s ohledem na čtení i zápis.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1085">the `P` property in `B` hides the `P` property in `A` with respect to both reading and writing.</span></span> <span data-ttu-id="56f2d-1086">Díky tomu se v příkazech</span><span class="sxs-lookup"><span data-stu-id="56f2d-1086">Thus, in the statements</span></span>
```csharp
B b = new B();
b.P = 1;          // Error, B.P is read-only
((A)b).P = 1;     // Ok, reference to A.P
```
<span data-ttu-id="56f2d-1087">přiřazení `b.P` způsobí chybu kompilace má být hlášen, protože jen pro čtení `P` vlastnost `B` skryje jen pro zápis `P` vlastnost v `A`.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1087">the assignment to `b.P` causes a compile-time error to be reported, since the read-only `P` property in `B` hides the write-only `P` property in `A`.</span></span> <span data-ttu-id="56f2d-1088">Upozorňujeme, že přetypování lze použít pro přístup k skrytého `P` vlastnost.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1088">Note, however, that a cast can be used to access the hidden `P` property.</span></span>

<span data-ttu-id="56f2d-1089">Na rozdíl od veřejné pole vlastnosti poskytují oddělení mezi vnitřní stav objektu a jeho veřejné rozhraní.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1089">Unlike public fields, properties provide a separation between an object's internal state and its public interface.</span></span> <span data-ttu-id="56f2d-1090">Podívejte se na příklad:</span><span class="sxs-lookup"><span data-stu-id="56f2d-1090">Consider the example:</span></span>
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

<span data-ttu-id="56f2d-1091">Tady `Label` používá třídy, dvě `int` pole, `x` a `y`, k jeho umístění úložiště.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1091">Here, the `Label` class uses two `int` fields, `x` and `y`, to store its location.</span></span> <span data-ttu-id="56f2d-1092">Umístění je veřejně vystavené jako `X` a `Y` vlastnost a jako `Location` vlastnost typu `Point`.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1092">The location is publicly exposed both as an `X` and a `Y` property and as a `Location` property of type `Point`.</span></span> <span data-ttu-id="56f2d-1093">Pokud v budoucí verzi systému `Label`, bude pohodlnější umístění jako úložiště `Point` interně, může být změnu provést, aniž by to ovlivnilo veřejné rozhraní třídy:</span><span class="sxs-lookup"><span data-stu-id="56f2d-1093">If, in a future version of `Label`, it becomes more convenient to store the location as a `Point` internally, the change can be made without affecting the public interface of the class:</span></span>
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

<span data-ttu-id="56f2d-1094">Měl `x` a `y` místo toho byla `public readonly` pole, bylo by možné provést změnu na `Label` třídy.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1094">Had `x` and `y` instead been `public readonly` fields, it would have been impossible to make such a change to the `Label` class.</span></span>

<span data-ttu-id="56f2d-1095">Vystavení stav prostřednictvím vlastnosti není nutně všechny méně efektivní než zveřejnění pole přímo.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1095">Exposing state through properties is not necessarily any less efficient than exposing fields directly.</span></span> <span data-ttu-id="56f2d-1096">Zejména při vlastnost nevirtuální a obsahuje pouze malé množství kódu, prostředí pro spuštění mohou nahradit volání přistupující objekty skutečný kód přístupové objekty.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1096">In particular, when a property is non-virtual and contains only a small amount of code, the execution environment may replace calls to accessors with the actual code of the accessors.</span></span> <span data-ttu-id="56f2d-1097">Tento proces se označuje jako ***vkládání***, a umožňuje tak efektivní jako přístup k poli přístup k vlastnostem, ale zachová se zvýší flexibilita při vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1097">This process is known as ***inlining***, and it makes property access as efficient as field access, yet preserves the increased flexibility of properties.</span></span>

<span data-ttu-id="56f2d-1098">Od volání `get` přístupový objekt je koncepčním ekvivalentem hodnoty pole pro čtení, bude považován za špatný styl programování pro `get` přistupující objekty pozorovatelný vedlejší účinky.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1098">Since invoking a `get` accessor is conceptually equivalent to reading the value of a field, it is considered bad programming style for `get` accessors to have observable side-effects.</span></span> <span data-ttu-id="56f2d-1099">V příkladu</span><span class="sxs-lookup"><span data-stu-id="56f2d-1099">In the example</span></span>
```csharp
class Counter
{
    private int next;

    public int Next {
        get { return next++; }
    }
}
```
<span data-ttu-id="56f2d-1100">Hodnota `Next` vlastnost závisí na počet, kolikrát vlastnost má dříve navštívili.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1100">the value of the `Next` property depends on the number of times the property has previously been accessed.</span></span> <span data-ttu-id="56f2d-1101">Proto přístup k vlastnosti vytváří pozorovatelný vedlejší efekt, a vlastnost by měla být implementována jako metoda místo.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1101">Thus, accessing the property produces an observable side-effect, and the property should be implemented as a method instead.</span></span>

<span data-ttu-id="56f2d-1102">"Žádné vedlejší účinky" konvence pro `get` přistupující objekty neznamená, že `get` přístupových objektů by měl vždy být zapsán jednoduše vrátit hodnoty uložené v polích.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1102">The "no side-effects" convention for `get` accessors doesn't mean that `get` accessors should always be written to simply return values stored in fields.</span></span> <span data-ttu-id="56f2d-1103">Ve skutečnosti `get` přistupující objekty často výpočtu hodnoty vlastnosti přístup k více polí nebo volání metod.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1103">Indeed, `get` accessors often compute the value of a property by accessing multiple fields or invoking methods.</span></span> <span data-ttu-id="56f2d-1104">Nicméně správně navržená `get` přístupového objektu provede žádná akce, které způsobují pozorovatelných změny ve stavu objektu.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1104">However, a properly designed `get` accessor performs no actions that cause observable changes in the state of the object.</span></span>

<span data-ttu-id="56f2d-1105">Vlastnosti lze použít k odložení inicializace prostředku až do okamžiku, kdy se nejprve odkazuje.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1105">Properties can be used to delay initialization of a resource until the moment it is first referenced.</span></span> <span data-ttu-id="56f2d-1106">Příklad:</span><span class="sxs-lookup"><span data-stu-id="56f2d-1106">For example:</span></span>
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

<span data-ttu-id="56f2d-1107">`Console` Třída obsahuje tři vlastnosti `In`, `Out`, a `Error`, které představují standardní vstup, výstup a chyby zařízení.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1107">The `Console` class contains three properties, `In`, `Out`, and `Error`, that represent the standard input, output, and error devices, respectively.</span></span> <span data-ttu-id="56f2d-1108">Zveřejněním tyto členy jako vlastnosti `Console` třídy můžete zpoždění inicializační, dokud skutečně spotřebujete.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1108">By exposing these members as properties, the `Console` class can delay their initialization until they are actually used.</span></span> <span data-ttu-id="56f2d-1109">Například při prvním odkazující `Out` vlastnosti, jako v</span><span class="sxs-lookup"><span data-stu-id="56f2d-1109">For example, upon first referencing the `Out` property, as in</span></span>
```csharp
Console.Out.WriteLine("hello, world");
```
<span data-ttu-id="56f2d-1110">základní `TextWriter` pro vytvoření výstupního zařízení.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1110">the underlying `TextWriter` for the output device is created.</span></span> <span data-ttu-id="56f2d-1111">Ale pokud žádný odkaz na aplikaci `In` a `Error` vlastnosti a pak žádné objekty jsou vytvořeny pro tato zařízení.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1111">But if the application makes no reference to the `In` and `Error` properties, then no objects are created for those devices.</span></span>

### <a name="automatically-implemented-properties"></a><span data-ttu-id="56f2d-1112">Automaticky implementované vlastnosti</span><span class="sxs-lookup"><span data-stu-id="56f2d-1112">Automatically implemented properties</span></span>

<span data-ttu-id="56f2d-1113">Automaticky implementované vlastnosti (nebo ***automatickou vlastnost*** zkráceně), je vlastnost bez extern neabstraktní s orgány pouze středník.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1113">An automatically implemented property (or ***auto-property*** for short), is a non-abstract non-extern property with semicolon-only accessor bodies.</span></span> <span data-ttu-id="56f2d-1114">Automatické vlastnosti musí mít přístupový objekt get a můžou mít přistupující objekt set.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1114">Auto-properties must have a get accessor and can optionally have a set accessor.</span></span>

<span data-ttu-id="56f2d-1115">Je-li vlastnost zadána jako automaticky implementovanou vlastnost, pole Skrytá zálohování je automaticky dostupný pro vlastnost a přístupové objekty jsou implementovány číst z a zapisovat do tohoto pole zálohování.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1115">When a property is specified as an automatically implemented property, a hidden backing field is automatically available for the property, and the accessors are implemented to read from and write to that backing field.</span></span> <span data-ttu-id="56f2d-1116">Pokud automatickou vlastnost nemá přístupový objekt set, je považováno za pomocné pole `readonly` ([pole jen pro čtení](classes.md#readonly-fields)).</span><span class="sxs-lookup"><span data-stu-id="56f2d-1116">If the auto-property has no set accessor, the backing field is considered `readonly` ([Readonly fields](classes.md#readonly-fields)).</span></span> <span data-ttu-id="56f2d-1117">Stejně jako `readonly` pole, jen pro funkci getter automatickou vlastnost může být přiřazena také v těle konstruktoru nadřazené třídy.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1117">Just like a `readonly` field, a getter-only auto-property can also be assigned to in the body of a constructor of the enclosing class.</span></span> <span data-ttu-id="56f2d-1118">Taková přiřazení se přiřadí přímo pomocné pole jen pro čtení vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1118">Such an assignment assigns directly to the readonly backing field of the property.</span></span>

<span data-ttu-id="56f2d-1119">Automatické vlastnosti můžou mít *property_initializer*, které platí přímo do pole zálohování jako *variable_initializer* ([proměnné inicializátory](classes.md#variable-initializers)) .</span><span class="sxs-lookup"><span data-stu-id="56f2d-1119">An auto-property may optionally have a *property_initializer*, which is applied directly to the backing field as a *variable_initializer* ([Variable initializers](classes.md#variable-initializers)).</span></span>

<span data-ttu-id="56f2d-1120">V následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="56f2d-1120">The following example:</span></span>
```csharp
public class Point {
    public int X { get; set; } = 0;
    public int Y { get; set; } = 0;
}
```
<span data-ttu-id="56f2d-1121">je ekvivalentní následující deklarace:</span><span class="sxs-lookup"><span data-stu-id="56f2d-1121">is equivalent to the following declaration:</span></span>
```csharp
public class Point {
    private int __x = 0;
    private int __y = 0;
    public int X { get { return __x; } set { __x = value; } }
    public int Y { get { return __y; } set { __y = value; } }
}
```

<span data-ttu-id="56f2d-1122">V následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="56f2d-1122">The following example:</span></span>
```csharp
public class ReadOnlyPoint
{
    public int X { get; }
    public int Y { get; }
    public ReadOnlyPoint(int x, int y) { X = x; Y = y; }
}
```
<span data-ttu-id="56f2d-1123">je ekvivalentní následující deklarace:</span><span class="sxs-lookup"><span data-stu-id="56f2d-1123">is equivalent to the following declaration:</span></span>
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

<span data-ttu-id="56f2d-1124">Všimněte si, že přiřazení pole jen pro čtení jsou platné, protože k nim dojde v rámci konstruktoru.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1124">Notice that the assignments to the readonly field are legal, because they occur within the constructor.</span></span>


### <a name="accessibility"></a><span data-ttu-id="56f2d-1125">Usnadnění</span><span class="sxs-lookup"><span data-stu-id="56f2d-1125">Accessibility</span></span>

<span data-ttu-id="56f2d-1126">Pokud má přistupující objekt *accessor_modifier*, tak doména přístupnosti ([usnadnění domén](basic-concepts.md#accessibility-domains)) přístupového objektu je určen pomocí deklarovaná přístupnost člena *accessor_modifier* .</span><span class="sxs-lookup"><span data-stu-id="56f2d-1126">If an accessor has an *accessor_modifier*, the accessibility domain ([Accessibility domains](basic-concepts.md#accessibility-domains)) of the accessor is determined using the declared accessibility of the *accessor_modifier*.</span></span> <span data-ttu-id="56f2d-1127">Pokud nemá přistupující objekt *accessor_modifier*, tak doména přístupnosti přístupového objektu se určí na základě deklarovaná přístupnost člena vlastnost nebo indexer.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1127">If an accessor does not have an *accessor_modifier*, the accessibility domain of the accessor is determined from the declared accessibility of the property or indexer.</span></span>

<span data-ttu-id="56f2d-1128">Přítomnost *accessor_modifier* nikdy ovlivňuje člen vyhledávání ([operátory](expressions.md#operators)) nebo rozlišení přetěžování ([rozlišení přetěžování](expressions.md#overload-resolution)).</span><span class="sxs-lookup"><span data-stu-id="56f2d-1128">The presence of an *accessor_modifier* never affects member lookup ([Operators](expressions.md#operators)) or overload resolution ([Overload resolution](expressions.md#overload-resolution)).</span></span> <span data-ttu-id="56f2d-1129">Modifikátory pro vlastnost nebo indexovací člen vždy určit, která vlastnost nebo indexer je vázán na, bez ohledu na místní přístup.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1129">The modifiers on the property or indexer always determine which property or indexer is bound to, regardless of the context of the access.</span></span>

<span data-ttu-id="56f2d-1130">Jakmile byl vybrán konkrétní vlastnost nebo indexovací člen, usnadnění domén konkrétní přistupující objekty používané slouží k určení platnost tohoto využití:</span><span class="sxs-lookup"><span data-stu-id="56f2d-1130">Once a particular property or indexer has been selected, the accessibility domains of the specific accessors involved are used to determine if that usage is valid:</span></span>

*  <span data-ttu-id="56f2d-1131">Pokud jako hodnotu ([hodnot výrazů](expressions.md#values-of-expressions)), `get` přístupového objektu musí existovat a být přístupná.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1131">If the usage is as a value ([Values of expressions](expressions.md#values-of-expressions)), the `get` accessor must exist and be accessible.</span></span>
*  <span data-ttu-id="56f2d-1132">Pokud jako cíl jednoduché přiřazení ([jednoduché přiřazení](expressions.md#simple-assignment)), `set` přístupového objektu musí existovat a být přístupná.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1132">If the usage is as the target of a simple assignment ([Simple assignment](expressions.md#simple-assignment)), the `set` accessor must exist and be accessible.</span></span>
*  <span data-ttu-id="56f2d-1133">Při použití jako cíl složeného přiřazení ([složené přiřazení](expressions.md#compound-assignment)), nebo jako cíl `++` nebo `--` operátory ([funkce členy](expressions.md#function-members).9, [ Výrazy typu Invocation](expressions.md#invocation-expressions)), i `get` přístupové objekty a `set` přístupového objektu musí existovat a být přístupná.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1133">If the usage is as the target of compound assignment ([Compound assignment](expressions.md#compound-assignment)), or as the target of the `++` or `--` operators ([Function members](expressions.md#function-members).9, [Invocation expressions](expressions.md#invocation-expressions)), both the `get` accessors and the `set` accessor must exist and be accessible.</span></span>

<span data-ttu-id="56f2d-1134">V následujícím příkladu, vlastnost `A.Text` byl skrytý za vlastnost `B.Text`, i v kontextech, kde je to jenom `set` přístupového objektu je volána.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1134">In the following example, the property `A.Text` is hidden by the property `B.Text`, even in contexts where only the `set` accessor is called.</span></span> <span data-ttu-id="56f2d-1135">Naproti tomu, vlastnost `B.Count` není přístupná pro třídy `M`, takže vlastnost přístupné `A.Count` místo ní se použije.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1135">In contrast, the property `B.Count` is not accessible to class `M`, so the accessible property `A.Count` is used instead.</span></span>

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

<span data-ttu-id="56f2d-1136">Přístupový objekt, který se používá k implementaci rozhraní nemusí mít *accessor_modifier*.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1136">An accessor that is used to implement an interface may not have an *accessor_modifier*.</span></span> <span data-ttu-id="56f2d-1137">Pokud pouze jeden přistupující objekt se používá k implementaci rozhraní, další přístupový objekt mohou být deklarovány pomocí *accessor_modifier*:</span><span class="sxs-lookup"><span data-stu-id="56f2d-1137">If only one accessor is used to implement an interface, the other accessor may be declared with an *accessor_modifier*:</span></span>
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

### <a name="virtual-sealed-override-and-abstract-property-accessors"></a><span data-ttu-id="56f2d-1138">Virtuální zapečetěná, přepsání a přístupové objekty abstraktní vlastnosti</span><span class="sxs-lookup"><span data-stu-id="56f2d-1138">Virtual, sealed, override, and abstract property accessors</span></span>

<span data-ttu-id="56f2d-1139">A `virtual` deklaraci vlastnosti určuje, že jsou virtuální přistupující objekty vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1139">A `virtual` property declaration specifies that the accessors of the property are virtual.</span></span> <span data-ttu-id="56f2d-1140">`virtual` Modifikátor se vztahuje na oba přistupující objekty vlastnosti pro čtení i zápis, není možné, pouze jeden přistupující objekt vlastnosti pro čtení a zápis na virtuální.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1140">The `virtual` modifier applies to both accessors of a read-write property—it is not possible for only one accessor of a read-write property to be virtual.</span></span>

<span data-ttu-id="56f2d-1141">`abstract` Deklaraci vlastnosti určuje, že jsou virtuální přistupující objekty vlastnosti, ale neposkytuje Skutečná implementace přístupové objekty.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1141">An `abstract` property declaration specifies that the accessors of the property are virtual, but does not provide an actual implementation of the accessors.</span></span> <span data-ttu-id="56f2d-1142">Odvozené neabstraktní třídy místo toho je potřeba poskytli vlastní implementaci pro přístupové objekty tak, že přepíšete vlastnost.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1142">Instead, non-abstract derived classes are required to provide their own implementation for the accessors by overriding the property.</span></span> <span data-ttu-id="56f2d-1143">Protože přístupový objekt pro deklaraci abstraktní vlastnosti poskytuje skutečné implementaci, jeho *accessor_body* jednoduše se skládá ze středníku.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1143">Because an accessor for an abstract property declaration provides no actual implementation, its *accessor_body* simply consists of a semicolon.</span></span>

<span data-ttu-id="56f2d-1144">Deklarace vlastnosti, která zahrnuje i `abstract` a `override` modifikátory Určuje, že vlastnost je abstraktní a přepisuje základní vlastnost.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1144">A property declaration that includes both the `abstract` and `override` modifiers specifies that the property is abstract and overrides a base property.</span></span> <span data-ttu-id="56f2d-1145">Přístupové objekty tyto vlastnosti jsou také abstraktní.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1145">The accessors of such a property are also abstract.</span></span>

<span data-ttu-id="56f2d-1146">Abstraktní vlastnost deklarace jsou povolené jenom v abstraktní třídy ([abstraktní třídy](classes.md#abstract-classes)). Přistupující objekty vlastnosti zděděné virtuální lze přepsat v odvozené třídě včetně deklaraci vlastnosti, která určuje, `override` směrnice.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1146">Abstract property declarations are only permitted in abstract classes ([Abstract classes](classes.md#abstract-classes)).The accessors of an inherited virtual property can be overridden in a derived class by including a property declaration that specifies an `override` directive.</span></span> <span data-ttu-id="56f2d-1147">Jedná se ***přepsání deklarace vlastnosti***.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1147">This is known as an ***overriding property declaration***.</span></span> <span data-ttu-id="56f2d-1148">Přepsání deklarace vlastnosti nedeklaruje novou vlastnost.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1148">An overriding property declaration does not declare a new property.</span></span> <span data-ttu-id="56f2d-1149">Místo toho ji jednoduše se specializuje implementace přistupující objekty existující virtuální vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1149">Instead, it simply specializes the implementations of the accessors of an existing virtual property.</span></span>

<span data-ttu-id="56f2d-1150">Přepsání deklarace vlastnost musíte zadat přesně stejné modifikátory, typ a název jako zděděné vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1150">An overriding property declaration must specify the exact same accessibility modifiers, type, and name as the inherited property.</span></span> <span data-ttu-id="56f2d-1151">Pokud zděděnou vlastnost má pouze jeden přistupující objekt (například pokud zděděnou vlastnost je jen pro čtení nebo jen pro zápis), vlastnost přepsání musí obsahovat pouze přistupujícího objektu.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1151">If the inherited property has only a single accessor (i.e., if the inherited property is read-only or write-only), the overriding property must include only that accessor.</span></span> <span data-ttu-id="56f2d-1152">Pokud zděděné vlastnosti obsahuje oba přistupující objekty (tj. Pokud zděděné vlastnosti je čtení a zápis), vlastnost přepsání může obsahovat jeden přistupující objekt nebo oba přistupující objekty.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1152">If the inherited property includes both accessors (i.e., if the inherited property is read-write), the overriding property can include either a single accessor or both accessors.</span></span>

<span data-ttu-id="56f2d-1153">Může obsahovat deklaraci vlastnosti přepsání `sealed` modifikátor.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1153">An overriding property declaration may include the `sealed` modifier.</span></span> <span data-ttu-id="56f2d-1154">Použití tohoto modifikátoru zabrání další přepisuje vlastnost odvozené třídy.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1154">Use of this modifier prevents a derived class from further overriding the property.</span></span> <span data-ttu-id="56f2d-1155">Přístupové objekty zapečetěné vlastnosti jsou také zapečetěné.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1155">The accessors of a sealed property are also sealed.</span></span>

<span data-ttu-id="56f2d-1156">Až na pár rozdílů v deklarace a volání syntaxe, virtuální, zapečetěné, přepsání a abstraktní přistupující objekty chovají stejně jako virtuální, zapečetěné, přepsání a abstraktní metody.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1156">Except for differences in declaration and invocation syntax, virtual, sealed, override, and abstract accessors behave exactly like virtual, sealed, override and abstract methods.</span></span> <span data-ttu-id="56f2d-1157">Konkrétně pravidel popsaných v [virtuální metody](classes.md#virtual-methods), [přepište metody](classes.md#override-methods), [zapečetěné metody](classes.md#sealed-methods), a [abstraktní metody](classes.md#abstract-methods) použít jako přístupové objekty byly metody odpovídající formuláře:</span><span class="sxs-lookup"><span data-stu-id="56f2d-1157">Specifically, the rules described in [Virtual methods](classes.md#virtual-methods), [Override methods](classes.md#override-methods), [Sealed methods](classes.md#sealed-methods), and [Abstract methods](classes.md#abstract-methods) apply as if accessors were methods of a corresponding form:</span></span>

*  <span data-ttu-id="56f2d-1158">A `get` přistupující objekt odpovídající konstruktor bez parametrů metody s návratovou hodnotou vlastnosti typu a stejného modifikátory jako příslušná vlastnost.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1158">A `get` accessor corresponds to a parameterless method with a return value of the property type and the same modifiers as the containing property.</span></span>
*  <span data-ttu-id="56f2d-1159">A `set` přistupující objekt odpovídající metodu s jednou hodnotou parametrem typu vlastnosti `void` návratový typ a modifikátory stejné jako příslušná vlastnost.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1159">A `set` accessor corresponds to a method with a single value parameter of the property type, a `void` return type, and the same modifiers as the containing property.</span></span>

<span data-ttu-id="56f2d-1160">V příkladu</span><span class="sxs-lookup"><span data-stu-id="56f2d-1160">In the example</span></span>
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
<span data-ttu-id="56f2d-1161">`X` je virtuální vlastnost jen pro čtení, `Y` je virtuální vlastnost pro čtení i zápis, a `Z` je abstraktní vlastnost pro čtení i zápis.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1161">`X` is a virtual read-only property, `Y` is a virtual read-write property, and `Z` is an abstract read-write property.</span></span> <span data-ttu-id="56f2d-1162">Protože `Z` je abstraktní třídu obsahující `A` také musí být deklarovaná jako abstraktní.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1162">Because `Z` is abstract, the containing class `A` must also be declared abstract.</span></span>

<span data-ttu-id="56f2d-1163">Třída, která je odvozena z `A` se zobrazuje níže:</span><span class="sxs-lookup"><span data-stu-id="56f2d-1163">A class that derives from `A` is show below:</span></span>
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

<span data-ttu-id="56f2d-1164">Tady, prohlášení o `X`, `Y`, a `Z` jsou přepsání deklarace vlastností.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1164">Here, the declarations of `X`, `Y`, and `Z` are overriding property declarations.</span></span> <span data-ttu-id="56f2d-1165">Každá vlastnost deklarace přesně shoduje se modifikátory, typ a název odpovídající zděděné vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1165">Each property declaration exactly matches the accessibility modifiers, type, and name of the corresponding inherited property.</span></span> <span data-ttu-id="56f2d-1166">`get` Přistupující objekt `X` a `set` přistupující objekt `Y` použít `base` – klíčové slovo pro přístup k zděděné přistupující objekty.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1166">The `get` accessor of `X` and the `set` accessor of `Y` use the `base` keyword to access the inherited accessors.</span></span> <span data-ttu-id="56f2d-1167">Deklarace `Z` přepíše přistupující objekty jak abstraktní – proto nejsou žádní členové nezpracovaných abstraktní funkce v `B`, a `B` smí být neabstraktní třída.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1167">The declaration of `Z` overrides both abstract accessors—thus, there are no outstanding abstract function members in `B`, and `B` is permitted to be a non-abstract class.</span></span>

<span data-ttu-id="56f2d-1168">Když je vlastnost deklarován jako `override`, všechny přistupující objekty přepsané musí být přístupná pro přepsání kódu.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1168">When a property is declared as an `override`, any overridden accessors must be accessible to the overriding code.</span></span> <span data-ttu-id="56f2d-1169">Kromě toho je deklarovaná přístupnost vlastnosti nebo indexeru, samotného a přistupujících objektů, se musí shodovat s přepsanému členu a přístupové objekty.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1169">In addition, the declared accessibility of both the property or indexer itself, and of the accessors, must match that of the overridden member and accessors.</span></span> <span data-ttu-id="56f2d-1170">Příklad:</span><span class="sxs-lookup"><span data-stu-id="56f2d-1170">For example:</span></span>
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

## <a name="events"></a><span data-ttu-id="56f2d-1171">Události</span><span class="sxs-lookup"><span data-stu-id="56f2d-1171">Events</span></span>

<span data-ttu-id="56f2d-1172">***Události*** je člen, který umožňuje objektu nebo třídě poskytnout oznámení.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1172">An ***event*** is a member that enables an object or class to provide notifications.</span></span> <span data-ttu-id="56f2d-1173">Klienti mohou připojit spustitelný kód pro události zadáním ***obslužné rutiny událostí***.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1173">Clients can attach executable code for events by supplying ***event handlers***.</span></span>

<span data-ttu-id="56f2d-1174">Události jsou deklarovány pomocí *event_declaration*s:</span><span class="sxs-lookup"><span data-stu-id="56f2d-1174">Events are declared using *event_declaration*s:</span></span>

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

<span data-ttu-id="56f2d-1175">*Event_declaration* může zahrnovat sadu *atributy* ([atributy](attributes.md)) a platnou kombinaci čtyři přístupu modifikátory přístupu ([modifikátorů přístupu ](classes.md#access-modifiers)), `new` ([new – modifikátor](classes.md#the-new-modifier)), `static` ([statická a instanci metody](classes.md#static-and-instance-methods)), `virtual` ([virtuální metody](classes.md#virtual-methods)), `override` ([Přepište metody](classes.md#override-methods)), `sealed` ([zapečetěné metody](classes.md#sealed-methods)), `abstract` ([abstraktní metody](classes.md#abstract-methods)), a `extern` ([Externí metody](classes.md#external-methods)) modifikátory.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1175">An *event_declaration* may include a set of *attributes* ([Attributes](attributes.md)) and a valid combination of the four access modifiers ([Access modifiers](classes.md#access-modifiers)), the `new` ([The new modifier](classes.md#the-new-modifier)),  `static` ([Static and instance methods](classes.md#static-and-instance-methods)), `virtual` ([Virtual methods](classes.md#virtual-methods)), `override` ([Override methods](classes.md#override-methods)), `sealed` ([Sealed methods](classes.md#sealed-methods)), `abstract` ([Abstract methods](classes.md#abstract-methods)), and `extern` ([External methods](classes.md#external-methods)) modifiers.</span></span>

<span data-ttu-id="56f2d-1176">Deklarace událostí se vztahují stejná pravidla jako deklarace metody ([metody](classes.md#methods)) s ohledem na platné kombinace modifikátorů.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1176">Event declarations are subject to the same rules as method declarations ([Methods](classes.md#methods)) with regard to valid combinations of modifiers.</span></span>

<span data-ttu-id="56f2d-1177">*Typ* události musí být *delegate_type* ([referenční typy](types.md#reference-types)) a že *delegate_type* musí být dlouhý aspoň jako dostupné jako samotné události ([usnadnění omezení](basic-concepts.md#accessibility-constraints)).</span><span class="sxs-lookup"><span data-stu-id="56f2d-1177">The *type* of an event declaration must be a *delegate_type* ([Reference types](types.md#reference-types)), and that *delegate_type* must be at least as accessible as the event itself ([Accessibility constraints](basic-concepts.md#accessibility-constraints)).</span></span>

<span data-ttu-id="56f2d-1178">Deklaraci události může zahrnovat *event_accessor_declarations*.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1178">An event declaration may include *event_accessor_declarations*.</span></span> <span data-ttu-id="56f2d-1179">Nicméně pokud tomu tak není, pro jiné externí, neabstraktní událostí, kompilátor poskytuje je automaticky ([události podobné poli](classes.md#field-like-events)); pro externí události přístupové objekty jsou k dispozici externě.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1179">However, if it does not, for non-extern, non-abstract events, the compiler supplies them automatically ([Field-like events](classes.md#field-like-events)); for extern events, the accessors are provided externally.</span></span>

<span data-ttu-id="56f2d-1180">Deklaraci události, která vynechává *event_accessor_declarations* definuje jeden nebo více událostí – jeden pro každou z *variable_declarator*s.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1180">An event declaration that omits *event_accessor_declarations* defines one or more events—one for each of the *variable_declarator*s.</span></span> <span data-ttu-id="56f2d-1181">Atributy a modifikátory platí pro všechny členy deklarované pomocí takové *event_declaration*.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1181">The attributes and modifiers apply to all of the members declared by such an *event_declaration*.</span></span>

<span data-ttu-id="56f2d-1182">Je chyba kompilace pro *event_declaration* oba `abstract` modifikátor a oddělené složenou závorku *event_accessor_declarations*.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1182">It is a compile-time error for an *event_declaration* to include both the `abstract` modifier and brace-delimited *event_accessor_declarations*.</span></span>

<span data-ttu-id="56f2d-1183">Pokud obsahuje deklaraci události `extern` modifikátor, události se říká, že ***externí událost***.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1183">When an event declaration includes an `extern` modifier, the event is said to be an ***external event***.</span></span> <span data-ttu-id="56f2d-1184">Protože deklaraci externí události poskytuje skutečné implementaci, jedná se o chybu, aby se oba `extern` modifikátor a *event_accessor_declarations*.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1184">Because an external event declaration provides no actual implementation, it is an error for it to include both the `extern` modifier and *event_accessor_declarations*.</span></span>

<span data-ttu-id="56f2d-1185">Je chyba kompilace pro *variable_declarator* deklarace událostí pomocí `abstract` nebo `external` modifikátor zahrnout *variable_initializer*.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1185">It is a compile-time error for a *variable_declarator* of an event declaration with an `abstract` or `external` modifier to include a *variable_initializer*.</span></span>

<span data-ttu-id="56f2d-1186">Událost lze použít jako operand na levé straně `+=` a `-=` operátory ([události přiřazení](expressions.md#event-assignment)).</span><span class="sxs-lookup"><span data-stu-id="56f2d-1186">An event can be used as the left-hand operand of the `+=` and `-=` operators ([Event assignment](expressions.md#event-assignment)).</span></span> <span data-ttu-id="56f2d-1187">Tyto operátory používají, připojit obslužných rutin událostí k a odebrání obslužné rutiny události z události, a řízení přístupu modifikátory přístupu události kontextech, ve kterých jsou povolené tyto operace.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1187">These operators are used, respectively, to attach event handlers to or to remove event handlers from an event, and the access modifiers of the event control the contexts in which such operations are permitted.</span></span>

<span data-ttu-id="56f2d-1188">Protože `+=` a `-=` jsou jediné operace, které jsou povoleny pro událost mimo typ, který deklaruje událost, externí kód můžete přidat a odebrat obslužné rutiny události, ale nelze žádným způsobem získat nebo upravit základní seznam událostí obslužné rutiny.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1188">Since `+=` and `-=` are the only operations that are permitted on an event outside the type that declares the event, external code can add and remove handlers for an event, but cannot in any other way obtain or modify the underlying list of event handlers.</span></span>

<span data-ttu-id="56f2d-1189">V operaci formuláře `x += y` nebo `x -= y`, když `x` je událost a odkaz na něho mimo typ, který obsahuje deklaraci `x`, výsledek operace má typ `void` (na rozdíl od nutnosti typ `x`, s hodnotou `x` po přiřazení).</span><span class="sxs-lookup"><span data-stu-id="56f2d-1189">In an operation of the form `x += y` or `x -= y`, when `x` is an event and the reference takes place outside the type that contains the declaration of `x`, the result of the operation has type `void` (as opposed to having the type of `x`, with the value of `x` after the assignment).</span></span> <span data-ttu-id="56f2d-1190">Toto pravidlo zakazuje externí kód nepřímo zkoumání základního delegáta události.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1190">This rule prohibits external code from indirectly examining the underlying delegate of an event.</span></span>

<span data-ttu-id="56f2d-1191">Následující příklad ukazuje, jak obslužné rutiny událostí jsou připojené k instancím typu `Button` třídy:</span><span class="sxs-lookup"><span data-stu-id="56f2d-1191">The following example shows how event handlers are attached to instances of the `Button` class:</span></span>
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

<span data-ttu-id="56f2d-1192">Tady `LoginDialog` konstruktor instance vytvoří dva `Button` instance a připojí obslužných rutin událostí k `Click` události.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1192">Here, the `LoginDialog` instance constructor creates two `Button` instances and attaches event handlers to the `Click` events.</span></span>

### <a name="field-like-events"></a><span data-ttu-id="56f2d-1193">Události podobné poli</span><span class="sxs-lookup"><span data-stu-id="56f2d-1193">Field-like events</span></span>

<span data-ttu-id="56f2d-1194">V rámci programu textu dané třídy nebo struktury, která obsahuje deklaraci události některé události lze použít jako pole.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1194">Within the program text of the class or struct that contains the declaration of an event, certain events can be used like fields.</span></span> <span data-ttu-id="56f2d-1195">Použije tímto způsobem, nesmí být událost `abstract` nebo `extern`a nesmějí obsahovat explicitně *event_accessor_declarations*.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1195">To be used in this way, an event must not be `abstract` or `extern`, and must not explicitly include *event_accessor_declarations*.</span></span> <span data-ttu-id="56f2d-1196">Tato událost lze použít v libovolném kontextu, který umožňuje pole.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1196">Such an event can be used in any context that permits a field.</span></span> <span data-ttu-id="56f2d-1197">Toto pole obsahuje delegáta ([delegáti](delegates.md)) odkazuje na seznam obslužných rutin událostí, které byly přidány k této události.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1197">The field contains a delegate ([Delegates](delegates.md)) which refers to the list of event handlers that have been added to the event.</span></span> <span data-ttu-id="56f2d-1198">Pokud nebyly přidány žádné obslužné rutiny událostí, bude pole obsahovat `null`.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1198">If no event handlers have been added, the field contains `null`.</span></span>

<span data-ttu-id="56f2d-1199">V příkladu</span><span class="sxs-lookup"><span data-stu-id="56f2d-1199">In the example</span></span>
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
<span data-ttu-id="56f2d-1200">`Click` slouží jako pole v rámci `Button` třídy.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1200">`Click` is used as a field within the `Button` class.</span></span> <span data-ttu-id="56f2d-1201">Jak ukazuje příklad, pole může být prověřit, upravit a použít ve výrazech vyvolání delegáta.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1201">As the example demonstrates, the field can be examined, modified, and used in delegate invocation expressions.</span></span> <span data-ttu-id="56f2d-1202">`OnClick` Metodu `Button` třídy "vyvolá" `Click` událostí.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1202">The `OnClick` method in the `Button` class "raises" the `Click` event.</span></span> <span data-ttu-id="56f2d-1203">Pojem vyvolání události je přesně ekvivalentní k vyvolání delegáta představované událostí – to znamená, nejsou žádné zvláštní jazykovým konstrukcím pro vyvolání události.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1203">The notion of raising an event is precisely equivalent to invoking the delegate represented by the event—thus, there are no special language constructs for raising events.</span></span> <span data-ttu-id="56f2d-1204">Všimněte si, že při vyvolání delegáta předchází kontrolou, která zajistí, že delegát je jiná než null.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1204">Note that the delegate invocation is preceded by a check that ensures the delegate is non-null.</span></span>

<span data-ttu-id="56f2d-1205">Mimo deklaraci `Button` třídy, `Click` člena lze použít pouze na levé straně `+=` a `-=` operátory, jako v</span><span class="sxs-lookup"><span data-stu-id="56f2d-1205">Outside the declaration of the `Button` class, the `Click` member can only be used on the left-hand side of the `+=` and `-=` operators, as in</span></span>
```csharp
b.Click += new EventHandler(...);
```
<span data-ttu-id="56f2d-1206">která připojí do seznamu vyvolání delegáta `Click` události, a</span><span class="sxs-lookup"><span data-stu-id="56f2d-1206">which appends a delegate to the invocation list of the `Click` event, and</span></span>
```csharp
b.Click -= new EventHandler(...);
```
<span data-ttu-id="56f2d-1207">které odebere ze seznamu vyvolání delegáta `Click` událostí.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1207">which removes a delegate from the invocation list of the `Click` event.</span></span>

<span data-ttu-id="56f2d-1208">Při kompilaci pole podobných událostí, kompilátor automaticky vytvoří úložiště pro uložení delegáta a vytvoří přistupující objekty události, které přidání nebo odebrání obslužných rutin událostí k pole delegáta.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1208">When compiling a field-like event, the compiler automatically creates storage to hold the delegate, and creates accessors for the event that add or remove event handlers to the delegate field.</span></span> <span data-ttu-id="56f2d-1209">Operace přidání a odebrání jsou bezpečná a může (ale nemusíte) být neučinili při držení zámku ([příkaz lock](statements.md#the-lock-statement)) na objekt obsahující instanci události, nebo objekt typu ([anonymní výrazy vytvoření objektu](expressions.md#anonymous-object-creation-expressions)) pro statické události.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1209">The addition and removal operations are thread safe, and may (but are not required to) be done while holding the lock ([The lock statement](statements.md#the-lock-statement)) on the containing object for an instance event, or the type object ([Anonymous object creation expressions](expressions.md#anonymous-object-creation-expressions)) for a static event.</span></span>

<span data-ttu-id="56f2d-1210">Proto deklaraci události instance formuláře:</span><span class="sxs-lookup"><span data-stu-id="56f2d-1210">Thus, an instance event declaration of the form:</span></span>
```csharp
class X
{
    public event D Ev;
}
```
<span data-ttu-id="56f2d-1211">bude zkompilována do něco ekvivalentní:</span><span class="sxs-lookup"><span data-stu-id="56f2d-1211">will be compiled to something equivalent to:</span></span>
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
<span data-ttu-id="56f2d-1212">V rámci třídy `X`, odkazy na `Ev` na levé straně `+=` a `-=` operátory způsobit přidat a odebrat přístupové objekty, které má být volána.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1212">Within the class `X`, references to `Ev` on the left-hand side of the `+=` and `-=` operators cause the add and remove accessors to be invoked.</span></span> <span data-ttu-id="56f2d-1213">Všechny odkazy na `Ev` jsou kompilovány do odkazu skryté pole `__Ev` místo ([přístup ke členu](expressions.md#member-access)).</span><span class="sxs-lookup"><span data-stu-id="56f2d-1213">All other references to `Ev` are compiled to reference the hidden field `__Ev` instead ([Member access](expressions.md#member-access)).</span></span> <span data-ttu-id="56f2d-1214">Název "`__Ev`" libovolný; skryté pole může mít libovolný název nebo není název vůbec.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1214">The name "`__Ev`" is arbitrary; the hidden field could have any name or no name at all.</span></span>

### <a name="event-accessors"></a><span data-ttu-id="56f2d-1215">Přístupové objekty událostí</span><span class="sxs-lookup"><span data-stu-id="56f2d-1215">Event accessors</span></span>

<span data-ttu-id="56f2d-1216">Deklarace událostí obvykle vynechat *event_accessor_declarations*, například `Button` výše uvedený příklad.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1216">Event declarations typically omit *event_accessor_declarations*, as in the `Button` example above.</span></span> <span data-ttu-id="56f2d-1217">Jedna situace to tedy zahrnuje případ, ve kterém není přijatelná náklady na uložení jednoho pole na událost.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1217">One situation for doing so involves the case in which the storage cost of one field per event is not acceptable.</span></span> <span data-ttu-id="56f2d-1218">V takových případech může obsahovat třídu *event_accessor_declarations* a použít privátní mechanismus pro ukládání seznamu obslužných rutin událostí.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1218">In such cases, a class can include *event_accessor_declarations* and use a private mechanism for storing the list of event handlers.</span></span>

<span data-ttu-id="56f2d-1219">*Event_accessor_declarations* události zadejte spustitelné příkazy, které jsou přidružené k přidávání a odebírání obslužných rutin událostí.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1219">The *event_accessor_declarations* of an event specify the executable statements associated with adding and removing event handlers.</span></span>

<span data-ttu-id="56f2d-1220">Deklarace přistupujícího objektu se skládají z *add_accessor_declaration* a *remove_accessor_declaration*.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1220">The accessor declarations consist of an *add_accessor_declaration* and a *remove_accessor_declaration*.</span></span> <span data-ttu-id="56f2d-1221">Každá deklarace přistupujícího objektu se skládá z tokenu `add` nebo `remove` následovaný *bloku*.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1221">Each accessor declaration consists of the token `add` or `remove` followed by a *block*.</span></span> <span data-ttu-id="56f2d-1222">*Bloku* přidružené *add_accessor_declaration* určuje příkazy ke spuštění při přidání obslužné rutiny události a *bloku* přidružené *remove_accessor_declaration* určuje příkazy ke spuštění při odebrání obslužné rutiny události.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1222">The *block* associated with an *add_accessor_declaration* specifies the statements to execute when an event handler is added, and the *block* associated with a *remove_accessor_declaration* specifies the statements to execute when an event handler is removed.</span></span>

<span data-ttu-id="56f2d-1223">Každý *add_accessor_declaration* a *remove_accessor_declaration* odpovídá metodu s jednou hodnotou parametru typu události a `void` návratového typu.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1223">Each *add_accessor_declaration* and *remove_accessor_declaration* corresponds to a method with a single value parameter of the event type and a `void` return type.</span></span> <span data-ttu-id="56f2d-1224">Implicitní parametr přístupový objekt události je pojmenován `value`.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1224">The implicit parameter of an event accessor is named `value`.</span></span> <span data-ttu-id="56f2d-1225">Při použití události v úloze událostí se používá přístupového objektu příslušné události.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1225">When an event is used in an event assignment, the appropriate event accessor is used.</span></span> <span data-ttu-id="56f2d-1226">Konkrétně Pokud operátor přiřazení je `+=` použije přístupový objekt add a operátor přiřazení je `-=` použije přístupový objekt remove.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1226">Specifically, if the assignment operator is `+=` then the add accessor is used, and if the assignment operator is `-=` then the remove accessor is used.</span></span> <span data-ttu-id="56f2d-1227">V obou případech zpracovával pravý operand operátoru přiřazení slouží jako argument přístupového objektu události.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1227">In either case, the right-hand operand of the assignment operator is used as the argument to the event accessor.</span></span> <span data-ttu-id="56f2d-1228">Blok *add_accessor_declaration* nebo *remove_accessor_declaration* musí splňovat pravidla pro `void` metod popsaných v [tělo metody](classes.md#method-body).</span><span class="sxs-lookup"><span data-stu-id="56f2d-1228">The block of an *add_accessor_declaration* or a *remove_accessor_declaration* must conform to the rules for `void` methods described in [Method body](classes.md#method-body).</span></span> <span data-ttu-id="56f2d-1229">Zejména `return` příkazy v těchto bloku není povoleno zadat výraz.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1229">In particular, `return` statements in such a block are not permitted to specify an expression.</span></span>

<span data-ttu-id="56f2d-1230">Protože přístupový objekt události má parametr s názvem implicitně `value`, je chyba kompilace pro místní proměnné nebo konstantní deklarované v přístupový objekt události s tímto názvem.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1230">Since an event accessor implicitly has a parameter named `value`, it is a compile-time error for a local variable or constant declared in an event accessor to have that name.</span></span>

<span data-ttu-id="56f2d-1231">V příkladu</span><span class="sxs-lookup"><span data-stu-id="56f2d-1231">In the example</span></span>
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
<span data-ttu-id="56f2d-1232">`Control` třída implementuje mechanismus interní úložiště pro události.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1232">the `Control` class implements an internal storage mechanism for events.</span></span> <span data-ttu-id="56f2d-1233">`AddEventHandler` Metoda přidruží delegáta hodnotu s klíčem, `GetEventHandler` delegáta aktuálně přidružené s klíčem, vrátí metoda hodnotu a `RemoveEventHandler` metoda odstraní delegáta jako obslužnou rutinu události pro zadané události.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1233">The `AddEventHandler` method associates a delegate value with a key, the `GetEventHandler` method returns the delegate currently associated with a key, and the `RemoveEventHandler` method removes a delegate as an event handler for the specified event.</span></span> <span data-ttu-id="56f2d-1234">Pravděpodobně základní mechanismus úložiště je navržená tak, aby se neúčtují žádné poplatky pro přidružení `null` delegovat hodnotu s klíčem, a proto neošetřené události využívat žádné úložiště.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1234">Presumably, the underlying storage mechanism is designed such that there is no cost for associating a `null` delegate value with a key, and thus unhandled events consume no storage.</span></span>

### <a name="static-and-instance-events"></a><span data-ttu-id="56f2d-1235">Statické a instance události</span><span class="sxs-lookup"><span data-stu-id="56f2d-1235">Static and instance events</span></span>

<span data-ttu-id="56f2d-1236">Pokud obsahuje deklaraci události `static` modifikátor, události se říká, že ***statické události***.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1236">When an event declaration includes a `static` modifier, the event is said to be a ***static event***.</span></span> <span data-ttu-id="56f2d-1237">Pokud ne `static` Modifikátor je k dispozici, události se říká, že ***událost instance***.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1237">When no `static` modifier is present, the event is said to be an ***instance event***.</span></span>

<span data-ttu-id="56f2d-1238">Statické události není přidružena k určité instanci a je chyba kompilace k odkazování na `this` v přístupových objektů statické události.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1238">A static event is not associated with a specific instance, and it is a compile-time error to refer to `this` in the accessors of a static event.</span></span>

<span data-ttu-id="56f2d-1239">Instance události je přidružen k dané instanci třídy a tato instance je přístupná jako `this` ([tento přístup](expressions.md#this-access)) v přístupové objekty z této události.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1239">An instance event is associated with a given instance of a class, and this instance can be accessed as `this` ([This access](expressions.md#this-access)) in the accessors of that event.</span></span>

<span data-ttu-id="56f2d-1240">Kdy se událost odkazuje v *member_access* ([přístup ke členu](expressions.md#member-access)) ve formátu `E.M`, pokud `M` je statická, `E` musí označují typ obsahující `M`a pokud `M` je událost instance E musí označit instanci typu, který obsahuje `M`.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1240">When an event is referenced in a *member_access* ([Member access](expressions.md#member-access)) of the form `E.M`, if `M` is a static event, `E` must denote a type containing `M`, and if `M` is an instance event, E must denote an instance of a type containing `M`.</span></span>

<span data-ttu-id="56f2d-1241">Rozdíly mezi statické a členy instance jsou popsány dále v [statické a instance členy](classes.md#static-and-instance-members).</span><span class="sxs-lookup"><span data-stu-id="56f2d-1241">The differences between static and instance members are discussed further in [Static and instance members](classes.md#static-and-instance-members).</span></span>

### <a name="virtual-sealed-override-and-abstract-event-accessors"></a><span data-ttu-id="56f2d-1242">Virtuální zapečetěná, přepsání a přístupových objektů událostí abstraktní</span><span class="sxs-lookup"><span data-stu-id="56f2d-1242">Virtual, sealed, override, and abstract event accessors</span></span>

<span data-ttu-id="56f2d-1243">A `virtual` deklaraci události Určuje, že přístupové objekty z této události je virtuální.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1243">A `virtual` event declaration specifies that the accessors of that event are virtual.</span></span> <span data-ttu-id="56f2d-1244">`virtual` Modifikátor se vztahuje na oba přistupující objekty události.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1244">The `virtual` modifier applies to both accessors of an event.</span></span>

<span data-ttu-id="56f2d-1245">`abstract` Deklaraci události Určuje, že jsou virtuální přistupující objekty události, ale neposkytuje Skutečná implementace přístupové objekty.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1245">An `abstract` event declaration specifies that the accessors of the event are virtual, but does not provide an actual implementation of the accessors.</span></span> <span data-ttu-id="56f2d-1246">Odvozené neabstraktní třídy místo toho je potřeba poskytli vlastní implementaci pro přístupové objekty tak, že přepíšete události.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1246">Instead, non-abstract derived classes are required to provide their own implementation for the accessors by overriding the event.</span></span> <span data-ttu-id="56f2d-1247">Protože deklarace abstraktní událost poskytuje skutečné implementaci, nemůže poskytnout oddělených složenou závorku *event_accessor_declarations*.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1247">Because an abstract event declaration provides no actual implementation, it cannot provide brace-delimited *event_accessor_declarations*.</span></span>

<span data-ttu-id="56f2d-1248">Deklaraci události, která zahrnuje i `abstract` a `override` modifikátory Určuje, že událost je abstraktní a přepisuje základní událost.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1248">An event declaration that includes both the `abstract` and `override` modifiers specifies that the event is abstract and overrides a base event.</span></span> <span data-ttu-id="56f2d-1249">Přístupové objekty tyto události jsou také abstraktní.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1249">The accessors of such an event are also abstract.</span></span>

<span data-ttu-id="56f2d-1250">Deklarace abstraktních událostí jsou povolené jenom v abstraktní třídy ([abstraktní třídy](classes.md#abstract-classes)).</span><span class="sxs-lookup"><span data-stu-id="56f2d-1250">Abstract event declarations are only permitted in abstract classes ([Abstract classes](classes.md#abstract-classes)).</span></span>

<span data-ttu-id="56f2d-1251">Přistupující objekty zděděné virtuální událost lze přepsat v odvozené třídě včetně deklaraci události, která určuje, `override` modifikátor.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1251">The accessors of an inherited virtual event can be overridden in a derived class by including an event declaration that specifies an `override` modifier.</span></span> <span data-ttu-id="56f2d-1252">Jedná se ***přepsání deklarace události***.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1252">This is known as an ***overriding event declaration***.</span></span> <span data-ttu-id="56f2d-1253">Přepsání deklarace události nedeklaruje novou událost.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1253">An overriding event declaration does not declare a new event.</span></span> <span data-ttu-id="56f2d-1254">Místo toho ji jednoduše se specializuje implementace přistupující objekty existující virtuální událost.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1254">Instead, it simply specializes the implementations of the accessors of an existing virtual event.</span></span>

<span data-ttu-id="56f2d-1255">Přepsání deklarace události musíte zadat přesně stejné modifikátory, typ a název jako přepsané událost.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1255">An overriding event declaration must specify the exact same accessibility modifiers, type, and name as the overridden event.</span></span>

<span data-ttu-id="56f2d-1256">Přepsání deklarace události může zahrnovat `sealed` modifikátor.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1256">An overriding event declaration may include the `sealed` modifier.</span></span> <span data-ttu-id="56f2d-1257">Použití tohoto modifikátoru zabrání přepsání další události odvozené třídy.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1257">Use of this modifier prevents a derived class from further overriding the event.</span></span> <span data-ttu-id="56f2d-1258">Přístupové objekty zapečetěné události jsou také zapečetěné.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1258">The accessors of a sealed event are also sealed.</span></span>

<span data-ttu-id="56f2d-1259">Je chyba kompilace pro přepsání deklarace události mají zahrnout `new` modifikátor.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1259">It is a compile-time error for an overriding event declaration to include a `new` modifier.</span></span>

<span data-ttu-id="56f2d-1260">Až na pár rozdílů v deklarace a volání syntaxe, virtuální, zapečetěné, přepsání a abstraktní přistupující objekty chovají stejně jako virtuální, zapečetěné, přepsání a abstraktní metody.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1260">Except for differences in declaration and invocation syntax, virtual, sealed, override, and abstract accessors behave exactly like virtual, sealed, override and abstract methods.</span></span> <span data-ttu-id="56f2d-1261">Konkrétně pravidel popsaných v [virtuální metody](classes.md#virtual-methods), [přepište metody](classes.md#override-methods), [zapečetěné metody](classes.md#sealed-methods), a [abstraktní metody](classes.md#abstract-methods) použít jako přístupové objekty byly metody odpovídající formuláře.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1261">Specifically, the rules described in [Virtual methods](classes.md#virtual-methods), [Override methods](classes.md#override-methods), [Sealed methods](classes.md#sealed-methods), and [Abstract methods](classes.md#abstract-methods) apply as if accessors were methods of a corresponding form.</span></span> <span data-ttu-id="56f2d-1262">Odpovídá každé přístupový objekt pro metodu s jednou hodnotou parametru typu události, `void` návratový typ a stejné modifikátory jako obsahující události.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1262">Each accessor corresponds to a method with a single value parameter of the event type, a `void` return type, and the same modifiers as the containing event.</span></span>

## <a name="indexers"></a><span data-ttu-id="56f2d-1263">Indexery</span><span class="sxs-lookup"><span data-stu-id="56f2d-1263">Indexers</span></span>

<span data-ttu-id="56f2d-1264">***Indexer*** je člen, který umožňuje objektu indexovat stejným způsobem jako pole.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1264">An ***indexer*** is a member that enables an object to be indexed in the same way as an array.</span></span> <span data-ttu-id="56f2d-1265">Indexery jsou deklarovány pomocí *indexer_declaration*s:</span><span class="sxs-lookup"><span data-stu-id="56f2d-1265">Indexers are declared using *indexer_declaration*s:</span></span>

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

<span data-ttu-id="56f2d-1266">*Indexer_declaration* může zahrnovat sadu *atributy* ([atributy](attributes.md)) a platnou kombinaci čtyři přístupu modifikátory přístupu ([modifikátorů přístupu ](classes.md#access-modifiers)), `new` ([new – modifikátor](classes.md#the-new-modifier)), `virtual` ([virtuální metody](classes.md#virtual-methods)), `override` ([přepište metody](classes.md#override-methods) ), `sealed` ([Zapečetěné metody](classes.md#sealed-methods)), `abstract` ([abstraktní metody](classes.md#abstract-methods)), a `extern` ([externí metody](classes.md#external-methods)) modifikátory.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1266">An *indexer_declaration* may include a set of *attributes* ([Attributes](attributes.md)) and a valid combination of the four access modifiers ([Access modifiers](classes.md#access-modifiers)), the `new` ([The new modifier](classes.md#the-new-modifier)), `virtual` ([Virtual methods](classes.md#virtual-methods)), `override` ([Override methods](classes.md#override-methods)), `sealed` ([Sealed methods](classes.md#sealed-methods)), `abstract` ([Abstract methods](classes.md#abstract-methods)), and `extern` ([External methods](classes.md#external-methods)) modifiers.</span></span>

<span data-ttu-id="56f2d-1267">Indexer deklarace se vztahují stejná pravidla jako deklarace metody ([metody](classes.md#methods)) s ohledem na platné kombinace modifikátorů s jednou výjimkou, která se statickém modifikátoru není povolena v deklaraci indexeru.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1267">Indexer declarations are subject to the same rules as method declarations ([Methods](classes.md#methods)) with regard to valid combinations of modifiers, with the one exception being that the static modifier is not permitted on an indexer declaration.</span></span>

<span data-ttu-id="56f2d-1268">Modifikátory `virtual`, `override`, a `abstract` se navzájem vylučují s výjimkou v jednom případě.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1268">The modifiers `virtual`, `override`, and `abstract` are mutually exclusive except in one case.</span></span> <span data-ttu-id="56f2d-1269">`abstract` a `override` modifikátory lze použít společně tak, aby abstraktní indexeru můžete přepsat virtuální jeden.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1269">The `abstract` and `override` modifiers may be used together so that an abstract indexer can override a virtual one.</span></span>

<span data-ttu-id="56f2d-1270">*Typ* deklarace Určuje typ elementu indexeru zavedeným deklarací z indexeru.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1270">The *type* of an indexer declaration specifies the element type of the indexer introduced by the declaration.</span></span> <span data-ttu-id="56f2d-1271">Indexer se nejedná explicitní implementace členu rozhraní, *typ* postupuje podle klíčového slova `this`.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1271">Unless the indexer is an explicit interface member implementation, the *type* is followed by the keyword `this`.</span></span> <span data-ttu-id="56f2d-1272">Člen implementace explicitního rozhraní naleznete *typ* následuje *interface_type*, "`.`" a klíčové slovo `this`.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1272">For an explicit interface member implementation, the *type* is followed by an *interface_type*, a "`.`", and the keyword `this`.</span></span> <span data-ttu-id="56f2d-1273">Na rozdíl od jiných členů nemají indexery uživatelského jména.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1273">Unlike other members, indexers do not have user-defined names.</span></span>

<span data-ttu-id="56f2d-1274">*Formal_parameter_list* Určuje parametry indexeru.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1274">The *formal_parameter_list* specifies the parameters of the indexer.</span></span> <span data-ttu-id="56f2d-1275">Seznam formálních parametrů indexer odpovídá signatuře metody ([parametry metody](classes.md#method-parameters)), s tím rozdílem, že musí být zadán minimálně jeden parametr a že `ref` a `out` nejsou povolené modifikátory parametrů .</span><span class="sxs-lookup"><span data-stu-id="56f2d-1275">The formal parameter list of an indexer corresponds to that of a method ([Method parameters](classes.md#method-parameters)), except that at least one parameter must be specified, and that the `ref` and `out` parameter modifiers are not permitted.</span></span>

<span data-ttu-id="56f2d-1276">*Typ* indexeru a každý z typů odkazuje *formal_parameter_list* musí být přinejmenším stejně dostupná jako samotný indexeru ([usnadnění omezení](basic-concepts.md#accessibility-constraints)).</span><span class="sxs-lookup"><span data-stu-id="56f2d-1276">The *type* of an indexer and each of the types referenced in the *formal_parameter_list* must be at least as accessible as the indexer itself ([Accessibility constraints](basic-concepts.md#accessibility-constraints)).</span></span>

<span data-ttu-id="56f2d-1277">*Indexer_body* může buď skládat ze ***přistupující objekt textu*** nebo ***tělo výrazu***.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1277">An *indexer_body* may either consist of an ***accessor body*** or an ***expression body***.</span></span> <span data-ttu-id="56f2d-1278">V textu přístupový objekt *accessor_declarations*, musí být uzavřen v "`{`"a"`}`" tokeny, deklarujte přístupových objektů ([přistupující objekty](classes.md#accessors)) vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1278">In an accessor body, *accessor_declarations*, which must be enclosed in "`{`" and "`}`" tokens, declare the accessors ([Accessors](classes.md#accessors)) of the property.</span></span> <span data-ttu-id="56f2d-1279">Přístupové objekty zadejte spustitelné příkazy, které jsou přidružené k čtení a zápisu vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1279">The accessors specify the executable statements associated with reading and writing the property.</span></span>

<span data-ttu-id="56f2d-1280">Tělo výrazu skládající se z "`=>`" následovaného výrazem `E` a středník se na základní text příkazu `{ get { return E; } }`a proto jde použít jenom k určení indexery jen pro funkci getter kde je výsledek getter Dal jeden výraz.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1280">An expression body consisting of "`=>`" followed by an expression `E` and a semicolon is exactly equivalent to the statement body `{ get { return E; } }`, and can therefore only be used to specify getter-only indexers where the result of the getter is given by a single expression.</span></span>

<span data-ttu-id="56f2d-1281">I když syntaxi pro přístup k elementu indexeru je stejné jako pro prvek pole, není indexer element klasifikovaná jako proměnnou.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1281">Even though the syntax for accessing an indexer element is the same as that for an array element, an indexer element is not classified as a variable.</span></span> <span data-ttu-id="56f2d-1282">Proto není možné předat jako element indexeru `ref` nebo `out` argument.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1282">Thus, it is not possible to pass an indexer element as a `ref` or `out` argument.</span></span>

<span data-ttu-id="56f2d-1283">Seznam formálních parametrů indexer definuje podpis ([podpisy a přetížení](basic-concepts.md#signatures-and-overloading)) indexeru.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1283">The formal parameter list of an indexer defines the signature ([Signatures and overloading](basic-concepts.md#signatures-and-overloading)) of the indexer.</span></span> <span data-ttu-id="56f2d-1284">Konkrétně podpis indexeru se skládá z počet a typy formálních parametrů.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1284">Specifically, the signature of an indexer consists of the number and types of its formal parameters.</span></span> <span data-ttu-id="56f2d-1285">Typ elementu a názvy formálních parametrů nejsou součástí indexer podpis.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1285">The element type and names of the formal parameters are not part of an indexer's signature.</span></span>

<span data-ttu-id="56f2d-1286">Podpis indexeru se musí lišit od podpisů ze všech indexerů deklarované ve stejné třídě.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1286">The signature of an indexer must differ from the signatures of all other indexers declared in the same class.</span></span>

<span data-ttu-id="56f2d-1287">Indexery a vlastnosti je v principu velmi podobné, ale liší následujícími způsoby:</span><span class="sxs-lookup"><span data-stu-id="56f2d-1287">Indexers and properties are very similar in concept, but differ in the following ways:</span></span>

*  <span data-ttu-id="56f2d-1288">Vlastnost je identifikován názvem, že indexer je identifikován jeho podpis.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1288">A property is identified by its name, whereas an indexer is identified by its signature.</span></span>
*  <span data-ttu-id="56f2d-1289">Vlastnost je přístupný prostřednictvím *simple_name* ([jednoduché názvy](expressions.md#simple-names)) nebo *member_access* ([přístup ke členu](expressions.md#member-access)), že indexer Element je přístupný prostřednictvím *element_access* ([přístup indexeru](expressions.md#indexer-access)).</span><span class="sxs-lookup"><span data-stu-id="56f2d-1289">A property is accessed through a *simple_name* ([Simple names](expressions.md#simple-names)) or a *member_access* ([Member access](expressions.md#member-access)), whereas an indexer element is accessed through an *element_access* ([Indexer access](expressions.md#indexer-access)).</span></span>
*  <span data-ttu-id="56f2d-1290">Vlastnost může být `static` člen, že indexer je vždy člena instance.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1290">A property can be a `static` member, whereas an indexer is always an instance member.</span></span>
*  <span data-ttu-id="56f2d-1291">A `get` přistupující objekt vlastnosti odpovídající metody bez vstupních parametrů, že `get` přistupující objekt indexer odpovídá metodu s stejného seznamu formálních parametrů jako indexeru.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1291">A `get` accessor of a property corresponds to a method with no parameters, whereas a `get` accessor of an indexer corresponds to a method with the same formal parameter list as the indexer.</span></span>
*  <span data-ttu-id="56f2d-1292">A `set` odpovídá přistupující objekt vlastnosti na metodu s jedním parametrem s názvem `value`, že `set` přistupující objekt indexer odpovídá metodu s stejného seznamu formálních parametrů jako indexeru a navíc další parametr s názvem `value`.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1292">A `set` accessor of a property corresponds to a method with a single parameter named `value`, whereas a `set` accessor of an indexer corresponds to a method with the same formal parameter list as the indexer, plus an additional parameter named `value`.</span></span>
*  <span data-ttu-id="56f2d-1293">Je chyba kompilace pro indexer přístupový objekt pro deklarování místní proměnné se stejným názvem jako parametr indexeru.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1293">It is a compile-time error for an indexer accessor to declare a local variable with the same name as an indexer parameter.</span></span>
*  <span data-ttu-id="56f2d-1294">V deklaraci vlastnosti přepsání zděděné vlastnosti přistupuje pomocí syntaxe `base.P`, kde `P` je název vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1294">In an overriding property declaration, the inherited property is accessed using the syntax `base.P`, where `P` is the property name.</span></span> <span data-ttu-id="56f2d-1295">V deklaraci indexer přepsání zděděné indexer přistupuje pomocí syntaxe `base[E]`, kde `E` je seznam výrazů oddělených čárkami.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1295">In an overriding indexer declaration, the inherited indexer is accessed using the syntax `base[E]`, where `E` is a comma separated list of expressions.</span></span>
*  <span data-ttu-id="56f2d-1296">Neexistuje koncept "automaticky implementované indexer".</span><span class="sxs-lookup"><span data-stu-id="56f2d-1296">There is no concept of an "automatically implemented indexer".</span></span> <span data-ttu-id="56f2d-1297">Jedná se o chybu, aby neabstraktní, neexterní indexer s průvodcem středník.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1297">It is an error to have a non-abstract, non-external indexer with semicolon accessors.</span></span>

<span data-ttu-id="56f2d-1298">Kromě těchto rozdílů všechna pravidla definovaná v [přistupující objekty](classes.md#accessors) a [automaticky implementované vlastnosti](classes.md#automatically-implemented-properties) platí pro indexer přistupující objekty také jako přístupové objekty vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1298">Aside from these differences, all rules defined in [Accessors](classes.md#accessors) and [Automatically implemented properties](classes.md#automatically-implemented-properties) apply to indexer accessors as well as to property accessors.</span></span>

<span data-ttu-id="56f2d-1299">Pokud obsahuje deklaraci indexer `extern` modifikátor, indexeru se říká, že ***externí indexer***.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1299">When an indexer declaration includes an `extern` modifier, the indexer is said to be an ***external indexer***.</span></span> <span data-ttu-id="56f2d-1300">Protože deklaraci externí indexer poskytuje skutečné implementaci, každá z jeho *accessor_declarations* skládá se ze středníku.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1300">Because an external indexer declaration provides no actual implementation, each of its *accessor_declarations* consists of a semicolon.</span></span>

<span data-ttu-id="56f2d-1301">Následující příklad deklaruje `BitArray` třídu, která implementuje indexeru pro přístup k jednotlivým bitů v bitové pole.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1301">The example below declares a `BitArray` class that implements an indexer for accessing the individual bits in the bit array.</span></span>
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

<span data-ttu-id="56f2d-1302">Instance `BitArray` třídy spotřebovává podstatně menší paměti než odpovídající `bool[]` (protože každá hodnota bývalé zabírá místo pouze jeden bit odkazující na jeden bajt), ale umožňuje stejné operace jako `bool[]`.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1302">An instance of the `BitArray` class consumes substantially less memory than a corresponding `bool[]` (since each value of the former occupies only one bit instead of the latter's one byte), but it permits the same operations as a `bool[]`.</span></span>

<span data-ttu-id="56f2d-1303">Následující `CountPrimes` třídy používá `BitArray` a klasického "x" algoritmus vypočítat počet základen mezi 1 a daný maximální:</span><span class="sxs-lookup"><span data-stu-id="56f2d-1303">The following `CountPrimes` class uses a `BitArray` and the classical "sieve" algorithm to compute the number of primes between 1 and a given maximum:</span></span>
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

<span data-ttu-id="56f2d-1304">Všimněte si, že syntaxe pro přístup k prvků `BitArray` je přesně stejné jako v případě `bool[]`.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1304">Note that the syntax for accessing elements of the `BitArray` is precisely the same as for a `bool[]`.</span></span>

<span data-ttu-id="56f2d-1305">Následující příklad ukazuje třídu 26 \* 10 mřížky, která má indexer se dvěma parametry.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1305">The following example shows a 26 \* 10 grid class that has an indexer with two parameters.</span></span> <span data-ttu-id="56f2d-1306">První parametr musí být velké nebo malé písmeno v rozsahu A – Z, a druhý musí být celé číslo v rozsahu 0 – 9.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1306">The first parameter is required to be an upper- or lowercase letter in the range A-Z, and the second is required to be an integer in the range 0-9.</span></span>

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

### <a name="indexer-overloading"></a><span data-ttu-id="56f2d-1307">Indexer přetížení</span><span class="sxs-lookup"><span data-stu-id="56f2d-1307">Indexer overloading</span></span>

<span data-ttu-id="56f2d-1308">Pravidla pro řešení přetížení indexeru jsou popsány v [odvození typu](expressions.md#type-inference).</span><span class="sxs-lookup"><span data-stu-id="56f2d-1308">The indexer overload resolution rules are described in [Type inference](expressions.md#type-inference).</span></span>

## <a name="operators"></a><span data-ttu-id="56f2d-1309">Operátory</span><span class="sxs-lookup"><span data-stu-id="56f2d-1309">Operators</span></span>

<span data-ttu-id="56f2d-1310">***Operátor*** je člen, který definuje význam výrazu operátor, který lze použít u instance třídy.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1310">An ***operator*** is a member that defines the meaning of an expression operator that can be applied to instances of the class.</span></span> <span data-ttu-id="56f2d-1311">Operátory jsou deklarovány pomocí *operator_declaration*s:</span><span class="sxs-lookup"><span data-stu-id="56f2d-1311">Operators are declared using *operator_declaration*s:</span></span>

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
    | 'right_shift' | '=='  | '!='  | '>'   | '<'   | '>='  | '<='
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

<span data-ttu-id="56f2d-1312">Existují tři kategorie přetížitelné operátory: unárních operátorů ([unárních operátorů](classes.md#unary-operators)), binární operátory ([binární operátory](classes.md#binary-operators)) a operátory převodu ([operátory převodu ](classes.md#conversion-operators)).</span><span class="sxs-lookup"><span data-stu-id="56f2d-1312">There are three categories of overloadable operators: Unary operators ([Unary operators](classes.md#unary-operators)), binary operators ([Binary operators](classes.md#binary-operators)), and conversion operators ([Conversion operators](classes.md#conversion-operators)).</span></span>

<span data-ttu-id="56f2d-1313">*Operator_body* je buď středník ***tělo s příkazy*** nebo ***tělo výrazu***.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1313">The *operator_body* is either a semicolon, a ***statement body*** or an ***expression body***.</span></span> <span data-ttu-id="56f2d-1314">Tělo s příkazy se skládá z *bloku*, která určuje příkazy ke spuštění při vyvolání operátor.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1314">A statement body consists of a *block*, which specifies the statements to execute when the operator is invoked.</span></span> <span data-ttu-id="56f2d-1315">*Bloku* musí splňovat pravidla pro vrácení hodnoty metod popsaných v [tělo metody](classes.md#method-body).</span><span class="sxs-lookup"><span data-stu-id="56f2d-1315">The *block* must conform to the rules for value-returning methods described in [Method body](classes.md#method-body).</span></span> <span data-ttu-id="56f2d-1316">Tělo výrazu se skládá z `=>` následovat výraz a středník a označuje jeden výraz k provádění při vyvolání operátor.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1316">An expression body consists of `=>` followed by an expression and a semicolon, and denotes a single expression to perform when the operator is invoked.</span></span>

<span data-ttu-id="56f2d-1317">Pro `extern` operátory, *operator_body* se skládá pouze ze středníku.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1317">For `extern` operators, the *operator_body* consists simply of a semicolon.</span></span> <span data-ttu-id="56f2d-1318">Pro všechny ostatní operátory *operator_body* je blok textu nebo tělo výrazu.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1318">For all other operators, the *operator_body* is either a block body or an expression body.</span></span>

<span data-ttu-id="56f2d-1319">Následující pravidla platí pro všechny deklarace operátoru:</span><span class="sxs-lookup"><span data-stu-id="56f2d-1319">The following rules apply to all operator declarations:</span></span>

*  <span data-ttu-id="56f2d-1320">Musí obsahovat deklarace operátoru `public` a `static` modifikátor.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1320">An operator declaration must include both a `public` and a `static` modifier.</span></span>
*  <span data-ttu-id="56f2d-1321">Parametry operátoru musí být parametry s hodnotou ([hodnoty parametrů](variables.md#value-parameters)).</span><span class="sxs-lookup"><span data-stu-id="56f2d-1321">The parameter(s) of an operator must be value parameters ([Value parameters](variables.md#value-parameters)).</span></span> <span data-ttu-id="56f2d-1322">Je chyba kompilace pro deklaraci operátor k určení `ref` nebo `out` parametry.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1322">It is a compile-time error for an operator declaration to specify `ref` or `out` parameters.</span></span>
*  <span data-ttu-id="56f2d-1323">Podpis operátora ([unárních operátorů](classes.md#unary-operators), [binární operátory](classes.md#binary-operators), [operátory převodu](classes.md#conversion-operators)) se musí lišit od podpisů všechny ostatní operátory, které jsou deklarované v stejné třídy.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1323">The signature of an operator ([Unary operators](classes.md#unary-operators), [Binary operators](classes.md#binary-operators), [Conversion operators](classes.md#conversion-operators)) must differ from the signatures of all other operators declared in the same class.</span></span>
*  <span data-ttu-id="56f2d-1324">Všechny typy odkazované v deklarace operátoru musí být přinejmenším stejně dostupná jako operátor ([usnadnění omezení](basic-concepts.md#accessibility-constraints)).</span><span class="sxs-lookup"><span data-stu-id="56f2d-1324">All types referenced in an operator declaration must be at least as accessible as the operator itself ([Accessibility constraints](basic-concepts.md#accessibility-constraints)).</span></span>
*  <span data-ttu-id="56f2d-1325">Jedná se o chybu pro stejný modifikátor objevit více než jednou v deklarace operátoru.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1325">It is an error for the same modifier to appear multiple times in an operator declaration.</span></span>

<span data-ttu-id="56f2d-1326">Každá kategorie operátorů ukládá další omezení, jak je popsáno v následujících částech.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1326">Each operator category imposes additional restrictions, as described in the following sections.</span></span>

<span data-ttu-id="56f2d-1327">Stejně jako ostatní členové v základní třídě je deklarovaný operátory jsou zděděny z odvozených tříd.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1327">Like other members, operators declared in a base class are inherited by derived classes.</span></span> <span data-ttu-id="56f2d-1328">Deklarace operátoru vždy vyžadují dané třídy nebo struktury, ve kterém je operátor deklarován k účasti v signatuře operátoru, a proto není možné operátor deklarovaná v odvozené třídě skrýt operátor v základní třídě je deklarovaný.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1328">Because operator declarations always require the class or struct in which the operator is declared to participate in the signature of the operator, it is not possible for an operator declared in a derived class to hide an operator declared in a base class.</span></span> <span data-ttu-id="56f2d-1329">Proto `new` modifikátor se nikdy vyžaduje a proto nikdy povoleno, v deklarace operátoru.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1329">Thus, the `new` modifier is never required, and therefore never permitted, in an operator declaration.</span></span>

<span data-ttu-id="56f2d-1330">Další informace o jednočlenné a binární operátory lze nalézt v [operátory](expressions.md#operators).</span><span class="sxs-lookup"><span data-stu-id="56f2d-1330">Additional information on unary and binary operators can be found in [Operators](expressions.md#operators).</span></span>

<span data-ttu-id="56f2d-1331">Další informace o operátory převodu lze nalézt v [uživatelem definované převody](conversions.md#user-defined-conversions).</span><span class="sxs-lookup"><span data-stu-id="56f2d-1331">Additional information on conversion operators can be found in [User-defined conversions](conversions.md#user-defined-conversions).</span></span>

### <a name="unary-operators"></a><span data-ttu-id="56f2d-1332">Unární operátory</span><span class="sxs-lookup"><span data-stu-id="56f2d-1332">Unary operators</span></span>

<span data-ttu-id="56f2d-1333">Následující pravidla platí pro unární operátor prohlášení, kde `T` označuje typ instance dané třídy nebo struktury, která obsahuje deklaraci operátor:</span><span class="sxs-lookup"><span data-stu-id="56f2d-1333">The following rules apply to unary operator declarations, where `T` denotes the instance type of the class or struct that contains the operator declaration:</span></span>

*  <span data-ttu-id="56f2d-1334">Unární operátor `+`, `-`, `!`, nebo `~` operátor musí vzít jeden parametr typu `T` nebo `T?` a může vrátit libovolného typu.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1334">A unary `+`, `-`, `!`, or `~` operator must take a single parameter of type `T` or `T?` and can return any type.</span></span>
*  <span data-ttu-id="56f2d-1335">Unární operátor `++` nebo `--` operátor musí vzít jeden parametr typu `T` nebo `T?` a musí vrátit, že stejný typ nebo typ odvozený z něj.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1335">A unary `++` or `--` operator must take a single parameter of type `T` or `T?` and must return that same type or a type derived from it.</span></span>
*  <span data-ttu-id="56f2d-1336">Unární operátor `true` nebo `false` operátor musí vzít jeden parametr typu `T` nebo `T?` a musí vrátit typ `bool`.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1336">A unary `true` or `false` operator must take a single parameter of type `T` or `T?` and must return type `bool`.</span></span>

<span data-ttu-id="56f2d-1337">Podpis unární operátor se skládá z tokenu – operátor (`+`, `-`, `!`, `~`, `++`, `--`, `true`, nebo `false`) a jeden formální parametr typu.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1337">The signature of a unary operator consists of the operator token (`+`, `-`, `!`, `~`, `++`, `--`, `true`, or `false`) and the type of the single formal parameter.</span></span> <span data-ttu-id="56f2d-1338">Návratový typ není součástí unární operátor podpis, ani není název formálních parametrů.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1338">The return type is not part of a unary operator's signature, nor is the name of the formal parameter.</span></span>

<span data-ttu-id="56f2d-1339">`true` a `false` unárních operátorů vyžadují pair-wise deklarace.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1339">The `true` and `false` unary operators require pair-wise declaration.</span></span> <span data-ttu-id="56f2d-1340">Chyba kompilace nastane, pokud jeden z těchto operátorů třída deklaruje bez také druhý deklarace.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1340">A compile-time error occurs if a class declares one of these operators without also declaring the other.</span></span> <span data-ttu-id="56f2d-1341">`true` a `false` operátory jsou popsány dále v [podmíněné logické operátory definované uživatelem](expressions.md#user-defined-conditional-logical-operators) a [logické výrazy](expressions.md#boolean-expressions).</span><span class="sxs-lookup"><span data-stu-id="56f2d-1341">The `true` and `false` operators are described further in [User-defined conditional logical operators](expressions.md#user-defined-conditional-logical-operators) and [Boolean expressions](expressions.md#boolean-expressions).</span></span>

<span data-ttu-id="56f2d-1342">Následující příklad ukazuje implementaci a následné použití `operator ++` pro třídu vector celé číslo:</span><span class="sxs-lookup"><span data-stu-id="56f2d-1342">The following example shows an implementation and subsequent usage of `operator ++` for an integer vector class:</span></span>
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

<span data-ttu-id="56f2d-1343">Všimněte si, jak metoda – operátor vrátí hodnotu vytvořený tak, že přidáte 1 s operandem, stejně jako zvýšení přípony a operátory snížení ([Příponové operátory Inkrementace a dekrementace operátory](expressions.md#postfix-increment-and-decrement-operators)) a předponu Inkrementace a dekrementace operátory ([předpony Inkrementace a dekrementace operátory](expressions.md#prefix-increment-and-decrement-operators)).</span><span class="sxs-lookup"><span data-stu-id="56f2d-1343">Note how the operator method returns the value produced by adding 1 to the operand, just like the  postfix increment and decrement operators ([Postfix increment and decrement operators](expressions.md#postfix-increment-and-decrement-operators)), and the prefix increment and decrement operators ([Prefix increment and decrement operators](expressions.md#prefix-increment-and-decrement-operators)).</span></span> <span data-ttu-id="56f2d-1344">Na rozdíl od v jazyce C++, tato metoda nemusí upravovat hodnota jeho operandu přímo.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1344">Unlike in C++, this method need not modify the value of its operand directly.</span></span> <span data-ttu-id="56f2d-1345">Ve skutečnosti Úprava hodnoty operandu by mohla narušit standardní sémantiku příponového operátoru Inkrementace.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1345">In fact, modifying the operand value would violate the standard semantics of the postfix increment operator.</span></span>

### <a name="binary-operators"></a><span data-ttu-id="56f2d-1346">Binární operátory</span><span class="sxs-lookup"><span data-stu-id="56f2d-1346">Binary operators</span></span>

<span data-ttu-id="56f2d-1347">Následující pravidla platí pro binární operátor prohlášení, kde `T` označuje typ instance dané třídy nebo struktury, která obsahuje deklaraci operátor:</span><span class="sxs-lookup"><span data-stu-id="56f2d-1347">The following rules apply to binary operator declarations, where `T` denotes the instance type of the class or struct that contains the operator declaration:</span></span>

*  <span data-ttu-id="56f2d-1348">Binární operátor bez posunutí musí přebírají dva parametry, nejméně jeden z nich musí mít typ `T` nebo `T?`a může vrátit libovolného typu.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1348">A binary non-shift operator must take two parameters, at least one of which must have type `T` or `T?`, and can return any type.</span></span>
*  <span data-ttu-id="56f2d-1349">Binární soubor `<<` nebo `>>` operátor musí mít dva parametry, první z nich musí mít typ `T` nebo `T?` a druhý z nich musí mít typ `int` nebo `int?`a může vrátit libovolného typu.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1349">A binary `<<` or `>>` operator must take two parameters, the first of which must have type `T` or `T?` and the second of which must have type `int` or `int?`, and can return any type.</span></span>

<span data-ttu-id="56f2d-1350">Podpis binárního operátoru se skládá z tokenu – operátor (`+`, `-`, `*`, `/`, `%`, `&`, `|`, `^`, `<<`, `>>`, `==`, `!=`, `>`, `<`, `>=`, nebo `<=`) a typy dva formální parametry.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1350">The signature of a binary operator consists of the operator token (`+`, `-`, `*`, `/`, `%`, `&`, `|`, `^`, `<<`, `>>`, `==`, `!=`, `>`, `<`, `>=`, or `<=`) and the types of the two formal parameters.</span></span> <span data-ttu-id="56f2d-1351">Názvy formálních parametrů a návratový typ nejsou součástí podpis binárního operátoru.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1351">The return type and the names of the formal parameters are not part of a binary operator's signature.</span></span>

<span data-ttu-id="56f2d-1352">Některé binární operátory vyžadují pair-wise deklarace.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1352">Certain binary operators require pair-wise declaration.</span></span> <span data-ttu-id="56f2d-1353">Pro každou deklaraci buď operátor dvojice musí být odpovídající deklarace operátoru které odpovídá páru licencí.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1353">For every declaration of either operator of a pair, there must be a matching declaration of the other operator of the pair.</span></span> <span data-ttu-id="56f2d-1354">Dvě deklarace operátoru případy, kdy se mají vracet hodnotu stejného typu a stejného typu pro každý parametr.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1354">Two operator declarations match when they have the same return type and the same type for each parameter.</span></span> <span data-ttu-id="56f2d-1355">Tyto operátory vyžadují pair-wise deklarace:</span><span class="sxs-lookup"><span data-stu-id="56f2d-1355">The following operators require pair-wise declaration:</span></span>

*  <span data-ttu-id="56f2d-1356">`operator ==` a `operator !=`</span><span class="sxs-lookup"><span data-stu-id="56f2d-1356">`operator ==` and `operator !=`</span></span>
*  <span data-ttu-id="56f2d-1357">`operator >` a `operator <`</span><span class="sxs-lookup"><span data-stu-id="56f2d-1357">`operator >` and `operator <`</span></span>
*  <span data-ttu-id="56f2d-1358">`operator >=` a `operator <=`</span><span class="sxs-lookup"><span data-stu-id="56f2d-1358">`operator >=` and `operator <=`</span></span>

### <a name="conversion-operators"></a><span data-ttu-id="56f2d-1359">operátory převodu</span><span class="sxs-lookup"><span data-stu-id="56f2d-1359">Conversion operators</span></span>

<span data-ttu-id="56f2d-1360">Zavádí deklaraci operátor převodu ***uživatelsky definovaný převod*** ([uživatelem definované převody](conversions.md#user-defined-conversions)) která posiluje předem definovaných implicitní a explicitní převody.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1360">A conversion operator declaration introduces a ***user-defined conversion*** ([User-defined conversions](conversions.md#user-defined-conversions)) which augments the pre-defined implicit and explicit conversions.</span></span>

<span data-ttu-id="56f2d-1361">Deklarace operátor převodu, která zahrnuje `implicit` – klíčové slovo představuje uživatelem definovaný implicitní převod.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1361">A conversion operator declaration that includes the `implicit` keyword introduces a user-defined implicit conversion.</span></span> <span data-ttu-id="56f2d-1362">Implicitní převod může dojít v různých situacích, včetně volání členské funkce, výrazy přetypování a přiřazení.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1362">Implicit conversions can occur in a variety of situations, including function member invocations, cast expressions, and assignments.</span></span> <span data-ttu-id="56f2d-1363">Toto je popsáno dále v [implicitních převodů](conversions.md#implicit-conversions).</span><span class="sxs-lookup"><span data-stu-id="56f2d-1363">This is described further in [Implicit conversions](conversions.md#implicit-conversions).</span></span>

<span data-ttu-id="56f2d-1364">Deklarace operátor převodu, která zahrnuje `explicit` – klíčové slovo představuje uživatelem definovaný explicitní převod.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1364">A conversion operator declaration that includes the `explicit` keyword introduces a user-defined explicit conversion.</span></span> <span data-ttu-id="56f2d-1365">Může dojít v výrazy přetypování explicitních převodů a jsou popsány dále v [explicitních převodů](conversions.md#explicit-conversions).</span><span class="sxs-lookup"><span data-stu-id="56f2d-1365">Explicit conversions can occur in cast expressions, and are described further in [Explicit conversions](conversions.md#explicit-conversions).</span></span>

<span data-ttu-id="56f2d-1366">Operátor převodu převede od zdrojového typu, který je označen typ parametru operátorů převodu s cílovým typem indikován návratového typu operátoru převodu.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1366">A conversion operator converts from a source type, indicated by the parameter type of the conversion operator, to a target type, indicated by the return type of the conversion operator.</span></span>

<span data-ttu-id="56f2d-1367">Pro typ daného zdroje `S` a cílový typ `T`, pokud `S` nebo `T` jsou typy s možnou hodnotou Null, umožní `S0` a `T0` odkazovat na základní typy, jinak `S0` a `T0` jsou rovno `S` a `T` v uvedeném pořadí.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1367">For a given source type `S` and target type `T`, if `S` or `T` are nullable types, let `S0` and `T0` refer to their underlying types, otherwise `S0` and `T0` are equal to `S` and `T` respectively.</span></span> <span data-ttu-id="56f2d-1368">Třídy nebo struktury je povolené pro deklaraci převod z typu zdrojové `S` s cílovým typem `T` pouze v případě, že jsou splněny všechny z následujících akcí:</span><span class="sxs-lookup"><span data-stu-id="56f2d-1368">A class or struct is permitted to declare a conversion from a source type `S` to a target type `T` only if all of the following are true:</span></span>

*  <span data-ttu-id="56f2d-1369">`S0` a `T0` jsou různé typy.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1369">`S0` and `T0` are different types.</span></span>
*  <span data-ttu-id="56f2d-1370">Buď `S0` nebo `T0` je typ třídy nebo struktury, ve kterém probíhá deklarace operátoru.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1370">Either `S0` or `T0` is the class or struct type in which the operator declaration takes place.</span></span>
*  <span data-ttu-id="56f2d-1371">Ani `S0` ani `T0` je *interface_type*.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1371">Neither `S0` nor `T0` is an *interface_type*.</span></span>
*  <span data-ttu-id="56f2d-1372">S výjimkou uživatelem definované převody neexistuje převod z `S` k `T` nebo z `T` k `S`.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1372">Excluding user-defined conversions, a conversion does not exist from `S` to `T` or from `T` to `S`.</span></span>

<span data-ttu-id="56f2d-1373">Pro účely těchto pravidel se žádné parametry přidružené k typu `S` nebo `T` jsou považovány za jedinečné typy, které mají vztah dědičnosti s jinými typy a jakákoliv omezení u těchto typů parametry budou ignorovány.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1373">For the purposes of these rules, any type parameters associated with `S` or `T` are considered to be unique types that have no inheritance relationship with other types, and any constraints on those type parameters are ignored.</span></span>

<span data-ttu-id="56f2d-1374">V příkladu</span><span class="sxs-lookup"><span data-stu-id="56f2d-1374">In the example</span></span>
```csharp
class C<T> {...}

class D<T>: C<T>
{
    public static implicit operator C<int>(D<T> value) {...}      // Ok
    public static implicit operator C<string>(D<T> value) {...}   // Ok
    public static implicit operator C<T>(D<T> value) {...}        // Error
}
```
<span data-ttu-id="56f2d-1375">první dva operátor deklarace jsou povolené, protože pro účely [indexery](classes.md#indexers).3, `T` a `int` a `string` v uvedeném pořadí jsou považovány za jedinečné typy se žádné relace.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1375">the first two operator declarations are permitted because, for the purposes of [Indexers](classes.md#indexers).3, `T` and `int` and `string` respectively are considered unique types with no relationship.</span></span> <span data-ttu-id="56f2d-1376">Ale třetí operátor se o chybu, protože `C<T>` je základní třída `D<T>`.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1376">However, the third operator is an error because `C<T>` is the base class of `D<T>`.</span></span>

<span data-ttu-id="56f2d-1377">Z druhé pravidlo, že vyplývá, že operátor převodu musíte převést na nebo z typu třídy nebo struktury, ve kterém je operátor deklarován.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1377">From the second rule it follows that a conversion operator must convert either to or from the class or struct type in which the operator is declared.</span></span> <span data-ttu-id="56f2d-1378">Například je možné pro typ třídy nebo struktury `C` definovat převod z `C` k `int` a z `int` k `C`, ale ne z `int` k `bool`.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1378">For example, it is possible for a class or struct type `C` to define a conversion from `C` to `int` and from `int` to `C`, but not from `int` to `bool`.</span></span>

<span data-ttu-id="56f2d-1379">Není možné přímo předefinovat předem definovaný převod.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1379">It is not possible to directly redefine a pre-defined conversion.</span></span> <span data-ttu-id="56f2d-1380">Proto nejsou povoleny operátory převodu k převedení z nebo do `object` protože implicitní a explicitní převody již neexistuje mezi `object` a všechny ostatní typy.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1380">Thus, conversion operators are not allowed to convert from or to `object` because implicit and explicit conversions already exist between `object` and all other types.</span></span> <span data-ttu-id="56f2d-1381">Podobně může být zdrojové ani cílové typy převodu základního typu, protože převod by pak již existuje.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1381">Likewise, neither the source nor the target types of a conversion can be a base type of the other, since a conversion would then already exist.</span></span>

<span data-ttu-id="56f2d-1382">Je však možné deklarovat operátory v obecných typech, které pro určitý typ argumentů, zadejte převody, které již existují jako předem definované převody.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1382">However, it is possible to declare operators on generic types that, for particular type arguments, specify conversions that already exist as pre-defined conversions.</span></span> <span data-ttu-id="56f2d-1383">V příkladu</span><span class="sxs-lookup"><span data-stu-id="56f2d-1383">In the example</span></span>
```csharp
struct Convertible<T>
{
    public static implicit operator Convertible<T>(T value) {...}
    public static explicit operator T(Convertible<T> value) {...}
}
```
<span data-ttu-id="56f2d-1384">Když zadejte `object` je zadaný jako argument typu pro `T`, druhý operátor deklaruje převod, který již existuje (implicitní a proto také explicitně, existuje převod z libovolný typ na typ `object`).</span><span class="sxs-lookup"><span data-stu-id="56f2d-1384">when type `object` is specified as a type argument for `T`, the second operator declares a conversion that already exists (an implicit, and therefore also an explicit, conversion exists from any type to type `object`).</span></span>

<span data-ttu-id="56f2d-1385">V případech, kde existuje předem definovaný převod mezi dvěma typy jsou ignorovány všechny uživatelem definované převody mezi typy.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1385">In cases where a pre-defined conversion exists between two types, any user-defined conversions between those types are ignored.</span></span> <span data-ttu-id="56f2d-1386">Konkrétně:</span><span class="sxs-lookup"><span data-stu-id="56f2d-1386">Specifically:</span></span>

*  <span data-ttu-id="56f2d-1387">Pokud předdefinované implicitní převod ([implicitních převodů](conversions.md#implicit-conversions)) z typu `S` na typ `T`, všechny uživatelem definované převody (implicitní nebo explicitní) z `S` k `T` jsou ignorovány.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1387">If a pre-defined implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)) exists from type `S` to type `T`, all user-defined conversions (implicit or explicit) from `S` to `T` are ignored.</span></span>
*  <span data-ttu-id="56f2d-1388">Pokud předem definované explicitní převod ([explicitních převodů](conversions.md#explicit-conversions)) z typu `S` na typ `T`, žádné uživatelem definované explicitní převody z `S` k `T` jsou ignorovány.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1388">If a pre-defined explicit conversion ([Explicit conversions](conversions.md#explicit-conversions)) exists from type `S` to type `T`, any user-defined explicit conversions from `S` to `T` are ignored.</span></span> <span data-ttu-id="56f2d-1389">Dále:</span><span class="sxs-lookup"><span data-stu-id="56f2d-1389">Furthermore:</span></span>

<span data-ttu-id="56f2d-1390">Pokud `T` je typu rozhraní, uživatelem definované implicitní převody z `S` k `T` jsou ignorovány.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1390">If `T` is an interface type, user-defined implicit conversions from `S` to `T` are ignored.</span></span>

<span data-ttu-id="56f2d-1391">Jinak, uživatelem definované implicitní převody z `S` k `T` jsou stále považovány za.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1391">Otherwise, user-defined implicit conversions from `S` to `T` are still considered.</span></span>

<span data-ttu-id="56f2d-1392">Pro všechny typy, ale `object`, operátory deklaroval `Convertible<T>` výše uvedených typů nejsou v rozporu s předem definované převody.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1392">For all types but `object`, the operators declared by the `Convertible<T>` type above do not conflict with pre-defined conversions.</span></span> <span data-ttu-id="56f2d-1393">Příklad:</span><span class="sxs-lookup"><span data-stu-id="56f2d-1393">For example:</span></span>
```csharp
void F(int i, Convertible<int> n) {
    i = n;                          // Error
    i = (int)n;                     // User-defined explicit conversion
    n = i;                          // User-defined implicit conversion
    n = (Convertible<int>)i;        // User-defined implicit conversion
}
```

<span data-ttu-id="56f2d-1394">Ale pro typ `object`, uživatelem definované převody ve všech případech ale jeden skrýt předem definované převody:</span><span class="sxs-lookup"><span data-stu-id="56f2d-1394">However, for type `object`, pre-defined conversions hide the user-defined conversions in all cases but one:</span></span>

```csharp
void F(object o, Convertible<object> n) {
    o = n;                         // Pre-defined boxing conversion
    o = (object)n;                 // Pre-defined boxing conversion
    n = o;                         // User-defined implicit conversion
    n = (Convertible<object>)o;    // Pre-defined unboxing conversion
}
```

<span data-ttu-id="56f2d-1395">Uživatelem definované převody nejsou povoleny pro převod z nebo do *interface_type*s.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1395">User-defined conversions are not allowed to convert from or to *interface_type*s.</span></span> <span data-ttu-id="56f2d-1396">Konkrétně se toto omezení zajišťuje, že žádné uživatelem definované dochází při převodu na *interface_type*a že převod na *interface_type* proběhne úspěšně pouze pokud objekt Převádí se ve skutečnosti implementuje zadaný *interface_type*.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1396">In particular, this restriction ensures that no user-defined transformations occur when converting to an *interface_type*, and that a conversion to an *interface_type* succeeds only if the object being converted actually implements the specified *interface_type*.</span></span>

<span data-ttu-id="56f2d-1397">Podpis operátora převodu se skládá z zdrojového a cílového typu.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1397">The signature of a conversion operator consists of the source type and the target type.</span></span> <span data-ttu-id="56f2d-1398">(Všimněte si, že se jedná o jedinou formou člena, u kterého návratový typ se účastní signatura.) `implicit` Nebo `explicit` klasifikace operátor převodu není součást podpisu obsluhy.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1398">(Note that this is the only form of member for which the return type participates in the signature.) The `implicit` or `explicit` classification of a conversion operator is not part of the operator's signature.</span></span> <span data-ttu-id="56f2d-1399">Proto třídy nebo struktury nelze deklarovat obě `implicit` a `explicit` operátor převodu se stejnými typy zdroj a cíl.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1399">Thus, a class or struct cannot declare both an `implicit` and an `explicit` conversion operator with the same source and target types.</span></span>

<span data-ttu-id="56f2d-1400">Obecně platí uživatelem definované implicitní převody by se měly navrhovat nikdy nevyvolají výjimky a nikdy dojít ke ztrátě informací.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1400">In general, user-defined implicit conversions should be designed to never throw exceptions and never lose information.</span></span> <span data-ttu-id="56f2d-1401">Je-li uživatelem definovaný převod může mít za následek výjimky (například, protože zdroj argument je mimo rozsah) nebo ke ztrátě informací (například se zahodí implikovaný bits) a pak tento převod musí být definován jako explicitní převod.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1401">If a user-defined conversion can give rise to exceptions (for example, because the source argument is out of range) or loss of information (such as discarding high-order bits), then that conversion should be defined as an explicit conversion.</span></span>

<span data-ttu-id="56f2d-1402">V příkladu</span><span class="sxs-lookup"><span data-stu-id="56f2d-1402">In the example</span></span>
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
<span data-ttu-id="56f2d-1403">Převod z `Digit` k `byte` je implicitní, protože nikdy vyvolá výjimky nebo ztratí informace, ale převod z `byte` k `Digit` je explicitní od `Digit` mohou představovat jenom podmnožinu možné hodnoty `byte`.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1403">the conversion from `Digit` to `byte` is implicit because it never throws exceptions or loses information, but the conversion from `byte` to `Digit` is explicit since `Digit` can only represent a subset of the possible values of a `byte`.</span></span>

## <a name="instance-constructors"></a><span data-ttu-id="56f2d-1404">Konstruktory instancí</span><span class="sxs-lookup"><span data-stu-id="56f2d-1404">Instance constructors</span></span>

<span data-ttu-id="56f2d-1405">***Konstruktor instance*** je člen, který implementuje akce potřebné k inicializaci instance třídy.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1405">An ***instance constructor*** is a member that implements the actions required to initialize an instance of a class.</span></span> <span data-ttu-id="56f2d-1406">Konstruktory instancí jsou deklarovány pomocí *constructor_declaration*s:</span><span class="sxs-lookup"><span data-stu-id="56f2d-1406">Instance constructors are declared using *constructor_declaration*s:</span></span>

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

<span data-ttu-id="56f2d-1407">A *constructor_declaration* může zahrnovat sadu *atributy* ([atributy](attributes.md)), platnou kombinaci čtyři přístupu modifikátory přístupu ([modifikátorů přístupu ](classes.md#access-modifiers)) a `extern` ([externí metody](classes.md#external-methods)) modifikátor.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1407">A *constructor_declaration* may include a set of *attributes* ([Attributes](attributes.md)), a valid combination of the four access modifiers ([Access modifiers](classes.md#access-modifiers)), and an `extern` ([External methods](classes.md#external-methods)) modifier.</span></span> <span data-ttu-id="56f2d-1408">Deklarace konstruktoru se nepovoluje patří stejné modifikátor více než jednou.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1408">A constructor declaration is not permitted to include the same modifier multiple times.</span></span>

<span data-ttu-id="56f2d-1409">*Identifikátor* z *constructor_declarator* název musí být třída, ve kterém je deklarována konstruktor instance.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1409">The *identifier* of a *constructor_declarator* must name the class in which the instance constructor is declared.</span></span> <span data-ttu-id="56f2d-1410">Pokud je zadán žádným jiným názvem, dojde k chybě kompilace.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1410">If any other name is specified, a compile-time error occurs.</span></span>

<span data-ttu-id="56f2d-1411">Volitelný *formal_parameter_list* instance konstruktor je v souladu s stejná pravidla jako *formal_parameter_list* metody ([metody](classes.md#methods)).</span><span class="sxs-lookup"><span data-stu-id="56f2d-1411">The optional *formal_parameter_list* of an instance constructor is subject to the same rules as the *formal_parameter_list* of a method ([Methods](classes.md#methods)).</span></span> <span data-ttu-id="56f2d-1412">Seznam formálních parametrů definuje podpis ([podpisy a přetížení](basic-concepts.md#signatures-and-overloading)) z konstruktoru instance a kterým se řídí proces rozlišení přetěžování ([odvození typu](expressions.md#type-inference)) vybere konkrétní konstruktor instance vyvolání.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1412">The formal parameter list defines the signature ([Signatures and overloading](basic-concepts.md#signatures-and-overloading)) of an instance constructor and governs the process whereby overload resolution ([Type inference](expressions.md#type-inference)) selects a particular instance constructor in an invocation.</span></span>

<span data-ttu-id="56f2d-1413">Každý z typů odkazuje *formal_parameter_list* instance konstruktor musí být přinejmenším stejně dostupná jako konstruktor samotný ([usnadnění omezení](basic-concepts.md#accessibility-constraints)).</span><span class="sxs-lookup"><span data-stu-id="56f2d-1413">Each of the types referenced in the *formal_parameter_list* of an instance constructor must be at least as accessible as the constructor itself ([Accessibility constraints](basic-concepts.md#accessibility-constraints)).</span></span>

<span data-ttu-id="56f2d-1414">Volitelný *constructor_initializer* určuje jiný konstruktor instance, který má být vyvolán před spuštěním příkazů uvedených v *constructor_body* tento konstruktor instance.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1414">The optional *constructor_initializer* specifies another instance constructor to invoke before executing the statements given in the *constructor_body* of this instance constructor.</span></span> <span data-ttu-id="56f2d-1415">Toto je popsáno dále v [inicializátory konstruktoru](classes.md#constructor-initializers).</span><span class="sxs-lookup"><span data-stu-id="56f2d-1415">This is described further in [Constructor initializers](classes.md#constructor-initializers).</span></span>

<span data-ttu-id="56f2d-1416">Pokud obsahuje deklaraci konstruktoru `extern` modifikátor, konstruktor se říká, že ***externí konstruktor***.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1416">When a constructor declaration includes an `extern` modifier, the constructor is said to be an ***external constructor***.</span></span> <span data-ttu-id="56f2d-1417">Protože deklaraci externí konstruktor poskytuje skutečné implementaci, jeho *constructor_body* skládá se ze středníku.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1417">Because an external constructor declaration provides no actual implementation, its *constructor_body* consists of a semicolon.</span></span> <span data-ttu-id="56f2d-1418">Pro všechny ostatní konstruktory *constructor_body* se skládá z *bloku* který určuje příkazy ke inicializuje novou instanci třídy.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1418">For all other constructors, the *constructor_body* consists of a *block* which specifies the statements to initialize a new instance of the class.</span></span> <span data-ttu-id="56f2d-1419">To přesně odpovídá *bloku* metody instance s `void` návratový typ ([tělo metody](classes.md#method-body)).</span><span class="sxs-lookup"><span data-stu-id="56f2d-1419">This corresponds exactly to the *block* of an instance method with a `void` return type ([Method body](classes.md#method-body)).</span></span>

<span data-ttu-id="56f2d-1420">Konstruktory instancí nedědí.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1420">Instance constructors are not inherited.</span></span> <span data-ttu-id="56f2d-1421">Proto třída nemá žádné konstruktory instancí než ty, které skutečně deklarovaná ve třídě.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1421">Thus, a class has no instance constructors other than those actually declared in the class.</span></span> <span data-ttu-id="56f2d-1422">Pokud třída obsahuje žádné deklarace konstruktoru instance, se automaticky poskytuje výchozí konstruktor instance ([výchozí konstruktory](classes.md#default-constructors)).</span><span class="sxs-lookup"><span data-stu-id="56f2d-1422">If a class contains no instance constructor declarations, a default instance constructor is automatically provided ([Default constructors](classes.md#default-constructors)).</span></span>

<span data-ttu-id="56f2d-1423">Jsou vyvolány konstruktory instancí *object_creation_expression*s ([výrazy vytvoření objektu](expressions.md#object-creation-expressions)) a přes *constructor_initializer*s.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1423">Instance constructors are invoked by *object_creation_expression*s ([Object creation expressions](expressions.md#object-creation-expressions)) and through *constructor_initializer*s.</span></span>

### <a name="constructor-initializers"></a><span data-ttu-id="56f2d-1424">Inicializátory konstruktoru</span><span class="sxs-lookup"><span data-stu-id="56f2d-1424">Constructor initializers</span></span>

<span data-ttu-id="56f2d-1425">Všechny konstruktory instancí (s výjimkou souborů pro třídu `object`) implicitně bezprostředně před patří volání jiného konstruktoru instance *constructor_body*.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1425">All instance constructors (except those for class `object`) implicitly include an invocation of another instance constructor immediately before the *constructor_body*.</span></span> <span data-ttu-id="56f2d-1426">Konstruktoru implicitně závisí *constructor_initializer*:</span><span class="sxs-lookup"><span data-stu-id="56f2d-1426">The constructor to implicitly invoke is determined by the *constructor_initializer*:</span></span>

*  <span data-ttu-id="56f2d-1427">Inicializátor konstruktoru instance formuláře `base(argument_list)` nebo `base()` způsobí, že od přímé základní třídy má být volána konstruktor instance.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1427">An instance constructor initializer of the form `base(argument_list)` or `base()` causes an instance constructor from the direct base class to be invoked.</span></span> <span data-ttu-id="56f2d-1428">Tento konstruktor je zaškrtnuto, pomocí *argument_list* -li k dispozici a pravidla rozlišení přetížení [rozlišení přetěžování](expressions.md#overload-resolution).</span><span class="sxs-lookup"><span data-stu-id="56f2d-1428">That constructor is selected using *argument_list* if present and the overload resolution rules of [Overload resolution](expressions.md#overload-resolution).</span></span> <span data-ttu-id="56f2d-1429">Sada konstruktory instancí Release candidate zahrnuje všechny dostupné instance konstruktory obsažené v přímé základní třídy nebo výchozího konstruktoru ([výchozí konstruktory](classes.md#default-constructors)), pokud jsou v deklarovány žádné konstruktory instancí přímé základní třídy.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1429">The set of candidate instance constructors consists of all accessible instance constructors contained in the direct base class, or the default constructor ([Default constructors](classes.md#default-constructors)), if no instance constructors are declared in the direct base class.</span></span> <span data-ttu-id="56f2d-1430">Pokud tato sada je prázdný nebo jediný konstruktor instance nejlepší nelze identifikovat, dojde k chybě kompilace.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1430">If this set is empty, or if a single best instance constructor cannot be identified, a compile-time error occurs.</span></span>
*  <span data-ttu-id="56f2d-1431">Inicializátor konstruktoru instance formuláře `this(argument-list)` nebo `this()` způsobí, že ze třídy samotný má být volána konstruktor instance.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1431">An instance constructor initializer of the form `this(argument-list)` or `this()` causes an instance constructor from the class itself to be invoked.</span></span> <span data-ttu-id="56f2d-1432">Konstruktor je zaškrtnuto, pomocí *argument_list* -li k dispozici a pravidla rozlišení přetížení [rozlišení přetěžování](expressions.md#overload-resolution).</span><span class="sxs-lookup"><span data-stu-id="56f2d-1432">The constructor is selected using *argument_list* if present and the overload resolution rules of [Overload resolution](expressions.md#overload-resolution).</span></span> <span data-ttu-id="56f2d-1433">Sada konstruktory instancí Release candidate zahrnuje všechny dostupné instance konstruktory deklarované v samotné třídě.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1433">The set of candidate instance constructors consists of all accessible instance constructors declared in the class itself.</span></span> <span data-ttu-id="56f2d-1434">Pokud tato sada je prázdný nebo jediný konstruktor instance nejlepší nelze identifikovat, dojde k chybě kompilace.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1434">If this set is empty, or if a single best instance constructor cannot be identified, a compile-time error occurs.</span></span> <span data-ttu-id="56f2d-1435">Pokud deklarace konstruktoru instance obsahuje inicializátor konstruktoru, který volá konstruktor samotný, dojde k chybě kompilace.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1435">If an instance constructor declaration includes a constructor initializer that invokes the constructor itself, a compile-time error occurs.</span></span>

<span data-ttu-id="56f2d-1436">Pokud konstruktor instance nemá žádný inicializátor konstruktoru, inicializátoru konstruktoru formuláře `base()` implicitně neposkytujeme.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1436">If an instance constructor has no constructor initializer, a constructor initializer of the form `base()` is implicitly provided.</span></span> <span data-ttu-id="56f2d-1437">Díky tomu se instanci deklarace konstruktoru formuláře</span><span class="sxs-lookup"><span data-stu-id="56f2d-1437">Thus, an instance constructor declaration of the form</span></span>
```csharp
C(...) {...}
```
<span data-ttu-id="56f2d-1438">přesně odpovídá</span><span class="sxs-lookup"><span data-stu-id="56f2d-1438">is exactly equivalent to</span></span>
```csharp
C(...): base() {...}
```

<span data-ttu-id="56f2d-1439">Obor parametry dána *formal_parameter_list* deklarace obsahuje konstruktor instance inicializátoru konstruktoru tohoto prohlášení.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1439">The scope of the parameters given by the *formal_parameter_list* of an instance constructor declaration includes the constructor initializer of that declaration.</span></span> <span data-ttu-id="56f2d-1440">Inicializátor konstruktoru. proto je povolený přístup k parametrů volanému konstruktoru.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1440">Thus, a constructor initializer is permitted to access the parameters of the constructor.</span></span> <span data-ttu-id="56f2d-1441">Příklad:</span><span class="sxs-lookup"><span data-stu-id="56f2d-1441">For example:</span></span>
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

<span data-ttu-id="56f2d-1442">Inicializátor konstruktoru instance nelze získat přístup k instanci vytváří.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1442">An instance constructor initializer cannot access the instance being created.</span></span> <span data-ttu-id="56f2d-1443">Proto je chyba kompilace odkazovat `this` v argumentu výrazu inicializátoru konstruktoru, jako je chyba kompilace pro výraz argumentu k odkazování kterýkoli člen instance prostřednictvím *simple_name*.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1443">Therefore it is a compile-time error to reference `this` in an argument expression of the constructor initializer, as is it a compile-time error for an argument expression to reference any instance member through a *simple_name*.</span></span>

### <a name="instance-variable-initializers"></a><span data-ttu-id="56f2d-1444">Inicializátory proměnné instance</span><span class="sxs-lookup"><span data-stu-id="56f2d-1444">Instance variable initializers</span></span>

<span data-ttu-id="56f2d-1445">Pokud konstruktor instance nemá žádný inicializátor konstruktoru, nebo má inicializátoru konstruktoru formuláře `base(...)`, tento konstruktor provádí implicitně inicializace určené *variable_initializer*s pole instancí deklarované ve své třídě.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1445">When an instance constructor has no constructor initializer, or it has a constructor initializer of the form `base(...)`, that constructor implicitly performs the initializations specified by the *variable_initializer*s of the instance fields declared in its class.</span></span> <span data-ttu-id="56f2d-1446">To odpovídá sekvenci přiřazení, která jsou spuštěna ihned po vstupu do konstruktoru a před implicitní volání konstruktoru přímou základní třídu.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1446">This corresponds to a sequence of assignments that are executed immediately upon entry to the constructor and before the implicit invocation of the direct base class constructor.</span></span> <span data-ttu-id="56f2d-1447">Proměnné inicializátory jsou provedeny v textové pořadí, v jakém jsou uvedeny v deklaraci třídy.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1447">The variable initializers are executed in the textual order in which they appear in the class declaration.</span></span>

### <a name="constructor-execution"></a><span data-ttu-id="56f2d-1448">Provádění konstruktoru</span><span class="sxs-lookup"><span data-stu-id="56f2d-1448">Constructor execution</span></span>

<span data-ttu-id="56f2d-1449">Inicializátory proměnné se transformují na přiřazovací příkazy a tyto přiřazovací příkazy jsou spouštěny před vyvoláním konstruktoru instance základní třídy.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1449">Variable initializers are transformed into assignment statements, and these assignment statements are executed before the invocation of the base class instance constructor.</span></span> <span data-ttu-id="56f2d-1450">Toto uspořádání zajistí, že všechna pole instancí jsou inicializovány jejich proměnné inicializátory předtím, než se spustí všechny příkazy, které mají přístup k této instanci.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1450">This ordering ensures that all instance fields are initialized by their variable initializers before any statements that have access to that instance are executed.</span></span>

<span data-ttu-id="56f2d-1451">Daný příklad</span><span class="sxs-lookup"><span data-stu-id="56f2d-1451">Given the example</span></span>
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
<span data-ttu-id="56f2d-1452">Když `new B()` slouží k vytvoření instance `B`, vytváří následující výstup:</span><span class="sxs-lookup"><span data-stu-id="56f2d-1452">when `new B()` is used to create an instance of `B`, the following output is produced:</span></span>
```
x = 1, y = 0
```

<span data-ttu-id="56f2d-1453">Hodnota `x` je 1, protože proměnná inicializátor, je proveden před vyvoláním konstruktoru instance základní třídy.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1453">The value of `x` is 1 because the variable initializer is executed before the base class instance constructor is invoked.</span></span> <span data-ttu-id="56f2d-1454">Však hodnoty `y` je 0 (výchozí hodnota `int`) protože přiřazení `y` není spuštěn až poté, co vrátí konstruktor základní třídy.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1454">However, the value of `y` is 0 (the default value of an `int`) because the assignment to `y` is not executed until after the base class constructor returns.</span></span>

<span data-ttu-id="56f2d-1455">Je vhodné popřemýšlet instance proměnné inicializátory a inicializátory konstruktoru jako příkazy, které jsou před něj vloženo automaticky *constructor_body*.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1455">It is useful to think of instance variable initializers and constructor initializers as statements that are automatically inserted before the *constructor_body*.</span></span> <span data-ttu-id="56f2d-1456">V příkladu</span><span class="sxs-lookup"><span data-stu-id="56f2d-1456">The example</span></span>
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
<span data-ttu-id="56f2d-1457">obsahuje několik proměnných inicializátory; také obsahuje konstruktor inicializátory obě formy (`base` a `this`).</span><span class="sxs-lookup"><span data-stu-id="56f2d-1457">contains several variable initializers; it also contains constructor initializers of both forms (`base` and `this`).</span></span> <span data-ttu-id="56f2d-1458">V příkladu odpovídá kódu je uvedeno níže, kde každý komentář označuje automaticky vložené – příkaz (syntaxi pro volání automaticky vložené konstruktoru není platný, ale slouží jenom ke znázornění mechanismu).</span><span class="sxs-lookup"><span data-stu-id="56f2d-1458">The example corresponds to the code shown below, where each comment indicates an automatically inserted statement (the syntax used for the automatically inserted constructor invocations isn't valid, but merely serves to illustrate the mechanism).</span></span>

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

### <a name="default-constructors"></a><span data-ttu-id="56f2d-1459">Výchozí konstruktory</span><span class="sxs-lookup"><span data-stu-id="56f2d-1459">Default constructors</span></span>

<span data-ttu-id="56f2d-1460">Pokud třída obsahuje žádné deklarace konstruktoru instance, se automaticky poskytuje výchozí konstruktor instance.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1460">If a class contains no instance constructor declarations, a default instance constructor is automatically provided.</span></span> <span data-ttu-id="56f2d-1461">Tento výchozí konstruktor jednoduše volá konstruktor základní třídy s přímým přístupem.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1461">That default constructor simply invokes the parameterless constructor of the direct base class.</span></span> <span data-ttu-id="56f2d-1462">Pokud je abstraktní třída je deklarovaná přístupnost pro výchozí konstruktor je chráněný.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1462">If the class is abstract then the declared accessibility for the default constructor is protected.</span></span> <span data-ttu-id="56f2d-1463">V opačném případě je deklarovaná přístupnost pro výchozí konstruktor je veřejný.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1463">Otherwise, the declared accessibility for the default constructor is public.</span></span> <span data-ttu-id="56f2d-1464">Proto výchozí konstruktor je vždy formuláře</span><span class="sxs-lookup"><span data-stu-id="56f2d-1464">Thus, the default constructor is always of the form</span></span>

```csharp
protected C(): base() {}
```
<span data-ttu-id="56f2d-1465">or</span><span class="sxs-lookup"><span data-stu-id="56f2d-1465">or</span></span>
```csharp
public C(): base() {}
```
<span data-ttu-id="56f2d-1466">kde `C` je název třídy.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1466">where `C` is the name of the class.</span></span> <span data-ttu-id="56f2d-1467">Pokud řešení přetížení není schopen určit jedinečné nejlepší kandidát pro inicializátoru konstruktoru základní třídy, dojde k chybě v době kompilace.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1467">If overload resolution is unable to determine a unique best candidate for the base class constructor initializer then a compile-time error occurs.</span></span>

<span data-ttu-id="56f2d-1468">V příkladu</span><span class="sxs-lookup"><span data-stu-id="56f2d-1468">In the example</span></span>
```csharp
class Message
{
    object sender;
    string text;
}
```
<span data-ttu-id="56f2d-1469">protože třída obsahuje žádné deklarace konstruktoru instance je k dispozici výchozí konstruktor.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1469">a default constructor is provided because the class contains no instance constructor declarations.</span></span> <span data-ttu-id="56f2d-1470">Proto je v příkladu přesně odpovídá</span><span class="sxs-lookup"><span data-stu-id="56f2d-1470">Thus, the example is precisely equivalent to</span></span>
```csharp
class Message
{
    object sender;
    string text;

    public Message(): base() {}
}
```

### <a name="private-constructors"></a><span data-ttu-id="56f2d-1471">Soukromé konstruktory</span><span class="sxs-lookup"><span data-stu-id="56f2d-1471">Private constructors</span></span>

<span data-ttu-id="56f2d-1472">Pokud třída `T` deklaruje pouze privátní instanci konstruktory, není možné pro třídy mimo textem programu `T` odvodit z `T` nebo přímo vytvářet instance `T`.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1472">When a class `T` declares only private instance constructors, it is not possible for classes outside the program text of `T` to derive from `T` or to directly create instances of `T`.</span></span> <span data-ttu-id="56f2d-1473">Proto pokud třída obsahuje pouze statické členy a není určen má být vytvořena, přidáním konstruktoru instance prázdný privátní brání vytváření instancí.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1473">Thus, if a class contains only static members and isn't intended to be instantiated, adding an empty private instance constructor will prevent instantiation.</span></span> <span data-ttu-id="56f2d-1474">Příklad:</span><span class="sxs-lookup"><span data-stu-id="56f2d-1474">For example:</span></span>
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

<span data-ttu-id="56f2d-1475">`Trig` Třídy skupiny s ní související metody a konstanty, ale není by se měly vytvořit.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1475">The `Trig` class groups related methods and constants, but is not intended to be instantiated.</span></span> <span data-ttu-id="56f2d-1476">Proto deklaruje jednu instanci privátní prázdný konstruktor.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1476">Therefore it declares a single empty private instance constructor.</span></span> <span data-ttu-id="56f2d-1477">Potlačit automatické generování výchozího konstruktoru musí být deklarován nejméně jedna instance konstruktoru.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1477">At least one instance constructor must be declared to suppress the automatic generation of a default constructor.</span></span>

### <a name="optional-instance-constructor-parameters"></a><span data-ttu-id="56f2d-1478">Parametry konstruktoru volitelné instance</span><span class="sxs-lookup"><span data-stu-id="56f2d-1478">Optional instance constructor parameters</span></span>

<span data-ttu-id="56f2d-1479">`this(...)` Formu inicializátoru konstruktoru se běžně používá v společně s přetížení k implementaci instance volitelné parametry konstruktoru.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1479">The `this(...)` form of constructor initializer is commonly used in conjunction with overloading to implement optional instance constructor parameters.</span></span> <span data-ttu-id="56f2d-1480">V příkladu</span><span class="sxs-lookup"><span data-stu-id="56f2d-1480">In the example</span></span>
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
<span data-ttu-id="56f2d-1481">první dvě instance konstruktory pouze zadat výchozí hodnoty pro chybějící argumenty.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1481">the first two instance constructors merely provide the default values for the missing arguments.</span></span> <span data-ttu-id="56f2d-1482">Používejte `this(...)` inicializátoru konstruktoru, který má být vyvolán třetí konstruktor instance, která ve skutečnosti provádí inicializuje novou instanci.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1482">Both use a `this(...)` constructor initializer to invoke the third instance constructor, which actually does the work of initializing the new instance.</span></span> <span data-ttu-id="56f2d-1483">Efekt je, volitelný konstruktor parametry:</span><span class="sxs-lookup"><span data-stu-id="56f2d-1483">The effect is that of optional constructor parameters:</span></span>
```csharp
Text t1 = new Text();                    // Same as Text(0, 0, null)
Text t2 = new Text(5, 10);               // Same as Text(5, 10, null)
Text t3 = new Text(5, 20, "Hello");
```

## <a name="static-constructors"></a><span data-ttu-id="56f2d-1484">Statické konstruktory</span><span class="sxs-lookup"><span data-stu-id="56f2d-1484">Static constructors</span></span>

<span data-ttu-id="56f2d-1485">A ***statický konstruktor*** je člen, který implementuje akce potřebné k inicializaci typu uzavřené třídy.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1485">A ***static constructor*** is a member that implements the actions required to initialize a closed class type.</span></span> <span data-ttu-id="56f2d-1486">Statické konstruktory jsou deklarovány pomocí *static_constructor_declaration*s:</span><span class="sxs-lookup"><span data-stu-id="56f2d-1486">Static constructors are declared using *static_constructor_declaration*s:</span></span>

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

<span data-ttu-id="56f2d-1487">A *static_constructor_declaration* může zahrnovat sadu *atributy* ([atributy](attributes.md)) a `extern` modifikátor ([externí metody](classes.md#external-methods)).</span><span class="sxs-lookup"><span data-stu-id="56f2d-1487">A *static_constructor_declaration* may include a set of *attributes* ([Attributes](attributes.md)) and an `extern` modifier ([External methods](classes.md#external-methods)).</span></span>

<span data-ttu-id="56f2d-1488">*Identifikátor* z *static_constructor_declaration* název musí být třída, ve kterém je deklarována statický konstruktor.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1488">The *identifier* of a *static_constructor_declaration* must name the class in which the static constructor is declared.</span></span> <span data-ttu-id="56f2d-1489">Pokud je zadán žádným jiným názvem, dojde k chybě kompilace.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1489">If any other name is specified, a compile-time error occurs.</span></span>

<span data-ttu-id="56f2d-1490">Pokud deklarace statického konstruktoru obsahuje `extern` modifikátor, statický konstruktor se říká, že ***externí statický konstruktor***.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1490">When a static constructor declaration includes an `extern` modifier, the static constructor is said to be an ***external static constructor***.</span></span> <span data-ttu-id="56f2d-1491">Protože deklaraci externí statický konstruktor poskytuje skutečné implementaci, jeho *static_constructor_body* skládá se ze středníku.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1491">Because an external static constructor declaration provides no actual implementation, its *static_constructor_body* consists of a semicolon.</span></span> <span data-ttu-id="56f2d-1492">Pro všechny ostatní deklarace statického konstruktoru *static_constructor_body* se skládá z *bloku* který určuje příkazy ke spuštění, aby bylo možné inicializovat třídu.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1492">For all other static constructor declarations, the *static_constructor_body* consists of a *block* which specifies the statements to execute in order to initialize the class.</span></span> <span data-ttu-id="56f2d-1493">To přesně odpovídá *method_body* statické metody s `void` návratový typ ([tělo metody](classes.md#method-body)).</span><span class="sxs-lookup"><span data-stu-id="56f2d-1493">This corresponds exactly to the *method_body* of a static method with a `void` return type ([Method body](classes.md#method-body)).</span></span>

<span data-ttu-id="56f2d-1494">Statické konstruktory nejsou děděna a nelze ji vyvolat přímo.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1494">Static constructors are not inherited, and cannot be called directly.</span></span>

<span data-ttu-id="56f2d-1495">Statický konstruktor pro typ třídy uzavřené spouští nejvýše jednou v doméně aplikace.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1495">The static constructor for a closed class type executes at most once in a given application domain.</span></span> <span data-ttu-id="56f2d-1496">Spuštění statický konstruktor se aktivuje při prvním z následujících událostí v rámci domény aplikace:</span><span class="sxs-lookup"><span data-stu-id="56f2d-1496">The execution of a static constructor is triggered by the first of the following events to occur within an application domain:</span></span>

*  <span data-ttu-id="56f2d-1497">Je vytvořena instance typu třídy.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1497">An instance of the class type is created.</span></span>
*  <span data-ttu-id="56f2d-1498">Žádné statické členy typu třídy jsou odkazovány.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1498">Any of the static members of the class type are referenced.</span></span>

<span data-ttu-id="56f2d-1499">Pokud třída obsahuje `Main` – metoda ([spuštění aplikace](basic-concepts.md#application-startup)) v které provádění začne, statický konstruktor pro danou třídu provede před `Main` metoda je volána.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1499">If a class contains the `Main` method ([Application Startup](basic-concepts.md#application-startup)) in which execution begins, the static constructor for that class executes before the `Main` method is called.</span></span>

<span data-ttu-id="56f2d-1500">Inicializace nového typu třídy uzavřené, nejprve novou sadu statických polí ([statické a instance pole](classes.md#static-and-instance-fields)) pro daný typ uzavřené se vytvoří.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1500">To initialize a new closed class type, first a new set of static fields ([Static and instance fields](classes.md#static-and-instance-fields)) for that particular closed type is created.</span></span> <span data-ttu-id="56f2d-1501">Každé statické pole se inicializuje na výchozí hodnotu ([výchozí hodnoty](variables.md#default-values)).</span><span class="sxs-lookup"><span data-stu-id="56f2d-1501">Each of the static fields is initialized to its default value ([Default values](variables.md#default-values)).</span></span> <span data-ttu-id="56f2d-1502">Další, statickými Inicializátory pole ([statické pole inicializace](classes.md#static-field-initialization)) jsou spouštěny pro tyto statická pole.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1502">Next, the static field initializers ([Static field initialization](classes.md#static-field-initialization)) are executed for those static fields.</span></span> <span data-ttu-id="56f2d-1503">Nakonec je spuštěn statický konstruktor.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1503">Finally, the static constructor is executed.</span></span>

<span data-ttu-id="56f2d-1504">V příkladu</span><span class="sxs-lookup"><span data-stu-id="56f2d-1504">The example</span></span>
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
<span data-ttu-id="56f2d-1505">musíte vytvořit výstup:</span><span class="sxs-lookup"><span data-stu-id="56f2d-1505">must produce the output:</span></span>
```
Init A
A.F
Init B
B.F
```
<span data-ttu-id="56f2d-1506">protože provádění `A`na statický konstruktor se aktivuje při volání `A.F`a provádění `B`na statický konstruktor se aktivuje při volání `B.F`.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1506">because the execution of `A`'s static constructor is triggered by the call to `A.F`, and the execution of `B`'s static constructor is triggered by the call to `B.F`.</span></span>

<span data-ttu-id="56f2d-1507">Je možné k vytvoření cyklické závislosti, které umožňují statická pole s proměnnou inicializátory má být dodržen v jejich výchozí hodnoty stavu.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1507">It is possible to construct circular dependencies that allow static fields with variable initializers to be observed in their default value state.</span></span>

<span data-ttu-id="56f2d-1508">V příkladu</span><span class="sxs-lookup"><span data-stu-id="56f2d-1508">The example</span></span>
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
<span data-ttu-id="56f2d-1509">Vytvoří výstup</span><span class="sxs-lookup"><span data-stu-id="56f2d-1509">produces the output</span></span>
```
X = 1, Y = 2
```

<span data-ttu-id="56f2d-1510">Ke spuštění `Main` , systém nejprve spuštěním metody inicializátor pro `B.Y`, před třídy `B`na statický konstruktor.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1510">To execute the `Main` method, the system first runs the initializer for `B.Y`, prior to class `B`'s static constructor.</span></span> <span data-ttu-id="56f2d-1511">`Y`pro inicializátor způsobí, že `A`na statický konstruktor, chcete-li spustit, protože hodnota `A.X` odkazuje.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1511">`Y`'s initializer causes `A`'s static constructor to be run because the value of `A.X` is referenced.</span></span> <span data-ttu-id="56f2d-1512">Statický konstruktor třídy `A` pak pokračuje k výpočtu hodnoty `X`a přitom tak načte výchozí hodnotu `Y`, což je nula.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1512">The static constructor of `A` in turn proceeds to compute the value of `X`, and in doing so fetches the default value of `Y`, which is zero.</span></span> <span data-ttu-id="56f2d-1513">`A.X` Proto je inicializován na hodnotu 1.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1513">`A.X` is thus initialized to 1.</span></span> <span data-ttu-id="56f2d-1514">Proces spuštění `A`statickými Inicializátory pole a statický konstruktor pak dokončí, vrácení k výpočtu počáteční hodnota `Y`, výsledkem bude 2.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1514">The process of running `A`'s static field initializers and static constructor then completes, returning to the calculation of the initial value of `Y`, the result of which becomes 2.</span></span>

<span data-ttu-id="56f2d-1515">Protože statický konstruktor provádí právě jednou pro každou uzavřený konstruovaný třídy typu, je vhodné místo pro vynutit kontroly za běhu na parametr typu, který nelze zaregistrovat v době kompilace prostřednictvím omezení ([parametr typu omezení](classes.md#type-parameter-constraints)).</span><span class="sxs-lookup"><span data-stu-id="56f2d-1515">Because the static constructor is executed exactly once for each closed constructed class type, it is a convenient place to enforce run-time checks on the type parameter that cannot be checked at compile-time via constraints ([Type parameter constraints](classes.md#type-parameter-constraints)).</span></span> <span data-ttu-id="56f2d-1516">Například následující typ používá statický konstruktor k vynucení, že je argument typu výčtu:</span><span class="sxs-lookup"><span data-stu-id="56f2d-1516">For example, the following type uses a static constructor to enforce that the type argument is an enum:</span></span>
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

## <a name="destructors"></a><span data-ttu-id="56f2d-1517">Destruktory</span><span class="sxs-lookup"><span data-stu-id="56f2d-1517">Destructors</span></span>

<span data-ttu-id="56f2d-1518">A ***destruktor*** je člen, který implementuje akce potřebné k destrukci instance třídy.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1518">A ***destructor*** is a member that implements the actions required to destruct an instance of a class.</span></span> <span data-ttu-id="56f2d-1519">Destruktor je deklarován pomocí *destructor_declaration*:</span><span class="sxs-lookup"><span data-stu-id="56f2d-1519">A destructor is declared using a *destructor_declaration*:</span></span>

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

<span data-ttu-id="56f2d-1520">A *destructor_declaration* může zahrnovat sadu *atributy* ([atributy](attributes.md)).</span><span class="sxs-lookup"><span data-stu-id="56f2d-1520">A *destructor_declaration* may include a set of *attributes* ([Attributes](attributes.md)).</span></span>

<span data-ttu-id="56f2d-1521">*Identifikátor* z *destructor_declaration* název musí být třída, ve kterém je deklarována destruktor.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1521">The *identifier* of a *destructor_declaration* must name the class in which the destructor is declared.</span></span> <span data-ttu-id="56f2d-1522">Pokud je zadán žádným jiným názvem, dojde k chybě kompilace.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1522">If any other name is specified, a compile-time error occurs.</span></span>

<span data-ttu-id="56f2d-1523">Pokud deklarace destruktoru obsahuje `extern` modifikátor, destruktor se říká, že ***externí destruktor***.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1523">When a destructor declaration includes an `extern` modifier, the destructor is said to be an ***external destructor***.</span></span> <span data-ttu-id="56f2d-1524">Protože deklaraci externí destruktor poskytuje skutečné implementaci, jeho *destructor_body* skládá se ze středníku.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1524">Because an external destructor declaration provides no actual implementation, its *destructor_body* consists of a semicolon.</span></span> <span data-ttu-id="56f2d-1525">Pro všechny ostatní destruktory *destructor_body* se skládá z *bloku* který určuje příkazy provést, aby destrukce instance třídy.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1525">For all other destructors, the *destructor_body* consists of a *block* which specifies the statements to execute in order to destruct an instance of the class.</span></span> <span data-ttu-id="56f2d-1526">A *destructor_body* přesně odpovídá *method_body* metody instance s `void` návratový typ ([tělo metody](classes.md#method-body)).</span><span class="sxs-lookup"><span data-stu-id="56f2d-1526">A *destructor_body* corresponds exactly to the *method_body* of an instance method with a `void` return type ([Method body](classes.md#method-body)).</span></span>

<span data-ttu-id="56f2d-1527">Destruktory, nejsou děděna.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1527">Destructors are not inherited.</span></span> <span data-ttu-id="56f2d-1528">Proto třída nemá žádné destruktory než ten, který může být deklarován v dané třídě.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1528">Thus, a class has no destructors other than the one which may be declared in that class.</span></span>

<span data-ttu-id="56f2d-1529">Protože destruktor je nemusíte mít žádné parametry, nemůže být přetížená, takže třída může mít, maximálně jeden destruktor.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1529">Since a destructor is required to have no parameters, it cannot be overloaded, so a class can have, at most, one destructor.</span></span>

<span data-ttu-id="56f2d-1530">Destruktory jsou vyvolány automaticky a nelze ji vyvolat explicitně.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1530">Destructors are invoked automatically, and cannot be invoked explicitly.</span></span> <span data-ttu-id="56f2d-1531">Když už není možné použít tuto instanci žádný kód, bude vhodné pro odstranění instance.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1531">An instance becomes eligible for destruction when it is no longer possible for any code to use that instance.</span></span> <span data-ttu-id="56f2d-1532">V okamžiku, jakmile bude instance nárok na odstranění může dojít k spuštění destruktoru pro instanci.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1532">Execution of the destructor for the instance may occur at any time after the instance becomes eligible for destruction.</span></span> <span data-ttu-id="56f2d-1533">Při destrukci instancí jsou volány destruktory v řetězu dědičnosti této instance, v pořadí od těch s nejvíce odvozené nejméně odvozený.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1533">When an instance is destructed, the destructors in that instance's inheritance chain are called, in order, from most derived to least derived.</span></span> <span data-ttu-id="56f2d-1534">Destruktor může být spuštěna v libovolném vlákně.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1534">A destructor may be executed on any thread.</span></span> <span data-ttu-id="56f2d-1535">Další informace o pravidlech, která určují, kdy a jak je spuštěn destruktor, naleznete v tématu [Automatická správa paměti](basic-concepts.md#automatic-memory-management).</span><span class="sxs-lookup"><span data-stu-id="56f2d-1535">For further discussion of the rules that govern when and how a destructor is executed, see [Automatic memory management](basic-concepts.md#automatic-memory-management).</span></span>

<span data-ttu-id="56f2d-1536">Výstup v příkladu</span><span class="sxs-lookup"><span data-stu-id="56f2d-1536">The output of the example</span></span>
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
<span data-ttu-id="56f2d-1537">is</span><span class="sxs-lookup"><span data-stu-id="56f2d-1537">is</span></span>
```
B's destructor
A's destructor
```
<span data-ttu-id="56f2d-1538">protože destruktory v řetěz dědičnosti jsou volány v pořadí od těch s nejvíce odvozené nejméně odvozený.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1538">since destructors in an inheritance chain are called in order, from most derived to least derived.</span></span>

<span data-ttu-id="56f2d-1539">Destruktory jsou implementované přepisování virtuální metody `Finalize` na `System.Object`.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1539">Destructors are implemented by overriding the virtual method `Finalize` on `System.Object`.</span></span> <span data-ttu-id="56f2d-1540">Potlačí tuto metodu nebo volání (nebo jeho přepsání) nejsou povoleny programy jazyka C# přímo.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1540">C# programs are not permitted to override this method or call it (or overrides of it) directly.</span></span> <span data-ttu-id="56f2d-1541">Například program</span><span class="sxs-lookup"><span data-stu-id="56f2d-1541">For instance, the program</span></span>
```csharp
class A 
{
    override protected void Finalize() {}    // error

    public void F() {
        this.Finalize();                     // error
    }
}
```
<span data-ttu-id="56f2d-1542">obsahuje dva chyby.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1542">contains two errors.</span></span>

<span data-ttu-id="56f2d-1543">Kompilátor se chová jako by tuto metodu a přepsání je vůbec neexistovat.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1543">The compiler behaves as if this method, and overrides of it, do not exist at all.</span></span> <span data-ttu-id="56f2d-1544">Proto tento program:</span><span class="sxs-lookup"><span data-stu-id="56f2d-1544">Thus, this program:</span></span>
```csharp
class A 
{
    void Finalize() {}                            // permitted
}
```
<span data-ttu-id="56f2d-1545">je platný, a skryje zobrazují metodu `System.Object`společnosti `Finalize` metody.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1545">is valid, and the method shown hides `System.Object`'s `Finalize` method.</span></span>

<span data-ttu-id="56f2d-1546">Informace o chování při z destruktoru je vyvolána výjimka, najdete v článku [způsob zpracování výjimek](exceptions.md#how-exceptions-are-handled).</span><span class="sxs-lookup"><span data-stu-id="56f2d-1546">For a discussion of the behavior when an exception is thrown from a destructor, see [How exceptions are handled](exceptions.md#how-exceptions-are-handled).</span></span>

## <a name="iterators"></a><span data-ttu-id="56f2d-1547">Iterátory</span><span class="sxs-lookup"><span data-stu-id="56f2d-1547">Iterators</span></span>

<span data-ttu-id="56f2d-1548">Členské funkce ([funkce členy](expressions.md#function-members)) implementované pomocí blok iterátoru ([bloky](statements.md#blocks)) je volána ***iterátoru***.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1548">A function member ([Function members](expressions.md#function-members)) implemented using an iterator block ([Blocks](statements.md#blocks)) is called an ***iterator***.</span></span>

<span data-ttu-id="56f2d-1549">Blok iterátoru, mohou být použity jako tělo funkce člena jako návratový typ odpovídající členské funkce je součástí rozhraní pro výčty ([rozhraní pro výčty](classes.md#enumerator-interfaces)) nebo jedna z vyčíslitelná rozhraní ([Vyčíslitelná rozhraní](classes.md#enumerable-interfaces)).</span><span class="sxs-lookup"><span data-stu-id="56f2d-1549">An iterator block may be used as the body of a function member as long as the return type of the corresponding function member is one of the enumerator interfaces ([Enumerator interfaces](classes.md#enumerator-interfaces)) or one of the enumerable interfaces ([Enumerable interfaces](classes.md#enumerable-interfaces)).</span></span> <span data-ttu-id="56f2d-1550">To může nastat, jako *method_body*, *operator_body* nebo *accessor_body*, zatímco události, konstruktory instancí, statické konstruktory a destruktory nelze implementovat jako iterátory.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1550">It can occur as a *method_body*, *operator_body* or *accessor_body*, whereas events, instance constructors, static constructors and destructors cannot be implemented as iterators.</span></span>

<span data-ttu-id="56f2d-1551">Je-li členské funkce je implementována pomocí blok iterátoru, je chyba kompilace pro seznam formálních parametrů členské funkce lze zadat `ref` nebo `out` parametry.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1551">When a function member is implemented using an iterator block, it is a compile-time error for the formal parameter list of the function member to specify any `ref` or `out` parameters.</span></span>

### <a name="enumerator-interfaces"></a><span data-ttu-id="56f2d-1552">Rozhraní pro výčty</span><span class="sxs-lookup"><span data-stu-id="56f2d-1552">Enumerator interfaces</span></span>

<span data-ttu-id="56f2d-1553">***Rozhraní pro výčty*** jsou obecné rozhraní `System.Collections.IEnumerator` a všech instancí obecného rozhraní `System.Collections.Generic.IEnumerator<T>`.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1553">The ***enumerator interfaces*** are the non-generic interface `System.Collections.IEnumerator` and all instantiations of the generic interface `System.Collections.Generic.IEnumerator<T>`.</span></span> <span data-ttu-id="56f2d-1554">Pro účely jako stručný výtah v této kapitole tato rozhraní jsou odkazovány jako `IEnumerator` a `IEnumerator<T>`v uvedeném pořadí.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1554">For the sake of brevity, in this chapter these interfaces are referenced as `IEnumerator` and `IEnumerator<T>`, respectively.</span></span>

### <a name="enumerable-interfaces"></a><span data-ttu-id="56f2d-1555">Vyčíslitelná rozhraní</span><span class="sxs-lookup"><span data-stu-id="56f2d-1555">Enumerable interfaces</span></span>

<span data-ttu-id="56f2d-1556">***Vyčíslitelná rozhraní*** jsou obecné rozhraní `System.Collections.IEnumerable` a všech instancí obecného rozhraní `System.Collections.Generic.IEnumerable<T>`.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1556">The ***enumerable interfaces*** are the non-generic interface `System.Collections.IEnumerable` and all instantiations of the generic interface `System.Collections.Generic.IEnumerable<T>`.</span></span> <span data-ttu-id="56f2d-1557">Pro účely jako stručný výtah v této kapitole tato rozhraní jsou odkazovány jako `IEnumerable` a `IEnumerable<T>`v uvedeném pořadí.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1557">For the sake of brevity, in this chapter these interfaces are referenced as `IEnumerable` and `IEnumerable<T>`, respectively.</span></span>

### <a name="yield-type"></a><span data-ttu-id="56f2d-1558">Typ příkazu yield</span><span class="sxs-lookup"><span data-stu-id="56f2d-1558">Yield type</span></span>

<span data-ttu-id="56f2d-1559">Iterátor vytvoří posloupnost hodnot, všechny stejného typu.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1559">An iterator produces a sequence of values, all of the same type.</span></span> <span data-ttu-id="56f2d-1560">Tento typ se nazývá ***yield typ*** iterátoru.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1560">This type is called the ***yield type*** of the iterator.</span></span>

*  <span data-ttu-id="56f2d-1561">Yield typ iterátoru, který vrací `IEnumerator` nebo `IEnumerable` je `object`.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1561">The yield type of an iterator that returns `IEnumerator` or `IEnumerable` is `object`.</span></span>
*  <span data-ttu-id="56f2d-1562">Yield typ iterátoru, který vrací `IEnumerator<T>` nebo `IEnumerable<T>` je `T`.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1562">The yield type of an iterator that returns `IEnumerator<T>` or `IEnumerable<T>` is `T`.</span></span>

### <a name="enumerator-objects"></a><span data-ttu-id="56f2d-1563">Výčet objektů</span><span class="sxs-lookup"><span data-stu-id="56f2d-1563">Enumerator objects</span></span>

<span data-ttu-id="56f2d-1564">Pokud člen funkce vrací enumerátor typ rozhraní je implementováno pomocí blok iterátoru, volání členské funkce nespustí ihned kód v bloku iterátoru.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1564">When a function member returning an enumerator interface type is implemented using an iterator block, invoking the function member does not immediately execute the code in the iterator block.</span></span> <span data-ttu-id="56f2d-1565">Místo toho ***objekt enumerator*** je vytvořena a vrácena.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1565">Instead, an ***enumerator object*** is created and returned.</span></span> <span data-ttu-id="56f2d-1566">Tento objekt zapouzdřuje kód určený v bloku iterátoru a dojde k provádění kódu v bloku iterátoru při objekt enumerator `MoveNext` vyvolání metody.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1566">This object encapsulates the code specified in the iterator block, and execution of the code in the iterator block occurs when the enumerator object's `MoveNext` method is invoked.</span></span> <span data-ttu-id="56f2d-1567">Objekt enumerátoru má následující vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="56f2d-1567">An enumerator object has the following characteristics:</span></span>

*  <span data-ttu-id="56f2d-1568">Implementuje `IEnumerator` a `IEnumerator<T>`, kde `T` yield typu iterátoru.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1568">It implements `IEnumerator` and `IEnumerator<T>`, where `T` is the yield type of the iterator.</span></span>
*  <span data-ttu-id="56f2d-1569">Implementuje `System.IDisposable`.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1569">It implements `System.IDisposable`.</span></span>
*  <span data-ttu-id="56f2d-1570">Je inicializována s kopii hodnoty argumentů (pokud existuje) a byla předána hodnota instanci členské funkce.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1570">It is initialized with a copy of the argument values (if any) and instance value passed to the function member.</span></span>
*  <span data-ttu-id="56f2d-1571">Má čtyři možných stavů ***před***, ***systémem***, ***pozastaveno***, a ***po***a je zpočátku v ***před***  stavu.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1571">It has four potential states, ***before***, ***running***, ***suspended***, and ***after***, and is initially in the ***before*** state.</span></span>

<span data-ttu-id="56f2d-1572">Objekt enumerátoru je obvykle instance třídy generované kompilátorem enumerátor, který zapouzdřuje kód v bloku iterátoru a implementuje rozhraní čítače, ale jiné metody implementace jsou možné.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1572">An enumerator object is typically an instance of a compiler-generated enumerator class that encapsulates the code in the iterator block and implements the enumerator interfaces, but other methods of implementation are possible.</span></span> <span data-ttu-id="56f2d-1573">Pokud třídu enumerátor je generovaný kompilátorem, se vnoří dané třídy, přímo nebo nepřímo, třídy obsahující členské funkce bude mít přístupnost private a bude mít název vyhrazený pro použití kompilátoru ([identifikátory ](lexical-structure.md#identifiers)).</span><span class="sxs-lookup"><span data-stu-id="56f2d-1573">If an enumerator class is generated by the compiler, that class will be nested, directly or indirectly, in the class containing the function member, it will have private accessibility, and it will have a name reserved for compiler use ([Identifiers](lexical-structure.md#identifiers)).</span></span>

<span data-ttu-id="56f2d-1574">Objekt enumerátoru může implementovat více rozhraní než uvedené výše.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1574">An enumerator object may implement more interfaces than those specified above.</span></span>

<span data-ttu-id="56f2d-1575">Následující části popisují přesné chování `MoveNext`, `Current`, a `Dispose` členy `IEnumerable` a `IEnumerable<T>` rozhraní implementace poskytované objekt enumerátoru.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1575">The following sections describe the exact behavior of the `MoveNext`, `Current`, and `Dispose` members of the `IEnumerable` and `IEnumerable<T>` interface implementations provided by an enumerator object.</span></span>

<span data-ttu-id="56f2d-1576">Všimněte si, že objekty enumerátor nepodporuje `IEnumerator.Reset` metody.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1576">Note that enumerator objects do not support the `IEnumerator.Reset` method.</span></span> <span data-ttu-id="56f2d-1577">Volání této metody způsobí, že `System.NotSupportedException` vyvolání.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1577">Invoking this method causes a `System.NotSupportedException` to be thrown.</span></span>

#### <a name="the-movenext-method"></a><span data-ttu-id="56f2d-1578">MoveNext – metoda</span><span class="sxs-lookup"><span data-stu-id="56f2d-1578">The MoveNext method</span></span>

<span data-ttu-id="56f2d-1579">`MoveNext` Metoda objekt enumerátoru zapouzdřuje kód blok iterátoru.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1579">The `MoveNext` method of an enumerator object encapsulates the code of an iterator block.</span></span> <span data-ttu-id="56f2d-1580">Volání `MoveNext` metody spustí kód v bloku iterátoru a nastaví `Current` vlastnost objekt enumerator podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1580">Invoking the `MoveNext` method executes code in the iterator block and sets the `Current` property of the enumerator object as appropriate.</span></span> <span data-ttu-id="56f2d-1581">Přesné akce prováděné `MoveNext` závisí na stavu objekt enumerator při `MoveNext` vyvolání:</span><span class="sxs-lookup"><span data-stu-id="56f2d-1581">The precise action performed by `MoveNext` depends on the state of the enumerator object when `MoveNext` is invoked:</span></span>

*  <span data-ttu-id="56f2d-1582">Pokud je stav objektu enumerátor ***před***, vyvolání `MoveNext`:</span><span class="sxs-lookup"><span data-stu-id="56f2d-1582">If the state of the enumerator object is ***before***, invoking `MoveNext`:</span></span>
   * <span data-ttu-id="56f2d-1583">Změní stav, který má ***systémem***.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1583">Changes the state to ***running***.</span></span>
   * <span data-ttu-id="56f2d-1584">Inicializuje parametry (včetně `this`) na hodnoty argumentu a hodnotu instance uložily v době, kdy byl inicializován objekt enumerator bloku iterátoru.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1584">Initializes the parameters (including `this`) of the iterator block to the argument values and instance value saved when the enumerator object was initialized.</span></span>
   * <span data-ttu-id="56f2d-1585">Opakuje blok iterátoru od začátku, dokud se provádění dojde k přerušení (jak je popsáno níže).</span><span class="sxs-lookup"><span data-stu-id="56f2d-1585">Executes the iterator block from the beginning until execution is interrupted (as described below).</span></span>
*  <span data-ttu-id="56f2d-1586">Pokud státu objekt enumerator ***systémem***, výsledek vyvolání `MoveNext` není zadána.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1586">If the state of the enumerator object is ***running***, the result of invoking `MoveNext` is unspecified.</span></span>
*  <span data-ttu-id="56f2d-1587">Pokud je stav objektu enumerátor ***pozastaveno***, vyvolání `MoveNext`:</span><span class="sxs-lookup"><span data-stu-id="56f2d-1587">If the state of the enumerator object is ***suspended***, invoking `MoveNext`:</span></span>
   * <span data-ttu-id="56f2d-1588">Změní stav, který má ***systémem***.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1588">Changes the state to ***running***.</span></span>
   * <span data-ttu-id="56f2d-1589">Obnoví hodnoty uloženy při provedení bloku iterátoru posledního pozastavení hodnoty všech místních proměnných a parametry (včetně to).</span><span class="sxs-lookup"><span data-stu-id="56f2d-1589">Restores the values of all local variables and parameters (including this) to the values saved when execution of the iterator block was last suspended.</span></span> <span data-ttu-id="56f2d-1590">Všimněte si, že obsah ze všech objektů, které odkazují tyto proměnné mohou změnily od předchozího volání k MoveNext.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1590">Note that the contents of any objects referenced by these variables may have changed since the previous call to MoveNext.</span></span>
   * <span data-ttu-id="56f2d-1591">Pokračuje v provádění bezprostředně po bloku iterátoru `yield return` příkaz, který způsobil pozastavení provádění a pokračuje, dokud provádění dojde k přerušení (jak je popsáno níže).</span><span class="sxs-lookup"><span data-stu-id="56f2d-1591">Resumes execution of the iterator block immediately following the `yield return` statement that caused the suspension of execution and continues until execution is interrupted (as described below).</span></span>
*  <span data-ttu-id="56f2d-1592">Pokud je stav objektu enumerátor ***po***, vyvolání `MoveNext` vrátí `false`.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1592">If the state of the enumerator object is ***after***, invoking `MoveNext` returns `false`.</span></span>


<span data-ttu-id="56f2d-1593">Při `MoveNext` provede blok iterátoru, provedení může být přerušeno čtyři způsoby: pomocí `yield return` příkaz, `yield break` prohlášení, prostřednictvím zjištění konce bloku iterátoru a výjimku vyvolána a rozšířena z blok iterátoru.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1593">When `MoveNext` executes the iterator block, execution can be interrupted in four ways: By a `yield return` statement, by a `yield break` statement, by encountering the end of the iterator block, and by an exception being thrown and propagated out of the iterator block.</span></span>

*  <span data-ttu-id="56f2d-1594">Když `yield return` zjistil – příkaz ([příkaz yield](statements.md#the-yield-statement)):</span><span class="sxs-lookup"><span data-stu-id="56f2d-1594">When a `yield return` statement is encountered ([The yield statement](statements.md#the-yield-statement)):</span></span>
   * <span data-ttu-id="56f2d-1595">Výraz zadaný v příkazu je vyhodnocen, implicitně převeden na typ yield a přiřazená `Current` vlastnost v objektu enumerátor.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1595">The expression given in the statement is evaluated, implicitly converted to the yield type, and assigned to the `Current` property of the enumerator object.</span></span>
   * <span data-ttu-id="56f2d-1596">Spuštění tělo iterátoru je pozastaveno.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1596">Execution of the iterator body is suspended.</span></span> <span data-ttu-id="56f2d-1597">Hodnoty všech místních proměnných a parametry (včetně `this`) jsou uloženy, protože je to umístění `yield return` příkazu.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1597">The values of all local variables and parameters (including `this`) are saved, as is the location of this `yield return` statement.</span></span> <span data-ttu-id="56f2d-1598">Pokud `yield return` příkaz je v jedné nebo více `try` blokuje, přidružené `finally` v tuto chvíli nejsou provedeny bloky.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1598">If the `yield return` statement is within one or more `try` blocks, the associated `finally` blocks are not executed at this time.</span></span>
   * <span data-ttu-id="56f2d-1599">Stav enumerátoru objektu se změní na ***pozastaveno***.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1599">The state of the enumerator object is changed to ***suspended***.</span></span>
   * <span data-ttu-id="56f2d-1600">`MoveNext` Vrátí metoda `true` volajícímu, označující, že byly úspěšně posunuty iterace na nejbližší hodnotu.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1600">The `MoveNext` method returns `true` to its caller, indicating that the iteration successfully advanced to the next value.</span></span>
*  <span data-ttu-id="56f2d-1601">Když `yield break` zjistil – příkaz ([příkaz yield](statements.md#the-yield-statement)):</span><span class="sxs-lookup"><span data-stu-id="56f2d-1601">When a `yield break` statement is encountered ([The yield statement](statements.md#the-yield-statement)):</span></span>
   * <span data-ttu-id="56f2d-1602">Pokud `yield break` příkaz je v jedné nebo více `try` blokuje, přidružené `finally` bloky provádějí.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1602">If the `yield break` statement is within one or more `try` blocks, the associated `finally` blocks are executed.</span></span>
   * <span data-ttu-id="56f2d-1603">Stav enumerátoru objektu se změní na ***po***.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1603">The state of the enumerator object is changed to ***after***.</span></span>
   * <span data-ttu-id="56f2d-1604">`MoveNext` Vrátí metoda `false` volajícímu, označující, že je k dokončení iterace.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1604">The `MoveNext` method returns `false` to its caller, indicating that the iteration is complete.</span></span>
*  <span data-ttu-id="56f2d-1605">Pokud je nalezen konec tělo iterátoru:</span><span class="sxs-lookup"><span data-stu-id="56f2d-1605">When the end of the iterator body is encountered:</span></span>
   * <span data-ttu-id="56f2d-1606">Stav enumerátoru objektu se změní na ***po***.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1606">The state of the enumerator object is changed to ***after***.</span></span>
   * <span data-ttu-id="56f2d-1607">`MoveNext` Vrátí metoda `false` volajícímu, označující, že je k dokončení iterace.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1607">The `MoveNext` method returns `false` to its caller, indicating that the iteration is complete.</span></span>
*  <span data-ttu-id="56f2d-1608">Když výjimka je výjimka vyvolána a rozšířena z bloku iterátoru:</span><span class="sxs-lookup"><span data-stu-id="56f2d-1608">When an exception is thrown and propagated out of the iterator block:</span></span>
   * <span data-ttu-id="56f2d-1609">Odpovídající `finally` bloky v tělo iterátoru bude proveden šíření výjimek.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1609">Appropriate `finally` blocks in the iterator body will have been executed by the exception propagation.</span></span>
   * <span data-ttu-id="56f2d-1610">Stav enumerátoru objektu se změní na ***po***.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1610">The state of the enumerator object is changed to ***after***.</span></span>
   * <span data-ttu-id="56f2d-1611">Šíření výjimek pokračuje do volajícího `MoveNext` metody.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1611">The exception propagation continues to the caller of the `MoveNext` method.</span></span>

#### <a name="the-current-property"></a><span data-ttu-id="56f2d-1612">Vlastnost Current</span><span class="sxs-lookup"><span data-stu-id="56f2d-1612">The Current property</span></span>

<span data-ttu-id="56f2d-1613">Objekt enumerátoru `Current` vlastnost je ovlivňován `yield return` příkazy v bloku iterátoru.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1613">An enumerator object's `Current` property is affected by `yield return` statements in the iterator block.</span></span>

<span data-ttu-id="56f2d-1614">Pokud je objekt enumerátoru v ***pozastaveno*** stavu, hodnota `Current` se hodnoty nastavené v předchozím volání `MoveNext`.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1614">When an enumerator object is in the ***suspended*** state, the value of `Current` is the value set by the previous call to `MoveNext`.</span></span> <span data-ttu-id="56f2d-1615">Pokud je objekt enumerátoru v ***před***, ***systémem***, nebo ***po*** stavy, výsledek přístup k `Current` není zadána.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1615">When an enumerator object is in the ***before***, ***running***, or ***after*** states, the result of accessing `Current` is unspecified.</span></span>

<span data-ttu-id="56f2d-1616">Iterátor s výtěžností typu jiného než `object`, výsledek přístup k `Current` prostřednictvím objekt enumerator `IEnumerable` odpovídá přístup k implementaci `Current` prostřednictvím objekt enumerator `IEnumerator<T>` implementace a přetypování výsledek, který má `object`.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1616">For an iterator with a yield type other than `object`, the result of accessing `Current` through the enumerator object's `IEnumerable` implementation corresponds to accessing `Current` through the enumerator object's `IEnumerator<T>` implementation and casting the result to `object`.</span></span>

#### <a name="the-dispose-method"></a><span data-ttu-id="56f2d-1617">Dispose – metoda</span><span class="sxs-lookup"><span data-stu-id="56f2d-1617">The Dispose method</span></span>

<span data-ttu-id="56f2d-1618">`Dispose` Metoda se používá k vyčištění iterace přeneste objekt enumerator do ***po*** stavu.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1618">The `Dispose` method is used to clean up the iteration by bringing the enumerator object to the ***after*** state.</span></span>

*  <span data-ttu-id="56f2d-1619">Pokud je stav objektu enumerátor ***před***, vyvolání `Dispose` změní stav, který má ***po***.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1619">If the state of the enumerator object is ***before***, invoking `Dispose` changes the state to ***after***.</span></span>
*  <span data-ttu-id="56f2d-1620">Pokud státu objekt enumerator ***systémem***, výsledek vyvolání `Dispose` není zadána.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1620">If the state of the enumerator object is ***running***, the result of invoking `Dispose` is unspecified.</span></span>
*  <span data-ttu-id="56f2d-1621">Pokud je stav objektu enumerátor ***pozastaveno***, vyvolání `Dispose`:</span><span class="sxs-lookup"><span data-stu-id="56f2d-1621">If the state of the enumerator object is ***suspended***, invoking `Dispose`:</span></span>
   * <span data-ttu-id="56f2d-1622">Změní stav, který má ***systémem***.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1622">Changes the state to ***running***.</span></span>
   * <span data-ttu-id="56f2d-1623">Provede všechny nakonec bloky jakoby posledního spuštěného `yield return` příkaz byly `yield break` příkazu.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1623">Executes any finally blocks as if the last executed `yield return` statement were a `yield break` statement.</span></span> <span data-ttu-id="56f2d-1624">Pokud to způsobí, že výjimka bude vyvolána a rozšíří mimo tělo iterátoru, stav objektu enumerátor je nastavený na ***po*** a výjimka se šíří do volajícího `Dispose` metody.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1624">If this causes an exception to be thrown and propagated out of the iterator body, the state of the enumerator object is set to ***after*** and the exception is propagated to the caller of the `Dispose` method.</span></span>
   * <span data-ttu-id="56f2d-1625">Změní stav, který má ***po***.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1625">Changes the state to ***after***.</span></span>
*  <span data-ttu-id="56f2d-1626">Pokud je stav objektu enumerátor ***po***, vyvolání `Dispose` nemá žádný vliv.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1626">If the state of the enumerator object is ***after***, invoking `Dispose` has no affect.</span></span>

### <a name="enumerable-objects"></a><span data-ttu-id="56f2d-1627">Vyčíslitelná objekty</span><span class="sxs-lookup"><span data-stu-id="56f2d-1627">Enumerable objects</span></span>

<span data-ttu-id="56f2d-1628">Pokud člen funkce vracející typ vyčíslitelná rozhraní je implementováno pomocí blok iterátoru, volání členské funkce nespustí ihned kód v bloku iterátoru.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1628">When a function member returning an enumerable interface type is implemented using an iterator block, invoking the function member does not immediately execute the code in the iterator block.</span></span> <span data-ttu-id="56f2d-1629">Místo toho ***vyčíslitelný objekt*** je vytvořena a vrácena.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1629">Instead, an ***enumerable object*** is created and returned.</span></span> <span data-ttu-id="56f2d-1630">Vyčíslitelný objekt `GetEnumerator` metoda vrací zadaný objekt enumerátoru, který zapouzdřuje kód v bloku iterátoru a dojde k provádění kódu v bloku iterátoru při objekt enumerator `MoveNext` vyvolání metody.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1630">The enumerable object's `GetEnumerator` method returns an enumerator object that encapsulates the code specified in the iterator block, and execution of the code in the iterator block occurs when the enumerator object's `MoveNext` method is invoked.</span></span> <span data-ttu-id="56f2d-1631">Vyčíslitelný objekt má následující vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="56f2d-1631">An enumerable object has the following characteristics:</span></span>

*  <span data-ttu-id="56f2d-1632">Implementuje `IEnumerable` a `IEnumerable<T>`, kde `T` yield typu iterátoru.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1632">It implements `IEnumerable` and `IEnumerable<T>`, where `T` is the yield type of the iterator.</span></span>
*  <span data-ttu-id="56f2d-1633">Je inicializována s kopii hodnoty argumentů (pokud existuje) a byla předána hodnota instanci členské funkce.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1633">It is initialized with a copy of the argument values (if any) and instance value passed to the function member.</span></span>

<span data-ttu-id="56f2d-1634">Vyčíslitelný objekt, je obvykle instance generované kompilátorem vyčíslitelné třídu, která zapouzdřuje kód v bloku iterátoru a implementuje vyčíslitelná rozhraní, ale jiné metody implementace jsou možné.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1634">An enumerable object is typically an instance of a compiler-generated enumerable class that encapsulates the code in the iterator block and implements the enumerable interfaces, but other methods of implementation are possible.</span></span> <span data-ttu-id="56f2d-1635">Pokud vyčíslitelné třídy je generovaný kompilátorem, se vnoří dané třídy, přímo nebo nepřímo, třídy obsahující členské funkce bude mít přístupnost private a bude mít název vyhrazený pro použití kompilátoru ([identifikátory ](lexical-structure.md#identifiers)).</span><span class="sxs-lookup"><span data-stu-id="56f2d-1635">If an enumerable class is generated by the compiler, that class will be nested, directly or indirectly, in the class containing the function member, it will have private accessibility, and it will have a name reserved for compiler use ([Identifiers](lexical-structure.md#identifiers)).</span></span>

<span data-ttu-id="56f2d-1636">Vyčíslitelný objekt může implementovat více rozhraní než uvedené výše.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1636">An enumerable object may implement more interfaces than those specified above.</span></span> <span data-ttu-id="56f2d-1637">Zejména může také implementovat vyčíslitelný objekt `IEnumerator` a `IEnumerator<T>`, díky tomu se může sloužit jako Výčtový objekt a enumerátor.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1637">In particular, an enumerable object may also implement `IEnumerator` and `IEnumerator<T>`, enabling it to serve as both an enumerable and an enumerator.</span></span> <span data-ttu-id="56f2d-1638">V tomto typu implementace nevyskytne první vyčíslitelný objekt `GetEnumerator` je vyvolána metoda, je vrácena sám vyčíslitelným objektem.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1638">In that type of implementation, the first time an enumerable object's `GetEnumerator` method is invoked, the enumerable object itself is returned.</span></span> <span data-ttu-id="56f2d-1639">Následné volání vyčíslitelný objekt `GetEnumerator`, pokud existuje, vrátí kopii vyčíslitelným objektem.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1639">Subsequent invocations of the enumerable object's `GetEnumerator`, if any, return a copy of the enumerable object.</span></span> <span data-ttu-id="56f2d-1640">Díky tomu se každá vrácená čítače mají svůj vlastní stav a změny v jedné enumerátor nebude mít vliv na jiné.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1640">Thus, each returned enumerator has its own state and changes in one enumerator will not affect another.</span></span>

#### <a name="the-getenumerator-method"></a><span data-ttu-id="56f2d-1641">GetEnumerator – metoda</span><span class="sxs-lookup"><span data-stu-id="56f2d-1641">The GetEnumerator method</span></span>

<span data-ttu-id="56f2d-1642">Vyčíslitelný objekt poskytuje implementaci `GetEnumerator` metody `IEnumerable` a `IEnumerable<T>` rozhraní.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1642">An enumerable object provides an implementation of the `GetEnumerator` methods of the `IEnumerable` and `IEnumerable<T>` interfaces.</span></span> <span data-ttu-id="56f2d-1643">Dva `GetEnumerator` metody sdílejí běžnou implementaci, která získá a vrátí objekt k dispozici enumerátoru.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1643">The two `GetEnumerator` methods share a common implementation that acquires and returns an available enumerator object.</span></span> <span data-ttu-id="56f2d-1644">Objekt enumerator inicializovat pomocí hodnoty argumentů a instanci hodnotu uložily v době, kdy vyčíslitelný objekt byl inicializován, ale jinak funkce objekt enumerátoru, jak je popsáno v [enumerátor objekty](classes.md#enumerator-objects).</span><span class="sxs-lookup"><span data-stu-id="56f2d-1644">The enumerator object is initialized with the argument values and instance value saved when the enumerable object was initialized, but otherwise the enumerator object functions as described in [Enumerator objects](classes.md#enumerator-objects).</span></span>

### <a name="implementation-example"></a><span data-ttu-id="56f2d-1645">Příklad implementace</span><span class="sxs-lookup"><span data-stu-id="56f2d-1645">Implementation example</span></span>

<span data-ttu-id="56f2d-1646">Tato část popisuje možnou implementaci iterátorů, co se týče standardní konstrukce jazyka C#.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1646">This section describes a possible implementation of iterators in terms of standard C# constructs.</span></span> <span data-ttu-id="56f2d-1647">Implementace je zde popsáno, je založená na stejné zásady použít kompilátor jazyka Microsoft C#, ale v žádném smyslu mandátem implementace nebo pouze jeden možný.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1647">The implementation described here is based on the same principles used by the Microsoft C# compiler, but it is by no means a mandated implementation or the only one possible.</span></span>

<span data-ttu-id="56f2d-1648">Následující `Stack<T>` implementuje třída jeho `GetEnumerator` metodu pomocí iterátoru.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1648">The following `Stack<T>` class implements its `GetEnumerator` method using an iterator.</span></span> <span data-ttu-id="56f2d-1649">Iterátor uvádí prvky zásobníku v horní části pořadí.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1649">The iterator enumerates the elements of the stack in top to bottom order.</span></span>

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

<span data-ttu-id="56f2d-1650">`GetEnumerator` Metody lze přeložit do instance třídy generované kompilátorem enumerátor, který zapouzdřuje kód v bloku iterátoru, jak je znázorněno v následujícím.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1650">The `GetEnumerator` method can be translated into an instantiation of a compiler-generated enumerator class that encapsulates the code in the iterator block, as shown in the following.</span></span>

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

<span data-ttu-id="56f2d-1651">V předchozím překladu kód v bloku iterátoru převedena na stavu počítače a umístí do `MoveNext` metoda třídy výčtu.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1651">In the preceding translation, the code in the iterator block is turned into a state machine and placed in the `MoveNext` method of the enumerator class.</span></span> <span data-ttu-id="56f2d-1652">Kromě toho místní proměnná `i` bude převedena na pole v objekt enumerator, takže můžete dál existovat mezi voláními `MoveNext`.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1652">Furthermore, the local variable `i` is turned into a field in the enumerator object so it can continue to exist across invocations of `MoveNext`.</span></span>

<span data-ttu-id="56f2d-1653">Následující příklad vytiskne jednoduchou tabulku násobení celých čísel 1 až 10.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1653">The following example prints a simple multiplication table of the integers 1 through 10.</span></span> <span data-ttu-id="56f2d-1654">`FromTo` Metoda v tomto příkladu vrácen vyčíslitelný objekt a je implementována pomocí iterátoru.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1654">The `FromTo` method in the example returns an enumerable object and is implemented using an iterator.</span></span>

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

<span data-ttu-id="56f2d-1655">`FromTo` Metody lze přeložit do instance třídy generované kompilátorem vyčíslitelné, který zapouzdřuje kód v bloku iterátoru, jak je znázorněno v následujícím.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1655">The `FromTo` method can be translated into an instantiation of a compiler-generated enumerable class that encapsulates the code in the iterator block, as shown in the following.</span></span>

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

<span data-ttu-id="56f2d-1656">Vyčíslitelná třída implementuje vyčíslitelná rozhraní a rozhraní čítače, díky tomu se může sloužit jako Výčtový objekt a enumerátor.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1656">The enumerable class implements both the enumerable interfaces and the enumerator interfaces, enabling it to serve as both an enumerable and an enumerator.</span></span> <span data-ttu-id="56f2d-1657">Prvním `GetEnumerator` je vyvolána metoda, je vrácena sám vyčíslitelným objektem.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1657">The first time the `GetEnumerator` method is invoked, the enumerable object itself is returned.</span></span> <span data-ttu-id="56f2d-1658">Následné volání vyčíslitelný objekt `GetEnumerator`, pokud existuje, vrátí kopii vyčíslitelným objektem.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1658">Subsequent invocations of the enumerable object's `GetEnumerator`, if any, return a copy of the enumerable object.</span></span> <span data-ttu-id="56f2d-1659">Díky tomu se každá vrácená čítače mají svůj vlastní stav a změny v jedné enumerátor nebude mít vliv na jiné.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1659">Thus, each returned enumerator has its own state and changes in one enumerator will not affect another.</span></span> <span data-ttu-id="56f2d-1660">`Interlocked.CompareExchange` Metoda se používá k zajištění operace bezpečné pro vlákna.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1660">The `Interlocked.CompareExchange` method is used to ensure thread-safe operation.</span></span>

<span data-ttu-id="56f2d-1661">`from` a `to` parametry jsou převedena na pole v třídě vyčíslitelný.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1661">The `from` and `to` parameters are turned into fields in the enumerable class.</span></span> <span data-ttu-id="56f2d-1662">Protože `from` se mění v bloku iterátoru, zobrazí se další `__from` pole se používá k uchování na počáteční hodnoty `from` v každé enumerátor.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1662">Because `from` is modified in the iterator block, an additional `__from` field is introduced to hold the initial value given to `from` in each enumerator.</span></span>

<span data-ttu-id="56f2d-1663">`MoveNext` Vyvolá metoda výjimku `InvalidOperationException` Pokud je volána, když `__state` je `0`.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1663">The `MoveNext` method throws an `InvalidOperationException` if it is called when `__state` is `0`.</span></span> <span data-ttu-id="56f2d-1664">Je to ochrana proti použití vyčíslitelný objekt jako objekt enumerátoru bez první volání `GetEnumerator`.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1664">This protects against use of the enumerable object as an enumerator object without first calling `GetEnumerator`.</span></span>

<span data-ttu-id="56f2d-1665">Následující příklad ukazuje jednoduchý stromu tříd.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1665">The following example shows a simple tree class.</span></span> <span data-ttu-id="56f2d-1666">`Tree<T>` Implementuje třída jeho `GetEnumerator` metodu pomocí iterátoru.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1666">The `Tree<T>` class implements its `GetEnumerator` method using an iterator.</span></span> <span data-ttu-id="56f2d-1667">Iterátor vytvoří výčet elementy stromu v pořadí infix.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1667">The iterator enumerates the elements of the tree in infix order.</span></span>

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

<span data-ttu-id="56f2d-1668">`GetEnumerator` Metody lze přeložit do instance třídy generované kompilátorem enumerátor, který zapouzdřuje kód v bloku iterátoru, jak je znázorněno v následujícím.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1668">The `GetEnumerator` method can be translated into an instantiation of a compiler-generated enumerator class that encapsulates the code in the iterator block, as shown in the following.</span></span>

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

<span data-ttu-id="56f2d-1669">Dočasné objekty generovaný kompilátorem, používané `foreach` příkazy jsou zrušeno vs. do `__left` a `__right` polí objekt enumerátoru.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1669">The compiler generated temporaries used in the `foreach` statements are lifted into the `__left` and `__right` fields of the enumerator object.</span></span> <span data-ttu-id="56f2d-1670">`__state` Pole objektu enumerátor se pečlivě aktualizuje tak, aby správné `Dispose()` metoda volána správně, pokud je vyvolána výjimka.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1670">The `__state` field of the enumerator object is carefully updated so that the correct `Dispose()` method will be called correctly if an exception is thrown.</span></span> <span data-ttu-id="56f2d-1671">Všimněte si, že není možné psát přeložený kód v jednoduchém `foreach` příkazy.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1671">Note that it is not possible to write the translated code with simple `foreach` statements.</span></span>

## <a name="async-functions"></a><span data-ttu-id="56f2d-1672">Asynchronní funkce</span><span class="sxs-lookup"><span data-stu-id="56f2d-1672">Async functions</span></span>

<span data-ttu-id="56f2d-1673">Metody ([metody](classes.md#methods)) nebo anonymní funkce ([výrazy anonymní funkce](expressions.md#anonymous-function-expressions)) s `async` Modifikátor je volána ***asynchronní funkci***.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1673">A method ([Methods](classes.md#methods)) or anonymous function ([Anonymous function expressions](expressions.md#anonymous-function-expressions)) with the `async` modifier is called an ***async function***.</span></span> <span data-ttu-id="56f2d-1674">Obecně platí termín ***asynchronní*** se používá k popisu jakýkoli druh funkce, která má `async` modifikátor.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1674">In general, the term ***async*** is used to describe any kind of function that has the `async` modifier.</span></span>

<span data-ttu-id="56f2d-1675">Chyba kompilace pro asynchronní funkce lze zadat seznam formálních parametrů je `ref` nebo `out` parametry.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1675">It is a compile-time error for the formal parameter list of an async function to specify any `ref` or `out` parameters.</span></span>

<span data-ttu-id="56f2d-1676">*Typ* asynchronní metody musí být buď `void` nebo ***typ úkolu***.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1676">The *return_type* of an async method must be either `void` or a ***task type***.</span></span> <span data-ttu-id="56f2d-1677">Typy úloh jsou `System.Threading.Tasks.Task` a typy vytvářejí na základě `System.Threading.Tasks.Task<T>`.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1677">The task types are `System.Threading.Tasks.Task` and types constructed from `System.Threading.Tasks.Task<T>`.</span></span> <span data-ttu-id="56f2d-1678">Pro účely jako stručný výtah v této kapitole tyto typy jsou odkazovány jako `Task` a `Task<T>`v uvedeném pořadí.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1678">For the sake of brevity, in this chapter these types are referenced as `Task` and `Task<T>`, respectively.</span></span> <span data-ttu-id="56f2d-1679">Asynchronní metody vracející typ úloh se říká, že vracejí task.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1679">An async method returning a task type is said to be task-returning.</span></span>

<span data-ttu-id="56f2d-1680">Přesné definice typů úloh je definován implementací, ale z hlediska tohoto jazyka je typ úkolu v jednom ze stavů nekompletní, bylo úspěšné nebo došlo k chybě.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1680">The exact definition of the task types is implementation defined, but from the language's point of view a task type is in one of the states incomplete, succeeded or faulted.</span></span> <span data-ttu-id="56f2d-1681">Úlohu chybnou zaznamenává relevantní výjimka.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1681">A faulted task records a pertinent exception.</span></span> <span data-ttu-id="56f2d-1682">Úspěšném `Task<T>` zaznamenává výsledek typu `T`.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1682">A succeeded `Task<T>` records a result of type `T`.</span></span> <span data-ttu-id="56f2d-1683">Typy úloh jsou očekávatelný a může proto být operandy výrazech await ([výrazech Await](expressions.md#await-expressions)).</span><span class="sxs-lookup"><span data-stu-id="56f2d-1683">Task types are awaitable, and can therefore be the operands of await expressions ([Await expressions](expressions.md#await-expressions)).</span></span>

<span data-ttu-id="56f2d-1684">Asynchronní vyvolání funkce má schopnost pozastavit vyhodnocení prostřednictvím výrazech await ([výrazech Await](expressions.md#await-expressions)) v jeho textu.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1684">An async function invocation has the ability to suspend evaluation by means of await expressions ([Await expressions](expressions.md#await-expressions)) in its body.</span></span> <span data-ttu-id="56f2d-1685">Vyhodnocení může později obnovit místě pozastavení operátor await výraz prostřednictvím ***delegát pokračování***.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1685">Evaluation may later be resumed at the point of the suspending await expression by means of a ***resumption delegate***.</span></span> <span data-ttu-id="56f2d-1686">Obnovení delegáta je typu `System.Action`, a když je vyvolán, vyhodnocení volání asynchronní funkce bude pokračovat z výrazu await, kde přestalo.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1686">The resumption delegate is of type `System.Action`, and when it is invoked, evaluation of the async function invocation will resume from the await expression where it left off.</span></span> <span data-ttu-id="56f2d-1687">***Aktuální volající*** asynchronní funkce volání je původní volajícího, pokud volání funkce nikdy byl pozastaven nebo nejnovější volající delegát pokračování jinak.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1687">The ***current caller*** of an async function invocation is the original caller if the function invocation has never been suspended, or the most recent caller of the resumption delegate otherwise.</span></span>

### <a name="evaluation-of-a-task-returning-async-function"></a><span data-ttu-id="56f2d-1688">Hodnocení produktu asynchronní funkci vracející úlohy</span><span class="sxs-lookup"><span data-stu-id="56f2d-1688">Evaluation of a task-returning async function</span></span>

<span data-ttu-id="56f2d-1689">Volání asynchronní funkci vracející úlohy způsobí, že instance typu vrácené úlohy budou vygenerovány.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1689">Invocation of a task-returning async function causes an instance of the returned task type to be generated.</span></span> <span data-ttu-id="56f2d-1690">Tento postup se nazývá ***návratová hodnota task*** asynchronní funkce.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1690">This is called the ***return task*** of the async function.</span></span> <span data-ttu-id="56f2d-1691">Úloha je zpočátku nekompletnímu stavu.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1691">The task is initially in an incomplete state.</span></span>

<span data-ttu-id="56f2d-1692">Tělo funkce asynchronní je pak vyhodnocen, dokud je buď pozastaveno (díky tomu, že výraz await) nebo ukončí, ve kterém ovládací prvek bod se vrátí volajícímu spolu návratová hodnota task.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1692">The async function body is then evaluated until it is either suspended (by reaching an await expression) or terminates, at which point control is returned to the caller, along with the return task.</span></span>

<span data-ttu-id="56f2d-1693">Při ukončení těla asynchronní funkce, vrácené úlohy je vyřadí z nekompletnímu stavu:</span><span class="sxs-lookup"><span data-stu-id="56f2d-1693">When the body of the async function terminates, the return task is moved out of the incomplete state:</span></span>

*  <span data-ttu-id="56f2d-1694">Pokud tělo funkce končí v důsledku dosáhnout návratový příkaz nebo konec textu, libovolná hodnota výsledku je zaznamenán v vrácené úlohou, které přejde do stavu úspěšné.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1694">If the function body terminates as the result of reaching a return statement or the end of the body, any result value is recorded in the return task, which is put into a succeeded state.</span></span>
*  <span data-ttu-id="56f2d-1695">Pokud tělo funkce končí v důsledku vydá nezachycenou výjimku ([příkazu throw](statements.md#the-throw-statement)) výjimky je zaznamenán v vrácené úlohou, které je umístěn v chybovém stavu.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1695">If the function body terminates as the result of an uncaught exception ([The throw statement](statements.md#the-throw-statement)) the exception is recorded in the return task which is put into a faulted state.</span></span>

### <a name="evaluation-of-a-void-returning-async-function"></a><span data-ttu-id="56f2d-1696">Hodnocení produktu asynchronní funkci vracející typ void</span><span class="sxs-lookup"><span data-stu-id="56f2d-1696">Evaluation of a void-returning async function</span></span>

<span data-ttu-id="56f2d-1697">Pokud je návratový typ asynchronní funkce `void`, vyhodnocení se liší od výše uvedených následujícím způsobem: protože nevrátí žádná úloha, funkci místo toho komunikuje dokončení a výjimky pro aktuální vlákno ***synchronizace kontext***.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1697">If the return type of the async function is `void`, evaluation differs from the above in the following way: Because no task is returned, the function instead communicates completion and exceptions to the current thread's ***synchronization context***.</span></span> <span data-ttu-id="56f2d-1698">Přesné definici kontext synchronizace je závislý na implementaci, ale je reprezentace "kde" spuštění aktuálního vlákna.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1698">The exact definition of synchronization context is implementation-dependent, but is a representation of "where" the current thread is running.</span></span> <span data-ttu-id="56f2d-1699">Kontext synchronizace zasláno oznámení, když vyhodnocení asynchronní funkci vracející hodnotu void začíná, dokončíte úspěšně nebo způsobí, že vydá nezachycenou výjimku, která je vyvolána.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1699">The synchronization context is notified when evaluation of a void-returning async function commences, completes successfully, or causes an uncaught exception to be thrown.</span></span>

<span data-ttu-id="56f2d-1700">To umožňuje sledovat, kolik asynchronní funkce vracející hodnotu void běží pod ním a rozhodování o způsobu šíření výjimky z nich kontextu.</span><span class="sxs-lookup"><span data-stu-id="56f2d-1700">This allows the context to keep track of how many void-returning async functions are running under it, and to decide how to propagate exceptions coming out of them.</span></span>
