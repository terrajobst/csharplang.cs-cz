---
ms.openlocfilehash: 94346034a667ad4af26796c0c4bbc96d6ed79aba
ms.sourcegitcommit: 7f7fc6e9e195e51b7ff8229aeaa70aa9fbbb63cb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/10/2019
ms.locfileid: "70876836"
---
# <a name="statements"></a><span data-ttu-id="308a0-101">Příkazy</span><span class="sxs-lookup"><span data-stu-id="308a0-101">Statements</span></span>

<span data-ttu-id="308a0-102">C#poskytuje celou řadu příkazů.</span><span class="sxs-lookup"><span data-stu-id="308a0-102">C# provides a variety of statements.</span></span> <span data-ttu-id="308a0-103">Většina těchto příkazů bude obeznámena s vývojáři, kteří mají program v jazyce C a C++.</span><span class="sxs-lookup"><span data-stu-id="308a0-103">Most of these statements will be familiar to developers who have programmed in C and C++.</span></span>

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

<span data-ttu-id="308a0-104">Neterminál *embedded_statement* se používá pro příkazy, které se zobrazí v jiných příkazech.</span><span class="sxs-lookup"><span data-stu-id="308a0-104">The *embedded_statement* nonterminal is used for statements that appear within other statements.</span></span> <span data-ttu-id="308a0-105">Použití příkazu *embedded_statement* spíše než *příkaz* vyloučí v těchto kontextech příkazy deklarace a příkazy s popiskem.</span><span class="sxs-lookup"><span data-stu-id="308a0-105">The use of *embedded_statement* rather than *statement* excludes the use of declaration statements and labeled statements in these contexts.</span></span> <span data-ttu-id="308a0-106">Příklad</span><span class="sxs-lookup"><span data-stu-id="308a0-106">The example</span></span>
```csharp
void F(bool b) {
    if (b)
        int i = 44;
}
```
<span data-ttu-id="308a0-107">Výsledkem je chyba při kompilaci, protože `if` příkaz vyžaduje *embedded_statement* , nikoli *příkaz* pro svou větev if.</span><span class="sxs-lookup"><span data-stu-id="308a0-107">results in a compile-time error because an `if` statement requires an *embedded_statement* rather than a *statement* for its if branch.</span></span> <span data-ttu-id="308a0-108">Pokud byl tento kód povolen, proměnná `i` by byla deklarována, ale nebyla nikdy použita.</span><span class="sxs-lookup"><span data-stu-id="308a0-108">If this code were permitted, then the variable `i` would be declared, but it could never be used.</span></span> <span data-ttu-id="308a0-109">Všimněte si však, že `i`vložením deklarace do bloku je platný příklad.</span><span class="sxs-lookup"><span data-stu-id="308a0-109">Note, however, that by placing `i`'s declaration in a block, the example is valid.</span></span>

## <a name="end-points-and-reachability"></a><span data-ttu-id="308a0-110">Koncové body a dosažitelnost</span><span class="sxs-lookup"><span data-stu-id="308a0-110">End points and reachability</span></span>

<span data-ttu-id="308a0-111">Každý příkaz má ***koncový bod***.</span><span class="sxs-lookup"><span data-stu-id="308a0-111">Every statement has an ***end point***.</span></span> <span data-ttu-id="308a0-112">V intuitivních výrazech je koncový bod příkazu umístěním, které bezprostředně následuje za příkazem.</span><span class="sxs-lookup"><span data-stu-id="308a0-112">In intuitive terms, the end point of a statement is the location that immediately follows the statement.</span></span> <span data-ttu-id="308a0-113">Pravidla spouštění pro složené příkazy (příkazy, které obsahují vložené příkazy) určují akci, která je provedena, když ovládací prvek dosáhne koncového bodu vloženého příkazu.</span><span class="sxs-lookup"><span data-stu-id="308a0-113">The execution rules for composite statements (statements that contain embedded statements) specify the action that is taken when control reaches the end point of an embedded statement.</span></span> <span data-ttu-id="308a0-114">Například když ovládací prvek dosáhne koncového bodu příkazu v bloku, je ovládací prvek převeden na další příkaz v bloku.</span><span class="sxs-lookup"><span data-stu-id="308a0-114">For example, when control reaches the end point of a statement in a block, control is transferred to the next statement in the block.</span></span>

<span data-ttu-id="308a0-115">Pokud je možné, že je příkaz dosažitelný provedením, je příkaz označen jako ***dostupný***.</span><span class="sxs-lookup"><span data-stu-id="308a0-115">If a statement can possibly be reached by execution, the statement is said to be ***reachable***.</span></span> <span data-ttu-id="308a0-116">Naopak, pokud není k dispozici možnost provedení příkazu, bude prohlášení ***nedostupné***.</span><span class="sxs-lookup"><span data-stu-id="308a0-116">Conversely, if there is no possibility that a statement will be executed, the statement is said to be ***unreachable***.</span></span>

<span data-ttu-id="308a0-117">V příkladu</span><span class="sxs-lookup"><span data-stu-id="308a0-117">In the example</span></span>
```csharp
void F() {
    Console.WriteLine("reachable");
    goto Label;
    Console.WriteLine("unreachable");
    Label:
    Console.WriteLine("reachable");
}
```
<span data-ttu-id="308a0-118">druhé vyvolání `Console.WriteLine` je nedosažitelné, protože neexistuje možnost, že se příkaz spustí.</span><span class="sxs-lookup"><span data-stu-id="308a0-118">the second invocation of `Console.WriteLine` is unreachable because there is no possibility that the statement will be executed.</span></span>

<span data-ttu-id="308a0-119">Pokud kompilátor zjistí, že je příkaz nedosažitelný, je hlášeno upozornění.</span><span class="sxs-lookup"><span data-stu-id="308a0-119">A warning is reported if the compiler determines that a statement is unreachable.</span></span> <span data-ttu-id="308a0-120">Konkrétně není chyba, pokud příkaz nebude dostupný.</span><span class="sxs-lookup"><span data-stu-id="308a0-120">It is specifically not an error for a statement to be unreachable.</span></span>

<span data-ttu-id="308a0-121">Chcete-li určit, zda je konkrétní příkaz nebo koncový bod dosažitelný, kompilátor provede analýzu toků podle pravidel dostupnosti definovaných pro každý příkaz.</span><span class="sxs-lookup"><span data-stu-id="308a0-121">To determine whether a particular statement or end point is reachable, the compiler performs flow analysis according to the reachability rules defined for each statement.</span></span> <span data-ttu-id="308a0-122">Analýza toku bere v úvahu hodnoty konstantních výrazů ([konstantní výrazy](expressions.md#constant-expressions)), které řídí chování příkazů, ale možné hodnoty nekonstantních výrazů nejsou považovány za.</span><span class="sxs-lookup"><span data-stu-id="308a0-122">The flow analysis takes into account the values of constant expressions ([Constant expressions](expressions.md#constant-expressions)) that control the behavior of statements, but the possible values of non-constant expressions are not considered.</span></span> <span data-ttu-id="308a0-123">Jinými slovy pro účely analýzy toku řízení je nekonstantní výraz daného typu považován za to, že má libovolnou možnou hodnotu tohoto typu.</span><span class="sxs-lookup"><span data-stu-id="308a0-123">In other words, for purposes of control flow analysis, a non-constant expression of a given type is considered to have any possible value of that type.</span></span>

<span data-ttu-id="308a0-124">V příkladu</span><span class="sxs-lookup"><span data-stu-id="308a0-124">In the example</span></span>
```csharp
void F() {
    const int i = 1;
    if (i == 2) Console.WriteLine("unreachable");
}
```
<span data-ttu-id="308a0-125">logický výraz `if` příkazu je konstantní výraz, protože oba operandy `==` operátoru jsou konstanty.</span><span class="sxs-lookup"><span data-stu-id="308a0-125">the boolean expression of the `if` statement is a constant expression because both operands of the `==` operator are constants.</span></span> <span data-ttu-id="308a0-126">Jelikož je konstantní výraz vyhodnocen v době kompilace, což vyprodukuje hodnotu `false` `Console.WriteLine` , vyvolání je považováno za nedosažitelné.</span><span class="sxs-lookup"><span data-stu-id="308a0-126">As the constant expression is evaluated at compile-time, producing the value `false`, the `Console.WriteLine` invocation is considered unreachable.</span></span> <span data-ttu-id="308a0-127">Pokud `i` se ale změní na lokální proměnnou</span><span class="sxs-lookup"><span data-stu-id="308a0-127">However, if `i` is changed to be a local variable</span></span>
```csharp
void F() {
    int i = 1;
    if (i == 2) Console.WriteLine("reachable");
}
```
<span data-ttu-id="308a0-128">`Console.WriteLine` volání je považováno za dosažitelné, i když ve skutečnosti nebude nikdy provedeno.</span><span class="sxs-lookup"><span data-stu-id="308a0-128">the `Console.WriteLine` invocation is considered reachable, even though, in reality, it will never be executed.</span></span>

<span data-ttu-id="308a0-129">*Blok* člena funkce je vždy považován za dostupný.</span><span class="sxs-lookup"><span data-stu-id="308a0-129">The *block* of a function member is always considered reachable.</span></span> <span data-ttu-id="308a0-130">Po úspěšném vyhodnocení pravidel dostupnosti každého příkazu v bloku je možné určit dostupnost jakéhokoli daného příkazu.</span><span class="sxs-lookup"><span data-stu-id="308a0-130">By successively evaluating the reachability rules of each statement in a block, the reachability of any given statement can be determined.</span></span>

<span data-ttu-id="308a0-131">V příkladu</span><span class="sxs-lookup"><span data-stu-id="308a0-131">In the example</span></span>
```csharp
void F(int x) {
    Console.WriteLine("start");
    if (x < 0) Console.WriteLine("negative");
}
```
<span data-ttu-id="308a0-132">dostupnost druhé `Console.WriteLine` je určena následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="308a0-132">the reachability of the second `Console.WriteLine` is determined as follows:</span></span>

*  <span data-ttu-id="308a0-133">První `Console.WriteLine` příkaz výrazu je dosažitelný, protože blok `F` metody je dosažitelný.</span><span class="sxs-lookup"><span data-stu-id="308a0-133">The first `Console.WriteLine` expression statement is reachable because the block of the `F` method is reachable.</span></span>
*  <span data-ttu-id="308a0-134">Koncový bod prvního `Console.WriteLine` příkazu výrazu je dosažitelný, protože tento příkaz je dosažitelný.</span><span class="sxs-lookup"><span data-stu-id="308a0-134">The end point of the first `Console.WriteLine` expression statement is reachable because that statement is reachable.</span></span>
*  <span data-ttu-id="308a0-135">Příkaz je dosažitelný, protože koncový bod prvního `Console.WriteLine` příkazu výrazu je dosažitelný. `if`</span><span class="sxs-lookup"><span data-stu-id="308a0-135">The `if` statement is reachable because the end point of the first `Console.WriteLine` expression statement is reachable.</span></span>
*  <span data-ttu-id="308a0-136">Druhý `Console.WriteLine` příkaz výrazu je dosažitelný, protože logický výraz `if` příkazu nemá konstantní hodnotu `false`.</span><span class="sxs-lookup"><span data-stu-id="308a0-136">The second `Console.WriteLine` expression statement is reachable because the boolean expression of the `if` statement does not have the constant value `false`.</span></span>

<span data-ttu-id="308a0-137">Existují dvě situace, ve kterých se jedná o chybu při kompilaci koncového bodu příkazu, aby byla dostupná:</span><span class="sxs-lookup"><span data-stu-id="308a0-137">There are two situations in which it is a compile-time error for the end point of a statement to be reachable:</span></span>

*  <span data-ttu-id="308a0-138">Vzhledem k tomu, že příkaznepovolujepřesměrovatoddílpřepínačedodalšíhooddílupřepínače,jednáseochybupřikompilacikoncovéhoboduseznamupříkazůoddílupřepínače,abybyldostupný.`switch`</span><span class="sxs-lookup"><span data-stu-id="308a0-138">Because the `switch` statement does not permit a switch section to "fall through" to the next switch section, it is a compile-time error for the end point of the statement list of a switch section to be reachable.</span></span> <span data-ttu-id="308a0-139">Pokud k této chybě dojde, obvykle se jedná o indikaci `break` chybějícího příkazu.</span><span class="sxs-lookup"><span data-stu-id="308a0-139">If this error occurs, it is typically an indication that a `break` statement is missing.</span></span>
*  <span data-ttu-id="308a0-140">Jedná se o chybu při kompilaci koncového bodu bloku členu funkce, který počítá hodnotu, která je dostupná.</span><span class="sxs-lookup"><span data-stu-id="308a0-140">It is a compile-time error for the end point of the block of a function member that computes a value to be reachable.</span></span> <span data-ttu-id="308a0-141">Pokud k této chybě dojde, obvykle se jedná o indikaci `return` chybějícího příkazu.</span><span class="sxs-lookup"><span data-stu-id="308a0-141">If this error occurs, it typically is an indication that a `return` statement is missing.</span></span>

## <a name="blocks"></a><span data-ttu-id="308a0-142">Bloky</span><span class="sxs-lookup"><span data-stu-id="308a0-142">Blocks</span></span>

<span data-ttu-id="308a0-143">*Blok* povoluje zápis více příkazů v kontextech, kde je povolen jediný příkaz.</span><span class="sxs-lookup"><span data-stu-id="308a0-143">A *block* permits multiple statements to be written in contexts where a single statement is allowed.</span></span>

```antlr
block
    : '{' statement_list? '}'
    ;
```

<span data-ttu-id="308a0-144">*Blok* se skládá z volitelných *statement_list* ([seznamů příkazů](statements.md#statement-lists)) uzavřených do složených závorek.</span><span class="sxs-lookup"><span data-stu-id="308a0-144">A *block* consists of an optional *statement_list* ([Statement lists](statements.md#statement-lists)), enclosed in braces.</span></span> <span data-ttu-id="308a0-145">Pokud je seznam příkazů vynechán, je tento blok označován jako prázdný.</span><span class="sxs-lookup"><span data-stu-id="308a0-145">If the statement list is omitted, the block is said to be empty.</span></span>

<span data-ttu-id="308a0-146">Blok může obsahovat příkazy deklarace ([příkazy deklarace](statements.md#declaration-statements)).</span><span class="sxs-lookup"><span data-stu-id="308a0-146">A block may contain declaration statements ([Declaration statements](statements.md#declaration-statements)).</span></span> <span data-ttu-id="308a0-147">Rozsah místní proměnné nebo konstanty deklarované v bloku je blok.</span><span class="sxs-lookup"><span data-stu-id="308a0-147">The scope of a local variable or constant declared in a block is the block.</span></span>

<span data-ttu-id="308a0-148">Blok se spustí takto:</span><span class="sxs-lookup"><span data-stu-id="308a0-148">A block is executed as follows:</span></span>

*  <span data-ttu-id="308a0-149">Pokud je blok prázdný, řízení se převede na koncový bod bloku.</span><span class="sxs-lookup"><span data-stu-id="308a0-149">If the block is empty, control is transferred to the end point of the block.</span></span>
*  <span data-ttu-id="308a0-150">Pokud blok není prázdný, ovládací prvek se přenese do seznamu příkazů.</span><span class="sxs-lookup"><span data-stu-id="308a0-150">If the block is not empty, control is transferred to the statement list.</span></span> <span data-ttu-id="308a0-151">Když a když ovládací prvek dosáhne koncového bodu seznamu příkazů, ovládací prvek se převede na koncový bod bloku.</span><span class="sxs-lookup"><span data-stu-id="308a0-151">When and if control reaches the end point of the statement list, control is transferred to the end point of the block.</span></span>

<span data-ttu-id="308a0-152">Seznam příkazů bloku je dosažitelný, pokud je samotný blok dostupný.</span><span class="sxs-lookup"><span data-stu-id="308a0-152">The statement list of a block is reachable if the block itself is reachable.</span></span>

<span data-ttu-id="308a0-153">Koncový bod bloku je dosažitelný, pokud je blok prázdný nebo pokud je koncový bod seznamu příkazů dostupný.</span><span class="sxs-lookup"><span data-stu-id="308a0-153">The end point of a block is reachable if the block is empty or if the end point of the statement list is reachable.</span></span>

<span data-ttu-id="308a0-154">*Blok* , který obsahuje jeden nebo více `yield` příkazů ([příkaz yield](statements.md#the-yield-statement)), se nazývá blok iterátoru.</span><span class="sxs-lookup"><span data-stu-id="308a0-154">A *block* that contains one or more `yield` statements ([The yield statement](statements.md#the-yield-statement)) is called an iterator block.</span></span> <span data-ttu-id="308a0-155">Bloky iterátoru slouží k implementaci členů funkce jako iterátory ([iterátory](classes.md#iterators)).</span><span class="sxs-lookup"><span data-stu-id="308a0-155">Iterator blocks are used to implement function members as iterators ([Iterators](classes.md#iterators)).</span></span> <span data-ttu-id="308a0-156">U bloků iterátoru platí některá další omezení:</span><span class="sxs-lookup"><span data-stu-id="308a0-156">Some additional restrictions apply to iterator blocks:</span></span>

*  <span data-ttu-id="308a0-157">Jedná se o chybu `return` při kompilaci, aby se příkaz objevil v bloku iterátoru (ale `yield return` příkazy jsou povoleny).</span><span class="sxs-lookup"><span data-stu-id="308a0-157">It is a compile-time error for a `return` statement to appear in an iterator block (but `yield return` statements are permitted).</span></span>
*  <span data-ttu-id="308a0-158">Jedná se o chybu při kompilaci, aby blok iterátoru obsahoval nezabezpečený kontext ([nezabezpečené](unsafe-code.md#unsafe-contexts)kontexty).</span><span class="sxs-lookup"><span data-stu-id="308a0-158">It is a compile-time error for an iterator block to contain an unsafe context ([Unsafe contexts](unsafe-code.md#unsafe-contexts)).</span></span> <span data-ttu-id="308a0-159">Blok iterátoru vždy definuje bezpečný kontext, a to i v případě, že je jeho deklarace vnořena v nezabezpečeném kontextu.</span><span class="sxs-lookup"><span data-stu-id="308a0-159">An iterator block always defines a safe context, even when its declaration is nested in an unsafe context.</span></span>

### <a name="statement-lists"></a><span data-ttu-id="308a0-160">Seznamy příkazů</span><span class="sxs-lookup"><span data-stu-id="308a0-160">Statement lists</span></span>

<span data-ttu-id="308a0-161">***Seznam příkazů*** se skládá z jednoho nebo více příkazů zapsaných v sekvenci.</span><span class="sxs-lookup"><span data-stu-id="308a0-161">A ***statement list*** consists of one or more statements written in sequence.</span></span> <span data-ttu-id="308a0-162">Seznamy příkazů se vyskytují v blocích s ( *Blocks*[) a](statements.md#blocks)v *switch_block*s ([příkaz switch](statements.md#the-switch-statement)).</span><span class="sxs-lookup"><span data-stu-id="308a0-162">Statement lists occur in *block*s ([Blocks](statements.md#blocks)) and in *switch_block*s ([The switch statement](statements.md#the-switch-statement)).</span></span>

```antlr
statement_list
    : statement+
    ;
```

<span data-ttu-id="308a0-163">Seznam příkazů je spuštěn převodem řízení na první příkaz.</span><span class="sxs-lookup"><span data-stu-id="308a0-163">A statement list is executed by transferring control to the first statement.</span></span> <span data-ttu-id="308a0-164">Když a pokud ovládací prvek dosáhne koncového bodu příkazu, bude ovládací prvek převeden na další příkaz.</span><span class="sxs-lookup"><span data-stu-id="308a0-164">When and if control reaches the end point of a statement, control is transferred to the next statement.</span></span> <span data-ttu-id="308a0-165">Když a když ovládací prvek dosáhne koncového bodu posledního příkazu, ovládací prvek se převede na koncový bod seznamu příkazů.</span><span class="sxs-lookup"><span data-stu-id="308a0-165">When and if control reaches the end point of the last statement, control is transferred to the end point of the statement list.</span></span>

<span data-ttu-id="308a0-166">Příkaz v seznamu příkazů je dosažitelný, pokud je splněna alespoň jedna z následujících podmínek:</span><span class="sxs-lookup"><span data-stu-id="308a0-166">A statement in a statement list is reachable if at least one of the following is true:</span></span>

*  <span data-ttu-id="308a0-167">Příkaz je prvním příkazem a samotný seznam příkazů je dosažitelný.</span><span class="sxs-lookup"><span data-stu-id="308a0-167">The statement is the first statement and the statement list itself is reachable.</span></span>
*  <span data-ttu-id="308a0-168">Koncový bod předchozího příkazu je dosažitelný.</span><span class="sxs-lookup"><span data-stu-id="308a0-168">The end point of the preceding statement is reachable.</span></span>
*  <span data-ttu-id="308a0-169">Příkaz je příkaz s popiskem a popisek je odkazován pomocí dosažitelného `goto` příkazu.</span><span class="sxs-lookup"><span data-stu-id="308a0-169">The statement is a labeled statement and the label is referenced by a reachable `goto` statement.</span></span>

<span data-ttu-id="308a0-170">Koncový bod seznamu příkazů je dosažitelný, pokud je koncový bod posledního příkazu v seznamu dosažitelný.</span><span class="sxs-lookup"><span data-stu-id="308a0-170">The end point of a statement list is reachable if the end point of the last statement in the list is reachable.</span></span>

## <a name="the-empty-statement"></a><span data-ttu-id="308a0-171">Prázdný příkaz</span><span class="sxs-lookup"><span data-stu-id="308a0-171">The empty statement</span></span>

<span data-ttu-id="308a0-172">*Empty_statement* neprovede žádnou akci.</span><span class="sxs-lookup"><span data-stu-id="308a0-172">An *empty_statement* does nothing.</span></span>

```antlr
empty_statement
    : ';'
    ;
```

<span data-ttu-id="308a0-173">Prázdný příkaz se používá, pokud nejsou žádné operace k provedení v kontextu, kde je vyžadován příkaz.</span><span class="sxs-lookup"><span data-stu-id="308a0-173">An empty statement is used when there are no operations to perform in a context where a statement is required.</span></span>

<span data-ttu-id="308a0-174">Provedení prázdného příkazu jednoduše převede řízení na koncový bod příkazu.</span><span class="sxs-lookup"><span data-stu-id="308a0-174">Execution of an empty statement simply transfers control to the end point of the statement.</span></span> <span data-ttu-id="308a0-175">Proto je koncový bod prázdného příkazu dosažitelný, pokud je prázdný příkaz dostupný.</span><span class="sxs-lookup"><span data-stu-id="308a0-175">Thus, the end point of an empty statement is reachable if the empty statement is reachable.</span></span>

<span data-ttu-id="308a0-176">Prázdný příkaz lze použít při zápisu `while` příkazu s textem s hodnotou null:</span><span class="sxs-lookup"><span data-stu-id="308a0-176">An empty statement can be used when writing a `while` statement with a null body:</span></span>
```csharp
bool ProcessMessage() {...}

void ProcessMessages() {
    while (ProcessMessage())
        ;
}
```

<span data-ttu-id="308a0-177">Prázdný příkaz lze také použít k deklaraci popisku těsně před uzavíracím znakem "`}`" bloku:</span><span class="sxs-lookup"><span data-stu-id="308a0-177">Also, an empty statement can be used to declare a label just before the closing "`}`" of a block:</span></span>
```csharp
void F() {
    ...
    if (done) goto exit;
    ...
    exit: ;
}
```

## <a name="labeled-statements"></a><span data-ttu-id="308a0-178">Příkazy s popiskem</span><span class="sxs-lookup"><span data-stu-id="308a0-178">Labeled statements</span></span>

<span data-ttu-id="308a0-179">*Labeled_statement* umožňuje, aby příkaz byl předponou.</span><span class="sxs-lookup"><span data-stu-id="308a0-179">A *labeled_statement* permits a statement to be prefixed by a label.</span></span> <span data-ttu-id="308a0-180">Příkazy s popiskem jsou povolené v blocích, ale nejsou povolené jako vložené příkazy.</span><span class="sxs-lookup"><span data-stu-id="308a0-180">Labeled statements are permitted in blocks, but are not permitted as embedded statements.</span></span>

```antlr
labeled_statement
    : identifier ':' statement
    ;
```

<span data-ttu-id="308a0-181">Příkaz s popiskem deklaruje popisek s názvem daným *identifikátorem*.</span><span class="sxs-lookup"><span data-stu-id="308a0-181">A labeled statement declares a label with the name given by the *identifier*.</span></span> <span data-ttu-id="308a0-182">Rozsah popisku je celý blok, ve kterém je popisek deklarován, včetně všech vnořených bloků.</span><span class="sxs-lookup"><span data-stu-id="308a0-182">The scope of a label is the whole block in which the label is declared, including any nested blocks.</span></span> <span data-ttu-id="308a0-183">Jedná se o chybu při kompilaci pro dva popisky se stejným názvem, aby měly překrývající se obory.</span><span class="sxs-lookup"><span data-stu-id="308a0-183">It is a compile-time error for two labels with the same name to have overlapping scopes.</span></span>

<span data-ttu-id="308a0-184">Na popisek lze odkazovat z `goto` příkazů ([příkaz goto](statements.md#the-goto-statement)) v rámci rozsahu popisku.</span><span class="sxs-lookup"><span data-stu-id="308a0-184">A label can be referenced from `goto` statements ([The goto statement](statements.md#the-goto-statement)) within the scope of the label.</span></span> <span data-ttu-id="308a0-185">To znamená, `goto` že příkazy mohou přenášet řízení v rámci bloků a mimo bloky, ale nikdy do bloků.</span><span class="sxs-lookup"><span data-stu-id="308a0-185">This means that `goto` statements can transfer control within blocks and out of blocks, but never into blocks.</span></span>

<span data-ttu-id="308a0-186">Popisky mají vlastní prostor deklarací a neovlivňují jiné identifikátory.</span><span class="sxs-lookup"><span data-stu-id="308a0-186">Labels have their own declaration space and do not interfere with other identifiers.</span></span> <span data-ttu-id="308a0-187">Příklad</span><span class="sxs-lookup"><span data-stu-id="308a0-187">The example</span></span>
```csharp
int F(int x) {
    if (x >= 0) goto x;
    x = -x;
    x: return x;
}
```
<span data-ttu-id="308a0-188">je platný a používá název `x` jako parametr i popisek.</span><span class="sxs-lookup"><span data-stu-id="308a0-188">is valid and uses the name `x` as both a parameter and a label.</span></span>

<span data-ttu-id="308a0-189">Provedení příkaz s popiskem odpovídá přesně provedení příkazu za popiskem.</span><span class="sxs-lookup"><span data-stu-id="308a0-189">Execution of a labeled statement corresponds exactly to execution of the statement following the label.</span></span>

<span data-ttu-id="308a0-190">Kromě dosažitelnosti, které poskytuje běžný tok řízení, je příkaz s popiskem dosažitelný, pokud je popisek odkazován pomocí dosažitelného `goto` příkazu.</span><span class="sxs-lookup"><span data-stu-id="308a0-190">In addition to the reachability provided by normal flow of control, a labeled statement is reachable if the label is referenced by a reachable `goto` statement.</span></span> <span data-ttu-id="308a0-191">Jímka `finally` `finally` `try`Pokud je `try` příkaz uvnitř, který obsahuje blok, a příkaz s popiskem je mimo a koncový bod bloku je nedosažitelný, pak příkaz s popiskem není dosažitelný z `goto` Tento `goto` příkaz.)</span><span class="sxs-lookup"><span data-stu-id="308a0-191">(Exception: If a `goto` statement is inside a `try` that includes a `finally` block, and the labeled statement is outside the `try`, and the end point of the `finally` block is unreachable, then the labeled statement is not reachable from that `goto` statement.)</span></span>

## <a name="declaration-statements"></a><span data-ttu-id="308a0-192">Příkazy deklarace</span><span class="sxs-lookup"><span data-stu-id="308a0-192">Declaration statements</span></span>

<span data-ttu-id="308a0-193">*Declaration_statement* deklaruje místní proměnnou nebo konstantu.</span><span class="sxs-lookup"><span data-stu-id="308a0-193">A *declaration_statement* declares a local variable or constant.</span></span> <span data-ttu-id="308a0-194">Příkazy deklarace jsou povolené v blocích, ale nejsou povolené jako vložené příkazy.</span><span class="sxs-lookup"><span data-stu-id="308a0-194">Declaration statements are permitted in blocks, but are not permitted as embedded statements.</span></span>

```antlr
declaration_statement
    : local_variable_declaration ';'
    | local_constant_declaration ';'
    ;
```

### <a name="local-variable-declarations"></a><span data-ttu-id="308a0-195">Deklarace místních proměnných</span><span class="sxs-lookup"><span data-stu-id="308a0-195">Local variable declarations</span></span>

<span data-ttu-id="308a0-196">*Local_variable_declaration* deklaruje jednu nebo více místních proměnných.</span><span class="sxs-lookup"><span data-stu-id="308a0-196">A *local_variable_declaration* declares one or more local variables.</span></span>

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

<span data-ttu-id="308a0-197">*Local_variable_type* *local_variable_declaration* buď přímo určuje typ proměnných zavedených deklarací, nebo označuje identifikátor `var` , který má být typu odvozen na základě inicializátor.</span><span class="sxs-lookup"><span data-stu-id="308a0-197">The *local_variable_type* of a *local_variable_declaration* either directly specifies the type of the variables introduced by the declaration, or indicates with the identifier `var` that the type should be inferred based on an initializer.</span></span> <span data-ttu-id="308a0-198">Po typu následuje seznam *local_variable_declarator*, z nichž každá zavádí novou proměnnou.</span><span class="sxs-lookup"><span data-stu-id="308a0-198">The type is followed by a list of *local_variable_declarator*s, each of which introduces a new variable.</span></span> <span data-ttu-id="308a0-199">*Local_variable_declarator* se skládá z *identifikátoru* , který proměnnou pojmenovává, volitelně následovaný "`=`" tokenem a *local_variable_initializer* , který poskytuje počáteční hodnotu proměnné.</span><span class="sxs-lookup"><span data-stu-id="308a0-199">A *local_variable_declarator* consists of an *identifier* that names the variable, optionally followed by an "`=`" token and a *local_variable_initializer* that gives the initial value of the variable.</span></span>

<span data-ttu-id="308a0-200">V kontextu deklarace lokální proměnné funguje var jako kontextové klíčové slovo ([klíčová slova](lexical-structure.md#keywords)). Pokud je *local_variable_type* zadán jako `var` a žádný typ s názvem `var` není v oboru, deklarace je ***implicitně typovou deklarací lokální proměnné***, jejíž typ je odvozen od typu přidruženého inicializátoru. vyjádření.</span><span class="sxs-lookup"><span data-stu-id="308a0-200">In the context of a local variable declaration, the identifier var acts as a contextual keyword ([Keywords](lexical-structure.md#keywords)).When the *local_variable_type* is specified as `var` and no type named `var` is in scope, the declaration is an ***implicitly typed local variable declaration***, whose type is inferred from the type of the associated initializer expression.</span></span> <span data-ttu-id="308a0-201">Implicitně typované deklarace lokálních proměnných podléhá následujícím omezením:</span><span class="sxs-lookup"><span data-stu-id="308a0-201">Implicitly typed local variable declarations are subject to the following restrictions:</span></span>

*  <span data-ttu-id="308a0-202">*Local_variable_declaration* nemůže obsahovat více *local_variable_declarator*s.</span><span class="sxs-lookup"><span data-stu-id="308a0-202">The *local_variable_declaration* cannot include multiple *local_variable_declarator*s.</span></span>
*  <span data-ttu-id="308a0-203">*Local_variable_declarator* musí zahrnovat *local_variable_initializer*.</span><span class="sxs-lookup"><span data-stu-id="308a0-203">The *local_variable_declarator* must include a *local_variable_initializer*.</span></span>
*  <span data-ttu-id="308a0-204">*Local_variable_initializer* musí být *výraz*.</span><span class="sxs-lookup"><span data-stu-id="308a0-204">The *local_variable_initializer* must be an *expression*.</span></span>
*  <span data-ttu-id="308a0-205">*Výraz* inicializátoru musí mít typ pro čas kompilace.</span><span class="sxs-lookup"><span data-stu-id="308a0-205">The initializer *expression* must have a compile-time type.</span></span>
*  <span data-ttu-id="308a0-206">*Výraz* inicializátoru nemůže odkazovat na samotný deklarovaný proměnnou.</span><span class="sxs-lookup"><span data-stu-id="308a0-206">The initializer *expression* cannot refer to the declared variable itself</span></span>

<span data-ttu-id="308a0-207">Následují příklady nesprávných implicitních typů deklarací místních proměnných:</span><span class="sxs-lookup"><span data-stu-id="308a0-207">The following are examples of incorrect implicitly typed local variable declarations:</span></span>

```csharp
var x;               // Error, no initializer to infer type from
var y = {1, 2, 3};   // Error, array initializer not permitted
var z = null;        // Error, null does not have a type
var u = x => x + 1;  // Error, anonymous functions do not have a type
var v = v++;         // Error, initializer cannot refer to variable itself
```

<span data-ttu-id="308a0-208">Hodnota místní proměnné je získána ve výrazu pomocí *simple_name* ([jednoduché názvy](expressions.md#simple-names)) a hodnota lokální proměnné je upravena pomocí *přiřazení* ([operátory přiřazení](expressions.md#assignment-operators)).</span><span class="sxs-lookup"><span data-stu-id="308a0-208">The value of a local variable is obtained in an expression using a *simple_name* ([Simple names](expressions.md#simple-names)), and the value of a local variable is modified using an *assignment* ([Assignment operators](expressions.md#assignment-operators)).</span></span> <span data-ttu-id="308a0-209">Místní proměnná musí být jednoznačně přiřazena ([jednoznačné přiřazení](variables.md#definite-assignment)) na každém místě, kde je jeho hodnota získána.</span><span class="sxs-lookup"><span data-stu-id="308a0-209">A local variable must be definitely assigned ([Definite assignment](variables.md#definite-assignment)) at each location where its value is obtained.</span></span>

<span data-ttu-id="308a0-210">Rozsah místní proměnné deklarované ve *local_variable_declaration* je blok, ve kterém se nachází deklarace.</span><span class="sxs-lookup"><span data-stu-id="308a0-210">The scope of a local variable declared in a *local_variable_declaration* is the block in which the declaration occurs.</span></span> <span data-ttu-id="308a0-211">Jedná se o chybu, která odkazuje na místní proměnnou v textové pozici, která předchází *local_variable_declarator* místní proměnné.</span><span class="sxs-lookup"><span data-stu-id="308a0-211">It is an error to refer to a local variable in a textual position that precedes the *local_variable_declarator* of the local variable.</span></span> <span data-ttu-id="308a0-212">V rámci rozsahu místní proměnné se jedná o chybu při kompilaci k deklaraci jiné místní proměnné nebo konstanty se stejným názvem.</span><span class="sxs-lookup"><span data-stu-id="308a0-212">Within the scope of a local variable, it is a compile-time error to declare another local variable or constant with the same name.</span></span>

<span data-ttu-id="308a0-213">Deklarace místní proměnné, která deklaruje více proměnných, je ekvivalentní více deklaracím jednotlivých proměnných stejného typu.</span><span class="sxs-lookup"><span data-stu-id="308a0-213">A local variable declaration that declares multiple variables is equivalent to multiple declarations of single variables with the same type.</span></span> <span data-ttu-id="308a0-214">Kromě toho inicializátor proměnné v deklaraci lokální proměnné odpovídá přesně příkazu přiřazení, který je vložen ihned po deklaraci.</span><span class="sxs-lookup"><span data-stu-id="308a0-214">Furthermore, a variable initializer in a local variable declaration corresponds exactly to an assignment statement that is inserted immediately after the declaration.</span></span>

<span data-ttu-id="308a0-215">Příklad</span><span class="sxs-lookup"><span data-stu-id="308a0-215">The example</span></span>
```csharp
void F() {
    int x = 1, y, z = x * 2;
}
```
<span data-ttu-id="308a0-216">odpovídá přesně</span><span class="sxs-lookup"><span data-stu-id="308a0-216">corresponds exactly to</span></span>
```csharp
void F() {
    int x; x = 1;
    int y;
    int z; z = x * 2;
}
```

<span data-ttu-id="308a0-217">V implicitní typové deklaraci lokální proměnné je typ deklarované místní proměnné stejný jako typ výrazu použitého k inicializaci proměnné.</span><span class="sxs-lookup"><span data-stu-id="308a0-217">In an implicitly typed local variable declaration, the type of the local variable being declared is taken to be the same as the type of the expression used to initialize the variable.</span></span> <span data-ttu-id="308a0-218">Příklad:</span><span class="sxs-lookup"><span data-stu-id="308a0-218">For example:</span></span>
```csharp
var i = 5;
var s = "Hello";
var d = 1.0;
var numbers = new int[] {1, 2, 3};
var orders = new Dictionary<int,Order>();
```

<span data-ttu-id="308a0-219">Implicitně typové deklarace místních proměnných jsou přesně ekvivalentem následujících explicitně typované deklarace:</span><span class="sxs-lookup"><span data-stu-id="308a0-219">The implicitly typed local variable declarations above are precisely equivalent to the following explicitly typed declarations:</span></span>
```csharp
int i = 5;
string s = "Hello";
double d = 1.0;
int[] numbers = new int[] {1, 2, 3};
Dictionary<int,Order> orders = new Dictionary<int,Order>();
```

### <a name="local-constant-declarations"></a><span data-ttu-id="308a0-220">Deklarace místní konstanty</span><span class="sxs-lookup"><span data-stu-id="308a0-220">Local constant declarations</span></span>

<span data-ttu-id="308a0-221">*Local_constant_declaration* deklaruje jednu nebo více místních konstant.</span><span class="sxs-lookup"><span data-stu-id="308a0-221">A *local_constant_declaration* declares one or more local constants.</span></span>

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

<span data-ttu-id="308a0-222">*Typ* *local_constant_declaration* určuje typ konstant zavedený deklarací.</span><span class="sxs-lookup"><span data-stu-id="308a0-222">The *type* of a *local_constant_declaration* specifies the type of the constants introduced by the declaration.</span></span> <span data-ttu-id="308a0-223">Po typu následuje seznam *constant_declarator*, z nichž každá zavádí novou konstantu.</span><span class="sxs-lookup"><span data-stu-id="308a0-223">The type is followed by a list of *constant_declarator*s, each of which introduces a new constant.</span></span> <span data-ttu-id="308a0-224">*Constant_declarator* se skládá z *identifikátoru* , který obsahuje název konstanty následovaný "`=`" tokenem následovaným *constant_expression* ([konstantními výrazy](expressions.md#constant-expressions)), které poskytují hodnotu konstanty.</span><span class="sxs-lookup"><span data-stu-id="308a0-224">A *constant_declarator* consists of an *identifier* that names the constant, followed by an "`=`" token, followed by a *constant_expression* ([Constant expressions](expressions.md#constant-expressions)) that gives the value of the constant.</span></span>

<span data-ttu-id="308a0-225">*Typ* a *constant_expression* deklarace místní konstanty musí splňovat stejná pravidla jako deklarace konstantního člena ([konstanty](classes.md#constants)).</span><span class="sxs-lookup"><span data-stu-id="308a0-225">The *type* and *constant_expression* of a local constant declaration must follow the same rules as those of a constant member declaration ([Constants](classes.md#constants)).</span></span>

<span data-ttu-id="308a0-226">Hodnota místní konstanty je získána ve výrazu pomocí *simple_name* ([jednoduché názvy](expressions.md#simple-names)).</span><span class="sxs-lookup"><span data-stu-id="308a0-226">The value of a local constant is obtained in an expression using a *simple_name* ([Simple names](expressions.md#simple-names)).</span></span>

<span data-ttu-id="308a0-227">Rozsah místní konstanty je blok, ve kterém k deklaraci dojde.</span><span class="sxs-lookup"><span data-stu-id="308a0-227">The scope of a local constant is the block in which the declaration occurs.</span></span> <span data-ttu-id="308a0-228">Odkaz na místní konstantu v textové pozici, která předchází jeho *constant_declarator*, je chyba.</span><span class="sxs-lookup"><span data-stu-id="308a0-228">It is an error to refer to a local constant in a textual position that precedes its *constant_declarator*.</span></span> <span data-ttu-id="308a0-229">V rámci rozsahu místní konstanty se jedná o chybu při kompilaci k deklaraci jiné místní proměnné nebo konstanty se stejným názvem.</span><span class="sxs-lookup"><span data-stu-id="308a0-229">Within the scope of a local constant, it is a compile-time error to declare another local variable or constant with the same name.</span></span>

<span data-ttu-id="308a0-230">Deklarace místní konstanty, která deklaruje více konstant, je ekvivalentní více deklaracím s jedním konstantou stejného typu.</span><span class="sxs-lookup"><span data-stu-id="308a0-230">A local constant declaration that declares multiple constants is equivalent to multiple declarations of single constants with the same type.</span></span>

## <a name="expression-statements"></a><span data-ttu-id="308a0-231">Příkazy výrazu</span><span class="sxs-lookup"><span data-stu-id="308a0-231">Expression statements</span></span>

<span data-ttu-id="308a0-232">*Expression_statement* vyhodnocuje daný výraz.</span><span class="sxs-lookup"><span data-stu-id="308a0-232">An *expression_statement* evaluates a given expression.</span></span> <span data-ttu-id="308a0-233">Hodnota vypočítaná výrazem, pokud existuje, je zahozena.</span><span class="sxs-lookup"><span data-stu-id="308a0-233">The value computed by the expression, if any, is discarded.</span></span>

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

<span data-ttu-id="308a0-234">Ne všechny výrazy jsou povoleny jako příkazy.</span><span class="sxs-lookup"><span data-stu-id="308a0-234">Not all expressions are permitted as statements.</span></span> <span data-ttu-id="308a0-235">Konkrétně se jako příkazy nepovolují `x + y` výrazy `x == 1` jako a, které pouze počítají hodnotu (která se zahodí).</span><span class="sxs-lookup"><span data-stu-id="308a0-235">In particular, expressions such as `x + y` and `x == 1` that merely compute a value (which will be discarded), are not permitted as statements.</span></span>

<span data-ttu-id="308a0-236">Provedení *expression_statement* vyhodnotí obsažený výraz a poté přenáší řízení na koncový bod *expression_statement*.</span><span class="sxs-lookup"><span data-stu-id="308a0-236">Execution of an *expression_statement* evaluates the contained expression and then transfers control to the end point of the *expression_statement*.</span></span> <span data-ttu-id="308a0-237">Koncový bod *expression_statement* je dosažitelný, pokud je tento *expression_statement* dosažitelný.</span><span class="sxs-lookup"><span data-stu-id="308a0-237">The end point of an *expression_statement* is reachable if that *expression_statement* is reachable.</span></span>

## <a name="selection-statements"></a><span data-ttu-id="308a0-238">Příkazy výběru</span><span class="sxs-lookup"><span data-stu-id="308a0-238">Selection statements</span></span>

<span data-ttu-id="308a0-239">Příkazy výběru vyberou jeden z několika možných příkazů pro spuštění na základě hodnoty nějakého výrazu.</span><span class="sxs-lookup"><span data-stu-id="308a0-239">Selection statements select one of a number of possible statements for execution based on the value of some expression.</span></span>

```antlr
selection_statement
    : if_statement
    | switch_statement
    ;
```

### <a name="the-if-statement"></a><span data-ttu-id="308a0-240">Příkaz if</span><span class="sxs-lookup"><span data-stu-id="308a0-240">The if statement</span></span>

<span data-ttu-id="308a0-241">`if` Příkaz vybere příkaz pro spuštění na základě hodnoty logického výrazu.</span><span class="sxs-lookup"><span data-stu-id="308a0-241">The `if` statement selects a statement for execution based on the value of a boolean expression.</span></span>

```antlr
if_statement
    : 'if' '(' boolean_expression ')' embedded_statement
    | 'if' '(' boolean_expression ')' embedded_statement 'else' embedded_statement
    ;
```

<span data-ttu-id="308a0-242">Část je přidružena k lexikálnímu `if` nejbližšímu, který je povolen syntaxí. `else`</span><span class="sxs-lookup"><span data-stu-id="308a0-242">An `else` part is associated with the lexically nearest preceding `if` that is allowed by the syntax.</span></span> <span data-ttu-id="308a0-243">`if` Proto příkaz formuláře</span><span class="sxs-lookup"><span data-stu-id="308a0-243">Thus, an `if` statement of the form</span></span>
```csharp
if (x) if (y) F(); else G();
```
<span data-ttu-id="308a0-244">je ekvivalentem</span><span class="sxs-lookup"><span data-stu-id="308a0-244">is equivalent to</span></span>
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

<span data-ttu-id="308a0-245">`if` Příkaz se spustí takto:</span><span class="sxs-lookup"><span data-stu-id="308a0-245">An `if` statement is executed as follows:</span></span>

*  <span data-ttu-id="308a0-246">*Boolean_expression* ([booleovské výrazy](expressions.md#boolean-expressions)) jsou vyhodnoceny.</span><span class="sxs-lookup"><span data-stu-id="308a0-246">The *boolean_expression* ([Boolean expressions](expressions.md#boolean-expressions)) is evaluated.</span></span>
*  <span data-ttu-id="308a0-247">Pokud logický výraz vrací `true`, je ovládací prvek převeden do prvního vloženého příkazu.</span><span class="sxs-lookup"><span data-stu-id="308a0-247">If the boolean expression yields `true`, control is transferred to the first embedded statement.</span></span> <span data-ttu-id="308a0-248">Když a když ovládací prvek dosáhne koncového bodu tohoto příkazu, ovládací prvek se převede na koncový bod `if` příkazu.</span><span class="sxs-lookup"><span data-stu-id="308a0-248">When and if control reaches the end point of that statement, control is transferred to the end point of the `if` statement.</span></span>
*  <span data-ttu-id="308a0-249">Pokud logický výraz `false` vyhodnotí a `else` Pokud je část přítomna, řízení je převedeno do druhého vloženého příkazu.</span><span class="sxs-lookup"><span data-stu-id="308a0-249">If the boolean expression yields `false` and if an `else` part is present, control is transferred to the second embedded statement.</span></span> <span data-ttu-id="308a0-250">Když a když ovládací prvek dosáhne koncového bodu tohoto příkazu, ovládací prvek se převede na koncový bod `if` příkazu.</span><span class="sxs-lookup"><span data-stu-id="308a0-250">When and if control reaches the end point of that statement, control is transferred to the end point of the `if` statement.</span></span>
*  <span data-ttu-id="308a0-251">Pokud logický výraz vrací `false` a `else` Pokud část není přítomna, řízení je převedeno na koncový bod `if` příkazu.</span><span class="sxs-lookup"><span data-stu-id="308a0-251">If the boolean expression yields `false` and if an `else` part is not present, control is transferred to the end point of the `if` statement.</span></span>

<span data-ttu-id="308a0-252">První vložený příkaz `if` příkazu je dosažitelný, `if` Pokud je příkaz dosažitelný a logický výraz nemá konstantní hodnotu `false`.</span><span class="sxs-lookup"><span data-stu-id="308a0-252">The first embedded statement of an `if` statement is reachable if the `if` statement is reachable and the boolean expression does not have the constant value `false`.</span></span>

<span data-ttu-id="308a0-253">Druhý vložený příkaz `if` příkazu, je-li k dispozici, je dosažitelný, `if` Pokud je příkaz dosažitelný a logický výraz nemá konstantní hodnotu. `true`</span><span class="sxs-lookup"><span data-stu-id="308a0-253">The second embedded statement of an `if` statement, if present, is reachable if the `if` statement is reachable and the boolean expression does not have the constant value `true`.</span></span>

<span data-ttu-id="308a0-254">Koncový bod `if` příkazu je dosažitelný, pokud je koncový bod alespoň jednoho z jeho vložených příkazů dosažitelný.</span><span class="sxs-lookup"><span data-stu-id="308a0-254">The end point of an `if` statement is reachable if the end point of at least one of its embedded statements is reachable.</span></span> <span data-ttu-id="308a0-255">Kromě `if` toho koncový bod příkazu bez `else` části `if` je dosažitelný, pokud je příkaz dosažitelný a logický výraz nemá konstantní hodnotu `true`.</span><span class="sxs-lookup"><span data-stu-id="308a0-255">In addition, the end point of an `if` statement with no `else` part is reachable if the `if` statement is reachable and the boolean expression does not have the constant value `true`.</span></span>

### <a name="the-switch-statement"></a><span data-ttu-id="308a0-256">Příkaz switch</span><span class="sxs-lookup"><span data-stu-id="308a0-256">The switch statement</span></span>

<span data-ttu-id="308a0-257">Příkaz switch vybere příkaz pro spuštění seznamu příkazů, který má přidružený popisek přepínače, který odpovídá hodnotě výrazu přepínače.</span><span class="sxs-lookup"><span data-stu-id="308a0-257">The switch statement selects for execution a statement list having an associated switch label that corresponds to the value of the switch expression.</span></span>

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

<span data-ttu-id="308a0-258">*Switch_statement* se skládá z klíčového `switch`slova následovaného výrazem v závorkách (nazývaný výraz Switch) následovaným *switch_block*.</span><span class="sxs-lookup"><span data-stu-id="308a0-258">A *switch_statement* consists of the keyword `switch`, followed by a parenthesized expression (called the switch expression), followed by a *switch_block*.</span></span> <span data-ttu-id="308a0-259">*Switch_block* se skládá z nuly nebo více *switch_section*s uzavřenými závorkami.</span><span class="sxs-lookup"><span data-stu-id="308a0-259">The *switch_block* consists of zero or more *switch_section*s, enclosed in braces.</span></span> <span data-ttu-id="308a0-260">Každý *switch_section* se skládá z jednoho nebo více *switch_label*, po kterých následuje *statement_list* ([seznamy příkazů](statements.md#statement-lists)).</span><span class="sxs-lookup"><span data-stu-id="308a0-260">Each *switch_section* consists of one or more *switch_label*s followed by a *statement_list* ([Statement lists](statements.md#statement-lists)).</span></span>

<span data-ttu-id="308a0-261">Typ`switch` ***řízení*** příkazu je vytvořen výrazem přepínače.</span><span class="sxs-lookup"><span data-stu-id="308a0-261">The ***governing type*** of a `switch` statement is established by the switch expression.</span></span>

*  <span data-ttu-id="308a0-262">Pokud je `sbyte`typ výrazu přepínače `ushort`, `byte`, `short` ,,`uint` ,,`long` ,`char`,, ,`string`nebo `int` `ulong` `bool`  *enum_type*, nebo pokud se jedná o typ s možnou hodnotou null odpovídající jednomu z těchto typů, pak to je typ `switch` řízení příkazu.</span><span class="sxs-lookup"><span data-stu-id="308a0-262">If the type of the switch expression is `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `bool`, `char`, `string`, or an *enum_type*, or if it is the nullable type corresponding to one of these types, then that is the governing type of the `switch` statement.</span></span>
*  <span data-ttu-id="308a0-263">V opačném případě musí existovat přesně jeden uživatelem definovaný implicitní převod ([uživatelem definované převody](conversions.md#user-defined-conversions)) z typu výrazu Switch na jeden z následujících možných typů řízení `sbyte`:, `byte`,, `ushort` `short` , `int` ,`uint`, ,`char`,, nebo`string`, typ s možnou hodnotou null, který odpovídá jednomu z těchto typů. `ulong` `long`</span><span class="sxs-lookup"><span data-stu-id="308a0-263">Otherwise, exactly one user-defined implicit conversion ([User-defined conversions](conversions.md#user-defined-conversions)) must exist from the type of the switch expression to one of the following possible governing types: `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `string`, or,  a nullable type corresponding to one of those types.</span></span>
*  <span data-ttu-id="308a0-264">V opačném případě, pokud žádný takový implicitní převod neexistuje nebo pokud existuje více než jeden takový implicitní převod, dojde k chybě při kompilaci.</span><span class="sxs-lookup"><span data-stu-id="308a0-264">Otherwise, if no such implicit conversion exists, or if more than one such implicit conversion exists, a compile-time error occurs.</span></span>

<span data-ttu-id="308a0-265">Konstantní výraz každého `case` popisku musí znamenat hodnotu, která je implicitně převoditelná ([implicitní převod](conversions.md#implicit-conversions)) na typ `switch` řízení příkazu.</span><span class="sxs-lookup"><span data-stu-id="308a0-265">The constant expression of each `case` label must denote a value that is implicitly convertible ([Implicit conversions](conversions.md#implicit-conversions)) to the governing type of the `switch` statement.</span></span> <span data-ttu-id="308a0-266">Pokud dva nebo více `case` jmenovek v rámci stejného `switch` příkazu určuje stejnou konstantní hodnotu, dojde k chybě v době kompilace.</span><span class="sxs-lookup"><span data-stu-id="308a0-266">A compile-time error occurs if two or more `case` labels in the same `switch` statement specify the same constant value.</span></span>

<span data-ttu-id="308a0-267">V příkazu switch může být nejvýše `default` jeden popisek.</span><span class="sxs-lookup"><span data-stu-id="308a0-267">There can be at most one `default` label in a switch statement.</span></span>

<span data-ttu-id="308a0-268">`switch` Příkaz se spustí takto:</span><span class="sxs-lookup"><span data-stu-id="308a0-268">A `switch` statement is executed as follows:</span></span>

*  <span data-ttu-id="308a0-269">Výraz přepínače je vyhodnocen a převeden na typ řízení.</span><span class="sxs-lookup"><span data-stu-id="308a0-269">The switch expression is evaluated and converted to the governing type.</span></span>
*  <span data-ttu-id="308a0-270">Pokud je jedna z konstant určená v `case` popisku v rámci stejného `switch` příkazu rovna hodnotě výrazu Switch, je ovládací prvek převeden do seznamu příkazů po odpovídajícím `case` popisku.</span><span class="sxs-lookup"><span data-stu-id="308a0-270">If one of the constants specified in a `case` label in the same `switch` statement is equal to the value of the switch expression, control is transferred to the statement list following the matched `case` label.</span></span>
*  <span data-ttu-id="308a0-271">Pokud žádná `case` konstanta zadaná v popisku v rámci stejného `switch` příkazu není rovna hodnotě `default` výrazu Switch, a pokud je popisek přítomen, ovládací prvek `default` se přenese do seznamu příkazů za popisek.</span><span class="sxs-lookup"><span data-stu-id="308a0-271">If none of the constants specified in `case` labels in the same `switch` statement is equal to the value of the switch expression, and if a `default` label is present, control is transferred to the statement list following the `default` label.</span></span>
*  <span data-ttu-id="308a0-272">Pokud žádná `case` konstanta zadaná v popisku v rámci stejného `switch` příkazu není rovna hodnotě výrazu Switch, a pokud není k dispozici žádný `default` popisek, `switch` je ovládací prvek převeden na koncový bod příkazu.</span><span class="sxs-lookup"><span data-stu-id="308a0-272">If none of the constants specified in `case` labels in the same `switch` statement is equal to the value of the switch expression, and if no `default` label is present, control is transferred to the end point of the `switch` statement.</span></span>

<span data-ttu-id="308a0-273">Pokud je koncový bod seznamu příkazů oddílu přepínače dosažitelný, dojde k chybě při kompilaci.</span><span class="sxs-lookup"><span data-stu-id="308a0-273">If the end point of the statement list of a switch section is reachable, a compile-time error occurs.</span></span> <span data-ttu-id="308a0-274">Tento postup se označuje jako pravidlo nespadající do rozsahu.</span><span class="sxs-lookup"><span data-stu-id="308a0-274">This is known as the "no fall through" rule.</span></span> <span data-ttu-id="308a0-275">Příklad</span><span class="sxs-lookup"><span data-stu-id="308a0-275">The example</span></span>
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
<span data-ttu-id="308a0-276">je platný, protože žádný oddíl Switch má dosažitelný koncový bod.</span><span class="sxs-lookup"><span data-stu-id="308a0-276">is valid because no switch section has a reachable end point.</span></span> <span data-ttu-id="308a0-277">Na rozdíl od jazyka C++C a provedení oddílu přepínače není povoleno "přejít do" do dalšího oddílu Switch a příklad</span><span class="sxs-lookup"><span data-stu-id="308a0-277">Unlike C and C++, execution of a switch section is not permitted to "fall through" to the next switch section, and the example</span></span>
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
<span data-ttu-id="308a0-278">má za následek chybu při kompilaci.</span><span class="sxs-lookup"><span data-stu-id="308a0-278">results in a compile-time error.</span></span> <span data-ttu-id="308a0-279">V případě provedení oddílu přepínače, který je následován provedením jiného oddílu přepínače, je nutné použít explicitní `goto case` příkaz `goto default` nebo.</span><span class="sxs-lookup"><span data-stu-id="308a0-279">When execution of a switch section is to be followed by execution of another switch section, an explicit `goto case` or `goto default` statement must be used:</span></span>
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

<span data-ttu-id="308a0-280">V *switch_section*je povoleno více popisků.</span><span class="sxs-lookup"><span data-stu-id="308a0-280">Multiple labels are permitted in a *switch_section*.</span></span> <span data-ttu-id="308a0-281">Příklad</span><span class="sxs-lookup"><span data-stu-id="308a0-281">The example</span></span>
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
<span data-ttu-id="308a0-282">je platný.</span><span class="sxs-lookup"><span data-stu-id="308a0-282">is valid.</span></span> <span data-ttu-id="308a0-283">Tento příklad nerušuje pravidlo "nespadají do", protože popisky `case 2:` a `default:` jsou součástí stejného *switch_section*.</span><span class="sxs-lookup"><span data-stu-id="308a0-283">The example does not violate the "no fall through" rule because the labels `case 2:` and `default:` are part of the same *switch_section*.</span></span>

<span data-ttu-id="308a0-284">Pravidlo "nespadající do" zabraňuje společné třídě chyb, ke kterým dojde v C a C++ kdy `break` jsou výrazy náhodně vynechány.</span><span class="sxs-lookup"><span data-stu-id="308a0-284">The "no fall through" rule prevents a common class of bugs that occur in C and C++ when `break` statements are accidentally omitted.</span></span> <span data-ttu-id="308a0-285">Kromě toho z důvodu tohoto pravidla mohou být oddíly `switch` přepínače v příkazu libovolně přeuspořádány bez ovlivnění chování příkazu.</span><span class="sxs-lookup"><span data-stu-id="308a0-285">In addition, because of this rule, the switch sections of a `switch` statement can be arbitrarily rearranged without affecting the behavior of the statement.</span></span> <span data-ttu-id="308a0-286">Například oddíly `switch` výše uvedeného příkazu mohou být vráceny bez ovlivnění chování příkazu:</span><span class="sxs-lookup"><span data-stu-id="308a0-286">For example, the sections of the `switch` statement above can be reversed without affecting the behavior of the statement:</span></span>
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

<span data-ttu-id="308a0-287">Seznam příkazů oddílu přepínače obvykle končí v `break`příkazu, `goto case`nebo `goto default` , ale všechny konstrukce, které vykreslují koncový bod seznamu příkazů, jsou povoleny.</span><span class="sxs-lookup"><span data-stu-id="308a0-287">The statement list of a switch section typically ends in a `break`, `goto case`, or `goto default` statement, but any construct that renders the end point of the statement list unreachable is permitted.</span></span> <span data-ttu-id="308a0-288">Například příkaz, který `while` je ovládán pomocí logického výrazu `true` , je znám tak, že nikdy nedosáhnou koncového bodu.</span><span class="sxs-lookup"><span data-stu-id="308a0-288">For example, a `while` statement controlled by the boolean expression `true` is known to never reach its end point.</span></span> <span data-ttu-id="308a0-289">Stejně tak příkaz `return`nebovždy přenáší řízení jinde a nikdy nedosáhne svého koncového bodu. `throw`</span><span class="sxs-lookup"><span data-stu-id="308a0-289">Likewise, a `throw` or `return` statement always transfers control elsewhere and never reaches its end point.</span></span> <span data-ttu-id="308a0-290">Proto je následující příklad platný:</span><span class="sxs-lookup"><span data-stu-id="308a0-290">Thus, the following example is valid:</span></span>
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

<span data-ttu-id="308a0-291">Typ řízení `switch` příkazu může být typ `string`.</span><span class="sxs-lookup"><span data-stu-id="308a0-291">The governing type of a `switch` statement may be the type `string`.</span></span> <span data-ttu-id="308a0-292">Příklad:</span><span class="sxs-lookup"><span data-stu-id="308a0-292">For example:</span></span>
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

<span data-ttu-id="308a0-293">Podobně jako operátory rovnosti řetězců ([operátory rovnosti řetězců](expressions.md#string-equality-operators)) `switch` , příkaz rozlišuje velká a malá písmena a provede daný oddíl přepínače pouze v `case` případě, že řetězec výrazu Switch přesně odpovídá konstantě popisku.</span><span class="sxs-lookup"><span data-stu-id="308a0-293">Like the string equality operators ([String equality operators](expressions.md#string-equality-operators)), the `switch` statement is case sensitive and will execute a given switch section only if the switch expression string exactly matches a `case` label constant.</span></span>

<span data-ttu-id="308a0-294">Pokud je `switch` `string`typ řízení příkazu, hodnota `null` je povolena jako konstanta jmenovky Case.</span><span class="sxs-lookup"><span data-stu-id="308a0-294">When the governing type of a `switch` statement is `string`, the value `null` is permitted as a case label constant.</span></span>

<span data-ttu-id="308a0-295">*Statement_list*s *switch_block* může obsahovat příkazy deklarace ([příkazy deklarace](statements.md#declaration-statements)).</span><span class="sxs-lookup"><span data-stu-id="308a0-295">The *statement_list*s of a *switch_block* may contain declaration statements ([Declaration statements](statements.md#declaration-statements)).</span></span> <span data-ttu-id="308a0-296">Rozsah místní proměnné nebo konstanty deklarované v bloku Switch je blok přepínače.</span><span class="sxs-lookup"><span data-stu-id="308a0-296">The scope of a local variable or constant declared in a switch block is the switch block.</span></span>

<span data-ttu-id="308a0-297">Seznam příkazů daného oddílu přepínače je dosažitelný, pokud `switch` je příkaz dosažitelný a alespoň jedna z následujících možností je pravdivá:</span><span class="sxs-lookup"><span data-stu-id="308a0-297">The statement list of a given switch section is reachable if the `switch` statement is reachable and at least one of the following is true:</span></span>

*  <span data-ttu-id="308a0-298">Výraz přepínače je nekonstantní hodnota.</span><span class="sxs-lookup"><span data-stu-id="308a0-298">The switch expression is a non-constant value.</span></span>
*  <span data-ttu-id="308a0-299">Výraz přepínače je konstantní hodnota, která odpovídá `case` popisku v části Switch.</span><span class="sxs-lookup"><span data-stu-id="308a0-299">The switch expression is a constant value that matches a `case` label in the switch section.</span></span>
*  <span data-ttu-id="308a0-300">Výraz přepínače je konstantní hodnota, která neodpovídá žádnému `case` popisku a oddíl Switch `default` obsahuje popisek.</span><span class="sxs-lookup"><span data-stu-id="308a0-300">The switch expression is a constant value that doesn't match any `case` label, and the switch section contains the `default` label.</span></span>
*  <span data-ttu-id="308a0-301">Na jmenovku přepínače oddílu Switch se odkazuje dosažitelný `goto case` příkaz nebo. `goto default`</span><span class="sxs-lookup"><span data-stu-id="308a0-301">A switch label of the switch section is referenced by a reachable `goto case` or `goto default` statement.</span></span>

<span data-ttu-id="308a0-302">Koncový bod `switch` příkazu je dosažitelný, pokud je splněna alespoň jedna z následujících podmínek:</span><span class="sxs-lookup"><span data-stu-id="308a0-302">The end point of a `switch` statement is reachable if at least one of the following is true:</span></span>

*  <span data-ttu-id="308a0-303">Příkaz obsahuje `break` dostupný příkaz, který ukončí příkaz.`switch` `switch`</span><span class="sxs-lookup"><span data-stu-id="308a0-303">The `switch` statement contains a reachable `break` statement that exits the `switch` statement.</span></span>
*  <span data-ttu-id="308a0-304">Příkaz je dosažitelný, výraz přepínače je nekonstantní hodnota a není k dispozici žádný `default` popisek. `switch`</span><span class="sxs-lookup"><span data-stu-id="308a0-304">The `switch` statement is reachable, the switch expression is a non-constant value, and no `default` label is present.</span></span>
*  <span data-ttu-id="308a0-305">Příkaz je dosažitelný, výraz přepínače je konstantní hodnota, která neodpovídá žádnému `case` popisku a není k dispozici `default` žádný popisek. `switch`</span><span class="sxs-lookup"><span data-stu-id="308a0-305">The `switch` statement is reachable, the switch expression is a constant value that doesn't match any `case` label, and no `default` label is present.</span></span>

## <a name="iteration-statements"></a><span data-ttu-id="308a0-306">Příkazy iterace</span><span class="sxs-lookup"><span data-stu-id="308a0-306">Iteration statements</span></span>

<span data-ttu-id="308a0-307">Příkazy iterace opakovaně spouštějí vložený příkaz.</span><span class="sxs-lookup"><span data-stu-id="308a0-307">Iteration statements repeatedly execute an embedded statement.</span></span>

```antlr
iteration_statement
    : while_statement
    | do_statement
    | for_statement
    | foreach_statement
    ;
```

### <a name="the-while-statement"></a><span data-ttu-id="308a0-308">Příkaz While</span><span class="sxs-lookup"><span data-stu-id="308a0-308">The while statement</span></span>

<span data-ttu-id="308a0-309">`while` Příkaz podmíněně spustí vložený příkaz nula nebo vícekrát.</span><span class="sxs-lookup"><span data-stu-id="308a0-309">The `while` statement conditionally executes an embedded statement zero or more times.</span></span>

```antlr
while_statement
    : 'while' '(' boolean_expression ')' embedded_statement
    ;
```

<span data-ttu-id="308a0-310">`while` Příkaz se spustí takto:</span><span class="sxs-lookup"><span data-stu-id="308a0-310">A `while` statement is executed as follows:</span></span>

*  <span data-ttu-id="308a0-311">*Boolean_expression* ([booleovské výrazy](expressions.md#boolean-expressions)) jsou vyhodnoceny.</span><span class="sxs-lookup"><span data-stu-id="308a0-311">The *boolean_expression* ([Boolean expressions](expressions.md#boolean-expressions)) is evaluated.</span></span>
*  <span data-ttu-id="308a0-312">Pokud logický výraz vrací `true`, je ovládací prvek převeden do vloženého příkazu.</span><span class="sxs-lookup"><span data-stu-id="308a0-312">If the boolean expression yields `true`, control is transferred to the embedded statement.</span></span> <span data-ttu-id="308a0-313">Když a pokud ovládací prvek dosáhne koncového bodu vloženého příkazu (případně z provádění `continue` příkazu), řízení je převedeno na začátek `while` příkazu.</span><span class="sxs-lookup"><span data-stu-id="308a0-313">When and if control reaches the end point of the embedded statement (possibly from execution of a `continue` statement), control is transferred to the beginning of the `while` statement.</span></span>
*  <span data-ttu-id="308a0-314">Je-li logický výraz `false`výsledkem, je ovládací prvek převeden na koncový bod `while` příkazu.</span><span class="sxs-lookup"><span data-stu-id="308a0-314">If the boolean expression yields `false`, control is transferred to the end point of the `while` statement.</span></span>

<span data-ttu-id="308a0-315">`while` V rámci vloženého příkazu `break` příkazu může být příkaz ([příkaz break](statements.md#the-break-statement)) použit k přenosu `while` ovládacího prvku na koncový bod příkazu (takže koncová iterace vloženého příkazu) a `continue` příkaz ([příkaz Continue](statements.md#the-continue-statement)) lze použít k přenosu ovládacího prvku do koncového bodu vloženého příkazu (čímž se provádí další `while` iterace příkazu).</span><span class="sxs-lookup"><span data-stu-id="308a0-315">Within the embedded statement of a `while` statement, a `break` statement ([The break statement](statements.md#the-break-statement)) may be used to transfer control to the end point of the `while` statement (thus ending iteration of the embedded statement), and a `continue` statement ([The continue statement](statements.md#the-continue-statement)) may be used to transfer control to the end point of the embedded statement (thus performing another iteration of the `while` statement).</span></span>

<span data-ttu-id="308a0-316">Vložený příkaz `while` příkazu je dosažitelný, `while` Pokud je příkaz dosažitelný a logický výraz nemá konstantní hodnotu `false`.</span><span class="sxs-lookup"><span data-stu-id="308a0-316">The embedded statement of a `while` statement is reachable if the `while` statement is reachable and the boolean expression does not have the constant value `false`.</span></span>

<span data-ttu-id="308a0-317">Koncový bod `while` příkazu je dosažitelný, pokud je splněna alespoň jedna z následujících podmínek:</span><span class="sxs-lookup"><span data-stu-id="308a0-317">The end point of a `while` statement is reachable if at least one of the following is true:</span></span>

*  <span data-ttu-id="308a0-318">Příkaz obsahuje `break` dostupný příkaz, který ukončí příkaz.`while` `while`</span><span class="sxs-lookup"><span data-stu-id="308a0-318">The `while` statement contains a reachable `break` statement that exits the `while` statement.</span></span>
*  <span data-ttu-id="308a0-319">Příkaz je dosažitelný a logický výraz nemá konstantní hodnotu `true`. `while`</span><span class="sxs-lookup"><span data-stu-id="308a0-319">The `while` statement is reachable and the boolean expression does not have the constant value `true`.</span></span>

### <a name="the-do-statement"></a><span data-ttu-id="308a0-320">Příkaz do</span><span class="sxs-lookup"><span data-stu-id="308a0-320">The do statement</span></span>

<span data-ttu-id="308a0-321">`do` Příkaz podmíněně spustí vložený příkaz jednou nebo vícekrát.</span><span class="sxs-lookup"><span data-stu-id="308a0-321">The `do` statement conditionally executes an embedded statement one or more times.</span></span>

```antlr
do_statement
    : 'do' embedded_statement 'while' '(' boolean_expression ')' ';'
    ;
```

<span data-ttu-id="308a0-322">`do` Příkaz se spustí takto:</span><span class="sxs-lookup"><span data-stu-id="308a0-322">A `do` statement is executed as follows:</span></span>

*  <span data-ttu-id="308a0-323">Ovládací prvek se přenese do vloženého příkazu.</span><span class="sxs-lookup"><span data-stu-id="308a0-323">Control is transferred to the embedded statement.</span></span>
*  <span data-ttu-id="308a0-324">Když a pokud ovládací prvek dosáhne koncového bodu vloženého příkazu (případně z provádění `continue` příkazu), je vyhodnocen *Boolean_expression* ([booleovské výrazy](expressions.md#boolean-expressions)).</span><span class="sxs-lookup"><span data-stu-id="308a0-324">When and if control reaches the end point of the embedded statement (possibly from execution of a `continue` statement), the *boolean_expression* ([Boolean expressions](expressions.md#boolean-expressions)) is evaluated.</span></span> <span data-ttu-id="308a0-325">Je-li logický výraz `true`výsledkem, je ovládací prvek převeden na začátek `do` příkazu.</span><span class="sxs-lookup"><span data-stu-id="308a0-325">If the boolean expression yields `true`, control is transferred to the beginning of the `do` statement.</span></span> <span data-ttu-id="308a0-326">V opačném případě je ovládací prvek převeden na koncový bod `do` příkazu.</span><span class="sxs-lookup"><span data-stu-id="308a0-326">Otherwise, control is transferred to the end point of the `do` statement.</span></span>

<span data-ttu-id="308a0-327">`do` V rámci vloženého příkazu `break` příkazu může být příkaz ([příkaz break](statements.md#the-break-statement)) použit k přenosu `do` ovládacího prvku na koncový bod příkazu (takže koncová iterace vloženého příkazu) a `continue` příkaz ([příkaz Continue](statements.md#the-continue-statement)) lze použít k přenosu řízení na koncový bod vloženého příkazu.</span><span class="sxs-lookup"><span data-stu-id="308a0-327">Within the embedded statement of a `do` statement, a `break` statement ([The break statement](statements.md#the-break-statement)) may be used to transfer control to the end point of the `do` statement (thus ending iteration of the embedded statement), and a `continue` statement ([The continue statement](statements.md#the-continue-statement)) may be used to transfer control to the end point of the embedded statement.</span></span>

<span data-ttu-id="308a0-328">Vložený příkaz `do` příkazu je dosažitelný, `do` Pokud je příkaz dosažitelný.</span><span class="sxs-lookup"><span data-stu-id="308a0-328">The embedded statement of a `do` statement is reachable if the `do` statement is reachable.</span></span>

<span data-ttu-id="308a0-329">Koncový bod `do` příkazu je dosažitelný, pokud je splněna alespoň jedna z následujících podmínek:</span><span class="sxs-lookup"><span data-stu-id="308a0-329">The end point of a `do` statement is reachable if at least one of the following is true:</span></span>

*  <span data-ttu-id="308a0-330">Příkaz obsahuje `break` dostupný příkaz, který ukončí příkaz.`do` `do`</span><span class="sxs-lookup"><span data-stu-id="308a0-330">The `do` statement contains a reachable `break` statement that exits the `do` statement.</span></span>
*  <span data-ttu-id="308a0-331">Koncový bod vloženého příkazu je dosažitelný a logický výraz nemá konstantní hodnotu `true`.</span><span class="sxs-lookup"><span data-stu-id="308a0-331">The end point of the embedded statement is reachable and the boolean expression does not have the constant value `true`.</span></span>

### <a name="the-for-statement"></a><span data-ttu-id="308a0-332">Příkaz for</span><span class="sxs-lookup"><span data-stu-id="308a0-332">The for statement</span></span>

<span data-ttu-id="308a0-333">`for` Příkaz vyhodnocuje sekvenci inicializačních výrazů a potom, zatímco je podmínka pravdivá, opakovaně spouští vložený příkaz a vyhodnocuje sekvenci výrazů iterace.</span><span class="sxs-lookup"><span data-stu-id="308a0-333">The `for` statement evaluates a sequence of initialization expressions and then, while a condition is true, repeatedly executes an embedded statement and evaluates a sequence of iteration expressions.</span></span>

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

<span data-ttu-id="308a0-334">*For_initializer*, pokud je k dispozici, se skládá buď z *local_variable_declaration* ([místní proměnná deklarace](statements.md#local-variable-declarations)), nebo seznamu *statement_expression*s ([příkazy výrazu](statements.md#expression-statements)) oddělené čárkami.</span><span class="sxs-lookup"><span data-stu-id="308a0-334">The *for_initializer*, if present, consists of either a *local_variable_declaration* ([Local variable declarations](statements.md#local-variable-declarations)) or a list of *statement_expression*s ([Expression statements](statements.md#expression-statements)) separated by commas.</span></span> <span data-ttu-id="308a0-335">Rozsah místní proměnné deklarované *for_initializer* začíná na *local_variable_declarator* pro proměnnou a rozšiřuje na konec vloženého příkazu.</span><span class="sxs-lookup"><span data-stu-id="308a0-335">The scope of a local variable declared by a *for_initializer* starts at the *local_variable_declarator* for the variable and extends to the end of the embedded statement.</span></span> <span data-ttu-id="308a0-336">Obor zahrnuje *for_condition* a *for_iterator*.</span><span class="sxs-lookup"><span data-stu-id="308a0-336">The scope includes the *for_condition* and the *for_iterator*.</span></span>

<span data-ttu-id="308a0-337">*For_condition*, pokud je přítomen, musí být *Boolean_expression* ([booleovské výrazy](expressions.md#boolean-expressions)).</span><span class="sxs-lookup"><span data-stu-id="308a0-337">The *for_condition*, if present, must be a *boolean_expression* ([Boolean expressions](expressions.md#boolean-expressions)).</span></span>

<span data-ttu-id="308a0-338">*For_iterator*, pokud je k dispozici, se skládá ze seznamu *statement_expression*s ([příkazy výrazu](statements.md#expression-statements)), které jsou odděleny čárkami.</span><span class="sxs-lookup"><span data-stu-id="308a0-338">The *for_iterator*, if present, consists of a list of *statement_expression*s ([Expression statements](statements.md#expression-statements)) separated by commas.</span></span>

<span data-ttu-id="308a0-339">Příkaz for se spustí takto:</span><span class="sxs-lookup"><span data-stu-id="308a0-339">A for statement is executed as follows:</span></span>

*  <span data-ttu-id="308a0-340">Pokud je přítomen *for_initializer* , Inicializátory proměnných nebo výrazy příkazů jsou spouštěny v pořadí, ve kterém jsou zapsány.</span><span class="sxs-lookup"><span data-stu-id="308a0-340">If a *for_initializer* is present, the variable initializers or statement expressions are executed in the order they are written.</span></span> <span data-ttu-id="308a0-341">Tento krok se provádí jenom jednou.</span><span class="sxs-lookup"><span data-stu-id="308a0-341">This step is only performed once.</span></span>
*  <span data-ttu-id="308a0-342">Pokud je přítomen *for_condition* , vyhodnotí se.</span><span class="sxs-lookup"><span data-stu-id="308a0-342">If a *for_condition* is present, it is evaluated.</span></span>
*  <span data-ttu-id="308a0-343">Pokud *for_condition* není k dispozici nebo pokud dojde k vyhodnocení `true`, řízení se přenese do vloženého příkazu.</span><span class="sxs-lookup"><span data-stu-id="308a0-343">If the *for_condition* is not present or if the evaluation yields `true`, control is transferred to the embedded statement.</span></span> <span data-ttu-id="308a0-344">Když a pokud ovládací prvek dosáhne koncového bodu vloženého příkazu (případně z provádění `continue` příkazu), jsou výrazy *for_iterator*, pokud existují, vyhodnocovány v sekvenci a poté je provedena další iterace, počínaje vyhodnocení *for_condition* v kroku výše.</span><span class="sxs-lookup"><span data-stu-id="308a0-344">When and if control reaches the end point of the embedded statement (possibly from execution of a `continue` statement), the expressions of the *for_iterator*, if any, are evaluated in sequence, and then another iteration is performed, starting with evaluation of the *for_condition* in the step above.</span></span>
*  <span data-ttu-id="308a0-345">Pokud je přítomen *for_condition* a vyhodnocení `false`, řízení je převedeno na `for` koncový bod příkazu.</span><span class="sxs-lookup"><span data-stu-id="308a0-345">If the *for_condition* is present and the evaluation yields `false`, control is transferred to the end point of the `for` statement.</span></span>

<span data-ttu-id="308a0-346">`for` V rámci vloženého příkazu `break` příkazu může být příkaz ([příkaz break](statements.md#the-break-statement)) použit k přenosu `for` ovládacího prvku na koncový bod příkazu (takže koncová iterace vloženého příkazu) a `continue` příkaz ([příkaz Continue](statements.md#the-continue-statement)) se dá použít k přenosu řízení na koncový bod vloženého příkazu (takže se spustí *for_iterator* a `for` provádí se další iterace příkazu, počínaje *for_condition*).</span><span class="sxs-lookup"><span data-stu-id="308a0-346">Within the embedded statement of a `for` statement, a `break` statement ([The break statement](statements.md#the-break-statement)) may be used to transfer control to the end point of the `for` statement (thus ending iteration of the embedded statement), and a `continue` statement ([The continue statement](statements.md#the-continue-statement)) may be used to transfer control to the end point of the embedded statement (thus executing the *for_iterator* and performing another iteration of the `for` statement, starting with the *for_condition*).</span></span>

<span data-ttu-id="308a0-347">Vložený příkaz `for` příkazu je dosažitelný, pokud je splněna jedna z následujících podmínek:</span><span class="sxs-lookup"><span data-stu-id="308a0-347">The embedded statement of a `for` statement is reachable if one of the following is true:</span></span>

*  <span data-ttu-id="308a0-348">Příkaz je dosažitelný a není k dispozici žádný *for_condition.* `for`</span><span class="sxs-lookup"><span data-stu-id="308a0-348">The `for` statement is reachable and no *for_condition* is present.</span></span>
*  <span data-ttu-id="308a0-349">Příkaz je dosažitelný a je přítomen *for_condition* a nemá konstantní hodnotu `false`. `for`</span><span class="sxs-lookup"><span data-stu-id="308a0-349">The `for` statement is reachable and a *for_condition* is present and does not have the constant value `false`.</span></span>

<span data-ttu-id="308a0-350">Koncový bod `for` příkazu je dosažitelný, pokud je splněna alespoň jedna z následujících podmínek:</span><span class="sxs-lookup"><span data-stu-id="308a0-350">The end point of a `for` statement is reachable if at least one of the following is true:</span></span>

*  <span data-ttu-id="308a0-351">Příkaz obsahuje `break` dostupný příkaz, který ukončí příkaz.`for` `for`</span><span class="sxs-lookup"><span data-stu-id="308a0-351">The `for` statement contains a reachable `break` statement that exits the `for` statement.</span></span>
*  <span data-ttu-id="308a0-352">Příkaz je dosažitelný a je přítomen *for_condition* a nemá konstantní hodnotu `true`. `for`</span><span class="sxs-lookup"><span data-stu-id="308a0-352">The `for` statement is reachable and a *for_condition* is present and does not have the constant value `true`.</span></span>

### <a name="the-foreach-statement"></a><span data-ttu-id="308a0-353">Příkaz foreach</span><span class="sxs-lookup"><span data-stu-id="308a0-353">The foreach statement</span></span>

<span data-ttu-id="308a0-354">`foreach` Příkaz vytvoří výčet prvků kolekce a spouští vložený příkaz pro každý prvek kolekce.</span><span class="sxs-lookup"><span data-stu-id="308a0-354">The `foreach` statement enumerates the elements of a collection, executing an embedded statement for each element of the collection.</span></span>

```antlr
foreach_statement
    : 'foreach' '(' local_variable_type identifier 'in' expression ')' embedded_statement
    ;
```

<span data-ttu-id="308a0-355">*Typ* a *identifikátor* `foreach` příkazu deklaruje ***proměnnou iterace*** příkazu.</span><span class="sxs-lookup"><span data-stu-id="308a0-355">The *type* and *identifier* of a `foreach` statement declare the ***iteration variable*** of the statement.</span></span> <span data-ttu-id="308a0-356">Pokud je `var`identifikátor přidělen jako local_variable_type a žádný typ s názvem není v oboru, proměnná iterace je označována jako implicitně typovou proměnnou iterace a jejím typem je typ prvku `var` `foreach` , jak je uvedeno níže.</span><span class="sxs-lookup"><span data-stu-id="308a0-356">If the `var` identifier is given as the *local_variable_type*, and no type named `var` is in scope, the iteration variable is said to be an ***implicitly typed iteration variable***, and its type is taken to be the element type of the `foreach` statement, as specified below.</span></span> <span data-ttu-id="308a0-357">Proměnná iterace odpovídá místní proměnné určené jen pro čtení s oborem, který se rozšíří přes vložený příkaz.</span><span class="sxs-lookup"><span data-stu-id="308a0-357">The iteration variable corresponds to a read-only local variable with a scope that extends over the embedded statement.</span></span> <span data-ttu-id="308a0-358">Během provádění `foreach` příkazu představuje proměnná iterace prvek kolekce, pro který je iterace právě prováděna.</span><span class="sxs-lookup"><span data-stu-id="308a0-358">During execution of a `foreach` statement, the iteration variable represents the collection element for which an iteration is currently being performed.</span></span> <span data-ttu-id="308a0-359">K chybě při kompilaci dojde v případě, že se vložený příkaz pokusí změnit proměnnou iterace (prostřednictvím přiřazení nebo `++` operátorů a `--` ) nebo předat proměnnou iterace jako `ref` parametr `out` or.</span><span class="sxs-lookup"><span data-stu-id="308a0-359">A compile-time error occurs if the embedded statement attempts to modify the iteration variable (via assignment or the `++` and `--` operators) or pass the iteration variable as a `ref` or `out` parameter.</span></span>

<span data-ttu-id="308a0-360">V následujících případech pro `IEnumerable`zkrácení, `IEnumerator` `IEnumerable<T>` , a `IEnumerator<T>` odkazují na odpovídající typy v oborech názvů `System.Collections` a `System.Collections.Generic`.</span><span class="sxs-lookup"><span data-stu-id="308a0-360">In the following, for brevity, `IEnumerable`, `IEnumerator`, `IEnumerable<T>` and `IEnumerator<T>` refer to the corresponding types in the namespaces `System.Collections` and `System.Collections.Generic`.</span></span>

<span data-ttu-id="308a0-361">Nejprve zpracování příkazu foreach v době kompilace Určuje ***typ kolekce***, ***typ enumerátoru*** a ***typ elementu*** výrazu.</span><span class="sxs-lookup"><span data-stu-id="308a0-361">The compile-time processing of a foreach statement first determines the ***collection type***, ***enumerator type*** and ***element type*** of the expression.</span></span> <span data-ttu-id="308a0-362">Toto určení pokračuje následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="308a0-362">This determination proceeds as follows:</span></span>

*  <span data-ttu-id="308a0-363">Pokud typ `X` *výrazu* je typ pole, pak existuje implicitní `X` referenční `IEnumerable` převod z na rozhraní (od `System.Array` implementace tohoto rozhraní).</span><span class="sxs-lookup"><span data-stu-id="308a0-363">If the type `X` of *expression* is an array type then there is an implicit reference conversion from `X` to the `IEnumerable` interface (since `System.Array` implements this interface).</span></span> <span data-ttu-id="308a0-364">***Typ kolekce*** je `IEnumerable` rozhraní, ***typ enumerátoru*** je `IEnumerator` rozhraní a ***typ prvku*** je typ prvku typu `X`pole.</span><span class="sxs-lookup"><span data-stu-id="308a0-364">The ***collection type*** is the `IEnumerable` interface, the ***enumerator type*** is the `IEnumerator` interface and the ***element type*** is the element type of the array type `X`.</span></span>
*  <span data-ttu-id="308a0-365">Pokud je `X` `IEnumerable` [](conversions.md#implicit-dynamic-conversions)typ výrazu poté, existuje implicitní převod z výrazu na rozhraní (implicitní dynamické převody). `dynamic`</span><span class="sxs-lookup"><span data-stu-id="308a0-365">If the type `X` of *expression* is `dynamic` then there is an implicit conversion from *expression* to the `IEnumerable` interface ([Implicit dynamic conversions](conversions.md#implicit-dynamic-conversions)).</span></span> <span data-ttu-id="308a0-366">***Typ kolekce*** je `IEnumerable` rozhraní a ***typ enumerátoru*** je `IEnumerator` rozhraní.</span><span class="sxs-lookup"><span data-stu-id="308a0-366">The ***collection type*** is the `IEnumerable` interface and the ***enumerator type*** is the `IEnumerator` interface.</span></span> <span data-ttu-id="308a0-367">`dynamic` `object` Pokud je identifikátorpřidělenjakolocal_variable_type,pakjetypprvku,jinak.`var`</span><span class="sxs-lookup"><span data-stu-id="308a0-367">If the `var` identifier is given as the *local_variable_type* then the ***element type*** is `dynamic`, otherwise it is `object`.</span></span>
*  <span data-ttu-id="308a0-368">V opačném případě určete, `X` zda má typ `GetEnumerator` odpovídající metodu:</span><span class="sxs-lookup"><span data-stu-id="308a0-368">Otherwise, determine whether the type `X` has an appropriate `GetEnumerator` method:</span></span>
   * <span data-ttu-id="308a0-369">Proveďte vyhledávání členů u typu `X` s identifikátorem `GetEnumerator` bez argumentů typu.</span><span class="sxs-lookup"><span data-stu-id="308a0-369">Perform member lookup on the type `X` with identifier `GetEnumerator` and no type arguments.</span></span> <span data-ttu-id="308a0-370">Pokud vyhledávání členů nevytvoří shodu, nebo vytvoří nejednoznačnost, nebo vytvoří shodu, která není skupinou metod, vyhledejte vyčíslitelné rozhraní, jak je popsáno níže.</span><span class="sxs-lookup"><span data-stu-id="308a0-370">If the member lookup does not produce a match, or it produces an ambiguity, or produces a match that is not a method group, check for an enumerable interface as described below.</span></span> <span data-ttu-id="308a0-371">Doporučuje se vystavovat upozornění, pokud vyhledávání členů vytvoří cokoli s výjimkou skupiny metod nebo neodpovídá.</span><span class="sxs-lookup"><span data-stu-id="308a0-371">It is recommended that a warning be issued if member lookup produces anything except a method group or no match.</span></span>
   * <span data-ttu-id="308a0-372">Proveďte rozlišení přetížení pomocí výsledné skupiny metod a prázdného seznamu argumentů.</span><span class="sxs-lookup"><span data-stu-id="308a0-372">Perform overload resolution using the resulting method group and an empty argument list.</span></span> <span data-ttu-id="308a0-373">Pokud výsledkem rozlišení přetížení nejsou žádné použitelné metody, výsledkem je nejednoznačnost nebo výsledkem je jediná nejlepší metoda, ale tato metoda je statická nebo není veřejná, vyhledejte vyčíslitelné rozhraní, jak je popsáno níže.</span><span class="sxs-lookup"><span data-stu-id="308a0-373">If overload resolution results in no applicable methods, results in an ambiguity, or results in a single best method but that method is either static or not public, check for an enumerable interface as described below.</span></span> <span data-ttu-id="308a0-374">Doporučuje se vydávat upozornění, pokud řešení přetížení vytvoří cokoli kromě nejednoznačné metody veřejné instance nebo žádné použitelné metody.</span><span class="sxs-lookup"><span data-stu-id="308a0-374">It is recommended that a warning be issued if overload resolution produces anything except an unambiguous public instance method or no applicable methods.</span></span>
   * <span data-ttu-id="308a0-375">Pokud návratový typ `E` `GetEnumerator` metody není typu třída, struktura ani typ rozhraní, je vytvořena chyba a nejsou provedeny žádné další kroky.</span><span class="sxs-lookup"><span data-stu-id="308a0-375">If the return type `E` of the `GetEnumerator` method is not a class, struct or interface type, an error is produced and no further steps are taken.</span></span>
   * <span data-ttu-id="308a0-376">Vyhledávání členů se provádí `E` s identifikátorem `Current` bez argumentů typu.</span><span class="sxs-lookup"><span data-stu-id="308a0-376">Member lookup is performed on `E` with the identifier `Current` and no type arguments.</span></span> <span data-ttu-id="308a0-377">Pokud vyhledávání členů nevrátí žádnou shodu, výsledkem je chyba, nebo výsledkem je cokoli s výjimkou veřejné vlastnosti instance, která umožňuje čtení, je vytvořena chyba a nejsou provedeny žádné další kroky.</span><span class="sxs-lookup"><span data-stu-id="308a0-377">If the member lookup produces no match, the result is an error, or the result is anything except a public instance property that permits reading, an error is produced and no further steps are taken.</span></span>
   * <span data-ttu-id="308a0-378">Vyhledávání členů se provádí `E` s identifikátorem `MoveNext` bez argumentů typu.</span><span class="sxs-lookup"><span data-stu-id="308a0-378">Member lookup is performed on `E` with the identifier `MoveNext` and no type arguments.</span></span> <span data-ttu-id="308a0-379">Pokud vyhledávání členů nevrátí žádnou shodu, výsledkem je chyba, nebo je výsledek cokoli kromě skupiny metod, je vytvořena chyba a nejsou provedeny žádné další kroky.</span><span class="sxs-lookup"><span data-stu-id="308a0-379">If the member lookup produces no match, the result is an error, or the result is anything except a method group, an error is produced and no further steps are taken.</span></span>
   * <span data-ttu-id="308a0-380">Rozlišení přetěžování se provádí ve skupině metod s prázdným seznamem argumentů.</span><span class="sxs-lookup"><span data-stu-id="308a0-380">Overload resolution is performed on the method group with an empty argument list.</span></span> <span data-ttu-id="308a0-381">Pokud výsledkem rozlišení přetížení nejsou žádné použitelné metody, výsledkem je nejednoznačnost nebo výsledkem je jediná nejlepší metoda, ale tato metoda je buď statická, nebo není veřejná, nebo její návratový typ `bool`není, je vytvořena chyba a nejsou provedeny žádné další kroky.</span><span class="sxs-lookup"><span data-stu-id="308a0-381">If overload resolution results in no applicable methods, results in an ambiguity, or results in a single best method but that method is either static or not public, or its return type is not `bool`, an error is produced and no further steps are taken.</span></span>
   * <span data-ttu-id="308a0-382">***Typ kolekce*** je `X`, ***typ enumerátoru*** je `E` `Current` a ***typ elementu*** je typ vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="308a0-382">The ***collection type*** is `X`, the ***enumerator type*** is `E`, and the ***element type*** is the type of the `Current` property.</span></span>

*  <span data-ttu-id="308a0-383">V opačném případě Ověřte vyčíslitelné rozhraní:</span><span class="sxs-lookup"><span data-stu-id="308a0-383">Otherwise, check for an enumerable interface:</span></span>
   * <span data-ttu-id="308a0-384">Pokud je mezi všemi typy `Ti` , pro které existuje implicitní převod z `X` na `IEnumerable<Ti>`, existuje jedinečný typ `T` , který `T` `dynamic` není a pro všechny ostatní `Ti` . implicitní převod z `IEnumerable<T>` na `IEnumerable<Ti>`, potom ***typ kolekce*** `IEnumerable<T>`je rozhraní, ***typ enumerátoru*** je rozhraní `IEnumerator<T>`a ***typ elementu*** je `T`.</span><span class="sxs-lookup"><span data-stu-id="308a0-384">If among all the types `Ti` for which there is an implicit conversion from `X` to `IEnumerable<Ti>`, there is a unique type `T` such that `T` is not `dynamic` and for all the other `Ti` there is an implicit conversion from `IEnumerable<T>` to `IEnumerable<Ti>`, then the ***collection type*** is the interface `IEnumerable<T>`, the ***enumerator type*** is the interface `IEnumerator<T>`, and the ***element type*** is `T`.</span></span>
   * <span data-ttu-id="308a0-385">V opačném případě, pokud existuje více než jeden `T`takový typ, je vytvořena chyba a nejsou provedeny žádné další kroky.</span><span class="sxs-lookup"><span data-stu-id="308a0-385">Otherwise, if there is more than one such type `T`, then an error is produced and no further steps are taken.</span></span>
   * <span data-ttu-id="308a0-386">V opačném případě, pokud existuje implicitní převod `X` z `System.Collections.IEnumerable` na rozhraní, pak ***typ kolekce*** je toto rozhraní, ***typ enumerátoru*** je rozhraní `System.Collections.IEnumerator`a ***typ elementu*** je `object`.</span><span class="sxs-lookup"><span data-stu-id="308a0-386">Otherwise, if there is an implicit conversion from `X` to the `System.Collections.IEnumerable` interface, then the ***collection type*** is this interface, the ***enumerator type*** is the interface `System.Collections.IEnumerator`, and the ***element type*** is `object`.</span></span>
   * <span data-ttu-id="308a0-387">V opačném případě se vytvoří chyba a neprovádí se žádné další kroky.</span><span class="sxs-lookup"><span data-stu-id="308a0-387">Otherwise, an error is produced and no further steps are taken.</span></span>

<span data-ttu-id="308a0-388">Výše uvedené kroky, pokud bylo úspěšné, jednoznačně vytvoří typ `C`kolekce, typ `E` enumerátoru a typ `T`elementu.</span><span class="sxs-lookup"><span data-stu-id="308a0-388">The above steps, if successful, unambiguously produce a collection type `C`, enumerator type `E` and element type `T`.</span></span> <span data-ttu-id="308a0-389">Příkaz foreach ve formátu</span><span class="sxs-lookup"><span data-stu-id="308a0-389">A foreach statement of the form</span></span>
```csharp
foreach (V v in x) embedded_statement
```
<span data-ttu-id="308a0-390">je pak rozbalen na:</span><span class="sxs-lookup"><span data-stu-id="308a0-390">is then expanded to:</span></span>
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

<span data-ttu-id="308a0-391">Proměnná `e` není viditelná ani přístupná k výrazu `x` nebo vloženému příkazu nebo žádnému jinému zdrojovému kódu tohoto programu.</span><span class="sxs-lookup"><span data-stu-id="308a0-391">The variable `e` is not visible to or accessible to the expression `x` or the embedded statement or any other source code of the program.</span></span> <span data-ttu-id="308a0-392">Proměnná `v` je určena jen pro čtení v příkazu Embedded.</span><span class="sxs-lookup"><span data-stu-id="308a0-392">The variable `v` is read-only in the embedded statement.</span></span> <span data-ttu-id="308a0-393">Pokud není k dispozici explicitní převod ([explicitní převody](conversions.md#explicit-conversions)) z `T` typu (typ elementu) na `V` ( *local_variable_type* v příkazu foreach), je vytvořena chyba a nejsou provedeny žádné další kroky.</span><span class="sxs-lookup"><span data-stu-id="308a0-393">If there is not an explicit conversion ([Explicit conversions](conversions.md#explicit-conversions)) from `T` (the element type) to `V` (the *local_variable_type* in the foreach statement), an error is produced and no further steps are taken.</span></span> <span data-ttu-id="308a0-394">Pokud `x` má hodnotu `null`, `System.NullReferenceException` je vyvolána v době běhu.</span><span class="sxs-lookup"><span data-stu-id="308a0-394">If `x` has the value `null`, a `System.NullReferenceException` is thrown at run-time.</span></span>

<span data-ttu-id="308a0-395">Implementace má povolenou implementaci daného příkazu foreach, například z důvodů výkonu, pokud je chování konzistentní s výše uvedeným rozšířením.</span><span class="sxs-lookup"><span data-stu-id="308a0-395">An implementation is permitted to implement a given foreach-statement differently, e.g. for performance reasons, as long as the behavior is consistent with the above expansion.</span></span>

<span data-ttu-id="308a0-396">Umístění `v` uvnitř smyčky while je důležité pro způsob, jakým je zachycena všemi anonymními funkcemi, ke kterým dochází v *embedded_statement*.</span><span class="sxs-lookup"><span data-stu-id="308a0-396">The placement of `v` inside the while loop is important for how it is captured by any anonymous function occurring in the *embedded_statement*.</span></span>

<span data-ttu-id="308a0-397">Příklad:</span><span class="sxs-lookup"><span data-stu-id="308a0-397">For example:</span></span>
```csharp
int[] values = { 7, 9, 13 };
Action f = null;

foreach (var value in values)
{
    if (f == null) f = () => Console.WriteLine("First value: " + value);
}

f();
```
<span data-ttu-id="308a0-398">Pokud `v` byla deklarována mimo smyčku while, bude sdílena mezi všemi iteracemi a její hodnotou za smyčkou for by byla konečná hodnota, `13`, což je `f` to, co by bylo vyvoláno.</span><span class="sxs-lookup"><span data-stu-id="308a0-398">If `v` was declared outside of the while loop, it would be shared among all iterations, and its value after the for loop would be the final value, `13`, which is what the invocation of `f` would print.</span></span> <span data-ttu-id="308a0-399">Místo toho, protože každá iterace má svou `v`vlastní proměnnou, bude hodnota `f` zachycená v první iteraci nadále uchovávat hodnotu `7`, která bude vytištěna.</span><span class="sxs-lookup"><span data-stu-id="308a0-399">Instead, because each iteration has its own variable `v`, the one captured by `f` in the first iteration will continue to hold the value `7`, which is what will be printed.</span></span> <span data-ttu-id="308a0-400">(Poznámka: starší verze C# deklarované `v` mimo smyčku while.)</span><span class="sxs-lookup"><span data-stu-id="308a0-400">(Note: earlier versions of C# declared `v` outside of the while loop.)</span></span>

<span data-ttu-id="308a0-401">Tělo bloku finally je konstruováno podle následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="308a0-401">The body of the finally block is constructed according to the following steps:</span></span>

*  <span data-ttu-id="308a0-402">Pokud existuje implicitní převod z `E` `System.IDisposable` na rozhraní, pak</span><span class="sxs-lookup"><span data-stu-id="308a0-402">If there is an implicit conversion from `E` to the `System.IDisposable` interface, then</span></span>
   *  <span data-ttu-id="308a0-403">Pokud `E` je typ hodnoty, která není null, klauzule finally je rozšířena na sémantický ekvivalent:</span><span class="sxs-lookup"><span data-stu-id="308a0-403">If `E` is a non-nullable value type then the finally clause is expanded to the semantic equivalent  of:</span></span>

      ```csharp
      finally {
          ((System.IDisposable)e).Dispose();
      }
      ```

   *  <span data-ttu-id="308a0-404">V opačném případě je klauzule finally rozšířena na sémantický ekvivalent:</span><span class="sxs-lookup"><span data-stu-id="308a0-404">Otherwise the finally clause is expanded to the semantic equivalent of:</span></span>

      ```csharp
      finally {
          if (e != null) ((System.IDisposable)e).Dispose();
      }
      ```

   <span data-ttu-id="308a0-405">s výjimkou `E` toho, že pokud je typ hodnoty nebo parametr typu, který je vytvořen jako typ hodnoty, `e` přetypování na `System.IDisposable` nezpůsobí, že by došlo k zabalení.</span><span class="sxs-lookup"><span data-stu-id="308a0-405">except that if `E` is a value type, or a type parameter instantiated to a value type, then the cast of `e` to `System.IDisposable` will not cause boxing to occur.</span></span>

*  <span data-ttu-id="308a0-406">V opačném `E` případě, pokud je zapečetěný typ, je klauzule finally rozbalena na prázdný blok:</span><span class="sxs-lookup"><span data-stu-id="308a0-406">Otherwise, if `E` is a sealed type, the finally clause is expanded to an empty block:</span></span>

   ```csharp
   finally {
   }
   ```

*  <span data-ttu-id="308a0-407">V opačném případě je klauzule finally rozšířena na:</span><span class="sxs-lookup"><span data-stu-id="308a0-407">Otherwise, the finally clause is expanded to:</span></span>

   ```csharp
   finally {
       System.IDisposable d = e as System.IDisposable;
       if (d != null) d.Dispose();
   }
   ```    

   <span data-ttu-id="308a0-408">Místní proměnná `d` není viditelná ani přístupná pro žádný uživatelský kód.</span><span class="sxs-lookup"><span data-stu-id="308a0-408">The local variable `d` is not visible to or accessible to any user code.</span></span> <span data-ttu-id="308a0-409">Konkrétně není v konfliktu s žádnou jinou proměnnou, jejíž obor obsahuje blok finally.</span><span class="sxs-lookup"><span data-stu-id="308a0-409">In particular, it does not conflict with any other variable whose scope includes the finally block.</span></span>

<span data-ttu-id="308a0-410">Pořadí, ve kterém `foreach` procházejí prvky pole, je následující: Pro jednorozměrná prvky pole jsou prochází ve vzestupném pořadí indexu počínaje indexem `0` a končí indexem. `Length - 1`</span><span class="sxs-lookup"><span data-stu-id="308a0-410">The order in which `foreach` traverses the elements of an array, is as follows: For single-dimensional arrays elements are traversed in increasing index order, starting with index `0` and ending with index `Length - 1`.</span></span> <span data-ttu-id="308a0-411">U multidimenzionálních polí jsou elementy procházeny tak, že jsou nejprve zvyšovány indexy pravého rozměru, potom následující levá dimenze a tak dále doleva.</span><span class="sxs-lookup"><span data-stu-id="308a0-411">For multi-dimensional arrays, elements are traversed such that the indices of the rightmost dimension are increased first, then the next left dimension, and so on to the left.</span></span>

<span data-ttu-id="308a0-412">Následující příklad vytiskne každou hodnotu v dvojrozměrném poli v pořadí prvků:</span><span class="sxs-lookup"><span data-stu-id="308a0-412">The following example prints out each value in a two-dimensional array, in element order:</span></span>
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
<span data-ttu-id="308a0-413">Výstup vyprodukovaný je následující:</span><span class="sxs-lookup"><span data-stu-id="308a0-413">The output produced is as follows:</span></span>
```csharp
1.2 2.3 3.4 4.5 5.6 6.7 7.8 8.9
```

<span data-ttu-id="308a0-414">V příkladu</span><span class="sxs-lookup"><span data-stu-id="308a0-414">In the example</span></span>
```csharp
int[] numbers = { 1, 3, 5, 7, 9 };
foreach (var n in numbers) Console.WriteLine(n);
```
<span data-ttu-id="308a0-415">typ `n` je `int` odvozený`numbers`, typ elementu.</span><span class="sxs-lookup"><span data-stu-id="308a0-415">the type of `n` is inferred to be `int`, the element type of `numbers`.</span></span>

## <a name="jump-statements"></a><span data-ttu-id="308a0-416">Jump – příkazy</span><span class="sxs-lookup"><span data-stu-id="308a0-416">Jump statements</span></span>

<span data-ttu-id="308a0-417">Příkazy skoku nepodmíněný přenos řízení.</span><span class="sxs-lookup"><span data-stu-id="308a0-417">Jump statements unconditionally transfer control.</span></span>

```antlr
jump_statement
    : break_statement
    | continue_statement
    | goto_statement
    | return_statement
    | throw_statement
    ;
```

<span data-ttu-id="308a0-418">Umístění, do kterého příkaz skoku přenáší řízení, se nazývá ***cíl*** příkazu skoku.</span><span class="sxs-lookup"><span data-stu-id="308a0-418">The location to which a jump statement transfers control is called the ***target*** of the jump statement.</span></span>

<span data-ttu-id="308a0-419">V případě, že se k příkazu skoku dojde v rámci bloku a cíl tohoto příkazu skoku je mimo tento blok, je příkaz skoku označován pro ***ukončení*** bloku.</span><span class="sxs-lookup"><span data-stu-id="308a0-419">When a jump statement occurs within a block, and the target of that jump statement is outside that block, the jump statement is said to ***exit*** the block.</span></span> <span data-ttu-id="308a0-420">Zatímco příkaz skoku může přenést řízení z bloku, nemůže nikdy přenést ovládací prvek do bloku.</span><span class="sxs-lookup"><span data-stu-id="308a0-420">While a jump statement may transfer control out of a block, it can never transfer control into a block.</span></span>

<span data-ttu-id="308a0-421">Spuštění příkazů skoku je komplikované přítomností `try` v rámci příkazů.</span><span class="sxs-lookup"><span data-stu-id="308a0-421">Execution of jump statements is complicated by the presence of intervening `try` statements.</span></span> <span data-ttu-id="308a0-422">V případě absence takových `try` příkazů příkaz skoku nepodmíněné přenáší řízení z příkazu skok na jeho cíl.</span><span class="sxs-lookup"><span data-stu-id="308a0-422">In the absence of such `try` statements, a jump statement unconditionally transfers control from the jump statement to its target.</span></span> <span data-ttu-id="308a0-423">V přítomnosti těchto `try` vydaných příkazů je provádění složitější.</span><span class="sxs-lookup"><span data-stu-id="308a0-423">In the presence of such intervening `try` statements, execution is more complex.</span></span> <span data-ttu-id="308a0-424">Pokud příkaz skoku `try` ukončí jeden nebo více bloků s přidruženými `finally` bloky, `finally` je počáteční přenos ovládacího prvku do bloku nejvnitřnějšího `try` příkazu.</span><span class="sxs-lookup"><span data-stu-id="308a0-424">If the jump statement exits one or more `try` blocks with associated `finally` blocks, control is initially transferred to the `finally` block of the innermost `try` statement.</span></span> <span data-ttu-id="308a0-425">Když a pokud ovládací prvek dosáhne koncového bodu `finally` bloku, je ovládací prvek převeden `finally` do bloku dalšího ohraničujícího `try` příkazu.</span><span class="sxs-lookup"><span data-stu-id="308a0-425">When and if control reaches the end point of a `finally` block, control is transferred to the `finally` block of the next enclosing `try` statement.</span></span> <span data-ttu-id="308a0-426">Tento proces se opakuje, `finally` dokud nebudou provedeny bloky všech `try` předaných příkazů.</span><span class="sxs-lookup"><span data-stu-id="308a0-426">This process is repeated until the `finally` blocks of all intervening `try` statements have been executed.</span></span>

<span data-ttu-id="308a0-427">V příkladu</span><span class="sxs-lookup"><span data-stu-id="308a0-427">In the example</span></span>
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
<span data-ttu-id="308a0-428">bloky přidružené ke dvěma `try` příkazům jsou spouštěny před přesměrováním ovládacího prvku na cíl příkazu skoku. `finally`</span><span class="sxs-lookup"><span data-stu-id="308a0-428">the `finally` blocks associated with two `try` statements are executed before control is transferred to the target of the jump statement.</span></span>

<span data-ttu-id="308a0-429">Výstup vyprodukovaný je následující:</span><span class="sxs-lookup"><span data-stu-id="308a0-429">The output produced is as follows:</span></span>
```
Before break
Innermost finally block
Outermost finally block
After break
```

### <a name="the-break-statement"></a><span data-ttu-id="308a0-430">Příkaz break</span><span class="sxs-lookup"><span data-stu-id="308a0-430">The break statement</span></span>

<span data-ttu-id="308a0-431">Příkazukončí`switch`nejbližší ohraničující `while` příkaz,`foreach` ,, nebo. `do` `break` `for`</span><span class="sxs-lookup"><span data-stu-id="308a0-431">The `break` statement exits the nearest enclosing `switch`, `while`, `do`, `for`, or `foreach` statement.</span></span>

```antlr
break_statement
    : 'break' ';'
    ;
```

<span data-ttu-id="308a0-432">`break` Cílem příkazu je koncový bod nejbližšího `do` `switch`ohraničujícího `while`příkazu,, `for`, nebo `foreach` .</span><span class="sxs-lookup"><span data-stu-id="308a0-432">The target of a `break` statement is the end point of the nearest enclosing `switch`, `while`, `do`, `for`, or `foreach` statement.</span></span> <span data-ttu-id="308a0-433">`switch` `do` `while`Pokud příkaz není uzavřen v příkazu, ,`for`, nebo`foreach` , dojde k chybě při kompilaci. `break`</span><span class="sxs-lookup"><span data-stu-id="308a0-433">If a `break` statement is not enclosed by a `switch`, `while`, `do`, `for`, or `foreach` statement, a compile-time error occurs.</span></span>

<span data-ttu-id="308a0-434">Je- `switch` `while` `do` `for`li více příkazů,, `foreach` , nebo v sobě vnořeno, příkaz se vztahuje pouze na nejvnitřnější výraz. `break`</span><span class="sxs-lookup"><span data-stu-id="308a0-434">When multiple `switch`, `while`, `do`, `for`, or `foreach` statements are nested within each other, a `break` statement applies only to the innermost statement.</span></span> <span data-ttu-id="308a0-435">Chcete-li přenést řízení napříč více úrovněmi vnoření `goto` , je nutné použít příkaz ([příkaz goto](statements.md#the-goto-statement)).</span><span class="sxs-lookup"><span data-stu-id="308a0-435">To transfer control across multiple nesting levels, a `goto` statement ([The goto statement](statements.md#the-goto-statement)) must be used.</span></span>

<span data-ttu-id="308a0-436">Příkaz nemůže ukončit blok ([příkaz try).](statements.md#the-try-statement) `finally` `break`</span><span class="sxs-lookup"><span data-stu-id="308a0-436">A `break` statement cannot exit a `finally` block ([The try statement](statements.md#the-try-statement)).</span></span> <span data-ttu-id="308a0-437">V případě `finally` `finally` `break` ,žedojdekpříkazuvrámcibloku,musíbýtcílpříkazuvrámcistejnéhobloku;vopačném`break` případě dojde k chybě při kompilaci.</span><span class="sxs-lookup"><span data-stu-id="308a0-437">When a `break` statement occurs within a `finally` block, the target of the `break` statement must be within the same `finally` block; otherwise, a compile-time error occurs.</span></span>

<span data-ttu-id="308a0-438">`break` Příkaz se spustí takto:</span><span class="sxs-lookup"><span data-stu-id="308a0-438">A `break` statement is executed as follows:</span></span>

*  <span data-ttu-id="308a0-439">`finally` `try` `finally` Pokud příkaz ukončí jeden nebo více `try` bloků s přidruženými bloky, je ovládací prvek zpočátku převeden do bloku nejvnitřnějšího příkazu. `break`</span><span class="sxs-lookup"><span data-stu-id="308a0-439">If the `break` statement exits one or more `try` blocks with associated `finally` blocks, control is initially transferred to the `finally` block of the innermost `try` statement.</span></span> <span data-ttu-id="308a0-440">Když a pokud ovládací prvek dosáhne koncového bodu `finally` bloku, je ovládací prvek převeden `finally` do bloku dalšího ohraničujícího `try` příkazu.</span><span class="sxs-lookup"><span data-stu-id="308a0-440">When and if control reaches the end point of a `finally` block, control is transferred to the `finally` block of the next enclosing `try` statement.</span></span> <span data-ttu-id="308a0-441">Tento proces se opakuje, `finally` dokud nebudou provedeny bloky všech `try` předaných příkazů.</span><span class="sxs-lookup"><span data-stu-id="308a0-441">This process is repeated until the `finally` blocks of all intervening `try` statements have been executed.</span></span>
*  <span data-ttu-id="308a0-442">Řízení je převedeno na cíl `break` příkazu.</span><span class="sxs-lookup"><span data-stu-id="308a0-442">Control is transferred to the target of the `break` statement.</span></span>

<span data-ttu-id="308a0-443">Vzhledem k `break` tomu, že příkaz nepodmíněně přenáší řízení jinde, koncový bod `break` příkazu není nikdy dosažitelný.</span><span class="sxs-lookup"><span data-stu-id="308a0-443">Because a `break` statement unconditionally transfers control elsewhere, the end point of a `break` statement is never reachable.</span></span>

### <a name="the-continue-statement"></a><span data-ttu-id="308a0-444">Příkaz Continue</span><span class="sxs-lookup"><span data-stu-id="308a0-444">The continue statement</span></span>

<span data-ttu-id="308a0-445">`do` `while` `foreach` `for`Příkaz spustí novou iteraci nejbližšího ohraničujícího příkazu,, nebo. `continue`</span><span class="sxs-lookup"><span data-stu-id="308a0-445">The `continue` statement starts a new iteration of the nearest enclosing `while`, `do`, `for`, or `foreach` statement.</span></span>

```antlr
continue_statement
    : 'continue' ';'
    ;
```

<span data-ttu-id="308a0-446">Cíl `continue` příkazu je koncový bod vloženého příkazu nejbližšího `while`ohraničujícího `do`příkazu,, `for`nebo `foreach` .</span><span class="sxs-lookup"><span data-stu-id="308a0-446">The target of a `continue` statement is the end point of the embedded statement of the nearest enclosing `while`, `do`, `for`, or `foreach` statement.</span></span> <span data-ttu-id="308a0-447">`while` `do` `for`Pokud příkaz není uzavřen v příkazu,, nebo `foreach` , dojde k chybě při kompilaci. `continue`</span><span class="sxs-lookup"><span data-stu-id="308a0-447">If a `continue` statement is not enclosed by a `while`, `do`, `for`, or `foreach` statement, a compile-time error occurs.</span></span>

<span data-ttu-id="308a0-448">Je- `while` `do` `for` `foreach` li více příkazů,, nebo vnořen mezi sebou, příkaz se vztahuje pouze na nejvnitřnější výraz. `continue`</span><span class="sxs-lookup"><span data-stu-id="308a0-448">When multiple `while`, `do`, `for`, or `foreach` statements are nested within each other, a `continue` statement applies only to the innermost statement.</span></span> <span data-ttu-id="308a0-449">Chcete-li přenést řízení napříč více úrovněmi vnoření `goto` , je nutné použít příkaz ([příkaz goto](statements.md#the-goto-statement)).</span><span class="sxs-lookup"><span data-stu-id="308a0-449">To transfer control across multiple nesting levels, a `goto` statement ([The goto statement](statements.md#the-goto-statement)) must be used.</span></span>

<span data-ttu-id="308a0-450">Příkaz nemůže ukončit blok ([příkaz try).](statements.md#the-try-statement) `finally` `continue`</span><span class="sxs-lookup"><span data-stu-id="308a0-450">A `continue` statement cannot exit a `finally` block ([The try statement](statements.md#the-try-statement)).</span></span> <span data-ttu-id="308a0-451">V případě `finally` `finally` `continue` ,žedojdekpříkazuvrámcibloku,musíbýtcílpříkazuvrámcistejnéhobloku;vopačném`continue` případě dojde k chybě při kompilaci.</span><span class="sxs-lookup"><span data-stu-id="308a0-451">When a `continue` statement occurs within a `finally` block, the target of the `continue` statement must be within the same `finally` block; otherwise a compile-time error occurs.</span></span>

<span data-ttu-id="308a0-452">`continue` Příkaz se spustí takto:</span><span class="sxs-lookup"><span data-stu-id="308a0-452">A `continue` statement is executed as follows:</span></span>

*  <span data-ttu-id="308a0-453">`finally` `try` `finally` Pokud příkaz ukončí jeden nebo více `try` bloků s přidruženými bloky, je ovládací prvek zpočátku převeden do bloku nejvnitřnějšího příkazu. `continue`</span><span class="sxs-lookup"><span data-stu-id="308a0-453">If the `continue` statement exits one or more `try` blocks with associated `finally` blocks, control is initially transferred to the `finally` block of the innermost `try` statement.</span></span> <span data-ttu-id="308a0-454">Když a pokud ovládací prvek dosáhne koncového bodu `finally` bloku, je ovládací prvek převeden `finally` do bloku dalšího ohraničujícího `try` příkazu.</span><span class="sxs-lookup"><span data-stu-id="308a0-454">When and if control reaches the end point of a `finally` block, control is transferred to the `finally` block of the next enclosing `try` statement.</span></span> <span data-ttu-id="308a0-455">Tento proces se opakuje, `finally` dokud nebudou provedeny bloky všech `try` předaných příkazů.</span><span class="sxs-lookup"><span data-stu-id="308a0-455">This process is repeated until the `finally` blocks of all intervening `try` statements have been executed.</span></span>
*  <span data-ttu-id="308a0-456">Řízení je převedeno na cíl `continue` příkazu.</span><span class="sxs-lookup"><span data-stu-id="308a0-456">Control is transferred to the target of the `continue` statement.</span></span>

<span data-ttu-id="308a0-457">Vzhledem k `continue` tomu, že příkaz nepodmíněně přenáší řízení jinde, koncový bod `continue` příkazu není nikdy dosažitelný.</span><span class="sxs-lookup"><span data-stu-id="308a0-457">Because a `continue` statement unconditionally transfers control elsewhere, the end point of a `continue` statement is never reachable.</span></span>

### <a name="the-goto-statement"></a><span data-ttu-id="308a0-458">Příkaz goto</span><span class="sxs-lookup"><span data-stu-id="308a0-458">The goto statement</span></span>

<span data-ttu-id="308a0-459">`goto` Příkaz přenáší řízení na příkaz, který je označen popiskem.</span><span class="sxs-lookup"><span data-stu-id="308a0-459">The `goto` statement transfers control to a statement that is marked by a label.</span></span>

```antlr
goto_statement
    : 'goto' identifier ';'
    | 'goto' 'case' constant_expression ';'
    | 'goto' 'default' ';'
    ;
```

<span data-ttu-id="308a0-460">Cíl `goto` příkazu *identifikátoru* je popisek příkazu se zadaným popiskem.</span><span class="sxs-lookup"><span data-stu-id="308a0-460">The target of a `goto` *identifier* statement is the labeled statement with the given label.</span></span> <span data-ttu-id="308a0-461">Pokud popisek s daným názvem neexistuje v aktuálním členovi funkce nebo pokud `goto` příkaz není v rámci oboru popisku, dojde k chybě při kompilaci.</span><span class="sxs-lookup"><span data-stu-id="308a0-461">If a label with the given name does not exist in the current function member, or if the `goto` statement is not within the scope of the label, a compile-time error occurs.</span></span> <span data-ttu-id="308a0-462">Toto pravidlo povoluje použití `goto` příkazu pro přenos řízení z vnořeného oboru, ale ne do vnořeného oboru.</span><span class="sxs-lookup"><span data-stu-id="308a0-462">This rule permits the use of a `goto` statement to transfer control out of a nested scope, but not into a nested scope.</span></span> <span data-ttu-id="308a0-463">V příkladu</span><span class="sxs-lookup"><span data-stu-id="308a0-463">In the example</span></span>
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
<span data-ttu-id="308a0-464">`goto` příkaz se používá k přenosu řízení z vnořeného oboru.</span><span class="sxs-lookup"><span data-stu-id="308a0-464">a `goto` statement is used to transfer control out of a nested scope.</span></span>

<span data-ttu-id="308a0-465">Cíl `goto case` příkazu je seznam příkazů v `switch` příkazu bezprostředně ohraničujícího příkaz ( `case` [příkaz switch](statements.md#the-switch-statement)), který obsahuje popisek s danou konstantní hodnotou.</span><span class="sxs-lookup"><span data-stu-id="308a0-465">The target of a `goto case` statement is the statement list in the immediately enclosing `switch` statement ([The switch statement](statements.md#the-switch-statement)), which contains a `case` label with the given constant value.</span></span> <span data-ttu-id="308a0-466">`switch` `switch` [](conversions.md#implicit-conversions)Pokud příkaz není uzavřený příkazem, pokud constant_expression není implicitně konvertibilní (implicitní převody) na typ řízení nejbližšího ohraničujícího příkazu, nebo pokud `goto case` nejbližší ohraničující `switch` příkaz `case` neobsahuje popisek s danou konstantní hodnotou, dojde k chybě při kompilaci.</span><span class="sxs-lookup"><span data-stu-id="308a0-466">If the `goto case` statement is not enclosed by a `switch` statement, if the *constant_expression* is not implicitly convertible ([Implicit conversions](conversions.md#implicit-conversions)) to the governing type of the nearest enclosing `switch` statement, or if the nearest enclosing `switch` statement does not contain a `case` label with the given constant value, a compile-time error occurs.</span></span>

<span data-ttu-id="308a0-467">Cíl `goto default` příkazu je seznam příkazů v `switch` příkazu bezprostředně ohraničujícího příkaz ( `default` [příkaz switch](statements.md#the-switch-statement)), který obsahuje popisek.</span><span class="sxs-lookup"><span data-stu-id="308a0-467">The target of a `goto default` statement is the statement list in the immediately enclosing `switch` statement ([The switch statement](statements.md#the-switch-statement)), which contains a `default` label.</span></span> <span data-ttu-id="308a0-468">Pokud příkaz není ohraničen `switch` příkazem, nebo `switch` Pokud nejbližší nadřazený příkaz `default` neobsahuje popisek, dojde k chybě při kompilaci. `goto default`</span><span class="sxs-lookup"><span data-stu-id="308a0-468">If the `goto default` statement is not enclosed by a `switch` statement, or if the nearest enclosing `switch` statement does not contain a `default` label, a compile-time error occurs.</span></span>

<span data-ttu-id="308a0-469">Příkaz nemůže ukončit blok ([příkaz try).](statements.md#the-try-statement) `finally` `goto`</span><span class="sxs-lookup"><span data-stu-id="308a0-469">A `goto` statement cannot exit a `finally` block ([The try statement](statements.md#the-try-statement)).</span></span> <span data-ttu-id="308a0-470">V případě `finally` `finally` `goto` ,žedojdekpříkazuvrámcibloku,cílpříkazumusíbýtvrámcistejnéhoblokunebovopačném`goto` případě dojde k chybě při kompilaci.</span><span class="sxs-lookup"><span data-stu-id="308a0-470">When a `goto` statement occurs within a `finally` block, the target of the `goto` statement must be within the same `finally` block, or otherwise a compile-time error occurs.</span></span>

<span data-ttu-id="308a0-471">`goto` Příkaz se spustí takto:</span><span class="sxs-lookup"><span data-stu-id="308a0-471">A `goto` statement is executed as follows:</span></span>

*  <span data-ttu-id="308a0-472">`finally` `try` `finally` Pokud příkaz ukončí jeden nebo více `try` bloků s přidruženými bloky, je ovládací prvek zpočátku převeden do bloku nejvnitřnějšího příkazu. `goto`</span><span class="sxs-lookup"><span data-stu-id="308a0-472">If the `goto` statement exits one or more `try` blocks with associated `finally` blocks, control is initially transferred to the `finally` block of the innermost `try` statement.</span></span> <span data-ttu-id="308a0-473">Když a pokud ovládací prvek dosáhne koncového bodu `finally` bloku, je ovládací prvek převeden `finally` do bloku dalšího ohraničujícího `try` příkazu.</span><span class="sxs-lookup"><span data-stu-id="308a0-473">When and if control reaches the end point of a `finally` block, control is transferred to the `finally` block of the next enclosing `try` statement.</span></span> <span data-ttu-id="308a0-474">Tento proces se opakuje, `finally` dokud nebudou provedeny bloky všech `try` předaných příkazů.</span><span class="sxs-lookup"><span data-stu-id="308a0-474">This process is repeated until the `finally` blocks of all intervening `try` statements have been executed.</span></span>
*  <span data-ttu-id="308a0-475">Řízení je převedeno na cíl `goto` příkazu.</span><span class="sxs-lookup"><span data-stu-id="308a0-475">Control is transferred to the target of the `goto` statement.</span></span>

<span data-ttu-id="308a0-476">Vzhledem k `goto` tomu, že příkaz nepodmíněně přenáší řízení jinde, koncový bod `goto` příkazu není nikdy dosažitelný.</span><span class="sxs-lookup"><span data-stu-id="308a0-476">Because a `goto` statement unconditionally transfers control elsewhere, the end point of a `goto` statement is never reachable.</span></span>

### <a name="the-return-statement"></a><span data-ttu-id="308a0-477">Příkaz return</span><span class="sxs-lookup"><span data-stu-id="308a0-477">The return statement</span></span>

<span data-ttu-id="308a0-478">Příkaz vrátí řízení aktuálnímu volajícímu funkce, ve `return` které se příkaz zobrazí. `return`</span><span class="sxs-lookup"><span data-stu-id="308a0-478">The `return` statement returns control to the current caller of the function in which the `return` statement appears.</span></span>

```antlr
return_statement
    : 'return' expression? ';'
    ;
```

<span data-ttu-id="308a0-479">`add` `void` `set` [](classes.md#method-body)Příkaz bez výrazu lze použít pouze ve členovi funkce, který nepočítá hodnotu, tedy metodu s typem výsledku (tělo metody), přistupující objekt vlastnosti nebo indexeru, `return` a `remove` přistupující objekty události, konstruktor instance, statický konstruktor nebo destruktor.</span><span class="sxs-lookup"><span data-stu-id="308a0-479">A `return` statement with no expression can be used only in a function member that does not compute a value, that is, a method with the result type ([Method body](classes.md#method-body)) `void`, the `set` accessor of a property or indexer, the `add` and `remove` accessors of an event, an instance constructor, a static constructor, or a destructor.</span></span>

<span data-ttu-id="308a0-480">Příkaz s výrazem lze použít pouze v členovi funkce, který vypočítá hodnotu, což je metoda s neprázdným typem výsledku `get` , přístupový objekt vlastnosti nebo indexeru nebo uživatelem definovaný operátor. `return`</span><span class="sxs-lookup"><span data-stu-id="308a0-480">A `return` statement with an expression can only be used in a function member that computes a value, that is, a method with a non-void result type, the `get` accessor of a property or indexer, or a user-defined operator.</span></span> <span data-ttu-id="308a0-481">Implicitní převod ([implicitní převody](conversions.md#implicit-conversions)) musí existovat z typu výrazu na návratový typ obsahujícího člena funkce.</span><span class="sxs-lookup"><span data-stu-id="308a0-481">An implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)) must exist from the type of the expression to the return type of the containing function member.</span></span>

<span data-ttu-id="308a0-482">Příkazy Return lze také použít v těle anonymních výrazů funkce ([výrazy anonymních funkcí](expressions.md#anonymous-function-expressions)) a účastnit se určení, které převody pro tyto funkce existují.</span><span class="sxs-lookup"><span data-stu-id="308a0-482">Return statements can also be used in the body of anonymous function expressions ([Anonymous function expressions](expressions.md#anonymous-function-expressions)), and participate in determining which conversions exist for those functions.</span></span>

<span data-ttu-id="308a0-483">Jedná se o chybu `return` `finally` v době kompilace pro zobrazení příkazu v bloku ([příkaz try](statements.md#the-try-statement)).</span><span class="sxs-lookup"><span data-stu-id="308a0-483">It is a compile-time error for a `return` statement to appear in a `finally` block ([The try statement](statements.md#the-try-statement)).</span></span>

<span data-ttu-id="308a0-484">`return` Příkaz se spustí takto:</span><span class="sxs-lookup"><span data-stu-id="308a0-484">A `return` statement is executed as follows:</span></span>

*  <span data-ttu-id="308a0-485">`return` Pokud příkaz určuje výraz, je vyhodnocen výraz a výsledná hodnota je převedena na návratový typ obsahující funkce implicitním převodem.</span><span class="sxs-lookup"><span data-stu-id="308a0-485">If the `return` statement specifies an expression, the expression is evaluated and the resulting value is converted to the return type of the containing function by an implicit conversion.</span></span> <span data-ttu-id="308a0-486">Výsledek převodu se stala výslednou hodnotou vytvořenou funkcí.</span><span class="sxs-lookup"><span data-stu-id="308a0-486">The result of the conversion becomes the result value produced by the function.</span></span>
*  <span data-ttu-id="308a0-487">`finally` `catch` `try` `finally` Je-li `try` příkaz uzavřen jedním nebo více nebo bloku s přidruženými bloky, je ovládací prvek zpočátku převeden do bloku nejvnitřnějšího příkazu. `return`</span><span class="sxs-lookup"><span data-stu-id="308a0-487">If the `return` statement is enclosed by one or more `try` or `catch` blocks with associated `finally` blocks, control is initially transferred to the `finally` block of the innermost `try` statement.</span></span> <span data-ttu-id="308a0-488">Když a pokud ovládací prvek dosáhne koncového bodu `finally` bloku, je ovládací prvek převeden `finally` do bloku dalšího ohraničujícího `try` příkazu.</span><span class="sxs-lookup"><span data-stu-id="308a0-488">When and if control reaches the end point of a `finally` block, control is transferred to the `finally` block of the next enclosing `try` statement.</span></span> <span data-ttu-id="308a0-489">Tento proces se opakuje, `finally` dokud nebudou provedeny bloky všech `try` ohraničujících příkazů.</span><span class="sxs-lookup"><span data-stu-id="308a0-489">This process is repeated until the `finally` blocks of all enclosing `try` statements have been executed.</span></span>
*  <span data-ttu-id="308a0-490">Pokud je nadřazená funkce neasynchronní funkcí, ovládací prvek je vrácen volajícímu obsahující funkci spolu s výslednou hodnotou, pokud existuje.</span><span class="sxs-lookup"><span data-stu-id="308a0-490">If the containing function is not an async function, control is returned to the caller of the containing function along with the result value, if any.</span></span>
*  <span data-ttu-id="308a0-491">Pokud je nadřazená funkce asynchronní funkce, vrátí se řízení aktuálnímu volajícímu a výsledná hodnota, pokud existuje, je zaznamenána v úloze vrácení, jak je popsáno v tématu ([rozhraní enumerátorů](classes.md#enumerator-interfaces)).</span><span class="sxs-lookup"><span data-stu-id="308a0-491">If the containing function is an async function, control is returned to the current caller, and the result value, if any, is recorded in the return task as described in ([Enumerator interfaces](classes.md#enumerator-interfaces)).</span></span>

<span data-ttu-id="308a0-492">Vzhledem k `return` tomu, že příkaz nepodmíněně přenáší řízení jinde, koncový bod `return` příkazu není nikdy dosažitelný.</span><span class="sxs-lookup"><span data-stu-id="308a0-492">Because a `return` statement unconditionally transfers control elsewhere, the end point of a `return` statement is never reachable.</span></span>

### <a name="the-throw-statement"></a><span data-ttu-id="308a0-493">Příkaz throw</span><span class="sxs-lookup"><span data-stu-id="308a0-493">The throw statement</span></span>

<span data-ttu-id="308a0-494">`throw` Příkaz vyvolá výjimku.</span><span class="sxs-lookup"><span data-stu-id="308a0-494">The `throw` statement throws an exception.</span></span>

```antlr
throw_statement
    : 'throw' expression? ';'
    ;
```

<span data-ttu-id="308a0-495">`throw` Příkaz s výrazem vyvolá hodnotu vytvořenou vyhodnocením výrazu.</span><span class="sxs-lookup"><span data-stu-id="308a0-495">A `throw` statement with an expression throws the value produced by evaluating the expression.</span></span> <span data-ttu-id="308a0-496">Výraz musí poznamenat hodnotu typu `System.Exception`třídy typu třídy, který je odvozen z `System.Exception` nebo typu parametru typu, který má `System.Exception` (nebo podtřídu) jako platnou základní třídu.</span><span class="sxs-lookup"><span data-stu-id="308a0-496">The expression must denote a value of the class type `System.Exception`, of a class type that derives from `System.Exception` or of a type parameter type that has `System.Exception` (or a subclass thereof) as its effective base class.</span></span> <span data-ttu-id="308a0-497">Pokud vygeneruje `null`vyhodnocení výrazu `System.NullReferenceException` , je vyvolána místo toho.</span><span class="sxs-lookup"><span data-stu-id="308a0-497">If evaluation of the expression produces `null`, a `System.NullReferenceException` is thrown instead.</span></span>

<span data-ttu-id="308a0-498">Příkaz bez výrazu lze použít pouze `catch` v bloku, v takovém případě příkaz znovu vyvolá výjimku, která je `catch` aktuálně zpracovávána tímto blokem. `throw`</span><span class="sxs-lookup"><span data-stu-id="308a0-498">A `throw` statement with no expression can be used only in a `catch` block, in which case that statement re-throws the exception that is currently being handled by that `catch` block.</span></span>

<span data-ttu-id="308a0-499">Vzhledem k `throw` tomu, že příkaz nepodmíněně přenáší řízení jinde, koncový bod `throw` příkazu není nikdy dosažitelný.</span><span class="sxs-lookup"><span data-stu-id="308a0-499">Because a `throw` statement unconditionally transfers control elsewhere, the end point of a `throw` statement is never reachable.</span></span>

<span data-ttu-id="308a0-500">Je-li vyvolána výjimka, je ovládací prvek převeden do první `catch` klauzule v `try` ohraničujícím příkazu, který může zpracovat výjimku.</span><span class="sxs-lookup"><span data-stu-id="308a0-500">When an exception is thrown, control is transferred to the first `catch` clause in an enclosing `try` statement that can handle the exception.</span></span> <span data-ttu-id="308a0-501">Proces, který probíhá z bodu výjimky vyvolané v bodě přenosu řízení na vhodnou obslužnou rutinu výjimky, je označován jako ***šíření výjimky***.</span><span class="sxs-lookup"><span data-stu-id="308a0-501">The process that takes place from the point of the exception being thrown to the point of transferring control to a suitable exception handler is known as ***exception propagation***.</span></span> <span data-ttu-id="308a0-502">Šíření výjimky se skládá z opakovaného vyhodnocení následujících kroků, dokud `catch` není nalezena klauzule, která se shoduje s výjimkou.</span><span class="sxs-lookup"><span data-stu-id="308a0-502">Propagation of an exception consists of repeatedly evaluating the following steps until a `catch` clause that matches the exception is found.</span></span> <span data-ttu-id="308a0-503">V tomto popisu je ***bod throw*** zpočátku umístěním, kde je vyvolána výjimka.</span><span class="sxs-lookup"><span data-stu-id="308a0-503">In this description, the ***throw point*** is initially the location at which the exception is thrown.</span></span>

*  <span data-ttu-id="308a0-504">V aktuálním členovi funkce je zkontrolován `try` každý příkaz, který je ohraničený bodem throw.</span><span class="sxs-lookup"><span data-stu-id="308a0-504">In the current function member, each `try` statement that encloses the throw point is examined.</span></span> <span data-ttu-id="308a0-505">Pro každý příkaz `S`, který začíná nejvnitřnějším `try` příkazem a končí vnějším `try` příkazem, jsou vyhodnoceny následující kroky:</span><span class="sxs-lookup"><span data-stu-id="308a0-505">For each statement `S`, starting with the innermost `try` statement and ending with the outermost `try` statement, the following steps are evaluated:</span></span>

   * <span data-ttu-id="308a0-506">`try` Pokud `catch` `catch` blok uzavíracího bodu uzavřete a pokud má parametr S jednu nebo více klauzulí, jsou klauzule zkontrolovány v pořadí podle vzhledu pro vyhledání vhodné obslužné rutiny pro výjimku podle pravidel uvedených v `S` [V části příkaz try](statements.md#the-try-statement).</span><span class="sxs-lookup"><span data-stu-id="308a0-506">If the `try` block of `S` encloses the throw point and if S has one or more `catch` clauses, the `catch` clauses are examined in order of appearance to locate a suitable handler for the exception, according to the rules specified in Section [The try statement](statements.md#the-try-statement).</span></span> <span data-ttu-id="308a0-507">Pokud je nalezena `catch` odpovídající klauzule, šíření výjimky je dokončeno přenesením ovládacího prvku na blok `catch` této klauzule.</span><span class="sxs-lookup"><span data-stu-id="308a0-507">If a matching `catch` clause is located, the exception propagation is completed by transferring control to the block of that `catch` clause.</span></span>

   * <span data-ttu-id="308a0-508">V opačném případě `try` , pokud blok `catch` nebo blok `S` uzavíracího bodu a `S` má `finally` blok, je ovládací prvek převeden do `finally` bloku.</span><span class="sxs-lookup"><span data-stu-id="308a0-508">Otherwise, if the `try` block or a `catch` block of `S` encloses the throw point and if `S` has a `finally` block, control is transferred to the `finally` block.</span></span> <span data-ttu-id="308a0-509">`finally` Pokud blok vyvolá jinou výjimku, zpracování aktuální výjimky je ukončeno.</span><span class="sxs-lookup"><span data-stu-id="308a0-509">If the `finally` block throws another exception, processing of the current exception is terminated.</span></span> <span data-ttu-id="308a0-510">V opačném případě, pokud ovládací prvek dosáhne koncového bodu `finally` bloku, pokračuje zpracování aktuální výjimky.</span><span class="sxs-lookup"><span data-stu-id="308a0-510">Otherwise, when control reaches the end point of the `finally` block, processing of the current exception is continued.</span></span>

*  <span data-ttu-id="308a0-511">Pokud nebyla obslužná rutina výjimky umístěna v aktuálním volání funkce, volání funkce je ukončeno a nastane jedna z následujících situací:</span><span class="sxs-lookup"><span data-stu-id="308a0-511">If an exception handler was not located in the current function invocation, the function invocation is terminated, and one of the following occurs:</span></span>

   * <span data-ttu-id="308a0-512">Pokud je aktuální funkce neasynchronní, výše uvedené kroky jsou opakovány pro volající funkce s bodem throw, který odpovídá příkazu, ze kterého byl člen funkce vyvolán.</span><span class="sxs-lookup"><span data-stu-id="308a0-512">If the current function is non-async, the steps above are repeated for the caller of the function with a throw point corresponding to the statement from which the function member was invoked.</span></span>

   * <span data-ttu-id="308a0-513">Pokud je aktuální funkce asynchronní a vrácení úlohy zpět, je výjimka zaznamenána do návratové úlohy, která je vložena do chybného nebo zrušeného stavu, jak je popsáno v tématu [rozhraní výčtu](classes.md#enumerator-interfaces).</span><span class="sxs-lookup"><span data-stu-id="308a0-513">If the current function is async and task-returning, the exception is recorded in the return task, which is put into a faulted or cancelled state as described in [Enumerator interfaces](classes.md#enumerator-interfaces).</span></span>

   * <span data-ttu-id="308a0-514">Pokud je aktuální funkce asynchronní a návratovou hodnotou void, je kontext synchronizace aktuálního vlákna upozorněn, jak je popsáno v tématu [výčtová rozhraní](classes.md#enumerable-interfaces).</span><span class="sxs-lookup"><span data-stu-id="308a0-514">If the current function is async and void-returning, the synchronization context of the current thread is notified as described in [Enumerable interfaces](classes.md#enumerable-interfaces).</span></span>

*  <span data-ttu-id="308a0-515">Pokud zpracování výjimek ukončí všechna volání členů funkce v aktuálním vlákně, což značí, že vlákno neobsahuje žádnou obslužnou rutinu pro výjimku, vlákno je ukončeno.</span><span class="sxs-lookup"><span data-stu-id="308a0-515">If the exception processing terminates all function member invocations in the current thread, indicating that the thread has no handler for the exception, then the thread is itself terminated.</span></span> <span data-ttu-id="308a0-516">Dopad takového ukončení je definován implementací.</span><span class="sxs-lookup"><span data-stu-id="308a0-516">The impact of such termination is implementation-defined.</span></span>

## <a name="the-try-statement"></a><span data-ttu-id="308a0-517">Příkaz try</span><span class="sxs-lookup"><span data-stu-id="308a0-517">The try statement</span></span>

<span data-ttu-id="308a0-518">`try` Příkaz poskytuje mechanismus pro zachycení výjimek, ke kterým dochází během provádění bloku.</span><span class="sxs-lookup"><span data-stu-id="308a0-518">The `try` statement provides a mechanism for catching exceptions that occur during execution of a block.</span></span> <span data-ttu-id="308a0-519">Kromě toho `try` příkaz poskytuje možnost zadat blok kódu, který je vždy spuštěn, když ovládací prvek `try` opustí příkaz.</span><span class="sxs-lookup"><span data-stu-id="308a0-519">Furthermore, the `try` statement provides the ability to specify a block of code that is always executed when control leaves the `try` statement.</span></span>

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

<span data-ttu-id="308a0-520">Existují tři možné formy `try` příkazů:</span><span class="sxs-lookup"><span data-stu-id="308a0-520">There are three possible forms of `try` statements:</span></span>

*  <span data-ttu-id="308a0-521">Blok následovaný jedním nebo více `catch` bloky. `try`</span><span class="sxs-lookup"><span data-stu-id="308a0-521">A `try` block followed by one or more `catch` blocks.</span></span>
*  <span data-ttu-id="308a0-522">`try` Blok následovaný`finally` blokem.</span><span class="sxs-lookup"><span data-stu-id="308a0-522">A `try` block followed by a `finally` block.</span></span>
*  <span data-ttu-id="308a0-523">Blok následovaný jedním nebo více `catch` bloky následovanými `finally` blokem. `try`</span><span class="sxs-lookup"><span data-stu-id="308a0-523">A `try` block followed by one or more `catch` blocks followed by a `finally` block.</span></span>

<span data-ttu-id="308a0-524">`System.Exception` `System.Exception` `System.Exception`Když klauzule určuje exception_specifier, musí být typ, typ, který je odvozen z nebo typ parametru typu, který má (nebo podtříd) jako jeho efektivní základní třídu. `catch`</span><span class="sxs-lookup"><span data-stu-id="308a0-524">When a `catch` clause specifies an *exception_specifier*, the type must be `System.Exception`, a type that derives from `System.Exception` or a type parameter type that has `System.Exception` (or a subclass thereof) as its effective base class.</span></span>

<span data-ttu-id="308a0-525">Když klauzule určuje jak exception_specifier s *identifikátorem*, je deklarována ***Proměnná výjimky*** daného názvu a typu. `catch`</span><span class="sxs-lookup"><span data-stu-id="308a0-525">When a `catch` clause specifies both an *exception_specifier* with an *identifier*, an ***exception variable*** of the given name and type is declared.</span></span> <span data-ttu-id="308a0-526">Proměnná výjimky odpovídá místní proměnné s rozsahem, který překračuje `catch` klauzuli.</span><span class="sxs-lookup"><span data-stu-id="308a0-526">The exception variable corresponds to a local variable with a scope that extends over the `catch` clause.</span></span> <span data-ttu-id="308a0-527">Během provádění *exception_filter* a *bloku*představuje proměnná výjimky Aktuálně zpracovávanou výjimku.</span><span class="sxs-lookup"><span data-stu-id="308a0-527">During execution of the *exception_filter* and *block*, the exception variable represents the exception currently being handled.</span></span> <span data-ttu-id="308a0-528">Pro účely jednoznačné kontroly přiřazení je proměnná výjimky považována za jednoznačně přiřazenou v celém oboru.</span><span class="sxs-lookup"><span data-stu-id="308a0-528">For purposes of definite assignment checking, the exception variable is considered definitely assigned in its entire scope.</span></span>

<span data-ttu-id="308a0-529">Pokud klauzule nezahrnuje název proměnné výjimky, není nemožné získat přístup k objektu výjimky ve filtru a `catch` bloku. `catch`</span><span class="sxs-lookup"><span data-stu-id="308a0-529">Unless a `catch` clause includes an exception variable name, it is impossible to access the exception object in the filter and `catch` block.</span></span>

<span data-ttu-id="308a0-530">Klauzule, která neurčuje *exception_specifier* , se nazývá obecná `catch` klauzule. `catch`</span><span class="sxs-lookup"><span data-stu-id="308a0-530">A `catch` clause that does not specify an *exception_specifier* is called a general `catch` clause.</span></span>

<span data-ttu-id="308a0-531">Některé programovací jazyky mohou podporovat výjimky, které nejsou reprezentovány jako objekt odvozený z `System.Exception`, i když takové výjimky by nikdy neměly být C# generovány kódem.</span><span class="sxs-lookup"><span data-stu-id="308a0-531">Some programming languages may support exceptions that are not representable as an object derived from `System.Exception`, although such exceptions could never be generated by C# code.</span></span> <span data-ttu-id="308a0-532">K zachycení `catch` takových výjimek se dá použít obecná klauzule.</span><span class="sxs-lookup"><span data-stu-id="308a0-532">A general `catch` clause may be used to catch such exceptions.</span></span> <span data-ttu-id="308a0-533">Proto obecná `catch` klauzule je sémanticky odlišná od jednoho, který určuje typ `System.Exception`, v tom, že předchozí může také zachytit výjimky z jiných jazyků.</span><span class="sxs-lookup"><span data-stu-id="308a0-533">Thus, a general `catch` clause is semantically different from one that specifies the type `System.Exception`, in that the former may also catch exceptions from other languages.</span></span>

<span data-ttu-id="308a0-534">Aby bylo možné najít obslužnou rutinu pro výjimku, `catch` jsou v lexikálním pořadí přezkoumány klauzule.</span><span class="sxs-lookup"><span data-stu-id="308a0-534">In order to locate a handler for an exception, `catch` clauses are examined in lexical order.</span></span> <span data-ttu-id="308a0-535">Pokud klauzule určuje typ, ale filtr výjimek, jedná se o chybu při kompilaci pro pozdější `catch` klauzuli v rámci stejného `try` příkazu k určení typu, který je stejný jako nebo je odvozen z typu, který je typu. `catch`</span><span class="sxs-lookup"><span data-stu-id="308a0-535">If a `catch` clause specifies a type but no exception filter, it is a compile-time error for a later `catch` clause in the same `try` statement to specify a type that is the same as, or is derived from, that type.</span></span> <span data-ttu-id="308a0-536">Pokud klauzule neurčuje žádný typ bez filtru, musí se jednat o poslední `catch` klauzuli pro tento `try` příkaz. `catch`</span><span class="sxs-lookup"><span data-stu-id="308a0-536">If a `catch` clause specifies no type and no filter, it must be the last `catch` clause for that `try` statement.</span></span>

<span data-ttu-id="308a0-537">V rámci `throw` `catch` bloku lze příkaz ([příkaz throw](statements.md#the-throw-statement)) bez výrazu použít k opětovnému vyvolání výjimky, která byla zachycena blokem. `catch`</span><span class="sxs-lookup"><span data-stu-id="308a0-537">Within a `catch` block, a `throw` statement ([The throw statement](statements.md#the-throw-statement)) with no expression can be used to re-throw the exception that was caught by the `catch` block.</span></span> <span data-ttu-id="308a0-538">Přiřazení proměnné výjimky nemění výjimku, která je znovu vyvolána.</span><span class="sxs-lookup"><span data-stu-id="308a0-538">Assignments to an exception variable do not alter the exception that is re-thrown.</span></span>

<span data-ttu-id="308a0-539">V příkladu</span><span class="sxs-lookup"><span data-stu-id="308a0-539">In the example</span></span>
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
<span data-ttu-id="308a0-540">Metoda `F` zachytí výjimku, zapisuje do konzoly nějaké diagnostické informace, změní proměnnou výjimky a znovu vyvolá výjimku.</span><span class="sxs-lookup"><span data-stu-id="308a0-540">the method `F` catches an exception, writes some diagnostic information to the console, alters the exception variable, and re-throws the exception.</span></span> <span data-ttu-id="308a0-541">Výjimka, která je znovu vyvolána, je původní výjimka, takže výstup vytvářený je:</span><span class="sxs-lookup"><span data-stu-id="308a0-541">The exception that is re-thrown is the original exception, so the output produced is:</span></span>
```
Exception in F: G
Exception in Main: G
```

<span data-ttu-id="308a0-542">Pokud první blok catch vyvolal `e` místo opětovného vyvolání aktuální výjimky, vyprodukovaný výstup by byl následující:</span><span class="sxs-lookup"><span data-stu-id="308a0-542">If the first catch block had thrown `e` instead of rethrowing the current exception, the output produced would be as follows:</span></span>
```
Exception in F: G
Exception in Main: F
```

<span data-ttu-id="308a0-543">Jedná se o `break`chybu při kompilaci příkazu, `continue`nebo `goto` pro přenos řízení `finally` z bloku.</span><span class="sxs-lookup"><span data-stu-id="308a0-543">It is a compile-time error for a `break`, `continue`, or `goto` statement to transfer control out of a `finally` block.</span></span> <span data-ttu-id="308a0-544">Když v `break` `finally` bloku `continue`dojde k `goto` příkazu, nebo, musí být cíl příkazu v rámci stejného `finally` bloku nebo jinak dojde k chybě při kompilaci.</span><span class="sxs-lookup"><span data-stu-id="308a0-544">When a `break`, `continue`, or `goto` statement occurs in a `finally` block, the target of the statement must be within the same `finally` block, or otherwise a compile-time error occurs.</span></span>

<span data-ttu-id="308a0-545">Jedná se o chybu při kompilaci, ke které `return` může příkaz `finally` v bloku dojít.</span><span class="sxs-lookup"><span data-stu-id="308a0-545">It is a compile-time error for a `return` statement to occur in a `finally` block.</span></span>

<span data-ttu-id="308a0-546">`try` Příkaz se spustí takto:</span><span class="sxs-lookup"><span data-stu-id="308a0-546">A `try` statement is executed as follows:</span></span>

*  <span data-ttu-id="308a0-547">Ovládací prvek se přenese `try` do bloku.</span><span class="sxs-lookup"><span data-stu-id="308a0-547">Control is transferred to the `try` block.</span></span>
*  <span data-ttu-id="308a0-548">Když a když ovládací prvek dosáhne koncového bodu `try` bloku:</span><span class="sxs-lookup"><span data-stu-id="308a0-548">When and if control reaches the end point of the `try` block:</span></span>
   *  <span data-ttu-id="308a0-549">Pokud příkaz má blok, je spuštěn `finally` blok. `finally` `try`</span><span class="sxs-lookup"><span data-stu-id="308a0-549">If the `try` statement has a `finally` block, the `finally` block is executed.</span></span>
   *  <span data-ttu-id="308a0-550">Ovládací prvek se přenese do koncového bodu `try` příkazu.</span><span class="sxs-lookup"><span data-stu-id="308a0-550">Control is transferred to the end point of the `try` statement.</span></span>

*  <span data-ttu-id="308a0-551">Pokud je výjimka rozšířena na `try` příkaz během provádění `try` bloku:</span><span class="sxs-lookup"><span data-stu-id="308a0-551">If an exception is propagated to the `try` statement during execution of the `try` block:</span></span>
   *  <span data-ttu-id="308a0-552">`catch` Klauzule, pokud existují, jsou zkontrolovány v pořadí podle vzhledu pro vyhledání vhodné obslužné rutiny pro výjimku.</span><span class="sxs-lookup"><span data-stu-id="308a0-552">The `catch` clauses, if any, are examined in order of appearance to locate a suitable handler for the exception.</span></span> <span data-ttu-id="308a0-553">`catch` Pokud klauzule neurčuje typ, nebo typ výjimky nebo základní typ typu výjimky:</span><span class="sxs-lookup"><span data-stu-id="308a0-553">If a `catch` clause does not specify a type, or specifies the exception type or a base type of the exception type:</span></span>
      *  <span data-ttu-id="308a0-554">`catch` Pokud klauzule deklaruje proměnnou výjimky, je objekt výjimky přiřazen proměnné výjimky.</span><span class="sxs-lookup"><span data-stu-id="308a0-554">If the `catch` clause declares an exception variable, the exception object is assigned to the exception variable.</span></span>
      *  <span data-ttu-id="308a0-555">`catch` Pokud klauzule deklaruje filtr výjimek, je vyhodnocen filtr.</span><span class="sxs-lookup"><span data-stu-id="308a0-555">If the `catch` clause declares an exception filter, the filter is evaluated.</span></span> <span data-ttu-id="308a0-556">Pokud se vyhodnotí `false`jako, klauzule catch není shodná a hledání pokračuje prostřednictvím jakékoli další `catch` klauzule pro vhodnou obslužnou rutinu.</span><span class="sxs-lookup"><span data-stu-id="308a0-556">If it evaluates to `false`, the catch clause is not a match, and the search continues through any subsequent `catch` clauses for a suitable handler.</span></span>
      *  <span data-ttu-id="308a0-557">V opačném případě je `catch` klauzulepovažovánazashoduaovládacíprveksepřenesedoodpovídajícíhobloku.`catch`</span><span class="sxs-lookup"><span data-stu-id="308a0-557">Otherwise, the `catch` clause is considered a match, and control is transferred to the matching `catch` block.</span></span>
      *  <span data-ttu-id="308a0-558">Když a když ovládací prvek dosáhne koncového bodu `catch` bloku:</span><span class="sxs-lookup"><span data-stu-id="308a0-558">When and if control reaches the end point of the `catch` block:</span></span>
         * <span data-ttu-id="308a0-559">Pokud příkaz má blok, je spuštěn `finally` blok. `finally` `try`</span><span class="sxs-lookup"><span data-stu-id="308a0-559">If the `try` statement has a `finally` block, the `finally` block is executed.</span></span>
         * <span data-ttu-id="308a0-560">Ovládací prvek se přenese do koncového bodu `try` příkazu.</span><span class="sxs-lookup"><span data-stu-id="308a0-560">Control is transferred to the end point of the `try` statement.</span></span>
      *  <span data-ttu-id="308a0-561">Pokud je výjimka rozšířena na `try` příkaz během provádění `catch` bloku:</span><span class="sxs-lookup"><span data-stu-id="308a0-561">If an exception is propagated to the `try` statement during execution of the `catch` block:</span></span>
         *  <span data-ttu-id="308a0-562">Pokud příkaz má blok, je spuštěn `finally` blok. `finally` `try`</span><span class="sxs-lookup"><span data-stu-id="308a0-562">If the `try` statement has a `finally` block, the `finally` block is executed.</span></span>
         *  <span data-ttu-id="308a0-563">Výjimka je šířena do dalšího ohraničujícího `try` příkazu.</span><span class="sxs-lookup"><span data-stu-id="308a0-563">The exception is propagated to the next enclosing `try` statement.</span></span>
   *  <span data-ttu-id="308a0-564">Pokud příkaz nemá žádné `catch` klauzule nebo pokud žádná `catch` klauzule neodpovídá výjimce: `try`</span><span class="sxs-lookup"><span data-stu-id="308a0-564">If the `try` statement has no `catch` clauses or if no `catch` clause matches the exception:</span></span>
      *  <span data-ttu-id="308a0-565">Pokud příkaz má blok, je spuštěn `finally` blok. `finally` `try`</span><span class="sxs-lookup"><span data-stu-id="308a0-565">If the `try` statement has a `finally` block, the `finally` block is executed.</span></span>
      *  <span data-ttu-id="308a0-566">Výjimka je šířena do dalšího ohraničujícího `try` příkazu.</span><span class="sxs-lookup"><span data-stu-id="308a0-566">The exception is propagated to the next enclosing `try` statement.</span></span>

<span data-ttu-id="308a0-567">Příkazy `finally` bloku jsou vždy spouštěny, pokud ovládací prvek `try` opustí příkaz.</span><span class="sxs-lookup"><span data-stu-id="308a0-567">The statements of a `finally` block are always executed when control leaves a `try` statement.</span></span> <span data-ttu-id="308a0-568">To platí, zda je přenos ovládacího prvku proveden jako výsledek normálního spuštění, v `break`důsledku provedení příkazu, `continue`, `goto`nebo `return` nebo v důsledku rozšiřování výjimky z `try` vydá.</span><span class="sxs-lookup"><span data-stu-id="308a0-568">This is true whether the control transfer occurs as a result of normal execution, as a result of executing a `break`, `continue`, `goto`, or `return` statement, or as a result of propagating an exception out of the `try` statement.</span></span>

<span data-ttu-id="308a0-569">Pokud je vyvolána výjimka během provádění `finally` bloku a není zachycena v rámci stejného bloku finally, je výjimka rozšířena do dalšího `try` ohraničujícího příkazu.</span><span class="sxs-lookup"><span data-stu-id="308a0-569">If an exception is thrown during execution of a `finally` block, and is not caught within the same finally block, the exception is propagated to the next enclosing `try` statement.</span></span> <span data-ttu-id="308a0-570">Pokud byla v procesu rozšiřování další výjimka, dojde ke ztrátě této výjimky.</span><span class="sxs-lookup"><span data-stu-id="308a0-570">If another exception was in the process of being propagated, that exception is lost.</span></span> <span data-ttu-id="308a0-571">Proces šíření výjimky je popsán dále v popisu `throw` příkazu ([příkaz throw](statements.md#the-throw-statement)).</span><span class="sxs-lookup"><span data-stu-id="308a0-571">The process of propagating an exception is discussed further in the description of the `throw` statement ([The throw statement](statements.md#the-throw-statement)).</span></span>

<span data-ttu-id="308a0-572">Blok příkazu je dosažitelný, pokud je příkazdosažitelný.`try` `try` `try`</span><span class="sxs-lookup"><span data-stu-id="308a0-572">The `try` block of a `try` statement is reachable if the `try` statement is reachable.</span></span>

<span data-ttu-id="308a0-573">Blok příkazu je dosažitelný, pokud je příkazdosažitelný.`try` `try` `catch`</span><span class="sxs-lookup"><span data-stu-id="308a0-573">A `catch` block of a `try` statement is reachable if the `try` statement is reachable.</span></span>

<span data-ttu-id="308a0-574">Blok příkazu je dosažitelný, pokud je příkazdosažitelný.`try` `try` `finally`</span><span class="sxs-lookup"><span data-stu-id="308a0-574">The `finally` block of a `try` statement is reachable if the `try` statement is reachable.</span></span>

<span data-ttu-id="308a0-575">Koncový bod `try` příkazu je dosažitelný, pokud jsou splněny obě následující podmínky:</span><span class="sxs-lookup"><span data-stu-id="308a0-575">The end point of a `try` statement is reachable if both of the following are true:</span></span>

*  <span data-ttu-id="308a0-576">Koncový bod `try` bloku je dosažitelný nebo je dostupný koncový bod alespoň jednoho `catch` bloku.</span><span class="sxs-lookup"><span data-stu-id="308a0-576">The end point of the `try` block is reachable or the end point of at least one `catch` block is reachable.</span></span>
*  <span data-ttu-id="308a0-577">Pokud je `finally` k dispozici blok, koncový bod `finally` bloku je dosažitelný.</span><span class="sxs-lookup"><span data-stu-id="308a0-577">If a `finally` block is present, the end point of the `finally` block is reachable.</span></span>

## <a name="the-checked-and-unchecked-statements"></a><span data-ttu-id="308a0-578">Kontrolované a nezaškrtnuté příkazy</span><span class="sxs-lookup"><span data-stu-id="308a0-578">The checked and unchecked statements</span></span>

<span data-ttu-id="308a0-579">Příkazy `checked` a`unchecked` slouží k řízení ***kontextu kontroly přetečení*** pro aritmetické operace a převody integrálního typu.</span><span class="sxs-lookup"><span data-stu-id="308a0-579">The `checked` and `unchecked` statements are used to control the ***overflow checking context*** for integral-type arithmetic operations and conversions.</span></span>

```antlr
checked_statement
    : 'checked' block
    ;

unchecked_statement
    : 'unchecked' block
    ;
```

<span data-ttu-id="308a0-580">Příkaz způsobí vyhodnocení všech výrazů v *bloku* v kontrolovaném kontextu a příkaz způsobí, že `unchecked` všechny výrazy v bloku budou vyhodnocovány v nekontrolovaném kontextu. `checked`</span><span class="sxs-lookup"><span data-stu-id="308a0-580">The `checked` statement causes all expressions in the *block* to be evaluated in a checked context, and the `unchecked` statement causes all expressions in the *block* to be evaluated in an unchecked context.</span></span>

<span data-ttu-id="308a0-581">`unchecked` Příkazy `checked` a `unchecked` jsoupřesnéekvivalentemoperátorůa(zkontrolovanýchanekontrolovanýchoperátorů),stímrozdílem`checked` , že pracují na blocích namísto výrazů.[](expressions.md#the-checked-and-unchecked-operators)</span><span class="sxs-lookup"><span data-stu-id="308a0-581">The `checked` and `unchecked` statements are precisely equivalent to the `checked` and `unchecked` operators ([The checked and unchecked operators](expressions.md#the-checked-and-unchecked-operators)), except that they operate on blocks instead of expressions.</span></span>

## <a name="the-lock-statement"></a><span data-ttu-id="308a0-582">Příkaz Lock</span><span class="sxs-lookup"><span data-stu-id="308a0-582">The lock statement</span></span>

<span data-ttu-id="308a0-583">`lock` Příkaz získá zámek vzájemného vyloučení pro daný objekt, provede příkaz a pak uvolní zámek.</span><span class="sxs-lookup"><span data-stu-id="308a0-583">The `lock` statement obtains the mutual-exclusion lock for a given object, executes a statement, and then releases the lock.</span></span>

```antlr
lock_statement
    : 'lock' '(' expression ')' embedded_statement
    ;
```

<span data-ttu-id="308a0-584">Výraz `lock` příkazu musí znamenat hodnotu typu, který je známý jako *reference_type*.</span><span class="sxs-lookup"><span data-stu-id="308a0-584">The expression of a `lock` statement must denote a value of a type known to be a *reference_type*.</span></span> <span data-ttu-id="308a0-585">Pro výraz `lock` příkazu není nikdy proveden žádný implicitní převod na zabalení ([převody zabalení](conversions.md#boxing-conversions)), a proto se jedná o chybu při kompilaci, která označuje hodnotu *value_type*.</span><span class="sxs-lookup"><span data-stu-id="308a0-585">No implicit boxing conversion ([Boxing conversions](conversions.md#boxing-conversions)) is ever performed for the expression of a `lock` statement, and thus it is a compile-time error for the expression to denote a value of a *value_type*.</span></span>

<span data-ttu-id="308a0-586">`lock` Příkaz formuláře</span><span class="sxs-lookup"><span data-stu-id="308a0-586">A `lock` statement of the form</span></span>
```csharp
lock (x) ...
```
<span data-ttu-id="308a0-587">kde `x` je výrazem *reference_type*, je přesně ekvivalentní</span><span class="sxs-lookup"><span data-stu-id="308a0-587">where `x` is an expression of a *reference_type*, is precisely equivalent to</span></span>
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
<span data-ttu-id="308a0-588">s výjimkou, že `x` je vyhodnocena pouze jednou.</span><span class="sxs-lookup"><span data-stu-id="308a0-588">except that `x` is only evaluated once.</span></span>

<span data-ttu-id="308a0-589">I když je držen zámek vzájemného vyloučení, může kód spuštěný ve stejném vláknu spuštění také získat a uvolnit zámek.</span><span class="sxs-lookup"><span data-stu-id="308a0-589">While a mutual-exclusion lock is held, code executing in the same execution thread can also obtain and release the lock.</span></span> <span data-ttu-id="308a0-590">Nicméně kód, který je spuštěn v jiných vláknech, je blokován pro získání zámku, dokud se zámek neuvolní.</span><span class="sxs-lookup"><span data-stu-id="308a0-590">However, code executing in other threads is blocked from obtaining the lock until the lock is released.</span></span>

<span data-ttu-id="308a0-591">Uzamčení `System.Type` objektů za účelem synchronizace přístupu ke statickým datům se nedoporučuje.</span><span class="sxs-lookup"><span data-stu-id="308a0-591">Locking `System.Type` objects in order to synchronize access to static data is not recommended.</span></span> <span data-ttu-id="308a0-592">Jiný kód může být uzamčen na stejném typu, což může vést k zablokování.</span><span class="sxs-lookup"><span data-stu-id="308a0-592">Other code might lock on the same type, which can result in deadlock.</span></span> <span data-ttu-id="308a0-593">Lepším řešením je synchronizovat přístup ke statickým datům uzamknutím privátního statického objektu.</span><span class="sxs-lookup"><span data-stu-id="308a0-593">A better approach is to synchronize access to static data by locking a private static object.</span></span> <span data-ttu-id="308a0-594">Příklad:</span><span class="sxs-lookup"><span data-stu-id="308a0-594">For example:</span></span>
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

## <a name="the-using-statement"></a><span data-ttu-id="308a0-595">Pomocí příkazu using</span><span class="sxs-lookup"><span data-stu-id="308a0-595">The using statement</span></span>

<span data-ttu-id="308a0-596">`using` Příkaz získá jeden nebo více prostředků, provede příkaz a pak odstraní prostředek.</span><span class="sxs-lookup"><span data-stu-id="308a0-596">The `using` statement obtains one or more resources, executes a statement, and then disposes of the resource.</span></span>

```antlr
using_statement
    : 'using' '(' resource_acquisition ')' embedded_statement
    ;

resource_acquisition
    : local_variable_declaration
    | expression
    ;
```

<span data-ttu-id="308a0-597">***Prostředek*** je třída nebo struktura, která implementuje `System.IDisposable`rozhraní, které zahrnuje jednu metodu bez parametrů s názvem. `Dispose`</span><span class="sxs-lookup"><span data-stu-id="308a0-597">A ***resource*** is a class or struct that implements `System.IDisposable`, which includes a single parameterless method named `Dispose`.</span></span> <span data-ttu-id="308a0-598">Kód, který používá prostředek, může zavolat `Dispose` , aby označoval, že prostředek již není potřeba.</span><span class="sxs-lookup"><span data-stu-id="308a0-598">Code that is using a resource can call `Dispose` to indicate that the resource is no longer needed.</span></span> <span data-ttu-id="308a0-599">Pokud `Dispose` není voláno, pak automatické vyřazení nakonec probíhá jako důsledek uvolňování paměti.</span><span class="sxs-lookup"><span data-stu-id="308a0-599">If `Dispose` is not called, then automatic disposal eventually occurs as a consequence of garbage collection.</span></span>

<span data-ttu-id="308a0-600">Pokud je forma *resource_acquisition* *local_variable_declaration* , pak typ *local_variable_declaration* musí být buď `dynamic` nebo typ, který lze implicitně převést na. `System.IDisposable`</span><span class="sxs-lookup"><span data-stu-id="308a0-600">If the form of *resource_acquisition* is *local_variable_declaration* then the type of the *local_variable_declaration* must be either `dynamic` or a type that can be implicitly converted to `System.IDisposable`.</span></span> <span data-ttu-id="308a0-601">Pokud je tvar výrazu *resource_acquisition* , pak tento výraz musí být implicitně převoditelné `System.IDisposable`na.</span><span class="sxs-lookup"><span data-stu-id="308a0-601">If the form of *resource_acquisition* is *expression* then this expression must be implicitly convertible to `System.IDisposable`.</span></span>

<span data-ttu-id="308a0-602">Místní proměnné deklarované v *resource_acquisition* jsou jen pro čtení a musí zahrnovat inicializátor.</span><span class="sxs-lookup"><span data-stu-id="308a0-602">Local variables declared in a *resource_acquisition* are read-only, and must include an initializer.</span></span> <span data-ttu-id="308a0-603">K chybě při kompilaci dojde v případě, že se vložený příkaz pokusí změnit tyto místní proměnné (prostřednictvím přiřazení nebo `++` operátorů a `--` ), přebírat jejich adresu nebo předat `ref` parametry nebo `out` .</span><span class="sxs-lookup"><span data-stu-id="308a0-603">A compile-time error occurs if the embedded statement attempts to modify these local variables (via assignment or the `++` and `--` operators) , take the address of them, or pass them as `ref` or `out` parameters.</span></span>

<span data-ttu-id="308a0-604">`using` Příkaz je přeložen na tři části: získání, využití a vyřazení.</span><span class="sxs-lookup"><span data-stu-id="308a0-604">A `using` statement is translated into three parts: acquisition, usage, and disposal.</span></span> <span data-ttu-id="308a0-605">Využití prostředku je implicitně uzavřeno v `try` příkazu, který `finally` obsahuje klauzuli.</span><span class="sxs-lookup"><span data-stu-id="308a0-605">Usage of the resource is implicitly enclosed in a `try` statement that includes a `finally` clause.</span></span> <span data-ttu-id="308a0-606">Tato `finally` klauzule odstraní prostředek.</span><span class="sxs-lookup"><span data-stu-id="308a0-606">This `finally` clause disposes of the resource.</span></span> <span data-ttu-id="308a0-607">Pokud je získán `Dispose` prostředek,neníprovedenožádnévoláníanení`null` vyvolána žádná výjimka.</span><span class="sxs-lookup"><span data-stu-id="308a0-607">If a `null` resource is acquired, then no call to `Dispose` is made, and no exception is thrown.</span></span> <span data-ttu-id="308a0-608">Pokud je prostředek typu `dynamic` , je dynamicky převeden prostřednictvím implicitního dynamického převodu ([implicitní dynamické převody](conversions.md#implicit-dynamic-conversions)) na `IDisposable` během akvizice, aby bylo zajištěno, že převod proběhne úspěšně před využitím a odvod.</span><span class="sxs-lookup"><span data-stu-id="308a0-608">If the resource is of type `dynamic` it is dynamically converted through an implicit dynamic conversion ([Implicit dynamic conversions](conversions.md#implicit-dynamic-conversions)) to `IDisposable` during acquisition in order to ensure that the conversion is successful before the usage and disposal.</span></span>

<span data-ttu-id="308a0-609">`using` Příkaz formuláře</span><span class="sxs-lookup"><span data-stu-id="308a0-609">A `using` statement of the form</span></span>
```csharp
using (ResourceType resource = expression) statement
```
<span data-ttu-id="308a0-610">odpovídá jedné ze tří možných rozšíření.</span><span class="sxs-lookup"><span data-stu-id="308a0-610">corresponds to one of three possible expansions.</span></span> <span data-ttu-id="308a0-611">Pokud `ResourceType` je typ hodnoty, která není null, je rozšíření</span><span class="sxs-lookup"><span data-stu-id="308a0-611">When `ResourceType` is a non-nullable value type, the expansion is</span></span>
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

<span data-ttu-id="308a0-612">V opačném `ResourceType` případě, kdy je typ hodnoty s možnou hodnotou null `dynamic`nebo typ odkazu jiný než je rozšíření</span><span class="sxs-lookup"><span data-stu-id="308a0-612">Otherwise, when `ResourceType` is a nullable value type or a reference type other than `dynamic`, the expansion is</span></span>
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

<span data-ttu-id="308a0-613">`ResourceType` V `dynamic`opačném případě je rozšíření</span><span class="sxs-lookup"><span data-stu-id="308a0-613">Otherwise, when `ResourceType` is `dynamic`, the expansion is</span></span>
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

<span data-ttu-id="308a0-614">V obou rozšířeních `resource` je proměnná v příkazu Embedded jen pro čtení `d` a proměnná je nepřístupná v a neviditelná pro vložený příkaz.</span><span class="sxs-lookup"><span data-stu-id="308a0-614">In either expansion, the `resource` variable is read-only in the embedded statement, and the `d` variable is inaccessible in, and invisible to, the embedded statement.</span></span>

<span data-ttu-id="308a0-615">Implementace má povolenou implementaci daného příkazu Using, například z důvodů výkonu, pokud je chování konzistentní s výše uvedeným rozšířením.</span><span class="sxs-lookup"><span data-stu-id="308a0-615">An implementation is permitted to implement a given using-statement differently, e.g. for performance reasons, as long as the behavior is consistent with the above expansion.</span></span>

<span data-ttu-id="308a0-616">`using` Příkaz formuláře</span><span class="sxs-lookup"><span data-stu-id="308a0-616">A `using` statement of the form</span></span>
```csharp
using (expression) statement
```
<span data-ttu-id="308a0-617">má stejné tři možné rozšíření.</span><span class="sxs-lookup"><span data-stu-id="308a0-617">has the same three possible expansions.</span></span> <span data-ttu-id="308a0-618">V tomto případě `ResourceType` je implicitně typ doby kompilace `expression`, pokud má,, pokud má nějaký typ.</span><span class="sxs-lookup"><span data-stu-id="308a0-618">In this case `ResourceType` is implicitly the compile-time type of the `expression`, if it has one.</span></span> <span data-ttu-id="308a0-619">V opačném `IDisposable` případě se samotné rozhraní používá `ResourceType`jako.</span><span class="sxs-lookup"><span data-stu-id="308a0-619">Otherwise the interface `IDisposable` itself is used as the `ResourceType`.</span></span> <span data-ttu-id="308a0-620">Tato `resource` proměnná je nepřístupná v a v rámci vloženého příkazu není viditelná.</span><span class="sxs-lookup"><span data-stu-id="308a0-620">The `resource` variable is inaccessible in, and invisible to, the embedded statement.</span></span>

<span data-ttu-id="308a0-621">Pokud má *resource_acquisition* formu *local_variable_declaration*, je možné získat více prostředků daného typu.</span><span class="sxs-lookup"><span data-stu-id="308a0-621">When a *resource_acquisition* takes the form of a *local_variable_declaration*, it is possible to acquire multiple resources of a given type.</span></span> <span data-ttu-id="308a0-622">`using` Příkaz formuláře</span><span class="sxs-lookup"><span data-stu-id="308a0-622">A `using` statement of the form</span></span>
```csharp
using (ResourceType r1 = e1, r2 = e2, ..., rN = eN) statement
```
<span data-ttu-id="308a0-623">je přesně ekvivalentní sekvenci vnořených `using` příkazů:</span><span class="sxs-lookup"><span data-stu-id="308a0-623">is precisely equivalent to a sequence of nested `using` statements:</span></span>
```csharp
using (ResourceType r1 = e1)
    using (ResourceType r2 = e2)
        ...
            using (ResourceType rN = eN)
                statement
```

<span data-ttu-id="308a0-624">Následující příklad vytvoří soubor s názvem `log.txt` a zapíše dva řádky textu do souboru.</span><span class="sxs-lookup"><span data-stu-id="308a0-624">The example below creates a file named `log.txt` and writes two lines of text to the file.</span></span> <span data-ttu-id="308a0-625">Příklad následně otevře stejný soubor pro čtení a zkopíruje obsažené řádky textu do konzoly.</span><span class="sxs-lookup"><span data-stu-id="308a0-625">The example then opens that same file for reading and copies the contained lines of text to the console.</span></span>
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

<span data-ttu-id="308a0-626">Vzhledem k tomu `TextReader` , že třídy `IDisposable` `using` a implementují rozhraní, může příklad použít příkazy k zajištění toho, aby byl podkladový soubor správně uzavřen po operacích zápisu nebo čtení. `TextWriter`</span><span class="sxs-lookup"><span data-stu-id="308a0-626">Since the `TextWriter` and `TextReader` classes implement the `IDisposable` interface, the example can use `using` statements to ensure that the underlying file is properly closed following the write or read operations.</span></span>

## <a name="the-yield-statement"></a><span data-ttu-id="308a0-627">Příkaz yield</span><span class="sxs-lookup"><span data-stu-id="308a0-627">The yield statement</span></span>

<span data-ttu-id="308a0-628">Příkaz se používá v bloku iterátoru ([bloky](statements.md#blocks)) k získání hodnoty objektu Enumerator ([objekty enumerátoru](classes.md#enumerator-objects)) nebo vyčíslitelného objektu ([vyčíslitelné objekty](classes.md#enumerable-objects)) iterátoru nebo k signalizaci konce iterace. `yield`</span><span class="sxs-lookup"><span data-stu-id="308a0-628">The `yield` statement is used in an iterator block ([Blocks](statements.md#blocks)) to yield a value to the enumerator object ([Enumerator objects](classes.md#enumerator-objects)) or enumerable object ([Enumerable objects](classes.md#enumerable-objects)) of an iterator or to signal the end of the iteration.</span></span>

```antlr
yield_statement
    : 'yield' 'return' expression ';'
    | 'yield' 'break' ';'
    ;
```

<span data-ttu-id="308a0-629">`yield`není rezervované slovo; má zvláštní význam pouze v případě, že je použit `return` bezprostředně `break` před klíčovým slovem or.</span><span class="sxs-lookup"><span data-stu-id="308a0-629">`yield` is not a reserved word; it has special meaning only when used immediately before a `return` or `break` keyword.</span></span> <span data-ttu-id="308a0-630">V jiných kontextech `yield` lze použít jako identifikátor.</span><span class="sxs-lookup"><span data-stu-id="308a0-630">In other contexts, `yield` can be used as an identifier.</span></span>

<span data-ttu-id="308a0-631">Existuje několik omezení, kde `yield` se může zobrazit příkaz, jak je popsáno v následujícím tématu.</span><span class="sxs-lookup"><span data-stu-id="308a0-631">There are several restrictions on where a `yield` statement can appear, as described in the following.</span></span>

*  <span data-ttu-id="308a0-632">Jedná se o chybu `yield` při kompilaci pro příkaz (z obou formulářů), který se zobrazí mimo *method_body*, *operator_body* nebo *accessor_body* .</span><span class="sxs-lookup"><span data-stu-id="308a0-632">It is a compile-time error for a `yield` statement (of either form) to appear outside a *method_body*, *operator_body* or *accessor_body*</span></span>
*  <span data-ttu-id="308a0-633">Jedná se o chybu při kompilaci pro `yield` příkaz (z obou formulářů), který se má objevit uvnitř anonymní funkce.</span><span class="sxs-lookup"><span data-stu-id="308a0-633">It is a compile-time error for a `yield` statement (of either form) to appear inside an anonymous function.</span></span>
*  <span data-ttu-id="308a0-634">Jedná se o chybu při kompilaci pro `yield` příkaz (z obou formulářů), který se má objevit `finally` v klauzuli `try` příkazu.</span><span class="sxs-lookup"><span data-stu-id="308a0-634">It is a compile-time error for a `yield` statement (of either form) to appear in the `finally` clause of a `try` statement.</span></span>
*  <span data-ttu-id="308a0-635">Jedná se o chybu `yield return` při kompilaci, aby se příkaz objevil kdekoli `try` v příkazu, který obsahuje jakékoli `catch` klauzule.</span><span class="sxs-lookup"><span data-stu-id="308a0-635">It is a compile-time error for a `yield return` statement to appear anywhere in a `try` statement that contains any `catch` clauses.</span></span>

<span data-ttu-id="308a0-636">Následující příklad ukazuje několik platných a neplatných použití `yield` příkazů.</span><span class="sxs-lookup"><span data-stu-id="308a0-636">The following example shows some valid and invalid uses of `yield` statements.</span></span>

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

<span data-ttu-id="308a0-637">Implicitní převod ([implicitní převody](conversions.md#implicit-conversions)) musí existovat z typu výrazu v `yield return` příkazu na typ Yield ([typ výtěžnosti](classes.md#yield-type)) iterátoru.</span><span class="sxs-lookup"><span data-stu-id="308a0-637">An implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)) must exist from the type of the expression in the `yield return` statement to the yield type ([Yield type](classes.md#yield-type)) of the iterator.</span></span>

<span data-ttu-id="308a0-638">`yield return` Příkaz se spustí takto:</span><span class="sxs-lookup"><span data-stu-id="308a0-638">A `yield return` statement is executed as follows:</span></span>

*  <span data-ttu-id="308a0-639">Výraz uvedený v příkazu je vyhodnocen, implicitně převeden na typ yield a přiřazený `Current` vlastnosti objektu enumerátor.</span><span class="sxs-lookup"><span data-stu-id="308a0-639">The expression given in the statement is evaluated, implicitly converted to the yield type, and assigned to the `Current` property of the enumerator object.</span></span>
*  <span data-ttu-id="308a0-640">Provádění bloku iterátoru je pozastaveno.</span><span class="sxs-lookup"><span data-stu-id="308a0-640">Execution of the iterator block is suspended.</span></span> <span data-ttu-id="308a0-641">Pokud je `try` `finally` příkaz v jednom nebo více blocích, přidružené bloky nejsou v tuto chvíli spuštěny. `yield return`</span><span class="sxs-lookup"><span data-stu-id="308a0-641">If the `yield return` statement is within one or more `try` blocks, the associated `finally` blocks are not executed at this time.</span></span>
*  <span data-ttu-id="308a0-642">Metoda objektu Enumerator se vrátí `true` svému volajícímu, což značí, že objekt enumerátoru se úspěšně pokročil na další položku. `MoveNext`</span><span class="sxs-lookup"><span data-stu-id="308a0-642">The `MoveNext` method of the enumerator object returns `true` to its caller, indicating that the enumerator object successfully advanced to the next item.</span></span>

<span data-ttu-id="308a0-643">Další volání `MoveNext` metody objektu Enumerator pokračuje v provádění bloku iterátoru, ze kterého byl naposled pozastaven.</span><span class="sxs-lookup"><span data-stu-id="308a0-643">The next call to the enumerator object's `MoveNext` method resumes execution of the iterator block from where it was last suspended.</span></span>

<span data-ttu-id="308a0-644">`yield break` Příkaz se spustí takto:</span><span class="sxs-lookup"><span data-stu-id="308a0-644">A `yield break` statement is executed as follows:</span></span>

*  <span data-ttu-id="308a0-645">`try` `finally` `finally` `try` Pokud je příkazuzavřenjednímnebovíceblokyspřidruženýmibloky,jeovládacíprvekzpočátkupřevedendoblokunejvnitřnějšíhopříkazu.`yield break`</span><span class="sxs-lookup"><span data-stu-id="308a0-645">If the `yield break` statement is enclosed by one or more `try` blocks with associated `finally` blocks, control is initially transferred to the `finally` block of the innermost `try` statement.</span></span> <span data-ttu-id="308a0-646">Když a pokud ovládací prvek dosáhne koncového bodu `finally` bloku, je ovládací prvek převeden `finally` do bloku dalšího ohraničujícího `try` příkazu.</span><span class="sxs-lookup"><span data-stu-id="308a0-646">When and if control reaches the end point of a `finally` block, control is transferred to the `finally` block of the next enclosing `try` statement.</span></span> <span data-ttu-id="308a0-647">Tento proces se opakuje, `finally` dokud nebudou provedeny bloky všech `try` ohraničujících příkazů.</span><span class="sxs-lookup"><span data-stu-id="308a0-647">This process is repeated until the `finally` blocks of all enclosing `try` statements have been executed.</span></span>
*  <span data-ttu-id="308a0-648">Ovládací prvek se vrátí volajícímu bloku iterátoru.</span><span class="sxs-lookup"><span data-stu-id="308a0-648">Control is returned to the caller of the iterator block.</span></span> <span data-ttu-id="308a0-649">Toto je buď `MoveNext` metoda, nebo `Dispose` metoda objektu Enumerator.</span><span class="sxs-lookup"><span data-stu-id="308a0-649">This is either the `MoveNext` method or `Dispose` method of the enumerator object.</span></span>

<span data-ttu-id="308a0-650">Vzhledem k `yield break` tomu, že příkaz nepodmíněně přenáší řízení jinde, koncový bod `yield break` příkazu není nikdy dosažitelný.</span><span class="sxs-lookup"><span data-stu-id="308a0-650">Because a `yield break` statement unconditionally transfers control elsewhere, the end point of a `yield break` statement is never reachable.</span></span>
