---
ms.openlocfilehash: b7bb7dd575d9e2e6d5dd85bdd3e535411e29fcf4
ms.sourcegitcommit: 3fc033b6e98ed7ecdf46a85c79b00a3a3ddcf963
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/18/2019
ms.locfileid: "47229901"
---
# <a name="variables"></a><span data-ttu-id="9fd41-101">Proměnné</span><span class="sxs-lookup"><span data-stu-id="9fd41-101">Variables</span></span>

<span data-ttu-id="9fd41-102">Proměnné představují umístění úložiště.</span><span class="sxs-lookup"><span data-stu-id="9fd41-102">Variables represent storage locations.</span></span> <span data-ttu-id="9fd41-103">Každá proměnná má typ, který určuje, jaké hodnoty můžou být uložené v proměnné.</span><span class="sxs-lookup"><span data-stu-id="9fd41-103">Every variable has a type that determines what values can be stored in the variable.</span></span> <span data-ttu-id="9fd41-104">C# je jazyk zajišťující bezpečnost typů a kompilátor jazyka C# zaručuje, že hodnoty uložené v proměnné jsou vždy příslušného typu.</span><span class="sxs-lookup"><span data-stu-id="9fd41-104">C# is a type-safe language, and the C# compiler guarantees that values stored in variables are always of the appropriate type.</span></span> <span data-ttu-id="9fd41-105">Hodnota proměnné lze změnit prostřednictvím přiřazení nebo použití `++` a `--` operátory.</span><span class="sxs-lookup"><span data-stu-id="9fd41-105">The value of a variable can be changed through assignment or through use of the `++` and `--` operators.</span></span>

<span data-ttu-id="9fd41-106">Proměnná musí být ***jednoznačně přiřazena*** ([jednoznačného přiřazení](variables.md#definite-assignment)) před jeho hodnotu lze získat.</span><span class="sxs-lookup"><span data-stu-id="9fd41-106">A variable must be ***definitely assigned*** ([Definite assignment](variables.md#definite-assignment)) before its value can be obtained.</span></span>

<span data-ttu-id="9fd41-107">Jak je popsáno v následujících částech, proměnné jsou buď ***původně přiřazené*** nebo ***zpočátku nepřiřazené***.</span><span class="sxs-lookup"><span data-stu-id="9fd41-107">As described in the following sections, variables are either ***initially assigned*** or ***initially unassigned***.</span></span> <span data-ttu-id="9fd41-108">Na začátku přiřazenou proměnnou nemá jasně definované počáteční hodnotu a je vždy považován za jednoznačně přiřazena.</span><span class="sxs-lookup"><span data-stu-id="9fd41-108">An initially assigned variable has a well-defined initial value and is always considered definitely assigned.</span></span> <span data-ttu-id="9fd41-109">Zpočátku Nepřiřazená proměnná nemá počáteční hodnotu.</span><span class="sxs-lookup"><span data-stu-id="9fd41-109">An initially unassigned variable has no initial value.</span></span> <span data-ttu-id="9fd41-110">Pro proměnnou zpočátku nepřiřazené považovat jednoznačně přiřazené do určitých umístění se musí objevit přiřazení k proměnné v každé možné spuštění cesta, která vede do tohoto umístění.</span><span class="sxs-lookup"><span data-stu-id="9fd41-110">For an initially unassigned variable to be considered definitely assigned at a certain location, an assignment to the variable must occur in every possible execution path leading to that location.</span></span>

## <a name="variable-categories"></a><span data-ttu-id="9fd41-111">Proměnné kategorie</span><span class="sxs-lookup"><span data-stu-id="9fd41-111">Variable categories</span></span>

<span data-ttu-id="9fd41-112">C# definuje proměnných sedm kategorií: statické proměnné, instance proměnné, prvky pole, parametry s hodnotou, parametry odkazů, výstupní parametry a lokální proměnné.</span><span class="sxs-lookup"><span data-stu-id="9fd41-112">C# defines seven categories of variables: static variables, instance variables, array elements, value parameters, reference parameters, output parameters, and local variables.</span></span> <span data-ttu-id="9fd41-113">Následující části popisují každou z těchto kategorií.</span><span class="sxs-lookup"><span data-stu-id="9fd41-113">The sections that follow describe each of these categories.</span></span>

<span data-ttu-id="9fd41-114">V příkladu</span><span class="sxs-lookup"><span data-stu-id="9fd41-114">In the example</span></span>
```csharp
class A
{
    public static int x;
    int y;

    void F(int[] v, int a, ref int b, out int c) {
        int i = 1;
        c = a + b++;
    }
}
```
<span data-ttu-id="9fd41-115">`x` je statická proměnná `y` je proměnná instance `v[0]` je k elementu pole `a` je parametr hodnota `b` je parametr odkaz `c` je výstupní parametr, a `i` místní proměnná.</span><span class="sxs-lookup"><span data-stu-id="9fd41-115">`x` is a static variable, `y` is an instance variable, `v[0]` is an array element, `a` is a value parameter, `b` is a reference parameter, `c` is an output parameter, and `i` is a local variable.</span></span>

### <a name="static-variables"></a><span data-ttu-id="9fd41-116">Statické proměnné</span><span class="sxs-lookup"><span data-stu-id="9fd41-116">Static variables</span></span>

<span data-ttu-id="9fd41-117">Pole deklarované s `static` modifikátor se volá ***statická proměnná***.</span><span class="sxs-lookup"><span data-stu-id="9fd41-117">A field declared with the `static` modifier is called a ***static variable***.</span></span> <span data-ttu-id="9fd41-118">Statická proměnná stává existence před spuštěním statického konstruktoru ([statické konstruktory](classes.md#static-constructors)) pro jeho nadřazeného typu a přestane existovat, pokud domény přidružené aplikace přestane existovat.</span><span class="sxs-lookup"><span data-stu-id="9fd41-118">A static variable comes into existence before execution of the static constructor ([Static constructors](classes.md#static-constructors)) for its containing type, and ceases to exist when the associated application domain ceases to exist.</span></span>

<span data-ttu-id="9fd41-119">Počáteční hodnota statická proměnná je výchozí hodnota ([výchozí hodnoty](variables.md#default-values)) typu proměnné.</span><span class="sxs-lookup"><span data-stu-id="9fd41-119">The initial value of a static variable is the default value ([Default values](variables.md#default-values)) of the variable's type.</span></span>

<span data-ttu-id="9fd41-120">Pro účely kontroly jednoznačného přiřazení statická proměnná je považován za původně přiřazený.</span><span class="sxs-lookup"><span data-stu-id="9fd41-120">For purposes of definite assignment checking, a static variable is considered initially assigned.</span></span>

### <a name="instance-variables"></a><span data-ttu-id="9fd41-121">Proměnné instance</span><span class="sxs-lookup"><span data-stu-id="9fd41-121">Instance variables</span></span>

<span data-ttu-id="9fd41-122">Pole deklarované bez `static` Modifikátor je volána ***proměnnou instance***.</span><span class="sxs-lookup"><span data-stu-id="9fd41-122">A field declared without the `static` modifier is called an ***instance variable***.</span></span>

#### <a name="instance-variables-in-classes"></a><span data-ttu-id="9fd41-123">Instance proměnné ve třídách</span><span class="sxs-lookup"><span data-stu-id="9fd41-123">Instance variables in classes</span></span>

<span data-ttu-id="9fd41-124">Proměnná instance třídy existence stává, když se vytvoří novou instanci této třídy a přestanou existovat, pokud neexistují žádné odkazy do této instance a po provedení instance – destruktor (pokud existuje).</span><span class="sxs-lookup"><span data-stu-id="9fd41-124">An instance variable of a class comes into existence when a new instance of that class is created, and ceases to exist when there are no references to that instance and the instance's destructor (if any) has executed.</span></span>

<span data-ttu-id="9fd41-125">Počáteční hodnota proměnné instance třídy, je výchozí hodnota ([výchozí hodnoty](variables.md#default-values)) typu proměnné.</span><span class="sxs-lookup"><span data-stu-id="9fd41-125">The initial value of an instance variable of a class is the default value ([Default values](variables.md#default-values)) of the variable's type.</span></span>

<span data-ttu-id="9fd41-126">Pro účely kontroly jednoznačného přiřazení proměnnou instance třídy se považuje za původně přiřazený.</span><span class="sxs-lookup"><span data-stu-id="9fd41-126">For the purpose of definite assignment checking, an instance variable of a class is considered initially assigned.</span></span>

#### <a name="instance-variables-in-structs"></a><span data-ttu-id="9fd41-127">Proměnné instance ve strukturách</span><span class="sxs-lookup"><span data-stu-id="9fd41-127">Instance variables in structs</span></span>

<span data-ttu-id="9fd41-128">Proměnná instance struktury má přesně stejnou dobu platnosti jako proměnnou struktury, ke kterému patří.</span><span class="sxs-lookup"><span data-stu-id="9fd41-128">An instance variable of a struct has exactly the same lifetime as the struct variable to which it belongs.</span></span> <span data-ttu-id="9fd41-129">Jinými slovy, při vstupu do existenci nebo přestane existovat, tak proměnnou typu Struktura příliš proveďte proměnné instance struktury.</span><span class="sxs-lookup"><span data-stu-id="9fd41-129">In other words, when a variable of a struct type comes into existence or ceases to exist, so too do the instance variables of the struct.</span></span>

<span data-ttu-id="9fd41-130">Počáteční přiřazení stavu instance proměnné struktury je stejné jako u obsahujícího proměnnou struktury.</span><span class="sxs-lookup"><span data-stu-id="9fd41-130">The initial assignment state of an instance variable of a struct is the same as that of the containing struct variable.</span></span> <span data-ttu-id="9fd41-131">Jinými slovy, když proměnné struktury je považován za původně přiřazené, tak příliš jsou své instance proměnné a při proměnné struktury je považován za zpočátku nepřiřazené, své instance proměnné jsou rovněž nepřiřazené.</span><span class="sxs-lookup"><span data-stu-id="9fd41-131">In other words, when a struct variable is considered initially assigned, so too are its instance variables, and when a struct variable is considered initially unassigned, its instance variables are likewise unassigned.</span></span>

### <a name="array-elements"></a><span data-ttu-id="9fd41-132">Prvky pole</span><span class="sxs-lookup"><span data-stu-id="9fd41-132">Array elements</span></span>

<span data-ttu-id="9fd41-133">Prvky pole existence začne, když je vytvořena instance pole a přestanou existovat, pokud neexistují žádné odkazy do této instance pole.</span><span class="sxs-lookup"><span data-stu-id="9fd41-133">The elements of an array come into existence when an array instance is created, and cease to exist when there are no references to that array instance.</span></span>

<span data-ttu-id="9fd41-134">Počáteční hodnotu všech prvků pole je výchozí hodnota ([výchozí hodnoty](variables.md#default-values)) typu prvků pole.</span><span class="sxs-lookup"><span data-stu-id="9fd41-134">The initial value of each of the elements of an array is the default value ([Default values](variables.md#default-values)) of the type of the array elements.</span></span>

<span data-ttu-id="9fd41-135">Pro účely kontroly jednoznačného přiřazení k elementu pole se považuje za původně přiřazený.</span><span class="sxs-lookup"><span data-stu-id="9fd41-135">For the purpose of definite assignment checking, an array element is considered initially assigned.</span></span>

### <a name="value-parameters"></a><span data-ttu-id="9fd41-136">Parametry s hodnotou</span><span class="sxs-lookup"><span data-stu-id="9fd41-136">Value parameters</span></span>

<span data-ttu-id="9fd41-137">Parametr deklarovaný bez `ref` nebo `out` modifikátor ***parametr hodnoty***.</span><span class="sxs-lookup"><span data-stu-id="9fd41-137">A parameter declared without a `ref` or `out` modifier is a ***value parameter***.</span></span>

<span data-ttu-id="9fd41-138">Hodnota parametru stává existence při volání členské funkce (metoda, konstruktor instance, přístupový objekt nebo operátor) nebo anonymní funkce, které parametr patří, a je inicializován s hodnotou argumentu zadaný ve volání.</span><span class="sxs-lookup"><span data-stu-id="9fd41-138">A value parameter comes into existence upon invocation of the function member (method, instance constructor, accessor, or operator) or anonymous function to which the parameter belongs, and is initialized with the value of the argument given in the invocation.</span></span> <span data-ttu-id="9fd41-139">Hodnota parametru obvykle přestane existovat po návratu funkce člena nebo anonymní funkce.</span><span class="sxs-lookup"><span data-stu-id="9fd41-139">A value parameter normally ceases to exist upon return of the function member or anonymous function.</span></span> <span data-ttu-id="9fd41-140">Nicméně pokud má parametr hodnoty zachycena anonymní funkce ([výrazy anonymní funkce](expressions.md#anonymous-function-expressions)), jeho životnosti rozšiřuje alespoň dokud delegát nebo strom výrazu, které jsou vytvořené z této anonymní funkce má nárok na uvolnění paměti.</span><span class="sxs-lookup"><span data-stu-id="9fd41-140">However, if the value parameter is captured by an anonymous function ([Anonymous function expressions](expressions.md#anonymous-function-expressions)), its life time extends at least until the delegate or expression tree created from that anonymous function is eligible for garbage collection.</span></span>

<span data-ttu-id="9fd41-141">Pro účely kontroly jednoznačného přiřazení parametr hodnoty se považuje za původně přiřazený.</span><span class="sxs-lookup"><span data-stu-id="9fd41-141">For the purpose of definite assignment checking, a value parameter is considered initially assigned.</span></span>

### <a name="reference-parameters"></a><span data-ttu-id="9fd41-142">Parametry odkazu</span><span class="sxs-lookup"><span data-stu-id="9fd41-142">Reference parameters</span></span>

<span data-ttu-id="9fd41-143">Parametr deklarovaný s `ref` modifikátor ***odkazovat na parametr***.</span><span class="sxs-lookup"><span data-stu-id="9fd41-143">A parameter declared with a `ref` modifier is a ***reference parameter***.</span></span>

<span data-ttu-id="9fd41-144">Parametr odkazu nevytváří nového umístění úložiště.</span><span class="sxs-lookup"><span data-stu-id="9fd41-144">A reference parameter does not create a new storage location.</span></span> <span data-ttu-id="9fd41-145">Místo toho referenční parametr představuje stejné úložiště jako proměnná, zadaný jako argument v členské funkci nebo volání anonymní funkce.</span><span class="sxs-lookup"><span data-stu-id="9fd41-145">Instead, a reference parameter represents the same storage location as the variable given as the argument in the function member or anonymous function invocation.</span></span> <span data-ttu-id="9fd41-146">Díky tomu se hodnota parametru odkazu je vždy stejný jako základní proměnné.</span><span class="sxs-lookup"><span data-stu-id="9fd41-146">Thus, the value of a reference parameter is always the same as the underlying variable.</span></span>

<span data-ttu-id="9fd41-147">Platí následující pravidla jednoznačného přiřazení k parametrům reference.</span><span class="sxs-lookup"><span data-stu-id="9fd41-147">The following definite assignment rules apply to reference parameters.</span></span> <span data-ttu-id="9fd41-148">Poznámka: jiná pravidla pro výstupní parametry, které jsou popsané v [výstupních parametrů](variables.md#output-parameters).</span><span class="sxs-lookup"><span data-stu-id="9fd41-148">Note the different rules for output parameters described in [Output parameters](variables.md#output-parameters).</span></span>

*  <span data-ttu-id="9fd41-149">Proměnná musí být jednoznačně přiřazena ([jednoznačného přiřazení](variables.md#definite-assignment)) předtím, než může být předán jako referenční parametr ve volání funkce člena nebo delegáta.</span><span class="sxs-lookup"><span data-stu-id="9fd41-149">A variable must be definitely assigned ([Definite assignment](variables.md#definite-assignment)) before it can be passed as a reference parameter in a function member or delegate invocation.</span></span>
*  <span data-ttu-id="9fd41-150">V rámci funkce člena nebo anonymní funkce je považován za původně přiřazené parametr odkazu.</span><span class="sxs-lookup"><span data-stu-id="9fd41-150">Within a function member or anonymous function, a reference parameter is considered initially assigned.</span></span>

<span data-ttu-id="9fd41-151">V rámci metodu instance nebo přístupový objekt instance typu Struktura `this` – klíčové slovo se chová stejně jako odkaz na parametr typu struktury ([tento přístup](expressions.md#this-access)).</span><span class="sxs-lookup"><span data-stu-id="9fd41-151">Within an instance method or instance accessor of a struct type, the `this` keyword behaves exactly as a reference parameter of the struct type ([This access](expressions.md#this-access)).</span></span>

### <a name="output-parameters"></a><span data-ttu-id="9fd41-152">Výstupní parametry</span><span class="sxs-lookup"><span data-stu-id="9fd41-152">Output parameters</span></span>

<span data-ttu-id="9fd41-153">Parametr deklarovaný pomocí `out` modifikátor ***výstupní parametr***.</span><span class="sxs-lookup"><span data-stu-id="9fd41-153">A parameter declared with an `out` modifier is an ***output parameter***.</span></span>

<span data-ttu-id="9fd41-154">Výstupní parametr nevytváří jinam úložiště.</span><span class="sxs-lookup"><span data-stu-id="9fd41-154">An output parameter does not create a new storage location.</span></span> <span data-ttu-id="9fd41-155">Místo toho výstupní parametr představuje stejné úložiště jako proměnná, zadaný jako argument ve volání funkce člena nebo delegáta.</span><span class="sxs-lookup"><span data-stu-id="9fd41-155">Instead, an output parameter represents the same storage location as the variable given as the argument in the function member or delegate invocation.</span></span> <span data-ttu-id="9fd41-156">Díky tomu se hodnota výstupního parametru je vždy stejný jako základní proměnné.</span><span class="sxs-lookup"><span data-stu-id="9fd41-156">Thus, the value of an output parameter is always the same as the underlying variable.</span></span>

<span data-ttu-id="9fd41-157">Platí následující pravidla jednoznačného přiřazení do výstupní parametry.</span><span class="sxs-lookup"><span data-stu-id="9fd41-157">The following definite assignment rules apply to output parameters.</span></span> <span data-ttu-id="9fd41-158">Všimněte si různých pravidel pro parametry odkazů je popsáno v [odkazovat na parametry](variables.md#reference-parameters).</span><span class="sxs-lookup"><span data-stu-id="9fd41-158">Note the different rules for reference parameters described in [Reference parameters](variables.md#reference-parameters).</span></span>

*  <span data-ttu-id="9fd41-159">Proměnná není jednoznačně přiřazena dříve, než může být předán jako výstupní parametr v členské funkci nebo vyvolání delegáta.</span><span class="sxs-lookup"><span data-stu-id="9fd41-159">A variable need not be definitely assigned before it can be passed as an output parameter in a function member or delegate invocation.</span></span>
*  <span data-ttu-id="9fd41-160">V důsledku normálního dokončení volání funkce člena nebo delegát každou proměnnou, která byla předána jako výstupní parametr je považován za přiřazené v této cestě spuštění.</span><span class="sxs-lookup"><span data-stu-id="9fd41-160">Following the normal completion of a function member or delegate invocation, each variable that was passed as an output parameter is considered assigned in that execution path.</span></span>
*  <span data-ttu-id="9fd41-161">V rámci funkce člena nebo anonymní funkce je považován za zpočátku nepřiřazené výstupní parametr.</span><span class="sxs-lookup"><span data-stu-id="9fd41-161">Within a function member or anonymous function, an output parameter is considered initially unassigned.</span></span>
*  <span data-ttu-id="9fd41-162">Každou výstupní parametr funkce člena nebo anonymní funkce musí být jednoznačně přiřazena ([jednoznačného přiřazení](variables.md#definite-assignment)) před funkce člena nebo anonymní funkce vrátí normálně.</span><span class="sxs-lookup"><span data-stu-id="9fd41-162">Every output parameter of a function member or anonymous function must be definitely assigned ([Definite assignment](variables.md#definite-assignment)) before the function member or anonymous function returns normally.</span></span>

<span data-ttu-id="9fd41-163">V rámci konstruktoru instance typu Struktura `this` – klíčové slovo se chová stejně jako výstupní parametr typu struktury ([tento přístup](expressions.md#this-access)).</span><span class="sxs-lookup"><span data-stu-id="9fd41-163">Within an instance constructor of a struct type, the `this` keyword behaves exactly as an output parameter of the struct type ([This access](expressions.md#this-access)).</span></span>

### <a name="local-variables"></a><span data-ttu-id="9fd41-164">Lokální proměnné</span><span class="sxs-lookup"><span data-stu-id="9fd41-164">Local variables</span></span>

<span data-ttu-id="9fd41-165">A ***lokální proměnná*** deklaroval *local_variable_declaration*, kterému může dojít v *bloku*, *for_statement*, *switch_statement* nebo *using_statement*; nebo *foreach_statement* nebo *specific_catch_clause* pro *try_statement*.</span><span class="sxs-lookup"><span data-stu-id="9fd41-165">A ***local variable*** is declared by a *local_variable_declaration*, which may occur in a *block*, a *for_statement*, a *switch_statement* or a *using_statement*; or by a *foreach_statement* or a *specific_catch_clause* for a *try_statement*.</span></span>

<span data-ttu-id="9fd41-166">Doba života lokální proměnná je část provádění programu, během které je zaručeno úložiště, které budou rezervovány pro něj.</span><span class="sxs-lookup"><span data-stu-id="9fd41-166">The lifetime of a local variable is the portion of program execution during which storage is guaranteed to be reserved for it.</span></span> <span data-ttu-id="9fd41-167">Tato doba života rozšiřuje nejméně ze vstupu *bloku*, *for_statement*, *switch_statement*, *using_statement*, *foreach_statement*, nebo *specific_catch_clause* s jakou je přidružený, dokud nebude spuštění, který *bloku*, *for_statement*, *switch_statement*, *using_statement*, *foreach_statement*, nebo *specific_catch_clause* končí žádným způsobem.</span><span class="sxs-lookup"><span data-stu-id="9fd41-167">This lifetime extends at least from entry into the *block*, *for_statement*, *switch_statement*, *using_statement*, *foreach_statement*, or *specific_catch_clause* with which it is associated, until execution of that *block*, *for_statement*, *switch_statement*, *using_statement*, *foreach_statement*, or *specific_catch_clause* ends in any way.</span></span> <span data-ttu-id="9fd41-168">(Zadání uzavřené *bloku* nebo volání metody pozastaví, ale nekončí, provádění aktuálního *bloku*, *for_statement*, *switch_statement* , *using_statement*, *foreach_statement*, nebo *specific_catch_clause*.) Pokud místní proměnná je zachycen anonymní funkce ([zachyceným proměnným vnější](expressions.md#captured-outer-variables)), dobu života rozšiřuje alespoň do stromu delegáta nebo výraz anonymní funkce, společně s další objekty, které jsou vytvořené zachycené proměnné odkazovat, jsou vhodné pro uvolnění paměti.</span><span class="sxs-lookup"><span data-stu-id="9fd41-168">(Entering an enclosed *block* or calling a method suspends, but does not end, execution of the current *block*, *for_statement*, *switch_statement*, *using_statement*, *foreach_statement*, or *specific_catch_clause*.) If the local variable is captured by an anonymous function ([Captured outer variables](expressions.md#captured-outer-variables)), its lifetime extends at least until the delegate or expression tree created from the anonymous function, along with any other objects that come to reference the captured variable, are eligible for garbage collection.</span></span>

<span data-ttu-id="9fd41-169">Pokud nadřazená *bloku*, *for_statement*, *switch_statement*, *using_statement*, *foreach_statement*, nebo *specific_catch_clause* zadání rekurzivně, je vytvořena nová instance lokální proměnné pokaždé, když a jeho *local_variable_initializer*, pokud existuje, je vyhodnocen pokaždé, když.</span><span class="sxs-lookup"><span data-stu-id="9fd41-169">If the parent *block*, *for_statement*, *switch_statement*, *using_statement*, *foreach_statement*, or *specific_catch_clause* is entered recursively, a new instance of the local variable is created each time, and its *local_variable_initializer*, if any, is evaluated each time.</span></span>

<span data-ttu-id="9fd41-170">Lokální proměnná zavedené *local_variable_declaration* nejsou inicializovány automaticky, a proto nemá žádnou výchozí hodnotu.</span><span class="sxs-lookup"><span data-stu-id="9fd41-170">A local variable introduced by a *local_variable_declaration* is not automatically initialized and thus has no default value.</span></span> <span data-ttu-id="9fd41-171">Pro účely kontroly jednoznačného přiřazení místní proměnné zavedené *local_variable_declaration* se považuje za zpočátku nepřiřazené.</span><span class="sxs-lookup"><span data-stu-id="9fd41-171">For the purpose of definite assignment checking, a local variable introduced by a *local_variable_declaration* is considered initially unassigned.</span></span> <span data-ttu-id="9fd41-172">A *local_variable_declaration* mohou zahrnovat *local_variable_initializer*, v takovém případě proměnné se považuje za jednoznačně přiřazená jenom po inicializační výraz ([ Příkazy deklarace](variables.md#declaration-statements)).</span><span class="sxs-lookup"><span data-stu-id="9fd41-172">A *local_variable_declaration* may include a *local_variable_initializer*, in which case the variable is considered definitely assigned only after the initializing expression ([Declaration statements](variables.md#declaration-statements)).</span></span>

<span data-ttu-id="9fd41-173">V rámci oboru zavedených v místní proměnné *local_variable_declaration*, je chyba kompilace k odkazování na tuto místní proměnnou v textové pozici, která předchází jeho *local_variable_declarator*.</span><span class="sxs-lookup"><span data-stu-id="9fd41-173">Within the scope of a local variable introduced by a *local_variable_declaration*, it is a compile-time error to refer to that local variable in a textual position that precedes its *local_variable_declarator*.</span></span> <span data-ttu-id="9fd41-174">Pokud deklarace lokální proměnné je implicitní ([místní deklarace proměnné](statements.md#local-variable-declarations)), je také chybou odkazovat na proměnné v rámci jeho *local_variable_declarator*.</span><span class="sxs-lookup"><span data-stu-id="9fd41-174">If the local variable declaration is implicit ([Local variable declarations](statements.md#local-variable-declarations)), it is also an error to refer to the variable within its *local_variable_declarator*.</span></span>

<span data-ttu-id="9fd41-175">Lokální proměnná zavedené *foreach_statement* nebo *specific_catch_clause* považován za jeho celý rozsah jednoznačně přiřazené.</span><span class="sxs-lookup"><span data-stu-id="9fd41-175">A local variable introduced by a *foreach_statement* or a *specific_catch_clause* is considered definitely assigned in its entire scope.</span></span>

<span data-ttu-id="9fd41-176">Skutečná doba života lokální proměnná je závislý na implementaci.</span><span class="sxs-lookup"><span data-stu-id="9fd41-176">The actual lifetime of a local variable is implementation-dependent.</span></span> <span data-ttu-id="9fd41-177">Například kompilátor může být staticky určit, že místní proměnné v bloku se používá jenom pro malá část tohoto bloku.</span><span class="sxs-lookup"><span data-stu-id="9fd41-177">For example, a compiler might statically determine that a local variable in a block is only used for a small portion of that block.</span></span> <span data-ttu-id="9fd41-178">Pomocí této analýzy, kompilátor může vygenerovat kód, který má za následek úložiště proměnné s kratší doby života než jeho obsahujícího bloku.</span><span class="sxs-lookup"><span data-stu-id="9fd41-178">Using this analysis, the compiler could generate code that results in the variable's storage having a shorter lifetime than its containing block.</span></span>

<span data-ttu-id="9fd41-179">Úložiště, na které odkazuje místní referenční proměnné je uvolněn nezávisle na životnost místní referenční proměnné ([Automatická správa paměti](basic-concepts.md#automatic-memory-management)).</span><span class="sxs-lookup"><span data-stu-id="9fd41-179">The storage referred to by a local reference variable is reclaimed independently of the lifetime of that local reference variable ([Automatic memory management](basic-concepts.md#automatic-memory-management)).</span></span>

## <a name="default-values"></a><span data-ttu-id="9fd41-180">Výchozí hodnoty</span><span class="sxs-lookup"><span data-stu-id="9fd41-180">Default values</span></span>

<span data-ttu-id="9fd41-181">Následující kategorie proměnné jsou automaticky inicializovány na výchozí hodnoty:</span><span class="sxs-lookup"><span data-stu-id="9fd41-181">The following categories of variables are automatically initialized to their default values:</span></span>

*  <span data-ttu-id="9fd41-182">Statické proměnné.</span><span class="sxs-lookup"><span data-stu-id="9fd41-182">Static variables.</span></span>
*  <span data-ttu-id="9fd41-183">Instance proměnné instance třídy.</span><span class="sxs-lookup"><span data-stu-id="9fd41-183">Instance variables of class instances.</span></span>
*  <span data-ttu-id="9fd41-184">Prvky pole.</span><span class="sxs-lookup"><span data-stu-id="9fd41-184">Array elements.</span></span>

<span data-ttu-id="9fd41-185">Výchozí hodnota proměnné a závisí na typu proměnné a je stanoven následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="9fd41-185">The default value of a variable depends on the type of the variable and is determined as follows:</span></span>

*  <span data-ttu-id="9fd41-186">Pro proměnnou *value_type*, výchozí hodnota je stejná jako hodnota vypočítaná aplikací *value_type*jeho výchozí konstruktor ([výchozí konstruktory](types.md#default-constructors)).</span><span class="sxs-lookup"><span data-stu-id="9fd41-186">For a variable of a *value_type*, the default value is the same as the value computed by the *value_type*'s default constructor ([Default constructors](types.md#default-constructors)).</span></span>
*  <span data-ttu-id="9fd41-187">Pro proměnnou *reference_type*, výchozí hodnota je `null`.</span><span class="sxs-lookup"><span data-stu-id="9fd41-187">For a variable of a *reference_type*, the default value is `null`.</span></span>

<span data-ttu-id="9fd41-188">Inicializace na výchozí hodnoty se obvykle provádí tak, že správce paměti nebo systému uvolňování paměti inicializovat paměť na hodnotu nula všechny bity, předtím, než je přidělená k použití.</span><span class="sxs-lookup"><span data-stu-id="9fd41-188">Initialization to default values is typically done by having the memory manager or garbage collector initialize memory to all-bits-zero before it is allocated for use.</span></span> <span data-ttu-id="9fd41-189">Z tohoto důvodu je vhodné použít všechny bity žádnou k reprezentaci nulový odkaz.</span><span class="sxs-lookup"><span data-stu-id="9fd41-189">For this reason, it is convenient to use all-bits-zero to represent the null reference.</span></span>

## <a name="definite-assignment"></a><span data-ttu-id="9fd41-190">Jednoznačného přiřazení</span><span class="sxs-lookup"><span data-stu-id="9fd41-190">Definite assignment</span></span>

<span data-ttu-id="9fd41-191">V daném místě v spustitelného kódu z členské funkce, proměnné se říká, že ***jednoznačně přiřazena*** Pokud kompilátor může být velmi, podle konkrétního toku statické analýzy ([přesné pravidla pro určování jednoznačného přiřazení](variables.md#precise-rules-for-determining-definite-assignment)), proměnná automaticky inicializován nebo má cíl alespoň jedno přiřazení.</span><span class="sxs-lookup"><span data-stu-id="9fd41-191">At a given location in the executable code of a function member, a variable is said to be ***definitely assigned*** if the compiler can prove, by a particular static flow analysis ([Precise rules for determining definite assignment](variables.md#precise-rules-for-determining-definite-assignment)), that the variable has been automatically initialized or has been the target of at least one assignment.</span></span> <span data-ttu-id="9fd41-192">Neformálně jsme uvedli, jsou pravidla jednoznačného přiřazení:</span><span class="sxs-lookup"><span data-stu-id="9fd41-192">Informally stated, the rules of definite assignment are:</span></span>

*  <span data-ttu-id="9fd41-193">Na začátku přiřazenou proměnnou ([původně přiřazený proměnné](variables.md#initially-assigned-variables)) je vždy považován za jednoznačně přiřazené.</span><span class="sxs-lookup"><span data-stu-id="9fd41-193">An initially assigned variable ([Initially assigned variables](variables.md#initially-assigned-variables)) is always considered definitely assigned.</span></span>
*  <span data-ttu-id="9fd41-194">Na začátku nepřiřazené proměnnou ([původně nepřiřazené proměnné](variables.md#initially-unassigned-variables)) se považuje za jednoznačně přiřazené do daného umístění. Pokud všemi možnými cestami spuštění vede do tohoto umístění obsahovat alespoň jeden z následujících akcí:</span><span class="sxs-lookup"><span data-stu-id="9fd41-194">An initially unassigned variable ([Initially unassigned variables](variables.md#initially-unassigned-variables)) is considered definitely assigned at a given location if all possible execution paths leading to that location contain at least one of the following:</span></span>
    * <span data-ttu-id="9fd41-195">Jednoduché přiřazení ([jednoduché přiřazení](expressions.md#simple-assignment)) v které proměnná je levý operand.</span><span class="sxs-lookup"><span data-stu-id="9fd41-195">A simple assignment ([Simple assignment](expressions.md#simple-assignment)) in which the variable is the left operand.</span></span>
    * <span data-ttu-id="9fd41-196">Výraz volání ([výrazy typu Invocation](expressions.md#invocation-expressions)) nebo výraz vytvoření objektu ([výrazy vytvoření objektu](expressions.md#object-creation-expressions)), která předá proměnná jako výstupní parametr.</span><span class="sxs-lookup"><span data-stu-id="9fd41-196">An invocation expression ([Invocation expressions](expressions.md#invocation-expressions)) or object creation expression ([Object creation expressions](expressions.md#object-creation-expressions)) that passes the variable as an output parameter.</span></span>
    * <span data-ttu-id="9fd41-197">Pro místní proměnnou, místní deklarace proměnné ([místní deklarace proměnné](statements.md#local-variable-declarations)), který obsahuje inicializátoru proměnné.</span><span class="sxs-lookup"><span data-stu-id="9fd41-197">For a local variable, a local variable declaration ([Local variable declarations](statements.md#local-variable-declarations)) that includes a variable initializer.</span></span>

<span data-ttu-id="9fd41-198">Formální specifikaci základní výše uvedených pravidel neformální je popsána v [původně přiřazený proměnné](variables.md#initially-assigned-variables), [původně nepřiřazené proměnné](variables.md#initially-unassigned-variables), a [přesné pravidla pro určování jednoznačného přiřazení](variables.md#precise-rules-for-determining-definite-assignment).</span><span class="sxs-lookup"><span data-stu-id="9fd41-198">The formal specification underlying the above informal rules is described in [Initially assigned variables](variables.md#initially-assigned-variables), [Initially unassigned variables](variables.md#initially-unassigned-variables), and [Precise rules for determining definite assignment](variables.md#precise-rules-for-determining-definite-assignment).</span></span>

<span data-ttu-id="9fd41-199">Stavy jednoznačného přiřazení instance proměnné *struct_type* proměnné jsou sledovány jako jednotlivě i hromadně.</span><span class="sxs-lookup"><span data-stu-id="9fd41-199">The definite assignment states of instance variables of a *struct_type* variable are tracked individually as well as collectively.</span></span> <span data-ttu-id="9fd41-200">V další pravidla výše uvedené, platí následující pravidla pro *struct_type* proměnné a jejich instance proměnné:</span><span class="sxs-lookup"><span data-stu-id="9fd41-200">In additional to the rules above, the following rules apply to *struct_type* variables and their instance variables:</span></span>

*  <span data-ttu-id="9fd41-201">Proměnná instance se považuje za jednoznačně přiřazené, pokud jeho obsahující *struct_type* proměnné se považuje za jednoznačně přiřazené.</span><span class="sxs-lookup"><span data-stu-id="9fd41-201">An instance variable is considered definitely assigned if its containing *struct_type* variable is considered definitely assigned.</span></span>
*  <span data-ttu-id="9fd41-202">A *struct_type* proměnné se považuje za jednoznačně přiřazené, pokud všechny její instance proměnné je považován za jednoznačně přiřazené.</span><span class="sxs-lookup"><span data-stu-id="9fd41-202">A *struct_type* variable is considered definitely assigned if each of its instance variables is considered definitely assigned.</span></span>

<span data-ttu-id="9fd41-203">Jednoznačného přiřazení je požadavek v následujících kontextů:</span><span class="sxs-lookup"><span data-stu-id="9fd41-203">Definite assignment is a requirement in the following contexts:</span></span>

*  <span data-ttu-id="9fd41-204">Proměnná musí být jednoznačně přiřazena v každém umístění, kde získat jeho hodnotu.</span><span class="sxs-lookup"><span data-stu-id="9fd41-204">A variable must be definitely assigned at each location where its value is obtained.</span></span> <span data-ttu-id="9fd41-205">Tím se zajistí, že nikdy nedojde nedefinované hodnoty.</span><span class="sxs-lookup"><span data-stu-id="9fd41-205">This ensures that undefined values never occur.</span></span> <span data-ttu-id="9fd41-206">Získat hodnotu proměnné, kromě případů, kdy považován za výskyt proměnné ve výrazu</span><span class="sxs-lookup"><span data-stu-id="9fd41-206">The occurrence of a variable in an expression is considered to obtain the value of the variable, except when</span></span>
    * <span data-ttu-id="9fd41-207">Proměnná je levý operand jednoduché přiřazení</span><span class="sxs-lookup"><span data-stu-id="9fd41-207">the variable is the left operand of a simple assignment,</span></span>
    * <span data-ttu-id="9fd41-208">Proměnná je předána jako výstupní parametr, nebo</span><span class="sxs-lookup"><span data-stu-id="9fd41-208">the variable is passed as an output parameter, or</span></span>
    * <span data-ttu-id="9fd41-209">Proměnná je *struct_type* proměnné a vyskytuje se jako levý operand přístup ke členu.</span><span class="sxs-lookup"><span data-stu-id="9fd41-209">the variable is a *struct_type* variable and occurs as the left operand of a member access.</span></span>
*  <span data-ttu-id="9fd41-210">Proměnná musí být jednoznačně přiřazena v každém umístění, kde je předán jako parametr odkazu.</span><span class="sxs-lookup"><span data-stu-id="9fd41-210">A variable must be definitely assigned at each location where it is passed as a reference parameter.</span></span> <span data-ttu-id="9fd41-211">Tím se zajistí, že můžete členské funkce volané zvažte referenční parametr původně přiřazený.</span><span class="sxs-lookup"><span data-stu-id="9fd41-211">This ensures that the function member being invoked can consider the reference parameter initially assigned.</span></span>
*  <span data-ttu-id="9fd41-212">Všechny výstupní parametry funkce člena musí být jednoznačně přiřazena v každém umístění, kde členské funkce vrátí (až `return` příkaz nebo prostřednictvím spuštění dojde na konec těla členské funkce).</span><span class="sxs-lookup"><span data-stu-id="9fd41-212">All output parameters of a function member must be definitely assigned at each location where the function member returns (through a `return` statement or through execution reaching the end of the function member body).</span></span> <span data-ttu-id="9fd41-213">Tím se zajistí, že členy funkce nevracejí nedefinované hodnoty ve výstupních parametrů, což umožní kompilátoru vzít v úvahu volání členské funkce, která přebírá proměnnou jako výstupní parametr ekvivalentní k přiřazení k proměnné.</span><span class="sxs-lookup"><span data-stu-id="9fd41-213">This ensures that function members do not return undefined values in output parameters, thus enabling the compiler to consider a function member invocation that takes a variable as an output parameter equivalent to an assignment to the variable.</span></span>
*  <span data-ttu-id="9fd41-214">`this` Proměnnou *struct_type* konstruktor instance musí být jednoznačně přiřazena v každém umístění, kde vrátí tento konstruktor instance.</span><span class="sxs-lookup"><span data-stu-id="9fd41-214">The `this` variable of a *struct_type* instance constructor must be definitely assigned at each location where that instance constructor returns.</span></span>

### <a name="initially-assigned-variables"></a><span data-ttu-id="9fd41-215">Zpočátku přiřazených proměnných</span><span class="sxs-lookup"><span data-stu-id="9fd41-215">Initially assigned variables</span></span>

<span data-ttu-id="9fd41-216">Následující kategorie proměnné jsou klasifikovány jako původně přiřazený:</span><span class="sxs-lookup"><span data-stu-id="9fd41-216">The following categories of variables are classified as initially assigned:</span></span>

*  <span data-ttu-id="9fd41-217">Statické proměnné.</span><span class="sxs-lookup"><span data-stu-id="9fd41-217">Static variables.</span></span>
*  <span data-ttu-id="9fd41-218">Instance proměnné instance třídy.</span><span class="sxs-lookup"><span data-stu-id="9fd41-218">Instance variables of class instances.</span></span>
*  <span data-ttu-id="9fd41-219">Instance proměnné původně přiřazené struktury proměnných.</span><span class="sxs-lookup"><span data-stu-id="9fd41-219">Instance variables of initially assigned struct variables.</span></span>
*  <span data-ttu-id="9fd41-220">Prvky pole.</span><span class="sxs-lookup"><span data-stu-id="9fd41-220">Array elements.</span></span>
*  <span data-ttu-id="9fd41-221">Parametry s hodnotou.</span><span class="sxs-lookup"><span data-stu-id="9fd41-221">Value parameters.</span></span>
*  <span data-ttu-id="9fd41-222">Odkaz na parametry.</span><span class="sxs-lookup"><span data-stu-id="9fd41-222">Reference parameters.</span></span>
*  <span data-ttu-id="9fd41-223">Proměnné deklarované v `catch` klauzule nebo `foreach` příkazu.</span><span class="sxs-lookup"><span data-stu-id="9fd41-223">Variables declared in a `catch` clause or a `foreach` statement.</span></span>

### <a name="initially-unassigned-variables"></a><span data-ttu-id="9fd41-224">Zpočátku nepřiřazené proměnné</span><span class="sxs-lookup"><span data-stu-id="9fd41-224">Initially unassigned variables</span></span>

<span data-ttu-id="9fd41-225">Následující kategorie proměnné jsou klasifikovány jako původně nepřiřazené:</span><span class="sxs-lookup"><span data-stu-id="9fd41-225">The following categories of variables are classified as initially unassigned:</span></span>

*  <span data-ttu-id="9fd41-226">Instance proměnné proměnných zpočátku nepřiřazenou strukturu.</span><span class="sxs-lookup"><span data-stu-id="9fd41-226">Instance variables of initially unassigned struct variables.</span></span>
*  <span data-ttu-id="9fd41-227">Výstupní parametry, včetně `this` proměnnou struktury konstruktory instancí.</span><span class="sxs-lookup"><span data-stu-id="9fd41-227">Output parameters, including the `this` variable of struct instance constructors.</span></span>
*  <span data-ttu-id="9fd41-228">Lokální proměnné, s výjimkou deklarované v `catch` klauzule nebo `foreach` příkazu.</span><span class="sxs-lookup"><span data-stu-id="9fd41-228">Local variables, except those declared in a `catch` clause or a `foreach` statement.</span></span>

### <a name="precise-rules-for-determining-definite-assignment"></a><span data-ttu-id="9fd41-229">Přesná pravidla pro určování jednoznačného přiřazení</span><span class="sxs-lookup"><span data-stu-id="9fd41-229">Precise rules for determining definite assignment</span></span>

<span data-ttu-id="9fd41-230">Aby bylo možné určit, že každou používá proměnnou je jednoznačně přiřazena, musíte použít kompilátor proces, který je ekvivalentní popsané v této části.</span><span class="sxs-lookup"><span data-stu-id="9fd41-230">In order to determine that each used variable is definitely assigned, the compiler must use a process that is equivalent to the one described in this section.</span></span>

<span data-ttu-id="9fd41-231">Kompilátor zpracovává text každého funkce člena, který má jednu nebo více zpočátku nepřiřazené proměnných.</span><span class="sxs-lookup"><span data-stu-id="9fd41-231">The compiler processes the body of each function member that has one or more initially unassigned variables.</span></span> <span data-ttu-id="9fd41-232">Pro každou proměnnou zpočátku nepřiřazené *v*, kompilátor Určuje ***jednoznačného přiřazení stavu*** pro *v* v každé z následujících bodů členské funkce:</span><span class="sxs-lookup"><span data-stu-id="9fd41-232">For each initially unassigned variable *v*, the compiler determines a ***definite assignment state*** for *v* at each of the following points in the function member:</span></span>

*  <span data-ttu-id="9fd41-233">Na začátku každého příkazu</span><span class="sxs-lookup"><span data-stu-id="9fd41-233">At the beginning of each statement</span></span>
*  <span data-ttu-id="9fd41-234">Na koncovém bodu ([koncové body a dostupnosti](statements.md#end-points-and-reachability)) každého příkazu</span><span class="sxs-lookup"><span data-stu-id="9fd41-234">At the end point ([End points and reachability](statements.md#end-points-and-reachability)) of each statement</span></span>
*  <span data-ttu-id="9fd41-235">Na každý oblouk který přenese ovládací prvek do jiného příkazu a koncový bod příkazu</span><span class="sxs-lookup"><span data-stu-id="9fd41-235">On each arc which transfers control to another statement or to the end point of a statement</span></span>
*  <span data-ttu-id="9fd41-236">Na začátku každého výrazu</span><span class="sxs-lookup"><span data-stu-id="9fd41-236">At the beginning of each expression</span></span>
*  <span data-ttu-id="9fd41-237">Na konci každého výrazu</span><span class="sxs-lookup"><span data-stu-id="9fd41-237">At the end of each expression</span></span>

<span data-ttu-id="9fd41-238">Stav jednoznačného přiřazení *v* může být buď:</span><span class="sxs-lookup"><span data-stu-id="9fd41-238">The definite assignment state of *v* can be either:</span></span>

*  <span data-ttu-id="9fd41-239">Jednoznačně přiřazena.</span><span class="sxs-lookup"><span data-stu-id="9fd41-239">Definitely assigned.</span></span> <span data-ttu-id="9fd41-240">To znamená, že na všechny toky možné ovládací prvek do této chvíle *v* byla přiřazena hodnota.</span><span class="sxs-lookup"><span data-stu-id="9fd41-240">This indicates that on all possible control flows to this point, *v* has been assigned a value.</span></span>
*  <span data-ttu-id="9fd41-241">Není jednoznačně přiřazena.</span><span class="sxs-lookup"><span data-stu-id="9fd41-241">Not definitely assigned.</span></span> <span data-ttu-id="9fd41-242">Pro stav proměnnou na konci výrazu typu `bool`, stav proměnná, která není jednoznačně přiřazena může (ale ne nutně) spadají do jedné z následujících stavů dílčí:</span><span class="sxs-lookup"><span data-stu-id="9fd41-242">For the state of a variable at the end of an expression of type `bool`, the state of a variable that isn't definitely assigned may (but doesn't necessarily) fall into one of the following sub-states:</span></span>
    * <span data-ttu-id="9fd41-243">Jednoznačně přiřazena po výraz hodnotu true.</span><span class="sxs-lookup"><span data-stu-id="9fd41-243">Definitely assigned after true expression.</span></span> <span data-ttu-id="9fd41-244">Tento stav znamená, že *v* je jednoznačně přiřazovat, je-li logický výraz vyhodnotí jako true, ale není přiřazen nutně pokud logický výraz vyhodnotí jako false.</span><span class="sxs-lookup"><span data-stu-id="9fd41-244">This state indicates that *v* is definitely assigned if the boolean expression evaluated as true, but is not necessarily assigned if the boolean expression evaluated as false.</span></span>
    * <span data-ttu-id="9fd41-245">Jednoznačně přiřazena po výraz hodnotu false.</span><span class="sxs-lookup"><span data-stu-id="9fd41-245">Definitely assigned after false expression.</span></span> <span data-ttu-id="9fd41-246">Tento stav znamená, že *v* je jednoznačně přiřazovat, je-li logický výraz vyhodnocen jako chybný, ale není přiřazen nutně pokud logický výraz vyhodnotí jako true.</span><span class="sxs-lookup"><span data-stu-id="9fd41-246">This state indicates that *v* is definitely assigned if the boolean expression evaluated as false, but is not necessarily assigned if the boolean expression evaluated as true.</span></span>

<span data-ttu-id="9fd41-247">Následující pravidla určují, jak stavu proměnné *v* je určen v každém umístění.</span><span class="sxs-lookup"><span data-stu-id="9fd41-247">The following rules govern how the state of a variable *v* is determined at each location.</span></span>

#### <a name="general-rules-for-statements"></a><span data-ttu-id="9fd41-248">Obecná pravidla pro příkazy</span><span class="sxs-lookup"><span data-stu-id="9fd41-248">General rules for statements</span></span>

*  <span data-ttu-id="9fd41-249">*v* není jednoznačně přiřazena na začátku těla členské funkce.</span><span class="sxs-lookup"><span data-stu-id="9fd41-249">*v* is not definitely assigned at the beginning of a function member body.</span></span>
*  <span data-ttu-id="9fd41-250">*v* je jednoznačně přiřazovat na začátku všechny nedostupné příkazu.</span><span class="sxs-lookup"><span data-stu-id="9fd41-250">*v* is definitely assigned at the beginning of any unreachable statement.</span></span>
*  <span data-ttu-id="9fd41-251">Stav jednoznačného přiřazení *v* na začátku další příkaz je určeno kontroluje se stav jednoznačného přiřazení *v* na všechny přenosy řízení toku, které se zaměřují na začátek příkaz.</span><span class="sxs-lookup"><span data-stu-id="9fd41-251">The definite assignment state of *v* at the beginning of any other statement is determined by checking the definite assignment state of *v* on all control flow transfers that target the beginning of that statement.</span></span> <span data-ttu-id="9fd41-252">Pokud (a pouze v případě) *v* je jednoznačně přiřazovat na všechny tyto přenosy tok řízení, se pak *v* je jednoznačně přiřazovat na začátku prohlášení.</span><span class="sxs-lookup"><span data-stu-id="9fd41-252">If (and only if) *v* is definitely assigned on all such control flow transfers, then *v* is definitely assigned at the beginning of the statement.</span></span> <span data-ttu-id="9fd41-253">Sadu možných řízení toku přenosů, je určena stejným způsobem jako u kontroly dostupnosti – příkaz ([koncové body a dostupnosti](statements.md#end-points-and-reachability)).</span><span class="sxs-lookup"><span data-stu-id="9fd41-253">The set of possible control flow transfers is determined in the same way as for checking statement reachability ([End points and reachability](statements.md#end-points-and-reachability)).</span></span>
*  <span data-ttu-id="9fd41-254">Stav jednoznačného přiřazení *v* na koncovém bodu bloku, `checked`, `unchecked`, `if`, `while`, `do`, `for`, `foreach`, `lock`, `using`, nebo `switch` se určí podle kontroluje se stav jednoznačného přiřazení *v* na všechny přenosy řízení toku, které se zaměřují koncový bod, který tento příkaz.</span><span class="sxs-lookup"><span data-stu-id="9fd41-254">The definite assignment state of *v* at the end point of a block, `checked`, `unchecked`, `if`, `while`, `do`, `for`, `foreach`, `lock`, `using`, or `switch` statement is determined by checking the definite assignment state of *v* on all control flow transfers that target the end point of that statement.</span></span> <span data-ttu-id="9fd41-255">Pokud *v* je jednoznačně přiřazovat na všechny tyto přenosy tok řízení, se pak *v* je jednoznačně přiřazovat na koncovém bodu příkazu.</span><span class="sxs-lookup"><span data-stu-id="9fd41-255">If *v* is definitely assigned on all such control flow transfers, then *v* is definitely assigned at the end point of the statement.</span></span> <span data-ttu-id="9fd41-256">Jinak. *v* není jednoznačně přiřazena na koncovém bodu příkazu.</span><span class="sxs-lookup"><span data-stu-id="9fd41-256">Otherwise; *v* is not definitely assigned at the end point of the statement.</span></span> <span data-ttu-id="9fd41-257">Sadu možných řízení toku přenosů, je určena stejným způsobem jako u kontroly dostupnosti – příkaz ([koncové body a dostupnosti](statements.md#end-points-and-reachability)).</span><span class="sxs-lookup"><span data-stu-id="9fd41-257">The set of possible control flow transfers is determined in the same way as for checking statement reachability ([End points and reachability](statements.md#end-points-and-reachability)).</span></span>

#### <a name="block-statements-checked-and-unchecked-statements"></a><span data-ttu-id="9fd41-258">Blok příkazů, zaškrtnutí a zaškrtnuté políčko příkazů</span><span class="sxs-lookup"><span data-stu-id="9fd41-258">Block statements, checked, and unchecked statements</span></span>

<span data-ttu-id="9fd41-259">Stav jednoznačného přiřazení *v* na ovládacím prvku přenos první příkaz seznamu příkazů v bloku (nebo koncový bod bloku, pokud je prázdný seznam příkazů) je stejný jako příkaz jednoznačného přiřazení *v* před blokem, `checked`, nebo `unchecked` příkazu.</span><span class="sxs-lookup"><span data-stu-id="9fd41-259">The definite assignment state of *v* on the control transfer to the first statement of the statement list in the block (or to the end point of the block, if the statement list is empty) is the same as the definite assignment statement of *v* before the block, `checked`, or `unchecked` statement.</span></span>

#### <a name="expression-statements"></a><span data-ttu-id="9fd41-260">Příkazy výrazů</span><span class="sxs-lookup"><span data-stu-id="9fd41-260">Expression statements</span></span>

<span data-ttu-id="9fd41-261">Pro příkaz výrazu *příkazu Insert* , který se skládá z výrazu *expr*:</span><span class="sxs-lookup"><span data-stu-id="9fd41-261">For an expression statement *stmt* that consists of the expression *expr*:</span></span>

*  <span data-ttu-id="9fd41-262">*v* má stejné jednoznačného přiřazení stavu na začátku *expr* stejně jako na začátku *příkazu Insert*.</span><span class="sxs-lookup"><span data-stu-id="9fd41-262">*v* has the same definite assignment state at the beginning of *expr* as at the beginning of *stmt*.</span></span>
*  <span data-ttu-id="9fd41-263">Pokud *v* pokud jednoznačně přiřazena na konci *expr*, je jednoznačně přiřazena na koncový bod *příkazu Insert*; jinak vrátí hodnotu; není jednoznačně přiřazena na koncový bod *příkazu Insert*.</span><span class="sxs-lookup"><span data-stu-id="9fd41-263">If *v* if definitely assigned at the end of *expr*, it is definitely assigned at the end point of *stmt*; otherwise; it is not definitely assigned at the end point of *stmt*.</span></span>

#### <a name="declaration-statements"></a><span data-ttu-id="9fd41-264">Příkazy deklarace</span><span class="sxs-lookup"><span data-stu-id="9fd41-264">Declaration statements</span></span>

*  <span data-ttu-id="9fd41-265">Pokud *příkazu Insert* příkazu deklarace bez inicializátorů, pak je *v* má stejný stav jednoznačného přiřazení na koncový bod *příkazu Insert* stejně jako na začátku *příkazu Insert*.</span><span class="sxs-lookup"><span data-stu-id="9fd41-265">If *stmt* is a declaration statement without initializers, then *v* has the same definite assignment state at the end point of *stmt* as at the beginning of *stmt*.</span></span>
*  <span data-ttu-id="9fd41-266">Pokud *příkazu Insert* příkazu deklarace s inicializátory, pak je stav jednoznačného přiřazení *v* je určen jako *příkazu Insert* byly seznam příkazů, pomocí jedné úlohy příkaz pro každou deklaraci s inicializátorem (v pořadí deklarace).</span><span class="sxs-lookup"><span data-stu-id="9fd41-266">If *stmt* is a declaration statement with initializers, then the definite assignment state for *v* is determined as if *stmt* were a statement list, with one assignment statement for each declaration with an initializer (in the order of declaration).</span></span>

#### <a name="if-statements"></a><span data-ttu-id="9fd41-267">Pokud se příkazy</span><span class="sxs-lookup"><span data-stu-id="9fd41-267">If statements</span></span>

<span data-ttu-id="9fd41-268">Pro `if` příkaz *příkazu Insert* formuláře:</span><span class="sxs-lookup"><span data-stu-id="9fd41-268">For an `if` statement *stmt* of the form:</span></span>
```csharp
if ( expr ) then_stmt else else_stmt
```

*  <span data-ttu-id="9fd41-269">*v* má stejné jednoznačného přiřazení stavu na začátku *expr* stejně jako na začátku *příkazu Insert*.</span><span class="sxs-lookup"><span data-stu-id="9fd41-269">*v* has the same definite assignment state at the beginning of *expr* as at the beginning of *stmt*.</span></span>
*  <span data-ttu-id="9fd41-270">Pokud *v* je jednoznačně přiřazovat na konci *expr*, pak je jednoznačně přiřazena na přenos řízení toku do *then_stmt* a buď *else_stmt*  nebo koncový bod z *příkazu Insert* Pokud neexistuje žádná klauzule else.</span><span class="sxs-lookup"><span data-stu-id="9fd41-270">If *v* is definitely assigned at the end of *expr*, then it is definitely assigned on the control flow transfer to *then_stmt* and to either *else_stmt* or to the end-point of *stmt* if there is no else clause.</span></span>
*  <span data-ttu-id="9fd41-271">Pokud *v* má stav "jednoznačně přiřazena po hodnotu true, výraz" na konci *expr*, pak je jednoznačně přiřazena na přenos řízení toku do *then_stmt*a ne určitě přiřazené na toku přenosu ovládacího prvku buď *else_stmt* nebo koncový bod z *příkazu Insert* Pokud neexistuje žádná klauzule else.</span><span class="sxs-lookup"><span data-stu-id="9fd41-271">If *v* has the state "definitely assigned after true expression" at the end of *expr*, then it is definitely assigned on the control flow transfer to *then_stmt*, and not definitely assigned on the control flow transfer to either *else_stmt* or to the end-point of *stmt* if there is no else clause.</span></span>
*  <span data-ttu-id="9fd41-272">Pokud *v* má stav "jednoznačně přiřazena po false výraz" na konci *expr*, pak je jednoznačně přiřazena na přenos řízení toku do *else_stmt*a ne na přenos řízení toku do jednoznačně přiřazena *then_stmt*.</span><span class="sxs-lookup"><span data-stu-id="9fd41-272">If *v* has the state "definitely assigned after false expression" at the end of *expr*, then it is definitely assigned on the control flow transfer to *else_stmt*, and not definitely assigned on the control flow transfer to *then_stmt*.</span></span> <span data-ttu-id="9fd41-273">Je jednoznačně přiřazena na koncový bod z *příkazu Insert* pouze v případě je jednoznačně přiřazena na koncový bod z *then_stmt*.</span><span class="sxs-lookup"><span data-stu-id="9fd41-273">It is definitely assigned at the end-point of *stmt* if and only if it is definitely assigned at the end-point of *then_stmt*.</span></span>
*  <span data-ttu-id="9fd41-274">V opačném případě *v* je považováno za nevyhovující jednoznačně přiřazené na toku přenosu ovládacího prvku buď *then_stmt* nebo *else_stmt*, nebo koncový bod z  *příkazu Insert* Pokud neexistuje žádná klauzule else.</span><span class="sxs-lookup"><span data-stu-id="9fd41-274">Otherwise, *v* is considered not definitely assigned on the control flow transfer to either the *then_stmt* or *else_stmt*, or to the end-point of *stmt* if there is no else clause.</span></span>

#### <a name="switch-statements"></a><span data-ttu-id="9fd41-275">Příkazy Switch</span><span class="sxs-lookup"><span data-stu-id="9fd41-275">Switch statements</span></span>

<span data-ttu-id="9fd41-276">V `switch` příkaz *příkazu Insert* s řídicí výraz *expr*:</span><span class="sxs-lookup"><span data-stu-id="9fd41-276">In a `switch` statement *stmt* with a controlling expression *expr*:</span></span>

*  <span data-ttu-id="9fd41-277">Stav jednoznačného přiřazení *v* na začátku *expr* je stejné jako stav *v* na začátku *příkazu Insert*.</span><span class="sxs-lookup"><span data-stu-id="9fd41-277">The definite assignment state of *v* at the beginning of *expr* is the same as the state of *v* at the beginning of *stmt*.</span></span>
*  <span data-ttu-id="9fd41-278">Stav jednoznačného přiřazení *v* v toku řízení přenosu do seznamu dostupný přepínač bloku příkazu je stejné jako stav jednoznačného přiřazení *v* na konci *expr*.</span><span class="sxs-lookup"><span data-stu-id="9fd41-278">The definite assignment state of *v* on the control flow transfer to a reachable switch block statement list is the same as the definite assignment state of *v* at the end of *expr*.</span></span>

#### <a name="while-statements"></a><span data-ttu-id="9fd41-279">While – příkazy</span><span class="sxs-lookup"><span data-stu-id="9fd41-279">While statements</span></span>

<span data-ttu-id="9fd41-280">Pro `while` příkaz *příkazu Insert* formuláře:</span><span class="sxs-lookup"><span data-stu-id="9fd41-280">For a `while` statement *stmt* of the form:</span></span>
```csharp
while ( expr ) while_body
```

*  <span data-ttu-id="9fd41-281">*v* má stejné jednoznačného přiřazení stavu na začátku *expr* stejně jako na začátku *příkazu Insert*.</span><span class="sxs-lookup"><span data-stu-id="9fd41-281">*v* has the same definite assignment state at the beginning of *expr* as at the beginning of *stmt*.</span></span>
*  <span data-ttu-id="9fd41-282">Pokud *v* je jednoznačně přiřazovat na konci *expr*, pak je jednoznačně přiřazena na přenos řízení toku do *while_body* a koncový bod  *příkazu Insert*.</span><span class="sxs-lookup"><span data-stu-id="9fd41-282">If *v* is definitely assigned at the end of *expr*, then it is definitely assigned on the control flow transfer to *while_body* and to the end point of *stmt*.</span></span>
*  <span data-ttu-id="9fd41-283">Pokud *v* má stav "jednoznačně přiřazena po hodnotu true, výraz" na konci *expr*, pak je jednoznačně přiřazena na přenos řízení toku do *while_body*, ale ne jednoznačně přiřazena na koncový bod z *příkazu Insert*.</span><span class="sxs-lookup"><span data-stu-id="9fd41-283">If *v* has the state "definitely assigned after true expression" at the end of *expr*, then it is definitely assigned on the control flow transfer to *while_body*, but not definitely assigned at the end-point of *stmt*.</span></span>
*  <span data-ttu-id="9fd41-284">Pokud *v* má stav "jednoznačně přiřazena po false výraz" na konci *expr*, pak je jednoznačně přiřazena na toku přenosu ovládacího prvku do koncového bodu *příkazu Insert* , ale není přiřazen jednoznačně na přenos řízení toku do *while_body*.</span><span class="sxs-lookup"><span data-stu-id="9fd41-284">If *v* has the state "definitely assigned after false expression" at the end of *expr*, then it is definitely assigned on the control flow transfer to the end point of *stmt*, but not definitely assigned on the control flow transfer to *while_body*.</span></span>

#### <a name="do-statements"></a><span data-ttu-id="9fd41-285">Proveďte příkazy</span><span class="sxs-lookup"><span data-stu-id="9fd41-285">Do statements</span></span>

<span data-ttu-id="9fd41-286">Pro `do` příkaz *příkazu Insert* formuláře:</span><span class="sxs-lookup"><span data-stu-id="9fd41-286">For a `do` statement *stmt* of the form:</span></span>
```csharp
do do_body while ( expr ) ;
```

*  <span data-ttu-id="9fd41-287">*v* má stejné jednoznačného přiřazení stavu v přenos řízení toku od začátku *příkazu Insert* k *do_body* stejně jako na začátku *příkazu Insert*.</span><span class="sxs-lookup"><span data-stu-id="9fd41-287">*v* has the same definite assignment state on the control flow transfer from the beginning of *stmt* to *do_body* as at the beginning of *stmt*.</span></span>
*  <span data-ttu-id="9fd41-288">*v* má stejné jednoznačného přiřazení stavu na začátku *expr* za koncový bod *do_body*.</span><span class="sxs-lookup"><span data-stu-id="9fd41-288">*v* has the same definite assignment state at the beginning of *expr* as at the end point of *do_body*.</span></span>
*  <span data-ttu-id="9fd41-289">Pokud *v* je jednoznačně přiřazovat na konci *expr*, pak je jednoznačně přiřazena na toku přenosu ovládacího prvku do koncového bodu *příkazu Insert*.</span><span class="sxs-lookup"><span data-stu-id="9fd41-289">If *v* is definitely assigned at the end of *expr*, then it is definitely assigned on the control flow transfer to the end point of *stmt*.</span></span>
*  <span data-ttu-id="9fd41-290">Pokud *v* má stav "jednoznačně přiřazena po false výraz" na konci *expr*, pak je jednoznačně přiřazena na toku přenosu ovládacího prvku do koncového bodu *příkazu Insert* .</span><span class="sxs-lookup"><span data-stu-id="9fd41-290">If *v* has the state "definitely assigned after false expression" at the end of *expr*, then it is definitely assigned on the control flow transfer to the end point of *stmt*.</span></span>

#### <a name="for-statements"></a><span data-ttu-id="9fd41-291">Pro příkazy</span><span class="sxs-lookup"><span data-stu-id="9fd41-291">For statements</span></span>

<span data-ttu-id="9fd41-292">Kontrola jednoznačného přiřazení `for` příkazu ve formátu:</span><span class="sxs-lookup"><span data-stu-id="9fd41-292">Definite assignment checking for a `for` statement of the form:</span></span>
```csharp
for ( for_initializer ; for_condition ; for_iterator ) embedded_statement
```
<span data-ttu-id="9fd41-293">Probíhá, jako kdyby byly napsány příkaz:</span><span class="sxs-lookup"><span data-stu-id="9fd41-293">is done as if the statement were written:</span></span>
```csharp
{
    for_initializer ;
    while ( for_condition ) {
        embedded_statement ;
        for_iterator ;
    }
}
```

<span data-ttu-id="9fd41-294">Pokud *for_condition* je vynecháno z `for` příkaz a pak vyhodnocování pokračuje jednoznačného přiřazení jakoby *for_condition* byla nahrazena `true` ve výše uvedené rozšíření .</span><span class="sxs-lookup"><span data-stu-id="9fd41-294">If the *for_condition* is omitted from the `for` statement, then evaluation of definite assignment proceeds as if *for_condition* were replaced with `true` in the above expansion.</span></span>

#### <a name="break-continue-and-goto-statements"></a><span data-ttu-id="9fd41-295">Přerušit, pokračovat a příkazy goto</span><span class="sxs-lookup"><span data-stu-id="9fd41-295">Break, continue, and goto statements</span></span>

<span data-ttu-id="9fd41-296">Stav jednoznačného přiřazení *v* na přenos řízení toku způsobené `break`, `continue`, nebo `goto` příkaz je stejné jako stav jednoznačného přiřazení *v* na začátek příkazu.</span><span class="sxs-lookup"><span data-stu-id="9fd41-296">The definite assignment state of *v* on the control flow transfer caused by a `break`, `continue`, or `goto` statement is the same as the definite assignment state of *v* at the beginning of the statement.</span></span>

#### <a name="throw-statements"></a><span data-ttu-id="9fd41-297">Throw – příkazy</span><span class="sxs-lookup"><span data-stu-id="9fd41-297">Throw statements</span></span>

<span data-ttu-id="9fd41-298">Pro příkaz *příkazu Insert* formuláře</span><span class="sxs-lookup"><span data-stu-id="9fd41-298">For a statement *stmt* of the form</span></span>
```csharp
throw expr ;
```

<span data-ttu-id="9fd41-299">Stav jednoznačného přiřazení *v* na začátku *expr* je stejné jako stav jednoznačného přiřazení *v* na začátku *příkazu Insert*.</span><span class="sxs-lookup"><span data-stu-id="9fd41-299">The definite assignment state of *v* at the beginning of *expr* is the same as the definite assignment state of *v* at the beginning of *stmt*.</span></span>

#### <a name="return-statements"></a><span data-ttu-id="9fd41-300">Příkazy Return</span><span class="sxs-lookup"><span data-stu-id="9fd41-300">Return statements</span></span>

<span data-ttu-id="9fd41-301">Pro příkaz *příkazu Insert* formuláře</span><span class="sxs-lookup"><span data-stu-id="9fd41-301">For a statement *stmt* of the form</span></span>
```csharp
return expr ;
```

*  <span data-ttu-id="9fd41-302">Stav jednoznačného přiřazení *v* na začátku *expr* je stejné jako stav jednoznačného přiřazení *v* na začátku *příkazu Insert*.</span><span class="sxs-lookup"><span data-stu-id="9fd41-302">The definite assignment state of *v* at the beginning of *expr* is the same as the definite assignment state of *v* at the beginning of *stmt*.</span></span>
*  <span data-ttu-id="9fd41-303">Pokud *v* je výstupní parametr, pak ho musí být jednoznačně přiřazena buď:</span><span class="sxs-lookup"><span data-stu-id="9fd41-303">If *v* is an output parameter, then it must be definitely assigned either:</span></span>
    * <span data-ttu-id="9fd41-304">Po *výraz*</span><span class="sxs-lookup"><span data-stu-id="9fd41-304">after *expr*</span></span>
    * <span data-ttu-id="9fd41-305">nebo na konci `finally` bloku `try` - `finally` nebo `try` - `catch` - `finally` , který obklopuje `return` příkazu.</span><span class="sxs-lookup"><span data-stu-id="9fd41-305">or at the end of the `finally` block of a `try`-`finally` or `try`-`catch`-`finally` that encloses the `return` statement.</span></span>

<span data-ttu-id="9fd41-306">Pro příkaz příkazu INSERT formuláře:</span><span class="sxs-lookup"><span data-stu-id="9fd41-306">For a statement stmt of the form:</span></span>
```csharp
return ;
```

*  <span data-ttu-id="9fd41-307">Pokud *v* je výstupní parametr, pak ho musí být jednoznačně přiřazena buď:</span><span class="sxs-lookup"><span data-stu-id="9fd41-307">If *v* is an output parameter, then it must be definitely assigned either:</span></span>
    * <span data-ttu-id="9fd41-308">před *příkazu INSERT*</span><span class="sxs-lookup"><span data-stu-id="9fd41-308">before *stmt*</span></span>
    * <span data-ttu-id="9fd41-309">nebo na konci `finally` bloku `try` - `finally` nebo `try` - `catch` - `finally` , který obklopuje `return` příkazu.</span><span class="sxs-lookup"><span data-stu-id="9fd41-309">or at the end of the `finally` block of a `try`-`finally` or `try`-`catch`-`finally` that encloses the `return` statement.</span></span>

#### <a name="try-catch-statements"></a><span data-ttu-id="9fd41-310">Try-catch – příkazy</span><span class="sxs-lookup"><span data-stu-id="9fd41-310">Try-catch statements</span></span>

<span data-ttu-id="9fd41-311">Pro příkaz *příkazu Insert* formuláře:</span><span class="sxs-lookup"><span data-stu-id="9fd41-311">For a statement *stmt* of the form:</span></span>
```csharp
try try_block
catch(...) catch_block_1
...
catch(...) catch_block_n
```

*  <span data-ttu-id="9fd41-312">Stav jednoznačného přiřazení *v* na začátku *try_block* je stejné jako stav jednoznačného přiřazení *v* na začátku *příkazu Insert*.</span><span class="sxs-lookup"><span data-stu-id="9fd41-312">The definite assignment state of *v* at the beginning of *try_block* is the same as the definite assignment state of *v* at the beginning of *stmt*.</span></span>
*  <span data-ttu-id="9fd41-313">Stav jednoznačného přiřazení *v* na začátku *catch_block_i* (pro všechny *můžu*) je stejný jako stav jednoznačného přiřazení *v*na začátku *příkazu Insert*.</span><span class="sxs-lookup"><span data-stu-id="9fd41-313">The definite assignment state of *v* at the beginning of *catch_block_i* (for any *i*) is the same as the definite assignment state of *v* at the beginning of *stmt*.</span></span>
*  <span data-ttu-id="9fd41-314">Stav jednoznačného přiřazení *v* na koncový bod z *příkazu Insert* je jednoznačně přiřazené if (a pouze v případě) *v* je jednoznačně přiřazovat na koncový bod z  *try_block* a každý *catch_block_i* (pro každý *můžu* od 1 do *n*).</span><span class="sxs-lookup"><span data-stu-id="9fd41-314">The definite assignment state of *v* at the end-point of *stmt* is definitely assigned if (and only if) *v* is definitely assigned at the end-point of *try_block* and every *catch_block_i* (for every *i* from 1 to *n*).</span></span>

#### <a name="try-finally-statements"></a><span data-ttu-id="9fd41-315">Try-finally – příkazy</span><span class="sxs-lookup"><span data-stu-id="9fd41-315">Try-finally statements</span></span>

<span data-ttu-id="9fd41-316">Pro `try` příkaz *příkazu Insert* formuláře:</span><span class="sxs-lookup"><span data-stu-id="9fd41-316">For a `try` statement *stmt* of the form:</span></span>
```csharp
try try_block finally finally_block
```

*  <span data-ttu-id="9fd41-317">Stav jednoznačného přiřazení *v* na začátku *try_block* je stejné jako stav jednoznačného přiřazení *v* na začátku *příkazu Insert*.</span><span class="sxs-lookup"><span data-stu-id="9fd41-317">The definite assignment state of *v* at the beginning of *try_block* is the same as the definite assignment state of *v* at the beginning of *stmt*.</span></span>
*  <span data-ttu-id="9fd41-318">Stav jednoznačného přiřazení *v* na začátku *finally_block* je stejné jako stav jednoznačného přiřazení *v* na začátku *příkazu INSERT* .</span><span class="sxs-lookup"><span data-stu-id="9fd41-318">The definite assignment state of *v* at the beginning of *finally_block* is the same as the definite assignment state of *v* at the beginning of *stmt*.</span></span>
*  <span data-ttu-id="9fd41-319">Stav jednoznačného přiřazení *v* na koncový bod z *příkazu Insert* je jednoznačně přiřazené if (a pouze v případě) platí alespoň jedna z následujících akcí:</span><span class="sxs-lookup"><span data-stu-id="9fd41-319">The definite assignment state of *v* at the end-point of *stmt* is definitely assigned if (and only if) at least one of the following is true:</span></span>
    * <span data-ttu-id="9fd41-320">*v* je jednoznačně přiřazovat na koncový bod z *try_block*</span><span class="sxs-lookup"><span data-stu-id="9fd41-320">*v* is definitely assigned at the end-point of *try_block*</span></span>
    * <span data-ttu-id="9fd41-321">*v* je jednoznačně přiřazovat na koncový bod z *finally_block*</span><span class="sxs-lookup"><span data-stu-id="9fd41-321">*v* is definitely assigned at the end-point of *finally_block*</span></span>

<span data-ttu-id="9fd41-322">Pokud přenos toku řízení (například `goto` příkaz), která začne v rámci *try_block*a končí mimo *try_block*, pak *v* je také považován za jednoznačně přiřazené na přenos řízení toku v případě *v* je jednoznačně přiřazovat na koncový bod z *finally_block*.</span><span class="sxs-lookup"><span data-stu-id="9fd41-322">If a control flow transfer (for example, a `goto` statement) is made that begins within *try_block*, and ends outside of *try_block*, then *v* is also considered definitely assigned on that control flow transfer if *v* is definitely assigned at the end-point of *finally_block*.</span></span> <span data-ttu-id="9fd41-323">(Toto není právě tehdy, pokud *v* je jednoznačně přiřazovat z nějakého jiného důvodu na tento převod řízení toku, a přesto považuje jednoznačně přiřazené.)</span><span class="sxs-lookup"><span data-stu-id="9fd41-323">(This is not an only if—if *v* is definitely assigned for another reason on this control flow transfer, then it is still considered definitely assigned.)</span></span>

#### <a name="try-catch-finally-statements"></a><span data-ttu-id="9fd41-324">Konstrukce try-catch – finally – příkazy</span><span class="sxs-lookup"><span data-stu-id="9fd41-324">Try-catch-finally statements</span></span>

<span data-ttu-id="9fd41-325">Analýza jednoznačného přiřazení `try` - `catch` - `finally` příkazu ve formátu:</span><span class="sxs-lookup"><span data-stu-id="9fd41-325">Definite assignment analysis for a `try`-`catch`-`finally` statement of the form:</span></span>
```csharp
try try_block
catch(...) catch_block_1
...
catch(...) catch_block_n
finally *finally_block*
```
<span data-ttu-id="9fd41-326">se provádí, jako kdyby byly příkaz `try` - `finally` uzavírající příkazu `try` - `catch` – příkaz:</span><span class="sxs-lookup"><span data-stu-id="9fd41-326">is done as if the statement were a `try`-`finally` statement enclosing a `try`-`catch` statement:</span></span>
```csharp
try {
    try try_block
    catch(...) catch_block_1
    ...
    catch(...) catch_block_n
}
finally finally_block
```

<span data-ttu-id="9fd41-327">Následující příklad ukazuje, jak různé bloky `try` – příkaz ([příkazu try](statements.md#the-try-statement)) ovlivnit jednoznačného přiřazení.</span><span class="sxs-lookup"><span data-stu-id="9fd41-327">The following example demonstrates how the different blocks of a `try` statement ([The try statement](statements.md#the-try-statement)) affect definite assignment.</span></span>
```csharp
class A
{
    static void F() {
        int i, j;
        try {
            goto LABEL;
            // neither i nor j definitely assigned
            i = 1;
            // i definitely assigned
        }

        catch {
            // neither i nor j definitely assigned
            i = 3;
            // i definitely assigned
        }

        finally {
            // neither i nor j definitely assigned
            j = 5;
            // j definitely assigned
            }
        // i and j definitely assigned
        LABEL:;
        // j definitely assigned

    }
}
```

#### <a name="foreach-statements"></a><span data-ttu-id="9fd41-328">Příkazy foreach</span><span class="sxs-lookup"><span data-stu-id="9fd41-328">Foreach statements</span></span>

<span data-ttu-id="9fd41-329">Pro `foreach` příkaz *příkazu Insert* formuláře:</span><span class="sxs-lookup"><span data-stu-id="9fd41-329">For a `foreach` statement *stmt* of the form:</span></span>
```csharp
foreach ( type identifier in expr ) embedded_statement
```

*  <span data-ttu-id="9fd41-330">Stav jednoznačného přiřazení *v* na začátku *expr* je stejné jako stav *v* na začátku *příkazu Insert*.</span><span class="sxs-lookup"><span data-stu-id="9fd41-330">The definite assignment state of *v* at the beginning of *expr* is the same as the state of *v* at the beginning of *stmt*.</span></span>
*  <span data-ttu-id="9fd41-331">Stav jednoznačného přiřazení *v* na přenos řízení toku do *embedded_statement* nebo koncový bod *příkazu Insert* je stejné jako stav *v* na konci *expr*.</span><span class="sxs-lookup"><span data-stu-id="9fd41-331">The definite assignment state of *v* on the control flow transfer to *embedded_statement* or to the end point of *stmt* is the same as the state of *v* at the end of *expr*.</span></span>

#### <a name="using-statements"></a><span data-ttu-id="9fd41-332">Pomocí příkazů</span><span class="sxs-lookup"><span data-stu-id="9fd41-332">Using statements</span></span>

<span data-ttu-id="9fd41-333">Pro `using` příkaz *příkazu Insert* formuláře:</span><span class="sxs-lookup"><span data-stu-id="9fd41-333">For a `using` statement *stmt* of the form:</span></span>
```csharp
using ( resource_acquisition ) embedded_statement
```

*  <span data-ttu-id="9fd41-334">Stav jednoznačného přiřazení *v* na začátku *resource_acquisition* je stejné jako stav *v* na začátku *příkazu Insert*.</span><span class="sxs-lookup"><span data-stu-id="9fd41-334">The definite assignment state of *v* at the beginning of *resource_acquisition* is the same as the state of *v* at the beginning of *stmt*.</span></span>
*  <span data-ttu-id="9fd41-335">Stav jednoznačného přiřazení *v* na přenos řízení toku do *embedded_statement* je stejné jako stav *v* na konci *resource_ získání*.</span><span class="sxs-lookup"><span data-stu-id="9fd41-335">The definite assignment state of *v* on the control flow transfer to *embedded_statement* is the same as the state of *v* at the end of *resource_acquisition*.</span></span>

#### <a name="lock-statements"></a><span data-ttu-id="9fd41-336">Příkazy zámku</span><span class="sxs-lookup"><span data-stu-id="9fd41-336">Lock statements</span></span>

<span data-ttu-id="9fd41-337">Pro `lock` příkaz *příkazu Insert* formuláře:</span><span class="sxs-lookup"><span data-stu-id="9fd41-337">For a `lock` statement *stmt* of the form:</span></span>
```csharp
lock ( expr ) embedded_statement
```

*  <span data-ttu-id="9fd41-338">Stav jednoznačného přiřazení *v* na začátku *expr* je stejné jako stav *v* na začátku *příkazu Insert*.</span><span class="sxs-lookup"><span data-stu-id="9fd41-338">The definite assignment state of *v* at the beginning of *expr* is the same as the state of *v* at the beginning of *stmt*.</span></span>
*  <span data-ttu-id="9fd41-339">Stav jednoznačného přiřazení *v* na přenos řízení toku do *embedded_statement* je stejné jako stav *v* na konci *expr*.</span><span class="sxs-lookup"><span data-stu-id="9fd41-339">The definite assignment state of *v* on the control flow transfer to *embedded_statement* is the same as the state of *v* at the end of *expr*.</span></span>

#### <a name="yield-statements"></a><span data-ttu-id="9fd41-340">Příkazy YIELD</span><span class="sxs-lookup"><span data-stu-id="9fd41-340">Yield statements</span></span>

<span data-ttu-id="9fd41-341">Pro `yield return` příkaz *příkazu Insert* formuláře:</span><span class="sxs-lookup"><span data-stu-id="9fd41-341">For a `yield return` statement *stmt* of the form:</span></span>
```csharp
yield return expr ;
```

*  <span data-ttu-id="9fd41-342">Stav jednoznačného přiřazení *v* na začátku *expr* je stejné jako stav *v* na začátku *příkazu Insert*.</span><span class="sxs-lookup"><span data-stu-id="9fd41-342">The definite assignment state of *v* at the beginning of *expr* is the same as the state of *v* at the beginning of *stmt*.</span></span>
*  <span data-ttu-id="9fd41-343">Stav jednoznačného přiřazení *v* na konci *příkazu Insert* je stejné jako stav *v* na konci *expr*.</span><span class="sxs-lookup"><span data-stu-id="9fd41-343">The definite assignment state of *v* at the end of *stmt* is the same as the state of *v* at the end of *expr*.</span></span>
*  <span data-ttu-id="9fd41-344">A `yield break` příkaz nemá žádný vliv na stav jednoznačného přiřazení.</span><span class="sxs-lookup"><span data-stu-id="9fd41-344">A `yield break` statement has no effect on the definite assignment state.</span></span>

#### <a name="general-rules-for-simple-expressions"></a><span data-ttu-id="9fd41-345">Obecná pravidla pro jednoduché výrazy</span><span class="sxs-lookup"><span data-stu-id="9fd41-345">General rules for simple expressions</span></span>

<span data-ttu-id="9fd41-346">Následující pravidlo se vztahuje na tyto druhy výrazů: literály ([literály](expressions.md#literals)), jednoduché názvy ([jednoduché názvy](expressions.md#simple-names)), výrazy přístupu členů ([přístup ke členu](expressions.md#member-access)), výrazy neindexovanou základní přístupu ([základní přístup](expressions.md#base-access)), `typeof` výrazy ([typeof – operátor](expressions.md#the-typeof-operator)), výchozí hodnota výrazy ([výchozí hodnotu výrazů ](expressions.md#default-value-expressions)) a `nameof` výrazy ([výrazy Nameof](expressions.md#nameof-expressions)).</span><span class="sxs-lookup"><span data-stu-id="9fd41-346">The following rule applies to these kinds of expressions: literals ([Literals](expressions.md#literals)), simple names ([Simple names](expressions.md#simple-names)), member access expressions ([Member access](expressions.md#member-access)), non-indexed base access expressions ([Base access](expressions.md#base-access)), `typeof` expressions ([The typeof operator](expressions.md#the-typeof-operator)), default value expressions ([Default value expressions](expressions.md#default-value-expressions)) and `nameof` expressions ([Nameof expressions](expressions.md#nameof-expressions)).</span></span>

*  <span data-ttu-id="9fd41-347">Stav jednoznačného přiřazení *v* na konci takový výraz je stejné jako stav jednoznačného přiřazení *v* na začátku výrazu.</span><span class="sxs-lookup"><span data-stu-id="9fd41-347">The definite assignment state of *v* at the end of such an expression is the same as the definite assignment state of *v* at the beginning of the expression.</span></span>

#### <a name="general-rules-for-expressions-with-embedded-expressions"></a><span data-ttu-id="9fd41-348">Obecná pravidla pro výrazy s vložené výrazy</span><span class="sxs-lookup"><span data-stu-id="9fd41-348">General rules for expressions with embedded expressions</span></span>

<span data-ttu-id="9fd41-349">Následující pravidla platí pro tyto druhy výrazů: výrazy v závorkách ([výrazech se závorkami](expressions.md#parenthesized-expressions)), výrazy přístupu – element ([přístup k prvkům](expressions.md#element-access)), základní přístup výrazy s indexování ([základní přístup](expressions.md#base-access)), zvýší a sníží výrazy ([Příponové operátory Inkrementace a dekrementace operátory](expressions.md#postfix-increment-and-decrement-operators), [předpony Inkrementace a dekrementace operátory](expressions.md#prefix-increment-and-decrement-operators)), výrazy přetypování ([výrazy přetypování](expressions.md#cast-expressions)), unární `+`, `-`, `~`, `*` výrazy, binární `+`, `-`, `*`, `/`, `%`, `<<`, `>>`, `<`, `<=`, `>`, `>=`, `==`, `!=`, `is`, `as`, `&`, `|`, `^` výrazy ([aritmetické operátory](expressions.md#arithmetic-operators), [operátory posunutí](expressions.md#shift-operators), [relační a typové zkoušky operátory](expressions.md#relational-and-type-testing-operators) [Logické operátory](expressions.md#logical-operators)), složené výrazy přiřazení ([složené přiřazení](expressions.md#compound-assignment)), `checked` a `unchecked` výrazy ([zaškrtnuto a nezaškrtnuto operátory](expressions.md#the-checked-and-unchecked-operators)), plus pole a delegáta vytváření výrazů ([operátor new](expressions.md#the-new-operator)).</span><span class="sxs-lookup"><span data-stu-id="9fd41-349">The following rules apply to these kinds of expressions: parenthesized expressions ([Parenthesized expressions](expressions.md#parenthesized-expressions)), element access expressions ([Element access](expressions.md#element-access)), base access expressions with indexing ([Base access](expressions.md#base-access)), increment and decrement expressions ([Postfix increment and decrement operators](expressions.md#postfix-increment-and-decrement-operators), [Prefix increment and decrement operators](expressions.md#prefix-increment-and-decrement-operators)), cast expressions ([Cast expressions](expressions.md#cast-expressions)), unary `+`, `-`, `~`, `*` expressions, binary `+`, `-`, `*`, `/`, `%`, `<<`, `>>`, `<`, `<=`, `>`, `>=`, `==`, `!=`, `is`, `as`, `&`, `|`, `^` expressions ([Arithmetic operators](expressions.md#arithmetic-operators), [Shift operators](expressions.md#shift-operators), [Relational and type-testing operators](expressions.md#relational-and-type-testing-operators), [Logical operators](expressions.md#logical-operators)), compound assignment expressions ([Compound assignment](expressions.md#compound-assignment)), `checked` and `unchecked` expressions ([The checked and unchecked operators](expressions.md#the-checked-and-unchecked-operators)), plus array and delegate creation expressions ([The new operator](expressions.md#the-new-operator)).</span></span>

<span data-ttu-id="9fd41-350">Každá z těchto výrazů má nejméně jeden dílčí výrazy, které jsou bezpodmínečně vyhodnocovány v pořadí pevné.</span><span class="sxs-lookup"><span data-stu-id="9fd41-350">Each of these expressions has one or more sub-expressions that are unconditionally evaluated in a fixed order.</span></span> <span data-ttu-id="9fd41-351">Například binární `%` operátor vyhodnotí levé straně operátoru a potom na pravé straně.</span><span class="sxs-lookup"><span data-stu-id="9fd41-351">For example, the binary `%` operator evaluates the left hand side of the operator, then the right hand side.</span></span> <span data-ttu-id="9fd41-352">Operace indexování vyhodnotí indexovaným výrazem a pak vyhodnotí každý indexové výrazy v pořadí zleva doprava.</span><span class="sxs-lookup"><span data-stu-id="9fd41-352">An indexing operation evaluates the indexed expression, and then evaluates each of the index expressions, in order from left to right.</span></span> <span data-ttu-id="9fd41-353">Pro výraz *expr*, který má dílčí výrazy *e1, e2,..., eN*, Vyhodnocená v tomto pořadí:</span><span class="sxs-lookup"><span data-stu-id="9fd41-353">For an expression *expr*, which has sub-expressions *e1, e2, ..., eN*, evaluated in that order:</span></span>

*  <span data-ttu-id="9fd41-354">Stav jednoznačného přiřazení *v* na začátku *e1* je stejný jako jednoznačného přiřazení stavu na začátku *expr*.</span><span class="sxs-lookup"><span data-stu-id="9fd41-354">The definite assignment state of *v* at the beginning of *e1* is the same as the definite assignment state at the beginning of *expr*.</span></span>
*  <span data-ttu-id="9fd41-355">Stav jednoznačného přiřazení *v* na začátku *ei* (*můžu* větší než jedna) je stejný jako stav jednoznačného přiřazení na konci předchozí dílčí výraz.</span><span class="sxs-lookup"><span data-stu-id="9fd41-355">The definite assignment state of *v* at the beginning of *ei* (*i* greater than one) is the same as the definite assignment state at the end of the previous sub-expression.</span></span>
*  <span data-ttu-id="9fd41-356">Stav jednoznačného přiřazení *v* na konci *expr* je stejné jako stav jednoznačného přiřazení na konci *eN*</span><span class="sxs-lookup"><span data-stu-id="9fd41-356">The definite assignment state of *v* at the end of *expr* is the same as the definite assignment state at the end of *eN*</span></span>

#### <a name="invocation-expressions-and-object-creation-expressions"></a><span data-ttu-id="9fd41-357">Výrazy typu Invocation a vytváření výrazy</span><span class="sxs-lookup"><span data-stu-id="9fd41-357">Invocation expressions and object creation expressions</span></span>

<span data-ttu-id="9fd41-358">Výraz volání *expr* formuláře:</span><span class="sxs-lookup"><span data-stu-id="9fd41-358">For an invocation expression *expr* of the form:</span></span>
```csharp
primary_expression ( arg1 , arg2 , ... , argN )
```
<span data-ttu-id="9fd41-359">nebo výraz vytvoření objektu ve formátu:</span><span class="sxs-lookup"><span data-stu-id="9fd41-359">or an object creation expression of the form:</span></span>
```csharp
new type ( arg1 , arg2 , ... , argN )
```

*  <span data-ttu-id="9fd41-360">Pro výraz vyvolání jednoznačného přiřazení stavu *v* před *primary_expression* je stejné jako stav *v* před *expr*.</span><span class="sxs-lookup"><span data-stu-id="9fd41-360">For an invocation expression, the definite assignment state of *v* before *primary_expression* is the same as the state of *v* before *expr*.</span></span>
*  <span data-ttu-id="9fd41-361">Pro výraz vyvolání jednoznačného přiřazení stavu *v* před *arg1* je stejné jako stav *v* po *primary_expression*.</span><span class="sxs-lookup"><span data-stu-id="9fd41-361">For an invocation expression, the definite assignment state of *v* before *arg1* is the same as the state of *v* after *primary_expression*.</span></span>
*  <span data-ttu-id="9fd41-362">Pro vytvoření výrazu objektu, stav jednoznačného přiřazení *v* před *arg1* je stejné jako stav *v* před *expr*.</span><span class="sxs-lookup"><span data-stu-id="9fd41-362">For an object creation expression, the definite assignment state of *v* before *arg1* is the same as the state of *v* before *expr*.</span></span>
*  <span data-ttu-id="9fd41-363">Pro každý argument *argi*, stav jednoznačného přiřazení *v* po *argi* se určuje podle pravidla normální výrazů, ignoruje všechny `ref` nebo `out`modifikátory.</span><span class="sxs-lookup"><span data-stu-id="9fd41-363">For each argument *argi*, the definite assignment state of *v* after *argi* is determined by the normal expression rules, ignoring any `ref` or `out` modifiers.</span></span>
*  <span data-ttu-id="9fd41-364">Pro každý argument *argi* libovolné *můžu* větší než jedna, stav jednoznačného přiřazení *v* před *argi* je stejné jako stav *v* po předchozí *arg*.</span><span class="sxs-lookup"><span data-stu-id="9fd41-364">For each argument *argi* for any *i* greater than one, the definite assignment state of *v* before *argi* is the same as the state of *v* after the previous *arg*.</span></span>
*  <span data-ttu-id="9fd41-365">Pokud proměnná *v* je předán jako `out` argument (tj, argument formuláře `out v`) v některé z argumentů, potom stav *v* po *expr* je jednoznačně přiřazena.</span><span class="sxs-lookup"><span data-stu-id="9fd41-365">If the variable *v* is passed as an `out` argument (i.e., an argument of the form `out v`) in any of the arguments, then the state of *v* after *expr* is definitely assigned.</span></span> <span data-ttu-id="9fd41-366">Jinak. Stav *v* po *expr* je stejné jako stav *v* po *argN*.</span><span class="sxs-lookup"><span data-stu-id="9fd41-366">Otherwise; the state of *v* after *expr* is the same as the state of *v* after *argN*.</span></span>
*  <span data-ttu-id="9fd41-367">Pro Inicializátory polí ([pole vytváření výrazů](expressions.md#array-creation-expressions)), inicializátorech objektu ([inicializátorech objektu](expressions.md#object-initializers)), inicializátory kolekce ([inicializátory kolekce](expressions.md#collection-initializers)) a Inicializátory anonymních objektů ([anonymní objekt vytváření výrazů](expressions.md#anonymous-object-creation-expressions)), stav jednoznačného přiřazení je určeno rozšíření, které tyto konstrukce jsou definovány z hlediska.</span><span class="sxs-lookup"><span data-stu-id="9fd41-367">For array initializers ([Array creation expressions](expressions.md#array-creation-expressions)), object initializers ([Object initializers](expressions.md#object-initializers)), collection initializers ([Collection initializers](expressions.md#collection-initializers)) and anonymous object initializers ([Anonymous object creation expressions](expressions.md#anonymous-object-creation-expressions)), the definite assignment state is determined by the expansion that these constructs are defined in terms of.</span></span>

#### <a name="simple-assignment-expressions"></a><span data-ttu-id="9fd41-368">Jednoduché přiřazení výrazy</span><span class="sxs-lookup"><span data-stu-id="9fd41-368">Simple assignment expressions</span></span>

<span data-ttu-id="9fd41-369">Pro výraz *expr* formuláře `w = expr_rhs`:</span><span class="sxs-lookup"><span data-stu-id="9fd41-369">For an expression *expr* of the form `w = expr_rhs`:</span></span>

*  <span data-ttu-id="9fd41-370">Stav jednoznačného přiřazení *v* před *expr_rhs* je stejné jako stav jednoznačného přiřazení *v* před *expr*.</span><span class="sxs-lookup"><span data-stu-id="9fd41-370">The definite assignment state of *v* before *expr_rhs* is the same as the definite assignment state of *v* before *expr*.</span></span>
*  <span data-ttu-id="9fd41-371">Stav jednoznačného přiřazení *v* po *expr* se určuje podle:</span><span class="sxs-lookup"><span data-stu-id="9fd41-371">The definite assignment state of *v* after *expr* is determined by:</span></span>
   * <span data-ttu-id="9fd41-372">Pokud *w* stejnou proměnnou, jako je *v*, pak jednoznačného přiřazení stavu *v* po *expr* jednoznačně přiřazena.</span><span class="sxs-lookup"><span data-stu-id="9fd41-372">If *w* is the same variable as *v*, then the definite assignment state of *v* after *expr* is definitely assigned.</span></span>
   * <span data-ttu-id="9fd41-373">Jinak, pokud dojde k přiřazení v rámci konstruktoru instance typu struktury, pokud *w* je přístup k vlastnosti s vyznačením automaticky implementovanou vlastnost *P* vytváří instance a *v* je pole Skrytá zálohování *P*, pak jednoznačného přiřazení stavu *v* po *expr* se o výborný prostředek přiřadit.</span><span class="sxs-lookup"><span data-stu-id="9fd41-373">Otherwise, if the assignment occurs within the instance constructor of a struct type, if *w* is a property access designating an automatically implemented property *P* on the instance being constructed and *v* is the hidden backing field of *P*, then the definite assignment state of *v* after *expr* is definitely assigned.</span></span>
   * <span data-ttu-id="9fd41-374">V opačném případě jednoznačného přiřazení stavu *v* po *expr* je stejné jako stav jednoznačného přiřazení *v* po *expr_rhs*.</span><span class="sxs-lookup"><span data-stu-id="9fd41-374">Otherwise, the definite assignment state of *v* after *expr* is the same as the definite assignment state of *v* after *expr_rhs*.</span></span>

#### <a name="-conditional-and-expressions"></a><span data-ttu-id="9fd41-375">& & (podmiňovací operátor AND) výrazů</span><span class="sxs-lookup"><span data-stu-id="9fd41-375">&& (conditional AND) expressions</span></span>

<span data-ttu-id="9fd41-376">Pro výraz *expr* formuláře `expr_first && expr_second`:</span><span class="sxs-lookup"><span data-stu-id="9fd41-376">For an expression *expr* of the form `expr_first && expr_second`:</span></span>

*  <span data-ttu-id="9fd41-377">Stav jednoznačného přiřazení *v* před *expr_first* je stejné jako stav jednoznačného přiřazení *v* před *expr*.</span><span class="sxs-lookup"><span data-stu-id="9fd41-377">The definite assignment state of *v* before *expr_first* is the same as the definite assignment state of *v* before *expr*.</span></span>
*  <span data-ttu-id="9fd41-378">Stav jednoznačného přiřazení *v* před *expr_second* je jednoznačně přiřazovat, pokud státu *v* po *expr_first* je buď určitě přiřazené nebo "přiřazené jednoznačně po Výraz true".</span><span class="sxs-lookup"><span data-stu-id="9fd41-378">The definite assignment state of *v* before *expr_second* is definitely assigned if the state of *v* after *expr_first* is either definitely assigned or "definitely assigned after true expression".</span></span> <span data-ttu-id="9fd41-379">V opačném případě ji není jednoznačně přiřazena.</span><span class="sxs-lookup"><span data-stu-id="9fd41-379">Otherwise, it is not definitely assigned.</span></span>
*  <span data-ttu-id="9fd41-380">Stav jednoznačného přiřazení *v* po *expr* se určuje podle:</span><span class="sxs-lookup"><span data-stu-id="9fd41-380">The definite assignment state of *v* after *expr* is determined by:</span></span>
    * <span data-ttu-id="9fd41-381">Pokud *expr_first* je konstantní výraz s hodnotou `false`, pak jednoznačného přiřazení stavu *v* po *expr* je stejný jako jednoznačného přiřazení Stav *v* po *expr_first*.</span><span class="sxs-lookup"><span data-stu-id="9fd41-381">If *expr_first* is a constant expression with the value `false`, then the definite assignment state of *v* after *expr* is the same as the definite assignment state of *v* after *expr_first*.</span></span>
    * <span data-ttu-id="9fd41-382">Jinak, pokud státu *v* po *expr_first* jednoznačně přiřazena, potom stav *v* po *expr* jednoznačně přiřazena.</span><span class="sxs-lookup"><span data-stu-id="9fd41-382">Otherwise, if the state of *v* after *expr_first* is definitely assigned, then the state of *v* after *expr* is definitely assigned.</span></span>
    * <span data-ttu-id="9fd41-383">Jinak, pokud stav *v* po *expr_second* jednoznačně přiřazena a stav *v* po *expr_first* je "jednoznačně přiřazené po false výraz"a pak stav *v* po *expr* jednoznačně přiřazena.</span><span class="sxs-lookup"><span data-stu-id="9fd41-383">Otherwise, if the state of *v* after *expr_second* is definitely assigned, and the state of *v* after *expr_first* is "definitely assigned after false expression", then the state of *v* after *expr* is definitely assigned.</span></span>
    * <span data-ttu-id="9fd41-384">Jinak, pokud státu *v* po *expr_second* jednoznačně přiřazena nebo "přiřazené jednoznačně po hodnotu true, výraz", potom stav *v* po  *výraz* "jednoznačně přiřazena po Výraz true".</span><span class="sxs-lookup"><span data-stu-id="9fd41-384">Otherwise, if the state of *v* after *expr_second* is definitely assigned or "definitely assigned after true expression", then the state of *v* after *expr* is "definitely assigned after true expression".</span></span>
    * <span data-ttu-id="9fd41-385">Jinak, pokud státu *v* po *expr_first* je "přiřazené jednoznačně po false výraz" a stav *v* po *expr_second* "přiřazené jednoznačně po false výraz", je stav *v* po *expr* "jednoznačně přiřazena po výrazu false".</span><span class="sxs-lookup"><span data-stu-id="9fd41-385">Otherwise, if the state of *v* after *expr_first* is "definitely assigned after false expression", and the state of *v* after *expr_second* is "definitely assigned after false expression", then the state of *v* after *expr* is "definitely assigned after false expression".</span></span>
    * <span data-ttu-id="9fd41-386">V opačném případě stav *v* po *expr* není jednoznačně přiřazena.</span><span class="sxs-lookup"><span data-stu-id="9fd41-386">Otherwise, the state of *v* after *expr* is not definitely assigned.</span></span>

<span data-ttu-id="9fd41-387">V příkladu</span><span class="sxs-lookup"><span data-stu-id="9fd41-387">In the example</span></span>
```csharp
class A
{
    static void F(int x, int y) {
        int i;
        if (x >= 0 && (i = y) >= 0) {
            // i definitely assigned
        }
        else {
            // i not definitely assigned
        }
        // i not definitely assigned
    }
}
```
<span data-ttu-id="9fd41-388">proměnné `i` se považuje za jednoznačně přiřazené v jednom z vložené příkazy `if` příkazu, ale ne v jiném.</span><span class="sxs-lookup"><span data-stu-id="9fd41-388">the variable `i` is considered definitely assigned in one of the embedded statements of an `if` statement but not in the other.</span></span> <span data-ttu-id="9fd41-389">V `if` příkaz v metodě `F`, proměnná `i` je jednoznačně přiřazena první příkaz vložený, protože provádění výrazu `(i = y)` vždy předchází spuštění tohoto příkazu embedded.</span><span class="sxs-lookup"><span data-stu-id="9fd41-389">In the `if` statement in method `F`, the variable `i` is definitely assigned in the first embedded statement because execution of the expression `(i = y)` always precedes execution of this embedded statement.</span></span> <span data-ttu-id="9fd41-390">Naproti tomu, proměnná `i` není jednoznačně přiřazena v druhém příkazu vložený, protože `x >= 0` může otestovali false, což vede k proměnné `i` se nepřiřazený.</span><span class="sxs-lookup"><span data-stu-id="9fd41-390">In contrast, the variable `i` is not definitely assigned in the second embedded statement, since `x >= 0` might have tested false, resulting in the variable `i` being unassigned.</span></span>

#### <a name="-conditional-or-expressions"></a><span data-ttu-id="9fd41-391">|| (podmiňovací operátor OR) výrazů</span><span class="sxs-lookup"><span data-stu-id="9fd41-391">|| (conditional OR) expressions</span></span>

<span data-ttu-id="9fd41-392">Pro výraz *expr* formuláře `expr_first || expr_second`:</span><span class="sxs-lookup"><span data-stu-id="9fd41-392">For an expression *expr* of the form `expr_first || expr_second`:</span></span>

*  <span data-ttu-id="9fd41-393">Stav jednoznačného přiřazení *v* před *expr_first* je stejné jako stav jednoznačného přiřazení *v* před *expr*.</span><span class="sxs-lookup"><span data-stu-id="9fd41-393">The definite assignment state of *v* before *expr_first* is the same as the definite assignment state of *v* before *expr*.</span></span>
*  <span data-ttu-id="9fd41-394">Stav jednoznačného přiřazení *v* před *expr_second* je jednoznačně přiřazovat, pokud státu *v* po *expr_first* je buď určitě přiřazené nebo "přiřazené jednoznačně po výrazu false".</span><span class="sxs-lookup"><span data-stu-id="9fd41-394">The definite assignment state of *v* before *expr_second* is definitely assigned if the state of *v* after *expr_first* is either definitely assigned or "definitely assigned after false expression".</span></span> <span data-ttu-id="9fd41-395">V opačném případě ji není jednoznačně přiřazena.</span><span class="sxs-lookup"><span data-stu-id="9fd41-395">Otherwise, it is not definitely assigned.</span></span>
*  <span data-ttu-id="9fd41-396">Příkaz jednoznačného přiřazení *v* po *expr* se určuje podle:</span><span class="sxs-lookup"><span data-stu-id="9fd41-396">The definite assignment statement of *v* after *expr* is determined by:</span></span>
    * <span data-ttu-id="9fd41-397">Pokud *expr_first* je konstantní výraz s hodnotou `true`, pak jednoznačného přiřazení stavu *v* po *expr* je stejný jako jednoznačného přiřazení Stav *v* po *expr_first*.</span><span class="sxs-lookup"><span data-stu-id="9fd41-397">If *expr_first* is a constant expression with the value `true`, then the definite assignment state of *v* after *expr* is the same as the definite assignment state of *v* after *expr_first*.</span></span>
    * <span data-ttu-id="9fd41-398">Jinak, pokud státu *v* po *expr_first* jednoznačně přiřazena, potom stav *v* po *expr* jednoznačně přiřazena.</span><span class="sxs-lookup"><span data-stu-id="9fd41-398">Otherwise, if the state of *v* after *expr_first* is definitely assigned, then the state of *v* after *expr* is definitely assigned.</span></span>
    * <span data-ttu-id="9fd41-399">Jinak, pokud stav *v* po *expr_second* jednoznačně přiřazena a stav *v* po *expr_first* je "jednoznačně přiřazené po hodnotu true, výraz"a pak stav *v* po *expr* jednoznačně přiřazena.</span><span class="sxs-lookup"><span data-stu-id="9fd41-399">Otherwise, if the state of *v* after *expr_second* is definitely assigned, and the state of *v* after *expr_first* is "definitely assigned after true expression", then the state of *v* after *expr* is definitely assigned.</span></span>
    * <span data-ttu-id="9fd41-400">Jinak, pokud státu *v* po *expr_second* jednoznačně přiřazena nebo "přiřazené jednoznačně po false výraz", potom stav *v* po *expr* "jednoznačně přiřazena po výrazu false".</span><span class="sxs-lookup"><span data-stu-id="9fd41-400">Otherwise, if the state of *v* after *expr_second* is definitely assigned or "definitely assigned after false expression", then the state of *v* after *expr* is "definitely assigned after false expression".</span></span>
    * <span data-ttu-id="9fd41-401">Jinak, pokud státu *v* po *expr_first* je "přiřazené jednoznačně po hodnotu true, výraz" a stav *v* po *expr_second*"přiřazené jednoznačně po hodnotu true, výraz", je stav *v* po *expr* "jednoznačně přiřazena po Výraz true".</span><span class="sxs-lookup"><span data-stu-id="9fd41-401">Otherwise, if the state of *v* after *expr_first* is "definitely assigned after true expression", and the state of *v* after *expr_second* is "definitely assigned after true expression", then the state of *v* after *expr* is "definitely assigned after true expression".</span></span>
    * <span data-ttu-id="9fd41-402">V opačném případě stav *v* po *expr* není jednoznačně přiřazena.</span><span class="sxs-lookup"><span data-stu-id="9fd41-402">Otherwise, the state of *v* after *expr* is not definitely assigned.</span></span>

<span data-ttu-id="9fd41-403">V příkladu</span><span class="sxs-lookup"><span data-stu-id="9fd41-403">In the example</span></span>
```csharp
class A
{
    static void G(int x, int y) {
        int i;
        if (x >= 0 || (i = y) >= 0) {
            // i not definitely assigned
        }
        else {
            // i definitely assigned
        }
        // i not definitely assigned
    }
}
```
<span data-ttu-id="9fd41-404">proměnné `i` se považuje za jednoznačně přiřazené v jednom z vložené příkazy `if` příkazu, ale ne v jiném.</span><span class="sxs-lookup"><span data-stu-id="9fd41-404">the variable `i` is considered definitely assigned in one of the embedded statements of an `if` statement but not in the other.</span></span> <span data-ttu-id="9fd41-405">V `if` příkaz v metodě `G`, proměnná `i` jednoznačně přiřazena v druhém příkazu vložený protože provádění výrazu `(i = y)` vždy předchází spuštění tohoto příkazu embedded.</span><span class="sxs-lookup"><span data-stu-id="9fd41-405">In the `if` statement in method `G`, the variable `i` is definitely assigned in the second embedded statement because execution of the expression `(i = y)` always precedes execution of this embedded statement.</span></span> <span data-ttu-id="9fd41-406">Naproti tomu, proměnná `i` není jednoznačně přiřazena v první příkaz vložený, protože `x >= 0` může otestovali nastavena hodnota true, což vede k proměnné `i` se nepřiřazený.</span><span class="sxs-lookup"><span data-stu-id="9fd41-406">In contrast, the variable `i` is not definitely assigned in the first embedded statement, since `x >= 0` might have tested true, resulting in the variable `i` being unassigned.</span></span>

#### <a name="-logical-negation-expressions"></a><span data-ttu-id="9fd41-407">!</span><span class="sxs-lookup"><span data-stu-id="9fd41-407">!</span></span> <span data-ttu-id="9fd41-408">výrazy (Logická negace)</span><span class="sxs-lookup"><span data-stu-id="9fd41-408">(logical negation) expressions</span></span>

<span data-ttu-id="9fd41-409">Pro výraz *expr* formuláře `! expr_operand`:</span><span class="sxs-lookup"><span data-stu-id="9fd41-409">For an expression *expr* of the form `! expr_operand`:</span></span>

*  <span data-ttu-id="9fd41-410">Stav jednoznačného přiřazení *v* před *expr_operand* je stejné jako stav jednoznačného přiřazení *v* před *expr*.</span><span class="sxs-lookup"><span data-stu-id="9fd41-410">The definite assignment state of *v* before *expr_operand* is the same as the definite assignment state of *v* before *expr*.</span></span>
*  <span data-ttu-id="9fd41-411">Stav jednoznačného přiřazení *v* po *expr* se určuje podle:</span><span class="sxs-lookup"><span data-stu-id="9fd41-411">The definite assignment state of *v* after *expr* is determined by:</span></span>
    * <span data-ttu-id="9fd41-412">Pokud stav *v* po \* expr_operand \* je jednoznačně přiřazena, potom stav *v* po *expr* jednoznačně přiřazena.</span><span class="sxs-lookup"><span data-stu-id="9fd41-412">If the state of *v* after \*expr_operand \*is definitely assigned, then the state of *v* after *expr* is definitely assigned.</span></span>
    * <span data-ttu-id="9fd41-413">Pokud státu *v* po \* expr_operand \* není jednoznačně přiřazena, potom stav *v* po *expr* není jednoznačně přiřazena.</span><span class="sxs-lookup"><span data-stu-id="9fd41-413">If the state of *v* after \*expr_operand \*is not definitely assigned, then the state of *v* after *expr* is not definitely assigned.</span></span>
    * <span data-ttu-id="9fd41-414">Pokud stav *v* po \* expr_operand \* "přiřazené jednoznačně po false výraz", je stav *v* po *expr* "jednoznačně přiřazena po true výraz".</span><span class="sxs-lookup"><span data-stu-id="9fd41-414">If the state of *v* after \*expr_operand \*is "definitely assigned after false expression", then the state of *v* after *expr* is "definitely assigned after true expression".</span></span>
    * <span data-ttu-id="9fd41-415">Pokud stav *v* po \* expr_operand \* "přiřazené jednoznačně po hodnotu true, výraz", je stav *v* po *expr* "jednoznačně přiřazena po false výraz".</span><span class="sxs-lookup"><span data-stu-id="9fd41-415">If the state of *v* after \*expr_operand \*is "definitely assigned after true expression", then the state of *v* after *expr* is "definitely assigned after false expression".</span></span>

#### <a name="-null-coalescing-expressions"></a><span data-ttu-id="9fd41-416">??</span><span class="sxs-lookup"><span data-stu-id="9fd41-416">??</span></span> <span data-ttu-id="9fd41-417">výrazy (nulové sloučení)</span><span class="sxs-lookup"><span data-stu-id="9fd41-417">(null coalescing) expressions</span></span>

<span data-ttu-id="9fd41-418">Pro výraz *expr* formuláře `expr_first ?? expr_second`:</span><span class="sxs-lookup"><span data-stu-id="9fd41-418">For an expression *expr* of the form `expr_first ?? expr_second`:</span></span>

*  <span data-ttu-id="9fd41-419">Stav jednoznačného přiřazení *v* před *expr_first* je stejné jako stav jednoznačného přiřazení *v* před *expr*.</span><span class="sxs-lookup"><span data-stu-id="9fd41-419">The definite assignment state of *v* before *expr_first* is the same as the definite assignment state of *v* before *expr*.</span></span>
*  <span data-ttu-id="9fd41-420">Stav jednoznačného přiřazení *v* před *expr_second* je stejné jako stav jednoznačného přiřazení *v* po *expr_first*.</span><span class="sxs-lookup"><span data-stu-id="9fd41-420">The definite assignment state of *v* before *expr_second* is the same as the definite assignment state of *v* after *expr_first*.</span></span>
*  <span data-ttu-id="9fd41-421">Příkaz jednoznačného přiřazení *v* po *expr* se určuje podle:</span><span class="sxs-lookup"><span data-stu-id="9fd41-421">The definite assignment statement of *v* after *expr* is determined by:</span></span>
    * <span data-ttu-id="9fd41-422">Pokud *expr_first* je konstantní výraz ([konstantní výrazy](expressions.md#constant-expressions)) s hodnotou null, pak bude stav *v* po *expr* stejný jako stav *v* po *expr_second*.</span><span class="sxs-lookup"><span data-stu-id="9fd41-422">If *expr_first* is a constant expression ([Constant expressions](expressions.md#constant-expressions)) with value null, then the the state of *v* after *expr* is the same as the state of *v* after *expr_second*.</span></span>
*  <span data-ttu-id="9fd41-423">V opačném případě stav *v* po *expr* je stejné jako stav jednoznačného přiřazení *v* po *expr_first*.</span><span class="sxs-lookup"><span data-stu-id="9fd41-423">Otherwise, the state of *v* after *expr* is the same as the definite assignment state of *v* after *expr_first*.</span></span>

#### <a name="-conditional-expressions"></a><span data-ttu-id="9fd41-424">?: (podmínky)</span><span class="sxs-lookup"><span data-stu-id="9fd41-424">?: (conditional) expressions</span></span>

<span data-ttu-id="9fd41-425">Pro výraz *expr* formuláře `expr_cond ? expr_true : expr_false`:</span><span class="sxs-lookup"><span data-stu-id="9fd41-425">For an expression *expr* of the form `expr_cond ? expr_true : expr_false`:</span></span>

*  <span data-ttu-id="9fd41-426">Stav jednoznačného přiřazení *v* před *expr_cond* je stejné jako stav *v* před *expr*.</span><span class="sxs-lookup"><span data-stu-id="9fd41-426">The definite assignment state of *v* before *expr_cond* is the same as the state of *v* before *expr*.</span></span>
*  <span data-ttu-id="9fd41-427">Stav jednoznačného přiřazení *v* před *expr_true* jednoznačně přiřazena pouze v případě obsahuje jeden z následujících akcí:</span><span class="sxs-lookup"><span data-stu-id="9fd41-427">The definite assignment state of *v* before *expr_true* is definitely assigned if and only if one of the following holds:</span></span>
    * <span data-ttu-id="9fd41-428">*expr_cond* je konstantní výraz s hodnotou `false`</span><span class="sxs-lookup"><span data-stu-id="9fd41-428">*expr_cond* is a constant expression with the value `false`</span></span>
    * <span data-ttu-id="9fd41-429">Stav *v* po *expr_cond* je jednoznačně přiřazovat nebo "jednoznačně přiřazena po Výraz true".</span><span class="sxs-lookup"><span data-stu-id="9fd41-429">the state of *v* after *expr_cond* is definitely assigned or "definitely assigned after true expression".</span></span>
*  <span data-ttu-id="9fd41-430">Stav jednoznačného přiřazení *v* před *expr_false* jednoznačně přiřazena pouze v případě obsahuje jeden z následujících akcí:</span><span class="sxs-lookup"><span data-stu-id="9fd41-430">The definite assignment state of *v* before *expr_false* is definitely assigned if and only if one of the following holds:</span></span>
    * <span data-ttu-id="9fd41-431">*expr_cond* je konstantní výraz s hodnotou `true`</span><span class="sxs-lookup"><span data-stu-id="9fd41-431">*expr_cond* is a constant expression with the value `true`</span></span>
*  <span data-ttu-id="9fd41-432">Stav *v* po *expr_cond* je jednoznačně přiřazovat nebo "jednoznačně přiřazena po výrazu false".</span><span class="sxs-lookup"><span data-stu-id="9fd41-432">the state of *v* after *expr_cond* is definitely assigned or "definitely assigned after false expression".</span></span>
*  <span data-ttu-id="9fd41-433">Stav jednoznačného přiřazení *v* po *expr* se určuje podle:</span><span class="sxs-lookup"><span data-stu-id="9fd41-433">The definite assignment state of *v* after *expr* is determined by:</span></span>
    * <span data-ttu-id="9fd41-434">Pokud *expr_cond* je konstantní výraz ([konstantní výrazy](expressions.md#constant-expressions)) s hodnotou `true` pak stav *v* po *expr* je stejný jako stav *v* po *expr_true*.</span><span class="sxs-lookup"><span data-stu-id="9fd41-434">If *expr_cond* is a constant expression ([Constant expressions](expressions.md#constant-expressions)) with value `true` then the state of *v* after *expr* is the same as the state of *v* after *expr_true*.</span></span>
    * <span data-ttu-id="9fd41-435">Jinak, pokud *expr_cond* je konstantní výraz ([konstantní výrazy](expressions.md#constant-expressions)) s hodnotou `false` pak stav *v* po *expr* je stejné jako stav *v* po *expr_false*.</span><span class="sxs-lookup"><span data-stu-id="9fd41-435">Otherwise, if *expr_cond* is a constant expression ([Constant expressions](expressions.md#constant-expressions)) with value `false` then the state of *v* after *expr* is the same as the state of *v* after *expr_false*.</span></span>
    * <span data-ttu-id="9fd41-436">Jinak, pokud státu *v* po *expr_true* je jednoznačně přiřazovat a stav *v* po *expr_false* se o výborný prostředek pak přiřazen stav *v* po *expr* jednoznačně přiřazena.</span><span class="sxs-lookup"><span data-stu-id="9fd41-436">Otherwise, if the state of *v* after *expr_true* is definitely assigned and the state of *v* after *expr_false* is definitely assigned, then the state of *v* after *expr* is definitely assigned.</span></span>
    * <span data-ttu-id="9fd41-437">V opačném případě stav *v* po *expr* není jednoznačně přiřazena.</span><span class="sxs-lookup"><span data-stu-id="9fd41-437">Otherwise, the state of *v* after *expr* is not definitely assigned.</span></span>

#### <a name="anonymous-functions"></a><span data-ttu-id="9fd41-438">Anonymní funkce</span><span class="sxs-lookup"><span data-stu-id="9fd41-438">Anonymous functions</span></span>

<span data-ttu-id="9fd41-439">Pro *lambda_expression* nebo *anonymous_method_expression* *expr* s tělem (buď *bloku* nebo *výraz* ) *tělo*:</span><span class="sxs-lookup"><span data-stu-id="9fd41-439">For a *lambda_expression* or *anonymous_method_expression* *expr* with a body (either *block* or *expression*) *body*:</span></span>

*  <span data-ttu-id="9fd41-440">Stav jednoznačného přiřazení vnější proměnná *v* před *tělo* je stejné jako stav *v* před *expr*.</span><span class="sxs-lookup"><span data-stu-id="9fd41-440">The definite assignment state of an outer variable *v* before *body* is the same as the state of *v* before *expr*.</span></span> <span data-ttu-id="9fd41-441">To znamená jednoznačného přiřazení stavu vnější proměnné je zděděno z kontextu anonymní funkce.</span><span class="sxs-lookup"><span data-stu-id="9fd41-441">That is, definite assignment state of outer variables is inherited from the context of the anonymous function.</span></span>
*  <span data-ttu-id="9fd41-442">Stav jednoznačného přiřazení vnější proměnná *v* po *expr* je stejné jako stav *v* před *expr*.</span><span class="sxs-lookup"><span data-stu-id="9fd41-442">The definite assignment state of an outer variable *v* after *expr* is the same as the state of *v* before *expr*.</span></span>

<span data-ttu-id="9fd41-443">V příkladu</span><span class="sxs-lookup"><span data-stu-id="9fd41-443">The example</span></span>
```csharp
delegate bool Filter(int i);

void F() {
    int max;

    // Error, max is not definitely assigned
    Filter f = (int n) => n < max;

    max = 5;
    DoWork(f);
}
```
<span data-ttu-id="9fd41-444">vygeneruje chybu kompilace od `max` není jednoznačně přiřazena, ve kterém je deklarována jako anonymní funkce.</span><span class="sxs-lookup"><span data-stu-id="9fd41-444">generates a compile-time error since `max` is not definitely assigned where the anonymous function is declared.</span></span> <span data-ttu-id="9fd41-445">V příkladu</span><span class="sxs-lookup"><span data-stu-id="9fd41-445">The example</span></span>
```csharp
delegate void D();

void F() {
    int n;
    D d = () => { n = 1; };

    d();

    // Error, n is not definitely assigned
    Console.WriteLine(n);
}
```
<span data-ttu-id="9fd41-446">také vygeneruje chybu kompilace od přiřazení `n` v anonymní funkce nemá žádný vliv na stav jednoznačného přiřazení `n` vně anonymní funkce.</span><span class="sxs-lookup"><span data-stu-id="9fd41-446">also generates a compile-time error since the assignment to `n` in the anonymous function has no affect on the definite assignment state of `n` outside the anonymous function.</span></span>

## <a name="variable-references"></a><span data-ttu-id="9fd41-447">Odkazy na proměnné</span><span class="sxs-lookup"><span data-stu-id="9fd41-447">Variable references</span></span>

<span data-ttu-id="9fd41-448">A *variable_reference* je *výraz* , který je klasifikován jako proměnnou.</span><span class="sxs-lookup"><span data-stu-id="9fd41-448">A *variable_reference* is an *expression* that is classified as a variable.</span></span> <span data-ttu-id="9fd41-449">A *variable_reference* označuje umístění úložiště, který je přístupný načíst aktuální hodnotu a uložte novou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="9fd41-449">A *variable_reference* denotes a storage location that can be accessed both to fetch the current value and to store a new value.</span></span>

```antlr
variable_reference
    : expression
    ;
```

<span data-ttu-id="9fd41-450">V jazyce C a C++ *variable_reference* se označuje jako *l-hodnoty*.</span><span class="sxs-lookup"><span data-stu-id="9fd41-450">In C and C++, a *variable_reference* is known as an *lvalue*.</span></span>

## <a name="atomicity-of-variable-references"></a><span data-ttu-id="9fd41-451">Atomicitu odkazy na proměnné</span><span class="sxs-lookup"><span data-stu-id="9fd41-451">Atomicity of variable references</span></span>

<span data-ttu-id="9fd41-452">Čtení a zápisy následující datové typy jsou atomické: `bool`, `char`, `byte`, `sbyte`, `short`, `ushort`, `uint`, `int`, `float`a typy odkazů.</span><span class="sxs-lookup"><span data-stu-id="9fd41-452">Reads and writes of the following data types are atomic: `bool`, `char`, `byte`, `sbyte`, `short`, `ushort`, `uint`, `int`, `float`, and reference types.</span></span> <span data-ttu-id="9fd41-453">Kromě toho přečtených a zapsaných typů výčtů se základním typem v předchozím seznamu jsou také atomické.</span><span class="sxs-lookup"><span data-stu-id="9fd41-453">In addition, reads and writes of enum types with an underlying type in the previous list are also atomic.</span></span> <span data-ttu-id="9fd41-454">Operace čtení a zápisu z ostatních typů, včetně `long`, `ulong`, `double`, a `decimal`, a také uživatelem definovaných typů, nemusí být atomické.</span><span class="sxs-lookup"><span data-stu-id="9fd41-454">Reads and writes of other types, including `long`, `ulong`, `double`, and `decimal`, as well as user-defined types, are not guaranteed to be atomic.</span></span> <span data-ttu-id="9fd41-455">Kromě funkcí knihovny určená pro tento účel neexistuje žádná záruka z atomických čtení modify-write, například v případě zvýšení nebo snížení.</span><span class="sxs-lookup"><span data-stu-id="9fd41-455">Aside from the library functions designed for that purpose, there is no guarantee of atomic read-modify-write, such as in the case of increment or decrement.</span></span>

