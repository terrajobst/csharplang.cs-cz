---
ms.openlocfilehash: c3b716e6eb331be2ee33fffeb42c1e2406cd3a5c
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/25/2019
ms.locfileid: "64488761"
---
# <a name="enums"></a><span data-ttu-id="5e83e-101">Výčty</span><span class="sxs-lookup"><span data-stu-id="5e83e-101">Enums</span></span>

<span data-ttu-id="5e83e-102">***Typ výčtu*** je typ odlišné hodnoty ([typů hodnot](types.md#value-types)), který deklaruje sadou pojmenovaných konstant.</span><span class="sxs-lookup"><span data-stu-id="5e83e-102">An ***enum type*** is a distinct value type ([Value types](types.md#value-types)) that declares a set of named constants.</span></span>

<span data-ttu-id="5e83e-103">V příkladu</span><span class="sxs-lookup"><span data-stu-id="5e83e-103">The example</span></span>

```csharp
enum Color
{
    Red,
    Green,
    Blue
}
```

<span data-ttu-id="5e83e-104">deklaruje typ výčtu s názvem `Color` s členy `Red`, `Green`, a `Blue`.</span><span class="sxs-lookup"><span data-stu-id="5e83e-104">declares an enum type named `Color` with members `Red`, `Green`, and `Blue`.</span></span>

## <a name="enum-declarations"></a><span data-ttu-id="5e83e-105">Deklarace výčtu</span><span class="sxs-lookup"><span data-stu-id="5e83e-105">Enum declarations</span></span>

<span data-ttu-id="5e83e-106">Deklarace výčtu deklaruje nový typ výčtu.</span><span class="sxs-lookup"><span data-stu-id="5e83e-106">An enum declaration declares a new enum type.</span></span> <span data-ttu-id="5e83e-107">Deklarace výčtu začíná klíčovým slovem `enum`a definuje název, dostupnost, nadřízený typ a členy výčtu.</span><span class="sxs-lookup"><span data-stu-id="5e83e-107">An enum declaration begins with the keyword `enum`, and defines the name, accessibility, underlying type, and members of the enum.</span></span>

```antlr
enum_declaration
    : attributes? enum_modifier* 'enum' identifier enum_base? enum_body ';'?
    ;

enum_base
    : ':' integral_type
    ;

enum_body
    : '{' enum_member_declarations? '}'
    | '{' enum_member_declarations ',' '}'
    ;
```

<span data-ttu-id="5e83e-108">Každý typ enum má odpovídající integrální typ s názvem ***nadřízený typ*** typu výčtu.</span><span class="sxs-lookup"><span data-stu-id="5e83e-108">Each enum type has a corresponding integral type called the ***underlying type*** of the enum type.</span></span> <span data-ttu-id="5e83e-109">Tento typ musí být schopno představovat všechny hodnoty výčtu definované ve výčtu.</span><span class="sxs-lookup"><span data-stu-id="5e83e-109">This underlying type must be able to represent all the enumerator values defined in the enumeration.</span></span> <span data-ttu-id="5e83e-110">Základní typ explicitně deklarovat deklarace výčtu `byte`, `sbyte`, `short`, `ushort`, `int`, `uint`, `long` nebo `ulong`.</span><span class="sxs-lookup"><span data-stu-id="5e83e-110">An enum declaration may explicitly declare an underlying type of `byte`, `sbyte`, `short`, `ushort`, `int`, `uint`, `long` or `ulong`.</span></span> <span data-ttu-id="5e83e-111">Všimněte si, že `char` nelze použít jako základního typu.</span><span class="sxs-lookup"><span data-stu-id="5e83e-111">Note that `char` cannot be used as an underlying type.</span></span> <span data-ttu-id="5e83e-112">Deklarace výčtu, která nedeklaruje explicitně nadřízený typ má základní typ `int`.</span><span class="sxs-lookup"><span data-stu-id="5e83e-112">An enum declaration that does not explicitly declare an underlying type has an underlying type of `int`.</span></span>

<span data-ttu-id="5e83e-113">V příkladu</span><span class="sxs-lookup"><span data-stu-id="5e83e-113">The example</span></span>

```csharp
enum Color: long
{
    Red,
    Green,
    Blue
}
```

<span data-ttu-id="5e83e-114">deklaruje výčet s základního typu `long`.</span><span class="sxs-lookup"><span data-stu-id="5e83e-114">declares an enum with an underlying type of `long`.</span></span> <span data-ttu-id="5e83e-115">Vývojáři zvolit použití základního typu `long`, jako v příkladu umožní použít hodnoty, které jsou v rozsahu `long` , ale není v rozsahu `int`, nebo zachovat tuto možnost v budoucnosti.</span><span class="sxs-lookup"><span data-stu-id="5e83e-115">A developer might choose to use an underlying type of `long`, as in the example, to enable the use of values that are in the range of `long` but not in the range of `int`, or to preserve this option for the future.</span></span>

## <a name="enum-modifiers"></a><span data-ttu-id="5e83e-116">Modifikátory výčtu</span><span class="sxs-lookup"><span data-stu-id="5e83e-116">Enum modifiers</span></span>

<span data-ttu-id="5e83e-117">*Enum_declaration* může volitelně zahrnovat posloupnost modifikátory výčtu:</span><span class="sxs-lookup"><span data-stu-id="5e83e-117">An *enum_declaration* may optionally include a sequence of enum modifiers:</span></span>

```antlr
enum_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    ;
```

<span data-ttu-id="5e83e-118">Je chyba kompilace pro stejný modifikátor se zobrazí více než jednou v deklaraci výčtu.</span><span class="sxs-lookup"><span data-stu-id="5e83e-118">It is a compile-time error for the same modifier to appear multiple times in an enum declaration.</span></span>

<span data-ttu-id="5e83e-119">Modifikátory deklarace výčtu mají stejný význam jako deklarace třídy ([třídy modifikátory](classes.md#class-modifiers)).</span><span class="sxs-lookup"><span data-stu-id="5e83e-119">The modifiers of an enum declaration have the same meaning as those of a class declaration ([Class modifiers](classes.md#class-modifiers)).</span></span> <span data-ttu-id="5e83e-120">Upozorňujeme však, který `abstract` a `sealed` modifikátory nejsou povolené v deklaraci výčtu.</span><span class="sxs-lookup"><span data-stu-id="5e83e-120">Note, however, that the `abstract` and `sealed` modifiers are not permitted in an enum declaration.</span></span> <span data-ttu-id="5e83e-121">Výčty nemohou být abstraktní a není povoleno odvození.</span><span class="sxs-lookup"><span data-stu-id="5e83e-121">Enums cannot be abstract and do not permit derivation.</span></span>

## <a name="enum-members"></a><span data-ttu-id="5e83e-122">Členy výčtu</span><span class="sxs-lookup"><span data-stu-id="5e83e-122">Enum members</span></span>

<span data-ttu-id="5e83e-123">Tělo deklaraci typu výčtu definuje nula nebo více členy výčtu, které jsou pojmenované konstanty typu výčtu.</span><span class="sxs-lookup"><span data-stu-id="5e83e-123">The body of an enum type declaration defines zero or more enum members, which are the named constants of the enum type.</span></span> <span data-ttu-id="5e83e-124">Žádné dva členy výčtu může mít stejný název.</span><span class="sxs-lookup"><span data-stu-id="5e83e-124">No two enum members can have the same name.</span></span>

```antlr
enum_member_declarations
    : enum_member_declaration (',' enum_member_declaration)*
    ;

enum_member_declaration
    : attributes? identifier ('=' constant_expression)?
    ;
```

<span data-ttu-id="5e83e-125">Každý člen výčtu má přidruženou konstantní hodnotu.</span><span class="sxs-lookup"><span data-stu-id="5e83e-125">Each enum member has an associated constant value.</span></span> <span data-ttu-id="5e83e-126">Typ této hodnoty je základní typ pro obsahující Výčet.</span><span class="sxs-lookup"><span data-stu-id="5e83e-126">The type of this value is the underlying type for the containing enum.</span></span> <span data-ttu-id="5e83e-127">Konstantní hodnoty pro každého člena výčtu musí být v rozsahu nadřízený typ výčtu.</span><span class="sxs-lookup"><span data-stu-id="5e83e-127">The constant value for each enum member must be in the range of the underlying type for the enum.</span></span> <span data-ttu-id="5e83e-128">V příkladu</span><span class="sxs-lookup"><span data-stu-id="5e83e-128">The example</span></span>

```csharp
enum Color: uint
{
    Red = -1,
    Green = -2,
    Blue = -3
}
```

<span data-ttu-id="5e83e-129">výsledkem chyba kompilace, protože konstantní hodnoty `-1`, `-2`, a `-3` nejsou v rozsahu podkladových celočíselného typu `uint`.</span><span class="sxs-lookup"><span data-stu-id="5e83e-129">results in a compile-time error because the constant values `-1`, `-2`, and `-3` are not in the range of the underlying integral type `uint`.</span></span>

<span data-ttu-id="5e83e-130">Více členů výčtu může sdílet stejné přidruženou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="5e83e-130">Multiple enum members may share the same associated value.</span></span> <span data-ttu-id="5e83e-131">V příkladu</span><span class="sxs-lookup"><span data-stu-id="5e83e-131">The example</span></span>

```csharp
enum Color 
{
    Red,
    Green,
    Blue,

    Max = Blue
}
```

<span data-ttu-id="5e83e-132">Zobrazí výčet v dva členy výčtu – `Blue` a `Max` --mají stejné přidružené hodnoty.</span><span class="sxs-lookup"><span data-stu-id="5e83e-132">shows an enum in which two enum members -- `Blue` and `Max` -- have the same associated value.</span></span>

<span data-ttu-id="5e83e-133">Přidružená hodnota člen výčtu je přiřazena implicitně nebo explicitně.</span><span class="sxs-lookup"><span data-stu-id="5e83e-133">The associated value of an enum member is assigned either implicitly or explicitly.</span></span> <span data-ttu-id="5e83e-134">Pokud má deklarace člena výčtu *constant_expression* inicializátor, hodnota tento konstantní výraz, implicitně převeden na podkladový typ výčtového typu, je přidružená hodnota člena výčtu.</span><span class="sxs-lookup"><span data-stu-id="5e83e-134">If the declaration of the enum member has a *constant_expression* initializer, the value of that constant expression, implicitly converted to the underlying type of the enum, is the associated value of the enum member.</span></span> <span data-ttu-id="5e83e-135">Pokud deklarace výčtu člen nemá žádný inicializátor, je její přidružené hodnoty nastavit implicitně, následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="5e83e-135">If the declaration of the enum member has no initializer, its associated value is set implicitly, as follows:</span></span>

*  <span data-ttu-id="5e83e-136">Pokud je člen výčtu první člen výčtu deklarovaný ve výčtu typu, je jeho přidruženou hodnotu nula.</span><span class="sxs-lookup"><span data-stu-id="5e83e-136">If the enum member is the first enum member declared in the enum type, its associated value is zero.</span></span>
*  <span data-ttu-id="5e83e-137">V opačném případě se přidružená hodnota člena výčtu získá zvýšením přidružená hodnota textový předchozí člen výčtu o jednu.</span><span class="sxs-lookup"><span data-stu-id="5e83e-137">Otherwise, the associated value of the enum member is obtained by increasing the associated value of the textually preceding enum member by one.</span></span> <span data-ttu-id="5e83e-138">Tato vyšší hodnota musí být v rozsahu hodnot, které může být reprezentován základní typ, jinak dojde k chybě kompilace.</span><span class="sxs-lookup"><span data-stu-id="5e83e-138">This increased value must be within the range of values that can be represented by the underlying type, otherwise a compile-time error occurs.</span></span>

<span data-ttu-id="5e83e-139">V příkladu</span><span class="sxs-lookup"><span data-stu-id="5e83e-139">The example</span></span>

```csharp
using System;

enum Color
{
    Red,
    Green = 10,
    Blue
}

class Test
{
    static void Main() {
        Console.WriteLine(StringFromColor(Color.Red));
        Console.WriteLine(StringFromColor(Color.Green));
        Console.WriteLine(StringFromColor(Color.Blue));
    }

    static string StringFromColor(Color c) {
        switch (c) {
            case Color.Red: 
                return String.Format("Red = {0}", (int) c);

            case Color.Green:
                return String.Format("Green = {0}", (int) c);

            case Color.Blue:
                return String.Format("Blue = {0}", (int) c);

            default:
                return "Invalid color";
        }
    }
}
```

<span data-ttu-id="5e83e-140">Vytiskne názvy členů výčtu a jejich přidružené hodnoty.</span><span class="sxs-lookup"><span data-stu-id="5e83e-140">prints out the enum member names and their associated values.</span></span> <span data-ttu-id="5e83e-141">Výstup bude:</span><span class="sxs-lookup"><span data-stu-id="5e83e-141">The output is:</span></span>

```
Red = 0
Green = 10
Blue = 11
```

<span data-ttu-id="5e83e-142">z následujících důvodů:</span><span class="sxs-lookup"><span data-stu-id="5e83e-142">for the following reasons:</span></span>

*  <span data-ttu-id="5e83e-143">člen výčtu `Red` se automaticky přiřadí hodnotu nula (protože nemá žádný inicializátor a je první člen výčtu);</span><span class="sxs-lookup"><span data-stu-id="5e83e-143">the enum member `Red` is automatically assigned the value zero (since it has no initializer and is the first enum member);</span></span>
*  <span data-ttu-id="5e83e-144">člen výčtu `Green` explicitně přiřazena hodnota `10`;</span><span class="sxs-lookup"><span data-stu-id="5e83e-144">the enum member `Green` is explicitly given the value `10`;</span></span>
*  <span data-ttu-id="5e83e-145">a člen výčtu `Blue` je automaticky přiřazena hodnota větší než člena, který mu předchází pomocí textu.</span><span class="sxs-lookup"><span data-stu-id="5e83e-145">and the enum member `Blue` is automatically assigned the value one greater than the member that textually precedes it.</span></span>

<span data-ttu-id="5e83e-146">Přidružená hodnota člen výčtu není, může přímo nebo nepřímo, použijte hodnotu vlastní člen přidružené výčtu.</span><span class="sxs-lookup"><span data-stu-id="5e83e-146">The associated value of an enum member may not, directly or indirectly, use the value of its own associated enum member.</span></span> <span data-ttu-id="5e83e-147">Než toto omezení Cykličnost mohou odkazovat na jiné inicializátory členů výčtu, bez ohledu na jejich umístění textové volně inicializátory členů výčtu.</span><span class="sxs-lookup"><span data-stu-id="5e83e-147">Other than this circularity restriction, enum member initializers may freely refer to other enum member initializers, regardless of their textual position.</span></span> <span data-ttu-id="5e83e-148">V rámci inicializátor člena výčtu jsou hodnoty ostatních členů výčtu vždy považovány za typu základní typ, tak, aby přetypování nejsou nutné k odkazování na ostatní členy výčtu.</span><span class="sxs-lookup"><span data-stu-id="5e83e-148">Within an enum member initializer, values of other enum members are always treated as having the type of their underlying type, so that casts are not necessary when referring to other enum members.</span></span>

<span data-ttu-id="5e83e-149">V příkladu</span><span class="sxs-lookup"><span data-stu-id="5e83e-149">The example</span></span>

```csharp
enum Circular
{
    A = B,
    B
}
```

<span data-ttu-id="5e83e-150">výsledkem chyba kompilace, protože prohlášení o `A` a `B` jsou cyklické.</span><span class="sxs-lookup"><span data-stu-id="5e83e-150">results in a compile-time error because the declarations of `A` and `B` are circular.</span></span> <span data-ttu-id="5e83e-151">`A` závisí na `B` explicitně, a `B` závisí na `A` implicitně.</span><span class="sxs-lookup"><span data-stu-id="5e83e-151">`A` depends on `B` explicitly, and `B` depends on `A` implicitly.</span></span>

<span data-ttu-id="5e83e-152">Jsou členové výčtu s názvem a obor způsobem přesně obdobná na pole v rámci třídy.</span><span class="sxs-lookup"><span data-stu-id="5e83e-152">Enum members are named and scoped in a manner exactly analogous to fields within classes.</span></span> <span data-ttu-id="5e83e-153">Rozsah člen výčtu je tělo jeho nadřazený typ výčtu.</span><span class="sxs-lookup"><span data-stu-id="5e83e-153">The scope of an enum member is the body of its containing enum type.</span></span> <span data-ttu-id="5e83e-154">V daném oboru členy výčtu může být podle jejich jednoduchý název.</span><span class="sxs-lookup"><span data-stu-id="5e83e-154">Within that scope, enum members can be referred to by their simple name.</span></span> <span data-ttu-id="5e83e-155">Název na člena výčtu musí být kvalifikován s názvem typu výčtu, od jiného kódu.</span><span class="sxs-lookup"><span data-stu-id="5e83e-155">From all other code, the name of an enum member must be qualified with the name of its enum type.</span></span> <span data-ttu-id="5e83e-156">Členů výčtu nemají žádné deklarovaná přístupnost – člen výčtu je přístupný, pokud jeho nadřazený typ výčtu je přístupný.</span><span class="sxs-lookup"><span data-stu-id="5e83e-156">Enum members do not have any declared accessibility -- an enum member is accessible if its containing enum type is accessible.</span></span>

## <a name="the-systemenum-type"></a><span data-ttu-id="5e83e-157">Typ System.Enum</span><span class="sxs-lookup"><span data-stu-id="5e83e-157">The System.Enum type</span></span>

<span data-ttu-id="5e83e-158">Typ `System.Enum` je abstraktní základní třída všech typů výčtu (to se liší a liší se od základní typ typu výčtu) a členy zděděné z `System.Enum` jsou k dispozici v libovolný typ výčtu.</span><span class="sxs-lookup"><span data-stu-id="5e83e-158">The type `System.Enum` is the abstract base class of all enum types (this is distinct and different from the underlying type of the enum type), and the members inherited from `System.Enum` are available in any enum type.</span></span> <span data-ttu-id="5e83e-159">Převod na uzavřené určení ([zabalení převody](types.md#boxing-conversions)) existuje z libovolného typu výčtu na `System.Enum`a unboxingového převodu ([rozbalení převody](types.md#unboxing-conversions)) existuje z `System.Enum` na libovolný typ výčtu.</span><span class="sxs-lookup"><span data-stu-id="5e83e-159">A boxing conversion ([Boxing conversions](types.md#boxing-conversions)) exists from any enum type to `System.Enum`, and an unboxing conversion ([Unboxing conversions](types.md#unboxing-conversions)) exists from `System.Enum` to any enum type.</span></span>

<span data-ttu-id="5e83e-160">Všimněte si, že `System.Enum` není samotné *enum_type*.</span><span class="sxs-lookup"><span data-stu-id="5e83e-160">Note that `System.Enum` is not itself an *enum_type*.</span></span> <span data-ttu-id="5e83e-161">Místo toho je *class_type* z kterého jsou všechny *enum_type*s jsou odvozeny.</span><span class="sxs-lookup"><span data-stu-id="5e83e-161">Rather, it is a *class_type* from which all *enum_type*s are derived.</span></span> <span data-ttu-id="5e83e-162">Typ `System.Enum` dědí z typu `System.ValueType` ([typ The System.ValueType](types.md#the-systemvaluetype-type)), který zase dědí z typu `object`.</span><span class="sxs-lookup"><span data-stu-id="5e83e-162">The type `System.Enum` inherits from the type `System.ValueType` ([The System.ValueType type](types.md#the-systemvaluetype-type)), which, in turn, inherits from type `object`.</span></span> <span data-ttu-id="5e83e-163">V době běhu, hodnota typu `System.Enum` může být `null` nebo odkaz zabalené hodnoty libovolného typu výčtu.</span><span class="sxs-lookup"><span data-stu-id="5e83e-163">At run-time, a value of type `System.Enum` can be `null` or a reference to a boxed value of any enum type.</span></span>

## <a name="enum-values-and-operations"></a><span data-ttu-id="5e83e-164">Hodnoty výčtu a operace</span><span class="sxs-lookup"><span data-stu-id="5e83e-164">Enum values and operations</span></span>

<span data-ttu-id="5e83e-165">Každý typ výčtu definuje odlišný typ; konverzi explicitní výčet ([výčet explicitní převody](conversions.md#explicit-enumeration-conversions)) je nutné k převodu mezi výčtovým typem a celočíselného typu, nebo mezi dvěma typy výčtu.</span><span class="sxs-lookup"><span data-stu-id="5e83e-165">Each enum type defines a distinct type; an explicit enumeration conversion ([Explicit enumeration conversions](conversions.md#explicit-enumeration-conversions)) is required to convert between an enum type and an integral type, or between two enum types.</span></span> <span data-ttu-id="5e83e-166">Sadu hodnot, které může přijmout typ výčtu není omezena jeho členy výčtu.</span><span class="sxs-lookup"><span data-stu-id="5e83e-166">The set of values that an enum type can take on is not limited by its enum members.</span></span> <span data-ttu-id="5e83e-167">Konkrétně se libovolná hodnota základního typu výčtu lze převést na typ výčtu a odlišné platná hodnota daného typu výčtu.</span><span class="sxs-lookup"><span data-stu-id="5e83e-167">In particular, any value of the underlying type of an enum can be cast to the enum type, and is a distinct valid value of that enum type.</span></span>

<span data-ttu-id="5e83e-168">Členové výčtu mají typ jejich nadřazeným typem výčtu (s výjimkou v rámci jiné inicializátory členů výčtu: naleznete v tématu [členy výčtu](enums.md#enum-members)).</span><span class="sxs-lookup"><span data-stu-id="5e83e-168">Enum members have the type of their containing enum type (except within other enum member initializers: see [Enum members](enums.md#enum-members)).</span></span> <span data-ttu-id="5e83e-169">Hodnota člen výčtu deklarovaný v typu výčtu `E` s přidruženou hodnotu `v` je `(E)v`.</span><span class="sxs-lookup"><span data-stu-id="5e83e-169">The value of an enum member declared in enum type `E` with associated value `v` is `(E)v`.</span></span>

<span data-ttu-id="5e83e-170">U hodnot typu výčtu typů lze použít následující operátory: `==`, `!=`, `<`, `>`, `<=`, `>=`  ([operátory porovnání výčet](expressions.md#enumeration-comparison-operators)) , binární `+`  ([operátor sčítání](expressions.md#addition-operator)), binární `-`  ([operátor odčítání](expressions.md#subtraction-operator)), `^`, `&` , `|`  ([Logické operátory výčet](expressions.md#enumeration-logical-operators)), `~`  ([operátor bitového doplňku](expressions.md#bitwise-complement-operator)), `++` a `--`  ([Příponové operátory Inkrementace a dekrementace operátory](expressions.md#postfix-increment-and-decrement-operators) a [předpony Inkrementace a dekrementace operátory](expressions.md#prefix-increment-and-decrement-operators)).</span><span class="sxs-lookup"><span data-stu-id="5e83e-170">The following operators can be used on values of enum types: `==`, `!=`, `<`, `>`, `<=`, `>=` ([Enumeration comparison operators](expressions.md#enumeration-comparison-operators)), binary `+` ([Addition operator](expressions.md#addition-operator)), binary `-` ([Subtraction operator](expressions.md#subtraction-operator)), `^`, `&`, `|` ([Enumeration logical operators](expressions.md#enumeration-logical-operators)), `~` ([Bitwise complement operator](expressions.md#bitwise-complement-operator)), `++` and `--` ([Postfix increment and decrement operators](expressions.md#postfix-increment-and-decrement-operators) and [Prefix increment and decrement operators](expressions.md#prefix-increment-and-decrement-operators)).</span></span>

<span data-ttu-id="5e83e-171">Každý typ výčtu je automaticky odvozen ze třídy `System.Enum` (která, pak je odvozena z `System.ValueType` a `object`).</span><span class="sxs-lookup"><span data-stu-id="5e83e-171">Every enum type automatically derives from the class `System.Enum` (which, in turn, derives from `System.ValueType` and `object`).</span></span> <span data-ttu-id="5e83e-172">Proto zděděné metody a vlastnosti této třídy lze použít u hodnot typu výčtu.</span><span class="sxs-lookup"><span data-stu-id="5e83e-172">Thus, inherited methods and properties of this class can be used on values of an enum type.</span></span>
