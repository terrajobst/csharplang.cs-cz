---
ms.openlocfilehash: 76065293f652979ab395e131d657e44899c5a65b
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484550"
---
# <a name="simplified-null-argument-checking"></a><span data-ttu-id="42cf8-101">Zjednodušená kontrola argumentů null</span><span class="sxs-lookup"><span data-stu-id="42cf8-101">Simplified Null Argument Checking</span></span>

## <a name="summary"></a><span data-ttu-id="42cf8-102">Souhrn</span><span class="sxs-lookup"><span data-stu-id="42cf8-102">Summary</span></span>
<span data-ttu-id="42cf8-103">Tento návrh poskytuje zjednodušenou syntaxi pro ověřování argumentů metody, které nejsou `null` a jejich `ArgumentNullException` správně.</span><span class="sxs-lookup"><span data-stu-id="42cf8-103">This proposal provides a simplified syntax for validating method arguments are not `null` and throwing `ArgumentNullException` appropriately.</span></span>

## <a name="motivation"></a><span data-ttu-id="42cf8-104">Motivační</span><span class="sxs-lookup"><span data-stu-id="42cf8-104">Motivation</span></span>
<span data-ttu-id="42cf8-105">Práce na návrhu referenčních typů s možnou hodnotou null nám způsobila kontrole kódu potřebného pro `null` ověřování argumentu.</span><span class="sxs-lookup"><span data-stu-id="42cf8-105">The work on designing nullable reference types has caused us to examine the code necessary for `null` argument validation.</span></span> <span data-ttu-id="42cf8-106">Vzhledem k tomu, že NRT nemá vliv na vývojáře spouštění kódu, musí stále přidávat kód `if (arg is null) throw`ch desek, a to i v projektech, které jsou plně `null` čisté.</span><span class="sxs-lookup"><span data-stu-id="42cf8-106">Given that NRT doesn't affect code execution developers still must add `if (arg is null) throw` boiler plate code even in projects which are fully `null` clean.</span></span> <span data-ttu-id="42cf8-107">Díky tomu máme přání prozkoumat minimální syntaxi pro argument `null` ověřování v jazyce.</span><span class="sxs-lookup"><span data-stu-id="42cf8-107">This gave us the desire to explore a minimal syntax for argument `null` validation in the language.</span></span> 

<span data-ttu-id="42cf8-108">I když se očekává, že se tato syntaxe pro ověřování parametrů `null`a často spáruje s NRT, že návrh je zcela nezávislý.</span><span class="sxs-lookup"><span data-stu-id="42cf8-108">While this `null` parameter validation syntax is expected to pair frequently with NRT the proposal is fully independent of it.</span></span> <span data-ttu-id="42cf8-109">Syntaxi lze použít nezávisle na direktivách `#nullable`.</span><span class="sxs-lookup"><span data-stu-id="42cf8-109">The syntax can be used independent of `#nullable` directives.</span></span>

## <a name="detailed-design"></a><span data-ttu-id="42cf8-110">Podrobný návrh</span><span class="sxs-lookup"><span data-stu-id="42cf8-110">Detailed Design</span></span> 

### <a name="null-validation-parameter-syntax"></a><span data-ttu-id="42cf8-111">Syntaxe parametru ověřování null</span><span class="sxs-lookup"><span data-stu-id="42cf8-111">Null validation parameter syntax</span></span>
<span data-ttu-id="42cf8-112">Operátor vykřičník, `!`, může být umístěn za názvem parametru v seznamu parametrů a to způsobí, že C# kompilátor vygeneruje standardní kontrolu `null` kódu pro daný parametr.</span><span class="sxs-lookup"><span data-stu-id="42cf8-112">The bang operator, `!`, can be positioned after a parameter name in a parameter list and this will cause the C# compiler to emit standard `null` checking code for that parameter.</span></span> <span data-ttu-id="42cf8-113">Tento postup se označuje jako `null` Syntaxe ověřovacího parametru.</span><span class="sxs-lookup"><span data-stu-id="42cf8-113">This is referred to as `null` validation parameter syntax.</span></span> <span data-ttu-id="42cf8-114">Příklad:</span><span class="sxs-lookup"><span data-stu-id="42cf8-114">For example:</span></span>

``` csharp
void M(string name!) {
    ...
}
```

<span data-ttu-id="42cf8-115">Budou přeloženy do:</span><span class="sxs-lookup"><span data-stu-id="42cf8-115">Will be translated into:</span></span>

``` csharp
void M(string name) {
    if (name is null) {
        throw new ArgumentNullException(nameof(name));
    }
    ...
}
```

<span data-ttu-id="42cf8-116">Vygenerovaná `null` se objeví před jakýmkoli vývojářem vytvořeným kódem v metodě.</span><span class="sxs-lookup"><span data-stu-id="42cf8-116">The generated `null` check will occur before any developer authored code in the method.</span></span> <span data-ttu-id="42cf8-117">Pokud více parametrů obsahuje operátor `!`, budou kontroly provedeny ve stejném pořadí, ve kterém jsou deklarovány parametry.</span><span class="sxs-lookup"><span data-stu-id="42cf8-117">When multiple parameters contain the `!` operator then the checks will occur in the same order as the parameters are declared.</span></span>

``` csharp
void M(string p1, string p2) {
    if (p1 is null) {
        throw new ArgumentNullException(nameof(p1));
    }
    if (p2 is null) {
        throw new ArgumentNullException(nameof(p2));
    }
    ...
}
```

<span data-ttu-id="42cf8-118">Tato kontrolu bude určeno konkrétně pro referenční rovnost na `null`, nevyvolává `==` ani uživatelsky definované operátory.</span><span class="sxs-lookup"><span data-stu-id="42cf8-118">The check will be specifically for reference equality to `null`, it does not invoke `==` or any user defined operators.</span></span> <span data-ttu-id="42cf8-119">To také znamená, že operátor `!` lze přidat pouze k parametrům, jejichž typ lze testovat pro rovnost před `null`.</span><span class="sxs-lookup"><span data-stu-id="42cf8-119">This also means the `!` operator can only be added to parameters whose type can be tested for equality against `null`.</span></span> <span data-ttu-id="42cf8-120">To znamená, že se nedá použít u parametru, jehož typ je známý jako typ hodnoty.</span><span class="sxs-lookup"><span data-stu-id="42cf8-120">This means it can't be used on a parameter whose type is known to be a value type.</span></span>

``` csharp
// Error: Cannot use ! on parameters who types derive from System.ValueType
void G<T>(T arg!) where T : struct {

}
```

<span data-ttu-id="42cf8-121">V případě konstruktoru dojde k ověření `null` před jakýmkoli jiným kódem v konstruktoru.</span><span class="sxs-lookup"><span data-stu-id="42cf8-121">In the case of a constructor the `null` validation will occur before any other code in the constructor.</span></span> <span data-ttu-id="42cf8-122">To zahrnuje:</span><span class="sxs-lookup"><span data-stu-id="42cf8-122">That includes:</span></span> 

- <span data-ttu-id="42cf8-123">Řetězení k jiným konstruktorům pomocí `this` nebo `base`</span><span class="sxs-lookup"><span data-stu-id="42cf8-123">Chaining to other constructors with `this` or `base`</span></span> 
- <span data-ttu-id="42cf8-124">Inicializátory polí, které se implicitně vyskytují v konstruktoru</span><span class="sxs-lookup"><span data-stu-id="42cf8-124">Field initializers which implicitly occur in the constructor</span></span>

<span data-ttu-id="42cf8-125">Příklad:</span><span class="sxs-lookup"><span data-stu-id="42cf8-125">For example:</span></span>

``` csharp
class C {
    string field = GetString();
    C(string name!): this(name) {
        ...
    }
}
```

<span data-ttu-id="42cf8-126">Přibližně se převede na následující:</span><span class="sxs-lookup"><span data-stu-id="42cf8-126">Will be roughly translated into the following:</span></span>

``` csharp
class C {
    C(string name)
        if (name is null) {
            throw new ArgumentNullException(nameof(name));
        }
        field = GetString();
        :this(name);
        ...
}
```

<span data-ttu-id="42cf8-127">Poznámka: Toto není právní C# kód, ale stačí pouze aproximace toho, co implementace dělá.</span><span class="sxs-lookup"><span data-stu-id="42cf8-127">Note: this is not legal C# code but instead just an approximation of what the implementation does.</span></span> 

<span data-ttu-id="42cf8-128">Syntaxe parametru `null` ověření bude také platná v seznamech parametrů lambda.</span><span class="sxs-lookup"><span data-stu-id="42cf8-128">The `null` validation parameter syntax will also be valid on lambda parameter lists.</span></span> <span data-ttu-id="42cf8-129">To platí i v syntaxi jediného parametru, která postrádá Parens.</span><span class="sxs-lookup"><span data-stu-id="42cf8-129">This is valid even in the single parameter syntax that lacks parens.</span></span>

``` csharp
void G() {
    // An identity lambda which throws on a null input
    Func<string, string> s = x! => x;
}
```

<span data-ttu-id="42cf8-130">Syntaxe je také platná u parametrů pro metody iterátoru.</span><span class="sxs-lookup"><span data-stu-id="42cf8-130">The syntax is also valid on parameters to iterator methods.</span></span> <span data-ttu-id="42cf8-131">Na rozdíl od jiného kódu v iterátoru dojde k ověření `null`, když je vyvolána metoda iterátoru, ne při vás provedl základního výčtu.</span><span class="sxs-lookup"><span data-stu-id="42cf8-131">Unlike other code in the iterator the `null` validation will occur when the iterator method is invoked, not when the underlying enumerator is walked.</span></span> <span data-ttu-id="42cf8-132">To platí pro tradiční nebo `async` iterátory.</span><span class="sxs-lookup"><span data-stu-id="42cf8-132">This is true for traditional or `async` iterators.</span></span>

``` csharp
class Iterators {
    IEnumerable<char> GetCharacters(string s!) {
        foreach (var c in s) {
            yield return c;
        }
    }

    void Use() {
        // The invocation of GetCharacters will throw
        IEnumerable<char> e = GetCharacters(null);
    }
}
```

<span data-ttu-id="42cf8-133">Operátor `!` lze použít pouze pro seznamy parametrů, které mají související tělo metody.</span><span class="sxs-lookup"><span data-stu-id="42cf8-133">The `!` operator can only be used for parameter lists which have an associated method body.</span></span> <span data-ttu-id="42cf8-134">To znamená, že se nedá použít v `abstract` metodě, `interface`, `delegate` nebo `partial` definici metody.</span><span class="sxs-lookup"><span data-stu-id="42cf8-134">This means it cannot be used in an `abstract` method, `interface`, `delegate` or `partial` method definition.</span></span>

### <a name="extending-is-null"></a><span data-ttu-id="42cf8-135">Rozšíření má hodnotu null.</span><span class="sxs-lookup"><span data-stu-id="42cf8-135">Extending is null</span></span>
<span data-ttu-id="42cf8-136">Typy, pro které je výraz `is null` platný, budou rozšířeny tak, aby zahrnovaly neomezení parametrů typu.</span><span class="sxs-lookup"><span data-stu-id="42cf8-136">The types for which the expression `is null` is valid will be extended to include unconstrained type parameters.</span></span> <span data-ttu-id="42cf8-137">Tím umožníte, aby vyplnil záměr kontroly `null` u všech typů, které jsou kontrolní `null` platné.</span><span class="sxs-lookup"><span data-stu-id="42cf8-137">This will allow it to fill the intent of checking for `null` on all types which a `null` check is valid.</span></span> <span data-ttu-id="42cf8-138">Konkrétně to znamená, že typy, které nejsou jednoznačně známy typů hodnot.</span><span class="sxs-lookup"><span data-stu-id="42cf8-138">Specifically that is types which are not definitely known to be value types.</span></span> <span data-ttu-id="42cf8-139">Například parametry typu, které jsou omezeny na `struct` nelze použít s touto syntaxí.</span><span class="sxs-lookup"><span data-stu-id="42cf8-139">For example Type parameters which are constrained to `struct` cannot be used with this syntax.</span></span>

``` csharp
void NullCheck<T1, T2>(T1 p1, T2 p2) where T2 : struct {
    // Okay: T1 could be a class or struct here.
    if (p1 is null) {
        ...
    }

    // Error 
    if (p2 is null) { 
        ...
    }
}
```

<span data-ttu-id="42cf8-140">Chování `is null` u parametru typu bude stejné jako `== null` dnes.</span><span class="sxs-lookup"><span data-stu-id="42cf8-140">The behavior of `is null` on a type parameter will be the same as `== null` today.</span></span> <span data-ttu-id="42cf8-141">V případech, kdy je parametr typu vytvořen jako typ hodnoty, bude kód vyhodnocen jako `false`.</span><span class="sxs-lookup"><span data-stu-id="42cf8-141">In the cases where the type parameter is instantiated as a value type the code will be evaluated as `false`.</span></span> <span data-ttu-id="42cf8-142">Pro případy, kde je odkazový typ, bude kód provádět správnou `is null` kontrolu.</span><span class="sxs-lookup"><span data-stu-id="42cf8-142">For cases where it is a reference type the code will do a proper `is null` check.</span></span>

### <a name="intersection-with-nullable-reference-types"></a><span data-ttu-id="42cf8-143">Průnik s odkazy s možnou hodnotou null</span><span class="sxs-lookup"><span data-stu-id="42cf8-143">Intersection with Nullable Reference Types</span></span>
<span data-ttu-id="42cf8-144">Jakýkoli parametr, u kterého je použit operátor `!`, bude začínat ne`null`m stavu s možnou hodnotou null.</span><span class="sxs-lookup"><span data-stu-id="42cf8-144">Any parameter which has a `!` operator applied to it's name will start with the nullable state being not `null`.</span></span> <span data-ttu-id="42cf8-145">To platí i v případě, že typ samotného parametru je potenciálně `null`.</span><span class="sxs-lookup"><span data-stu-id="42cf8-145">This is true even if the type of the parameter itself is potentially `null`.</span></span> <span data-ttu-id="42cf8-146">K tomu může dojít s explicitním typem s možnou hodnotou null, například řekněme `string?`nebo s neomezeným parametrem typu.</span><span class="sxs-lookup"><span data-stu-id="42cf8-146">That can occur with an explicitly nullable type, such as say `string?`, or with an unconstrained type parameter.</span></span> 

<span data-ttu-id="42cf8-147">Pokud je syntaxe `!` v parametrech kombinována s typem s možnou hodnotou null pro parametr, bude kompilátor vydávat upozornění:</span><span class="sxs-lookup"><span data-stu-id="42cf8-147">When a `!` syntax on parameters is combined with an explicitly nullable type on the parameter then a warning will be issued by the compiler:</span></span>

``` csharp
void WarnCase<T>(
    string? name!, // Warning: combining explicit null checking with a nullable type
    T value1 // Okay
)
```

## <a name="open-issues"></a><span data-ttu-id="42cf8-148">Otevřené problémy</span><span class="sxs-lookup"><span data-stu-id="42cf8-148">Open Issues</span></span>
<span data-ttu-id="42cf8-149">Žádná</span><span class="sxs-lookup"><span data-stu-id="42cf8-149">None</span></span>

## <a name="considerations"></a><span data-ttu-id="42cf8-150">Požadavky</span><span class="sxs-lookup"><span data-stu-id="42cf8-150">Considerations</span></span>

### <a name="constructors"></a><span data-ttu-id="42cf8-151">Konstruktory</span><span class="sxs-lookup"><span data-stu-id="42cf8-151">Constructors</span></span>
<span data-ttu-id="42cf8-152">Generování kódu pro konstruktory znamená, že při přechodu ze standardního ověřování `null` dnes a `null` Syntaxe ověřovacího parametru (`!`) je k dispozici malá, ale nepozorovatelná změna chování.</span><span class="sxs-lookup"><span data-stu-id="42cf8-152">The code generation for constructors means there is a small, but observable, behavior change when moving from standard `null` validation today and the `null` validation parameter syntax (`!`).</span></span> <span data-ttu-id="42cf8-153">Ověřování na úrovni Standard `null` proběhne po inicializátorech polí a všech voláních `base` nebo `this`.</span><span class="sxs-lookup"><span data-stu-id="42cf8-153">The `null` check in standard validation occurs after both field initializers and any `base` or `this` calls.</span></span> <span data-ttu-id="42cf8-154">To znamená, že vývojář nemůže nutně migrovat 100% jejich `null` ověřování na novou syntaxi.</span><span class="sxs-lookup"><span data-stu-id="42cf8-154">This means a developer can't necessarily migrate 100% of their `null` validation to the new syntax.</span></span> <span data-ttu-id="42cf8-155">Konstruktory alespoň vyžadují určitou kontrolu.</span><span class="sxs-lookup"><span data-stu-id="42cf8-155">Constructors at least require some inspection.</span></span>

<span data-ttu-id="42cf8-156">Po diskuzi, i když bylo rozhodnuto, že to je velmi nepravděpodobné, že by došlo k významným problémům při přijímání.</span><span class="sxs-lookup"><span data-stu-id="42cf8-156">After discussion though it was decided that this is very unlikely to cause any significant adoption issues.</span></span> <span data-ttu-id="42cf8-157">Je více logické, že `null` spustit před jakoukoli logikou v konstruktoru.</span><span class="sxs-lookup"><span data-stu-id="42cf8-157">It's more logical that the `null` check run before any logic in the constructor does.</span></span> <span data-ttu-id="42cf8-158">Může znovu navštěvovat, pokud jsou zjištěny významné problémy s kompatibilitou.</span><span class="sxs-lookup"><span data-stu-id="42cf8-158">Can revisit if significant compat issues are discovered.</span></span>

### <a name="warning-when-mixing--and-"></a><span data-ttu-id="42cf8-159">Upozornění při smíchání?</span><span class="sxs-lookup"><span data-stu-id="42cf8-159">Warning when mixing ?</span></span> <span data-ttu-id="42cf8-160">ani!</span><span class="sxs-lookup"><span data-stu-id="42cf8-160">and !</span></span>
<span data-ttu-id="42cf8-161">Při použití syntaxe `!` na parametr, který je explicitně zadaný na typ s možnou hodnotou null, se musí zadat upozornění, zda by mělo být vydáno upozornění.</span><span class="sxs-lookup"><span data-stu-id="42cf8-161">There was a lengthy discussion on whether or not a warning should be issued when the `!` syntax is applied to a parameter which is explicitly typed to a nullable type.</span></span> <span data-ttu-id="42cf8-162">Na povrchu vypadá jako deklarace nesmyslná vývojářem, ale existují případy, kdy hierarchie typů můžou vynutit od vývojářů takovou situaci.</span><span class="sxs-lookup"><span data-stu-id="42cf8-162">On the surface it seems like a nonsensical declaration by the developer but there are cases where type hierarchies could force developers into such a situation.</span></span> 

<span data-ttu-id="42cf8-163">Vezměte v úvahu následující hierarchii tříd v rámci řady sestavení (za předpokladu, že všechny jsou kompilovány s povolenou kontrolou `null`):</span><span class="sxs-lookup"><span data-stu-id="42cf8-163">Consider the following class hierarchy across a series of assemblies (assuming all are compiled with `null` checking enabled):</span></span>

``` csharp
// Assembly1
abstract class C1 {
    protected abstract void M(object o); 
}

// Assembly2
abstract class C2 : C1 {

}

// Assembly3
abstract class C3 : C2 { 
    protected override void M(object o!) {
        ...
    }
}
```

<span data-ttu-id="42cf8-164">Tady se autor `C3` rozhodl přidat `null` ověření do parametru `o`.</span><span class="sxs-lookup"><span data-stu-id="42cf8-164">Here the author of `C3` decided to add `null` validation to the parameter `o`.</span></span> <span data-ttu-id="42cf8-165">To je zcela v souladu s tím, jak je funkce určena pro použití.</span><span class="sxs-lookup"><span data-stu-id="42cf8-165">This is completely in line with how the feature is intended to be used.</span></span>

<span data-ttu-id="42cf8-166">Nyní si představte, že se autor Assembly2 rozhodne přidat následující přepsání:</span><span class="sxs-lookup"><span data-stu-id="42cf8-166">Now imagine at a later date the author of Assembly2 decides to add the following override:</span></span>

``` csharp
// Assembly2
abstract class C2 : C1 {
   protected override void M(object? o) { 
       ...
   }
}
```

<span data-ttu-id="42cf8-167">To je povoleno pomocí typů odkazů s možnou hodnotou null, protože je právní, aby byl kontrakt flexibilnější pro vstupní pozice.</span><span class="sxs-lookup"><span data-stu-id="42cf8-167">This is allowed by nullable reference types as it's legal to make the contract more flexible for input positions.</span></span> <span data-ttu-id="42cf8-168">Funkce NRT obecně umožňuje rozumným spolu/kontravariancem u parametrů nebo vrácení hodnoty null.</span><span class="sxs-lookup"><span data-stu-id="42cf8-168">The NRT feature in general allows for reasonable co/contravariance on parameter / return nullability.</span></span> <span data-ttu-id="42cf8-169">Jazyk ale provede kontrolu souběžného/kontravariance na základě nejvíce konkrétního přepsání, nikoli původní deklarace.</span><span class="sxs-lookup"><span data-stu-id="42cf8-169">However the language does the co/contravariance checking based on the most specific override, not the original declaration.</span></span> <span data-ttu-id="42cf8-170">To znamená, že autor Assembly3 zobrazí upozornění týkající se typu `o` neshoduje a bude muset změnit signaturu na následující, aby se vyloučila:</span><span class="sxs-lookup"><span data-stu-id="42cf8-170">This means the author of Assembly3 will get a warning about the type of `o` not matching and will need to change the signature to the following to eliminate it:</span></span> 

``` csharp
// Assembly3
abstract class C3 : C2 { 
    protected override void M(object? o!) {
        ...
    }
}
```

<span data-ttu-id="42cf8-171">V tomto okamžiku má autor Assembly3 několik možností:</span><span class="sxs-lookup"><span data-stu-id="42cf8-171">At this point the author of Assembly3 has a few choices:</span></span>

- <span data-ttu-id="42cf8-172">Můžou přijmout nebo potlačit upozornění týkající se `object?` a `object` neshody.</span><span class="sxs-lookup"><span data-stu-id="42cf8-172">They can accept / suppress the warning about `object?` and `object` mismatch.</span></span>
- <span data-ttu-id="42cf8-173">Můžou přijmout nebo potlačit upozornění týkající se `object?` a `!` neshody.</span><span class="sxs-lookup"><span data-stu-id="42cf8-173">They can accept / suppress the warning about `object?` and `!` mismatch.</span></span>
- <span data-ttu-id="42cf8-174">Můžou jenom odebrat kontrolu ověřování `null` (odstranit `!` a provést explicitní kontrolu).</span><span class="sxs-lookup"><span data-stu-id="42cf8-174">They can just remove the `null` validation check (delete `!` and do explicit checking)</span></span>

<span data-ttu-id="42cf8-175">Jedná se o reálný scénář, ale je teď nápad, že se vám upozornění přesune vpřed.</span><span class="sxs-lookup"><span data-stu-id="42cf8-175">This is a real scenario but for now the idea is to move forward with the warning.</span></span> <span data-ttu-id="42cf8-176">Pokud se upozornění stane častěji, než jsme předpokládali, můžeme ho později odebrat (obráceně není pravda).</span><span class="sxs-lookup"><span data-stu-id="42cf8-176">If it turns out the warning happens more frequently than we anticipate then we can remove it later (the reverse is not true).</span></span>

### <a name="implicit-property-setter-arguments"></a><span data-ttu-id="42cf8-177">Argumenty setter pro implicitní vlastnost</span><span class="sxs-lookup"><span data-stu-id="42cf8-177">Implicit property setter arguments</span></span>
<span data-ttu-id="42cf8-178">Argument `value` parametru je implicitní a nezobrazuje se v žádném seznamu parametrů.</span><span class="sxs-lookup"><span data-stu-id="42cf8-178">The `value` argument of a parameter is implicit and does not appear in any parameter list.</span></span> <span data-ttu-id="42cf8-179">To znamená, že nemůže být cílem této funkce.</span><span class="sxs-lookup"><span data-stu-id="42cf8-179">That means it cannot be a target of this feature.</span></span> <span data-ttu-id="42cf8-180">Syntaxe setter vlastnosti se dá rozšířit tak, aby zahrnovala seznam parametrů, aby bylo možné použít operátor `!`.</span><span class="sxs-lookup"><span data-stu-id="42cf8-180">The property setter syntax could be extended to include a parameter list to allow the `!` operator to be applied.</span></span> <span data-ttu-id="42cf8-181">Ale to se podílí na nápadu této funkce, která usnadňuje `null` ověřování.</span><span class="sxs-lookup"><span data-stu-id="42cf8-181">But that cuts against the idea of this feature making `null` validation simpler.</span></span> <span data-ttu-id="42cf8-182">Vzhledem k tomu, že argument implicitní `value` nebude s touto funkcí fungovat.</span><span class="sxs-lookup"><span data-stu-id="42cf8-182">As such the implicit `value` argument just won't work with this feature.</span></span>

## <a name="future-considerations"></a><span data-ttu-id="42cf8-183">Budoucí požadavky</span><span class="sxs-lookup"><span data-stu-id="42cf8-183">Future Considerations</span></span>
<span data-ttu-id="42cf8-184">Žádná</span><span class="sxs-lookup"><span data-stu-id="42cf8-184">None</span></span>
