---
ms.openlocfilehash: dbea611280a644adc25247b9887986e129c59b68
ms.sourcegitcommit: a5e393b018b04dfa55aae0000290ca087b508495
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/14/2019
ms.locfileid: "72310369"
---
# <a name="unsafe-code"></a><span data-ttu-id="229bc-101">Nebezpečný kód</span><span class="sxs-lookup"><span data-stu-id="229bc-101">Unsafe code</span></span>

<span data-ttu-id="229bc-102">Základní C# jazyk definovaný v předchozích kapitolách se liší hlavně od jazyka C a C++ při vynechání ukazatelů jako datového typu.</span><span class="sxs-lookup"><span data-stu-id="229bc-102">The core C# language, as defined in the preceding chapters, differs notably from C and C++ in its omission of pointers as a data type.</span></span> <span data-ttu-id="229bc-103">Místo toho C# poskytuje odkazy a možnost vytvářet objekty, které jsou spravovány systémem uvolňování paměti.</span><span class="sxs-lookup"><span data-stu-id="229bc-103">Instead, C# provides references and the ability to create objects that are managed by a garbage collector.</span></span> <span data-ttu-id="229bc-104">Tento návrh, společně s jinými funkcemi, C# je mnohem bezpečnější než jazyk C nebo. C++</span><span class="sxs-lookup"><span data-stu-id="229bc-104">This design, coupled with other features, makes C# a much safer language than C or C++.</span></span> <span data-ttu-id="229bc-105">V základním C# jazyce není jednoduše možné mít neinicializovaná proměnnou, ukazatel "dangling" nebo výraz, který indexuje pole nad rámec jeho hranic.</span><span class="sxs-lookup"><span data-stu-id="229bc-105">In the core C# language it is simply not possible to have an uninitialized variable, a "dangling" pointer, or an expression that indexes an array beyond its bounds.</span></span> <span data-ttu-id="229bc-106">Všechny kategorie chyb, které rutinně Plague C a C++ programy jsou tak eliminovány.</span><span class="sxs-lookup"><span data-stu-id="229bc-106">Whole categories of bugs that routinely plague C and C++ programs are thus eliminated.</span></span>

<span data-ttu-id="229bc-107">I když je prakticky každá konstrukce typu ukazatele v C++ jazyce C nebo má protějšek typu C#odkazu v, existují situace, kdy je možné, že přístup k typům ukazatelů bude nezbytný.</span><span class="sxs-lookup"><span data-stu-id="229bc-107">While practically every pointer type construct in C or C++ has a reference type counterpart in C#, nonetheless, there are situations where access to pointer types becomes a necessity.</span></span> <span data-ttu-id="229bc-108">Například propojení s podkladovým operačním systémem, přístup k zařízení mapované paměti nebo implementace algoritmu, který je časově kritický, nemusí být možné nebo praktické bez přístupu k ukazatelům.</span><span class="sxs-lookup"><span data-stu-id="229bc-108">For example, interfacing with the underlying operating system, accessing a memory-mapped device, or implementing a time-critical algorithm may not be possible or practical without access to pointers.</span></span> <span data-ttu-id="229bc-109">Aby bylo možné tuto potřebu C# vyřešit, poskytuje možnost psát ***nezabezpečený kód***.</span><span class="sxs-lookup"><span data-stu-id="229bc-109">To address this need, C# provides the ability to write ***unsafe code***.</span></span>

<span data-ttu-id="229bc-110">V nebezpečném kódu je možné deklarovat a pracovat na ukazatelích pro provádění převodů mezi ukazateli a integrálními typy, pro převzetí adresy proměnných a tak dále.</span><span class="sxs-lookup"><span data-stu-id="229bc-110">In unsafe code it is possible to declare and operate on pointers, to perform conversions between pointers and integral types, to take the address of variables, and so forth.</span></span> <span data-ttu-id="229bc-111">Ve smyslu je psaní nebezpečného kódu podobně jako psaní kódu jazyka C v C# rámci programu.</span><span class="sxs-lookup"><span data-stu-id="229bc-111">In a sense, writing unsafe code is much like writing C code within a C# program.</span></span>

<span data-ttu-id="229bc-112">Nezabezpečený kód je ve skutečnosti "bezpečnou" funkcí z perspektivy vývojářů i uživatelů.</span><span class="sxs-lookup"><span data-stu-id="229bc-112">Unsafe code is in fact a "safe" feature from the perspective of both developers and users.</span></span> <span data-ttu-id="229bc-113">Nezabezpečený kód musí být jasně označený modifikátorem `unsafe`, takže vývojáři nemůžou nepoužívat nezabezpečené funkce omylem a prováděcí modul funguje, aby se zajistilo, že nezabezpečený kód nejde spustit v nedůvěryhodném prostředí.</span><span class="sxs-lookup"><span data-stu-id="229bc-113">Unsafe code must be clearly marked with the modifier `unsafe`, so developers can't possibly use unsafe features accidentally, and the execution engine works to ensure that unsafe code cannot be executed in an untrusted environment.</span></span>

## <a name="unsafe-contexts"></a><span data-ttu-id="229bc-114">Nezabezpečené kontexty</span><span class="sxs-lookup"><span data-stu-id="229bc-114">Unsafe contexts</span></span>

<span data-ttu-id="229bc-115">Nezabezpečené funkce nástroje C# jsou k dispozici pouze v nebezpečných kontextech.</span><span class="sxs-lookup"><span data-stu-id="229bc-115">The unsafe features of C# are available only in unsafe contexts.</span></span> <span data-ttu-id="229bc-116">Nezabezpečený kontext je představený zahrnutím modifikátoru `unsafe` v deklaraci typu nebo členu nebo využitím *unsafe_statement*:</span><span class="sxs-lookup"><span data-stu-id="229bc-116">An unsafe context is introduced by including an `unsafe` modifier in the declaration of a type or member, or by employing an *unsafe_statement*:</span></span>

*  <span data-ttu-id="229bc-117">Deklarace třídy, struktury, rozhraní nebo delegáta může obsahovat modifikátor `unsafe`. v takovém případě je celý textový rozsah deklarace typu (včetně těla třídy, struktury nebo rozhraní) považován za nezabezpečený kontext.</span><span class="sxs-lookup"><span data-stu-id="229bc-117">A declaration of a class, struct, interface, or delegate may include an `unsafe` modifier, in which case the entire textual extent of that type declaration (including the body of the class, struct, or interface) is considered an unsafe context.</span></span>
*  <span data-ttu-id="229bc-118">Deklarace pole, metody, vlastnosti, události, indexeru, operátora, konstruktoru instance, destruktoru nebo statického konstruktoru může zahrnovat modifikátor `unsafe`. v takovém případě je celý textový rozsah této deklarace člena považován za nezabezpečený kontext.</span><span class="sxs-lookup"><span data-stu-id="229bc-118">A declaration of a field, method, property, event, indexer, operator, instance constructor, destructor, or static constructor may include an `unsafe` modifier, in which case the entire textual extent of that member declaration is considered an unsafe context.</span></span>
*  <span data-ttu-id="229bc-119">*Unsafe_statement* povoluje použití nebezpečného kontextu v rámci *bloku*.</span><span class="sxs-lookup"><span data-stu-id="229bc-119">An *unsafe_statement* enables the use of an unsafe context within a *block*.</span></span> <span data-ttu-id="229bc-120">Celý textový rozsah asociovaného *bloku* je považován za nezabezpečený kontext.</span><span class="sxs-lookup"><span data-stu-id="229bc-120">The entire textual extent of the associated *block* is considered an unsafe context.</span></span>

<span data-ttu-id="229bc-121">Níže jsou uvedeny související gramatické výroby.</span><span class="sxs-lookup"><span data-stu-id="229bc-121">The associated grammar productions are shown below.</span></span>

```antlr
class_modifier_unsafe
    : 'unsafe'
    ;

struct_modifier_unsafe
    : 'unsafe'
    ;

interface_modifier_unsafe
    : 'unsafe'
    ;

delegate_modifier_unsafe
    : 'unsafe'
    ;

field_modifier_unsafe
    : 'unsafe'
    ;

method_modifier_unsafe
    : 'unsafe'
    ;

property_modifier_unsafe
    : 'unsafe'
    ;

event_modifier_unsafe
    : 'unsafe'
    ;

indexer_modifier_unsafe
    : 'unsafe'
    ;

operator_modifier_unsafe
    : 'unsafe'
    ;

constructor_modifier_unsafe
    : 'unsafe'
    ;

destructor_declaration_unsafe
    : attributes? 'extern'? 'unsafe'? '~' identifier '(' ')' destructor_body
    | attributes? 'unsafe'? 'extern'? '~' identifier '(' ')' destructor_body
    ;

static_constructor_modifiers_unsafe
    : 'extern'? 'unsafe'? 'static'
    | 'unsafe'? 'extern'? 'static'
    | 'extern'? 'static' 'unsafe'?
    | 'unsafe'? 'static' 'extern'?
    | 'static' 'extern'? 'unsafe'?
    | 'static' 'unsafe'? 'extern'?
    ;

embedded_statement_unsafe
    : unsafe_statement
    | fixed_statement
    ;

unsafe_statement
    : 'unsafe' block
    ;
```

<span data-ttu-id="229bc-122">V příkladu</span><span class="sxs-lookup"><span data-stu-id="229bc-122">In the example</span></span>

```csharp
public unsafe struct Node
{
    public int Value;
    public Node* Left;
    public Node* Right;
}
```

<span data-ttu-id="229bc-123">Modifikátor `unsafe` určený v deklaraci struktury způsobí, že celý textový rozsah deklarace struktury se stane nebezpečným kontextem.</span><span class="sxs-lookup"><span data-stu-id="229bc-123">the `unsafe` modifier specified in the struct declaration causes the entire textual extent of the struct declaration to become an unsafe context.</span></span> <span data-ttu-id="229bc-124">Proto je možné deklarovat `Left` a `Right` pole jako typ ukazatele.</span><span class="sxs-lookup"><span data-stu-id="229bc-124">Thus, it is possible to declare the `Left` and `Right` fields to be of a pointer type.</span></span> <span data-ttu-id="229bc-125">Lze také zapsat příklad výše.</span><span class="sxs-lookup"><span data-stu-id="229bc-125">The example above could also be written</span></span>

```csharp
public struct Node
{
    public int Value;
    public unsafe Node* Left;
    public unsafe Node* Right;
}
```

<span data-ttu-id="229bc-126">V tomto případě modifikátory `unsafe` v deklaracích polí způsobí, že tyto deklarace budou považovány za nebezpečné kontexty.</span><span class="sxs-lookup"><span data-stu-id="229bc-126">Here, the `unsafe` modifiers in the field declarations cause those declarations to be considered unsafe contexts.</span></span>

<span data-ttu-id="229bc-127">Kromě vytvoření nebezpečného kontextu, což umožňuje použití typů ukazatelů, modifikátor `unsafe` nemá žádný vliv na typ nebo člen.</span><span class="sxs-lookup"><span data-stu-id="229bc-127">Other than establishing an unsafe context, thus permitting the use of pointer types, the `unsafe` modifier has no effect on a type or a member.</span></span> <span data-ttu-id="229bc-128">V příkladu</span><span class="sxs-lookup"><span data-stu-id="229bc-128">In the example</span></span>

```csharp
public class A
{
    public unsafe virtual void F() {
        char* p;
        ...
    }
}

public class B: A
{
    public override void F() {
        base.F();
        ...
    }
}
```

<span data-ttu-id="229bc-129">Modifikátor `unsafe` v metodě `F` v `A` jednoduše způsobí, že se textový rozsah `F` stane nebezpečným kontextem, ve kterém lze použít nebezpečné funkce jazyka.</span><span class="sxs-lookup"><span data-stu-id="229bc-129">the `unsafe` modifier on the `F` method in `A` simply causes the textual extent of `F` to become an unsafe context in which the unsafe features of the language can be used.</span></span> <span data-ttu-id="229bc-130">V přepsání `F` v `B`není nutné znovu zadat modifikátor `unsafe` – Pokud `F` metoda v `B` sama potřebuje přístup k nebezpečným funkcím.</span><span class="sxs-lookup"><span data-stu-id="229bc-130">In the override of `F` in `B`, there is no need to re-specify the `unsafe` modifier -- unless, of course, the `F` method in `B` itself needs access to unsafe features.</span></span>

<span data-ttu-id="229bc-131">Tato situace je mírně odlišná, pokud je typ ukazatele součástí signatury metody.</span><span class="sxs-lookup"><span data-stu-id="229bc-131">The situation is slightly different when a pointer type is part of the method's signature</span></span>

```csharp
public unsafe class A
{
    public virtual void F(char* p) {...}
}

public class B: A
{
    public unsafe override void F(char* p) {...}
}
```

<span data-ttu-id="229bc-132">V důsledku toho, že signatura `F`obsahuje typ ukazatele, může být zapsána pouze v nezabezpečeném kontextu.</span><span class="sxs-lookup"><span data-stu-id="229bc-132">Here, because `F`'s signature includes a pointer type, it can only be written in an unsafe context.</span></span> <span data-ttu-id="229bc-133">Nezabezpečený kontext však lze začlenit buď tak, že celou třídu není bezpečná, jako je případ v `A`, nebo zahrnutím modifikátoru `unsafe` v deklaraci metody, stejně jako v případě `B`.</span><span class="sxs-lookup"><span data-stu-id="229bc-133">However, the unsafe context can be introduced by either making the entire class unsafe, as is the case in `A`, or by including an `unsafe` modifier in the method declaration, as is the case in `B`.</span></span>

## <a name="pointer-types"></a><span data-ttu-id="229bc-134">Typy ukazatelů</span><span class="sxs-lookup"><span data-stu-id="229bc-134">Pointer types</span></span>

<span data-ttu-id="229bc-135">V nezabezpečeném kontextu může být *typ* ([typy](types.md)) *pointer_type* a také *value_type* nebo *reference_type*.</span><span class="sxs-lookup"><span data-stu-id="229bc-135">In an unsafe context, a *type* ([Types](types.md)) may be a *pointer_type* as well as a *value_type* or a *reference_type*.</span></span> <span data-ttu-id="229bc-136">*Pointer_type* však lze použít také ve výrazu `typeof` ([výrazy vytváření anonymních objektů](expressions.md#anonymous-object-creation-expressions)) mimo nezabezpečený kontext, protože takové použití není bezpečné.</span><span class="sxs-lookup"><span data-stu-id="229bc-136">However, a *pointer_type* may also be used in a `typeof` expression ([Anonymous object creation expressions](expressions.md#anonymous-object-creation-expressions)) outside of an unsafe context as such usage is not unsafe.</span></span>

```antlr
type_unsafe
    : pointer_type
    ;
```

<span data-ttu-id="229bc-137">*Pointer_type* se zapisuje jako *unmanaged_type* nebo klíčové slovo `void`a za ním následuje token `*`:</span><span class="sxs-lookup"><span data-stu-id="229bc-137">A *pointer_type* is written as an *unmanaged_type* or the keyword `void`, followed by a `*` token:</span></span>

```antlr
pointer_type
    : unmanaged_type '*'
    | 'void' '*'
    ;

unmanaged_type
    : type
    ;
```

<span data-ttu-id="229bc-138">Typ zadaný před `*` v typu ukazatele se nazývá ***typ referenční*** typu ukazatele.</span><span class="sxs-lookup"><span data-stu-id="229bc-138">The type specified before the `*` in a pointer type is called the ***referent type*** of the pointer type.</span></span> <span data-ttu-id="229bc-139">Představuje typ proměnné, do které hodnota typu ukazatele odkazuje.</span><span class="sxs-lookup"><span data-stu-id="229bc-139">It represents the type of the variable to which a value of the pointer type points.</span></span>

<span data-ttu-id="229bc-140">Na rozdíl od odkazů (hodnoty typů odkazů) ukazatele nejsou sledovány systémem uvolňování paměti – systém uvolňování paměti nemá žádné znalosti ukazatelů a dat, na která odkazují.</span><span class="sxs-lookup"><span data-stu-id="229bc-140">Unlike references (values of reference types), pointers are not tracked by the garbage collector -- the garbage collector has no knowledge of pointers and the data to which they point.</span></span> <span data-ttu-id="229bc-141">Z tohoto důvodu ukazatel není povolen odkazování na odkaz nebo na strukturu, která obsahuje odkazy, a typ referenční ukazatele musí být *unmanaged_type*.</span><span class="sxs-lookup"><span data-stu-id="229bc-141">For this reason a pointer is not permitted to point to a reference or to a struct that contains references, and the referent type of a pointer must be an *unmanaged_type*.</span></span>

<span data-ttu-id="229bc-142">*Unmanaged_type* je jakýkoliv typ, který není *reference_type* nebo konstruovaný typ a neobsahuje *reference_type* nebo vytvořená pole typu na žádné úrovni vnoření.</span><span class="sxs-lookup"><span data-stu-id="229bc-142">An *unmanaged_type* is any type that isn't a *reference_type* or constructed type, and doesn't contain *reference_type* or constructed type fields at any level of nesting.</span></span> <span data-ttu-id="229bc-143">Jinými slovy *unmanaged_type* je jedna z následujících:</span><span class="sxs-lookup"><span data-stu-id="229bc-143">In other words, an *unmanaged_type* is one of the following:</span></span>

*  <span data-ttu-id="229bc-144">`sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, `decimal`nebo `bool`.</span><span class="sxs-lookup"><span data-stu-id="229bc-144">`sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, `decimal`, or `bool`.</span></span>
*  <span data-ttu-id="229bc-145">Jakékoli *enum_type*.</span><span class="sxs-lookup"><span data-stu-id="229bc-145">Any *enum_type*.</span></span>
*  <span data-ttu-id="229bc-146">Jakékoli *pointer_type*.</span><span class="sxs-lookup"><span data-stu-id="229bc-146">Any *pointer_type*.</span></span>
*  <span data-ttu-id="229bc-147">Jakékoli uživatelsky definované *struct_type* , které nejsou konstruovaným typem a obsahují pouze pole *unmanaged_type*.</span><span class="sxs-lookup"><span data-stu-id="229bc-147">Any user-defined *struct_type* that is not a constructed type and contains fields of *unmanaged_type*s only.</span></span>

<span data-ttu-id="229bc-148">Intuitivní pravidlo pro kombinování ukazatelů a odkazů je, že referents odkazy (objekty) mají obsahovat ukazatele, ale referents ukazatelů nemají oprávnění obsahovat odkazy.</span><span class="sxs-lookup"><span data-stu-id="229bc-148">The intuitive rule for mixing of pointers and references is that referents of references (objects) are permitted to contain pointers, but referents of pointers are not permitted to contain references.</span></span>

<span data-ttu-id="229bc-149">V následující tabulce jsou uvedeny některé příklady typů ukazatelů:</span><span class="sxs-lookup"><span data-stu-id="229bc-149">Some examples of pointer types are given in the table below:</span></span>

| <span data-ttu-id="229bc-150">__Příklad__</span><span class="sxs-lookup"><span data-stu-id="229bc-150">__Example__</span></span> | <span data-ttu-id="229bc-151">__Popis__</span><span class="sxs-lookup"><span data-stu-id="229bc-151">__Description__</span></span>                               |
|-------------|-----------------------------------------------|
| `byte*`     | <span data-ttu-id="229bc-152">Ukazatel na `byte`</span><span class="sxs-lookup"><span data-stu-id="229bc-152">Pointer to `byte`</span></span>                             |
| `char*`     | <span data-ttu-id="229bc-153">Ukazatel na `char`</span><span class="sxs-lookup"><span data-stu-id="229bc-153">Pointer to `char`</span></span>                             |
| `int**`     | <span data-ttu-id="229bc-154">Ukazatel na ukazatel na `int`</span><span class="sxs-lookup"><span data-stu-id="229bc-154">Pointer to pointer to `int`</span></span>                   |
| `int*[]`    | <span data-ttu-id="229bc-155">Jednorozměrné pole ukazatelů na `int`</span><span class="sxs-lookup"><span data-stu-id="229bc-155">Single-dimensional array of pointers to `int`</span></span> |
| `void*`     | <span data-ttu-id="229bc-156">Ukazatel na neznámý typ</span><span class="sxs-lookup"><span data-stu-id="229bc-156">Pointer to unknown type</span></span>                       |

<span data-ttu-id="229bc-157">Pro danou implementaci musí mít všechny typy ukazatelů stejnou velikost a reprezentace.</span><span class="sxs-lookup"><span data-stu-id="229bc-157">For a given implementation, all pointer types must have the same size and representation.</span></span>

<span data-ttu-id="229bc-158">Na rozdíl od jazyka C++C a, pokud je více ukazatelů deklarováno ve stejné deklaraci C# , v `*` je zapsána společně s podkladovým typem, nikoli jako předpona punctuator na každém názvu ukazatele.</span><span class="sxs-lookup"><span data-stu-id="229bc-158">Unlike C and C++, when multiple pointers are declared in the same declaration, in C# the `*` is written along with the underlying type only, not as a prefix punctuator on each pointer name.</span></span> <span data-ttu-id="229bc-159">Například</span><span class="sxs-lookup"><span data-stu-id="229bc-159">For example</span></span>

```csharp
int* pi, pj;    // NOT as int *pi, *pj;
```

<span data-ttu-id="229bc-160">Hodnota ukazatele typu `T*` představuje adresu proměnné typu `T`.</span><span class="sxs-lookup"><span data-stu-id="229bc-160">The value of a pointer having type `T*` represents the address of a variable of type `T`.</span></span> <span data-ttu-id="229bc-161">Operátor dereference ukazatele `*` ([Indirekce ukazatele](unsafe-code.md#pointer-indirection)) se dá použít pro přístup k této proměnné.</span><span class="sxs-lookup"><span data-stu-id="229bc-161">The pointer indirection operator `*` ([Pointer indirection](unsafe-code.md#pointer-indirection)) may be used to access this variable.</span></span> <span data-ttu-id="229bc-162">Například s ohledem na proměnnou `P` typu `int*`výraz `*P` označuje `int` proměnnou nalezenou na adrese obsažené v `P`.</span><span class="sxs-lookup"><span data-stu-id="229bc-162">For example, given a variable `P` of type `int*`, the expression `*P` denotes the `int` variable found at the address contained in `P`.</span></span>

<span data-ttu-id="229bc-163">Podobně jako odkaz na objekt může být `null`ukazatel.</span><span class="sxs-lookup"><span data-stu-id="229bc-163">Like an object reference, a pointer may be `null`.</span></span> <span data-ttu-id="229bc-164">Použití operátoru dereference na `null` ukazatel má za následek chování definované implementací.</span><span class="sxs-lookup"><span data-stu-id="229bc-164">Applying the indirection operator to a `null` pointer results in implementation-defined behavior.</span></span> <span data-ttu-id="229bc-165">Ukazatel s hodnotou `null` je reprezentován všemi-bity-Zero.</span><span class="sxs-lookup"><span data-stu-id="229bc-165">A pointer with value `null` is represented by all-bits-zero.</span></span>

<span data-ttu-id="229bc-166">Typ `void*` představuje ukazatel na neznámý typ.</span><span class="sxs-lookup"><span data-stu-id="229bc-166">The `void*` type represents a pointer to an unknown type.</span></span> <span data-ttu-id="229bc-167">Vzhledem k tomu, že typ referenční je neznámý, nelze operátor dereference použít na ukazatel typu `void*`, ani na takový ukazatel nelze provádět aritmetické operace.</span><span class="sxs-lookup"><span data-stu-id="229bc-167">Because the referent type is unknown, the indirection operator cannot be applied to a pointer of type `void*`, nor can any arithmetic be performed on such a pointer.</span></span> <span data-ttu-id="229bc-168">Nicméně ukazatel typu `void*` lze přetypovat na jakýkoli jiný typ ukazatele (a naopak).</span><span class="sxs-lookup"><span data-stu-id="229bc-168">However, a pointer of type `void*` can be cast to any other pointer type (and vice versa).</span></span>

<span data-ttu-id="229bc-169">Typy ukazatelů jsou samostatnou kategorií typů.</span><span class="sxs-lookup"><span data-stu-id="229bc-169">Pointer types are a separate category of types.</span></span> <span data-ttu-id="229bc-170">Na rozdíl od typů odkazu a hodnot typy ukazatelů nedědí z `object` a mezi typy ukazatelů a `object`neexistují žádné převody.</span><span class="sxs-lookup"><span data-stu-id="229bc-170">Unlike reference types and value types, pointer types do not inherit from `object` and no conversions exist between pointer types and `object`.</span></span> <span data-ttu-id="229bc-171">Konkrétně zabalení a rozbalení (zabalení[a rozbalení](types.md#boxing-and-unboxing)) nejsou pro ukazatele podporovány.</span><span class="sxs-lookup"><span data-stu-id="229bc-171">In particular, boxing and unboxing ([Boxing and unboxing](types.md#boxing-and-unboxing)) are not supported for pointers.</span></span> <span data-ttu-id="229bc-172">Převody jsou však povoleny mezi různými typy ukazatelů a mezi typy ukazatelů a celočíselnými typy.</span><span class="sxs-lookup"><span data-stu-id="229bc-172">However, conversions are permitted between different pointer types and between pointer types and the integral types.</span></span> <span data-ttu-id="229bc-173">Tento postup je popsaný v tématu [převody ukazatelů](unsafe-code.md#pointer-conversions).</span><span class="sxs-lookup"><span data-stu-id="229bc-173">This is described in [Pointer conversions](unsafe-code.md#pointer-conversions).</span></span>

<span data-ttu-id="229bc-174">*Pointer_type* nelze použít jako argument typu ([konstruované typy](types.md#constructed-types)) a odvození typu ([odvození typu](expressions.md#type-inference)) se nedaří při voláních obecných metod, které by byly odvozeny argumentem typu jako typ ukazatele.</span><span class="sxs-lookup"><span data-stu-id="229bc-174">A *pointer_type* cannot be used as a type argument ([Constructed types](types.md#constructed-types)), and type inference ([Type inference](expressions.md#type-inference)) fails on generic method calls that would have inferred a type argument to be a pointer type.</span></span>

<span data-ttu-id="229bc-175">Jako typ pole typu volatile (pole s modifikátorem[volatile](classes.md#volatile-fields)) se dá použít *pointer_type* .</span><span class="sxs-lookup"><span data-stu-id="229bc-175">A *pointer_type* may be used as the type of a volatile field ([Volatile fields](classes.md#volatile-fields)).</span></span>

<span data-ttu-id="229bc-176">I když mohou být ukazatele předány jako `ref` nebo `out` parametry, tak může dojít k nedefinovanému chování, protože ukazatel může být dobře nastaven tak, aby odkazoval na místní proměnnou, která již neexistuje, je-li volaná metoda vrácena, nebo pevný objekt, na který je odkazováno, již není opraven.</span><span class="sxs-lookup"><span data-stu-id="229bc-176">Although pointers can be passed as `ref` or `out` parameters, doing so can cause undefined behavior, since the pointer may well be set to point to a local variable which no longer exists when the called method returns, or the fixed object to which it used to point, is no longer fixed.</span></span> <span data-ttu-id="229bc-177">Příklad:</span><span class="sxs-lookup"><span data-stu-id="229bc-177">For example:</span></span>

```csharp
using System;

class Test
{
    static int value = 20;

    unsafe static void F(out int* pi1, ref int* pi2) {
        int i = 10;
        pi1 = &i;

        fixed (int* pj = &value) {
            // ...
            pi2 = pj;
        }
    }

    static void Main() {
        int i = 10;
        unsafe {
            int* px1;
            int* px2 = &i;

            F(out px1, ref px2);

            Console.WriteLine("*px1 = {0}, *px2 = {1}",
                *px1, *px2);    // undefined behavior
        }
    }
}
```

<span data-ttu-id="229bc-178">Metoda může vracet hodnotu nějakého typu a tento typ může být ukazatel.</span><span class="sxs-lookup"><span data-stu-id="229bc-178">A method can return a value of some type, and that type can be a pointer.</span></span> <span data-ttu-id="229bc-179">Například pokud je předána ukazatel na souvislou sekvenci `int`s, počet prvků sekvence a jinou hodnotu `int`, následující metoda vrátí adresu této hodnoty v této sekvenci, pokud dojde ke shodě; v opačném případě vrátí `null`:</span><span class="sxs-lookup"><span data-stu-id="229bc-179">For example, when given a pointer to a contiguous sequence of `int`s, that sequence's element count, and some other `int` value, the following method returns the address of that value in that sequence, if a match occurs; otherwise it returns `null`:</span></span>

```csharp
unsafe static int* Find(int* pi, int size, int value) {
    for (int i = 0; i < size; ++i) {
        if (*pi == value) 
            return pi;
        ++pi;
    }
    return null;
}
```

<span data-ttu-id="229bc-180">V nezabezpečeném kontextu jsou k dispozici několik konstrukcí pro provoz na ukazatelích:</span><span class="sxs-lookup"><span data-stu-id="229bc-180">In an unsafe context, several constructs are available for operating on pointers:</span></span>

*  <span data-ttu-id="229bc-181">Operátor `*` lze použít k provedení dereference ukazatele ([dereference ukazatele](unsafe-code.md#pointer-indirection)).</span><span class="sxs-lookup"><span data-stu-id="229bc-181">The `*` operator may be used to perform pointer indirection ([Pointer indirection](unsafe-code.md#pointer-indirection)).</span></span>
*  <span data-ttu-id="229bc-182">Operátor `->` lze použít pro přístup k členu struktury prostřednictvím ukazatele ([Přístup člena ukazatele](unsafe-code.md#pointer-member-access)).</span><span class="sxs-lookup"><span data-stu-id="229bc-182">The `->` operator may be used to access a member of a struct through a pointer ([Pointer member access](unsafe-code.md#pointer-member-access)).</span></span>
*  <span data-ttu-id="229bc-183">Operátor `[]` lze použít k indexování ukazatele ([přístup k prvku ukazatele](unsafe-code.md#pointer-element-access)).</span><span class="sxs-lookup"><span data-stu-id="229bc-183">The `[]` operator may be used to index a pointer ([Pointer element access](unsafe-code.md#pointer-element-access)).</span></span>
*  <span data-ttu-id="229bc-184">Operátor `&` lze použít k získání adresy proměnné ([operátor address-of](unsafe-code.md#the-address-of-operator)).</span><span class="sxs-lookup"><span data-stu-id="229bc-184">The `&` operator may be used to obtain the address of a variable ([The address-of operator](unsafe-code.md#the-address-of-operator)).</span></span>
*  <span data-ttu-id="229bc-185">Operátory `++` a `--` lze použít k zvýšení a snížení ukazatelů ([zvýšení a snížení ukazatele](unsafe-code.md#pointer-increment-and-decrement)).</span><span class="sxs-lookup"><span data-stu-id="229bc-185">The `++` and `--` operators may be used to increment and decrement pointers ([Pointer increment and decrement](unsafe-code.md#pointer-increment-and-decrement)).</span></span>
*  <span data-ttu-id="229bc-186">Operátory `+` a `-` lze použít k provádění aritmetického ukazatele ([aritmetický ukazatel](unsafe-code.md#pointer-arithmetic)).</span><span class="sxs-lookup"><span data-stu-id="229bc-186">The `+` and `-` operators may be used to perform pointer arithmetic ([Pointer arithmetic](unsafe-code.md#pointer-arithmetic)).</span></span>
*  <span data-ttu-id="229bc-187">Operátory `==`, `!=`, `<`, `>`, `<=`a `=>` lze použít k porovnání ukazatelů ([porovnání ukazatelů](unsafe-code.md#pointer-comparison)).</span><span class="sxs-lookup"><span data-stu-id="229bc-187">The `==`, `!=`, `<`, `>`, `<=`, and `=>` operators may be used to compare pointers ([Pointer comparison](unsafe-code.md#pointer-comparison)).</span></span>
*  <span data-ttu-id="229bc-188">Operátor `stackalloc` lze použít k přidělení paměti ze zásobníku volání ([vyrovnávací paměti pevné velikosti](unsafe-code.md#fixed-size-buffers)).</span><span class="sxs-lookup"><span data-stu-id="229bc-188">The `stackalloc` operator may be used to allocate memory from the call stack ([Fixed size buffers](unsafe-code.md#fixed-size-buffers)).</span></span>
*  <span data-ttu-id="229bc-189">Příkaz `fixed` lze použít k dočasné opravě proměnné tak, aby se její adresa mohla získat ([příkaz fixed](unsafe-code.md#the-fixed-statement)).</span><span class="sxs-lookup"><span data-stu-id="229bc-189">The `fixed` statement may be used to temporarily fix a variable so its address can be obtained ([The fixed statement](unsafe-code.md#the-fixed-statement)).</span></span>

## <a name="fixed-and-moveable-variables"></a><span data-ttu-id="229bc-190">Pevné a mobilní proměnné</span><span class="sxs-lookup"><span data-stu-id="229bc-190">Fixed and moveable variables</span></span>

<span data-ttu-id="229bc-191">Operátor address-of ([operátor address-of](unsafe-code.md#the-address-of-operator)) a příkaz `fixed` ([příkaz fixed](unsafe-code.md#the-fixed-statement)) rozdělují proměnné do dvou kategorií: ***pevné proměnné*** a ***přenosné proměnné***.</span><span class="sxs-lookup"><span data-stu-id="229bc-191">The address-of operator ([The address-of operator](unsafe-code.md#the-address-of-operator)) and the `fixed` statement ([The fixed statement](unsafe-code.md#the-fixed-statement)) divide variables into two categories: ***Fixed variables*** and ***moveable variables***.</span></span>

<span data-ttu-id="229bc-192">Pevné proměnné jsou umístěny v umístěních úložiště, která nejsou ovlivněna operací uvolňování paměti.</span><span class="sxs-lookup"><span data-stu-id="229bc-192">Fixed variables reside in storage locations that are unaffected by operation of the garbage collector.</span></span> <span data-ttu-id="229bc-193">(Příklady pevných proměnných zahrnují lokální proměnné, hodnoty parametrů a proměnné, které jsou vytvořeny pomocí přesměrování ukazatelů.) Na druhé straně se přenosné proměnné nacházejí v umístěních úložiště, která se vztahují k přemístění nebo vyřazení systémem uvolňování paměti.</span><span class="sxs-lookup"><span data-stu-id="229bc-193">(Examples of fixed variables include local variables, value parameters, and variables created by dereferencing pointers.) On the other hand, moveable variables reside in storage locations that are subject to relocation or disposal by the garbage collector.</span></span> <span data-ttu-id="229bc-194">(Příklady pohyblivých proměnných zahrnují pole v objektech a prvcích polí.)</span><span class="sxs-lookup"><span data-stu-id="229bc-194">(Examples of moveable variables include fields in objects and elements of arrays.)</span></span>

<span data-ttu-id="229bc-195">Operátor `&` ([operátor address-of](unsafe-code.md#the-address-of-operator)) umožňuje, aby se adresa pevné proměnné získala bez omezení.</span><span class="sxs-lookup"><span data-stu-id="229bc-195">The `&` operator ([The address-of operator](unsafe-code.md#the-address-of-operator)) permits the address of a fixed variable to be obtained without restrictions.</span></span> <span data-ttu-id="229bc-196">Vzhledem k tomu, že pohyblivá proměnná je předmětem přemístění nebo likvidace systémem uvolňování paměti, adresa pohyblivé proměnné lze získat pouze pomocí příkazu `fixed` ([příkaz fixed](unsafe-code.md#the-fixed-statement)) a tato adresa zůstává platná pouze po dobu trvání tohoto příkazu `fixed`.</span><span class="sxs-lookup"><span data-stu-id="229bc-196">However, because a moveable variable is subject to relocation or disposal by the garbage collector, the address of a moveable variable can only be obtained using a `fixed` statement ([The fixed statement](unsafe-code.md#the-fixed-statement)), and that address remains valid only for the duration of that `fixed` statement.</span></span>

<span data-ttu-id="229bc-197">V přesném smyslu je pevná proměnná jedna z následujících:</span><span class="sxs-lookup"><span data-stu-id="229bc-197">In precise terms, a fixed variable is one of the following:</span></span>

*  <span data-ttu-id="229bc-198">Proměnná, která je výsledkem *simple_name* ([jednoduché názvy](expressions.md#simple-names)), která odkazuje na místní proměnnou nebo parametr hodnoty, pokud proměnná není zachycena anonymní funkcí.</span><span class="sxs-lookup"><span data-stu-id="229bc-198">A variable resulting from a *simple_name* ([Simple names](expressions.md#simple-names)) that refers to a local variable or a value parameter, unless the variable is captured by an anonymous function.</span></span>
*  <span data-ttu-id="229bc-199">Proměnná, která je výsledkem *member_access* ([členský přístup](expressions.md#member-access)) formuláře `V.I`, kde `V` je pevná proměnná *struct_type*.</span><span class="sxs-lookup"><span data-stu-id="229bc-199">A variable resulting from a *member_access* ([Member access](expressions.md#member-access)) of the form `V.I`, where `V` is a fixed variable of a *struct_type*.</span></span>
*  <span data-ttu-id="229bc-200">Proměnná, která vyplývají z *pointer_indirection_expression* ([dereference ukazatele](unsafe-code.md#pointer-indirection)) formuláře `*P`, *pointer_member_access* ([přístup ke členu ukazatele](unsafe-code.md#pointer-member-access)) formuláře `P->I`nebo *pointer_element_access* ([přístup k prvku ukazatele](unsafe-code.md#pointer-element-access)) formuláře `P[E]`.</span><span class="sxs-lookup"><span data-stu-id="229bc-200">A variable resulting from a *pointer_indirection_expression* ([Pointer indirection](unsafe-code.md#pointer-indirection)) of the form `*P`, a *pointer_member_access* ([Pointer member access](unsafe-code.md#pointer-member-access)) of the form `P->I`, or a *pointer_element_access* ([Pointer element access](unsafe-code.md#pointer-element-access)) of the form `P[E]`.</span></span>

<span data-ttu-id="229bc-201">Všechny ostatní proměnné jsou klasifikovány jako přenosné proměnné.</span><span class="sxs-lookup"><span data-stu-id="229bc-201">All other variables are classified as moveable variables.</span></span>

<span data-ttu-id="229bc-202">Všimněte si, že statické pole je klasifikované jako mobilní proměnná.</span><span class="sxs-lookup"><span data-stu-id="229bc-202">Note that a static field is classified as a moveable variable.</span></span> <span data-ttu-id="229bc-203">Všimněte si také, že parametr `ref` nebo `out` je klasifikován jako pohyblivá proměnná, i když argument zadaný pro parametr je pevná proměnná.</span><span class="sxs-lookup"><span data-stu-id="229bc-203">Also note that a `ref` or `out` parameter is classified as a moveable variable, even if the argument given for the parameter is a fixed variable.</span></span> <span data-ttu-id="229bc-204">Nakonec si všimněte, že proměnná vytvořená pomocí přesměrování ukazatele je vždy klasifikována jako pevná proměnná.</span><span class="sxs-lookup"><span data-stu-id="229bc-204">Finally, note that a variable produced by dereferencing a pointer is always classified as a fixed variable.</span></span>

## <a name="pointer-conversions"></a><span data-ttu-id="229bc-205">Převody ukazatele</span><span class="sxs-lookup"><span data-stu-id="229bc-205">Pointer conversions</span></span>

<span data-ttu-id="229bc-206">V nezabezpečeném kontextu je sada dostupných implicitních převodů ([implicitní převody](conversions.md#implicit-conversions)) rozšířena tak, aby zahrnovala následující implicitní převody ukazatelů:</span><span class="sxs-lookup"><span data-stu-id="229bc-206">In an unsafe context, the set of available implicit conversions ([Implicit conversions](conversions.md#implicit-conversions)) is extended to include the following implicit pointer conversions:</span></span>

*  <span data-ttu-id="229bc-207">Z libovolného *pointer_type* na typ `void*`.</span><span class="sxs-lookup"><span data-stu-id="229bc-207">From any *pointer_type* to the type `void*`.</span></span>
*  <span data-ttu-id="229bc-208">Z `null` literálu *pointer_type*.</span><span class="sxs-lookup"><span data-stu-id="229bc-208">From the `null` literal to any *pointer_type*.</span></span>

<span data-ttu-id="229bc-209">Kromě toho v nezabezpečeném kontextu je sada dostupných explicitních převodů ([explicitní převody](conversions.md#explicit-conversions)) rozšířena tak, aby zahrnovala následující explicitní převody ukazatelů:</span><span class="sxs-lookup"><span data-stu-id="229bc-209">Additionally, in an unsafe context, the set of available explicit conversions ([Explicit conversions](conversions.md#explicit-conversions)) is extended to include the following explicit pointer conversions:</span></span>

*  <span data-ttu-id="229bc-210">Z libovolného *pointer_type* na jakékoli jiné *pointer_type*.</span><span class="sxs-lookup"><span data-stu-id="229bc-210">From any *pointer_type* to any other *pointer_type*.</span></span>
*  <span data-ttu-id="229bc-211">Z `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`*nebo `ulong` na pointer_type.*</span><span class="sxs-lookup"><span data-stu-id="229bc-211">From `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, or `ulong` to any *pointer_type*.</span></span>
*  <span data-ttu-id="229bc-212">Z libovolného *pointer_type* na `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`nebo `ulong`.</span><span class="sxs-lookup"><span data-stu-id="229bc-212">From any *pointer_type* to `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, or `ulong`.</span></span>

<span data-ttu-id="229bc-213">V nezabezpečeném kontextu sada standardních implicitních převodů ([standardní implicitní převody](conversions.md#standard-implicit-conversions)) obsahuje následující převod ukazatele:</span><span class="sxs-lookup"><span data-stu-id="229bc-213">Finally, in an unsafe context, the set of standard implicit conversions ([Standard implicit conversions](conversions.md#standard-implicit-conversions)) includes the following pointer conversion:</span></span>

*  <span data-ttu-id="229bc-214">Z libovolného *pointer_type* na typ `void*`.</span><span class="sxs-lookup"><span data-stu-id="229bc-214">From any *pointer_type* to the type `void*`.</span></span>

<span data-ttu-id="229bc-215">Převody mezi dvěma typy ukazatelů nikdy nezmění skutečnou hodnotu ukazatele.</span><span class="sxs-lookup"><span data-stu-id="229bc-215">Conversions between two pointer types never change the actual pointer value.</span></span> <span data-ttu-id="229bc-216">Jinými slovy, převod z jednoho typu ukazatele na jiný nemá žádný vliv na podkladovou adresu určenou ukazatelem.</span><span class="sxs-lookup"><span data-stu-id="229bc-216">In other words, a conversion from one pointer type to another has no effect on the underlying address given by the pointer.</span></span>

<span data-ttu-id="229bc-217">Když je jeden typ ukazatele převeden na jiný, pokud výsledný ukazatel není správně zarovnán pro typ Point-to, chování není definováno, pokud je výsledek zpětně odkazován.</span><span class="sxs-lookup"><span data-stu-id="229bc-217">When one pointer type is converted to another, if the resulting pointer is not correctly aligned for the pointed-to type, the behavior is undefined if the result is dereferenced.</span></span> <span data-ttu-id="229bc-218">Obecně platí, že pojem "správně zarovnán" je přenositelný: Pokud je ukazatel na typ `A` správně zarovnán pro ukazatel na typ `B`, který je zase správně zarovnán pro ukazatel na typ `C`, pak je ukazatel na typ `A` správně zarovnán pro ukazatel na typ `C`.</span><span class="sxs-lookup"><span data-stu-id="229bc-218">In general, the concept "correctly aligned" is transitive: if a pointer to type `A` is correctly aligned for a pointer to type `B`, which, in turn, is correctly aligned for a pointer to type `C`, then a pointer to type `A` is correctly aligned for a pointer to type `C`.</span></span>

<span data-ttu-id="229bc-219">Vezměte v úvahu následující případ, ve kterém je k proměnné s jedním typem přistupované prostřednictvím ukazatele na jiný typ:</span><span class="sxs-lookup"><span data-stu-id="229bc-219">Consider the following case in which a variable having one type is accessed via a pointer to a different type:</span></span>

```csharp
char c = 'A';
char* pc = &c;
void* pv = pc;
int* pi = (int*)pv;
int i = *pi;         // undefined
*pi = 123456;        // undefined
```

<span data-ttu-id="229bc-220">Když je typ ukazatele převeden na ukazatel na Byte, výsledek odkazuje na nejnižší adresovaný bajt proměnné.</span><span class="sxs-lookup"><span data-stu-id="229bc-220">When a pointer type is converted to a pointer to byte, the result points to the lowest addressed byte of the variable.</span></span> <span data-ttu-id="229bc-221">Po sobě jdoucí zvýšení výsledku, až po velikost proměnné, vrátí ukazatel na zbývající bajty této proměnné.</span><span class="sxs-lookup"><span data-stu-id="229bc-221">Successive increments of the result, up to the size of the variable, yield pointers to the remaining bytes of that variable.</span></span> <span data-ttu-id="229bc-222">Například následující metoda zobrazí každý z osmi bajtů v hodnotě Double jako šestnáctkovou hodnotu:</span><span class="sxs-lookup"><span data-stu-id="229bc-222">For example, the following method displays each of the eight bytes in a double as a hexadecimal value:</span></span>

```csharp
using System;

class Test
{
    unsafe static void Main() {
      double d = 123.456e23;
        unsafe {
           byte* pb = (byte*)&d;
            for (int i = 0; i < sizeof(double); ++i)
               Console.Write("{0:X2} ", *pb++);
            Console.WriteLine();
        }
    }
}
```

<span data-ttu-id="229bc-223">Vytvářený výstup samozřejmě závisí na endian.</span><span class="sxs-lookup"><span data-stu-id="229bc-223">Of course, the output produced depends on endianness.</span></span>

<span data-ttu-id="229bc-224">Mapování mezi ukazateli a celými čísly jsou definovaná implementací.</span><span class="sxs-lookup"><span data-stu-id="229bc-224">Mappings between pointers and integers are implementation-defined.</span></span> <span data-ttu-id="229bc-225">Nicméně u architektur PROCESORů 32 \* a 64 s lineárním adresním prostorem se převody ukazatelů na nebo z integrálních typů obvykle chovají stejně jako převody `uint` nebo `ulong` hodnot, v uvedeném pořadí, do nebo z těchto integrálních typů.</span><span class="sxs-lookup"><span data-stu-id="229bc-225">However, on 32\* and 64-bit CPU architectures with a linear address space, conversions of pointers to or from integral types typically behave exactly like conversions of `uint` or `ulong` values, respectively, to or from those integral types.</span></span>

### <a name="pointer-arrays"></a><span data-ttu-id="229bc-226">Pole ukazatelů</span><span class="sxs-lookup"><span data-stu-id="229bc-226">Pointer arrays</span></span>

<span data-ttu-id="229bc-227">V nezabezpečeném kontextu mohou být vytvořena pole ukazatelů.</span><span class="sxs-lookup"><span data-stu-id="229bc-227">In an unsafe context, arrays of pointers can be constructed.</span></span> <span data-ttu-id="229bc-228">V polích ukazatelů jsou povoleny pouze některé převody, které platí pro jiné typy polí:</span><span class="sxs-lookup"><span data-stu-id="229bc-228">Only some of the conversions that apply to other array types are allowed on pointer arrays:</span></span>

*  <span data-ttu-id="229bc-229">Implicitní převod odkazu ([implicitní převod odkazů](conversions.md#implicit-reference-conversions)) z libovolného *array_type* na `System.Array` a rozhraní, které implementuje, platí také pro pole ukazatelů.</span><span class="sxs-lookup"><span data-stu-id="229bc-229">The implicit reference conversion ([Implicit reference conversions](conversions.md#implicit-reference-conversions)) from any *array_type* to `System.Array` and the interfaces it implements also applies to pointer arrays.</span></span> <span data-ttu-id="229bc-230">Nicméně jakýkoliv pokus o přístup k prvkům pole pomocí `System.Array` nebo rozhraní, které implementuje, způsobí výjimku za běhu, protože typy ukazatelů nelze převést na `object`.</span><span class="sxs-lookup"><span data-stu-id="229bc-230">However, any attempt to access the array elements through `System.Array` or the interfaces it implements will result in an exception at run-time, as pointer types are not convertible to `object`.</span></span>
*  <span data-ttu-id="229bc-231">Implicitní a explicitní převody odkazů ([implicitní převody odkazů](conversions.md#implicit-reference-conversions), [explicitní převody odkazů](conversions.md#explicit-reference-conversions)) z jednorozměrného typu pole `S[]` na `System.Collections.Generic.IList<T>` a jeho obecné základní rozhraní se nikdy nevztahují na pole ukazatelů, protože typy ukazatelů nejde použít jako argumenty typu a neexistují žádné převody z typů ukazatelů na typy bez ukazatelů.</span><span class="sxs-lookup"><span data-stu-id="229bc-231">The implicit and explicit reference conversions ([Implicit reference conversions](conversions.md#implicit-reference-conversions), [Explicit reference conversions](conversions.md#explicit-reference-conversions)) from a single-dimensional array type `S[]` to `System.Collections.Generic.IList<T>` and its generic base interfaces never apply to pointer arrays, since pointer types cannot be used as type arguments, and there are no conversions from pointer types to non-pointer types.</span></span>
*  <span data-ttu-id="229bc-232">Explicitní převod odkazu ([explicitní převody odkazů](conversions.md#explicit-reference-conversions)) z `System.Array` a rozhraní, které implementuje pro všechny *array_type* , se vztahují na pole ukazatelů.</span><span class="sxs-lookup"><span data-stu-id="229bc-232">The explicit reference conversion ([Explicit reference conversions](conversions.md#explicit-reference-conversions)) from `System.Array` and the interfaces it implements to any *array_type* applies to pointer arrays.</span></span>
*  <span data-ttu-id="229bc-233">Explicitní převody odkazů ([explicitní referenční převody](conversions.md#explicit-reference-conversions)) z `System.Collections.Generic.IList<S>` a jeho základních rozhraní na typ jednorozměrného pole `T[]` se nikdy nevztahují na pole ukazatelů, protože typy ukazatelů nejde použít jako argumenty typu a neexistují žádné převody z typů ukazatelů na typy bez ukazatelů.</span><span class="sxs-lookup"><span data-stu-id="229bc-233">The explicit reference conversions ([Explicit reference conversions](conversions.md#explicit-reference-conversions)) from `System.Collections.Generic.IList<S>` and its base interfaces to a single-dimensional array type `T[]` never applies to pointer arrays, since pointer types cannot be used as type arguments, and there are no conversions from pointer types to non-pointer types.</span></span>

<span data-ttu-id="229bc-234">Tato omezení znamenají, že rozšíření pro příkaz `foreach` nad poli popsanými v [příkazu foreach](statements.md#the-foreach-statement) nelze použít na pole ukazatelů.</span><span class="sxs-lookup"><span data-stu-id="229bc-234">These restrictions mean that the expansion for the `foreach` statement over arrays described in [The foreach statement](statements.md#the-foreach-statement) cannot be applied to pointer arrays.</span></span> <span data-ttu-id="229bc-235">Místo toho příkaz foreach formuláře</span><span class="sxs-lookup"><span data-stu-id="229bc-235">Instead, a foreach statement of the form</span></span>

```csharp
foreach (V v in x) embedded_statement
```

<span data-ttu-id="229bc-236">kde typ `x` je typ pole `T[,,...,]`formuláře, `N` je počet dimenzí minus 1 a `T` nebo `V` je typ ukazatele, rozbalený pomocí vnořených smyček následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="229bc-236">where the type of `x` is an array type of the form `T[,,...,]`, `N` is the number of dimensions minus 1 and `T` or `V` is a pointer type, is expanded using nested for-loops as follows:</span></span>

```csharp
{
    T[,,...,] a = x;
    for (int i0 = a.GetLowerBound(0); i0 <= a.GetUpperBound(0); i0++)
    for (int i1 = a.GetLowerBound(1); i1 <= a.GetUpperBound(1); i1++)
    ...
    for (int iN = a.GetLowerBound(N); iN <= a.GetUpperBound(N); iN++) {
        V v = (V)a.GetValue(i0,i1,...,iN);
        embedded_statement
    }
}
```

<span data-ttu-id="229bc-237">Proměnné `a`, `i0`, `i1`,..., `iN` nejsou viditelné ani dostupné *pro `x` nebo* jiný zdrojový kód programu.</span><span class="sxs-lookup"><span data-stu-id="229bc-237">The variables `a`, `i0`, `i1`, ..., `iN` are not visible to or accessible to `x` or the *embedded_statement* or any other source code of the program.</span></span> <span data-ttu-id="229bc-238">Proměnná `v` v příkazu Embedded jen pro čtení.</span><span class="sxs-lookup"><span data-stu-id="229bc-238">The variable `v` is read-only in the embedded statement.</span></span> <span data-ttu-id="229bc-239">Pokud není k dispozici explicitní převod ([převody ukazatelů](unsafe-code.md#pointer-conversions)) z `T` (typ elementu) na `V`, je vytvořena chyba a nejsou provedeny žádné další kroky.</span><span class="sxs-lookup"><span data-stu-id="229bc-239">If there is not an explicit conversion ([Pointer conversions](unsafe-code.md#pointer-conversions)) from `T` (the element type) to `V`, an error is produced and no further steps are taken.</span></span> <span data-ttu-id="229bc-240">Pokud má `x` hodnotu `null`, je vyvolána `System.NullReferenceException` v době běhu.</span><span class="sxs-lookup"><span data-stu-id="229bc-240">If `x` has the value `null`, a `System.NullReferenceException` is thrown at run-time.</span></span>

## <a name="pointers-in-expressions"></a><span data-ttu-id="229bc-241">Ukazatelé ve výrazech</span><span class="sxs-lookup"><span data-stu-id="229bc-241">Pointers in expressions</span></span>

<span data-ttu-id="229bc-242">V nezabezpečeném kontextu může výraz vracet výsledek typu ukazatele, ale mimo nezabezpečený kontext, jedná se o chybu při kompilaci, aby výraz byl typu ukazatele.</span><span class="sxs-lookup"><span data-stu-id="229bc-242">In an unsafe context, an expression may yield a result of a pointer type, but outside an unsafe context it is a compile-time error for an expression to be of a pointer type.</span></span> <span data-ttu-id="229bc-243">V přesném termínu mimo nezabezpečený kontext dojde k chybě při kompilaci, *pokud simple_name* ([jednoduché názvy](expressions.md#simple-names)), *member_access* ([přístup členů](expressions.md#member-access)), *invocation_expression* ([výrazy vyvolání](expressions.md#invocation-expressions)) nebo *element_access* ([přístup k prvkům](expressions.md#element-access)) je typu ukazatele.</span><span class="sxs-lookup"><span data-stu-id="229bc-243">In precise terms, outside an unsafe context a compile-time error occurs if any *simple_name* ([Simple names](expressions.md#simple-names)), *member_access* ([Member access](expressions.md#member-access)), *invocation_expression* ([Invocation expressions](expressions.md#invocation-expressions)), or *element_access* ([Element access](expressions.md#element-access)) is of a pointer type.</span></span>

<span data-ttu-id="229bc-244">V nezabezpečeném kontextu, *primary_no_array_creation_expression* ([primární výrazy](expressions.md#primary-expressions)) a *unary_expression* ([unární operátory](expressions.md#unary-operators)), umožňují následující dodatečné konstrukce:</span><span class="sxs-lookup"><span data-stu-id="229bc-244">In an unsafe context, the *primary_no_array_creation_expression* ([Primary expressions](expressions.md#primary-expressions)) and *unary_expression* ([Unary operators](expressions.md#unary-operators)) productions permit the following additional constructs:</span></span>

```antlr
primary_no_array_creation_expression_unsafe
    : pointer_member_access
    | pointer_element_access
    | sizeof_expression
    ;

unary_expression_unsafe
    : pointer_indirection_expression
    | addressof_expression
    ;
```

<span data-ttu-id="229bc-245">Tyto konstrukce jsou popsány v následujících částech.</span><span class="sxs-lookup"><span data-stu-id="229bc-245">These constructs are described in the following sections.</span></span> <span data-ttu-id="229bc-246">Přednost a asociativita nebezpečných operátorů je odvozena gramatikou.</span><span class="sxs-lookup"><span data-stu-id="229bc-246">The precedence and associativity of the unsafe operators is implied by the grammar.</span></span>

### <a name="pointer-indirection"></a><span data-ttu-id="229bc-247">Indirekce ukazatele</span><span class="sxs-lookup"><span data-stu-id="229bc-247">Pointer indirection</span></span>

<span data-ttu-id="229bc-248">*Pointer_indirection_expression* se skládá z hvězdičky (`*`) následovaných *unary_expression*.</span><span class="sxs-lookup"><span data-stu-id="229bc-248">A *pointer_indirection_expression* consists of an asterisk (`*`) followed by a *unary_expression*.</span></span>

```antlr
pointer_indirection_expression
    : '*' unary_expression
    ;
```

<span data-ttu-id="229bc-249">Unární operátor `*` označuje nepřímý odkaz na ukazatel a používá se k získání proměnné, na kterou ukazatel ukazuje.</span><span class="sxs-lookup"><span data-stu-id="229bc-249">The unary `*` operator denotes pointer indirection and is used to obtain the variable to which a pointer points.</span></span> <span data-ttu-id="229bc-250">Výsledek vyhodnocení `*P`, kde `P` je výraz typu ukazatele `T*`, je proměnná typu `T`.</span><span class="sxs-lookup"><span data-stu-id="229bc-250">The result of evaluating `*P`, where `P` is an expression of a pointer type `T*`, is a variable of type `T`.</span></span> <span data-ttu-id="229bc-251">Jedná se o chybu při kompilaci pro použití unárního operátoru `*` na výraz typu `void*` nebo na výraz, který není typu ukazatele.</span><span class="sxs-lookup"><span data-stu-id="229bc-251">It is a compile-time error to apply the unary `*` operator to an expression of type `void*` or to an expression that isn't of a pointer type.</span></span>

<span data-ttu-id="229bc-252">Účinek použití unárního operátoru `*` na ukazatel `null` je definován implementací.</span><span class="sxs-lookup"><span data-stu-id="229bc-252">The effect of applying the unary `*` operator to a `null` pointer is implementation-defined.</span></span> <span data-ttu-id="229bc-253">Konkrétně není nijak zaručeno, že tato operace vyvolá `System.NullReferenceException`.</span><span class="sxs-lookup"><span data-stu-id="229bc-253">In particular, there is no guarantee that this operation throws a `System.NullReferenceException`.</span></span>

<span data-ttu-id="229bc-254">Pokud je k ukazateli přiřazena neplatná hodnota, chování unárního operátoru `*` není definováno.</span><span class="sxs-lookup"><span data-stu-id="229bc-254">If an invalid value has been assigned to the pointer, the behavior of the unary `*` operator is undefined.</span></span> <span data-ttu-id="229bc-255">Mezi neplatné hodnoty pro přesměrování ukazatele pomocí unárního operátoru `*` je adresa nevhodně zarovnána na typ, na který se odkazuje (viz příklad [Převod ukazatelů](unsafe-code.md#pointer-conversions)), a adresu proměnné po konci své životnosti.</span><span class="sxs-lookup"><span data-stu-id="229bc-255">Among the invalid values for dereferencing a pointer by the unary `*` operator are an address inappropriately aligned for the type pointed to (see example in [Pointer conversions](unsafe-code.md#pointer-conversions)), and the address of a variable after the end of its lifetime.</span></span>

<span data-ttu-id="229bc-256">Pro účely analýzy jednoznačného přiřazení je proměnná vytvořená pomocí vyhodnocení výrazu `*P` formuláře považována za původně přiřazenou ([původně přiřazené proměnné](variables.md#initially-assigned-variables)).</span><span class="sxs-lookup"><span data-stu-id="229bc-256">For purposes of definite assignment analysis, a variable produced by evaluating an expression of the form `*P` is considered initially assigned ([Initially assigned variables](variables.md#initially-assigned-variables)).</span></span>

### <a name="pointer-member-access"></a><span data-ttu-id="229bc-257">Přístup ke členu ukazatele</span><span class="sxs-lookup"><span data-stu-id="229bc-257">Pointer member access</span></span>

<span data-ttu-id="229bc-258">*Pointer_member_access* se skládá z *primary_expression*, po kterém následuje token "`->`" následovaný *identifikátorem* a nepovinným *type_argument_list*.</span><span class="sxs-lookup"><span data-stu-id="229bc-258">A *pointer_member_access* consists of a *primary_expression*, followed by a "`->`" token, followed by an *identifier* and an optional *type_argument_list*.</span></span>

```antlr
pointer_member_access
    : primary_expression '->' identifier
    ;
```

<span data-ttu-id="229bc-259">V přístupu člena ukazatele na `P->I`formuláře musí být `P` výrazem jiného typu ukazatele než `void*`a `I` musí poznamenat přístupný člen typu, na který `P` body.</span><span class="sxs-lookup"><span data-stu-id="229bc-259">In a pointer member access of the form `P->I`, `P` must be an expression of a pointer type other than `void*`, and `I` must denote an accessible member of the type to which `P` points.</span></span>

<span data-ttu-id="229bc-260">Přístup člena ukazatele do formuláře `P->I` je vyhodnocen přesně jako `(*P).I`.</span><span class="sxs-lookup"><span data-stu-id="229bc-260">A pointer member access of the form `P->I` is evaluated exactly as `(*P).I`.</span></span> <span data-ttu-id="229bc-261">Popis operátoru indirekce ukazatele (`*`) naleznete v tématu [dereference ukazatele](unsafe-code.md#pointer-indirection).</span><span class="sxs-lookup"><span data-stu-id="229bc-261">For a description of the pointer indirection operator (`*`), see [Pointer indirection](unsafe-code.md#pointer-indirection).</span></span> <span data-ttu-id="229bc-262">Popis operátoru přístupu ke členu (`.`) naleznete v tématu [Member Access](expressions.md#member-access).</span><span class="sxs-lookup"><span data-stu-id="229bc-262">For a description of the member access operator (`.`), see [Member access](expressions.md#member-access).</span></span>

<span data-ttu-id="229bc-263">V příkladu</span><span class="sxs-lookup"><span data-stu-id="229bc-263">In the example</span></span>

```csharp
using System;

struct Point
{
    public int x;
    public int y;

    public override string ToString() {
        return "(" + x + "," + y + ")";
    }
}

class Test
{
    static void Main() {
        Point point;
        unsafe {
            Point* p = &point;
            p->x = 10;
            p->y = 20;
            Console.WriteLine(p->ToString());
        }
    }
}
```

<span data-ttu-id="229bc-264">operátor `->` se používá pro přístup k polím a vyvolání metody struktury prostřednictvím ukazatele.</span><span class="sxs-lookup"><span data-stu-id="229bc-264">the `->` operator is used to access fields and invoke a method of a struct through a pointer.</span></span> <span data-ttu-id="229bc-265">Vzhledem k tomu, že je operace `P->I` přesně rovnocenná `(*P).I`, může být metoda `Main` zapsána stejně jako:</span><span class="sxs-lookup"><span data-stu-id="229bc-265">Because the operation `P->I` is precisely equivalent to `(*P).I`, the `Main` method could equally well have been written:</span></span>

```csharp
class Test
{
    static void Main() {
        Point point;
        unsafe {
            Point* p = &point;
            (*p).x = 10;
            (*p).y = 20;
            Console.WriteLine((*p).ToString());
        }
    }
}
```

### <a name="pointer-element-access"></a><span data-ttu-id="229bc-266">Přístup k prvku ukazatele</span><span class="sxs-lookup"><span data-stu-id="229bc-266">Pointer element access</span></span>

<span data-ttu-id="229bc-267">*Pointer_element_access* se skládá z *primary_no_array_creation_expression* následovaného výrazem uzavřeným v části "`[`" a "`]`".</span><span class="sxs-lookup"><span data-stu-id="229bc-267">A *pointer_element_access* consists of a *primary_no_array_creation_expression* followed by an expression enclosed in "`[`" and "`]`".</span></span>

```antlr
pointer_element_access
    : primary_no_array_creation_expression '[' expression ']'
    ;
```

<span data-ttu-id="229bc-268">V prvku ukazatele přístup k `P[E]`formuláře musí být `P` výrazem jiného typu ukazatele než `void*`a `E` musí být výraz, který lze implicitně převést na `int`, `uint`, `long`nebo `ulong`.</span><span class="sxs-lookup"><span data-stu-id="229bc-268">In a pointer element access of the form `P[E]`, `P` must be an expression of a pointer type other than `void*`, and `E` must be an expression that can be implicitly converted to `int`, `uint`, `long`, or `ulong`.</span></span>

<span data-ttu-id="229bc-269">Přístup k prvku `P[E]` formuláře je vyhodnocen přesně jako `*(P + E)`.</span><span class="sxs-lookup"><span data-stu-id="229bc-269">A pointer element access of the form `P[E]` is evaluated exactly as `*(P + E)`.</span></span> <span data-ttu-id="229bc-270">Popis operátoru indirekce ukazatele (`*`) naleznete v tématu [dereference ukazatele](unsafe-code.md#pointer-indirection).</span><span class="sxs-lookup"><span data-stu-id="229bc-270">For a description of the pointer indirection operator (`*`), see [Pointer indirection](unsafe-code.md#pointer-indirection).</span></span> <span data-ttu-id="229bc-271">Popis operátoru sčítání ukazatelů (`+`) naleznete v tématu [aritmetický ukazatel](unsafe-code.md#pointer-arithmetic).</span><span class="sxs-lookup"><span data-stu-id="229bc-271">For a description of the pointer addition operator (`+`), see [Pointer arithmetic](unsafe-code.md#pointer-arithmetic).</span></span>

<span data-ttu-id="229bc-272">V příkladu</span><span class="sxs-lookup"><span data-stu-id="229bc-272">In the example</span></span>

```csharp
class Test
{
    static void Main() {
        unsafe {
            char* p = stackalloc char[256];
            for (int i = 0; i < 256; i++) p[i] = (char)i;
        }
    }
}
```

<span data-ttu-id="229bc-273">přístup k prvku ukazatele se používá k inicializaci vyrovnávací paměti znaků ve smyčce `for`.</span><span class="sxs-lookup"><span data-stu-id="229bc-273">a pointer element access is used to initialize the character buffer in a `for` loop.</span></span> <span data-ttu-id="229bc-274">Vzhledem k tomu, že je operace `P[E]` přesně rovnocenná `*(P + E)`, může být příklad stejně dobře zapsán:</span><span class="sxs-lookup"><span data-stu-id="229bc-274">Because the operation `P[E]` is precisely equivalent to `*(P + E)`, the example could equally well have been written:</span></span>

```csharp
class Test
{
    static void Main() {
        unsafe {
            char* p = stackalloc char[256];
            for (int i = 0; i < 256; i++) *(p + i) = (char)i;
        }
    }
}
```

<span data-ttu-id="229bc-275">Operátor přístupu k elementu ukazatele nekontroluje chyby mimo hranice a chování při přístupu k elementu, který je mimo rozsah, není definované.</span><span class="sxs-lookup"><span data-stu-id="229bc-275">The pointer element access operator does not check for out-of-bounds errors and the behavior when accessing an out-of-bounds element is undefined.</span></span> <span data-ttu-id="229bc-276">To je stejné jako v jazyce C C++a.</span><span class="sxs-lookup"><span data-stu-id="229bc-276">This is the same as C and C++.</span></span>

### <a name="the-address-of-operator"></a><span data-ttu-id="229bc-277">Operátor address-of</span><span class="sxs-lookup"><span data-stu-id="229bc-277">The address-of operator</span></span>

<span data-ttu-id="229bc-278">*Addressof_expression* se skládá ampersand (`&`) následovaný *unary_expression*.</span><span class="sxs-lookup"><span data-stu-id="229bc-278">An *addressof_expression* consists of an ampersand (`&`) followed by a *unary_expression*.</span></span>

```antlr
addressof_expression
    : '&' unary_expression
    ;
```

<span data-ttu-id="229bc-279">Vzhledem k `E` výrazu, který je typu `T` a je klasifikován jako pevná proměnná ([pevné a pohyblivé proměnné](unsafe-code.md#fixed-and-moveable-variables)), konstrukce `&E` vypočítá adresu proměnné dané `E`.</span><span class="sxs-lookup"><span data-stu-id="229bc-279">Given an expression `E` which is of a type `T` and is classified as a fixed variable ([Fixed and moveable variables](unsafe-code.md#fixed-and-moveable-variables)), the construct `&E` computes the address of the variable given by `E`.</span></span> <span data-ttu-id="229bc-280">Typ výsledku je `T*` a je klasifikován jako hodnota.</span><span class="sxs-lookup"><span data-stu-id="229bc-280">The type of the result is `T*` and is classified as a value.</span></span> <span data-ttu-id="229bc-281">K chybě při kompilaci dojde v případě, že `E` není klasifikován jako proměnná, pokud je `E` klasifikován jako místní proměnná jen pro čtení, nebo pokud `E` označuje pohyblivou proměnnou.</span><span class="sxs-lookup"><span data-stu-id="229bc-281">A compile-time error occurs if `E` is not classified as a variable, if `E` is classified as a read-only local variable, or if `E` denotes a moveable variable.</span></span> <span data-ttu-id="229bc-282">V posledním případě lze pomocí příkazu fixed ([příkaz fixed](unsafe-code.md#the-fixed-statement)) dočasně "opravit" proměnnou před získáním její adresy.</span><span class="sxs-lookup"><span data-stu-id="229bc-282">In the last case, a fixed statement ([The fixed statement](unsafe-code.md#the-fixed-statement)) can be used to temporarily "fix" the variable before obtaining its address.</span></span> <span data-ttu-id="229bc-283">Jak je uvedeno v [přístupu ke členu](expressions.md#member-access), mimo konstruktor instance nebo statický konstruktor pro strukturu nebo třídu, která definuje `readonly` pole, toto pole je považováno za hodnotu, nikoli proměnnou.</span><span class="sxs-lookup"><span data-stu-id="229bc-283">As stated in [Member access](expressions.md#member-access), outside an instance constructor or static constructor for a struct or class that defines a `readonly` field, that field is considered a value, not a variable.</span></span> <span data-ttu-id="229bc-284">V takovém případě se adresa nedá vzít.</span><span class="sxs-lookup"><span data-stu-id="229bc-284">As such, its address cannot be taken.</span></span> <span data-ttu-id="229bc-285">Obdobně nelze adresu konstanty považovat.</span><span class="sxs-lookup"><span data-stu-id="229bc-285">Similarly, the address of a constant cannot be taken.</span></span>

<span data-ttu-id="229bc-286">Operátor `&` nepožaduje, aby byl jeho argument jednoznačně přiřazen, ale za `&` operace je proměnná, na kterou je operátor použit, považována za jednoznačně přiřazenou v cestě provádění, ve které k operaci dojde.</span><span class="sxs-lookup"><span data-stu-id="229bc-286">The `&` operator does not require its argument to be definitely assigned, but following an `&` operation, the variable to which the operator is applied is considered definitely assigned in the execution path in which the operation occurs.</span></span> <span data-ttu-id="229bc-287">Je zodpovědností programátora, aby se zajistilo, že v této situaci bude provedeno správné inicializaci proměnné.</span><span class="sxs-lookup"><span data-stu-id="229bc-287">It is the responsibility of the programmer to ensure that correct initialization of the variable actually does take place in this situation.</span></span>

<span data-ttu-id="229bc-288">V příkladu</span><span class="sxs-lookup"><span data-stu-id="229bc-288">In the example</span></span>

```csharp
using System;

class Test
{
    static void Main() {
        int i;
        unsafe {
            int* p = &i;
            *p = 123;
        }
        Console.WriteLine(i);
    }
}
```

<span data-ttu-id="229bc-289">`i` se považuje za jednoznačně přiřazenou po `&i` operaci použitou k inicializaci `p`.</span><span class="sxs-lookup"><span data-stu-id="229bc-289">`i` is considered definitely assigned following the `&i` operation used to initialize `p`.</span></span> <span data-ttu-id="229bc-290">Přiřazení `*p` v účinnosti inicializuje `i`, ale zahrnutí této inicializace je zodpovědností programátora a při odebrání přiřazení by nedocházelo k žádné chybě v době kompilace.</span><span class="sxs-lookup"><span data-stu-id="229bc-290">The assignment to `*p` in effect initializes `i`, but the inclusion of this initialization is the responsibility of the programmer, and no compile-time error would occur if the assignment was removed.</span></span>

<span data-ttu-id="229bc-291">Pravidla jednoznačného přiřazení pro operátor `&` existují tak, aby bylo možné zabránit redundantní inicializaci místních proměnných.</span><span class="sxs-lookup"><span data-stu-id="229bc-291">The rules of definite assignment for the `&` operator exist such that redundant initialization of local variables can be avoided.</span></span> <span data-ttu-id="229bc-292">Mnoho externích rozhraní API například převezme ukazatel na strukturu, která je vyplněna rozhraním API.</span><span class="sxs-lookup"><span data-stu-id="229bc-292">For example, many external APIs take a pointer to a structure which is filled in by the API.</span></span> <span data-ttu-id="229bc-293">Volání těchto rozhraní API obvykle předávají adresu místní proměnné struktury a bez pravidla, která by vyžadovala redundantní inicializaci proměnné struct.</span><span class="sxs-lookup"><span data-stu-id="229bc-293">Calls to such APIs typically pass the address of a local struct variable, and without the rule, redundant initialization of the struct variable would be required.</span></span>

### <a name="pointer-increment-and-decrement"></a><span data-ttu-id="229bc-294">Zvýšení a snížení ukazatele</span><span class="sxs-lookup"><span data-stu-id="229bc-294">Pointer increment and decrement</span></span>

<span data-ttu-id="229bc-295">V nezabezpečeném kontextu lze použít operátory `++` a `--` (operátory[přírůstku a snížení](expressions.md#postfix-increment-and-decrement-operators) [předpony a modifikátory zvýšení a snížení předpony](expressions.md#prefix-increment-and-decrement-operators)), s výjimkou `void*`.</span><span class="sxs-lookup"><span data-stu-id="229bc-295">In an unsafe context, the `++` and `--` operators ([Postfix increment and decrement operators](expressions.md#postfix-increment-and-decrement-operators) and [Prefix increment and decrement operators](expressions.md#prefix-increment-and-decrement-operators)) can be applied to pointer variables of all types except `void*`.</span></span> <span data-ttu-id="229bc-296">Proto pro každý typ ukazatele `T*`jsou implicitně definovány následující operátory:</span><span class="sxs-lookup"><span data-stu-id="229bc-296">Thus, for every pointer type `T*`, the following operators are implicitly defined:</span></span>

```csharp
T* operator ++(T* x);
T* operator --(T* x);
```

<span data-ttu-id="229bc-297">Operátory vytváří stejné výsledky jako `x + 1` a `x - 1`, v uvedeném pořadí ([aritmetický ukazatel](unsafe-code.md#pointer-arithmetic)).</span><span class="sxs-lookup"><span data-stu-id="229bc-297">The operators produce the same results as `x + 1` and `x - 1`, respectively ([Pointer arithmetic](unsafe-code.md#pointer-arithmetic)).</span></span> <span data-ttu-id="229bc-298">Jinými slovy, operátor `++` pro proměnnou typu `T*`přidá `sizeof(T)` na adresu obsaženou v proměnné a operátor `--` odečte `sizeof(T)` od adresy obsažené v proměnné.</span><span class="sxs-lookup"><span data-stu-id="229bc-298">In other words, for a pointer variable of type `T*`, the `++` operator adds `sizeof(T)` to the address contained in the variable, and the `--` operator subtracts `sizeof(T)` from the address contained in the variable.</span></span>

<span data-ttu-id="229bc-299">Pokud operace zvýšení nebo snížení ukazatele přetéká v doméně typu ukazatele, výsledek je definován implementací, ale nejsou vyprodukovány žádné výjimky.</span><span class="sxs-lookup"><span data-stu-id="229bc-299">If a pointer increment or decrement operation overflows the domain of the pointer type, the result is implementation-defined, but no exceptions are produced.</span></span>

### <a name="pointer-arithmetic"></a><span data-ttu-id="229bc-300">Aritmetika ukazatele</span><span class="sxs-lookup"><span data-stu-id="229bc-300">Pointer arithmetic</span></span>

<span data-ttu-id="229bc-301">V nezabezpečeném kontextu lze použít operátory `+` a `-` ([operátor sčítání](expressions.md#addition-operator) a [odčítání](expressions.md#subtraction-operator)) na hodnoty všech typů ukazatelů s výjimkou `void*`.</span><span class="sxs-lookup"><span data-stu-id="229bc-301">In an unsafe context, the `+` and `-` operators ([Addition operator](expressions.md#addition-operator) and [Subtraction operator](expressions.md#subtraction-operator)) can be applied to values of all pointer types except `void*`.</span></span> <span data-ttu-id="229bc-302">Proto pro každý typ ukazatele `T*`jsou implicitně definovány následující operátory:</span><span class="sxs-lookup"><span data-stu-id="229bc-302">Thus, for every pointer type `T*`, the following operators are implicitly defined:</span></span>

```csharp
T* operator +(T* x, int y);
T* operator +(T* x, uint y);
T* operator +(T* x, long y);
T* operator +(T* x, ulong y);

T* operator +(int x, T* y);
T* operator +(uint x, T* y);
T* operator +(long x, T* y);
T* operator +(ulong x, T* y);

T* operator -(T* x, int y);
T* operator -(T* x, uint y);
T* operator -(T* x, long y);
T* operator -(T* x, ulong y);

long operator -(T* x, T* y);
```

<span data-ttu-id="229bc-303">Vzhledem k `P` výrazu `T*` typu ukazatele a výrazu `N` typu `int`, `uint`, `long`nebo `ulong`, výrazy `P + N` a `N + P` výpočet hodnoty ukazatele typu `T*`, který je výsledkem přidání `N * sizeof(T)` k adrese uvedené `P`.</span><span class="sxs-lookup"><span data-stu-id="229bc-303">Given an expression `P` of a pointer type `T*` and an expression `N` of type `int`, `uint`, `long`, or `ulong`, the expressions `P + N` and `N + P` compute the pointer value of type `T*` that results from adding `N * sizeof(T)` to the address given by `P`.</span></span> <span data-ttu-id="229bc-304">Podobně výraz `P - N` vypočítá hodnotu ukazatele typu `T*`, která je výsledkem odčítání `N * sizeof(T)` od adresy zadané `P`.</span><span class="sxs-lookup"><span data-stu-id="229bc-304">Likewise, the expression `P - N` computes the pointer value of type `T*` that results from subtracting `N * sizeof(T)` from the address given by `P`.</span></span>

<span data-ttu-id="229bc-305">Zadané dva výrazy, `P` a `Q`typu ukazatele `T*`, výraz `P - Q` vypočítá rozdíl mezi adresami zadanými `P` a `Q` a potom tento rozdíl vydělí `sizeof(T)`.</span><span class="sxs-lookup"><span data-stu-id="229bc-305">Given two expressions, `P` and `Q`, of a pointer type `T*`, the expression `P - Q` computes the difference between the addresses given by `P` and `Q` and then divides that difference by `sizeof(T)`.</span></span> <span data-ttu-id="229bc-306">Typ výsledku je vždy `long`.</span><span class="sxs-lookup"><span data-stu-id="229bc-306">The type of the result is always `long`.</span></span> <span data-ttu-id="229bc-307">V důsledku toho se `P - Q` počítá jako `((long)(P) - (long)(Q)) / sizeof(T)`.</span><span class="sxs-lookup"><span data-stu-id="229bc-307">In effect, `P - Q` is computed as `((long)(P) - (long)(Q)) / sizeof(T)`.</span></span>

<span data-ttu-id="229bc-308">Příklad:</span><span class="sxs-lookup"><span data-stu-id="229bc-308">For example:</span></span>

```csharp
using System;

class Test
{
    static void Main() {
        unsafe {
            int* values = stackalloc int[20];
            int* p = &values[1];
            int* q = &values[15];
            Console.WriteLine("p - q = {0}", p - q);
            Console.WriteLine("q - p = {0}", q - p);
        }
    }
}
```

<span data-ttu-id="229bc-309">který vytváří výstup:</span><span class="sxs-lookup"><span data-stu-id="229bc-309">which produces the output:</span></span>

```console
p - q = -14
q - p = 14
```

<span data-ttu-id="229bc-310">Pokud aritmetická operace ukazatele přetéká doménu typu ukazatele, výsledek je zkrácen v způsobem definovaném implementací, ale žádné výjimky se nevytvoří.</span><span class="sxs-lookup"><span data-stu-id="229bc-310">If a pointer arithmetic operation overflows the domain of the pointer type, the result is truncated in an implementation-defined fashion, but no exceptions are produced.</span></span>

### <a name="pointer-comparison"></a><span data-ttu-id="229bc-311">Porovnání ukazatelů</span><span class="sxs-lookup"><span data-stu-id="229bc-311">Pointer comparison</span></span>

<span data-ttu-id="229bc-312">V nezabezpečeném kontextu lze použít operátory `==`, `!=`, `<`, `>`, `<=`a `=>` ([relační a operátor testování typů](expressions.md#relational-and-type-testing-operators)) pro hodnoty všech typů ukazatelů.</span><span class="sxs-lookup"><span data-stu-id="229bc-312">In an unsafe context, the `==`, `!=`, `<`, `>`, `<=`, and `=>` operators ([Relational and type-testing operators](expressions.md#relational-and-type-testing-operators)) can be applied to values of all pointer types.</span></span> <span data-ttu-id="229bc-313">Operátory porovnání ukazatelů jsou:</span><span class="sxs-lookup"><span data-stu-id="229bc-313">The pointer comparison operators are:</span></span>

```csharp
bool operator ==(void* x, void* y);
bool operator !=(void* x, void* y);
bool operator <(void* x, void* y);
bool operator >(void* x, void* y);
bool operator <=(void* x, void* y);
bool operator >=(void* x, void* y);
```

<span data-ttu-id="229bc-314">Vzhledem k tomu, že implicitní převod existuje z libovolného typu ukazatele na typ `void*`, lze pomocí těchto operátorů porovnat operandy libovolného typu ukazatele.</span><span class="sxs-lookup"><span data-stu-id="229bc-314">Because an implicit conversion exists from any pointer type to the `void*` type, operands of any pointer type can be compared using these operators.</span></span> <span data-ttu-id="229bc-315">Relační operátory porovnávají adresy zadané pomocí dvou operandů, jako kdyby byla celá čísla bez znaménka.</span><span class="sxs-lookup"><span data-stu-id="229bc-315">The comparison operators compare the addresses given by the two operands as if they were unsigned integers.</span></span>

### <a name="the-sizeof-operator"></a><span data-ttu-id="229bc-316">Operátor sizeof</span><span class="sxs-lookup"><span data-stu-id="229bc-316">The sizeof operator</span></span>

<span data-ttu-id="229bc-317">Operátor `sizeof` vrátí počet bajtů obsazených proměnnou daného typu.</span><span class="sxs-lookup"><span data-stu-id="229bc-317">The `sizeof` operator returns the number of bytes occupied by a variable of a given type.</span></span> <span data-ttu-id="229bc-318">Typ zadaný jako operand pro `sizeof` musí být *unmanaged_type* ([typy ukazatelů](unsafe-code.md#pointer-types)).</span><span class="sxs-lookup"><span data-stu-id="229bc-318">The type specified as an operand to `sizeof` must be an *unmanaged_type* ([Pointer types](unsafe-code.md#pointer-types)).</span></span>

```antlr
sizeof_expression
    : 'sizeof' '(' unmanaged_type ')'
    ;
```

<span data-ttu-id="229bc-319">Výsledek operátoru `sizeof` je hodnota typu `int`.</span><span class="sxs-lookup"><span data-stu-id="229bc-319">The result of the `sizeof` operator is a value of type `int`.</span></span> <span data-ttu-id="229bc-320">Pro určité předdefinované typy operátor `sizeof` vrací konstantní hodnotu, jak je znázorněno v následující tabulce.</span><span class="sxs-lookup"><span data-stu-id="229bc-320">For certain predefined types, the `sizeof` operator yields a constant value as shown in the table below.</span></span>


| <span data-ttu-id="229bc-321">__Vyjádření__</span><span class="sxs-lookup"><span data-stu-id="229bc-321">__Expression__</span></span>   | <span data-ttu-id="229bc-322">__výsledek__</span><span class="sxs-lookup"><span data-stu-id="229bc-322">__Result__</span></span> |
|------------------|------------|
| `sizeof(sbyte)`  | `1`        |
| `sizeof(byte)`   | `1`        |
| `sizeof(short)`  | `2`        |
| `sizeof(ushort)` | `2`        |
| `sizeof(int)`    | `4`        |
| `sizeof(uint)`   | `4`        |
| `sizeof(long)`   | `8`        |
| `sizeof(ulong)`  | `8`        |
| `sizeof(char)`   | `2`        |
| `sizeof(float)`  | `4`        |
| `sizeof(double)` | `8`        |
| `sizeof(bool)`   | `1`        |

<span data-ttu-id="229bc-323">Pro všechny ostatní typy je výsledkem operátoru `sizeof` definovaná implementace a je klasifikována jako hodnota, nikoli konstanta.</span><span class="sxs-lookup"><span data-stu-id="229bc-323">For all other types, the result of the `sizeof` operator is implementation-defined and is classified as a value, not a constant.</span></span>

<span data-ttu-id="229bc-324">Pořadí, ve kterém jsou členové zabaleni do struktury, není určeno.</span><span class="sxs-lookup"><span data-stu-id="229bc-324">The order in which members are packed into a struct is unspecified.</span></span>

<span data-ttu-id="229bc-325">Pro účely zarovnání může být nepojmenované odsazení na začátku struktury, v rámci struktury a na konci struktury.</span><span class="sxs-lookup"><span data-stu-id="229bc-325">For alignment purposes, there may be unnamed padding at the beginning of a struct, within a struct, and at the end of the struct.</span></span> <span data-ttu-id="229bc-326">Obsah bitů použitý jako výplň je neurčitý.</span><span class="sxs-lookup"><span data-stu-id="229bc-326">The contents of the bits used as padding are indeterminate.</span></span>

<span data-ttu-id="229bc-327">Při použití na operand, který má typ struktury, je výsledkem celkový počet bajtů v proměnné tohoto typu, včetně jakéhokoli odsazení.</span><span class="sxs-lookup"><span data-stu-id="229bc-327">When applied to an operand that has struct type, the result is the total number of bytes in a variable of that type, including any padding.</span></span>

## <a name="the-fixed-statement"></a><span data-ttu-id="229bc-328">Příkaz fixed</span><span class="sxs-lookup"><span data-stu-id="229bc-328">The fixed statement</span></span>

<span data-ttu-id="229bc-329">V nezabezpečeném kontextu produkční prostředí *embedded_statement* ([příkazy](statements.md)) umožňuje další konstrukci, příkaz `fixed`, který se používá k "opravě" pohyblivé proměnné, aby její adresa zůstala konstantní po dobu trvání příkazu.</span><span class="sxs-lookup"><span data-stu-id="229bc-329">In an unsafe context, the *embedded_statement* ([Statements](statements.md)) production permits an additional construct, the `fixed` statement, which is used to "fix" a moveable variable such that its address remains constant for the duration of the statement.</span></span>

```antlr
fixed_statement
    : 'fixed' '(' pointer_type fixed_pointer_declarators ')' embedded_statement
    ;

fixed_pointer_declarators
    : fixed_pointer_declarator (','  fixed_pointer_declarator)*
    ;

fixed_pointer_declarator
    : identifier '=' fixed_pointer_initializer
    ;

fixed_pointer_initializer
    : '&' variable_reference
    | expression
    ;
```

<span data-ttu-id="229bc-330">Každý *fixed_pointer_declarator* deklaruje místní proměnnou daného *pointer_type* a inicializuje tuto místní proměnnou s adresou vypočítanou odpovídajícím *fixed_pointer_initializer*.</span><span class="sxs-lookup"><span data-stu-id="229bc-330">Each *fixed_pointer_declarator* declares a local variable of the given *pointer_type* and initializes that local variable with the address computed by the corresponding *fixed_pointer_initializer*.</span></span> <span data-ttu-id="229bc-331">Lokální proměnná deklarovaná v příkazu `fixed` je přístupná v jakémkoli *fixed_pointer_initializer*, ke kterým dochází napravo od deklarace proměnné, a v *embedded_statement* příkazu `fixed`.</span><span class="sxs-lookup"><span data-stu-id="229bc-331">A local variable declared in a `fixed` statement is accessible in any *fixed_pointer_initializer*s occurring to the right of that variable's declaration, and in the *embedded_statement* of the `fixed` statement.</span></span> <span data-ttu-id="229bc-332">Lokální proměnná deklarovaná příkazem `fixed` je považována za jen pro čtení.</span><span class="sxs-lookup"><span data-stu-id="229bc-332">A local variable declared by a `fixed` statement is considered read-only.</span></span> <span data-ttu-id="229bc-333">K chybě při kompilaci dojde v případě, že se vložený příkaz pokusí změnit tuto místní proměnnou (prostřednictvím přiřazení nebo operátorů `++` a `--`) nebo předat jako parametr `ref` nebo `out`.</span><span class="sxs-lookup"><span data-stu-id="229bc-333">A compile-time error occurs if the embedded statement attempts to modify this local variable (via assignment or the `++` and `--` operators) or pass it as a `ref` or `out` parameter.</span></span>

<span data-ttu-id="229bc-334">*Fixed_pointer_initializer* může být jedna z následujících:</span><span class="sxs-lookup"><span data-stu-id="229bc-334">A *fixed_pointer_initializer* can be one of the following:</span></span>

*  <span data-ttu-id="229bc-335">Token "`&`" následovaný *variable_reference* ([přesné pravidlo pro stanovení jednoznačného přiřazení](variables.md#precise-rules-for-determining-definite-assignment)) k pohyblivé proměnné ([pevné a mobilní proměnné](unsafe-code.md#fixed-and-moveable-variables)) nespravovaného typu `T`, za předpokladu, že je typ `T*` implicitně převoditelný na typ ukazatele uvedený v příkazu `fixed`.</span><span class="sxs-lookup"><span data-stu-id="229bc-335">The token "`&`" followed by a *variable_reference* ([Precise rules for determining definite assignment](variables.md#precise-rules-for-determining-definite-assignment)) to a moveable variable ([Fixed and moveable variables](unsafe-code.md#fixed-and-moveable-variables)) of an unmanaged type `T`, provided the type `T*` is implicitly convertible to the pointer type given in the `fixed` statement.</span></span> <span data-ttu-id="229bc-336">V tomto případě inicializátor vypočítá adresu dané proměnné a proměnná je zaručena, aby zůstala na pevné adrese po dobu trvání příkazu `fixed`.</span><span class="sxs-lookup"><span data-stu-id="229bc-336">In this case, the initializer computes the address of the given variable, and the variable is guaranteed to remain at a fixed address for the duration of the `fixed` statement.</span></span>
*  <span data-ttu-id="229bc-337">Výraz *array_type* s elementy nespravovaného typu `T`za předpokladu, že typ `T*` je implicitně převoditelné na typ ukazatele uvedený v příkazu `fixed`.</span><span class="sxs-lookup"><span data-stu-id="229bc-337">An expression of an *array_type* with elements of an unmanaged type `T`, provided the type `T*` is implicitly convertible to the pointer type given in the `fixed` statement.</span></span> <span data-ttu-id="229bc-338">V tomto případě inicializátor vypočítá adresu prvního prvku v poli a celé pole je zaručeno, že zůstane na pevné adrese po dobu trvání `fixed`ho příkazu.</span><span class="sxs-lookup"><span data-stu-id="229bc-338">In this case, the initializer computes the address of the first element in the array, and the entire array is guaranteed to remain at a fixed address for the duration of the `fixed` statement.</span></span> <span data-ttu-id="229bc-339">Pokud je výraz pole null nebo pokud má pole nulové prvky, inicializátor vypočítá adresu rovnou nule.</span><span class="sxs-lookup"><span data-stu-id="229bc-339">If the array expression is null or if the array has zero elements, the initializer computes an address equal to zero.</span></span>
*  <span data-ttu-id="229bc-340">Výraz typu `string`za předpokladu, že typ `char*` je implicitně převoditelné na typ ukazatele uvedený v příkazu `fixed`.</span><span class="sxs-lookup"><span data-stu-id="229bc-340">An expression of type `string`, provided the type `char*` is implicitly convertible to the pointer type given in the `fixed` statement.</span></span> <span data-ttu-id="229bc-341">V tomto případě inicializátor vypočítá adresu prvního znaku v řetězci a celý řetězec zaručuje, že zůstane na pevné adrese po dobu trvání příkazu `fixed`.</span><span class="sxs-lookup"><span data-stu-id="229bc-341">In this case, the initializer computes the address of the first character in the string, and the entire string is guaranteed to remain at a fixed address for the duration of the `fixed` statement.</span></span> <span data-ttu-id="229bc-342">Chování příkazu `fixed` je definováno implementací, pokud má řetězcový výraz hodnotu null.</span><span class="sxs-lookup"><span data-stu-id="229bc-342">The behavior of the `fixed` statement is implementation-defined if the string expression is null.</span></span>
*  <span data-ttu-id="229bc-343">*Simple_name* nebo *member_access* odkazující na člena vyrovnávací paměti s pevnou velikostí pohyblivé proměnné, pokud je typ člena vyrovnávací paměti s pevnou velikostí implicitně převoditelný na typ ukazatele uvedený v příkazu `fixed`.</span><span class="sxs-lookup"><span data-stu-id="229bc-343">A *simple_name* or *member_access* that references a fixed size buffer member of a moveable variable, provided the type of the fixed size buffer member is implicitly convertible to the pointer type given in the `fixed` statement.</span></span> <span data-ttu-id="229bc-344">V tomto případě inicializátor vypočítá ukazatel na první prvek vyrovnávací paměti pevné velikosti ([vyrovnávací paměti pevné velikosti ve výrazech](unsafe-code.md#fixed-size-buffers-in-expressions)) a vyrovnávací paměť pevné velikosti je zaručena, aby zůstala na pevné adrese po dobu trvání příkazu `fixed`.</span><span class="sxs-lookup"><span data-stu-id="229bc-344">In this case, the initializer computes a pointer to the first element of the fixed size buffer ([Fixed size buffers in expressions](unsafe-code.md#fixed-size-buffers-in-expressions)), and the fixed size buffer is guaranteed to remain at a fixed address for the duration of the `fixed` statement.</span></span>

<span data-ttu-id="229bc-345">Pro každou adresu vypočítanou *fixed_pointer_initializer* `fixed` příkaz zajistí, že proměnná odkazovaná adresou není předmětem přemístění nebo vyřazení systémem uvolňování paměti po dobu trvání příkazu `fixed`.</span><span class="sxs-lookup"><span data-stu-id="229bc-345">For each address computed by a *fixed_pointer_initializer* the `fixed` statement ensures that the variable referenced by the address is not subject to relocation or disposal by the garbage collector for the duration of the `fixed` statement.</span></span> <span data-ttu-id="229bc-346">Například pokud adresa vypočítaná *fixed_pointer_initializer* odkazuje na pole objektu nebo prvku instance pole, příkaz `fixed` garantuje, že objekt obsahující instanci objektu není přemístěné nebo vyřazený během životnosti prohlášení.</span><span class="sxs-lookup"><span data-stu-id="229bc-346">For example, if the address computed by a *fixed_pointer_initializer* references a field of an object or an element of an array instance, the `fixed` statement guarantees that the containing object instance is not relocated or disposed of during the lifetime of the statement.</span></span>

<span data-ttu-id="229bc-347">Je zodpovědností programátora, aby se zajistilo, že ukazatelé vytvořené pomocí `fixed` příkazy nezůstanou po provedení těchto příkazů.</span><span class="sxs-lookup"><span data-stu-id="229bc-347">It is the programmer's responsibility to ensure that pointers created by `fixed` statements do not survive beyond execution of those statements.</span></span> <span data-ttu-id="229bc-348">Například pokud jsou ukazatele vytvořené pomocí příkazů `fixed` předány externím rozhraním API, je úkolem programátora zajistit, aby rozhraní API nezůstala žádná paměť těchto ukazatelů.</span><span class="sxs-lookup"><span data-stu-id="229bc-348">For example, when pointers created by `fixed` statements are passed to external APIs, it is the programmer's responsibility to ensure that the APIs retain no memory of these pointers.</span></span>

<span data-ttu-id="229bc-349">Pevné objekty můžou způsobit fragmentaci haldy (protože se nedají přesunout).</span><span class="sxs-lookup"><span data-stu-id="229bc-349">Fixed objects may cause fragmentation of the heap (because they can't be moved).</span></span> <span data-ttu-id="229bc-350">Z tohoto důvodu by objekty měly být opraveny pouze v případě nezbytně nutné a pak pouze pro nejkratší možné množství času.</span><span class="sxs-lookup"><span data-stu-id="229bc-350">For that reason, objects should be fixed only when absolutely necessary and then only for the shortest amount of time possible.</span></span>

<span data-ttu-id="229bc-351">Příklad</span><span class="sxs-lookup"><span data-stu-id="229bc-351">The example</span></span>

```csharp
class Test
{
    static int x;
    int y;

    unsafe static void F(int* p) {
        *p = 1;
    }

    static void Main() {
        Test t = new Test();
        int[] a = new int[10];
        unsafe {
            fixed (int* p = &x) F(p);
            fixed (int* p = &t.y) F(p);
            fixed (int* p = &a[0]) F(p);
            fixed (int* p = a) F(p);
        }
    }
}
```

<span data-ttu-id="229bc-352">ukazuje několik použití příkazu `fixed`.</span><span class="sxs-lookup"><span data-stu-id="229bc-352">demonstrates several uses of the `fixed` statement.</span></span> <span data-ttu-id="229bc-353">První příkaz opraví a získá adresu statického pole, druhý příkaz opraví a získá adresu pole instance a třetí příkaz opraví a získá adresu elementu pole.</span><span class="sxs-lookup"><span data-stu-id="229bc-353">The first statement fixes and obtains the address of a static field, the second statement fixes and obtains the address of an instance field, and the third statement fixes and obtains the address of an array element.</span></span> <span data-ttu-id="229bc-354">V každém případě by došlo k chybě při použití regulárního operátoru `&`, protože proměnné jsou klasifikovány jako mobilní proměnné.</span><span class="sxs-lookup"><span data-stu-id="229bc-354">In each case it would have been an error to use the regular `&` operator since the variables are all classified as moveable variables.</span></span>

<span data-ttu-id="229bc-355">Čtvrtý `fixed` příkaz v předchozím příkladu vytvoří podobný výsledek jako třetí.</span><span class="sxs-lookup"><span data-stu-id="229bc-355">The fourth `fixed` statement in the example above produces a similar result to the third.</span></span>

<span data-ttu-id="229bc-356">Tento příklad příkazu `fixed` používá `string`:</span><span class="sxs-lookup"><span data-stu-id="229bc-356">This example of the `fixed` statement uses `string`:</span></span>

```csharp
class Test
{
    static string name = "xx";

    unsafe static void F(char* p) {
        for (int i = 0; p[i] != '\0'; ++i)
            Console.WriteLine(p[i]);
    }

    static void Main() {
        unsafe {
            fixed (char* p = name) F(p);
            fixed (char* p = "xx") F(p);
        }
    }
}
```

<span data-ttu-id="229bc-357">V nezabezpečené prvky pole kontextu jednorozměrného pole jsou uloženy ve vzestupném pořadí indexů, počínaje indexem `0` a končící `Length - 1`indexu.</span><span class="sxs-lookup"><span data-stu-id="229bc-357">In an unsafe context array elements of single-dimensional arrays are stored in increasing index order, starting with index `0` and ending with index `Length - 1`.</span></span> <span data-ttu-id="229bc-358">U multidimenzionálních polí jsou prvky pole uloženy tak, aby byly nejprve zvyšovány indexy pravého rozměru, potom následující levá dimenze a tak dále doleva.</span><span class="sxs-lookup"><span data-stu-id="229bc-358">For multi-dimensional arrays, array elements are stored such that the indices of the rightmost dimension are increased first, then the next left dimension, and so on to the left.</span></span> <span data-ttu-id="229bc-359">V rámci příkazu `fixed`, který získá ukazatel `p` na instanci pole `a`, hodnoty ukazatele od `p` po `p + a.Length - 1` reprezentují adresy prvků v poli.</span><span class="sxs-lookup"><span data-stu-id="229bc-359">Within a `fixed` statement that obtains a pointer `p` to an array instance `a`, the pointer values ranging from `p` to `p + a.Length - 1` represent addresses of the elements in the array.</span></span> <span data-ttu-id="229bc-360">Podobně proměnné, od `p[0]` po `p[a.Length - 1]` reprezentují skutečné prvky pole.</span><span class="sxs-lookup"><span data-stu-id="229bc-360">Likewise, the variables ranging from `p[0]` to `p[a.Length - 1]` represent the actual array elements.</span></span> <span data-ttu-id="229bc-361">S ohledem na způsob, jakým jsou pole uložena, můžeme považovat pole libovolné dimenze, jako kdyby byla lineární.</span><span class="sxs-lookup"><span data-stu-id="229bc-361">Given the way in which arrays are stored, we can treat an array of any dimension as though it were linear.</span></span>

<span data-ttu-id="229bc-362">Příklad:</span><span class="sxs-lookup"><span data-stu-id="229bc-362">For example:</span></span>

```csharp
using System;

class Test
{
    static void Main() {
        int[,,] a = new int[2,3,4];
        unsafe {
            fixed (int* p = a) {
                for (int i = 0; i < a.Length; ++i)    // treat as linear
                    p[i] = i;
            }
        }

        for (int i = 0; i < 2; ++i)
            for (int j = 0; j < 3; ++j) {
                for (int k = 0; k < 4; ++k)
                    Console.Write("[{0},{1},{2}] = {3,2} ", i, j, k, a[i,j,k]);
                Console.WriteLine();
            }
    }
}
```

<span data-ttu-id="229bc-363">který vytváří výstup:</span><span class="sxs-lookup"><span data-stu-id="229bc-363">which produces the output:</span></span>

```console
[0,0,0] =  0 [0,0,1] =  1 [0,0,2] =  2 [0,0,3] =  3
[0,1,0] =  4 [0,1,1] =  5 [0,1,2] =  6 [0,1,3] =  7
[0,2,0] =  8 [0,2,1] =  9 [0,2,2] = 10 [0,2,3] = 11
[1,0,0] = 12 [1,0,1] = 13 [1,0,2] = 14 [1,0,3] = 15
[1,1,0] = 16 [1,1,1] = 17 [1,1,2] = 18 [1,1,3] = 19
[1,2,0] = 20 [1,2,1] = 21 [1,2,2] = 22 [1,2,3] = 23
```

<span data-ttu-id="229bc-364">V příkladu</span><span class="sxs-lookup"><span data-stu-id="229bc-364">In the example</span></span>

```csharp
class Test
{
    unsafe static void Fill(int* p, int count, int value) {
        for (; count != 0; count--) *p++ = value;
    }

    static void Main() {
        int[] a = new int[100];
        unsafe {
            fixed (int* p = a) Fill(p, 100, -1);
        }
    }
}
```

<span data-ttu-id="229bc-365">příkaz `fixed` slouží k opravě pole tak, aby jeho adresa mohla být předána metodě, která přebírá ukazatel.</span><span class="sxs-lookup"><span data-stu-id="229bc-365">a `fixed` statement is used to fix an array so its address can be passed to a method that takes a pointer.</span></span>

<span data-ttu-id="229bc-366">V tomto příkladu:</span><span class="sxs-lookup"><span data-stu-id="229bc-366">In the example:</span></span>

```csharp
unsafe struct Font
{
    public int size;
    public fixed char name[32];
}

class Test
{
    unsafe static void PutString(string s, char* buffer, int bufSize) {
        int len = s.Length;
        if (len > bufSize) len = bufSize;
        for (int i = 0; i < len; i++) buffer[i] = s[i];
        for (int i = len; i < bufSize; i++) buffer[i] = (char)0;
    }

    Font f;

    unsafe static void Main()
    {
        Test test = new Test();
        test.f.size = 10;
        fixed (char* p = test.f.name) {
            PutString("Times New Roman", p, 32);
        }
    }
}
```

<span data-ttu-id="229bc-367">příkaz fixed se používá k opravě vyrovnávací paměti pevné velikosti struktury, aby její adresa mohla být použita jako ukazatel.</span><span class="sxs-lookup"><span data-stu-id="229bc-367">a fixed statement is used to fix a fixed size buffer of a struct so its address can be used as a pointer.</span></span>

<span data-ttu-id="229bc-368">Hodnota `char*` vytvořená opravou instance řetězce vždy odkazuje na řetězec zakončený hodnotou null.</span><span class="sxs-lookup"><span data-stu-id="229bc-368">A `char*` value produced by fixing a string instance always points to a null-terminated string.</span></span> <span data-ttu-id="229bc-369">V rámci příkazu fixed, který získá ukazatel `p` na `s`instance řetězce, hodnoty ukazatele od `p` po `p + s.Length - 1` reprezentují adresy znaků v řetězci a hodnota ukazatele `p + s.Length` vždy odkazuje na znak null (znak s hodnotou `'\0'`).</span><span class="sxs-lookup"><span data-stu-id="229bc-369">Within a fixed statement that obtains a pointer `p` to a string instance `s`, the pointer values ranging from `p` to `p + s.Length - 1` represent addresses of the characters in the string, and the pointer value `p + s.Length` always points to a null character (the character with value `'\0'`).</span></span>

<span data-ttu-id="229bc-370">Změna objektů spravovaného typu prostřednictvím pevných ukazatelů může mít za následek nedefinované chování.</span><span class="sxs-lookup"><span data-stu-id="229bc-370">Modifying objects of managed type through fixed pointers can results in undefined behavior.</span></span> <span data-ttu-id="229bc-371">Například vzhledem k tomu, že řetězce jsou neměnné, je úkolem programátora zajistit, aby se znaky odkazované ukazatelem na pevný řetězec nezměnily.</span><span class="sxs-lookup"><span data-stu-id="229bc-371">For example, because strings are immutable, it is the programmer's responsibility to ensure that the characters referenced by a pointer to a fixed string are not modified.</span></span>

<span data-ttu-id="229bc-372">Automatické ukončení řetězce s hodnotou null je zvláště pohodlné při volání externích rozhraní API, která očekávají řetězce "C-Style".</span><span class="sxs-lookup"><span data-stu-id="229bc-372">The automatic null-termination of strings is particularly convenient when calling external APIs that expect "C-style" strings.</span></span> <span data-ttu-id="229bc-373">Všimněte si však, že instance řetězce má povoleno obsahovat znaky null.</span><span class="sxs-lookup"><span data-stu-id="229bc-373">Note, however, that a string instance is permitted to contain null characters.</span></span> <span data-ttu-id="229bc-374">Pokud jsou tyto znaky null k dispozici, řetězec se zkrátí, pokud je považován za `char*`zakončený hodnotou null.</span><span class="sxs-lookup"><span data-stu-id="229bc-374">If such null characters are present, the string will appear truncated when treated as a null-terminated `char*`.</span></span>

## <a name="fixed-size-buffers"></a><span data-ttu-id="229bc-375">Vyrovnávací paměti pevné velikosti</span><span class="sxs-lookup"><span data-stu-id="229bc-375">Fixed size buffers</span></span>

<span data-ttu-id="229bc-376">Vyrovnávací paměti pevné velikosti slouží k deklaraci "Style" v řádkových polích jako členů struktury a jsou primárně užitečné pro propojení s nespravovanými rozhraními API.</span><span class="sxs-lookup"><span data-stu-id="229bc-376">Fixed size buffers are used to declare "C style" in-line arrays as members of structs, and are primarily useful for interfacing with unmanaged APIs.</span></span>

### <a name="fixed-size-buffer-declarations"></a><span data-ttu-id="229bc-377">Deklarace vyrovnávací paměti pevné velikosti</span><span class="sxs-lookup"><span data-stu-id="229bc-377">Fixed size buffer declarations</span></span>

<span data-ttu-id="229bc-378">***Vyrovnávací paměť pevné velikosti*** je člen, který představuje úložiště pro vyrovnávací paměť pevné délky proměnných daného typu.</span><span class="sxs-lookup"><span data-stu-id="229bc-378">A ***fixed size buffer*** is a member that represents storage for a fixed length buffer of variables of a given type.</span></span> <span data-ttu-id="229bc-379">Deklarace vyrovnávací paměti pevné velikosti zavádí jednu nebo více vyrovnávacích pamětí s pevnou velikostí daného typu elementu.</span><span class="sxs-lookup"><span data-stu-id="229bc-379">A fixed size buffer declaration introduces one or more fixed size buffers of a given element type.</span></span> <span data-ttu-id="229bc-380">Vyrovnávací paměti pevné velikosti jsou povoleny pouze v deklaracích struktury a mohou být provedeny pouze v nezabezpečených kontextech ([nebezpečné kontexty](unsafe-code.md#unsafe-contexts)).</span><span class="sxs-lookup"><span data-stu-id="229bc-380">Fixed size buffers are only permitted in struct declarations and can only occur in unsafe contexts ([Unsafe contexts](unsafe-code.md#unsafe-contexts)).</span></span>

```antlr
struct_member_declaration_unsafe
    : fixed_size_buffer_declaration
    ;

fixed_size_buffer_declaration
    : attributes? fixed_size_buffer_modifier* 'fixed' buffer_element_type fixed_size_buffer_declarator+ ';'
    ;

fixed_size_buffer_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    | 'unsafe'
    ;

buffer_element_type
    : type
    ;

fixed_size_buffer_declarator
    : identifier '[' constant_expression ']'
    ;
```

<span data-ttu-id="229bc-381">Deklarace vyrovnávací paměti pevné velikosti může zahrnovat sadu atributů ([atributů](attributes.md)), modifikátor `new` ([modifikátory](classes.md#modifiers)), platnou kombinaci obou modifikátorů přístupu ([parametry typu a omezení](classes.md#type-parameters-and-constraints)) a modifikátoru `unsafe` ([nebezpečné kontexty](unsafe-code.md#unsafe-contexts)).</span><span class="sxs-lookup"><span data-stu-id="229bc-381">A fixed size buffer declaration may include a set of attributes ([Attributes](attributes.md)), a `new` modifier ([Modifiers](classes.md#modifiers)), a valid combination of the four access modifiers ([Type parameters and constraints](classes.md#type-parameters-and-constraints)) and an `unsafe` modifier ([Unsafe contexts](unsafe-code.md#unsafe-contexts)).</span></span> <span data-ttu-id="229bc-382">Atributy a modifikátory se vztahují na všechny členy deklarované deklarací vyrovnávací paměti s pevnou velikostí.</span><span class="sxs-lookup"><span data-stu-id="229bc-382">The attributes and modifiers apply to all of the members declared by the fixed size buffer declaration.</span></span> <span data-ttu-id="229bc-383">Jedná se o chybu, aby se stejný modifikátor zobrazoval víckrát v deklaraci vyrovnávací paměti s pevnou velikostí.</span><span class="sxs-lookup"><span data-stu-id="229bc-383">It is an error for the same modifier to appear multiple times in a fixed size buffer declaration.</span></span>

<span data-ttu-id="229bc-384">Deklarace vyrovnávací paměti s pevnou velikostí není povolená pro zahrnutí modifikátoru `static`.</span><span class="sxs-lookup"><span data-stu-id="229bc-384">A fixed size buffer declaration is not permitted to include the `static` modifier.</span></span>

<span data-ttu-id="229bc-385">Typ elementu bufferu pro deklaraci vyrovnávací paměti s pevnou velikostí určuje typ prvku vyrovnávací paměti, kterou deklarace zavedla.</span><span class="sxs-lookup"><span data-stu-id="229bc-385">The buffer element type of a fixed size buffer declaration specifies the element type of the buffer(s) introduced by the declaration.</span></span> <span data-ttu-id="229bc-386">Typ elementu buffer musí být jeden z předdefinovaných typů `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`nebo `bool`.</span><span class="sxs-lookup"><span data-stu-id="229bc-386">The buffer element type must be one of the predefined types `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, or `bool`.</span></span>

<span data-ttu-id="229bc-387">Typ elementu bufferu následuje seznam deklarátory vyrovnávací paměti s pevnou velikostí, z nichž každá zavádí nového člena.</span><span class="sxs-lookup"><span data-stu-id="229bc-387">The buffer element type is followed by a list of fixed size buffer declarators, each of which introduces a new member.</span></span> <span data-ttu-id="229bc-388">Deklarátor vyrovnávací paměti s pevnou velikostí se skládá z identifikátoru, který člen pojmenovává, následovaný konstantním výrazem uzavřeným v `[` a `]` tokeny.</span><span class="sxs-lookup"><span data-stu-id="229bc-388">A fixed size buffer declarator consists of an identifier that names the member, followed by a constant expression enclosed in `[` and `]` tokens.</span></span> <span data-ttu-id="229bc-389">Konstantní výraz označuje počet prvků v členu zavedený deklarátor vyrovnávací paměti s pevnou velikostí.</span><span class="sxs-lookup"><span data-stu-id="229bc-389">The constant expression denotes the number of elements in the member introduced by that fixed size buffer declarator.</span></span> <span data-ttu-id="229bc-390">Typ konstantního výrazu musí být implicitně převeden na typ `int`a hodnota musí být kladné celé číslo, které není nula.</span><span class="sxs-lookup"><span data-stu-id="229bc-390">The type of the constant expression must be implicitly convertible to type `int`, and the value must be a non-zero positive integer.</span></span>

<span data-ttu-id="229bc-391">Prvky vyrovnávací paměti s pevnou velikostí jsou zaručeny tak, aby byly rozloženy postupně v paměti.</span><span class="sxs-lookup"><span data-stu-id="229bc-391">The elements of a fixed size buffer are guaranteed to be laid out sequentially in memory.</span></span>

<span data-ttu-id="229bc-392">Deklarace vyrovnávací paměti s pevnou velikostí, která deklaruje více vyrovnávacích pamětí s pevnou velikostí, je ekvivalentní více deklaracím jediné deklarace vyrovnávací paměti s pevnou velikostí se stejnými atributy a typy prvků.</span><span class="sxs-lookup"><span data-stu-id="229bc-392">A fixed size buffer declaration that declares multiple fixed size buffers is equivalent to multiple declarations of a single fixed size buffer declaration with the same attributes, and element types.</span></span> <span data-ttu-id="229bc-393">Například</span><span class="sxs-lookup"><span data-stu-id="229bc-393">For example</span></span>

```csharp
unsafe struct A
{
   public fixed int x[5], y[10], z[100];
}
```

<span data-ttu-id="229bc-394">je ekvivalentem</span><span class="sxs-lookup"><span data-stu-id="229bc-394">is equivalent to</span></span>

```csharp
unsafe struct A
{
   public fixed int x[5];
   public fixed int y[10];
   public fixed int z[100];
}
```

### <a name="fixed-size-buffers-in-expressions"></a><span data-ttu-id="229bc-395">Vyrovnávací paměti pevné velikosti ve výrazech</span><span class="sxs-lookup"><span data-stu-id="229bc-395">Fixed size buffers in expressions</span></span>

<span data-ttu-id="229bc-396">Vyhledávání členů ([operátoři](expressions.md#operators)) člena vyrovnávací paměti pevné velikosti pokračuje přesně stejně jako vyhledávání členů pole.</span><span class="sxs-lookup"><span data-stu-id="229bc-396">Member lookup ([Operators](expressions.md#operators)) of a fixed size buffer member proceeds exactly like member lookup of a field.</span></span>

<span data-ttu-id="229bc-397">Vyrovnávací paměť pevné velikosti může být ve výrazu odkazována pomocí *simple_name* ([odvození typu](expressions.md#type-inference)) nebo *member_access* ([Kontrola při kompilaci dynamického překladu přetížení](expressions.md#compile-time-checking-of-dynamic-overload-resolution)).</span><span class="sxs-lookup"><span data-stu-id="229bc-397">A fixed size buffer can be referenced in an expression using a *simple_name* ([Type inference](expressions.md#type-inference)) or a *member_access* ([Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution)).</span></span>

<span data-ttu-id="229bc-398">Když je člen vyrovnávací paměti s pevnou velikostí odkazován jako jednoduchý název, je efekt stejný jako členský přístup k formuláři `this.I`, kde `I` je členem vyrovnávací paměti s pevnou velikostí.</span><span class="sxs-lookup"><span data-stu-id="229bc-398">When a fixed size buffer member is referenced as a simple name, the effect is the same as a member access of the form `this.I`, where `I` is the fixed size buffer member.</span></span>

<span data-ttu-id="229bc-399">V případě přístupu člena formuláře `E.I`, pokud `E` je typu struktura a vyhledávání členů `I` v tomto typu struktury identifikuje člena pevné velikosti, `E.I` je vyhodnocen jako následující:</span><span class="sxs-lookup"><span data-stu-id="229bc-399">In a member access of the form `E.I`, if `E` is of a struct type and a member lookup of `I` in that struct type identifies a fixed size member, then `E.I` is evaluated an classified as follows:</span></span>

*  <span data-ttu-id="229bc-400">Pokud `E.I` výraz neproběhne v nebezpečném kontextu, dojde k chybě při kompilaci.</span><span class="sxs-lookup"><span data-stu-id="229bc-400">If the expression `E.I` does not occur in an unsafe context, a compile-time error occurs.</span></span>
*  <span data-ttu-id="229bc-401">Pokud je `E` klasifikován jako hodnota, dojde k chybě při kompilaci.</span><span class="sxs-lookup"><span data-stu-id="229bc-401">If `E` is classified as a value, a compile-time error occurs.</span></span>
*  <span data-ttu-id="229bc-402">V opačném případě, pokud je `E` pohyblivé proměnné ([pevné a mobilní proměnné](unsafe-code.md#fixed-and-moveable-variables)) a výraz `E.I` není *fixed_pointer_initializer* ([příkaz fixed](unsafe-code.md#the-fixed-statement)), dojde k chybě při kompilaci.</span><span class="sxs-lookup"><span data-stu-id="229bc-402">Otherwise, if `E` is a moveable variable ([Fixed and moveable variables](unsafe-code.md#fixed-and-moveable-variables)) and the expression `E.I` is not a *fixed_pointer_initializer* ([The fixed statement](unsafe-code.md#the-fixed-statement)), a compile-time error occurs.</span></span>
*  <span data-ttu-id="229bc-403">V opačném případě `E` odkazuje na pevnou proměnnou a výsledek výrazu je ukazatel na první prvek člena vyrovnávací paměti pevné velikosti `I` v `E`.</span><span class="sxs-lookup"><span data-stu-id="229bc-403">Otherwise, `E` references a fixed variable and the result of the expression is a pointer to the first element of the fixed size buffer member `I` in `E`.</span></span> <span data-ttu-id="229bc-404">Výsledek je typu `S*`, kde `S` je typ prvku `I`a je klasifikován jako hodnota.</span><span class="sxs-lookup"><span data-stu-id="229bc-404">The result is of type `S*`, where `S` is the element type of `I`, and is classified as a value.</span></span>

<span data-ttu-id="229bc-405">Následující prvky vyrovnávací paměti s pevnou velikostí lze použít při operacích s ukazateli z prvního prvku.</span><span class="sxs-lookup"><span data-stu-id="229bc-405">The subsequent elements of the fixed size buffer can be accessed using pointer operations from the first element.</span></span> <span data-ttu-id="229bc-406">Přístup k prvkům vyrovnávací paměti s pevnou velikostí je na rozdíl od přístupu k polím nebezpečná operace a není zaškrtnuta rozsah.</span><span class="sxs-lookup"><span data-stu-id="229bc-406">Unlike access to arrays, access to the elements of a fixed size buffer is an unsafe operation and is not range checked.</span></span>

<span data-ttu-id="229bc-407">Následující příklad deklaruje a používá strukturu s členem vyrovnávací paměti s pevnou velikostí.</span><span class="sxs-lookup"><span data-stu-id="229bc-407">The following example declares and uses a struct with a fixed size buffer member.</span></span>

```csharp
unsafe struct Font
{
    public int size;
    public fixed char name[32];
}

class Test
{
    unsafe static void PutString(string s, char* buffer, int bufSize) {
        int len = s.Length;
        if (len > bufSize) len = bufSize;
        for (int i = 0; i < len; i++) buffer[i] = s[i];
        for (int i = len; i < bufSize; i++) buffer[i] = (char)0;
    }

    unsafe static void Main()
    {
        Font f;
        f.size = 10;
        PutString("Times New Roman", f.name, 32);
    }
}
```

### <a name="definite-assignment-checking"></a><span data-ttu-id="229bc-408">Kontrola přiřazení na určitou dobu</span><span class="sxs-lookup"><span data-stu-id="229bc-408">Definite assignment checking</span></span>

<span data-ttu-id="229bc-409">Vyrovnávací paměti s pevnou velikostí nepodléhají jednoznačné kontrole přiřazení ([jednoznačné přiřazení](variables.md#definite-assignment)) a členy vyrovnávací paměti pevné velikosti jsou ignorovány pro účely jednoznačné kontroly přiřazení proměnných typu struktury.</span><span class="sxs-lookup"><span data-stu-id="229bc-409">Fixed size buffers are not subject to definite assignment checking ([Definite assignment](variables.md#definite-assignment)), and fixed size buffer members are ignored for purposes of definite assignment checking of struct type variables.</span></span>

<span data-ttu-id="229bc-410">Když je nejvzdálenější objekt obsahující proměnnou struktury pro člena vyrovnávací paměti pevné velikosti statická proměnná, proměnná instance třídy nebo element pole, prvky vyrovnávací paměti pevné velikosti jsou automaticky inicializovány na výchozí hodnoty ([výchozí hodnoty](variables.md#default-values)).</span><span class="sxs-lookup"><span data-stu-id="229bc-410">When the outermost containing struct variable of a fixed size buffer member is a static variable, an instance variable of a class instance, or an array element, the elements of the fixed size buffer are automatically initialized to their default values ([Default values](variables.md#default-values)).</span></span> <span data-ttu-id="229bc-411">Ve všech ostatních případech není počáteční obsah vyrovnávací paměti s pevnou velikostí definován.</span><span class="sxs-lookup"><span data-stu-id="229bc-411">In all other cases, the initial content of a fixed size buffer is undefined.</span></span>

## <a name="stack-allocation"></a><span data-ttu-id="229bc-412">Přidělení zásobníku</span><span class="sxs-lookup"><span data-stu-id="229bc-412">Stack allocation</span></span>

<span data-ttu-id="229bc-413">V nezabezpečeném kontextu může deklarace místní proměnné ([deklarace místní proměnné](statements.md#local-variable-declarations)) zahrnovat inicializátor přidělení zásobníku, který přiděluje paměť ze zásobníku volání.</span><span class="sxs-lookup"><span data-stu-id="229bc-413">In an unsafe context, a local variable declaration ([Local variable declarations](statements.md#local-variable-declarations)) may include a stack allocation initializer which allocates memory from the call stack.</span></span>

```antlr
local_variable_initializer_unsafe
    : stackalloc_initializer
    ;

stackalloc_initializer
    : 'stackalloc' unmanaged_type '[' expression ']'
    ;
```

<span data-ttu-id="229bc-414">*Unmanaged_type* označuje typ položek, které budou uloženy v nově přiděleném umístění, a *výraz* označuje počet těchto položek.</span><span class="sxs-lookup"><span data-stu-id="229bc-414">The *unmanaged_type* indicates the type of the items that will be stored in the newly allocated location, and the *expression* indicates the number of these items.</span></span> <span data-ttu-id="229bc-415">Společně, určují velikost požadované alokace.</span><span class="sxs-lookup"><span data-stu-id="229bc-415">Taken together, these specify the required allocation size.</span></span> <span data-ttu-id="229bc-416">Vzhledem k tomu, že velikost přidělení zásobníku nemůže být záporná, jedná se o chybu při kompilaci, která určuje počet položek jako *constant_expression* , která se vyhodnocuje jako záporná hodnota.</span><span class="sxs-lookup"><span data-stu-id="229bc-416">Since the size of a stack allocation cannot be negative, it is a compile-time error to specify the number of items as a *constant_expression* that evaluates to a negative value.</span></span>

<span data-ttu-id="229bc-417">Inicializátor přidělení zásobníku formuláře `stackalloc T[E]` vyžaduje, aby `T` být nespravovaného typu ([typy ukazatelů](unsafe-code.md#pointer-types)) a `E` jako výraz typu `int`.</span><span class="sxs-lookup"><span data-stu-id="229bc-417">A stack allocation initializer of the form `stackalloc T[E]` requires `T` to be an unmanaged type ([Pointer types](unsafe-code.md#pointer-types)) and `E` to be an expression of type `int`.</span></span> <span data-ttu-id="229bc-418">Konstrukce přiděluje `E * sizeof(T)` bajtů ze zásobníku volání a vrací ukazatel typu `T*`na nově přidělený blok.</span><span class="sxs-lookup"><span data-stu-id="229bc-418">The construct allocates `E * sizeof(T)` bytes from the call stack and returns a pointer, of type `T*`, to the newly allocated block.</span></span> <span data-ttu-id="229bc-419">Je-li `E` záporná hodnota, chování není definováno.</span><span class="sxs-lookup"><span data-stu-id="229bc-419">If `E` is a negative value, then the behavior is undefined.</span></span> <span data-ttu-id="229bc-420">Pokud je `E` nula, není provedena žádná alokace a vrácený ukazatel je definován implementací.</span><span class="sxs-lookup"><span data-stu-id="229bc-420">If `E` is zero, then no allocation is made, and the pointer returned is implementation-defined.</span></span> <span data-ttu-id="229bc-421">Pokud není k dispozici dostatek paměti pro přidělení bloku dané velikosti, je vyvolána `System.StackOverflowException`.</span><span class="sxs-lookup"><span data-stu-id="229bc-421">If there is not enough memory available to allocate a block of the given size, a `System.StackOverflowException` is thrown.</span></span>

<span data-ttu-id="229bc-422">Obsah nově přidělené paměti není definován.</span><span class="sxs-lookup"><span data-stu-id="229bc-422">The content of the newly allocated memory is undefined.</span></span>

<span data-ttu-id="229bc-423">Inicializátory přidělení zásobníku nejsou povolené v blocích `catch` nebo `finally` ([příkaz try](statements.md#the-try-statement)).</span><span class="sxs-lookup"><span data-stu-id="229bc-423">Stack allocation initializers are not permitted in `catch` or `finally` blocks ([The try statement](statements.md#the-try-statement)).</span></span>

<span data-ttu-id="229bc-424">Neexistuje žádný způsob, jak explicitně uvolnit paměť přidělenou pomocí `stackalloc`.</span><span class="sxs-lookup"><span data-stu-id="229bc-424">There is no way to explicitly free memory allocated using `stackalloc`.</span></span> <span data-ttu-id="229bc-425">Všechny bloky paměti přidělené zásobníku vytvořené během provádění členu funkce jsou automaticky zahozeny, když tento člen funkce vrátí.</span><span class="sxs-lookup"><span data-stu-id="229bc-425">All stack allocated memory blocks created during the execution of a function member are automatically discarded when that function member returns.</span></span> <span data-ttu-id="229bc-426">To odpovídá funkci `alloca`, rozšíření obvykle nalezeno v C a C++ implementacích.</span><span class="sxs-lookup"><span data-stu-id="229bc-426">This corresponds to the `alloca` function, an extension commonly found in C and C++ implementations.</span></span>

<span data-ttu-id="229bc-427">V příkladu</span><span class="sxs-lookup"><span data-stu-id="229bc-427">In the example</span></span>

```csharp
using System;

class Test
{
    static string IntToString(int value) {
        int n = value >= 0? value: -value;
        unsafe {
            char* buffer = stackalloc char[16];
            char* p = buffer + 16;
            do {
                *--p = (char)(n % 10 + '0');
                n /= 10;
            } while (n != 0);
            if (value < 0) *--p = '-';
            return new string(p, 0, (int)(buffer + 16 - p));
        }
    }

    static void Main() {
        Console.WriteLine(IntToString(12345));
        Console.WriteLine(IntToString(-999));
    }
}
```

<span data-ttu-id="229bc-428">v metodě `IntToString` se používá inicializátor `stackalloc` k přidělení vyrovnávací paměti 16 znaků v zásobníku.</span><span class="sxs-lookup"><span data-stu-id="229bc-428">a `stackalloc` initializer is used in the `IntToString` method to allocate a buffer of 16 characters on the stack.</span></span> <span data-ttu-id="229bc-429">Vyrovnávací paměť se automaticky zahodí, když se metoda vrátí.</span><span class="sxs-lookup"><span data-stu-id="229bc-429">The buffer is automatically discarded when the method returns.</span></span>

## <a name="dynamic-memory-allocation"></a><span data-ttu-id="229bc-430">Dynamické přidělování paměti</span><span class="sxs-lookup"><span data-stu-id="229bc-430">Dynamic memory allocation</span></span>

<span data-ttu-id="229bc-431">S výjimkou operátoru `stackalloc` C# neposkytuje žádné předdefinované konstrukce pro správu paměti, která není v paměti pro uvolňování paměti.</span><span class="sxs-lookup"><span data-stu-id="229bc-431">Except for the `stackalloc` operator, C# provides no predefined constructs for managing non-garbage collected memory.</span></span> <span data-ttu-id="229bc-432">Tyto služby jsou obvykle poskytovány prostřednictvím podpory knihoven tříd nebo naimportované přímo z podkladového operačního systému.</span><span class="sxs-lookup"><span data-stu-id="229bc-432">Such services are typically provided by supporting class libraries or imported directly from the underlying operating system.</span></span> <span data-ttu-id="229bc-433">Například třída `Memory` níže ukazuje, jak je možné, že jsou k dispozici funkce haldy základního operačního systému C#:</span><span class="sxs-lookup"><span data-stu-id="229bc-433">For example, the `Memory` class below illustrates how the heap functions of an underlying operating system might be accessed from C#:</span></span>

```csharp
using System;
using System.Runtime.InteropServices;

public static unsafe class Memory
{
    // Handle for the process heap. This handle is used in all calls to the
    // HeapXXX APIs in the methods below.
    private static readonly IntPtr s_heap = GetProcessHeap();

    // Allocates a memory block of the given size. The allocated memory is
    // automatically initialized to zero.
    public static void* Alloc(int size)
    {
        void* result = HeapAlloc(s_heap, HEAP_ZERO_MEMORY, (UIntPtr)size);
        if (result == null) throw new OutOfMemoryException();
        return result;
    }

    // Copies count bytes from src to dst. The source and destination
    // blocks are permitted to overlap.
    public static void Copy(void* src, void* dst, int count)
    {
        byte* ps = (byte*)src;
        byte* pd = (byte*)dst;
        if (ps > pd)
        {
            for (; count != 0; count--) *pd++ = *ps++;
        }
        else if (ps < pd)
        {
            for (ps += count, pd += count; count != 0; count--) *--pd = *--ps;
        }
    }

    // Frees a memory block.
    public static void Free(void* block)
    {
        if (!HeapFree(s_heap, 0, block)) throw new InvalidOperationException();
    }

    // Re-allocates a memory block. If the reallocation request is for a
    // larger size, the additional region of memory is automatically
    // initialized to zero.
    public static void* ReAlloc(void* block, int size)
    {
        void* result = HeapReAlloc(s_heap, HEAP_ZERO_MEMORY, block, (UIntPtr)size);
        if (result == null) throw new OutOfMemoryException();
        return result;
    }

    // Returns the size of a memory block.
    public static int SizeOf(void* block)
    {
        int result = (int)HeapSize(s_heap, 0, block);
        if (result == -1) throw new InvalidOperationException();
        return result;
    }

    // Heap API flags
    private const int HEAP_ZERO_MEMORY = 0x00000008;

    // Heap API functions
    [DllImport("kernel32")]
    private static extern IntPtr GetProcessHeap();

    [DllImport("kernel32")]
    private static extern void* HeapAlloc(IntPtr hHeap, int flags, UIntPtr size);

    [DllImport("kernel32")]
    private static extern bool HeapFree(IntPtr hHeap, int flags, void* block);

    [DllImport("kernel32")]
    private static extern void* HeapReAlloc(IntPtr hHeap, int flags, void* block, UIntPtr size);

    [DllImport("kernel32")]
    private static extern UIntPtr HeapSize(IntPtr hHeap, int flags, void* block);
}
```

<span data-ttu-id="229bc-434">Příklad, který používá třídu `Memory`, je uveden níže:</span><span class="sxs-lookup"><span data-stu-id="229bc-434">An example that uses the `Memory` class is given below:</span></span>

```csharp
class Test
{
    static unsafe void Main()
    {
        byte* buffer = null;
        try
        {
            const int Size = 256;
            buffer = (byte*)Memory.Alloc(Size);
            for (int i = 0; i < Size; i++) buffer[i] = (byte)i;
            byte[] array = new byte[Size];
            fixed (byte* p = array) Memory.Copy(buffer, p, Size);
            for (int i = 0; i < Size; i++) Console.WriteLine(array[i]);
        }
        finally
        {
            if (buffer != null) Memory.Free(buffer);
        }
    }
}
```

<span data-ttu-id="229bc-435">Příklad přiděluje 256 bajtů paměti prostřednictvím `Memory.Alloc` a inicializuje blok paměti hodnotami, které se zvyšují z 0 na 255.</span><span class="sxs-lookup"><span data-stu-id="229bc-435">The example allocates 256 bytes of memory through `Memory.Alloc` and initializes the memory block with values increasing from 0 to 255.</span></span> <span data-ttu-id="229bc-436">Poté přidělí pole bajtů elementu 256 a používá `Memory.Copy` ke zkopírování obsahu bloku paměti do pole bajtů.</span><span class="sxs-lookup"><span data-stu-id="229bc-436">It then allocates a 256 element byte array and uses `Memory.Copy` to copy the contents of the memory block into the byte array.</span></span> <span data-ttu-id="229bc-437">Nakonec je blok paměti uvolněn pomocí `Memory.Free` a obsah pole bajtů je výstupem v konzole.</span><span class="sxs-lookup"><span data-stu-id="229bc-437">Finally, the memory block is freed using `Memory.Free` and the contents of the byte array are output on the console.</span></span>
