---
ms.openlocfilehash: 90001cf3d48f216787fc65e59166ec57c5d0ca34
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/25/2019
ms.locfileid: "64488802"
---
# <a name="unsafe-code"></a><span data-ttu-id="eaf18-101">Nebezpečný kód</span><span class="sxs-lookup"><span data-stu-id="eaf18-101">Unsafe code</span></span>

<span data-ttu-id="eaf18-102">Základní jazyk C#, jak jsou definovány v předchozích kapitol, se liší zejména z jazyka C a C++ v jeho vynechání ukazatele jako datový typ.</span><span class="sxs-lookup"><span data-stu-id="eaf18-102">The core C# language, as defined in the preceding chapters, differs notably from C and C++ in its omission of pointers as a data type.</span></span> <span data-ttu-id="eaf18-103">Místo toho jazyk C# poskytuje odkazů a schopnost vytvářet objekty, které se spravují přes systému uvolňování paměti.</span><span class="sxs-lookup"><span data-stu-id="eaf18-103">Instead, C# provides references and the ability to create objects that are managed by a garbage collector.</span></span> <span data-ttu-id="eaf18-104">Tento návrh spolu s dalšími funkcemi, díky C# mnohem bezpečnější jazyka C nebo C++.</span><span class="sxs-lookup"><span data-stu-id="eaf18-104">This design, coupled with other features, makes C# a much safer language than C or C++.</span></span> <span data-ttu-id="eaf18-105">V základním jazyce C# jazyce není jednoduše možné mít neinicializované proměnné, ukazatel "nepropojená" nebo výraz, který indexuje pole nad rámec jeho hranice.</span><span class="sxs-lookup"><span data-stu-id="eaf18-105">In the core C# language it is simply not possible to have an uninitialized variable, a "dangling" pointer, or an expression that indexes an array beyond its bounds.</span></span> <span data-ttu-id="eaf18-106">Celé kategorie chyb, který pravidelně mor C a programy v jazyce C++ jsou tedy vyloučeny.</span><span class="sxs-lookup"><span data-stu-id="eaf18-106">Whole categories of bugs that routinely plague C and C++ programs are thus eliminated.</span></span>

<span data-ttu-id="eaf18-107">Zatímco prakticky každé konstrukce typu ukazatele v jazyce C nebo C++ protějšek typu odkazu v jazyce C#, jsou však situace, ve kterém přístup k typy ukazatelů bude nezbytné.</span><span class="sxs-lookup"><span data-stu-id="eaf18-107">While practically every pointer type construct in C or C++ has a reference type counterpart in C#, nonetheless, there are situations where access to pointer types becomes a necessity.</span></span> <span data-ttu-id="eaf18-108">Například propojení s základního operačního systému, přístup k zařízení mapované paměti nebo implementaci kritického pro čas algoritmus nemusí být možné nebo praktické bez přístupu k ukazateli.</span><span class="sxs-lookup"><span data-stu-id="eaf18-108">For example, interfacing with the underlying operating system, accessing a memory-mapped device, or implementing a time-critical algorithm may not be possible or practical without access to pointers.</span></span> <span data-ttu-id="eaf18-109">Chcete-li tyto potřeby řeší, C# poskytuje schopnost psát ***nezabezpečený kód***.</span><span class="sxs-lookup"><span data-stu-id="eaf18-109">To address this need, C# provides the ability to write ***unsafe code***.</span></span>

<span data-ttu-id="eaf18-110">Nezabezpečený kód je možné deklarovat a fungovat u ukazatelů, provádět převody mezi ukazatele a integrálními typy převzít adresu proměnné a tak dále.</span><span class="sxs-lookup"><span data-stu-id="eaf18-110">In unsafe code it is possible to declare and operate on pointers, to perform conversions between pointers and integral types, to take the address of variables, and so forth.</span></span> <span data-ttu-id="eaf18-111">V tom smyslu psaní nezabezpečený kód je mnohem psaní kódu jazyka C v rámci programu v jazyce C#.</span><span class="sxs-lookup"><span data-stu-id="eaf18-111">In a sense, writing unsafe code is much like writing C code within a C# program.</span></span>

<span data-ttu-id="eaf18-112">Nezabezpečený kód je ve skutečnosti "bezpečné" funkce z pohledu vývojářů a uživatelů.</span><span class="sxs-lookup"><span data-stu-id="eaf18-112">Unsafe code is in fact a "safe" feature from the perspective of both developers and users.</span></span> <span data-ttu-id="eaf18-113">Nezabezpečený kód musí být přehledně označen modifikátorem `unsafe`, takže vývojáři funkce nelze používat potenciálně nebezpečné omylem a prováděcí modul funguje a jak můžete zajistit, že nezabezpečený kód nelze provést v prostředí nedůvěryhodný.</span><span class="sxs-lookup"><span data-stu-id="eaf18-113">Unsafe code must be clearly marked with the modifier `unsafe`, so developers can't possibly use unsafe features accidentally, and the execution engine works to ensure that unsafe code cannot be executed in an untrusted environment.</span></span>

## <a name="unsafe-contexts"></a><span data-ttu-id="eaf18-114">Nezabezpečený kontext</span><span class="sxs-lookup"><span data-stu-id="eaf18-114">Unsafe contexts</span></span>

<span data-ttu-id="eaf18-115">Nezabezpečený funkce jazyka C# jsou k dispozici pouze v kontextu unsafe.</span><span class="sxs-lookup"><span data-stu-id="eaf18-115">The unsafe features of C# are available only in unsafe contexts.</span></span> <span data-ttu-id="eaf18-116">Je zavedený nezabezpečený kontext včetně `unsafe` modifikátor v deklaraci typu nebo člena, nebo když *unsafe_statement*:</span><span class="sxs-lookup"><span data-stu-id="eaf18-116">An unsafe context is introduced by including an `unsafe` modifier in the declaration of a type or member, or by employing an *unsafe_statement*:</span></span>

*  <span data-ttu-id="eaf18-117">Může obsahovat deklaraci třídy, struktury, rozhraní nebo delegáta `unsafe` modifikátor, ve kterém případ celý textový rozsahu deklarace tohoto typu (včetně těla třídy, struktury nebo rozhraní) se považuje za nezabezpečený kontext.</span><span class="sxs-lookup"><span data-stu-id="eaf18-117">A declaration of a class, struct, interface, or delegate may include an `unsafe` modifier, in which case the entire textual extent of that type declaration (including the body of the class, struct, or interface) is considered an unsafe context.</span></span>
*  <span data-ttu-id="eaf18-118">Může obsahovat deklaraci pole, metody, vlastnosti, události, indexer, – operátor, konstruktor instance, – destruktor, nebo statický konstruktor `unsafe` modifikátor, ve kterém je případ celý textový rozsah této deklarace člena považovány za nebezpečné kontext.</span><span class="sxs-lookup"><span data-stu-id="eaf18-118">A declaration of a field, method, property, event, indexer, operator, instance constructor, destructor, or static constructor may include an `unsafe` modifier, in which case the entire textual extent of that member declaration is considered an unsafe context.</span></span>
*  <span data-ttu-id="eaf18-119">*Unsafe_statement* umožňuje použít v nezabezpečeném kontextu *bloku*.</span><span class="sxs-lookup"><span data-stu-id="eaf18-119">An *unsafe_statement* enables the use of an unsafe context within a *block*.</span></span> <span data-ttu-id="eaf18-120">Celý textový rozsahu přidruženého *bloku* se považuje za nezabezpečený kontext.</span><span class="sxs-lookup"><span data-stu-id="eaf18-120">The entire textual extent of the associated *block* is considered an unsafe context.</span></span>

<span data-ttu-id="eaf18-121">Níže se zobrazují výroby přidružené gramatiky.</span><span class="sxs-lookup"><span data-stu-id="eaf18-121">The associated grammar productions are shown below.</span></span>

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

<span data-ttu-id="eaf18-122">V příkladu</span><span class="sxs-lookup"><span data-stu-id="eaf18-122">In the example</span></span>

```csharp
public unsafe struct Node
{
    public int Value;
    public Node* Left;
    public Node* Right;
}
```

<span data-ttu-id="eaf18-123">`unsafe` modifikátor zadaného v deklaraci struktury způsobí, že se celý textový rozsahu deklarace struktury se nezabezpečený kontext.</span><span class="sxs-lookup"><span data-stu-id="eaf18-123">the `unsafe` modifier specified in the struct declaration causes the entire textual extent of the struct declaration to become an unsafe context.</span></span> <span data-ttu-id="eaf18-124">Proto je možné deklarovat `Left` a `Right` pole typu ukazatel.</span><span class="sxs-lookup"><span data-stu-id="eaf18-124">Thus, it is possible to declare the `Left` and `Right` fields to be of a pointer type.</span></span> <span data-ttu-id="eaf18-125">Výše uvedený příklad také zapsat.</span><span class="sxs-lookup"><span data-stu-id="eaf18-125">The example above could also be written</span></span>

```csharp
public struct Node
{
    public int Value;
    public unsafe Node* Left;
    public unsafe Node* Right;
}
```

<span data-ttu-id="eaf18-126">Tady `unsafe` modifikátorů v deklaracích pole způsobit, že tyto deklarace, aby bylo považováno za nezabezpečený kontext.</span><span class="sxs-lookup"><span data-stu-id="eaf18-126">Here, the `unsafe` modifiers in the field declarations cause those declarations to be considered unsafe contexts.</span></span>

<span data-ttu-id="eaf18-127">Kromě zřízení nezabezpečený kontext, tak umožňuje použití typů ukazatele `unsafe` modifikátor nemá žádný vliv na typ nebo člen.</span><span class="sxs-lookup"><span data-stu-id="eaf18-127">Other than establishing an unsafe context, thus permitting the use of pointer types, the `unsafe` modifier has no effect on a type or a member.</span></span> <span data-ttu-id="eaf18-128">V příkladu</span><span class="sxs-lookup"><span data-stu-id="eaf18-128">In the example</span></span>

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

<span data-ttu-id="eaf18-129">`unsafe` modifikátor `F` metoda ve `A` jednoduše způsobí, že textové rozsah `F` stane nezabezpečený kontext, ve které je možné nezabezpečené funkce jazyka.</span><span class="sxs-lookup"><span data-stu-id="eaf18-129">the `unsafe` modifier on the `F` method in `A` simply causes the textual extent of `F` to become an unsafe context in which the unsafe features of the language can be used.</span></span> <span data-ttu-id="eaf18-130">V přepsání z `F` v `B`, není nutné znovu zadat `unsafe` modifikátor – Pokud, samozřejmě `F` metoda ve `B` samotný potřebuje přístup k funkcím unsafe.</span><span class="sxs-lookup"><span data-stu-id="eaf18-130">In the override of `F` in `B`, there is no need to re-specify the `unsafe` modifier -- unless, of course, the `F` method in `B` itself needs access to unsafe features.</span></span>

<span data-ttu-id="eaf18-131">Situace se mírně liší, když typ ukazatele je součástí podpis metody</span><span class="sxs-lookup"><span data-stu-id="eaf18-131">The situation is slightly different when a pointer type is part of the method's signature</span></span>

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

<span data-ttu-id="eaf18-132">Tady protože `F`podpis obsahuje typ ukazatele, to je možné zapsat jen v nezabezpečeném kontextu.</span><span class="sxs-lookup"><span data-stu-id="eaf18-132">Here, because `F`'s signature includes a pointer type, it can only be written in an unsafe context.</span></span> <span data-ttu-id="eaf18-133">Však může být zavedeno nezabezpečeném kontextu provedením buď celá třída velmi nebezpečné, stejně jako v případě v `A`, nebo tak `unsafe` modifikátor v deklaraci metody, stejně jako v případě v `B`.</span><span class="sxs-lookup"><span data-stu-id="eaf18-133">However, the unsafe context can be introduced by either making the entire class unsafe, as is the case in `A`, or by including an `unsafe` modifier in the method declaration, as is the case in `B`.</span></span>

## <a name="pointer-types"></a><span data-ttu-id="eaf18-134">Typy ukazatelů</span><span class="sxs-lookup"><span data-stu-id="eaf18-134">Pointer types</span></span>

<span data-ttu-id="eaf18-135">V nezabezpečeném kontextu *typ* ([typy](types.md)) může být *pointer_type* a také *value_type* nebo *reference_type* .</span><span class="sxs-lookup"><span data-stu-id="eaf18-135">In an unsafe context, a *type* ([Types](types.md)) may be a *pointer_type* as well as a *value_type* or a *reference_type*.</span></span> <span data-ttu-id="eaf18-136">Ale *pointer_type* mohou být využity také v `typeof` výrazu ([anonymní objekt vytváření výrazů](expressions.md#anonymous-object-creation-expressions)) mimo nezabezpečeném kontextu. v důsledku použití není unsafe.</span><span class="sxs-lookup"><span data-stu-id="eaf18-136">However, a *pointer_type* may also be used in a `typeof` expression ([Anonymous object creation expressions](expressions.md#anonymous-object-creation-expressions)) outside of an unsafe context as such usage is not unsafe.</span></span>

```antlr
type_unsafe
    : pointer_type
    ;
```

<span data-ttu-id="eaf18-137">A *pointer_type* je zapsán jako *unmanaged_type* nebo klíčové slovo `void`a po něm `*` token:</span><span class="sxs-lookup"><span data-stu-id="eaf18-137">A *pointer_type* is written as an *unmanaged_type* or the keyword `void`, followed by a `*` token:</span></span>

```antlr
pointer_type
    : unmanaged_type '*'
    | 'void' '*'
    ;

unmanaged_type
    : type
    ;
```

<span data-ttu-id="eaf18-138">Typ určený před `*` na ukazatel typu nazývá ***referenční typ*** typu ukazatele.</span><span class="sxs-lookup"><span data-stu-id="eaf18-138">The type specified before the `*` in a pointer type is called the ***referent type*** of the pointer type.</span></span> <span data-ttu-id="eaf18-139">Představuje typ proměnné, na kterou ukazuje hodnotu typu ukazatel.</span><span class="sxs-lookup"><span data-stu-id="eaf18-139">It represents the type of the variable to which a value of the pointer type points.</span></span>

<span data-ttu-id="eaf18-140">Na rozdíl od odkazy (hodnoty typy odkazů) nebudou pro účely systémem uvolňování ukazatele – systému uvolňování paměti je vůbec nezná ukazatele a data, na který odkazují.</span><span class="sxs-lookup"><span data-stu-id="eaf18-140">Unlike references (values of reference types), pointers are not tracked by the garbage collector -- the garbage collector has no knowledge of pointers and the data to which they point.</span></span> <span data-ttu-id="eaf18-141">Z tohoto důvodu ukazatel není povolené tak, aby odkazoval na odkaz nebo na strukturu, která obsahuje odkazy, a musí být typu referenční ukazatel *unmanaged_type*.</span><span class="sxs-lookup"><span data-stu-id="eaf18-141">For this reason a pointer is not permitted to point to a reference or to a struct that contains references, and the referent type of a pointer must be an *unmanaged_type*.</span></span>

<span data-ttu-id="eaf18-142">*Unmanaged_type* je libovolný typ, který není *reference_type* nebo konstruovaný typ. a neobsahuje *reference_type* nebo konstruovaný typ pole na libovolné úrovni vnoření.</span><span class="sxs-lookup"><span data-stu-id="eaf18-142">An *unmanaged_type* is any type that isn't a *reference_type* or constructed type, and doesn't contain *reference_type* or constructed type fields at any level of nesting.</span></span> <span data-ttu-id="eaf18-143">Jinými slovy *unmanaged_type* je jedním z následujících akcí:</span><span class="sxs-lookup"><span data-stu-id="eaf18-143">In other words, an *unmanaged_type* is one of the following:</span></span>

*  <span data-ttu-id="eaf18-144">`sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, `decimal`, nebo `bool`.</span><span class="sxs-lookup"><span data-stu-id="eaf18-144">`sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, `decimal`, or `bool`.</span></span>
*  <span data-ttu-id="eaf18-145">Žádné *enum_type*.</span><span class="sxs-lookup"><span data-stu-id="eaf18-145">Any *enum_type*.</span></span>
*  <span data-ttu-id="eaf18-146">Any *pointer_type*.</span><span class="sxs-lookup"><span data-stu-id="eaf18-146">Any *pointer_type*.</span></span>
*  <span data-ttu-id="eaf18-147">Všechny uživatelem definované *struct_type* , který není konstruovaný typ a obsahuje pole *unmanaged_type*pouze s.</span><span class="sxs-lookup"><span data-stu-id="eaf18-147">Any user-defined *struct_type* that is not a constructed type and contains fields of *unmanaged_type*s only.</span></span>

<span data-ttu-id="eaf18-148">Intuitivní pravidlo pro kombinování ukazatele a reference je, že referents odkazů (objektů) jsou povolené tak, aby obsahovala ukazatele, ale tak, aby obsahovala odkazy nejsou povoleny referents ukazatelů.</span><span class="sxs-lookup"><span data-stu-id="eaf18-148">The intuitive rule for mixing of pointers and references is that referents of references (objects) are permitted to contain pointers, but referents of pointers are not permitted to contain references.</span></span>

<span data-ttu-id="eaf18-149">V následující tabulce jsou uvedeny některé příklady typů ukazatelů:</span><span class="sxs-lookup"><span data-stu-id="eaf18-149">Some examples of pointer types are given in the table below:</span></span>

| <span data-ttu-id="eaf18-150">__Příklad__</span><span class="sxs-lookup"><span data-stu-id="eaf18-150">__Example__</span></span> | <span data-ttu-id="eaf18-151">__Popis__</span><span class="sxs-lookup"><span data-stu-id="eaf18-151">__Description__</span></span>                               |
|-------------|-----------------------------------------------|
| `byte*`     | <span data-ttu-id="eaf18-152">ukazatel na `byte`</span><span class="sxs-lookup"><span data-stu-id="eaf18-152">Pointer to `byte`</span></span>                             |
| `char*`     | <span data-ttu-id="eaf18-153">ukazatel na `char`</span><span class="sxs-lookup"><span data-stu-id="eaf18-153">Pointer to `char`</span></span>                             |
| `int**`     | <span data-ttu-id="eaf18-154">Ukazatel na ukazatel na `int`</span><span class="sxs-lookup"><span data-stu-id="eaf18-154">Pointer to pointer to `int`</span></span>                   |
| `int*[]`    | <span data-ttu-id="eaf18-155">Jednorozměrné pole ukazatelů na `int`</span><span class="sxs-lookup"><span data-stu-id="eaf18-155">Single-dimensional array of pointers to `int`</span></span> |
| `void*`     | <span data-ttu-id="eaf18-156">Ukazatel na neznámý typ.</span><span class="sxs-lookup"><span data-stu-id="eaf18-156">Pointer to unknown type</span></span>                       |

<span data-ttu-id="eaf18-157">Pro danou implementaci všechny typy ukazatelů musí mít stejné velikosti a reprezentace.</span><span class="sxs-lookup"><span data-stu-id="eaf18-157">For a given implementation, all pointer types must have the same size and representation.</span></span>

<span data-ttu-id="eaf18-158">Na rozdíl od jazyka C a C++, když jsou deklarovány většího počtu ukazatelů ve stejné deklaraci v jazyce C# `*` je zapsán spolu s základní typ, ne jako předpona interpunkci na názvů jednotlivých ukazatelů.</span><span class="sxs-lookup"><span data-stu-id="eaf18-158">Unlike C and C++, when multiple pointers are declared in the same declaration, in C# the `*` is written along with the underlying type only, not as a prefix punctuator on each pointer name.</span></span> <span data-ttu-id="eaf18-159">Příklad</span><span class="sxs-lookup"><span data-stu-id="eaf18-159">For example</span></span>

```csharp
int* pi, pj;    // NOT as int *pi, *pj;
```

<span data-ttu-id="eaf18-160">Hodnota ukazatele s typem `T*` představuje adresu proměnné typu `T`.</span><span class="sxs-lookup"><span data-stu-id="eaf18-160">The value of a pointer having type `T*` represents the address of a variable of type `T`.</span></span> <span data-ttu-id="eaf18-161">Operátor dereference ukazatele `*` ([dereferenci ukazatele](unsafe-code.md#pointer-indirection)) slouží pro přístup k této proměnné.</span><span class="sxs-lookup"><span data-stu-id="eaf18-161">The pointer indirection operator `*` ([Pointer indirection](unsafe-code.md#pointer-indirection)) may be used to access this variable.</span></span> <span data-ttu-id="eaf18-162">Mějme například proměnná `P` typu `int*`, výraz `*P` označuje `int` nalezenou v adrese obsažené v proměnné `P`.</span><span class="sxs-lookup"><span data-stu-id="eaf18-162">For example, given a variable `P` of type `int*`, the expression `*P` denotes the `int` variable found at the address contained in `P`.</span></span>

<span data-ttu-id="eaf18-163">Jako odkaz na objekt může být ukazatel `null`.</span><span class="sxs-lookup"><span data-stu-id="eaf18-163">Like an object reference, a pointer may be `null`.</span></span> <span data-ttu-id="eaf18-164">Použití operátoru dereference na `null` ukazatel výsledkem chování definované implementací.</span><span class="sxs-lookup"><span data-stu-id="eaf18-164">Applying the indirection operator to a `null` pointer results in implementation-defined behavior.</span></span> <span data-ttu-id="eaf18-165">Ukazatel s hodnotou `null` je reprezentována všechny bity nula.</span><span class="sxs-lookup"><span data-stu-id="eaf18-165">A pointer with value `null` is represented by all-bits-zero.</span></span>

<span data-ttu-id="eaf18-166">`void*` Typ představuje ukazatel na neznámého typu.</span><span class="sxs-lookup"><span data-stu-id="eaf18-166">The `void*` type represents a pointer to an unknown type.</span></span> <span data-ttu-id="eaf18-167">Protože referenční typ není znám, operátor dereference nelze použít na ukazatel typu `void*`, ani žádné aritmetický provést na takový ukazatel.</span><span class="sxs-lookup"><span data-stu-id="eaf18-167">Because the referent type is unknown, the indirection operator cannot be applied to a pointer of type `void*`, nor can any arithmetic be performed on such a pointer.</span></span> <span data-ttu-id="eaf18-168">Ale ukazatel typu `void*` může být převeden na jiný typ ukazatele (a naopak).</span><span class="sxs-lookup"><span data-stu-id="eaf18-168">However, a pointer of type `void*` can be cast to any other pointer type (and vice versa).</span></span>

<span data-ttu-id="eaf18-169">Typy ukazatelů jsou samostatné kategorie typů.</span><span class="sxs-lookup"><span data-stu-id="eaf18-169">Pointer types are a separate category of types.</span></span> <span data-ttu-id="eaf18-170">Na rozdíl od typy hodnot a odkazové typy, typy ukazatelů nedědí z `object` a neexistuje žádná možnost převodu mezi typy ukazatelů a `object`.</span><span class="sxs-lookup"><span data-stu-id="eaf18-170">Unlike reference types and value types, pointer types do not inherit from `object` and no conversions exist between pointer types and `object`.</span></span> <span data-ttu-id="eaf18-171">Zejména, zabalení a rozbalení ([zabalení a rozbalení](types.md#boxing-and-unboxing)) nejsou podporovány pro ukazatele.</span><span class="sxs-lookup"><span data-stu-id="eaf18-171">In particular, boxing and unboxing ([Boxing and unboxing](types.md#boxing-and-unboxing)) are not supported for pointers.</span></span> <span data-ttu-id="eaf18-172">Však povoleno převody mezi typy ukazatelů různých a mezi typy ukazatele a integrálními typy.</span><span class="sxs-lookup"><span data-stu-id="eaf18-172">However, conversions are permitted between different pointer types and between pointer types and the integral types.</span></span> <span data-ttu-id="eaf18-173">To je popsáno v [převody ukazatele](unsafe-code.md#pointer-conversions).</span><span class="sxs-lookup"><span data-stu-id="eaf18-173">This is described in [Pointer conversions](unsafe-code.md#pointer-conversions).</span></span>

<span data-ttu-id="eaf18-174">A *pointer_type* nelze použít jako argument typu ([vytvořený typy](types.md#constructed-types)) a odvození typu ([odvození typu](expressions.md#type-inference)) je neúspěšná na volání obecné metody, která bude mít odvodit typ argumentu na typ ukazatele.</span><span class="sxs-lookup"><span data-stu-id="eaf18-174">A *pointer_type* cannot be used as a type argument ([Constructed types](types.md#constructed-types)), and type inference ([Type inference](expressions.md#type-inference)) fails on generic method calls that would have inferred a type argument to be a pointer type.</span></span>

<span data-ttu-id="eaf18-175">A *pointer_type* může sloužit jako typ pole s modifikátorem volatile ([pole s modifikátorem Volatile](classes.md#volatile-fields)).</span><span class="sxs-lookup"><span data-stu-id="eaf18-175">A *pointer_type* may be used as the type of a volatile field ([Volatile fields](classes.md#volatile-fields)).</span></span>

<span data-ttu-id="eaf18-176">I když ukazatele mohou být předány jako `ref` nebo `out` parametry, díky tomu může způsobit nedefinované chování, protože ukazatel může dobře nastavit tak, aby odkazoval na místní proměnná, která již existuje návratu volané metody nebo pevné objektu, na který sloužící k odkazovaní, už nebude vyřešen.</span><span class="sxs-lookup"><span data-stu-id="eaf18-176">Although pointers can be passed as `ref` or `out` parameters, doing so can cause undefined behavior, since the pointer may well be set to point to a local variable which no longer exists when the called method returns, or the fixed object to which it used to point, is no longer fixed.</span></span> <span data-ttu-id="eaf18-177">Příklad:</span><span class="sxs-lookup"><span data-stu-id="eaf18-177">For example:</span></span>

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

<span data-ttu-id="eaf18-178">Metoda může vrátit hodnotu typu a typ může být ukazatel.</span><span class="sxs-lookup"><span data-stu-id="eaf18-178">A method can return a value of some type, and that type can be a pointer.</span></span> <span data-ttu-id="eaf18-179">Například když dán ukazatel souvislý sekvence `int`s, počet prvků tohoto pořadí a některé další `int` hodnoty, následující metodu vrátí adresu této hodnoty v pořadí, pokud je nalezena shoda; v opačném případě vrátí `null`:</span><span class="sxs-lookup"><span data-stu-id="eaf18-179">For example, when given a pointer to a contiguous sequence of `int`s, that sequence's element count, and some other `int` value, the following method returns the address of that value in that sequence, if a match occurs; otherwise it returns `null`:</span></span>

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

<span data-ttu-id="eaf18-180">V nezabezpečeném kontextu jsou k dispozici pro provozování na ukazatelích několik konstruktorů:</span><span class="sxs-lookup"><span data-stu-id="eaf18-180">In an unsafe context, several constructs are available for operating on pointers:</span></span>

*  <span data-ttu-id="eaf18-181">`*` Operátor může být použit provést dereferenci ukazatele ([dereferenci ukazatele](unsafe-code.md#pointer-indirection)).</span><span class="sxs-lookup"><span data-stu-id="eaf18-181">The `*` operator may be used to perform pointer indirection ([Pointer indirection](unsafe-code.md#pointer-indirection)).</span></span>
*  <span data-ttu-id="eaf18-182">`->` Operátor může být použit pro přístup ke členu struktury prostřednictvím ukazatele ([přístupu k členovi](unsafe-code.md#pointer-member-access)).</span><span class="sxs-lookup"><span data-stu-id="eaf18-182">The `->` operator may be used to access a member of a struct through a pointer ([Pointer member access](unsafe-code.md#pointer-member-access)).</span></span>
*  <span data-ttu-id="eaf18-183">`[]` Operátor může použít k indexování ukazatel ([přístup k prvkům ukazatel](unsafe-code.md#pointer-element-access)).</span><span class="sxs-lookup"><span data-stu-id="eaf18-183">The `[]` operator may be used to index a pointer ([Pointer element access](unsafe-code.md#pointer-element-access)).</span></span>
*  <span data-ttu-id="eaf18-184">`&` Operátor může být použit pro získání adresy proměnné ([operátoru address-of](unsafe-code.md#the-address-of-operator)).</span><span class="sxs-lookup"><span data-stu-id="eaf18-184">The `&` operator may be used to obtain the address of a variable ([The address-of operator](unsafe-code.md#the-address-of-operator)).</span></span>
*  <span data-ttu-id="eaf18-185">`++` a `--` lze operátory Inkrementace a dekrementace ukazatelů ([ukazatel Inkrementace a dekrementace](unsafe-code.md#pointer-increment-and-decrement)).</span><span class="sxs-lookup"><span data-stu-id="eaf18-185">The `++` and `--` operators may be used to increment and decrement pointers ([Pointer increment and decrement](unsafe-code.md#pointer-increment-and-decrement)).</span></span>
*  <span data-ttu-id="eaf18-186">`+` a `-` operátory lze provést aritmetiku ukazatele ([aritmetické operace ukazatele](unsafe-code.md#pointer-arithmetic)).</span><span class="sxs-lookup"><span data-stu-id="eaf18-186">The `+` and `-` operators may be used to perform pointer arithmetic ([Pointer arithmetic](unsafe-code.md#pointer-arithmetic)).</span></span>
*  <span data-ttu-id="eaf18-187">`==`, `!=`, `<`, `>`, `<=`, A `=>` operátory může použít k porovnání ukazatelů ([porovnání ukazatelů](unsafe-code.md#pointer-comparison)).</span><span class="sxs-lookup"><span data-stu-id="eaf18-187">The `==`, `!=`, `<`, `>`, `<=`, and `=>` operators may be used to compare pointers ([Pointer comparison](unsafe-code.md#pointer-comparison)).</span></span>
*  <span data-ttu-id="eaf18-188">`stackalloc` Operátor může být použit k přidělení paměti ze zásobníku volání ([pevných vyrovnávacích pamětí velikost](unsafe-code.md#fixed-size-buffers)).</span><span class="sxs-lookup"><span data-stu-id="eaf18-188">The `stackalloc` operator may be used to allocate memory from the call stack ([Fixed size buffers](unsafe-code.md#fixed-size-buffers)).</span></span>
*  <span data-ttu-id="eaf18-189">`fixed` Příkaz se dá použít dočasně vyřešit proměnnou tak můžete získat adresu ([příkazu fixed](unsafe-code.md#the-fixed-statement)).</span><span class="sxs-lookup"><span data-stu-id="eaf18-189">The `fixed` statement may be used to temporarily fix a variable so its address can be obtained ([The fixed statement](unsafe-code.md#the-fixed-statement)).</span></span>

## <a name="fixed-and-moveable-variables"></a><span data-ttu-id="eaf18-190">Oprava a přesunutelný proměnné</span><span class="sxs-lookup"><span data-stu-id="eaf18-190">Fixed and moveable variables</span></span>

<span data-ttu-id="eaf18-191">Operátor address-of ([operátoru address-of](unsafe-code.md#the-address-of-operator)) a `fixed` – příkaz ([příkazu fixed](unsafe-code.md#the-fixed-statement)) proměnné rozdělit do dvou kategorií: ***Oprava proměnné*** a ***přesunutelný proměnné***.</span><span class="sxs-lookup"><span data-stu-id="eaf18-191">The address-of operator ([The address-of operator](unsafe-code.md#the-address-of-operator)) and the `fixed` statement ([The fixed statement](unsafe-code.md#the-fixed-statement)) divide variables into two categories: ***Fixed variables*** and ***moveable variables***.</span></span>

<span data-ttu-id="eaf18-192">Oprava proměnné se nacházejí v umístění úložiště, které jsou ovlivněny operace systému uvolňování paměti.</span><span class="sxs-lookup"><span data-stu-id="eaf18-192">Fixed variables reside in storage locations that are unaffected by operation of the garbage collector.</span></span> <span data-ttu-id="eaf18-193">(Pevné proměnné příklady lokální proměnné, parametry s hodnotou a proměnných vytvořené pomocí přesměrování ukazatele.) Na druhé straně přesunutelný proměnné se nacházejí v umístění úložiště, které jsou v souladu s přemístění nebo vyřazení systémem uvolňování.</span><span class="sxs-lookup"><span data-stu-id="eaf18-193">(Examples of fixed variables include local variables, value parameters, and variables created by dereferencing pointers.) On the other hand, moveable variables reside in storage locations that are subject to relocation or disposal by the garbage collector.</span></span> <span data-ttu-id="eaf18-194">(Příklady přesunutelný proměnných zahrnout pole objektů a prvky pole.)</span><span class="sxs-lookup"><span data-stu-id="eaf18-194">(Examples of moveable variables include fields in objects and elements of arrays.)</span></span>

<span data-ttu-id="eaf18-195">`&` – Operátor ([operátoru address-of](unsafe-code.md#the-address-of-operator)) umožňuje adresu pevná proměnná ho získat bez omezení.</span><span class="sxs-lookup"><span data-stu-id="eaf18-195">The `&` operator ([The address-of operator](unsafe-code.md#the-address-of-operator)) permits the address of a fixed variable to be obtained without restrictions.</span></span> <span data-ttu-id="eaf18-196">Ale protože přesunutelný proměnná je v souladu s přemístění nebo vyřazení modulem garbage collector, adresy přesunutelný proměnné je možné získat pomocí `fixed` – příkaz ([příkazu fixed](unsafe-code.md#the-fixed-statement)) a tuto adresu zůstane platný pouze po dobu trvání, která `fixed` příkazu.</span><span class="sxs-lookup"><span data-stu-id="eaf18-196">However, because a moveable variable is subject to relocation or disposal by the garbage collector, the address of a moveable variable can only be obtained using a `fixed` statement ([The fixed statement](unsafe-code.md#the-fixed-statement)), and that address remains valid only for the duration of that `fixed` statement.</span></span>

<span data-ttu-id="eaf18-197">Přesně řečeno je pevná proměnná jednu z následujících akcí:</span><span class="sxs-lookup"><span data-stu-id="eaf18-197">In precise terms, a fixed variable is one of the following:</span></span>

*  <span data-ttu-id="eaf18-198">Proměnné vyplývající z *simple_name* ([jednoduché názvy](expressions.md#simple-names)), který odkazuje na místní proměnná nebo parametr hodnoty, pokud není zachycena proměnné anonymní funkce.</span><span class="sxs-lookup"><span data-stu-id="eaf18-198">A variable resulting from a *simple_name* ([Simple names](expressions.md#simple-names)) that refers to a local variable or a value parameter, unless the variable is captured by an anonymous function.</span></span>
*  <span data-ttu-id="eaf18-199">Proměnné vyplývající z *member_access* ([přístup ke členu](expressions.md#member-access)) ve formátu `V.I`, kde `V` je pevná proměnná *struct_type*.</span><span class="sxs-lookup"><span data-stu-id="eaf18-199">A variable resulting from a *member_access* ([Member access](expressions.md#member-access)) of the form `V.I`, where `V` is a fixed variable of a *struct_type*.</span></span>
*  <span data-ttu-id="eaf18-200">Proměnné vyplývající z *pointer_indirection_expression* ([dereferenci ukazatele](unsafe-code.md#pointer-indirection)) ve formátu `*P`, *pointer_member_access* ([Přístupu k členovi](unsafe-code.md#pointer-member-access)) ve formátu `P->I`, nebo *pointer_element_access* ([přístup k prvkům ukazatel](unsafe-code.md#pointer-element-access)) ve formátu `P[E]`.</span><span class="sxs-lookup"><span data-stu-id="eaf18-200">A variable resulting from a *pointer_indirection_expression* ([Pointer indirection](unsafe-code.md#pointer-indirection)) of the form `*P`, a *pointer_member_access* ([Pointer member access](unsafe-code.md#pointer-member-access)) of the form `P->I`, or a *pointer_element_access* ([Pointer element access](unsafe-code.md#pointer-element-access)) of the form `P[E]`.</span></span>

<span data-ttu-id="eaf18-201">Všechny ostatní proměnné jsou klasifikovány jako přesunutelný proměnné.</span><span class="sxs-lookup"><span data-stu-id="eaf18-201">All other variables are classified as moveable variables.</span></span>

<span data-ttu-id="eaf18-202">Všimněte si, že statické pole je klasifikován tak přesunutelný proměnné.</span><span class="sxs-lookup"><span data-stu-id="eaf18-202">Note that a static field is classified as a moveable variable.</span></span> <span data-ttu-id="eaf18-203">Všimněte si také, že `ref` nebo `out` parametr klasifikovaný jako proměnnou přesunutelný i v případě, že je argument zadaný pro parametr pevná proměnná.</span><span class="sxs-lookup"><span data-stu-id="eaf18-203">Also note that a `ref` or `out` parameter is classified as a moveable variable, even if the argument given for the parameter is a fixed variable.</span></span> <span data-ttu-id="eaf18-204">Nakonec Upozorňujeme, že proměnné vytvořené metodou odkazován ukazatelem je vždy jsou klasifikovány jako pevná proměnná.</span><span class="sxs-lookup"><span data-stu-id="eaf18-204">Finally, note that a variable produced by dereferencing a pointer is always classified as a fixed variable.</span></span>

## <a name="pointer-conversions"></a><span data-ttu-id="eaf18-205">Převody ukazatele</span><span class="sxs-lookup"><span data-stu-id="eaf18-205">Pointer conversions</span></span>

<span data-ttu-id="eaf18-206">V nezabezpečeném kontextu, sady k dispozici implicitní převody ([implicitních převodů](conversions.md#implicit-conversions)) je rozšířen o následující převody implicitní ukazatel:</span><span class="sxs-lookup"><span data-stu-id="eaf18-206">In an unsafe context, the set of available implicit conversions ([Implicit conversions](conversions.md#implicit-conversions)) is extended to include the following implicit pointer conversions:</span></span>

*  <span data-ttu-id="eaf18-207">Z libovolného *pointer_type* typu `void*`.</span><span class="sxs-lookup"><span data-stu-id="eaf18-207">From any *pointer_type* to the type `void*`.</span></span>
*  <span data-ttu-id="eaf18-208">Z `null` literál k libovolnému *pointer_type*.</span><span class="sxs-lookup"><span data-stu-id="eaf18-208">From the `null` literal to any *pointer_type*.</span></span>

<span data-ttu-id="eaf18-209">Kromě toho v nezabezpečeném kontextu, sady k dispozici explicitní převody ([explicitních převodů](conversions.md#explicit-conversions)) je rozšířen o následující převody explicitních ukazatele:</span><span class="sxs-lookup"><span data-stu-id="eaf18-209">Additionally, in an unsafe context, the set of available explicit conversions ([Explicit conversions](conversions.md#explicit-conversions)) is extended to include the following explicit pointer conversions:</span></span>

*  <span data-ttu-id="eaf18-210">Z libovolného *pointer_type* u kteréhokoli jiného *pointer_type*.</span><span class="sxs-lookup"><span data-stu-id="eaf18-210">From any *pointer_type* to any other *pointer_type*.</span></span>
*  <span data-ttu-id="eaf18-211">Z `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, nebo `ulong` k libovolnému *pointer_type*.</span><span class="sxs-lookup"><span data-stu-id="eaf18-211">From `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, or `ulong` to any *pointer_type*.</span></span>
*  <span data-ttu-id="eaf18-212">Z libovolného *pointer_type* k `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, nebo `ulong`.</span><span class="sxs-lookup"><span data-stu-id="eaf18-212">From any *pointer_type* to `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, or `ulong`.</span></span>

<span data-ttu-id="eaf18-213">Nakonec v nezabezpečeném kontextu, sadu standardních implicitní převody ([standardní implicitní převody](conversions.md#standard-implicit-conversions)) zahrnuje následující převod ukazatelů:</span><span class="sxs-lookup"><span data-stu-id="eaf18-213">Finally, in an unsafe context, the set of standard implicit conversions ([Standard implicit conversions](conversions.md#standard-implicit-conversions)) includes the following pointer conversion:</span></span>

*  <span data-ttu-id="eaf18-214">Z libovolného *pointer_type* typu `void*`.</span><span class="sxs-lookup"><span data-stu-id="eaf18-214">From any *pointer_type* to the type `void*`.</span></span>

<span data-ttu-id="eaf18-215">Převody mezi typy ukazatelů, dva nikdy nezmění hodnotu skutečné ukazatele.</span><span class="sxs-lookup"><span data-stu-id="eaf18-215">Conversions between two pointer types never change the actual pointer value.</span></span> <span data-ttu-id="eaf18-216">Jinými slovy převod z typu jeden ukazatel na jiný nemá žádný vliv na základní adrese dán ukazatel.</span><span class="sxs-lookup"><span data-stu-id="eaf18-216">In other words, a conversion from one pointer type to another has no effect on the underlying address given by the pointer.</span></span>

<span data-ttu-id="eaf18-217">Pokud jeden typ ukazatele je převeden na jiný, v případě, že výsledný ukazatel není pro typ odkazovala na správně zarovnán, chování není definováno, pokud je přistoupit přes ukazatel výsledku.</span><span class="sxs-lookup"><span data-stu-id="eaf18-217">When one pointer type is converted to another, if the resulting pointer is not correctly aligned for the pointed-to type, the behavior is undefined if the result is dereferenced.</span></span> <span data-ttu-id="eaf18-218">Obecně je přenositelný koncept "správně zarovnané": Pokud na ukazatel na typ `A` správně zarovnán ukazatele na typ `B`, který pak je správně zarovnán ukazatele na typ `C`, pak ukazatel na typ `A`správně zarovnán ukazatele na typ `C`.</span><span class="sxs-lookup"><span data-stu-id="eaf18-218">In general, the concept "correctly aligned" is transitive: if a pointer to type `A` is correctly aligned for a pointer to type `B`, which, in turn, is correctly aligned for a pointer to type `C`, then a pointer to type `A` is correctly aligned for a pointer to type `C`.</span></span>

<span data-ttu-id="eaf18-219">Zvažte následující případ, ve kterém se proměnná s jednoho typu přistupuje přes ukazatel na jiný typ:</span><span class="sxs-lookup"><span data-stu-id="eaf18-219">Consider the following case in which a variable having one type is accessed via a pointer to a different type:</span></span>

```csharp
char c = 'A';
char* pc = &c;
void* pv = pc;
int* pi = (int*)pv;
int i = *pi;         // undefined
*pi = 123456;        // undefined
```

<span data-ttu-id="eaf18-220">Pokud typ ukazatele převést na ukazatel bajt, výsledek body na nejnižší bajt adresovaný proměnné.</span><span class="sxs-lookup"><span data-stu-id="eaf18-220">When a pointer type is converted to a pointer to byte, the result points to the lowest addressed byte of the variable.</span></span> <span data-ttu-id="eaf18-221">Postupné přírůstky výsledku, až do velikosti proměnné, zobrazit odkazy na zbývající bajty proměnné.</span><span class="sxs-lookup"><span data-stu-id="eaf18-221">Successive increments of the result, up to the size of the variable, yield pointers to the remaining bytes of that variable.</span></span> <span data-ttu-id="eaf18-222">Například následující metoda zobrazí každých osm bajtů v typu double jako šestnáctkovou hodnotu:</span><span class="sxs-lookup"><span data-stu-id="eaf18-222">For example, the following method displays each of the eight bytes in a double as a hexadecimal value:</span></span>

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

<span data-ttu-id="eaf18-223">Výstup vytvořený samozřejmě závisí na ukládání významných bajtů.</span><span class="sxs-lookup"><span data-stu-id="eaf18-223">Of course, the output produced depends on endianness.</span></span>

<span data-ttu-id="eaf18-224">Mapování mezi ukazatele a celá čísla jsou definované implementací.</span><span class="sxs-lookup"><span data-stu-id="eaf18-224">Mappings between pointers and integers are implementation-defined.</span></span> <span data-ttu-id="eaf18-225">Ale na 32 \* a 64-bit CPU architektury pomocí lineárního adresního prostoru, převody ukazatelů na nebo z celočíselných typů chovají obvykle úplně stejně jako převody `uint` nebo `ulong` hodnoty, resp. do nebo z těchto celočíselných typů.</span><span class="sxs-lookup"><span data-stu-id="eaf18-225">However, on 32\* and 64-bit CPU architectures with a linear address space, conversions of pointers to or from integral types typically behave exactly like conversions of `uint` or `ulong` values, respectively, to or from those integral types.</span></span>

### <a name="pointer-arrays"></a><span data-ttu-id="eaf18-226">Pole ukazatelů</span><span class="sxs-lookup"><span data-stu-id="eaf18-226">Pointer arrays</span></span>

<span data-ttu-id="eaf18-227">V nezabezpečeném kontextu lze sestavit pole ukazatelů.</span><span class="sxs-lookup"><span data-stu-id="eaf18-227">In an unsafe context, arrays of pointers can be constructed.</span></span> <span data-ttu-id="eaf18-228">Pro pole ukazatelů jsou povolené jenom některé z převody, které se vztahují na jiné typy polí:</span><span class="sxs-lookup"><span data-stu-id="eaf18-228">Only some of the conversions that apply to other array types are allowed on pointer arrays:</span></span>

*  <span data-ttu-id="eaf18-229">Implicitní referenční převod ([odkaz na implicitní převody](conversions.md#implicit-reference-conversions)) z jakékoli *array_type* k `System.Array` a také implementuje rozhraní se vztahuje na ukazatel pole.</span><span class="sxs-lookup"><span data-stu-id="eaf18-229">The implicit reference conversion ([Implicit reference conversions](conversions.md#implicit-reference-conversions)) from any *array_type* to `System.Array` and the interfaces it implements also applies to pointer arrays.</span></span> <span data-ttu-id="eaf18-230">Ale žádný pokus o přístup k prvkům pole pomocí `System.Array` nebo rozhraní implementuje způsobí výjimku za běhu, protože nejsou převoditelné na typy ukazatelů `object`.</span><span class="sxs-lookup"><span data-stu-id="eaf18-230">However, any attempt to access the array elements through `System.Array` or the interfaces it implements will result in an exception at run-time, as pointer types are not convertible to `object`.</span></span>
*  <span data-ttu-id="eaf18-231">Odkazovat na implicitní a explicitní převody ([odkaz na implicitní převody](conversions.md#implicit-reference-conversions), [odkaz na explicitní převody](conversions.md#explicit-reference-conversions)) z typu jednorozměrné pole `S[]` k `System.Collections.Generic.IList<T>` a jeho obecné základní rozhraní se nikdy nepoužívejte k polím ukazatel, protože typy ukazatelů nelze použít jako argumenty typu a nejsou žádné převody z typů ukazatele na typech bez ukazatele typy.</span><span class="sxs-lookup"><span data-stu-id="eaf18-231">The implicit and explicit reference conversions ([Implicit reference conversions](conversions.md#implicit-reference-conversions), [Explicit reference conversions](conversions.md#explicit-reference-conversions)) from a single-dimensional array type `S[]` to `System.Collections.Generic.IList<T>` and its generic base interfaces never apply to pointer arrays, since pointer types cannot be used as type arguments, and there are no conversions from pointer types to non-pointer types.</span></span>
*  <span data-ttu-id="eaf18-232">Převod explicitní odkaz ([odkaz na explicitní převody](conversions.md#explicit-reference-conversions)) z `System.Array` a rozhraní implementuje k libovolnému *array_type* platí pro pole ukazatel.</span><span class="sxs-lookup"><span data-stu-id="eaf18-232">The explicit reference conversion ([Explicit reference conversions](conversions.md#explicit-reference-conversions)) from `System.Array` and the interfaces it implements to any *array_type* applies to pointer arrays.</span></span>
*  <span data-ttu-id="eaf18-233">Odkaz na explicitní převody ([převody explicitní odkaz](conversions.md#explicit-reference-conversions)) z `System.Collections.Generic.IList<S>` a její základní rozhraní pro typ jednorozměrné pole `T[]` nikdy platí pro pole ukazatel, protože nemůže být typy ukazatelů použít jako argumenty typu a nejsou žádné převody z typů ukazatele na typech bez ukazatele typy.</span><span class="sxs-lookup"><span data-stu-id="eaf18-233">The explicit reference conversions ([Explicit reference conversions](conversions.md#explicit-reference-conversions)) from `System.Collections.Generic.IList<S>` and its base interfaces to a single-dimensional array type `T[]` never applies to pointer arrays, since pointer types cannot be used as type arguments, and there are no conversions from pointer types to non-pointer types.</span></span>

<span data-ttu-id="eaf18-234">Tato omezení znamenají, že rozšíření pro `foreach` příkaz v polích podle [příkazu foreach](statements.md#the-foreach-statement) nejde použít u polí ukazatele.</span><span class="sxs-lookup"><span data-stu-id="eaf18-234">These restrictions mean that the expansion for the `foreach` statement over arrays described in [The foreach statement](statements.md#the-foreach-statement) cannot be applied to pointer arrays.</span></span> <span data-ttu-id="eaf18-235">Místo toho příkaz foreach formuláře</span><span class="sxs-lookup"><span data-stu-id="eaf18-235">Instead, a foreach statement of the form</span></span>

```csharp
foreach (V v in x) embedded_statement
```

<span data-ttu-id="eaf18-236">Pokud typ `x` je typ pole formuláře `T[,,...,]`, `N` je počet rozměrů minus 1 a `T` nebo `V` je typ ukazatele, rozbalení vnořené pro smyčky následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="eaf18-236">where the type of `x` is an array type of the form `T[,,...,]`, `N` is the number of dimensions minus 1 and `T` or `V` is a pointer type, is expanded using nested for-loops as follows:</span></span>

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

<span data-ttu-id="eaf18-237">Proměnné `a`, `i0`, `i1`,..., `iN` nejsou přístupné nebo viditelné pro `x` nebo *embedded_statement* nebo jiný zdrojový kód programu.</span><span class="sxs-lookup"><span data-stu-id="eaf18-237">The variables `a`, `i0`, `i1`, ..., `iN` are not visible to or accessible to `x` or the *embedded_statement* or any other source code of the program.</span></span> <span data-ttu-id="eaf18-238">Proměnná `v` je jen pro čtení v vloženým příkazem.</span><span class="sxs-lookup"><span data-stu-id="eaf18-238">The variable `v` is read-only in the embedded statement.</span></span> <span data-ttu-id="eaf18-239">Pokud není k dispozici explicitní převod ([převody ukazatele](unsafe-code.md#pointer-conversions)) z `T` (typ prvku) pro `V`, je vytvořen chybu a jsou přesměrováni žádné další kroky.</span><span class="sxs-lookup"><span data-stu-id="eaf18-239">If there is not an explicit conversion ([Pointer conversions](unsafe-code.md#pointer-conversions)) from `T` (the element type) to `V`, an error is produced and no further steps are taken.</span></span> <span data-ttu-id="eaf18-240">Pokud `x` má hodnotu `null`, `System.NullReferenceException` je vyvolána v době běhu.</span><span class="sxs-lookup"><span data-stu-id="eaf18-240">If `x` has the value `null`, a `System.NullReferenceException` is thrown at run-time.</span></span>

## <a name="pointers-in-expressions"></a><span data-ttu-id="eaf18-241">Ukazatele ve výrazech</span><span class="sxs-lookup"><span data-stu-id="eaf18-241">Pointers in expressions</span></span>

<span data-ttu-id="eaf18-242">V nezabezpečeném kontextu výraz může přinést výsledek typu ukazatele, ale mimo nezabezpečený kontext je chyba kompilace pro výraz typu ukazatele.</span><span class="sxs-lookup"><span data-stu-id="eaf18-242">In an unsafe context, an expression may yield a result of a pointer type, but outside an unsafe context it is a compile-time error for an expression to be of a pointer type.</span></span> <span data-ttu-id="eaf18-243">Přesně řečeno mimo nezabezpečený kontext kompilace dojde k chybě s případné *simple_name* ([jednoduché názvy](expressions.md#simple-names)), *member_access* ([přístup ke členu ](expressions.md#member-access)), *invocation_expression* ([výrazy typu Invocation](expressions.md#invocation-expressions)), nebo *element_access* ([přístup k prvkům](expressions.md#element-access)) je typ ukazatele.</span><span class="sxs-lookup"><span data-stu-id="eaf18-243">In precise terms, outside an unsafe context a compile-time error occurs if any *simple_name* ([Simple names](expressions.md#simple-names)), *member_access* ([Member access](expressions.md#member-access)), *invocation_expression* ([Invocation expressions](expressions.md#invocation-expressions)), or *element_access* ([Element access](expressions.md#element-access)) is of a pointer type.</span></span>

<span data-ttu-id="eaf18-244">V nezabezpečeném kontextu *primary_no_array_creation_expression* ([primární výrazy](expressions.md#primary-expressions)) a *unary_expression* ([unárních operátorů](expressions.md#unary-operators)) produkční povolit následující další konstrukcí:</span><span class="sxs-lookup"><span data-stu-id="eaf18-244">In an unsafe context, the *primary_no_array_creation_expression* ([Primary expressions](expressions.md#primary-expressions)) and *unary_expression* ([Unary operators](expressions.md#unary-operators)) productions permit the following additional constructs:</span></span>

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

<span data-ttu-id="eaf18-245">Tyto konstruktory jsou popsány v následujících částech.</span><span class="sxs-lookup"><span data-stu-id="eaf18-245">These constructs are described in the following sections.</span></span> <span data-ttu-id="eaf18-246">Priorita a asociativita operátorů nebezpečné odvozené od gramatiky.</span><span class="sxs-lookup"><span data-stu-id="eaf18-246">The precedence and associativity of the unsafe operators is implied by the grammar.</span></span>

### <a name="pointer-indirection"></a><span data-ttu-id="eaf18-247">Dereferenci ukazatele</span><span class="sxs-lookup"><span data-stu-id="eaf18-247">Pointer indirection</span></span>

<span data-ttu-id="eaf18-248">A *pointer_indirection_expression* se skládá z hvězdičku (`*`) následované *unary_expression*.</span><span class="sxs-lookup"><span data-stu-id="eaf18-248">A *pointer_indirection_expression* consists of an asterisk (`*`) followed by a *unary_expression*.</span></span>

```antlr
pointer_indirection_expression
    : '*' unary_expression
    ;
```

<span data-ttu-id="eaf18-249">Unární `*` operátor označuje dereferenci ukazatele a slouží k získání proměnné, na který ukazatel ukazuje.</span><span class="sxs-lookup"><span data-stu-id="eaf18-249">The unary `*` operator denotes pointer indirection and is used to obtain the variable to which a pointer points.</span></span> <span data-ttu-id="eaf18-250">Výsledek vyhodnocení výrazu `*P`, kde `P` je výraz typu ukazatele `T*`, je proměnná typu `T`.</span><span class="sxs-lookup"><span data-stu-id="eaf18-250">The result of evaluating `*P`, where `P` is an expression of a pointer type `T*`, is a variable of type `T`.</span></span> <span data-ttu-id="eaf18-251">Je chyba kompilace použít unární `*` operátoru ve výrazu typu `void*` nebo výraz, který není typu ukazatel.</span><span class="sxs-lookup"><span data-stu-id="eaf18-251">It is a compile-time error to apply the unary `*` operator to an expression of type `void*` or to an expression that isn't of a pointer type.</span></span>

<span data-ttu-id="eaf18-252">Účinek použití unární `*` operátor `null` ukazatel, je definováno implementací.</span><span class="sxs-lookup"><span data-stu-id="eaf18-252">The effect of applying the unary `*` operator to a `null` pointer is implementation-defined.</span></span> <span data-ttu-id="eaf18-253">Především neexistuje žádná záruka, že tato operace vyvolá `System.NullReferenceException`.</span><span class="sxs-lookup"><span data-stu-id="eaf18-253">In particular, there is no guarantee that this operation throws a `System.NullReferenceException`.</span></span>

<span data-ttu-id="eaf18-254">Pokud má přiřazenou neplatnou hodnotu na ukazatel, chování unární `*` operátor není definován.</span><span class="sxs-lookup"><span data-stu-id="eaf18-254">If an invalid value has been assigned to the pointer, the behavior of the unary `*` operator is undefined.</span></span> <span data-ttu-id="eaf18-255">Mezi neplatné hodnoty pro přístup přes ukazatel pomocí unární `*` operátor jsou adresy pro typ ukazuje, nesprávně zarovnána (viz příklad v [převody ukazatele](unsafe-code.md#pointer-conversions)) a adresu proměnné po konci svého životního cyklu.</span><span class="sxs-lookup"><span data-stu-id="eaf18-255">Among the invalid values for dereferencing a pointer by the unary `*` operator are an address inappropriately aligned for the type pointed to (see example in [Pointer conversions](unsafe-code.md#pointer-conversions)), and the address of a variable after the end of its lifetime.</span></span>

<span data-ttu-id="eaf18-256">Pro účely analýzy jednoznačného přiřazení produkovaný proměnnou vyhodnocení výrazu ve formátu `*P` se považuje za původně přiřazené ([původně přiřazený proměnné](variables.md#initially-assigned-variables)).</span><span class="sxs-lookup"><span data-stu-id="eaf18-256">For purposes of definite assignment analysis, a variable produced by evaluating an expression of the form `*P` is considered initially assigned ([Initially assigned variables](variables.md#initially-assigned-variables)).</span></span>

### <a name="pointer-member-access"></a><span data-ttu-id="eaf18-257">Přístupu k členovi</span><span class="sxs-lookup"><span data-stu-id="eaf18-257">Pointer member access</span></span>

<span data-ttu-id="eaf18-258">A *pointer_member_access* se skládá z *primary_expression*a po něm "`->`" token a po něm *identifikátor* a volitelné *type_argument_list*.</span><span class="sxs-lookup"><span data-stu-id="eaf18-258">A *pointer_member_access* consists of a *primary_expression*, followed by a "`->`" token, followed by an *identifier* and an optional *type_argument_list*.</span></span>

```antlr
pointer_member_access
    : primary_expression '->' identifier
    ;
```

<span data-ttu-id="eaf18-259">V přístupu ke členu ukazatel formuláře `P->I`, `P` musí být výraz ukazatele typu jiného než `void*`, a `I` musí označení přístupný člen typu, na který `P` body.</span><span class="sxs-lookup"><span data-stu-id="eaf18-259">In a pointer member access of the form `P->I`, `P` must be an expression of a pointer type other than `void*`, and `I` must denote an accessible member of the type to which `P` points.</span></span>

<span data-ttu-id="eaf18-260">Přístup ke členu ukazatel formuláře `P->I` se vyhodnotí přesně jako `(*P).I`.</span><span class="sxs-lookup"><span data-stu-id="eaf18-260">A pointer member access of the form `P->I` is evaluated exactly as `(*P).I`.</span></span> <span data-ttu-id="eaf18-261">Popis operátor dereference ukazatele (`*`), najdete v článku [dereferenci ukazatele](unsafe-code.md#pointer-indirection).</span><span class="sxs-lookup"><span data-stu-id="eaf18-261">For a description of the pointer indirection operator (`*`), see [Pointer indirection](unsafe-code.md#pointer-indirection).</span></span> <span data-ttu-id="eaf18-262">Popis operátor přístupu členů (`.`), najdete v článku [přístup ke členu](expressions.md#member-access).</span><span class="sxs-lookup"><span data-stu-id="eaf18-262">For a description of the member access operator (`.`), see [Member access](expressions.md#member-access).</span></span>

<span data-ttu-id="eaf18-263">V příkladu</span><span class="sxs-lookup"><span data-stu-id="eaf18-263">In the example</span></span>

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

<span data-ttu-id="eaf18-264">`->` operátor se používá pro přístup k polím a vyvolání metody struktury prostřednictvím ukazatele.</span><span class="sxs-lookup"><span data-stu-id="eaf18-264">the `->` operator is used to access fields and invoke a method of a struct through a pointer.</span></span> <span data-ttu-id="eaf18-265">Protože operace `P->I` přesně odpovídá `(*P).I`, `Main` metoda může stejně dobře být napsán takto:</span><span class="sxs-lookup"><span data-stu-id="eaf18-265">Because the operation `P->I` is precisely equivalent to `(*P).I`, the `Main` method could equally well have been written:</span></span>

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

### <a name="pointer-element-access"></a><span data-ttu-id="eaf18-266">Přístup k prvkům ukazatele</span><span class="sxs-lookup"><span data-stu-id="eaf18-266">Pointer element access</span></span>

<span data-ttu-id="eaf18-267">A *pointer_element_access* se skládá z *primary_no_array_creation_expression* následovaný výraz uzavřený do "`[`"a"`]`".</span><span class="sxs-lookup"><span data-stu-id="eaf18-267">A *pointer_element_access* consists of a *primary_no_array_creation_expression* followed by an expression enclosed in "`[`" and "`]`".</span></span>

```antlr
pointer_element_access
    : primary_no_array_creation_expression '[' expression ']'
    ;
```

<span data-ttu-id="eaf18-268">V přístup k prvkům ukazatel formuláře `P[E]`, `P` musí být výraz ukazatele typu jiného než `void*`, a `E` musí být výraz, který lze implicitně převést na `int`, `uint`, `long`, nebo `ulong`.</span><span class="sxs-lookup"><span data-stu-id="eaf18-268">In a pointer element access of the form `P[E]`, `P` must be an expression of a pointer type other than `void*`, and `E` must be an expression that can be implicitly converted to `int`, `uint`, `long`, or `ulong`.</span></span>

<span data-ttu-id="eaf18-269">Přístup k prvkům ukazatel formuláře `P[E]` se vyhodnotí přesně jako `*(P + E)`.</span><span class="sxs-lookup"><span data-stu-id="eaf18-269">A pointer element access of the form `P[E]` is evaluated exactly as `*(P + E)`.</span></span> <span data-ttu-id="eaf18-270">Popis operátor dereference ukazatele (`*`), najdete v článku [dereferenci ukazatele](unsafe-code.md#pointer-indirection).</span><span class="sxs-lookup"><span data-stu-id="eaf18-270">For a description of the pointer indirection operator (`*`), see [Pointer indirection](unsafe-code.md#pointer-indirection).</span></span> <span data-ttu-id="eaf18-271">Popis ukazatele operátor sčítání (`+`), najdete v článku [aritmetické operace ukazatele](unsafe-code.md#pointer-arithmetic).</span><span class="sxs-lookup"><span data-stu-id="eaf18-271">For a description of the pointer addition operator (`+`), see [Pointer arithmetic](unsafe-code.md#pointer-arithmetic).</span></span>

<span data-ttu-id="eaf18-272">V příkladu</span><span class="sxs-lookup"><span data-stu-id="eaf18-272">In the example</span></span>

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

<span data-ttu-id="eaf18-273">přístup k prvkům ukazatel slouží k inicializaci vyrovnávací paměti pro znaky v `for` smyčky.</span><span class="sxs-lookup"><span data-stu-id="eaf18-273">a pointer element access is used to initialize the character buffer in a `for` loop.</span></span> <span data-ttu-id="eaf18-274">Protože operace `P[E]` přesně odpovídá `*(P + E)`, v příkladu může stejně dobře být napsán takto:</span><span class="sxs-lookup"><span data-stu-id="eaf18-274">Because the operation `P[E]` is precisely equivalent to `*(P + E)`, the example could equally well have been written:</span></span>

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

<span data-ttu-id="eaf18-275">Operátor přístupu k elementu ukazatel nekontroluje celočíselných chyby a chování při přístupu k celočíselných element není definován.</span><span class="sxs-lookup"><span data-stu-id="eaf18-275">The pointer element access operator does not check for out-of-bounds errors and the behavior when accessing an out-of-bounds element is undefined.</span></span> <span data-ttu-id="eaf18-276">To je stejný jako C a C++.</span><span class="sxs-lookup"><span data-stu-id="eaf18-276">This is the same as C and C++.</span></span>

### <a name="the-address-of-operator"></a><span data-ttu-id="eaf18-277">Operátor address-of</span><span class="sxs-lookup"><span data-stu-id="eaf18-277">The address-of operator</span></span>

<span data-ttu-id="eaf18-278">*Addressof_expression* se skládá z znak ampersand (`&`) následované *unary_expression*.</span><span class="sxs-lookup"><span data-stu-id="eaf18-278">An *addressof_expression* consists of an ampersand (`&`) followed by a *unary_expression*.</span></span>

```antlr
addressof_expression
    : '&' unary_expression
    ;
```

<span data-ttu-id="eaf18-279">Zadaný výraz `E` je typu `T` a je klasifikován jako pevná proměnná ([Fixed a přesunutelný proměnné](unsafe-code.md#fixed-and-moveable-variables)), konstrukce `&E` vypočítá adresy proměnné Dal `E`.</span><span class="sxs-lookup"><span data-stu-id="eaf18-279">Given an expression `E` which is of a type `T` and is classified as a fixed variable ([Fixed and moveable variables](unsafe-code.md#fixed-and-moveable-variables)), the construct `&E` computes the address of the variable given by `E`.</span></span> <span data-ttu-id="eaf18-280">Typ výsledku je `T*` a je klasifikován jako hodnotu.</span><span class="sxs-lookup"><span data-stu-id="eaf18-280">The type of the result is `T*` and is classified as a value.</span></span> <span data-ttu-id="eaf18-281">Pokud dojde k chybě kompilace `E` není jsou klasifikovány jako proměnnou, pokud `E` je klasifikován jako místní proměnná jen pro čtení, nebo pokud `E` označuje přesunutelný proměnné.</span><span class="sxs-lookup"><span data-stu-id="eaf18-281">A compile-time error occurs if `E` is not classified as a variable, if `E` is classified as a read-only local variable, or if `E` denotes a moveable variable.</span></span> <span data-ttu-id="eaf18-282">V posledním případě fixed – příkaz ([příkazu fixed](unsafe-code.md#the-fixed-statement)) lze dočasně "Opravit" Proměnná před získáním adresy.</span><span class="sxs-lookup"><span data-stu-id="eaf18-282">In the last case, a fixed statement ([The fixed statement](unsafe-code.md#the-fixed-statement)) can be used to temporarily "fix" the variable before obtaining its address.</span></span> <span data-ttu-id="eaf18-283">Jak je uvedeno v [přístup ke členu](expressions.md#member-access), vně konstruktor instance nebo statický konstruktor pro struktury nebo třídy, která definuje, aplikace `readonly` pole, toto pole je považován za hodnotu, není proměnná.</span><span class="sxs-lookup"><span data-stu-id="eaf18-283">As stated in [Member access](expressions.md#member-access), outside an instance constructor or static constructor for a struct or class that defines a `readonly` field, that field is considered a value, not a variable.</span></span> <span data-ttu-id="eaf18-284">V důsledku toho nelze vytvářet jeho adresu.</span><span class="sxs-lookup"><span data-stu-id="eaf18-284">As such, its address cannot be taken.</span></span> <span data-ttu-id="eaf18-285">Podobně nelze vytvářet adresu konstanty.</span><span class="sxs-lookup"><span data-stu-id="eaf18-285">Similarly, the address of a constant cannot be taken.</span></span>

<span data-ttu-id="eaf18-286">`&` Operátor nevyžaduje, aby její argument představoval jednoznačně přiřazené, ale následující `&` operace, proměnné, ke které se použije operátor, který je považován za jednoznačně přiřazené v cestě provádění dojde k operaci.</span><span class="sxs-lookup"><span data-stu-id="eaf18-286">The `&` operator does not require its argument to be definitely assigned, but following an `&` operation, the variable to which the operator is applied is considered definitely assigned in the execution path in which the operation occurs.</span></span> <span data-ttu-id="eaf18-287">Je zodpovědností programátorovi, aby se ujistěte, že správné inicializace proměnné ve skutečnosti proběhnout v této situaci.</span><span class="sxs-lookup"><span data-stu-id="eaf18-287">It is the responsibility of the programmer to ensure that correct initialization of the variable actually does take place in this situation.</span></span>

<span data-ttu-id="eaf18-288">V příkladu</span><span class="sxs-lookup"><span data-stu-id="eaf18-288">In the example</span></span>

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

<span data-ttu-id="eaf18-289">`i` je považován za jednoznačně přiřazené následující `&i` operace, která slouží k inicializaci `p`.</span><span class="sxs-lookup"><span data-stu-id="eaf18-289">`i` is considered definitely assigned following the `&i` operation used to initialize `p`.</span></span> <span data-ttu-id="eaf18-290">Přiřazení `*p` platit inicializuje `i`, ale zahrnutí tato inicializace zodpovídá programátor a žádná chyba kompilace ke kterým by byl odebrán přiřazení.</span><span class="sxs-lookup"><span data-stu-id="eaf18-290">The assignment to `*p` in effect initializes `i`, but the inclusion of this initialization is the responsibility of the programmer, and no compile-time error would occur if the assignment was removed.</span></span>

<span data-ttu-id="eaf18-291">Pravidla pro jednoznačného přiřazení `&` operátor existovat tak, že se můžete vyhnout redundantní inicializace místních proměnných.</span><span class="sxs-lookup"><span data-stu-id="eaf18-291">The rules of definite assignment for the `&` operator exist such that redundant initialization of local variables can be avoided.</span></span> <span data-ttu-id="eaf18-292">Například mnoho externí rozhraní API využít ukazatel na strukturu, která je vyplněna pomocí rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="eaf18-292">For example, many external APIs take a pointer to a structure which is filled in by the API.</span></span> <span data-ttu-id="eaf18-293">Volání těchto rozhraní API, typicky pass adresy proměnné místní struktury a bez pravidla, by bylo zapotřebí redundantní inicializaci proměnné struktury.</span><span class="sxs-lookup"><span data-stu-id="eaf18-293">Calls to such APIs typically pass the address of a local struct variable, and without the rule, redundant initialization of the struct variable would be required.</span></span>

### <a name="pointer-increment-and-decrement"></a><span data-ttu-id="eaf18-294">Ukazatel přírůstek a snížení</span><span class="sxs-lookup"><span data-stu-id="eaf18-294">Pointer increment and decrement</span></span>

<span data-ttu-id="eaf18-295">V nezabezpečeném kontextu `++` a `--` operátory ([Příponové operátory Inkrementace a dekrementace operátory](expressions.md#postfix-increment-and-decrement-operators) a [předpony Inkrementace a dekrementace operátory](expressions.md#prefix-increment-and-decrement-operators)) lze použít na ukazatel proměnné typů všechny s výjimkou `void*`.</span><span class="sxs-lookup"><span data-stu-id="eaf18-295">In an unsafe context, the `++` and `--` operators ([Postfix increment and decrement operators](expressions.md#postfix-increment-and-decrement-operators) and [Prefix increment and decrement operators](expressions.md#prefix-increment-and-decrement-operators)) can be applied to pointer variables of all types except `void*`.</span></span> <span data-ttu-id="eaf18-296">Díky tomu se pro každý typ ukazatele `T*`, implicitně jsou definovány následující operátory:</span><span class="sxs-lookup"><span data-stu-id="eaf18-296">Thus, for every pointer type `T*`, the following operators are implicitly defined:</span></span>

```csharp
T* operator ++(T* x);
T* operator --(T* x);
```

<span data-ttu-id="eaf18-297">Operátory produkuje stejné výsledky jako `x + 1` a `x - 1`v uvedeném pořadí ([aritmetické operace ukazatele](unsafe-code.md#pointer-arithmetic)).</span><span class="sxs-lookup"><span data-stu-id="eaf18-297">The operators produce the same results as `x + 1` and `x - 1`, respectively ([Pointer arithmetic](unsafe-code.md#pointer-arithmetic)).</span></span> <span data-ttu-id="eaf18-298">Jinými slovy, pro proměnné ukazatele typu `T*`, `++` přidá operátor `sizeof(T)` na adrese obsažené v proměnné a `--` operátor odečte `sizeof(T)` v adrese obsažené v proměnné.</span><span class="sxs-lookup"><span data-stu-id="eaf18-298">In other words, for a pointer variable of type `T*`, the `++` operator adds `sizeof(T)` to the address contained in the variable, and the `--` operator subtracts `sizeof(T)` from the address contained in the variable.</span></span>

<span data-ttu-id="eaf18-299">Pokud ukazatel zvýšení nebo snížení došlo k přetečení domény typ ukazatele, výsledkem je definováno implementací, ale se budou vytvářet žádné výjimky.</span><span class="sxs-lookup"><span data-stu-id="eaf18-299">If a pointer increment or decrement operation overflows the domain of the pointer type, the result is implementation-defined, but no exceptions are produced.</span></span>

### <a name="pointer-arithmetic"></a><span data-ttu-id="eaf18-300">Aritmetika ukazatele</span><span class="sxs-lookup"><span data-stu-id="eaf18-300">Pointer arithmetic</span></span>

<span data-ttu-id="eaf18-301">V nezabezpečeném kontextu `+` a `-` operátory ([operátor sčítání](expressions.md#addition-operator) a [operátor odčítání](expressions.md#subtraction-operator)) lze použít u hodnot všechny typy ukazatelů, s výjimkou `void*`.</span><span class="sxs-lookup"><span data-stu-id="eaf18-301">In an unsafe context, the `+` and `-` operators ([Addition operator](expressions.md#addition-operator) and [Subtraction operator](expressions.md#subtraction-operator)) can be applied to values of all pointer types except `void*`.</span></span> <span data-ttu-id="eaf18-302">Díky tomu se pro každý typ ukazatele `T*`, implicitně jsou definovány následující operátory:</span><span class="sxs-lookup"><span data-stu-id="eaf18-302">Thus, for every pointer type `T*`, the following operators are implicitly defined:</span></span>

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

<span data-ttu-id="eaf18-303">Zadaný výraz `P` typu ukazatele `T*` a výraz `N` typu `int`, `uint`, `long`, nebo `ulong`, výrazy `P + N` a `N + P` compute Hodnota ukazatele typu `T*` , která je výsledkem sečtení `N * sizeof(T)` adresu uvedenou ve `P`.</span><span class="sxs-lookup"><span data-stu-id="eaf18-303">Given an expression `P` of a pointer type `T*` and an expression `N` of type `int`, `uint`, `long`, or `ulong`, the expressions `P + N` and `N + P` compute the pointer value of type `T*` that results from adding `N * sizeof(T)` to the address given by `P`.</span></span> <span data-ttu-id="eaf18-304">Obdobně výraz `P - N` vypočítá hodnotu ukazatele typu `T*` , že výsledky daných `N * sizeof(T)` z adresy poskytnuté `P`.</span><span class="sxs-lookup"><span data-stu-id="eaf18-304">Likewise, the expression `P - N` computes the pointer value of type `T*` that results from subtracting `N * sizeof(T)` from the address given by `P`.</span></span>

<span data-ttu-id="eaf18-305">Zadaný dvou výrazů `P` a `Q`, typu ukazatele `T*`, výraz `P - Q` vypočítá rozdíl mezi adresami zadanými `P` a `Q` a tento rozdíl vydělí `sizeof(T)`.</span><span class="sxs-lookup"><span data-stu-id="eaf18-305">Given two expressions, `P` and `Q`, of a pointer type `T*`, the expression `P - Q` computes the difference between the addresses given by `P` and `Q` and then divides that difference by `sizeof(T)`.</span></span> <span data-ttu-id="eaf18-306">Typ výsledku je vždy `long`.</span><span class="sxs-lookup"><span data-stu-id="eaf18-306">The type of the result is always `long`.</span></span> <span data-ttu-id="eaf18-307">V důsledku toho `P - Q` je vypočítán jako `((long)(P) - (long)(Q)) / sizeof(T)`.</span><span class="sxs-lookup"><span data-stu-id="eaf18-307">In effect, `P - Q` is computed as `((long)(P) - (long)(Q)) / sizeof(T)`.</span></span>

<span data-ttu-id="eaf18-308">Příklad:</span><span class="sxs-lookup"><span data-stu-id="eaf18-308">For example:</span></span>

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

<span data-ttu-id="eaf18-309">které vytvoří výstup:</span><span class="sxs-lookup"><span data-stu-id="eaf18-309">which produces the output:</span></span>

```
p - q = -14
q - p = 14
```

<span data-ttu-id="eaf18-310">Pokud Přetečení aritmetické operace ukazatele domény typ ukazatele, výsledek je oříznutá. podporuje definovanou implementací, ale se budou vytvářet žádné výjimky.</span><span class="sxs-lookup"><span data-stu-id="eaf18-310">If a pointer arithmetic operation overflows the domain of the pointer type, the result is truncated in an implementation-defined fashion, but no exceptions are produced.</span></span>

### <a name="pointer-comparison"></a><span data-ttu-id="eaf18-311">Porovnání ukazatelů</span><span class="sxs-lookup"><span data-stu-id="eaf18-311">Pointer comparison</span></span>

<span data-ttu-id="eaf18-312">V nezabezpečeném kontextu `==`, `!=`, `<`, `>`, `<=`, a `=>` operátory ([relační a typové zkoušky operátory](expressions.md#relational-and-type-testing-operators)) lze použít u hodnot všech typy ukazatelů.</span><span class="sxs-lookup"><span data-stu-id="eaf18-312">In an unsafe context, the `==`, `!=`, `<`, `>`, `<=`, and `=>` operators ([Relational and type-testing operators](expressions.md#relational-and-type-testing-operators)) can be applied to values of all pointer types.</span></span> <span data-ttu-id="eaf18-313">Operátory porovnání ukazatele jsou:</span><span class="sxs-lookup"><span data-stu-id="eaf18-313">The pointer comparison operators are:</span></span>

```csharp
bool operator ==(void* x, void* y);
bool operator !=(void* x, void* y);
bool operator <(void* x, void* y);
bool operator >(void* x, void* y);
bool operator <=(void* x, void* y);
bool operator >=(void* x, void* y);
```

<span data-ttu-id="eaf18-314">Vzhledem k tomu, že existuje implicitní převod z libovolného typu ukazatel na `void*` typ operandy libovolný typ ukazatele lze porovnat pomocí těchto operátorů.</span><span class="sxs-lookup"><span data-stu-id="eaf18-314">Because an implicit conversion exists from any pointer type to the `void*` type, operands of any pointer type can be compared using these operators.</span></span> <span data-ttu-id="eaf18-315">Relační operátory porovnávají adresy poskytnuté dva operandy, jako by byly celých čísel bez znaménka.</span><span class="sxs-lookup"><span data-stu-id="eaf18-315">The comparison operators compare the addresses given by the two operands as if they were unsigned integers.</span></span>

### <a name="the-sizeof-operator"></a><span data-ttu-id="eaf18-316">Sizeof – operátor</span><span class="sxs-lookup"><span data-stu-id="eaf18-316">The sizeof operator</span></span>

<span data-ttu-id="eaf18-317">`sizeof` Operátor vrátí počet bajtů obsazena proměnnou daného typu.</span><span class="sxs-lookup"><span data-stu-id="eaf18-317">The `sizeof` operator returns the number of bytes occupied by a variable of a given type.</span></span> <span data-ttu-id="eaf18-318">Typ určený jako operand `sizeof` musí být *unmanaged_type* ([typy ukazatelů](unsafe-code.md#pointer-types)).</span><span class="sxs-lookup"><span data-stu-id="eaf18-318">The type specified as an operand to `sizeof` must be an *unmanaged_type* ([Pointer types](unsafe-code.md#pointer-types)).</span></span>

```antlr
sizeof_expression
    : 'sizeof' '(' unmanaged_type ')'
    ;
```

<span data-ttu-id="eaf18-319">Výsledkem `sizeof` operátor je hodnota typu `int`.</span><span class="sxs-lookup"><span data-stu-id="eaf18-319">The result of the `sizeof` operator is a value of type `int`.</span></span> <span data-ttu-id="eaf18-320">Pro některé předdefinované typy, `sizeof` operátor vrací konstantní hodnoty, jak je znázorněno v následující tabulce.</span><span class="sxs-lookup"><span data-stu-id="eaf18-320">For certain predefined types, the `sizeof` operator yields a constant value as shown in the table below.</span></span>


| <span data-ttu-id="eaf18-321">__Výraz__</span><span class="sxs-lookup"><span data-stu-id="eaf18-321">__Expression__</span></span>   | <span data-ttu-id="eaf18-322">__výsledek__</span><span class="sxs-lookup"><span data-stu-id="eaf18-322">__Result__</span></span> |
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

<span data-ttu-id="eaf18-323">Pro všechny ostatní typy, výsledek `sizeof` operátor je definováno implementací a je klasifikován jako hodnotu, není konstanta.</span><span class="sxs-lookup"><span data-stu-id="eaf18-323">For all other types, the result of the `sizeof` operator is implementation-defined and is classified as a value, not a constant.</span></span>

<span data-ttu-id="eaf18-324">Pořadí, ve kterém jsou členy zkomprimována do struktury není zadána.</span><span class="sxs-lookup"><span data-stu-id="eaf18-324">The order in which members are packed into a struct is unspecified.</span></span>

<span data-ttu-id="eaf18-325">Pro účely zarovnání může nepojmenované odsazení na začátek struktury, v rámci struktury a na konci struktury.</span><span class="sxs-lookup"><span data-stu-id="eaf18-325">For alignment purposes, there may be unnamed padding at the beginning of a struct, within a struct, and at the end of the struct.</span></span> <span data-ttu-id="eaf18-326">Obsah bity jako odsazení jsou neurčité.</span><span class="sxs-lookup"><span data-stu-id="eaf18-326">The contents of the bits used as padding are indeterminate.</span></span>

<span data-ttu-id="eaf18-327">Při použití na operand má typ struktura, výsledek je celkový počet bajtů v proměnnou daného typu, včetně žádné odsazení.</span><span class="sxs-lookup"><span data-stu-id="eaf18-327">When applied to an operand that has struct type, the result is the total number of bytes in a variable of that type, including any padding.</span></span>

## <a name="the-fixed-statement"></a><span data-ttu-id="eaf18-328">Fixed – příkaz</span><span class="sxs-lookup"><span data-stu-id="eaf18-328">The fixed statement</span></span>

<span data-ttu-id="eaf18-329">V nezabezpečeném kontextu *embedded_statement* ([příkazy](statements.md)) produkční povoluje Další konstrukce, `fixed` příkaz, který se používá na "Opravit" přesunutelný proměnnou tak, aby jeho po dobu trvání příkaz konstantní adresu.</span><span class="sxs-lookup"><span data-stu-id="eaf18-329">In an unsafe context, the *embedded_statement* ([Statements](statements.md)) production permits an additional construct, the `fixed` statement, which is used to "fix" a moveable variable such that its address remains constant for the duration of the statement.</span></span>

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

<span data-ttu-id="eaf18-330">Každý *fixed_pointer_declarator* deklaruje místní proměnnou daný *pointer_type* a inicializuje tuto místní proměnnou s adresou počítají tak, že odpovídající *fixed_ pointer_initializer*.</span><span class="sxs-lookup"><span data-stu-id="eaf18-330">Each *fixed_pointer_declarator* declares a local variable of the given *pointer_type* and initializes that local variable with the address computed by the corresponding *fixed_pointer_initializer*.</span></span> <span data-ttu-id="eaf18-331">Místní proměnná deklarovaná ve `fixed` příkaz je dostupný v libovolném *fixed_pointer_initializer*s, ke kterým dochází k pravému deklarace danou proměnnou a v *embedded_statement* z `fixed` příkazu.</span><span class="sxs-lookup"><span data-stu-id="eaf18-331">A local variable declared in a `fixed` statement is accessible in any *fixed_pointer_initializer*s occurring to the right of that variable's declaration, and in the *embedded_statement* of the `fixed` statement.</span></span> <span data-ttu-id="eaf18-332">Lokální proměnná deklarovaná příkazem `fixed` příkaz je považován za jen pro čtení.</span><span class="sxs-lookup"><span data-stu-id="eaf18-332">A local variable declared by a `fixed` statement is considered read-only.</span></span> <span data-ttu-id="eaf18-333">Chyba kompilace v případě vloženým příkazem se pokusí upravit tuto místní proměnnou (prostřednictvím přiřazení nebo `++` a `--` operátory) nebo předat ji jako `ref` nebo `out` parametru.</span><span class="sxs-lookup"><span data-stu-id="eaf18-333">A compile-time error occurs if the embedded statement attempts to modify this local variable (via assignment or the `++` and `--` operators) or pass it as a `ref` or `out` parameter.</span></span>

<span data-ttu-id="eaf18-334">A *fixed_pointer_initializer* může být jedna z následujících akcí:</span><span class="sxs-lookup"><span data-stu-id="eaf18-334">A *fixed_pointer_initializer* can be one of the following:</span></span>

*  <span data-ttu-id="eaf18-335">Token "`&`" následované *variable_reference* ([přesné pravidla pro určování jednoznačného přiřazení](variables.md#precise-rules-for-determining-definite-assignment)) přesunutelný proměnné ([Fixed a přesunutelný proměnné](unsafe-code.md#fixed-and-moveable-variables)) nespravovaný typ `T`, zadaný typ `T*` implicitně převést na typ ukazatele, který je uveden v `fixed` příkazu.</span><span class="sxs-lookup"><span data-stu-id="eaf18-335">The token "`&`" followed by a *variable_reference* ([Precise rules for determining definite assignment](variables.md#precise-rules-for-determining-definite-assignment)) to a moveable variable ([Fixed and moveable variables](unsafe-code.md#fixed-and-moveable-variables)) of an unmanaged type `T`, provided the type `T*` is implicitly convertible to the pointer type given in the `fixed` statement.</span></span> <span data-ttu-id="eaf18-336">V takovém případě inicializátoru vypočítá adresy dané proměnné a proměnná je zaručeno, že zůstanou na pevnou adresu po dobu trvání `fixed` příkazu.</span><span class="sxs-lookup"><span data-stu-id="eaf18-336">In this case, the initializer computes the address of the given variable, and the variable is guaranteed to remain at a fixed address for the duration of the `fixed` statement.</span></span>
*  <span data-ttu-id="eaf18-337">Výraz *array_type* elementy nespravovaným typem `T`, zadaný typ `T*` implicitně převést na typ ukazatele, který je uveden v `fixed` příkazu.</span><span class="sxs-lookup"><span data-stu-id="eaf18-337">An expression of an *array_type* with elements of an unmanaged type `T`, provided the type `T*` is implicitly convertible to the pointer type given in the `fixed` statement.</span></span> <span data-ttu-id="eaf18-338">V takovém případě inicializátoru vypočítá adresu první prvek v poli, a je zaručeno, že celého pole zůstanou na pevnou adresu po dobu trvání `fixed` příkazu.</span><span class="sxs-lookup"><span data-stu-id="eaf18-338">In this case, the initializer computes the address of the first element in the array, and the entire array is guaranteed to remain at a fixed address for the duration of the `fixed` statement.</span></span> <span data-ttu-id="eaf18-339">Pokud výraz pole má hodnotu null nebo pole nemá nulovým počtem elementů, vypočítá inicializátoru adresu roven nule.</span><span class="sxs-lookup"><span data-stu-id="eaf18-339">If the array expression is null or if the array has zero elements, the initializer computes an address equal to zero.</span></span>
*  <span data-ttu-id="eaf18-340">Výraz typu `string`, zadaný typ `char*` implicitně převést na typ ukazatele, který je uveden v `fixed` příkazu.</span><span class="sxs-lookup"><span data-stu-id="eaf18-340">An expression of type `string`, provided the type `char*` is implicitly convertible to the pointer type given in the `fixed` statement.</span></span> <span data-ttu-id="eaf18-341">V takovém případě inicializátoru vypočítá adresu prvního znaku v řetězci, a celý řetězec je zaručeno, že zůstanou na pevnou adresu po dobu trvání `fixed` příkazu.</span><span class="sxs-lookup"><span data-stu-id="eaf18-341">In this case, the initializer computes the address of the first character in the string, and the entire string is guaranteed to remain at a fixed address for the duration of the `fixed` statement.</span></span> <span data-ttu-id="eaf18-342">Chování `fixed` příkaz je definováno implementací pokud řetězcový výraz má hodnotu null.</span><span class="sxs-lookup"><span data-stu-id="eaf18-342">The behavior of the `fixed` statement is implementation-defined if the string expression is null.</span></span>
*  <span data-ttu-id="eaf18-343">A *simple_name* nebo *member_access* , která odkazuje na člen vyrovnávací paměti pevné velikosti přesunutelný proměnná, zadaný typ člena vyrovnávací paměti pevné velikosti je implicitně převést na zadaný typ ukazatele v `fixed` příkazu.</span><span class="sxs-lookup"><span data-stu-id="eaf18-343">A *simple_name* or *member_access* that references a fixed size buffer member of a moveable variable, provided the type of the fixed size buffer member is implicitly convertible to the pointer type given in the `fixed` statement.</span></span> <span data-ttu-id="eaf18-344">V takovém případě inicializátoru vypočítá ukazatel na první prvek vyrovnávací paměti pevné velikosti ([ve výrazech pevnou velikost vyrovnávací paměti](unsafe-code.md#fixed-size-buffers-in-expressions)), a vyrovnávací paměti pevné velikosti je zaručeno, že zůstanou na pevnou adresu po dobu trvání `fixed`příkazu.</span><span class="sxs-lookup"><span data-stu-id="eaf18-344">In this case, the initializer computes a pointer to the first element of the fixed size buffer ([Fixed size buffers in expressions](unsafe-code.md#fixed-size-buffers-in-expressions)), and the fixed size buffer is guaranteed to remain at a fixed address for the duration of the `fixed` statement.</span></span>

<span data-ttu-id="eaf18-345">Pro každou adresu počítají tak, že *fixed_pointer_initializer* `fixed` prohlášení zajišťuje, že proměnná odkazuje na adresu není v souladu s přemístění nebo vyřazení pomocí systému uvolňování paměti po dobu trvání `fixed` příkazu.</span><span class="sxs-lookup"><span data-stu-id="eaf18-345">For each address computed by a *fixed_pointer_initializer* the `fixed` statement ensures that the variable referenced by the address is not subject to relocation or disposal by the garbage collector for the duration of the `fixed` statement.</span></span> <span data-ttu-id="eaf18-346">Například, pokud adresa počítají tak, že *fixed_pointer_initializer* odkazuje na pole objektu, nebo element pole instance `fixed` příkaz zaručuje, že není přemístění obsahující instanci objektu nebo odstraněny po celou dobu životnosti příkazu.</span><span class="sxs-lookup"><span data-stu-id="eaf18-346">For example, if the address computed by a *fixed_pointer_initializer* references a field of an object or an element of an array instance, the `fixed` statement guarantees that the containing object instance is not relocated or disposed of during the lifetime of the statement.</span></span>

<span data-ttu-id="eaf18-347">Je programátorovi povinností ujistit se, že ukazatele vytvořil `fixed` příkazy nepřežije nad rámec spuštění těchto příkazů.</span><span class="sxs-lookup"><span data-stu-id="eaf18-347">It is the programmer's responsibility to ensure that pointers created by `fixed` statements do not survive beyond execution of those statements.</span></span> <span data-ttu-id="eaf18-348">Například když ukazatele vytvořil `fixed` příkazy jsou předány do rozhraní API pro externí, zodpovídá programátor Ujistěte se, že rozhraní API pro zachování paměti těchto ukazatelů.</span><span class="sxs-lookup"><span data-stu-id="eaf18-348">For example, when pointers created by `fixed` statements are passed to external APIs, it is the programmer's responsibility to ensure that the APIs retain no memory of these pointers.</span></span>

<span data-ttu-id="eaf18-349">Oprava objekty může způsobit fragmentace haldy (protože nelze jej přesunout).</span><span class="sxs-lookup"><span data-stu-id="eaf18-349">Fixed objects may cause fragmentation of the heap (because they can't be moved).</span></span> <span data-ttu-id="eaf18-350">Z tohoto důvodu byste opravit objekty pouze v případě, že je nezbytně nutné a pak jenom za nejkratší dobu nejvíce času.</span><span class="sxs-lookup"><span data-stu-id="eaf18-350">For that reason, objects should be fixed only when absolutely necessary and then only for the shortest amount of time possible.</span></span>

<span data-ttu-id="eaf18-351">V příkladu</span><span class="sxs-lookup"><span data-stu-id="eaf18-351">The example</span></span>

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

<span data-ttu-id="eaf18-352">ukazuje použití několika `fixed` příkazu.</span><span class="sxs-lookup"><span data-stu-id="eaf18-352">demonstrates several uses of the `fixed` statement.</span></span> <span data-ttu-id="eaf18-353">První příkaz oprav a získá adresu statické pole, druhý příkaz opravy a získá adresu pole instance a třetí příkaz oprav a získá adresu k elementu pole.</span><span class="sxs-lookup"><span data-stu-id="eaf18-353">The first statement fixes and obtains the address of a static field, the second statement fixes and obtains the address of an instance field, and the third statement fixes and obtains the address of an array element.</span></span> <span data-ttu-id="eaf18-354">V každém případě by byl chybně použit standardní `&` operátor vzhledem k tomu, že proměnné jsou klasifikovány jako přesunutelný proměnné.</span><span class="sxs-lookup"><span data-stu-id="eaf18-354">In each case it would have been an error to use the regular `&` operator since the variables are all classified as moveable variables.</span></span>

<span data-ttu-id="eaf18-355">Čtvrtý `fixed` podobného výsledku na třetí vytvoří příkaz v předchozím příkladu.</span><span class="sxs-lookup"><span data-stu-id="eaf18-355">The fourth `fixed` statement in the example above produces a similar result to the third.</span></span>

<span data-ttu-id="eaf18-356">Tento příklad `fixed` používá příkaz `string`:</span><span class="sxs-lookup"><span data-stu-id="eaf18-356">This example of the `fixed` statement uses `string`:</span></span>

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

<span data-ttu-id="eaf18-357">V nezabezpečeném kontextu pole prvků jednorozměrná pole jsou uloženy ve vzestupném pořadí index, počínaje indexem `0` a konče index `Length - 1`.</span><span class="sxs-lookup"><span data-stu-id="eaf18-357">In an unsafe context array elements of single-dimensional arrays are stored in increasing index order, starting with index `0` and ending with index `Length - 1`.</span></span> <span data-ttu-id="eaf18-358">Pro vícerozměrná pole, pole, které prvky jsou uloženy tak, aby se nejdřív zvyšují indexy rozměr nejvíce vpravo pak dimenze, na další a tak dále ponecháno na levé straně.</span><span class="sxs-lookup"><span data-stu-id="eaf18-358">For multi-dimensional arrays, array elements are stored such that the indices of the rightmost dimension are increased first, then the next left dimension, and so on to the left.</span></span> <span data-ttu-id="eaf18-359">V rámci `fixed` příkaz, který získá ukazatel `p` do pole instance `a`, od hodnoty ukazatele `p` k `p + a.Length - 1` představují adresy prvků v poli.</span><span class="sxs-lookup"><span data-stu-id="eaf18-359">Within a `fixed` statement that obtains a pointer `p` to an array instance `a`, the pointer values ranging from `p` to `p + a.Length - 1` represent addresses of the elements in the array.</span></span> <span data-ttu-id="eaf18-360">Obdobně proměnné od `p[0]` k `p[a.Length - 1]` zastupují elementy skutečné pole.</span><span class="sxs-lookup"><span data-stu-id="eaf18-360">Likewise, the variables ranging from `p[0]` to `p[a.Length - 1]` represent the actual array elements.</span></span> <span data-ttu-id="eaf18-361">Zadaný způsob, ve kterém jsou uložené pole, jako by šlo lineární jsme lze považovat všechny dimenze pole.</span><span class="sxs-lookup"><span data-stu-id="eaf18-361">Given the way in which arrays are stored, we can treat an array of any dimension as though it were linear.</span></span>

<span data-ttu-id="eaf18-362">Příklad:</span><span class="sxs-lookup"><span data-stu-id="eaf18-362">For example:</span></span>

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

<span data-ttu-id="eaf18-363">které vytvoří výstup:</span><span class="sxs-lookup"><span data-stu-id="eaf18-363">which produces the output:</span></span>

```
[0,0,0] =  0 [0,0,1] =  1 [0,0,2] =  2 [0,0,3] =  3
[0,1,0] =  4 [0,1,1] =  5 [0,1,2] =  6 [0,1,3] =  7
[0,2,0] =  8 [0,2,1] =  9 [0,2,2] = 10 [0,2,3] = 11
[1,0,0] = 12 [1,0,1] = 13 [1,0,2] = 14 [1,0,3] = 15
[1,1,0] = 16 [1,1,1] = 17 [1,1,2] = 18 [1,1,3] = 19
[1,2,0] = 20 [1,2,1] = 21 [1,2,2] = 22 [1,2,3] = 23
```

<span data-ttu-id="eaf18-364">V příkladu</span><span class="sxs-lookup"><span data-stu-id="eaf18-364">In the example</span></span>

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

<span data-ttu-id="eaf18-365">`fixed` prohlášení se používá k opravit pole, aby jeho adresy může být předán metodu, která přijímá ukazatel.</span><span class="sxs-lookup"><span data-stu-id="eaf18-365">a `fixed` statement is used to fix an array so its address can be passed to a method that takes a pointer.</span></span>

<span data-ttu-id="eaf18-366">V tomto příkladu:</span><span class="sxs-lookup"><span data-stu-id="eaf18-366">In the example:</span></span>

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

<span data-ttu-id="eaf18-367">fixed – příkaz se používá k vyřešení vyrovnávací paměti pevné velikosti struktury tak jeho adresy může sloužit jako ukazatel.</span><span class="sxs-lookup"><span data-stu-id="eaf18-367">a fixed statement is used to fix a fixed size buffer of a struct so its address can be used as a pointer.</span></span>

<span data-ttu-id="eaf18-368">A `char*` hodnotu vytvořený po opravě instanci řetězce, vždy odkazuje na řetězec zakončený hodnotou null.</span><span class="sxs-lookup"><span data-stu-id="eaf18-368">A `char*` value produced by fixing a string instance always points to a null-terminated string.</span></span> <span data-ttu-id="eaf18-369">V rámci příkazu fixed, který získá ukazatel `p` instanci řetězec `s`, od hodnoty ukazatele `p` k `p + s.Length - 1` představují adresy znaků v řetězci a hodnota ukazatele `p + s.Length` vždy odkazuje na znak null (znak s hodnotou `'\0'`).</span><span class="sxs-lookup"><span data-stu-id="eaf18-369">Within a fixed statement that obtains a pointer `p` to a string instance `s`, the pointer values ranging from `p` to `p + s.Length - 1` represent addresses of the characters in the string, and the pointer value `p + s.Length` always points to a null character (the character with value `'\0'`).</span></span>

<span data-ttu-id="eaf18-370">Změny objektů typu spravované prostřednictvím pevné odkazy můžete za následek nedefinované chování.</span><span class="sxs-lookup"><span data-stu-id="eaf18-370">Modifying objects of managed type through fixed pointers can results in undefined behavior.</span></span> <span data-ttu-id="eaf18-371">Například protože řetězce jsou neměnné, je programátora povinností ujistit se, že znaky odkazuje ukazatel na řetězec pevné délky nezmění.</span><span class="sxs-lookup"><span data-stu-id="eaf18-371">For example, because strings are immutable, it is the programmer's responsibility to ensure that the characters referenced by a pointer to a fixed string are not modified.</span></span>

<span data-ttu-id="eaf18-372">Automatické ukončení hodnotou null řetězců je obzvláště užitečná při volání externí rozhraní API, která očekávají řetězce "Ve stylu jazyka C".</span><span class="sxs-lookup"><span data-stu-id="eaf18-372">The automatic null-termination of strings is particularly convenient when calling external APIs that expect "C-style" strings.</span></span> <span data-ttu-id="eaf18-373">Všimněte si však, že smí obsahovat instanci řetězce obsahující znaky s hodnotou null.</span><span class="sxs-lookup"><span data-stu-id="eaf18-373">Note, however, that a string instance is permitted to contain null characters.</span></span> <span data-ttu-id="eaf18-374">Pokud tyto znaky s hodnotou null, bude při považován za zakončený hodnotou null zobrazit ořezané řetězec `char*`.</span><span class="sxs-lookup"><span data-stu-id="eaf18-374">If such null characters are present, the string will appear truncated when treated as a null-terminated `char*`.</span></span>

## <a name="fixed-size-buffers"></a><span data-ttu-id="eaf18-375">Vyrovnávací paměti pevné velikosti</span><span class="sxs-lookup"><span data-stu-id="eaf18-375">Fixed size buffers</span></span>

<span data-ttu-id="eaf18-376">Vyrovnávací paměti pevné velikosti slouží k deklaraci pole v řádku "Stylu C" jako členy struktury a jsou užitečné hlavně pro propojení s nespravované rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="eaf18-376">Fixed size buffers are used to declare "C style" in-line arrays as members of structs, and are primarily useful for interfacing with unmanaged APIs.</span></span>

### <a name="fixed-size-buffer-declarations"></a><span data-ttu-id="eaf18-377">Deklarace vyrovnávací paměti pevné velikosti</span><span class="sxs-lookup"><span data-stu-id="eaf18-377">Fixed size buffer declarations</span></span>

<span data-ttu-id="eaf18-378">A ***pevné velikosti vyrovnávací paměti*** je člen, který představuje úložiště pro vyrovnávací paměť pevné délky proměnných daného typu.</span><span class="sxs-lookup"><span data-stu-id="eaf18-378">A ***fixed size buffer*** is a member that represents storage for a fixed length buffer of variables of a given type.</span></span> <span data-ttu-id="eaf18-379">Deklarace vyrovnávací paměti pevné velikosti představuje jeden nebo více vyrovnávací paměti pevné velikosti typu daného elementu.</span><span class="sxs-lookup"><span data-stu-id="eaf18-379">A fixed size buffer declaration introduces one or more fixed size buffers of a given element type.</span></span> <span data-ttu-id="eaf18-380">Vyrovnávací paměti pevné velikosti jsou povolené jenom v deklaracích struktury a může dojít pouze v kontextu unsafe ([nezabezpečený kontext](unsafe-code.md#unsafe-contexts)).</span><span class="sxs-lookup"><span data-stu-id="eaf18-380">Fixed size buffers are only permitted in struct declarations and can only occur in unsafe contexts ([Unsafe contexts](unsafe-code.md#unsafe-contexts)).</span></span>

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

<span data-ttu-id="eaf18-381">Deklarace vyrovnávací paměti pevné velikosti může zahrnovat sadu atributů ([atributy](attributes.md)), `new` modifikátor ([modifikátory](classes.md#modifiers)), platnou kombinaci čtyři přístupu modifikátory přístupu ([typu parametry a omezením](classes.md#type-parameters-and-constraints)) a `unsafe` modifikátor ([nezabezpečený kontext](unsafe-code.md#unsafe-contexts)).</span><span class="sxs-lookup"><span data-stu-id="eaf18-381">A fixed size buffer declaration may include a set of attributes ([Attributes](attributes.md)), a `new` modifier ([Modifiers](classes.md#modifiers)), a valid combination of the four access modifiers ([Type parameters and constraints](classes.md#type-parameters-and-constraints)) and an `unsafe` modifier ([Unsafe contexts](unsafe-code.md#unsafe-contexts)).</span></span> <span data-ttu-id="eaf18-382">Atributy a modifikátory platí pro všechny členy deklarované deklarací vyrovnávací paměti pevné velikosti.</span><span class="sxs-lookup"><span data-stu-id="eaf18-382">The attributes and modifiers apply to all of the members declared by the fixed size buffer declaration.</span></span> <span data-ttu-id="eaf18-383">Jedná se o chybu pro stejný modifikátor objevit více než jednou v deklaraci vyrovnávací paměti pevné velikosti.</span><span class="sxs-lookup"><span data-stu-id="eaf18-383">It is an error for the same modifier to appear multiple times in a fixed size buffer declaration.</span></span>

<span data-ttu-id="eaf18-384">Deklarace vyrovnávací paměti pevné velikosti není dovoleno zahrnout `static` modifikátor.</span><span class="sxs-lookup"><span data-stu-id="eaf18-384">A fixed size buffer declaration is not permitted to include the `static` modifier.</span></span>

<span data-ttu-id="eaf18-385">Typ elementu deklaraci vyrovnávací paměti pevné velikosti vyrovnávací paměti určuje typ elementu vyrovnávací paměti zavedeným deklarací.</span><span class="sxs-lookup"><span data-stu-id="eaf18-385">The buffer element type of a fixed size buffer declaration specifies the element type of the buffer(s) introduced by the declaration.</span></span> <span data-ttu-id="eaf18-386">Typ prvku vyrovnávací paměti musí být jeden z předdefinovaných typů `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, nebo `bool`.</span><span class="sxs-lookup"><span data-stu-id="eaf18-386">The buffer element type must be one of the predefined types `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, or `bool`.</span></span>

<span data-ttu-id="eaf18-387">Typ prvku vyrovnávací paměti následuje seznam deklarátorů vyrovnávací paměti pevné velikosti, z nichž každý představuje nového člena.</span><span class="sxs-lookup"><span data-stu-id="eaf18-387">The buffer element type is followed by a list of fixed size buffer declarators, each of which introduces a new member.</span></span> <span data-ttu-id="eaf18-388">Deklarátor vyrovnávací paměti pevné velikosti se skládá z identifikátor, který pojmenovává člen, za nímž následuje konstantní výraz uzavřený do `[` a `]` tokeny.</span><span class="sxs-lookup"><span data-stu-id="eaf18-388">A fixed size buffer declarator consists of an identifier that names the member, followed by a constant expression enclosed in `[` and `]` tokens.</span></span> <span data-ttu-id="eaf18-389">Konstantní výraz označuje počet prvků v členu zavedené tento deklarátor vyrovnávací paměti pevné velikosti.</span><span class="sxs-lookup"><span data-stu-id="eaf18-389">The constant expression denotes the number of elements in the member introduced by that fixed size buffer declarator.</span></span> <span data-ttu-id="eaf18-390">Typ konstantního výrazu musí být implicitně převést na typ `int`, a hodnota musí být nenulové kladné celé číslo.</span><span class="sxs-lookup"><span data-stu-id="eaf18-390">The type of the constant expression must be implicitly convertible to type `int`, and the value must be a non-zero positive integer.</span></span>

<span data-ttu-id="eaf18-391">Je zaručeno, že prvky vyrovnávací paměti pevné velikosti rozloží postupně v paměti.</span><span class="sxs-lookup"><span data-stu-id="eaf18-391">The elements of a fixed size buffer are guaranteed to be laid out sequentially in memory.</span></span>

<span data-ttu-id="eaf18-392">Deklarace vyrovnávací paměti pevné velikosti, která deklaruje několik vyrovnávací paměti pevné velikosti je ekvivalentní více deklarací deklaraci jeden pevné velikosti vyrovnávací paměti s stejné atributy a typy prvků.</span><span class="sxs-lookup"><span data-stu-id="eaf18-392">A fixed size buffer declaration that declares multiple fixed size buffers is equivalent to multiple declarations of a single fixed size buffer declaration with the same attributes, and element types.</span></span> <span data-ttu-id="eaf18-393">Příklad</span><span class="sxs-lookup"><span data-stu-id="eaf18-393">For example</span></span>

```csharp
unsafe struct A
{
   public fixed int x[5], y[10], z[100];
}
```

<span data-ttu-id="eaf18-394">je ekvivalentem</span><span class="sxs-lookup"><span data-stu-id="eaf18-394">is equivalent to</span></span>

```csharp
unsafe struct A
{
   public fixed int x[5];
   public fixed int y[10];
   public fixed int z[100];
}
```

### <a name="fixed-size-buffers-in-expressions"></a><span data-ttu-id="eaf18-395">Vyrovnávací paměti pevné velikosti ve výrazech</span><span class="sxs-lookup"><span data-stu-id="eaf18-395">Fixed size buffers in expressions</span></span>

<span data-ttu-id="eaf18-396">Člen vyhledávání ([operátory](expressions.md#operators)) s pevnou velikostí vyrovnávací paměti člen pokračuje úplně stejně jako člen vyhledávací pole.</span><span class="sxs-lookup"><span data-stu-id="eaf18-396">Member lookup ([Operators](expressions.md#operators)) of a fixed size buffer member proceeds exactly like member lookup of a field.</span></span>

<span data-ttu-id="eaf18-397">Vyrovnávací paměť pevné velikosti můžete odkazovat pomocí výrazu *simple_name* ([odvození typu](expressions.md#type-inference)) nebo *member_access* ([kompilace kontrolu řešení přetížení dynamické](expressions.md#compile-time-checking-of-dynamic-overload-resolution)).</span><span class="sxs-lookup"><span data-stu-id="eaf18-397">A fixed size buffer can be referenced in an expression using a *simple_name* ([Type inference](expressions.md#type-inference)) or a *member_access* ([Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution)).</span></span>

<span data-ttu-id="eaf18-398">Při odkazování na člena vyrovnávací paměti pevné velikosti jako jednoduchý název, efekt je stejný jako přístup ke členu formuláře `this.I`, kde `I` je člen vyrovnávací paměti pevné velikosti.</span><span class="sxs-lookup"><span data-stu-id="eaf18-398">When a fixed size buffer member is referenced as a simple name, the effect is the same as a member access of the form `this.I`, where `I` is the fixed size buffer member.</span></span>

<span data-ttu-id="eaf18-399">V přístupu ke členu formuláře `E.I`, pokud `E` je typu Struktura a člen vyhledávání `I` v tom, že typ struktury identifikuje členem pevné velikosti, pak `E.I` je vyhodnocen utajované následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="eaf18-399">In a member access of the form `E.I`, if `E` is of a struct type and a member lookup of `I` in that struct type identifies a fixed size member, then `E.I` is evaluated an classified as follows:</span></span>

*  <span data-ttu-id="eaf18-400">Pokud výraz `E.I` nedojde v nezabezpečeném kontextu, dojde k chybě kompilace.</span><span class="sxs-lookup"><span data-stu-id="eaf18-400">If the expression `E.I` does not occur in an unsafe context, a compile-time error occurs.</span></span>
*  <span data-ttu-id="eaf18-401">Pokud `E` je klasifikován jako hodnotu a dojde k chybě kompilace.</span><span class="sxs-lookup"><span data-stu-id="eaf18-401">If `E` is classified as a value, a compile-time error occurs.</span></span>
*  <span data-ttu-id="eaf18-402">Jinak, pokud `E` přesunutelný proměnná ([Fixed a přesunutelný proměnné](unsafe-code.md#fixed-and-moveable-variables)) a výraz `E.I` není *fixed_pointer_initializer* ([pevné příkaz](unsafe-code.md#the-fixed-statement)), dojde k chybě kompilace.</span><span class="sxs-lookup"><span data-stu-id="eaf18-402">Otherwise, if `E` is a moveable variable ([Fixed and moveable variables](unsafe-code.md#fixed-and-moveable-variables)) and the expression `E.I` is not a *fixed_pointer_initializer* ([The fixed statement](unsafe-code.md#the-fixed-statement)), a compile-time error occurs.</span></span>
*  <span data-ttu-id="eaf18-403">V opačném případě `E` odkazuje pevná proměnná a výsledek výrazu je ukazatel na první prvek člen vyrovnávací paměti pevné velikosti `I` v `E`.</span><span class="sxs-lookup"><span data-stu-id="eaf18-403">Otherwise, `E` references a fixed variable and the result of the expression is a pointer to the first element of the fixed size buffer member `I` in `E`.</span></span> <span data-ttu-id="eaf18-404">Výsledek je typu `S*`, kde `S` je typ prvku `I`a je klasifikován jako hodnotu.</span><span class="sxs-lookup"><span data-stu-id="eaf18-404">The result is of type `S*`, where `S` is the element type of `I`, and is classified as a value.</span></span>

<span data-ttu-id="eaf18-405">Následné prvky vyrovnávací paměti pevné velikosti lze přistupovat pomocí ukazatele operace od prvního prvku.</span><span class="sxs-lookup"><span data-stu-id="eaf18-405">The subsequent elements of the fixed size buffer can be accessed using pointer operations from the first element.</span></span> <span data-ttu-id="eaf18-406">Na rozdíl od přístup k polím přístup k prvkům vyrovnávací paměti pevné velikosti je nebezpečné operace a není zaškrtnuté políčko rozsahu.</span><span class="sxs-lookup"><span data-stu-id="eaf18-406">Unlike access to arrays, access to the elements of a fixed size buffer is an unsafe operation and is not range checked.</span></span>

<span data-ttu-id="eaf18-407">Následující příklad deklaruje a používá strukturu se členem vyrovnávací paměti pevné velikosti.</span><span class="sxs-lookup"><span data-stu-id="eaf18-407">The following example declares and uses a struct with a fixed size buffer member.</span></span>

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

### <a name="definite-assignment-checking"></a><span data-ttu-id="eaf18-408">Kontrola jednoznačného přiřazení</span><span class="sxs-lookup"><span data-stu-id="eaf18-408">Definite assignment checking</span></span>

<span data-ttu-id="eaf18-409">Vyrovnávací paměti pevné velikosti nejsou v souladu s Kontrola jednoznačného přiřazení ([jednoznačného přiřazení](variables.md#definite-assignment)), a členy vyrovnávací paměti pevné velikosti jsou ignorovány pro účely kontroly proměnné typu Struktura jednoznačného přiřazení.</span><span class="sxs-lookup"><span data-stu-id="eaf18-409">Fixed size buffers are not subject to definite assignment checking ([Definite assignment](variables.md#definite-assignment)), and fixed size buffer members are ignored for purposes of definite assignment checking of struct type variables.</span></span>

<span data-ttu-id="eaf18-410">Do vnějšího obsahující proměnné struktury člena vyrovnávací paměti pevné velikosti je statická proměnná, proměnnou instance instanci třídy nebo k elementu pole, prvky vyrovnávací paměti pevné velikosti jsou automaticky inicializovány na výchozí hodnoty ([Výchozí hodnoty](variables.md#default-values)).</span><span class="sxs-lookup"><span data-stu-id="eaf18-410">When the outermost containing struct variable of a fixed size buffer member is a static variable, an instance variable of a class instance, or an array element, the elements of the fixed size buffer are automatically initialized to their default values ([Default values](variables.md#default-values)).</span></span> <span data-ttu-id="eaf18-411">Ve všech ostatních případech původní obsah vyrovnávací paměti pevné velikosti není definováno.</span><span class="sxs-lookup"><span data-stu-id="eaf18-411">In all other cases, the initial content of a fixed size buffer is undefined.</span></span>

## <a name="stack-allocation"></a><span data-ttu-id="eaf18-412">Přidělení zásobníku</span><span class="sxs-lookup"><span data-stu-id="eaf18-412">Stack allocation</span></span>

<span data-ttu-id="eaf18-413">V nezabezpečeném kontextu deklarace lokální proměnné ([místní deklarace proměnné](statements.md#local-variable-declarations)) může obsahovat inicializátor přidělení zásobníku, která přidělí paměť ze zásobníku volání.</span><span class="sxs-lookup"><span data-stu-id="eaf18-413">In an unsafe context, a local variable declaration ([Local variable declarations](statements.md#local-variable-declarations)) may include a stack allocation initializer which allocates memory from the call stack.</span></span>

```antlr
local_variable_initializer_unsafe
    : stackalloc_initializer
    ;

stackalloc_initializer
    : 'stackalloc' unmanaged_type '[' expression ']'
    ;
```

<span data-ttu-id="eaf18-414">*Unmanaged_type* Určuje typ položky, které se uloží do nově přiděleného umístění a *výraz* označuje číslo, které z těchto položek.</span><span class="sxs-lookup"><span data-stu-id="eaf18-414">The *unmanaged_type* indicates the type of the items that will be stored in the newly allocated location, and the *expression* indicates the number of these items.</span></span> <span data-ttu-id="eaf18-415">Společně tyto zadejte velikost požadované alokace.</span><span class="sxs-lookup"><span data-stu-id="eaf18-415">Taken together, these specify the required allocation size.</span></span> <span data-ttu-id="eaf18-416">Protože velikost přidělení zásobníku nemůže být záporná, je chyba kompilace můžete určit počet položek jako *constant_expression* vyhodnocenou nečíselnou na zápornou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="eaf18-416">Since the size of a stack allocation cannot be negative, it is a compile-time error to specify the number of items as a *constant_expression* that evaluates to a negative value.</span></span>

<span data-ttu-id="eaf18-417">Inicializátoru přidělení zásobníku ve tvaru `stackalloc T[E]` vyžaduje `T` bude nespravovaným typem ([typy ukazatelů](unsafe-code.md#pointer-types)) a `E` bude výraz typu `int`.</span><span class="sxs-lookup"><span data-stu-id="eaf18-417">A stack allocation initializer of the form `stackalloc T[E]` requires `T` to be an unmanaged type ([Pointer types](unsafe-code.md#pointer-types)) and `E` to be an expression of type `int`.</span></span> <span data-ttu-id="eaf18-418">Konstrukce přiděluje `E * sizeof(T)` bajtů z volání zásobníku a vrátí ukazatel typu `T*`, do nově přiděleného bloku.</span><span class="sxs-lookup"><span data-stu-id="eaf18-418">The construct allocates `E * sizeof(T)` bytes from the call stack and returns a pointer, of type `T*`, to the newly allocated block.</span></span> <span data-ttu-id="eaf18-419">Pokud `E` je hodnota záporná, pak chování není definováno.</span><span class="sxs-lookup"><span data-stu-id="eaf18-419">If `E` is a negative value, then the behavior is undefined.</span></span> <span data-ttu-id="eaf18-420">Pokud `E` je nula, pak je provedena bez přidělení a Vrácený ukazatel je definováno implementací.</span><span class="sxs-lookup"><span data-stu-id="eaf18-420">If `E` is zero, then no allocation is made, and the pointer returned is implementation-defined.</span></span> <span data-ttu-id="eaf18-421">Pokud není k dispozici dostatek paměti k přidělení bloku určité velikosti, `System.StackOverflowException` je vyvolána výjimka.</span><span class="sxs-lookup"><span data-stu-id="eaf18-421">If there is not enough memory available to allocate a block of the given size, a `System.StackOverflowException` is thrown.</span></span>

<span data-ttu-id="eaf18-422">Obsah nově přidělenou paměť není definován.</span><span class="sxs-lookup"><span data-stu-id="eaf18-422">The content of the newly allocated memory is undefined.</span></span>

<span data-ttu-id="eaf18-423">Inicializátory přidělení zásobníku nejsou povolené v `catch` nebo `finally` bloky ([příkazu try](statements.md#the-try-statement)).</span><span class="sxs-lookup"><span data-stu-id="eaf18-423">Stack allocation initializers are not permitted in `catch` or `finally` blocks ([The try statement](statements.md#the-try-statement)).</span></span>

<span data-ttu-id="eaf18-424">Neexistuje žádný způsob, jak explicitně uvolnit paměť přidělena pomocí `stackalloc`.</span><span class="sxs-lookup"><span data-stu-id="eaf18-424">There is no way to explicitly free memory allocated using `stackalloc`.</span></span> <span data-ttu-id="eaf18-425">Po návratu této členské funkce automaticky zahodí všechny bloky paměti přiděleny vytvořené během provádění členské funkce.</span><span class="sxs-lookup"><span data-stu-id="eaf18-425">All stack allocated memory blocks created during the execution of a function member are automatically discarded when that function member returns.</span></span> <span data-ttu-id="eaf18-426">To odpovídá `alloca` funkce, rozšíření běžně vyskytují v implementace jazyka C a C++.</span><span class="sxs-lookup"><span data-stu-id="eaf18-426">This corresponds to the `alloca` function, an extension commonly found in C and C++ implementations.</span></span>

<span data-ttu-id="eaf18-427">V příkladu</span><span class="sxs-lookup"><span data-stu-id="eaf18-427">In the example</span></span>

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

<span data-ttu-id="eaf18-428">`stackalloc` inicializátoru je používán `IntToString` metoda přidělit vyrovnávací paměť v zásobníku 16 znaků.</span><span class="sxs-lookup"><span data-stu-id="eaf18-428">a `stackalloc` initializer is used in the `IntToString` method to allocate a buffer of 16 characters on the stack.</span></span> <span data-ttu-id="eaf18-429">Vyrovnávací paměť se automaticky zruší po návratu metody.</span><span class="sxs-lookup"><span data-stu-id="eaf18-429">The buffer is automatically discarded when the method returns.</span></span>

## <a name="dynamic-memory-allocation"></a><span data-ttu-id="eaf18-430">Dynamické přidělení paměti</span><span class="sxs-lookup"><span data-stu-id="eaf18-430">Dynamic memory allocation</span></span>

<span data-ttu-id="eaf18-431">S výjimkou `stackalloc` operátor C# poskytuje žádné předdefinované konstrukce pro správu shromážděných paměti bez uvolnění paměti.</span><span class="sxs-lookup"><span data-stu-id="eaf18-431">Except for the `stackalloc` operator, C# provides no predefined constructs for managing non-garbage collected memory.</span></span> <span data-ttu-id="eaf18-432">Tyto služby jsou obvykle poskytuje podpůrné knihovny tříd nebo importovat přímo od základního operačního systému.</span><span class="sxs-lookup"><span data-stu-id="eaf18-432">Such services are typically provided by supporting class libraries or imported directly from the underlying operating system.</span></span> <span data-ttu-id="eaf18-433">Například `Memory` třídy níže ukazuje, jak funkce haldy základního operačního systému by mohly být přístupné z C#:</span><span class="sxs-lookup"><span data-stu-id="eaf18-433">For example, the `Memory` class below illustrates how the heap functions of an underlying operating system might be accessed from C#:</span></span>

```csharp
using System;
using System.Runtime.InteropServices;

public unsafe class Memory
{
    // Handle for the process heap. This handle is used in all calls to the
    // HeapXXX APIs in the methods below.
    static int ph = GetProcessHeap();

    // Private instance constructor to prevent instantiation.
    private Memory() {}

    // Allocates a memory block of the given size. The allocated memory is
    // automatically initialized to zero.
    public static void* Alloc(int size) {
        void* result = HeapAlloc(ph, HEAP_ZERO_MEMORY, size);
        if (result == null) throw new OutOfMemoryException();
        return result;
    }

    // Copies count bytes from src to dst. The source and destination
    // blocks are permitted to overlap.
    public static void Copy(void* src, void* dst, int count) {
        byte* ps = (byte*)src;
        byte* pd = (byte*)dst;
        if (ps > pd) {
            for (; count != 0; count--) *pd++ = *ps++;
        }
        else if (ps < pd) {
            for (ps += count, pd += count; count != 0; count--) *--pd = *--ps;
        }
    }

    // Frees a memory block.
    public static void Free(void* block) {
        if (!HeapFree(ph, 0, block)) throw new InvalidOperationException();
    }

    // Re-allocates a memory block. If the reallocation request is for a
    // larger size, the additional region of memory is automatically
    // initialized to zero.
    public static void* ReAlloc(void* block, int size) {
        void* result = HeapReAlloc(ph, HEAP_ZERO_MEMORY, block, size);
        if (result == null) throw new OutOfMemoryException();
        return result;
    }

    // Returns the size of a memory block.
    public static int SizeOf(void* block) {
        int result = HeapSize(ph, 0, block);
        if (result == -1) throw new InvalidOperationException();
        return result;
    }

    // Heap API flags
    const int HEAP_ZERO_MEMORY = 0x00000008;

    // Heap API functions
    [DllImport("kernel32")]
    static extern int GetProcessHeap();

    [DllImport("kernel32")]
    static extern void* HeapAlloc(int hHeap, int flags, int size);

    [DllImport("kernel32")]
    static extern bool HeapFree(int hHeap, int flags, void* block);

    [DllImport("kernel32")]
    static extern void* HeapReAlloc(int hHeap, int flags, void* block, int size);

    [DllImport("kernel32")]
    static extern int HeapSize(int hHeap, int flags, void* block);
}
```

<span data-ttu-id="eaf18-434">Příklad, který se používá `Memory` třídy jsou vypsáni níže:</span><span class="sxs-lookup"><span data-stu-id="eaf18-434">An example that uses the `Memory` class is given below:</span></span>

```csharp
class Test
{
    static void Main() {
        unsafe {
            byte* buffer = (byte*)Memory.Alloc(256);
            try {
                for (int i = 0; i < 256; i++) buffer[i] = (byte)i;
                byte[] array = new byte[256];
                fixed (byte* p = array) Memory.Copy(buffer, p, 256); 
            }
            finally {
                Memory.Free(buffer);
            }
            for (int i = 0; i < 256; i++) Console.WriteLine(array[i]);
        }
    }
}
```

<span data-ttu-id="eaf18-435">V příkladu se přiděluje 256 bajtů paměti prostřednictvím `Memory.Alloc` a inicializuje blok paměti s hodnotami zvýšení od 0 do 255.</span><span class="sxs-lookup"><span data-stu-id="eaf18-435">The example allocates 256 bytes of memory through `Memory.Alloc` and initializes the memory block with values increasing from 0 to 255.</span></span> <span data-ttu-id="eaf18-436">Potom přiděluje pole bajtů 256 element a používá `Memory.Copy` chcete zkopírovat obsah bloku paměti do bajtového pole.</span><span class="sxs-lookup"><span data-stu-id="eaf18-436">It then allocates a 256 element byte array and uses `Memory.Copy` to copy the contents of the memory block into the byte array.</span></span> <span data-ttu-id="eaf18-437">Nakonec blok paměti je uvolněna pomocí `Memory.Free` a obsah bajtové pole je výstup na konzole.</span><span class="sxs-lookup"><span data-stu-id="eaf18-437">Finally, the memory block is freed using `Memory.Free` and the contents of the byte array are output on the console.</span></span>
