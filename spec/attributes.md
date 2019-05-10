---
ms.openlocfilehash: a8ad8a8b3eda1d00fa745bd92e4371eacc36b79f
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/25/2019
ms.locfileid: "64488829"
---
# <a name="attributes"></a><span data-ttu-id="1fddf-101">Atributy</span><span class="sxs-lookup"><span data-stu-id="1fddf-101">Attributes</span></span>

<span data-ttu-id="1fddf-102">Velká část jazyka C# umožňuje programátorovi, aby zadejte deklarativní informace o entitách, které jsou definovány v programu.</span><span class="sxs-lookup"><span data-stu-id="1fddf-102">Much of the C# language enables the programmer to specify declarative information about the entities defined in the program.</span></span> <span data-ttu-id="1fddf-103">Například je určená pro usnadnění metody ve třídě upravení s *method_modifier*s `public`, `protected`, `internal`, a `private`.</span><span class="sxs-lookup"><span data-stu-id="1fddf-103">For example, the accessibility of a method in a class is specified by decorating it with the *method_modifier*s `public`, `protected`, `internal`, and `private`.</span></span>

<span data-ttu-id="1fddf-104">C# umožňuje programátorům vytvářet nové typy deklarativní informací nazývaných ***atributy***.</span><span class="sxs-lookup"><span data-stu-id="1fddf-104">C# enables programmers to invent new kinds of declarative information, called ***attributes***.</span></span> <span data-ttu-id="1fddf-105">Programátoři pak můžete připojit atributy na různé entity programu a načíst informace o atributu v prostředí za běhu.</span><span class="sxs-lookup"><span data-stu-id="1fddf-105">Programmers can then attach attributes to various program entities, and retrieve attribute information in a run-time environment.</span></span> <span data-ttu-id="1fddf-106">Například může definovat rozhraní `HelpAttribute` atribut, který může být umístěn v určitých prvků programu (například třídy a metody) k poskytování mapování z těchto prvků programu do jejich dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="1fddf-106">For instance, a framework might define a `HelpAttribute` attribute that can be placed on certain program elements (such as classes and methods) to provide a mapping from those program elements to their documentation.</span></span>

<span data-ttu-id="1fddf-107">Atributy jsou definované pomocí deklarace třídy atributů ([atribut třídy](attributes.md#attribute-classes)), který může mít poziční a pojmenované parametry ([Positional a pojmenované parametry](attributes.md#positional-and-named-parameters)).</span><span class="sxs-lookup"><span data-stu-id="1fddf-107">Attributes are defined through the declaration of attribute classes ([Attribute classes](attributes.md#attribute-classes)), which may have positional and named parameters ([Positional and named parameters](attributes.md#positional-and-named-parameters)).</span></span> <span data-ttu-id="1fddf-108">Atributy jsou připojeny k entitám v programu v C# pomocí atributů specifikací ([specifikace atributu](attributes.md#attribute-specification)) a mohou být načteny při spuštění jako instance atributu ([instance atributu do mezipaměti](attributes.md#attribute-instances)).</span><span class="sxs-lookup"><span data-stu-id="1fddf-108">Attributes are attached to entities in a C# program using attribute specifications ([Attribute specification](attributes.md#attribute-specification)), and can be retrieved at run-time as attribute instances ([Attribute instances](attributes.md#attribute-instances)).</span></span>

## <a name="attribute-classes"></a><span data-ttu-id="1fddf-109">Třídy atributů</span><span class="sxs-lookup"><span data-stu-id="1fddf-109">Attribute classes</span></span>

<span data-ttu-id="1fddf-110">Třída, která dědí z abstraktní třídy `System.Attribute`, ať už přímo či nepřímo, je ***třídy atributů***.</span><span class="sxs-lookup"><span data-stu-id="1fddf-110">A class that derives from the abstract class `System.Attribute`, whether directly or indirectly, is an ***attribute class***.</span></span> <span data-ttu-id="1fddf-111">Deklarace třídy atributu definuje nový druh ***atribut*** , který je možné použít v deklaraci.</span><span class="sxs-lookup"><span data-stu-id="1fddf-111">The declaration of an attribute class defines a new kind of ***attribute*** that can be placed on a declaration.</span></span> <span data-ttu-id="1fddf-112">Podle konvence jsou pojmenovány třídy atributů s příponu `Attribute`.</span><span class="sxs-lookup"><span data-stu-id="1fddf-112">By convention, attribute classes are named with a suffix of `Attribute`.</span></span> <span data-ttu-id="1fddf-113">Použití atributu mohou zahrnout nebo vynechejte tuto příponu.</span><span class="sxs-lookup"><span data-stu-id="1fddf-113">Uses of an attribute may either include or omit this suffix.</span></span>

### <a name="attribute-usage"></a><span data-ttu-id="1fddf-114">Použití atributu</span><span class="sxs-lookup"><span data-stu-id="1fddf-114">Attribute usage</span></span>

<span data-ttu-id="1fddf-115">Atribut `AttributeUsage` ([atribut The AttributeUsage](attributes.md#the-attributeusage-attribute)) se používá k popisu použití třídy atributu.</span><span class="sxs-lookup"><span data-stu-id="1fddf-115">The attribute `AttributeUsage` ([The AttributeUsage attribute](attributes.md#the-attributeusage-attribute)) is used to describe how an attribute class can be used.</span></span>

<span data-ttu-id="1fddf-116">`AttributeUsage` má poziční parametr ([Positional a pojmenované parametry](attributes.md#positional-and-named-parameters)), která umožňuje třídu atributu k určení typů deklarací, na kterých je možné.</span><span class="sxs-lookup"><span data-stu-id="1fddf-116">`AttributeUsage` has a positional parameter ([Positional and named parameters](attributes.md#positional-and-named-parameters)) that enables an attribute class to specify the kinds of declarations on which it can be used.</span></span> <span data-ttu-id="1fddf-117">V příkladu</span><span class="sxs-lookup"><span data-stu-id="1fddf-117">The example</span></span>

```csharp
using System;

[AttributeUsage(AttributeTargets.Class | AttributeTargets.Interface)]
public class SimpleAttribute: Attribute 
{
    ...
}
```

<span data-ttu-id="1fddf-118">definuje třídu atributu s názvem `SimpleAttribute` , který můžete použít u *class_declaration*s a *interface_declaration*pouze s.</span><span class="sxs-lookup"><span data-stu-id="1fddf-118">defines an attribute class named `SimpleAttribute` that can be placed on *class_declaration*s and *interface_declaration*s only.</span></span> <span data-ttu-id="1fddf-119">V příkladu</span><span class="sxs-lookup"><span data-stu-id="1fddf-119">The example</span></span>

```csharp
[Simple] class Class1 {...}

[Simple] interface Interface1 {...}
```

<span data-ttu-id="1fddf-120">ukazuje použití několika `Simple` atribut.</span><span class="sxs-lookup"><span data-stu-id="1fddf-120">shows several uses of the `Simple` attribute.</span></span> <span data-ttu-id="1fddf-121">I když tento atribut je definován s názvem `SimpleAttribute`, když tento atribut se používá, `Attribute` přípona může být vynechán, výsledkem je krátký název `Simple`.</span><span class="sxs-lookup"><span data-stu-id="1fddf-121">Although this attribute is defined with the name `SimpleAttribute`, when this attribute is used, the `Attribute` suffix may be omitted, resulting in the short name `Simple`.</span></span> <span data-ttu-id="1fddf-122">Výše uvedený příklad je proto sémanticky ekvivalentní následujícímu zápisu:</span><span class="sxs-lookup"><span data-stu-id="1fddf-122">Thus, the example above is semantically equivalent to the following:</span></span>

```csharp
[SimpleAttribute] class Class1 {...}

[SimpleAttribute] interface Interface1 {...}
```

<span data-ttu-id="1fddf-123">`AttributeUsage` má pojmenovaným parametrem ([Positional a pojmenované parametry](attributes.md#positional-and-named-parameters)) volá `AllowMultiple`, která udává, zda atribut lze zadat více než jednou pro danou entitu.</span><span class="sxs-lookup"><span data-stu-id="1fddf-123">`AttributeUsage` has a named parameter ([Positional and named parameters](attributes.md#positional-and-named-parameters)) called `AllowMultiple`, which indicates whether the attribute can be specified more than once for a given entity.</span></span> <span data-ttu-id="1fddf-124">Pokud `AllowMultiple` atribut třídy má hodnotu true, pak je danou třídu atributů ***více použít atribut třídy***a je možné zadat více než jednou entitou.</span><span class="sxs-lookup"><span data-stu-id="1fddf-124">If `AllowMultiple` for an attribute class is true, then that attribute class is a ***multi-use attribute class***, and can be specified more than once on an entity.</span></span> <span data-ttu-id="1fddf-125">Pokud `AllowMultiple` atributu třída má hodnotu false nebo neurčená a potom je danou třídu atributů ***jedno použití třídy atributů***a je možné zadat maximálně jednou entitou.</span><span class="sxs-lookup"><span data-stu-id="1fddf-125">If `AllowMultiple` for an attribute class is false or it is unspecified, then that attribute class is a ***single-use attribute class***, and can be specified at most once on an entity.</span></span>

<span data-ttu-id="1fddf-126">V příkladu</span><span class="sxs-lookup"><span data-stu-id="1fddf-126">The example</span></span>

```csharp
using System;

[AttributeUsage(AttributeTargets.Class, AllowMultiple = true)]
public class AuthorAttribute: Attribute
{
    private string name;

    public AuthorAttribute(string name) {
        this.name = name;
    }

    public string Name {
        get { return name; }
    }
}
```

<span data-ttu-id="1fddf-127">definuje třídu více použít atribut s názvem `AuthorAttribute`.</span><span class="sxs-lookup"><span data-stu-id="1fddf-127">defines a multi-use attribute class named `AuthorAttribute`.</span></span> <span data-ttu-id="1fddf-128">V příkladu</span><span class="sxs-lookup"><span data-stu-id="1fddf-128">The example</span></span>

```csharp
[Author("Brian Kernighan"), Author("Dennis Ritchie")] 
class Class1
{
    ...
}
```

<span data-ttu-id="1fddf-129">ukazuje deklaraci třídy s dvěma způsoby použití `Author` atribut.</span><span class="sxs-lookup"><span data-stu-id="1fddf-129">shows a class declaration with two uses of the `Author` attribute.</span></span>

<span data-ttu-id="1fddf-130">`AttributeUsage` má jiné pojmenovaný parametr s názvem `Inherited`, což znamená, zda atribut, pokud zadaný na základní třídu, je také děděné třídy, které jsou odvozeny z této základní třídy.</span><span class="sxs-lookup"><span data-stu-id="1fddf-130">`AttributeUsage` has another named parameter called `Inherited`, which indicates whether the attribute, when specified on a base class, is also inherited by classes that derive from that base class.</span></span> <span data-ttu-id="1fddf-131">Pokud `Inherited` atribut třídy má hodnotu true, pak tento atribut pochází.</span><span class="sxs-lookup"><span data-stu-id="1fddf-131">If `Inherited` for an attribute class is true, then that attribute is inherited.</span></span> <span data-ttu-id="1fddf-132">Pokud `Inherited` atributu třída má hodnotu false, pak tento atribut není zděděno.</span><span class="sxs-lookup"><span data-stu-id="1fddf-132">If `Inherited` for an attribute class is false then that attribute is not inherited.</span></span> <span data-ttu-id="1fddf-133">Pokud neurčená, jeho výchozí hodnota je true.</span><span class="sxs-lookup"><span data-stu-id="1fddf-133">If it is unspecified, its default value is true.</span></span>

<span data-ttu-id="1fddf-134">Třídu atributu `X` nemají `AttributeUsage` atribut připojena k němu, jako v</span><span class="sxs-lookup"><span data-stu-id="1fddf-134">An attribute class `X` not having an `AttributeUsage` attribute attached to it, as in</span></span>

```csharp
using System;

class X: Attribute {...}
```

<span data-ttu-id="1fddf-135">Je ekvivalentní následujícímu zápisu:</span><span class="sxs-lookup"><span data-stu-id="1fddf-135">is equivalent to the following:</span></span>

```csharp
using System;

[AttributeUsage(
    AttributeTargets.All,
    AllowMultiple = false,
    Inherited = true)
]
class X: Attribute {...}
```

### <a name="positional-and-named-parameters"></a><span data-ttu-id="1fddf-136">Pojmenované a poziční parametry</span><span class="sxs-lookup"><span data-stu-id="1fddf-136">Positional and named parameters</span></span>

<span data-ttu-id="1fddf-137">Atribut třídy mohou mít ***poziční parametry*** a ***pojmenované parametry***.</span><span class="sxs-lookup"><span data-stu-id="1fddf-137">Attribute classes can have ***positional parameters*** and ***named parameters***.</span></span> <span data-ttu-id="1fddf-138">Každý veřejný konstruktor instance pro třídu atributu definuje platné posloupnost poziční parametry pro danou třídu atributů.</span><span class="sxs-lookup"><span data-stu-id="1fddf-138">Each public instance constructor for an attribute class defines a valid sequence of positional parameters for that attribute class.</span></span> <span data-ttu-id="1fddf-139">Každé nestatické pole veřejné čtení a zápis a vlastnosti pro třídu atributu definuje pojmenovaný parametr pro třídu atributu.</span><span class="sxs-lookup"><span data-stu-id="1fddf-139">Each non-static public read-write field and property for an attribute class defines a named parameter for the attribute class.</span></span>

<span data-ttu-id="1fddf-140">V příkladu</span><span class="sxs-lookup"><span data-stu-id="1fddf-140">The example</span></span>

```csharp
using System;

[AttributeUsage(AttributeTargets.Class)]
public class HelpAttribute: Attribute
{
    public HelpAttribute(string url) {        // Positional parameter
        ...
    }

    public string Topic {                     // Named parameter
        get {...}
        set {...}
    }

    public string Url {
        get {...}
    }
}
```

<span data-ttu-id="1fddf-141">definuje třídu atributu s názvem `HelpAttribute` , který má jeden pozičních parametrů více dopředu, `url`a jednu s názvem parametru `Topic`.</span><span class="sxs-lookup"><span data-stu-id="1fddf-141">defines an attribute class named `HelpAttribute` that has one positional parameter, `url`, and one named parameter, `Topic`.</span></span> <span data-ttu-id="1fddf-142">Přestože jde o nestatická a public, vlastnost `Url` nedefinuje parametr s názvem, protože se nejedná o čtení a zápis.</span><span class="sxs-lookup"><span data-stu-id="1fddf-142">Although it is non-static and public, the property `Url` does not define a named parameter, since it is not read-write.</span></span>

<span data-ttu-id="1fddf-143">Tato třída atributu může být použit takto:</span><span class="sxs-lookup"><span data-stu-id="1fddf-143">This attribute class might be used as follows:</span></span>

```csharp
[Help("http://www.mycompany.com/.../Class1.htm")]
class Class1
{
    ...
}

[Help("http://www.mycompany.com/.../Misc.htm", Topic = "Class2")]
class Class2
{
    ...
}
```

### <a name="attribute-parameter-types"></a><span data-ttu-id="1fddf-144">Typy parametrů atributů</span><span class="sxs-lookup"><span data-stu-id="1fddf-144">Attribute parameter types</span></span>

<span data-ttu-id="1fddf-145">Typy poziční a pojmenované parametry atributu třídy jsou omezené na ***typy parametrů atributů***, které jsou:</span><span class="sxs-lookup"><span data-stu-id="1fddf-145">The types of positional and named parameters for an attribute class are limited to the ***attribute parameter types***, which are:</span></span>

*  <span data-ttu-id="1fddf-146">Jedním z následujících typů: `bool`, `byte`, `char`, `double`, `float`, `int`, `long`, `sbyte`, `short`, `string`, `uint`, `ulong`, `ushort`.</span><span class="sxs-lookup"><span data-stu-id="1fddf-146">One of the following types: `bool`, `byte`, `char`, `double`, `float`, `int`, `long`, `sbyte`, `short`, `string`, `uint`, `ulong`, `ushort`.</span></span>
*  <span data-ttu-id="1fddf-147">Typ `object`.</span><span class="sxs-lookup"><span data-stu-id="1fddf-147">The type `object`.</span></span>
*  <span data-ttu-id="1fddf-148">Typ `System.Type`.</span><span class="sxs-lookup"><span data-stu-id="1fddf-148">The type `System.Type`.</span></span>
*  <span data-ttu-id="1fddf-149">Zadaný typ výčtu, nemá přístupnost public a typy, ve kterých je vnořené (pokud existuje) je navíc přístupnost public ([specifikace atributu](attributes.md#attribute-specification)).</span><span class="sxs-lookup"><span data-stu-id="1fddf-149">An enum type, provided it has public accessibility and the types in which it is nested (if any) also have public accessibility ([Attribute specification](attributes.md#attribute-specification)).</span></span>
*  <span data-ttu-id="1fddf-150">Jednorozměrná pole z výše uvedených typů.</span><span class="sxs-lookup"><span data-stu-id="1fddf-150">Single-dimensional arrays of the above types.</span></span>
*  <span data-ttu-id="1fddf-151">Argument konstruktoru nebo veřejné pole, která nemá jeden z těchto typů nelze použít jako parametr poziční nebo pojmenované ve specifikaci atributu.</span><span class="sxs-lookup"><span data-stu-id="1fddf-151">A constructor argument or public field which does not have one of these types, cannot be used as a positional or named parameter in an attribute specification.</span></span>

## <a name="attribute-specification"></a><span data-ttu-id="1fddf-152">Specifikace atributu</span><span class="sxs-lookup"><span data-stu-id="1fddf-152">Attribute specification</span></span>

<span data-ttu-id="1fddf-153">***Specifikace atributu*** je aplikace dříve definovaný atribut deklarace.</span><span class="sxs-lookup"><span data-stu-id="1fddf-153">***Attribute specification*** is the application of a previously defined attribute to a declaration.</span></span> <span data-ttu-id="1fddf-154">Atribut je deklarativní informace, které je určená pro deklaraci.</span><span class="sxs-lookup"><span data-stu-id="1fddf-154">An attribute is a piece of additional declarative information that is specified for a declaration.</span></span> <span data-ttu-id="1fddf-155">Atributy lze zadat v globálním oboru (k určení atributů na obsahující sestavení nebo modul) a pro *type_declaration*s ([typ deklarace](namespaces.md#type-declarations)), *class_member_declaration* s ([omezení parametru typu](classes.md#type-parameter-constraints)), *interface_member_declaration*s ([členy rozhraní](interfaces.md#interface-members)), *struct_member _declaration*s ([členy struktury](structs.md#struct-members)), *enum_member_declaration*s ([členy výčtu](enums.md#enum-members)), *accessor_declarations*  ([Přistupující objekty](classes.md#accessors)), *event_accessor_declarations* ([události podobné poli](classes.md#field-like-events)), a *formal_parameter_list*s ([parametry metody](classes.md#method-parameters)).</span><span class="sxs-lookup"><span data-stu-id="1fddf-155">Attributes can be specified at global scope (to specify attributes on the containing assembly or module) and for *type_declaration*s ([Type declarations](namespaces.md#type-declarations)), *class_member_declaration*s ([Type parameter constraints](classes.md#type-parameter-constraints)), *interface_member_declaration*s ([Interface members](interfaces.md#interface-members)), *struct_member_declaration*s ([Struct members](structs.md#struct-members)), *enum_member_declaration*s ([Enum members](enums.md#enum-members)), *accessor_declarations* ([Accessors](classes.md#accessors)), *event_accessor_declarations* ([Field-like events](classes.md#field-like-events)), and *formal_parameter_list*s ([Method parameters](classes.md#method-parameters)).</span></span>

<span data-ttu-id="1fddf-156">Atributy jsou určené v ***atribut oddíly***.</span><span class="sxs-lookup"><span data-stu-id="1fddf-156">Attributes are specified in ***attribute sections***.</span></span> <span data-ttu-id="1fddf-157">Oddíl atribut se skládá z dvojice hranaté závorky, které před a za čárkou oddělený seznam jednoho nebo více atributů.</span><span class="sxs-lookup"><span data-stu-id="1fddf-157">An attribute section consists of a pair of square brackets, which surround a comma-separated list of one or more attributes.</span></span> <span data-ttu-id="1fddf-158">Pořadí, ve kterém jsou zadané atributy do tohoto seznamu, a pořadí, ve kterém oddíly připojené do stejné entity programu jsou uspořádané, není důležité.</span><span class="sxs-lookup"><span data-stu-id="1fddf-158">The order in which attributes are specified in such a list, and the order in which sections attached to the same program entity are arranged, is not significant.</span></span> <span data-ttu-id="1fddf-159">Například atributů specifikací `[A][B]`, `[B][A]`, `[A,B]`, a `[B,A]` jsou ekvivalentní.</span><span class="sxs-lookup"><span data-stu-id="1fddf-159">For instance, the attribute specifications `[A][B]`, `[B][A]`, `[A,B]`, and `[B,A]` are equivalent.</span></span>

```antlr
global_attributes
    : global_attribute_section+
    ;

global_attribute_section
    : '[' global_attribute_target_specifier attribute_list ']'
    | '[' global_attribute_target_specifier attribute_list ',' ']'
    ;

global_attribute_target_specifier
    : global_attribute_target ':'
    ;

global_attribute_target
    : 'assembly'
    | 'module'
    ;

attributes
    : attribute_section+
    ;

attribute_section
    : '[' attribute_target_specifier? attribute_list ']'
    | '[' attribute_target_specifier? attribute_list ',' ']'
    ;

attribute_target_specifier
    : attribute_target ':'
    ;

attribute_target
    : 'field'
    | 'event'
    | 'method'
    | 'param'
    | 'property'
    | 'return'
    | 'type'
    ;

attribute_list
    : attribute (',' attribute)*
    ;

attribute
    : attribute_name attribute_arguments?
    ;

attribute_name
    : type_name
    ;

attribute_arguments
    : '(' positional_argument_list? ')'
    | '(' positional_argument_list ',' named_argument_list ')'
    | '(' named_argument_list ')'
    ;

positional_argument_list
    : positional_argument (',' positional_argument)*
    ;

positional_argument
    : attribute_argument_expression
    ;

named_argument_list
    : named_argument (','  named_argument)*
    ;

named_argument
    : identifier '=' attribute_argument_expression
    ;

attribute_argument_expression
    : expression
    ;
```

<span data-ttu-id="1fddf-160">Atribut se skládá ze *attribute_name v relaci* a volitelný seznam poziční a pojmenované argumenty.</span><span class="sxs-lookup"><span data-stu-id="1fddf-160">An attribute consists of an *attribute_name* and an optional list of positional and named arguments.</span></span> <span data-ttu-id="1fddf-161">Poziční argumenty (pokud existuje) předcházet pojmenované argumenty.</span><span class="sxs-lookup"><span data-stu-id="1fddf-161">The positional arguments (if any) precede the named arguments.</span></span> <span data-ttu-id="1fddf-162">Poziční argument se skládá ze *attribute_argument_expression*; pojmenovaný argument se skládá z názvu, za nímž následuje shodné znaménko, za nímž následuje *attribute_argument_expression*, který společně , jsou omezena stejnými pravidly jako jednoduchého přiřazení.</span><span class="sxs-lookup"><span data-stu-id="1fddf-162">A positional argument consists of an *attribute_argument_expression*; a named argument consists of a name, followed by an equal sign, followed by an *attribute_argument_expression*, which, together, are constrained by the same rules as simple assignment.</span></span> <span data-ttu-id="1fddf-163">Pojmenované argumenty pořadí není důležité.</span><span class="sxs-lookup"><span data-stu-id="1fddf-163">The order of named arguments is not significant.</span></span>

<span data-ttu-id="1fddf-164">*Attribute_name v relaci* Určuje třídu atributu.</span><span class="sxs-lookup"><span data-stu-id="1fddf-164">The *attribute_name* identifies an attribute class.</span></span> <span data-ttu-id="1fddf-165">Pokud formu *attribute_name v relaci* je *type_name* potom tento název musí odkazovat na třídu atributu.</span><span class="sxs-lookup"><span data-stu-id="1fddf-165">If the form of *attribute_name* is *type_name* then this name must refer to an attribute class.</span></span> <span data-ttu-id="1fddf-166">V opačném případě dojde k chybě v době kompilace.</span><span class="sxs-lookup"><span data-stu-id="1fddf-166">Otherwise, a compile-time error occurs.</span></span> <span data-ttu-id="1fddf-167">V příkladu</span><span class="sxs-lookup"><span data-stu-id="1fddf-167">The example</span></span>

```csharp
class Class1 {}

[Class1] class Class2 {}    // Error
```

<span data-ttu-id="1fddf-168">výsledkem chyba kompilace, protože se pokouší použít `Class1` jako atribut třídu při `Class1` není třída atributu.</span><span class="sxs-lookup"><span data-stu-id="1fddf-168">results in a compile-time error because it attempts to use `Class1` as an attribute class when `Class1` is not an attribute class.</span></span>

<span data-ttu-id="1fddf-169">Určitých kontextech povolit specifikaci atributu na více než jeden cíl.</span><span class="sxs-lookup"><span data-stu-id="1fddf-169">Certain contexts permit the specification of an attribute on more than one target.</span></span> <span data-ttu-id="1fddf-170">Program můžete explicitně určit cílový zahrnutím *attribute_target_specifier*.</span><span class="sxs-lookup"><span data-stu-id="1fddf-170">A program can explicitly specify the target by including an *attribute_target_specifier*.</span></span> <span data-ttu-id="1fddf-171">Pokud atribut je umístěn na globální úrovni, *global_attribute_target_specifier* je povinný.</span><span class="sxs-lookup"><span data-stu-id="1fddf-171">When an attribute is placed at the global level, a *global_attribute_target_specifier* is required.</span></span> <span data-ttu-id="1fddf-172">V jiných umístěních, se použije přiměřené výchozí hodnoty, ale *attribute_target_specifier* lze potvrdit nebo potlačit výchozí v některých případech nejednoznačný (nebo pouze potvrzují výchozí v-dvojznačné případy).</span><span class="sxs-lookup"><span data-stu-id="1fddf-172">In all other locations, a reasonable default is applied, but an *attribute_target_specifier* can be used to affirm or override the default in certain ambiguous cases (or to just affirm the default in non-ambiguous cases).</span></span> <span data-ttu-id="1fddf-173">Proto, obvykle *attribute_target_specifier*s lze vynechat s výjimkou na globální úrovni.</span><span class="sxs-lookup"><span data-stu-id="1fddf-173">Thus, typically, *attribute_target_specifier*s can be omitted except at the global level.</span></span> <span data-ttu-id="1fddf-174">Potenciálně nejednoznačný kontexty jsou vyřešeny následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="1fddf-174">The potentially ambiguous contexts are resolved as follows:</span></span>

*  <span data-ttu-id="1fddf-175">Atribut zadaný u globálního rozsahu můžete použít buď do cílového sestavení nebo modulu cíl.</span><span class="sxs-lookup"><span data-stu-id="1fddf-175">An attribute specified at global scope can apply either to the target assembly or the target module.</span></span> <span data-ttu-id="1fddf-176">Neexistuje žádná výchozí hodnota pro tento kontext, tak *attribute_target_specifier* je vždy vyžadován v tomto kontextu.</span><span class="sxs-lookup"><span data-stu-id="1fddf-176">No default exists for this context, so an *attribute_target_specifier* is always required in this context.</span></span> <span data-ttu-id="1fddf-177">Přítomnost `assembly` *attribute_target_specifier* naznačuje, že atribut používá k cíli sestavení; přítomnost `module` *attribute_target_specifier* Označuje, že platí atribut pro cílový modul.</span><span class="sxs-lookup"><span data-stu-id="1fddf-177">The presence of the `assembly` *attribute_target_specifier* indicates that the attribute applies to the target assembly; the presence of the `module` *attribute_target_specifier* indicates that the attribute applies to the target module.</span></span>
*  <span data-ttu-id="1fddf-178">Na delegáta, který byl deklarován nebo na jeho návratovou hodnotu, můžete použít atribut zadaný v deklaraci delegáta.</span><span class="sxs-lookup"><span data-stu-id="1fddf-178">An attribute specified on a delegate declaration can apply either to the delegate being declared or to its return value.</span></span> <span data-ttu-id="1fddf-179">Chybí *attribute_target_specifier*, platí atribut pro delegáta.</span><span class="sxs-lookup"><span data-stu-id="1fddf-179">In the absence of an *attribute_target_specifier*, the attribute applies to the delegate.</span></span> <span data-ttu-id="1fddf-180">Přítomnost `type` *attribute_target_specifier* naznačuje, že atribut používá k delegování; přítomnost `return` *attribute_target_specifier* Označuje, že použije atribut návratovou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="1fddf-180">The presence of the `type` *attribute_target_specifier* indicates that the attribute applies to the delegate; the presence of the `return` *attribute_target_specifier* indicates that the attribute applies to the return value.</span></span>
*  <span data-ttu-id="1fddf-181">Atribut zadaný v deklaraci metody můžete použít metody deklarované nebo jeho návratovou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="1fddf-181">An attribute specified on a method declaration can apply either to the method being declared or to its return value.</span></span> <span data-ttu-id="1fddf-182">Chybí *attribute_target_specifier*, platí atribut pro metodu.</span><span class="sxs-lookup"><span data-stu-id="1fddf-182">In the absence of an *attribute_target_specifier*, the attribute applies to the method.</span></span> <span data-ttu-id="1fddf-183">Přítomnost `method` *attribute_target_specifier* naznačuje, že atribut používá k metodě; přítomnost `return` *attribute_target_specifier* označuje že platí atribut pro návratovou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="1fddf-183">The presence of the `method` *attribute_target_specifier* indicates that the attribute applies to the method; the presence of the `return` *attribute_target_specifier* indicates that the attribute applies to the return value.</span></span>
*  <span data-ttu-id="1fddf-184">Atribut zadaný u deklarace operátoru lze použít operátor, který byl deklarován nebo jeho návratovou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="1fddf-184">An attribute specified on an operator declaration can apply either to the operator being declared or to its return value.</span></span> <span data-ttu-id="1fddf-185">Chybí *attribute_target_specifier*, platí atribut pro operátor.</span><span class="sxs-lookup"><span data-stu-id="1fddf-185">In the absence of an *attribute_target_specifier*, the attribute applies to the operator.</span></span> <span data-ttu-id="1fddf-186">Přítomnost `method` *attribute_target_specifier* naznačuje, že atribut používá operátor; přítomnost `return` *attribute_target_specifier* Označuje, že použije atribut návratovou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="1fddf-186">The presence of the `method` *attribute_target_specifier* indicates that the attribute applies to the operator; the presence of the `return` *attribute_target_specifier* indicates that the attribute applies to the return value.</span></span>
*  <span data-ttu-id="1fddf-187">Atribut zadaný v deklaraci události, která vynechává přístupových objektů událostí můžete použít pro události deklarované, související pole (Pokud je událost není abstraktní) nebo pro související přidání a odebrání metody.</span><span class="sxs-lookup"><span data-stu-id="1fddf-187">An attribute specified on an event declaration that omits event accessors can apply to the event being declared, to the associated field (if the event is not abstract), or to the associated add and remove methods.</span></span> <span data-ttu-id="1fddf-188">Chybí *attribute_target_specifier*, platí atribut pro událost.</span><span class="sxs-lookup"><span data-stu-id="1fddf-188">In the absence of an *attribute_target_specifier*, the attribute applies to the event.</span></span> <span data-ttu-id="1fddf-189">Přítomnost `event` *attribute_target_specifier* naznačuje, že atribut používá k události; přítomnost `field` *attribute_target_specifier* označuje platí atribut pro pole. a přítomnost `method` *attribute_target_specifier* naznačuje, že používá atribut do metody.</span><span class="sxs-lookup"><span data-stu-id="1fddf-189">The presence of the `event` *attribute_target_specifier* indicates that the attribute applies to the event; the presence of the `field` *attribute_target_specifier* indicates that the attribute applies to the field; and the presence of the `method` *attribute_target_specifier* indicates that the attribute applies to the methods.</span></span>
*  <span data-ttu-id="1fddf-190">Atribut zadaný u deklarace přistupující objekt get pro vlastnost nebo indexovací člen deklarace můžete použít metodu přidružené nebo jeho návratovou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="1fddf-190">An attribute specified on a get accessor declaration for a property or indexer declaration can apply either to the associated method or to its return value.</span></span> <span data-ttu-id="1fddf-191">Chybí *attribute_target_specifier*, platí atribut pro metodu.</span><span class="sxs-lookup"><span data-stu-id="1fddf-191">In the absence of an *attribute_target_specifier*, the attribute applies to the method.</span></span> <span data-ttu-id="1fddf-192">Přítomnost `method` *attribute_target_specifier* naznačuje, že atribut používá k metodě; přítomnost `return` *attribute_target_specifier* označuje že platí atribut pro návratovou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="1fddf-192">The presence of the `method` *attribute_target_specifier* indicates that the attribute applies to the method; the presence of the `return` *attribute_target_specifier* indicates that the attribute applies to the return value.</span></span>
*  <span data-ttu-id="1fddf-193">Atribut na přístupový objekt set pro vlastnost nebo indexovací člen deklarace můžete použít na přidruženou metodu nebo na jeho jedinou implicitní parametr.</span><span class="sxs-lookup"><span data-stu-id="1fddf-193">An attribute specified on a set accessor for a property or indexer declaration can apply either to the associated method or to its lone implicit parameter.</span></span> <span data-ttu-id="1fddf-194">Chybí *attribute_target_specifier*, platí atribut pro metodu.</span><span class="sxs-lookup"><span data-stu-id="1fddf-194">In the absence of an *attribute_target_specifier*, the attribute applies to the method.</span></span> <span data-ttu-id="1fddf-195">Přítomnost `method` *attribute_target_specifier* naznačuje, že atribut používá k metodě; přítomnost `param` *attribute_target_specifier* označuje platí atribut pro parametr; přítomnost `return` *attribute_target_specifier* označuje, že použije atribut návratovou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="1fddf-195">The presence of the `method` *attribute_target_specifier* indicates that the attribute applies to the method; the presence of the `param` *attribute_target_specifier* indicates that the attribute applies to the parameter; the presence of the `return` *attribute_target_specifier* indicates that the attribute applies to the return value.</span></span>
*  <span data-ttu-id="1fddf-196">Atribut zadaný v deklaraci přístupový objekt add nebo remove pro deklaraci události můžete použít buď na přidruženou metodu, nebo jeho jedinou parametru.</span><span class="sxs-lookup"><span data-stu-id="1fddf-196">An attribute specified on an add or remove accessor declaration for an event declaration can apply either to the associated method or to its lone parameter.</span></span> <span data-ttu-id="1fddf-197">Chybí *attribute_target_specifier*, platí atribut pro metodu.</span><span class="sxs-lookup"><span data-stu-id="1fddf-197">In the absence of an *attribute_target_specifier*, the attribute applies to the method.</span></span> <span data-ttu-id="1fddf-198">Přítomnost `method` *attribute_target_specifier* naznačuje, že atribut používá k metodě; přítomnost `param` *attribute_target_specifier* označuje platí atribut pro parametr; přítomnost `return` *attribute_target_specifier* označuje, že použije atribut návratovou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="1fddf-198">The presence of the `method` *attribute_target_specifier* indicates that the attribute applies to the method; the presence of the `param` *attribute_target_specifier* indicates that the attribute applies to the parameter; the presence of the `return` *attribute_target_specifier* indicates that the attribute applies to the return value.</span></span>

<span data-ttu-id="1fddf-199">V jiných kontextech, zahrnutí *attribute_target_specifier* je povolené, ale zbytečné.</span><span class="sxs-lookup"><span data-stu-id="1fddf-199">In other contexts, inclusion of an *attribute_target_specifier* is permitted but unnecessary.</span></span> <span data-ttu-id="1fddf-200">Například deklaraci třídy může obsahovat nebo vynechat specifikátor `type`:</span><span class="sxs-lookup"><span data-stu-id="1fddf-200">For instance, a class declaration may either include or omit the specifier `type`:</span></span>

```csharp
[type: Author("Brian Kernighan")]
class Class1 {}

[Author("Dennis Ritchie")]
class Class2 {}
```

<span data-ttu-id="1fddf-201">Jedná se o chybu, chcete-li určit neplatný *attribute_target_specifier*.</span><span class="sxs-lookup"><span data-stu-id="1fddf-201">It is an error to specify an invalid *attribute_target_specifier*.</span></span> <span data-ttu-id="1fddf-202">Například specifikátor `param` nelze použít v deklaraci třídy:</span><span class="sxs-lookup"><span data-stu-id="1fddf-202">For instance, the specifier `param` cannot be used on a class declaration:</span></span>

```csharp
[param: Author("Brian Kernighan")]        // Error
class Class1 {}
```

<span data-ttu-id="1fddf-203">Podle konvence jsou pojmenovány třídy atributů s příponu `Attribute`.</span><span class="sxs-lookup"><span data-stu-id="1fddf-203">By convention, attribute classes are named with a suffix of `Attribute`.</span></span> <span data-ttu-id="1fddf-204">*Attribute_name v relaci* formuláře *type_name* může zahrnovat nebo vynechejte tuto příponu.</span><span class="sxs-lookup"><span data-stu-id="1fddf-204">An *attribute_name* of the form *type_name* may either include or omit this suffix.</span></span> <span data-ttu-id="1fddf-205">Pokud se najde třídu atributu s i bez této přípony, existuje nejednoznačnost a způsobí chybu kompilace.</span><span class="sxs-lookup"><span data-stu-id="1fddf-205">If an attribute class is found both with and without this suffix, an ambiguity is present, and a compile-time error results.</span></span> <span data-ttu-id="1fddf-206">Pokud *attribute_name v relaci* je napsán tak, aby jeho krajním *identifikátor* Doslovný identifikátor ([identifikátory](lexical-structure.md#identifiers)), pak pouze atribut bez přípony je nalezena shoda, což umožní tato nejednoznačnost vyřešit.</span><span class="sxs-lookup"><span data-stu-id="1fddf-206">If the *attribute_name* is spelled such that its right-most *identifier* is a verbatim identifier ([Identifiers](lexical-structure.md#identifiers)), then only an attribute without a suffix is matched, thus enabling such an ambiguity to be resolved.</span></span> <span data-ttu-id="1fddf-207">V příkladu</span><span class="sxs-lookup"><span data-stu-id="1fddf-207">The example</span></span>

```csharp
using System;

[AttributeUsage(AttributeTargets.All)]
public class X: Attribute
{}

[AttributeUsage(AttributeTargets.All)]
public class XAttribute: Attribute
{}

[X]                     // Error: ambiguity
class Class1 {}

[XAttribute]            // Refers to XAttribute
class Class2 {}

[@X]                    // Refers to X
class Class3 {}

[@XAttribute]           // Refers to XAttribute
class Class4 {}
```

<span data-ttu-id="1fddf-208">ukazuje dvě atribut s názvem třídy `X` a `XAttribute`.</span><span class="sxs-lookup"><span data-stu-id="1fddf-208">shows two attribute classes named `X` and `XAttribute`.</span></span> <span data-ttu-id="1fddf-209">Atribut `[X]` je nejednoznačný, protože by mohla odkazovat buď `X` nebo `XAttribute`.</span><span class="sxs-lookup"><span data-stu-id="1fddf-209">The attribute `[X]` is ambiguous, since it could refer to either `X` or `XAttribute`.</span></span> <span data-ttu-id="1fddf-210">Použití Doslovný identifikátor umožňuje přesnou záměr zadání v těchto výjimečných případech.</span><span class="sxs-lookup"><span data-stu-id="1fddf-210">Using a verbatim identifier allows the exact intent to be specified in such rare cases.</span></span> <span data-ttu-id="1fddf-211">Atribut `[XAttribute]` není nejednoznačný (i když by být, pokud byla třída atributu s názvem `XAttributeAttribute`!).</span><span class="sxs-lookup"><span data-stu-id="1fddf-211">The attribute `[XAttribute]` is not ambiguous (although it would be if there was an attribute class named `XAttributeAttribute`!).</span></span> <span data-ttu-id="1fddf-212">Pokud deklarace třídy `X` Odebereme, pak oba atributy odkazovat na třídu atributu s názvem `XAttribute`, následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="1fddf-212">If the declaration for class `X` is removed, then both attributes refer to the attribute class named `XAttribute`, as follows:</span></span>

```csharp
using System;

[AttributeUsage(AttributeTargets.All)]
public class XAttribute: Attribute
{}

[X]                     // Refers to XAttribute
class Class1 {}

[XAttribute]            // Refers to XAttribute
class Class2 {}

[@X]                    // Error: no attribute named "X"
class Class3 {}
```

<span data-ttu-id="1fddf-213">Je chyba kompilace použít třídu atributů jedno použití více než jednou na stejné entity.</span><span class="sxs-lookup"><span data-stu-id="1fddf-213">It is a compile-time error to use a single-use attribute class more than once on the same entity.</span></span> <span data-ttu-id="1fddf-214">V příkladu</span><span class="sxs-lookup"><span data-stu-id="1fddf-214">The example</span></span>

```csharp
using System;

[AttributeUsage(AttributeTargets.Class)]
public class HelpStringAttribute: Attribute
{
    string value;

    public HelpStringAttribute(string value) {
        this.value = value;
    }

    public string Value {
        get {...}
    }
}

[HelpString("Description of Class1")]
[HelpString("Another description of Class1")]
public class Class1 {}
```

<span data-ttu-id="1fddf-215">výsledkem chyba kompilace, protože se pokouší použít `HelpString`, tedy třídu atributu na jedno použití více než jednou v deklaraci `Class1`.</span><span class="sxs-lookup"><span data-stu-id="1fddf-215">results in a compile-time error because it attempts to use `HelpString`, which is a single-use attribute class, more than once on the declaration of `Class1`.</span></span>

<span data-ttu-id="1fddf-216">Výraz `E` je *attribute_argument_expression* Pokud jsou splněny všechny následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="1fddf-216">An expression `E` is an *attribute_argument_expression* if all of the following statements are true:</span></span>

*  <span data-ttu-id="1fddf-217">Typ `E` je typ parametru atributu ([typy parametrů atributů](attributes.md#attribute-parameter-types)).</span><span class="sxs-lookup"><span data-stu-id="1fddf-217">The type of `E` is an attribute parameter type ([Attribute parameter types](attributes.md#attribute-parameter-types)).</span></span>
*  <span data-ttu-id="1fddf-218">V době kompilace, hodnota `E` bylo možné přeložit na jednu z následujících akcí:</span><span class="sxs-lookup"><span data-stu-id="1fddf-218">At compile-time, the value of `E` can be resolved to one of the following:</span></span>
   * <span data-ttu-id="1fddf-219">Konstantní hodnotu.</span><span class="sxs-lookup"><span data-stu-id="1fddf-219">A constant value.</span></span>
   * <span data-ttu-id="1fddf-220">A `System.Type` objektu.</span><span class="sxs-lookup"><span data-stu-id="1fddf-220">A `System.Type` object.</span></span>
   * <span data-ttu-id="1fddf-221">Jednorozměrné pole *attribute_argument_expression*s.</span><span class="sxs-lookup"><span data-stu-id="1fddf-221">A one-dimensional array of *attribute_argument_expression*s.</span></span>

<span data-ttu-id="1fddf-222">Příklad:</span><span class="sxs-lookup"><span data-stu-id="1fddf-222">For example:</span></span>

```csharp
using System;

[AttributeUsage(AttributeTargets.Class)]
public class TestAttribute: Attribute
{
    public int P1 {
        get {...}
        set {...}
    }

    public Type P2 {
        get {...}
        set {...}
    }

    public object P3 {
        get {...}
        set {...}
    }
}

[Test(P1 = 1234, P3 = new int[] {1, 3, 5}, P2 = typeof(float))]
class MyClass {}
```

<span data-ttu-id="1fddf-223">A *typeof_expression* ([operátor typeof](expressions.md#the-typeof-operator)) použít jako výraz argumentu atributu může odkazovat neobecný typ, uzavřený konstruovaný typ. nebo nevázaný parametr generického typu, ale nemůže odkazovat Otevřete typu.</span><span class="sxs-lookup"><span data-stu-id="1fddf-223">A *typeof_expression* ([The typeof operator](expressions.md#the-typeof-operator)) used as an attribute argument expression can reference a non-generic type, a closed constructed type, or an unbound generic type, but it cannot reference an open type.</span></span> <span data-ttu-id="1fddf-224">Tím je zajištěno, že výraz lze vyřešit v době kompilace.</span><span class="sxs-lookup"><span data-stu-id="1fddf-224">This is to ensure that the expression can be resolved at compile-time.</span></span>

```csharp
class A: Attribute
{
    public A(Type t) {...}
}

class G<T>
{
    [A(typeof(T))] T t;                  // Error, open type in attribute
}

class X
{
    [A(typeof(List<int>))] int x;        // Ok, closed constructed type
    [A(typeof(List<>))] int y;           // Ok, unbound generic type
}
```

## <a name="attribute-instances"></a><span data-ttu-id="1fddf-225">Instance atributu</span><span class="sxs-lookup"><span data-stu-id="1fddf-225">Attribute instances</span></span>

<span data-ttu-id="1fddf-226">***Instance atributu*** je instanci, která představuje atribut v době běhu.</span><span class="sxs-lookup"><span data-stu-id="1fddf-226">An ***attribute instance*** is an instance that represents an attribute at run-time.</span></span> <span data-ttu-id="1fddf-227">Atribut je definován pomocí třídy atributu, poziční argumenty a pojmenované argumenty.</span><span class="sxs-lookup"><span data-stu-id="1fddf-227">An attribute is defined with an attribute class, positional arguments, and named arguments.</span></span> <span data-ttu-id="1fddf-228">Instance atributu je instancí třídy atributu, který je inicializován s poziční a pojmenované argumenty.</span><span class="sxs-lookup"><span data-stu-id="1fddf-228">An attribute instance is an instance of the attribute class that is initialized with the positional and named arguments.</span></span>

<span data-ttu-id="1fddf-229">Načtení instance atributu zahrnuje kompilace a spuštění zpracování, jak je popsáno v následujících částech.</span><span class="sxs-lookup"><span data-stu-id="1fddf-229">Retrieval of an attribute instance involves both compile-time and run-time processing, as described in the following sections.</span></span>

### <a name="compilation-of-an-attribute"></a><span data-ttu-id="1fddf-230">Kompilace atributu</span><span class="sxs-lookup"><span data-stu-id="1fddf-230">Compilation of an attribute</span></span>

<span data-ttu-id="1fddf-231">Kompilace *atribut* třídou atributu `T`, *positional_argument_list* `P` a *named_argument_list* `N`, se skládá z následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="1fddf-231">The compilation of an *attribute* with attribute class `T`, *positional_argument_list* `P` and *named_argument_list* `N`, consists of the following steps:</span></span>

*  <span data-ttu-id="1fddf-232">Postupujte podle kroků zpracování kompilace pro kompilaci *object_creation_expression* formuláře `new T(P)`.</span><span class="sxs-lookup"><span data-stu-id="1fddf-232">Follow the compile-time processing steps for compiling an *object_creation_expression* of the form `new T(P)`.</span></span> <span data-ttu-id="1fddf-233">Tyto kroky vyústí v chybu v době kompilace, nebo určit konstruktor instance `C` na `T` , který může být vyvolána v době běhu.</span><span class="sxs-lookup"><span data-stu-id="1fddf-233">These steps either result in a compile-time error, or determine an instance constructor `C` on `T` that can be invoked at run-time.</span></span>
*  <span data-ttu-id="1fddf-234">Pokud `C` nemá přístupnost public, dojde k chybě v době kompilace.</span><span class="sxs-lookup"><span data-stu-id="1fddf-234">If `C` does not have public accessibility, then a compile-time error occurs.</span></span>
*  <span data-ttu-id="1fddf-235">Pro každou *named_argument* `Arg` v `N`:</span><span class="sxs-lookup"><span data-stu-id="1fddf-235">For each *named_argument* `Arg` in `N`:</span></span>
   * <span data-ttu-id="1fddf-236">Umožní `Name` být *identifikátor* z *named_argument* `Arg`.</span><span class="sxs-lookup"><span data-stu-id="1fddf-236">Let `Name` be the *identifier* of the *named_argument* `Arg`.</span></span>
   * <span data-ttu-id="1fddf-237">`Name` musíte určit o nestatická čtení a zápis veřejné pole ani vlastnost na `T`.</span><span class="sxs-lookup"><span data-stu-id="1fddf-237">`Name` must identify a non-static read-write public field or property on `T`.</span></span> <span data-ttu-id="1fddf-238">Pokud `T` nemá žádný odpovídající pole nebo vlastnost, dojde k chybě v době kompilace.</span><span class="sxs-lookup"><span data-stu-id="1fddf-238">If `T` has no such field or property, then a compile-time error occurs.</span></span>
*  <span data-ttu-id="1fddf-239">Následující informace pro vytvoření instance za běhu atributu: třída atributů `T`, konstruktor instance `C` na `T`, *positional_argument_list* `P` a *named_argument_list* `N`.</span><span class="sxs-lookup"><span data-stu-id="1fddf-239">Keep the following information for run-time instantiation of the attribute: the attribute class `T`, the instance constructor `C` on `T`, the *positional_argument_list* `P` and the *named_argument_list* `N`.</span></span>

### <a name="run-time-retrieval-of-an-attribute-instance"></a><span data-ttu-id="1fddf-240">Za běhu načítání instance atributu</span><span class="sxs-lookup"><span data-stu-id="1fddf-240">Run-time retrieval of an attribute instance</span></span>

<span data-ttu-id="1fddf-241">Kompilace *atribut* vrací třídu atributu `T`, konstruktor instance `C` na `T`, *positional_argument_list* `P`a *named_argument_list* `N`.</span><span class="sxs-lookup"><span data-stu-id="1fddf-241">Compilation of an *attribute* yields an attribute class `T`, an instance constructor `C` on `T`, a *positional_argument_list* `P`, and a *named_argument_list* `N`.</span></span> <span data-ttu-id="1fddf-242">Tyto informace zadané, je možné načíst instanci atributu v době běhu pomocí následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="1fddf-242">Given this information, an attribute instance can be retrieved at run-time using the following steps:</span></span>

*  <span data-ttu-id="1fddf-243">Postupujte podle kroků zpracování za běhu pro spuštění *object_creation_expression* formuláře `new T(P)`, pomocí konstruktoru instance `C` počítáno v době kompilace.</span><span class="sxs-lookup"><span data-stu-id="1fddf-243">Follow the run-time processing steps for executing an *object_creation_expression* of the form `new T(P)`, using the instance constructor `C` as determined at compile-time.</span></span> <span data-ttu-id="1fddf-244">Tyto kroky za následek výjimku, nebo vytvořit instanci `O` z `T`.</span><span class="sxs-lookup"><span data-stu-id="1fddf-244">These steps either result in an exception, or produce an instance `O` of `T`.</span></span>
*  <span data-ttu-id="1fddf-245">Pro každou *named_argument* `Arg` v `N`, v pořadí:</span><span class="sxs-lookup"><span data-stu-id="1fddf-245">For each *named_argument* `Arg` in `N`, in order:</span></span>
   * <span data-ttu-id="1fddf-246">Umožní `Name` být *identifikátor* z *named_argument* `Arg`.</span><span class="sxs-lookup"><span data-stu-id="1fddf-246">Let `Name` be the *identifier* of the *named_argument* `Arg`.</span></span> <span data-ttu-id="1fddf-247">Pokud `Name` neidentifikuje nestatické veřejné čtení a zápis pole nebo vlastnost na `O`, pak je vyvolána výjimka.</span><span class="sxs-lookup"><span data-stu-id="1fddf-247">If `Name` does not identify a non-static public read-write field or property on `O`, then an exception is thrown.</span></span>
   * <span data-ttu-id="1fddf-248">Umožní `Value` být výsledek vyhodnocení výrazu *attribute_argument_expression* z `Arg`.</span><span class="sxs-lookup"><span data-stu-id="1fddf-248">Let `Value` be the result of evaluating the *attribute_argument_expression* of `Arg`.</span></span>
   * <span data-ttu-id="1fddf-249">Pokud `Name` identifikuje pole na `O`, nastavte toto pole na `Value`.</span><span class="sxs-lookup"><span data-stu-id="1fddf-249">If `Name` identifies a field on `O`, then set this field to `Value`.</span></span>
   * <span data-ttu-id="1fddf-250">V opačném případě `Name` identifikuje vlastnosti v `O`.</span><span class="sxs-lookup"><span data-stu-id="1fddf-250">Otherwise, `Name` identifies a property on `O`.</span></span> <span data-ttu-id="1fddf-251">Tuto vlastnost nastavte na `Value`.</span><span class="sxs-lookup"><span data-stu-id="1fddf-251">Set this property to `Value`.</span></span>
   * <span data-ttu-id="1fddf-252">Výsledkem je `O`, instance třídy atributu `T` , který byl inicializován s *positional_argument_list* `P` a *named_argument_list* `N`.</span><span class="sxs-lookup"><span data-stu-id="1fddf-252">The result is `O`, an instance of the attribute class `T` that has been initialized with the *positional_argument_list* `P` and the *named_argument_list* `N`.</span></span>

## <a name="reserved-attributes"></a><span data-ttu-id="1fddf-253">Vyhrazené atributy</span><span class="sxs-lookup"><span data-stu-id="1fddf-253">Reserved attributes</span></span>

<span data-ttu-id="1fddf-254">Malý počet atributy ovlivňují jazyk nějakým způsobem.</span><span class="sxs-lookup"><span data-stu-id="1fddf-254">A small number of attributes affect the language in some way.</span></span> <span data-ttu-id="1fddf-255">Tyto atributy zahrnout:</span><span class="sxs-lookup"><span data-stu-id="1fddf-255">These attributes include:</span></span>

*  <span data-ttu-id="1fddf-256">`System.AttributeUsageAttribute` ([Atribut the AttributeUsage](attributes.md#the-attributeusage-attribute)), který se používá k popisu způsoby, ve kterém můžete použít třídu atributu.</span><span class="sxs-lookup"><span data-stu-id="1fddf-256">`System.AttributeUsageAttribute` ([The AttributeUsage attribute](attributes.md#the-attributeusage-attribute)), which is used to describe the ways in which an attribute class can be used.</span></span>
*  <span data-ttu-id="1fddf-257">`System.Diagnostics.ConditionalAttribute` ([The podmíněný atribut](attributes.md#the-conditional-attribute)), který se používá k definování podmíněné metody.</span><span class="sxs-lookup"><span data-stu-id="1fddf-257">`System.Diagnostics.ConditionalAttribute` ([The Conditional attribute](attributes.md#the-conditional-attribute)), which is used to define conditional methods.</span></span>
*  <span data-ttu-id="1fddf-258">`System.ObsoleteAttribute` ([The zastaralé atribut](attributes.md#the-obsolete-attribute)), který se používá k označení člena jako zastaralé.</span><span class="sxs-lookup"><span data-stu-id="1fddf-258">`System.ObsoleteAttribute` ([The Obsolete attribute](attributes.md#the-obsolete-attribute)), which is used to mark a member as obsolete.</span></span>
*  <span data-ttu-id="1fddf-259">`System.Runtime.CompilerServices.CallerLineNumberAttribute`, `System.Runtime.CompilerServices.CallerFilePathAttribute` a `System.Runtime.CompilerServices.CallerMemberNameAttribute` ([atributy informace o volajícím](attributes.md#caller-info-attributes)), které se používají k zadání informací o volání kontextu na volitelné parametry.</span><span class="sxs-lookup"><span data-stu-id="1fddf-259">`System.Runtime.CompilerServices.CallerLineNumberAttribute`, `System.Runtime.CompilerServices.CallerFilePathAttribute` and `System.Runtime.CompilerServices.CallerMemberNameAttribute` ([Caller info attributes](attributes.md#caller-info-attributes)), which are used to supply information about the calling context to optional parameters.</span></span>

### <a name="the-attributeusage-attribute"></a><span data-ttu-id="1fddf-260">Atribut AttributeUsage</span><span class="sxs-lookup"><span data-stu-id="1fddf-260">The AttributeUsage attribute</span></span>

<span data-ttu-id="1fddf-261">Atribut `AttributeUsage` se používá k popisu způsobu, ve kterém lze použít na třídu atributu.</span><span class="sxs-lookup"><span data-stu-id="1fddf-261">The attribute `AttributeUsage` is used to describe the manner in which the attribute class can be used.</span></span>

<span data-ttu-id="1fddf-262">Třída, která je upravena pomocí `AttributeUsage` atribut musí být odvozen od `System.Attribute`, buď přímo nebo nepřímo.</span><span class="sxs-lookup"><span data-stu-id="1fddf-262">A class that is decorated with the `AttributeUsage` attribute must derive from `System.Attribute`, either directly or indirectly.</span></span> <span data-ttu-id="1fddf-263">V opačném případě dojde k chybě v době kompilace.</span><span class="sxs-lookup"><span data-stu-id="1fddf-263">Otherwise, a compile-time error occurs.</span></span>

```csharp
namespace System
{
    [AttributeUsage(AttributeTargets.Class)]
    public class AttributeUsageAttribute: Attribute
    {
        public AttributeUsageAttribute(AttributeTargets validOn) {...}
        public virtual bool AllowMultiple { get {...} set {...} }
        public virtual bool Inherited { get {...} set {...} }
        public virtual AttributeTargets ValidOn { get {...} }
    }

    public enum AttributeTargets
    {
        Assembly     = 0x0001,
        Module       = 0x0002,
        Class        = 0x0004,
        Struct       = 0x0008,
        Enum         = 0x0010,
        Constructor  = 0x0020,
        Method       = 0x0040,
        Property     = 0x0080,
        Field        = 0x0100,
        Event        = 0x0200,
        Interface    = 0x0400,
        Parameter    = 0x0800,
        Delegate     = 0x1000,
        ReturnValue  = 0x2000,

        All = Assembly | Module | Class | Struct | Enum | Constructor | 
            Method | Property | Field | Event | Interface | Parameter | 
            Delegate | ReturnValue
    }
}
```

### <a name="the-conditional-attribute"></a><span data-ttu-id="1fddf-264">Atribut Conditional.</span><span class="sxs-lookup"><span data-stu-id="1fddf-264">The Conditional attribute</span></span>

<span data-ttu-id="1fddf-265">Atribut `Conditional` umožňuje definici ***podmíněné metody*** a ***atribut conditional třídy***.</span><span class="sxs-lookup"><span data-stu-id="1fddf-265">The attribute `Conditional` enables the definition of ***conditional methods*** and ***conditional attribute classes***.</span></span>

```csharp
namespace System.Diagnostics
{
    [AttributeUsage(AttributeTargets.Method | AttributeTargets.Class, AllowMultiple = true)]
    public class ConditionalAttribute: Attribute
    {
        public ConditionalAttribute(string conditionString) {...}
        public string ConditionString { get {...} }
    }
}
```

#### <a name="conditional-methods"></a><span data-ttu-id="1fddf-266">Podmíněné metody.</span><span class="sxs-lookup"><span data-stu-id="1fddf-266">Conditional methods</span></span>

<span data-ttu-id="1fddf-267">Metody upravené pomocí `Conditional` atribut je podmíněná metoda.</span><span class="sxs-lookup"><span data-stu-id="1fddf-267">A method decorated with the `Conditional` attribute is a conditional method.</span></span> <span data-ttu-id="1fddf-268">`Conditional` Atribut označuje podmínku otestováním symbol podmíněné kompilace.</span><span class="sxs-lookup"><span data-stu-id="1fddf-268">The `Conditional` attribute indicates a condition by testing a conditional compilation symbol.</span></span> <span data-ttu-id="1fddf-269">Volání podmíněné metody jsou zahrnuty nebo v závislosti na tom, zda je tento symbol definován místě volání vynechán.</span><span class="sxs-lookup"><span data-stu-id="1fddf-269">Calls to a conditional method are either included or omitted depending on whether this symbol is defined at the point of the call.</span></span> <span data-ttu-id="1fddf-270">Pokud je definován symbol, je součástí; volání v opačném případě je vynechána, volání (včetně hodnocení příjemce a parametry volání).</span><span class="sxs-lookup"><span data-stu-id="1fddf-270">If the symbol is defined, the call is included; otherwise, the call (including evaluation of the receiver and parameters of the call) is omitted.</span></span>

<span data-ttu-id="1fddf-271">Podmíněná metoda je v souladu s následujícími omezeními:</span><span class="sxs-lookup"><span data-stu-id="1fddf-271">A conditional method is subject to the following restrictions:</span></span>

*  <span data-ttu-id="1fddf-272">Podmíněná metoda musí být metoda ve *class_declaration* nebo *struct_declaration*.</span><span class="sxs-lookup"><span data-stu-id="1fddf-272">The conditional method must be a method in a *class_declaration* or *struct_declaration*.</span></span> <span data-ttu-id="1fddf-273">Pokud dojde k chybě kompilace `Conditional` je zadán atribut pro metodu v deklaraci rozhraní.</span><span class="sxs-lookup"><span data-stu-id="1fddf-273">A compile-time error occurs if the `Conditional` attribute is specified on a method in an interface declaration.</span></span>
*  <span data-ttu-id="1fddf-274">Podmíněná metoda musí mít typ vrácené hodnoty `void`.</span><span class="sxs-lookup"><span data-stu-id="1fddf-274">The conditional method must have a return type of `void`.</span></span>
*  <span data-ttu-id="1fddf-275">Podmíněná metoda nesmí být označené `override` modifikátor.</span><span class="sxs-lookup"><span data-stu-id="1fddf-275">The conditional method must not be marked with the `override` modifier.</span></span> <span data-ttu-id="1fddf-276">Podmíněná metoda může být označena s `virtual` modifikátor, ale.</span><span class="sxs-lookup"><span data-stu-id="1fddf-276">A conditional method may be marked with the `virtual` modifier, however.</span></span> <span data-ttu-id="1fddf-277">Přepsání metody jsou implicitně podmíněné a nesmí být označené explicitně `Conditional` atribut.</span><span class="sxs-lookup"><span data-stu-id="1fddf-277">Overrides of such a method are implicitly conditional, and must not be explicitly marked with a `Conditional` attribute.</span></span>
*  <span data-ttu-id="1fddf-278">Podmíněná metoda nesmí být implementace metody rozhraní.</span><span class="sxs-lookup"><span data-stu-id="1fddf-278">The conditional method must not be an implementation of an interface method.</span></span> <span data-ttu-id="1fddf-279">V opačném případě dojde k chybě v době kompilace.</span><span class="sxs-lookup"><span data-stu-id="1fddf-279">Otherwise, a compile-time error occurs.</span></span>

<span data-ttu-id="1fddf-280">Kromě toho dojde k chybě kompilace, pokud podmíněná metoda se používá v *delegate_creation_expression*.</span><span class="sxs-lookup"><span data-stu-id="1fddf-280">In addition, a compile-time error occurs if a conditional method is used in a *delegate_creation_expression*.</span></span> <span data-ttu-id="1fddf-281">V příkladu</span><span class="sxs-lookup"><span data-stu-id="1fddf-281">The example</span></span>

```csharp
#define DEBUG

using System;
using System.Diagnostics;

class Class1 
{
    [Conditional("DEBUG")]
    public static void M() {
        Console.WriteLine("Executed Class1.M");
    }
}

class Class2
{
    public static void Test() {
        Class1.M();
    }
}
```

<span data-ttu-id="1fddf-282">deklaruje `Class1.M` jako podmíněná metoda.</span><span class="sxs-lookup"><span data-stu-id="1fddf-282">declares `Class1.M` as a conditional method.</span></span> <span data-ttu-id="1fddf-283">`Class2`společnosti `Test` metoda volá tuto metodu.</span><span class="sxs-lookup"><span data-stu-id="1fddf-283">`Class2`'s `Test` method calls this method.</span></span> <span data-ttu-id="1fddf-284">Protože podmíněné kompilace symbol `DEBUG` je definováno, pokud `Class2.Test` je volána, bude se volat `M`.</span><span class="sxs-lookup"><span data-stu-id="1fddf-284">Since the conditional compilation symbol `DEBUG` is defined, if `Class2.Test` is called, it will call `M`.</span></span> <span data-ttu-id="1fddf-285">Pokud se symbol `DEBUG` kdyby byla definována, pak `Class2.Test` by volat `Class1.M`.</span><span class="sxs-lookup"><span data-stu-id="1fddf-285">If the symbol `DEBUG` had not been defined, then `Class2.Test` would not call `Class1.M`.</span></span>

<span data-ttu-id="1fddf-286">Je důležité si uvědomit, že zahrnutí nebo vyloučení volání podmíněné metody se řídí symboly podmíněné kompilace místě volání.</span><span class="sxs-lookup"><span data-stu-id="1fddf-286">It is important to note that the inclusion or exclusion of a call to a conditional method is controlled by the conditional compilation symbols at the point of the call.</span></span> <span data-ttu-id="1fddf-287">V příkladu</span><span class="sxs-lookup"><span data-stu-id="1fddf-287">In the example</span></span>

<span data-ttu-id="1fddf-288">Soubor `class1.cs`:</span><span class="sxs-lookup"><span data-stu-id="1fddf-288">File `class1.cs`:</span></span>

```csharp
using System.Diagnostics;

class Class1 
{
    [Conditional("DEBUG")]
    public static void F() {
        Console.WriteLine("Executed Class1.F");
    }
}
```

<span data-ttu-id="1fddf-289">Soubor `class2.cs`:</span><span class="sxs-lookup"><span data-stu-id="1fddf-289">File `class2.cs`:</span></span>

```csharp
#define DEBUG

class Class2
{
    public static void G() {
        Class1.F();                // F is called
    }
}
```

<span data-ttu-id="1fddf-290">Soubor `class3.cs`:</span><span class="sxs-lookup"><span data-stu-id="1fddf-290">File `class3.cs`:</span></span>

```csharp
#undef DEBUG

class Class3
{
    public static void H() {
        Class1.F();                // F is not called
    }
}
```

<span data-ttu-id="1fddf-291">třídy `Class2` a `Class3` každý obsahuje volání metodě podmíněného `Class1.F`, což je podmíněné podle, jestli `DEBUG` je definována.</span><span class="sxs-lookup"><span data-stu-id="1fddf-291">the classes `Class2` and `Class3` each contain calls to the conditional method `Class1.F`, which is conditional based on whether or not `DEBUG` is defined.</span></span> <span data-ttu-id="1fddf-292">Protože je tento symbol definovaný v rámci `Class2` , ale ne `Class3`, volání `F` v `Class2` je zahrnuta, při volání `F` v `Class3` je vynechán.</span><span class="sxs-lookup"><span data-stu-id="1fddf-292">Since this symbol is defined in the context of `Class2` but not `Class3`, the call to `F` in `Class2` is included, while the call to `F` in `Class3` is omitted.</span></span>

<span data-ttu-id="1fddf-293">Použití podmíněné metody v řetěz dědičnosti může být matoucí.</span><span class="sxs-lookup"><span data-stu-id="1fddf-293">The use of conditional methods in an inheritance chain can be confusing.</span></span> <span data-ttu-id="1fddf-294">Volání podmíněná metoda prostřednictvím `base`, formuláře `base.M`, jsou v souladu s pravidly volání normální podmíněná metoda.</span><span class="sxs-lookup"><span data-stu-id="1fddf-294">Calls made to a conditional method through `base`, of the form `base.M`, are subject to the normal conditional method call rules.</span></span> <span data-ttu-id="1fddf-295">V příkladu</span><span class="sxs-lookup"><span data-stu-id="1fddf-295">In the example</span></span>

<span data-ttu-id="1fddf-296">Soubor `class1.cs`:</span><span class="sxs-lookup"><span data-stu-id="1fddf-296">File `class1.cs`:</span></span>

```csharp
using System;
using System.Diagnostics;

class Class1 
{
    [Conditional("DEBUG")]
    public virtual void M() {
        Console.WriteLine("Class1.M executed");
    }
}
```

<span data-ttu-id="1fddf-297">Soubor `class2.cs`:</span><span class="sxs-lookup"><span data-stu-id="1fddf-297">File `class2.cs`:</span></span>

```csharp
using System;

class Class2: Class1
{
    public override void M() {
        Console.WriteLine("Class2.M executed");
        base.M();                        // base.M is not called!
    }
}
```

<span data-ttu-id="1fddf-298">Soubor `class3.cs`:</span><span class="sxs-lookup"><span data-stu-id="1fddf-298">File `class3.cs`:</span></span>

```csharp
#define DEBUG

using System;

class Class3
{
    public static void Test() {
        Class2 c = new Class2();
        c.M();                            // M is called
    }
}
```

<span data-ttu-id="1fddf-299">`Class2` obsahuje volání `M` definované v její základní třídě.</span><span class="sxs-lookup"><span data-stu-id="1fddf-299">`Class2` includes a call to the `M` defined in its base class.</span></span> <span data-ttu-id="1fddf-300">Toto volání je vynechána, protože podmíněné na základě přítomnosti symbolu je základní metoda `DEBUG`, který není definován.</span><span class="sxs-lookup"><span data-stu-id="1fddf-300">This call is omitted because the base method is conditional based on the presence of the symbol `DEBUG`, which is undefined.</span></span> <span data-ttu-id="1fddf-301">Díky tomu se metoda zapisuje do konzoly "`Class2.M executed`" pouze.</span><span class="sxs-lookup"><span data-stu-id="1fddf-301">Thus, the method writes to the console "`Class2.M executed`" only.</span></span> <span data-ttu-id="1fddf-302">Použití rozumné *pp_declaration*s můžete tyto problémy eliminovat.</span><span class="sxs-lookup"><span data-stu-id="1fddf-302">Judicious use of *pp_declaration*s can eliminate such problems.</span></span>

#### <a name="conditional-attribute-classes"></a><span data-ttu-id="1fddf-303">Atribut Conditional třídy</span><span class="sxs-lookup"><span data-stu-id="1fddf-303">Conditional attribute classes</span></span>

<span data-ttu-id="1fddf-304">Třídu atributu ([atribut třídy](attributes.md#attribute-classes)) dekorovaných s jedním nebo více `Conditional` je atributy ***třídě atribut conditional***.</span><span class="sxs-lookup"><span data-stu-id="1fddf-304">An attribute class ([Attribute classes](attributes.md#attribute-classes)) decorated with one or more `Conditional` attributes is a ***conditional attribute class***.</span></span> <span data-ttu-id="1fddf-305">Třída atribut conditional je tedy přidružené symboly podmíněné kompilace deklarované v jeho `Conditional` atributy.</span><span class="sxs-lookup"><span data-stu-id="1fddf-305">A conditional attribute class is thus associated with the conditional compilation symbols declared in its `Conditional` attributes.</span></span> <span data-ttu-id="1fddf-306">V tomto příkladu:</span><span class="sxs-lookup"><span data-stu-id="1fddf-306">This example:</span></span>

```csharp
using System;
using System.Diagnostics;
[Conditional("ALPHA")]
[Conditional("BETA")]
public class TestAttribute : Attribute {}
```

<span data-ttu-id="1fddf-307">deklaruje `TestAttribute` jako atribut conditional třídy přidružené symboly podmíněné kompilace `ALPHA` a `BETA`.</span><span class="sxs-lookup"><span data-stu-id="1fddf-307">declares `TestAttribute` as a conditional attribute class associated with the conditional compilations symbols `ALPHA` and `BETA`.</span></span>

<span data-ttu-id="1fddf-308">Atribut specifikace ([specifikace atributu](attributes.md#attribute-specification)) podmíněné atributu jsou zahrnuty, pokud jeden nebo více jeho symboly podmíněné kompilace přidružené je definována místě specifikace, jinak atribut specifikace je vynechán.</span><span class="sxs-lookup"><span data-stu-id="1fddf-308">Attribute specifications ([Attribute specification](attributes.md#attribute-specification)) of a conditional attribute are included if one or more of its associated conditional compilation symbols is defined at the point of specification, otherwise the attribute specification is omitted.</span></span>

<span data-ttu-id="1fddf-309">Je důležité si uvědomit, že zahrnutí nebo vyloučení specifikaci atributu třídy atribut conditional je řízen symboly podmíněné kompilace místě specifikaci.</span><span class="sxs-lookup"><span data-stu-id="1fddf-309">It is important to note that the inclusion or exclusion of an attribute specification of a conditional attribute class is controlled by the conditional compilation symbols at the point of the specification.</span></span> <span data-ttu-id="1fddf-310">V příkladu</span><span class="sxs-lookup"><span data-stu-id="1fddf-310">In the example</span></span>

<span data-ttu-id="1fddf-311">Soubor `test.cs`:</span><span class="sxs-lookup"><span data-stu-id="1fddf-311">File `test.cs`:</span></span>

```csharp
using System;
using System.Diagnostics;

[Conditional("DEBUG")]

public class TestAttribute : Attribute {}
```

<span data-ttu-id="1fddf-312">Soubor `class1.cs`:</span><span class="sxs-lookup"><span data-stu-id="1fddf-312">File `class1.cs`:</span></span>

```csharp
#define DEBUG

[Test]                // TestAttribute is specified

class Class1 {}
```

<span data-ttu-id="1fddf-313">Soubor `class2.cs`:</span><span class="sxs-lookup"><span data-stu-id="1fddf-313">File `class2.cs`:</span></span>

```csharp
#undef DEBUG

[Test]                 // TestAttribute is not specified

class Class2 {}
```

<span data-ttu-id="1fddf-314">třídy `Class1` a `Class2` jsou každý upravené pomocí atributu `Test`, což je podmíněné podle, jestli `DEBUG` je definována.</span><span class="sxs-lookup"><span data-stu-id="1fddf-314">the classes `Class1` and `Class2` are each decorated with attribute `Test`, which is conditional based on whether or not `DEBUG` is defined.</span></span> <span data-ttu-id="1fddf-315">Protože je tento symbol definovaný v rámci `Class1` , ale ne `Class2`, specifikaci `Test` atribut na `Class1` je zahrnuta při určení `Test` atribut na `Class2` je vynechán.</span><span class="sxs-lookup"><span data-stu-id="1fddf-315">Since this symbol is defined in the context of `Class1` but not `Class2`, the specification of the `Test` attribute on `Class1` is included, while the specification of the `Test` attribute on `Class2` is omitted.</span></span>

### <a name="the-obsolete-attribute"></a><span data-ttu-id="1fddf-316">Atribut zastaralé</span><span class="sxs-lookup"><span data-stu-id="1fddf-316">The Obsolete attribute</span></span>

<span data-ttu-id="1fddf-317">Atribut `Obsolete` slouží k označení typy a členy typů, které by se už nebude používat.</span><span class="sxs-lookup"><span data-stu-id="1fddf-317">The attribute `Obsolete` is used to mark types and members of types that should no longer be used.</span></span>

```csharp
namespace System
{
    [AttributeUsage(
        AttributeTargets.Class | 
        AttributeTargets.Struct |
        AttributeTargets.Enum | 
        AttributeTargets.Interface | 
        AttributeTargets.Delegate |
        AttributeTargets.Method | 
        AttributeTargets.Constructor |
        AttributeTargets.Property | 
        AttributeTargets.Field |
        AttributeTargets.Event,
        Inherited = false)
    ]
    public class ObsoleteAttribute: Attribute
    {
        public ObsoleteAttribute() {...}
        public ObsoleteAttribute(string message) {...}
        public ObsoleteAttribute(string message, bool error) {...}
        public string Message { get {...} }
        public bool IsError { get {...} }
    }
}
```

<span data-ttu-id="1fddf-318">Pokud program používá typ nebo člen, který je doplněn `Obsolete` atribut, kompilátor vyvolá upozornění nebo chybu.</span><span class="sxs-lookup"><span data-stu-id="1fddf-318">If a program uses a type or member that is decorated with the `Obsolete` attribute, the compiler issues a warning or an error.</span></span> <span data-ttu-id="1fddf-319">Konkrétně, kompilátor vyvolá upozornění-li zadán žádný parametr error, nebo pokud parametr error je k dispozici a má hodnotu `false`.</span><span class="sxs-lookup"><span data-stu-id="1fddf-319">Specifically, the compiler issues a warning if no error parameter is provided, or if the error parameter is provided and has the value `false`.</span></span> <span data-ttu-id="1fddf-320">Kompilátor vyvolá chybu, pokud je zadán parametr error a má hodnotu `true`.</span><span class="sxs-lookup"><span data-stu-id="1fddf-320">The compiler issues an error if the error parameter is specified and has the value `true`.</span></span>

<span data-ttu-id="1fddf-321">V příkladu</span><span class="sxs-lookup"><span data-stu-id="1fddf-321">In the example</span></span>

```csharp
[Obsolete("This class is obsolete; use class B instead")]
class A
{
    public void F() {}
}

class B
{
    public void F() {}
}

class Test
{
    static void Main() {
        A a = new A();         // Warning
        a.F();
    }
}
```

<span data-ttu-id="1fddf-322">Třída `A` je doplněn `Obsolete` atribut.</span><span class="sxs-lookup"><span data-stu-id="1fddf-322">the class `A` is decorated with the `Obsolete` attribute.</span></span> <span data-ttu-id="1fddf-323">Každé použití klíčového `A` v `Main` výsledky upozornění, která zahrnuje zadaná zpráva "Tato třída je zastaralá; Třída B místo toho použijte."</span><span class="sxs-lookup"><span data-stu-id="1fddf-323">Each use of `A` in `Main` results in a warning that includes the specified message, "This class is obsolete; use class B instead."</span></span>

### <a name="caller-info-attributes"></a><span data-ttu-id="1fddf-324">Atributy informace o volajícím</span><span class="sxs-lookup"><span data-stu-id="1fddf-324">Caller info attributes</span></span>

<span data-ttu-id="1fddf-325">Pro účely, jako je protokolování a vytváření sestav je někdy užitečné pro členské funkce určité kompilace informace o volajícím kódu.</span><span class="sxs-lookup"><span data-stu-id="1fddf-325">For purposes such as logging and reporting, it is sometimes useful for a function member to obtain certain compile-time information about the calling code.</span></span> <span data-ttu-id="1fddf-326">Atributy informace o volajícím poskytují způsob, jak transparentně předejte tyto informace.</span><span class="sxs-lookup"><span data-stu-id="1fddf-326">The caller info attributes provide a way to pass such information transparently.</span></span>

<span data-ttu-id="1fddf-327">Pokud volitelný parametr, je opatřen poznámkou jeden z atributů informace o volajícím, vynechání odpovídající argument ve volání nezpůsobí nutně výchozí hodnotu parametru nahrazena.</span><span class="sxs-lookup"><span data-stu-id="1fddf-327">When an optional parameter is annotated with one of the caller info attributes, omitting the corresponding argument in a call does not necessarily cause the default parameter value to be substituted.</span></span> <span data-ttu-id="1fddf-328">Místo toho pokud je zadaných informací o volání kontextu k dispozici, tyto informace se předá jako hodnotu argumentu.</span><span class="sxs-lookup"><span data-stu-id="1fddf-328">Instead, if the specified information about the calling context is available, that information will be passed as the argument value.</span></span>

<span data-ttu-id="1fddf-329">Příklad:</span><span class="sxs-lookup"><span data-stu-id="1fddf-329">For example:</span></span>

```csharp
using System.Runtime.CompilerServices

...

public void Log(
    [CallerLineNumber] int line = -1,
    [CallerFilePath]   string path = null,
    [CallerMemberName] string name = null
)
{
    Console.WriteLine((line < 0) ? "No line" : "Line "+ line);
    Console.WriteLine((path == null) ? "No file path" : path);
    Console.WriteLine((name == null) ? "No member name" : name);
}
```

<span data-ttu-id="1fddf-330">Volání `Log()` bez argumentů by tisk řádku číslo a cestu k souboru volání, stejně jako název člena, ve kterém došlo k volání.</span><span class="sxs-lookup"><span data-stu-id="1fddf-330">A call to `Log()` with no arguments would print the line number and file path of the call, as well as the name of the member within which the call occurred.</span></span>

<span data-ttu-id="1fddf-331">Atributy informace o volajícím se může vyskytnout u volitelné parametry kdekoli, včetně v deklaraci delegáta.</span><span class="sxs-lookup"><span data-stu-id="1fddf-331">Caller info attributes can occur on optional parameters anywhere, including in delegate declarations.</span></span> <span data-ttu-id="1fddf-332">Ale atributy informace o konkrétní volající mají omezení na typy parametrů, které se může atribut, tak, aby se vždy být proveden implicitní převod z nahrazenou hodnotu na typ parametru.</span><span class="sxs-lookup"><span data-stu-id="1fddf-332">However, the specific caller info attributes have restrictions on the types of the parameters they can attribute, so that there will always be an implicit conversion from a substituted value to the parameter type.</span></span>

<span data-ttu-id="1fddf-333">Jedná se o chybu v parametru definování a provádění části deklarace částečné metody nastavili stejný atribut informace o volajícím.</span><span class="sxs-lookup"><span data-stu-id="1fddf-333">It is an error to have the same caller info attribute on a parameter of both the defining and implementing part of a partial method declaration.</span></span> <span data-ttu-id="1fddf-334">Použijí se pouze atributy volající informace v části definující, vzhledem k tomu dochází pouze v části implementující atributy informace o volajícím jsou ignorovány.</span><span class="sxs-lookup"><span data-stu-id="1fddf-334">Only caller info attributes in the defining part are applied, whereas caller info attributes occurring only in the implementing part are ignored.</span></span>

<span data-ttu-id="1fddf-335">Informace o subjektu volajícím nemá vliv na řešení přetížení.</span><span class="sxs-lookup"><span data-stu-id="1fddf-335">Caller information does not affect overload resolution.</span></span> <span data-ttu-id="1fddf-336">Jak s atributy volitelné parametry jsou vynechány stále ze zdrojového kódu volajícího, rozlišení přetížení ignoruje tyto parametry stejně, jako je ignorován jiné vynechaný volitelné parametry ([rozlišení přetěžování](expressions.md#overload-resolution)).</span><span class="sxs-lookup"><span data-stu-id="1fddf-336">As the attributed optional parameters are still omitted from the source code of the caller, overload resolution ignores those parameters in the same way it ignores other omitted optional parameters ([Overload resolution](expressions.md#overload-resolution)).</span></span>

<span data-ttu-id="1fddf-337">Informace o subjektu volajícím nahrazuje pouze při vyvolání funkce explicitně ve zdrojovém kódu.</span><span class="sxs-lookup"><span data-stu-id="1fddf-337">Caller information is only substituted when a function is explicitly invoked in source code.</span></span> <span data-ttu-id="1fddf-338">Implicitní vyvolání například volání konstruktoru nadřazené implicitní nemají zdrojové umístění a nebude nahradit informace o volajícím.</span><span class="sxs-lookup"><span data-stu-id="1fddf-338">Implicit invocations such as implicit parent constructor calls do not have a source location and will not substitute caller information.</span></span> <span data-ttu-id="1fddf-339">Navíc nebudou volání, které jsou vázány dynamicky nahradit informace o volajícím.</span><span class="sxs-lookup"><span data-stu-id="1fddf-339">Also, calls that are dynamically bound will not substitute caller information.</span></span> <span data-ttu-id="1fddf-340">Informace o volajícím s atributy parametr se vynechá v takových případech, když se místo toho používá zadanou výchozí hodnotu parametru.</span><span class="sxs-lookup"><span data-stu-id="1fddf-340">When a caller info attributed parameter is omitted in such cases, the specified default value of the parameter is used instead.</span></span>

<span data-ttu-id="1fddf-341">Jedinou výjimkou je výrazy dotazu.</span><span class="sxs-lookup"><span data-stu-id="1fddf-341">One exception is query-expressions.</span></span> <span data-ttu-id="1fddf-342">Tyto adresy jsou považované za syntaktické rozšíření, a pokud volání, rozbalte Pokud chcete vynechat, nechte volitelné parametry s atributy informace o volajícím, informace o subjektu volajícím bude nahrazena.</span><span class="sxs-lookup"><span data-stu-id="1fddf-342">These are considered syntactic expansions, and if the calls they expand to omit optional parameters with caller info attributes, caller information will be substituted.</span></span> <span data-ttu-id="1fddf-343">Umístění používá je klauzule dotazu, který byl vytvořen volání.</span><span class="sxs-lookup"><span data-stu-id="1fddf-343">The location used is the location of the query clause which the call was generated from.</span></span>

<span data-ttu-id="1fddf-344">Pokud na daný parametr není zadán více než jeden atribut informace o volajícím, jsou upřednostňovány v následujícím pořadí: `CallerLineNumber`, `CallerFilePath`, `CallerMemberName`.</span><span class="sxs-lookup"><span data-stu-id="1fddf-344">If more than one caller info attribute is specified on a given parameter, they are preferred in the following order: `CallerLineNumber`, `CallerFilePath`, `CallerMemberName`.</span></span>

#### <a name="the-callerlinenumber-attribute"></a><span data-ttu-id="1fddf-345">Atribut CallerLineNumber</span><span class="sxs-lookup"><span data-stu-id="1fddf-345">The CallerLineNumber attribute</span></span>

<span data-ttu-id="1fddf-346">`System.Runtime.CompilerServices.CallerLineNumberAttribute` Může po standardní implicitní převod na volitelné parametry ([standardní implicitní převody](conversions.md#standard-implicit-conversions)) z konstantní hodnoty `int.MaxValue` na typ parametru.</span><span class="sxs-lookup"><span data-stu-id="1fddf-346">The `System.Runtime.CompilerServices.CallerLineNumberAttribute` is allowed on optional parameters when there is a standard implicit conversion ([Standard implicit conversions](conversions.md#standard-implicit-conversions)) from the constant value `int.MaxValue` to the parameter's type.</span></span> <span data-ttu-id="1fddf-347">Tím se zajistí, že jakékoli číslo řádku záporná až tuto hodnotu lze předat bez chyb.</span><span class="sxs-lookup"><span data-stu-id="1fddf-347">This ensures that any non-negative line number up to that value can be passed without error.</span></span>

<span data-ttu-id="1fddf-348">Pokud volitelný parametr se vynechá volání funkce z umístění ve zdrojovém kódu `CallerLineNumberAttribute`, pak číselný literál, který představuje číslo řádku v tomto umístění se používá jako argument k vyvolání místo výchozí hodnotu parametru.</span><span class="sxs-lookup"><span data-stu-id="1fddf-348">If a function invocation from a location in source code omits an optional parameter with the `CallerLineNumberAttribute`, then a numeric literal representing that location's line number is used as an argument to the invocation instead of the default parameter value.</span></span>

<span data-ttu-id="1fddf-349">V případě vyvolání pokrývá více řádků, řádek zvolili je závislý na implementaci.</span><span class="sxs-lookup"><span data-stu-id="1fddf-349">If the invocation spans multiple lines, the line chosen is implementation-dependent.</span></span>

<span data-ttu-id="1fddf-350">Všimněte si, že mohou být ovlivněny číslo řádku `#line` direktivy ([řádek direktivy](lexical-structure.md#line-directives)).</span><span class="sxs-lookup"><span data-stu-id="1fddf-350">Note that the line number may be affected by `#line` directives ([Line directives](lexical-structure.md#line-directives)).</span></span>

#### <a name="the-callerfilepath-attribute"></a><span data-ttu-id="1fddf-351">Atribut CallerFilePath</span><span class="sxs-lookup"><span data-stu-id="1fddf-351">The CallerFilePath attribute</span></span>

<span data-ttu-id="1fddf-352">`System.Runtime.CompilerServices.CallerFilePathAttribute` Může po standardní implicitní převod na volitelné parametry ([standardní implicitní převody](conversions.md#standard-implicit-conversions)) z `string` na typ parametru.</span><span class="sxs-lookup"><span data-stu-id="1fddf-352">The `System.Runtime.CompilerServices.CallerFilePathAttribute` is allowed on optional parameters when there is a standard implicit conversion ([Standard implicit conversions](conversions.md#standard-implicit-conversions)) from `string` to the parameter's type.</span></span>

<span data-ttu-id="1fddf-353">Pokud volitelný parametr se vynechá volání funkce z umístění ve zdrojovém kódu `CallerFilePathAttribute`, pak Textový literál představuje cestu k souboru tohoto umístění se používá jako argument k vyvolání místo výchozí hodnotu parametru.</span><span class="sxs-lookup"><span data-stu-id="1fddf-353">If a function invocation from a location in source code omits an optional parameter with the `CallerFilePathAttribute`, then a string literal representing that location's file path is used as an argument to the invocation instead of the default parameter value.</span></span>

<span data-ttu-id="1fddf-354">Formát cesty k souboru je závislý na implementaci.</span><span class="sxs-lookup"><span data-stu-id="1fddf-354">The format of the file path is implementation-dependent.</span></span>

<span data-ttu-id="1fddf-355">Všimněte si, že cesta k souboru může být ovlivněno `#line` direktivy ([řádek direktivy](lexical-structure.md#line-directives)).</span><span class="sxs-lookup"><span data-stu-id="1fddf-355">Note that the file path may be affected by `#line` directives ([Line directives](lexical-structure.md#line-directives)).</span></span>

#### <a name="the-callermembername-attribute"></a><span data-ttu-id="1fddf-356">Atribut CallerMemberName</span><span class="sxs-lookup"><span data-stu-id="1fddf-356">The CallerMemberName attribute</span></span>

<span data-ttu-id="1fddf-357">`System.Runtime.CompilerServices.CallerMemberNameAttribute` Může po standardní implicitní převod na volitelné parametry ([standardní implicitní převody](conversions.md#standard-implicit-conversions)) z `string` na typ parametru.</span><span class="sxs-lookup"><span data-stu-id="1fddf-357">The `System.Runtime.CompilerServices.CallerMemberNameAttribute` is allowed on optional parameters when there is a standard implicit conversion ([Standard implicit conversions](conversions.md#standard-implicit-conversions)) from `string` to the parameter's type.</span></span>

<span data-ttu-id="1fddf-358">Pokud volání funkce z umístění v rámci těla členské funkce nebo v rámci atributu použitý pro členské funkce samotný nebo jeho návratový typ, parametry nebo parametry typu v vynechá zdrojového kódu s volitelným parametrem `CallerMemberNameAttribute`, o řetězcový literál představující název, se kterou se používá jako argument k vyvolání místo výchozí hodnotu parametru.</span><span class="sxs-lookup"><span data-stu-id="1fddf-358">If a function invocation from a location within the body of a function member or within an attribute applied to the function member itself or its return type, parameters or type parameters in source code omits an optional parameter with the `CallerMemberNameAttribute`, then a string literal representing the name of that member is used as an argument to the invocation instead of the default parameter value.</span></span>

<span data-ttu-id="1fddf-359">Pro volání, ke kterým dochází v rámci obecných metod se používá pouze samotný název metody bez seznamu parametrů typu.</span><span class="sxs-lookup"><span data-stu-id="1fddf-359">For invocations that occur within generic methods, only the method name itself is used, without the type parameter list.</span></span>

<span data-ttu-id="1fddf-360">Pro volání, ke kterým dochází v rámci implementace explicitního rozhraní člen se používá pouze samotný název metody bez kvalifikace předchozí rozhraní.</span><span class="sxs-lookup"><span data-stu-id="1fddf-360">For invocations that occur within explicit interface member implementations, only the method name itself is used, without the preceding interface qualification.</span></span>

<span data-ttu-id="1fddf-361">Pro vyvolání, ke kterým dochází v rámci přistupující objekty vlastnosti nebo události je použít název člena, vlastnosti nebo samotné události.</span><span class="sxs-lookup"><span data-stu-id="1fddf-361">For invocations that occur within property or event accessors, the member name used is that of the property or event itself.</span></span>

<span data-ttu-id="1fddf-362">Pro volání, ke kterým dochází v rámci přístupových objektů indexer, použít název člena je dodaného `IndexerNameAttribute` ([atribut The IndexerName](attributes.md#the-indexername-attribute)) na člen indexer, pokud jsou k dispozici, nebo výchozí název `Item` jinak.</span><span class="sxs-lookup"><span data-stu-id="1fddf-362">For invocations that occur within indexer accessors, the member name used is that supplied by an `IndexerNameAttribute` ([The IndexerName attribute](attributes.md#the-indexername-attribute)) on the indexer member, if present, or the default name `Item` otherwise.</span></span>

<span data-ttu-id="1fddf-363">Název, který slouží pro vyvolání, ke kterým dochází v rámci deklarace konstruktory instancí, statické konstruktory, destruktory a operátory člen je závislý na implementaci.</span><span class="sxs-lookup"><span data-stu-id="1fddf-363">For invocations that occur within declarations of instance constructors, static constructors, destructors and operators the member name used is implementation-dependent.</span></span>

## <a name="attributes-for-interoperation"></a><span data-ttu-id="1fddf-364">Atributy pro spolupráci</span><span class="sxs-lookup"><span data-stu-id="1fddf-364">Attributes for Interoperation</span></span>

<span data-ttu-id="1fddf-365">Poznámka: Tato část se vztahuje pouze na implementaci rozhraní Microsoft .NET C#.</span><span class="sxs-lookup"><span data-stu-id="1fddf-365">Note: This section is applicable only to the Microsoft .NET implementation of C#.</span></span>

### <a name="interoperation-with-com-and-win32-components"></a><span data-ttu-id="1fddf-366">Vzájemná spolupráce s komponentami COM a Win32</span><span class="sxs-lookup"><span data-stu-id="1fddf-366">Interoperation with COM and Win32 components</span></span>

<span data-ttu-id="1fddf-367">.NET runtime obsahuje velký počet atributů, které programu povolit programy jazyka C# pro spolupráci s komponenty napsané s využitím modelu COM a knihovny DLL systému Win32.</span><span class="sxs-lookup"><span data-stu-id="1fddf-367">The .NET run-time provides a large number of attributes that enable C# programs to interoperate with components written using COM and Win32 DLLs.</span></span> <span data-ttu-id="1fddf-368">Například `DllImport` atribut lze použít na `static extern` indikace, že implementace metody se nachází v knihovně DLL systému Win32.</span><span class="sxs-lookup"><span data-stu-id="1fddf-368">For example, the `DllImport` attribute can be used on a `static extern` method to indicate that the implementation of the method is to be found in a Win32 DLL.</span></span> <span data-ttu-id="1fddf-369">Tyto atributy jsou součástí `System.Runtime.InteropServices` obor názvů a podrobnou dokumentaci pro tyto atributy se nachází v dokumentaci k modulu runtime .NET.</span><span class="sxs-lookup"><span data-stu-id="1fddf-369">These attributes are found in the `System.Runtime.InteropServices` namespace, and detailed documentation for these attributes is found in the .NET runtime documentation.</span></span>

### <a name="interoperation-with-other-net-languages"></a><span data-ttu-id="1fddf-370">Vzájemná spolupráce s jinými jazyky rozhraní .NET</span><span class="sxs-lookup"><span data-stu-id="1fddf-370">Interoperation with other .NET languages</span></span>

#### <a name="the-indexername-attribute"></a><span data-ttu-id="1fddf-371">Atribut IndexerName</span><span class="sxs-lookup"><span data-stu-id="1fddf-371">The IndexerName attribute</span></span>

<span data-ttu-id="1fddf-372">Indexery jsou implementovány v .NET pomocí indexované vlastnosti a mít název v metadatech .NET.</span><span class="sxs-lookup"><span data-stu-id="1fddf-372">Indexers are implemented in .NET using indexed properties, and have a name in the .NET metadata.</span></span> <span data-ttu-id="1fddf-373">Pokud ne `IndexerName` atribut je k dispozici pro indexer a pak název `Item` se používá ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="1fddf-373">If no `IndexerName` attribute is present for an indexer, then the name `Item` is used by default.</span></span> <span data-ttu-id="1fddf-374">`IndexerName` Atribut umožňuje vývojář, abyste mohli přepsat toto výchozí nastavení a zadejte jiný název.</span><span class="sxs-lookup"><span data-stu-id="1fddf-374">The `IndexerName` attribute enables a developer to override this default and specify a different name.</span></span>

```csharp
namespace System.Runtime.CompilerServices.CSharp
{
    [AttributeUsage(AttributeTargets.Property)]
    public class IndexerNameAttribute: Attribute
    {
        public IndexerNameAttribute(string indexerName) {...}
        public string Value { get {...} } 
    }
}
```
