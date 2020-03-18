---
ms.openlocfilehash: 6088a5cd41926d828013f1b8e5736fd2b7939e44
ms.sourcegitcommit: da452002c3f472165a0e1fa7759f494cc703ae31
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/27/2019
ms.locfileid: "79485040"
---
# <a name="compile-time-enforcement-of-safety-for-ref-like-types"></a><span data-ttu-id="fd248-101">Doba kompilace vynucování zabezpečení pro typy s odkazem na typ odkazu</span><span class="sxs-lookup"><span data-stu-id="fd248-101">Compile time enforcement of safety for ref-like types</span></span>

## <a name="introduction"></a><span data-ttu-id="fd248-102">Úvod</span><span class="sxs-lookup"><span data-stu-id="fd248-102">Introduction</span></span>

<span data-ttu-id="fd248-103">Hlavním důvodem pro další bezpečnostní pravidla při práci s typy jako `Span<T>` a `ReadOnlySpan<T>` je, že tyto typy musí být omezeny na zásobník spouštění.</span><span class="sxs-lookup"><span data-stu-id="fd248-103">The main reason for the additional safety rules when dealing with types like `Span<T>` and `ReadOnlySpan<T>` is that such types must be confined to the execution stack.</span></span>
 
<span data-ttu-id="fd248-104">Existují dva důvody, proč `Span<T>` a podobné typy musí být typy pouze zásobníku.</span><span class="sxs-lookup"><span data-stu-id="fd248-104">There are two reasons why `Span<T>` and similar types must be a stack-only types.</span></span>

1. <span data-ttu-id="fd248-105">`Span<T>` je sémanticky strukturou obsahující odkaz a rozsah-`(ref T data, int length)`.</span><span class="sxs-lookup"><span data-stu-id="fd248-105">`Span<T>` is semantically a struct containing a reference and a range - `(ref T data, int length)`.</span></span> <span data-ttu-id="fd248-106">Bez ohledu na skutečnou implementaci by zápis do takové struktury nepředstavoval atomii.</span><span class="sxs-lookup"><span data-stu-id="fd248-106">Regardless of actual implementation, writes to such struct would not be atomic.</span></span> <span data-ttu-id="fd248-107">Souběžná odtrhlina této struktury by vedla k tomu, že `length` neodpovídají `data`, což způsobuje přístup mimo rozsah a narušení bezpečnosti typů, což by nakonec mohlo vést k poškození haldy GC v zdánlivě bezpečném kódu.</span><span class="sxs-lookup"><span data-stu-id="fd248-107">Concurrent "tearing" of such struct would lead to the possibility of `length` not matching the `data`, causing out-of-range accesses and type-safety violations, which ultimately could result in GC heap corruption in seemingly "safe" code.</span></span>
2. <span data-ttu-id="fd248-108">Některé implementace `Span<T>` doslova obsahují spravovaný ukazatel v jednom z jeho polí.</span><span class="sxs-lookup"><span data-stu-id="fd248-108">Some implementations of `Span<T>` literally contain a managed pointer in one of its fields.</span></span> <span data-ttu-id="fd248-109">Spravované ukazatele nejsou podporovány jako pole objektů haldy a kódu, který spravuje za účelem vložení spravovaného ukazatele na haldu GC obvykle k chybě v době běhu.</span><span class="sxs-lookup"><span data-stu-id="fd248-109">Managed pointers are not supported as fields of heap objects and code that manages to put a managed pointer on the GC heap typically crashes at JIT time.</span></span>

<span data-ttu-id="fd248-110">Pokud jsou instance `Span<T>` omezené na existenci pouze zásobníku spuštění, budou všechny výše uvedené problémy zmírnit.</span><span class="sxs-lookup"><span data-stu-id="fd248-110">All the above problems would be alleviated if instances of `Span<T>` are constrained to exist only on the execution stack.</span></span> 

<span data-ttu-id="fd248-111">K dalšímu problému dojde z důvodu složení.</span><span class="sxs-lookup"><span data-stu-id="fd248-111">An additional problem arises due to composition.</span></span> <span data-ttu-id="fd248-112">Obecně je žádoucí vytvořit složitější datové typy, které by mohly vkládat `Span<T>` a `ReadOnlySpan<T>` instance.</span><span class="sxs-lookup"><span data-stu-id="fd248-112">It would be generally desirable to build more complex data types that would embed `Span<T>` and `ReadOnlySpan<T>` instances.</span></span> <span data-ttu-id="fd248-113">Takové složené typy by musely být struktury a měly by sdílet všechna rizika a požadavky `Span<T>`.</span><span class="sxs-lookup"><span data-stu-id="fd248-113">Such composite types would have to be structs and would share all the hazards and requirements of `Span<T>`.</span></span> <span data-ttu-id="fd248-114">V důsledku toho by se tady popsaná pravidla zabezpečení měla zobrazit v celém rozsahu **_typů podobných typu ref_** .</span><span class="sxs-lookup"><span data-stu-id="fd248-114">As a result the safety rules described here should be viewed as applicable to the whole range of **_ref-like types_**.</span></span>

<span data-ttu-id="fd248-115">[Specifikace jazyka konceptu](#draft-language-specification) je určena k zajištění toho, aby hodnoty typu odkazu byly pouze v zásobníku.</span><span class="sxs-lookup"><span data-stu-id="fd248-115">The [draft language specification](#draft-language-specification) is intended to ensure that values of a ref-like type occurs only on the stack.</span></span>

## <a name="generalized-ref-like-types-in-source-code"></a><span data-ttu-id="fd248-116">Zobecněné typy `ref-like` ve zdrojovém kódu</span><span class="sxs-lookup"><span data-stu-id="fd248-116">Generalized `ref-like` types in source code</span></span>

<span data-ttu-id="fd248-117">`ref-like` struktury jsou explicitně označeny ve zdrojovém kódu pomocí modifikátoru `ref`:</span><span class="sxs-lookup"><span data-stu-id="fd248-117">`ref-like` structs are explicitly marked in the source code using `ref` modifier:</span></span>

```csharp
ref struct TwoSpans<T>
{
    // can have ref-like instance fields
    public Span<T> first;
    public Span<T> second;
} 

// error: arrays of ref-like types are not allowed. 
TwoSpans<T>[] arr = null;

```

<span data-ttu-id="fd248-118">Určení struktury jako ref jako umožní, aby struktura měla pole instance s odkazem, a také všechny požadavky na typy s odkazem, které se vztahují na strukturu.</span><span class="sxs-lookup"><span data-stu-id="fd248-118">Designating a struct as ref-like will allow the struct to have ref-like instance fields and will also make all the requirements of ref-like types applicable to the struct.</span></span> 

## <a name="metadata-representation-or-ref-like-structs"></a><span data-ttu-id="fd248-119">Reprezentace metadat nebo struktury podobné odkazům</span><span class="sxs-lookup"><span data-stu-id="fd248-119">Metadata representation or ref-like structs</span></span>

<span data-ttu-id="fd248-120">Struktury podobné odkazu budou označeny atributem **System. Runtime. CompilerServices. IsRefLikeAttribute** .</span><span class="sxs-lookup"><span data-stu-id="fd248-120">Ref-like structs will be marked with **System.Runtime.CompilerServices.IsRefLikeAttribute** attribute.</span></span>

<span data-ttu-id="fd248-121">Atribut se přidá do běžných základních knihoven, jako je `mscorlib`.</span><span class="sxs-lookup"><span data-stu-id="fd248-121">The attribute will be added to common base libraries such as `mscorlib`.</span></span> <span data-ttu-id="fd248-122">V případě, že atribut není k dispozici, kompilátor vygeneruje interní, podobně jako jiné atributy Embedded-na vyžádání, jako je například `IsReadOnlyAttribute`.</span><span class="sxs-lookup"><span data-stu-id="fd248-122">In a case if the attribute is not available, compiler will generate an internal one similarly to other embedded-on-demand attributes such as `IsReadOnlyAttribute`.</span></span>

<span data-ttu-id="fd248-123">Bude provedena další míra, která brání použití struktur typu ref v kompilátorech, které nejsou obeznámeny s pravidly zabezpečení (zahrnuje C# kompilátory před rozhraním, ve kterém je tato funkce implementována).</span><span class="sxs-lookup"><span data-stu-id="fd248-123">An additional measure will be taken to prevent the use of ref-like structs in compilers not familiar with the safety rules (this includes C# compilers prior to the one in which this feature is implemented).</span></span> 

<span data-ttu-id="fd248-124">Neexistují žádné další vhodné alternativy, které fungují ve starších kompilátorech bez obsluhy, atribut `Obsolete` se známým řetězcem bude přidán do všech struktur podobně jako referenčních.</span><span class="sxs-lookup"><span data-stu-id="fd248-124">Having no other good alternatives that work in old compilers without servicing, an `Obsolete` attribute with a known string will be added to all ref-like structs.</span></span> <span data-ttu-id="fd248-125">Kompilátory, které vědí, jak použít typy ref, budou tuto konkrétní formu `Obsolete`ignorovat.</span><span class="sxs-lookup"><span data-stu-id="fd248-125">Compilers that know how to use ref-like types will ignore this particular form of `Obsolete`.</span></span>

<span data-ttu-id="fd248-126">Typická reprezentace metadat:</span><span class="sxs-lookup"><span data-stu-id="fd248-126">A typical metadata representation:</span></span>

```csharp
    [IsRefLike]
    [Obsolete("Types with embedded references are not supported in this version of your compiler.")]
    public struct TwoSpans<T>
    {
       // . . . .
    }
```

<span data-ttu-id="fd248-127">Poznámka: není to cílem, aby to bylo možné provést, protože jakékoli použití typů s odkazem na staré kompilátory nefunguje 100%.</span><span class="sxs-lookup"><span data-stu-id="fd248-127">NOTE: it is not the goal to make it so that any use of ref-like types on old compilers fails 100%.</span></span> <span data-ttu-id="fd248-128">To je obtížné dosáhnout a není bezpodmínečně nutné.</span><span class="sxs-lookup"><span data-stu-id="fd248-128">That is hard to achieve and is not strictly necessary.</span></span> <span data-ttu-id="fd248-129">Například by byl vždy způsob, jak získat `Obsolete` pomocí dynamického kódu, nebo například vytváření pole typů s odkazem pomocí reflexe.</span><span class="sxs-lookup"><span data-stu-id="fd248-129">For example there would always be a way to get around the `Obsolete` using dynamic code or, for example, creating an array of ref-like types through reflection.</span></span>

<span data-ttu-id="fd248-130">Zejména pokud chce uživatel skutečně umístit atribut `Obsolete` nebo `Deprecated` na typ podobný odkazem, nebudete mít žádnou volbu, než vygenerujeme předdefinovanou hodnotu, protože atribut `Obsolete` nelze použít více než jednou.</span><span class="sxs-lookup"><span data-stu-id="fd248-130">In particular, if user wants to actually put an `Obsolete` or `Deprecated` attribute on a ref-like type, we will have no choice other than not emitting the predefined one since `Obsolete` attribute cannot be applied more than once..</span></span>  

## <a name="examples"></a><span data-ttu-id="fd248-131">Příklady:</span><span class="sxs-lookup"><span data-stu-id="fd248-131">Examples:</span></span>

```csharp
SpanLikeType M1(ref SpanLikeType x, Span<byte> y)
{
    // this is all valid, unconcerned with stack-referring stuff
    var local = new SpanLikeType(y);
    x = local;
    return x;
}

void Test1(ref SpanLikeType param1, Span<byte> param2)
{
    Span<byte> stackReferring1 = stackalloc byte[10];
    var stackReferring2 = new SpanLikeType(stackReferring1);

    // this is allowed
    stackReferring2 = M1(ref stackReferring2, stackReferring1);

    // this is NOT allowed
    stackReferring2 = M1(ref param1, stackReferring1);

    // this is NOT allowed
    param1 = M1(ref stackReferring2, stackReferring1);

    // this is NOT allowed
    param2 = stackReferring1.Slice(10);

    // this is allowed
    param1 = new SpanLikeType(param2);

    // this is allowed
    stackReferring2 = param1;
}

ref SpanLikeType M2(ref SpanLikeType x)
{
    return ref x;
}

ref SpanLikeType Test2(ref SpanLikeType param1, Span<byte> param2)
{
    Span<byte> stackReferring1 = stackalloc byte[10];
    var stackReferring2 = new SpanLikeType(stackReferring1);

    ref var stackReferring3 = M2(ref stackReferring2);

    // this is allowed
    stackReferring3 = M1(ref stackReferring2, stackReferring1);

    // this is allowed
    M2(ref stackReferring3) = stackReferring2;

    // this is NOT allowed
    M1(ref param1) = stackReferring2;

    // this is NOT allowed
    param1 = stackReferring3;

    // this is NOT allowed
    return ref stackReferring3;

    // this is allowed
    return ref param1;
}

```

----------------

## <a name="draft-language-specification"></a><span data-ttu-id="fd248-132">Specifikace jazyka konceptu</span><span class="sxs-lookup"><span data-stu-id="fd248-132">Draft language specification</span></span>

<span data-ttu-id="fd248-133">Níže popisujeme sadu pravidel zabezpečení pro typy s odkazem (`ref struct`), aby se zajistilo, že hodnoty těchto typů nastávají pouze v zásobníku.</span><span class="sxs-lookup"><span data-stu-id="fd248-133">Below we describe a set of safety rules for ref-like types (`ref struct`s) to ensure that values of these types occur only on the stack.</span></span> <span data-ttu-id="fd248-134">Jiná, jednodušší sada pravidel zabezpečení by byla možná, pokud národní prostředí nelze předat odkazem.</span><span class="sxs-lookup"><span data-stu-id="fd248-134">A different, simpler set of safety rules would be possible if locals cannot be passed by reference.</span></span> <span data-ttu-id="fd248-135">Tato specifikace by také umožnila bezpečné opětovné přiřazení místních hodnot ref.</span><span class="sxs-lookup"><span data-stu-id="fd248-135">This specification would also permit the safe reassignment of ref locals.</span></span>

### <a name="overview"></a><span data-ttu-id="fd248-136">Přehled</span><span class="sxs-lookup"><span data-stu-id="fd248-136">Overview</span></span>

<span data-ttu-id="fd248-137">K jednotlivým výrazům přiřadíme v době kompilace koncept toho, s jakým rozsahem je povolený příkaz k úniku a "bezpečnému řídicímu panelu".</span><span class="sxs-lookup"><span data-stu-id="fd248-137">We associate with each expression at compile-time the concept of what scope that expression is permitted to escape to, "safe-to-escape".</span></span> <span data-ttu-id="fd248-138">Podobně platí, že pro každou lvalue udržujeme koncept, na který je odkaz, na který se vztahuje odkaz, ke kterému se dá odkazovat pomocí příkazu "ref-Safe-to-Escape".</span><span class="sxs-lookup"><span data-stu-id="fd248-138">Similarly, for each lvalue we maintain a concept of what scope a reference to it is permitted to escape to, "ref-safe-to-escape".</span></span> <span data-ttu-id="fd248-139">Pro daný výraz l-hodnoty se mohou lišit.</span><span class="sxs-lookup"><span data-stu-id="fd248-139">For a given lvalue expression, these may be different.</span></span>

<span data-ttu-id="fd248-140">Jedná se o podobnou funkci "bezpečného vrácení" lokálních hodnot typu ref, ale je přesnější.</span><span class="sxs-lookup"><span data-stu-id="fd248-140">These are analogous to the "safe to return" of the ref locals feature, but it is more fine-grained.</span></span> <span data-ttu-id="fd248-141">Místo toho, aby se "bezpečné" vrácené položky v rámci výrazu zavedly pouze bez ohledu na to, zda (nebo ne) může být řídicí Metoda uvozena jako celek, záznamy typu bezpečného k úniku, které mají rozsah</span><span class="sxs-lookup"><span data-stu-id="fd248-141">Where the "safe-to-return" of an expression records only whether (or not) it may escape the enclosing method as a whole, the safe-to-escape records which scope it may escape to (which scope it may not escape beyond).</span></span> <span data-ttu-id="fd248-142">Základní bezpečnostní mechanismus se vynutil následujícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="fd248-142">The basic safety mechanism is enforced as follows.</span></span> <span data-ttu-id="fd248-143">Vzhledem k přiřazení z výrazu E1 s oborem typu "bezpečného", s rozsahem "S1", výrazem (lvalue), který je typu Safe-to-Escape S2, se jedná o chybu, pokud je S2 širší rozsah než S1.</span><span class="sxs-lookup"><span data-stu-id="fd248-143">Given an assignment from an expression E1 with a safe-to-escape scope S1, to an (lvalue) expression E2 with safe-to-escape scope S2, it is an error if S2 is a wider scope than S1.</span></span> <span data-ttu-id="fd248-144">Podle konstrukce jsou dva obory S1 a S2 v relaci vnořování, protože právní výraz je vždy bezpečný – vrácen z nějakého oboru ohraničujícího výraz.</span><span class="sxs-lookup"><span data-stu-id="fd248-144">By construction, the two scopes S1 and S2 are in a nesting relationship, because a legal expression is always safe-to-return from some scope enclosing the expression.</span></span>

<span data-ttu-id="fd248-145">V době, kdy je to dostačující pro účely analýzy, pro podporu pouze dvou oborů – externě na metodu a rozsahu nejvyšší úrovně metody.</span><span class="sxs-lookup"><span data-stu-id="fd248-145">For the time being it is sufficient, for the purpose of the analysis, to support just two scopes - external to the method, and top-level scope of the method.</span></span> <span data-ttu-id="fd248-146">Důvodem je, že hodnoty typu ref s vnitřními obory nelze vytvořit a lokální hodnoty REF nepodporují opakované přiřazení.</span><span class="sxs-lookup"><span data-stu-id="fd248-146">That is because ref-like values with inner scopes cannot be created and ref locals do not support re-assignment.</span></span> <span data-ttu-id="fd248-147">Tato pravidla však mohou podporovat více než dvě úrovně rozsahu.</span><span class="sxs-lookup"><span data-stu-id="fd248-147">The rules, however, can support more than two scope levels.</span></span>

<span data-ttu-id="fd248-148">Přesná pravidla pro výpočet stavu *bezpečných a vrácených* výrazů a pravidla upravující legalitu výrazů sledují.</span><span class="sxs-lookup"><span data-stu-id="fd248-148">The precise rules for computing the *safe-to-return* status of an expression, and the rules governing the legality of expressions, follow.</span></span>

### <a name="ref-safe-to-escape"></a><span data-ttu-id="fd248-149">ref-Safe-to-Escape</span><span class="sxs-lookup"><span data-stu-id="fd248-149">ref-safe-to-escape</span></span>

<span data-ttu-id="fd248-150">Typ *ref-Safe-to-Escape* je obor, ohraničující výraz l-hodnota, na který je bezpečné pro odkaz na lvalue pro Escape.</span><span class="sxs-lookup"><span data-stu-id="fd248-150">The *ref-safe-to-escape* is a scope, enclosing an lvalue expression, to which it is safe for a ref to the lvalue to escape to.</span></span> <span data-ttu-id="fd248-151">Pokud je tento obor celou metodou, říkáme, že odkaz na lvalue je *bezpečný pro návrat* z metody.</span><span class="sxs-lookup"><span data-stu-id="fd248-151">If that scope is the entire method, we say that a ref to the lvalue is *safe to return* from the method.</span></span>

### <a name="safe-to-escape"></a><span data-ttu-id="fd248-152">bezpečné – řídicí</span><span class="sxs-lookup"><span data-stu-id="fd248-152">safe-to-escape</span></span>

<span data-ttu-id="fd248-153">Typ *Safe-to-Escape* je obor, ve kterém je uveden výraz, na který je bezpečná hodnota pro únik hodnoty.</span><span class="sxs-lookup"><span data-stu-id="fd248-153">The *safe-to-escape* is a scope, enclosing an expression, to which it is safe for the value to escape to.</span></span> <span data-ttu-id="fd248-154">Pokud je tento obor celou metodou, říkáme, že hodnotu lze *bezpečně vrátit* z metody.</span><span class="sxs-lookup"><span data-stu-id="fd248-154">If that scope is the entire method, we say that a the value is *safe to return* from the method.</span></span>

<span data-ttu-id="fd248-155">Výraz, jehož typ není typu `ref struct`, je *bezpečným návratem* z celé nadřazené metody.</span><span class="sxs-lookup"><span data-stu-id="fd248-155">An expression whose type is not a `ref struct` type is *safe-to-return* from the entire enclosing method.</span></span> <span data-ttu-id="fd248-156">V opačném případě odkazujeme na níže uvedená pravidla.</span><span class="sxs-lookup"><span data-stu-id="fd248-156">Otherwise we refer to the rules below.</span></span>

#### <a name="parameters"></a><span data-ttu-id="fd248-157">Parametry</span><span class="sxs-lookup"><span data-stu-id="fd248-157">Parameters</span></span>

<span data-ttu-id="fd248-158">L-hodnota určující formální parametr je typu *ref-Safe-to-Escape* (odkazem) následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="fd248-158">An lvalue designating a formal parameter is *ref-safe-to-escape* (by reference) as follows:</span></span>
- <span data-ttu-id="fd248-159">Pokud je parametr `ref`, `out`nebo `in` parametr, jedná se o *bezpečný* přístup z celé metody (např. pomocí příkazu `return ref`); případech</span><span class="sxs-lookup"><span data-stu-id="fd248-159">If the parameter is a `ref`, `out`, or `in` parameter, it is *ref-safe-to-escape* from the entire method (e.g. by a `return ref` statement); otherwise</span></span>
- <span data-ttu-id="fd248-160">Je-li parametr `this` parametrem struktury, jedná se o *bezpečný* přístup z oboru na nejvyšší úrovni metody (ale ne z celé samotné metody); [Ukázka](#struct-this-escape)</span><span class="sxs-lookup"><span data-stu-id="fd248-160">If the parameter is the `this` parameter of a struct type, it is *ref-safe-to-escape* to the top-level scope of the method (but not from the entire method itself); [Sample](#struct-this-escape)</span></span>
- <span data-ttu-id="fd248-161">V opačném případě parametr je parametr hodnoty a jedná se o *bezpečný přístup k* hlavnímu panelu na nejvyšší úrovni metody (ale ne z samotné metody).</span><span class="sxs-lookup"><span data-stu-id="fd248-161">Otherwise the parameter is a value parameter, and it is *ref-safe-to-escape* to the top-level scope of the method (but not from the method itself).</span></span>

<span data-ttu-id="fd248-162">Výraz, který je hodnotou rvalue určující použití formálního parametru, je z celé metody (například pomocí příkazu `return`) *zabezpečený řídicí* znak (podle hodnoty).</span><span class="sxs-lookup"><span data-stu-id="fd248-162">An expression that is an rvalue designating the use of a formal parameter is *safe-to-escape* (by value) from the entire method (e.g. by a `return` statement).</span></span> <span data-ttu-id="fd248-163">To platí i pro parametr `this`.</span><span class="sxs-lookup"><span data-stu-id="fd248-163">This applies to the `this` parameter as well.</span></span>

#### <a name="locals"></a><span data-ttu-id="fd248-164">Místní hodnoty</span><span class="sxs-lookup"><span data-stu-id="fd248-164">Locals</span></span>

<span data-ttu-id="fd248-165">L-hodnota označující místní proměnnou je *ref-Safe-to-Escape* (odkazem) následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="fd248-165">An lvalue designating a local variable is *ref-safe-to-escape* (by reference) as follows:</span></span>
- <span data-ttu-id="fd248-166">Pokud proměnná je `ref` proměnná, pak je její *bezpečný-na řídicí* znak z jeho inicializačního výrazu převedena na číslo, které je bezpečné – bez *odkazu* ; případech</span><span class="sxs-lookup"><span data-stu-id="fd248-166">If the variable is a `ref` variable, then its *ref-safe-to-escape* is taken from the *ref-safe-to-escape* of its initializing expression; otherwise</span></span>
- <span data-ttu-id="fd248-167">Proměnná je typově *bezpečná-to-Escape* oboru, ve kterém byla deklarována.</span><span class="sxs-lookup"><span data-stu-id="fd248-167">The variable is *ref-safe-to-escape* the scope in which it was declared.</span></span>

<span data-ttu-id="fd248-168">Výraz, který je hodnotou rvalue označující použití lokální proměnné, je *zabezpečený řídicím* znakem (podle hodnoty) následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="fd248-168">An expression that is an rvalue designating the use of a local variable is *safe-to-escape* (by value) as follows:</span></span>
- <span data-ttu-id="fd248-169">Ale obecné pravidlo, které je výše, místní, jehož typ není `ref struct` typ je *bezpečný – návrat* z celé nadřazené metody.</span><span class="sxs-lookup"><span data-stu-id="fd248-169">But the general rule above, a local whose type is not a `ref struct` type is *safe-to-return* from the entire enclosing method.</span></span>
- <span data-ttu-id="fd248-170">Pokud je proměnná proměnnou iterace `foreach` smyčky, je rozsah " *bezpečného* " ovládacího prvku proměnné stejný jako *bezpečný-řídící* znak výrazu `foreach` smyčky.</span><span class="sxs-lookup"><span data-stu-id="fd248-170">If the variable is an iteration variable of a `foreach` loop, then the variable's *safe-to-escape* scope is the same as the *safe-to-escape* of the `foreach` loop's expression.</span></span>
- <span data-ttu-id="fd248-171">Místní typ `ref struct` a neinicializované v bodě deklarace je *bezpečným návratem* z celé nadřazené metody.</span><span class="sxs-lookup"><span data-stu-id="fd248-171">A local of `ref struct` type and uninitialized at the point of declaration is *safe-to-return* from the entire enclosing method.</span></span>
- <span data-ttu-id="fd248-172">V opačném případě je typ proměnné `ref struct` typ a deklarace proměnné vyžaduje inicializátor.</span><span class="sxs-lookup"><span data-stu-id="fd248-172">Otherwise the variable's type is a `ref struct` type, and the variable's declaration requires an initializer.</span></span> <span data-ttu-id="fd248-173">Rozsah *bezpečného řídicího* panelu proměnné je stejný jako *bezpečný-řídicí* znak jeho inicializátoru.</span><span class="sxs-lookup"><span data-stu-id="fd248-173">The variable's *safe-to-escape* scope is the same as the *safe-to-escape* of its initializer.</span></span>

#### <a name="field-reference"></a><span data-ttu-id="fd248-174">Odkaz na pole</span><span class="sxs-lookup"><span data-stu-id="fd248-174">Field reference</span></span>

<span data-ttu-id="fd248-175">L-hodnota označující odkaz na pole, `e.F`, je *ref-Safe-to-Escape* (odkazem) následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="fd248-175">An lvalue designating a reference to a field, `e.F`, is *ref-safe-to-escape* (by reference) as follows:</span></span>
- <span data-ttu-id="fd248-176">Pokud je `e` typu odkazu, jedná se o *bezpečný přístup k znaku* z celé metody; případech</span><span class="sxs-lookup"><span data-stu-id="fd248-176">If `e` is of a reference type, it is *ref-safe-to-escape* from the entire method; otherwise</span></span>
- <span data-ttu-id="fd248-177">Pokud `e` je hodnotový typ, jeho *ref-to-* Escape je pořízeno z `e`typu " *bezpečného* " bez odkazů.</span><span class="sxs-lookup"><span data-stu-id="fd248-177">If `e` is of a value type, its *ref-safe-to-escape* is taken from the *ref-safe-to-escape* of `e`.</span></span>

<span data-ttu-id="fd248-178">Rvalue označující odkaz na pole, `e.F`, má rozsah *bezpečných uvozovacích znaků* , který je stejný jako *bezpečný-řídicí* znak `e`.</span><span class="sxs-lookup"><span data-stu-id="fd248-178">An rvalue designating a reference to a field, `e.F`, has a *safe-to-escape* scope that is the same as the *safe-to-escape* of `e`.</span></span>

#### <a name="operators-including-"></a><span data-ttu-id="fd248-179">Operátory, včetně `?:`</span><span class="sxs-lookup"><span data-stu-id="fd248-179">Operators including `?:`</span></span>

<span data-ttu-id="fd248-180">Aplikace uživatelsky definovaného operátoru je považována za vyvolání metody.</span><span class="sxs-lookup"><span data-stu-id="fd248-180">The application of a user-defined operator is treated as a method invocation.</span></span>

<span data-ttu-id="fd248-181">U operátoru, který má za následek rvalue, jako je například `e1 + e2` nebo `c ? e1 : e2`, je *bezpečného řídicího* panelu výsledku nejužším oborem v rámci *bezpečného řídicího* panelu operandů operátoru.</span><span class="sxs-lookup"><span data-stu-id="fd248-181">For an operator that yields an rvalue, such as `e1 + e2` or `c ? e1 : e2`, the *safe-to-escape* of the result is the narrowest scope among the *safe-to-escape* of the operands of the operator.</span></span>  <span data-ttu-id="fd248-182">Jako důsledek pro unární operátor, který poskytuje rvalue, jako je například `+e`, *bezpečné-řídicí znaky* výsledku je " *bezpečné* " řídicí znaky operandu.</span><span class="sxs-lookup"><span data-stu-id="fd248-182">As a consequence, for a unary operator that yields an rvalue, such as `+e`, the *safe-to-escape* of the result is the *safe-to-escape* of the operand.</span></span>

<span data-ttu-id="fd248-183">Pro operátor, který vrací hodnotu lvalue, například `c ? ref e1 : ref e2`</span><span class="sxs-lookup"><span data-stu-id="fd248-183">For an operator that yields an lvalue, such as `c ? ref e1 : ref e2`</span></span>
- <span data-ttu-id="fd248-184">*argument REF-to-Escape* výsledku je nejužším oborem v rámci typu ref- *to-Escape* operandů operátoru.</span><span class="sxs-lookup"><span data-stu-id="fd248-184">the *ref-safe-to-escape* of the result is the narrowest scope among the *ref-safe-to-escape* of the operands of the operator.</span></span>
- <span data-ttu-id="fd248-185">*bezpečné řídicí znaky* operandů musí souhlasit a to je *bezpečné-řídicí znaky* výsledné hodnoty lvalue.</span><span class="sxs-lookup"><span data-stu-id="fd248-185">the *safe-to-escape* of the operands must agree, and that is the *safe-to-escape* of the resulting lvalue.</span></span>

#### <a name="method-invocation"></a><span data-ttu-id="fd248-186">Vyvolání metody</span><span class="sxs-lookup"><span data-stu-id="fd248-186">Method invocation</span></span>

<span data-ttu-id="fd248-187">L-hodnota, která je výsledkem volání metody vracené odkazem, `e1.M(e2, ...)` je typově *bezpečná-to-Escape* nejnižší z následujících rozsahů:</span><span class="sxs-lookup"><span data-stu-id="fd248-187">An lvalue resulting from a ref-returning method invocation `e1.M(e2, ...)` is *ref-safe-to-escape* the smallest of the following scopes:</span></span>
- <span data-ttu-id="fd248-188">Celá ohraničující metoda</span><span class="sxs-lookup"><span data-stu-id="fd248-188">The entire enclosing method</span></span>
- <span data-ttu-id="fd248-189">*bezpečné-řídicí znaky* všech výrazů argumentu `ref` a `out` (kromě přijímače)</span><span class="sxs-lookup"><span data-stu-id="fd248-189">the *ref-safe-to-escape* of all `ref` and `out` argument expressions (excluding the receiver)</span></span>
- <span data-ttu-id="fd248-190">Pro každý parametr `in` metody, pokud je k dispozici odpovídající výraz, který je l-hodnota, jeho *ref-to-Escape*, jinak nejbližší nadřazený rozsah</span><span class="sxs-lookup"><span data-stu-id="fd248-190">For each `in` parameter of the method, if there is a corresponding expression that is an lvalue, its *ref-safe-to-escape*, otherwise the nearest enclosing scope</span></span>
- <span data-ttu-id="fd248-191">*bezpečné-řídicí znaky* všech výrazů argumentů (včetně přijímače)</span><span class="sxs-lookup"><span data-stu-id="fd248-191">the *safe-to-escape* of all argument expressions (including the receiver)</span></span>

> <span data-ttu-id="fd248-192">Poznámka: poslední odrážka je nutná ke zpracování kódu, jako např.</span><span class="sxs-lookup"><span data-stu-id="fd248-192">Note: the last bullet is necessary to handle code such as</span></span>
> ```csharp
> var sp = new Span(...)
> return ref sp[0];
> ```
> <span data-ttu-id="fd248-193">nebo</span><span class="sxs-lookup"><span data-stu-id="fd248-193">or</span></span>
> ```csharp
> return ref M(sp, 0);
> ```

<span data-ttu-id="fd248-194">Hodnota rvalue, která je výsledkem volání metody `e1.M(e2, ...)`, je *bezpečná-to-Escape* od nejmenšího z následujících oborů:</span><span class="sxs-lookup"><span data-stu-id="fd248-194">An rvalue resulting from a method invocation `e1.M(e2, ...)` is *safe-to-escape* from the smallest of the following scopes:</span></span>
- <span data-ttu-id="fd248-195">Celá ohraničující metoda</span><span class="sxs-lookup"><span data-stu-id="fd248-195">The entire enclosing method</span></span>
- <span data-ttu-id="fd248-196">*bezpečné-řídicí znaky* všech výrazů argumentů (včetně přijímače)</span><span class="sxs-lookup"><span data-stu-id="fd248-196">the *safe-to-escape* of all argument expressions (including the receiver)</span></span>

#### <a name="an-rvalue"></a><span data-ttu-id="fd248-197">Rvalue</span><span class="sxs-lookup"><span data-stu-id="fd248-197">An Rvalue</span></span>
<span data-ttu-id="fd248-198">Rvalue je z nejbližšího nadřazeného oboru *zabezpečený proti odkazům na řídicí znaky* .</span><span class="sxs-lookup"><span data-stu-id="fd248-198">An rvalue is *ref-safe-to-escape* from the nearest enclosing scope.</span></span> <span data-ttu-id="fd248-199">K tomu dochází například při vyvolání, například `M(ref d.Length)`, kde `d` je typu `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="fd248-199">This occurs for example in an invocation such as `M(ref d.Length)` where `d` is of type `dynamic`.</span></span> <span data-ttu-id="fd248-200">Je také konzistentní s (a případně subsumes) našeho zpracování argumentů odpovídajících parametrům `in`. \*</span><span class="sxs-lookup"><span data-stu-id="fd248-200">It is also consistent with (and perhaps subsumes) our handling of arguments corresponding to `in` parameters.\*</span></span>

#### <a name="property-invocations"></a><span data-ttu-id="fd248-201">Vyvolání vlastností</span><span class="sxs-lookup"><span data-stu-id="fd248-201">Property invocations</span></span>

<span data-ttu-id="fd248-202">Vyvolání vlastnosti (buď `get`, nebo `set`) se považuje za metody vyvolání podkladové metody výše uvedenými pravidly.</span><span class="sxs-lookup"><span data-stu-id="fd248-202">A property invocation (either `get` or `set`) it treated as a method invocation of the underlying method by the above rules.</span></span>

#### `stackalloc`

<span data-ttu-id="fd248-203">Výraz stackalloc je rvalue, který je *bezpečný-k úniku* do rozsahu nejvyšší úrovně metody (ale ne z celé samotné metody).</span><span class="sxs-lookup"><span data-stu-id="fd248-203">A stackalloc expression is an rvalue that is *safe-to-escape* to the top-level scope of the method (but not from the entire method itself).</span></span>

#### <a name="constructor-invocations"></a><span data-ttu-id="fd248-204">Vyvolání konstruktoru</span><span class="sxs-lookup"><span data-stu-id="fd248-204">Constructor invocations</span></span>

<span data-ttu-id="fd248-205">Výraz `new`, který vyvolá konstruktor, dodržuje stejná pravidla jako volání metody, která je považována za to, že vrací typ, který je vytvořen.</span><span class="sxs-lookup"><span data-stu-id="fd248-205">A `new` expression that invokes a constructor obeys the same rules as a method invocation that is considered to return the type being constructed.</span></span>

<span data-ttu-id="fd248-206">V případě *bezpečného* připojení není širší než nejmenší z *bezpečných řídicích znaků* všech argumentů a operandů výrazů inicializátoru objektu, rekurzivně, pokud je k dispozici inicializátor.</span><span class="sxs-lookup"><span data-stu-id="fd248-206">In addition *safe-to-escape* is no wider than the smallest of the *safe-to-escape* of all arguments/operands of the object initializer expressions, recursively, if initializer is present.</span></span> 

#### <a name="span-constructor"></a><span data-ttu-id="fd248-207">Span – konstruktor</span><span class="sxs-lookup"><span data-stu-id="fd248-207">Span constructor</span></span>
<span data-ttu-id="fd248-208">Jazyk spoléhá na `Span<T>` nemá konstruktor následujícího tvaru:</span><span class="sxs-lookup"><span data-stu-id="fd248-208">The language relies on `Span<T>` not having a constructor of the following form:</span></span>

```csharp
void Example(ref int x)
{
    // Create a span of length one
    var span = new Span<int>(ref x); 
}
```

<span data-ttu-id="fd248-209">Takový konstruktor zpřístupňuje `Span<T>`, které se používají jako pole Nerozlišená z pole `ref`.</span><span class="sxs-lookup"><span data-stu-id="fd248-209">Such a constructor makes `Span<T>` which are used as fields indistinguishable from a `ref` field.</span></span> <span data-ttu-id="fd248-210">Bezpečnostní pravidla popsaná v tomto dokumentu závisí na `ref` polích, která nejsou platnou konstrukcí v C#nebo .NET.</span><span class="sxs-lookup"><span data-stu-id="fd248-210">The safety rules described in this document depend on `ref` fields not being a valid construct in C#, or .NET.</span></span>

#### <a name="default-expressions"></a><span data-ttu-id="fd248-211">výrazy `default`</span><span class="sxs-lookup"><span data-stu-id="fd248-211">`default` expressions</span></span>

<span data-ttu-id="fd248-212">Výraz `default` je z celé nadřazené metody *bezpečný-na řídicí* znak.</span><span class="sxs-lookup"><span data-stu-id="fd248-212">A `default` expression is *safe-to-escape* from the entire enclosing method.</span></span>

## <a name="language-constraints"></a><span data-ttu-id="fd248-213">Jazyková omezení</span><span class="sxs-lookup"><span data-stu-id="fd248-213">Language Constraints</span></span>

<span data-ttu-id="fd248-214">Chceme zajistit, aby žádná `ref` místní proměnná a žádná proměnná typu `ref struct` neodkazovala na paměť zásobníku nebo proměnné, které už nejsou aktivní.</span><span class="sxs-lookup"><span data-stu-id="fd248-214">We wish to ensure that no `ref` local variable, and no variable of `ref struct` type, refers to stack memory or variables that are no longer alive.</span></span> <span data-ttu-id="fd248-215">Proto máme následující omezení jazyka:</span><span class="sxs-lookup"><span data-stu-id="fd248-215">We therefore have the following language constraints:</span></span>

- <span data-ttu-id="fd248-216">Ani parametr ref ani místní ani parametr ani parametr ani parametr ani lokální typ `ref struct` nelze převolat do lambda nebo místní funkce.</span><span class="sxs-lookup"><span data-stu-id="fd248-216">Neither a ref parameter, nor a ref local, nor a parameter or local of a `ref struct` type can be lifted into a lambda or local function.</span></span>

- <span data-ttu-id="fd248-217">Parametr ref ani parametr `ref struct` typu nemůže být argumentem metody iterátoru nebo metody `async`.</span><span class="sxs-lookup"><span data-stu-id="fd248-217">Neither a ref parameter nor a parameter of a `ref struct` type may be an argument on an iterator method or an `async` method.</span></span>

- <span data-ttu-id="fd248-218">Lokální odkaz ani místní typ `ref struct` nesmí být v rozsahu v místě příkazu `yield return` nebo výrazu `await`.</span><span class="sxs-lookup"><span data-stu-id="fd248-218">Neither a ref local, nor a local of a `ref struct` type may be in scope at the point of a `yield return` statement or an `await` expression.</span></span>

- <span data-ttu-id="fd248-219">Typ `ref struct` nesmí být použit jako argument typu nebo jako typ prvku v typu řazené kolekce členů.</span><span class="sxs-lookup"><span data-stu-id="fd248-219">A `ref struct` type may not be used as a type argument, or as an element type in a tuple type.</span></span>

- <span data-ttu-id="fd248-220">Typ `ref struct` nemůže být deklarovaným typem pole, s tím rozdílem, že se může jednat o deklarovaný typ pole instance jiného `ref struct`.</span><span class="sxs-lookup"><span data-stu-id="fd248-220">A `ref struct` type may not be the declared type of a field, except that it may be the declared type of an instance field of another `ref struct`.</span></span>

- <span data-ttu-id="fd248-221">Typ `ref struct` nesmí být typem elementu pole.</span><span class="sxs-lookup"><span data-stu-id="fd248-221">A `ref struct` type may not be the element type of an array.</span></span>

- <span data-ttu-id="fd248-222">Hodnota typu `ref struct` nesmí být zabalená:</span><span class="sxs-lookup"><span data-stu-id="fd248-222">A value of a `ref struct` type may not be boxed:</span></span>
  - <span data-ttu-id="fd248-223">Neexistuje žádný převod typu `ref struct` na `object` typu nebo na typ `System.ValueType`.</span><span class="sxs-lookup"><span data-stu-id="fd248-223">There is no conversion from a `ref struct` type to the type `object` or the type `System.ValueType`.</span></span>
  - <span data-ttu-id="fd248-224">Typ `ref struct` nemůže být deklarovaný pro implementaci libovolného rozhraní.</span><span class="sxs-lookup"><span data-stu-id="fd248-224">A `ref struct` type may not be declared to implement any interface</span></span>
  - <span data-ttu-id="fd248-225">Žádná metoda instance deklarovaná v `object` ani v `System.ValueType`, ale není přepsána v `ref struct` typu, může být volána s příjemcem daného `ref struct` typu.</span><span class="sxs-lookup"><span data-stu-id="fd248-225">No instance method declared in `object` or in `System.ValueType` but not overridden in a `ref struct` type may be called with a receiver of that `ref struct` type.</span></span>
  - <span data-ttu-id="fd248-226">Převodem metody na typ delegáta nemůže být zachycena žádná metoda instance typu `ref struct`.</span><span class="sxs-lookup"><span data-stu-id="fd248-226">No instance method of a `ref struct` type may be captured by method conversion to a delegate type.</span></span>

- <span data-ttu-id="fd248-227">Pro `ref e1 = ref e2`pro změnu přiřazení ref musí být `e2` s označením *ref-to-Escape* přinejmenším v rámci rozsahu, který je *bezpečný pro přístup k referenčnímu znaku* `e1`.</span><span class="sxs-lookup"><span data-stu-id="fd248-227">For a ref reassignment `ref e1 = ref e2`, the *ref-safe-to-escape* of `e2` must be at least as wide a scope as the *ref-safe-to-escape* of `e1`.</span></span>

- <span data-ttu-id="fd248-228">Pro příkaz ref Return `return ref e1`musí být `e1` typu *ref-to-Escape* z celé metody bezpečný na *řídicí* znak.</span><span class="sxs-lookup"><span data-stu-id="fd248-228">For a ref return statement `return ref e1`, the *ref-safe-to-escape* of `e1` must be *ref-safe-to-escape* from the entire method.</span></span> <span data-ttu-id="fd248-229">(TODO: budeme také potřebovat pravidlo, které `e1` musí být *bezpečné-k Escape* z celé metody, nebo jestli je redundantní?)</span><span class="sxs-lookup"><span data-stu-id="fd248-229">(TODO: Do we also need a rule that `e1` must be *safe-to-escape* from the entire method, or is that redundant?)</span></span>

- <span data-ttu-id="fd248-230">V případě návratového příkazu `return e1`*musí být* *bezpečná-to-Escape* `e1` z celé metody.</span><span class="sxs-lookup"><span data-stu-id="fd248-230">For a return statement `return e1`, the *safe-to-escape* of `e1` must be *safe-to-escape* from the entire method.</span></span>

- <span data-ttu-id="fd248-231">U `e1 = e2`přiřazení, pokud typ `e1` je `ref struct` typ, pak musí být *bezpečná-řídicí sekvence* `e2` aspoň jako síť s *bezpečným únikem* `e1`.</span><span class="sxs-lookup"><span data-stu-id="fd248-231">For an assignment `e1 = e2`, if the type of `e1` is a `ref struct` type, then the *safe-to-escape* of `e2` must be at least as wide a scope as the *safe-to-escape* of `e1`.</span></span>

- <span data-ttu-id="fd248-232">Pro vyvolání metody, pokud existuje argument `ref` nebo `out` `ref struct`ho typu (včetně přijímače) s *bezpečným řídicím* znakem E1, nemůže mít žádný argument (včetně přijímače) užší *bezpečný přístup k řídicímu* panelu než E1.</span><span class="sxs-lookup"><span data-stu-id="fd248-232">For a method invocation if there is a `ref` or `out` argument of a `ref struct` type (including the receiver), with *safe-to-escape* E1, then no argument (including the receiver) may have a narrower *safe-to-escape* than E1.</span></span> [<span data-ttu-id="fd248-233">Ukázka</span><span class="sxs-lookup"><span data-stu-id="fd248-233">Sample</span></span>](#method-arguments-must-match)

- <span data-ttu-id="fd248-234">Lokální funkce nebo anonymní funkce nesmí odkazovat na místní nebo parametr typu `ref struct` deklarovaného v ohraničujícím oboru.</span><span class="sxs-lookup"><span data-stu-id="fd248-234">A local function or anonymous function may not refer to a local or parameter of `ref struct` type declared in an enclosing scope.</span></span>

> <span data-ttu-id="fd248-235">***Otevřít problém:*** Potřebujeme nějaké pravidlo, které nám umožní vytvořit chybu, když se bude potřebovat překládat hodnoty zásobníku `ref struct`ho typu na výraz await, například v kódu.</span><span class="sxs-lookup"><span data-stu-id="fd248-235">***Open Issue:*** We need some rule that permits us to produce an error when needing to spill a stack value of a `ref struct` type at an await expression, for example in the code</span></span>
> ```csharp
> Foo(new Span<int>(...), await e2);
> ```

## <a name="explanations"></a><span data-ttu-id="fd248-236">Odůvodněn</span><span class="sxs-lookup"><span data-stu-id="fd248-236">Explanations</span></span>
<span data-ttu-id="fd248-237">Tato vysvětlení a ukázky vám pomůžou vysvětlit, proč existuje celá řada výše uvedených pravidel zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="fd248-237">These explanations and samples help explain why many of the safety rules above exist</span></span>

### <a name="method-arguments-must-match"></a><span data-ttu-id="fd248-238">Argumenty metody se musí shodovat.</span><span class="sxs-lookup"><span data-stu-id="fd248-238">Method Arguments Must Match</span></span>
<span data-ttu-id="fd248-239">Při vyvolání metody, kde je `out`, `ref` parametr, který je `ref struct`, včetně přijímače, musí mít všechny `ref struct` stejnou dobu života.</span><span class="sxs-lookup"><span data-stu-id="fd248-239">When invoking a method where there is an `out`, `ref` parameter that is a `ref struct` including the receiver then all of the `ref struct` need to have the same lifetime.</span></span> <span data-ttu-id="fd248-240">To je nezbytné, C# protože musí učinit všechna rozhodnutí o tom, že se jedná o bezpečnost životního cyklu na základě informací dostupných v signatuře metody a životnosti hodnot na webu volání.</span><span class="sxs-lookup"><span data-stu-id="fd248-240">This is necessary because C# must make all of it's decisions around lifetime safety based on the information available in the signature of the method and the lifetime of the values at the call site.</span></span> 

<span data-ttu-id="fd248-241">Pokud existují `ref` parametry, které jsou `ref struct` pak je reálnou možnost, že by mohly prohodit obsah.</span><span class="sxs-lookup"><span data-stu-id="fd248-241">When there are `ref` parameters that are `ref struct` then there is the possiblity they could swap around their contents.</span></span> <span data-ttu-id="fd248-242">Proto je na webu volání nutné zajistit, aby všechny tyto **případné** swapy byly kompatibilní.</span><span class="sxs-lookup"><span data-stu-id="fd248-242">Hence at the call site we must ensure all of these **potential** swaps are compatible.</span></span> <span data-ttu-id="fd248-243">Pokud jazyk vynutil, pak bude umožňovat špatný kód podobný následujícímu.</span><span class="sxs-lookup"><span data-stu-id="fd248-243">If the language didn't enforce that then it will allow for bad code like the following.</span></span>

```csharp
void M1(ref Span<int> s1)
{
    Span<int> s2 = stackalloc int[1];
    Swap(ref s1, ref s2);
}

void Swap(ref Span<int> x, ref int Span<int> y)
{
    // This will effectively assign the stackalloc to the s1 parameter and allow it
    // to escape to the caller of M1
    ref x = ref y; 
}
```

<span data-ttu-id="fd248-244">Omezení na přijímači je nezbytné, protože Přestože žádný z jeho obsahu není bezpečný pro přístup z více důvodů, může uložit zadané hodnoty.</span><span class="sxs-lookup"><span data-stu-id="fd248-244">The restriction on the receiver is necessary because while none of its contents are ref-safe-to-escape it can store the provided values.</span></span> <span data-ttu-id="fd248-245">To znamená, že se neshodnou životností můžete vytvořit bezpečnostní otvor typu následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="fd248-245">This means with mismatched lifetimes you could create a type safety hole in the following way:</span></span>

```csharp
ref struct S
{
    public Span<int> Span;

    public void Set(Span<int> span)
    {
        Span = span;
    }
}

void Broken(ref S s)
{
    Span<int> span = stackalloc int[1];

    // The result of a stackalloc is now stored in s.Span and escaped to the caller
    // of Broken
    s.Set(span); 
}
```

### <a name="struct-this-escape"></a><span data-ttu-id="fd248-246">Strukturovat tento řídicí znak</span><span class="sxs-lookup"><span data-stu-id="fd248-246">Struct This Escape</span></span>
<span data-ttu-id="fd248-247">Pokud je v rozsahu pravidla zabezpečení, hodnota `this` v členu instance je modelovaná jako parametr členu.</span><span class="sxs-lookup"><span data-stu-id="fd248-247">When it comes to span safety rules the `this` value in an instance member is modeled as a parameter to the member.</span></span> <span data-ttu-id="fd248-248">Nyní `struct` typ `this` je ve skutečnosti `ref S`, kde `class` je jednoduše `S` (pro členy `class / struct` s názvem S).</span><span class="sxs-lookup"><span data-stu-id="fd248-248">Now for a `struct` the type of `this` is actually `ref S` where in a `class` it's simply `S` (for members of a `class / struct` named S).</span></span> 

<span data-ttu-id="fd248-249">Zatím `this` má odlišná pravidla uvozovacích parametrů než jiné parametry `ref`.</span><span class="sxs-lookup"><span data-stu-id="fd248-249">Yet `this` has different escaping rules than other `ref` parameters.</span></span> <span data-ttu-id="fd248-250">Konkrétně se nejedná o bezpečný a řídicí znak, zatímco jiné parametry jsou:</span><span class="sxs-lookup"><span data-stu-id="fd248-250">Specifically it is not ref-safe-to-escape while other parameters are:</span></span>

```csharp
ref struct S
{ 
    int Field;

    // Illegal because this isn't safe to escape as ref
    ref int Get() => ref Field;

    // Legal
    ref int GetParam(ref int p) => ref p;
}
```

<span data-ttu-id="fd248-251">Důvodem pro toto omezení je fakt, že se při vyvolání `struct` členů něco udělat.</span><span class="sxs-lookup"><span data-stu-id="fd248-251">The reason for this restriction actually has little to do with `struct` member invocation.</span></span> <span data-ttu-id="fd248-252">Existují některá pravidla, která je potřeba odpracovat s ohledem na vyvolání členů v `struct`ch členech, u kterých je příjemce rvalue.</span><span class="sxs-lookup"><span data-stu-id="fd248-252">There are some rules that need to be worked out with respect to member invocation on `struct` members where the receiver is an rvalue.</span></span> <span data-ttu-id="fd248-253">Ale to je velmi obtížné.</span><span class="sxs-lookup"><span data-stu-id="fd248-253">But that is very approachable.</span></span> 

<span data-ttu-id="fd248-254">Důvod tohoto omezení je ve skutečnosti o vyvolání rozhraní.</span><span class="sxs-lookup"><span data-stu-id="fd248-254">The reason for this restriction is actually about interface invocation.</span></span> <span data-ttu-id="fd248-255">Konkrétně se jedná o to, zda by následující vzorek neměl nebo neměl být zkompilován;</span><span class="sxs-lookup"><span data-stu-id="fd248-255">Specifically it comes down to whether or not the following sample should or should not compile;</span></span>

```csharp
interface I1
{
    ref int Get();
}

ref int Use<T>(T p)
    where T : I1
{
    return ref p.Get();
}
```

<span data-ttu-id="fd248-256">Vezměte v úvahu případ, kdy je `T` vytvořena instance jako `struct`.</span><span class="sxs-lookup"><span data-stu-id="fd248-256">Consider the case where `T` is instantiated as a `struct`.</span></span> <span data-ttu-id="fd248-257">Pokud je parametr `this` zabezpečený proti odkazu, pak vrácení `p.Get` může ukazovat na zásobník (konkrétně se může jednat o pole v instanci typu `T`).</span><span class="sxs-lookup"><span data-stu-id="fd248-257">If the `this` parameter is ref-safe-to-escape then the return of `p.Get` could point to the stack (specifically it could be a field inside of the instantiated type of `T`).</span></span> <span data-ttu-id="fd248-258">To znamená, že jazyk nemůže způsobit, že by se tato ukázka mohla kompilovat, protože by mohla vracet `ref` do umístění zásobníku.</span><span class="sxs-lookup"><span data-stu-id="fd248-258">That means the language could not allow this sample to compile as it could be returning a `ref` to a stack location.</span></span> <span data-ttu-id="fd248-259">Na druhé straně, pokud `this` není bezpečné-pro-Escape a pak `p.Get` nemůže odkazovat na zásobník, a proto je bezpečné ho vrátit.</span><span class="sxs-lookup"><span data-stu-id="fd248-259">On the other hand if `this` is not ref-safe-to-escape then `p.Get` cannot refer to the stack and hence it's safe to return.</span></span> 

<span data-ttu-id="fd248-260">Důvodem je, že uvozovací znaky `this` v `struct` jsou opravdu všechny o rozhraních.</span><span class="sxs-lookup"><span data-stu-id="fd248-260">This is why the escapability of `this` in a `struct` is really all about interfaces.</span></span> <span data-ttu-id="fd248-261">Je možné, že je to pro práci naprosto, ale má na trhu volno.</span><span class="sxs-lookup"><span data-stu-id="fd248-261">It can absolutely be made to work but it has a trade off.</span></span> <span data-ttu-id="fd248-262">Návrh nakonec vychází z přístupnosti flexibilního vytváření rozhraní.</span><span class="sxs-lookup"><span data-stu-id="fd248-262">The design eventually came down in favor of making interfaces more flexible.</span></span> 

<span data-ttu-id="fd248-263">I když je to možné, můžeme to zmírnit i v budoucnu.</span><span class="sxs-lookup"><span data-stu-id="fd248-263">There is potential for us to relax this in the future though.</span></span> 

## <a name="future-considerations"></a><span data-ttu-id="fd248-264">Budoucí požadavky</span><span class="sxs-lookup"><span data-stu-id="fd248-264">Future Considerations</span></span>

### <a name="length-one-spant-over-ref-values"></a><span data-ttu-id="fd248-265">Délku jednoho rozsahu\<T > přes ref Values</span><span class="sxs-lookup"><span data-stu-id="fd248-265">Length one Span\<T> over ref values</span></span>
<span data-ttu-id="fd248-266">I když v současné době ještě není, existují případy, kdy by bylo možné vytvořit délku jedné instance `Span<T>` instance s hodnotou by měla být přínosná:</span><span class="sxs-lookup"><span data-stu-id="fd248-266">Though not legal today there are cases where creating a length one `Span<T>` instance over a value would be beneficial:</span></span>

```csharp
void RefExample()
{
    int x = ...;

    // Today creating a length one Span<int> requires a stackalloc and a new 
    // local
    Span<int> span1 = stackalloc [] { x };
    Use(span1);
    x = span1[0]; 

    // Simpler to just allow length one span
    var span2 = new Span<int>(ref x);
    Use(span2);
}
```

<span data-ttu-id="fd248-267">Tato funkce získá přísnější omezení, Pokud zavedeme omezení na [vyrovnávací paměti s pevnou velikostí](https://github.com/dotnet/csharplang/blob/master/proposals/fixed-sized-buffers.md) , protože by to mohlo způsobit `Span<T>` instancí ještě větší.</span><span class="sxs-lookup"><span data-stu-id="fd248-267">This feature gets more compelling if we lift the restrictions on [fixed sized buffers](https://github.com/dotnet/csharplang/blob/master/proposals/fixed-sized-buffers.md) as it would allow for `Span<T>` instances of even greater length.</span></span> 

<span data-ttu-id="fd248-268">Pokud je někdy potřeba přejít k této cestě, může to tento jazyk zajišťovat tím, že zajistí, že se tyto instance `Span<T>` jenom dolů.</span><span class="sxs-lookup"><span data-stu-id="fd248-268">If there is ever a need to go down this path then the language could accommodate this by ensuring such `Span<T>` instances were downward facing only.</span></span> <span data-ttu-id="fd248-269">To znamená, že se pouze někdy jednalo o *bezpečnou cestu* k oboru, ve kterém byly vytvořeny.</span><span class="sxs-lookup"><span data-stu-id="fd248-269">That is they were only ever *safe-to-escape* to the scope in which they were created.</span></span> <span data-ttu-id="fd248-270">To zajistí, že jazyk nikdy neměl považovat `ref` hodnotu uvozovací metodu pomocí `ref struct` návrat nebo pole `ref struct`.</span><span class="sxs-lookup"><span data-stu-id="fd248-270">This ensure the language never had to consider a `ref` value escaping a method via a `ref struct` return or field of `ref struct`.</span></span> <span data-ttu-id="fd248-271">To by mohlo také vyžadovat další změny pro rozpoznání takových konstruktorů, jako zachycení `ref` parametr tímto způsobem.</span><span class="sxs-lookup"><span data-stu-id="fd248-271">This would likely also require further changes to recognize such constructors as capturing a `ref` parameter in this way though.</span></span>
