---
ms.openlocfilehash: 3b142d7dbda8a94a4cf2c973a2e380065dcbf5ee
ms.sourcegitcommit: 892af9016b3317a8fae12d195014dc38ba51cf16
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/01/2019
ms.locfileid: "71703969"
---
# <a name="enums"></a><span data-ttu-id="34414-101">Výčty</span><span class="sxs-lookup"><span data-stu-id="34414-101">Enums</span></span>

<span data-ttu-id="34414-102">***Typ výčtu*** je jedinečný typ hodnoty ([typy hodnot](types.md#value-types)), který deklaruje sadu pojmenovaných konstant.</span><span class="sxs-lookup"><span data-stu-id="34414-102">An ***enum type*** is a distinct value type ([Value types](types.md#value-types)) that declares a set of named constants.</span></span>

<span data-ttu-id="34414-103">Příklad</span><span class="sxs-lookup"><span data-stu-id="34414-103">The example</span></span>

```csharp
enum Color
{
    Red,
    Green,
    Blue
}
```

<span data-ttu-id="34414-104">Deklaruje typ výčtu s názvem `Color` s členy `Red`, `Green` a `Blue`.</span><span class="sxs-lookup"><span data-stu-id="34414-104">declares an enum type named `Color` with members `Red`, `Green`, and `Blue`.</span></span>

## <a name="enum-declarations"></a><span data-ttu-id="34414-105">Deklarace výčtu</span><span class="sxs-lookup"><span data-stu-id="34414-105">Enum declarations</span></span>

<span data-ttu-id="34414-106">Deklarace výčtu deklaruje nový typ výčtu.</span><span class="sxs-lookup"><span data-stu-id="34414-106">An enum declaration declares a new enum type.</span></span> <span data-ttu-id="34414-107">Deklarace výčtu začíná klíčovým slovem `enum` a definuje název, přístupnost, nadřízený typ a členy výčtu.</span><span class="sxs-lookup"><span data-stu-id="34414-107">An enum declaration begins with the keyword `enum`, and defines the name, accessibility, underlying type, and members of the enum.</span></span>

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

<span data-ttu-id="34414-108">Každý typ výčtu má odpovídající celočíselný typ nazvaný ***základní typ*** výčtového typu.</span><span class="sxs-lookup"><span data-stu-id="34414-108">Each enum type has a corresponding integral type called the ***underlying type*** of the enum type.</span></span> <span data-ttu-id="34414-109">Tento nadřízený typ musí být schopný reprezentovat všechny hodnoty výčtu definované ve výčtu.</span><span class="sxs-lookup"><span data-stu-id="34414-109">This underlying type must be able to represent all the enumerator values defined in the enumeration.</span></span> <span data-ttu-id="34414-110">Deklarace výčtu může explicitně deklarovat základní typ `byte`, `sbyte`, `short`, `ushort`, `int`, `uint`, `long` nebo `ulong`.</span><span class="sxs-lookup"><span data-stu-id="34414-110">An enum declaration may explicitly declare an underlying type of `byte`, `sbyte`, `short`, `ushort`, `int`, `uint`, `long` or `ulong`.</span></span> <span data-ttu-id="34414-111">Všimněte si, že `char` nelze použít jako podkladový typ.</span><span class="sxs-lookup"><span data-stu-id="34414-111">Note that `char` cannot be used as an underlying type.</span></span> <span data-ttu-id="34414-112">Deklarace výčtu, která explicitně nedeklaruje nadřízený typ, má podkladový typ `int`.</span><span class="sxs-lookup"><span data-stu-id="34414-112">An enum declaration that does not explicitly declare an underlying type has an underlying type of `int`.</span></span>

<span data-ttu-id="34414-113">Příklad</span><span class="sxs-lookup"><span data-stu-id="34414-113">The example</span></span>

```csharp
enum Color: long
{
    Red,
    Green,
    Blue
}
```

<span data-ttu-id="34414-114">deklaruje výčet s podkladovým typem `long`.</span><span class="sxs-lookup"><span data-stu-id="34414-114">declares an enum with an underlying type of `long`.</span></span> <span data-ttu-id="34414-115">Vývojář se může rozhodnout použít základní typ `long`, jako v příkladu, pro povolení použití hodnot, které jsou v rozsahu `long`, ale ne v rozsahu `int`, nebo pro zachování této možnosti pro budoucnost.</span><span class="sxs-lookup"><span data-stu-id="34414-115">A developer might choose to use an underlying type of `long`, as in the example, to enable the use of values that are in the range of `long` but not in the range of `int`, or to preserve this option for the future.</span></span>

## <a name="enum-modifiers"></a><span data-ttu-id="34414-116">Modifikátory výčtu</span><span class="sxs-lookup"><span data-stu-id="34414-116">Enum modifiers</span></span>

<span data-ttu-id="34414-117">*Enum_declaration* může volitelně zahrnovat posloupnost modifikátorů výčtu:</span><span class="sxs-lookup"><span data-stu-id="34414-117">An *enum_declaration* may optionally include a sequence of enum modifiers:</span></span>

```antlr
enum_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    ;
```

<span data-ttu-id="34414-118">Jedná se o chybu při kompilaci, aby se stejný modifikátor zobrazoval víckrát v deklaraci výčtu.</span><span class="sxs-lookup"><span data-stu-id="34414-118">It is a compile-time error for the same modifier to appear multiple times in an enum declaration.</span></span>

<span data-ttu-id="34414-119">Modifikátory výčtového typu mají stejný význam jako deklarace třídy ([modifikátory třídy](classes.md#class-modifiers)).</span><span class="sxs-lookup"><span data-stu-id="34414-119">The modifiers of an enum declaration have the same meaning as those of a class declaration ([Class modifiers](classes.md#class-modifiers)).</span></span> <span data-ttu-id="34414-120">Upozorňujeme však, že modifikátory `abstract` a `sealed` nejsou v deklaraci výčtu povoleny.</span><span class="sxs-lookup"><span data-stu-id="34414-120">Note, however, that the `abstract` and `sealed` modifiers are not permitted in an enum declaration.</span></span> <span data-ttu-id="34414-121">Výčty nemohou být abstraktní a nepovolují odvození.</span><span class="sxs-lookup"><span data-stu-id="34414-121">Enums cannot be abstract and do not permit derivation.</span></span>

## <a name="enum-members"></a><span data-ttu-id="34414-122">Členy výčtu</span><span class="sxs-lookup"><span data-stu-id="34414-122">Enum members</span></span>

<span data-ttu-id="34414-123">Tělo deklarace výčtového typu definuje nula nebo více členů výčtu, které jsou pojmenované konstanty typu výčtu.</span><span class="sxs-lookup"><span data-stu-id="34414-123">The body of an enum type declaration defines zero or more enum members, which are the named constants of the enum type.</span></span> <span data-ttu-id="34414-124">Žádný ze dvou členů výčtu nesmí mít stejný název.</span><span class="sxs-lookup"><span data-stu-id="34414-124">No two enum members can have the same name.</span></span>

```antlr
enum_member_declarations
    : enum_member_declaration (',' enum_member_declaration)*
    ;

enum_member_declaration
    : attributes? identifier ('=' constant_expression)?
    ;
```

<span data-ttu-id="34414-125">Každý člen výčtu má přidruženou konstantní hodnotu.</span><span class="sxs-lookup"><span data-stu-id="34414-125">Each enum member has an associated constant value.</span></span> <span data-ttu-id="34414-126">Typ této hodnoty je základní typ pro obsažený výčet.</span><span class="sxs-lookup"><span data-stu-id="34414-126">The type of this value is the underlying type for the containing enum.</span></span> <span data-ttu-id="34414-127">Hodnota konstanty pro každý člen výčtu musí být v rozsahu nadřazeného typu pro výčet.</span><span class="sxs-lookup"><span data-stu-id="34414-127">The constant value for each enum member must be in the range of the underlying type for the enum.</span></span> <span data-ttu-id="34414-128">Příklad</span><span class="sxs-lookup"><span data-stu-id="34414-128">The example</span></span>

```csharp
enum Color: uint
{
    Red = -1,
    Green = -2,
    Blue = -3
}
```

<span data-ttu-id="34414-129">dojde k chybě v době kompilace, protože hodnoty konstant `-1`, `-2` a `-3` nejsou v rozsahu základního integrálního typu `uint`.</span><span class="sxs-lookup"><span data-stu-id="34414-129">results in a compile-time error because the constant values `-1`, `-2`, and `-3` are not in the range of the underlying integral type `uint`.</span></span>

<span data-ttu-id="34414-130">Víc členů výčtu může sdílet stejnou přidruženou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="34414-130">Multiple enum members may share the same associated value.</span></span> <span data-ttu-id="34414-131">Příklad</span><span class="sxs-lookup"><span data-stu-id="34414-131">The example</span></span>

```csharp
enum Color 
{
    Red,
    Green,
    Blue,

    Max = Blue
}
```

<span data-ttu-id="34414-132">zobrazuje výčet, ve kterém jsou dva členy výčtu--`Blue` a `Max`--mají stejnou přidruženou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="34414-132">shows an enum in which two enum members -- `Blue` and `Max` -- have the same associated value.</span></span>

<span data-ttu-id="34414-133">Přidružená hodnota člena výčtu je přiřazena implicitně nebo explicitně.</span><span class="sxs-lookup"><span data-stu-id="34414-133">The associated value of an enum member is assigned either implicitly or explicitly.</span></span> <span data-ttu-id="34414-134">Pokud deklarace člena výčtu má inicializátor *constant_expression* , hodnota tohoto konstantního výrazu implicitně převedená na nadřízený typ výčtu je přidružená hodnota člena výčtu.</span><span class="sxs-lookup"><span data-stu-id="34414-134">If the declaration of the enum member has a *constant_expression* initializer, the value of that constant expression, implicitly converted to the underlying type of the enum, is the associated value of the enum member.</span></span> <span data-ttu-id="34414-135">Pokud deklarace člena výčtu nemá žádný inicializátor, je jeho přidružená hodnota nastavena implicitně následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="34414-135">If the declaration of the enum member has no initializer, its associated value is set implicitly, as follows:</span></span>

*  <span data-ttu-id="34414-136">Pokud je člen výčtu prvním členem výčtu deklarovaným v typu výčtu, jeho přidružená hodnota je nula.</span><span class="sxs-lookup"><span data-stu-id="34414-136">If the enum member is the first enum member declared in the enum type, its associated value is zero.</span></span>
*  <span data-ttu-id="34414-137">V opačném případě se přidružená hodnota člena výčtu získá zvýšením přidružené hodnoty textu před výčtem, který je členem.</span><span class="sxs-lookup"><span data-stu-id="34414-137">Otherwise, the associated value of the enum member is obtained by increasing the associated value of the textually preceding enum member by one.</span></span> <span data-ttu-id="34414-138">Tato zvýšená hodnota musí být v rozsahu hodnot, které mohou být reprezentovány podkladovým typem, v opačném případě dojde k chybě při kompilaci.</span><span class="sxs-lookup"><span data-stu-id="34414-138">This increased value must be within the range of values that can be represented by the underlying type, otherwise a compile-time error occurs.</span></span>

<span data-ttu-id="34414-139">Příklad</span><span class="sxs-lookup"><span data-stu-id="34414-139">The example</span></span>

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

<span data-ttu-id="34414-140">vytiskne názvy členů výčtu a jejich přidružené hodnoty.</span><span class="sxs-lookup"><span data-stu-id="34414-140">prints out the enum member names and their associated values.</span></span> <span data-ttu-id="34414-141">Výstup je:</span><span class="sxs-lookup"><span data-stu-id="34414-141">The output is:</span></span>

```console
Red = 0
Green = 10
Blue = 11
```

<span data-ttu-id="34414-142">z následujících důvodů:</span><span class="sxs-lookup"><span data-stu-id="34414-142">for the following reasons:</span></span>

*  <span data-ttu-id="34414-143">člen výčtu `Red` automaticky přiřadí hodnotu nula (protože nemá žádný inicializátor a je prvním členem výčtu);</span><span class="sxs-lookup"><span data-stu-id="34414-143">the enum member `Red` is automatically assigned the value zero (since it has no initializer and is the first enum member);</span></span>
*  <span data-ttu-id="34414-144">člen výčtu `Green` má explicitně udělenou hodnotu `10`;</span><span class="sxs-lookup"><span data-stu-id="34414-144">the enum member `Green` is explicitly given the value `10`;</span></span>
*  <span data-ttu-id="34414-145">a člen výčtu `Blue` automaticky přiřazuje hodnotu vyšší než člen, který předá text.</span><span class="sxs-lookup"><span data-stu-id="34414-145">and the enum member `Blue` is automatically assigned the value one greater than the member that textually precedes it.</span></span>

<span data-ttu-id="34414-146">Přidružená hodnota člena výčtu nesmí přímo ani nepřímo používat hodnotu vlastního přidruženého člena výčtu.</span><span class="sxs-lookup"><span data-stu-id="34414-146">The associated value of an enum member may not, directly or indirectly, use the value of its own associated enum member.</span></span> <span data-ttu-id="34414-147">Kromě tohoto omezení cyklace mohou Inicializátory členů výčtu volně odkazovat na jiné Inicializátory členů výčtu, bez ohledu na jejich textovou pozici.</span><span class="sxs-lookup"><span data-stu-id="34414-147">Other than this circularity restriction, enum member initializers may freely refer to other enum member initializers, regardless of their textual position.</span></span> <span data-ttu-id="34414-148">V rámci inicializátoru členů výčtu jsou hodnoty jiných členů výčtu vždy zpracovány jako typ jejich nadřazeného typu, takže přetypování není nutné při odkazování na jiné členy výčtu.</span><span class="sxs-lookup"><span data-stu-id="34414-148">Within an enum member initializer, values of other enum members are always treated as having the type of their underlying type, so that casts are not necessary when referring to other enum members.</span></span>

<span data-ttu-id="34414-149">Příklad</span><span class="sxs-lookup"><span data-stu-id="34414-149">The example</span></span>

```csharp
enum Circular
{
    A = B,
    B
}
```

<span data-ttu-id="34414-150">dojde k chybě v době kompilace, protože deklarace `A` a `B` jsou cyklické.</span><span class="sxs-lookup"><span data-stu-id="34414-150">results in a compile-time error because the declarations of `A` and `B` are circular.</span></span> <span data-ttu-id="34414-151">`A` závisí explicitně na `B` a `B` je implicitně závislá na `A`.</span><span class="sxs-lookup"><span data-stu-id="34414-151">`A` depends on `B` explicitly, and `B` depends on `A` implicitly.</span></span>

<span data-ttu-id="34414-152">Členy výčtu jsou pojmenovány a vymezeny způsobem, který je přesně podobný polím v rámci tříd.</span><span class="sxs-lookup"><span data-stu-id="34414-152">Enum members are named and scoped in a manner exactly analogous to fields within classes.</span></span> <span data-ttu-id="34414-153">Rozsah člena výčtu je tělo jeho obsahujícího typu výčtu.</span><span class="sxs-lookup"><span data-stu-id="34414-153">The scope of an enum member is the body of its containing enum type.</span></span> <span data-ttu-id="34414-154">V rámci tohoto oboru můžou být členy výčtu odkazováni podle jejich jednoduchého názvu.</span><span class="sxs-lookup"><span data-stu-id="34414-154">Within that scope, enum members can be referred to by their simple name.</span></span> <span data-ttu-id="34414-155">Ze všech ostatních kódů musí být název člena výčtu kvalifikován názvem jeho typu výčtu.</span><span class="sxs-lookup"><span data-stu-id="34414-155">From all other code, the name of an enum member must be qualified with the name of its enum type.</span></span> <span data-ttu-id="34414-156">Členové výčtu nemají žádné deklarované přístupnost – člen výčtu je přístupný, pokud je jeho nadřazený typ výčtu přístupný.</span><span class="sxs-lookup"><span data-stu-id="34414-156">Enum members do not have any declared accessibility -- an enum member is accessible if its containing enum type is accessible.</span></span>

## <a name="the-systemenum-type"></a><span data-ttu-id="34414-157">Typ System. Enum</span><span class="sxs-lookup"><span data-stu-id="34414-157">The System.Enum type</span></span>

<span data-ttu-id="34414-158">Typ `System.Enum` je abstraktní základní třída všech typů výčtu (to je rozdílné a odlišná od základního typu typu výčtu) a členy zděděné z `System.Enum` jsou k dispozici v jakémkoli typu výčtu.</span><span class="sxs-lookup"><span data-stu-id="34414-158">The type `System.Enum` is the abstract base class of all enum types (this is distinct and different from the underlying type of the enum type), and the members inherited from `System.Enum` are available in any enum type.</span></span> <span data-ttu-id="34414-159">Převod zabalení ([převody zabalení](types.md#boxing-conversions)) existuje z libovolného typu výčtu na `System.Enum` a převod rozbalení ([převody rozbalení](types.md#unboxing-conversions)) existuje z `System.Enum` na libovolný typ výčtu.</span><span class="sxs-lookup"><span data-stu-id="34414-159">A boxing conversion ([Boxing conversions](types.md#boxing-conversions)) exists from any enum type to `System.Enum`, and an unboxing conversion ([Unboxing conversions](types.md#unboxing-conversions)) exists from `System.Enum` to any enum type.</span></span>

<span data-ttu-id="34414-160">Všimněte si, že `System.Enum` není *enum_type*.</span><span class="sxs-lookup"><span data-stu-id="34414-160">Note that `System.Enum` is not itself an *enum_type*.</span></span> <span data-ttu-id="34414-161">Místo toho je *class_type* , ze kterého jsou odvozeny všechny *enum_typey*.</span><span class="sxs-lookup"><span data-stu-id="34414-161">Rather, it is a *class_type* from which all *enum_type*s are derived.</span></span> <span data-ttu-id="34414-162">Typ `System.Enum` dědí z typu `System.ValueType` ([typ System. ValueType](types.md#the-systemvaluetype-type)), který naopak dědí z typu `object`.</span><span class="sxs-lookup"><span data-stu-id="34414-162">The type `System.Enum` inherits from the type `System.ValueType` ([The System.ValueType type](types.md#the-systemvaluetype-type)), which, in turn, inherits from type `object`.</span></span> <span data-ttu-id="34414-163">V době běhu může být hodnota typu `System.Enum` `null` nebo odkaz na zabalenou hodnotu libovolného typu výčtu.</span><span class="sxs-lookup"><span data-stu-id="34414-163">At run-time, a value of type `System.Enum` can be `null` or a reference to a boxed value of any enum type.</span></span>

## <a name="enum-values-and-operations"></a><span data-ttu-id="34414-164">Výčtové hodnoty a operace</span><span class="sxs-lookup"><span data-stu-id="34414-164">Enum values and operations</span></span>

<span data-ttu-id="34414-165">Každý typ výčtu definuje odlišný typ; pro převod mezi výčtovým typem a integrálním typem nebo mezi dvěma typy výčtu se vyžaduje explicitní převod výčtu ([explicitní převody výčtu](conversions.md#explicit-enumeration-conversions)).</span><span class="sxs-lookup"><span data-stu-id="34414-165">Each enum type defines a distinct type; an explicit enumeration conversion ([Explicit enumeration conversions](conversions.md#explicit-enumeration-conversions)) is required to convert between an enum type and an integral type, or between two enum types.</span></span> <span data-ttu-id="34414-166">Sada hodnot, na jejichž základě může typ výčtu pobírat, není omezena členy výčtu.</span><span class="sxs-lookup"><span data-stu-id="34414-166">The set of values that an enum type can take on is not limited by its enum members.</span></span> <span data-ttu-id="34414-167">Konkrétně jakákoli hodnota nadřazeného typu výčtu může být převedena na typ výčtu a je odlišná platná hodnota tohoto typu výčtu.</span><span class="sxs-lookup"><span data-stu-id="34414-167">In particular, any value of the underlying type of an enum can be cast to the enum type, and is a distinct valid value of that enum type.</span></span>

<span data-ttu-id="34414-168">Členy výčtu mají typ svého obsahujícího výčtového typu (s výjimkou jiných inicializátorů členů výčtu: viz [výčet členů](enums.md#enum-members)).</span><span class="sxs-lookup"><span data-stu-id="34414-168">Enum members have the type of their containing enum type (except within other enum member initializers: see [Enum members](enums.md#enum-members)).</span></span> <span data-ttu-id="34414-169">Hodnota člena výčtu deklarovaného v typu výčtu `E` s přidruženou hodnotou `v` je `(E)v`.</span><span class="sxs-lookup"><span data-stu-id="34414-169">The value of an enum member declared in enum type `E` with associated value `v` is `(E)v`.</span></span>

<span data-ttu-id="34414-170">Následující operátory lze použít na hodnoty typů výčtu: `==`, `!=`, `<`, `>`, `<=`, `>=` @ no__t-6 ([operátory porovnání výčtu](expressions.md#enumeration-comparison-operators)), binární `+` @ no__t-9 ([operátor sčítání](expressions.md#addition-operator)), binární 1 @ no__ t-12 ([operátor odčítání](expressions.md#subtraction-operator)), 4, 5, 6 @ no__t-17 ([logické operátory výčtu](expressions.md#enumeration-logical-operators)), 9 @ no__t-20 ([Operátor bitového doplňku](expressions.md#bitwise-complement-operator)), 2 a 3 @ no__t-24 ([přírůstek přípony a operátory snížení](expressions.md#postfix-increment-and-decrement-operators) a [operátory přírůstku a snížení předpony](expressions.md#prefix-increment-and-decrement-operators)).</span><span class="sxs-lookup"><span data-stu-id="34414-170">The following operators can be used on values of enum types: `==`, `!=`, `<`, `>`, `<=`, `>=` ([Enumeration comparison operators](expressions.md#enumeration-comparison-operators)), binary `+` ([Addition operator](expressions.md#addition-operator)), binary `-` ([Subtraction operator](expressions.md#subtraction-operator)), `^`, `&`, `|` ([Enumeration logical operators](expressions.md#enumeration-logical-operators)), `~` ([Bitwise complement operator](expressions.md#bitwise-complement-operator)), `++` and `--` ([Postfix increment and decrement operators](expressions.md#postfix-increment-and-decrement-operators) and [Prefix increment and decrement operators](expressions.md#prefix-increment-and-decrement-operators)).</span></span>

<span data-ttu-id="34414-171">Každý typ výčtu je automaticky odvozen od třídy `System.Enum` (což je naopak odvozeno z `System.ValueType` a `object`).</span><span class="sxs-lookup"><span data-stu-id="34414-171">Every enum type automatically derives from the class `System.Enum` (which, in turn, derives from `System.ValueType` and `object`).</span></span> <span data-ttu-id="34414-172">Proto lze zděděné metody a vlastnosti této třídy použít pro hodnoty typu výčtu.</span><span class="sxs-lookup"><span data-stu-id="34414-172">Thus, inherited methods and properties of this class can be used on values of an enum type.</span></span>
