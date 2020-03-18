---
ms.openlocfilehash: 25756c1811d5e6dc97512ce70f99ab7fefa91c4a
ms.sourcegitcommit: 2a6dffb60718065ece95df75e1cc7eb509e48a8d
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/01/2020
ms.locfileid: "79485236"
---
# <a name="records-work-in-progress"></a><span data-ttu-id="95fc6-101">Zaznamenává probíhající práci.</span><span class="sxs-lookup"><span data-stu-id="95fc6-101">Records Work-in-Progress</span></span>

<span data-ttu-id="95fc6-102">Na rozdíl od ostatních návrhů se nejedná o návrh sám o sobě, ale průběh práce navržený k zaznamenání rozhodnutí o návrhu konsensu pro funkci záznamů.</span><span class="sxs-lookup"><span data-stu-id="95fc6-102">Unlike the other records proposals, this is not a proposal in itself, but a work-in-progress designed to record consensus design decisions for the records feature.</span></span> <span data-ttu-id="95fc6-103">Podrobnosti specifikace budou přidány podle potřeby pro řešení otázek.</span><span class="sxs-lookup"><span data-stu-id="95fc6-103">Specification detail will be added as necessary to resolve questions.</span></span>

<span data-ttu-id="95fc6-104">Syntaxe pro záznam je navržena tak, aby se přidala takto:</span><span class="sxs-lookup"><span data-stu-id="95fc6-104">The syntax for a record is proposed to be added as follows:</span></span>

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

<span data-ttu-id="95fc6-105">`attributes` bez terminálu taky umožní nový kontextový atribut `data`.</span><span class="sxs-lookup"><span data-stu-id="95fc6-105">The `attributes` non-terminal will also permit a new contextual attribute, `data`.</span></span>

<span data-ttu-id="95fc6-106">Třída (struct) deklarovaná se seznamem parametrů nebo modifikátorem `data` se označuje jako třída záznamu (struktura záznamu), z nichž jeden je typ záznamu.</span><span class="sxs-lookup"><span data-stu-id="95fc6-106">A class (struct) declared with a parameter list or `data` modifier is called a record class (record struct), either of which is a record type.</span></span>

<span data-ttu-id="95fc6-107">Deklarace typu záznamu bez seznamu parametrů a modifikátoru `data` je chybná.</span><span class="sxs-lookup"><span data-stu-id="95fc6-107">It is an error to declare a record type without both a parameter list and the `data` modifier.</span></span>

## <a name="members-of-a-record-type"></a><span data-ttu-id="95fc6-108">Členové typu záznamu</span><span class="sxs-lookup"><span data-stu-id="95fc6-108">Members of a record type</span></span>

<span data-ttu-id="95fc6-109">Kromě členů deklarovaných v těle třídy má typ záznamu následující další členy:</span><span class="sxs-lookup"><span data-stu-id="95fc6-109">In addition to the members declared in the class-body, a record type has the following additional members:</span></span>

### <a name="primary-constructor"></a><span data-ttu-id="95fc6-110">Primární konstruktor</span><span class="sxs-lookup"><span data-stu-id="95fc6-110">Primary Constructor</span></span>

<span data-ttu-id="95fc6-111">Typ záznamu má veřejný konstruktor, jehož signatura odpovídá parametrům hodnoty deklarace typu.</span><span class="sxs-lookup"><span data-stu-id="95fc6-111">A record type has a public constructor whose signature corresponds to the value parameters of the type declaration.</span></span> <span data-ttu-id="95fc6-112">Označuje se jako primární konstruktor pro daný typ a způsobí, že implicitně deklarovaný výchozí konstruktor bude potlačen.</span><span class="sxs-lookup"><span data-stu-id="95fc6-112">This is called the primary constructor for the type, and causes the implicitly declared default constructor to be suppressed.</span></span> <span data-ttu-id="95fc6-113">Jedná se o chybu pro primární konstruktor a konstruktor se stejným podpisem již ve třídě existuje.</span><span class="sxs-lookup"><span data-stu-id="95fc6-113">It is an error to have a primary constructor and a constructor with the same signature already present in the class.</span></span>
<span data-ttu-id="95fc6-114">Za běhu primárního konstruktoru</span><span class="sxs-lookup"><span data-stu-id="95fc6-114">At runtime the primary constructor</span></span> 

1. <span data-ttu-id="95fc6-115">spustí Inicializátory pole instance uvedené v těle třídy; a potom vyvolá konstruktor základní třídy bez argumentů.</span><span class="sxs-lookup"><span data-stu-id="95fc6-115">executes the instance field initializers appearing in the class-body; and then  invokes the base class constructor with no arguments.</span></span>

1. <span data-ttu-id="95fc6-116">Inicializuje pole zálohování generované kompilátorem pro vlastnosti odpovídající parametrům hodnoty (pokud jsou tyto vlastnosti zadány kompilátorem, viz [syntetizované vlastnosti](#Synthesized Properties))</span><span class="sxs-lookup"><span data-stu-id="95fc6-116">initializes compiler-generated backing fields for the properties corresponding to the value parameters (if these properties are compiler-provided; see [Synthesized properties](#Synthesized Properties))</span></span>


<span data-ttu-id="95fc6-117">[] TODO: přidat syntaxi a specifikaci základního volání pro výběr základního konstruktoru prostřednictvím řešení přetížení</span><span class="sxs-lookup"><span data-stu-id="95fc6-117">[ ] TODO: add base call syntax and specification about choosing base constructor through overload resolution</span></span>

### <a name="properties"></a><span data-ttu-id="95fc6-118">Vlastnosti</span><span class="sxs-lookup"><span data-stu-id="95fc6-118">Properties</span></span>

<span data-ttu-id="95fc6-119">Pro každý parametr záznamu deklarace typu záznamu je k dispozici odpovídající člen veřejné vlastnosti, jehož název a typ jsou přijímány z deklarace parametru value.</span><span class="sxs-lookup"><span data-stu-id="95fc6-119">For each record parameter of a record type declaration there is a corresponding public property member whose name and type are taken from the value parameter declaration.</span></span> <span data-ttu-id="95fc6-120">Pokud žádná konkrétní (tj. neabstraktní) vlastnost s přístupovým objektem Get a s tímto názvem a typem je explicitně deklarována nebo zděděna, je vytvořena kompilátorem následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="95fc6-120">If no concrete (i.e. non-abstract) property with a get accessor and with this name and type is explicitly declared or inherited, it is produced by the compiler as follows:</span></span>

<span data-ttu-id="95fc6-121">Pro strukturu záznamu nebo třídu záznamu:</span><span class="sxs-lookup"><span data-stu-id="95fc6-121">For a record struct or a record class:</span></span>

* <span data-ttu-id="95fc6-122">Je vytvořena veřejná vlastnost pouze pro získání.</span><span class="sxs-lookup"><span data-stu-id="95fc6-122">A public get-only auto-property is created.</span></span> <span data-ttu-id="95fc6-123">Jeho hodnota je inicializována během konstrukce s hodnotou odpovídajícího primárního parametru konstruktoru.</span><span class="sxs-lookup"><span data-stu-id="95fc6-123">Its value is initialized during construction with the value of the corresponding primary constructor parameter.</span></span> <span data-ttu-id="95fc6-124">Všechna "párové" zděděná přístupová metoda get abstraktních vlastností je přepsána.</span><span class="sxs-lookup"><span data-stu-id="95fc6-124">Each "matching" inherited abstract property's get accessor is overridden.</span></span>

### <a name="equality-members"></a><span data-ttu-id="95fc6-125">Členové rovnosti</span><span class="sxs-lookup"><span data-stu-id="95fc6-125">Equality members</span></span>

<span data-ttu-id="95fc6-126">Typy záznamů poskytují syntetizované implementace následujících metod:</span><span class="sxs-lookup"><span data-stu-id="95fc6-126">Record types produce synthesized implementations for the following methods:</span></span>

* <span data-ttu-id="95fc6-127">`object.GetHashCode()` přepsat, pokud není zapečetěné nebo zadáno uživatelem</span><span class="sxs-lookup"><span data-stu-id="95fc6-127">`object.GetHashCode()` override, unless it is sealed or user provided</span></span>
* <span data-ttu-id="95fc6-128">`object.Equals(object)` přepsat, pokud není zapečetěné nebo zadáno uživatelem</span><span class="sxs-lookup"><span data-stu-id="95fc6-128">`object.Equals(object)` override, unless it is sealed or user provided</span></span>
* <span data-ttu-id="95fc6-129">`T Equals(T)` metoda, kde `T` je aktuální typ</span><span class="sxs-lookup"><span data-stu-id="95fc6-129">`T Equals(T)` method, where `T` is the current type</span></span>

<span data-ttu-id="95fc6-130">`T Equals(T)` je určen k provádění rovnosti hodnot, porovnává vlastnost se stejným názvem jako každý parametr primárního konstruktoru s odpovídající vlastností druhého typu.</span><span class="sxs-lookup"><span data-stu-id="95fc6-130">`T Equals(T)` is specified to perform value equality, comparing the property with same name as each primary constructor parameter to the corresponding property of the other type.</span></span>
<span data-ttu-id="95fc6-131">`object.Equals` provádí ekvivalent</span><span class="sxs-lookup"><span data-stu-id="95fc6-131">`object.Equals` performs the equivalent of</span></span>

```C#
override Equals(object o) => Equals(o as T);
```
