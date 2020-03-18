---
ms.openlocfilehash: 3df21c5816be90387a6cd9242e99ba11f43dfd1c
ms.sourcegitcommit: f61a06970fa0562d2e40363fae3948eb168624ca
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/14/2020
ms.locfileid: "79485166"
---
# <a name="pattern-matching-for-c-7"></a><span data-ttu-id="4e633-101">Porovnávání vzorů C# pro 7</span><span class="sxs-lookup"><span data-stu-id="4e633-101">Pattern Matching for C# 7</span></span>

<span data-ttu-id="4e633-102">Rozšíření pro porovnávání vzorů pro C# zajištění mnoha výhod algebraických datových typů a porovnávání vzorů z funkčních jazyků, ale způsobem, který hladce integruje s chováním základního jazyka.</span><span class="sxs-lookup"><span data-stu-id="4e633-102">Pattern matching extensions for C# enable many of the benefits of algebraic data types and pattern matching from functional languages, but in a way that smoothly integrates with the feel of the underlying language.</span></span> <span data-ttu-id="4e633-103">Základní funkce jsou: [typy záznamů](https://github.com/dotnet/csharplang/blob/master/proposals/records.md), které jsou typy, jejichž sémantický význam je popsán tvarem dat; a porovnávání vzorů, což je nový formulář s výrazem, který umožňuje extrémně stručné rozložení s těmito datovými typy.</span><span class="sxs-lookup"><span data-stu-id="4e633-103">The basic features are: [record types](https://github.com/dotnet/csharplang/blob/master/proposals/records.md), which are types whose semantic meaning is described by the shape of the data; and pattern matching, which is a new expression form that enables extremely concise multilevel decomposition of these data types.</span></span> <span data-ttu-id="4e633-104">Prvky tohoto přístupu jsou nechte inspirovat souvisejícími funkcemi v programovacích jazycích [F#](https://www.microsoft.com/en-us/research/wp-content/uploads/2016/02/p29-syme.pdf "Rozšiřitelné porovnávání vzorů přes zjednodušený jazyk") a [Scala](https://infoscience.epfl.ch/record/98468/files/MatchingObjectsWithPatterns-TR.pdf "Vyhovující objekty se vzorci").</span><span class="sxs-lookup"><span data-stu-id="4e633-104">Elements of this approach are inspired by related features in the programming languages [F#](https://www.microsoft.com/en-us/research/wp-content/uploads/2016/02/p29-syme.pdf "Extensible Pattern Matching Via a Lightweight Language") and [Scala](https://infoscience.epfl.ch/record/98468/files/MatchingObjectsWithPatterns-TR.pdf "Matching Objects With Patterns").</span></span>

## <a name="is-expression"></a><span data-ttu-id="4e633-105">Výraz is</span><span class="sxs-lookup"><span data-stu-id="4e633-105">Is expression</span></span>

<span data-ttu-id="4e633-106">Operátor `is` je rozšířen o testování výrazu na základě *vzoru*.</span><span class="sxs-lookup"><span data-stu-id="4e633-106">The `is` operator is extended to test an expression against a *pattern*.</span></span>

```antlr
relational_expression
    : relational_expression 'is' pattern
    ;
```

<span data-ttu-id="4e633-107">Tato forma *relational_expression* je kromě existujících formulářů ve C# specifikaci.</span><span class="sxs-lookup"><span data-stu-id="4e633-107">This form of *relational_expression* is in addition to the existing forms in the C# specification.</span></span> <span data-ttu-id="4e633-108">Jedná se o chybu při kompilaci, pokud *relational_expression* vlevo od `is` tokenu neurčuje hodnotu nebo nemá typ.</span><span class="sxs-lookup"><span data-stu-id="4e633-108">It is a compile-time error if the *relational_expression* to the left of the `is` token does not designate a value or does not have a type.</span></span>

<span data-ttu-id="4e633-109">Každý *identifikátor* vzoru zavádí novou místní proměnnou, která je *omezena* na základě `true`ho operátoru `is` (tj. *jednoznačně přiřazený při hodnotě true*).</span><span class="sxs-lookup"><span data-stu-id="4e633-109">Every *identifier* of the pattern introduces a new local variable that is *definitely assigned* after the `is` operator is `true` (i.e. *definitely assigned when true*).</span></span>

> <span data-ttu-id="4e633-110">Poznámka: existují technicky nejednoznačnost mezi *typem* v `is-expression` a *constant_pattern*, z nichž každá může být platným analýzou kvalifikovaného identifikátoru.</span><span class="sxs-lookup"><span data-stu-id="4e633-110">Note: There is technically an ambiguity between *type* in an `is-expression` and *constant_pattern*, either of which might be a valid parse of a qualified identifier.</span></span> <span data-ttu-id="4e633-111">Pokusíme se vytvořit propojení jako typ pro kompatibilitu s předchozími verzemi jazyka; jenom v případě, že se to nepovede, můžeme ho vyřešit jako v ostatních kontextech na první nalezenou věc (která musí být buď konstanta, nebo typ).</span><span class="sxs-lookup"><span data-stu-id="4e633-111">We try to bind it as a type for compatibility with previous versions of the language; only if that fails do we resolve it as we do in other contexts, to the first thing found (which must be either a constant or a type).</span></span> <span data-ttu-id="4e633-112">Tato nejednoznačnost je k dispozici pouze na pravé straně výrazu `is`.</span><span class="sxs-lookup"><span data-stu-id="4e633-112">This ambiguity is only present on the right-hand-side of an `is` expression.</span></span>

## <a name="patterns"></a><span data-ttu-id="4e633-113">Vzory</span><span class="sxs-lookup"><span data-stu-id="4e633-113">Patterns</span></span>

<span data-ttu-id="4e633-114">V operátoru `is` a v *switch_statement* se používají vzory, které vyjadřují tvar dat, ke kterým se budou porovnávat příchozí data.</span><span class="sxs-lookup"><span data-stu-id="4e633-114">Patterns are used in the `is` operator and in a *switch_statement* to express the shape of data against which incoming data is to be compared.</span></span> <span data-ttu-id="4e633-115">Vzorce mohou být rekurzivní, aby mohly být části dat porovnány s dílčími vzory.</span><span class="sxs-lookup"><span data-stu-id="4e633-115">Patterns may be recursive so that parts of the data may be matched against sub-patterns.</span></span>

```antlr
pattern
    : declaration_pattern
    | constant_pattern
    | var_pattern
    ;

declaration_pattern
    : type simple_designation
    ;

constant_pattern
    : shift_expression
    ;

var_pattern
    : 'var' simple_designation
    ;
```

> <span data-ttu-id="4e633-116">Poznámka: existují technicky nejednoznačnost mezi *typem* v `is-expression` a *constant_pattern*, z nichž každá může být platným analýzou kvalifikovaného identifikátoru.</span><span class="sxs-lookup"><span data-stu-id="4e633-116">Note: There is technically an ambiguity between *type* in an `is-expression` and *constant_pattern*, either of which might be a valid parse of a qualified identifier.</span></span> <span data-ttu-id="4e633-117">Pokusíme se vytvořit propojení jako typ pro kompatibilitu s předchozími verzemi jazyka; jenom v případě, že se to nepovede, můžeme ho vyřešit jako v ostatních kontextech na první nalezenou věc (která musí být buď konstanta, nebo typ).</span><span class="sxs-lookup"><span data-stu-id="4e633-117">We try to bind it as a type for compatibility with previous versions of the language; only if that fails do we resolve it as we do in other contexts, to the first thing found (which must be either a constant or a type).</span></span> <span data-ttu-id="4e633-118">Tato nejednoznačnost je k dispozici pouze na pravé straně výrazu `is`.</span><span class="sxs-lookup"><span data-stu-id="4e633-118">This ambiguity is only present on the right-hand-side of an `is` expression.</span></span>

### <a name="declaration-pattern"></a><span data-ttu-id="4e633-119">Vzor deklarace</span><span class="sxs-lookup"><span data-stu-id="4e633-119">Declaration pattern</span></span>

<span data-ttu-id="4e633-120">*Declaration_pattern* testuje, zda je výraz daný typ, a přetypování na tento typ, pokud je test úspěšný.</span><span class="sxs-lookup"><span data-stu-id="4e633-120">The *declaration_pattern* both tests that an expression is of a given type and casts it to that type if the test succeeds.</span></span> <span data-ttu-id="4e633-121">Pokud je *simple_designation* identifikátor, zavádí místní proměnnou daného typu pojmenovaného daným identifikátorem.</span><span class="sxs-lookup"><span data-stu-id="4e633-121">If the *simple_designation* is an identifier, it introduces a local variable of the given type named by the given identifier.</span></span> <span data-ttu-id="4e633-122">Tato lokální proměnná je *jednoznačně přiřazena* , pokud je výsledkem operace porovnávání se vzorem hodnota true.</span><span class="sxs-lookup"><span data-stu-id="4e633-122">That local variable is *definitely assigned* when the result of the pattern-matching operation is true.</span></span>

```antlr
declaration_pattern
    : type simple_designation
    ;
```

<span data-ttu-id="4e633-123">Sémantika modulu runtime tohoto výrazu je, že testuje typ modulu runtime *relational_expression* operandu na levé straně proti *typu* ve vzoru.</span><span class="sxs-lookup"><span data-stu-id="4e633-123">The runtime semantic of this expression is that it tests the runtime type of the left-hand *relational_expression* operand against the *type* in the pattern.</span></span> <span data-ttu-id="4e633-124">Pokud se jedná o tento typ modulu runtime (nebo nějaký podtyp), je výsledek `is operator` `true`.</span><span class="sxs-lookup"><span data-stu-id="4e633-124">If it is of that runtime type (or some subtype), the result of the `is operator` is `true`.</span></span> <span data-ttu-id="4e633-125">Deklaruje novou místní proměnnou pojmenovanou *identifikátorem* , který je přiřazena hodnota operandu na levé straně při `true`výsledku.</span><span class="sxs-lookup"><span data-stu-id="4e633-125">It declares a new local variable named by the *identifier* that is assigned the value of the left-hand operand when the result is `true`.</span></span>

<span data-ttu-id="4e633-126">Určité kombinace statického typu na levé straně a daného typu se považují za nekompatibilní a výsledkem je chyba při kompilaci.</span><span class="sxs-lookup"><span data-stu-id="4e633-126">Certain combinations of static type of the left-hand-side and the given type are considered incompatible and result in compile-time error.</span></span> <span data-ttu-id="4e633-127">Hodnota statického typu `E` je označována jako *vzor kompatibilní* s typem `T`, pokud existuje převod identity, implicitní převod odkazu, převod zabalení, explicitní převod odkazu nebo převod rozbalení z `E` na `T`.</span><span class="sxs-lookup"><span data-stu-id="4e633-127">A value of static type `E` is said to be *pattern compatible* with the type `T` if there exists an identity conversion, an implicit reference conversion, a boxing conversion, an explicit reference conversion, or an unboxing conversion from `E` to `T`.</span></span> <span data-ttu-id="4e633-128">Jedná se o chybu při kompilaci, pokud výraz typu `E` není kompatibilní se vzorem typu v rámci vzoru typu, se kterým se shoduje.</span><span class="sxs-lookup"><span data-stu-id="4e633-128">It is a compile-time error if an expression of type `E` is not pattern compatible with the type in a type pattern that it is matched with.</span></span>

> <span data-ttu-id="4e633-129">Poznámka: [v C# 7,1 jsme toto rozšířili](../csharp-7.1/generics-pattern-match.md) tak, aby povolovala operace porovnávání se vzorem, pokud je typ vstupu nebo typ `T` otevřený.</span><span class="sxs-lookup"><span data-stu-id="4e633-129">Note: [In C# 7.1 we extend this](../csharp-7.1/generics-pattern-match.md) to permit a pattern-matching operation if either the input type or the type `T` is an open type.</span></span> <span data-ttu-id="4e633-130">Tento odstavec se nahrazuje tímto:</span><span class="sxs-lookup"><span data-stu-id="4e633-130">This paragraph is replaced by the following:</span></span>
> 
> <span data-ttu-id="4e633-131">Určité kombinace statického typu na levé straně a daného typu se považují za nekompatibilní a výsledkem je chyba při kompilaci.</span><span class="sxs-lookup"><span data-stu-id="4e633-131">Certain combinations of static type of the left-hand-side and the given type are considered incompatible and result in compile-time error.</span></span> <span data-ttu-id="4e633-132">Hodnota statického typu `E` je označována jako *vzor kompatibilní* s typem `T`, pokud existuje převod identity, implicitní převod odkazu, převod zabalení, explicitní převod odkazu nebo převod rozbalení z `E` na `T`**nebo pokud je buď `E` nebo `T` otevřeným typem**.</span><span class="sxs-lookup"><span data-stu-id="4e633-132">A value of static type `E` is said to be *pattern compatible* with the type `T` if there exists an identity conversion, an implicit reference conversion, a boxing conversion, an explicit reference conversion, or an unboxing conversion from `E` to `T`, **or if either `E` or `T` is an open type**.</span></span> <span data-ttu-id="4e633-133">Jedná se o chybu při kompilaci, pokud výraz typu `E` není kompatibilní se vzorem typu v rámci vzoru typu, se kterým se shoduje.</span><span class="sxs-lookup"><span data-stu-id="4e633-133">It is a compile-time error if an expression of type `E` is not pattern compatible with the type in a type pattern that it is matched with.</span></span>

<span data-ttu-id="4e633-134">Vzor deklarace je vhodný pro provádění testů typu odkazu v době běhu a nahrazuje idiom</span><span class="sxs-lookup"><span data-stu-id="4e633-134">The declaration pattern is useful for performing run-time type tests of reference types, and replaces the idiom</span></span>

```csharp
var v = expr as Type;
if (v != null) { // code using v }
```

<span data-ttu-id="4e633-135">S mírným výstižným</span><span class="sxs-lookup"><span data-stu-id="4e633-135">With the slightly more concise</span></span>

```csharp
if (expr is Type v) { // code using v }
```

<span data-ttu-id="4e633-136">Jedná se o chybu, pokud je *typ* hodnota s možnou hodnotou null.</span><span class="sxs-lookup"><span data-stu-id="4e633-136">It is an error if *type* is a nullable value type.</span></span>

<span data-ttu-id="4e633-137">Vzor deklarace lze použít k otestování hodnot typů s možnou hodnotou null: hodnota typu `Nullable<T>` (nebo zabalená `T`) odpovídá vzoru typu `T2 id`, pokud hodnota není null a typ `T2` je `T`nebo některý základní typ nebo rozhraní `T`.</span><span class="sxs-lookup"><span data-stu-id="4e633-137">The declaration pattern can be used to test values of nullable types: a value of type `Nullable<T>` (or a boxed `T`) matches a type pattern `T2 id` if the value is non-null and the type of `T2` is `T`, or some base type or interface of `T`.</span></span> <span data-ttu-id="4e633-138">Například v fragmentu kódu</span><span class="sxs-lookup"><span data-stu-id="4e633-138">For example, in the code fragment</span></span>

```csharp
int? x = 3;
if (x is int v) { // code using v }
```

<span data-ttu-id="4e633-139">Podmínka příkazu `if` je `true` za běhu a proměnná `v` obsahuje hodnotu `3` typu `int` uvnitř bloku.</span><span class="sxs-lookup"><span data-stu-id="4e633-139">The condition of the `if` statement is `true` at runtime and the variable `v` holds the value `3` of type `int` inside the block.</span></span>

### <a name="constant-pattern"></a><span data-ttu-id="4e633-140">Konstantní vzorek</span><span class="sxs-lookup"><span data-stu-id="4e633-140">Constant pattern</span></span>

```antlr
constant_pattern
    : shift_expression
    ;
```

<span data-ttu-id="4e633-141">Konstantní vzor testuje hodnotu výrazu s konstantní hodnotou.</span><span class="sxs-lookup"><span data-stu-id="4e633-141">A constant pattern tests the value of an expression against a constant value.</span></span> <span data-ttu-id="4e633-142">Konstanta může být libovolný konstantní výraz, jako je například literál, název deklarované `const` proměnné nebo konstanta výčtu nebo výraz `typeof`.</span><span class="sxs-lookup"><span data-stu-id="4e633-142">The constant may be any constant expression, such as a literal, the name of a declared `const` variable, or an enumeration constant, or a `typeof` expression.</span></span>

<span data-ttu-id="4e633-143">Pokud jsou jak *e* , tak *c* celočíselné typy, vzor se považuje za odpovídající, pokud je výsledek výrazu `e == c` `true`.</span><span class="sxs-lookup"><span data-stu-id="4e633-143">If both *e* and *c* are of integral types, the pattern is considered matched if the result of the expression `e == c` is `true`.</span></span>

<span data-ttu-id="4e633-144">V opačném případě se vzor považuje za shodný, pokud `object.Equals(e, c)` vrátí `true`.</span><span class="sxs-lookup"><span data-stu-id="4e633-144">Otherwise the pattern is considered matching if `object.Equals(e, c)` returns `true`.</span></span> <span data-ttu-id="4e633-145">V tomto případě se jedná o chybu při kompilaci, pokud statický typ *e* není *vzor kompatibilní* s typem konstanty.</span><span class="sxs-lookup"><span data-stu-id="4e633-145">In this case it is a compile-time error if the static type of *e* is not *pattern compatible* with the type of the constant.</span></span>

### <a name="var-pattern"></a><span data-ttu-id="4e633-146">variabilní vzorek</span><span class="sxs-lookup"><span data-stu-id="4e633-146">Var pattern</span></span>

```antlr
var_pattern
    : 'var' simple_designation
    ;
```

<span data-ttu-id="4e633-147">Výraz *e* se shoduje s *var_pattern* vždy.</span><span class="sxs-lookup"><span data-stu-id="4e633-147">An expression *e* matches a *var_pattern* always.</span></span> <span data-ttu-id="4e633-148">Jinými slovy, porovnávání se *vzorkem var* vždy proběhne úspěšně.</span><span class="sxs-lookup"><span data-stu-id="4e633-148">In other words, a match to a *var pattern* always succeeds.</span></span> <span data-ttu-id="4e633-149">Pokud je *simple_designation* identifikátor, pak za běhu je hodnota *e* vázána na nově zavedenou místní proměnnou.</span><span class="sxs-lookup"><span data-stu-id="4e633-149">If the *simple_designation* is an identifier, then at runtime the value of *e* is bound to a newly introduced local variable.</span></span> <span data-ttu-id="4e633-150">Typ lokální proměnné je statický typ *e*.</span><span class="sxs-lookup"><span data-stu-id="4e633-150">The type of the local variable is the static type of *e*.</span></span>

<span data-ttu-id="4e633-151">Pokud se název `var` váže k typu, jedná se o chybu.</span><span class="sxs-lookup"><span data-stu-id="4e633-151">It is an error if the name `var` binds to a type.</span></span>

## <a name="switch-statement"></a><span data-ttu-id="4e633-152">Příkaz switch</span><span class="sxs-lookup"><span data-stu-id="4e633-152">Switch statement</span></span>

<span data-ttu-id="4e633-153">Příkaz `switch` je rozšířen o výběr pro spuštění prvního bloku, který má přidružený vzor, který odpovídá *výrazu přepínače*.</span><span class="sxs-lookup"><span data-stu-id="4e633-153">The `switch` statement is extended to select for execution the first block having an associated pattern that matches the *switch expression*.</span></span>

```antlr
switch_label
    : 'case' complex_pattern case_guard? ':'
    | 'case' constant_expression case_guard? ':'
    | 'default' ':'
    ;

case_guard
    : 'when' expression
    ;
```

<span data-ttu-id="4e633-154">Pořadí, ve kterém se vzorce shodují, není definováno.</span><span class="sxs-lookup"><span data-stu-id="4e633-154">The order in which patterns are matched is not defined.</span></span> <span data-ttu-id="4e633-155">Kompilátor je povolený pro porovnávání vzorů mimo pořadí a opakované použití výsledků už odpovídajících vzorů k výpočtu výsledku porovnávání jiných vzorů.</span><span class="sxs-lookup"><span data-stu-id="4e633-155">A compiler is permitted to match patterns out of order, and to reuse the results of already matched patterns to compute the result of matching of other patterns.</span></span>

<span data-ttu-id="4e633-156">Pokud je přítomen *případ-Guard* , je jeho výraz typu `bool`.</span><span class="sxs-lookup"><span data-stu-id="4e633-156">If a *case-guard* is present, its expression is of type `bool`.</span></span> <span data-ttu-id="4e633-157">Je vyhodnocen jako další podmínka, která musí být splněna, aby byl případ považován za splněný.</span><span class="sxs-lookup"><span data-stu-id="4e633-157">It is evaluated as an additional condition that must be satisfied for the case to be considered satisfied.</span></span>

<span data-ttu-id="4e633-158">Jedná se o chybu, pokud *switch_label* nemůže mít za běhu žádný účinek, protože jeho vzor je subsumed v předchozích případech.</span><span class="sxs-lookup"><span data-stu-id="4e633-158">It is an error if a *switch_label* can have no effect at runtime because its pattern is subsumed by previous cases.</span></span> <span data-ttu-id="4e633-159">[TODO: máme přesnější informace o technikách, které kompilátor potřebuje k dosažení tohoto rozhodnutí.]</span><span class="sxs-lookup"><span data-stu-id="4e633-159">[TODO: We should be more precise about the techniques the compiler is required to use to reach this judgment.]</span></span>

<span data-ttu-id="4e633-160">Proměnná vzoru deklarovaná v *switch_label* je jednoznačně přiřazena v bloku Case, pokud a pouze v případě, že tento blok případu obsahuje přesně jeden *switch_label*.</span><span class="sxs-lookup"><span data-stu-id="4e633-160">A pattern variable declared in a *switch_label* is definitely assigned in its case block if and only if that case block contains precisely one *switch_label*.</span></span>

<span data-ttu-id="4e633-161">[TODO: je potřeba určit, kdy má být *blok přepínače* dosažitelný.]</span><span class="sxs-lookup"><span data-stu-id="4e633-161">[TODO: We should specify when a *switch block* is reachable.]</span></span>

### <a name="scope-of-pattern-variables"></a><span data-ttu-id="4e633-162">Rozsah proměnných vzoru</span><span class="sxs-lookup"><span data-stu-id="4e633-162">Scope of pattern variables</span></span>

<span data-ttu-id="4e633-163">Rozsah proměnné deklarované ve vzoru je následující:</span><span class="sxs-lookup"><span data-stu-id="4e633-163">The scope of a variable declared in a pattern is as follows:</span></span>

- <span data-ttu-id="4e633-164">Pokud je vzorek jmenovka Case, pak obor proměnné je *blok případu*.</span><span class="sxs-lookup"><span data-stu-id="4e633-164">If the pattern is a case label, then the scope of the variable is the *case block*.</span></span>

<span data-ttu-id="4e633-165">V opačném případě je proměnná deklarována ve výrazu *is_pattern* a její obor je založen na konstruktoru, který je okamžitě ohraničený výrazem, který obsahuje výraz *is_pattern* následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="4e633-165">Otherwise the variable is declared in an *is_pattern* expression, and its scope is based on the construct immediately enclosing the expression containing the *is_pattern* expression as follows:</span></span>

- <span data-ttu-id="4e633-166">Pokud je výraz ve výrazu lambda s výrazem těle, jeho rozsah je tělo lambda.</span><span class="sxs-lookup"><span data-stu-id="4e633-166">If the expression is in an expression-bodied lambda, its scope is the body of the lambda.</span></span>
- <span data-ttu-id="4e633-167">Pokud je výraz v metodě nebo vlastnosti těle výrazu, jeho obor je tělo metody nebo vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="4e633-167">If the expression is in an expression-bodied method or property, its scope is the body of the method or property.</span></span>
- <span data-ttu-id="4e633-168">Pokud je výraz v klauzuli `when` klauzule `catch`, je jeho oborem klauzule `catch`.</span><span class="sxs-lookup"><span data-stu-id="4e633-168">If the expression is in a `when` clause of a `catch` clause, its scope is that `catch` clause.</span></span>
- <span data-ttu-id="4e633-169">Pokud je výraz ve *iteration_statement*, jeho obor je pouze tento příkaz.</span><span class="sxs-lookup"><span data-stu-id="4e633-169">If the expression is in an *iteration_statement*, its scope is just that statement.</span></span>
- <span data-ttu-id="4e633-170">Jinak, pokud je výraz v nějakém jiném formuláři příkazu, jeho rozsah je obor obsahující příkaz.</span><span class="sxs-lookup"><span data-stu-id="4e633-170">Otherwise if the expression is in some other statement form, its scope is the scope containing the statement.</span></span>

<span data-ttu-id="4e633-171">Pro účely určení rozsahu se *embedded_statement* považuje za jeho vlastní obor.</span><span class="sxs-lookup"><span data-stu-id="4e633-171">For the purpose of determining the scope, an *embedded_statement* is considered to be in its own scope.</span></span> <span data-ttu-id="4e633-172">Například gramatika pro *if_statement* je</span><span class="sxs-lookup"><span data-stu-id="4e633-172">For example, the grammar for an *if_statement* is</span></span>

``` antlr
if_statement
    : 'if' '(' boolean_expression ')' embedded_statement
    | 'if' '(' boolean_expression ')' embedded_statement 'else' embedded_statement
    ;
```

<span data-ttu-id="4e633-173">Takže pokud řízený příkaz *if_statement* deklaruje proměnnou vzoru, je její obor omezen na tento *embedded_statement*:</span><span class="sxs-lookup"><span data-stu-id="4e633-173">So if the controlled statement of an *if_statement* declares a pattern variable, its scope is restricted to that *embedded_statement*:</span></span>

```csharp
if (x) M(y is var z);
```

<span data-ttu-id="4e633-174">V tomto případě je rozsah `z` vloženým příkazem `M(y is var z);`.</span><span class="sxs-lookup"><span data-stu-id="4e633-174">In this case the scope of `z` is the embedded statement `M(y is var z);`.</span></span>

<span data-ttu-id="4e633-175">Jiné případy jsou chyby z jiných důvodů (například ve výchozí hodnotě parametru nebo atributu, z nichž obě jsou chyby, protože tyto kontexty vyžadují konstantní výraz).</span><span class="sxs-lookup"><span data-stu-id="4e633-175">Other cases are errors for other reasons (e.g. in a parameter's default value or an attribute, both of which are an error because those contexts require a constant expression).</span></span>

> <span data-ttu-id="4e633-176">[V C# 7,3 jsme přidali následující kontexty](../csharp-7.3/expression-variables-in-initializers.md) , ve kterých může být deklarována proměnná vzoru:</span><span class="sxs-lookup"><span data-stu-id="4e633-176">[In C# 7.3 we added the following contexts](../csharp-7.3/expression-variables-in-initializers.md) in which a pattern variable may be declared:</span></span>
> - <span data-ttu-id="4e633-177">Pokud je výraz v *inicializátoru konstruktoru*, jeho obor je *inicializátor konstruktoru* a tělo konstruktoru.</span><span class="sxs-lookup"><span data-stu-id="4e633-177">If the expression is in a *constructor initializer*, its scope is the *constructor initializer* and the constructor's body.</span></span>
> - <span data-ttu-id="4e633-178">Pokud je výraz v inicializátoru pole, jeho obor je *equals_value_clause* , ve kterém se zobrazí.</span><span class="sxs-lookup"><span data-stu-id="4e633-178">If the expression is in a field initializer, its scope is the *equals_value_clause* in which it appears.</span></span>
> - <span data-ttu-id="4e633-179">Pokud je výraz v klauzuli dotazu, která je určena k překladu do těla výrazu lambda, jeho obor je pouze tento výraz.</span><span class="sxs-lookup"><span data-stu-id="4e633-179">If the expression is in a query clause that is specified to be translated into the body of a lambda, its scope is just that expression.</span></span>

## <a name="changes-to-syntactic-disambiguation"></a><span data-ttu-id="4e633-180">Změny syntaktického nejednoznačnosti</span><span class="sxs-lookup"><span data-stu-id="4e633-180">Changes to syntactic disambiguation</span></span>

<span data-ttu-id="4e633-181">Existují situace zahrnující obecné typy, ve kterých C# je gramatika nejednoznačná, a specifikace jazyka říká, jak tyto nejednoznačnosti vyřešit:</span><span class="sxs-lookup"><span data-stu-id="4e633-181">There are situations involving generics where the C# grammar is ambiguous, and the language spec says how to resolve those ambiguities:</span></span>

> #### <a name="7652-grammar-ambiguities"></a><span data-ttu-id="4e633-182">7.6.5.2 gramatické nejednoznačnosti</span><span class="sxs-lookup"><span data-stu-id="4e633-182">7.6.5.2 Grammar ambiguities</span></span>
> <span data-ttu-id="4e633-183">Výroby pro *jednoduché názvy* (§ 7.6.3) a *Member-Access* (§ 7.6.5) mohou vést k nejednoznačnosti v gramatice pro výrazy.</span><span class="sxs-lookup"><span data-stu-id="4e633-183">The productions for *simple-name* (§7.6.3) and *member-access* (§7.6.5) can give rise to ambiguities in the grammar for expressions.</span></span> <span data-ttu-id="4e633-184">Například příkaz:</span><span class="sxs-lookup"><span data-stu-id="4e633-184">For example, the statement:</span></span>
> ```csharp
> F(G<A,B>(7));
> ```
> <span data-ttu-id="4e633-185">může být interpretován jako volání `F` se dvěma argumenty, `G < A` a `B > (7)`.</span><span class="sxs-lookup"><span data-stu-id="4e633-185">could be interpreted as a call to `F` with two arguments, `G < A` and `B > (7)`.</span></span> <span data-ttu-id="4e633-186">Alternativně může být interpretován jako volání `F` s jedním argumentem, což je volání obecné metody `G` se dvěma argumenty typu a jedním regulárním argumentem.</span><span class="sxs-lookup"><span data-stu-id="4e633-186">Alternatively, it could be interpreted as a call to `F` with one argument, which is a call to a generic method `G` with two type arguments and one regular argument.</span></span>

> <span data-ttu-id="4e633-187">V případě, že je možné sekvenci tokenů analyzovat (v kontextu) jako *jednoduchý název* (§ 7.6.3), *přístup k členu* (§ 7.6.5) nebo *ukazatel-Member-Access* (§ 18.5.2) končící na *typ-argument-list* (§ 4.4.1), je přezkoumán token hned po ukončení `>` tokenu.</span><span class="sxs-lookup"><span data-stu-id="4e633-187">If a sequence of tokens can be parsed (in context) as a *simple-name* (§7.6.3), *member-access* (§7.6.5), or *pointer-member-access* (§18.5.2) ending with a *type-argument-list* (§4.4.1), the token immediately following the closing `>` token is examined.</span></span> <span data-ttu-id="4e633-188">Pokud je jeden z</span><span class="sxs-lookup"><span data-stu-id="4e633-188">If it is one of</span></span>
> ```none
> (  )  ]  }  :  ;  ,  .  ?  ==  !=  |  ^
> ```
> <span data-ttu-id="4e633-189">pak je *typ-argument-seznam* uložen jako součást *jednoduchého názvu*, přístupu *člena* nebo *ukazatele-člena* a jakékoli další možné analýzy sekvence tokenů je zahozena.</span><span class="sxs-lookup"><span data-stu-id="4e633-189">then the *type-argument-list* is retained as part of the *simple-name*, *member-access* or *pointer-member-access* and any other possible parse of the sequence of tokens is discarded.</span></span> <span data-ttu-id="4e633-190">V opačném případě se *seznam argumentů typu* nepovažuje za součást *jednoduchého názvu*, *přístupu k členu* nebo > ukazatel na *člena*, a to i v případě, že není možné analyzovat sekvenci tokenů.</span><span class="sxs-lookup"><span data-stu-id="4e633-190">Otherwise, the *type-argument-list* is not considered to be part of the *simple-name*, *member-access* or > *pointer-member-access*, even if there is no other possible parse of the sequence of tokens.</span></span> <span data-ttu-id="4e633-191">Všimněte si, že tato pravidla nejsou aplikována při analýze *typu seznamu-argumentu* v *oboru názvů nebo názvu typu* (§ 3,8).</span><span class="sxs-lookup"><span data-stu-id="4e633-191">Note that these rules are not applied when parsing a *type-argument-list* in a *namespace-or-type-name* (§3.8).</span></span> <span data-ttu-id="4e633-192">Příkaz</span><span class="sxs-lookup"><span data-stu-id="4e633-192">The statement</span></span>
> ```csharp
> F(G<A,B>(7));
> ```
> <span data-ttu-id="4e633-193">bude podle tohoto pravidla interpretována jako volání `F` s jedním argumentem, což je volání obecné metody `G` se dvěma argumenty typu a jedním regulárním argumentem.</span><span class="sxs-lookup"><span data-stu-id="4e633-193">will, according to this rule, be interpreted as a call to `F` with one argument, which is a call to a generic method `G` with two type arguments and one regular argument.</span></span> <span data-ttu-id="4e633-194">Příkazy</span><span class="sxs-lookup"><span data-stu-id="4e633-194">The statements</span></span>
> ```csharp
> F(G < A, B > 7);
> F(G < A, B >> 7);
> ```
> <span data-ttu-id="4e633-195">každý bude interpretován jako volání `F` se dvěma argumenty.</span><span class="sxs-lookup"><span data-stu-id="4e633-195">will each be interpreted as a call to `F` with two arguments.</span></span> <span data-ttu-id="4e633-196">Příkaz</span><span class="sxs-lookup"><span data-stu-id="4e633-196">The statement</span></span>
> ```csharp
> x = F < A > +y;
> ```
> <span data-ttu-id="4e633-197">bude interpretován jako operátor menší než, větší než a unární operátor plus, jako by byl například zápis příkazu `x = (F < A) > (+y)`, namísto jako *jednoduchého názvu* se *seznamem argumentů typu* a následovaným binárním operátorem Plus.</span><span class="sxs-lookup"><span data-stu-id="4e633-197">will be interpreted as a less than operator, greater than operator, and unary plus operator, as if the statement had been written `x = (F < A) > (+y)`, instead of as a *simple-name* with a *type-argument-list* followed by a binary plus operator.</span></span> <span data-ttu-id="4e633-198">V příkazu</span><span class="sxs-lookup"><span data-stu-id="4e633-198">In the statement</span></span>
> ```csharp
> x = y is C<T> + z;
> ```
> <span data-ttu-id="4e633-199">tokeny `C<T>` jsou interpretovány jako *název oboru názvů nebo názvu* pomocí *seznamu typu argument-seznam*.</span><span class="sxs-lookup"><span data-stu-id="4e633-199">the tokens `C<T>` are interpreted as a *namespace-or-type-name* with a *type-argument-list*.</span></span>

<span data-ttu-id="4e633-200">V C# 7 se zavádí několik změn, které tyto pravidla pro odstraňování nejasnosti neumožňují zvládnout složitost jazyka.</span><span class="sxs-lookup"><span data-stu-id="4e633-200">There are a number of changes being introduced in C# 7 that make these disambiguation rules no longer sufficient to handle the complexity of the language.</span></span>

### <a name="out-variable-declarations"></a><span data-ttu-id="4e633-201">Deklarace proměnné out</span><span class="sxs-lookup"><span data-stu-id="4e633-201">Out variable declarations</span></span>

<span data-ttu-id="4e633-202">Nyní je možné deklarovat proměnnou v argumentu out:</span><span class="sxs-lookup"><span data-stu-id="4e633-202">It is now possible to declare a variable in an out argument:</span></span>

```csharp
M(out Type name);
```

<span data-ttu-id="4e633-203">Typ ale může být obecný:</span><span class="sxs-lookup"><span data-stu-id="4e633-203">However, the type may be generic:</span></span> 

```csharp
M(out A<B> name);
```

<span data-ttu-id="4e633-204">Vzhledem k tomu, že gramatika jazyka argumentu používá *výraz*, podléhá tomuto kontextu pravidlo pro nejednoznačnosti.</span><span class="sxs-lookup"><span data-stu-id="4e633-204">Since the language grammar for the argument uses *expression*, this context is subject to the disambiguation rule.</span></span> <span data-ttu-id="4e633-205">V tomto případě uzavírací `>` následuje *identifikátor*, což není jedna z tokenů, která umožňuje, aby byla zpracována jako *seznam argumentů typu*.</span><span class="sxs-lookup"><span data-stu-id="4e633-205">In this case the closing `>` is followed by an *identifier*, which is not one of the tokens that permits it to be treated as a *type-argument-list*.</span></span> <span data-ttu-id="4e633-206">Proto navrhnem **Přidání *identifikátoru* do sady tokenů, které aktivují nejednoznačnost na *seznam typů argumentů*.**</span><span class="sxs-lookup"><span data-stu-id="4e633-206">I therefore propose to **add *identifier* to the set of tokens that triggers the disambiguation to a *type-argument-list*.**</span></span>

### <a name="tuples-and-deconstruction-declarations"></a><span data-ttu-id="4e633-207">Řazené kolekce členů a prohlášení o dekonstrukci</span><span class="sxs-lookup"><span data-stu-id="4e633-207">Tuples and deconstruction declarations</span></span>

<span data-ttu-id="4e633-208">Literál řazené kolekce členů funguje v přesně stejném problému.</span><span class="sxs-lookup"><span data-stu-id="4e633-208">A tuple literal runs into exactly the same issue.</span></span> <span data-ttu-id="4e633-209">Vzít v úvahu výraz řazené kolekce členů</span><span class="sxs-lookup"><span data-stu-id="4e633-209">Consider the tuple expression</span></span>

```csharp
(A < B, C > D, E < F, G > H)
```

<span data-ttu-id="4e633-210">V rámci starých C# 6 pravidel pro analýzu seznamu argumentů by se tato analýza mohla analyzovat jako řazená kolekce členů se čtyřmi prvky, počínaje `A < B` jako první.</span><span class="sxs-lookup"><span data-stu-id="4e633-210">Under the old C# 6 rules for parsing an argument list, this would parse as a tuple with four elements, starting with `A < B` as the first.</span></span> <span data-ttu-id="4e633-211">Pokud se však zobrazí na levé straně dekonstrukce, chceme, aby došlo k nejasnostem aktivovaném tokenem *identifikátoru* , jak je popsáno výše:</span><span class="sxs-lookup"><span data-stu-id="4e633-211">However, when this appears on the left of a deconstruction, we want the disambiguation triggered by the *identifier* token as described above:</span></span>

```csharp
(A<B,C> D, E<F,G> H) = e;
```

<span data-ttu-id="4e633-212">Toto je deklarace dekonstrukce, která deklaruje dvě proměnné, první z nich je typu `A<B,C>` a pojmenovaný `D`.</span><span class="sxs-lookup"><span data-stu-id="4e633-212">This is a deconstruction declaration which declares two variables, the first of which is of type `A<B,C>` and named `D`.</span></span> <span data-ttu-id="4e633-213">Jinými slovy literál řazené kolekce členů obsahuje dva výrazy, z nichž každý je výraz deklarace.</span><span class="sxs-lookup"><span data-stu-id="4e633-213">In other words, the tuple literal contains two expressions, each of which is a declaration expression.</span></span>

<span data-ttu-id="4e633-214">Pro jednoduchost specifikace a kompilátoru navrhuji, aby se tento literál řazené kolekce členů analyzoval jako řazená kolekce členů se dvěma prvky, ať už se zobrazuje, a to bez ohledu na to, jestli se zobrazuje na levé straně přiřazení.</span><span class="sxs-lookup"><span data-stu-id="4e633-214">For simplicity of the specification and compiler, I propose that this tuple literal be parsed as a two-element tuple wherever it appears (whether or not it appears on the left-hand-side of an assignment).</span></span> <span data-ttu-id="4e633-215">To by představovalo přirozený výsledek nejasnosti popsané v předchozí části.</span><span class="sxs-lookup"><span data-stu-id="4e633-215">That would be a natural result of the disambiguation described in the previous section.</span></span>

### <a name="pattern-matching"></a><span data-ttu-id="4e633-216">Porovnávání vzorů</span><span class="sxs-lookup"><span data-stu-id="4e633-216">Pattern-matching</span></span>

<span data-ttu-id="4e633-217">Porovnávání vzorů zavádí nový kontext, ve kterém vznikne nejednoznačnost typu výrazu.</span><span class="sxs-lookup"><span data-stu-id="4e633-217">Pattern matching introduces a new context where the expression-type ambiguity arises.</span></span> <span data-ttu-id="4e633-218">Dřív byla na pravé straně operátoru `is` typ.</span><span class="sxs-lookup"><span data-stu-id="4e633-218">Previously the right-hand-side of an `is` operator was a type.</span></span> <span data-ttu-id="4e633-219">Nyní může být typ nebo výraz a pokud se jedná o typ, za kterým může následovat identifikátor.</span><span class="sxs-lookup"><span data-stu-id="4e633-219">Now it can be a type or expression, and if it is a type it may be followed by an identifier.</span></span> <span data-ttu-id="4e633-220">To může technicky změnit význam stávajícího kódu:</span><span class="sxs-lookup"><span data-stu-id="4e633-220">This can, technically, change the meaning of existing code:</span></span>

```csharp
var x = e is T < A > B;
```

<span data-ttu-id="4e633-221">To se dá analyzovat podle pravidel C# 6 jako</span><span class="sxs-lookup"><span data-stu-id="4e633-221">This could be parsed under C#6 rules as</span></span>

```csharp
var x = ((e is T) < A) > B;
```

<span data-ttu-id="4e633-222">ale v části pod pravidly C# 7 (s navrhovanou nejednoznačnosti) se analyzují jako</span><span class="sxs-lookup"><span data-stu-id="4e633-222">but under under C#7 rules (with the disambiguation proposed above) would be parsed as</span></span>

```csharp
var x = e is T<A> B;
```

<span data-ttu-id="4e633-223">který deklaruje proměnnou `B` typu `T<A>`.</span><span class="sxs-lookup"><span data-stu-id="4e633-223">which declares a variable `B` of type `T<A>`.</span></span> <span data-ttu-id="4e633-224">Naštěstí nativní a Roslyn kompilátory mají chybu, při které poskytnou chybu syntaxe v kódu C# 6.</span><span class="sxs-lookup"><span data-stu-id="4e633-224">Fortunately, the native and Roslyn compilers have a bug whereby they give a syntax error on the C#6 code.</span></span> <span data-ttu-id="4e633-225">Tato konkrétní zásadní změna tedy nezáleží na tom.</span><span class="sxs-lookup"><span data-stu-id="4e633-225">Therefore this particular breaking change is not a concern.</span></span>

<span data-ttu-id="4e633-226">Porovnávání vzorů zavádí další tokeny, které by měly vyřídit rozlišení nejednoznačnosti směrem k výběru typu.</span><span class="sxs-lookup"><span data-stu-id="4e633-226">Pattern-matching introduces additional tokens that should drive the ambiguity resolution toward selecting a type.</span></span> <span data-ttu-id="4e633-227">Následující příklady stávajícího kódu v C# 6 budou přerušeny bez dalších pravidel pro nejednoznačnost:</span><span class="sxs-lookup"><span data-stu-id="4e633-227">The following examples of existing valid C#6 code would be broken without additional disambiguation rules:</span></span>

```csharp
var x = e is A<B> && f;            // &&
var x = e is A<B> || f;            // ||
var x = e is A<B> & f;             // &
var x = e is A<B>[];               // [
```

### <a name="proposed-change-to-the-disambiguation-rule"></a><span data-ttu-id="4e633-228">Navrhovaná změna pravidla nejednoznačnosti</span><span class="sxs-lookup"><span data-stu-id="4e633-228">Proposed change to the disambiguation rule</span></span>

<span data-ttu-id="4e633-229">Navrhuji revizi specifikace a změnit seznam nejednoznačných tokenů z</span><span class="sxs-lookup"><span data-stu-id="4e633-229">I propose to revise the specification to change the list of disambiguating tokens from</span></span>

>
```none
(  )  ]  }  :  ;  ,  .  ?  ==  !=  |  ^
```

<span data-ttu-id="4e633-230">na</span><span class="sxs-lookup"><span data-stu-id="4e633-230">to</span></span>

>
```none
(  )  ]  }  :  ;  ,  .  ?  ==  !=  |  ^  &&  ||  &  [
```

<span data-ttu-id="4e633-231">A v některých kontextech považujeme *identifikátor* za nejednoznačnost tokenu.</span><span class="sxs-lookup"><span data-stu-id="4e633-231">And, in certain contexts, we treat *identifier* as a disambiguating token.</span></span> <span data-ttu-id="4e633-232">Tyto kontexty jsou místo, kde se nejednoznačná sekvence tokenů nachází bezprostředně před jedním z klíčových slov `is`, `case`nebo `out`, nebo při analýze prvního prvku literálu řazené kolekce členů (v takovém případě tokeny předcházejí `(` nebo `:` a identifikátor je následován `,`) nebo následným prvkem literálu řazené kolekce členů.</span><span class="sxs-lookup"><span data-stu-id="4e633-232">Those contexts are where the sequence of tokens being disambiguated is immediately preceded by one of the keywords `is`, `case`, or `out`, or arises while parsing the first element of a tuple literal (in which case the tokens are preceded by `(` or `:` and the identifier is followed by a `,`) or a subsequent element of a tuple literal.</span></span>

### <a name="modified-disambiguation-rule"></a><span data-ttu-id="4e633-233">Změněné pravidlo pro nejednoznačnosti</span><span class="sxs-lookup"><span data-stu-id="4e633-233">Modified disambiguation rule</span></span>

<span data-ttu-id="4e633-234">Změněné pravidlo pro nejednoznačnosti by mohlo vypadat nějak takto.</span><span class="sxs-lookup"><span data-stu-id="4e633-234">The revised disambiguation rule would be something like this</span></span>

> <span data-ttu-id="4e633-235">Pokud je možné sekvenci tokenů analyzovat (v kontextu) jako *jednoduchý název* (§ 7.6.3), *přístup pro member* (§ 7.6.5) nebo *ukazatel-Member-Access* (§ 18.5.2) končící na *seznam argumentů typu* (§ 4.4.1), token hned za ukončovacím `>` token se vyšetří, pokud je</span><span class="sxs-lookup"><span data-stu-id="4e633-235">If a sequence of tokens can be parsed (in context) as a *simple-name* (§7.6.3), *member-access* (§7.6.5), or *pointer-member-access* (§18.5.2) ending with a *type-argument-list* (§4.4.1), the token immediately following the closing `>` token is examined, to see if it is</span></span>
> - <span data-ttu-id="4e633-236">Jedna z `(  )  ]  }  :  ;  ,  .  ?  ==  !=  |  ^  &&  ||  &  [`; ani</span><span class="sxs-lookup"><span data-stu-id="4e633-236">One of `(  )  ]  }  :  ;  ,  .  ?  ==  !=  |  ^  &&  ||  &  [`; or</span></span>
> - <span data-ttu-id="4e633-237">Jeden z relačních operátorů `<  >  <=  >=  is as`; ani</span><span class="sxs-lookup"><span data-stu-id="4e633-237">One of the relational operators `<  >  <=  >=  is as`; or</span></span>
> - <span data-ttu-id="4e633-238">Kontextové klíčové slovo dotazu, které se zobrazuje ve výrazu dotazu; ani</span><span class="sxs-lookup"><span data-stu-id="4e633-238">A contextual query keyword appearing inside a query expression; or</span></span>
> - <span data-ttu-id="4e633-239">V některých kontextech považujeme *identifikátor* za nejednoznačný token.</span><span class="sxs-lookup"><span data-stu-id="4e633-239">In certain contexts, we treat *identifier* as a disambiguating token.</span></span> <span data-ttu-id="4e633-240">Tyto kontexty jsou místo, kde se nejednoznačná sekvence tokenů nachází bezprostředně před jedním z klíčových slov `is`, `case` nebo `out`, nebo při analýze prvního prvku literálu řazené kolekce členů (v takovém případě tokeny předcházejí `(` nebo `:` a identifikátor je následován `,`) nebo následným prvkem literálu řazené kolekce členů.</span><span class="sxs-lookup"><span data-stu-id="4e633-240">Those contexts are where the sequence of tokens being disambiguated is immediately preceded by one of the keywords `is`, `case` or `out`, or arises while parsing the first element of a tuple literal (in which case the tokens are preceded by `(` or `:` and the identifier is followed by a `,`) or a subsequent element of a tuple literal.</span></span>
> 
> <span data-ttu-id="4e633-241">Pokud je tento token mezi seznamem nebo identifikátorem v takovém kontextu, pak je *seznam typu argument-seznam* uložen jako součást *jednoduchého názvu*, *přístupu člena* nebo *ukazatele-člena* a jakékoli další možné analýzy sekvence tokenů je zahozena.</span><span class="sxs-lookup"><span data-stu-id="4e633-241">If the following token is among this list, or an identifier in such a context, then the *type-argument-list* is retained as part of the *simple-name*, *member-access* or  *pointer-member-access* and any other possible parse of the sequence of tokens is discarded.</span></span>  <span data-ttu-id="4e633-242">V opačném případě *typ argument-seznam* není považován za součást *jednoduchého názvu*, *přístupu k členu* nebo *ukazateli na člena*, a to i v případě, že neexistuje jiné možné analýzy sekvence tokenů.</span><span class="sxs-lookup"><span data-stu-id="4e633-242">Otherwise, the *type-argument-list* is not considered to be part of the *simple-name*, *member-access* or *pointer-member-access*, even if there is no other possible parse of the sequence of tokens.</span></span> <span data-ttu-id="4e633-243">Všimněte si, že tato pravidla nejsou aplikována při analýze *typu seznamu-argumentu* v *oboru názvů nebo názvu typu* (§ 3,8).</span><span class="sxs-lookup"><span data-stu-id="4e633-243">Note that these rules are not applied when parsing a *type-argument-list* in a *namespace-or-type-name* (§3.8).</span></span>

### <a name="breaking-changes-due-to-this-proposal"></a><span data-ttu-id="4e633-244">Zásadní změny způsobené tímto návrhem</span><span class="sxs-lookup"><span data-stu-id="4e633-244">Breaking changes due to this proposal</span></span>

<span data-ttu-id="4e633-245">Z důvodu tohoto navrhovaného pravidla pro zrušení nejednoznačnosti nejsou známy žádné průlomové změny.</span><span class="sxs-lookup"><span data-stu-id="4e633-245">No breaking changes are known due to this proposed disambiguation rule.</span></span>

### <a name="interesting-examples"></a><span data-ttu-id="4e633-246">Zajímavé příklady</span><span class="sxs-lookup"><span data-stu-id="4e633-246">Interesting examples</span></span>

<span data-ttu-id="4e633-247">Tady je několik zajímavých výsledků těchto pravidel pro nejednoznačnost:</span><span class="sxs-lookup"><span data-stu-id="4e633-247">Here are some interesting results of these disambiguation rules:</span></span>

<span data-ttu-id="4e633-248">Výraz `(A < B, C > D)` je řazená kolekce členů se dvěma prvky, každé porovnání.</span><span class="sxs-lookup"><span data-stu-id="4e633-248">The expression `(A < B, C > D)` is a tuple with two elements, each a comparison.</span></span>

<span data-ttu-id="4e633-249">Výraz `(A<B,C> D, E)` je řazená kolekce členů se dvěma prvky, první z nich je výraz deklarace.</span><span class="sxs-lookup"><span data-stu-id="4e633-249">The expression `(A<B,C> D, E)` is a tuple with two elements, the first of which is a declaration expression.</span></span>

<span data-ttu-id="4e633-250">`M(A < B, C > D, E)` volání má tři argumenty.</span><span class="sxs-lookup"><span data-stu-id="4e633-250">The invocation `M(A < B, C > D, E)` has three arguments.</span></span>

<span data-ttu-id="4e633-251">`M(out A<B,C> D, E)` vyvolání má dva argumenty, přičemž první z nich je deklarace `out`.</span><span class="sxs-lookup"><span data-stu-id="4e633-251">The invocation `M(out A<B,C> D, E)` has two arguments, the first of which is an `out` declaration.</span></span>

<span data-ttu-id="4e633-252">Výraz `e is A<B> C` používá výraz deklarace.</span><span class="sxs-lookup"><span data-stu-id="4e633-252">The expression `e is A<B> C` uses a declaration expression.</span></span>

<span data-ttu-id="4e633-253">Popisek případu `case A<B> C:` používá výraz deklarace.</span><span class="sxs-lookup"><span data-stu-id="4e633-253">The case label `case A<B> C:` uses a declaration expression.</span></span>

## <a name="some-examples-of-pattern-matching"></a><span data-ttu-id="4e633-254">Příklady porovnávání vzorů</span><span class="sxs-lookup"><span data-stu-id="4e633-254">Some examples of pattern matching</span></span>

### <a name="is-as"></a><span data-ttu-id="4e633-255">Je as</span><span class="sxs-lookup"><span data-stu-id="4e633-255">Is-As</span></span>

<span data-ttu-id="4e633-256">Idiom můžeme nahradit</span><span class="sxs-lookup"><span data-stu-id="4e633-256">We can replace the idiom</span></span>

```csharp
var v = expr as Type;   
if (v != null) {
    // code using v
}
```

<span data-ttu-id="4e633-257">S mírným výstižným a přímým přístupem</span><span class="sxs-lookup"><span data-stu-id="4e633-257">With the slightly more concise and direct</span></span>

```csharp
if (expr is Type v) {
    // code using v
}
```

### <a name="testing-nullable"></a><span data-ttu-id="4e633-258">Testování s možnou hodnotou null</span><span class="sxs-lookup"><span data-stu-id="4e633-258">Testing nullable</span></span>

<span data-ttu-id="4e633-259">Idiom můžeme nahradit</span><span class="sxs-lookup"><span data-stu-id="4e633-259">We can replace the idiom</span></span>

```csharp
Type? v = x?.y?.z;
if (v.HasValue) {
    var value = v.GetValueOrDefault();
    // code using value
}
```

<span data-ttu-id="4e633-260">S mírným výstižným a přímým přístupem</span><span class="sxs-lookup"><span data-stu-id="4e633-260">With the slightly more concise and direct</span></span>

```csharp
if (x?.y?.z is Type value) {
    // code using value
}
```

### <a name="arithmetic-simplification"></a><span data-ttu-id="4e633-261">Aritmetické zjednodušení</span><span class="sxs-lookup"><span data-stu-id="4e633-261">Arithmetic simplification</span></span>

<span data-ttu-id="4e633-262">Předpokládejme, že definujeme sadu rekurzivních typů, které reprezentují výrazy (na samostatný návrh):</span><span class="sxs-lookup"><span data-stu-id="4e633-262">Suppose we define a set of recursive types to represent expressions (per a separate proposal):</span></span>

```csharp
abstract class Expr;
class X() : Expr;
class Const(double Value) : Expr;
class Add(Expr Left, Expr Right) : Expr;
class Mult(Expr Left, Expr Right) : Expr;
class Neg(Expr Value) : Expr;
```

<span data-ttu-id="4e633-263">Nyní můžeme definovat funkci pro výpočet (nesnížený) odvození výrazu:</span><span class="sxs-lookup"><span data-stu-id="4e633-263">Now we can define a function to compute the (unreduced) derivative of an expression:</span></span>

```csharp
Expr Deriv(Expr e)
{
  switch (e) {
    case X(): return Const(1);
    case Const(*): return Const(0);
    case Add(var Left, var Right):
      return Add(Deriv(Left), Deriv(Right));
    case Mult(var Left, var Right):
      return Add(Mult(Deriv(Left), Right), Mult(Left, Deriv(Right)));
    case Neg(var Value):
      return Neg(Deriv(Value));
  }
}
```

<span data-ttu-id="4e633-264">Zjednodušený výraz znázorňuje poziční vzory:</span><span class="sxs-lookup"><span data-stu-id="4e633-264">An expression simplifier demonstrates positional patterns:</span></span>

```csharp
Expr Simplify(Expr e)
{
  switch (e) {
    case Mult(Const(0), *): return Const(0);
    case Mult(*, Const(0)): return Const(0);
    case Mult(Const(1), var x): return Simplify(x);
    case Mult(var x, Const(1)): return Simplify(x);
    case Mult(Const(var l), Const(var r)): return Const(l*r);
    case Add(Const(0), var x): return Simplify(x);
    case Add(var x, Const(0)): return Simplify(x);
    case Add(Const(var l), Const(var r)): return Const(l+r);
    case Neg(Const(var k)): return Const(-k);
    default: return e;
  }
}
```
