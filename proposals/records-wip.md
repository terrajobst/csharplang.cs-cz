---
ms.openlocfilehash: 258ae6865c5b2c3103a0cdf7e1e5a2cdee11e740
ms.sourcegitcommit: 1e1c7c72b156e2fbc54d6d6ac8d21bca9934d8d2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/26/2020
ms.locfileid: "80281954"
---
# <a name="records-work-in-progress"></a><span data-ttu-id="965ca-101">Zaznamenává probíhající práci.</span><span class="sxs-lookup"><span data-stu-id="965ca-101">Records Work-in-Progress</span></span>

<span data-ttu-id="965ca-102">Na rozdíl od ostatních návrhů se nejedná o návrh sám o sobě, ale průběh práce navržený k zaznamenání rozhodnutí o návrhu konsensu pro funkci záznamů.</span><span class="sxs-lookup"><span data-stu-id="965ca-102">Unlike the other records proposals, this is not a proposal in itself, but a work-in-progress designed to record consensus design decisions for the records feature.</span></span> <span data-ttu-id="965ca-103">Podrobnosti specifikace budou přidány podle potřeby pro řešení otázek.</span><span class="sxs-lookup"><span data-stu-id="965ca-103">Specification detail will be added as necessary to resolve questions.</span></span>

<span data-ttu-id="965ca-104">Syntaxe pro záznam je navržena tak, aby se přidala takto:</span><span class="sxs-lookup"><span data-stu-id="965ca-104">The syntax for a record is proposed to be added as follows:</span></span>

```antlr
class_declaration
    : attributes? class_modifiers? 'partial'? 'class' identifier type_parameter_list?
      parameter_list? type_parameter_constraints_clauses? class_body
    ;

struct_declaration
    : attributes? struct_modifiers? 'partial'? 'struct' identifier type_parameter_list?
      parameter_list? struct_interfaces? type_parameter_constraints_clauses? struct_body
    ;

class_body
    : '{' class_member_declarations? '}'
    | ';'
    ;

struct_body
    : '{' struct_members_declarations? '}'
    | ';'
    ;
```

<span data-ttu-id="965ca-105">`attributes` bez terminálu taky umožní nový kontextový atribut `data`.</span><span class="sxs-lookup"><span data-stu-id="965ca-105">The `attributes` non-terminal will also permit a new contextual attribute, `data`.</span></span>

<span data-ttu-id="965ca-106">Třída (struct) deklarovaná se seznamem parametrů nebo modifikátorem `data` se označuje jako třída záznamu (struktura záznamu), z nichž jeden je typ záznamu.</span><span class="sxs-lookup"><span data-stu-id="965ca-106">A class (struct) declared with a parameter list or `data` modifier is called a record class (record struct), either of which is a record type.</span></span>

<span data-ttu-id="965ca-107">Deklarace typu záznamu bez seznamu parametrů a modifikátoru `data` je chybná.</span><span class="sxs-lookup"><span data-stu-id="965ca-107">It is an error to declare a record type without both a parameter list and the `data` modifier.</span></span>

## <a name="members-of-a-record-type"></a><span data-ttu-id="965ca-108">Členové typu záznamu</span><span class="sxs-lookup"><span data-stu-id="965ca-108">Members of a record type</span></span>

<span data-ttu-id="965ca-109">Kromě členů deklarovaných v těle třídy má typ záznamu následující další členy:</span><span class="sxs-lookup"><span data-stu-id="965ca-109">In addition to the members declared in the class-body, a record type has the following additional members:</span></span>

### <a name="primary-constructor"></a><span data-ttu-id="965ca-110">Primární konstruktor</span><span class="sxs-lookup"><span data-stu-id="965ca-110">Primary Constructor</span></span>

<span data-ttu-id="965ca-111">Typ záznamu má veřejný konstruktor, jehož signatura odpovídá parametrům hodnoty deklarace typu.</span><span class="sxs-lookup"><span data-stu-id="965ca-111">A record type has a public constructor whose signature corresponds to the value parameters of the type declaration.</span></span> <span data-ttu-id="965ca-112">Označuje se jako primární konstruktor pro daný typ a způsobí, že implicitně deklarovaný výchozí konstruktor bude potlačen.</span><span class="sxs-lookup"><span data-stu-id="965ca-112">This is called the primary constructor for the type, and causes the implicitly declared default constructor to be suppressed.</span></span> <span data-ttu-id="965ca-113">Jedná se o chybu pro primární konstruktor a konstruktor se stejným podpisem již ve třídě existuje.</span><span class="sxs-lookup"><span data-stu-id="965ca-113">It is an error to have a primary constructor and a constructor with the same signature already present in the class.</span></span>
<span data-ttu-id="965ca-114">Za běhu primárního konstruktoru</span><span class="sxs-lookup"><span data-stu-id="965ca-114">At runtime the primary constructor</span></span> 

1. <span data-ttu-id="965ca-115">spustí Inicializátory pole instance uvedené v těle třídy; a potom vyvolá konstruktor základní třídy bez argumentů.</span><span class="sxs-lookup"><span data-stu-id="965ca-115">executes the instance field initializers appearing in the class-body; and then  invokes the base class constructor with no arguments.</span></span>

1. <span data-ttu-id="965ca-116">Inicializuje pole zálohování generované kompilátorem pro vlastnosti odpovídající parametrům hodnoty (pokud jsou tyto vlastnosti zadány kompilátorem, viz [syntetizované vlastnosti](#Synthesized Properties))</span><span class="sxs-lookup"><span data-stu-id="965ca-116">initializes compiler-generated backing fields for the properties corresponding to the value parameters (if these properties are compiler-provided; see [Synthesized properties](#Synthesized Properties))</span></span>


<span data-ttu-id="965ca-117">[] TODO: přidat syntaxi a specifikaci základního volání pro výběr základního konstruktoru prostřednictvím řešení přetížení</span><span class="sxs-lookup"><span data-stu-id="965ca-117">[ ] TODO: add base call syntax and specification about choosing base constructor through overload resolution</span></span>

### <a name="properties"></a><span data-ttu-id="965ca-118">Vlastnosti</span><span class="sxs-lookup"><span data-stu-id="965ca-118">Properties</span></span>

<span data-ttu-id="965ca-119">Pro každý parametr záznamu deklarace typu záznamu je k dispozici odpovídající člen veřejné vlastnosti, jehož název a typ jsou přijímány z deklarace parametru value.</span><span class="sxs-lookup"><span data-stu-id="965ca-119">For each record parameter of a record type declaration there is a corresponding public property member whose name and type are taken from the value parameter declaration.</span></span> <span data-ttu-id="965ca-120">Pokud žádná konkrétní (tj. neabstraktní) vlastnost s přístupovým objektem Get a s tímto názvem a typem je explicitně deklarována nebo zděděna, je vytvořena kompilátorem následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="965ca-120">If no concrete (i.e. non-abstract) property with a get accessor and with this name and type is explicitly declared or inherited, it is produced by the compiler as follows:</span></span>

<span data-ttu-id="965ca-121">Pro strukturu záznamu nebo třídu záznamu:</span><span class="sxs-lookup"><span data-stu-id="965ca-121">For a record struct or a record class:</span></span>

* <span data-ttu-id="965ca-122">Je vytvořena veřejná vlastnost pouze pro získání.</span><span class="sxs-lookup"><span data-stu-id="965ca-122">A public get-only auto-property is created.</span></span> <span data-ttu-id="965ca-123">Jeho hodnota je inicializována během konstrukce s hodnotou odpovídajícího primárního parametru konstruktoru.</span><span class="sxs-lookup"><span data-stu-id="965ca-123">Its value is initialized during construction with the value of the corresponding primary constructor parameter.</span></span> <span data-ttu-id="965ca-124">Všechna "párové" zděděná přístupová metoda get abstraktních vlastností je přepsána.</span><span class="sxs-lookup"><span data-stu-id="965ca-124">Each "matching" inherited abstract property's get accessor is overridden.</span></span>

### <a name="equality-members"></a><span data-ttu-id="965ca-125">Členové rovnosti</span><span class="sxs-lookup"><span data-stu-id="965ca-125">Equality members</span></span>

<span data-ttu-id="965ca-126">Typy záznamů poskytují syntetizované implementace následujících metod:</span><span class="sxs-lookup"><span data-stu-id="965ca-126">Record types produce synthesized implementations for the following methods:</span></span>

* <span data-ttu-id="965ca-127">`object.GetHashCode()` přepsat, pokud není zapečetěné nebo zadáno uživatelem</span><span class="sxs-lookup"><span data-stu-id="965ca-127">`object.GetHashCode()` override, unless it is sealed or user provided</span></span>
* <span data-ttu-id="965ca-128">`object.Equals(object)` přepsat, pokud není zapečetěné nebo zadáno uživatelem</span><span class="sxs-lookup"><span data-stu-id="965ca-128">`object.Equals(object)` override, unless it is sealed or user provided</span></span>
* <span data-ttu-id="965ca-129">`T Equals(T)` metoda, kde `T` je aktuální typ</span><span class="sxs-lookup"><span data-stu-id="965ca-129">`T Equals(T)` method, where `T` is the current type</span></span>

<span data-ttu-id="965ca-130">`T Equals(T)` je určen k provádění rovnosti hodnot, porovnává vlastnost se stejným názvem jako každý parametr primárního konstruktoru s odpovídající vlastností druhého typu.</span><span class="sxs-lookup"><span data-stu-id="965ca-130">`T Equals(T)` is specified to perform value equality, comparing the property with same name as each primary constructor parameter to the corresponding property of the other type.</span></span>
<span data-ttu-id="965ca-131">`object.Equals` provádí ekvivalent</span><span class="sxs-lookup"><span data-stu-id="965ca-131">`object.Equals` performs the equivalent of</span></span>

```C#
override Equals(object o) => Equals(o as T);
```

## <a name="with-expression"></a><span data-ttu-id="965ca-132">výraz `with`</span><span class="sxs-lookup"><span data-stu-id="965ca-132">`with` expression</span></span>

<span data-ttu-id="965ca-133">Výraz `with` je nový výraz pomocí následující syntaxe.</span><span class="sxs-lookup"><span data-stu-id="965ca-133">A `with` expression is a new expression using the following syntax.</span></span>

```antlr
with_expression
    : switch_expression
    | switch_expression 'with' anonymous_object_initializer
```

<span data-ttu-id="965ca-134">Výraz `with` umožňuje "nedestruktivní mutace", navržený tak, aby vytvořil kopii výrazu příjemce se změnami vlastností, které jsou uvedeny v `anonymous_object_initializer`.</span><span class="sxs-lookup"><span data-stu-id="965ca-134">A `with` expression allows for "non-destructive mutation", designed to produce a copy of the receiver expression with modifications to properties listed in the `anonymous_object_initializer`.</span></span>

<span data-ttu-id="965ca-135">Platný výraz `with` má přijímač s typem, který není typu void.</span><span class="sxs-lookup"><span data-stu-id="965ca-135">A valid `with` expression has a receiver with a non-void type.</span></span> <span data-ttu-id="965ca-136">Typ přijímače musí obsahovat přístupnou metodu instance nazvanou `With` s příslušnými parametry a návratovým typem.</span><span class="sxs-lookup"><span data-stu-id="965ca-136">The receiver type must contain an accessible instance method called `With` with the appropriate parameters and return type.</span></span> <span data-ttu-id="965ca-137">Jedná se o chybu, pokud existuje více metod `With` bez přepsání.</span><span class="sxs-lookup"><span data-stu-id="965ca-137">It is an error if there are multiple non-override `With` methods.</span></span> <span data-ttu-id="965ca-138">Pokud existuje více `With` přepsání, musí existovat nepřepisující `With` metoda, která je cílovou metodou.</span><span class="sxs-lookup"><span data-stu-id="965ca-138">If there are multiple `With` overrides, there must be a non-override `With` method, which is the target method.</span></span> <span data-ttu-id="965ca-139">V opačném případě musí být k dispozici právě jedna metoda `With`.</span><span class="sxs-lookup"><span data-stu-id="965ca-139">Otherwise, there must be exactly one `With` method.</span></span>

<span data-ttu-id="965ca-140">Na pravé straně `with` výrazu je `anonymous_object_initializer` se sekvencí přiřazení s polem nebo vlastností přijímače na levé straně přiřazení a libovolným výrazem na pravé straně, který se implicitně předává na typ na levé straně.</span><span class="sxs-lookup"><span data-stu-id="965ca-140">On the right hand side of the `with` expression is an `anonymous_object_initializer` with a sequence of assignments with a field or property of the receiver on the left-hand side of the assignment, and an arbitrary expression on the right-hand side which is implicitly convertible to the type of the left-hand side.</span></span>

<span data-ttu-id="965ca-141">S ohledem na cílovou metodu `With` musí být návratový typ typu typu výrazu příjemce nebo jeho základního typu.</span><span class="sxs-lookup"><span data-stu-id="965ca-141">Given a target `With` method, the return type must be the type of the receiver expression type, or a base type thereof.</span></span> <span data-ttu-id="965ca-142">Pro každý parametr metody `With` musí být k dispozici odpovídající pole instance nebo vlastnost čitelný na typu příjemce se stejným názvem a stejným typem.</span><span class="sxs-lookup"><span data-stu-id="965ca-142">For each parameter of the `With` method, there must be an accessible corresponding instance field or readable property on the receiver type with the same name and the same type.</span></span> <span data-ttu-id="965ca-143">Každá vlastnost nebo pole na pravé straně výrazu with musí také odpovídat parametru se stejným názvem v metodě `With`.</span><span class="sxs-lookup"><span data-stu-id="965ca-143">Each property or field in the right-hand side of the With expression must also correspond to a parameter of the same name in the `With` method.</span></span>

<span data-ttu-id="965ca-144">S ohledem na platnou metodu `With` je vyhodnocení výrazu `with` ekvivalentní volání metody `With` s výrazy v `anonymous_object_initializer` nahrazeny parametrem se stejným názvem jako vlastnost na levé straně.</span><span class="sxs-lookup"><span data-stu-id="965ca-144">Given a valid `With` method, the evaluation of a `with` expression is equivalent to calling the `With` method with the expressions in the `anonymous_object_initializer` substituted for the parameter of the same name as the property on the left hand side.</span></span> <span data-ttu-id="965ca-145">Pokud neexistuje žádná vyhovující vlastnost pro daný parametr v `anonymous_object_initializer`, je argumentem vyhodnoceno pole nebo vlastnost se stejným názvem na přijímači.</span><span class="sxs-lookup"><span data-stu-id="965ca-145">If there is no matching property for a given parameter in the `anonymous_object_initializer`, the argument is the evaluation of the field or property of the same name on the receiver.</span></span>

<span data-ttu-id="965ca-146">Pořadí vyhodnocení vedlejších účinků je následující, přičemž každý výraz se vyhodnocuje přesně jednou:</span><span class="sxs-lookup"><span data-stu-id="965ca-146">The order of evaluation of side effects is as follows, with each expression evaluated exactly once:</span></span>

1. <span data-ttu-id="965ca-147">Výraz přijímače</span><span class="sxs-lookup"><span data-stu-id="965ca-147">Receiver expression</span></span>

2. <span data-ttu-id="965ca-148">Výrazy v `anonymous_object_initializer`v lexikálním pořadí</span><span class="sxs-lookup"><span data-stu-id="965ca-148">Expressions in the `anonymous_object_initializer`, in lexical order</span></span>

3. <span data-ttu-id="965ca-149">Vyhodnocení jakýchkoli vlastností, které odpovídají parametrům `With` metody, v pořadí podle definice parametrů `With` metody.</span><span class="sxs-lookup"><span data-stu-id="965ca-149">The evaluation of any properties matching the `With` method parameters, in order of definition of the `With` method parameters.</span></span>