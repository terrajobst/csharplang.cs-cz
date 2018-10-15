# <a name="types"></a><span data-ttu-id="d43e3-101">Typy</span><span class="sxs-lookup"><span data-stu-id="d43e3-101">Types</span></span>

<span data-ttu-id="d43e3-102">Typy jazyka C# jsou rozděleny do dvou hlavních kategorií: ***typů hodnot*** a ***referenční typy***.</span><span class="sxs-lookup"><span data-stu-id="d43e3-102">The types of the C# language are divided into two main categories: ***value types*** and ***reference types***.</span></span> <span data-ttu-id="d43e3-103">Typy hodnot a odkazové typy mohou být ***obecných typů***, což trvat jednu nebo více ***parametry typu***.</span><span class="sxs-lookup"><span data-stu-id="d43e3-103">Both value types and reference types may be ***generic types***, which take one or more ***type parameters***.</span></span> <span data-ttu-id="d43e3-104">Parametry typu můžete určit oba typy hodnot a typy odkazů.</span><span class="sxs-lookup"><span data-stu-id="d43e3-104">Type parameters can designate both value types and reference types.</span></span>

```antlr
type
    : value_type
    | reference_type
    | type_parameter
    | type_unsafe
    ;
```

<span data-ttu-id="d43e3-105">Poslední kategorie typů ukazatele, je k dispozici pouze v nezabezpečený kód.</span><span class="sxs-lookup"><span data-stu-id="d43e3-105">The final category of types, pointers, is available only in unsafe code.</span></span> <span data-ttu-id="d43e3-106">Tento postup je popsán dále v [typy ukazatelů](unsafe-code.md#pointer-types).</span><span class="sxs-lookup"><span data-stu-id="d43e3-106">This is discussed further in [Pointer types](unsafe-code.md#pointer-types).</span></span>

<span data-ttu-id="d43e3-107">Typy hodnot se liší od typy odkazů v tom, že proměnné typů hodnoty přímo obsahovat svá data, zatímco proměnné odkazu na typy úložiště ***odkazy*** ke svým datům, ten se označuje jako ***objekty***.</span><span class="sxs-lookup"><span data-stu-id="d43e3-107">Value types differ from reference types in that variables of the value types directly contain their data, whereas variables of the reference types store ***references*** to their data, the latter being known as ***objects***.</span></span> <span data-ttu-id="d43e3-108">V případě typů odkazu je možné pro dvě proměnné odkazovat na stejný objekt a proto možná pro operace v rámci jedné proměnné ovlivňovat objekt odkazovaný jinou proměnnou.</span><span class="sxs-lookup"><span data-stu-id="d43e3-108">With reference types, it is possible for two variables to reference the same object, and thus possible for operations on one variable to affect the object referenced by the other variable.</span></span> <span data-ttu-id="d43e3-109">S typy hodnot proměnných každý mají své vlastní kopii dat a není možné pro operace se na nich se má vliv na jinou.</span><span class="sxs-lookup"><span data-stu-id="d43e3-109">With value types, the variables each have their own copy of the data, and it is not possible for operations on one to affect the other.</span></span>

<span data-ttu-id="d43e3-110">Systém typů jazyka C# je jednotný tak, aby jako objekt lze považovat hodnotu libovolného typu.</span><span class="sxs-lookup"><span data-stu-id="d43e3-110">C#'s type system is unified such that a value of any type can be treated as an object.</span></span> <span data-ttu-id="d43e3-111">Všechny typy v jazyce C# přímo nebo nepřímo odvozuje od `object` typ, třídy a `object` je ultimate základní třída všech typů.</span><span class="sxs-lookup"><span data-stu-id="d43e3-111">Every type in C# directly or indirectly derives from the `object` class type, and `object` is the ultimate base class of all types.</span></span> <span data-ttu-id="d43e3-112">Hodnoty odkazové typy jsou považovány za objekty jednoduše tak, že zobrazení hodnoty jako typ `object`.</span><span class="sxs-lookup"><span data-stu-id="d43e3-112">Values of reference types are treated as objects simply by viewing the values as type `object`.</span></span> <span data-ttu-id="d43e3-113">Hodnoty typy hodnot jsou považovány za objekty pomocí provádí operace zabalení a rozbalení ([zabalení a rozbalení](types.md#boxing-and-unboxing)).</span><span class="sxs-lookup"><span data-stu-id="d43e3-113">Values of value types are treated as objects by performing boxing and unboxing operations ([Boxing and unboxing](types.md#boxing-and-unboxing)).</span></span>

## <a name="value-types"></a><span data-ttu-id="d43e3-114">Typy hodnot</span><span class="sxs-lookup"><span data-stu-id="d43e3-114">Value types</span></span>

<span data-ttu-id="d43e3-115">Typ hodnoty je struktura typu nebo typu výčtu.</span><span class="sxs-lookup"><span data-stu-id="d43e3-115">A value type is either a struct type or an enumeration type.</span></span> <span data-ttu-id="d43e3-116">Jazyk C# poskytuje sadu předdefinovaných struktury typů volá se, ***jednoduché typy***.</span><span class="sxs-lookup"><span data-stu-id="d43e3-116">C# provides a set of predefined struct types called the ***simple types***.</span></span> <span data-ttu-id="d43e3-117">Jednoduché typy jsou označeny pomocí vyhrazených slov.</span><span class="sxs-lookup"><span data-stu-id="d43e3-117">The simple types are identified through reserved words.</span></span>

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

<span data-ttu-id="d43e3-118">Na rozdíl od proměnné typu odkazu, může obsahovat proměnné hodnotového typu hodnota `null` pouze v případě, že typ hodnoty je typ připouštějící hodnotu Null.</span><span class="sxs-lookup"><span data-stu-id="d43e3-118">Unlike a variable of a reference type, a variable of a value type can contain the value `null` only if the value type is a nullable type.</span></span>  <span data-ttu-id="d43e3-119">Pro každý typ hodnotu null je odpovídající typ s možnou hodnotou Null představující stejnou sadu hodnot a hodnoty `null`.</span><span class="sxs-lookup"><span data-stu-id="d43e3-119">For every non-nullable value type there is a corresponding nullable value type denoting the same set of values plus the value `null`.</span></span>

<span data-ttu-id="d43e3-120">Přiřazení proměnné typu hodnoty vytvoří kopii přiřazené hodnoty.</span><span class="sxs-lookup"><span data-stu-id="d43e3-120">Assignment to a variable of a value type creates a copy of the value being assigned.</span></span> <span data-ttu-id="d43e3-121">Tím se liší od přiřazení k proměnné typu odkazu, který zkopíruje odkaz, ale ne identifikovaný odkaz na objekt.</span><span class="sxs-lookup"><span data-stu-id="d43e3-121">This differs from assignment to a variable of a reference type, which copies the reference but not the object identified by the reference.</span></span>

### <a name="the-systemvaluetype-type"></a><span data-ttu-id="d43e3-122">Typ System.ValueType</span><span class="sxs-lookup"><span data-stu-id="d43e3-122">The System.ValueType type</span></span>

<span data-ttu-id="d43e3-123">Všechny hodnotové typy implicitně dědí z třídy `System.ValueType`, který zase dědí z třídy `object`.</span><span class="sxs-lookup"><span data-stu-id="d43e3-123">All value types implicitly inherit from the class `System.ValueType`, which, in turn, inherits from class `object`.</span></span> <span data-ttu-id="d43e3-124">Není možné pro jakýkoli typ odvození od typu hodnoty a typy hodnot jsou proto implicitně zapečetěné ([zapečetěné třídy](classes.md#sealed-classes)).</span><span class="sxs-lookup"><span data-stu-id="d43e3-124">It is not possible for any type to derive from a value type, and value types are thus implicitly sealed ([Sealed classes](classes.md#sealed-classes)).</span></span>

<span data-ttu-id="d43e3-125">Všimněte si, že `System.ValueType` sám není *value_type*.</span><span class="sxs-lookup"><span data-stu-id="d43e3-125">Note that `System.ValueType` is not itself a *value_type*.</span></span> <span data-ttu-id="d43e3-126">Místo toho je *class_type* z kterého jsou všechny *value_type*s je automaticky odvozen.</span><span class="sxs-lookup"><span data-stu-id="d43e3-126">Rather, it is a *class_type* from which all *value_type*s are automatically derived.</span></span>

### <a name="default-constructors"></a><span data-ttu-id="d43e3-127">Výchozí konstruktory</span><span class="sxs-lookup"><span data-stu-id="d43e3-127">Default constructors</span></span>

<span data-ttu-id="d43e3-128">Všechny hodnotové typy implicitně deklarovat konstruktor instance bez parametrů, volá se, ***výchozí konstruktor***.</span><span class="sxs-lookup"><span data-stu-id="d43e3-128">All value types implicitly declare a public parameterless instance constructor called the ***default constructor***.</span></span> <span data-ttu-id="d43e3-129">Výchozí konstruktor vrátí instanci inicializován nulou označované jako ***výchozí hodnota*** pro typ hodnoty:</span><span class="sxs-lookup"><span data-stu-id="d43e3-129">The default constructor returns a zero-initialized instance known as the ***default value*** for the value type:</span></span>

*  <span data-ttu-id="d43e3-130">Pro všechny *simple_type*s, výchozí hodnota je hodnotu vytvořenou testovaným bitový vzor všechny nul:</span><span class="sxs-lookup"><span data-stu-id="d43e3-130">For all *simple_type*s, the default value is the value produced by a bit pattern of all zeros:</span></span>
    * <span data-ttu-id="d43e3-131">Pro `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, a `ulong`, výchozí hodnota je `0`.</span><span class="sxs-lookup"><span data-stu-id="d43e3-131">For `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, and `ulong`, the default value is `0`.</span></span>
    * <span data-ttu-id="d43e3-132">Pro `char`, výchozí hodnota je `'\x0000'`.</span><span class="sxs-lookup"><span data-stu-id="d43e3-132">For `char`, the default value is `'\x0000'`.</span></span>
    * <span data-ttu-id="d43e3-133">Pro `float`, výchozí hodnota je `0.0f`.</span><span class="sxs-lookup"><span data-stu-id="d43e3-133">For `float`, the default value is `0.0f`.</span></span>
    * <span data-ttu-id="d43e3-134">Pro `double`, výchozí hodnota je `0.0d`.</span><span class="sxs-lookup"><span data-stu-id="d43e3-134">For `double`, the default value is `0.0d`.</span></span>
    * <span data-ttu-id="d43e3-135">Pro `decimal`, výchozí hodnota je `0.0m`.</span><span class="sxs-lookup"><span data-stu-id="d43e3-135">For `decimal`, the default value is `0.0m`.</span></span>
    * <span data-ttu-id="d43e3-136">Pro `bool`, výchozí hodnota je `false`.</span><span class="sxs-lookup"><span data-stu-id="d43e3-136">For `bool`, the default value is `false`.</span></span>
*  <span data-ttu-id="d43e3-137">Pro *enum_type* `E`, výchozí hodnota je `0`, převést na typ `E`.</span><span class="sxs-lookup"><span data-stu-id="d43e3-137">For an *enum_type* `E`, the default value is `0`, converted to the type `E`.</span></span>
*  <span data-ttu-id="d43e3-138">Pro *struct_type*, výchozí hodnota je hodnota vytvořen nastavením všechna pole na výchozí hodnoty a referenční dokumentace všech polí typu `null`.</span><span class="sxs-lookup"><span data-stu-id="d43e3-138">For a *struct_type*, the default value is the value produced by setting all value type fields to their default value and all reference type fields to `null`.</span></span>
*  <span data-ttu-id="d43e3-139">Pro *nullable_type* výchozí hodnota je instance, pro kterou `HasValue` vlastnost má hodnotu false a `Value` vlastnost není definována.</span><span class="sxs-lookup"><span data-stu-id="d43e3-139">For a *nullable_type* the default value is an instance for which the `HasValue` property is false and the `Value` property is undefined.</span></span> <span data-ttu-id="d43e3-140">Výchozí hodnota je také ***hodnota null*** typu s možnou hodnotou Null.</span><span class="sxs-lookup"><span data-stu-id="d43e3-140">The default value is also known as the ***null value*** of the nullable type.</span></span>

<span data-ttu-id="d43e3-141">Stejně jako ostatní konstruktor instance je vyvolán pomocí výchozího konstruktoru typu hodnoty `new` operátor.</span><span class="sxs-lookup"><span data-stu-id="d43e3-141">Like any other instance constructor, the default constructor of a value type is invoked using the `new` operator.</span></span> <span data-ttu-id="d43e3-142">Z důvodu efektivity není tento požadavek určen ve skutečnosti mít implementaci generovat volání konstruktoru.</span><span class="sxs-lookup"><span data-stu-id="d43e3-142">For efficiency reasons, this requirement is not intended to actually have the implementation generate a constructor call.</span></span> <span data-ttu-id="d43e3-143">V příkladu níže proměnné `i` a `j` jsou inicializovány na nulu.</span><span class="sxs-lookup"><span data-stu-id="d43e3-143">In the example below, variables `i` and `j` are both initialized to zero.</span></span>

```csharp
class A
{
    void F() {
        int i = 0;
        int j = new int();
    }
}
```

<span data-ttu-id="d43e3-144">Vzhledem k tomu, že každý typ hodnoty implicitně obsahuje konstruktor instance bez parametrů, není možné u typu struktura obsahuje explicitní deklarace konstruktor bez parametrů.</span><span class="sxs-lookup"><span data-stu-id="d43e3-144">Because every value type implicitly has a public parameterless instance constructor, it is not possible for a struct type to contain an explicit declaration of a parameterless constructor.</span></span> <span data-ttu-id="d43e3-145">Chcete-li deklarovat instanci parametrizované konstruktory ale smí typu Struktura ([konstruktory](structs.md#constructors)).</span><span class="sxs-lookup"><span data-stu-id="d43e3-145">A struct type is however permitted to declare parameterized instance constructors ([Constructors](structs.md#constructors)).</span></span>

### <a name="struct-types"></a><span data-ttu-id="d43e3-146">Typy struktury</span><span class="sxs-lookup"><span data-stu-id="d43e3-146">Struct types</span></span>

<span data-ttu-id="d43e3-147">Typ struktura je hodnotový typ, který lze deklarovat konstanty, pole, metody, vlastnosti, indexery, operátory, konstruktory instancí, statické konstruktory a vnořené typy.</span><span class="sxs-lookup"><span data-stu-id="d43e3-147">A struct type is a value type that can declare constants, fields, methods, properties, indexers, operators, instance constructors, static constructors, and nested types.</span></span> <span data-ttu-id="d43e3-148">Deklarace typy struktury je popsána v [deklarace struktury](structs.md#struct-declarations).</span><span class="sxs-lookup"><span data-stu-id="d43e3-148">The declaration of struct types is described in [Struct declarations](structs.md#struct-declarations).</span></span>

### <a name="simple-types"></a><span data-ttu-id="d43e3-149">Jednoduché typy</span><span class="sxs-lookup"><span data-stu-id="d43e3-149">Simple types</span></span>

<span data-ttu-id="d43e3-150">Jazyk C# poskytuje sadu předdefinovaných struktury typů volá se, ***jednoduché typy***.</span><span class="sxs-lookup"><span data-stu-id="d43e3-150">C# provides a set of predefined struct types called the ***simple types***.</span></span> <span data-ttu-id="d43e3-151">Jednoduché typy jsou označeny pomocí vyhrazených slov, ale jsou jednoduše aliasy pro typy předdefinované struktury v těchto vyhrazených slov `System` obor názvů, jak je popsáno v následující tabulce.</span><span class="sxs-lookup"><span data-stu-id="d43e3-151">The simple types are identified through reserved words, but these reserved words are simply aliases for predefined struct types in the `System` namespace, as described in the table below.</span></span>


| <span data-ttu-id="d43e3-152">__Vyhrazené slovo__</span><span class="sxs-lookup"><span data-stu-id="d43e3-152">__Reserved word__</span></span> | <span data-ttu-id="d43e3-153">__Typ s aliasem__</span><span class="sxs-lookup"><span data-stu-id="d43e3-153">__Aliased type__</span></span> |
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

<span data-ttu-id="d43e3-154">Protože jednoduchý typ aliasy typu Struktura, má každý jednoduchý typ členů.</span><span class="sxs-lookup"><span data-stu-id="d43e3-154">Because a simple type aliases a struct type, every simple type has members.</span></span> <span data-ttu-id="d43e3-155">Například `int` má členy deklarované v `System.Int32` a členy zděděné z `System.Object`, a jsou povoleny následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="d43e3-155">For example, `int` has the members declared in `System.Int32` and the members inherited from `System.Object`, and the following statements are permitted:</span></span>

```csharp
int i = int.MaxValue;           // System.Int32.MaxValue constant
string s = i.ToString();        // System.Int32.ToString() instance method
string t = 123.ToString();      // System.Int32.ToString() instance method
```

<span data-ttu-id="d43e3-156">Jednoduché typy liší od jiné typy struct, že umožňují některé další operace:</span><span class="sxs-lookup"><span data-stu-id="d43e3-156">The simple types differ from other struct types in that they permit certain additional operations:</span></span>

*  <span data-ttu-id="d43e3-157">Povolit většinu jednoduchých typů hodnot má být vytvořen napsáním *literály* ([literály](lexical-structure.md#literals)).</span><span class="sxs-lookup"><span data-stu-id="d43e3-157">Most simple types permit values to be created by writing *literals* ([Literals](lexical-structure.md#literals)).</span></span> <span data-ttu-id="d43e3-158">Například `123` je literál typu `int` a `'a'` je literál typu `char`.</span><span class="sxs-lookup"><span data-stu-id="d43e3-158">For example, `123` is a literal of type `int` and `'a'` is a literal of type `char`.</span></span> <span data-ttu-id="d43e3-159">C# umožňuje žádné ustanovení pro literály typy struktury obecně a jiné než výchozí hodnoty jiné typy struktury jsou nakonec vždy vytvořené prostřednictvím konstruktory instancí z těchto typů struktury.</span><span class="sxs-lookup"><span data-stu-id="d43e3-159">C# makes no provision for literals of struct types in general, and non-default values of other struct types are ultimately always created through instance constructors of those struct types.</span></span>
*  <span data-ttu-id="d43e3-160">Když jsou jako operandy výrazu všechny jednoduchý typ konstanty, je možné kompilátor pro vyhodnocení výrazu v době kompilace.</span><span class="sxs-lookup"><span data-stu-id="d43e3-160">When the operands of an expression are all simple type constants, it is possible for the compiler to evaluate the expression at compile-time.</span></span> <span data-ttu-id="d43e3-161">Takový výraz se označuje jako *constant_expression* ([konstantní výrazy](expressions.md#constant-expressions)).</span><span class="sxs-lookup"><span data-stu-id="d43e3-161">Such an expression is known as a *constant_expression* ([Constant expressions](expressions.md#constant-expressions)).</span></span> <span data-ttu-id="d43e3-162">Výrazy zahrnující operátory definované ve jiné typy struct se nepovažují za konstantní výrazy.</span><span class="sxs-lookup"><span data-stu-id="d43e3-162">Expressions involving operators defined by other struct types are not considered to be constant expressions.</span></span>
*  <span data-ttu-id="d43e3-163">Prostřednictvím `const` deklarace je možné deklarovat konstanty jednoduché typy ([konstanty](classes.md#constants)).</span><span class="sxs-lookup"><span data-stu-id="d43e3-163">Through `const` declarations it is possible to declare constants of the simple types ([Constants](classes.md#constants)).</span></span> <span data-ttu-id="d43e3-164">Není možné mít konstanty jiné typy struct, ale Azure nenabízí podobné efekt `static readonly` pole.</span><span class="sxs-lookup"><span data-stu-id="d43e3-164">It is not possible to have constants of other struct types, but a similar effect is provided by `static readonly` fields.</span></span>
*  <span data-ttu-id="d43e3-165">Jednoduché typy zahrnující převody mohl podílet na vyhodnocení operátorů převodu definován jiným typem struct, ale operátor uživatelsky definovaný převod nikdy mohl podílet na vyhodnocení jiného uživatelem definovaný operátor ([hodnocení uživatelem definované převody](conversions.md#evaluation-of-user-defined-conversions)).</span><span class="sxs-lookup"><span data-stu-id="d43e3-165">Conversions involving simple types can participate in evaluation of conversion operators defined by other struct types, but a user-defined conversion operator can never participate in evaluation of another user-defined operator ([Evaluation of user-defined conversions](conversions.md#evaluation-of-user-defined-conversions)).</span></span>

### <a name="integral-types"></a><span data-ttu-id="d43e3-166">Celočíselné typy</span><span class="sxs-lookup"><span data-stu-id="d43e3-166">Integral types</span></span>

<span data-ttu-id="d43e3-167">C# podporuje devět celočíselnými typy: `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, a `char`.</span><span class="sxs-lookup"><span data-stu-id="d43e3-167">C# supports nine integral types: `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, and `char`.</span></span> <span data-ttu-id="d43e3-168">Integrální typy mají následující velikostí a oblastí hodnot:</span><span class="sxs-lookup"><span data-stu-id="d43e3-168">The integral types have the following sizes and ranges of values:</span></span>

*  <span data-ttu-id="d43e3-169">`sbyte` Představuje typ podepsaný 8bitová celá čísla se hodnoty v rozmezí -128 až 127.</span><span class="sxs-lookup"><span data-stu-id="d43e3-169">The `sbyte` type represents signed 8-bit integers with values between -128 and 127.</span></span>
*  <span data-ttu-id="d43e3-170">`byte` Představuje typ bez znaménka 8bitová celá čísla se hodnoty v rozmezí 0 až 255.</span><span class="sxs-lookup"><span data-stu-id="d43e3-170">The `byte` type represents unsigned 8-bit integers with values between 0 and 255.</span></span>
*  <span data-ttu-id="d43e3-171">`short` Představuje typ podepsaný 16bitová celá čísla s hodnotami mezi -32768 a 32767.</span><span class="sxs-lookup"><span data-stu-id="d43e3-171">The `short` type represents signed 16-bit integers with values between -32768 and 32767.</span></span>
*  <span data-ttu-id="d43e3-172">`ushort` Typ představuje celých čísel bez znaménka 16bitové hodnoty mezi 0 a 65535.</span><span class="sxs-lookup"><span data-stu-id="d43e3-172">The `ushort` type represents unsigned 16-bit integers with values between 0 and 65535.</span></span>
*  <span data-ttu-id="d43e3-173">`int` Představuje typ podepsaný 32bitová celá čísla s hodnotami mezi -2147483648 a 2147483647.</span><span class="sxs-lookup"><span data-stu-id="d43e3-173">The `int` type represents signed 32-bit integers with values between -2147483648 and 2147483647.</span></span>
*  <span data-ttu-id="d43e3-174">`uint` Představuje typ bez znaménka 32bitových celých čísel pomocí hodnoty v rozmezí 0 až 4294967295.</span><span class="sxs-lookup"><span data-stu-id="d43e3-174">The `uint` type represents unsigned 32-bit integers with values between 0 and 4294967295.</span></span>
*  <span data-ttu-id="d43e3-175">`long` Představuje typ podepsaný 64bitová celá čísla mezi -9223372036854775808 a 9223372036854775807 hodnotami.</span><span class="sxs-lookup"><span data-stu-id="d43e3-175">The `long` type represents signed 64-bit integers with values between -9223372036854775808 and 9223372036854775807.</span></span>
*  <span data-ttu-id="d43e3-176">`ulong` Představuje typ bez znaménka 64bitová celá čísla se hodnoty v rozmezí 0 až 18446744073709551615.</span><span class="sxs-lookup"><span data-stu-id="d43e3-176">The `ulong` type represents unsigned 64-bit integers with values between 0 and 18446744073709551615.</span></span>
*  <span data-ttu-id="d43e3-177">`char` Typ představuje celých čísel bez znaménka 16bitové hodnoty mezi 0 a 65535.</span><span class="sxs-lookup"><span data-stu-id="d43e3-177">The `char` type represents unsigned 16-bit integers with values between 0 and 65535.</span></span> <span data-ttu-id="d43e3-178">Sadu možných hodnot `char` typu odpovídá znakové sady Unicode.</span><span class="sxs-lookup"><span data-stu-id="d43e3-178">The set of possible values for the `char` type corresponds to the Unicode character set.</span></span> <span data-ttu-id="d43e3-179">I když `char` má stejnou reprezentaci jako `ushort`, ne všechny operace povolena u jednoho typu nejsou povoleny na druhé.</span><span class="sxs-lookup"><span data-stu-id="d43e3-179">Although `char` has the same representation as `ushort`, not all operations permitted on one type are permitted on the other.</span></span>

<span data-ttu-id="d43e3-180">Integrálového typu jednočlenné a binární operátory jsou vždy pracovat s podepsaný 32 bitů přesnosti, bez znaménka 32 bitů přesnosti, podepsaný 64 bitů přesnosti nebo bez znaménka 64 bitů přesnosti:</span><span class="sxs-lookup"><span data-stu-id="d43e3-180">The integral-type unary and binary operators always operate with signed 32-bit precision, unsigned 32-bit precision, signed 64-bit precision, or unsigned 64-bit precision:</span></span>

*  <span data-ttu-id="d43e3-181">Pro unární `+` a `~` operátory, operand je převeden na typ `T`, kde `T` je prvním příspěvkem `int`, `uint`, `long`, a `ulong` plně, který může představovat všechny možné hodnoty operandu.</span><span class="sxs-lookup"><span data-stu-id="d43e3-181">For the unary `+` and `~` operators, the operand is converted to type `T`, where `T` is the first of `int`, `uint`, `long`, and `ulong` that can fully represent all possible values of the operand.</span></span> <span data-ttu-id="d43e3-182">Operace se pak provede pomocí přesnost typu `T`, a typ výsledku je `T`.</span><span class="sxs-lookup"><span data-stu-id="d43e3-182">The operation is then performed using the precision of type `T`, and the type of the result is `T`.</span></span>
*  <span data-ttu-id="d43e3-183">Pro unární `-` operátor operand je převeden na typ `T`, kde `T` je prvním příspěvkem `int` a `long` plně představující všechny možné hodnoty operandu.</span><span class="sxs-lookup"><span data-stu-id="d43e3-183">For the unary `-` operator, the operand is converted to type `T`, where `T` is the first of `int` and `long` that can fully represent all possible values of the operand.</span></span> <span data-ttu-id="d43e3-184">Operace se pak provede pomocí přesnost typu `T`, a typ výsledku je `T`.</span><span class="sxs-lookup"><span data-stu-id="d43e3-184">The operation is then performed using the precision of type `T`, and the type of the result is `T`.</span></span> <span data-ttu-id="d43e3-185">Unární `-` operátor nelze použít pro operandy typu `ulong`.</span><span class="sxs-lookup"><span data-stu-id="d43e3-185">The unary `-` operator cannot be applied to operands of type `ulong`.</span></span>
*  <span data-ttu-id="d43e3-186">Pro binární soubor `+`, `-`, `*`, `/`, `%`, `&`, `^`, `|`, `==`, `!=`, `>`, `<`, `>=`, a `<=` operátory, operandy jsou převedeny na typ `T`, kde `T` je prvním příspěvkem `int`, `uint`, `long`, a `ulong` plně představující všechny možné hodnoty pro oba operandy.</span><span class="sxs-lookup"><span data-stu-id="d43e3-186">For the binary `+`, `-`, `*`, `/`, `%`, `&`, `^`, `|`, `==`, `!=`, `>`, `<`, `>=`, and `<=` operators, the operands are converted to type `T`, where `T` is the first of `int`, `uint`, `long`, and `ulong` that can fully represent all possible values of both operands.</span></span> <span data-ttu-id="d43e3-187">Operace se pak provede pomocí přesnost typu `T`, a typ výsledku je `T` (nebo `bool` pro relační operátory).</span><span class="sxs-lookup"><span data-stu-id="d43e3-187">The operation is then performed using the precision of type `T`, and the type of the result is `T` (or `bool` for the relational operators).</span></span> <span data-ttu-id="d43e3-188">Není povoleno pro jeden operand typu `long` a druhou typu `ulong` s binárními operátory.</span><span class="sxs-lookup"><span data-stu-id="d43e3-188">It is not permitted for one operand to be of type `long` and the other to be of type `ulong` with the binary operators.</span></span>
*  <span data-ttu-id="d43e3-189">Pro binární soubor `<<` a `>>` operátory, levý operand je převeden na typ `T`, kde `T` je prvním příspěvkem `int`, `uint`, `long`, a `ulong` plně, který může představovat všechny možné hodnoty operandu.</span><span class="sxs-lookup"><span data-stu-id="d43e3-189">For the binary `<<` and `>>` operators, the left operand is converted to type `T`, where `T` is the first of `int`, `uint`, `long`, and `ulong` that can fully represent all possible values of the operand.</span></span> <span data-ttu-id="d43e3-190">Operace se pak provede pomocí přesnost typu `T`, a typ výsledku je `T`.</span><span class="sxs-lookup"><span data-stu-id="d43e3-190">The operation is then performed using the precision of type `T`, and the type of the result is `T`.</span></span>

<span data-ttu-id="d43e3-191">`char` Typ je klasifikován jako s integrálním typem, ale liší se od jiné typy celých čísel dvěma způsoby:</span><span class="sxs-lookup"><span data-stu-id="d43e3-191">The `char` type is classified as an integral type, but it differs from the other integral types in two ways:</span></span>

*  <span data-ttu-id="d43e3-192">Neexistují žádné implicitní převody z jiných typů `char` typu.</span><span class="sxs-lookup"><span data-stu-id="d43e3-192">There are no implicit conversions from other types to the `char` type.</span></span> <span data-ttu-id="d43e3-193">Zejména i když `sbyte`, `byte`, a `ushort` typy mají rozsahy hodnot, které jsou plně reprezentovat pomocí `char` implicitní převody z typů `sbyte`, `byte`, nebo `ushort` na `char` neexistují.</span><span class="sxs-lookup"><span data-stu-id="d43e3-193">In particular, even though the `sbyte`, `byte`, and `ushort` types have ranges of values that are fully representable using the `char` type, implicit conversions from `sbyte`, `byte`, or `ushort` to `char` do not exist.</span></span>
*  <span data-ttu-id="d43e3-194">Konstanty objektu `char` typ musí zapsat, protože *character_literal*s nebo jako *integer_literal*s v kombinaci s přetypování na typ `char`.</span><span class="sxs-lookup"><span data-stu-id="d43e3-194">Constants of the `char` type must be written as *character_literal*s or as *integer_literal*s in combination with a cast to type `char`.</span></span> <span data-ttu-id="d43e3-195">Například `(char)10` je stejný jako `'\x000A'`.</span><span class="sxs-lookup"><span data-stu-id="d43e3-195">For example, `(char)10` is the same as `'\x000A'`.</span></span>

<span data-ttu-id="d43e3-196">`checked` a `unchecked` operátorů a příkazů, které se používají k řízení přetečení zjišťování integrálového typu aritmetické operace a převody ([operátory zaškrtnuto a nezaškrtnuto](expressions.md#the-checked-and-unchecked-operators)).</span><span class="sxs-lookup"><span data-stu-id="d43e3-196">The `checked` and `unchecked` operators and statements are used to control overflow checking for integral-type arithmetic operations and conversions ([The checked and unchecked operators](expressions.md#the-checked-and-unchecked-operators)).</span></span> <span data-ttu-id="d43e3-197">V `checked` kontextu, přetečení způsobí chybu kompilace nebo způsobí, že `System.OverflowException` vyvolání.</span><span class="sxs-lookup"><span data-stu-id="d43e3-197">In a `checked` context, an overflow produces a compile-time error or causes a `System.OverflowException` to be thrown.</span></span> <span data-ttu-id="d43e3-198">V `unchecked` kontextu, jsou ignorovány přetečení a se zahodí všechny nejvyšším bity, které se nevejdou do cílového typu.</span><span class="sxs-lookup"><span data-stu-id="d43e3-198">In an `unchecked` context, overflows are ignored and any high-order bits that do not fit in the destination type are discarded.</span></span>

### <a name="floating-point-types"></a><span data-ttu-id="d43e3-199">Typy s plovoucí desetinnou čárkou</span><span class="sxs-lookup"><span data-stu-id="d43e3-199">Floating point types</span></span>

<span data-ttu-id="d43e3-200">C# podporuje dva typy s plovoucí desetinnou čárkou: `float` a `double`.</span><span class="sxs-lookup"><span data-stu-id="d43e3-200">C# supports two floating point types: `float` and `double`.</span></span> <span data-ttu-id="d43e3-201">`float` a `double` typy jsou reprezentovány pomocí 32-bit jednoduchou přesností a 64-bit dvojité přesnosti IEEE 754 formátů, což poskytuje následující sadu hodnot:</span><span class="sxs-lookup"><span data-stu-id="d43e3-201">The `float` and `double` types are represented using the 32-bit single-precision and 64-bit double-precision IEEE 754 formats, which provide the following sets of values:</span></span>

*  <span data-ttu-id="d43e3-202">Kladné nula a záporná nula.</span><span class="sxs-lookup"><span data-stu-id="d43e3-202">Positive zero and negative zero.</span></span> <span data-ttu-id="d43e3-203">Ve většině situací, kladné nula a záporná nula chovají stejně jako jednoduchý hodnota nula, ale určité operace rozlišovat mezi dvěma ([operátor dělení](expressions.md#division-operator)).</span><span class="sxs-lookup"><span data-stu-id="d43e3-203">In most situations, positive zero and negative zero behave identically as the simple value zero, but certain operations distinguish between the two ([Division operator](expressions.md#division-operator)).</span></span>
*  <span data-ttu-id="d43e3-204">Kladné nekonečno a záporné nekonečno.</span><span class="sxs-lookup"><span data-stu-id="d43e3-204">Positive infinity and negative infinity.</span></span> <span data-ttu-id="d43e3-205">Tyto operace jako nenulové číslo dělení nulou vytvářených nekonečno.</span><span class="sxs-lookup"><span data-stu-id="d43e3-205">Infinities are produced by such operations as dividing a non-zero number by zero.</span></span> <span data-ttu-id="d43e3-206">Například `1.0 / 0.0` vrací kladné nekonečno, a `-1.0 / 0.0` výnosy záporné nekonečno.</span><span class="sxs-lookup"><span data-stu-id="d43e3-206">For example, `1.0 / 0.0` yields positive infinity, and `-1.0 / 0.0` yields negative infinity.</span></span>
*  <span data-ttu-id="d43e3-207">***Not a Number*** hodnoty často označovaná zkratkou NaN.</span><span class="sxs-lookup"><span data-stu-id="d43e3-207">The ***Not-a-Number*** value, often abbreviated NaN.</span></span> <span data-ttu-id="d43e3-208">Neplatná operace s plovoucí desetinnou čárkou, např. dělení nulou nula generované hodnoty NaN.</span><span class="sxs-lookup"><span data-stu-id="d43e3-208">NaNs are produced by invalid floating-point operations, such as dividing zero by zero.</span></span>
*  <span data-ttu-id="d43e3-209">Konečná sada nenulové hodnoty ve formuláři `s * m * 2^e`, kde `s` 1 nebo -1, a `m` a `e` se určují podle konkrétního typu s plovoucí desetinnou čárkou: pro `float`, `0 < m < 2^24` a `-149 <= e <= 104`a pro `double`, `0 < m < 2^53` a `1075 <= e <= 970`.</span><span class="sxs-lookup"><span data-stu-id="d43e3-209">The finite set of non-zero values of the form `s * m * 2^e`, where `s` is 1 or -1, and `m` and `e` are determined by the particular floating-point type: For `float`, `0 < m < 2^24` and `-149 <= e <= 104`, and for `double`, `0 < m < 2^53` and `1075 <= e <= 970`.</span></span> <span data-ttu-id="d43e3-210">Nenormalizovaná čísla s plovoucí desetinnou čárkou jsou považovány za platné nenulové hodnoty.</span><span class="sxs-lookup"><span data-stu-id="d43e3-210">Denormalized floating-point numbers are considered valid non-zero values.</span></span>

<span data-ttu-id="d43e3-211">`float` Typ může představovat hodnotu přibližně `1.5 * 10^-45` k `3.4 * 10^38` s přesností 7 číslic.</span><span class="sxs-lookup"><span data-stu-id="d43e3-211">The `float` type can represent values ranging from approximately `1.5 * 10^-45` to `3.4 * 10^38` with a precision of 7 digits.</span></span>

<span data-ttu-id="d43e3-212">`double` Typ může představovat hodnotu přibližně `5.0 * 10^-324` k `1.7 × 10^308` s přesností 15 až 16 číslic.</span><span class="sxs-lookup"><span data-stu-id="d43e3-212">The `double` type can represent values ranging from approximately `5.0 * 10^-324` to `1.7 × 10^308` with a precision of 15-16 digits.</span></span>

<span data-ttu-id="d43e3-213">Pokud jeden z operandů binárního operátoru s plovoucí desetinnou čárkou typu, pak je druhý operand musí být integrálního typu nebo typu s plovoucí desetinnou čárkou a operace se vyhodnotí takto:</span><span class="sxs-lookup"><span data-stu-id="d43e3-213">If one of the operands of a binary operator is of a floating-point type, then the other operand must be of an integral type or a floating-point type, and the operation is evaluated as follows:</span></span>

*  <span data-ttu-id="d43e3-214">Pokud jeden z operandů je typu celé číslo, tento operand je převeden na typ s plovoucí desetinnou čárkou je druhý operand.</span><span class="sxs-lookup"><span data-stu-id="d43e3-214">If one of the operands is of an integral type, then that operand is converted to the floating-point type of the other operand.</span></span>
*  <span data-ttu-id="d43e3-215">Potom, pokud platí některá z operandů typu `double`, je druhý operand je převeden na `double`, operace se provádí pomocí alespoň `double` rozsah a přesnost a typ výsledku je `double` (nebo `bool` pro relační operátory).</span><span class="sxs-lookup"><span data-stu-id="d43e3-215">Then, if either of the operands is of type `double`, the other operand is converted to `double`, the operation is performed using at least `double` range and precision, and the type of the result is `double` (or `bool` for the relational operators).</span></span>
*  <span data-ttu-id="d43e3-216">V opačném případě se operace provádí používat alespoň `float` rozsah a přesnost a typ výsledku je `float` (nebo `bool` pro relační operátory).</span><span class="sxs-lookup"><span data-stu-id="d43e3-216">Otherwise, the operation is performed using at least `float` range and precision, and the type of the result is `float` (or `bool` for the relational operators).</span></span>

<span data-ttu-id="d43e3-217">Operátory s plovoucí desetinnou čárkou, včetně operátorů přiřazení nikdy vytvořit výjimky.</span><span class="sxs-lookup"><span data-stu-id="d43e3-217">The floating-point operators, including the assignment operators, never produce exceptions.</span></span> <span data-ttu-id="d43e3-218">Místo toho ve výjimečných případech s plovoucí desetinnou čárkou operací se dostaví nula, nekonečno a NaN, jak je popsáno níže:</span><span class="sxs-lookup"><span data-stu-id="d43e3-218">Instead, in exceptional situations, floating-point operations produce zero, infinity, or NaN, as described below:</span></span>

*  <span data-ttu-id="d43e3-219">Pokud výsledek operace s plovoucí desetinnou čárkou je příliš malá pro cílový formát, výsledek operace stane kladné nula nebo záporná nula.</span><span class="sxs-lookup"><span data-stu-id="d43e3-219">If the result of a floating-point operation is too small for the destination format, the result of the operation becomes positive zero or negative zero.</span></span>
*  <span data-ttu-id="d43e3-220">Pokud výsledek operace s plovoucí desetinnou čárkou je příliš velký pro cílový formát, bude výsledek operace kladné nekonečno nebo záporné nekonečno.</span><span class="sxs-lookup"><span data-stu-id="d43e3-220">If the result of a floating-point operation is too large for the destination format, the result of the operation becomes positive infinity or negative infinity.</span></span>
*  <span data-ttu-id="d43e3-221">Výsledek operace s plovoucí desetinnou čárkou operace je neplatná, stane NaN.</span><span class="sxs-lookup"><span data-stu-id="d43e3-221">If a floating-point operation is invalid, the result of the operation becomes NaN.</span></span>
*  <span data-ttu-id="d43e3-222">Pokud jeden nebo oba operandy operaci s plovoucí desetinnou čárkou je NaN, stane se výsledek operace NaN.</span><span class="sxs-lookup"><span data-stu-id="d43e3-222">If one or both operands of a floating-point operation is NaN, the result of the operation becomes NaN.</span></span>

<span data-ttu-id="d43e3-223">S vyšší přesností než typ výsledku operace lze provádět operace s plovoucí desetinnou čárkou.</span><span class="sxs-lookup"><span data-stu-id="d43e3-223">Floating-point operations may be performed with higher precision than the result type of the operation.</span></span> <span data-ttu-id="d43e3-224">Například některé hardwarovou architekturou podporovat "Rozšířená" nebo "long double" s plovoucí desetinnou čárkou typu s větší rozsah a přesnost než `double` zadejte a implicitně vykonává všechny operace s plovoucí desetinnou čárkou pomocí tohoto typu vyšší přesnost.</span><span class="sxs-lookup"><span data-stu-id="d43e3-224">For example, some hardware architectures support an "extended" or "long double" floating-point type with greater range and precision than the `double` type, and implicitly perform all floating-point operations using this higher precision type.</span></span> <span data-ttu-id="d43e3-225">Pouze na nadměrné náklady na výkon takový hardwarovou architekturou provádět k provádění operací s plovoucí desetinnou čárkou s přesností na menší a nikoli vyžadovat implementaci ztrácí výkonu a přesnosti, C# umožňuje vyšší přesnost typu bude používá pro všechny operace s plovoucí desetinnou čárkou.</span><span class="sxs-lookup"><span data-stu-id="d43e3-225">Only at excessive cost in performance can such hardware architectures be made to perform floating-point operations with less precision, and rather than require an implementation to forfeit both performance and precision, C# allows a higher precision type to be used for all floating-point operations.</span></span> <span data-ttu-id="d43e3-226">Než jasné přesnější výsledky, to má jen zřídka měřitelné důsledky.</span><span class="sxs-lookup"><span data-stu-id="d43e3-226">Other than delivering more precise results, this rarely has any measurable effects.</span></span> <span data-ttu-id="d43e3-227">Nicméně ve výrazech formuláře `x * y / z`, kde násobení vytváří výsledek, který je mimo `double` rozsah, ale následné dělení přináší dočasné výsledek zpět do `double` v rozsahu, skutečnost, že je výraz vyhodnoceny v rozsahu vyšší formátu může způsobit konečných výsledků bude vytvořen místo nekonečno.</span><span class="sxs-lookup"><span data-stu-id="d43e3-227">However, in expressions of the form `x * y / z`, where the multiplication produces a result that is outside the `double` range, but the subsequent division brings the temporary result back into the `double` range, the fact that the expression is evaluated in a higher range format may cause a finite result to be produced instead of an infinity.</span></span>

### <a name="the-decimal-type"></a><span data-ttu-id="d43e3-228">Typ decimal</span><span class="sxs-lookup"><span data-stu-id="d43e3-228">The decimal type</span></span>

<span data-ttu-id="d43e3-229">`decimal` Typ je typ 128bitových dat. vhodný pro výpočty finančních a přepočty měn.</span><span class="sxs-lookup"><span data-stu-id="d43e3-229">The `decimal` type is a 128-bit data type suitable for financial and monetary calculations.</span></span> <span data-ttu-id="d43e3-230">`decimal` Typ může představovat hodnotu `1.0 * 10^-28` na přibližně `7.9 * 10^28` s 28 až 29 platnými číslicemi.</span><span class="sxs-lookup"><span data-stu-id="d43e3-230">The `decimal` type can represent values ranging from `1.0 * 10^-28` to approximately `7.9 * 10^28` with 28-29 significant digits.</span></span>

<span data-ttu-id="d43e3-231">Omezená sada hodnot typu `decimal` jsou ve tvaru `(-1)^s * c * 10^-e`, kde znaménko `s` je 0 nebo 1, koeficient `c` je dán `0 <= *c* < 2^96`a rozšířit možnosti škálování `e` je tak, aby `0 <= e <= 28`. `decimal` Typ nepodporuje podepsaný nuly, nekonečno nebo na NaN.</span><span class="sxs-lookup"><span data-stu-id="d43e3-231">The finite set of values of type `decimal` are of the form `(-1)^s * c * 10^-e`, where the sign `s` is 0 or 1, the coefficient `c` is given by `0 <= *c* < 2^96`, and the scale `e` is such that `0 <= e <= 28`.The `decimal` type does not support signed zeros, infinities, or NaN's.</span></span> <span data-ttu-id="d43e3-232">A `decimal` je reprezentována jako celé číslo verze 96 měřítkem řídit sílu deset.</span><span class="sxs-lookup"><span data-stu-id="d43e3-232">A `decimal` is represented as a 96-bit integer scaled by a power of ten.</span></span> <span data-ttu-id="d43e3-233">Pro `decimal`s absolutní hodnota menší než `1.0m`, hodnota je přesné 28 desetinné čárky, ale žádné další.</span><span class="sxs-lookup"><span data-stu-id="d43e3-233">For `decimal`s with an absolute value less than `1.0m`, the value is exact to the 28th decimal place, but no further.</span></span> <span data-ttu-id="d43e3-234">Pro `decimal`s absolutní hodnota větší než nebo rovna hodnotě `1.0m`, hodnota je přesné 28 nebo 29 číslic.</span><span class="sxs-lookup"><span data-stu-id="d43e3-234">For `decimal`s with an absolute value greater than or equal to `1.0m`, the value is exact to 28 or 29 digits.</span></span> <span data-ttu-id="d43e3-235">Contrary k `float` a `double` datové typy, decimal desetinná čísla, jako je například 0,1 může být reprezentován přesně `decimal` reprezentace.</span><span class="sxs-lookup"><span data-stu-id="d43e3-235">Contrary to the `float` and `double` data types, decimal fractional numbers such as 0.1 can be represented exactly in the `decimal` representation.</span></span> <span data-ttu-id="d43e3-236">V `float` a `double` reprezentace, tato čísla jsou často nekonečné zlomky provádění těchto reprezentace náchylnější k zaokrouhlení chyby.</span><span class="sxs-lookup"><span data-stu-id="d43e3-236">In the `float` and `double` representations, such numbers are often infinite fractions, making those representations more prone to round-off errors.</span></span>

<span data-ttu-id="d43e3-237">Pokud je jeden z operandů binární operátor typu `decimal`, je druhý operand musí být integrálního typu nebo typu `decimal`.</span><span class="sxs-lookup"><span data-stu-id="d43e3-237">If one of the operands of a binary operator is of type `decimal`, then the other operand must be of an integral type or of type `decimal`.</span></span> <span data-ttu-id="d43e3-238">Je-li operand celočíselný typ je k dispozici, je převedena na `decimal` předtím, než se operace provádí.</span><span class="sxs-lookup"><span data-stu-id="d43e3-238">If an integral type operand is present, it is converted to `decimal` before the operation is performed.</span></span>

<span data-ttu-id="d43e3-239">Výsledkem operace na hodnotách typu `decimal` je, který by byl výsledkem výpočtu přesné výsledky (zachování škálování, jak jsou definovány pro každý operátor) a potom zaokrouhlení přizpůsobena reprezentace.</span><span class="sxs-lookup"><span data-stu-id="d43e3-239">The result of an operation on values of type `decimal` is that which would result from calculating an exact result (preserving scale, as defined for each operator) and then rounding to fit the representation.</span></span> <span data-ttu-id="d43e3-240">Výsledky jsou zaokrouhleny na nejbližší reprezentovatelnou hodnotu, a pokud výsledek je stejně blízko dvě reprezentovatelných hodnot, hodnotu, která má sudé číslo nejméně významných číslic pozici (to se označuje jako "banker je zaokrouhlení").</span><span class="sxs-lookup"><span data-stu-id="d43e3-240">Results are rounded to the nearest representable value, and, when a result is equally close to two representable values, to the value that has an even number in the least significant digit position (this is known as "banker's rounding").</span></span> <span data-ttu-id="d43e3-241">Žádný výsledek má vždy znaménko čísla 0 a měřítkem 0.</span><span class="sxs-lookup"><span data-stu-id="d43e3-241">A zero result always has a sign of 0 and a scale of 0.</span></span>

<span data-ttu-id="d43e3-242">V případě desítkové aritmetické operace vytvoří hodnotu menší než nebo rovna hodnotě `5 * 10^-29` výsledek operace v absolutní hodnota klesne na nulu.</span><span class="sxs-lookup"><span data-stu-id="d43e3-242">If a decimal arithmetic operation produces a value less than or equal to `5 * 10^-29` in absolute value, the result of the operation becomes zero.</span></span> <span data-ttu-id="d43e3-243">Pokud `decimal` vytváří výsledek, který je příliš velká pro aritmetické operace `decimal` formátu, `System.OverflowException` je vyvolána.</span><span class="sxs-lookup"><span data-stu-id="d43e3-243">If a `decimal` arithmetic operation produces a result that is too large for the `decimal` format, a `System.OverflowException` is thrown.</span></span>

<span data-ttu-id="d43e3-244">`decimal` Typ má zato větší přesnost ale menší rozsah než u typů s plovoucí desetinnou čárkou.</span><span class="sxs-lookup"><span data-stu-id="d43e3-244">The `decimal` type has greater precision but smaller range than the floating-point types.</span></span> <span data-ttu-id="d43e3-245">Proto převody z typů s plovoucí desetinnou čárkou na `decimal` vzniknout výjimky přetečení a převody z `decimal` na typy s plovoucí desetinnou čárkou může způsobit ztrátu přesnosti.</span><span class="sxs-lookup"><span data-stu-id="d43e3-245">Thus, conversions from the floating-point types to `decimal` might produce overflow exceptions, and conversions from `decimal` to the floating-point types might cause loss of precision.</span></span> <span data-ttu-id="d43e3-246">Z těchto důvodů, neexistuje žádný implicitní převod mezi typy s plovoucí desetinnou čárkou a `decimal`, a bez explicitního přetypování, není možné kombinovat s plovoucí desetinnou čárkou a `decimal` operandy ve stejném výrazu.</span><span class="sxs-lookup"><span data-stu-id="d43e3-246">For these reasons, no implicit conversions exist between the floating-point types and `decimal`, and without explicit casts, it is not possible to mix floating-point and `decimal` operands in the same expression.</span></span>

### <a name="the-bool-type"></a><span data-ttu-id="d43e3-247">Typ bool</span><span class="sxs-lookup"><span data-stu-id="d43e3-247">The bool type</span></span>

<span data-ttu-id="d43e3-248">`bool` Typ představuje logickou logické množství.</span><span class="sxs-lookup"><span data-stu-id="d43e3-248">The `bool` type represents boolean logical quantities.</span></span> <span data-ttu-id="d43e3-249">Možné hodnoty typu `bool` jsou `true` a `false`.</span><span class="sxs-lookup"><span data-stu-id="d43e3-249">The possible values of type `bool` are `true` and `false`.</span></span>

<span data-ttu-id="d43e3-250">Neexistuje žádný standardní převod mezi `bool` a dalších typů.</span><span class="sxs-lookup"><span data-stu-id="d43e3-250">No standard conversions exist between `bool` and other types.</span></span> <span data-ttu-id="d43e3-251">Konkrétně se `bool` typ je samostatný a oddělený od celočíselných typů a `bool` hodnotu nelze použít namísto celé číslo a naopak.</span><span class="sxs-lookup"><span data-stu-id="d43e3-251">In particular, the `bool` type is distinct and separate from the integral types, and a `bool` value cannot be used in place of an integral value, and vice versa.</span></span>

<span data-ttu-id="d43e3-252">V jazycích C a C++ nulová hodnota integrálního typu nebo s plovoucí desetinnou čárkou nebo ukazatel s hodnotou null lze převést na logickou hodnotu `false`, a nenulové hodnoty s plovoucí desetinnou čárkou nebo celočíselné nebo jiných nulového ukazatele lze převést na logickou hodnotu `true`.</span><span class="sxs-lookup"><span data-stu-id="d43e3-252">In the C and C++ languages, a zero integral or floating-point value, or a null pointer can be converted to the boolean value `false`, and a non-zero integral or floating-point value, or a non-null pointer can be converted to the boolean value `true`.</span></span> <span data-ttu-id="d43e3-253">V jazyce C#, můžete tyto převody provést explicitní porovnání hodnotu s plovoucí desetinnou čárkou nebo celočíselné hodnotě nula, nebo explicitní odkaz na objekt k porovnání `null`.</span><span class="sxs-lookup"><span data-stu-id="d43e3-253">In C#, such conversions are accomplished by explicitly comparing an integral or floating-point value to zero, or by explicitly comparing an object reference to `null`.</span></span>

### <a name="enumeration-types"></a><span data-ttu-id="d43e3-254">Výčtové typy</span><span class="sxs-lookup"><span data-stu-id="d43e3-254">Enumeration types</span></span>

<span data-ttu-id="d43e3-255">Typ výčtu je odlišný typ se pojmenovaných konstant.</span><span class="sxs-lookup"><span data-stu-id="d43e3-255">An enumeration type is a distinct type with named constants.</span></span> <span data-ttu-id="d43e3-256">Každý typ výčtu se základní typ, který musí být `byte`, `sbyte`, `short`, `ushort`, `int`, `uint`, `long` nebo `ulong`.</span><span class="sxs-lookup"><span data-stu-id="d43e3-256">Every enumeration type has an underlying type, which must be `byte`, `sbyte`, `short`, `ushort`, `int`, `uint`, `long` or `ulong`.</span></span> <span data-ttu-id="d43e3-257">Sadu hodnot typu výčtu je stejný jako sadu hodnot ze základního typu.</span><span class="sxs-lookup"><span data-stu-id="d43e3-257">The set of values of the enumeration type is the same as the set of values of the underlying type.</span></span> <span data-ttu-id="d43e3-258">Hodnoty typu výčtu nejsou omezené na hodnoty pojmenovaných konstant.</span><span class="sxs-lookup"><span data-stu-id="d43e3-258">Values of the enumeration type are not restricted to the values of the named constants.</span></span> <span data-ttu-id="d43e3-259">Výčtové typy jsou definované pomocí deklarace výčtů ([deklarace výčtu](enums.md#enum-declarations)).</span><span class="sxs-lookup"><span data-stu-id="d43e3-259">Enumeration types are defined through enumeration declarations ([Enum declarations](enums.md#enum-declarations)).</span></span>

### <a name="nullable-types"></a><span data-ttu-id="d43e3-260">Typy s povolenou hodnotou Null</span><span class="sxs-lookup"><span data-stu-id="d43e3-260">Nullable types</span></span>

<span data-ttu-id="d43e3-261">Typ připouštějící hodnotu Null, může představovat všechny hodnoty jeho ***nadřízený typ*** plus další hodnotu null.</span><span class="sxs-lookup"><span data-stu-id="d43e3-261">A nullable type can represent all values of its ***underlying type*** plus an additional null value.</span></span> <span data-ttu-id="d43e3-262">Typ s možnou hodnotou Null se zapíše `T?`, kde `T` je základní typ.</span><span class="sxs-lookup"><span data-stu-id="d43e3-262">A nullable type is written `T?`, where `T` is the underlying type.</span></span> <span data-ttu-id="d43e3-263">Tato syntaxe je zkratka pro `System.Nullable<T>`, a dvě různými formami zaměnitelné.</span><span class="sxs-lookup"><span data-stu-id="d43e3-263">This syntax is shorthand for `System.Nullable<T>`, and the two forms can be used interchangeably.</span></span>

<span data-ttu-id="d43e3-264">A ***hodnotu Null typu*** a naopak je libovolný typ hodnoty jiné než `System.Nullable<T>` a jeho zkrácený tvar vlastností `T?` (pro všechny `T`), a navíc všechny parametr typu, který je omezen na hodnotu Null typu (to znamená všechny parametr typu s `struct` omezení).</span><span class="sxs-lookup"><span data-stu-id="d43e3-264">A ***non-nullable value type*** conversely is any value type other than `System.Nullable<T>` and its shorthand `T?` (for any `T`), plus any type parameter that is constrained to be a non-nullable value type (that is, any type parameter with a `struct` constraint).</span></span> <span data-ttu-id="d43e3-265">`System.Nullable<T>` Typ Určuje typ omezení hodnoty pro `T` ([omezení parametru typu](classes.md#type-parameter-constraints)), což znamená, že základní typ s možnou hodnotou Null typu může být libovolný typ hodnotu Null.</span><span class="sxs-lookup"><span data-stu-id="d43e3-265">The `System.Nullable<T>` type specifies the value type constraint for `T` ([Type parameter constraints](classes.md#type-parameter-constraints)), which means that the underlying type of a nullable type can be any non-nullable value type.</span></span> <span data-ttu-id="d43e3-266">Základní typ s možnou hodnotou Null typu nemůže být typ připouštějící hodnotu null nebo typ odkazu.</span><span class="sxs-lookup"><span data-stu-id="d43e3-266">The underlying type of a nullable type cannot be a nullable type or a reference type.</span></span> <span data-ttu-id="d43e3-267">Například `int??` a `string?` jsou neplatné typy.</span><span class="sxs-lookup"><span data-stu-id="d43e3-267">For example, `int??` and `string?` are invalid types.</span></span>

<span data-ttu-id="d43e3-268">Instanci typu s možnou hodnotou Null `T?` mají dvě veřejné vlastnosti jen pro čtení:</span><span class="sxs-lookup"><span data-stu-id="d43e3-268">An instance of a nullable type `T?` has two public read-only properties:</span></span>

*  <span data-ttu-id="d43e3-269">A `HasValue` vlastnost typu `bool`</span><span class="sxs-lookup"><span data-stu-id="d43e3-269">A `HasValue` property of type `bool`</span></span>
*  <span data-ttu-id="d43e3-270">A `Value` vlastnost typu `T`</span><span class="sxs-lookup"><span data-stu-id="d43e3-270">A `Value` property of type `T`</span></span>

<span data-ttu-id="d43e3-271">Instance, pro kterou `HasValue` je PRAVDA se říká, že chcete mít hodnotu null.</span><span class="sxs-lookup"><span data-stu-id="d43e3-271">An instance for which `HasValue` is true is said to be non-null.</span></span> <span data-ttu-id="d43e3-272">Nenulové instance obsahuje známou hodnotou a `Value` vrátí tuto hodnotu.</span><span class="sxs-lookup"><span data-stu-id="d43e3-272">A non-null instance contains a known value and `Value` returns that value.</span></span>

<span data-ttu-id="d43e3-273">Instance, pro kterou `HasValue` je false se říká, že chcete mít hodnotu null.</span><span class="sxs-lookup"><span data-stu-id="d43e3-273">An instance for which `HasValue` is false is said to be null.</span></span> <span data-ttu-id="d43e3-274">Nedefinovaná hodnota je instanci null.</span><span class="sxs-lookup"><span data-stu-id="d43e3-274">A null instance has an undefined value.</span></span> <span data-ttu-id="d43e3-275">Při čtení `Value` instance null způsobí, že `System.InvalidOperationException` vyvolání.</span><span class="sxs-lookup"><span data-stu-id="d43e3-275">Attempting to read the `Value` of a null instance causes a `System.InvalidOperationException` to be thrown.</span></span> <span data-ttu-id="d43e3-276">Přístup k procesu `Value` vlastnost instance s možnou hodnotou Null se označuje jako ***rozbalení***.</span><span class="sxs-lookup"><span data-stu-id="d43e3-276">The process of accessing the `Value` property of a nullable instance is referred to as ***unwrapping***.</span></span>

<span data-ttu-id="d43e3-277">Kromě výchozího konstruktoru, každý typ s možnou hodnotou Null `T?` má veřejný konstruktor, který přijímá jeden argument typu `T`.</span><span class="sxs-lookup"><span data-stu-id="d43e3-277">In addition to the default constructor, every nullable type `T?` has a public constructor that takes a single argument of type `T`.</span></span> <span data-ttu-id="d43e3-278">Zadána hodnota `x` typu `T`, vyvolání konstruktoru formuláře</span><span class="sxs-lookup"><span data-stu-id="d43e3-278">Given a value `x` of type `T`, a constructor invocation of the form</span></span>

```csharp
new T?(x)
```
<span data-ttu-id="d43e3-279">vytvoří instanci nenulovou `T?` pro kterou `Value` vlastnost `x`.</span><span class="sxs-lookup"><span data-stu-id="d43e3-279">creates a non-null instance of `T?` for which the `Value` property is `x`.</span></span> <span data-ttu-id="d43e3-280">Proces vytváření nenulovou instanci typu s možnou hodnotou Null pro dané hodnoty se označuje jako ***obtékání***.</span><span class="sxs-lookup"><span data-stu-id="d43e3-280">The process of creating a non-null instance of a nullable type for a given value is referred to as ***wrapping***.</span></span>

<span data-ttu-id="d43e3-281">Implicitní převody jsou k dispozici na `null` literál `T?` ([převody literál s hodnotou Null](conversions.md#null-literal-conversions)) a z `T` k `T?` ([implicitních převodů s možnou hodnotou Null](conversions.md#implicit-nullable-conversions)).</span><span class="sxs-lookup"><span data-stu-id="d43e3-281">Implicit conversions are available from the `null` literal to `T?` ([Null literal conversions](conversions.md#null-literal-conversions)) and from `T` to `T?` ([Implicit nullable conversions](conversions.md#implicit-nullable-conversions)).</span></span>

## <a name="reference-types"></a><span data-ttu-id="d43e3-282">Odkazové typy</span><span class="sxs-lookup"><span data-stu-id="d43e3-282">Reference types</span></span>

<span data-ttu-id="d43e3-283">Odkazový typ je typ třídy, typem rozhraní, typem pole nebo typ delegáta.</span><span class="sxs-lookup"><span data-stu-id="d43e3-283">A reference type is a class type, an interface type, an array type, or a delegate type.</span></span>

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

<span data-ttu-id="d43e3-284">Hodnota typu odkazu je odkazem na ***instance*** typu, ten se označuje jako ***objekt***.</span><span class="sxs-lookup"><span data-stu-id="d43e3-284">A reference type value is a reference to an ***instance*** of the type, the latter known as an ***object***.</span></span> <span data-ttu-id="d43e3-285">Zvláštní hodnota `null` je kompatibilní se všemi typy odkazu a ukazuje na nepřítomnost instance.</span><span class="sxs-lookup"><span data-stu-id="d43e3-285">The special value `null` is compatible with all reference types and indicates the absence of an instance.</span></span>

### <a name="class-types"></a><span data-ttu-id="d43e3-286">Typy tříd</span><span class="sxs-lookup"><span data-stu-id="d43e3-286">Class types</span></span>

<span data-ttu-id="d43e3-287">Typ třídy definuje datová struktura, která obsahuje datové členy (konstant a polí), funkce členy (metody, vlastnosti, události, indexery, operátory, konstruktory instancí, destruktory a statické konstruktory) a vnořené typy.</span><span class="sxs-lookup"><span data-stu-id="d43e3-287">A class type defines a data structure that contains data members (constants and fields), function members (methods, properties, events, indexers, operators, instance constructors, destructors and static constructors), and nested types.</span></span> <span data-ttu-id="d43e3-288">Typy tříd podporují dědičnost, mechanismus, kterým odvozené třídy můžete rozšířit a specialize základní třídy.</span><span class="sxs-lookup"><span data-stu-id="d43e3-288">Class types support inheritance, a mechanism whereby derived classes can extend and specialize base classes.</span></span> <span data-ttu-id="d43e3-289">Instance typů tříd jsou vytvořené pomocí *object_creation_expression*s ([výrazy vytvoření objektu](expressions.md#object-creation-expressions)).</span><span class="sxs-lookup"><span data-stu-id="d43e3-289">Instances of class types are created using *object_creation_expression*s ([Object creation expressions](expressions.md#object-creation-expressions)).</span></span>

<span data-ttu-id="d43e3-290">Typy tříd jsou popsány v [třídy](classes.md).</span><span class="sxs-lookup"><span data-stu-id="d43e3-290">Class types are described in [Classes](classes.md).</span></span>

<span data-ttu-id="d43e3-291">Některé typy předdefinované třídy mají zvláštní význam v jazyce C#, jak je popsáno v následující tabulce.</span><span class="sxs-lookup"><span data-stu-id="d43e3-291">Certain predefined class types have special meaning in the C# language, as described in the table below.</span></span>


| <span data-ttu-id="d43e3-292">__Typ třídy.__</span><span class="sxs-lookup"><span data-stu-id="d43e3-292">__Class type__</span></span>     | <span data-ttu-id="d43e3-293">__Popis__</span><span class="sxs-lookup"><span data-stu-id="d43e3-293">__Description__</span></span>                                         |
|--------------------|---------------------------------------------------------|
| `System.Object`    | <span data-ttu-id="d43e3-294">Ultimate základní třídy pro všechny ostatní typy.</span><span class="sxs-lookup"><span data-stu-id="d43e3-294">The ultimate base class of all other types.</span></span> <span data-ttu-id="d43e3-295">Zobrazit [typ objektu](types.md#the-object-type).</span><span class="sxs-lookup"><span data-stu-id="d43e3-295">See [The object type](types.md#the-object-type).</span></span> | 
| `System.String`    | <span data-ttu-id="d43e3-296">Typ řetězec jazyka C#.</span><span class="sxs-lookup"><span data-stu-id="d43e3-296">The string type of the C# language.</span></span> <span data-ttu-id="d43e3-297">Zobrazit [typ řetězce](types.md#the-string-type).</span><span class="sxs-lookup"><span data-stu-id="d43e3-297">See [The string type](types.md#the-string-type).</span></span>         |
| `System.ValueType` | <span data-ttu-id="d43e3-298">Základní třída všechny hodnotové typy.</span><span class="sxs-lookup"><span data-stu-id="d43e3-298">The base class of all value types.</span></span> <span data-ttu-id="d43e3-299">Zobrazit [typ The System.ValueType](types.md#the-systemvaluetype-type).</span><span class="sxs-lookup"><span data-stu-id="d43e3-299">See [The System.ValueType type](types.md#the-systemvaluetype-type).</span></span>          |
| `System.Enum`      | <span data-ttu-id="d43e3-300">Základní třída všech typů výčtu.</span><span class="sxs-lookup"><span data-stu-id="d43e3-300">The base class of all enum types.</span></span> <span data-ttu-id="d43e3-301">Zobrazit [výčty](enums.md).</span><span class="sxs-lookup"><span data-stu-id="d43e3-301">See [Enums](enums.md).</span></span>              |
| `System.Array`     | <span data-ttu-id="d43e3-302">Základní třída všechny typy polí.</span><span class="sxs-lookup"><span data-stu-id="d43e3-302">The base class of all array types.</span></span> <span data-ttu-id="d43e3-303">Zobrazit [pole](arrays.md).</span><span class="sxs-lookup"><span data-stu-id="d43e3-303">See [Arrays](arrays.md).</span></span>             |
| `System.Delegate`  | <span data-ttu-id="d43e3-304">Základní třídy pro všemi typy delegátů.</span><span class="sxs-lookup"><span data-stu-id="d43e3-304">The base class of all delegate types.</span></span> <span data-ttu-id="d43e3-305">Zobrazit [delegáti](delegates.md).</span><span class="sxs-lookup"><span data-stu-id="d43e3-305">See [Delegates](delegates.md).</span></span>          |
| `System.Exception` | <span data-ttu-id="d43e3-306">Základní třída všech typů výjimek.</span><span class="sxs-lookup"><span data-stu-id="d43e3-306">The base class of all exception types.</span></span> <span data-ttu-id="d43e3-307">Zobrazit [výjimky](exceptions.md).</span><span class="sxs-lookup"><span data-stu-id="d43e3-307">See [Exceptions](exceptions.md).</span></span>         |

### <a name="the-object-type"></a><span data-ttu-id="d43e3-308">Typ objektu</span><span class="sxs-lookup"><span data-stu-id="d43e3-308">The object type</span></span>

<span data-ttu-id="d43e3-309">`object` Typu třídy je ultimate základní třídy pro všechny ostatní typy.</span><span class="sxs-lookup"><span data-stu-id="d43e3-309">The `object` class type is the ultimate base class of all other types.</span></span> <span data-ttu-id="d43e3-310">Všechny typy v jazyce C# přímo nebo nepřímo odvozuje od `object` typ třídy.</span><span class="sxs-lookup"><span data-stu-id="d43e3-310">Every type in C# directly or indirectly derives from the `object` class type.</span></span>

<span data-ttu-id="d43e3-311">Klíčové slovo `object` je pouze aliasem pro třídu předdefinované `System.Object`.</span><span class="sxs-lookup"><span data-stu-id="d43e3-311">The keyword `object` is simply an alias for the predefined class `System.Object`.</span></span>

### <a name="the-dynamic-type"></a><span data-ttu-id="d43e3-312">Dynamický typ.</span><span class="sxs-lookup"><span data-stu-id="d43e3-312">The dynamic type</span></span>

<span data-ttu-id="d43e3-313">`dynamic` Typ, jako je třeba `object`, můžete odkazovat na libovolný objekt.</span><span class="sxs-lookup"><span data-stu-id="d43e3-313">The `dynamic` type, like `object`, can reference any object.</span></span> <span data-ttu-id="d43e3-314">Kdy jsou operátory pro výrazy typu používat `dynamic`, jejich rozlišení je odloženo, dokud spuštění programu.</span><span class="sxs-lookup"><span data-stu-id="d43e3-314">When operators are applied to expressions of type `dynamic`, their resolution is deferred until the program is run.</span></span> <span data-ttu-id="d43e3-315">Proto pokud operátor nelze použít právně pro odkazovaného objektu, je předána žádná chyba během kompilace.</span><span class="sxs-lookup"><span data-stu-id="d43e3-315">Thus, if the operator cannot legally be applied to the referenced object, no error is given during compilation.</span></span> <span data-ttu-id="d43e3-316">Místo toho bude vyvolána výjimka při překladu operátor, který selže v době běhu.</span><span class="sxs-lookup"><span data-stu-id="d43e3-316">Instead an exception will be thrown when resolution of the operator fails at run-time.</span></span>

<span data-ttu-id="d43e3-317">Jeho účelem je umožnit dynamické vazby, která je popsána v části [dynamické vazby](expressions.md#dynamic-binding).</span><span class="sxs-lookup"><span data-stu-id="d43e3-317">Its purpose is to allow dynamic binding, which is described in detail in [Dynamic binding](expressions.md#dynamic-binding).</span></span>

<span data-ttu-id="d43e3-318">`dynamic` je považován za shodný s `object` s výjimkou v těchto ohledech:</span><span class="sxs-lookup"><span data-stu-id="d43e3-318">`dynamic` is considered identical to `object` except in the following respects:</span></span>

*  <span data-ttu-id="d43e3-319">Operace na výrazy typu `dynamic` může být vázaný dynamicky ([dynamické vazby](expressions.md#dynamic-binding)).</span><span class="sxs-lookup"><span data-stu-id="d43e3-319">Operations on expressions of type `dynamic` can be dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)).</span></span>
*  <span data-ttu-id="d43e3-320">Odvození typu ([odvození typu](expressions.md#type-inference)) preferovali `dynamic` přes `object` Pokud jsou obě kandidáty.</span><span class="sxs-lookup"><span data-stu-id="d43e3-320">Type inference ([Type inference](expressions.md#type-inference)) will prefer `dynamic` over `object` if both are candidates.</span></span>

<span data-ttu-id="d43e3-321">Z důvodu této ekvivalence obsahuje následující:</span><span class="sxs-lookup"><span data-stu-id="d43e3-321">Because of this equivalence, the following holds:</span></span>

*  <span data-ttu-id="d43e3-322">Zde je implicitní identity převod mezi `object` a `dynamic`a mezi sestavené typy, které jsou stejné při nahrazování `dynamic` s `object`</span><span class="sxs-lookup"><span data-stu-id="d43e3-322">There is an implicit identity conversion between `object` and `dynamic`, and between constructed types that are the same when replacing `dynamic` with `object`</span></span>
*  <span data-ttu-id="d43e3-323">Implicitní a explicitní převody do a z `object` platí také do a z `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="d43e3-323">Implicit and explicit conversions to and from `object` also apply to and from `dynamic`.</span></span>
*  <span data-ttu-id="d43e3-324">Podpisy metod, které jsou stejné při nahrazování `dynamic` s `object` jsou považovány za stejného podpisu</span><span class="sxs-lookup"><span data-stu-id="d43e3-324">Method signatures that are the same when replacing `dynamic` with `object` are considered the same signature</span></span>
*  <span data-ttu-id="d43e3-325">Typ `dynamic` je nerozeznatelná od `object` v době běhu.</span><span class="sxs-lookup"><span data-stu-id="d43e3-325">The type `dynamic` is indistinguishable from `object` at run-time.</span></span>
*  <span data-ttu-id="d43e3-326">Výraz typu `dynamic` se označuje jako ***dynamického výrazu***.</span><span class="sxs-lookup"><span data-stu-id="d43e3-326">An expression of the type `dynamic` is referred to as a ***dynamic expression***.</span></span>

### <a name="the-string-type"></a><span data-ttu-id="d43e3-327">Typ string</span><span class="sxs-lookup"><span data-stu-id="d43e3-327">The string type</span></span>

<span data-ttu-id="d43e3-328">`string` Typ je typ třídy sealed, který dědí přímo z `object`.</span><span class="sxs-lookup"><span data-stu-id="d43e3-328">The `string` type is a sealed class type that inherits directly from `object`.</span></span> <span data-ttu-id="d43e3-329">Instance `string` třídy představují řetězce znaků Unicode.</span><span class="sxs-lookup"><span data-stu-id="d43e3-329">Instances of the `string` class represent Unicode character strings.</span></span>

<span data-ttu-id="d43e3-330">Hodnoty `string` typu lze zapsat jako řetězec literálů ([řetězcových literálů](lexical-structure.md#string-literals)).</span><span class="sxs-lookup"><span data-stu-id="d43e3-330">Values of the `string` type can be written as string literals ([String literals](lexical-structure.md#string-literals)).</span></span>

<span data-ttu-id="d43e3-331">Klíčové slovo `string` je pouze aliasem pro třídu předdefinované `System.String`.</span><span class="sxs-lookup"><span data-stu-id="d43e3-331">The keyword `string` is simply an alias for the predefined class `System.String`.</span></span>

### <a name="interface-types"></a><span data-ttu-id="d43e3-332">Typy rozhraní</span><span class="sxs-lookup"><span data-stu-id="d43e3-332">Interface types</span></span>

<span data-ttu-id="d43e3-333">Rozhraní definuje kontrakt.</span><span class="sxs-lookup"><span data-stu-id="d43e3-333">An interface defines a contract.</span></span> <span data-ttu-id="d43e3-334">Třída nebo struktura, která implementuje rozhraní musí dodržovat jeho kontrakt.</span><span class="sxs-lookup"><span data-stu-id="d43e3-334">A class or struct that implements an interface must adhere to its contract.</span></span> <span data-ttu-id="d43e3-335">Rozhraní může dědit z více základních rozhraní a třídy nebo struktury může implementovat více rozhraní.</span><span class="sxs-lookup"><span data-stu-id="d43e3-335">An interface may inherit from multiple base interfaces, and a class or struct may implement multiple interfaces.</span></span>

<span data-ttu-id="d43e3-336">Typy rozhraní jsou popsány v [rozhraní](interfaces.md).</span><span class="sxs-lookup"><span data-stu-id="d43e3-336">Interface types are described in [Interfaces](interfaces.md).</span></span>

### <a name="array-types"></a><span data-ttu-id="d43e3-337">Typy polí</span><span class="sxs-lookup"><span data-stu-id="d43e3-337">Array types</span></span>

<span data-ttu-id="d43e3-338">Pole je datová struktura, která obsahuje nula nebo více proměnných, které jsou přístupné prostřednictvím vypočítané indexy.</span><span class="sxs-lookup"><span data-stu-id="d43e3-338">An array is a data structure that contains zero or more variables which are accessed through computed indices.</span></span> <span data-ttu-id="d43e3-339">Proměnné, které jsou obsaženy v poli, také nazývají prvky pole, jsou všechny stejného typu a tento typ se nazývá typ prvku pole.</span><span class="sxs-lookup"><span data-stu-id="d43e3-339">The variables contained in an array, also called the elements of the array, are all of the same type, and this type is called the element type of the array.</span></span>

<span data-ttu-id="d43e3-340">Typy pole jsou popsány v [pole](arrays.md).</span><span class="sxs-lookup"><span data-stu-id="d43e3-340">Array types are described in [Arrays](arrays.md).</span></span>

### <a name="delegate-types"></a><span data-ttu-id="d43e3-341">Typy delegátů</span><span class="sxs-lookup"><span data-stu-id="d43e3-341">Delegate types</span></span>

<span data-ttu-id="d43e3-342">Delegát je datová struktura, která odkazuje na jeden nebo více metod.</span><span class="sxs-lookup"><span data-stu-id="d43e3-342">A delegate is a data structure that refers to one or more methods.</span></span> <span data-ttu-id="d43e3-343">Pro instanci metody, také odkazuje na jejich odpovídající instance objektů.</span><span class="sxs-lookup"><span data-stu-id="d43e3-343">For instance methods, it also refers to their corresponding object instances.</span></span>

<span data-ttu-id="d43e3-344">Nejbližší ekvivalent delegáta v jazyce C nebo C++ je ukazatel na funkci, ale že ukazatele na funkci, mohou odkazovat pouze na statické funkce, můžete odkazovat na statické a instanci metody delegáta.</span><span class="sxs-lookup"><span data-stu-id="d43e3-344">The closest equivalent of a delegate in C or C++ is a function pointer, but whereas a function pointer can only reference static functions, a delegate can reference both static and instance methods.</span></span> <span data-ttu-id="d43e3-345">V takovém případě delegáta ukládá pouze odkaz na metody vstupního bodu, ale také odkaz na instanci objektu, na kterém se má vyvolat metodu.</span><span class="sxs-lookup"><span data-stu-id="d43e3-345">In the latter case, the delegate stores not only a reference to the method's entry point, but also a reference to the object instance on which to invoke the method.</span></span>

<span data-ttu-id="d43e3-346">Typy delegátů jsou popsány v [delegáti](delegates.md).</span><span class="sxs-lookup"><span data-stu-id="d43e3-346">Delegate types are described in [Delegates](delegates.md).</span></span>

## <a name="boxing-and-unboxing"></a><span data-ttu-id="d43e3-347">Zabalení a rozbalení</span><span class="sxs-lookup"><span data-stu-id="d43e3-347">Boxing and unboxing</span></span>

<span data-ttu-id="d43e3-348">Pojem zabalení a rozbalení je systém typů jazyka C#.</span><span class="sxs-lookup"><span data-stu-id="d43e3-348">The concept of boxing and unboxing is central to C#'s type system.</span></span> <span data-ttu-id="d43e3-349">Představuje most mezi *value_type*s a *reference_type*s tím, že všechny výhody *value_type* má být převeden do a z typů `object`.</span><span class="sxs-lookup"><span data-stu-id="d43e3-349">It provides a bridge between *value_type*s and *reference_type*s by permitting any value of a *value_type* to be converted to and from type `object`.</span></span> <span data-ttu-id="d43e3-350">Zabalení a rozbalení umožňuje jednotný přehled o systému typů, ve které hodnotu libovolného typu lze nakonec zacházet jako objekt.</span><span class="sxs-lookup"><span data-stu-id="d43e3-350">Boxing and unboxing enables a unified view of the type system wherein a value of any type can ultimately be treated as an object.</span></span>

### <a name="boxing-conversions"></a><span data-ttu-id="d43e3-351">Zabalení převody</span><span class="sxs-lookup"><span data-stu-id="d43e3-351">Boxing conversions</span></span>

<span data-ttu-id="d43e3-352">Umožňuje převod na uzavřené určení *value_type* má implicitně převést na *reference_type*.</span><span class="sxs-lookup"><span data-stu-id="d43e3-352">A boxing conversion permits a *value_type* to be implicitly converted to a *reference_type*.</span></span> <span data-ttu-id="d43e3-353">Existují následující převody zabalení:</span><span class="sxs-lookup"><span data-stu-id="d43e3-353">The following boxing conversions exist:</span></span>

*  <span data-ttu-id="d43e3-354">Z libovolného *value_type* typu `object`.</span><span class="sxs-lookup"><span data-stu-id="d43e3-354">From any *value_type* to the type `object`.</span></span>
*  <span data-ttu-id="d43e3-355">Z libovolného *value_type* typu `System.ValueType`.</span><span class="sxs-lookup"><span data-stu-id="d43e3-355">From any *value_type* to the type `System.ValueType`.</span></span>
*  <span data-ttu-id="d43e3-356">Z libovolného *non_nullable_value_type* k libovolnému *interface_type* implementované *value_type*.</span><span class="sxs-lookup"><span data-stu-id="d43e3-356">From any *non_nullable_value_type* to any *interface_type* implemented by the *value_type*.</span></span>
*  <span data-ttu-id="d43e3-357">Z libovolného *nullable_type* k libovolnému *interface_type* implementovaných základní typ *nullable_type*.</span><span class="sxs-lookup"><span data-stu-id="d43e3-357">From any *nullable_type* to any *interface_type* implemented by the underlying type of the *nullable_type*.</span></span>
*  <span data-ttu-id="d43e3-358">Z libovolného *enum_type* typu `System.Enum`.</span><span class="sxs-lookup"><span data-stu-id="d43e3-358">From any *enum_type* to the type `System.Enum`.</span></span>
*  <span data-ttu-id="d43e3-359">Z libovolného *nullable_type* s jako základ *enum_type* typu `System.Enum`.</span><span class="sxs-lookup"><span data-stu-id="d43e3-359">From any *nullable_type* with an underlying *enum_type* to the type `System.Enum`.</span></span>
*  <span data-ttu-id="d43e3-360">Vezměte na vědomí, že implicitní převod z typu parametru jako převod na uzavřené určení bude proveden, pokud v době běhu skončilo převod z typu hodnoty na typ odkazu ([implicitních převodů zahrnující parametry typu](conversions.md#implicit-conversions-involving-type-parameters)).</span><span class="sxs-lookup"><span data-stu-id="d43e3-360">Note that an implicit conversion from a type parameter will be executed as a boxing conversion if at run-time it ends up converting from a value type to a reference type ([Implicit conversions involving type parameters](conversions.md#implicit-conversions-involving-type-parameters)).</span></span>

<span data-ttu-id="d43e3-361">Zabalení hodnotu *non_nullable_value_type* se skládá z přidělování instancí objektu a kopírování *non_nullable_value_type* hodnoty do této instance.</span><span class="sxs-lookup"><span data-stu-id="d43e3-361">Boxing a value of a *non_nullable_value_type* consists of allocating an object instance and copying the *non_nullable_value_type* value into that instance.</span></span>

<span data-ttu-id="d43e3-362">Zabalení hodnotu *nullable_type* vytvoří odkaz s hodnotou null, pokud je `null` hodnotu (`HasValue` je `false`), nebo výsledek rozbalení a jinak zabalení zdrojovou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="d43e3-362">Boxing a value of a *nullable_type* produces a null reference if it is the `null` value (`HasValue` is `false`), or the result of unwrapping and boxing the underlying value otherwise.</span></span>

<span data-ttu-id="d43e3-363">Samotný proces zabalení hodnotu *non_nullable_value_type* je nejlépe vysvětlit užívat existenci obecný ***třída zabalení***, který se chová jako kdyby byly deklarovány následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="d43e3-363">The actual process of boxing a value of a *non_nullable_value_type* is best explained by imagining the existence of a generic ***boxing class***, which behaves as if it were declared as follows:</span></span>

```csharp
sealed class Box<T>: System.ValueType
{
    T value;

    public Box(T t) {
        value = t;
    }
}
```

<span data-ttu-id="d43e3-364">Zabalení hodnoty `v` typu `T` nyní se skládá z provádění výrazu `new Box<T>(v)`a vrátí výsledný instance jako hodnotu typu `object`.</span><span class="sxs-lookup"><span data-stu-id="d43e3-364">Boxing of a value `v` of type `T` now consists of executing the expression `new Box<T>(v)`, and returning the resulting instance as a value of type `object`.</span></span> <span data-ttu-id="d43e3-365">Díky tomu se příkazy</span><span class="sxs-lookup"><span data-stu-id="d43e3-365">Thus, the statements</span></span>
```csharp
int i = 123;
object box = i;
```
<span data-ttu-id="d43e3-366">koncepčně odpovídají</span><span class="sxs-lookup"><span data-stu-id="d43e3-366">conceptually correspond to</span></span>
```csharp
int i = 123;
object box = new Box<int>(i);
```

<span data-ttu-id="d43e3-367">Třída zabalení jako `Box<T>` výše neexistuje ve skutečnosti a dynamického typu zabalené hodnoty není ve skutečnosti typem třídy.</span><span class="sxs-lookup"><span data-stu-id="d43e3-367">A boxing class like `Box<T>` above doesn't actually exist and the dynamic type of a boxed value isn't actually a class type.</span></span> <span data-ttu-id="d43e3-368">Místo toho zabalené hodnoty typu `T` má dynamického typu `T`a pomocí dynamického typu kontroly `is` operátor můžete jednoduše odkazovat na typ `T`.</span><span class="sxs-lookup"><span data-stu-id="d43e3-368">Instead, a boxed value of type `T` has the dynamic type `T`, and a dynamic type check using the `is` operator can simply reference type `T`.</span></span> <span data-ttu-id="d43e3-369">Například</span><span class="sxs-lookup"><span data-stu-id="d43e3-369">For example,</span></span>
```csharp
int i = 123;
object box = i;
if (box is int) {
    Console.Write("Box contains an int");
}
```
<span data-ttu-id="d43e3-370">výstup řetězec "`Box contains an int`" v konzole.</span><span class="sxs-lookup"><span data-stu-id="d43e3-370">will output the string "`Box contains an int`" on the console.</span></span>

<span data-ttu-id="d43e3-371">Převod na uzavřené určení znamená vytvoření kopie hodnoty se v poli.</span><span class="sxs-lookup"><span data-stu-id="d43e3-371">A boxing conversion implies making a copy of the value being boxed.</span></span> <span data-ttu-id="d43e3-372">Tím se liší od převod *reference_type* na typ `object`, kterými se hodnota dál odkazovat na stejnou instanci a jednoduše se považuje za méně odvozený typ `object`.</span><span class="sxs-lookup"><span data-stu-id="d43e3-372">This is different from a conversion of a *reference_type* to type `object`, in which the value continues to reference the same instance and simply is regarded as the less derived type `object`.</span></span> <span data-ttu-id="d43e3-373">Mějme například deklarace</span><span class="sxs-lookup"><span data-stu-id="d43e3-373">For example, given the declaration</span></span>
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
<span data-ttu-id="d43e3-374">Následující příkazy</span><span class="sxs-lookup"><span data-stu-id="d43e3-374">the following statements</span></span>
```csharp
Point p = new Point(10, 10);
object box = p;
p.x = 20;
Console.Write(((Point)box).x);
```
<span data-ttu-id="d43e3-375">bude výstupní hodnotu 10 v konzole, protože operace implicitního zabalení, ke které dochází v přiřazení `p` k `box` způsobí, že hodnota `p` ke zkopírování.</span><span class="sxs-lookup"><span data-stu-id="d43e3-375">will output the value 10 on the console because the implicit boxing operation that occurs in the assignment of `p` to `box` causes the value of `p` to be copied.</span></span> <span data-ttu-id="d43e3-376">Měl `Point` byla deklarována `class` hodnota 20 místo toho bude výstup, protože `p` a `box` by odkazovat na stejnou instanci.</span><span class="sxs-lookup"><span data-stu-id="d43e3-376">Had `Point` been declared a `class` instead, the value 20 would be output because `p` and `box` would reference the same instance.</span></span>

### <a name="unboxing-conversions"></a><span data-ttu-id="d43e3-377">Rozbalení převody</span><span class="sxs-lookup"><span data-stu-id="d43e3-377">Unboxing conversions</span></span>

<span data-ttu-id="d43e3-378">Povoluje unboxingového převodu *reference_type* má být explicitně převeden na *value_type*.</span><span class="sxs-lookup"><span data-stu-id="d43e3-378">An unboxing conversion permits a *reference_type* to be explicitly converted to a *value_type*.</span></span> <span data-ttu-id="d43e3-379">Existují následující rozbalení převody:</span><span class="sxs-lookup"><span data-stu-id="d43e3-379">The following unboxing conversions exist:</span></span>

*  <span data-ttu-id="d43e3-380">Z typu `object` k libovolnému *value_type*.</span><span class="sxs-lookup"><span data-stu-id="d43e3-380">From the type `object` to any *value_type*.</span></span>
*  <span data-ttu-id="d43e3-381">Z typu `System.ValueType` k libovolnému *value_type*.</span><span class="sxs-lookup"><span data-stu-id="d43e3-381">From the type `System.ValueType` to any *value_type*.</span></span>
*  <span data-ttu-id="d43e3-382">Z libovolného *interface_type* k libovolnému *non_nullable_value_type* , který implementuje *interface_type*.</span><span class="sxs-lookup"><span data-stu-id="d43e3-382">From any *interface_type* to any *non_nullable_value_type* that implements the *interface_type*.</span></span>
*  <span data-ttu-id="d43e3-383">Z libovolného *interface_type* k libovolnému *nullable_type* jehož základní typ implementuje *interface_type*.</span><span class="sxs-lookup"><span data-stu-id="d43e3-383">From any *interface_type* to any *nullable_type* whose underlying type implements the *interface_type*.</span></span>
*  <span data-ttu-id="d43e3-384">Z typu `System.Enum` k libovolnému *enum_type*.</span><span class="sxs-lookup"><span data-stu-id="d43e3-384">From the type `System.Enum` to any *enum_type*.</span></span>
*  <span data-ttu-id="d43e3-385">Z typu `System.Enum` k libovolnému *nullable_type* s jako základ *enum_type*.</span><span class="sxs-lookup"><span data-stu-id="d43e3-385">From the type `System.Enum` to any *nullable_type* with an underlying *enum_type*.</span></span>
*  <span data-ttu-id="d43e3-386">Vezměte na vědomí, že explicitní převod typu parametru jako unboxingového převodu. bude proveden, pokud v době běhu skončilo převod z typu odkazu na typ hodnoty ([explicitních převodů dynamické](conversions.md#explicit-dynamic-conversions)).</span><span class="sxs-lookup"><span data-stu-id="d43e3-386">Note that an explicit conversion to a type parameter will be executed as an unboxing conversion if at run-time it ends up converting from a reference type to a value type ([Explicit dynamic conversions](conversions.md#explicit-dynamic-conversions)).</span></span>

<span data-ttu-id="d43e3-387">Rozbalení operace *non_nullable_value_type* se skládá z nejdřív zkontrolovali, že se instance objektu je zabalený hodnotu daný *non_nullable_value_type*a zkopírujte hodnotu z celkového počtu instance.</span><span class="sxs-lookup"><span data-stu-id="d43e3-387">An unboxing operation to a *non_nullable_value_type* consists of first checking that the object instance is a boxed value of the given *non_nullable_value_type*, and then copying the value out of the instance.</span></span>

<span data-ttu-id="d43e3-388">K rozbalení *nullable_type* vytvoří hodnotu null *nullable_type* zdrojového operandu, je-li `null`, nebo zabalená výsledek rozbalení instance objektu určeného k základního typu *nullable_type* jinak.</span><span class="sxs-lookup"><span data-stu-id="d43e3-388">Unboxing to a *nullable_type* produces the null value of the *nullable_type* if the source operand is `null`, or the wrapped result of unboxing the object instance to the underlying type of the *nullable_type* otherwise.</span></span>

<span data-ttu-id="d43e3-389">Odkaz na třídu imaginární zabalení je popsáno v předchozí části, unboxingového převodu objektu `box` k *value_type* `T` se skládá z provádění výrazu `((Box<T>)box).value`.</span><span class="sxs-lookup"><span data-stu-id="d43e3-389">Referring to the imaginary boxing class described in the previous section, an unboxing conversion of an object `box` to a *value_type* `T` consists of executing the expression `((Box<T>)box).value`.</span></span> <span data-ttu-id="d43e3-390">Díky tomu se příkazy</span><span class="sxs-lookup"><span data-stu-id="d43e3-390">Thus, the statements</span></span>
```csharp
object box = 123;
int i = (int)box;
```
<span data-ttu-id="d43e3-391">koncepčně odpovídají</span><span class="sxs-lookup"><span data-stu-id="d43e3-391">conceptually correspond to</span></span>
```csharp
object box = new Box<int>(123);
int i = ((Box<int>)box).value;
```

<span data-ttu-id="d43e3-392">Pro rozbalení převod na daný *non_nullable_value_type* úspěšné v době běhu, hodnota zdrojový operand musí být odkaz na o zabalenou hodnotu, která *non_nullable_value_type*.</span><span class="sxs-lookup"><span data-stu-id="d43e3-392">For an unboxing conversion to a given *non_nullable_value_type* to succeed at run-time, the value of the source operand must be a reference to a boxed value of that *non_nullable_value_type*.</span></span> <span data-ttu-id="d43e3-393">Pokud je zdrojový operand `null`, `System.NullReferenceException` je vyvolána výjimka.</span><span class="sxs-lookup"><span data-stu-id="d43e3-393">If the source operand is `null`, a `System.NullReferenceException` is thrown.</span></span> <span data-ttu-id="d43e3-394">Pokud zdrojový operand je odkaz na objekt není kompatibilní `System.InvalidCastException` je vyvolána výjimka.</span><span class="sxs-lookup"><span data-stu-id="d43e3-394">If the source operand is a reference to an incompatible object, a `System.InvalidCastException` is thrown.</span></span>

<span data-ttu-id="d43e3-395">Pro rozbalení převod na daný *nullable_type* úspěšné v době běhu, hodnota zdrojový operand musí být buď `null` nebo odkaz na základní o zabalenou hodnotu *non_nullable_value_type* z *nullable_type*.</span><span class="sxs-lookup"><span data-stu-id="d43e3-395">For an unboxing conversion to a given *nullable_type* to succeed at run-time, the value of the source operand must be either `null` or a reference to a boxed value of the underlying *non_nullable_value_type* of the *nullable_type*.</span></span> <span data-ttu-id="d43e3-396">Pokud zdrojový operand je odkaz na objekt není kompatibilní `System.InvalidCastException` je vyvolána výjimka.</span><span class="sxs-lookup"><span data-stu-id="d43e3-396">If the source operand is a reference to an incompatible object, a `System.InvalidCastException` is thrown.</span></span>

## <a name="constructed-types"></a><span data-ttu-id="d43e3-397">Sestavené typy</span><span class="sxs-lookup"><span data-stu-id="d43e3-397">Constructed types</span></span>

<span data-ttu-id="d43e3-398">Deklarace obecného typu, samostatně, označuje ***nevázaných obecného typu*** jako "plán", který se používá k vytvoření mnoho různých typů, mimo jiné použití ***argumenty typu***.</span><span class="sxs-lookup"><span data-stu-id="d43e3-398">A generic type declaration, by itself, denotes an ***unbound generic type*** that is used as a "blueprint" to form many different types, by way of applying ***type arguments***.</span></span> <span data-ttu-id="d43e3-399">Zadejte argumenty jsou zapsány v lomených závorkách (`<` a `>`) hned za název obecného typu.</span><span class="sxs-lookup"><span data-stu-id="d43e3-399">The type arguments are written within angle brackets (`<` and `>`) immediately following the name of the generic type.</span></span> <span data-ttu-id="d43e3-400">Typ, který obsahuje alespoň jeden argument typu je volána ***konstruovaný typ.***.</span><span class="sxs-lookup"><span data-stu-id="d43e3-400">A type that includes at least one type argument is called a ***constructed type***.</span></span> <span data-ttu-id="d43e3-401">Konstruovaný typ. je možné ve většině případů v jazyce, ve kterém můžete zobrazit název typu.</span><span class="sxs-lookup"><span data-stu-id="d43e3-401">A constructed type can be used in most places in the language in which a type name can appear.</span></span> <span data-ttu-id="d43e3-402">Nevázaný parametr generického typu jde použít jenom v rámci *typeof_expression* ([operátor typeof](expressions.md#the-typeof-operator)).</span><span class="sxs-lookup"><span data-stu-id="d43e3-402">An unbound generic type can only be used within a *typeof_expression* ([The typeof operator](expressions.md#the-typeof-operator)).</span></span>

<span data-ttu-id="d43e3-403">Sestavené typy lze také použít ve výrazech jako jednoduchý názvy ([jednoduché názvy](expressions.md#simple-names)), nebo když přístup ke členovi ([přístup ke členu](expressions.md#member-access)).</span><span class="sxs-lookup"><span data-stu-id="d43e3-403">Constructed types can also be used in expressions as simple names ([Simple names](expressions.md#simple-names)) or when accessing a member ([Member access](expressions.md#member-access)).</span></span>

<span data-ttu-id="d43e3-404">Když *namespace_or_type_name* je Vyhodnocená, jenom obecné typy s správné číslo, které jsou považovány za parametry typu.</span><span class="sxs-lookup"><span data-stu-id="d43e3-404">When a *namespace_or_type_name* is evaluated, only generic types with the correct number of type parameters are considered.</span></span> <span data-ttu-id="d43e3-405">Proto je možné použít stejný identifikátor pro identifikaci různých typů, tak dlouho, dokud typy mají různý počet parametrů typu.</span><span class="sxs-lookup"><span data-stu-id="d43e3-405">Thus, it is possible to use the same identifier to identify different types, as long as the types have different numbers of type parameters.</span></span> <span data-ttu-id="d43e3-406">To je užitečné při kombinování obecných a neobecných třídách ve stejném programu:</span><span class="sxs-lookup"><span data-stu-id="d43e3-406">This is useful when mixing generic and non-generic classes in the same program:</span></span>

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

<span data-ttu-id="d43e3-407">A *type_name* konstruovaný typ může identifikovat, i když není přímo zadat parametry typu.</span><span class="sxs-lookup"><span data-stu-id="d43e3-407">A *type_name* might identify a constructed type even though it doesn't specify type parameters directly.</span></span> <span data-ttu-id="d43e3-408">Tato situace může nastat, pokud typu je vnořená v deklaraci obecné třídy a instance typu obsahující deklarace se implicitně používá pro vyhledávání názvu ([vnořené typy obecných tříd](classes.md#nested-types-in-generic-classes)):</span><span class="sxs-lookup"><span data-stu-id="d43e3-408">This can occur where a type is nested within a generic class declaration, and the instance type of the containing declaration is implicitly used for name lookup ([Nested types in generic classes](classes.md#nested-types-in-generic-classes)):</span></span>

```csharp
class Outer<T>
{
    public class Inner {...}

    public Inner i;                // Type of i is Outer<T>.Inner
}
```

<span data-ttu-id="d43e3-409">Nezabezpečený kód konstruovaný typ nelze použít jako *unmanaged_type* ([typy ukazatelů](unsafe-code.md#pointer-types)).</span><span class="sxs-lookup"><span data-stu-id="d43e3-409">In unsafe code, a constructed type cannot be used as an *unmanaged_type* ([Pointer types](unsafe-code.md#pointer-types)).</span></span>

### <a name="type-arguments"></a><span data-ttu-id="d43e3-410">Argumenty typu</span><span class="sxs-lookup"><span data-stu-id="d43e3-410">Type arguments</span></span>

<span data-ttu-id="d43e3-411">Každý argument v seznamu argumentů typu se používá jednoduchý *typ*.</span><span class="sxs-lookup"><span data-stu-id="d43e3-411">Each argument in a type argument list is simply a *type*.</span></span>

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

<span data-ttu-id="d43e3-412">Nezabezpečený kód ([nezabezpečený kód](unsafe-code.md)), *type_argument* nesmí být typu ukazatel.</span><span class="sxs-lookup"><span data-stu-id="d43e3-412">In unsafe code ([Unsafe code](unsafe-code.md)), a *type_argument* may not be a pointer type.</span></span> <span data-ttu-id="d43e3-413">Každý argument typu musí splňovat žádné omezení na odpovídající parametr typu ([omezení parametru typu](classes.md#type-parameter-constraints)).</span><span class="sxs-lookup"><span data-stu-id="d43e3-413">Each type argument must satisfy any constraints on the corresponding type parameter ([Type parameter constraints](classes.md#type-parameter-constraints)).</span></span>

### <a name="open-and-closed-types"></a><span data-ttu-id="d43e3-414">Otevřené a uzavřené typy</span><span class="sxs-lookup"><span data-stu-id="d43e3-414">Open and closed types</span></span>

<span data-ttu-id="d43e3-415">Všechny typy mohou být klasifikovány jako buď ***otevřete typy*** nebo ***uzavření typů***.</span><span class="sxs-lookup"><span data-stu-id="d43e3-415">All types can be classified as either ***open types*** or ***closed types***.</span></span> <span data-ttu-id="d43e3-416">Otevřený typ je typ, který zahrnuje parametry typu.</span><span class="sxs-lookup"><span data-stu-id="d43e3-416">An open type is a type that involves type parameters.</span></span> <span data-ttu-id="d43e3-417">Přesněji řečeno:</span><span class="sxs-lookup"><span data-stu-id="d43e3-417">More specifically:</span></span>

*  <span data-ttu-id="d43e3-418">Parametr typu definuje otevřeného typu.</span><span class="sxs-lookup"><span data-stu-id="d43e3-418">A type parameter defines an open type.</span></span>
*  <span data-ttu-id="d43e3-419">Typ pole je otevřený typ pouze v případě jeho typ prvku je otevřeného typu.</span><span class="sxs-lookup"><span data-stu-id="d43e3-419">An array type is an open type if and only if its element type is an open type.</span></span>
*  <span data-ttu-id="d43e3-420">Konstruovaný typ je otevřený typ a pouze v případě jeden nebo více jeho argumentů typu je otevřeného typu.</span><span class="sxs-lookup"><span data-stu-id="d43e3-420">A constructed type is an open type if and only if one or more of its type arguments is an open type.</span></span> <span data-ttu-id="d43e3-421">Konstruovaný vnořený typ je otevřený typ a pouze v případě nejméně jeden z argumentů typu nebo argumenty typu nadřazeného typu (typů) je otevřeného typu.</span><span class="sxs-lookup"><span data-stu-id="d43e3-421">A constructed nested type is an open type if and only if one or more of its type arguments or the type arguments of its containing type(s) is an open type.</span></span>

<span data-ttu-id="d43e3-422">Uzavřený typ je typ, který není otevřeného typu.</span><span class="sxs-lookup"><span data-stu-id="d43e3-422">A closed type is a type that is not an open type.</span></span>

<span data-ttu-id="d43e3-423">V době běhu veškerý kód v rámci deklarace obecného typu je provedena v kontextu uzavřený konstruovaný typ, který byl vytvořen použití argumentů typu pro obecný deklarace.</span><span class="sxs-lookup"><span data-stu-id="d43e3-423">At run-time, all of the code within a generic type declaration is executed in the context of a closed constructed type that was created by applying type arguments to the generic declaration.</span></span> <span data-ttu-id="d43e3-424">Každý z parametrů typu v rámci obecného typu je vázán na konkrétním typu za běhu.</span><span class="sxs-lookup"><span data-stu-id="d43e3-424">Each type parameter within the generic type is bound to a particular run-time type.</span></span> <span data-ttu-id="d43e3-425">Zpracování za běhu všechny příkazy a výrazy vždy dojde k zavřené typy a otevřené typy dojde jenom během kompilace zpracování.</span><span class="sxs-lookup"><span data-stu-id="d43e3-425">The run-time processing of all statements and expressions always occurs with closed types, and open types occur only during compile-time processing.</span></span>

<span data-ttu-id="d43e3-426">Každý uzavřený konstruovaný typ. má svou vlastní sadu statických proměnných, které nejsou sdíleny s jinými uzavřený konstruovaný typy.</span><span class="sxs-lookup"><span data-stu-id="d43e3-426">Each closed constructed type has its own set of static variables, which are not shared with any other closed constructed types.</span></span> <span data-ttu-id="d43e3-427">Protože otevřeného typu v době běhu neexistuje, nejsou žádné statických proměnných spojených s otevřeného typu.</span><span class="sxs-lookup"><span data-stu-id="d43e3-427">Since an open type does not exist at run-time, there are no static variables associated with an open type.</span></span> <span data-ttu-id="d43e3-428">Dva typy uzavřený konstruovaný jsou stejného typu, pokud jsou konstruovány ze stejné nevázaný parametr generického typu a jejich odpovídající argumenty typu jsou stejného typu.</span><span class="sxs-lookup"><span data-stu-id="d43e3-428">Two closed constructed types are the same type if they are constructed from the same unbound generic type, and their corresponding type arguments are the same type.</span></span>

### <a name="bound-and-unbound-types"></a><span data-ttu-id="d43e3-429">Provázaná a nevázaného typy</span><span class="sxs-lookup"><span data-stu-id="d43e3-429">Bound and unbound types</span></span>

<span data-ttu-id="d43e3-430">Termín ***nevázaný typ*** odkazuje na jiného než obecného typu nebo nevázaný parametr generického typu.</span><span class="sxs-lookup"><span data-stu-id="d43e3-430">The term ***unbound type*** refers to a non-generic type or an unbound generic type.</span></span> <span data-ttu-id="d43e3-431">Termín ***vázaný typ*** odkazuje na jiného než obecného typu nebo konstruovaný typ.</span><span class="sxs-lookup"><span data-stu-id="d43e3-431">The term ***bound type*** refers to a non-generic type or a constructed type.</span></span>

<span data-ttu-id="d43e3-432">Nevázanému typu odkazuje na entity deklarované v deklaraci typu.</span><span class="sxs-lookup"><span data-stu-id="d43e3-432">An unbound type refers to the entity declared by a type declaration.</span></span> <span data-ttu-id="d43e3-433">Nevázaný parametr generického typu je sám není typem a nelze použít jako typ proměnné, argumentu nebo návratovou hodnotu, nebo jako základního typu.</span><span class="sxs-lookup"><span data-stu-id="d43e3-433">An unbound generic type is not itself a type, and cannot be used as the type of a variable, argument or return value, or as a base type.</span></span> <span data-ttu-id="d43e3-434">Jediný konstruktor, ve kterém může být odkazováno nevázaný parametr generického typu je `typeof` výrazu ([operátor typeof](expressions.md#the-typeof-operator)).</span><span class="sxs-lookup"><span data-stu-id="d43e3-434">The only construct in which an unbound generic type can be referenced is the `typeof` expression ([The typeof operator](expressions.md#the-typeof-operator)).</span></span>

### <a name="satisfying-constraints"></a><span data-ttu-id="d43e3-435">Splňující omezení</span><span class="sxs-lookup"><span data-stu-id="d43e3-435">Satisfying constraints</span></span>

<span data-ttu-id="d43e3-436">Pokaždé, když je odkazováno konstruovaný typ nebo obecné metody, zadaný typ argumenty jsou porovnávána s omezeními parametrů typů deklarována v obecném typu nebo metodě ([omezení parametru typu](classes.md#type-parameter-constraints)).</span><span class="sxs-lookup"><span data-stu-id="d43e3-436">Whenever a constructed type or generic method is referenced, the supplied type arguments are checked against the type parameter constraints declared on the generic type or method ([Type parameter constraints](classes.md#type-parameter-constraints)).</span></span> <span data-ttu-id="d43e3-437">Pro každou `where` klauzule, argument typu `A` , který odpovídá pojmenovaný parametr typu je porovnávána s každé omezení následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="d43e3-437">For each `where` clause, the type argument `A` that corresponds to the named type parameter is checked against each constraint as follows:</span></span>

*  <span data-ttu-id="d43e3-438">Pokud je omezení typu třídy, typ rozhraní nebo parametr typu, dejte `C` představují, že omezení s zadaný typ argumenty nahrazené za všechny parametry typu, které se zobrazují v omezení.</span><span class="sxs-lookup"><span data-stu-id="d43e3-438">If the constraint is a class type, an interface type, or a type parameter, let `C` represent that constraint with the supplied type arguments substituted for any type parameters that appear in the constraint.</span></span> <span data-ttu-id="d43e3-439">Tím se uspokojí omezení, musí být případ, který typ `A` převést na typ `C` pomocí jedné z následujících akcí:</span><span class="sxs-lookup"><span data-stu-id="d43e3-439">To satisfy the constraint, it must be the case that type `A` is convertible to type `C` by one of the following:</span></span>
    * <span data-ttu-id="d43e3-440">Konverzi identity ([Identity převod](conversions.md#identity-conversion))</span><span class="sxs-lookup"><span data-stu-id="d43e3-440">An identity conversion ([Identity conversion](conversions.md#identity-conversion))</span></span>
    * <span data-ttu-id="d43e3-441">Implicitní referenční převod ([odkaz na implicitní převody](conversions.md#implicit-reference-conversions))</span><span class="sxs-lookup"><span data-stu-id="d43e3-441">An implicit reference conversion ([Implicit reference conversions](conversions.md#implicit-reference-conversions))</span></span>
    * <span data-ttu-id="d43e3-442">Převod na uzavřené určení ([zabalení převody](conversions.md#boxing-conversions)), za předpokladu, že je typ A typ hodnotu Null.</span><span class="sxs-lookup"><span data-stu-id="d43e3-442">A boxing conversion ([Boxing conversions](conversions.md#boxing-conversions)), provided that type A is a non-nullable value type.</span></span>
    * <span data-ttu-id="d43e3-443">Implicitní převod parametrů typu odkazu, zabalení nebo z parametru typu `A` k `C`.</span><span class="sxs-lookup"><span data-stu-id="d43e3-443">An implicit reference, boxing or type parameter conversion from a type parameter `A` to `C`.</span></span>
*  <span data-ttu-id="d43e3-444">Pokud je omezení, omezení typu odkazu (`class`), typ `A` musí splňovat jeden z následujících akcí:</span><span class="sxs-lookup"><span data-stu-id="d43e3-444">If the constraint is the reference type constraint (`class`), the type `A` must satisfy one of the following:</span></span>
    * <span data-ttu-id="d43e3-445">`A` je typ rozhraní, typ třídy, typ delegáta nebo typ pole.</span><span class="sxs-lookup"><span data-stu-id="d43e3-445">`A` is an interface type, class type, delegate type or array type.</span></span> <span data-ttu-id="d43e3-446">Všimněte si, že `System.ValueType` a `System.Enum` jsou odkazové typy, které odpovídají tomuto omezení.</span><span class="sxs-lookup"><span data-stu-id="d43e3-446">Note that `System.ValueType` and `System.Enum` are reference types that satisfy this constraint.</span></span>
    * <span data-ttu-id="d43e3-447">`A` je parametr typu, který je znám jako typ odkazu ([omezení parametru typu](classes.md#type-parameter-constraints)).</span><span class="sxs-lookup"><span data-stu-id="d43e3-447">`A` is a type parameter that is known to be a reference type ([Type parameter constraints](classes.md#type-parameter-constraints)).</span></span>
*  <span data-ttu-id="d43e3-448">Pokud je typ omezení hodnoty omezení (`struct`), typ `A` musí splňovat jeden z následujících akcí:</span><span class="sxs-lookup"><span data-stu-id="d43e3-448">If the constraint is the value type constraint (`struct`), the type `A` must satisfy one of the following:</span></span>
    * <span data-ttu-id="d43e3-449">`A` je typ struktury nebo výčtového typu, ale není typu s možnou hodnotou Null.</span><span class="sxs-lookup"><span data-stu-id="d43e3-449">`A` is a struct type or enum type, but not a nullable type.</span></span> <span data-ttu-id="d43e3-450">Všimněte si, že `System.ValueType` a `System.Enum` jsou odkazové typy, které nesplňují omezení.</span><span class="sxs-lookup"><span data-stu-id="d43e3-450">Note that `System.ValueType` and `System.Enum` are reference types that do not satisfy this constraint.</span></span>
    * <span data-ttu-id="d43e3-451">`A` je parametr typu s omezení typu hodnoty ([omezení parametru typu](classes.md#type-parameter-constraints)).</span><span class="sxs-lookup"><span data-stu-id="d43e3-451">`A` is a type parameter having the value type constraint ([Type parameter constraints](classes.md#type-parameter-constraints)).</span></span>
*  <span data-ttu-id="d43e3-452">Pokud je omezení, omezení konstruktoru `new()`, typ `A` nesmí být `abstract` a musí mít veřejný konstruktor bez parametrů.</span><span class="sxs-lookup"><span data-stu-id="d43e3-452">If the constraint is the constructor constraint `new()`, the type `A` must not be `abstract` and must have a public parameterless constructor.</span></span> <span data-ttu-id="d43e3-453">Tím je splněna, pokud platí jedna z následujících akcí:</span><span class="sxs-lookup"><span data-stu-id="d43e3-453">This is satisfied if one of the following is true:</span></span>
    * <span data-ttu-id="d43e3-454">`A` je typ hodnoty, protože všechny hodnotové typy mít veřejný výchozí konstruktor ([výchozí konstruktory](types.md#default-constructors)).</span><span class="sxs-lookup"><span data-stu-id="d43e3-454">`A` is a value type, since all value types have a public default constructor ([Default constructors](types.md#default-constructors)).</span></span>
    * <span data-ttu-id="d43e3-455">`A` je parametr typu s omezení konstruktoru ([omezení parametru typu](classes.md#type-parameter-constraints)).</span><span class="sxs-lookup"><span data-stu-id="d43e3-455">`A` is a type parameter having the constructor constraint ([Type parameter constraints](classes.md#type-parameter-constraints)).</span></span>
    * <span data-ttu-id="d43e3-456">`A` je parametr typu s omezení typu hodnoty ([omezení parametru typu](classes.md#type-parameter-constraints)).</span><span class="sxs-lookup"><span data-stu-id="d43e3-456">`A` is a type parameter having the value type constraint ([Type parameter constraints](classes.md#type-parameter-constraints)).</span></span>
    * <span data-ttu-id="d43e3-457">`A` je třída, která není `abstract` a obsahuje explicitně deklarované `public` konstruktor bez parametrů.</span><span class="sxs-lookup"><span data-stu-id="d43e3-457">`A` is a class that is not `abstract` and contains an explicitly declared `public` constructor with no parameters.</span></span>
    * <span data-ttu-id="d43e3-458">`A` není `abstract` a má výchozí konstruktor ([výchozí konstruktory](classes.md#default-constructors)).</span><span class="sxs-lookup"><span data-stu-id="d43e3-458">`A` is not `abstract` and has a default constructor ([Default constructors](classes.md#default-constructors)).</span></span>

<span data-ttu-id="d43e3-459">Chyba kompilace nastane, pokud jeden nebo více omezení parametru typu nejsou splněné argumenty daného typu.</span><span class="sxs-lookup"><span data-stu-id="d43e3-459">A compile-time error occurs if one or more of a type parameter's constraints are not satisfied by the given type arguments.</span></span>

<span data-ttu-id="d43e3-460">Protože nedědí parametry typu, omezení nejsou nikdy buď dědí.</span><span class="sxs-lookup"><span data-stu-id="d43e3-460">Since type parameters are not inherited, constraints are never inherited either.</span></span> <span data-ttu-id="d43e3-461">V následujícím příkladu `D` musí určovat omezení u jeho typu parametru `T` tak, aby `T` splňuje omezení stanovené základní třídy `B<T>`.</span><span class="sxs-lookup"><span data-stu-id="d43e3-461">In the example below, `D` needs to specify the constraint on its type parameter `T` so that `T` satisfies the constraint imposed by the base class `B<T>`.</span></span> <span data-ttu-id="d43e3-462">Naproti tomu třídy `E` nemusí zadat omezení, protože `List<T>` implementuje `IEnumerable` libovolné `T`.</span><span class="sxs-lookup"><span data-stu-id="d43e3-462">In contrast, class `E` need not specify a constraint, because `List<T>` implements `IEnumerable` for any `T`.</span></span>

```csharp
class B<T> where T: IEnumerable {...}

class D<T>: B<T> where T: IEnumerable {...}

class E<T>: B<List<T>> {...}
```

## <a name="type-parameters"></a><span data-ttu-id="d43e3-463">Parametry typu</span><span class="sxs-lookup"><span data-stu-id="d43e3-463">Type parameters</span></span>

<span data-ttu-id="d43e3-464">Parametr typu je identifikátor označující typ hodnoty nebo typ odkazu, který parametr vázán v době běhu.</span><span class="sxs-lookup"><span data-stu-id="d43e3-464">A type parameter is an identifier designating a value type or reference type that the parameter is bound to at run-time.</span></span>

```antlr
type_parameter
    : identifier
    ;
```

<span data-ttu-id="d43e3-465">Protože parametr typu může být vytvořena s mnoha různých skutečný typ argumentů, parametry typu mají mírně odlišné operace a omezení než u jiných typů.</span><span class="sxs-lookup"><span data-stu-id="d43e3-465">Since a type parameter can be instantiated with many different actual type arguments, type parameters have slightly different operations and restrictions than other types.</span></span> <span data-ttu-id="d43e3-466">Zde jsou některé z nich:</span><span class="sxs-lookup"><span data-stu-id="d43e3-466">These include:</span></span>

*  <span data-ttu-id="d43e3-467">Parametr typu nelze použít přímo na deklarace základní třídy ([základní třída](classes.md#base-class)) nebo rozhraní ([seznamy parametru typu Variant](interfaces.md#variant-type-parameter-lists)).</span><span class="sxs-lookup"><span data-stu-id="d43e3-467">A type parameter cannot be used directly to declare a base class ([Base class](classes.md#base-class)) or interface ([Variant type parameter lists](interfaces.md#variant-type-parameter-lists)).</span></span>
*  <span data-ttu-id="d43e3-468">Pravidla pro vyhledávání člen v typu, že parametry závisí na omezení, pokud existuje, použitý u parametru typu.</span><span class="sxs-lookup"><span data-stu-id="d43e3-468">The rules for member lookup on type parameters depend on the constraints, if any, applied to the type parameter.</span></span> <span data-ttu-id="d43e3-469">Jsou podrobně popsány v [člen vyhledávání](expressions.md#member-lookup).</span><span class="sxs-lookup"><span data-stu-id="d43e3-469">They are detailed in [Member lookup](expressions.md#member-lookup).</span></span>
*  <span data-ttu-id="d43e3-470">K dispozici převody pro parametr typu závisí na omezení, pokud existuje, použitý u parametru typu.</span><span class="sxs-lookup"><span data-stu-id="d43e3-470">The available conversions for a type parameter depend on the constraints, if any, applied to the type parameter.</span></span> <span data-ttu-id="d43e3-471">Jsou podrobně popsány v [implicitních převodů zahrnující parametry typu](conversions.md#implicit-conversions-involving-type-parameters) a [explicitních převodů dynamické](conversions.md#explicit-dynamic-conversions).</span><span class="sxs-lookup"><span data-stu-id="d43e3-471">They are detailed in [Implicit conversions involving type parameters](conversions.md#implicit-conversions-involving-type-parameters) and [Explicit dynamic conversions](conversions.md#explicit-dynamic-conversions).</span></span>
*  <span data-ttu-id="d43e3-472">Literál `null` nelze převést na typ daný parametr typu s výjimkou případů, pokud je parametr typu je znám jako typ odkazu ([implicitních převodů zahrnující parametry typu](conversions.md#implicit-conversions-involving-type-parameters)).</span><span class="sxs-lookup"><span data-stu-id="d43e3-472">The literal `null` cannot be converted to a type given by a type parameter, except if the type parameter is known to be a reference type ([Implicit conversions involving type parameters](conversions.md#implicit-conversions-involving-type-parameters)).</span></span> <span data-ttu-id="d43e3-473">Ale `default` výrazu ([výchozí hodnotu výrazy](expressions.md#default-value-expressions)) lze použít.</span><span class="sxs-lookup"><span data-stu-id="d43e3-473">However, a `default` expression ([Default value expressions](expressions.md#default-value-expressions)) can be used instead.</span></span> <span data-ttu-id="d43e3-474">Kromě toho lze porovnat hodnotu s typ daný parametr typu s `null` pomocí `==` a `!=` ([operátory rovnosti pro typ odkazu](expressions.md#reference-type-equality-operators)), pokud parametr typu neobsahuje omezení typu hodnoty.</span><span class="sxs-lookup"><span data-stu-id="d43e3-474">In addition, a value with a type given by a type parameter can be compared with `null` using `==` and `!=` ([Reference type equality operators](expressions.md#reference-type-equality-operators)) unless the type parameter has the value type constraint.</span></span>
*  <span data-ttu-id="d43e3-475">A `new` výrazu ([výrazy vytvoření objektu](expressions.md#object-creation-expressions)) lze použít pouze s parametrem typu Pokud parametr typu je omezená *constructor_constraint* nebo typ omezení (hodnoty[ Typ omezení parametru](classes.md#type-parameter-constraints)).</span><span class="sxs-lookup"><span data-stu-id="d43e3-475">A `new` expression ([Object creation expressions](expressions.md#object-creation-expressions)) can only be used with a type parameter if the type parameter is constrained by a *constructor_constraint* or the value type constraint ([Type parameter constraints](classes.md#type-parameter-constraints)).</span></span>
*  <span data-ttu-id="d43e3-476">Parametr typu nelze použít kdekoli v rámci atributu.</span><span class="sxs-lookup"><span data-stu-id="d43e3-476">A type parameter cannot be used anywhere within an attribute.</span></span>
*  <span data-ttu-id="d43e3-477">Parametr typu nelze použít v přístupu ke členu ([přístup ke členu](expressions.md#member-access)) nebo název typu ([Namespace a zadejte názvy](basic-concepts.md#namespace-and-type-names)) k identifikaci statických členů nebo vnořeného typu.</span><span class="sxs-lookup"><span data-stu-id="d43e3-477">A type parameter cannot be used in a member access ([Member access](expressions.md#member-access)) or type name ([Namespace and type names](basic-concepts.md#namespace-and-type-names)) to identify a static member or a nested type.</span></span>
*  <span data-ttu-id="d43e3-478">Nebezpečný kód, parametr typu nelze použít jako *unmanaged_type* ([typy ukazatelů](unsafe-code.md#pointer-types)).</span><span class="sxs-lookup"><span data-stu-id="d43e3-478">In unsafe code, a type parameter cannot be used as an *unmanaged_type* ([Pointer types](unsafe-code.md#pointer-types)).</span></span>

<span data-ttu-id="d43e3-479">Jako typ parametry typu jsou čistě konstrukci za kompilace.</span><span class="sxs-lookup"><span data-stu-id="d43e3-479">As a type, type parameters are purely a compile-time construct.</span></span> <span data-ttu-id="d43e3-480">V době běhu je každý parametr typu vázán na run-time typu, který byl zadán zadáním argument typu pro obecný typ deklarace.</span><span class="sxs-lookup"><span data-stu-id="d43e3-480">At run-time, each type parameter is bound to a run-time type that was specified by supplying a type argument to the generic type declaration.</span></span> <span data-ttu-id="d43e3-481">Typ proměnné deklarované pomocí parametru typ se v době běhu, proto být uzavřený konstruovaný typ. ([otevřené a uzavřené typy](types.md#open-and-closed-types)).</span><span class="sxs-lookup"><span data-stu-id="d43e3-481">Thus, the type of a variable declared with a type parameter will, at run-time, be a closed constructed type ([Open and closed types](types.md#open-and-closed-types)).</span></span> <span data-ttu-id="d43e3-482">Provádění běhu všechny příkazy a výrazy, které zahrnují parametry typu používá skutečný typ, který byl zadán jako argument typu pro tento parametr.</span><span class="sxs-lookup"><span data-stu-id="d43e3-482">The run-time execution of all statements and expressions involving type parameters uses the actual type that was supplied as the type argument for that parameter.</span></span>

## <a name="expression-tree-types"></a><span data-ttu-id="d43e3-483">Typy stromu výrazů</span><span class="sxs-lookup"><span data-stu-id="d43e3-483">Expression tree types</span></span>

<span data-ttu-id="d43e3-484">***Stromy výrazů*** povolit lambda výrazy, aby se dala vyjádřit jako datové struktury místo spustitelného kódu.</span><span class="sxs-lookup"><span data-stu-id="d43e3-484">***Expression trees*** permit lambda expressions to be represented as data structures instead of executable code.</span></span> <span data-ttu-id="d43e3-485">Stromy výrazů jsou hodnoty ***typy stromu výrazů*** formuláře `System.Linq.Expressions.Expression<D>`, kde `D` libovolný typ delegáta.</span><span class="sxs-lookup"><span data-stu-id="d43e3-485">Expression trees are values of ***expression tree types*** of the form `System.Linq.Expressions.Expression<D>`, where `D` is any delegate type.</span></span> <span data-ttu-id="d43e3-486">Zbytek této specifikaci jsme bude odkazovat na tyto typy pomocí zkrácený `Expression<D>`.</span><span class="sxs-lookup"><span data-stu-id="d43e3-486">For the remainder of this specification we will refer to these types using the shorthand `Expression<D>`.</span></span>

<span data-ttu-id="d43e3-487">Pokud existuje z výrazu lambda převod na typ delegáta `D`, také existuje převod do typu stromu výrazu `Expression<D>`.</span><span class="sxs-lookup"><span data-stu-id="d43e3-487">If a conversion exists from a lambda expression to a delegate type `D`, a conversion also exists to the expression tree type `Expression<D>`.</span></span> <span data-ttu-id="d43e3-488">Zatímco převodu výrazu lambda, typu delegátu generuje delegáta, který odkazuje na spustitelného kódu pro výraz lambda, převod na typu stromu výrazu vytvoří reprezentaci výrazu strom výrazu lambda.</span><span class="sxs-lookup"><span data-stu-id="d43e3-488">Whereas the conversion of a lambda expression to a delegate type generates a delegate that references executable code for the lambda expression, conversion to an expression tree type creates an expression tree representation of the lambda expression.</span></span>

<span data-ttu-id="d43e3-489">Stromy výrazů jsou efektivní data v paměti reprezentace výrazů lambda a struktuře výrazu lambda transparentní a explicitní.</span><span class="sxs-lookup"><span data-stu-id="d43e3-489">Expression trees are efficient in-memory data representations of lambda expressions and make the structure of the lambda expression transparent and explicit.</span></span>

<span data-ttu-id="d43e3-490">Stejně jako typ delegáta `D`, `Expression<D>` má parametr a návratové typy, které jsou stejné jako u `D`.</span><span class="sxs-lookup"><span data-stu-id="d43e3-490">Just like a delegate type `D`, `Expression<D>` is said to have parameter and return types, which are the same as those of `D`.</span></span>

<span data-ttu-id="d43e3-491">Následující příklad představuje výraz lambda jako spustitelný kód i ve stromu výrazu.</span><span class="sxs-lookup"><span data-stu-id="d43e3-491">The following example represents a lambda expression both as executable code and as an expression tree.</span></span> <span data-ttu-id="d43e3-492">Vzhledem k tomu, že existuje převod na `Func<int,int>`, také existuje převod na `Expression<Func<int,int>>`:</span><span class="sxs-lookup"><span data-stu-id="d43e3-492">Because a conversion exists to `Func<int,int>`, a conversion also exists to `Expression<Func<int,int>>`:</span></span>

```csharp
Func<int,int> del = x => x + 1;                    // Code

Expression<Func<int,int>> exp = x => x + 1;        // Data
```

<span data-ttu-id="d43e3-493">Následující tato přiřazení delegáta `del` odkazuje na metodu, která vrací `x + 1`a strom výrazu `exp` odkazuje datová struktura, která popisuje výraz `x => x + 1`.</span><span class="sxs-lookup"><span data-stu-id="d43e3-493">Following these assignments, the delegate `del` references a method that returns `x + 1`, and the expression tree `exp` references a data structure that describes the expression `x => x + 1`.</span></span>

<span data-ttu-id="d43e3-494">Přesné definici obecného typu `Expression<D>` a také přesné pravidla pro vytváření stromu výrazů při převodu typu stromu výrazu lambda výraz, jsou nad rámec téhle specifikaci.</span><span class="sxs-lookup"><span data-stu-id="d43e3-494">The exact definition of the generic type `Expression<D>` as well as the precise rules for constructing an expression tree when a lambda expression is converted to an expression tree type, are both outside the scope of this specification.</span></span>

<span data-ttu-id="d43e3-495">Je důležité, aby explicitní dvě věci:</span><span class="sxs-lookup"><span data-stu-id="d43e3-495">Two things are important to make explicit:</span></span>

*  <span data-ttu-id="d43e3-496">Ne všechny lambda výrazy můžete převést na stromy výrazů.</span><span class="sxs-lookup"><span data-stu-id="d43e3-496">Not all lambda expressions can be converted to expression trees.</span></span> <span data-ttu-id="d43e3-497">Nelze reprezentovat například lambda výrazy s těla příkazu a výrazů lambda, který obsahuje výrazy přiřazení.</span><span class="sxs-lookup"><span data-stu-id="d43e3-497">For instance, lambda expressions with statement bodies, and lambda expressions containing assignment expressions cannot be represented.</span></span> <span data-ttu-id="d43e3-498">V těchto případech převod stále existuje, ale selže v době kompilace.</span><span class="sxs-lookup"><span data-stu-id="d43e3-498">In these cases, a conversion still exists, but will fail at compile-time.</span></span> <span data-ttu-id="d43e3-499">Tyto výjimky jsou podrobně popsány v [anonymní funkce převody](conversions.md#anonymous-function-conversions).</span><span class="sxs-lookup"><span data-stu-id="d43e3-499">These exceptions are detailed in [Anonymous function conversions](conversions.md#anonymous-function-conversions).</span></span>
*   <span data-ttu-id="d43e3-500">`Expression<D>` poskytuje metodu instance `Compile` produkuje delegát typu `D`:</span><span class="sxs-lookup"><span data-stu-id="d43e3-500">`Expression<D>` offers an instance method `Compile` which produces a delegate of type `D`:</span></span>

    ```csharp
    Func<int,int> del2 = exp.Compile();
    ```

    <span data-ttu-id="d43e3-501">Vyvolání tohoto delegáta způsobí, že kód reprezentována stromu výrazů má být proveden.</span><span class="sxs-lookup"><span data-stu-id="d43e3-501">Invoking this delegate causes the code represented by the expression tree to be executed.</span></span> <span data-ttu-id="d43e3-502">Proto výše uvedené definice, del a del2 jsou ekvivalentní a následující dva příkazy bude mít stejný účinek:</span><span class="sxs-lookup"><span data-stu-id="d43e3-502">Thus, given the definitions above, del and del2 are equivalent, and the following two statements will have the same effect:</span></span>

    ```csharp
    int i1 = del(1);
    
    int i2 = del2(1);
    ```

    <span data-ttu-id="d43e3-503">Po spuštění tohoto kódu `i1` a `i2` budou mít obě hodnotu `2`.</span><span class="sxs-lookup"><span data-stu-id="d43e3-503">After executing this code,  `i1` and `i2` will both have the value `2`.</span></span>

