---
ms.openlocfilehash: d6519ff57b4a98c4eec8ccbf310303432ac3255e
ms.sourcegitcommit: 65ea1e6dc02853e37e7f2088e2b6cc08d01d1044
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/27/2019
ms.locfileid: "79485026"
---
# <a name="ranges"></a><span data-ttu-id="28ba8-101">Rozsahy</span><span class="sxs-lookup"><span data-stu-id="28ba8-101">Ranges</span></span>

## <a name="summary"></a><span data-ttu-id="28ba8-102">Souhrn</span><span class="sxs-lookup"><span data-stu-id="28ba8-102">Summary</span></span>

<span data-ttu-id="28ba8-103">Tato funkce se týká poskytování dvou nových operátorů, které umožňují vytvářet objekty `System.Index` a `System.Range` a používat je k indexování a řezů kolekcí za běhu.</span><span class="sxs-lookup"><span data-stu-id="28ba8-103">This feature is about delivering two new operators that allow constructing `System.Index` and `System.Range` objects, and using them to index/slice collections at runtime.</span></span>

## <a name="overview"></a><span data-ttu-id="28ba8-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="28ba8-104">Overview</span></span>

### <a name="well-known-types-and-members"></a><span data-ttu-id="28ba8-105">Dobře známé typy a členy</span><span class="sxs-lookup"><span data-stu-id="28ba8-105">Well-known types and members</span></span>

<span data-ttu-id="28ba8-106">Chcete-li použít nové syntaktické formy pro `System.Index` a `System.Range`, mohou být v závislosti na tom, jaké syntaktické tvary použity, pravděpodobně nezbytné nové známé typy a členy.</span><span class="sxs-lookup"><span data-stu-id="28ba8-106">To use the new syntactic forms for `System.Index` and `System.Range`, new well-known types and members may be necessary, depending on which syntactic forms are used.</span></span>

<span data-ttu-id="28ba8-107">Chcete-li použít operátor "Hat" (`^`), je nutné zadat následující</span><span class="sxs-lookup"><span data-stu-id="28ba8-107">To use the "hat" operator (`^`), the following is required</span></span>

```csharp
namespace System
{
    public readonly struct Index
    {
        public Index(int value, bool fromEnd);
    }
}
```

<span data-ttu-id="28ba8-108">Chcete-li použít typ `System.Index` jako argument v přístupu k elementu pole, je požadován následující člen:</span><span class="sxs-lookup"><span data-stu-id="28ba8-108">To use the `System.Index` type as an argument in an array element access, the following member is required:</span></span>

```csharp
int System.Index.GetOffset(int length);
```

<span data-ttu-id="28ba8-109">Syntaxe `..` pro `System.Range` bude vyžadovat typ `System.Range` a také jeden nebo více následujících členů:</span><span class="sxs-lookup"><span data-stu-id="28ba8-109">The `..` syntax for `System.Range` will require the `System.Range` type, as well as one or more of the following members:</span></span>

```csharp
namespace System
{
    public readonly struct Range
    {
        public Range(System.Index start, System.Index end);
        public static Range StartAt(System.Index start);
        public static Range EndAt(System.Index end);
        public static Range All { get; }
    }
}
```

<span data-ttu-id="28ba8-110">Syntaxe `..` umožňuje buď, nebo žádné argumenty, které nemají chybět.</span><span class="sxs-lookup"><span data-stu-id="28ba8-110">The `..` syntax allows for either, both, or none of its arguments to be absent.</span></span> <span data-ttu-id="28ba8-111">Bez ohledu na počet argumentů je konstruktor `Range` vždy dostačující pro použití syntaxe `Range`.</span><span class="sxs-lookup"><span data-stu-id="28ba8-111">Regardless of the number of arguments, the `Range` constructor is always sufficient for using the `Range` syntax.</span></span> <span data-ttu-id="28ba8-112">Pokud je však přítomen některý z ostatních členů a chybí jeden nebo více argumentů `..`, může být příslušný člen nahrazen.</span><span class="sxs-lookup"><span data-stu-id="28ba8-112">However, if any of the other members are present and one or more of the `..` arguments are missing, the appropriate member may be substituted.</span></span>

<span data-ttu-id="28ba8-113">Nakonec pro hodnotu typu `System.Range` pro použití ve výrazu přístupu k elementu pole musí být k dispozici následující člen:</span><span class="sxs-lookup"><span data-stu-id="28ba8-113">Finally, for a value of type `System.Range` to be used in an array element access expression, the following member must be present:</span></span>

```csharp
namespace System.Runtime.CompilerServices
{
    public static class RuntimeHelpers
    {
        public static T[] GetSubArray<T>(T[] array, System.Range range);
    }
}
```

### <a name="systemindex"></a><span data-ttu-id="28ba8-114">System. index</span><span class="sxs-lookup"><span data-stu-id="28ba8-114">System.Index</span></span>

<span data-ttu-id="28ba8-115">C#nemá žádný způsob, jak indexovat kolekci z konce, ale místo toho většina indexerů používá výraz "od začátku" nebo "length-i".</span><span class="sxs-lookup"><span data-stu-id="28ba8-115">C# has no way of indexing a collection from the end, but rather most indexers use the "from start" notion, or do a "length - i" expression.</span></span> <span data-ttu-id="28ba8-116">Zavádíme nový Indexový výraz, který znamená "od konce".</span><span class="sxs-lookup"><span data-stu-id="28ba8-116">We introduce a new Index expression that means "from the end".</span></span> <span data-ttu-id="28ba8-117">Funkce zavede nový unární operátor "Hat".</span><span class="sxs-lookup"><span data-stu-id="28ba8-117">The feature will introduce a new unary prefix "hat" operator.</span></span> <span data-ttu-id="28ba8-118">Jeho jeden operand musí být převoditelné na `System.Int32`.</span><span class="sxs-lookup"><span data-stu-id="28ba8-118">Its single operand must be convertible to `System.Int32`.</span></span> <span data-ttu-id="28ba8-119">Bude snížena na příslušné volání metody továrny `System.Index`.</span><span class="sxs-lookup"><span data-stu-id="28ba8-119">It will be lowered into the appropriate `System.Index` factory method call.</span></span>

<span data-ttu-id="28ba8-120">Gramatiku pro *unary_expression* rozšiřujeme pomocí následujícího formuláře další syntaxe:</span><span class="sxs-lookup"><span data-stu-id="28ba8-120">We augment the grammar for *unary_expression* with the following additional syntax form:</span></span>

```antlr
unary_expression
    : '^' unary_expression
    ;
```

<span data-ttu-id="28ba8-121">Tento index voláme *od* operátoru end.</span><span class="sxs-lookup"><span data-stu-id="28ba8-121">We call this the *index from end* operator.</span></span> <span data-ttu-id="28ba8-122">Předdefinovaný *index od koncových* operátorů je následující:</span><span class="sxs-lookup"><span data-stu-id="28ba8-122">The predefined *index from end* operators are as follows:</span></span>

```csharp
    System.Index operator ^(int fromEnd);
```

<span data-ttu-id="28ba8-123">Chování tohoto operátoru je definováno pouze pro vstupní hodnoty větší nebo rovny nule.</span><span class="sxs-lookup"><span data-stu-id="28ba8-123">The behavior of this operator is only defined for input values greater than or equal to zero.</span></span>

<span data-ttu-id="28ba8-124">Příklady:</span><span class="sxs-lookup"><span data-stu-id="28ba8-124">Examples:</span></span>

```csharp
var thirdItem = list[2];                // list[2]
var lastItem = list[^1];                // list[Index.CreateFromEnd(1)]

var multiDimensional = list[3, ^2]      // list[3, Index.CreateFromEnd(2)]
```

#### <a name="systemrange"></a><span data-ttu-id="28ba8-125">System. Range</span><span class="sxs-lookup"><span data-stu-id="28ba8-125">System.Range</span></span>

<span data-ttu-id="28ba8-126">C#nemá žádný syntaktický způsob pro přístup k "rozsahům" nebo "řezů" kolekcí.</span><span class="sxs-lookup"><span data-stu-id="28ba8-126">C# has no syntactic way to access "ranges" or "slices" of collections.</span></span> <span data-ttu-id="28ba8-127">Obvykle jsou uživatelé nuceni implementovat komplexní struktury pro filtrování/fungování v řezech paměti nebo pro metody LINQ, jako je `list.Skip(5).Take(2)`.</span><span class="sxs-lookup"><span data-stu-id="28ba8-127">Usually users are forced to implement complex structures to filter/operate on slices of memory, or resort to LINQ methods like `list.Skip(5).Take(2)`.</span></span> <span data-ttu-id="28ba8-128">Díky přidání `System.Span<T>` a dalších podobných typů je důležitější, aby tento druh operace byl podporován na hlubší úrovni jazyka a modulu runtime a měly rozhraní jednotně.</span><span class="sxs-lookup"><span data-stu-id="28ba8-128">With the addition of `System.Span<T>` and other similar types, it becomes more important to have this kind of operation supported on a deeper level in the language/runtime, and have the interface unified.</span></span>

<span data-ttu-id="28ba8-129">Jazyk zavádí nový operátor rozsahu `x..y`.</span><span class="sxs-lookup"><span data-stu-id="28ba8-129">The language will introduce a new range operator `x..y`.</span></span> <span data-ttu-id="28ba8-130">Je to binární operátor vpony, který přijímá dva výrazy.</span><span class="sxs-lookup"><span data-stu-id="28ba8-130">It is a binary infix operator that accepts two expressions.</span></span> <span data-ttu-id="28ba8-131">Operand může být vynechán (níže uvedené příklady) a musí být převoditelné na `System.Index`.</span><span class="sxs-lookup"><span data-stu-id="28ba8-131">Either operand can be omitted (examples below), and they have to be convertible to `System.Index`.</span></span> <span data-ttu-id="28ba8-132">Bude snížena na příslušné volání metody Factory `System.Range`.</span><span class="sxs-lookup"><span data-stu-id="28ba8-132">It will be lowered to the appropriate `System.Range` factory method call.</span></span>

<span data-ttu-id="28ba8-133">– Nahrazujeme C# gramatická pravidla pro *multiplicative_expression* následující (aby bylo možné zavést novou úroveň priority):</span><span class="sxs-lookup"><span data-stu-id="28ba8-133">-We replace the C# grammar rules for *multiplicative_expression* with the following (in order to introduce a new precedence level):</span></span>

```antlr
range_expression
    : unary_expression
    | range_expression? '..' range_expression?
    ;

multiplicative_expression
    : range_expression
    | multiplicative_expression '*' range_expression
    | multiplicative_expression '/' range_expression
    | multiplicative_expression '%' range_expression
    ;
```

<span data-ttu-id="28ba8-134">Všechny formuláře *operátoru rozsahu* mají stejnou prioritu.</span><span class="sxs-lookup"><span data-stu-id="28ba8-134">All forms of the *range operator* have the same precedence.</span></span> <span data-ttu-id="28ba8-135">Tato nová skupina priorit je nižší než *unární operátory* a vyšší než *mulitiplicative aritmetické operátory*.</span><span class="sxs-lookup"><span data-stu-id="28ba8-135">This new precedence group is lower than the *unary operators* and higher than the *mulitiplicative arithmetic operators*.</span></span>

<span data-ttu-id="28ba8-136">Voláme operátor `..`, který je *operátorem rozsahu*.</span><span class="sxs-lookup"><span data-stu-id="28ba8-136">We call the `..` operator the *range operator*.</span></span> <span data-ttu-id="28ba8-137">Předdefinovaný operátor rozsahu může být zhruba srozumitelný, aby odpovídal vyvolání předdefinovaného operátoru tohoto formuláře:</span><span class="sxs-lookup"><span data-stu-id="28ba8-137">The built-in range operator can roughly be understood to correspond to the invocation of a built-in operator of this form:</span></span>

```csharp
    System.Range operator ..(Index start = 0, Index end = ^0);
```

<span data-ttu-id="28ba8-138">Příklady:</span><span class="sxs-lookup"><span data-stu-id="28ba8-138">Examples:</span></span>

```csharp
var slice1 = list[2..^3];               // list[Range.Create(2, Index.CreateFromEnd(3))]
var slice2 = list[..^3];                // list[Range.ToEnd(Index.CreateFromEnd(3))]
var slice3 = list[2..];                 // list[Range.FromStart(2)]
var slice4 = list[..];                  // list[Range.All]

var multiDimensional = list[1..2, ..]   // list[Range.Create(1, 2), Range.All]
```

<span data-ttu-id="28ba8-139">Kromě toho by `System.Index` měl mít implicitní převod z `System.Int32`, aby se nemusely přetížit pro kombinování celých čísel a indexů přes multidimenzionální signatury.</span><span class="sxs-lookup"><span data-stu-id="28ba8-139">Moreover, `System.Index` should have an implicit conversion from `System.Int32`, in order not to need to overload for mixing integers and indexes over multi-dimensional signatures.</span></span>

## <a name="adding-index-and-range-support-to-existing-library-types"></a><span data-ttu-id="28ba8-140">Přidání podpory pro index a rozsah do existujících typů knihoven</span><span class="sxs-lookup"><span data-stu-id="28ba8-140">Adding Index and Range support to existing library types</span></span>

### <a name="implicit-index-support"></a><span data-ttu-id="28ba8-141">Podpora implicitního indexu</span><span class="sxs-lookup"><span data-stu-id="28ba8-141">Implicit Index support</span></span>

<span data-ttu-id="28ba8-142">Jazyk poskytne člen indexeru instance s jedním parametrem typu `Index` pro typy, které splňují následující kritéria:</span><span class="sxs-lookup"><span data-stu-id="28ba8-142">The language will provide an instance indexer member with a single parameter of type `Index` for types which meet the following criteria:</span></span>

- <span data-ttu-id="28ba8-143">Typ je vypočítán.</span><span class="sxs-lookup"><span data-stu-id="28ba8-143">The type is Countable.</span></span>
- <span data-ttu-id="28ba8-144">Typ má přístupný indexer instance, který jako argument přijímá jeden `int`.</span><span class="sxs-lookup"><span data-stu-id="28ba8-144">The type has an accessible instance indexer which takes a single `int` as the argument.</span></span>
- <span data-ttu-id="28ba8-145">Typ nemá přístupný indexer instancí, který jako první parametr přebírá `Index`.</span><span class="sxs-lookup"><span data-stu-id="28ba8-145">The type does not have an accessible instance indexer which takes an `Index` as the first parameter.</span></span> <span data-ttu-id="28ba8-146">`Index` musí být jediný parametr nebo zbývající parametry musí být nepovinné.</span><span class="sxs-lookup"><span data-stu-id="28ba8-146">The `Index` must be the only parameter or the remaining parameters must be optional.</span></span>

<span data-ttu-id="28ba8-147">Typ je ***vypočítán*** , pokud má vlastnost s názvem `Length` nebo `Count` s přístupným mechanismem getter a návratovým typem `int`.</span><span class="sxs-lookup"><span data-stu-id="28ba8-147">A type is ***Countable*** if it  has a property named `Length` or `Count` with an accessible getter and a return type of `int`.</span></span> <span data-ttu-id="28ba8-148">Jazyk může tuto vlastnost použít k převedení výrazu typu `Index` do `int` v místě výrazu bez nutnosti použít typ `Index` vůbec.</span><span class="sxs-lookup"><span data-stu-id="28ba8-148">The language can make use of this property to convert an expression of type `Index` into an `int` at the point of the expression without the need to use the type `Index` at all.</span></span> <span data-ttu-id="28ba8-149">V případě, že jsou přítomny `Length` i `Count`, bude preferovaná `Length`.</span><span class="sxs-lookup"><span data-stu-id="28ba8-149">In case both `Length` and `Count` are present, `Length` will be preferred.</span></span> <span data-ttu-id="28ba8-150">Pro zjednodušení předá návrh bude používat název `Length` k reprezentaci `Count` nebo `Length`.</span><span class="sxs-lookup"><span data-stu-id="28ba8-150">For simplicity going forward, the proposal will use the name `Length` to represent `Count` or `Length`.</span></span>

<span data-ttu-id="28ba8-151">U takových typů bude jazyk fungovat jako v případě, že se jedná o člena indexu formuláře `T this[Index index]` kde `T` je návratový typ indexeru založeného na `int` včetně všech `ref` poznámek ve stylu.</span><span class="sxs-lookup"><span data-stu-id="28ba8-151">For such types, the language will act as if there is an index member of the form `T this[Index index]` where `T` is the return type of the `int` based indexer including any `ref` style annotations.</span></span> <span data-ttu-id="28ba8-152">Nový člen bude mít stejné `get` a `set` členy s porovnávacím přístupným přístupností jako indexerem `int`.</span><span class="sxs-lookup"><span data-stu-id="28ba8-152">The new member will have the same `get` and `set` members with matching accessibility as the `int` indexer.</span></span> 

<span data-ttu-id="28ba8-153">Nový indexer bude implementován převodem argumentu typu `Index` do `int` a vyvoláním indexeru založeného na `int`.</span><span class="sxs-lookup"><span data-stu-id="28ba8-153">The new indexer will be implemented by converting the argument of type `Index` into an `int` and emitting a call to the `int` based indexer.</span></span> <span data-ttu-id="28ba8-154">Pro účely diskuze umožňuje použít příklad `receiver[expr]`.</span><span class="sxs-lookup"><span data-stu-id="28ba8-154">For discussion purposes, lets use the example of `receiver[expr]`.</span></span> <span data-ttu-id="28ba8-155">Převod `expr` na `int` se projeví takto:</span><span class="sxs-lookup"><span data-stu-id="28ba8-155">The conversion of `expr` to `int` will occur as follows:</span></span>

- <span data-ttu-id="28ba8-156">Pokud má argument formu `^expr2` a typ `expr2` je `int`, bude přeložen na `receiver.Length - expr2`.</span><span class="sxs-lookup"><span data-stu-id="28ba8-156">When the argument is of the form `^expr2` and the type of `expr2` is `int`, it will be translated to `receiver.Length - expr2`.</span></span>
- <span data-ttu-id="28ba8-157">V opačném případě bude převedena jako `expr.GetOffset(receiver.Length)`.</span><span class="sxs-lookup"><span data-stu-id="28ba8-157">Otherwise, it will be translated as `expr.GetOffset(receiver.Length)`.</span></span>

<span data-ttu-id="28ba8-158">To umožňuje vývojářům používat funkci `Index` u stávajících typů bez nutnosti úprav.</span><span class="sxs-lookup"><span data-stu-id="28ba8-158">This allows for developers to use the `Index` feature on existing types without the need for modification.</span></span> <span data-ttu-id="28ba8-159">Příklad:</span><span class="sxs-lookup"><span data-stu-id="28ba8-159">For example:</span></span>

``` csharp
List<char> list = ...;
var value = list[^1]; 

// Gets translated to 
var value = list[list.Count - 1]; 
```

<span data-ttu-id="28ba8-160">Výrazy `receiver` a `Length` budou podle potřeby vyloučeny, aby se zajistilo, že se všechny vedlejší účinky spustí pouze jednou.</span><span class="sxs-lookup"><span data-stu-id="28ba8-160">The `receiver` and `Length` expressions will be spilled as appropriate to ensure any side effects are only executed once.</span></span> <span data-ttu-id="28ba8-161">Příklad:</span><span class="sxs-lookup"><span data-stu-id="28ba8-161">For example:</span></span>

``` csharp
class Collection {
    private int[] _array = new[] { 1, 2, 3 };

    int Length {
        get {
            Console.Write("Length ");
            return _array.Length;
        }
    }

    int this[int index] => _array[index];
}

class SideEffect {
    Collection Get() {
        Console.Write("Get ");
        return new Collection();
    }

    void Use() { 
        int i = Get()[^1];
        Console.WriteLine(i);
    }
}
```

<span data-ttu-id="28ba8-162">Tento kód vytiskne "Get Length 3".</span><span class="sxs-lookup"><span data-stu-id="28ba8-162">This code will print "Get Length 3".</span></span>

### <a name="implicit-range-support"></a><span data-ttu-id="28ba8-163">Podpora implicitního rozsahu</span><span class="sxs-lookup"><span data-stu-id="28ba8-163">Implicit Range support</span></span>

<span data-ttu-id="28ba8-164">Jazyk poskytne člen indexeru instance s jedním parametrem typu `Range` pro typy, které splňují následující kritéria:</span><span class="sxs-lookup"><span data-stu-id="28ba8-164">The language will provide an instance indexer member with a single parameter of type `Range` for types which meet the following criteria:</span></span>

- <span data-ttu-id="28ba8-165">Typ je vypočítán.</span><span class="sxs-lookup"><span data-stu-id="28ba8-165">The type is Countable.</span></span>
- <span data-ttu-id="28ba8-166">Typ má přístupný člen s názvem `Slice`, který má dva parametry typu `int`.</span><span class="sxs-lookup"><span data-stu-id="28ba8-166">The type has an accessible member named `Slice` which has two parameters of type `int`.</span></span>
- <span data-ttu-id="28ba8-167">Typ nemá indexer instancí, který jako první parametr přebírá jeden `Range`.</span><span class="sxs-lookup"><span data-stu-id="28ba8-167">The type does not have an instance indexer which takes a single `Range` as the first parameter.</span></span> <span data-ttu-id="28ba8-168">`Range` musí být jediný parametr nebo zbývající parametry musí být nepovinné.</span><span class="sxs-lookup"><span data-stu-id="28ba8-168">The `Range` must be the only parameter or the remaining parameters must be optional.</span></span>

<span data-ttu-id="28ba8-169">U takových typů se jazyk sváže, jako by byl člen indexu formuláře `T this[Range range]`, kde `T` je návratový typ `Slice` metody, včetně všech `ref` poznámek ve stylu.</span><span class="sxs-lookup"><span data-stu-id="28ba8-169">For such types, the language will bind as if there is an index member of the form `T this[Range range]` where `T` is the return type of the `Slice` method including any `ref` style annotations.</span></span> <span data-ttu-id="28ba8-170">Nový člen také bude mít k dispozici přístupnost s `Slice`.</span><span class="sxs-lookup"><span data-stu-id="28ba8-170">The new member will also have matching accessibility with `Slice`.</span></span> 

<span data-ttu-id="28ba8-171">Pokud je indexer založený na `Range` vázaný na výraz s názvem `receiver`, bude snížen převodem výrazu `Range` na dvě hodnoty, které jsou poté předány metodě `Slice`.</span><span class="sxs-lookup"><span data-stu-id="28ba8-171">When the `Range` based indexer is bound on an expression named `receiver`, it will be lowered by converting the `Range` expression into two values that are then passed to the `Slice` method.</span></span> <span data-ttu-id="28ba8-172">Pro účely diskuze umožňuje použít příklad `receiver[expr]`.</span><span class="sxs-lookup"><span data-stu-id="28ba8-172">For discussion purposes, lets use the example of `receiver[expr]`.</span></span>

<span data-ttu-id="28ba8-173">První argument `Slice` bude získán převodem typovaného výrazu následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="28ba8-173">The first argument of `Slice` will be obtained by converting the typed expression in the following way:</span></span>

- <span data-ttu-id="28ba8-174">Pokud má `expr` `expr1..expr2` formuláře (kde se `expr2` může vynechat) a `expr1` má typ `int`, bude vygenerován jako `expr1`.</span><span class="sxs-lookup"><span data-stu-id="28ba8-174">When `expr` is of the form `expr1..expr2` (where `expr2` can be omitted) and `expr1` has type `int`, then it will be emitted as `expr1`.</span></span>
- <span data-ttu-id="28ba8-175">Pokud má `expr` `^expr1..expr2` (kde je možné vynechání `expr2`), bude vygenerována jako `receiver.Length - expr1`.</span><span class="sxs-lookup"><span data-stu-id="28ba8-175">When `expr` is of the form `^expr1..expr2` (where `expr2` can be omitted), then it will be emitted as `receiver.Length - expr1`.</span></span>
- <span data-ttu-id="28ba8-176">Pokud má `expr` `..expr2` (kde je možné vynechání `expr2`), bude vygenerována jako `0`.</span><span class="sxs-lookup"><span data-stu-id="28ba8-176">When `expr` is of the form `..expr2` (where `expr2` can be omitted), then it will be emitted as `0`.</span></span>
- <span data-ttu-id="28ba8-177">V opačném případě bude vygenerována jako `expr.Start.GetOffset(receiver.Length)`.</span><span class="sxs-lookup"><span data-stu-id="28ba8-177">Otherwise, it will be emitted as `expr.Start.GetOffset(receiver.Length)`.</span></span>

<span data-ttu-id="28ba8-178">Tato hodnota se znovu použije při výpočtu druhého argumentu `Slice`.</span><span class="sxs-lookup"><span data-stu-id="28ba8-178">This value will be re-used in the calculation of the second `Slice` argument.</span></span> <span data-ttu-id="28ba8-179">V takovém případě se bude označovat jako `start`.</span><span class="sxs-lookup"><span data-stu-id="28ba8-179">When doing so it will be referred to as `start`.</span></span> <span data-ttu-id="28ba8-180">Druhý argument `Slice` bude získán převodem výrazu zadaného rozsahu následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="28ba8-180">The second argument of `Slice` will be obtained by converting the range typed expression in the following way:</span></span>

- <span data-ttu-id="28ba8-181">Pokud má `expr` `expr1..expr2` formuláře (kde se `expr1` může vynechat) a `expr2` má typ `int`, bude vygenerován jako `expr2 - start`.</span><span class="sxs-lookup"><span data-stu-id="28ba8-181">When `expr` is of the form `expr1..expr2` (where `expr1` can be omitted) and `expr2` has type `int`, then it will be emitted as `expr2 - start`.</span></span>
- <span data-ttu-id="28ba8-182">Pokud má `expr` `expr1..^expr2` (kde je možné vynechání `expr1`), bude vygenerována jako `(receiver.Length - expr2) - start`.</span><span class="sxs-lookup"><span data-stu-id="28ba8-182">When `expr` is of the form `expr1..^expr2` (where `expr1` can be omitted), then it will be emitted as `(receiver.Length - expr2) - start`.</span></span>
- <span data-ttu-id="28ba8-183">Pokud má `expr` `expr1..` (kde je možné vynechání `expr1`), bude vygenerována jako `receiver.Length - start`.</span><span class="sxs-lookup"><span data-stu-id="28ba8-183">When `expr` is of the form `expr1..` (where `expr1` can be omitted), then it will be emitted as `receiver.Length - start`.</span></span>
- <span data-ttu-id="28ba8-184">V opačném případě bude vygenerována jako `expr.End.GetOffset(receiver.Length) - start`.</span><span class="sxs-lookup"><span data-stu-id="28ba8-184">Otherwise, it will be emitted as `expr.End.GetOffset(receiver.Length) - start`.</span></span>

<span data-ttu-id="28ba8-185">`receiver`, `Length` a `expr` výrazy budou přecházet podle potřeby, aby se zajistilo, že se všechny vedlejší účinky spustí pouze jednou.</span><span class="sxs-lookup"><span data-stu-id="28ba8-185">The `receiver`, `Length` and `expr` expressions will be spilled as appropriate to ensure any side effects are only executed once.</span></span> <span data-ttu-id="28ba8-186">Příklad:</span><span class="sxs-lookup"><span data-stu-id="28ba8-186">For example:</span></span>

``` csharp
class Collection {
    private int[] _array = new[] { 1, 2, 3 };

    int Length {
        get {
            Console.Write("Length ");
            return _array.Length;
        }
    }

    int[] Slice(int start, int length) { 
        var slice = new int[length];
        Array.Copy(_array, start, slice, 0, length);
        return slice;
    }
}

class SideEffect {
    Collection Get() {
        Console.Write("Get ");
        return new Collection();
    }

    void Use() { 
        var array = Get()[0..2];
        Console.WriteLine(array.length);
    }
}
```

<span data-ttu-id="28ba8-187">Tento kód vytiskne "Get Length 2".</span><span class="sxs-lookup"><span data-stu-id="28ba8-187">This code will print "Get Length 2".</span></span>

<span data-ttu-id="28ba8-188">V jazyce se zvláštními případy rozumí následující známé typy:</span><span class="sxs-lookup"><span data-stu-id="28ba8-188">The language will special case the following known types:</span></span> 

- <span data-ttu-id="28ba8-189">`string`: místo `Slice`se použije `Substring` metody.</span><span class="sxs-lookup"><span data-stu-id="28ba8-189">`string`: the method `Substring` will be used instead of `Slice`.</span></span>
- <span data-ttu-id="28ba8-190">`array`: místo `Slice`se použije `System.Reflection.CompilerServices.GetSubArray` metody.</span><span class="sxs-lookup"><span data-stu-id="28ba8-190">`array`: the method `System.Reflection.CompilerServices.GetSubArray` will be used instead of `Slice`.</span></span>

## <a name="alternatives"></a><span data-ttu-id="28ba8-191">Alternativy</span><span class="sxs-lookup"><span data-stu-id="28ba8-191">Alternatives</span></span>

<span data-ttu-id="28ba8-192">Nové operátory (`^` a `..`) jsou syntaktický cukr.</span><span class="sxs-lookup"><span data-stu-id="28ba8-192">The new operators (`^` and `..`) are syntactic sugar.</span></span> <span data-ttu-id="28ba8-193">Tato funkce může být implementována explicitními voláními metod `System.Index` a `System.Range` továrnou, ale bude mít za následek mnohem více často používaný kód a prostředí bude neintuitivní.</span><span class="sxs-lookup"><span data-stu-id="28ba8-193">The functionality can be implemented by explicit calls to `System.Index` and `System.Range` factory methods, but it will result in a lot more boilerplate code, and the experience will be unintuitive.</span></span>

## <a name="il-representation"></a><span data-ttu-id="28ba8-194">Reprezentace IL</span><span class="sxs-lookup"><span data-stu-id="28ba8-194">IL Representation</span></span>

<span data-ttu-id="28ba8-195">Tyto dva operátory budou sníženy na pravidelná volání indexerů/metod bez změny v následných vrstvách kompilátoru.</span><span class="sxs-lookup"><span data-stu-id="28ba8-195">These two operators will be lowered to regular indexer/method calls, with no change in subsequent compiler layers.</span></span>

## <a name="runtime-behavior"></a><span data-ttu-id="28ba8-196">Chování za běhu</span><span class="sxs-lookup"><span data-stu-id="28ba8-196">Runtime behavior</span></span>

- <span data-ttu-id="28ba8-197">Kompilátor může optimalizovat indexery pro předdefinované typy, jako jsou pole a řetězce, a snížit indexování na příslušné existující metody.</span><span class="sxs-lookup"><span data-stu-id="28ba8-197">Compiler can optimize indexers for built-in types like arrays and strings, and lower the indexing to the appropriate existing methods.</span></span>
- <span data-ttu-id="28ba8-198">`System.Index` bude vyvolána, pokud je vytvořena se zápornou hodnotou.</span><span class="sxs-lookup"><span data-stu-id="28ba8-198">`System.Index` will throw if constructed with a negative value.</span></span>
- <span data-ttu-id="28ba8-199">`^0` nevyvolává, ale překládá se na délku kolekce/výčtu, do které je předáno.</span><span class="sxs-lookup"><span data-stu-id="28ba8-199">`^0` does not throw, but it translates to the length of the collection/enumerable it is supplied to.</span></span>
- <span data-ttu-id="28ba8-200">`Range.All` je sémanticky ekvivalentní `0..^0`a lze je rozkonstruovat na tyto indexy.</span><span class="sxs-lookup"><span data-stu-id="28ba8-200">`Range.All` is semantically equivalent to `0..^0`, and can be deconstructed to these indices.</span></span>

## <a name="considerations"></a><span data-ttu-id="28ba8-201">Požadavky</span><span class="sxs-lookup"><span data-stu-id="28ba8-201">Considerations</span></span>

### <a name="detect-indexable-based-on-icollection"></a><span data-ttu-id="28ba8-202">Zjistit index založený na rozhraní ICollection</span><span class="sxs-lookup"><span data-stu-id="28ba8-202">Detect Indexable based on ICollection</span></span>

<span data-ttu-id="28ba8-203">Inspiraci pro toto chování byly inicializátory kolekce.</span><span class="sxs-lookup"><span data-stu-id="28ba8-203">The inspiration for this behavior was collection initializers.</span></span> <span data-ttu-id="28ba8-204">Použití struktury typu k vyjádření, že se rozhodl do funkce.</span><span class="sxs-lookup"><span data-stu-id="28ba8-204">Using the structure of a type to convey that it had opted into a feature.</span></span> <span data-ttu-id="28ba8-205">V případě typů inicializátorů kolekce se může přihlásit k funkci implementací rozhraní `IEnumerable` (neobecné).</span><span class="sxs-lookup"><span data-stu-id="28ba8-205">In the case of collection initializers types can opt into the feature by implementing the interface `IEnumerable` (non generic).</span></span>

<span data-ttu-id="28ba8-206">Tento návrh původně vyžadoval, aby tyto typy implementovaly `ICollection`, aby se daly kvalifikovat jako indexovaná.</span><span class="sxs-lookup"><span data-stu-id="28ba8-206">This proposal initially required that types implement `ICollection` in order to qualify as Indexable.</span></span> <span data-ttu-id="28ba8-207">V takovém případě se vyžaduje několik speciálních případů:</span><span class="sxs-lookup"><span data-stu-id="28ba8-207">That required a number of special cases though:</span></span>

- <span data-ttu-id="28ba8-208">`ref struct`: tyto typy nemohou implementovat rozhraní, jako `Span<T>` jsou ideální pro podporu indexů a rozsahů.</span><span class="sxs-lookup"><span data-stu-id="28ba8-208">`ref struct`: these cannot implement interfaces yet types like `Span<T>` are ideal for index / range support.</span></span> 
- <span data-ttu-id="28ba8-209">`string`: neimplementuje `ICollection` a přidávání tohoto `interface` má velké náklady.</span><span class="sxs-lookup"><span data-stu-id="28ba8-209">`string`: does not implement `ICollection` and adding that `interface` has a large cost.</span></span>

<span data-ttu-id="28ba8-210">To znamená, že se už vyžaduje podpora speciálních typů klíčů.</span><span class="sxs-lookup"><span data-stu-id="28ba8-210">This means to support key types special casing is already needed.</span></span> <span data-ttu-id="28ba8-211">Speciální velká a malá písmena `string`a jsou méně zajímavá, protože jazyk funguje v jiných oblastech (`foreach` nižších, konstant atd.). Speciální velká a malá písmena `ref struct` jsou více týkající se toho, že se jedná o zvláštní velká a malá písmena celé třídy typů.</span><span class="sxs-lookup"><span data-stu-id="28ba8-211">The special casing of `string` is less interesting as the language does this in other areas (`foreach` lowering, constants, etc ...). The special casing of `ref struct` is more concerning as it's special casing an entire class of types.</span></span> <span data-ttu-id="28ba8-212">Jsou označeny jako Indexovaelné, pokud jednoduše obsahují vlastnost s názvem `Count` s návratovým typem `int`.</span><span class="sxs-lookup"><span data-stu-id="28ba8-212">They get labeled as Indexable if they simply have a property named `Count` with a return type of `int`.</span></span> 

<span data-ttu-id="28ba8-213">Po zvážení návrhu byl tento návrh normalizován, aby řekněme, že jakýkoli typ, který má vlastnost `Count` / `Length` s návratovým typem `int` je indexovaná.</span><span class="sxs-lookup"><span data-stu-id="28ba8-213">After consideration the design was normalized to say that any type which has a property `Count` / `Length` with a return type of `int` is Indexable.</span></span> <span data-ttu-id="28ba8-214">Tím se odstraní všechna speciální velká a malá písmena, a to i pro `string` a pole.</span><span class="sxs-lookup"><span data-stu-id="28ba8-214">That removes all special casing, even for `string` and arrays.</span></span>

### <a name="detect-just-count"></a><span data-ttu-id="28ba8-215">Zjistit počet pouze</span><span class="sxs-lookup"><span data-stu-id="28ba8-215">Detect just Count</span></span>

<span data-ttu-id="28ba8-216">Zjišťování názvů vlastností `Count` nebo `Length` ztěžuje návrh bitové konstrukce.</span><span class="sxs-lookup"><span data-stu-id="28ba8-216">Detecting on the property names `Count` or `Length` does complicate the design a bit.</span></span> <span data-ttu-id="28ba8-217">Výběr pouze jedné pro standardizaci, i když není dostačující, aby se vyloučil velký počet typů:</span><span class="sxs-lookup"><span data-stu-id="28ba8-217">Picking just one to standardize though is not sufficient as it ends up excluding a large number of types:</span></span>

- <span data-ttu-id="28ba8-218">Použít `Length`: vyloučí v podstatě většinu každé kolekce v System. Collections a subnamespaces.</span><span class="sxs-lookup"><span data-stu-id="28ba8-218">Use `Length`: excludes pretty much every collection in System.Collections and sub-namespaces.</span></span> <span data-ttu-id="28ba8-219">Ty jsou obvykle odvozeny od `ICollection` a proto upřednostňují `Count` nad délkou.</span><span class="sxs-lookup"><span data-stu-id="28ba8-219">Those tend to derive from `ICollection` and hence prefer `Count` over length.</span></span>
- <span data-ttu-id="28ba8-220">Use `Count`: vyloučí `string`, pole, `Span<T>` a nejvíce `ref struct` založených typů.</span><span class="sxs-lookup"><span data-stu-id="28ba8-220">Use `Count`: excludes `string`, arrays, `Span<T>` and most `ref struct` based types</span></span>

<span data-ttu-id="28ba8-221">Nadbytečné komplikace při prvotní detekci indexovaných typů jsou v jiných aspektech převáženy svými zjednodušeními.</span><span class="sxs-lookup"><span data-stu-id="28ba8-221">The extra complication on the initial detection of Indexable types is outweighed by its simplification in other aspects.</span></span>

### <a name="choice-of-slice-as-a-name"></a><span data-ttu-id="28ba8-222">Volba řezu jako názvu</span><span class="sxs-lookup"><span data-stu-id="28ba8-222">Choice of Slice as a name</span></span>

<span data-ttu-id="28ba8-223">Název `Slice` byl vybrán jako standardní název pro operace stylu řezu v rozhraní .NET.</span><span class="sxs-lookup"><span data-stu-id="28ba8-223">The name `Slice` was chosen as it's the de-facto standard name for slice style operations in .NET.</span></span> <span data-ttu-id="28ba8-224">Počínaje netcoreapp 2.1 jsou všechny typy řezů rozsahu použity pro operace dělení `Slice` názvu.</span><span class="sxs-lookup"><span data-stu-id="28ba8-224">Starting with netcoreapp2.1 all span style types use the name `Slice` for slicing operations.</span></span> <span data-ttu-id="28ba8-225">Před netcoreapp 2.1 zatím neexistují žádné příklady vytváření řezů, které by bylo možné v případě příkladu najít.</span><span class="sxs-lookup"><span data-stu-id="28ba8-225">Prior to netcoreapp2.1 there really aren't any examples of slicing to look to for an example.</span></span> <span data-ttu-id="28ba8-226">Typy jako `List<T>`, `ArraySegment<T>`, `SortedList<T>` by byly ideální pro vytváření řezů, ale koncept neexistoval při přidání typů.</span><span class="sxs-lookup"><span data-stu-id="28ba8-226">Types like `List<T>`, `ArraySegment<T>`, `SortedList<T>` would've been ideal for slicing but the concept didn't exist when types were added.</span></span> 

<span data-ttu-id="28ba8-227">Proto je `Slice` jediným příkladem, který byl vybrán jako název.</span><span class="sxs-lookup"><span data-stu-id="28ba8-227">Thus, `Slice` being the sole example, it was chosen as the name.</span></span>

### <a name="index-target-type-conversion"></a><span data-ttu-id="28ba8-228">Převod cílového typu indexu</span><span class="sxs-lookup"><span data-stu-id="28ba8-228">Index target type conversion</span></span>

<span data-ttu-id="28ba8-229">Dalším způsobem, jak zobrazit transformaci `Index` ve výrazu indexeru, je jako převod cílového typu.</span><span class="sxs-lookup"><span data-stu-id="28ba8-229">Another way to view the `Index` transformation in an indexer expression is as a target type conversion.</span></span> <span data-ttu-id="28ba8-230">Místo vazby, jako kdyby byl členem formuláře `return_type this[Index]`, jazyk místo toho přiřadí cílový typový převod do `int`.</span><span class="sxs-lookup"><span data-stu-id="28ba8-230">Instead of binding as if there is a member of the form `return_type this[Index]`, the language instead assigns a target typed conversion to `int`.</span></span> 

<span data-ttu-id="28ba8-231">Tento koncept se dá zobecnit na všechny členské přístupy na základě počtu typů.</span><span class="sxs-lookup"><span data-stu-id="28ba8-231">This concept could be generalized to all member access on Countable types.</span></span> <span data-ttu-id="28ba8-232">Vždy, když je výraz s typem `Index` použit jako argument pro vyvolání člena instance a přijímač je možné vyhodnotit, bude mít výraz převod cílového typu na hodnotu `int`.</span><span class="sxs-lookup"><span data-stu-id="28ba8-232">Whenever an expression with type `Index` is used as an argument to an instance member invocation and the receiver is Countable then the expression will have a target type conversion to `int`.</span></span> <span data-ttu-id="28ba8-233">Vyvolání členů, která se vztahují k tomuto převodu, zahrnují metody, indexery, vlastnosti, metody rozšíření atd... Pouze konstruktory jsou vyloučeny, protože nemají žádného přijímače.</span><span class="sxs-lookup"><span data-stu-id="28ba8-233">The member invocations applicable for this conversion include methods, indexers, properties, extension methods, etc ... Only constructors are excluded as they have no receiver.</span></span> 

<span data-ttu-id="28ba8-234">Převod cílového typu se implementuje následovně pro libovolný výraz, který má typ `Index`.</span><span class="sxs-lookup"><span data-stu-id="28ba8-234">The target type conversion will be implemented as follows for any expression which has a type of `Index`.</span></span> <span data-ttu-id="28ba8-235">Pro účely diskuze můžete použít příklad `receiver[expr]`:</span><span class="sxs-lookup"><span data-stu-id="28ba8-235">For discussion purposes lets use the example of `receiver[expr]`:</span></span>

- <span data-ttu-id="28ba8-236">Pokud má `expr` `^expr2` a typ `expr2` `int`, bude přeložen na `receiver.Length - expr2`.</span><span class="sxs-lookup"><span data-stu-id="28ba8-236">When `expr` is of the form `^expr2` and the type of `expr2` is `int`, it will be translated to `receiver.Length - expr2`.</span></span>
- <span data-ttu-id="28ba8-237">V opačném případě bude převedena jako `expr.GetOffset(receiver.Length)`.</span><span class="sxs-lookup"><span data-stu-id="28ba8-237">Otherwise, it will be translated as `expr.GetOffset(receiver.Length)`.</span></span>

<span data-ttu-id="28ba8-238">Výrazy `receiver` a `Length` budou podle potřeby vyloučeny, aby se zajistilo, že se všechny vedlejší účinky spustí pouze jednou.</span><span class="sxs-lookup"><span data-stu-id="28ba8-238">The `receiver` and `Length` expressions will be spilled as appropriate to ensure any side effects are only executed once.</span></span> <span data-ttu-id="28ba8-239">Příklad:</span><span class="sxs-lookup"><span data-stu-id="28ba8-239">For example:</span></span>

``` csharp
class Collection {
    private int[] _array = new[] { 1, 2, 3 };

    int Length {
        get {
            Console.Write("Length ");
            return _array.Length;
        }
    }

    int GetAt(int index) => _array[index];
}

class SideEffect {
    Collection Get() {
        Console.Write("Get ");
        return new Collection();
    }

    void Use() { 
        int i = Get().GetAt(^1);
        Console.WriteLine(i);
    }
}
```

<span data-ttu-id="28ba8-240">Tento kód vytiskne "Get Length 3".</span><span class="sxs-lookup"><span data-stu-id="28ba8-240">This code will print "Get Length 3".</span></span> 

<span data-ttu-id="28ba8-241">Tato funkce bude prospěšná pro každého člena, který má parametr, který představoval index.</span><span class="sxs-lookup"><span data-stu-id="28ba8-241">This feature would be beneficial to any member which had a parameter that represented an index.</span></span> <span data-ttu-id="28ba8-242">Například `List<T>.InsertAt`.</span><span class="sxs-lookup"><span data-stu-id="28ba8-242">For example `List<T>.InsertAt`.</span></span> <span data-ttu-id="28ba8-243">To má taky možnost záměny, protože jazyk neposkytuje žádné pokyny k tomu, jestli je výraz určený pro indexování.</span><span class="sxs-lookup"><span data-stu-id="28ba8-243">This also has the potential for confusion as the language can't give any guidance as to whether or not an expression is meant for indexing.</span></span> <span data-ttu-id="28ba8-244">Vše může být převedeno libovolný výraz `Index`, aby `int` při vyvolání člena na základě typu Count.</span><span class="sxs-lookup"><span data-stu-id="28ba8-244">All it can do is convert any `Index` expression to `int` when invoking a member on a Countable type.</span></span> 

<span data-ttu-id="28ba8-245">Podléhající</span><span class="sxs-lookup"><span data-stu-id="28ba8-245">Restrictions:</span></span>

- <span data-ttu-id="28ba8-246">Tento převod je použitelný pouze v případě, že výraz typu `Index` je přímo argumentem člena.</span><span class="sxs-lookup"><span data-stu-id="28ba8-246">This conversion is only applicable when the expression with type `Index` is directly an argument to the member.</span></span> <span data-ttu-id="28ba8-247">Neplatí to pro žádné vnořené výrazy.</span><span class="sxs-lookup"><span data-stu-id="28ba8-247">It would not apply to any nested expressions.</span></span>

## <a name="decisions-made-during-implementation"></a><span data-ttu-id="28ba8-248">Rozhodnutí učiněná během implementace</span><span class="sxs-lookup"><span data-stu-id="28ba8-248">Decisions made during implementation</span></span>

- <span data-ttu-id="28ba8-249">Všichni členové ve vzoru musí být členy instance.</span><span class="sxs-lookup"><span data-stu-id="28ba8-249">All members in the pattern must be instance members</span></span>
- <span data-ttu-id="28ba8-250">Pokud se Metoda Length najde, ale má nesprávný návratový typ, pokračujte v hledání počtu.</span><span class="sxs-lookup"><span data-stu-id="28ba8-250">If a Length method is found but it has the wrong return type, continue looking for Count</span></span>
- <span data-ttu-id="28ba8-251">Indexer použitý pro vzor indexu musí mít právě jeden parametr int.</span><span class="sxs-lookup"><span data-stu-id="28ba8-251">The indexer used for the Index pattern must have exactly one int parameter</span></span>
- <span data-ttu-id="28ba8-252">Metoda řezu použitá pro vzorek rozsahu musí mít přesně dva parametry int.</span><span class="sxs-lookup"><span data-stu-id="28ba8-252">The Slice method used for the Range pattern must have exactly two int parameters</span></span>
- <span data-ttu-id="28ba8-253">Při hledání členů vzorů hledáme původní definice, nikoli konstruované členy.</span><span class="sxs-lookup"><span data-stu-id="28ba8-253">When looking for the pattern members, we look for original definitions, not constructed members</span></span>

## <a name="design-meetings"></a><span data-ttu-id="28ba8-254">Schůzky pro návrh</span><span class="sxs-lookup"><span data-stu-id="28ba8-254">Design Meetings</span></span>

- https://github.com/dotnet/csharplang/blob/master/meetings/2019/LDM-2019-04-01.md

## <a name="questions"></a><span data-ttu-id="28ba8-255">Otázky</span><span class="sxs-lookup"><span data-stu-id="28ba8-255">Questions</span></span>
