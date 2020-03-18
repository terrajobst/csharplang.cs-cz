---
ms.openlocfilehash: aecbf4298053debdb479ad3aaf6e3db468918455
ms.sourcegitcommit: 02b535d712cc9e022290e14d9e0324508b28f231
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/26/2020
ms.locfileid: "79485208"
---
# <a name="nullable-reference-types-specification"></a><span data-ttu-id="30126-101">Specifikace typů odkazů s možnou hodnotou null</span><span class="sxs-lookup"><span data-stu-id="30126-101">Nullable Reference Types Specification</span></span>

<span data-ttu-id="30126-102">Jedná se o probíhající práci – několik částí chybí nebo jsou neúplné.</span><span class="sxs-lookup"><span data-stu-id="30126-102">\*\*\* This is a work in progress - several parts are missing or incomplete.</span></span> ***

## <a name="syntax"></a><span data-ttu-id="30126-103">Syntaxe</span><span class="sxs-lookup"><span data-stu-id="30126-103">Syntax</span></span>

### <a name="nullable-reference-types"></a><span data-ttu-id="30126-104">Odkazové typy s možnou hodnotou null</span><span class="sxs-lookup"><span data-stu-id="30126-104">Nullable reference types</span></span>

<span data-ttu-id="30126-105">Typy odkazů s možnou hodnotou null mají stejnou syntaxi `T?` jako krátká forma typu hodnot s možnou hodnotou null, ale nemají odpovídající dlouhý tvar.</span><span class="sxs-lookup"><span data-stu-id="30126-105">Nullable reference types have the same syntax `T?` as the short form of nullable value types, but do not have a corresponding long form.</span></span>

<span data-ttu-id="30126-106">Pro účely specifikace je aktuální `nullable_type`ovaná výroba přejmenována na `nullable_value_type`a přidána `nullable_reference_type` produkce:</span><span class="sxs-lookup"><span data-stu-id="30126-106">For the purposes of the specification, the current `nullable_type` production is renamed to `nullable_value_type`, and a `nullable_reference_type` production is added:</span></span>

```antlr
reference_type
    : ...
    | nullable_reference_type
    ;
    
nullable_reference_type
    : non_nullable_reference_type '?'
    ;
    
non_nullable_reference_type
    : type
    ;
```

<span data-ttu-id="30126-107">`non_nullable_reference_type` v `nullable_reference_type` musí být odkazový typ, který nemůže mít hodnotu null (třída, rozhraní, delegát nebo pole), nebo parametr typu, který je omezený na odkaz typu, který neumožňuje hodnotu null (prostřednictvím omezení `class` nebo jiné třídy než `object`).</span><span class="sxs-lookup"><span data-stu-id="30126-107">The `non_nullable_reference_type` in a `nullable_reference_type` must be a non-nullable reference type (class, interface, delegate or array), or a type parameter that is constrained to be a non-nullable reference type (through the `class` constraint, or a class other than `object`).</span></span>

<span data-ttu-id="30126-108">Typy odkazů s možnou hodnotou null se nemůžou vyskytovat v následujících umístěních:</span><span class="sxs-lookup"><span data-stu-id="30126-108">Nullable reference types cannot occur in the following positions:</span></span>

- <span data-ttu-id="30126-109">jako základní třída nebo rozhraní</span><span class="sxs-lookup"><span data-stu-id="30126-109">as a base class or interface</span></span>
- <span data-ttu-id="30126-110">jako příjemce `member_access`</span><span class="sxs-lookup"><span data-stu-id="30126-110">as the receiver of a `member_access`</span></span>
- <span data-ttu-id="30126-111">jako `type` v `object_creation_expression`</span><span class="sxs-lookup"><span data-stu-id="30126-111">as the `type` in an `object_creation_expression`</span></span>
- <span data-ttu-id="30126-112">jako `delegate_type` v `delegate_creation_expression`</span><span class="sxs-lookup"><span data-stu-id="30126-112">as the `delegate_type` in a `delegate_creation_expression`</span></span>
- <span data-ttu-id="30126-113">jako `type` v `is_expression`, `catch_clause` nebo `type_pattern`</span><span class="sxs-lookup"><span data-stu-id="30126-113">as the `type` in an `is_expression`, a `catch_clause` or a `type_pattern`</span></span>
- <span data-ttu-id="30126-114">jako `interface` v plně kvalifikovaném názvu člena rozhraní</span><span class="sxs-lookup"><span data-stu-id="30126-114">as the `interface` in a fully qualified interface member name</span></span>

<span data-ttu-id="30126-115">U `nullable_reference_type`, kde je kontext anotace s možnou hodnotou null zakázán, se zobrazí upozornění.</span><span class="sxs-lookup"><span data-stu-id="30126-115">A warning is given on a `nullable_reference_type` where the nullable annotation context is disabled.</span></span>

### <a name="nullable-class-constraint"></a><span data-ttu-id="30126-116">Omezení třídy s možnou hodnotou null</span><span class="sxs-lookup"><span data-stu-id="30126-116">Nullable class constraint</span></span>

<span data-ttu-id="30126-117">Omezení `class` má `class?`protějšek s možnou hodnotou null:</span><span class="sxs-lookup"><span data-stu-id="30126-117">The `class` constraint has a nullable counterpart `class?`:</span></span>

```antlr
primary_constraint
    : ...
    | 'class' '?'
    ;
```

### <a name="the-null-forgiving-operator"></a><span data-ttu-id="30126-118">Operátor null-striktní</span><span class="sxs-lookup"><span data-stu-id="30126-118">The null-forgiving operator</span></span>

<span data-ttu-id="30126-119">Operátor po opravě `!` se nazývá operátor null-striktní.</span><span class="sxs-lookup"><span data-stu-id="30126-119">The post-fix `!` operator is called the null-forgiving operator.</span></span>

```antlr
primary_expression
    : ...
    | null_forgiving_expression
    ;
    
null_forgiving_expression
    : primary_expression '!'
    ;
```

<span data-ttu-id="30126-120">`primary_expression` musí být typu odkazu.</span><span class="sxs-lookup"><span data-stu-id="30126-120">The `primary_expression` must be of a reference type.</span></span>  

<span data-ttu-id="30126-121">Operátor `!` přípon nemá žádný běhový efekt – vyhodnotí výsledek podkladového výrazu.</span><span class="sxs-lookup"><span data-stu-id="30126-121">The postfix `!` operator has no runtime effect - it evaluates to the result of the underlying expression.</span></span> <span data-ttu-id="30126-122">Jeho jedinou rolí je změnit stav null výrazu a omezit upozornění na základě jeho použití.</span><span class="sxs-lookup"><span data-stu-id="30126-122">Its only role is to change the null state of the expression, and to limit warnings given on its use.</span></span>

### <a name="nullable-implicitly-typed-local-variables"></a><span data-ttu-id="30126-123">implicitně typované lokální proměnné s možnou hodnotou null</span><span class="sxs-lookup"><span data-stu-id="30126-123">nullable implicitly typed local variables</span></span>

<span data-ttu-id="30126-124">`var` odvodí typ s poznámkami pro typy odkazů.</span><span class="sxs-lookup"><span data-stu-id="30126-124">`var` infers an annotated type for reference types.</span></span>
<span data-ttu-id="30126-125">Například v `var s = "";` `var` je odvozen jako `string?`.</span><span class="sxs-lookup"><span data-stu-id="30126-125">For instance, in `var s = "";` the `var` is inferred as `string?`.</span></span>

### <a name="nullable-compiler-directives"></a><span data-ttu-id="30126-126">Nullable – direktivy kompilátoru</span><span class="sxs-lookup"><span data-stu-id="30126-126">Nullable compiler directives</span></span>

<span data-ttu-id="30126-127">direktivy `#nullable` určují kontexty anotace a upozornění s možnou hodnotou null.</span><span class="sxs-lookup"><span data-stu-id="30126-127">`#nullable` directives control the nullable annotation and warning contexts.</span></span>

```antlr
pp_directive
    : ...
    | pp_nullable
    ;
    
pp_nullable
    : whitespace? '#' whitespace? 'nullable' whitespace nullable_action pp_new_line
    ;
    
nullable_action
    : 'disable'
    | 'enable'
    | 'restore'
    ;
```

<span data-ttu-id="30126-128">direktivy `#pragma warning` jsou rozbalené, aby bylo možné změnit kontext varování s možnou hodnotou null a povolit, aby byla jednotlivá upozornění zapnutá, i když jsou ve výchozím nastavení zakázaná:</span><span class="sxs-lookup"><span data-stu-id="30126-128">`#pragma warning` directives are expanded to allow changing the nullable warning context, and to allow individual warnings to be enabled on even when they're disabled by default:</span></span>

```antlr
pragma_warning_body
    : ...
    | 'warning' whitespace nullable_action whitespace 'nullable'
    ;

warning_action
    : ...
    | 'enable'
    ;
```

<span data-ttu-id="30126-129">Všimněte si, že nová forma `pragma_warning_body` používá `nullable_action`, nikoli `warning_action`.</span><span class="sxs-lookup"><span data-stu-id="30126-129">Note that the new form of `pragma_warning_body` uses `nullable_action`, not `warning_action`.</span></span>

## <a name="nullable-contexts"></a><span data-ttu-id="30126-130">Kontexty s možnou hodnotou null</span><span class="sxs-lookup"><span data-stu-id="30126-130">Nullable contexts</span></span>

<span data-ttu-id="30126-131">Každý řádek zdrojového kódu má *kontext anotace s možnou hodnotou null* a *výstražný kontext s možnou hodnotou null*.</span><span class="sxs-lookup"><span data-stu-id="30126-131">Every line of source code has a *nullable annotation context* and a *nullable warning context*.</span></span> <span data-ttu-id="30126-132">Tyto ovládací prvky mají vliv na to, jestli jsou anotace s možnou hodnotou null, a zda jsou zadány upozornění</span><span class="sxs-lookup"><span data-stu-id="30126-132">These control whether nullable annotations have effect, and whether nullability warnings are given.</span></span> <span data-ttu-id="30126-133">Kontext poznámky daného řádku je buď *zakázán* , nebo *povolen*.</span><span class="sxs-lookup"><span data-stu-id="30126-133">The annotation context of a given line is either *disabled* or *enabled*.</span></span> <span data-ttu-id="30126-134">Kontext varování daného řádku je buď *zakázán* , nebo *povolen*.</span><span class="sxs-lookup"><span data-stu-id="30126-134">The warning context of a given line is either *disabled* or *enabled*.</span></span>

<span data-ttu-id="30126-135">Oba kontexty lze zadat na úrovni projektu (mimo C# zdrojový kód) nebo kdekoli v rámci zdrojového souboru prostřednictvím `#nullable` direktiv před procesorem.</span><span class="sxs-lookup"><span data-stu-id="30126-135">Both contexts can be specified at the project level (outside of C# source code), or anywhere within a source file via `#nullable` pre-processor directives.</span></span> <span data-ttu-id="30126-136">Pokud nejsou k dispozici žádná nastavení na úrovni projektu, je výchozí nastavení pro oba kontexty *zakázáno*.</span><span class="sxs-lookup"><span data-stu-id="30126-136">If no project level settings are provided the default is for both contexts to be *disabled*.</span></span>

<span data-ttu-id="30126-137">Direktiva `#nullable` řídí kontexty poznámek a upozornění v rámci zdrojového textu a má přednost před nastavením na úrovni projektu.</span><span class="sxs-lookup"><span data-stu-id="30126-137">The `#nullable` directive controls the annotation and warning contexts within the source text, and take precedence over the project-level settings.</span></span>

<span data-ttu-id="30126-138">Direktiva nastavuje kontexty, které ovládací prvky řídí pro následné řádky kódu, dokud ji nepřepisuje jiná direktiva nebo dokud nekončí konec zdrojového souboru.</span><span class="sxs-lookup"><span data-stu-id="30126-138">A directive sets the context(s) it controls for subsequent lines of code, until another directive overrides it, or until the end of the source file.</span></span>

<span data-ttu-id="30126-139">Účinek direktiv je následující:</span><span class="sxs-lookup"><span data-stu-id="30126-139">The effect of the directives is as follows:</span></span>

- <span data-ttu-id="30126-140">`#nullable disable`: nastavuje anotaci s možnou hodnotou null a kontexty upozornění na *zakázáno* .</span><span class="sxs-lookup"><span data-stu-id="30126-140">`#nullable disable`: Sets the nullable annotation and warning contexts to *disabled*</span></span>
- <span data-ttu-id="30126-141">`#nullable enable`: nastaví pro *povolenou* anotaci a kontexty upozornění s možnou hodnotou null.</span><span class="sxs-lookup"><span data-stu-id="30126-141">`#nullable enable`: Sets the nullable annotation and warning contexts to *enabled*</span></span>
- <span data-ttu-id="30126-142">`#nullable restore`: obnoví nastavení projektu s možnou anotací a kontexty upozornění.</span><span class="sxs-lookup"><span data-stu-id="30126-142">`#nullable restore`: Restores the nullable annotation and warning contexts to project settings</span></span>
- <span data-ttu-id="30126-143">`#nullable disable annotations`: nastaví kontext anotace s možnou hodnotou null na *disabled* .</span><span class="sxs-lookup"><span data-stu-id="30126-143">`#nullable disable annotations`: Sets the nullable annotation context to *disabled*</span></span>
- <span data-ttu-id="30126-144">`#nullable enable annotations`: nastaví kontext anotace s možnou hodnotou null na *povoleno* .</span><span class="sxs-lookup"><span data-stu-id="30126-144">`#nullable enable annotations`: Sets the nullable annotation context to *enabled*</span></span>
- <span data-ttu-id="30126-145">`#nullable restore annotations`: obnoví kontext anotace s možnou hodnotou null na nastavení projektu.</span><span class="sxs-lookup"><span data-stu-id="30126-145">`#nullable restore annotations`: Restores the nullable annotation context to project settings</span></span>
- <span data-ttu-id="30126-146">`#nullable disable warnings`: nastaví výstražný kontext s možnou hodnotou null na *disabled* .</span><span class="sxs-lookup"><span data-stu-id="30126-146">`#nullable disable warnings`: Sets the nullable warning context to *disabled*</span></span>
- <span data-ttu-id="30126-147">`#nullable enable warnings`: nastaví výstražný kontext s možnou hodnotou null na *povoleno* .</span><span class="sxs-lookup"><span data-stu-id="30126-147">`#nullable enable warnings`: Sets the nullable warning context to *enabled*</span></span>
- <span data-ttu-id="30126-148">`#nullable restore warnings`: obnoví kontext upozornění s možnou hodnotou null na nastavení projektu.</span><span class="sxs-lookup"><span data-stu-id="30126-148">`#nullable restore warnings`: Restores the nullable warning context to project settings</span></span>

## <a name="nullability-of-types"></a><span data-ttu-id="30126-149">Hodnota null typů</span><span class="sxs-lookup"><span data-stu-id="30126-149">Nullability of types</span></span>

<span data-ttu-id="30126-150">Daný typ může mít jednu ze čtyř nullabilities: *oblivious*, která není *null*, *Nullable* a *Unknown*.</span><span class="sxs-lookup"><span data-stu-id="30126-150">A given type can have one of four nullabilities: *Oblivious*, *nonnullable*, *nullable* and *unknown*.</span></span> 

<span data-ttu-id="30126-151">*Nenullelné* a *neznámé* typy mohou způsobit upozornění, pokud je jim přiřazena potenciální `null` hodnota.</span><span class="sxs-lookup"><span data-stu-id="30126-151">*Nonnullable* and *unknown* types may cause warnings if a potential `null` value is assigned to them.</span></span> <span data-ttu-id="30126-152">*Oblivious* a typy s *možnou hodnotou null* jsou ale "*null" přiřazovatelné*a můžou mít k nim přiřazené `null` hodnoty bez upozornění.</span><span class="sxs-lookup"><span data-stu-id="30126-152">*Oblivious* and *nullable* types, however, are "*null-assignable*" and can have `null` values assigned to them without warnings.</span></span> 

<span data-ttu-id="30126-153">*Oblivious* a *nenulové* typy lze odkázat nebo přiřadit bez upozornění.</span><span class="sxs-lookup"><span data-stu-id="30126-153">*Oblivious* and *nonnullable* types can be dereferenced or assigned without warnings.</span></span> <span data-ttu-id="30126-154">Hodnoty s *možnou hodnotou null* a *neznámými* typy jsou však "*nenáročné na hodnotu null*" a mohou způsobit upozornění při přečtení nebo přiřazení bez správné kontroly hodnoty null.</span><span class="sxs-lookup"><span data-stu-id="30126-154">Values of *nullable* and *unknown* types, however, are "*null-yielding*" and may cause warnings when dereferenced or assigned without proper null checking.</span></span> 

<span data-ttu-id="30126-155">*Výchozí stav null* typu pro získávání hodnoty null je "možná null".</span><span class="sxs-lookup"><span data-stu-id="30126-155">The *default null state* of a null-yielding type is "maybe null".</span></span> <span data-ttu-id="30126-156">Výchozí stav null typu nenulových hodnot, který není null, je "NOT NULL".</span><span class="sxs-lookup"><span data-stu-id="30126-156">The default null state of a non-null-yielding type is "not null".</span></span>

<span data-ttu-id="30126-157">Typ typu a kontext anotace s možnou hodnotou null se vyskytuje v určení jeho hodnoty null:</span><span class="sxs-lookup"><span data-stu-id="30126-157">The kind of type and the nullable annotation context it occurs in determine its nullability:</span></span>

- <span data-ttu-id="30126-158">`S` typ hodnoty, který není null, je vždycky *nenull* .</span><span class="sxs-lookup"><span data-stu-id="30126-158">A nonnullable value type `S` is always *nonnullable*</span></span>
- <span data-ttu-id="30126-159">Typ hodnoty s možnou hodnotou null `S?` je vždycky *null* .</span><span class="sxs-lookup"><span data-stu-id="30126-159">A nullable value type `S?` is always *nullable*</span></span>
- <span data-ttu-id="30126-160">Neoznačený odkazový typ `C` v kontextu *zakázané* poznámky je *oblivious*</span><span class="sxs-lookup"><span data-stu-id="30126-160">An unannotated reference type `C` in a *disabled* annotation context is *oblivious*</span></span>
- <span data-ttu-id="30126-161">Typ odkazu, který není označený `C` v kontextu *povolené* poznámky, není *null* .</span><span class="sxs-lookup"><span data-stu-id="30126-161">An unannotated reference type `C` in an *enabled* annotation context is *nonnullable*</span></span>
- <span data-ttu-id="30126-162">Typ odkazu s možnou hodnotou null `C?` může *mít hodnotu null* (může se ale v kontextu *zakázané* poznámky vracet).</span><span class="sxs-lookup"><span data-stu-id="30126-162">A nullable reference type `C?` is *nullable* (but a warning may be yielded in a *disabled* annotation context)</span></span>

<span data-ttu-id="30126-163">Parametry typu navíc berou v úvahu svá omezení:</span><span class="sxs-lookup"><span data-stu-id="30126-163">Type parameters additionally take their constraints into account:</span></span>

- <span data-ttu-id="30126-164">Parametr typu `T`, kde všechna omezení (pokud existují) mají buď typy s*možnou hodnotou* null (Nullable a *Unknown*), nebo je omezení `class?` *neznámé* .</span><span class="sxs-lookup"><span data-stu-id="30126-164">A type parameter `T` where all constraints (if any) are either null-yielding types (*nullable* and *unknown*) or the `class?` constraint is *unknown*</span></span>
- <span data-ttu-id="30126-165">Parametr typu `T`, kde nejméně jedno omezení je buď *oblivious* , nebo není *null* , nebo jedno z `struct` nebo omezení `class` je</span><span class="sxs-lookup"><span data-stu-id="30126-165">A type parameter `T` where at least one constraint is either *oblivious* or *nonnullable* or one of the `struct` or `class` constraints is</span></span>
    - <span data-ttu-id="30126-166">*oblivious* v kontextu *zakázané* poznámky</span><span class="sxs-lookup"><span data-stu-id="30126-166">*oblivious* in a *disabled* annotation context</span></span>
    - <span data-ttu-id="30126-167">v *povoleném* kontextu anotace není možné *mít hodnotu null* .</span><span class="sxs-lookup"><span data-stu-id="30126-167">*nonnullable* in an *enabled* annotation context</span></span>
- <span data-ttu-id="30126-168">Parametr typu s možnou hodnotou null `T?`, kde nejméně jedno omezení `T`je *oblivious* nebo není *null* nebo jedno z `struct` nebo `class` omezení je</span><span class="sxs-lookup"><span data-stu-id="30126-168">A nullable type parameter `T?` where at least one of `T`'s constraints is *oblivious* or *nonnullable* or one of the `struct` or `class` constraints, is</span></span>
    - <span data-ttu-id="30126-169">hodnota *null* v kontextu *zakázané* poznámky (ale je výsledkem upozornění)</span><span class="sxs-lookup"><span data-stu-id="30126-169">*nullable* in a *disabled* annotation context (but a warning is yielded)</span></span>
    - <span data-ttu-id="30126-170">*Nullable* v *povoleném* kontextu anotace</span><span class="sxs-lookup"><span data-stu-id="30126-170">*nullable* in an *enabled* annotation context</span></span>

<span data-ttu-id="30126-171">Pro parametr typu `T`je `T?` povoleno pouze v případě, že je `T` znám jako typ hodnoty nebo se jedná o typ odkazu.</span><span class="sxs-lookup"><span data-stu-id="30126-171">For a type parameter `T`, `T?` is only allowed if `T` is known to be a value type or known to be a reference type.</span></span>

### <a name="oblivious-vs-nonnullable"></a><span data-ttu-id="30126-172">Oblivious vs – nemožnost null</span><span class="sxs-lookup"><span data-stu-id="30126-172">Oblivious vs nonnullable</span></span>

<span data-ttu-id="30126-173">`type` se považuje za výskyt v daném kontextu poznámky, pokud je poslední token typu v rámci daného kontextu.</span><span class="sxs-lookup"><span data-stu-id="30126-173">A `type` is deemed to occur in a given annotation context when the last token of the type is within that context.</span></span>

<span data-ttu-id="30126-174">Zda daný typ odkazu `C` ve zdrojovém kódu je interpretován jako oblivious nebo není null, závisí na kontextu poznámky tohoto zdrojového kódu.</span><span class="sxs-lookup"><span data-stu-id="30126-174">Whether a given reference type `C` in source code is interpreted as oblivious or nonnullable depends on the annotation context of that source code.</span></span> <span data-ttu-id="30126-175">Po navázání se však považuje za součást tohoto typu a "při jejich" přenosu ", např. při nahrazování argumentů obecného typu.</span><span class="sxs-lookup"><span data-stu-id="30126-175">But once established, it is considered part of that type, and "travels with it" e.g. during substitution of generic type arguments.</span></span> <span data-ttu-id="30126-176">Je to tak, jak je Poznámka jako `?` typu, ale neviditelná.</span><span class="sxs-lookup"><span data-stu-id="30126-176">It is as if there is an annotation like `?` on the type, but invisible.</span></span>

## <a name="constraints"></a><span data-ttu-id="30126-177">Omezení</span><span class="sxs-lookup"><span data-stu-id="30126-177">Constraints</span></span>

<span data-ttu-id="30126-178">Typy odkazů s možnou hodnotou null lze použít jako obecná omezení.</span><span class="sxs-lookup"><span data-stu-id="30126-178">Nullable reference types can be used as generic constraints.</span></span> <span data-ttu-id="30126-179">Kromě toho `object` nyní platí jako explicitní omezení.</span><span class="sxs-lookup"><span data-stu-id="30126-179">Furthermore `object` is now valid as an explicit constraint.</span></span> <span data-ttu-id="30126-180">Neexistence omezení je nyní ekvivalentní omezení `object?` (namísto `object`), ale (na rozdíl od `object` před) `object?` není zakázáno jako explicitní omezení.</span><span class="sxs-lookup"><span data-stu-id="30126-180">Absence of a constraint is now equivalent to an `object?` constraint (instead of `object`), but (unlike `object` before) `object?` is not prohibited as an explicit constraint.</span></span>

<span data-ttu-id="30126-181">`class?` je nové omezení označující "možný typ odkazu s možnou hodnotou null", zatímco `class` označuje "typ odkazu, který neumožňuje hodnotu null".</span><span class="sxs-lookup"><span data-stu-id="30126-181">`class?` is a new constraint denoting "possibly nullable reference type", whereas `class` denotes "nonnullable reference type".</span></span>

<span data-ttu-id="30126-182">Hodnota null argumentu typu nebo omezení nemá vliv na to, jestli typ splňuje omezení, s výjimkou případů, kdy to již dnes nevyhovuje omezení `struct`.</span><span class="sxs-lookup"><span data-stu-id="30126-182">The nullability of a type argument or of a constraint does not impact whether the type satisfies the constraint, except where that is already the case today (nullable value types do not satisfy the `struct` constraint).</span></span> <span data-ttu-id="30126-183">Pokud však argument typu nesplňuje požadavky na hodnotu NULL omezení, může být zadáno upozornění.</span><span class="sxs-lookup"><span data-stu-id="30126-183">However, if the type argument does not satisfy the nullability requirements of the constraint, a warning may be given.</span></span>

## <a name="null-state-and-null-tracking"></a><span data-ttu-id="30126-184">Nulový stav a sledování hodnoty null</span><span class="sxs-lookup"><span data-stu-id="30126-184">Null state and null tracking</span></span>

<span data-ttu-id="30126-185">Každý výraz v daném zdrojovém umístění má *stav null*, který označuje, zda se má potenciálně vyhodnotit jako hodnota null.</span><span class="sxs-lookup"><span data-stu-id="30126-185">Every expression in a given source location has a *null state*, which indicated whether it is believed to potentially evaluate to null.</span></span> <span data-ttu-id="30126-186">Stav null je buď "NOT NULL" nebo "pravděpodobně null".</span><span class="sxs-lookup"><span data-stu-id="30126-186">The null state is either "not null" or "maybe null".</span></span> <span data-ttu-id="30126-187">Stav null se používá k určení, zda by mělo být zadáno upozornění na hodnotu null – nezabezpečené převody a odkazy.</span><span class="sxs-lookup"><span data-stu-id="30126-187">The null state is used to determine whether a warning should be given about null-unsafe conversions and dereferences.</span></span>

### <a name="null-tracking-for-variables"></a><span data-ttu-id="30126-188">Sledování hodnoty null pro proměnné</span><span class="sxs-lookup"><span data-stu-id="30126-188">Null tracking for variables</span></span>

<span data-ttu-id="30126-189">Pro určité výrazy zaznamenání proměnných nebo vlastností je stav null sledován mezi výskyty na základě přiřazení k těmto hodnotám, v nich byly provedeny testy a tok řízení mezi nimi.</span><span class="sxs-lookup"><span data-stu-id="30126-189">For certain expressions denoting variables or properties, the null state is tracked between occurrences, based on assignments to them, tests performed on them and the control flow between them.</span></span> <span data-ttu-id="30126-190">To se podobá tomu, jak je u proměnných sledováno jednoznačné přiřazení.</span><span class="sxs-lookup"><span data-stu-id="30126-190">This is similar to how definite assignment is tracked for variables.</span></span> <span data-ttu-id="30126-191">Sledované výrazy jsou ty z následujícího formuláře:</span><span class="sxs-lookup"><span data-stu-id="30126-191">The tracked expressions are the ones of the following form:</span></span>

```antlr
tracked_expression
    : simple_name
    | this
    | base
    | tracked_expression '.' identifier
    ;
```

<span data-ttu-id="30126-192">Kde identifikátory označují pole nebo vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="30126-192">Where the identifiers denote fields or properties.</span></span>

<span data-ttu-id="30126-193">***Popsat přechody stavu s hodnotou null podobně jako u jednoznačného přiřazení***</span><span class="sxs-lookup"><span data-stu-id="30126-193">***Describe null state transitions similar to definite assignment***</span></span>

### <a name="null-state-for-expressions"></a><span data-ttu-id="30126-194">Nulový stav pro výrazy</span><span class="sxs-lookup"><span data-stu-id="30126-194">Null state for expressions</span></span>

<span data-ttu-id="30126-195">Nulový stav výrazu je odvozen z jeho formuláře a typu a z nulového stavu proměnných, které jsou v něm zapojeny.</span><span class="sxs-lookup"><span data-stu-id="30126-195">The null state of an expression is derived from its form and type, and from the null state of variables involved in it.</span></span>

### <a name="literals"></a><span data-ttu-id="30126-196">Literály</span><span class="sxs-lookup"><span data-stu-id="30126-196">Literals</span></span>

<span data-ttu-id="30126-197">Nulová stav literálu `null` je "možná null".</span><span class="sxs-lookup"><span data-stu-id="30126-197">The null state of a `null` literal is "maybe null".</span></span> <span data-ttu-id="30126-198">Stav null literálu `default`, který je převáděn na typ, který je známý jako nehodnotný typ hodnoty, který není null, je "možná null".</span><span class="sxs-lookup"><span data-stu-id="30126-198">The null state of a `default` literal that is being converted to a type that is known not to be a nonnullable value type is "maybe null".</span></span> <span data-ttu-id="30126-199">Nulový stav jakéhokoli jiného literálu je "NOT NULL".</span><span class="sxs-lookup"><span data-stu-id="30126-199">The null state of any other literal is "not null".</span></span>

### <a name="simple-names"></a><span data-ttu-id="30126-200">Jednoduché názvy</span><span class="sxs-lookup"><span data-stu-id="30126-200">Simple names</span></span>

<span data-ttu-id="30126-201">Není-li `simple_name` klasifikován jako hodnota, má stav null hodnotu not null.</span><span class="sxs-lookup"><span data-stu-id="30126-201">If a `simple_name` is not classified as a value, its null state is "not null".</span></span> <span data-ttu-id="30126-202">V opačném případě se jedná o sledovaný výraz a jeho stav null je jeho sledovaný stav null v tomto zdrojovém umístění.</span><span class="sxs-lookup"><span data-stu-id="30126-202">Otherwise it is a tracked expression, and its null state is its tracked null state at this source location.</span></span>

### <a name="member-access"></a><span data-ttu-id="30126-203">Přístup ke členům</span><span class="sxs-lookup"><span data-stu-id="30126-203">Member access</span></span>

<span data-ttu-id="30126-204">Není-li `member_access` klasifikován jako hodnota, má stav null hodnotu not null.</span><span class="sxs-lookup"><span data-stu-id="30126-204">If a `member_access` is not classified as a value, its null state is "not null".</span></span> <span data-ttu-id="30126-205">Jinak, pokud se jedná o sledovaný výraz, jeho stav null je jeho sledovaný stav null v tomto zdrojovém umístění.</span><span class="sxs-lookup"><span data-stu-id="30126-205">Otherwise, if it is a tracked expression, its null state is its tracked null state at this source location.</span></span> <span data-ttu-id="30126-206">V opačném případě je stav null výchozím stavem null.</span><span class="sxs-lookup"><span data-stu-id="30126-206">Otherwise its null state is the default null state of its type.</span></span>

### <a name="invocation-expressions"></a><span data-ttu-id="30126-207">Výrazy vyvolání</span><span class="sxs-lookup"><span data-stu-id="30126-207">Invocation expressions</span></span>

<span data-ttu-id="30126-208">Pokud `invocation_expression` vyvolá člena deklarovaného s jedním nebo více atributy pro speciální chování null, je stav null určen těmito atributy.</span><span class="sxs-lookup"><span data-stu-id="30126-208">If an `invocation_expression` invokes a member that is declared with one or more attributes for special null behavior, the null state is determined by those attributes.</span></span> <span data-ttu-id="30126-209">V opačném případě je nulový stav výrazu výchozím prázdným stavem typu.</span><span class="sxs-lookup"><span data-stu-id="30126-209">Otherwise the null state of the expression is the default null state of its type.</span></span>

### <a name="element-access"></a><span data-ttu-id="30126-210">Přístup k prvkům</span><span class="sxs-lookup"><span data-stu-id="30126-210">Element access</span></span>

<span data-ttu-id="30126-211">Pokud `element_access` vyvolá indexer deklarovaný s jedním nebo více atributy pro speciální chování null, je stav null určen těmito atributy.</span><span class="sxs-lookup"><span data-stu-id="30126-211">If an `element_access` invokes an indexer that is declared with one or more attributes for special null behavior, the null state is determined by those attributes.</span></span> <span data-ttu-id="30126-212">V opačném případě je nulový stav výrazu výchozím prázdným stavem typu.</span><span class="sxs-lookup"><span data-stu-id="30126-212">Otherwise the null state of the expression is the default null state of its type.</span></span>

### <a name="base-access"></a><span data-ttu-id="30126-213">Základní přístup</span><span class="sxs-lookup"><span data-stu-id="30126-213">Base access</span></span>

<span data-ttu-id="30126-214">Pokud `B` označuje základní typ nadřazeného typu, `base.I` má stejný nulový stav jako `((B)this).I` a `base[E]` má stejný stav null jako `((B)this)[E]`.</span><span class="sxs-lookup"><span data-stu-id="30126-214">If `B` denotes the base type of the enclosing type, `base.I` has the same null state as `((B)this).I` and `base[E]` has the same null state as `((B)this)[E]`.</span></span>

### <a name="default-expressions"></a><span data-ttu-id="30126-215">Výchozí výrazy</span><span class="sxs-lookup"><span data-stu-id="30126-215">Default expressions</span></span>

<span data-ttu-id="30126-216">`default(T)` má nulový stav "non-null", pokud je `T` znám jako neprázdný typ hodnoty.</span><span class="sxs-lookup"><span data-stu-id="30126-216">`default(T)` has the null state "non-null" if `T` is known to be a nonnullable value type.</span></span> <span data-ttu-id="30126-217">V opačném případě má nulový stav "možná null".</span><span class="sxs-lookup"><span data-stu-id="30126-217">Otherwise it has the null state "maybe null".</span></span>

### <a name="null-conditional-expressions"></a><span data-ttu-id="30126-218">Podmíněné výrazy s hodnotou null</span><span class="sxs-lookup"><span data-stu-id="30126-218">Null-conditional expressions</span></span>

<span data-ttu-id="30126-219">`null_conditional_expression` má nulový stav "možná null".</span><span class="sxs-lookup"><span data-stu-id="30126-219">A `null_conditional_expression` has the null state "maybe null".</span></span>

### <a name="cast-expressions"></a><span data-ttu-id="30126-220">Výrazy cast</span><span class="sxs-lookup"><span data-stu-id="30126-220">Cast expressions</span></span>

<span data-ttu-id="30126-221">Pokud výraz cast `(T)E` vyvolá uživatelsky definovaný převod, pak je nulový stav výrazu výchozím stavem null pro daný typ.</span><span class="sxs-lookup"><span data-stu-id="30126-221">If a cast expression `(T)E` invokes a user-defined conversion, then the null state of the expression is the default null state for its type.</span></span> <span data-ttu-id="30126-222">V opačném případě, pokud `T` má hodnotu null (*Nullable* nebo *Unknown*), je stav null "možná null".</span><span class="sxs-lookup"><span data-stu-id="30126-222">Otherwise, if `T` is null-yielding (*nullable* or *unknown*) then the null state is "maybe null".</span></span> <span data-ttu-id="30126-223">V opačném případě je stav null stejný jako stav null `E`.</span><span class="sxs-lookup"><span data-stu-id="30126-223">Otherwise the null state is the same as the null state of `E`.</span></span>

### <a name="await-expressions"></a><span data-ttu-id="30126-224">Výrazy await</span><span class="sxs-lookup"><span data-stu-id="30126-224">Await expressions</span></span>

<span data-ttu-id="30126-225">Stav null `await E` je výchozím stavem null typu.</span><span class="sxs-lookup"><span data-stu-id="30126-225">The null state of `await E` is the default null state of its type.</span></span>

### <a name="the-as-operator"></a><span data-ttu-id="30126-226">Operátor `as`</span><span class="sxs-lookup"><span data-stu-id="30126-226">The `as` operator</span></span>

<span data-ttu-id="30126-227">Výraz `as` má nulový stav "možná null".</span><span class="sxs-lookup"><span data-stu-id="30126-227">An `as` expression has the null state "maybe null".</span></span>

### <a name="the-null-coalescing-operator"></a><span data-ttu-id="30126-228">Operátor slučování null</span><span class="sxs-lookup"><span data-stu-id="30126-228">The null-coalescing operator</span></span>

<span data-ttu-id="30126-229">`E1 ?? E2` má stejný stav null jako `E2`</span><span class="sxs-lookup"><span data-stu-id="30126-229">`E1 ?? E2` has the same null state as `E2`</span></span>

### <a name="the-conditional-operator"></a><span data-ttu-id="30126-230">Podmíněný operátor</span><span class="sxs-lookup"><span data-stu-id="30126-230">The conditional operator</span></span>

<span data-ttu-id="30126-231">Nulová stav `E1 ? E2 : E3` je "NOT NULL", pokud je nulový stav obou `E2` a `E3` "NOT NULL".</span><span class="sxs-lookup"><span data-stu-id="30126-231">The null state of `E1 ? E2 : E3` is "not null" if the null state of both `E2` and `E3` are "not null".</span></span> <span data-ttu-id="30126-232">V opačném případě je to "možná hodnota null".</span><span class="sxs-lookup"><span data-stu-id="30126-232">Otherwise it is "maybe null".</span></span>

### <a name="query-expressions"></a><span data-ttu-id="30126-233">Výrazy dotazů</span><span class="sxs-lookup"><span data-stu-id="30126-233">Query expressions</span></span>

<span data-ttu-id="30126-234">Stav null výrazu dotazu je výchozím stavem null typu.</span><span class="sxs-lookup"><span data-stu-id="30126-234">The null state of a query expression is the default null state of its type.</span></span>

### <a name="assignment-operators"></a><span data-ttu-id="30126-235">Operátory přiřazení</span><span class="sxs-lookup"><span data-stu-id="30126-235">Assignment operators</span></span>

<span data-ttu-id="30126-236">`E1 = E2` a `E1 op= E2` mají stejný stav null jako `E2` po použití jakýchkoli implicitních převodů.</span><span class="sxs-lookup"><span data-stu-id="30126-236">`E1 = E2` and `E1 op= E2` have the same null state as `E2` after any implicit conversions have been applied.</span></span>

### <a name="unary-and-binary-operators"></a><span data-ttu-id="30126-237">Unární a binární operátory</span><span class="sxs-lookup"><span data-stu-id="30126-237">Unary and binary operators</span></span>

<span data-ttu-id="30126-238">Pokud unární nebo binární operátor vyvolá uživatelsky definovaný operátor deklarovaný s jedním nebo více atributy pro speciální chování null, je stav null určen těmito atributy.</span><span class="sxs-lookup"><span data-stu-id="30126-238">If a unary or binary operator invokes an user-defined operator that is declared with one or more attributes for special null behavior, the null state is determined by those attributes.</span></span> <span data-ttu-id="30126-239">V opačném případě je nulový stav výrazu výchozím prázdným stavem typu.</span><span class="sxs-lookup"><span data-stu-id="30126-239">Otherwise the null state of the expression is the default null state of its type.</span></span>

<span data-ttu-id="30126-240">***Pro binární `+` přes řetězce a delegáty něco nějakého?***</span><span class="sxs-lookup"><span data-stu-id="30126-240">***Something special to do for binary `+` over strings and delegates?***</span></span>

### <a name="expressions-that-propagate-null-state"></a><span data-ttu-id="30126-241">Výrazy, které rozšiřují stav null</span><span class="sxs-lookup"><span data-stu-id="30126-241">Expressions that propagate null state</span></span>

<span data-ttu-id="30126-242">`(E)`, `checked(E)` a `unchecked(E)` mají stejný stav null jako `E`.</span><span class="sxs-lookup"><span data-stu-id="30126-242">`(E)`, `checked(E)` and `unchecked(E)` all have the same null state as `E`.</span></span>

### <a name="expressions-that-are-never-null"></a><span data-ttu-id="30126-243">Výrazy, které nemají hodnotu null</span><span class="sxs-lookup"><span data-stu-id="30126-243">Expressions that are never null</span></span>

<span data-ttu-id="30126-244">Stav null následujících formulářů výrazů je vždy "NOT NULL":</span><span class="sxs-lookup"><span data-stu-id="30126-244">The null state of the following expression forms is always "not null":</span></span>

- <span data-ttu-id="30126-245">přístup k `this`</span><span class="sxs-lookup"><span data-stu-id="30126-245">`this` access</span></span>
- <span data-ttu-id="30126-246">Interpolované řetězce</span><span class="sxs-lookup"><span data-stu-id="30126-246">interpolated strings</span></span>
- <span data-ttu-id="30126-247">výrazy `new` (objekt, delegát, anonymní objekty a vytváření polí)</span><span class="sxs-lookup"><span data-stu-id="30126-247">`new` expressions (object, delegate, anonymous object and array creation expressions)</span></span>
- <span data-ttu-id="30126-248">výrazy `typeof`</span><span class="sxs-lookup"><span data-stu-id="30126-248">`typeof` expressions</span></span>
- <span data-ttu-id="30126-249">výrazy `nameof`</span><span class="sxs-lookup"><span data-stu-id="30126-249">`nameof` expressions</span></span>
- <span data-ttu-id="30126-250">anonymní funkce (anonymní metody a lambda výrazy)</span><span class="sxs-lookup"><span data-stu-id="30126-250">anonymous functions (anonymous methods and lambda expressions)</span></span>
- <span data-ttu-id="30126-251">výrazy s hodnotou null – striktní</span><span class="sxs-lookup"><span data-stu-id="30126-251">null-forgiving expressions</span></span>
- <span data-ttu-id="30126-252">výrazy `is`</span><span class="sxs-lookup"><span data-stu-id="30126-252">`is` expressions</span></span>

## <a name="type-inference"></a><span data-ttu-id="30126-253">Odvození typu</span><span class="sxs-lookup"><span data-stu-id="30126-253">Type inference</span></span>

### <a name="type-inference-for-var"></a><span data-ttu-id="30126-254">Odvození typu pro `var`</span><span class="sxs-lookup"><span data-stu-id="30126-254">Type inference for `var`</span></span>

<span data-ttu-id="30126-255">Odvozený typ pro lokální proměnné deklarované pomocí `var` je informován stavem null inicializačního výrazu.</span><span class="sxs-lookup"><span data-stu-id="30126-255">The type inferred for local variables declared with `var` is informed by the null state of the initializing expression.</span></span>

```csharp
var x = E;
```

<span data-ttu-id="30126-256">Pokud typ `E` je typ odkazu s možnou hodnotou null `C?` a stav null `E` není null, pak je `C`typ odvozený pro `x`.</span><span class="sxs-lookup"><span data-stu-id="30126-256">If the type of `E` is a nullable reference type `C?` and the null state of `E` is "not null" then the type inferred for `x` is `C`.</span></span> <span data-ttu-id="30126-257">V opačném případě odvozený typ je typ `E`.</span><span class="sxs-lookup"><span data-stu-id="30126-257">Otherwise, the inferred type is the type of `E`.</span></span>

<span data-ttu-id="30126-258">Hodnota null typu odvozená pro `x` je určena výše, jak je popsáno výše v závislosti na kontextu poznámky `var`, stejně jako v případě, že byl typ zadán explicitně na této pozici.</span><span class="sxs-lookup"><span data-stu-id="30126-258">The nullability of the type inferred for `x` is determined as described above, based on the annotation context of the `var`, just as if the type had been given explicitly in that position.</span></span>

### <a name="type-inference-for-var"></a><span data-ttu-id="30126-259">Odvození typu pro `var?`</span><span class="sxs-lookup"><span data-stu-id="30126-259">Type inference for `var?`</span></span>

<span data-ttu-id="30126-260">Odvozený typ pro lokální proměnné deklarované pomocí `var?` je nezávislý na nulovém stavu inicializačního výrazu.</span><span class="sxs-lookup"><span data-stu-id="30126-260">The type inferred for local variables declared with `var?` is independent of the null state of the initializing expression.</span></span>

```csharp
var? x = E;
```

<span data-ttu-id="30126-261">Pokud typ `T` `E` je typ hodnoty s možnou hodnotou null nebo typ odkazu s možnou hodnotou null, je `T`odvozený typ `x`.</span><span class="sxs-lookup"><span data-stu-id="30126-261">If the type `T` of `E` is a nullable value type or a nullable reference type then the type inferred for `x` is `T`.</span></span> <span data-ttu-id="30126-262">V opačném případě, pokud je `T` typ hodnoty, která není null `S` je odvozený typ `S?`.</span><span class="sxs-lookup"><span data-stu-id="30126-262">Otherwise, if `T` is a nonnullable value type `S` the type inferred is `S?`.</span></span> <span data-ttu-id="30126-263">V opačném případě, pokud je `T` odkazový typ, který není null `C` je odvozený typ `C?`.</span><span class="sxs-lookup"><span data-stu-id="30126-263">Otherwise, if `T` is a nonnullable reference type `C` the type inferred is `C?`.</span></span> <span data-ttu-id="30126-264">V opačném případě je deklarace neplatná.</span><span class="sxs-lookup"><span data-stu-id="30126-264">Otherwise, the declaration is illegal.</span></span>

<span data-ttu-id="30126-265">Hodnota null typu odvozeného pro `x` je vždy *null*.</span><span class="sxs-lookup"><span data-stu-id="30126-265">The nullability of the type inferred for `x` is always *nullable*.</span></span>

### <a name="generic-type-inference"></a><span data-ttu-id="30126-266">Odvození obecného typu</span><span class="sxs-lookup"><span data-stu-id="30126-266">Generic type inference</span></span>

<span data-ttu-id="30126-267">Odvození obecného typu je vylepšeno, aby bylo možné určit, zda odvozené typy odkazů mají mít hodnotu null nebo ne.</span><span class="sxs-lookup"><span data-stu-id="30126-267">Generic type inference is enhanced to help decide whether inferred reference types should be nullable or not.</span></span> <span data-ttu-id="30126-268">Toto je nejlepší úsilí, které nezahrnuje a sám o sobě nepřinese upozornění, ale může vést k upozorněním s možnou hodnotou null, pokud jsou odvozeny typy vybraného přetížení aplikovány na argumenty.</span><span class="sxs-lookup"><span data-stu-id="30126-268">This is a best effort, and does not in and of itself yield warnings, but may lead to nullable warnings when the inferred types of the selected overload are applied to the arguments.</span></span>

<span data-ttu-id="30126-269">Odvození typu nespoléhá na kontext poznámky pro příchozí typy.</span><span class="sxs-lookup"><span data-stu-id="30126-269">The type inference does not rely on the annotation context of incoming types.</span></span> <span data-ttu-id="30126-270">Místo toho je odvozený `type`, který získá svůj vlastní kontext poznámky z místa, kde by byl, pokud byl vyjádřen explicitně.</span><span class="sxs-lookup"><span data-stu-id="30126-270">Instead a `type` is inferred which acquires its own annotation context from where it "would have been" if it had been expressed explicitly.</span></span> <span data-ttu-id="30126-271">Tato podtržítka představuje roli pro odvození typu jako pohodlí pro to, co byste mohli sami napsat.</span><span class="sxs-lookup"><span data-stu-id="30126-271">This underscores the role of type inference as a convenience for what you could have written yourself.</span></span>

<span data-ttu-id="30126-272">Přesněji je kontext poznámky pro odvozený typ argumentu kontextem tokenu, který by následoval za parametrem `<...>`ho typu. měl by se jednat o jeden. například název obecné metody, která je volána.</span><span class="sxs-lookup"><span data-stu-id="30126-272">More precisely, the annotation context for an inferred type argument is the context of the token that would have been followed by the `<...>` type parameter list, had there been one; i.e. the name of the generic method being called.</span></span> <span data-ttu-id="30126-273">Pro výrazy dotazů, které jsou přeloženy na taková volání, je kontext pořízen z počátečního klíčového slova klauzule dotazu, ze které je volání vygenerováno.</span><span class="sxs-lookup"><span data-stu-id="30126-273">For query expressions that translate to such calls, the context is taken from the initial contextual keyword of the query clause from which the call is generated.</span></span>

### <a name="the-first-phase"></a><span data-ttu-id="30126-274">První fáze</span><span class="sxs-lookup"><span data-stu-id="30126-274">The first phase</span></span>

<span data-ttu-id="30126-275">Typy odkazů s možnou hodnotou null se zanášejí do hranic z počátečních výrazů, jak je popsáno níže.</span><span class="sxs-lookup"><span data-stu-id="30126-275">Nullable reference types flow into the bounds from the initial expressions, as described below.</span></span> <span data-ttu-id="30126-276">Kromě toho jsou zavedeny dva nové druhy mezí, totiž `null` a `default`.</span><span class="sxs-lookup"><span data-stu-id="30126-276">In addition, two new kinds of bounds, namely `null` and `default` are introduced.</span></span> <span data-ttu-id="30126-277">Jejich účelem je přecházet z výskytů `null` nebo `default` ve vstupních výrazech, což může způsobit, že odvozený typ bude mít hodnotu null, i když by jinak nevznikl.</span><span class="sxs-lookup"><span data-stu-id="30126-277">Their purpose is to carry through occurrences of `null` or `default` in the input expressions, which may cause an inferred type to be nullable, even when it otherwise wouldn't.</span></span> <span data-ttu-id="30126-278">To funguje i *pro typy s možnou hodnotou null* , které jsou vylepšené pro vyzvednutí "hodnoty null" v procesu odvození.</span><span class="sxs-lookup"><span data-stu-id="30126-278">This works even for nullable *value* types, which are enhanced to pick up "nullness" in the inference process.</span></span>

<span data-ttu-id="30126-279">Určení toho, co je možné přidat v první fázi, je vylepšeno následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="30126-279">The determination of what bounds to add in the first phase are enhanced as follows:</span></span>

<span data-ttu-id="30126-280">Pokud má argument `Ei` odkazový typ, typ `U` použitý pro odvození závisí na stavu null `Ei` a také na deklarovaném typu:</span><span class="sxs-lookup"><span data-stu-id="30126-280">If an argument `Ei` has a reference type, the type `U` used for inference depends on the null state of `Ei` as well as its declared type:</span></span>
- <span data-ttu-id="30126-281">Pokud deklarovaný typ je typ odkazu, který není `U0`, nebo typ odkazu s možnou hodnotou null `U0?` potom</span><span class="sxs-lookup"><span data-stu-id="30126-281">If the declared type is a nonnullable reference type `U0` or a nullable reference type `U0?` then</span></span>
    - <span data-ttu-id="30126-282">Pokud je nulový stav `Ei` "NOT NULL", pak `U` je `U0`</span><span class="sxs-lookup"><span data-stu-id="30126-282">if the null state of `Ei` is "not null" then `U` is `U0`</span></span>
    - <span data-ttu-id="30126-283">Pokud je stav null `Ei` "možná null", pak `U` je `U0?`</span><span class="sxs-lookup"><span data-stu-id="30126-283">if the null state of `Ei` is "maybe null" then `U` is `U0?`</span></span>
- <span data-ttu-id="30126-284">Jinak, pokud `Ei` má deklarovaný typ, `U` je tento typ</span><span class="sxs-lookup"><span data-stu-id="30126-284">Otherwise if `Ei` has a declared type, `U` is that type</span></span>
- <span data-ttu-id="30126-285">Jinak, pokud je `Ei` `null` pak je `U` speciálním vázaným `null`</span><span class="sxs-lookup"><span data-stu-id="30126-285">Otherwise if `Ei` is `null` then `U` is the special bound `null`</span></span>
- <span data-ttu-id="30126-286">Jinak, pokud je `Ei` `default` pak je `U` speciálním vázaným `default`</span><span class="sxs-lookup"><span data-stu-id="30126-286">Otherwise if `Ei` is `default` then `U` is the special bound `default`</span></span>
- <span data-ttu-id="30126-287">Jinak není provedeno odvození.</span><span class="sxs-lookup"><span data-stu-id="30126-287">Otherwise no inference is made.</span></span>

### <a name="exact-upper-bound-and-lower-bound-inferences"></a><span data-ttu-id="30126-288">Přesná, shora vázaná a s nižšími nároky na odvozená</span><span class="sxs-lookup"><span data-stu-id="30126-288">Exact, upper-bound and lower-bound inferences</span></span>

<span data-ttu-id="30126-289">V odvozených *od* typu `U` *k* typu `V`, pokud `V` je typ odkazu s možnou hodnotou null `V0?`, pak `V0` používá se místo `V` v následujících klauzulích.</span><span class="sxs-lookup"><span data-stu-id="30126-289">In inferences *from* the type `U` *to* the type `V`, if `V` is a nullable reference type `V0?`, then `V0` is used instead of `V` in the following clauses.</span></span>
- <span data-ttu-id="30126-290">Je-li `V` jednou z neopravených proměnných typu, `U` je přidána jako přesné, horní nebo dolní mez jako před</span><span class="sxs-lookup"><span data-stu-id="30126-290">If `V` is one of the unfixed type variables, `U` is added as an exact, upper or lower bound as before</span></span>
- <span data-ttu-id="30126-291">V opačném případě, pokud je `U` `null` nebo `default`, není provedeno odvození</span><span class="sxs-lookup"><span data-stu-id="30126-291">Otherwise, if `U` is `null` or `default`, no inference is made</span></span>
- <span data-ttu-id="30126-292">V opačném případě, pokud je `U` `U0?`typu odkazu s možnou hodnotou null, je místo `U` v následujících klauzulích použit `U0`.</span><span class="sxs-lookup"><span data-stu-id="30126-292">Otherwise, if `U` is a nullable reference type `U0?`, then `U0` is used instead of `U` in the subsequent clauses.</span></span>

<span data-ttu-id="30126-293">Podstata je, že hodnota null, která se vztahuje přímo k jedné z nepevných proměnných, se zachová do jejich hranic.</span><span class="sxs-lookup"><span data-stu-id="30126-293">The essence is that nullability that pertains directly to one of the unfixed type variables is preserved into its bounds.</span></span> <span data-ttu-id="30126-294">Pro odvození, která se dále přesměrují do zdrojového a cílového typu na druhé straně, je hodnota null ignorována.</span><span class="sxs-lookup"><span data-stu-id="30126-294">For the inferences that recurse further into the source and target types, on the other hand, nullability is ignored.</span></span> <span data-ttu-id="30126-295">Může nebo nemusí odpovídat, ale pokud ne, bude upozornění vydáno později, pokud je vybráno a použito přetížení.</span><span class="sxs-lookup"><span data-stu-id="30126-295">It may or may not match, but if it doesn't, a warning will be issued later if the overload is chosen and applied.</span></span>

### <a name="fixing"></a><span data-ttu-id="30126-296">Opravě</span><span class="sxs-lookup"><span data-stu-id="30126-296">Fixing</span></span>

<span data-ttu-id="30126-297">Specifikace v současné době neprovádí dobrou úlohu popisující, co se stane, když je mezi sebou identita více, ale liší se.</span><span class="sxs-lookup"><span data-stu-id="30126-297">The spec currently does not do a good job of describing what happens when multiple bounds are identity convertible to each other, but are different.</span></span> <span data-ttu-id="30126-298">K tomu může dojít mezi `object` a `dynamic`, mezi typy řazené kolekce členů, které se liší pouze v názvech prvků, mezi typy sestavenými z nich a nyní mezi `C` a `C?` pro typy odkazů.</span><span class="sxs-lookup"><span data-stu-id="30126-298">This may happen between `object` and `dynamic`, between tuple types that differ only in element names, between types constructed thereof and now also between `C` and `C?` for reference types.</span></span>

<span data-ttu-id="30126-299">Kromě toho je potřeba rozšířit "hodnotu null" ze vstupních výrazů na typ výsledku.</span><span class="sxs-lookup"><span data-stu-id="30126-299">In addition we need to propagate "nullness" from the input expressions to the result type.</span></span> 

<span data-ttu-id="30126-300">Pro zpracování těchto řešení přidáváme další fáze pro opravu, která je teď:</span><span class="sxs-lookup"><span data-stu-id="30126-300">To handle these we add more phases to fixing, which is now:</span></span>

1. <span data-ttu-id="30126-301">Shromážděte všechny typy ve všech hranicích jako kandidáty, přičemž odebrání `?` ze všech, které jsou typy odkazů s možnou hodnotou null.</span><span class="sxs-lookup"><span data-stu-id="30126-301">Gather all the types in all the bounds as candidates, removing `?` from all that are nullable reference types</span></span>
2. <span data-ttu-id="30126-302">Eliminujte kandidáty na základě požadavků přesně, dolních a horních mezí (udržení `null` a `default` hranic).</span><span class="sxs-lookup"><span data-stu-id="30126-302">Eliminate candidates based on requirements of exact, lower and upper bounds (keeping `null` and `default` bounds)</span></span>
3. <span data-ttu-id="30126-303">Eliminujte kandidáty, které nemají implicitní převod na všechny ostatní kandidáty.</span><span class="sxs-lookup"><span data-stu-id="30126-303">Eliminate candidates that do not have an implicit conversion to all the other candidates</span></span>
4. <span data-ttu-id="30126-304">Pokud zbývající kandidáti nemají všechny převody identity na sebe navzájem, pak se typ odvození nezdařil.</span><span class="sxs-lookup"><span data-stu-id="30126-304">If the remaining candidates do not all have identity conversions to one another, then type inference fails</span></span>
5. <span data-ttu-id="30126-305">*Sloučit* zbývající kandidáty, jak je popsáno níže</span><span class="sxs-lookup"><span data-stu-id="30126-305">*Merge* the remaining candidates as described below</span></span>
6. <span data-ttu-id="30126-306">Pokud výsledný kandidát je odkazový typ nebo typ hodnoty, která není null, a *všechny* přesné hranice nebo *jakékoli* dolní meze jsou typy hodnot s možnou hodnotou null, typy odkazů s možnou hodnotou null, `null` nebo `default`a `?` je přidána do výsledného kandidáta, takže je typ hodnoty s možnou hodnotou null nebo odkaz.</span><span class="sxs-lookup"><span data-stu-id="30126-306">If the resulting candidate is a reference type or a nonnullable value type and *all* of the exact bounds or *any* of the lower bounds are nullable value types, nullable reference types, `null` or `default`, then `?` is added to the resulting candidate, making it a nullable value type or reference type.</span></span>

<span data-ttu-id="30126-307">*Sloučení* je popsáno mezi dvěma typy kandidátů.</span><span class="sxs-lookup"><span data-stu-id="30126-307">*Merging* is described between two candidate types.</span></span> <span data-ttu-id="30126-308">Je tranzitivní a komutativníý, takže kandidáty se můžou sloučit do libovolného pořadí se stejným konečným výsledkem.</span><span class="sxs-lookup"><span data-stu-id="30126-308">It is transitive and commutative, so the candidates can be merged in any order with the same ultimate result.</span></span> <span data-ttu-id="30126-309">Není definováno, pokud nejsou dva typy kandidátů vzájemně převoditelné.</span><span class="sxs-lookup"><span data-stu-id="30126-309">It is undefined if the two candidate types are not identity convertible to each other.</span></span>

<span data-ttu-id="30126-310">Funkce *Merge* přijímá dva typy kandidátů a směr ( *+* nebo *-* ):</span><span class="sxs-lookup"><span data-stu-id="30126-310">The *Merge* function takes two candidate types and a direction (*+* or *-*):</span></span>

- <span data-ttu-id="30126-311">*Merge*(`T`, `T`, *d*) = t</span><span class="sxs-lookup"><span data-stu-id="30126-311">*Merge*(`T`, `T`, *d*) = T</span></span>
- <span data-ttu-id="30126-312">*Merge*(`S`, `T?`, *+* ) = *Merge*(`S?`, `T`, *+* ) = *Merge*(`S`, `T`, *+* )`?`</span><span class="sxs-lookup"><span data-stu-id="30126-312">*Merge*(`S`, `T?`, *+*) = *Merge*(`S?`, `T`, *+*) = *Merge*(`S`, `T`, *+*)`?`</span></span>
- <span data-ttu-id="30126-313">*Sloučení*(`S`, `T?`, *-* ) = *sloučení*(`S?`, `T`, *-* ) = *sloučení*(`S`, `T`, *-* )</span><span class="sxs-lookup"><span data-stu-id="30126-313">*Merge*(`S`, `T?`, *-*) = *Merge*(`S?`, `T`, *-*) = *Merge*(`S`, `T`, *-*)</span></span>
- <span data-ttu-id="30126-314">*Merge*(`C<S1,...,Sn>`, `C<T1,...,Tn>`, *+* ) = `C<`*sloučení*(`S1`, `T1`, *d1*)`,...,`*sloučení*(`Sn`, `Tn`, *DN*)`>`, *kde*</span><span class="sxs-lookup"><span data-stu-id="30126-314">*Merge*(`C<S1,...,Sn>`, `C<T1,...,Tn>`, *+*) = `C<`*Merge*(`S1`, `T1`, *d1*)`,...,`*Merge*(`Sn`, `Tn`, *dn*)`>`, *where*</span></span>
    - <span data-ttu-id="30126-315">`di` =  *+* , pokud je parametr ' th type ' `C<...>` `i`kovariantní</span><span class="sxs-lookup"><span data-stu-id="30126-315">`di` = *+* if the `i`'th type parameter of `C<...>` is covariant</span></span>
    - <span data-ttu-id="30126-316">`di` =  *-* , pokud je parametr ' th type ' `C<...>` `i`kontraindikace nebo invariantní</span><span class="sxs-lookup"><span data-stu-id="30126-316">`di` = *-* if the `i`'th type parameter of `C<...>` is contra- or invariant</span></span>
- <span data-ttu-id="30126-317">*Merge*(`C<S1,...,Sn>`, `C<T1,...,Tn>`, *-* ) = `C<`*sloučení*(`S1`, `T1`, *d1*)`,...,`*sloučení*(`Sn`, `Tn`, *DN*)`>`, *kde*</span><span class="sxs-lookup"><span data-stu-id="30126-317">*Merge*(`C<S1,...,Sn>`, `C<T1,...,Tn>`, *-*) = `C<`*Merge*(`S1`, `T1`, *d1*)`,...,`*Merge*(`Sn`, `Tn`, *dn*)`>`, *where*</span></span>
    - <span data-ttu-id="30126-318">`di` =  *-* , pokud je parametr ' th type ' `C<...>` `i`kovariantní</span><span class="sxs-lookup"><span data-stu-id="30126-318">`di` = *-* if the `i`'th type parameter of `C<...>` is covariant</span></span>
    - <span data-ttu-id="30126-319">`di` =  *+* , pokud je parametr ' th type ' `C<...>` `i`kontraindikace nebo invariantní</span><span class="sxs-lookup"><span data-stu-id="30126-319">`di` = *+* if the `i`'th type parameter of `C<...>` is contra- or invariant</span></span>
- <span data-ttu-id="30126-320">*Merge*(`(S1 s1,..., Sn sn)`, `(T1 t1,..., Tn tn)`, *d*) = `(`*sloučení*(`S1`, `T1`, *d*)`n1,...,`*sloučení*(`Sn`, `Tn`, *d*) `nn)`, *kde*</span><span class="sxs-lookup"><span data-stu-id="30126-320">*Merge*(`(S1 s1,..., Sn sn)`, `(T1 t1,..., Tn tn)`, *d*) = `(`*Merge*(`S1`, `T1`, *d*)`n1,...,`*Merge*(`Sn`, `Tn`, *d*) `nn)`, *where*</span></span>
    - <span data-ttu-id="30126-321">`ni` chybí, pokud se `si` a `ti` liší, nebo pokud chybí obojí.</span><span class="sxs-lookup"><span data-stu-id="30126-321">`ni` is absent if `si` and `ti` differ, or if both are absent</span></span>
    - <span data-ttu-id="30126-322">`ni` je `si`, pokud `si` a `ti` stejné</span><span class="sxs-lookup"><span data-stu-id="30126-322">`ni` is `si` if `si` and `ti` are the same</span></span>
- <span data-ttu-id="30126-323">*Merge*(`object`, `dynamic`) = *Merge*(`dynamic`, `object`) = `dynamic`</span><span class="sxs-lookup"><span data-stu-id="30126-323">*Merge*(`object`, `dynamic`) = *Merge*(`dynamic`, `object`) = `dynamic`</span></span>

## <a name="warnings"></a><span data-ttu-id="30126-324">Upozornění</span><span class="sxs-lookup"><span data-stu-id="30126-324">Warnings</span></span>

### <a name="potential-null-assignment"></a><span data-ttu-id="30126-325">Potenciální null přiřazení</span><span class="sxs-lookup"><span data-stu-id="30126-325">Potential null assignment</span></span>

### <a name="potential-null-dereference"></a><span data-ttu-id="30126-326">Potenciální zpětný odkaz na hodnotu null</span><span class="sxs-lookup"><span data-stu-id="30126-326">Potential null dereference</span></span>

### <a name="constraint-nullability-mismatch"></a><span data-ttu-id="30126-327">Neshoda omezení hodnoty null</span><span class="sxs-lookup"><span data-stu-id="30126-327">Constraint nullability mismatch</span></span>

### <a name="nullable-types-in-disabled-annotation-context"></a><span data-ttu-id="30126-328">Typy s možnou hodnotou null v kontextu zakázané poznámky</span><span class="sxs-lookup"><span data-stu-id="30126-328">Nullable types in disabled annotation context</span></span>

## <a name="attributes-for-special-null-behavior"></a><span data-ttu-id="30126-329">Atributy pro speciální chování null</span><span class="sxs-lookup"><span data-stu-id="30126-329">Attributes for special null behavior</span></span>


