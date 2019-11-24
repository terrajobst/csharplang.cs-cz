---
ms.openlocfilehash: 6dd1dde67597b2125de9a1aa2fab9144128d533f
ms.sourcegitcommit: 892af9016b3317a8fae12d195014dc38ba51cf16
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/01/2019
ms.locfileid: "71704032"
---
# <a name="structs"></a><span data-ttu-id="71eb6-101">Struktury</span><span class="sxs-lookup"><span data-stu-id="71eb6-101">Structs</span></span>

<span data-ttu-id="71eb6-102">Struktury jsou podobné třídám, které představují datové struktury, které mohou obsahovat datové členy a členy funkce.</span><span class="sxs-lookup"><span data-stu-id="71eb6-102">Structs are similar to classes in that they represent data structures that can contain data members and function members.</span></span> <span data-ttu-id="71eb6-103">Nicméně na rozdíl od tříd, struktury jsou typy hodnot a nevyžadují přidělení haldy.</span><span class="sxs-lookup"><span data-stu-id="71eb6-103">However, unlike classes, structs are value types and do not require heap allocation.</span></span> <span data-ttu-id="71eb6-104">Proměnná typu struktury přímo obsahuje data struktury, zatímco proměnná typu třídy obsahuje odkaz na data, druhá je známá jako objekt.</span><span class="sxs-lookup"><span data-stu-id="71eb6-104">A variable of a struct type directly contains the data of the struct, whereas a variable of a class type contains a reference to the data, the latter known as an object.</span></span>

<span data-ttu-id="71eb6-105">Struktury jsou zvláště užitečné pro malé datové struktury, které mají sémantiku hodnot.</span><span class="sxs-lookup"><span data-stu-id="71eb6-105">Structs are particularly useful for small data structures that have value semantics.</span></span> <span data-ttu-id="71eb6-106">Komplexní čísla, body v systému souřadnic nebo páry klíč-hodnota ve slovníku jsou všechny dobrými příklady struktur.</span><span class="sxs-lookup"><span data-stu-id="71eb6-106">Complex numbers, points in a coordinate system, or key-value pairs in a dictionary are all good examples of structs.</span></span> <span data-ttu-id="71eb6-107">Klíč k těmto datovým strukturám je, že mají několik datových členů, které nevyžadují použití dědičnosti nebo referenční identity, a že je lze pohodlně implementovat pomocí sémantiky hodnot, kde přiřazení kopíruje hodnotu namísto odkazu.</span><span class="sxs-lookup"><span data-stu-id="71eb6-107">Key to these data structures is that they have few data members, that they do not require use of inheritance or referential identity, and that they can be conveniently implemented using value semantics where assignment copies the value instead of the reference.</span></span>

<span data-ttu-id="71eb6-108">Jak je popsáno v [jednoduchých typech](types.md#simple-types), jednoduché typy poskytované C#, například `int`, `double`a `bool`, jsou ve skutečnosti všechny typy struktury.</span><span class="sxs-lookup"><span data-stu-id="71eb6-108">As described in [Simple types](types.md#simple-types), the simple types provided by C#, such as `int`, `double`, and `bool`, are in fact all struct types.</span></span> <span data-ttu-id="71eb6-109">Stejně jako tyto předdefinované typy jsou struktury, lze také použít struktury a přetížení operátoru pro implementaci nových primitivních typů v C# jazyce.</span><span class="sxs-lookup"><span data-stu-id="71eb6-109">Just as these predefined types are structs, it is also possible to use structs and operator overloading to implement new "primitive" types in the C# language.</span></span> <span data-ttu-id="71eb6-110">Dva příklady těchto typů jsou uvedeny na konci této kapitoly ([Příklady struktury](structs.md#struct-examples)).</span><span class="sxs-lookup"><span data-stu-id="71eb6-110">Two examples of such types are given at the end of this chapter ([Struct examples](structs.md#struct-examples)).</span></span>

## <a name="struct-declarations"></a><span data-ttu-id="71eb6-111">Deklarace struktury</span><span class="sxs-lookup"><span data-stu-id="71eb6-111">Struct declarations</span></span>

<span data-ttu-id="71eb6-112">*Struct_declaration* je *type_declaration* ([deklarace typu](namespaces.md#type-declarations)), která deklaruje novou strukturu:</span><span class="sxs-lookup"><span data-stu-id="71eb6-112">A *struct_declaration* is a *type_declaration* ([Type declarations](namespaces.md#type-declarations)) that declares a new struct:</span></span>

```antlr
struct_declaration
    : attributes? struct_modifier* 'partial'? 'struct' identifier type_parameter_list?
      struct_interfaces? type_parameter_constraints_clause* struct_body ';'?
    ;
```

<span data-ttu-id="71eb6-113">*Struct_declaration* se skládá z volitelné sady *atributů* ([atributů](attributes.md)) následovaný volitelnou sadou *struct_modifier*s ([modifikátory struktury](structs.md#struct-modifiers)) následovaným volitelným modifikátorem `partial` následovaným klíčovým slovem `struct` a *identifikátorem* , který je názvem struktury, následovaným *nepovinným Type_parameter_list specifikací* ([parametry typu](classes.md#type-parameters)) následovanými volitelnou *struct_interfaces* specifikací ([částečný modifikátor ](structs.md#partial-modifier))) následovaný volitelnou specifikací *type_parameter_constraints_clause*s ([omezeními parametrů typu](classes.md#type-parameter-constraints)), za kterými následuje *struct_body* ([tělo struktury](structs.md#struct-body)), volitelně následovaný středníkem.</span><span class="sxs-lookup"><span data-stu-id="71eb6-113">A *struct_declaration* consists of an optional set of *attributes* ([Attributes](attributes.md)), followed by an optional set of *struct_modifier*s ([Struct modifiers](structs.md#struct-modifiers)), followed by an optional `partial` modifier, followed by the keyword `struct` and an *identifier* that names the struct, followed by an optional *type_parameter_list* specification ([Type parameters](classes.md#type-parameters)), followed by an optional *struct_interfaces* specification ([Partial modifier](structs.md#partial-modifier)) ), followed by an optional *type_parameter_constraints_clause*s specification ([Type parameter constraints](classes.md#type-parameter-constraints)), followed by a *struct_body* ([Struct body](structs.md#struct-body)), optionally followed by a semicolon.</span></span>

### <a name="struct-modifiers"></a><span data-ttu-id="71eb6-114">Modifikátory struktury</span><span class="sxs-lookup"><span data-stu-id="71eb6-114">Struct modifiers</span></span>

<span data-ttu-id="71eb6-115">*Struct_declaration* může volitelně zahrnovat posloupnost modifikátorů struktury:</span><span class="sxs-lookup"><span data-stu-id="71eb6-115">A *struct_declaration* may optionally include a sequence of struct modifiers:</span></span>

```antlr
struct_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    | struct_modifier_unsafe
    ;
```

<span data-ttu-id="71eb6-116">Jedná se o chybu při kompilaci, aby se stejný modifikátor zobrazoval víckrát v deklaraci struktury.</span><span class="sxs-lookup"><span data-stu-id="71eb6-116">It is a compile-time error for the same modifier to appear multiple times in a struct declaration.</span></span>

<span data-ttu-id="71eb6-117">Modifikátory deklarace struktury mají stejný význam jako deklarace třídy ([deklarace tříd](classes.md#class-declarations)).</span><span class="sxs-lookup"><span data-stu-id="71eb6-117">The modifiers of a struct declaration have the same meaning as those of a class declaration ([Class declarations](classes.md#class-declarations)).</span></span>

### <a name="partial-modifier"></a><span data-ttu-id="71eb6-118">Částečný modifikátor</span><span class="sxs-lookup"><span data-stu-id="71eb6-118">Partial modifier</span></span>

<span data-ttu-id="71eb6-119">Modifikátor `partial` označuje, že tato *struct_declaration* je částečná deklarace typu.</span><span class="sxs-lookup"><span data-stu-id="71eb6-119">The `partial` modifier indicates that this *struct_declaration* is a partial type declaration.</span></span> <span data-ttu-id="71eb6-120">Vícenásobná deklarace částečné struktury se stejným názvem v rámci ohraničujícího oboru názvů nebo deklarace typu jsou zkombinovány o jednu deklaraci struktury za základě pravidel určených v [částečných typech](classes.md#partial-types).</span><span class="sxs-lookup"><span data-stu-id="71eb6-120">Multiple partial struct declarations with the same name within an enclosing namespace or type declaration combine to form one struct declaration, following the rules specified in [Partial types](classes.md#partial-types).</span></span>

### <a name="struct-interfaces"></a><span data-ttu-id="71eb6-121">Rozhraní struktury</span><span class="sxs-lookup"><span data-stu-id="71eb6-121">Struct interfaces</span></span>

<span data-ttu-id="71eb6-122">Deklarace struktury může zahrnovat specifikaci *struct_interfaces* . v takovém případě se pro strukturu říká přímá implementace daných typů rozhraní.</span><span class="sxs-lookup"><span data-stu-id="71eb6-122">A struct declaration may include a *struct_interfaces* specification, in which case the struct is said to directly implement the given interface types.</span></span>

```antlr
struct_interfaces
    : ':' interface_type_list
    ;
```

<span data-ttu-id="71eb6-123">Implementace rozhraní jsou podrobněji popsány v [implementacích rozhraní](interfaces.md#interface-implementations).</span><span class="sxs-lookup"><span data-stu-id="71eb6-123">Interface implementations are discussed further in [Interface implementations](interfaces.md#interface-implementations).</span></span>

### <a name="struct-body"></a><span data-ttu-id="71eb6-124">Tělo struktury</span><span class="sxs-lookup"><span data-stu-id="71eb6-124">Struct body</span></span>

<span data-ttu-id="71eb6-125">*Struct_body* struktury definuje členy struktury.</span><span class="sxs-lookup"><span data-stu-id="71eb6-125">The *struct_body* of a struct defines the members of the struct.</span></span>

```antlr
struct_body
    : '{' struct_member_declaration* '}'
    ;
```

## <a name="struct-members"></a><span data-ttu-id="71eb6-126">Členové struktury</span><span class="sxs-lookup"><span data-stu-id="71eb6-126">Struct members</span></span>

<span data-ttu-id="71eb6-127">Členy struktury se skládají ze členů zavedených jeho *struct_member_declaration*s a členy zděděnými z `System.ValueType`typu.</span><span class="sxs-lookup"><span data-stu-id="71eb6-127">The members of a struct consist of the members introduced by its *struct_member_declaration*s and the members inherited from the type `System.ValueType`.</span></span>

```antlr
struct_member_declaration
    : constant_declaration
    | field_declaration
    | method_declaration
    | property_declaration
    | event_declaration
    | indexer_declaration
    | operator_declaration
    | constructor_declaration
    | static_constructor_declaration
    | type_declaration
    | struct_member_declaration_unsafe
    ;
```

<span data-ttu-id="71eb6-128">S výjimkou rozdílů uvedených v [rozdílech třídy a struktury](structs.md#class-and-struct-differences)jsou popisy členů třídy, které jsou zadány ve [členech třídy](classes.md#class-members) pomocí [iterátorů](classes.md#iterators) , použity také pro členy struktury.</span><span class="sxs-lookup"><span data-stu-id="71eb6-128">Except for the differences noted in [Class and struct differences](structs.md#class-and-struct-differences), the descriptions of class members provided in [Class members](classes.md#class-members) through [Iterators](classes.md#iterators) apply to struct members as well.</span></span>

## <a name="class-and-struct-differences"></a><span data-ttu-id="71eb6-129">Rozdíly třídy a struktury</span><span class="sxs-lookup"><span data-stu-id="71eb6-129">Class and struct differences</span></span>

<span data-ttu-id="71eb6-130">Struktury se liší od tříd v několika důležitých způsobech:</span><span class="sxs-lookup"><span data-stu-id="71eb6-130">Structs differ from classes in several important ways:</span></span>

*  <span data-ttu-id="71eb6-131">Struktury jsou typy hodnot ([sémantika hodnot](structs.md#value-semantics)).</span><span class="sxs-lookup"><span data-stu-id="71eb6-131">Structs are value types ([Value semantics](structs.md#value-semantics)).</span></span>
*  <span data-ttu-id="71eb6-132">Všechny typy struktury implicitně dědí z `System.ValueType` třídy ([Dědičnost](structs.md#inheritance)).</span><span class="sxs-lookup"><span data-stu-id="71eb6-132">All struct types implicitly inherit from the class `System.ValueType` ([Inheritance](structs.md#inheritance)).</span></span>
*  <span data-ttu-id="71eb6-133">Přiřazení k proměnné typu struktury vytvoří kopii přiřazené hodnoty ([přiřazení](structs.md#assignment)).</span><span class="sxs-lookup"><span data-stu-id="71eb6-133">Assignment to a variable of a struct type creates a copy of the value being assigned ([Assignment](structs.md#assignment)).</span></span>
*  <span data-ttu-id="71eb6-134">Výchozí hodnota struktury je hodnota vytvořená nastavením všech polí Typ hodnoty na jejich výchozí hodnotu a všechna pole odkazového typu na `null` ([výchozí hodnoty](structs.md#default-values)).</span><span class="sxs-lookup"><span data-stu-id="71eb6-134">The default value of a struct is the value produced by setting all value type fields to their default value and all reference type fields to `null` ([Default values](structs.md#default-values)).</span></span>
*  <span data-ttu-id="71eb6-135">Operace zabalení a rozbalení se používají k převodu mezi typem struktury a `object` (zabalení[a rozbalení](structs.md#boxing-and-unboxing)).</span><span class="sxs-lookup"><span data-stu-id="71eb6-135">Boxing and unboxing operations are used to convert between a struct type and `object` ([Boxing and unboxing](structs.md#boxing-and-unboxing)).</span></span>
*  <span data-ttu-id="71eb6-136">Význam `this` se pro struktury ([Tento přístup](expressions.md#this-access)) liší.</span><span class="sxs-lookup"><span data-stu-id="71eb6-136">The meaning of `this` is different for structs ([This access](expressions.md#this-access)).</span></span>
*  <span data-ttu-id="71eb6-137">Deklarace polí instance pro strukturu nejsou povoleny pro zahrnutí inicializátorů proměnných ([Inicializátory pole](structs.md#field-initializers)).</span><span class="sxs-lookup"><span data-stu-id="71eb6-137">Instance field declarations for a struct are not permitted to include variable initializers ([Field initializers](structs.md#field-initializers)).</span></span>
*  <span data-ttu-id="71eb6-138">Struktura není povolená pro deklaraci konstruktoru instance bez parametrů ([konstruktory](structs.md#constructors)).</span><span class="sxs-lookup"><span data-stu-id="71eb6-138">A struct is not permitted to declare a parameterless instance constructor ([Constructors](structs.md#constructors)).</span></span>
*  <span data-ttu-id="71eb6-139">Struktura není oprávněná deklarovat destruktor ([destruktory](structs.md#destructors)).</span><span class="sxs-lookup"><span data-stu-id="71eb6-139">A struct is not permitted to declare a destructor ([Destructors](structs.md#destructors)).</span></span>

### <a name="value-semantics"></a><span data-ttu-id="71eb6-140">Sémantika hodnoty</span><span class="sxs-lookup"><span data-stu-id="71eb6-140">Value semantics</span></span>

<span data-ttu-id="71eb6-141">Struktury jsou typy hodnot ([typy hodnot](types.md#value-types)) a jsou označeny jako sémantika hodnoty.</span><span class="sxs-lookup"><span data-stu-id="71eb6-141">Structs are value types ([Value types](types.md#value-types)) and are said to have value semantics.</span></span> <span data-ttu-id="71eb6-142">Třídy jsou na druhé straně odkazy na typy ([odkazové typy](types.md#reference-types)) a jsou označeny jako referenční sémantika.</span><span class="sxs-lookup"><span data-stu-id="71eb6-142">Classes, on the other hand, are reference types ([Reference types](types.md#reference-types)) and are said to have reference semantics.</span></span>

<span data-ttu-id="71eb6-143">Proměnná typu struktury přímo obsahuje data struktury, zatímco proměnná typu třídy obsahuje odkaz na data, druhá je známá jako objekt.</span><span class="sxs-lookup"><span data-stu-id="71eb6-143">A variable of a struct type directly contains the data of the struct, whereas a variable of a class type contains a reference to the data, the latter known as an object.</span></span> <span data-ttu-id="71eb6-144">Když `B` struktury obsahuje pole instance typu `A` a `A` je typ struktury, jedná se o chybu při kompilaci, která `A` závisí na `B` nebo typu vytvořeném z `B`.</span><span class="sxs-lookup"><span data-stu-id="71eb6-144">When a struct `B` contains an instance field of type `A` and `A` is a struct type, it is a compile-time error for `A` to depend on `B` or a type constructed from `B`.</span></span> <span data-ttu-id="71eb6-145">Struktura `X` ***přímo závisí na*** struktuře `Y`, pokud `X` obsahuje pole instance typu `Y`.</span><span class="sxs-lookup"><span data-stu-id="71eb6-145">A struct `X` ***directly depends on*** a struct `Y` if `X` contains an instance field of type `Y`.</span></span> <span data-ttu-id="71eb6-146">V této definici je kompletní sada struktur, na které struktura závisí, ***přímý uzávěr přímo závislá na*** vztahu.</span><span class="sxs-lookup"><span data-stu-id="71eb6-146">Given this definition, the complete set of structs upon which a struct depends is the transitive closure of the ***directly depends on*** relationship.</span></span>  <span data-ttu-id="71eb6-147">Například</span><span class="sxs-lookup"><span data-stu-id="71eb6-147">For example</span></span>
```csharp
struct Node
{
    int data;
    Node next; // error, Node directly depends on itself
}
```
<span data-ttu-id="71eb6-148">je chyba, protože `Node` obsahuje pole instance vlastního typu.</span><span class="sxs-lookup"><span data-stu-id="71eb6-148">is an error because `Node` contains an instance field of its own type.</span></span>  <span data-ttu-id="71eb6-149">Jiný příklad</span><span class="sxs-lookup"><span data-stu-id="71eb6-149">Another example</span></span>
```csharp
struct A { B b; }

struct B { C c; }

struct C { A a; }
```
<span data-ttu-id="71eb6-150">je chyba, protože každý z typů `A`, `B`a `C` závisí na sobě navzájem.</span><span class="sxs-lookup"><span data-stu-id="71eb6-150">is an error because each of the types `A`, `B`, and `C` depend on each other.</span></span>

<span data-ttu-id="71eb6-151">U tříd je možné, aby dvě proměnné odkazovaly na stejný objekt a bylo tak možné, aby operace na jedné proměnné ovlivnily objekt, na který je odkazováno jinou proměnnou.</span><span class="sxs-lookup"><span data-stu-id="71eb6-151">With classes, it is possible for two variables to reference the same object, and thus possible for operations on one variable to affect the object referenced by the other variable.</span></span> <span data-ttu-id="71eb6-152">U struktur mají proměnné, které mají svou vlastní kopii dat (s výjimkou `ref` a `out` proměnných parametrů), a není možné, aby operace na jednom byly ovlivněny druhým.</span><span class="sxs-lookup"><span data-stu-id="71eb6-152">With structs, the variables each have their own copy of the data (except in the case of `ref` and `out` parameter variables), and it is not possible for operations on one to affect the other.</span></span> <span data-ttu-id="71eb6-153">Vzhledem k tomu, že struktury neodkazují na typy, není možné `null`hodnoty typu struktury.</span><span class="sxs-lookup"><span data-stu-id="71eb6-153">Furthermore, because structs are not reference types, it is not possible for values of a struct type to be `null`.</span></span>

<span data-ttu-id="71eb6-154">Daná deklarace</span><span class="sxs-lookup"><span data-stu-id="71eb6-154">Given the declaration</span></span>
```csharp
struct Point
{
    public int x, y;

    public Point(int x, int y) {
        this.x = x;
        this.y = y;
    }
}
```
<span data-ttu-id="71eb6-155">fragment kódu</span><span class="sxs-lookup"><span data-stu-id="71eb6-155">the code fragment</span></span>
```csharp
Point a = new Point(10, 10);
Point b = a;
a.x = 100;
System.Console.WriteLine(b.x);
```
<span data-ttu-id="71eb6-156">Vytvoří výstup hodnoty `10`.</span><span class="sxs-lookup"><span data-stu-id="71eb6-156">outputs the value `10`.</span></span> <span data-ttu-id="71eb6-157">Přiřazení `a` pro `b` vytvoří kopii hodnoty a `b` tím není ovlivněno přiřazením `a.x`.</span><span class="sxs-lookup"><span data-stu-id="71eb6-157">The assignment of `a` to `b` creates a copy of the value, and `b` is thus unaffected by the assignment to `a.x`.</span></span> <span data-ttu-id="71eb6-158">Místo toho byly `Point` deklarovány jako třídy, výstup by byl `100`, protože `a` a `b` by odkazovaly na stejný objekt.</span><span class="sxs-lookup"><span data-stu-id="71eb6-158">Had `Point` instead been declared as a class, the output would be `100` because `a` and `b` would reference the same object.</span></span>

### <a name="inheritance"></a><span data-ttu-id="71eb6-159">Dědičnost</span><span class="sxs-lookup"><span data-stu-id="71eb6-159">Inheritance</span></span>

<span data-ttu-id="71eb6-160">Všechny typy struktury implicitně dědí z `System.ValueType`třídy, což zase dědí z třídy `object`.</span><span class="sxs-lookup"><span data-stu-id="71eb6-160">All struct types implicitly inherit from the class `System.ValueType`, which, in turn, inherits from class `object`.</span></span> <span data-ttu-id="71eb6-161">Deklarace struktury může určovat seznam implementovaných rozhraní, ale není možné, aby deklarace struktury určovala základní třídu.</span><span class="sxs-lookup"><span data-stu-id="71eb6-161">A struct declaration may specify a list of implemented interfaces, but it is not possible for a struct declaration to specify a base class.</span></span>

<span data-ttu-id="71eb6-162">Typy struktury nejsou nikdy abstraktní a jsou vždy implicitně zapečetěné.</span><span class="sxs-lookup"><span data-stu-id="71eb6-162">Struct types are never abstract and are always implicitly sealed.</span></span> <span data-ttu-id="71eb6-163">V deklaraci struktury nejsou proto povoleny modifikátory `abstract` a `sealed`.</span><span class="sxs-lookup"><span data-stu-id="71eb6-163">The `abstract` and `sealed` modifiers are therefore not permitted in a struct declaration.</span></span>

<span data-ttu-id="71eb6-164">Vzhledem k tomu, že dědičnost není pro struktury podporovaná, deklarovaná přístupnost člena struktury nemůže být `protected` ani `protected internal`.</span><span class="sxs-lookup"><span data-stu-id="71eb6-164">Since inheritance isn't supported for structs, the declared accessibility of a struct member cannot be `protected` or `protected internal`.</span></span>

<span data-ttu-id="71eb6-165">Členy funkce ve struktuře nelze `abstract` ani `virtual`a modifikátor `override` je povolen pouze pro přepsání metod zděděných z `System.ValueType`.</span><span class="sxs-lookup"><span data-stu-id="71eb6-165">Function members in a struct cannot be `abstract` or `virtual`, and the `override` modifier is allowed only to override methods inherited from `System.ValueType`.</span></span>

### <a name="assignment"></a><span data-ttu-id="71eb6-166">Přiřazení</span><span class="sxs-lookup"><span data-stu-id="71eb6-166">Assignment</span></span>

<span data-ttu-id="71eb6-167">Přiřazení k proměnné typu struktury vytvoří kopii hodnoty, která je přiřazena.</span><span class="sxs-lookup"><span data-stu-id="71eb6-167">Assignment to a variable of a struct type creates a copy of the value being assigned.</span></span> <span data-ttu-id="71eb6-168">To se liší od přiřazení k proměnné typu třídy, které kopírují odkaz, ale nikoli objekt identifikovaný odkazem.</span><span class="sxs-lookup"><span data-stu-id="71eb6-168">This differs from assignment to a variable of a class type, which copies the reference but not the object identified by the reference.</span></span>

<span data-ttu-id="71eb6-169">Podobně jako u přiřazení, pokud je struktura předána jako parametr hodnoty nebo vrácena jako výsledek členu funkce, je vytvořena kopie struktury.</span><span class="sxs-lookup"><span data-stu-id="71eb6-169">Similar to an assignment, when a struct is passed as a value parameter or returned as the result of a function member, a copy of the struct is created.</span></span> <span data-ttu-id="71eb6-170">Struktura může být předána odkazem na člena funkce pomocí parametru `ref` nebo `out`.</span><span class="sxs-lookup"><span data-stu-id="71eb6-170">A struct may be passed by reference to a function member using a `ref` or `out` parameter.</span></span>

<span data-ttu-id="71eb6-171">Pokud je cílem přiřazení vlastnost nebo indexer struktury, výraz instance přidružený k vlastnosti nebo přístupu indexeru musí být klasifikován jako proměnná.</span><span class="sxs-lookup"><span data-stu-id="71eb6-171">When a property or indexer of a struct is the target of an assignment, the instance expression associated with the property or indexer access must be classified as a variable.</span></span> <span data-ttu-id="71eb6-172">Pokud je výraz instance klasifikován jako hodnota, dojde k chybě při kompilaci.</span><span class="sxs-lookup"><span data-stu-id="71eb6-172">If the instance expression is classified as a value, a compile-time error occurs.</span></span> <span data-ttu-id="71eb6-173">Tato informace je podrobněji popsána v tématu [jednoduché přiřazení](expressions.md#simple-assignment).</span><span class="sxs-lookup"><span data-stu-id="71eb6-173">This is described in further detail in [Simple assignment](expressions.md#simple-assignment).</span></span>

### <a name="default-values"></a><span data-ttu-id="71eb6-174">Výchozí hodnoty</span><span class="sxs-lookup"><span data-stu-id="71eb6-174">Default values</span></span>

<span data-ttu-id="71eb6-175">Jak je popsáno ve [výchozích hodnotách](variables.md#default-values), několik druhů proměnných se automaticky inicializuje na výchozí hodnotu při jejich vytvoření.</span><span class="sxs-lookup"><span data-stu-id="71eb6-175">As described in [Default values](variables.md#default-values), several kinds of variables are automatically initialized to their default value when they are created.</span></span> <span data-ttu-id="71eb6-176">Pro proměnné typů tříd a jiných typů odkazů je tato výchozí hodnota `null`.</span><span class="sxs-lookup"><span data-stu-id="71eb6-176">For variables of class types and other reference types, this default value is `null`.</span></span> <span data-ttu-id="71eb6-177">Nicméně vzhledem k tomu, že struktury jsou typy hodnot, které nemohou být `null`, výchozí hodnota struktury je hodnota vytvořená nastavením všech polí hodnotového typu na jejich výchozí hodnotu a všechna pole odkazového typu na `null`.</span><span class="sxs-lookup"><span data-stu-id="71eb6-177">However, since structs are value types that cannot be `null`, the default value of a struct is the value produced by setting all value type fields to their default value and all reference type fields to `null`.</span></span>

<span data-ttu-id="71eb6-178">V příkladu se odkazuje na strukturu `Point` deklarované výše, příklad</span><span class="sxs-lookup"><span data-stu-id="71eb6-178">Referring to the `Point` struct declared above, the example</span></span>
```csharp
Point[] a = new Point[100];
```
<span data-ttu-id="71eb6-179">Inicializuje všechny `Point` v poli na hodnotu vytvořenou nastavením `x` a `y` polí na hodnotu nula.</span><span class="sxs-lookup"><span data-stu-id="71eb6-179">initializes each `Point` in the array to the value produced by setting the `x` and `y` fields to zero.</span></span>

<span data-ttu-id="71eb6-180">Výchozí hodnota struktury odpovídá hodnotě vrácené výchozím konstruktorem struktury ([výchozí konstruktory](types.md#default-constructors)).</span><span class="sxs-lookup"><span data-stu-id="71eb6-180">The default value of a struct corresponds to the value returned by the default constructor of the struct ([Default constructors](types.md#default-constructors)).</span></span> <span data-ttu-id="71eb6-181">Na rozdíl od třídy struktura není povolena k deklaraci konstruktoru instance bez parametrů.</span><span class="sxs-lookup"><span data-stu-id="71eb6-181">Unlike a class, a struct is not permitted to declare a parameterless instance constructor.</span></span> <span data-ttu-id="71eb6-182">Místo toho má každá struktura implicitně konstruktor instance bez parametrů, který vždycky vrací hodnotu, která je výsledkem nastavení všech polí typu hodnoty na výchozí hodnotu a všechna pole odkazového typu na `null`.</span><span class="sxs-lookup"><span data-stu-id="71eb6-182">Instead, every struct implicitly has a parameterless instance constructor which always returns the value that results from setting all value type fields to their default value and all reference type fields to `null`.</span></span>

<span data-ttu-id="71eb6-183">Struktury by měly být navržené tak, aby braly výchozí stav inicializace na platný stav.</span><span class="sxs-lookup"><span data-stu-id="71eb6-183">Structs should be designed to consider the default initialization state a valid state.</span></span> <span data-ttu-id="71eb6-184">V příkladu</span><span class="sxs-lookup"><span data-stu-id="71eb6-184">In the example</span></span>
```csharp
using System;

struct KeyValuePair
{
    string key;
    string value;

    public KeyValuePair(string key, string value) {
        if (key == null || value == null) throw new ArgumentException();
        this.key = key;
        this.value = value;
    }
}
```
<span data-ttu-id="71eb6-185">uživatelsky definovaný konstruktor instance chrání proti hodnotám null pouze tam, kde je explicitně volána.</span><span class="sxs-lookup"><span data-stu-id="71eb6-185">the user-defined instance constructor protects against null values only where it is explicitly called.</span></span> <span data-ttu-id="71eb6-186">V případech, kdy `KeyValuePair` proměnná podléhá inicializaci výchozí hodnoty, budou mít pole `key` a `value` hodnotu null a struktura musí být připravená na zpracování tohoto stavu.</span><span class="sxs-lookup"><span data-stu-id="71eb6-186">In cases where a `KeyValuePair` variable is subject to default value initialization, the `key` and `value` fields will be null, and the struct must be prepared to handle this state.</span></span>

### <a name="boxing-and-unboxing"></a><span data-ttu-id="71eb6-187">Zabalení a rozbalení</span><span class="sxs-lookup"><span data-stu-id="71eb6-187">Boxing and unboxing</span></span>

<span data-ttu-id="71eb6-188">Hodnota typu třídy může být převedena na typ `object` nebo na typ rozhraní, který je implementován třídou jednoduše tím, že v době kompilace považuje odkaz za jiný typ.</span><span class="sxs-lookup"><span data-stu-id="71eb6-188">A value of a class type can be converted to type `object` or to an interface type that is implemented by the class simply by treating the reference as another type at compile-time.</span></span> <span data-ttu-id="71eb6-189">Stejně tak hodnota typu `object` nebo hodnota typu rozhraní lze převést zpět na typ třídy beze změny odkazu (ale v tomto případě je vyžadována kontrolní rutina typu runtime).</span><span class="sxs-lookup"><span data-stu-id="71eb6-189">Likewise, a value of type `object` or a value of an interface type can be converted back to a class type without changing the reference (but of course a run-time type check is required in this case).</span></span>

<span data-ttu-id="71eb6-190">Vzhledem k tomu, že struktury nejsou odkazy na typy, jsou tyto operace pro typy struktury implementovány jinak.</span><span class="sxs-lookup"><span data-stu-id="71eb6-190">Since structs are not reference types, these operations are implemented differently for struct types.</span></span> <span data-ttu-id="71eb6-191">Je-li hodnota typu struktury převedena na typ `object` nebo na typ rozhraní, který je implementován strukturou, dojde k operaci zabalení.</span><span class="sxs-lookup"><span data-stu-id="71eb6-191">When a value of a struct type is converted to type `object` or to an interface type that is implemented by the struct, a boxing operation takes place.</span></span> <span data-ttu-id="71eb6-192">Podobně, pokud je hodnota typu `object` nebo hodnota typu rozhraní převedena zpět na typ struktury, bude provedena operace rozbalení.</span><span class="sxs-lookup"><span data-stu-id="71eb6-192">Likewise, when a value of type `object` or a value of an interface type is converted back to a struct type, an unboxing operation takes place.</span></span> <span data-ttu-id="71eb6-193">Klíčovým rozdílem ze stejných operací na typech tříd je, že zabalení a rozbalení zkopíruje hodnotu struktury buď do, nebo z zabalené instance.</span><span class="sxs-lookup"><span data-stu-id="71eb6-193">A key difference from the same operations on class types is that boxing and unboxing copies the struct value either into or out of the boxed instance.</span></span> <span data-ttu-id="71eb6-194">Proto se po zabalení nebo rozbalení operace změny provedené v nezabalené struktuře neprojeví v zabalené struktuře.</span><span class="sxs-lookup"><span data-stu-id="71eb6-194">Thus, following a boxing or unboxing operation, changes made to the unboxed struct are not reflected in the boxed struct.</span></span>

<span data-ttu-id="71eb6-195">Když typ struktury přepíše virtuální metodu děděnou z `System.Object` (například `Equals`, `GetHashCode`nebo `ToString`), volání virtuální metody prostřednictvím instance typu struktury nezpůsobí, že dojde k zabalení.</span><span class="sxs-lookup"><span data-stu-id="71eb6-195">When a struct type overrides a virtual method inherited from `System.Object` (such as `Equals`, `GetHashCode`, or `ToString`), invocation of the virtual method through an instance of the struct type does not cause boxing to occur.</span></span> <span data-ttu-id="71eb6-196">To platí i v případě, že se jako parametr typu používá struktura a k vyvolání dojde prostřednictvím instance typu parametru typu.</span><span class="sxs-lookup"><span data-stu-id="71eb6-196">This is true even when the struct is used as a type parameter and the invocation occurs through an instance of the type parameter type.</span></span> <span data-ttu-id="71eb6-197">Příklad:</span><span class="sxs-lookup"><span data-stu-id="71eb6-197">For example:</span></span>
```csharp
using System;

struct Counter
{
    int value;

    public override string ToString() {
        value++;
        return value.ToString();
    }
}

class Program
{
    static void Test<T>() where T: new() {
        T x = new T();
        Console.WriteLine(x.ToString());
        Console.WriteLine(x.ToString());
        Console.WriteLine(x.ToString());
    }

    static void Main() {
        Test<Counter>();
    }
}
```

<span data-ttu-id="71eb6-198">Výstup programu je:</span><span class="sxs-lookup"><span data-stu-id="71eb6-198">The output of the program is:</span></span>
```console
1
2
3
```

<span data-ttu-id="71eb6-199">I když je špatný styl `ToString` mít vedlejší účinky, příklad ukazuje, že žádné zabalení neproběhlo pro tři vyvolání `x.ToString()`.</span><span class="sxs-lookup"><span data-stu-id="71eb6-199">Although it is bad style for `ToString` to have side effects, the example demonstrates that no boxing occurred for the three invocations of `x.ToString()`.</span></span>

<span data-ttu-id="71eb6-200">Podobně zabalení nikdy neproběhne implicitně při přístupu ke členu v parametru omezeného typu.</span><span class="sxs-lookup"><span data-stu-id="71eb6-200">Similarly, boxing never implicitly occurs when accessing a member on a constrained type parameter.</span></span> <span data-ttu-id="71eb6-201">Předpokládejme například, že rozhraní `ICounter` obsahuje metodu `Increment`, kterou lze použít k úpravě hodnoty.</span><span class="sxs-lookup"><span data-stu-id="71eb6-201">For example, suppose an interface `ICounter` contains a method `Increment` which can be used to modify a value.</span></span> <span data-ttu-id="71eb6-202">Pokud je `ICounter` použito jako omezení, implementace metody `Increment` je volána s odkazem na proměnnou, kterou `Increment` byl volán na, nikdy do zabalené kopie.</span><span class="sxs-lookup"><span data-stu-id="71eb6-202">If `ICounter` is used as a constraint, the implementation of the `Increment` method is called with a reference to the variable that `Increment` was called on, never a boxed copy.</span></span>

```csharp
using System;

interface ICounter
{
    void Increment();
}

struct Counter: ICounter
{
    int value;

    public override string ToString() {
        return value.ToString();
    }

    void ICounter.Increment() {
        value++;
    }
}

class Program
{
    static void Test<T>() where T: ICounter, new() {
        T x = new T();
        Console.WriteLine(x);
        x.Increment();                    // Modify x
        Console.WriteLine(x);
        ((ICounter)x).Increment();        // Modify boxed copy of x
        Console.WriteLine(x);
    }

    static void Main() {
        Test<Counter>();
    }
}
```

<span data-ttu-id="71eb6-203">První volání `Increment` upravuje hodnotu v `x`proměnných.</span><span class="sxs-lookup"><span data-stu-id="71eb6-203">The first call to `Increment` modifies the value in the variable `x`.</span></span> <span data-ttu-id="71eb6-204">To není ekvivalentní druhému volání `Increment`, které upraví hodnotu v zabalené kopii `x`.</span><span class="sxs-lookup"><span data-stu-id="71eb6-204">This is not equivalent to the second call to `Increment`, which modifies the value in a boxed copy of `x`.</span></span> <span data-ttu-id="71eb6-205">Proto výstup programu je:</span><span class="sxs-lookup"><span data-stu-id="71eb6-205">Thus, the output of the program is:</span></span>
```console
0
1
1
```

<span data-ttu-id="71eb6-206">Další podrobnosti o zabalení a rozbalení naleznete v tématu [zabalení a rozbalení](types.md#boxing-and-unboxing).</span><span class="sxs-lookup"><span data-stu-id="71eb6-206">For further details on boxing and unboxing, see [Boxing and unboxing](types.md#boxing-and-unboxing).</span></span>

### <a name="meaning-of-this"></a><span data-ttu-id="71eb6-207">Význam tohoto</span><span class="sxs-lookup"><span data-stu-id="71eb6-207">Meaning of this</span></span>

<span data-ttu-id="71eb6-208">V rámci konstruktoru instance nebo členu funkce instance třídy je `this` klasifikován jako hodnota.</span><span class="sxs-lookup"><span data-stu-id="71eb6-208">Within an instance constructor or instance function member of a class, `this` is classified as a value.</span></span> <span data-ttu-id="71eb6-209">Takže zatímco `this` lze použít k odkazování na instanci, pro kterou byl člen funkce vyvolán, není možné přiřadit k `this` v členu funkce třídy.</span><span class="sxs-lookup"><span data-stu-id="71eb6-209">Thus, while `this` can be used to refer to the instance for which the function member was invoked, it is not possible to assign to `this` in a function member of a class.</span></span>

<span data-ttu-id="71eb6-210">V rámci konstruktoru instance struktury, `this` odpovídá parametru `out` typu struktury a v rámci členu funkce instance struktury, `this` odpovídá parametru `ref` typu struktury.</span><span class="sxs-lookup"><span data-stu-id="71eb6-210">Within an instance constructor of a struct, `this` corresponds to an `out` parameter of the struct type, and within an instance function member of a struct, `this` corresponds to a `ref` parameter of the struct type.</span></span> <span data-ttu-id="71eb6-211">V obou případech je `this` klasifikován jako proměnná a je možné upravit celou strukturu, pro kterou byl člen funkce vyvolán přiřazením `this` nebo předáním tohoto jako `ref` nebo `out` parametr.</span><span class="sxs-lookup"><span data-stu-id="71eb6-211">In both cases, `this` is classified as a variable, and it is possible to modify the entire struct for which the function member was invoked by assigning to `this` or by passing this as a `ref` or `out` parameter.</span></span>

### <a name="field-initializers"></a><span data-ttu-id="71eb6-212">Inicializátory polí</span><span class="sxs-lookup"><span data-stu-id="71eb6-212">Field initializers</span></span>

<span data-ttu-id="71eb6-213">Jak je popsáno ve [výchozích hodnotách](structs.md#default-values), výchozí hodnota struktury sestává z hodnoty, která je výsledkem nastavení všech polí typu hodnoty na výchozí hodnotu a všech polí typu odkazu na `null`.</span><span class="sxs-lookup"><span data-stu-id="71eb6-213">As described in [Default values](structs.md#default-values), the default value of a struct consists of the value that results from setting all value type fields to their default value and all reference type fields to `null`.</span></span> <span data-ttu-id="71eb6-214">Z tohoto důvodu struktura nepovoluje deklarace pole instance pro zahrnutí inicializátorů proměnných.</span><span class="sxs-lookup"><span data-stu-id="71eb6-214">For this reason, a struct does not permit instance field declarations to include variable initializers.</span></span> <span data-ttu-id="71eb6-215">Toto omezení platí pouze pro pole instance.</span><span class="sxs-lookup"><span data-stu-id="71eb6-215">This restriction applies only to instance fields.</span></span> <span data-ttu-id="71eb6-216">Statická pole struktury mají povolený zahrnutí inicializátorů proměnných.</span><span class="sxs-lookup"><span data-stu-id="71eb6-216">Static fields of a struct are permitted to include variable initializers.</span></span>

<span data-ttu-id="71eb6-217">Příklad</span><span class="sxs-lookup"><span data-stu-id="71eb6-217">The example</span></span>
```csharp
struct Point
{
    public int x = 1;  // Error, initializer not permitted
    public int y = 1;  // Error, initializer not permitted
}
```
<span data-ttu-id="71eb6-218">došlo k chybě, protože deklarace pole instance obsahují proměnné inicializátorů.</span><span class="sxs-lookup"><span data-stu-id="71eb6-218">is in error because the instance field declarations include variable initializers.</span></span>

### <a name="constructors"></a><span data-ttu-id="71eb6-219">Konstruktory</span><span class="sxs-lookup"><span data-stu-id="71eb6-219">Constructors</span></span>

<span data-ttu-id="71eb6-220">Na rozdíl od třídy struktura není povolena k deklaraci konstruktoru instance bez parametrů.</span><span class="sxs-lookup"><span data-stu-id="71eb6-220">Unlike a class, a struct is not permitted to declare a parameterless instance constructor.</span></span> <span data-ttu-id="71eb6-221">Místo toho má každá struktura implicitně konstruktor instance bez parametrů, který vždycky vrací hodnotu, která je výsledkem nastavení všech polí typu hodnoty na výchozí hodnotu a všechna pole odkazového typu na hodnotu null ([výchozí konstruktory](types.md#default-constructors)).</span><span class="sxs-lookup"><span data-stu-id="71eb6-221">Instead, every struct implicitly has a parameterless instance constructor which always returns the value that results from setting all value type fields to their default value and all reference type fields to null ([Default constructors](types.md#default-constructors)).</span></span> <span data-ttu-id="71eb6-222">Struktura může deklarovat konstruktory instancí s parametry.</span><span class="sxs-lookup"><span data-stu-id="71eb6-222">A struct can declare instance constructors having parameters.</span></span> <span data-ttu-id="71eb6-223">Například</span><span class="sxs-lookup"><span data-stu-id="71eb6-223">For example</span></span>
```csharp
struct Point
{
    int x, y;

    public Point(int x, int y) {
        this.x = x;
        this.y = y;
    }
}
```

<span data-ttu-id="71eb6-224">S ohledem na výše uvedenou deklaraci jsou příkazy</span><span class="sxs-lookup"><span data-stu-id="71eb6-224">Given the above declaration, the statements</span></span>
```csharp
Point p1 = new Point();
Point p2 = new Point(0, 0);
```
<span data-ttu-id="71eb6-225">Vytvoří `Point` s `x` a `y` inicializovaný jako nula.</span><span class="sxs-lookup"><span data-stu-id="71eb6-225">both create a `Point` with `x` and `y` initialized to zero.</span></span>

<span data-ttu-id="71eb6-226">Konstruktor instance struktury není povolený pro zahrnutí inicializátoru konstruktoru formuláře `base(...)`.</span><span class="sxs-lookup"><span data-stu-id="71eb6-226">A struct instance constructor is not permitted to include a constructor initializer of the form `base(...)`.</span></span>

<span data-ttu-id="71eb6-227">Pokud konstruktor instance struktury neurčuje inicializátor konstruktoru, `this` proměnná odpovídá parametru `out` typu struktury a podobně jako parametr `out`, `this` musí být jednoznačně přiřazen ([jednoznačné přiřazení](variables.md#definite-assignment)) na každém místě, kde se konstruktor vrátí.</span><span class="sxs-lookup"><span data-stu-id="71eb6-227">If the struct instance constructor doesn't specify a constructor initializer, the `this` variable corresponds to an `out` parameter of the struct type, and similar to an `out` parameter, `this` must be definitely assigned ([Definite assignment](variables.md#definite-assignment)) at every location where the constructor returns.</span></span> <span data-ttu-id="71eb6-228">Pokud konstruktor instance struktury určuje inicializátor konstruktoru, proměnná `this` odpovídá parametru `ref` typu struktury a podobá se `ref` parametru, `this` je považována za jednoznačně přiřazenou pro vstup do těla konstruktoru.</span><span class="sxs-lookup"><span data-stu-id="71eb6-228">If the struct instance constructor specifies a constructor initializer, the `this` variable corresponds to a `ref` parameter of the struct type, and similar to a `ref` parameter, `this` is considered definitely assigned on entry to the constructor body.</span></span> <span data-ttu-id="71eb6-229">Zvažte následující implementaci konstruktoru instance:</span><span class="sxs-lookup"><span data-stu-id="71eb6-229">Consider the instance constructor implementation below:</span></span>
```csharp
struct Point
{
    int x, y;

    public int X {
        set { x = value; }
    }

    public int Y {
        set { y = value; }
    }

    public Point(int x, int y) {
        X = x;        // error, this is not yet definitely assigned
        Y = y;        // error, this is not yet definitely assigned
    }
}
```

<span data-ttu-id="71eb6-230">Není možné volat žádnou členskou funkci instance (včetně přístupových objektů set pro vlastnosti `X` a `Y`), dokud nebudou přiřazena všechna pole strukturované struktury.</span><span class="sxs-lookup"><span data-stu-id="71eb6-230">No instance member function (including the set accessors for the properties `X` and `Y`) can be called until all fields of the struct being constructed have been definitely assigned.</span></span> <span data-ttu-id="71eb6-231">Jediná výjimka zahrnuje automaticky implementované vlastnosti ([automaticky implementované vlastnosti](classes.md#automatically-implemented-properties)).</span><span class="sxs-lookup"><span data-stu-id="71eb6-231">The only exception involves automatically implemented properties ([Automatically implemented properties](classes.md#automatically-implemented-properties)).</span></span> <span data-ttu-id="71eb6-232">Neomezená pravidla přiřazení ([jednoduché výrazy přiřazení](variables.md#simple-assignment-expressions)) specificky vyloučí přiřazení k automatické vlastnosti typu struktury v rámci konstruktoru instance daného typu struktury. takové přiřazení je považováno za jednoznačné přiřazení skrytého pole zálohování automatické vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="71eb6-232">The definite assignment rules ([Simple assignment expressions](variables.md#simple-assignment-expressions)) specifically exempt assignment to an auto-property of a struct type within an instance constructor of that struct type: such an assignment is considered a definite assignment of the hidden backing field of the auto-property.</span></span> <span data-ttu-id="71eb6-233">Proto jsou povoleny následující:</span><span class="sxs-lookup"><span data-stu-id="71eb6-233">Thus, the following is allowed:</span></span>

```csharp
struct Point
{
    public int X { get; set; }
    public int Y { get; set; }

    public Point(int x, int y) {
        X = x;      // allowed, definitely assigns backing field
        Y = y;      // allowed, definitely assigns backing field
    }
```

### <a name="destructors"></a><span data-ttu-id="71eb6-234">Destruktory</span><span class="sxs-lookup"><span data-stu-id="71eb6-234">Destructors</span></span>

<span data-ttu-id="71eb6-235">Struktura není oprávněná deklarovat destruktor.</span><span class="sxs-lookup"><span data-stu-id="71eb6-235">A struct is not permitted to declare a destructor.</span></span>

### <a name="static-constructors"></a><span data-ttu-id="71eb6-236">Statické konstruktory</span><span class="sxs-lookup"><span data-stu-id="71eb6-236">Static constructors</span></span>

<span data-ttu-id="71eb6-237">Statické konstruktory pro struktury se řídí většinou stejných pravidel jako u tříd.</span><span class="sxs-lookup"><span data-stu-id="71eb6-237">Static constructors for structs follow most of the same rules as for classes.</span></span> <span data-ttu-id="71eb6-238">Provedení statického konstruktoru pro typ struktury je aktivováno prvním z následujících událostí, které se mají provést v rámci domény aplikace:</span><span class="sxs-lookup"><span data-stu-id="71eb6-238">The execution of a static constructor for a struct type is triggered by the first of the following events to occur within an application domain:</span></span>

*  <span data-ttu-id="71eb6-239">Odkazuje se na statický člen typu struktury.</span><span class="sxs-lookup"><span data-stu-id="71eb6-239">A static member of the struct type is referenced.</span></span>
*  <span data-ttu-id="71eb6-240">Je volán explicitně deklarovaný konstruktor typu struktury.</span><span class="sxs-lookup"><span data-stu-id="71eb6-240">An explicitly declared constructor of the struct type is called.</span></span>

<span data-ttu-id="71eb6-241">Vytváření výchozích hodnot ([výchozích hodnot](structs.md#default-values)) typů struktury neaktivuje statický konstruktor.</span><span class="sxs-lookup"><span data-stu-id="71eb6-241">The creation of default values ([Default values](structs.md#default-values)) of struct types does not trigger the static constructor.</span></span> <span data-ttu-id="71eb6-242">(Příkladem je počáteční hodnota prvků v poli.)</span><span class="sxs-lookup"><span data-stu-id="71eb6-242">(An example of this is the initial value of elements in an array.)</span></span>

## <a name="struct-examples"></a><span data-ttu-id="71eb6-243">Příklady struktury</span><span class="sxs-lookup"><span data-stu-id="71eb6-243">Struct examples</span></span>

<span data-ttu-id="71eb6-244">Následující příklad ukazuje dva významné příklady použití typů `struct` k vytváření typů, které se dají použít podobně jako předdefinované typy jazyka, ale se změněnou sémantikou.</span><span class="sxs-lookup"><span data-stu-id="71eb6-244">The following shows two significant examples of using `struct` types to create types that can be used similarly to the predefined types of the language, but with modified semantics.</span></span>

### <a name="database-integer-type"></a><span data-ttu-id="71eb6-245">Celočíselný typ databáze</span><span class="sxs-lookup"><span data-stu-id="71eb6-245">Database integer type</span></span>

<span data-ttu-id="71eb6-246">`DBInt` struktura níže implementuje celočíselný typ, který může představovat úplnou sadu hodnot `int` typu a další stav, který označuje neznámou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="71eb6-246">The `DBInt` struct below implements an integer type that can represent the complete set of values of the `int` type, plus an additional state that indicates an unknown value.</span></span> <span data-ttu-id="71eb6-247">Typ s těmito charakteristikami se běžně používá v databázích.</span><span class="sxs-lookup"><span data-stu-id="71eb6-247">A type with these characteristics is commonly used in databases.</span></span>

```csharp
using System;

public struct DBInt
{
    // The Null member represents an unknown DBInt value.

    public static readonly DBInt Null = new DBInt();

    // When the defined field is true, this DBInt represents a known value
    // which is stored in the value field. When the defined field is false,
    // this DBInt represents an unknown value, and the value field is 0.

    int value;
    bool defined;

    // Private instance constructor. Creates a DBInt with a known value.

    DBInt(int value) {
        this.value = value;
        this.defined = true;
    }

    // The IsNull property is true if this DBInt represents an unknown value.

    public bool IsNull { get { return !defined; } }

    // The Value property is the known value of this DBInt, or 0 if this
    // DBInt represents an unknown value.

    public int Value { get { return value; } }

    // Implicit conversion from int to DBInt.

    public static implicit operator DBInt(int x) {
        return new DBInt(x);
    }

    // Explicit conversion from DBInt to int. Throws an exception if the
    // given DBInt represents an unknown value.

    public static explicit operator int(DBInt x) {
        if (!x.defined) throw new InvalidOperationException();
        return x.value;
    }

    public static DBInt operator +(DBInt x) {
        return x;
    }

    public static DBInt operator -(DBInt x) {
        return x.defined ? -x.value : Null;
    }

    public static DBInt operator +(DBInt x, DBInt y) {
        return x.defined && y.defined? x.value + y.value: Null;
    }

    public static DBInt operator -(DBInt x, DBInt y) {
        return x.defined && y.defined? x.value - y.value: Null;
    }

    public static DBInt operator *(DBInt x, DBInt y) {
        return x.defined && y.defined? x.value * y.value: Null;
    }

    public static DBInt operator /(DBInt x, DBInt y) {
        return x.defined && y.defined? x.value / y.value: Null;
    }

    public static DBInt operator %(DBInt x, DBInt y) {
        return x.defined && y.defined? x.value % y.value: Null;
    }

    public static DBBool operator ==(DBInt x, DBInt y) {
        return x.defined && y.defined? x.value == y.value: DBBool.Null;
    }

    public static DBBool operator !=(DBInt x, DBInt y) {
        return x.defined && y.defined? x.value != y.value: DBBool.Null;
    }

    public static DBBool operator >(DBInt x, DBInt y) {
        return x.defined && y.defined? x.value > y.value: DBBool.Null;
    }

    public static DBBool operator <(DBInt x, DBInt y) {
        return x.defined && y.defined? x.value < y.value: DBBool.Null;
    }

    public static DBBool operator >=(DBInt x, DBInt y) {
        return x.defined && y.defined? x.value >= y.value: DBBool.Null;
    }

    public static DBBool operator <=(DBInt x, DBInt y) {
        return x.defined && y.defined? x.value <= y.value: DBBool.Null;
    }

    public override bool Equals(object obj) {
        if (!(obj is DBInt)) return false;
        DBInt x = (DBInt)obj;
        return value == x.value && defined == x.defined;
    }

    public override int GetHashCode() {
        return value;
    }

    public override string ToString() {
        return defined? value.ToString(): "DBInt.Null";
    }
}
```

### <a name="database-boolean-type"></a><span data-ttu-id="71eb6-248">Typ databáze typu Boolean</span><span class="sxs-lookup"><span data-stu-id="71eb6-248">Database boolean type</span></span>

<span data-ttu-id="71eb6-249">`DBBool` struktura níže implementuje logický typ se třemi hodnotami.</span><span class="sxs-lookup"><span data-stu-id="71eb6-249">The `DBBool` struct below implements a three-valued logical type.</span></span> <span data-ttu-id="71eb6-250">Možné hodnoty tohoto typu jsou `DBBool.True`, `DBBool.False`a `DBBool.Null`, kde `Null` člen označuje neznámou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="71eb6-250">The possible values of this type are `DBBool.True`, `DBBool.False`, and `DBBool.Null`, where the `Null` member indicates an unknown value.</span></span> <span data-ttu-id="71eb6-251">Tyto tři logické typy jsou běžně používány v databázích.</span><span class="sxs-lookup"><span data-stu-id="71eb6-251">Such three-valued logical types are commonly used in databases.</span></span>

```csharp
using System;

public struct DBBool
{
    // The three possible DBBool values.

    public static readonly DBBool Null = new DBBool(0);
    public static readonly DBBool False = new DBBool(-1);
    public static readonly DBBool True = new DBBool(1);

    // Private field that stores -1, 0, 1 for False, Null, True.

    sbyte value;

    // Private instance constructor. The value parameter must be -1, 0, or 1.

    DBBool(int value) {
        this.value = (sbyte)value;
    }

    // Properties to examine the value of a DBBool. Return true if this
    // DBBool has the given value, false otherwise.

    public bool IsNull { get { return value == 0; } }

    public bool IsFalse { get { return value < 0; } }

    public bool IsTrue { get { return value > 0; } }

    // Implicit conversion from bool to DBBool. Maps true to DBBool.True and
    // false to DBBool.False.

    public static implicit operator DBBool(bool x) {
        return x? True: False;
    }

    // Explicit conversion from DBBool to bool. Throws an exception if the
    // given DBBool is Null, otherwise returns true or false.

    public static explicit operator bool(DBBool x) {
        if (x.value == 0) throw new InvalidOperationException();
        return x.value > 0;
    }

    // Equality operator. Returns Null if either operand is Null, otherwise
    // returns True or False.

    public static DBBool operator ==(DBBool x, DBBool y) {
        if (x.value == 0 || y.value == 0) return Null;
        return x.value == y.value? True: False;
    }

    // Inequality operator. Returns Null if either operand is Null, otherwise
    // returns True or False.

    public static DBBool operator !=(DBBool x, DBBool y) {
        if (x.value == 0 || y.value == 0) return Null;
        return x.value != y.value? True: False;
    }

    // Logical negation operator. Returns True if the operand is False, Null
    // if the operand is Null, or False if the operand is True.

    public static DBBool operator !(DBBool x) {
        return new DBBool(-x.value);
    }

    // Logical AND operator. Returns False if either operand is False,
    // otherwise Null if either operand is Null, otherwise True.

    public static DBBool operator &(DBBool x, DBBool y) {
        return new DBBool(x.value < y.value? x.value: y.value);
    }

    // Logical OR operator. Returns True if either operand is True, otherwise
    // Null if either operand is Null, otherwise False.

    public static DBBool operator |(DBBool x, DBBool y) {
        return new DBBool(x.value > y.value? x.value: y.value);
    }

    // Definitely true operator. Returns true if the operand is True, false
    // otherwise.

    public static bool operator true(DBBool x) {
        return x.value > 0;
    }

    // Definitely false operator. Returns true if the operand is False, false
    // otherwise.

    public static bool operator false(DBBool x) {
        return x.value < 0;
    }

    public override bool Equals(object obj) {
        if (!(obj is DBBool)) return false;
        return value == ((DBBool)obj).value;
    }

    public override int GetHashCode() {
        return value;
    }

    public override string ToString() {
        if (value > 0) return "DBBool.True";
        if (value < 0) return "DBBool.False";
        return "DBBool.Null";
    }
}
```
