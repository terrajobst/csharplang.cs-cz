---
ms.openlocfilehash: fad1425e2871f395a2eb1f39faccbc773d88d6a3
ms.sourcegitcommit: da1180f7eacdd5067b32d291a76e6764159e00fe
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/05/2019
ms.locfileid: "79485061"
---
# <a name="recursive-pattern-matching"></a><span data-ttu-id="18c79-101">Rekurzivní porovnávání vzorů</span><span class="sxs-lookup"><span data-stu-id="18c79-101">Recursive Pattern Matching</span></span>

## <a name="summary"></a><span data-ttu-id="18c79-102">Souhrn</span><span class="sxs-lookup"><span data-stu-id="18c79-102">Summary</span></span>
[summary]: #summary

<span data-ttu-id="18c79-103">Rozšíření pro porovnávání vzorů pro C# zajištění mnoha výhod algebraických datových typů a porovnávání vzorů z funkčních jazyků, ale způsobem, který hladce integruje s chováním základního jazyka.</span><span class="sxs-lookup"><span data-stu-id="18c79-103">Pattern matching extensions for C# enable many of the benefits of algebraic data types and pattern matching from functional languages, but in a way that smoothly integrates with the feel of the underlying language.</span></span> <span data-ttu-id="18c79-104">Prvky tohoto přístupu jsou nechte inspirovat souvisejícími funkcemi v programovacích jazycích [F#](http://www.msr-waypoint.net/pubs/79947/p29-syme.pdf "Rozšiřitelné porovnávání vzorů přes zjednodušený jazyk") a [Scala](https://link.springer.com/content/pdf/10.1007%2F978-3-540-73589-2.pdf "Vyhovující objekty se vzory, stránka 273").</span><span class="sxs-lookup"><span data-stu-id="18c79-104">Elements of this approach are inspired by related features in the programming languages [F#](http://www.msr-waypoint.net/pubs/79947/p29-syme.pdf "Extensible Pattern Matching Via a Lightweight Language") and [Scala](https://link.springer.com/content/pdf/10.1007%2F978-3-540-73589-2.pdf "Matching Objects With Patterns, page 273").</span></span>

## <a name="detailed-design"></a><span data-ttu-id="18c79-105">Podrobný návrh</span><span class="sxs-lookup"><span data-stu-id="18c79-105">Detailed design</span></span>
[design]: #detailed-design

### <a name="is-expression"></a><span data-ttu-id="18c79-106">Výraz is</span><span class="sxs-lookup"><span data-stu-id="18c79-106">Is Expression</span></span>

<span data-ttu-id="18c79-107">Operátor `is` je rozšířen o testování výrazu na základě *vzoru*.</span><span class="sxs-lookup"><span data-stu-id="18c79-107">The `is` operator is extended to test an expression against a *pattern*.</span></span>

```antlr
relational_expression
    : is_pattern_expression
    ;
is_pattern_expression
    : relational_expression 'is' pattern
    ;
```

<span data-ttu-id="18c79-108">Tato forma *relational_expression* je kromě existujících formulářů ve C# specifikaci.</span><span class="sxs-lookup"><span data-stu-id="18c79-108">This form of *relational_expression* is in addition to the existing forms in the C# specification.</span></span> <span data-ttu-id="18c79-109">Jedná se o chybu při kompilaci, pokud *relational_expression* vlevo od `is` tokenu neurčuje hodnotu nebo nemá typ.</span><span class="sxs-lookup"><span data-stu-id="18c79-109">It is a compile-time error if the *relational_expression* to the left of the `is` token does not designate a value or does not have a type.</span></span>

<span data-ttu-id="18c79-110">Každý *identifikátor* vzoru zavádí novou místní proměnnou, která je *omezena* na základě `true`ho operátoru `is` (tj. *jednoznačně přiřazený při hodnotě true*).</span><span class="sxs-lookup"><span data-stu-id="18c79-110">Every *identifier* of the pattern introduces a new local variable that is *definitely assigned* after the `is` operator is `true` (i.e. *definitely assigned when true*).</span></span>

> <span data-ttu-id="18c79-111">Poznámka: existují technicky nejednoznačnost mezi *typem* v `is-expression` a *constant_pattern*, z nichž každá může být platným analýzou kvalifikovaného identifikátoru.</span><span class="sxs-lookup"><span data-stu-id="18c79-111">Note: There is technically an ambiguity between *type* in an `is-expression` and *constant_pattern*, either of which might be a valid parse of a qualified identifier.</span></span> <span data-ttu-id="18c79-112">Pokusíme se vytvořit propojení jako typ pro kompatibilitu s předchozími verzemi jazyka; jenom v případě, že se to nepodaří, vyřešíme ho jako výraz v jiných kontextech na první nalezenou věc (která musí být buď konstanta, nebo typ).</span><span class="sxs-lookup"><span data-stu-id="18c79-112">We try to bind it as a type for compatibility with previous versions of the language; only if that fails do we resolve it as we do an expression in other contexts, to the first thing found (which must be either a constant or a type).</span></span> <span data-ttu-id="18c79-113">Tato nejednoznačnost je k dispozici pouze na pravé straně výrazu `is`.</span><span class="sxs-lookup"><span data-stu-id="18c79-113">This ambiguity is only present on the right-hand-side of an `is` expression.</span></span>

### <a name="patterns"></a><span data-ttu-id="18c79-114">Vzory</span><span class="sxs-lookup"><span data-stu-id="18c79-114">Patterns</span></span>

<span data-ttu-id="18c79-115">Vzory jsou používány v operátoru *is_pattern* , v *switch_statement*a v *switch_expression* pro vyjádření tvaru dat, proti kterým jsou příchozí data (která volají vstupní hodnotu) porovnána.</span><span class="sxs-lookup"><span data-stu-id="18c79-115">Patterns are used in the *is_pattern* operator, in a *switch_statement*, and in a *switch_expression* to express the shape of data against which incoming data  (which we call the input value) is to be compared.</span></span> <span data-ttu-id="18c79-116">Vzorce mohou být rekurzivní, aby mohly být části dat porovnány s dílčími vzory.</span><span class="sxs-lookup"><span data-stu-id="18c79-116">Patterns may be recursive so that parts of the data may be matched against sub-patterns.</span></span>

```antlr
pattern
    : declaration_pattern
    | constant_pattern
    | var_pattern
    | positional_pattern
    | property_pattern
    | discard_pattern
    ;
declaration_pattern
    : type simple_designation
    ;
constant_pattern
    : expression
    ;
var_pattern
    : 'var' designation
    ;
positional_pattern
    : type? '(' subpatterns? ')' property_subpattern? simple_designation?
    ;
subpatterns
    : subpattern
    | subpattern ',' subpatterns
    ;
subpattern
    : pattern
    | identifier ':' pattern
    ;
property_subpattern
    : '{' subpatterns? '}'
    ;
property_pattern
    : type? property_subpattern simple_designation?
    ;
simple_designation
    : single_variable_designation
    | discard_designation
    ;
discard_pattern
    : '_'
    ;
```

#### <a name="declaration-pattern"></a><span data-ttu-id="18c79-117">Vzor deklarace</span><span class="sxs-lookup"><span data-stu-id="18c79-117">Declaration Pattern</span></span>

```antlr
declaration_pattern
    : type simple_designation
    ;
```

<span data-ttu-id="18c79-118">*Declaration_pattern* testuje, zda je výraz daný typ, a přetypování na tento typ, pokud je test úspěšný.</span><span class="sxs-lookup"><span data-stu-id="18c79-118">The *declaration_pattern* both tests that an expression is of a given type and casts it to that type if the test succeeds.</span></span> <span data-ttu-id="18c79-119">To může představovat místní proměnnou daného typu pojmenované daným identifikátorem, pokud je označení *single_variable_designation*.</span><span class="sxs-lookup"><span data-stu-id="18c79-119">This may introduce a local variable of the given type named by the given identifier, if the designation is a *single_variable_designation*.</span></span> <span data-ttu-id="18c79-120">Tato lokální proměnná je *jednoznačně přiřazena* , pokud je výsledek operace porovnávání se vzorem `true`.</span><span class="sxs-lookup"><span data-stu-id="18c79-120">That local variable is *definitely assigned* when the result of the pattern-matching operation is `true`.</span></span>

<span data-ttu-id="18c79-121">Sémantika modulu runtime tohoto výrazu je, že testuje typ modulu runtime *relational_expression* operandu na levé straně proti *typu* ve vzoru.</span><span class="sxs-lookup"><span data-stu-id="18c79-121">The runtime semantic of this expression is that it tests the runtime type of the left-hand *relational_expression* operand against the *type* in the pattern.</span></span>  <span data-ttu-id="18c79-122">Pokud se jedná o tento typ modulu runtime (nebo některé podtypy) a ne `null`, výsledek `is operator` je `true`.</span><span class="sxs-lookup"><span data-stu-id="18c79-122">If it is of that runtime type (or some subtype) and not `null`, the result of the `is operator` is `true`.</span></span>

<span data-ttu-id="18c79-123">Určité kombinace statického typu na levé straně a daného typu se považují za nekompatibilní a výsledkem je chyba při kompilaci.</span><span class="sxs-lookup"><span data-stu-id="18c79-123">Certain combinations of static type of the left-hand-side and the given type are considered incompatible and result in compile-time error.</span></span> <span data-ttu-id="18c79-124">Hodnota statického typu `E` je označována jako *kompatibilní se vzorem* typu `T` v případě, že existuje převod identity, implicitní převod odkazu, převod zabalení, explicitní převod odkazu nebo převod rozbalení z `E` na `T`nebo pokud jeden z těchto typů je otevřený typ.</span><span class="sxs-lookup"><span data-stu-id="18c79-124">A value of static type `E` is said to be *pattern-compatible* with a type `T` if there exists an identity conversion, an implicit reference conversion, a boxing conversion, an explicit reference conversion, or an unboxing conversion from `E` to `T`, or if one of those types is an open type.</span></span> <span data-ttu-id="18c79-125">Jedná se o chybu při kompilaci, pokud není vstup typu `E` *kompatibilní se vzorem* *typu v rámci* vzoru typu, se kterým se shoduje.</span><span class="sxs-lookup"><span data-stu-id="18c79-125">It is a compile-time error if an input of type `E` is not *pattern-compatible* with the *type* in a type pattern that it is matched with.</span></span>

<span data-ttu-id="18c79-126">Vzor typu je vhodný pro provádění testů typu odkazu v době běhu a nahrazuje idiom</span><span class="sxs-lookup"><span data-stu-id="18c79-126">The type pattern is useful for performing run-time type tests of reference types, and replaces the idiom</span></span>

```csharp
var v = expr as Type;
if (v != null) { // code using v
```

<span data-ttu-id="18c79-127">S mírným výstižným</span><span class="sxs-lookup"><span data-stu-id="18c79-127">With the slightly more concise</span></span>

```csharp
if (expr is Type v) { // code using v
```

<span data-ttu-id="18c79-128">Jedná se o chybu, pokud je *typ* hodnota s možnou hodnotou null.</span><span class="sxs-lookup"><span data-stu-id="18c79-128">It is an error if *type* is a nullable value type.</span></span>

<span data-ttu-id="18c79-129">Vzor typu lze použít k otestování hodnot typů s možnou hodnotou null: hodnota typu `Nullable<T>` (nebo zabalená `T`) odpovídá vzoru typu `T2 id`, pokud hodnota není null a typ `T2` je `T`nebo některý základní typ nebo rozhraní `T`.</span><span class="sxs-lookup"><span data-stu-id="18c79-129">The type pattern can be used to test values of nullable types: a value of type `Nullable<T>` (or a boxed `T`) matches a type pattern `T2 id` if the value is non-null and the type of `T2` is `T`, or some base type or interface of `T`.</span></span> <span data-ttu-id="18c79-130">Například v fragmentu kódu</span><span class="sxs-lookup"><span data-stu-id="18c79-130">For example, in the code fragment</span></span>

```csharp
int? x = 3;
if (x is int v) { // code using v
```

<span data-ttu-id="18c79-131">Podmínka příkazu `if` je `true` za běhu a proměnná `v` obsahuje hodnotu `3` typu `int` uvnitř bloku.</span><span class="sxs-lookup"><span data-stu-id="18c79-131">The condition of the `if` statement is `true` at runtime and the variable `v` holds the value `3` of type `int` inside the block.</span></span> <span data-ttu-id="18c79-132">Po bloku je proměnná `v` v oboru, ale není jednoznačně přiřazená.</span><span class="sxs-lookup"><span data-stu-id="18c79-132">After the block the variable `v` is in scope but not definitely assigned.</span></span>

#### <a name="constant-pattern"></a><span data-ttu-id="18c79-133">Konstantní vzorek</span><span class="sxs-lookup"><span data-stu-id="18c79-133">Constant Pattern</span></span>

```antlr
constant_pattern
    : constant_expression
    ;
```

<span data-ttu-id="18c79-134">Konstantní vzor testuje hodnotu výrazu s konstantní hodnotou.</span><span class="sxs-lookup"><span data-stu-id="18c79-134">A constant pattern tests the value of an expression against a constant value.</span></span> <span data-ttu-id="18c79-135">Konstanta může být libovolný konstantní výraz, jako je například literál, název deklarované `const` proměnné nebo konstanta výčtu.</span><span class="sxs-lookup"><span data-stu-id="18c79-135">The constant may be any constant expression, such as a literal, the name of a declared `const` variable, or an enumeration constant.</span></span> <span data-ttu-id="18c79-136">Pokud vstupní hodnota není otevřený typ, konstantní výraz je implicitně převeden na typ odpovídajícího výrazu; Pokud typ vstupní hodnoty není *kompatibilní se vzorem* s typem konstantního výrazu, operace porovnávání se vzorem je chyba.</span><span class="sxs-lookup"><span data-stu-id="18c79-136">When the input value is not an open type, the constant expression is implicitly converted to the type of the matched expression; if the type of the input value is not *pattern-compatible* with the type of the constant expression, the pattern-matching operation is an error.</span></span>

<span data-ttu-id="18c79-137">Vzor *c* se považuje za vyhovující převedenou vstupní hodnotu *e* , pokud `object.Equals(c, e)` vrátí `true`.</span><span class="sxs-lookup"><span data-stu-id="18c79-137">The pattern *c* is considered matching the converted input value *e* if `object.Equals(c, e)` would return `true`.</span></span>

<span data-ttu-id="18c79-138">Očekáváme, `e is null` jako nejběžnější způsob testování `null` v nově vytvořeném kódu, protože nemůže vyvolat uživatelsky definovanou `operator==`.</span><span class="sxs-lookup"><span data-stu-id="18c79-138">We expect to see `e is null` as the most common way to test for `null` in newly written code, as it cannot invoke a user-defined `operator==`.</span></span>

#### <a name="var-pattern"></a><span data-ttu-id="18c79-139">Variabilní vzorek</span><span class="sxs-lookup"><span data-stu-id="18c79-139">Var Pattern</span></span>

```antlr
var_pattern
    : 'var' designation
    ;
designation
    : simple_designation
    | tuple_designation
    ;
simple_designation
    : single_variable_designation
    | discard_designation
    ;
single_variable_designation
    : identifier
    ;
discard_designation
    : _
    ;
tuple_designation
    : '(' designations? ')'
    ;
designations
    : designation
    | designations ',' designation
    ;
```

<span data-ttu-id="18c79-140">Pokud je *označením* *simple_designation*, výraz *e* odpovídá vzoru.</span><span class="sxs-lookup"><span data-stu-id="18c79-140">If the *designation* is a *simple_designation*, an expression *e* matches the pattern.</span></span> <span data-ttu-id="18c79-141">Jinými slovy, shoda se *vzorkem var* vždy uspěje s *simple_designation*.</span><span class="sxs-lookup"><span data-stu-id="18c79-141">In other words, a match to a *var pattern* always succeeds with a *simple_designation*.</span></span> <span data-ttu-id="18c79-142">Pokud je *simple_designation* *single_variable_designation*, hodnota *e* je vázána na nově zavedenou místní proměnnou.</span><span class="sxs-lookup"><span data-stu-id="18c79-142">If the *simple_designation* is a *single_variable_designation*, the value of *e* is bounds to a newly introduced local variable.</span></span> <span data-ttu-id="18c79-143">Typ lokální proměnné je statický typ *e*.</span><span class="sxs-lookup"><span data-stu-id="18c79-143">The type of the local variable is the static type of *e*.</span></span>

<span data-ttu-id="18c79-144">Pokud je *označením* *tuple_designation*, pak je vzor ekvivalentní *positional_pattern* formuláře `(var` *označení*,... `)` kde *označení*s se nacházejí v *tuple_designation*.</span><span class="sxs-lookup"><span data-stu-id="18c79-144">If the *designation* is a *tuple_designation*, then the pattern is equivalent to a *positional_pattern* of the form `(var` *designation*, ... `)` where the *designation*s are those found within the *tuple_designation*.</span></span>  <span data-ttu-id="18c79-145">Například vzor `var (x, (y, z))` je ekvivalentem `(var x, (var y, var z))`.</span><span class="sxs-lookup"><span data-stu-id="18c79-145">For example, the pattern `var (x, (y, z))` is equivalent to `(var x, (var y, var z))`.</span></span>

<span data-ttu-id="18c79-146">Pokud se název `var` váže k typu, jedná se o chybu.</span><span class="sxs-lookup"><span data-stu-id="18c79-146">It is an error if the name `var` binds to a type.</span></span>

#### <a name="discard-pattern"></a><span data-ttu-id="18c79-147">Zahodit vzor</span><span class="sxs-lookup"><span data-stu-id="18c79-147">Discard Pattern</span></span>

```antlr
discard_pattern
    : '_'
    ;
```

<span data-ttu-id="18c79-148">Výraz *e* odpovídá vzorci `_` vždy.</span><span class="sxs-lookup"><span data-stu-id="18c79-148">An expression *e* matches the pattern `_` always.</span></span> <span data-ttu-id="18c79-149">Jinými slovy, každý výraz odpovídá vzoru zahození.</span><span class="sxs-lookup"><span data-stu-id="18c79-149">In other words, every expression matches the discard pattern.</span></span>

<span data-ttu-id="18c79-150">Vzor zahození nesmí být použit jako vzor *is_pattern_expression*.</span><span class="sxs-lookup"><span data-stu-id="18c79-150">A discard pattern may not be used as the pattern of an *is_pattern_expression*.</span></span>

#### <a name="positional-pattern"></a><span data-ttu-id="18c79-151">Poziční vzor</span><span class="sxs-lookup"><span data-stu-id="18c79-151">Positional Pattern</span></span>

<span data-ttu-id="18c79-152">Poziční vzor kontroluje, zda vstupní hodnota není `null`, vyvolá příslušnou metodu `Deconstruct` a provede další porovnávání vzorů pro výsledné hodnoty.</span><span class="sxs-lookup"><span data-stu-id="18c79-152">A positional pattern checks that the input value is not `null`, invokes an appropriate `Deconstruct` method, and performs further pattern matching on the resulting values.</span></span>  <span data-ttu-id="18c79-153">Podporuje také syntaxi vzoru podobné řazené kolekce členů (bez zadání zadaného typu), pokud je typ vstupní hodnoty stejný jako typ obsahující `Deconstruct`, nebo pokud je typ vstupní hodnoty typ řazené kolekce členů, nebo pokud je typ vstupní hodnoty `object` nebo `ITuple` a typ vstupní hodnoty je nebo a běhový typ výrazu implementuje `ITuple`.</span><span class="sxs-lookup"><span data-stu-id="18c79-153">It also supports a tuple-like pattern syntax (without the type being provided) when the type of the input value is the same as the type containing `Deconstruct`, or if the type of the input value is a tuple type, or if the type of the input value is `object` or `ITuple` and the runtime type of the expression implements `ITuple`.</span></span>

```antlr
positional_pattern
    : type? '(' subpatterns? ')' property_subpattern? simple_designation?
    ;
subpatterns
    : subpattern
    | subpattern ',' subpatterns
    ;
subpattern
    : pattern
    | identifier ':' pattern
    ;
```

<span data-ttu-id="18c79-154">Pokud je tento *typ* vynechán, převezmeme to jako statický typ vstupní hodnoty.</span><span class="sxs-lookup"><span data-stu-id="18c79-154">If the *type* is omitted, we take it to be the static type of the input value.</span></span>

<span data-ttu-id="18c79-155">Vzhledem k hodnotě vstupní hodnoty pro *typ* vzoru `(` *subpattern_list* `)`, je vybrána metoda hledáním v *typu* pro přístupné deklarace `Deconstruct` a výběrem jednoho z nich pomocí stejných pravidel jako u deklarace dekonstrukce.</span><span class="sxs-lookup"><span data-stu-id="18c79-155">Given a match of an input value to the pattern *type* `(` *subpattern_list* `)`, a method is selected by searching in *type* for accessible declarations of `Deconstruct` and selecting one among them using the same rules as for the deconstruction declaration.</span></span>

<span data-ttu-id="18c79-156">Jedná se o chybu, pokud *positional_pattern* vynechá typ, má jeden dílčí *vzor* bez *identifikátoru*, nemá žádné *property_subpattern* a neobsahuje žádné *simple_designation*.</span><span class="sxs-lookup"><span data-stu-id="18c79-156">It is an error if a *positional_pattern* omits the type, has a single *subpattern* without an *identifier*, has no *property_subpattern* and has no *simple_designation*.</span></span> <span data-ttu-id="18c79-157">Tato nejednoznačnost mezi *constant_pattern* , která je v závorkách, a *positional_pattern*.</span><span class="sxs-lookup"><span data-stu-id="18c79-157">This disambiguates between a *constant_pattern* that is parenthesized and a *positional_pattern*.</span></span>

<span data-ttu-id="18c79-158">Aby bylo možné extrahovat hodnoty odpovídající vzorům v seznamu,</span><span class="sxs-lookup"><span data-stu-id="18c79-158">In order to extract the values to match against the patterns in the list,</span></span>
- <span data-ttu-id="18c79-159">Pokud byl *typ* vynechán a typ vstupní hodnoty je typ řazené kolekce členů, musí být počet dílčích vzorů stejný jako mohutnost řazené kolekce členů.</span><span class="sxs-lookup"><span data-stu-id="18c79-159">If *type* was omitted and the input value's type is a tuple type, then the number of subpatterns is required to be the same as the cardinality of the tuple.</span></span> <span data-ttu-id="18c79-160">Každý prvek řazené kolekce členů je porovnán s odpovídajícím dílčím *vzorem*a shoda je úspěšná, pokud je vše úspěšné.</span><span class="sxs-lookup"><span data-stu-id="18c79-160">Each tuple element is matched against the corresponding *subpattern*, and the match succeeds if all of these succeed.</span></span> <span data-ttu-id="18c79-161">Pokud má jakýkoliv dílčí *vzor* *identifikátor*, pak musí název prvku řazené kolekce členů na odpovídající pozici v typu řazené kolekce členů.</span><span class="sxs-lookup"><span data-stu-id="18c79-161">If any *subpattern* has an *identifier*, then that must name a tuple element at the corresponding position in the tuple type.</span></span>
- <span data-ttu-id="18c79-162">V opačném případě, pokud existuje vhodný `Deconstruct` jako člen *typu*, jedná se o chybu při kompilaci, pokud typ vstupní hodnoty není *kompatibilní se vzorem* *typu*.</span><span class="sxs-lookup"><span data-stu-id="18c79-162">Otherwise, if a suitable `Deconstruct` exists as a member of *type*, it is a compile-time error if the type of the input value is not *pattern-compatible* with *type*.</span></span> <span data-ttu-id="18c79-163">Za běhu je vstupní hodnota testována proti *typu*.</span><span class="sxs-lookup"><span data-stu-id="18c79-163">At runtime the input value is tested against *type*.</span></span> <span data-ttu-id="18c79-164">Pokud se to nepovede, neproběhne porovnávání pozičního vzoru.</span><span class="sxs-lookup"><span data-stu-id="18c79-164">If this fails then the positional pattern match fails.</span></span> <span data-ttu-id="18c79-165">V případě úspěchu se vstupní hodnota převede na tento typ a `Deconstruct` je vyvolána s novými proměnnými generovanými kompilátorem pro příjem parametrů `out`.</span><span class="sxs-lookup"><span data-stu-id="18c79-165">If it succeeds,  the input value is converted to this type and `Deconstruct` is invoked with fresh compiler-generated variables to receive the `out` parameters.</span></span> <span data-ttu-id="18c79-166">Každá přijatá hodnota je porovnána s odpovídajícím dílčím *vzorem*a shoda bude úspěšná, pokud je vše úspěšné.</span><span class="sxs-lookup"><span data-stu-id="18c79-166">Each value that was received is matched against the corresponding *subpattern*, and the match succeeds if all of these succeed.</span></span> <span data-ttu-id="18c79-167">Pokud má jakýkoliv dílčí *vzor* *identifikátor*, pak musí parametr pojmenovat na odpovídající pozici `Deconstruct`.</span><span class="sxs-lookup"><span data-stu-id="18c79-167">If any *subpattern* has an *identifier*, then that must name a parameter at the corresponding position of `Deconstruct`.</span></span>
- <span data-ttu-id="18c79-168">Jinak, pokud byl *typ* vynechán, a vstupní hodnota je typu `object` nebo `ITuple` nebo nějaký typ, který lze převést na `ITuple` implicitním převodem odkazu, a v rámci dílčích vzorů se neobjeví žádný *identifikátor* , bude spárováno s použitím `ITuple`.</span><span class="sxs-lookup"><span data-stu-id="18c79-168">Otherwise if *type* was omitted, and the input value is of type `object` or `ITuple` or some type that can be converted to `ITuple` by an implicit reference conversion, and no *identifier* appears among the subpatterns, then we match using `ITuple`.</span></span>
- <span data-ttu-id="18c79-169">V opačném případě se jedná o chybu při kompilaci.</span><span class="sxs-lookup"><span data-stu-id="18c79-169">Otherwise the pattern is a compile-time error.</span></span>

<span data-ttu-id="18c79-170">Pořadí, ve kterém se podvzorci shodují za běhu, není specifikováno a neúspěšná shoda se nemůže pokusit porovnat všechny dílčí vzory.</span><span class="sxs-lookup"><span data-stu-id="18c79-170">The order in which subpatterns are matched at runtime is unspecified, and a failed match may not attempt to match all subpatterns.</span></span>

##### <a name="example"></a><span data-ttu-id="18c79-171">Příklad</span><span class="sxs-lookup"><span data-stu-id="18c79-171">Example</span></span>

<span data-ttu-id="18c79-172">Tento příklad používá mnoho funkcí popsaných v této specifikaci.</span><span class="sxs-lookup"><span data-stu-id="18c79-172">This example uses many of the features described in this specification</span></span>

``` c#
    var newState = (GetState(), action, hasKey) switch {
        (DoorState.Closed, Action.Open, _) => DoorState.Opened,
        (DoorState.Opened, Action.Close, _) => DoorState.Closed,
        (DoorState.Closed, Action.Lock, true) => DoorState.Locked,
        (DoorState.Locked, Action.Unlock, true) => DoorState.Closed,
        (var state, _, _) => state };
```

#### <a name="property-pattern"></a><span data-ttu-id="18c79-173">Vzor vlastnosti</span><span class="sxs-lookup"><span data-stu-id="18c79-173">Property Pattern</span></span>

<span data-ttu-id="18c79-174">Vzor vlastnosti kontroluje, zda vstupní hodnota není `null` a rekurzivně odpovídá hodnotám extrahovaných pomocí přístupných vlastností nebo polí.</span><span class="sxs-lookup"><span data-stu-id="18c79-174">A property pattern checks that the input value is not `null` and recursively matches values extracted by the use of accessible properties or fields.</span></span>

```antlr
property_pattern
    : type? property_subpattern simple_designation?
    ;
property_subpattern
    : '{' '}'
    | '{' subpatterns ','? '}'
    ;
```

<span data-ttu-id="18c79-175">Jedná se o chybu, pokud jakýkoli dílčí _vzor_ _property_pattern_ neobsahuje _identifikátor_ (musí být druhý tvar, který má _identifikátor_).</span><span class="sxs-lookup"><span data-stu-id="18c79-175">It is an error if any _subpattern_ of a _property_pattern_ does not contain an _identifier_ (it must be of the second form, which has an _identifier_).</span></span>  <span data-ttu-id="18c79-176">Koncová čárka po posledním podvzoru je volitelná.</span><span class="sxs-lookup"><span data-stu-id="18c79-176">A trailing comma after the last subpattern is optional.</span></span>

<span data-ttu-id="18c79-177">Všimněte si, že vzor pro kontrolu hodnoty null nespadají do vzoru triviální vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="18c79-177">Note that a null-checking pattern falls out of a trivial property pattern.</span></span> <span data-ttu-id="18c79-178">Chcete-li zjistit, zda `s` řetězec není null, můžete napsat libovolný z následujících formulářů</span><span class="sxs-lookup"><span data-stu-id="18c79-178">To check if the string `s` is non-null, you can write any of the following forms</span></span>

```csharp
if (s is object o) ... // o is of type object
if (s is string x) ... // x is of type string
if (s is {} x) ... // x is of type string
if (s is {}) ...
```

<span data-ttu-id="18c79-179">Vzhledem k tomu, že se shoda s výrazem *e* pro *typ* vzoru `{` *property_pattern_list* `}`, jedná se o chybu při kompilaci, pokud výraz *e* není *kompatibilní se vzorem* typu *t* , který je určen *typem*.</span><span class="sxs-lookup"><span data-stu-id="18c79-179">Given a match of an expression *e* to the pattern *type* `{` *property_pattern_list* `}`, it is a compile-time error if the expression *e* is not *pattern-compatible* with the type *T* designated by *type*.</span></span> <span data-ttu-id="18c79-180">Pokud tento typ chybí, převezmeme to jako statický typ *e*.</span><span class="sxs-lookup"><span data-stu-id="18c79-180">If the type is absent, we take it to be the static type of *e*.</span></span> <span data-ttu-id="18c79-181">Pokud je *identifikátor* přítomen, deklaruje proměnnou *vzoru typu typ.*</span><span class="sxs-lookup"><span data-stu-id="18c79-181">If the *identifier* is present, it declares a pattern variable of type *type*.</span></span> <span data-ttu-id="18c79-182">Každý identifikátor, který se zobrazuje na levé straně *property_pattern_list* , musí určovat přístupnou vlastnost nebo pole *T*, které lze číst. Pokud je *simple_designation* přítomna simple_designation *property_pattern* , definuje proměnnou vzoru typu *T*.</span><span class="sxs-lookup"><span data-stu-id="18c79-182">Each of the identifiers appearing on the left-hand-side of its *property_pattern_list* must designate an accessible readable property or field of *T*. If the *simple_designation* of the *property_pattern* is present, it defines a pattern variable of type *T*.</span></span>

<span data-ttu-id="18c79-183">Za běhu je výraz testován proti *T*. Pokud se to nepovede, neproběhne shoda vzoru vlastností a výsledek je `false`.</span><span class="sxs-lookup"><span data-stu-id="18c79-183">At runtime, the expression is tested against *T*. If this fails then the property pattern match fails and the result is `false`.</span></span> <span data-ttu-id="18c79-184">Pokud je to úspěšné, pak je přečteno každé *property_subpattern* pole nebo vlastnost a její hodnota se shoduje s odpovídajícím vzorem.</span><span class="sxs-lookup"><span data-stu-id="18c79-184">If it succeeds, then each *property_subpattern* field or property is read and its value matched against its corresponding pattern.</span></span> <span data-ttu-id="18c79-185">Výsledek celé shody je `false` pouze v případě, že výsledek kteréhokoli z nich je `false`.</span><span class="sxs-lookup"><span data-stu-id="18c79-185">The result of the whole match is `false` only if the result of any of these is `false`.</span></span> <span data-ttu-id="18c79-186">Pořadí, ve kterém se podvzorci shodují, není zadáno a neúspěšná shoda nesmí odpovídat všem podvzorům za běhu.</span><span class="sxs-lookup"><span data-stu-id="18c79-186">The order in which subpatterns are matched is not specified, and a failed match may not match all subpatterns at runtime.</span></span> <span data-ttu-id="18c79-187">Je-li shoda úspěšná a *simple_designation* *property_pattern* je *Single_variable_designation*, definuje proměnnou typu *T* , která je přiřazena odpovídající hodnota.</span><span class="sxs-lookup"><span data-stu-id="18c79-187">If the match succeeds and the *simple_designation* of the *property_pattern* is a *single_variable_designation*, it defines a variable of type *T* that is assigned the matched value.</span></span>

> <span data-ttu-id="18c79-188">Poznámka: vzor vlastnosti lze použít pro porovnávání vzorů s anonymními typy.</span><span class="sxs-lookup"><span data-stu-id="18c79-188">Note: The property pattern can be used to pattern-match with anonymous types.</span></span>

##### <a name="example"></a><span data-ttu-id="18c79-189">Příklad</span><span class="sxs-lookup"><span data-stu-id="18c79-189">Example</span></span>

```csharp
if (o is string { Length: 5 } s)
```

### <a name="switch-expression"></a><span data-ttu-id="18c79-190">Výraz Switch</span><span class="sxs-lookup"><span data-stu-id="18c79-190">Switch Expression</span></span>

<span data-ttu-id="18c79-191">K podpoře sémantiky `switch`jako pro kontext výrazu se přidá *switch_expression* .</span><span class="sxs-lookup"><span data-stu-id="18c79-191">A *switch_expression* is added to support `switch`-like semantics for an expression context.</span></span>

<span data-ttu-id="18c79-192">Syntaxe C# jazyka se rozšiřuje s následujícími syntaktickými výrobou:</span><span class="sxs-lookup"><span data-stu-id="18c79-192">The C# language syntax is augmented with the following syntactic productions:</span></span>

```antlr
multiplicative_expression
    : switch_expression
    | multiplicative_expression '*' switch_expression
    | multiplicative_expression '/' switch_expression
    | multiplicative_expression '%' switch_expression
    ;
switch_expression
    : range_expression 'switch' '{' '}'
    | range_expression 'switch' '{' switch_expression_arms ','? '}'
    ;
switch_expression_arms
    : switch_expression_arm
    | switch_expression_arms ',' switch_expression_arm
    ;
switch_expression_arm
    : pattern case_guard? '=>' expression
    ;
case_guard
    : 'when' null_coalescing_expression
    ;
```

<span data-ttu-id="18c79-193">*Switch_expression* není povolen jako *expression_statement*.</span><span class="sxs-lookup"><span data-stu-id="18c79-193">The *switch_expression* is not permitted as an *expression_statement*.</span></span>

> <span data-ttu-id="18c79-194">Požadavků to v budoucí revizi.</span><span class="sxs-lookup"><span data-stu-id="18c79-194">We are looking at relaxing this in a future revision.</span></span>

<span data-ttu-id="18c79-195">Typ *switch_expression* je [*nejlepší běžný typ*](https://github.com/dotnet/csharplang/blob/master/spec/expressions.md#finding-the-best-common-type-of-a-set-of-expressions) výrazů zobrazený napravo od `=>`ch tokenů *switch_expression_arm*s, pokud takový typ existuje a výraz v každém ARM výrazu Switch lze implicitně převést na tento typ.</span><span class="sxs-lookup"><span data-stu-id="18c79-195">The type of the *switch_expression* is the [*best common type*](https://github.com/dotnet/csharplang/blob/master/spec/expressions.md#finding-the-best-common-type-of-a-set-of-expressions) of the expressions appearing to the right of the `=>` tokens of the *switch_expression_arm*s if such a type exists and the expression in every arm of the switch expression can be implicitly converted to that type.</span></span>  <span data-ttu-id="18c79-196">Kromě toho přidáme nový *Převod výrazu Switch*, což je předdefinovaný implicitní převod z výrazu Switch na každý typ `T`, pro který existuje implicitní převod z výrazu každého arm na `T`.</span><span class="sxs-lookup"><span data-stu-id="18c79-196">In addition, we add a new *switch expression conversion*, which is a predefined implicit conversion from a switch expression to every type `T` for which there exists an implicit conversion from each arm's expression to `T`.</span></span>

<span data-ttu-id="18c79-197">Jedná se o chybu, pokud některý vzorek *switch_expression_arm*nemůže ovlivnit výsledek, protože předchozí vzor a ochrana se vždy shodují.</span><span class="sxs-lookup"><span data-stu-id="18c79-197">It is an error if some *switch_expression_arm*'s pattern cannot affect the result because some previous pattern and guard will always match.</span></span>

<span data-ttu-id="18c79-198">Výraz přepínače je označován jako *vyčerpávající* , pokud některé ARM výrazu Switch zpracovává všechny hodnoty jeho vstupu.</span><span class="sxs-lookup"><span data-stu-id="18c79-198">A switch expression is said to be *exhaustive* if some arm of the switch expression handles every value of its input.</span></span>  <span data-ttu-id="18c79-199">Kompilátor vydá upozornění, pokud výraz přepínače není *vyčerpávající*.</span><span class="sxs-lookup"><span data-stu-id="18c79-199">The compiler shall produce a warning if a switch expression is not *exhaustive*.</span></span>

<span data-ttu-id="18c79-200">Za běhu je výsledkem *switch_expression* hodnota *výrazu* prvního *switch_expression_arm* , pro který výraz na levé straně *switch_expression* odpovídá vzorci *switch_expression_arm*a který *case_guard* *switch_expression_arm*, pokud je k dispozici, je vyhodnocen jako `true`.</span><span class="sxs-lookup"><span data-stu-id="18c79-200">At runtime, the result of the *switch_expression* is the value of the *expression* of the first *switch_expression_arm* for which the expression on the left-hand-side of the *switch_expression* matches the *switch_expression_arm*'s pattern, and for which the *case_guard* of the *switch_expression_arm*, if present, evaluates to `true`.</span></span> <span data-ttu-id="18c79-201">Pokud neexistuje žádný takový *switch_expression_arm*, *switch_expression* vyvolá instanci výjimky `System.Runtime.CompilerServices.SwitchExpressionException`.</span><span class="sxs-lookup"><span data-stu-id="18c79-201">If there is no such *switch_expression_arm*, the *switch_expression* throws an instance of the exception `System.Runtime.CompilerServices.SwitchExpressionException`.</span></span>

### <a name="optional-parens-when-switching-on-a-tuple-literal"></a><span data-ttu-id="18c79-202">Volitelné Parens při přepínání na literál řazené kolekce členů</span><span class="sxs-lookup"><span data-stu-id="18c79-202">Optional parens when switching on a tuple literal</span></span>

<span data-ttu-id="18c79-203">Aby bylo možné přepnout na literál řazené kolekce členů pomocí *switch_statement*, musíte napsat, co vypadá jako redundantní Parens</span><span class="sxs-lookup"><span data-stu-id="18c79-203">In order to switch on a tuple literal using the *switch_statement*, you have to write what appear to be redundant parens</span></span>

```csharp
switch ((a, b))
{
```

<span data-ttu-id="18c79-204">Povolení</span><span class="sxs-lookup"><span data-stu-id="18c79-204">To permit</span></span>

```csharp
switch (a, b)
{
```

<span data-ttu-id="18c79-205">kulaté závorky příkazu switch jsou nepovinné, když je výraz přepnutý na literál řazené kolekce členů.</span><span class="sxs-lookup"><span data-stu-id="18c79-205">the parentheses of the switch statement are optional when the expression being switched on is a tuple literal.</span></span>

### <a name="order-of-evaluation-in-pattern-matching"></a><span data-ttu-id="18c79-206">Pořadí vyhodnocení při porovnávání vzorů</span><span class="sxs-lookup"><span data-stu-id="18c79-206">Order of evaluation in pattern-matching</span></span>

<span data-ttu-id="18c79-207">Poskytnutí flexibility kompilátoru při změně pořadí operací provedených během porovnávání vzorů může umožňovat flexibilitu, kterou lze použít ke zlepšení efektivity porovnávání vzorů.</span><span class="sxs-lookup"><span data-stu-id="18c79-207">Giving the compiler flexibility in reordering the operations executed during pattern-matching can permit flexibility that can be used to improve the efficiency of pattern-matching.</span></span> <span data-ttu-id="18c79-208">Požadavek (nevykonatelný) by byl takový, že vlastnosti, ke kterým se přistupoval, a metody dekonstrukce musí být "Pure" (vedlejší účinky jsou bezplatné, idempotentní atd.).</span><span class="sxs-lookup"><span data-stu-id="18c79-208">The (unenforced) requirement would be that properties accessed in a pattern, and the Deconstruct methods, are required to be "pure" (side-effect free, idempotent, etc).</span></span> <span data-ttu-id="18c79-209">To neznamená, že by se vám jako koncept jazyka přidala čistota, ale jenom to, že by to mělo být pro kompilátor flexibilita při operacích s přeřazením.</span><span class="sxs-lookup"><span data-stu-id="18c79-209">That doesn't mean that we would add purity as a language concept, only that we would allow the compiler flexibility in reordering operations.</span></span>

<span data-ttu-id="18c79-210">**Řešení 2018-04-04 LDM**: potvrzeno: kompilátor má oprávnění změnit pořadí volání `Deconstruct`, přístup k vlastnostem a vyvolání metod v `ITuple`a může předpokládat, že vrácené hodnoty jsou stejné z více volání.</span><span class="sxs-lookup"><span data-stu-id="18c79-210">**Resolution 2018-04-04 LDM**: confirmed: the compiler is permitted to reorder calls to `Deconstruct`, property accesses, and invocations of methods in `ITuple`, and may assume that returned values are the same from multiple calls.</span></span> <span data-ttu-id="18c79-211">Kompilátor by neměl volat funkce, které nemůžou mít vliv na výsledek, a před provedením jakýchkoli změn v pořadí vygenerovaných kompilátorem vyhodnocování v budoucnu budeme mít pozor.</span><span class="sxs-lookup"><span data-stu-id="18c79-211">The compiler should not invoke functions that cannot affect the result, and we will be very careful before making any changes to the compiler-generated order of evaluation in the future.</span></span>

### <a name="some-possible-optimizations"></a><span data-ttu-id="18c79-212">Některé možné optimalizace</span><span class="sxs-lookup"><span data-stu-id="18c79-212">Some Possible Optimizations</span></span>

<span data-ttu-id="18c79-213">Kompilace porovnávání se vzorci může využít běžné části vzorů.</span><span class="sxs-lookup"><span data-stu-id="18c79-213">The compilation of pattern matching can take advantage of common parts of patterns.</span></span> <span data-ttu-id="18c79-214">Například pokud je test typu nejvyšší úrovně dvou po sobě jdoucích vzorů v *switch_statement* stejný typ, generovaný kód může přeskočit test typu pro druhý vzor.</span><span class="sxs-lookup"><span data-stu-id="18c79-214">For example, if the top-level type test of two successive patterns in a *switch_statement* is the same type, the generated code can skip the type test for the second pattern.</span></span>

<span data-ttu-id="18c79-215">Pokud jsou některé ze vzorů celé číslo nebo řetězce, kompilátor může generovat stejný druh kódu, který generuje pro příkaz switch-v dřívějších verzích jazyka.</span><span class="sxs-lookup"><span data-stu-id="18c79-215">When some of the patterns are integers or strings, the compiler can generate the same kind of code it generates for a switch-statement in earlier versions of the language.</span></span>

<span data-ttu-id="18c79-216">Další informace o těchto typech optimalizací najdete v článku [[Scott a Ramsey (2000)]](https://www.cs.tufts.edu/~nr/cs257/archive/norman-ramsey/match.pdf "Kdy se shodují s heuristickými heuristickými metodami?").</span><span class="sxs-lookup"><span data-stu-id="18c79-216">For more on these kinds of optimizations, see [[Scott and Ramsey (2000)]](https://www.cs.tufts.edu/~nr/cs257/archive/norman-ramsey/match.pdf "When Do Match-Compilation Heuristics Matter?").</span></span>
