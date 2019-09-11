---
ms.openlocfilehash: 2c87cafb8591b9dff2aa517b65af80ab263c7faa
ms.sourcegitcommit: 7f7fc6e9e195e51b7ff8229aeaa70aa9fbbb63cb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/10/2019
ms.locfileid: "70876904"
---
# <a name="classes"></a><span data-ttu-id="479b6-101">Třídy</span><span class="sxs-lookup"><span data-stu-id="479b6-101">Classes</span></span>

<span data-ttu-id="479b6-102">Třída je datová struktura, která může obsahovat datové členy (konstanty a pole), členy funkce (metody, vlastnosti, události, indexery, operátory, konstruktory instancí, destruktory a statické konstruktory) a vnořené typy.</span><span class="sxs-lookup"><span data-stu-id="479b6-102">A class is a data structure that may contain data members (constants and fields), function members (methods, properties, events, indexers, operators, instance constructors, destructors and static constructors), and nested types.</span></span> <span data-ttu-id="479b6-103">Typy tříd podporují dědičnost, mechanismus, který může odvozená třída zvětšit a specializovat základní třídu.</span><span class="sxs-lookup"><span data-stu-id="479b6-103">Class types support inheritance, a mechanism whereby a derived class can extend and specialize a base class.</span></span>

## <a name="class-declarations"></a><span data-ttu-id="479b6-104">Deklarace tříd</span><span class="sxs-lookup"><span data-stu-id="479b6-104">Class declarations</span></span>

<span data-ttu-id="479b6-105">*Class_declaration* je *type_declaration* ([deklarace typu](namespaces.md#type-declarations)), který deklaruje novou třídu.</span><span class="sxs-lookup"><span data-stu-id="479b6-105">A *class_declaration* is a *type_declaration* ([Type declarations](namespaces.md#type-declarations)) that declares a new class.</span></span>

```antlr
class_declaration
    : attributes? class_modifier* 'partial'? 'class' identifier type_parameter_list?
      class_base? type_parameter_constraints_clause* class_body ';'?
    ;
```

<span data-ttu-id="479b6-106">*Class_declaration* se skládá z volitelné sady *atributů* ([atributů](attributes.md)) následovaný volitelnou sadou *class_modifier*s ([modifikátory třídy](classes.md#class-modifiers)) následovaný volitelným `partial` modifikátorem následovaným parametrem klíčové `class` slovo a *identifikátor* , který pojmenovává třídu, následovaný volitelnou *type_parameter_list* ([parametry typu](classes.md#type-parameters)) následovaný nepovinnou specifikací *class_base* ([základní specifikace třídy ](classes.md#class-base-specification)) následovaný volitelnou sadou *type_parameter_constraints_clause*s ([omezeními parametrů typu](classes.md#type-parameter-constraints)) následovaným *class_body* ([tělo třídy](classes.md#class-body)), volitelně následovaný středníkem.</span><span class="sxs-lookup"><span data-stu-id="479b6-106">A *class_declaration* consists of an optional set of *attributes* ([Attributes](attributes.md)), followed by an optional set of *class_modifier*s ([Class modifiers](classes.md#class-modifiers)), followed by an optional `partial` modifier, followed by the keyword `class` and an *identifier* that names the class, followed by an optional *type_parameter_list* ([Type parameters](classes.md#type-parameters)), followed by an optional *class_base* specification ([Class base specification](classes.md#class-base-specification)) , followed by an optional set of *type_parameter_constraints_clause*s ([Type parameter constraints](classes.md#type-parameter-constraints)), followed by a *class_body* ([Class body](classes.md#class-body)), optionally followed by a semicolon.</span></span>

<span data-ttu-id="479b6-107">Deklarace třídy nemůže poskytovat *type_parameter_constraints_clause*s, pokud zároveň neposkytuje *type_parameter_list*.</span><span class="sxs-lookup"><span data-stu-id="479b6-107">A class declaration cannot supply *type_parameter_constraints_clause*s unless it also supplies a *type_parameter_list*.</span></span>

<span data-ttu-id="479b6-108">Deklarace třídy, která poskytuje *type_parameter_list* , je ***Obecná deklarace třídy***.</span><span class="sxs-lookup"><span data-stu-id="479b6-108">A class declaration that supplies a *type_parameter_list* is a ***generic class declaration***.</span></span> <span data-ttu-id="479b6-109">Kromě toho jakákoli třída vnořená uvnitř deklarace obecné třídy nebo deklarace obecné struktury je sám deklarací obecné třídy, protože k vytvoření vytvořeného typu musí být zadány parametry typu pro nadřazený typ.</span><span class="sxs-lookup"><span data-stu-id="479b6-109">Additionally, any class nested inside a generic class declaration or a generic struct declaration is itself a generic class declaration, since type parameters for the containing type must be supplied to create a constructed type.</span></span>

### <a name="class-modifiers"></a><span data-ttu-id="479b6-110">Modifikátory třídy</span><span class="sxs-lookup"><span data-stu-id="479b6-110">Class modifiers</span></span>

<span data-ttu-id="479b6-111">*Class_declaration* může volitelně zahrnovat posloupnost modifikátorů třídy:</span><span class="sxs-lookup"><span data-stu-id="479b6-111">A *class_declaration* may optionally include a sequence of class modifiers:</span></span>

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

<span data-ttu-id="479b6-112">Jedná se o chybu při kompilaci, aby se stejný modifikátor zobrazoval víckrát v deklaraci třídy.</span><span class="sxs-lookup"><span data-stu-id="479b6-112">It is a compile-time error for the same modifier to appear multiple times in a class declaration.</span></span>

<span data-ttu-id="479b6-113">`new` Modifikátor je povolen pro vnořené třídy.</span><span class="sxs-lookup"><span data-stu-id="479b6-113">The `new` modifier is permitted on nested classes.</span></span> <span data-ttu-id="479b6-114">Určuje, že Třída skrývá zděděného člena se stejným názvem, jak je popsáno v [novém modifikátoru](classes.md#the-new-modifier).</span><span class="sxs-lookup"><span data-stu-id="479b6-114">It specifies that the class hides an inherited member by the same name, as described in [The new modifier](classes.md#the-new-modifier).</span></span> <span data-ttu-id="479b6-115">Jedná se o chybu `new` při kompilaci, aby se modifikátor zobrazoval v deklaraci třídy, která není vnořená deklarace třídy.</span><span class="sxs-lookup"><span data-stu-id="479b6-115">It is a compile-time error for the `new` modifier to appear on a class declaration that is not a nested class declaration.</span></span>

<span data-ttu-id="479b6-116">Modifikátory `protected` ,,`internal` a`private`řídípřístupnosttřídy. `public`</span><span class="sxs-lookup"><span data-stu-id="479b6-116">The `public`, `protected`, `internal`, and `private` modifiers control the accessibility of the class.</span></span> <span data-ttu-id="479b6-117">V závislosti na kontextu, ve kterém se vyskytuje deklarace třídy, nemusí být některé z těchto modifikátorů povoleny ([deklarovaná přístupnost](basic-concepts.md#declared-accessibility)).</span><span class="sxs-lookup"><span data-stu-id="479b6-117">Depending on the context in which the class declaration occurs, some of these modifiers may not be permitted ([Declared accessibility](basic-concepts.md#declared-accessibility)).</span></span>

<span data-ttu-id="479b6-118">Modifikátory a`static`jsou popsány v následujících částech. `sealed` `abstract`</span><span class="sxs-lookup"><span data-stu-id="479b6-118">The `abstract`, `sealed` and `static` modifiers are discussed in the following sections.</span></span>

#### <a name="abstract-classes"></a><span data-ttu-id="479b6-119">Abstraktní třídy</span><span class="sxs-lookup"><span data-stu-id="479b6-119">Abstract classes</span></span>

<span data-ttu-id="479b6-120">`abstract` Modifikátor slouží k označení toho, že třída je nekompletní a že je určena pro použití pouze jako základní třída.</span><span class="sxs-lookup"><span data-stu-id="479b6-120">The `abstract` modifier is used to indicate that a class is incomplete and that it is intended to be used only as a base class.</span></span> <span data-ttu-id="479b6-121">Abstraktní třída se od neabstraktní třídy liší následujícími způsoby:</span><span class="sxs-lookup"><span data-stu-id="479b6-121">An abstract class differs from a non-abstract class in the following ways:</span></span>

*  <span data-ttu-id="479b6-122">Nelze vytvořit instanci abstraktní třídy přímo a jedná se o chybu při kompilaci pro použití `new` operátoru pro abstraktní třídu.</span><span class="sxs-lookup"><span data-stu-id="479b6-122">An abstract class cannot be instantiated directly, and it is a compile-time error to use the `new` operator on an abstract class.</span></span> <span data-ttu-id="479b6-123">I když je možné mít proměnné a hodnoty, jejichž typy kompilace jsou abstraktní, takové proměnné a hodnoty budou nutně buď `null` nebo obsahovat odkazy na instance neabstraktních tříd odvozených z abstraktních typů.</span><span class="sxs-lookup"><span data-stu-id="479b6-123">While it is possible to have variables and values whose compile-time types are abstract, such variables and values will necessarily either be `null` or contain references to instances of non-abstract classes derived from the abstract types.</span></span>
*  <span data-ttu-id="479b6-124">Abstraktní třída je povolena (ale není vyžadována), aby obsahovala abstraktní členy.</span><span class="sxs-lookup"><span data-stu-id="479b6-124">An abstract class is permitted (but not required) to contain abstract members.</span></span>
*  <span data-ttu-id="479b6-125">Abstraktní třída nemůže být zapečetěná.</span><span class="sxs-lookup"><span data-stu-id="479b6-125">An abstract class cannot be sealed.</span></span>

<span data-ttu-id="479b6-126">Pokud je Neabstraktní třída odvozena z abstraktní třídy, Neabstraktní třída musí zahrnovat skutečné implementace všech zděděných abstraktních členů, čímž se přepsaly tyto abstraktní členy.</span><span class="sxs-lookup"><span data-stu-id="479b6-126">When a non-abstract class is derived from an abstract class, the non-abstract class must include actual implementations of all inherited abstract members, thereby overriding those abstract members.</span></span> <span data-ttu-id="479b6-127">V příkladu</span><span class="sxs-lookup"><span data-stu-id="479b6-127">In the example</span></span>
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
<span data-ttu-id="479b6-128">abstraktní třída `A` zavádí abstraktní metodu `F`.</span><span class="sxs-lookup"><span data-stu-id="479b6-128">the abstract class `A` introduces an abstract method `F`.</span></span> <span data-ttu-id="479b6-129">Třída `B` zavádí další metodu `G`, ale vzhledem k tomu, že neposkytuje implementaci `F`, `B` musí být také deklarována jako abstraktní.</span><span class="sxs-lookup"><span data-stu-id="479b6-129">Class `B` introduces an additional method `G`, but since it doesn't provide an implementation of `F`, `B` must also be declared abstract.</span></span> <span data-ttu-id="479b6-130">`C` Přepisuje`F` třídu a poskytuje skutečnou implementaci.</span><span class="sxs-lookup"><span data-stu-id="479b6-130">Class `C` overrides `F` and provides an actual implementation.</span></span> <span data-ttu-id="479b6-131">Vzhledem k tomu, že v `C`, nejsou k dispozici žádní abstraktní členové, `C` je povolen (ale není vyžadován) pro neabstraktní.</span><span class="sxs-lookup"><span data-stu-id="479b6-131">Since there are no abstract members in `C`, `C` is permitted (but not required) to be non-abstract.</span></span>

#### <a name="sealed-classes"></a><span data-ttu-id="479b6-132">Zapečetěné třídy</span><span class="sxs-lookup"><span data-stu-id="479b6-132">Sealed classes</span></span>

<span data-ttu-id="479b6-133">`sealed` Modifikátor slouží k zabránění odvození z třídy.</span><span class="sxs-lookup"><span data-stu-id="479b6-133">The `sealed` modifier is used to prevent derivation from a class.</span></span> <span data-ttu-id="479b6-134">K chybě při kompilaci dojde, pokud je zapečetěná třída zadána jako základní třída jiné třídy.</span><span class="sxs-lookup"><span data-stu-id="479b6-134">A compile-time error occurs if a sealed class is specified as the base class of another class.</span></span>

<span data-ttu-id="479b6-135">Zapečetěná třída nemůže být také abstraktní třída.</span><span class="sxs-lookup"><span data-stu-id="479b6-135">A sealed class cannot also be an abstract class.</span></span>

<span data-ttu-id="479b6-136">`sealed` Modifikátor se používá hlavně k zabránění nezamýšlenému odvození, ale také umožňuje určité optimalizace za běhu.</span><span class="sxs-lookup"><span data-stu-id="479b6-136">The `sealed` modifier is primarily used to prevent unintended derivation, but it also enables certain run-time optimizations.</span></span> <span data-ttu-id="479b6-137">Konkrétně vzhledem k tomu, že zapečetěná třída je známa tak, že nikdy nemá žádné odvozené třídy, je možné transformovat vyvolání členů virtuální funkce na zapečetěných instancích třídy na nevirtuální vyvolání.</span><span class="sxs-lookup"><span data-stu-id="479b6-137">In particular, because a sealed class is known to never have any derived classes, it is possible to transform virtual function member invocations on sealed class instances into non-virtual invocations.</span></span>

#### <a name="static-classes"></a><span data-ttu-id="479b6-138">Statické třídy</span><span class="sxs-lookup"><span data-stu-id="479b6-138">Static classes</span></span>

<span data-ttu-id="479b6-139">Modifikátor slouží k označení třídy deklarované jako ***statické třídy.*** `static`</span><span class="sxs-lookup"><span data-stu-id="479b6-139">The `static` modifier is used to mark the class being declared as a ***static class***.</span></span> <span data-ttu-id="479b6-140">Nelze vytvořit instanci statické třídy, nelze ji použít jako typ a může obsahovat pouze statické členy.</span><span class="sxs-lookup"><span data-stu-id="479b6-140">A static class cannot be instantiated, cannot be used as a type and can contain only static members.</span></span> <span data-ttu-id="479b6-141">Pouze statická třída může obsahovat deklarace rozšiřujících metod ([metod rozšíření](classes.md#extension-methods)).</span><span class="sxs-lookup"><span data-stu-id="479b6-141">Only a static class can contain declarations of extension methods ([Extension methods](classes.md#extension-methods)).</span></span>

<span data-ttu-id="479b6-142">Deklarace statické třídy podléhá následujícím omezením:</span><span class="sxs-lookup"><span data-stu-id="479b6-142">A static class declaration is subject to the following restrictions:</span></span>

*  <span data-ttu-id="479b6-143">Statická třída nesmí obsahovat `sealed` modifikátor or. `abstract`</span><span class="sxs-lookup"><span data-stu-id="479b6-143">A static class may not include a `sealed` or `abstract` modifier.</span></span> <span data-ttu-id="479b6-144">Upozorňujeme však, že vzhledem k tomu, že statická třída nemůže být vytvořena nebo odvozena z, se chová, jako by byla zapečetěná i abstraktní.</span><span class="sxs-lookup"><span data-stu-id="479b6-144">Note, however, that since a static class cannot be instantiated or derived from, it behaves as if it was both sealed and abstract.</span></span>
*  <span data-ttu-id="479b6-145">Statická třída nesmí obsahovat specifikaci *class_base* ([základní specifikace třídy](classes.md#class-base-specification)) a nemůže explicitně zadat základní třídu nebo seznam implementovaných rozhraní.</span><span class="sxs-lookup"><span data-stu-id="479b6-145">A static class may not include a *class_base* specification ([Class base specification](classes.md#class-base-specification)) and cannot explicitly specify a base class or a list of implemented interfaces.</span></span> <span data-ttu-id="479b6-146">Statická třída implicitně dědí z typu `object`.</span><span class="sxs-lookup"><span data-stu-id="479b6-146">A static class implicitly inherits from type `object`.</span></span>
*  <span data-ttu-id="479b6-147">Statická třída může obsahovat pouze statické členy ([statické a členy instance](classes.md#static-and-instance-members)).</span><span class="sxs-lookup"><span data-stu-id="479b6-147">A static class can only contain static members ([Static and instance members](classes.md#static-and-instance-members)).</span></span> <span data-ttu-id="479b6-148">Všimněte si, že konstanty a vnořené typy jsou klasifikovány jako statické členy.</span><span class="sxs-lookup"><span data-stu-id="479b6-148">Note that constants and nested types are classified as static members.</span></span>
*  <span data-ttu-id="479b6-149">Statická třída nemůže mít členy s `protected` nebo `protected internal` deklarovanou přístupností.</span><span class="sxs-lookup"><span data-stu-id="479b6-149">A static class cannot have members with `protected` or `protected internal` declared accessibility.</span></span>

<span data-ttu-id="479b6-150">Jedná se o chybu při kompilaci, která porušuje některá z těchto omezení.</span><span class="sxs-lookup"><span data-stu-id="479b6-150">It is a compile-time error to violate any of these restrictions.</span></span>

<span data-ttu-id="479b6-151">Statická třída nemá žádné konstruktory instancí.</span><span class="sxs-lookup"><span data-stu-id="479b6-151">A static class has no instance constructors.</span></span> <span data-ttu-id="479b6-152">Není možné deklarovat konstruktor instance ve statické třídě a není k dispozici žádný výchozí konstruktor instance ([výchozí konstruktory](classes.md#default-constructors)) pro statickou třídu.</span><span class="sxs-lookup"><span data-stu-id="479b6-152">It is not possible to declare an instance constructor in a static class, and no default instance constructor ([Default constructors](classes.md#default-constructors)) is provided for a static class.</span></span>

<span data-ttu-id="479b6-153">Členové statické třídy nejsou automaticky statický a deklarace členů musí explicitně obsahovat `static` modifikátor (s výjimkou konstant a vnořených typů).</span><span class="sxs-lookup"><span data-stu-id="479b6-153">The members of a static class are not automatically static, and the member declarations must explicitly include a `static` modifier (except for constants and nested types).</span></span> <span data-ttu-id="479b6-154">Pokud je třída vnořena do statické vnější třídy, vnořená třída není statickou třídou, pokud explicitně neobsahuje `static` modifikátor.</span><span class="sxs-lookup"><span data-stu-id="479b6-154">When a class is nested within a static outer class, the nested class is not a static class unless it explicitly includes a `static` modifier.</span></span>

<span data-ttu-id="479b6-155">__Odkazování na typy statických tříd__</span><span class="sxs-lookup"><span data-stu-id="479b6-155">__Referencing static class types__</span></span>

<span data-ttu-id="479b6-156">*Namespace_or_type_name* ([obor názvů a názvy typů](basic-concepts.md#namespace-and-type-names)) je povolený odkazování na statickou třídu, pokud</span><span class="sxs-lookup"><span data-stu-id="479b6-156">A *namespace_or_type_name* ([Namespace and type names](basic-concepts.md#namespace-and-type-names)) is permitted to reference a static class if</span></span>

*  <span data-ttu-id="479b6-157">*Namespace_or_type_name* `T` je v namespace_or_type_name formuláře nebo `T.I`</span><span class="sxs-lookup"><span data-stu-id="479b6-157">The *namespace_or_type_name* is the `T` in a *namespace_or_type_name* of the form `T.I`, or</span></span>
*  <span data-ttu-id="479b6-158">*Namespace_or_type_name* `T` je v *typeof_expression* ([seznam argumentů](expressions.md#argument-lists)1) formuláře `typeof(T)`.</span><span class="sxs-lookup"><span data-stu-id="479b6-158">The *namespace_or_type_name* is the `T` in a *typeof_expression* ([Argument lists](expressions.md#argument-lists)1) of the form `typeof(T)`.</span></span>

<span data-ttu-id="479b6-159">*Primary_expression* ([Členové funkce](expressions.md#function-members)) mají povolen odkaz na statickou třídu, pokud</span><span class="sxs-lookup"><span data-stu-id="479b6-159">A *primary_expression* ([Function members](expressions.md#function-members)) is permitted to reference a static class if</span></span>

*  <span data-ttu-id="479b6-160">*Primary_expression* `E` je v *member_access* ([Kontrola dynamického přetěžování při kompilaci](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) formuláře `E.I`.</span><span class="sxs-lookup"><span data-stu-id="479b6-160">The *primary_expression* is the `E` in a *member_access* ([Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) of the form `E.I`.</span></span>

<span data-ttu-id="479b6-161">V jakémkoli jiném kontextu se jedná o chybu při kompilaci, která odkazuje na statickou třídu.</span><span class="sxs-lookup"><span data-stu-id="479b6-161">In any other context it is a compile-time error to reference a static class.</span></span> <span data-ttu-id="479b6-162">Například se jedná o chybu pro statickou třídu, která bude použita jako základní třída, typ prvku ([vnořené typy](classes.md#nested-types)) člena, argument obecného typu nebo omezení parametru typu.</span><span class="sxs-lookup"><span data-stu-id="479b6-162">For example, it is an error for a static class to be used as a base class, a constituent type ([Nested types](classes.md#nested-types)) of a member, a generic type argument, or a type parameter constraint.</span></span> <span data-ttu-id="479b6-163">Stejně tak statická třída nemůže být použita v typu pole, typu ukazatele `new` , výrazu, výrazu přetypování `is` , výrazu `sizeof` , `as` výrazu, výrazu nebo výrazu výchozí hodnoty.</span><span class="sxs-lookup"><span data-stu-id="479b6-163">Likewise, a static class cannot be used in an array type, a pointer type, a `new` expression, a cast expression, an `is` expression, an `as` expression, a `sizeof` expression, or a default value expression.</span></span>

### <a name="partial-modifier"></a><span data-ttu-id="479b6-164">Částečný modifikátor</span><span class="sxs-lookup"><span data-stu-id="479b6-164">Partial modifier</span></span>

<span data-ttu-id="479b6-165">Modifikátor slouží k označení, že toto class_declaration je částečná deklarace typu. `partial`</span><span class="sxs-lookup"><span data-stu-id="479b6-165">The `partial` modifier is used to indicate that this *class_declaration* is a partial type declaration.</span></span> <span data-ttu-id="479b6-166">Vícenásobná deklarace částečného typu se stejným názvem v rámci ohraničujícího oboru názvů nebo deklarace typu jsou zkombinovány o jednu deklaraci typu podle pravidel zadaných v [částečných typech](classes.md#partial-types).</span><span class="sxs-lookup"><span data-stu-id="479b6-166">Multiple partial type declarations with the same name within an enclosing namespace or type declaration combine to form one type declaration, following the rules specified in [Partial types](classes.md#partial-types).</span></span>

<span data-ttu-id="479b6-167">Deklarace třídy distribuované přes samostatné segmenty textu programu může být užitečná, pokud jsou tyto segmenty vyráběny nebo udržovány v různých kontextech.</span><span class="sxs-lookup"><span data-stu-id="479b6-167">Having the declaration of a class distributed over separate segments of program text can be useful if these segments are produced or maintained in different contexts.</span></span> <span data-ttu-id="479b6-168">Například jedna část deklarace třídy může být vygenerována počítačem, zatímco druhá je ručně vytvořen.</span><span class="sxs-lookup"><span data-stu-id="479b6-168">For instance, one part of a class declaration may be machine generated, whereas the other is manually authored.</span></span> <span data-ttu-id="479b6-169">Textové oddělení těchto dvou brání aktualizacím, které jsou v konfliktu s aktualizacemi.</span><span class="sxs-lookup"><span data-stu-id="479b6-169">Textual separation of the two prevents updates by one from conflicting with updates by the other.</span></span>

### <a name="type-parameters"></a><span data-ttu-id="479b6-170">Parametry typu</span><span class="sxs-lookup"><span data-stu-id="479b6-170">Type parameters</span></span>

<span data-ttu-id="479b6-171">Parametr typu je jednoduchý identifikátor, který označuje zástupný symbol pro zadaný argument typu pro vytvoření konstruovaného typu.</span><span class="sxs-lookup"><span data-stu-id="479b6-171">A type parameter is a simple identifier that denotes a placeholder for a type argument supplied to create a constructed type.</span></span> <span data-ttu-id="479b6-172">Parametr typu je formální zástupný symbol pro typ, který bude dodán později.</span><span class="sxs-lookup"><span data-stu-id="479b6-172">A type parameter is a formal placeholder for a type that will be supplied later.</span></span> <span data-ttu-id="479b6-173">Naopak argument typu ([argumenty typu](types.md#type-arguments)) je skutečný typ, který je nahrazen parametrem typu při vytvoření vytvořeného typu.</span><span class="sxs-lookup"><span data-stu-id="479b6-173">By contrast, a type argument ([Type arguments](types.md#type-arguments)) is the actual type that is substituted for the type parameter when a constructed type is created.</span></span>

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

<span data-ttu-id="479b6-174">Každý parametr typu v deklaraci třídy definuje název v prostoru deklarací ([deklarace](basic-concepts.md#declarations)) této třídy.</span><span class="sxs-lookup"><span data-stu-id="479b6-174">Each type parameter in a class declaration defines a name in the declaration space ([Declarations](basic-concepts.md#declarations)) of that class.</span></span> <span data-ttu-id="479b6-175">Proto nemůže mít stejný název jako jiný parametr typu nebo člen deklarovaný v této třídě.</span><span class="sxs-lookup"><span data-stu-id="479b6-175">Thus, it cannot have the same name as another type parameter or a member declared in that class.</span></span> <span data-ttu-id="479b6-176">Parametr typu nemůže mít stejný název jako samotný typ.</span><span class="sxs-lookup"><span data-stu-id="479b6-176">A type parameter cannot have the same name as the type itself.</span></span>

### <a name="class-base-specification"></a><span data-ttu-id="479b6-177">Základní specifikace třídy</span><span class="sxs-lookup"><span data-stu-id="479b6-177">Class base specification</span></span>

<span data-ttu-id="479b6-178">Deklarace třídy může obsahovat specifikaci *class_base* , která definuje přímou základní třídu třídy a rozhraní ([rozhraní](interfaces.md)) přímo implementované třídou.</span><span class="sxs-lookup"><span data-stu-id="479b6-178">A class declaration may include a *class_base* specification, which defines the direct base class of the class and the interfaces ([Interfaces](interfaces.md)) directly implemented by the class.</span></span>

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

<span data-ttu-id="479b6-179">Základní třída zadaná v deklaraci třídy může být typ konstruované třídy ([vytvořené typy](types.md#constructed-types)).</span><span class="sxs-lookup"><span data-stu-id="479b6-179">The base class specified in a class declaration can be a constructed class type ([Constructed types](types.md#constructed-types)).</span></span> <span data-ttu-id="479b6-180">Základní třída nemůže být parametrem typu sama o sobě, i když může zahrnovat parametry typu, které jsou v oboru.</span><span class="sxs-lookup"><span data-stu-id="479b6-180">A base class cannot be a type parameter on its own, though it can involve the type parameters that are in scope.</span></span>

```csharp
class Extend<V>: V {}            // Error, type parameter used as base class
```

#### <a name="base-classes"></a><span data-ttu-id="479b6-181">Základní třídy</span><span class="sxs-lookup"><span data-stu-id="479b6-181">Base classes</span></span>

<span data-ttu-id="479b6-182">Pokud je *class_type* obsažen v *class_base*, určuje přímou základní třídu deklarované třídy.</span><span class="sxs-lookup"><span data-stu-id="479b6-182">When a *class_type* is included in the *class_base*, it specifies the direct base class of the class being declared.</span></span> <span data-ttu-id="479b6-183">Pokud deklarace třídy nemá žádný *class_base*nebo pokud *class_base* uvádí pouze typy rozhraní, `object`předpokládá se, že přímá základní třída je.</span><span class="sxs-lookup"><span data-stu-id="479b6-183">If a class declaration has no *class_base*, or if the *class_base* lists only interface types, the direct base class is assumed to be `object`.</span></span> <span data-ttu-id="479b6-184">Třída dědí členy ze své přímé základní třídy, jak je popsáno v tématu [Dědičnost](classes.md#inheritance).</span><span class="sxs-lookup"><span data-stu-id="479b6-184">A class inherits members from its direct base class, as described in [Inheritance](classes.md#inheritance).</span></span>

<span data-ttu-id="479b6-185">V příkladu</span><span class="sxs-lookup"><span data-stu-id="479b6-185">In the example</span></span>
```csharp
class A {}

class B: A {}
```
<span data-ttu-id="479b6-186">Třída `A` je označována jako přímá základní `B`třída a `B` je označována jako odvozena z `A`.</span><span class="sxs-lookup"><span data-stu-id="479b6-186">class `A` is said to be the direct base class of `B`, and `B` is said to be derived from `A`.</span></span> <span data-ttu-id="479b6-187">Vzhledem `A` k tomu, že explicitně nespecifikuje přímou základní třídu, její Přímá základní třída `object`je implicitně.</span><span class="sxs-lookup"><span data-stu-id="479b6-187">Since `A` does not explicitly specify a direct base class, its direct base class is implicitly `object`.</span></span>

<span data-ttu-id="479b6-188">V případě konstruovaného typu třídy, pokud je v deklaraci obecné třídy specifikována základní třída, je základní třída konstruovaného typu získána nahrazením, pro každý *type_parameter* v deklaraci základní třídy, odpovídající *type_argument* z konstruovaného typu.</span><span class="sxs-lookup"><span data-stu-id="479b6-188">For a constructed class type, if a base class is specified in the generic class declaration, the base class of the constructed type is obtained by substituting, for each *type_parameter* in the base class declaration, the corresponding *type_argument* of the constructed type.</span></span> <span data-ttu-id="479b6-189">S ohledem na deklarace obecných tříd</span><span class="sxs-lookup"><span data-stu-id="479b6-189">Given the generic class declarations</span></span>
```csharp
class B<U,V> {...}

class G<T>: B<string,T[]> {...}
```
<span data-ttu-id="479b6-190">základní třída konstruovaného typu `G<int>` by byla. `B<string,int[]>`</span><span class="sxs-lookup"><span data-stu-id="479b6-190">the base class of the constructed type `G<int>` would be `B<string,int[]>`.</span></span>

<span data-ttu-id="479b6-191">Přímá základní třída typu třídy musí být alespoň dostupná jako typ samotné třídy ([domény pro usnadnění](basic-concepts.md#accessibility-domains)).</span><span class="sxs-lookup"><span data-stu-id="479b6-191">The direct base class of a class type must be at least as accessible as the class type itself ([Accessibility domains](basic-concepts.md#accessibility-domains)).</span></span> <span data-ttu-id="479b6-192">Například se jedná o chybu `public` při kompilaci třídy, která má být odvozena `private` z třídy nebo `internal` .</span><span class="sxs-lookup"><span data-stu-id="479b6-192">For example, it is a compile-time error for a `public` class to derive from a `private` or `internal` class.</span></span>

<span data-ttu-id="479b6-193">Přímá základní třída typu třídy nesmí být žádné z následujících typů: `System.Array`, `System.Delegate`, `System.MulticastDelegate`, `System.Enum`nebo `System.ValueType`.</span><span class="sxs-lookup"><span data-stu-id="479b6-193">The direct base class of a class type must not be any of the following types: `System.Array`, `System.Delegate`, `System.MulticastDelegate`, `System.Enum`, or `System.ValueType`.</span></span> <span data-ttu-id="479b6-194">Kromě toho deklarace obecné třídy nemůže používat `System.Attribute` jako přímou nebo nepřímou základní třídu.</span><span class="sxs-lookup"><span data-stu-id="479b6-194">Furthermore, a generic class declaration cannot use `System.Attribute` as a direct or indirect base class.</span></span>

<span data-ttu-id="479b6-195">Při určování významu přímé základní třídy `A` specifikace třídy `B`se dočasně předpokládá, `B` že přímá základní třída je dočasně považována `object`za.</span><span class="sxs-lookup"><span data-stu-id="479b6-195">While determining the meaning of the direct base class specification `A` of a class `B`, the direct base class of `B` is temporarily assumed to be `object`.</span></span> <span data-ttu-id="479b6-196">Intuitivní to zajistí, že význam specifikace základní třídy nemůže rekurzivně záviset sám na sobě.</span><span class="sxs-lookup"><span data-stu-id="479b6-196">Intuitively this ensures that the meaning of a base class specification cannot recursively depend on itself.</span></span> <span data-ttu-id="479b6-197">Příklad:</span><span class="sxs-lookup"><span data-stu-id="479b6-197">The example:</span></span>
```csharp
class A<T> {
   public class B {}
}

class C : A<C.B> {}
```
<span data-ttu-id="479b6-198">`A<C.B>` došlo k chybě `object`, protože v základní třídě `C` je určena Přímá základní třída, která je považována za, a proto (podle pravidel [názvů a názvů typů](basic-concepts.md#namespace-and-type-names)) `C` není považována za člena. `B`.</span><span class="sxs-lookup"><span data-stu-id="479b6-198">is in error since in the base class specification `A<C.B>` the direct base class of `C` is considered to be `object`, and hence (by the rules of [Namespace and type names](basic-concepts.md#namespace-and-type-names))  `C` is not considered to have a member `B`.</span></span>

<span data-ttu-id="479b6-199">Základní třídy typu třídy jsou přímá základní třída a její základní třídy.</span><span class="sxs-lookup"><span data-stu-id="479b6-199">The base classes of a class type are the direct base class and its base classes.</span></span> <span data-ttu-id="479b6-200">Jinými slovy, sada základních tříd je přenosný uzávěr vztahu přímé základní třídy.</span><span class="sxs-lookup"><span data-stu-id="479b6-200">In other words, the set of base classes is the transitive closure of the direct base class relationship.</span></span> <span data-ttu-id="479b6-201">Odkazování na výše uvedený příklad jsou `B` `A` základní třídy pro a `object`.</span><span class="sxs-lookup"><span data-stu-id="479b6-201">Referring to the example above, the base classes of `B` are `A` and `object`.</span></span> <span data-ttu-id="479b6-202">V příkladu</span><span class="sxs-lookup"><span data-stu-id="479b6-202">In the example</span></span>
```csharp
class A {...}

class B<T>: A {...}

class C<T>: B<IComparable<T>> {...}

class D<T>: C<T[]> {...}
```
<span data-ttu-id="479b6-203">základní `D<int>` třídy pro jsou `B<IComparable<int[]>>` ,,`A`a .`object` `C<int[]>`</span><span class="sxs-lookup"><span data-stu-id="479b6-203">the base classes of `D<int>` are `C<int[]>`, `B<IComparable<int[]>>`, `A`, and `object`.</span></span>

<span data-ttu-id="479b6-204">S výjimkou `object`třídy každý typ třídy obsahuje přesně jednu přímou základní třídu.</span><span class="sxs-lookup"><span data-stu-id="479b6-204">Except for class `object`, every class type has exactly one direct base class.</span></span> <span data-ttu-id="479b6-205">`object` Třída nemá žádnou přímou základní třídu a jedná se o konečnou základní třídu všech ostatních tříd.</span><span class="sxs-lookup"><span data-stu-id="479b6-205">The `object` class has no direct base class and is the ultimate base class of all other classes.</span></span>

<span data-ttu-id="479b6-206">Je-li `B` Třída odvozena z třídy `A`, jedná se o chybu při `A` kompilaci, která závisí na `B`.</span><span class="sxs-lookup"><span data-stu-id="479b6-206">When a class `B` derives from a class `A`, it is a compile-time error for `A` to depend on `B`.</span></span> <span data-ttu-id="479b6-207">Třída ***přímo závisí na*** své přímé základní třídě (pokud existuje) a ***přímo závisí na*** třídě, ve které je okamžitě vnořena (pokud existuje).</span><span class="sxs-lookup"><span data-stu-id="479b6-207">A class ***directly depends on*** its direct base class (if any) and ***directly depends on*** the class within which it is immediately nested (if any).</span></span> <span data-ttu-id="479b6-208">V rámci této definice je kompletní sada tříd, na kterých je třída závislá, reflexivní a přenosný uzávěr ***přímo závislá na*** vztahu.</span><span class="sxs-lookup"><span data-stu-id="479b6-208">Given this definition, the complete set of classes upon which a class depends is the reflexive and transitive closure of the ***directly depends on*** relationship.</span></span>

<span data-ttu-id="479b6-209">Příklad</span><span class="sxs-lookup"><span data-stu-id="479b6-209">The example</span></span>
```csharp
class A: A {}
```
<span data-ttu-id="479b6-210">je chybné, protože třída závisí sám na sobě.</span><span class="sxs-lookup"><span data-stu-id="479b6-210">is erroneous because the class depends on itself.</span></span> <span data-ttu-id="479b6-211">Podobně příklad</span><span class="sxs-lookup"><span data-stu-id="479b6-211">Likewise, the example</span></span>
```csharp
class A: B {}
class B: C {}
class C: A {}
```
<span data-ttu-id="479b6-212">došlo k chybě, protože třídy jsou cyklicky závislé.</span><span class="sxs-lookup"><span data-stu-id="479b6-212">is in error because the classes circularly depend on themselves.</span></span> <span data-ttu-id="479b6-213">Nakonec příklad</span><span class="sxs-lookup"><span data-stu-id="479b6-213">Finally, the example</span></span>
```csharp
class A: B.C {}

class B: A
{
    public class C {}
}
```
<span data-ttu-id="479b6-214">má za následek chybu při kompilaci, protože `A` závisí na `B.C` (její přímé základní třídě), která závisí na `B` (její bezprostředně ohraničující třídu), která je cyklicky závislá na `A`.</span><span class="sxs-lookup"><span data-stu-id="479b6-214">results in a compile-time error because `A` depends on `B.C` (its direct base class), which depends on `B` (its immediately enclosing class), which circularly depends on `A`.</span></span>

<span data-ttu-id="479b6-215">Všimněte si, že třída nezávisí na třídách, které jsou v ní vnořené.</span><span class="sxs-lookup"><span data-stu-id="479b6-215">Note that a class does not depend on the classes that are nested within it.</span></span> <span data-ttu-id="479b6-216">V příkladu</span><span class="sxs-lookup"><span data-stu-id="479b6-216">In the example</span></span>
```csharp
class A
{
    class B: A {}
}
```
<span data-ttu-id="479b6-217">`B`závisí na `A` (protože `A` je to přímá základní třída a její bezprostředně ohraničující třída), ale `A` nezávisí na `B` (protože `B` není ani základní ani třída nadřazené třídy. `A`).</span><span class="sxs-lookup"><span data-stu-id="479b6-217">`B` depends on `A` (because `A` is both its direct base class and its immediately enclosing class), but `A` does not depend on `B` (since `B` is neither a base class nor an enclosing class of `A`).</span></span> <span data-ttu-id="479b6-218">Proto je příklad platný.</span><span class="sxs-lookup"><span data-stu-id="479b6-218">Thus, the example is valid.</span></span>

<span data-ttu-id="479b6-219">Není možné odvozovat od `sealed` třídy.</span><span class="sxs-lookup"><span data-stu-id="479b6-219">It is not possible to derive from a `sealed` class.</span></span> <span data-ttu-id="479b6-220">V příkladu</span><span class="sxs-lookup"><span data-stu-id="479b6-220">In the example</span></span>
```csharp
sealed class A {}

class B: A {}            // Error, cannot derive from a sealed class
```
<span data-ttu-id="479b6-221">Třída `B` je v chybě, protože se pokouší odvozovat `sealed` od třídy `A`.</span><span class="sxs-lookup"><span data-stu-id="479b6-221">class `B` is in error because it attempts to derive from the `sealed` class `A`.</span></span>

#### <a name="interface-implementations"></a><span data-ttu-id="479b6-222">Implementace rozhraní</span><span class="sxs-lookup"><span data-stu-id="479b6-222">Interface implementations</span></span>

<span data-ttu-id="479b6-223">Specifikace *class_base* může obsahovat seznam typů rozhraní. v takovém případě třída je označována jako přímá implementace daných typů rozhraní.</span><span class="sxs-lookup"><span data-stu-id="479b6-223">A *class_base* specification may include a list of interface types, in which case the class is said to directly implement the given interface types.</span></span> <span data-ttu-id="479b6-224">Implementace rozhraní jsou podrobněji popsány v [implementacích rozhraní](interfaces.md#interface-implementations).</span><span class="sxs-lookup"><span data-stu-id="479b6-224">Interface implementations are discussed further in [Interface implementations](interfaces.md#interface-implementations).</span></span>

### <a name="type-parameter-constraints"></a><span data-ttu-id="479b6-225">Omezení parametrů typu</span><span class="sxs-lookup"><span data-stu-id="479b6-225">Type parameter constraints</span></span>

<span data-ttu-id="479b6-226">Deklarace obecného typu a metody mohou volitelně určovat omezení parametrů typu zahrnutím *type_parameter_constraints_clause*s.</span><span class="sxs-lookup"><span data-stu-id="479b6-226">Generic type and method declarations can optionally specify type parameter constraints by including *type_parameter_constraints_clause*s.</span></span>

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

<span data-ttu-id="479b6-227">Každý *type_parameter_constraints_clause* se skládá z tokenu `where`následovaný názvem parametru typu, následovaný dvojtečkou a seznamem omezení pro tento parametr typu.</span><span class="sxs-lookup"><span data-stu-id="479b6-227">Each *type_parameter_constraints_clause* consists of the token `where`, followed by the name of a type parameter, followed by a colon and the list of constraints for that type parameter.</span></span> <span data-ttu-id="479b6-228">Pro každý parametr typu může existovat `where` maximálně jedna klauzule `where` a klauzule mohou být uvedeny v libovolném pořadí.</span><span class="sxs-lookup"><span data-stu-id="479b6-228">There can be at most one `where` clause for each type parameter, and the `where` clauses can be listed in any order.</span></span> <span data-ttu-id="479b6-229">Podobně jako tokeny `set` `where` a v přistupujícím objektu vlastnosti token není klíčové slovo. `get`</span><span class="sxs-lookup"><span data-stu-id="479b6-229">Like the `get` and `set` tokens in a property accessor, the `where` token is not a keyword.</span></span>

<span data-ttu-id="479b6-230">Seznam omezení uvedených v `where` klauzuli může zahrnovat kteroukoli z následujících součástí v tomto pořadí: jedno primární omezení, jedno nebo více sekundárních omezení a `new()`omezení konstruktoru.</span><span class="sxs-lookup"><span data-stu-id="479b6-230">The list of constraints given in a `where` clause can include any of the following components, in this order: a single primary constraint, one or more secondary constraints, and the constructor constraint, `new()`.</span></span>

<span data-ttu-id="479b6-231">`class` Primárním omezením může být typ třídy nebo ***omezení typu odkazu*** nebo ***omezení*** `struct`typu hodnoty.</span><span class="sxs-lookup"><span data-stu-id="479b6-231">A primary constraint can be a class type or the ***reference type constraint*** `class` or the ***value type constraint*** `struct`.</span></span> <span data-ttu-id="479b6-232">Sekundární omezení může být *type_parameter* nebo *INTERFACE_TYPE*.</span><span class="sxs-lookup"><span data-stu-id="479b6-232">A secondary constraint can be a *type_parameter* or *interface_type*.</span></span>

<span data-ttu-id="479b6-233">Omezení typu odkazu určuje, že argument typu použitý pro parametr typu musí být odkazový typ.</span><span class="sxs-lookup"><span data-stu-id="479b6-233">The reference type constraint specifies that a type argument used for the type parameter must be a reference type.</span></span> <span data-ttu-id="479b6-234">Toto omezení neplatí pro všechny typy tříd, typy rozhraní, typy delegátů, typy polí a parametry typu, které jsou známé jako typ odkazu (jak je definováno níže).</span><span class="sxs-lookup"><span data-stu-id="479b6-234">All class types, interface types, delegate types, array types, and type parameters known to be a reference type (as defined below) satisfy this constraint.</span></span>

<span data-ttu-id="479b6-235">Omezení typu hodnoty určuje, že argument typu použitý pro parametr typu musí být typ hodnoty, která není null.</span><span class="sxs-lookup"><span data-stu-id="479b6-235">The value type constraint specifies that a type argument used for the type parameter must be a non-nullable value type.</span></span> <span data-ttu-id="479b6-236">Toto omezení neodpovídají všem typům struktury, které nejsou null, výčtové typy a parametry typu s omezením typu hodnoty.</span><span class="sxs-lookup"><span data-stu-id="479b6-236">All non-nullable struct types, enum types, and type parameters having the value type constraint satisfy this constraint.</span></span> <span data-ttu-id="479b6-237">Všimněte si, že i když typ hodnoty, typ s povolenou hodnotou null ([typy s možnou hodnotou null](types.md#nullable-types)) nevyhovuje omezení hodnotového typu.</span><span class="sxs-lookup"><span data-stu-id="479b6-237">Note that although classified as a value type, a nullable type ([Nullable types](types.md#nullable-types)) does not satisfy the value type constraint.</span></span> <span data-ttu-id="479b6-238">Parametr typu s omezením typu hodnoty nemůže mít také *constructor_constraint*.</span><span class="sxs-lookup"><span data-stu-id="479b6-238">A type parameter having the value type constraint cannot also have the *constructor_constraint*.</span></span>

<span data-ttu-id="479b6-239">Typy ukazatelů nikdy nemůžou být argumenty typu a nepovažují se za nevyhovující buď omezením typu odkazu nebo typu hodnoty.</span><span class="sxs-lookup"><span data-stu-id="479b6-239">Pointer types are never allowed to be type arguments and are not considered to satisfy either the reference type or value type constraints.</span></span>

<span data-ttu-id="479b6-240">Pokud je omezení typ třídy, typ rozhraní nebo parametr typu, tento typ Určuje minimální "základní typ", který každý argument typu použitý pro tento parametr typu musí podporovat.</span><span class="sxs-lookup"><span data-stu-id="479b6-240">If a constraint is a class type, an interface type, or a type parameter, that type specifies a minimal "base type" that every type argument used for that type parameter must support.</span></span> <span data-ttu-id="479b6-241">Vždy, když je použit konstruovaný typ nebo obecná metoda, je argument typu zkontrolován proti omezením parametru typu v době kompilace.</span><span class="sxs-lookup"><span data-stu-id="479b6-241">Whenever a constructed type or generic method is used, the type argument is checked against the constraints on the type parameter at compile-time.</span></span> <span data-ttu-id="479b6-242">Zadaný argument typu musí splňovat podmínky popsané v tématu [vyhovujícím omezením](types.md#satisfying-constraints).</span><span class="sxs-lookup"><span data-stu-id="479b6-242">The type argument supplied must satisfy the conditions described in [Satisfying constraints](types.md#satisfying-constraints).</span></span>

<span data-ttu-id="479b6-243">Omezení *class_type* musí splňovat následující pravidla:</span><span class="sxs-lookup"><span data-stu-id="479b6-243">A *class_type* constraint must satisfy the following rules:</span></span>

*  <span data-ttu-id="479b6-244">Typ musí být typ třídy.</span><span class="sxs-lookup"><span data-stu-id="479b6-244">The type must be a class type.</span></span>
*  <span data-ttu-id="479b6-245">Typ nesmí být `sealed`.</span><span class="sxs-lookup"><span data-stu-id="479b6-245">The type must not be `sealed`.</span></span>
*  <span data-ttu-id="479b6-246">Typ nesmí být jeden z následujících typů `System.Array`:, `System.Delegate`, `System.Enum`nebo `System.ValueType`.</span><span class="sxs-lookup"><span data-stu-id="479b6-246">The type must not be one of the following types: `System.Array`, `System.Delegate`, `System.Enum`, or `System.ValueType`.</span></span>
*  <span data-ttu-id="479b6-247">Typ nesmí být `object`.</span><span class="sxs-lookup"><span data-stu-id="479b6-247">The type must not be `object`.</span></span> <span data-ttu-id="479b6-248">Vzhledem k tomu, že `object`všechny typy jsou odvozeny z, takové omezení by nebylo nijak ovlivněno, pokud bylo povoleno.</span><span class="sxs-lookup"><span data-stu-id="479b6-248">Because all types derive from `object`, such a constraint would have no effect if it were permitted.</span></span>
*  <span data-ttu-id="479b6-249">Pouze jedno omezení pro daný parametr typu může být typem třídy.</span><span class="sxs-lookup"><span data-stu-id="479b6-249">At most one constraint for a given type parameter can be a class type.</span></span>

<span data-ttu-id="479b6-250">Typ zadaný jako omezení *INTERFACE_TYPE* musí splňovat následující pravidla:</span><span class="sxs-lookup"><span data-stu-id="479b6-250">A type specified as an *interface_type* constraint must satisfy the following rules:</span></span>

*  <span data-ttu-id="479b6-251">Typ musí být typ rozhraní.</span><span class="sxs-lookup"><span data-stu-id="479b6-251">The type must be an interface type.</span></span>
*  <span data-ttu-id="479b6-252">Typ nesmí být v dané `where` klauzuli uveden více než jednou.</span><span class="sxs-lookup"><span data-stu-id="479b6-252">A type must not be specified more than once in a given `where` clause.</span></span>

<span data-ttu-id="479b6-253">V obou případech omezení může zahrnovat jakékoli parametry typu asociovaného typu nebo deklarace metody jako součást konstruovaného typu a může zahrnovat deklarovaný typ.</span><span class="sxs-lookup"><span data-stu-id="479b6-253">In either case, the constraint can involve any of the type parameters of the associated type or method declaration as part of a constructed type, and can involve the type being declared.</span></span>

<span data-ttu-id="479b6-254">Jakýkoli typ třídy nebo rozhraní zadaný jako omezení parametru typu musí být alespoň přístupná ([Omezení přístupnosti](basic-concepts.md#accessibility-constraints)) jako obecný typ nebo metoda, která je deklarována.</span><span class="sxs-lookup"><span data-stu-id="479b6-254">Any class or interface type specified as a type parameter constraint must be at least as accessible ([Accessibility constraints](basic-concepts.md#accessibility-constraints)) as the generic type or method being declared.</span></span>

<span data-ttu-id="479b6-255">Typ zadaný jako omezení *type_parameter* musí splňovat následující pravidla:</span><span class="sxs-lookup"><span data-stu-id="479b6-255">A type specified as a *type_parameter* constraint must satisfy the following rules:</span></span>

*  <span data-ttu-id="479b6-256">Typ musí být parametr typu.</span><span class="sxs-lookup"><span data-stu-id="479b6-256">The type must be a type parameter.</span></span>
*  <span data-ttu-id="479b6-257">Typ nesmí být v dané `where` klauzuli uveden více než jednou.</span><span class="sxs-lookup"><span data-stu-id="479b6-257">A type must not be specified more than once in a given `where` clause.</span></span>

<span data-ttu-id="479b6-258">Kromě toho nesmí být v grafu závislostí parametrů typu žádné cykly, kde závislost je přenosný vztah definovaný pomocí:</span><span class="sxs-lookup"><span data-stu-id="479b6-258">In addition there must be no cycles in the dependency graph of type parameters, where dependency is a transitive relation defined by:</span></span>

*  <span data-ttu-id="479b6-259">Pokud `T` je parametr typu použit jako omezení pro `S` parametr `S` typu, ***závisí na*** `T`.</span><span class="sxs-lookup"><span data-stu-id="479b6-259">If a type parameter `T` is used as a constraint for type parameter `S` then `S` ***depends on*** `T`.</span></span>
*  <span data-ttu-id="479b6-260">Pokud parametr `S` typu závisí na parametru `T` typu a závisí na `T` parametru `U` typu, pak `S` ***závisí na*** `U`.</span><span class="sxs-lookup"><span data-stu-id="479b6-260">If a type parameter `S` depends on a type parameter `T` and `T` depends on a type parameter `U` then `S` ***depends on*** `U`.</span></span>

<span data-ttu-id="479b6-261">Vzhledem k tomuto vztahu se jedná o chybu při kompilaci pro parametr typu, který závisí sám na sobě (přímo nebo nepřímo).</span><span class="sxs-lookup"><span data-stu-id="479b6-261">Given this relation, it is a compile-time error for a type parameter to depend on itself (directly or indirectly).</span></span>

<span data-ttu-id="479b6-262">Všechna omezení musí být konzistentní mezi parametry závislého typu.</span><span class="sxs-lookup"><span data-stu-id="479b6-262">Any constraints must be consistent among dependent type parameters.</span></span> <span data-ttu-id="479b6-263">Pokud parametr `S` typu závisí na parametru `T` typu, pak:</span><span class="sxs-lookup"><span data-stu-id="479b6-263">If type parameter `S` depends on type parameter `T` then:</span></span>

*  <span data-ttu-id="479b6-264">`T`nesmí mít omezení typu hodnoty.</span><span class="sxs-lookup"><span data-stu-id="479b6-264">`T` must not have the value type constraint.</span></span> <span data-ttu-id="479b6-265">V opačném případě `S` `T`je to efektivně zapečetěné, takže by bylo nutné, aby bylo stejného typu, jako s vyloučením nutnosti dvou parametrů typu. `T`</span><span class="sxs-lookup"><span data-stu-id="479b6-265">Otherwise, `T` is effectively sealed so `S` would be forced to be the same type as `T`, eliminating the need for two type parameters.</span></span>
*  <span data-ttu-id="479b6-266">Pokud `S` má`T` omezení typu hodnoty, nesmí mít omezení *class_type* .</span><span class="sxs-lookup"><span data-stu-id="479b6-266">If `S` has the value type constraint then `T` must not have a *class_type* constraint.</span></span>
*  <span data-ttu-id="479b6-267">Pokud `S` má omezení `T` *class_type a* má `A` omezení class_type,musíbýtpřevodidentityneboimplicitníodkaznapřevod`B`zna `A` `B`nebo implicitní převod odkazu z `B` na. `A`</span><span class="sxs-lookup"><span data-stu-id="479b6-267">If `S` has a *class_type* constraint `A` and `T` has a *class_type* constraint `B` then there must be an identity conversion or implicit reference conversion from `A` to `B` or an implicit reference conversion from `B` to `A`.</span></span>
*  <span data-ttu-id="479b6-268">Pokud `S` také závisí na parametru `U` typu a `U` má omezení `A` class_type a má `T` omezení `B` *class_type* , musí existovat převod identity. nebo implicitní převod odkazu z `A` na `B` nebo implicitní odkaz na převod z `B` na `A`.</span><span class="sxs-lookup"><span data-stu-id="479b6-268">If `S` also depends on type parameter `U` and `U` has a *class_type* constraint `A` and `T` has a *class_type* constraint `B` then there must be an identity conversion or implicit reference conversion from `A` to `B` or an implicit reference conversion from `B` to `A`.</span></span>

<span data-ttu-id="479b6-269">Je platný pro `S` , že má omezení typu hodnoty a `T` má omezení typu odkazu.</span><span class="sxs-lookup"><span data-stu-id="479b6-269">It is valid for `S` to have the value type constraint and `T` to have the reference type constraint.</span></span> <span data-ttu-id="479b6-270">Toto omezení `T` je efektivní pro typy `System.Object`, `System.ValueType`, `System.Enum`a libovolný typ rozhraní.</span><span class="sxs-lookup"><span data-stu-id="479b6-270">Effectively this limits `T` to the types `System.Object`, `System.ValueType`, `System.Enum`, and any interface type.</span></span>

<span data-ttu-id="479b6-271">Pokud klauzule pro parametr typu obsahuje omezení konstruktoru (které má formulář `new()`), `new` je možné použít operátor k vytvoření instancí typu ([výrazy vytváření objektů](expressions.md#object-creation-expressions)). `where`</span><span class="sxs-lookup"><span data-stu-id="479b6-271">If the `where` clause for a type parameter includes a constructor constraint (which has the form `new()`), it is possible to use the `new` operator to create instances of the type ([Object creation expressions](expressions.md#object-creation-expressions)).</span></span> <span data-ttu-id="479b6-272">Jakýkoli argument typu, který se používá pro parametr typu s omezením konstruktoru, musí mít veřejný konstruktor bez parametrů (Tento konstruktor má implicitně existovat pro libovolný typ hodnoty) nebo musí být parametr typu s omezením nebo omezením hodnotového typu (viz [Omezení parametru typu](classes.md#type-parameter-constraints) pro podrobnosti).</span><span class="sxs-lookup"><span data-stu-id="479b6-272">Any type argument used for a type parameter with a constructor constraint must have a public parameterless constructor (this constructor implicitly exists for any value type) or be a type parameter having the value type constraint or constructor constraint (see [Type parameter constraints](classes.md#type-parameter-constraints) for details).</span></span>

<span data-ttu-id="479b6-273">Níže jsou uvedeny příklady omezení:</span><span class="sxs-lookup"><span data-stu-id="479b6-273">The following are examples of constraints:</span></span>
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

<span data-ttu-id="479b6-274">Následující příklad je chyba, protože způsobuje cyklické vazby v grafu závislostí parametrů typu:</span><span class="sxs-lookup"><span data-stu-id="479b6-274">The following example is in error because it causes a circularity in the dependency graph of the type parameters:</span></span>
```csharp
class Circular<S,T>
    where S: T
    where T: S                // Error, circularity in dependency graph
{
    ...
}
```

<span data-ttu-id="479b6-275">Následující příklady ilustrují další neplatnou situaci:</span><span class="sxs-lookup"><span data-stu-id="479b6-275">The following examples illustrate additional invalid situations:</span></span>
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

<span data-ttu-id="479b6-276">***Efektivní základní třída*** parametru `T` typu je definována takto:</span><span class="sxs-lookup"><span data-stu-id="479b6-276">The ***effective base class*** of a type parameter `T` is defined as follows:</span></span>

*  <span data-ttu-id="479b6-277">Pokud `T` nemá omezení primárních omezení nebo parametrů typu, jeho efektivní základní třída je `object`.</span><span class="sxs-lookup"><span data-stu-id="479b6-277">If `T` has no primary constraints or type parameter constraints, its effective base class is `object`.</span></span>
*  <span data-ttu-id="479b6-278">Pokud `T` má omezení typu hodnoty, jeho efektivní základní třída je `System.ValueType`.</span><span class="sxs-lookup"><span data-stu-id="479b6-278">If `T` has the value type constraint, its effective base class is `System.ValueType`.</span></span>
*  <span data-ttu-id="479b6-279">Pokud `T` `C`má omezení `C` class_type, ale žádné omezení *type_parameter* , jeho efektivní základní třída je.</span><span class="sxs-lookup"><span data-stu-id="479b6-279">If `T` has a *class_type* constraint `C` but no *type_parameter* constraints, its effective base class is `C`.</span></span>
*  <span data-ttu-id="479b6-280">Pokud `T` nemá žádné omezení *class_type* , ale má nejméně jedno omezení *type_parameter* , jeho efektivní základní třída je[nejvýšený](conversions.md#lifted-conversion-operators)typ (přenesené operátory převodu) v sadě efektivních základních tříd jeho *type_ omezení parametru* .</span><span class="sxs-lookup"><span data-stu-id="479b6-280">If `T` has no *class_type* constraint but has one or more *type_parameter* constraints, its effective base class is the most encompassed type ([Lifted conversion operators](conversions.md#lifted-conversion-operators)) in the set of effective base classes of its *type_parameter* constraints.</span></span> <span data-ttu-id="479b6-281">Pravidla konzistence zajišťují, že existuje takový typ, který nejlépe zahrnuje.</span><span class="sxs-lookup"><span data-stu-id="479b6-281">The consistency rules ensure that such a most encompassed type exists.</span></span>
*  <span data-ttu-id="479b6-282">Pokud `T` má omezení *class_type* i jedno nebo více *type_parameter* omezení, jeho efektivní základní třída je[nejvýšený](conversions.md#lifted-conversion-operators)typ (přenesené operátory převodu) v sadě sestávající z *class_type* omezení a efektivní základní třídy jeho omezení type_parameter. `T`</span><span class="sxs-lookup"><span data-stu-id="479b6-282">If `T` has both a *class_type* constraint and one or more *type_parameter* constraints, its effective base class is the most encompassed type ([Lifted conversion operators](conversions.md#lifted-conversion-operators)) in the set consisting of the *class_type* constraint of `T` and the effective base classes of its *type_parameter* constraints.</span></span> <span data-ttu-id="479b6-283">Pravidla konzistence zajišťují, že existuje takový typ, který nejlépe zahrnuje.</span><span class="sxs-lookup"><span data-stu-id="479b6-283">The consistency rules ensure that such a most encompassed type exists.</span></span>
*  <span data-ttu-id="479b6-284">Pokud `T` má omezení typu odkazu, ale žádná omezení *class_type* , jeho efektivní základní třída je `object`.</span><span class="sxs-lookup"><span data-stu-id="479b6-284">If `T` has the reference type constraint but no *class_type* constraints, its effective base class is `object`.</span></span>

<span data-ttu-id="479b6-285">Pro účely těchto pravidel platí, že pokud `V` má T omezení, které je *value_type*, použijte místo nejpřesnější základní typ `V` , který je *class_type*.</span><span class="sxs-lookup"><span data-stu-id="479b6-285">For the purpose of these rules, if T has a constraint `V` that is a *value_type*, use instead the most specific base type of `V` that is a *class_type*.</span></span> <span data-ttu-id="479b6-286">K tomu nemůže nikdy dojít v explicitně daném omezení, ale může dojít v případě, že jsou omezení obecné metody implicitně zděděna přepsáním deklarace metody nebo explicitní implementací metody rozhraní.</span><span class="sxs-lookup"><span data-stu-id="479b6-286">This can never happen in an explicitly given constraint, but may occur when the constraints of a generic method are implicitly inherited by an overriding method declaration or an explicit implementation of an interface method.</span></span>

<span data-ttu-id="479b6-287">Tato pravidla zajišťují, že účinná základní třída je vždy *class_type*.</span><span class="sxs-lookup"><span data-stu-id="479b6-287">These rules ensure that the effective base class is always a *class_type*.</span></span>

<span data-ttu-id="479b6-288">***Efektivní sada rozhraní*** parametru `T` typu je definována takto:</span><span class="sxs-lookup"><span data-stu-id="479b6-288">The ***effective interface set*** of a type parameter `T` is defined as follows:</span></span>

*  <span data-ttu-id="479b6-289">Pokud `T` nemá žádný *secondary_constraints*, je jeho efektivní sada rozhraní prázdná.</span><span class="sxs-lookup"><span data-stu-id="479b6-289">If `T` has no *secondary_constraints*, its effective interface set is empty.</span></span>
*  <span data-ttu-id="479b6-290">Pokud `T` má omezení *INTERFACE_TYPE* , ale žádné omezení *type_parameter* , jeho efektivní sada rozhraní je svou sadou omezení *INTERFACE_TYPE* .</span><span class="sxs-lookup"><span data-stu-id="479b6-290">If `T` has *interface_type* constraints but no *type_parameter* constraints, its effective interface set is its set of *interface_type* constraints.</span></span>
*  <span data-ttu-id="479b6-291">Pokud `T` nemá žádná omezení *INTERFACE_TYPE* , ale má omezení *type_parameter* , jeho efektivní sada rozhraní je sjednocení platných sad rozhraní svých omezení *type_parameter* .</span><span class="sxs-lookup"><span data-stu-id="479b6-291">If `T` has no *interface_type* constraints but has *type_parameter* constraints, its effective interface set is the union of the effective interface sets of its *type_parameter* constraints.</span></span>
*  <span data-ttu-id="479b6-292">Pokud `T` má omezení *INTERFACE_TYPE* i omezení *type_parameter* , jeho efektivní sada rozhraní je sjednocení své sady omezení *INTERFACE_TYPE* a efektivní sady rozhraní své *type_parameter* omezení.</span><span class="sxs-lookup"><span data-stu-id="479b6-292">If `T` has both *interface_type* constraints and *type_parameter* constraints, its effective interface set is the union of its set of *interface_type* constraints and the effective interface sets of its *type_parameter* constraints.</span></span>

<span data-ttu-id="479b6-293">Parametr typu je ***známý jako odkazový typ*** , pokud má omezení typu odkazu nebo jeho efektivní základní třídu `object` není nebo. `System.ValueType`</span><span class="sxs-lookup"><span data-stu-id="479b6-293">A type parameter is ***known to be a reference type*** if it has the reference type constraint or its effective base class is not `object` or `System.ValueType`.</span></span>

<span data-ttu-id="479b6-294">Hodnoty typu parametru omezeného typu lze použít pro přístup ke členům instance, které jsou odvozeny omezeními.</span><span class="sxs-lookup"><span data-stu-id="479b6-294">Values of a constrained type parameter type can be used to access the instance members implied by the constraints.</span></span> <span data-ttu-id="479b6-295">V příkladu</span><span class="sxs-lookup"><span data-stu-id="479b6-295">In the example</span></span>
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
<span data-ttu-id="479b6-296">metody `IPrintable` lze vyvolat přímo na `x` , protože `T` je omezeno na vždy implementovat `IPrintable`.</span><span class="sxs-lookup"><span data-stu-id="479b6-296">the methods of `IPrintable` can be invoked directly on `x` because `T` is constrained to always implement `IPrintable`.</span></span>

### <a name="class-body"></a><span data-ttu-id="479b6-297">Tělo třídy</span><span class="sxs-lookup"><span data-stu-id="479b6-297">Class body</span></span>

<span data-ttu-id="479b6-298">*Class_body* třídy definuje členy této třídy.</span><span class="sxs-lookup"><span data-stu-id="479b6-298">The *class_body* of a class defines the members of that class.</span></span>

```antlr
class_body
    : '{' class_member_declaration* '}'
    ;
```

## <a name="partial-types"></a><span data-ttu-id="479b6-299">Částečné typy</span><span class="sxs-lookup"><span data-stu-id="479b6-299">Partial types</span></span>

<span data-ttu-id="479b6-300">Deklarace typu může být rozdělená mezi několik ***deklarací částečného typu***.</span><span class="sxs-lookup"><span data-stu-id="479b6-300">A type declaration can be split across multiple ***partial type declarations***.</span></span> <span data-ttu-id="479b6-301">Deklarace typu je vytvořena z jeho částí podle pravidel v této části, přičemž je považována za jednu deklaraci během doby kompilace programu a za běhu.</span><span class="sxs-lookup"><span data-stu-id="479b6-301">The type declaration is constructed from its parts by following the rules in this section, whereupon it is treated as a single declaration during the remainder of the compile-time and run-time processing of the program.</span></span>

<span data-ttu-id="479b6-302">*Class_declaration*, *struct_declaration* nebo *interface_declaration* představuje `partial` deklaraci částečného typu, pokud obsahuje modifikátor.</span><span class="sxs-lookup"><span data-stu-id="479b6-302">A *class_declaration*, *struct_declaration* or *interface_declaration* represents a partial type declaration if it includes a `partial` modifier.</span></span> <span data-ttu-id="479b6-303">`partial`není klíčové slovo a funguje pouze jako modifikátor, pokud se zobrazí bezprostředně před `class`jedním z klíčových slov `struct` nebo `interface` v deklaraci typu nebo před typem `void` v deklaraci metody.</span><span class="sxs-lookup"><span data-stu-id="479b6-303">`partial` is not a keyword, and only acts as a modifier if it appears immediately before one of the keywords `class`, `struct` or `interface` in a type declaration, or before the type `void` in a method declaration.</span></span> <span data-ttu-id="479b6-304">V jiných kontextech je možné ji použít jako běžný identifikátor.</span><span class="sxs-lookup"><span data-stu-id="479b6-304">In other contexts it can be used as a normal identifier.</span></span>

<span data-ttu-id="479b6-305">Každá část deklarace částečného typu musí obsahovat `partial` modifikátor.</span><span class="sxs-lookup"><span data-stu-id="479b6-305">Each part of a partial type declaration must include a `partial` modifier.</span></span> <span data-ttu-id="479b6-306">Musí mít stejný název a být deklarován ve stejném oboru názvů nebo deklaraci typu jako ostatní části.</span><span class="sxs-lookup"><span data-stu-id="479b6-306">It must have the same name  and be declared in the same namespace or type declaration as the other parts.</span></span> <span data-ttu-id="479b6-307">Modifikátor označuje, že další části deklarace typu mohou existovat jinde, ale existence těchto dalších částí není požadavkem; je platná pro typ s jedinou deklarací pro `partial` zahrnutí modifikátoru. `partial`</span><span class="sxs-lookup"><span data-stu-id="479b6-307">The `partial` modifier indicates that additional parts of the type declaration may exist elsewhere, but the existence of such additional parts is not a requirement; it is valid for a type with a single declaration to include the `partial` modifier.</span></span>

<span data-ttu-id="479b6-308">Všechny části částečného typu musí být kompilovány dohromady tak, aby mohly být součásti sloučeny v době kompilace do jediné deklarace typu.</span><span class="sxs-lookup"><span data-stu-id="479b6-308">All parts of a partial type must be compiled together such that the parts can be merged at compile-time into a single type declaration.</span></span> <span data-ttu-id="479b6-309">Částečné typy specificky neumožňují rozšíření již kompilovaných typů.</span><span class="sxs-lookup"><span data-stu-id="479b6-309">Partial types specifically do not allow already compiled types to be extended.</span></span>

<span data-ttu-id="479b6-310">Vnořené typy mohou být deklarovány ve více částech pomocí `partial` modifikátoru.</span><span class="sxs-lookup"><span data-stu-id="479b6-310">Nested types may be declared in multiple parts by using the `partial` modifier.</span></span> <span data-ttu-id="479b6-311">Nadřazený typ je obvykle deklarován pomocí `partial` i a každá část vnořeného typu je deklarována v jiné části nadřazeného typu.</span><span class="sxs-lookup"><span data-stu-id="479b6-311">Typically, the containing type is declared using `partial` as well, and each part of the nested type is declared in a different part of the containing type.</span></span>

<span data-ttu-id="479b6-312">`partial` Modifikátor není povolený pro deklarace delegáta nebo výčtu.</span><span class="sxs-lookup"><span data-stu-id="479b6-312">The `partial` modifier is not permitted on delegate or enum declarations.</span></span>

### <a name="attributes"></a><span data-ttu-id="479b6-313">Atributy</span><span class="sxs-lookup"><span data-stu-id="479b6-313">Attributes</span></span>

<span data-ttu-id="479b6-314">Atributy částečného typu jsou určeny kombinováním v nespecifikovaném pořadí, atributů každé části.</span><span class="sxs-lookup"><span data-stu-id="479b6-314">The attributes of a partial type are determined by combining, in an unspecified order, the attributes of each of the parts.</span></span> <span data-ttu-id="479b6-315">Pokud je atribut umístěn na více částech, je ekvivalentní určení atributu vícekrát pro daný typ.</span><span class="sxs-lookup"><span data-stu-id="479b6-315">If an attribute is placed on multiple parts, it is equivalent to specifying the attribute multiple times on the type.</span></span> <span data-ttu-id="479b6-316">Například dvě části:</span><span class="sxs-lookup"><span data-stu-id="479b6-316">For example, the two parts:</span></span>

```csharp
[Attr1, Attr2("hello")]
partial class A {}

[Attr3, Attr2("goodbye")]
partial class A {}
```
<span data-ttu-id="479b6-317">jsou ekvivalentní k deklaraci, jako například:</span><span class="sxs-lookup"><span data-stu-id="479b6-317">are equivalent to a declaration such as:</span></span>
```csharp
[Attr1, Attr2("hello"), Attr3, Attr2("goodbye")]
class A {}
```

<span data-ttu-id="479b6-318">Atributy u parametrů typu jsou kombinovány podobným způsobem.</span><span class="sxs-lookup"><span data-stu-id="479b6-318">Attributes on type parameters combine in a similar fashion.</span></span>

### <a name="modifiers"></a><span data-ttu-id="479b6-319">Modifikátory</span><span class="sxs-lookup"><span data-stu-id="479b6-319">Modifiers</span></span>

<span data-ttu-id="479b6-320">Pokud částečná deklarace typu zahrnuje specifikaci přístupnosti ( `public`, `protected`, `internal`a `private` modifikátory), musí souhlasit se všemi ostatními částmi, které obsahují specifikaci přístupnosti.</span><span class="sxs-lookup"><span data-stu-id="479b6-320">When a partial type declaration includes an accessibility specification (the `public`, `protected`, `internal`, and `private` modifiers) it must agree with all other parts that include an accessibility specification.</span></span> <span data-ttu-id="479b6-321">Pokud žádná část částečného typu neobsahuje specifikaci přístupnosti, typ se udělí příslušnému výchozímu usnadnění ([deklarovaný přístup](basic-concepts.md#declared-accessibility)).</span><span class="sxs-lookup"><span data-stu-id="479b6-321">If no part of a partial type includes an accessibility specification, the type is given the appropriate default accessibility ([Declared accessibility](basic-concepts.md#declared-accessibility)).</span></span>

<span data-ttu-id="479b6-322">Pokud jeden nebo více částečných deklarací vnořeného typu obsahuje `new` modifikátor, není ohlášeno žádné upozornění, pokud vnořený typ skryje zděděný člen ([skrytím prostřednictvím dědičnosti](basic-concepts.md#hiding-through-inheritance)).</span><span class="sxs-lookup"><span data-stu-id="479b6-322">If one or more partial declarations of a nested type include a `new` modifier, no warning is reported if the nested type hides an inherited member ([Hiding through inheritance](basic-concepts.md#hiding-through-inheritance)).</span></span>

<span data-ttu-id="479b6-323">Pokud jedna nebo více částečných deklarací třídy obsahuje `abstract` modifikátor, je třída považována za abstraktní ([abstraktní třídy](classes.md#abstract-classes)).</span><span class="sxs-lookup"><span data-stu-id="479b6-323">If one or more partial declarations of a class include an `abstract` modifier, the class is considered abstract ([Abstract classes](classes.md#abstract-classes)).</span></span> <span data-ttu-id="479b6-324">V opačném případě se třída považuje za neabstraktní.</span><span class="sxs-lookup"><span data-stu-id="479b6-324">Otherwise, the class is considered non-abstract.</span></span>

<span data-ttu-id="479b6-325">Pokud jedna nebo více částečných deklarací třídy obsahuje `sealed` modifikátor, třída je považována za zapečetěnou ([zapečetěné třídy](classes.md#sealed-classes)).</span><span class="sxs-lookup"><span data-stu-id="479b6-325">If one or more partial declarations of a class include a `sealed` modifier, the class is considered sealed ([Sealed classes](classes.md#sealed-classes)).</span></span> <span data-ttu-id="479b6-326">V opačném případě je třída považována za nezapečetěnou.</span><span class="sxs-lookup"><span data-stu-id="479b6-326">Otherwise, the class is considered unsealed.</span></span>

<span data-ttu-id="479b6-327">Všimněte si, že třída nemůže být abstraktní i zapečetěná.</span><span class="sxs-lookup"><span data-stu-id="479b6-327">Note that a class cannot be both abstract and sealed.</span></span>

<span data-ttu-id="479b6-328">Pokud je modifikátor použit v deklaraci částečného typu, pouze tato konkrétní část je považována za nezabezpečený kontext (nebezpečné kontexty).[](unsafe-code.md#unsafe-contexts) `unsafe`</span><span class="sxs-lookup"><span data-stu-id="479b6-328">When the `unsafe` modifier is used on a partial type declaration, only that particular part is considered an unsafe context ([Unsafe contexts](unsafe-code.md#unsafe-contexts)).</span></span>

### <a name="type-parameters-and-constraints"></a><span data-ttu-id="479b6-329">Parametry a omezení typu</span><span class="sxs-lookup"><span data-stu-id="479b6-329">Type parameters and constraints</span></span>

<span data-ttu-id="479b6-330">Je-li obecný typ deklarován ve více částech, musí každá část uvádět parametry typu.</span><span class="sxs-lookup"><span data-stu-id="479b6-330">If a generic type is declared in multiple parts, each part must state the type parameters.</span></span> <span data-ttu-id="479b6-331">Každá část musí mít stejný počet parametrů typu a stejný název pro každý parametr typu v daném pořadí.</span><span class="sxs-lookup"><span data-stu-id="479b6-331">Each part must have the same number of type parameters, and the same name for each type parameter, in order.</span></span>

<span data-ttu-id="479b6-332">Pokud částečná deklarace obecného typu obsahuje omezení (`where` klauzule), musí tato omezení souhlasit se všemi ostatními částmi, které zahrnují omezení.</span><span class="sxs-lookup"><span data-stu-id="479b6-332">When a partial generic type declaration includes constraints (`where` clauses), the constraints must agree with all other parts that include constraints.</span></span> <span data-ttu-id="479b6-333">Konkrétně každá část, která obsahuje omezení, musí mít omezení pro stejnou sadu parametrů typu a pro každý parametr typu musí být sady omezení primárního, sekundárního a konstruktoru ekvivalentní.</span><span class="sxs-lookup"><span data-stu-id="479b6-333">Specifically, each part that includes constraints must have constraints for the same set of type parameters, and for each type parameter the sets of primary, secondary, and constructor constraints must be equivalent.</span></span> <span data-ttu-id="479b6-334">Dvě sady omezení jsou ekvivalentní, pokud obsahují stejné členy.</span><span class="sxs-lookup"><span data-stu-id="479b6-334">Two sets of constraints are equivalent if they contain the same members.</span></span> <span data-ttu-id="479b6-335">Pokud žádná část částečného obecného typu neurčuje omezení parametru typu, parametry typu jsou považovány za neomezeno.</span><span class="sxs-lookup"><span data-stu-id="479b6-335">If no part of a partial generic type specifies type parameter constraints, the type parameters are considered unconstrained.</span></span>

<span data-ttu-id="479b6-336">Příklad</span><span class="sxs-lookup"><span data-stu-id="479b6-336">The example</span></span>
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
<span data-ttu-id="479b6-337">je správné, protože tyto části, které obsahují omezení (první dva) efektivně určují stejnou sadu omezení PRIMARY, sekundární a konstruktor pro stejnou sadu parametrů typu, v uvedeném pořadí.</span><span class="sxs-lookup"><span data-stu-id="479b6-337">is correct because those parts that include constraints (the first two) effectively specify the same set of primary, secondary, and constructor constraints for the same set of type parameters, respectively.</span></span>

### <a name="base-class"></a><span data-ttu-id="479b6-338">Základní třída</span><span class="sxs-lookup"><span data-stu-id="479b6-338">Base class</span></span>

<span data-ttu-id="479b6-339">Když deklarace částečné třídy obsahuje specifikaci základní třídy, musí souhlasit se všemi ostatními částmi, které obsahují specifikaci základní třídy.</span><span class="sxs-lookup"><span data-stu-id="479b6-339">When a partial class declaration includes a base class specification it must agree with all other parts that include a base class specification.</span></span> <span data-ttu-id="479b6-340">Pokud žádná část částečné třídy neobsahuje specifikaci základní třídy, bude základní třída `System.Object` ([základní třídy](classes.md#base-classes)).</span><span class="sxs-lookup"><span data-stu-id="479b6-340">If no part of a partial class includes a base class specification, the base class becomes `System.Object` ([Base classes](classes.md#base-classes)).</span></span>

### <a name="base-interfaces"></a><span data-ttu-id="479b6-341">Základní rozhraní</span><span class="sxs-lookup"><span data-stu-id="479b6-341">Base interfaces</span></span>

<span data-ttu-id="479b6-342">Sada základních rozhraní pro typ deklarovaných ve více částech je sjednocení základních rozhraní specifikovaných v každé části.</span><span class="sxs-lookup"><span data-stu-id="479b6-342">The set of base interfaces for a type declared in multiple parts is the union of the base interfaces specified on each part.</span></span> <span data-ttu-id="479b6-343">Konkrétní základní rozhraní může být v každé části pojmenováno pouze jednou, ale je povoleno pro více částí názvů stejných základních rozhraní.</span><span class="sxs-lookup"><span data-stu-id="479b6-343">A particular base interface may only be named once on each part, but it is permitted for multiple parts to name the same base interface(s).</span></span> <span data-ttu-id="479b6-344">Musí existovat pouze jedna implementace členů kteréhokoli daného základního rozhraní.</span><span class="sxs-lookup"><span data-stu-id="479b6-344">There must only be one implementation of the members of any given base interface.</span></span>

<span data-ttu-id="479b6-345">V příkladu</span><span class="sxs-lookup"><span data-stu-id="479b6-345">In the example</span></span>
```csharp
partial class C: IA, IB {...}

partial class C: IC {...}

partial class C: IA, IB {...}
```
<span data-ttu-id="479b6-346">sada základních rozhraní pro `C` třídu je `IA`, `IB`a `IC`.</span><span class="sxs-lookup"><span data-stu-id="479b6-346">the set of base interfaces for class `C` is `IA`, `IB`, and `IC`.</span></span>

<span data-ttu-id="479b6-347">Každá část obvykle poskytuje implementaci rozhraní deklarovaných v této části; Nejedná se však o požadavek.</span><span class="sxs-lookup"><span data-stu-id="479b6-347">Typically, each part provides an implementation of the interface(s) declared on that part; however, this is not a requirement.</span></span> <span data-ttu-id="479b6-348">Část může poskytovat implementaci rozhraní deklarovaného v jiné části:</span><span class="sxs-lookup"><span data-stu-id="479b6-348">A part may provide the implementation for an interface declared on a different part:</span></span>
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

### <a name="members"></a><span data-ttu-id="479b6-349">Členové</span><span class="sxs-lookup"><span data-stu-id="479b6-349">Members</span></span>

<span data-ttu-id="479b6-350">S výjimkou částečných metod ([částečné metody](classes.md#partial-methods)) je množina členů typu deklarovaného ve více částech jednoduše sjednocením sady členů deklarované v každé části.</span><span class="sxs-lookup"><span data-stu-id="479b6-350">With the exception of partial methods ([Partial methods](classes.md#partial-methods)), the set of members of a type declared in multiple parts is simply the union of the set of members declared in each part.</span></span> <span data-ttu-id="479b6-351">Těla všech částí deklarace typu sdílejí stejné místo deklarace ([deklarace](basic-concepts.md#declarations)) a rozsah jednotlivých členů ([oborů](basic-concepts.md#scopes)) se rozšiřuje na tělo všech částí.</span><span class="sxs-lookup"><span data-stu-id="479b6-351">The bodies of all parts of the type declaration share the same declaration space ([Declarations](basic-concepts.md#declarations)), and the scope of each member ([Scopes](basic-concepts.md#scopes)) extends to the bodies of all the parts.</span></span> <span data-ttu-id="479b6-352">Doména přístupnosti libovolného člena vždy obsahuje všechny části ohraničujícího typu; `private` člen deklarovaný v jedné části je volně dostupný z jiné části.</span><span class="sxs-lookup"><span data-stu-id="479b6-352">The accessibility domain of any member always includes all the parts of the enclosing type; a `private` member declared in one part is freely accessible from another part.</span></span> <span data-ttu-id="479b6-353">Jedná se o chybu při kompilaci, která deklaruje stejného člena ve více než jedné části typu, pokud tento člen není typu s `partial` modifikátorem.</span><span class="sxs-lookup"><span data-stu-id="479b6-353">It is a compile-time error to declare the same member in more than one part of the type, unless that member is a type with the `partial` modifier.</span></span>

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

<span data-ttu-id="479b6-354">Řazení členů v rámci typu je zřídka významné pro C# kód, ale může být významné při propojení s jinými jazyky a prostředími.</span><span class="sxs-lookup"><span data-stu-id="479b6-354">The ordering of members within a type is rarely significant to C# code, but may be significant when interfacing with other languages and environments.</span></span> <span data-ttu-id="479b6-355">V těchto případech není definováno řazení členů v rámci typu deklarovaného ve více částech.</span><span class="sxs-lookup"><span data-stu-id="479b6-355">In these cases, the ordering of members within a type declared in multiple parts is undefined.</span></span>

### <a name="partial-methods"></a><span data-ttu-id="479b6-356">Částečné metody</span><span class="sxs-lookup"><span data-stu-id="479b6-356">Partial methods</span></span>

<span data-ttu-id="479b6-357">Částečné metody mohou být definovány v jedné části deklarace typu a implementovány v jiném.</span><span class="sxs-lookup"><span data-stu-id="479b6-357">Partial methods can be defined in one part of a type declaration and implemented in another.</span></span> <span data-ttu-id="479b6-358">Implementace je volitelná; Pokud žádná část neimplementuje částečnou metodu, deklarace částečné metody a všechna volání jsou z deklarace typu vycházející z kombinace částí.</span><span class="sxs-lookup"><span data-stu-id="479b6-358">The implementation is optional; if no part implements the partial method, the partial method declaration and all calls to it are removed from the type declaration resulting from the combination of the parts.</span></span>

<span data-ttu-id="479b6-359">Částečné metody nemůžou definovat modifikátory přístupu, ale jsou implicitně `private`.</span><span class="sxs-lookup"><span data-stu-id="479b6-359">Partial methods cannot define access modifiers, but are implicitly `private`.</span></span> <span data-ttu-id="479b6-360">Návratový typ musí být `void`a jejich parametry nemohou `out` mít modifikátor.</span><span class="sxs-lookup"><span data-stu-id="479b6-360">Their return type must be `void`, and their parameters cannot have the `out` modifier.</span></span> <span data-ttu-id="479b6-361">Identifikátor `partial` je rozpoznán jako speciální klíčové slovo v deklaraci metody pouze v případě, že se zobrazuje přímo `void` před typem. v opačném případě lze použít jako normální identifikátor.</span><span class="sxs-lookup"><span data-stu-id="479b6-361">The identifier `partial` is recognized as a special keyword in a method declaration only if it appears right before the `void` type; otherwise it can be used as a normal identifier.</span></span> <span data-ttu-id="479b6-362">Částečná metoda nemůže explicitně implementovat metody rozhraní.</span><span class="sxs-lookup"><span data-stu-id="479b6-362">A partial method cannot explicitly implement interface methods.</span></span>

<span data-ttu-id="479b6-363">Existují dva druhy deklarací částečné metody: Je-li tělo deklarace metody středníkem, deklarace je označována jako ***definice částečné deklarace metody***.</span><span class="sxs-lookup"><span data-stu-id="479b6-363">There are two kinds of partial method declarations: If the body of the method declaration is a semicolon, the declaration is said to be a ***defining partial method declaration***.</span></span> <span data-ttu-id="479b6-364">Pokud je text uveden jako *blok*, deklarace je označována jako ***implementace částečné deklarace metody***.</span><span class="sxs-lookup"><span data-stu-id="479b6-364">If the body is given as a *block*, the declaration is said to be an ***implementing partial method declaration***.</span></span> <span data-ttu-id="479b6-365">V rámci deklarace typu může být pouze jedna třída definující deklaraci částečné metody s daným podpisem a může existovat pouze jedna implementace částečné deklarace metody s daným podpisem.</span><span class="sxs-lookup"><span data-stu-id="479b6-365">Across the parts of a type declaration there can be only one defining partial method declaration with a given signature, and there can be only one implementing partial method declaration with a given signature.</span></span> <span data-ttu-id="479b6-366">Pokud je poskytnuta deklarace částečné metody, musí existovat odpovídající definující deklarace částečné metody a deklarace se musí shodovat, jak je uvedeno v následujícím seznamu:</span><span class="sxs-lookup"><span data-stu-id="479b6-366">If an implementing partial method declaration is given, a corresponding defining partial method declaration must exist, and the declarations must match as specified in the following:</span></span>

* <span data-ttu-id="479b6-367">Deklarace musí mít stejné modifikátory (i když ne nutně ve stejném pořadí), název metody, počet parametrů typu a počet parametrů.</span><span class="sxs-lookup"><span data-stu-id="479b6-367">The declarations must have the same modifiers (although not necessarily in the same order), method name, number of type parameters and number of parameters.</span></span>
* <span data-ttu-id="479b6-368">Odpovídající parametry v deklaracích musí mít stejné modifikátory (i když ne nutně ve stejném pořadí) a stejné typy (rozdíly modulo v názvech parametrů typu).</span><span class="sxs-lookup"><span data-stu-id="479b6-368">Corresponding parameters in the declarations must have the same modifiers (although not necessarily in the same order) and the same types (modulo differences in type parameter names).</span></span>
* <span data-ttu-id="479b6-369">Odpovídající parametry typu v deklaracích musí mít stejná omezení (rozdíly modulo v názvech parametrů typu).</span><span class="sxs-lookup"><span data-stu-id="479b6-369">Corresponding type parameters in the declarations must have the same constraints (modulo differences in type parameter names).</span></span>

<span data-ttu-id="479b6-370">Implementace částečné deklarace metody se může vyskytovat ve stejné části jako odpovídající deklarace částečné metody.</span><span class="sxs-lookup"><span data-stu-id="479b6-370">An implementing partial method declaration can appear in the same part as the corresponding defining partial method declaration.</span></span>

<span data-ttu-id="479b6-371">V rámci řešení přetížení se účastní pouze definující částečná metoda.</span><span class="sxs-lookup"><span data-stu-id="479b6-371">Only a defining partial method participates in overload resolution.</span></span> <span data-ttu-id="479b6-372">Proto bez ohledu na to, zda je deklarace implementována, mohou být výrazy volání přeloženy na vyvolání částečné metody.</span><span class="sxs-lookup"><span data-stu-id="479b6-372">Thus, whether or not an implementing declaration is given, invocation expressions may resolve to invocations of the partial method.</span></span> <span data-ttu-id="479b6-373">Vzhledem k tomu, že částečná metoda vždy vrátí hodnotu `void`, takové výrazy vyvolání budou vždy příkazy výrazu.</span><span class="sxs-lookup"><span data-stu-id="479b6-373">Because a partial method always returns `void`, such invocation expressions will always be expression statements.</span></span> <span data-ttu-id="479b6-374">Vzhledem k tomu, že částečná metoda je `private`implicitně implicitní, takové příkazy budou vždy provedeny v rámci jedné z částí deklarace typu, v rámci které je deklarována částečná metoda.</span><span class="sxs-lookup"><span data-stu-id="479b6-374">Furthermore, because a partial method is implicitly `private`, such statements will always occur within one of the parts of the type declaration within which the partial method is declared.</span></span>

<span data-ttu-id="479b6-375">Pokud žádná část deklarace částečného typu neobsahuje implementaci deklarace pro danou částečnou metodu, jakýkoli příkaz výrazu, který je vyvolán, je jednoduše odebrán z deklarace kombinovaného typu.</span><span class="sxs-lookup"><span data-stu-id="479b6-375">If no part of a partial type declaration contains an implementing declaration for a given partial method, any expression statement invoking it is simply removed from the combined type declaration.</span></span> <span data-ttu-id="479b6-376">Proto výraz vyvolání, včetně všech základních výrazů, nemá žádný vliv na dobu běhu.</span><span class="sxs-lookup"><span data-stu-id="479b6-376">Thus the invocation expression, including any constituent expressions, has no effect at run-time.</span></span> <span data-ttu-id="479b6-377">Částečná metoda je také odebrána a nebude členem kombinované deklarace typu.</span><span class="sxs-lookup"><span data-stu-id="479b6-377">The partial method itself is also removed and will not be a member of the combined type declaration.</span></span>

<span data-ttu-id="479b6-378">Pokud pro danou částečnou metodu existuje implementovaná deklarace, jsou zachovány volání částečných metod.</span><span class="sxs-lookup"><span data-stu-id="479b6-378">If an implementing declaration exist for a given partial method, the invocations of the partial methods are retained.</span></span> <span data-ttu-id="479b6-379">Částečná metoda dává deklaraci metody podobně jako implementace částečné metody deklarace, s výjimkou následujících:</span><span class="sxs-lookup"><span data-stu-id="479b6-379">The partial method gives rise to a method declaration similar to the implementing partial method declaration except for the following:</span></span>

* <span data-ttu-id="479b6-380">`partial` Modifikátor není zahrnutý.</span><span class="sxs-lookup"><span data-stu-id="479b6-380">The `partial` modifier is not included</span></span>
* <span data-ttu-id="479b6-381">Atributy v deklaraci výsledné metody jsou kombinované atributy definující a implementující deklaraci částečné metody v nespecifikovaném pořadí.</span><span class="sxs-lookup"><span data-stu-id="479b6-381">The attributes in the resulting method declaration are the combined attributes of the defining and the implementing partial method declaration in unspecified order.</span></span> <span data-ttu-id="479b6-382">Duplicity nejsou odebrány.</span><span class="sxs-lookup"><span data-stu-id="479b6-382">Duplicates are not removed.</span></span>
* <span data-ttu-id="479b6-383">Atributy parametrů výsledné deklarace metody jsou kombinované atributy odpovídajících parametrů definující a implementující deklaraci částečné metody v nespecifikovaném pořadí.</span><span class="sxs-lookup"><span data-stu-id="479b6-383">The attributes on the parameters of the resulting method declaration are the combined attributes of the corresponding parameters of the defining and the implementing partial method declaration in unspecified order.</span></span> <span data-ttu-id="479b6-384">Duplicity nejsou odebrány.</span><span class="sxs-lookup"><span data-stu-id="479b6-384">Duplicates are not removed.</span></span>

<span data-ttu-id="479b6-385">Pokud je udělena deklarace definující deklaraci, ale nikoli implementující deklaraci, platí následující omezení:</span><span class="sxs-lookup"><span data-stu-id="479b6-385">If a defining declaration but not an implementing declaration is given for a partial method M, the following restrictions apply:</span></span>

* <span data-ttu-id="479b6-386">Jedná se o chybu při kompilaci, která umožňuje vytvořit delegáta metody ([výrazy vytváření delegátů](expressions.md#delegate-creation-expressions)).</span><span class="sxs-lookup"><span data-stu-id="479b6-386">It is a compile-time error to create a delegate to method ([Delegate creation expressions](expressions.md#delegate-creation-expressions)).</span></span>
* <span data-ttu-id="479b6-387">Jedná se o chybu při kompilaci, na kterou se `M` odkazuje uvnitř anonymní funkce, která je převedena na typ stromu výrazu ([vyhodnocení anonymních převodů funkcí na typy stromu výrazů](conversions.md#evaluation-of-anonymous-function-conversions-to-expression-tree-types)).</span><span class="sxs-lookup"><span data-stu-id="479b6-387">It is a compile-time error to refer to `M` inside an anonymous function that is converted to an expression tree type ([Evaluation of anonymous function conversions to expression tree types](conversions.md#evaluation-of-anonymous-function-conversions-to-expression-tree-types)).</span></span>
* <span data-ttu-id="479b6-388">Výrazy, které se vyskytují jako součást vyvolání, `M` nemají vliv na určitý stav přiřazení ([jednoznačné přiřazení](variables.md#definite-assignment)), což může potenciálně vést k chybám při kompilaci.</span><span class="sxs-lookup"><span data-stu-id="479b6-388">Expressions occurring as part of an invocation of `M` do not affect the definite assignment state ([Definite assignment](variables.md#definite-assignment)), which can potentially lead to compile-time errors.</span></span>
* <span data-ttu-id="479b6-389">`M`nemůže být vstupním bodem aplikace ([spuštění aplikace](basic-concepts.md#application-startup)).</span><span class="sxs-lookup"><span data-stu-id="479b6-389">`M` cannot be the entry point for an application ([Application Startup](basic-concepts.md#application-startup)).</span></span>

<span data-ttu-id="479b6-390">Částečné metody jsou užitečné pro umožnění jedné části deklarace typu k přizpůsobení chování jiné části, například, který je generován nástrojem.</span><span class="sxs-lookup"><span data-stu-id="479b6-390">Partial methods are useful for allowing one part of a type declaration to customize the behavior of another part, e.g., one that is generated by a tool.</span></span> <span data-ttu-id="479b6-391">Zvažte následující deklaraci částečné třídy:</span><span class="sxs-lookup"><span data-stu-id="479b6-391">Consider the following partial class declaration:</span></span>
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

<span data-ttu-id="479b6-392">Pokud je tato třída zkompilována bez jakýchkoli jiných částí, definice deklarací částečné metody a jejich vyvolání budou odebrány a výsledná kombinovaná deklarace třídy bude odpovídat následujícímu:</span><span class="sxs-lookup"><span data-stu-id="479b6-392">If this class is compiled without any other parts, the defining partial method declarations and their invocations will be removed, and the resulting combined class declaration will be equivalent to the following:</span></span>
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

<span data-ttu-id="479b6-393">Předpokládejme, že je k dispozici další část, která poskytuje implementaci deklarace částečných metod:</span><span class="sxs-lookup"><span data-stu-id="479b6-393">Assume that another part is given, however, which provides implementing declarations of the partial methods:</span></span>
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

<span data-ttu-id="479b6-394">Nakonec bude výsledná kombinovaná deklarace třídy ekvivalentní následujícímu:</span><span class="sxs-lookup"><span data-stu-id="479b6-394">Then the resulting combined class declaration will be equivalent to the following:</span></span>
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

### <a name="name-binding"></a><span data-ttu-id="479b6-395">Vazba názvu</span><span class="sxs-lookup"><span data-stu-id="479b6-395">Name binding</span></span>

<span data-ttu-id="479b6-396">Přestože každá část rozšiřitelného typu musí být deklarována v rámci stejného oboru názvů, části jsou obvykle zapisovány v rámci různých deklarací oboru názvů.</span><span class="sxs-lookup"><span data-stu-id="479b6-396">Although each part of an extensible type must be declared within the same namespace, the parts are typically written within different namespace declarations.</span></span> <span data-ttu-id="479b6-397">Proto mohou být `using` pro každou část k dispozici různé direktivy ([direktivy using](namespaces.md#using-directives)).</span><span class="sxs-lookup"><span data-stu-id="479b6-397">Thus, different `using` directives ([Using directives](namespaces.md#using-directives)) may be present for each part.</span></span> <span data-ttu-id="479b6-398">Při interpretaci jednoduchých názvů ([odvození typu](expressions.md#type-inference)) v rámci jedné součásti se považují pouze `using` direktivy oboru názvů, které obklopují tuto část.</span><span class="sxs-lookup"><span data-stu-id="479b6-398">When interpreting simple names ([Type inference](expressions.md#type-inference)) within one part, only the `using` directives of the namespace declaration(s) enclosing that part are considered.</span></span> <span data-ttu-id="479b6-399">To může mít za následek, že stejný identifikátor má různé významy v různých částech:</span><span class="sxs-lookup"><span data-stu-id="479b6-399">This may result in the same identifier having different meanings in different parts:</span></span>
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

## <a name="class-members"></a><span data-ttu-id="479b6-400">Členové třídy</span><span class="sxs-lookup"><span data-stu-id="479b6-400">Class members</span></span>

<span data-ttu-id="479b6-401">Členy třídy se skládají ze členů zavedených jeho *class_member_declaration*s a členy zděděných z přímé základní třídy.</span><span class="sxs-lookup"><span data-stu-id="479b6-401">The members of a class consist of the members introduced by its *class_member_declaration*s and the members inherited from the direct base class.</span></span>

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

<span data-ttu-id="479b6-402">Členy typu třídy jsou rozděleny do následujících kategorií:</span><span class="sxs-lookup"><span data-stu-id="479b6-402">The members of a class type are divided into the following categories:</span></span>

*  <span data-ttu-id="479b6-403">Konstanty, které reprezentují konstantní hodnoty přidružené ke třídě ([konstanty](classes.md#constants)).</span><span class="sxs-lookup"><span data-stu-id="479b6-403">Constants, which represent constant values associated with the class ([Constants](classes.md#constants)).</span></span>
*  <span data-ttu-id="479b6-404">Pole, která jsou proměnné třídy ([pole](classes.md#fields)).</span><span class="sxs-lookup"><span data-stu-id="479b6-404">Fields, which are the variables of the class ([Fields](classes.md#fields)).</span></span>
*  <span data-ttu-id="479b6-405">Metody, které implementují výpočty a akce, které mohou být provedeny třídou ([metody](classes.md#methods)).</span><span class="sxs-lookup"><span data-stu-id="479b6-405">Methods, which implement the computations and actions that can be performed by the class ([Methods](classes.md#methods)).</span></span>
*  <span data-ttu-id="479b6-406">Vlastnosti, které definují pojmenované vlastnosti a akce spojené s čtením a zápisem těchto vlastností ([vlastností](classes.md#properties)).</span><span class="sxs-lookup"><span data-stu-id="479b6-406">Properties, which define named characteristics and the actions associated with reading and writing those characteristics ([Properties](classes.md#properties)).</span></span>
*  <span data-ttu-id="479b6-407">Události, které definují oznámení, která mohou být vygenerována třídou ([událostmi](classes.md#events)).</span><span class="sxs-lookup"><span data-stu-id="479b6-407">Events, which define notifications that can be generated by the class ([Events](classes.md#events)).</span></span>
*  <span data-ttu-id="479b6-408">Indexery, které umožňují indexování instancí třídy stejným způsobem (syntakticky) jako pole ([indexery](classes.md#indexers)).</span><span class="sxs-lookup"><span data-stu-id="479b6-408">Indexers, which permit instances of the class to be indexed in the same way (syntactically) as arrays ([Indexers](classes.md#indexers)).</span></span>
*  <span data-ttu-id="479b6-409">Operátory, které definují operátory výrazů, které mohou být aplikovány na instance třídy ([Operators](classes.md#operators)).</span><span class="sxs-lookup"><span data-stu-id="479b6-409">Operators, which define the expression operators that can be applied to instances of the class ([Operators](classes.md#operators)).</span></span>
*  <span data-ttu-id="479b6-410">Konstruktory instancí, které implementují akce vyžadované k inicializaci instancí třídy ([konstruktory instancí](classes.md#instance-constructors))</span><span class="sxs-lookup"><span data-stu-id="479b6-410">Instance constructors, which implement the actions required to initialize instances of the class ([Instance constructors](classes.md#instance-constructors))</span></span>
*  <span data-ttu-id="479b6-411">Destruktory, které implementují akce, které mají být provedeny před tím, než dojde k trvalému zahození instancí třídy ([destruktory](classes.md#destructors)).</span><span class="sxs-lookup"><span data-stu-id="479b6-411">Destructors, which implement the actions to be performed before instances of the class are permanently discarded ([Destructors](classes.md#destructors)).</span></span>
*  <span data-ttu-id="479b6-412">Statické konstruktory, které implementují akce vyžadované k inicializaci samotné třídy ([statické konstruktory](classes.md#static-constructors)).</span><span class="sxs-lookup"><span data-stu-id="479b6-412">Static constructors, which implement the actions required to initialize the class itself ([Static constructors](classes.md#static-constructors)).</span></span>
*  <span data-ttu-id="479b6-413">Typy, které představují typy, které jsou místní pro třídu ([vnořené typy](classes.md#nested-types)).</span><span class="sxs-lookup"><span data-stu-id="479b6-413">Types, which represent the types that are local to the class ([Nested types](classes.md#nested-types)).</span></span>

<span data-ttu-id="479b6-414">Členy, které mohou obsahovat spustitelný kód, jsou souhrnně označovány jako *Členové funkce* typu třídy.</span><span class="sxs-lookup"><span data-stu-id="479b6-414">Members that can contain executable code are collectively known as the *function members* of the class type.</span></span> <span data-ttu-id="479b6-415">Členy funkce typu třídy jsou metody, vlastnosti, události, indexery, operátory, konstruktory instancí, destruktory a statické konstruktory daného typu třídy.</span><span class="sxs-lookup"><span data-stu-id="479b6-415">The function members of a class type are the methods, properties, events, indexers, operators, instance constructors,  destructors, and static constructors of that class type.</span></span>

<span data-ttu-id="479b6-416">*Class_declaration* vytvoří nové místo deklarace ([deklarace](basic-concepts.md#declarations)) a *class_member_declarationy*, které jsou okamžitě obsaženy v *class_declaration* , zavádí nové členy do tohoto prostoru deklarací.</span><span class="sxs-lookup"><span data-stu-id="479b6-416">A *class_declaration* creates a new declaration space ([Declarations](basic-concepts.md#declarations)), and the *class_member_declaration*s immediately contained by the *class_declaration* introduce new members into this declaration space.</span></span> <span data-ttu-id="479b6-417">Následující pravidla platí pro *class_member_declaration*s:</span><span class="sxs-lookup"><span data-stu-id="479b6-417">The following rules apply to *class_member_declaration*s:</span></span>

*  <span data-ttu-id="479b6-418">Konstruktory instancí, destruktory a statické konstruktory musí mít stejný název jako bezprostředně ohraničující třída.</span><span class="sxs-lookup"><span data-stu-id="479b6-418">Instance constructors, destructors and static constructors must have the same name as the immediately enclosing class.</span></span> <span data-ttu-id="479b6-419">Všichni ostatní členové musí mít názvy, které se liší od názvu bezprostředně ohraničující třídy.</span><span class="sxs-lookup"><span data-stu-id="479b6-419">All other members must have names that differ from the name of the immediately enclosing class.</span></span>
*  <span data-ttu-id="479b6-420">Název konstanty, pole, vlastnosti, události nebo typu se musí lišit od názvů všech ostatních členů deklarovaných ve stejné třídě.</span><span class="sxs-lookup"><span data-stu-id="479b6-420">The name of a constant, field, property, event, or type must differ from the names of all other members declared in the same class.</span></span>
*  <span data-ttu-id="479b6-421">Název metody se musí lišit od názvů všech ostatních neodpovídajících metod deklarovaných ve stejné třídě.</span><span class="sxs-lookup"><span data-stu-id="479b6-421">The name of a method must differ from the names of all other non-methods declared in the same class.</span></span> <span data-ttu-id="479b6-422">Kromě toho signatura ([signatury a přetížení](basic-concepts.md#signatures-and-overloading)) metody se musí lišit od signatur všech ostatních metod deklarovaných ve stejné třídě a dvě metody deklarované ve stejné třídě nemusí mít signatury, které se liší výhradně pomocí `ref` a. `out`.</span><span class="sxs-lookup"><span data-stu-id="479b6-422">In addition, the signature ([Signatures and overloading](basic-concepts.md#signatures-and-overloading)) of a method must differ from the signatures of all other methods declared in the same class, and two methods declared in the same class may not have signatures that differ solely by `ref` and `out`.</span></span>
*  <span data-ttu-id="479b6-423">Signatura konstruktoru instance se musí lišit od signatur všech ostatních konstruktorů instancí deklarovaných ve stejné třídě a dva konstruktory deklarované ve stejné třídě nemusí mít signatury, které se liší výhradně pomocí `ref` a. `out`</span><span class="sxs-lookup"><span data-stu-id="479b6-423">The signature of an instance constructor must differ from the signatures of all other instance constructors declared in the same class, and two constructors declared in the same class may not have signatures that differ solely by `ref` and `out`.</span></span>
*  <span data-ttu-id="479b6-424">Signatura indexeru se musí lišit od signatur všech ostatních indexerů deklarovaných ve stejné třídě.</span><span class="sxs-lookup"><span data-stu-id="479b6-424">The signature of an indexer must differ from the signatures of all other indexers declared in the same class.</span></span>
*  <span data-ttu-id="479b6-425">Signatura operátoru se musí lišit od signatur všech ostatních operátorů deklarovaných ve stejné třídě.</span><span class="sxs-lookup"><span data-stu-id="479b6-425">The signature of an operator must differ from the signatures of all other operators declared in the same class.</span></span>

<span data-ttu-id="479b6-426">Zděděné členy typu třídy ([Dědičnost](classes.md#inheritance)) nejsou součástí prostoru deklarací třídy.</span><span class="sxs-lookup"><span data-stu-id="479b6-426">The inherited members of a class type ([Inheritance](classes.md#inheritance)) are not part of the declaration space of a class.</span></span> <span data-ttu-id="479b6-427">Odvozená třída proto může deklarovat člen se stejným názvem nebo signaturou jako zděděný člen (což v důsledku toho skrývá zděděný člen).</span><span class="sxs-lookup"><span data-stu-id="479b6-427">Thus, a derived class is allowed to declare a member with the same name or signature as an inherited member (which in effect hides the inherited member).</span></span>

### <a name="the-instance-type"></a><span data-ttu-id="479b6-428">Typ instance</span><span class="sxs-lookup"><span data-stu-id="479b6-428">The instance type</span></span>

<span data-ttu-id="479b6-429">Každá deklarace třídy má přidružený vázaný typ ([vázané a nevázané typy](types.md#bound-and-unbound-types)), ***typ instance***.</span><span class="sxs-lookup"><span data-stu-id="479b6-429">Each class declaration has an associated bound type ([Bound and unbound types](types.md#bound-and-unbound-types)), the ***instance type***.</span></span> <span data-ttu-id="479b6-430">Pro deklaraci obecné třídy je typ instance vytvořen vytvořením konstruovaného typu ([vytvořené typy](types.md#constructed-types)) z deklarace typu, přičemž každý ze zadaných argumentů typu je odpovídajícím parametrem typu.</span><span class="sxs-lookup"><span data-stu-id="479b6-430">For a generic class declaration, the instance type is formed by creating a constructed type ([Constructed types](types.md#constructed-types)) from the type declaration, with each of the supplied type arguments being the corresponding type parameter.</span></span> <span data-ttu-id="479b6-431">Vzhledem k tomu, že typ instance používá parametry typu, lze použít pouze v případě, že jsou parametry typu v oboru; To je uvnitř deklarace třídy.</span><span class="sxs-lookup"><span data-stu-id="479b6-431">Since the instance type uses the type parameters, it can only be used where the type parameters are in scope; that is, inside the class declaration.</span></span> <span data-ttu-id="479b6-432">Typ instance je typ `this` pro kód napsaný uvnitř deklarace třídy.</span><span class="sxs-lookup"><span data-stu-id="479b6-432">The instance type is the type of `this` for code written inside the class declaration.</span></span> <span data-ttu-id="479b6-433">Pro jiné než obecné třídy je typ instance jednoduše deklarovanou třídou.</span><span class="sxs-lookup"><span data-stu-id="479b6-433">For non-generic classes, the instance type is simply the declared class.</span></span> <span data-ttu-id="479b6-434">Následující znázorňuje několik deklarací třídy spolu s jejich typy instancí:</span><span class="sxs-lookup"><span data-stu-id="479b6-434">The following shows several class declarations along with their instance types:</span></span> 
```csharp
class A<T>                           // instance type: A<T>
{
    class B {}                       // instance type: A<T>.B
    class C<U> {}                    // instance type: A<T>.C<U>
}

class D {}                           // instance type: D
```

### <a name="members-of-constructed-types"></a><span data-ttu-id="479b6-435">Členové konstruovaných typů</span><span class="sxs-lookup"><span data-stu-id="479b6-435">Members of constructed types</span></span>

<span data-ttu-id="479b6-436">Nezděděné členy konstruovaného typu jsou získány nahrazením, pro každý *type_parameter* v deklaraci členu, odpovídající *type_argument* konstruovaného typu.</span><span class="sxs-lookup"><span data-stu-id="479b6-436">The non-inherited members of a constructed type are obtained by substituting, for each *type_parameter* in the member declaration, the corresponding *type_argument* of the constructed type.</span></span> <span data-ttu-id="479b6-437">Proces nahrazení je založen na sémantickém významu deklarace typu a není pouhou náhradou textu.</span><span class="sxs-lookup"><span data-stu-id="479b6-437">The substitution process is based on the semantic meaning of type declarations, and is not simply textual substitution.</span></span>

<span data-ttu-id="479b6-438">Například s ohledem na obecnou deklaraci třídy</span><span class="sxs-lookup"><span data-stu-id="479b6-438">For example, given the generic class declaration</span></span>
```csharp
class Gen<T,U>
{
    public T[,] a;
    public void G(int i, T t, Gen<U,T> gt) {...}
    public U Prop { get {...} set {...} }
    public int H(double d) {...}
}
```
<span data-ttu-id="479b6-439">konstruovaný typ `Gen<int[],IComparable<string>>` má následující členy:</span><span class="sxs-lookup"><span data-stu-id="479b6-439">the constructed type `Gen<int[],IComparable<string>>` has the following members:</span></span>
```csharp
public int[,][] a;
public void G(int i, int[] t, Gen<IComparable<string>,int[]> gt) {...}
public IComparable<string> Prop { get {...} set {...} }
public int H(double d) {...}
```

<span data-ttu-id="479b6-440">Typ členu `a` v deklaraci `Gen` obecné třídy je " `T`dvojrozměrné pole", takže typ člena `a` v konstruovaném typu výše je "dvojrozměrné pole jednorozměrného pole ".`int`", nebo `int[,][]`.</span><span class="sxs-lookup"><span data-stu-id="479b6-440">The type of the member `a` in the generic class declaration `Gen` is "two-dimensional array of `T`", so the type of the member `a` in the constructed type above is "two-dimensional array of one-dimensional array of `int`", or `int[,][]`.</span></span>

<span data-ttu-id="479b6-441">V rámci členů funkce instance `this` je typem instance typ ([typ instance](classes.md#the-instance-type)) obsahující deklaraci.</span><span class="sxs-lookup"><span data-stu-id="479b6-441">Within instance function members, the type of `this` is the instance type ([The instance type](classes.md#the-instance-type)) of the containing declaration.</span></span>

<span data-ttu-id="479b6-442">Všichni členové obecné třídy mohou používat parametry typu z jakékoliv ohraničující třídy, a to buď přímo, nebo jako součást konstruovaného typu.</span><span class="sxs-lookup"><span data-stu-id="479b6-442">All members of a generic class can use type parameters from any enclosing class, either directly or as part of a constructed type.</span></span> <span data-ttu-id="479b6-443">Pokud je v době běhu použit konkrétní uzavřený konstruovaný typ ([otevřené a uzavřené typy](types.md#open-and-closed-types)), je každé použití parametru typu nahrazeno skutečným argumentem typu zadaným pro konstruovaný typ.</span><span class="sxs-lookup"><span data-stu-id="479b6-443">When a particular closed constructed type ([Open and closed types](types.md#open-and-closed-types)) is used at run-time, each use of a type parameter is replaced with the actual type argument supplied to the constructed type.</span></span> <span data-ttu-id="479b6-444">Příklad:</span><span class="sxs-lookup"><span data-stu-id="479b6-444">For example:</span></span>
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

### <a name="inheritance"></a><span data-ttu-id="479b6-445">Dědičnost</span><span class="sxs-lookup"><span data-stu-id="479b6-445">Inheritance</span></span>

<span data-ttu-id="479b6-446">Třída ***dědí*** členy svého typu přímé základní třídy.</span><span class="sxs-lookup"><span data-stu-id="479b6-446">A class ***inherits*** the members of its direct base class type.</span></span> <span data-ttu-id="479b6-447">Dědičnost znamená, že třída implicitně obsahuje všechny členy svého typu přímé základní třídy, s výjimkou konstruktorů instancí, destruktorů a statických konstruktorů základní třídy.</span><span class="sxs-lookup"><span data-stu-id="479b6-447">Inheritance means that a class implicitly contains all members of its direct base class type, except for the instance constructors, destructors and static constructors of the base class.</span></span> <span data-ttu-id="479b6-448">Mezi důležité aspekty dědičnosti patří:</span><span class="sxs-lookup"><span data-stu-id="479b6-448">Some important aspects of inheritance are:</span></span>

*  <span data-ttu-id="479b6-449">Dědičnost je tranzitivní.</span><span class="sxs-lookup"><span data-stu-id="479b6-449">Inheritance is transitive.</span></span> <span data-ttu-id="479b6-450">Pokud `C` je odvozen z `B`a `B` je odvozen z `A`, pak `C` dědí členy deklarované v `B` a také členy deklarované v `A`.</span><span class="sxs-lookup"><span data-stu-id="479b6-450">If `C` is derived from `B`, and `B` is derived from `A`, then `C` inherits the members declared in `B` as well as the members declared in `A`.</span></span>
*  <span data-ttu-id="479b6-451">Odvozená třída rozšiřuje svou přímou základní třídu.</span><span class="sxs-lookup"><span data-stu-id="479b6-451">A derived class extends its direct base class.</span></span> <span data-ttu-id="479b6-452">Odvozená třída může přidat nové členy do těch, které dědí, ale nemůže odebrat definici zděděného člena.</span><span class="sxs-lookup"><span data-stu-id="479b6-452">A derived class can add new members to those it inherits, but it cannot remove the definition of an inherited member.</span></span>
*  <span data-ttu-id="479b6-453">Konstruktory instancí, destruktory a statické konstruktory nejsou děděny, ale všichni ostatní členové jsou bez ohledu na deklaraci přístupu ([přístupu členů](basic-concepts.md#member-access)).</span><span class="sxs-lookup"><span data-stu-id="479b6-453">Instance constructors, destructors, and static constructors are not inherited, but all other members are, regardless of their declared accessibility ([Member access](basic-concepts.md#member-access)).</span></span> <span data-ttu-id="479b6-454">V závislosti na deklaraci přístupnosti ale nemusí být zděděné členy přístupné v odvozené třídě.</span><span class="sxs-lookup"><span data-stu-id="479b6-454">However, depending on their declared accessibility, inherited members might not be accessible in a derived class.</span></span>
*  <span data-ttu-id="479b6-455">Odvozená třída může ***Skrýt*** ([skrytím prostřednictvím dědičnosti](basic-concepts.md#hiding-through-inheritance)) zděděné členy deklarováním nových členů se stejným názvem nebo signaturou.</span><span class="sxs-lookup"><span data-stu-id="479b6-455">A derived class can ***hide*** ([Hiding through inheritance](basic-concepts.md#hiding-through-inheritance)) inherited members by declaring new members with the same name or signature.</span></span> <span data-ttu-id="479b6-456">Všimněte si, že při skrývání zděděného člena nedojde k odebrání tohoto člena – pouze k tomu, aby byl tento člen přístupný přímo prostřednictvím odvozené třídy.</span><span class="sxs-lookup"><span data-stu-id="479b6-456">Note however that hiding an inherited member does not remove that member—it merely makes that member inaccessible directly through the derived class.</span></span>
*  <span data-ttu-id="479b6-457">Instance třídy obsahuje sadu všech polí instance deklarované ve třídě a jejích základních třídách a implicitní převod ([implicitní převody odkazů](conversions.md#implicit-reference-conversions)) existují z odvozené třídy typu na libovolný z jeho základních typů tříd.</span><span class="sxs-lookup"><span data-stu-id="479b6-457">An instance of a class contains a set of all instance fields declared in the class and its base classes, and an implicit conversion ([Implicit reference conversions](conversions.md#implicit-reference-conversions)) exists from a derived class type to any of its base class types.</span></span> <span data-ttu-id="479b6-458">Proto odkaz na instanci některé odvozené třídy může být zpracován jako odkaz na instanci libovolné z jeho základních tříd.</span><span class="sxs-lookup"><span data-stu-id="479b6-458">Thus, a reference to an instance of some derived class can be treated as a reference to an instance of any of its base classes.</span></span>
*  <span data-ttu-id="479b6-459">Třída může deklarovat virtuální metody, vlastnosti a indexery a odvozené třídy mohou přepsat implementaci těchto členů funkce.</span><span class="sxs-lookup"><span data-stu-id="479b6-459">A class can declare virtual methods, properties, and indexers, and derived classes can override the implementation of these function members.</span></span> <span data-ttu-id="479b6-460">To umožňuje třídám navést k polymorfnímu chování při zaznamenání akcí prováděných vyvoláním členu funkce se liší v závislosti na typu běhu instance, jejímž prostřednictvím je tento člen funkce vyvolán.</span><span class="sxs-lookup"><span data-stu-id="479b6-460">This enables classes to exhibit polymorphic behavior wherein the actions performed by a function member invocation varies depending on the run-time type of the instance through which that function member is invoked.</span></span>

<span data-ttu-id="479b6-461">Zděděný člen typu konstruované třídy jsou členy okamžitého typu základní třídy ([základní třídy](classes.md#base-classes)), které jsou nalezeny nahrazením argumentů typu konstruovaného typu pro každý výskyt odpovídajících parametrů typu v  *specifikace class_base*</span><span class="sxs-lookup"><span data-stu-id="479b6-461">The inherited member of a constructed class type are the members of the immediate base class type ([Base classes](classes.md#base-classes)), which is found by substituting the type arguments of the constructed type for each occurrence of the corresponding type parameters in the *class_base* specification.</span></span> <span data-ttu-id="479b6-462">Tyto členy jsou následně transformovány nahrazením, pro každý *type_parameter* v deklaraci členu, odpovídající *type_argument* specifikace *class_base* .</span><span class="sxs-lookup"><span data-stu-id="479b6-462">These members, in turn, are transformed by substituting, for each *type_parameter* in the member declaration, the corresponding *type_argument* of the *class_base* specification.</span></span>

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

<span data-ttu-id="479b6-463">Ve výše uvedeném příkladu `D<int>` má konstruovaný typ nezděděný člen `public int G(string s)` získaný nahrazením argumentu `int` typu pro parametr `T`typu.</span><span class="sxs-lookup"><span data-stu-id="479b6-463">In the above example, the constructed type `D<int>` has a non-inherited member `public int G(string s)` obtained by substituting the type argument `int` for the type parameter `T`.</span></span> <span data-ttu-id="479b6-464">`D<int>`má také zděděného člena z deklarace `B`třídy.</span><span class="sxs-lookup"><span data-stu-id="479b6-464">`D<int>` also has an inherited member from the class declaration `B`.</span></span> <span data-ttu-id="479b6-465">Tento zděděný člen `B<int[]>` je určen prvním určením typu `D<int>` základní třídy nahrazením `int` pro `T` specifikaci `B<T[]>`základní třídy.</span><span class="sxs-lookup"><span data-stu-id="479b6-465">This inherited member is determined by first determining the base class type `B<int[]>` of `D<int>` by substituting `int` for `T` in the base class specification `B<T[]>`.</span></span> <span data-ttu-id="479b6-466">Pak jako argument `B`typu pro `int[]` je nahrazeno `U` v v `public U F(long index)`, pokud chcete převzít zděděného člena `public int[] F(long index)`.</span><span class="sxs-lookup"><span data-stu-id="479b6-466">Then, as a type argument to `B`, `int[]` is substituted for `U` in `public U F(long index)`, yielding the inherited member `public int[] F(long index)`.</span></span>

### <a name="the-new-modifier"></a><span data-ttu-id="479b6-467">Nový modifikátor</span><span class="sxs-lookup"><span data-stu-id="479b6-467">The new modifier</span></span>

<span data-ttu-id="479b6-468">*Class_member_declaration* má oprávnění deklarovat člena se stejným názvem nebo signaturou jako zděděný člen.</span><span class="sxs-lookup"><span data-stu-id="479b6-468">A *class_member_declaration* is permitted to declare a member with the same name or signature as an inherited member.</span></span> <span data-ttu-id="479b6-469">V případě, že k tomu dojde, člen odvozené třídy je uveden ke ***skrytí*** člena základní třídy.</span><span class="sxs-lookup"><span data-stu-id="479b6-469">When this occurs, the derived class member is said to ***hide*** the base class member.</span></span> <span data-ttu-id="479b6-470">Skrytí zděděného člena není považováno za chybu, ale způsobí, že kompilátor vydá upozornění.</span><span class="sxs-lookup"><span data-stu-id="479b6-470">Hiding an inherited member is not considered an error, but it does cause the compiler to issue a warning.</span></span> <span data-ttu-id="479b6-471">Chcete-li potlačit upozornění, deklaraci členu odvozené třídy může obsahovat `new` modifikátor, který označuje, že odvozený člen je určen pro skrytí základního člena.</span><span class="sxs-lookup"><span data-stu-id="479b6-471">To suppress the warning, the declaration of the derived class member can include a `new` modifier to indicate that the derived member is intended to hide the base member.</span></span> <span data-ttu-id="479b6-472">Toto téma se podrobněji popisuje [skrytím dědičnosti](basic-concepts.md#hiding-through-inheritance).</span><span class="sxs-lookup"><span data-stu-id="479b6-472">This topic is discussed further in [Hiding through inheritance](basic-concepts.md#hiding-through-inheritance).</span></span>

<span data-ttu-id="479b6-473">Pokud je v deklaraci, která neskrývá zděděný člen, zahrnut modifikátor,zobrazíseupozorněnínatentoefekt.`new`</span><span class="sxs-lookup"><span data-stu-id="479b6-473">If a `new` modifier is included in a declaration that doesn't hide an inherited member, a warning to that effect is issued.</span></span> <span data-ttu-id="479b6-474">Toto upozornění se potlačí odebráním `new` modifikátoru.</span><span class="sxs-lookup"><span data-stu-id="479b6-474">This warning is suppressed by removing the `new` modifier.</span></span>

### <a name="access-modifiers"></a><span data-ttu-id="479b6-475">Modifikátory přístupu</span><span class="sxs-lookup"><span data-stu-id="479b6-475">Access modifiers</span></span>

<span data-ttu-id="479b6-476">*Class_member_declaration* může mít jeden z pěti možných druhů deklarovaného usnadnění ([deklarovaný přístup](basic-concepts.md#declared-accessibility) `public`):, `protected internal`, `protected`, `internal`nebo `private`.</span><span class="sxs-lookup"><span data-stu-id="479b6-476">A *class_member_declaration* can have any one of the five possible kinds of declared accessibility ([Declared accessibility](basic-concepts.md#declared-accessibility)): `public`, `protected internal`, `protected`, `internal`, or `private`.</span></span> <span data-ttu-id="479b6-477">S výjimkou `protected internal` kombinace se jedná o chybu při kompilaci k určení více než jednoho modifikátoru přístupu.</span><span class="sxs-lookup"><span data-stu-id="479b6-477">Except for the `protected internal` combination, it is a compile-time error to specify more than one access modifier.</span></span> <span data-ttu-id="479b6-478">Pokud *class_member_declaration* nezahrnuje žádné modifikátory přístupu, `private` předpokládá se.</span><span class="sxs-lookup"><span data-stu-id="479b6-478">When a *class_member_declaration* does not include any access modifiers, `private` is assumed.</span></span>

### <a name="constituent-types"></a><span data-ttu-id="479b6-479">Typy prvků</span><span class="sxs-lookup"><span data-stu-id="479b6-479">Constituent types</span></span>

<span data-ttu-id="479b6-480">Typy, které se používají v deklaraci členu, se nazývají typy prvků daného člena.</span><span class="sxs-lookup"><span data-stu-id="479b6-480">Types that are used in the declaration of a member are called the constituent types of that member.</span></span> <span data-ttu-id="479b6-481">Možné typy prvků jsou typ konstanty, pole, vlastnost, událost nebo indexer, návratový typ metody nebo operátoru a typy parametrů metody, indexeru, operátoru nebo konstruktoru instance.</span><span class="sxs-lookup"><span data-stu-id="479b6-481">Possible constituent types are the type of a constant, field, property, event, or indexer, the return type of a method or operator, and the parameter types of a method, indexer, operator, or instance constructor.</span></span> <span data-ttu-id="479b6-482">Typy prvků členu musí být alespoň tak přístupné jako samotný člen ([Omezení přístupnosti](basic-concepts.md#accessibility-constraints)).</span><span class="sxs-lookup"><span data-stu-id="479b6-482">The constituent types of a member must be at least as accessible as that member itself ([Accessibility constraints](basic-concepts.md#accessibility-constraints)).</span></span>

### <a name="static-and-instance-members"></a><span data-ttu-id="479b6-483">Statické členy a členové instancí</span><span class="sxs-lookup"><span data-stu-id="479b6-483">Static and instance members</span></span>

<span data-ttu-id="479b6-484">Členy třídy jsou buď ***statické členy*** , nebo ***členy instance***.</span><span class="sxs-lookup"><span data-stu-id="479b6-484">Members of a class are either ***static members*** or ***instance members***.</span></span> <span data-ttu-id="479b6-485">Obecně řečeno, je vhodné si představit statické členy jako patřící do typů tříd a členů instancí jako patřící do objektů (instance typů tříd).</span><span class="sxs-lookup"><span data-stu-id="479b6-485">Generally speaking, it is useful to think of static members as belonging to class types and instance members as belonging to objects (instances of class types).</span></span>

<span data-ttu-id="479b6-486">Pokud pole, metoda, vlastnost, událost, operátor nebo deklarace konstruktoru obsahují `static` modifikátor, deklaruje statický člen.</span><span class="sxs-lookup"><span data-stu-id="479b6-486">When a field, method, property, event, operator, or constructor declaration includes a `static` modifier, it declares a static member.</span></span> <span data-ttu-id="479b6-487">Kromě toho deklarace konstanty nebo typu implicitně deklaruje statický člen.</span><span class="sxs-lookup"><span data-stu-id="479b6-487">In addition, a constant or type declaration implicitly declares a static member.</span></span> <span data-ttu-id="479b6-488">Statické členy mají následující vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="479b6-488">Static members have the following characteristics:</span></span>

*  <span data-ttu-id="479b6-489">Je-li na `M` *member_access* ([členský přístup](expressions.md#member-access)) formuláře `E.M`odkazováno na statický člen, `E` musí poznamenat typ obsahující `M`.</span><span class="sxs-lookup"><span data-stu-id="479b6-489">When a static member `M` is referenced in a *member_access* ([Member access](expressions.md#member-access)) of the form `E.M`, `E` must denote a type containing `M`.</span></span> <span data-ttu-id="479b6-490">Jedná se o chybu při kompilaci pro `E` označení instance.</span><span class="sxs-lookup"><span data-stu-id="479b6-490">It is a compile-time error for `E` to denote an instance.</span></span>
*  <span data-ttu-id="479b6-491">Statické pole identifikuje přesně jedno umístění úložiště, které se má sdílet všemi instancemi daného typu uzavřené třídy.</span><span class="sxs-lookup"><span data-stu-id="479b6-491">A static field identifies exactly one storage location to be shared by all instances of a given closed class type.</span></span> <span data-ttu-id="479b6-492">Bez ohledu na to, kolik instancí daného typu uzavřené třídy je vytvořeno, existuje pouze jedna kopie statického pole.</span><span class="sxs-lookup"><span data-stu-id="479b6-492">No matter how many instances of a given closed class type are created, there is only ever one copy of a static field.</span></span>
*  <span data-ttu-id="479b6-493">Člen statické funkce (metoda, vlastnost, událost, operátor nebo konstruktor) nepracuje na konkrétní instanci a jedná se o chybu při kompilaci, na kterou se odkazuje `this` v takovém členu funkce.</span><span class="sxs-lookup"><span data-stu-id="479b6-493">A static function member (method, property, event, operator, or constructor) does not operate on a specific instance, and it is a compile-time error to refer to `this` in such a function member.</span></span>

<span data-ttu-id="479b6-494">V případě, že deklarace `static` pole, metody, vlastnosti, události, indexer, konstruktoru nebo destruktoru neobsahuje modifikátor, deklaruje člen instance.</span><span class="sxs-lookup"><span data-stu-id="479b6-494">When a field, method, property, event, indexer, constructor, or destructor declaration does not include a `static` modifier, it declares an instance member.</span></span> <span data-ttu-id="479b6-495">(Člen instance se někdy označuje jako nestatický člen.) Členové instance mají následující vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="479b6-495">(An instance member is sometimes called a non-static member.) Instance members have the following characteristics:</span></span>

*  <span data-ttu-id="479b6-496">`M` Pokud je člen instance odkazován v *member_access* (přístupu ke[členu](expressions.md#member-access)) formuláře `E.M`, `E` musí poznamenat instanci typu obsahující `M`.</span><span class="sxs-lookup"><span data-stu-id="479b6-496">When an instance member `M` is referenced in a *member_access* ([Member access](expressions.md#member-access)) of the form `E.M`, `E` must denote an instance of a type containing `M`.</span></span> <span data-ttu-id="479b6-497">Jedná se o chybu při vazbě pro `E` zaznamenání typu.</span><span class="sxs-lookup"><span data-stu-id="479b6-497">It is a binding-time error for `E` to denote a type.</span></span>
*  <span data-ttu-id="479b6-498">Každá instance třídy obsahuje samostatnou sadu všech polí instance třídy.</span><span class="sxs-lookup"><span data-stu-id="479b6-498">Every instance of a class contains a separate set of all instance fields of the class.</span></span>
*  <span data-ttu-id="479b6-499">Člen funkce instance (metoda, vlastnost, indexer, konstruktor instance nebo destruktor) pracuje na dané instanci třídy a k této instanci může přistupovat jako `this` ([Tento přístup](expressions.md#this-access)).</span><span class="sxs-lookup"><span data-stu-id="479b6-499">An instance function member (method, property, indexer, instance constructor, or destructor) operates on a given instance of the class, and this instance can be accessed as `this` ([This access](expressions.md#this-access)).</span></span>

<span data-ttu-id="479b6-500">Následující příklad ukazuje pravidla pro přístup ke statickým a instancím členů:</span><span class="sxs-lookup"><span data-stu-id="479b6-500">The following example illustrates the rules for accessing static and instance members:</span></span>
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

<span data-ttu-id="479b6-501">Metoda ukazuje, že v členu funkce instance lze použít simple_name ([jednoduché názvy](expressions.md#simple-names)) pro přístup ke členům instance i ke statickým členům. `F`</span><span class="sxs-lookup"><span data-stu-id="479b6-501">The `F` method shows that in an instance function member, a *simple_name* ([Simple names](expressions.md#simple-names)) can be used to access both instance members and static members.</span></span> <span data-ttu-id="479b6-502">Metoda ukazuje, že ve statickém členovi funkce se jedná o chybu při kompilaci pro přístup k členu instance prostřednictvím *simple_name.* `G`</span><span class="sxs-lookup"><span data-stu-id="479b6-502">The `G` method shows that in a static function member, it is a compile-time error to access an instance member through a *simple_name*.</span></span> <span data-ttu-id="479b6-503">Metoda ukazuje, že v *member_access* ([přístupu ke členu](expressions.md#member-access)) musí být členové instance přístupné prostřednictvím instancí a ke statickým členům musí přistupovat prostřednictvím typů. `Main`</span><span class="sxs-lookup"><span data-stu-id="479b6-503">The `Main` method shows that in a *member_access* ([Member access](expressions.md#member-access)), instance members must be accessed through instances, and static members must be accessed through types.</span></span>

### <a name="nested-types"></a><span data-ttu-id="479b6-504">Vnořené typy</span><span class="sxs-lookup"><span data-stu-id="479b6-504">Nested types</span></span>

<span data-ttu-id="479b6-505">Typ deklarovaný v rámci deklarace třídy nebo struktury se nazývá ***vnořený typ***.</span><span class="sxs-lookup"><span data-stu-id="479b6-505">A type declared within a class or struct declaration is called a ***nested type***.</span></span> <span data-ttu-id="479b6-506">Typ, který je deklarován v rámci kompilační jednotky nebo oboru názvů je volána ***nevnořený typ***.</span><span class="sxs-lookup"><span data-stu-id="479b6-506">A type that is declared within a compilation unit or namespace is called a ***non-nested type***.</span></span>

<span data-ttu-id="479b6-507">V příkladu</span><span class="sxs-lookup"><span data-stu-id="479b6-507">In the example</span></span>
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
<span data-ttu-id="479b6-508">Třída `B` je vnořený typ, protože je deklarován v rámci `A`třídy a třída `A` je nevnořený typ, protože je deklarována v rámci kompilační jednotky.</span><span class="sxs-lookup"><span data-stu-id="479b6-508">class `B` is a nested type because it is declared within class `A`, and class `A` is a non-nested type because it is declared within a compilation unit.</span></span>

#### <a name="fully-qualified-name"></a><span data-ttu-id="479b6-509">Plně kvalifikovaný název</span><span class="sxs-lookup"><span data-stu-id="479b6-509">Fully qualified name</span></span>

<span data-ttu-id="479b6-510">Plně kvalifikovaný název ([plně kvalifikované názvy](basic-concepts.md#fully-qualified-names)) pro vnořený typ je `S.N` tam, kde `S` je plně kvalifikovaný název typu, ve kterém je deklarován `N` typ.</span><span class="sxs-lookup"><span data-stu-id="479b6-510">The fully qualified name ([Fully qualified names](basic-concepts.md#fully-qualified-names)) for a nested type is `S.N` where `S` is the fully qualified name of the type in which type `N` is declared.</span></span>

#### <a name="declared-accessibility"></a><span data-ttu-id="479b6-511">Deklarovaná přístupnost</span><span class="sxs-lookup"><span data-stu-id="479b6-511">Declared accessibility</span></span>

<span data-ttu-id="479b6-512">Nevnořené typy mohou mít `public` nebo `internal` být deklarované přístupnosti `internal` a mít ve výchozím nastavení deklaraci přístupnosti.</span><span class="sxs-lookup"><span data-stu-id="479b6-512">Non-nested types can have `public` or `internal` declared accessibility and have `internal` declared accessibility by default.</span></span> <span data-ttu-id="479b6-513">Vnořené typy mohou mít tyto formuláře deklarovaného přístupnosti a navíc jednu nebo více dalších forem deklarovaného usnadnění, v závislosti na tom, zda je obsažený typ třída nebo struktura:</span><span class="sxs-lookup"><span data-stu-id="479b6-513">Nested types can have these forms of declared accessibility too, plus one or more additional forms of declared accessibility, depending on whether the containing type is a class or struct:</span></span>

*  <span data-ttu-id="479b6-514">Vnořený typ deklarovaný ve třídě může mít některou z pěti forem deklarovaného přístupnosti (`public`, `protected` `internal` `protected internal`,, `private` nebo `private`) a, podobně jako ostatní členy třídy, je použita výchozí deklarace. přístup.</span><span class="sxs-lookup"><span data-stu-id="479b6-514">A nested type that is declared in a class can have any of five forms of declared accessibility (`public`, `protected internal`, `protected`, `internal`, or `private`) and, like other class members, defaults to `private` declared accessibility.</span></span>
*  <span data-ttu-id="479b6-515">Vnořený typ, který je deklarován ve struktuře, může mít jakékoli tři formy deklarovaného přístupnosti `internal`(`public`, `private`, nebo) a podobně jako ostatní členy struktury, `private` přičemž výchozí hodnota je deklarovaná přístupnost.</span><span class="sxs-lookup"><span data-stu-id="479b6-515">A nested type that is declared in a struct can have any of three forms of declared accessibility (`public`, `internal`, or `private`) and, like other struct members, defaults to `private` declared accessibility.</span></span>

<span data-ttu-id="479b6-516">Příklad</span><span class="sxs-lookup"><span data-stu-id="479b6-516">The example</span></span>
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
<span data-ttu-id="479b6-517">deklaruje soukromou vnořenou třídu `Node`.</span><span class="sxs-lookup"><span data-stu-id="479b6-517">declares a private nested class `Node`.</span></span>

#### <a name="hiding"></a><span data-ttu-id="479b6-518">Skrytí</span><span class="sxs-lookup"><span data-stu-id="479b6-518">Hiding</span></span>

<span data-ttu-id="479b6-519">Vnořený typ může skrýt ([Skrytí názvu](basic-concepts.md#name-hiding)) základního člena.</span><span class="sxs-lookup"><span data-stu-id="479b6-519">A nested type may hide ([Name hiding](basic-concepts.md#name-hiding)) a base member.</span></span> <span data-ttu-id="479b6-520">`new` Modifikátor je povolen pro deklarace vnořeného typu, takže skrytí lze vyjádřit explicitně.</span><span class="sxs-lookup"><span data-stu-id="479b6-520">The `new` modifier is permitted on nested type declarations so that hiding can be expressed explicitly.</span></span> <span data-ttu-id="479b6-521">Příklad</span><span class="sxs-lookup"><span data-stu-id="479b6-521">The example</span></span>
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
<span data-ttu-id="479b6-522">zobrazuje vnořenou třídu `M` , která skrývá metodu `M` definovanou v `Base`.</span><span class="sxs-lookup"><span data-stu-id="479b6-522">shows a nested class `M` that hides the method `M` defined in `Base`.</span></span>

#### <a name="this-access"></a><span data-ttu-id="479b6-523">Tento přístup</span><span class="sxs-lookup"><span data-stu-id="479b6-523">this access</span></span>

<span data-ttu-id="479b6-524">Vnořený typ a jeho obsahující typ nemají zvláštní vztah s ohledem na *This_Access* ([Tento přístup](expressions.md#this-access)).</span><span class="sxs-lookup"><span data-stu-id="479b6-524">A nested type and its containing type do not have a special relationship with regard to *this_access* ([This access](expressions.md#this-access)).</span></span> <span data-ttu-id="479b6-525">Konkrétně se `this` v rámci vnořeného typu nedá použít k odkazování na instance obsaženého typu.</span><span class="sxs-lookup"><span data-stu-id="479b6-525">Specifically, `this` within a nested type cannot be used to refer to instance members of the containing type.</span></span> <span data-ttu-id="479b6-526">V případech, kdy vnořený typ potřebuje přístup k členům instance svého nadřazeného typu, je možné přístup poskytnout poskytnutím `this` pro instanci nadřazeného typu jako argument konstruktoru pro vnořený typ.</span><span class="sxs-lookup"><span data-stu-id="479b6-526">In cases where a nested type needs access to the instance members of its containing type, access can be provided by providing the `this` for the instance of the containing type as a constructor argument for the nested type.</span></span> <span data-ttu-id="479b6-527">Následující příklad</span><span class="sxs-lookup"><span data-stu-id="479b6-527">The following example</span></span>
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
<span data-ttu-id="479b6-528">ukazuje tuto techniku.</span><span class="sxs-lookup"><span data-stu-id="479b6-528">shows this technique.</span></span> <span data-ttu-id="479b6-529">`C` Instance třídy `C`vytvoří instanci `Nested` třídy apředá`this` vlastní konstruktorkonstruktoru,abybylomožnéposkytnoutnáslednémupřístupučlenyinstance.`Nested`</span><span class="sxs-lookup"><span data-stu-id="479b6-529">An instance of `C` creates an instance of `Nested` and passes its own `this` to `Nested`'s constructor in order to provide subsequent access to `C`'s instance members.</span></span>

#### <a name="access-to-private-and-protected-members-of-the-containing-type"></a><span data-ttu-id="479b6-530">Přístup k soukromým a chráněným členům nadřazeného typu</span><span class="sxs-lookup"><span data-stu-id="479b6-530">Access to private and protected members of the containing type</span></span>

<span data-ttu-id="479b6-531">Vnořený typ má přístup ke všem členům, které mají přístup k jeho nadřazenému typu, včetně členů nadřazeného typu, který má `private` a `protected` deklarované přístupnosti.</span><span class="sxs-lookup"><span data-stu-id="479b6-531">A nested type has access to all of the members that are accessible to its containing type, including members of the containing type that have `private` and `protected` declared accessibility.</span></span> <span data-ttu-id="479b6-532">Příklad</span><span class="sxs-lookup"><span data-stu-id="479b6-532">The example</span></span>
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
<span data-ttu-id="479b6-533">zobrazuje třídu `C` , která obsahuje vnořenou třídu `Nested`.</span><span class="sxs-lookup"><span data-stu-id="479b6-533">shows a class `C` that contains a nested class `Nested`.</span></span> <span data-ttu-id="479b6-534">V `Nested`rámci, metoda `G` volá statickou `F` metodudefinovanou`C`v a máprivátnídeklaracipřístupnosti.`F`</span><span class="sxs-lookup"><span data-stu-id="479b6-534">Within `Nested`, the method `G` calls the static method `F` defined in `C`, and `F` has private declared accessibility.</span></span>

<span data-ttu-id="479b6-535">Vnořený typ také může přistupovat k chráněným členům definovaným v základním typu jeho nadřazeného typu.</span><span class="sxs-lookup"><span data-stu-id="479b6-535">A nested type also may access protected members defined in a base type of its containing type.</span></span> <span data-ttu-id="479b6-536">V příkladu</span><span class="sxs-lookup"><span data-stu-id="479b6-536">In the example</span></span>
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
<span data-ttu-id="479b6-537">vnořená třída `Derived.Nested` přistupuje k chráněné metodě `F` definované v `Derived`základní třídě `Derived`, `Base`pomocí volání prostřednictvím instance.</span><span class="sxs-lookup"><span data-stu-id="479b6-537">the nested class `Derived.Nested` accesses the protected method `F` defined in `Derived`'s base class, `Base`, by calling through an instance of `Derived`.</span></span>

#### <a name="nested-types-in-generic-classes"></a><span data-ttu-id="479b6-538">Vnořené typy v obecných třídách</span><span class="sxs-lookup"><span data-stu-id="479b6-538">Nested types in generic classes</span></span>

<span data-ttu-id="479b6-539">Deklarace obecné třídy může obsahovat deklarace vnořeného typu.</span><span class="sxs-lookup"><span data-stu-id="479b6-539">A generic class declaration can contain nested type declarations.</span></span> <span data-ttu-id="479b6-540">V rámci vnořených typů lze použít parametry typu ohraničující třídy.</span><span class="sxs-lookup"><span data-stu-id="479b6-540">The type parameters of the enclosing class can be used within the nested types.</span></span> <span data-ttu-id="479b6-541">Vnořená deklarace typu může obsahovat další parametry typu, které platí pouze pro vnořený typ.</span><span class="sxs-lookup"><span data-stu-id="479b6-541">A nested type declaration can contain additional type parameters that apply only to the nested type.</span></span>

<span data-ttu-id="479b6-542">Každá deklarace typu obsažená v deklaraci obecné třídy je implicitně deklarace obecného typu.</span><span class="sxs-lookup"><span data-stu-id="479b6-542">Every type declaration contained within a generic class declaration is implicitly a generic type declaration.</span></span> <span data-ttu-id="479b6-543">Při psaní odkazu na typ vnořený v rámci obecného typu musí být obsažený konstruovaný typ, včetně jeho argumentů typu, pojmenován.</span><span class="sxs-lookup"><span data-stu-id="479b6-543">When writing a reference to a type nested within a generic type, the containing constructed type, including its type arguments, must be named.</span></span> <span data-ttu-id="479b6-544">V rámci vnější třídy však lze vnořený typ použít bez kvalifikace; typ instance vnější třídy lze implicitně použít při vytváření vnořeného typu.</span><span class="sxs-lookup"><span data-stu-id="479b6-544">However, from within the outer class, the nested type can be used without qualification; the instance type of the outer class can be implicitly used when constructing the nested type.</span></span> <span data-ttu-id="479b6-545">Následující příklad ukazuje tři různé správné způsoby, jak odkazovat na konstruovaný typ vytvořený z `Inner`; první dvě jsou ekvivalentní:</span><span class="sxs-lookup"><span data-stu-id="479b6-545">The following example shows three different correct ways to refer to a constructed type created from `Inner`; the first two are equivalent:</span></span>
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

<span data-ttu-id="479b6-546">Přestože se jedná o špatný styl programování, parametr typu ve vnořeném typu může skrýt člen nebo parametr typu deklarovaný ve vnějším typu:</span><span class="sxs-lookup"><span data-stu-id="479b6-546">Although it is bad programming style, a type parameter in a nested type can hide a member or type parameter declared in the outer type:</span></span>
```csharp
class Outer<T>
{
    class Inner<T>        // Valid, hides Outer's T
    {
        public T t;       // Refers to Inner's T
    }
}
```

### <a name="reserved-member-names"></a><span data-ttu-id="479b6-547">Rezervované názvy členů</span><span class="sxs-lookup"><span data-stu-id="479b6-547">Reserved member names</span></span>

<span data-ttu-id="479b6-548">Aby bylo možné zajistit C# podkladovou implementaci za běhu, pro každou deklaraci zdrojového člena, který je vlastností, událostí nebo indexerem, musí implementace vyhradit dva signatury metod na základě druhu deklarace člena, jeho názvu a typu.</span><span class="sxs-lookup"><span data-stu-id="479b6-548">To facilitate the underlying C# run-time implementation, for each source member declaration that is a property, event, or indexer, the implementation must reserve two method signatures based on the kind of the member declaration, its name, and its type.</span></span> <span data-ttu-id="479b6-549">Jedná se o chybu v době kompilace pro program, který deklaruje člena, jehož signatura odpovídá jednomu z těchto vyhrazených signatur, i když základní implementace modulu runtime nepoužívá tyto rezervace.</span><span class="sxs-lookup"><span data-stu-id="479b6-549">It is a compile-time error for a program to declare a member whose signature matches one of these reserved signatures, even if the underlying run-time implementation does not make use of these reservations.</span></span>

<span data-ttu-id="479b6-550">Rezervované názvy nezavádí deklarace, takže se nepodílejí na vyhledávání členů.</span><span class="sxs-lookup"><span data-stu-id="479b6-550">The reserved names do not introduce declarations, thus they do not participate in member lookup.</span></span> <span data-ttu-id="479b6-551">Přidružené signatury rezervovaných metod deklarace se ale účastní dědičnosti ([dědičnosti](classes.md#inheritance)) a můžou být skryté pomocí `new` modifikátoru ([nový modifikátor](classes.md#the-new-modifier)).</span><span class="sxs-lookup"><span data-stu-id="479b6-551">However, a declaration's associated reserved method signatures do participate in inheritance ([Inheritance](classes.md#inheritance)), and can be hidden with the `new` modifier ([The new modifier](classes.md#the-new-modifier)).</span></span>

<span data-ttu-id="479b6-552">Rezervace těchto názvů slouží jako tři účely:</span><span class="sxs-lookup"><span data-stu-id="479b6-552">The reservation of these names serves three purposes:</span></span>

*  <span data-ttu-id="479b6-553">Pokud chcete, aby základní implementace používala běžný identifikátor jako název metody pro získání nebo nastavení přístupu k funkci C# Language.</span><span class="sxs-lookup"><span data-stu-id="479b6-553">To allow the underlying implementation to use an ordinary identifier as a method name for get or set access to the C# language feature.</span></span>
*  <span data-ttu-id="479b6-554">Aby mohly jiné jazyky spolupracovat s použitím běžného identifikátoru jako názvu metody pro získání nebo nastavení přístupu k funkci C# jazyka.</span><span class="sxs-lookup"><span data-stu-id="479b6-554">To allow other languages to interoperate using an ordinary identifier as a method name for get or set access to the C# language feature.</span></span>
*  <span data-ttu-id="479b6-555">Aby bylo zajištěno, že zdroj přijatý jedním vyhovujícím kompilátorem, je přijímán jiným, tím, že jsou specifické názvy rezervovaných členů konzistentní napříč všemi C# implementacemi.</span><span class="sxs-lookup"><span data-stu-id="479b6-555">To help ensure that the source accepted by one conforming compiler is accepted by another, by making the specifics of reserved member names consistent across all C# implementations.</span></span>

<span data-ttu-id="479b6-556">Deklarace destruktoru ([destruktorů](classes.md#destructors)) také způsobí, že signatura bude rezervována ([názvy členů vyhrazené pro destruktory](classes.md#member-names-reserved-for-destructors)).</span><span class="sxs-lookup"><span data-stu-id="479b6-556">The declaration of a destructor ([Destructors](classes.md#destructors)) also causes a signature to be reserved ([Member names reserved for destructors](classes.md#member-names-reserved-for-destructors)).</span></span>

#### <a name="member-names-reserved-for-properties"></a><span data-ttu-id="479b6-557">Názvy členů rezervované pro vlastnosti</span><span class="sxs-lookup"><span data-stu-id="479b6-557">Member names reserved for properties</span></span>

<span data-ttu-id="479b6-558">U vlastnosti `P` ([vlastností](classes.md#properties)) typu `T`jsou rezervovány následující signatury:</span><span class="sxs-lookup"><span data-stu-id="479b6-558">For a property `P` ([Properties](classes.md#properties)) of type `T`, the following signatures are reserved:</span></span>

```csharp
T get_P();
void set_P(T value);
```

<span data-ttu-id="479b6-559">Oba signatury jsou rezervované, a to i v případě, že je vlastnost jen pro čtení nebo jen pro zápis.</span><span class="sxs-lookup"><span data-stu-id="479b6-559">Both signatures are reserved, even if the property is read-only or write-only.</span></span>

<span data-ttu-id="479b6-560">V příkladu</span><span class="sxs-lookup"><span data-stu-id="479b6-560">In the example</span></span>
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
<span data-ttu-id="479b6-561">Třída `A` definuje vlastnost `P`, která je jen pro čtení, a zachovává tak `get_P` signatury pro metody a `set_P` .</span><span class="sxs-lookup"><span data-stu-id="479b6-561">a class `A` defines a read-only property `P`, thus reserving signatures for `get_P` and `set_P` methods.</span></span> <span data-ttu-id="479b6-562">Třída `B` je odvozena z `A` a skrývá oba tyto vyhrazené signatury.</span><span class="sxs-lookup"><span data-stu-id="479b6-562">A class `B` derives from `A` and hides both of these reserved signatures.</span></span> <span data-ttu-id="479b6-563">Příklad vytvoří výstup:</span><span class="sxs-lookup"><span data-stu-id="479b6-563">The example produces the output:</span></span>
```
123
123
456
```

#### <a name="member-names-reserved-for-events"></a><span data-ttu-id="479b6-564">Názvy členů rezervované pro události</span><span class="sxs-lookup"><span data-stu-id="479b6-564">Member names reserved for events</span></span>

<span data-ttu-id="479b6-565">Pro událost `E` ([události](classes.md#events)) typu `T`delegáta jsou vyhrazena následující signatury:</span><span class="sxs-lookup"><span data-stu-id="479b6-565">For an event `E` ([Events](classes.md#events)) of delegate type `T`, the following signatures are reserved:</span></span>
```csharp
void add_E(T handler);
void remove_E(T handler);
```

#### <a name="member-names-reserved-for-indexers"></a><span data-ttu-id="479b6-566">Názvy členů rezervované pro indexery</span><span class="sxs-lookup"><span data-stu-id="479b6-566">Member names reserved for indexers</span></span>

<span data-ttu-id="479b6-567">Pro indexer ([indexery](classes.md#indexers)) typu `T` se seznamem `L`parametrů jsou rezervované následující signatury:</span><span class="sxs-lookup"><span data-stu-id="479b6-567">For an indexer ([Indexers](classes.md#indexers)) of type `T` with parameter-list `L`, the following signatures are reserved:</span></span>
```csharp
T get_Item(L);
void set_Item(L, T value);
```

<span data-ttu-id="479b6-568">Oba signatury jsou rezervované, i když je indexer jen pro čtení nebo jen pro zápis.</span><span class="sxs-lookup"><span data-stu-id="479b6-568">Both signatures are reserved, even if the indexer is read-only or write-only.</span></span>

<span data-ttu-id="479b6-569">Název `Item` člena je navíc rezervovaný.</span><span class="sxs-lookup"><span data-stu-id="479b6-569">Furthermore the member name `Item` is reserved.</span></span>

#### <a name="member-names-reserved-for-destructors"></a><span data-ttu-id="479b6-570">Názvy členů rezervované pro destruktory</span><span class="sxs-lookup"><span data-stu-id="479b6-570">Member names reserved for destructors</span></span>

<span data-ttu-id="479b6-571">Pro třídu obsahující destruktor ([destruktory](classes.md#destructors)) je následující signatura vyhrazena:</span><span class="sxs-lookup"><span data-stu-id="479b6-571">For a class containing a destructor ([Destructors](classes.md#destructors)), the following signature is reserved:</span></span>
```csharp
void Finalize();
```

## <a name="constants"></a><span data-ttu-id="479b6-572">Konstanty</span><span class="sxs-lookup"><span data-stu-id="479b6-572">Constants</span></span>

<span data-ttu-id="479b6-573">***Konstanta*** je člen třídy, který představuje konstantní hodnotu: hodnotu, kterou lze vypočítat v době kompilace.</span><span class="sxs-lookup"><span data-stu-id="479b6-573">A ***constant*** is a class member that represents a constant value: a value that can be computed at compile-time.</span></span> <span data-ttu-id="479b6-574">*Constant_declaration* zavádí jednu nebo více konstant daného typu.</span><span class="sxs-lookup"><span data-stu-id="479b6-574">A *constant_declaration* introduces one or more constants of a given type.</span></span>

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

<span data-ttu-id="479b6-575">*Constant_declaration* může zahrnovat sadu *atributů* ( `new` [atributů](attributes.md)), modifikátoru ([nový modifikátor](classes.md#the-new-modifier)) a platnou kombinaci čtyř modifikátorů přístupu ([modifikátory přístupu](classes.md#access-modifiers)).</span><span class="sxs-lookup"><span data-stu-id="479b6-575">A *constant_declaration* may include a set of *attributes* ([Attributes](attributes.md)), a `new` modifier ([The new modifier](classes.md#the-new-modifier)), and a valid combination of the four access modifiers ([Access modifiers](classes.md#access-modifiers)).</span></span> <span data-ttu-id="479b6-576">Atributy a modifikátory se vztahují na všechny členy deklarované *constant_declaration*.</span><span class="sxs-lookup"><span data-stu-id="479b6-576">The attributes and modifiers apply to all of the members declared by the *constant_declaration*.</span></span> <span data-ttu-id="479b6-577">I když jsou konstanty považovány za statické členy, *constant_declaration* ani nepožaduje `static` ani neumožňuje modifikátor.</span><span class="sxs-lookup"><span data-stu-id="479b6-577">Even though constants are considered static members, a *constant_declaration* neither requires nor allows a `static` modifier.</span></span> <span data-ttu-id="479b6-578">Je-li stejný modifikátor v deklaraci konstanty uveden několikrát, jedná se o chybu.</span><span class="sxs-lookup"><span data-stu-id="479b6-578">It is an error for the same modifier to appear multiple times in a constant declaration.</span></span>

<span data-ttu-id="479b6-579">*Typ* *constant_declaration* určuje typ členů zavedených deklarací.</span><span class="sxs-lookup"><span data-stu-id="479b6-579">The *type* of a *constant_declaration* specifies the type of the members introduced by the declaration.</span></span> <span data-ttu-id="479b6-580">Po typu následuje seznam *constant_declarator*, z nichž každý zavádí nového člena.</span><span class="sxs-lookup"><span data-stu-id="479b6-580">The type is followed by a list of *constant_declarator*s, each of which introduces a new member.</span></span> <span data-ttu-id="479b6-581">*Constant_declarator* se skládá z *identifikátoru* , který má za člena název, následovaný tokenem "`=`" následovaným *constant_expression* ([konstantními výrazy](expressions.md#constant-expressions)), které přidělí hodnotu člena.</span><span class="sxs-lookup"><span data-stu-id="479b6-581">A *constant_declarator* consists of an *identifier* that names the member, followed by an "`=`" token, followed by a *constant_expression* ([Constant expressions](expressions.md#constant-expressions)) that gives the value of the member.</span></span>

<span data-ttu-id="479b6-582">*Typ* určený v deklaraci konstanty musí být `sbyte` `ushort` `uint` `long` `ulong`, `byte`, `short`,, `int`,,, ,`char`, ,`float` `double`, ,`decimal` *,, a enum_type*nebo *reference_type*. `bool` `string`</span><span class="sxs-lookup"><span data-stu-id="479b6-582">The *type* specified in a constant declaration must be `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, `decimal`, `bool`, `string`, an *enum_type*, or a *reference_type*.</span></span> <span data-ttu-id="479b6-583">Každý *constant_expression* musí vracet hodnotu cílového typu nebo typu, který lze převést na cílový typ pomocí implicitního převodu ([implicitní převody](conversions.md#implicit-conversions)).</span><span class="sxs-lookup"><span data-stu-id="479b6-583">Each *constant_expression* must yield a value of the target type or of a type that can be converted to the target type by an implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)).</span></span>

<span data-ttu-id="479b6-584">*Typ* konstanty musí být alespoň tak přístupný jako konstanta sama ([Omezení přístupnosti](basic-concepts.md#accessibility-constraints)).</span><span class="sxs-lookup"><span data-stu-id="479b6-584">The *type* of a constant must be at least as accessible as the constant itself ([Accessibility constraints](basic-concepts.md#accessibility-constraints)).</span></span>

<span data-ttu-id="479b6-585">Hodnota konstanty je získána ve výrazu pomocí *simple_name* ([jednoduché názvy](expressions.md#simple-names)) nebo *member_access* ([přístup ke členu](expressions.md#member-access)).</span><span class="sxs-lookup"><span data-stu-id="479b6-585">The value of a constant is obtained in an expression using a *simple_name* ([Simple names](expressions.md#simple-names)) or a *member_access* ([Member access](expressions.md#member-access)).</span></span>

<span data-ttu-id="479b6-586">Konstanta se může účastnit *constant_expression*.</span><span class="sxs-lookup"><span data-stu-id="479b6-586">A constant can itself participate in a *constant_expression*.</span></span> <span data-ttu-id="479b6-587">Proto může být konstanta použita v jakékoli konstrukci, která vyžaduje *constant_expression*.</span><span class="sxs-lookup"><span data-stu-id="479b6-587">Thus, a constant may be used in any construct that requires a *constant_expression*.</span></span> <span data-ttu-id="479b6-588">Příklady takových konstrukcí zahrnují `case` popisky, `goto case` příkazy, `enum` deklarace členů, atributy a další konstantní deklarace.</span><span class="sxs-lookup"><span data-stu-id="479b6-588">Examples of such constructs include `case` labels, `goto case` statements, `enum` member declarations, attributes, and other constant declarations.</span></span>

<span data-ttu-id="479b6-589">Jak je popsáno v [konstantních výrazech](expressions.md#constant-expressions), *constant_expression* je výraz, který lze plně vyhodnotit v době kompilace.</span><span class="sxs-lookup"><span data-stu-id="479b6-589">As described in [Constant expressions](expressions.md#constant-expressions), a *constant_expression* is an expression that can be fully evaluated at compile-time.</span></span> <span data-ttu-id="479b6-590">Vzhledem k tomu, že jediný způsob, jak vytvořit jinou hodnotu než null, která `string` je reference_type jiným než `new` , je použít operátor a `new` protože operátor není povolen v *constant_expression*, jedinou možnou hodnotou pro konstanty *reference_type*s jiné než `string` jsou `null`.</span><span class="sxs-lookup"><span data-stu-id="479b6-590">Since the only way to create a non-null value of a *reference_type* other than `string` is to apply the `new` operator, and since the `new` operator is not permitted in a *constant_expression*, the only possible value for constants of *reference_type*s other than `string` is `null`.</span></span>

<span data-ttu-id="479b6-591">Je-li požadován symbolický název pro konstantní hodnotu, ale pokud typ této hodnoty není v deklaraci konstanty povolen, nebo pokud hodnotu nelze vypočítat v době kompilace *constant_expression*, `readonly` pole ([pole jen pro čtení ](classes.md#readonly-fields)), lze použít místo toho.</span><span class="sxs-lookup"><span data-stu-id="479b6-591">When a symbolic name for a constant value is desired, but when the type of that value is not permitted in a constant declaration, or when the value cannot be computed at compile-time by a *constant_expression*, a `readonly` field ([Readonly fields](classes.md#readonly-fields)) may be used instead.</span></span>

<span data-ttu-id="479b6-592">Deklarace konstanty, která deklaruje více konstant je ekvivalentní více deklaracím s jedním konstantou se stejnými atributy, modifikátory a typem.</span><span class="sxs-lookup"><span data-stu-id="479b6-592">A constant declaration that declares multiple constants is equivalent to multiple declarations of single constants with the same attributes, modifiers, and type.</span></span> <span data-ttu-id="479b6-593">Příklad</span><span class="sxs-lookup"><span data-stu-id="479b6-593">For example</span></span>
```csharp
class A
{
    public const double X = 1.0, Y = 2.0, Z = 3.0;
}
```
<span data-ttu-id="479b6-594">je ekvivalentem</span><span class="sxs-lookup"><span data-stu-id="479b6-594">is equivalent to</span></span>
```csharp
class A
{
    public const double X = 1.0;
    public const double Y = 2.0;
    public const double Z = 3.0;
}
```

<span data-ttu-id="479b6-595">Konstanty jsou povoleny v závislosti na jiných konstantách v rámci stejného programu, pokud závislosti nejsou cyklického charakteru.</span><span class="sxs-lookup"><span data-stu-id="479b6-595">Constants are permitted to depend on other constants within the same program as long as the dependencies are not of a circular nature.</span></span> <span data-ttu-id="479b6-596">Kompilátor automaticky uspořádá pro vyhodnocení konstantních deklarací v příslušném pořadí.</span><span class="sxs-lookup"><span data-stu-id="479b6-596">The compiler automatically arranges to evaluate the constant declarations in the appropriate order.</span></span> <span data-ttu-id="479b6-597">V příkladu</span><span class="sxs-lookup"><span data-stu-id="479b6-597">In the example</span></span>
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
<span data-ttu-id="479b6-598">Kompilátor nejprve vyhodnotí `A.Y`a poté `B.Z`vyhodnotí a nakonec `A.X`vyhodnotí, a vrátí `10`hodnoty `11`, a `12`.</span><span class="sxs-lookup"><span data-stu-id="479b6-598">the compiler first evaluates `A.Y`, then evaluates `B.Z`, and finally evaluates `A.X`, producing the values `10`, `11`, and `12`.</span></span> <span data-ttu-id="479b6-599">Konstantní deklarace mohou záviset na konstantách z jiných programů, ale tyto závislosti jsou možné pouze v jednom směru.</span><span class="sxs-lookup"><span data-stu-id="479b6-599">Constant declarations may depend on constants from other programs, but such dependencies are only possible in one direction.</span></span> <span data-ttu-id="479b6-600">Odkazy na výše uvedený příklad, pokud `A` a `B` byly deklarovány v samostatných programech, by mohly být závislé `A.X` na `B.Z`, ale `B.Z` mohou být pak nezávislá `A.Y`na.</span><span class="sxs-lookup"><span data-stu-id="479b6-600">Referring to the example above, if `A` and `B` were declared in separate programs, it would be possible for `A.X` to depend on `B.Z`, but `B.Z` could then not simultaneously depend on `A.Y`.</span></span>

## <a name="fields"></a><span data-ttu-id="479b6-601">Pole</span><span class="sxs-lookup"><span data-stu-id="479b6-601">Fields</span></span>

<span data-ttu-id="479b6-602">***Pole*** je člen, který představuje proměnnou přidruženou k objektu nebo třídě.</span><span class="sxs-lookup"><span data-stu-id="479b6-602">A ***field*** is a member that represents a variable associated with an object or class.</span></span> <span data-ttu-id="479b6-603">*Field_declaration* zavádí jedno nebo více polí daného typu.</span><span class="sxs-lookup"><span data-stu-id="479b6-603">A *field_declaration* introduces one or more fields of a given type.</span></span>

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

<span data-ttu-id="479b6-604">*Field_declaration* může zahrnovat sadu *atributů* `new` ([atributů](attributes.md)), modifikátor ([nový modifikátor](classes.md#the-new-modifier)), platnou kombinaci `static` modifikátorů čtyř přístupu ([modifikátory přístupu](classes.md#access-modifiers)) a Modifikátor ([statická a pole instance](classes.md#static-and-instance-fields)).</span><span class="sxs-lookup"><span data-stu-id="479b6-604">A *field_declaration* may include a set of *attributes* ([Attributes](attributes.md)), a `new` modifier ([The new modifier](classes.md#the-new-modifier)), a valid combination of the four access modifiers ([Access modifiers](classes.md#access-modifiers)), and a `static` modifier ([Static and instance fields](classes.md#static-and-instance-fields)).</span></span> <span data-ttu-id="479b6-605">Kromě toho může *field_declaration* `readonly` obsahovat modifikátor ([pole jen pro čtení](classes.md#readonly-fields)) nebo modifikátor ( `volatile` pole s modifikátorem[volatile](classes.md#volatile-fields)), ale ne obojí.</span><span class="sxs-lookup"><span data-stu-id="479b6-605">In addition, a *field_declaration* may include a `readonly` modifier ([Readonly fields](classes.md#readonly-fields)) or a `volatile` modifier ([Volatile fields](classes.md#volatile-fields)) but not both.</span></span> <span data-ttu-id="479b6-606">Atributy a modifikátory se vztahují na všechny členy deklarované *field_declaration*.</span><span class="sxs-lookup"><span data-stu-id="479b6-606">The attributes and modifiers apply to all of the members declared by the *field_declaration*.</span></span> <span data-ttu-id="479b6-607">Je-li stejný modifikátor v deklaraci pole uveden několikrát, jedná se o chybu.</span><span class="sxs-lookup"><span data-stu-id="479b6-607">It is an error for the same modifier to appear multiple times in a field declaration.</span></span>

<span data-ttu-id="479b6-608">*Typ* *field_declaration* určuje typ členů zavedených deklarací.</span><span class="sxs-lookup"><span data-stu-id="479b6-608">The *type* of a *field_declaration* specifies the type of the members introduced by the declaration.</span></span> <span data-ttu-id="479b6-609">Po typu následuje seznam *variable_declarator*, z nichž každý zavádí nového člena.</span><span class="sxs-lookup"><span data-stu-id="479b6-609">The type is followed by a list of *variable_declarator*s, each of which introduces a new member.</span></span> <span data-ttu-id="479b6-610">*Variable_declarator* se skládá z *identifikátoru* , který je členem, volitelně následovaný`=`tokenem a *variable_initializer* ([Inicializátory proměnných](classes.md#variable-initializers)), který poskytuje počáteční hodnotu tohoto člena.</span><span class="sxs-lookup"><span data-stu-id="479b6-610">A *variable_declarator* consists of an *identifier* that names that member, optionally followed by an "`=`" token and a *variable_initializer* ([Variable initializers](classes.md#variable-initializers)) that gives the initial value of that member.</span></span>

<span data-ttu-id="479b6-611">*Typ* pole musí být alespoň přístupný jako pole samotné ([Omezení přístupnosti](basic-concepts.md#accessibility-constraints)).</span><span class="sxs-lookup"><span data-stu-id="479b6-611">The *type* of a field must be at least as accessible as the field itself ([Accessibility constraints](basic-concepts.md#accessibility-constraints)).</span></span>

<span data-ttu-id="479b6-612">Hodnota pole se získá ve výrazu s použitím *simple_name* ([jednoduché názvy](expressions.md#simple-names)) nebo *member_access* ([přístup ke členu](expressions.md#member-access)).</span><span class="sxs-lookup"><span data-stu-id="479b6-612">The value of a field is obtained in an expression using a *simple_name* ([Simple names](expressions.md#simple-names)) or a *member_access* ([Member access](expressions.md#member-access)).</span></span> <span data-ttu-id="479b6-613">Hodnota pole, které není jen pro čtení, je upravena pomocí *přiřazení* ([operátor přiřazení](expressions.md#assignment-operators)).</span><span class="sxs-lookup"><span data-stu-id="479b6-613">The value of a non-readonly field is modified using an *assignment* ([Assignment operators](expressions.md#assignment-operators)).</span></span> <span data-ttu-id="479b6-614">Hodnota pole, které není jen pro čtení, může být získána i upravena pomocí operátorů přírůstku[a snížení](expressions.md#postfix-increment-and-decrement-operators)předpony a operátorů přírůstku a snížení předpony ([zvýšení a snížení předpony). Operators](expressions.md#prefix-increment-and-decrement-operators)).</span><span class="sxs-lookup"><span data-stu-id="479b6-614">The value of a non-readonly field can be both obtained and modified using postfix increment and decrement operators ([Postfix increment and decrement operators](expressions.md#postfix-increment-and-decrement-operators)) and prefix increment and decrement operators ([Prefix increment and decrement operators](expressions.md#prefix-increment-and-decrement-operators)).</span></span>

<span data-ttu-id="479b6-615">Deklarace pole, která deklaruje více polí, je ekvivalentní více deklaracím jednoho pole se stejnými atributy, modifikátory a typem.</span><span class="sxs-lookup"><span data-stu-id="479b6-615">A field declaration that declares multiple fields is equivalent to multiple declarations of single fields with the same attributes, modifiers, and type.</span></span> <span data-ttu-id="479b6-616">Příklad</span><span class="sxs-lookup"><span data-stu-id="479b6-616">For example</span></span>
```csharp
class A
{
    public static int X = 1, Y, Z = 100;
}
```
<span data-ttu-id="479b6-617">je ekvivalentem</span><span class="sxs-lookup"><span data-stu-id="479b6-617">is equivalent to</span></span>
```csharp
class A
{
    public static int X = 1;
    public static int Y;
    public static int Z = 100;
}
```

### <a name="static-and-instance-fields"></a><span data-ttu-id="479b6-618">Statická pole a pole instance</span><span class="sxs-lookup"><span data-stu-id="479b6-618">Static and instance fields</span></span>

<span data-ttu-id="479b6-619">Když deklarace pole obsahuje `static` modifikátor, pole zavedená deklarací jsou ***statická pole***.</span><span class="sxs-lookup"><span data-stu-id="479b6-619">When a field declaration includes a `static` modifier, the fields introduced by the declaration are ***static fields***.</span></span> <span data-ttu-id="479b6-620">Pokud není `static` k dispozici žádný modifikátor, pole zavedená deklarací jsou ***pole instance***.</span><span class="sxs-lookup"><span data-stu-id="479b6-620">When no `static` modifier is present, the fields introduced by the declaration are ***instance fields***.</span></span> <span data-ttu-id="479b6-621">Pole statických polí a instancí jsou dva z několika druhů proměnných ([proměnných](variables.md)) podporovaných C#a v době, kdy jsou označovány jako ***statické proměnné*** a ***proměnné instancí***v uvedeném pořadí.</span><span class="sxs-lookup"><span data-stu-id="479b6-621">Static fields and instance fields are two of the several kinds of variables ([Variables](variables.md)) supported by C#, and at times they are referred to as ***static variables*** and ***instance variables***, respectively.</span></span>

<span data-ttu-id="479b6-622">Statické pole není součástí určité instance. místo toho je sdíleno mezi všemi instancemi uzavřeného typu ([otevřené a uzavřené typy](types.md#open-and-closed-types)).</span><span class="sxs-lookup"><span data-stu-id="479b6-622">A static field is not part of a specific instance; instead, it is shared amongst all instances of a closed type ([Open and closed types](types.md#open-and-closed-types)).</span></span> <span data-ttu-id="479b6-623">Bez ohledu na to, kolik instancí uzavřeného typu třídy je vytvořeno, existuje pouze jedna kopie statického pole pro přidruženou doménu aplikace.</span><span class="sxs-lookup"><span data-stu-id="479b6-623">No matter how many instances of a closed class type are created, there is only ever one copy of a static field for the associated application domain.</span></span>

<span data-ttu-id="479b6-624">Příklad:</span><span class="sxs-lookup"><span data-stu-id="479b6-624">For example:</span></span>
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

<span data-ttu-id="479b6-625">Pole instance patří do instance.</span><span class="sxs-lookup"><span data-stu-id="479b6-625">An instance field belongs to an instance.</span></span> <span data-ttu-id="479b6-626">Konkrétně každá instance třídy obsahuje samostatnou sadu všech polí instance této třídy.</span><span class="sxs-lookup"><span data-stu-id="479b6-626">Specifically, every instance of a class contains a separate set of all the instance fields of that class.</span></span>

<span data-ttu-id="479b6-627">Pokud je na pole odkazováno v *member_access* ([členský přístup](expressions.md#member-access)) formuláře `E.M`, pokud `E` `M` je statickým polem, musí to znamenat typ obsahující `M`a pokud `M` je pole instance, musí být E. označuje instanci typu obsahujícího `M`.</span><span class="sxs-lookup"><span data-stu-id="479b6-627">When a field is referenced in a *member_access* ([Member access](expressions.md#member-access)) of the form `E.M`, if `M` is a static field, `E` must denote a type containing `M`, and if `M` is an instance field, E must denote an instance of a type containing `M`.</span></span>

<span data-ttu-id="479b6-628">Rozdíly mezi členy static a instance jsou podrobněji popsány ve [statických a instancích členů](classes.md#static-and-instance-members).</span><span class="sxs-lookup"><span data-stu-id="479b6-628">The differences between static and instance members are discussed further in [Static and instance members](classes.md#static-and-instance-members).</span></span>

### <a name="readonly-fields"></a><span data-ttu-id="479b6-629">Pole jen pro čtení</span><span class="sxs-lookup"><span data-stu-id="479b6-629">Readonly fields</span></span>

<span data-ttu-id="479b6-630">Pokud *field_declaration* obsahuje `readonly` modifikátor, pole zavedená deklarací jsou ***pole jen pro čtení***.</span><span class="sxs-lookup"><span data-stu-id="479b6-630">When a *field_declaration* includes a `readonly` modifier, the fields introduced by the declaration are ***readonly fields***.</span></span> <span data-ttu-id="479b6-631">Přímá přiřazení k polím jen pro čtení se můžou vyskytovat jenom jako součást této deklarace nebo konstruktoru instance nebo statického konstruktoru ve stejné třídě.</span><span class="sxs-lookup"><span data-stu-id="479b6-631">Direct assignments to readonly fields can only occur as part of that declaration or in an instance constructor or static constructor in the same class.</span></span> <span data-ttu-id="479b6-632">(Pole jen pro čtení lze v těchto kontextech přiřadit vícekrát.) Konkrétně Přímá přiřazení k `readonly` poli jsou povolena pouze v následujících kontextech:</span><span class="sxs-lookup"><span data-stu-id="479b6-632">(A readonly field can be assigned to multiple times in these contexts.) Specifically, direct assignments to a `readonly` field are permitted only in the following contexts:</span></span>

*  <span data-ttu-id="479b6-633">V *variable_declarator* , který zavádí pole (včetně *variable_initializer* v deklaraci).</span><span class="sxs-lookup"><span data-stu-id="479b6-633">In the *variable_declarator* that introduces the field (by including a *variable_initializer* in the declaration).</span></span>
*  <span data-ttu-id="479b6-634">Pro pole instance v konstruktorech instancí třídy, která obsahuje deklaraci pole; pro statické pole ve statickém konstruktoru třídy, která obsahuje deklaraci pole.</span><span class="sxs-lookup"><span data-stu-id="479b6-634">For an instance field, in the instance constructors of the class that contains the field declaration; for a static field, in the static constructor of the class that contains the field declaration.</span></span> <span data-ttu-id="479b6-635">Jsou to také pouze kontexty, ve kterých je platný pro předání `readonly` pole `out` jako parametru nebo `ref` .</span><span class="sxs-lookup"><span data-stu-id="479b6-635">These are also the only contexts in which it is valid to pass a `readonly` field as an `out` or `ref` parameter.</span></span>

<span data-ttu-id="479b6-636">Pokus o přiřazení k `readonly` poli nebo jeho předání `out` jako parametr nebo `ref` v jakémkoli jiném kontextu je chyba při kompilaci.</span><span class="sxs-lookup"><span data-stu-id="479b6-636">Attempting to assign to a `readonly` field or pass it as an `out` or `ref` parameter in any other context is a compile-time error.</span></span>

#### <a name="using-static-readonly-fields-for-constants"></a><span data-ttu-id="479b6-637">Použití statických polí jen pro čtení pro konstanty</span><span class="sxs-lookup"><span data-stu-id="479b6-637">Using static readonly fields for constants</span></span>

<span data-ttu-id="479b6-638">Pole je užitečné, pokud je požadován symbolický název hodnoty konstanty, ale pokud typ hodnoty není `const` v deklaraci povolený, nebo když hodnotu nelze vypočítat v době kompilace. `static readonly`</span><span class="sxs-lookup"><span data-stu-id="479b6-638">A `static readonly` field is useful when a symbolic name for a constant value is desired, but when the type of the value is not permitted in a `const` declaration, or when the value cannot be computed at compile-time.</span></span> <span data-ttu-id="479b6-639">V příkladu</span><span class="sxs-lookup"><span data-stu-id="479b6-639">In the example</span></span>
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
<span data-ttu-id="479b6-640">`Black`členy, `White`, ,`Red`anelzedeklarovatjako členy,protožejejichhodnotynelzevypočítatvdoběkompilace.`const` `Green` `Blue`</span><span class="sxs-lookup"><span data-stu-id="479b6-640">the `Black`, `White`, `Red`, `Green`, and `Blue` members cannot be declared as `const` members because their values cannot be computed at compile-time.</span></span> <span data-ttu-id="479b6-641">`static readonly` Namísto toho je ale deklarujete stejně jako stejný efekt.</span><span class="sxs-lookup"><span data-stu-id="479b6-641">However, declaring them `static readonly` instead has much the same effect.</span></span>

#### <a name="versioning-of-constants-and-static-readonly-fields"></a><span data-ttu-id="479b6-642">Správa verzí konstant a statických polí jen pro čtení</span><span class="sxs-lookup"><span data-stu-id="479b6-642">Versioning of constants and static readonly fields</span></span>

<span data-ttu-id="479b6-643">Konstanty a pole ReadOnly mají různé sémantiky binárních verzí.</span><span class="sxs-lookup"><span data-stu-id="479b6-643">Constants and readonly fields have different binary versioning semantics.</span></span> <span data-ttu-id="479b6-644">Pokud výraz odkazuje na konstantu, je hodnota konstanty získána v době kompilace, ale pokud výraz odkazuje na pole jen pro čtení, hodnota pole se nezíská až do doby běhu.</span><span class="sxs-lookup"><span data-stu-id="479b6-644">When an expression references a constant, the value of the constant is obtained at compile-time, but when an expression references a readonly field, the value of the field is not obtained until run-time.</span></span> <span data-ttu-id="479b6-645">Vezměte v úvahu aplikaci, která se skládá ze dvou samostatných programů:</span><span class="sxs-lookup"><span data-stu-id="479b6-645">Consider an application that consists of two separate programs:</span></span>
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

<span data-ttu-id="479b6-646">Obory `Program2` názvů aoznačujídvaprogramy,kteréjsoukompiloványsamostatně.`Program1`</span><span class="sxs-lookup"><span data-stu-id="479b6-646">The `Program1` and `Program2` namespaces denote two programs that are compiled separately.</span></span> <span data-ttu-id="479b6-647">Vzhledem `Program1.Utils.X` k tomu, že je deklarován jako statické pole jen pro čtení, `Console.WriteLine` není výstup hodnoty příkazu znám v době kompilace, ale je spíše získán za běhu.</span><span class="sxs-lookup"><span data-stu-id="479b6-647">Because `Program1.Utils.X` is declared as a static readonly field, the value output by the `Console.WriteLine` statement is not known at compile-time, but rather is obtained at run-time.</span></span> <span data-ttu-id="479b6-648">Proto `X` Pokud je hodnota změněna a `Program1` znovu zkompilována, `Console.WriteLine` příkaz bude výstupem nové hodnoty, i když `Program2` není znovu zkompilován.</span><span class="sxs-lookup"><span data-stu-id="479b6-648">Thus, if the value of `X` is changed and `Program1` is recompiled, the `Console.WriteLine` statement will output the new value even if `Program2` isn't recompiled.</span></span> <span data-ttu-id="479b6-649">Nicméně existovala `X` konstanta, `X` hodnota by byla získána v době `Program2` kompilace a by zůstala neovlivněná změnami v `Program1` , dokud `Program2` není znovu zkompilována.</span><span class="sxs-lookup"><span data-stu-id="479b6-649">However, had `X` been a constant, the value of `X` would have been obtained at the time `Program2` was compiled, and would remain unaffected by changes in `Program1` until `Program2` is recompiled.</span></span>

### <a name="volatile-fields"></a><span data-ttu-id="479b6-650">Pole s modifikátorem volatile</span><span class="sxs-lookup"><span data-stu-id="479b6-650">Volatile fields</span></span>

<span data-ttu-id="479b6-651">Pokud *field_declaration* obsahuje `volatile` modifikátor, pole zavedená touto deklarací jsou pole typu ***volatile***.</span><span class="sxs-lookup"><span data-stu-id="479b6-651">When a *field_declaration* includes a `volatile` modifier, the fields introduced by that declaration are ***volatile fields***.</span></span>

<span data-ttu-id="479b6-652">Pro nestálá pole mohou optimalizační techniky, které mění pořadí instrukcí, vést k neočekávaným a nepředvídatelným výsledkům v aplikacích s více vlákny, které přistupují k polím bez synchronizace,[jako je například lock_statement ( Lock – příkaz](statements.md#the-lock-statement)).</span><span class="sxs-lookup"><span data-stu-id="479b6-652">For non-volatile fields, optimization techniques that reorder instructions can lead to unexpected and unpredictable results in multi-threaded programs that access fields without synchronization such as that provided by the *lock_statement* ([The lock statement](statements.md#the-lock-statement)).</span></span> <span data-ttu-id="479b6-653">Tyto optimalizace mohou být provedeny kompilátorem, systémem za běhu nebo podle hardwaru.</span><span class="sxs-lookup"><span data-stu-id="479b6-653">These optimizations can be performed by the compiler, by the run-time system, or by hardware.</span></span> <span data-ttu-id="479b6-654">U polí typu volatile jsou tyto optimalizace pro změnu pořadí omezené:</span><span class="sxs-lookup"><span data-stu-id="479b6-654">For volatile fields, such reordering optimizations are restricted:</span></span>

*  <span data-ttu-id="479b6-655">Čtení pole s modifikátorem volatile se označuje jako ***nestálé čtení***.</span><span class="sxs-lookup"><span data-stu-id="479b6-655">A read of a volatile field is called a ***volatile read***.</span></span> <span data-ttu-id="479b6-656">Nestálá čtení má "získání sémantiky"; To znamená, že před jakýmikoli odkazy na paměť, ke které dojde v sekvenci instrukcí, je zaručena Tato situace.</span><span class="sxs-lookup"><span data-stu-id="479b6-656">A volatile read has "acquire semantics"; that is, it is guaranteed to occur prior to any references to memory that occur after it in the instruction sequence.</span></span>
*  <span data-ttu-id="479b6-657">Zápis pole s modifikátorem volatile se nazývá ***zápis typu volatile***.</span><span class="sxs-lookup"><span data-stu-id="479b6-657">A write of a volatile field is called a ***volatile write***.</span></span> <span data-ttu-id="479b6-658">Volatile zápis má "sémantiku vydání"; To znamená, že je zaručeno, že nastane po odkazech na paměť před instrukcí zápisu v sekvenci instrukcí.</span><span class="sxs-lookup"><span data-stu-id="479b6-658">A volatile write has "release semantics"; that is, it is guaranteed to happen after any memory references prior to the write instruction in the instruction sequence.</span></span>

<span data-ttu-id="479b6-659">Tato omezení zajistí, že všechna vlákna budou sledovat nestálá zápisy provedené jakýmkoli jiným vláknem v pořadí, ve kterém byly provedeny.</span><span class="sxs-lookup"><span data-stu-id="479b6-659">These restrictions ensure that all threads will observe volatile writes performed by any other thread in the order in which they were performed.</span></span> <span data-ttu-id="479b6-660">Vyhovující implementace není nutná k poskytnutí jediného celkového řazení pro nestálá zápisy, který je vidět ze všech vláken provádění.</span><span class="sxs-lookup"><span data-stu-id="479b6-660">A conforming implementation is not required to provide a single total ordering of volatile writes as seen from all threads of execution.</span></span> <span data-ttu-id="479b6-661">Typ pole s modifikátorem volatile musí být jeden z následujících:</span><span class="sxs-lookup"><span data-stu-id="479b6-661">The type of a volatile field must be one of the following:</span></span>

*  <span data-ttu-id="479b6-662">*Reference_type*.</span><span class="sxs-lookup"><span data-stu-id="479b6-662">A *reference_type*.</span></span>
*  <span data-ttu-id="479b6-663">`byte`Typ ,`char`,, ,`int`, ,,`bool`,,, nebo .`System.UIntPtr` `float` `ushort` `short` `sbyte` `uint` `System.IntPtr`</span><span class="sxs-lookup"><span data-stu-id="479b6-663">The type `byte`, `sbyte`, `short`, `ushort`, `int`, `uint`, `char`, `float`, `bool`, `System.IntPtr`, or `System.UIntPtr`.</span></span>
*  <span data-ttu-id="479b6-664">*Enum_type* `byte`, který má základní typ výčtu, `sbyte`, `short`, `ushort`, `int`nebo `uint`.</span><span class="sxs-lookup"><span data-stu-id="479b6-664">An *enum_type* having an enum base type of `byte`, `sbyte`, `short`, `ushort`, `int`, or `uint`.</span></span>

<span data-ttu-id="479b6-665">Příklad</span><span class="sxs-lookup"><span data-stu-id="479b6-665">The example</span></span>
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
<span data-ttu-id="479b6-666">Vytvoří výstup:</span><span class="sxs-lookup"><span data-stu-id="479b6-666">produces the output:</span></span>
```
result = 143
```

<span data-ttu-id="479b6-667">V tomto příkladu metoda `Main` spustí nové vlákno, které spouští metodu. `Thread2`</span><span class="sxs-lookup"><span data-stu-id="479b6-667">In this example, the method `Main` starts a new thread that runs the method `Thread2`.</span></span> <span data-ttu-id="479b6-668">Tato metoda ukládá hodnotu do nestálého pole s názvem `result`a pak je uloženo `true` v poli `finished`volatile.</span><span class="sxs-lookup"><span data-stu-id="479b6-668">This method stores a value into a non-volatile field called `result`, then stores `true` in the volatile field `finished`.</span></span> <span data-ttu-id="479b6-669">Hlavní vlákno počká, až bude pole `finished` nastaveno na `true`, a pak přečte pole `result`.</span><span class="sxs-lookup"><span data-stu-id="479b6-669">The main thread waits for the field `finished` to be set to `true`, then reads the field `result`.</span></span> <span data-ttu-id="479b6-670">Vzhledem `finished` k tomu, `volatile`že bylo deklarováno, musí hlavní vlákno `143` načíst hodnotu z `result`pole.</span><span class="sxs-lookup"><span data-stu-id="479b6-670">Since `finished` has been declared `volatile`, the main thread must read the value `143` from the field `result`.</span></span> <span data-ttu-id="479b6-671">Pokud nebylo `finished` deklarováno `volatile`pole, bylo by `result` povoleno, aby bylo úložiště viditelné pro hlavní vlákno po uložení do `finished`, a proto hlavní vlákno přečte hodnotu `0` z pole `result`.</span><span class="sxs-lookup"><span data-stu-id="479b6-671">If the field `finished` had not been declared `volatile`, then it would be permissible for the store to `result` to be visible to the main thread after the store to `finished`, and hence for the main thread to read the value `0` from the field `result`.</span></span> <span data-ttu-id="479b6-672">`finished` Deklarace`volatile` jako pole zabrání v takové nekonzistenci.</span><span class="sxs-lookup"><span data-stu-id="479b6-672">Declaring `finished` as a `volatile` field prevents any such inconsistency.</span></span>

### <a name="field-initialization"></a><span data-ttu-id="479b6-673">Inicializace pole</span><span class="sxs-lookup"><span data-stu-id="479b6-673">Field initialization</span></span>

<span data-ttu-id="479b6-674">Počáteční hodnota pole, zda se jedná o statické pole nebo pole instance, je výchozí hodnota ([výchozí hodnoty](variables.md#default-values)) typu pole.</span><span class="sxs-lookup"><span data-stu-id="479b6-674">The initial value of a field, whether it be a static field or an instance field, is the default value ([Default values](variables.md#default-values)) of the field's type.</span></span> <span data-ttu-id="479b6-675">Není možné sledovat hodnotu pole před touto výchozí inicializací a pole není tedy nikdy "uninicializovaný".</span><span class="sxs-lookup"><span data-stu-id="479b6-675">It is not possible to observe the value of a field before this default initialization has occurred, and a field is thus never "uninitialized".</span></span> <span data-ttu-id="479b6-676">Příklad</span><span class="sxs-lookup"><span data-stu-id="479b6-676">The example</span></span>
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
<span data-ttu-id="479b6-677">Vytvoří výstup</span><span class="sxs-lookup"><span data-stu-id="479b6-677">produces the output</span></span>
```
b = False, i = 0
```
<span data-ttu-id="479b6-678">vzhledem `b` k `i` tomu, že jsou automaticky inicializovány výchozí hodnoty.</span><span class="sxs-lookup"><span data-stu-id="479b6-678">because `b` and `i` are both automatically initialized to default values.</span></span>

### <a name="variable-initializers"></a><span data-ttu-id="479b6-679">Inicializátory proměnných</span><span class="sxs-lookup"><span data-stu-id="479b6-679">Variable initializers</span></span>

<span data-ttu-id="479b6-680">Deklarace polí můžou zahrnovat *variable_initializer*s.</span><span class="sxs-lookup"><span data-stu-id="479b6-680">Field declarations may include *variable_initializer*s.</span></span> <span data-ttu-id="479b6-681">Pro statická pole, Inicializátory proměnných odpovídají příkazům přiřazení provedeným během inicializace třídy.</span><span class="sxs-lookup"><span data-stu-id="479b6-681">For static fields, variable initializers correspond to assignment statements that are executed during class initialization.</span></span> <span data-ttu-id="479b6-682">Pro pole instancí odpovídají Inicializátory proměnných příkazy přiřazení, které jsou spuštěny při vytvoření instance třídy.</span><span class="sxs-lookup"><span data-stu-id="479b6-682">For instance fields, variable initializers correspond to assignment statements that are executed when an instance of the class is created.</span></span>

<span data-ttu-id="479b6-683">Příklad</span><span class="sxs-lookup"><span data-stu-id="479b6-683">The example</span></span>
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
<span data-ttu-id="479b6-684">Vytvoří výstup</span><span class="sxs-lookup"><span data-stu-id="479b6-684">produces the output</span></span>
```
x = 1.4142135623731, i = 100, s = Hello
```
<span data-ttu-id="479b6-685">vzhledem k `x` tomu, že k přiřazení dojde v případě, že se spustí `i` Inicializátory statických polí a dojde k přiřazení a `s` dojde k provedení inicializátorů pole instance.</span><span class="sxs-lookup"><span data-stu-id="479b6-685">because an assignment to `x` occurs when static field initializers execute and assignments to `i` and `s` occur when the instance field initializers execute.</span></span>

<span data-ttu-id="479b6-686">Výchozí inicializace hodnoty popsané v části [inicializace pole](classes.md#field-initialization) probíhá pro všechna pole, včetně polí, která mají Inicializátory proměnných.</span><span class="sxs-lookup"><span data-stu-id="479b6-686">The default value initialization described in [Field initialization](classes.md#field-initialization) occurs for all fields, including fields that have variable initializers.</span></span> <span data-ttu-id="479b6-687">Proto při inicializaci třídy jsou všechna statická pole v této třídě nejprve inicializována na jejich výchozí hodnoty a poté jsou provedeny Inicializátory statického pole v textovém pořadí.</span><span class="sxs-lookup"><span data-stu-id="479b6-687">Thus, when a class is initialized, all static fields in that class are first initialized to their default values, and then the static field initializers are executed in textual order.</span></span> <span data-ttu-id="479b6-688">Podobně platí, že když je vytvořena instance třídy, všechna pole instance v této instanci jsou nejprve inicializována na jejich výchozí hodnoty a poté jsou v textovém pořadí provedeny Inicializátory pole instance.</span><span class="sxs-lookup"><span data-stu-id="479b6-688">Likewise, when an instance of a class is created, all instance fields in that instance are first initialized to their default values, and then the instance field initializers are executed in textual order.</span></span>

<span data-ttu-id="479b6-689">Je možné, že statická pole s Inicializátory proměnných budou pozorována ve svém výchozím stavu.</span><span class="sxs-lookup"><span data-stu-id="479b6-689">It is possible for static fields with variable initializers to be observed in their default value state.</span></span> <span data-ttu-id="479b6-690">Nicméně to se důrazně nedoporučuje jako u stylu.</span><span class="sxs-lookup"><span data-stu-id="479b6-690">However, this is strongly discouraged as a matter of style.</span></span> <span data-ttu-id="479b6-691">Příklad</span><span class="sxs-lookup"><span data-stu-id="479b6-691">The example</span></span>
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
<span data-ttu-id="479b6-692">vykazuje toto chování.</span><span class="sxs-lookup"><span data-stu-id="479b6-692">exhibits this behavior.</span></span> <span data-ttu-id="479b6-693">Bez ohledu na cyklické definice a a b je program platný.</span><span class="sxs-lookup"><span data-stu-id="479b6-693">Despite the circular definitions of a and b, the program is valid.</span></span> <span data-ttu-id="479b6-694">Výsledkem bude výstup</span><span class="sxs-lookup"><span data-stu-id="479b6-694">It results in the output</span></span>
```
a = 1, b = 2
```
<span data-ttu-id="479b6-695">vzhledem k tomu, `a` že `b` statická pole a `0` jsou inicializována na ( `int`výchozí hodnota pro) před spuštěním jejich inicializátorů.</span><span class="sxs-lookup"><span data-stu-id="479b6-695">because the static fields `a` and `b` are initialized to `0` (the default value for `int`) before their initializers are executed.</span></span> <span data-ttu-id="479b6-696">Když je inicializátor pro `a` spuštění, `b` hodnota je nula, a proto `a` je inicializována na `1`.</span><span class="sxs-lookup"><span data-stu-id="479b6-696">When the initializer for `a` runs, the value of `b` is zero, and so `a` is initialized to `1`.</span></span> <span data-ttu-id="479b6-697">V případě inicializátoru `b` pro spuštění je `a` hodnota již `1`, a proto `b` je inicializována na `2`.</span><span class="sxs-lookup"><span data-stu-id="479b6-697">When the initializer for `b` runs, the value of `a` is already `1`, and so `b` is initialized to `2`.</span></span>

#### <a name="static-field-initialization"></a><span data-ttu-id="479b6-698">Inicializace statických polí</span><span class="sxs-lookup"><span data-stu-id="479b6-698">Static field initialization</span></span>

<span data-ttu-id="479b6-699">Inicializátory proměnných statického pole třídy odpovídají sekvenci přiřazení, která je spuštěna v textovém pořadí, ve kterém se nacházejí v deklaraci třídy.</span><span class="sxs-lookup"><span data-stu-id="479b6-699">The static field variable initializers of a class correspond to a sequence of assignments that are executed in the textual order in which they appear in the class declaration.</span></span> <span data-ttu-id="479b6-700">Pokud ve třídě existuje statický konstruktor ([statické konstruktory](classes.md#static-constructors)), provádění statických inicializátorů polí probíhá bezprostředně před spuštěním tohoto statického konstruktoru.</span><span class="sxs-lookup"><span data-stu-id="479b6-700">If a static constructor ([Static constructors](classes.md#static-constructors)) exists in the class, execution of the static field initializers occurs immediately prior to executing that static constructor.</span></span> <span data-ttu-id="479b6-701">V opačném případě jsou Inicializátory statických polí spouštěny v době závislé na implementaci před prvním použitím statického pole této třídy.</span><span class="sxs-lookup"><span data-stu-id="479b6-701">Otherwise, the static field initializers are executed at an implementation-dependent time prior to the first use of a static field of that class.</span></span> <span data-ttu-id="479b6-702">Příklad</span><span class="sxs-lookup"><span data-stu-id="479b6-702">The example</span></span>
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
<span data-ttu-id="479b6-703">může způsobit výstup:</span><span class="sxs-lookup"><span data-stu-id="479b6-703">might produce either the output:</span></span>
```
Init A
Init B
1 1
```
<span data-ttu-id="479b6-704">nebo výstup:</span><span class="sxs-lookup"><span data-stu-id="479b6-704">or the output:</span></span>
```
Init B
Init A
1 1
```
<span data-ttu-id="479b6-705">vzhledem k tomu, `X`že spuštění inicializátoru inicializátoru a `Y`inicializátoru může probíhat v obou objednávkách, jsou omezené jenom na to, aby se nastaly před odkazy na tato pole.</span><span class="sxs-lookup"><span data-stu-id="479b6-705">because the execution of `X`'s initializer and `Y`'s initializer could occur in either order; they are only constrained to occur before the references to those fields.</span></span> <span data-ttu-id="479b6-706">V tomto příkladu je však:</span><span class="sxs-lookup"><span data-stu-id="479b6-706">However, in the example:</span></span>
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
<span data-ttu-id="479b6-707">výstup musí být:</span><span class="sxs-lookup"><span data-stu-id="479b6-707">the output must be:</span></span>
```
Init B
Init A
1 1
```
<span data-ttu-id="479b6-708">vzhledem k tomu, `B`že pravidla pro použití statických konstruktorů (jak je definováno ve [statických konstruktorech](classes.md#static-constructors)) poskytují statické konstruktory (a proto `B`statické inicializátory pole) `A`musí být spuštěny před statickým Inicializátory konstruktoru a pole.</span><span class="sxs-lookup"><span data-stu-id="479b6-708">because the rules for when static constructors execute (as defined in [Static constructors](classes.md#static-constructors)) provide that `B`'s static constructor (and hence `B`'s static field initializers) must run before `A`'s static constructor and field initializers.</span></span>

#### <a name="instance-field-initialization"></a><span data-ttu-id="479b6-709">Inicializace pole instance</span><span class="sxs-lookup"><span data-stu-id="479b6-709">Instance field initialization</span></span>

<span data-ttu-id="479b6-710">Inicializátory proměnných pole instance třídy odpovídají sekvenci přiřazení, která jsou spouštěna ihned po zadání některého z konstruktorů instance ([Inicializátory konstruktoru](classes.md#constructor-initializers)) dané třídy.</span><span class="sxs-lookup"><span data-stu-id="479b6-710">The instance field variable initializers of a class correspond to a sequence of assignments that are executed immediately upon entry to any one of the instance constructors ([Constructor initializers](classes.md#constructor-initializers)) of that class.</span></span> <span data-ttu-id="479b6-711">Inicializátory proměnných jsou spouštěny v textovém pořadí, ve kterém jsou uvedeny v deklaraci třídy.</span><span class="sxs-lookup"><span data-stu-id="479b6-711">The variable initializers are executed in the textual order in which they appear in the class declaration.</span></span> <span data-ttu-id="479b6-712">Proces vytvoření a inicializace instance třídy je podrobněji popsán v části [konstruktory instancí](classes.md#instance-constructors).</span><span class="sxs-lookup"><span data-stu-id="479b6-712">The class instance creation and initialization process is described further in [Instance constructors](classes.md#instance-constructors).</span></span>

<span data-ttu-id="479b6-713">Inicializátor proměnné pro pole instance nemůže odkazovat na vytvořenou instanci.</span><span class="sxs-lookup"><span data-stu-id="479b6-713">A variable initializer for an instance field cannot reference the instance being created.</span></span> <span data-ttu-id="479b6-714">Proto se jedná o chybu při kompilaci na odkazování `this` v inicializátoru proměnné, protože se jedná o chybu při kompilaci pro inicializátor proměnné na odkazování na člen instance pomocí *simple_name*.</span><span class="sxs-lookup"><span data-stu-id="479b6-714">Thus, it is a compile-time error to reference `this` in a variable initializer, as it is a compile-time error for a variable initializer to reference any instance member through a *simple_name*.</span></span> <span data-ttu-id="479b6-715">V příkladu</span><span class="sxs-lookup"><span data-stu-id="479b6-715">In the example</span></span>
```csharp
class A
{
    int x = 1;
    int y = x + 1;        // Error, reference to instance member of this
}
```
<span data-ttu-id="479b6-716">Proměnná inicializátoru pro `y` výsledky v době kompilace má za následek chybu při kompilaci, protože odkazuje na člen vytvářené instance.</span><span class="sxs-lookup"><span data-stu-id="479b6-716">the variable initializer for `y` results in a compile-time error because it references a member of the instance being created.</span></span>

## <a name="methods"></a><span data-ttu-id="479b6-717">Metody</span><span class="sxs-lookup"><span data-stu-id="479b6-717">Methods</span></span>

<span data-ttu-id="479b6-718">***Metoda*** je člen, který implementuje výpočet nebo akci, kterou lze provést pomocí objektu nebo třídy.</span><span class="sxs-lookup"><span data-stu-id="479b6-718">A ***method*** is a member that implements a computation or action that can be performed by an object or class.</span></span> <span data-ttu-id="479b6-719">Metody jsou deklarovány pomocí *method_declaration*s:</span><span class="sxs-lookup"><span data-stu-id="479b6-719">Methods are declared using *method_declaration*s:</span></span>

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

<span data-ttu-id="479b6-720">*Method_declaration* může zahrnovat sadu *atributů* ([atributů](attributes.md)) a platnou kombinaci modifikátorů čtyř přístupu ([modifikátory](classes.md#access-modifiers) `new` přístupu), a to ([nový modifikátor](classes.md#the-new-modifier)) `static` ([statický a metody instance](classes.md#static-and-instance-methods)), `virtual` ([virtuální metody](classes.md#virtual-methods)), `override` ([metody](classes.md#override-methods) `abstract` přepisu) `sealed` ,[(](classes.md#sealed-methods)metody přepsání), ([abstraktní](classes.md#abstract-methods)metody) a `extern`([Externí metody](classes.md#external-methods)) modifikátory.</span><span class="sxs-lookup"><span data-stu-id="479b6-720">A *method_declaration* may include a set of *attributes* ([Attributes](attributes.md)) and a valid combination of the four access modifiers ([Access modifiers](classes.md#access-modifiers)), the `new` ([The new modifier](classes.md#the-new-modifier)),  `static` ([Static and instance methods](classes.md#static-and-instance-methods)), `virtual` ([Virtual methods](classes.md#virtual-methods)), `override` ([Override methods](classes.md#override-methods)), `sealed` ([Sealed methods](classes.md#sealed-methods)), `abstract` ([Abstract methods](classes.md#abstract-methods)), and `extern` ([External methods](classes.md#external-methods)) modifiers.</span></span>

<span data-ttu-id="479b6-721">Deklarace má platnou kombinaci modifikátorů, pokud jsou splněny všechny následující podmínky:</span><span class="sxs-lookup"><span data-stu-id="479b6-721">A declaration has a valid combination of modifiers if all of the following are true:</span></span>

*  <span data-ttu-id="479b6-722">Deklarace zahrnuje platnou kombinaci modifikátorů přístupu ([modifikátory přístupu](classes.md#access-modifiers)).</span><span class="sxs-lookup"><span data-stu-id="479b6-722">The declaration includes a valid combination of access modifiers ([Access modifiers](classes.md#access-modifiers)).</span></span>
*  <span data-ttu-id="479b6-723">Deklarace nezahrnuje stejný modifikátor víckrát.</span><span class="sxs-lookup"><span data-stu-id="479b6-723">The declaration does not include the same modifier multiple times.</span></span>
*  <span data-ttu-id="479b6-724">Deklarace zahrnuje maximálně jeden z následujících modifikátorů: `static`, `virtual`, a `override`.</span><span class="sxs-lookup"><span data-stu-id="479b6-724">The declaration includes at most one of the following modifiers: `static`, `virtual`, and `override`.</span></span>
*  <span data-ttu-id="479b6-725">Deklarace zahrnuje maximálně jeden z následujících modifikátorů: `new` a. `override`</span><span class="sxs-lookup"><span data-stu-id="479b6-725">The declaration includes at most one of the following modifiers: `new` and `override`.</span></span>
*  <span data-ttu-id="479b6-726">`abstract` Pokud deklarace obsahuje modifikátor, pak deklarace neobsahuje žádné následující modifikátory: `virtual` `static`, `sealed` nebo `extern`.</span><span class="sxs-lookup"><span data-stu-id="479b6-726">If the declaration includes the `abstract` modifier, then the declaration does not include any of the following modifiers: `static`, `virtual`, `sealed` or `extern`.</span></span>
*  <span data-ttu-id="479b6-727">Pokud deklarace obsahuje `private` modifikátor, pak deklarace neobsahuje žádné následující modifikátory: `virtual`, `override`nebo `abstract`.</span><span class="sxs-lookup"><span data-stu-id="479b6-727">If the declaration includes the `private` modifier, then the declaration does not include any of the following modifiers: `virtual`, `override`, or `abstract`.</span></span>
*  <span data-ttu-id="479b6-728">Pokud deklarace obsahuje `sealed` modifikátor, pak deklarace `override` obsahuje také modifikátor.</span><span class="sxs-lookup"><span data-stu-id="479b6-728">If the declaration includes the `sealed` modifier, then the declaration also includes the `override` modifier.</span></span>
*  <span data-ttu-id="479b6-729">Pokud deklarace obsahuje `partial` modifikátor, pak nezahrnuje žádné z následujících modifikátorů: `new`, `public`, `protected`, `internal`, `private`, `virtual`,, `override` `sealed` , `abstract`nebo .`extern`</span><span class="sxs-lookup"><span data-stu-id="479b6-729">If the declaration includes the `partial` modifier, then it does not include any of the following modifiers: `new`, `public`, `protected`, `internal`, `private`, `virtual`, `sealed`, `override`, `abstract`, or `extern`.</span></span>

<span data-ttu-id="479b6-730">Metoda, která má `async` modifikátor, je asynchronní funkce a postupuje podle pravidel popsaných v tématu [asynchronní funkce](classes.md#async-functions).</span><span class="sxs-lookup"><span data-stu-id="479b6-730">A method that has the `async` modifier is an async function and follows the rules described in [Async functions](classes.md#async-functions).</span></span>

<span data-ttu-id="479b6-731">Typ *deklarace* metody určuje typ počítané hodnoty a vrácenou metodou.</span><span class="sxs-lookup"><span data-stu-id="479b6-731">The *return_type* of a method declaration specifies the type of the value computed and returned by the method.</span></span> <span data-ttu-id="479b6-732">*Typ* je `void` v případě, že metoda nevrací hodnotu.</span><span class="sxs-lookup"><span data-stu-id="479b6-732">The *return_type* is `void` if the method does not return a value.</span></span> <span data-ttu-id="479b6-733">Pokud deklarace obsahuje `partial` modifikátor, pak návratový typ musí být `void`.</span><span class="sxs-lookup"><span data-stu-id="479b6-733">If the declaration includes the `partial` modifier, then the return type must be `void`.</span></span>

<span data-ttu-id="479b6-734">*MEMBER_NAME* Určuje název metody.</span><span class="sxs-lookup"><span data-stu-id="479b6-734">The *member_name* specifies the name of the method.</span></span> <span data-ttu-id="479b6-735">Pokud metoda není explicitní implementací člena rozhraní ([explicitní implementace členů rozhraní](interfaces.md#explicit-interface-member-implementations)), *MEMBER_NAME* je pouze *identifikátor*.</span><span class="sxs-lookup"><span data-stu-id="479b6-735">Unless the method is an explicit interface member implementation ([Explicit interface member implementations](interfaces.md#explicit-interface-member-implementations)), the *member_name* is simply an *identifier*.</span></span> <span data-ttu-id="479b6-736">Pro explicitní implementaci člena rozhraní se *MEMBER_NAME* skládá z *INTERFACE_TYPE* následovaných "`.`" a *identifikátorem*.</span><span class="sxs-lookup"><span data-stu-id="479b6-736">For an explicit interface member implementation, the *member_name* consists of an *interface_type* followed by a "`.`" and an *identifier*.</span></span>

<span data-ttu-id="479b6-737">Volitelné *type_parameter_list* Určuje parametry typu metody ([parametry typu](classes.md#type-parameters)).</span><span class="sxs-lookup"><span data-stu-id="479b6-737">The optional *type_parameter_list* specifies the type parameters of the method ([Type parameters](classes.md#type-parameters)).</span></span> <span data-ttu-id="479b6-738">Pokud je určena *type_parameter_list* , metoda je ***Obecná metoda***.</span><span class="sxs-lookup"><span data-stu-id="479b6-738">If a *type_parameter_list* is specified the method is a ***generic method***.</span></span> <span data-ttu-id="479b6-739">Pokud má `extern` metoda modifikátor, nelze zadat *type_parameter_list* .</span><span class="sxs-lookup"><span data-stu-id="479b6-739">If the method has an `extern` modifier, a *type_parameter_list* cannot be specified.</span></span>

<span data-ttu-id="479b6-740">Volitelné *formal_parameter_list* Určuje parametry metody ([parametry metody](classes.md#method-parameters)).</span><span class="sxs-lookup"><span data-stu-id="479b6-740">The optional *formal_parameter_list* specifies the parameters of the method ([Method parameters](classes.md#method-parameters)).</span></span>

<span data-ttu-id="479b6-741">Volitelná *type_parameter_constraints_clause*s určují omezení pro jednotlivé parametry typu ([omezení parametrů typu](classes.md#type-parameter-constraints)) a mohou být zadána pouze v případě, že je zadán také parametr *type_parameter_list* a metoda nemá `override` modifikátor.</span><span class="sxs-lookup"><span data-stu-id="479b6-741">The optional *type_parameter_constraints_clause*s specify constraints on individual type parameters ([Type parameter constraints](classes.md#type-parameter-constraints)) and may only be specified if a *type_parameter_list* is also supplied, and the method does not have an `override` modifier.</span></span>

<span data-ttu-id="479b6-742">*Typ* a každý z typů, na které se odkazuje v *formal_parameter_list* metody, musí být alespoň tak přístupný jako samotná metoda ([Omezení přístupnosti](basic-concepts.md#accessibility-constraints)).</span><span class="sxs-lookup"><span data-stu-id="479b6-742">The *return_type* and each of the types referenced in the *formal_parameter_list* of a method must be at least as accessible as the method itself ([Accessibility constraints](basic-concepts.md#accessibility-constraints)).</span></span>

<span data-ttu-id="479b6-743">*Method_body* je buď středník, ***text příkazu*** nebo ***tělo výrazu***.</span><span class="sxs-lookup"><span data-stu-id="479b6-743">The *method_body* is either a semicolon, a ***statement body*** or an ***expression body***.</span></span> <span data-ttu-id="479b6-744">Tělo příkazu se skládá z *bloku*, který určuje příkazy, které mají být provedeny při volání metody.</span><span class="sxs-lookup"><span data-stu-id="479b6-744">A statement body consists of a *block*, which specifies the statements to execute when the method is invoked.</span></span> <span data-ttu-id="479b6-745">Tělo výrazu se skládá `=>` za následováním *výrazu* a středníkem a označuje jeden výraz, který má být proveden při vyvolání metody.</span><span class="sxs-lookup"><span data-stu-id="479b6-745">An expression body consists of `=>` followed by an *expression* and a semicolon, and denotes a single expression to perform when the method is invoked.</span></span> 

<span data-ttu-id="479b6-746">Pro `abstract` metody `extern` a *method_body* sestávají pouze středníkem.</span><span class="sxs-lookup"><span data-stu-id="479b6-746">For `abstract` and `extern` methods, the *method_body* consists simply of a semicolon.</span></span> <span data-ttu-id="479b6-747">Pro `partial` metody, které může *method_body* obsahovat buď středník, blok těla nebo tělo výrazu.</span><span class="sxs-lookup"><span data-stu-id="479b6-747">For `partial` methods the *method_body* may consist of either a semicolon, a block body or an expression body.</span></span> <span data-ttu-id="479b6-748">Pro všechny ostatní metody je *method_body* buď tělo bloku, nebo tělo výrazu.</span><span class="sxs-lookup"><span data-stu-id="479b6-748">For all other methods, the *method_body* is either a block body or an expression body.</span></span>

<span data-ttu-id="479b6-749">Pokud se *method_body* skládá ze středníku, deklarace nesmí obsahovat `async` modifikátor.</span><span class="sxs-lookup"><span data-stu-id="479b6-749">If the *method_body* consists of a semicolon, then the declaration may not include the `async` modifier.</span></span>

<span data-ttu-id="479b6-750">Název, seznam parametrů typu a seznam formálních parametrů metody definují podpis ([signatury a přetížení](basic-concepts.md#signatures-and-overloading)) metody.</span><span class="sxs-lookup"><span data-stu-id="479b6-750">The name, the type parameter list and the formal parameter list of a method define the signature ([Signatures and overloading](basic-concepts.md#signatures-and-overloading)) of the method.</span></span> <span data-ttu-id="479b6-751">Konkrétně signatura metody se skládá z jejího názvu, počtu parametrů typu a počtu, modifikátorů a typů jeho formálních parametrů.</span><span class="sxs-lookup"><span data-stu-id="479b6-751">Specifically, the signature of a method consists of its name, the number of type parameters and the number, modifiers, and types of its formal parameters.</span></span> <span data-ttu-id="479b6-752">Pro tyto účely je jakýkoli parametr typu metody, která se vyskytuje v typu formálního parametru, identifikován jako není jeho názvem, ale podle jeho ordinální pozice v seznamu argumentů typu metody. Návratový typ není součástí signatury metody, ani se nejedná o názvy parametrů typu nebo formální parametry.</span><span class="sxs-lookup"><span data-stu-id="479b6-752">For these purposes, any type parameter of the method that occurs in the type of a formal parameter is identified not by its name, but by its ordinal position in the type argument list of the method.The return type is not part of a method's signature, nor are the names of the type parameters or the formal parameters.</span></span>

<span data-ttu-id="479b6-753">Název metody se musí lišit od názvů všech ostatních neodpovídajících metod deklarovaných ve stejné třídě.</span><span class="sxs-lookup"><span data-stu-id="479b6-753">The name of a method must differ from the names of all other non-methods declared in the same class.</span></span> <span data-ttu-id="479b6-754">Kromě toho signatura metody se musí lišit od signatur všech ostatních metod deklarovaných ve stejné třídě a dvě metody deklarované ve stejné třídě nemusí mít signatury, které se liší výhradně pomocí `ref` a. `out`</span><span class="sxs-lookup"><span data-stu-id="479b6-754">In addition, the signature of a method must differ from the signatures of all other methods declared in the same class, and two methods declared in the same class may not have signatures that differ solely by `ref` and `out`.</span></span>

<span data-ttu-id="479b6-755">*Type_parameter*metody jsou v oboru v rámci *method_declaration*a lze je použít k vytvoření typů v celém oboru v typu *typ*, *method_body*a *type_parameter_constraints_clause*s, ale ne v *atributech*.</span><span class="sxs-lookup"><span data-stu-id="479b6-755">The method's *type_parameter*s are in scope throughout the *method_declaration*, and can be used to form types throughout that scope in *return_type*, *method_body*, and *type_parameter_constraints_clause*s but not in *attributes*.</span></span>

<span data-ttu-id="479b6-756">Všechny formální parametry a parametry typu musí mít jiné názvy.</span><span class="sxs-lookup"><span data-stu-id="479b6-756">All formal parameters and type parameters must have different names.</span></span>

### <a name="method-parameters"></a><span data-ttu-id="479b6-757">Parametry metody</span><span class="sxs-lookup"><span data-stu-id="479b6-757">Method parameters</span></span>

<span data-ttu-id="479b6-758">Parametry metody, pokud existuje, jsou deklarovány metodou *formal_parameter_list*metody.</span><span class="sxs-lookup"><span data-stu-id="479b6-758">The parameters of a method, if any, are declared by the method's *formal_parameter_list*.</span></span>

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

<span data-ttu-id="479b6-759">Seznam formálních parametrů se skládá z jednoho nebo více parametrů oddělených čárkami, jejichž *parameter_array*může být pouze poslední.</span><span class="sxs-lookup"><span data-stu-id="479b6-759">The formal parameter list consists of one or more comma-separated parameters of which only the last may be a *parameter_array*.</span></span>

<span data-ttu-id="479b6-760">*Fixed_parameter* se skládá z volitelné sady *atributů* ([atributů](attributes.md) `ref` `out` ), volitelného nebo `this` modifikátoru *typu*, *identifikátoru* a volitelného *default_. Argument*.</span><span class="sxs-lookup"><span data-stu-id="479b6-760">A *fixed_parameter* consists of an optional set of *attributes* ([Attributes](attributes.md)), an optional `ref`, `out` or `this` modifier, a *type*, an *identifier* and an optional *default_argument*.</span></span> <span data-ttu-id="479b6-761">Každý *fixed_parameter* deklaruje parametr daného typu se zadaným názvem.</span><span class="sxs-lookup"><span data-stu-id="479b6-761">Each *fixed_parameter* declares a parameter of the given type with the given name.</span></span> <span data-ttu-id="479b6-762">`this` Modifikátor označuje metodu jako metodu rozšíření a je povolen pouze pro první parametr statické metody.</span><span class="sxs-lookup"><span data-stu-id="479b6-762">The `this` modifier designates the method as an extension method and is only allowed on the first parameter of a static method.</span></span> <span data-ttu-id="479b6-763">Rozšiřující metody jsou dále popsány v tématu [metody rozšíření](classes.md#extension-methods).</span><span class="sxs-lookup"><span data-stu-id="479b6-763">Extension methods are further described in [Extension methods](classes.md#extension-methods).</span></span>

<span data-ttu-id="479b6-764">*Fixed_parameter* s *default_argument* je označován jako ***volitelný parametr***, zatímco *fixed_parameter* bez *default_argument* je ***povinný parametr***.</span><span class="sxs-lookup"><span data-stu-id="479b6-764">A *fixed_parameter* with a *default_argument* is known as an ***optional parameter***, whereas a *fixed_parameter* without a *default_argument* is a ***required parameter***.</span></span> <span data-ttu-id="479b6-765">Požadovaný parametr se nesmí nacházet po volitelném parametru v *formal_parameter_list*.</span><span class="sxs-lookup"><span data-stu-id="479b6-765">A required parameter may not appear after an optional parameter in a *formal_parameter_list*.</span></span>

<span data-ttu-id="479b6-766">Parametr `ref` nebo `out` nemůže mít *default_argument*.</span><span class="sxs-lookup"><span data-stu-id="479b6-766">A `ref` or `out` parameter cannot have a *default_argument*.</span></span> <span data-ttu-id="479b6-767">*Výraz* v *default_argument* musí být jedna z následujících:</span><span class="sxs-lookup"><span data-stu-id="479b6-767">The *expression* in a *default_argument* must be one of the following:</span></span>

*  <span data-ttu-id="479b6-768">a *constant_expression*</span><span class="sxs-lookup"><span data-stu-id="479b6-768">a *constant_expression*</span></span>
*  <span data-ttu-id="479b6-769">výraz formuláře `new S()` , kde `S` je hodnotový typ</span><span class="sxs-lookup"><span data-stu-id="479b6-769">an expression of the form `new S()` where `S` is a value type</span></span>
*  <span data-ttu-id="479b6-770">výraz formuláře `default(S)` , kde `S` je hodnotový typ</span><span class="sxs-lookup"><span data-stu-id="479b6-770">an expression of the form `default(S)` where `S` is a value type</span></span>

<span data-ttu-id="479b6-771">*Výraz* musí být implicitně převoditelný pomocí identity nebo konverze s možnou hodnotou null na typ parametru.</span><span class="sxs-lookup"><span data-stu-id="479b6-771">The *expression* must be implicitly convertible by an identity or nullable conversion to the type of the parameter.</span></span>

<span data-ttu-id="479b6-772">Pokud dojde k volitelným parametrům v deklaraci částečné metody ([částečné metody](classes.md#partial-methods)), explicitní implementace člena rozhraní ([explicitní implementace členů rozhraní](interfaces.md#explicit-interface-member-implementations)) nebo v deklaraci indexeru s jedním parametrem ([ Indexery](classes.md#indexers)) kompilátor by měl poskytnout upozornění, protože tito členové nemohou být nikdy vyvoláni způsobem, který umožňuje vynechat argumenty.</span><span class="sxs-lookup"><span data-stu-id="479b6-772">If optional parameters occur in an implementing partial method declaration ([Partial methods](classes.md#partial-methods)) , an explicit interface member implementation ([Explicit interface member implementations](interfaces.md#explicit-interface-member-implementations)) or in a single-parameter indexer declaration ([Indexers](classes.md#indexers)) the compiler should give a warning, since these members can never be invoked in a way that permits arguments to be omitted.</span></span>

<span data-ttu-id="479b6-773">*Parameter_array* se skládá z volitelné sady *atributů* ( `params` [atributů](attributes.md)), modifikátoru, *array_type*a *identifikátoru*.</span><span class="sxs-lookup"><span data-stu-id="479b6-773">A *parameter_array* consists of an optional set of *attributes* ([Attributes](attributes.md)), a `params` modifier, an *array_type*, and an *identifier*.</span></span> <span data-ttu-id="479b6-774">Pole parametrů deklaruje jeden parametr daného typu pole se zadaným názvem.</span><span class="sxs-lookup"><span data-stu-id="479b6-774">A parameter array declares a single parameter of the given array type with the given name.</span></span> <span data-ttu-id="479b6-775">*Array_type* pole parametrů musí být jednorozměrného typu pole ([typy polí](arrays.md#array-types)).</span><span class="sxs-lookup"><span data-stu-id="479b6-775">The *array_type* of a parameter array must be a single-dimensional array type ([Array types](arrays.md#array-types)).</span></span> <span data-ttu-id="479b6-776">V volání metody umožňuje pole parametrů zadat buď jeden argument daného typu pole, nebo umožňuje zadat nula nebo více argumentů typu elementu pole.</span><span class="sxs-lookup"><span data-stu-id="479b6-776">In a method invocation, a parameter array permits either a single argument of the given array type to be specified, or it permits zero or more arguments of the array element type to be specified.</span></span> <span data-ttu-id="479b6-777">Pole parametrů jsou podrobněji popsány v [polích parametrů](classes.md#parameter-arrays).</span><span class="sxs-lookup"><span data-stu-id="479b6-777">Parameter arrays are described further in [Parameter arrays](classes.md#parameter-arrays).</span></span>

<span data-ttu-id="479b6-778">K *parameter_array* může dojít po volitelném parametru, ale nemůže mít výchozí hodnotu – vynechání argumentů pro *parameter_array* by vedlo k vytvoření prázdného pole.</span><span class="sxs-lookup"><span data-stu-id="479b6-778">A *parameter_array* may occur after an optional parameter, but cannot have a default value -- the omission of arguments for a *parameter_array* would instead result in the creation of an empty array.</span></span>

<span data-ttu-id="479b6-779">Následující příklad znázorňuje různé druhy parametrů:</span><span class="sxs-lookup"><span data-stu-id="479b6-779">The following example illustrates different kinds of parameters:</span></span>
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

<span data-ttu-id="479b6-780">V *formal_parameter_list* pro `M` `b` `d` `s` `t` je povinný parametr ref, je povinný parametr`o` hodnoty,, a jsou volitelné parametry hodnoty. `i` a `a` je pole parametrů.</span><span class="sxs-lookup"><span data-stu-id="479b6-780">In the *formal_parameter_list* for `M`, `i` is a required ref parameter, `d` is a required value parameter, `b`, `s`, `o` and `t` are optional value parameters and `a` is a parameter array.</span></span>

<span data-ttu-id="479b6-781">Deklarace metody vytvoří samostatný prostor deklarace pro parametry, parametry typu a místní proměnné.</span><span class="sxs-lookup"><span data-stu-id="479b6-781">A method declaration creates a separate declaration space for parameters, type parameters and local variables.</span></span> <span data-ttu-id="479b6-782">Názvy jsou představeny do tohoto prostoru deklarace seznamem parametrů typu a seznamem formálních parametrů metody a deklarací místní proměnné v *bloku* metody.</span><span class="sxs-lookup"><span data-stu-id="479b6-782">Names are introduced into this declaration space by the type parameter list and the formal parameter list of the method and by local variable declarations in the *block* of the method.</span></span> <span data-ttu-id="479b6-783">Jedná se o chybu, pokud dva členy prostoru deklarace metod mají stejný název.</span><span class="sxs-lookup"><span data-stu-id="479b6-783">It is an error for two members of a method declaration space to have the same name.</span></span> <span data-ttu-id="479b6-784">Jedná se o chybu pro místo deklarace metody a místní prostor deklarace proměnné vnořeného prostoru deklarace, který obsahuje prvky se stejným názvem.</span><span class="sxs-lookup"><span data-stu-id="479b6-784">It is an error for the method declaration space and the local variable declaration space of a nested declaration space to contain elements with the same name.</span></span>

<span data-ttu-id="479b6-785">Vyvolání metody ([vyvolání metod](expressions.md#method-invocations)) vytvoří kopii, která je specifická pro toto vyvolání, formální parametry a lokální proměnné metody a seznam argumentů vyvolání přiřadí k nově vytvořenému formálnímu seznamu hodnoty nebo odkazy na proměnné. ukazatelů.</span><span class="sxs-lookup"><span data-stu-id="479b6-785">A method invocation ([Method invocations](expressions.md#method-invocations)) creates a copy, specific to that invocation, of the formal parameters and local variables of the method, and the argument list of the invocation assigns values or variable references to the newly created formal parameters.</span></span> <span data-ttu-id="479b6-786">V rámci *bloku* metody mohou být formální parametry odkazovány pomocí jejich identifikátorů ve výrazech *simple_name* ([jednoduché názvy](expressions.md#simple-names)).</span><span class="sxs-lookup"><span data-stu-id="479b6-786">Within the *block* of a method, formal parameters can be referenced by their identifiers in *simple_name* expressions ([Simple names](expressions.md#simple-names)).</span></span>

<span data-ttu-id="479b6-787">Existují čtyři druhy formálních parametrů:</span><span class="sxs-lookup"><span data-stu-id="479b6-787">There are four kinds of formal parameters:</span></span>

*  <span data-ttu-id="479b6-788">Parametry hodnot, které jsou deklarovány bez modifikátorů.</span><span class="sxs-lookup"><span data-stu-id="479b6-788">Value parameters, which are declared without any modifiers.</span></span>
*  <span data-ttu-id="479b6-789">Referenční parametry, které jsou deklarovány s `ref` modifikátorem.</span><span class="sxs-lookup"><span data-stu-id="479b6-789">Reference parameters, which are declared with the `ref` modifier.</span></span>
*  <span data-ttu-id="479b6-790">Výstupní parametry, které jsou deklarovány s `out` modifikátorem.</span><span class="sxs-lookup"><span data-stu-id="479b6-790">Output parameters, which are declared with the `out` modifier.</span></span>
*  <span data-ttu-id="479b6-791">Pole parametrů, která jsou deklarována s `params` modifikátorem.</span><span class="sxs-lookup"><span data-stu-id="479b6-791">Parameter arrays, which are declared with the `params` modifier.</span></span>

<span data-ttu-id="479b6-792">Jak je popsáno v části [signatury a přetížení](basic-concepts.md#signatures-and-overloading), `ref` jsou `out` modifikátory a součástí signatury metody, ale `params` modifikátor ne.</span><span class="sxs-lookup"><span data-stu-id="479b6-792">As described in [Signatures and overloading](basic-concepts.md#signatures-and-overloading), the `ref` and `out` modifiers are part of a method's signature, but the `params` modifier is not.</span></span>

#### <a name="value-parameters"></a><span data-ttu-id="479b6-793">Parametry hodnoty</span><span class="sxs-lookup"><span data-stu-id="479b6-793">Value parameters</span></span>

<span data-ttu-id="479b6-794">Parametr deklarovaný bez modifikátorů je parametrem hodnoty.</span><span class="sxs-lookup"><span data-stu-id="479b6-794">A parameter declared with no modifiers is a value parameter.</span></span> <span data-ttu-id="479b6-795">Parametr hodnoty odpovídá místní proměnné, která vrací počáteční hodnotu z odpovídajícího argumentu zadaného ve volání metody.</span><span class="sxs-lookup"><span data-stu-id="479b6-795">A value parameter corresponds to a local variable that gets its initial value from the corresponding argument supplied in the method invocation.</span></span>

<span data-ttu-id="479b6-796">Pokud je formální parametr parametr hodnoty, odpovídající argument ve volání metody musí být výraz, který je implicitně konvertibilní ([implicitní převody](conversions.md#implicit-conversions)) na formální typ parametru.</span><span class="sxs-lookup"><span data-stu-id="479b6-796">When a formal parameter is a value parameter, the corresponding argument in a method invocation must be an expression that is implicitly convertible ([Implicit conversions](conversions.md#implicit-conversions)) to the formal parameter type.</span></span>

<span data-ttu-id="479b6-797">Metoda je povolena k přiřazení nových hodnot parametru hodnoty.</span><span class="sxs-lookup"><span data-stu-id="479b6-797">A method is permitted to assign new values to a value parameter.</span></span> <span data-ttu-id="479b6-798">Tato přiřazení mají vliv jenom na umístění místního úložiště reprezentované parametrem value – nemají žádný vliv na skutečný argument uvedený ve volání metody.</span><span class="sxs-lookup"><span data-stu-id="479b6-798">Such assignments only affect the local storage location represented by the value parameter—they have no effect on the actual argument given in the method invocation.</span></span>

#### <a name="reference-parameters"></a><span data-ttu-id="479b6-799">Parametry odkazu</span><span class="sxs-lookup"><span data-stu-id="479b6-799">Reference parameters</span></span>

<span data-ttu-id="479b6-800">Parametr deklarovaný s `ref` modifikátorem je parametr odkazu.</span><span class="sxs-lookup"><span data-stu-id="479b6-800">A parameter declared with a `ref` modifier is a reference parameter.</span></span> <span data-ttu-id="479b6-801">Na rozdíl od parametru hodnoty nevytvoří parametr reference nové umístění úložiště.</span><span class="sxs-lookup"><span data-stu-id="479b6-801">Unlike a value parameter, a reference parameter does not create a new storage location.</span></span> <span data-ttu-id="479b6-802">Místo toho parametr reference představuje stejné umístění úložiště jako proměnná zadaná jako argument ve volání metody.</span><span class="sxs-lookup"><span data-stu-id="479b6-802">Instead, a reference parameter represents the same storage location as the variable given as the argument in the method invocation.</span></span>

<span data-ttu-id="479b6-803">Pokud je formálním parametrem odkazový parametr, odpovídající argument ve volání metody musí sestávat z klíčového slova `ref` následovaného *variable_reference* ([přesné pravidlo pro určení jednoznačného přiřazení](variables.md#precise-rules-for-determining-definite-assignment)) stejné. jako formální parametr zadejte.</span><span class="sxs-lookup"><span data-stu-id="479b6-803">When a formal parameter is a reference parameter, the corresponding argument in a method invocation must consist of the keyword `ref` followed by a *variable_reference* ([Precise rules for determining definite assignment](variables.md#precise-rules-for-determining-definite-assignment)) of the same type as the formal parameter.</span></span> <span data-ttu-id="479b6-804">Proměnná musí být jednoznačně přiřazena dříve, než může být předána jako parametr reference.</span><span class="sxs-lookup"><span data-stu-id="479b6-804">A variable must be definitely assigned before it can be passed as a reference parameter.</span></span>

<span data-ttu-id="479b6-805">V rámci metody je referenční parametr vždy považován za jednoznačně přiřazený.</span><span class="sxs-lookup"><span data-stu-id="479b6-805">Within a method, a reference parameter is always considered definitely assigned.</span></span>

<span data-ttu-id="479b6-806">Metoda deklarovaná jako iterátor ([iterátory](classes.md#iterators)) nemůže mít parametry odkazu.</span><span class="sxs-lookup"><span data-stu-id="479b6-806">A method declared as an iterator ([Iterators](classes.md#iterators)) cannot have reference parameters.</span></span>

<span data-ttu-id="479b6-807">Příklad</span><span class="sxs-lookup"><span data-stu-id="479b6-807">The example</span></span>
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
<span data-ttu-id="479b6-808">Vytvoří výstup</span><span class="sxs-lookup"><span data-stu-id="479b6-808">produces the output</span></span>
```
i = 2, j = 1
```

<span data-ttu-id="479b6-809">Pro `Swap` vyvolání v `Main`, `x` představuje a`i` představuje`j`. `y`</span><span class="sxs-lookup"><span data-stu-id="479b6-809">For the invocation of `Swap` in `Main`, `x` represents `i` and `y` represents `j`.</span></span> <span data-ttu-id="479b6-810">Proto má vyvolání účinek na záměnu hodnot `i` a. `j`</span><span class="sxs-lookup"><span data-stu-id="479b6-810">Thus, the invocation has the effect of swapping the values of `i` and `j`.</span></span>

<span data-ttu-id="479b6-811">V metodě, která přebírá parametry odkazu, je možné, že více názvů představuje stejné umístění úložiště.</span><span class="sxs-lookup"><span data-stu-id="479b6-811">In a method that takes reference parameters it is possible for multiple names to represent the same storage location.</span></span> <span data-ttu-id="479b6-812">V příkladu</span><span class="sxs-lookup"><span data-stu-id="479b6-812">In the example</span></span>
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
<span data-ttu-id="479b6-813">`F` vyvolání v `s` rámcipředává`b`odkaz na pro i `a`. `G`</span><span class="sxs-lookup"><span data-stu-id="479b6-813">the invocation of `F` in `G` passes a reference to `s` for both `a` and `b`.</span></span> <span data-ttu-id="479b6-814">Pro toto vyvolání se tedy `s`názvy, `a`a `b` všechny odkazují na stejné umístění úložiště a tři přiřazení všechny upraví pole `s`instance.</span><span class="sxs-lookup"><span data-stu-id="479b6-814">Thus, for that invocation, the names `s`, `a`, and `b` all refer to the same storage location, and the three assignments all modify the instance field `s`.</span></span>

#### <a name="output-parameters"></a><span data-ttu-id="479b6-815">Výstupní parametry</span><span class="sxs-lookup"><span data-stu-id="479b6-815">Output parameters</span></span>

<span data-ttu-id="479b6-816">Parametr deklarovaný s `out` modifikátorem je výstupní parametr.</span><span class="sxs-lookup"><span data-stu-id="479b6-816">A parameter declared with an `out` modifier is an output parameter.</span></span> <span data-ttu-id="479b6-817">Podobně jako parametr reference nevytváří výstupní parametr nové umístění úložiště.</span><span class="sxs-lookup"><span data-stu-id="479b6-817">Similar to a reference parameter, an output parameter does not create a new storage location.</span></span> <span data-ttu-id="479b6-818">Namísto toho výstupní parametr představuje stejné umístění úložiště jako proměnná zadaná jako argument ve volání metody.</span><span class="sxs-lookup"><span data-stu-id="479b6-818">Instead, an output parameter represents the same storage location as the variable given as the argument in the method invocation.</span></span>

<span data-ttu-id="479b6-819">Pokud je formálním parametrem výstupní parametr, odpovídající argument ve volání metody musí sestávat z klíčového slova `out` následovaného *variable_reference* ([přesné pravidlo pro určení jednoznačného přiřazení](variables.md#precise-rules-for-determining-definite-assignment)) stejné. jako formální parametr zadejte.</span><span class="sxs-lookup"><span data-stu-id="479b6-819">When a formal parameter is an output parameter, the corresponding argument in a method invocation must consist of the keyword `out` followed by a *variable_reference* ([Precise rules for determining definite assignment](variables.md#precise-rules-for-determining-definite-assignment)) of the same type as the formal parameter.</span></span> <span data-ttu-id="479b6-820">Proměnná nemusí být jednoznačně přiřazena, než může být předána jako výstupní parametr, ale po vyvolání, kde byla proměnná předána jako výstupní parametr, je proměnná považována za jednoznačně přiřazenou.</span><span class="sxs-lookup"><span data-stu-id="479b6-820">A variable need not be definitely assigned before it can be passed as an output parameter, but following an invocation where a variable was passed as an output parameter, the variable is considered definitely assigned.</span></span>

<span data-ttu-id="479b6-821">V rámci metody, stejně jako místní proměnná, je výstupní parametr zpočátku považován za nepřiřazený a musí být jednoznačně přiřazen před použitím jeho hodnoty.</span><span class="sxs-lookup"><span data-stu-id="479b6-821">Within a method, just like a local variable, an output parameter is initially considered unassigned and must be definitely assigned before its value is used.</span></span>

<span data-ttu-id="479b6-822">Každý výstupní parametr metody musí být jednoznačně přiřazen předtím, než se metoda vrátí.</span><span class="sxs-lookup"><span data-stu-id="479b6-822">Every output parameter of a method must be definitely assigned before the method returns.</span></span>

<span data-ttu-id="479b6-823">Metoda deklarovaná jako částečná metoda ([částečné metody](classes.md#partial-methods)) nebo iterátory ([iterátory](classes.md#iterators)) nemůže mít výstupní parametry.</span><span class="sxs-lookup"><span data-stu-id="479b6-823">A method declared as a partial method ([Partial methods](classes.md#partial-methods)) or an iterator ([Iterators](classes.md#iterators)) cannot have output parameters.</span></span>

<span data-ttu-id="479b6-824">Výstupní parametry jsou obvykle používány v metodách, které vytváří více návratových hodnot.</span><span class="sxs-lookup"><span data-stu-id="479b6-824">Output parameters are typically used in methods that produce multiple return values.</span></span> <span data-ttu-id="479b6-825">Příklad:</span><span class="sxs-lookup"><span data-stu-id="479b6-825">For example:</span></span>
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

<span data-ttu-id="479b6-826">Příklad vytvoří výstup:</span><span class="sxs-lookup"><span data-stu-id="479b6-826">The example produces the output:</span></span>
```
c:\Windows\System\
hello.txt
```

<span data-ttu-id="479b6-827">Všimněte si, `dir` že `name` proměnné a lze odřadit `SplitPath`před jejich předáním a že jsou považovány za jednoznačně přiřazené po volání.</span><span class="sxs-lookup"><span data-stu-id="479b6-827">Note that the `dir` and `name` variables can be unassigned before they are passed to `SplitPath`, and that they are considered definitely assigned following the call.</span></span>

#### <a name="parameter-arrays"></a><span data-ttu-id="479b6-828">Pole parametrů</span><span class="sxs-lookup"><span data-stu-id="479b6-828">Parameter arrays</span></span>

<span data-ttu-id="479b6-829">Parametr deklarovaný s `params` modifikátorem je pole parametrů.</span><span class="sxs-lookup"><span data-stu-id="479b6-829">A parameter declared with a `params` modifier is a parameter array.</span></span> <span data-ttu-id="479b6-830">Pokud seznam formálních parametrů obsahuje pole parametrů, musí být posledním parametrem v seznamu a musí být jednorozměrného typu pole.</span><span class="sxs-lookup"><span data-stu-id="479b6-830">If a formal parameter list includes a parameter array, it must be the last parameter in the list and it must be of a single-dimensional array type.</span></span> <span data-ttu-id="479b6-831">Typy `string[]` `string[,]` a `string[][]` lze například použít jako typ pole parametrů, ale typ ale nemůže.</span><span class="sxs-lookup"><span data-stu-id="479b6-831">For example, the types `string[]` and `string[][]` can be used as the type of a parameter array, but the type `string[,]` can not.</span></span> <span data-ttu-id="479b6-832">`params` Modifikátor nemůžete kombinovat s `ref` modifikátory a `out`.</span><span class="sxs-lookup"><span data-stu-id="479b6-832">It is not possible to combine the `params` modifier with the modifiers `ref` and `out`.</span></span>

<span data-ttu-id="479b6-833">Pole parametrů povoluje, aby byly argumenty zadány jedním ze dvou způsobů volání metody:</span><span class="sxs-lookup"><span data-stu-id="479b6-833">A parameter array permits arguments to be specified in one of two ways in a method invocation:</span></span>

*  <span data-ttu-id="479b6-834">Argument zadaný pro pole parametrů může být jeden výraz, který je implicitně převoditelný ([implicitní převod](conversions.md#implicit-conversions)) na typ pole parametrů.</span><span class="sxs-lookup"><span data-stu-id="479b6-834">The argument given for a parameter array can be a single expression that is implicitly convertible ([Implicit conversions](conversions.md#implicit-conversions)) to the parameter array type.</span></span> <span data-ttu-id="479b6-835">V tomto případě pole parametrů funguje přesně jako parametr hodnoty.</span><span class="sxs-lookup"><span data-stu-id="479b6-835">In this case, the parameter array acts precisely like a value parameter.</span></span>
*  <span data-ttu-id="479b6-836">Alternativně může vyvolání zadat nula nebo více argumentů pro pole parametrů, kde každý argument je výraz, který je implicitně převoditelný ([implicitní převod](conversions.md#implicit-conversions)) na typ prvku pole parametrů.</span><span class="sxs-lookup"><span data-stu-id="479b6-836">Alternatively, the invocation can specify zero or more arguments for the parameter array, where each argument is an expression that is implicitly convertible ([Implicit conversions](conversions.md#implicit-conversions)) to the element type of the parameter array.</span></span> <span data-ttu-id="479b6-837">V tomto případě vyvolání vytvoří instanci typu pole parametru s délkou odpovídající počtu argumentů, inicializuje prvky instance pole pomocí daných hodnot argumentů a použije nově vytvořenou instanci pole jako aktuální. Argument.</span><span class="sxs-lookup"><span data-stu-id="479b6-837">In this case, the invocation creates an instance of the parameter array type with a length corresponding to the number of arguments, initializes the elements of the array instance with the given argument values, and uses the newly created array instance as the actual argument.</span></span>

<span data-ttu-id="479b6-838">S výjimkou povolení proměnlivého počtu argumentů ve volání je pole parametru přesně ekvivalentní parametrům hodnot (parametrům[hodnot](classes.md#value-parameters)) stejného typu.</span><span class="sxs-lookup"><span data-stu-id="479b6-838">Except for allowing a variable number of arguments in an invocation, a parameter array is precisely equivalent to a value parameter ([Value parameters](classes.md#value-parameters)) of the same type.</span></span>

<span data-ttu-id="479b6-839">Příklad</span><span class="sxs-lookup"><span data-stu-id="479b6-839">The example</span></span>
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
<span data-ttu-id="479b6-840">Vytvoří výstup</span><span class="sxs-lookup"><span data-stu-id="479b6-840">produces the output</span></span>
```
Array contains 3 elements: 1 2 3
Array contains 4 elements: 10 20 30 40
Array contains 0 elements:
```

<span data-ttu-id="479b6-841">První vyvolání `F` jednoduše předává pole `a` jako parametr hodnoty.</span><span class="sxs-lookup"><span data-stu-id="479b6-841">The first invocation of `F` simply passes the array `a` as a value parameter.</span></span> <span data-ttu-id="479b6-842">Druhé vyvolání `F` automaticky vytvoří čtyři prvky `int[]` s danými hodnotami elementu a předá tuto instanci pole jako parametr hodnoty.</span><span class="sxs-lookup"><span data-stu-id="479b6-842">The second invocation of `F` automatically creates a four-element `int[]` with the given element values and passes that array instance as a value parameter.</span></span> <span data-ttu-id="479b6-843">Podobně třetí vyvolání `F` vytvoří nulový element `int[]` a předá tuto instanci jako parametr hodnoty.</span><span class="sxs-lookup"><span data-stu-id="479b6-843">Likewise, the third invocation of `F` creates a zero-element `int[]` and passes that instance as a value parameter.</span></span> <span data-ttu-id="479b6-844">Druhé a třetí volání jsou přesně ekvivalentní zápisu:</span><span class="sxs-lookup"><span data-stu-id="479b6-844">The second and third invocations are precisely equivalent to writing:</span></span>
```csharp
F(new int[] {10, 20, 30, 40});
F(new int[] {});
```

<span data-ttu-id="479b6-845">Při provádění řešení přetížení může být metoda s polem parametrů platná buď v normálním tvaru, nebo v rozbaleném formátu ([příslušný člen funkce](expressions.md#applicable-function-member)).</span><span class="sxs-lookup"><span data-stu-id="479b6-845">When performing overload resolution, a method with a parameter array may be applicable either in its normal form or in its expanded form ([Applicable function member](expressions.md#applicable-function-member)).</span></span> <span data-ttu-id="479b6-846">Rozbalená forma metody je k dispozici pouze v případě, že normální forma metody není platná a pouze pokud příslušná metoda se stejným podpisem jako rozšířený formulář již není deklarována ve stejném typu.</span><span class="sxs-lookup"><span data-stu-id="479b6-846">The expanded form of a method is available only if the normal form of the method is not applicable and only if an applicable method with the same signature as the expanded form is not already declared in the same type.</span></span>

<span data-ttu-id="479b6-847">Příklad</span><span class="sxs-lookup"><span data-stu-id="479b6-847">The example</span></span>
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
<span data-ttu-id="479b6-848">Vytvoří výstup</span><span class="sxs-lookup"><span data-stu-id="479b6-848">produces the output</span></span>
```
F();
F(object[]);
F(object,object);
F(object[]);
F(object[]);
```

<span data-ttu-id="479b6-849">V příkladu jsou jako běžné metody již do třídy zahrnuty dvě z možných rozšířených forem metody s polem parametrů.</span><span class="sxs-lookup"><span data-stu-id="479b6-849">In the example, two of the possible expanded forms of the method with a parameter array are already included in the class as regular methods.</span></span> <span data-ttu-id="479b6-850">Tyto rozšířené formuláře se proto neberou v úvahu při provádění řešení přetížení a první a třetí metoda vyvolání tak vybere běžné metody.</span><span class="sxs-lookup"><span data-stu-id="479b6-850">These expanded forms are therefore not considered when performing overload resolution, and the first and third method invocations thus select the regular methods.</span></span> <span data-ttu-id="479b6-851">Když třída deklaruje metodu s polem parametrů, není neobvyklá a také obsahuje některé z rozšířených formulářů jako běžné metody.</span><span class="sxs-lookup"><span data-stu-id="479b6-851">When a class declares a method with a parameter array, it is not uncommon to also include some of the expanded forms as regular methods.</span></span> <span data-ttu-id="479b6-852">Díky tomu je možné se vyhnout přidělení instance pole, která nastane, když je vyvolána rozšířená forma metody s polem parametru.</span><span class="sxs-lookup"><span data-stu-id="479b6-852">By doing so it is possible to avoid the allocation of an array instance that occurs when an expanded form of a method with a parameter array is invoked.</span></span>

<span data-ttu-id="479b6-853">Když je `object[]`typ pole parametru, dojde k potenciální nejednoznačnosti mezi obvyklou formou metody a vynaloženým formulářem pro jeden `object` parametr.</span><span class="sxs-lookup"><span data-stu-id="479b6-853">When the type of a parameter array is `object[]`, a potential ambiguity arises between the normal form of the method and the expended form for a single `object` parameter.</span></span> <span data-ttu-id="479b6-854">Důvodem nejednoznačnosti je, že je sama `object[]` o sobě implicitně převoditelná na `object`typ.</span><span class="sxs-lookup"><span data-stu-id="479b6-854">The reason for the ambiguity is that an `object[]` is itself implicitly convertible to type `object`.</span></span> <span data-ttu-id="479b6-855">Nejednoznačnost nepředstavuje žádný problém, ale vzhledem k tomu, že je možné ho vyřešit vložením přetypování, pokud je to potřeba.</span><span class="sxs-lookup"><span data-stu-id="479b6-855">The ambiguity presents no problem, however, since it can be resolved by inserting a cast if needed.</span></span>

<span data-ttu-id="479b6-856">Příklad</span><span class="sxs-lookup"><span data-stu-id="479b6-856">The example</span></span>
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
<span data-ttu-id="479b6-857">Vytvoří výstup</span><span class="sxs-lookup"><span data-stu-id="479b6-857">produces the output</span></span>
```
System.Int32 System.String System.Double
System.Object[]
System.Object[]
System.Int32 System.String System.Double
```

<span data-ttu-id="479b6-858">V prvním a posledním volání aplikace `F`je normální forma pro `F` platná, protože implicitní převod existuje z typu argumentu na typ parametru (oba jsou typu `object[]`).</span><span class="sxs-lookup"><span data-stu-id="479b6-858">In the first and last invocations of `F`, the normal form of `F` is applicable because an implicit conversion exists from the argument type to the parameter type (both are of type `object[]`).</span></span> <span data-ttu-id="479b6-859">Proto rozlišení přetížení vybírá normální formu `F`a argument je předán jako parametr regulární hodnoty.</span><span class="sxs-lookup"><span data-stu-id="479b6-859">Thus, overload resolution selects the normal form of `F`, and the argument is passed as a regular value parameter.</span></span> <span data-ttu-id="479b6-860">V druhém a třetím vyvolání není platná normální forma `F` , protože z typu argumentu neexistuje implicitní převod na typ parametru (typ `object` nelze implicitně převést na typ `object[]`).</span><span class="sxs-lookup"><span data-stu-id="479b6-860">In the second and third invocations, the normal form of `F` is not applicable because no implicit conversion exists from the argument type to the parameter type (type `object` cannot be implicitly converted to type `object[]`).</span></span> <span data-ttu-id="479b6-861">Nicméně rozbalená forma pro `F` je, takže je vybrána při řešení přetížení.</span><span class="sxs-lookup"><span data-stu-id="479b6-861">However, the expanded form of `F` is applicable, so it is selected by overload resolution.</span></span> <span data-ttu-id="479b6-862">Výsledkem je, že vyvolání jednoho prvku `object[]` je vytvořeno voláním a jeden prvek pole je inicializován s danou hodnotou argumentu (což je sám odkaz `object[]`na).</span><span class="sxs-lookup"><span data-stu-id="479b6-862">As a result, a one-element `object[]` is created by the invocation, and the single element of the array is initialized with the given argument value (which itself is a reference to an `object[]`).</span></span>

### <a name="static-and-instance-methods"></a><span data-ttu-id="479b6-863">Statické a instanční metody</span><span class="sxs-lookup"><span data-stu-id="479b6-863">Static and instance methods</span></span>

<span data-ttu-id="479b6-864">Pokud deklarace metody obsahuje `static` modifikátor, tato metoda je označována jako statická metoda.</span><span class="sxs-lookup"><span data-stu-id="479b6-864">When a method declaration includes a `static` modifier, that method is said to be a static method.</span></span> <span data-ttu-id="479b6-865">Pokud není `static` k dispozici žádný modifikátor, metoda je označována jako metoda instance.</span><span class="sxs-lookup"><span data-stu-id="479b6-865">When no `static` modifier is present, the method is said to be an instance method.</span></span>

<span data-ttu-id="479b6-866">Statická metoda nepracuje na konkrétní instanci a jedná se o chybu při kompilaci, na kterou `this` se odkazuje ve statické metodě.</span><span class="sxs-lookup"><span data-stu-id="479b6-866">A static method does not operate on a specific instance, and it is a compile-time error to refer to `this` in a static method.</span></span>

<span data-ttu-id="479b6-867">Metoda instance pracuje na dané instanci třídy a k této instanci může přistupovat jako `this` ([Tento přístup](expressions.md#this-access)).</span><span class="sxs-lookup"><span data-stu-id="479b6-867">An instance method operates on a given instance of a class, and that instance can be accessed as `this` ([This access](expressions.md#this-access)).</span></span>

<span data-ttu-id="479b6-868">Pokud je metoda odkazována v *member_access* ([členský přístup](expressions.md#member-access) `E.M`) formuláře, pokud `M` je statická metoda, `E` musí poznamenat typ obsahující `M`a pokud `M` je metoda instance, musí se poznamenat instance typu, který obsahuje `M`. `E`</span><span class="sxs-lookup"><span data-stu-id="479b6-868">When a method is referenced in a *member_access* ([Member access](expressions.md#member-access)) of the form `E.M`, if `M` is a static method, `E` must denote a type containing `M`, and if `M` is an instance method, `E` must denote an instance of a type containing `M`.</span></span>

<span data-ttu-id="479b6-869">Rozdíly mezi členy static a instance jsou podrobněji popsány ve [statických a instancích členů](classes.md#static-and-instance-members).</span><span class="sxs-lookup"><span data-stu-id="479b6-869">The differences between static and instance members are discussed further in [Static and instance members](classes.md#static-and-instance-members).</span></span>

### <a name="virtual-methods"></a><span data-ttu-id="479b6-870">Virtuální metody</span><span class="sxs-lookup"><span data-stu-id="479b6-870">Virtual methods</span></span>

<span data-ttu-id="479b6-871">Pokud deklarace metody instance obsahuje `virtual` modifikátor, tato metoda je označována jako virtuální metoda.</span><span class="sxs-lookup"><span data-stu-id="479b6-871">When an instance method declaration includes a `virtual` modifier, that method is said to be a virtual method.</span></span> <span data-ttu-id="479b6-872">Pokud není `virtual` k dispozici žádný modifikátor, metoda je označována jako nevirtuální metoda.</span><span class="sxs-lookup"><span data-stu-id="479b6-872">When no `virtual` modifier is present, the method is said to be a non-virtual method.</span></span>

<span data-ttu-id="479b6-873">Implementace nevirtuální metody je invariantní: Implementace je stejná, bez ohledu na to, zda je metoda vyvolána na instanci třídy, ve které je deklarována, nebo instanci odvozené třídy.</span><span class="sxs-lookup"><span data-stu-id="479b6-873">The implementation of a non-virtual method is invariant: The implementation is the same whether the method is invoked on an instance of the class in which it is declared or an instance of a derived class.</span></span> <span data-ttu-id="479b6-874">Naproti tomu implementace virtuální metody může být nahrazena odvozenými třídami.</span><span class="sxs-lookup"><span data-stu-id="479b6-874">In contrast, the implementation of a virtual method can be superseded by derived classes.</span></span> <span data-ttu-id="479b6-875">Proces nahrazení implementace zděděné virtuální metody je označován jako ***přepsání*** této metody ([metody přepisu](classes.md#override-methods)).</span><span class="sxs-lookup"><span data-stu-id="479b6-875">The process of superseding the implementation of an inherited virtual method is known as ***overriding*** that method ([Override methods](classes.md#override-methods)).</span></span>

<span data-ttu-id="479b6-876">V volání virtuální metody určuje ***Typ spuštění*** instance, pro kterou se volání provádí, způsob, jakým je implementace samotné metody vyvolána.</span><span class="sxs-lookup"><span data-stu-id="479b6-876">In a virtual method invocation, the ***run-time type*** of the instance for which that invocation takes place determines the actual method implementation to invoke.</span></span> <span data-ttu-id="479b6-877">V nevirtuálním volání metody je ***Typ doby kompilace*** instance určujícím faktorem.</span><span class="sxs-lookup"><span data-stu-id="479b6-877">In a non-virtual method invocation, the ***compile-time type*** of the instance is the determining factor.</span></span> <span data-ttu-id="479b6-878">V přesném termínu, když je `N` vyvolána metoda s názvem seznamem `A` argumentů v instanci s typem `C` pro dobu kompilace a typem `R` za běhu (kde `R` je buď `C` nebo odvozenou třídou z `C`) se volání zpracovává takto:</span><span class="sxs-lookup"><span data-stu-id="479b6-878">In precise terms, when a method named `N` is invoked with an argument list `A` on an instance with a compile-time type `C` and a run-time type `R` (where `R` is either `C` or a class derived from `C`), the invocation is processed as follows:</span></span>

*  <span data-ttu-id="479b6-879">Nejprve je rozlišení přetěžování použito pro `C`, `N`a `A`, pro výběr konkrétní metody `M` ze sady metod deklarovaných a zděděných pomocí `C`.</span><span class="sxs-lookup"><span data-stu-id="479b6-879">First, overload resolution is applied to `C`, `N`, and `A`, to select a specific method `M` from the set of methods declared in and inherited by `C`.</span></span> <span data-ttu-id="479b6-880">Tento postup je popsaný v tématu [vyvolání metod](expressions.md#method-invocations).</span><span class="sxs-lookup"><span data-stu-id="479b6-880">This is described in [Method invocations](expressions.md#method-invocations).</span></span>
*  <span data-ttu-id="479b6-881">V případě `M` , že je metoda jiná než virtuální, `M` je vyvolána.</span><span class="sxs-lookup"><span data-stu-id="479b6-881">Then, if `M` is a non-virtual method, `M` is invoked.</span></span>
*  <span data-ttu-id="479b6-882">V opačném případě `M` `R` je virtuální metoda a je vyvolána největší odvozená implementace s ohledem na. `M`</span><span class="sxs-lookup"><span data-stu-id="479b6-882">Otherwise, `M` is a virtual method, and the most derived implementation of `M` with respect to `R` is invoked.</span></span>

<span data-ttu-id="479b6-883">Pro každou virtuální metodu deklarovanou nebo zděděnou třídou existuje ***největší odvozená implementace*** metody s ohledem na tuto třídu.</span><span class="sxs-lookup"><span data-stu-id="479b6-883">For every virtual method declared in or inherited by a class, there exists a ***most derived implementation*** of the method with respect to that class.</span></span> <span data-ttu-id="479b6-884">Největší odvozená implementace virtuální metody `M` s ohledem na třídu `R` je určena následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="479b6-884">The most derived implementation of a virtual method `M` with respect to a class `R` is determined as follows:</span></span>

*  <span data-ttu-id="479b6-885">Pokud `R` obsahuje úvodní `virtual` deklaraci `M`, `M`pak toto je největší odvozená implementace.</span><span class="sxs-lookup"><span data-stu-id="479b6-885">If `R` contains the introducing `virtual` declaration of `M`, then this is the most derived implementation of `M`.</span></span>
*  <span data-ttu-id="479b6-886">V opačném `R` případě, `override` Pokud `M`obsahuje, je toto největší odvozená implementace `M`.</span><span class="sxs-lookup"><span data-stu-id="479b6-886">Otherwise, if `R` contains an `override` of `M`, then this is the most derived implementation of `M`.</span></span>
*  <span data-ttu-id="479b6-887">V opačném případě je nejvíce odvozená implementace `M` s ohledem na `R` stejná jako největší odvozená implementace `M` `R`s ohledem na přímou základní třídu.</span><span class="sxs-lookup"><span data-stu-id="479b6-887">Otherwise, the most derived implementation of `M` with respect to `R` is the same as the most derived implementation of `M` with respect to the direct base class of `R`.</span></span>

<span data-ttu-id="479b6-888">Následující příklad znázorňuje rozdíly mezi virtuálními a nevirtuálními metodami:</span><span class="sxs-lookup"><span data-stu-id="479b6-888">The following example illustrates the differences between virtual and non-virtual methods:</span></span>
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

<span data-ttu-id="479b6-889">V příkladu `A` představuje `F` nevirtuální a virtuální metodu `G`.</span><span class="sxs-lookup"><span data-stu-id="479b6-889">In the example, `A` introduces a non-virtual method `F` and a virtual method `G`.</span></span> <span data-ttu-id="479b6-890">Třída `B` zavádí novou nevirtuální metodu `F`, takže Skryje zděděné `F`a také přepíše zděděnou metodu `G`.</span><span class="sxs-lookup"><span data-stu-id="479b6-890">The class `B` introduces a new non-virtual method `F`, thus hiding the inherited `F`, and also overrides the inherited method `G`.</span></span> <span data-ttu-id="479b6-891">Příklad vytvoří výstup:</span><span class="sxs-lookup"><span data-stu-id="479b6-891">The example produces the output:</span></span>
```
A.F
B.F
B.G
B.G
```

<span data-ttu-id="479b6-892">Všimněte si, že `a.G()` příkaz `B.G`vyvolá, nikoli `A.G`.</span><span class="sxs-lookup"><span data-stu-id="479b6-892">Notice that the statement `a.G()` invokes `B.G`, not `A.G`.</span></span> <span data-ttu-id="479b6-893">Důvodem je, že typ modulu runtime instance (což je `B`), nikoli typ doby kompilace instance (což je `A`), Určuje skutečnou implementaci metody, která má být vyvolána.</span><span class="sxs-lookup"><span data-stu-id="479b6-893">This is because the run-time type of the instance (which is `B`), not the compile-time type of the instance (which is `A`), determines the actual method implementation to invoke.</span></span>

<span data-ttu-id="479b6-894">Vzhledem k tomu, že metody mohou skrýt zděděné metody, je možné, že třída bude obsahovat několik virtuálních metod se stejnou signaturou.</span><span class="sxs-lookup"><span data-stu-id="479b6-894">Because methods are allowed to hide inherited methods, it is possible for a class to contain several virtual methods with the same signature.</span></span> <span data-ttu-id="479b6-895">To nepředstavuje problém, protože všechny kromě nejvíce odvozené metody jsou skryté.</span><span class="sxs-lookup"><span data-stu-id="479b6-895">This does not present an ambiguity problem, since all but the most derived method are hidden.</span></span> <span data-ttu-id="479b6-896">V příkladu</span><span class="sxs-lookup"><span data-stu-id="479b6-896">In the example</span></span>
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
<span data-ttu-id="479b6-897">třídy `C` a`D` obsahují dvě virtuální metody se stejnou signaturou: Ten, který představil `A` , a ten, který `C`představil.</span><span class="sxs-lookup"><span data-stu-id="479b6-897">the `C` and `D` classes contain two virtual methods with the same signature: The one introduced by `A` and the one introduced by `C`.</span></span> <span data-ttu-id="479b6-898">Metoda, kterou zavedla `C` , skrývá metodu děděnou z. `A`</span><span class="sxs-lookup"><span data-stu-id="479b6-898">The method introduced by `C` hides the method inherited from `A`.</span></span> <span data-ttu-id="479b6-899">Proto `D` deklarace override v přepisuje metodu `C`, kterou zavádí, a není možné `D` přepsat metodu zavedenou pomocí `A`.</span><span class="sxs-lookup"><span data-stu-id="479b6-899">Thus, the override declaration in `D` overrides the method introduced by `C`, and it is not possible for `D` to override the method introduced by `A`.</span></span> <span data-ttu-id="479b6-900">Příklad vytvoří výstup:</span><span class="sxs-lookup"><span data-stu-id="479b6-900">The example produces the output:</span></span>
```
B.F
B.F
D.F
D.F
```

<span data-ttu-id="479b6-901">Všimněte si, že je možné vyvolat skrytou virtuální metodu přístupem k instanci `D` prostřednictvím méně odvozeného typu, ve kterém není metoda skrytá.</span><span class="sxs-lookup"><span data-stu-id="479b6-901">Note that it is possible to invoke the hidden virtual method by accessing an instance of `D` through a less derived type in which the method is not hidden.</span></span>

### <a name="override-methods"></a><span data-ttu-id="479b6-902">Metody přepsání</span><span class="sxs-lookup"><span data-stu-id="479b6-902">Override methods</span></span>

<span data-ttu-id="479b6-903">Pokud deklarace metody instance obsahuje `override` modifikátor, metoda je označována jako ***Metoda přepsání***.</span><span class="sxs-lookup"><span data-stu-id="479b6-903">When an instance method declaration includes an `override` modifier, the method is said to be an ***override method***.</span></span> <span data-ttu-id="479b6-904">Metoda override přepíše zděděnou virtuální metodu se stejnou signaturou.</span><span class="sxs-lookup"><span data-stu-id="479b6-904">An override method overrides an inherited virtual method with the same signature.</span></span> <span data-ttu-id="479b6-905">Zatímco deklarace virtuální metody zavádí novou metodu, deklarace metody přepsání specializuje existující zděděnou virtuální metodu tím, že poskytuje novou implementaci této metody.</span><span class="sxs-lookup"><span data-stu-id="479b6-905">Whereas a virtual method declaration introduces a new method, an override method declaration specializes an existing inherited virtual method by providing a new implementation of that method.</span></span>

<span data-ttu-id="479b6-906">Metoda přepsaná `override` deklarací je označována jako ***přepsaná základní metoda***.</span><span class="sxs-lookup"><span data-stu-id="479b6-906">The method overridden by an `override` declaration is known as the ***overridden base method***.</span></span> <span data-ttu-id="479b6-907">Pro metodu `M` přepsání deklarovanou ve třídě `C`je určena přepsaná základní metoda prozkoumáním `C`každého typu základní třídy, `C` počínaje přímým typem základní třídy a pokračováním každého po sobě. typ přímé základní třídy, dokud v daném typu základní třídy, je umístěna alespoň jedna přístupná metoda, která má stejný podpis jako `M` po nahrazení argumentů typu.</span><span class="sxs-lookup"><span data-stu-id="479b6-907">For an override method `M` declared in a class `C`, the overridden base method is determined by examining each base class type of `C`, starting with the direct base class type of `C` and continuing with each successive direct base class type, until in a given base class type at least one accessible method is located which has the same signature as `M` after substitution of type arguments.</span></span> <span data-ttu-id="479b6-908">Pro účely nalezení přepsané základní metody `public`je metoda považována za dostupnou `protected` `protected internal`, pokud je, pokud je, pokud je, pokud je, nebo pokud `internal` je, a deklarována ve stejném programu jako `C`.</span><span class="sxs-lookup"><span data-stu-id="479b6-908">For the purposes of locating the overridden base method, a method is considered accessible if it is `public`, if it is `protected`, if it is `protected internal`, or if it is `internal` and declared in the same program as `C`.</span></span>

<span data-ttu-id="479b6-909">Dojde k chybě při kompilaci, pokud pro deklaraci přepsání nejsou splněné všechny následující podmínky:</span><span class="sxs-lookup"><span data-stu-id="479b6-909">A compile-time error occurs unless all of the following are true for an override declaration:</span></span>

*  <span data-ttu-id="479b6-910">Přepsaná základní metoda může být umístěna, jak je popsáno výše.</span><span class="sxs-lookup"><span data-stu-id="479b6-910">An overridden base method can be located as described above.</span></span>
*  <span data-ttu-id="479b6-911">Právě jedna taková přepsaná základní metoda existuje.</span><span class="sxs-lookup"><span data-stu-id="479b6-911">There is exactly one such overridden base method.</span></span> <span data-ttu-id="479b6-912">Toto omezení má vliv pouze v případě, že typ základní třídy je konstruovaný typ, kde nahrazení argumentů typu provede stejnou signaturu dvou metod.</span><span class="sxs-lookup"><span data-stu-id="479b6-912">This restriction has effect only if the base class type is a constructed type where the substitution of type arguments makes the signature of two methods the same.</span></span>
*  <span data-ttu-id="479b6-913">Přepsaná základní metoda je metoda virtuální, abstraktní nebo potlačení.</span><span class="sxs-lookup"><span data-stu-id="479b6-913">The overridden base method is a virtual, abstract, or override method.</span></span> <span data-ttu-id="479b6-914">Jinými slovy, přepsaná základní metoda nemůže být statická nebo není virtuální.</span><span class="sxs-lookup"><span data-stu-id="479b6-914">In other words, the overridden base method cannot be static or non-virtual.</span></span>
*  <span data-ttu-id="479b6-915">Přepsaná základní metoda není zapečetěná metoda.</span><span class="sxs-lookup"><span data-stu-id="479b6-915">The overridden base method is not a sealed method.</span></span>
*  <span data-ttu-id="479b6-916">Metoda override a přepsaná základní metoda mají stejný návratový typ.</span><span class="sxs-lookup"><span data-stu-id="479b6-916">The override method and the overridden base method have the same return type.</span></span>
*  <span data-ttu-id="479b6-917">Deklarace přepisu a přepsaná základní metoda mají stejné deklarované přístupnosti.</span><span class="sxs-lookup"><span data-stu-id="479b6-917">The override declaration and the overridden base method have the same declared accessibility.</span></span> <span data-ttu-id="479b6-918">Jinými slovy deklarace přepisu nemůže změnit přístupnost virtuální metody.</span><span class="sxs-lookup"><span data-stu-id="479b6-918">In other words, an override declaration cannot change the accessibility of the virtual method.</span></span> <span data-ttu-id="479b6-919">Nicméně, pokud je přepsaná základní metoda chráněna jako interní a je deklarována v jiném sestavení než sestavení obsahující metodu přepsání, musí být pro metodu override deklarovaná přístupnost chráněná.</span><span class="sxs-lookup"><span data-stu-id="479b6-919">However, if the overridden base method is protected internal and it is declared in a different assembly than the assembly containing the override method then the override method's declared accessibility must be protected.</span></span>
*  <span data-ttu-id="479b6-920">Deklarace override neurčuje Type-Parameter-Constraint-klauzule.</span><span class="sxs-lookup"><span data-stu-id="479b6-920">The override declaration does not specify type-parameter-constraints-clauses.</span></span> <span data-ttu-id="479b6-921">Místo toho jsou omezení děděna z přepsané základní metody.</span><span class="sxs-lookup"><span data-stu-id="479b6-921">Instead the constraints are inherited from the overridden base method.</span></span> <span data-ttu-id="479b6-922">Všimněte si, že omezení, která jsou parametry typu v přepsané metodě, mohou být nahrazena argumenty typu ve zděděném omezení.</span><span class="sxs-lookup"><span data-stu-id="479b6-922">Note that constraints that are type parameters in the overridden method may be replaced by type arguments in the inherited constraint.</span></span> <span data-ttu-id="479b6-923">To může vést k omezením, která nejsou platná v případě explicitního určení, jako jsou například typy hodnot nebo zapečetěné typy.</span><span class="sxs-lookup"><span data-stu-id="479b6-923">This can lead to constraints that are not legal when explicitly specified, such as value types or sealed types.</span></span>

<span data-ttu-id="479b6-924">Následující příklad ukazuje, jak přepisování pravidel funguje pro obecné třídy:</span><span class="sxs-lookup"><span data-stu-id="479b6-924">The following example demonstrates how the overriding rules work for generic classes:</span></span>
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

<span data-ttu-id="479b6-925">Deklarace přepsání má přístup k přepsané základní metodě pomocí *base_access* ([základní přístup](expressions.md#base-access)).</span><span class="sxs-lookup"><span data-stu-id="479b6-925">An override declaration can access the overridden base method using a *base_access* ([Base access](expressions.md#base-access)).</span></span> <span data-ttu-id="479b6-926">V příkladu</span><span class="sxs-lookup"><span data-stu-id="479b6-926">In the example</span></span>
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
<span data-ttu-id="479b6-927">`A`vyvolání v `B` vyvolá`PrintFields`metodudeklarovanouv. `base.PrintFields()`</span><span class="sxs-lookup"><span data-stu-id="479b6-927">the `base.PrintFields()` invocation in `B` invokes the `PrintFields` method declared in `A`.</span></span> <span data-ttu-id="479b6-928">*Base_access* zakáže mechanismus virtuálního vyvolání a jednoduše zachází se základní metodou jako nevirtuální metoda.</span><span class="sxs-lookup"><span data-stu-id="479b6-928">A *base_access* disables the virtual invocation mechanism and simply treats the base method as a non-virtual method.</span></span> <span data-ttu-id="479b6-929">Bylo vyvolání `B` volání v napsáno `((A)this).PrintFields()`, `PrintFields` by rekurzivně vyvolalo metodu deklarovanou v `B`, nikoli deklaraci, která `A`je deklarována v, protože `PrintFields` je virtuální a běhový typ .`((A)this)` je .`B`</span><span class="sxs-lookup"><span data-stu-id="479b6-929">Had the invocation in `B` been written `((A)this).PrintFields()`, it would recursively invoke the `PrintFields` method declared in `B`, not the one declared in `A`, since `PrintFields` is virtual and the run-time type of `((A)this)` is `B`.</span></span>

<span data-ttu-id="479b6-930">Pouze zahrnutím `override` modifikátoru může metoda přepsat jinou metodu.</span><span class="sxs-lookup"><span data-stu-id="479b6-930">Only by including an `override` modifier can a method override another method.</span></span> <span data-ttu-id="479b6-931">Ve všech ostatních případech metoda se stejnou signaturou jako zděděná metoda jednoduše skrývá zděděnou metodu.</span><span class="sxs-lookup"><span data-stu-id="479b6-931">In all other cases, a method with the same signature as an inherited method simply hides the inherited method.</span></span> <span data-ttu-id="479b6-932">V příkladu</span><span class="sxs-lookup"><span data-stu-id="479b6-932">In the example</span></span>
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
<span data-ttu-id="479b6-933">`F` `A`metoda v `B` nezahrnuje`override` modifikátor, proto nepřepisuje metodu v. `F`</span><span class="sxs-lookup"><span data-stu-id="479b6-933">the `F` method in `B` does not include an `override` modifier and therefore does not override the `F` method in `A`.</span></span> <span data-ttu-id="479b6-934">Místo toho `F` metoda v `B` skrývá metodu v `A`a upozornění je `new` hlášeno, protože deklarace neobsahuje modifikátor.</span><span class="sxs-lookup"><span data-stu-id="479b6-934">Rather, the `F` method in `B` hides the method in `A`, and a warning is reported because the declaration does not include a `new` modifier.</span></span>

<span data-ttu-id="479b6-935">V příkladu</span><span class="sxs-lookup"><span data-stu-id="479b6-935">In the example</span></span>
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
<span data-ttu-id="479b6-936">`F` metoda v `A`skrývá virtuální `F` metodu `B` děděnou z.</span><span class="sxs-lookup"><span data-stu-id="479b6-936">the `F` method in `B` hides the virtual `F` method inherited from `A`.</span></span> <span data-ttu-id="479b6-937">Vzhledem k tomu `F` , `B` že nový v nástroji má privátní přístup, jeho obor `B` obsahuje pouze tělo třídy a nerozšiřuje `C`jej na.</span><span class="sxs-lookup"><span data-stu-id="479b6-937">Since the new `F` in `B` has private access, its scope only includes the class body of `B` and does not extend to `C`.</span></span> <span data-ttu-id="479b6-938">Proto deklarace `F` v `C` jeoprávněna`A`přepsat zděděnéz.`F`</span><span class="sxs-lookup"><span data-stu-id="479b6-938">Therefore, the declaration of `F` in `C` is permitted to override the `F` inherited from `A`.</span></span>

### <a name="sealed-methods"></a><span data-ttu-id="479b6-939">Zapečetěné metody</span><span class="sxs-lookup"><span data-stu-id="479b6-939">Sealed methods</span></span>

<span data-ttu-id="479b6-940">Pokud deklarace metody instance obsahuje `sealed` modifikátor, tato metoda je označována jako ***zapečetěná metoda***.</span><span class="sxs-lookup"><span data-stu-id="479b6-940">When an instance method declaration includes a `sealed` modifier, that method is said to be a ***sealed method***.</span></span> <span data-ttu-id="479b6-941">Pokud deklarace metody instance obsahuje `sealed` modifikátor, musí také `override` obsahovat modifikátor.</span><span class="sxs-lookup"><span data-stu-id="479b6-941">If an instance method declaration includes the  `sealed` modifier, it must also include the `override` modifier.</span></span> <span data-ttu-id="479b6-942">`sealed` Použití modifikátoru zabrání odvození třídy z dalšího přepsání metody.</span><span class="sxs-lookup"><span data-stu-id="479b6-942">Use of the `sealed` modifier prevents a derived class from further overriding the method.</span></span>

<span data-ttu-id="479b6-943">V příkladu</span><span class="sxs-lookup"><span data-stu-id="479b6-943">In the example</span></span>
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
<span data-ttu-id="479b6-944">Třída `B` poskytuje dvě metody přepisu `F` : metoda, která `G` má `sealed` modifikátor a metodu, která není.</span><span class="sxs-lookup"><span data-stu-id="479b6-944">the class `B` provides two override methods: an `F` method that has the `sealed` modifier and a `G` method that does not.</span></span> <span data-ttu-id="479b6-945">`B`použití zapečetění `modifier` zabrání `C` dalšímu přepsání `F`.</span><span class="sxs-lookup"><span data-stu-id="479b6-945">`B`'s use of the sealed `modifier` prevents `C` from further overriding `F`.</span></span>

### <a name="abstract-methods"></a><span data-ttu-id="479b6-946">Abstraktní metody</span><span class="sxs-lookup"><span data-stu-id="479b6-946">Abstract methods</span></span>

<span data-ttu-id="479b6-947">Pokud deklarace metody instance obsahuje `abstract` modifikátor, tato metoda je označována jako ***abstraktní metoda***.</span><span class="sxs-lookup"><span data-stu-id="479b6-947">When an instance method declaration includes an `abstract` modifier, that method is said to be an ***abstract method***.</span></span> <span data-ttu-id="479b6-948">I když je abstraktní metoda implicitně také virtuální metodou, nemůže mít modifikátor `virtual`.</span><span class="sxs-lookup"><span data-stu-id="479b6-948">Although an abstract method is implicitly also a virtual method, it cannot have the modifier `virtual`.</span></span>

<span data-ttu-id="479b6-949">Deklarace abstraktní metody zavádí novou virtuální metodu, ale neposkytuje implementaci této metody.</span><span class="sxs-lookup"><span data-stu-id="479b6-949">An abstract method declaration introduces a new virtual method but does not provide an implementation of that method.</span></span> <span data-ttu-id="479b6-950">Místo toho jsou vyžadovány neabstraktní odvozené třídy k poskytnutí vlastní implementace přepsáním této metody.</span><span class="sxs-lookup"><span data-stu-id="479b6-950">Instead, non-abstract derived classes are required to provide their own implementation by overriding that method.</span></span> <span data-ttu-id="479b6-951">Vzhledem k tomu, že abstraktní metoda neposkytuje žádnou skutečnou implementaci, *method_body* abstraktní metody se jednoduše skládá ze střední hodnoty.</span><span class="sxs-lookup"><span data-stu-id="479b6-951">Because an abstract method provides no actual implementation, the *method_body* of an abstract method simply consists of a semicolon.</span></span>

<span data-ttu-id="479b6-952">Deklarace abstraktní metody jsou povoleny pouze v abstraktních třídách ([abstraktní třídy](classes.md#abstract-classes)).</span><span class="sxs-lookup"><span data-stu-id="479b6-952">Abstract method declarations are only permitted in abstract classes ([Abstract classes](classes.md#abstract-classes)).</span></span>

<span data-ttu-id="479b6-953">V příkladu</span><span class="sxs-lookup"><span data-stu-id="479b6-953">In the example</span></span>
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
<span data-ttu-id="479b6-954">`Shape` třída definuje abstraktní pojem objektu geometrického tvaru, který může malovat sám.</span><span class="sxs-lookup"><span data-stu-id="479b6-954">the `Shape` class defines the abstract notion of a geometrical shape object that can paint itself.</span></span> <span data-ttu-id="479b6-955">Metoda `Paint` je abstraktní, protože není k dispozici žádná smysluplná výchozí implementace.</span><span class="sxs-lookup"><span data-stu-id="479b6-955">The `Paint` method is abstract because there is no meaningful default implementation.</span></span> <span data-ttu-id="479b6-956">Třídy `Ellipse` a `Box` jsou konkrétní`Shape` implementace.</span><span class="sxs-lookup"><span data-stu-id="479b6-956">The `Ellipse` and `Box` classes are concrete `Shape` implementations.</span></span> <span data-ttu-id="479b6-957">Vzhledem k tomu, že tyto třídy jsou neabstraktní, jsou vyžadovány `Paint` pro přepsání metody a poskytnutí skutečné implementace.</span><span class="sxs-lookup"><span data-stu-id="479b6-957">Because these classes are non-abstract, they are required to override the `Paint` method and provide an actual implementation.</span></span>

<span data-ttu-id="479b6-958">Jedná se o chybu při kompilaci pro *base_access* ([základní přístup](expressions.md#base-access)) pro odkazování na abstraktní metodu.</span><span class="sxs-lookup"><span data-stu-id="479b6-958">It is a compile-time error for a *base_access* ([Base access](expressions.md#base-access)) to reference an abstract method.</span></span> <span data-ttu-id="479b6-959">V příkladu</span><span class="sxs-lookup"><span data-stu-id="479b6-959">In the example</span></span>
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
<span data-ttu-id="479b6-960">pro `base.F()` vyvolání je hlášena chyba při kompilaci, protože odkazuje na abstraktní metodu.</span><span class="sxs-lookup"><span data-stu-id="479b6-960">a compile-time error is reported for the `base.F()` invocation because it references an abstract method.</span></span>

<span data-ttu-id="479b6-961">Deklarace abstraktní metody je povolena pro přepsání virtuální metody.</span><span class="sxs-lookup"><span data-stu-id="479b6-961">An abstract method declaration is permitted to override a virtual method.</span></span> <span data-ttu-id="479b6-962">To umožňuje abstraktní třídě vynutit znovu implementaci metody v odvozených třídách a provede původní implementaci metody jako nedostupné.</span><span class="sxs-lookup"><span data-stu-id="479b6-962">This allows an abstract class to force re-implementation of the method in derived classes, and makes the original implementation of the method unavailable.</span></span> <span data-ttu-id="479b6-963">V příkladu</span><span class="sxs-lookup"><span data-stu-id="479b6-963">In the example</span></span>
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
<span data-ttu-id="479b6-964">Třída `A` deklaruje virtuální metodu, třída `B` Přepisuje tuto metodu abstraktní metodou a třída `C` Přepisuje abstraktní metodu k poskytnutí vlastní implementace.</span><span class="sxs-lookup"><span data-stu-id="479b6-964">class `A` declares a virtual method, class `B` overrides this method with an abstract method, and class `C` overrides the abstract method to provide its own implementation.</span></span>

### <a name="external-methods"></a><span data-ttu-id="479b6-965">Externí metody</span><span class="sxs-lookup"><span data-stu-id="479b6-965">External methods</span></span>

<span data-ttu-id="479b6-966">Pokud deklarace metody obsahuje `extern` modifikátor, tato metoda je označována jako ***externí metoda***.</span><span class="sxs-lookup"><span data-stu-id="479b6-966">When a method declaration includes an `extern` modifier, that method is said to be an ***external method***.</span></span> <span data-ttu-id="479b6-967">Externí metody jsou implementovány externě, obvykle v jiném jazyce než C#.</span><span class="sxs-lookup"><span data-stu-id="479b6-967">External methods are implemented externally, typically using a language other than C#.</span></span> <span data-ttu-id="479b6-968">Vzhledem k tomu, že deklarace externí metody neposkytuje žádnou skutečnou implementaci, *method_body* vnější metody se skládá středníkem.</span><span class="sxs-lookup"><span data-stu-id="479b6-968">Because an external method declaration provides no actual implementation, the *method_body* of an external method simply consists of a semicolon.</span></span> <span data-ttu-id="479b6-969">Externí metoda nemůže být obecná.</span><span class="sxs-lookup"><span data-stu-id="479b6-969">An external method may not be generic.</span></span>

<span data-ttu-id="479b6-970">Modifikátor se obvykle používá ve spojení `DllImport` s atributem ([součinností s komponentami com a Win32](attributes.md#interoperation-with-com-and-win32-components)), což umožňuje implementovat externí metody pomocí knihoven DLL (knihovny dynamického propojení). `extern`</span><span class="sxs-lookup"><span data-stu-id="479b6-970">The `extern` modifier is typically used in conjunction with a `DllImport` attribute ([Interoperation with COM and Win32 components](attributes.md#interoperation-with-com-and-win32-components)), allowing external methods to be implemented by DLLs (Dynamic Link Libraries).</span></span> <span data-ttu-id="479b6-971">Spouštěcí prostředí může podporovat jiné mechanismy, při kterých lze zadat implementace externích metod.</span><span class="sxs-lookup"><span data-stu-id="479b6-971">The execution environment may support other mechanisms whereby implementations of external methods can be provided.</span></span>

<span data-ttu-id="479b6-972">Pokud externí metoda obsahuje `DllImport` atribut, deklarace metody musí také `static` obsahovat modifikátor.</span><span class="sxs-lookup"><span data-stu-id="479b6-972">When an external method includes a `DllImport` attribute, the method declaration must also include a `static` modifier.</span></span> <span data-ttu-id="479b6-973">Tento příklad ukazuje použití `extern` modifikátoru `DllImport` a atributu:</span><span class="sxs-lookup"><span data-stu-id="479b6-973">This example demonstrates the use of the `extern` modifier and the `DllImport` attribute:</span></span>
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

### <a name="partial-methods-recap"></a><span data-ttu-id="479b6-974">Částečné metody (rekapitulace)</span><span class="sxs-lookup"><span data-stu-id="479b6-974">Partial methods (recap)</span></span>

<span data-ttu-id="479b6-975">Pokud deklarace metody obsahuje `partial` modifikátor, tato metoda je označována jako ***částečná metoda***.</span><span class="sxs-lookup"><span data-stu-id="479b6-975">When a method declaration includes a `partial` modifier, that method is said to be a ***partial method***.</span></span> <span data-ttu-id="479b6-976">Částečné metody lze deklarovat pouze jako členy částečných typů ([částečné typy](classes.md#partial-types)) a jsou předmětem řady omezení.</span><span class="sxs-lookup"><span data-stu-id="479b6-976">Partial methods can only be declared as members of partial types ([Partial types](classes.md#partial-types)), and are subject to a number of restrictions.</span></span> <span data-ttu-id="479b6-977">Částečné metody jsou podrobněji popsány v [části částečné metody](classes.md#partial-methods).</span><span class="sxs-lookup"><span data-stu-id="479b6-977">Partial methods are further described in [Partial methods](classes.md#partial-methods).</span></span>

### <a name="extension-methods"></a><span data-ttu-id="479b6-978">Metody rozšíření</span><span class="sxs-lookup"><span data-stu-id="479b6-978">Extension methods</span></span>

<span data-ttu-id="479b6-979">Pokud první parametr metody obsahuje `this` modifikátor, tato metoda je označována jako ***metoda rozšíření***.</span><span class="sxs-lookup"><span data-stu-id="479b6-979">When the first parameter of a method includes the `this` modifier, that method is said to be an ***extension method***.</span></span> <span data-ttu-id="479b6-980">Metody rozšíření lze deklarovat pouze v neobecných statických třídách, které nejsou vnořené.</span><span class="sxs-lookup"><span data-stu-id="479b6-980">Extension methods can only be declared in non-generic, non-nested static classes.</span></span> <span data-ttu-id="479b6-981">První parametr metody rozšíření nemůže mít modifikátory jiné než `this`a typ parametru nemůže být typu ukazatel.</span><span class="sxs-lookup"><span data-stu-id="479b6-981">The first parameter of an extension method can have no modifiers other than `this`, and the parameter type cannot be a pointer type.</span></span>

<span data-ttu-id="479b6-982">Následuje příklad statické třídy, která deklaruje dvě metody rozšíření:</span><span class="sxs-lookup"><span data-stu-id="479b6-982">The following is an example of a static class that declares two extension methods:</span></span>
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

<span data-ttu-id="479b6-983">Metoda rozšíření je běžnou statickou metodou.</span><span class="sxs-lookup"><span data-stu-id="479b6-983">An extension method is a regular static method.</span></span> <span data-ttu-id="479b6-984">Kromě toho, kde je jeho ohraničující statická třída v oboru, lze metodu rozšíření vyvolat pomocí syntaxe volání metody instance ([vyvolání metody rozšíření](expressions.md#extension-method-invocations)) pomocí výrazu příjemce jako prvního argumentu.</span><span class="sxs-lookup"><span data-stu-id="479b6-984">In addition, where its enclosing static class is in scope, an extension method can be invoked using instance method invocation syntax ([Extension method invocations](expressions.md#extension-method-invocations)), using the receiver expression as the first argument.</span></span>

<span data-ttu-id="479b6-985">Následující program používá metody rozšíření deklarované výše:</span><span class="sxs-lookup"><span data-stu-id="479b6-985">The following program uses the extension methods declared above:</span></span>
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

<span data-ttu-id="479b6-986">Metoda je k dispozici `string[]`v `ToInt32` a metoda je k dispozici na `string`, protože byla deklarována jako metody rozšíření. `Slice`</span><span class="sxs-lookup"><span data-stu-id="479b6-986">The `Slice` method is available on the `string[]`, and the `ToInt32` method is available on `string`, because they have been declared as extension methods.</span></span> <span data-ttu-id="479b6-987">Význam tohoto programu je stejný jako následující při použití běžných volání statických metod:</span><span class="sxs-lookup"><span data-stu-id="479b6-987">The meaning of the program is the same as the following, using ordinary static method calls:</span></span>
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

### <a name="method-body"></a><span data-ttu-id="479b6-988">Tělo metody</span><span class="sxs-lookup"><span data-stu-id="479b6-988">Method body</span></span>

<span data-ttu-id="479b6-989">*Method_body* deklarace metody se skládá buď z těla bloku, textu výrazu, nebo od středníku.</span><span class="sxs-lookup"><span data-stu-id="479b6-989">The *method_body* of a method declaration consists of either a block body, an expression body or a semicolon.</span></span>

<span data-ttu-id="479b6-990">***Výsledný typ*** metody je `void` , pokud je `void`návratový typ, nebo pokud je metoda asynchronní a návratový typ je `System.Threading.Tasks.Task`.</span><span class="sxs-lookup"><span data-stu-id="479b6-990">The ***result type*** of a method is `void` if the return type is `void`, or if the method is async and the return type is `System.Threading.Tasks.Task`.</span></span> <span data-ttu-id="479b6-991">V opačném případě typ výsledku neasynchronní metody je jeho návratový typ a výsledný typ asynchronní metody s návratovým typem `System.Threading.Tasks.Task<T>` je. `T`</span><span class="sxs-lookup"><span data-stu-id="479b6-991">Otherwise, the result type of a non-async method is its return type, and the result type of an async method with return type `System.Threading.Tasks.Task<T>` is `T`.</span></span>

<span data-ttu-id="479b6-992">V případě, že metoda `void` má typ výsledku a tělo bloku, `return` příkazy ([příkaz return](statements.md#the-return-statement)) v bloku nepovolují zadání výrazu.</span><span class="sxs-lookup"><span data-stu-id="479b6-992">When a method has a `void` result type and a block body, `return` statements ([The return statement](statements.md#the-return-statement)) in the block are not permitted to specify an expression.</span></span> <span data-ttu-id="479b6-993">Pokud se provádění bloku metody void dokončí normálně (to znamená, že řízení projde na konci těla metody), tato metoda jednoduše vrátí na aktuálního volajícího.</span><span class="sxs-lookup"><span data-stu-id="479b6-993">If execution of the block of a void method completes normally (that is, control flows off the end of the method body), that method simply returns to its current caller.</span></span>
    
<span data-ttu-id="479b6-994">Pokud má `void` metoda výsledek a tělo výrazu, musí být výraz `E` *statement_expression*a tělo je přesně ekvivalentní tělo bloku ve formuláři `{ E; }`.</span><span class="sxs-lookup"><span data-stu-id="479b6-994">When a method has a `void` result and an expression body, the expression `E` must be a *statement_expression*, and the body is exactly equivalent to a block body of the form `{ E; }`.</span></span>
    
<span data-ttu-id="479b6-995">Pokud má metoda typ výsledku, který není typu void, a tělo bloku, každý `return` příkaz v bloku musí specifikovat výraz, který je implicitně převeden na typ výsledku.</span><span class="sxs-lookup"><span data-stu-id="479b6-995">When a method has a non-void result type and a block body, each `return` statement in the block must specify an expression that is implicitly convertible to the result type.</span></span> <span data-ttu-id="479b6-996">Koncový bod těla bloku metody vracející hodnoty nesmí být dosažitelný.</span><span class="sxs-lookup"><span data-stu-id="479b6-996">The endpoint of a block body of a value-returning method must not be reachable.</span></span> <span data-ttu-id="479b6-997">Jinými slovy, v metodě vracející hodnoty s tělo bloku není ovládací prvek povolen tok na konec těla metody.</span><span class="sxs-lookup"><span data-stu-id="479b6-997">In other words, in a value-returning method with a block body, control is not permitted to flow off the end of the method body.</span></span>
    
<span data-ttu-id="479b6-998">Pokud má metoda typ výsledku, který není typu void, a tělo výrazu, musí být výraz implicitně převeden na výsledný typ a tělo je přesně ekvivalentní tělo bloku ve formuláři `{ return E; }`.</span><span class="sxs-lookup"><span data-stu-id="479b6-998">When a method has a non-void result type and an expression body, the expression must be implicitly convertible to the result type, and the body is exactly equivalent to a block body of the form `{ return E; }`.</span></span>
    
<span data-ttu-id="479b6-999">V příkladu</span><span class="sxs-lookup"><span data-stu-id="479b6-999">In the example</span></span>
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
<span data-ttu-id="479b6-1000">metoda vracející `F` hodnoty má za následek chybu při kompilaci, protože řízení může přesměrovat konec těla metody.</span><span class="sxs-lookup"><span data-stu-id="479b6-1000">the value-returning `F` method results in a compile-time error because control can flow off the end of the method body.</span></span> <span data-ttu-id="479b6-1001">Metody `G` a`H` jsou správné, protože všechny možné cesty provádění končí v příkazu return, který určuje návratovou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="479b6-1001">The `G` and `H` methods are correct because all possible execution paths end in a return statement that specifies a return value.</span></span> <span data-ttu-id="479b6-1002">`I` Metoda je správná, protože její tělo je ekvivalentní bloku příkazu s pouze jediným příkazem Return.</span><span class="sxs-lookup"><span data-stu-id="479b6-1002">The `I` method is correct, because its body is equivalent to a statement block with just a single return statement in it.</span></span>

### <a name="method-overloading"></a><span data-ttu-id="479b6-1003">Přetížení metody</span><span class="sxs-lookup"><span data-stu-id="479b6-1003">Method overloading</span></span>

<span data-ttu-id="479b6-1004">Pravidla řešení přetížení metod jsou popsána v tématu [odvození typu](expressions.md#type-inference).</span><span class="sxs-lookup"><span data-stu-id="479b6-1004">The method overload resolution rules are described in [Type inference](expressions.md#type-inference).</span></span>

## <a name="properties"></a><span data-ttu-id="479b6-1005">Vlastnosti</span><span class="sxs-lookup"><span data-stu-id="479b6-1005">Properties</span></span>

<span data-ttu-id="479b6-1006">***Vlastnost*** je člen, který poskytuje přístup k vlastnosti objektu nebo třídy.</span><span class="sxs-lookup"><span data-stu-id="479b6-1006">A ***property*** is a member that provides access to a characteristic of an object or a class.</span></span> <span data-ttu-id="479b6-1007">Mezi příklady vlastností patří délka řetězce, velikost písma, titulek okna, jméno zákazníka atd....</span><span class="sxs-lookup"><span data-stu-id="479b6-1007">Examples of properties include the length of a string, the size of a font, the caption of a window, the name of a customer, and so on.</span></span> <span data-ttu-id="479b6-1008">Vlastnosti jsou přirozené rozšíření polí – oba se nazývají členy s přidruženými typy a syntaxe pro přístup k polím a vlastnostem je stejná.</span><span class="sxs-lookup"><span data-stu-id="479b6-1008">Properties are a natural extension of fields—both are named members with associated types, and the syntax for accessing fields and properties is the same.</span></span> <span data-ttu-id="479b6-1009">Na rozdíl od polí ale vlastnosti neoznačují umístění úložiště.</span><span class="sxs-lookup"><span data-stu-id="479b6-1009">However, unlike fields, properties do not denote storage locations.</span></span> <span data-ttu-id="479b6-1010">Místo toho mají vlastnosti ***přistupující objekty*** , které určují příkazy, které mají být provedeny, když jsou jejich hodnoty čteny nebo zapsány.</span><span class="sxs-lookup"><span data-stu-id="479b6-1010">Instead, properties have ***accessors*** that specify the statements to be executed when their values are read or written.</span></span> <span data-ttu-id="479b6-1011">Vlastnosti tak poskytují mechanismus pro přidružení akcí ke čtení a zápisu atributů objektu; Kromě toho povolují, aby tyto atributy byly vypočítány.</span><span class="sxs-lookup"><span data-stu-id="479b6-1011">Properties thus provide a mechanism for associating actions with the reading and writing of an object's attributes; furthermore, they permit such attributes to be computed.</span></span>

<span data-ttu-id="479b6-1012">Vlastnosti jsou deklarovány pomocí *property_declaration*s:</span><span class="sxs-lookup"><span data-stu-id="479b6-1012">Properties are declared using *property_declaration*s:</span></span>

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

<span data-ttu-id="479b6-1013">*Property_declaration* může zahrnovat sadu *atributů* ([atributů](attributes.md)) a platnou kombinaci modifikátorů čtyř přístupu ([modifikátory](classes.md#access-modifiers) `new` přístupu), a to ([nový modifikátor](classes.md#the-new-modifier)) `static` ([statický a metody instance](classes.md#static-and-instance-methods)), `virtual` ([virtuální metody](classes.md#virtual-methods)), `override` ([metody](classes.md#override-methods) `abstract` přepisu) `sealed` ,[(](classes.md#sealed-methods)metody přepsání), ([abstraktní](classes.md#abstract-methods)metody) a `extern`([Externí metody](classes.md#external-methods)) modifikátory.</span><span class="sxs-lookup"><span data-stu-id="479b6-1013">A *property_declaration* may include a set of *attributes* ([Attributes](attributes.md)) and a valid combination of the four access modifiers ([Access modifiers](classes.md#access-modifiers)), the `new` ([The new modifier](classes.md#the-new-modifier)),  `static` ([Static and instance methods](classes.md#static-and-instance-methods)), `virtual` ([Virtual methods](classes.md#virtual-methods)), `override` ([Override methods](classes.md#override-methods)), `sealed` ([Sealed methods](classes.md#sealed-methods)), `abstract` ([Abstract methods](classes.md#abstract-methods)), and `extern` ([External methods](classes.md#external-methods)) modifiers.</span></span>

<span data-ttu-id="479b6-1014">Deklarace vlastností jsou v souladu se stejnými pravidly jako deklarace metod ([metody](classes.md#methods)) s ohledem na platné kombinace modifikátorů.</span><span class="sxs-lookup"><span data-stu-id="479b6-1014">Property declarations are subject to the same rules as method declarations ([Methods](classes.md#methods)) with regard to valid combinations of modifiers.</span></span>

<span data-ttu-id="479b6-1015">*Typ* deklarace vlastnosti určuje typ vlastnosti zavedené deklarací a *MEMBER_NAME* Určuje název vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="479b6-1015">The *type* of a property declaration specifies the type of the property introduced by the declaration, and the *member_name* specifies the name of the property.</span></span> <span data-ttu-id="479b6-1016">Pokud vlastnost není explicitní implementací člena rozhraní, *MEMBER_NAME* je pouze *identifikátor*.</span><span class="sxs-lookup"><span data-stu-id="479b6-1016">Unless the property is an explicit interface member implementation, the *member_name* is simply an *identifier*.</span></span> <span data-ttu-id="479b6-1017">Pro explicitní implementaci člena rozhraní ([explicitní implementace členů rozhraní](interfaces.md#explicit-interface-member-implementations)) se *MEMBER_NAME* skládá z *INTERFACE_TYPE* následovaných "`.`" a *identifikátorem*.</span><span class="sxs-lookup"><span data-stu-id="479b6-1017">For an explicit interface member implementation ([Explicit interface member implementations](interfaces.md#explicit-interface-member-implementations)), the *member_name* consists of an *interface_type* followed by a "`.`" and an *identifier*.</span></span>

<span data-ttu-id="479b6-1018">*Typ* vlastnosti musí být alespoň tak přístupný jako samotná vlastnost ([Omezení přístupnosti](basic-concepts.md#accessibility-constraints)).</span><span class="sxs-lookup"><span data-stu-id="479b6-1018">The *type* of a property must be at least as accessible as the property itself ([Accessibility constraints](basic-concepts.md#accessibility-constraints)).</span></span>

<span data-ttu-id="479b6-1019">*Property_body* může buď sestávat z ***těla přistupujícího objektu*** nebo ***textu výrazu***.</span><span class="sxs-lookup"><span data-stu-id="479b6-1019">A *property_body* may either consist of an ***accessor body*** or an ***expression body***.</span></span> <span data-ttu-id="479b6-1020">V těle objektu pro přístup, *accessor_declarations*, který musí být uzavřen v`{`tokenech "`}`" a "", deklarujte přistupující objekty ([přistupující objekty](classes.md#accessors)) vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="479b6-1020">In an accessor body,  *accessor_declarations*, which must be enclosed in "`{`" and "`}`" tokens, declare the accessors ([Accessors](classes.md#accessors)) of the property.</span></span> <span data-ttu-id="479b6-1021">Přistupující objekty určují spustitelné příkazy spojené s čtením a zápisem vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="479b6-1021">The accessors specify the executable statements associated with reading and writing the property.</span></span>

<span data-ttu-id="479b6-1022">Tělo `=>` výrazu sestávající z *výrazu následovaný výrazem* `E` a středníkem je přesně ekvivalentní tělo `{ get { return E; } }`příkazu a lze jej proto použít pouze k zadání vlastností pouze pro getter, kde výsledek metoda getter je dána jediným výrazem.</span><span class="sxs-lookup"><span data-stu-id="479b6-1022">An expression body consisting of `=>` followed by an *expression* `E` and a semicolon is exactly equivalent to the statement body `{ get { return E; } }`, and can therefore only be used to specify getter-only properties where the result of the getter is given by a single expression.</span></span>

<span data-ttu-id="479b6-1023">*Property_initializer* se může předávat jenom pro automaticky implementované vlastnosti ([automaticky implementované vlastnosti](classes.md#automatically-implemented-properties)) a vyvolá inicializaci podkladového pole těchto vlastností s hodnotou zadanou *výrazem.* .</span><span class="sxs-lookup"><span data-stu-id="479b6-1023">A *property_initializer* may only be given for an automatically implemented property ([Automatically implemented properties](classes.md#automatically-implemented-properties)), and causes the initialization of the underlying field of such properties with the value given by the *expression*.</span></span>

<span data-ttu-id="479b6-1024">I když syntaxe pro přístup k vlastnosti je stejná jako u pole, vlastnost není klasifikována jako proměnná.</span><span class="sxs-lookup"><span data-stu-id="479b6-1024">Even though the syntax for accessing a property is the same as that for a field, a property is not classified as a variable.</span></span> <span data-ttu-id="479b6-1025">Proto není možné předat vlastnost jako `ref` argument or. `out`</span><span class="sxs-lookup"><span data-stu-id="479b6-1025">Thus, it is not possible to pass a property as a `ref` or `out` argument.</span></span>

<span data-ttu-id="479b6-1026">Pokud deklarace vlastnosti obsahuje `extern` modifikátor, vlastnost je označována jako ***externí vlastnost***.</span><span class="sxs-lookup"><span data-stu-id="479b6-1026">When a property declaration includes an `extern` modifier, the property is said to be an ***external property***.</span></span> <span data-ttu-id="479b6-1027">Vzhledem k tomu, že deklarace externí vlastnosti neposkytuje žádnou skutečnou implementaci, každá z jeho *accessor_declarations* se skládá ze střední hodnoty.</span><span class="sxs-lookup"><span data-stu-id="479b6-1027">Because an external property declaration provides no actual implementation, each of its *accessor_declarations* consists of a semicolon.</span></span>

### <a name="static-and-instance-properties"></a><span data-ttu-id="479b6-1028">Statické a vlastnosti instance</span><span class="sxs-lookup"><span data-stu-id="479b6-1028">Static and instance properties</span></span>

<span data-ttu-id="479b6-1029">Pokud deklarace vlastnosti obsahuje `static` modifikátor, vlastnost je označována jako ***statická vlastnost***.</span><span class="sxs-lookup"><span data-stu-id="479b6-1029">When a property declaration includes a `static` modifier, the property is said to be a ***static property***.</span></span> <span data-ttu-id="479b6-1030">Pokud není `static` k dispozici žádný modifikátor, vlastnost je označována jako ***vlastnost instance***.</span><span class="sxs-lookup"><span data-stu-id="479b6-1030">When no `static` modifier is present, the property is said to be an ***instance property***.</span></span>

<span data-ttu-id="479b6-1031">Statická vlastnost není přidružena k určité instanci a jedná se o chybu při kompilaci, na kterou se odkazuje `this` v přístupových objektech statické vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="479b6-1031">A static property is not associated with a specific instance, and it is a compile-time error to refer to `this` in the accessors of a static property.</span></span>

<span data-ttu-id="479b6-1032">Vlastnost instance je přidružena k dané instanci třídy a k této instanci lze v přístupových objektech této `this` vlastnosti přistupovat jako ([Tento přístup](expressions.md#this-access)).</span><span class="sxs-lookup"><span data-stu-id="479b6-1032">An instance property is associated with a given instance of a class, and that instance can be accessed as `this` ([This access](expressions.md#this-access)) in the accessors of that property.</span></span>

<span data-ttu-id="479b6-1033">Pokud je vlastnost odkazována v *member_access* ([přístupu ke členu](expressions.md#member-access)) `E.M`formuláře, pokud `M` je statická vlastnost, `E` musí znamenat typ obsahující `M`a pokud `M` je instancí. vlastnost, E musí poznamenat instanci typu obsahující `M`.</span><span class="sxs-lookup"><span data-stu-id="479b6-1033">When a property is referenced in a *member_access* ([Member access](expressions.md#member-access)) of the form `E.M`, if `M` is a static property, `E` must denote a type containing `M`, and if `M` is an instance property, E must denote an instance of a type containing `M`.</span></span>

<span data-ttu-id="479b6-1034">Rozdíly mezi členy static a instance jsou podrobněji popsány ve [statických a instancích členů](classes.md#static-and-instance-members).</span><span class="sxs-lookup"><span data-stu-id="479b6-1034">The differences between static and instance members are discussed further in [Static and instance members](classes.md#static-and-instance-members).</span></span>

### <a name="accessors"></a><span data-ttu-id="479b6-1035">Přístupové objekty</span><span class="sxs-lookup"><span data-stu-id="479b6-1035">Accessors</span></span>

<span data-ttu-id="479b6-1036">*Accessor_declarations* vlastnosti určují spustitelné příkazy přidružené ke čtení a zápisu této vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="479b6-1036">The *accessor_declarations* of a property specify the executable statements associated with reading and writing that property.</span></span>

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

<span data-ttu-id="479b6-1037">Deklarace přistupujícího objektu se skládají z *get_accessor_declaration*, *set_accessor_declaration*nebo obou.</span><span class="sxs-lookup"><span data-stu-id="479b6-1037">The accessor declarations consist of a *get_accessor_declaration*, a *set_accessor_declaration*, or both.</span></span> <span data-ttu-id="479b6-1038">Každá deklarace přístupového objektu se skládá `get` z `set` tokenu nebo po něm následovat volitelná *accessor_modifier* a *accessor_body*.</span><span class="sxs-lookup"><span data-stu-id="479b6-1038">Each accessor declaration consists of the token `get` or `set` followed by an optional *accessor_modifier* and an *accessor_body*.</span></span>

<span data-ttu-id="479b6-1039">Použití *accessor_modifier*s se řídí následujícími omezeními:</span><span class="sxs-lookup"><span data-stu-id="479b6-1039">The use of *accessor_modifier*s is governed by the following restrictions:</span></span>

*  <span data-ttu-id="479b6-1040">*Accessor_modifier* se nesmí použít v rozhraní nebo v explicitní implementaci člena rozhraní.</span><span class="sxs-lookup"><span data-stu-id="479b6-1040">An *accessor_modifier* may not be used in an interface or in an explicit interface member implementation.</span></span>
*  <span data-ttu-id="479b6-1041">U vlastnosti nebo indexeru, který nemá žádný `override` modifikátor, je *accessor_modifier* povolen pouze v případě, že vlastnost nebo indexovací člen má `get` přístup `set` k objektu a a je povolen pouze v jednom z těchto přístupových objektů.</span><span class="sxs-lookup"><span data-stu-id="479b6-1041">For a property or indexer that has no `override` modifier, an *accessor_modifier* is permitted only if the property or indexer has both a `get` and `set` accessor, and then is permitted only on one of those accessors.</span></span>
*  <span data-ttu-id="479b6-1042">U vlastnosti nebo indexeru, který obsahuje `override` modifikátor, musí přistupující objekt odpovídat *accessor_modifier*(pokud existuje) přepsaného přístupového objektu.</span><span class="sxs-lookup"><span data-stu-id="479b6-1042">For a property or indexer that includes an `override` modifier, an accessor must match the *accessor_modifier*, if any, of the accessor being overridden.</span></span>
*  <span data-ttu-id="479b6-1043">*Accessor_modifier* musí deklarovat přístupnost, která je striktně přísnější než deklarovaná přístupnost vlastnosti nebo samotného indexeru.</span><span class="sxs-lookup"><span data-stu-id="479b6-1043">The *accessor_modifier* must declare an accessibility that is strictly more restrictive than the declared accessibility of the property or indexer itself.</span></span> <span data-ttu-id="479b6-1044">Bude přesný:</span><span class="sxs-lookup"><span data-stu-id="479b6-1044">To be precise:</span></span>
   * <span data-ttu-id="479b6-1045">Pokud vlastnost nebo `public`indexer má deklaraci přístupnosti, může být *accessor_modifier* buď `protected internal`, `internal`, `protected`nebo `private`.</span><span class="sxs-lookup"><span data-stu-id="479b6-1045">If the property or indexer has a declared accessibility of `public`, the *accessor_modifier* may be either `protected internal`, `internal`, `protected`, or `private`.</span></span>
   * <span data-ttu-id="479b6-1046">Pokud vlastnost nebo indexer `protected internal`má deklaraci přístupnosti, může být *accessor_modifier* buď `internal`, `protected`nebo `private`.</span><span class="sxs-lookup"><span data-stu-id="479b6-1046">If the property or indexer has a declared accessibility of `protected internal`, the *accessor_modifier* may be either `internal`, `protected`, or `private`.</span></span>
   * <span data-ttu-id="479b6-1047">Pokud vlastnost `internal` nebo indexer má deklaraci přístupnosti nebo `protected`, musí být `private`accessor_modifier.</span><span class="sxs-lookup"><span data-stu-id="479b6-1047">If the property or indexer has a declared accessibility of `internal` or `protected`, the *accessor_modifier* must be `private`.</span></span>
   * <span data-ttu-id="479b6-1048">Pokud vlastnost nebo indexovací člen má deklaraci přístupnosti `private`, nelze použít žádný *accessor_modifier* .</span><span class="sxs-lookup"><span data-stu-id="479b6-1048">If the property or indexer has a declared accessibility of `private`, no *accessor_modifier* may be used.</span></span>

<span data-ttu-id="479b6-1049">V případě `extern` vlastností a jsou accessor_body pro každý přistupující objekt jednoduše středníkem `abstract` .</span><span class="sxs-lookup"><span data-stu-id="479b6-1049">For `abstract` and `extern` properties, the *accessor_body* for each accessor specified is simply a semicolon.</span></span> <span data-ttu-id="479b6-1050">Neabstraktní vlastnost, která není typu extern, může mít každý *accessor_body* středníkem, v takovém případě se jedná o ***automaticky implementovanou vlastnost*** ([automaticky implementované vlastnosti](classes.md#automatically-implemented-properties)).</span><span class="sxs-lookup"><span data-stu-id="479b6-1050">A non-abstract, non-extern property may have each *accessor_body* be a semicolon, in which case it is an ***automatically implemented property*** ([Automatically implemented properties](classes.md#automatically-implemented-properties)).</span></span> <span data-ttu-id="479b6-1051">Automaticky implementovaná vlastnost musí mít alespoň přistupující objekt get.</span><span class="sxs-lookup"><span data-stu-id="479b6-1051">An automatically implemented property must have at least a get accessor.</span></span> <span data-ttu-id="479b6-1052">Pro přistupující objekty jakékoli jiné neabstraktní nebo neexterné vlastnosti je *accessor_body* *blok* , který určuje příkazy, které mají být provedeny, když je vyvolán odpovídající přistupující objekt.</span><span class="sxs-lookup"><span data-stu-id="479b6-1052">For the accessors of any other non-abstract, non-extern property, the *accessor_body* is a *block* which specifies the statements to be executed when the corresponding accessor is invoked.</span></span>

<span data-ttu-id="479b6-1053">`get` Přistupující objekt odpovídá metodě bez parametrů s návratovou hodnotou typu vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="479b6-1053">A `get` accessor corresponds to a parameterless method with a return value of the property type.</span></span> <span data-ttu-id="479b6-1054">S výjimkou cíle přiřazení, při odkazování na vlastnost ve výrazu, `get` je objekt přistupující k vlastnosti vyvolán pro výpočet hodnoty vlastnosti ([hodnoty výrazů](expressions.md#values-of-expressions)).</span><span class="sxs-lookup"><span data-stu-id="479b6-1054">Except as the target of an assignment, when a property is referenced in an expression, the `get` accessor of the property is invoked to compute the value of the property ([Values of expressions](expressions.md#values-of-expressions)).</span></span> <span data-ttu-id="479b6-1055">Tělo `get` přístupového objektu musí splňovat pravidla pro metody vracející hodnoty popsané v [těle metody](classes.md#method-body).</span><span class="sxs-lookup"><span data-stu-id="479b6-1055">The body of a `get` accessor must conform to the rules for value-returning methods described in [Method body](classes.md#method-body).</span></span> <span data-ttu-id="479b6-1056">Konkrétně všechny `return` příkazy v těle `get` přístupového objektu musí určovat výraz, který lze implicitně převést na typ vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="479b6-1056">In particular, all `return` statements in the body of a `get` accessor must specify an expression that is implicitly convertible to the property type.</span></span> <span data-ttu-id="479b6-1057">Kromě toho nesmí být koncový bod `get` přístupového objektu dostupný.</span><span class="sxs-lookup"><span data-stu-id="479b6-1057">Furthermore, the endpoint of a `get` accessor must not be reachable.</span></span>

<span data-ttu-id="479b6-1058">Přistupující objekt odpovídá metodě s parametrem s jednou hodnotou typu vlastnosti `void` a návratovým typem. `set`</span><span class="sxs-lookup"><span data-stu-id="479b6-1058">A `set` accessor corresponds to a method with a single value parameter of the property type and a `void` return type.</span></span> <span data-ttu-id="479b6-1059">Implicitní parametr `set` přístupového objektu je vždy pojmenován `value`.</span><span class="sxs-lookup"><span data-stu-id="479b6-1059">The implicit parameter of a `set` accessor is always named `value`.</span></span> <span data-ttu-id="479b6-1060">Je-li na vlastnost odkazováno jako na cíl přiřazení ([operátor přiřazení](expressions.md#assignment-operators)) `++` nebo jako Operand operátoru `--` nebo (operátory[přírůstku a snížení](expressions.md#postfix-increment-and-decrement-operators) [předpony, modifikátor přírůstku a snížení předpony](expressions.md#prefix-increment-and-decrement-operators)), přistupující objekt je vyvolán s argumentem (jehož hodnota je pravá strana přiřazení nebo operand `++` operátoru OR `--` ), který poskytuje novou hodnotu ([jednoduché přiřazení](expressions.md#simple-assignment)). `set`</span><span class="sxs-lookup"><span data-stu-id="479b6-1060">When a property is referenced as the target of an assignment ([Assignment operators](expressions.md#assignment-operators)), or as the operand of `++` or `--` ([Postfix increment and decrement operators](expressions.md#postfix-increment-and-decrement-operators), [Prefix increment and decrement operators](expressions.md#prefix-increment-and-decrement-operators)), the `set` accessor is invoked with an argument (whose value is that of the right-hand side of the assignment or the operand of the `++` or `--` operator) that provides the new value ([Simple assignment](expressions.md#simple-assignment)).</span></span> <span data-ttu-id="479b6-1061">Tělo `set` přístupového objektu musí splňovat pravidla pro `void` metody popsané v [těle metody](classes.md#method-body).</span><span class="sxs-lookup"><span data-stu-id="479b6-1061">The body of a `set` accessor must conform to the rules for `void` methods described in [Method body](classes.md#method-body).</span></span> <span data-ttu-id="479b6-1062">Konkrétně `return` příkazy`set` v těle přístupového objektu nejsou povoleny pro zadání výrazu.</span><span class="sxs-lookup"><span data-stu-id="479b6-1062">In particular, `return` statements in the `set` accessor body are not permitted to specify an expression.</span></span> <span data-ttu-id="479b6-1063">Vzhledem k tomu, že `value` `set` objektpropřístupimplicitněobsahujeparametrsnázvem,jednáseochybupřikompilacimístníproměnnénebokonstantnídeklaracevpřístupovémobjektu,aby`set` měl tento název.</span><span class="sxs-lookup"><span data-stu-id="479b6-1063">Since a `set` accessor implicitly has a parameter named `value`, it is a compile-time error for a local variable or constant declaration in a `set` accessor to have that name.</span></span>

<span data-ttu-id="479b6-1064">Na základě přítomnosti nebo nepřítomnosti `get` přístupových objektů a `set` je vlastnost klasifikována takto:</span><span class="sxs-lookup"><span data-stu-id="479b6-1064">Based on the presence or absence of the `get` and `set` accessors, a property is classified as follows:</span></span>

*  <span data-ttu-id="479b6-1065">Vlastnost, která obsahuje `get` přístupový objekt `set` i přistupující objekt, je označována jako vlastnost ***pro čtení i zápis*** .</span><span class="sxs-lookup"><span data-stu-id="479b6-1065">A property that includes both a `get` accessor and a `set` accessor is said to be a ***read-write*** property.</span></span>
*  <span data-ttu-id="479b6-1066">Vlastnost, která má pouze `get` přistupující objekt, je označována jako vlastnost ***jen pro čtení*** .</span><span class="sxs-lookup"><span data-stu-id="479b6-1066">A property that has only a `get` accessor is said to be a ***read-only*** property.</span></span> <span data-ttu-id="479b6-1067">Jedná se o chybu při kompilaci pro vlastnost, která je jen pro čtení, aby byla cílem přiřazení.</span><span class="sxs-lookup"><span data-stu-id="479b6-1067">It is a compile-time error for a read-only property to be the target of an assignment.</span></span>
*  <span data-ttu-id="479b6-1068">Vlastnost, která má pouze `set` přistupující objekt, je označována jako vlastnost pouze pro ***zápis*** .</span><span class="sxs-lookup"><span data-stu-id="479b6-1068">A property that has only a `set` accessor is said to be a ***write-only*** property.</span></span> <span data-ttu-id="479b6-1069">S výjimkou cíle přiřazení se jedná o chybu při kompilaci pro odkazování na vlastnost pouze pro zápis ve výrazu.</span><span class="sxs-lookup"><span data-stu-id="479b6-1069">Except as the target of an assignment, it is a compile-time error to reference a write-only property in an expression.</span></span>

<span data-ttu-id="479b6-1070">V příkladu</span><span class="sxs-lookup"><span data-stu-id="479b6-1070">In the example</span></span>
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
<span data-ttu-id="479b6-1071">ovládací prvek deklaruje veřejnou `Caption` vlastnost. `Button`</span><span class="sxs-lookup"><span data-stu-id="479b6-1071">the `Button` control declares a public `Caption` property.</span></span> <span data-ttu-id="479b6-1072">Přistupující objekt `Caption` vlastnosti vrátí řetězec uložený v soukromém `caption` poli. `get`</span><span class="sxs-lookup"><span data-stu-id="479b6-1072">The `get` accessor of the `Caption` property returns the string stored in the private `caption` field.</span></span> <span data-ttu-id="479b6-1073">`set` Přístupový objekt kontroluje, zda je nová hodnota odlišná od aktuální hodnoty, a pokud ano, uloží novou hodnotu a znovu vykreslí ovládací prvek.</span><span class="sxs-lookup"><span data-stu-id="479b6-1073">The `set` accessor checks if the new value is different from the current value, and if so, it stores the new value and repaints the control.</span></span> <span data-ttu-id="479b6-1074">Vlastnosti často postupují podle výše uvedeného vzoru: Přístupový objekt jednoduše vrátí hodnotu uloženou v soukromém poli `set` a přistupující objekt změní toto soukromé pole a pak provede jakékoli další akce, které jsou vyžadovány k úplné aktualizaci stavu objektu. `get`</span><span class="sxs-lookup"><span data-stu-id="479b6-1074">Properties often follow the pattern shown above: The `get` accessor simply returns a value stored in a private field, and the `set` accessor modifies that private field and then performs any additional actions required to fully update the state of the object.</span></span>

<span data-ttu-id="479b6-1075">Podle výše uvedené `Caption` třídyjepříkladem`Button` použití vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="479b6-1075">Given the `Button` class above, the following is an example of use of the `Caption` property:</span></span>
```csharp
Button okButton = new Button();
okButton.Caption = "OK";            // Invokes set accessor
string s = okButton.Caption;        // Invokes get accessor
```

<span data-ttu-id="479b6-1076">V `set` tomto případě je přístup k objektu vyvolán přiřazením hodnoty vlastnosti `get` a přístup je vyvolán odkazem na vlastnost ve výrazu.</span><span class="sxs-lookup"><span data-stu-id="479b6-1076">Here, the `set` accessor is invoked by assigning a value to the property, and the `get` accessor is invoked by referencing the property in an expression.</span></span>

<span data-ttu-id="479b6-1077">Přístupové objekty `set` a vlastnosti nejsou jedinečné, a není možné deklarovat přístupové objekty pro vlastnost samostatně. `get`</span><span class="sxs-lookup"><span data-stu-id="479b6-1077">The `get` and `set` accessors of a property are not distinct members, and it is not possible to declare the accessors of a property separately.</span></span> <span data-ttu-id="479b6-1078">V takovém případě není možné, aby dva přistupující objekty vlastnosti pro čtení a zápis měly jinou přístupnost.</span><span class="sxs-lookup"><span data-stu-id="479b6-1078">As such, it is not possible for the two accessors of a read-write property to have different accessibility.</span></span> <span data-ttu-id="479b6-1079">Příklad</span><span class="sxs-lookup"><span data-stu-id="479b6-1079">The example</span></span>
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
<span data-ttu-id="479b6-1080">nedeklaruje jednu vlastnost pro čtení i zápis.</span><span class="sxs-lookup"><span data-stu-id="479b6-1080">does not declare a single read-write property.</span></span> <span data-ttu-id="479b6-1081">Místo toho deklaruje dvě vlastnosti se stejným názvem, jeden jen pro čtení a jeden jen pro zápis.</span><span class="sxs-lookup"><span data-stu-id="479b6-1081">Rather, it declares two properties with the same name, one read-only and one write-only.</span></span> <span data-ttu-id="479b6-1082">Vzhledem k tomu, že dva členy deklarované ve stejné třídě nemohou mít stejný název, v příkladu dojde k chybě při kompilaci.</span><span class="sxs-lookup"><span data-stu-id="479b6-1082">Since two members declared in the same class cannot have the same name, the example causes a compile-time error to occur.</span></span>

<span data-ttu-id="479b6-1083">Když odvozená třída deklaruje vlastnost se stejným názvem jako zděděné vlastnosti, skryje odvozená vlastnost děděné vlastnosti s ohledem na čtení i zápis.</span><span class="sxs-lookup"><span data-stu-id="479b6-1083">When a derived class declares a property by the same name as an inherited property, the derived property hides the inherited property with respect to both reading and writing.</span></span> <span data-ttu-id="479b6-1084">V příkladu</span><span class="sxs-lookup"><span data-stu-id="479b6-1084">In the example</span></span>
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
<span data-ttu-id="479b6-1085">vlastnost v `B` nástroji skrývá`P` vlastnost v`A` s ohledem na čtení i zápis. `P`</span><span class="sxs-lookup"><span data-stu-id="479b6-1085">the `P` property in `B` hides the `P` property in `A` with respect to both reading and writing.</span></span> <span data-ttu-id="479b6-1086">Proto v příkazech</span><span class="sxs-lookup"><span data-stu-id="479b6-1086">Thus, in the statements</span></span>
```csharp
B b = new B();
b.P = 1;          // Error, B.P is read-only
((A)b).P = 1;     // Ok, reference to A.P
```
<span data-ttu-id="479b6-1087">přiřazení, které `b.P` způsobí nahlášení chyby při kompilaci, vzhledem k tomu, že `P` vlastnost jen pro čtení v `B` nástroji skrývá vlastnost pouze `P` pro zápis v `A`.</span><span class="sxs-lookup"><span data-stu-id="479b6-1087">the assignment to `b.P` causes a compile-time error to be reported, since the read-only `P` property in `B` hides the write-only `P` property in `A`.</span></span> <span data-ttu-id="479b6-1088">Upozorňujeme však, že k přístupu k skryté `P` vlastnosti lze použít přetypování.</span><span class="sxs-lookup"><span data-stu-id="479b6-1088">Note, however, that a cast can be used to access the hidden `P` property.</span></span>

<span data-ttu-id="479b6-1089">Na rozdíl od veřejných polí vlastnosti poskytují oddělení vnitřní stav objektu a jeho veřejné rozhraní.</span><span class="sxs-lookup"><span data-stu-id="479b6-1089">Unlike public fields, properties provide a separation between an object's internal state and its public interface.</span></span> <span data-ttu-id="479b6-1090">Vezměte v úvahu příklad:</span><span class="sxs-lookup"><span data-stu-id="479b6-1090">Consider the example:</span></span>
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

<span data-ttu-id="479b6-1091">Tady třída používá dvě `int` pole, `x` a `y`k uložení jeho umístění. `Label`</span><span class="sxs-lookup"><span data-stu-id="479b6-1091">Here, the `Label` class uses two `int` fields, `x` and `y`, to store its location.</span></span> <span data-ttu-id="479b6-1092">Umístění je veřejně `X` vystaveno jako `Y` a vlastnost i jako `Location` vlastnost typu `Point`.</span><span class="sxs-lookup"><span data-stu-id="479b6-1092">The location is publicly exposed both as an `X` and a `Y` property and as a `Location` property of type `Point`.</span></span> <span data-ttu-id="479b6-1093">Pokud v budoucí verzi `Label`nástroje, je vhodnější Uložit umístění `Point` jako interní, může být změna provedena bez vlivu na veřejné rozhraní třídy:</span><span class="sxs-lookup"><span data-stu-id="479b6-1093">If, in a future version of `Label`, it becomes more convenient to store the location as a `Point` internally, the change can be made without affecting the public interface of the class:</span></span>
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

<span data-ttu-id="479b6-1094">Bylo `x` a `y` místo toho `public readonly` bylo možnéprovést`Label` takovou změnu třídy.</span><span class="sxs-lookup"><span data-stu-id="479b6-1094">Had `x` and `y` instead been `public readonly` fields, it would have been impossible to make such a change to the `Label` class.</span></span>

<span data-ttu-id="479b6-1095">Vystavení stavu prostřednictvím vlastností není nutně efektivní než vystavení polí přímo.</span><span class="sxs-lookup"><span data-stu-id="479b6-1095">Exposing state through properties is not necessarily any less efficient than exposing fields directly.</span></span> <span data-ttu-id="479b6-1096">Zejména pokud je vlastnost nevirtuální a obsahuje pouze malý objem kódu, může spouštěcí prostředí nahradit volání přístupových objektů skutečným kódem přístupových objektů.</span><span class="sxs-lookup"><span data-stu-id="479b6-1096">In particular, when a property is non-virtual and contains only a small amount of code, the execution environment may replace calls to accessors with the actual code of the accessors.</span></span> <span data-ttu-id="479b6-1097">Tento proces se označuje jako a dává přístup k vlastnostem jako účinný přístup k ***polím, ale***zachovává zvýšenou flexibilitu vlastností.</span><span class="sxs-lookup"><span data-stu-id="479b6-1097">This process is known as ***inlining***, and it makes property access as efficient as field access, yet preserves the increased flexibility of properties.</span></span>

<span data-ttu-id="479b6-1098">Vzhledem k tomu, `get` že vyvolání přístupového objektu je v koncepčním ekvivalentu pro čtení hodnoty pole, je považováno za `get` špatný programovací styl pro přistupující objekty, aby měly pozorovatelící vedlejší účinky.</span><span class="sxs-lookup"><span data-stu-id="479b6-1098">Since invoking a `get` accessor is conceptually equivalent to reading the value of a field, it is considered bad programming style for `get` accessors to have observable side-effects.</span></span> <span data-ttu-id="479b6-1099">V příkladu</span><span class="sxs-lookup"><span data-stu-id="479b6-1099">In the example</span></span>
```csharp
class Counter
{
    private int next;

    public int Next {
        get { return next++; }
    }
}
```
<span data-ttu-id="479b6-1100">hodnota `Next` vlastnosti závisí na počtu dříve přístupů k vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="479b6-1100">the value of the `Next` property depends on the number of times the property has previously been accessed.</span></span> <span data-ttu-id="479b6-1101">Proto přístup k vlastnosti vytvoří pozorovatelný vedlejší efekt a vlastnost by měla být namísto toho implementována jako metoda.</span><span class="sxs-lookup"><span data-stu-id="479b6-1101">Thus, accessing the property produces an observable side-effect, and the property should be implemented as a method instead.</span></span>

<span data-ttu-id="479b6-1102">Konvence žádné vedlejší účinky pro `get` přístupové objekty neznamená, že přistupující objekty by měly být vždy zapisovány, `get` aby jednoduše vracely hodnoty uložené v polích.</span><span class="sxs-lookup"><span data-stu-id="479b6-1102">The "no side-effects" convention for `get` accessors doesn't mean that `get` accessors should always be written to simply return values stored in fields.</span></span> <span data-ttu-id="479b6-1103">Přistupující objekty `get` budou často počítat hodnotu vlastnosti tím, že získávají přístup k více polím nebo vyvolání metod.</span><span class="sxs-lookup"><span data-stu-id="479b6-1103">Indeed, `get` accessors often compute the value of a property by accessing multiple fields or invoking methods.</span></span> <span data-ttu-id="479b6-1104">Správně navržený `get` přistupující objekt ale neprovede žádné akce, které způsobují pozorovatelné změny stavu objektu.</span><span class="sxs-lookup"><span data-stu-id="479b6-1104">However, a properly designed `get` accessor performs no actions that cause observable changes in the state of the object.</span></span>

<span data-ttu-id="479b6-1105">Vlastnosti lze použít ke zpoždění inicializace prostředku až do okamžiku, kdy se na něj poprvé odkazuje.</span><span class="sxs-lookup"><span data-stu-id="479b6-1105">Properties can be used to delay initialization of a resource until the moment it is first referenced.</span></span> <span data-ttu-id="479b6-1106">Příklad:</span><span class="sxs-lookup"><span data-stu-id="479b6-1106">For example:</span></span>
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

<span data-ttu-id="479b6-1107">Třída obsahuje tři vlastnosti `In` `Error`,, a, které reprezentují standardní vstupní, výstupní a chybové zařízení. `Out` `Console`</span><span class="sxs-lookup"><span data-stu-id="479b6-1107">The `Console` class contains three properties, `In`, `Out`, and `Error`, that represent the standard input, output, and error devices, respectively.</span></span> <span data-ttu-id="479b6-1108">Zveřejněním těchto členů jako vlastností může `Console` třída zpozdit svou inicializaci, dokud nebudou skutečně použity.</span><span class="sxs-lookup"><span data-stu-id="479b6-1108">By exposing these members as properties, the `Console` class can delay their initialization until they are actually used.</span></span> <span data-ttu-id="479b6-1109">Například při prvním odkazování na `Out` vlastnost, jako v</span><span class="sxs-lookup"><span data-stu-id="479b6-1109">For example, upon first referencing the `Out` property, as in</span></span>
```csharp
Console.Out.WriteLine("hello, world");
```
<span data-ttu-id="479b6-1110">Vytvoří se `TextWriter` podklad pro výstupní zařízení.</span><span class="sxs-lookup"><span data-stu-id="479b6-1110">the underlying `TextWriter` for the output device is created.</span></span> <span data-ttu-id="479b6-1111">Pokud ale aplikace nevytváří žádné odkazy na `In` vlastnosti a `Error` , nevytvoří se pro tato zařízení žádné objekty.</span><span class="sxs-lookup"><span data-stu-id="479b6-1111">But if the application makes no reference to the `In` and `Error` properties, then no objects are created for those devices.</span></span>

### <a name="automatically-implemented-properties"></a><span data-ttu-id="479b6-1112">Automaticky implementované vlastnosti</span><span class="sxs-lookup"><span data-stu-id="479b6-1112">Automatically implemented properties</span></span>

<span data-ttu-id="479b6-1113">Automaticky implementovaná vlastnost (nebo ***Automatická vlastnost*** pro krátkou hodnotu) je neabstraktní neexterní vlastnost s tělem přístupového objektu jenom středníkem.</span><span class="sxs-lookup"><span data-stu-id="479b6-1113">An automatically implemented property (or ***auto-property*** for short), is a non-abstract non-extern property with semicolon-only accessor bodies.</span></span> <span data-ttu-id="479b6-1114">K automatickým vlastnostem musí mít přistupující objekt get a volitelně může mít přístupový objekt set.</span><span class="sxs-lookup"><span data-stu-id="479b6-1114">Auto-properties must have a get accessor and can optionally have a set accessor.</span></span>

<span data-ttu-id="479b6-1115">Je-li vlastnost zadána jako automaticky implementovaná vlastnost, je pro vlastnost automaticky k dispozici skryté pole zálohování a přistupující objekty jsou implementovány pro čtení a zápis do daného pole pro zálohování.</span><span class="sxs-lookup"><span data-stu-id="479b6-1115">When a property is specified as an automatically implemented property, a hidden backing field is automatically available for the property, and the accessors are implemented to read from and write to that backing field.</span></span> <span data-ttu-id="479b6-1116">Pokud automatická vlastnost nemá žádný přistupující objekt set, je pole zálohování považováno za `readonly` ([pole jen pro čtení](classes.md#readonly-fields)).</span><span class="sxs-lookup"><span data-stu-id="479b6-1116">If the auto-property has no set accessor, the backing field is considered `readonly` ([Readonly fields](classes.md#readonly-fields)).</span></span> <span data-ttu-id="479b6-1117">Stejně jako `readonly` pole lze také přiřadit automatickou vlastnost pouze pro getter do těla konstruktoru ohraničující třídy.</span><span class="sxs-lookup"><span data-stu-id="479b6-1117">Just like a `readonly` field, a getter-only auto-property can also be assigned to in the body of a constructor of the enclosing class.</span></span> <span data-ttu-id="479b6-1118">Takové přiřazení přiřadí přímo k poli pro zálohování jen pro čtení ve vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="479b6-1118">Such an assignment assigns directly to the readonly backing field of the property.</span></span>

<span data-ttu-id="479b6-1119">Automatická vlastnost může volitelně mít *property_initializer*, který se aplikuje přímo na pole pro zálohování jako *variable_initializer* ([Inicializátory proměnných](classes.md#variable-initializers)).</span><span class="sxs-lookup"><span data-stu-id="479b6-1119">An auto-property may optionally have a *property_initializer*, which is applied directly to the backing field as a *variable_initializer* ([Variable initializers](classes.md#variable-initializers)).</span></span>

<span data-ttu-id="479b6-1120">Následující příklad:</span><span class="sxs-lookup"><span data-stu-id="479b6-1120">The following example:</span></span>
```csharp
public class Point {
    public int X { get; set; } = 0;
    public int Y { get; set; } = 0;
}
```
<span data-ttu-id="479b6-1121">je ekvivalentní následující deklaraci:</span><span class="sxs-lookup"><span data-stu-id="479b6-1121">is equivalent to the following declaration:</span></span>
```csharp
public class Point {
    private int __x = 0;
    private int __y = 0;
    public int X { get { return __x; } set { __x = value; } }
    public int Y { get { return __y; } set { __y = value; } }
}
```

<span data-ttu-id="479b6-1122">Následující příklad:</span><span class="sxs-lookup"><span data-stu-id="479b6-1122">The following example:</span></span>
```csharp
public class ReadOnlyPoint
{
    public int X { get; }
    public int Y { get; }
    public ReadOnlyPoint(int x, int y) { X = x; Y = y; }
}
```
<span data-ttu-id="479b6-1123">je ekvivalentní následující deklaraci:</span><span class="sxs-lookup"><span data-stu-id="479b6-1123">is equivalent to the following declaration:</span></span>
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

<span data-ttu-id="479b6-1124">Všimněte si, že přiřazení k poli jen pro čtení jsou zákonná, protože se vyskytují v rámci konstruktoru.</span><span class="sxs-lookup"><span data-stu-id="479b6-1124">Notice that the assignments to the readonly field are legal, because they occur within the constructor.</span></span>


### <a name="accessibility"></a><span data-ttu-id="479b6-1125">Usnadnění</span><span class="sxs-lookup"><span data-stu-id="479b6-1125">Accessibility</span></span>

<span data-ttu-id="479b6-1126">Pokud má přistupující objekt *accessor_modifier*, doména přístupnosti ([domény přístupnosti](basic-concepts.md#accessibility-domains)) je určena pomocí deklarovaného přístupnosti *accessor_modifier*.</span><span class="sxs-lookup"><span data-stu-id="479b6-1126">If an accessor has an *accessor_modifier*, the accessibility domain ([Accessibility domains](basic-concepts.md#accessibility-domains)) of the accessor is determined using the declared accessibility of the *accessor_modifier*.</span></span> <span data-ttu-id="479b6-1127">Pokud přístupový objekt nemá *accessor_modifier*, je doména přístupnosti přistupujícího objektu určena z deklarovaného přístupnosti vlastnosti nebo indexeru.</span><span class="sxs-lookup"><span data-stu-id="479b6-1127">If an accessor does not have an *accessor_modifier*, the accessibility domain of the accessor is determined from the declared accessibility of the property or indexer.</span></span>

<span data-ttu-id="479b6-1128">Přítomnost *accessor_modifier* nikdy nemá vliv na vyhledávání členů ([Operators](expressions.md#operators)) nebo rozlišení přetěžování ([řešení přetížení](expressions.md#overload-resolution)).</span><span class="sxs-lookup"><span data-stu-id="479b6-1128">The presence of an *accessor_modifier* never affects member lookup ([Operators](expressions.md#operators)) or overload resolution ([Overload resolution](expressions.md#overload-resolution)).</span></span> <span data-ttu-id="479b6-1129">Modifikátory u vlastnosti nebo indexeru vždy určují, na kterou vlastnost nebo indexer je svázán, bez ohledu na kontext přístupu.</span><span class="sxs-lookup"><span data-stu-id="479b6-1129">The modifiers on the property or indexer always determine which property or indexer is bound to, regardless of the context of the access.</span></span>

<span data-ttu-id="479b6-1130">Po výběru konkrétní vlastnosti nebo indexeru se k určení, jestli je toto použití platné, použijí domény přístupnosti konkrétních přístupových objektů:</span><span class="sxs-lookup"><span data-stu-id="479b6-1130">Once a particular property or indexer has been selected, the accessibility domains of the specific accessors involved are used to determine if that usage is valid:</span></span>

*  <span data-ttu-id="479b6-1131">Pokud je použití jako hodnota ([hodnoty výrazů](expressions.md#values-of-expressions)), `get` přistupující objekt musí existovat a být přístupný.</span><span class="sxs-lookup"><span data-stu-id="479b6-1131">If the usage is as a value ([Values of expressions](expressions.md#values-of-expressions)), the `get` accessor must exist and be accessible.</span></span>
*  <span data-ttu-id="479b6-1132">Pokud je použití jako cíl jednoduchého přiřazení ([jednoduché přiřazení](expressions.md#simple-assignment)), `set` přístup musí existovat a být přístupný.</span><span class="sxs-lookup"><span data-stu-id="479b6-1132">If the usage is as the target of a simple assignment ([Simple assignment](expressions.md#simple-assignment)), the `set` accessor must exist and be accessible.</span></span>
*  <span data-ttu-id="479b6-1133">Pokud je použití jako cíl složeného přiřazení ([složené přiřazení](expressions.md#compound-assignment)) nebo `++` jako cíl operátorů nebo `--` ( `get` [Členové funkce](expressions.md#function-members).9, [výrazy vyvolání](expressions.md#invocation-expressions)), oba přistupující objekty a `set` přístupový objekt musí existovat a být přístupný.</span><span class="sxs-lookup"><span data-stu-id="479b6-1133">If the usage is as the target of compound assignment ([Compound assignment](expressions.md#compound-assignment)), or as the target of the `++` or `--` operators ([Function members](expressions.md#function-members).9, [Invocation expressions](expressions.md#invocation-expressions)), both the `get` accessors and the `set` accessor must exist and be accessible.</span></span>

<span data-ttu-id="479b6-1134">V následujícím příkladu je vlastnost `A.Text` skrytá vlastností `B.Text`, a to i v `set` kontextech, kde je volána pouze přístupový objekt.</span><span class="sxs-lookup"><span data-stu-id="479b6-1134">In the following example, the property `A.Text` is hidden by the property `B.Text`, even in contexts where only the `set` accessor is called.</span></span> <span data-ttu-id="479b6-1135">Naproti tomu vlastnost `B.Count` není přístupná třídě `M`, takže se místo toho použije vlastnost `A.Count` přístupná.</span><span class="sxs-lookup"><span data-stu-id="479b6-1135">In contrast, the property `B.Count` is not accessible to class `M`, so the accessible property `A.Count` is used instead.</span></span>

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

<span data-ttu-id="479b6-1136">Přistupující objekt, který se používá k implementaci rozhraní, nesmí mít objekt *accessor_modifier*.</span><span class="sxs-lookup"><span data-stu-id="479b6-1136">An accessor that is used to implement an interface may not have an *accessor_modifier*.</span></span> <span data-ttu-id="479b6-1137">Pokud je k implementaci rozhraní použit pouze jeden přistupující objekt, druhý přistupující objekt může být deklarován s *accessor_modifier*:</span><span class="sxs-lookup"><span data-stu-id="479b6-1137">If only one accessor is used to implement an interface, the other accessor may be declared with an *accessor_modifier*:</span></span>
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

### <a name="virtual-sealed-override-and-abstract-property-accessors"></a><span data-ttu-id="479b6-1138">Virtuální, zapečetěné, přepsané a abstraktní přístupové objekty vlastnosti</span><span class="sxs-lookup"><span data-stu-id="479b6-1138">Virtual, sealed, override, and abstract property accessors</span></span>

<span data-ttu-id="479b6-1139">Deklarace `virtual` vlastnosti určuje, že přistupující objekty vlastnosti jsou virtuální.</span><span class="sxs-lookup"><span data-stu-id="479b6-1139">A `virtual` property declaration specifies that the accessors of the property are virtual.</span></span> <span data-ttu-id="479b6-1140">`virtual` Modifikátor platí pro přistupující objekty vlastnosti pro čtení i zápis – není možné pouze jeden přistupující objekt vlastnosti pro čtení i zápis, který by měl být virtuální.</span><span class="sxs-lookup"><span data-stu-id="479b6-1140">The `virtual` modifier applies to both accessors of a read-write property—it is not possible for only one accessor of a read-write property to be virtual.</span></span>

<span data-ttu-id="479b6-1141">Deklarace `abstract` vlastnosti určuje, že přistupující objekty vlastnosti jsou virtuální, ale neposkytuje skutečnou implementaci přistupujících objektů.</span><span class="sxs-lookup"><span data-stu-id="479b6-1141">An `abstract` property declaration specifies that the accessors of the property are virtual, but does not provide an actual implementation of the accessors.</span></span> <span data-ttu-id="479b6-1142">Místo toho jsou vyžadovány neabstraktní odvozené třídy pro zajištění vlastní implementace přístupových objektů přepsáním vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="479b6-1142">Instead, non-abstract derived classes are required to provide their own implementation for the accessors by overriding the property.</span></span> <span data-ttu-id="479b6-1143">Vzhledem k tomu, že přistupující objekt pro deklaraci abstraktní vlastnosti neposkytuje žádnou skutečnou implementaci, jeho *accessor_body* se jednoduše skládá ze střední hodnoty.</span><span class="sxs-lookup"><span data-stu-id="479b6-1143">Because an accessor for an abstract property declaration provides no actual implementation, its *accessor_body* simply consists of a semicolon.</span></span>

<span data-ttu-id="479b6-1144">Deklarace vlastnosti, která zahrnuje `abstract` modifikátory a `override` , určuje, že vlastnost je abstraktní a Přepisuje základní vlastnost.</span><span class="sxs-lookup"><span data-stu-id="479b6-1144">A property declaration that includes both the `abstract` and `override` modifiers specifies that the property is abstract and overrides a base property.</span></span> <span data-ttu-id="479b6-1145">Přístupové objekty takové vlastnosti jsou také abstraktní.</span><span class="sxs-lookup"><span data-stu-id="479b6-1145">The accessors of such a property are also abstract.</span></span>

<span data-ttu-id="479b6-1146">Deklarace abstraktních vlastností jsou povolené jenom v abstraktních třídách ([abstraktní třídy](classes.md#abstract-classes)). Přistupující objekty zděděné virtuální vlastnosti mohou být přepsány v odvozené třídě zahrnutím deklarace vlastnosti, která určuje `override` direktivu.</span><span class="sxs-lookup"><span data-stu-id="479b6-1146">Abstract property declarations are only permitted in abstract classes ([Abstract classes](classes.md#abstract-classes)).The accessors of an inherited virtual property can be overridden in a derived class by including a property declaration that specifies an `override` directive.</span></span> <span data-ttu-id="479b6-1147">Toto je známo jako ***přepsání deklarace vlastnosti***.</span><span class="sxs-lookup"><span data-stu-id="479b6-1147">This is known as an ***overriding property declaration***.</span></span> <span data-ttu-id="479b6-1148">Přepsání deklarace vlastnosti nedeklaruje novou vlastnost.</span><span class="sxs-lookup"><span data-stu-id="479b6-1148">An overriding property declaration does not declare a new property.</span></span> <span data-ttu-id="479b6-1149">Místo toho se pouze specializují implementace přístupových objektů existující virtuální vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="479b6-1149">Instead, it simply specializes the implementations of the accessors of an existing virtual property.</span></span>

<span data-ttu-id="479b6-1150">Přepisující deklaraci vlastnosti musí určovat přesně stejné Modifikátory dostupnosti, typ a název jako zděděnou vlastnost.</span><span class="sxs-lookup"><span data-stu-id="479b6-1150">An overriding property declaration must specify the exact same accessibility modifiers, type, and name as the inherited property.</span></span> <span data-ttu-id="479b6-1151">Pokud má zděděná vlastnost pouze jeden přistupující objekt (tj. Pokud je zděděná vlastnost jen pro čtení nebo jen pro zápis), překrytá vlastnost musí zahrnovat pouze tento přístupový objekt.</span><span class="sxs-lookup"><span data-stu-id="479b6-1151">If the inherited property has only a single accessor (i.e., if the inherited property is read-only or write-only), the overriding property must include only that accessor.</span></span> <span data-ttu-id="479b6-1152">Pokud zděděná vlastnost zahrnuje přístupové objekty (tj. Pokud je zděděná vlastnost určena pro čtení i zápis), přepsání vlastnosti může zahrnovat buď jeden přistupující objekt, nebo oba přístupové objekty.</span><span class="sxs-lookup"><span data-stu-id="479b6-1152">If the inherited property includes both accessors (i.e., if the inherited property is read-write), the overriding property can include either a single accessor or both accessors.</span></span>

<span data-ttu-id="479b6-1153">Přepisující deklaraci vlastnosti může obsahovat `sealed` modifikátor.</span><span class="sxs-lookup"><span data-stu-id="479b6-1153">An overriding property declaration may include the `sealed` modifier.</span></span> <span data-ttu-id="479b6-1154">Použití tohoto modifikátoru zabrání odvození třídy z dalšího přepsání vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="479b6-1154">Use of this modifier prevents a derived class from further overriding the property.</span></span> <span data-ttu-id="479b6-1155">Přístupové objekty zapečetěné vlastnosti jsou také zapečetěné.</span><span class="sxs-lookup"><span data-stu-id="479b6-1155">The accessors of a sealed property are also sealed.</span></span>

<span data-ttu-id="479b6-1156">S výjimkou rozdílů v deklaraci a syntaxi vyvolání, virtuálních, zapečetěných, přepsání a abstraktních přístupových objektů se chovají stejně jako virtuální, zapečetěné, přepisování a abstraktní metody.</span><span class="sxs-lookup"><span data-stu-id="479b6-1156">Except for differences in declaration and invocation syntax, virtual, sealed, override, and abstract accessors behave exactly like virtual, sealed, override and abstract methods.</span></span> <span data-ttu-id="479b6-1157">Konkrétně pravidla, která jsou popsána v tématu [virtuální metody](classes.md#virtual-methods), [metody přepsání](classes.md#override-methods), [zapečetěné metody](classes.md#sealed-methods)a [abstraktní metody](classes.md#abstract-methods) , se použijí, jako by přistupující objekty byly metody odpovídající formy:</span><span class="sxs-lookup"><span data-stu-id="479b6-1157">Specifically, the rules described in [Virtual methods](classes.md#virtual-methods), [Override methods](classes.md#override-methods), [Sealed methods](classes.md#sealed-methods), and [Abstract methods](classes.md#abstract-methods) apply as if accessors were methods of a corresponding form:</span></span>

*  <span data-ttu-id="479b6-1158">`get` Přistupující objekt odpovídá metodě bez parametrů s návratovou hodnotou typu vlastnosti a stejnými modifikátory jako obsahující vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="479b6-1158">A `get` accessor corresponds to a parameterless method with a return value of the property type and the same modifiers as the containing property.</span></span>
*  <span data-ttu-id="479b6-1159">Přístupový objekt odpovídá metodě s parametrem s jednou hodnotou typu vlastnosti `void` , návratovým typem a stejnými modifikátory jako obsahující vlastnosti. `set`</span><span class="sxs-lookup"><span data-stu-id="479b6-1159">A `set` accessor corresponds to a method with a single value parameter of the property type, a `void` return type, and the same modifiers as the containing property.</span></span>

<span data-ttu-id="479b6-1160">V příkladu</span><span class="sxs-lookup"><span data-stu-id="479b6-1160">In the example</span></span>
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
<span data-ttu-id="479b6-1161">`X`je virtuální vlastnost jen pro čtení, `Y` je virtuální vlastností pro čtení i zápis a `Z` je abstraktní vlastnost pro čtení i zápis.</span><span class="sxs-lookup"><span data-stu-id="479b6-1161">`X` is a virtual read-only property, `Y` is a virtual read-write property, and `Z` is an abstract read-write property.</span></span> <span data-ttu-id="479b6-1162">Protože `Z` je abstraktní, obsažená třída `A` musí být také deklarována jako abstraktní.</span><span class="sxs-lookup"><span data-stu-id="479b6-1162">Because `Z` is abstract, the containing class `A` must also be declared abstract.</span></span>

<span data-ttu-id="479b6-1163">Třída, která je odvozena `A` z, je zobrazena níže:</span><span class="sxs-lookup"><span data-stu-id="479b6-1163">A class that derives from `A` is show below:</span></span>
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

<span data-ttu-id="479b6-1164">Tady, deklarace `X`, `Y`a `Z` přepisují deklarace vlastností.</span><span class="sxs-lookup"><span data-stu-id="479b6-1164">Here, the declarations of `X`, `Y`, and `Z` are overriding property declarations.</span></span> <span data-ttu-id="479b6-1165">Každá deklarace vlastnosti přesně odpovídá modifikátorům přístupnosti, typu a názvu odpovídající zděděné vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="479b6-1165">Each property declaration exactly matches the accessibility modifiers, type, and name of the corresponding inherited property.</span></span> <span data-ttu-id="479b6-1166">`get` `Y` Přístupovýobjekt`base` objektu a`set`přistupující objekt k použití klíčového slova pro přístup k zděděným přístupovým objektům. `X`</span><span class="sxs-lookup"><span data-stu-id="479b6-1166">The `get` accessor of `X` and the `set` accessor of `Y` use the `base` keyword to access the inherited accessors.</span></span> <span data-ttu-id="479b6-1167">Deklarace `Z` přepíše jak abstraktní přistupující objekty, takže neexistují žádné zbývající členy abstraktní funkce v `B` `B` a je povoleno být Neabstraktní třída.</span><span class="sxs-lookup"><span data-stu-id="479b6-1167">The declaration of `Z` overrides both abstract accessors—thus, there are no outstanding abstract function members in `B`, and `B` is permitted to be a non-abstract class.</span></span>

<span data-ttu-id="479b6-1168">Je-li vlastnost deklarována jako `override`, musí být všechny přepsané přistupující objekty přístupné přepsání kódu.</span><span class="sxs-lookup"><span data-stu-id="479b6-1168">When a property is declared as an `override`, any overridden accessors must be accessible to the overriding code.</span></span> <span data-ttu-id="479b6-1169">Kromě toho musí být deklarované přístupnosti samotné vlastnosti i indexeru a přistupující objekty shodné s tím, že přepsané členy a přistupující objekty.</span><span class="sxs-lookup"><span data-stu-id="479b6-1169">In addition, the declared accessibility of both the property or indexer itself, and of the accessors, must match that of the overridden member and accessors.</span></span> <span data-ttu-id="479b6-1170">Příklad:</span><span class="sxs-lookup"><span data-stu-id="479b6-1170">For example:</span></span>
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

## <a name="events"></a><span data-ttu-id="479b6-1171">Události</span><span class="sxs-lookup"><span data-stu-id="479b6-1171">Events</span></span>

<span data-ttu-id="479b6-1172">***Událost*** je člen, který umožňuje objektu nebo třídě poskytnout oznámení.</span><span class="sxs-lookup"><span data-stu-id="479b6-1172">An ***event*** is a member that enables an object or class to provide notifications.</span></span> <span data-ttu-id="479b6-1173">Klienti mohou připojit spustitelný kód pro události tím, že dodávají ***obslužné rutiny událostí***.</span><span class="sxs-lookup"><span data-stu-id="479b6-1173">Clients can attach executable code for events by supplying ***event handlers***.</span></span>

<span data-ttu-id="479b6-1174">Události jsou deklarovány pomocí *event_declaration*s:</span><span class="sxs-lookup"><span data-stu-id="479b6-1174">Events are declared using *event_declaration*s:</span></span>

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

<span data-ttu-id="479b6-1175">*Event_declaration* může zahrnovat sadu *atributů* ([atributů](attributes.md)) a platnou kombinaci modifikátorů čtyř přístupu ([modifikátory](classes.md#access-modifiers) `new` přístupu), a to ([nový modifikátor](classes.md#the-new-modifier)) `static` ([statický a metody instance](classes.md#static-and-instance-methods)), `virtual` ([virtuální metody](classes.md#virtual-methods)), `override` ([metody](classes.md#override-methods) `abstract` přepisu) `sealed` ,[(](classes.md#sealed-methods)metody přepsání), ([abstraktní](classes.md#abstract-methods)metody) a `extern`([Externí metody](classes.md#external-methods)) modifikátory.</span><span class="sxs-lookup"><span data-stu-id="479b6-1175">An *event_declaration* may include a set of *attributes* ([Attributes](attributes.md)) and a valid combination of the four access modifiers ([Access modifiers](classes.md#access-modifiers)), the `new` ([The new modifier](classes.md#the-new-modifier)),  `static` ([Static and instance methods](classes.md#static-and-instance-methods)), `virtual` ([Virtual methods](classes.md#virtual-methods)), `override` ([Override methods](classes.md#override-methods)), `sealed` ([Sealed methods](classes.md#sealed-methods)), `abstract` ([Abstract methods](classes.md#abstract-methods)), and `extern` ([External methods](classes.md#external-methods)) modifiers.</span></span>

<span data-ttu-id="479b6-1176">Deklarace událostí podléhají stejným pravidlům jako deklarace metod ([metody](classes.md#methods)) s ohledem na platné kombinace modifikátorů.</span><span class="sxs-lookup"><span data-stu-id="479b6-1176">Event declarations are subject to the same rules as method declarations ([Methods](classes.md#methods)) with regard to valid combinations of modifiers.</span></span>

<span data-ttu-id="479b6-1177">Typ deklarace události musí být *typu* *delegate_type* ([odkazové typy](types.md#reference-types)) a tento *delegate_type* musí být alespoň tak přístupný jako samotná událost ([Omezení přístupnosti](basic-concepts.md#accessibility-constraints)).</span><span class="sxs-lookup"><span data-stu-id="479b6-1177">The *type* of an event declaration must be a *delegate_type* ([Reference types](types.md#reference-types)), and that *delegate_type* must be at least as accessible as the event itself ([Accessibility constraints](basic-concepts.md#accessibility-constraints)).</span></span>

<span data-ttu-id="479b6-1178">Deklarace události může zahrnovat *event_accessor_declarations*.</span><span class="sxs-lookup"><span data-stu-id="479b6-1178">An event declaration may include *event_accessor_declarations*.</span></span> <span data-ttu-id="479b6-1179">Nicméně pokud to neplatí pro neexterní, neabstraktní události, kompilátor je automaticky dodá ([události podobné poli](classes.md#field-like-events)); pro externí události jsou přístupové objekty poskytovány externě.</span><span class="sxs-lookup"><span data-stu-id="479b6-1179">However, if it does not, for non-extern, non-abstract events, the compiler supplies them automatically ([Field-like events](classes.md#field-like-events)); for extern events, the accessors are provided externally.</span></span>

<span data-ttu-id="479b6-1180">Deklarace události, která vynechává *event_accessor_declarations* , definuje jednu nebo více událostí – jeden pro každou *variable_declarator*.</span><span class="sxs-lookup"><span data-stu-id="479b6-1180">An event declaration that omits *event_accessor_declarations* defines one or more events—one for each of the *variable_declarator*s.</span></span> <span data-ttu-id="479b6-1181">Atributy a modifikátory se vztahují na všechny členy deklarované tímto *event_declaration*.</span><span class="sxs-lookup"><span data-stu-id="479b6-1181">The attributes and modifiers apply to all of the members declared by such an *event_declaration*.</span></span>

<span data-ttu-id="479b6-1182">Jedná se o chybu při kompilaci, kterou může *event_declaration* zahrnovat modifikátor a `abstract` *event_accessor_declarations*s oddělovači ve složených závorkách.</span><span class="sxs-lookup"><span data-stu-id="479b6-1182">It is a compile-time error for an *event_declaration* to include both the `abstract` modifier and brace-delimited *event_accessor_declarations*.</span></span>

<span data-ttu-id="479b6-1183">Pokud deklarace události obsahuje `extern` modifikátor, událost je označována jako ***externí událost***.</span><span class="sxs-lookup"><span data-stu-id="479b6-1183">When an event declaration includes an `extern` modifier, the event is said to be an ***external event***.</span></span> <span data-ttu-id="479b6-1184">Vzhledem k tomu, že deklarace externí události neposkytuje žádnou skutečnou implementaci, jedná se o chybu, která `extern` by zahrnovala jak modifikátor, tak *event_accessor_declarations*.</span><span class="sxs-lookup"><span data-stu-id="479b6-1184">Because an external event declaration provides no actual implementation, it is an error for it to include both the `extern` modifier and *event_accessor_declarations*.</span></span>

<span data-ttu-id="479b6-1185">Jedná se o chybu při kompilaci pro *variable_declarator* deklarace události s `abstract` modifikátorem or `external` pro zahrnutí *variable_initializer*.</span><span class="sxs-lookup"><span data-stu-id="479b6-1185">It is a compile-time error for a *variable_declarator* of an event declaration with an `abstract` or `external` modifier to include a *variable_initializer*.</span></span>

<span data-ttu-id="479b6-1186">Událost lze použít jako levý operand `+=` operátorů a `-=` ([přiřazení události](expressions.md#event-assignment)).</span><span class="sxs-lookup"><span data-stu-id="479b6-1186">An event can be used as the left-hand operand of the `+=` and `-=` operators ([Event assignment](expressions.md#event-assignment)).</span></span> <span data-ttu-id="479b6-1187">Tyto operátory se používají k připojení obslužných rutin událostí k nebo k odebrání obslužných rutin událostí z události a modifikátorů přístupu ovládacího prvku události, ve kterém jsou tyto operace povoleny.</span><span class="sxs-lookup"><span data-stu-id="479b6-1187">These operators are used, respectively, to attach event handlers to or to remove event handlers from an event, and the access modifiers of the event control the contexts in which such operations are permitted.</span></span>

<span data-ttu-id="479b6-1188">Vzhledem `+=` k `-=` tomu, že a jsou pouze operace, které jsou povoleny pro událost mimo typ, který deklaruje událost, může externí kód přidat a odebrat obslužné rutiny pro událost, ale nemůže žádným jiným způsobem získat nebo změnit základní seznam události. žádostí.</span><span class="sxs-lookup"><span data-stu-id="479b6-1188">Since `+=` and `-=` are the only operations that are permitted on an event outside the type that declares the event, external code can add and remove handlers for an event, but cannot in any other way obtain or modify the underlying list of event handlers.</span></span>

<span data-ttu-id="479b6-1189">V `x += y` operaci formuláře `void` nebo `x -= y`, když `x` je událost a odkaz probíhá `x`mimo typ, který obsahuje deklaraci, výsledek operace je typu (na rozdíl od typ `x` s`x` hodnotou po přiřazení).</span><span class="sxs-lookup"><span data-stu-id="479b6-1189">In an operation of the form `x += y` or `x -= y`, when `x` is an event and the reference takes place outside the type that contains the declaration of `x`, the result of the operation has type `void` (as opposed to having the type of `x`, with the value of `x` after the assignment).</span></span> <span data-ttu-id="479b6-1190">Toto pravidlo zabrání externímu kódu v nepřímém zkoumání základního delegáta události.</span><span class="sxs-lookup"><span data-stu-id="479b6-1190">This rule prohibits external code from indirectly examining the underlying delegate of an event.</span></span>

<span data-ttu-id="479b6-1191">Následující příklad ukazuje, jak jsou obslužné rutiny událostí připojeny k instancím `Button` třídy:</span><span class="sxs-lookup"><span data-stu-id="479b6-1191">The following example shows how event handlers are attached to instances of the `Button` class:</span></span>
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

<span data-ttu-id="479b6-1192">Zde konstruktor `Button` instance vytvoří dvě instance a připojí obslužné rutiny události k `Click` událostem. `LoginDialog`</span><span class="sxs-lookup"><span data-stu-id="479b6-1192">Here, the `LoginDialog` instance constructor creates two `Button` instances and attaches event handlers to the `Click` events.</span></span>

### <a name="field-like-events"></a><span data-ttu-id="479b6-1193">Události podobné poli</span><span class="sxs-lookup"><span data-stu-id="479b6-1193">Field-like events</span></span>

<span data-ttu-id="479b6-1194">V textu programu třídy nebo struktury, která obsahuje deklaraci události, mohou být některé události použity jako pole.</span><span class="sxs-lookup"><span data-stu-id="479b6-1194">Within the program text of the class or struct that contains the declaration of an event, certain events can be used like fields.</span></span> <span data-ttu-id="479b6-1195">Pro použití tímto způsobem nesmí být `abstract` událost ani `extern`a nesmí explicitně zahrnovat *event_accessor_declarations*.</span><span class="sxs-lookup"><span data-stu-id="479b6-1195">To be used in this way, an event must not be `abstract` or `extern`, and must not explicitly include *event_accessor_declarations*.</span></span> <span data-ttu-id="479b6-1196">Taková událost se dá použít v jakémkoli kontextu, který povoluje pole.</span><span class="sxs-lookup"><span data-stu-id="479b6-1196">Such an event can be used in any context that permits a field.</span></span> <span data-ttu-id="479b6-1197">Pole obsahuje delegáta ([Delegáti](delegates.md)), který odkazuje na seznam obslužných rutin událostí přidaných do události.</span><span class="sxs-lookup"><span data-stu-id="479b6-1197">The field contains a delegate ([Delegates](delegates.md)) which refers to the list of event handlers that have been added to the event.</span></span> <span data-ttu-id="479b6-1198">Pokud nebyly přidány žádné obslužné rutiny událostí, pole obsahuje `null`.</span><span class="sxs-lookup"><span data-stu-id="479b6-1198">If no event handlers have been added, the field contains `null`.</span></span>

<span data-ttu-id="479b6-1199">V příkladu</span><span class="sxs-lookup"><span data-stu-id="479b6-1199">In the example</span></span>
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
<span data-ttu-id="479b6-1200">`Click`slouží jako pole v rámci `Button` třídy.</span><span class="sxs-lookup"><span data-stu-id="479b6-1200">`Click` is used as a field within the `Button` class.</span></span> <span data-ttu-id="479b6-1201">Jak ukazuje příklad, pole lze prozkoumat, upravit a použít ve výrazech volání delegátů.</span><span class="sxs-lookup"><span data-stu-id="479b6-1201">As the example demonstrates, the field can be examined, modified, and used in delegate invocation expressions.</span></span> <span data-ttu-id="479b6-1202">`OnClick` Metoda`Button` ve`Click` třídě vyvolá událost.</span><span class="sxs-lookup"><span data-stu-id="479b6-1202">The `OnClick` method in the `Button` class "raises" the `Click` event.</span></span> <span data-ttu-id="479b6-1203">Pojem vyvolání události je přesně shodný s voláním delegáta reprezentovaného událostí, takže neexistují žádné speciální jazykové konstrukce pro vyvolání událostí.</span><span class="sxs-lookup"><span data-stu-id="479b6-1203">The notion of raising an event is precisely equivalent to invoking the delegate represented by the event—thus, there are no special language constructs for raising events.</span></span> <span data-ttu-id="479b6-1204">Všimněte si, že volání delegáta předchází kontrolu, která zajišťuje, že delegát je jiný než null.</span><span class="sxs-lookup"><span data-stu-id="479b6-1204">Note that the delegate invocation is preceded by a check that ensures the delegate is non-null.</span></span>

<span data-ttu-id="479b6-1205">Mimo deklaraci `Button` třídy `Click` může být člen použit pouze na levé straně `+=` operátoru and `-=` , jako v</span><span class="sxs-lookup"><span data-stu-id="479b6-1205">Outside the declaration of the `Button` class, the `Click` member can only be used on the left-hand side of the `+=` and `-=` operators, as in</span></span>
```csharp
b.Click += new EventHandler(...);
```
<span data-ttu-id="479b6-1206">který připojí delegáta k seznamu `Click` vyvolání události a</span><span class="sxs-lookup"><span data-stu-id="479b6-1206">which appends a delegate to the invocation list of the `Click` event, and</span></span>
```csharp
b.Click -= new EventHandler(...);
```
<span data-ttu-id="479b6-1207">Tím se odebere delegát ze seznamu `Click` vyvolání události.</span><span class="sxs-lookup"><span data-stu-id="479b6-1207">which removes a delegate from the invocation list of the `Click` event.</span></span>

<span data-ttu-id="479b6-1208">Při kompilování události podobné poli kompilátor automaticky vytvoří úložiště pro blokování delegáta a vytvoří přistupující objekty pro událost, která přidá nebo odebere obslužné rutiny události do pole delegát.</span><span class="sxs-lookup"><span data-stu-id="479b6-1208">When compiling a field-like event, the compiler automatically creates storage to hold the delegate, and creates accessors for the event that add or remove event handlers to the delegate field.</span></span> <span data-ttu-id="479b6-1209">Operace sčítání a odebírání jsou bezpečné pro přístup z více vláken a mohou (ale nemusí) být provedeny, když držíte zámek ([příkaz Lock](statements.md#the-lock-statement)) na obsahujícím objektu události instance nebo objekt typu ([výrazy vytváření anonymních objektů](expressions.md#anonymous-object-creation-expressions)). pro statickou událost.</span><span class="sxs-lookup"><span data-stu-id="479b6-1209">The addition and removal operations are thread safe, and may (but are not required to) be done while holding the lock ([The lock statement](statements.md#the-lock-statement)) on the containing object for an instance event, or the type object ([Anonymous object creation expressions](expressions.md#anonymous-object-creation-expressions)) for a static event.</span></span>

<span data-ttu-id="479b6-1210">Proto deklarace události instance formuláře:</span><span class="sxs-lookup"><span data-stu-id="479b6-1210">Thus, an instance event declaration of the form:</span></span>
```csharp
class X
{
    public event D Ev;
}
```
<span data-ttu-id="479b6-1211">bude zkompilována do podobného:</span><span class="sxs-lookup"><span data-stu-id="479b6-1211">will be compiled to something equivalent to:</span></span>
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
<span data-ttu-id="479b6-1212">V rámci třídy `X` `Ev` odkazy na levou stranu `+=` operátoru and `-=` způsobí vyvolání přístupových objektů Add a Remove.</span><span class="sxs-lookup"><span data-stu-id="479b6-1212">Within the class `X`, references to `Ev` on the left-hand side of the `+=` and `-=` operators cause the add and remove accessors to be invoked.</span></span> <span data-ttu-id="479b6-1213">Všechny ostatní odkazy na `Ev` jsou kompilovány tak, aby odkazovaly na skryté pole `__Ev` namísto ([přístup k přístupu členů](expressions.md#member-access)).</span><span class="sxs-lookup"><span data-stu-id="479b6-1213">All other references to `Ev` are compiled to reference the hidden field `__Ev` instead ([Member access](expressions.md#member-access)).</span></span> <span data-ttu-id="479b6-1214">Název "`__Ev`" je libovolný. skryté pole může mít libovolný název nebo žádný název.</span><span class="sxs-lookup"><span data-stu-id="479b6-1214">The name "`__Ev`" is arbitrary; the hidden field could have any name or no name at all.</span></span>

### <a name="event-accessors"></a><span data-ttu-id="479b6-1215">Přístupové objekty událostí</span><span class="sxs-lookup"><span data-stu-id="479b6-1215">Event accessors</span></span>

<span data-ttu-id="479b6-1216">Deklarace událostí typicky vynechávají *event_accessor_declarations*, jako v `Button` předchozím příkladu.</span><span class="sxs-lookup"><span data-stu-id="479b6-1216">Event declarations typically omit *event_accessor_declarations*, as in the `Button` example above.</span></span> <span data-ttu-id="479b6-1217">Jedna z těchto situací zahrnuje případ, ve kterém není přijatelné náklady na úložiště jednoho pole na každou událost.</span><span class="sxs-lookup"><span data-stu-id="479b6-1217">One situation for doing so involves the case in which the storage cost of one field per event is not acceptable.</span></span> <span data-ttu-id="479b6-1218">V takových případech může třída zahrnovat *event_accessor_declarations* a používat privátní mechanizmus pro ukládání seznamu obslužných rutin událostí.</span><span class="sxs-lookup"><span data-stu-id="479b6-1218">In such cases, a class can include *event_accessor_declarations* and use a private mechanism for storing the list of event handlers.</span></span>

<span data-ttu-id="479b6-1219">*Event_accessor_declarations* události specifikuje spustitelné příkazy přidružené k přidávání a odebírání obslužných rutin událostí.</span><span class="sxs-lookup"><span data-stu-id="479b6-1219">The *event_accessor_declarations* of an event specify the executable statements associated with adding and removing event handlers.</span></span>

<span data-ttu-id="479b6-1220">Deklarace přistupujícího objektu se skládají z *add_accessor_declaration* a *remove_accessor_declaration*.</span><span class="sxs-lookup"><span data-stu-id="479b6-1220">The accessor declarations consist of an *add_accessor_declaration* and a *remove_accessor_declaration*.</span></span> <span data-ttu-id="479b6-1221">Každá deklarace přistupujícího objektu se `add` skládá `remove` z tokenu nebo následovaný *blokem*.</span><span class="sxs-lookup"><span data-stu-id="479b6-1221">Each accessor declaration consists of the token `add` or `remove` followed by a *block*.</span></span> <span data-ttu-id="479b6-1222">*Blok* přidružený k *add_accessor_declaration* Určuje příkazy, které mají být provedeny při přidání obslužné rutiny události, a *blok* přidružený k *remove_accessor_declaration* Určuje příkazy, které mají být provedeny. Při odebrání obslužné rutiny události.</span><span class="sxs-lookup"><span data-stu-id="479b6-1222">The *block* associated with an *add_accessor_declaration* specifies the statements to execute when an event handler is added, and the *block* associated with a *remove_accessor_declaration* specifies the statements to execute when an event handler is removed.</span></span>

<span data-ttu-id="479b6-1223">Každý *add_accessor_declaration* a *remove_accessor_declaration* odpovídá metodě s parametrem s jednou hodnotou `void` typu události a návratovým typem.</span><span class="sxs-lookup"><span data-stu-id="479b6-1223">Each *add_accessor_declaration* and *remove_accessor_declaration* corresponds to a method with a single value parameter of the event type and a `void` return type.</span></span> <span data-ttu-id="479b6-1224">Implicitní parametr přístupového objektu události má název `value`.</span><span class="sxs-lookup"><span data-stu-id="479b6-1224">The implicit parameter of an event accessor is named `value`.</span></span> <span data-ttu-id="479b6-1225">V případě, že se v přiřazení události používá událost, použije se odpovídající přístupový objekt události.</span><span class="sxs-lookup"><span data-stu-id="479b6-1225">When an event is used in an event assignment, the appropriate event accessor is used.</span></span> <span data-ttu-id="479b6-1226">Konkrétně, je-li operátor `+=` přiřazení použit, je použit přistupující objekt Add a je-li `-=` operátor přiřazení, je použit přístupový objekt Remove.</span><span class="sxs-lookup"><span data-stu-id="479b6-1226">Specifically, if the assignment operator is `+=` then the add accessor is used, and if the assignment operator is `-=` then the remove accessor is used.</span></span> <span data-ttu-id="479b6-1227">V obou případech je pravý operand operátoru přiřazení použit jako argument přístupového objektu události.</span><span class="sxs-lookup"><span data-stu-id="479b6-1227">In either case, the right-hand operand of the assignment operator is used as the argument to the event accessor.</span></span> <span data-ttu-id="479b6-1228">Blok *add_accessor_declaration* nebo *remove_accessor_declaration* musí odpovídat pravidlům pro `void` metody popsané v [těle metody](classes.md#method-body).</span><span class="sxs-lookup"><span data-stu-id="479b6-1228">The block of an *add_accessor_declaration* or a *remove_accessor_declaration* must conform to the rules for `void` methods described in [Method body](classes.md#method-body).</span></span> <span data-ttu-id="479b6-1229">Konkrétně `return` příkazy v takovém bloku nejsou povoleny pro určení výrazu.</span><span class="sxs-lookup"><span data-stu-id="479b6-1229">In particular, `return` statements in such a block are not permitted to specify an expression.</span></span>

<span data-ttu-id="479b6-1230">Vzhledem k tomu, že objekt pro přístup k události `value`má implicitně parametr s názvem, jedná se o chybu při kompilaci pro místní proměnnou nebo konstantu deklarovanou v přístupovém objektu události, aby měl tento název.</span><span class="sxs-lookup"><span data-stu-id="479b6-1230">Since an event accessor implicitly has a parameter named `value`, it is a compile-time error for a local variable or constant declared in an event accessor to have that name.</span></span>

<span data-ttu-id="479b6-1231">V příkladu</span><span class="sxs-lookup"><span data-stu-id="479b6-1231">In the example</span></span>
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
<span data-ttu-id="479b6-1232">`Control` třída implementuje interní mechanismus úložiště pro události.</span><span class="sxs-lookup"><span data-stu-id="479b6-1232">the `Control` class implements an internal storage mechanism for events.</span></span> <span data-ttu-id="479b6-1233">Metoda přidruží hodnotu delegáta k klíči `GetEventHandler` , metoda vrátí delegáta, který je aktuálně přidružen k klíči, a `RemoveEventHandler` metoda odebere delegáta jako obslužnou rutinu události pro zadanou událost. `AddEventHandler`</span><span class="sxs-lookup"><span data-stu-id="479b6-1233">The `AddEventHandler` method associates a delegate value with a key, the `GetEventHandler` method returns the delegate currently associated with a key, and the `RemoveEventHandler` method removes a delegate as an event handler for the specified event.</span></span> <span data-ttu-id="479b6-1234">Proto je podkladový mechanismus úložiště navržený tak, že se neúčtují žádné náklady na přidružení `null` hodnoty delegáta k klíči, takže nezpracované události nevyužívají žádné úložiště.</span><span class="sxs-lookup"><span data-stu-id="479b6-1234">Presumably, the underlying storage mechanism is designed such that there is no cost for associating a `null` delegate value with a key, and thus unhandled events consume no storage.</span></span>

### <a name="static-and-instance-events"></a><span data-ttu-id="479b6-1235">Statické a instance události</span><span class="sxs-lookup"><span data-stu-id="479b6-1235">Static and instance events</span></span>

<span data-ttu-id="479b6-1236">Pokud deklarace události obsahuje `static` modifikátor, událost je označována jako ***statická událost***.</span><span class="sxs-lookup"><span data-stu-id="479b6-1236">When an event declaration includes a `static` modifier, the event is said to be a ***static event***.</span></span> <span data-ttu-id="479b6-1237">Pokud není `static` k dispozici žádný modifikátor, událost je označována jako ***událost instance***.</span><span class="sxs-lookup"><span data-stu-id="479b6-1237">When no `static` modifier is present, the event is said to be an ***instance event***.</span></span>

<span data-ttu-id="479b6-1238">Statická událost není přidružena k určité instanci a jedná se o chybu při kompilaci, na kterou se odkazuje `this` v přístupových objektech statické události.</span><span class="sxs-lookup"><span data-stu-id="479b6-1238">A static event is not associated with a specific instance, and it is a compile-time error to refer to `this` in the accessors of a static event.</span></span>

<span data-ttu-id="479b6-1239">Událost instance je přidružena k dané instanci třídy a k této instanci lze přistupovat jako `this` ([přístup](expressions.md#this-access)) v přístupových objektech této události.</span><span class="sxs-lookup"><span data-stu-id="479b6-1239">An instance event is associated with a given instance of a class, and this instance can be accessed as `this` ([This access](expressions.md#this-access)) in the accessors of that event.</span></span>

<span data-ttu-id="479b6-1240">Pokud je událost odkazována v *member_access* ([přístupu ke členu](expressions.md#member-access)) `E.M`formuláře, pokud `M` je statická událost, `E` musí poznamenat typ obsahující `M`a pokud `M` je událost instance, E musí označuje instanci typu obsahujícího `M`.</span><span class="sxs-lookup"><span data-stu-id="479b6-1240">When an event is referenced in a *member_access* ([Member access](expressions.md#member-access)) of the form `E.M`, if `M` is a static event, `E` must denote a type containing `M`, and if `M` is an instance event, E must denote an instance of a type containing `M`.</span></span>

<span data-ttu-id="479b6-1241">Rozdíly mezi členy static a instance jsou podrobněji popsány ve [statických a instancích členů](classes.md#static-and-instance-members).</span><span class="sxs-lookup"><span data-stu-id="479b6-1241">The differences between static and instance members are discussed further in [Static and instance members](classes.md#static-and-instance-members).</span></span>

### <a name="virtual-sealed-override-and-abstract-event-accessors"></a><span data-ttu-id="479b6-1242">Virtuální, zapečetěné, přepsání a abstraktní přístupové objekty událostí</span><span class="sxs-lookup"><span data-stu-id="479b6-1242">Virtual, sealed, override, and abstract event accessors</span></span>

<span data-ttu-id="479b6-1243">Deklarace `virtual` události Určuje, že přistupující objekty této události jsou virtuální.</span><span class="sxs-lookup"><span data-stu-id="479b6-1243">A `virtual` event declaration specifies that the accessors of that event are virtual.</span></span> <span data-ttu-id="479b6-1244">`virtual` Modifikátor platí pro přistupující objekty události.</span><span class="sxs-lookup"><span data-stu-id="479b6-1244">The `virtual` modifier applies to both accessors of an event.</span></span>

<span data-ttu-id="479b6-1245">Deklarace `abstract` události Určuje, že přistupující objekty události jsou virtuální, ale neposkytuje skutečnou implementaci přistupujících objektů.</span><span class="sxs-lookup"><span data-stu-id="479b6-1245">An `abstract` event declaration specifies that the accessors of the event are virtual, but does not provide an actual implementation of the accessors.</span></span> <span data-ttu-id="479b6-1246">Místo toho jsou vyžadovány neabstraktní odvozené třídy pro zajištění vlastní implementace přístupových objektů přepsáním události.</span><span class="sxs-lookup"><span data-stu-id="479b6-1246">Instead, non-abstract derived classes are required to provide their own implementation for the accessors by overriding the event.</span></span> <span data-ttu-id="479b6-1247">Vzhledem k tomu, že deklarace abstraktní události neposkytuje žádnou skutečnou implementaci, nemůže poskytnout *event_accessor_declarations*oddělený závorkami.</span><span class="sxs-lookup"><span data-stu-id="479b6-1247">Because an abstract event declaration provides no actual implementation, it cannot provide brace-delimited *event_accessor_declarations*.</span></span>

<span data-ttu-id="479b6-1248">Deklarace události, která zahrnuje `abstract` modifikátory a `override` , určuje, že událost je abstraktní a Přepisuje základní událost.</span><span class="sxs-lookup"><span data-stu-id="479b6-1248">An event declaration that includes both the `abstract` and `override` modifiers specifies that the event is abstract and overrides a base event.</span></span> <span data-ttu-id="479b6-1249">Přístupové objekty takové události jsou také abstraktní.</span><span class="sxs-lookup"><span data-stu-id="479b6-1249">The accessors of such an event are also abstract.</span></span>

<span data-ttu-id="479b6-1250">Abstraktní deklarace událostí jsou povolené jenom v abstraktních třídách ([abstraktní třídy](classes.md#abstract-classes)).</span><span class="sxs-lookup"><span data-stu-id="479b6-1250">Abstract event declarations are only permitted in abstract classes ([Abstract classes](classes.md#abstract-classes)).</span></span>

<span data-ttu-id="479b6-1251">Přistupující objekty zděděné virtuální události lze přepsat v odvozené třídě zahrnutím deklarace události, která určuje `override` modifikátor.</span><span class="sxs-lookup"><span data-stu-id="479b6-1251">The accessors of an inherited virtual event can be overridden in a derived class by including an event declaration that specifies an `override` modifier.</span></span> <span data-ttu-id="479b6-1252">To se označuje jako ***přepsání deklarace události***.</span><span class="sxs-lookup"><span data-stu-id="479b6-1252">This is known as an ***overriding event declaration***.</span></span> <span data-ttu-id="479b6-1253">Přepsání deklarace události nedeklaruje novou událost.</span><span class="sxs-lookup"><span data-stu-id="479b6-1253">An overriding event declaration does not declare a new event.</span></span> <span data-ttu-id="479b6-1254">Místo toho se pouze specializují implementace přístupových objektů existující virtuální události.</span><span class="sxs-lookup"><span data-stu-id="479b6-1254">Instead, it simply specializes the implementations of the accessors of an existing virtual event.</span></span>

<span data-ttu-id="479b6-1255">Přepisující deklaraci události musí určovat přesně stejné Modifikátory dostupnosti, typ a název jako přepsanou událost.</span><span class="sxs-lookup"><span data-stu-id="479b6-1255">An overriding event declaration must specify the exact same accessibility modifiers, type, and name as the overridden event.</span></span>

<span data-ttu-id="479b6-1256">Přepisující deklaraci události může obsahovat `sealed` modifikátor.</span><span class="sxs-lookup"><span data-stu-id="479b6-1256">An overriding event declaration may include the `sealed` modifier.</span></span> <span data-ttu-id="479b6-1257">Použití tohoto modifikátoru zabrání odvození třídy v dalším přepsání události.</span><span class="sxs-lookup"><span data-stu-id="479b6-1257">Use of this modifier prevents a derived class from further overriding the event.</span></span> <span data-ttu-id="479b6-1258">Přístupové objekty zapečetěné události jsou také zapečetěné.</span><span class="sxs-lookup"><span data-stu-id="479b6-1258">The accessors of a sealed event are also sealed.</span></span>

<span data-ttu-id="479b6-1259">Jedná se o chybu při kompilaci, která přepisuje deklaraci události, aby zahrnovala `new` modifikátor.</span><span class="sxs-lookup"><span data-stu-id="479b6-1259">It is a compile-time error for an overriding event declaration to include a `new` modifier.</span></span>

<span data-ttu-id="479b6-1260">S výjimkou rozdílů v deklaraci a syntaxi vyvolání, virtuálních, zapečetěných, přepsání a abstraktních přístupových objektů se chovají stejně jako virtuální, zapečetěné, přepisování a abstraktní metody.</span><span class="sxs-lookup"><span data-stu-id="479b6-1260">Except for differences in declaration and invocation syntax, virtual, sealed, override, and abstract accessors behave exactly like virtual, sealed, override and abstract methods.</span></span> <span data-ttu-id="479b6-1261">Konkrétně pravidla, která jsou popsána v tématu [virtuální metody](classes.md#virtual-methods), [metody přepisu](classes.md#override-methods), [zapečetěné metody](classes.md#sealed-methods)a [abstraktní metody](classes.md#abstract-methods) , se použijí, jako přistupující objekty byly metody odpovídající formy.</span><span class="sxs-lookup"><span data-stu-id="479b6-1261">Specifically, the rules described in [Virtual methods](classes.md#virtual-methods), [Override methods](classes.md#override-methods), [Sealed methods](classes.md#sealed-methods), and [Abstract methods](classes.md#abstract-methods) apply as if accessors were methods of a corresponding form.</span></span> <span data-ttu-id="479b6-1262">Každý přistupující objekt odpovídá metodě s parametrem s jednou hodnotou typu události, `void` návratový typ a stejné modifikátory jako obsažená událost.</span><span class="sxs-lookup"><span data-stu-id="479b6-1262">Each accessor corresponds to a method with a single value parameter of the event type, a `void` return type, and the same modifiers as the containing event.</span></span>

## <a name="indexers"></a><span data-ttu-id="479b6-1263">Indexery</span><span class="sxs-lookup"><span data-stu-id="479b6-1263">Indexers</span></span>

<span data-ttu-id="479b6-1264">***Indexer*** je člen, který umožňuje indexování objektu stejným způsobem jako pole.</span><span class="sxs-lookup"><span data-stu-id="479b6-1264">An ***indexer*** is a member that enables an object to be indexed in the same way as an array.</span></span> <span data-ttu-id="479b6-1265">Indexery jsou deklarovány pomocí *indexer_declaration*s:</span><span class="sxs-lookup"><span data-stu-id="479b6-1265">Indexers are declared using *indexer_declaration*s:</span></span>

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

<span data-ttu-id="479b6-1266">*Indexer_declaration* může zahrnovat sadu *atributů* ([atributů](attributes.md)) a platnou kombinaci modifikátorů čtyř přístupu ([modifikátory](classes.md#access-modifiers) `new` přístupu), a to ([nový modifikátor](classes.md#the-new-modifier)), `virtual` ([ Virtuální metody](classes.md#virtual-methods)), `override` ([metody](classes.md#override-methods) `sealed` přepisu),[(](classes.md#sealed-methods) `abstract` metody přepsání), ([abstraktní](classes.md#abstract-methods)metody) a `extern` modifikátory ([externí metody](classes.md#external-methods)).</span><span class="sxs-lookup"><span data-stu-id="479b6-1266">An *indexer_declaration* may include a set of *attributes* ([Attributes](attributes.md)) and a valid combination of the four access modifiers ([Access modifiers](classes.md#access-modifiers)), the `new` ([The new modifier](classes.md#the-new-modifier)), `virtual` ([Virtual methods](classes.md#virtual-methods)), `override` ([Override methods](classes.md#override-methods)), `sealed` ([Sealed methods](classes.md#sealed-methods)), `abstract` ([Abstract methods](classes.md#abstract-methods)), and `extern` ([External methods](classes.md#external-methods)) modifiers.</span></span>

<span data-ttu-id="479b6-1267">Deklarace indexeru podléhají stejným pravidlům jako deklarace metod ([metody](classes.md#methods)) s ohledem na platné kombinace modifikátorů, s jednou výjimkou, že statický modifikátor není povolený u deklarace indexeru.</span><span class="sxs-lookup"><span data-stu-id="479b6-1267">Indexer declarations are subject to the same rules as method declarations ([Methods](classes.md#methods)) with regard to valid combinations of modifiers, with the one exception being that the static modifier is not permitted on an indexer declaration.</span></span>

<span data-ttu-id="479b6-1268">Modifikátory `virtual`, `override`a `abstract` se vzájemně vylučují, s výjimkou jednoho případu.</span><span class="sxs-lookup"><span data-stu-id="479b6-1268">The modifiers `virtual`, `override`, and `abstract` are mutually exclusive except in one case.</span></span> <span data-ttu-id="479b6-1269">Modifikátory `override` a mohou být použity společně, aby abstraktní indexer mohl přepsat virtuální. `abstract`</span><span class="sxs-lookup"><span data-stu-id="479b6-1269">The `abstract` and `override` modifiers may be used together so that an abstract indexer can override a virtual one.</span></span>

<span data-ttu-id="479b6-1270">*Typ* deklarace indexer určuje typ elementu indexeru zavedeného deklarací.</span><span class="sxs-lookup"><span data-stu-id="479b6-1270">The *type* of an indexer declaration specifies the element type of the indexer introduced by the declaration.</span></span> <span data-ttu-id="479b6-1271">Pokud indexer není explicitní implementací člena rozhraní, pak je *typ* následován klíčovým slovem `this`.</span><span class="sxs-lookup"><span data-stu-id="479b6-1271">Unless the indexer is an explicit interface member implementation, the *type* is followed by the keyword `this`.</span></span> <span data-ttu-id="479b6-1272">Pro explicitní implementaci člena rozhraní je *typ* následován *INTERFACE_TYPE*, "`.`" a klíčovým slovem. `this`</span><span class="sxs-lookup"><span data-stu-id="479b6-1272">For an explicit interface member implementation, the *type* is followed by an *interface_type*, a "`.`", and the keyword `this`.</span></span> <span data-ttu-id="479b6-1273">Na rozdíl od jiných členů indexery nemají uživatelsky definované názvy.</span><span class="sxs-lookup"><span data-stu-id="479b6-1273">Unlike other members, indexers do not have user-defined names.</span></span>

<span data-ttu-id="479b6-1274">*Formal_parameter_list* Určuje parametry indexeru.</span><span class="sxs-lookup"><span data-stu-id="479b6-1274">The *formal_parameter_list* specifies the parameters of the indexer.</span></span> <span data-ttu-id="479b6-1275">Seznam formálních parametrů indexeru odpovídá metodě ([parametrům metody](classes.md#method-parameters)), s tím rozdílem, že je nutné zadat alespoň jeden parametr a zda `ref` nejsou povoleny modifikátory parametrů a. `out`</span><span class="sxs-lookup"><span data-stu-id="479b6-1275">The formal parameter list of an indexer corresponds to that of a method ([Method parameters](classes.md#method-parameters)), except that at least one parameter must be specified, and that the `ref` and `out` parameter modifiers are not permitted.</span></span>

<span data-ttu-id="479b6-1276">*Typ* indexeru a každý z typů odkazovaných v *formal_parameter_list* musí být alespoň tak přístupný jako indexer samotný ([Omezení přístupnosti](basic-concepts.md#accessibility-constraints)).</span><span class="sxs-lookup"><span data-stu-id="479b6-1276">The *type* of an indexer and each of the types referenced in the *formal_parameter_list* must be at least as accessible as the indexer itself ([Accessibility constraints](basic-concepts.md#accessibility-constraints)).</span></span>

<span data-ttu-id="479b6-1277">*Indexer_body* může buď sestávat z ***těla přistupujícího objektu*** nebo ***textu výrazu***.</span><span class="sxs-lookup"><span data-stu-id="479b6-1277">An *indexer_body* may either consist of an ***accessor body*** or an ***expression body***.</span></span> <span data-ttu-id="479b6-1278">V těle objektu pro přístup, *accessor_declarations*, který musí být uzavřen v`{`tokenech "`}`" a "", deklarujte přistupující objekty ([přistupující objekty](classes.md#accessors)) vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="479b6-1278">In an accessor body, *accessor_declarations*, which must be enclosed in "`{`" and "`}`" tokens, declare the accessors ([Accessors](classes.md#accessors)) of the property.</span></span> <span data-ttu-id="479b6-1279">Přistupující objekty určují spustitelné příkazy spojené s čtením a zápisem vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="479b6-1279">The accessors specify the executable statements associated with reading and writing the property.</span></span>

<span data-ttu-id="479b6-1280">Tělo výrazu sestávající z "`=>`" následovaný výrazem `E` a středníkem je přesně ekvivalentní tělo `{ get { return E; } }`příkazu, a lze jej proto použít pouze k zadání indexerů pouze pro getter, kde je výsledek getter předána jediným výrazem.</span><span class="sxs-lookup"><span data-stu-id="479b6-1280">An expression body consisting of "`=>`" followed by an expression `E` and a semicolon is exactly equivalent to the statement body `{ get { return E; } }`, and can therefore only be used to specify getter-only indexers where the result of the getter is given by a single expression.</span></span>

<span data-ttu-id="479b6-1281">I když je syntaxe pro přístup k prvku indexeru stejná jako u prvku pole, element indexeru není klasifikován jako proměnná.</span><span class="sxs-lookup"><span data-stu-id="479b6-1281">Even though the syntax for accessing an indexer element is the same as that for an array element, an indexer element is not classified as a variable.</span></span> <span data-ttu-id="479b6-1282">Proto není možné předat element indexeru jako `ref` argument or. `out`</span><span class="sxs-lookup"><span data-stu-id="479b6-1282">Thus, it is not possible to pass an indexer element as a `ref` or `out` argument.</span></span>

<span data-ttu-id="479b6-1283">Seznam formálních parametrů indexeru definuje podpis ([signatury a přetížení](basic-concepts.md#signatures-and-overloading)) indexeru.</span><span class="sxs-lookup"><span data-stu-id="479b6-1283">The formal parameter list of an indexer defines the signature ([Signatures and overloading](basic-concepts.md#signatures-and-overloading)) of the indexer.</span></span> <span data-ttu-id="479b6-1284">Konkrétně signatura indexeru se skládá z počtu a typů jeho formálních parametrů.</span><span class="sxs-lookup"><span data-stu-id="479b6-1284">Specifically, the signature of an indexer consists of the number and types of its formal parameters.</span></span> <span data-ttu-id="479b6-1285">Typ elementu a názvy formálních parametrů nejsou součástí signatury indexeru.</span><span class="sxs-lookup"><span data-stu-id="479b6-1285">The element type and names of the formal parameters are not part of an indexer's signature.</span></span>

<span data-ttu-id="479b6-1286">Signatura indexeru se musí lišit od signatur všech ostatních indexerů deklarovaných ve stejné třídě.</span><span class="sxs-lookup"><span data-stu-id="479b6-1286">The signature of an indexer must differ from the signatures of all other indexers declared in the same class.</span></span>

<span data-ttu-id="479b6-1287">Indexery a vlastnosti jsou v konceptu velmi podobné, ale liší se následujícími způsoby:</span><span class="sxs-lookup"><span data-stu-id="479b6-1287">Indexers and properties are very similar in concept, but differ in the following ways:</span></span>

*  <span data-ttu-id="479b6-1288">Vlastnost je identifikována názvem, zatímco indexer je identifikován podle jeho signatury.</span><span class="sxs-lookup"><span data-stu-id="479b6-1288">A property is identified by its name, whereas an indexer is identified by its signature.</span></span>
*  <span data-ttu-id="479b6-1289">Vlastnost je přístupná prostřednictvím *simple_name* ([jednoduchých názvů](expressions.md#simple-names)) nebo *member_access* ([přístup členů](expressions.md#member-access)), zatímco element indexeru je přístupný prostřednictvím *element_access* ([přístup indexerem](expressions.md#indexer-access)).</span><span class="sxs-lookup"><span data-stu-id="479b6-1289">A property is accessed through a *simple_name* ([Simple names](expressions.md#simple-names)) or a *member_access* ([Member access](expressions.md#member-access)), whereas an indexer element is accessed through an *element_access* ([Indexer access](expressions.md#indexer-access)).</span></span>
*  <span data-ttu-id="479b6-1290">Vlastnost může být `static` členem, zatímco indexer je vždy členem instance.</span><span class="sxs-lookup"><span data-stu-id="479b6-1290">A property can be a `static` member, whereas an indexer is always an instance member.</span></span>
*  <span data-ttu-id="479b6-1291">Přistupující objekt vlastnosti odpovídá metodě bez parametrů, `get` zatímco přistupující objekt indexeru odpovídá metodě se stejným formálním seznamem parametrů jako indexer. `get`</span><span class="sxs-lookup"><span data-stu-id="479b6-1291">A `get` accessor of a property corresponds to a method with no parameters, whereas a `get` accessor of an indexer corresponds to a method with the same formal parameter list as the indexer.</span></span>
*  <span data-ttu-id="479b6-1292">Přistupující objekt vlastnosti odpovídá metodě s jediným parametrem s názvem `value`, zatímco `set` přistupující objekt indexeru odpovídá metodě se stejným formálním seznamem parametrů jako indexer a další parametr. `set` název `value`.</span><span class="sxs-lookup"><span data-stu-id="479b6-1292">A `set` accessor of a property corresponds to a method with a single parameter named `value`, whereas a `set` accessor of an indexer corresponds to a method with the same formal parameter list as the indexer, plus an additional parameter named `value`.</span></span>
*  <span data-ttu-id="479b6-1293">Jedná se o chybu při kompilaci pro přistupující objekt indexeru pro deklaraci místní proměnné se stejným názvem jako parametr indexeru.</span><span class="sxs-lookup"><span data-stu-id="479b6-1293">It is a compile-time error for an indexer accessor to declare a local variable with the same name as an indexer parameter.</span></span>
*  <span data-ttu-id="479b6-1294">V deklaraci přepsání vlastnosti je zděděná vlastnost k ní přistupovaná `base.P`pomocí syntaxe `P` , kde je název vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="479b6-1294">In an overriding property declaration, the inherited property is accessed using the syntax `base.P`, where `P` is the property name.</span></span> <span data-ttu-id="479b6-1295">V rámci přepsání deklarace indexeru se zděděnému indexeru přistupuje pomocí syntaxe `base[E]`, kde `E` je seznam výrazů oddělených čárkami.</span><span class="sxs-lookup"><span data-stu-id="479b6-1295">In an overriding indexer declaration, the inherited indexer is accessed using the syntax `base[E]`, where `E` is a comma separated list of expressions.</span></span>
*  <span data-ttu-id="479b6-1296">Neexistuje žádný koncept "automaticky implementovaného indexeru".</span><span class="sxs-lookup"><span data-stu-id="479b6-1296">There is no concept of an "automatically implemented indexer".</span></span> <span data-ttu-id="479b6-1297">Není k dispozici chyba pro neabstraktního neexterního indexeru s přístupovými objekty středník.</span><span class="sxs-lookup"><span data-stu-id="479b6-1297">It is an error to have a non-abstract, non-external indexer with semicolon accessors.</span></span>

<span data-ttu-id="479b6-1298">Kromě těchto rozdílů se všechna pravidla definovaná v [přístupových](classes.md#accessors) objektů a [automaticky implementované vlastnosti](classes.md#automatically-implemented-properties) vztahují na přistupující objekty indexeru i na přistupující objekty vlastností.</span><span class="sxs-lookup"><span data-stu-id="479b6-1298">Aside from these differences, all rules defined in [Accessors](classes.md#accessors) and [Automatically implemented properties](classes.md#automatically-implemented-properties) apply to indexer accessors as well as to property accessors.</span></span>

<span data-ttu-id="479b6-1299">Pokud deklarace indexeru obsahuje `extern` modifikátor, indexer je označován jako ***externí indexer***.</span><span class="sxs-lookup"><span data-stu-id="479b6-1299">When an indexer declaration includes an `extern` modifier, the indexer is said to be an ***external indexer***.</span></span> <span data-ttu-id="479b6-1300">Vzhledem k tomu, že externí deklarace indexeru neposkytuje žádnou skutečnou implementaci, každá z jeho *accessor_declarations* se skládá ze střední hodnoty.</span><span class="sxs-lookup"><span data-stu-id="479b6-1300">Because an external indexer declaration provides no actual implementation, each of its *accessor_declarations* consists of a semicolon.</span></span>

<span data-ttu-id="479b6-1301">Následující příklad deklaruje `BitArray` třídu, která implementuje indexer pro přístup k jednotlivým bitům v bitovém poli.</span><span class="sxs-lookup"><span data-stu-id="479b6-1301">The example below declares a `BitArray` class that implements an indexer for accessing the individual bits in the bit array.</span></span>
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

<span data-ttu-id="479b6-1302">Instance `BitArray` třídy spotřebovává podstatně méně paměti než odpovídající `bool[]` (vzhledem k tomu, že každá hodnota bývalého zabírá pouze jeden bit namísto jeho jednoho bajtu), ale umožňuje stejné operace jako `bool[]`.</span><span class="sxs-lookup"><span data-stu-id="479b6-1302">An instance of the `BitArray` class consumes substantially less memory than a corresponding `bool[]` (since each value of the former occupies only one bit instead of the latter's one byte), but it permits the same operations as a `bool[]`.</span></span>

<span data-ttu-id="479b6-1303">Následující `CountPrimes` Třída`BitArray` používá k výpočtu počtu primárních operací mezi 1 a daným maximem klasický algoritmus "síto":</span><span class="sxs-lookup"><span data-stu-id="479b6-1303">The following `CountPrimes` class uses a `BitArray` and the classical "sieve" algorithm to compute the number of primes between 1 and a given maximum:</span></span>
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

<span data-ttu-id="479b6-1304">Všimněte si, že syntaxe pro přístup k elementům `BitArray` je přesně stejná jako `bool[]`u pro.</span><span class="sxs-lookup"><span data-stu-id="479b6-1304">Note that the syntax for accessing elements of the `BitArray` is precisely the same as for a `bool[]`.</span></span>

<span data-ttu-id="479b6-1305">Následující příklad ukazuje třídu mřížky o 26 \* 10, která má indexer se dvěma parametry.</span><span class="sxs-lookup"><span data-stu-id="479b6-1305">The following example shows a 26 \* 10 grid class that has an indexer with two parameters.</span></span> <span data-ttu-id="479b6-1306">První parametr je povinný jako velké nebo malé písmeno v rozsahu A – Z a druhý musí být celé číslo v rozsahu 0-9.</span><span class="sxs-lookup"><span data-stu-id="479b6-1306">The first parameter is required to be an upper- or lowercase letter in the range A-Z, and the second is required to be an integer in the range 0-9.</span></span>

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

### <a name="indexer-overloading"></a><span data-ttu-id="479b6-1307">Přetížení indexeru</span><span class="sxs-lookup"><span data-stu-id="479b6-1307">Indexer overloading</span></span>

<span data-ttu-id="479b6-1308">Pravidla rozlišení přetížení indexeru jsou popsána v tématu [odvození typu](expressions.md#type-inference).</span><span class="sxs-lookup"><span data-stu-id="479b6-1308">The indexer overload resolution rules are described in [Type inference](expressions.md#type-inference).</span></span>

## <a name="operators"></a><span data-ttu-id="479b6-1309">Operátory</span><span class="sxs-lookup"><span data-stu-id="479b6-1309">Operators</span></span>

<span data-ttu-id="479b6-1310">***Operátor*** je člen, který definuje význam operátoru výrazu, který lze použít na instance třídy.</span><span class="sxs-lookup"><span data-stu-id="479b6-1310">An ***operator*** is a member that defines the meaning of an expression operator that can be applied to instances of the class.</span></span> <span data-ttu-id="479b6-1311">Operátory jsou deklarovány pomocí *operator_declaration*s:</span><span class="sxs-lookup"><span data-stu-id="479b6-1311">Operators are declared using *operator_declaration*s:</span></span>

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

<span data-ttu-id="479b6-1312">Existují tři kategorie přetížených operátorů: Unární operátory ([unární operátory](classes.md#unary-operators)), binární operátory ([binární operátory](classes.md#binary-operators)) a operátory převodu ([operátory převodu](classes.md#conversion-operators)).</span><span class="sxs-lookup"><span data-stu-id="479b6-1312">There are three categories of overloadable operators: Unary operators ([Unary operators](classes.md#unary-operators)), binary operators ([Binary operators](classes.md#binary-operators)), and conversion operators ([Conversion operators](classes.md#conversion-operators)).</span></span>

<span data-ttu-id="479b6-1313">*Operator_body* je buď středník, ***text příkazu*** nebo ***tělo výrazu***.</span><span class="sxs-lookup"><span data-stu-id="479b6-1313">The *operator_body* is either a semicolon, a ***statement body*** or an ***expression body***.</span></span> <span data-ttu-id="479b6-1314">Tělo příkazu se skládá z *bloku*, který určuje příkazy, které mají být provedeny při vyvolání operátoru.</span><span class="sxs-lookup"><span data-stu-id="479b6-1314">A statement body consists of a *block*, which specifies the statements to execute when the operator is invoked.</span></span> <span data-ttu-id="479b6-1315">*Blok* musí odpovídat pravidlům pro metody vracející hodnoty popsané v [těle metody](classes.md#method-body).</span><span class="sxs-lookup"><span data-stu-id="479b6-1315">The *block* must conform to the rules for value-returning methods described in [Method body](classes.md#method-body).</span></span> <span data-ttu-id="479b6-1316">Tělo výrazu se skládá `=>` za následováním výrazu a středníkem a označuje jeden výraz, který má být proveden při vyvolání operátoru.</span><span class="sxs-lookup"><span data-stu-id="479b6-1316">An expression body consists of `=>` followed by an expression and a semicolon, and denotes a single expression to perform when the operator is invoked.</span></span>

<span data-ttu-id="479b6-1317">V `extern` případě operátorů se *operator_body* skládá ze středníku.</span><span class="sxs-lookup"><span data-stu-id="479b6-1317">For `extern` operators, the *operator_body* consists simply of a semicolon.</span></span> <span data-ttu-id="479b6-1318">Pro všechny ostatní operátory je *operator_body* buď blokové tělo, nebo tělo výrazu.</span><span class="sxs-lookup"><span data-stu-id="479b6-1318">For all other operators, the *operator_body* is either a block body or an expression body.</span></span>

<span data-ttu-id="479b6-1319">Následující pravidla platí pro všechny deklarace operátorů:</span><span class="sxs-lookup"><span data-stu-id="479b6-1319">The following rules apply to all operator declarations:</span></span>

*  <span data-ttu-id="479b6-1320">Deklarace operátoru musí zahrnovat jak `public` `static` a, tak i modifikátor.</span><span class="sxs-lookup"><span data-stu-id="479b6-1320">An operator declaration must include both a `public` and a `static` modifier.</span></span>
*  <span data-ttu-id="479b6-1321">Parametr (y) operátoru musí být parametry hodnoty ([parametry hodnoty](variables.md#value-parameters)).</span><span class="sxs-lookup"><span data-stu-id="479b6-1321">The parameter(s) of an operator must be value parameters ([Value parameters](variables.md#value-parameters)).</span></span> <span data-ttu-id="479b6-1322">Jedná se o chybu při kompilaci pro deklaraci operátora k určení `ref` nebo `out` parametrů.</span><span class="sxs-lookup"><span data-stu-id="479b6-1322">It is a compile-time error for an operator declaration to specify `ref` or `out` parameters.</span></span>
*  <span data-ttu-id="479b6-1323">Signatura operátoru ([unární operátory](classes.md#unary-operators), [binární operátory](classes.md#binary-operators), [operátory převodu](classes.md#conversion-operators)) se musí lišit od signatur všech ostatních operátorů deklarovaných ve stejné třídě.</span><span class="sxs-lookup"><span data-stu-id="479b6-1323">The signature of an operator ([Unary operators](classes.md#unary-operators), [Binary operators](classes.md#binary-operators), [Conversion operators](classes.md#conversion-operators)) must differ from the signatures of all other operators declared in the same class.</span></span>
*  <span data-ttu-id="479b6-1324">Všechny typy, na které je odkazováno v deklaraci operátoru, musí být alespoň tak přístupné jako samotný operátor ([Omezení přístupnosti](basic-concepts.md#accessibility-constraints)).</span><span class="sxs-lookup"><span data-stu-id="479b6-1324">All types referenced in an operator declaration must be at least as accessible as the operator itself ([Accessibility constraints](basic-concepts.md#accessibility-constraints)).</span></span>
*  <span data-ttu-id="479b6-1325">Je-li stejný modifikátor v deklaraci operátoru uveden několikrát, jedná se o chybu.</span><span class="sxs-lookup"><span data-stu-id="479b6-1325">It is an error for the same modifier to appear multiple times in an operator declaration.</span></span>

<span data-ttu-id="479b6-1326">Každá kategorie operátoru ukládá další omezení, jak je popsáno v následujících částech.</span><span class="sxs-lookup"><span data-stu-id="479b6-1326">Each operator category imposes additional restrictions, as described in the following sections.</span></span>

<span data-ttu-id="479b6-1327">Podobně jako ostatní členové jsou operátory deklarované v základní třídě děděny odvozenými třídami.</span><span class="sxs-lookup"><span data-stu-id="479b6-1327">Like other members, operators declared in a base class are inherited by derived classes.</span></span> <span data-ttu-id="479b6-1328">Vzhledem k tomu, že deklarace operátorů vždy vyžadují třídu nebo strukturu, ve které je operátor deklarován pro účast v Signature operátoru, není možné použít operátor deklarovaný v odvozené třídě pro skrytí operátoru deklarovaného v základní třídě.</span><span class="sxs-lookup"><span data-stu-id="479b6-1328">Because operator declarations always require the class or struct in which the operator is declared to participate in the signature of the operator, it is not possible for an operator declared in a derived class to hide an operator declared in a base class.</span></span> <span data-ttu-id="479b6-1329">`new` Proto modifikátor není nikdy vyžadován a v deklaraci operátoru není nikdy povoleno.</span><span class="sxs-lookup"><span data-stu-id="479b6-1329">Thus, the `new` modifier is never required, and therefore never permitted, in an operator declaration.</span></span>

<span data-ttu-id="479b6-1330">Další informace o unárních a binárních operátorech lze nalézt v [operátorech](expressions.md#operators).</span><span class="sxs-lookup"><span data-stu-id="479b6-1330">Additional information on unary and binary operators can be found in [Operators](expressions.md#operators).</span></span>

<span data-ttu-id="479b6-1331">Další informace o operátorech převodu lze nalézt v [uživatelsky definovaných převodech](conversions.md#user-defined-conversions).</span><span class="sxs-lookup"><span data-stu-id="479b6-1331">Additional information on conversion operators can be found in [User-defined conversions](conversions.md#user-defined-conversions).</span></span>

### <a name="unary-operators"></a><span data-ttu-id="479b6-1332">Unární operátory</span><span class="sxs-lookup"><span data-stu-id="479b6-1332">Unary operators</span></span>

<span data-ttu-id="479b6-1333">Následující pravidla platí pro deklarace unárních operátorů, `T` kde označuje typ instance třídy nebo struktury, která obsahuje deklaraci operátoru:</span><span class="sxs-lookup"><span data-stu-id="479b6-1333">The following rules apply to unary operator declarations, where `T` denotes the instance type of the class or struct that contains the operator declaration:</span></span>

*  <span data-ttu-id="479b6-1334">Unární `+`operátor `T` , `-`, `!` `T?` nebo musímítjedenparametrtypunebomůževracetlibovolnýtyp.`~`</span><span class="sxs-lookup"><span data-stu-id="479b6-1334">A unary `+`, `-`, `!`, or `~` operator must take a single parameter of type `T` or `T?` and can return any type.</span></span>
*  <span data-ttu-id="479b6-1335">Unární `++` operátor OR `--` musí mít jeden parametr typu `T` nebo `T?` a musí vracet stejný typ nebo z něj odvozený typ.</span><span class="sxs-lookup"><span data-stu-id="479b6-1335">A unary `++` or `--` operator must take a single parameter of type `T` or `T?` and must return that same type or a type derived from it.</span></span>
*  <span data-ttu-id="479b6-1336">Unární `true` operátor OR `false` musí mít jeden parametr typu `T` nebo `T?` musí vracet typ `bool`.</span><span class="sxs-lookup"><span data-stu-id="479b6-1336">A unary `true` or `false` operator must take a single parameter of type `T` or `T?` and must return type `bool`.</span></span>

<span data-ttu-id="479b6-1337">Signatura unárního operátoru se skládá z tokenu operátoru `-`( `!``+`, `~`, `++` `--`,, `true`,, `false`nebo) a typu jednoho formálního parametru.</span><span class="sxs-lookup"><span data-stu-id="479b6-1337">The signature of a unary operator consists of the operator token (`+`, `-`, `!`, `~`, `++`, `--`, `true`, or `false`) and the type of the single formal parameter.</span></span> <span data-ttu-id="479b6-1338">Návratový typ není součástí signatury unárního operátoru, ani se nejedná o název formálního parametru.</span><span class="sxs-lookup"><span data-stu-id="479b6-1338">The return type is not part of a unary operator's signature, nor is the name of the formal parameter.</span></span>

<span data-ttu-id="479b6-1339">Unární operátory `false` a vyžadují deklaraci párového páru. `true`</span><span class="sxs-lookup"><span data-stu-id="479b6-1339">The `true` and `false` unary operators require pair-wise declaration.</span></span> <span data-ttu-id="479b6-1340">K chybě v době kompilace dojde, pokud třída deklaruje jeden z těchto operátorů bez nutnosti deklarovat i druhý.</span><span class="sxs-lookup"><span data-stu-id="479b6-1340">A compile-time error occurs if a class declares one of these operators without also declaring the other.</span></span> <span data-ttu-id="479b6-1341">Operátory `true` a`false` jsou podrobněji popsány v [uživatelsky definovaných podmíněných logických operátorech](expressions.md#user-defined-conditional-logical-operators) a [logických výrazech](expressions.md#boolean-expressions).</span><span class="sxs-lookup"><span data-stu-id="479b6-1341">The `true` and `false` operators are described further in [User-defined conditional logical operators](expressions.md#user-defined-conditional-logical-operators) and [Boolean expressions](expressions.md#boolean-expressions).</span></span>

<span data-ttu-id="479b6-1342">Následující příklad ukazuje implementaci a následné použití `operator ++` pro třídu celočíselného vektoru:</span><span class="sxs-lookup"><span data-stu-id="479b6-1342">The following example shows an implementation and subsequent usage of `operator ++` for an integer vector class:</span></span>
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

<span data-ttu-id="479b6-1343">Všimněte si, jak metoda operátora vrací hodnotu vytvořenou přidáním 1 k operandu, stejně jako operátory přírůstku a snížení předpony (operátory přírůstku[a](expressions.md#postfix-increment-and-decrement-operators)snížení předpony) a operátory přírůstku a snížení předpony ([předpona operátory přírůstku a snížení](expressions.md#prefix-increment-and-decrement-operators)).</span><span class="sxs-lookup"><span data-stu-id="479b6-1343">Note how the operator method returns the value produced by adding 1 to the operand, just like the  postfix increment and decrement operators ([Postfix increment and decrement operators](expressions.md#postfix-increment-and-decrement-operators)), and the prefix increment and decrement operators ([Prefix increment and decrement operators](expressions.md#prefix-increment-and-decrement-operators)).</span></span> <span data-ttu-id="479b6-1344">Na rozdíl od C++, tato metoda nemusí změnit hodnotu svého operandu přímo.</span><span class="sxs-lookup"><span data-stu-id="479b6-1344">Unlike in C++, this method need not modify the value of its operand directly.</span></span> <span data-ttu-id="479b6-1345">Ve skutečnosti Změna hodnoty operandu by narušila standardní sémantiku operátoru přírůstku.</span><span class="sxs-lookup"><span data-stu-id="479b6-1345">In fact, modifying the operand value would violate the standard semantics of the postfix increment operator.</span></span>

### <a name="binary-operators"></a><span data-ttu-id="479b6-1346">Binární operátory</span><span class="sxs-lookup"><span data-stu-id="479b6-1346">Binary operators</span></span>

<span data-ttu-id="479b6-1347">Následující pravidla platí pro deklarace binárních operátorů, `T` kde označuje typ instance třídy nebo struktury, která obsahuje deklaraci operátoru:</span><span class="sxs-lookup"><span data-stu-id="479b6-1347">The following rules apply to binary operator declarations, where `T` denotes the instance type of the class or struct that contains the operator declaration:</span></span>

*  <span data-ttu-id="479b6-1348">Binární operátor bez posunutí musí mít dva parametry, minimálně jeden z nich musí mít typ `T` nebo `T?`a může vracet libovolný typ.</span><span class="sxs-lookup"><span data-stu-id="479b6-1348">A binary non-shift operator must take two parameters, at least one of which must have type `T` or `T?`, and can return any type.</span></span>
*  <span data-ttu-id="479b6-1349">`<<` Binární nebo `T?` `T` `int?` `int` operátor musí mít dva parametry, první z nich musí mít typ nebo a druhý z nich musí mít typ nebo a může vracet libovolný typ. `>>`</span><span class="sxs-lookup"><span data-stu-id="479b6-1349">A binary `<<` or `>>` operator must take two parameters, the first of which must have type `T` or `T?` and the second of which must have type `int` or `int?`, and can return any type.</span></span>

<span data-ttu-id="479b6-1350">Signatura binárního operátoru se skládá z tokenu operátoru `-`( `*``+`, `/`, `%`, `&`, `|`, `^`, `<<`, `>>`,,, `==`, `!=`, ,,`>`nebo )atypydvou`<=`formálníchparametrů. `<` `>=`</span><span class="sxs-lookup"><span data-stu-id="479b6-1350">The signature of a binary operator consists of the operator token (`+`, `-`, `*`, `/`, `%`, `&`, `|`, `^`, `<<`, `>>`, `==`, `!=`, `>`, `<`, `>=`, or `<=`) and the types of the two formal parameters.</span></span> <span data-ttu-id="479b6-1351">Návratový typ a názvy formálních parametrů nejsou součástí signatury binárního operátoru.</span><span class="sxs-lookup"><span data-stu-id="479b6-1351">The return type and the names of the formal parameters are not part of a binary operator's signature.</span></span>

<span data-ttu-id="479b6-1352">Některé binární operátory vyžadují párové deklarace.</span><span class="sxs-lookup"><span data-stu-id="479b6-1352">Certain binary operators require pair-wise declaration.</span></span> <span data-ttu-id="479b6-1353">Pro každou deklaraci každého operátoru páru musí existovat deklarace, která odpovídá druhému operátoru.</span><span class="sxs-lookup"><span data-stu-id="479b6-1353">For every declaration of either operator of a pair, there must be a matching declaration of the other operator of the pair.</span></span> <span data-ttu-id="479b6-1354">Dvě deklarace operátoru se shodují, pokud mají stejný návratový typ a stejný typ pro každý parametr.</span><span class="sxs-lookup"><span data-stu-id="479b6-1354">Two operator declarations match when they have the same return type and the same type for each parameter.</span></span> <span data-ttu-id="479b6-1355">Následující operátory vyžadují párové deklarace:</span><span class="sxs-lookup"><span data-stu-id="479b6-1355">The following operators require pair-wise declaration:</span></span>

*  <span data-ttu-id="479b6-1356">`operator ==` a `operator !=`</span><span class="sxs-lookup"><span data-stu-id="479b6-1356">`operator ==` and `operator !=`</span></span>
*  <span data-ttu-id="479b6-1357">`operator >` a `operator <`</span><span class="sxs-lookup"><span data-stu-id="479b6-1357">`operator >` and `operator <`</span></span>
*  <span data-ttu-id="479b6-1358">`operator >=` a `operator <=`</span><span class="sxs-lookup"><span data-stu-id="479b6-1358">`operator >=` and `operator <=`</span></span>

### <a name="conversion-operators"></a><span data-ttu-id="479b6-1359">Operátory převodu</span><span class="sxs-lookup"><span data-stu-id="479b6-1359">Conversion operators</span></span>

<span data-ttu-id="479b6-1360">Deklarace operátoru převodu zavádí ***uživatelsky definovaný převod*** ([uživatelsky definované převody](conversions.md#user-defined-conversions)), které rozšiřují předdefinované implicitní a explicitní převody.</span><span class="sxs-lookup"><span data-stu-id="479b6-1360">A conversion operator declaration introduces a ***user-defined conversion*** ([User-defined conversions](conversions.md#user-defined-conversions)) which augments the pre-defined implicit and explicit conversions.</span></span>

<span data-ttu-id="479b6-1361">Deklarace operátoru převodu, která zahrnuje `implicit` klíčové slovo zavádí uživatelem definovaný implicitní převod.</span><span class="sxs-lookup"><span data-stu-id="479b6-1361">A conversion operator declaration that includes the `implicit` keyword introduces a user-defined implicit conversion.</span></span> <span data-ttu-id="479b6-1362">Implicitní převody mohou nastat v různých situacích, včetně vyvolání členů funkce, výrazů přetypování a přiřazení.</span><span class="sxs-lookup"><span data-stu-id="479b6-1362">Implicit conversions can occur in a variety of situations, including function member invocations, cast expressions, and assignments.</span></span> <span data-ttu-id="479b6-1363">Tento postup je popsán dále v tématu [implicitní převody](conversions.md#implicit-conversions).</span><span class="sxs-lookup"><span data-stu-id="479b6-1363">This is described further in [Implicit conversions](conversions.md#implicit-conversions).</span></span>

<span data-ttu-id="479b6-1364">Deklarace operátoru převodu, která zahrnuje `explicit` klíčové slovo zavádí uživatelem definovaný explicitní převod.</span><span class="sxs-lookup"><span data-stu-id="479b6-1364">A conversion operator declaration that includes the `explicit` keyword introduces a user-defined explicit conversion.</span></span> <span data-ttu-id="479b6-1365">Explicitní převody mohou nastat ve výrazech přetypování a jsou popsány dále v tématu [explicitní převody](conversions.md#explicit-conversions).</span><span class="sxs-lookup"><span data-stu-id="479b6-1365">Explicit conversions can occur in cast expressions, and are described further in [Explicit conversions](conversions.md#explicit-conversions).</span></span>

<span data-ttu-id="479b6-1366">Operátor převodu se převede ze zdrojového typu určeného parametrem typu operátoru převodu na cílový typ, který je označen návratovým typem operátoru převodu.</span><span class="sxs-lookup"><span data-stu-id="479b6-1366">A conversion operator converts from a source type, indicated by the parameter type of the conversion operator, to a target type, indicated by the return type of the conversion operator.</span></span>

<span data-ttu-id="479b6-1367">Pro `S` daný typ zdroje a cílový typ `T`, pokud `S` nebo `T` jsou typy s možnou hodnotou `S0` null `T0` , umožňují a odkazují na jejich základní `S0` typy `T0` , jinak a je rovno `T`avuvedenémpořadí `S` .</span><span class="sxs-lookup"><span data-stu-id="479b6-1367">For a given source type `S` and target type `T`, if `S` or `T` are nullable types, let `S0` and `T0` refer to their underlying types, otherwise `S0` and `T0` are equal to `S` and `T` respectively.</span></span> <span data-ttu-id="479b6-1368">Třída nebo struktura je oprávněna deklarovat převod ze zdrojového typu `S` na cílový typ `T` pouze v případě, že jsou splněny všechny následující podmínky:</span><span class="sxs-lookup"><span data-stu-id="479b6-1368">A class or struct is permitted to declare a conversion from a source type `S` to a target type `T` only if all of the following are true:</span></span>

*  <span data-ttu-id="479b6-1369">`S0`a `T0` jsou různé typy.</span><span class="sxs-lookup"><span data-stu-id="479b6-1369">`S0` and `T0` are different types.</span></span>
*  <span data-ttu-id="479b6-1370">Buď `S0` nebo`T0` je typem třídy nebo struktury, ve které se provádí deklarace operátoru.</span><span class="sxs-lookup"><span data-stu-id="479b6-1370">Either `S0` or `T0` is the class or struct type in which the operator declaration takes place.</span></span>
*  <span data-ttu-id="479b6-1371">Ani se nejedná o *INTERFACE_TYPE.* `S0` `T0`</span><span class="sxs-lookup"><span data-stu-id="479b6-1371">Neither `S0` nor `T0` is an *interface_type*.</span></span>
*  <span data-ttu-id="479b6-1372">S výjimkou uživatelem `S` definovaných převodů neexistuje převod z `T` na nebo z `T` na `S`.</span><span class="sxs-lookup"><span data-stu-id="479b6-1372">Excluding user-defined conversions, a conversion does not exist from `S` to `T` or from `T` to `S`.</span></span>

<span data-ttu-id="479b6-1373">Pro účely těchto pravidel jsou všechny parametry typu přidružené `S` k nebo `T` považovány za jedinečné typy, které nemají žádný vztah dědičnosti s jinými typy, a jakákoli omezení u těchto parametrů typu jsou ignorována.</span><span class="sxs-lookup"><span data-stu-id="479b6-1373">For the purposes of these rules, any type parameters associated with `S` or `T` are considered to be unique types that have no inheritance relationship with other types, and any constraints on those type parameters are ignored.</span></span>

<span data-ttu-id="479b6-1374">V příkladu</span><span class="sxs-lookup"><span data-stu-id="479b6-1374">In the example</span></span>
```csharp
class C<T> {...}

class D<T>: C<T>
{
    public static implicit operator C<int>(D<T> value) {...}      // Ok
    public static implicit operator C<string>(D<T> value) {...}   // Ok
    public static implicit operator C<T>(D<T> value) {...}        // Error
}
```
<span data-ttu-id="479b6-1375">první dva deklarace operátoru jsou povoleny, protože pro účely [indexerů](classes.md#indexers).3 `T` a `int` `string` v je považovány za jedinečné typy bez vztahu.</span><span class="sxs-lookup"><span data-stu-id="479b6-1375">the first two operator declarations are permitted because, for the purposes of [Indexers](classes.md#indexers).3, `T` and `int` and `string` respectively are considered unique types with no relationship.</span></span> <span data-ttu-id="479b6-1376">Třetí operátor je však chybou, protože `C<T>` je základní `D<T>`třídou třídy.</span><span class="sxs-lookup"><span data-stu-id="479b6-1376">However, the third operator is an error because `C<T>` is the base class of `D<T>`.</span></span>

<span data-ttu-id="479b6-1377">Od druhého pravidla následuje, že operátor převodu musí převést buď na nebo z třídy nebo typu struktury, ve které je operátor deklarován.</span><span class="sxs-lookup"><span data-stu-id="479b6-1377">From the second rule it follows that a conversion operator must convert either to or from the class or struct type in which the operator is declared.</span></span> <span data-ttu-id="479b6-1378">Například je `C` možné třídu nebo typ struktury definovat převod z `C` na `int` a z `int` na `C`, ale ne z `int` na `bool`.</span><span class="sxs-lookup"><span data-stu-id="479b6-1378">For example, it is possible for a class or struct type `C` to define a conversion from `C` to `int` and from `int` to `C`, but not from `int` to `bool`.</span></span>

<span data-ttu-id="479b6-1379">Není možné přímo znovu definovat předem definovaný převod.</span><span class="sxs-lookup"><span data-stu-id="479b6-1379">It is not possible to directly redefine a pre-defined conversion.</span></span> <span data-ttu-id="479b6-1380">Proto operátory převodu nepovolují převod z nebo na `object` , protože implicitní a explicitní převody již existují mezi `object` a všemi ostatními typy.</span><span class="sxs-lookup"><span data-stu-id="479b6-1380">Thus, conversion operators are not allowed to convert from or to `object` because implicit and explicit conversions already exist between `object` and all other types.</span></span> <span data-ttu-id="479b6-1381">Stejně tak, ani zdrojový ani cílový typ převodu může být základní typ druhého, protože převod by pak již existoval.</span><span class="sxs-lookup"><span data-stu-id="479b6-1381">Likewise, neither the source nor the target types of a conversion can be a base type of the other, since a conversion would then already exist.</span></span>

<span data-ttu-id="479b6-1382">Je však možné deklarovat operátory u obecných typů, které pro konkrétní argumenty typu určují převody, které již existují jako předdefinované převody.</span><span class="sxs-lookup"><span data-stu-id="479b6-1382">However, it is possible to declare operators on generic types that, for particular type arguments, specify conversions that already exist as pre-defined conversions.</span></span> <span data-ttu-id="479b6-1383">V příkladu</span><span class="sxs-lookup"><span data-stu-id="479b6-1383">In the example</span></span>
```csharp
struct Convertible<T>
{
    public static implicit operator Convertible<T>(T value) {...}
    public static explicit operator T(Convertible<T> value) {...}
}
```
<span data-ttu-id="479b6-1384">Pokud je `object` typ zadán jako argument typu pro `T`, druhý operátor deklaruje převod, který již existuje (implicitní, a tedy také explicitní, převod existuje z libovolného typu na typ `object`).</span><span class="sxs-lookup"><span data-stu-id="479b6-1384">when type `object` is specified as a type argument for `T`, the second operator declares a conversion that already exists (an implicit, and therefore also an explicit, conversion exists from any type to type `object`).</span></span>

<span data-ttu-id="479b6-1385">V případech, kdy existuje předem definovaný převod mezi dvěma typy, jsou ignorovány uživatelsky definované převody mezi těmito typy.</span><span class="sxs-lookup"><span data-stu-id="479b6-1385">In cases where a pre-defined conversion exists between two types, any user-defined conversions between those types are ignored.</span></span> <span data-ttu-id="479b6-1386">Určen</span><span class="sxs-lookup"><span data-stu-id="479b6-1386">Specifically:</span></span>

*  <span data-ttu-id="479b6-1387">Pokud existuje předem definovaný implicitní převod ([implicitní převody](conversions.md#implicit-conversions)) `S` z typu na typ `T`, všechny uživatelsky definované převody (implicitní nebo explicitní) z `S` na `T` jsou ignorovány.</span><span class="sxs-lookup"><span data-stu-id="479b6-1387">If a pre-defined implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)) exists from type `S` to type `T`, all user-defined conversions (implicit or explicit) from `S` to `T` are ignored.</span></span>
*  <span data-ttu-id="479b6-1388">Pokud existuje předem definovaný explicitní převod ([explicitní převody](conversions.md#explicit-conversions)) `S` z typu na typ `T`, všechny uživatelem definované explicitní převody z `S` na `T` jsou ignorovány.</span><span class="sxs-lookup"><span data-stu-id="479b6-1388">If a pre-defined explicit conversion ([Explicit conversions](conversions.md#explicit-conversions)) exists from type `S` to type `T`, any user-defined explicit conversions from `S` to `T` are ignored.</span></span> <span data-ttu-id="479b6-1389">Mimoto</span><span class="sxs-lookup"><span data-stu-id="479b6-1389">Furthermore:</span></span>

<span data-ttu-id="479b6-1390">Pokud `T` je typ rozhraní, uživatelem definované implicitní převody z `S` na `T` jsou ignorovány.</span><span class="sxs-lookup"><span data-stu-id="479b6-1390">If `T` is an interface type, user-defined implicit conversions from `S` to `T` are ignored.</span></span>

<span data-ttu-id="479b6-1391">V opačném případě uživatelem definované implicitní převody `S` z `T` na na jsou stále považovány za.</span><span class="sxs-lookup"><span data-stu-id="479b6-1391">Otherwise, user-defined implicit conversions from `S` to `T` are still considered.</span></span>

<span data-ttu-id="479b6-1392">Pro všechny typy, `object`ale operátory deklarované `Convertible<T>` výše uvedeným typem nejsou v konfliktu s předem definovanými převody.</span><span class="sxs-lookup"><span data-stu-id="479b6-1392">For all types but `object`, the operators declared by the `Convertible<T>` type above do not conflict with pre-defined conversions.</span></span> <span data-ttu-id="479b6-1393">Příklad:</span><span class="sxs-lookup"><span data-stu-id="479b6-1393">For example:</span></span>
```csharp
void F(int i, Convertible<int> n) {
    i = n;                          // Error
    i = (int)n;                     // User-defined explicit conversion
    n = i;                          // User-defined implicit conversion
    n = (Convertible<int>)i;        // User-defined implicit conversion
}
```

<span data-ttu-id="479b6-1394">Nicméně u typů `object`, předem definovaných převodů skryjte uživatelsky definované převody ve všech případech, ale jednu z těchto hodnot:</span><span class="sxs-lookup"><span data-stu-id="479b6-1394">However, for type `object`, pre-defined conversions hide the user-defined conversions in all cases but one:</span></span>

```csharp
void F(object o, Convertible<object> n) {
    o = n;                         // Pre-defined boxing conversion
    o = (object)n;                 // Pre-defined boxing conversion
    n = o;                         // User-defined implicit conversion
    n = (Convertible<object>)o;    // Pre-defined unboxing conversion
}
```

<span data-ttu-id="479b6-1395">Uživatelem definované převody nejsou povoleny pro převod z nebo na *INTERFACE_TYPE*s.</span><span class="sxs-lookup"><span data-stu-id="479b6-1395">User-defined conversions are not allowed to convert from or to *interface_type*s.</span></span> <span data-ttu-id="479b6-1396">Konkrétně toto omezení zajistí, že při převodu na *INTERFACE_TYPE*nedochází k žádným uživatelsky definovaným transformacím a převod na *INTERFACE_TYPE* se zdaří jenom v případě, že převedený objekt ve skutečnosti implementuje. zadaný *INTERFACE_TYPE*.</span><span class="sxs-lookup"><span data-stu-id="479b6-1396">In particular, this restriction ensures that no user-defined transformations occur when converting to an *interface_type*, and that a conversion to an *interface_type* succeeds only if the object being converted actually implements the specified *interface_type*.</span></span>

<span data-ttu-id="479b6-1397">Signatura operátoru převodu se skládá ze zdrojového typu a cílového typu.</span><span class="sxs-lookup"><span data-stu-id="479b6-1397">The signature of a conversion operator consists of the source type and the target type.</span></span> <span data-ttu-id="479b6-1398">(Všimněte si, že se jedná o jedinou formu člena, pro který se v signatuře účastní návratový typ.) Klasifikace `implicit` nebo`explicit` operátor převodu není součástí signatury operátoru.</span><span class="sxs-lookup"><span data-stu-id="479b6-1398">(Note that this is the only form of member for which the return type participates in the signature.) The `implicit` or `explicit` classification of a conversion operator is not part of the operator's signature.</span></span> <span data-ttu-id="479b6-1399">Třída nebo struktura proto nemůže deklarovat `implicit` operátor `explicit` převodu a operátor převodu se stejnými zdrojovými a cílovými typy.</span><span class="sxs-lookup"><span data-stu-id="479b6-1399">Thus, a class or struct cannot declare both an `implicit` and an `explicit` conversion operator with the same source and target types.</span></span>

<span data-ttu-id="479b6-1400">Obecně platí, že uživatelsky definované implicitní převody by měly být navržené tak, aby nikdy nevolaly výjimky a nikdy neztratily informace.</span><span class="sxs-lookup"><span data-stu-id="479b6-1400">In general, user-defined implicit conversions should be designed to never throw exceptions and never lose information.</span></span> <span data-ttu-id="479b6-1401">Pokud uživatelsky definovaný převod může vést k výjimkám (například protože zdrojový argument je mimo rozsah) nebo ztratil informace (například zahození vysokého počtu bitů), pak tento převod by měl být definován jako explicitní převod.</span><span class="sxs-lookup"><span data-stu-id="479b6-1401">If a user-defined conversion can give rise to exceptions (for example, because the source argument is out of range) or loss of information (such as discarding high-order bits), then that conversion should be defined as an explicit conversion.</span></span>

<span data-ttu-id="479b6-1402">V příkladu</span><span class="sxs-lookup"><span data-stu-id="479b6-1402">In the example</span></span>
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
<span data-ttu-id="479b6-1403">`Digit` převod z `Digit` na `byte` je implicitní, protože nikdy nevyvolává výjimky nebo ztratí informace, ale převod z `byte` na `Digit` je explicitní, protože může představovat pouze podmnožinu možných `byte`hodnoty.</span><span class="sxs-lookup"><span data-stu-id="479b6-1403">the conversion from `Digit` to `byte` is implicit because it never throws exceptions or loses information, but the conversion from `byte` to `Digit` is explicit since `Digit` can only represent a subset of the possible values of a `byte`.</span></span>

## <a name="instance-constructors"></a><span data-ttu-id="479b6-1404">Konstruktory instancí</span><span class="sxs-lookup"><span data-stu-id="479b6-1404">Instance constructors</span></span>

<span data-ttu-id="479b6-1405">***Konstruktor instance*** je člen, který implementuje akce vyžadované pro inicializaci instance třídy.</span><span class="sxs-lookup"><span data-stu-id="479b6-1405">An ***instance constructor*** is a member that implements the actions required to initialize an instance of a class.</span></span> <span data-ttu-id="479b6-1406">Konstruktory instancí jsou deklarovány pomocí *constructor_declaration*s:</span><span class="sxs-lookup"><span data-stu-id="479b6-1406">Instance constructors are declared using *constructor_declaration*s:</span></span>

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

<span data-ttu-id="479b6-1407">*Constructor_declaration* může zahrnovat sadu *atributů* ([atributů](attributes.md)), platnou kombinaci modifikátorů přístupu ke čtyřem `extern` ([modifikátory přístupu](classes.md#access-modifiers)) a modifikátor ([externí metody](classes.md#external-methods)).</span><span class="sxs-lookup"><span data-stu-id="479b6-1407">A *constructor_declaration* may include a set of *attributes* ([Attributes](attributes.md)), a valid combination of the four access modifiers ([Access modifiers](classes.md#access-modifiers)), and an `extern` ([External methods](classes.md#external-methods)) modifier.</span></span> <span data-ttu-id="479b6-1408">Deklarace konstruktoru nemůže zahrnovat stejný modifikátor víckrát.</span><span class="sxs-lookup"><span data-stu-id="479b6-1408">A constructor declaration is not permitted to include the same modifier multiple times.</span></span>

<span data-ttu-id="479b6-1409">*Identifikátor* *constructor_declarator* musí pojmenovat třídu, ve které je deklarován konstruktor instance.</span><span class="sxs-lookup"><span data-stu-id="479b6-1409">The *identifier* of a *constructor_declarator* must name the class in which the instance constructor is declared.</span></span> <span data-ttu-id="479b6-1410">Pokud je zadán jiný název, dojde k chybě při kompilaci.</span><span class="sxs-lookup"><span data-stu-id="479b6-1410">If any other name is specified, a compile-time error occurs.</span></span>

<span data-ttu-id="479b6-1411">Volitelná *formal_parameter_list* konstruktoru instance podléhá stejným pravidlům jako *Formal_parameter_list* metody ([metody](classes.md#methods)).</span><span class="sxs-lookup"><span data-stu-id="479b6-1411">The optional *formal_parameter_list* of an instance constructor is subject to the same rules as the *formal_parameter_list* of a method ([Methods](classes.md#methods)).</span></span> <span data-ttu-id="479b6-1412">Seznam formálních parametrů definuje podpis ([signatury a přetížení](basic-concepts.md#signatures-and-overloading)) konstruktoru instance a řídí proces, při kterém řešení přetížení ([odvození typu](expressions.md#type-inference)) vybere konkrétní konstruktor instance ve volání.</span><span class="sxs-lookup"><span data-stu-id="479b6-1412">The formal parameter list defines the signature ([Signatures and overloading](basic-concepts.md#signatures-and-overloading)) of an instance constructor and governs the process whereby overload resolution ([Type inference](expressions.md#type-inference)) selects a particular instance constructor in an invocation.</span></span>

<span data-ttu-id="479b6-1413">Každý typ odkazovaný v *formal_parameter_list* konstruktoru instance musí být alespoň tak přístupný jako samotný konstruktor ([Omezení přístupnosti](basic-concepts.md#accessibility-constraints)).</span><span class="sxs-lookup"><span data-stu-id="479b6-1413">Each of the types referenced in the *formal_parameter_list* of an instance constructor must be at least as accessible as the constructor itself ([Accessibility constraints](basic-concepts.md#accessibility-constraints)).</span></span>

<span data-ttu-id="479b6-1414">Nepovinná *constructor_initializer* určuje jiný konstruktor instance, který se má vyvolat před spuštěním příkazů uvedených v *constructor_body* tohoto konstruktoru instance.</span><span class="sxs-lookup"><span data-stu-id="479b6-1414">The optional *constructor_initializer* specifies another instance constructor to invoke before executing the statements given in the *constructor_body* of this instance constructor.</span></span> <span data-ttu-id="479b6-1415">Tento postup je popsán dále v [inicializátorech konstruktoru](classes.md#constructor-initializers).</span><span class="sxs-lookup"><span data-stu-id="479b6-1415">This is described further in [Constructor initializers](classes.md#constructor-initializers).</span></span>

<span data-ttu-id="479b6-1416">Když deklarace konstruktoru obsahuje `extern` modifikátor, konstruktor je označován jako ***externí konstruktor***.</span><span class="sxs-lookup"><span data-stu-id="479b6-1416">When a constructor declaration includes an `extern` modifier, the constructor is said to be an ***external constructor***.</span></span> <span data-ttu-id="479b6-1417">Vzhledem k tomu, že deklarace externího konstruktoru neposkytuje žádnou skutečnou implementaci, její *constructor_body* se skládá ze střední hodnoty.</span><span class="sxs-lookup"><span data-stu-id="479b6-1417">Because an external constructor declaration provides no actual implementation, its *constructor_body* consists of a semicolon.</span></span> <span data-ttu-id="479b6-1418">Pro všechny ostatní konstruktory se *constructor_body* skládá z *bloku* , který určuje příkazy pro inicializaci nové instance třídy.</span><span class="sxs-lookup"><span data-stu-id="479b6-1418">For all other constructors, the *constructor_body* consists of a *block* which specifies the statements to initialize a new instance of the class.</span></span> <span data-ttu-id="479b6-1419">To odpovídá přesně *bloku* metody instance s `void` návratovým typem ([tělo metody](classes.md#method-body)).</span><span class="sxs-lookup"><span data-stu-id="479b6-1419">This corresponds exactly to the *block* of an instance method with a `void` return type ([Method body](classes.md#method-body)).</span></span>

<span data-ttu-id="479b6-1420">Konstruktory instancí nejsou děděny.</span><span class="sxs-lookup"><span data-stu-id="479b6-1420">Instance constructors are not inherited.</span></span> <span data-ttu-id="479b6-1421">Třída proto nemá žádné konstruktory instancí jiné než ty, které jsou ve skutečnosti deklarovány ve třídě.</span><span class="sxs-lookup"><span data-stu-id="479b6-1421">Thus, a class has no instance constructors other than those actually declared in the class.</span></span> <span data-ttu-id="479b6-1422">Pokud třída neobsahuje deklarace konstruktoru instance, je automaticky poskytnut výchozí konstruktor instance ([výchozí konstruktory](classes.md#default-constructors)).</span><span class="sxs-lookup"><span data-stu-id="479b6-1422">If a class contains no instance constructor declarations, a default instance constructor is automatically provided ([Default constructors](classes.md#default-constructors)).</span></span>

<span data-ttu-id="479b6-1423">Konstruktory instancí jsou vyvolány pomocí *object_creation_expression*s ([výrazy vytváření objektů](expressions.md#object-creation-expressions)) a prostřednictvím *constructor_initializer*s.</span><span class="sxs-lookup"><span data-stu-id="479b6-1423">Instance constructors are invoked by *object_creation_expression*s ([Object creation expressions](expressions.md#object-creation-expressions)) and through *constructor_initializer*s.</span></span>

### <a name="constructor-initializers"></a><span data-ttu-id="479b6-1424">Inicializátory konstruktoru</span><span class="sxs-lookup"><span data-stu-id="479b6-1424">Constructor initializers</span></span>

<span data-ttu-id="479b6-1425">Všechny konstruktory instancí (kromě těch pro třídu `object`) implicitně zahrnují vyvolání jiného konstruktoru instance těsně před *constructor_body*.</span><span class="sxs-lookup"><span data-stu-id="479b6-1425">All instance constructors (except those for class `object`) implicitly include an invocation of another instance constructor immediately before the *constructor_body*.</span></span> <span data-ttu-id="479b6-1426">Konstruktor pro implicitní vyvolání je určen *constructor_initializer*:</span><span class="sxs-lookup"><span data-stu-id="479b6-1426">The constructor to implicitly invoke is determined by the *constructor_initializer*:</span></span>

*  <span data-ttu-id="479b6-1427">Inicializátor konstruktoru instance formuláře `base(argument_list)` nebo `base()` způsobí vyvolání konstruktoru instance z přímé základní třídy.</span><span class="sxs-lookup"><span data-stu-id="479b6-1427">An instance constructor initializer of the form `base(argument_list)` or `base()` causes an instance constructor from the direct base class to be invoked.</span></span> <span data-ttu-id="479b6-1428">Tento konstruktor je vybrán pomocí *argument_list* , pokud je k dispozici, a pravidla rozlišení přetížení pro [rozlišení přetěžování](expressions.md#overload-resolution).</span><span class="sxs-lookup"><span data-stu-id="479b6-1428">That constructor is selected using *argument_list* if present and the overload resolution rules of [Overload resolution](expressions.md#overload-resolution).</span></span> <span data-ttu-id="479b6-1429">Sada konstruktorů instance kandidátů se skládá ze všech přístupných konstruktorů instancí obsažených v přímé základní třídě nebo výchozího konstruktoru ([výchozí konstruktory](classes.md#default-constructors)), pokud nejsou deklarovány žádné konstruktory instancí v přímé základní třídě.</span><span class="sxs-lookup"><span data-stu-id="479b6-1429">The set of candidate instance constructors consists of all accessible instance constructors contained in the direct base class, or the default constructor ([Default constructors](classes.md#default-constructors)), if no instance constructors are declared in the direct base class.</span></span> <span data-ttu-id="479b6-1430">Pokud je tato sada prázdná nebo pokud nelze identifikovat jeden nejlepší konstruktor instance, dojde k chybě při kompilaci.</span><span class="sxs-lookup"><span data-stu-id="479b6-1430">If this set is empty, or if a single best instance constructor cannot be identified, a compile-time error occurs.</span></span>
*  <span data-ttu-id="479b6-1431">Inicializátor konstruktoru instance formuláře `this(argument-list)` nebo `this()` způsobí vyvolání konstruktoru instance ze samotné třídy.</span><span class="sxs-lookup"><span data-stu-id="479b6-1431">An instance constructor initializer of the form `this(argument-list)` or `this()` causes an instance constructor from the class itself to be invoked.</span></span> <span data-ttu-id="479b6-1432">Konstruktor je vybrán pomocí *argument_list* , pokud je k dispozici, a pravidla rozlišení přetížení pro [rozlišení přetěžování](expressions.md#overload-resolution).</span><span class="sxs-lookup"><span data-stu-id="479b6-1432">The constructor is selected using *argument_list* if present and the overload resolution rules of [Overload resolution](expressions.md#overload-resolution).</span></span> <span data-ttu-id="479b6-1433">Sada konstruktorů instancí kandidátů se skládá ze všech přístupných konstruktorů instancí deklarovaných ve třídě samotné.</span><span class="sxs-lookup"><span data-stu-id="479b6-1433">The set of candidate instance constructors consists of all accessible instance constructors declared in the class itself.</span></span> <span data-ttu-id="479b6-1434">Pokud je tato sada prázdná nebo pokud nelze identifikovat jeden nejlepší konstruktor instance, dojde k chybě při kompilaci.</span><span class="sxs-lookup"><span data-stu-id="479b6-1434">If this set is empty, or if a single best instance constructor cannot be identified, a compile-time error occurs.</span></span> <span data-ttu-id="479b6-1435">Pokud deklarace konstruktoru instance obsahuje inicializátor konstruktoru, který vyvolá samotný konstruktor, dojde k chybě při kompilaci.</span><span class="sxs-lookup"><span data-stu-id="479b6-1435">If an instance constructor declaration includes a constructor initializer that invokes the constructor itself, a compile-time error occurs.</span></span>

<span data-ttu-id="479b6-1436">Pokud konstruktor instance nemá inicializátor konstruktoru, implicitně se poskytne inicializátor konstruktoru formuláře `base()` .</span><span class="sxs-lookup"><span data-stu-id="479b6-1436">If an instance constructor has no constructor initializer, a constructor initializer of the form `base()` is implicitly provided.</span></span> <span data-ttu-id="479b6-1437">Proto deklarace konstruktoru instance formuláře</span><span class="sxs-lookup"><span data-stu-id="479b6-1437">Thus, an instance constructor declaration of the form</span></span>
```csharp
C(...) {...}
```
<span data-ttu-id="479b6-1438">je přesně stejný jako</span><span class="sxs-lookup"><span data-stu-id="479b6-1438">is exactly equivalent to</span></span>
```csharp
C(...): base() {...}
```

<span data-ttu-id="479b6-1439">Rozsah parametrů předaných *formal_parameter_list* deklarace konstruktoru instance zahrnuje inicializátor konstruktoru této deklarace.</span><span class="sxs-lookup"><span data-stu-id="479b6-1439">The scope of the parameters given by the *formal_parameter_list* of an instance constructor declaration includes the constructor initializer of that declaration.</span></span> <span data-ttu-id="479b6-1440">Proto inicializátor konstruktoru má povolen přístup k parametrům konstruktoru.</span><span class="sxs-lookup"><span data-stu-id="479b6-1440">Thus, a constructor initializer is permitted to access the parameters of the constructor.</span></span> <span data-ttu-id="479b6-1441">Příklad:</span><span class="sxs-lookup"><span data-stu-id="479b6-1441">For example:</span></span>
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

<span data-ttu-id="479b6-1442">Inicializátor konstruktoru instance nemá přístup k instanci, kterou vytváříte.</span><span class="sxs-lookup"><span data-stu-id="479b6-1442">An instance constructor initializer cannot access the instance being created.</span></span> <span data-ttu-id="479b6-1443">Proto se jedná o chybu `this` při kompilaci v rámci výrazu argumentu inicializátoru konstruktoru, jako je chyba při kompilaci pro výraz argumentu na odkazování na člen instance pomocí *simple_name*.</span><span class="sxs-lookup"><span data-stu-id="479b6-1443">Therefore it is a compile-time error to reference `this` in an argument expression of the constructor initializer, as is it a compile-time error for an argument expression to reference any instance member through a *simple_name*.</span></span>

### <a name="instance-variable-initializers"></a><span data-ttu-id="479b6-1444">Inicializátory proměnných instance</span><span class="sxs-lookup"><span data-stu-id="479b6-1444">Instance variable initializers</span></span>

<span data-ttu-id="479b6-1445">Pokud konstruktor instance nemá inicializátor konstruktoru, nebo má inicializátor konstruktoru formuláře `base(...)`, tento konstruktor implicitně provede inicializace určené *variable_initializer*s v polích instance. deklarováno v jeho třídě.</span><span class="sxs-lookup"><span data-stu-id="479b6-1445">When an instance constructor has no constructor initializer, or it has a constructor initializer of the form `base(...)`, that constructor implicitly performs the initializations specified by the *variable_initializer*s of the instance fields declared in its class.</span></span> <span data-ttu-id="479b6-1446">To odpovídá sekvenci přiřazení, která je provedena ihned po vstupu do konstruktoru a před implicitním voláním konstruktoru přímé základní třídy.</span><span class="sxs-lookup"><span data-stu-id="479b6-1446">This corresponds to a sequence of assignments that are executed immediately upon entry to the constructor and before the implicit invocation of the direct base class constructor.</span></span> <span data-ttu-id="479b6-1447">Inicializátory proměnných jsou spouštěny v textovém pořadí, ve kterém jsou uvedeny v deklaraci třídy.</span><span class="sxs-lookup"><span data-stu-id="479b6-1447">The variable initializers are executed in the textual order in which they appear in the class declaration.</span></span>

### <a name="constructor-execution"></a><span data-ttu-id="479b6-1448">Spuštění konstruktoru</span><span class="sxs-lookup"><span data-stu-id="479b6-1448">Constructor execution</span></span>

<span data-ttu-id="479b6-1449">Inicializátory proměnných jsou transformovány na příkazy přiřazení a tyto příkazy přiřazení jsou spuštěny před voláním konstruktoru instance základní třídy.</span><span class="sxs-lookup"><span data-stu-id="479b6-1449">Variable initializers are transformed into assignment statements, and these assignment statements are executed before the invocation of the base class instance constructor.</span></span> <span data-ttu-id="479b6-1450">Toto řazení zajišťuje, aby všechna pole instance byla inicializována svými Inicializátory proměnných před všemi příkazy, které mají přístup k této instanci.</span><span class="sxs-lookup"><span data-stu-id="479b6-1450">This ordering ensures that all instance fields are initialized by their variable initializers before any statements that have access to that instance are executed.</span></span>

<span data-ttu-id="479b6-1451">Uvedený příklad</span><span class="sxs-lookup"><span data-stu-id="479b6-1451">Given the example</span></span>
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
<span data-ttu-id="479b6-1452">Když `new B()` se používá k vytvoření `B`instance, je vytvořen následující výstup:</span><span class="sxs-lookup"><span data-stu-id="479b6-1452">when `new B()` is used to create an instance of `B`, the following output is produced:</span></span>
```
x = 1, y = 0
```

<span data-ttu-id="479b6-1453">Hodnota `x` je 1, protože inicializátor proměnné je proveden před vyvoláním konstruktoru instance základní třídy.</span><span class="sxs-lookup"><span data-stu-id="479b6-1453">The value of `x` is 1 because the variable initializer is executed before the base class instance constructor is invoked.</span></span> <span data-ttu-id="479b6-1454">Hodnota `y` je však 0 (výchozí hodnota `int`), protože přiřazení k `y` není provedeno až po návratu konstruktoru základní třídy.</span><span class="sxs-lookup"><span data-stu-id="479b6-1454">However, the value of `y` is 0 (the default value of an `int`) because the assignment to `y` is not executed until after the base class constructor returns.</span></span>

<span data-ttu-id="479b6-1455">Je vhodné si představit Inicializátory proměnných instancí a Inicializátory konstruktoru jako příkazy, které jsou automaticky vloženy před *constructor_body*.</span><span class="sxs-lookup"><span data-stu-id="479b6-1455">It is useful to think of instance variable initializers and constructor initializers as statements that are automatically inserted before the *constructor_body*.</span></span> <span data-ttu-id="479b6-1456">Příklad</span><span class="sxs-lookup"><span data-stu-id="479b6-1456">The example</span></span>
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
<span data-ttu-id="479b6-1457">obsahuje několik inicializátorů proměnných; obsahuje také Inicializátory konstruktoru obou tvarů (`base` a `this`).</span><span class="sxs-lookup"><span data-stu-id="479b6-1457">contains several variable initializers; it also contains constructor initializers of both forms (`base` and `this`).</span></span> <span data-ttu-id="479b6-1458">Příklad odpovídá kódu uvedenému níže, kde každý komentář indikuje automaticky vložený příkaz (syntaxe použitá pro automaticky vložená volání konstruktoru není platná, ale slouží pouze k ilustraci mechanismu).</span><span class="sxs-lookup"><span data-stu-id="479b6-1458">The example corresponds to the code shown below, where each comment indicates an automatically inserted statement (the syntax used for the automatically inserted constructor invocations isn't valid, but merely serves to illustrate the mechanism).</span></span>

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

### <a name="default-constructors"></a><span data-ttu-id="479b6-1459">Výchozí konstruktory</span><span class="sxs-lookup"><span data-stu-id="479b6-1459">Default constructors</span></span>

<span data-ttu-id="479b6-1460">Pokud třída neobsahuje deklarace konstruktoru instance, je automaticky poskytnut výchozí konstruktor instance.</span><span class="sxs-lookup"><span data-stu-id="479b6-1460">If a class contains no instance constructor declarations, a default instance constructor is automatically provided.</span></span> <span data-ttu-id="479b6-1461">Tento výchozí konstruktor jednoduše vyvolá konstruktor bez parametrů přímé základní třídy.</span><span class="sxs-lookup"><span data-stu-id="479b6-1461">That default constructor simply invokes the parameterless constructor of the direct base class.</span></span> <span data-ttu-id="479b6-1462">Je-li třída abstraktní, je deklarovaná přístupnost pro výchozí konstruktor chráněna.</span><span class="sxs-lookup"><span data-stu-id="479b6-1462">If the class is abstract then the declared accessibility for the default constructor is protected.</span></span> <span data-ttu-id="479b6-1463">V opačném případě je deklarovaná přístupnost pro výchozí konstruktor veřejná.</span><span class="sxs-lookup"><span data-stu-id="479b6-1463">Otherwise, the declared accessibility for the default constructor is public.</span></span> <span data-ttu-id="479b6-1464">Proto výchozí konstruktor je vždy ve tvaru</span><span class="sxs-lookup"><span data-stu-id="479b6-1464">Thus, the default constructor is always of the form</span></span>

```csharp
protected C(): base() {}
```
<span data-ttu-id="479b6-1465">or</span><span class="sxs-lookup"><span data-stu-id="479b6-1465">or</span></span>
```csharp
public C(): base() {}
```
<span data-ttu-id="479b6-1466">kde `C` je název třídy.</span><span class="sxs-lookup"><span data-stu-id="479b6-1466">where `C` is the name of the class.</span></span> <span data-ttu-id="479b6-1467">Pokud řešení přetížení nemůže určit jedinečný nejlepší kandidát pro inicializátor konstruktoru základní třídy, dojde k chybě při kompilaci.</span><span class="sxs-lookup"><span data-stu-id="479b6-1467">If overload resolution is unable to determine a unique best candidate for the base class constructor initializer then a compile-time error occurs.</span></span>

<span data-ttu-id="479b6-1468">V příkladu</span><span class="sxs-lookup"><span data-stu-id="479b6-1468">In the example</span></span>
```csharp
class Message
{
    object sender;
    string text;
}
```
<span data-ttu-id="479b6-1469">k dispozici je výchozí konstruktor, protože třída neobsahuje žádné deklarace konstruktoru instance.</span><span class="sxs-lookup"><span data-stu-id="479b6-1469">a default constructor is provided because the class contains no instance constructor declarations.</span></span> <span data-ttu-id="479b6-1470">Proto je příklad přesně ekvivalentní</span><span class="sxs-lookup"><span data-stu-id="479b6-1470">Thus, the example is precisely equivalent to</span></span>
```csharp
class Message
{
    object sender;
    string text;

    public Message(): base() {}
}
```

### <a name="private-constructors"></a><span data-ttu-id="479b6-1471">Soukromé konstruktory</span><span class="sxs-lookup"><span data-stu-id="479b6-1471">Private constructors</span></span>

<span data-ttu-id="479b6-1472">Když třída `T` deklaruje pouze konstruktory privátní instance, není možné použít třídy mimo `T` text programu pro odvození z `T` nebo přímo vytvářet instance `T`.</span><span class="sxs-lookup"><span data-stu-id="479b6-1472">When a class `T` declares only private instance constructors, it is not possible for classes outside the program text of `T` to derive from `T` or to directly create instances of `T`.</span></span> <span data-ttu-id="479b6-1473">Proto pokud třída obsahuje pouze statické členy a není určena pro vytvoření instance, přidání prázdného konstruktoru soukromé instance zabrání vytvoření instance.</span><span class="sxs-lookup"><span data-stu-id="479b6-1473">Thus, if a class contains only static members and isn't intended to be instantiated, adding an empty private instance constructor will prevent instantiation.</span></span> <span data-ttu-id="479b6-1474">Příklad:</span><span class="sxs-lookup"><span data-stu-id="479b6-1474">For example:</span></span>
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

<span data-ttu-id="479b6-1475">`Trig` Třídy seskupují související metody a konstanty, ale nejsou určeny pro vytvoření instance.</span><span class="sxs-lookup"><span data-stu-id="479b6-1475">The `Trig` class groups related methods and constants, but is not intended to be instantiated.</span></span> <span data-ttu-id="479b6-1476">Proto deklaruje jeden prázdný konstruktor privátní instance.</span><span class="sxs-lookup"><span data-stu-id="479b6-1476">Therefore it declares a single empty private instance constructor.</span></span> <span data-ttu-id="479b6-1477">Pro potlačení automatické generace výchozího konstruktoru musí být deklarován alespoň jeden konstruktor instance.</span><span class="sxs-lookup"><span data-stu-id="479b6-1477">At least one instance constructor must be declared to suppress the automatic generation of a default constructor.</span></span>

### <a name="optional-instance-constructor-parameters"></a><span data-ttu-id="479b6-1478">Volitelné parametry konstruktoru instance</span><span class="sxs-lookup"><span data-stu-id="479b6-1478">Optional instance constructor parameters</span></span>

<span data-ttu-id="479b6-1479">`this(...)` Tvar inicializátoru konstruktoru se obvykle používá ve spojení s přetížením pro implementaci volitelných parametrů konstruktoru instance.</span><span class="sxs-lookup"><span data-stu-id="479b6-1479">The `this(...)` form of constructor initializer is commonly used in conjunction with overloading to implement optional instance constructor parameters.</span></span> <span data-ttu-id="479b6-1480">V příkladu</span><span class="sxs-lookup"><span data-stu-id="479b6-1480">In the example</span></span>
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
<span data-ttu-id="479b6-1481">první dva konstruktory instance pouze poskytují výchozí hodnoty pro chybějící argumenty.</span><span class="sxs-lookup"><span data-stu-id="479b6-1481">the first two instance constructors merely provide the default values for the missing arguments.</span></span> <span data-ttu-id="479b6-1482">Oba používají `this(...)` inicializátor konstruktoru k vyvolání třetího konstruktoru instance, který ve skutečnosti provádí inicializaci nové instance.</span><span class="sxs-lookup"><span data-stu-id="479b6-1482">Both use a `this(...)` constructor initializer to invoke the third instance constructor, which actually does the work of initializing the new instance.</span></span> <span data-ttu-id="479b6-1483">Výsledkem je, že volitelné parametry konstruktoru:</span><span class="sxs-lookup"><span data-stu-id="479b6-1483">The effect is that of optional constructor parameters:</span></span>
```csharp
Text t1 = new Text();                    // Same as Text(0, 0, null)
Text t2 = new Text(5, 10);               // Same as Text(5, 10, null)
Text t3 = new Text(5, 20, "Hello");
```

## <a name="static-constructors"></a><span data-ttu-id="479b6-1484">Statické konstruktory</span><span class="sxs-lookup"><span data-stu-id="479b6-1484">Static constructors</span></span>

<span data-ttu-id="479b6-1485">***Statický konstruktor*** je člen, který implementuje akce vyžadované k inicializaci typu uzavřené třídy.</span><span class="sxs-lookup"><span data-stu-id="479b6-1485">A ***static constructor*** is a member that implements the actions required to initialize a closed class type.</span></span> <span data-ttu-id="479b6-1486">Statické konstruktory jsou deklarovány pomocí *static_constructor_declaration*s:</span><span class="sxs-lookup"><span data-stu-id="479b6-1486">Static constructors are declared using *static_constructor_declaration*s:</span></span>

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

<span data-ttu-id="479b6-1487">*Static_constructor_declaration* může obsahovat sadu *atributů* ( `extern` [atributů](attributes.md)) a modifikátor ([externí metody](classes.md#external-methods)).</span><span class="sxs-lookup"><span data-stu-id="479b6-1487">A *static_constructor_declaration* may include a set of *attributes* ([Attributes](attributes.md)) and an `extern` modifier ([External methods](classes.md#external-methods)).</span></span>

<span data-ttu-id="479b6-1488">*Identifikátor* *static_constructor_declaration* musí pojmenovat třídu, ve které je deklarován statický konstruktor.</span><span class="sxs-lookup"><span data-stu-id="479b6-1488">The *identifier* of a *static_constructor_declaration* must name the class in which the static constructor is declared.</span></span> <span data-ttu-id="479b6-1489">Pokud je zadán jiný název, dojde k chybě při kompilaci.</span><span class="sxs-lookup"><span data-stu-id="479b6-1489">If any other name is specified, a compile-time error occurs.</span></span>

<span data-ttu-id="479b6-1490">Pokud deklarace statického konstruktoru obsahuje `extern` modifikátor, statický konstruktor je označován jako ***externí statický konstruktor***.</span><span class="sxs-lookup"><span data-stu-id="479b6-1490">When a static constructor declaration includes an `extern` modifier, the static constructor is said to be an ***external static constructor***.</span></span> <span data-ttu-id="479b6-1491">Vzhledem k tomu, že deklarace externích statických konstruktorů neposkytuje žádnou skutečnou implementaci, její *static_constructor_body* se skládá ze střední hodnoty.</span><span class="sxs-lookup"><span data-stu-id="479b6-1491">Because an external static constructor declaration provides no actual implementation, its *static_constructor_body* consists of a semicolon.</span></span> <span data-ttu-id="479b6-1492">Pro všechny ostatní deklarace statického konstruktoru se *static_constructor_body* skládá z *bloku* , který určuje příkazy, které mají být provedeny, aby bylo možné třídu inicializovat.</span><span class="sxs-lookup"><span data-stu-id="479b6-1492">For all other static constructor declarations, the *static_constructor_body* consists of a *block* which specifies the statements to execute in order to initialize the class.</span></span> <span data-ttu-id="479b6-1493">To odpovídá přesně *method_body* statické metody s `void` návratovým typem ([tělo metody](classes.md#method-body)).</span><span class="sxs-lookup"><span data-stu-id="479b6-1493">This corresponds exactly to the *method_body* of a static method with a `void` return type ([Method body](classes.md#method-body)).</span></span>

<span data-ttu-id="479b6-1494">Statické konstruktory nejsou zděděné a nelze je volat přímo.</span><span class="sxs-lookup"><span data-stu-id="479b6-1494">Static constructors are not inherited, and cannot be called directly.</span></span>

<span data-ttu-id="479b6-1495">Statický konstruktor pro uzavřený typ třídy se v dané doméně aplikace provede maximálně jednou.</span><span class="sxs-lookup"><span data-stu-id="479b6-1495">The static constructor for a closed class type executes at most once in a given application domain.</span></span> <span data-ttu-id="479b6-1496">Provedení statického konstruktoru je aktivováno prvním z následujících událostí, které se mají provést v rámci domény aplikace:</span><span class="sxs-lookup"><span data-stu-id="479b6-1496">The execution of a static constructor is triggered by the first of the following events to occur within an application domain:</span></span>

*  <span data-ttu-id="479b6-1497">Vytvoří se instance typu třídy.</span><span class="sxs-lookup"><span data-stu-id="479b6-1497">An instance of the class type is created.</span></span>
*  <span data-ttu-id="479b6-1498">Je odkazováno na všechny statické členy typu třídy.</span><span class="sxs-lookup"><span data-stu-id="479b6-1498">Any of the static members of the class type are referenced.</span></span>

<span data-ttu-id="479b6-1499">Pokud třída obsahuje `Main` metodu ([spuštění aplikace](basic-concepts.md#application-startup)), ve které začíná spuštění, statický konstruktor pro tuto `Main` třídu se spustí před voláním metody.</span><span class="sxs-lookup"><span data-stu-id="479b6-1499">If a class contains the `Main` method ([Application Startup](basic-concepts.md#application-startup)) in which execution begins, the static constructor for that class executes before the `Main` method is called.</span></span>

<span data-ttu-id="479b6-1500">Pro inicializaci nového typu uzavřené třídy je nejprve vytvořena nová sada statických polí ([statická a pole instance](classes.md#static-and-instance-fields)) pro tento konkrétní uzavřený typ.</span><span class="sxs-lookup"><span data-stu-id="479b6-1500">To initialize a new closed class type, first a new set of static fields ([Static and instance fields](classes.md#static-and-instance-fields)) for that particular closed type is created.</span></span> <span data-ttu-id="479b6-1501">Každé statické pole je inicializováno na výchozí hodnotu ([výchozí hodnoty](variables.md#default-values)).</span><span class="sxs-lookup"><span data-stu-id="479b6-1501">Each of the static fields is initialized to its default value ([Default values](variables.md#default-values)).</span></span> <span data-ttu-id="479b6-1502">Dále jsou pro Tato statická pole spouštěny Inicializátory statických polí ([inicializace statických polí](classes.md#static-field-initialization)).</span><span class="sxs-lookup"><span data-stu-id="479b6-1502">Next, the static field initializers ([Static field initialization](classes.md#static-field-initialization)) are executed for those static fields.</span></span> <span data-ttu-id="479b6-1503">Nakonec se spustí statický konstruktor.</span><span class="sxs-lookup"><span data-stu-id="479b6-1503">Finally, the static constructor is executed.</span></span>

<span data-ttu-id="479b6-1504">Příklad</span><span class="sxs-lookup"><span data-stu-id="479b6-1504">The example</span></span>
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
<span data-ttu-id="479b6-1505">musí mít výstup:</span><span class="sxs-lookup"><span data-stu-id="479b6-1505">must produce the output:</span></span>
```
Init A
A.F
Init B
B.F
```
<span data-ttu-id="479b6-1506">vzhledem `A`k tomu `A.F` ,že`B.F`provádění statického konstruktoru je aktivováno voláním a spuštění statickéhokonstruktorujeaktivovánovolánímmetody.`B`</span><span class="sxs-lookup"><span data-stu-id="479b6-1506">because the execution of `A`'s static constructor is triggered by the call to `A.F`, and the execution of `B`'s static constructor is triggered by the call to `B.F`.</span></span>

<span data-ttu-id="479b6-1507">Je možné vytvořit kruhové závislosti, které umožňují, aby statická pole s Inicializátory proměnných byla pozorována ve svém výchozím stavu.</span><span class="sxs-lookup"><span data-stu-id="479b6-1507">It is possible to construct circular dependencies that allow static fields with variable initializers to be observed in their default value state.</span></span>

<span data-ttu-id="479b6-1508">Příklad</span><span class="sxs-lookup"><span data-stu-id="479b6-1508">The example</span></span>
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
<span data-ttu-id="479b6-1509">Vytvoří výstup</span><span class="sxs-lookup"><span data-stu-id="479b6-1509">produces the output</span></span>
```
X = 1, Y = 2
```

<span data-ttu-id="479b6-1510">Chcete-li `Main` spustit metodu, systém nejprve spustí inicializátor pro `B.Y`, před statickým konstruktorem třídy `B`.</span><span class="sxs-lookup"><span data-stu-id="479b6-1510">To execute the `Main` method, the system first runs the initializer for `B.Y`, prior to class `B`'s static constructor.</span></span> <span data-ttu-id="479b6-1511">`Y`způsobují `A`, že se má spustit statický konstruktor inicializátoru, protože se `A.X` na něj odkazuje hodnota.</span><span class="sxs-lookup"><span data-stu-id="479b6-1511">`Y`'s initializer causes `A`'s static constructor to be run because the value of `A.X` is referenced.</span></span> <span data-ttu-id="479b6-1512">Statický konstruktor třídy `A` dále pokračuje na výpočet `X`hodnoty a při tom načítá výchozí hodnotu `Y`, která je nula.</span><span class="sxs-lookup"><span data-stu-id="479b6-1512">The static constructor of `A` in turn proceeds to compute the value of `X`, and in doing so fetches the default value of `Y`, which is zero.</span></span> <span data-ttu-id="479b6-1513">`A.X`je tedy inicializována na hodnotu 1.</span><span class="sxs-lookup"><span data-stu-id="479b6-1513">`A.X` is thus initialized to 1.</span></span> <span data-ttu-id="479b6-1514">Proces spuštění `A`inicializátorů statických polí a statického konstruktoru se pak dokončí a vrátí se do výpočtu počáteční `Y`hodnoty, výsledek, který se bude 2.</span><span class="sxs-lookup"><span data-stu-id="479b6-1514">The process of running `A`'s static field initializers and static constructor then completes, returning to the calculation of the initial value of `Y`, the result of which becomes 2.</span></span>

<span data-ttu-id="479b6-1515">Vzhledem k tomu, že je statický konstruktor proveden právě jednou pro každý uzavřený typ třídy, je to vhodné místo pro vynutit kontroly za běhu u parametru typu, který nelze zkontrolovat v době kompilace prostřednictvím omezení ([omezení parametrů typu](classes.md#type-parameter-constraints)). .</span><span class="sxs-lookup"><span data-stu-id="479b6-1515">Because the static constructor is executed exactly once for each closed constructed class type, it is a convenient place to enforce run-time checks on the type parameter that cannot be checked at compile-time via constraints ([Type parameter constraints](classes.md#type-parameter-constraints)).</span></span> <span data-ttu-id="479b6-1516">Například následující typ používá statický konstruktor k vymáhání, že argument typu je výčet:</span><span class="sxs-lookup"><span data-stu-id="479b6-1516">For example, the following type uses a static constructor to enforce that the type argument is an enum:</span></span>
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

## <a name="destructors"></a><span data-ttu-id="479b6-1517">Destruktory</span><span class="sxs-lookup"><span data-stu-id="479b6-1517">Destructors</span></span>

<span data-ttu-id="479b6-1518">***Destruktor*** je člen, který implementuje akce vyžadované k destrukcií instance třídy.</span><span class="sxs-lookup"><span data-stu-id="479b6-1518">A ***destructor*** is a member that implements the actions required to destruct an instance of a class.</span></span> <span data-ttu-id="479b6-1519">Destruktor je deklarovaný pomocí *destructor_declaration*:</span><span class="sxs-lookup"><span data-stu-id="479b6-1519">A destructor is declared using a *destructor_declaration*:</span></span>

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

<span data-ttu-id="479b6-1520">*Destructor_declaration* může obsahovat sadu *atributů* ([atributů](attributes.md)).</span><span class="sxs-lookup"><span data-stu-id="479b6-1520">A *destructor_declaration* may include a set of *attributes* ([Attributes](attributes.md)).</span></span>

<span data-ttu-id="479b6-1521">*Identifikátor* *destructor_declaration* musí pojmenovat třídu, ve které je destruktor deklarovaný.</span><span class="sxs-lookup"><span data-stu-id="479b6-1521">The *identifier* of a *destructor_declaration* must name the class in which the destructor is declared.</span></span> <span data-ttu-id="479b6-1522">Pokud je zadán jiný název, dojde k chybě při kompilaci.</span><span class="sxs-lookup"><span data-stu-id="479b6-1522">If any other name is specified, a compile-time error occurs.</span></span>

<span data-ttu-id="479b6-1523">Když deklarace destruktoru obsahuje `extern` modifikátor, destruktor je označován jako ***externí destruktor***.</span><span class="sxs-lookup"><span data-stu-id="479b6-1523">When a destructor declaration includes an `extern` modifier, the destructor is said to be an ***external destructor***.</span></span> <span data-ttu-id="479b6-1524">Vzhledem k tomu, že deklarace externího destruktoru neposkytuje žádnou skutečnou implementaci, její *destructor_body* se skládá ze střední hodnoty.</span><span class="sxs-lookup"><span data-stu-id="479b6-1524">Because an external destructor declaration provides no actual implementation, its *destructor_body* consists of a semicolon.</span></span> <span data-ttu-id="479b6-1525">Pro všechny ostatní destruktory se *destructor_body* skládá z *bloku* , který určuje příkazy, které mají být provedeny, aby destrukci instanci třídy.</span><span class="sxs-lookup"><span data-stu-id="479b6-1525">For all other destructors, the *destructor_body* consists of a *block* which specifies the statements to execute in order to destruct an instance of the class.</span></span> <span data-ttu-id="479b6-1526">*Destructor_body* odpovídá přesně *method_body* `void` metody instance s návratovým typem ([tělo metody](classes.md#method-body)).</span><span class="sxs-lookup"><span data-stu-id="479b6-1526">A *destructor_body* corresponds exactly to the *method_body* of an instance method with a `void` return type ([Method body](classes.md#method-body)).</span></span>

<span data-ttu-id="479b6-1527">Destruktory nejsou děděny.</span><span class="sxs-lookup"><span data-stu-id="479b6-1527">Destructors are not inherited.</span></span> <span data-ttu-id="479b6-1528">Třída proto nemá žádné destruktory jiné než ta, která může být deklarována v této třídě.</span><span class="sxs-lookup"><span data-stu-id="479b6-1528">Thus, a class has no destructors other than the one which may be declared in that class.</span></span>

<span data-ttu-id="479b6-1529">Vzhledem k tomu, že destruktor musí mít žádné parametry, nemůže být přetížený, takže třída může mít maximálně jeden destruktor.</span><span class="sxs-lookup"><span data-stu-id="479b6-1529">Since a destructor is required to have no parameters, it cannot be overloaded, so a class can have, at most, one destructor.</span></span>

<span data-ttu-id="479b6-1530">Destruktory jsou vyvolány automaticky a nelze je vyvolat explicitně.</span><span class="sxs-lookup"><span data-stu-id="479b6-1530">Destructors are invoked automatically, and cannot be invoked explicitly.</span></span> <span data-ttu-id="479b6-1531">Instance se stane oprávněným zničením, pokud již není možné pro použití této instance žádným kódem.</span><span class="sxs-lookup"><span data-stu-id="479b6-1531">An instance becomes eligible for destruction when it is no longer possible for any code to use that instance.</span></span> <span data-ttu-id="479b6-1532">Spuštění destruktoru pro instanci může nastat kdykoli poté, co se instance stává nárokem na zničení.</span><span class="sxs-lookup"><span data-stu-id="479b6-1532">Execution of the destructor for the instance may occur at any time after the instance becomes eligible for destruction.</span></span> <span data-ttu-id="479b6-1533">Když je instance destrukturovaná, jsou volány destruktory v řetězu dědičnosti dané instance, od největší odvozené k nejméně odvozené.</span><span class="sxs-lookup"><span data-stu-id="479b6-1533">When an instance is destructed, the destructors in that instance's inheritance chain are called, in order, from most derived to least derived.</span></span> <span data-ttu-id="479b6-1534">Destruktor může být proveden na jakémkoli vlákně.</span><span class="sxs-lookup"><span data-stu-id="479b6-1534">A destructor may be executed on any thread.</span></span> <span data-ttu-id="479b6-1535">Další diskuzi o pravidlech, která určují, kdy a jak se má destruktor spustit, najdete v tématu [Automatická správa paměti](basic-concepts.md#automatic-memory-management).</span><span class="sxs-lookup"><span data-stu-id="479b6-1535">For further discussion of the rules that govern when and how a destructor is executed, see [Automatic memory management](basic-concepts.md#automatic-memory-management).</span></span>

<span data-ttu-id="479b6-1536">Výstup příkladu</span><span class="sxs-lookup"><span data-stu-id="479b6-1536">The output of the example</span></span>
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
<span data-ttu-id="479b6-1537">is</span><span class="sxs-lookup"><span data-stu-id="479b6-1537">is</span></span>
```
B's destructor
A's destructor
```
<span data-ttu-id="479b6-1538">vzhledem k tomu, že destruktory v řetězu dědičnosti jsou volány v pořadí, od největší odvozené k nejméně odvozenému.</span><span class="sxs-lookup"><span data-stu-id="479b6-1538">since destructors in an inheritance chain are called in order, from most derived to least derived.</span></span>

<span data-ttu-id="479b6-1539">Destruktory jsou implementovány přepsáním virtuální metody `Finalize` na `System.Object`.</span><span class="sxs-lookup"><span data-stu-id="479b6-1539">Destructors are implemented by overriding the virtual method `Finalize` on `System.Object`.</span></span> <span data-ttu-id="479b6-1540">C#programy nemají oprávnění přepsat tuto metodu nebo je volat (nebo potlačením) přímo.</span><span class="sxs-lookup"><span data-stu-id="479b6-1540">C# programs are not permitted to override this method or call it (or overrides of it) directly.</span></span> <span data-ttu-id="479b6-1541">Např. program</span><span class="sxs-lookup"><span data-stu-id="479b6-1541">For instance, the program</span></span>
```csharp
class A 
{
    override protected void Finalize() {}    // error

    public void F() {
        this.Finalize();                     // error
    }
}
```
<span data-ttu-id="479b6-1542">obsahuje dvě chyby.</span><span class="sxs-lookup"><span data-stu-id="479b6-1542">contains two errors.</span></span>

<span data-ttu-id="479b6-1543">Kompilátor se chová, jako by tato metoda a přepsání neexistovaly vůbec.</span><span class="sxs-lookup"><span data-stu-id="479b6-1543">The compiler behaves as if this method, and overrides of it, do not exist at all.</span></span> <span data-ttu-id="479b6-1544">Proto tento program:</span><span class="sxs-lookup"><span data-stu-id="479b6-1544">Thus, this program:</span></span>
```csharp
class A 
{
    void Finalize() {}                            // permitted
}
```
<span data-ttu-id="479b6-1545">je platný a metoda zobrazuje skryté `System.Object` `Finalize` metody.</span><span class="sxs-lookup"><span data-stu-id="479b6-1545">is valid, and the method shown hides `System.Object`'s `Finalize` method.</span></span>

<span data-ttu-id="479b6-1546">Diskuzi o chování při vyvolání výjimky z destruktoru naleznete v tématu [jak jsou zpracovávány výjimky](exceptions.md#how-exceptions-are-handled).</span><span class="sxs-lookup"><span data-stu-id="479b6-1546">For a discussion of the behavior when an exception is thrown from a destructor, see [How exceptions are handled](exceptions.md#how-exceptions-are-handled).</span></span>

## <a name="iterators"></a><span data-ttu-id="479b6-1547">Iterátory</span><span class="sxs-lookup"><span data-stu-id="479b6-1547">Iterators</span></span>

<span data-ttu-id="479b6-1548">Člen funkce ([členy funkce](expressions.md#function-members)) implementovaný pomocí bloku iterátoru ([bloky](statements.md#blocks)) se nazývá ***iterátor***.</span><span class="sxs-lookup"><span data-stu-id="479b6-1548">A function member ([Function members](expressions.md#function-members)) implemented using an iterator block ([Blocks](statements.md#blocks)) is called an ***iterator***.</span></span>

<span data-ttu-id="479b6-1549">Blok iterátoru lze použít jako tělo členu funkce, pokud je návratový typ odpovídajícího členu funkce jedním z rozhraní enumerátoru ([rozhraní enumerátorů](classes.md#enumerator-interfaces)) nebo jednoho ze vyčíslitelné rozhraní ([vyčíslitelné](classes.md#enumerable-interfaces)rozhraní). .</span><span class="sxs-lookup"><span data-stu-id="479b6-1549">An iterator block may be used as the body of a function member as long as the return type of the corresponding function member is one of the enumerator interfaces ([Enumerator interfaces](classes.md#enumerator-interfaces)) or one of the enumerable interfaces ([Enumerable interfaces](classes.md#enumerable-interfaces)).</span></span> <span data-ttu-id="479b6-1550">Může dojít jako *method_body*, *operator_body* nebo *accessor_body*, zatímco události, konstruktory instancí, statické konstruktory a destruktory nelze implementovat jako iterátory.</span><span class="sxs-lookup"><span data-stu-id="479b6-1550">It can occur as a *method_body*, *operator_body* or *accessor_body*, whereas events, instance constructors, static constructors and destructors cannot be implemented as iterators.</span></span>

<span data-ttu-id="479b6-1551">Pokud je člen funkce implementován pomocí bloku iterátoru, jedná se o chybu při kompilaci pro seznam formálních parametrů člena funkce k určení `ref` parametrů nebo. `out`</span><span class="sxs-lookup"><span data-stu-id="479b6-1551">When a function member is implemented using an iterator block, it is a compile-time error for the formal parameter list of the function member to specify any `ref` or `out` parameters.</span></span>

### <a name="enumerator-interfaces"></a><span data-ttu-id="479b6-1552">Rozhraní enumerátorů</span><span class="sxs-lookup"><span data-stu-id="479b6-1552">Enumerator interfaces</span></span>

<span data-ttu-id="479b6-1553">***Rozhraní enumerátoru*** jsou neobecné rozhraní `System.Collections.IEnumerator` a všechny instance obecného rozhraní. `System.Collections.Generic.IEnumerator<T>`</span><span class="sxs-lookup"><span data-stu-id="479b6-1553">The ***enumerator interfaces*** are the non-generic interface `System.Collections.IEnumerator` and all instantiations of the generic interface `System.Collections.Generic.IEnumerator<T>`.</span></span> <span data-ttu-id="479b6-1554">V zkrácení v této kapitole jsou tato rozhraní odkazována jako `IEnumerator` a `IEnumerator<T>`v uvedeném pořadí.</span><span class="sxs-lookup"><span data-stu-id="479b6-1554">For the sake of brevity, in this chapter these interfaces are referenced as `IEnumerator` and `IEnumerator<T>`, respectively.</span></span>

### <a name="enumerable-interfaces"></a><span data-ttu-id="479b6-1555">Výčtová rozhraní</span><span class="sxs-lookup"><span data-stu-id="479b6-1555">Enumerable interfaces</span></span>

<span data-ttu-id="479b6-1556">***Výčtová rozhraní*** jsou neobecné rozhraní `System.Collections.IEnumerable` a všechny instance obecného rozhraní `System.Collections.Generic.IEnumerable<T>`.</span><span class="sxs-lookup"><span data-stu-id="479b6-1556">The ***enumerable interfaces*** are the non-generic interface `System.Collections.IEnumerable` and all instantiations of the generic interface `System.Collections.Generic.IEnumerable<T>`.</span></span> <span data-ttu-id="479b6-1557">V zkrácení v této kapitole jsou tato rozhraní odkazována jako `IEnumerable` a `IEnumerable<T>`v uvedeném pořadí.</span><span class="sxs-lookup"><span data-stu-id="479b6-1557">For the sake of brevity, in this chapter these interfaces are referenced as `IEnumerable` and `IEnumerable<T>`, respectively.</span></span>

### <a name="yield-type"></a><span data-ttu-id="479b6-1558">Typ yield</span><span class="sxs-lookup"><span data-stu-id="479b6-1558">Yield type</span></span>

<span data-ttu-id="479b6-1559">Iterátor vytvoří sekvenci hodnot, všechny stejného typu.</span><span class="sxs-lookup"><span data-stu-id="479b6-1559">An iterator produces a sequence of values, all of the same type.</span></span> <span data-ttu-id="479b6-1560">Tento typ se nazývá ***typ yield*** iterátoru.</span><span class="sxs-lookup"><span data-stu-id="479b6-1560">This type is called the ***yield type*** of the iterator.</span></span>

*  <span data-ttu-id="479b6-1561">Typ yield iterátoru, který vrací `IEnumerator` nebo `IEnumerable` je `object`.</span><span class="sxs-lookup"><span data-stu-id="479b6-1561">The yield type of an iterator that returns `IEnumerator` or `IEnumerable` is `object`.</span></span>
*  <span data-ttu-id="479b6-1562">Typ yield iterátoru, který vrací `IEnumerator<T>` nebo `IEnumerable<T>` je `T`.</span><span class="sxs-lookup"><span data-stu-id="479b6-1562">The yield type of an iterator that returns `IEnumerator<T>` or `IEnumerable<T>` is `T`.</span></span>

### <a name="enumerator-objects"></a><span data-ttu-id="479b6-1563">Čítače objektů</span><span class="sxs-lookup"><span data-stu-id="479b6-1563">Enumerator objects</span></span>

<span data-ttu-id="479b6-1564">Když člen funkce vracející typ rozhraní enumerátoru je implementován pomocí bloku iterátoru, volání členu funkce nespustí okamžitě kód v bloku iterátoru.</span><span class="sxs-lookup"><span data-stu-id="479b6-1564">When a function member returning an enumerator interface type is implemented using an iterator block, invoking the function member does not immediately execute the code in the iterator block.</span></span> <span data-ttu-id="479b6-1565">Místo toho se vytvoří a vrátí ***objekt enumerátoru*** .</span><span class="sxs-lookup"><span data-stu-id="479b6-1565">Instead, an ***enumerator object*** is created and returned.</span></span> <span data-ttu-id="479b6-1566">Tento objekt zapouzdřuje kód zadaný v bloku iterátoru a spuštění kódu v bloku iterátoru nastane, pokud je vyvolána `MoveNext` metoda objektu Enumerator.</span><span class="sxs-lookup"><span data-stu-id="479b6-1566">This object encapsulates the code specified in the iterator block, and execution of the code in the iterator block occurs when the enumerator object's `MoveNext` method is invoked.</span></span> <span data-ttu-id="479b6-1567">Objekt enumerátoru má následující vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="479b6-1567">An enumerator object has the following characteristics:</span></span>

*  <span data-ttu-id="479b6-1568">Implementuje `IEnumerator` a `IEnumerator<T>`, kde`T` je typ yield iterátoru.</span><span class="sxs-lookup"><span data-stu-id="479b6-1568">It implements `IEnumerator` and `IEnumerator<T>`, where `T` is the yield type of the iterator.</span></span>
*  <span data-ttu-id="479b6-1569">Implementuje `System.IDisposable`.</span><span class="sxs-lookup"><span data-stu-id="479b6-1569">It implements `System.IDisposable`.</span></span>
*  <span data-ttu-id="479b6-1570">Inicializuje se pomocí kopie hodnot argumentů (pokud existuje) a hodnoty instance předané členu funkce.</span><span class="sxs-lookup"><span data-stu-id="479b6-1570">It is initialized with a copy of the argument values (if any) and instance value passed to the function member.</span></span>
*  <span data-ttu-id="479b6-1571">Má čtyři možné stavy, ***před***, po ***spuštění***, ***pozastavení***a ***po***a je zpočátku ve stavu ***před*** .</span><span class="sxs-lookup"><span data-stu-id="479b6-1571">It has four potential states, ***before***, ***running***, ***suspended***, and ***after***, and is initially in the ***before*** state.</span></span>

<span data-ttu-id="479b6-1572">Objekt enumerátor je obvykle instancí třídy výčtu generované kompilátorem, která zapouzdřuje kód v bloku iterátoru a implementuje rozhraní enumerátorů, ale je možné použít i jiné metody implementace.</span><span class="sxs-lookup"><span data-stu-id="479b6-1572">An enumerator object is typically an instance of a compiler-generated enumerator class that encapsulates the code in the iterator block and implements the enumerator interfaces, but other methods of implementation are possible.</span></span> <span data-ttu-id="479b6-1573">Pokud je třída enumerátoru generována kompilátorem, tato třída bude vnořena, přímo nebo nepřímo, ve třídě obsahující člena funkce, bude mít privátní přístupnost a bude mít název vyhrazený pro použití kompilátorem ([identifikátory](lexical-structure.md#identifiers)).</span><span class="sxs-lookup"><span data-stu-id="479b6-1573">If an enumerator class is generated by the compiler, that class will be nested, directly or indirectly, in the class containing the function member, it will have private accessibility, and it will have a name reserved for compiler use ([Identifiers](lexical-structure.md#identifiers)).</span></span>

<span data-ttu-id="479b6-1574">Objekt enumerátoru může implementovat více rozhraní, než je uvedeno výše.</span><span class="sxs-lookup"><span data-stu-id="479b6-1574">An enumerator object may implement more interfaces than those specified above.</span></span>

<span data-ttu-id="479b6-1575">V následujících částech jsou popsány přesné chování `MoveNext`, `Current`a v `Dispose` implementacích rozhraní `IEnumerable` a `IEnumerable<T>` , které jsou k dispozici v objektu Enumerator.</span><span class="sxs-lookup"><span data-stu-id="479b6-1575">The following sections describe the exact behavior of the `MoveNext`, `Current`, and `Dispose` members of the `IEnumerable` and `IEnumerable<T>` interface implementations provided by an enumerator object.</span></span>

<span data-ttu-id="479b6-1576">Všimněte si, že objekty enumerátoru tuto `IEnumerator.Reset` metodu nepodporují.</span><span class="sxs-lookup"><span data-stu-id="479b6-1576">Note that enumerator objects do not support the `IEnumerator.Reset` method.</span></span> <span data-ttu-id="479b6-1577">Vyvolání této metody způsobí, že `System.NotSupportedException` bude vyvolána výjimka.</span><span class="sxs-lookup"><span data-stu-id="479b6-1577">Invoking this method causes a `System.NotSupportedException` to be thrown.</span></span>

#### <a name="the-movenext-method"></a><span data-ttu-id="479b6-1578">Metoda MoveNext</span><span class="sxs-lookup"><span data-stu-id="479b6-1578">The MoveNext method</span></span>

<span data-ttu-id="479b6-1579">`MoveNext` Metoda objektu Enumerator zapouzdřuje kód bloku iterátoru.</span><span class="sxs-lookup"><span data-stu-id="479b6-1579">The `MoveNext` method of an enumerator object encapsulates the code of an iterator block.</span></span> <span data-ttu-id="479b6-1580">Vyvolání `MoveNext` metody spustí kód v bloku iterátoru a podle potřeby `Current` nastaví vlastnost objektu Enumerator.</span><span class="sxs-lookup"><span data-stu-id="479b6-1580">Invoking the `MoveNext` method executes code in the iterator block and sets the `Current` property of the enumerator object as appropriate.</span></span> <span data-ttu-id="479b6-1581">Přesná akce prováděná `MoveNext` v závislosti na stavu objektu enumerátoru, když `MoveNext` je vyvolána:</span><span class="sxs-lookup"><span data-stu-id="479b6-1581">The precise action performed by `MoveNext` depends on the state of the enumerator object when `MoveNext` is invoked:</span></span>

*  <span data-ttu-id="479b6-1582">Pokud je stav objektu enumerátoru ***před***, vyvolání `MoveNext`:</span><span class="sxs-lookup"><span data-stu-id="479b6-1582">If the state of the enumerator object is ***before***, invoking `MoveNext`:</span></span>
   * <span data-ttu-id="479b6-1583">Změní stav na ***spuštěno***.</span><span class="sxs-lookup"><span data-stu-id="479b6-1583">Changes the state to ***running***.</span></span>
   * <span data-ttu-id="479b6-1584">Inicializuje parametry (včetně `this`) bloku iterátoru na hodnoty argumentů a hodnotu instance uloženou při inicializaci objektu Enumerator.</span><span class="sxs-lookup"><span data-stu-id="479b6-1584">Initializes the parameters (including `this`) of the iterator block to the argument values and instance value saved when the enumerator object was initialized.</span></span>
   * <span data-ttu-id="479b6-1585">Spustí blok iterátoru od začátku až do přerušení provádění (jak je popsáno níže).</span><span class="sxs-lookup"><span data-stu-id="479b6-1585">Executes the iterator block from the beginning until execution is interrupted (as described below).</span></span>
*  <span data-ttu-id="479b6-1586">Pokud je ***spuštěn***stav objektu Enumerator, není výsledek vyvolání `MoveNext` uveden.</span><span class="sxs-lookup"><span data-stu-id="479b6-1586">If the state of the enumerator object is ***running***, the result of invoking `MoveNext` is unspecified.</span></span>
*  <span data-ttu-id="479b6-1587">Pokud je stav objektu enumerátoru ***pozastaveno***, volání `MoveNext`:</span><span class="sxs-lookup"><span data-stu-id="479b6-1587">If the state of the enumerator object is ***suspended***, invoking `MoveNext`:</span></span>
   * <span data-ttu-id="479b6-1588">Změní stav na ***spuštěno***.</span><span class="sxs-lookup"><span data-stu-id="479b6-1588">Changes the state to ***running***.</span></span>
   * <span data-ttu-id="479b6-1589">Obnoví hodnoty všech místních proměnných a parametrů (včetně tohoto) do hodnot uložených při posledním pozastavení běhu bloku iterátoru.</span><span class="sxs-lookup"><span data-stu-id="479b6-1589">Restores the values of all local variables and parameters (including this) to the values saved when execution of the iterator block was last suspended.</span></span> <span data-ttu-id="479b6-1590">Všimněte si, že obsah všech objektů, na které tyto proměnné odkazovaly, se mohou od předchozího volání metody MoveNext změnit.</span><span class="sxs-lookup"><span data-stu-id="479b6-1590">Note that the contents of any objects referenced by these variables may have changed since the previous call to MoveNext.</span></span>
   * <span data-ttu-id="479b6-1591">Pokračuje v provádění bloku iterátoru hned po `yield return` příkazu, který způsobil pozastavení provádění, a pokračuje, dokud nebude provádění přerušeno (jak je popsáno níže).</span><span class="sxs-lookup"><span data-stu-id="479b6-1591">Resumes execution of the iterator block immediately following the `yield return` statement that caused the suspension of execution and continues until execution is interrupted (as described below).</span></span>
*  <span data-ttu-id="479b6-1592">Pokud je stav objektu enumerátor ***po***, volání `MoveNext` funkce vrátí `false`.</span><span class="sxs-lookup"><span data-stu-id="479b6-1592">If the state of the enumerator object is ***after***, invoking `MoveNext` returns `false`.</span></span>


<span data-ttu-id="479b6-1593">Když `MoveNext` se spustí blok iterátoru, provádění se dá přerušit čtyřmi způsoby: `yield return` Pomocí příkazu `yield break` , pomocí příkazu, narazí na konec bloku iterátoru a vyvolala se výjimka a rozšíří se z bloku iterátoru.</span><span class="sxs-lookup"><span data-stu-id="479b6-1593">When `MoveNext` executes the iterator block, execution can be interrupted in four ways: By a `yield return` statement, by a `yield break` statement, by encountering the end of the iterator block, and by an exception being thrown and propagated out of the iterator block.</span></span>

*  <span data-ttu-id="479b6-1594">Při zjištění příkazu ([příkaz yield):](statements.md#the-yield-statement) `yield return`</span><span class="sxs-lookup"><span data-stu-id="479b6-1594">When a `yield return` statement is encountered ([The yield statement](statements.md#the-yield-statement)):</span></span>
   * <span data-ttu-id="479b6-1595">Výraz uvedený v příkazu je vyhodnocen, implicitně převeden na typ yield a přiřazený `Current` vlastnosti objektu enumerátor.</span><span class="sxs-lookup"><span data-stu-id="479b6-1595">The expression given in the statement is evaluated, implicitly converted to the yield type, and assigned to the `Current` property of the enumerator object.</span></span>
   * <span data-ttu-id="479b6-1596">Provádění textu iterátoru je pozastaveno.</span><span class="sxs-lookup"><span data-stu-id="479b6-1596">Execution of the iterator body is suspended.</span></span> <span data-ttu-id="479b6-1597">Hodnoty všech místních proměnných a parametrů (včetně `this`) jsou uloženy, stejně jako umístění tohoto `yield return` příkazu.</span><span class="sxs-lookup"><span data-stu-id="479b6-1597">The values of all local variables and parameters (including `this`) are saved, as is the location of this `yield return` statement.</span></span> <span data-ttu-id="479b6-1598">Pokud je `try` `finally` příkaz v jednom nebo více blocích, přidružené bloky nejsou v tuto chvíli spuštěny. `yield return`</span><span class="sxs-lookup"><span data-stu-id="479b6-1598">If the `yield return` statement is within one or more `try` blocks, the associated `finally` blocks are not executed at this time.</span></span>
   * <span data-ttu-id="479b6-1599">Stav objektu enumerátoru je změněn na ***pozastaveno***.</span><span class="sxs-lookup"><span data-stu-id="479b6-1599">The state of the enumerator object is changed to ***suspended***.</span></span>
   * <span data-ttu-id="479b6-1600">Metoda se vrátí `true` volajícímu, což značí, že iteraci se úspěšně pokročila na další hodnotu. `MoveNext`</span><span class="sxs-lookup"><span data-stu-id="479b6-1600">The `MoveNext` method returns `true` to its caller, indicating that the iteration successfully advanced to the next value.</span></span>
*  <span data-ttu-id="479b6-1601">Při zjištění příkazu ([příkaz yield):](statements.md#the-yield-statement) `yield break`</span><span class="sxs-lookup"><span data-stu-id="479b6-1601">When a `yield break` statement is encountered ([The yield statement](statements.md#the-yield-statement)):</span></span>
   * <span data-ttu-id="479b6-1602">Pokud je `try` `finally` příkaz v jednom nebo více blocích, jsou spuštěny přidružené bloky. `yield break`</span><span class="sxs-lookup"><span data-stu-id="479b6-1602">If the `yield break` statement is within one or more `try` blocks, the associated `finally` blocks are executed.</span></span>
   * <span data-ttu-id="479b6-1603">Stav objektu enumerátoru se změní na ***po***.</span><span class="sxs-lookup"><span data-stu-id="479b6-1603">The state of the enumerator object is changed to ***after***.</span></span>
   * <span data-ttu-id="479b6-1604">Metoda se vrátí `false` volajícímu, což značí, že je iterace dokončená. `MoveNext`</span><span class="sxs-lookup"><span data-stu-id="479b6-1604">The `MoveNext` method returns `false` to its caller, indicating that the iteration is complete.</span></span>
*  <span data-ttu-id="479b6-1605">Při zjištění konce textu iterátoru:</span><span class="sxs-lookup"><span data-stu-id="479b6-1605">When the end of the iterator body is encountered:</span></span>
   * <span data-ttu-id="479b6-1606">Stav objektu enumerátoru se změní na ***po***.</span><span class="sxs-lookup"><span data-stu-id="479b6-1606">The state of the enumerator object is changed to ***after***.</span></span>
   * <span data-ttu-id="479b6-1607">Metoda se vrátí `false` volajícímu, což značí, že je iterace dokončená. `MoveNext`</span><span class="sxs-lookup"><span data-stu-id="479b6-1607">The `MoveNext` method returns `false` to its caller, indicating that the iteration is complete.</span></span>
*  <span data-ttu-id="479b6-1608">Když je vyvolána výjimka a šířena mimo blok iterátoru:</span><span class="sxs-lookup"><span data-stu-id="479b6-1608">When an exception is thrown and propagated out of the iterator block:</span></span>
   * <span data-ttu-id="479b6-1609">Příslušné `finally` bloky v těle iterátoru budou provedeny při šíření výjimky.</span><span class="sxs-lookup"><span data-stu-id="479b6-1609">Appropriate `finally` blocks in the iterator body will have been executed by the exception propagation.</span></span>
   * <span data-ttu-id="479b6-1610">Stav objektu enumerátoru se změní na ***po***.</span><span class="sxs-lookup"><span data-stu-id="479b6-1610">The state of the enumerator object is changed to ***after***.</span></span>
   * <span data-ttu-id="479b6-1611">Šíření výjimky pokračuje volajícím `MoveNext` metody.</span><span class="sxs-lookup"><span data-stu-id="479b6-1611">The exception propagation continues to the caller of the `MoveNext` method.</span></span>

#### <a name="the-current-property"></a><span data-ttu-id="479b6-1612">Aktuální vlastnost</span><span class="sxs-lookup"><span data-stu-id="479b6-1612">The Current property</span></span>

<span data-ttu-id="479b6-1613">`Current` Vlastnost objektu enumerátoru je `yield return` ovlivněna příkazy v bloku iterátoru.</span><span class="sxs-lookup"><span data-stu-id="479b6-1613">An enumerator object's `Current` property is affected by `yield return` statements in the iterator block.</span></span>

<span data-ttu-id="479b6-1614">Když je objekt enumerátoru v ***pozastaveném*** stavu, hodnota `Current` je hodnota nastavená předchozím voláním metody `MoveNext`.</span><span class="sxs-lookup"><span data-stu-id="479b6-1614">When an enumerator object is in the ***suspended*** state, the value of `Current` is the value set by the previous call to `MoveNext`.</span></span> <span data-ttu-id="479b6-1615">Pokud je objekt enumerátoru v ***před***, ***spuštěný***nebo ***po*** stavech, není výsledek přístupu `Current` uveden.</span><span class="sxs-lookup"><span data-stu-id="479b6-1615">When an enumerator object is in the ***before***, ***running***, or ***after*** states, the result of accessing `Current` is unspecified.</span></span>

<span data-ttu-id="479b6-1616">Pro iterátory s typem yield, který je `object`jiný než, výsledek přístupu `Current` `IEnumerable` přes implementaci objektu enumerátoru odpovídá přístupu `Current` prostřednictvím objektu `IEnumerator<T>` enumerátoru. implementace a přetypování výsledku do `object`.</span><span class="sxs-lookup"><span data-stu-id="479b6-1616">For an iterator with a yield type other than `object`, the result of accessing `Current` through the enumerator object's `IEnumerable` implementation corresponds to accessing `Current` through the enumerator object's `IEnumerator<T>` implementation and casting the result to `object`.</span></span>

#### <a name="the-dispose-method"></a><span data-ttu-id="479b6-1617">Metoda Dispose</span><span class="sxs-lookup"><span data-stu-id="479b6-1617">The Dispose method</span></span>

<span data-ttu-id="479b6-1618">Metoda slouží k vyčištění iterace tím, že se objekt enumerátoru ***po stavu po.*** `Dispose`</span><span class="sxs-lookup"><span data-stu-id="479b6-1618">The `Dispose` method is used to clean up the iteration by bringing the enumerator object to the ***after*** state.</span></span>

*  <span data-ttu-id="479b6-1619">Pokud je stav objektu enumerátoru ***před***, volání `Dispose` změní stav na ***po***.</span><span class="sxs-lookup"><span data-stu-id="479b6-1619">If the state of the enumerator object is ***before***, invoking `Dispose` changes the state to ***after***.</span></span>
*  <span data-ttu-id="479b6-1620">Pokud je ***spuštěn***stav objektu Enumerator, není výsledek vyvolání `Dispose` uveden.</span><span class="sxs-lookup"><span data-stu-id="479b6-1620">If the state of the enumerator object is ***running***, the result of invoking `Dispose` is unspecified.</span></span>
*  <span data-ttu-id="479b6-1621">Pokud je stav objektu enumerátoru ***pozastaveno***, volání `Dispose`:</span><span class="sxs-lookup"><span data-stu-id="479b6-1621">If the state of the enumerator object is ***suspended***, invoking `Dispose`:</span></span>
   * <span data-ttu-id="479b6-1622">Změní stav na ***spuštěno***.</span><span class="sxs-lookup"><span data-stu-id="479b6-1622">Changes the state to ***running***.</span></span>
   * <span data-ttu-id="479b6-1623">Provede všechny bloky finally, jako by byl příkaz `yield return` naposledy spuštěn. `yield break`</span><span class="sxs-lookup"><span data-stu-id="479b6-1623">Executes any finally blocks as if the last executed `yield return` statement were a `yield break` statement.</span></span> <span data-ttu-id="479b6-1624">V případě, že dojde k výjimce a šíření výjimky z těla iterátoru, je stav objektu enumerátoru nastaven na hodnotu ***po*** a výjimka je šířena volajícímu `Dispose` metody.</span><span class="sxs-lookup"><span data-stu-id="479b6-1624">If this causes an exception to be thrown and propagated out of the iterator body, the state of the enumerator object is set to ***after*** and the exception is propagated to the caller of the `Dispose` method.</span></span>
   * <span data-ttu-id="479b6-1625">Změní stav na ***po***.</span><span class="sxs-lookup"><span data-stu-id="479b6-1625">Changes the state to ***after***.</span></span>
*  <span data-ttu-id="479b6-1626">Pokud je stav objektu enumerátor ***po***, vyvolání `Dispose` nemá žádný vliv.</span><span class="sxs-lookup"><span data-stu-id="479b6-1626">If the state of the enumerator object is ***after***, invoking `Dispose` has no affect.</span></span>

### <a name="enumerable-objects"></a><span data-ttu-id="479b6-1627">Vyčíslitelné objekty</span><span class="sxs-lookup"><span data-stu-id="479b6-1627">Enumerable objects</span></span>

<span data-ttu-id="479b6-1628">Když člen funkce vracející Výčtový typ rozhraní je implementován pomocí bloku iterátoru, volání členu funkce nespustí okamžitě kód v bloku iterátoru.</span><span class="sxs-lookup"><span data-stu-id="479b6-1628">When a function member returning an enumerable interface type is implemented using an iterator block, invoking the function member does not immediately execute the code in the iterator block.</span></span> <span data-ttu-id="479b6-1629">Místo toho je vytvořen a vrácen ***vyčíslitelné objekt*** .</span><span class="sxs-lookup"><span data-stu-id="479b6-1629">Instead, an ***enumerable object*** is created and returned.</span></span> <span data-ttu-id="479b6-1630">`GetEnumerator` Metoda vyčíslitelného objektu vrátí objekt enumerátoru, který zapouzdřuje kód zadaný v bloku iterátoru, a spuštění kódu v bloku iterátoru nastane, když je vyvolána `MoveNext` metoda objektu Enumerator.</span><span class="sxs-lookup"><span data-stu-id="479b6-1630">The enumerable object's `GetEnumerator` method returns an enumerator object that encapsulates the code specified in the iterator block, and execution of the code in the iterator block occurs when the enumerator object's `MoveNext` method is invoked.</span></span> <span data-ttu-id="479b6-1631">Vyčíslitelné objekty mají následující vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="479b6-1631">An enumerable object has the following characteristics:</span></span>

*  <span data-ttu-id="479b6-1632">Implementuje `IEnumerable` a `IEnumerable<T>`, kde`T` je typ yield iterátoru.</span><span class="sxs-lookup"><span data-stu-id="479b6-1632">It implements `IEnumerable` and `IEnumerable<T>`, where `T` is the yield type of the iterator.</span></span>
*  <span data-ttu-id="479b6-1633">Inicializuje se pomocí kopie hodnot argumentů (pokud existuje) a hodnoty instance předané členu funkce.</span><span class="sxs-lookup"><span data-stu-id="479b6-1633">It is initialized with a copy of the argument values (if any) and instance value passed to the function member.</span></span>

<span data-ttu-id="479b6-1634">Vyčíslitelné objekt je obvykle instancí výčtové třídy generované kompilátorem, která zapouzdřuje kód v bloku iterátoru a implementuje Výčtová rozhraní, ale mohou být k dispozici jiné metody implementace.</span><span class="sxs-lookup"><span data-stu-id="479b6-1634">An enumerable object is typically an instance of a compiler-generated enumerable class that encapsulates the code in the iterator block and implements the enumerable interfaces, but other methods of implementation are possible.</span></span> <span data-ttu-id="479b6-1635">Pokud je Výčtová třída generována kompilátorem, tato třída bude vnořena, přímo nebo nepřímo, ve třídě obsahující člena funkce, bude mít privátní přístupnost a bude mít název vyhrazený pro použití kompilátorem ([identifikátory](lexical-structure.md#identifiers)).</span><span class="sxs-lookup"><span data-stu-id="479b6-1635">If an enumerable class is generated by the compiler, that class will be nested, directly or indirectly, in the class containing the function member, it will have private accessibility, and it will have a name reserved for compiler use ([Identifiers](lexical-structure.md#identifiers)).</span></span>

<span data-ttu-id="479b6-1636">Vyčíslitelné objekty mohou implementovat více rozhraní, než je uvedeno výše.</span><span class="sxs-lookup"><span data-stu-id="479b6-1636">An enumerable object may implement more interfaces than those specified above.</span></span> <span data-ttu-id="479b6-1637">Konkrétně může implementovat `IEnumerator` také vyčíslitelné objekt a `IEnumerator<T>`jeho povolení sloužit jako vyčíslitelné i enumerátor.</span><span class="sxs-lookup"><span data-stu-id="479b6-1637">In particular, an enumerable object may also implement `IEnumerator` and `IEnumerator<T>`, enabling it to serve as both an enumerable and an enumerator.</span></span> <span data-ttu-id="479b6-1638">V tomto typu implementace při prvním vyvolání `GetEnumerator` metody vyčíslitelného objektu je vrácen vyčíslitelné samotný objekt.</span><span class="sxs-lookup"><span data-stu-id="479b6-1638">In that type of implementation, the first time an enumerable object's `GetEnumerator` method is invoked, the enumerable object itself is returned.</span></span> <span data-ttu-id="479b6-1639">Následná vyvolání vyčíslitelného objektu `GetEnumerator`, pokud nějaký objekt, vrátí kopii vyčíslitelného objektu.</span><span class="sxs-lookup"><span data-stu-id="479b6-1639">Subsequent invocations of the enumerable object's `GetEnumerator`, if any, return a copy of the enumerable object.</span></span> <span data-ttu-id="479b6-1640">Proto každý vrácený enumerátor má svůj vlastní stav a změny v jednom enumerátoru nebudou mít vliv na další.</span><span class="sxs-lookup"><span data-stu-id="479b6-1640">Thus, each returned enumerator has its own state and changes in one enumerator will not affect another.</span></span>

#### <a name="the-getenumerator-method"></a><span data-ttu-id="479b6-1641">Metoda GetEnumerator</span><span class="sxs-lookup"><span data-stu-id="479b6-1641">The GetEnumerator method</span></span>

<span data-ttu-id="479b6-1642">Vyčíslitelné objekty poskytují implementaci `GetEnumerator` metod `IEnumerable` rozhraní a `IEnumerable<T>` .</span><span class="sxs-lookup"><span data-stu-id="479b6-1642">An enumerable object provides an implementation of the `GetEnumerator` methods of the `IEnumerable` and `IEnumerable<T>` interfaces.</span></span> <span data-ttu-id="479b6-1643">Tyto dvě `GetEnumerator` metody sdílejí společnou implementaci, která získává a vrací dostupný objekt enumerátoru.</span><span class="sxs-lookup"><span data-stu-id="479b6-1643">The two `GetEnumerator` methods share a common implementation that acquires and returns an available enumerator object.</span></span> <span data-ttu-id="479b6-1644">Objekt enumerátor je inicializován s hodnotami argumentů a hodnotou instance uloženou při inicializaci vyčíslitelného objektu, ale v opačném případě funkce objektu Enumerator funguje tak, jak je popsáno v tématu [čítače objektů](classes.md#enumerator-objects).</span><span class="sxs-lookup"><span data-stu-id="479b6-1644">The enumerator object is initialized with the argument values and instance value saved when the enumerable object was initialized, but otherwise the enumerator object functions as described in [Enumerator objects](classes.md#enumerator-objects).</span></span>

### <a name="implementation-example"></a><span data-ttu-id="479b6-1645">Příklad implementace</span><span class="sxs-lookup"><span data-stu-id="479b6-1645">Implementation example</span></span>

<span data-ttu-id="479b6-1646">Tato část popisuje možnou implementaci iterátorů z pohledu standardních C# konstrukcí.</span><span class="sxs-lookup"><span data-stu-id="479b6-1646">This section describes a possible implementation of iterators in terms of standard C# constructs.</span></span> <span data-ttu-id="479b6-1647">Zde popsaná implementace je založena na stejných zásadách, které používá kompilátor společnosti C# Microsoft, ale nejedná se o udělenou implementaci nebo o jedinou možnost.</span><span class="sxs-lookup"><span data-stu-id="479b6-1647">The implementation described here is based on the same principles used by the Microsoft C# compiler, but it is by no means a mandated implementation or the only one possible.</span></span>

<span data-ttu-id="479b6-1648">Následující `Stack<T>` třída implementuje svou `GetEnumerator` metodu pomocí iterátoru.</span><span class="sxs-lookup"><span data-stu-id="479b6-1648">The following `Stack<T>` class implements its `GetEnumerator` method using an iterator.</span></span> <span data-ttu-id="479b6-1649">Iterátor vytvoří výčet prvků zásobníku v horním pořadí.</span><span class="sxs-lookup"><span data-stu-id="479b6-1649">The iterator enumerates the elements of the stack in top to bottom order.</span></span>

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

<span data-ttu-id="479b6-1650">`GetEnumerator` Metodu lze přeložit do instance třídy enumerátor generovaných kompilátorem, která zapouzdřuje kód do bloku iterátoru, jak je znázorněno v následujícím příkladu.</span><span class="sxs-lookup"><span data-stu-id="479b6-1650">The `GetEnumerator` method can be translated into an instantiation of a compiler-generated enumerator class that encapsulates the code in the iterator block, as shown in the following.</span></span>

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

<span data-ttu-id="479b6-1651">V předchozím překladu je kód v bloku iterátoru převeden na Stavový počítač a umístěn do `MoveNext` metody třídy Enumerator.</span><span class="sxs-lookup"><span data-stu-id="479b6-1651">In the preceding translation, the code in the iterator block is turned into a state machine and placed in the `MoveNext` method of the enumerator class.</span></span> <span data-ttu-id="479b6-1652">Kromě toho je místní proměnná `i` převedena do pole v objektu Enumerator, aby mohla nadále existovat ve voláních `MoveNext`.</span><span class="sxs-lookup"><span data-stu-id="479b6-1652">Furthermore, the local variable `i` is turned into a field in the enumerator object so it can continue to exist across invocations of `MoveNext`.</span></span>

<span data-ttu-id="479b6-1653">V následujícím příkladu je vytištěna Jednoduchá tabulka násobení celých čísel od 1 do 10.</span><span class="sxs-lookup"><span data-stu-id="479b6-1653">The following example prints a simple multiplication table of the integers 1 through 10.</span></span> <span data-ttu-id="479b6-1654">`FromTo` Metoda v příkladu vrátí vyčíslitelného objektu a je implementována pomocí iterátoru.</span><span class="sxs-lookup"><span data-stu-id="479b6-1654">The `FromTo` method in the example returns an enumerable object and is implemented using an iterator.</span></span>

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

<span data-ttu-id="479b6-1655">`FromTo` Metodu lze přeložit do instance výčtové třídy generované kompilátorem, která zapouzdřuje kód do bloku iterátoru, jak je znázorněno v následujícím příkladu.</span><span class="sxs-lookup"><span data-stu-id="479b6-1655">The `FromTo` method can be translated into an instantiation of a compiler-generated enumerable class that encapsulates the code in the iterator block, as shown in the following.</span></span>

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

<span data-ttu-id="479b6-1656">Vyčíslitelné třída implementuje vyčíslitelné rozhraní i rozhraní enumerátoru a umožňuje tak, aby sloužila jako vyčíslitelné i enumerátor.</span><span class="sxs-lookup"><span data-stu-id="479b6-1656">The enumerable class implements both the enumerable interfaces and the enumerator interfaces, enabling it to serve as both an enumerable and an enumerator.</span></span> <span data-ttu-id="479b6-1657">Při prvním `GetEnumerator` vyvolání metody se vrátí vyčíslitelné samotný objekt.</span><span class="sxs-lookup"><span data-stu-id="479b6-1657">The first time the `GetEnumerator` method is invoked, the enumerable object itself is returned.</span></span> <span data-ttu-id="479b6-1658">Následná vyvolání vyčíslitelného objektu `GetEnumerator`, pokud nějaký objekt, vrátí kopii vyčíslitelného objektu.</span><span class="sxs-lookup"><span data-stu-id="479b6-1658">Subsequent invocations of the enumerable object's `GetEnumerator`, if any, return a copy of the enumerable object.</span></span> <span data-ttu-id="479b6-1659">Proto každý vrácený enumerátor má svůj vlastní stav a změny v jednom enumerátoru nebudou mít vliv na další.</span><span class="sxs-lookup"><span data-stu-id="479b6-1659">Thus, each returned enumerator has its own state and changes in one enumerator will not affect another.</span></span> <span data-ttu-id="479b6-1660">`Interlocked.CompareExchange` Metoda se používá k zajištění operace bezpečné pro přístup z více vláken.</span><span class="sxs-lookup"><span data-stu-id="479b6-1660">The `Interlocked.CompareExchange` method is used to ensure thread-safe operation.</span></span>

<span data-ttu-id="479b6-1661">Parametry `from` a`to` jsou přeměněny na pole v vyčíslitelné třídě.</span><span class="sxs-lookup"><span data-stu-id="479b6-1661">The `from` and `to` parameters are turned into fields in the enumerable class.</span></span> <span data-ttu-id="479b6-1662">Vzhledem `from` k tomu, že je v bloku iterátoru `__from` upraveno, je zavedeno další pole, `from` které bude uchovávat počáteční hodnotu zadanou v každém enumerátoru.</span><span class="sxs-lookup"><span data-stu-id="479b6-1662">Because `from` is modified in the iterator block, an additional `__from` field is introduced to hold the initial value given to `from` in each enumerator.</span></span>

<span data-ttu-id="479b6-1663">Metoda vyvolá výjimku `InvalidOperationException` , pokud je volána, když `__state` je `0`. `MoveNext`</span><span class="sxs-lookup"><span data-stu-id="479b6-1663">The `MoveNext` method throws an `InvalidOperationException` if it is called when `__state` is `0`.</span></span> <span data-ttu-id="479b6-1664">To chrání před použitím vyčíslitelného objektu jako objektu enumerátoru bez prvního volání `GetEnumerator`.</span><span class="sxs-lookup"><span data-stu-id="479b6-1664">This protects against use of the enumerable object as an enumerator object without first calling `GetEnumerator`.</span></span>

<span data-ttu-id="479b6-1665">Následující příklad ukazuje jednoduchou stromovou třídu.</span><span class="sxs-lookup"><span data-stu-id="479b6-1665">The following example shows a simple tree class.</span></span> <span data-ttu-id="479b6-1666">Třída implementuje svou `GetEnumerator` metodu pomocí iterátoru. `Tree<T>`</span><span class="sxs-lookup"><span data-stu-id="479b6-1666">The `Tree<T>` class implements its `GetEnumerator` method using an iterator.</span></span> <span data-ttu-id="479b6-1667">Iterátor vytvoří výčet prvků stromu v vpony pořadí.</span><span class="sxs-lookup"><span data-stu-id="479b6-1667">The iterator enumerates the elements of the tree in infix order.</span></span>

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

<span data-ttu-id="479b6-1668">`GetEnumerator` Metodu lze přeložit do instance třídy enumerátor generovaných kompilátorem, která zapouzdřuje kód do bloku iterátoru, jak je znázorněno v následujícím příkladu.</span><span class="sxs-lookup"><span data-stu-id="479b6-1668">The `GetEnumerator` method can be translated into an instantiation of a compiler-generated enumerator class that encapsulates the code in the iterator block, as shown in the following.</span></span>

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

<span data-ttu-id="479b6-1669">Kompilátor generovaný dočasné objekty použitý v `foreach` příkazech se přenese `__left` do polí a `__right` objektu Enumerator.</span><span class="sxs-lookup"><span data-stu-id="479b6-1669">The compiler generated temporaries used in the `foreach` statements are lifted into the `__left` and `__right` fields of the enumerator object.</span></span> <span data-ttu-id="479b6-1670">Pole objektu Enumerator je pečlivě aktualizováno, aby se správná `Dispose()` metoda volala správně, pokud je vyvolána výjimka. `__state`</span><span class="sxs-lookup"><span data-stu-id="479b6-1670">The `__state` field of the enumerator object is carefully updated so that the correct `Dispose()` method will be called correctly if an exception is thrown.</span></span> <span data-ttu-id="479b6-1671">Všimněte si, že není možné zapisovat přeložený kód pomocí jednoduchých `foreach` příkazů.</span><span class="sxs-lookup"><span data-stu-id="479b6-1671">Note that it is not possible to write the translated code with simple `foreach` statements.</span></span>

## <a name="async-functions"></a><span data-ttu-id="479b6-1672">Asynchronní funkce</span><span class="sxs-lookup"><span data-stu-id="479b6-1672">Async functions</span></span>

<span data-ttu-id="479b6-1673">Metoda ([metody](classes.md#methods)) nebo anonymní funkce ([výrazy anonymní funkce](expressions.md#anonymous-function-expressions) `async` ) s modifikátorem se nazývá ***asynchronní funkce***.</span><span class="sxs-lookup"><span data-stu-id="479b6-1673">A method ([Methods](classes.md#methods)) or anonymous function ([Anonymous function expressions](expressions.md#anonymous-function-expressions)) with the `async` modifier is called an ***async function***.</span></span> <span data-ttu-id="479b6-1674">Obecně se pojem ***Async*** používá k popisu libovolného druhu funkce, která má `async` modifikátor.</span><span class="sxs-lookup"><span data-stu-id="479b6-1674">In general, the term ***async*** is used to describe any kind of function that has the `async` modifier.</span></span>

<span data-ttu-id="479b6-1675">Jedná se o chybu při kompilaci pro seznam formálních parametrů asynchronní funkce pro určení `ref` parametrů nebo. `out`</span><span class="sxs-lookup"><span data-stu-id="479b6-1675">It is a compile-time error for the formal parameter list of an async function to specify any `ref` or `out` parameters.</span></span>

<span data-ttu-id="479b6-1676">Typ asynchronní metody musí být buď `void` nebo ***typu úkolu***.</span><span class="sxs-lookup"><span data-stu-id="479b6-1676">The *return_type* of an async method must be either `void` or a ***task type***.</span></span> <span data-ttu-id="479b6-1677">Typy úloh jsou `System.Threading.Tasks.Task` a typy vytvořené z `System.Threading.Tasks.Task<T>`.</span><span class="sxs-lookup"><span data-stu-id="479b6-1677">The task types are `System.Threading.Tasks.Task` and types constructed from `System.Threading.Tasks.Task<T>`.</span></span> <span data-ttu-id="479b6-1678">Pro účely zkrácení je v této kapitole odkazováno `Task` na tyto typy, a `Task<T>`to v uvedeném pořadí.</span><span class="sxs-lookup"><span data-stu-id="479b6-1678">For the sake of brevity, in this chapter these types are referenced as `Task` and `Task<T>`, respectively.</span></span> <span data-ttu-id="479b6-1679">Asynchronní metoda, která vrací typ úkolu, je označována jako vracející úlohy.</span><span class="sxs-lookup"><span data-stu-id="479b6-1679">An async method returning a task type is said to be task-returning.</span></span>

<span data-ttu-id="479b6-1680">Přesná definice typů úloh je definovaná implementace, ale v bodě zobrazení je typ úlohy v jednom ze stavů neúplný, úspěšný nebo chybný.</span><span class="sxs-lookup"><span data-stu-id="479b6-1680">The exact definition of the task types is implementation defined, but from the language's point of view a task type is in one of the states incomplete, succeeded or faulted.</span></span> <span data-ttu-id="479b6-1681">Úloha s chybou zaznamená určitou výjimku.</span><span class="sxs-lookup"><span data-stu-id="479b6-1681">A faulted task records a pertinent exception.</span></span> <span data-ttu-id="479b6-1682">Výsledkem úspěšného `Task<T>` záznamu je výsledek typu `T`.</span><span class="sxs-lookup"><span data-stu-id="479b6-1682">A succeeded `Task<T>` records a result of type `T`.</span></span> <span data-ttu-id="479b6-1683">Typy úloh jsou await, a proto mohou být operandy výrazů await ([výrazy await](expressions.md#await-expressions)).</span><span class="sxs-lookup"><span data-stu-id="479b6-1683">Task types are awaitable, and can therefore be the operands of await expressions ([Await expressions](expressions.md#await-expressions)).</span></span>

<span data-ttu-id="479b6-1684">Asynchronní vyvolání funkce má schopnost pozastavit vyhodnocení pomocí výrazů await ([výrazy await](expressions.md#await-expressions)) ve svém těle.</span><span class="sxs-lookup"><span data-stu-id="479b6-1684">An async function invocation has the ability to suspend evaluation by means of await expressions ([Await expressions](expressions.md#await-expressions)) in its body.</span></span> <span data-ttu-id="479b6-1685">Vyhodnocení může být později obnoveno v okamžiku pozastavení výrazu await pomocí ***delegáta pokračování***.</span><span class="sxs-lookup"><span data-stu-id="479b6-1685">Evaluation may later be resumed at the point of the suspending await expression by means of a ***resumption delegate***.</span></span> <span data-ttu-id="479b6-1686">Delegát pokračování je typu `System.Action`a při jeho vyvolání dojde k vyhodnocení vyvolání asynchronní funkce z výrazu await, kde skončila.</span><span class="sxs-lookup"><span data-stu-id="479b6-1686">The resumption delegate is of type `System.Action`, and when it is invoked, evaluation of the async function invocation will resume from the await expression where it left off.</span></span> <span data-ttu-id="479b6-1687">***Aktuální volající*** asynchronní volání funkce je původní volající, pokud se volání funkce nikdy pozastavilo nebo Poslední volající delegáta pokračování v opačném případě.</span><span class="sxs-lookup"><span data-stu-id="479b6-1687">The ***current caller*** of an async function invocation is the original caller if the function invocation has never been suspended, or the most recent caller of the resumption delegate otherwise.</span></span>

### <a name="evaluation-of-a-task-returning-async-function"></a><span data-ttu-id="479b6-1688">Vyhodnocení asynchronní funkce vracející úlohu</span><span class="sxs-lookup"><span data-stu-id="479b6-1688">Evaluation of a task-returning async function</span></span>

<span data-ttu-id="479b6-1689">Volání asynchronní funkce vracející úlohu způsobí, že se vygeneruje instance vráceného typu úkolu.</span><span class="sxs-lookup"><span data-stu-id="479b6-1689">Invocation of a task-returning async function causes an instance of the returned task type to be generated.</span></span> <span data-ttu-id="479b6-1690">To se označuje jako ***návratová úloha*** asynchronní funkce.</span><span class="sxs-lookup"><span data-stu-id="479b6-1690">This is called the ***return task*** of the async function.</span></span> <span data-ttu-id="479b6-1691">Úloha je zpočátku v neúplném stavu.</span><span class="sxs-lookup"><span data-stu-id="479b6-1691">The task is initially in an incomplete state.</span></span>

<span data-ttu-id="479b6-1692">Text asynchronní funkce se pak vyhodnotí, dokud se buď pozastaví (tím, že se dokončí výraz await), nebo se ukončí, když se ovládací prvek, na který se vrací, vrátí volajícímu, spolu s návratovou úlohou.</span><span class="sxs-lookup"><span data-stu-id="479b6-1692">The async function body is then evaluated until it is either suspended (by reaching an await expression) or terminates, at which point control is returned to the caller, along with the return task.</span></span>

<span data-ttu-id="479b6-1693">Po ukončení textu asynchronní funkce se úloha vrácení přemístí mimo nekompletní stav:</span><span class="sxs-lookup"><span data-stu-id="479b6-1693">When the body of the async function terminates, the return task is moved out of the incomplete state:</span></span>

*  <span data-ttu-id="479b6-1694">Pokud tělo funkce skončí jako výsledek dosažení návratového příkazu nebo konce těla, je do návratové úlohy zaznamenána jakákoli výsledná hodnota, která je vložena do úspěšného stavu.</span><span class="sxs-lookup"><span data-stu-id="479b6-1694">If the function body terminates as the result of reaching a return statement or the end of the body, any result value is recorded in the return task, which is put into a succeeded state.</span></span>
*  <span data-ttu-id="479b6-1695">Pokud tělo funkce skončí jako výsledek nezachycené výjimky ([příkaz throw](statements.md#the-throw-statement)), je výjimka zaznamenána v úloze vrácení, která je umístěna do chybového stavu.</span><span class="sxs-lookup"><span data-stu-id="479b6-1695">If the function body terminates as the result of an uncaught exception ([The throw statement](statements.md#the-throw-statement)) the exception is recorded in the return task which is put into a faulted state.</span></span>

### <a name="evaluation-of-a-void-returning-async-function"></a><span data-ttu-id="479b6-1696">Vyhodnocení asynchronní funkce vracející typ void</span><span class="sxs-lookup"><span data-stu-id="479b6-1696">Evaluation of a void-returning async function</span></span>

<span data-ttu-id="479b6-1697">Pokud je `void`návratový typ asynchronní funkce, vyhodnocování se liší od výše uvedeného následujícím způsobem: Vzhledem k tomu, že není vrácen žádný úkol, funkce místo toho komunikuje s dokončováním a výjimkou do ***kontextu synchronizace***aktuálního vlákna.</span><span class="sxs-lookup"><span data-stu-id="479b6-1697">If the return type of the async function is `void`, evaluation differs from the above in the following way: Because no task is returned, the function instead communicates completion and exceptions to the current thread's ***synchronization context***.</span></span> <span data-ttu-id="479b6-1698">Přesná definice synchronizačního kontextu je závislá na implementaci, ale je reprezentace "Where", na kterém je spuštěno aktuální vlákno.</span><span class="sxs-lookup"><span data-stu-id="479b6-1698">The exact definition of synchronization context is implementation-dependent, but is a representation of "where" the current thread is running.</span></span> <span data-ttu-id="479b6-1699">Kontext synchronizace je upozorněn, když je zahájeno vyhodnocení asynchronní funkce vracející anulování, úspěšné dokončení nebo způsobí vyvolání nezachycené výjimky.</span><span class="sxs-lookup"><span data-stu-id="479b6-1699">The synchronization context is notified when evaluation of a void-returning async function commences, completes successfully, or causes an uncaught exception to be thrown.</span></span>

<span data-ttu-id="479b6-1700">Díky tomu může kontext sledovat, kolik asynchronních funkcí vracejících se změnami je spuštěných v rámci něj, a rozhodnout se, jak šířit výjimky, které z nich přicházejí.</span><span class="sxs-lookup"><span data-stu-id="479b6-1700">This allows the context to keep track of how many void-returning async functions are running under it, and to decide how to propagate exceptions coming out of them.</span></span>
