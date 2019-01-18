---
ms.openlocfilehash: 0a09585f4f885647230354c66a2449abb7ef1f44
ms.sourcegitcommit: 3fc033b6e98ed7ecdf46a85c79b00a3a3ddcf963
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/18/2019
ms.locfileid: "47229900"
---
# <a name="interfaces"></a><span data-ttu-id="d0561-101">Rozhraní</span><span class="sxs-lookup"><span data-stu-id="d0561-101">Interfaces</span></span>

<span data-ttu-id="d0561-102">Rozhraní definuje kontrakt.</span><span class="sxs-lookup"><span data-stu-id="d0561-102">An interface defines a contract.</span></span> <span data-ttu-id="d0561-103">Třída nebo struktura, která implementuje rozhraní musí dodržovat jeho kontrakt.</span><span class="sxs-lookup"><span data-stu-id="d0561-103">A class or struct that implements an interface must adhere to its contract.</span></span> <span data-ttu-id="d0561-104">Rozhraní může dědit z více základních rozhraní a třídy nebo struktury může implementovat více rozhraní.</span><span class="sxs-lookup"><span data-stu-id="d0561-104">An interface may inherit from multiple base interfaces, and a class or struct may implement multiple interfaces.</span></span>

<span data-ttu-id="d0561-105">Rozhraní může obsahovat metody, vlastnosti, události a indexery.</span><span class="sxs-lookup"><span data-stu-id="d0561-105">Interfaces can contain methods, properties, events, and indexers.</span></span> <span data-ttu-id="d0561-106">Rozhraní samotné neposkytuje implementace jeho členů, které definuje.</span><span class="sxs-lookup"><span data-stu-id="d0561-106">The interface itself does not provide implementations for the members that it defines.</span></span> <span data-ttu-id="d0561-107">Rozhraní určuje pouze členy, které je třeba dodat ze třídy nebo struktury, které implementují rozhraní.</span><span class="sxs-lookup"><span data-stu-id="d0561-107">The interface merely specifies the members that must be supplied by classes or structs that implement the interface.</span></span>

## <a name="interface-declarations"></a><span data-ttu-id="d0561-108">Deklarace rozhraní.</span><span class="sxs-lookup"><span data-stu-id="d0561-108">Interface declarations</span></span>

<span data-ttu-id="d0561-109">*Interface_declaration* je *type_declaration* ([typ deklarace](namespaces.md#type-declarations)), který deklaruje nový typ rozhraní.</span><span class="sxs-lookup"><span data-stu-id="d0561-109">An *interface_declaration* is a *type_declaration* ([Type declarations](namespaces.md#type-declarations)) that declares a new interface type.</span></span>

```antlr
interface_declaration
    : attributes? interface_modifier* 'partial'? 'interface'
      identifier variant_type_parameter_list? interface_base?
      type_parameter_constraints_clause* interface_body ';'?
    ;
```

<span data-ttu-id="d0561-110">*Interface_declaration* se skládá z volitelné sadu *atributy* ([atributy](attributes.md)) následovaný volitelná sada *interface_modifier*s ([rozhraní modifikátory](interfaces.md#interface-modifiers)), následovaným volitelnou `partial` modifikátor, za nímž následuje klíčové slovo `interface` a *identifikátor* , názvy rozhraní, za nímž následuje volitelným *variant_type_parameter_list* specifikace ([seznamy parametru typu Variant](interfaces.md#variant-type-parameter-lists)), následovaným volitelnou *interface_base* specifikace ([základní rozhraní](interfaces.md#base-interfaces)), následovaným volitelnou *type_parameter_constraints_clause*s specifikace ([omezení parametru typu](classes.md#type-parameter-constraints)) , za nímž následuje *interface_body* ([rozhraní text](interfaces.md#interface-body)), volitelně za nímž následuje středníkem.</span><span class="sxs-lookup"><span data-stu-id="d0561-110">An *interface_declaration* consists of an optional set of *attributes* ([Attributes](attributes.md)), followed by an optional set of *interface_modifier*s ([Interface modifiers](interfaces.md#interface-modifiers)), followed by an optional `partial` modifier, followed by the keyword `interface` and an *identifier* that names the interface, followed by an optional *variant_type_parameter_list* specification ([Variant type parameter lists](interfaces.md#variant-type-parameter-lists)), followed by an optional *interface_base* specification ([Base interfaces](interfaces.md#base-interfaces)), followed by an optional *type_parameter_constraints_clause*s specification ([Type parameter constraints](classes.md#type-parameter-constraints)), followed by an *interface_body* ([Interface body](interfaces.md#interface-body)), optionally followed by a semicolon.</span></span>

### <a name="interface-modifiers"></a><span data-ttu-id="d0561-111">Modifikátory rozhraní</span><span class="sxs-lookup"><span data-stu-id="d0561-111">Interface modifiers</span></span>

<span data-ttu-id="d0561-112">*Interface_declaration* může volitelně zahrnovat posloupnost modifikátory rozhraní:</span><span class="sxs-lookup"><span data-stu-id="d0561-112">An *interface_declaration* may optionally include a sequence of interface modifiers:</span></span>

```antlr
interface_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    | interface_modifier_unsafe
    ;
```

<span data-ttu-id="d0561-113">Je chyba kompilace pro stejný modifikátor se zobrazí více než jednou v deklaraci rozhraní.</span><span class="sxs-lookup"><span data-stu-id="d0561-113">It is a compile-time error for the same modifier to appear multiple times in an interface declaration.</span></span>

<span data-ttu-id="d0561-114">`new` Modifikátor je povolené jenom u rozhraní definované v rámci třídy.</span><span class="sxs-lookup"><span data-stu-id="d0561-114">The `new` modifier is only permitted on interfaces defined within a class.</span></span> <span data-ttu-id="d0561-115">Určuje, že rozhraní skrývá zděděného člena se stejným názvem, jak je popsáno v [new – modifikátor](classes.md#the-new-modifier).</span><span class="sxs-lookup"><span data-stu-id="d0561-115">It specifies that the interface hides an inherited member by the same name, as described in [The new modifier](classes.md#the-new-modifier).</span></span>

<span data-ttu-id="d0561-116">`public`, `protected`, `internal`, A `private` řídit modifikátory přístupnosti rozhraní.</span><span class="sxs-lookup"><span data-stu-id="d0561-116">The `public`, `protected`, `internal`, and `private` modifiers control the accessibility of the interface.</span></span> <span data-ttu-id="d0561-117">V závislosti na kontextu, ve kterém dochází k deklaraci rozhraní, může být povoleno pouze některé tyto modifikátory ([deklarovaná přístupnost](basic-concepts.md#declared-accessibility)).</span><span class="sxs-lookup"><span data-stu-id="d0561-117">Depending on the context in which the interface declaration occurs, only some of these modifiers may be permitted ([Declared accessibility](basic-concepts.md#declared-accessibility)).</span></span>

### <a name="partial-modifier"></a><span data-ttu-id="d0561-118">Částečný modifikátor</span><span class="sxs-lookup"><span data-stu-id="d0561-118">Partial modifier</span></span>

<span data-ttu-id="d0561-119">`partial` Modifikátor znamená, že to *interface_declaration* je částečný typ deklarace.</span><span class="sxs-lookup"><span data-stu-id="d0561-119">The `partial` modifier indicates that this *interface_declaration* is a partial type declaration.</span></span> <span data-ttu-id="d0561-120">Více deklarací částečné rozhraní se stejným názvem v rámci nadřazeného oboru názvů nebo typ deklarace se dá formuláře jedno rozhraní prohlášení, dle pravidel uvedených v [částečné typy](classes.md#partial-types).</span><span class="sxs-lookup"><span data-stu-id="d0561-120">Multiple partial interface declarations with the same name within an enclosing namespace or type declaration combine to form one interface declaration, following the rules specified in [Partial types](classes.md#partial-types).</span></span>

### <a name="variant-type-parameter-lists"></a><span data-ttu-id="d0561-121">Seznamy parametrů typ Variant</span><span class="sxs-lookup"><span data-stu-id="d0561-121">Variant type parameter lists</span></span>

<span data-ttu-id="d0561-122">Seznamy parametrů variantního typu může dojít pouze na typy rozhraní a delegátů.</span><span class="sxs-lookup"><span data-stu-id="d0561-122">Variant type parameter lists can only occur on interface and delegate types.</span></span> <span data-ttu-id="d0561-123">Rozdíl od běžných *type_parameter_list*s je volitelný *variance_annotation* na každý z parametrů typu.</span><span class="sxs-lookup"><span data-stu-id="d0561-123">The difference from ordinary *type_parameter_list*s is the optional *variance_annotation* on each type parameter.</span></span>

```antlr
variant_type_parameter_list
    : '<' variant_type_parameters '>'
    ;

variant_type_parameters
    : attributes? variance_annotation? type_parameter
    | variant_type_parameters ',' attributes? variance_annotation? type_parameter
    ;

variance_annotation
    : 'in'
    | 'out'
    ;
```

<span data-ttu-id="d0561-124">Pokud je anotaci variance `out`, typ parametru je označen jako ***kovariantní***.</span><span class="sxs-lookup"><span data-stu-id="d0561-124">If the variance annotation is `out`, the type parameter is said to be ***covariant***.</span></span> <span data-ttu-id="d0561-125">Pokud je anotaci variance `in`, typ parametru je označen jako ***kontravariantní***.</span><span class="sxs-lookup"><span data-stu-id="d0561-125">If the variance annotation is `in`, the type parameter is said to be ***contravariant***.</span></span> <span data-ttu-id="d0561-126">Pokud neexistuje žádný anotaci variance, typ parametru je označen jako ***invariantní***.</span><span class="sxs-lookup"><span data-stu-id="d0561-126">If there is no variance annotation, the type parameter is said to be ***invariant***.</span></span>

<span data-ttu-id="d0561-127">V příkladu</span><span class="sxs-lookup"><span data-stu-id="d0561-127">In the example</span></span>
```csharp
interface C<out X, in Y, Z> 
{
  X M(Y y);
  Z P { get; set; }
}
```
<span data-ttu-id="d0561-128">`X` je kovariantní `Y` je kontravariantní a `Z` je neutrální.</span><span class="sxs-lookup"><span data-stu-id="d0561-128">`X` is covariant, `Y` is contravariant and `Z` is invariant.</span></span>

#### <a name="variance-safety"></a><span data-ttu-id="d0561-129">Bezpečný přístup z více odchylek</span><span class="sxs-lookup"><span data-stu-id="d0561-129">Variance safety</span></span>

<span data-ttu-id="d0561-130">Výskyt anotace variance v seznamu parametrů typu typu omezuje na místech, kde může dojít k typům v deklaraci typu.</span><span class="sxs-lookup"><span data-stu-id="d0561-130">The occurrence of variance annotations in the type parameter list of a type restricts the places where types can occur within the type declaration.</span></span>

<span data-ttu-id="d0561-131">Typ `T` je ***výstup unsafe*** pokud obsahuje jeden z následujících akcí:</span><span class="sxs-lookup"><span data-stu-id="d0561-131">A type `T` is ***output-unsafe*** if one of the following holds:</span></span>

*  <span data-ttu-id="d0561-132">`T` je parametr kontravariantního typu</span><span class="sxs-lookup"><span data-stu-id="d0561-132">`T` is a contravariant type parameter</span></span>
*  <span data-ttu-id="d0561-133">`T` je typ pole s typem výstupu unsafe – element</span><span class="sxs-lookup"><span data-stu-id="d0561-133">`T` is an array type with an output-unsafe element type</span></span>
*  <span data-ttu-id="d0561-134">`T` je typ rozhraní nebo delegát `S<A1,...,Ak>` vytvořený z obecného typu `S<X1,...,Xk>` where pro nejméně jednu `Ai` obsahuje jeden z následujících akcí:</span><span class="sxs-lookup"><span data-stu-id="d0561-134">`T` is an interface or delegate type `S<A1,...,Ak>` constructed from a generic type `S<X1,...,Xk>` where for at least one `Ai` one of the following holds:</span></span>
   * <span data-ttu-id="d0561-135">`Xi` je kovariantní nebo neutrální a `Ai` není bezpečné pro výstup.</span><span class="sxs-lookup"><span data-stu-id="d0561-135">`Xi` is covariant or invariant and `Ai` is output-unsafe.</span></span>
   * <span data-ttu-id="d0561-136">`Xi` je kontravariantní nebo invariantní a `Ai` je bezpečná pro vstup.</span><span class="sxs-lookup"><span data-stu-id="d0561-136">`Xi` is contravariant or invariant and `Ai` is input-safe.</span></span>
   
<span data-ttu-id="d0561-137">Typ `T` je ***vstup nebezpečné*** pokud obsahuje jeden z následujících akcí:</span><span class="sxs-lookup"><span data-stu-id="d0561-137">A type `T` is ***input-unsafe*** if one of the following holds:</span></span>

*  <span data-ttu-id="d0561-138">`T` je parametr kovariantního typu</span><span class="sxs-lookup"><span data-stu-id="d0561-138">`T` is a covariant type parameter</span></span>
*  <span data-ttu-id="d0561-139">`T` je typ pole s typem vstupu unsafe – element</span><span class="sxs-lookup"><span data-stu-id="d0561-139">`T` is an array type with an input-unsafe element type</span></span>
*  <span data-ttu-id="d0561-140">`T` je typ rozhraní nebo delegát `S<A1,...,Ak>` vytvořený z obecného typu `S<X1,...,Xk>` where pro nejméně jednu `Ai` obsahuje jeden z následujících akcí:</span><span class="sxs-lookup"><span data-stu-id="d0561-140">`T` is an interface or delegate type `S<A1,...,Ak>` constructed from a generic type `S<X1,...,Xk>` where for at least one `Ai` one of the following holds:</span></span>
   * <span data-ttu-id="d0561-141">`Xi` je kovariantní nebo neutrální a `Ai` není bezpečné pro vstup.</span><span class="sxs-lookup"><span data-stu-id="d0561-141">`Xi` is covariant or invariant and `Ai` is input-unsafe.</span></span>
   * <span data-ttu-id="d0561-142">`Xi` je kontravariantní nebo invariantní a `Ai` není bezpečné pro výstup.</span><span class="sxs-lookup"><span data-stu-id="d0561-142">`Xi` is contravariant or invariant and `Ai` is output-unsafe.</span></span>

<span data-ttu-id="d0561-143">Intuitivně nebezpečné výstupní typ je zakázáno v poloze výstup a vstup nezabezpečený typ je zakázáno na vstupní pozici.</span><span class="sxs-lookup"><span data-stu-id="d0561-143">Intuitively, an output-unsafe type is prohibited in an output position, and an input-unsafe type is prohibited in an input position.</span></span>

<span data-ttu-id="d0561-144">Typ je ***bezpečný výstup*** Pokud není výstupní nebezpečné, a ***vstup typově bezpečný*** nesplnění vstup unsafe.</span><span class="sxs-lookup"><span data-stu-id="d0561-144">A type is ***output-safe*** if it is not output-unsafe, and ***input-safe*** if it is not input-unsafe.</span></span>

#### <a name="variance-conversion"></a><span data-ttu-id="d0561-145">Převod odchylka</span><span class="sxs-lookup"><span data-stu-id="d0561-145">Variance conversion</span></span>

<span data-ttu-id="d0561-146">Účelem anotace variance je zadání mírnější (ale pořád typově bezpečný) převody na typy rozhraní a delegátů.</span><span class="sxs-lookup"><span data-stu-id="d0561-146">The purpose of variance annotations is to provide for more lenient (but still type safe) conversions to interface and delegate types.</span></span> <span data-ttu-id="d0561-147">Tím účelem definice implicitní ([implicitních převodů](conversions.md#implicit-conversions)) a explicitní převody ([explicitních převodů](conversions.md#explicit-conversions)) ujistěte se, použijte pojem variance převoditelnosti, která je definovaná následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="d0561-147">To this end the definitions of implicit ([Implicit conversions](conversions.md#implicit-conversions)) and explicit conversions ([Explicit conversions](conversions.md#explicit-conversions)) make use of the notion of variance-convertibility, which is defined as follows:</span></span>

<span data-ttu-id="d0561-148">Typ `T<A1,...,An>` je odchylka převoditelné na typ `T<B1,...,Bn>` Pokud `T` rozhraní nebo typ delegáta deklarovat s parametry variantního typu `T<X1,...,Xn>`a pro každý parametr variantního typu `Xi` jednu z následujících obsahuje:</span><span class="sxs-lookup"><span data-stu-id="d0561-148">A type `T<A1,...,An>` is variance-convertible to a type `T<B1,...,Bn>` if `T` is either an interface or a delegate type declared with the variant type parameters `T<X1,...,Xn>`, and for each variant type parameter `Xi` one of the following holds:</span></span>

*  <span data-ttu-id="d0561-149">`Xi` je kovariantní a implicitní převod odkazu nebo identity `Ai` do `Bi`</span><span class="sxs-lookup"><span data-stu-id="d0561-149">`Xi` is covariant and an implicit reference or identity conversion exists from `Ai` to `Bi`</span></span>
*  <span data-ttu-id="d0561-150">`Xi` je kontravariantní a implicitní odkaz nebo převod identity `Bi` do `Ai`</span><span class="sxs-lookup"><span data-stu-id="d0561-150">`Xi` is contravariant and an implicit reference or identity conversion exists from `Bi` to `Ai`</span></span>
*  <span data-ttu-id="d0561-151">`Xi` invariantní a identitu, která existuje převod z `Ai` do `Bi`</span><span class="sxs-lookup"><span data-stu-id="d0561-151">`Xi` is invariant and an identity conversion exists from `Ai` to `Bi`</span></span>

### <a name="base-interfaces"></a><span data-ttu-id="d0561-152">Základní rozhraní</span><span class="sxs-lookup"><span data-stu-id="d0561-152">Base interfaces</span></span>

<span data-ttu-id="d0561-153">Rozhraní může dědit z nuly nebo více typů rozhraní, které se volají ***explicitní základních rozhraní*** rozhraní.</span><span class="sxs-lookup"><span data-stu-id="d0561-153">An interface can inherit from zero or more interface types, which are called the ***explicit base interfaces*** of the interface.</span></span> <span data-ttu-id="d0561-154">Když rozhraní obsahuje jedno nebo více explicitní základní rozhraní, pak v deklaraci rozhraní, identifikátor rozhraní následuje dvojtečka a čárkou oddělený seznam typů základní rozhraní.</span><span class="sxs-lookup"><span data-stu-id="d0561-154">When an interface has one or more explicit base interfaces, then in the declaration of that interface, the interface identifier is followed by a colon and a comma separated list of base interface types.</span></span>

```antlr
interface_base
    : ':' interface_type_list
    ;
```

<span data-ttu-id="d0561-155">Pro typ vybudované rozhraní explicitní základních rozhraní jsou tvořeny tak explicitní základní rozhraní deklarací na deklarace obecného typu a nahrazování pro každou *type_parameter* v základní rozhraní deklarace, odpovídající *type_argument* sestaveného typu.</span><span class="sxs-lookup"><span data-stu-id="d0561-155">For a constructed interface type, the explicit base interfaces are formed by taking the explicit base interface declarations on the generic type declaration, and substituting, for each *type_parameter* in the base interface declaration, the corresponding *type_argument* of the constructed type.</span></span>

<span data-ttu-id="d0561-156">Explicitní základní rozhraní rozhraní musí být přinejmenším stejně dostupná jako rozhraní samotného ([usnadnění omezení](basic-concepts.md#accessibility-constraints)).</span><span class="sxs-lookup"><span data-stu-id="d0561-156">The explicit base interfaces of an interface must be at least as accessible as the interface itself ([Accessibility constraints](basic-concepts.md#accessibility-constraints)).</span></span> <span data-ttu-id="d0561-157">Například je chyba kompilace k určení `private` nebo `internal` rozhraní v *interface_base* z `public` rozhraní.</span><span class="sxs-lookup"><span data-stu-id="d0561-157">For example, it is a compile-time error to specify a `private` or `internal` interface in the *interface_base* of a `public` interface.</span></span>

<span data-ttu-id="d0561-158">Je chyba kompilace pro rozhraní přímo nebo nepřímo dědí sám od sebe.</span><span class="sxs-lookup"><span data-stu-id="d0561-158">It is a compile-time error for an interface to directly or indirectly inherit from itself.</span></span>

<span data-ttu-id="d0561-159">***Základní rozhraní*** rozhraní jsou explicitní základní rozhraní a jejich základní rozhraní.</span><span class="sxs-lookup"><span data-stu-id="d0561-159">The ***base interfaces*** of an interface are the explicit base interfaces and their base interfaces.</span></span> <span data-ttu-id="d0561-160">Jinými slovy sadu základních rozhraní je kompletní přechodné uzavření explicitní základní rozhraní, jejich explicitní základní rozhraní a tak dále.</span><span class="sxs-lookup"><span data-stu-id="d0561-160">In other words, the set of base interfaces is the complete transitive closure of the explicit base interfaces, their explicit base interfaces, and so on.</span></span> <span data-ttu-id="d0561-161">Rozhraní zdědí všechny členy své základní rozhraní.</span><span class="sxs-lookup"><span data-stu-id="d0561-161">An interface inherits all members of its base interfaces.</span></span> <span data-ttu-id="d0561-162">V příkladu</span><span class="sxs-lookup"><span data-stu-id="d0561-162">In the example</span></span>
```csharp
interface IControl
{
    void Paint();
}

interface ITextBox: IControl
{
    void SetText(string text);
}

interface IListBox: IControl
{
    void SetItems(string[] items);
}

interface IComboBox: ITextBox, IListBox {}
```
<span data-ttu-id="d0561-163">základní rozhraní `IComboBox` jsou `IControl`, `ITextBox`, a `IListBox`.</span><span class="sxs-lookup"><span data-stu-id="d0561-163">the base interfaces of `IComboBox` are `IControl`, `ITextBox`, and `IListBox`.</span></span>

<span data-ttu-id="d0561-164">Jinými slovy `IComboBox` rozhraní výše dědí členy `SetText` a `SetItems` stejně jako `Paint`.</span><span class="sxs-lookup"><span data-stu-id="d0561-164">In other words, the `IComboBox` interface above inherits members `SetText` and `SetItems` as well as `Paint`.</span></span>

<span data-ttu-id="d0561-165">Každé základní rozhraní rozhraní musí být bezpečná pro výstup ([Variance bezpečnosti](interfaces.md#variance-safety)).</span><span class="sxs-lookup"><span data-stu-id="d0561-165">Every base interface of an interface must be output-safe ([Variance safety](interfaces.md#variance-safety)).</span></span> <span data-ttu-id="d0561-166">Třída nebo struktura, která implementuje rozhraní také implicitně implementuje všechna rozhraní základní rozhraní.</span><span class="sxs-lookup"><span data-stu-id="d0561-166">A class or struct that implements an interface also implicitly implements all of the interface's base interfaces.</span></span>

### <a name="interface-body"></a><span data-ttu-id="d0561-167">Rozhraní text</span><span class="sxs-lookup"><span data-stu-id="d0561-167">Interface body</span></span>

<span data-ttu-id="d0561-168">*Interface_body* rozhraní definuje členy rozhraní.</span><span class="sxs-lookup"><span data-stu-id="d0561-168">The *interface_body* of an interface defines the members of the interface.</span></span>

```antlr
interface_body
    : '{' interface_member_declaration* '}'
    ;
```

## <a name="interface-members"></a><span data-ttu-id="d0561-169">Členy rozhraní</span><span class="sxs-lookup"><span data-stu-id="d0561-169">Interface members</span></span>

<span data-ttu-id="d0561-170">Členové rozhraní jsou členy zděděné ze základní rozhraní a členy deklarované pomocí samotným rozhraním.</span><span class="sxs-lookup"><span data-stu-id="d0561-170">The members of an interface are the members inherited from the base interfaces and the members declared by the interface itself.</span></span>

```antlr
interface_member_declaration
    : interface_method_declaration
    | interface_property_declaration
    | interface_event_declaration
    | interface_indexer_declaration
    ;
```

<span data-ttu-id="d0561-171">Deklaraci rozhraní může deklarovat nula nebo více členů.</span><span class="sxs-lookup"><span data-stu-id="d0561-171">An interface declaration may declare zero or more members.</span></span> <span data-ttu-id="d0561-172">Členy rozhraní musí být metody, vlastnosti, události nebo indexery.</span><span class="sxs-lookup"><span data-stu-id="d0561-172">The members of an interface must be methods, properties, events, or indexers.</span></span> <span data-ttu-id="d0561-173">Rozhraní nemohou obsahovat konstanty, pole, operátory, konstruktory instancí, destruktory nebo typy a rozhraní může obsahovat statické členy libovolného typu.</span><span class="sxs-lookup"><span data-stu-id="d0561-173">An interface cannot contain constants, fields, operators, instance constructors, destructors, or types, nor can an interface contain static members of any kind.</span></span>

<span data-ttu-id="d0561-174">Všechny členy rozhraní implicitně mít přístup public.</span><span class="sxs-lookup"><span data-stu-id="d0561-174">All interface members implicitly have public access.</span></span> <span data-ttu-id="d0561-175">Je chyba kompilace pro deklarace člena rozhraní zahrnout všechny modifikátory.</span><span class="sxs-lookup"><span data-stu-id="d0561-175">It is a compile-time error for interface member declarations to include any modifiers.</span></span> <span data-ttu-id="d0561-176">Konkrétně se členy rozhraní nelze použít deklaraci modifikátory `abstract`, `public`, `protected`, `internal`, `private`, `virtual`, `override`, nebo `static`.</span><span class="sxs-lookup"><span data-stu-id="d0561-176">In particular, interfaces members cannot be declared with the modifiers `abstract`, `public`, `protected`, `internal`, `private`, `virtual`, `override`, or `static`.</span></span>

<span data-ttu-id="d0561-177">V příkladu</span><span class="sxs-lookup"><span data-stu-id="d0561-177">The example</span></span>
```csharp
public delegate void StringListEvent(IStringList sender);

public interface IStringList
{
    void Add(string s);
    int Count { get; }
    event StringListEvent Changed;
    string this[int index] { get; set; }
}
```
<span data-ttu-id="d0561-178">deklaruje rozhraní, které obsahuje jeden z možných druhy členů: Metody, vlastnosti, události a indexer.</span><span class="sxs-lookup"><span data-stu-id="d0561-178">declares an interface that contains one each of the possible kinds of members: A method, a property, an event, and an indexer.</span></span>

<span data-ttu-id="d0561-179">*Interface_declaration* vytvoří nové prohlášení prostor ([deklarace](basic-concepts.md#declarations)) a *interface_member_declaration*s okamžitě obsažených *interface_declaration* zavést nové členy do tohoto prostoru deklarace.</span><span class="sxs-lookup"><span data-stu-id="d0561-179">An *interface_declaration* creates a new declaration space ([Declarations](basic-concepts.md#declarations)), and the *interface_member_declaration*s immediately contained by the *interface_declaration* introduce new members into this declaration space.</span></span> <span data-ttu-id="d0561-180">Následující pravidla platí pro *interface_member_declaration*s:</span><span class="sxs-lookup"><span data-stu-id="d0561-180">The following rules apply to *interface_member_declaration*s:</span></span>

*  <span data-ttu-id="d0561-181">Název metody se musí lišit od názvů všech vlastnosti a události deklarované ve stejné rozhraní.</span><span class="sxs-lookup"><span data-stu-id="d0561-181">The name of a method must differ from the names of all properties and events declared in the same interface.</span></span> <span data-ttu-id="d0561-182">Kromě toho, podpis ([podpisy a přetížení](basic-concepts.md#signatures-and-overloading)) z metody se musí lišit od podpisů všechny ostatní metody s deklarací ve stejné rozhraní, a dvě metody s deklarací ve stejné rozhraní nemusí máte podpisů, který se liší pouze `ref` a `out`.</span><span class="sxs-lookup"><span data-stu-id="d0561-182">In addition, the signature ([Signatures and overloading](basic-concepts.md#signatures-and-overloading)) of a method must differ from the signatures of all other methods declared in the same interface, and two methods declared in the same interface may not have signatures that differ solely by `ref` and `out`.</span></span>
*  <span data-ttu-id="d0561-183">Název vlastnosti nebo události se musí lišit od názvů ostatních členů deklarovaná ve stejné rozhraní.</span><span class="sxs-lookup"><span data-stu-id="d0561-183">The name of a property or event must differ from the names of all other members declared in the same interface.</span></span>
*  <span data-ttu-id="d0561-184">Podpis indexeru se musí lišit od podpisů ze všech indexerů je deklarována v stejné rozhraní.</span><span class="sxs-lookup"><span data-stu-id="d0561-184">The signature of an indexer must differ from the signatures of all other indexers declared in the same interface.</span></span>

<span data-ttu-id="d0561-185">Zděděné členy rozhraní jsou speciálně není součástí místa deklarace rozhraní.</span><span class="sxs-lookup"><span data-stu-id="d0561-185">The inherited members of an interface are specifically not part of the declaration space of the interface.</span></span> <span data-ttu-id="d0561-186">Proto rozhraní může deklarovat člena se stejným názvem a signaturou jako zděděného člena.</span><span class="sxs-lookup"><span data-stu-id="d0561-186">Thus, an interface is allowed to declare a member with the same name or signature as an inherited member.</span></span> <span data-ttu-id="d0561-187">Pokud k tomu dojde, se říká, že člen odvozené rozhraní skrýt člen základního rozhraní.</span><span class="sxs-lookup"><span data-stu-id="d0561-187">When this occurs, the derived interface member is said to hide the base interface member.</span></span> <span data-ttu-id="d0561-188">Skrytí zděděného člena není považováno za chybu, ale to způsobit, že kompilátor vydat upozornění.</span><span class="sxs-lookup"><span data-stu-id="d0561-188">Hiding an inherited member is not considered an error, but it does cause the compiler to issue a warning.</span></span> <span data-ttu-id="d0561-189">Potlačit upozornění, musí obsahovat deklarace člena odvozené rozhraní `new` modifikátor k označení, že odvozené člen má skrýt základního člena.</span><span class="sxs-lookup"><span data-stu-id="d0561-189">To suppress the warning, the declaration of the derived interface member must include a `new` modifier to indicate that the derived member is intended to hide the base member.</span></span> <span data-ttu-id="d0561-190">V tomto tématu je popsán dále v [skrytí prostřednictvím dědičnosti](basic-concepts.md#hiding-through-inheritance).</span><span class="sxs-lookup"><span data-stu-id="d0561-190">This topic is discussed further in [Hiding through inheritance](basic-concepts.md#hiding-through-inheritance).</span></span>

<span data-ttu-id="d0561-191">Pokud `new` Modifikátor je součástí deklarace, která neskryje zděděného člena, objeví se upozornění o tom.</span><span class="sxs-lookup"><span data-stu-id="d0561-191">If a `new` modifier is included in a declaration that doesn't hide an inherited member, a warning is issued to that effect.</span></span> <span data-ttu-id="d0561-192">Toto upozornění je potlačeno odebráním `new` modifikátor.</span><span class="sxs-lookup"><span data-stu-id="d0561-192">This warning is suppressed by removing the `new` modifier.</span></span>

<span data-ttu-id="d0561-193">Všimněte si, že členy ve třídě `object` nejsou, přísně vzato členy libovolné rozhraní ([členy rozhraní](interfaces.md#interface-members)).</span><span class="sxs-lookup"><span data-stu-id="d0561-193">Note that the members in class `object` are not, strictly speaking, members of any interface ([Interface members](interfaces.md#interface-members)).</span></span> <span data-ttu-id="d0561-194">Ale členy ve třídě `object` jsou k dispozici prostřednictvím člen vyhledávání v libovolný typ rozhraní ([člen vyhledávání](expressions.md#member-lookup)).</span><span class="sxs-lookup"><span data-stu-id="d0561-194">However, the members in class `object` are available via member lookup in any interface type ([Member lookup](expressions.md#member-lookup)).</span></span>

### <a name="interface-methods"></a><span data-ttu-id="d0561-195">Metody rozhraní</span><span class="sxs-lookup"><span data-stu-id="d0561-195">Interface methods</span></span>

<span data-ttu-id="d0561-196">Metody rozhraní jsou deklarovány pomocí *interface_method_declaration*s:</span><span class="sxs-lookup"><span data-stu-id="d0561-196">Interface methods are declared using *interface_method_declaration*s:</span></span>

```antlr
interface_method_declaration
    : attributes? 'new'? return_type identifier type_parameter_list
      '(' formal_parameter_list? ')' type_parameter_constraints_clause* ';'
    ;
```

<span data-ttu-id="d0561-197">*Atributy*, *typ*, *identifikátor*, a *formal_parameter_list* deklarace metody rozhraní mají stejné To znamená jako deklarace metody ve třídě ([metody](classes.md#methods)).</span><span class="sxs-lookup"><span data-stu-id="d0561-197">The *attributes*, *return_type*, *identifier*, and *formal_parameter_list* of an interface method declaration have the same meaning as those of a method declaration in a class ([Methods](classes.md#methods)).</span></span> <span data-ttu-id="d0561-198">Deklaraci metody rozhraní není povolené zadat těla metody a deklarace proto vždy končí středníkem.</span><span class="sxs-lookup"><span data-stu-id="d0561-198">An interface method declaration is not permitted to specify a method body, and the declaration therefore always ends with a semicolon.</span></span>

<span data-ttu-id="d0561-199">Každý formální parametr typu metody rozhraní musí být bezpečná pro vstup ([Variance bezpečnosti](interfaces.md#variance-safety)), a návratový typ musí být buď `void` nebo výstup typově bezpečné.</span><span class="sxs-lookup"><span data-stu-id="d0561-199">Each formal parameter type of an interface method must be input-safe ([Variance safety](interfaces.md#variance-safety)), and the return type must be either `void` or output-safe.</span></span> <span data-ttu-id="d0561-200">Kromě toho každé omezení typu třídy, rozhraní typu omezení a omezení parametru typu na libovolný typ parametru metody musí být bezpečná pro vstup.</span><span class="sxs-lookup"><span data-stu-id="d0561-200">Furthermore, each class type constraint, interface type constraint and type parameter constraint on any type parameter of the method must be input-safe.</span></span>

<span data-ttu-id="d0561-201">Tato pravidla ujistěte se, že všechny kovariantní nebo kontravariantní využití rozhraní zůstává bezpečnost typů.</span><span class="sxs-lookup"><span data-stu-id="d0561-201">These rules ensure that any covariant or contravariant usage of the interface remains type-safe.</span></span> <span data-ttu-id="d0561-202">Například</span><span class="sxs-lookup"><span data-stu-id="d0561-202">For example,</span></span>
```csharp
interface I<out T> { void M<U>() where U : T; }
```
<span data-ttu-id="d0561-203">je neplatné. protože použití `T` jako omezení parametru typu na `U` není bezpečná pro vstup.</span><span class="sxs-lookup"><span data-stu-id="d0561-203">is illegal because the usage of `T` as a type parameter constraint on `U` is not input-safe.</span></span>

<span data-ttu-id="d0561-204">Bylo toto omezení není použito by bylo možné porušení bezpečnosti typů následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="d0561-204">Were this restriction not in place it would be possible to violate type safety in the following manner:</span></span>
```csharp
class B {}
class D : B{}
class E : B {}
class C : I<D> { public void M<U>() {...} }
...
I<B> b = new C();
b.M<E>();
```
<span data-ttu-id="d0561-205">Toto je ve skutečnosti volání `C.M<E>`.</span><span class="sxs-lookup"><span data-stu-id="d0561-205">This is actually a call to `C.M<E>`.</span></span> <span data-ttu-id="d0561-206">Ale toto volání vyžaduje, aby `E` odvozovat `D`, takže bezpečnost typů by se tady došlo k porušení.</span><span class="sxs-lookup"><span data-stu-id="d0561-206">But that call requires that `E` derive from `D`, so type safety would be violated here.</span></span>

### <a name="interface-properties"></a><span data-ttu-id="d0561-207">Vlastnosti rozhraní</span><span class="sxs-lookup"><span data-stu-id="d0561-207">Interface properties</span></span>

<span data-ttu-id="d0561-208">Vlastnosti rozhraní jsou deklarovány pomocí *interface_property_declaration*s:</span><span class="sxs-lookup"><span data-stu-id="d0561-208">Interface properties are declared using *interface_property_declaration*s:</span></span>

```antlr
interface_property_declaration
    : attributes? 'new'? type identifier '{' interface_accessors '}'
    ;

interface_accessors
    : attributes? 'get' ';'
    | attributes? 'set' ';'
    | attributes? 'get' ';' attributes? 'set' ';'
    | attributes? 'set' ';' attributes? 'get' ';'
    ;
```

<span data-ttu-id="d0561-209">*Atributy*, *typ*, a *identifikátor* deklaraci vlastnosti rozhraní mají stejný význam jako deklarace vlastnosti ve třídě ([ Vlastnosti](classes.md#properties)).</span><span class="sxs-lookup"><span data-stu-id="d0561-209">The *attributes*, *type*, and *identifier* of an interface property declaration have the same meaning as those of a property declaration in a class ([Properties](classes.md#properties)).</span></span>

<span data-ttu-id="d0561-210">Přistupující objekty deklaraci vlastnosti rozhraní odpovídají přistupující objekty vlastnosti deklaraci třídy ([přistupující objekty](classes.md#accessors)), s tím rozdílem, že tělo přístupový objekt musí být vždy středník.</span><span class="sxs-lookup"><span data-stu-id="d0561-210">The accessors of an interface property declaration correspond to the accessors of a class property declaration ([Accessors](classes.md#accessors)), except that the accessor body must always be a semicolon.</span></span> <span data-ttu-id="d0561-211">Proto přístupové objekty jednoduše označí, zda je vlastnost pro čtení i zápis, jen pro čtení nebo jen pro zápis.</span><span class="sxs-lookup"><span data-stu-id="d0561-211">Thus, the accessors simply indicate whether the property is read-write, read-only, or write-only.</span></span>

<span data-ttu-id="d0561-212">Výstup typově bezpečný, pokud existuje přístupový objekt get musí být typu vlastnost rozhraní a musí být vstup typově bezpečný, pokud existuje přístupový objekt set.</span><span class="sxs-lookup"><span data-stu-id="d0561-212">The type of an interface property must be output-safe if there is a get accessor, and must be input-safe if there is a set accessor.</span></span>

### <a name="interface-events"></a><span data-ttu-id="d0561-213">Události rozhraní</span><span class="sxs-lookup"><span data-stu-id="d0561-213">Interface events</span></span>

<span data-ttu-id="d0561-214">Události rozhraní jsou deklarovány pomocí *interface_event_declaration*s:</span><span class="sxs-lookup"><span data-stu-id="d0561-214">Interface events are declared using *interface_event_declaration*s:</span></span>

```antlr
interface_event_declaration
    : attributes? 'new'? 'event' type identifier ';'
    ;
```

<span data-ttu-id="d0561-215">*Atributy*, *typ*, a *identifikátor* deklaraci události rozhraní mají stejný význam jako deklaraci události ve třídě ([události ](classes.md#events)).</span><span class="sxs-lookup"><span data-stu-id="d0561-215">The *attributes*, *type*, and *identifier* of an interface event declaration have the same meaning as those of an event declaration in a class ([Events](classes.md#events)).</span></span>

<span data-ttu-id="d0561-216">Bezpečná pro vstup musí být typu události rozhraní.</span><span class="sxs-lookup"><span data-stu-id="d0561-216">The type of an interface event must be input-safe.</span></span>

### <a name="interface-indexers"></a><span data-ttu-id="d0561-217">Indexery v rozhraní</span><span class="sxs-lookup"><span data-stu-id="d0561-217">Interface indexers</span></span>

<span data-ttu-id="d0561-218">Indexery v rozhraní jsou deklarovány pomocí *interface_indexer_declaration*s:</span><span class="sxs-lookup"><span data-stu-id="d0561-218">Interface indexers are declared using *interface_indexer_declaration*s:</span></span>

```antlr
interface_indexer_declaration
    : attributes? 'new'? type 'this' '[' formal_parameter_list ']' '{' interface_accessors '}'
    ;
```

<span data-ttu-id="d0561-219">*Atributy*, *typ*, a *formal_parameter_list* indexer deklarace rozhraní mají stejný význam jako deklarace indexeru ve třídě ([ Indexery](classes.md#indexers)).</span><span class="sxs-lookup"><span data-stu-id="d0561-219">The *attributes*, *type*, and *formal_parameter_list* of an interface indexer declaration have the same meaning as those of an indexer declaration in a class ([Indexers](classes.md#indexers)).</span></span>

<span data-ttu-id="d0561-220">Přistupující objekty deklaraci rozhraní indexeru odpovídají přistupující objekty indexer deklaraci třídy ([indexery](classes.md#indexers)), s tím rozdílem, že tělo přístupový objekt musí být vždy středník.</span><span class="sxs-lookup"><span data-stu-id="d0561-220">The accessors of an interface indexer declaration correspond to the accessors of a class indexer declaration ([Indexers](classes.md#indexers)), except that the accessor body must always be a semicolon.</span></span> <span data-ttu-id="d0561-221">Proto přístupové objekty jednoduše označí, zda je indexeru pro čtení i zápis, jen pro čtení nebo jen pro zápis.</span><span class="sxs-lookup"><span data-stu-id="d0561-221">Thus, the accessors simply indicate whether the indexer is read-write, read-only, or write-only.</span></span>

<span data-ttu-id="d0561-222">Všechny typy formálních parametrů indexer rozhraní musí být bezpečná pro vstup.</span><span class="sxs-lookup"><span data-stu-id="d0561-222">All the formal parameter types of an interface indexer must be input-safe .</span></span> <span data-ttu-id="d0561-223">Kromě toho všechny `out` nebo `ref` typy formálních parametrů musí být také výstup typově bezpečné.</span><span class="sxs-lookup"><span data-stu-id="d0561-223">In addition, any `out` or `ref` formal parameter types must also be output-safe.</span></span> <span data-ttu-id="d0561-224">Poznámka: to dokonce i `out` parametry musejí být vstup typově bezpečný, kvůli omezením platformy pro základní spuštění.</span><span class="sxs-lookup"><span data-stu-id="d0561-224">Note that even `out` parameters are required to be input-safe, due to a limitation of the underlying execution platform.</span></span>

<span data-ttu-id="d0561-225">Výstup typově bezpečný, pokud existuje přístupový objekt get musí být typu rozhraní indexeru a musí být vstup typově bezpečný, pokud existuje přístupový objekt set.</span><span class="sxs-lookup"><span data-stu-id="d0561-225">The type of an interface indexer must be output-safe if there is a get accessor, and must be input-safe if there is a set accessor.</span></span>

### <a name="interface-member-access"></a><span data-ttu-id="d0561-226">Přístup ke členu rozhraní</span><span class="sxs-lookup"><span data-stu-id="d0561-226">Interface member access</span></span>

<span data-ttu-id="d0561-227">Rozhraní členové budou přístupní prostřednictvím přístupu ke členu ([přístup ke členu](expressions.md#member-access)) a přístup indexeru ([přístup indexeru](expressions.md#indexer-access)) výrazů formuláře `I.M` a `I[A]`, kde `I` je typu rozhraní, `M` je metoda, vlastnost nebo událost typu rozhraní, a `A` je seznam argumentů indexeru.</span><span class="sxs-lookup"><span data-stu-id="d0561-227">Interface members are accessed through member access ([Member access](expressions.md#member-access)) and indexer access ([Indexer access](expressions.md#indexer-access)) expressions of the form `I.M` and `I[A]`, where `I` is an interface type, `M` is a method, property, or event of that interface type, and `A` is an indexer argument list.</span></span>

<span data-ttu-id="d0561-228">Pro rozhraní, které se týkají výhradně jednoduchou dědičností (přesně žádnou nebo jednu přímou základní rozhraní každého rozhraní v řetězu dědičnosti má), důsledky člena vyhledávání ([člen vyhledávání](expressions.md#member-lookup)), volání metody ([ Volání metod](expressions.md#method-invocations)) a přístup indexeru ([přístup indexeru](expressions.md#indexer-access)) pravidla jsou stejné jako u třídy a struktury: Skrýt členy více odvozený méně odvozené členy se stejným názvem a signaturou.</span><span class="sxs-lookup"><span data-stu-id="d0561-228">For interfaces that are strictly single-inheritance (each interface in the inheritance chain has exactly zero or one direct base interface), the effects of the member lookup ([Member lookup](expressions.md#member-lookup)), method invocation ([Method invocations](expressions.md#method-invocations)), and indexer access ([Indexer access](expressions.md#indexer-access)) rules are exactly the same as for classes and structs: More derived members hide less derived members with the same name or signature.</span></span> <span data-ttu-id="d0561-229">Pro rozhraní vícenásobné dědičnosti nejednoznačnosti může dojít, když dva nebo víc nesouvisejících základních rozhraní deklarovat členy s totožným názvem a signaturou.</span><span class="sxs-lookup"><span data-stu-id="d0561-229">However, for multiple-inheritance interfaces, ambiguities can occur when two or more unrelated base interfaces declare members with the same name or signature.</span></span> <span data-ttu-id="d0561-230">Tato část ukazuje několik příkladů takových situacích.</span><span class="sxs-lookup"><span data-stu-id="d0561-230">This section shows several examples of such situations.</span></span> <span data-ttu-id="d0561-231">Ve všech případech je možné přeložit nejednoznačnosti explicitní přetypování.</span><span class="sxs-lookup"><span data-stu-id="d0561-231">In all cases, explicit casts can be used to resolve the ambiguities.</span></span>

<span data-ttu-id="d0561-232">V příkladu</span><span class="sxs-lookup"><span data-stu-id="d0561-232">In the example</span></span>
```csharp
interface IList
{
    int Count { get; set; }
}

interface ICounter
{
    void Count(int i);
}

interface IListCounter: IList, ICounter {}

class C
{
    void Test(IListCounter x) {
        x.Count(1);                  // Error
        x.Count = 1;                 // Error
        ((IList)x).Count = 1;        // Ok, invokes IList.Count.set
        ((ICounter)x).Count(1);      // Ok, invokes ICounter.Count
    }
}
```
<span data-ttu-id="d0561-233">první dva příkazy způsobit chyby při kompilaci, protože člen vyhledávání ([člen vyhledávání](expressions.md#member-lookup)) z `Count` v `IListCounter` je nejednoznačný.</span><span class="sxs-lookup"><span data-stu-id="d0561-233">the first two statements cause compile-time errors because the member lookup ([Member lookup](expressions.md#member-lookup)) of `Count` in `IListCounter` is ambiguous.</span></span> <span data-ttu-id="d0561-234">Jak je znázorněno v příkladu, přetypování se vyřešit nejednoznačnost `x` na typ odpovídající základní rozhraní.</span><span class="sxs-lookup"><span data-stu-id="d0561-234">As illustrated by the example, the ambiguity is resolved by casting `x` to the appropriate base interface type.</span></span> <span data-ttu-id="d0561-235">Tyto položky CAST mít žádné náklady za běhu – pouze sestávají z následujících zobrazení instance jako méně odvozený typ v době kompilace.</span><span class="sxs-lookup"><span data-stu-id="d0561-235">Such casts have no run-time costs—they merely consist of viewing the instance as a less derived type at compile-time.</span></span>

<span data-ttu-id="d0561-236">V příkladu</span><span class="sxs-lookup"><span data-stu-id="d0561-236">In the example</span></span>
```csharp
interface IInteger
{
    void Add(int i);
}

interface IDouble
{
    void Add(double d);
}

interface INumber: IInteger, IDouble {}

class C
{
    void Test(INumber n) {
        n.Add(1);                // Invokes IInteger.Add
        n.Add(1.0);              // Only IDouble.Add is applicable
        ((IInteger)n).Add(1);    // Only IInteger.Add is a candidate
        ((IDouble)n).Add(1);     // Only IDouble.Add is a candidate
    }
}
```
<span data-ttu-id="d0561-237">vyvolání `n.Add(1)` vybere `IInteger.Add` použitím pravidla rozlišení přetížení [rozlišení přetěžování](expressions.md#overload-resolution).</span><span class="sxs-lookup"><span data-stu-id="d0561-237">the invocation `n.Add(1)` selects `IInteger.Add` by applying the overload resolution rules of [Overload resolution](expressions.md#overload-resolution).</span></span> <span data-ttu-id="d0561-238">Podobně při vyvolání `n.Add(1.0)` vybere `IDouble.Add`.</span><span class="sxs-lookup"><span data-stu-id="d0561-238">Similarly the invocation `n.Add(1.0)` selects `IDouble.Add`.</span></span> <span data-ttu-id="d0561-239">Po vložení explicitní přetypování, je pouze jeden Release candidate metoda a proto nejednoznačnosti.</span><span class="sxs-lookup"><span data-stu-id="d0561-239">When explicit casts are inserted, there is only one candidate method, and thus no ambiguity.</span></span>

<span data-ttu-id="d0561-240">V příkladu</span><span class="sxs-lookup"><span data-stu-id="d0561-240">In the example</span></span>
```csharp
interface IBase
{
    void F(int i);
}

interface ILeft: IBase
{
    new void F(int i);
}

interface IRight: IBase
{
    void G();
}

interface IDerived: ILeft, IRight {}

class A
{
    void Test(IDerived d) {
        d.F(1);                 // Invokes ILeft.F
        ((IBase)d).F(1);        // Invokes IBase.F
        ((ILeft)d).F(1);        // Invokes ILeft.F
        ((IRight)d).F(1);       // Invokes IBase.F
    }
}
```
<span data-ttu-id="d0561-241">`IBase.F` skryl člen `ILeft.F` člena.</span><span class="sxs-lookup"><span data-stu-id="d0561-241">the `IBase.F` member is hidden by the `ILeft.F` member.</span></span> <span data-ttu-id="d0561-242">Vyvolání `d.F(1)` takto vybere `ILeft.F`, i když `IBase.F` se nebudou skryté v přístupové cestě, která provede `IRight`.</span><span class="sxs-lookup"><span data-stu-id="d0561-242">The invocation `d.F(1)` thus selects `ILeft.F`, even though `IBase.F` appears to not be hidden in the access path that leads through `IRight`.</span></span>

<span data-ttu-id="d0561-243">Intuitivní pravidlo pro skrytí v rozhraních vícenásobné dědičnosti je jednoduše toto: Pokud je člen skrytý v jakékoli cestě přístup, je skrytý ve všech cestách přístup.</span><span class="sxs-lookup"><span data-stu-id="d0561-243">The intuitive rule for hiding in multiple-inheritance interfaces is simply this: If a member is hidden in any access path, it is hidden in all access paths.</span></span> <span data-ttu-id="d0561-244">Protože cesta pro přístup z `IDerived` k `ILeft` k `IBase` skryje `IBase.F`, člen také skrytý v přístupové cestě z `IDerived` k `IRight` k `IBase`.</span><span class="sxs-lookup"><span data-stu-id="d0561-244">Because the access path from `IDerived` to `ILeft` to `IBase` hides `IBase.F`, the member is also hidden in the access path from `IDerived` to `IRight` to `IBase`.</span></span>

## <a name="fully-qualified-interface-member-names"></a><span data-ttu-id="d0561-245">Rozhraní plně kvalifikované názvy členů</span><span class="sxs-lookup"><span data-stu-id="d0561-245">Fully qualified interface member names</span></span>

<span data-ttu-id="d0561-246">Člen rozhraní se někdy označuje podle jeho ***plně kvalifikovaný název***.</span><span class="sxs-lookup"><span data-stu-id="d0561-246">An interface member is sometimes referred to by its ***fully qualified name***.</span></span> <span data-ttu-id="d0561-247">Plně kvalifikovaný název člena rozhraní se skládá z názvu rozhraní, ve kterém je deklarována, následované tečkou, za nímž následuje název člena člena.</span><span class="sxs-lookup"><span data-stu-id="d0561-247">The fully qualified name of an interface member consists of the name of the interface in which the member is declared, followed by a dot, followed by the name of the member.</span></span> <span data-ttu-id="d0561-248">Plně kvalifikovaný název členu odkazuje na rozhraní, ve kterém člen je deklarovaný.</span><span class="sxs-lookup"><span data-stu-id="d0561-248">The fully qualified name of a member references the interface in which the member is declared.</span></span> <span data-ttu-id="d0561-249">Mějme například deklarace</span><span class="sxs-lookup"><span data-stu-id="d0561-249">For example, given the declarations</span></span>
```csharp
interface IControl
{
    void Paint();
}

interface ITextBox: IControl
{
    void SetText(string text);
}
```
<span data-ttu-id="d0561-250">plně kvalifikovaný název `Paint` je `IControl.Paint` a plně kvalifikovaný název `SetText` je `ITextBox.SetText`.</span><span class="sxs-lookup"><span data-stu-id="d0561-250">the fully qualified name of `Paint` is `IControl.Paint` and the fully qualified name of `SetText` is `ITextBox.SetText`.</span></span>

<span data-ttu-id="d0561-251">V příkladu výše, není možné odkazovat na `Paint` jako `ITextBox.Paint`.</span><span class="sxs-lookup"><span data-stu-id="d0561-251">In the example above, it is not possible to refer to `Paint` as `ITextBox.Paint`.</span></span>

<span data-ttu-id="d0561-252">Pokud rozhraní je součástí oboru názvů, plně kvalifikovaný název člena rozhraní obsahuje název oboru názvů.</span><span class="sxs-lookup"><span data-stu-id="d0561-252">When an interface is part of a namespace, the fully qualified name of an interface member includes the namespace name.</span></span> <span data-ttu-id="d0561-253">Příklad</span><span class="sxs-lookup"><span data-stu-id="d0561-253">For example</span></span>
```csharp
namespace System
{
    public interface ICloneable
    {
        object Clone();
    }
}
```

<span data-ttu-id="d0561-254">Tady, plně kvalifikovaný název `Clone` je metoda `System.ICloneable.Clone`.</span><span class="sxs-lookup"><span data-stu-id="d0561-254">Here, the fully qualified name of the `Clone` method is `System.ICloneable.Clone`.</span></span>

## <a name="interface-implementations"></a><span data-ttu-id="d0561-255">Implementace rozhraní</span><span class="sxs-lookup"><span data-stu-id="d0561-255">Interface implementations</span></span>

<span data-ttu-id="d0561-256">Pomocí třídy a struktury mohou implementovat rozhraní.</span><span class="sxs-lookup"><span data-stu-id="d0561-256">Interfaces may be implemented by classes and structs.</span></span> <span data-ttu-id="d0561-257">K označení, že třídy nebo struktury přímo implementuje rozhraní, je identifikátor rozhraní součástí seznamu třídy základní třídy nebo struktury.</span><span class="sxs-lookup"><span data-stu-id="d0561-257">To indicate that a class or struct directly implements an interface, the interface identifier is included in the base class list of the class or struct.</span></span> <span data-ttu-id="d0561-258">Příklad:</span><span class="sxs-lookup"><span data-stu-id="d0561-258">For example:</span></span>
```csharp
interface ICloneable
{
    object Clone();
}

interface IComparable
{
    int CompareTo(object other);
}

class ListEntry: ICloneable, IComparable
{
    public object Clone() {...}
    public int CompareTo(object other) {...}
}
```

<span data-ttu-id="d0561-259">Třída nebo struktura, která implementuje rozhraní přímo také přímo implementuje všechna rozhraní základní rozhraní implicitně.</span><span class="sxs-lookup"><span data-stu-id="d0561-259">A class or struct that directly implements an interface also directly implements all of the interface's base interfaces implicitly.</span></span> <span data-ttu-id="d0561-260">To platí i v případě, že dané třídy nebo struktury neobsahuje explicitně všech základních rozhraní v seznamu základních tříd.</span><span class="sxs-lookup"><span data-stu-id="d0561-260">This is true even if the class or struct doesn't explicitly list all base interfaces in the base class list.</span></span> <span data-ttu-id="d0561-261">Příklad:</span><span class="sxs-lookup"><span data-stu-id="d0561-261">For example:</span></span>
```csharp
interface IControl
{
    void Paint();
}

interface ITextBox: IControl
{
    void SetText(string text);
}

class TextBox: ITextBox
{
    public void Paint() {...}
    public void SetText(string text) {...}
}
```

<span data-ttu-id="d0561-262">Tady třídy `TextBox` implementuje oba `IControl` a `ITextBox`.</span><span class="sxs-lookup"><span data-stu-id="d0561-262">Here, class `TextBox` implements both `IControl` and `ITextBox`.</span></span>

<span data-ttu-id="d0561-263">Pokud třída `C` přímo implementuje rozhraní, všechny třídy odvozené z jazyka C implicitně také implementovat rozhraní.</span><span class="sxs-lookup"><span data-stu-id="d0561-263">When a class `C` directly implements an interface, all classes derived from C also implement the interface implicitly.</span></span> <span data-ttu-id="d0561-264">Základní rozhraní zadané v deklaraci třídy mohou být typy vybudované rozhraní ([vytvořený typy](types.md#constructed-types)).</span><span class="sxs-lookup"><span data-stu-id="d0561-264">The base interfaces specified in a class declaration can be constructed interface types ([Constructed types](types.md#constructed-types)).</span></span> <span data-ttu-id="d0561-265">Základní rozhraní nemůže být parametr typu sama o sobě, i když to může zahrnovat parametry typu, které jsou v oboru.</span><span class="sxs-lookup"><span data-stu-id="d0561-265">A base interface cannot be a type parameter on its own, though it can involve the type parameters that are in scope.</span></span> <span data-ttu-id="d0561-266">Následující kód ukazuje, jak implementovat a rozšířit sestavené typy třídy:</span><span class="sxs-lookup"><span data-stu-id="d0561-266">The following code illustrates how a class can implement and extend constructed types:</span></span>
```csharp
class C<U,V> {}

interface I1<V> {}

class D: C<string,int>, I1<string> {}

class E<T>: C<int,T>, I1<T> {}
```

<span data-ttu-id="d0561-267">Základní rozhraní deklaraci obecné třídy musí splňovat pravidla jedinečnost je popsáno v [jedinečnost implementovaná rozhraní](interfaces.md#uniqueness-of-implemented-interfaces).</span><span class="sxs-lookup"><span data-stu-id="d0561-267">The base interfaces of a generic class declaration must satisfy the uniqueness rule described in [Uniqueness of implemented interfaces](interfaces.md#uniqueness-of-implemented-interfaces).</span></span>

### <a name="explicit-interface-member-implementations"></a><span data-ttu-id="d0561-268">Člen implementace explicitního rozhraní</span><span class="sxs-lookup"><span data-stu-id="d0561-268">Explicit interface member implementations</span></span>

<span data-ttu-id="d0561-269">Pro účely provádění rozhraní, třídy nebo struktury může deklarovat ***implementace explicitního rozhraní členských***.</span><span class="sxs-lookup"><span data-stu-id="d0561-269">For purposes of implementing interfaces, a class or struct may declare ***explicit interface member implementations***.</span></span> <span data-ttu-id="d0561-270">Implementace explicitního rozhraní člen je deklarace metody, vlastnosti, události nebo indexeru, který odkazuje na rozhraní plně kvalifikovaný název členu.</span><span class="sxs-lookup"><span data-stu-id="d0561-270">An explicit interface member implementation is a method, property, event, or indexer declaration that references a fully qualified interface member name.</span></span> <span data-ttu-id="d0561-271">Příklad</span><span class="sxs-lookup"><span data-stu-id="d0561-271">For example</span></span>
```csharp
interface IList<T>
{
    T[] GetElements();
}

interface IDictionary<K,V>
{
    V this[K key];
    void Add(K key, V value);
}

class List<T>: IList<T>, IDictionary<int,T>
{
    T[] IList<T>.GetElements() {...}
    T IDictionary<int,T>.this[int index] {...}
    void IDictionary<int,T>.Add(int index, T value) {...}
}
```

<span data-ttu-id="d0561-272">Tady `IDictionary<int,T>.this` a `IDictionary<int,T>.Add` jsou člen implementace explicitního rozhraní.</span><span class="sxs-lookup"><span data-stu-id="d0561-272">Here `IDictionary<int,T>.this` and `IDictionary<int,T>.Add` are explicit interface member implementations.</span></span>

<span data-ttu-id="d0561-273">V některých případech nemusí být vhodný pro implementující třídu, ve kterém může být případ tohoto člena rozhraní implementované pomocí explicitní implementace členu rozhraní název člena rozhraní.</span><span class="sxs-lookup"><span data-stu-id="d0561-273">In some cases, the name of an interface member may not be appropriate for the implementing class, in which case the interface member may be implemented using explicit interface member implementation.</span></span> <span data-ttu-id="d0561-274">Třída implementace souborovou abstrakci, například by pravděpodobně implementovat `Close` členskou funkci, která má za následek uvolnění souboru prostředků a implementovat `Dispose` metodu `IDisposable` rozhraní pomocí explicitního rozhraní implementace člena:</span><span class="sxs-lookup"><span data-stu-id="d0561-274">A class implementing a file abstraction, for example, would likely implement a `Close` member function that has the effect of releasing the file resource, and implement the `Dispose` method of the `IDisposable` interface using explicit interface member implementation:</span></span>
```csharp
interface IDisposable
{
    void Dispose();
}

class MyFile: IDisposable
{
    void IDisposable.Dispose() {
        Close();
    }

    public void Close() {
        // Do what's necessary to close the file
        System.GC.SuppressFinalize(this);
    }
}
```

<span data-ttu-id="d0561-275">Není možné přistupovat explicitní implementace členu rozhraní prostřednictvím jeho plně kvalifikovaný název volání metody, přístup k vlastnosti nebo indexeru přístup.</span><span class="sxs-lookup"><span data-stu-id="d0561-275">It is not possible to access an explicit interface member implementation through its fully qualified name in a method invocation, property access, or indexer access.</span></span> <span data-ttu-id="d0561-276">Implementace explicitního rozhraní člen je přístupný pouze prostřednictvím instance rozhraní a se odkazuje v tomto případě jednoduše tak, že její název člena.</span><span class="sxs-lookup"><span data-stu-id="d0561-276">An explicit interface member implementation can only be accessed through an interface instance, and is in that case referenced simply by its member name.</span></span>

<span data-ttu-id="d0561-277">Je chyba kompilace pro člen implementace explicitního rozhraní zahrnout modifikátory přístupu a je chyba kompilace zahrnout modifikátory `abstract`, `virtual`, `override`, nebo `static`.</span><span class="sxs-lookup"><span data-stu-id="d0561-277">It is a compile-time error for an explicit interface member implementation to include access modifiers, and it is a compile-time error to include the modifiers `abstract`, `virtual`, `override`, or `static`.</span></span>

<span data-ttu-id="d0561-278">Implementace explicitního rozhraní členských mají různou přístupností. vlastnosti než ostatní členy.</span><span class="sxs-lookup"><span data-stu-id="d0561-278">Explicit interface member implementations have different accessibility characteristics than other members.</span></span> <span data-ttu-id="d0561-279">Protože jsou implementace explicitního rozhraní členských nikdy přístupné prostřednictvím jejich plně kvalifikovaný název metody vyvolání nebo přístup k vlastnosti, jsou ve smyslu privátní.</span><span class="sxs-lookup"><span data-stu-id="d0561-279">Because explicit interface member implementations are never accessible through their fully qualified name in a method invocation or a property access, they are in a sense private.</span></span> <span data-ttu-id="d0561-280">Nicméně protože jsou dostupné prostřednictvím instance rozhraní, jsou ve smyslu také veřejné.</span><span class="sxs-lookup"><span data-stu-id="d0561-280">However, since they can be accessed through an interface instance, they are in a sense also public.</span></span>

<span data-ttu-id="d0561-281">Implementace explicitního rozhraní členských mají dva primární účely:</span><span class="sxs-lookup"><span data-stu-id="d0561-281">Explicit interface member implementations serve two primary purposes:</span></span>

*  <span data-ttu-id="d0561-282">Protože implementace explicitního rozhraní člen není přístupný prostřednictvím instance třídy nebo struktury, umožňují implementací rozhraní, které se mají vyloučit z veřejného rozhraní, třídy nebo struktury.</span><span class="sxs-lookup"><span data-stu-id="d0561-282">Because explicit interface member implementations are not accessible through class or struct instances, they allow interface implementations to be excluded from the public interface of a class or struct.</span></span> <span data-ttu-id="d0561-283">To je zvlášť užitečné, když třída nebo struktura implementuje interní rozhraní, které nejsou důležité pro příjemce této třídy nebo struktury.</span><span class="sxs-lookup"><span data-stu-id="d0561-283">This is particularly useful when a class or struct implements an internal interface that is of no interest to a consumer of that class or struct.</span></span>
*  <span data-ttu-id="d0561-284">Implementace explicitního rozhraní členských Povolit odstraňování mnohoznačnosti rozhraní členů se stejným podpisem.</span><span class="sxs-lookup"><span data-stu-id="d0561-284">Explicit interface member implementations allow disambiguation of interface members with the same signature.</span></span> <span data-ttu-id="d0561-285">Bez implementace explicitního rozhraní členských to by jinak nebylo možné pro třídu nebo strukturu mít různé implementace rozhraní členy se stejným podpisem a návratový typ, jako by ji nebylo možné pro třídu nebo strukturu mít žádnou implementaci všechny členy rozhraní se stejným podpisem ale s různými návratové typy.</span><span class="sxs-lookup"><span data-stu-id="d0561-285">Without explicit interface member implementations it would be impossible for a class or struct to have different implementations of interface members with the same signature and return type, as would it be impossible for a class or struct to have any implementation at all of interface members with the same signature but with different return types.</span></span>

<span data-ttu-id="d0561-286">Implementace explicitního rozhraní člen platný třídy nebo struktury název rozhraní v její základní třídy seznamu, který obsahuje člena, jehož plně kvalifikovaný název, typ a typy parametrů přesně odpovídat názvům členů explicitního rozhraní implementace.</span><span class="sxs-lookup"><span data-stu-id="d0561-286">For an explicit interface member implementation to be valid, the class or struct must name an interface in its base class list that contains a member whose fully qualified name, type, and parameter types exactly match those of the explicit interface member implementation.</span></span> <span data-ttu-id="d0561-287">Díky tomu se v následující třídy</span><span class="sxs-lookup"><span data-stu-id="d0561-287">Thus, in the following class</span></span>
```csharp
class Shape: ICloneable
{
    object ICloneable.Clone() {...}
    int IComparable.CompareTo(object other) {...}    // invalid
}
```
<span data-ttu-id="d0561-288">deklarace `IComparable.CompareTo` výsledkem chyba kompilace, protože `IComparable` není uvedena v seznamu základních tříd z `Shape` a není základní rozhraní `ICloneable`.</span><span class="sxs-lookup"><span data-stu-id="d0561-288">the declaration of `IComparable.CompareTo` results in a compile-time error because `IComparable` is not listed in the base class list of `Shape` and is not a base interface of `ICloneable`.</span></span> <span data-ttu-id="d0561-289">Stejně tak v deklaracích</span><span class="sxs-lookup"><span data-stu-id="d0561-289">Likewise, in the declarations</span></span>
```csharp
class Shape: ICloneable
{
    object ICloneable.Clone() {...}
}

class Ellipse: Shape
{
    object ICloneable.Clone() {...}    // invalid
}
```
<span data-ttu-id="d0561-290">deklarace `ICloneable.Clone` v `Ellipse` výsledkem chyba kompilace, protože `ICloneable` není výslovně uvedené v seznamu základních tříd z `Ellipse`.</span><span class="sxs-lookup"><span data-stu-id="d0561-290">the declaration of `ICloneable.Clone` in `Ellipse` results in a compile-time error because `ICloneable` is not explicitly listed in the base class list of `Ellipse`.</span></span>

<span data-ttu-id="d0561-291">Plně kvalifikovaný název člena rozhraní musí odkazovat na rozhraní, ve kterém byl deklarován člen.</span><span class="sxs-lookup"><span data-stu-id="d0561-291">The fully qualified name of an interface member must reference the interface in which the member was declared.</span></span> <span data-ttu-id="d0561-292">Proto v deklaracích</span><span class="sxs-lookup"><span data-stu-id="d0561-292">Thus, in the declarations</span></span>
```csharp
interface IControl
{
    void Paint();
}

interface ITextBox: IControl
{
    void SetText(string text);
}

class TextBox: ITextBox
{
    void IControl.Paint() {...}
    void ITextBox.SetText(string text) {...}
}
```
<span data-ttu-id="d0561-293">implementace explicitního rozhraní člen `Paint` musí být napsaná jako `IControl.Paint`.</span><span class="sxs-lookup"><span data-stu-id="d0561-293">the explicit interface member implementation of `Paint` must be written as `IControl.Paint`.</span></span>

### <a name="uniqueness-of-implemented-interfaces"></a><span data-ttu-id="d0561-294">Jedinečnost implementovaného rozhraní</span><span class="sxs-lookup"><span data-stu-id="d0561-294">Uniqueness of implemented interfaces</span></span>

<span data-ttu-id="d0561-295">Rozhraní implementovaná deklarace obecného typu musí zůstat jedinečné pro všechny možné sestavené typy.</span><span class="sxs-lookup"><span data-stu-id="d0561-295">The interfaces implemented by a generic type declaration must remain unique for all possible constructed types.</span></span> <span data-ttu-id="d0561-296">Bez tohoto pravidla by bylo možné určit správnou metodu pro volání pro určité sestavené typy.</span><span class="sxs-lookup"><span data-stu-id="d0561-296">Without this rule, it would be impossible to determine the correct method to call for certain constructed types.</span></span> <span data-ttu-id="d0561-297">Předpokládejme například, deklaraci obecné třídy byly převody povoleny k zapsání následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="d0561-297">For example, suppose a generic class declaration were permitted to be written as follows:</span></span>
```csharp
interface I<T>
{
    void F();
}

class X<U,V>: I<U>, I<V>                    // Error: I<U> and I<V> conflict
{
    void I<U>.F() {...}
    void I<V>.F() {...}
}
```

<span data-ttu-id="d0561-298">Byla to povolené, by bylo možné určit, jaký kód ke spuštění v následujících případech:</span><span class="sxs-lookup"><span data-stu-id="d0561-298">Were this permitted, it would be impossible to determine which code to execute in the following case:</span></span>
```csharp
I<int> x = new X<int,int>();
x.F();
```

<span data-ttu-id="d0561-299">Pokud chcete zjistit, pokud rozhraní seznamu deklarace obecného typu je platný, jsou prováděny následovně:</span><span class="sxs-lookup"><span data-stu-id="d0561-299">To determine if the interface list of a generic type declaration is valid, the following steps are performed:</span></span>

*  <span data-ttu-id="d0561-300">Umožní `L` být seznam rozhraní přímo zadaný v deklaraci rozhraní, struktury nebo obecná třída `C`.</span><span class="sxs-lookup"><span data-stu-id="d0561-300">Let `L` be the list of interfaces directly specified in a generic class, struct, or interface declaration `C`.</span></span>
*  <span data-ttu-id="d0561-301">Přidat do `L` některé základní rozhraní již v rozhraní `L`.</span><span class="sxs-lookup"><span data-stu-id="d0561-301">Add to `L` any base interfaces of the interfaces already in `L`.</span></span>
*  <span data-ttu-id="d0561-302">Odebrat duplicity z `L`.</span><span class="sxs-lookup"><span data-stu-id="d0561-302">Remove any duplicates from `L`.</span></span>
*  <span data-ttu-id="d0561-303">Pokud všechny možné konstruovaný typ vytvořený z `C` by po argumenty typu jsou nahrazeny do `L`, způsobit, že dvě rozhraní v `L` shodovat, pak deklarace `C` je neplatný.</span><span class="sxs-lookup"><span data-stu-id="d0561-303">If any possible constructed type created from `C` would, after type arguments are substituted into `L`, cause two interfaces in `L` to be identical, then the declaration of `C` is invalid.</span></span> <span data-ttu-id="d0561-304">Deklarace omezení nejsou považovány za při určování všechny možné sestavené typy.</span><span class="sxs-lookup"><span data-stu-id="d0561-304">Constraint declarations are not considered when determining all possible constructed types.</span></span>

<span data-ttu-id="d0561-305">V deklaraci třídy `X` nad seznamem rozhraní `L` se skládá z `I<U>` a `I<V>`.</span><span class="sxs-lookup"><span data-stu-id="d0561-305">In the class declaration `X` above, the interface list `L` consists of `I<U>` and `I<V>`.</span></span> <span data-ttu-id="d0561-306">Deklarace není platná, protože některé konstruovaný typ s `U` a `V` stejným typem by způsobila tato dvě rozhraní být identické typy.</span><span class="sxs-lookup"><span data-stu-id="d0561-306">The declaration is invalid because any constructed type with `U` and `V` being the same type would cause these two interfaces to be identical types.</span></span>

<span data-ttu-id="d0561-307">Je možné, určených na úrovních různé dědičnosti sjednotit rozhraní:</span><span class="sxs-lookup"><span data-stu-id="d0561-307">It is possible for interfaces specified at different inheritance levels to unify:</span></span>
```csharp
interface I<T>
{
    void F();
}

class Base<U>: I<U>
{
    void I<U>.F() {...}
}

class Derived<U,V>: Base<U>, I<V>    // Ok
{
    void I<V>.F() {...}
}
```

<span data-ttu-id="d0561-308">Tento kód je platný i v případě `Derived<U,V>` implementuje oba `I<U>` a `I<V>`.</span><span class="sxs-lookup"><span data-stu-id="d0561-308">This code is valid even though `Derived<U,V>` implements both `I<U>` and `I<V>`.</span></span> <span data-ttu-id="d0561-309">Kód</span><span class="sxs-lookup"><span data-stu-id="d0561-309">The code</span></span>
```csharp
I<int> x = new Derived<int,int>();
x.F();
```
<span data-ttu-id="d0561-310">vyvolá metodu v `Derived`, protože `Derived<int,int>` efektivně implementuje znovu `I<int>` ([opětovná implementace rozhraní](interfaces.md#interface-re-implementation)).</span><span class="sxs-lookup"><span data-stu-id="d0561-310">invokes the method in `Derived`, since `Derived<int,int>` effectively re-implements `I<int>` ([Interface re-implementation](interfaces.md#interface-re-implementation)).</span></span>

### <a name="implementation-of-generic-methods"></a><span data-ttu-id="d0561-311">Provádění obecné metody</span><span class="sxs-lookup"><span data-stu-id="d0561-311">Implementation of generic methods</span></span>

<span data-ttu-id="d0561-312">Při obecné metody implicitně implementuje metodu rozhraní omezení uvedená pro každý parametr typu metody musí být v obou deklarace ekvivalentní (po libovolný typ rozhraní parametry nahrazeny příslušnými argumenty typu), kde – Metoda parametry typu jsou označeny ordinální Pozice zleva doprava.</span><span class="sxs-lookup"><span data-stu-id="d0561-312">When a generic method implicitly implements an interface method, the constraints given for each method type parameter must be equivalent in both declarations (after any interface type parameters are replaced with the appropriate type arguments), where method type parameters are identified by ordinal positions, left to right.</span></span>

<span data-ttu-id="d0561-313">Při obecné metody explicitně implementuje metodu rozhraní, ale bez omezení jsou povoleny v implementaci metody.</span><span class="sxs-lookup"><span data-stu-id="d0561-313">When a generic method explicitly implements an interface method, however, no constraints are allowed on the implementing method.</span></span> <span data-ttu-id="d0561-314">Místo toho se omezení dědí z rozhraní – metoda</span><span class="sxs-lookup"><span data-stu-id="d0561-314">Instead, the constraints are inherited from the interface method</span></span>

```csharp
interface I<A,B,C>
{
    void F<T>(T t) where T: A;
    void G<T>(T t) where T: B;
    void H<T>(T t) where T: C;
}

class C: I<object,C,string>
{
    public void F<T>(T t) {...}                    // Ok
    public void G<T>(T t) where T: C {...}         // Ok
    public void H<T>(T t) where T: string {...}    // Error
}
```

<span data-ttu-id="d0561-315">Metoda `C.F<T>` implicitně implementuje `I<object,C,string>.F<T>`.</span><span class="sxs-lookup"><span data-stu-id="d0561-315">The method `C.F<T>` implicitly implements `I<object,C,string>.F<T>`.</span></span> <span data-ttu-id="d0561-316">V takovém případě `C.F<T>` není požadováno (ani povolené) k určení omezení `T:object` od `object` je implicitní omezení pro všechny parametry typu.</span><span class="sxs-lookup"><span data-stu-id="d0561-316">In this case, `C.F<T>` is not required (nor permitted) to specify the constraint `T:object` since `object` is an implicit constraint on all type parameters.</span></span> <span data-ttu-id="d0561-317">Metoda `C.G<T>` implicitně implementuje `I<object,C,string>.G<T>` vzhledem k tomu, že omezení shodovat s hodnotami v rozhraní, po nahrazení parametrů typu rozhraní s odpovídající argumenty typu.</span><span class="sxs-lookup"><span data-stu-id="d0561-317">The method `C.G<T>` implicitly implements `I<object,C,string>.G<T>` because the constraints match those in the interface, after the interface type parameters are replaced with the corresponding type arguments.</span></span> <span data-ttu-id="d0561-318">Omezení pro metodu `C.H<T>` se o chybu, protože zapečetěné typy (`string` v tomto případě) nejde použít jako omezení.</span><span class="sxs-lookup"><span data-stu-id="d0561-318">The constraint for method `C.H<T>` is an error because sealed types (`string` in this case) cannot be used as constraints.</span></span> <span data-ttu-id="d0561-319">Vynechání omezení by také být chybu, protože omezení implementace metody implicitní rozhraní jsou vyžadovaný pro shodu.</span><span class="sxs-lookup"><span data-stu-id="d0561-319">Omitting the constraint would also be an error since constraints of implicit interface method implementations are required to match.</span></span> <span data-ttu-id="d0561-320">Proto není možné implicitně implementovat `I<object,C,string>.H<T>`.</span><span class="sxs-lookup"><span data-stu-id="d0561-320">Thus, it is impossible to implicitly implement `I<object,C,string>.H<T>`.</span></span> <span data-ttu-id="d0561-321">Tato metoda rozhraní je možné implementovat pouze pomocí implementace explicitního rozhraní člena:</span><span class="sxs-lookup"><span data-stu-id="d0561-321">This interface method can only be implemented using an explicit interface member implementation:</span></span>
```csharp
class C: I<object,C,string>
{
    ...

    public void H<U>(U u) where U: class {...}

    void I<object,C,string>.H<T>(T t) {
        string s = t;    // Ok
        H<T>(t);
    }
}
```

<span data-ttu-id="d0561-322">V tomto příkladu volá člen implementace explicitního rozhraní veřejnou metodu s výhradně slabšími omezeními.</span><span class="sxs-lookup"><span data-stu-id="d0561-322">In this example, the explicit interface member implementation invokes a public method having strictly weaker constraints.</span></span> <span data-ttu-id="d0561-323">Všimněte si, že přiřazení z `t` k `s` je platný od `T` dědí omezení `T:string`, i když toto omezení není anyAttribute ve zdrojovém kódu.</span><span class="sxs-lookup"><span data-stu-id="d0561-323">Note that the assignment from `t` to `s` is valid since `T` inherits a constraint of `T:string`, even though this constraint is not expressible in source code.</span></span>

### <a name="interface-mapping"></a><span data-ttu-id="d0561-324">Mapování rozhraní</span><span class="sxs-lookup"><span data-stu-id="d0561-324">Interface mapping</span></span>

<span data-ttu-id="d0561-325">Třídy nebo struktury, musíte zadat implementace členů rozhraní, které jsou uvedeny v seznamu základních tříd z dané třídy nebo struktury.</span><span class="sxs-lookup"><span data-stu-id="d0561-325">A class or struct must provide implementations of all members of the interfaces that are listed in the base class list of the class or struct.</span></span> <span data-ttu-id="d0561-326">Proces vyhledávání v implementující třída nebo struktura implementuje členy daného rozhraní se označuje jako ***mapování rozhraní***.</span><span class="sxs-lookup"><span data-stu-id="d0561-326">The process of locating implementations of interface members in an implementing class or struct is known as ***interface mapping***.</span></span>

<span data-ttu-id="d0561-327">Mapování rozhraní třídy nebo struktury `C` vyhledá implementace pro každého člena každé rozhraní, udávaná v seznamu základních tříd z `C`.</span><span class="sxs-lookup"><span data-stu-id="d0561-327">Interface mapping for a class or struct `C` locates an implementation for each member of each interface specified in the base class list of `C`.</span></span> <span data-ttu-id="d0561-328">Implementace členu rozhraní konkrétní `I.M`, kde `I` rozhraní, ve kterém je člen `M` je deklarována, je určen tím, že kontroluje každá třída nebo struktura `S`počínaje `C` a opakující se pro každou po sobě jdoucích základní třídu `C`, dokud nebude nalezen odpovídající:</span><span class="sxs-lookup"><span data-stu-id="d0561-328">The implementation of a particular interface member `I.M`, where `I` is the interface in which the member `M` is declared, is determined by examining each class or struct `S`, starting with `C` and repeating for each successive base class of `C`, until a match is located:</span></span>

*  <span data-ttu-id="d0561-329">Pokud `S` obsahuje prohlášení o implementace explicitního rozhraní člena, která odpovídá `I` a `M`, pak tento člen je provádění `I.M`.</span><span class="sxs-lookup"><span data-stu-id="d0561-329">If `S` contains a declaration of an explicit interface member implementation that matches `I` and `M`, then this member is the implementation of `I.M`.</span></span>
*  <span data-ttu-id="d0561-330">Jinak, pokud `S` obsahuje prohlášení o nestatická veřejný člen, který odpovídá `M`, pak tento člen je provádění `I.M`.</span><span class="sxs-lookup"><span data-stu-id="d0561-330">Otherwise, if `S` contains a declaration of a non-static public member that matches `M`, then this member is the implementation of `I.M`.</span></span> <span data-ttu-id="d0561-331">Pokud je více než jeden člen shody neurčené který člen je provádění `I.M`.</span><span class="sxs-lookup"><span data-stu-id="d0561-331">If more than one member matches, it is unspecified which member is the implementation of `I.M`.</span></span> <span data-ttu-id="d0561-332">Tato situace může nastat, pouze pokud `S` je konstruovaný typ. Pokud dva členy, jak je deklarován v obecném typu mít různé podpisy, ale argumenty typu provádět jejich podpisy identické.</span><span class="sxs-lookup"><span data-stu-id="d0561-332">This situation can only occur if `S` is a constructed type where the two members as declared in the generic type have different signatures, but the type arguments make their signatures identical.</span></span>

<span data-ttu-id="d0561-333">Chyba kompilace v případě implementace nejde najít pro všechny členy všechna rozhraní zadané v seznamu základních tříd z `C`.</span><span class="sxs-lookup"><span data-stu-id="d0561-333">A compile-time error occurs if implementations cannot be located for all members of all interfaces specified in the base class list of `C`.</span></span> <span data-ttu-id="d0561-334">Mějte na paměti, že členy rozhraní zahrnout tyto členy, které jsou zděděny ze základních rozhraní.</span><span class="sxs-lookup"><span data-stu-id="d0561-334">Note that the members of an interface include those members that are inherited from base interfaces.</span></span>

<span data-ttu-id="d0561-335">Pro účely mapování rozhraní, člen třídy `A` odpovídá člena rozhraní `B` při:</span><span class="sxs-lookup"><span data-stu-id="d0561-335">For purposes of interface mapping, a class member `A` matches an interface member `B` when:</span></span>

*  <span data-ttu-id="d0561-336">`A` a `B` jsou metody a název, typ, a seznam formálních parametrů `A` a `B` jsou identické.</span><span class="sxs-lookup"><span data-stu-id="d0561-336">`A` and `B` are methods, and the name, type, and formal parameter lists of `A` and `B` are identical.</span></span>
*  <span data-ttu-id="d0561-337">`A` a `B` jsou vlastnosti, název a typ `A` a `B` jsou identické, a `A` má stejné přístupové objekty jako `B` (`A` smí mít přistupující objekty další, pokud se nejedná o explicitní rozhraní implementace člena).</span><span class="sxs-lookup"><span data-stu-id="d0561-337">`A` and `B` are properties, the name and type of `A` and `B` are identical, and `A` has the same accessors as `B` (`A` is permitted to have additional accessors if it is not an explicit interface member implementation).</span></span>
*  <span data-ttu-id="d0561-338">`A` a `B` jsou události a název a typ `A` a `B` jsou identické.</span><span class="sxs-lookup"><span data-stu-id="d0561-338">`A` and `B` are events, and the name and type of `A` and `B` are identical.</span></span>
*  <span data-ttu-id="d0561-339">`A` a `B` indexery, je typ a formální parametr seznam `A` a `B` jsou identické, a `A` má stejné přístupové objekty jako `B` (`A` smí mít další přístupových objektů, pokud se nejedná explicitní implementace členu rozhraní).</span><span class="sxs-lookup"><span data-stu-id="d0561-339">`A` and `B` are indexers, the type and formal parameter lists of `A` and `B` are identical, and `A` has the same accessors as `B` (`A` is permitted to have additional accessors if it is not an explicit interface member implementation).</span></span>

<span data-ttu-id="d0561-340">Významné důsledky algoritmu mapování rozhraní jsou následující:</span><span class="sxs-lookup"><span data-stu-id="d0561-340">Notable implications of the interface mapping algorithm are:</span></span>

*  <span data-ttu-id="d0561-341">Implementace explicitního rozhraní členských modulů má přednost před ostatní členové ve stejné třídě nebo struktuře při určování člen třídy nebo struktury, který implementuje člena rozhraní.</span><span class="sxs-lookup"><span data-stu-id="d0561-341">Explicit interface member implementations take precedence over other members in the same class or struct when determining the class or struct member that implements an interface member.</span></span>
*  <span data-ttu-id="d0561-342">Neveřejné ani statické členy účastnit mapování rozhraní.</span><span class="sxs-lookup"><span data-stu-id="d0561-342">Neither non-public nor static members participate in interface mapping.</span></span>

<span data-ttu-id="d0561-343">V příkladu</span><span class="sxs-lookup"><span data-stu-id="d0561-343">In the example</span></span>
```csharp
interface ICloneable
{
    object Clone();
}

class C: ICloneable
{
    object ICloneable.Clone() {...}
    public object Clone() {...}
}
```
<span data-ttu-id="d0561-344">`ICloneable.Clone` členem `C` stane provádění `Clone` v `ICloneable` vzhledem k tomu, že člen implementace explicitního rozhraní přednost před ostatními členy.</span><span class="sxs-lookup"><span data-stu-id="d0561-344">the `ICloneable.Clone` member of `C` becomes the implementation of `Clone` in `ICloneable` because explicit interface member implementations take precedence over other members.</span></span>

<span data-ttu-id="d0561-345">Pokud třída nebo struktura implementuje dvě nebo více rozhraní, který obsahuje člena se stejným názvem, typem a typy parametrů, je možné mapovat každou z těchto členů rozhraní do jednoho člena třídy nebo struktury.</span><span class="sxs-lookup"><span data-stu-id="d0561-345">If a class or struct implements two or more interfaces containing a member with the same name, type, and parameter types, it is possible to map each of those interface members onto a single class or struct member.</span></span> <span data-ttu-id="d0561-346">Příklad</span><span class="sxs-lookup"><span data-stu-id="d0561-346">For example</span></span>
```csharp
interface IControl
{
    void Paint();
}

interface IForm
{
    void Paint();
}

class Page: IControl, IForm
{
    public void Paint() {...}
}
```

<span data-ttu-id="d0561-347">Tady `Paint` obě metody `IControl` a `IForm` jsou mapovány na `Paint` metoda ve `Page`.</span><span class="sxs-lookup"><span data-stu-id="d0561-347">Here, the `Paint` methods of both `IControl` and `IForm` are mapped onto the `Paint` method in `Page`.</span></span> <span data-ttu-id="d0561-348">To je samozřejmě také možné mít samostatné explicitní člen implementace rozhraní pro tyto dvě metody.</span><span class="sxs-lookup"><span data-stu-id="d0561-348">It is of course also possible to have separate explicit interface member implementations for the two methods.</span></span>

<span data-ttu-id="d0561-349">Pokud třída nebo struktura implementuje rozhraní, které obsahuje skryté členy, musí být některé členy nutně implementované prostřednictvím implementace explicitního rozhraní člena.</span><span class="sxs-lookup"><span data-stu-id="d0561-349">If a class or struct implements an interface that contains hidden members, then some members must necessarily be implemented through explicit interface member implementations.</span></span> <span data-ttu-id="d0561-350">Příklad</span><span class="sxs-lookup"><span data-stu-id="d0561-350">For example</span></span>
```csharp
interface IBase
{
    int P { get; }
}

interface IDerived: IBase
{
    new int P();
}
```

<span data-ttu-id="d0561-351">Implementace tohoto rozhraní by vyžadovaly alespoň jeden explicitní implementace členu rozhraní a by proveďte jednu z následujících forem</span><span class="sxs-lookup"><span data-stu-id="d0561-351">An implementation of this interface would require at least one explicit interface member implementation, and would take one of the following forms</span></span>
```csharp
class C: IDerived
{
    int IBase.P { get {...} }
    int IDerived.P() {...}
}

class C: IDerived
{
    public int P { get {...} }
    int IDerived.P() {...}
}

class C: IDerived
{
    int IBase.P { get {...} }
    public int P() {...}
}
```

<span data-ttu-id="d0561-352">Pokud třída implementuje více rozhraní, které mají stejný základní rozhraní, může být pouze jedna implementace základní rozhraní.</span><span class="sxs-lookup"><span data-stu-id="d0561-352">When a class implements multiple interfaces that have the same base interface, there can be only one implementation of the base interface.</span></span> <span data-ttu-id="d0561-353">V příkladu</span><span class="sxs-lookup"><span data-stu-id="d0561-353">In the example</span></span>
```csharp
interface IControl
{
    void Paint();
}

interface ITextBox: IControl
{
    void SetText(string text);
}

interface IListBox: IControl
{
    void SetItems(string[] items);
}

class ComboBox: IControl, ITextBox, IListBox
{
    void IControl.Paint() {...}
    void ITextBox.SetText(string text) {...}
    void IListBox.SetItems(string[] items) {...}
}
```
<span data-ttu-id="d0561-354">není možné mít odlišnou implementaci pro `IControl` s názvem v seznamu základních tříd, `IControl` děděné `ITextBox`a `IControl` děděné `IListBox`.</span><span class="sxs-lookup"><span data-stu-id="d0561-354">it is not possible to have separate implementations for the `IControl` named in the base class list, the `IControl` inherited by `ITextBox`, and the `IControl` inherited by `IListBox`.</span></span> <span data-ttu-id="d0561-355">Ve skutečnosti není potuchy samostatné identity pro tato rozhraní.</span><span class="sxs-lookup"><span data-stu-id="d0561-355">Indeed, there is no notion of a separate identity for these interfaces.</span></span> <span data-ttu-id="d0561-356">Místo toho implementace `ITextBox` a `IListBox` sdílet stejné provádění `IControl`, a `ComboBox` považován za jednoduše implementovat tři rozhraní `IControl`, `ITextBox`, a `IListBox`.</span><span class="sxs-lookup"><span data-stu-id="d0561-356">Rather, the implementations of `ITextBox` and `IListBox` share the same implementation of `IControl`, and `ComboBox` is simply considered to implement three interfaces, `IControl`, `ITextBox`, and `IListBox`.</span></span>

<span data-ttu-id="d0561-357">Členy základní třídy součástí mapování rozhraní.</span><span class="sxs-lookup"><span data-stu-id="d0561-357">The members of a base class participate in interface mapping.</span></span> <span data-ttu-id="d0561-358">V příkladu</span><span class="sxs-lookup"><span data-stu-id="d0561-358">In the example</span></span>
```csharp
interface Interface1
{
    void F();
}

class Class1
{
    public void F() {}
    public void G() {}
}

class Class2: Class1, Interface1
{
    new public void G() {}
}
```
<span data-ttu-id="d0561-359">Metoda `F` v `Class1` se používá v `Class2`vaší implementace `Interface1`.</span><span class="sxs-lookup"><span data-stu-id="d0561-359">the method `F` in `Class1` is used in `Class2`'s implementation of `Interface1`.</span></span>

### <a name="interface-implementation-inheritance"></a><span data-ttu-id="d0561-360">Dědičnost implementace rozhraní</span><span class="sxs-lookup"><span data-stu-id="d0561-360">Interface implementation inheritance</span></span>

<span data-ttu-id="d0561-361">Třída dědí všechny implementace rozhraní poskytovaných jejích základních tříd.</span><span class="sxs-lookup"><span data-stu-id="d0561-361">A class inherits all interface implementations provided by its base classes.</span></span>

<span data-ttu-id="d0561-362">Bez explicitně ***pokaždé znova implementovány*** rozhraní, odvozené třídy nelze žádným způsobem změnit mapování rozhraní dědí z jejích základních tříd.</span><span class="sxs-lookup"><span data-stu-id="d0561-362">Without explicitly ***re-implementing*** an interface, a derived class cannot in any way alter the interface mappings it inherits from its base classes.</span></span> <span data-ttu-id="d0561-363">Například v deklaracích</span><span class="sxs-lookup"><span data-stu-id="d0561-363">For example, in the declarations</span></span>
```csharp
interface IControl
{
    void Paint();
}

class Control: IControl
{
    public void Paint() {...}
}

class TextBox: Control
{
    new public void Paint() {...}
}
```
<span data-ttu-id="d0561-364">`Paint` metoda ve `TextBox` skryje `Paint` metoda ve `Control`, ale nezmění mapování `Control.Paint` do `IControl.Paint`a volání `Paint` prostřednictvím třídy instance a interface instance bude mít následujících efektů</span><span class="sxs-lookup"><span data-stu-id="d0561-364">the `Paint` method in `TextBox` hides the `Paint` method in `Control`, but it does not alter the mapping of `Control.Paint` onto `IControl.Paint`, and calls to `Paint` through class instances and interface instances will have the following effects</span></span>
```csharp
Control c = new Control();
TextBox t = new TextBox();
IControl ic = c;
IControl it = t;
c.Paint();            // invokes Control.Paint();
t.Paint();            // invokes TextBox.Paint();
ic.Paint();           // invokes Control.Paint();
it.Paint();           // invokes Control.Paint();
```

<span data-ttu-id="d0561-365">Ale když metody rozhraní je mapována na virtuální metodu ve třídě, je možné pro odvozeným třídám přepsat virtuální metodu a změňte implementaci rozhraní.</span><span class="sxs-lookup"><span data-stu-id="d0561-365">However, when an interface method is mapped onto a virtual method in a class, it is possible for derived classes to override the virtual method and alter the implementation of the interface.</span></span> <span data-ttu-id="d0561-366">Například přepsání deklarace výše na</span><span class="sxs-lookup"><span data-stu-id="d0561-366">For example, rewriting the declarations above to</span></span>
```csharp
interface IControl
{
    void Paint();
}

class Control: IControl
{
    public virtual void Paint() {...}
}

class TextBox: Control
{
    public override void Paint() {...}
}
```
<span data-ttu-id="d0561-367">se teď mělo být dodržen následujících efektů</span><span class="sxs-lookup"><span data-stu-id="d0561-367">the following effects will now be observed</span></span>
```csharp
Control c = new Control();
TextBox t = new TextBox();
IControl ic = c;
IControl it = t;
c.Paint();            // invokes Control.Paint();
t.Paint();            // invokes TextBox.Paint();
ic.Paint();           // invokes Control.Paint();
it.Paint();           // invokes TextBox.Paint();
```

<span data-ttu-id="d0561-368">Vzhledem k tomu, že člen implementace explicitního rozhraní nejde použít deklaraci virtuální, není možné přepsat explicitní implementace členu rozhraní.</span><span class="sxs-lookup"><span data-stu-id="d0561-368">Since explicit interface member implementations cannot be declared virtual, it is not possible to override an explicit interface member implementation.</span></span> <span data-ttu-id="d0561-369">Ale je zcela platná pro člena implementace explicitního rozhraní volat jinou metodu, a, jiné metody mohou být deklarovány virtuální umožňující odvozených třídách přepsat.</span><span class="sxs-lookup"><span data-stu-id="d0561-369">However, it is perfectly valid for an explicit interface member implementation to call another method, and that other method can be declared virtual to allow derived classes to override it.</span></span> <span data-ttu-id="d0561-370">Příklad</span><span class="sxs-lookup"><span data-stu-id="d0561-370">For example</span></span>
```csharp
interface IControl
{
    void Paint();
}

class Control: IControl
{
    void IControl.Paint() { PaintControl(); }
    protected virtual void PaintControl() {...}
}

class TextBox: Control
{
    protected override void PaintControl() {...}
}
```

<span data-ttu-id="d0561-371">Tady třídy odvozené z `Control` můžete specialize provádění `IControl.Paint` tak, že přepíšete `PaintControl` metody.</span><span class="sxs-lookup"><span data-stu-id="d0561-371">Here, classes derived from `Control` can specialize the implementation of `IControl.Paint` by overriding the `PaintControl` method.</span></span>

### <a name="interface-re-implementation"></a><span data-ttu-id="d0561-372">Opětovná implementace rozhraní</span><span class="sxs-lookup"><span data-stu-id="d0561-372">Interface re-implementation</span></span>

<span data-ttu-id="d0561-373">Třída, která dědí implementaci rozhraní povoleno ***znovu implementovat*** rozhraní zahrnutím v seznamu základních tříd.</span><span class="sxs-lookup"><span data-stu-id="d0561-373">A class that inherits an interface implementation is permitted to ***re-implement*** the interface by including it in the base class list.</span></span>

<span data-ttu-id="d0561-374">Opětovná implementace rozhraní se řídí přesně stejná rozhraní mapování pravidla, počáteční implementace rozhraní.</span><span class="sxs-lookup"><span data-stu-id="d0561-374">A re-implementation of an interface follows exactly the same interface mapping rules as an initial implementation of an interface.</span></span> <span data-ttu-id="d0561-375">Zděděné rozhraní mapování tedy nemá žádný vliv na mapování rozhraní stanovit opětovná implementace rozhraní.</span><span class="sxs-lookup"><span data-stu-id="d0561-375">Thus, the inherited interface mapping has no effect whatsoever on the interface mapping established for the re-implementation of the interface.</span></span> <span data-ttu-id="d0561-376">Například v deklaracích</span><span class="sxs-lookup"><span data-stu-id="d0561-376">For example, in the declarations</span></span>
```csharp
interface IControl
{
    void Paint();
}

class Control: IControl
{
    void IControl.Paint() {...}
}

class MyControl: Control, IControl
{
    public void Paint() {}
}
```
<span data-ttu-id="d0561-377">fakt, který `Control` mapuje `IControl.Paint` do `Control.IControl.Paint` nemá vliv na opětovná implementace v `MyControl`, namapovaná `IControl.Paint` na `MyControl.Paint`.</span><span class="sxs-lookup"><span data-stu-id="d0561-377">the fact that `Control` maps `IControl.Paint` onto `Control.IControl.Paint` doesn't affect the re-implementation in `MyControl`, which maps `IControl.Paint` onto `MyControl.Paint`.</span></span>

<span data-ttu-id="d0561-378">Zděděná deklarace veřejného člena a člen zděděný explicitní rozhraní, které deklarace účastnit procesu rozhraní mapování pro znovu implementovaných rozhraní.</span><span class="sxs-lookup"><span data-stu-id="d0561-378">Inherited public member declarations and inherited explicit interface member declarations participate in the interface mapping process for re-implemented interfaces.</span></span> <span data-ttu-id="d0561-379">Příklad</span><span class="sxs-lookup"><span data-stu-id="d0561-379">For example</span></span>
```csharp
interface IMethods
{
    void F();
    void G();
    void H();
    void I();
}

class Base: IMethods
{
    void IMethods.F() {}
    void IMethods.G() {}
    public void H() {}
    public void I() {}
}

class Derived: Base, IMethods
{
    public void F() {}
    void IMethods.H() {}
}
```

<span data-ttu-id="d0561-380">Tady, provádění `IMethods` v `Derived` mapuje metody rozhraní do `Derived.F`, `Base.IMethods.G`, `Derived.IMethods.H`, a `Base.I`.</span><span class="sxs-lookup"><span data-stu-id="d0561-380">Here, the implementation of `IMethods` in `Derived` maps the interface methods onto `Derived.F`, `Base.IMethods.G`, `Derived.IMethods.H`, and `Base.I`.</span></span>

<span data-ttu-id="d0561-381">Pokud třída implementuje rozhraní, je implicitně také implementuje všechna rozhraní základní rozhraní.</span><span class="sxs-lookup"><span data-stu-id="d0561-381">When a class implements an interface, it implicitly also implements all of that interface's base interfaces.</span></span> <span data-ttu-id="d0561-382">Obdobně opětovná implementace rozhraní je také implicitně opětovná implementace všech základních rozhraní rozhraní.</span><span class="sxs-lookup"><span data-stu-id="d0561-382">Likewise, a re-implementation of an interface is also implicitly a re-implementation of all of the interface's base interfaces.</span></span> <span data-ttu-id="d0561-383">Příklad</span><span class="sxs-lookup"><span data-stu-id="d0561-383">For example</span></span>
```csharp
interface IBase
{
    void F();
}

interface IDerived: IBase
{
    void G();
}

class C: IDerived
{
    void IBase.F() {...}
    void IDerived.G() {...}
}

class D: C, IDerived
{
    public void F() {...}
    public void G() {...}
}
```

<span data-ttu-id="d0561-384">Tady, opětovná implementace `IDerived` také znovu implementuje `IBase`, mapování `IBase.F` na `D.F`.</span><span class="sxs-lookup"><span data-stu-id="d0561-384">Here, the re-implementation of `IDerived` also re-implements `IBase`, mapping `IBase.F` onto `D.F`.</span></span>

### <a name="abstract-classes-and-interfaces"></a><span data-ttu-id="d0561-385">Abstraktní třídy a rozhraní</span><span class="sxs-lookup"><span data-stu-id="d0561-385">Abstract classes and interfaces</span></span>

<span data-ttu-id="d0561-386">Stejně jako neabstraktní třídy musíte zadat abstraktní třída implementace všech členů, které jsou uvedeny v seznamu základních tříd třídy rozhraní.</span><span class="sxs-lookup"><span data-stu-id="d0561-386">Like a non-abstract class, an abstract class must provide implementations of all members of the interfaces that are listed in the base class list of the class.</span></span> <span data-ttu-id="d0561-387">Abstraktní třídy je však povoleno mapování rozhraní metod na abstraktní metody.</span><span class="sxs-lookup"><span data-stu-id="d0561-387">However, an abstract class is permitted to map interface methods onto abstract methods.</span></span> <span data-ttu-id="d0561-388">Příklad</span><span class="sxs-lookup"><span data-stu-id="d0561-388">For example</span></span>
```csharp
interface IMethods
{
    void F();
    void G();
}

abstract class C: IMethods
{
    public abstract void F();
    public abstract void G();
}
```

<span data-ttu-id="d0561-389">Tady, provádění `IMethods` mapuje `F` a `G` na abstraktní metody, které musí být přepsána nastaveními v neabstraktní třídy, které jsou odvozeny z `C`.</span><span class="sxs-lookup"><span data-stu-id="d0561-389">Here, the implementation of `IMethods` maps `F` and `G` onto abstract methods, which must be overridden in non-abstract classes that derive from `C`.</span></span>

<span data-ttu-id="d0561-390">Všimněte si, že člen implementace explicitního rozhraní nemohou být abstraktní, ale implementace explicitního rozhraní členských samozřejmě povoleno volat abstraktní metody.</span><span class="sxs-lookup"><span data-stu-id="d0561-390">Note that explicit interface member implementations cannot be abstract, but explicit interface member implementations are of course permitted to call abstract methods.</span></span> <span data-ttu-id="d0561-391">Příklad</span><span class="sxs-lookup"><span data-stu-id="d0561-391">For example</span></span>
```csharp
interface IMethods
{
    void F();
    void G();
}

abstract class C: IMethods
{
    void IMethods.F() { FF(); }
    void IMethods.G() { GG(); }
    protected abstract void FF();
    protected abstract void GG();
}
```

<span data-ttu-id="d0561-392">Tady, neabstraktní třídy, které jsou odvozeny z `C` by bylo zapotřebí k přepsání `FF` a `GG`, díky tomu Skutečná implementace `IMethods`.</span><span class="sxs-lookup"><span data-stu-id="d0561-392">Here, non-abstract classes that derive from `C` would be required to override `FF` and `GG`, thus providing the actual implementation of `IMethods`.</span></span>
