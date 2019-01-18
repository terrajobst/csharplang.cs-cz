---
ms.openlocfilehash: 155c1beecddfdfcce2e7948bcb8d6b80428fbd7a
ms.sourcegitcommit: 3fc033b6e98ed7ecdf46a85c79b00a3a3ddcf963
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/18/2019
ms.locfileid: "47229911"
---
# <a name="arrays"></a><span data-ttu-id="9f21d-101">Pole</span><span class="sxs-lookup"><span data-stu-id="9f21d-101">Arrays</span></span>

<span data-ttu-id="9f21d-102">Pole je datová struktura, která obsahuje několik proměnných, které jsou přístupné prostřednictvím vypočítané indexy.</span><span class="sxs-lookup"><span data-stu-id="9f21d-102">An array is a data structure that contains a number of variables which are accessed through computed indices.</span></span> <span data-ttu-id="9f21d-103">Proměnné, které jsou obsaženy v poli, také nazývají prvky pole, jsou všechny stejného typu a tento typ se nazývá typ prvku pole.</span><span class="sxs-lookup"><span data-stu-id="9f21d-103">The variables contained in an array, also called the elements of the array, are all of the same type, and this type is called the element type of the array.</span></span>

<span data-ttu-id="9f21d-104">Pole má řazení, která určuje počet indexů, které jsou přidružené k každý prvek pole.</span><span class="sxs-lookup"><span data-stu-id="9f21d-104">An array has a rank which determines the number of indices associated with each array element.</span></span> <span data-ttu-id="9f21d-105">Rozměr pole se také označuje jako rozměry pole.</span><span class="sxs-lookup"><span data-stu-id="9f21d-105">The rank of an array is also referred to as the dimensions of the array.</span></span> <span data-ttu-id="9f21d-106">Pole s rozměrem jedna se nazývá ***jednorozměrné pole***.</span><span class="sxs-lookup"><span data-stu-id="9f21d-106">An array with a rank of one is called a ***single-dimensional array***.</span></span> <span data-ttu-id="9f21d-107">Pole s rozměrem větší než jedna se nazývá ***vícerozměrné pole***.</span><span class="sxs-lookup"><span data-stu-id="9f21d-107">An array with a rank greater than one is called a ***multi-dimensional array***.</span></span> <span data-ttu-id="9f21d-108">Konkrétní velikosti vícerozměrná pole jsou často označovány jako dvourozměrné pole, trojrozměrného pole a tak dále.</span><span class="sxs-lookup"><span data-stu-id="9f21d-108">Specific sized multi-dimensional arrays are often referred to as two-dimensional arrays, three-dimensional arrays, and so on.</span></span>

<span data-ttu-id="9f21d-109">Každé pole má přidružené délky, což je je celé číslo větší než nebo rovna nule.</span><span class="sxs-lookup"><span data-stu-id="9f21d-109">Each dimension of an array has an associated length which is an integral number greater than or equal to zero.</span></span> <span data-ttu-id="9f21d-110">Délky dimenzí nejsou součástí typu pole, ale místo toho jsou vytvořeny při vytvoření instance typu pole v době běhu.</span><span class="sxs-lookup"><span data-stu-id="9f21d-110">The dimension lengths are not part of the type of the array, but rather are established when an instance of the array type is created at run-time.</span></span> <span data-ttu-id="9f21d-111">Délka dimenze určuje platný rozsah indexů pro daná dimenze: Pro dimenzi délky `N`, musí být v rozsahu indexy `0` k `N - 1` (včetně).</span><span class="sxs-lookup"><span data-stu-id="9f21d-111">The length of a dimension determines the valid range of indices for that dimension: For a dimension of length `N`, indices can range from `0` to `N - 1` inclusive.</span></span> <span data-ttu-id="9f21d-112">Celkový počet prvků v poli je produktem délek každé dimenze v poli.</span><span class="sxs-lookup"><span data-stu-id="9f21d-112">The total number of elements in an array is the product of the lengths of each dimension in the array.</span></span> <span data-ttu-id="9f21d-113">Pokud jeden nebo více dimenzí pole mít nulovou délku, se říká, že pole bylo prázdné.</span><span class="sxs-lookup"><span data-stu-id="9f21d-113">If one or more of the dimensions of an array have a length of zero, the array is said to be empty.</span></span>

<span data-ttu-id="9f21d-114">Typ elementu pole může být libovolného typu, včetně typu pole.</span><span class="sxs-lookup"><span data-stu-id="9f21d-114">The element type of an array can be any type, including an array type.</span></span>

## <a name="array-types"></a><span data-ttu-id="9f21d-115">Typy polí</span><span class="sxs-lookup"><span data-stu-id="9f21d-115">Array types</span></span>

<span data-ttu-id="9f21d-116">Typ pole je zapsán jako *non_array_type* následovaného jednou nebo více *rank_specifier*s:</span><span class="sxs-lookup"><span data-stu-id="9f21d-116">An array type is written as a *non_array_type* followed by one or more *rank_specifier*s:</span></span>

```antlr
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
```

<span data-ttu-id="9f21d-117">A *non_array_type* libovolnou *typ* , který je sám *array_type*.</span><span class="sxs-lookup"><span data-stu-id="9f21d-117">A *non_array_type* is any *type* that is not itself an *array_type*.</span></span>

<span data-ttu-id="9f21d-118">Řád objektu typu pole je dán první *rank_specifier* v *array_type*: A *rank_specifier* znamená, že pole je pole s rozměrem jednu plus počet "`,`" tokeny v *rank_specifier*.</span><span class="sxs-lookup"><span data-stu-id="9f21d-118">The rank of an array type is given by the leftmost *rank_specifier* in the *array_type*: A *rank_specifier* indicates that the array is an array with a rank of one plus the number of "`,`" tokens in the *rank_specifier*.</span></span>

<span data-ttu-id="9f21d-119">Typ prvku typu pole je typ, který je výsledkem odstranění první *rank_specifier*:</span><span class="sxs-lookup"><span data-stu-id="9f21d-119">The element type of an array type is the type that results from deleting the leftmost *rank_specifier*:</span></span>

*  <span data-ttu-id="9f21d-120">Typ pole formuláře `T[R]` je pole s pořadím `R` a typ bez pole elementu `T`.</span><span class="sxs-lookup"><span data-stu-id="9f21d-120">An array type of the form `T[R]` is an array with rank `R` and a non-array element type `T`.</span></span>
*  <span data-ttu-id="9f21d-121">Typ pole formuláře `T[R][R1]...[Rn]` je pole s pořadím `R` a typ elementu `T[R1]...[Rn]`.</span><span class="sxs-lookup"><span data-stu-id="9f21d-121">An array type of the form `T[R][R1]...[Rn]` is an array with rank `R` and an element type `T[R1]...[Rn]`.</span></span>

<span data-ttu-id="9f21d-122">V důsledku toho *rank_specifier*s se čtou zleva doprava před posledním prvkem mimo pole typu.</span><span class="sxs-lookup"><span data-stu-id="9f21d-122">In effect, the *rank_specifier*s are read from left to right before the final non-array element type.</span></span> <span data-ttu-id="9f21d-123">Typ `int[][,,][,]` je jednorozměrné pole trojrozměrného pole dvojrozměrné pole `int`.</span><span class="sxs-lookup"><span data-stu-id="9f21d-123">The type `int[][,,][,]` is a single-dimensional array of three-dimensional arrays of two-dimensional arrays of `int`.</span></span>

<span data-ttu-id="9f21d-124">V době běhu, může být hodnota typu pole `null` nebo odkaz na instanci daného typu pole.</span><span class="sxs-lookup"><span data-stu-id="9f21d-124">At run-time, a value of an array type can be `null` or a reference to an instance of that array type.</span></span>

### <a name="the-systemarray-type"></a><span data-ttu-id="9f21d-125">Typ System.Array</span><span class="sxs-lookup"><span data-stu-id="9f21d-125">The System.Array type</span></span>

<span data-ttu-id="9f21d-126">Typ `System.Array` je abstraktní základní typ pro všechny typy polí.</span><span class="sxs-lookup"><span data-stu-id="9f21d-126">The type `System.Array` is the abstract base type of all array types.</span></span> <span data-ttu-id="9f21d-127">Implicitní referenční převod ([odkaz na implicitní převody](conversions.md#implicit-reference-conversions)) existuje z libovolného typu pole k `System.Array`a převodu explicitní odkaz ([odkaz na explicitní převody](conversions.md#explicit-reference-conversions)) existuje z `System.Array` na libovolný typ pole.</span><span class="sxs-lookup"><span data-stu-id="9f21d-127">An implicit reference conversion ([Implicit reference conversions](conversions.md#implicit-reference-conversions)) exists from any array type to `System.Array`, and an explicit reference conversion ([Explicit reference conversions](conversions.md#explicit-reference-conversions)) exists from `System.Array` to any array type.</span></span> <span data-ttu-id="9f21d-128">Všimněte si, že `System.Array` není samotné *array_type*.</span><span class="sxs-lookup"><span data-stu-id="9f21d-128">Note that `System.Array` is not itself an *array_type*.</span></span> <span data-ttu-id="9f21d-129">Místo toho je *class_type* z kterého jsou všechny *array_type*s jsou odvozeny.</span><span class="sxs-lookup"><span data-stu-id="9f21d-129">Rather, it is a *class_type* from which all *array_type*s are derived.</span></span>

<span data-ttu-id="9f21d-130">V době běhu, hodnota typu `System.Array` může být `null` nebo odkaz na instanci typu pole.</span><span class="sxs-lookup"><span data-stu-id="9f21d-130">At run-time, a value of type `System.Array` can be `null` or a reference to an instance of any array type.</span></span>

### <a name="arrays-and-the-generic-ilist-interface"></a><span data-ttu-id="9f21d-131">Pole a její obecná rozhraní IList</span><span class="sxs-lookup"><span data-stu-id="9f21d-131">Arrays and the generic IList interface</span></span>

<span data-ttu-id="9f21d-132">Jednorozměrné pole `T[]` implementuje rozhraní `System.Collections.Generic.IList<T>` (`IList<T>` zkráceně) a jeho základní rozhraní.</span><span class="sxs-lookup"><span data-stu-id="9f21d-132">A one-dimensional array `T[]` implements the interface `System.Collections.Generic.IList<T>` (`IList<T>` for short) and its base interfaces.</span></span> <span data-ttu-id="9f21d-133">Proto je implicitní převod z `T[]` k `IList<T>` a jeho základní rozhraní.</span><span class="sxs-lookup"><span data-stu-id="9f21d-133">Accordingly, there is an implicit conversion from `T[]` to `IList<T>` and its base interfaces.</span></span> <span data-ttu-id="9f21d-134">Kromě toho, pokud je implicitní referenční převod z `S` k `T` pak `S[]` implementuje `IList<T>` a implicitní referenční převod z `S[]` k `IList<T>` a její základní rozhraní () [Odkaz na implicitní převody](conversions.md#implicit-reference-conversions)).</span><span class="sxs-lookup"><span data-stu-id="9f21d-134">In addition, if there is an implicit reference conversion from `S` to `T` then `S[]` implements `IList<T>` and there is an implicit reference conversion from `S[]` to `IList<T>` and its base interfaces ([Implicit reference conversions](conversions.md#implicit-reference-conversions)).</span></span> <span data-ttu-id="9f21d-135">Při konverzi explicitní odkaz z `S` k `T` je převod explicitní odkaz `S[]` k `IList<T>` a jeho základní rozhraní ([odkaz na explicitní převody](conversions.md#explicit-reference-conversions)).</span><span class="sxs-lookup"><span data-stu-id="9f21d-135">If there is an explicit reference conversion from `S` to `T` then there is an explicit reference conversion from `S[]` to `IList<T>` and its base interfaces ([Explicit reference conversions](conversions.md#explicit-reference-conversions)).</span></span> <span data-ttu-id="9f21d-136">Příklad:</span><span class="sxs-lookup"><span data-stu-id="9f21d-136">For example:</span></span>
```csharp
using System.Collections.Generic;

class Test
{
    static void Main() {
        string[] sa = new string[5];
        object[] oa1 = new object[5];
        object[] oa2 = sa;

        IList<string> lst1 = sa;                    // Ok
        IList<string> lst2 = oa1;                   // Error, cast needed
        IList<object> lst3 = sa;                    // Ok
        IList<object> lst4 = oa1;                   // Ok

        IList<string> lst5 = (IList<string>)oa1;    // Exception
        IList<string> lst6 = (IList<string>)oa2;    // Ok
    }
}
```

<span data-ttu-id="9f21d-137">Přiřazení `lst2 = oa1` vygeneruje chybu kompilace od převod z `object[]` k `IList<string>` je explicitní převod, ne implicitní.</span><span class="sxs-lookup"><span data-stu-id="9f21d-137">The assignment `lst2 = oa1` generates a compile-time error since the conversion from `object[]` to `IList<string>` is an explicit conversion, not implicit.</span></span> <span data-ttu-id="9f21d-138">Přetypování `(IList<string>)oa1` způsobí výjimku, která je vyvolána v době běhu od `oa1` odkazy `object[]` a ne `string[]`.</span><span class="sxs-lookup"><span data-stu-id="9f21d-138">The cast `(IList<string>)oa1` will cause an exception to be thrown at run-time since `oa1` references an `object[]` and not a `string[]`.</span></span> <span data-ttu-id="9f21d-139">Ale přetypování `(IList<string>)oa2` nezpůsobí výjimku, která je vyvolána od `oa2` odkazy `string[]`.</span><span class="sxs-lookup"><span data-stu-id="9f21d-139">However the cast `(IList<string>)oa2` will not cause an exception to be thrown since `oa2` references a `string[]`.</span></span>

<span data-ttu-id="9f21d-140">Pokaždé, když se explicitní nebo implicitní referenční převod z `S[]` k `IList<T>`, je také odkaz na explicitní převod z `IList<T>` a její základní rozhraní pro `S[]` ([explicitní odkaz převody](conversions.md#explicit-reference-conversions)).</span><span class="sxs-lookup"><span data-stu-id="9f21d-140">Whenever there is an implicit or explicit reference conversion from `S[]` to `IList<T>`, there is also an explicit reference conversion from `IList<T>` and its base interfaces to `S[]` ([Explicit reference conversions](conversions.md#explicit-reference-conversions)).</span></span>

<span data-ttu-id="9f21d-141">Pokud typ pole `S[]` implementuje `IList<T>`, některé členy implementovaného rozhraní může vyvolat výjimky.</span><span class="sxs-lookup"><span data-stu-id="9f21d-141">When an array type `S[]` implements `IList<T>`, some of the members of the implemented interface may throw exceptions.</span></span> <span data-ttu-id="9f21d-142">Přesné chování implementaci rozhraní je nad rámec téhle specifikaci.</span><span class="sxs-lookup"><span data-stu-id="9f21d-142">The precise behavior of the implementation of the interface is beyond the scope of this specification.</span></span>

## <a name="array-creation"></a><span data-ttu-id="9f21d-143">Vytvoření pole</span><span class="sxs-lookup"><span data-stu-id="9f21d-143">Array creation</span></span>

<span data-ttu-id="9f21d-144">Pole instancí jsou vytvářeny *array_creation_expression*s ([pole vytváření výrazů](expressions.md#array-creation-expressions)) nebo pole nebo místní deklarace proměnných, které patří *array_initializer*([Inicializátory pole](arrays.md#array-initializers)).</span><span class="sxs-lookup"><span data-stu-id="9f21d-144">Array instances are created by *array_creation_expression*s ([Array creation expressions](expressions.md#array-creation-expressions)) or by field or local variable declarations that include an *array_initializer* ([Array initializers](arrays.md#array-initializers)).</span></span>

<span data-ttu-id="9f21d-145">Když je vytvořena instance pole, počet rozměrů a délka každé dimenze jsou vytvořeny a pak zůstal neměnný po celou dobu života instance.</span><span class="sxs-lookup"><span data-stu-id="9f21d-145">When an array instance is created, the rank and length of each dimension are established and then remain constant for the entire lifetime of the instance.</span></span> <span data-ttu-id="9f21d-146">Jinými slovy není možné změnit pořadí existujícího pole instance, ani je možné ke změně velikosti jeho rozměrů.</span><span class="sxs-lookup"><span data-stu-id="9f21d-146">In other words, it is not possible to change the rank of an existing array instance, nor is it possible to resize its dimensions.</span></span>

<span data-ttu-id="9f21d-147">Pole instance je vždy typu pole.</span><span class="sxs-lookup"><span data-stu-id="9f21d-147">An array instance is always of an array type.</span></span> <span data-ttu-id="9f21d-148">`System.Array` Typ je abstraktní typ, který se nedá vytvořit instance.</span><span class="sxs-lookup"><span data-stu-id="9f21d-148">The `System.Array` type is an abstract type that cannot be instantiated.</span></span>

<span data-ttu-id="9f21d-149">Prvky pole vytvořené *array_creation_expression*s jsou vždy inicializovat na výchozí hodnoty ([výchozí hodnoty](variables.md#default-values)).</span><span class="sxs-lookup"><span data-stu-id="9f21d-149">Elements of arrays created by *array_creation_expression*s are always initialized to their default value ([Default values](variables.md#default-values)).</span></span>

## <a name="array-element-access"></a><span data-ttu-id="9f21d-150">Přístup k prvkům pole</span><span class="sxs-lookup"><span data-stu-id="9f21d-150">Array element access</span></span>

<span data-ttu-id="9f21d-151">Prvky pole jsou přístupné pomocí *element_access* výrazy ([pole přístup](expressions.md#array-access)) ve formátu `A[I1, I2, ..., In]`, kde `A` je výraz typu pole a každý `Ix` je Výraz typu `int`, `uint`, `long`, `ulong`, nebo lze implicitně převést na jeden nebo více z těchto typů.</span><span class="sxs-lookup"><span data-stu-id="9f21d-151">Array elements are accessed using *element_access* expressions ([Array access](expressions.md#array-access)) of the form `A[I1, I2, ..., In]`, where `A` is an expression of an array type and each `Ix` is an expression of type `int`, `uint`, `long`, `ulong`, or can be implicitly converted to one or more of these types.</span></span> <span data-ttu-id="9f21d-152">Výsledek přístupového prvku pole je proměnná, konkrétně prvku pole zvolila indexy.</span><span class="sxs-lookup"><span data-stu-id="9f21d-152">The result of an array element access is a variable, namely the array element selected by the indices.</span></span>

<span data-ttu-id="9f21d-153">Prvky pole jsou uvedené použití `foreach` – příkaz ([příkazu foreach](statements.md#the-foreach-statement)).</span><span class="sxs-lookup"><span data-stu-id="9f21d-153">The elements of an array can be enumerated using a `foreach` statement ([The foreach statement](statements.md#the-foreach-statement)).</span></span>

## <a name="array-members"></a><span data-ttu-id="9f21d-154">Členy pole</span><span class="sxs-lookup"><span data-stu-id="9f21d-154">Array members</span></span>

<span data-ttu-id="9f21d-155">Každé pole Typ dědí členy deklarované pomocí `System.Array` typu.</span><span class="sxs-lookup"><span data-stu-id="9f21d-155">Every array type inherits the members declared by the `System.Array` type.</span></span>

## <a name="array-covariance"></a><span data-ttu-id="9f21d-156">Kovariance polí</span><span class="sxs-lookup"><span data-stu-id="9f21d-156">Array covariance</span></span>

<span data-ttu-id="9f21d-157">Pro jakékoliv dva *reference_type*s `A` a `B`, pokud implicitní referenční převod ([odkaz na implicitní převody](conversions.md#implicit-reference-conversions)) nebo explicitní odkaz na převod ([ Odkaz na explicitní převody](conversions.md#explicit-reference-conversions)) existuje z `A` k `B`, pak také existuje stejný referenční převod z typu pole `A[R]` na typ pole `B[R]`, kde `R` je libovolný daný *rank_specifier* (ale stejný pro pole typů).</span><span class="sxs-lookup"><span data-stu-id="9f21d-157">For any two *reference_type*s `A` and `B`, if an implicit reference conversion ([Implicit reference conversions](conversions.md#implicit-reference-conversions)) or explicit reference conversion ([Explicit reference conversions](conversions.md#explicit-reference-conversions)) exists from `A` to `B`, then the same reference conversion also exists from the array type `A[R]` to the array type `B[R]`, where `R` is any given *rank_specifier* (but the same for both array types).</span></span> <span data-ttu-id="9f21d-158">Tento vztah se označuje jako ***kovariance polí***.</span><span class="sxs-lookup"><span data-stu-id="9f21d-158">This relationship is known as ***array covariance***.</span></span> <span data-ttu-id="9f21d-159">Kovariance polí zejména znamená, že hodnota typu pole `A[R]` ve skutečnosti může být odkazem na instanci typu pole `B[R]`, pokud existuje implicitní referenční převod z `B` k `A`.</span><span class="sxs-lookup"><span data-stu-id="9f21d-159">Array covariance in particular means that a value of an array type `A[R]` may actually be a reference to an instance of an array type `B[R]`, provided an implicit reference conversion exists from `B` to `A`.</span></span>

<span data-ttu-id="9f21d-160">Z důvodu kovariance polí přiřazení k prvkům pole typu odkazu zahrnovat kontrolu za běhu, který zajišťuje, že hodnota přiřazením k prvku pole je ve skutečnosti povolených typu ([jednoduché přiřazení](expressions.md#simple-assignment)).</span><span class="sxs-lookup"><span data-stu-id="9f21d-160">Because of array covariance, assignments to elements of reference type arrays include a run-time check which ensures that the value being assigned to the array element is actually of a permitted type ([Simple assignment](expressions.md#simple-assignment)).</span></span> <span data-ttu-id="9f21d-161">Příklad:</span><span class="sxs-lookup"><span data-stu-id="9f21d-161">For example:</span></span>
```csharp
class Test
{
    static void Fill(object[] array, int index, int count, object value) {
        for (int i = index; i < index + count; i++) array[i] = value;
    }

    static void Main() {
        string[] strings = new string[100];
        Fill(strings, 0, 100, "Undefined");
        Fill(strings, 0, 10, null);
        Fill(strings, 90, 10, 0);
    }
}
```

<span data-ttu-id="9f21d-162">Přiřazení `array[i]` v `Fill` metoda implicitně zahrnuje kontrolu za běhu, který zajistí, že objekt odkazovaný zadaným parametrem `value` je buď `null` nebo instanci, která je kompatibilní s typem skutečným prvkem `array`.</span><span class="sxs-lookup"><span data-stu-id="9f21d-162">The assignment to `array[i]` in the `Fill` method implicitly includes a run-time check which ensures that the object referenced by `value` is either `null` or an instance that is compatible with the actual element type of `array`.</span></span> <span data-ttu-id="9f21d-163">V `Main`, první dvě volání `Fill` úspěšné, ale třetí způsobí vyvolání `System.ArrayTypeMismatchException` při spuštění první přiřazení k vyvolání `array[i]`.</span><span class="sxs-lookup"><span data-stu-id="9f21d-163">In `Main`, the first two invocations of `Fill` succeed, but the third invocation causes a `System.ArrayTypeMismatchException` to be thrown upon executing the first assignment to `array[i]`.</span></span> <span data-ttu-id="9f21d-164">K výjimce dochází, protože zabalený `int` nelze ukládat v `string` pole.</span><span class="sxs-lookup"><span data-stu-id="9f21d-164">The exception occurs because a boxed `int` cannot be stored in a `string` array.</span></span>

<span data-ttu-id="9f21d-165">Kovariance polí konkrétně nerozšiřuje polí *value_type*s.</span><span class="sxs-lookup"><span data-stu-id="9f21d-165">Array covariance specifically does not extend to arrays of *value_type*s.</span></span> <span data-ttu-id="9f21d-166">Například neexistuje žádný převod tohoto povolení `int[]` zacházeno jako `object[]`.</span><span class="sxs-lookup"><span data-stu-id="9f21d-166">For example, no conversion exists that permits an `int[]` to be treated as an `object[]`.</span></span>

## <a name="array-initializers"></a><span data-ttu-id="9f21d-167">Inicializátory pole</span><span class="sxs-lookup"><span data-stu-id="9f21d-167">Array initializers</span></span>

<span data-ttu-id="9f21d-168">Inicializátory pole je možné zadat deklarace pole ([pole](classes.md#fields)), místní deklarace proměnné ([místní deklarace proměnné](statements.md#local-variable-declarations)) a pole vytváření výrazů ([vytvoření pole výrazy](expressions.md#array-creation-expressions)):</span><span class="sxs-lookup"><span data-stu-id="9f21d-168">Array initializers may be specified in field declarations ([Fields](classes.md#fields)), local variable declarations ([Local variable declarations](statements.md#local-variable-declarations)), and array creation expressions ([Array creation expressions](expressions.md#array-creation-expressions)):</span></span>

```antlr
array_initializer
    : '{' variable_initializer_list? '}'
    | '{' variable_initializer_list ',' '}'
    ;

variable_initializer_list
    : variable_initializer (',' variable_initializer)*
    ;

variable_initializer
    : expression
    | array_initializer
    ;
```

<span data-ttu-id="9f21d-169">Inicializátor pole se skládá z posloupnost proměnné inicializátory, ohraničená "`{`"a"`}`"tokeny a oddělené"`,`" tokeny.</span><span class="sxs-lookup"><span data-stu-id="9f21d-169">An array initializer consists of a sequence of variable initializers, enclosed by "`{`" and "`}`" tokens and separated by "`,`" tokens.</span></span> <span data-ttu-id="9f21d-170">Každý inicializátor proměnné je výraz, nebo v případě vícerozměrné pole inicializátor vnořeného pole.</span><span class="sxs-lookup"><span data-stu-id="9f21d-170">Each variable initializer is an expression or, in the case of a multi-dimensional array, a nested array initializer.</span></span>

<span data-ttu-id="9f21d-171">Kontext, ve kterém se používá inicializátor pole určuje typ pole, který je inicializován.</span><span class="sxs-lookup"><span data-stu-id="9f21d-171">The context in which an array initializer is used determines the type of the array being initialized.</span></span> <span data-ttu-id="9f21d-172">V výraz vytvoření pole Typ pole bezprostředně předchází inicializátor nebo je odvozen z výrazů v inicializátoru pole.</span><span class="sxs-lookup"><span data-stu-id="9f21d-172">In an array creation expression, the array type immediately precedes the initializer, or is inferred from the expressions in the array initializer.</span></span> <span data-ttu-id="9f21d-173">Typ pole je v poli nebo deklarace proměnné typu pole nebo deklarované proměnné.</span><span class="sxs-lookup"><span data-stu-id="9f21d-173">In a field or variable declaration, the array type is the type of the field or variable being declared.</span></span> <span data-ttu-id="9f21d-174">Při inicializátor pole se používá v poli nebo deklarace proměnné, jako například:</span><span class="sxs-lookup"><span data-stu-id="9f21d-174">When an array initializer is used in a field or variable declaration, such as:</span></span>
```csharp
int[] a = {0, 2, 4, 6, 8};
```
<span data-ttu-id="9f21d-175">je jednoduše zkratka pro výraz vytvoření pole ekvivalentní:</span><span class="sxs-lookup"><span data-stu-id="9f21d-175">it is simply shorthand for an equivalent array creation expression:</span></span>
```csharp
int[] a = new int[] {0, 2, 4, 6, 8};
```

<span data-ttu-id="9f21d-176">Pro jednorozměrné pole musí obsahovat inicializátor pole sekvenci výrazů, které jsou kompatibilní s typem elementu pole přiřazení.</span><span class="sxs-lookup"><span data-stu-id="9f21d-176">For a single-dimensional array, the array initializer must consist of a sequence of expressions that are assignment compatible with the element type of the array.</span></span> <span data-ttu-id="9f21d-177">Výrazy inicializaci prvků pole ve vzestupném pořadí, počínaje element na pozici nula.</span><span class="sxs-lookup"><span data-stu-id="9f21d-177">The expressions initialize array elements in increasing order, starting with the element at index zero.</span></span> <span data-ttu-id="9f21d-178">Počet výrazů v inicializátoru pole určuje vytváří instance pole.</span><span class="sxs-lookup"><span data-stu-id="9f21d-178">The number of expressions in the array initializer determines the length of the array instance being created.</span></span> <span data-ttu-id="9f21d-179">Například výše inicializátor pole vytvoří `int[]` instance o délce 5 a potom inicializuje instanci s použitím následujících hodnot:</span><span class="sxs-lookup"><span data-stu-id="9f21d-179">For example, the array initializer above creates an `int[]` instance of length 5 and then initializes the instance with the following values:</span></span>
```csharp
a[0] = 0; a[1] = 2; a[2] = 4; a[3] = 6; a[4] = 8;
```

<span data-ttu-id="9f21d-180">Vícerozměrné pole musí mít inicializátor pole libovolný počet úrovní vnoření jako dimenze v poli.</span><span class="sxs-lookup"><span data-stu-id="9f21d-180">For a multi-dimensional array, the array initializer must have as many levels of nesting as there are dimensions in the array.</span></span> <span data-ttu-id="9f21d-181">Nejkrajnější úroveň vnoření odpovídá úplně vlevo dimenze a vnitřní úroveň vnoření odpovídá rozměr nejvíce vpravo.</span><span class="sxs-lookup"><span data-stu-id="9f21d-181">The outermost nesting level corresponds to the leftmost dimension and the innermost nesting level corresponds to the rightmost dimension.</span></span> <span data-ttu-id="9f21d-182">Délka každé dimenze pole je určeno počtem prvků na odpovídající úroveň vnoření v inicializátoru pole.</span><span class="sxs-lookup"><span data-stu-id="9f21d-182">The length of each dimension of the array is determined by the number of elements at the corresponding nesting level in the array initializer.</span></span> <span data-ttu-id="9f21d-183">Pro každý inicializátor vnořeného pole. počet prvků, které musí být stejná jako jiné Inicializátory polí na stejné úrovni.</span><span class="sxs-lookup"><span data-stu-id="9f21d-183">For each nested array initializer, the number of elements must be the same as the other array initializers at the same level.</span></span> <span data-ttu-id="9f21d-184">Příklad:</span><span class="sxs-lookup"><span data-stu-id="9f21d-184">The example:</span></span>
```csharp
int[,] b = {{0, 1}, {2, 3}, {4, 5}, {6, 7}, {8, 9}};
```
<span data-ttu-id="9f21d-185">Vytvoří dvourozměrné pole o délce 5 pro první dimenzi a o délce 2 pro rozměr nejvíce vpravo:</span><span class="sxs-lookup"><span data-stu-id="9f21d-185">creates a two-dimensional array with a length of five for the leftmost dimension and a length of two for the rightmost dimension:</span></span>
```csharp
int[,] b = new int[5, 2];
```
<span data-ttu-id="9f21d-186">a pak inicializuje pole instance s použitím následujících hodnot:</span><span class="sxs-lookup"><span data-stu-id="9f21d-186">and then initializes the array instance with the following values:</span></span>
```csharp
b[0, 0] = 0; b[0, 1] = 1;
b[1, 0] = 2; b[1, 1] = 3;
b[2, 0] = 4; b[2, 1] = 5;
b[3, 0] = 6; b[3, 1] = 7;
b[4, 0] = 8; b[4, 1] = 9;
```

<span data-ttu-id="9f21d-187">Dimenzi než úplně vpravo je s nulovou délkou směru, je-li předpokládá se, že následující dimenze jsou také mít nulovou délku.</span><span class="sxs-lookup"><span data-stu-id="9f21d-187">If a dimension other than the rightmost is given with length zero, the subsequent dimensions are assumed to also have length zero.</span></span> <span data-ttu-id="9f21d-188">Příklad:</span><span class="sxs-lookup"><span data-stu-id="9f21d-188">The example:</span></span>
```csharp
int[,] c = {};
```
<span data-ttu-id="9f21d-189">Vytvoří dvourozměrné pole o délce 0 pro první a rozměr nejvíce vpravo:</span><span class="sxs-lookup"><span data-stu-id="9f21d-189">creates a two-dimensional array with a length of zero for both the leftmost and the rightmost dimension:</span></span>
```csharp
int[,] c = new int[0, 0];
```

<span data-ttu-id="9f21d-190">Pokud výraz vytvoření pole zahrnuje explicitní dimenze délky a inicializátor pole, délky musí být konstantní výrazy a počtem prvků na každou úroveň vnoření musí shodovat s délkou odpovídající dimenze.</span><span class="sxs-lookup"><span data-stu-id="9f21d-190">When an array creation expression includes both explicit dimension lengths and an array initializer, the lengths must be constant expressions and the number of elements at each nesting level must match the corresponding dimension length.</span></span> <span data-ttu-id="9f21d-191">Následuje několik příkladů:</span><span class="sxs-lookup"><span data-stu-id="9f21d-191">Here are some examples:</span></span>
```csharp
int i = 3;
int[] x = new int[3] {0, 1, 2};        // OK
int[] y = new int[i] {0, 1, 2};        // Error, i not a constant
int[] z = new int[3] {0, 1, 2, 3};     // Error, length/initializer mismatch
```

<span data-ttu-id="9f21d-192">Tady, inicializátor pro `y` výsledkem chyba kompilace, protože výraz délky dimenzí není konstantní a inicializátor pro `z` výsledkem chyba kompilace, protože délka a počet prvků v Inicializátor vzájemně nesouhlasí.</span><span class="sxs-lookup"><span data-stu-id="9f21d-192">Here, the initializer for `y` results in a compile-time error because the dimension length expression is not a constant, and the initializer for `z` results in a compile-time error because the length and the number of elements in the initializer do not agree.</span></span>
