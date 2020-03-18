---
ms.openlocfilehash: 49720d62c496ff904eacacb31ae29ef97a5aefaa
ms.sourcegitcommit: 8152182f0a477cb3082e625b607262cc459a17f3
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/23/2019
ms.locfileid: "79484942"
---
# <a name="records"></a><span data-ttu-id="b827f-101">záznamy</span><span class="sxs-lookup"><span data-stu-id="b827f-101">records</span></span>

* <span data-ttu-id="b827f-102">Navrženo [x]</span><span class="sxs-lookup"><span data-stu-id="b827f-102">[x] Proposed</span></span>
* <span data-ttu-id="b827f-103">[] Prototyp: [dokončeno](https://github.com/PROTOTYPE_OWNER/roslyn/BRANCH_NAME)</span><span class="sxs-lookup"><span data-stu-id="b827f-103">[ ] Prototype: [Complete](https://github.com/PROTOTYPE_OWNER/roslyn/BRANCH_NAME)</span></span>
* <span data-ttu-id="b827f-104">[] [Implementace: probíhá](https://github.com/dotnet/roslyn/BRANCH_NAME)</span><span class="sxs-lookup"><span data-stu-id="b827f-104">[ ] Implementation: [In Progress](https://github.com/dotnet/roslyn/BRANCH_NAME)</span></span>
* <span data-ttu-id="b827f-105">[] Specifikace: specifikace konceptu je uzavřená.</span><span class="sxs-lookup"><span data-stu-id="b827f-105">[ ] Specification: Draft specification enclosed</span></span>

## <a name="summary"></a><span data-ttu-id="b827f-106">Souhrn</span><span class="sxs-lookup"><span data-stu-id="b827f-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="b827f-107">Záznamy jsou nové, zjednodušené formuláře deklarace pro C# typy třídy a struktury, které kombinují výhody řady jednodušších funkcí.</span><span class="sxs-lookup"><span data-stu-id="b827f-107">Records are a new, simplified declaration form for C# class and struct types that combine the benefits of a number of simpler features.</span></span> <span data-ttu-id="b827f-108">Popisujeme nové funkce (parametry volajícího a *s-Expression*s), dávají syntaxi a sémantiku pro deklarace záznamů a pak poskytují několik příkladů.</span><span class="sxs-lookup"><span data-stu-id="b827f-108">We describe the new features (caller-receiver parameters and *with-expression*s), give the syntax and semantics for record declarations, and then provide some examples.</span></span>


## <a name="motivation"></a><span data-ttu-id="b827f-109">Motivační</span><span class="sxs-lookup"><span data-stu-id="b827f-109">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="b827f-110">Významný počet deklarací typu v C# nástroji je trochu více než agregované kolekce typových dat.</span><span class="sxs-lookup"><span data-stu-id="b827f-110">A significant number of type declarations in C# are little more than aggregate collections of typed data.</span></span> <span data-ttu-id="b827f-111">Deklarace takových typů bohužel vyžaduje Skvělé využívání často používaného kódu.</span><span class="sxs-lookup"><span data-stu-id="b827f-111">Unfortunately, declaring such types requires a great deal of boilerplate code.</span></span> <span data-ttu-id="b827f-112">*Záznamy* poskytují mechanismus pro deklaraci datového typu tím, že popisují členy agregace spolu s dodatečným kódem nebo odchylkami od obvyklého často používaného textu, pokud existují.</span><span class="sxs-lookup"><span data-stu-id="b827f-112">*Records* provide a mechanism for declaring a datatype by describing the members of the aggregate along with additional code or deviations from the usual boilerplate, if any.</span></span>

<span data-ttu-id="b827f-113">Viz [Příklady](#examples)níže.</span><span class="sxs-lookup"><span data-stu-id="b827f-113">See [Examples](#examples), below.</span></span>

## <a name="detailed-design"></a><span data-ttu-id="b827f-114">Podrobný návrh</span><span class="sxs-lookup"><span data-stu-id="b827f-114">Detailed design</span></span>
[design]: #detailed-design

### <a name="caller-receiver-parameters"></a><span data-ttu-id="b827f-115">parametry přijímače volajícího</span><span class="sxs-lookup"><span data-stu-id="b827f-115">caller-receiver parameters</span></span>

<span data-ttu-id="b827f-116">V současné době musí být *výchozí argument* parametru metody</span><span class="sxs-lookup"><span data-stu-id="b827f-116">Currently a method parameter's *default-argument* must be</span></span> 
- <span data-ttu-id="b827f-117">*konstantní výraz*; ani</span><span class="sxs-lookup"><span data-stu-id="b827f-117">a *constant-expression*; or</span></span>
- <span data-ttu-id="b827f-118">výraz formuláře `new S()`, kde `S` je hodnotový typ; ani</span><span class="sxs-lookup"><span data-stu-id="b827f-118">an expression of the form `new S()` where `S` is a value type; or</span></span>
- <span data-ttu-id="b827f-119">výraz formuláře `default(S)`, kde `S` je hodnotový typ</span><span class="sxs-lookup"><span data-stu-id="b827f-119">an expression of the form `default(S)` where `S` is a value type</span></span>

<span data-ttu-id="b827f-120">Rozšíříme to tak, aby přidalo následující</span><span class="sxs-lookup"><span data-stu-id="b827f-120">We extend this to add the following</span></span>
- <span data-ttu-id="b827f-121">výraz `this.Identifier` formuláře</span><span class="sxs-lookup"><span data-stu-id="b827f-121">an expression of the form `this.Identifier`</span></span>

<span data-ttu-id="b827f-122">Tento nový formulář se nazývá *výchozí argument typu volající přijímač*a je povolený jenom v případě, že jsou splněné všechny následující podmínky.</span><span class="sxs-lookup"><span data-stu-id="b827f-122">This new form is called a *caller-receiver default-argument*, and is allowed only if all of the following are satisfied</span></span>
- <span data-ttu-id="b827f-123">Metoda, ve které se vyskytuje, je metoda instance; ani</span><span class="sxs-lookup"><span data-stu-id="b827f-123">The method in which it appears is an instance method; and</span></span>
- <span data-ttu-id="b827f-124">Výraz `this.Identifier` váže ke členu instance ohraničujícího typu, který musí být buď pole, nebo vlastnost; ani</span><span class="sxs-lookup"><span data-stu-id="b827f-124">The expression `this.Identifier` binds to an instance member of the enclosing type, which must be either a field or a property; and</span></span>
- <span data-ttu-id="b827f-125">Člen, ke kterému se váže (a přístupový objekt `get`, pokud se jedná o vlastnost) je alespoň tak přístupný jako metoda; ani</span><span class="sxs-lookup"><span data-stu-id="b827f-125">The member to which it binds (and the `get` accessor, if it is a property) is at least as accessible as the method; and</span></span>
- <span data-ttu-id="b827f-126">Typ `this.Identifier` je implicitně převoditelný pomocí identity nebo konverze s možnou hodnotou null na typ parametru (Toto je stávající omezení ve *výchozím argumentu*).</span><span class="sxs-lookup"><span data-stu-id="b827f-126">The type of `this.Identifier` is implicitly convertible by an identity or nullable conversion to the type of the parameter (this is an existing constraint on *default-argument*).</span></span>

<span data-ttu-id="b827f-127">Pokud je argument vynechán od vyvolání člena funkce pro odpovídající volitelný parametr s *výchozím argumentem volajícího přijímače*, hodnota člena příjemce je implicitně předána.</span><span class="sxs-lookup"><span data-stu-id="b827f-127">When an argument is omitted from an invocation of a function member for a corresponding optional parameter with a *caller-receiver default-argument*, the value of the receiver's member is implicitly passed.</span></span> 

> <span data-ttu-id="b827f-128">**Poznámky k návrhu**: hlavní důvod parametru volajícího přijímače je podpora *výrazu with-Expression*.</span><span class="sxs-lookup"><span data-stu-id="b827f-128">**Design Notes**: the main reason for the caller-receiver parameter is to support the *with-expression*.</span></span> <span data-ttu-id="b827f-129">Nápad je, že můžete deklarovat metodu jako tuto</span><span class="sxs-lookup"><span data-stu-id="b827f-129">The idea is that you can declare a method like this</span></span>
> ```cs
> class Point
> {
>     public readonly int X;
>     public readonly int Y;
>     public Point With(int x = this.X, int y = this.Y) => new Point(x, y);
>     // etc
> }
> ```
> <span data-ttu-id="b827f-130">a pak ho použijte takto</span><span class="sxs-lookup"><span data-stu-id="b827f-130">and then use it like this</span></span>
> ```cs
>     Point p = new Point(3, 4);
>     p = p.With(x: 1);
> ```
> <span data-ttu-id="b827f-131">Chcete-li vytvořit novou `Point` stejným způsobem jako stávající `Point` ale s hodnotou `X` změněno.</span><span class="sxs-lookup"><span data-stu-id="b827f-131">To create a new `Point` just like an existing `Point` but with the value of `X` changed.</span></span>
> 
> <span data-ttu-id="b827f-132">Jedná se o otevřenou otázku bez ohledu na to, jestli je syntaktická forma *with-Expression* přidávána až po podpoře parametrů volajícího přijímače, takže by to bylo možné, takže by to mělo být *místo* *kromě* *výrazu with-Expression*.</span><span class="sxs-lookup"><span data-stu-id="b827f-132">It is an open question whether or not the syntactic form of the *with-expression* is worth adding once we have support for caller-receiver parameters, so it is possible we would do this *instead of* rather than *in addition to* the *with-expression*.</span></span>

- <span data-ttu-id="b827f-133">[] **Otevření problému**: Jaké je pořadí, ve kterém je *výchozí argument volajícího přijímače* vyhodnocen s ohledem na jiné argumenty?</span><span class="sxs-lookup"><span data-stu-id="b827f-133">[ ] **Open issue**: What is the order in which a *caller-receiver default-argument* is evaluated with respect to other arguments?</span></span> <span data-ttu-id="b827f-134">Řekněme, že není určen?</span><span class="sxs-lookup"><span data-stu-id="b827f-134">Should we say that it is unspecified?</span></span>

### <a name="with-expressions"></a><span data-ttu-id="b827f-135">výrazy with</span><span class="sxs-lookup"><span data-stu-id="b827f-135">with-expressions</span></span>

<span data-ttu-id="b827f-136">Navrhuje se nový formulář výrazu:</span><span class="sxs-lookup"><span data-stu-id="b827f-136">A new expression form is proposed:</span></span>

```antlr
primary_expression
    : with_expression
    ;

with_expression
    : primary_expression 'with' '{' with_initializer_list '}'
    ;

with_initializer_list
    : with_initializer
    | with_initializer ',' with_initializer_list
    ;

with_initializer
    : identifier '=' expression
    ;
```

<span data-ttu-id="b827f-137">Token `with` je nové klíčové slovo závislé na kontextu.</span><span class="sxs-lookup"><span data-stu-id="b827f-137">The token `with` is a new context-sensitive keyword.</span></span>

<span data-ttu-id="b827f-138">Každý *identifikátor* nalevo od *with_initializer* musí být svázán s polem nebo vlastností přístupné instance typu *primary_expression* *with_expression*.</span><span class="sxs-lookup"><span data-stu-id="b827f-138">Each *identifier* on the left of a *with_initializer* must bind to an accessible instance field or property of the type of the *primary_expression* of the *with_expression*.</span></span> <span data-ttu-id="b827f-139">Mezi těmito identifikátory daného *with_expression*nesmí existovat žádný duplicitní název.</span><span class="sxs-lookup"><span data-stu-id="b827f-139">There may be no duplicated name among these identifiers of a given *with_expression*.</span></span>

<span data-ttu-id="b827f-140">*With_expression* formuláře</span><span class="sxs-lookup"><span data-stu-id="b827f-140">A *with_expression* of the form</span></span>

> <span data-ttu-id="b827f-141">`with` `{` *identifikátoru* *E1* = *e2*,... `}`</span><span class="sxs-lookup"><span data-stu-id="b827f-141">*e1* `with` `{` *identifier* = *e2*, ... `}`</span></span>

<span data-ttu-id="b827f-142">se považuje za vyvolání formuláře.</span><span class="sxs-lookup"><span data-stu-id="b827f-142">is treated as an invocation of the form</span></span>

> <span data-ttu-id="b827f-143">*e1*`.With(`*identifier2*`:` *e2*,...`)`</span><span class="sxs-lookup"><span data-stu-id="b827f-143">*e1*`.With(`*identifier2*`:` *e2*, ...`)`</span></span>

<span data-ttu-id="b827f-144">Kde, pro každou metodu s názvem `With`, která je přístupným členem instance *E1*, vybíráme *identifier2* jako název prvního parametru v této metodě, který má parametr volajícího přijímače, který je stejný člen jako pole instance nebo vlastnost vázaná na *identifikátor*.</span><span class="sxs-lookup"><span data-stu-id="b827f-144">Where, for each method named `With` that is an accessible instance member of *e1*, we select *identifier2* as the name of the first parameter in that method that has a caller-receiver parameter that is the same member as the instance field or property bound to *identifier*.</span></span> <span data-ttu-id="b827f-145">V případě, že žádný takový parametr nelze identifikovat, je-li metoda vyloučena z úvahy.</span><span class="sxs-lookup"><span data-stu-id="b827f-145">If no such parameter can be identified that method is eliminated from consideration.</span></span> <span data-ttu-id="b827f-146">Metoda, která má být vyvolána, je vybrána z zbývajících kandidátů podle řešení přetížení.</span><span class="sxs-lookup"><span data-stu-id="b827f-146">The method to be invoked is selected from among the remaining candidates by overload resolution.</span></span>

> <span data-ttu-id="b827f-147">**Poznámky k návrhu**: zadané parametry přijímače volajícího, mnoho výhod *výrazu with* je k dispozici bez tohoto speciálního formátu syntaxe.</span><span class="sxs-lookup"><span data-stu-id="b827f-147">**Design Notes**: Given caller-receiver parameters, many of the benefits of the *with-expression* are available without this special syntax form.</span></span> <span data-ttu-id="b827f-148">Proto zvažujeme, jestli je to potřeba.</span><span class="sxs-lookup"><span data-stu-id="b827f-148">We are therefore considering whether or not it is needed.</span></span> <span data-ttu-id="b827f-149">Hlavní výhodou je umožnění jednoho na program v podobě názvů polí a vlastností, nikoli z podmínek názvů parametrů.</span><span class="sxs-lookup"><span data-stu-id="b827f-149">Its main benefit is allowing one to program in terms of the names of fields and properties, rather than in terms of the names of parameters.</span></span> <span data-ttu-id="b827f-150">Tímto způsobem vylepšíme čitelnost i kvalitu nástrojů (např. jít na definici na identifikátoru *with_expression* by se místo parametru metody mohly přejít na vlastnost).</span><span class="sxs-lookup"><span data-stu-id="b827f-150">In this way we improve both readability and the quality of tooling (e.g. go-to-definition on the identifier of a *with_expression* would navigate to the property rather than to a method parameter).</span></span>

- <span data-ttu-id="b827f-151">[] **Otevření problému**: Tento popis by měl být upravený tak, aby podporoval metody rozšíření.</span><span class="sxs-lookup"><span data-stu-id="b827f-151">[ ] **Open issue**: This description should be modified to support extension methods.</span></span>

### <a name="pattern-matching"></a><span data-ttu-id="b827f-152">shoda vzoru</span><span class="sxs-lookup"><span data-stu-id="b827f-152">pattern-matching</span></span>

<span data-ttu-id="b827f-153">Pro specifikaci `Deconstruct` a jeho vztahu k porovnávání vzorů se podívejte na [specifikaci porovnávání vzorů](csharp-8.0/patterns.md#positional-pattern) .</span><span class="sxs-lookup"><span data-stu-id="b827f-153">See the [Pattern Matching Specification](csharp-8.0/patterns.md#positional-pattern) for a specification of `Deconstruct` and its relationship to pattern-matching.</span></span>

> <span data-ttu-id="b827f-154">**Poznámky k návrhu**: na základě `Deconstruct` vygenerovaného kompilátorem jak je uvedeno v tomto dokumentu a specifikace pro porovnávání vzorů, deklarace záznamů</span><span class="sxs-lookup"><span data-stu-id="b827f-154">**Design Notes**: By virtue of the compiler-generated `Deconstruct` as specified herein, and the specification for pattern-matching, a record declaration</span></span>
> ```cs
> public class Point(int X, int Y);
> ```
> <span data-ttu-id="b827f-155">bude podporovat poziční porovnávání vzorů následujícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="b827f-155">will support positional pattern-matching as follows</span></span>
> ```cs
> Point p = new Point(3, 4);
> if (p is Point(3, var y)) { // if X is 3
>     Console.WriteLine(y);   // print Y
> }
> ```

### <a name="record-type-declarations"></a><span data-ttu-id="b827f-156">deklarace typu záznamu</span><span class="sxs-lookup"><span data-stu-id="b827f-156">record type declarations</span></span>

<span data-ttu-id="b827f-157">Syntaxe pro `class` nebo deklarace `struct` je rozšířena na parametry hodnot, které jsou podporovány. parametry se stanou vlastnostmi tohoto typu:</span><span class="sxs-lookup"><span data-stu-id="b827f-157">The syntax for a `class` or `struct` declaration is extended to support value parameters; the parameters become properties of the type:</span></span>

```antlr
class_declaration
    : attributes? class_modifiers? 'partial'? 'class' identifier type_parameter_list?
      record_parameters? record_class_base? type_parameter_constraints_clauses? class_body
    ;

struct_declaration
    : attributes? struct_modifiers? 'partial'? 'struct' identifier type_parameter_list?
      record_parameters? struct_interfaces? type_parameter_constraints_clauses? struct_body
    ;

record_class_base
    : class_type record_base_arguments?
    | interface_type_list
    | class_type record_base_arguments? ',' interface_type_list
    ;

record_base_arguments
    : '(' argument_list? ')'
    ;

record_parameters
    : '(' record_parameter_list? ')'
    ;

record_parameter_list
    : record_parameter
    | record_parameter ',' record_parameter_list
    ;

record_parameter
    : attributes? type identifier record_property_name? default_argument?
    ;

record_property_name
    : ':' identifier
    ;

class_body
    : '{' class_member_declarations? '}'
    | ';'
    ;

struct_body
    : '{' struct_members_declarations? '}'
    | ';'
    ;
```

> <span data-ttu-id="b827f-158">**Poznámky k návrhu**: vzhledem k tomu, že typy záznamů jsou často užitečné, aniž by museli být členy explicitně deklarovány v těle třídy, upravujeme syntaxi deklarace, aby tělo bylo možné jednoduše použít jako středník.</span><span class="sxs-lookup"><span data-stu-id="b827f-158">**Design Notes**: Because record types are often useful without the need for any members explicitly declared in a class-body, we modify the syntax of the declaration to allow a body to be simply a semicolon.</span></span>

<span data-ttu-id="b827f-159">Třída (struct) deklarovaná pomocí *parametrů Record* se nazývá *Třída záznamu* (*Struktura záznamu*), která je *typu záznamu*.</span><span class="sxs-lookup"><span data-stu-id="b827f-159">A class (struct) declared with the *record-parameters* is called a *record class* (*record struct*), either of which is a *record type*.</span></span>

- <span data-ttu-id="b827f-160">[] **Otevření problému**: musíme do gramatiky zahrnout *primary_constructor_body* , aby se mohl objevit v rámci deklarace typu záznamu.</span><span class="sxs-lookup"><span data-stu-id="b827f-160">[ ] **Open issue**: We need to include *primary_constructor_body* in the grammar so that it can appear inside a record type declaration.</span></span>
- <span data-ttu-id="b827f-161">[] **Otevření problému**: jaká jsou pravidla konfliktů názvů pro názvy parametrů?</span><span class="sxs-lookup"><span data-stu-id="b827f-161">[ ] **Open issue**: What are the name conflict rules for the parameter names?</span></span> <span data-ttu-id="b827f-162">Pravděpodobně jedna z nich není povolena v konfliktu s parametrem typu nebo jiným *parametrem záznamu*.</span><span class="sxs-lookup"><span data-stu-id="b827f-162">Presumably one is not allowed to conflict with a type parameter or another *record-parameter*.</span></span>
- <span data-ttu-id="b827f-163">[] **Otevření problému**: musíme zadat rozsah parametrů záznamu.</span><span class="sxs-lookup"><span data-stu-id="b827f-163">[ ] **Open issue**: We need to specify the scope of the record-parameters.</span></span> <span data-ttu-id="b827f-164">Kde je lze použít?</span><span class="sxs-lookup"><span data-stu-id="b827f-164">Where can they be used?</span></span> <span data-ttu-id="b827f-165">V rámci inicializátorů polí instance a *primary_constructor_body* nejméně.</span><span class="sxs-lookup"><span data-stu-id="b827f-165">Presumably within instance field initializers and *primary_constructor_body* at least.</span></span>
- <span data-ttu-id="b827f-166">[] **Otevření problému**: může být deklarace typu záznamu částečná?</span><span class="sxs-lookup"><span data-stu-id="b827f-166">[ ] **Open issue**: Can a record type declaration be partial?</span></span> <span data-ttu-id="b827f-167">Pokud ano, musíte parametry opakovat v každé části?</span><span class="sxs-lookup"><span data-stu-id="b827f-167">If so, must the parameters be repeated on each part?</span></span>

#### <a name="members-of-a-record-type"></a><span data-ttu-id="b827f-168">Členové typu záznamu</span><span class="sxs-lookup"><span data-stu-id="b827f-168">Members of a record type</span></span>

<span data-ttu-id="b827f-169">Kromě členů deklarovaných v *těle třídy*má typ záznamu následující další členy:</span><span class="sxs-lookup"><span data-stu-id="b827f-169">In addition to the members declared in the *class-body*, a record type has the following additional members:</span></span>

##### <a name="primary-constructor"></a><span data-ttu-id="b827f-170">Primární konstruktor</span><span class="sxs-lookup"><span data-stu-id="b827f-170">Primary Constructor</span></span>

<span data-ttu-id="b827f-171">Typ záznamu má konstruktor `public`, jehož signatura odpovídá parametrům hodnoty deklarace typu.</span><span class="sxs-lookup"><span data-stu-id="b827f-171">A record type has a `public` constructor whose signature corresponds to the value parameters of the type declaration.</span></span> <span data-ttu-id="b827f-172">Označuje se jako *primární konstruktor* pro daný typ a způsobí, že implicitně deklarovaný *výchozí konstruktor* bude potlačen.</span><span class="sxs-lookup"><span data-stu-id="b827f-172">This is called the *primary constructor* for the type, and causes the implicitly declared *default constructor* to be suppressed.</span></span>

<span data-ttu-id="b827f-173">Za běhu primárního konstruktoru</span><span class="sxs-lookup"><span data-stu-id="b827f-173">At runtime the primary constructor</span></span>

* <span data-ttu-id="b827f-174">Inicializuje pole zálohování generované kompilátorem pro vlastnosti odpovídající parametrům hodnoty (pokud jsou tyto vlastnosti poskytovány kompilátorem; [viz 1.1.2](#1.1.2)); Stisknutím</span><span class="sxs-lookup"><span data-stu-id="b827f-174">initializes compiler-generated backing fields for the properties corresponding to the value parameters (if these properties are compiler-provided; [see 1.1.2](#1.1.2)); then</span></span>
* <span data-ttu-id="b827f-175">spustí Inicializátory pole instance uvedené v *těle třídy*; a potom</span><span class="sxs-lookup"><span data-stu-id="b827f-175">executes the instance field initializers appearing in the *class-body*; and then</span></span>
* <span data-ttu-id="b827f-176">vyvolá konstruktor základní třídy:</span><span class="sxs-lookup"><span data-stu-id="b827f-176">invokes a base class constructor:</span></span>
    * <span data-ttu-id="b827f-177">Pokud jsou v *record_base_arguments*argumenty, je vyvolán základní konstruktor vybraný pomocí řešení přetížení s těmito argumenty;</span><span class="sxs-lookup"><span data-stu-id="b827f-177">If there are arguments in the *record_base_arguments*, a base constructor selected by overload resolution with these arguments is invoked;</span></span>
    * <span data-ttu-id="b827f-178">V opačném případě je základní konstruktor vyvolán bez argumentů.</span><span class="sxs-lookup"><span data-stu-id="b827f-178">Otherwise a base constructor is invoked with no arguments.</span></span>
* <span data-ttu-id="b827f-179">spustí tělo každé *primary_constructor_body*, pokud existuje, ve zdrojové objednávce.</span><span class="sxs-lookup"><span data-stu-id="b827f-179">executes the body of each *primary_constructor_body*, if any, in source order.</span></span>

- <span data-ttu-id="b827f-180">[] **Otevření problému**: musíme specifikovat toto pořadí, zejména u všech kompilačních jednotek pro částečné.</span><span class="sxs-lookup"><span data-stu-id="b827f-180">[ ] **Open issue**: We need to specify that order, particularly across compilation units for partials.</span></span>
- <span data-ttu-id="b827f-181">[] **Otevření problému**: musíme specifikovat, že každý explicitně deklarovaný konstruktor musí být zřetězený s primárním konstruktorem.</span><span class="sxs-lookup"><span data-stu-id="b827f-181">[ ] **Open Issue**: We need to specify that every explicitly declared constructor must chain to the primary constructor.</span></span>
- <span data-ttu-id="b827f-182">[] **Otevření problému**: by mělo být povoleno změnit modifikátor přístupu u primárního konstruktoru?</span><span class="sxs-lookup"><span data-stu-id="b827f-182">[ ] **Open issue**: Should it be allowed to change the access modifier on the primary constructor?</span></span>
- <span data-ttu-id="b827f-183">[] **Otevření problému**: v struktuře záznamu je chyba, že neexistují žádné parametry záznamu?</span><span class="sxs-lookup"><span data-stu-id="b827f-183">[ ] **Open issue**: In a record struct, it is an error for there to be no record parameters?</span></span>

##### <a name="primary-constructor-body"></a><span data-ttu-id="b827f-184">Tělo primárního konstruktoru</span><span class="sxs-lookup"><span data-stu-id="b827f-184">Primary constructor body</span></span>

```antlr
primary_constructor_body
    : attributes? constructor_modifiers? identifier block
    ;
```

<span data-ttu-id="b827f-185">*Primary_constructor_body* lze použít pouze v rámci deklarace typu záznamu.</span><span class="sxs-lookup"><span data-stu-id="b827f-185">A *primary_constructor_body* may only be used within a record type declaration.</span></span> <span data-ttu-id="b827f-186">*Identifikátor* *primary_constructor_body* musí pojmenovat typ záznamu, ve kterém je deklarován.</span><span class="sxs-lookup"><span data-stu-id="b827f-186">The *identifier* of a *primary_constructor_body* shall name the record type in which it is declared.</span></span>

<span data-ttu-id="b827f-187">*Primary_constructor_body* sám nedeklaruje člena, ale je to způsob, jak programátorovi poskytnout atributy pro a zadat přístup k primárnímu konstruktoru typu záznamu.</span><span class="sxs-lookup"><span data-stu-id="b827f-187">The *primary_constructor_body* does not declare a member on its own, but is a way for the programmer to provide attributes for, and specify the access of, a record type's primary constructor.</span></span> <span data-ttu-id="b827f-188">Umožňuje také programátorovi poskytnout další kód, který se spustí, když je vytvořena instance typu záznamu.</span><span class="sxs-lookup"><span data-stu-id="b827f-188">It also enables the programmer to provide additional code that will be executed when an instance of the record type is constructed.</span></span>

- <span data-ttu-id="b827f-189">[] **Otevření problému**: Nezapomeňte si uvědomit, že výchozí konstruktor struktury ho přeskočí.</span><span class="sxs-lookup"><span data-stu-id="b827f-189">[ ] **Open issue**: We should note that a struct default constructor bypasses this.</span></span>
- <span data-ttu-id="b827f-190">[] **Otevření problému**: je potřeba zadat pořadí spouštění inicializace.</span><span class="sxs-lookup"><span data-stu-id="b827f-190">[ ] **Open issue**: We should specify the execution order of initialization.</span></span>
- <span data-ttu-id="b827f-191">[] **Otevření problému**: Pokud máme v deklaraci typu nezáznamový typ něco jako *primary_constructor_body* (předpokládá se, že neexistují atributy a modifikátory), a považujeme za to, že by to byl kód inicializátoru pole instance?</span><span class="sxs-lookup"><span data-stu-id="b827f-191">[ ] **Open issue**: Should we allow something like a *primary_constructor_body* (presumably without attributes and modifiers) in a non-record type declaration, and treat it like we would the code of an instance field initializer?</span></span>

##### <a name="properties"></a><span data-ttu-id="b827f-192">Vlastnosti</span><span class="sxs-lookup"><span data-stu-id="b827f-192">Properties</span></span>

<span data-ttu-id="b827f-193">Pro každý parametr záznamu deklarace typu záznamu existuje odpovídající člen vlastnosti `public`, jehož název a typ jsou převedené z deklarace parametru value.</span><span class="sxs-lookup"><span data-stu-id="b827f-193">For each record parameter of a record type declaration there is a corresponding `public` property member whose name and type are taken from the value parameter declaration.</span></span> <span data-ttu-id="b827f-194">Jeho název je *identifikátor* *record_property_name*, pokud je k dispozici, nebo *identifikátor* *record_parameter* jinak.</span><span class="sxs-lookup"><span data-stu-id="b827f-194">Its name is the *identifier* of the *record_property_name*, if present, or the *identifier* of the *record_parameter* otherwise.</span></span> <span data-ttu-id="b827f-195">Pokud žádná konkrétní (tj. neabstraktní) veřejná vlastnost s přistupujícím objektem `get` a s tímto názvem a typem je explicitně deklarována nebo zděděna, je vytvořena kompilátorem následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="b827f-195">If no concrete (i.e. non-abstract) public property with a `get` accessor and with this name and type is explicitly declared or inherited, it is produced by the compiler as follows:</span></span>

* <span data-ttu-id="b827f-196">Pro *strukturu záznamu* nebo *třídu záznamu*`sealed`:</span><span class="sxs-lookup"><span data-stu-id="b827f-196">For a *record struct* or a `sealed` *record class*:</span></span>
 * <span data-ttu-id="b827f-197">`private` `readonly` pole je vytvořeno jako pole zálohování pro vlastnost `readonly`.</span><span class="sxs-lookup"><span data-stu-id="b827f-197">A `private` `readonly` field is produced as a backing field for a `readonly` property.</span></span> <span data-ttu-id="b827f-198">Jeho hodnota je inicializována během konstrukce s hodnotou odpovídajícího primárního parametru konstruktoru.</span><span class="sxs-lookup"><span data-stu-id="b827f-198">Its value is initialized during construction with the value of the corresponding primary constructor parameter.</span></span>
 * <span data-ttu-id="b827f-199">Vlastnost přístupového objektu `get` je implementovaná pro návratovou hodnotu pole pro zálohování.</span><span class="sxs-lookup"><span data-stu-id="b827f-199">The property's `get` accessor is implemented to return the value of the backing field.</span></span>
 * <span data-ttu-id="b827f-200">Všechny "párové" zděděné přístupové objekty `get` virtuální vlastnosti jsou přepsány.</span><span class="sxs-lookup"><span data-stu-id="b827f-200">Each "matching" inherited virtual property's `get` accessor is overridden.</span></span>

> <span data-ttu-id="b827f-201">**Poznámky k návrhu**: jinými slovy Pokud rozšiřujete základní třídu nebo implementujete rozhraní, které deklaruje veřejnou abstraktní vlastnost se stejným názvem a typem jako parametr záznamu, je tato vlastnost přepsána nebo implementována.</span><span class="sxs-lookup"><span data-stu-id="b827f-201">**Design notes**: In other words, if you extend a base class or implement an interface that declares a public abstract property with the same name and type as a record parameter, that property is overridden or implemented.</span></span>

- <span data-ttu-id="b827f-202">[] **Otevření problému**: je možné změnit modifikátor přístupu u vlastnosti, pokud je explicitně deklarována?</span><span class="sxs-lookup"><span data-stu-id="b827f-202">[ ] **Open issue**: Should it be possible to change the access modifier on a property when it is explicitly declared?</span></span>
- <span data-ttu-id="b827f-203">[] **Otevření problému**: mělo by být možné nahradit pole pro vlastnost?</span><span class="sxs-lookup"><span data-stu-id="b827f-203">[ ] **Open issue**: Should it be possible to substitute a field for a property?</span></span>

##### <a name="object-methods"></a><span data-ttu-id="b827f-204">Metody objektů</span><span class="sxs-lookup"><span data-stu-id="b827f-204">Object Methods</span></span>

<span data-ttu-id="b827f-205">V případě *struktury záznamu* nebo *třídy záznamu*`sealed` jsou implementace metod `object.GetHashCode()` a `object.Equals(object)` vytvářeny kompilátorem, pokud jej neposkytuje uživatel.</span><span class="sxs-lookup"><span data-stu-id="b827f-205">For a *record struct* or a `sealed` *record class*, implementations of the methods `object.GetHashCode()` and `object.Equals(object)` are produced by the compiler unless provided by the user.</span></span>

- <span data-ttu-id="b827f-206">[] **Otevření problému**: doporučujeme přesně zadat implementaci.</span><span class="sxs-lookup"><span data-stu-id="b827f-206">[ ] **Open issue**: We should precisely specify their implementation.</span></span>
- <span data-ttu-id="b827f-207">[] **Otevření problému**: Doporučujeme také přidat rozhraní `IEquatable<T>` pro daný typ záznamu a určit, že implementace jsou k dispozici.</span><span class="sxs-lookup"><span data-stu-id="b827f-207">[ ] **Open issue**: We should also add the interface `IEquatable<T>` for the record type and specify that implementations are provided.</span></span>
- <span data-ttu-id="b827f-208">[] **Otevření problému**: Doporučujeme také určit, že implementujeme všechny `IEquatable<T>.Equals`.</span><span class="sxs-lookup"><span data-stu-id="b827f-208">[ ] **Open issue**: We should also specify that we implement every `IEquatable<T>.Equals`.</span></span>
- <span data-ttu-id="b827f-209">[] **Otevření problému**: doporučujeme přesně zadat, jak řešíme problém rovnosti záznamů na začátku dědičnosti záznamu: konkrétně způsob, jakým generujeme metody rovnosti tak, aby byly symetrické, přenositelné, reflexivní atd.</span><span class="sxs-lookup"><span data-stu-id="b827f-209">[ ] **Open issue**: We should specify precisely how we solve the problem of Equals in the face of record inheritance: specifically how we generate equality methods such that they are symmetric, transitive, reflexive, etc.</span></span>
- <span data-ttu-id="b827f-210">[] **Otevření problému**: bylo navrženo, že pro typy záznamů implementujeme `operator ==` a `operator !=`.</span><span class="sxs-lookup"><span data-stu-id="b827f-210">[ ] **Open issue**: It has been proposed that we implement `operator ==` and `operator !=` for record types.</span></span>
- <span data-ttu-id="b827f-211">[] **Otevření problému**: máme automaticky vygenerovat implementaci `object.ToString`?</span><span class="sxs-lookup"><span data-stu-id="b827f-211">[ ] **Open issue**: Should we auto-generate an implementation of `object.ToString`?</span></span>

##### `Deconstruct`

<span data-ttu-id="b827f-212">Typ záznamu obsahuje metodu `public` generovanou kompilátorem `void Deconstruct`, pokud uživatel nezadá nějaký podpis.</span><span class="sxs-lookup"><span data-stu-id="b827f-212">A record type has a compiler-generated `public` method `void Deconstruct` unless one with any signature is provided by the user.</span></span> <span data-ttu-id="b827f-213">Každý parametr je `out` parametr se stejným názvem a typem jako odpovídající parametr typu záznamu.</span><span class="sxs-lookup"><span data-stu-id="b827f-213">Each parameter is an `out` parameter of the same name and type as the corresponding parameter of the record type.</span></span> <span data-ttu-id="b827f-214">Implementace této metody pomocí kompilátoru přiřadí každému `out` parametru hodnotu odpovídající vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="b827f-214">The compiler-provided implementation of this method shall assign each `out` parameter with the value of the corresponding property.</span></span>

<span data-ttu-id="b827f-215">Prohlédněte [si specifikaci porovnávání vzorů](csharp-8.0/patterns.md#positional-pattern) pro sémantiku `Deconstruct`.</span><span class="sxs-lookup"><span data-stu-id="b827f-215">See [the pattern-matching specification](csharp-8.0/patterns.md#positional-pattern) for the semantics of `Deconstruct`.</span></span>

##### <a name="with-method"></a><span data-ttu-id="b827f-216">`With` – metoda</span><span class="sxs-lookup"><span data-stu-id="b827f-216">`With` method</span></span>

<span data-ttu-id="b827f-217">Pokud není k dispozici uživatelsky deklarovaný člen s názvem `With` deklarované, typ záznamu má metodu poskytnutou kompilátorem s názvem `With`, jejíž návratový typ je samotný typ záznamu a obsahuje jeden parametr hodnoty odpovídající každému *parametru záznamu* ve stejném pořadí, že tyto parametry jsou uvedeny v deklaraci typu záznamu.</span><span class="sxs-lookup"><span data-stu-id="b827f-217">Unless there is a user-declared member named `With` declared, a record type has a compiler-provided method named `With` whose return type is the record type itself, and containing one value parameter corresponding to each *record-parameter* in the same order that these parameters appear in the record type declaration.</span></span> <span data-ttu-id="b827f-218">Každý parametr musí mít *výchozí argument typu volajícího přijímače* odpovídající vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="b827f-218">Each parameter shall have a *caller-receiver default-argument* of the corresponding property.</span></span>

<span data-ttu-id="b827f-219">V třídě `abstract` záznamu je metoda `With` poskytnutá kompilátorem abstraktní.</span><span class="sxs-lookup"><span data-stu-id="b827f-219">In an `abstract` record class, the compiler-provided `With` method is abstract.</span></span> <span data-ttu-id="b827f-220">V rámci struktury záznamu nebo zapečetěné třídy záznamů je `With` metoda poskytnutá kompilátorem `sealed`.</span><span class="sxs-lookup"><span data-stu-id="b827f-220">In a record struct, or a sealed record class, the compiler-provided `With` method is `sealed`.</span></span> <span data-ttu-id="b827f-221">V opačném případě `With` metoda, která je součástí kompilátoru, je virtuální a její implementace vrátí novou instanci vytvořenou vyvoláním primárního konstruktoru s parametry jako argumenty pro vytvoření nové instance z parametrů a vrácení této nové instance.</span><span class="sxs-lookup"><span data-stu-id="b827f-221">Otherwise the compiler-provided `With` method is \`virtual and its implementation shall return a new instance produced by invoking the primary constructor with the parameters as arguments to create a new instance from the parameters, and return that new instance.</span></span>

- <span data-ttu-id="b827f-222">[] **Otevření problému**: Doporučujeme také určit, za jakých podmínek přepíšeme nebo implementujete zděděné metody Virtual `With` nebo `With` metody z implementovaných rozhraní.</span><span class="sxs-lookup"><span data-stu-id="b827f-222">[ ] **Open issue**: We should also specify under what conditions we override or implement inherited virtual `With` methods or `With` methods from implemented interfaces.</span></span>
- <span data-ttu-id="b827f-223">[] **Otevření problému**: máme to říci, co se stane, když zdědíme nevirtuální `With` metodu.</span><span class="sxs-lookup"><span data-stu-id="b827f-223">[ ] **Open issue**: We should say what happens when we inherit a non-virtual `With` method.</span></span>

> <span data-ttu-id="b827f-224">**Poznámky k návrhu**: vzhledem k tomu, že typy záznamů jsou ve výchozím nastavení neměnné, poskytuje metoda `With` způsob vytvoření nové instance, která je stejná jako existující instance, ale s vybranými vlastnostmi pro nové hodnoty.</span><span class="sxs-lookup"><span data-stu-id="b827f-224">**Design notes**: Because record types are by default immutable, the `With` method provides a way of creating a new instance that is the same as an existing instance but with selected properties given new values.</span></span> <span data-ttu-id="b827f-225">Například zadaný</span><span class="sxs-lookup"><span data-stu-id="b827f-225">For example, given</span></span>
> ```cs
> public class Point(int X, int Y);
> ```
> <span data-ttu-id="b827f-226">k dispozici je člen poskytnutý kompilátorem.</span><span class="sxs-lookup"><span data-stu-id="b827f-226">there is a compiler-provided member</span></span>
> ```cs
>     public virtual Point With(int X = this.X, int Y = this.Y) => new Point(X, Y);
> ```
> <span data-ttu-id="b827f-227">Který povoluje proměnnou typu záznamu</span><span class="sxs-lookup"><span data-stu-id="b827f-227">Which enables an variable of the record type</span></span>
> ```cs
> var p = new Point(3, 4);
> ```
> <span data-ttu-id="b827f-228">má být nahrazena instancí, která má jednu nebo více různých vlastností.</span><span class="sxs-lookup"><span data-stu-id="b827f-228">to be replaced with an instance that has one or more properties different</span></span>
> ```cs
>     p = p.With(X: 5);
> ```
> <span data-ttu-id="b827f-229">To se dá také vyjádřit pomocí *with_expression*:</span><span class="sxs-lookup"><span data-stu-id="b827f-229">This can also be expressed using the *with_expression*:</span></span>
> ```cs
>     p = p with { X = 5 };
> ```

### <a name="examples"></a><span data-ttu-id="b827f-230">Příklady</span><span class="sxs-lookup"><span data-stu-id="b827f-230">Examples</span></span>

#### <a name="compatibility-of-record-types"></a><span data-ttu-id="b827f-231">Kompatibilita typů záznamů</span><span class="sxs-lookup"><span data-stu-id="b827f-231">Compatibility of record types</span></span>

<span data-ttu-id="b827f-232">Vzhledem k tomu, že programátor může přidat členy do deklarace typu záznamu, je často možné změnit sadu prvků záznamu, aniž by to ovlivnilo stávající klienty.</span><span class="sxs-lookup"><span data-stu-id="b827f-232">Because the programmer can add members to a record type declaration, it is often possible to change the set of record elements without affecting existing clients.</span></span> <span data-ttu-id="b827f-233">Například s ohledem na počáteční verzi typu záznamu</span><span class="sxs-lookup"><span data-stu-id="b827f-233">For example, given an initial version of a record type</span></span>

```cs
// v1
public class Person(string Name, DateTime DateOfBirth);
```

<span data-ttu-id="b827f-234">Nový prvek typu záznamu lze compatibly přidat do další revize typu, aniž by to ovlivnilo binární nebo zdrojovou kompatibilitu:</span><span class="sxs-lookup"><span data-stu-id="b827f-234">A new element of the record type can be compatibly added in the next revision of the type without affecting binary or source compatibility:</span></span>

```cs
// v2
public class Person(string Name, DateTime DateOfBirth, string HomeTown)
{
    // Note: below operations added to retain binary compatibility with v1
    public Person(string Name, DateTime DateOfBirth) : this(Name, DateOfBirth, string.Empty) {}
    public static void operator is(Person self, out string Name, out DateTime DateOfBirth)
        { Name = self.Name; DateOfBirth = self.DateOfBirth; }
    public Person With(string Name, DateTime DateOfBirth) => new Person(Name, DateOfBirth);
}
```

#### <a name="record-struct-example"></a><span data-ttu-id="b827f-235">Příklad struktury záznamu</span><span class="sxs-lookup"><span data-stu-id="b827f-235">record struct example</span></span>

<span data-ttu-id="b827f-236">Tato struktura záznamu</span><span class="sxs-lookup"><span data-stu-id="b827f-236">This record struct</span></span>

```cs
public struct Pair(object First, object Second);
```

<span data-ttu-id="b827f-237">je přeloženo na tento kód</span><span class="sxs-lookup"><span data-stu-id="b827f-237">is translated to this code</span></span>

```cs
public struct Pair : IEquatable<Pair>
{
    public object First { get; }
    public object Second { get; }
    public Pair(object First, object Second)
    {
        this.First = First;
        this.Second = Second;
    }
    public bool Equals(Pair other) // for IEquatable<Pair>
    {
        return Equals(First, other.First) && Equals(Second, other.Second);
    }
    public override bool Equals(object other)
    {
        return (other as Pair)?.Equals(this) == true;
    }
    public override int GetHashCode()
    {
        return (First?.GetHashCode()*17 + Second?.GetHashCode()).GetValueOrDefault();
    }
    public Pair With(object First = this.First, object Second = this.Second) => new Pair(First, Second);
    public void Deconstruct(out object First, out object Second)
    {
        First = self.First;
        Second = self.Second;
    }
}
```

- <span data-ttu-id="b827f-238">[] **Otevření problému**: má-li být implementace rovnosti (dvojice jiných) veřejným členem dvojice?</span><span class="sxs-lookup"><span data-stu-id="b827f-238">[ ] **Open issue**: should the implementation of Equals(Pair other) be a public member of Pair?</span></span>
- <span data-ttu-id="b827f-239">[] **Otevření problému**: tato implementace `Equals` není symetrická na tváři dědičnosti.</span><span class="sxs-lookup"><span data-stu-id="b827f-239">[ ] **Open issue**: This implementation of `Equals` is not symmetric in the face of inheritance.</span></span>
- <span data-ttu-id="b827f-240">[] **Otevření problému**: by měl záznam deklarovat `operator ==` a `operator !=`?</span><span class="sxs-lookup"><span data-stu-id="b827f-240">[ ] **Open issue**: Should a record declare `operator ==` and `operator !=`?</span></span>

> <span data-ttu-id="b827f-241">**Poznámky k návrhu**: vzhledem k tomu, že jeden typ záznamu může dědit z jiné a tato implementace `Equals` by v takovém případě nebyla v takovém případě symetrická, není správná.</span><span class="sxs-lookup"><span data-stu-id="b827f-241">**Design notes**: Because one record type can inherit from another, and this implementation of `Equals` would not be symmetric in that case, it is not correct.</span></span> <span data-ttu-id="b827f-242">Navrhli jsme implementovat rovnost tímto způsobem:</span><span class="sxs-lookup"><span data-stu-id="b827f-242">We propose to implement equality this way:</span></span>
> ```cs
>     public bool Equals(Pair other) // for IEquatable<Pair>
>     {
>         return other != null && EqualityContract == other.EqualityContract &&
>             Equals(First, other.First) && Equals(Second, other.Second);
>     }
>     protected virtual Type EqualityContract => typeof(Pair);
> ```
> <span data-ttu-id="b827f-243">Odvozené záznamy by `override EqualityContract`.</span><span class="sxs-lookup"><span data-stu-id="b827f-243">Derived records would `override EqualityContract`.</span></span> <span data-ttu-id="b827f-244">Méně atraktivní alternativou je omezení dědičnosti.</span><span class="sxs-lookup"><span data-stu-id="b827f-244">The less attractive alternative is to restrict inheritance.</span></span>

#### <a name="sealed-record-example"></a><span data-ttu-id="b827f-245">Příklad zapečetěného záznamu</span><span class="sxs-lookup"><span data-stu-id="b827f-245">sealed record example</span></span>

<span data-ttu-id="b827f-246">Tato zapečetěná třída záznamu</span><span class="sxs-lookup"><span data-stu-id="b827f-246">This sealed record class</span></span>

```cs
public sealed class Student(string Name, decimal Gpa);
```

<span data-ttu-id="b827f-247">se převede do tohoto kódu.</span><span class="sxs-lookup"><span data-stu-id="b827f-247">is translated into this code</span></span>

```cs
public sealed class Student : IEquatable<Student>
{
    public string Name { get; }
    public decimal Gpa { get; }
    public Student(string Name, decimal Gpa)
    {
        this.Name = Name;
        this.Gpa = Gpa;
    }
    public bool Equals(Student other) // for IEquatable<Student>
    {
        return other != null && Equals(Name, other.Name) && Equals(Gpa, other.Gpa);
    }
    public override bool Equals(object other)
    {
        return this.Equals(other as Student);
    }
    public override int GetHashCode()
    {
        return (Name?.GetHashCode()*17 + Gpa?.GetHashCode()).GetValueOrDefault();
    }
    public Student With(string Name = this.Name, decimal Gpa = this.Gpa) => new Student(Name, Gpa);
    public void Deconstruct(out string Name, out decimal Gpa)
    {
        Name = self.Name;
        Gpa = self.Gpa;
    }
}
```

#### <a name="abstract-record-class-example"></a><span data-ttu-id="b827f-248">příklad třídy abstraktního záznamu</span><span class="sxs-lookup"><span data-stu-id="b827f-248">abstract record class example</span></span>

<span data-ttu-id="b827f-249">Tato abstraktní třída záznamů</span><span class="sxs-lookup"><span data-stu-id="b827f-249">This abstract record class</span></span>

```cs
public abstract class Person(string Name);
```

<span data-ttu-id="b827f-250">se převede do tohoto kódu.</span><span class="sxs-lookup"><span data-stu-id="b827f-250">is translated into this code</span></span>

```cs
public abstract class Person : IEquatable<Person>
{
    public string Name { get; }
    public Person(string Name)
    {
        this.Name = Name;
    }
    public bool Equals(Person other)
    {
        return other != null && Equals(Name, other.Name);
    }
    public override bool Equals(object other)
    {
        return Equals(other as Person);
    }
    public override int GetHashCode()
    {
        return (Name?.GetHashCode()).GetValueOrDefault();
    }
    public abstract Person With(string Name = this.Name);
    public void Deconstruct(Person self, out string Name)
    {
        Name = self.Name;
    }
}
```

#### <a name="combining-abstract-and-sealed-records"></a><span data-ttu-id="b827f-251">kombinování abstraktních a zapečetěných záznamů</span><span class="sxs-lookup"><span data-stu-id="b827f-251">combining abstract and sealed records</span></span>

<span data-ttu-id="b827f-252">Byla zadána třída abstraktního záznamu `Person` výše, tato třída zapečetěného záznamu</span><span class="sxs-lookup"><span data-stu-id="b827f-252">Given the abstract record class `Person` above, this sealed record class</span></span>

```cs
public sealed class Student(string Name, decimal Gpa) : Person(Name);
```

<span data-ttu-id="b827f-253">se převede do tohoto kódu.</span><span class="sxs-lookup"><span data-stu-id="b827f-253">is translated into this code</span></span>

```cs
public sealed class Student : Person, IEquatable<Student>
{
    public override string Name { get; }
    public decimal Gpa { get; }
    public Student(string Name, decimal Gpa) : base(Name)
    {
        this.Name = Name;
        this.Gpa = Gpa;
    }
    public override bool Equals(Student other) // for IEquatable<Student>
    {
        return Equals(Name, other.Name) && Equals(Gpa, other.Gpa);
    }
    public bool Equals(Person other) // for IEquatable<Person>
    {
        return (other as Student)?.Equals(this) == true;
    }
    public override bool Equals(object other)
    {
        return (other as Student)?.Equals(this) == true;
    }
    public override int GetHashCode()
    {
        return (Name?.GetHashCode()*17 + Gpa.GetHashCode()).GetValueOrDefault();
    }
    public Student With(string Name = this.Name, decimal Gpa = this.Gpa) => new Student(Name, Gpa);
    public override Person With(string Name = this.Name) => new Student(Name, Gpa);
    public void Deconstruct(Student self, out string Name, out decimal Gpa)
    {
        Name = self.Name;
        Gpa = self.Gpa;
    }
}
```

## <a name="drawbacks"></a><span data-ttu-id="b827f-254">Nevýhody</span><span class="sxs-lookup"><span data-stu-id="b827f-254">Drawbacks</span></span>
[drawbacks]: #drawbacks

<span data-ttu-id="b827f-255">Stejně jako u libovolné jazykové funkce musíme dotazovat, jestli se další složitost tohoto jazyka znovu vyplácí v další čistotě, která je k dispozici pro tělo C# programů, které by mohly využít výhod této funkce.</span><span class="sxs-lookup"><span data-stu-id="b827f-255">As with any language feature, we must question whether the additional complexity to the language is repaid in the additional clarity offered to the body of C# programs that would benefit from the feature.</span></span>

## <a name="alternatives"></a><span data-ttu-id="b827f-256">Alternativy</span><span class="sxs-lookup"><span data-stu-id="b827f-256">Alternatives</span></span>
[alternatives]: #alternatives

<span data-ttu-id="b827f-257">Za 6 se C# považujeme za přidání *primárních konstruktorů* .</span><span class="sxs-lookup"><span data-stu-id="b827f-257">We considered adding *primary constructors* in C# 6.</span></span> <span data-ttu-id="b827f-258">I když zabírají stejnou syntaktickou plochu jako tento návrh, zjistili jsme, že se jedná o krátké výhody nabízené záznamy.</span><span class="sxs-lookup"><span data-stu-id="b827f-258">Although they occupy the same syntactic surface as this proposal, we found that they fell short of the advantages offered by records.</span></span>

## <a name="unresolved-questions"></a><span data-ttu-id="b827f-259">Nevyřešené dotazy</span><span class="sxs-lookup"><span data-stu-id="b827f-259">Unresolved questions</span></span>
[unresolved]: #unresolved-questions

<span data-ttu-id="b827f-260">Otevřené otázky se zobrazí v těle návrhu.</span><span class="sxs-lookup"><span data-stu-id="b827f-260">Open questions appear in the body of the proposal.</span></span>

## <a name="design-meetings"></a><span data-ttu-id="b827f-261">Schůzky pro návrh</span><span class="sxs-lookup"><span data-stu-id="b827f-261">Design meetings</span></span>

<span data-ttu-id="b827f-262">Bude doplněno</span><span class="sxs-lookup"><span data-stu-id="b827f-262">TBD</span></span>
