# <a name="statements"></a><span data-ttu-id="2395b-101">Příkazy</span><span class="sxs-lookup"><span data-stu-id="2395b-101">Statements</span></span>

<span data-ttu-id="2395b-102">C# poskytuje celou paletu příkazů.</span><span class="sxs-lookup"><span data-stu-id="2395b-102">C# provides a variety of statements.</span></span> <span data-ttu-id="2395b-103">Většina z těchto příkazů bude zkušenosti vývojáře, kteří mají programovat v jazycích C a C++.</span><span class="sxs-lookup"><span data-stu-id="2395b-103">Most of these statements will be familiar to developers who have programmed in C and C++.</span></span>

```antlr
statement
    : labeled_statement
    | declaration_statement
    | embedded_statement
    ;

embedded_statement
    : block
    | empty_statement
    | expression_statement
    | selection_statement
    | iteration_statement
    | jump_statement
    | try_statement
    | checked_statement
    | unchecked_statement
    | lock_statement
    | using_statement
    | yield_statement
    | embedded_statement_unsafe
    ;
```

<span data-ttu-id="2395b-104">*Embedded_statement* neterminálu se používá pro příkazy, které se zobrazují v rámci jiných příkazů.</span><span class="sxs-lookup"><span data-stu-id="2395b-104">The *embedded_statement* nonterminal is used for statements that appear within other statements.</span></span> <span data-ttu-id="2395b-105">Použití *embedded_statement* spíše než *příkaz* vyloučí použití příkazy deklarace a příkazy s popiskem v těchto kontextech.</span><span class="sxs-lookup"><span data-stu-id="2395b-105">The use of *embedded_statement* rather than *statement* excludes the use of declaration statements and labeled statements in these contexts.</span></span> <span data-ttu-id="2395b-106">V příkladu</span><span class="sxs-lookup"><span data-stu-id="2395b-106">The example</span></span>
```csharp
void F(bool b) {
    if (b)
        int i = 44;
}
```
<span data-ttu-id="2395b-107">výsledkem chyba kompilace, protože `if` příkazu vyžaduje *embedded_statement* spíše než *příkaz* pro jeho Pokud větev.</span><span class="sxs-lookup"><span data-stu-id="2395b-107">results in a compile-time error because an `if` statement requires an *embedded_statement* rather than a *statement* for its if branch.</span></span> <span data-ttu-id="2395b-108">Pokud tento kód byly převody povoleny, pak proměnná `i` by být deklarovány, ale může nikdy nepoužívá.</span><span class="sxs-lookup"><span data-stu-id="2395b-108">If this code were permitted, then the variable `i` would be declared, but it could never be used.</span></span> <span data-ttu-id="2395b-109">Mějte na paměti, ale, tak, že `i`na deklaraci v bloku, v příkladu je platný.</span><span class="sxs-lookup"><span data-stu-id="2395b-109">Note, however, that by placing `i`'s declaration in a block, the example is valid.</span></span>

## <a name="end-points-and-reachability"></a><span data-ttu-id="2395b-110">Koncové body a dostupnosti</span><span class="sxs-lookup"><span data-stu-id="2395b-110">End points and reachability</span></span>

<span data-ttu-id="2395b-111">Každý příkaz má ***koncový bod***.</span><span class="sxs-lookup"><span data-stu-id="2395b-111">Every statement has an ***end point***.</span></span> <span data-ttu-id="2395b-112">Intuitivní řečeno koncový bod příkazu je umístění, které bezprostředně následuje po příkazu.</span><span class="sxs-lookup"><span data-stu-id="2395b-112">In intuitive terms, the end point of a statement is the location that immediately follows the statement.</span></span> <span data-ttu-id="2395b-113">Spuštění pravidla pro složené příkazy (příkazy, které obsahují vložené příkazy) určují akce, která se provede, když ovládací prvek dosáhne koncového bodu vloženým příkazem.</span><span class="sxs-lookup"><span data-stu-id="2395b-113">The execution rules for composite statements (statements that contain embedded statements) specify the action that is taken when control reaches the end point of an embedded statement.</span></span> <span data-ttu-id="2395b-114">Například když ovládací prvek dosáhne příkazu v bloku koncový bod, je kontrola předána dalšímu příkazu v bloku.</span><span class="sxs-lookup"><span data-stu-id="2395b-114">For example, when control reaches the end point of a statement in a block, control is transferred to the next statement in the block.</span></span>

<span data-ttu-id="2395b-115">Pokud příkaz může být dosažitelný z provádění, příkaz se říká, že ***dostupný***.</span><span class="sxs-lookup"><span data-stu-id="2395b-115">If a statement can possibly be reached by execution, the statement is said to be ***reachable***.</span></span> <span data-ttu-id="2395b-116">Naopak, pokud není možné, že se spustí příkaz, příkaz se říká, že ***nedostupný***.</span><span class="sxs-lookup"><span data-stu-id="2395b-116">Conversely, if there is no possibility that a statement will be executed, the statement is said to be ***unreachable***.</span></span>

<span data-ttu-id="2395b-117">V příkladu</span><span class="sxs-lookup"><span data-stu-id="2395b-117">In the example</span></span>
```csharp
void F() {
    Console.WriteLine("reachable");
    goto Label;
    Console.WriteLine("unreachable");
    Label:
    Console.WriteLine("reachable");
}
```
<span data-ttu-id="2395b-118">druhé volání `Console.WriteLine` nedosažitelný, proto není možné, že se spustí příkaz.</span><span class="sxs-lookup"><span data-stu-id="2395b-118">the second invocation of `Console.WriteLine` is unreachable because there is no possibility that the statement will be executed.</span></span>

<span data-ttu-id="2395b-119">Upozornění bude nahlášena, pokud kompilátor zjistí, že příkaz nedosažitelný.</span><span class="sxs-lookup"><span data-stu-id="2395b-119">A warning is reported if the compiler determines that a statement is unreachable.</span></span> <span data-ttu-id="2395b-120">Je speciálně není chyba pro příkaz nedostupná.</span><span class="sxs-lookup"><span data-stu-id="2395b-120">It is specifically not an error for a statement to be unreachable.</span></span>

<span data-ttu-id="2395b-121">Pokud chcete zjistit, jestli je konkrétní příkaz nebo koncový bod dostupný, kompilátor provede analýzu toku podle dostupnosti pravidel definovaných příkazu for each.</span><span class="sxs-lookup"><span data-stu-id="2395b-121">To determine whether a particular statement or end point is reachable, the compiler performs flow analysis according to the reachability rules defined for each statement.</span></span> <span data-ttu-id="2395b-122">Analýzy toku bere v úvahu hodnoty konstantních výrazů ([konstantní výrazy](expressions.md#constant-expressions)), které řídí chování příkazy, ale nejsou považovány za možných hodnot, která není konstantní výrazy.</span><span class="sxs-lookup"><span data-stu-id="2395b-122">The flow analysis takes into account the values of constant expressions ([Constant expressions](expressions.md#constant-expressions)) that control the behavior of statements, but the possible values of non-constant expressions are not considered.</span></span> <span data-ttu-id="2395b-123">Jinými slovy pro účely analýzy toku řízení nekonstantní výraz daného typu se považuje za všechny možné hodnotu daného typu.</span><span class="sxs-lookup"><span data-stu-id="2395b-123">In other words, for purposes of control flow analysis, a non-constant expression of a given type is considered to have any possible value of that type.</span></span>

<span data-ttu-id="2395b-124">V příkladu</span><span class="sxs-lookup"><span data-stu-id="2395b-124">In the example</span></span>
```csharp
void F() {
    const int i = 1;
    if (i == 2) Console.WriteLine("unreachable");
}
```
<span data-ttu-id="2395b-125">logický výraz `if` příkazu je konstantní výraz, protože oba operandy `==` operátor jsou konstanty.</span><span class="sxs-lookup"><span data-stu-id="2395b-125">the boolean expression of the `if` statement is a constant expression because both operands of the `==` operator are constants.</span></span> <span data-ttu-id="2395b-126">Jako konstantní výraz je vyhodnocen v době kompilace, vytváření hodnota `false`, `Console.WriteLine` volání se považuje za nedostupný.</span><span class="sxs-lookup"><span data-stu-id="2395b-126">As the constant expression is evaluated at compile-time, producing the value `false`, the `Console.WriteLine` invocation is considered unreachable.</span></span> <span data-ttu-id="2395b-127">Nicméně pokud `i` se změní, aby byla místní proměnné</span><span class="sxs-lookup"><span data-stu-id="2395b-127">However, if `i` is changed to be a local variable</span></span>
```csharp
void F() {
    int i = 1;
    if (i == 2) Console.WriteLine("reachable");
}
```
<span data-ttu-id="2395b-128">`Console.WriteLine` volání se považuje za dostupný, i když ve skutečnosti, bude nikdy spuštěn.</span><span class="sxs-lookup"><span data-stu-id="2395b-128">the `Console.WriteLine` invocation is considered reachable, even though, in reality, it will never be executed.</span></span>

<span data-ttu-id="2395b-129">*Bloku* funkce člena je vždy považován za dostupný.</span><span class="sxs-lookup"><span data-stu-id="2395b-129">The *block* of a function member is always considered reachable.</span></span> <span data-ttu-id="2395b-130">Postupně po vyhodnocení pravidla dostupnosti každého příkazu v bloku, se dá určit dostupnosti jakékoli daný příkaz.</span><span class="sxs-lookup"><span data-stu-id="2395b-130">By successively evaluating the reachability rules of each statement in a block, the reachability of any given statement can be determined.</span></span>

<span data-ttu-id="2395b-131">V příkladu</span><span class="sxs-lookup"><span data-stu-id="2395b-131">In the example</span></span>
```csharp
void F(int x) {
    Console.WriteLine("start");
    if (x < 0) Console.WriteLine("negative");
}
```
<span data-ttu-id="2395b-132">připojení z druhé `Console.WriteLine` je stanoven následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="2395b-132">the reachability of the second `Console.WriteLine` is determined as follows:</span></span>

*  <span data-ttu-id="2395b-133">První `Console.WriteLine` příkaz výrazu je dostupný protože bloku `F` metoda je dostupný.</span><span class="sxs-lookup"><span data-stu-id="2395b-133">The first `Console.WriteLine` expression statement is reachable because the block of the `F` method is reachable.</span></span>
*  <span data-ttu-id="2395b-134">Koncový bod první `Console.WriteLine` příkaz výrazu je dostupný, protože tento příkaz je dostupný.</span><span class="sxs-lookup"><span data-stu-id="2395b-134">The end point of the first `Console.WriteLine` expression statement is reachable because that statement is reachable.</span></span>
*  <span data-ttu-id="2395b-135">`if` Příkaz je dostupný, protože koncový bod prvního `Console.WriteLine` příkaz výrazu je dostupný.</span><span class="sxs-lookup"><span data-stu-id="2395b-135">The `if` statement is reachable because the end point of the first `Console.WriteLine` expression statement is reachable.</span></span>
*  <span data-ttu-id="2395b-136">Druhá `Console.WriteLine` příkaz výrazu je dostupný protože logický výraz `if` příkaz nemá konstantní hodnotu `false`.</span><span class="sxs-lookup"><span data-stu-id="2395b-136">The second `Console.WriteLine` expression statement is reachable because the boolean expression of the `if` statement does not have the constant value `false`.</span></span>

<span data-ttu-id="2395b-137">Existují dvě situace, ve kterých je chyba kompilace pro koncový bod příkazu být dostupná pro:</span><span class="sxs-lookup"><span data-stu-id="2395b-137">There are two situations in which it is a compile-time error for the end point of a statement to be reachable:</span></span>

*  <span data-ttu-id="2395b-138">Vzhledem k tomu, `switch` příkaz neumožňuje části přepínače, aby "" k další části přepínače, je chyba kompilace pro koncový bod seznamu příkazů oddíl přepínače být dostupná.</span><span class="sxs-lookup"><span data-stu-id="2395b-138">Because the `switch` statement does not permit a switch section to "fall through" to the next switch section, it is a compile-time error for the end point of the statement list of a switch section to be reachable.</span></span> <span data-ttu-id="2395b-139">Pokud k této chybě dochází, je obvykle jako ukazatel toho, který `break` příkaz nebyl nalezen.</span><span class="sxs-lookup"><span data-stu-id="2395b-139">If this error occurs, it is typically an indication that a `break` statement is missing.</span></span>
*  <span data-ttu-id="2395b-140">Je chyba kompilace pro koncový bod bloku funkce člena, který vypočítá hodnotu být dostupná.</span><span class="sxs-lookup"><span data-stu-id="2395b-140">It is a compile-time error for the end point of the block of a function member that computes a value to be reachable.</span></span> <span data-ttu-id="2395b-141">Pokud k této chybě dochází, obvykle je údaj, který `return` příkaz nebyl nalezen.</span><span class="sxs-lookup"><span data-stu-id="2395b-141">If this error occurs, it typically is an indication that a `return` statement is missing.</span></span>

## <a name="blocks"></a><span data-ttu-id="2395b-142">Bloky</span><span class="sxs-lookup"><span data-stu-id="2395b-142">Blocks</span></span>

<span data-ttu-id="2395b-143">A *bloku* povoluje více příkazů, které má být zapsán v kontextech, kde je povoleno jeden příkaz.</span><span class="sxs-lookup"><span data-stu-id="2395b-143">A *block* permits multiple statements to be written in contexts where a single statement is allowed.</span></span>

```antlr
block
    : '{' statement_list? '}'
    ;
```

<span data-ttu-id="2395b-144">A *bloku* se skládá z volitelné *statement_list* ([příkaz uvádí](statements.md#statement-lists)), uzavřené ve složených závorkách.</span><span class="sxs-lookup"><span data-stu-id="2395b-144">A *block* consists of an optional *statement_list* ([Statement lists](statements.md#statement-lists)), enclosed in braces.</span></span> <span data-ttu-id="2395b-145">Pokud je vynechán seznam příkazů, se říká, že blok prázdný.</span><span class="sxs-lookup"><span data-stu-id="2395b-145">If the statement list is omitted, the block is said to be empty.</span></span>

<span data-ttu-id="2395b-146">Blok mohou obsahovat příkazy deklarace ([příkazy deklarace](statements.md#declaration-statements)).</span><span class="sxs-lookup"><span data-stu-id="2395b-146">A block may contain declaration statements ([Declaration statements](statements.md#declaration-statements)).</span></span> <span data-ttu-id="2395b-147">Obor lokální proměnné nebo konstantní deklarované v bloku je blok.</span><span class="sxs-lookup"><span data-stu-id="2395b-147">The scope of a local variable or constant declared in a block is the block.</span></span>

<span data-ttu-id="2395b-148">Blok provádí následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="2395b-148">A block is executed as follows:</span></span>

*  <span data-ttu-id="2395b-149">Pokud blok je prázdný, ovládací prvek bude převeden na koncový bod bloku.</span><span class="sxs-lookup"><span data-stu-id="2395b-149">If the block is empty, control is transferred to the end point of the block.</span></span>
*  <span data-ttu-id="2395b-150">Pokud blok není prázdná, ovládací prvek bude převeden do seznamu příkazů.</span><span class="sxs-lookup"><span data-stu-id="2395b-150">If the block is not empty, control is transferred to the statement list.</span></span> <span data-ttu-id="2395b-151">Když a ovládací prvek dosáhne koncového bodu seznam příkazů, ovládací prvek bude převeden na koncový bod bloku.</span><span class="sxs-lookup"><span data-stu-id="2395b-151">When and if control reaches the end point of the statement list, control is transferred to the end point of the block.</span></span>

<span data-ttu-id="2395b-152">Seznam příkazů bloku je dostupná, pokud je dostupný samotného bloku.</span><span class="sxs-lookup"><span data-stu-id="2395b-152">The statement list of a block is reachable if the block itself is reachable.</span></span>

<span data-ttu-id="2395b-153">Koncový bod bloku je dostupná, pokud blok je prázdný nebo pokud je dostupný koncový bod seznamu příkazů.</span><span class="sxs-lookup"><span data-stu-id="2395b-153">The end point of a block is reachable if the block is empty or if the end point of the statement list is reachable.</span></span>

<span data-ttu-id="2395b-154">A *bloku* , který obsahuje jeden nebo více `yield` příkazy ([příkaz yield](statements.md#the-yield-statement)) se nazývá blok iterátoru.</span><span class="sxs-lookup"><span data-stu-id="2395b-154">A *block* that contains one or more `yield` statements ([The yield statement](statements.md#the-yield-statement)) is called an iterator block.</span></span> <span data-ttu-id="2395b-155">Iterátor bloky se používají k implementaci funkce členy jako iterátory ([iterátory](classes.md#iterators)).</span><span class="sxs-lookup"><span data-stu-id="2395b-155">Iterator blocks are used to implement function members as iterators ([Iterators](classes.md#iterators)).</span></span> <span data-ttu-id="2395b-156">Některé další omezení se vztahují na iterátor bloků:</span><span class="sxs-lookup"><span data-stu-id="2395b-156">Some additional restrictions apply to iterator blocks:</span></span>

*  <span data-ttu-id="2395b-157">Je chyba kompilace pro `return` příkazu se zobrazí v blok iterátoru (ale `yield return` příkazy jsou povolené).</span><span class="sxs-lookup"><span data-stu-id="2395b-157">It is a compile-time error for a `return` statement to appear in an iterator block (but `yield return` statements are permitted).</span></span>
*  <span data-ttu-id="2395b-158">Je chyba kompilace pro blok iterátoru tak, aby obsahovala kontextu unsafe ([nezabezpečený kontext](unsafe-code.md#unsafe-contexts)).</span><span class="sxs-lookup"><span data-stu-id="2395b-158">It is a compile-time error for an iterator block to contain an unsafe context ([Unsafe contexts](unsafe-code.md#unsafe-contexts)).</span></span> <span data-ttu-id="2395b-159">Blok iterátoru vždy definuje bezpečné kontextu, i v případě, že jeho deklarace je vnořená v nezabezpečeném kontextu.</span><span class="sxs-lookup"><span data-stu-id="2395b-159">An iterator block always defines a safe context, even when its declaration is nested in an unsafe context.</span></span>

### <a name="statement-lists"></a><span data-ttu-id="2395b-160">Seznamy – příkaz</span><span class="sxs-lookup"><span data-stu-id="2395b-160">Statement lists</span></span>

<span data-ttu-id="2395b-161">A ***seznamu příkazů*** se skládá z jednoho nebo více příkazů, které jsou napsané v pořadí.</span><span class="sxs-lookup"><span data-stu-id="2395b-161">A ***statement list*** consists of one or more statements written in sequence.</span></span> <span data-ttu-id="2395b-162">Probíhá příkaz seznamy *bloku*s ([bloky](statements.md#blocks)) a v *switch_block*s ([příkazu switch](statements.md#the-switch-statement)).</span><span class="sxs-lookup"><span data-stu-id="2395b-162">Statement lists occur in *block*s ([Blocks](statements.md#blocks)) and in *switch_block*s ([The switch statement](statements.md#the-switch-statement)).</span></span>

```antlr
statement_list
    : statement+
    ;
```

<span data-ttu-id="2395b-163">Seznam příkazů provádí přenos řízení na první příkaz.</span><span class="sxs-lookup"><span data-stu-id="2395b-163">A statement list is executed by transferring control to the first statement.</span></span> <span data-ttu-id="2395b-164">Když a ovládací prvek dosáhne příkazu koncový bod, je kontrola předána dalšímu příkazu.</span><span class="sxs-lookup"><span data-stu-id="2395b-164">When and if control reaches the end point of a statement, control is transferred to the next statement.</span></span> <span data-ttu-id="2395b-165">Když a ovládací prvek dosáhne koncového bodu poslední příkaz, ovládací prvek bude převeden na koncový bod seznamu příkazů.</span><span class="sxs-lookup"><span data-stu-id="2395b-165">When and if control reaches the end point of the last statement, control is transferred to the end point of the statement list.</span></span>

<span data-ttu-id="2395b-166">Příkaz v seznamu příkazů je dostupný, pokud platí alespoň jedna z následujících akcí:</span><span class="sxs-lookup"><span data-stu-id="2395b-166">A statement in a statement list is reachable if at least one of the following is true:</span></span>

*  <span data-ttu-id="2395b-167">Příkaz je prvním příkazem a samotný seznam příkaz je dostupný.</span><span class="sxs-lookup"><span data-stu-id="2395b-167">The statement is the first statement and the statement list itself is reachable.</span></span>
*  <span data-ttu-id="2395b-168">Koncový bod předchozí příkaz je dostupný.</span><span class="sxs-lookup"><span data-stu-id="2395b-168">The end point of the preceding statement is reachable.</span></span>
*  <span data-ttu-id="2395b-169">Příkaz je příkaz s popiskem a popisek se odkazuje dostupné `goto` příkazu.</span><span class="sxs-lookup"><span data-stu-id="2395b-169">The statement is a labeled statement and the label is referenced by a reachable `goto` statement.</span></span>

<span data-ttu-id="2395b-170">Pokud je dostupný koncový bod poslední příkaz v seznamu je dostupný koncový bod seznamu příkazů.</span><span class="sxs-lookup"><span data-stu-id="2395b-170">The end point of a statement list is reachable if the end point of the last statement in the list is reachable.</span></span>

## <a name="the-empty-statement"></a><span data-ttu-id="2395b-171">Prázdný příkaz</span><span class="sxs-lookup"><span data-stu-id="2395b-171">The empty statement</span></span>

<span data-ttu-id="2395b-172">*Empty_statement* nemá žádný účinek.</span><span class="sxs-lookup"><span data-stu-id="2395b-172">An *empty_statement* does nothing.</span></span>

```antlr
empty_statement
    : ';'
    ;
```

<span data-ttu-id="2395b-173">Prázdný příkaz se používá, když nejsou žádná operace mají provést v kontextu, ve kterém jsou vyžadována příkaz.</span><span class="sxs-lookup"><span data-stu-id="2395b-173">An empty statement is used when there are no operations to perform in a context where a statement is required.</span></span>

<span data-ttu-id="2395b-174">Provádění prázdný příkaz jednoduše přenese ovládací prvek na konec příkazu.</span><span class="sxs-lookup"><span data-stu-id="2395b-174">Execution of an empty statement simply transfers control to the end point of the statement.</span></span> <span data-ttu-id="2395b-175">Koncový bod prázdný příkaz je tedy pokud prázdný příkaz je dostupný.</span><span class="sxs-lookup"><span data-stu-id="2395b-175">Thus, the end point of an empty statement is reachable if the empty statement is reachable.</span></span>

<span data-ttu-id="2395b-176">Prázdný příkaz může být použit při zápisu `while` příkaz s hodnotou null text:</span><span class="sxs-lookup"><span data-stu-id="2395b-176">An empty statement can be used when writing a `while` statement with a null body:</span></span>
```csharp
bool ProcessMessage() {...}

void ProcessMessages() {
    while (ProcessMessage())
        ;
}
```

<span data-ttu-id="2395b-177">Navíc prázdný příkaz je možné deklarovat popisek těsně před uzavírací "`}`" bloku:</span><span class="sxs-lookup"><span data-stu-id="2395b-177">Also, an empty statement can be used to declare a label just before the closing "`}`" of a block:</span></span>
```csharp
void F() {
    ...
    if (done) goto exit;
    ...
    exit: ;
}
```

## <a name="labeled-statements"></a><span data-ttu-id="2395b-178">Příkaz s popiskem</span><span class="sxs-lookup"><span data-stu-id="2395b-178">Labeled statements</span></span>

<span data-ttu-id="2395b-179">A *labeled_statement* povoluje příkazu k mít předponu popisek.</span><span class="sxs-lookup"><span data-stu-id="2395b-179">A *labeled_statement* permits a statement to be prefixed by a label.</span></span> <span data-ttu-id="2395b-180">Příkaz s popiskem jsou povolené v blocích, ale nejsou povolené jako vložené příkazy.</span><span class="sxs-lookup"><span data-stu-id="2395b-180">Labeled statements are permitted in blocks, but are not permitted as embedded statements.</span></span>

```antlr
labeled_statement
    : identifier ':' statement
    ;
```

<span data-ttu-id="2395b-181">Příkaz s popiskem deklaruje popisek s názvem dána *identifikátor*.</span><span class="sxs-lookup"><span data-stu-id="2395b-181">A labeled statement declares a label with the name given by the *identifier*.</span></span> <span data-ttu-id="2395b-182">Obor popisku je celý blok ve kterém je deklarována popisek, včetně všech vnořených bloků.</span><span class="sxs-lookup"><span data-stu-id="2395b-182">The scope of a label is the whole block in which the label is declared, including any nested blocks.</span></span> <span data-ttu-id="2395b-183">Je chyba kompilace pro dva popisky se stejným názvem, aby mají překrývající se rozsahy.</span><span class="sxs-lookup"><span data-stu-id="2395b-183">It is a compile-time error for two labels with the same name to have overlapping scopes.</span></span>

<span data-ttu-id="2395b-184">Popisek můžete odkazovat z `goto` příkazy ([příkazu goto](statements.md#the-goto-statement)) v rámci oboru popisku.</span><span class="sxs-lookup"><span data-stu-id="2395b-184">A label can be referenced from `goto` statements ([The goto statement](statements.md#the-goto-statement)) within the scope of the label.</span></span> <span data-ttu-id="2395b-185">To znamená, že `goto` příkazů může přenést řízení v rámci bloků a z bloků, ale nikdy do bloků.</span><span class="sxs-lookup"><span data-stu-id="2395b-185">This means that `goto` statements can transfer control within blocks and out of blocks, but never into blocks.</span></span>

<span data-ttu-id="2395b-186">Popisky své vlastní prohlášení místa a nejsou v konfliktu s další identifikátory.</span><span class="sxs-lookup"><span data-stu-id="2395b-186">Labels have their own declaration space and do not interfere with other identifiers.</span></span> <span data-ttu-id="2395b-187">V příkladu</span><span class="sxs-lookup"><span data-stu-id="2395b-187">The example</span></span>
```csharp
int F(int x) {
    if (x >= 0) goto x;
    x = -x;
    x: return x;
}
```
<span data-ttu-id="2395b-188">je platný a používá název `x` jako parametr a popisek.</span><span class="sxs-lookup"><span data-stu-id="2395b-188">is valid and uses the name `x` as both a parameter and a label.</span></span>

<span data-ttu-id="2395b-189">Spuštění příkaz s popiskem přesně odpovídá provádění příkazu za příkazem popisek.</span><span class="sxs-lookup"><span data-stu-id="2395b-189">Execution of a labeled statement corresponds exactly to execution of the statement following the label.</span></span>

<span data-ttu-id="2395b-190">Kromě dostupnosti poskytuje běžný tok řízení, je dostupná, pokud popisek se odkazuje dostupné příkaz s popiskem `goto` příkazu.</span><span class="sxs-lookup"><span data-stu-id="2395b-190">In addition to the reachability provided by normal flow of control, a labeled statement is reachable if the label is referenced by a reachable `goto` statement.</span></span> <span data-ttu-id="2395b-191">(Výjimka: Pokud `goto` příkazu se nachází uvnitř `try` , který obsahuje `finally` bloku a příkaz s popiskem je mimo `try`a koncový bod `finally` bloku nedostupný, pak příkaz s popiskem není dosažitelný z který `goto` příkazu.)</span><span class="sxs-lookup"><span data-stu-id="2395b-191">(Exception: If a `goto` statement is inside a `try` that includes a `finally` block, and the labeled statement is outside the `try`, and the end point of the `finally` block is unreachable, then the labeled statement is not reachable from that `goto` statement.)</span></span>

## <a name="declaration-statements"></a><span data-ttu-id="2395b-192">Příkazy deklarace</span><span class="sxs-lookup"><span data-stu-id="2395b-192">Declaration statements</span></span>

<span data-ttu-id="2395b-193">A *declaration_statement* deklaruje místní proměnnou nebo konstantu.</span><span class="sxs-lookup"><span data-stu-id="2395b-193">A *declaration_statement* declares a local variable or constant.</span></span> <span data-ttu-id="2395b-194">Příkazy deklarace jsou povolené v blocích, ale nejsou povolené jako vložené příkazy.</span><span class="sxs-lookup"><span data-stu-id="2395b-194">Declaration statements are permitted in blocks, but are not permitted as embedded statements.</span></span>

```antlr
declaration_statement
    : local_variable_declaration ';'
    | local_constant_declaration ';'
    ;
```

### <a name="local-variable-declarations"></a><span data-ttu-id="2395b-195">Místní deklarace proměnné</span><span class="sxs-lookup"><span data-stu-id="2395b-195">Local variable declarations</span></span>

<span data-ttu-id="2395b-196">A *local_variable_declaration* deklaruje jednoho nebo několika lokálními proměnnými.</span><span class="sxs-lookup"><span data-stu-id="2395b-196">A *local_variable_declaration* declares one or more local variables.</span></span>

```antlr
local_variable_declaration
    : local_variable_type local_variable_declarators
    ;

local_variable_type
    : type
    | 'var'
    ;

local_variable_declarators
    : local_variable_declarator
    | local_variable_declarators ',' local_variable_declarator
    ;

local_variable_declarator
    : identifier
    | identifier '=' local_variable_initializer
    ;

local_variable_initializer
    : expression
    | array_initializer
    | local_variable_initializer_unsafe
    ;
```

<span data-ttu-id="2395b-197">*Local_variable_type* z *local_variable_declaration* přímo určuje typ proměnné zavedeným deklarací, nebo označuje s identifikátorem `var` , který typ by měl odvodit podle inicializátor.</span><span class="sxs-lookup"><span data-stu-id="2395b-197">The *local_variable_type* of a *local_variable_declaration* either directly specifies the type of the variables introduced by the declaration, or indicates with the identifier `var` that the type should be inferred based on an initializer.</span></span> <span data-ttu-id="2395b-198">Typ následuje seznam *local_variable_declarator*s, z nichž každý představuje novou proměnnou.</span><span class="sxs-lookup"><span data-stu-id="2395b-198">The type is followed by a list of *local_variable_declarator*s, each of which introduces a new variable.</span></span> <span data-ttu-id="2395b-199">A *local_variable_declarator* se skládá ze *identifikátor* , název proměnné, může volitelně následovat "`=`" token a *local_variable_initializer* , která obsahuje počáteční hodnotu proměnné.</span><span class="sxs-lookup"><span data-stu-id="2395b-199">A *local_variable_declarator* consists of an *identifier* that names the variable, optionally followed by an "`=`" token and a *local_variable_initializer* that gives the initial value of the variable.</span></span>

<span data-ttu-id="2395b-200">V rámci deklarace lokální proměnné, var identifikátor funguje jako kontextové klíčové slovo ([klíčová slova](lexical-structure.md#keywords)). Když *local_variable_type* je zadán jako `var` a žádný typ s názvem `var` je v oboru, je deklarace ***implicitně typované lokální proměnné deklarace***, jehož typ je odvodit z typu přidružené inicializačního výrazu.</span><span class="sxs-lookup"><span data-stu-id="2395b-200">In the context of a local variable declaration, the identifier var acts as a contextual keyword ([Keywords](lexical-structure.md#keywords)).When the *local_variable_type* is specified as `var` and no type named `var` is in scope, the declaration is an ***implicitly typed local variable declaration***, whose type is inferred from the type of the associated initializer expression.</span></span> <span data-ttu-id="2395b-201">Implicitně typované lokální deklarace proměnných, které se vztahují následující omezení:</span><span class="sxs-lookup"><span data-stu-id="2395b-201">Implicitly typed local variable declarations are subject to the following restrictions:</span></span>

*  <span data-ttu-id="2395b-202">*Local_variable_declaration* nesmí obsahovat více *local_variable_declarator*s.</span><span class="sxs-lookup"><span data-stu-id="2395b-202">The *local_variable_declaration* cannot include multiple *local_variable_declarator*s.</span></span>
*  <span data-ttu-id="2395b-203">*Local_variable_declarator* musí obsahovat *local_variable_initializer*.</span><span class="sxs-lookup"><span data-stu-id="2395b-203">The *local_variable_declarator* must include a *local_variable_initializer*.</span></span>
*  <span data-ttu-id="2395b-204">*Local_variable_initializer* musí být *výraz*.</span><span class="sxs-lookup"><span data-stu-id="2395b-204">The *local_variable_initializer* must be an *expression*.</span></span>
*  <span data-ttu-id="2395b-205">Inicializátor *výraz* musí mít typ za kompilace.</span><span class="sxs-lookup"><span data-stu-id="2395b-205">The initializer *expression* must have a compile-time type.</span></span>
*  <span data-ttu-id="2395b-206">Inicializátor *výraz* nemůže odkazovat na proměnné deklarované samotný</span><span class="sxs-lookup"><span data-stu-id="2395b-206">The initializer *expression* cannot refer to the declared variable itself</span></span>

<span data-ttu-id="2395b-207">Následují příklady nesprávné implicitně typované lokální deklarace proměnných:</span><span class="sxs-lookup"><span data-stu-id="2395b-207">The following are examples of incorrect implicitly typed local variable declarations:</span></span>

```csharp
var x;               // Error, no initializer to infer type from
var y = {1, 2, 3};   // Error, array initializer not permitted
var z = null;        // Error, null does not have a type
var u = x => x + 1;  // Error, anonymous functions do not have a type
var v = v++;         // Error, initializer cannot refer to variable itself
```

<span data-ttu-id="2395b-208">V pomocí výrazu se získá hodnota místní proměnné *simple_name* ([jednoduché názvy](expressions.md#simple-names)), a hodnotu místní proměnné je upravit pomocí *přiřazení* () [Operátory přiřazení](expressions.md#assignment-operators)).</span><span class="sxs-lookup"><span data-stu-id="2395b-208">The value of a local variable is obtained in an expression using a *simple_name* ([Simple names](expressions.md#simple-names)), and the value of a local variable is modified using an *assignment* ([Assignment operators](expressions.md#assignment-operators)).</span></span> <span data-ttu-id="2395b-209">Lokální proměnná musí být jednoznačně přiřazena ([jednoznačného přiřazení](variables.md#definite-assignment)) v každém umístění, kde získat jeho hodnotu.</span><span class="sxs-lookup"><span data-stu-id="2395b-209">A local variable must be definitely assigned ([Definite assignment](variables.md#definite-assignment)) at each location where its value is obtained.</span></span>

<span data-ttu-id="2395b-210">Lokální proměnná deklarovaná v rozsahu *local_variable_declaration* je blok ve kterém dochází k deklaraci.</span><span class="sxs-lookup"><span data-stu-id="2395b-210">The scope of a local variable declared in a *local_variable_declaration* is the block in which the declaration occurs.</span></span> <span data-ttu-id="2395b-211">Jedná se o chybu k odkazování na místní proměnnou v textové pozici, která předchází *local_variable_declarator* lokální proměnné.</span><span class="sxs-lookup"><span data-stu-id="2395b-211">It is an error to refer to a local variable in a textual position that precedes the *local_variable_declarator* of the local variable.</span></span> <span data-ttu-id="2395b-212">V rámci oboru místní proměnná je chyba kompilace, chcete-li deklarovat jinou místní proměnné nebo konstantní se stejným názvem.</span><span class="sxs-lookup"><span data-stu-id="2395b-212">Within the scope of a local variable, it is a compile-time error to declare another local variable or constant with the same name.</span></span>

<span data-ttu-id="2395b-213">Je ekvivalentní více deklarací jedné proměnné se stejným typem deklarace lokální proměnné, která deklaruje několik proměnných.</span><span class="sxs-lookup"><span data-stu-id="2395b-213">A local variable declaration that declares multiple variables is equivalent to multiple declarations of single variables with the same type.</span></span> <span data-ttu-id="2395b-214">Kromě toho inicializátoru proměnné v deklaraci lokální proměnné přesně odpovídá přiřazovací příkaz, který je vložen hned za deklaraci.</span><span class="sxs-lookup"><span data-stu-id="2395b-214">Furthermore, a variable initializer in a local variable declaration corresponds exactly to an assignment statement that is inserted immediately after the declaration.</span></span>

<span data-ttu-id="2395b-215">V příkladu</span><span class="sxs-lookup"><span data-stu-id="2395b-215">The example</span></span>
```csharp
void F() {
    int x = 1, y, z = x * 2;
}
```
<span data-ttu-id="2395b-216">přesně odpovídá</span><span class="sxs-lookup"><span data-stu-id="2395b-216">corresponds exactly to</span></span>
```csharp
void F() {
    int x; x = 1;
    int y;
    int z; z = x * 2;
}
```

<span data-ttu-id="2395b-217">V implicitně typované lokální proměnné deklarace typ místní proměnné deklarované se používá stejný jako typ výrazu použitý k inicializaci proměnné.</span><span class="sxs-lookup"><span data-stu-id="2395b-217">In an implicitly typed local variable declaration, the type of the local variable being declared is taken to be the same as the type of the expression used to initialize the variable.</span></span> <span data-ttu-id="2395b-218">Příklad:</span><span class="sxs-lookup"><span data-stu-id="2395b-218">For example:</span></span>
```csharp
var i = 5;
var s = "Hello";
var d = 1.0;
var numbers = new int[] {1, 2, 3};
var orders = new Dictionary<int,Order>();
```

<span data-ttu-id="2395b-219">Implicitně typovaná místní proměnné prohlášení výše jsou přesně odpovídá explicitně následující deklarace:</span><span class="sxs-lookup"><span data-stu-id="2395b-219">The implicitly typed local variable declarations above are precisely equivalent to the following explicitly typed declarations:</span></span>
```csharp
int i = 5;
string s = "Hello";
double d = 1.0;
int[] numbers = new int[] {1, 2, 3};
Dictionary<int,Order> orders = new Dictionary<int,Order>();
```

### <a name="local-constant-declarations"></a><span data-ttu-id="2395b-220">Místní deklarace konstanty</span><span class="sxs-lookup"><span data-stu-id="2395b-220">Local constant declarations</span></span>

<span data-ttu-id="2395b-221">A *local_constant_declaration* deklaruje nejmíň jeden místní konstanty.</span><span class="sxs-lookup"><span data-stu-id="2395b-221">A *local_constant_declaration* declares one or more local constants.</span></span>

```antlr
local_constant_declaration
    : 'const' type constant_declarators
    ;

constant_declarators
    : constant_declarator (',' constant_declarator)*
    ;

constant_declarator
    : identifier '=' constant_expression
    ;
```

<span data-ttu-id="2395b-222">*Typ* z *local_constant_declaration* Určuje typ konstanty zavedeným deklarací.</span><span class="sxs-lookup"><span data-stu-id="2395b-222">The *type* of a *local_constant_declaration* specifies the type of the constants introduced by the declaration.</span></span> <span data-ttu-id="2395b-223">Typ následuje seznam *constant_declarator*s, z nichž každý představuje nové konstantou.</span><span class="sxs-lookup"><span data-stu-id="2395b-223">The type is followed by a list of *constant_declarator*s, each of which introduces a new constant.</span></span> <span data-ttu-id="2395b-224">A *constant_declarator* se skládá ze *identifikátor* této názvy – konstanta, za nímž následuje "`=`" token a po něm *constant_expression* ([ Výrazy konstant](expressions.md#constant-expressions)), který vrací hodnotu konstanty.</span><span class="sxs-lookup"><span data-stu-id="2395b-224">A *constant_declarator* consists of an *identifier* that names the constant, followed by an "`=`" token, followed by a *constant_expression* ([Constant expressions](expressions.md#constant-expressions)) that gives the value of the constant.</span></span>

<span data-ttu-id="2395b-225">*Typ* a *constant_expression* místní deklarace konstanty musí řídit stejnými pravidly jako deklarace konstantní členské ([konstanty](classes.md#constants)).</span><span class="sxs-lookup"><span data-stu-id="2395b-225">The *type* and *constant_expression* of a local constant declaration must follow the same rules as those of a constant member declaration ([Constants](classes.md#constants)).</span></span>

<span data-ttu-id="2395b-226">Hodnota lokální konstanta je získanou v pomocí výrazu *simple_name* ([jednoduché názvy](expressions.md#simple-names)).</span><span class="sxs-lookup"><span data-stu-id="2395b-226">The value of a local constant is obtained in an expression using a *simple_name* ([Simple names](expressions.md#simple-names)).</span></span>

<span data-ttu-id="2395b-227">Obor lokální konstanta je blok ve kterém dochází k deklaraci.</span><span class="sxs-lookup"><span data-stu-id="2395b-227">The scope of a local constant is the block in which the declaration occurs.</span></span> <span data-ttu-id="2395b-228">Jedná se o chybu k odkazování na lokální konstanta v textové pozici, která předchází jeho *constant_declarator*.</span><span class="sxs-lookup"><span data-stu-id="2395b-228">It is an error to refer to a local constant in a textual position that precedes its *constant_declarator*.</span></span> <span data-ttu-id="2395b-229">V rámci oboru místní konstantu je chyba kompilace, chcete-li deklarovat jinou místní proměnné nebo konstantní se stejným názvem.</span><span class="sxs-lookup"><span data-stu-id="2395b-229">Within the scope of a local constant, it is a compile-time error to declare another local variable or constant with the same name.</span></span>

<span data-ttu-id="2395b-230">Místní deklarace konstanty, který deklaruje více konstant je ekvivalentní více deklarací jedné konstanty se stejným typem.</span><span class="sxs-lookup"><span data-stu-id="2395b-230">A local constant declaration that declares multiple constants is equivalent to multiple declarations of single constants with the same type.</span></span>

## <a name="expression-statements"></a><span data-ttu-id="2395b-231">Příkazy výrazů</span><span class="sxs-lookup"><span data-stu-id="2395b-231">Expression statements</span></span>

<span data-ttu-id="2395b-232">*Expression_statement* vyhodnotí zadaný výraz.</span><span class="sxs-lookup"><span data-stu-id="2395b-232">An *expression_statement* evaluates a given expression.</span></span> <span data-ttu-id="2395b-233">Hodnota vypočítaná aplikací výrazu, pokud existuje, se zahodí.</span><span class="sxs-lookup"><span data-stu-id="2395b-233">The value computed by the expression, if any, is discarded.</span></span>

```antlr
expression_statement
    : statement_expression ';'
    ;

statement_expression
    : invocation_expression
    | null_conditional_invocation_expression
    | object_creation_expression
    | assignment
    | post_increment_expression
    | post_decrement_expression
    | pre_increment_expression
    | pre_decrement_expression
    | await_expression
    ;
```

<span data-ttu-id="2395b-234">Ne všechny výrazy nejsou povoleny jako příkazy.</span><span class="sxs-lookup"><span data-stu-id="2395b-234">Not all expressions are permitted as statements.</span></span> <span data-ttu-id="2395b-235">Ve výrazech konkrétní, jako například `x + y` a `x == 1` , který pouze výpočtu hodnoty (což se zahodí), nejsou povolené jako příkazy.</span><span class="sxs-lookup"><span data-stu-id="2395b-235">In particular, expressions such as `x + y` and `x == 1` that merely compute a value (which will be discarded), are not permitted as statements.</span></span>

<span data-ttu-id="2395b-236">Spuštění *expression_statement* vyhodnotí výraz v omezením a pak přenese ovládací prvek na koncový bod *expression_statement*.</span><span class="sxs-lookup"><span data-stu-id="2395b-236">Execution of an *expression_statement* evaluates the contained expression and then transfers control to the end point of the *expression_statement*.</span></span> <span data-ttu-id="2395b-237">Koncový bod *expression_statement* dostupný, pokud to *expression_statement* je dostupný.</span><span class="sxs-lookup"><span data-stu-id="2395b-237">The end point of an *expression_statement* is reachable if that *expression_statement* is reachable.</span></span>

## <a name="selection-statements"></a><span data-ttu-id="2395b-238">Příkazy výběru</span><span class="sxs-lookup"><span data-stu-id="2395b-238">Selection statements</span></span>

<span data-ttu-id="2395b-239">Příkazy výběru vyberte jedním z možných příkazů pro spuštění na základě hodnoty některých výrazu.</span><span class="sxs-lookup"><span data-stu-id="2395b-239">Selection statements select one of a number of possible statements for execution based on the value of some expression.</span></span>

```antlr
selection_statement
    : if_statement
    | switch_statement
    ;
```

### <a name="the-if-statement"></a><span data-ttu-id="2395b-240">If – příkaz</span><span class="sxs-lookup"><span data-stu-id="2395b-240">The if statement</span></span>

<span data-ttu-id="2395b-241">`if` Příkaz vybere příkaz pro spuštění na základě hodnoty logického výrazu.</span><span class="sxs-lookup"><span data-stu-id="2395b-241">The `if` statement selects a statement for execution based on the value of a boolean expression.</span></span>

```antlr
if_statement
    : 'if' '(' boolean_expression ')' embedded_statement
    | 'if' '(' boolean_expression ')' embedded_statement 'else' embedded_statement
    ;
```

<span data-ttu-id="2395b-242">`else` Část souvisí s předchozím lexikálně nejbližší `if` , který je povolen pomocí syntaxe.</span><span class="sxs-lookup"><span data-stu-id="2395b-242">An `else` part is associated with the lexically nearest preceding `if` that is allowed by the syntax.</span></span> <span data-ttu-id="2395b-243">Proto `if` příkaz formuláře</span><span class="sxs-lookup"><span data-stu-id="2395b-243">Thus, an `if` statement of the form</span></span>
```csharp
if (x) if (y) F(); else G();
```
<span data-ttu-id="2395b-244">je ekvivalentem</span><span class="sxs-lookup"><span data-stu-id="2395b-244">is equivalent to</span></span>
```csharp
if (x) {
    if (y) {
        F();
    }
    else {
        G();
    }
}
```

<span data-ttu-id="2395b-245">`if` Je proveden příkaz takto:</span><span class="sxs-lookup"><span data-stu-id="2395b-245">An `if` statement is executed as follows:</span></span>

*  <span data-ttu-id="2395b-246">*Boolean_expression* ([logické výrazy](expressions.md#boolean-expressions)) je vyhodnocen.</span><span class="sxs-lookup"><span data-stu-id="2395b-246">The *boolean_expression* ([Boolean expressions](expressions.md#boolean-expressions)) is evaluated.</span></span>
*  <span data-ttu-id="2395b-247">Je-li logický výraz vrací `true`, ovládací prvek bude převeden na první příkaz vložený.</span><span class="sxs-lookup"><span data-stu-id="2395b-247">If the boolean expression yields `true`, control is transferred to the first embedded statement.</span></span> <span data-ttu-id="2395b-248">Když, pokud ovládací prvek dosáhne koncového bodu, který tento příkaz Ovládací prvek bude převeden na koncový bod `if` příkazu.</span><span class="sxs-lookup"><span data-stu-id="2395b-248">When and if control reaches the end point of that statement, control is transferred to the end point of the `if` statement.</span></span>
*  <span data-ttu-id="2395b-249">Je-li logický výraz vrací `false` a pokud `else` část je k dispozici, ovládací prvek bude převeden na druhý vloženým příkazem.</span><span class="sxs-lookup"><span data-stu-id="2395b-249">If the boolean expression yields `false` and if an `else` part is present, control is transferred to the second embedded statement.</span></span> <span data-ttu-id="2395b-250">Když, pokud ovládací prvek dosáhne koncového bodu, který tento příkaz Ovládací prvek bude převeden na koncový bod `if` příkazu.</span><span class="sxs-lookup"><span data-stu-id="2395b-250">When and if control reaches the end point of that statement, control is transferred to the end point of the `if` statement.</span></span>
*  <span data-ttu-id="2395b-251">Je-li logický výraz vrací `false` a pokud `else` část není k dispozici, ovládací prvek bude převeden na koncový bod `if` příkaz.</span><span class="sxs-lookup"><span data-stu-id="2395b-251">If the boolean expression yields `false` and if an `else` part is not present, control is transferred to the end point of the `if` statement.</span></span>

<span data-ttu-id="2395b-252">Vložené první příkaz `if` příkaz je dostupný Pokud `if` příkaz je dostupný a logický výraz nemá konstantní hodnotu `false`.</span><span class="sxs-lookup"><span data-stu-id="2395b-252">The first embedded statement of an `if` statement is reachable if the `if` statement is reachable and the boolean expression does not have the constant value `false`.</span></span>

<span data-ttu-id="2395b-253">Druhá vložené prohlášení o `if` příkazu, pokud jsou k dispozici, je dostupný Pokud `if` příkaz je dostupný a logický výraz nemá konstantní hodnotu `true`.</span><span class="sxs-lookup"><span data-stu-id="2395b-253">The second embedded statement of an `if` statement, if present, is reachable if the `if` statement is reachable and the boolean expression does not have the constant value `true`.</span></span>

<span data-ttu-id="2395b-254">Koncový bod `if` příkaz je dostupný, pokud je dostupný koncový bod alespoň jeden z jeho vložených příkazů.</span><span class="sxs-lookup"><span data-stu-id="2395b-254">The end point of an `if` statement is reachable if the end point of at least one of its embedded statements is reachable.</span></span> <span data-ttu-id="2395b-255">Kromě toho koncový bod `if` příkaz bez `else` část je dostupný Pokud `if` příkaz je dostupný a logický výraz nemá konstantní hodnotu `true`.</span><span class="sxs-lookup"><span data-stu-id="2395b-255">In addition, the end point of an `if` statement with no `else` part is reachable if the `if` statement is reachable and the boolean expression does not have the constant value `true`.</span></span>

### <a name="the-switch-statement"></a><span data-ttu-id="2395b-256">Příkaz switch</span><span class="sxs-lookup"><span data-stu-id="2395b-256">The switch statement</span></span>

<span data-ttu-id="2395b-257">Příkaz switch vybere pro spuštění seznamu příkazů s popisek přidružený přepínač, který odpovídá hodnotě výraz přepínače.</span><span class="sxs-lookup"><span data-stu-id="2395b-257">The switch statement selects for execution a statement list having an associated switch label that corresponds to the value of the switch expression.</span></span>

```antlr
switch_statement
    : 'switch' '(' expression ')' switch_block
    ;

switch_block
    : '{' switch_section* '}'
    ;

switch_section
    : switch_label+ statement_list
    ;

switch_label
    : 'case' constant_expression ':'
    | 'default' ':'
    ;
```

<span data-ttu-id="2395b-258">A *switch_statement* se skládá z klíčového slova `switch`, za nímž následuje výrazu v závorkách (označované jako výraz přepínače), za nímž následuje *switch_block*.</span><span class="sxs-lookup"><span data-stu-id="2395b-258">A *switch_statement* consists of the keyword `switch`, followed by a parenthesized expression (called the switch expression), followed by a *switch_block*.</span></span> <span data-ttu-id="2395b-259">*Switch_block* se skládá z nula nebo více *switch_section*s uzavřeny ve složených závorkách.</span><span class="sxs-lookup"><span data-stu-id="2395b-259">The *switch_block* consists of zero or more *switch_section*s, enclosed in braces.</span></span> <span data-ttu-id="2395b-260">Každý *switch_section* obsahuje jeden nebo více *switch_label*s za nímž následuje *statement_list* ([příkaz uvádí](statements.md#statement-lists)).</span><span class="sxs-lookup"><span data-stu-id="2395b-260">Each *switch_section* consists of one or more *switch_label*s followed by a *statement_list* ([Statement lists](statements.md#statement-lists)).</span></span>

<span data-ttu-id="2395b-261">***Řídící typ*** z `switch` příkaz pokládáme stav, výrazem přepnutí.</span><span class="sxs-lookup"><span data-stu-id="2395b-261">The ***governing type*** of a `switch` statement is established by the switch expression.</span></span>

*  <span data-ttu-id="2395b-262">Pokud je typ výrazu přepínače `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `bool`, `char`, `string`, nebo *enum_type*, nebo pokud je typ připouštějící hodnotu Null odpovídá jedné z těchto typů, pak je správní typ `switch` příkazu.</span><span class="sxs-lookup"><span data-stu-id="2395b-262">If the type of the switch expression is `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `bool`, `char`, `string`, or an *enum_type*, or if it is the nullable type corresponding to one of these types, then that is the governing type of the `switch` statement.</span></span>
*  <span data-ttu-id="2395b-263">V opačném případě právě jeden uživatelský implicitní převod ([uživatelem definované převody](conversions.md#user-defined-conversions)) z typu výrazu přepínače, musí existovat na jednu z následujících možných řídící typy: `sbyte`, `byte`, `short` , `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `string`, nebo typ připouštějící hodnotu Null odpovídá jedné z těchto typů.</span><span class="sxs-lookup"><span data-stu-id="2395b-263">Otherwise, exactly one user-defined implicit conversion ([User-defined conversions](conversions.md#user-defined-conversions)) must exist from the type of the switch expression to one of the following possible governing types: `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `string`, or,  a nullable type corresponding to one of those types.</span></span>
*  <span data-ttu-id="2395b-264">Jinak pokud neexistuje žádný implicitní převod, nebo pokud více než jedna implicitní převod existuje, dojde k chybě kompilace.</span><span class="sxs-lookup"><span data-stu-id="2395b-264">Otherwise, if no such implicit conversion exists, or if more than one such implicit conversion exists, a compile-time error occurs.</span></span>

<span data-ttu-id="2395b-265">Konstantní výraz každé `case` popisku musí označují hodnotu, která implicitně převést ([implicitních převodů](conversions.md#implicit-conversions)) celopodnikové typu `switch` příkazu.</span><span class="sxs-lookup"><span data-stu-id="2395b-265">The constant expression of each `case` label must denote a value that is implicitly convertible ([Implicit conversions](conversions.md#implicit-conversions)) to the governing type of the `switch` statement.</span></span> <span data-ttu-id="2395b-266">Chyba kompilace nastane, pokud dva nebo více `case` popisky ve stejném `switch` příkazu zadejte stejnou hodnotu konstanty.</span><span class="sxs-lookup"><span data-stu-id="2395b-266">A compile-time error occurs if two or more `case` labels in the same `switch` statement specify the same constant value.</span></span>

<span data-ttu-id="2395b-267">Může existovat maximálně jeden `default` popisků v příkazu switch.</span><span class="sxs-lookup"><span data-stu-id="2395b-267">There can be at most one `default` label in a switch statement.</span></span>

<span data-ttu-id="2395b-268">A `switch` je proveden příkaz takto:</span><span class="sxs-lookup"><span data-stu-id="2395b-268">A `switch` statement is executed as follows:</span></span>

*  <span data-ttu-id="2395b-269">Výraz přepínače je vyhodnotit a převedeny na typ řízení.</span><span class="sxs-lookup"><span data-stu-id="2395b-269">The switch expression is evaluated and converted to the governing type.</span></span>
*  <span data-ttu-id="2395b-270">Pokud některou z konstant podle `case` popisek ve stejném `switch` příkaz je rovna hodnotě výraz přepínače, ovládací prvek bude převeden do seznamu příkazů podle odpovídajícího `case` popisek.</span><span class="sxs-lookup"><span data-stu-id="2395b-270">If one of the constants specified in a `case` label in the same `switch` statement is equal to the value of the switch expression, control is transferred to the statement list following the matched `case` label.</span></span>
*  <span data-ttu-id="2395b-271">Pokud žádná konstanta zadané v `case` popisky ve stejném `switch` příkaz je rovna hodnotě výraz přepínače a pokud `default` popisek je k dispozici, ovládací prvek bude převeden na příkaz následující seznam `default` Popisek.</span><span class="sxs-lookup"><span data-stu-id="2395b-271">If none of the constants specified in `case` labels in the same `switch` statement is equal to the value of the switch expression, and if a `default` label is present, control is transferred to the statement list following the `default` label.</span></span>
*  <span data-ttu-id="2395b-272">Pokud žádná konstanta podle `case` popisky ve stejném `switch` příkaz je rovna hodnotě výraz přepínače a pokud ne `default` popisek je k dispozici, ovládací prvek bude převeden na koncový bod `switch` příkaz.</span><span class="sxs-lookup"><span data-stu-id="2395b-272">If none of the constants specified in `case` labels in the same `switch` statement is equal to the value of the switch expression, and if no `default` label is present, control is transferred to the end point of the `switch` statement.</span></span>

<span data-ttu-id="2395b-273">Pokud je dostupný koncový bod seznamu příkazů části přepínače, dojde k chybě kompilace.</span><span class="sxs-lookup"><span data-stu-id="2395b-273">If the end point of the statement list of a switch section is reachable, a compile-time error occurs.</span></span> <span data-ttu-id="2395b-274">To se označuje jako "žádné fall prostřednictvím" pravidlo.</span><span class="sxs-lookup"><span data-stu-id="2395b-274">This is known as the "no fall through" rule.</span></span> <span data-ttu-id="2395b-275">V příkladu</span><span class="sxs-lookup"><span data-stu-id="2395b-275">The example</span></span>
```csharp
switch (i) {
case 0:
    CaseZero();
    break;
case 1:
    CaseOne();
    break;
default:
    CaseOthers();
    break;
}
```
<span data-ttu-id="2395b-276">je platný, protože žádný oddíl přepínače má dosažitelný koncový bod.</span><span class="sxs-lookup"><span data-stu-id="2395b-276">is valid because no switch section has a reachable end point.</span></span> <span data-ttu-id="2395b-277">Na rozdíl od jazyka C a C++ spouštěcí oddíl přepínače není dovoleno "předat" k další části přepínače a v příkladu</span><span class="sxs-lookup"><span data-stu-id="2395b-277">Unlike C and C++, execution of a switch section is not permitted to "fall through" to the next switch section, and the example</span></span>
```csharp
switch (i) {
case 0:
    CaseZero();
case 1:
    CaseZeroOrOne();
default:
    CaseAny();
}
```
<span data-ttu-id="2395b-278">výsledkem chyba kompilace.</span><span class="sxs-lookup"><span data-stu-id="2395b-278">results in a compile-time error.</span></span> <span data-ttu-id="2395b-279">Při provádění oddíl přepínače má následovat provádění jiné části přepínače, explicitní `goto case` nebo `goto default` musíte použít příkaz:</span><span class="sxs-lookup"><span data-stu-id="2395b-279">When execution of a switch section is to be followed by execution of another switch section, an explicit `goto case` or `goto default` statement must be used:</span></span>
```csharp
switch (i) {
case 0:
    CaseZero();
    goto case 1;
case 1:
    CaseZeroOrOne();
    goto default;
default:
    CaseAny();
    break;
}
```

<span data-ttu-id="2395b-280">Více popisky jsou povoleny v *switch_section*.</span><span class="sxs-lookup"><span data-stu-id="2395b-280">Multiple labels are permitted in a *switch_section*.</span></span> <span data-ttu-id="2395b-281">V příkladu</span><span class="sxs-lookup"><span data-stu-id="2395b-281">The example</span></span>
```csharp
switch (i) {
case 0:
    CaseZero();
    break;
case 1:
    CaseOne();
    break;
case 2:
default:
    CaseTwo();
    break;
}
```
<span data-ttu-id="2395b-282">je platný.</span><span class="sxs-lookup"><span data-stu-id="2395b-282">is valid.</span></span> <span data-ttu-id="2395b-283">V příkladu nebudou porušovat pravidla "žádné fall prostřednictvím" protože popisky `case 2:` a `default:` jsou součástí stejného *switch_section*.</span><span class="sxs-lookup"><span data-stu-id="2395b-283">The example does not violate the "no fall through" rule because the labels `case 2:` and `default:` are part of the same *switch_section*.</span></span>

<span data-ttu-id="2395b-284">Pravidlo "žádné fall prostřednictvím" Zabrání běžné třídy chyb, ke kterým dochází v jazyce C a C++ při `break` příkazy jsou vynechány omylem.</span><span class="sxs-lookup"><span data-stu-id="2395b-284">The "no fall through" rule prevents a common class of bugs that occur in C and C++ when `break` statements are accidentally omitted.</span></span> <span data-ttu-id="2395b-285">Kromě toho kvůli tomuto pravidlu oddílů přepínače `switch` příkaz může být libovolně změnit jejich uspořádání aniž by to ovlivnilo chování příkazu.</span><span class="sxs-lookup"><span data-stu-id="2395b-285">In addition, because of this rule, the switch sections of a `switch` statement can be arbitrarily rearranged without affecting the behavior of the statement.</span></span> <span data-ttu-id="2395b-286">Například oddíly `switch` výše uvedeného příkazu je možné vrátit zpět, aniž by to ovlivnilo chování příkazu:</span><span class="sxs-lookup"><span data-stu-id="2395b-286">For example, the sections of the `switch` statement above can be reversed without affecting the behavior of the statement:</span></span>
```csharp
switch (i) {
default:
    CaseAny();
    break;
case 1:
    CaseZeroOrOne();
    goto default;
case 0:
    CaseZero();
    goto case 1;
}
```

<span data-ttu-id="2395b-287">Seznam příkazů oddíl přepínače se obvykle ukončí v `break`, `goto case`, nebo `goto default` je povolen příkaz, ale žádné konstrukci, která vykreslí koncový bod seznamu příkazů nedostupný.</span><span class="sxs-lookup"><span data-stu-id="2395b-287">The statement list of a switch section typically ends in a `break`, `goto case`, or `goto default` statement, but any construct that renders the end point of the statement list unreachable is permitted.</span></span> <span data-ttu-id="2395b-288">Například `while` příkaz řídí logický výraz `true` známé nikdy dosah její koncový bod.</span><span class="sxs-lookup"><span data-stu-id="2395b-288">For example, a `while` statement controlled by the boolean expression `true` is known to never reach its end point.</span></span> <span data-ttu-id="2395b-289">Podobně `throw` nebo `return` příkaz vždy přenese ovládací prvek jinde a nikdy dosáhne její koncový bod.</span><span class="sxs-lookup"><span data-stu-id="2395b-289">Likewise, a `throw` or `return` statement always transfers control elsewhere and never reaches its end point.</span></span> <span data-ttu-id="2395b-290">Proto je platný v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="2395b-290">Thus, the following example is valid:</span></span>
```csharp
switch (i) {
case 0:
    while (true) F();
case 1:
    throw new ArgumentException();
case 2:
    return;
}
```

<span data-ttu-id="2395b-291">Typ řízení `switch` příkaz může být typu `string`.</span><span class="sxs-lookup"><span data-stu-id="2395b-291">The governing type of a `switch` statement may be the type `string`.</span></span> <span data-ttu-id="2395b-292">Příklad:</span><span class="sxs-lookup"><span data-stu-id="2395b-292">For example:</span></span>
```csharp
void DoCommand(string command) {
    switch (command.ToLower()) {
    case "run":
        DoRun();
        break;
    case "save":
        DoSave();
        break;
    case "quit":
        DoQuit();
        break;
    default:
        InvalidCommand(command);
        break;
    }
}
```

<span data-ttu-id="2395b-293">Operátory rovnosti řetězce, jako jsou ([řetězec operátory rovnosti](expressions.md#string-equality-operators)), `switch` příkazu je velká a malá písmena a spustí daný přepínač části pouze v případě, že řetězec výraz přepínače se přesně shoduje `case` popisek Toto je konstanta.</span><span class="sxs-lookup"><span data-stu-id="2395b-293">Like the string equality operators ([String equality operators](expressions.md#string-equality-operators)), the `switch` statement is case sensitive and will execute a given switch section only if the switch expression string exactly matches a `case` label constant.</span></span>

<span data-ttu-id="2395b-294">Když správní typ `switch` příkaz je `string`, hodnota `null` smí obsahovat jako konstanta popisek případu.</span><span class="sxs-lookup"><span data-stu-id="2395b-294">When the governing type of a `switch` statement is `string`, the value `null` is permitted as a case label constant.</span></span>

<span data-ttu-id="2395b-295">*Statement_list*s *switch_block* mohou obsahovat příkazy deklarace ([příkazy deklarace](statements.md#declaration-statements)).</span><span class="sxs-lookup"><span data-stu-id="2395b-295">The *statement_list*s of a *switch_block* may contain declaration statements ([Declaration statements](statements.md#declaration-statements)).</span></span> <span data-ttu-id="2395b-296">Obor lokální proměnné nebo konstantní deklarované v bloku přepínačů je blok switch.</span><span class="sxs-lookup"><span data-stu-id="2395b-296">The scope of a local variable or constant declared in a switch block is the switch block.</span></span>

<span data-ttu-id="2395b-297">Seznam příkazů daný přepínač oddílu je dostupný Pokud `switch` příkaz je dostupný a platí alespoň jedna z následujících akcí:</span><span class="sxs-lookup"><span data-stu-id="2395b-297">The statement list of a given switch section is reachable if the `switch` statement is reachable and at least one of the following is true:</span></span>

*  <span data-ttu-id="2395b-298">Výraz přepnutí má hodnotu, která není konstantní.</span><span class="sxs-lookup"><span data-stu-id="2395b-298">The switch expression is a non-constant value.</span></span>
*  <span data-ttu-id="2395b-299">Výraz přepnutí má konstantní hodnotu, která odpovídá `case` popisek v části přepínače.</span><span class="sxs-lookup"><span data-stu-id="2395b-299">The switch expression is a constant value that matches a `case` label in the switch section.</span></span>
*  <span data-ttu-id="2395b-300">Výraz přepnutí má konstantní hodnotu, která neodpovídá žádnému `case` popisek a oddíl přepínače obsahuje `default` popisek.</span><span class="sxs-lookup"><span data-stu-id="2395b-300">The switch expression is a constant value that doesn't match any `case` label, and the switch section contains the `default` label.</span></span>
*  <span data-ttu-id="2395b-301">Popisek přepínače oddíl přepínače se odkazuje dostupné `goto case` nebo `goto default` příkazu.</span><span class="sxs-lookup"><span data-stu-id="2395b-301">A switch label of the switch section is referenced by a reachable `goto case` or `goto default` statement.</span></span>

<span data-ttu-id="2395b-302">Koncový bod `switch` příkaz je dostupný, pokud platí alespoň jedna z následujících akcí:</span><span class="sxs-lookup"><span data-stu-id="2395b-302">The end point of a `switch` statement is reachable if at least one of the following is true:</span></span>

*  <span data-ttu-id="2395b-303">`switch` Obsahuje prohlášení dostupný `break` příkaz, který ukončí `switch` příkazu.</span><span class="sxs-lookup"><span data-stu-id="2395b-303">The `switch` statement contains a reachable `break` statement that exits the `switch` statement.</span></span>
*  <span data-ttu-id="2395b-304">`switch` Příkaz je dostupný, výraz přepnutí má hodnotu, která není konstantní a ne `default` popisek je k dispozici.</span><span class="sxs-lookup"><span data-stu-id="2395b-304">The `switch` statement is reachable, the switch expression is a non-constant value, and no `default` label is present.</span></span>
*  <span data-ttu-id="2395b-305">`switch` Příkaz je dostupný, výraz přepnutí má konstantní hodnotu, která neodpovídá žádnému `case` popisek a ne `default` popisek je k dispozici.</span><span class="sxs-lookup"><span data-stu-id="2395b-305">The `switch` statement is reachable, the switch expression is a constant value that doesn't match any `case` label, and no `default` label is present.</span></span>

## <a name="iteration-statements"></a><span data-ttu-id="2395b-306">Příkazy iterace</span><span class="sxs-lookup"><span data-stu-id="2395b-306">Iteration statements</span></span>

<span data-ttu-id="2395b-307">Příkazy iterace spouštět opakovaně vloženým příkazem.</span><span class="sxs-lookup"><span data-stu-id="2395b-307">Iteration statements repeatedly execute an embedded statement.</span></span>

```antlr
iteration_statement
    : while_statement
    | do_statement
    | for_statement
    | foreach_statement
    ;
```

### <a name="the-while-statement"></a><span data-ttu-id="2395b-308">While – příkaz</span><span class="sxs-lookup"><span data-stu-id="2395b-308">The while statement</span></span>

<span data-ttu-id="2395b-309">`while` Příkaz podmíněně provede vloženým příkazem nulakrát nebo vícekrát.</span><span class="sxs-lookup"><span data-stu-id="2395b-309">The `while` statement conditionally executes an embedded statement zero or more times.</span></span>

```antlr
while_statement
    : 'while' '(' boolean_expression ')' embedded_statement
    ;
```

<span data-ttu-id="2395b-310">A `while` je proveden příkaz takto:</span><span class="sxs-lookup"><span data-stu-id="2395b-310">A `while` statement is executed as follows:</span></span>

*  <span data-ttu-id="2395b-311">*Boolean_expression* ([logické výrazy](expressions.md#boolean-expressions)) je vyhodnocen.</span><span class="sxs-lookup"><span data-stu-id="2395b-311">The *boolean_expression* ([Boolean expressions](expressions.md#boolean-expressions)) is evaluated.</span></span>
*  <span data-ttu-id="2395b-312">Je-li logický výraz vrací `true`, je kontrola předána příkazu embedded.</span><span class="sxs-lookup"><span data-stu-id="2395b-312">If the boolean expression yields `true`, control is transferred to the embedded statement.</span></span> <span data-ttu-id="2395b-313">Pokud a v případě, že ovládací prvek dosáhne koncového bodu vloženým příkazem (pravděpodobně z provádění `continue` příkaz), ovládací prvek bude převeden na začátek `while` příkazu.</span><span class="sxs-lookup"><span data-stu-id="2395b-313">When and if control reaches the end point of the embedded statement (possibly from execution of a `continue` statement), control is transferred to the beginning of the `while` statement.</span></span>
*  <span data-ttu-id="2395b-314">Je-li logický výraz vrací `false`, ovládací prvek bude převeden na koncový bod `while` příkazu.</span><span class="sxs-lookup"><span data-stu-id="2395b-314">If the boolean expression yields `false`, control is transferred to the end point of the `while` statement.</span></span>

<span data-ttu-id="2395b-315">V rámci příkazu vložený `while` příkaz, `break` – příkaz ([příkaz break](statements.md#the-break-statement)) slouží k řízení je převedeno na koncový bod `while` – příkaz (tedy končící iteraci vložený příkaz) a `continue` – příkaz ([příkaz continue](statements.md#the-continue-statement)) slouží k řízení je převedeno na koncový bod vloženým příkazem (tedy provádí další iteraci `while` příkaz).</span><span class="sxs-lookup"><span data-stu-id="2395b-315">Within the embedded statement of a `while` statement, a `break` statement ([The break statement](statements.md#the-break-statement)) may be used to transfer control to the end point of the `while` statement (thus ending iteration of the embedded statement), and a `continue` statement ([The continue statement](statements.md#the-continue-statement)) may be used to transfer control to the end point of the embedded statement (thus performing another iteration of the `while` statement).</span></span>

<span data-ttu-id="2395b-316">Příkaz vložený `while` příkaz je dostupný Pokud `while` příkaz je dostupný a logický výraz nemá konstantní hodnotu `false`.</span><span class="sxs-lookup"><span data-stu-id="2395b-316">The embedded statement of a `while` statement is reachable if the `while` statement is reachable and the boolean expression does not have the constant value `false`.</span></span>

<span data-ttu-id="2395b-317">Koncový bod `while` příkaz je dostupný, pokud platí alespoň jedna z následujících akcí:</span><span class="sxs-lookup"><span data-stu-id="2395b-317">The end point of a `while` statement is reachable if at least one of the following is true:</span></span>

*  <span data-ttu-id="2395b-318">`while` Obsahuje prohlášení dostupný `break` příkaz, který ukončí `while` příkazu.</span><span class="sxs-lookup"><span data-stu-id="2395b-318">The `while` statement contains a reachable `break` statement that exits the `while` statement.</span></span>
*  <span data-ttu-id="2395b-319">`while` Příkaz je dostupný a logický výraz nemá konstantní hodnotu `true`.</span><span class="sxs-lookup"><span data-stu-id="2395b-319">The `while` statement is reachable and the boolean expression does not have the constant value `true`.</span></span>

### <a name="the-do-statement"></a><span data-ttu-id="2395b-320">– Příkaz</span><span class="sxs-lookup"><span data-stu-id="2395b-320">The do statement</span></span>

<span data-ttu-id="2395b-321">`do` Příkaz podmíněně provede vloženým příkazem jednou nebo vícekrát.</span><span class="sxs-lookup"><span data-stu-id="2395b-321">The `do` statement conditionally executes an embedded statement one or more times.</span></span>

```antlr
do_statement
    : 'do' embedded_statement 'while' '(' boolean_expression ')' ';'
    ;
```

<span data-ttu-id="2395b-322">A `do` je proveden příkaz takto:</span><span class="sxs-lookup"><span data-stu-id="2395b-322">A `do` statement is executed as follows:</span></span>

*  <span data-ttu-id="2395b-323">Ovládací prvek bude převeden na vloženým příkazem.</span><span class="sxs-lookup"><span data-stu-id="2395b-323">Control is transferred to the embedded statement.</span></span>
*  <span data-ttu-id="2395b-324">Pokud a v případě, že ovládací prvek dosáhne koncového bodu vloženým příkazem (pravděpodobně z provádění `continue` příkaz), *boolean_expression* ([logické výrazy](expressions.md#boolean-expressions)) je vyhodnocen.</span><span class="sxs-lookup"><span data-stu-id="2395b-324">When and if control reaches the end point of the embedded statement (possibly from execution of a `continue` statement), the *boolean_expression* ([Boolean expressions](expressions.md#boolean-expressions)) is evaluated.</span></span> <span data-ttu-id="2395b-325">Je-li logický výraz vrací `true`, ovládací prvek bude převeden na začátek `do` příkazu.</span><span class="sxs-lookup"><span data-stu-id="2395b-325">If the boolean expression yields `true`, control is transferred to the beginning of the `do` statement.</span></span> <span data-ttu-id="2395b-326">V opačném případě ovládací prvek bude převeden na koncový bod `do` příkazu.</span><span class="sxs-lookup"><span data-stu-id="2395b-326">Otherwise, control is transferred to the end point of the `do` statement.</span></span>

<span data-ttu-id="2395b-327">V rámci příkazu vložený `do` příkaz, `break` – příkaz ([příkaz break](statements.md#the-break-statement)) slouží k řízení je převedeno na koncový bod `do` – příkaz (tedy končící iteraci vložený příkaz) a `continue` – příkaz ([příkaz continue](statements.md#the-continue-statement)) slouží k řízení je převedeno na koncový bod vloženým příkazem.</span><span class="sxs-lookup"><span data-stu-id="2395b-327">Within the embedded statement of a `do` statement, a `break` statement ([The break statement](statements.md#the-break-statement)) may be used to transfer control to the end point of the `do` statement (thus ending iteration of the embedded statement), and a `continue` statement ([The continue statement](statements.md#the-continue-statement)) may be used to transfer control to the end point of the embedded statement.</span></span>

<span data-ttu-id="2395b-328">Příkaz vložený `do` příkaz je dostupný Pokud `do` příkaz je dostupný.</span><span class="sxs-lookup"><span data-stu-id="2395b-328">The embedded statement of a `do` statement is reachable if the `do` statement is reachable.</span></span>

<span data-ttu-id="2395b-329">Koncový bod `do` příkaz je dostupný, pokud platí alespoň jedna z následujících akcí:</span><span class="sxs-lookup"><span data-stu-id="2395b-329">The end point of a `do` statement is reachable if at least one of the following is true:</span></span>

*  <span data-ttu-id="2395b-330">`do` Obsahuje prohlášení dostupný `break` příkaz, který ukončí `do` příkazu.</span><span class="sxs-lookup"><span data-stu-id="2395b-330">The `do` statement contains a reachable `break` statement that exits the `do` statement.</span></span>
*  <span data-ttu-id="2395b-331">Koncový bod vloženým příkazem je dostupný a logický výraz nemá konstantní hodnotu `true`.</span><span class="sxs-lookup"><span data-stu-id="2395b-331">The end point of the embedded statement is reachable and the boolean expression does not have the constant value `true`.</span></span>

### <a name="the-for-statement"></a><span data-ttu-id="2395b-332">Příkazu for</span><span class="sxs-lookup"><span data-stu-id="2395b-332">The for statement</span></span>

<span data-ttu-id="2395b-333">`for` Příkaz vyhodnocen jako posloupnost výrazy inicializace a poté, dokud je podmínka true, opakovaně provede vloženým příkazem a vyhodnocuje sekvenci výrazů iterace.</span><span class="sxs-lookup"><span data-stu-id="2395b-333">The `for` statement evaluates a sequence of initialization expressions and then, while a condition is true, repeatedly executes an embedded statement and evaluates a sequence of iteration expressions.</span></span>

```antlr
for_statement
    : 'for' '(' for_initializer? ';' for_condition? ';' for_iterator? ')' embedded_statement
    ;

for_initializer
    : local_variable_declaration
    | statement_expression_list
    ;

for_condition
    : boolean_expression
    ;

for_iterator
    : statement_expression_list
    ;

statement_expression_list
    : statement_expression (',' statement_expression)*
    ;
```

<span data-ttu-id="2395b-334">*For_initializer*, pokud jsou k dispozici, se skládá buď *local_variable_declaration* ([místní deklarace proměnné](statements.md#local-variable-declarations)) nebo seznam *statement_ výraz*s ([příkazy výrazů](statements.md#expression-statements)) oddělený čárkami.</span><span class="sxs-lookup"><span data-stu-id="2395b-334">The *for_initializer*, if present, consists of either a *local_variable_declaration* ([Local variable declarations](statements.md#local-variable-declarations)) or a list of *statement_expression*s ([Expression statements](statements.md#expression-statements)) separated by commas.</span></span> <span data-ttu-id="2395b-335">Obor lokální proměnná deklarovaná příkazem *for_initializer* začíná *local_variable_declarator* proměnné a rozšiřuje na konec vloženým příkazem.</span><span class="sxs-lookup"><span data-stu-id="2395b-335">The scope of a local variable declared by a *for_initializer* starts at the *local_variable_declarator* for the variable and extends to the end of the embedded statement.</span></span> <span data-ttu-id="2395b-336">Rozsah zahrnuje *for_condition* a *for_iterator*.</span><span class="sxs-lookup"><span data-stu-id="2395b-336">The scope includes the *for_condition* and the *for_iterator*.</span></span>

<span data-ttu-id="2395b-337">*For_condition*, pokud jsou k dispozici, musí být *boolean_expression* ([logické výrazy](expressions.md#boolean-expressions)).</span><span class="sxs-lookup"><span data-stu-id="2395b-337">The *for_condition*, if present, must be a *boolean_expression* ([Boolean expressions](expressions.md#boolean-expressions)).</span></span>

<span data-ttu-id="2395b-338">*For_iterator*, pokud jsou k dispozici, se skládá ze seznamu *statement_expression*s ([příkazy výrazů](statements.md#expression-statements)) oddělený čárkami.</span><span class="sxs-lookup"><span data-stu-id="2395b-338">The *for_iterator*, if present, consists of a list of *statement_expression*s ([Expression statements](statements.md#expression-statements)) separated by commas.</span></span>

<span data-ttu-id="2395b-339">Pro příkaz je proveden následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="2395b-339">A for statement is executed as follows:</span></span>

*  <span data-ttu-id="2395b-340">Pokud *for_initializer* je k dispozici, proměnné inicializátory nebo výrazy příkazu jsou provedeny v pořadí, jsou zapsány.</span><span class="sxs-lookup"><span data-stu-id="2395b-340">If a *for_initializer* is present, the variable initializers or statement expressions are executed in the order they are written.</span></span> <span data-ttu-id="2395b-341">Tento krok se provádí pouze jednou.</span><span class="sxs-lookup"><span data-stu-id="2395b-341">This step is only performed once.</span></span>
*  <span data-ttu-id="2395b-342">Pokud *for_condition* je k dispozici, je vyhodnocen.</span><span class="sxs-lookup"><span data-stu-id="2395b-342">If a *for_condition* is present, it is evaluated.</span></span>
*  <span data-ttu-id="2395b-343">Pokud *for_condition* není k dispozici nebo pokud je výsledkem vyhodnocení `true`, je kontrola předána příkazu embedded.</span><span class="sxs-lookup"><span data-stu-id="2395b-343">If the *for_condition* is not present or if the evaluation yields `true`, control is transferred to the embedded statement.</span></span> <span data-ttu-id="2395b-344">Pokud a v případě, že ovládací prvek dosáhne koncového bodu vloženým příkazem (pravděpodobně z provádění `continue` příkaz), výrazů operátoru *for_iterator*, pokud existují, jsou vyhodnocovány v pořadí, a pak je jiné iterace provést, počínaje vyhodnocení *for_condition* v předchozím kroku.</span><span class="sxs-lookup"><span data-stu-id="2395b-344">When and if control reaches the end point of the embedded statement (possibly from execution of a `continue` statement), the expressions of the *for_iterator*, if any, are evaluated in sequence, and then another iteration is performed, starting with evaluation of the *for_condition* in the step above.</span></span>
*  <span data-ttu-id="2395b-345">Pokud *for_condition* je k dispozici a vyhodnocení vrací `false`, ovládací prvek bude převeden na koncový bod `for` příkazu.</span><span class="sxs-lookup"><span data-stu-id="2395b-345">If the *for_condition* is present and the evaluation yields `false`, control is transferred to the end point of the `for` statement.</span></span>

<span data-ttu-id="2395b-346">V rámci příkazu vložený `for` příkaz, `break` – příkaz ([příkaz break](statements.md#the-break-statement)) slouží k řízení je převedeno na koncový bod `for` – příkaz (tedy končící iteraci vložený příkaz) a `continue` – příkaz ([příkaz continue](statements.md#the-continue-statement)) slouží k řízení je převedeno na koncový bod vloženým příkazem (tedy provádění *for_iterator* a provedení další iteraci `for` prohlášení, počínaje *for_condition*).</span><span class="sxs-lookup"><span data-stu-id="2395b-346">Within the embedded statement of a `for` statement, a `break` statement ([The break statement](statements.md#the-break-statement)) may be used to transfer control to the end point of the `for` statement (thus ending iteration of the embedded statement), and a `continue` statement ([The continue statement](statements.md#the-continue-statement)) may be used to transfer control to the end point of the embedded statement (thus executing the *for_iterator* and performing another iteration of the `for` statement, starting with the *for_condition*).</span></span>

<span data-ttu-id="2395b-347">Příkaz vložený `for` příkaz je dostupný, pokud platí jedna z následujících akcí:</span><span class="sxs-lookup"><span data-stu-id="2395b-347">The embedded statement of a `for` statement is reachable if one of the following is true:</span></span>

*  <span data-ttu-id="2395b-348">`for` Příkaz je dostupný a ne *for_condition* je k dispozici.</span><span class="sxs-lookup"><span data-stu-id="2395b-348">The `for` statement is reachable and no *for_condition* is present.</span></span>
*  <span data-ttu-id="2395b-349">`for` Příkaz je dostupný a *for_condition* je k dispozici a nemá konstantní hodnotu `false`.</span><span class="sxs-lookup"><span data-stu-id="2395b-349">The `for` statement is reachable and a *for_condition* is present and does not have the constant value `false`.</span></span>

<span data-ttu-id="2395b-350">Koncový bod `for` příkaz je dostupný, pokud platí alespoň jedna z následujících akcí:</span><span class="sxs-lookup"><span data-stu-id="2395b-350">The end point of a `for` statement is reachable if at least one of the following is true:</span></span>

*  <span data-ttu-id="2395b-351">`for` Obsahuje prohlášení dostupný `break` příkaz, který ukončí `for` příkazu.</span><span class="sxs-lookup"><span data-stu-id="2395b-351">The `for` statement contains a reachable `break` statement that exits the `for` statement.</span></span>
*  <span data-ttu-id="2395b-352">`for` Příkaz je dostupný a *for_condition* je k dispozici a nemá konstantní hodnotu `true`.</span><span class="sxs-lookup"><span data-stu-id="2395b-352">The `for` statement is reachable and a *for_condition* is present and does not have the constant value `true`.</span></span>

### <a name="the-foreach-statement"></a><span data-ttu-id="2395b-353">Příkaz foreach</span><span class="sxs-lookup"><span data-stu-id="2395b-353">The foreach statement</span></span>

<span data-ttu-id="2395b-354">`foreach` Příkaz vytvoří výčet prvků kolekce, provádění vloženým příkazem pro každý prvek kolekce.</span><span class="sxs-lookup"><span data-stu-id="2395b-354">The `foreach` statement enumerates the elements of a collection, executing an embedded statement for each element of the collection.</span></span>

```antlr
foreach_statement
    : 'foreach' '(' local_variable_type identifier 'in' expression ')' embedded_statement
    ;
```

<span data-ttu-id="2395b-355">*Typ* a *identifikátor* z `foreach` deklarovat příkaz ***proměnné iterace*** příkazu.</span><span class="sxs-lookup"><span data-stu-id="2395b-355">The *type* and *identifier* of a `foreach` statement declare the ***iteration variable*** of the statement.</span></span> <span data-ttu-id="2395b-356">Pokud `var` identifikátor je zadána jako *local_variable_type*a žádný typ s názvem `var` je v oboru, iterační proměnná se říká, že ***proměnné implicitně typované iterace***, a její typ slov za typ prvku `foreach` prohlášení, jak je uvedeno níže.</span><span class="sxs-lookup"><span data-stu-id="2395b-356">If the `var` identifier is given as the *local_variable_type*, and no type named `var` is in scope, the iteration variable is said to be an ***implicitly typed iteration variable***, and its type is taken to be the element type of the `foreach` statement, as specified below.</span></span> <span data-ttu-id="2395b-357">Iterační proměnná odpovídá jen pro čtení místní proměnné s rozsahem, který rozšiřuje nad vloženým příkazem.</span><span class="sxs-lookup"><span data-stu-id="2395b-357">The iteration variable corresponds to a read-only local variable with a scope that extends over the embedded statement.</span></span> <span data-ttu-id="2395b-358">Během provádění `foreach` prohlášení, iterační proměnná představuje prvek kolekce, pro který je aktuálně provádění iterace.</span><span class="sxs-lookup"><span data-stu-id="2395b-358">During execution of a `foreach` statement, the iteration variable represents the collection element for which an iteration is currently being performed.</span></span> <span data-ttu-id="2395b-359">Chyba kompilace v případě vloženým příkazem se pokusí upravit proměnné iterace (prostřednictvím přiřazení nebo `++` a `--` operátory) nebo předejte jako proměnnou iterace `ref` nebo `out` parametru.</span><span class="sxs-lookup"><span data-stu-id="2395b-359">A compile-time error occurs if the embedded statement attempts to modify the iteration variable (via assignment or the `++` and `--` operators) or pass the iteration variable as a `ref` or `out` parameter.</span></span>

<span data-ttu-id="2395b-360">V následujícím příkladu jako stručný výtah `IEnumerable`, `IEnumerator`, `IEnumerable<T>` a `IEnumerator<T>` odkazují na odpovídající typy v oborech názvů `System.Collections` a `System.Collections.Generic`.</span><span class="sxs-lookup"><span data-stu-id="2395b-360">In the following, for brevity, `IEnumerable`, `IEnumerator`, `IEnumerable<T>` and `IEnumerator<T>` refer to the corresponding types in the namespaces `System.Collections` and `System.Collections.Generic`.</span></span>

<span data-ttu-id="2395b-361">Kompilace zpracování příkazu foreach nejdřív zjistí, ***typ kolekce***, ***výčtovým typem*** a ***typ elementu*** výrazu.</span><span class="sxs-lookup"><span data-stu-id="2395b-361">The compile-time processing of a foreach statement first determines the ***collection type***, ***enumerator type*** and ***element type*** of the expression.</span></span> <span data-ttu-id="2395b-362">Určení probíhá následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="2395b-362">This determination proceeds as follows:</span></span>

*  <span data-ttu-id="2395b-363">Pokud typ `X` z *výraz* je typ pole je implicitní referenční převod z `X` k `IEnumerable` rozhraní (protože `System.Array` implementuje toto rozhraní).</span><span class="sxs-lookup"><span data-stu-id="2395b-363">If the type `X` of *expression* is an array type then there is an implicit reference conversion from `X` to the `IEnumerable` interface (since `System.Array` implements this interface).</span></span> <span data-ttu-id="2395b-364">***Typ kolekce*** je `IEnumerable` rozhraní, ***výčtovým typem*** je `IEnumerator` rozhraní a ***typ elementu*** je typ prvku Typ pole `X`.</span><span class="sxs-lookup"><span data-stu-id="2395b-364">The ***collection type*** is the `IEnumerable` interface, the ***enumerator type*** is the `IEnumerator` interface and the ***element type*** is the element type of the array type `X`.</span></span>
*  <span data-ttu-id="2395b-365">Pokud typ `X` z *výraz* je `dynamic` je implicitní převod z *výraz* k `IEnumerable` rozhraní ([implicitní dynamic převody](conversions.md#implicit-dynamic-conversions)).</span><span class="sxs-lookup"><span data-stu-id="2395b-365">If the type `X` of *expression* is `dynamic` then there is an implicit conversion from *expression* to the `IEnumerable` interface ([Implicit dynamic conversions](conversions.md#implicit-dynamic-conversions)).</span></span> <span data-ttu-id="2395b-366">***Typ kolekce*** je `IEnumerable` rozhraní a ***výčtovým typem*** je `IEnumerator` rozhraní.</span><span class="sxs-lookup"><span data-stu-id="2395b-366">The ***collection type*** is the `IEnumerable` interface and the ***enumerator type*** is the `IEnumerator` interface.</span></span> <span data-ttu-id="2395b-367">Pokud `var` identifikátor je zadána jako *local_variable_type* pak bude ***typ elementu*** je `dynamic`, v opačném případě je `object`.</span><span class="sxs-lookup"><span data-stu-id="2395b-367">If the `var` identifier is given as the *local_variable_type* then the ***element type*** is `dynamic`, otherwise it is `object`.</span></span>
*  <span data-ttu-id="2395b-368">V opačném případě určit, zda typ `X` má odpovídající `GetEnumerator` metody:</span><span class="sxs-lookup"><span data-stu-id="2395b-368">Otherwise, determine whether the type `X` has an appropriate `GetEnumerator` method:</span></span>
   * <span data-ttu-id="2395b-369">Provádět vyhledávání člen v typu `X` s identifikátorem `GetEnumerator` a žádné argumenty typu.</span><span class="sxs-lookup"><span data-stu-id="2395b-369">Perform member lookup on the type `X` with identifier `GetEnumerator` and no type arguments.</span></span> <span data-ttu-id="2395b-370">Pokud je výsledkem vyhledávání člen není shoda, nebo ho vytvoří nejednoznačnost, nebo je výsledný shodu, která není skupinou – metoda, hledá vyčíslitelná rozhraní jak je popsáno níže.</span><span class="sxs-lookup"><span data-stu-id="2395b-370">If the member lookup does not produce a match, or it produces an ambiguity, or produces a match that is not a method group, check for an enumerable interface as described below.</span></span> <span data-ttu-id="2395b-371">Doporučuje se, že se objeví upozornění, pokud člen vyhledávání vytvoří nic s výjimkou skupinu metod nebo žádná shoda.</span><span class="sxs-lookup"><span data-stu-id="2395b-371">It is recommended that a warning be issued if member lookup produces anything except a method group or no match.</span></span>
   * <span data-ttu-id="2395b-372">Proveďte řešení přetížení pomocí výsledný skupinu metod a prázdným seznamem argumentů.</span><span class="sxs-lookup"><span data-stu-id="2395b-372">Perform overload resolution using the resulting method group and an empty argument list.</span></span> <span data-ttu-id="2395b-373">Řešení přetížení výsledků v žádné příslušné metody, výsledkem nejednoznačnost, zda výsledkem jednoho nejlepší metody, ale, že metoda je buď statický, nebo není veřejné vyhledat vyčíslitelná rozhraní, jak je popsáno níže.</span><span class="sxs-lookup"><span data-stu-id="2395b-373">If overload resolution results in no applicable methods, results in an ambiguity, or results in a single best method but that method is either static or not public, check for an enumerable interface as described below.</span></span> <span data-ttu-id="2395b-374">Doporučuje se, že se objeví upozornění v případě přetížení vytvoří nic s výjimkou metody jednoznačný veřejné instance nebo žádné použitelné metody.</span><span class="sxs-lookup"><span data-stu-id="2395b-374">It is recommended that a warning be issued if overload resolution produces anything except an unambiguous public instance method or no applicable methods.</span></span>
   * <span data-ttu-id="2395b-375">Pokud návratový typ `E` z `GetEnumerator` metoda se nejedná o třídu, je vytvořen typ struktury nebo rozhraní, chybu a jsou přesměrováni žádné další kroky.</span><span class="sxs-lookup"><span data-stu-id="2395b-375">If the return type `E` of the `GetEnumerator` method is not a class, struct or interface type, an error is produced and no further steps are taken.</span></span>
   * <span data-ttu-id="2395b-376">Člen vyhledávání se provádí na `E` s identifikátorem `Current` a žádné argumenty typu.</span><span class="sxs-lookup"><span data-stu-id="2395b-376">Member lookup is performed on `E` with the identifier `Current` and no type arguments.</span></span> <span data-ttu-id="2395b-377">Pokud člen vyhledávání vytvoří žádná shoda, výsledkem je chyba nebo výsledek je kromě veřejnou vlastnost instance, která umožňuje čtení, je vytvořen chybu a jsou přesměrováni žádné další kroky.</span><span class="sxs-lookup"><span data-stu-id="2395b-377">If the member lookup produces no match, the result is an error, or the result is anything except a public instance property that permits reading, an error is produced and no further steps are taken.</span></span>
   * <span data-ttu-id="2395b-378">Člen vyhledávání se provádí na `E` s identifikátorem `MoveNext` a žádné argumenty typu.</span><span class="sxs-lookup"><span data-stu-id="2395b-378">Member lookup is performed on `E` with the identifier `MoveNext` and no type arguments.</span></span> <span data-ttu-id="2395b-379">Pokud člen vyhledávání vytvoří žádná shoda, výsledkem je chyba nebo kromě skupinu metod. Výsledkem je, je vytvořen chybu a jsou přesměrováni žádné další kroky.</span><span class="sxs-lookup"><span data-stu-id="2395b-379">If the member lookup produces no match, the result is an error, or the result is anything except a method group, an error is produced and no further steps are taken.</span></span>
   * <span data-ttu-id="2395b-380">Řešení přetížení se provádí na skupinu metod s prázdným seznamem argumentů.</span><span class="sxs-lookup"><span data-stu-id="2395b-380">Overload resolution is performed on the method group with an empty argument list.</span></span> <span data-ttu-id="2395b-381">Pokud výsledky rozlišení přetížení v žádné příslušné metody, výsledky v nejednoznačnost nebo výsledky v jediné nejlepší metody, ale tato metoda je buď statický, nebo není veřejné nebo její typ vrácené hodnoty není `bool`, je vytvořen chybu a jsou přesměrováni žádné další kroky.</span><span class="sxs-lookup"><span data-stu-id="2395b-381">If overload resolution results in no applicable methods, results in an ambiguity, or results in a single best method but that method is either static or not public, or its return type is not `bool`, an error is produced and no further steps are taken.</span></span>
   * <span data-ttu-id="2395b-382">***Typ kolekce*** je `X`, ***výčtovým typem*** je `E`a ***typ elementu*** je typ `Current` vlastnost.</span><span class="sxs-lookup"><span data-stu-id="2395b-382">The ***collection type*** is `X`, the ***enumerator type*** is `E`, and the ***element type*** is the type of the `Current` property.</span></span>

*  <span data-ttu-id="2395b-383">V opačném případě zkontrolujte vyčíslitelná rozhraní:</span><span class="sxs-lookup"><span data-stu-id="2395b-383">Otherwise, check for an enumerable interface:</span></span>
   * <span data-ttu-id="2395b-384">Pokud mezi všechny typy `Ti` pro něž neexistuje implicitní převod z `X` k `IEnumerable<Ti>`, je jedinečný typ `T` tak, aby `T` není `dynamic` a pro všechny ostatní `Ti` je implicitní převod z `IEnumerable<T>` k `IEnumerable<Ti>`, pak bude ***typ kolekce*** je rozhraní `IEnumerable<T>`, ***výčtovým typem*** je rozhraní `IEnumerator<T>`a ***typ elementu*** je `T`.</span><span class="sxs-lookup"><span data-stu-id="2395b-384">If among all the types `Ti` for which there is an implicit conversion from `X` to `IEnumerable<Ti>`, there is a unique type `T` such that `T` is not `dynamic` and for all the other `Ti` there is an implicit conversion from `IEnumerable<T>` to `IEnumerable<Ti>`, then the ***collection type*** is the interface `IEnumerable<T>`, the ***enumerator type*** is the interface `IEnumerator<T>`, and the ***element type*** is `T`.</span></span>
   * <span data-ttu-id="2395b-385">Jinak, pokud existuje více než jeden takový typ `T`, je vytvořen chybu a jsou provedeny žádné další kroky.</span><span class="sxs-lookup"><span data-stu-id="2395b-385">Otherwise, if there is more than one such type `T`, then an error is produced and no further steps are taken.</span></span>
   * <span data-ttu-id="2395b-386">Jinak, pokud je implicitní převod z `X` k `System.Collections.IEnumerable` rozhraní, pak bude ***typ kolekce*** je toto rozhraní ***výčtovým typem*** je rozhraní `System.Collections.IEnumerator`a ***typ elementu*** je `object`.</span><span class="sxs-lookup"><span data-stu-id="2395b-386">Otherwise, if there is an implicit conversion from `X` to the `System.Collections.IEnumerable` interface, then the ***collection type*** is this interface, the ***enumerator type*** is the interface `System.Collections.IEnumerator`, and the ***element type*** is `object`.</span></span>
   * <span data-ttu-id="2395b-387">V opačném případě je vytvořen chybu a jsou přesměrováni žádné další kroky.</span><span class="sxs-lookup"><span data-stu-id="2395b-387">Otherwise, an error is produced and no further steps are taken.</span></span>

<span data-ttu-id="2395b-388">Výše uvedené kroky, pokud je úspěšná, jednoznačně vytvořit typ kolekce `C`, výčtovým typem `E` a typ prvku `T`.</span><span class="sxs-lookup"><span data-stu-id="2395b-388">The above steps, if successful, unambiguously produce a collection type `C`, enumerator type `E` and element type `T`.</span></span> <span data-ttu-id="2395b-389">Příkaz foreach formuláře</span><span class="sxs-lookup"><span data-stu-id="2395b-389">A foreach statement of the form</span></span>
```csharp
foreach (V v in x) embedded_statement
```
<span data-ttu-id="2395b-390">je pak rozšířen pro:</span><span class="sxs-lookup"><span data-stu-id="2395b-390">is then expanded to:</span></span>
```csharp
{
    E e = ((C)(x)).GetEnumerator();
    try {
        while (e.MoveNext()) {
            V v = (V)(T)e.Current;
            embedded_statement
        }
    }
    finally {
        ... // Dispose e
    }
}
```

<span data-ttu-id="2395b-391">Proměnná `e` není viditelná nebo přístupné pro výraz `x` nebo vloženým příkazem nebo jiný zdrojový kód programu.</span><span class="sxs-lookup"><span data-stu-id="2395b-391">The variable `e` is not visible to or accessible to the expression `x` or the embedded statement or any other source code of the program.</span></span> <span data-ttu-id="2395b-392">Proměnná `v` je jen pro čtení v vloženým příkazem.</span><span class="sxs-lookup"><span data-stu-id="2395b-392">The variable `v` is read-only in the embedded statement.</span></span> <span data-ttu-id="2395b-393">Pokud není k dispozici explicitní převod ([explicitních převodů](conversions.md#explicit-conversions)) z `T` (typ prvku) pro `V` ( *local_variable_type* v příkazu foreach), vytvořilo chybu a jsou přesměrováni žádné další kroky.</span><span class="sxs-lookup"><span data-stu-id="2395b-393">If there is not an explicit conversion ([Explicit conversions](conversions.md#explicit-conversions)) from `T` (the element type) to `V` (the *local_variable_type* in the foreach statement), an error is produced and no further steps are taken.</span></span> <span data-ttu-id="2395b-394">Pokud `x` má hodnotu `null`, `System.NullReferenceException` je vyvolána v době běhu.</span><span class="sxs-lookup"><span data-stu-id="2395b-394">If `x` has the value `null`, a `System.NullReferenceException` is thrown at run-time.</span></span>

<span data-ttu-id="2395b-395">Implementace je povolený pro implementaci daného příkazu foreach odlišně, např. z důvodů výkonu za předpokladu, chování je konzistentní s rozšiřující se výše.</span><span class="sxs-lookup"><span data-stu-id="2395b-395">An implementation is permitted to implement a given foreach-statement differently, e.g. for performance reasons, as long as the behavior is consistent with the above expansion.</span></span>

<span data-ttu-id="2395b-396">Umístění `v` uvnitř while je důležité, jak je zachycena v jakékoli anonymní funkci, ke kterým dochází v smyčky *embedded_statement*.</span><span class="sxs-lookup"><span data-stu-id="2395b-396">The placement of `v` inside the while loop is important for how it is captured by any anonymous function occurring in the *embedded_statement*.</span></span>

<span data-ttu-id="2395b-397">Příklad:</span><span class="sxs-lookup"><span data-stu-id="2395b-397">For example:</span></span>
```csharp
int[] values = { 7, 9, 13 };
Action f = null;

foreach (var value in values)
{
    if (f == null) f = () => Console.WriteLine("First value: " + value);
}

f();
```
<span data-ttu-id="2395b-398">Pokud `v` byla deklarována mimo while smyčky, by být sdílena mezi všech iterací a jeho hodnota po smyčky by být konečná hodnota `13`, který je co vyvolání typu `f` vytiskněte.</span><span class="sxs-lookup"><span data-stu-id="2395b-398">If `v` was declared outside of the while loop, it would be shared among all iterations, and its value after the for loop would be the final value, `13`, which is what the invocation of `f` would print.</span></span> <span data-ttu-id="2395b-399">Místo toho protože každé iterace má svůj vlastní proměnnou `v`, je zachycená výrazem `f` v první iteraci budou pro uchování hodnoty `7`, což je, co budou zobrazeny.</span><span class="sxs-lookup"><span data-stu-id="2395b-399">Instead, because each iteration has its own variable `v`, the one captured by `f` in the first iteration will continue to hold the value `7`, which is what will be printed.</span></span> <span data-ttu-id="2395b-400">(Poznámka: starší verze jazyka C# deklarované `v` smyčku while mimo.)</span><span class="sxs-lookup"><span data-stu-id="2395b-400">(Note: earlier versions of C# declared `v` outside of the while loop.)</span></span>

<span data-ttu-id="2395b-401">Text nakonec bloku je vytvořený podle následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="2395b-401">The body of the finally block is constructed according to the following steps:</span></span>

*  <span data-ttu-id="2395b-402">Pokud je implicitní převod z `E` k `System.IDisposable` rozhraní, pak</span><span class="sxs-lookup"><span data-stu-id="2395b-402">If there is an implicit conversion from `E` to the `System.IDisposable` interface, then</span></span>
   *  <span data-ttu-id="2395b-403">Pokud `E` je typ hodnoty Null bude nakonec klauzule rozbalen a sémantické ekvivalent:</span><span class="sxs-lookup"><span data-stu-id="2395b-403">If `E` is a non-nullable value type then the finally clause is expanded to the semantic equivalent  of:</span></span>

      ```csharp
      finally {
          ((System.IDisposable)e).Dispose();
      }
      ```

   *  <span data-ttu-id="2395b-404">V opačném případě nakonec klauzule rozbalen a sémantické ekvivalent:</span><span class="sxs-lookup"><span data-stu-id="2395b-404">Otherwise the finally clause is expanded to the semantic equivalent of:</span></span>

      ```csharp
      finally {
          if (e != null) ((System.IDisposable)e).Dispose();
      }
      ```

   <span data-ttu-id="2395b-405">s výjimkou, že pokud `E` je hodnotový typ nebo parametr typu instance typu hodnoty a pak přetypování souřadnic `e` k `System.IDisposable` nezpůsobí zabalení dochází.</span><span class="sxs-lookup"><span data-stu-id="2395b-405">except that if `E` is a value type, or a type parameter instantiated to a value type, then the cast of `e` to `System.IDisposable` will not cause boxing to occur.</span></span>

*  <span data-ttu-id="2395b-406">Jinak, pokud `E` je zapečetěný typ, nakonec klauzule rozbalen a prázdný blok:</span><span class="sxs-lookup"><span data-stu-id="2395b-406">Otherwise, if `E` is a sealed type, the finally clause is expanded to an empty block:</span></span>

   ```csharp
   finally {
   }
   ```

*  <span data-ttu-id="2395b-407">V opačném případě nakonec klauzule rozbalen do:</span><span class="sxs-lookup"><span data-stu-id="2395b-407">Otherwise, the finally clause is expanded to:</span></span>

   ```csharp
   finally {
       System.IDisposable d = e as System.IDisposable;
       if (d != null) d.Dispose();
   }
   ```    

   <span data-ttu-id="2395b-408">Lokální proměnná `d` není viditelné pro nebo přístup k veškerému kódu uživatele.</span><span class="sxs-lookup"><span data-stu-id="2395b-408">The local variable `d` is not visible to or accessible to any user code.</span></span> <span data-ttu-id="2395b-409">Konkrétně to není v konfliktu s jakoukoli jinou proměnnou, jejíž obor obsahuje bloku finally.</span><span class="sxs-lookup"><span data-stu-id="2395b-409">In particular, it does not conflict with any other variable whose scope includes the finally block.</span></span>

<span data-ttu-id="2395b-410">Pořadí, ve kterém `foreach` prochází přes prvky pole, vypadá takto: Pro prvky jednorozměrná pole je provázán ve vzestupném pořadí indexů, počínaje indexem `0` a konče index `Length - 1`.</span><span class="sxs-lookup"><span data-stu-id="2395b-410">The order in which `foreach` traverses the elements of an array, is as follows: For single-dimensional arrays elements are traversed in increasing index order, starting with index `0` and ending with index `Length - 1`.</span></span> <span data-ttu-id="2395b-411">Pro vícerozměrná pole jsou prvky procházet tak, že indexy rozměr nejvíce vpravo se zvýšenou první pak další levé dimenze nalevo a tak dále.</span><span class="sxs-lookup"><span data-stu-id="2395b-411">For multi-dimensional arrays, elements are traversed such that the indices of the rightmost dimension are increased first, then the next left dimension, and so on to the left.</span></span>

<span data-ttu-id="2395b-412">Následující příklad vytiskne každé hodnoty v dvojrozměrné pole, v pořadí elementů:</span><span class="sxs-lookup"><span data-stu-id="2395b-412">The following example prints out each value in a two-dimensional array, in element order:</span></span>
```csharp
using System;

class Test
{
    static void Main() {
        double[,] values = {
            {1.2, 2.3, 3.4, 4.5},
            {5.6, 6.7, 7.8, 8.9}
        };

        foreach (double elementValue in values)
            Console.Write("{0} ", elementValue);

        Console.WriteLine();
    }
}
```
<span data-ttu-id="2395b-413">Výstup vytvořený vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="2395b-413">The output produced is as follows:</span></span>
```csharp
1.2 2.3 3.4 4.5 5.6 6.7 7.8 8.9
```

<span data-ttu-id="2395b-414">V příkladu</span><span class="sxs-lookup"><span data-stu-id="2395b-414">In the example</span></span>
```csharp
int[] numbers = { 1, 3, 5, 7, 9 };
foreach (var n in numbers) Console.WriteLine(n);
```
<span data-ttu-id="2395b-415">typ `n` odvozena jako `int`, typ elementu `numbers`.</span><span class="sxs-lookup"><span data-stu-id="2395b-415">the type of `n` is inferred to be `int`, the element type of `numbers`.</span></span>

## <a name="jump-statements"></a><span data-ttu-id="2395b-416">Jump – příkazy</span><span class="sxs-lookup"><span data-stu-id="2395b-416">Jump statements</span></span>

<span data-ttu-id="2395b-417">Příkazy skoku bezpodmínečně přenést řízení.</span><span class="sxs-lookup"><span data-stu-id="2395b-417">Jump statements unconditionally transfer control.</span></span>

```antlr
jump_statement
    : break_statement
    | continue_statement
    | goto_statement
    | return_statement
    | throw_statement
    ;
```

<span data-ttu-id="2395b-418">Umístění, ke kterému předává příkaz skoku řízení se nazývá ***cílové*** příkazu jump.</span><span class="sxs-lookup"><span data-stu-id="2395b-418">The location to which a jump statement transfers control is called the ***target*** of the jump statement.</span></span>

<span data-ttu-id="2395b-419">Pokud v rámci bloku dojde k výpisu odkazů, a cíl, který tento příkaz skoku je mimo tento blok, příkaz skoku se říká, že ***ukončit*** bloku.</span><span class="sxs-lookup"><span data-stu-id="2395b-419">When a jump statement occurs within a block, and the target of that jump statement is outside that block, the jump statement is said to ***exit*** the block.</span></span> <span data-ttu-id="2395b-420">Při výpisu odkazů může přenést řízení mimo blok, se nikdy nepřenášejí ovládacího prvku do bloku.</span><span class="sxs-lookup"><span data-stu-id="2395b-420">While a jump statement may transfer control out of a block, it can never transfer control into a block.</span></span>

<span data-ttu-id="2395b-421">Spuštění příkazů skoku je složité přítomností intervenující `try` příkazy.</span><span class="sxs-lookup"><span data-stu-id="2395b-421">Execution of jump statements is complicated by the presence of intervening `try` statements.</span></span> <span data-ttu-id="2395b-422">Chybí těchto `try` příkazy, příkaz skoku bezpodmínečně přenese ovládací prvek z příkazu skoku k cíli.</span><span class="sxs-lookup"><span data-stu-id="2395b-422">In the absence of such `try` statements, a jump statement unconditionally transfers control from the jump statement to its target.</span></span> <span data-ttu-id="2395b-423">Za přítomnosti takové intervenující `try` příkazy, provedení je složitější.</span><span class="sxs-lookup"><span data-stu-id="2395b-423">In the presence of such intervening `try` statements, execution is more complex.</span></span> <span data-ttu-id="2395b-424">Pokud se příkaz skoku ukončí jeden nebo více `try` přidružené bloky s `finally` bloky, ovládací prvek zpočátku bude převeden na `finally` bloku nejvnitřnější `try` příkazu.</span><span class="sxs-lookup"><span data-stu-id="2395b-424">If the jump statement exits one or more `try` blocks with associated `finally` blocks, control is initially transferred to the `finally` block of the innermost `try` statement.</span></span> <span data-ttu-id="2395b-425">Pokud a v případě, že ovládací prvek dosáhne koncového bodu `finally` bloku, ovládací prvek bude převeden na `finally` další uzavírající blok `try` příkazu.</span><span class="sxs-lookup"><span data-stu-id="2395b-425">When and if control reaches the end point of a `finally` block, control is transferred to the `finally` block of the next enclosing `try` statement.</span></span> <span data-ttu-id="2395b-426">Tento proces se opakuje, dokud `finally` všechny bloky použité `try` příkazy byly provedeny.</span><span class="sxs-lookup"><span data-stu-id="2395b-426">This process is repeated until the `finally` blocks of all intervening `try` statements have been executed.</span></span>

<span data-ttu-id="2395b-427">V příkladu</span><span class="sxs-lookup"><span data-stu-id="2395b-427">In the example</span></span>
```csharp
using System;

class Test
{
    static void Main() {
        while (true) {
            try {
                try {
                    Console.WriteLine("Before break");
                    break;
                }
                finally {
                    Console.WriteLine("Innermost finally block");
                }
            }
            finally {
                Console.WriteLine("Outermost finally block");
            }
        }
        Console.WriteLine("After break");
    }
}
```
<span data-ttu-id="2395b-428">`finally` bloky, které jsou přidružené dvě `try` příkazy jsou spouštěny před ovládací prvek bude převeden na cíl příkazu skoku.</span><span class="sxs-lookup"><span data-stu-id="2395b-428">the `finally` blocks associated with two `try` statements are executed before control is transferred to the target of the jump statement.</span></span>

<span data-ttu-id="2395b-429">Výstup vytvořený vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="2395b-429">The output produced is as follows:</span></span>
```
Before break
Innermost finally block
Outermost finally block
After break
```

### <a name="the-break-statement"></a><span data-ttu-id="2395b-430">Příkaz break</span><span class="sxs-lookup"><span data-stu-id="2395b-430">The break statement</span></span>

<span data-ttu-id="2395b-431">`break` Ukončí nejbližšího obklopujícího příkazu `switch`, `while`, `do`, `for`, nebo `foreach` příkazu.</span><span class="sxs-lookup"><span data-stu-id="2395b-431">The `break` statement exits the nearest enclosing `switch`, `while`, `do`, `for`, or `foreach` statement.</span></span>

```antlr
break_statement
    : 'break' ';'
    ;
```

<span data-ttu-id="2395b-432">Cíl `break` příkaz je koncový bod nejbližšího obklopujícího `switch`, `while`, `do`, `for`, nebo `foreach` příkazu.</span><span class="sxs-lookup"><span data-stu-id="2395b-432">The target of a `break` statement is the end point of the nearest enclosing `switch`, `while`, `do`, `for`, or `foreach` statement.</span></span> <span data-ttu-id="2395b-433">Pokud `break` příkazu není uzavřená v `switch`, `while`, `do`, `for`, nebo `foreach` příkaz, vyvolá chybu v době kompilace.</span><span class="sxs-lookup"><span data-stu-id="2395b-433">If a `break` statement is not enclosed by a `switch`, `while`, `do`, `for`, or `foreach` statement, a compile-time error occurs.</span></span>

<span data-ttu-id="2395b-434">Když více `switch`, `while`, `do`, `for`, nebo `foreach` příkazy jsou vnořeny do sebe navzájem `break` příkaz platí jenom pro vnitřní příkaz.</span><span class="sxs-lookup"><span data-stu-id="2395b-434">When multiple `switch`, `while`, `do`, `for`, or `foreach` statements are nested within each other, a `break` statement applies only to the innermost statement.</span></span> <span data-ttu-id="2395b-435">Přenos řízení napříč více úrovní vnoření, `goto` – příkaz ([příkazu goto](statements.md#the-goto-statement)) musí být použita.</span><span class="sxs-lookup"><span data-stu-id="2395b-435">To transfer control across multiple nesting levels, a `goto` statement ([The goto statement](statements.md#the-goto-statement)) must be used.</span></span>

<span data-ttu-id="2395b-436">A `break` příkaz nemůže ukončit `finally` blok ([příkazu try](statements.md#the-try-statement)).</span><span class="sxs-lookup"><span data-stu-id="2395b-436">A `break` statement cannot exit a `finally` block ([The try statement](statements.md#the-try-statement)).</span></span> <span data-ttu-id="2395b-437">Při `break` příkazu vyvolá se v rámci `finally` blokovat, cíl `break` příkaz musí být v rámci stejného `finally` blokovat; v opačném případě dojde k chybě kompilace.</span><span class="sxs-lookup"><span data-stu-id="2395b-437">When a `break` statement occurs within a `finally` block, the target of the `break` statement must be within the same `finally` block; otherwise, a compile-time error occurs.</span></span>

<span data-ttu-id="2395b-438">A `break` je proveden příkaz takto:</span><span class="sxs-lookup"><span data-stu-id="2395b-438">A `break` statement is executed as follows:</span></span>

*  <span data-ttu-id="2395b-439">Pokud `break` příkaz ukončí jeden nebo více `try` přidružené bloky s `finally` bloky, ovládací prvek zpočátku bude převeden na `finally` bloku nejvnitřnější `try` příkaz.</span><span class="sxs-lookup"><span data-stu-id="2395b-439">If the `break` statement exits one or more `try` blocks with associated `finally` blocks, control is initially transferred to the `finally` block of the innermost `try` statement.</span></span> <span data-ttu-id="2395b-440">Pokud a v případě, že ovládací prvek dosáhne koncového bodu `finally` bloku, ovládací prvek bude převeden na `finally` další uzavírající blok `try` příkazu.</span><span class="sxs-lookup"><span data-stu-id="2395b-440">When and if control reaches the end point of a `finally` block, control is transferred to the `finally` block of the next enclosing `try` statement.</span></span> <span data-ttu-id="2395b-441">Tento proces se opakuje, dokud `finally` všechny bloky použité `try` příkazy byly provedeny.</span><span class="sxs-lookup"><span data-stu-id="2395b-441">This process is repeated until the `finally` blocks of all intervening `try` statements have been executed.</span></span>
*  <span data-ttu-id="2395b-442">Ovládací prvek bude převeden do cílového místa `break` příkazu.</span><span class="sxs-lookup"><span data-stu-id="2395b-442">Control is transferred to the target of the `break` statement.</span></span>

<span data-ttu-id="2395b-443">Protože `break` příkaz bezpodmínečně převede ovládací prvek jinde, koncový bod `break` příkazu není dostupný.</span><span class="sxs-lookup"><span data-stu-id="2395b-443">Because a `break` statement unconditionally transfers control elsewhere, the end point of a `break` statement is never reachable.</span></span>

### <a name="the-continue-statement"></a><span data-ttu-id="2395b-444">Příkaz continue</span><span class="sxs-lookup"><span data-stu-id="2395b-444">The continue statement</span></span>

<span data-ttu-id="2395b-445">`continue` Spustí novou iteraci nejbližšího ohraničujícího příkazu `while`, `do`, `for`, nebo `foreach` příkazu.</span><span class="sxs-lookup"><span data-stu-id="2395b-445">The `continue` statement starts a new iteration of the nearest enclosing `while`, `do`, `for`, or `foreach` statement.</span></span>

```antlr
continue_statement
    : 'continue' ';'
    ;
```

<span data-ttu-id="2395b-446">Cíl `continue` příkaz je koncový bod vloženým příkazem nejbližšího obklopujícího `while`, `do`, `for`, nebo `foreach` příkazu.</span><span class="sxs-lookup"><span data-stu-id="2395b-446">The target of a `continue` statement is the end point of the embedded statement of the nearest enclosing `while`, `do`, `for`, or `foreach` statement.</span></span> <span data-ttu-id="2395b-447">Pokud `continue` příkazu není uzavřená v `while`, `do`, `for`, nebo `foreach` příkaz, vyvolá chybu v době kompilace.</span><span class="sxs-lookup"><span data-stu-id="2395b-447">If a `continue` statement is not enclosed by a `while`, `do`, `for`, or `foreach` statement, a compile-time error occurs.</span></span>

<span data-ttu-id="2395b-448">Když více `while`, `do`, `for`, nebo `foreach` příkazy jsou vnořeny do sebe navzájem `continue` příkaz platí jenom pro vnitřní příkaz.</span><span class="sxs-lookup"><span data-stu-id="2395b-448">When multiple `while`, `do`, `for`, or `foreach` statements are nested within each other, a `continue` statement applies only to the innermost statement.</span></span> <span data-ttu-id="2395b-449">Přenos řízení napříč více úrovní vnoření, `goto` – příkaz ([příkazu goto](statements.md#the-goto-statement)) musí být použita.</span><span class="sxs-lookup"><span data-stu-id="2395b-449">To transfer control across multiple nesting levels, a `goto` statement ([The goto statement](statements.md#the-goto-statement)) must be used.</span></span>

<span data-ttu-id="2395b-450">A `continue` příkaz nemůže ukončit `finally` blok ([příkazu try](statements.md#the-try-statement)).</span><span class="sxs-lookup"><span data-stu-id="2395b-450">A `continue` statement cannot exit a `finally` block ([The try statement](statements.md#the-try-statement)).</span></span> <span data-ttu-id="2395b-451">Při `continue` příkazu vyvolá se v rámci `finally` blokovat, cíl `continue` příkaz musí být v rámci stejného `finally` blokovat; v opačném případě dojde k chybě kompilace.</span><span class="sxs-lookup"><span data-stu-id="2395b-451">When a `continue` statement occurs within a `finally` block, the target of the `continue` statement must be within the same `finally` block; otherwise a compile-time error occurs.</span></span>

<span data-ttu-id="2395b-452">A `continue` je proveden příkaz takto:</span><span class="sxs-lookup"><span data-stu-id="2395b-452">A `continue` statement is executed as follows:</span></span>

*  <span data-ttu-id="2395b-453">Pokud `continue` příkaz ukončí jeden nebo více `try` přidružené bloky s `finally` bloky, ovládací prvek zpočátku bude převeden na `finally` bloku nejvnitřnější `try` příkaz.</span><span class="sxs-lookup"><span data-stu-id="2395b-453">If the `continue` statement exits one or more `try` blocks with associated `finally` blocks, control is initially transferred to the `finally` block of the innermost `try` statement.</span></span> <span data-ttu-id="2395b-454">Pokud a v případě, že ovládací prvek dosáhne koncového bodu `finally` bloku, ovládací prvek bude převeden na `finally` další uzavírající blok `try` příkazu.</span><span class="sxs-lookup"><span data-stu-id="2395b-454">When and if control reaches the end point of a `finally` block, control is transferred to the `finally` block of the next enclosing `try` statement.</span></span> <span data-ttu-id="2395b-455">Tento proces se opakuje, dokud `finally` všechny bloky použité `try` příkazy byly provedeny.</span><span class="sxs-lookup"><span data-stu-id="2395b-455">This process is repeated until the `finally` blocks of all intervening `try` statements have been executed.</span></span>
*  <span data-ttu-id="2395b-456">Ovládací prvek bude převeden do cílového místa `continue` příkazu.</span><span class="sxs-lookup"><span data-stu-id="2395b-456">Control is transferred to the target of the `continue` statement.</span></span>

<span data-ttu-id="2395b-457">Protože `continue` příkaz bezpodmínečně převede ovládací prvek jinde, koncový bod `continue` příkazu není dostupný.</span><span class="sxs-lookup"><span data-stu-id="2395b-457">Because a `continue` statement unconditionally transfers control elsewhere, the end point of a `continue` statement is never reachable.</span></span>

### <a name="the-goto-statement"></a><span data-ttu-id="2395b-458">Goto – příkaz</span><span class="sxs-lookup"><span data-stu-id="2395b-458">The goto statement</span></span>

<span data-ttu-id="2395b-459">`goto` Příkaz přenese ovládací prvek na příkaz, který je označen popiskem.</span><span class="sxs-lookup"><span data-stu-id="2395b-459">The `goto` statement transfers control to a statement that is marked by a label.</span></span>

```antlr
goto_statement
    : 'goto' identifier ';'
    | 'goto' 'case' constant_expression ';'
    | 'goto' 'default' ';'
    ;
```

<span data-ttu-id="2395b-460">Cíl `goto` *identifikátor* příkaz je příkaz s popiskem s danou popiskem.</span><span class="sxs-lookup"><span data-stu-id="2395b-460">The target of a `goto` *identifier* statement is the labeled statement with the given label.</span></span> <span data-ttu-id="2395b-461">Pokud popisek se zadaným názvem neexistuje v aktuálním členské funkce, nebo pokud `goto` příkaz není v rámci oboru popisku, dojde k chybě kompilace.</span><span class="sxs-lookup"><span data-stu-id="2395b-461">If a label with the given name does not exist in the current function member, or if the `goto` statement is not within the scope of the label, a compile-time error occurs.</span></span> <span data-ttu-id="2395b-462">Toto pravidlo povoluje použití `goto` příkaz k předání řízení z vnořené oboru, ale ne do vnořené oboru.</span><span class="sxs-lookup"><span data-stu-id="2395b-462">This rule permits the use of a `goto` statement to transfer control out of a nested scope, but not into a nested scope.</span></span> <span data-ttu-id="2395b-463">V příkladu</span><span class="sxs-lookup"><span data-stu-id="2395b-463">In the example</span></span>
```csharp
using System;

class Test
{
    static void Main(string[] args) {
        string[,] table = {
            {"Red", "Blue", "Green"},
            {"Monday", "Wednesday", "Friday"}
        };

        foreach (string str in args) {
            int row, colm;
            for (row = 0; row <= 1; ++row)
                for (colm = 0; colm <= 2; ++colm)
                    if (str == table[row,colm])
                         goto done;

            Console.WriteLine("{0} not found", str);
            continue;
    done:
            Console.WriteLine("Found {0} at [{1}][{2}]", str, row, colm);
        }
    }
}
```
<span data-ttu-id="2395b-464">`goto` prohlášení se používá k přenosu řízení z vnořené oboru.</span><span class="sxs-lookup"><span data-stu-id="2395b-464">a `goto` statement is used to transfer control out of a nested scope.</span></span>

<span data-ttu-id="2395b-465">Cíl `goto case` příkaz je seznamu příkazů ve těsně uzavírajícím `switch` – příkaz ([příkazu switch](statements.md#the-switch-statement)), obsahující `case` popisek s danou hodnotu konstanty.</span><span class="sxs-lookup"><span data-stu-id="2395b-465">The target of a `goto case` statement is the statement list in the immediately enclosing `switch` statement ([The switch statement](statements.md#the-switch-statement)), which contains a `case` label with the given constant value.</span></span> <span data-ttu-id="2395b-466">Pokud `goto case` příkazu není uzavřená v `switch` příkazu, pokud *constant_expression* není implicitně převést ([implicitní převody](conversions.md#implicit-conversions)) celopodnikové typu nejbližší uzavírající `switch` příkazu, nebo pokud nejbližšího obklopujícího `switch` neobsahuje příkaz `case` popisek s danou konstantní hodnoty, dojde k chybě kompilace.</span><span class="sxs-lookup"><span data-stu-id="2395b-466">If the `goto case` statement is not enclosed by a `switch` statement, if the *constant_expression* is not implicitly convertible ([Implicit conversions](conversions.md#implicit-conversions)) to the governing type of the nearest enclosing `switch` statement, or if the nearest enclosing `switch` statement does not contain a `case` label with the given constant value, a compile-time error occurs.</span></span>

<span data-ttu-id="2395b-467">Cíl `goto default` příkaz je seznamu příkazů ve těsně uzavírajícím `switch` – příkaz ([příkazu switch](statements.md#the-switch-statement)), obsahující `default` popisek.</span><span class="sxs-lookup"><span data-stu-id="2395b-467">The target of a `goto default` statement is the statement list in the immediately enclosing `switch` statement ([The switch statement](statements.md#the-switch-statement)), which contains a `default` label.</span></span> <span data-ttu-id="2395b-468">Pokud `goto default` příkazu není uzavřená v `switch` příkazu, nebo pokud nejbližšího obklopujícího `switch` příkaz neobsahuje `default` popisek, dojde k chybě kompilace.</span><span class="sxs-lookup"><span data-stu-id="2395b-468">If the `goto default` statement is not enclosed by a `switch` statement, or if the nearest enclosing `switch` statement does not contain a `default` label, a compile-time error occurs.</span></span>

<span data-ttu-id="2395b-469">A `goto` příkaz nemůže ukončit `finally` blok ([příkazu try](statements.md#the-try-statement)).</span><span class="sxs-lookup"><span data-stu-id="2395b-469">A `goto` statement cannot exit a `finally` block ([The try statement](statements.md#the-try-statement)).</span></span> <span data-ttu-id="2395b-470">Při `goto` příkazu vyvolá se v rámci `finally` blokovat, cíl `goto` příkaz musí být v rámci stejného `finally` blok, nebo v opačném případě dojde k chybě kompilace.</span><span class="sxs-lookup"><span data-stu-id="2395b-470">When a `goto` statement occurs within a `finally` block, the target of the `goto` statement must be within the same `finally` block, or otherwise a compile-time error occurs.</span></span>

<span data-ttu-id="2395b-471">A `goto` je proveden příkaz takto:</span><span class="sxs-lookup"><span data-stu-id="2395b-471">A `goto` statement is executed as follows:</span></span>

*  <span data-ttu-id="2395b-472">Pokud `goto` příkaz ukončí jeden nebo více `try` přidružené bloky s `finally` bloky, ovládací prvek zpočátku bude převeden na `finally` bloku nejvnitřnější `try` příkaz.</span><span class="sxs-lookup"><span data-stu-id="2395b-472">If the `goto` statement exits one or more `try` blocks with associated `finally` blocks, control is initially transferred to the `finally` block of the innermost `try` statement.</span></span> <span data-ttu-id="2395b-473">Pokud a v případě, že ovládací prvek dosáhne koncového bodu `finally` bloku, ovládací prvek bude převeden na `finally` další uzavírající blok `try` příkazu.</span><span class="sxs-lookup"><span data-stu-id="2395b-473">When and if control reaches the end point of a `finally` block, control is transferred to the `finally` block of the next enclosing `try` statement.</span></span> <span data-ttu-id="2395b-474">Tento proces se opakuje, dokud `finally` všechny bloky použité `try` příkazy byly provedeny.</span><span class="sxs-lookup"><span data-stu-id="2395b-474">This process is repeated until the `finally` blocks of all intervening `try` statements have been executed.</span></span>
*  <span data-ttu-id="2395b-475">Ovládací prvek bude převeden do cílového místa `goto` příkazu.</span><span class="sxs-lookup"><span data-stu-id="2395b-475">Control is transferred to the target of the `goto` statement.</span></span>

<span data-ttu-id="2395b-476">Protože `goto` příkaz bezpodmínečně převede ovládací prvek jinde, koncový bod `goto` příkazu není dostupný.</span><span class="sxs-lookup"><span data-stu-id="2395b-476">Because a `goto` statement unconditionally transfers control elsewhere, the end point of a `goto` statement is never reachable.</span></span>

### <a name="the-return-statement"></a><span data-ttu-id="2395b-477">Příkaz return</span><span class="sxs-lookup"><span data-stu-id="2395b-477">The return statement</span></span>

<span data-ttu-id="2395b-478">`return` Příkaz vrátí řízení volajícímu aktuální funkce, ve kterém `return` objevuje příkaz.</span><span class="sxs-lookup"><span data-stu-id="2395b-478">The `return` statement returns control to the current caller of the function in which the `return` statement appears.</span></span>

```antlr
return_statement
    : 'return' expression? ';'
    ;
```

<span data-ttu-id="2395b-479">A `return` příkaz s žádný výraz lze použít pouze v funkce členu, který nelze vypočítat hodnotu tedy metoda s výsledným typem ([tělo metody](classes.md#method-body)) `void`, `set` přistupující objekt vlastnosti nebo indexer, který je `add` a `remove` přistupující objekty událost, konstruktor instance, statický konstruktor nebo destruktor.</span><span class="sxs-lookup"><span data-stu-id="2395b-479">A `return` statement with no expression can be used only in a function member that does not compute a value, that is, a method with the result type ([Method body](classes.md#method-body)) `void`, the `set` accessor of a property or indexer, the `add` and `remove` accessors of an event, an instance constructor, a static constructor, or a destructor.</span></span>

<span data-ttu-id="2395b-480">A `return` příkaz s výrazem jde použít jenom v členské funkci, která vypočítá hodnotu tedy metoda s typem výsledku není void, `get` přistupující objekt vlastnosti nebo indexeru nebo uživatelem definovaný operátor.</span><span class="sxs-lookup"><span data-stu-id="2395b-480">A `return` statement with an expression can only be used in a function member that computes a value, that is, a method with a non-void result type, the `get` accessor of a property or indexer, or a user-defined operator.</span></span> <span data-ttu-id="2395b-481">Implicitní převod ([implicitních převodů](conversions.md#implicit-conversions)) z typu výrazu, musí existovat na návratový typ obsahující členské funkce.</span><span class="sxs-lookup"><span data-stu-id="2395b-481">An implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)) must exist from the type of the expression to the return type of the containing function member.</span></span>

<span data-ttu-id="2395b-482">Vrátí příkazy lze použít také v těle výrazu anonymní funkce ([výrazy anonymní funkce](expressions.md#anonymous-function-expressions)) a účast při určování, které převody existovat pro tyto funkce.</span><span class="sxs-lookup"><span data-stu-id="2395b-482">Return statements can also be used in the body of anonymous function expressions ([Anonymous function expressions](expressions.md#anonymous-function-expressions)), and participate in determining which conversions exist for those functions.</span></span>

<span data-ttu-id="2395b-483">Je chyba kompilace pro `return` příkazu se zobrazí v `finally` blok ([příkazu try](statements.md#the-try-statement)).</span><span class="sxs-lookup"><span data-stu-id="2395b-483">It is a compile-time error for a `return` statement to appear in a `finally` block ([The try statement](statements.md#the-try-statement)).</span></span>

<span data-ttu-id="2395b-484">A `return` je proveden příkaz takto:</span><span class="sxs-lookup"><span data-stu-id="2395b-484">A `return` statement is executed as follows:</span></span>

*  <span data-ttu-id="2395b-485">Pokud `return` příkaz Určuje výraz, výraz je vyhodnocen a výsledná hodnota je převedena na typ vrácené hodnoty funkce obsahující podle implicitní převod.</span><span class="sxs-lookup"><span data-stu-id="2395b-485">If the `return` statement specifies an expression, the expression is evaluated and the resulting value is converted to the return type of the containing function by an implicit conversion.</span></span> <span data-ttu-id="2395b-486">Výsledkem převodu se změní hodnota výsledku vytvořeného funkcí.</span><span class="sxs-lookup"><span data-stu-id="2395b-486">The result of the conversion becomes the result value produced by the function.</span></span>
*  <span data-ttu-id="2395b-487">Pokud `return` příkazu není uzavřen v jedné nebo více `try` nebo `catch` přidružené bloky s `finally` bloky, ovládací prvek zpočátku bude převeden na `finally` bloku nejvnitřnější `try` příkaz.</span><span class="sxs-lookup"><span data-stu-id="2395b-487">If the `return` statement is enclosed by one or more `try` or `catch` blocks with associated `finally` blocks, control is initially transferred to the `finally` block of the innermost `try` statement.</span></span> <span data-ttu-id="2395b-488">Pokud a v případě, že ovládací prvek dosáhne koncového bodu `finally` bloku, ovládací prvek bude převeden na `finally` další uzavírající blok `try` příkazu.</span><span class="sxs-lookup"><span data-stu-id="2395b-488">When and if control reaches the end point of a `finally` block, control is transferred to the `finally` block of the next enclosing `try` statement.</span></span> <span data-ttu-id="2395b-489">Tento proces se opakuje, dokud `finally` blokuje všechny nadřazené `try` příkazy byly provedeny.</span><span class="sxs-lookup"><span data-stu-id="2395b-489">This process is repeated until the `finally` blocks of all enclosing `try` statements have been executed.</span></span>
*  <span data-ttu-id="2395b-490">Pokud obsahující funkce není asynchronní funkce, ovládací prvek se vrátí volajícímu metody obsahující funkce včetně hodnoty výsledku pokud existuje.</span><span class="sxs-lookup"><span data-stu-id="2395b-490">If the containing function is not an async function, control is returned to the caller of the containing function along with the result value, if any.</span></span>
*  <span data-ttu-id="2395b-491">Pokud je asynchronní funkce obsahující funkce, ovládací prvek se vrátí volajícímu aktuální a výslednou hodnotu, pokud existuje, je zaznamenán v Vrácená úloha jak je popsáno v ([rozhraní pro výčty](classes.md#enumerator-interfaces)).</span><span class="sxs-lookup"><span data-stu-id="2395b-491">If the containing function is an async function, control is returned to the current caller, and the result value, if any, is recorded in the return task as described in ([Enumerator interfaces](classes.md#enumerator-interfaces)).</span></span>

<span data-ttu-id="2395b-492">Protože `return` příkaz bezpodmínečně převede ovládací prvek jinde, koncový bod `return` příkazu není dostupný.</span><span class="sxs-lookup"><span data-stu-id="2395b-492">Because a `return` statement unconditionally transfers control elsewhere, the end point of a `return` statement is never reachable.</span></span>

### <a name="the-throw-statement"></a><span data-ttu-id="2395b-493">Příkaz throw</span><span class="sxs-lookup"><span data-stu-id="2395b-493">The throw statement</span></span>

<span data-ttu-id="2395b-494">`throw` Příkazu dojde k výjimce.</span><span class="sxs-lookup"><span data-stu-id="2395b-494">The `throw` statement throws an exception.</span></span>

```antlr
throw_statement
    : 'throw' expression? ';'
    ;
```

<span data-ttu-id="2395b-495">A `throw` Vyvolá příkaz s výrazem hodnotu vytvořenou testovaným vyhodnocení výrazu.</span><span class="sxs-lookup"><span data-stu-id="2395b-495">A `throw` statement with an expression throws the value produced by evaluating the expression.</span></span> <span data-ttu-id="2395b-496">Výraz musí označují hodnotu typu třídy `System.Exception`, typu třídy, která je odvozena z `System.Exception` nebo typu parametru typu, který má `System.Exception` (nebo jejich podtřída) jako jeho základní třída.</span><span class="sxs-lookup"><span data-stu-id="2395b-496">The expression must denote a value of the class type `System.Exception`, of a class type that derives from `System.Exception` or of a type parameter type that has `System.Exception` (or a subclass thereof) as its effective base class.</span></span> <span data-ttu-id="2395b-497">V případě vyhodnocování výrazu vytvoří `null`, `System.NullReferenceException` místo toho je vyvolána výjimka.</span><span class="sxs-lookup"><span data-stu-id="2395b-497">If evaluation of the expression produces `null`, a `System.NullReferenceException` is thrown instead.</span></span>

<span data-ttu-id="2395b-498">A `throw` příkaz s žádný výraz se dá použít jenom ve `catch` blokovat, v takovém případě, který tento příkaz znovu vyvolá výjimku, která je aktuálně zpracovávanou díky tomu `catch` bloku.</span><span class="sxs-lookup"><span data-stu-id="2395b-498">A `throw` statement with no expression can be used only in a `catch` block, in which case that statement re-throws the exception that is currently being handled by that `catch` block.</span></span>

<span data-ttu-id="2395b-499">Protože `throw` příkaz bezpodmínečně převede ovládací prvek jinde, koncový bod `throw` příkazu není dostupný.</span><span class="sxs-lookup"><span data-stu-id="2395b-499">Because a `throw` statement unconditionally transfers control elsewhere, the end point of a `throw` statement is never reachable.</span></span>

<span data-ttu-id="2395b-500">Když je vyvolána výjimka, ovládací prvek bude převeden na první `catch` klauzuli v nadřazeném `try` příkaz, který může zpracovat výjimku.</span><span class="sxs-lookup"><span data-stu-id="2395b-500">When an exception is thrown, control is transferred to the first `catch` clause in an enclosing `try` statement that can handle the exception.</span></span> <span data-ttu-id="2395b-501">Proces, který se provádí z bodu výjimky do místa přenos řízení na obslužnou rutinu vhodná výjimka se označuje jako ***šíření výjimek***.</span><span class="sxs-lookup"><span data-stu-id="2395b-501">The process that takes place from the point of the exception being thrown to the point of transferring control to a suitable exception handler is known as ***exception propagation***.</span></span> <span data-ttu-id="2395b-502">Šíření výjimky se skládá z opakovaného vyhodnocování následující kroky, dokud `catch` nenajde klauzuli, která odpovídá výjimku.</span><span class="sxs-lookup"><span data-stu-id="2395b-502">Propagation of an exception consists of repeatedly evaluating the following steps until a `catch` clause that matches the exception is found.</span></span> <span data-ttu-id="2395b-503">V tomto popisu ***throw bodu*** je zpočátku umístění, ve kterém je vyvolána výjimka.</span><span class="sxs-lookup"><span data-stu-id="2395b-503">In this description, the ***throw point*** is initially the location at which the exception is thrown.</span></span>

*  <span data-ttu-id="2395b-504">V aktuální funkce členu každý `try` příkaz, který obklopuje bodu throw je zkontrolován.</span><span class="sxs-lookup"><span data-stu-id="2395b-504">In the current function member, each `try` statement that encloses the throw point is examined.</span></span> <span data-ttu-id="2395b-505">Příkazu for each `S`počínaje nejvnitřnější `try` příkazu a konče nejkrajnější `try` prohlášení, jsou vyhodnocovány následující kroky:</span><span class="sxs-lookup"><span data-stu-id="2395b-505">For each statement `S`, starting with the innermost `try` statement and ending with the outermost `try` statement, the following steps are evaluated:</span></span>

   * <span data-ttu-id="2395b-506">Pokud `try` bloku `S` obklopuje bodu throw a pokud S má jeden nebo více `catch` klauzulí se operátor `catch` klauzule jsou zkoumány podle pořadí vzhled pro vyhledání vhodné obslužné rutiny pro výjimky, podle pravidel zadaných v Část [příkazu try](statements.md#the-try-statement).</span><span class="sxs-lookup"><span data-stu-id="2395b-506">If the `try` block of `S` encloses the throw point and if S has one or more `catch` clauses, the `catch` clauses are examined in order of appearance to locate a suitable handler for the exception, according to the rules specified in Section [The try statement](statements.md#the-try-statement).</span></span> <span data-ttu-id="2395b-507">Pokud odpovídající `catch` klauzule nachází, šíření výjimek je dokončit přenos řízení na blok, který `catch` klauzuli.</span><span class="sxs-lookup"><span data-stu-id="2395b-507">If a matching `catch` clause is located, the exception propagation is completed by transferring control to the block of that `catch` clause.</span></span>

   * <span data-ttu-id="2395b-508">Jinak, pokud `try` bloku nebo `catch` bloku `S` obklopuje bodu throw a pokud `S` má `finally` bloku, ovládací prvek bude převeden na `finally` bloku.</span><span class="sxs-lookup"><span data-stu-id="2395b-508">Otherwise, if the `try` block or a `catch` block of `S` encloses the throw point and if `S` has a `finally` block, control is transferred to the `finally` block.</span></span> <span data-ttu-id="2395b-509">Pokud `finally` bloku vyvolá další výjimku, se ukončí zpracování aktuální výjimky.</span><span class="sxs-lookup"><span data-stu-id="2395b-509">If the `finally` block throws another exception, processing of the current exception is terminated.</span></span> <span data-ttu-id="2395b-510">Jinak, když ovládací prvek dosáhne koncového bodu `finally` bloku, pokračovat zpracování aktuální výjimky.</span><span class="sxs-lookup"><span data-stu-id="2395b-510">Otherwise, when control reaches the end point of the `finally` block, processing of the current exception is continued.</span></span>

*  <span data-ttu-id="2395b-511">Pokud nebyl v aktuálním volání funkce obslužné rutiny výjimek, volání funkce se ukončí a dojde k jedné z následujících akcí:</span><span class="sxs-lookup"><span data-stu-id="2395b-511">If an exception handler was not located in the current function invocation, the function invocation is terminated, and one of the following occurs:</span></span>

   * <span data-ttu-id="2395b-512">Pokud je aktuální funkce není asynchronní, se opakují výše uvedených kroků pro volající funkci s bodem throw odpovídající příkaz, ze kterého byla vyvolána členské funkce.</span><span class="sxs-lookup"><span data-stu-id="2395b-512">If the current function is non-async, the steps above are repeated for the caller of the function with a throw point corresponding to the statement from which the function member was invoked.</span></span>

   * <span data-ttu-id="2395b-513">Pokud je aktuální funkce async a vracející úlohy, výjimka se zaznamená do vrácené úlohou, které přejde do stavu chybovém nebo bylo zrušeno, jak je popsáno v [rozhraní pro výčty](classes.md#enumerator-interfaces).</span><span class="sxs-lookup"><span data-stu-id="2395b-513">If the current function is async and task-returning, the exception is recorded in the return task, which is put into a faulted or cancelled state as described in [Enumerator interfaces](classes.md#enumerator-interfaces).</span></span>

   * <span data-ttu-id="2395b-514">Pokud aktuální funkci je asynchronní a vracející hodnotu void, je oznámení kontext synchronizace aktuálního vlákna, jak je popsáno v [vyčíslitelná rozhraní](classes.md#enumerable-interfaces).</span><span class="sxs-lookup"><span data-stu-id="2395b-514">If the current function is async and void-returning, the synchronization context of the current thread is notified as described in [Enumerable interfaces](classes.md#enumerable-interfaces).</span></span>

*  <span data-ttu-id="2395b-515">Zpracování výjimek ukončí všechny volání členské funkce v aktuálním vlákně, označující, že vlákno nemá žádná obslužná rutina výjimky, pak vlákna je ukončen.</span><span class="sxs-lookup"><span data-stu-id="2395b-515">If the exception processing terminates all function member invocations in the current thread, indicating that the thread has no handler for the exception, then the thread is itself terminated.</span></span> <span data-ttu-id="2395b-516">Dopad ukončení, je definováno implementací.</span><span class="sxs-lookup"><span data-stu-id="2395b-516">The impact of such termination is implementation-defined.</span></span>

## <a name="the-try-statement"></a><span data-ttu-id="2395b-517">Příkaz try</span><span class="sxs-lookup"><span data-stu-id="2395b-517">The try statement</span></span>

<span data-ttu-id="2395b-518">`try` Příkaz poskytuje mechanismus pro zachycování výjimek, ke kterým dochází při provádění bloku.</span><span class="sxs-lookup"><span data-stu-id="2395b-518">The `try` statement provides a mechanism for catching exceptions that occur during execution of a block.</span></span> <span data-ttu-id="2395b-519">Kromě toho `try` poskytuje možnost určit blok kódu, který je vždy spuštěn, když ovládací prvek opustí příkaz `try` příkazu.</span><span class="sxs-lookup"><span data-stu-id="2395b-519">Furthermore, the `try` statement provides the ability to specify a block of code that is always executed when control leaves the `try` statement.</span></span>

```antlr
try_statement
    : 'try' block catch_clause+
    | 'try' block finally_clause
    | 'try' block catch_clause+ finally_clause
    ;

catch_clause
    : 'catch' exception_specifier? exception_filter?  block
    ;

exception_specifier
    : '(' type identifier? ')'
    ;

exception_filter
    : 'when' '(' expression ')'
    ;

finally_clause
    : 'finally' block
    ;
```

<span data-ttu-id="2395b-520">Existují tři možné formy `try` příkazy:</span><span class="sxs-lookup"><span data-stu-id="2395b-520">There are three possible forms of `try` statements:</span></span>

*  <span data-ttu-id="2395b-521">A `try` bloku, za nímž následuje jedna nebo více `catch` bloky.</span><span class="sxs-lookup"><span data-stu-id="2395b-521">A `try` block followed by one or more `catch` blocks.</span></span>
*  <span data-ttu-id="2395b-522">A `try` bloku, za nímž následuje `finally` bloku.</span><span class="sxs-lookup"><span data-stu-id="2395b-522">A `try` block followed by a `finally` block.</span></span>
*  <span data-ttu-id="2395b-523">A `try` bloku, za nímž následuje jedna nebo více `catch` bloky, za nímž následuje `finally` bloku.</span><span class="sxs-lookup"><span data-stu-id="2395b-523">A `try` block followed by one or more `catch` blocks followed by a `finally` block.</span></span>

<span data-ttu-id="2395b-524">Při `catch` určuje klauzuli *exception_specifier*, musí být typu `System.Exception`, typ, který je odvozen od `System.Exception` nebo typ parametru typu, který má `System.Exception` (nebo jejich podtřída) jako jeho platit Základní třída.</span><span class="sxs-lookup"><span data-stu-id="2395b-524">When a `catch` clause specifies an *exception_specifier*, the type must be `System.Exception`, a type that derives from `System.Exception` or a type parameter type that has `System.Exception` (or a subclass thereof) as its effective base class.</span></span>

<span data-ttu-id="2395b-525">Při `catch` klauzule určuje, jak *exception_specifier* s *identifikátor*, ***proměnné exception*** zadaný název a typ je deklarován.</span><span class="sxs-lookup"><span data-stu-id="2395b-525">When a `catch` clause specifies both an *exception_specifier* with an *identifier*, an ***exception variable*** of the given name and type is declared.</span></span> <span data-ttu-id="2395b-526">Proměnné exception odpovídá na místní proměnnou s rozsahem, který rozšiřuje přes `catch` klauzuli.</span><span class="sxs-lookup"><span data-stu-id="2395b-526">The exception variable corresponds to a local variable with a scope that extends over the `catch` clause.</span></span> <span data-ttu-id="2395b-527">Během provádění *exception_filter* a *bloku*, proměnné exception představuje aktuálně zpracovávanou výjimku.</span><span class="sxs-lookup"><span data-stu-id="2395b-527">During execution of the *exception_filter* and *block*, the exception variable represents the exception currently being handled.</span></span> <span data-ttu-id="2395b-528">Pro účely kontroly jednoznačného přiřazení proměnné exception se považuje za jednoznačně přiřazena v jeho celý rozsah.</span><span class="sxs-lookup"><span data-stu-id="2395b-528">For purposes of definite assignment checking, the exception variable is considered definitely assigned in its entire scope.</span></span>

<span data-ttu-id="2395b-529">Pokud `catch` klauzule obsahuje název proměnné výjimky, není možné získat přístup k objektu výjimka ve filtru a `catch` bloku.</span><span class="sxs-lookup"><span data-stu-id="2395b-529">Unless a `catch` clause includes an exception variable name, it is impossible to access the exception object in the filter and `catch` block.</span></span>

<span data-ttu-id="2395b-530">A `catch` klauzuli, která neurčuje *exception_specifier* nazývá Obecné `catch` klauzuli.</span><span class="sxs-lookup"><span data-stu-id="2395b-530">A `catch` clause that does not specify an *exception_specifier* is called a general `catch` clause.</span></span>

<span data-ttu-id="2395b-531">Některé z nich můžou podporovat výjimky, které nejsou reprezentovat objekt odvozena z `System.Exception`, i když tyto výjimky může nebudou nikdy generována pomocí kódu jazyka C#.</span><span class="sxs-lookup"><span data-stu-id="2395b-531">Some programming languages may support exceptions that are not representable as an object derived from `System.Exception`, although such exceptions could never be generated by C# code.</span></span> <span data-ttu-id="2395b-532">Obecné `catch` klauzuli může použít k zachycení takové výjimky.</span><span class="sxs-lookup"><span data-stu-id="2395b-532">A general `catch` clause may be used to catch such exceptions.</span></span> <span data-ttu-id="2395b-533">Proto Obecné `catch` je klauzule sémanticky rozdílnými z jednoho, který určuje typ `System.Exception`, v tom, že dříve uvedené může také zachytit výjimky z jiných jazyků.</span><span class="sxs-lookup"><span data-stu-id="2395b-533">Thus, a general `catch` clause is semantically different from one that specifies the type `System.Exception`, in that the former may also catch exceptions from other languages.</span></span>

<span data-ttu-id="2395b-534">Aby bylo možné najít obslužnou rutinu výjimky, `catch` klauzule jsou zkoumány podle pořadí slov.</span><span class="sxs-lookup"><span data-stu-id="2395b-534">In order to locate a handler for an exception, `catch` clauses are examined in lexical order.</span></span> <span data-ttu-id="2395b-535">Pokud `catch` klauzule určuje typ, ale žádný filtr výjimek, je chyba kompilace pro pozdější `catch` klauzule ve stejném `try` příkaz a zadejte typ, který je stejný jako, nebo je odvozen z typu.</span><span class="sxs-lookup"><span data-stu-id="2395b-535">If a `catch` clause specifies a type but no exception filter, it is a compile-time error for a later `catch` clause in the same `try` statement to specify a type that is the same as, or is derived from, that type.</span></span> <span data-ttu-id="2395b-536">Pokud `catch` klauzule určuje žádný typ a žádný filtr, musí být poslední `catch` klauzuli, která `try` příkazu.</span><span class="sxs-lookup"><span data-stu-id="2395b-536">If a `catch` clause specifies no type and no filter, it must be the last `catch` clause for that `try` statement.</span></span>

<span data-ttu-id="2395b-537">V rámci `catch` bloku, `throw` – příkaz ([příkazu throw](statements.md#the-throw-statement)) s žádný výraz lze použít pro opětovné vyvolání výjimky, která byla zachycena `catch` bloku.</span><span class="sxs-lookup"><span data-stu-id="2395b-537">Within a `catch` block, a `throw` statement ([The throw statement](statements.md#the-throw-statement)) with no expression can be used to re-throw the exception that was caught by the `catch` block.</span></span> <span data-ttu-id="2395b-538">Přiřazení proměnné výjimka nemění, který je znovu vyvolána výjimka.</span><span class="sxs-lookup"><span data-stu-id="2395b-538">Assignments to an exception variable do not alter the exception that is re-thrown.</span></span>

<span data-ttu-id="2395b-539">V příkladu</span><span class="sxs-lookup"><span data-stu-id="2395b-539">In the example</span></span>
```csharp
using System;

class Test
{
    static void F() {
        try {
            G();
        }
        catch (Exception e) {
            Console.WriteLine("Exception in F: " + e.Message);
            e = new Exception("F");
            throw;                // re-throw
        }
    }

    static void G() {
        throw new Exception("G");
    }

    static void Main() {
        try {
            F();
        }
        catch (Exception e) {
            Console.WriteLine("Exception in Main: " + e.Message);
        }
    }
}
```
<span data-ttu-id="2395b-540">Metoda `F` zachytí výjimku, zapíše některé diagnostické informace do konzoly, mění proměnné exception a znovu vyvolá výjimku.</span><span class="sxs-lookup"><span data-stu-id="2395b-540">the method `F` catches an exception, writes some diagnostic information to the console, alters the exception variable, and re-throws the exception.</span></span> <span data-ttu-id="2395b-541">Výjimka, která je znovu vyvolána je původní výjimky, takže je výstup vytvořený:</span><span class="sxs-lookup"><span data-stu-id="2395b-541">The exception that is re-thrown is the original exception, so the output produced is:</span></span>
```
Exception in F: G
Exception in Main: G
```

<span data-ttu-id="2395b-542">Pokud měl vyvolána první blok catch `e` místo opětné vyvolání aktuální výjimky, výstup vytvořený by měl vypadat takto:</span><span class="sxs-lookup"><span data-stu-id="2395b-542">If the first catch block had thrown `e` instead of rethrowing the current exception, the output produced would be as follows:</span></span>
```csharp
Exception in F: G
Exception in Main: F
```

<span data-ttu-id="2395b-543">Je chyba kompilace pro `break`, `continue`, nebo `goto` příkazu k předání řízení z celkového počtu `finally` bloku.</span><span class="sxs-lookup"><span data-stu-id="2395b-543">It is a compile-time error for a `break`, `continue`, or `goto` statement to transfer control out of a `finally` block.</span></span> <span data-ttu-id="2395b-544">Při `break`, `continue`, nebo `goto` příkaz probíhá `finally` bloku, cílem příkazu musí být v rámci stejného `finally` bloku, nebo v opačném případě dojde k chybě kompilace.</span><span class="sxs-lookup"><span data-stu-id="2395b-544">When a `break`, `continue`, or `goto` statement occurs in a `finally` block, the target of the statement must be within the same `finally` block, or otherwise a compile-time error occurs.</span></span>

<span data-ttu-id="2395b-545">Je chyba kompilace pro `return` vyskytuje v příkazu `finally` bloku.</span><span class="sxs-lookup"><span data-stu-id="2395b-545">It is a compile-time error for a `return` statement to occur in a `finally` block.</span></span>

<span data-ttu-id="2395b-546">A `try` je proveden příkaz takto:</span><span class="sxs-lookup"><span data-stu-id="2395b-546">A `try` statement is executed as follows:</span></span>

*  <span data-ttu-id="2395b-547">Ovládací prvek bude převeden na `try` bloku.</span><span class="sxs-lookup"><span data-stu-id="2395b-547">Control is transferred to the `try` block.</span></span>
*  <span data-ttu-id="2395b-548">Pokud a v případě, že ovládací prvek dosáhne koncového bodu `try` blok:</span><span class="sxs-lookup"><span data-stu-id="2395b-548">When and if control reaches the end point of the `try` block:</span></span>
   *  <span data-ttu-id="2395b-549">Pokud `try` příkaz má `finally` bloku `finally` je blok proveden.</span><span class="sxs-lookup"><span data-stu-id="2395b-549">If the `try` statement has a `finally` block, the `finally` block is executed.</span></span>
   *  <span data-ttu-id="2395b-550">Ovládací prvek bude převeden na koncový bod `try` příkazu.</span><span class="sxs-lookup"><span data-stu-id="2395b-550">Control is transferred to the end point of the `try` statement.</span></span>

*  <span data-ttu-id="2395b-551">Pokud se výjimka šíří do `try` příkaz během provádění `try` blok:</span><span class="sxs-lookup"><span data-stu-id="2395b-551">If an exception is propagated to the `try` statement during execution of the `try` block:</span></span>
   *  <span data-ttu-id="2395b-552">`catch` Klauzule, pokud existují, jsou zkoumány podle pořadí vzhled najít vhodné obslužné rutiny výjimky.</span><span class="sxs-lookup"><span data-stu-id="2395b-552">The `catch` clauses, if any, are examined in order of appearance to locate a suitable handler for the exception.</span></span> <span data-ttu-id="2395b-553">Pokud `catch` klauzule není zadán typ nebo určuje typ výjimky nebo základní typ typu výjimky:</span><span class="sxs-lookup"><span data-stu-id="2395b-553">If a `catch` clause does not specify a type, or specifies the exception type or a base type of the exception type:</span></span>
      *  <span data-ttu-id="2395b-554">Pokud `catch` klauzule deklaruje proměnnou výjimky, je přiřazen objekt výjimky do proměnné výjimky.</span><span class="sxs-lookup"><span data-stu-id="2395b-554">If the `catch` clause declares an exception variable, the exception object is assigned to the exception variable.</span></span>
      *  <span data-ttu-id="2395b-555">Pokud `catch` deklaruje klauzule filtru výjimky, je vyhodnocen filtru.</span><span class="sxs-lookup"><span data-stu-id="2395b-555">If the `catch` clause declares an exception filter, the filter is evaluated.</span></span> <span data-ttu-id="2395b-556">Pokud je vyhodnocen jako `false`klauzule catch není shoda a pokračuje v hledání prostřednictvím libovolného následné `catch` klauzule pro obslužnou rutinu vhodná.</span><span class="sxs-lookup"><span data-stu-id="2395b-556">If it evaluates to `false`, the catch clause is not a match, and the search continues through any subsequent `catch` clauses for a suitable handler.</span></span>
      *  <span data-ttu-id="2395b-557">V opačném případě `catch` klauzule považován za shodný a ovládací prvek bude převeden na odpovídající `catch` bloku.</span><span class="sxs-lookup"><span data-stu-id="2395b-557">Otherwise, the `catch` clause is considered a match, and control is transferred to the matching `catch` block.</span></span>
      *  <span data-ttu-id="2395b-558">Pokud a v případě, že ovládací prvek dosáhne koncového bodu `catch` blok:</span><span class="sxs-lookup"><span data-stu-id="2395b-558">When and if control reaches the end point of the `catch` block:</span></span>
         * <span data-ttu-id="2395b-559">Pokud `try` příkaz má `finally` bloku `finally` je blok proveden.</span><span class="sxs-lookup"><span data-stu-id="2395b-559">If the `try` statement has a `finally` block, the `finally` block is executed.</span></span>
         * <span data-ttu-id="2395b-560">Ovládací prvek bude převeden na koncový bod `try` příkazu.</span><span class="sxs-lookup"><span data-stu-id="2395b-560">Control is transferred to the end point of the `try` statement.</span></span>
      *  <span data-ttu-id="2395b-561">Pokud se výjimka šíří do `try` příkaz během provádění `catch` blok:</span><span class="sxs-lookup"><span data-stu-id="2395b-561">If an exception is propagated to the `try` statement during execution of the `catch` block:</span></span>
         *  <span data-ttu-id="2395b-562">Pokud `try` příkaz má `finally` bloku `finally` je blok proveden.</span><span class="sxs-lookup"><span data-stu-id="2395b-562">If the `try` statement has a `finally` block, the `finally` block is executed.</span></span>
         *  <span data-ttu-id="2395b-563">Výjimka se šíří do další uzavírající `try` příkazu.</span><span class="sxs-lookup"><span data-stu-id="2395b-563">The exception is propagated to the next enclosing `try` statement.</span></span>
   *  <span data-ttu-id="2395b-564">Pokud `try` příkazu nemá žádné `catch` klauzule nebo pokud žádná `catch` klauzule odpovídá výjimku:</span><span class="sxs-lookup"><span data-stu-id="2395b-564">If the `try` statement has no `catch` clauses or if no `catch` clause matches the exception:</span></span>
      *  <span data-ttu-id="2395b-565">Pokud `try` příkaz má `finally` bloku `finally` je blok proveden.</span><span class="sxs-lookup"><span data-stu-id="2395b-565">If the `try` statement has a `finally` block, the `finally` block is executed.</span></span>
      *  <span data-ttu-id="2395b-566">Výjimka se šíří do další uzavírající `try` příkazu.</span><span class="sxs-lookup"><span data-stu-id="2395b-566">The exception is propagated to the next enclosing `try` statement.</span></span>

<span data-ttu-id="2395b-567">Příkazy `finally` bloku jsou provedeny vždy, když ovládací prvek opustí `try` příkazu.</span><span class="sxs-lookup"><span data-stu-id="2395b-567">The statements of a `finally` block are always executed when control leaves a `try` statement.</span></span> <span data-ttu-id="2395b-568">Je hodnota true Určuje, zda přenos ovládacího prvku dojde v důsledku normálního provedení, v důsledku spuštění `break`, `continue`, `goto`, nebo `return` příkazu, nebo v důsledku šíření výjimky z `try` příkaz.</span><span class="sxs-lookup"><span data-stu-id="2395b-568">This is true whether the control transfer occurs as a result of normal execution, as a result of executing a `break`, `continue`, `goto`, or `return` statement, or as a result of propagating an exception out of the `try` statement.</span></span>

<span data-ttu-id="2395b-569">Pokud dojde k výjimce za běhu `finally` blok a není zachycena v rámci stejného bloku finally, se výjimka šíří do další značka `try` příkazu.</span><span class="sxs-lookup"><span data-stu-id="2395b-569">If an exception is thrown during execution of a `finally` block, and is not caught within the same finally block, the exception is propagated to the next enclosing `try` statement.</span></span> <span data-ttu-id="2395b-570">Pokud právě rozšířen byla další výjimku, tato výjimka bude ztracena.</span><span class="sxs-lookup"><span data-stu-id="2395b-570">If another exception was in the process of being propagated, that exception is lost.</span></span> <span data-ttu-id="2395b-571">Proces šíření výjimky je popsána dále v popisu `throw` – příkaz ([příkazu throw](statements.md#the-throw-statement)).</span><span class="sxs-lookup"><span data-stu-id="2395b-571">The process of propagating an exception is discussed further in the description of the `throw` statement ([The throw statement](statements.md#the-throw-statement)).</span></span>

<span data-ttu-id="2395b-572">`try` Bloku `try` příkaz je dostupný Pokud `try` příkaz je dostupný.</span><span class="sxs-lookup"><span data-stu-id="2395b-572">The `try` block of a `try` statement is reachable if the `try` statement is reachable.</span></span>

<span data-ttu-id="2395b-573">A `catch` bloku `try` příkaz je dostupný Pokud `try` příkaz je dostupný.</span><span class="sxs-lookup"><span data-stu-id="2395b-573">A `catch` block of a `try` statement is reachable if the `try` statement is reachable.</span></span>

<span data-ttu-id="2395b-574">`finally` Bloku `try` příkaz je dostupný Pokud `try` příkaz je dostupný.</span><span class="sxs-lookup"><span data-stu-id="2395b-574">The `finally` block of a `try` statement is reachable if the `try` statement is reachable.</span></span>

<span data-ttu-id="2395b-575">Koncový bod `try` příkaz je dostupný, pokud jsou splněny obě z následujících akcí:</span><span class="sxs-lookup"><span data-stu-id="2395b-575">The end point of a `try` statement is reachable if both of the following are true:</span></span>

*  <span data-ttu-id="2395b-576">Koncový bod `try` blok je dostupný nebo koncový bod alespoň jeden `catch` blok je dostupný.</span><span class="sxs-lookup"><span data-stu-id="2395b-576">The end point of the `try` block is reachable or the end point of at least one `catch` block is reachable.</span></span>
*  <span data-ttu-id="2395b-577">Pokud `finally` blok je k dispozici, koncový bod `finally` blok je dostupný.</span><span class="sxs-lookup"><span data-stu-id="2395b-577">If a `finally` block is present, the end point of the `finally` block is reachable.</span></span>

## <a name="the-checked-and-unchecked-statements"></a><span data-ttu-id="2395b-578">Příkazy zaškrtnuto a nezaškrtnuto</span><span class="sxs-lookup"><span data-stu-id="2395b-578">The checked and unchecked statements</span></span>

<span data-ttu-id="2395b-579">`checked` a `unchecked` příkazy se používají k řízení ***kontextu kontroly přetečení*** integrálového typu aritmetické operace a převody.</span><span class="sxs-lookup"><span data-stu-id="2395b-579">The `checked` and `unchecked` statements are used to control the ***overflow checking context*** for integral-type arithmetic operations and conversions.</span></span>

```antlr
checked_statement
    : 'checked' block
    ;

unchecked_statement
    : 'unchecked' block
    ;
```

<span data-ttu-id="2395b-580">`checked` Příkaz způsobí, že všechny výrazy v *bloku* který se má vyhodnotit ve zkontrolovaném kontextu a `unchecked` příkaz způsobí, že všechny výrazy v *bloku* má být vyhodnocen v nezkontrolovaném kontextu.</span><span class="sxs-lookup"><span data-stu-id="2395b-580">The `checked` statement causes all expressions in the *block* to be evaluated in a checked context, and the `unchecked` statement causes all expressions in the *block* to be evaluated in an unchecked context.</span></span>

<span data-ttu-id="2395b-581">`checked` a `unchecked` příkazy jsou přesně odpovídá `checked` a `unchecked` operátory ([operátory zaškrtnuto a nezaškrtnuto](expressions.md#the-checked-and-unchecked-operators)), s tím rozdílem, že tyto nástroje fungují na bloky namísto výrazů .</span><span class="sxs-lookup"><span data-stu-id="2395b-581">The `checked` and `unchecked` statements are precisely equivalent to the `checked` and `unchecked` operators ([The checked and unchecked operators](expressions.md#the-checked-and-unchecked-operators)), except that they operate on blocks instead of expressions.</span></span>

## <a name="the-lock-statement"></a><span data-ttu-id="2395b-582">Příkaz lock</span><span class="sxs-lookup"><span data-stu-id="2395b-582">The lock statement</span></span>

<span data-ttu-id="2395b-583">`lock` Příkaz získá zámek pro vzájemné vyloučení pro daný objekt, provede příkaz a poté uvolní zámek.</span><span class="sxs-lookup"><span data-stu-id="2395b-583">The `lock` statement obtains the mutual-exclusion lock for a given object, executes a statement, and then releases the lock.</span></span>

```antlr
lock_statement
    : 'lock' '(' expression ')' embedded_statement
    ;
```

<span data-ttu-id="2395b-584">Výraz `lock` příkaz musí označují hodnotu typu známé jako *reference_type*.</span><span class="sxs-lookup"><span data-stu-id="2395b-584">The expression of a `lock` statement must denote a value of a type known to be a *reference_type*.</span></span> <span data-ttu-id="2395b-585">Žádný převod na uzavřené určení implicitní ([zabalení převody](conversions.md#boxing-conversions)) je nikdy neprováděl pro výraz `lock` příkaz a proto je chyba kompilace pro výraz, který má označení hodnotu *value_type*.</span><span class="sxs-lookup"><span data-stu-id="2395b-585">No implicit boxing conversion ([Boxing conversions](conversions.md#boxing-conversions)) is ever performed for the expression of a `lock` statement, and thus it is a compile-time error for the expression to denote a value of a *value_type*.</span></span>

<span data-ttu-id="2395b-586">A `lock` příkaz formuláře</span><span class="sxs-lookup"><span data-stu-id="2395b-586">A `lock` statement of the form</span></span>
```csharp
lock (x) ...
```
<span data-ttu-id="2395b-587">kde `x` je výraz *reference_type*, přesně odpovídá</span><span class="sxs-lookup"><span data-stu-id="2395b-587">where `x` is an expression of a *reference_type*, is precisely equivalent to</span></span>
```csharp
bool __lockWasTaken = false;
try {
    System.Threading.Monitor.Enter(x, ref __lockWasTaken);
    ...
}
finally {
    if (__lockWasTaken) System.Threading.Monitor.Exit(x);
}
```
<span data-ttu-id="2395b-588">s tím rozdílem, že `x` se jenom vyhodnotí jednou.</span><span class="sxs-lookup"><span data-stu-id="2395b-588">except that `x` is only evaluated once.</span></span>

<span data-ttu-id="2395b-589">Dokud je držen zámek vzájemné vyloučení, provádění kódu ve stejném vláknu spuštění můžete také získat a uvolní zámek.</span><span class="sxs-lookup"><span data-stu-id="2395b-589">While a mutual-exclusion lock is held, code executing in the same execution thread can also obtain and release the lock.</span></span> <span data-ttu-id="2395b-590">Nicméně spuštění kódu v jiných vláken se blokuje získání zámku, dokud se zámek je uvolněn.</span><span class="sxs-lookup"><span data-stu-id="2395b-590">However, code executing in other threads is blocked from obtaining the lock until the lock is released.</span></span>

<span data-ttu-id="2395b-591">Uzamčení `System.Type` objektů, aby se synchronizovaly přístup ke statickým datům se nedoporučuje.</span><span class="sxs-lookup"><span data-stu-id="2395b-591">Locking `System.Type` objects in order to synchronize access to static data is not recommended.</span></span> <span data-ttu-id="2395b-592">Další kód může zamknout na stejný typ, který může vést k zablokování.</span><span class="sxs-lookup"><span data-stu-id="2395b-592">Other code might lock on the same type, which can result in deadlock.</span></span> <span data-ttu-id="2395b-593">Lepším řešením je synchronizovat přístup ke statickým datům zamčením privátní statický objekt.</span><span class="sxs-lookup"><span data-stu-id="2395b-593">A better approach is to synchronize access to static data by locking a private static object.</span></span> <span data-ttu-id="2395b-594">Příklad:</span><span class="sxs-lookup"><span data-stu-id="2395b-594">For example:</span></span>
```csharp
class Cache
{
    private static readonly object synchronizationObject = new object();

    public static void Add(object x) {
        lock (Cache.synchronizationObject) {
            ...
        }
    }

    public static void Remove(object x) {
        lock (Cache.synchronizationObject) {
            ...
        }
    }
}
```

## <a name="the-using-statement"></a><span data-ttu-id="2395b-595">Pomocí příkazu using</span><span class="sxs-lookup"><span data-stu-id="2395b-595">The using statement</span></span>

<span data-ttu-id="2395b-596">`using` Příkaz získá jeden nebo více zdrojů, provede příkaz a uvolní prostředku.</span><span class="sxs-lookup"><span data-stu-id="2395b-596">The `using` statement obtains one or more resources, executes a statement, and then disposes of the resource.</span></span>

```antlr
using_statement
    : 'using' '(' resource_acquisition ')' embedded_statement
    ;

resource_acquisition
    : local_variable_declaration
    | expression
    ;
```

<span data-ttu-id="2395b-597">A ***prostředků*** je třída nebo struktura, která implementuje `System.IDisposable`, což zahrnuje jeden konstruktor bez parametrů metodu s názvem `Dispose`.</span><span class="sxs-lookup"><span data-stu-id="2395b-597">A ***resource*** is a class or struct that implements `System.IDisposable`, which includes a single parameterless method named `Dispose`.</span></span> <span data-ttu-id="2395b-598">Můžete volat kód, který používá prostředek `Dispose` k označení, že prostředek už nepotřebujete.</span><span class="sxs-lookup"><span data-stu-id="2395b-598">Code that is using a resource can call `Dispose` to indicate that the resource is no longer needed.</span></span> <span data-ttu-id="2395b-599">Pokud `Dispose` není volána, dojde k automatické vyřazení nakonec následkem uvolňování paměti.</span><span class="sxs-lookup"><span data-stu-id="2395b-599">If `Dispose` is not called, then automatic disposal eventually occurs as a consequence of garbage collection.</span></span>

<span data-ttu-id="2395b-600">Pokud formu *resource_acquisition* je *local_variable_declaration* potom je typ *local_variable_declaration* musí být buď `dynamic` nebo typ. který lze implicitně převést na `System.IDisposable`.</span><span class="sxs-lookup"><span data-stu-id="2395b-600">If the form of *resource_acquisition* is *local_variable_declaration* then the type of the *local_variable_declaration* must be either `dynamic` or a type that can be implicitly converted to `System.IDisposable`.</span></span> <span data-ttu-id="2395b-601">Pokud formu *resource_acquisition* je *výraz* tento výraz musí být implicitně převeditelný na `System.IDisposable`.</span><span class="sxs-lookup"><span data-stu-id="2395b-601">If the form of *resource_acquisition* is *expression* then this expression must be implicitly convertible to `System.IDisposable`.</span></span>

<span data-ttu-id="2395b-602">Lokální proměnné deklarované v *resource_acquisition* jsou jen pro čtení a musí obsahovat inicializátor.</span><span class="sxs-lookup"><span data-stu-id="2395b-602">Local variables declared in a *resource_acquisition* are read-only, and must include an initializer.</span></span> <span data-ttu-id="2395b-603">Chyba kompilace v případě vloženým příkazem se pokusí upravit tyto místní proměnné (prostřednictvím přiřazení nebo `++` a `--` operátory), převzít adresu proměnné je, nebo předejte jako `ref` nebo `out` parametry.</span><span class="sxs-lookup"><span data-stu-id="2395b-603">A compile-time error occurs if the embedded statement attempts to modify these local variables (via assignment or the `++` and `--` operators) , take the address of them, or pass them as `ref` or `out` parameters.</span></span>

<span data-ttu-id="2395b-604">A `using` příkaz je přeložen do tří částí: získání, využití a vyřazení.</span><span class="sxs-lookup"><span data-stu-id="2395b-604">A `using` statement is translated into three parts: acquisition, usage, and disposal.</span></span> <span data-ttu-id="2395b-605">Využití prostředku je implicitně uzavřen v `try` příkaz, který zahrnuje `finally` klauzuli.</span><span class="sxs-lookup"><span data-stu-id="2395b-605">Usage of the resource is implicitly enclosed in a `try` statement that includes a `finally` clause.</span></span> <span data-ttu-id="2395b-606">To `finally` klauzule uvolní prostředku.</span><span class="sxs-lookup"><span data-stu-id="2395b-606">This `finally` clause disposes of the resource.</span></span> <span data-ttu-id="2395b-607">Pokud `null` prostředku je požadován, pak bez volání `Dispose` se provádí, a není vyvolána žádná výjimka.</span><span class="sxs-lookup"><span data-stu-id="2395b-607">If a `null` resource is acquired, then no call to `Dispose` is made, and no exception is thrown.</span></span> <span data-ttu-id="2395b-608">Pokud prostředek je typu `dynamic` převede se dynamicky prostřednictvím implicitního převodu dynamického ([implicitních převodů dynamické](conversions.md#implicit-dynamic-conversions)) k `IDisposable` během pořízení, aby se zajistilo, že je převod úspěšná, až poté využití a vyřazení.</span><span class="sxs-lookup"><span data-stu-id="2395b-608">If the resource is of type `dynamic` it is dynamically converted through an implicit dynamic conversion ([Implicit dynamic conversions](conversions.md#implicit-dynamic-conversions)) to `IDisposable` during acquisition in order to ensure that the conversion is successful before the usage and disposal.</span></span>

<span data-ttu-id="2395b-609">A `using` příkaz formuláře</span><span class="sxs-lookup"><span data-stu-id="2395b-609">A `using` statement of the form</span></span>
```csharp
using (ResourceType resource = expression) statement
```
<span data-ttu-id="2395b-610">představuje jednu ze tří možných rozšíření.</span><span class="sxs-lookup"><span data-stu-id="2395b-610">corresponds to one of three possible expansions.</span></span> <span data-ttu-id="2395b-611">Když `ResourceType` je typ hodnoty Null, je rozšíření</span><span class="sxs-lookup"><span data-stu-id="2395b-611">When `ResourceType` is a non-nullable value type, the expansion is</span></span>
```csharp
{
    ResourceType resource = expression;
    try {
        statement;
    }
    finally {
        ((IDisposable)resource).Dispose();
    }
}
```

<span data-ttu-id="2395b-612">V opačném případě `ResourceType` je typ s možnou hodnotou Null nebo odkazového typu jiného než `dynamic`, je rozšíření</span><span class="sxs-lookup"><span data-stu-id="2395b-612">Otherwise, when `ResourceType` is a nullable value type or a reference type other than `dynamic`, the expansion is</span></span>
```csharp
{
    ResourceType resource = expression;
    try {
        statement;
    }
    finally {
        if (resource != null) ((IDisposable)resource).Dispose();
    }
}
```

<span data-ttu-id="2395b-613">V opačném případě `ResourceType` je `dynamic`, je rozšíření</span><span class="sxs-lookup"><span data-stu-id="2395b-613">Otherwise, when `ResourceType` is `dynamic`, the expansion is</span></span>
```csharp
{
    ResourceType resource = expression;
    IDisposable d = (IDisposable)resource;
    try {
        statement;
    }
    finally {
        if (d != null) d.Dispose();
    }
}
```

<span data-ttu-id="2395b-614">V obou rozšíření `resource` je proměnná jen pro čtení v příkazu vložené a `d` proměnná se do nedostupný a neviditelná, vloženým příkazem.</span><span class="sxs-lookup"><span data-stu-id="2395b-614">In either expansion, the `resource` variable is read-only in the embedded statement, and the `d` variable is inaccessible in, and invisible to, the embedded statement.</span></span>

<span data-ttu-id="2395b-615">Implementace je povolený pro implementaci daného příkazu using odlišně, např. z důvodů výkonu za předpokladu, chování je konzistentní s rozšiřující se výše.</span><span class="sxs-lookup"><span data-stu-id="2395b-615">An implementation is permitted to implement a given using-statement differently, e.g. for performance reasons, as long as the behavior is consistent with the above expansion.</span></span>

<span data-ttu-id="2395b-616">A `using` příkaz formuláře</span><span class="sxs-lookup"><span data-stu-id="2395b-616">A `using` statement of the form</span></span>
```csharp
using (expression) statement
```
<span data-ttu-id="2395b-617">má stejné tři možné rozšíření.</span><span class="sxs-lookup"><span data-stu-id="2395b-617">has the same three possible expansions.</span></span> <span data-ttu-id="2395b-618">V tomto případě `ResourceType` je implicitně typu kompilace `expression`, má-li nějaký.</span><span class="sxs-lookup"><span data-stu-id="2395b-618">In this case `ResourceType` is implicitly the compile-time type of the `expression`, if it has one.</span></span> <span data-ttu-id="2395b-619">V opačném případě rozhraní `IDisposable` samotné se používá jako `ResourceType`.</span><span class="sxs-lookup"><span data-stu-id="2395b-619">Otherwise the interface `IDisposable` itself is used as the `ResourceType`.</span></span> <span data-ttu-id="2395b-620">`resource` Proměnná se do nedostupný a neviditelná, vloženým příkazem.</span><span class="sxs-lookup"><span data-stu-id="2395b-620">The `resource` variable is inaccessible in, and invisible to, the embedded statement.</span></span>

<span data-ttu-id="2395b-621">Když *resource_acquisition* má formu *local_variable_declaration*, je možné získat více prostředků daného typu.</span><span class="sxs-lookup"><span data-stu-id="2395b-621">When a *resource_acquisition* takes the form of a *local_variable_declaration*, it is possible to acquire multiple resources of a given type.</span></span> <span data-ttu-id="2395b-622">A `using` příkaz formuláře</span><span class="sxs-lookup"><span data-stu-id="2395b-622">A `using` statement of the form</span></span>
```csharp
using (ResourceType r1 = e1, r2 = e2, ..., rN = eN) statement
```
<span data-ttu-id="2395b-623">je vnořená přesně odpovídá sekvenci `using` příkazy:</span><span class="sxs-lookup"><span data-stu-id="2395b-623">is precisely equivalent to a sequence of nested `using` statements:</span></span>
```csharp
using (ResourceType r1 = e1)
    using (ResourceType r2 = e2)
        ...
            using (ResourceType rN = eN)
                statement
```

<span data-ttu-id="2395b-624">Následující příklad vytvoří soubor s názvem `log.txt` a zapíše dva řádky textu do souboru.</span><span class="sxs-lookup"><span data-stu-id="2395b-624">The example below creates a file named `log.txt` and writes two lines of text to the file.</span></span> <span data-ttu-id="2395b-625">V příkladu pak otevře že stejný soubor pro čtení a zkopíruje omezením řádků textu do konzoly.</span><span class="sxs-lookup"><span data-stu-id="2395b-625">The example then opens that same file for reading and copies the contained lines of text to the console.</span></span>
```csharp
using System;
using System.IO;

class Test
{
    static void Main() {
        using (TextWriter w = File.CreateText("log.txt")) {
            w.WriteLine("This is line one");
            w.WriteLine("This is line two");
        }

        using (TextReader r = File.OpenText("log.txt")) {
            string s;
            while ((s = r.ReadLine()) != null) {
                Console.WriteLine(s);
            }

        }
    }
}
```

<span data-ttu-id="2395b-626">Protože `TextWriter` a `TextReader` implementace třídy `IDisposable` rozhraní, můžete použít v příkladu `using` příkazy a ujistěte se, že je podkladový soubor řádně uzavřeny po zápisu operace čtení.</span><span class="sxs-lookup"><span data-stu-id="2395b-626">Since the `TextWriter` and `TextReader` classes implement the `IDisposable` interface, the example can use `using` statements to ensure that the underlying file is properly closed following the write or read operations.</span></span>

## <a name="the-yield-statement"></a><span data-ttu-id="2395b-627">Příkaz yield</span><span class="sxs-lookup"><span data-stu-id="2395b-627">The yield statement</span></span>

<span data-ttu-id="2395b-628">`yield` Prohlášení se používá v blok iterátoru ([bloky](statements.md#blocks)) na hodnotu, která objekt enumerator ([výčtu objektů](classes.md#enumerator-objects)) nebo vyčíslitelný objekt ([vyčíslitelné objekty](classes.md#enumerable-objects)) iterátoru nebo který signalizuje, že konce iterace.</span><span class="sxs-lookup"><span data-stu-id="2395b-628">The `yield` statement is used in an iterator block ([Blocks](statements.md#blocks)) to yield a value to the enumerator object ([Enumerator objects](classes.md#enumerator-objects)) or enumerable object ([Enumerable objects](classes.md#enumerable-objects)) of an iterator or to signal the end of the iteration.</span></span>

```antlr
yield_statement
    : 'yield' 'return' expression ';'
    | 'yield' 'break' ';'
    ;
```

<span data-ttu-id="2395b-629">`yield` není vyhrazené slovo má zvláštní význam pouze bezprostředně před `return` nebo `break` – klíčové slovo.</span><span class="sxs-lookup"><span data-stu-id="2395b-629">`yield` is not a reserved word; it has special meaning only when used immediately before a `return` or `break` keyword.</span></span> <span data-ttu-id="2395b-630">V jiných kontextech `yield` lze použít jako identifikátor.</span><span class="sxs-lookup"><span data-stu-id="2395b-630">In other contexts, `yield` can be used as an identifier.</span></span>

<span data-ttu-id="2395b-631">Existuje několik omezení umístění, ve kterém `yield` příkazu můžete zobrazit, jak je popsáno v následující.</span><span class="sxs-lookup"><span data-stu-id="2395b-631">There are several restrictions on where a `yield` statement can appear, as described in the following.</span></span>

*  <span data-ttu-id="2395b-632">Je chyba kompilace pro `yield` – příkaz (z obou tvarech) zobrazit mimo *method_body*, *operator_body* nebo *accessor_body*</span><span class="sxs-lookup"><span data-stu-id="2395b-632">It is a compile-time error for a `yield` statement (of either form) to appear outside a *method_body*, *operator_body* or *accessor_body*</span></span>
*  <span data-ttu-id="2395b-633">Je chyba kompilace pro `yield` – příkaz (z obou tvarech) se zobrazí uvnitř anonymní funkce.</span><span class="sxs-lookup"><span data-stu-id="2395b-633">It is a compile-time error for a `yield` statement (of either form) to appear inside an anonymous function.</span></span>
*  <span data-ttu-id="2395b-634">Je chyba kompilace pro `yield` – příkaz (z obou tvarech) se zobrazí v `finally` klauzuli `try` příkazu.</span><span class="sxs-lookup"><span data-stu-id="2395b-634">It is a compile-time error for a `yield` statement (of either form) to appear in the `finally` clause of a `try` statement.</span></span>
*  <span data-ttu-id="2395b-635">Je chyba kompilace pro `yield return` vyskytovat kdekoli v příkazu `try` příkaz, který obsahuje některý `catch` klauzule.</span><span class="sxs-lookup"><span data-stu-id="2395b-635">It is a compile-time error for a `yield return` statement to appear anywhere in a `try` statement that contains any `catch` clauses.</span></span>

<span data-ttu-id="2395b-636">Následující příklad ukazuje některé platné a neplatné použití `yield` příkazy.</span><span class="sxs-lookup"><span data-stu-id="2395b-636">The following example shows some valid and invalid uses of `yield` statements.</span></span>

```csharp
delegate IEnumerable<int> D();

IEnumerator<int> GetEnumerator() {
    try {
        yield return 1;        // Ok
        yield break;           // Ok
    }
    finally {
        yield return 2;        // Error, yield in finally
        yield break;           // Error, yield in finally
    }

    try {
        yield return 3;        // Error, yield return in try...catch
        yield break;           // Ok
    }
    catch {
        yield return 4;        // Error, yield return in try...catch
        yield break;           // Ok
    }

    D d = delegate { 
        yield return 5;        // Error, yield in an anonymous function
    }; 
}

int MyMethod() {
    yield return 1;            // Error, wrong return type for an iterator block
}
```

<span data-ttu-id="2395b-637">Implicitní převod ([implicitních převodů](conversions.md#implicit-conversions)) z typu výrazu v, musí existovat `yield return` příkazu yield typ ([Yield typ](classes.md#yield-type)) iterátoru.</span><span class="sxs-lookup"><span data-stu-id="2395b-637">An implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)) must exist from the type of the expression in the `yield return` statement to the yield type ([Yield type](classes.md#yield-type)) of the iterator.</span></span>

<span data-ttu-id="2395b-638">A `yield return` je proveden příkaz takto:</span><span class="sxs-lookup"><span data-stu-id="2395b-638">A `yield return` statement is executed as follows:</span></span>

*  <span data-ttu-id="2395b-639">Výraz zadaný v příkazu je vyhodnocen, implicitně převeden na typ yield a přiřazená `Current` vlastnost v objektu enumerátor.</span><span class="sxs-lookup"><span data-stu-id="2395b-639">The expression given in the statement is evaluated, implicitly converted to the yield type, and assigned to the `Current` property of the enumerator object.</span></span>
*  <span data-ttu-id="2395b-640">Spouštění bloku iterátoru je pozastaveno.</span><span class="sxs-lookup"><span data-stu-id="2395b-640">Execution of the iterator block is suspended.</span></span> <span data-ttu-id="2395b-641">Pokud `yield return` příkaz je v jedné nebo více `try` blokuje, přidružené `finally` v tuto chvíli nejsou provedeny bloky.</span><span class="sxs-lookup"><span data-stu-id="2395b-641">If the `yield return` statement is within one or more `try` blocks, the associated `finally` blocks are not executed at this time.</span></span>
*  <span data-ttu-id="2395b-642">`MoveNext` Vrátí metoda objekt enumerator `true` na volající funkci, která udává, že objekt enumerator byly úspěšně posunuty na další položku.</span><span class="sxs-lookup"><span data-stu-id="2395b-642">The `MoveNext` method of the enumerator object returns `true` to its caller, indicating that the enumerator object successfully advanced to the next item.</span></span>

<span data-ttu-id="2395b-643">Další volání objekt enumerator `MoveNext` metoda pokračuje v provádění bloku iterátoru od kdy posledního pozastavení.</span><span class="sxs-lookup"><span data-stu-id="2395b-643">The next call to the enumerator object's `MoveNext` method resumes execution of the iterator block from where it was last suspended.</span></span>

<span data-ttu-id="2395b-644">A `yield break` je proveden příkaz takto:</span><span class="sxs-lookup"><span data-stu-id="2395b-644">A `yield break` statement is executed as follows:</span></span>

*  <span data-ttu-id="2395b-645">Pokud `yield break` příkazu není uzavřen v jedné nebo více `try` přidružené bloky s `finally` bloky, ovládací prvek zpočátku bude převeden na `finally` bloku nejvnitřnější `try` příkaz.</span><span class="sxs-lookup"><span data-stu-id="2395b-645">If the `yield break` statement is enclosed by one or more `try` blocks with associated `finally` blocks, control is initially transferred to the `finally` block of the innermost `try` statement.</span></span> <span data-ttu-id="2395b-646">Pokud a v případě, že ovládací prvek dosáhne koncového bodu `finally` bloku, ovládací prvek bude převeden na `finally` další uzavírající blok `try` příkazu.</span><span class="sxs-lookup"><span data-stu-id="2395b-646">When and if control reaches the end point of a `finally` block, control is transferred to the `finally` block of the next enclosing `try` statement.</span></span> <span data-ttu-id="2395b-647">Tento proces se opakuje, dokud `finally` blokuje všechny nadřazené `try` příkazy byly provedeny.</span><span class="sxs-lookup"><span data-stu-id="2395b-647">This process is repeated until the `finally` blocks of all enclosing `try` statements have been executed.</span></span>
*  <span data-ttu-id="2395b-648">Ovládací prvek se vrátí volajícímu metody blok iterátoru.</span><span class="sxs-lookup"><span data-stu-id="2395b-648">Control is returned to the caller of the iterator block.</span></span> <span data-ttu-id="2395b-649">Je to `MoveNext` metoda nebo `Dispose` metodu objektu enumerátor.</span><span class="sxs-lookup"><span data-stu-id="2395b-649">This is either the `MoveNext` method or `Dispose` method of the enumerator object.</span></span>

<span data-ttu-id="2395b-650">Protože `yield break` příkaz bezpodmínečně převede ovládací prvek jinde, koncový bod `yield break` příkazu není dostupný.</span><span class="sxs-lookup"><span data-stu-id="2395b-650">Because a `yield break` statement unconditionally transfers control elsewhere, the end point of a `yield break` statement is never reachable.</span></span>
