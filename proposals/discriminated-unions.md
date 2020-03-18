---
ms.openlocfilehash: 2e4a3a32def6900797c151264c984378b09b4988
ms.sourcegitcommit: 5983461e05be62f39c77383cb7857539518cb04f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/15/2019
ms.locfileid: "79485124"
---

# <a name="discriminated-unions--enum-class"></a><span data-ttu-id="8d984-101">Rozlišené sjednocení/`enum class`</span><span class="sxs-lookup"><span data-stu-id="8d984-101">Discriminated unions / `enum class`</span></span>

<span data-ttu-id="8d984-102">`enum class`ES je nový druh deklarace typu, který se někdy označuje jako rozlišené sjednocení, kde každá z nich má daný typ a každá instance se nepřekrývá.</span><span class="sxs-lookup"><span data-stu-id="8d984-102">`enum class`es are a new kind of type declaration, sometimes referred to as discriminated unions, where each every possible instance the type is listed, and each instance is non-overlapping.</span></span>

<span data-ttu-id="8d984-103">`enum class` je definována pomocí následující syntaxe:</span><span class="sxs-lookup"><span data-stu-id="8d984-103">An `enum class` is defined using the following syntax:</span></span>

```antlr
enum_class
    : 'partial'? 'enum class' identifier type_parameter_list? type_parameter_constraints_clause* 
      '{' enum_class_body '}'
    ;

enum_class_body
    : enum_class_cases?
    | enum_class_cases ','
    ;

enum_class_cases
    : enum_class_case
    | enum_class_case ',' enum_class_cases
    ;

enum_class_case
    : enum_class
    | class_declaration
    | identifier type_parameter_list? '(' formal_parameter_list? ')'
    | identifier
    ;

```

<span data-ttu-id="8d984-104">Ukázková syntaxe:</span><span class="sxs-lookup"><span data-stu-id="8d984-104">Sample syntax:</span></span>

```C#
enum class Shape
{
    Rectangle(float Width, float Length),
    Circle(float Radius),
}
```

## <a name="semantics"></a><span data-ttu-id="8d984-105">Sémantiku</span><span class="sxs-lookup"><span data-stu-id="8d984-105">Semantics</span></span>

<span data-ttu-id="8d984-106">Definice `enum class` definuje kořenový typ, což je abstraktní třída se stejným názvem jako deklarace `enum class` a sada členů, z nichž každý má typ, který je podtypem kořenového typu.</span><span class="sxs-lookup"><span data-stu-id="8d984-106">An `enum class` definition defines a root type, which is an abstract class of the same name as the `enum class` declaration, and a set of members, each of which has a type which is a subtype of the root type.</span></span> <span data-ttu-id="8d984-107">Pokud existuje více definic `partial enum class`, budou všichni členové považováni za členy definice třídy výčtu.</span><span class="sxs-lookup"><span data-stu-id="8d984-107">If there are multiple `partial enum class` definitions, all members will be considered members of the enum class definition.</span></span> <span data-ttu-id="8d984-108">Na rozdíl od uživatelsky definované definice abstraktní třídy je typ kořene `enum class` ve výchozím nastavení částečný a definuje tak, aby měl výchozí *soukromý* konstruktor bez parametrů.</span><span class="sxs-lookup"><span data-stu-id="8d984-108">Unlike a user-defined abstract class definition, the `enum class` root type is partial by default and defined to have a default *private* parameter-less constructor.</span></span>

<span data-ttu-id="8d984-109">Všimněte si, že vzhledem k tomu, že je kořenový typ definován jako částečná abstraktní třída, mohou být přidány i částečné definice *kořenového typu* , kde jsou povoleny standardní formáty syntaxe pro tělo třídy.</span><span class="sxs-lookup"><span data-stu-id="8d984-109">Note that, since the root type is defined to be a partial abstract class, partial definitions of the *root type* may also be added, where standard syntax forms for a class body are allowed.</span></span>
<span data-ttu-id="8d984-110">Nicméně žádné typy nemůžou přímo dědit z kořenového typu v žádné deklaraci, kromě těch, které jsou zadané jako `enum class` členy.</span><span class="sxs-lookup"><span data-stu-id="8d984-110">However, no types may directly inherit from the root type in any declaration, aside from those specified as `enum class` members.</span></span> <span data-ttu-id="8d984-111">Kromě toho nejsou pro kořenový typ povoleny žádné uživatelsky definované konstruktory.</span><span class="sxs-lookup"><span data-stu-id="8d984-111">In addition, no user-defined constructors are permitted for the root type.</span></span>

<span data-ttu-id="8d984-112">Existují čtyři typy deklarací `enum class` členů:</span><span class="sxs-lookup"><span data-stu-id="8d984-112">There are four kinds of `enum class` member declarations:</span></span>

* <span data-ttu-id="8d984-113">Jednoduché členy třídy</span><span class="sxs-lookup"><span data-stu-id="8d984-113">simple class members</span></span>

* <span data-ttu-id="8d984-114">komplexní členové třídy</span><span class="sxs-lookup"><span data-stu-id="8d984-114">complex class members</span></span>

* <span data-ttu-id="8d984-115">`enum class` členů</span><span class="sxs-lookup"><span data-stu-id="8d984-115">`enum class` members</span></span>

* <span data-ttu-id="8d984-116">členy hodnoty.</span><span class="sxs-lookup"><span data-stu-id="8d984-116">value members.</span></span>

### <a name="simple-class-members"></a><span data-ttu-id="8d984-117">Jednoduché členy třídy</span><span class="sxs-lookup"><span data-stu-id="8d984-117">Simple class members</span></span>

<span data-ttu-id="8d984-118">Jednoduchá deklarace člena třídy definuje novou vnořenou třídu "Record" (úmyslně nedefinovanou v tomto dokumentu) se stejným názvem.</span><span class="sxs-lookup"><span data-stu-id="8d984-118">A simple class member declaration defines a new nested "record" class (intentionally left undefined in this document) with the same name.</span></span> <span data-ttu-id="8d984-119">Vnořená třída dědí z kořenového typu.</span><span class="sxs-lookup"><span data-stu-id="8d984-119">The nested class inherits from the root type.</span></span>

<span data-ttu-id="8d984-120">Dle výše uvedeného ukázkového kódu,</span><span class="sxs-lookup"><span data-stu-id="8d984-120">Given the sample code above,</span></span>

```C#
enum class Shape
{
    Rectangle(float Width, float Length),
    Circle(float Radius)
}
```

<span data-ttu-id="8d984-121">deklarace `enum class` má sémantiku ekvivalentní následující deklaraci.</span><span class="sxs-lookup"><span data-stu-id="8d984-121">the `enum class` declaration has semantics equivalent to the following declaration</span></span>

```C#
partial abstract class Shape
{
    public data class Rectangle(float Width, float Length) : Shape,
    public data class Circle(float Radius) : Shape
}
```

### <a name="complex-class-members"></a><span data-ttu-id="8d984-122">Komplexní členové třídy</span><span class="sxs-lookup"><span data-stu-id="8d984-122">Complex class members</span></span>

<span data-ttu-id="8d984-123">Celou deklaraci třídy můžete také vnořit pod `enum class` deklaraci.</span><span class="sxs-lookup"><span data-stu-id="8d984-123">You can also nest an entire class declaration below an `enum class` declaration.</span></span> <span data-ttu-id="8d984-124">Bude považována za vnořenou třídu kořenového typu.</span><span class="sxs-lookup"><span data-stu-id="8d984-124">It will be treated as a nested class of the root type.</span></span> <span data-ttu-id="8d984-125">Syntaxe umožňuje jakékoli deklarace třídy, ale je vyžadována pro komplexní člen třídy, aby dědil z deklarace přímo obsahující `enum class`.</span><span class="sxs-lookup"><span data-stu-id="8d984-125">The syntax allows any class declaration, but it is required for the complex class member to inherit from the direct containing `enum class` declaration.</span></span> 

### <a name="enum-class-members"></a><span data-ttu-id="8d984-126">`enum class` členů</span><span class="sxs-lookup"><span data-stu-id="8d984-126">`enum class` members</span></span>

<span data-ttu-id="8d984-127">`enum classes` může být vnořen do sebe navzájem, např.</span><span class="sxs-lookup"><span data-stu-id="8d984-127">`enum classes` can be nested under each other, e.g.</span></span>

```C#
enum class Expr
{
    enum class Binary
    {
        Addition(Expr left, Expr right),
        Multiplication(Expr left, Expr right)
    }
}
```

<span data-ttu-id="8d984-128">To je téměř totožné s sémantikou `enum class`nejvyšší úrovně, s tím rozdílem, že vnořená třída výčtu definuje vnořený kořenový typ a vše pod vnořenou třídou výčtu je podtypem vnořeného typu root, namísto kořenového typu nejvyšší úrovně.</span><span class="sxs-lookup"><span data-stu-id="8d984-128">This is almost identical to the semantics of a top-level `enum class`, except that the nested enum class defines a nested root type, and everything below the nested enum class is a subtype of the nested root type, instead of the top-level root type.</span></span>

```C#
partial abstract class Expr
{
    partial abstract class Binary : Expr
    {
        public data class Addition(Expr left, Expr right) : Binary,
        public data class Multiplication(Expr left, Expr right) : Binary
    }
}
```

### <a name="value-members"></a><span data-ttu-id="8d984-129">Členové hodnoty</span><span class="sxs-lookup"><span data-stu-id="8d984-129">Value members</span></span>

<span data-ttu-id="8d984-130">`enum classes` mohou obsahovat také členy hodnot.</span><span class="sxs-lookup"><span data-stu-id="8d984-130">`enum classes` can also contain value members.</span></span> <span data-ttu-id="8d984-131">Členové hodnoty definují statické vlastnosti pouze pro získání na kořenovém typu, které vracejí také typ root, např.</span><span class="sxs-lookup"><span data-stu-id="8d984-131">Value members define public get-only static properties on the root type that also return the root type, e.g.</span></span>

```C#
enum class Color
{
    Red,
    Green
}
```

<span data-ttu-id="8d984-132">má vlastnosti, které jsou ekvivalentní</span><span class="sxs-lookup"><span data-stu-id="8d984-132">has properties equivalent to</span></span>

```C#
partial abstract class Color
{
    public static Color Red => ...;
    public static Color Green => ...;
}
```

<span data-ttu-id="8d984-133">Kompletní sémantika se považuje za podrobnosti implementace, ale je zaručeno, že se pro každou vlastnost vrátí jedna jedinečná instance a stejná instance se vrátí při opakovaném vyvolání.</span><span class="sxs-lookup"><span data-stu-id="8d984-133">The complete semantics are considered an implementation detail, but it is guaranteed that one unique instance will be returned for each property, and the same instance will be returned on repeated invocations.</span></span>


### <a name="switch-expression-and-patterns"></a><span data-ttu-id="8d984-134">Přepnout výraz a vzory</span><span class="sxs-lookup"><span data-stu-id="8d984-134">Switch expression and patterns</span></span>

<span data-ttu-id="8d984-135">Existují některé navržené úpravy pro porovnávání vzorů a výraz přepínače pro zpracování `enum classes`.</span><span class="sxs-lookup"><span data-stu-id="8d984-135">There are some proposed adjustments to pattern matching and the switch expression to handle `enum classes`.</span></span> <span data-ttu-id="8d984-136">Výrazy Switch mohou již odpovídat typům pomocí vzorce proměnné, ale v současné době pro typy odkazů nejsou považovány za dokončené žádné sady přepínacích zbraní ve výrazu Switch, s výjimkou porovnávání proti statickému typu argumentu nebo podtypu.</span><span class="sxs-lookup"><span data-stu-id="8d984-136">Switch expressions can already match types through the variable pattern, but for currently for reference types, no set of switch arms in the switch expression are considered complete, except for matching against the static type of the argument, or a subtype.</span></span>

<span data-ttu-id="8d984-137">Výrazy přepínače by se změnily tak, že pokud je kořenový typ `enum class` statickým typem argumentu pro výraz Switch a existuje sada vzorů, které odpovídají všem členům výčtu, pak bude přepínač považován za vyčerpávající.</span><span class="sxs-lookup"><span data-stu-id="8d984-137">Switch expressions would be changed such that, if the root type of an `enum class` is the static type of the argument to the switch expression, and there is a set of patterns matching all members of the enum, then the switch will be considered exhaustive.</span></span>

<span data-ttu-id="8d984-138">Vzhledem k tomu, že členy hodnoty nejsou konstanty a nedefinují nové statické typy, nelze je aktuálně porovnat podle vzoru.</span><span class="sxs-lookup"><span data-stu-id="8d984-138">Since value members are not constants and do not define new static types, they currently cannot be matched by pattern.</span></span> <span data-ttu-id="8d984-139">Aby to bylo možné, byl přidán nový vzor s použitím syntaxe konstanty Pattern, aby bylo možné porovnat s `enum class`mi členy hodnoty.</span><span class="sxs-lookup"><span data-stu-id="8d984-139">To make this possible, a new pattern using the constant pattern syntax will be added to allow match against `enum class` value members.</span></span> <span data-ttu-id="8d984-140">Shoda je definována tak, aby byla úspěšná, pokud a pouze v případě, že se argument shoduje se vzorem a hodnota vrácená členem `enum class` hodnota by odkazovala na stejnou hodnotu, přestože implementace není nutná k provedení této kontroly.</span><span class="sxs-lookup"><span data-stu-id="8d984-140">The match is defined to succeed if and only if the argument to the pattern match and the value returned by the `enum class` value member would be reference equal, although the implementation is not required to perform this check.</span></span>


## <a name="open-questions"></a><span data-ttu-id="8d984-141">Otevřené otázky</span><span class="sxs-lookup"><span data-stu-id="8d984-141">Open questions</span></span>

- <span data-ttu-id="8d984-142">[] Co znamená společný typ algoritmu o `enum class` členů?</span><span class="sxs-lookup"><span data-stu-id="8d984-142">[ ] What does the common type algorithm say about `enum class` members?</span></span> <span data-ttu-id="8d984-143">Je tento platný kód?</span><span class="sxs-lookup"><span data-stu-id="8d984-143">Is this valid code?</span></span>
    * `var x = b ? new Shape.Rectangle(...) : new Shape.Circle(...)`

- <span data-ttu-id="8d984-144">[] Přidání nového vzoru pouze pro členy hodnoty se zdají být těžké.</span><span class="sxs-lookup"><span data-stu-id="8d984-144">[ ] Adding a new pattern just for value members seems heavy handed.</span></span> <span data-ttu-id="8d984-145">Je k dispozici obecnější konstrukce verzí, která dává smysl?</span><span class="sxs-lookup"><span data-stu-id="8d984-145">Is there a more general version construction that makes sense?</span></span>
    - <span data-ttu-id="8d984-146">[] Členové hodnoty také nemají správnou mapu konstrukce paralelní vnořené třídy z tohoto důvodu</span><span class="sxs-lookup"><span data-stu-id="8d984-146">[ ] Value members also do not map well to a parallel nested class construction because of this</span></span>

- <span data-ttu-id="8d984-147">[] Je přepínání s argumentem, který zaručuje, že `enum class` statický typ by měl být konstantní?</span><span class="sxs-lookup"><span data-stu-id="8d984-147">[ ] Is switching against an argument with an `enum class` static type guaranteed to be constant-time?</span></span>

- <span data-ttu-id="8d984-148">[] By existoval způsob, jak zajistit, aby `enum class`y ES nebyly považovány za dokončené ve výrazu Switch?</span><span class="sxs-lookup"><span data-stu-id="8d984-148">[ ] Should there be a way to make `enum class`es not be considered complete in the switch expression?</span></span> <span data-ttu-id="8d984-149">Předpona s `virtual`?</span><span class="sxs-lookup"><span data-stu-id="8d984-149">Prefix with `virtual`?</span></span>

- <span data-ttu-id="8d984-150">[] Jaké modifikátory by měly být povolené na `enum class`?</span><span class="sxs-lookup"><span data-stu-id="8d984-150">[ ] What modifiers should be permitted on `enum class`?</span></span>