---
ms.openlocfilehash: 088c4a77cecde490c556c44c239a3496f896582e
ms.sourcegitcommit: 4ddf18d000734c1b6d0a48127bf338086fc3f2c3
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/05/2019
ms.locfileid: "73616142"
---
# <a name="types"></a><span data-ttu-id="df7d3-101">Typy</span><span class="sxs-lookup"><span data-stu-id="df7d3-101">Types</span></span>

<span data-ttu-id="df7d3-102">Typy C# jazyka jsou rozděleny do dvou hlavních kategorií: ***typy hodnot*** a ***odkazové typy***.</span><span class="sxs-lookup"><span data-stu-id="df7d3-102">The types of the C# language are divided into two main categories: ***value types*** and ***reference types***.</span></span> <span data-ttu-id="df7d3-103">Oba typy hodnot i typy odkazů můžou být ***Obecné typy***, které přebírají jeden nebo víc ***parametrů typu***.</span><span class="sxs-lookup"><span data-stu-id="df7d3-103">Both value types and reference types may be ***generic types***, which take one or more ***type parameters***.</span></span> <span data-ttu-id="df7d3-104">Parametry typu mohou určovat typy hodnot a odkazové typy.</span><span class="sxs-lookup"><span data-stu-id="df7d3-104">Type parameters can designate both value types and reference types.</span></span>

```antlr
type
    : value_type
    | reference_type
    | type_parameter
    | type_unsafe
    ;
```

<span data-ttu-id="df7d3-105">Konečná kategorie typů, ukazatelů, je k dispozici pouze v nebezpečném kódu.</span><span class="sxs-lookup"><span data-stu-id="df7d3-105">The final category of types, pointers, is available only in unsafe code.</span></span> <span data-ttu-id="df7d3-106">Tento popis je podrobněji popsán v tématu [typy ukazatelů](unsafe-code.md#pointer-types).</span><span class="sxs-lookup"><span data-stu-id="df7d3-106">This is discussed further in [Pointer types](unsafe-code.md#pointer-types).</span></span>

<span data-ttu-id="df7d3-107">Typy hodnot se liší od typů odkazů v tom, že proměnné typů hodnot přímo obsahují svá data, zatímco proměnné referenčních typů ukládají ***odkazy*** na jejich data a ta se označují jako ***objekty***.</span><span class="sxs-lookup"><span data-stu-id="df7d3-107">Value types differ from reference types in that variables of the value types directly contain their data, whereas variables of the reference types store ***references*** to their data, the latter being known as ***objects***.</span></span> <span data-ttu-id="df7d3-108">U typů odkazů je možné, aby dvě proměnné odkazovaly na stejný objekt a bylo tak možné, aby operace na jedné proměnné ovlivnily objekt, na který je odkazováno z jiné proměnné.</span><span class="sxs-lookup"><span data-stu-id="df7d3-108">With reference types, it is possible for two variables to reference the same object, and thus possible for operations on one variable to affect the object referenced by the other variable.</span></span> <span data-ttu-id="df7d3-109">S typy hodnot mají proměnné, které mají svou vlastní kopii dat, a není možné, že operace na jednom mají vliv na ostatní.</span><span class="sxs-lookup"><span data-stu-id="df7d3-109">With value types, the variables each have their own copy of the data, and it is not possible for operations on one to affect the other.</span></span>

<span data-ttu-id="df7d3-110">C#systém typů je sjednocený tak, že hodnota libovolného typu může být zpracována jako objekt.</span><span class="sxs-lookup"><span data-stu-id="df7d3-110">C#'s type system is unified such that a value of any type can be treated as an object.</span></span> <span data-ttu-id="df7d3-111">Každý typ C# přímo nebo nepřímo je odvozen z typu `object` třídy a `object` je nejvyšší základní třídou všech typů.</span><span class="sxs-lookup"><span data-stu-id="df7d3-111">Every type in C# directly or indirectly derives from the `object` class type, and `object` is the ultimate base class of all types.</span></span> <span data-ttu-id="df7d3-112">Hodnoty typů odkazů se považují za objekty pouhým zobrazením hodnot jako typu `object`.</span><span class="sxs-lookup"><span data-stu-id="df7d3-112">Values of reference types are treated as objects simply by viewing the values as type `object`.</span></span> <span data-ttu-id="df7d3-113">Hodnoty typů hodnot se považují za objekty prováděním zabalení a rozbalení operací (zabalení[a rozbalení](types.md#boxing-and-unboxing)).</span><span class="sxs-lookup"><span data-stu-id="df7d3-113">Values of value types are treated as objects by performing boxing and unboxing operations ([Boxing and unboxing](types.md#boxing-and-unboxing)).</span></span>

## <a name="value-types"></a><span data-ttu-id="df7d3-114">Typy hodnot</span><span class="sxs-lookup"><span data-stu-id="df7d3-114">Value types</span></span>

<span data-ttu-id="df7d3-115">Typ hodnoty je buď typ struktury, nebo typ výčtu.</span><span class="sxs-lookup"><span data-stu-id="df7d3-115">A value type is either a struct type or an enumeration type.</span></span> <span data-ttu-id="df7d3-116">C#poskytuje sadu předdefinovaných typů struktur označovaných jako ***jednoduché typy***.</span><span class="sxs-lookup"><span data-stu-id="df7d3-116">C# provides a set of predefined struct types called the ***simple types***.</span></span> <span data-ttu-id="df7d3-117">Jednoduché typy jsou identifikovány prostřednictvím vyhrazených slov.</span><span class="sxs-lookup"><span data-stu-id="df7d3-117">The simple types are identified through reserved words.</span></span>

```antlr
value_type
    : struct_type
    | enum_type
    ;

struct_type
    : type_name
    | simple_type
    | nullable_type
    ;

simple_type
    : numeric_type
    | 'bool'
    ;

numeric_type
    : integral_type
    | floating_point_type
    | 'decimal'
    ;

integral_type
    : 'sbyte'
    | 'byte'
    | 'short'
    | 'ushort'
    | 'int'
    | 'uint'
    | 'long'
    | 'ulong'
    | 'char'
    ;

floating_point_type
    : 'float'
    | 'double'
    ;

nullable_type
    : non_nullable_value_type '?'
    ;

non_nullable_value_type
    : type
    ;

enum_type
    : type_name
    ;
```

<span data-ttu-id="df7d3-118">Na rozdíl od proměnné typu odkazu může proměnná typu hodnoty obsahovat hodnotu `null` pouze v případě, že typ hodnoty je typ s možnou hodnotou null.</span><span class="sxs-lookup"><span data-stu-id="df7d3-118">Unlike a variable of a reference type, a variable of a value type can contain the value `null` only if the value type is a nullable type.</span></span>  <span data-ttu-id="df7d3-119">Pro každý typ hodnoty, která není null, existuje odpovídající typ hodnoty s možnou hodnotou null, který označuje stejnou sadu hodnot a hodnotu `null`.</span><span class="sxs-lookup"><span data-stu-id="df7d3-119">For every non-nullable value type there is a corresponding nullable value type denoting the same set of values plus the value `null`.</span></span>

<span data-ttu-id="df7d3-120">Přiřazení proměnné typu hodnoty vytvoří kopii hodnoty, která je přiřazena.</span><span class="sxs-lookup"><span data-stu-id="df7d3-120">Assignment to a variable of a value type creates a copy of the value being assigned.</span></span> <span data-ttu-id="df7d3-121">To se liší od přiřazení k proměnné typu odkazu, který kopíruje odkaz, ale nikoli objekt identifikovaný odkazem.</span><span class="sxs-lookup"><span data-stu-id="df7d3-121">This differs from assignment to a variable of a reference type, which copies the reference but not the object identified by the reference.</span></span>

### <a name="the-systemvaluetype-type"></a><span data-ttu-id="df7d3-122">Typ System. ValueType</span><span class="sxs-lookup"><span data-stu-id="df7d3-122">The System.ValueType type</span></span>

<span data-ttu-id="df7d3-123">Všechny typy hodnot jsou implicitně děděny od třídy `System.ValueType`, která zase dědí z třídy `object`.</span><span class="sxs-lookup"><span data-stu-id="df7d3-123">All value types implicitly inherit from the class `System.ValueType`, which, in turn, inherits from class `object`.</span></span> <span data-ttu-id="df7d3-124">Není možné, aby žádný typ byl odvozen od typu hodnoty a typy hodnot jsou tedy implicitně zapečetěné ([zapečetěné třídy](classes.md#sealed-classes)).</span><span class="sxs-lookup"><span data-stu-id="df7d3-124">It is not possible for any type to derive from a value type, and value types are thus implicitly sealed ([Sealed classes](classes.md#sealed-classes)).</span></span>

<span data-ttu-id="df7d3-125">Všimněte si, že `System.ValueType` *value_type*sám o sobě.</span><span class="sxs-lookup"><span data-stu-id="df7d3-125">Note that `System.ValueType` is not itself a *value_type*.</span></span> <span data-ttu-id="df7d3-126">Místo toho je *class_type* , ze kterého jsou automaticky odvozeny všechny *value_typey*.</span><span class="sxs-lookup"><span data-stu-id="df7d3-126">Rather, it is a *class_type* from which all *value_type*s are automatically derived.</span></span>

### <a name="default-constructors"></a><span data-ttu-id="df7d3-127">Výchozí konstruktory</span><span class="sxs-lookup"><span data-stu-id="df7d3-127">Default constructors</span></span>

<span data-ttu-id="df7d3-128">Všechny typy hodnot implicitně deklaruje veřejný konstruktor instance bez parametrů s názvem ***výchozí konstruktor***.</span><span class="sxs-lookup"><span data-stu-id="df7d3-128">All value types implicitly declare a public parameterless instance constructor called the ***default constructor***.</span></span> <span data-ttu-id="df7d3-129">Výchozí konstruktor vrátí instanci s nulovou inicializací, která je známá jako ***Výchozí hodnota*** pro typ hodnoty:</span><span class="sxs-lookup"><span data-stu-id="df7d3-129">The default constructor returns a zero-initialized instance known as the ***default value*** for the value type:</span></span>

*  <span data-ttu-id="df7d3-130">V případě všech *Simple_Type*s je výchozí hodnotou hodnota vytvořená pomocí bitového vzoru všech nul:</span><span class="sxs-lookup"><span data-stu-id="df7d3-130">For all *simple_type*s, the default value is the value produced by a bit pattern of all zeros:</span></span>
    * <span data-ttu-id="df7d3-131">V případě `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`a `ulong`, je výchozí hodnota `0`.</span><span class="sxs-lookup"><span data-stu-id="df7d3-131">For `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, and `ulong`, the default value is `0`.</span></span>
    * <span data-ttu-id="df7d3-132">Pro `char`je výchozí hodnota `'\x0000'`.</span><span class="sxs-lookup"><span data-stu-id="df7d3-132">For `char`, the default value is `'\x0000'`.</span></span>
    * <span data-ttu-id="df7d3-133">Pro `float`je výchozí hodnota `0.0f`.</span><span class="sxs-lookup"><span data-stu-id="df7d3-133">For `float`, the default value is `0.0f`.</span></span>
    * <span data-ttu-id="df7d3-134">Pro `double`je výchozí hodnota `0.0d`.</span><span class="sxs-lookup"><span data-stu-id="df7d3-134">For `double`, the default value is `0.0d`.</span></span>
    * <span data-ttu-id="df7d3-135">Pro `decimal`je výchozí hodnota `0.0m`.</span><span class="sxs-lookup"><span data-stu-id="df7d3-135">For `decimal`, the default value is `0.0m`.</span></span>
    * <span data-ttu-id="df7d3-136">Pro `bool`je výchozí hodnota `false`.</span><span class="sxs-lookup"><span data-stu-id="df7d3-136">For `bool`, the default value is `false`.</span></span>
*  <span data-ttu-id="df7d3-137">Pro *enum_type* `E`je výchozí hodnota `0`převedená na typ `E`.</span><span class="sxs-lookup"><span data-stu-id="df7d3-137">For an *enum_type* `E`, the default value is `0`, converted to the type `E`.</span></span>
*  <span data-ttu-id="df7d3-138">Pro *struct_type*je výchozí hodnotou hodnota vytvořená nastavením všech polí hodnotového typu na jejich výchozí hodnotu a všechna pole odkazového typu na `null`.</span><span class="sxs-lookup"><span data-stu-id="df7d3-138">For a *struct_type*, the default value is the value produced by setting all value type fields to their default value and all reference type fields to `null`.</span></span>
*  <span data-ttu-id="df7d3-139">Pro *nullable_type* výchozí hodnota je instance, pro kterou je vlastnost `HasValue` false a vlastnost `Value` není definována.</span><span class="sxs-lookup"><span data-stu-id="df7d3-139">For a *nullable_type* the default value is an instance for which the `HasValue` property is false and the `Value` property is undefined.</span></span> <span data-ttu-id="df7d3-140">Výchozí hodnota je také známá jako ***hodnota null*** typu s možnou hodnotou null.</span><span class="sxs-lookup"><span data-stu-id="df7d3-140">The default value is also known as the ***null value*** of the nullable type.</span></span>

<span data-ttu-id="df7d3-141">Podobně jako jakýkoli jiný konstruktor instance je výchozí konstruktor hodnotového typu vyvolán pomocí operátoru `new`.</span><span class="sxs-lookup"><span data-stu-id="df7d3-141">Like any other instance constructor, the default constructor of a value type is invoked using the `new` operator.</span></span> <span data-ttu-id="df7d3-142">Z důvodu efektivity není tento požadavek určen jako ve skutečnosti, že implementace konstruktoru vygenerovala volání konstruktoru.</span><span class="sxs-lookup"><span data-stu-id="df7d3-142">For efficiency reasons, this requirement is not intended to actually have the implementation generate a constructor call.</span></span> <span data-ttu-id="df7d3-143">V následujícím příkladu jsou proměnné `i` a `j` inicializovány na hodnotu nula.</span><span class="sxs-lookup"><span data-stu-id="df7d3-143">In the example below, variables `i` and `j` are both initialized to zero.</span></span>

```csharp
class A
{
    void F() {
        int i = 0;
        int j = new int();
    }
}
```

<span data-ttu-id="df7d3-144">Vzhledem k tomu, že každý typ hodnoty má implicitně veřejný konstruktor instance bez parametrů, není možné, aby typ struktury obsahoval explicitní deklaraci konstruktoru bez parametrů.</span><span class="sxs-lookup"><span data-stu-id="df7d3-144">Because every value type implicitly has a public parameterless instance constructor, it is not possible for a struct type to contain an explicit declaration of a parameterless constructor.</span></span> <span data-ttu-id="df7d3-145">Typ struktury je však povoleno deklarovat parametrizované konstruktory instancí ([konstruktory](structs.md#constructors)).</span><span class="sxs-lookup"><span data-stu-id="df7d3-145">A struct type is however permitted to declare parameterized instance constructors ([Constructors](structs.md#constructors)).</span></span>

### <a name="struct-types"></a><span data-ttu-id="df7d3-146">Typy struktury</span><span class="sxs-lookup"><span data-stu-id="df7d3-146">Struct types</span></span>

<span data-ttu-id="df7d3-147">Typ struktury je typ hodnoty, který může deklarovat konstanty, pole, metody, vlastnosti, indexery, operátory, konstruktory instancí, statické konstruktory a vnořené typy.</span><span class="sxs-lookup"><span data-stu-id="df7d3-147">A struct type is a value type that can declare constants, fields, methods, properties, indexers, operators, instance constructors, static constructors, and nested types.</span></span> <span data-ttu-id="df7d3-148">Deklarace typů struktury je popsána v tématu [deklarace struktury](structs.md#struct-declarations).</span><span class="sxs-lookup"><span data-stu-id="df7d3-148">The declaration of struct types is described in [Struct declarations](structs.md#struct-declarations).</span></span>

### <a name="simple-types"></a><span data-ttu-id="df7d3-149">Jednoduché typy</span><span class="sxs-lookup"><span data-stu-id="df7d3-149">Simple types</span></span>

<span data-ttu-id="df7d3-150">C#poskytuje sadu předdefinovaných typů struktur označovaných jako ***jednoduché typy***.</span><span class="sxs-lookup"><span data-stu-id="df7d3-150">C# provides a set of predefined struct types called the ***simple types***.</span></span> <span data-ttu-id="df7d3-151">Jednoduché typy jsou identifikovány prostřednictvím rezervovaných slov, ale tato vyhrazená slova jsou jednoduše aliasy pro předdefinované typy struktury v oboru názvů `System`, jak je popsáno v následující tabulce.</span><span class="sxs-lookup"><span data-stu-id="df7d3-151">The simple types are identified through reserved words, but these reserved words are simply aliases for predefined struct types in the `System` namespace, as described in the table below.</span></span>


| <span data-ttu-id="df7d3-152">__Rezervované slovo__</span><span class="sxs-lookup"><span data-stu-id="df7d3-152">__Reserved word__</span></span> | <span data-ttu-id="df7d3-153">__Typ s aliasem__</span><span class="sxs-lookup"><span data-stu-id="df7d3-153">__Aliased type__</span></span> |
|-------------------|------------------|
| `sbyte`           | `System.SByte`   | 
| `byte`            | `System.Byte`    | 
| `short`           | `System.Int16`   | 
| `ushort`          | `System.UInt16`  | 
| `int`             | `System.Int32`   | 
| `uint`            | `System.UInt32`  | 
| `long`            | `System.Int64`   | 
| `ulong`           | `System.UInt64`  | 
| `char`            | `System.Char`    | 
| `float`           | `System.Single`  | 
| `double`          | `System.Double`  | 
| `bool`            | `System.Boolean` | 
| `decimal`         | `System.Decimal` | 

<span data-ttu-id="df7d3-154">Vzhledem k tomu, že jednoduchý typ aliasuje typ struktury, každý jednoduchý typ má členy.</span><span class="sxs-lookup"><span data-stu-id="df7d3-154">Because a simple type aliases a struct type, every simple type has members.</span></span> <span data-ttu-id="df7d3-155">Například `int` má členy deklarované v `System.Int32` a členy zděděné z `System.Object`a jsou povoleny následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="df7d3-155">For example, `int` has the members declared in `System.Int32` and the members inherited from `System.Object`, and the following statements are permitted:</span></span>

```csharp
int i = int.MaxValue;           // System.Int32.MaxValue constant
string s = i.ToString();        // System.Int32.ToString() instance method
string t = 123.ToString();      // System.Int32.ToString() instance method
```

<span data-ttu-id="df7d3-156">Jednoduché typy se liší od jiných typů struktury v tom, že umožňují určité další operace:</span><span class="sxs-lookup"><span data-stu-id="df7d3-156">The simple types differ from other struct types in that they permit certain additional operations:</span></span>

*  <span data-ttu-id="df7d3-157">Většina jednoduchých typů povoluje vytváření hodnot zápisem *literálů* ([literály](lexical-structure.md#literals)).</span><span class="sxs-lookup"><span data-stu-id="df7d3-157">Most simple types permit values to be created by writing *literals* ([Literals](lexical-structure.md#literals)).</span></span> <span data-ttu-id="df7d3-158">Například `123` je literál typu `int` a `'a'` je literál typu `char`.</span><span class="sxs-lookup"><span data-stu-id="df7d3-158">For example, `123` is a literal of type `int` and `'a'` is a literal of type `char`.</span></span> <span data-ttu-id="df7d3-159">C#neposkytuje žádné zřizování pro literály typů struktury obecně a jiné než výchozí hodnoty jiných typů struktury jsou nakonec vždy vytvářeny prostřednictvím konstruktorů instancí těchto typů struktury.</span><span class="sxs-lookup"><span data-stu-id="df7d3-159">C# makes no provision for literals of struct types in general, and non-default values of other struct types are ultimately always created through instance constructors of those struct types.</span></span>
*  <span data-ttu-id="df7d3-160">Pokud jsou operandy výrazu všechny jednoduché konstanty typu, je možné, že kompilátor vyhodnotí výraz v době kompilace.</span><span class="sxs-lookup"><span data-stu-id="df7d3-160">When the operands of an expression are all simple type constants, it is possible for the compiler to evaluate the expression at compile-time.</span></span> <span data-ttu-id="df7d3-161">Takový výraz je označován jako *constant_expression* ([konstantní výrazy](expressions.md#constant-expressions)).</span><span class="sxs-lookup"><span data-stu-id="df7d3-161">Such an expression is known as a *constant_expression* ([Constant expressions](expressions.md#constant-expressions)).</span></span> <span data-ttu-id="df7d3-162">Výrazy zahrnující operátory definované jinými typy struktury nejsou považovány za konstantní výrazy.</span><span class="sxs-lookup"><span data-stu-id="df7d3-162">Expressions involving operators defined by other struct types are not considered to be constant expressions.</span></span>
*  <span data-ttu-id="df7d3-163">Prostřednictvím deklarace `const` je možné deklarovat konstanty jednoduchých typů ([konstant](classes.md#constants)).</span><span class="sxs-lookup"><span data-stu-id="df7d3-163">Through `const` declarations it is possible to declare constants of the simple types ([Constants](classes.md#constants)).</span></span> <span data-ttu-id="df7d3-164">Není možné mít konstanty jiných typů struktury, ale podobný efekt poskytuje `static readonly` pole.</span><span class="sxs-lookup"><span data-stu-id="df7d3-164">It is not possible to have constants of other struct types, but a similar effect is provided by `static readonly` fields.</span></span>
*  <span data-ttu-id="df7d3-165">Převody zahrnující jednoduché typy se mohou účastnit vyhodnocení operátorů převodu definovaných jinými typy struktury, ale uživatelsky definovaný operátor převodu se nikdy nemůže zúčastnit vyhodnocení jiného uživatelsky definovaného operátoru ([vyhodnocení uživatelem definované převody](conversions.md#evaluation-of-user-defined-conversions)).</span><span class="sxs-lookup"><span data-stu-id="df7d3-165">Conversions involving simple types can participate in evaluation of conversion operators defined by other struct types, but a user-defined conversion operator can never participate in evaluation of another user-defined operator ([Evaluation of user-defined conversions](conversions.md#evaluation-of-user-defined-conversions)).</span></span>

### <a name="integral-types"></a><span data-ttu-id="df7d3-166">Celočíselné typy</span><span class="sxs-lookup"><span data-stu-id="df7d3-166">Integral types</span></span>

<span data-ttu-id="df7d3-167">C#podporuje devět integrálních typů: `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`a `char`.</span><span class="sxs-lookup"><span data-stu-id="df7d3-167">C# supports nine integral types: `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, and `char`.</span></span> <span data-ttu-id="df7d3-168">Integrální typy mají následující velikosti a rozsahy hodnot:</span><span class="sxs-lookup"><span data-stu-id="df7d3-168">The integral types have the following sizes and ranges of values:</span></span>

*  <span data-ttu-id="df7d3-169">Typ `sbyte` představuje 8bitové celé číslo se znaménkem hodnoty mezi-128 a 127.</span><span class="sxs-lookup"><span data-stu-id="df7d3-169">The `sbyte` type represents signed 8-bit integers with values between -128 and 127.</span></span>
*  <span data-ttu-id="df7d3-170">Typ `byte` představuje 8bitové celé číslo bez znaménka s hodnotami mezi 0 a 255.</span><span class="sxs-lookup"><span data-stu-id="df7d3-170">The `byte` type represents unsigned 8-bit integers with values between 0 and 255.</span></span>
*  <span data-ttu-id="df7d3-171">Typ `short` představuje 16bitová celá čísla se znaménkem hodnoty mezi-32768 a 32767.</span><span class="sxs-lookup"><span data-stu-id="df7d3-171">The `short` type represents signed 16-bit integers with values between -32768 and 32767.</span></span>
*  <span data-ttu-id="df7d3-172">Typ `ushort` představuje nepodepsaná 16bitová celá čísla s hodnotami mezi 0 a 65535.</span><span class="sxs-lookup"><span data-stu-id="df7d3-172">The `ushort` type represents unsigned 16-bit integers with values between 0 and 65535.</span></span>
*  <span data-ttu-id="df7d3-173">Typ `int` představuje podepsaná 32 celých čísel s hodnotami mezi-2147483648 a 2147483647.</span><span class="sxs-lookup"><span data-stu-id="df7d3-173">The `int` type represents signed 32-bit integers with values between -2147483648 and 2147483647.</span></span>
*  <span data-ttu-id="df7d3-174">Typ `uint` představuje nepodepsaná 32 celých čísel s hodnotami mezi 0 a 4294967295.</span><span class="sxs-lookup"><span data-stu-id="df7d3-174">The `uint` type represents unsigned 32-bit integers with values between 0 and 4294967295.</span></span>
*  <span data-ttu-id="df7d3-175">Typ `long` představuje podepsaná 64 celých čísel s hodnotami mezi-9223372036854775808 a 9223372036854775807.</span><span class="sxs-lookup"><span data-stu-id="df7d3-175">The `long` type represents signed 64-bit integers with values between -9223372036854775808 and 9223372036854775807.</span></span>
*  <span data-ttu-id="df7d3-176">Typ `ulong` představuje nepodepsaná 64 celých čísel s hodnotami mezi 0 a 18446744073709551615.</span><span class="sxs-lookup"><span data-stu-id="df7d3-176">The `ulong` type represents unsigned 64-bit integers with values between 0 and 18446744073709551615.</span></span>
*  <span data-ttu-id="df7d3-177">Typ `char` představuje nepodepsaná 16bitová celá čísla s hodnotami mezi 0 a 65535.</span><span class="sxs-lookup"><span data-stu-id="df7d3-177">The `char` type represents unsigned 16-bit integers with values between 0 and 65535.</span></span> <span data-ttu-id="df7d3-178">Sada možných hodnot pro typ `char` odpovídá sadě znaků Unicode.</span><span class="sxs-lookup"><span data-stu-id="df7d3-178">The set of possible values for the `char` type corresponds to the Unicode character set.</span></span> <span data-ttu-id="df7d3-179">I když `char` má stejnou reprezentaci jako `ushort`, nejsou povoleny všechny operace povolené u jednoho typu.</span><span class="sxs-lookup"><span data-stu-id="df7d3-179">Although `char` has the same representation as `ushort`, not all operations permitted on one type are permitted on the other.</span></span>

<span data-ttu-id="df7d3-180">Unární a binární operátory integrálního typu vždy pracují s podepsanou 32-bitovou přesností, nepodepsaná 32-bit Precision, podepsaná 64-bit Precision nebo bez 64 znaménka na.</span><span class="sxs-lookup"><span data-stu-id="df7d3-180">The integral-type unary and binary operators always operate with signed 32-bit precision, unsigned 32-bit precision, signed 64-bit precision, or unsigned 64-bit precision:</span></span>

*  <span data-ttu-id="df7d3-181">U unárních operátorů `+` a `~` je operand převeden na typ `T`, kde `T` je první z `int`, `uint`, `long`a `ulong`, které mohou plně představovat všechny možné hodnoty operandu.</span><span class="sxs-lookup"><span data-stu-id="df7d3-181">For the unary `+` and `~` operators, the operand is converted to type `T`, where `T` is the first of `int`, `uint`, `long`, and `ulong` that can fully represent all possible values of the operand.</span></span> <span data-ttu-id="df7d3-182">Operace se pak provede pomocí přesnosti typu `T`a typ výsledku je `T`.</span><span class="sxs-lookup"><span data-stu-id="df7d3-182">The operation is then performed using the precision of type `T`, and the type of the result is `T`.</span></span>
*  <span data-ttu-id="df7d3-183">Pro unární operátor `-` je operand převeden na typ `T`, kde `T` je první `int` a `long`, který může plně reprezentovat všechny možné hodnoty operandu.</span><span class="sxs-lookup"><span data-stu-id="df7d3-183">For the unary `-` operator, the operand is converted to type `T`, where `T` is the first of `int` and `long` that can fully represent all possible values of the operand.</span></span> <span data-ttu-id="df7d3-184">Operace se pak provede pomocí přesnosti typu `T`a typ výsledku je `T`.</span><span class="sxs-lookup"><span data-stu-id="df7d3-184">The operation is then performed using the precision of type `T`, and the type of the result is `T`.</span></span> <span data-ttu-id="df7d3-185">Unární operátor `-` nelze použít na operandy typu `ulong`.</span><span class="sxs-lookup"><span data-stu-id="df7d3-185">The unary `-` operator cannot be applied to operands of type `ulong`.</span></span>
*  <span data-ttu-id="df7d3-186">Pro binární `+`, `-`, `*`, `/`, `%`, `&`, `^`, `|`, `==`, `!=`, `>`, `<`, `>=`a `<=` operátory , jsou operandy převedeny na typ `T`, kde `T` je první z `int`, `uint`, `long`a `ulong`, které mohou plně představovat všechny možné hodnoty obou operandů.</span><span class="sxs-lookup"><span data-stu-id="df7d3-186">For the binary `+`, `-`, `*`, `/`, `%`, `&`, `^`, `|`, `==`, `!=`, `>`, `<`, `>=`, and `<=` operators, the operands are converted to type `T`, where `T` is the first of `int`, `uint`, `long`, and `ulong` that can fully represent all possible values of both operands.</span></span> <span data-ttu-id="df7d3-187">Operace se pak provede pomocí přesnosti typu `T`a typ výsledku je `T` (nebo `bool` pro relační operátory).</span><span class="sxs-lookup"><span data-stu-id="df7d3-187">The operation is then performed using the precision of type `T`, and the type of the result is `T` (or `bool` for the relational operators).</span></span> <span data-ttu-id="df7d3-188">Není povoleno, aby jeden operand byl typu `long` a druhý pro typ `ulong` s binárními operátory.</span><span class="sxs-lookup"><span data-stu-id="df7d3-188">It is not permitted for one operand to be of type `long` and the other to be of type `ulong` with the binary operators.</span></span>
*  <span data-ttu-id="df7d3-189">Pro binární `<<` a `>>` operátory je levý operand převeden na typ `T`, kde `T` je první z `int`, `uint`, `long`a `ulong`, které mohou plně představovat všechny možné hodnoty operandu.</span><span class="sxs-lookup"><span data-stu-id="df7d3-189">For the binary `<<` and `>>` operators, the left operand is converted to type `T`, where `T` is the first of `int`, `uint`, `long`, and `ulong` that can fully represent all possible values of the operand.</span></span> <span data-ttu-id="df7d3-190">Operace se pak provede pomocí přesnosti typu `T`a typ výsledku je `T`.</span><span class="sxs-lookup"><span data-stu-id="df7d3-190">The operation is then performed using the precision of type `T`, and the type of the result is `T`.</span></span>

<span data-ttu-id="df7d3-191">Typ `char` je klasifikován jako integrální typ, ale liší se od ostatních integrálních typů dvěma způsoby:</span><span class="sxs-lookup"><span data-stu-id="df7d3-191">The `char` type is classified as an integral type, but it differs from the other integral types in two ways:</span></span>

*  <span data-ttu-id="df7d3-192">Neexistují žádné implicitní převody z jiných typů na typ `char`.</span><span class="sxs-lookup"><span data-stu-id="df7d3-192">There are no implicit conversions from other types to the `char` type.</span></span> <span data-ttu-id="df7d3-193">Zejména, i když `sbyte`, `byte`a `ushort` typy mají rozsah hodnot, které jsou plně reprezentovatelné pomocí `char`ho typu, implicitní převody z `sbyte`, `byte`nebo `ushort` na `char` neexistuje.</span><span class="sxs-lookup"><span data-stu-id="df7d3-193">In particular, even though the `sbyte`, `byte`, and `ushort` types have ranges of values that are fully representable using the `char` type, implicit conversions from `sbyte`, `byte`, or `ushort` to `char` do not exist.</span></span>
*  <span data-ttu-id="df7d3-194">Konstanty `char`ho typu musí být zapsány jako *character_literal*s nebo jako *integer_literal*s v kombinaci s přetypováním na typ `char`.</span><span class="sxs-lookup"><span data-stu-id="df7d3-194">Constants of the `char` type must be written as *character_literal*s or as *integer_literal*s in combination with a cast to type `char`.</span></span> <span data-ttu-id="df7d3-195">Například `(char)10` je stejný jako `'\x000A'`.</span><span class="sxs-lookup"><span data-stu-id="df7d3-195">For example, `(char)10` is the same as `'\x000A'`.</span></span>

<span data-ttu-id="df7d3-196">Operátory a příkazy `checked` a `unchecked` slouží k řízení kontroly přetečení pro aritmetické operace typu integrálního typu a pro převody ([kontrolované a nezaškrtnuté operátory](expressions.md#the-checked-and-unchecked-operators)).</span><span class="sxs-lookup"><span data-stu-id="df7d3-196">The `checked` and `unchecked` operators and statements are used to control overflow checking for integral-type arithmetic operations and conversions ([The checked and unchecked operators](expressions.md#the-checked-and-unchecked-operators)).</span></span> <span data-ttu-id="df7d3-197">V kontextu `checked` přetečení generuje chybu při kompilaci nebo způsobí, že `System.OverflowException` vyvoláno.</span><span class="sxs-lookup"><span data-stu-id="df7d3-197">In a `checked` context, an overflow produces a compile-time error or causes a `System.OverflowException` to be thrown.</span></span> <span data-ttu-id="df7d3-198">V kontextu `unchecked` jsou přetečení ignorovány a jakékoli bity s vysokým pořadím, které se nevejdou do cílového typu, budou zahozeny.</span><span class="sxs-lookup"><span data-stu-id="df7d3-198">In an `unchecked` context, overflows are ignored and any high-order bits that do not fit in the destination type are discarded.</span></span>

### <a name="floating-point-types"></a><span data-ttu-id="df7d3-199">Typy s plovoucí desetinnou čárkou</span><span class="sxs-lookup"><span data-stu-id="df7d3-199">Floating point types</span></span>

<span data-ttu-id="df7d3-200">C#podporuje dva typy s plovoucí desetinnou čárkou: `float` a `double`.</span><span class="sxs-lookup"><span data-stu-id="df7d3-200">C# supports two floating point types: `float` and `double`.</span></span> <span data-ttu-id="df7d3-201">Typy `float` a `double` jsou reprezentovány pomocí 32 formátů IEEE 754 s jednou přesností a 64 s jednou přesností, které poskytují následující sady hodnot:</span><span class="sxs-lookup"><span data-stu-id="df7d3-201">The `float` and `double` types are represented using the 32-bit single-precision and 64-bit double-precision IEEE 754 formats, which provide the following sets of values:</span></span>

*  <span data-ttu-id="df7d3-202">Kladná nula a záporná nula.</span><span class="sxs-lookup"><span data-stu-id="df7d3-202">Positive zero and negative zero.</span></span> <span data-ttu-id="df7d3-203">Ve většině případů se kladné nula a záporná nula chovají stejně jako jednoduchá hodnota nula, ale některé operace rozlišují mezi dvěma ([operátor dělení](expressions.md#division-operator)).</span><span class="sxs-lookup"><span data-stu-id="df7d3-203">In most situations, positive zero and negative zero behave identically as the simple value zero, but certain operations distinguish between the two ([Division operator](expressions.md#division-operator)).</span></span>
*  <span data-ttu-id="df7d3-204">Kladné nekonečno a záporné nekonečno.</span><span class="sxs-lookup"><span data-stu-id="df7d3-204">Positive infinity and negative infinity.</span></span> <span data-ttu-id="df7d3-205">Nekonečny jsou vytvářeny takovými operacemi, které vydělí nenulové číslo nulou.</span><span class="sxs-lookup"><span data-stu-id="df7d3-205">Infinities are produced by such operations as dividing a non-zero number by zero.</span></span> <span data-ttu-id="df7d3-206">Například `1.0 / 0.0` poskytne kladné nekonečno a `-1.0 / 0.0` výsledkem záporné nekonečno.</span><span class="sxs-lookup"><span data-stu-id="df7d3-206">For example, `1.0 / 0.0` yields positive infinity, and `-1.0 / 0.0` yields negative infinity.</span></span>
*  <span data-ttu-id="df7d3-207">Hodnota ***není číslo*** , často zkrácená NaN.</span><span class="sxs-lookup"><span data-stu-id="df7d3-207">The ***Not-a-Number*** value, often abbreviated NaN.</span></span> <span data-ttu-id="df7d3-208">Hodnoty NaN jsou vytvářeny pomocí neplatných operací s plovoucí desetinnou čárkou, jako je například dělení nulou nulou.</span><span class="sxs-lookup"><span data-stu-id="df7d3-208">NaNs are produced by invalid floating-point operations, such as dividing zero by zero.</span></span>
*  <span data-ttu-id="df7d3-209">Konečná sada nenulových hodnot formuláře `s * m * 2^e`, kde `s` je 1 nebo-1 a `m` a `e` určují konkrétní typ s plovoucí desetinnou čárkou: pro `float``0 < m < 2^24` a `-149 <= e <= 104`a pro `double``0 < m < 2^53` a `-1075 <= e <= 970`.</span><span class="sxs-lookup"><span data-stu-id="df7d3-209">The finite set of non-zero values of the form `s * m * 2^e`, where `s` is 1 or -1, and `m` and `e` are determined by the particular floating-point type: For `float`, `0 < m < 2^24` and `-149 <= e <= 104`, and for `double`, `0 < m < 2^53` and `-1075 <= e <= 970`.</span></span> <span data-ttu-id="df7d3-210">Denormalizovaná čísla s plovoucí desetinnou čárkou se považují za platné nenulové hodnoty.</span><span class="sxs-lookup"><span data-stu-id="df7d3-210">Denormalized floating-point numbers are considered valid non-zero values.</span></span>

<span data-ttu-id="df7d3-211">Typ `float` může reprezentovat hodnoty od přibližně `1.5 * 10^-45` po `3.4 * 10^38` s přesností 7 číslic.</span><span class="sxs-lookup"><span data-stu-id="df7d3-211">The `float` type can represent values ranging from approximately `1.5 * 10^-45` to `3.4 * 10^38` with a precision of 7 digits.</span></span>

<span data-ttu-id="df7d3-212">Typ `double` může představovat hodnoty od přibližně `5.0 * 10^-324` až po `1.7 × 10^308` s přesností 15-16 číslic.</span><span class="sxs-lookup"><span data-stu-id="df7d3-212">The `double` type can represent values ranging from approximately `5.0 * 10^-324` to `1.7 × 10^308` with a precision of 15-16 digits.</span></span>

<span data-ttu-id="df7d3-213">Pokud je jeden z operandů binárního operátoru typu s plovoucí desetinnou čárkou, pak druhý operand musí být integrálního typu nebo typu s plovoucí desetinnou čárkou a operace je vyhodnocena takto:</span><span class="sxs-lookup"><span data-stu-id="df7d3-213">If one of the operands of a binary operator is of a floating-point type, then the other operand must be of an integral type or a floating-point type, and the operation is evaluated as follows:</span></span>

*  <span data-ttu-id="df7d3-214">Pokud je jeden z operandů integrálního typu, pak je tento operand převeden na typ s plovoucí desetinnou čárkou v jiném operandu.</span><span class="sxs-lookup"><span data-stu-id="df7d3-214">If one of the operands is of an integral type, then that operand is converted to the floating-point type of the other operand.</span></span>
*  <span data-ttu-id="df7d3-215">Poté, pokud je jeden z operandů typu `double`, je druhý operand převeden na `double`, operace se provádí s minimálním `double`m rozsahem a přesností a typ výsledku je `double` (nebo `bool` pro relační operátory).</span><span class="sxs-lookup"><span data-stu-id="df7d3-215">Then, if either of the operands is of type `double`, the other operand is converted to `double`, the operation is performed using at least `double` range and precision, and the type of the result is `double` (or `bool` for the relational operators).</span></span>
*  <span data-ttu-id="df7d3-216">V opačném případě se operace provádí pomocí alespoň `float` rozsahu a přesnosti a typ výsledku je `float` (nebo `bool` pro relační operátory).</span><span class="sxs-lookup"><span data-stu-id="df7d3-216">Otherwise, the operation is performed using at least `float` range and precision, and the type of the result is `float` (or `bool` for the relational operators).</span></span>

<span data-ttu-id="df7d3-217">Operátory s plovoucí desetinnou čárkou, včetně operátorů přiřazení, nikdy nevyvolávají výjimky.</span><span class="sxs-lookup"><span data-stu-id="df7d3-217">The floating-point operators, including the assignment operators, never produce exceptions.</span></span> <span data-ttu-id="df7d3-218">Místo toho se ve výjimečných situacích operace s plovoucí desetinnou čárkou vydávají nuly, nekonečno nebo NaN, jak je popsáno níže:</span><span class="sxs-lookup"><span data-stu-id="df7d3-218">Instead, in exceptional situations, floating-point operations produce zero, infinity, or NaN, as described below:</span></span>

*  <span data-ttu-id="df7d3-219">Pokud je výsledek operace s plovoucí desetinnou čárkou příliš malý pro cílový formát, výsledek operace se bude kladná nula nebo záporná nula.</span><span class="sxs-lookup"><span data-stu-id="df7d3-219">If the result of a floating-point operation is too small for the destination format, the result of the operation becomes positive zero or negative zero.</span></span>
*  <span data-ttu-id="df7d3-220">Pokud je výsledek operace s plovoucí desetinnou čárkou příliš velký pro cílový formát, výsledkem operace bude kladné nekonečno nebo záporné nekonečno.</span><span class="sxs-lookup"><span data-stu-id="df7d3-220">If the result of a floating-point operation is too large for the destination format, the result of the operation becomes positive infinity or negative infinity.</span></span>
*  <span data-ttu-id="df7d3-221">Pokud je operace s plovoucí desetinnou čárkou neplatná, výsledek operace se nastaví na hodnotu NaN.</span><span class="sxs-lookup"><span data-stu-id="df7d3-221">If a floating-point operation is invalid, the result of the operation becomes NaN.</span></span>
*  <span data-ttu-id="df7d3-222">Pokud je jeden nebo oba operandy operace s plovoucí desetinnou čárkou NaN, výsledkem operace bude NaN.</span><span class="sxs-lookup"><span data-stu-id="df7d3-222">If one or both operands of a floating-point operation is NaN, the result of the operation becomes NaN.</span></span>

<span data-ttu-id="df7d3-223">Operace s plovoucí desetinnou čárkou mohou být provedeny s větší přesností než typ výsledku operace.</span><span class="sxs-lookup"><span data-stu-id="df7d3-223">Floating-point operations may be performed with higher precision than the result type of the operation.</span></span> <span data-ttu-id="df7d3-224">Například některé hardwarové architektury podporují typ s plovoucí desetinnou čárkou "rozšířený" nebo "long double" s větším rozsahem a přesností než `double` typ a implicitně provádějí všechny operace s plovoucí desetinnou čárkou, které používají tento typ vyšší přesnosti.</span><span class="sxs-lookup"><span data-stu-id="df7d3-224">For example, some hardware architectures support an "extended" or "long double" floating-point type with greater range and precision than the `double` type, and implicitly perform all floating-point operations using this higher precision type.</span></span> <span data-ttu-id="df7d3-225">K provádění operací s plovoucí desetinnou čárkou s menší přesností můžou být takové hardwarové architektury prováděné jenom za nadměrných nákladů a místo toho, aby implementace napadly výkon i C# přesnost, umožňovaly vyšší typ přesnosti. bude použito pro všechny operace s plovoucí desetinnou čárkou.</span><span class="sxs-lookup"><span data-stu-id="df7d3-225">Only at excessive cost in performance can such hardware architectures be made to perform floating-point operations with less precision, and rather than require an implementation to forfeit both performance and precision, C# allows a higher precision type to be used for all floating-point operations.</span></span> <span data-ttu-id="df7d3-226">Kromě dodávání přesnější výsledků má tato zřídka jakékoli měřitelné účinky.</span><span class="sxs-lookup"><span data-stu-id="df7d3-226">Other than delivering more precise results, this rarely has any measurable effects.</span></span> <span data-ttu-id="df7d3-227">Nicméně ve výrazech formuláře `x * y / z`, kde násobení vytvoří výsledek, který je mimo `double` rozsah, ale další dělení vrátí dočasný výsledek zpět do `double` rozsahu, fakt, že výraz je vyhodnocen v Formát většího rozsahu může způsobit, že se vytvoří konečný výsledek namísto nekonečna.</span><span class="sxs-lookup"><span data-stu-id="df7d3-227">However, in expressions of the form `x * y / z`, where the multiplication produces a result that is outside the `double` range, but the subsequent division brings the temporary result back into the `double` range, the fact that the expression is evaluated in a higher range format may cause a finite result to be produced instead of an infinity.</span></span>

### <a name="the-decimal-type"></a><span data-ttu-id="df7d3-228">Typ Decimal</span><span class="sxs-lookup"><span data-stu-id="df7d3-228">The decimal type</span></span>

<span data-ttu-id="df7d3-229">Typ `decimal` je 128 datový typ, který je vhodný pro finanční a peněžní výpočty.</span><span class="sxs-lookup"><span data-stu-id="df7d3-229">The `decimal` type is a 128-bit data type suitable for financial and monetary calculations.</span></span> <span data-ttu-id="df7d3-230">Typ `decimal` může představovat hodnoty od `1.0 * 10^-28` po přibližně `7.9 * 10^28` s 28-29mi platnými číslicemi.</span><span class="sxs-lookup"><span data-stu-id="df7d3-230">The `decimal` type can represent values ranging from `1.0 * 10^-28` to approximately `7.9 * 10^28` with 28-29 significant digits.</span></span>

<span data-ttu-id="df7d3-231">Konečná sada hodnot typu `decimal` má formu `(-1)^s * c * 10^-e`, kde `s` znaménka je 0 nebo 1, `c` koeficient je dán `0 <= *c* < 2^96`a `e` škálování je takové, `0 <= e <= 28`. Typ `decimal` nepodporuje podepsané nuly, nekonečny nebo NaN.</span><span class="sxs-lookup"><span data-stu-id="df7d3-231">The finite set of values of type `decimal` are of the form `(-1)^s * c * 10^-e`, where the sign `s` is 0 or 1, the coefficient `c` is given by `0 <= *c* < 2^96`, and the scale `e` is such that `0 <= e <= 28`.The `decimal` type does not support signed zeros, infinities, or NaN's.</span></span> <span data-ttu-id="df7d3-232">`decimal` je reprezentován jako 96 celé číslo, které se škáluje mocninou deseti.</span><span class="sxs-lookup"><span data-stu-id="df7d3-232">A `decimal` is represented as a 96-bit integer scaled by a power of ten.</span></span> <span data-ttu-id="df7d3-233">Pro `decimal`s absolutní hodnotou menší než `1.0m`je hodnota přesně na 28 desetinné místo, ale ještě ne.</span><span class="sxs-lookup"><span data-stu-id="df7d3-233">For `decimal`s with an absolute value less than `1.0m`, the value is exact to the 28th decimal place, but no further.</span></span> <span data-ttu-id="df7d3-234">Pro `decimal`s absolutní hodnotou větší nebo rovnou `1.0m`je hodnota přesně na 28 nebo 29 číslic.</span><span class="sxs-lookup"><span data-stu-id="df7d3-234">For `decimal`s with an absolute value greater than or equal to `1.0m`, the value is exact to 28 or 29 digits.</span></span> <span data-ttu-id="df7d3-235">V rozporu s datovými typy `float` a `double` mohou být desítkové zlomkové hodnoty, například 0,1, zobrazeny přesně v `decimal` reprezentaci.</span><span class="sxs-lookup"><span data-stu-id="df7d3-235">Contrary to the `float` and `double` data types, decimal fractional numbers such as 0.1 can be represented exactly in the `decimal` representation.</span></span> <span data-ttu-id="df7d3-236">V `float` a `double` reprezentace jsou taková čísla často nekonečné zlomky, takže tyto reprezentace jsou lépe náchylné k chybám zaokrouhlení.</span><span class="sxs-lookup"><span data-stu-id="df7d3-236">In the `float` and `double` representations, such numbers are often infinite fractions, making those representations more prone to round-off errors.</span></span>

<span data-ttu-id="df7d3-237">Pokud je jeden z operandů binárního operátoru typu `decimal`, pak druhý operand musí být integrálního typu nebo typu `decimal`.</span><span class="sxs-lookup"><span data-stu-id="df7d3-237">If one of the operands of a binary operator is of type `decimal`, then the other operand must be of an integral type or of type `decimal`.</span></span> <span data-ttu-id="df7d3-238">Pokud je přítomen operand integrálního typu, je převedena na `decimal` před provedením operace.</span><span class="sxs-lookup"><span data-stu-id="df7d3-238">If an integral type operand is present, it is converted to `decimal` before the operation is performed.</span></span>

<span data-ttu-id="df7d3-239">Výsledkem operace s hodnotami typu `decimal` je to, což by vedlo k výpočtu přesného výsledku (zachovávání stupnice, jak je definováno pro každý operátor) a pak zaokrouhlování podle reprezentace.</span><span class="sxs-lookup"><span data-stu-id="df7d3-239">The result of an operation on values of type `decimal` is that which would result from calculating an exact result (preserving scale, as defined for each operator) and then rounding to fit the representation.</span></span> <span data-ttu-id="df7d3-240">Výsledky se zaokrouhlují na nejbližší možnou hodnotu a v případě, že se výsledek rovnoměrně blíží dvěma hodnotám, na který má sudé číslo v nejméně významném umístění číslice (označuje se jako "zaokrouhlení bank").</span><span class="sxs-lookup"><span data-stu-id="df7d3-240">Results are rounded to the nearest representable value, and, when a result is equally close to two representable values, to the value that has an even number in the least significant digit position (this is known as "banker's rounding").</span></span> <span data-ttu-id="df7d3-241">Nulový výsledek vždy má znak 0 a měřítko 0.</span><span class="sxs-lookup"><span data-stu-id="df7d3-241">A zero result always has a sign of 0 and a scale of 0.</span></span>

<span data-ttu-id="df7d3-242">Pokud Desítková aritmetická operace vytvoří hodnotu menší nebo rovnou `5 * 10^-29` v absolutní hodnotě, výsledek operace se rovná nule.</span><span class="sxs-lookup"><span data-stu-id="df7d3-242">If a decimal arithmetic operation produces a value less than or equal to `5 * 10^-29` in absolute value, the result of the operation becomes zero.</span></span> <span data-ttu-id="df7d3-243">Pokud `decimal` aritmetické operace vytvoří výsledek, který je příliš velký pro formát `decimal`, je vyvolána `System.OverflowException`.</span><span class="sxs-lookup"><span data-stu-id="df7d3-243">If a `decimal` arithmetic operation produces a result that is too large for the `decimal` format, a `System.OverflowException` is thrown.</span></span>

<span data-ttu-id="df7d3-244">Typ `decimal` má větší přesnost, ale menší rozsah než typy s plovoucí desetinnou čárkou.</span><span class="sxs-lookup"><span data-stu-id="df7d3-244">The `decimal` type has greater precision but smaller range than the floating-point types.</span></span> <span data-ttu-id="df7d3-245">Proto převody z typů s plovoucí desetinnou čárkou na `decimal` mohou generovat výjimky přetečení a převody z `decimal` na typy s plovoucí desetinnou čárkou mohou způsobit ztrátu přesnosti.</span><span class="sxs-lookup"><span data-stu-id="df7d3-245">Thus, conversions from the floating-point types to `decimal` might produce overflow exceptions, and conversions from `decimal` to the floating-point types might cause loss of precision.</span></span> <span data-ttu-id="df7d3-246">Z těchto důvodů neexistují žádné implicitní převody mezi typy s plovoucí desetinnou čárkou a `decimal`a bez explicitního přetypování, není možné ve stejném výrazu kombinovat operandy s plovoucí desetinnou čárkou a `decimal`.</span><span class="sxs-lookup"><span data-stu-id="df7d3-246">For these reasons, no implicit conversions exist between the floating-point types and `decimal`, and without explicit casts, it is not possible to mix floating-point and `decimal` operands in the same expression.</span></span>

### <a name="the-bool-type"></a><span data-ttu-id="df7d3-247">Typ bool</span><span class="sxs-lookup"><span data-stu-id="df7d3-247">The bool type</span></span>

<span data-ttu-id="df7d3-248">Typ `bool` představuje logické logické množství.</span><span class="sxs-lookup"><span data-stu-id="df7d3-248">The `bool` type represents boolean logical quantities.</span></span> <span data-ttu-id="df7d3-249">Možné hodnoty typu `bool` jsou `true` a `false`.</span><span class="sxs-lookup"><span data-stu-id="df7d3-249">The possible values of type `bool` are `true` and `false`.</span></span>

<span data-ttu-id="df7d3-250">Mezi `bool` a jinými typy neexistují žádné standardní převody.</span><span class="sxs-lookup"><span data-stu-id="df7d3-250">No standard conversions exist between `bool` and other types.</span></span> <span data-ttu-id="df7d3-251">Konkrétně je typ `bool` jedinečný a oddělený od integrálních typů a hodnotu `bool` nelze použít místo celočíselné hodnoty a naopak.</span><span class="sxs-lookup"><span data-stu-id="df7d3-251">In particular, the `bool` type is distinct and separate from the integral types, and a `bool` value cannot be used in place of an integral value, and vice versa.</span></span>

<span data-ttu-id="df7d3-252">V jazycích C a C++ , nulových integrálních hodnot nebo hodnot s plovoucí desetinnou čárkou, nebo ukazatel s hodnotou null lze převést na logickou hodnotu `false`a nenulovou hodnotu integrálu nebo hodnoty s plovoucí desetinnou čárkou, nebo ukazatel, který není null, lze převést na logickou hodnotu `true`.</span><span class="sxs-lookup"><span data-stu-id="df7d3-252">In the C and C++ languages, a zero integral or floating-point value, or a null pointer can be converted to the boolean value `false`, and a non-zero integral or floating-point value, or a non-null pointer can be converted to the boolean value `true`.</span></span> <span data-ttu-id="df7d3-253">V C#je tyto převody provedeny explicitním porovnáním hodnoty integrální nebo plovoucí desetinné čárky na nulu nebo explicitním porovnáním odkazu na objekt `null`.</span><span class="sxs-lookup"><span data-stu-id="df7d3-253">In C#, such conversions are accomplished by explicitly comparing an integral or floating-point value to zero, or by explicitly comparing an object reference to `null`.</span></span>

### <a name="enumeration-types"></a><span data-ttu-id="df7d3-254">Výčtové typy</span><span class="sxs-lookup"><span data-stu-id="df7d3-254">Enumeration types</span></span>

<span data-ttu-id="df7d3-255">Typ výčtu je odlišný typ s pojmenovanými konstantami.</span><span class="sxs-lookup"><span data-stu-id="df7d3-255">An enumeration type is a distinct type with named constants.</span></span> <span data-ttu-id="df7d3-256">Každý typ výčtu má základní typ, který musí být `byte`, `sbyte`, `short`, `ushort`, `int`, `uint`, `long` nebo `ulong`.</span><span class="sxs-lookup"><span data-stu-id="df7d3-256">Every enumeration type has an underlying type, which must be `byte`, `sbyte`, `short`, `ushort`, `int`, `uint`, `long` or `ulong`.</span></span> <span data-ttu-id="df7d3-257">Sada hodnot typu výčtu je stejná jako sada hodnot základního typu.</span><span class="sxs-lookup"><span data-stu-id="df7d3-257">The set of values of the enumeration type is the same as the set of values of the underlying type.</span></span> <span data-ttu-id="df7d3-258">Hodnoty typu výčtu nejsou omezeny na hodnoty s názvem konstanty.</span><span class="sxs-lookup"><span data-stu-id="df7d3-258">Values of the enumeration type are not restricted to the values of the named constants.</span></span> <span data-ttu-id="df7d3-259">Výčtové typy jsou definovány prostřednictvím deklarací výčtu ([deklarace výčtu](enums.md#enum-declarations)).</span><span class="sxs-lookup"><span data-stu-id="df7d3-259">Enumeration types are defined through enumeration declarations ([Enum declarations](enums.md#enum-declarations)).</span></span>

### <a name="nullable-types"></a><span data-ttu-id="df7d3-260">Typy s povolenou hodnotou Null</span><span class="sxs-lookup"><span data-stu-id="df7d3-260">Nullable types</span></span>

<span data-ttu-id="df7d3-261">Typ s možnou hodnotou null může představovat všechny hodnoty svého ***nadřízeného typu*** a další hodnotu null.</span><span class="sxs-lookup"><span data-stu-id="df7d3-261">A nullable type can represent all values of its ***underlying type*** plus an additional null value.</span></span> <span data-ttu-id="df7d3-262">Typ s možnou hodnotou null je napsaný `T?`, kde `T` je základní typ.</span><span class="sxs-lookup"><span data-stu-id="df7d3-262">A nullable type is written `T?`, where `T` is the underlying type.</span></span> <span data-ttu-id="df7d3-263">Tato syntaxe je zkrácený pro `System.Nullable<T>`a dva formuláře lze zaměnit.</span><span class="sxs-lookup"><span data-stu-id="df7d3-263">This syntax is shorthand for `System.Nullable<T>`, and the two forms can be used interchangeably.</span></span>

<span data-ttu-id="df7d3-264">***Typ hodnoty*** , která není null, je naopak libovolný typ hodnoty jiný než `System.Nullable<T>` a jeho zkrácený `T?` (pro všechny `T`) a jakýkoli parametr typu, který je omezený na typ hodnoty, která není null (to znamená, že libovolný parametr typu s `struct` omezení).</span><span class="sxs-lookup"><span data-stu-id="df7d3-264">A ***non-nullable value type*** conversely is any value type other than `System.Nullable<T>` and its shorthand `T?` (for any `T`), plus any type parameter that is constrained to be a non-nullable value type (that is, any type parameter with a `struct` constraint).</span></span> <span data-ttu-id="df7d3-265">Typ `System.Nullable<T>` určuje omezení typu hodnoty pro `T` ([omezení parametrů typu](classes.md#type-parameter-constraints)), což znamená, že nadřízený typ typu s možnou hodnotou null může být typ hodnoty, která není null.</span><span class="sxs-lookup"><span data-stu-id="df7d3-265">The `System.Nullable<T>` type specifies the value type constraint for `T` ([Type parameter constraints](classes.md#type-parameter-constraints)), which means that the underlying type of a nullable type can be any non-nullable value type.</span></span> <span data-ttu-id="df7d3-266">Podkladový typ typu s možnou hodnotou null nemůže být typ s možnou hodnotou null nebo odkazový typ.</span><span class="sxs-lookup"><span data-stu-id="df7d3-266">The underlying type of a nullable type cannot be a nullable type or a reference type.</span></span> <span data-ttu-id="df7d3-267">Například `int??` a `string?` jsou neplatné typy.</span><span class="sxs-lookup"><span data-stu-id="df7d3-267">For example, `int??` and `string?` are invalid types.</span></span>

<span data-ttu-id="df7d3-268">Instance typu s možnou hodnotou null `T?` má dvě veřejné vlastnosti jen pro čtení:</span><span class="sxs-lookup"><span data-stu-id="df7d3-268">An instance of a nullable type `T?` has two public read-only properties:</span></span>

*  <span data-ttu-id="df7d3-269">Vlastnost `HasValue` typu `bool`</span><span class="sxs-lookup"><span data-stu-id="df7d3-269">A `HasValue` property of type `bool`</span></span>
*  <span data-ttu-id="df7d3-270">Vlastnost `Value` typu `T`</span><span class="sxs-lookup"><span data-stu-id="df7d3-270">A `Value` property of type `T`</span></span>

<span data-ttu-id="df7d3-271">Instance, pro kterou je `HasValue` true, se říká, že to není null.</span><span class="sxs-lookup"><span data-stu-id="df7d3-271">An instance for which `HasValue` is true is said to be non-null.</span></span> <span data-ttu-id="df7d3-272">Instance, která není null, obsahuje známou hodnotu a `Value` tuto hodnotu vrací.</span><span class="sxs-lookup"><span data-stu-id="df7d3-272">A non-null instance contains a known value and `Value` returns that value.</span></span>

<span data-ttu-id="df7d3-273">Instance, pro kterou je `HasValue` false, je označována jako null.</span><span class="sxs-lookup"><span data-stu-id="df7d3-273">An instance for which `HasValue` is false is said to be null.</span></span> <span data-ttu-id="df7d3-274">Instance null má nedefinovanou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="df7d3-274">A null instance has an undefined value.</span></span> <span data-ttu-id="df7d3-275">Při pokusu o čtení `Value` instance null dojde k vyvolání `System.InvalidOperationException`.</span><span class="sxs-lookup"><span data-stu-id="df7d3-275">Attempting to read the `Value` of a null instance causes a `System.InvalidOperationException` to be thrown.</span></span> <span data-ttu-id="df7d3-276">Proces přístupu k vlastnosti `Value` instance s možnou hodnotou null je označován jako ***rozbalení***.</span><span class="sxs-lookup"><span data-stu-id="df7d3-276">The process of accessing the `Value` property of a nullable instance is referred to as ***unwrapping***.</span></span>

<span data-ttu-id="df7d3-277">Kromě výchozího konstruktoru každý typ s povolenou hodnotou null `T?` má veřejný konstruktor, který přijímá jeden argument typu `T`.</span><span class="sxs-lookup"><span data-stu-id="df7d3-277">In addition to the default constructor, every nullable type `T?` has a public constructor that takes a single argument of type `T`.</span></span> <span data-ttu-id="df7d3-278">Předaná hodnota `x` typu `T`, volání konstruktoru formuláře.</span><span class="sxs-lookup"><span data-stu-id="df7d3-278">Given a value `x` of type `T`, a constructor invocation of the form</span></span>

```csharp
new T?(x)
```
<span data-ttu-id="df7d3-279">Vytvoří instanci, která není null `T?` pro kterou je `x`vlastnost `Value`.</span><span class="sxs-lookup"><span data-stu-id="df7d3-279">creates a non-null instance of `T?` for which the `Value` property is `x`.</span></span> <span data-ttu-id="df7d3-280">Proces vytvoření instance s možnou hodnotou null pro danou hodnotu je označován jako ***zalamování***.</span><span class="sxs-lookup"><span data-stu-id="df7d3-280">The process of creating a non-null instance of a nullable type for a given value is referred to as ***wrapping***.</span></span>

<span data-ttu-id="df7d3-281">Implicitní převody jsou k dispozici z `null`ho literálu pro `T?` ([převody literálů s hodnotou null](conversions.md#null-literal-conversions)) a z `T` na `T?` ([implicitní převody s možnou hodnotou null](conversions.md#implicit-nullable-conversions)).</span><span class="sxs-lookup"><span data-stu-id="df7d3-281">Implicit conversions are available from the `null` literal to `T?` ([Null literal conversions](conversions.md#null-literal-conversions)) and from `T` to `T?` ([Implicit nullable conversions](conversions.md#implicit-nullable-conversions)).</span></span>

## <a name="reference-types"></a><span data-ttu-id="df7d3-282">Odkazové typy</span><span class="sxs-lookup"><span data-stu-id="df7d3-282">Reference types</span></span>

<span data-ttu-id="df7d3-283">Odkazový typ je typ třídy, typ rozhraní, typ pole nebo typ delegáta.</span><span class="sxs-lookup"><span data-stu-id="df7d3-283">A reference type is a class type, an interface type, an array type, or a delegate type.</span></span>

```antlr
reference_type
    : class_type
    | interface_type
    | array_type
    | delegate_type
    ;

class_type
    : type_name
    | 'object'
    | 'dynamic'
    | 'string'
    ;

interface_type
    : type_name
    ;

array_type
    : non_array_type rank_specifier+
    ;

non_array_type
    : type
    ;

rank_specifier
    : '[' dim_separator* ']'
    ;

dim_separator
    : ','
    ;

delegate_type
    : type_name
    ;
```

<span data-ttu-id="df7d3-284">Hodnota typu odkazu je odkaz na ***instanci*** typu, druhá se nazývá ***objekt***.</span><span class="sxs-lookup"><span data-stu-id="df7d3-284">A reference type value is a reference to an ***instance*** of the type, the latter known as an ***object***.</span></span> <span data-ttu-id="df7d3-285">Speciální hodnota `null` je kompatibilní se všemi typy odkazů a označuje absenci instance.</span><span class="sxs-lookup"><span data-stu-id="df7d3-285">The special value `null` is compatible with all reference types and indicates the absence of an instance.</span></span>

### <a name="class-types"></a><span data-ttu-id="df7d3-286">Typy tříd</span><span class="sxs-lookup"><span data-stu-id="df7d3-286">Class types</span></span>

<span data-ttu-id="df7d3-287">Typ třídy definuje strukturu dat, která obsahuje datové členy (konstanty a pole), členy funkce (metody, vlastnosti, události, indexery, operátory, konstruktory instancí, destruktory a statické konstruktory) a vnořené typy.</span><span class="sxs-lookup"><span data-stu-id="df7d3-287">A class type defines a data structure that contains data members (constants and fields), function members (methods, properties, events, indexers, operators, instance constructors, destructors and static constructors), and nested types.</span></span> <span data-ttu-id="df7d3-288">Typy tříd podporují dědičnost, mechanismus, který odvozené třídy mohou roztáhnout a specializovat základní třídy.</span><span class="sxs-lookup"><span data-stu-id="df7d3-288">Class types support inheritance, a mechanism whereby derived classes can extend and specialize base classes.</span></span> <span data-ttu-id="df7d3-289">Instance typů tříd jsou vytvořeny pomocí *object_creation_expression*s ([výrazy pro vytváření objektů](expressions.md#object-creation-expressions)).</span><span class="sxs-lookup"><span data-stu-id="df7d3-289">Instances of class types are created using *object_creation_expression*s ([Object creation expressions](expressions.md#object-creation-expressions)).</span></span>

<span data-ttu-id="df7d3-290">Typy tříd jsou popsány v tématu [třídy](classes.md).</span><span class="sxs-lookup"><span data-stu-id="df7d3-290">Class types are described in [Classes](classes.md).</span></span>

<span data-ttu-id="df7d3-291">Některé předdefinované typy tříd mají v C# jazyce zvláštní význam, jak je popsáno v následující tabulce.</span><span class="sxs-lookup"><span data-stu-id="df7d3-291">Certain predefined class types have special meaning in the C# language, as described in the table below.</span></span>


| <span data-ttu-id="df7d3-292">__Typ třídy__</span><span class="sxs-lookup"><span data-stu-id="df7d3-292">__Class type__</span></span>     | <span data-ttu-id="df7d3-293">__Popis__</span><span class="sxs-lookup"><span data-stu-id="df7d3-293">__Description__</span></span>                                         |
|--------------------|---------------------------------------------------------|
| `System.Object`    | <span data-ttu-id="df7d3-294">Nejvyšší základní třída všech ostatních typů.</span><span class="sxs-lookup"><span data-stu-id="df7d3-294">The ultimate base class of all other types.</span></span> <span data-ttu-id="df7d3-295">Podívejte [se na typ objektu](types.md#the-object-type).</span><span class="sxs-lookup"><span data-stu-id="df7d3-295">See [The object type](types.md#the-object-type).</span></span> | 
| `System.String`    | <span data-ttu-id="df7d3-296">Typ řetězce C# jazyka</span><span class="sxs-lookup"><span data-stu-id="df7d3-296">The string type of the C# language.</span></span> <span data-ttu-id="df7d3-297">Podívejte [se na typ řetězce](types.md#the-string-type).</span><span class="sxs-lookup"><span data-stu-id="df7d3-297">See [The string type](types.md#the-string-type).</span></span>         |
| `System.ValueType` | <span data-ttu-id="df7d3-298">Základní třída všech typů hodnot.</span><span class="sxs-lookup"><span data-stu-id="df7d3-298">The base class of all value types.</span></span> <span data-ttu-id="df7d3-299">Podívejte [se na typ System. ValueType](types.md#the-systemvaluetype-type).</span><span class="sxs-lookup"><span data-stu-id="df7d3-299">See [The System.ValueType type](types.md#the-systemvaluetype-type).</span></span>          |
| `System.Enum`      | <span data-ttu-id="df7d3-300">Základní třída všech typů výčtu.</span><span class="sxs-lookup"><span data-stu-id="df7d3-300">The base class of all enum types.</span></span> <span data-ttu-id="df7d3-301">Viz [výčty](enums.md).</span><span class="sxs-lookup"><span data-stu-id="df7d3-301">See [Enums](enums.md).</span></span>              |
| `System.Array`     | <span data-ttu-id="df7d3-302">Základní třída všech typů polí.</span><span class="sxs-lookup"><span data-stu-id="df7d3-302">The base class of all array types.</span></span> <span data-ttu-id="df7d3-303">Viz [pole](arrays.md).</span><span class="sxs-lookup"><span data-stu-id="df7d3-303">See [Arrays](arrays.md).</span></span>             |
| `System.Delegate`  | <span data-ttu-id="df7d3-304">Základní třída všech typů delegátů.</span><span class="sxs-lookup"><span data-stu-id="df7d3-304">The base class of all delegate types.</span></span> <span data-ttu-id="df7d3-305">Viz [Delegáti](delegates.md).</span><span class="sxs-lookup"><span data-stu-id="df7d3-305">See [Delegates](delegates.md).</span></span>          |
| `System.Exception` | <span data-ttu-id="df7d3-306">Základní třída všech typů výjimek.</span><span class="sxs-lookup"><span data-stu-id="df7d3-306">The base class of all exception types.</span></span> <span data-ttu-id="df7d3-307">Viz [výjimky](exceptions.md).</span><span class="sxs-lookup"><span data-stu-id="df7d3-307">See [Exceptions](exceptions.md).</span></span>         |

### <a name="the-object-type"></a><span data-ttu-id="df7d3-308">Typ objektu</span><span class="sxs-lookup"><span data-stu-id="df7d3-308">The object type</span></span>

<span data-ttu-id="df7d3-309">Typ třídy `object` je nejvyšší základní třídou všech ostatních typů.</span><span class="sxs-lookup"><span data-stu-id="df7d3-309">The `object` class type is the ultimate base class of all other types.</span></span> <span data-ttu-id="df7d3-310">Každý typ C# přímo nebo nepřímo je odvozen z typu `object` třídy.</span><span class="sxs-lookup"><span data-stu-id="df7d3-310">Every type in C# directly or indirectly derives from the `object` class type.</span></span>

<span data-ttu-id="df7d3-311">Klíčové slovo `object` je pouze alias pro předdefinovaný `System.Object`třídy.</span><span class="sxs-lookup"><span data-stu-id="df7d3-311">The keyword `object` is simply an alias for the predefined class `System.Object`.</span></span>

### <a name="the-dynamic-type"></a><span data-ttu-id="df7d3-312">Dynamický typ</span><span class="sxs-lookup"><span data-stu-id="df7d3-312">The dynamic type</span></span>

<span data-ttu-id="df7d3-313">Typ `dynamic`, jako je `object`, může odkazovat na libovolný objekt.</span><span class="sxs-lookup"><span data-stu-id="df7d3-313">The `dynamic` type, like `object`, can reference any object.</span></span> <span data-ttu-id="df7d3-314">Pokud jsou operátory aplikovány na výrazy typu `dynamic`, jejich rozlišení je odloženo, dokud se program nespustí.</span><span class="sxs-lookup"><span data-stu-id="df7d3-314">When operators are applied to expressions of type `dynamic`, their resolution is deferred until the program is run.</span></span> <span data-ttu-id="df7d3-315">Proto, pokud operátor nemůže být právně použit na odkazovaný objekt, není během kompilace uvedena žádná chyba.</span><span class="sxs-lookup"><span data-stu-id="df7d3-315">Thus, if the operator cannot legally be applied to the referenced object, no error is given during compilation.</span></span> <span data-ttu-id="df7d3-316">Namísto toho dojde k výjimce, pokud překlad operátoru selže v době běhu.</span><span class="sxs-lookup"><span data-stu-id="df7d3-316">Instead an exception will be thrown when resolution of the operator fails at run-time.</span></span>

<span data-ttu-id="df7d3-317">Jeho účelem je umožnění dynamické vazby, která je podrobněji popsána v tématu [dynamická vazba](expressions.md#dynamic-binding).</span><span class="sxs-lookup"><span data-stu-id="df7d3-317">Its purpose is to allow dynamic binding, which is described in detail in [Dynamic binding](expressions.md#dynamic-binding).</span></span>

<span data-ttu-id="df7d3-318">`dynamic` se považuje za identické s `object` s výjimkou následujících hledisek:</span><span class="sxs-lookup"><span data-stu-id="df7d3-318">`dynamic` is considered identical to `object` except in the following respects:</span></span>

*  <span data-ttu-id="df7d3-319">Operace s výrazy typu `dynamic` můžou být dynamicky vázané ([dynamická vazba](expressions.md#dynamic-binding)).</span><span class="sxs-lookup"><span data-stu-id="df7d3-319">Operations on expressions of type `dynamic` can be dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)).</span></span>
*  <span data-ttu-id="df7d3-320">Odvození typu ([odvození typu](expressions.md#type-inference)) upřednostňuje `dynamic` přes `object`, pokud jsou oba kandidáti.</span><span class="sxs-lookup"><span data-stu-id="df7d3-320">Type inference ([Type inference](expressions.md#type-inference)) will prefer `dynamic` over `object` if both are candidates.</span></span>

<span data-ttu-id="df7d3-321">Z důvodu této ekvivalence jsou následující:</span><span class="sxs-lookup"><span data-stu-id="df7d3-321">Because of this equivalence, the following holds:</span></span>

*  <span data-ttu-id="df7d3-322">Existuje implicitní převod identity mezi `object` a `dynamic`a mezi konstruovanými typy, které jsou stejné při nahrazování `dynamic` s `object`</span><span class="sxs-lookup"><span data-stu-id="df7d3-322">There is an implicit identity conversion between `object` and `dynamic`, and between constructed types that are the same when replacing `dynamic` with `object`</span></span>
*  <span data-ttu-id="df7d3-323">Implicitní a explicitní převody na a z `object` vztahují také na a z `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="df7d3-323">Implicit and explicit conversions to and from `object` also apply to and from `dynamic`.</span></span>
*  <span data-ttu-id="df7d3-324">Signatury metod, které jsou stejné při nahrazování `dynamic` s `object`, se považují za stejnou signaturu.</span><span class="sxs-lookup"><span data-stu-id="df7d3-324">Method signatures that are the same when replacing `dynamic` with `object` are considered the same signature</span></span>
*  <span data-ttu-id="df7d3-325">Typ `dynamic` nelze odlišit od `object` v době běhu.</span><span class="sxs-lookup"><span data-stu-id="df7d3-325">The type `dynamic` is indistinguishable from `object` at run-time.</span></span>
*  <span data-ttu-id="df7d3-326">Výraz typu `dynamic` je označován jako ***dynamický výraz***.</span><span class="sxs-lookup"><span data-stu-id="df7d3-326">An expression of the type `dynamic` is referred to as a ***dynamic expression***.</span></span>

### <a name="the-string-type"></a><span data-ttu-id="df7d3-327">Typ řetězce</span><span class="sxs-lookup"><span data-stu-id="df7d3-327">The string type</span></span>

<span data-ttu-id="df7d3-328">Typ `string` je zapečetěný typ třídy, který dědí přímo z `object`.</span><span class="sxs-lookup"><span data-stu-id="df7d3-328">The `string` type is a sealed class type that inherits directly from `object`.</span></span> <span data-ttu-id="df7d3-329">Instance `string` třídy reprezentují řetězce znaků Unicode.</span><span class="sxs-lookup"><span data-stu-id="df7d3-329">Instances of the `string` class represent Unicode character strings.</span></span>

<span data-ttu-id="df7d3-330">Hodnoty typu `string` lze zapsat jako řetězcové literály ([řetězcové literály](lexical-structure.md#string-literals)).</span><span class="sxs-lookup"><span data-stu-id="df7d3-330">Values of the `string` type can be written as string literals ([String literals](lexical-structure.md#string-literals)).</span></span>

<span data-ttu-id="df7d3-331">Klíčové slovo `string` je pouze alias pro předdefinovaný `System.String`třídy.</span><span class="sxs-lookup"><span data-stu-id="df7d3-331">The keyword `string` is simply an alias for the predefined class `System.String`.</span></span>

### <a name="interface-types"></a><span data-ttu-id="df7d3-332">Typy rozhraní</span><span class="sxs-lookup"><span data-stu-id="df7d3-332">Interface types</span></span>

<span data-ttu-id="df7d3-333">Rozhraní definuje kontrakt.</span><span class="sxs-lookup"><span data-stu-id="df7d3-333">An interface defines a contract.</span></span> <span data-ttu-id="df7d3-334">Třída nebo struktura, která implementuje rozhraní, musí vyhovovat jeho kontraktu.</span><span class="sxs-lookup"><span data-stu-id="df7d3-334">A class or struct that implements an interface must adhere to its contract.</span></span> <span data-ttu-id="df7d3-335">Rozhraní může dědit z více základních rozhraní a třída nebo struktura může implementovat více rozhraní.</span><span class="sxs-lookup"><span data-stu-id="df7d3-335">An interface may inherit from multiple base interfaces, and a class or struct may implement multiple interfaces.</span></span>

<span data-ttu-id="df7d3-336">Typy rozhraní jsou popsány v tématu [rozhraní](interfaces.md).</span><span class="sxs-lookup"><span data-stu-id="df7d3-336">Interface types are described in [Interfaces](interfaces.md).</span></span>

### <a name="array-types"></a><span data-ttu-id="df7d3-337">Typy polí</span><span class="sxs-lookup"><span data-stu-id="df7d3-337">Array types</span></span>

<span data-ttu-id="df7d3-338">Pole je datová struktura, která obsahuje nula nebo více proměnných, které jsou k dispozici prostřednictvím vypočítaných indexů.</span><span class="sxs-lookup"><span data-stu-id="df7d3-338">An array is a data structure that contains zero or more variables which are accessed through computed indices.</span></span> <span data-ttu-id="df7d3-339">Proměnné obsažené v poli, označované také jako prvky pole, jsou všechny stejného typu a tento typ se nazývá typ elementu pole.</span><span class="sxs-lookup"><span data-stu-id="df7d3-339">The variables contained in an array, also called the elements of the array, are all of the same type, and this type is called the element type of the array.</span></span>

<span data-ttu-id="df7d3-340">Typy polí jsou popsány v [polích](arrays.md).</span><span class="sxs-lookup"><span data-stu-id="df7d3-340">Array types are described in [Arrays](arrays.md).</span></span>

### <a name="delegate-types"></a><span data-ttu-id="df7d3-341">Typy delegátů</span><span class="sxs-lookup"><span data-stu-id="df7d3-341">Delegate types</span></span>

<span data-ttu-id="df7d3-342">Delegát je datová struktura, která odkazuje na jednu nebo více metod.</span><span class="sxs-lookup"><span data-stu-id="df7d3-342">A delegate is a data structure that refers to one or more methods.</span></span> <span data-ttu-id="df7d3-343">Pro metody instance také odkazuje na odpovídající instance objektů.</span><span class="sxs-lookup"><span data-stu-id="df7d3-343">For instance methods, it also refers to their corresponding object instances.</span></span>

<span data-ttu-id="df7d3-344">Nejbližší ekvivalent delegáta v jazyce C nebo C++ je ukazatel na funkci, ale zatímco ukazatel funkce může odkazovat pouze na statické funkce, delegát může odkazovat jak na statické, tak na metody instance.</span><span class="sxs-lookup"><span data-stu-id="df7d3-344">The closest equivalent of a delegate in C or C++ is a function pointer, but whereas a function pointer can only reference static functions, a delegate can reference both static and instance methods.</span></span> <span data-ttu-id="df7d3-345">V druhém případě delegát ukládá nejen odkaz na vstupní bod metody, ale také odkaz na instanci objektu, na které se má metoda vyvolat.</span><span class="sxs-lookup"><span data-stu-id="df7d3-345">In the latter case, the delegate stores not only a reference to the method's entry point, but also a reference to the object instance on which to invoke the method.</span></span>

<span data-ttu-id="df7d3-346">Typy delegátů jsou popsány v tématu [Delegáti](delegates.md).</span><span class="sxs-lookup"><span data-stu-id="df7d3-346">Delegate types are described in [Delegates](delegates.md).</span></span>

## <a name="boxing-and-unboxing"></a><span data-ttu-id="df7d3-347">Zabalení a rozbalení</span><span class="sxs-lookup"><span data-stu-id="df7d3-347">Boxing and unboxing</span></span>

<span data-ttu-id="df7d3-348">Pojem zabalení a rozbalení je od centrálního C#typu systém.</span><span class="sxs-lookup"><span data-stu-id="df7d3-348">The concept of boxing and unboxing is central to C#'s type system.</span></span> <span data-ttu-id="df7d3-349">Poskytuje most mezi *value_type*a a *reference_type*s tím, že umožňuje převod jakékoli hodnoty *value_type* na typ a z typu `object`.</span><span class="sxs-lookup"><span data-stu-id="df7d3-349">It provides a bridge between *value_type*s and *reference_type*s by permitting any value of a *value_type* to be converted to and from type `object`.</span></span> <span data-ttu-id="df7d3-350">Zabalení a rozbalení umožňuje jednotný pohled na typ systému, ve kterém je hodnota libovolného typu považována za objekt.</span><span class="sxs-lookup"><span data-stu-id="df7d3-350">Boxing and unboxing enables a unified view of the type system wherein a value of any type can ultimately be treated as an object.</span></span>

### <a name="boxing-conversions"></a><span data-ttu-id="df7d3-351">Převody zabalení</span><span class="sxs-lookup"><span data-stu-id="df7d3-351">Boxing conversions</span></span>

<span data-ttu-id="df7d3-352">Převod zabalení umožňuje *value_type* implicitně převést na *reference_type*.</span><span class="sxs-lookup"><span data-stu-id="df7d3-352">A boxing conversion permits a *value_type* to be implicitly converted to a *reference_type*.</span></span> <span data-ttu-id="df7d3-353">Existují následující převody zabalení:</span><span class="sxs-lookup"><span data-stu-id="df7d3-353">The following boxing conversions exist:</span></span>

*  <span data-ttu-id="df7d3-354">Z libovolného *value_type* na typ `object`.</span><span class="sxs-lookup"><span data-stu-id="df7d3-354">From any *value_type* to the type `object`.</span></span>
*  <span data-ttu-id="df7d3-355">Z libovolného *value_type* na typ `System.ValueType`.</span><span class="sxs-lookup"><span data-stu-id="df7d3-355">From any *value_type* to the type `System.ValueType`.</span></span>
*  <span data-ttu-id="df7d3-356">Z libovolného *non_nullable_value_type* na libovolný *INTERFACE_TYPE* implementovaný v *value_type*.</span><span class="sxs-lookup"><span data-stu-id="df7d3-356">From any *non_nullable_value_type* to any *interface_type* implemented by the *value_type*.</span></span>
*  <span data-ttu-id="df7d3-357">Z libovolného *nullable_type* na libovolný *INTERFACE_TYPE* implementovaný podkladovým typem *nullable_type*.</span><span class="sxs-lookup"><span data-stu-id="df7d3-357">From any *nullable_type* to any *interface_type* implemented by the underlying type of the *nullable_type*.</span></span>
*  <span data-ttu-id="df7d3-358">Z libovolného *enum_type* na typ `System.Enum`.</span><span class="sxs-lookup"><span data-stu-id="df7d3-358">From any *enum_type* to the type `System.Enum`.</span></span>
*  <span data-ttu-id="df7d3-359">Z libovolného *nullable_typeu* s podkladovým *enum_type* na typ `System.Enum`.</span><span class="sxs-lookup"><span data-stu-id="df7d3-359">From any *nullable_type* with an underlying *enum_type* to the type `System.Enum`.</span></span>
*  <span data-ttu-id="df7d3-360">Všimněte si, že implicitní převod z parametru typu bude proveden jako převod zabalení, pokud v době běhu skončí převod z typu hodnoty na typ odkazu ([implicitní převody zahrnující parametry typu](conversions.md#implicit-conversions-involving-type-parameters)).</span><span class="sxs-lookup"><span data-stu-id="df7d3-360">Note that an implicit conversion from a type parameter will be executed as a boxing conversion if at run-time it ends up converting from a value type to a reference type ([Implicit conversions involving type parameters](conversions.md#implicit-conversions-involving-type-parameters)).</span></span>

<span data-ttu-id="df7d3-361">Zabalení hodnoty *non_nullable_value_type* se skládá z přidělení instance objektu a zkopírování hodnoty *non_nullable_value_type* do této instance.</span><span class="sxs-lookup"><span data-stu-id="df7d3-361">Boxing a value of a *non_nullable_value_type* consists of allocating an object instance and copying the *non_nullable_value_type* value into that instance.</span></span>

<span data-ttu-id="df7d3-362">Zabalením hodnoty *nullable_type* se vytvoří odkaz s hodnotou null, pokud se jedná o hodnotu `null` (`HasValue` je `false`), nebo výsledek rozbalení a zabalení podkladové hodnoty v opačném případě.</span><span class="sxs-lookup"><span data-stu-id="df7d3-362">Boxing a value of a *nullable_type* produces a null reference if it is the `null` value (`HasValue` is `false`), or the result of unwrapping and boxing the underlying value otherwise.</span></span>

<span data-ttu-id="df7d3-363">Skutečný proces zabalení hodnoty *non_nullable_value_type* je nejlépe vysvětleno tím, že se porozumí existence obecné ***třídy zabalení***, která se chová, jako by byla deklarována takto:</span><span class="sxs-lookup"><span data-stu-id="df7d3-363">The actual process of boxing a value of a *non_nullable_value_type* is best explained by imagining the existence of a generic ***boxing class***, which behaves as if it were declared as follows:</span></span>

```csharp
sealed class Box<T>: System.ValueType
{
    T value;

    public Box(T t) {
        value = t;
    }
}
```

<span data-ttu-id="df7d3-364">Zabalení hodnoty `v` typu `T` nyní se skládá z spuštění `new Box<T>(v)`výrazu a vrácení výsledné instance jako hodnoty typu `object`.</span><span class="sxs-lookup"><span data-stu-id="df7d3-364">Boxing of a value `v` of type `T` now consists of executing the expression `new Box<T>(v)`, and returning the resulting instance as a value of type `object`.</span></span> <span data-ttu-id="df7d3-365">Příkazy tedy</span><span class="sxs-lookup"><span data-stu-id="df7d3-365">Thus, the statements</span></span>
```csharp
int i = 123;
object box = i;
```
<span data-ttu-id="df7d3-366">koncepčně souhlasí</span><span class="sxs-lookup"><span data-stu-id="df7d3-366">conceptually correspond to</span></span>
```csharp
int i = 123;
object box = new Box<int>(i);
```

<span data-ttu-id="df7d3-367">Třída zabalení, jako je výše uvedená `Box<T>`, ve skutečnosti neexistuje a dynamický typ zabalené hodnoty není ve skutečnosti typu třídy.</span><span class="sxs-lookup"><span data-stu-id="df7d3-367">A boxing class like `Box<T>` above doesn't actually exist and the dynamic type of a boxed value isn't actually a class type.</span></span> <span data-ttu-id="df7d3-368">Místo toho je zabalená hodnota typu `T`a `T`dynamického typu a při kontrole dynamického typu pomocí operátoru `is` lze jednoduše odkazovat typ `T`.</span><span class="sxs-lookup"><span data-stu-id="df7d3-368">Instead, a boxed value of type `T` has the dynamic type `T`, and a dynamic type check using the `is` operator can simply reference type `T`.</span></span> <span data-ttu-id="df7d3-369">Například</span><span class="sxs-lookup"><span data-stu-id="df7d3-369">For example,</span></span>
```csharp
int i = 123;
object box = i;
if (box is int) {
    Console.Write("Box contains an int");
}
```
<span data-ttu-id="df7d3-370">provede výstup řetězce "`Box contains an int`" v konzole nástroje.</span><span class="sxs-lookup"><span data-stu-id="df7d3-370">will output the string "`Box contains an int`" on the console.</span></span>

<span data-ttu-id="df7d3-371">Převod zabalení předpokládá, že je kopie hodnoty zabalená.</span><span class="sxs-lookup"><span data-stu-id="df7d3-371">A boxing conversion implies making a copy of the value being boxed.</span></span> <span data-ttu-id="df7d3-372">To se liší od konverze *reference_type* na typ `object`, ve kterém hodnota nadále odkazuje na stejnou instanci a jednoduše je považována za méně odvozený typ `object`.</span><span class="sxs-lookup"><span data-stu-id="df7d3-372">This is different from a conversion of a *reference_type* to type `object`, in which the value continues to reference the same instance and simply is regarded as the less derived type `object`.</span></span> <span data-ttu-id="df7d3-373">Například s ohledem na deklaraci</span><span class="sxs-lookup"><span data-stu-id="df7d3-373">For example, given the declaration</span></span>
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
<span data-ttu-id="df7d3-374">následující příkazy</span><span class="sxs-lookup"><span data-stu-id="df7d3-374">the following statements</span></span>
```csharp
Point p = new Point(10, 10);
object box = p;
p.x = 20;
Console.Write(((Point)box).x);
```
<span data-ttu-id="df7d3-375">vypíše hodnotu 10 v konzole, protože operace implicitního zabalení, ke které dojde v přiřazení `p` k `box` způsobí, že se zkopíruje hodnota `p`.</span><span class="sxs-lookup"><span data-stu-id="df7d3-375">will output the value 10 on the console because the implicit boxing operation that occurs in the assignment of `p` to `box` causes the value of `p` to be copied.</span></span> <span data-ttu-id="df7d3-376">Bylo `Point` deklarované `class` namísto toho, hodnota 20 by byla výstup, protože `p` a `box` by odkazovaly na stejnou instanci.</span><span class="sxs-lookup"><span data-stu-id="df7d3-376">Had `Point` been declared a `class` instead, the value 20 would be output because `p` and `box` would reference the same instance.</span></span>

### <a name="unboxing-conversions"></a><span data-ttu-id="df7d3-377">Převody rozbalení</span><span class="sxs-lookup"><span data-stu-id="df7d3-377">Unboxing conversions</span></span>

<span data-ttu-id="df7d3-378">Převod rozbalení umožňuje *reference_type* explicitně převést na *value_type*.</span><span class="sxs-lookup"><span data-stu-id="df7d3-378">An unboxing conversion permits a *reference_type* to be explicitly converted to a *value_type*.</span></span> <span data-ttu-id="df7d3-379">Existují následující převody rozbalení:</span><span class="sxs-lookup"><span data-stu-id="df7d3-379">The following unboxing conversions exist:</span></span>

*  <span data-ttu-id="df7d3-380">Z typu `object` na libovolný *value_type*.</span><span class="sxs-lookup"><span data-stu-id="df7d3-380">From the type `object` to any *value_type*.</span></span>
*  <span data-ttu-id="df7d3-381">Z typu `System.ValueType` na libovolný *value_type*.</span><span class="sxs-lookup"><span data-stu-id="df7d3-381">From the type `System.ValueType` to any *value_type*.</span></span>
*  <span data-ttu-id="df7d3-382">Z libovolného *INTERFACE_TYPE* na libovolný *non_nullable_value_type* , který implementuje *INTERFACE_TYPE*.</span><span class="sxs-lookup"><span data-stu-id="df7d3-382">From any *interface_type* to any *non_nullable_value_type* that implements the *interface_type*.</span></span>
*  <span data-ttu-id="df7d3-383">Z libovolného *INTERFACE_TYPE* na všechny *nullable_type* , jejichž základní typ implementuje *INTERFACE_TYPE*.</span><span class="sxs-lookup"><span data-stu-id="df7d3-383">From any *interface_type* to any *nullable_type* whose underlying type implements the *interface_type*.</span></span>
*  <span data-ttu-id="df7d3-384">Z typu `System.Enum` na libovolný *enum_type*.</span><span class="sxs-lookup"><span data-stu-id="df7d3-384">From the type `System.Enum` to any *enum_type*.</span></span>
*  <span data-ttu-id="df7d3-385">Z typu `System.Enum` do libovolného *nullable_type* s podkladovým *enum_type*.</span><span class="sxs-lookup"><span data-stu-id="df7d3-385">From the type `System.Enum` to any *nullable_type* with an underlying *enum_type*.</span></span>
*  <span data-ttu-id="df7d3-386">Všimněte si, že explicitní převod na parametr typu bude proveden jako převod rozbalení, pokud v době běhu skončí převod z typu odkazu na typ hodnoty ([explicitní dynamické převody](conversions.md#explicit-dynamic-conversions)).</span><span class="sxs-lookup"><span data-stu-id="df7d3-386">Note that an explicit conversion to a type parameter will be executed as an unboxing conversion if at run-time it ends up converting from a reference type to a value type ([Explicit dynamic conversions](conversions.md#explicit-dynamic-conversions)).</span></span>

<span data-ttu-id="df7d3-387">Rozbalení operace *non_nullable_value_type* se skládá z první kontroly, zda je instance objektu zabalená hodnota daného *non_nullable_value_type*a pak se hodnota zkopíruje mimo instanci.</span><span class="sxs-lookup"><span data-stu-id="df7d3-387">An unboxing operation to a *non_nullable_value_type* consists of first checking that the object instance is a boxed value of the given *non_nullable_value_type*, and then copying the value out of the instance.</span></span>

<span data-ttu-id="df7d3-388">Rozbalení na *nullable_type* vytvoří hodnotu null *nullable_type* , pokud je zdrojový operand `null`, nebo zabalený výsledek rozbalení instance objektu do základního typu *nullable_type* v opačném případě.</span><span class="sxs-lookup"><span data-stu-id="df7d3-388">Unboxing to a *nullable_type* produces the null value of the *nullable_type* if the source operand is `null`, or the wrapped result of unboxing the object instance to the underlying type of the *nullable_type* otherwise.</span></span>

<span data-ttu-id="df7d3-389">Odkazy na imaginární třídu zabalení popsanou v předchozí části, převod rozbalení objektu `box` na *value_type* `T` se skládá z spuštění `((Box<T>)box).value`výrazu.</span><span class="sxs-lookup"><span data-stu-id="df7d3-389">Referring to the imaginary boxing class described in the previous section, an unboxing conversion of an object `box` to a *value_type* `T` consists of executing the expression `((Box<T>)box).value`.</span></span> <span data-ttu-id="df7d3-390">Příkazy tedy</span><span class="sxs-lookup"><span data-stu-id="df7d3-390">Thus, the statements</span></span>
```csharp
object box = 123;
int i = (int)box;
```
<span data-ttu-id="df7d3-391">koncepčně souhlasí</span><span class="sxs-lookup"><span data-stu-id="df7d3-391">conceptually correspond to</span></span>
```csharp
object box = new Box<int>(123);
int i = ((Box<int>)box).value;
```

<span data-ttu-id="df7d3-392">V případě převodu rozbalení na daný *non_nullable_value_type* v době běhu musí být hodnota zdrojového operandu odkazem na zabalenou hodnotu tohoto *non_nullable_value_type*.</span><span class="sxs-lookup"><span data-stu-id="df7d3-392">For an unboxing conversion to a given *non_nullable_value_type* to succeed at run-time, the value of the source operand must be a reference to a boxed value of that *non_nullable_value_type*.</span></span> <span data-ttu-id="df7d3-393">Pokud je zdrojový operand `null`, je vyvolána `System.NullReferenceException`.</span><span class="sxs-lookup"><span data-stu-id="df7d3-393">If the source operand is `null`, a `System.NullReferenceException` is thrown.</span></span> <span data-ttu-id="df7d3-394">Pokud je zdrojový operand odkazem na nekompatibilní objekt, je vyvolána `System.InvalidCastException`.</span><span class="sxs-lookup"><span data-stu-id="df7d3-394">If the source operand is a reference to an incompatible object, a `System.InvalidCastException` is thrown.</span></span>

<span data-ttu-id="df7d3-395">V případě převodu rozbalení na daný *nullable_type* v době běhu musí být hodnota zdrojového operandu buď `null`, nebo odkaz na zabalenou hodnotu podkladového *non_nullable_value_typeu* *nullable_type*.</span><span class="sxs-lookup"><span data-stu-id="df7d3-395">For an unboxing conversion to a given *nullable_type* to succeed at run-time, the value of the source operand must be either `null` or a reference to a boxed value of the underlying *non_nullable_value_type* of the *nullable_type*.</span></span> <span data-ttu-id="df7d3-396">Pokud je zdrojový operand odkazem na nekompatibilní objekt, je vyvolána `System.InvalidCastException`.</span><span class="sxs-lookup"><span data-stu-id="df7d3-396">If the source operand is a reference to an incompatible object, a `System.InvalidCastException` is thrown.</span></span>

## <a name="constructed-types"></a><span data-ttu-id="df7d3-397">Vytvořené typy</span><span class="sxs-lookup"><span data-stu-id="df7d3-397">Constructed types</span></span>

<span data-ttu-id="df7d3-398">Deklarace obecného typu sám o sobě označuje ***nevázaný obecný typ*** , který se používá jako "podrobný plán" pro vytvoření mnoha různých typů, a to tak, že použijete ***argumenty typu***.</span><span class="sxs-lookup"><span data-stu-id="df7d3-398">A generic type declaration, by itself, denotes an ***unbound generic type*** that is used as a "blueprint" to form many different types, by way of applying ***type arguments***.</span></span> <span data-ttu-id="df7d3-399">Argumenty typu jsou zapsány v lomených závorkách (`<` a `>`) hned za názvem obecného typu.</span><span class="sxs-lookup"><span data-stu-id="df7d3-399">The type arguments are written within angle brackets (`<` and `>`) immediately following the name of the generic type.</span></span> <span data-ttu-id="df7d3-400">Typ, který obsahuje alespoň jeden argument typu, se nazývá ***konstruovaný typ***.</span><span class="sxs-lookup"><span data-stu-id="df7d3-400">A type that includes at least one type argument is called a ***constructed type***.</span></span> <span data-ttu-id="df7d3-401">Konstruovaný typ lze použít na většině míst v jazyce, ve kterém se může zobrazit název typu.</span><span class="sxs-lookup"><span data-stu-id="df7d3-401">A constructed type can be used in most places in the language in which a type name can appear.</span></span> <span data-ttu-id="df7d3-402">Nevázaný obecný typ se dá použít jenom v rámci *typeof_expression* ([operátor typeof](expressions.md#the-typeof-operator)).</span><span class="sxs-lookup"><span data-stu-id="df7d3-402">An unbound generic type can only be used within a *typeof_expression* ([The typeof operator](expressions.md#the-typeof-operator)).</span></span>

<span data-ttu-id="df7d3-403">Vytvořené typy lze také použít ve výrazech jako jednoduché názvy ([jednoduché názvy](expressions.md#simple-names)) nebo při přístupu ke členu ([přístupu ke členu](expressions.md#member-access)).</span><span class="sxs-lookup"><span data-stu-id="df7d3-403">Constructed types can also be used in expressions as simple names ([Simple names](expressions.md#simple-names)) or when accessing a member ([Member access](expressions.md#member-access)).</span></span>

<span data-ttu-id="df7d3-404">Při vyhodnocování *namespace_or_type_name* se považují pouze obecné typy se správným počtem parametrů typu.</span><span class="sxs-lookup"><span data-stu-id="df7d3-404">When a *namespace_or_type_name* is evaluated, only generic types with the correct number of type parameters are considered.</span></span> <span data-ttu-id="df7d3-405">Proto je možné použít stejný identifikátor k identifikaci různých typů, pokud typy mají různé počty parametrů typu.</span><span class="sxs-lookup"><span data-stu-id="df7d3-405">Thus, it is possible to use the same identifier to identify different types, as long as the types have different numbers of type parameters.</span></span> <span data-ttu-id="df7d3-406">To je užitečné při kombinování obecných a neobecných tříd ve stejném programu:</span><span class="sxs-lookup"><span data-stu-id="df7d3-406">This is useful when mixing generic and non-generic classes in the same program:</span></span>

```csharp
namespace Widgets
{
    class Queue {...}
    class Queue<TElement> {...}
}

namespace MyApplication
{
    using Widgets;

    class X
    {
        Queue q1;            // Non-generic Widgets.Queue
        Queue<int> q2;       // Generic Widgets.Queue
    }
}
```

<span data-ttu-id="df7d3-407">*TYPE_NAME* může identifikovat konstruovaný typ, i když neurčuje parametry typu přímo.</span><span class="sxs-lookup"><span data-stu-id="df7d3-407">A *type_name* might identify a constructed type even though it doesn't specify type parameters directly.</span></span> <span data-ttu-id="df7d3-408">K tomu může dojít, když je typ vnořený v deklaraci obecné třídy a typ instance obsahující deklarace se implicitně používá pro vyhledávání názvů ([vnořené typy v obecných třídách](classes.md#nested-types-in-generic-classes)):</span><span class="sxs-lookup"><span data-stu-id="df7d3-408">This can occur where a type is nested within a generic class declaration, and the instance type of the containing declaration is implicitly used for name lookup ([Nested types in generic classes](classes.md#nested-types-in-generic-classes)):</span></span>

```csharp
class Outer<T>
{
    public class Inner {...}

    public Inner i;                // Type of i is Outer<T>.Inner
}
```

<span data-ttu-id="df7d3-409">V nebezpečném kódu nelze použít konstruovaný typ jako *unmanaged_type* ([typy ukazatelů](unsafe-code.md#pointer-types)).</span><span class="sxs-lookup"><span data-stu-id="df7d3-409">In unsafe code, a constructed type cannot be used as an *unmanaged_type* ([Pointer types](unsafe-code.md#pointer-types)).</span></span>

### <a name="type-arguments"></a><span data-ttu-id="df7d3-410">Argumenty typu</span><span class="sxs-lookup"><span data-stu-id="df7d3-410">Type arguments</span></span>

<span data-ttu-id="df7d3-411">Každý argument v seznamu argumentů typu je jednoduše *typ*.</span><span class="sxs-lookup"><span data-stu-id="df7d3-411">Each argument in a type argument list is simply a *type*.</span></span>

```antlr
type_argument_list
    : '<' type_arguments '>'
    ;

type_arguments
    : type_argument (',' type_argument)*
    ;

type_argument
    : type
    ;
```

<span data-ttu-id="df7d3-412">V nebezpečném kódu ([nezabezpečený kód](unsafe-code.md)) nesmí být *type_argument* typem ukazatele.</span><span class="sxs-lookup"><span data-stu-id="df7d3-412">In unsafe code ([Unsafe code](unsafe-code.md)), a *type_argument* may not be a pointer type.</span></span> <span data-ttu-id="df7d3-413">Každý argument typu musí splňovat jakákoli omezení na odpovídajícím parametru typu ([omezení parametrů typu](classes.md#type-parameter-constraints)).</span><span class="sxs-lookup"><span data-stu-id="df7d3-413">Each type argument must satisfy any constraints on the corresponding type parameter ([Type parameter constraints](classes.md#type-parameter-constraints)).</span></span>

### <a name="open-and-closed-types"></a><span data-ttu-id="df7d3-414">Otevřené a uzavřené typy</span><span class="sxs-lookup"><span data-stu-id="df7d3-414">Open and closed types</span></span>

<span data-ttu-id="df7d3-415">Všechny typy mohou být klasifikovány buď jako ***otevřené typy*** nebo ***uzavřené typy***.</span><span class="sxs-lookup"><span data-stu-id="df7d3-415">All types can be classified as either ***open types*** or ***closed types***.</span></span> <span data-ttu-id="df7d3-416">Otevřený typ je typ, který zahrnuje parametry typu.</span><span class="sxs-lookup"><span data-stu-id="df7d3-416">An open type is a type that involves type parameters.</span></span> <span data-ttu-id="df7d3-417">Konkrétně:</span><span class="sxs-lookup"><span data-stu-id="df7d3-417">More specifically:</span></span>

*  <span data-ttu-id="df7d3-418">Parametr typu definuje otevřený typ.</span><span class="sxs-lookup"><span data-stu-id="df7d3-418">A type parameter defines an open type.</span></span>
*  <span data-ttu-id="df7d3-419">Typ pole je otevřený typ pouze v případě, že je jeho typ prvku otevřený typ.</span><span class="sxs-lookup"><span data-stu-id="df7d3-419">An array type is an open type if and only if its element type is an open type.</span></span>
*  <span data-ttu-id="df7d3-420">Konstruovaný typ je otevřený typ pouze v případě, že jeden nebo více jeho argumentů typu je otevřený typ.</span><span class="sxs-lookup"><span data-stu-id="df7d3-420">A constructed type is an open type if and only if one or more of its type arguments is an open type.</span></span> <span data-ttu-id="df7d3-421">Konstruovaný vnořený typ je otevřený typ, pokud a pouze v případě, že jeden nebo více jeho argumentů typu nebo argumenty typu jeho obsahujícího typu je otevřený typ.</span><span class="sxs-lookup"><span data-stu-id="df7d3-421">A constructed nested type is an open type if and only if one or more of its type arguments or the type arguments of its containing type(s) is an open type.</span></span>

<span data-ttu-id="df7d3-422">Uzavřený typ je typ, který není otevřený typ.</span><span class="sxs-lookup"><span data-stu-id="df7d3-422">A closed type is a type that is not an open type.</span></span>

<span data-ttu-id="df7d3-423">V době běhu je veškerý kód v rámci deklarace obecného typu proveden v kontextu uzavřeného typu, který byl vytvořen pomocí argumentů typu pro obecnou deklaraci.</span><span class="sxs-lookup"><span data-stu-id="df7d3-423">At run-time, all of the code within a generic type declaration is executed in the context of a closed constructed type that was created by applying type arguments to the generic declaration.</span></span> <span data-ttu-id="df7d3-424">Každý parametr typu v rámci obecného typu je vázán na určitý typ běhu.</span><span class="sxs-lookup"><span data-stu-id="df7d3-424">Each type parameter within the generic type is bound to a particular run-time type.</span></span> <span data-ttu-id="df7d3-425">Běhové zpracování všech příkazů a výrazů vždy probíhá s uzavřenými typy a otevřené typy nastávají pouze během zpracování při kompilaci.</span><span class="sxs-lookup"><span data-stu-id="df7d3-425">The run-time processing of all statements and expressions always occurs with closed types, and open types occur only during compile-time processing.</span></span>

<span data-ttu-id="df7d3-426">Každý uzavřený konstruovaný typ má svou vlastní sadu statických proměnných, které nejsou sdíleny s žádnými jinými uzavřenými konstruovanými typy.</span><span class="sxs-lookup"><span data-stu-id="df7d3-426">Each closed constructed type has its own set of static variables, which are not shared with any other closed constructed types.</span></span> <span data-ttu-id="df7d3-427">Vzhledem k tomu, že otevřený typ neexistuje v době běhu, nejsou k otevřenému typu přidruženy žádné statické proměnné.</span><span class="sxs-lookup"><span data-stu-id="df7d3-427">Since an open type does not exist at run-time, there are no static variables associated with an open type.</span></span> <span data-ttu-id="df7d3-428">Dva uzavřené konstruované typy jsou stejného typu, pokud jsou vyrobeny ze stejného obecného typu bez vazby a jejich odpovídající argumenty typu jsou stejného typu.</span><span class="sxs-lookup"><span data-stu-id="df7d3-428">Two closed constructed types are the same type if they are constructed from the same unbound generic type, and their corresponding type arguments are the same type.</span></span>

### <a name="bound-and-unbound-types"></a><span data-ttu-id="df7d3-429">Vázané a nevázané typy</span><span class="sxs-lookup"><span data-stu-id="df7d3-429">Bound and unbound types</span></span>

<span data-ttu-id="df7d3-430">Termín ***nevázaný typ*** odkazuje na neobecný typ nebo na nevázaný obecný typ.</span><span class="sxs-lookup"><span data-stu-id="df7d3-430">The term ***unbound type*** refers to a non-generic type or an unbound generic type.</span></span> <span data-ttu-id="df7d3-431">Pojem ***vázaný typ*** odkazuje na neobecný typ nebo konstruovaný typ.</span><span class="sxs-lookup"><span data-stu-id="df7d3-431">The term ***bound type*** refers to a non-generic type or a constructed type.</span></span>

<span data-ttu-id="df7d3-432">Nevázaný typ odkazuje na entitu deklarovanou deklarací typu.</span><span class="sxs-lookup"><span data-stu-id="df7d3-432">An unbound type refers to the entity declared by a type declaration.</span></span> <span data-ttu-id="df7d3-433">Nevázaný obecný typ není sám o sobě typu a nedá se použít jako typ proměnné, argumentu nebo návratové hodnoty nebo jako základní typ.</span><span class="sxs-lookup"><span data-stu-id="df7d3-433">An unbound generic type is not itself a type, and cannot be used as the type of a variable, argument or return value, or as a base type.</span></span> <span data-ttu-id="df7d3-434">Jedinou konstrukcí, ve které je možné odkazovat na nevázaný obecný typ, je výraz `typeof` ([operátor typeof](expressions.md#the-typeof-operator)).</span><span class="sxs-lookup"><span data-stu-id="df7d3-434">The only construct in which an unbound generic type can be referenced is the `typeof` expression ([The typeof operator](expressions.md#the-typeof-operator)).</span></span>

### <a name="satisfying-constraints"></a><span data-ttu-id="df7d3-435">Splnění omezení</span><span class="sxs-lookup"><span data-stu-id="df7d3-435">Satisfying constraints</span></span>

<span data-ttu-id="df7d3-436">Pokaždé, když je odkazováno na konstruovaný typ nebo obecnou metodu, jsou zadávané argumenty typu zkontrolovány proti omezení parametru typu deklarovaném na obecném typu nebo metodě ([omezení parametrů typu](classes.md#type-parameter-constraints)).</span><span class="sxs-lookup"><span data-stu-id="df7d3-436">Whenever a constructed type or generic method is referenced, the supplied type arguments are checked against the type parameter constraints declared on the generic type or method ([Type parameter constraints](classes.md#type-parameter-constraints)).</span></span> <span data-ttu-id="df7d3-437">Pro každou klauzuli `where` se pro každé omezení kontroluje argument typu `A`, který odpovídá parametru pojmenovaného typu, a to následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="df7d3-437">For each `where` clause, the type argument `A` that corresponds to the named type parameter is checked against each constraint as follows:</span></span>

*  <span data-ttu-id="df7d3-438">Pokud je omezení typem třídy, typem rozhraní nebo parametrem typu, nechejte `C` představovat toto omezení s poskytnutými argumenty typu nahrazené pro všechny parametry typu, které se zobrazí v omezení.</span><span class="sxs-lookup"><span data-stu-id="df7d3-438">If the constraint is a class type, an interface type, or a type parameter, let `C` represent that constraint with the supplied type arguments substituted for any type parameters that appear in the constraint.</span></span> <span data-ttu-id="df7d3-439">Aby bylo omezení splněno, musí se jednat o případ, že typ `A` je převoditelné na typ `C` jedním z následujících způsobů:</span><span class="sxs-lookup"><span data-stu-id="df7d3-439">To satisfy the constraint, it must be the case that type `A` is convertible to type `C` by one of the following:</span></span>
    * <span data-ttu-id="df7d3-440">Převod identity ([převod identity](conversions.md#identity-conversion))</span><span class="sxs-lookup"><span data-stu-id="df7d3-440">An identity conversion ([Identity conversion](conversions.md#identity-conversion))</span></span>
    * <span data-ttu-id="df7d3-441">Implicitní převod odkazu ([implicitní převody odkazů](conversions.md#implicit-reference-conversions))</span><span class="sxs-lookup"><span data-stu-id="df7d3-441">An implicit reference conversion ([Implicit reference conversions](conversions.md#implicit-reference-conversions))</span></span>
    * <span data-ttu-id="df7d3-442">Převod zabalení ([převody zabalení](conversions.md#boxing-conversions)) za předpokladu, že typ A je typ hodnoty, která není null.</span><span class="sxs-lookup"><span data-stu-id="df7d3-442">A boxing conversion ([Boxing conversions](conversions.md#boxing-conversions)), provided that type A is a non-nullable value type.</span></span>
    * <span data-ttu-id="df7d3-443">Implicitní odkaz, zabalení nebo převod parametru typu z parametru typu `A` do `C`.</span><span class="sxs-lookup"><span data-stu-id="df7d3-443">An implicit reference, boxing or type parameter conversion from a type parameter `A` to `C`.</span></span>
*  <span data-ttu-id="df7d3-444">Pokud je omezení omezení typu odkazu (`class`), typ `A` musí splňovat jednu z následujících možností:</span><span class="sxs-lookup"><span data-stu-id="df7d3-444">If the constraint is the reference type constraint (`class`), the type `A` must satisfy one of the following:</span></span>
    * <span data-ttu-id="df7d3-445">`A` je typ rozhraní, typ třídy, typ delegáta nebo typ pole.</span><span class="sxs-lookup"><span data-stu-id="df7d3-445">`A` is an interface type, class type, delegate type or array type.</span></span> <span data-ttu-id="df7d3-446">Všimněte si, že `System.ValueType` a `System.Enum` jsou odkazové typy, které toto omezení vyhoví.</span><span class="sxs-lookup"><span data-stu-id="df7d3-446">Note that `System.ValueType` and `System.Enum` are reference types that satisfy this constraint.</span></span>
    * <span data-ttu-id="df7d3-447">`A` je parametr typu, který je známý jako typ odkazu ([omezení parametrů typu](classes.md#type-parameter-constraints)).</span><span class="sxs-lookup"><span data-stu-id="df7d3-447">`A` is a type parameter that is known to be a reference type ([Type parameter constraints](classes.md#type-parameter-constraints)).</span></span>
*  <span data-ttu-id="df7d3-448">Pokud je omezení typu hodnoty (`struct`), typ `A` musí splňovat jednu z následujících hodnot:</span><span class="sxs-lookup"><span data-stu-id="df7d3-448">If the constraint is the value type constraint (`struct`), the type `A` must satisfy one of the following:</span></span>
    * <span data-ttu-id="df7d3-449">`A` je typ struktury nebo typ výčtu, ale ne typ s možnou hodnotou null.</span><span class="sxs-lookup"><span data-stu-id="df7d3-449">`A` is a struct type or enum type, but not a nullable type.</span></span> <span data-ttu-id="df7d3-450">Všimněte si, že `System.ValueType` a `System.Enum` jsou odkazové typy, které nesplňují toto omezení.</span><span class="sxs-lookup"><span data-stu-id="df7d3-450">Note that `System.ValueType` and `System.Enum` are reference types that do not satisfy this constraint.</span></span>
    * <span data-ttu-id="df7d3-451">`A` je parametr typu s omezením typu hodnoty ([omezení parametrů typu](classes.md#type-parameter-constraints)).</span><span class="sxs-lookup"><span data-stu-id="df7d3-451">`A` is a type parameter having the value type constraint ([Type parameter constraints](classes.md#type-parameter-constraints)).</span></span>
*  <span data-ttu-id="df7d3-452">Pokud je omezením `new()`omezení konstruktoru, typ `A` nesmí být `abstract` a musí mít veřejný konstruktor bez parametrů.</span><span class="sxs-lookup"><span data-stu-id="df7d3-452">If the constraint is the constructor constraint `new()`, the type `A` must not be `abstract` and must have a public parameterless constructor.</span></span> <span data-ttu-id="df7d3-453">To je splněno, pokud je splněna jedna z následujících podmínek:</span><span class="sxs-lookup"><span data-stu-id="df7d3-453">This is satisfied if one of the following is true:</span></span>
    * <span data-ttu-id="df7d3-454">`A` je hodnotový typ, protože všechny typy hodnot mají veřejný výchozí konstruktor ([výchozí konstruktory](types.md#default-constructors)).</span><span class="sxs-lookup"><span data-stu-id="df7d3-454">`A` is a value type, since all value types have a public default constructor ([Default constructors](types.md#default-constructors)).</span></span>
    * <span data-ttu-id="df7d3-455">`A` je parametr typu s omezením konstruktoru ([omezení parametrů typu](classes.md#type-parameter-constraints)).</span><span class="sxs-lookup"><span data-stu-id="df7d3-455">`A` is a type parameter having the constructor constraint ([Type parameter constraints](classes.md#type-parameter-constraints)).</span></span>
    * <span data-ttu-id="df7d3-456">`A` je parametr typu s omezením typu hodnoty ([omezení parametrů typu](classes.md#type-parameter-constraints)).</span><span class="sxs-lookup"><span data-stu-id="df7d3-456">`A` is a type parameter having the value type constraint ([Type parameter constraints](classes.md#type-parameter-constraints)).</span></span>
    * <span data-ttu-id="df7d3-457">`A` je třída, která není `abstract` a obsahuje explicitně deklarovaný `public` konstruktor bez parametrů.</span><span class="sxs-lookup"><span data-stu-id="df7d3-457">`A` is a class that is not `abstract` and contains an explicitly declared `public` constructor with no parameters.</span></span>
    * <span data-ttu-id="df7d3-458">`A` není `abstract` a má výchozí konstruktor ([výchozí konstruktory](classes.md#default-constructors)).</span><span class="sxs-lookup"><span data-stu-id="df7d3-458">`A` is not `abstract` and has a default constructor ([Default constructors](classes.md#default-constructors)).</span></span>

<span data-ttu-id="df7d3-459">V době kompilace dojde k chybě, pokud některé z omezení parametru typu nejsou splněny danými argumenty typu.</span><span class="sxs-lookup"><span data-stu-id="df7d3-459">A compile-time error occurs if one or more of a type parameter's constraints are not satisfied by the given type arguments.</span></span>

<span data-ttu-id="df7d3-460">Vzhledem k tomu, že parametry typu nejsou děděny, nejsou omezení nikdy děděna.</span><span class="sxs-lookup"><span data-stu-id="df7d3-460">Since type parameters are not inherited, constraints are never inherited either.</span></span> <span data-ttu-id="df7d3-461">V následujícím příkladu `D` nutné zadat omezení pro svůj parametr typu `T` tak, aby `T` splňovala omezení, které je stanoveno `B<T>`ou základní třídy.</span><span class="sxs-lookup"><span data-stu-id="df7d3-461">In the example below, `D` needs to specify the constraint on its type parameter `T` so that `T` satisfies the constraint imposed by the base class `B<T>`.</span></span> <span data-ttu-id="df7d3-462">Naproti tomu třída `E` nemusí určovat omezení, protože `List<T>` implementuje `IEnumerable` pro všechny `T`.</span><span class="sxs-lookup"><span data-stu-id="df7d3-462">In contrast, class `E` need not specify a constraint, because `List<T>` implements `IEnumerable` for any `T`.</span></span>

```csharp
class B<T> where T: IEnumerable {...}

class D<T>: B<T> where T: IEnumerable {...}

class E<T>: B<List<T>> {...}
```

## <a name="type-parameters"></a><span data-ttu-id="df7d3-463">Parametry typu</span><span class="sxs-lookup"><span data-stu-id="df7d3-463">Type parameters</span></span>

<span data-ttu-id="df7d3-464">Parametr typu je identifikátor označující typ hodnoty nebo typ odkazu, na který je parametr vázán za běhu.</span><span class="sxs-lookup"><span data-stu-id="df7d3-464">A type parameter is an identifier designating a value type or reference type that the parameter is bound to at run-time.</span></span>

```antlr
type_parameter
    : identifier
    ;
```

<span data-ttu-id="df7d3-465">Vzhledem k tomu, že je možné vytvořit instanci parametru typu s mnoha různými skutečnými argumenty typu, mají parametry typu mírně různé operace a omezení než jiné typy.</span><span class="sxs-lookup"><span data-stu-id="df7d3-465">Since a type parameter can be instantiated with many different actual type arguments, type parameters have slightly different operations and restrictions than other types.</span></span> <span data-ttu-id="df7d3-466">Zde jsou některé z nich:</span><span class="sxs-lookup"><span data-stu-id="df7d3-466">These include:</span></span>

*  <span data-ttu-id="df7d3-467">Parametr typu nelze použít přímo k deklaraci základní třídy ([základní třídy](classes.md#base-class)) nebo rozhraní ([seznamů parametrů variant typu](interfaces.md#variant-type-parameter-lists)).</span><span class="sxs-lookup"><span data-stu-id="df7d3-467">A type parameter cannot be used directly to declare a base class ([Base class](classes.md#base-class)) or interface ([Variant type parameter lists](interfaces.md#variant-type-parameter-lists)).</span></span>
*  <span data-ttu-id="df7d3-468">Pravidla pro vyhledávání členů u parametrů typu závisí na omezeních, pokud jsou použita pro parametr typu.</span><span class="sxs-lookup"><span data-stu-id="df7d3-468">The rules for member lookup on type parameters depend on the constraints, if any, applied to the type parameter.</span></span> <span data-ttu-id="df7d3-469">Jsou podrobně popsány v [hledání členů](expressions.md#member-lookup).</span><span class="sxs-lookup"><span data-stu-id="df7d3-469">They are detailed in [Member lookup](expressions.md#member-lookup).</span></span>
*  <span data-ttu-id="df7d3-470">Dostupné převody pro parametr typu závisí na omezeních, pokud jsou použita pro parametr typu.</span><span class="sxs-lookup"><span data-stu-id="df7d3-470">The available conversions for a type parameter depend on the constraints, if any, applied to the type parameter.</span></span> <span data-ttu-id="df7d3-471">Jsou podrobně popsány v [implicitních převodech týkajících se parametrů typu](conversions.md#implicit-conversions-involving-type-parameters) a [explicitních dynamických převodů](conversions.md#explicit-dynamic-conversions).</span><span class="sxs-lookup"><span data-stu-id="df7d3-471">They are detailed in [Implicit conversions involving type parameters](conversions.md#implicit-conversions-involving-type-parameters) and [Explicit dynamic conversions](conversions.md#explicit-dynamic-conversions).</span></span>
*  <span data-ttu-id="df7d3-472">Literál `null` nelze převést na typ předaný parametrem typu, s výjimkou toho, pokud je parametr typu znám jako typ odkazu ([implicitní převody zahrnující parametry typu](conversions.md#implicit-conversions-involving-type-parameters)).</span><span class="sxs-lookup"><span data-stu-id="df7d3-472">The literal `null` cannot be converted to a type given by a type parameter, except if the type parameter is known to be a reference type ([Implicit conversions involving type parameters](conversions.md#implicit-conversions-involving-type-parameters)).</span></span> <span data-ttu-id="df7d3-473">Místo toho se ale dá použít výraz `default` ([výrazy s výchozí hodnotou](expressions.md#default-value-expressions)).</span><span class="sxs-lookup"><span data-stu-id="df7d3-473">However, a `default` expression ([Default value expressions](expressions.md#default-value-expressions)) can be used instead.</span></span> <span data-ttu-id="df7d3-474">Kromě toho může být hodnota typu zadaná parametrem typu porovnána s `null` pomocí `==` a `!=` ([operátory rovnosti typu odkazu](expressions.md#reference-type-equality-operators)), pokud parametr typu nemá omezení typu hodnoty.</span><span class="sxs-lookup"><span data-stu-id="df7d3-474">In addition, a value with a type given by a type parameter can be compared with `null` using `==` and `!=` ([Reference type equality operators](expressions.md#reference-type-equality-operators)) unless the type parameter has the value type constraint.</span></span>
*  <span data-ttu-id="df7d3-475">Výraz `new` ([výrazy vytvoření objektu](expressions.md#object-creation-expressions)) lze použít pouze s parametrem typu, pokud je parametr typu omezen hodnotou *constructor_constraint* nebo omezením typu hodnoty ([omezení parametrů typu](classes.md#type-parameter-constraints)).</span><span class="sxs-lookup"><span data-stu-id="df7d3-475">A `new` expression ([Object creation expressions](expressions.md#object-creation-expressions)) can only be used with a type parameter if the type parameter is constrained by a *constructor_constraint* or the value type constraint ([Type parameter constraints](classes.md#type-parameter-constraints)).</span></span>
*  <span data-ttu-id="df7d3-476">Parametr typu se nedá použít kdekoli v rámci atributu.</span><span class="sxs-lookup"><span data-stu-id="df7d3-476">A type parameter cannot be used anywhere within an attribute.</span></span>
*  <span data-ttu-id="df7d3-477">Parametr typu se nedá použít v přístupu ke členu ([přístup k přístupu](expressions.md#member-access)ke členu) nebo názvu typu ([obor názvů a názvy typů](basic-concepts.md#namespace-and-type-names)) k identifikaci statického člena nebo vnořeného typu.</span><span class="sxs-lookup"><span data-stu-id="df7d3-477">A type parameter cannot be used in a member access ([Member access](expressions.md#member-access)) or type name ([Namespace and type names](basic-concepts.md#namespace-and-type-names)) to identify a static member or a nested type.</span></span>
*  <span data-ttu-id="df7d3-478">V nebezpečném kódu parametr typu nelze použít jako *unmanaged_type* ([typy ukazatelů](unsafe-code.md#pointer-types)).</span><span class="sxs-lookup"><span data-stu-id="df7d3-478">In unsafe code, a type parameter cannot be used as an *unmanaged_type* ([Pointer types](unsafe-code.md#pointer-types)).</span></span>

<span data-ttu-id="df7d3-479">Jako typ jsou parametry typu čistě konstrukce v čase kompilace.</span><span class="sxs-lookup"><span data-stu-id="df7d3-479">As a type, type parameters are purely a compile-time construct.</span></span> <span data-ttu-id="df7d3-480">V době běhu jsou jednotlivé parametry typu vázány na typ modulu runtime, který byl zadán zadáním argumentu typu pro deklaraci obecného typu.</span><span class="sxs-lookup"><span data-stu-id="df7d3-480">At run-time, each type parameter is bound to a run-time type that was specified by supplying a type argument to the generic type declaration.</span></span> <span data-ttu-id="df7d3-481">Proto typ proměnné deklarované s parametrem typu v době běhu bude uzavřený konstruovaný typ ([otevřené a uzavřené typy](types.md#open-and-closed-types)).</span><span class="sxs-lookup"><span data-stu-id="df7d3-481">Thus, the type of a variable declared with a type parameter will, at run-time, be a closed constructed type ([Open and closed types](types.md#open-and-closed-types)).</span></span> <span data-ttu-id="df7d3-482">Spuštění všech příkazů a výrazů týkajících se parametrů typu za běhu používá skutečný typ, který byl zadán jako argument typu pro daný parametr.</span><span class="sxs-lookup"><span data-stu-id="df7d3-482">The run-time execution of all statements and expressions involving type parameters uses the actual type that was supplied as the type argument for that parameter.</span></span>

## <a name="expression-tree-types"></a><span data-ttu-id="df7d3-483">Typy stromu výrazů</span><span class="sxs-lookup"><span data-stu-id="df7d3-483">Expression tree types</span></span>

<span data-ttu-id="df7d3-484">***Stromy výrazů*** umožňují znázornit výrazy lambda jako datové struktury namísto spustitelného kódu.</span><span class="sxs-lookup"><span data-stu-id="df7d3-484">***Expression trees*** permit lambda expressions to be represented as data structures instead of executable code.</span></span> <span data-ttu-id="df7d3-485">Stromy výrazů jsou hodnoty ***typů stromové struktury výrazu*** `System.Linq.Expressions.Expression<D>`formuláře, kde `D` je jakýkoli typ delegáta.</span><span class="sxs-lookup"><span data-stu-id="df7d3-485">Expression trees are values of ***expression tree types*** of the form `System.Linq.Expressions.Expression<D>`, where `D` is any delegate type.</span></span> <span data-ttu-id="df7d3-486">Pro zbytek této specifikace budeme na tyto typy odkazovat pomocí `Expression<D>`zkráceným na tyto typy.</span><span class="sxs-lookup"><span data-stu-id="df7d3-486">For the remainder of this specification we will refer to these types using the shorthand `Expression<D>`.</span></span>

<span data-ttu-id="df7d3-487">Pokud převod existuje z výrazu lambda na typ delegáta `D`, existuje také převod na typ stromu výrazu `Expression<D>`.</span><span class="sxs-lookup"><span data-stu-id="df7d3-487">If a conversion exists from a lambda expression to a delegate type `D`, a conversion also exists to the expression tree type `Expression<D>`.</span></span> <span data-ttu-id="df7d3-488">Zatímco převod výrazu lambda na typ delegáta vygeneruje delegáta, který odkazuje na spustitelný kód pro lambda výraz, převod na typ stromové struktury výrazu vytvoří reprezentace stromu výrazu lambda výrazu.</span><span class="sxs-lookup"><span data-stu-id="df7d3-488">Whereas the conversion of a lambda expression to a delegate type generates a delegate that references executable code for the lambda expression, conversion to an expression tree type creates an expression tree representation of the lambda expression.</span></span>

<span data-ttu-id="df7d3-489">Stromy výrazů jsou efektivní reprezentace dat v paměti pro výrazy lambda a vytvoření struktury výrazu lambda transparentní a explicitní.</span><span class="sxs-lookup"><span data-stu-id="df7d3-489">Expression trees are efficient in-memory data representations of lambda expressions and make the structure of the lambda expression transparent and explicit.</span></span>

<span data-ttu-id="df7d3-490">Podobně jako typ delegáta `D`, `Expression<D>` se říká, že mají parametry a návratové typy, které jsou stejné jako u `D`.</span><span class="sxs-lookup"><span data-stu-id="df7d3-490">Just like a delegate type `D`, `Expression<D>` is said to have parameter and return types, which are the same as those of `D`.</span></span>

<span data-ttu-id="df7d3-491">Následující příklad představuje výraz lambda jak jako spustitelný kód, tak jako strom výrazu.</span><span class="sxs-lookup"><span data-stu-id="df7d3-491">The following example represents a lambda expression both as executable code and as an expression tree.</span></span> <span data-ttu-id="df7d3-492">Vzhledem k tomu, že existuje převod na `Func<int,int>`, existuje také převod na `Expression<Func<int,int>>`:</span><span class="sxs-lookup"><span data-stu-id="df7d3-492">Because a conversion exists to `Func<int,int>`, a conversion also exists to `Expression<Func<int,int>>`:</span></span>

```csharp
Func<int,int> del = x => x + 1;                    // Code

Expression<Func<int,int>> exp = x => x + 1;        // Data
```

<span data-ttu-id="df7d3-493">Po těchto přiřazeních `del` delegát odkazuje na metodu, která vrací `x + 1`a strom výrazu `exp` odkazuje na strukturu dat, která popisuje výraz `x => x + 1`.</span><span class="sxs-lookup"><span data-stu-id="df7d3-493">Following these assignments, the delegate `del` references a method that returns `x + 1`, and the expression tree `exp` references a data structure that describes the expression `x => x + 1`.</span></span>

<span data-ttu-id="df7d3-494">Přesná definice obecného typu `Expression<D>` a také přesné pravidla pro sestavování stromu výrazu v případě, že je výraz lambda převeden na typ stromu výrazu, jsou oba mimo rozsah této specifikace.</span><span class="sxs-lookup"><span data-stu-id="df7d3-494">The exact definition of the generic type `Expression<D>` as well as the precise rules for constructing an expression tree when a lambda expression is converted to an expression tree type, are both outside the scope of this specification.</span></span>

<span data-ttu-id="df7d3-495">Existují dvě věci, které je třeba provést explicitně:</span><span class="sxs-lookup"><span data-stu-id="df7d3-495">Two things are important to make explicit:</span></span>

*  <span data-ttu-id="df7d3-496">Ne všechny výrazy lambda lze převést na stromy výrazů.</span><span class="sxs-lookup"><span data-stu-id="df7d3-496">Not all lambda expressions can be converted to expression trees.</span></span> <span data-ttu-id="df7d3-497">Například lambda výrazy s tělomi příkazů a lambda výrazy obsahující výrazy přiřazení nelze znázornit.</span><span class="sxs-lookup"><span data-stu-id="df7d3-497">For instance, lambda expressions with statement bodies, and lambda expressions containing assignment expressions cannot be represented.</span></span> <span data-ttu-id="df7d3-498">V těchto případech převod stále existuje, ale v době kompilace se nezdaří.</span><span class="sxs-lookup"><span data-stu-id="df7d3-498">In these cases, a conversion still exists, but will fail at compile-time.</span></span> <span data-ttu-id="df7d3-499">Tyto výjimky jsou podrobně popsané v [anonymních převodech funkcí](conversions.md#anonymous-function-conversions).</span><span class="sxs-lookup"><span data-stu-id="df7d3-499">These exceptions are detailed in [Anonymous function conversions](conversions.md#anonymous-function-conversions).</span></span>
*   <span data-ttu-id="df7d3-500">`Expression<D>` nabízí metodu instance `Compile`, která vytvoří delegáta typu `D`:</span><span class="sxs-lookup"><span data-stu-id="df7d3-500">`Expression<D>` offers an instance method `Compile` which produces a delegate of type `D`:</span></span>

    ```csharp
    Func<int,int> del2 = exp.Compile();
    ```

    <span data-ttu-id="df7d3-501">Volání tohoto delegáta způsobí spuštění kódu reprezentovaného stromovou strukturou výrazu.</span><span class="sxs-lookup"><span data-stu-id="df7d3-501">Invoking this delegate causes the code represented by the expression tree to be executed.</span></span> <span data-ttu-id="df7d3-502">Proto jsou s ohledem na výše uvedené definice ekvivalentní del a del2 a následující dva příkazy budou mít stejný účinek:</span><span class="sxs-lookup"><span data-stu-id="df7d3-502">Thus, given the definitions above, del and del2 are equivalent, and the following two statements will have the same effect:</span></span>

    ```csharp
    int i1 = del(1);
    
    int i2 = del2(1);
    ```

    <span data-ttu-id="df7d3-503">Po spuštění tohoto kódu budou `i1` a `i2` obě hodnoty `2`.</span><span class="sxs-lookup"><span data-stu-id="df7d3-503">After executing this code,  `i1` and `i2` will both have the value `2`.</span></span>

