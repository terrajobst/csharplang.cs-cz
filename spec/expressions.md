---
ms.openlocfilehash: 130898a8b5a7b8eb986b314cb4cf78038e840b02
ms.sourcegitcommit: 09e0ddec3bb6aa99b7340158bbac86a5a8243b43
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/24/2019
ms.locfileid: "66193932"
---
# <a name="expressions"></a><span data-ttu-id="dec56-101">Výrazy</span><span class="sxs-lookup"><span data-stu-id="dec56-101">Expressions</span></span>

<span data-ttu-id="dec56-102">Výraz je posloupnost operátorů a operandů.</span><span class="sxs-lookup"><span data-stu-id="dec56-102">An expression is a sequence of operators and operands.</span></span> <span data-ttu-id="dec56-103">Tato kapitola definuje syntaxe pořadí vyhodnocení operandů a operátory a výrazy význam.</span><span class="sxs-lookup"><span data-stu-id="dec56-103">This chapter defines the syntax, order of evaluation of operands and operators, and meaning of expressions.</span></span>

## <a name="expression-classifications"></a><span data-ttu-id="dec56-104">Klasifikacích výraz</span><span class="sxs-lookup"><span data-stu-id="dec56-104">Expression classifications</span></span>

<span data-ttu-id="dec56-105">Výraz je klasifikován tak jeden z následujících akcí:</span><span class="sxs-lookup"><span data-stu-id="dec56-105">An expression is classified as one of the following:</span></span>

*  <span data-ttu-id="dec56-106">Hodnota.</span><span class="sxs-lookup"><span data-stu-id="dec56-106">A value.</span></span> <span data-ttu-id="dec56-107">Každá hodnota má přidruženého typu.</span><span class="sxs-lookup"><span data-stu-id="dec56-107">Every value has an associated type.</span></span>
*  <span data-ttu-id="dec56-108">Proměnná.</span><span class="sxs-lookup"><span data-stu-id="dec56-108">A variable.</span></span> <span data-ttu-id="dec56-109">Každá proměnná je přidružen typ, jmenovitě deklarovaný typ proměnné.</span><span class="sxs-lookup"><span data-stu-id="dec56-109">Every variable has an associated type, namely the declared type of the variable.</span></span>
*  <span data-ttu-id="dec56-110">Obor názvů.</span><span class="sxs-lookup"><span data-stu-id="dec56-110">A namespace.</span></span> <span data-ttu-id="dec56-111">Výraz s této klasifikace může být použit pouze jako levé straně *member_access* ([přístup ke členu](expressions.md#member-access)).</span><span class="sxs-lookup"><span data-stu-id="dec56-111">An expression with this classification can only appear as the left hand side of a *member_access* ([Member access](expressions.md#member-access)).</span></span> <span data-ttu-id="dec56-112">V libovolném kontextu výrazu jsou klasifikovány jako obor názvů způsobí chybu kompilace.</span><span class="sxs-lookup"><span data-stu-id="dec56-112">In any other context, an expression classified as a namespace causes a compile-time error.</span></span>
*  <span data-ttu-id="dec56-113">Typ.</span><span class="sxs-lookup"><span data-stu-id="dec56-113">A type.</span></span> <span data-ttu-id="dec56-114">Výraz s této klasifikace může být použit pouze jako levé straně *member_access* ([přístup ke členu](expressions.md#member-access)), nebo jako operand pro `as` – operátor ([operátor as ](expressions.md#the-as-operator)), `is` – operátor ([je operátor](expressions.md#the-is-operator)), nebo `typeof` – operátor ([operátor typeof](expressions.md#the-typeof-operator)).</span><span class="sxs-lookup"><span data-stu-id="dec56-114">An expression with this classification can only appear as the left hand side of a *member_access* ([Member access](expressions.md#member-access)), or as an operand for the `as` operator ([The as operator](expressions.md#the-as-operator)), the `is` operator ([The is operator](expressions.md#the-is-operator)), or the `typeof` operator ([The typeof operator](expressions.md#the-typeof-operator)).</span></span> <span data-ttu-id="dec56-115">V libovolném kontextu jsou klasifikovány jako typ výrazu způsobí chybu kompilace.</span><span class="sxs-lookup"><span data-stu-id="dec56-115">In any other context, an expression classified as a type causes a compile-time error.</span></span>
*  <span data-ttu-id="dec56-116">Metoda skupinu, která je sada přetížené metody, výsledkem vyhledávání člena ([člen vyhledávání](expressions.md#member-lookup)).</span><span class="sxs-lookup"><span data-stu-id="dec56-116">A method group, which is a set of overloaded methods resulting from a member lookup ([Member lookup](expressions.md#member-lookup)).</span></span> <span data-ttu-id="dec56-117">Skupinu metod. může mít přidruženou instanci výraz a seznam argumentů přidruženého typu.</span><span class="sxs-lookup"><span data-stu-id="dec56-117">A method group may have an associated instance expression and an associated type argument list.</span></span> <span data-ttu-id="dec56-118">Při vyvolání metody instance, bude výsledkem vyhodnocení výrazu instance instance reprezentována `this` ([tento přístup](expressions.md#this-access)).</span><span class="sxs-lookup"><span data-stu-id="dec56-118">When an instance method is invoked, the result of evaluating the instance expression becomes the instance represented by `this` ([This access](expressions.md#this-access)).</span></span> <span data-ttu-id="dec56-119">Skupinu metod. je povolen v *invocation_expression* ([výrazy typu Invocation](expressions.md#invocation-expressions)), *delegate_creation_expression* ([delegovat vytváření výrazy](expressions.md#delegate-creation-expressions)) a jako levé straně je operátor a může být implicitně převeden na typ delegáta kompatibilní ([Metoda skupiny převody](conversions.md#method-group-conversions)).</span><span class="sxs-lookup"><span data-stu-id="dec56-119">A method group is permitted in an *invocation_expression* ([Invocation expressions](expressions.md#invocation-expressions)) , a *delegate_creation_expression* ([Delegate creation expressions](expressions.md#delegate-creation-expressions)) and as the left hand side of an is operator, and can be implicitly converted to a compatible delegate type ([Method group conversions](conversions.md#method-group-conversions)).</span></span> <span data-ttu-id="dec56-120">V libovolném kontextu výrazu jsou klasifikovány jako skupinu metod způsobí chybu kompilace.</span><span class="sxs-lookup"><span data-stu-id="dec56-120">In any other context, an expression classified as a method group causes a compile-time error.</span></span>
*  <span data-ttu-id="dec56-121">Literál null.</span><span class="sxs-lookup"><span data-stu-id="dec56-121">A null literal.</span></span> <span data-ttu-id="dec56-122">Výraz s této klasifikace lze implicitně převést na typ odkazu nebo typ připouštějící hodnotu Null.</span><span class="sxs-lookup"><span data-stu-id="dec56-122">An expression with this classification can be implicitly converted to a reference type or nullable type.</span></span>
*  <span data-ttu-id="dec56-123">Anonymní funkce.</span><span class="sxs-lookup"><span data-stu-id="dec56-123">An anonymous function.</span></span> <span data-ttu-id="dec56-124">Výraz s této klasifikace lze implicitně převést na typ kompatibilní delegáta nebo typu stromu výrazu.</span><span class="sxs-lookup"><span data-stu-id="dec56-124">An expression with this classification can be implicitly converted to a compatible delegate type or expression tree type.</span></span>
*  <span data-ttu-id="dec56-125">Přístup k vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="dec56-125">A property access.</span></span> <span data-ttu-id="dec56-126">Každý přístup k vlastnosti je přidružen typ, jmenovitě typ vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="dec56-126">Every property access has an associated type, namely the type of the property.</span></span> <span data-ttu-id="dec56-127">Přístup k vlastnosti kromě toho může mít přidruženou instanci výrazu.</span><span class="sxs-lookup"><span data-stu-id="dec56-127">Furthermore, a property access may have an associated instance expression.</span></span> <span data-ttu-id="dec56-128">Když přístupový objekt ( `get` nebo `set` bloku) instance je vyvolán přístup k vlastnostem, výsledek vyhodnocení výrazu instance se změní instance reprezentována `this` ([tento přístup](expressions.md#this-access)).</span><span class="sxs-lookup"><span data-stu-id="dec56-128">When an accessor (the `get` or `set` block) of an instance property access is invoked, the result of evaluating the instance expression becomes the instance represented by `this` ([This access](expressions.md#this-access)).</span></span>
*  <span data-ttu-id="dec56-129">Přístup k události.</span><span class="sxs-lookup"><span data-stu-id="dec56-129">An event access.</span></span> <span data-ttu-id="dec56-130">Každý přístup k události je přidružen typ, jmenovitě typ události.</span><span class="sxs-lookup"><span data-stu-id="dec56-130">Every event access has an associated type, namely the type of the event.</span></span> <span data-ttu-id="dec56-131">Kromě toho události přístupu může mít přidruženou instanci výrazu.</span><span class="sxs-lookup"><span data-stu-id="dec56-131">Furthermore, an event access may have an associated instance expression.</span></span> <span data-ttu-id="dec56-132">O událost přístupu se může zobrazit jako operand levé straně `+=` a `-=` operátory ([události přiřazení](expressions.md#event-assignment)).</span><span class="sxs-lookup"><span data-stu-id="dec56-132">An event access may appear as the left hand operand of the `+=` and `-=` operators ([Event assignment](expressions.md#event-assignment)).</span></span> <span data-ttu-id="dec56-133">V libovolném kontextu výrazu jsou klasifikovány jako událost přístupu způsobí chybu kompilace.</span><span class="sxs-lookup"><span data-stu-id="dec56-133">In any other context, an expression classified as an event access causes a compile-time error.</span></span>
*  <span data-ttu-id="dec56-134">Přístup indexeru.</span><span class="sxs-lookup"><span data-stu-id="dec56-134">An indexer access.</span></span> <span data-ttu-id="dec56-135">Každý přístup indexeru je přidružen typ, a to typ elementu indexeru.</span><span class="sxs-lookup"><span data-stu-id="dec56-135">Every indexer access has an associated type, namely the element type of the indexer.</span></span> <span data-ttu-id="dec56-136">Kromě toho přístup indexeru má přidruženou instanci výraz a seznam argumentů přidružený.</span><span class="sxs-lookup"><span data-stu-id="dec56-136">Furthermore, an indexer access has an associated instance expression and an associated argument list.</span></span> <span data-ttu-id="dec56-137">Když přístupový objekt ( `get` nebo `set` bloku) z indexeru přístup, je vyvolána, výsledek vyhodnocení výrazu instance se změní instance reprezentována `this` ([tento přístup](expressions.md#this-access)) a výsledek tohoto objektu hodnocení seznamu argumentů se změní na seznamu parametrů vyvolání.</span><span class="sxs-lookup"><span data-stu-id="dec56-137">When an accessor (the `get` or `set` block) of an indexer access is invoked, the result of evaluating the instance expression becomes the instance represented by `this` ([This access](expressions.md#this-access)), and the result of evaluating the argument list becomes the parameter list of the invocation.</span></span>
*  <span data-ttu-id="dec56-138">Nothing.</span><span class="sxs-lookup"><span data-stu-id="dec56-138">Nothing.</span></span> <span data-ttu-id="dec56-139">Proběhne, když je výraz vyvolání metody s návratovým typem `void`.</span><span class="sxs-lookup"><span data-stu-id="dec56-139">This occurs when the expression is an invocation of a method with a return type of `void`.</span></span> <span data-ttu-id="dec56-140">Výraz klasifikován jako nic je platná pouze v kontextu *statement_expression* ([příkazy výrazů](statements.md#expression-statements)).</span><span class="sxs-lookup"><span data-stu-id="dec56-140">An expression classified as nothing is only valid in the context of a *statement_expression* ([Expression statements](statements.md#expression-statements)).</span></span>

<span data-ttu-id="dec56-141">Konečný výsledek výrazu se nikdy obor názvů, typ, metodu skupiny nebo přístup k události.</span><span class="sxs-lookup"><span data-stu-id="dec56-141">The final result of an expression is never a namespace, type, method group, or event access.</span></span> <span data-ttu-id="dec56-142">Místo toho jak bylo uvedeno výše, tyto výrazy jsou zprostředkující konstrukce, které jsou povolené jenom v určitých kontextech.</span><span class="sxs-lookup"><span data-stu-id="dec56-142">Rather, as noted above, these categories of expressions are intermediate constructs that are only permitted in certain contexts.</span></span>

<span data-ttu-id="dec56-143">Přístup k vlastnosti nebo přístup indexeru je vždy překlasifikován sám jako hodnotu pomocí provádí vyvolání *načtení přístupového objektu* nebo *nastavení přístupového objektu*.</span><span class="sxs-lookup"><span data-stu-id="dec56-143">A property access or indexer access is always reclassified as a value by performing an invocation of the *get accessor* or the *set accessor*.</span></span> <span data-ttu-id="dec56-144">Zejména přístupový objekt je určen podle kontextu vlastnost nebo indexovací člen přístupu: Pokud je cílem přiřazení, přístup *nastavení přístupového objektu* se vyvolá, aby přiřadí novou hodnotu ([jednoduché přiřazení](expressions.md#simple-assignment)).</span><span class="sxs-lookup"><span data-stu-id="dec56-144">The particular accessor is determined by the context of the property or indexer access: If the access is the target of an assignment, the *set accessor* is invoked to assign a new value ([Simple assignment](expressions.md#simple-assignment)).</span></span> <span data-ttu-id="dec56-145">V opačném případě *načtení přístupového objektu* se vyvolá k získání aktuální hodnoty ([hodnot výrazů](expressions.md#values-of-expressions)).</span><span class="sxs-lookup"><span data-stu-id="dec56-145">Otherwise, the *get accessor* is invoked to obtain the current value ([Values of expressions](expressions.md#values-of-expressions)).</span></span>

### <a name="values-of-expressions"></a><span data-ttu-id="dec56-146">Hodnoty výrazů</span><span class="sxs-lookup"><span data-stu-id="dec56-146">Values of expressions</span></span>

<span data-ttu-id="dec56-147">Většina konstrukce, které se týkají výraz takže v konečném důsledku vyžaduje výraz, který má označení ***hodnota***.</span><span class="sxs-lookup"><span data-stu-id="dec56-147">Most of the constructs that involve an expression ultimately require the expression to denote a ***value***.</span></span> <span data-ttu-id="dec56-148">V takových případech Pokud skutečný výraz označuje obor názvů, typ, skupinu metod nebo nic, dojde k chybě kompilace.</span><span class="sxs-lookup"><span data-stu-id="dec56-148">In such cases, if the actual expression denotes a namespace, a type, a method group, or nothing, a compile-time error occurs.</span></span> <span data-ttu-id="dec56-149">Ale pokud výraz označuje přístup k vlastnosti, indexeru přístupu nebo proměnná, hodnota pro vlastnost, indexer nebo proměnná je implicitně nahrazena:</span><span class="sxs-lookup"><span data-stu-id="dec56-149">However, if the expression denotes a property access, an indexer access, or a variable, the value of the property, indexer, or variable is implicitly substituted:</span></span>

*  <span data-ttu-id="dec56-150">Hodnota proměnné je jednoduše hodnota aktuálně uložené v umístění úložiště, který je identifikován proměnné.</span><span class="sxs-lookup"><span data-stu-id="dec56-150">The value of a variable is simply the value currently stored in the storage location identified by the variable.</span></span> <span data-ttu-id="dec56-151">Proměnná je třeba zvážit, jsou přiřazené ([jednoznačného přiřazení](variables.md#definite-assignment)) před jeho hodnotu lze získat nebo jinak dojde k chybě kompilace.</span><span class="sxs-lookup"><span data-stu-id="dec56-151">A variable must be considered definitely assigned ([Definite assignment](variables.md#definite-assignment)) before its value can be obtained, or otherwise a compile-time error occurs.</span></span>
*  <span data-ttu-id="dec56-152">Získá hodnota výraz přístupu k vlastnosti s vyvoláním *načtení přístupového objektu* vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="dec56-152">The value of a property access expression is obtained by invoking the *get accessor* of the property.</span></span> <span data-ttu-id="dec56-153">Pokud vlastnost nemá žádný *načtení přístupového objektu*, dojde k chybě kompilace.</span><span class="sxs-lookup"><span data-stu-id="dec56-153">If the property has no *get accessor*, a compile-time error occurs.</span></span> <span data-ttu-id="dec56-154">V opačném případě vyvolání členské funkce ([kompilace kontrolu dynamické přetížení](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) se provádí, a výsledek volání se stane hodnotou výrazu přístup k vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="dec56-154">Otherwise, a function member invocation ([Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) is performed, and the result of the invocation becomes the value of the property access expression.</span></span>
*  <span data-ttu-id="dec56-155">Hodnota výrazu přístup indexeru je získala při vyvolání *načtení přístupového objektu* indexeru.</span><span class="sxs-lookup"><span data-stu-id="dec56-155">The value of an indexer access expression is obtained by invoking the *get accessor* of the indexer.</span></span> <span data-ttu-id="dec56-156">Pokud nemá žádné indexeru *načtení přístupového objektu*, dojde k chybě kompilace.</span><span class="sxs-lookup"><span data-stu-id="dec56-156">If the indexer has no *get accessor*, a compile-time error occurs.</span></span> <span data-ttu-id="dec56-157">V opačném případě vyvolání členské funkce ([kompilace kontrolu dynamické přetížení](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) se provádí v argumentu seznamu přidružená k výrazu přístup indexeru a výsledek volání se stane hodnota výrazu přístup indexeru.</span><span class="sxs-lookup"><span data-stu-id="dec56-157">Otherwise, a function member invocation ([Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) is performed with the argument list associated with the indexer access expression, and the result of the invocation becomes the value of the indexer access expression.</span></span>

## <a name="static-and-dynamic-binding"></a><span data-ttu-id="dec56-158">Statické a dynamické vazby</span><span class="sxs-lookup"><span data-stu-id="dec56-158">Static and Dynamic Binding</span></span>

<span data-ttu-id="dec56-159">Procesu, který určuje význam operace na základě typu nebo hodnota základních výrazů (argumenty, operandy, příjemci) se často označuje jako ***vazby***.</span><span class="sxs-lookup"><span data-stu-id="dec56-159">The process of determining the meaning of an operation based on the type or value of constituent expressions (arguments, operands, receivers) is often referred to as ***binding***.</span></span> <span data-ttu-id="dec56-160">Pro instanci význam volání metody závisí na typu příjemce a argumenty.</span><span class="sxs-lookup"><span data-stu-id="dec56-160">For instance the meaning of a method call is determined based on the type of the receiver and arguments.</span></span> <span data-ttu-id="dec56-161">Význam operátoru je určen na základě typu operandů.</span><span class="sxs-lookup"><span data-stu-id="dec56-161">The meaning of an operator is determined based on the type of its operands.</span></span>

<span data-ttu-id="dec56-162">V jazyce C# významu operace je obvykle určena v době kompilace, založená na kompilaci typu z jeho základních výrazů.</span><span class="sxs-lookup"><span data-stu-id="dec56-162">In C# the meaning of an operation is usually determined at compile-time, based on the compile-time type of its constituent expressions.</span></span> <span data-ttu-id="dec56-163">Podobně pokud výraz obsahuje chybu, chyba rozpoznáno a Ohlášeno kompilátorem.</span><span class="sxs-lookup"><span data-stu-id="dec56-163">Likewise, if an expression contains an error, the error is detected and reported by the compiler.</span></span> <span data-ttu-id="dec56-164">Tento postup se označuje jako ***statické vazby***.</span><span class="sxs-lookup"><span data-stu-id="dec56-164">This approach is known as ***static binding***.</span></span>

<span data-ttu-id="dec56-165">Ale pokud je výraz dynamického výrazu (tj. nemá typ `dynamic`) to znamená, že všechny vazby, který je součástí by měla vycházet z jeho typ za běhu (tj. skutečný typ objektu je označuje za běhu) místo typu má v za kompilace.</span><span class="sxs-lookup"><span data-stu-id="dec56-165">However, if an expression is a dynamic expression (i.e. has the type `dynamic`) this indicates that any binding that it participates in should be based on its run-time type (i.e. the actual type of the object it denotes at run-time) rather than the type it has at compile-time.</span></span> <span data-ttu-id="dec56-166">Vazba tato operace je proto odložena až do doby, kde je operace má být proveden za běhu programu.</span><span class="sxs-lookup"><span data-stu-id="dec56-166">The binding of such an operation is therefore deferred until the time where the operation is to be executed during the running of the program.</span></span> <span data-ttu-id="dec56-167">To se označuje jako ***dynamické vazby***.</span><span class="sxs-lookup"><span data-stu-id="dec56-167">This is referred to as ***dynamic binding***.</span></span>

<span data-ttu-id="dec56-168">Když se operace dynamicky vazba, žádné nebo téměř žádné kontrola se neprovádí kompilátorem.</span><span class="sxs-lookup"><span data-stu-id="dec56-168">When an operation is dynamically bound, little or no checking is performed by the compiler.</span></span> <span data-ttu-id="dec56-169">Místo toho pokud vazbu za běhu selže, jsou hlášeny chyby jako výjimky za běhu.</span><span class="sxs-lookup"><span data-stu-id="dec56-169">Instead if the run-time binding fails, errors are reported as exceptions at run-time.</span></span>

<span data-ttu-id="dec56-170">Následující operace v jazyce C# jsou v souladu s vazby:</span><span class="sxs-lookup"><span data-stu-id="dec56-170">The following operations in C# are subject to binding:</span></span>

*  <span data-ttu-id="dec56-171">Přístup ke členu: `e.M`</span><span class="sxs-lookup"><span data-stu-id="dec56-171">Member access: `e.M`</span></span>
*  <span data-ttu-id="dec56-172">Volání metody: `e.M(e1, ..., eN)`</span><span class="sxs-lookup"><span data-stu-id="dec56-172">Method invocation: `e.M(e1, ..., eN)`</span></span>
*  <span data-ttu-id="dec56-173">Volání delegáta:`e(e1, ..., eN)`</span><span class="sxs-lookup"><span data-stu-id="dec56-173">Delegate invocation:`e(e1, ..., eN)`</span></span>
*  <span data-ttu-id="dec56-174">Přístup k prvkům: `e[e1, ..., eN]`</span><span class="sxs-lookup"><span data-stu-id="dec56-174">Element access: `e[e1, ..., eN]`</span></span>
*  <span data-ttu-id="dec56-175">Vytvoření objektu: `new C(e1, ..., eN)`</span><span class="sxs-lookup"><span data-stu-id="dec56-175">Object creation: `new C(e1, ..., eN)`</span></span>
*  <span data-ttu-id="dec56-176">Přetížení unárních operátorů: `+`, `-`, `!`, `~`, `++`, `--`, `true`, `false`</span><span class="sxs-lookup"><span data-stu-id="dec56-176">Overloaded unary operators: `+`, `-`, `!`, `~`, `++`, `--`, `true`, `false`</span></span>
*  <span data-ttu-id="dec56-177">Přetížené operátory binární: `+`, `-`, `*`, `/`, `%`, `&`, `&&`, `|`, `||`, `??`, `^`, `<<` , `>>`, `==`,`!=`, `>`, `<`, `>=`, `<=`</span><span class="sxs-lookup"><span data-stu-id="dec56-177">Overloaded binary operators: `+`, `-`, `*`, `/`, `%`, `&`, `&&`, `|`, `||`, `??`, `^`, `<<`, `>>`, `==`,`!=`, `>`, `<`, `>=`, `<=`</span></span>
*  <span data-ttu-id="dec56-178">Operátory přiřazení: `=`, `+=`, `-=`, `*=`, `/=`, `%=`, `&=`, `|=`, `^=`, `<<=`, `>>=`</span><span class="sxs-lookup"><span data-stu-id="dec56-178">Assignment operators: `=`, `+=`, `-=`, `*=`, `/=`, `%=`, `&=`, `|=`, `^=`, `<<=`, `>>=`</span></span>
*  <span data-ttu-id="dec56-179">Implicitní a explicitní převody</span><span class="sxs-lookup"><span data-stu-id="dec56-179">Implicit and explicit conversions</span></span>

<span data-ttu-id="dec56-180">Pokud se jedná o žádné dynamické výrazy jazyka C# výchozí statické vazby, což znamená, že typy kompilace základních výrazů se používají v procesu výběru.</span><span class="sxs-lookup"><span data-stu-id="dec56-180">When no dynamic expressions are involved, C# defaults to static binding, which means that the compile-time types of constituent expressions are used in the selection process.</span></span> <span data-ttu-id="dec56-181">Ale po dynamického výrazu jedné z nich základních výrazů v operacích výše uvedené operace místo dynamicky vázán.</span><span class="sxs-lookup"><span data-stu-id="dec56-181">However, when one of the constituent expressions in the operations listed above is a dynamic expression, the operation is instead dynamically bound.</span></span>

### <a name="binding-time"></a><span data-ttu-id="dec56-182">Doba vazby</span><span class="sxs-lookup"><span data-stu-id="dec56-182">Binding-time</span></span>

<span data-ttu-id="dec56-183">Statická vazba probíhá v době kompilace, zatímco dynamická vazba se provádí v době běhu.</span><span class="sxs-lookup"><span data-stu-id="dec56-183">Static binding takes place at compile-time, whereas dynamic binding takes place at run-time.</span></span> <span data-ttu-id="dec56-184">V následujících částech se termín ***doba vazby*** odkazuje na za kompilace nebo za běhu, v závislosti na tom, kdy vazba probíhá.</span><span class="sxs-lookup"><span data-stu-id="dec56-184">In the following sections, the term ***binding-time*** refers to either compile-time or run-time, depending on when the binding takes place.</span></span>

<span data-ttu-id="dec56-185">Následující příklad ukazuje názory statické a dynamické vazby a doby vazby:</span><span class="sxs-lookup"><span data-stu-id="dec56-185">The following example illustrates the notions of static and dynamic binding and of binding-time:</span></span>
```csharp
object  o = 5;
dynamic d = 5;

Console.WriteLine(5);  // static  binding to Console.WriteLine(int)
Console.WriteLine(o);  // static  binding to Console.WriteLine(object)
Console.WriteLine(d);  // dynamic binding to Console.WriteLine(int)
```

<span data-ttu-id="dec56-186">První dvě volání jsou staticky vázány: přetížení `Console.WriteLine` se vybere na základě typu kompilace svého argumentu.</span><span class="sxs-lookup"><span data-stu-id="dec56-186">The first two calls are statically bound: the overload of `Console.WriteLine` is picked based on the compile-time type of their argument.</span></span> <span data-ttu-id="dec56-187">Doba vazby tedy za kompilace.</span><span class="sxs-lookup"><span data-stu-id="dec56-187">Thus, the binding-time is compile-time.</span></span>

<span data-ttu-id="dec56-188">Třetí volání dynamicky vázán: přetížení `Console.WriteLine` se vybere na základě typu za běhu svého argumentu.</span><span class="sxs-lookup"><span data-stu-id="dec56-188">The third call is dynamically bound: the overload of `Console.WriteLine` is picked based on the run-time type of its argument.</span></span> <span data-ttu-id="dec56-189">Důvodem je argument dynamického výrazu – jeho typ za kompilace je `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="dec56-189">This happens because the argument is a dynamic expression -- its compile-time type is `dynamic`.</span></span> <span data-ttu-id="dec56-190">Doba vazby pro třetí volání je tedy za běhu.</span><span class="sxs-lookup"><span data-stu-id="dec56-190">Thus, the binding-time for the third call is run-time.</span></span>

### <a name="dynamic-binding"></a><span data-ttu-id="dec56-191">Dynamické vazby</span><span class="sxs-lookup"><span data-stu-id="dec56-191">Dynamic binding</span></span>

<span data-ttu-id="dec56-192">Účelem dynamické vazby je umožňuje programům C# pro interakci s ***dynamických objektů***, například systém typů objektů, které se neřídí běžných pravidel jazyka C#.</span><span class="sxs-lookup"><span data-stu-id="dec56-192">The purpose of dynamic binding is to allow C# programs to interact with ***dynamic objects***, i.e. objects that do not follow the normal rules of the C# type system.</span></span> <span data-ttu-id="dec56-193">Dynamické objekty mohou být objekty v jiných programovacích jazycích systémech různých typů nebo mohou být objekty, které jsou nastavené pro implementaci sémantiky vlastní vazby pro různé operace prostřednictvím kódu programu.</span><span class="sxs-lookup"><span data-stu-id="dec56-193">Dynamic objects may be objects from other programming languages with different types systems, or they may be objects that are programmatically setup to implement their own binding semantics for different operations.</span></span>

<span data-ttu-id="dec56-194">Mechanismus, pomocí kterého dynamický objekt implementuje vlastní sémantiku je definován implementací.</span><span class="sxs-lookup"><span data-stu-id="dec56-194">The mechanism by which a dynamic object implements its own semantics is implementation defined.</span></span> <span data-ttu-id="dec56-195">V příslušném rozhraní – znovu definován implementací – je implementováno dynamických objektů na signál pro C# doba běhu, ke kterým mají zvláštní sémantiku.</span><span class="sxs-lookup"><span data-stu-id="dec56-195">A given interface -- again implementation defined -- is implemented by dynamic objects to signal to the C# run-time that they have special semantics.</span></span> <span data-ttu-id="dec56-196">Díky tomu se pokaždé, když se operace na dynamický objekt se dynamicky vázán, sémantika vlastní vazby, ne ty jazyka C# uvedené v tomto dokumentu, převzít kontrolu nad.</span><span class="sxs-lookup"><span data-stu-id="dec56-196">Thus, whenever operations on a dynamic object are dynamically bound, their own binding semantics, rather than those of C# as specified in this document, take over.</span></span>

<span data-ttu-id="dec56-197">Účely dynamické vazby je chcete povolit vzájemné spolupráce s dynamickými objekty, C# umožňuje dynamické vazby u všech objektů, ať už jsou dynamické nebo ne.</span><span class="sxs-lookup"><span data-stu-id="dec56-197">While the purpose of dynamic binding is to allow interoperation with dynamic objects, C# allows dynamic binding on all objects, whether they are dynamic or not.</span></span> <span data-ttu-id="dec56-198">To umožňuje zajistit plynulejší integraci dynamických objektů, jako výsledky operací s nimi samy o sobě může být dynamické objekty, ale jsou stále typu Neznámý na programátorovi v době kompilace.</span><span class="sxs-lookup"><span data-stu-id="dec56-198">This allows for a smoother integration of dynamic objects, as the results of operations on them may not themselves be dynamic objects, but are still of a type unknown to the programmer at compile-time.</span></span> <span data-ttu-id="dec56-199">Dynamické vazby také může pomoct eliminovat náchylné založenými na reflexi kódu i v případě, že nejsou žádné objekty, které jsou zahrnuté dynamických objektů.</span><span class="sxs-lookup"><span data-stu-id="dec56-199">Also dynamic binding can help eliminate error-prone reflection-based code even when no objects involved are dynamic objects.</span></span>

<span data-ttu-id="dec56-200">Následující části popisují pro každý konstrukce v jazyce přesně při dynamické vazby se používá, co kompilaci kontrolu za – Pokud se použije některý – a jaké kompilaci výrazu and výsledek klasifikace je.</span><span class="sxs-lookup"><span data-stu-id="dec56-200">The following sections describe for each construct in the language exactly when dynamic binding is applied, what compile time checking -- if any -- is applied, and what the compile-time result and expression classification is.</span></span>

### <a name="types-of-constituent-expressions"></a><span data-ttu-id="dec56-201">Typy základních výrazů</span><span class="sxs-lookup"><span data-stu-id="dec56-201">Types of constituent expressions</span></span>

<span data-ttu-id="dec56-202">Při operaci je staticky vázán, typ výrazu základní (např. příjemce, argument, index nebo operand) je vždy považuje za kompilace typ tohoto výrazu.</span><span class="sxs-lookup"><span data-stu-id="dec56-202">When an operation is statically bound, the type of a constituent expression (e.g. a receiver, an argument, an index or an operand) is always considered to be the compile-time type of that expression.</span></span>

<span data-ttu-id="dec56-203">Když se operace dynamicky vazba, typ základní výrazu je určen různými způsoby v závislosti na kompilaci typu základní výraz:</span><span class="sxs-lookup"><span data-stu-id="dec56-203">When an operation is dynamically bound, the type of a constituent expression is determined in different ways depending on the compile-time type of the constituent expression:</span></span>

*  <span data-ttu-id="dec56-204">Základní výraz typu kompilace `dynamic` je považován za mít typ skutečnou hodnotu, která se výraz vyhodnotí za běhu</span><span class="sxs-lookup"><span data-stu-id="dec56-204">A constituent expression of compile-time type `dynamic` is considered to have the type of the actual value that the expression evaluates to at runtime</span></span>
*  <span data-ttu-id="dec56-205">Základní výraz, jehož typ za kompilace je parametr typu má mít typ, který typ parametru je vázán na za běhu</span><span class="sxs-lookup"><span data-stu-id="dec56-205">A constituent expression whose compile-time type is a type parameter is considered to have the type which the type parameter is bound to at runtime</span></span>
*  <span data-ttu-id="dec56-206">Jinak základní výraz je považován za mít jeho typ za kompilace.</span><span class="sxs-lookup"><span data-stu-id="dec56-206">Otherwise the constituent expression is considered to have its compile-time type.</span></span>

## <a name="operators"></a><span data-ttu-id="dec56-207">Operátory</span><span class="sxs-lookup"><span data-stu-id="dec56-207">Operators</span></span>

<span data-ttu-id="dec56-208">Výrazy se vytvářejí na základě ***operandy*** a ***operátory***.</span><span class="sxs-lookup"><span data-stu-id="dec56-208">Expressions are constructed from ***operands*** and ***operators***.</span></span> <span data-ttu-id="dec56-209">Operátory výrazu označují operace, které chcete použít pro operandy.</span><span class="sxs-lookup"><span data-stu-id="dec56-209">The operators of an expression indicate which operations to apply to the operands.</span></span> <span data-ttu-id="dec56-210">Příklady operátorů `+`, `-`, `*`, `/`, a `new`.</span><span class="sxs-lookup"><span data-stu-id="dec56-210">Examples of operators include `+`, `-`, `*`, `/`, and `new`.</span></span> <span data-ttu-id="dec56-211">Příklady operandy: literály, pole, místní proměnné a výrazy.</span><span class="sxs-lookup"><span data-stu-id="dec56-211">Examples of operands include literals, fields, local variables, and expressions.</span></span>

<span data-ttu-id="dec56-212">Existují tři typy operátorů:</span><span class="sxs-lookup"><span data-stu-id="dec56-212">There are three kinds of operators:</span></span>

*  <span data-ttu-id="dec56-213">Unární operátory.</span><span class="sxs-lookup"><span data-stu-id="dec56-213">Unary operators.</span></span> <span data-ttu-id="dec56-214">Unární operátory používají jeden operand a použijte buď předpony (jako `--x`) nebo přípony zápis (například `x++`).</span><span class="sxs-lookup"><span data-stu-id="dec56-214">The unary operators take one operand and use either prefix notation (such as `--x`) or postfix notation (such as `x++`).</span></span>
*  <span data-ttu-id="dec56-215">Binární operátory.</span><span class="sxs-lookup"><span data-stu-id="dec56-215">Binary operators.</span></span> <span data-ttu-id="dec56-216">Binární operátory přebírají dva operandy a všechny používají infixová notace (například `x + y`).</span><span class="sxs-lookup"><span data-stu-id="dec56-216">The binary operators take two operands and all use infix notation (such as `x + y`).</span></span>
*  <span data-ttu-id="dec56-217">Ternární operátor.</span><span class="sxs-lookup"><span data-stu-id="dec56-217">Ternary operator.</span></span> <span data-ttu-id="dec56-218">Pouze jeden Ternární operátor `?:`, existuje; má tři operandy a používá infix zápis (`c ? x : y`).</span><span class="sxs-lookup"><span data-stu-id="dec56-218">Only one ternary operator, `?:`, exists; it takes three operands and uses infix notation (`c ? x : y`).</span></span>

<span data-ttu-id="dec56-219">Pořadí vyhodnocení operátorů ve výrazu má vliv ***prioritu*** a ***asociativita*** operátorů ([Priorita a asociativita operátora](expressions.md#operator-precedence-and-associativity)) .</span><span class="sxs-lookup"><span data-stu-id="dec56-219">The order of evaluation of operators in an expression is determined by the ***precedence*** and ***associativity*** of the operators ([Operator precedence and associativity](expressions.md#operator-precedence-and-associativity)).</span></span>

<span data-ttu-id="dec56-220">Operandy ve výrazu jsou vyhodnoceny zleva doprava.</span><span class="sxs-lookup"><span data-stu-id="dec56-220">Operands in an expression are evaluated from left to right.</span></span> <span data-ttu-id="dec56-221">Například v `F(i) + G(i++) * H(i)`, metoda `F` jsou volány pomocí původní hodnota `i`, then – metoda `G` volána s původní hodnota `i`a nakonec – metoda `H` je volána s novou hodnotou `i`.</span><span class="sxs-lookup"><span data-stu-id="dec56-221">For example, in `F(i) + G(i++) * H(i)`, method `F` is called using the old value of `i`, then method `G` is called with the old value of `i`, and, finally, method `H` is called with the new value of `i`.</span></span> <span data-ttu-id="dec56-222">Toto je oddělené od a nesouvisejících priorita operátorů.</span><span class="sxs-lookup"><span data-stu-id="dec56-222">This is separate from and unrelated to operator precedence.</span></span>

<span data-ttu-id="dec56-223">Některé operátory lze ***přetížené***.</span><span class="sxs-lookup"><span data-stu-id="dec56-223">Certain operators can be ***overloaded***.</span></span> <span data-ttu-id="dec56-224">Přetížení operátoru umožňuje uživatelem definovaný operátor implementace rozhraní u operací zadat, pokud jeden nebo oba operandy jsou uživatelem definované třídě nebo struktuře typu ([přetížení operátoru](expressions.md#operator-overloading)).</span><span class="sxs-lookup"><span data-stu-id="dec56-224">Operator overloading permits user-defined operator implementations to be specified for operations where one or both of the operands are of a user-defined class or struct type ([Operator overloading](expressions.md#operator-overloading)).</span></span>

### <a name="operator-precedence-and-associativity"></a><span data-ttu-id="dec56-225">Priorita a asociativita operátora</span><span class="sxs-lookup"><span data-stu-id="dec56-225">Operator precedence and associativity</span></span>

<span data-ttu-id="dec56-226">Pokud výraz obsahuje více operátorů ***prioritu*** operátorů určuje pořadí, ve kterém jsou jednotlivé operátory vyhodnocovány.</span><span class="sxs-lookup"><span data-stu-id="dec56-226">When an expression contains multiple operators, the ***precedence*** of the operators controls the order in which the individual operators are evaluated.</span></span> <span data-ttu-id="dec56-227">Například výraz `x + y * z` se vyhodnotí jako `x + (y * z)` vzhledem k tomu, `*` operátor má vyšší prioritu než binární soubor `+` – operátor.</span><span class="sxs-lookup"><span data-stu-id="dec56-227">For example, the expression `x + y * z` is evaluated as `x + (y * z)` because the `*` operator has higher precedence than the binary `+` operator.</span></span> <span data-ttu-id="dec56-228">Přednost operátoru pokládáme stav, definicí jeho přidružené gramatiky výroby.</span><span class="sxs-lookup"><span data-stu-id="dec56-228">The precedence of an operator is established by the definition of its associated grammar production.</span></span> <span data-ttu-id="dec56-229">Například *additive_expression* se skládá z posloupnost *multiplicative_expression*s oddělené `+` nebo `-` operátory, což `+` a `-` nižší prioritu než operátory `*`, `/`, a `%` operátory.</span><span class="sxs-lookup"><span data-stu-id="dec56-229">For example, an *additive_expression* consists of a sequence of *multiplicative_expression*s separated by `+` or `-` operators, thus giving the `+` and `-` operators lower precedence than the `*`, `/`, and `%` operators.</span></span>

<span data-ttu-id="dec56-230">Následující tabulka shrnuje všechny operátory v pořadí dle priority od nejvyšší po nejnižší:</span><span class="sxs-lookup"><span data-stu-id="dec56-230">The following table summarizes all operators in order of precedence from highest to lowest:</span></span>

| <span data-ttu-id="dec56-231">__Oddíl__</span><span class="sxs-lookup"><span data-stu-id="dec56-231">__Section__</span></span>                                                                                   | <span data-ttu-id="dec56-232">__Kategorie__</span><span class="sxs-lookup"><span data-stu-id="dec56-232">__Category__</span></span>                | <span data-ttu-id="dec56-233">__Operátory__</span><span class="sxs-lookup"><span data-stu-id="dec56-233">__Operators__</span></span> | 
|-----------------------------------------------------------------------------------------------|-----------------------------|---------------|
| [<span data-ttu-id="dec56-234">Primární výrazy</span><span class="sxs-lookup"><span data-stu-id="dec56-234">Primary expressions</span></span>](expressions.md#primary-expressions)                                     | <span data-ttu-id="dec56-235">Primární</span><span class="sxs-lookup"><span data-stu-id="dec56-235">Primary</span></span>                     | <span data-ttu-id="dec56-236">`x.y`  `f(x)`  `a[x]`  `x++`  `x--`  `new`  `typeof`  `default`  `checked`  `unchecked`  `delegate`</span><span class="sxs-lookup"><span data-stu-id="dec56-236">`x.y`  `f(x)`  `a[x]`  `x++`  `x--`  `new`  `typeof`  `default`  `checked`  `unchecked`  `delegate`</span></span> | 
| [<span data-ttu-id="dec56-237">Unární operátory</span><span class="sxs-lookup"><span data-stu-id="dec56-237">Unary operators</span></span>](expressions.md#unary-operators)                                             | <span data-ttu-id="dec56-238">Unární</span><span class="sxs-lookup"><span data-stu-id="dec56-238">Unary</span></span>                       | <span data-ttu-id="dec56-239">`+`  `-`  `!`  `~`  `++x`  `--x`  `(T)x`</span><span class="sxs-lookup"><span data-stu-id="dec56-239">`+`  `-`  `!`  `~`  `++x`  `--x`  `(T)x`</span></span> | 
| [<span data-ttu-id="dec56-240">Aritmetické operátory</span><span class="sxs-lookup"><span data-stu-id="dec56-240">Arithmetic operators</span></span>](expressions.md#arithmetic-operators)                                   | <span data-ttu-id="dec56-241">Násobení</span><span class="sxs-lookup"><span data-stu-id="dec56-241">Multiplicative</span></span>              | <span data-ttu-id="dec56-242">`*`  `/`  `%`</span><span class="sxs-lookup"><span data-stu-id="dec56-242">`*`  `/`  `%`</span></span> | 
| [<span data-ttu-id="dec56-243">Aritmetické operátory</span><span class="sxs-lookup"><span data-stu-id="dec56-243">Arithmetic operators</span></span>](expressions.md#arithmetic-operators)                                   | <span data-ttu-id="dec56-244">Additive</span><span class="sxs-lookup"><span data-stu-id="dec56-244">Additive</span></span>                    | <span data-ttu-id="dec56-245">`+`  `-`</span><span class="sxs-lookup"><span data-stu-id="dec56-245">`+`  `-`</span></span>      | 
| [<span data-ttu-id="dec56-246">Operátory posunutí</span><span class="sxs-lookup"><span data-stu-id="dec56-246">Shift operators</span></span>](expressions.md#shift-operators)                                             | <span data-ttu-id="dec56-247">SHIFT</span><span class="sxs-lookup"><span data-stu-id="dec56-247">Shift</span></span>                       | <span data-ttu-id="dec56-248">`<<`  `>>`</span><span class="sxs-lookup"><span data-stu-id="dec56-248">`<<`  `>>`</span></span>    | 
| [<span data-ttu-id="dec56-249">Operátory relační a typové zkoušky</span><span class="sxs-lookup"><span data-stu-id="dec56-249">Relational and type-testing operators</span></span>](expressions.md#relational-and-type-testing-operators) | <span data-ttu-id="dec56-250">Relační a typové zkoušky</span><span class="sxs-lookup"><span data-stu-id="dec56-250">Relational and type testing</span></span> | <span data-ttu-id="dec56-251">`<`  `>`  `<=`  `>=`  `is`  `as`</span><span class="sxs-lookup"><span data-stu-id="dec56-251">`<`  `>`  `<=`  `>=`  `is`  `as`</span></span> | 
| [<span data-ttu-id="dec56-252">Operátory relační a typové zkoušky</span><span class="sxs-lookup"><span data-stu-id="dec56-252">Relational and type-testing operators</span></span>](expressions.md#relational-and-type-testing-operators) | <span data-ttu-id="dec56-253">Rovnost</span><span class="sxs-lookup"><span data-stu-id="dec56-253">Equality</span></span>                    | <span data-ttu-id="dec56-254">`==`  `!=`</span><span class="sxs-lookup"><span data-stu-id="dec56-254">`==`  `!=`</span></span>    | 
| [<span data-ttu-id="dec56-255">Logické operátory</span><span class="sxs-lookup"><span data-stu-id="dec56-255">Logical operators</span></span>](expressions.md#logical-operators)                                         | <span data-ttu-id="dec56-256">Logický operátor AND</span><span class="sxs-lookup"><span data-stu-id="dec56-256">Logical AND</span></span>                 | `&`           | 
| [<span data-ttu-id="dec56-257">Logické operátory</span><span class="sxs-lookup"><span data-stu-id="dec56-257">Logical operators</span></span>](expressions.md#logical-operators)                                         | <span data-ttu-id="dec56-258">Logický operátor XOR</span><span class="sxs-lookup"><span data-stu-id="dec56-258">Logical XOR</span></span>                 | `^`           | 
| [<span data-ttu-id="dec56-259">Logické operátory</span><span class="sxs-lookup"><span data-stu-id="dec56-259">Logical operators</span></span>](expressions.md#logical-operators)                                         | <span data-ttu-id="dec56-260">Logický operátor OR</span><span class="sxs-lookup"><span data-stu-id="dec56-260">Logical OR</span></span>                  | <code>&#124;</code>           |
| [<span data-ttu-id="dec56-261">Podmíněné logické operátory</span><span class="sxs-lookup"><span data-stu-id="dec56-261">Conditional logical operators</span></span>](expressions.md#conditional-logical-operators)                 | <span data-ttu-id="dec56-262">Podmiňovací operátor AND</span><span class="sxs-lookup"><span data-stu-id="dec56-262">Conditional AND</span></span>             | `&&`          | 
| [<span data-ttu-id="dec56-263">Podmíněné logické operátory</span><span class="sxs-lookup"><span data-stu-id="dec56-263">Conditional logical operators</span></span>](expressions.md#conditional-logical-operators)                 | <span data-ttu-id="dec56-264">Podmiňovací operátor OR</span><span class="sxs-lookup"><span data-stu-id="dec56-264">Conditional OR</span></span>              | <code>&#124;&#124;</code>          | 
| [<span data-ttu-id="dec56-265">Null operátor sloučení</span><span class="sxs-lookup"><span data-stu-id="dec56-265">The null coalescing operator</span></span>](expressions.md#the-null-coalescing-operator)                   | <span data-ttu-id="dec56-266">Nulové sloučení</span><span class="sxs-lookup"><span data-stu-id="dec56-266">Null coalescing</span></span>             | `??`          | 
| [<span data-ttu-id="dec56-267">Podmíněný operátor</span><span class="sxs-lookup"><span data-stu-id="dec56-267">Conditional operator</span></span>](expressions.md#conditional-operator)                                   | <span data-ttu-id="dec56-268">Podmiňovací operátor</span><span class="sxs-lookup"><span data-stu-id="dec56-268">Conditional</span></span>                 | `?:`          | 
| <span data-ttu-id="dec56-269">[Operátory přiřazení](expressions.md#assignment-operators), [výrazy anonymní funkce](expressions.md#anonymous-function-expressions)</span><span class="sxs-lookup"><span data-stu-id="dec56-269">[Assignment operators](expressions.md#assignment-operators), [Anonymous function expressions](expressions.md#anonymous-function-expressions)</span></span>  | <span data-ttu-id="dec56-270">Přiřazení a výraz lambda</span><span class="sxs-lookup"><span data-stu-id="dec56-270">Assignment and lambda expression</span></span> | <span data-ttu-id="dec56-271">`=`  `*=`  `/=`  `%=`  `+=`  `-=`  `<<=`  `>>=`  `&=`  `^=`  <code>&#124;=</code>  `=>`</span><span class="sxs-lookup"><span data-stu-id="dec56-271">`=`  `*=`  `/=`  `%=`  `+=`  `-=`  `<<=`  `>>=`  `&=`  `^=`  <code>&#124;=</code>  `=>`</span></span> | 

<span data-ttu-id="dec56-272">V případě operand mezi dva operátory se stejnou prioritou – ovládací prvky pořadí, ve kterém jsou operace prováděny asociativity operátorů:</span><span class="sxs-lookup"><span data-stu-id="dec56-272">When an operand occurs between two operators with the same precedence, the associativity of the operators controls the order in which the operations are performed:</span></span>

*  <span data-ttu-id="dec56-273">S výjimkou operátory přiřazení a null operátor sloučení, všechny binární operátory jsou ***asociativní zleva***, což znamená, že operace se provádějí zleva doprava.</span><span class="sxs-lookup"><span data-stu-id="dec56-273">Except for the assignment operators and the null coalescing operator, all binary operators are ***left-associative***, meaning that operations are performed from left to right.</span></span> <span data-ttu-id="dec56-274">Například `x + y + z` se vyhodnotí jako `(x + y) + z`.</span><span class="sxs-lookup"><span data-stu-id="dec56-274">For example, `x + y + z` is evaluated as `(x + y) + z`.</span></span>
*  <span data-ttu-id="dec56-275">Operátory přiřazení, null operátor sloučení a podmiňovací operátor (`?:`) jsou ***asociativní zprava***, což znamená, že operace se provádějí zprava doleva.</span><span class="sxs-lookup"><span data-stu-id="dec56-275">The assignment operators, the null coalescing operator and the conditional operator (`?:`) are ***right-associative***, meaning that operations are performed from right to left.</span></span> <span data-ttu-id="dec56-276">Například `x = y = z` se vyhodnotí jako `x = (y = z)`.</span><span class="sxs-lookup"><span data-stu-id="dec56-276">For example, `x = y = z` is evaluated as `x = (y = z)`.</span></span>

<span data-ttu-id="dec56-277">Přednost a asociativita operátorů lze ovládat pomocí závorek.</span><span class="sxs-lookup"><span data-stu-id="dec56-277">Precedence and associativity can be controlled using parentheses.</span></span> <span data-ttu-id="dec56-278">Například `x + y * z` nejprve vynásobí `y` podle `z` a pak přidá výsledek, který má `x`, ale `(x + y) * z` nejprve přidá `x` a `y` a pak vynásobí výsledků `z`.</span><span class="sxs-lookup"><span data-stu-id="dec56-278">For example, `x + y * z` first multiplies `y` by `z` and then adds the result to `x`, but `(x + y) * z` first adds `x` and `y` and then multiplies the result by `z`.</span></span>

### <a name="operator-overloading"></a><span data-ttu-id="dec56-279">Přetížení operátoru</span><span class="sxs-lookup"><span data-stu-id="dec56-279">Operator overloading</span></span>

<span data-ttu-id="dec56-280">Všechny jednočlenné a binární operátory mají předdefinované implementace, které jsou automaticky dostupné v libovolný výraz.</span><span class="sxs-lookup"><span data-stu-id="dec56-280">All unary and binary operators have predefined implementations that are automatically available in any expression.</span></span> <span data-ttu-id="dec56-281">Kromě předdefinovaných implementací, uživatelsky definované implementace může být zavedeno zahrnutím `operator` prohlášení do třídy a struktury ([operátory](classes.md#operators)).</span><span class="sxs-lookup"><span data-stu-id="dec56-281">In addition to the predefined implementations, user-defined implementations can be introduced by including `operator` declarations in classes and structs ([Operators](classes.md#operators)).</span></span> <span data-ttu-id="dec56-282">Uživatelem definovaný operátor implementace vždy přednost před implementací předdefinovaný operátor: Pouze pokud neexistují žádné použitelné uživatelem definovaný operátor implementace bude brát předdefinovaný operátor implementace, jak je popsáno v [unární operátor rozlišení přetěžování](expressions.md#unary-operator-overload-resolution) a [binární operátor přetížení rozlišení](expressions.md#binary-operator-overload-resolution).</span><span class="sxs-lookup"><span data-stu-id="dec56-282">User-defined operator implementations always take precedence over predefined operator implementations: Only when no applicable user-defined operator implementations exist will the predefined operator implementations be considered, as described in [Unary operator overload resolution](expressions.md#unary-operator-overload-resolution) and [Binary operator overload resolution](expressions.md#binary-operator-overload-resolution).</span></span>

<span data-ttu-id="dec56-283">***Očekával se přetěžovatelný unární operátory*** jsou:</span><span class="sxs-lookup"><span data-stu-id="dec56-283">The ***overloadable unary operators*** are:</span></span>
```csharp
+   -   !   ~   ++   --   true   false
```

<span data-ttu-id="dec56-284">I když `true` a `false` nejsou explicitně použít ve výrazech (a proto nejsou zahrnuty v tabulce priority v [Priorita a asociativita operátora](expressions.md#operator-precedence-and-associativity)), protože jsou jsou považovány za operátory vyvolána v několika kontextech výraz: logické výrazy ([logické výrazy](expressions.md#boolean-expressions)) a výrazy, které obsahují podmínku ([Podmiňovací operátor](expressions.md#conditional-operator)) a podmíněné logické operátory ([podmíněné logické operátory](expressions.md#conditional-logical-operators)).</span><span class="sxs-lookup"><span data-stu-id="dec56-284">Although `true` and `false` are not used explicitly in expressions (and therefore are not included in the precedence table in [Operator precedence and associativity](expressions.md#operator-precedence-and-associativity)), they are considered operators because they are invoked in several expression contexts: boolean expressions ([Boolean expressions](expressions.md#boolean-expressions)) and expressions involving the conditional ([Conditional operator](expressions.md#conditional-operator)), and conditional logical operators ([Conditional logical operators](expressions.md#conditional-logical-operators)).</span></span>

<span data-ttu-id="dec56-285">***Očekával se přetěžovatelný binární operátory*** jsou:</span><span class="sxs-lookup"><span data-stu-id="dec56-285">The ***overloadable binary operators*** are:</span></span>
```csharp
+   -   *   /   %   &   |   ^   <<   >>   ==   !=   >   <   >=   <=
```

<span data-ttu-id="dec56-286">Mohou být přetíženy pouze operátory, které jsou uvedené výše.</span><span class="sxs-lookup"><span data-stu-id="dec56-286">Only the operators listed above can be overloaded.</span></span> <span data-ttu-id="dec56-287">Zejména, není možné přetížit přístup ke členu, volání metody nebo `=`, `&&`, `||`, `??`, `?:`, `=>`, `checked`, `unchecked`, `new`, `typeof`, `default`, `as`, a `is` operátory.</span><span class="sxs-lookup"><span data-stu-id="dec56-287">In particular, it is not possible to overload member access, method invocation, or the `=`, `&&`, `||`, `??`, `?:`, `=>`, `checked`, `unchecked`, `new`, `typeof`, `default`, `as`, and `is` operators.</span></span>

<span data-ttu-id="dec56-288">Při je binární operátor přetížen, odpovídající operátor přiřazení, pokud existuje, je také implicitně přetížená.</span><span class="sxs-lookup"><span data-stu-id="dec56-288">When a binary operator is overloaded, the corresponding assignment operator, if any, is also implicitly overloaded.</span></span> <span data-ttu-id="dec56-289">Například přetížení operátoru `*` je také přetížení operátoru `*=`.</span><span class="sxs-lookup"><span data-stu-id="dec56-289">For example, an overload of operator `*` is also an overload of operator `*=`.</span></span> <span data-ttu-id="dec56-290">Toto je popsáno dále v [složené přiřazení](expressions.md#compound-assignment).</span><span class="sxs-lookup"><span data-stu-id="dec56-290">This is described further in [Compound assignment](expressions.md#compound-assignment).</span></span> <span data-ttu-id="dec56-291">Všimněte si, že operátor přiřazení (`=`) se nedají přetěžovat.</span><span class="sxs-lookup"><span data-stu-id="dec56-291">Note that the assignment operator itself (`=`) cannot be overloaded.</span></span> <span data-ttu-id="dec56-292">Přiřazení vždy provádí jednoduché kopírování bitové hodnoty do proměnné.</span><span class="sxs-lookup"><span data-stu-id="dec56-292">An assignment always performs a simple bit-wise copy of a value into a variable.</span></span>

<span data-ttu-id="dec56-293">Operace, jako například přetypování `(T)x`, jsou přetížené poskytnutím uživatelem definované převody ([uživatelem definované převody](conversions.md#user-defined-conversions)).</span><span class="sxs-lookup"><span data-stu-id="dec56-293">Cast operations, such as `(T)x`, are overloaded by providing user-defined conversions ([User-defined conversions](conversions.md#user-defined-conversions)).</span></span>

<span data-ttu-id="dec56-294">Přístup k elementu, jako například `a[x]`, není považováno za očekával se přetěžovatelný operátor.</span><span class="sxs-lookup"><span data-stu-id="dec56-294">Element access, such as `a[x]`, is not considered an overloadable operator.</span></span> <span data-ttu-id="dec56-295">Místo toho uživatelem definované indexování se podporuje na indexery ([indexery](classes.md#indexers)).</span><span class="sxs-lookup"><span data-stu-id="dec56-295">Instead, user-defined indexing is supported through indexers ([Indexers](classes.md#indexers)).</span></span>

<span data-ttu-id="dec56-296">Ve výrazech operátory je odkazováno pomocí zápisu operátor a v deklaracích, operátory jsou odkazovány pomocí funkční zápis.</span><span class="sxs-lookup"><span data-stu-id="dec56-296">In expressions, operators are referenced using operator notation, and in declarations, operators are referenced using functional notation.</span></span> <span data-ttu-id="dec56-297">Následující tabulka znázorňuje vztah mezi funkční zápisy jednočlenné a binární operátory a operátor.</span><span class="sxs-lookup"><span data-stu-id="dec56-297">The following table shows the relationship between operator and functional notations for unary and binary operators.</span></span> <span data-ttu-id="dec56-298">V první položce *op* označuje všechny očekával se přetěžovatelný unární operátor předpony.</span><span class="sxs-lookup"><span data-stu-id="dec56-298">In the first entry, *op* denotes any overloadable unary prefix operator.</span></span> <span data-ttu-id="dec56-299">V položce druhý *op* označuje příponový unární `++` a `--` operátory.</span><span class="sxs-lookup"><span data-stu-id="dec56-299">In the second entry, *op* denotes the unary postfix `++` and `--` operators.</span></span> <span data-ttu-id="dec56-300">V položce třetí *op* označuje všechny očekával se přetěžovatelný binární operátor.</span><span class="sxs-lookup"><span data-stu-id="dec56-300">In the third entry, *op* denotes any overloadable binary operator.</span></span>


| <span data-ttu-id="dec56-301">__Zápis operátoru__</span><span class="sxs-lookup"><span data-stu-id="dec56-301">__Operator notation__</span></span> | <span data-ttu-id="dec56-302">__Funkční zápis__</span><span class="sxs-lookup"><span data-stu-id="dec56-302">__Functional notation__</span></span> |
|-----------------------|-------------------------|
| `op x`                | `operator op(x)`        | 
| `x op`                | `operator op(x)`        | 
| `x op y`              | `operator op(x,y)`      | 

<span data-ttu-id="dec56-303">Uživatelem definovaný operátor deklarace vždy vyžadují alespoň jeden z parametrů typu třídy nebo struktury, obsahující deklarace operátoru.</span><span class="sxs-lookup"><span data-stu-id="dec56-303">User-defined operator declarations always require at least one of the parameters to be of the class or struct type that contains the operator declaration.</span></span> <span data-ttu-id="dec56-304">Proto není možné pro uživatelem definovaný operátor mít stejný podpis jako předdefinovaný operátor.</span><span class="sxs-lookup"><span data-stu-id="dec56-304">Thus, it is not possible for a user-defined operator to have the same signature as a predefined operator.</span></span>

<span data-ttu-id="dec56-305">Uživatelem definovaný operátor deklarace nelze upravit, syntaxe, Priorita a asociativita operátora.</span><span class="sxs-lookup"><span data-stu-id="dec56-305">User-defined operator declarations cannot modify the syntax, precedence, or associativity of an operator.</span></span> <span data-ttu-id="dec56-306">Například `/` operátor je vždy binárních operátorů, vždy má úroveň priority zadané v [Priorita a asociativita operátora](expressions.md#operator-precedence-and-associativity)a vždy je asociativní zprava doleva.</span><span class="sxs-lookup"><span data-stu-id="dec56-306">For example, the `/` operator is always a binary operator, always has the precedence level specified in [Operator precedence and associativity](expressions.md#operator-precedence-and-associativity), and is always left-associative.</span></span>

<span data-ttu-id="dec56-307">I když je možné pro uživatelem definovaný operátor provádět jakékoli výpočty, které ho pleases, implementace, které výsledky než ty, kteří budou intuitivně se důrazně nedoporučuje.</span><span class="sxs-lookup"><span data-stu-id="dec56-307">While it is possible for a user-defined operator to perform any computation it pleases, implementations that produce results other than those that are intuitively expected are strongly discouraged.</span></span> <span data-ttu-id="dec56-308">Například implementace `operator ==` musí porovnat dva operandy na rovnost a vrátí odpovídající `bool` výsledek.</span><span class="sxs-lookup"><span data-stu-id="dec56-308">For example, an implementation of `operator ==` should compare the two operands for equality and return an appropriate `bool` result.</span></span>

<span data-ttu-id="dec56-309">Popisy jednotlivých operátorech v [primární výrazy](expressions.md#primary-expressions) prostřednictvím [podmíněné logické operátory](expressions.md#conditional-logical-operators) zadejte předdefinované implementace operátorů a veškerá další pravidla, které se vztahují Každý operátor.</span><span class="sxs-lookup"><span data-stu-id="dec56-309">The descriptions of individual operators in [Primary expressions](expressions.md#primary-expressions) through [Conditional logical operators](expressions.md#conditional-logical-operators) specify the predefined implementations of the operators and any additional rules that apply to each operator.</span></span> <span data-ttu-id="dec56-310">Ujistěte se, popisy použijte podmínek ***unární operátor rozlišení přetěžování***, ***binárním operátorem rozlišení přetěžování***, a ***číselné povýšení***, definice, které jsou najít v následujících částech.</span><span class="sxs-lookup"><span data-stu-id="dec56-310">The descriptions make use of the terms ***unary operator overload resolution***, ***binary operator overload resolution***, and ***numeric promotion***, definitions of which are found in the following sections.</span></span>

### <a name="unary-operator-overload-resolution"></a><span data-ttu-id="dec56-311">Řešení přetížení unární operátor</span><span class="sxs-lookup"><span data-stu-id="dec56-311">Unary operator overload resolution</span></span>

<span data-ttu-id="dec56-312">Operace formuláře `op x` nebo `x op`, kde `op` se očekával se přetěžovatelný unární operátor a `x` je výraz typu `X`, zpracování následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="dec56-312">An operation of the form `op x` or `x op`, where `op` is an overloadable unary operator, and `x` is an expression of type `X`, is processed as follows:</span></span>

*  <span data-ttu-id="dec56-313">Sadu Release candidate uživatelsky definované operátory poskytované `X` pro operaci `operator op(x)` je určen pomocí pravidel pro [Release Candidate uživatelsky definované operátory](expressions.md#candidate-user-defined-operators).</span><span class="sxs-lookup"><span data-stu-id="dec56-313">The set of candidate user-defined operators provided by `X` for the operation `operator op(x)` is determined using the rules of [Candidate user-defined operators](expressions.md#candidate-user-defined-operators).</span></span>
*  <span data-ttu-id="dec56-314">Pokud sadu Release candidate uživatelsky definované operátory není prázdná, to bude sadu operátorů Release candidate pro operaci.</span><span class="sxs-lookup"><span data-stu-id="dec56-314">If the set of candidate user-defined operators is not empty, then this becomes the set of candidate operators for the operation.</span></span> <span data-ttu-id="dec56-315">V opačném případě předdefinované unární `operator op` implementací, včetně jejich zdvižené formulářů, stane sadu operátorů Release candidate pro operaci.</span><span class="sxs-lookup"><span data-stu-id="dec56-315">Otherwise, the predefined unary `operator op` implementations, including their lifted forms, become the set of candidate operators for the operation.</span></span> <span data-ttu-id="dec56-316">Předdefinované implementace daný operátor jsou uvedeny v popisu operátoru ([primární výrazy](expressions.md#primary-expressions) a [unárních operátorů](expressions.md#unary-operators)).</span><span class="sxs-lookup"><span data-stu-id="dec56-316">The predefined implementations of a given operator are specified in the description of the operator ([Primary expressions](expressions.md#primary-expressions) and [Unary operators](expressions.md#unary-operators)).</span></span>
*  <span data-ttu-id="dec56-317">Pravidla rozlišení přetížení [rozlišení přetěžování](expressions.md#overload-resolution) aplikují i na sadu operátorů Release candidate vybrat nejlepší operátor s ohledem na seznam argumentů `(x)`, a tento operátor změní výsledek přetížení Proces překladu.</span><span class="sxs-lookup"><span data-stu-id="dec56-317">The overload resolution rules of [Overload resolution](expressions.md#overload-resolution) are applied to the set of candidate operators to select the best operator with respect to the argument list `(x)`, and this operator becomes the result of the overload resolution process.</span></span> <span data-ttu-id="dec56-318">Pokud se nepodaří určit přetížení k výběru jednoho nejlepší operátoru, dojde k chybě vazby čas.</span><span class="sxs-lookup"><span data-stu-id="dec56-318">If overload resolution fails to select a single best operator, a binding-time error occurs.</span></span>

### <a name="binary-operator-overload-resolution"></a><span data-ttu-id="dec56-319">Binární operátor rozlišení přetížení</span><span class="sxs-lookup"><span data-stu-id="dec56-319">Binary operator overload resolution</span></span>

<span data-ttu-id="dec56-320">Operace formuláře `x op y`, kde `op` se očekával se přetěžovatelný binární operátor `x` je výraz typu `X`, a `y` je výraz typu `Y`, zpracování následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="dec56-320">An operation of the form `x op y`, where `op` is an overloadable binary operator, `x` is an expression of type `X`, and `y` is an expression of type `Y`, is processed as follows:</span></span>

*  <span data-ttu-id="dec56-321">Sadu Release candidate uživatelsky definované operátory poskytované `X` a `Y` pro operaci `operator op(x,y)` je určen.</span><span class="sxs-lookup"><span data-stu-id="dec56-321">The set of candidate user-defined operators provided by `X` and `Y` for the operation `operator op(x,y)` is determined.</span></span> <span data-ttu-id="dec56-322">Sada zahrnuje union operátorů Release candidate poskytované `X` operátory a operátory Release candidate poskytované `Y`, každý určené pomocí pravidel pro [Release Candidate uživatelsky definované operátory](expressions.md#candidate-user-defined-operators).</span><span class="sxs-lookup"><span data-stu-id="dec56-322">The set consists of the union of the candidate operators provided by `X` and the candidate operators provided by `Y`, each determined using the rules of [Candidate user-defined operators](expressions.md#candidate-user-defined-operators).</span></span> <span data-ttu-id="dec56-323">Pokud `X` a `Y` jsou stejného typu, nebo pokud `X` a `Y` jsou odvozeny z běžné základní typ, pak sdílené Release candidate operátory vyskytovat jenom v sadě kombinované jednou.</span><span class="sxs-lookup"><span data-stu-id="dec56-323">If `X` and `Y` are the same type, or if `X` and `Y` are derived from a common base type, then shared candidate operators only occur in the combined set once.</span></span>
*  <span data-ttu-id="dec56-324">Pokud sadu Release candidate uživatelsky definované operátory není prázdná, to bude sadu operátorů Release candidate pro operaci.</span><span class="sxs-lookup"><span data-stu-id="dec56-324">If the set of candidate user-defined operators is not empty, then this becomes the set of candidate operators for the operation.</span></span> <span data-ttu-id="dec56-325">V opačném případě předdefinovaný binární `operator op` implementací, včetně jejich zdvižené formulářů, stane sadu operátorů Release candidate pro operaci.</span><span class="sxs-lookup"><span data-stu-id="dec56-325">Otherwise, the predefined binary `operator op` implementations, including their lifted forms,  become the set of candidate operators for the operation.</span></span> <span data-ttu-id="dec56-326">Předdefinované implementace daný operátor jsou uvedeny v popisu operátoru ([aritmetické operátory](expressions.md#arithmetic-operators) prostřednictvím [podmíněné logické operátory](expressions.md#conditional-logical-operators)).</span><span class="sxs-lookup"><span data-stu-id="dec56-326">The predefined implementations of a given operator are specified in the description of the operator ([Arithmetic operators](expressions.md#arithmetic-operators) through [Conditional logical operators](expressions.md#conditional-logical-operators)).</span></span> <span data-ttu-id="dec56-327">Pro předdefinované operátory výčtu a delegáta jen operátory považovat za jsou definované typem enum nebo delegate, který je typ vazby time jeden z operandů.</span><span class="sxs-lookup"><span data-stu-id="dec56-327">For predefined enum and delegate operators, the only operators considered are those defined by an enum or delegate type that is the binding-time type of one of the operands.</span></span>
*  <span data-ttu-id="dec56-328">Pravidla rozlišení přetížení [rozlišení přetěžování](expressions.md#overload-resolution) aplikují i na sadu operátorů Release candidate vybrat nejlepší operátor s ohledem na seznam argumentů `(x,y)`, a tento operátor změní výsledek přetížení Proces překladu.</span><span class="sxs-lookup"><span data-stu-id="dec56-328">The overload resolution rules of [Overload resolution](expressions.md#overload-resolution) are applied to the set of candidate operators to select the best operator with respect to the argument list `(x,y)`, and this operator becomes the result of the overload resolution process.</span></span> <span data-ttu-id="dec56-329">Pokud se nepodaří určit přetížení k výběru jednoho nejlepší operátoru, dojde k chybě vazby čas.</span><span class="sxs-lookup"><span data-stu-id="dec56-329">If overload resolution fails to select a single best operator, a binding-time error occurs.</span></span>

### <a name="candidate-user-defined-operators"></a><span data-ttu-id="dec56-330">Uživatelem definované operátory Release Candidate</span><span class="sxs-lookup"><span data-stu-id="dec56-330">Candidate user-defined operators</span></span>

<span data-ttu-id="dec56-331">Typ zadaný `T` a operace `operator op(A)`, kde `op` se očekával se přetěžovatelný operátor a `A` je seznam argumentů, sadu Release candidate uživatelsky definované operátory poskytované `T` pro `operator op(A)` je určen následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="dec56-331">Given a type `T` and an operation `operator op(A)`, where `op` is an overloadable operator and `A` is an argument list, the set of candidate user-defined operators provided by `T` for `operator op(A)` is determined as follows:</span></span>

*  <span data-ttu-id="dec56-332">Určení typu `T0`.</span><span class="sxs-lookup"><span data-stu-id="dec56-332">Determine the type `T0`.</span></span> <span data-ttu-id="dec56-333">Pokud `T` je typ připouštějící hodnotu Null, `T0` je základní typ, v opačném případě `T0` rovná `T`.</span><span class="sxs-lookup"><span data-stu-id="dec56-333">If `T` is a nullable type, `T0` is its underlying type, otherwise `T0` is equal to `T`.</span></span>
*  <span data-ttu-id="dec56-334">Pro všechny `operator op` deklarace v `T0` a všechny zrušeno formy takových operátorů, pokud platí aspoň jeden – operátor ([použitelná funkce člena](expressions.md#applicable-function-member)) s ohledem na seznam argumentů `A`, pak sada operátory Release Candidate se skládá z takových příslušných operátorů v `T0`.</span><span class="sxs-lookup"><span data-stu-id="dec56-334">For all `operator op` declarations in `T0` and all lifted forms of such operators, if at least one operator is applicable ([Applicable function member](expressions.md#applicable-function-member)) with respect to the argument list `A`, then the set of candidate operators consists of all such applicable operators in `T0`.</span></span>
*  <span data-ttu-id="dec56-335">Jinak, pokud `T0` je `object`, sadu operátorů Release candidate je prázdný.</span><span class="sxs-lookup"><span data-stu-id="dec56-335">Otherwise, if `T0` is `object`, the set of candidate operators is empty.</span></span>
*  <span data-ttu-id="dec56-336">V opačném případě poskytuje sadu operátorů Release candidate `T0` sadu Release candidate operátory poskytované přímé základní třídy `T0`, nebo efektivní základní třídu `T0` Pokud `T0` je parametr typu.</span><span class="sxs-lookup"><span data-stu-id="dec56-336">Otherwise, the set of candidate operators provided by `T0` is the set of candidate operators provided by the direct base class of `T0`, or the effective base class of `T0` if `T0` is a type parameter.</span></span>

### <a name="numeric-promotions"></a><span data-ttu-id="dec56-337">Číselné propagačních akcí</span><span class="sxs-lookup"><span data-stu-id="dec56-337">Numeric promotions</span></span>

<span data-ttu-id="dec56-338">Číselné povýšení se skládá z automaticky provádět určité implicitní převody operandů předdefinované jednočlenné a binární číselný operátory.</span><span class="sxs-lookup"><span data-stu-id="dec56-338">Numeric promotion consists of automatically performing certain implicit conversions of the operands of the predefined unary and binary numeric operators.</span></span> <span data-ttu-id="dec56-339">Číselné promoakce není odlišné mechanismus, ale spíše efekt použití předdefinované operátory řešení přetížení.</span><span class="sxs-lookup"><span data-stu-id="dec56-339">Numeric promotion is not a distinct mechanism, but rather an effect of applying overload resolution to the predefined operators.</span></span> <span data-ttu-id="dec56-340">Číselné povýšení konkrétně nemá vliv na vyhodnocení operátory definované uživatelem, i když je možné implementovat uživatelsky definované operátory vykazovat podobné účinky.</span><span class="sxs-lookup"><span data-stu-id="dec56-340">Numeric promotion specifically does not affect evaluation of user-defined operators, although user-defined operators can be implemented to exhibit similar effects.</span></span>

<span data-ttu-id="dec56-341">Jako příklad číselné povýšení, vezměte v úvahu předdefinované implementace binárního souboru `*` operátor:</span><span class="sxs-lookup"><span data-stu-id="dec56-341">As an example of numeric promotion, consider the predefined implementations of the binary `*` operator:</span></span>

```csharp
int operator *(int x, int y);
uint operator *(uint x, uint y);
long operator *(long x, long y);
ulong operator *(ulong x, ulong y);
float operator *(float x, float y);
double operator *(double x, double y);
decimal operator *(decimal x, decimal y);
```

<span data-ttu-id="dec56-342">Při přetížení pravidla řešení ([rozlišení přetěžování](expressions.md#overload-resolution)) se použijí pro tuto sadu operátorů, je vybrat první operátory, pro které existuje implicitní převody z typů operand efekt.</span><span class="sxs-lookup"><span data-stu-id="dec56-342">When overload resolution rules ([Overload resolution](expressions.md#overload-resolution)) are applied to this set of operators, the effect is to select the first of the operators for which implicit conversions exist from the operand types.</span></span> <span data-ttu-id="dec56-343">Například pro operaci `b * s`, kde `b` je `byte` a `s` je `short`, vybere rozlišení přetížení `operator *(int,int)` jako nejlepší operátor.</span><span class="sxs-lookup"><span data-stu-id="dec56-343">For example, for the operation `b * s`, where `b` is a `byte` and `s` is a `short`, overload resolution selects `operator *(int,int)` as the best operator.</span></span> <span data-ttu-id="dec56-344">Proto efekt je, že `b` a `s` jsou převedeny na `int`, a typ výsledku je `int`.</span><span class="sxs-lookup"><span data-stu-id="dec56-344">Thus, the effect is that `b` and `s` are converted to `int`, and the type of the result is `int`.</span></span> <span data-ttu-id="dec56-345">Podobně pro operaci `i * d`, kde `i` je `int` a `d` je `double`, vybere rozlišení přetížení `operator *(double,double)` jako nejlepší operátor.</span><span class="sxs-lookup"><span data-stu-id="dec56-345">Likewise, for the operation `i * d`, where `i` is an `int` and `d` is a `double`, overload resolution selects `operator *(double,double)` as the best operator.</span></span>

#### <a name="unary-numeric-promotions"></a><span data-ttu-id="dec56-346">Unární číselné propagačních akcí</span><span class="sxs-lookup"><span data-stu-id="dec56-346">Unary numeric promotions</span></span>

<span data-ttu-id="dec56-347">Pro operandy předdefinovaného dojde k povýšení číselné unární `+`, `-`, a `~` unárních operátorů.</span><span class="sxs-lookup"><span data-stu-id="dec56-347">Unary numeric promotion occurs for the operands of the predefined `+`, `-`, and `~` unary operators.</span></span> <span data-ttu-id="dec56-348">Unární číselné povýšení jednoduše se skládá z převodu operandy typu `sbyte`, `byte`, `short`, `ushort`, nebo `char` na typ `int`.</span><span class="sxs-lookup"><span data-stu-id="dec56-348">Unary numeric promotion simply consists of converting operands of type `sbyte`, `byte`, `short`, `ushort`, or `char` to type `int`.</span></span> <span data-ttu-id="dec56-349">Kromě toho pro unární `-` operátor unární číselné povýšení převede operandy typu `uint` na typ `long`.</span><span class="sxs-lookup"><span data-stu-id="dec56-349">Additionally, for the unary `-` operator, unary numeric promotion converts operands of type `uint` to type `long`.</span></span>

#### <a name="binary-numeric-promotions"></a><span data-ttu-id="dec56-350">Binární číselný propagačních akcí</span><span class="sxs-lookup"><span data-stu-id="dec56-350">Binary numeric promotions</span></span>

<span data-ttu-id="dec56-351">Pro operandy předdefinovaného dojde k binární číselný povýšení `+`, `-`, `*`, `/`, `%`, `&`, `|`, `^`, `==`, `!=`, `>`, `<`, `>=`, a `<=` binární operátory.</span><span class="sxs-lookup"><span data-stu-id="dec56-351">Binary numeric promotion occurs for the operands of the predefined `+`, `-`, `*`, `/`, `%`, `&`, `|`, `^`, `==`, `!=`, `>`, `<`, `>=`, and `<=` binary operators.</span></span> <span data-ttu-id="dec56-352">Oba operandy binární číselný povýšení implicitně převede na společný typ, který se v případě – relační operátory stane typ výsledku operace.</span><span class="sxs-lookup"><span data-stu-id="dec56-352">Binary numeric promotion implicitly converts both operands to a common type which, in case of the non-relational operators, also becomes the result type of the operation.</span></span> <span data-ttu-id="dec56-353">Binární číselný povýšení se skládá z použití následující pravidla v pořadí, ve kterém jsou uvedeny zde:</span><span class="sxs-lookup"><span data-stu-id="dec56-353">Binary numeric promotion consists of applying the following rules, in the order they appear here:</span></span>

*  <span data-ttu-id="dec56-354">Pokud některý operand je typu `decimal`, je druhý operand je převeden na typ `decimal`, nebo pokud je druhý operand je typu, dojde k chybě vazby čas `float` nebo `double`.</span><span class="sxs-lookup"><span data-stu-id="dec56-354">If either operand is of type `decimal`, the other operand is converted to type `decimal`, or a binding-time error occurs if the other operand is of type `float` or `double`.</span></span>
*  <span data-ttu-id="dec56-355">Jinak, pokud některý operand je typu `double`, je druhý operand je převeden na typ `double`.</span><span class="sxs-lookup"><span data-stu-id="dec56-355">Otherwise, if either operand is of type `double`, the other operand is converted to type `double`.</span></span>
*  <span data-ttu-id="dec56-356">Jinak, pokud některý operand je typu `float`, je druhý operand je převeden na typ `float`.</span><span class="sxs-lookup"><span data-stu-id="dec56-356">Otherwise, if either operand is of type `float`, the other operand is converted to type `float`.</span></span>
*  <span data-ttu-id="dec56-357">Jinak, pokud některý operand je typu `ulong`, je druhý operand je převeden na typ `ulong`, nebo pokud je druhý operand je typu, dojde k chybě vazby čas `sbyte`, `short`, `int`, nebo `long`.</span><span class="sxs-lookup"><span data-stu-id="dec56-357">Otherwise, if either operand is of type `ulong`, the other operand is converted to type `ulong`, or a binding-time error occurs if the other operand is of type `sbyte`, `short`, `int`, or `long`.</span></span>
*  <span data-ttu-id="dec56-358">Jinak, pokud některý operand je typu `long`, je druhý operand je převeden na typ `long`.</span><span class="sxs-lookup"><span data-stu-id="dec56-358">Otherwise, if either operand is of type `long`, the other operand is converted to type `long`.</span></span>
*  <span data-ttu-id="dec56-359">Jinak, pokud některý operand je typu `uint` a druhý operand je typu `sbyte`, `short`, nebo `int`, jsou oba operandy převedeny na typ `long`.</span><span class="sxs-lookup"><span data-stu-id="dec56-359">Otherwise, if either operand is of type `uint` and the other operand is of type `sbyte`, `short`, or `int`, both operands are converted to type `long`.</span></span>
*  <span data-ttu-id="dec56-360">Jinak, pokud některý operand je typu `uint`, je druhý operand je převeden na typ `uint`.</span><span class="sxs-lookup"><span data-stu-id="dec56-360">Otherwise, if either operand is of type `uint`, the other operand is converted to type `uint`.</span></span>
*  <span data-ttu-id="dec56-361">Jinak jsou oba operandy převedeny na typ `int`.</span><span class="sxs-lookup"><span data-stu-id="dec56-361">Otherwise, both operands are converted to type `int`.</span></span>

<span data-ttu-id="dec56-362">Všimněte si, že první pravidlo nepovoluje žádné operace, které kombinovat `decimal` typ s `double` a `float` typy.</span><span class="sxs-lookup"><span data-stu-id="dec56-362">Note that the first rule disallows any operations that mix the `decimal` type with the `double` and `float` types.</span></span> <span data-ttu-id="dec56-363">Následující pravidlo ze skutečnosti, že neexistují žádné implicitní převody mezi `decimal` typ a `double` a `float` typy.</span><span class="sxs-lookup"><span data-stu-id="dec56-363">The rule follows from the fact that there are no implicit conversions between the `decimal` type and the `double` and `float` types.</span></span>

<span data-ttu-id="dec56-364">Všimněte si také, že není možné pro operand typu `ulong` Pokud je druhý operand je celočíselný typ se znaménkem.</span><span class="sxs-lookup"><span data-stu-id="dec56-364">Also note that it is not possible for an operand to be of type `ulong` when the other operand is of a signed integral type.</span></span> <span data-ttu-id="dec56-365">Důvodem je, že neexistuje žádné celočíselného typu, který může představovat celou škálu `ulong` a také podepsaných integrálních typů.</span><span class="sxs-lookup"><span data-stu-id="dec56-365">The reason is that no integral type exists that can represent the full range of `ulong` as well as the signed integral types.</span></span>

<span data-ttu-id="dec56-366">V obou případech výše výraz přetypování lze explicitně převést na typ, který je kompatibilní s je druhý operand jeden operand.</span><span class="sxs-lookup"><span data-stu-id="dec56-366">In both of the above cases, a cast expression can be used to explicitly convert one operand to a type that is compatible with the other operand.</span></span>

<span data-ttu-id="dec56-367">V příkladu</span><span class="sxs-lookup"><span data-stu-id="dec56-367">In the example</span></span>
```csharp
decimal AddPercent(decimal x, double percent) {
    return x * (1.0 + percent / 100.0);
}
```
<span data-ttu-id="dec56-368">Doba vazby chybě dochází, protože `decimal` nelze bude vynásobené hodnotou `double`.</span><span class="sxs-lookup"><span data-stu-id="dec56-368">a binding-time error occurs because a `decimal` cannot be multiplied by a `double`.</span></span> <span data-ttu-id="dec56-369">Explicitně převedením Druhý operand, který se problém nevyřeší `decimal`, následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="dec56-369">The error is resolved by explicitly converting the second operand to `decimal`, as follows:</span></span>

```csharp
decimal AddPercent(decimal x, double percent) {
    return x * (decimal)(1.0 + percent / 100.0);
}
```

### <a name="lifted-operators"></a><span data-ttu-id="dec56-370">Zdvižené operátory</span><span class="sxs-lookup"><span data-stu-id="dec56-370">Lifted operators</span></span>

<span data-ttu-id="dec56-371">***Zrušeno vs. operátory*** povolit předdefinované a uživatelem definované operátory, které pracují s typy hodnot neumožňující hodnotu lze použít také s možnou hodnotou Null formuláře z těchto typů.</span><span class="sxs-lookup"><span data-stu-id="dec56-371">***Lifted operators*** permit predefined and user-defined operators that operate on non-nullable value types to also be used with nullable forms of those types.</span></span> <span data-ttu-id="dec56-372">Zdvižené operátory se vytvářejí na základě předdefinovaných a uživatelem definované operátory, které splňují určité požadavky, jak je popsáno v následujících tématech:</span><span class="sxs-lookup"><span data-stu-id="dec56-372">Lifted operators are constructed from predefined and user-defined operators that meet certain requirements, as described in the following:</span></span>

*   <span data-ttu-id="dec56-373">Unárních operátorů</span><span class="sxs-lookup"><span data-stu-id="dec56-373">For the unary operators</span></span>

    ```csharp
    +  ++  -  --  !  ~
    ```

    <span data-ttu-id="dec56-374">zdvižené formulář operátoru existuje, pokud jsou typy operandů a výsledek oba typy hodnot neumožňující hodnotu.</span><span class="sxs-lookup"><span data-stu-id="dec56-374">a lifted form of an operator exists if the operand and result types are both non-nullable value types.</span></span> <span data-ttu-id="dec56-375">Zdvižené formuláře je vytvořený tak, že přidáte jediného `?` Modifikátor pro typy operandů a výsledek.</span><span class="sxs-lookup"><span data-stu-id="dec56-375">The lifted form is constructed by adding a single `?` modifier to the operand and result types.</span></span> <span data-ttu-id="dec56-376">Zdvižené operátor vytvoří hodnotu null, je-li operand hodnotu null.</span><span class="sxs-lookup"><span data-stu-id="dec56-376">The lifted operator produces a null value if the operand is null.</span></span> <span data-ttu-id="dec56-377">V opačném případě operátor zdvižené rozbalí operand, použije operátor základní a zabalí výsledek.</span><span class="sxs-lookup"><span data-stu-id="dec56-377">Otherwise, the lifted operator unwraps the operand, applies the underlying operator, and wraps the result.</span></span>

*   <span data-ttu-id="dec56-378">Pro binární operátory</span><span class="sxs-lookup"><span data-stu-id="dec56-378">For the binary operators</span></span>

    ```csharp
    +  -  *  /  %  &  |  ^  <<  >>
    ```

    <span data-ttu-id="dec56-379">zdvižené formulář operátoru existuje-li operand a výsledek typy jsou všechny typy hodnot neumožňující hodnotu.</span><span class="sxs-lookup"><span data-stu-id="dec56-379">a lifted form of an operator exists if the operand and result types are all non-nullable value types.</span></span> <span data-ttu-id="dec56-380">Zdvižené formuláře je vytvořený tak, že přidáte jediného `?` Modifikátor pro každý typ operandu a výsledek.</span><span class="sxs-lookup"><span data-stu-id="dec56-380">The lifted form is constructed by adding a single `?` modifier to each operand and result type.</span></span> <span data-ttu-id="dec56-381">Zdvižené operátor vytvoří hodnotu null, pokud jeden nebo oba operandy hodnotu null (se výjimka `&` a `|` provozovatelé `bool?` zadejte, jak je popsáno v [logické logické operátory](expressions.md#boolean-logical-operators)).</span><span class="sxs-lookup"><span data-stu-id="dec56-381">The lifted operator produces a null value if one or both operands are null (an exception being the `&` and `|` operators of the `bool?` type, as described in [Boolean logical operators](expressions.md#boolean-logical-operators)).</span></span> <span data-ttu-id="dec56-382">V opačném případě operátor zdvižené rozbalí operandy, použije operátor základní a zabalí výsledek.</span><span class="sxs-lookup"><span data-stu-id="dec56-382">Otherwise, the lifted operator unwraps the operands, applies the underlying operator, and wraps the result.</span></span>

*   <span data-ttu-id="dec56-383">Pro operátory rovnosti</span><span class="sxs-lookup"><span data-stu-id="dec56-383">For the equality operators</span></span>

    ```csharp
    ==  !=
    ```

    <span data-ttu-id="dec56-384">zdvižené formulář operátoru existuje-li typy operandů jsou typy hodnot neumožňující hodnotu a zda je typ výsledku `bool`.</span><span class="sxs-lookup"><span data-stu-id="dec56-384">a lifted form of an operator exists if the operand types are both non-nullable value types and if the result type is `bool`.</span></span> <span data-ttu-id="dec56-385">Zdvižené formuláře je vytvořený tak, že přidáte jediného `?` Modifikátor pro každý typ operandu.</span><span class="sxs-lookup"><span data-stu-id="dec56-385">The lifted form is constructed by adding a single `?` modifier to each operand type.</span></span> <span data-ttu-id="dec56-386">Operátor zdvižené bere v úvahu rovnosti dvou hodnot null a hodnota null nerovnost na libovolnou hodnotu jinou hodnotu než null.</span><span class="sxs-lookup"><span data-stu-id="dec56-386">The lifted operator considers two null values equal, and a null value unequal to any non-null value.</span></span> <span data-ttu-id="dec56-387">Pokud jsou oba operandy jinou hodnotu než null, zdvižené operátor, který se rozbalí operandy a použije základní operátoru pro vytvoření `bool` výsledek.</span><span class="sxs-lookup"><span data-stu-id="dec56-387">If both operands are non-null, the lifted operator unwraps the operands and applies the underlying operator to produce the `bool` result.</span></span>

*   <span data-ttu-id="dec56-388">Pro relační operátory</span><span class="sxs-lookup"><span data-stu-id="dec56-388">For the relational operators</span></span>

    ```csharp
    <  >  <=  >=
    ```

    <span data-ttu-id="dec56-389">zdvižené formulář operátoru existuje-li typy operandů jsou typy hodnot neumožňující hodnotu a zda je typ výsledku `bool`.</span><span class="sxs-lookup"><span data-stu-id="dec56-389">a lifted form of an operator exists if the operand types are both non-nullable value types and if the result type is `bool`.</span></span> <span data-ttu-id="dec56-390">Zdvižené formuláře je vytvořený tak, že přidáte jediného `?` Modifikátor pro každý typ operandu.</span><span class="sxs-lookup"><span data-stu-id="dec56-390">The lifted form is constructed by adding a single `?` modifier to each operand type.</span></span> <span data-ttu-id="dec56-391">Zdvižené operátor vytvoří hodnotu `false` Pokud jeden nebo oba operandy jsou null.</span><span class="sxs-lookup"><span data-stu-id="dec56-391">The lifted operator produces the value `false` if one or both operands are null.</span></span> <span data-ttu-id="dec56-392">V opačném případě operátor zdvižené rozbalí operandy a použije základní operátoru pro vytvoření `bool` výsledek.</span><span class="sxs-lookup"><span data-stu-id="dec56-392">Otherwise, the lifted operator unwraps the operands and applies the underlying operator to produce the `bool` result.</span></span>

## <a name="member-lookup"></a><span data-ttu-id="dec56-393">Člen vyhledávání</span><span class="sxs-lookup"><span data-stu-id="dec56-393">Member lookup</span></span>

<span data-ttu-id="dec56-394">Člen vyhledávání je proces, kterým se určuje podle názvu v kontextu typu.</span><span class="sxs-lookup"><span data-stu-id="dec56-394">A member lookup is the process whereby the meaning of a name in the context of a type is determined.</span></span> <span data-ttu-id="dec56-395">Člen vyhledávání může dojít v rámci vyhodnocování *simple_name* ([jednoduché názvy](expressions.md#simple-names)) nebo *member_access* ([přístup ke členu](expressions.md#member-access)) v výraz.</span><span class="sxs-lookup"><span data-stu-id="dec56-395">A member lookup can occur as part of evaluating a *simple_name* ([Simple names](expressions.md#simple-names)) or a *member_access* ([Member access](expressions.md#member-access)) in an expression.</span></span> <span data-ttu-id="dec56-396">Pokud *simple_name* nebo *member_access* vyskytuje se jako *primary_expression* ze *invocation_expression* ([ Volání metod](expressions.md#method-invocations)), člen se říká, že má být volána.</span><span class="sxs-lookup"><span data-stu-id="dec56-396">If the *simple_name* or *member_access* occurs as the *primary_expression* of an *invocation_expression* ([Method invocations](expressions.md#method-invocations)), the member is said to be invoked.</span></span>

<span data-ttu-id="dec56-397">Pokud je člen metody nebo události, nebo pokud je konstanta, pole nebo vlastnost typu delegáta ([delegáti](delegates.md)) nebo typ `dynamic` ([dynamického typu](types.md#the-dynamic-type)), pak člen je označen jako *nevyvolatelný*.</span><span class="sxs-lookup"><span data-stu-id="dec56-397">If a member is a method or event, or if it is a constant, field or property of either a delegate type ([Delegates](delegates.md)) or the type `dynamic` ([The dynamic type](types.md#the-dynamic-type)), then the member is said to be *invocable*.</span></span>

<span data-ttu-id="dec56-398">Člen vyhledávání bere v úvahu nejen názvu členem, ale také počet parametrů typu, který člen má a určuje, zda je přístupný člen.</span><span class="sxs-lookup"><span data-stu-id="dec56-398">Member lookup considers not only the name of a member but also the number of type parameters the member has and whether the member is accessible.</span></span> <span data-ttu-id="dec56-399">Pro účely vyhledávání člen obecné metody a vnořených obecných typech mají počet parametrů typu, které jsou uvedené v jejich odpovídajících deklarací a všechny ostatní členové mají nulové parametry typu.</span><span class="sxs-lookup"><span data-stu-id="dec56-399">For the purposes of member lookup, generic methods and nested generic types have the number of type parameters indicated in their respective declarations and all other members have zero type parameters.</span></span>

<span data-ttu-id="dec56-400">Člen vyhledávání názvu `N` s `K`  zadejte parametry typu `T` zpracování následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="dec56-400">A member lookup of a name `N` with `K` type parameters in a type `T` is processed as follows:</span></span>

*  <span data-ttu-id="dec56-401">První, sadu přístupní členové s názvem `N` závisí:</span><span class="sxs-lookup"><span data-stu-id="dec56-401">First, a set of accessible members named `N` is determined:</span></span>
    * <span data-ttu-id="dec56-402">Pokud `T` je parametr typu sjednocení sad přístupní členové s názvem je sada `N` v jednotlivých typů stanoveno, omezení pro primární nebo sekundární omezení ([omezení parametru typu](classes.md#type-parameter-constraints)) pro  `T`, spolu s sadu přístupní členové s názvem `N` v `object`.</span><span class="sxs-lookup"><span data-stu-id="dec56-402">If `T` is a type parameter, then the set is the union of the sets of accessible members named `N` in each of the types specified as a primary constraint or secondary constraint ([Type parameter constraints](classes.md#type-parameter-constraints)) for `T`, along with the set of accessible members named `N` in `object`.</span></span>
    * <span data-ttu-id="dec56-403">V opačném případě sada zahrnuje vše je přístupné ([přístup ke členu](basic-concepts.md#member-access)) členové s názvem `N` v `T`, včetně zděděných členů a přístupní členové s názvem `N` v `object`.</span><span class="sxs-lookup"><span data-stu-id="dec56-403">Otherwise, the set consists of all accessible ([Member access](basic-concepts.md#member-access)) members named `N` in `T`, including inherited members and the accessible members named `N` in `object`.</span></span> <span data-ttu-id="dec56-404">Pokud `T` konstruovaný typ, je získat sadu členů nahrazením argumentů typu, jak je popsáno v [členy sestavené typy](classes.md#members-of-constructed-types).</span><span class="sxs-lookup"><span data-stu-id="dec56-404">If `T` is a constructed type, the set of members is obtained by substituting type arguments as described in [Members of constructed types](classes.md#members-of-constructed-types).</span></span> <span data-ttu-id="dec56-405">Členy, které patří `override` modifikátor jsou vyloučeny ze sady.</span><span class="sxs-lookup"><span data-stu-id="dec56-405">Members that include an `override` modifier are excluded from the set.</span></span>
*  <span data-ttu-id="dec56-406">Dále, pokud `K` je nula, všechny vnořené typy deklarací, jejichž zahrnují parametry typu se odeberou.</span><span class="sxs-lookup"><span data-stu-id="dec56-406">Next, if `K` is zero, all nested types whose declarations include type parameters are removed.</span></span> <span data-ttu-id="dec56-407">Pokud `K` není nulový, všichni členové s jiným číslem typu parametry se odeberou.</span><span class="sxs-lookup"><span data-stu-id="dec56-407">If `K` is not zero, all members with a different number of type parameters are removed.</span></span> <span data-ttu-id="dec56-408">Všimněte si, že `K` je nula, metody s parametry nejsou odebrány od procesu odvození typu typ ([odvození typu](expressions.md#type-inference)) možné odvodit argumenty typu.</span><span class="sxs-lookup"><span data-stu-id="dec56-408">Note that when `K` is zero, methods having type parameters are not removed, since the type inference process ([Type inference](expressions.md#type-inference)) might be able to infer the type arguments.</span></span>
*  <span data-ttu-id="dec56-409">Další, pokud je člen *vyvolána*, to všechno bez-*nevyvolatelný* členy jsou odebrány z objektu set.</span><span class="sxs-lookup"><span data-stu-id="dec56-409">Next, if the member is *invoked*, all non-*invocable* members are removed from the set.</span></span>
*  <span data-ttu-id="dec56-410">V dalším kroku členy, které jsou skryté členy jiné se odeberou ze sady.</span><span class="sxs-lookup"><span data-stu-id="dec56-410">Next, members that are hidden by other members are removed from the set.</span></span> <span data-ttu-id="dec56-411">Pro každého člena `S.M` v sadě, kde `S` typ, ve kterém je člen `M` je teď deklarována, se použijí následující pravidla:</span><span class="sxs-lookup"><span data-stu-id="dec56-411">For every member `S.M` in the set, where `S` is the type in which the member `M` is declared, the following rules are applied:</span></span>
    * <span data-ttu-id="dec56-412">Pokud `M` je – konstanta, pole, vlastnosti, události nebo člen výčtu, pak všechny členy deklarované v základní typ `S` se odeberou ze sady.</span><span class="sxs-lookup"><span data-stu-id="dec56-412">If `M` is a constant, field, property, event, or enumeration member, then all members declared in a base type of `S` are removed from the set.</span></span>
    * <span data-ttu-id="dec56-413">Pokud `M` je deklarace typu, pak všechny jiné typy deklarovaný v základní typ `S` se odeberou ze sady, a všechny deklarace s stejný počet parametrů typu jako `M` deklarovaný v základní typ `S` jsou odebrány ze sady.</span><span class="sxs-lookup"><span data-stu-id="dec56-413">If `M` is a type declaration, then all non-types declared in a base type of `S` are removed from the set, and all type declarations with the same number of type parameters as `M` declared in a base type of `S` are removed from the set.</span></span>
    * <span data-ttu-id="dec56-414">Pokud `M` je metoda, pak všechny členy jiné metody deklarované v základní typ `S` se odeberou ze sady.</span><span class="sxs-lookup"><span data-stu-id="dec56-414">If `M` is a method, then all non-method members declared in a base type of `S` are removed from the set.</span></span>
*  <span data-ttu-id="dec56-415">V dalším kroku členy rozhraní, které jsou skryté členy třídy jsou odebrány z objektu set.</span><span class="sxs-lookup"><span data-stu-id="dec56-415">Next, interface members that are hidden by class members are removed from the set.</span></span> <span data-ttu-id="dec56-416">Tento krok má vliv pouze, pokud `T` je parametr typu a `T` i základní třída má jiné než `object` a efektivní rozhraní neprázdný nastavit ([omezení parametru typu](classes.md#type-parameter-constraints)).</span><span class="sxs-lookup"><span data-stu-id="dec56-416">This step only has an effect if `T` is a type parameter and `T` has both an effective base class other than `object` and a non-empty effective interface set ([Type parameter constraints](classes.md#type-parameter-constraints)).</span></span> <span data-ttu-id="dec56-417">Pro každého člena `S.M` v sadě, kde `S` typ, ve kterém je člen `M` je teď deklarována, pokud se použijí následující pravidla `S` deklarace třídy je jiné než `object`:</span><span class="sxs-lookup"><span data-stu-id="dec56-417">For every member `S.M` in the set, where `S` is the type in which the member `M` is declared, the following rules are applied if `S` is a class declaration other than `object`:</span></span>
    * <span data-ttu-id="dec56-418">Pokud `M` je – konstanta, pole, vlastnosti, události, člen výčtu nebo deklarace typu, pak všechny členy deklarované v deklaraci rozhraní jsou odebrány z objektu set.</span><span class="sxs-lookup"><span data-stu-id="dec56-418">If `M` is a constant, field, property, event, enumeration member, or type declaration, then all members declared in an interface declaration are removed from the set.</span></span>
    * <span data-ttu-id="dec56-419">Pokud `M` je metoda, pak všechny členy jiné metody deklarované v deklaraci rozhraní se odeberou ze sady a všech metod se stejným podpisem jako `M` deklarované v rozhraní deklarace se odeberou ze sady.</span><span class="sxs-lookup"><span data-stu-id="dec56-419">If `M` is a method, then all non-method members declared in an interface declaration are removed from the set, and all methods with the same signature as `M` declared in an interface declaration are removed from the set.</span></span>
*  <span data-ttu-id="dec56-420">Nakonec odebrání skryté členy, se určit výsledek vyhledávání:</span><span class="sxs-lookup"><span data-stu-id="dec56-420">Finally, having removed hidden members, the result of the lookup is determined:</span></span>
    * <span data-ttu-id="dec56-421">Pokud sada obsahuje jeden člen, který není metoda, je tento člen výsledků vyhledávání.</span><span class="sxs-lookup"><span data-stu-id="dec56-421">If the set consists of a single member that is not a method, then this member is the result of the lookup.</span></span>
    * <span data-ttu-id="dec56-422">Jinak, pokud sada obsahuje pouze metody, pak tato skupina metod je výsledek vyhledávání.</span><span class="sxs-lookup"><span data-stu-id="dec56-422">Otherwise, if the set contains only methods, then this group of methods is the result of the lookup.</span></span>
    * <span data-ttu-id="dec56-423">V opačném případě vyhledávání je nejednoznačný a dojde k chybě vazby čas.</span><span class="sxs-lookup"><span data-stu-id="dec56-423">Otherwise, the lookup is ambiguous, and a binding-time error occurs.</span></span>

<span data-ttu-id="dec56-424">Pro člen vyhledávání v jiné typy než typy parametrů a rozhraní a člen vyhledávání v rozhraní, která jsou výhradně jednoduchou dědičností (každé rozhraní v řetězu dědičnosti má přesně žádnou nebo jednu přímou základní rozhraní), je efekt pravidla vyhledávání jednoduše, která odvozena základních členů skrýt členy s totožným názvem a signaturou.</span><span class="sxs-lookup"><span data-stu-id="dec56-424">For member lookups in types other than type parameters and interfaces, and member lookups in interfaces that are strictly single-inheritance (each interface in the inheritance chain has exactly zero or one direct base interface), the effect of the lookup rules is simply that derived members hide base members with the same name or signature.</span></span> <span data-ttu-id="dec56-425">Tyto jednoduché dědičnosti vyhledávání nejsou nikdy nejednoznačný.</span><span class="sxs-lookup"><span data-stu-id="dec56-425">Such single-inheritance lookups are never ambiguous.</span></span> <span data-ttu-id="dec56-426">Nejednoznačnosti, které může být mohou vyplývat z vyhledávání člena v rozhraní vícenásobné dědičnosti jsou popsány v [rozhraní přístup ke členu](interfaces.md#interface-member-access).</span><span class="sxs-lookup"><span data-stu-id="dec56-426">The ambiguities that can possibly arise from member lookups in multiple-inheritance interfaces are described in [Interface member access](interfaces.md#interface-member-access).</span></span>

### <a name="base-types"></a><span data-ttu-id="dec56-427">Základní typy</span><span class="sxs-lookup"><span data-stu-id="dec56-427">Base types</span></span>

<span data-ttu-id="dec56-428">Pro účely člen vyhledávání, typ `T` se považuje za mají následující základní typy:</span><span class="sxs-lookup"><span data-stu-id="dec56-428">For purposes of member lookup, a type `T` is considered to have the following base types:</span></span>

*  <span data-ttu-id="dec56-429">Pokud `T` je `object`, pak `T` nemá žádný základní typ.</span><span class="sxs-lookup"><span data-stu-id="dec56-429">If `T` is `object`, then `T` has no base type.</span></span>
*  <span data-ttu-id="dec56-430">Pokud `T` je *enum_type*, základní typy `T` jsou typy tříd `System.Enum`, `System.ValueType`, a `object`.</span><span class="sxs-lookup"><span data-stu-id="dec56-430">If `T` is an *enum_type*, the base types of `T` are the class types `System.Enum`, `System.ValueType`, and `object`.</span></span>
*  <span data-ttu-id="dec56-431">Pokud `T` je *struct_type*, základní typy `T` jsou typy tříd `System.ValueType` a `object`.</span><span class="sxs-lookup"><span data-stu-id="dec56-431">If `T` is a *struct_type*, the base types of `T` are the class types `System.ValueType` and `object`.</span></span>
*  <span data-ttu-id="dec56-432">Pokud `T` je *class_type*, základní typy `T` jsou základní třídy `T`, včetně typu třídy `object`.</span><span class="sxs-lookup"><span data-stu-id="dec56-432">If `T` is a *class_type*, the base types of `T` are the base classes of `T`, including the class type `object`.</span></span>
*  <span data-ttu-id="dec56-433">Pokud `T` je *interface_type*, základní typy `T` jsou základní rozhraní `T` a typ třídy `object`.</span><span class="sxs-lookup"><span data-stu-id="dec56-433">If `T` is an *interface_type*, the base types of `T` are the base interfaces of `T` and the class type `object`.</span></span>
*  <span data-ttu-id="dec56-434">Pokud `T` je *array_type*, základní typy `T` jsou typy tříd `System.Array` a `object`.</span><span class="sxs-lookup"><span data-stu-id="dec56-434">If `T` is an *array_type*, the base types of `T` are the class types `System.Array` and `object`.</span></span>
*  <span data-ttu-id="dec56-435">Pokud `T` je *delegate_type*, základní typy `T` jsou typy tříd `System.Delegate` a `object`.</span><span class="sxs-lookup"><span data-stu-id="dec56-435">If `T` is a *delegate_type*, the base types of `T` are the class types `System.Delegate` and `object`.</span></span>

## <a name="function-members"></a><span data-ttu-id="dec56-436">Členové – funkce</span><span class="sxs-lookup"><span data-stu-id="dec56-436">Function members</span></span>

<span data-ttu-id="dec56-437">Funkční členy jsou členy, které obsahují spustitelné příkazy.</span><span class="sxs-lookup"><span data-stu-id="dec56-437">Function members are members that contain executable statements.</span></span> <span data-ttu-id="dec56-438">Funkční členy jsou vždy členy typů a nemůžou být členy skupiny obory názvů.</span><span class="sxs-lookup"><span data-stu-id="dec56-438">Function members are always members of types and cannot be members of namespaces.</span></span> <span data-ttu-id="dec56-439">C# definuje následující kategorie funkcí členů:</span><span class="sxs-lookup"><span data-stu-id="dec56-439">C# defines the following categories of function members:</span></span>

*  <span data-ttu-id="dec56-440">Metody</span><span class="sxs-lookup"><span data-stu-id="dec56-440">Methods</span></span>
*  <span data-ttu-id="dec56-441">Vlastnosti</span><span class="sxs-lookup"><span data-stu-id="dec56-441">Properties</span></span>
*  <span data-ttu-id="dec56-442">Události</span><span class="sxs-lookup"><span data-stu-id="dec56-442">Events</span></span>
*  <span data-ttu-id="dec56-443">Indexery</span><span class="sxs-lookup"><span data-stu-id="dec56-443">Indexers</span></span>
*  <span data-ttu-id="dec56-444">Uživatelem definované operátory</span><span class="sxs-lookup"><span data-stu-id="dec56-444">User-defined operators</span></span>
*  <span data-ttu-id="dec56-445">Konstruktory instancí</span><span class="sxs-lookup"><span data-stu-id="dec56-445">Instance constructors</span></span>
*  <span data-ttu-id="dec56-446">Statické konstruktory</span><span class="sxs-lookup"><span data-stu-id="dec56-446">Static constructors</span></span>
*  <span data-ttu-id="dec56-447">Destruktory</span><span class="sxs-lookup"><span data-stu-id="dec56-447">Destructors</span></span>

<span data-ttu-id="dec56-448">S výjimkou destruktory a statické konstruktory (což nesmí být volány explicitně) jsou spuštěny příkazy součástí členy funkce prostřednictvím volání členské funkce.</span><span class="sxs-lookup"><span data-stu-id="dec56-448">Except for destructors and static constructors (which cannot be invoked explicitly), the statements contained in function members are executed through function member invocations.</span></span> <span data-ttu-id="dec56-449">Skutečná syntaxe pro zápis volání členské funkce závisí na kategorii členu konkrétní funkce.</span><span class="sxs-lookup"><span data-stu-id="dec56-449">The actual syntax for writing a function member invocation depends on the particular function member category.</span></span>

<span data-ttu-id="dec56-450">Seznam argumentů ([seznamy argumentů](expressions.md#argument-lists)) z členské funkce volání poskytuje skutečné hodnoty nebo odkazy na proměnné parametrů členské funkce.</span><span class="sxs-lookup"><span data-stu-id="dec56-450">The argument list ([Argument lists](expressions.md#argument-lists)) of a function member invocation provides actual values or variable references for the parameters of the function member.</span></span>

<span data-ttu-id="dec56-451">Volání obecné metody mohou použít odvození typu k určení sady argumenty typu pro metodu.</span><span class="sxs-lookup"><span data-stu-id="dec56-451">Invocations of generic methods may employ type inference to determine the set of type arguments to pass to the method.</span></span> <span data-ttu-id="dec56-452">Tento proces je popsán v [odvození typu](expressions.md#type-inference).</span><span class="sxs-lookup"><span data-stu-id="dec56-452">This process is described in [Type inference](expressions.md#type-inference).</span></span>

<span data-ttu-id="dec56-453">Volání metody, indexery, operátory a konstruktory instancí využívat řešení přetížení pro určení, které Release candidate sadu členů funkce vyvolat.</span><span class="sxs-lookup"><span data-stu-id="dec56-453">Invocations of methods, indexers, operators and instance constructors employ overload resolution to determine which of a candidate set of function members to invoke.</span></span> <span data-ttu-id="dec56-454">Tento proces je popsán v [rozlišení přetěžování](expressions.md#overload-resolution).</span><span class="sxs-lookup"><span data-stu-id="dec56-454">This process is described in [Overload resolution](expressions.md#overload-resolution).</span></span>

<span data-ttu-id="dec56-455">Po chvíli vazba byla zjištěna členem určitou funkci, pravděpodobně prostřednictvím řešení přetížení volání členské funkce skutečný proces za běhu je popsáno v [kontrolu dynamické přetíženíkompilace](expressions.md#compile-time-checking-of-dynamic-overload-resolution).</span><span class="sxs-lookup"><span data-stu-id="dec56-455">Once a particular function member has been identified at binding-time, possibly through overload resolution, the actual run-time process of invoking the function member is described in [Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution).</span></span>

<span data-ttu-id="dec56-456">Následující tabulka shrnuje zpracování, které u něho v konstruktorech zahrnující šesti kategorií funkce členy, které lze explicitně vyvolat.</span><span class="sxs-lookup"><span data-stu-id="dec56-456">The following table summarizes the processing that takes place in constructs involving the six categories of function members that can be explicitly invoked.</span></span> <span data-ttu-id="dec56-457">V tabulce `e`, `x`, `y`, a `value` označení výrazy, které jsou klasifikovány jako proměnné nebo hodnoty, `T` Určuje výraz, který jsou klasifikovány jako typ, `F` je jednoduchý název metody a `P` je jednoduchý název vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="dec56-457">In the table, `e`, `x`, `y`, and `value` indicate expressions classified as variables or values, `T` indicates an expression classified as a type, `F` is the simple name of a method, and `P` is the simple name of a property.</span></span>


| <span data-ttu-id="dec56-458">__Konstrukce__</span><span class="sxs-lookup"><span data-stu-id="dec56-458">__Construct__</span></span>     | <span data-ttu-id="dec56-459">__Příklad__</span><span class="sxs-lookup"><span data-stu-id="dec56-459">__Example__</span></span>    | <span data-ttu-id="dec56-460">__Popis__</span><span class="sxs-lookup"><span data-stu-id="dec56-460">__Description__</span></span> |
|-------------------|----------------|-----------------|
| <span data-ttu-id="dec56-461">Volání metody</span><span class="sxs-lookup"><span data-stu-id="dec56-461">Method invocation</span></span> | `F(x,y)`       | <span data-ttu-id="dec56-462">Řešení přetížení se použije pro výběr nejlepší metody `F` v obsahující třídy nebo struktury.</span><span class="sxs-lookup"><span data-stu-id="dec56-462">Overload resolution is applied to select the best method `F` in the containing class or struct.</span></span> <span data-ttu-id="dec56-463">Metoda je volána s seznamu argumentů `(x,y)`.</span><span class="sxs-lookup"><span data-stu-id="dec56-463">The method is invoked with the argument list `(x,y)`.</span></span> <span data-ttu-id="dec56-464">Pokud metoda není `static`, je výraz instance `this`.</span><span class="sxs-lookup"><span data-stu-id="dec56-464">If the method is not `static`, the instance expression is `this`.</span></span> | 
|                   | `T.F(x,y)`     | <span data-ttu-id="dec56-465">Řešení přetížení se použije pro výběr nejlepší metody `F` ve třídě nebo struktuře `T`.</span><span class="sxs-lookup"><span data-stu-id="dec56-465">Overload resolution is applied to select the best method `F` in the class or struct `T`.</span></span> <span data-ttu-id="dec56-466">Chyba vazby za nastane, pokud metoda není `static`.</span><span class="sxs-lookup"><span data-stu-id="dec56-466">A binding-time error occurs if the method is not `static`.</span></span> <span data-ttu-id="dec56-467">Metoda je volána s seznamu argumentů `(x,y)`.</span><span class="sxs-lookup"><span data-stu-id="dec56-467">The method is invoked with the argument list `(x,y)`.</span></span> | 
|                   | `e.F(x,y)`     | <span data-ttu-id="dec56-468">Řešení přetížení se použije k výběru nejvhodnější způsob F v třídy, struktury nebo rozhraní uvedena v každém typu `e`.</span><span class="sxs-lookup"><span data-stu-id="dec56-468">Overload resolution is applied to select the best method F in the class, struct, or interface given by the type of `e`.</span></span> <span data-ttu-id="dec56-469">Pokud je metoda, dojde k chybě doba vazby `static`.</span><span class="sxs-lookup"><span data-stu-id="dec56-469">A binding-time error occurs if the method is `static`.</span></span> <span data-ttu-id="dec56-470">Metoda je volána s výraz instance `e` a seznam argumentů `(x,y)`.</span><span class="sxs-lookup"><span data-stu-id="dec56-470">The method is invoked with the instance expression `e` and the argument list `(x,y)`.</span></span> | 
| <span data-ttu-id="dec56-471">Přístup k vlastnosti</span><span class="sxs-lookup"><span data-stu-id="dec56-471">Property access</span></span>   | `P`            | <span data-ttu-id="dec56-472">`get` Přistupující objekt vlastnosti `P` v obsahující třídy nebo struktury je vyvolána.</span><span class="sxs-lookup"><span data-stu-id="dec56-472">The `get` accessor of the property `P` in the containing class or struct is invoked.</span></span> <span data-ttu-id="dec56-473">Pokud dojde k chybě kompilace `P` je jen pro zápis.</span><span class="sxs-lookup"><span data-stu-id="dec56-473">A compile-time error occurs if `P` is write-only.</span></span> <span data-ttu-id="dec56-474">Pokud `P` není `static`, je výraz instance `this`.</span><span class="sxs-lookup"><span data-stu-id="dec56-474">If `P` is not `static`, the instance expression is `this`.</span></span> | 
|                   | `P = value`    | <span data-ttu-id="dec56-475">`set` Přistupující objekt vlastnosti `P` v obsahující třídy nebo struktury je vyvolán pomocí seznamu argumentů `(value)`.</span><span class="sxs-lookup"><span data-stu-id="dec56-475">The `set` accessor of the property `P` in the containing class or struct is invoked with the argument list `(value)`.</span></span> <span data-ttu-id="dec56-476">Pokud dojde k chybě kompilace `P` je jen pro čtení.</span><span class="sxs-lookup"><span data-stu-id="dec56-476">A compile-time error occurs if `P` is read-only.</span></span> <span data-ttu-id="dec56-477">Pokud `P` není `static`, je výraz instance `this`.</span><span class="sxs-lookup"><span data-stu-id="dec56-477">If `P` is not `static`, the instance expression is `this`.</span></span> | 
|                   | `T.P`          | <span data-ttu-id="dec56-478">`get` Přistupující objekt vlastnosti `P` ve třídě nebo struktuře `T` je vyvolána.</span><span class="sxs-lookup"><span data-stu-id="dec56-478">The `get` accessor of the property `P` in the class or struct `T` is invoked.</span></span> <span data-ttu-id="dec56-479">Pokud dojde k chybě kompilace `P` není `static` nebo, pokud `P` je jen pro zápis.</span><span class="sxs-lookup"><span data-stu-id="dec56-479">A compile-time error occurs if `P` is not `static` or if `P` is write-only.</span></span> | 
|                   | `T.P = value`  | <span data-ttu-id="dec56-480">`set` Přistupující objekt vlastnosti `P` ve třídě nebo struktuře `T` vyvolání seznamu argumentů `(value)`.</span><span class="sxs-lookup"><span data-stu-id="dec56-480">The `set` accessor of the property `P` in the class or struct `T` is invoked with the argument list `(value)`.</span></span> <span data-ttu-id="dec56-481">Pokud dojde k chybě kompilace `P` není `static` nebo, pokud `P` je jen pro čtení.</span><span class="sxs-lookup"><span data-stu-id="dec56-481">A compile-time error occurs if `P` is not `static` or if `P` is read-only.</span></span> | 
|                   | `e.P`          | <span data-ttu-id="dec56-482">`get` Přistupující objekt vlastnosti `P` v třídy, struktury nebo rozhraní uvedena v každém typu `e` vyvolání výraz instance `e`.</span><span class="sxs-lookup"><span data-stu-id="dec56-482">The `get` accessor of the property `P` in the class, struct, or interface given by the type of `e` is invoked with the instance expression `e`.</span></span> <span data-ttu-id="dec56-483">Pokud dojde k chybě vazby čas `P` je `static` nebo, pokud `P` je jen pro zápis.</span><span class="sxs-lookup"><span data-stu-id="dec56-483">A binding-time error occurs if `P` is `static` or if `P` is write-only.</span></span> | 
|                   | `e.P = value`  | <span data-ttu-id="dec56-484">`set` Přistupující objekt vlastnosti `P` v třídy, struktury nebo rozhraní uvedena v každém typu `e` vyvolání výraz instance `e` a seznam argumentů `(value)`.</span><span class="sxs-lookup"><span data-stu-id="dec56-484">The `set` accessor of the property `P` in the class, struct, or interface given by the type of `e` is invoked with the instance expression `e` and the argument list `(value)`.</span></span> <span data-ttu-id="dec56-485">Pokud dojde k chybě vazby čas `P` je `static` nebo, pokud `P` je jen pro čtení.</span><span class="sxs-lookup"><span data-stu-id="dec56-485">A binding-time error occurs if `P` is `static` or if `P` is read-only.</span></span> | 
| <span data-ttu-id="dec56-486">Přístup k události</span><span class="sxs-lookup"><span data-stu-id="dec56-486">Event access</span></span>      | `E += value`   | <span data-ttu-id="dec56-487">`add` Přístupového objektu události `E` v obsahující třídy nebo struktury je vyvolána.</span><span class="sxs-lookup"><span data-stu-id="dec56-487">The `add` accessor of the event `E` in the containing class or struct is invoked.</span></span> <span data-ttu-id="dec56-488">Pokud `E` není statická, je výraz instance `this`.</span><span class="sxs-lookup"><span data-stu-id="dec56-488">If `E` is not static, the instance expression is `this`.</span></span> | 
|                   | `E -= value`   | <span data-ttu-id="dec56-489">`remove` Přístupového objektu události `E` v obsahující třídy nebo struktury je vyvolána.</span><span class="sxs-lookup"><span data-stu-id="dec56-489">The `remove` accessor of the event `E` in the containing class or struct is invoked.</span></span> <span data-ttu-id="dec56-490">Pokud `E` není statická, je výraz instance `this`.</span><span class="sxs-lookup"><span data-stu-id="dec56-490">If `E` is not static, the instance expression is `this`.</span></span> | 
|                   | `T.E += value` | <span data-ttu-id="dec56-491">`add` Přístupového objektu události `E` ve třídě nebo struktuře `T` je vyvolána.</span><span class="sxs-lookup"><span data-stu-id="dec56-491">The `add` accessor of the event `E` in the class or struct `T` is invoked.</span></span> <span data-ttu-id="dec56-492">Pokud dojde k chybě vazby čas `E` není statická.</span><span class="sxs-lookup"><span data-stu-id="dec56-492">A binding-time error occurs if `E` is not static.</span></span> | 
|                   | `T.E -= value` | <span data-ttu-id="dec56-493">`remove` Přístupového objektu události `E` ve třídě nebo struktuře `T` je vyvolána.</span><span class="sxs-lookup"><span data-stu-id="dec56-493">The `remove` accessor of the event `E` in the class or struct `T` is invoked.</span></span> <span data-ttu-id="dec56-494">Pokud dojde k chybě vazby čas `E` není statická.</span><span class="sxs-lookup"><span data-stu-id="dec56-494">A binding-time error occurs if `E` is not static.</span></span> | 
|                   | `e.E += value` | <span data-ttu-id="dec56-495">`add` Přístupového objektu události `E` v třídy, struktury nebo rozhraní uvedena v každém typu `e` vyvolání výraz instance `e`.</span><span class="sxs-lookup"><span data-stu-id="dec56-495">The `add` accessor of the event `E` in the class, struct, or interface given by the type of `e` is invoked with the instance expression `e`.</span></span> <span data-ttu-id="dec56-496">Pokud dojde k chybě vazby čas `E` je statická.</span><span class="sxs-lookup"><span data-stu-id="dec56-496">A binding-time error occurs if `E` is static.</span></span> | 
|                   | `e.E -= value` | <span data-ttu-id="dec56-497">`remove` Přístupového objektu události `E` v třídy, struktury nebo rozhraní uvedena v každém typu `e` vyvolání výraz instance `e`.</span><span class="sxs-lookup"><span data-stu-id="dec56-497">The `remove` accessor of the event `E` in the class, struct, or interface given by the type of `e` is invoked with the instance expression `e`.</span></span> <span data-ttu-id="dec56-498">Pokud dojde k chybě vazby čas `E` je statická.</span><span class="sxs-lookup"><span data-stu-id="dec56-498">A binding-time error occurs if `E` is static.</span></span> | 
| <span data-ttu-id="dec56-499">Přístup indexeru</span><span class="sxs-lookup"><span data-stu-id="dec56-499">Indexer access</span></span>    | `e[x,y]`       | <span data-ttu-id="dec56-500">Řešení přetížení se použije k výběru nejvhodnější indexer v třídy, struktury nebo rozhraní uvedena v každém typu e.</span><span class="sxs-lookup"><span data-stu-id="dec56-500">Overload resolution is applied to select the best indexer in the class, struct, or interface given by the type of e.</span></span> <span data-ttu-id="dec56-501">`get` Vyvolání přistupující objekt indexer s výrazem instance `e` a seznam argumentů `(x,y)`.</span><span class="sxs-lookup"><span data-stu-id="dec56-501">The `get` accessor of the indexer is invoked with the instance expression `e` and the argument list `(x,y)`.</span></span> <span data-ttu-id="dec56-502">Pokud indexeru je jen pro zápis, dojde k chybě vazby – za.</span><span class="sxs-lookup"><span data-stu-id="dec56-502">A binding-time error occurs if the indexer is write-only.</span></span> | 
|                   | `e[x,y] = value` | <span data-ttu-id="dec56-503">Řešení přetížení se použije k výběru nejvhodnější indexer v třídy, struktury nebo rozhraní uvedena v každém typu `e`.</span><span class="sxs-lookup"><span data-stu-id="dec56-503">Overload resolution is applied to select the best indexer in the class, struct, or interface given by the type of `e`.</span></span> <span data-ttu-id="dec56-504">`set` Vyvolání přistupující objekt indexer s výrazem instance `e` a seznam argumentů `(x,y,value)`.</span><span class="sxs-lookup"><span data-stu-id="dec56-504">The `set` accessor of the indexer is invoked with the instance expression `e` and the argument list `(x,y,value)`.</span></span> <span data-ttu-id="dec56-505">Pokud indexeru je jen pro čtení, dojde k chybě vazby – za.</span><span class="sxs-lookup"><span data-stu-id="dec56-505">A binding-time error occurs if the indexer is read-only.</span></span> | 
| <span data-ttu-id="dec56-506">Operátor vyvolání</span><span class="sxs-lookup"><span data-stu-id="dec56-506">Operator invocation</span></span> | `-x`         | <span data-ttu-id="dec56-507">Řešení přetížení se použije k výběru nejvhodnější unární operátor v dané třídy nebo struktury uvedena v každém typu `x`.</span><span class="sxs-lookup"><span data-stu-id="dec56-507">Overload resolution is applied to select the best unary operator in the class or struct given by the type of `x`.</span></span> <span data-ttu-id="dec56-508">Vybraný operátor vyvolání seznamu argumentů `(x)`.</span><span class="sxs-lookup"><span data-stu-id="dec56-508">The selected operator is invoked with the argument list `(x)`.</span></span> | 
|                     | `x + y`      | <span data-ttu-id="dec56-509">Řešení přetížení se použije k výběru nejvhodnější binární operátor v třídy nebo struktury, které jsou uvedeny typy `x` a `y`.</span><span class="sxs-lookup"><span data-stu-id="dec56-509">Overload resolution is applied to select the best binary operator in the classes or structs given by the types of `x` and `y`.</span></span> <span data-ttu-id="dec56-510">Vybraný operátor vyvolání seznamu argumentů `(x,y)`.</span><span class="sxs-lookup"><span data-stu-id="dec56-510">The selected operator is invoked with the argument list `(x,y)`.</span></span> | 
| <span data-ttu-id="dec56-511">Vyvolání konstruktoru instance</span><span class="sxs-lookup"><span data-stu-id="dec56-511">Instance constructor invocation</span></span> | `new T(x,y)` | <span data-ttu-id="dec56-512">Řešení přetížení se použije k výběru nejvhodnější konstruktor instance ve třídě nebo struktuře `T`.</span><span class="sxs-lookup"><span data-stu-id="dec56-512">Overload resolution is applied to select the best instance constructor in the class or struct `T`.</span></span> <span data-ttu-id="dec56-513">Vyvolání konstruktoru instance seznamu argumentů `(x,y)`.</span><span class="sxs-lookup"><span data-stu-id="dec56-513">The instance constructor is invoked with the argument list `(x,y)`.</span></span> | 

### <a name="argument-lists"></a><span data-ttu-id="dec56-514">Seznamy argumentů</span><span class="sxs-lookup"><span data-stu-id="dec56-514">Argument lists</span></span>

<span data-ttu-id="dec56-515">Každý člen a delegáta volání funkce obsahuje seznam argumentů, které poskytuje skutečné hodnoty nebo odkazy na proměnné parametrů členské funkce.</span><span class="sxs-lookup"><span data-stu-id="dec56-515">Every function member and delegate invocation includes an argument list which provides actual values or variable references for the parameters of the function member.</span></span> <span data-ttu-id="dec56-516">Syntaxe pro zadání seznamu argumentů volání členské funkce závisí na kategorii členu funkce:</span><span class="sxs-lookup"><span data-stu-id="dec56-516">The syntax for specifying the argument list of a function member invocation depends on the function member category:</span></span>

*  <span data-ttu-id="dec56-517">Pro instanci konstruktorů, metod, indexerů a delegátů, argumenty jsou zadané jako *argument_list*, jak je popsáno níže.</span><span class="sxs-lookup"><span data-stu-id="dec56-517">For instance constructors, methods, indexers and delegates, the arguments are specified as an *argument_list*, as described below.</span></span> <span data-ttu-id="dec56-518">Pro indexery, při vyvolání `set` přístupový objekt, v seznamu argumentů navíc obsahuje výraz zadaný jako pravý operand operátoru přiřazení.</span><span class="sxs-lookup"><span data-stu-id="dec56-518">For indexers, when invoking the `set` accessor, the argument list additionally includes the expression specified as the right operand of the assignment operator.</span></span>
*  <span data-ttu-id="dec56-519">Pro vlastnosti, je prázdný seznam argumentů, při vyvolání `get` přístupový objekt a skládá se z výraz zadaný jako pravý operand operátoru přiřazení při vyvolání `set` přistupujícího objektu.</span><span class="sxs-lookup"><span data-stu-id="dec56-519">For properties, the argument list is empty when invoking the `get` accessor, and consists of the expression specified as the right operand of the assignment operator when invoking the `set` accessor.</span></span>
*  <span data-ttu-id="dec56-520">Pro události, seznam argumentů se skládá z výraz zadaný jako pravý operand `+=` nebo `-=` operátor.</span><span class="sxs-lookup"><span data-stu-id="dec56-520">For events, the argument list consists of the expression specified as the right operand of the `+=` or `-=` operator.</span></span>
*  <span data-ttu-id="dec56-521">Pro uživatelsky definované operátory seznamu argumentů se skládá z jediného operandu unární operátor nebo dva operandy binární operátor.</span><span class="sxs-lookup"><span data-stu-id="dec56-521">For user-defined operators, the argument list consists of the single operand of the unary operator or the two operands of the binary operator.</span></span>

<span data-ttu-id="dec56-522">Argumenty vlastnosti ([vlastnosti](classes.md#properties)), události ([události](classes.md#events)) a uživatelsky definované operátory ([operátory](classes.md#operators)) jsou vždy předány jako parametry s hodnotou ([ Hodnoty parametrů](classes.md#value-parameters)).</span><span class="sxs-lookup"><span data-stu-id="dec56-522">The arguments of properties ([Properties](classes.md#properties)), events ([Events](classes.md#events)), and user-defined operators ([Operators](classes.md#operators)) are always passed as value parameters ([Value parameters](classes.md#value-parameters)).</span></span> <span data-ttu-id="dec56-523">Argumenty indexery ([indexery](classes.md#indexers)) jsou vždy předány jako parametry s hodnotou ([hodnoty parametrů](classes.md#value-parameters)) nebo pole parametrů ([pole parametrů](classes.md#parameter-arrays)).</span><span class="sxs-lookup"><span data-stu-id="dec56-523">The arguments of indexers ([Indexers](classes.md#indexers)) are always passed as value parameters ([Value parameters](classes.md#value-parameters)) or parameter arrays ([Parameter arrays](classes.md#parameter-arrays)).</span></span> <span data-ttu-id="dec56-524">Parametry odkazu a výstup nejsou podporovány pro tyto kategorie funkce členů.</span><span class="sxs-lookup"><span data-stu-id="dec56-524">Reference and output parameters are not supported for these categories of function members.</span></span>

<span data-ttu-id="dec56-525">Argumenty vyvolání konstruktoru, metoda, indexer nebo delegáta instance jsou zadány jako *argument_list*:</span><span class="sxs-lookup"><span data-stu-id="dec56-525">The arguments of an instance constructor, method, indexer or delegate invocation are specified as an *argument_list*:</span></span>

```antlr
argument_list
    : argument (',' argument)*
    ;

argument
    : argument_name? argument_value
    ;

argument_name
    : identifier ':'
    ;

argument_value
    : expression
    | 'ref' variable_reference
    | 'out' variable_reference
    ;
```

<span data-ttu-id="dec56-526">*Argument_list* obsahuje jeden nebo více *argument*s, oddělené čárkami.</span><span class="sxs-lookup"><span data-stu-id="dec56-526">An *argument_list* consists of one or more *argument*s, separated by commas.</span></span> <span data-ttu-id="dec56-527">Každý argument se skládá z volitelné *argument_name* následovaný *argument_value*.</span><span class="sxs-lookup"><span data-stu-id="dec56-527">Each argument consists of an optional  *argument_name* followed by an *argument_value*.</span></span> <span data-ttu-id="dec56-528">*Argument* s *argument_name* se označuje jako ***pojmenovaný argument***, že *argument* bez  *argument_name* je ***poziční argument***.</span><span class="sxs-lookup"><span data-stu-id="dec56-528">An *argument* with an *argument_name* is referred to as a ***named argument***, whereas an *argument* without an *argument_name* is a ***positional argument***.</span></span> <span data-ttu-id="dec56-529">Jedná se o chybu pro poziční argument, který se zobrazí za pojmenovaným argumentem v *argument_list*.</span><span class="sxs-lookup"><span data-stu-id="dec56-529">It is an error for a positional argument to appear after a named argument in an *argument_list*.</span></span>

<span data-ttu-id="dec56-530">*Argument_value* můžete provést jednu z následujících forem:</span><span class="sxs-lookup"><span data-stu-id="dec56-530">The *argument_value* can take one of the following forms:</span></span>

*  <span data-ttu-id="dec56-531">*Výraz*, což indikuje, že argument je předán jako parametr hodnoty ([hodnoty parametrů](classes.md#value-parameters)).</span><span class="sxs-lookup"><span data-stu-id="dec56-531">An *expression*, indicating that the argument is passed as a value parameter ([Value parameters](classes.md#value-parameters)).</span></span>
*  <span data-ttu-id="dec56-532">Klíčové slovo `ref` následovaný *variable_reference* ([odkazy na proměnné](variables.md#variable-references)), což indikuje, že argument je předán jako parametr odkazu ([odkazovat na parametry ](classes.md#reference-parameters)).</span><span class="sxs-lookup"><span data-stu-id="dec56-532">The keyword `ref` followed by a *variable_reference* ([Variable references](variables.md#variable-references)), indicating that the argument is passed as a reference parameter ([Reference parameters](classes.md#reference-parameters)).</span></span> <span data-ttu-id="dec56-533">Proměnná musí být jednoznačně přiřazena ([jednoznačného přiřazení](variables.md#definite-assignment)) předtím, než může být předán jako parametr odkazu.</span><span class="sxs-lookup"><span data-stu-id="dec56-533">A variable must be definitely assigned ([Definite assignment](variables.md#definite-assignment)) before it can be passed as a reference parameter.</span></span> <span data-ttu-id="dec56-534">Klíčové slovo `out` následovaný *variable_reference* ([odkazy na proměnné](variables.md#variable-references)), což indikuje, že argument je předán jako výstupní parametr ([výstupních parametrů](classes.md#output-parameters)).</span><span class="sxs-lookup"><span data-stu-id="dec56-534">The keyword `out` followed by a *variable_reference* ([Variable references](variables.md#variable-references)), indicating that the argument is passed as an output parameter ([Output parameters](classes.md#output-parameters)).</span></span> <span data-ttu-id="dec56-535">Proměnná je považován za jednoznačně přiřazené ([jednoznačného přiřazení](variables.md#definite-assignment)) po volání členské funkce ve které se předá proměnná jako výstupní parametr.</span><span class="sxs-lookup"><span data-stu-id="dec56-535">A variable is considered definitely assigned ([Definite assignment](variables.md#definite-assignment)) following a function member invocation in which the variable is passed as an output parameter.</span></span>

#### <a name="corresponding-parameters"></a><span data-ttu-id="dec56-536">Odpovídající parametry</span><span class="sxs-lookup"><span data-stu-id="dec56-536">Corresponding parameters</span></span>

<span data-ttu-id="dec56-537">Pro každý argument v seznamu argumentů musí existovat odpovídající parametr v členské funkci nebo vyvolání delegáta.</span><span class="sxs-lookup"><span data-stu-id="dec56-537">For each argument in an argument list there has to be a corresponding parameter in the function member or delegate being invoked.</span></span>

<span data-ttu-id="dec56-538">Seznam parametrů používaných tímto se určuje následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="dec56-538">The parameter list used in the following is determined as follows:</span></span>

*  <span data-ttu-id="dec56-539">Pro virtuální metody a indexery definované ve třídách seznam parametrů se vybere od nejkonkrétnější deklarace nebo přepsat z členské funkce, počínaje statický typ příjemce a prohledávat jejích základních tříd.</span><span class="sxs-lookup"><span data-stu-id="dec56-539">For virtual methods and indexers defined in classes, the parameter list is picked from the most specific declaration or override of the function member, starting with the static type of the receiver, and searching through its base classes.</span></span>
*  <span data-ttu-id="dec56-540">Pro metody rozhraní a indexery, seznam parametrů výběru formuláře nejspecifičtější definice člena, počínaje typ rozhraní a prohledávat základních rozhraní.</span><span class="sxs-lookup"><span data-stu-id="dec56-540">For interface methods and indexers, the parameter list is picked form the most specific definition of the member, starting with the interface type and searching through the base interfaces.</span></span> <span data-ttu-id="dec56-541">Pokud seznam jedinečných parametrů nenajde, je vytvořen seznam parametrů s nepřístupný názvy a žádné volitelné parametry, tak, aby volání nelze použít pojmenované parametry nebo vynechejte volitelné argumenty.</span><span class="sxs-lookup"><span data-stu-id="dec56-541">If no unique parameter list is found, a parameter list with inaccessible names and no optional parameters is constructed, so that invocations cannot use named parameters or omit optional arguments.</span></span>
*  <span data-ttu-id="dec56-542">Pro částečné metody se používá parametr seznam definující deklarace částečné metody.</span><span class="sxs-lookup"><span data-stu-id="dec56-542">For partial methods, the parameter list of the defining partial method declaration is used.</span></span>
*  <span data-ttu-id="dec56-543">Pro všechny ostatní funkce členy a delegáti je pouze jeden seznam parametrů, které se bude používat.</span><span class="sxs-lookup"><span data-stu-id="dec56-543">For all other function members and delegates there is only a single parameter list, which is the one used.</span></span>

<span data-ttu-id="dec56-544">Pozice argumentu nebo parametr se definuje jako počet argumentů nebo parametry předcházející v seznam argumentů nebo seznamu parametrů.</span><span class="sxs-lookup"><span data-stu-id="dec56-544">The position of an argument or parameter is defined as the number of arguments or parameters preceding it in the argument list or parameter list.</span></span>

<span data-ttu-id="dec56-545">Odpovídající parametry pro argumenty pro členské funkce jsou vytvořeny následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="dec56-545">The corresponding parameters for function member arguments are established as follows:</span></span>

*  <span data-ttu-id="dec56-546">Argumenty v *argument_list* konstruktory instancí, metod, indexerů a delegátů:</span><span class="sxs-lookup"><span data-stu-id="dec56-546">Arguments in the *argument_list* of instance constructors, methods, indexers and delegates:</span></span>
    * <span data-ttu-id="dec56-547">Poziční argument, kde dochází k dlouhodobého parametr na stejné pozici v seznamu parametrů odpovídá tomuto parametru.</span><span class="sxs-lookup"><span data-stu-id="dec56-547">A positional argument where a fixed parameter occurs at the same position in the parameter list corresponds to that parameter.</span></span>
    * <span data-ttu-id="dec56-548">Poziční argument členské funkce s polem parametrů vyvolání v podobě normální odpovídá pole parametrů, které se musí vyskytovat na stejné pozici v seznamu parametrů.</span><span class="sxs-lookup"><span data-stu-id="dec56-548">A positional argument of a function member with a parameter array invoked in its normal form corresponds to the parameter  array, which must occur at the same position in the parameter list.</span></span>
    * <span data-ttu-id="dec56-549">Poziční argument členské funkce s polem parametrů vyvolání v podobě rozšířené žádná pevná parametr kde dojde k na stejné pozici v seznamu parametrů, odpovídá na prvek v poli parametrů.</span><span class="sxs-lookup"><span data-stu-id="dec56-549">A positional argument of a function member with a parameter array invoked in its expanded form, where no fixed parameter occurs at the same position in the parameter list, corresponds to an element in the parameter array.</span></span>
    * <span data-ttu-id="dec56-550">Pojmenovaný argument odpovídající parametru se stejným názvem v seznamu parametrů.</span><span class="sxs-lookup"><span data-stu-id="dec56-550">A named argument corresponds to the parameter of the same name in the parameter list.</span></span>
    * <span data-ttu-id="dec56-551">Pro indexery, při vyvolání `set` přístupový objekt, výraz zadaný jako pravý operand operátoru přiřazení odpovídá implicitní `value` parametr `set` deklarace přistupujícího objektu.</span><span class="sxs-lookup"><span data-stu-id="dec56-551">For indexers, when invoking the `set` accessor, the expression specified as the right operand of the assignment operator corresponds to the implicit `value` parameter of the `set` accessor declaration.</span></span>
*  <span data-ttu-id="dec56-552">Pro vlastnosti, při vyvolání `get` existuje přístupový objekt se žádné argumenty.</span><span class="sxs-lookup"><span data-stu-id="dec56-552">For properties, when invoking the `get` accessor there are no arguments.</span></span> <span data-ttu-id="dec56-553">Při vyvolání `set` přístupový objekt, výraz zadaný jako pravý operand operátoru přiřazení odpovídá implicitní `value` parametr `set` deklarace přistupujícího objektu.</span><span class="sxs-lookup"><span data-stu-id="dec56-553">When invoking the `set` accessor, the expression specified as the right operand of the assignment operator corresponds to the implicit `value` parameter of the `set` accessor declaration.</span></span>
*  <span data-ttu-id="dec56-554">Uživatelem definované unárních operátorů (včetně převodů) jeden operand odpovídá jeden parametr deklarace operátoru.</span><span class="sxs-lookup"><span data-stu-id="dec56-554">For user-defined unary operators (including conversions), the single operand corresponds to the single parameter of the operator declaration.</span></span>
*  <span data-ttu-id="dec56-555">Pro binární operátory definované uživatelem levý operand odpovídá na první parametr a pravý operand odpovídá druhý parametr deklarace operátoru.</span><span class="sxs-lookup"><span data-stu-id="dec56-555">For user-defined binary operators, the left operand corresponds to the first parameter, and the right operand corresponds to the second parameter of the operator declaration.</span></span>

#### <a name="run-time-evaluation-of-argument-lists"></a><span data-ttu-id="dec56-556">Vyhodnocení seznamů argumentů</span><span class="sxs-lookup"><span data-stu-id="dec56-556">Run-time evaluation of argument lists</span></span>

<span data-ttu-id="dec56-557">Při zpracování volání členské funkce za běhu ([kompilace kontrolu dynamické přetížení](expressions.md#compile-time-checking-of-dynamic-overload-resolution)), výrazy nebo odkazy na proměnné seznamu argumentů se vyhodnocují v pořadí zleva doprava, jako následující:</span><span class="sxs-lookup"><span data-stu-id="dec56-557">During the run-time processing of a function member invocation ([Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution)), the expressions or variable references of an argument list are evaluated in order, from left to right, as follows:</span></span>

*  <span data-ttu-id="dec56-558">Hodnota parametru, je vyhodnocen výraz argumentu a implicitní převod ([implicitních převodů](conversions.md#implicit-conversions)) do odpovídajícího parametru typu provést.</span><span class="sxs-lookup"><span data-stu-id="dec56-558">For a value parameter, the argument expression is evaluated and an implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)) to the corresponding parameter type is performed.</span></span> <span data-ttu-id="dec56-559">Výsledná hodnota bude počáteční hodnota parametru hodnotu ve volání funkce člena.</span><span class="sxs-lookup"><span data-stu-id="dec56-559">The resulting value becomes the initial value of the value parameter in the function member invocation.</span></span>
*  <span data-ttu-id="dec56-560">Pro parametr odkaz nebo výstup reference proměnné je vyhodnocena a výsledný umístění úložiště, změní umístění úložiště, který je reprezentován parametrem ve volání funkce člena.</span><span class="sxs-lookup"><span data-stu-id="dec56-560">For a reference or output parameter, the variable reference is evaluated and the resulting storage location becomes the storage location represented by the parameter in the function member invocation.</span></span> <span data-ttu-id="dec56-561">Pokud odkaz na proměnnou zadána jako parametr odkaz nebo výstup je prvek pole *reference_type*, kontrolu za běhu se provádí za účelem zajištění, že typ prvku pole je stejný jako typ parametru.</span><span class="sxs-lookup"><span data-stu-id="dec56-561">If the variable reference given as a reference or output parameter is an array element of a *reference_type*, a run-time check is performed to ensure that the element type of the array is identical to the type of the parameter.</span></span> <span data-ttu-id="dec56-562">Pokud tato kontrola neúspěšná, `System.ArrayTypeMismatchException` je vyvolána výjimka.</span><span class="sxs-lookup"><span data-stu-id="dec56-562">If this check fails, a `System.ArrayTypeMismatchException` is thrown.</span></span>

<span data-ttu-id="dec56-563">Metody, indexery a konstruktory instancí může deklarovat jejich krajní pravý parametr bude pole parametrů ([pole parametrů](classes.md#parameter-arrays)).</span><span class="sxs-lookup"><span data-stu-id="dec56-563">Methods, indexers, and instance constructors may declare their right-most parameter to be a parameter array ([Parameter arrays](classes.md#parameter-arrays)).</span></span> <span data-ttu-id="dec56-564">Tyto funkce členy jsou vyvolány v jejich normálním formuláře nebo v jejich rozšířené podobě, v závislosti na které se vztahuje ([použít funkční člen](expressions.md#applicable-function-member)):</span><span class="sxs-lookup"><span data-stu-id="dec56-564">Such function members are invoked either in their normal form or in their expanded form depending on which is applicable ([Applicable function member](expressions.md#applicable-function-member)):</span></span>

*  <span data-ttu-id="dec56-565">Po vyvolání členské funkce s polem parametrů v podobě normální argument pro pole parametrů musí být jediný výraz, který je implicitně převést ([implicitních převodů](conversions.md#implicit-conversions)) na typ pole parametrů.</span><span class="sxs-lookup"><span data-stu-id="dec56-565">When a function member with a parameter array is invoked in its normal form, the argument given for the parameter array must be a single expression that is implicitly convertible ([Implicit conversions](conversions.md#implicit-conversions)) to the parameter array type.</span></span> <span data-ttu-id="dec56-566">V tomto případě pole parametrů funguje přesně jako hodnotu parametru.</span><span class="sxs-lookup"><span data-stu-id="dec56-566">In this case, the parameter array acts precisely like a value parameter.</span></span>
*  <span data-ttu-id="dec56-567">Po vyvolání členské funkce s polem parametrů v podobě rozšířené vyvolání musíte zadat nuly nebo více poziční argumenty pro pole parametrů, kde každý argument je výraz, který je implicitně převést ([implicitní převody](conversions.md#implicit-conversions)) na typ elementu pole parametrů.</span><span class="sxs-lookup"><span data-stu-id="dec56-567">When a function member with a parameter array is invoked in its expanded form, the invocation must specify zero or more positional arguments for the parameter array, where each argument is an expression that is implicitly convertible ([Implicit conversions](conversions.md#implicit-conversions)) to the element type of the parameter array.</span></span> <span data-ttu-id="dec56-568">V takovém případě vyvolání vytvoří instanci typu parametru pole s délkou odpovídající počet argumentů, inicializuje prvky instance pole s hodnotami daný argument a používá instanci nově vytvořené pole jako skutečný argument.</span><span class="sxs-lookup"><span data-stu-id="dec56-568">In this case, the invocation creates an instance of the parameter array type with a length corresponding to the number of arguments, initializes the elements of the array instance with the given argument values, and uses the newly created array instance as the actual argument.</span></span>

<span data-ttu-id="dec56-569">Výrazy seznam argumentů jsou vždy vyhodnoceny v pořadí, ve kterém jsou zapsány.</span><span class="sxs-lookup"><span data-stu-id="dec56-569">The expressions of an argument list are always evaluated in the order they are written.</span></span> <span data-ttu-id="dec56-570">Díky tomu se v příkladu</span><span class="sxs-lookup"><span data-stu-id="dec56-570">Thus, the example</span></span>
```csharp
class Test
{
    static void F(int x, int y = -1, int z = -2) {
        System.Console.WriteLine("x = {0}, y = {1}, z = {2}", x, y, z);
    }

    static void Main() {
        int i = 0;
        F(i++, i++, i++);
        F(z: i++, x: i++);
    }
}
```
<span data-ttu-id="dec56-571">Vytvoří výstup</span><span class="sxs-lookup"><span data-stu-id="dec56-571">produces the output</span></span>
```
x = 0, y = 1, z = 2
x = 4, y = -1, z = 3
```

<span data-ttu-id="dec56-572">Pravidla pole společné odchylky ([kovariance polí](arrays.md#array-covariance)) povolit hodnotu typu pole `A[]` byl odkaz na instanci typu pole `B[]`, pokud existuje implicitní referenční převod z `B` do `A`.</span><span class="sxs-lookup"><span data-stu-id="dec56-572">The array co-variance rules ([Array covariance](arrays.md#array-covariance)) permit a value of an array type `A[]` to be a reference to an instance of an array type `B[]`, provided an implicit reference conversion exists from `B` to `A`.</span></span> <span data-ttu-id="dec56-573">Z důvodu tato pravidla, pokud prvek pole *reference_type* je předán jako parametr odkaz nebo výstup, je potřeba zajistit, že typ skutečným prvkem pole je stejná jako parametru kontrolu za běhu.</span><span class="sxs-lookup"><span data-stu-id="dec56-573">Because of these rules, when an array element of a *reference_type* is passed as a reference or output parameter, a run-time check is required to ensure that the actual element type of the array is identical to that of the parameter.</span></span> <span data-ttu-id="dec56-574">V příkladu</span><span class="sxs-lookup"><span data-stu-id="dec56-574">In the example</span></span>
```csharp
class Test
{
    static void F(ref object x) {...}

    static void Main() {
        object[] a = new object[10];
        object[] b = new string[10];
        F(ref a[0]);        // Ok
        F(ref b[1]);        // ArrayTypeMismatchException
    }
}
```
<span data-ttu-id="dec56-575">druhé volání `F` způsobí, že `System.ArrayTypeMismatchException` vyvolání, protože typ skutečným prvkem `b` je `string` a ne `object`.</span><span class="sxs-lookup"><span data-stu-id="dec56-575">the second invocation of `F` causes a `System.ArrayTypeMismatchException` to be thrown because the actual element type of `b` is `string` and not `object`.</span></span>

<span data-ttu-id="dec56-576">Při vyvolání členské funkce s polem parametrů v podobě rozšířené, volání se zpracovává stejně, jako kdyby výraz vytvoření pole s inicializátorem pole ([pole vytváření výrazů](expressions.md#array-creation-expressions)) byl vložen kolem rozšířené parametry.</span><span class="sxs-lookup"><span data-stu-id="dec56-576">When a function member with a parameter array is invoked in its expanded form, the invocation is processed exactly as if an array creation expression with an array initializer ([Array creation expressions](expressions.md#array-creation-expressions)) was inserted around the expanded parameters.</span></span> <span data-ttu-id="dec56-577">Mějme například deklarace</span><span class="sxs-lookup"><span data-stu-id="dec56-577">For example, given the declaration</span></span>
```csharp
void F(int x, int y, params object[] args);
```
<span data-ttu-id="dec56-578">Následující volání formuláři Rozšířené metody</span><span class="sxs-lookup"><span data-stu-id="dec56-578">the following invocations of the expanded form of the method</span></span>
```csharp
F(10, 20);
F(10, 20, 30, 40);
F(10, 20, 1, "hello", 3.0);
```
<span data-ttu-id="dec56-579">přesně odpovídají</span><span class="sxs-lookup"><span data-stu-id="dec56-579">correspond exactly to</span></span>
```csharp
F(10, 20, new object[] {});
F(10, 20, new object[] {30, 40});
F(10, 20, new object[] {1, "hello", 3.0});
```

<span data-ttu-id="dec56-580">Všimněte si zejména, pokud nejsou žádné argumenty pro pole parametrů se vytvoří prázdné pole.</span><span class="sxs-lookup"><span data-stu-id="dec56-580">In particular, note that an empty array is created when there are zero arguments given for the parameter array.</span></span>

<span data-ttu-id="dec56-581">Pokud argumenty jsou vynechány z členské funkce s odpovídající volitelné parametry, výchozí argumenty deklarace členské funkce jsou implicitně předány.</span><span class="sxs-lookup"><span data-stu-id="dec56-581">When arguments are omitted from a function member with corresponding optional parameters, the default arguments of the function member declaration are implicitly passed.</span></span> <span data-ttu-id="dec56-582">Protože jsou vždy konstantní, jejich vyhodnocení neovlivní pořadí vyhodnocování zbývajících argumentů.</span><span class="sxs-lookup"><span data-stu-id="dec56-582">Because these are always constant, their evaluation will not impact the evaluation order of the remaining arguments.</span></span>

### <a name="type-inference"></a><span data-ttu-id="dec56-583">Odvození typu</span><span class="sxs-lookup"><span data-stu-id="dec56-583">Type inference</span></span>

<span data-ttu-id="dec56-584">Při volání obecné metody bez zadání argumentů, ***odvození typu*** proces pokusí odvodit typ argumenty pro volání.</span><span class="sxs-lookup"><span data-stu-id="dec56-584">When a generic method is called without specifying type arguments, a ***type inference*** process attempts to infer type arguments for the call.</span></span> <span data-ttu-id="dec56-585">Přítomnost odvození typu umožňuje pohodlnější syntaxi pro volání obecné metody a umožňuje programátorovi, aby se vyhnout zadávání informací o typu redundantní.</span><span class="sxs-lookup"><span data-stu-id="dec56-585">The presence of type inference allows a more convenient syntax to be used for calling a generic method, and allows the programmer to avoid specifying redundant type information.</span></span> <span data-ttu-id="dec56-586">Mějme například deklarace metody:</span><span class="sxs-lookup"><span data-stu-id="dec56-586">For example, given the method declaration:</span></span>
```csharp
class Chooser
{
    static Random rand = new Random();

    public static T Choose<T>(T first, T second) {
        return (rand.Next(2) == 0)? first: second;
    }
}
```
<span data-ttu-id="dec56-587">je možné vyvolat `Choose` metody bez explicitního určení argumentu typu:</span><span class="sxs-lookup"><span data-stu-id="dec56-587">it is possible to invoke the `Choose` method without explicitly specifying a type argument:</span></span>
```csharp
int i = Chooser.Choose(5, 213);                 // Calls Choose<int>

string s = Chooser.Choose("foo", "bar");        // Calls Choose<string>
```

<span data-ttu-id="dec56-588">Prostřednictvím odvození typu proměnné, argumenty typu `int` a `string` se stanoví z argumentů metody.</span><span class="sxs-lookup"><span data-stu-id="dec56-588">Through type inference, the type arguments `int` and `string` are determined from the arguments to the method.</span></span>

<span data-ttu-id="dec56-589">Dojde k odvození typu při zpracování časově vazbu volání metody ([volání metod](expressions.md#method-invocations)) a probíhá před krokem rozlišení přetížení volání.</span><span class="sxs-lookup"><span data-stu-id="dec56-589">Type inference occurs as part of the binding-time processing of a method invocation ([Method invocations](expressions.md#method-invocations)) and takes place before the overload resolution step of the invocation.</span></span> <span data-ttu-id="dec56-590">Když konkrétní metody skupiny je zadaný ve volání metody a jako součást volání metody nejsou zadány žádné argumenty typu, odvození typu platí pro každý obecnou metodu skupinu metod.</span><span class="sxs-lookup"><span data-stu-id="dec56-590">When a particular method group is specified in a method invocation, and no type arguments are specified as part of the method invocation, type inference is applied to each generic method in the method group.</span></span> <span data-ttu-id="dec56-591">Pokud bude úspěšné odvození typu proměnné, argumenty typu slouží k určení typů argumentů pro následné přetížení.</span><span class="sxs-lookup"><span data-stu-id="dec56-591">If type inference succeeds, then the inferred type arguments are used to determine the types of arguments for subsequent overload resolution.</span></span> <span data-ttu-id="dec56-592">Pokud řešení přetížení vybere obecné metody jako ta, která se má vyvolat, argumenty typu slouží jako skutečný typ argumenty pro vyvolání.</span><span class="sxs-lookup"><span data-stu-id="dec56-592">If overload resolution chooses a generic method as the one to invoke, then the inferred type arguments are used as the actual type arguments for the invocation.</span></span> <span data-ttu-id="dec56-593">Pokud se nepodaří odvození typu proměnné pro konkrétní metody, této metodě není součástí řešení přetížení.</span><span class="sxs-lookup"><span data-stu-id="dec56-593">If type inference for a particular method fails, that method does not participate in overload resolution.</span></span> <span data-ttu-id="dec56-594">Selhání odvození typu, v a sám sebe, nezpůsobí chybu v době vazby.</span><span class="sxs-lookup"><span data-stu-id="dec56-594">The failure of type inference, in and of itself, does not cause a binding-time error.</span></span> <span data-ttu-id="dec56-595">Však to často vede k chybu v době vazby při řešení přetížení pak nepodaří najít žádné použitelné metody.</span><span class="sxs-lookup"><span data-stu-id="dec56-595">However, it often leads to a binding-time error when overload resolution then fails to find any applicable methods.</span></span>

<span data-ttu-id="dec56-596">Pokud zadaný počet argumentů, které se liší od počtu parametrů v metodě, pak okamžitě odvození.</span><span class="sxs-lookup"><span data-stu-id="dec56-596">If the supplied number of arguments is different than the number of parameters in the method, then inference immediately fails.</span></span> <span data-ttu-id="dec56-597">V opačném případě Předpokládejme, že obecné metody obsahuje následující podpis:</span><span class="sxs-lookup"><span data-stu-id="dec56-597">Otherwise, assume that the generic method has the following signature:</span></span>
```csharp
Tr M<X1,...,Xn>(T1 x1, ..., Tm xm)
```

<span data-ttu-id="dec56-598">Pomocí volání metody formuláře `M(E1...Em)` úlohou odvození typu je najít jedinečnou argumentů `S1...Sn` pro každý z parametrů typu `X1...Xn` tak, aby volání `M<S1...Sn>(E1...Em)` vstupuje v platnost.</span><span class="sxs-lookup"><span data-stu-id="dec56-598">With a method call of the form `M(E1...Em)` the task of type inference is to find unique type arguments `S1...Sn` for each of the type parameters `X1...Xn` so that the call `M<S1...Sn>(E1...Em)` becomes valid.</span></span>

<span data-ttu-id="dec56-599">Během procesu odvození každý parametr typu `Xi` je buď *oprava* na určitý typ `Si` nebo *nevyřešené* s přidruženou sadu *hranice*.</span><span class="sxs-lookup"><span data-stu-id="dec56-599">During the process of inference each type parameter `Xi` is either *fixed* to a particular type `Si` or *unfixed* with an associated set of *bounds*.</span></span> <span data-ttu-id="dec56-600">Každá hranice je nějaký typ `T`.</span><span class="sxs-lookup"><span data-stu-id="dec56-600">Each of the bounds is some type `T`.</span></span> <span data-ttu-id="dec56-601">Zpočátku každý typ proměnné `Xi` je laťky s prázdnou sadou mezí.</span><span class="sxs-lookup"><span data-stu-id="dec56-601">Initially each type variable `Xi` is unfixed with an empty set of bounds.</span></span>

<span data-ttu-id="dec56-602">Odvození typu proměnné se provádí ve fázích.</span><span class="sxs-lookup"><span data-stu-id="dec56-602">Type inference takes place in phases.</span></span> <span data-ttu-id="dec56-603">Jednotlivé fáze se pokusí odvodit argumenty typu pro další proměnné typu závislosti na zjištěních předchozí fáze.</span><span class="sxs-lookup"><span data-stu-id="dec56-603">Each phase will try to infer type arguments for more type variables based on the findings of the previous phase.</span></span> <span data-ttu-id="dec56-604">První fáze provede některé počáteční závěry mezí, zatímco druhá fáze oprav typ proměnné na konkrétní typy a odvodí z nich další hranice.</span><span class="sxs-lookup"><span data-stu-id="dec56-604">The first phase makes some initial inferences of bounds, whereas the second phase fixes type variables to specific types and infers further bounds.</span></span> <span data-ttu-id="dec56-605">Druhá fáze může být třeba několikrát opakuje.</span><span class="sxs-lookup"><span data-stu-id="dec56-605">The second phase may have to be repeated a number of times.</span></span>

<span data-ttu-id="dec56-606">*Poznámka:* Typ odvození probíhá nejen při volání obecné metody.</span><span class="sxs-lookup"><span data-stu-id="dec56-606">*Note:* Type inference takes place not only when a generic method is called.</span></span> <span data-ttu-id="dec56-607">Odvození typu pro převod skupin metoda je popsaná v [odvození pro převod skupin metoda typu](expressions.md#type-inference-for-conversion-of-method-groups) a najít nejlepší společný typ sada výrazů je popsána v [hledání nejlepší společný typ sady výrazů](expressions.md#finding-the-best-common-type-of-a-set-of-expressions).</span><span class="sxs-lookup"><span data-stu-id="dec56-607">Type inference for conversion of method groups is described in [Type inference for conversion of method groups](expressions.md#type-inference-for-conversion-of-method-groups) and finding the best common type of a set of expressions is described in [Finding the best common type of a set of expressions](expressions.md#finding-the-best-common-type-of-a-set-of-expressions).</span></span>

#### <a name="the-first-phase"></a><span data-ttu-id="dec56-608">První fáze</span><span class="sxs-lookup"><span data-stu-id="dec56-608">The first phase</span></span>

<span data-ttu-id="dec56-609">Pro každou argumentů metody `Ei`:</span><span class="sxs-lookup"><span data-stu-id="dec56-609">For each of the method arguments `Ei`:</span></span>

*   <span data-ttu-id="dec56-610">Pokud `Ei` je anonymní funkce *odvození typu explicitní parametr* ([explicitní parametr typu závěry](expressions.md#explicit-parameter-type-inferences)) se provádí z `Ei` do `Ti`</span><span class="sxs-lookup"><span data-stu-id="dec56-610">If `Ei` is an anonymous function, an *explicit parameter type inference* ([Explicit parameter type inferences](expressions.md#explicit-parameter-type-inferences)) is made from `Ei` to `Ti`</span></span>
*   <span data-ttu-id="dec56-611">Jinak, pokud `Ei` má typ `U` a `xi` je hodnota parametru o *dolní mez odvození* tvoří *z* `U` *k* `Ti`.</span><span class="sxs-lookup"><span data-stu-id="dec56-611">Otherwise, if `Ei` has a type `U` and `xi` is a value parameter then a *lower-bound inference* is made *from* `U` *to* `Ti`.</span></span>
*   <span data-ttu-id="dec56-612">Jinak, pokud `Ei` má typ `U` a `xi` je `ref` nebo `out` parametr pak *přesná odvození* tvoří *z* `U` *k* `Ti`.</span><span class="sxs-lookup"><span data-stu-id="dec56-612">Otherwise, if `Ei` has a type `U` and `xi` is a `ref` or `out` parameter then an *exact inference* is made *from* `U` *to* `Ti`.</span></span>
*   <span data-ttu-id="dec56-613">V opačném případě se provádí bez odvození pro tento argument.</span><span class="sxs-lookup"><span data-stu-id="dec56-613">Otherwise, no inference is made for this argument.</span></span>


#### <a name="the-second-phase"></a><span data-ttu-id="dec56-614">Druhá fáze</span><span class="sxs-lookup"><span data-stu-id="dec56-614">The second phase</span></span>

<span data-ttu-id="dec56-615">Druhá fáze probíhá následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="dec56-615">The second phase proceeds as follows:</span></span>

*   <span data-ttu-id="dec56-616">Všechny *nevyřešené* typ proměnné `Xi` které nikoli *závisí na* ([závislost](expressions.md#dependence)) jakékoli `Xj` jsou pevné ([řešení](expressions.md#fixing)).</span><span class="sxs-lookup"><span data-stu-id="dec56-616">All *unfixed* type variables `Xi` which do not *depend on* ([Dependence](expressions.md#dependence)) any `Xj` are fixed ([Fixing](expressions.md#fixing)).</span></span>
*   <span data-ttu-id="dec56-617">Pokud neexistuje žádný takový typ proměnné, všechny *nevyřešené* typ proměnné `Xi` jsou *oprava* pro které všechny tyto uchování:</span><span class="sxs-lookup"><span data-stu-id="dec56-617">If no such type variables exist, all *unfixed* type variables `Xi` are *fixed* for which all of the following hold:</span></span>
    *   <span data-ttu-id="dec56-618">Existuje alespoň jednu proměnnou typu `Xj` , který závisí na `Xi`</span><span class="sxs-lookup"><span data-stu-id="dec56-618">There is at least one type variable `Xj` that depends on `Xi`</span></span>
    *   <span data-ttu-id="dec56-619">`Xi` má jiné prázdnou sadu mezí</span><span class="sxs-lookup"><span data-stu-id="dec56-619">`Xi` has a non-empty set of bounds</span></span>
*   <span data-ttu-id="dec56-620">Pokud neexistuje žádný takový typ proměnné a stále dochází k *nevyřešené* typ proměnné, typu odvození selže.</span><span class="sxs-lookup"><span data-stu-id="dec56-620">If no such type variables exist and there are still *unfixed* type variables, type inference fails.</span></span>
*   <span data-ttu-id="dec56-621">Jinak, pokud žádné další *nevyřešené* proměnné typu neexistuje, proběhne úspěšně odvození typu proměnné.</span><span class="sxs-lookup"><span data-stu-id="dec56-621">Otherwise, if no further *unfixed* type variables exist, type inference succeeds.</span></span>
*   <span data-ttu-id="dec56-622">Jinak pro všechny argumenty `Ei` s odpovídající typ parametru `Ti` kde *výstupní typy* ([výstupní typy](expressions.md#output-types)) obsahují *nevyřešené* Zadejte proměnné `Xj` ale *typů vstupu* ([typů vstupu](expressions.md#input-types)) nepodporují, *výstup odvození typu* ([závěry typ výstupu ](expressions.md#output-type-inferences)) se provádí *z* `Ei` *k* `Ti`.</span><span class="sxs-lookup"><span data-stu-id="dec56-622">Otherwise, for all arguments `Ei` with corresponding parameter type `Ti` where the *output types* ([Output types](expressions.md#output-types)) contain *unfixed* type variables `Xj` but the *input types* ([Input types](expressions.md#input-types)) do not, an *output type inference* ([Output type inferences](expressions.md#output-type-inferences)) is made *from* `Ei` *to* `Ti`.</span></span> <span data-ttu-id="dec56-623">Druhá fáze se pak opakuje.</span><span class="sxs-lookup"><span data-stu-id="dec56-623">Then the second phase is repeated.</span></span>

#### <a name="input-types"></a><span data-ttu-id="dec56-624">Vstupní typy.</span><span class="sxs-lookup"><span data-stu-id="dec56-624">Input types</span></span>

<span data-ttu-id="dec56-625">Pokud `E` je skupinu metod nebo implicitně typované anonymní funkce a `T` je delegát typu nebo typu stromu výrazu a všechny typy parametrů `T` jsou *typů vstupu* z `E` *s typem* `T`.</span><span class="sxs-lookup"><span data-stu-id="dec56-625">If `E` is a method group or implicitly typed anonymous function and `T` is a delegate type or expression tree type then all the parameter types of `T` are *input types* of `E` *with type* `T`.</span></span>

####  <a name="output-types"></a><span data-ttu-id="dec56-626">Výstupní typy</span><span class="sxs-lookup"><span data-stu-id="dec56-626">Output types</span></span>

<span data-ttu-id="dec56-627">Pokud `E` skupinu metod nebo anonymní funkce a `T` typu nebo typu stromu výrazu pak návratový typ je delegát `T` je *výstupní typ* `E` *s typem*  `T`.</span><span class="sxs-lookup"><span data-stu-id="dec56-627">If `E` is a method group or an anonymous function and `T` is a delegate type or expression tree type then the return type of `T` is an *output type of* `E` *with type* `T`.</span></span>

#### <a name="dependence"></a><span data-ttu-id="dec56-628">Závislost</span><span class="sxs-lookup"><span data-stu-id="dec56-628">Dependence</span></span>

<span data-ttu-id="dec56-629">*Nevyřešené* proměnná typu `Xi` *závisí přímo na* proměnnou typu laťky `Xj` po dobu některé argument `Ek` s typem `Tk` `Xj` Vyvolá se v *typ vstupu* z `Ek` s typem `Tk` a `Xi` probíhá *typ výstupu* z `Ek` s typem `Tk`.</span><span class="sxs-lookup"><span data-stu-id="dec56-629">An *unfixed* type variable `Xi` *depends directly on* an unfixed type variable `Xj` if for some argument `Ek` with type `Tk` `Xj` occurs in an *input type* of `Ek` with type `Tk` and `Xi` occurs in an *output type* of `Ek` with type `Tk`.</span></span>

<span data-ttu-id="dec56-630">`Xj` *závisí na* `Xi` Pokud `Xj` *závisí přímo na* `Xi` nebo, pokud `Xi` *závisí přímo na* `Xk` a `Xk` *závisí na* `Xj`.</span><span class="sxs-lookup"><span data-stu-id="dec56-630">`Xj` *depends on* `Xi` if `Xj` *depends directly on* `Xi` or if `Xi` *depends directly on* `Xk` and `Xk` *depends on* `Xj`.</span></span> <span data-ttu-id="dec56-631">Tedy "závisí na" přechodné, ale ne reflexivní uzavření "závisí přímo na".</span><span class="sxs-lookup"><span data-stu-id="dec56-631">Thus "depends on" is the transitive but not reflexive closure of "depends directly on".</span></span>

#### <a name="output-type-inferences"></a><span data-ttu-id="dec56-632">Výstupní typ závěry</span><span class="sxs-lookup"><span data-stu-id="dec56-632">Output type inferences</span></span>

<span data-ttu-id="dec56-633">*Výstup odvození typu* tvoří *z* výraz `E` *k* typu `T` následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="dec56-633">An *output type inference* is made *from* an expression `E` *to* a type `T` in the following way:</span></span>

*  <span data-ttu-id="dec56-634">Pokud `E` je anonymní funkce s odvozený návratový typ `U` ([Inferred návratový typ](expressions.md#inferred-return-type)) a `T` není typ delegáta nebo typu stromu výrazu s návratovým typem `Tb`, pak *dolní mez odvození* ([dolní mez závěry](expressions.md#lower-bound-inferences)) se provádí *z* `U` *k* `Tb`.</span><span class="sxs-lookup"><span data-stu-id="dec56-634">If `E` is an anonymous function with inferred return type  `U` ([Inferred return type](expressions.md#inferred-return-type)) and `T` is a delegate type or expression tree type with return type `Tb`, then a *lower-bound inference* ([Lower-bound inferences](expressions.md#lower-bound-inferences)) is made *from* `U` *to* `Tb`.</span></span>
*  <span data-ttu-id="dec56-635">Jinak, pokud `E` je skupinu metod a `T` není typ delegáta nebo typu stromu výrazu se typy parametrů `T1...Tk` a návratový typ `Tb`a rozlišení přetížení `E` s typy `T1...Tk` dává Jediná metoda s návratovým typem `U`, o *dolní mez odvození* tvoří *z* `U` *k* `Tb`.</span><span class="sxs-lookup"><span data-stu-id="dec56-635">Otherwise, if `E` is a method group and `T` is a delegate type or expression tree type with parameter types `T1...Tk` and return type `Tb`, and overload resolution of `E` with the types `T1...Tk` yields a single method with return type `U`, then a *lower-bound inference* is made *from* `U` *to* `Tb`.</span></span>
*  <span data-ttu-id="dec56-636">Jinak, pokud `E` je výraz s typem `U`, o *dolní mez odvození* tvoří *z* `U` *k* `T`.</span><span class="sxs-lookup"><span data-stu-id="dec56-636">Otherwise, if `E` is an expression with type `U`, then a *lower-bound inference* is made *from* `U` *to* `T`.</span></span>
*  <span data-ttu-id="dec56-637">V opačném případě nebudou provedeny žádné závěry.</span><span class="sxs-lookup"><span data-stu-id="dec56-637">Otherwise, no inferences are made.</span></span>

#### <a name="explicit-parameter-type-inferences"></a><span data-ttu-id="dec56-638">Explicitní parametr typu závěry</span><span class="sxs-lookup"><span data-stu-id="dec56-638">Explicit parameter type inferences</span></span>

<span data-ttu-id="dec56-639">*Odvození typu explicitní parametr* tvoří *z* výraz `E` *k* typu `T` následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="dec56-639">An *explicit parameter type inference* is made *from* an expression `E` *to* a type `T` in the following way:</span></span>

*  <span data-ttu-id="dec56-640">Pokud `E` je explicitně anonymní funkce se typy parametrů `U1...Uk` a `T` není typ delegáta nebo typu stromu výrazu se typy parametrů `V1...Vk` pak pro každou `Ui` *přesné odvození* ([přesná závěry](expressions.md#exact-inferences)) se provádí *z* `Ui` *k* odpovídající `Vi`.</span><span class="sxs-lookup"><span data-stu-id="dec56-640">If `E` is an explicitly typed anonymous function with parameter types `U1...Uk` and `T` is a delegate type or expression tree type with parameter types `V1...Vk` then for each `Ui` an *exact inference* ([Exact inferences](expressions.md#exact-inferences)) is made *from* `Ui` *to* the corresponding `Vi`.</span></span>

#### <a name="exact-inferences"></a><span data-ttu-id="dec56-641">Přesné závěry</span><span class="sxs-lookup"><span data-stu-id="dec56-641">Exact inferences</span></span>

<span data-ttu-id="dec56-642">*Přesná odvození* *z* typu `U` *k* typu `V` se provádí následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="dec56-642">An *exact inference* *from* a type `U` *to* a type `V` is made as follows:</span></span>

*  <span data-ttu-id="dec56-643">Pokud `V` je jedním z *nevyřešené* `Xi` pak `U` je přidána do sady přesné mezí pro `Xi`.</span><span class="sxs-lookup"><span data-stu-id="dec56-643">If `V` is one of the *unfixed* `Xi` then `U` is added to the set of exact bounds for `Xi`.</span></span>

*  <span data-ttu-id="dec56-644">Jinak, nastaví `V1...Vk` a `U1...Uk` určuje kontrolu, pokud platí kterákoli z následujících případech:</span><span class="sxs-lookup"><span data-stu-id="dec56-644">Otherwise, sets `V1...Vk` and `U1...Uk` are determined by checking if any of the following cases apply:</span></span>

   *  <span data-ttu-id="dec56-645">`V` je typ pole `V1[...]` a `U` je typem pole `U1[...]` stejného řádu</span><span class="sxs-lookup"><span data-stu-id="dec56-645">`V` is an array type `V1[...]` and `U` is an array type `U1[...]`  of the same rank</span></span>
   *  <span data-ttu-id="dec56-646">`V` je typ `V1?` a `U` je typ `U1?`</span><span class="sxs-lookup"><span data-stu-id="dec56-646">`V` is the type `V1?` and `U` is the type `U1?`</span></span>
   *  <span data-ttu-id="dec56-647">`V` je konstruovaný typ `C<V1...Vk>`a `U` je konstruovaný typ. `C<U1...Uk>`</span><span class="sxs-lookup"><span data-stu-id="dec56-647">`V` is a constructed type `C<V1...Vk>`and `U` is a constructed type `C<U1...Uk>`</span></span>

   <span data-ttu-id="dec56-648">Pokud pak platí kterákoli z těchto případů *přesná odvození* tvoří *z* každý `Ui` *k* odpovídající `Vi`.</span><span class="sxs-lookup"><span data-stu-id="dec56-648">If any of these cases apply then an *exact inference* is made *from* each `Ui` *to* the corresponding `Vi`.</span></span>

*  <span data-ttu-id="dec56-649">V opačném případě nebudou provedeny žádné závěry.</span><span class="sxs-lookup"><span data-stu-id="dec56-649">Otherwise no inferences are made.</span></span>

#### <a name="lower-bound-inferences"></a><span data-ttu-id="dec56-650">Dolní mez závěry</span><span class="sxs-lookup"><span data-stu-id="dec56-650">Lower-bound inferences</span></span>

<span data-ttu-id="dec56-651">A *dolní mez odvození* *z* typu `U` *k* typu `V` se provádí následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="dec56-651">A *lower-bound inference* *from* a type `U` *to* a type `V` is made as follows:</span></span>

*  <span data-ttu-id="dec56-652">Pokud `V` je jedním z *nevyřešené* `Xi` pak `U` je přidat do sady dolní meze `Xi`.</span><span class="sxs-lookup"><span data-stu-id="dec56-652">If `V` is one of the *unfixed* `Xi` then `U` is added to the set of lower bounds for `Xi`.</span></span>
*  <span data-ttu-id="dec56-653">Jinak, pokud `V` je typ `V1?`a `U` je typ `U1?` pak dolní mez odvození se provádí z `U1` k `V1`.</span><span class="sxs-lookup"><span data-stu-id="dec56-653">Otherwise, if `V` is the type `V1?`and `U` is the type `U1?` then a lower bound inference is made from `U1` to `V1`.</span></span>
*  <span data-ttu-id="dec56-654">Jinak, nastaví `U1...Uk` a `V1...Vk` určuje kontrolu, pokud platí kterákoli z následujících případech:</span><span class="sxs-lookup"><span data-stu-id="dec56-654">Otherwise, sets `U1...Uk` and `V1...Vk` are determined by checking if any of the following cases apply:</span></span>
   *  <span data-ttu-id="dec56-655">`V` je typ pole `V1[...]` a `U` je typem pole `U1[...]` (nebo parametr typu, jehož efektivní základní typ je `U1[...]`) ze stejné pořadí</span><span class="sxs-lookup"><span data-stu-id="dec56-655">`V` is an array type `V1[...]` and `U` is an array type `U1[...]` (or a type parameter whose effective base type is `U1[...]`) of the same rank</span></span>
   *  <span data-ttu-id="dec56-656">`V` je jedním z `IEnumerable<V1>`, `ICollection<V1>` nebo `IList<V1>` a `U` je jednorozměrné pole typu `U1[]`(nebo parametr typu, jehož efektivní základní typ je `U1[]`)</span><span class="sxs-lookup"><span data-stu-id="dec56-656">`V` is one of `IEnumerable<V1>`, `ICollection<V1>` or `IList<V1>` and `U` is a one-dimensional array type `U1[]`(or a type parameter whose effective base type is `U1[]`)</span></span>
   *  <span data-ttu-id="dec56-657">`V` je konstruovaný typ třídy, struktury, rozhraní nebo delegát `C<V1...Vk>` a je jedinečný typ `C<U1...Uk>` tak, aby `U` (nebo pokud `U` je parametr typu, její základní třída nebo členem její sada efektivní rozhraní) je stejné jako, dědí z (přímo nebo nepřímo), nebo implementuje (přímo nebo nepřímo) `C<U1...Uk>`.</span><span class="sxs-lookup"><span data-stu-id="dec56-657">`V` is a constructed class, struct, interface or delegate type `C<V1...Vk>` and there is a unique type `C<U1...Uk>` such that `U` (or, if `U` is a type parameter, its effective base class or any member of its effective interface set) is identical to, inherits from (directly or indirectly), or implements (directly or indirectly) `C<U1...Uk>`.</span></span>

      <span data-ttu-id="dec56-658">(Omezení "jedinečnost" znamená, že v případě rozhraní `C<T> {} class U: C<X>, C<Y> {}`, pak žádný odvození, provádí při odvozování z `U` k `C<T>` protože `U1` může být `X` nebo `Y`.)</span><span class="sxs-lookup"><span data-stu-id="dec56-658">(The "uniqueness" restriction means that in the case interface `C<T> {} class U: C<X>, C<Y> {}`, then no inference is made when inferring from `U` to `C<T>` because `U1` could be `X` or `Y`.)</span></span>

   <span data-ttu-id="dec56-659">Pokud platí kterákoli z těchto případů, pak se provádí odvození *z* každý `Ui` *k* odpovídající `Vi` následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="dec56-659">If any of these cases apply then an inference is made *from* each `Ui` *to* the corresponding `Vi` as follows:</span></span>

   *  <span data-ttu-id="dec56-660">Pokud `Ui` není známa jako typ odkazu a *přesná odvození* tvoří</span><span class="sxs-lookup"><span data-stu-id="dec56-660">If `Ui` is not known to be a reference type then an *exact inference* is made</span></span>
   *  <span data-ttu-id="dec56-661">Jinak, pokud `U` je typem pole o *dolní mez odvození* provedení</span><span class="sxs-lookup"><span data-stu-id="dec56-661">Otherwise, if `U` is an array type then a *lower-bound inference* is made</span></span>
   *  <span data-ttu-id="dec56-662">Jinak, pokud `V` je `C<V1...Vk>` pak odvození závisí na parametr typu i tý `C`:</span><span class="sxs-lookup"><span data-stu-id="dec56-662">Otherwise, if `V` is `C<V1...Vk>` then inference depends on the i-th type parameter of `C`:</span></span>
      *  <span data-ttu-id="dec56-663">Pokud se jedná se o kovariantní o *dolní mez odvození* tvoří.</span><span class="sxs-lookup"><span data-stu-id="dec56-663">If it is covariant then a *lower-bound inference* is made.</span></span>
      *  <span data-ttu-id="dec56-664">Pokud je kontravariantní pak *horní mez odvození* tvoří.</span><span class="sxs-lookup"><span data-stu-id="dec56-664">If it is contravariant then an *upper-bound inference* is made.</span></span>
      *  <span data-ttu-id="dec56-665">Pokud je invariantní pak *přesná odvození* tvoří.</span><span class="sxs-lookup"><span data-stu-id="dec56-665">If it is invariant then an *exact inference* is made.</span></span>
*  <span data-ttu-id="dec56-666">V opačném případě nebudou provedeny žádné závěry.</span><span class="sxs-lookup"><span data-stu-id="dec56-666">Otherwise, no inferences are made.</span></span>

#### <a name="upper-bound-inferences"></a><span data-ttu-id="dec56-667">Horní mez závěry</span><span class="sxs-lookup"><span data-stu-id="dec56-667">Upper-bound inferences</span></span>

<span data-ttu-id="dec56-668">*Horní mez odvození* *z* typu `U` *k* typu `V` se provádí následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="dec56-668">An *upper-bound inference* *from* a type `U` *to* a type `V` is made as follows:</span></span>

*  <span data-ttu-id="dec56-669">Pokud `V` je jedním z *nevyřešené* `Xi` pak `U` je přidat do sady horní mez pro `Xi`.</span><span class="sxs-lookup"><span data-stu-id="dec56-669">If `V` is one of the *unfixed* `Xi` then `U` is added to the set of upper bounds for `Xi`.</span></span>
*  <span data-ttu-id="dec56-670">Jinak, nastaví `V1...Vk` a `U1...Uk` určuje kontrolu, pokud platí kterákoli z následujících případech:</span><span class="sxs-lookup"><span data-stu-id="dec56-670">Otherwise, sets `V1...Vk` and `U1...Uk` are determined by checking if any of the following cases apply:</span></span>
   *  <span data-ttu-id="dec56-671">`U` je typ pole `U1[...]` a `V` je typem pole `V1[...]` stejného řádu</span><span class="sxs-lookup"><span data-stu-id="dec56-671">`U` is an array type `U1[...]` and `V` is an array type `V1[...]` of the same rank</span></span>
   *  <span data-ttu-id="dec56-672">`U` je jedním z `IEnumerable<Ue>`, `ICollection<Ue>` nebo `IList<Ue>` a `V` je jednorozměrné pole typu `Ve[]`</span><span class="sxs-lookup"><span data-stu-id="dec56-672">`U` is one of `IEnumerable<Ue>`, `ICollection<Ue>` or `IList<Ue>` and `V` is a one-dimensional array type `Ve[]`</span></span>
   *  <span data-ttu-id="dec56-673">`U` je typ `U1?` a `V` je typ `V1?`</span><span class="sxs-lookup"><span data-stu-id="dec56-673">`U` is the type `U1?` and `V` is the type `V1?`</span></span>
   *  <span data-ttu-id="dec56-674">`U` je vytvořený třídy, struktury, rozhraní nebo delegát typu `C<U1...Uk>` a `V` (přímo nebo nepřímo) je třída, struktura, rozhraní nebo delegát typu, který se shoduje s dědí z (přímo nebo nepřímo) nebo implementuje jedinečný typ `C<V1...Vk>`</span><span class="sxs-lookup"><span data-stu-id="dec56-674">`U` is constructed class, struct, interface or delegate type `C<U1...Uk>` and `V` is a class, struct, interface or delegate type which is identical to, inherits from (directly or indirectly), or implements (directly or indirectly) a unique type `C<V1...Vk>`</span></span>

      <span data-ttu-id="dec56-675">(Omezení "jedinečnost" znamená, že pokud budeme mít `interface C<T>{} class V<Z>: C<X<Z>>, C<Y<Z>>{}`, pak žádný odvození, provádí při odvozování z `C<U1>` k `V<Q>`.</span><span class="sxs-lookup"><span data-stu-id="dec56-675">(The "uniqueness" restriction means that if we have `interface C<T>{} class V<Z>: C<X<Z>>, C<Y<Z>>{}`, then no inference is made when inferring from `C<U1>` to `V<Q>`.</span></span> <span data-ttu-id="dec56-676">Závěry nejsou provedeny z `U1` buď `X<Q>` nebo `Y<Q>`.)</span><span class="sxs-lookup"><span data-stu-id="dec56-676">Inferences are not made from `U1` to either `X<Q>` or `Y<Q>`.)</span></span>

   <span data-ttu-id="dec56-677">Pokud platí kterákoli z těchto případů, pak se provádí odvození *z* každý `Ui` *k* odpovídající `Vi` následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="dec56-677">If any of these cases apply then an inference is made *from* each `Ui` *to* the corresponding `Vi` as follows:</span></span>
   *  <span data-ttu-id="dec56-678">Pokud `Ui` není známa jako typ odkazu a *přesná odvození* tvoří</span><span class="sxs-lookup"><span data-stu-id="dec56-678">If  `Ui` is not known to be a reference type then an *exact inference* is made</span></span>
   *  <span data-ttu-id="dec56-679">Jinak, pokud `V` typ pole je pak *horní mez odvození* provedení</span><span class="sxs-lookup"><span data-stu-id="dec56-679">Otherwise, if `V` is an array type then an *upper-bound inference* is made</span></span>
   *  <span data-ttu-id="dec56-680">Jinak, pokud `U` je `C<U1...Uk>` pak odvození závisí na parametr typu i tý `C`:</span><span class="sxs-lookup"><span data-stu-id="dec56-680">Otherwise, if `U` is `C<U1...Uk>` then inference depends on the i-th type parameter of `C`:</span></span>
      *  <span data-ttu-id="dec56-681">Pokud se jedná se o kovariantní pak *horní mez odvození* tvoří.</span><span class="sxs-lookup"><span data-stu-id="dec56-681">If it is covariant then an *upper-bound inference* is made.</span></span>
      *  <span data-ttu-id="dec56-682">Pokud je kontravariantní o *dolní mez odvození* tvoří.</span><span class="sxs-lookup"><span data-stu-id="dec56-682">If it is contravariant then a *lower-bound inference* is made.</span></span>
      *  <span data-ttu-id="dec56-683">Pokud je invariantní pak *přesná odvození* tvoří.</span><span class="sxs-lookup"><span data-stu-id="dec56-683">If it is invariant then an *exact inference* is made.</span></span>
*  <span data-ttu-id="dec56-684">V opačném případě nebudou provedeny žádné závěry.</span><span class="sxs-lookup"><span data-stu-id="dec56-684">Otherwise, no inferences are made.</span></span>   

#### <a name="fixing"></a><span data-ttu-id="dec56-685">Oprava</span><span class="sxs-lookup"><span data-stu-id="dec56-685">Fixing</span></span>

<span data-ttu-id="dec56-686">*Nevyřešené* proměnná typu `Xi` sadu hranice je *oprava* následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="dec56-686">An *unfixed* type variable `Xi` with a set of bounds is *fixed* as follows:</span></span>

*  <span data-ttu-id="dec56-687">Sada *Release candidate typy* `Uj` začíná jako sadu v sadě hranice pro všechny typy `Xi`.</span><span class="sxs-lookup"><span data-stu-id="dec56-687">The set of *candidate types* `Uj` starts out as the set of all types in the set of bounds for `Xi`.</span></span>
*  <span data-ttu-id="dec56-688">Následně prozkoumáme každý mez pro `Xi` pak: Pro každý přesné mez `U` z `Xi` všechny typy `Uj` které nejsou identické s `U` se odeberou ze sady Release candidate.</span><span class="sxs-lookup"><span data-stu-id="dec56-688">We then examine each bound for `Xi` in turn: For each exact bound `U` of `Xi` all types `Uj` which are not identical to `U` are removed from the candidate set.</span></span> <span data-ttu-id="dec56-689">Pro každý dolní mez `U` z `Xi` všechny typy `Uj` do nichž je *není* implicitní převod z `U` se odeberou ze sady Release candidate.</span><span class="sxs-lookup"><span data-stu-id="dec56-689">For each lower bound `U` of `Xi` all types `Uj` to which there is *not* an implicit conversion from `U` are removed from the candidate set.</span></span> <span data-ttu-id="dec56-690">Pro každý horní mez `U` z `Xi` všechny typy `Uj` z nichž je *není* implicitní převod na `U` se odeberou ze sady Release candidate.</span><span class="sxs-lookup"><span data-stu-id="dec56-690">For each upper bound `U` of `Xi` all types `Uj` from which there is *not* an implicit conversion to `U` are removed from the candidate set.</span></span>
*  <span data-ttu-id="dec56-691">Pokud mezi zbývající typy Release candidate `Uj` je jedinečný typ `V` ze které je implicitní převod na všech ostatních Release candidate typů, pak `Xi` je pevně `V`.</span><span class="sxs-lookup"><span data-stu-id="dec56-691">If among the remaining candidate types `Uj` there is a unique type `V` from which there is an implicit conversion to all the other candidate types, then `Xi` is fixed to `V`.</span></span>
*  <span data-ttu-id="dec56-692">V opačném případě odvození typu nezdaří.</span><span class="sxs-lookup"><span data-stu-id="dec56-692">Otherwise, type inference fails.</span></span>

#### <a name="inferred-return-type"></a><span data-ttu-id="dec56-693">Odvozený návratový typ.</span><span class="sxs-lookup"><span data-stu-id="dec56-693">Inferred return type</span></span>

<span data-ttu-id="dec56-694">Odvozený typ vrácené hodnoty anonymní funkci `F` se používá při překladu typu odvození a přetížení.</span><span class="sxs-lookup"><span data-stu-id="dec56-694">The inferred return type of an anonymous function `F` is used during type inference and overload resolution.</span></span> <span data-ttu-id="dec56-695">Odvozený návratový typ lze určit pouze pro anonymní funkce, kde všech parametrů, které typy jsou označovány buď protože jsou výslovně uvedené, zajišťována konverzi anonymní funkce nebo vyvozen při odvození typu na nadřazeném obecné volání metody.</span><span class="sxs-lookup"><span data-stu-id="dec56-695">The inferred return type can only be determined for an anonymous function where all parameter types are known, either because they are explicitly given, provided through an anonymous function conversion or inferred during type inference on an enclosing generic method invocation.</span></span>

<span data-ttu-id="dec56-696">***Odvodit typ výsledku*** je stanoven následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="dec56-696">The ***inferred result type*** is determined as follows:</span></span>

*  <span data-ttu-id="dec56-697">Pokud tělo `F` je *výraz* , která má typ, pak typ odvozený výsledku `F` je typ tohoto výrazu.</span><span class="sxs-lookup"><span data-stu-id="dec56-697">If the body of `F` is an *expression* that has a type, then the inferred result type of `F` is the type of that expression.</span></span>
*  <span data-ttu-id="dec56-698">Pokud tělo `F` je *bloku* a sada výrazů v bloku `return` příkazů má doporučené společný typ `T` ([hledání nejlepší společný typ sada výrazů](expressions.md#finding-the-best-common-type-of-a-set-of-expressions)), pak odvozený typ výsledku `F` je `T`.</span><span class="sxs-lookup"><span data-stu-id="dec56-698">If the body of `F` is a *block* and the set of expressions in the block's `return` statements has a best common type `T` ([Finding the best common type of a set of expressions](expressions.md#finding-the-best-common-type-of-a-set-of-expressions)), then the inferred result type of `F` is `T`.</span></span>
*  <span data-ttu-id="dec56-699">V opačném případě nejde odvodit typ výsledku pro `F`.</span><span class="sxs-lookup"><span data-stu-id="dec56-699">Otherwise, a result type cannot be inferred for `F`.</span></span>

<span data-ttu-id="dec56-700">***Odvodit návratový typ*** je stanoven následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="dec56-700">The ***inferred return type*** is determined as follows:</span></span>

*  <span data-ttu-id="dec56-701">Pokud `F` je asynchronní a tělo `F` je výrazem jsou klasifikovány jako nic ([výrazu klasifikace](expressions.md#expression-classifications)), nebo blok příkazů, kde mají žádné návratové příkazy výrazů, odvozený návratový typ je `System.Threading.Tasks.Task`</span><span class="sxs-lookup"><span data-stu-id="dec56-701">If `F` is async and the body of `F` is either an expression classified as nothing ([Expression classifications](expressions.md#expression-classifications)), or a statement block where no return statements have expressions, the inferred return type is `System.Threading.Tasks.Task`</span></span>
*  <span data-ttu-id="dec56-702">Pokud `F` je asynchronní a má typ odvozené výsledek `T`, odvozený návratový typ je `System.Threading.Tasks.Task<T>`.</span><span class="sxs-lookup"><span data-stu-id="dec56-702">If `F` is async and has an inferred result type `T`, the inferred return type is `System.Threading.Tasks.Task<T>`.</span></span>
*  <span data-ttu-id="dec56-703">Pokud `F` je jiné asynchronní a má typ odvozené výsledek `T`, odvozený návratový typ je `T`.</span><span class="sxs-lookup"><span data-stu-id="dec56-703">If `F` is non-async and has an inferred result type `T`, the inferred return type is `T`.</span></span>
*  <span data-ttu-id="dec56-704">V opačném případě nejde odvodit návratový typ pro `F`.</span><span class="sxs-lookup"><span data-stu-id="dec56-704">Otherwise a return type cannot be inferred for `F`.</span></span>

<span data-ttu-id="dec56-705">Jako příklad zahrnující anonymní funkce odvození typu, zvažte `Select` metodu rozšíření deklarovat v `System.Linq.Enumerable` třídy:</span><span class="sxs-lookup"><span data-stu-id="dec56-705">As an example of type inference involving anonymous functions, consider the `Select` extension method declared in the `System.Linq.Enumerable` class:</span></span>
```csharp
namespace System.Linq
{
    public static class Enumerable
    {
        public static IEnumerable<TResult> Select<TSource,TResult>(
            this IEnumerable<TSource> source,
            Func<TSource,TResult> selector)
        {
            foreach (TSource element in source) yield return selector(element);
        }
    }
}
```

<span data-ttu-id="dec56-706">Za předpokladu, že `System.Linq` obor názvů byla importována pomocí `using` klauzule a zadané třídy `Customer` s `Name` vlastnost typu `string`, `Select` metodu je možné vybrat názvy seznam zákazníků:</span><span class="sxs-lookup"><span data-stu-id="dec56-706">Assuming the `System.Linq` namespace was imported with a `using` clause, and given a class `Customer` with a `Name` property of type `string`, the `Select` method can be used to select the names of a list of customers:</span></span>
```csharp
List<Customer> customers = GetCustomerList();
IEnumerable<string> names = customers.Select(c => c.Name);
```

<span data-ttu-id="dec56-707">Volání metody rozšíření ([volání metod rozšíření](expressions.md#extension-method-invocations)) z `Select` zpracovává přepis volání do volání statické metody:</span><span class="sxs-lookup"><span data-stu-id="dec56-707">The extension method invocation ([Extension method invocations](expressions.md#extension-method-invocations)) of `Select` is processed by rewriting the invocation to a static method invocation:</span></span>
```csharp
IEnumerable<string> names = Enumerable.Select(customers, c => c.Name);
```

<span data-ttu-id="dec56-708">Vzhledem k tomu, že argumenty typu nejsou explicitně zadané, odvození typu je použít k odvození argumenty typu.</span><span class="sxs-lookup"><span data-stu-id="dec56-708">Since type arguments were not explicitly specified, type inference is used to infer the type arguments.</span></span> <span data-ttu-id="dec56-709">Nejprve je potřeba `customers` argument má vztah k `source` parametr odvození `T` bude `Customer`.</span><span class="sxs-lookup"><span data-stu-id="dec56-709">First, the `customers` argument is related to the `source` parameter, inferring `T` to be `Customer`.</span></span> <span data-ttu-id="dec56-710">Potom pomocí anonymní funkce zadejte procesu odvození popsané výš, `c` je daný typ `Customer`a výraz `c.Name` souvisí s návratový typ `selector` parametr, odvození `S` bude `string`.</span><span class="sxs-lookup"><span data-stu-id="dec56-710">Then, using the anonymous function type inference process described above, `c` is given type `Customer`, and the expression `c.Name` is related to the return type of the `selector` parameter, inferring `S` to be `string`.</span></span> <span data-ttu-id="dec56-711">Proto je ekvivalentní k vyvolání</span><span class="sxs-lookup"><span data-stu-id="dec56-711">Thus, the invocation is equivalent to</span></span>
```csharp
Sequence.Select<Customer,string>(customers, (Customer c) => c.Name)
```
<span data-ttu-id="dec56-712">a výsledek je typu `IEnumerable<string>`.</span><span class="sxs-lookup"><span data-stu-id="dec56-712">and the result is of type `IEnumerable<string>`.</span></span>

<span data-ttu-id="dec56-713">Následující příklad ukazuje, jak anonymní typ funkce odvození umožňuje informace o typu "tok" mezi argumenty ve volání obecné metody.</span><span class="sxs-lookup"><span data-stu-id="dec56-713">The following example demonstrates how anonymous function type inference allows type information to "flow" between arguments in a generic method invocation.</span></span> <span data-ttu-id="dec56-714">Zadané metody:</span><span class="sxs-lookup"><span data-stu-id="dec56-714">Given the method:</span></span>
```csharp
static Z F<X,Y,Z>(X value, Func<X,Y> f1, Func<Y,Z> f2) {
    return f2(f1(value));
}
```

<span data-ttu-id="dec56-715">Odvození typu připouštějícího pro vyvolání:</span><span class="sxs-lookup"><span data-stu-id="dec56-715">Type inference for the invocation:</span></span>
```csharp
double seconds = F("1:15:30", s => TimeSpan.Parse(s), t => t.TotalSeconds);
```
<span data-ttu-id="dec56-716">Probíhá následujícím způsobem: První, argument `"1:15:30"` souvisí s `value` parametr odvození `X` bude `string`.</span><span class="sxs-lookup"><span data-stu-id="dec56-716">proceeds as follows: First, the argument `"1:15:30"` is related to the `value` parameter, inferring `X` to be `string`.</span></span> <span data-ttu-id="dec56-717">Potom, parametr první anonymní funkce `s`, dostane odvozený typ `string`a výraz `TimeSpan.Parse(s)` souvisí s návratovým typem `f1`, odvození `Y` bude `System.TimeSpan`.</span><span class="sxs-lookup"><span data-stu-id="dec56-717">Then, the parameter of the first anonymous function, `s`, is given the inferred type `string`, and the expression `TimeSpan.Parse(s)` is related to the return type of `f1`, inferring `Y` to be `System.TimeSpan`.</span></span> <span data-ttu-id="dec56-718">Nakonec parametr druhý anonymní funkce `t`, dostane odvozený typ `System.TimeSpan`a výraz `t.TotalSeconds` souvisí s návratovým typem `f2`, odvození `Z` bude `double`.</span><span class="sxs-lookup"><span data-stu-id="dec56-718">Finally, the parameter of the second anonymous function, `t`, is given the inferred type `System.TimeSpan`, and the expression `t.TotalSeconds` is related to the return type of `f2`, inferring `Z` to be `double`.</span></span> <span data-ttu-id="dec56-719">Proto je výsledek volání typu `double`.</span><span class="sxs-lookup"><span data-stu-id="dec56-719">Thus, the result of the invocation is of type `double`.</span></span>

#### <a name="type-inference-for-conversion-of-method-groups"></a><span data-ttu-id="dec56-720">Odvození typu pro převod skupin – metoda</span><span class="sxs-lookup"><span data-stu-id="dec56-720">Type inference for conversion of method groups</span></span>

<span data-ttu-id="dec56-721">Podobně jako u volání obecné metody se odvození typu musí také použít při skupinu metod `M` obsahující obecnou metodu je převeden na delegáta daného typu `D` ([Metoda skupiny převody](conversions.md#method-group-conversions)).</span><span class="sxs-lookup"><span data-stu-id="dec56-721">Similar to calls of generic methods, type inference must also be applied when a method group `M` containing a generic method is converted to a given delegate type `D` ([Method group conversions](conversions.md#method-group-conversions)).</span></span> <span data-ttu-id="dec56-722">Zadané metody</span><span class="sxs-lookup"><span data-stu-id="dec56-722">Given a method</span></span>
```csharp
Tr M<X1...Xn>(T1 x1 ... Tm xm)
```
<span data-ttu-id="dec56-723">a skupina metod `M` přiřazením na typ delegáta `D` úlohou odvození typu je najít argumenty typu `S1...Sn` tak, aby výraz:</span><span class="sxs-lookup"><span data-stu-id="dec56-723">and the method group `M` being assigned to the delegate type `D` the task of type inference is to find type arguments `S1...Sn` so that the expression:</span></span>
```csharp
M<S1...Sn>
```
<span data-ttu-id="dec56-724">stane se kompatibilní ([delegovat deklarace](delegates.md#delegate-declarations)) s `D`.</span><span class="sxs-lookup"><span data-stu-id="dec56-724">becomes compatible ([Delegate declarations](delegates.md#delegate-declarations)) with `D`.</span></span>

<span data-ttu-id="dec56-725">Na rozdíl od algoritmus odvození typu pro volání obecné metody, v tomto případě existují pouze argument *typy*, žádný argument *výrazy*.</span><span class="sxs-lookup"><span data-stu-id="dec56-725">Unlike the type inference algorithm for generic method calls, in this case there are only argument *types*, no argument *expressions*.</span></span> <span data-ttu-id="dec56-726">Zejména, neexistují žádné anonymní funkce a proto není nutné více fázích odvození.</span><span class="sxs-lookup"><span data-stu-id="dec56-726">In particular, there are no anonymous functions and hence no need for multiple phases of inference.</span></span>

<span data-ttu-id="dec56-727">Místo toho všechny `Xi` jsou považovány za *nevyřešené*a *dolní mez odvození* tvoří *z* každému typu argumentu `Uj` z `D` *k* odpovídající typ parametru `Tj` z `M`.</span><span class="sxs-lookup"><span data-stu-id="dec56-727">Instead, all `Xi` are considered *unfixed*, and a *lower-bound inference* is made *from* each argument type `Uj` of `D` *to* the corresponding parameter type `Tj` of `M`.</span></span> <span data-ttu-id="dec56-728">Pokud pro některou `Xi` nebyly nalezeny žádné hranice, typ odvození.</span><span class="sxs-lookup"><span data-stu-id="dec56-728">If for any of the `Xi` no bounds were found, type inference fails.</span></span> <span data-ttu-id="dec56-729">V opačném případě všechny `Xi` jsou *oprava* odpovídající `Si`, které jsou výsledkem odvození typu proměnné.</span><span class="sxs-lookup"><span data-stu-id="dec56-729">Otherwise, all `Xi` are *fixed* to corresponding `Si`, which are the result of type inference.</span></span>

#### <a name="finding-the-best-common-type-of-a-set-of-expressions"></a><span data-ttu-id="dec56-730">Vyhledání nejlepších společný typ sada výrazů</span><span class="sxs-lookup"><span data-stu-id="dec56-730">Finding the best common type of a set of expressions</span></span>

<span data-ttu-id="dec56-731">V některých případech se musí odvodit pro sadu výrazů stejného typu.</span><span class="sxs-lookup"><span data-stu-id="dec56-731">In some cases, a common type needs to be inferred for a set of expressions.</span></span> <span data-ttu-id="dec56-732">Zejména typy elementů implicitně typovaná pole a návratové typy anonymní funkce s *bloku* úřadů, které se nacházejí v tímto způsobem.</span><span class="sxs-lookup"><span data-stu-id="dec56-732">In particular, the element types of implicitly typed arrays and the return types of anonymous functions with *block* bodies are found in this way.</span></span>

<span data-ttu-id="dec56-733">Intuitivně, vzhledem k sadě výrazů `E1...Em` tento odvození by měl být ekvivalentní volání metody</span><span class="sxs-lookup"><span data-stu-id="dec56-733">Intuitively, given a set of expressions `E1...Em` this inference should be equivalent to calling a method</span></span>
```csharp
Tr M<X>(X x1 ... X xm)
```
<span data-ttu-id="dec56-734">s `Ei` jako argumenty.</span><span class="sxs-lookup"><span data-stu-id="dec56-734">with the `Ei` as arguments.</span></span>

<span data-ttu-id="dec56-735">Přesněji řečeno, odvození začíná *nevyřešené* proměnná typu `X`.</span><span class="sxs-lookup"><span data-stu-id="dec56-735">More precisely, the inference starts out with an *unfixed* type variable `X`.</span></span> <span data-ttu-id="dec56-736">*Výstupní typ závěry* pak vyvinou *z* každý `Ei` *k* `X`.</span><span class="sxs-lookup"><span data-stu-id="dec56-736">*Output type inferences* are then made *from* each `Ei` *to* `X`.</span></span> <span data-ttu-id="dec56-737">Nakonec `X` je *oprava* a pokud je úspěšná, výsledný typ `S` je Výsledný nejlepší společný typ pro výraz.</span><span class="sxs-lookup"><span data-stu-id="dec56-737">Finally, `X` is *fixed* and, if successful, the resulting type `S` is the resulting best common type for the expressions.</span></span> <span data-ttu-id="dec56-738">Pokud neexistuje žádný takový `S` existuje, výrazy mají žádné doporučené společný typ.</span><span class="sxs-lookup"><span data-stu-id="dec56-738">If no such `S` exists, the expressions have no best common type.</span></span>

### <a name="overload-resolution"></a><span data-ttu-id="dec56-739">Rozlišení přetěžování</span><span class="sxs-lookup"><span data-stu-id="dec56-739">Overload resolution</span></span>

<span data-ttu-id="dec56-740">Řešení přetížení se doba vazby mechanismus pro výběr nejlepší funkce člena k vyvolání zadaný seznam argumentů a sadu Release candidate funkce členů.</span><span class="sxs-lookup"><span data-stu-id="dec56-740">Overload resolution is a binding-time mechanism for selecting the best function member to invoke given an argument list and a set of candidate function members.</span></span> <span data-ttu-id="dec56-741">Řešení přetížení vybere členské funkce k vyvolání v následujících různých kontextech v rámci jazyka C#:</span><span class="sxs-lookup"><span data-stu-id="dec56-741">Overload resolution selects the function member to invoke in the following distinct contexts within C#:</span></span>

*  <span data-ttu-id="dec56-742">Vyvolání metody s názvem v *invocation_expression* ([volání metod](expressions.md#method-invocations)).</span><span class="sxs-lookup"><span data-stu-id="dec56-742">Invocation of a method named in an *invocation_expression* ([Method invocations](expressions.md#method-invocations)).</span></span>
*  <span data-ttu-id="dec56-743">Vyvolání konstruktoru instance s názvem v *object_creation_expression* ([výrazy vytvoření objektu](expressions.md#object-creation-expressions)).</span><span class="sxs-lookup"><span data-stu-id="dec56-743">Invocation of an instance constructor named in an *object_creation_expression* ([Object creation expressions](expressions.md#object-creation-expressions)).</span></span>
*  <span data-ttu-id="dec56-744">Vyvolání indexeru přistupující objekt prostřednictvím *element_access* ([přístup k prvkům](expressions.md#element-access)).</span><span class="sxs-lookup"><span data-stu-id="dec56-744">Invocation of an indexer accessor through an *element_access* ([Element access](expressions.md#element-access)).</span></span>
*  <span data-ttu-id="dec56-745">Vyvolání předdefinovaných nebo uživatelem definovaný operátor odkazuje ve výrazu ([unární operátor rozlišení přetěžování](expressions.md#unary-operator-overload-resolution) a [binárním operátorem rozlišení přetěžování](expressions.md#binary-operator-overload-resolution)).</span><span class="sxs-lookup"><span data-stu-id="dec56-745">Invocation of a predefined or user-defined operator referenced in an expression ([Unary operator overload resolution](expressions.md#unary-operator-overload-resolution) and [Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)).</span></span>

<span data-ttu-id="dec56-746">Každá z těchto kontextech definuje sadu Release candidate funkce členů a seznamu argumentů svůj vlastní jedinečný způsobem, jak je podrobně popsán v části výše uvedených.</span><span class="sxs-lookup"><span data-stu-id="dec56-746">Each of these contexts defines the set of candidate function members and the list of arguments in its own unique way, as described in detail in the sections listed above.</span></span> <span data-ttu-id="dec56-747">Například sadu kandidáty pro volání metody nezahrnuje metody označené `override` ([člen vyhledávání](expressions.md#member-lookup)), a metody základní třídy nejsou kandidáty pokud platí všechny metody v odvozené třídě ([ Volání metod](expressions.md#method-invocations)).</span><span class="sxs-lookup"><span data-stu-id="dec56-747">For example, the set of candidates for a method invocation does not include methods marked `override` ([Member lookup](expressions.md#member-lookup)), and methods in a base class are not candidates if any method in a derived class is applicable ([Method invocations](expressions.md#method-invocations)).</span></span>

<span data-ttu-id="dec56-748">Jakmile se zjistily členy funkce Release candidate a seznam argumentů, výběr nejlepší členské funkce je stejná ve všech případech:</span><span class="sxs-lookup"><span data-stu-id="dec56-748">Once the candidate function members and the argument list have been identified, the selection of the best function member is the same in all cases:</span></span>

*  <span data-ttu-id="dec56-749">Zadaný sadu členů funkce použít Release candidate, nejlepší funkce člena v tom, že sada se nachází.</span><span class="sxs-lookup"><span data-stu-id="dec56-749">Given the set of applicable candidate function members, the best function member in that set is located.</span></span> <span data-ttu-id="dec56-750">Pokud sada obsahuje pouze jeden člen funkce, je daný člen funkce nejlepší členské funkce.</span><span class="sxs-lookup"><span data-stu-id="dec56-750">If the set contains only one function member, then that function member is the best function member.</span></span> <span data-ttu-id="dec56-751">V opačném případě je nejlepší členské funkce člena jednu funkci, která je obecně lepší než všechny ostatní funkce členy s ohledem na zadaného seznamu argumentů, za předpokladu, že každý člen funkce je ve srovnání s všichni členové funkce pomocí pravidel v [ Lepší členské funkce](expressions.md#better-function-member).</span><span class="sxs-lookup"><span data-stu-id="dec56-751">Otherwise, the best function member is the one function member that is better than all other function members with respect to the given argument list, provided that each function member is compared to all other function members using the rules in [Better function member](expressions.md#better-function-member).</span></span> <span data-ttu-id="dec56-752">Pokud existuje přesně jeden člen funkce, která je obecně lepší než všechny ostatní funkce členové, pak je nejednoznačné volání funkce člena a dojde k chybě vazby čas.</span><span class="sxs-lookup"><span data-stu-id="dec56-752">If there is not exactly one function member that is better than all other function members, then the function member invocation is ambiguous and a binding-time error occurs.</span></span>

<span data-ttu-id="dec56-753">V dalších částech definovat přesně význam podmínky ***použitelná funkce člena*** a ***lepší členské funkce***.</span><span class="sxs-lookup"><span data-stu-id="dec56-753">The following sections define the exact meanings of the terms ***applicable function member*** and ***better function member***.</span></span>

#### <a name="applicable-function-member"></a><span data-ttu-id="dec56-754">Člen příslušné funkce</span><span class="sxs-lookup"><span data-stu-id="dec56-754">Applicable function member</span></span>

<span data-ttu-id="dec56-755">Členské funkce se říká, že ***použitelná funkce člena*** s ohledem na seznam argumentů `A` Pokud jsou splněny všechny z následujících akcí:</span><span class="sxs-lookup"><span data-stu-id="dec56-755">A function member is said to be an ***applicable function member*** with respect to an argument list `A` when all of the following are true:</span></span>

*  <span data-ttu-id="dec56-756">Každý argument v `A` odpovídá parametru v deklaraci členské funkce, jak je popsáno v [odpovídající parametry](expressions.md#corresponding-parameters), a žádné parametry, které odpovídá žádný argument je volitelný parametr.</span><span class="sxs-lookup"><span data-stu-id="dec56-756">Each argument in `A` corresponds to a parameter in the function member declaration as described in [Corresponding parameters](expressions.md#corresponding-parameters), and any parameter to which no argument corresponds is an optional parameter.</span></span>
*  <span data-ttu-id="dec56-757">Pro každý argument v `A`, předávání režimu argumentu parametrů (například, hodnota `ref`, nebo `out`) je stejný jako režim parametru předávání příslušného parametru a</span><span class="sxs-lookup"><span data-stu-id="dec56-757">For each argument in `A`, the parameter passing mode of the argument (i.e., value, `ref`, or `out`) is identical to the parameter passing mode of the corresponding parameter, and</span></span>
   *  <span data-ttu-id="dec56-758">pro parametr hodnoty nebo pole parametrů, implicitní převod ([implicitních převodů](conversions.md#implicit-conversions)) existuje v argumentu na typ odpovídající parametr nebo</span><span class="sxs-lookup"><span data-stu-id="dec56-758">for a value parameter or a parameter array, an implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)) exists from the argument to the type of the corresponding parameter, or</span></span>
   *  <span data-ttu-id="dec56-759">pro `ref` nebo `out` parametr, typ argumentu je stejné jako odpovídající parametr typu.</span><span class="sxs-lookup"><span data-stu-id="dec56-759">for a `ref` or `out` parameter, the type of the argument is identical to the type of the corresponding parameter.</span></span> <span data-ttu-id="dec56-760">Po všech `ref` nebo `out` parametr je alias pro předaného argumentu.</span><span class="sxs-lookup"><span data-stu-id="dec56-760">After all, a `ref` or `out` parameter is an alias for the argument passed.</span></span>

<span data-ttu-id="dec56-761">Funkce člena, která zahrnuje parametr pole Pokud je člen funkce použít podle výše uvedených pravidel, se říká fungovaly v jeho ***normalizačním formuláři***.</span><span class="sxs-lookup"><span data-stu-id="dec56-761">For a function member that includes a parameter array, if the function member is applicable by the above rules, it is said to be applicable in its ***normal form***.</span></span> <span data-ttu-id="dec56-762">Pokud člen funkce, která zahrnuje parametr pole se nedá použít v podobě normální, členské funkce může být místo toho použít v jeho ***rozšířit formuláře***:</span><span class="sxs-lookup"><span data-stu-id="dec56-762">If a function member that includes a parameter array is not applicable in its normal form, the function member may instead be applicable in its ***expanded form***:</span></span>

*  <span data-ttu-id="dec56-763">Rozšířené formuláře je vytvořený tak, že nahradíte pole parametrů v deklaraci členské funkce s hodnotou nula nebo více parametrů hodnotu parametru typu prvku pole, tento počet argumentů v seznamu argumentů `A` odpovídá celkové počet parametrů.</span><span class="sxs-lookup"><span data-stu-id="dec56-763">The expanded form is constructed by replacing the parameter array in the function member declaration with zero or more value parameters of the element type of the parameter array such that the number of arguments in the argument list `A` matches the total number of parameters.</span></span> <span data-ttu-id="dec56-764">Pokud `A` má menší počet argumentů, než je počet parametrů pevné v deklaraci členské funkce, rozšířené formu členské funkce nelze zkonstruovat a se tedy nedá použít.</span><span class="sxs-lookup"><span data-stu-id="dec56-764">If `A` has fewer arguments than the number of fixed parameters in the function member declaration, the expanded form of the function member cannot be constructed and is thus not applicable.</span></span>
*  <span data-ttu-id="dec56-765">V opačném případě se vztahuje Pokud pro každý argument v rozšířené formuláře `A` režim předávání parametru argumentu je stejný jako režim parametru předávání příslušného parametru a</span><span class="sxs-lookup"><span data-stu-id="dec56-765">Otherwise, the expanded form is applicable if for each argument in `A` the parameter passing mode of the argument is identical to the parameter passing mode of the corresponding parameter, and</span></span>
   *  <span data-ttu-id="dec56-766">pro parametr pevná hodnota nebo hodnota parametru vytvořené rozšíření implicitní převod ([implicitních převodů](conversions.md#implicit-conversions)) existuje z typu argumentu na typ odpovídající parametr nebo</span><span class="sxs-lookup"><span data-stu-id="dec56-766">for a fixed value parameter or a value parameter created by the expansion, an implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)) exists from the type of the argument to the type of the corresponding parameter, or</span></span>
   *  <span data-ttu-id="dec56-767">pro `ref` nebo `out` parametr, typ argumentu je stejné jako odpovídající parametr typu.</span><span class="sxs-lookup"><span data-stu-id="dec56-767">for a `ref` or `out` parameter, the type of the argument is identical to the type of the corresponding parameter.</span></span>

#### <a name="better-function-member"></a><span data-ttu-id="dec56-768">Lepší členské funkce</span><span class="sxs-lookup"><span data-stu-id="dec56-768">Better function member</span></span>

<span data-ttu-id="dec56-769">Pro účely stanovení lepší členské funkce je vytvořen seznam stripped-down argumentů A obsahující pouze výrazy argument sami v pořadí, ve kterém jsou uvedeny v původní seznam argumentů.</span><span class="sxs-lookup"><span data-stu-id="dec56-769">For the purposes of determining the better function member, a stripped-down argument list A is constructed containing just the argument expressions themselves in the order they appear in the original argument list.</span></span>

<span data-ttu-id="dec56-770">Parametr uvádí pro jednotlivé funkce Release candidate, do které jsou členy vytvořeny následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="dec56-770">Parameter lists for each of the candidate function members are constructed in the following way:</span></span>

*  <span data-ttu-id="dec56-771">Rozšířené forma se používá, pokud bylo použitelné pouze ve formuláři Rozšířené členské funkce.</span><span class="sxs-lookup"><span data-stu-id="dec56-771">The expanded form is used if the function member was applicable only in the expanded form.</span></span>
*  <span data-ttu-id="dec56-772">Volitelné parametry bez odpovídající argumentů se odeberou ze seznamu parametrů</span><span class="sxs-lookup"><span data-stu-id="dec56-772">Optional parameters with no corresponding arguments are removed from the parameter list</span></span>
*  <span data-ttu-id="dec56-773">Parametry jsou přeuspořádají, aby k nim dojde na stejné pozici jako příslušný argument v seznamu argumentů.</span><span class="sxs-lookup"><span data-stu-id="dec56-773">The parameters are reordered so that they occur at the same position as the corresponding argument in the argument list.</span></span>

<span data-ttu-id="dec56-774">Zadaný seznam argumentů `A` sadu výrazy argumentu `{E1, E2, ..., En}` a dva členy příslušné funkce `Mp` a `Mq` se typy parametrů `{P1, P2, ..., Pn}` a `{Q1, Q2, ..., Qn}`, `Mp` je definován jako ***lepší členské funkce*** než `Mq` Pokud</span><span class="sxs-lookup"><span data-stu-id="dec56-774">Given an argument list `A` with a set of argument expressions `{E1, E2, ..., En}` and two applicable function members `Mp` and `Mq` with parameter types `{P1, P2, ..., Pn}` and `{Q1, Q2, ..., Qn}`, `Mp` is defined to be a ***better function member*** than `Mq` if</span></span>

*  <span data-ttu-id="dec56-775">pro každý argument implicitní převod z `Ex` k `Qx` není lepší než implicitní převod z `Ex` k `Px`, a</span><span class="sxs-lookup"><span data-stu-id="dec56-775">for each argument, the implicit conversion from `Ex` to `Qx` is not better than the implicit conversion from `Ex` to `Px`, and</span></span>
*  <span data-ttu-id="dec56-776">pro nejméně jeden argument, převod z `Ex` k `Px` je obecně lepší než převod z `Ex` k `Qx`.</span><span class="sxs-lookup"><span data-stu-id="dec56-776">for at least one argument, the conversion from `Ex` to `Px` is better than the conversion from `Ex` to `Qx`.</span></span>

<span data-ttu-id="dec56-777">Při provádění vyhodnocení, pokud `Mp` nebo `Mq` platí v podobě rozšířené, pak `Px` nebo `Qx` odkazuje na parametr ve formuláři rozbaleného seznamu parametrů.</span><span class="sxs-lookup"><span data-stu-id="dec56-777">When performing this evaluation, if `Mp` or `Mq` is applicable in its expanded form, then `Px` or `Qx` refers to a parameter in the expanded form of the parameter list.</span></span>

<span data-ttu-id="dec56-778">V případě, že parametr typu pořadí `{P1, P2, ..., Pn}` a `{Q1, Q2, ..., Qn}` jsou ekvivalentní (to znamená každou `Pi` má konverzi identity do odpovídajících `Qi`), se použijí následující pravidla shody, v pořadí, chcete-li určit, tím lepší Členské funkce.</span><span class="sxs-lookup"><span data-stu-id="dec56-778">In case the parameter type sequences `{P1, P2, ..., Pn}` and `{Q1, Q2, ..., Qn}` are equivalent (i.e. each `Pi` has an identity conversion to the corresponding `Qi`), the following tie-breaking rules are applied, in order, to determine the better function member.</span></span>

*  <span data-ttu-id="dec56-779">Pokud `Mp` je neobecnou metodu a `Mq` je obecná metoda, pak `Mp` je obecně lepší než `Mq`.</span><span class="sxs-lookup"><span data-stu-id="dec56-779">If `Mp` is a non-generic method and `Mq` is a generic method, then `Mp` is better than `Mq`.</span></span>
*  <span data-ttu-id="dec56-780">Jinak, pokud `Mp` lze použít v podobě normální a `Mq` má `params` pole a platí jenom v podobě rozšířené, pak `Mp` je obecně lepší než `Mq`.</span><span class="sxs-lookup"><span data-stu-id="dec56-780">Otherwise, if `Mp` is applicable in its normal form and `Mq` has a `params` array and is applicable only in its expanded form, then `Mp` is better than `Mq`.</span></span>
*  <span data-ttu-id="dec56-781">Jinak, pokud `Mp` prohlásil více parametrů než `Mq`, pak `Mp` je obecně lepší než `Mq`.</span><span class="sxs-lookup"><span data-stu-id="dec56-781">Otherwise, if `Mp` has more declared parameters than `Mq`, then `Mp` is better than `Mq`.</span></span> <span data-ttu-id="dec56-782">Tato situace může nastat, pokud obě metody mají `params` pole a jsou použitelné jenom v rozšířené formuláře.</span><span class="sxs-lookup"><span data-stu-id="dec56-782">This can occur if both methods have `params` arrays and are applicable only in their expanded forms.</span></span>
*  <span data-ttu-id="dec56-783">Jinak pokud všechny parametry `Mp` mít odpovídající argument, že výchozí argumenty nahrazené za nejméně jeden volitelný parametr `Mq` pak `Mp` je obecně lepší než `Mq`.</span><span class="sxs-lookup"><span data-stu-id="dec56-783">Otherwise if all parameters of `Mp` have a corresponding argument whereas default arguments need to be substituted for at least one optional parameter in `Mq` then `Mp` is better than `Mq`.</span></span>
*  <span data-ttu-id="dec56-784">Jinak, pokud `Mp` má zvláštní typy parametrů než `Mq`, pak `Mp` je obecně lepší než `Mq`.</span><span class="sxs-lookup"><span data-stu-id="dec56-784">Otherwise, if `Mp` has more specific parameter types than `Mq`, then `Mp` is better than `Mq`.</span></span> <span data-ttu-id="dec56-785">Umožní `{R1, R2, ..., Rn}` a `{S1, S2, ..., Sn}` představují typů bez instancí a nerozbalený parametrů `Mp` a `Mq`.</span><span class="sxs-lookup"><span data-stu-id="dec56-785">Let `{R1, R2, ..., Rn}` and `{S1, S2, ..., Sn}` represent the uninstantiated and unexpanded parameter types of `Mp` and `Mq`.</span></span> <span data-ttu-id="dec56-786">`Mp`na typy parametrů jsou podrobnější než `Mq`Pokud pro každý parametr `Rx` není méně specifické než `Sx`a pro nejméně jeden parametr, `Rx` je konkrétnější než `Sx`:</span><span class="sxs-lookup"><span data-stu-id="dec56-786">`Mp`'s parameter types are more specific than `Mq`'s if, for each parameter, `Rx` is not less specific than `Sx`, and, for at least one parameter, `Rx` is more specific than `Sx`:</span></span>
   *  <span data-ttu-id="dec56-787">Parametr typu je méně specifické než beztypový parametr.</span><span class="sxs-lookup"><span data-stu-id="dec56-787">A type parameter is less specific than a non-type parameter.</span></span>
   *  <span data-ttu-id="dec56-788">Rekurzivně, konstruovaný typ je konkrétnější než jiné konstruovaný typ (se stejným číslem argumenty typu), pokud alespoň jeden argument typu je konkrétnější a žádný argument typu je méně specifické než argument typu v jiném.</span><span class="sxs-lookup"><span data-stu-id="dec56-788">Recursively, a constructed type is more specific than another constructed type (with the same number of type arguments) if at least one type argument is more specific and no type argument is less specific than the corresponding type argument in the other.</span></span>
   *  <span data-ttu-id="dec56-789">Typ pole je konkrétnější než jiný typ pole (s stejný počet rozměrů), pokud je typ prvku prvního konkrétnější než druhý typ elementu.</span><span class="sxs-lookup"><span data-stu-id="dec56-789">An array type is more specific than another array type (with the same number of dimensions) if the element type of the first is more specific than the element type of the second.</span></span>
*  <span data-ttu-id="dec56-790">Jinak Pokud jeden člen je operátor zrušeno a druhý je operátor zdvižené,-zrušeno ten je lepší.</span><span class="sxs-lookup"><span data-stu-id="dec56-790">Otherwise if one member is a non-lifted operator and  the other is a lifted operator, the non-lifted one is better.</span></span>
*  <span data-ttu-id="dec56-791">V opačném případě je lepší ani členské funkce.</span><span class="sxs-lookup"><span data-stu-id="dec56-791">Otherwise, neither function member is better.</span></span>

#### <a name="better-conversion-from-expression"></a><span data-ttu-id="dec56-792">Lepší převod z výrazu</span><span class="sxs-lookup"><span data-stu-id="dec56-792">Better conversion from expression</span></span>

<span data-ttu-id="dec56-793">Zadaný implicitní převod `C1` , která převede z výrazu `E` na typ `T1`a implicitní převod `C2` , která převede z výrazu `E` na typ `T2`, `C1` je ***lepší převod*** než `C2` Pokud `E` přesně neodpovídá `T2` a obsahuje alespoň jeden z následujících akcí:</span><span class="sxs-lookup"><span data-stu-id="dec56-793">Given an implicit conversion `C1` that converts from an expression `E` to a type `T1`, and an implicit conversion `C2` that converts from an expression `E` to a type `T2`, `C1` is a ***better conversion*** than `C2` if `E` does not exactly match `T2` and at least one of the following holds:</span></span>

* <span data-ttu-id="dec56-794">`E` přesně odpovídá `T1` ([přesně odpovídající výraz](expressions.md#exactly-matching-expression))</span><span class="sxs-lookup"><span data-stu-id="dec56-794">`E` exactly matches `T1` ([Exactly matching Expression](expressions.md#exactly-matching-expression))</span></span>
* <span data-ttu-id="dec56-795">`T1` je lepší cílem převod než `T2` ([lepší převod cílové](expressions.md#better-conversion-target))</span><span class="sxs-lookup"><span data-stu-id="dec56-795">`T1` is a better conversion target than `T2` ([Better conversion target](expressions.md#better-conversion-target))</span></span>

#### <a name="exactly-matching-expression"></a><span data-ttu-id="dec56-796">Přesně odpovídající výraz</span><span class="sxs-lookup"><span data-stu-id="dec56-796">Exactly matching Expression</span></span>

<span data-ttu-id="dec56-797">Zadaný výraz `E` a typ `T`, `E` přesně odpovídá `T` pokud obsahuje jeden z následujících akcí:</span><span class="sxs-lookup"><span data-stu-id="dec56-797">Given an expression `E` and a type `T`, `E` exactly matches `T` if one of the following holds:</span></span>

*  <span data-ttu-id="dec56-798">`E` má typ `S`, a převod identity `S` do `T`</span><span class="sxs-lookup"><span data-stu-id="dec56-798">`E` has a type `S`, and an identity conversion exists from `S` to `T`</span></span>
*  <span data-ttu-id="dec56-799">`E` je anonymní funkce `T` je typ delegátu `D` nebo typu stromu výrazu `Expression<D>` a obsahuje jeden z následujících akcí:</span><span class="sxs-lookup"><span data-stu-id="dec56-799">`E` is an anonymous function, `T` is either a delegate type `D` or an expression tree type `Expression<D>` and one of the following holds:</span></span>
   *  <span data-ttu-id="dec56-800">Odvozený návratový typ `X` existuje pro `E` v rámci seznamu parametrů `D` ([Inferred návratový typ](expressions.md#inferred-return-type)), a převod identity `X` na návratový typ `D`</span><span class="sxs-lookup"><span data-stu-id="dec56-800">An inferred return type `X` exists for `E` in the context of the parameter list of `D` ([Inferred return type](expressions.md#inferred-return-type)), and an identity conversion exists from `X` to the return type of `D`</span></span>
   *  <span data-ttu-id="dec56-801">Buď `E` je jiné asynchronní a `D` má návratový typ `Y` nebo `E` je asynchronní a `D` má návratový typ `Task<Y>`, a obsahuje jeden z následujících akcí:</span><span class="sxs-lookup"><span data-stu-id="dec56-801">Either `E` is non-async and `D` has a return type `Y` or `E` is async and `D` has a return type `Task<Y>`, and one of the following holds:</span></span>
      * <span data-ttu-id="dec56-802">Tělo `E` je výraz, který přesně odpovídá `Y`</span><span class="sxs-lookup"><span data-stu-id="dec56-802">The body of `E` is an expression that exactly matches `Y`</span></span>
      * <span data-ttu-id="dec56-803">Tělo `E` je blok příkazů, kde každý návratový příkaz vrátí výraz, který přesně odpovídá `Y`</span><span class="sxs-lookup"><span data-stu-id="dec56-803">The body of `E` is a statement block where every return statement returns an expression that exactly matches `Y`</span></span>

#### <a name="better-conversion-target"></a><span data-ttu-id="dec56-804">Lepší převod cíl</span><span class="sxs-lookup"><span data-stu-id="dec56-804">Better conversion target</span></span>

<span data-ttu-id="dec56-805">Uvedené dva různé typy `T1` a `T2`, `T1` je cílem převod lepší než `T2` Pokud žádný implicitní převod z `T2` k `T1` existuje, a obsahuje alespoň jeden z následujících akcí:</span><span class="sxs-lookup"><span data-stu-id="dec56-805">Given two different types `T1` and `T2`, `T1` is a better conversion target than `T2` if no implicit conversion from `T2` to `T1` exists, and at least one of the following holds:</span></span>

*  <span data-ttu-id="dec56-806">Implicitní převod z `T1` k `T2` existuje</span><span class="sxs-lookup"><span data-stu-id="dec56-806">An implicit conversion from `T1` to `T2` exists</span></span>
*  <span data-ttu-id="dec56-807">`T1` je typ delegátu `D1` nebo typu stromu výrazu `Expression<D1>`, `T2` je typ delegátu `D2` nebo typu stromu výrazu `Expression<D2>`, `D1` má návratový typ `S1` a jeden z obsahuje následující:</span><span class="sxs-lookup"><span data-stu-id="dec56-807">`T1` is either a delegate type `D1` or an expression tree type `Expression<D1>`, `T2` is either a delegate type `D2` or an expression tree type `Expression<D2>`, `D1` has a return type `S1` and one of the following holds:</span></span>
   * <span data-ttu-id="dec56-808">`D2` vrací typ void</span><span class="sxs-lookup"><span data-stu-id="dec56-808">`D2` is void returning</span></span>
   * <span data-ttu-id="dec56-809">`D2` má návratový typ `S2`, a `S1` je cílem převod lepší než `S2`</span><span class="sxs-lookup"><span data-stu-id="dec56-809">`D2` has a return type `S2`, and `S1` is a better conversion target than `S2`</span></span>
*  <span data-ttu-id="dec56-810">`T1` je `Task<S1>`, `T2` je `Task<S2>`, a `S1` je cílem převod lepší než `S2`</span><span class="sxs-lookup"><span data-stu-id="dec56-810">`T1` is `Task<S1>`, `T2` is `Task<S2>`, and `S1` is a better conversion target than `S2`</span></span>
*  <span data-ttu-id="dec56-811">`T1` je `S1` nebo `S1?` kde `S1` je celočíselný typ se znaménkem, a `T2` je `S2` nebo `S2?` kde `S2` je celočíselný typ bez znaménka.</span><span class="sxs-lookup"><span data-stu-id="dec56-811">`T1` is `S1` or `S1?` where `S1` is a signed integral type, and `T2` is `S2` or `S2?` where `S2` is an unsigned integral type.</span></span> <span data-ttu-id="dec56-812">Konkrétně:</span><span class="sxs-lookup"><span data-stu-id="dec56-812">Specifically:</span></span>
   * <span data-ttu-id="dec56-813">`S1` je `sbyte` a `S2` je `byte`, `ushort`, `uint`, nebo `ulong`</span><span class="sxs-lookup"><span data-stu-id="dec56-813">`S1` is `sbyte` and `S2` is `byte`, `ushort`, `uint`, or `ulong`</span></span>
   * <span data-ttu-id="dec56-814">`S1` je `short` a `S2` je `ushort`, `uint`, nebo `ulong`</span><span class="sxs-lookup"><span data-stu-id="dec56-814">`S1` is `short` and `S2` is `ushort`, `uint`, or `ulong`</span></span>
   * <span data-ttu-id="dec56-815">`S1` je `int` a `S2` je `uint`, nebo `ulong`</span><span class="sxs-lookup"><span data-stu-id="dec56-815">`S1` is `int` and `S2` is `uint`, or `ulong`</span></span>
   * <span data-ttu-id="dec56-816">`S1` je `long` a `S2` je `ulong`</span><span class="sxs-lookup"><span data-stu-id="dec56-816">`S1` is `long` and `S2` is `ulong`</span></span>

#### <a name="overloading-in-generic-classes"></a><span data-ttu-id="dec56-817">Přetížení v obecné třídy</span><span class="sxs-lookup"><span data-stu-id="dec56-817">Overloading in generic classes</span></span>

<span data-ttu-id="dec56-818">Podpisy, jak je deklarován musí být jedinečný, je možné, že nahrazení argumentů typu výsledkem identickými podpisy.</span><span class="sxs-lookup"><span data-stu-id="dec56-818">While signatures as declared must be unique, it is possible that substitution of type arguments results in identical signatures.</span></span> <span data-ttu-id="dec56-819">V takových případech pravidla shody výše uvedené řešení přetížení vybere nejspecifičtější člena.</span><span class="sxs-lookup"><span data-stu-id="dec56-819">In such cases, the tie-breaking rules of overload resolution above will pick the most specific member.</span></span>

<span data-ttu-id="dec56-820">Následující příklady ukazují přetížení, které jsou platné a neplatné podle tohoto pravidla:</span><span class="sxs-lookup"><span data-stu-id="dec56-820">The following examples show overloads that are valid and invalid according to this rule:</span></span>

```csharp
interface I1<T> {...}

interface I2<T> {...}

class G1<U>
{
    int F1(U u);                  // Overload resolution for G<int>.F1
    int F1(int i);                // will pick non-generic

    void F2(I1<U> a);             // Valid overload
    void F2(I2<U> a);
}

class G2<U,V>
{
    void F3(U u, V v);            // Valid, but overload resolution for
    void F3(V v, U u);            // G2<int,int>.F3 will fail

    void F4(U u, I1<V> v);        // Valid, but overload resolution for    
    void F4(I1<V> v, U u);        // G2<I1<int>,int>.F4 will fail

    void F5(U u1, I1<V> v2);      // Valid overload
    void F5(V v1, U u2);

    void F6(ref U u);             // valid overload
    void F6(out V v);
}
```

### <a name="compile-time-checking-of-dynamic-overload-resolution"></a><span data-ttu-id="dec56-821">Kompilace kontrolu dynamické přetížení</span><span class="sxs-lookup"><span data-stu-id="dec56-821">Compile-time checking of dynamic overload resolution</span></span>

<span data-ttu-id="dec56-822">Pro operace s nejvíce dynamicky vazbou sadu možné kandidáty pro rozlišení neznámé v době kompilace.</span><span class="sxs-lookup"><span data-stu-id="dec56-822">For most dynamically bound operations the set of possible candidates for resolution is unknown at compile-time.</span></span> <span data-ttu-id="dec56-823">V některých případech ale sadu Release candidate je známý v době kompilace:</span><span class="sxs-lookup"><span data-stu-id="dec56-823">In certain cases, however the candidate set is known at compile-time:</span></span>

*  <span data-ttu-id="dec56-824">Volání statické metody s dynamických argumentů</span><span class="sxs-lookup"><span data-stu-id="dec56-824">Static method calls with dynamic arguments</span></span>
*  <span data-ttu-id="dec56-825">Pokud příjemce není výraz dynamického volání metod instance</span><span class="sxs-lookup"><span data-stu-id="dec56-825">Instance method calls where the receiver is not a dynamic expression</span></span>
*  <span data-ttu-id="dec56-826">Pokud příjemce není výraz dynamického volání indexeru</span><span class="sxs-lookup"><span data-stu-id="dec56-826">Indexer calls where the receiver is not a dynamic expression</span></span>
*  <span data-ttu-id="dec56-827">Volání konstruktoru pomocí dynamických argumentů</span><span class="sxs-lookup"><span data-stu-id="dec56-827">Constructor calls with dynamic arguments</span></span>

<span data-ttu-id="dec56-828">V těchto případech se provádí omezené kontrola v době kompilace pro každého kandidáta zobrazíte, pokud některý z nich může být použít v době běhu. Tato kontrola se skládá z následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="dec56-828">In these cases a limited compile-time check is performed for each candidate to see if any of them could possibly apply at run-time.This check consists of the following steps:</span></span>

*  <span data-ttu-id="dec56-829">Odvození částečného typu: Jakýkoli typ argumentu, který přímo nebo nepřímo není závislý na argumentu typu `dynamic` odvozena pomocí pravidel pro [odvození typu](expressions.md#type-inference).</span><span class="sxs-lookup"><span data-stu-id="dec56-829">Partial type inference: Any type argument that does not depend directly or indirectly on an argument of type `dynamic` is inferred using the rules of [Type inference](expressions.md#type-inference).</span></span> <span data-ttu-id="dec56-830">Zbývající argumenty typu nejsou známé.</span><span class="sxs-lookup"><span data-stu-id="dec56-830">The remaining type arguments are unknown.</span></span>
*  <span data-ttu-id="dec56-831">Kontrola použitelnosti částečné: Použitelnost proběhne podle [použít funkční člen](expressions.md#applicable-function-member), ale ignoruje parametry, jejichž typy neznámé.</span><span class="sxs-lookup"><span data-stu-id="dec56-831">Partial applicability check: Applicability is checked according to [Applicable function member](expressions.md#applicable-function-member), but ignoring parameters whose types are unknown.</span></span>
*  <span data-ttu-id="dec56-832">Pokud žádný kandidát předá tento test, dojde k chybě v době kompilace.</span><span class="sxs-lookup"><span data-stu-id="dec56-832">If no candidate passes this test, a compile-time error occurs.</span></span>

### <a name="function-member-invocation"></a><span data-ttu-id="dec56-833">Volání funkce člena</span><span class="sxs-lookup"><span data-stu-id="dec56-833">Function member invocation</span></span>

<span data-ttu-id="dec56-834">Tato část popisuje proces, který se provádí v době běhu k vyvolání konkrétní funkce člena.</span><span class="sxs-lookup"><span data-stu-id="dec56-834">This section describes the process that takes place at run-time to invoke a particular function member.</span></span> <span data-ttu-id="dec56-835">Předpokládá se, že proces doba vazby již určil konkrétního členu, který chcete vyvolat, případně s použitím rozlišení přetěžování na sadu Release candidate funkční členy.</span><span class="sxs-lookup"><span data-stu-id="dec56-835">It is assumed that a binding-time process has already determined the particular member to invoke, possibly by applying overload resolution to a set of candidate function members.</span></span>

<span data-ttu-id="dec56-836">Pro účely popisu vyvolání procesu jsou členy funkce rozdělit do dvou kategorií:</span><span class="sxs-lookup"><span data-stu-id="dec56-836">For purposes of describing the invocation process, function members are divided into two categories:</span></span>

*  <span data-ttu-id="dec56-837">Funkce statických členů.</span><span class="sxs-lookup"><span data-stu-id="dec56-837">Static function members.</span></span> <span data-ttu-id="dec56-838">Toto jsou konstruktory instancí, statické metody, přistupující objekty statické vlastnosti a operátory definované uživatelem.</span><span class="sxs-lookup"><span data-stu-id="dec56-838">These are instance constructors, static methods, static property accessors, and user-defined operators.</span></span> <span data-ttu-id="dec56-839">Funkce statických členů jsou vždy nevirtuální.</span><span class="sxs-lookup"><span data-stu-id="dec56-839">Static function members are always non-virtual.</span></span>
*  <span data-ttu-id="dec56-840">Členy instance funkce.</span><span class="sxs-lookup"><span data-stu-id="dec56-840">Instance function members.</span></span> <span data-ttu-id="dec56-841">Toto jsou metody instance, přistupující objekty vlastnosti instance a přístupové objekty indexeru.</span><span class="sxs-lookup"><span data-stu-id="dec56-841">These are instance methods, instance property accessors, and indexer accessors.</span></span> <span data-ttu-id="dec56-842">Členy instance funkce jsou nevirtuální nebo virtuální a jsou vždy vyvolána na konkrétní instanci.</span><span class="sxs-lookup"><span data-stu-id="dec56-842">Instance function members are either non-virtual or virtual, and are always invoked on a particular instance.</span></span> <span data-ttu-id="dec56-843">Instanci se počítají tak, že výraz instance a bude dostupný v rámci členských funkcí jako `this` ([tento přístup](expressions.md#this-access)).</span><span class="sxs-lookup"><span data-stu-id="dec56-843">The instance is computed by an instance expression, and it becomes accessible within the function member as `this` ([This access](expressions.md#this-access)).</span></span>

<span data-ttu-id="dec56-844">Zpracování za běhu volání členských funkcí se skládá z následujících kroků, kde `M` je členské funkce a v případě `M` je členem instance `E` je výraz instance:</span><span class="sxs-lookup"><span data-stu-id="dec56-844">The run-time processing of a function member invocation consists of the following steps, where `M` is the function member and, if `M` is an instance member, `E` is the instance expression:</span></span>

*  <span data-ttu-id="dec56-845">Pokud `M` členem statické funkce:</span><span class="sxs-lookup"><span data-stu-id="dec56-845">If `M` is a static function member:</span></span>
   * <span data-ttu-id="dec56-846">Seznam argumentů je vyhodnocen, jak je popsáno v [seznamy argumentů](expressions.md#argument-lists).</span><span class="sxs-lookup"><span data-stu-id="dec56-846">The argument list is evaluated as described in [Argument lists](expressions.md#argument-lists).</span></span>
   * <span data-ttu-id="dec56-847">`M` je vyvolána.</span><span class="sxs-lookup"><span data-stu-id="dec56-847">`M` is invoked.</span></span>

*  <span data-ttu-id="dec56-848">Pokud `M` je členem instance funkce deklarované v *value_type*:</span><span class="sxs-lookup"><span data-stu-id="dec56-848">If `M` is an instance function member declared in a *value_type*:</span></span>
   * <span data-ttu-id="dec56-849">`E` je vyhodnocen.</span><span class="sxs-lookup"><span data-stu-id="dec56-849">`E` is evaluated.</span></span> <span data-ttu-id="dec56-850">Je-li toto vyhodnocení způsobí výjimku, jsou spuštěny žádné další kroky.</span><span class="sxs-lookup"><span data-stu-id="dec56-850">If this evaluation causes an exception, then no further steps are executed.</span></span>
   * <span data-ttu-id="dec56-851">Pokud `E` není jsou klasifikovány jako proměnnou a dočasný místní proměnná `E`typ je vytvořen a hodnota `E` je přiřazená k této proměnné.</span><span class="sxs-lookup"><span data-stu-id="dec56-851">If `E` is not classified as a variable, then a temporary local variable of `E`'s type is created and the value of `E` is assigned to that variable.</span></span> <span data-ttu-id="dec56-852">`E` je pak překlasifikován sám jako odkaz na tento dočasný místní proměnné.</span><span class="sxs-lookup"><span data-stu-id="dec56-852">`E` is then reclassified as a reference to that temporary local variable.</span></span> <span data-ttu-id="dec56-853">Dočasná proměnná je dostupné jako `this` v rámci `M`, ale ne v žádným jiným způsobem.</span><span class="sxs-lookup"><span data-stu-id="dec56-853">The temporary variable is accessible as `this` within `M`, but not in any other way.</span></span> <span data-ttu-id="dec56-854">Proto pouze tehdy, když `E` je true proměnné je možné pro volajícího, aby sledovat změny, které `M` provede `this`.</span><span class="sxs-lookup"><span data-stu-id="dec56-854">Thus, only when `E` is a true variable is it possible for the caller to observe the changes that `M` makes to `this`.</span></span>
   * <span data-ttu-id="dec56-855">Seznam argumentů je vyhodnocen, jak je popsáno v [seznamy argumentů](expressions.md#argument-lists).</span><span class="sxs-lookup"><span data-stu-id="dec56-855">The argument list is evaluated as described in [Argument lists](expressions.md#argument-lists).</span></span>
   * <span data-ttu-id="dec56-856">`M` je vyvolána.</span><span class="sxs-lookup"><span data-stu-id="dec56-856">`M` is invoked.</span></span> <span data-ttu-id="dec56-857">Proměnná odkazuje `E` stane proměnná odkazuje `this`.</span><span class="sxs-lookup"><span data-stu-id="dec56-857">The variable referenced by `E` becomes the variable referenced by `this`.</span></span>

*  <span data-ttu-id="dec56-858">Pokud `M` je členem instance funkce deklarované v *reference_type*:</span><span class="sxs-lookup"><span data-stu-id="dec56-858">If `M` is an instance function member declared in a *reference_type*:</span></span>
   * <span data-ttu-id="dec56-859">`E` je vyhodnocen.</span><span class="sxs-lookup"><span data-stu-id="dec56-859">`E` is evaluated.</span></span> <span data-ttu-id="dec56-860">Je-li toto vyhodnocení způsobí výjimku, jsou spuštěny žádné další kroky.</span><span class="sxs-lookup"><span data-stu-id="dec56-860">If this evaluation causes an exception, then no further steps are executed.</span></span>
   * <span data-ttu-id="dec56-861">Seznam argumentů je vyhodnocen, jak je popsáno v [seznamy argumentů](expressions.md#argument-lists).</span><span class="sxs-lookup"><span data-stu-id="dec56-861">The argument list is evaluated as described in [Argument lists](expressions.md#argument-lists).</span></span>
   * <span data-ttu-id="dec56-862">Pokud typ `E` je *value_type*, převod na uzavřené určení ([zabalení převody](types.md#boxing-conversions)) se provádí za účelem převodu `E` na typ `object`, a `E` se považuje za Chcete-li být typu `object` v následujících krocích.</span><span class="sxs-lookup"><span data-stu-id="dec56-862">If the type of `E` is a *value_type*, a boxing conversion ([Boxing conversions](types.md#boxing-conversions)) is performed to convert `E` to type `object`, and `E` is considered to be of type `object` in the following steps.</span></span> <span data-ttu-id="dec56-863">V takovém případě `M` může být jenom členem `System.Object`.</span><span class="sxs-lookup"><span data-stu-id="dec56-863">In this case, `M` could only be a member of `System.Object`.</span></span>
   * <span data-ttu-id="dec56-864">Hodnota `E` je zaškrtnuté políčko platný.</span><span class="sxs-lookup"><span data-stu-id="dec56-864">The value of `E` is checked to be valid.</span></span> <span data-ttu-id="dec56-865">Pokud hodnota `E` je `null`, `System.NullReferenceException` je vyvolána, a že jsou provedeny žádné další kroky.</span><span class="sxs-lookup"><span data-stu-id="dec56-865">If the value of `E` is `null`, a `System.NullReferenceException` is thrown and no further steps are executed.</span></span>
   * <span data-ttu-id="dec56-866">Implementace členské funkce k vyvolání se určuje:</span><span class="sxs-lookup"><span data-stu-id="dec56-866">The function member implementation to invoke is determined:</span></span>
     * <span data-ttu-id="dec56-867">Pokud je typ vazby time `E` je rozhraní, členské funkce k vyvolání je implementace `M` poskytuje run-time typu instance odkazované procedurou `E`.</span><span class="sxs-lookup"><span data-stu-id="dec56-867">If the binding-time type of `E` is an interface, the function member to invoke is the implementation of `M` provided by the run-time type of the instance referenced by `E`.</span></span> <span data-ttu-id="dec56-868">Tento člen funkce je určeno použitím z pravidel mapování rozhraní ([mapování rozhraní](interfaces.md#interface-mapping)) k určení provádění `M` poskytuje run-time typu instance odkazované procedurou `E`.</span><span class="sxs-lookup"><span data-stu-id="dec56-868">This function member is determined by applying the interface mapping rules ([Interface mapping](interfaces.md#interface-mapping)) to determine the implementation of `M` provided by the run-time type of the instance referenced by `E`.</span></span>
     * <span data-ttu-id="dec56-869">Jinak, pokud `M` členem virtuální funkce, je člen funkce k vyvolání provádění `M` poskytuje run-time typu instance odkazované procedurou `E`.</span><span class="sxs-lookup"><span data-stu-id="dec56-869">Otherwise, if `M` is a virtual function member, the function member to invoke is the implementation of `M` provided by the run-time type of the instance referenced by `E`.</span></span> <span data-ttu-id="dec56-870">Tento člen funkce je určeno použitím z pravidel pro určení implementace Většina odvozených ([virtuální metody](classes.md#virtual-methods)) z `M` s ohledem na run-time typu instance odkazované procedurou `E`.</span><span class="sxs-lookup"><span data-stu-id="dec56-870">This function member is determined by applying the rules for determining the most derived implementation ([Virtual methods](classes.md#virtual-methods)) of `M` with respect to the run-time type of the instance referenced by `E`.</span></span>
     * <span data-ttu-id="dec56-871">V opačném případě `M` je člen bez virtuální funkce a členské funkce k vyvolání `M` samotný.</span><span class="sxs-lookup"><span data-stu-id="dec56-871">Otherwise, `M` is a non-virtual function member, and the function member to invoke is `M` itself.</span></span>
   * <span data-ttu-id="dec56-872">Implementace členské funkce nadefinovali v předchozím kroku, je vyvolána.</span><span class="sxs-lookup"><span data-stu-id="dec56-872">The function member implementation determined in the step above is invoked.</span></span> <span data-ttu-id="dec56-873">Objekt odkazovaný zadaným parametrem `E` stane objekt odkazovaný zadaným parametrem `this`.</span><span class="sxs-lookup"><span data-stu-id="dec56-873">The object referenced by `E` becomes the object referenced by `this`.</span></span>

#### <a name="invocations-on-boxed-instances"></a><span data-ttu-id="dec56-874">Volání na zabalený instancí</span><span class="sxs-lookup"><span data-stu-id="dec56-874">Invocations on boxed instances</span></span>

<span data-ttu-id="dec56-875">Členské funkce implementované v *value_type* lze vyvolat pomocí zabalený instanci, která *value_type* v následujících situacích:</span><span class="sxs-lookup"><span data-stu-id="dec56-875">A function member implemented in a *value_type* can be invoked through a boxed instance of that *value_type* in the following situations:</span></span>

*  <span data-ttu-id="dec56-876">Kdy je člen funkce `override` metody zděděné z typu `object` a je vyvolána prostřednictvím výrazu instance typu `object`.</span><span class="sxs-lookup"><span data-stu-id="dec56-876">When the function member is an `override` of a method inherited from type `object` and is invoked through an instance expression of type `object`.</span></span>
*  <span data-ttu-id="dec56-877">Když členské funkce je implementace funkce člena rozhraní a je vyvolána prostřednictvím výrazu instance *interface_type*.</span><span class="sxs-lookup"><span data-stu-id="dec56-877">When the function member is an implementation of an interface function member and is invoked through an instance expression of an *interface_type*.</span></span>
*  <span data-ttu-id="dec56-878">Členské funkce je při vyvolání prostřednictvím delegáta.</span><span class="sxs-lookup"><span data-stu-id="dec56-878">When the function member is invoked through a delegate.</span></span>

<span data-ttu-id="dec56-879">V těchto situacích pevně určené instance se považuje za obsahovat proměnnou *value_type*, a že tato proměnná bude proměnná odkazuje `this` v rámci volání členské funkce.</span><span class="sxs-lookup"><span data-stu-id="dec56-879">In these situations, the boxed instance is considered to contain a variable of the *value_type*, and this variable becomes the variable referenced by `this` within the function member invocation.</span></span> <span data-ttu-id="dec56-880">Konkrétně to znamená, že při vyvolání funkce člena pevně určené instance, je možné pro členské funkce ke změně hodnoty obsažené v pevně určené instance.</span><span class="sxs-lookup"><span data-stu-id="dec56-880">In particular, this means that when a function member is invoked on a boxed instance, it is possible for the function member to modify the value contained in the boxed instance.</span></span>

## <a name="primary-expressions"></a><span data-ttu-id="dec56-881">Primární výrazy</span><span class="sxs-lookup"><span data-stu-id="dec56-881">Primary expressions</span></span>

<span data-ttu-id="dec56-882">Primární výrazy zahrnují nejjednodušší typy výrazů.</span><span class="sxs-lookup"><span data-stu-id="dec56-882">Primary expressions include the simplest forms of expressions.</span></span>

```antlr
primary_expression
    : primary_no_array_creation_expression
    | array_creation_expression
    ;

primary_no_array_creation_expression
    : literal
    | interpolated_string_expression
    | simple_name
    | parenthesized_expression
    | member_access
    | invocation_expression
    | element_access
    | this_access
    | base_access
    | post_increment_expression
    | post_decrement_expression
    | object_creation_expression
    | delegate_creation_expression
    | anonymous_object_creation_expression
    | typeof_expression
    | checked_expression
    | unchecked_expression
    | default_value_expression
    | nameof_expression
    | anonymous_method_expression
    | primary_no_array_creation_expression_unsafe
    ;
```

<span data-ttu-id="dec56-883">Primární výrazy jsou rozděleny mezi *array_creation_expression*s a *primary_no_array_creation_expression*s.</span><span class="sxs-lookup"><span data-stu-id="dec56-883">Primary expressions are divided between *array_creation_expression*s and *primary_no_array_creation_expression*s.</span></span> <span data-ttu-id="dec56-884">Výraz vytvoření pole se zpracuje tímto způsobem, místo výpis společně s další jednoduchý výraz formuláře, umožňuje gramatiky tak potenciálně matoucí kódu, jako</span><span class="sxs-lookup"><span data-stu-id="dec56-884">Treating array-creation-expression in this way, rather than listing it along with the other simple expression forms, enables the grammar to disallow potentially confusing code such as</span></span>
```csharp
object o = new int[3][1];
```
<span data-ttu-id="dec56-885">které by jinak interpretován jako</span><span class="sxs-lookup"><span data-stu-id="dec56-885">which would otherwise be interpreted as</span></span>
```csharp
object o = (new int[3])[1];
```

### <a name="literals"></a><span data-ttu-id="dec56-886">Literály</span><span class="sxs-lookup"><span data-stu-id="dec56-886">Literals</span></span>

<span data-ttu-id="dec56-887">A *primary_expression* , který se skládá z *literálu* ([literály](lexical-structure.md#literals)) je klasifikován jako hodnotu.</span><span class="sxs-lookup"><span data-stu-id="dec56-887">A *primary_expression* that consists of a *literal* ([Literals](lexical-structure.md#literals)) is classified as a value.</span></span>


### <a name="interpolated-strings"></a><span data-ttu-id="dec56-888">Interpolované řetězce</span><span class="sxs-lookup"><span data-stu-id="dec56-888">Interpolated strings</span></span>

<span data-ttu-id="dec56-889">*Interpolated_string_expression* se skládá z `$` znaménka následovaného pravidelného či verbatim řetězcový literál, ve které otvory odděleny `{` a `}`, uzavřete výrazy a formátování specifikace.</span><span class="sxs-lookup"><span data-stu-id="dec56-889">An *interpolated_string_expression* consists of a `$` sign followed by a regular or verbatim string literal, wherein holes, delimited by `{` and `}`, enclose expressions and formatting specifications.</span></span> <span data-ttu-id="dec56-890">Interpolovaný řetězcový výraz ale je výsledkem *interpolated_string_literal* , které bylo přerušeno na jednotlivé tokeny, jak je popsáno v [interpolované řetězce literálů](lexical-structure.md#interpolated-string-literals).</span><span class="sxs-lookup"><span data-stu-id="dec56-890">An interpolated string expression is the result of an *interpolated_string_literal* that has been broken up into individual tokens, as described in [Interpolated string literals](lexical-structure.md#interpolated-string-literals).</span></span>

```antlr
interpolated_string_expression
    : '$' interpolated_regular_string
    | '$' interpolated_verbatim_string
    ;

interpolated_regular_string
    : interpolated_regular_string_whole
    | interpolated_regular_string_start interpolated_regular_string_body interpolated_regular_string_end
    ;

interpolated_regular_string_body
    : interpolation (interpolated_regular_string_mid interpolation)*
    ;

interpolation
    : expression
    | expression ',' constant_expression
    ;

interpolated_verbatim_string
    : interpolated_verbatim_string_whole
    | interpolated_verbatim_string_start interpolated_verbatim_string_body interpolated_verbatim_string_end
    ;

interpolated_verbatim_string_body
    : interpolation (interpolated_verbatim_string_mid interpolation)+
    ;
```

<span data-ttu-id="dec56-891">*Constant_expression* interpolaci musí mít implicitní převod na `int`.</span><span class="sxs-lookup"><span data-stu-id="dec56-891">The *constant_expression* in an interpolation must have an implicit conversion to `int`.</span></span>

<span data-ttu-id="dec56-892">*Interpolated_string_expression* klasifikovaný jako hodnotu.</span><span class="sxs-lookup"><span data-stu-id="dec56-892">An *interpolated_string_expression* is classified as a value.</span></span> <span data-ttu-id="dec56-893">Pokud je okamžitě převést na `System.IFormattable` nebo `System.FormattableString` s konverzi implicitní interpolovaném řetězci ([implicitní interpolované řetězce převody](conversions.md#implicit-interpolated-string-conversions)), má interpolovaný řetězcový výraz tohoto typu.</span><span class="sxs-lookup"><span data-stu-id="dec56-893">If it is immediately converted to `System.IFormattable` or `System.FormattableString` with an implicit interpolated string conversion ([Implicit interpolated string conversions](conversions.md#implicit-interpolated-string-conversions)), the interpolated string expression has that type.</span></span> <span data-ttu-id="dec56-894">V opačném případě má typ `string`.</span><span class="sxs-lookup"><span data-stu-id="dec56-894">Otherwise, it has the type `string`.</span></span>

<span data-ttu-id="dec56-895">Pokud je typem interpolovaného řetězce `System.IFormattable` nebo `System.FormattableString`, význam je volání `System.Runtime.CompilerServices.FormattableStringFactory.Create`.</span><span class="sxs-lookup"><span data-stu-id="dec56-895">If the type of an interpolated string is `System.IFormattable` or `System.FormattableString`, the meaning is a call to `System.Runtime.CompilerServices.FormattableStringFactory.Create`.</span></span> <span data-ttu-id="dec56-896">Pokud je typ `string`, význam výrazu je volání `string.Format`.</span><span class="sxs-lookup"><span data-stu-id="dec56-896">If the type is `string`, the meaning of the expression is a call to `string.Format`.</span></span> <span data-ttu-id="dec56-897">V obou případech se seznamem argumentů volání se skládá z formátu řetězcový literál s zástupné symboly pro každý interpolace a argument pro každý výraz odpovídá místo zástupné znaky.</span><span class="sxs-lookup"><span data-stu-id="dec56-897">In both cases, the argument list of the call consists of a format string literal with placeholders for each interpolation, and an argument for each expression corresponding to the place holders.</span></span>

<span data-ttu-id="dec56-898">Formát řetězcového literálu se vypočte takto, kde `N` je počet interpolace v *interpolated_string_expression*:</span><span class="sxs-lookup"><span data-stu-id="dec56-898">The format string literal is constructed as follows, where `N` is the number of interpolations in the *interpolated_string_expression*:</span></span>

*  <span data-ttu-id="dec56-899">Pokud *interpolated_regular_string_whole* nebo *interpolated_verbatim_string_whole* následuje `$` podepsat, pak tento token je literál řetězce formátu.</span><span class="sxs-lookup"><span data-stu-id="dec56-899">If an *interpolated_regular_string_whole* or an *interpolated_verbatim_string_whole* follows the `$` sign, then the format string literal is that token.</span></span>
*  <span data-ttu-id="dec56-900">Formát řetězcového literálu, jinak se skládá z:</span><span class="sxs-lookup"><span data-stu-id="dec56-900">Otherwise, the format string literal consists of:</span></span> 
   *  <span data-ttu-id="dec56-901">První *interpolated_regular_string_start* nebo *interpolated_verbatim_string_start*</span><span class="sxs-lookup"><span data-stu-id="dec56-901">First the *interpolated_regular_string_start* or *interpolated_verbatim_string_start*</span></span>
   *  <span data-ttu-id="dec56-902">Pak pro každé číslo `I` z `0` k `N-1`:</span><span class="sxs-lookup"><span data-stu-id="dec56-902">Then for each number `I` from `0` to `N-1`:</span></span> 
      * <span data-ttu-id="dec56-903">Desítkový zápis `I`</span><span class="sxs-lookup"><span data-stu-id="dec56-903">The decimal representation of `I`</span></span>
      * <span data-ttu-id="dec56-904">Když se poté odpovídající *interpolace* má *constant_expression*, `,` (čárka), za nímž následuje Desítková vyjádření hodnoty *constant_expression*</span><span class="sxs-lookup"><span data-stu-id="dec56-904">Then, if the corresponding *interpolation* has a *constant_expression*, a `,` (comma) followed by the decimal representation of the value of the *constant_expression*</span></span>
      * <span data-ttu-id="dec56-905">Pak bude *interpolated_regular_string_mid*, *interpolated_regular_string_end*, *interpolated_verbatim_string_mid* nebo *interpolated_ verbatim_string_end* hned za odpovídající interpolace.</span><span class="sxs-lookup"><span data-stu-id="dec56-905">Then the *interpolated_regular_string_mid*, *interpolated_regular_string_end*, *interpolated_verbatim_string_mid* or *interpolated_verbatim_string_end* immediately following the corresponding interpolation.</span></span>

<span data-ttu-id="dec56-906">Další argumenty jsou jednoduše *výrazy* z *interpolace* (pokud existuje), v pořadí.</span><span class="sxs-lookup"><span data-stu-id="dec56-906">The subsequent arguments are simply the *expressions* from the *interpolations* (if any), in order.</span></span>

<span data-ttu-id="dec56-907">TODO: příklady.</span><span class="sxs-lookup"><span data-stu-id="dec56-907">TODO: examples.</span></span>


### <a name="simple-names"></a><span data-ttu-id="dec56-908">Jednoduché názvy</span><span class="sxs-lookup"><span data-stu-id="dec56-908">Simple names</span></span>

<span data-ttu-id="dec56-909">A *simple_name* identifikátor, může volitelně následovat seznam argumentů typu se skládá ze:</span><span class="sxs-lookup"><span data-stu-id="dec56-909">A *simple_name* consists of an identifier, optionally followed by a type argument list:</span></span>

```antlr
simple_name
    : identifier type_argument_list?
    ;
```

<span data-ttu-id="dec56-910">A *simple_name* je buď ve formátu `I` nebo formuláře `I<A1,...,Ak>`, kde `I` je jediným identifikátorem a `<A1,...,Ak>` je volitelný *type_argument_list*.</span><span class="sxs-lookup"><span data-stu-id="dec56-910">A *simple_name* is either of the form `I` or of the form `I<A1,...,Ak>`, where `I` is a single identifier and `<A1,...,Ak>` is an optional *type_argument_list*.</span></span> <span data-ttu-id="dec56-911">Pokud ne *type_argument_list* je zadán, vezměte v úvahu `K` být nula.</span><span class="sxs-lookup"><span data-stu-id="dec56-911">When no *type_argument_list* is specified, consider `K` to be zero.</span></span> <span data-ttu-id="dec56-912">*Simple_name* je vyhodnocen a klasifikovat následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="dec56-912">The *simple_name* is evaluated and classified as follows:</span></span>

*  <span data-ttu-id="dec56-913">Pokud `K` je nula a *simple_name* se zobrazí v rámci *bloku* a, pokud *bloku*společnosti (nebo nadřazeném *bloku*společnosti) místní Deklarace proměnné místa ([deklarace](basic-concepts.md#declarations)) obsahuje místní proměnná, parametr nebo konstanta s názvem `I`, pak bude *simple_name* odkazuje na tuto místní proměnnou parametr nebo konstantu a je klasifikován jako na proměnnou nebo hodnotu.</span><span class="sxs-lookup"><span data-stu-id="dec56-913">If `K` is zero and the *simple_name* appears within a *block* and if the *block*'s (or an enclosing *block*'s) local variable declaration space ([Declarations](basic-concepts.md#declarations)) contains a local variable, parameter or constant with name `I`, then the *simple_name* refers to that local variable, parameter or constant and is classified as a variable or value.</span></span>
*  <span data-ttu-id="dec56-914">Pokud `K` je nula a *simple_name* se zobrazí v těle deklarace obecné metody a pokud tato deklarace obsahuje parametr typu s názvem `I`, pak bude *simple_name*odkazuje na parametr typu.</span><span class="sxs-lookup"><span data-stu-id="dec56-914">If `K` is zero and the *simple_name* appears within the body of a generic method declaration and if that declaration includes a type parameter with name `I`, then the *simple_name* refers to that type parameter.</span></span>
*  <span data-ttu-id="dec56-915">Jinak pro každý typ instance `T` ([typ instance](classes.md#the-instance-type)), počínaje instance typu bezprostředně vložená deklarace typu a budete pokračovat s typem instance každé nadřazené třídu nebo strukturu deklarace (pokud existuje):</span><span class="sxs-lookup"><span data-stu-id="dec56-915">Otherwise, for each instance type `T` ([The instance type](classes.md#the-instance-type)), starting with the instance type of the immediately enclosing type declaration and continuing with the instance type of each enclosing class or struct declaration (if any):</span></span>
   *  <span data-ttu-id="dec56-916">Pokud `K` je nula a deklarace `T` obsahuje parametr typu s názvem `I`, pak bude *simple_name* odkazuje na parametr typu.</span><span class="sxs-lookup"><span data-stu-id="dec56-916">If `K` is zero and the declaration of `T` includes a type parameter with name `I`, then the *simple_name* refers to that type parameter.</span></span>
   *  <span data-ttu-id="dec56-917">Jinak, pokud člen vyhledávání ([člen vyhledávání](expressions.md#member-lookup)) z `I` v `T` s `K`  argumenty typu vytváří shoda:</span><span class="sxs-lookup"><span data-stu-id="dec56-917">Otherwise, if a member lookup ([Member lookup](expressions.md#member-lookup)) of `I` in `T` with `K` type arguments produces a match:</span></span>
      * <span data-ttu-id="dec56-918">Pokud `T` je typ instance bezprostředně nadřazeného typu třídy nebo struktury a vyhledávání označuje jeden nebo více metod, výsledek je skupina metoda s výrazem přidruženou instanci `this`.</span><span class="sxs-lookup"><span data-stu-id="dec56-918">If `T` is the instance type of the immediately enclosing class or struct type and the lookup identifies one or more methods, the result is a method group with an associated instance expression of `this`.</span></span> <span data-ttu-id="dec56-919">Pokud byl zadán seznam argumentů typu se používá při volání obecné metody ([volání metod](expressions.md#method-invocations)).</span><span class="sxs-lookup"><span data-stu-id="dec56-919">If a type argument list was specified, it is used in calling a generic method ([Method invocations](expressions.md#method-invocations)).</span></span>
      * <span data-ttu-id="dec56-920">Jinak, pokud `T` je typ instance bezprostředně nadřazeného typu třídy nebo struktury, pokud vyhledávání identifikuje člena instance, a pokud odkaz nastane v těle konstruktoru instance, metoda instance nebo přístupový objekt instance výsledek je stejný jako přístup ke členu ([přístup ke členu](expressions.md#member-access)) ve formátu `this.I`.</span><span class="sxs-lookup"><span data-stu-id="dec56-920">Otherwise, if `T` is the instance type of the immediately enclosing class or struct type, if the lookup identifies an instance member, and if the reference occurs within the body of an instance constructor, an instance method, or an instance accessor, the result is the same as a member access ([Member access](expressions.md#member-access)) of the form `this.I`.</span></span> <span data-ttu-id="dec56-921">K tomu může dojít pouze při `K` je nula.</span><span class="sxs-lookup"><span data-stu-id="dec56-921">This can only happen when `K` is zero.</span></span>
      * <span data-ttu-id="dec56-922">Jinak, výsledek je stejný jako přístup ke členu ([přístup ke členu](expressions.md#member-access)) ve formátu `T.I` nebo `T.I<A1,...,Ak>`.</span><span class="sxs-lookup"><span data-stu-id="dec56-922">Otherwise, the result is the same as a member access ([Member access](expressions.md#member-access)) of the form `T.I` or `T.I<A1,...,Ak>`.</span></span> <span data-ttu-id="dec56-923">V takovém případě je chyba doba vazby pro *simple_name* k odkazování na člena instance.</span><span class="sxs-lookup"><span data-stu-id="dec56-923">In this case, it is a binding-time error for the *simple_name* to refer to an instance member.</span></span>

*  <span data-ttu-id="dec56-924">Jinak pro každý obor názvů `N`začíná s oborem názvů, ve kterém *simple_name* dojde, budete pokračovat s každý nadřazený obor názvů (pokud existuje) a konče globální obor názvů následující kroky jsou vyhodnocen, dokud se entita nachází:</span><span class="sxs-lookup"><span data-stu-id="dec56-924">Otherwise, for each namespace `N`, starting with the namespace in which the *simple_name* occurs, continuing with each enclosing namespace (if any), and ending with the global namespace, the following steps are evaluated until an entity is located:</span></span>
   *  <span data-ttu-id="dec56-925">Pokud `K` je nula a `I` je název oboru názvů v `N`, pak:</span><span class="sxs-lookup"><span data-stu-id="dec56-925">If `K` is zero and `I` is the name of a namespace in `N`, then:</span></span>
      * <span data-ttu-id="dec56-926">Pokud umístění ve kterém *simple_name* dojde k není uzavřen v deklaraci oboru názvů pro `N` a obsahuje deklaraci oboru názvů *extern_alias_directive* nebo  *using_alias_directive* , která přidruží název `I` s oborem názvů nebo typ, pak bude *simple_name* je nejednoznačný a dojde k chybě kompilace.</span><span class="sxs-lookup"><span data-stu-id="dec56-926">If the location where the *simple_name* occurs is enclosed by a namespace declaration for `N` and the namespace declaration contains an *extern_alias_directive* or *using_alias_directive* that associates the name `I` with a namespace or type, then the *simple_name* is ambiguous and a compile-time error occurs.</span></span>
      * <span data-ttu-id="dec56-927">V opačném případě *simple_name* odkazuje na obor názvů s názvem `I` v `N`.</span><span class="sxs-lookup"><span data-stu-id="dec56-927">Otherwise, the *simple_name* refers to the namespace named `I` in `N`.</span></span>
   *  <span data-ttu-id="dec56-928">Jinak, pokud `N` obsahuje přístupného typu s názvem `I` a `K`  parametry typu, pak:</span><span class="sxs-lookup"><span data-stu-id="dec56-928">Otherwise, if `N` contains an accessible type having name `I` and `K` type parameters, then:</span></span>
      * <span data-ttu-id="dec56-929">Pokud `K` je nula a umístění, kde *simple_name* dojde k není uzavřen v deklaraci oboru názvů pro `N` a obsahuje deklaraci oboru názvů *extern_alias_directive*nebo *using_alias_directive* , která přidruží název `I` s oborem názvů nebo typ, pak bude *simple_name* je nejednoznačný a dojde k chybě kompilace.</span><span class="sxs-lookup"><span data-stu-id="dec56-929">If `K` is zero and the location where the *simple_name* occurs is enclosed by a namespace declaration for `N` and the namespace declaration contains an *extern_alias_directive* or *using_alias_directive* that associates the name `I` with a namespace or type, then the *simple_name* is ambiguous and a compile-time error occurs.</span></span>
      * <span data-ttu-id="dec56-930">V opačném případě *namespace_or_type_name* odkazuje na typ vytvořený s argumenty daného typu.</span><span class="sxs-lookup"><span data-stu-id="dec56-930">Otherwise, the *namespace_or_type_name* refers to the type constructed with the given type arguments.</span></span>
   *  <span data-ttu-id="dec56-931">Jinak, pokud umístění ve kterém *simple_name* dojde k není uzavřen v deklaraci oboru názvů pro `N`:</span><span class="sxs-lookup"><span data-stu-id="dec56-931">Otherwise, if the location where the *simple_name* occurs is enclosed by a namespace declaration for `N`:</span></span>
      * <span data-ttu-id="dec56-932">Pokud `K` je nula a obsahuje deklaraci oboru názvů *extern_alias_directive* nebo *using_alias_directive* , která přidruží název `I` s importovaným oborem názvů nebo typ, pak bude *simple_name* odkazuje na tento obor názvů nebo typ.</span><span class="sxs-lookup"><span data-stu-id="dec56-932">If `K` is zero and the namespace declaration contains an *extern_alias_directive* or *using_alias_directive* that associates the name `I` with an imported namespace or type, then the *simple_name* refers to that namespace or type.</span></span>
      * <span data-ttu-id="dec56-933">Jinak, pokud deklarace oborů názvů a typ importované tímto seznamem *using_namespace_directive*s a *using_static_directive*s deklarace oboru názvů obsahovat právě jeden typ přístupné nebo rozšíření statický člen s názvem `I` a `K`  parametry typu, pak bude *simple_name* odkazuje na tento typ nebo člen vytvořený s argumenty daného typu.</span><span class="sxs-lookup"><span data-stu-id="dec56-933">Otherwise, if the namespaces and type declarations imported by the *using_namespace_directive*s and *using_static_directive*s of the namespace declaration contain exactly one accessible type or non-extension static member having name `I` and `K` type parameters, then the *simple_name* refers to that type or member constructed with the given type arguments.</span></span>
      * <span data-ttu-id="dec56-934">Jinak, pokud jsou obory názvů a typy importoval *using_namespace_directive*s deklarace oboru názvů obsahovat více než jeden dostupný typ nebo statický člen rozšiřující metoda s názvem `I` a `K`  parametry typu, pak bude *simple_name* je nejednoznačný a dojde k chybě.</span><span class="sxs-lookup"><span data-stu-id="dec56-934">Otherwise, if the namespaces and types imported by the *using_namespace_directive*s of the namespace declaration contain more than one accessible type or non-extension-method static member having name `I` and `K` type parameters, then the *simple_name* is ambiguous and an error occurs.</span></span>

   <span data-ttu-id="dec56-935">Mějte na paměti, že celý tento krok je přesně paralelní na odpovídající krok zpracování *namespace_or_type_name* ([Namespace a zadejte názvy](basic-concepts.md#namespace-and-type-names)).</span><span class="sxs-lookup"><span data-stu-id="dec56-935">Note that this entire step is exactly parallel to the corresponding step in the processing of a *namespace_or_type_name* ([Namespace and type names](basic-concepts.md#namespace-and-type-names)).</span></span>

*  <span data-ttu-id="dec56-936">V opačném případě *simple_name* je nedefinovaný a dojde k chybě kompilace.</span><span class="sxs-lookup"><span data-stu-id="dec56-936">Otherwise, the *simple_name* is undefined and a compile-time error occurs.</span></span>


### <a name="parenthesized-expressions"></a><span data-ttu-id="dec56-937">Výrazy v závorkách.</span><span class="sxs-lookup"><span data-stu-id="dec56-937">Parenthesized expressions</span></span>

<span data-ttu-id="dec56-938">A *parenthesized_expression* se skládá ze *výraz* uzavřený do závorek.</span><span class="sxs-lookup"><span data-stu-id="dec56-938">A *parenthesized_expression* consists of an *expression* enclosed in parentheses.</span></span>

```antlr
parenthesized_expression
    : '(' expression ')'
    ;
```

<span data-ttu-id="dec56-939">A *parenthesized_expression* se vyhodnocuje na základě vyhodnocení *výraz* v závorkách.</span><span class="sxs-lookup"><span data-stu-id="dec56-939">A *parenthesized_expression* is evaluated by evaluating the *expression* within the parentheses.</span></span> <span data-ttu-id="dec56-940">Pokud *výraz* v závorkách označuje obor názvů nebo typ, dojde k chybě kompilace.</span><span class="sxs-lookup"><span data-stu-id="dec56-940">If the *expression* within the parentheses denotes a namespace or type, a compile-time error occurs.</span></span> <span data-ttu-id="dec56-941">V opačném případě výsledek *parenthesized_expression* je výsledkem vyhodnocení uzavřeného *výraz*.</span><span class="sxs-lookup"><span data-stu-id="dec56-941">Otherwise, the result of the *parenthesized_expression* is the result of the evaluation of the contained *expression*.</span></span>

### <a name="member-access"></a><span data-ttu-id="dec56-942">Přístup ke členům</span><span class="sxs-lookup"><span data-stu-id="dec56-942">Member access</span></span>

<span data-ttu-id="dec56-943">A *member_access* se skládá z *primary_expression*, *predefined_type*, nebo *qualified_alias_member*následovaný "`.`"token a po něm *identifikátor*volitelně následovaný *type_argument_list*.</span><span class="sxs-lookup"><span data-stu-id="dec56-943">A *member_access* consists of a *primary_expression*, a *predefined_type*, or a *qualified_alias_member*, followed by a "`.`" token, followed by an *identifier*, optionally followed by a *type_argument_list*.</span></span>

```antlr
member_access
    : primary_expression '.' identifier type_argument_list?
    | predefined_type '.' identifier type_argument_list?
    | qualified_alias_member '.' identifier
    ;

predefined_type
    : 'bool'   | 'byte'  | 'char'  | 'decimal' | 'double' | 'float' | 'int' | 'long'
    | 'object' | 'sbyte' | 'short' | 'string'  | 'uint'   | 'ulong' | 'ushort'
    ;
```

<span data-ttu-id="dec56-944">*Qualified_alias_member* produkčním prostředí je definována v [Namespace alias kvalifikátory](namespaces.md#namespace-alias-qualifiers).</span><span class="sxs-lookup"><span data-stu-id="dec56-944">The *qualified_alias_member* production is defined in [Namespace alias qualifiers](namespaces.md#namespace-alias-qualifiers).</span></span>

<span data-ttu-id="dec56-945">A *member_access* je buď ve formátu `E.I` nebo formuláře `E.I<A1, ..., Ak>`, kde `E` je primární výraz, `I` je jediným identifikátorem a `<A1, ..., Ak>` je volitelný  *type_argument_list*.</span><span class="sxs-lookup"><span data-stu-id="dec56-945">A *member_access* is either of the form `E.I` or of the form `E.I<A1, ..., Ak>`, where `E` is a primary-expression, `I` is a single identifier and `<A1, ..., Ak>` is an optional *type_argument_list*.</span></span> <span data-ttu-id="dec56-946">Pokud ne *type_argument_list* je zadán, vezměte v úvahu `K` být nula.</span><span class="sxs-lookup"><span data-stu-id="dec56-946">When no *type_argument_list* is specified, consider `K` to be zero.</span></span>

<span data-ttu-id="dec56-947">A *member_access* s *primary_expression* typu `dynamic` dynamicky vázán ([dynamické vazby](expressions.md#dynamic-binding)).</span><span class="sxs-lookup"><span data-stu-id="dec56-947">A *member_access* with a *primary_expression* of type `dynamic` is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)).</span></span> <span data-ttu-id="dec56-948">V tomto případě kompilátor klasifikuje přístup ke členu jako přístup k vlastnosti typu `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="dec56-948">In this case the compiler classifies the member access as a property access of type `dynamic`.</span></span> <span data-ttu-id="dec56-949">Pravidly dole určit význam *member_access* se pak použijí v době běhu, místo kompilaci typu run-time typu *primary_expression*.</span><span class="sxs-lookup"><span data-stu-id="dec56-949">The rules below to determine the meaning of the *member_access* are then applied at run-time, using the run-time type instead of the compile-time type of the *primary_expression*.</span></span> <span data-ttu-id="dec56-950">Pokud tato klasifikace za běhu vede na skupinu metod, pak musí být přístup ke členu *primary_expression* ze *invocation_expression*.</span><span class="sxs-lookup"><span data-stu-id="dec56-950">If this run-time classification leads to a method group, then the member access must be the *primary_expression* of an *invocation_expression*.</span></span>

<span data-ttu-id="dec56-951">*Member_access* je vyhodnocen a klasifikovat následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="dec56-951">The *member_access* is evaluated and classified as follows:</span></span>

*  <span data-ttu-id="dec56-952">Pokud `K` je nula a `E` je obor názvů a `E` obsahuje vnořené oboru názvů s názvem `I`, výsledek je tento obor názvů.</span><span class="sxs-lookup"><span data-stu-id="dec56-952">If `K` is zero and `E` is a namespace and `E` contains a nested namespace with name `I`, then the result is that namespace.</span></span>
*  <span data-ttu-id="dec56-953">Jinak, pokud `E` je obor názvů a `E` obsahuje přístupného typu s názvem `I` a `K`  zadejte parametry, výsledkem je vytvořen s daným typem argumenty typu.</span><span class="sxs-lookup"><span data-stu-id="dec56-953">Otherwise, if `E` is a namespace and `E` contains an accessible type having name `I` and `K` type parameters, then the result is that type constructed with the given type arguments.</span></span>
*  <span data-ttu-id="dec56-954">Pokud `E` je *predefined_type* nebo *primary_expression* klasifikovat jako typ, pokud `E` se nejedná o parametr typu a pokud člen vyhledávání ([člen vyhledávání](expressions.md#member-lookup)) z `I` v `E` s `K`  parametry typu vytváří shodu, pak `E.I` je vyhodnocen a klasifikovat následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="dec56-954">If `E` is a *predefined_type* or a *primary_expression* classified as a type, if `E` is not a type parameter, and if a member lookup ([Member lookup](expressions.md#member-lookup)) of `I` in `E` with `K` type parameters produces a match, then `E.I` is evaluated and classified as follows:</span></span>
   *  <span data-ttu-id="dec56-955">Pokud `I` identifikuje typ, výsledek je vytvořen s daným typem argumenty typu.</span><span class="sxs-lookup"><span data-stu-id="dec56-955">If `I` identifies a type, then the result is that type constructed with the given type arguments.</span></span>
   *  <span data-ttu-id="dec56-956">Pokud `I` identifikuje jednu nebo více metod, výsledek je skupinu metod s žádný výraz přidruženou instanci.</span><span class="sxs-lookup"><span data-stu-id="dec56-956">If `I` identifies one or more methods, then the result is a method group with no associated instance expression.</span></span> <span data-ttu-id="dec56-957">Pokud byl zadán seznam argumentů typu se používá při volání obecné metody ([volání metod](expressions.md#method-invocations)).</span><span class="sxs-lookup"><span data-stu-id="dec56-957">If a type argument list was specified, it is used in calling a generic method ([Method invocations](expressions.md#method-invocations)).</span></span>
   *  <span data-ttu-id="dec56-958">Pokud `I` identifikuje `static` vlastnost a potom výsledek je přístup k vlastnosti s žádný výraz přidruženou instanci.</span><span class="sxs-lookup"><span data-stu-id="dec56-958">If `I` identifies a `static` property, then the result is a property access with no associated instance expression.</span></span>
   *  <span data-ttu-id="dec56-959">Pokud `I` identifikuje `static` pole:</span><span class="sxs-lookup"><span data-stu-id="dec56-959">If `I` identifies a `static` field:</span></span>
      * <span data-ttu-id="dec56-960">Pokud je datové pole `readonly` a odkaz proběhne mimo statický konstruktor třídy nebo struktury, ve kterém je deklarována pole a výsledkem je hodnota, konkrétně hodnota statické pole `I` v `E`.</span><span class="sxs-lookup"><span data-stu-id="dec56-960">If the field is `readonly` and the reference occurs outside the static constructor of the class or struct in which the field is declared, then the result is a value, namely the value of the static field `I` in `E`.</span></span>
      * <span data-ttu-id="dec56-961">Jinak, výsledek je proměnná, konkrétně statické pole `I` v `E`.</span><span class="sxs-lookup"><span data-stu-id="dec56-961">Otherwise, the result is a variable, namely the static field `I` in `E`.</span></span>
   *  <span data-ttu-id="dec56-962">Pokud `I` identifikuje `static` události:</span><span class="sxs-lookup"><span data-stu-id="dec56-962">If `I` identifies a `static` event:</span></span>
      * <span data-ttu-id="dec56-963">Pokud dojde k odkazu v rámci třídy nebo struktury, ve kterém je deklarována jako události a události byla deklarovaná bez *event_accessor_declarations* ([události](classes.md#events)), pak `E.I` se právě zpracovává. jakoby `I` byly statické pole.</span><span class="sxs-lookup"><span data-stu-id="dec56-963">If the reference occurs within the class or struct in which the event is declared, and the event was declared without *event_accessor_declarations* ([Events](classes.md#events)), then `E.I` is processed exactly as if `I` were a static field.</span></span>
      * <span data-ttu-id="dec56-964">Jinak výsledkem je přístup k události pomocí žádný výraz přidruženou instanci.</span><span class="sxs-lookup"><span data-stu-id="dec56-964">Otherwise, the result is an event access with no associated instance expression.</span></span>
   *  <span data-ttu-id="dec56-965">Pokud `I` identifikuje konstantě, a výsledkem je hodnota, konkrétně hodnota této konstanty.</span><span class="sxs-lookup"><span data-stu-id="dec56-965">If `I` identifies a constant, then the result is a value, namely the value of that constant.</span></span>
    * <span data-ttu-id="dec56-966">Pokud `I` identifikuje na člena výčtu a výsledkem je hodnota, konkrétně hodnotu tohoto člena výčtu.</span><span class="sxs-lookup"><span data-stu-id="dec56-966">If `I` identifies an enumeration member, then the result is a value, namely the value of that enumeration member.</span></span>
    * <span data-ttu-id="dec56-967">V opačném případě `E.I` je odkaz na člena je neplatný a dojde k chybě kompilace.</span><span class="sxs-lookup"><span data-stu-id="dec56-967">Otherwise, `E.I` is an invalid member reference, and a compile-time error occurs.</span></span>
*  <span data-ttu-id="dec56-968">Pokud `E` přístup k vlastnostem, přístup indexeru, proměnné nebo hodnotu, jehož typ je `T`a člen vyhledávání ([člen vyhledávání](expressions.md#member-lookup)) z `I` v `T` s `K`  argumenty typu vytváří shodu, pak `E.I` je vyhodnocen a klasifikovat následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="dec56-968">If `E` is a property access, indexer access, variable, or value, the type of which is `T`, and a member lookup ([Member lookup](expressions.md#member-lookup)) of `I` in `T` with `K` type arguments produces a match, then `E.I` is evaluated and classified as follows:</span></span>
   *  <span data-ttu-id="dec56-969">První, pokud `E` vlastnost nebo indexer přístup, je hodnota vlastnosti nebo získat přístup indexeru ([hodnot výrazů](expressions.md#values-of-expressions)) a `E` přeřazených jako hodnotu.</span><span class="sxs-lookup"><span data-stu-id="dec56-969">First, if `E` is a property or indexer access, then the value of the property or indexer access is obtained ([Values of expressions](expressions.md#values-of-expressions)) and `E` is reclassified as a value.</span></span>
   *  <span data-ttu-id="dec56-970">Pokud `I` identifikuje jednu nebo více metod, výsledek je skupinu metod s výrazem přidruženou instanci `E`.</span><span class="sxs-lookup"><span data-stu-id="dec56-970">If `I` identifies one or more methods, then the result is a method group with an associated instance expression of `E`.</span></span> <span data-ttu-id="dec56-971">Pokud byl zadán seznam argumentů typu se používá při volání obecné metody ([volání metod](expressions.md#method-invocations)).</span><span class="sxs-lookup"><span data-stu-id="dec56-971">If a type argument list was specified, it is used in calling a generic method ([Method invocations](expressions.md#method-invocations)).</span></span>
   *  <span data-ttu-id="dec56-972">Pokud `I` identifikuje vlastnost instance,</span><span class="sxs-lookup"><span data-stu-id="dec56-972">If `I` identifies an instance property,</span></span>
      * <span data-ttu-id="dec56-973">Pokud `E` je `this`, `I` identifikuje automaticky implementované vlastnosti ([automaticky implementované vlastnosti](classes.md#automatically-implemented-properties)) bez setter a odkaz nastane v rámci konstruktoru instance pro Typ třídy nebo struktury `T`, výsledek je proměnná, konkrétně pole Skrytá zálohování pro automatickou vlastnost Dal `I` v instanci `T` určené pomocí `this`.</span><span class="sxs-lookup"><span data-stu-id="dec56-973">If `E` is `this`, `I` identifies an automatically implemented property ([Automatically implemented properties](classes.md#automatically-implemented-properties)) without a setter, and the reference occurs within an instance constructor for a class or struct type `T`, then the result is a variable, namely the hidden backing field for the auto-property given by `I` in the instance of `T` given by `this`.</span></span>
      * <span data-ttu-id="dec56-974">Jinak, výsledkem je přístup k vlastnosti s výrazem přidruženou instanci `E`.</span><span class="sxs-lookup"><span data-stu-id="dec56-974">Otherwise, the result is a property access with an associated instance expression of `E`.</span></span>
   *  <span data-ttu-id="dec56-975">Pokud `T` je *class_type* a `I` pole instance, která identifikuje *class_type*:</span><span class="sxs-lookup"><span data-stu-id="dec56-975">If `T` is a *class_type* and `I` identifies an instance field of that *class_type*:</span></span>
      * <span data-ttu-id="dec56-976">Pokud hodnota `E` je `null`, o `System.NullReferenceException` je vyvolána výjimka.</span><span class="sxs-lookup"><span data-stu-id="dec56-976">If the value of `E` is `null`, then a `System.NullReferenceException` is thrown.</span></span>
      * <span data-ttu-id="dec56-977">Jinak, pokud je datové pole `readonly` a odkaz proběhne mimo konstruktor instance třídy, ve kterém je deklarována pole a výsledkem je hodnota, konkrétně hodnota pole `I` v objekt odkazovaný zadaným parametrem `E`.</span><span class="sxs-lookup"><span data-stu-id="dec56-977">Otherwise, if the field is `readonly` and the reference occurs outside an instance constructor of the class in which the field is declared, then the result is a value, namely the value of the field `I` in the object referenced by `E`.</span></span>
      * <span data-ttu-id="dec56-978">Jinak, výsledek je proměnná, konkrétně pole `I` v objekt odkazovaný zadaným parametrem `E`.</span><span class="sxs-lookup"><span data-stu-id="dec56-978">Otherwise, the result is a variable, namely the field `I` in the object referenced by `E`.</span></span>
   *  <span data-ttu-id="dec56-979">Pokud `T` je *struct_type* a `I` pole instance, která identifikuje *struct_type*:</span><span class="sxs-lookup"><span data-stu-id="dec56-979">If `T` is a *struct_type* and `I` identifies an instance field of that *struct_type*:</span></span>
      * <span data-ttu-id="dec56-980">Pokud `E` je hodnota, nebo pokud je datové pole `readonly` a odkaz proběhne mimo konstruktor instance struktury, ve kterém je deklarována pole a výsledkem je hodnota, konkrétně hodnota pole `I` v dána instance – struktura  `E`.</span><span class="sxs-lookup"><span data-stu-id="dec56-980">If `E` is a value, or if the field is `readonly` and the reference occurs outside an instance constructor of the struct in which the field is declared, then the result is a value, namely the value of the field `I` in the struct instance given by `E`.</span></span>
      * <span data-ttu-id="dec56-981">Jinak, výsledek je proměnná, konkrétně pole `I` v instanci struktury Dal `E`.</span><span class="sxs-lookup"><span data-stu-id="dec56-981">Otherwise, the result is a variable, namely the field `I` in the struct instance given by `E`.</span></span>
   *  <span data-ttu-id="dec56-982">Pokud `I` identifikuje instanci události:</span><span class="sxs-lookup"><span data-stu-id="dec56-982">If `I` identifies an instance event:</span></span>
      * <span data-ttu-id="dec56-983">Pokud dojde k odkazu v rámci třídy nebo struktury, ve kterém je deklarována jako události a události byla deklarovaná bez *event_accessor_declarations* ([události](classes.md#events)), a nedojde k jako odkaz Levá strana příkazu `+=` nebo `-=` operátor, pak `E.I` se právě zpracovává jako `I` bylo pole instance.</span><span class="sxs-lookup"><span data-stu-id="dec56-983">If the reference occurs within the class or struct in which the event is declared, and the event was declared without *event_accessor_declarations* ([Events](classes.md#events)), and the reference does not occur as the left-hand side of a `+=` or `-=` operator, then `E.I` is processed exactly as if `I` was an instance field.</span></span>
      * <span data-ttu-id="dec56-984">Jinak, výsledkem je přístup k události s výrazem přidruženou instanci `E`.</span><span class="sxs-lookup"><span data-stu-id="dec56-984">Otherwise, the result is an event access with an associated instance expression of `E`.</span></span>
*  <span data-ttu-id="dec56-985">V opačném případě je proveden pokus o zpracování `E.I` jako volání metody rozšíření ([volání metod rozšíření](expressions.md#extension-method-invocations)).</span><span class="sxs-lookup"><span data-stu-id="dec56-985">Otherwise, an attempt is made to process `E.I` as an extension method invocation ([Extension method invocations](expressions.md#extension-method-invocations)).</span></span> <span data-ttu-id="dec56-986">Když se to nepovede, `E.I` je odkaz na člena je neplatný a dojde k chybě vazby čas.</span><span class="sxs-lookup"><span data-stu-id="dec56-986">If this fails, `E.I` is an invalid member reference, and a binding-time error occurs.</span></span>

#### <a name="identical-simple-names-and-type-names"></a><span data-ttu-id="dec56-987">Stejný jednoduchý název a názvy typů</span><span class="sxs-lookup"><span data-stu-id="dec56-987">Identical simple names and type names</span></span>

<span data-ttu-id="dec56-988">V přístupu ke členu formuláře `E.I`, pokud `E` je jediným identifikátorem a pokud význam `E` jako *simple_name* ([jednoduché názvy](expressions.md#simple-names)) je konstanta, pole, vlastnost, lokální proměnná ani parametr stejného typu jako význam `E` jako *type_name* ([Namespace a zadejte názvy](basic-concepts.md#namespace-and-type-names)), pak oba možných významů z `E` jsou povolené.</span><span class="sxs-lookup"><span data-stu-id="dec56-988">In a member access of the form `E.I`, if `E` is a single identifier, and if the meaning of `E` as a *simple_name* ([Simple names](expressions.md#simple-names)) is a constant, field, property, local variable, or parameter with the same type as the meaning of `E` as a *type_name* ([Namespace and type names](basic-concepts.md#namespace-and-type-names)), then both possible meanings of `E` are permitted.</span></span> <span data-ttu-id="dec56-989">Dva možné význam `E.I` nejsou nikdy nejednoznačný, protože `I` nutně musí být členem typu `E` v obou případech.</span><span class="sxs-lookup"><span data-stu-id="dec56-989">The two possible meanings of `E.I` are never ambiguous, since `I` must necessarily be a member of the type `E` in both cases.</span></span> <span data-ttu-id="dec56-990">Jinými slovy, pravidlo jednoduše umožňuje přístup na statické členy a vnořené typy `E` kde kompilace by jinak mají došlo k chybě.</span><span class="sxs-lookup"><span data-stu-id="dec56-990">In other words, the rule simply permits access to the static members and nested types of `E` where a compile-time error would otherwise have occurred.</span></span> <span data-ttu-id="dec56-991">Příklad:</span><span class="sxs-lookup"><span data-stu-id="dec56-991">For example:</span></span>
```csharp
struct Color
{
    public static readonly Color White = new Color(...);
    public static readonly Color Black = new Color(...);

    public Color Complement() {...}
}

class A
{
    public Color Color;                // Field Color of type Color

    void F() {
        Color = Color.Black;           // References Color.Black static member
        Color = Color.Complement();    // Invokes Complement() on Color field
    }

    static void G() {
        Color c = Color.White;         // References Color.White static member
    }
}
```

#### <a name="grammar-ambiguities"></a><span data-ttu-id="dec56-992">Gramatika nejednoznačnosti</span><span class="sxs-lookup"><span data-stu-id="dec56-992">Grammar ambiguities</span></span>

<span data-ttu-id="dec56-993">Výroby pro *simple_name* ([jednoduché názvy](expressions.md#simple-names)) a *member_access* ([přístup ke členu](expressions.md#member-access)) může mít za následek nejednoznačnosti v Gramatika výrazů.</span><span class="sxs-lookup"><span data-stu-id="dec56-993">The productions for *simple_name* ([Simple names](expressions.md#simple-names)) and *member_access* ([Member access](expressions.md#member-access)) can give rise to ambiguities in the grammar for expressions.</span></span> <span data-ttu-id="dec56-994">Například příkaz:</span><span class="sxs-lookup"><span data-stu-id="dec56-994">For example, the statement:</span></span>
```
F(G<A,B>(7));
```
<span data-ttu-id="dec56-995">možné interpretovat jako volání `F` se dvěma argumenty, `G < A` a `B > (7)`.</span><span class="sxs-lookup"><span data-stu-id="dec56-995">could be interpreted as a call to `F` with two arguments, `G < A` and `B > (7)`.</span></span> <span data-ttu-id="dec56-996">Alternativně se dá interpretovat jako volání `F` s jedním argumentem, který je volání obecné metody `G` pomocí dva argumenty typu a pravidelné jeden argument.</span><span class="sxs-lookup"><span data-stu-id="dec56-996">Alternatively, it could be interpreted as a call to `F` with one argument, which is a call to a generic method `G` with two type arguments and one regular argument.</span></span>

<span data-ttu-id="dec56-997">Pokud posloupnost tokeny můžete analyzovat (v kontextu) jako *simple_name* ([jednoduché názvy](expressions.md#simple-names)), *member_access* ([přístup ke členu](expressions.md#member-access)), nebo *pointer_member_access* ([přístupu k členovi](unsafe-code.md#pointer-member-access)) končí *type_argument_list* ([argumenty typu](types.md#type-arguments)), Token hned za uzavírací `>` token je zkontrolován.</span><span class="sxs-lookup"><span data-stu-id="dec56-997">If a sequence of tokens can be parsed (in context) as a *simple_name* ([Simple names](expressions.md#simple-names)), *member_access* ([Member access](expressions.md#member-access)), or *pointer_member_access* ([Pointer member access](unsafe-code.md#pointer-member-access)) ending with a *type_argument_list* ([Type arguments](types.md#type-arguments)), the token immediately following the closing `>` token is examined.</span></span> <span data-ttu-id="dec56-998">Pokud je jeden z</span><span class="sxs-lookup"><span data-stu-id="dec56-998">If it is one of</span></span>
```csharp
(  )  ]  }  :  ;  ,  .  ?  ==  !=  |  ^
```
<span data-ttu-id="dec56-999">pak bude *type_argument_list* se uchovávají v rámci *simple_name*, *member_access* nebo *pointer_member_access* a jakékoli Další možné parsovat posloupnost tokeny se zahodí.</span><span class="sxs-lookup"><span data-stu-id="dec56-999">then the *type_argument_list* is retained as part of the *simple_name*, *member_access* or *pointer_member_access* and any other possible parse of the sequence of tokens is discarded.</span></span> <span data-ttu-id="dec56-1000">V opačném případě *type_argument_list* se nepovažuje za součást *simple_name*, *member_access* nebo *pointer_member_access*i v případě, že neexistuje žádná možné parsovat sekvence tokenů.</span><span class="sxs-lookup"><span data-stu-id="dec56-1000">Otherwise, the *type_argument_list* is not considered to be part of the *simple_name*, *member_access* or *pointer_member_access*, even if there is no other possible parse of the sequence of tokens.</span></span> <span data-ttu-id="dec56-1001">Všimněte si, že tato pravidla se použijí při analýze *type_argument_list* v *namespace_or_type_name* ([Namespace a zadejte názvy](basic-concepts.md#namespace-and-type-names)).</span><span class="sxs-lookup"><span data-stu-id="dec56-1001">Note that these rules are not applied when parsing a *type_argument_list* in a *namespace_or_type_name* ([Namespace and type names](basic-concepts.md#namespace-and-type-names)).</span></span> <span data-ttu-id="dec56-1002">Příkaz</span><span class="sxs-lookup"><span data-stu-id="dec56-1002">The statement</span></span>
```csharp
F(G<A,B>(7));
```
<span data-ttu-id="dec56-1003">podle tohoto pravidla se, se bude interpretovat jako volání `F` s jedním argumentem, který je volání obecné metody `G` pomocí dva argumenty typu a pravidelné jeden argument.</span><span class="sxs-lookup"><span data-stu-id="dec56-1003">will, according to this rule, be interpreted as a call to `F` with one argument, which is a call to a generic method `G` with two type arguments and one regular argument.</span></span> <span data-ttu-id="dec56-1004">Příkazy</span><span class="sxs-lookup"><span data-stu-id="dec56-1004">The statements</span></span>
```csharp
F(G < A, B > 7);
F(G < A, B >> 7);
```
<span data-ttu-id="dec56-1005">bude každý interpretovat jako volání `F` se dvěma argumenty.</span><span class="sxs-lookup"><span data-stu-id="dec56-1005">will each be interpreted as a call to `F` with two arguments.</span></span> <span data-ttu-id="dec56-1006">Příkaz</span><span class="sxs-lookup"><span data-stu-id="dec56-1006">The statement</span></span>
```csharp
x = F < A > +y;
```
<span data-ttu-id="dec56-1007">budou interpretovány jako méně než operátor větší než operátor, operátor a Unární plus, jako by měl byl zapsán příkaz `x = (F < A) > (+y)`, místo jako *simple_name* s *type_argument_list* Následuje operátor plus binární soubor.</span><span class="sxs-lookup"><span data-stu-id="dec56-1007">will be interpreted as a less than operator, greater than operator, and unary plus operator, as if the statement had been written `x = (F < A) > (+y)`, instead of as a *simple_name* with a *type_argument_list* followed by a binary plus operator.</span></span> <span data-ttu-id="dec56-1008">V příkazu</span><span class="sxs-lookup"><span data-stu-id="dec56-1008">In the statement</span></span>
```csharp
x = y is C<T> + z;
```
<span data-ttu-id="dec56-1009">tokeny `C<T>` jsou interpretovány jako *namespace_or_type_name* s *type_argument_list*.</span><span class="sxs-lookup"><span data-stu-id="dec56-1009">the tokens `C<T>` are interpreted as a *namespace_or_type_name* with a *type_argument_list*.</span></span>

### <a name="invocation-expressions"></a><span data-ttu-id="dec56-1010">Výrazy typu Invocation</span><span class="sxs-lookup"><span data-stu-id="dec56-1010">Invocation expressions</span></span>

<span data-ttu-id="dec56-1011">*Invocation_expression* se používá k volání metody.</span><span class="sxs-lookup"><span data-stu-id="dec56-1011">An *invocation_expression* is used to invoke a method.</span></span>

```antlr
invocation_expression
    : primary_expression '(' argument_list? ')'
    ;
```

<span data-ttu-id="dec56-1012">*Invocation_expression* dynamicky vázán ([dynamické vazby](expressions.md#dynamic-binding)) Pokud obsahuje nejméně jednu z následujících akcí:</span><span class="sxs-lookup"><span data-stu-id="dec56-1012">An *invocation_expression* is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)) if at least one of the following holds:</span></span>

* <span data-ttu-id="dec56-1013">*Primary_expression* má typ kompilace `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="dec56-1013">The *primary_expression* has compile-time type `dynamic`.</span></span>
* <span data-ttu-id="dec56-1014">Alespoň jeden argument nepovinný *argument_list* má typ kompilace `dynamic` a *primary_expression* nemá typ delegáta.</span><span class="sxs-lookup"><span data-stu-id="dec56-1014">At least one argument of the optional *argument_list* has compile-time type `dynamic` and the *primary_expression* does not have a delegate type.</span></span>

<span data-ttu-id="dec56-1015">V tomto případě kompilátor klasifikuje *invocation_expression* jako hodnotu typu `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="dec56-1015">In this case the compiler classifies the *invocation_expression* as a value of type `dynamic`.</span></span> <span data-ttu-id="dec56-1016">Pravidly dole určit význam *invocation_expression* se pak použijí v době běhu, run-time typu místo kompilaci typu těch, které *primary_expression* a argumenty, které mají typ kompilace `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="dec56-1016">The rules below to determine the meaning of the *invocation_expression* are then applied at run-time, using the run-time type instead of the compile-time type of those of the *primary_expression* and arguments which have the compile-time type `dynamic`.</span></span> <span data-ttu-id="dec56-1017">Pokud *primary_expression* nemá typ kompilace `dynamic`, pak volání metody projde Kontrola času kompilace omezená, jak je popsáno v [kompilace kontrolu dynamické přetížení ](expressions.md#compile-time-checking-of-dynamic-overload-resolution).</span><span class="sxs-lookup"><span data-stu-id="dec56-1017">If the *primary_expression* does not have compile-time type `dynamic`, then the method invocation undergoes a limited compile time check as described in [Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution).</span></span>

<span data-ttu-id="dec56-1018">*Primary_expression* ze *invocation_expression* musí být metoda skupiny nebo hodnotu *delegate_type*.</span><span class="sxs-lookup"><span data-stu-id="dec56-1018">The *primary_expression* of an *invocation_expression* must be a method group or a value of a *delegate_type*.</span></span> <span data-ttu-id="dec56-1019">Pokud *primary_expression* je skupinu metod *invocation_expression* je volání metody ([volání metod](expressions.md#method-invocations)).</span><span class="sxs-lookup"><span data-stu-id="dec56-1019">If the *primary_expression* is a method group, the *invocation_expression* is a method invocation ([Method invocations](expressions.md#method-invocations)).</span></span> <span data-ttu-id="dec56-1020">Pokud *primary_expression* . má hodnotu *delegate_type*, *invocation_expression* vyvolání delegáta ([delegáta volání](expressions.md#delegate-invocations)).</span><span class="sxs-lookup"><span data-stu-id="dec56-1020">If the *primary_expression* is a value of a *delegate_type*, the *invocation_expression* is a delegate invocation ([Delegate invocations](expressions.md#delegate-invocations)).</span></span> <span data-ttu-id="dec56-1021">Pokud *primary_expression* není skupinu metod ani hodnotu *delegate_type*, dojde k chybě vazby čas.</span><span class="sxs-lookup"><span data-stu-id="dec56-1021">If the *primary_expression* is neither a method group nor a value of a *delegate_type*, a binding-time error occurs.</span></span>

<span data-ttu-id="dec56-1022">Volitelný *argument_list* ([seznamy argumentů](expressions.md#argument-lists)) obsahuje hodnoty nebo odkazy na proměnné pro parametry metody.</span><span class="sxs-lookup"><span data-stu-id="dec56-1022">The optional *argument_list* ([Argument lists](expressions.md#argument-lists)) provides values or variable references for the parameters of the method.</span></span>

<span data-ttu-id="dec56-1023">Výsledek vyhodnocení výrazu *invocation_expression* klasifikovaný následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="dec56-1023">The result of evaluating an *invocation_expression* is classified as follows:</span></span>

*  <span data-ttu-id="dec56-1024">Pokud *invocation_expression* vyvolá metoda nebo delegát, který vrátí `void`, výsledek má hodnotu nothing.</span><span class="sxs-lookup"><span data-stu-id="dec56-1024">If the *invocation_expression* invokes a method or delegate that returns `void`, the result is nothing.</span></span> <span data-ttu-id="dec56-1025">Výraz, který je klasifikován jako nic je povolený jenom v kontextu *statement_expression* ([příkazy výrazů](statements.md#expression-statements)) nebo jako text *lambda_expression*([Výrazy anonymní funkce](expressions.md#anonymous-function-expressions)).</span><span class="sxs-lookup"><span data-stu-id="dec56-1025">An expression that is classified as nothing is permitted only in the context of a *statement_expression* ([Expression statements](statements.md#expression-statements)) or as the body of a *lambda_expression* ([Anonymous function expressions](expressions.md#anonymous-function-expressions)).</span></span> <span data-ttu-id="dec56-1026">Jinak dojde k chybě vazby čas.</span><span class="sxs-lookup"><span data-stu-id="dec56-1026">Otherwise a binding-time error occurs.</span></span>
*  <span data-ttu-id="dec56-1027">Jinak výsledkem je hodnota typu vrácené z metody nebo delegáta.</span><span class="sxs-lookup"><span data-stu-id="dec56-1027">Otherwise, the result is a value of the type returned by the method or delegate.</span></span>

#### <a name="method-invocations"></a><span data-ttu-id="dec56-1028">Volání metod</span><span class="sxs-lookup"><span data-stu-id="dec56-1028">Method invocations</span></span>

<span data-ttu-id="dec56-1029">Pro volání metody *primary_expression* z *invocation_expression* musí být skupinu metod.</span><span class="sxs-lookup"><span data-stu-id="dec56-1029">For a method invocation, the *primary_expression* of the *invocation_expression* must be a method group.</span></span> <span data-ttu-id="dec56-1030">Skupinu metod identifikuje jednu metodu, která se má vyvolat nebo sadu přetížených metod, ze kterého mohou vybírat konkrétní metody, která se má vyvolat.</span><span class="sxs-lookup"><span data-stu-id="dec56-1030">The method group identifies the one method to invoke or the set of overloaded methods from which to choose a specific method to invoke.</span></span> <span data-ttu-id="dec56-1031">V druhém případě je stanovení konkrétní metoda k vyvolání založené na poskytované typy argumentů v kontextu *argument_list*.</span><span class="sxs-lookup"><span data-stu-id="dec56-1031">In the latter case, determination of the specific method to invoke is based on the context provided by the types of the arguments in the *argument_list*.</span></span>

<span data-ttu-id="dec56-1032">Vazby – čas zpracování volání metody formuláře `M(A)`, kde `M` je skupina – metoda (verzovaným *type_argument_list*), a `A` je volitelný *argument_ seznam*, se skládá z následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="dec56-1032">The binding-time processing of a method invocation of the form `M(A)`, where `M` is a method group (possibly including a *type_argument_list*), and `A` is an optional *argument_list*, consists of the following steps:</span></span>

*  <span data-ttu-id="dec56-1033">Sadu metod pro volání metody je vytvořený.</span><span class="sxs-lookup"><span data-stu-id="dec56-1033">The set of candidate methods for the method invocation is constructed.</span></span> <span data-ttu-id="dec56-1034">Pro každou metodu `F` přidružené ke skupině metoda `M`:</span><span class="sxs-lookup"><span data-stu-id="dec56-1034">For each method `F` associated with the method group `M`:</span></span>
   *  <span data-ttu-id="dec56-1035">Pokud `F` není obecná, `F` je Release candidate při:</span><span class="sxs-lookup"><span data-stu-id="dec56-1035">If `F` is non-generic, `F` is a candidate when:</span></span>
      * <span data-ttu-id="dec56-1036">`M` nemá žádný seznam argumentů typu a</span><span class="sxs-lookup"><span data-stu-id="dec56-1036">`M` has no type argument list, and</span></span>
      * <span data-ttu-id="dec56-1037">`F` lze použít s ohledem na `A` ([použít funkční člen](expressions.md#applicable-function-member)).</span><span class="sxs-lookup"><span data-stu-id="dec56-1037">`F` is applicable with respect to `A` ([Applicable function member](expressions.md#applicable-function-member)).</span></span>
   *  <span data-ttu-id="dec56-1038">Pokud `F` je obecný a `M` nemá žádný seznam argumentů typu `F` je Release candidate při:</span><span class="sxs-lookup"><span data-stu-id="dec56-1038">If `F` is generic and `M` has no type argument list, `F` is a candidate when:</span></span>
      * <span data-ttu-id="dec56-1039">Odvození typu ([odvození typu](expressions.md#type-inference)) úspěšná, odvozování seznam argumentů pro volání, a</span><span class="sxs-lookup"><span data-stu-id="dec56-1039">Type inference ([Type inference](expressions.md#type-inference)) succeeds, inferring a list of type arguments for the call, and</span></span>
      * <span data-ttu-id="dec56-1040">Jakmile se argumenty typu jsou substituovány za parametry typu odpovídající metoda, všechny sestavené typy v seznamu parametrů F vyhovět jejich omezením ([neodpovídajících omezení](types.md#satisfying-constraints)) a seznamu parametrů `F` lze použít s ohledem na `A` ([použít funkční člen](expressions.md#applicable-function-member)).</span><span class="sxs-lookup"><span data-stu-id="dec56-1040">Once the inferred type arguments are substituted for the corresponding method type parameters, all constructed types in the parameter list of F satisfy their constraints ([Satisfying constraints](types.md#satisfying-constraints)), and the parameter list of `F` is applicable with respect to `A` ([Applicable function member](expressions.md#applicable-function-member)).</span></span>
   *  <span data-ttu-id="dec56-1041">Pokud `F` je obecný a `M` obsahuje seznam argumentů typu, `F` je Release candidate při:</span><span class="sxs-lookup"><span data-stu-id="dec56-1041">If `F` is generic and `M` includes a type argument list, `F` is a candidate when:</span></span>
      * <span data-ttu-id="dec56-1042">`F` byly zadány v seznamu argumentů typu má stejný počet parametrů typu metoda a</span><span class="sxs-lookup"><span data-stu-id="dec56-1042">`F` has the same number of method type parameters as were supplied in the type argument list, and</span></span>
      * <span data-ttu-id="dec56-1043">Jakmile se argumenty typu jsou substituovány za parametry typu odpovídající metoda, všechny sestavené typy v seznamu parametrů F vyhovět jejich omezením ([neodpovídajících omezení](types.md#satisfying-constraints)) a seznamu parametrů `F` lze použít s ohledem na `A` ([použít funkční člen](expressions.md#applicable-function-member)).</span><span class="sxs-lookup"><span data-stu-id="dec56-1043">Once the type arguments are substituted for the corresponding method type parameters, all constructed types in the parameter list of F satisfy their constraints ([Satisfying constraints](types.md#satisfying-constraints)), and the parameter list of `F` is applicable with respect to `A` ([Applicable function member](expressions.md#applicable-function-member)).</span></span>
*  <span data-ttu-id="dec56-1044">Sadu metod Release candidate je omezit tak, aby obsahovala pouze metody ze nejvíce odvozené typy: Pro každou metodu `C.F` v sadě, kde `C` je typ, ve kterém metoda `F` je teď deklarována, všechny metody deklarované v základní typ `C` se odeberou ze sady.</span><span class="sxs-lookup"><span data-stu-id="dec56-1044">The set of candidate methods is reduced to contain only methods from the most derived types: For each method `C.F` in the set, where `C` is the type in which the method `F` is declared, all methods declared in a base type of `C` are removed from the set.</span></span> <span data-ttu-id="dec56-1045">Kromě toho pokud `C` typu třídy je jiné než `object`, všechny metody deklarované v rozhraní typu se odeberou ze sady.</span><span class="sxs-lookup"><span data-stu-id="dec56-1045">Furthermore, if `C` is a class type other than `object`, all methods declared in an interface type are removed from the set.</span></span> <span data-ttu-id="dec56-1046">(Toto druhé pravidlo pouze nemá vliv při výsledek vyhledávání člena pro parametr typu s efektivní základní třídy, než je objekt a efektivní rozhraní neprázdný nastavit skupinu metod.)</span><span class="sxs-lookup"><span data-stu-id="dec56-1046">(This latter rule only has affect when the method group was the result of a member lookup on a type parameter having an effective base class other than object and a non-empty effective interface set.)</span></span>
*  <span data-ttu-id="dec56-1047">Pokud výslednou sadu metod je prázdný, další zpracování podél následující kroky jsou opuštěných a místo toho proveden pokus o zpracování volání jako volání metody rozšíření ([volání metod rozšíření](expressions.md#extension-method-invocations)).</span><span class="sxs-lookup"><span data-stu-id="dec56-1047">If the resulting set of candidate methods is empty, then further processing along the following steps are abandoned, and instead an attempt is made to process the invocation as an extension method invocation ([Extension method invocations](expressions.md#extension-method-invocations)).</span></span> <span data-ttu-id="dec56-1048">Pokud se to nepodaří, neexistují žádné použitelné metody a dojde k chybě vazby čas.</span><span class="sxs-lookup"><span data-stu-id="dec56-1048">If this fails, then no applicable methods exist, and a binding-time error occurs.</span></span>
*  <span data-ttu-id="dec56-1049">Nejlepší metody sadu metod Release candidate je identifikován pomocí pravidel rozlišení přetížení [rozlišení přetěžování](expressions.md#overload-resolution).</span><span class="sxs-lookup"><span data-stu-id="dec56-1049">The best method of the set of candidate methods is identified using the overload resolution rules of [Overload resolution](expressions.md#overload-resolution).</span></span> <span data-ttu-id="dec56-1050">Pokud nebylo možné identifikovat jeden nejlepší metody, je nejednoznačné volání metody a dojde k chybě vazby čas.</span><span class="sxs-lookup"><span data-stu-id="dec56-1050">If a single best method cannot be identified, the method invocation is ambiguous, and a binding-time error occurs.</span></span> <span data-ttu-id="dec56-1051">Při překladu přetížení parametrů Obecné metody jsou považovány za po nahrazení argumentů typu (zadaný, nebo vyvozen) pro odpovídající typ parametry metody.</span><span class="sxs-lookup"><span data-stu-id="dec56-1051">When performing overload resolution, the parameters of a generic method are considered after substituting the type arguments (supplied or inferred) for the corresponding method type parameters.</span></span>
*  <span data-ttu-id="dec56-1052">Poslední zvolené nejlepší metody ověřování:</span><span class="sxs-lookup"><span data-stu-id="dec56-1052">Final validation of the chosen best method is performed:</span></span>
   * <span data-ttu-id="dec56-1053">Metoda ověření v kontextu skupinu metod: Nejlepší metodou je statickou metodu, musí mít skupinu metod. výsledkem *simple_name* nebo *member_access* prostřednictvím typu.</span><span class="sxs-lookup"><span data-stu-id="dec56-1053">The method is validated in the context of the method group: If the best method is a static method, the method group must have resulted from a *simple_name* or a *member_access* through a type.</span></span> <span data-ttu-id="dec56-1054">Nejlepší způsob je metoda instance, musí mít skupinu metod. výsledkem *simple_name*, *member_access* prostřednictvím proměnné nebo hodnotu, nebo *base_access*.</span><span class="sxs-lookup"><span data-stu-id="dec56-1054">If the best method is an instance method, the method group must have resulted from a *simple_name*, a *member_access* through a variable or value, or a *base_access*.</span></span> <span data-ttu-id="dec56-1055">Pokud ani jedno z těchto požadavků je hodnota true, dojde k chybě vazby čas.</span><span class="sxs-lookup"><span data-stu-id="dec56-1055">If neither of these requirements is true, a binding-time error occurs.</span></span>
   * <span data-ttu-id="dec56-1056">Pokud je nejlepší metodou je obecná metoda, zadejte argumenty (poskytnuté nebo odvozené) jsou porovnávána s omezením ([neodpovídajících omezení](types.md#satisfying-constraints)) deklarovat v obecné metody.</span><span class="sxs-lookup"><span data-stu-id="dec56-1056">If the best method is a generic method, the type arguments (supplied or inferred) are checked against the constraints ([Satisfying constraints](types.md#satisfying-constraints)) declared on the generic method.</span></span> <span data-ttu-id="dec56-1057">Pokud některý z argumentů typu nevyhovuje odpovídající omezení u parametru typu, dojde k chybě vazby čas.</span><span class="sxs-lookup"><span data-stu-id="dec56-1057">If any type argument does not satisfy the corresponding constraint(s) on the type parameter, a binding-time error occurs.</span></span>

<span data-ttu-id="dec56-1058">Jakmile metodu byla vybrána a ověřený v době vazby výše uvedené kroky, skutečné vyvolání za běhu se zpracovává podle pravidel objektů volání členské funkce popsané v [kompilace kontrolu dynamické přetížení ](expressions.md#compile-time-checking-of-dynamic-overload-resolution).</span><span class="sxs-lookup"><span data-stu-id="dec56-1058">Once a method has been selected and validated at binding-time by the above steps, the actual run-time invocation is processed according to the rules of function member invocation described in [Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution).</span></span>

<span data-ttu-id="dec56-1059">Intuitivní efekt výše popsaná pravidla překladu je následujícím způsobem: Vyhledat konkrétní metody vyvolané volání metody, začněte s typ označený volání metody a pokračovat, dokud nebude nalezen alespoň jednu metodu použít, přístupná, bez přepsání deklarace celým řetězcem dědičnosti.</span><span class="sxs-lookup"><span data-stu-id="dec56-1059">The intuitive effect of the resolution rules described above is as follows: To locate the particular method invoked by a method invocation, start with the type indicated by the method invocation and proceed up the inheritance chain until at least one applicable, accessible, non-override method declaration is found.</span></span> <span data-ttu-id="dec56-1060">Následně odvození typu proměnné a řešení v sadě použít, přístupná, bez přepsání metody deklarované v tomto typu přetížení a vyvolat metodu tedy vybrali.</span><span class="sxs-lookup"><span data-stu-id="dec56-1060">Then perform type inference and overload resolution on the set of applicable, accessible, non-override methods declared in that type and invoke the method thus selected.</span></span> <span data-ttu-id="dec56-1061">Pokud nebyla nalezena žádná metoda, zkuste místo toho zpracovat volání jako volání metody rozšíření.</span><span class="sxs-lookup"><span data-stu-id="dec56-1061">If no method was found, try instead to process the invocation as an extension method invocation.</span></span>

#### <a name="extension-method-invocations"></a><span data-ttu-id="dec56-1062">Volání metod rozšíření</span><span class="sxs-lookup"><span data-stu-id="dec56-1062">Extension method invocations</span></span>

<span data-ttu-id="dec56-1063">Ve volání metody ([volání na instancích zabalený](expressions.md#invocations-on-boxed-instances)) jednoho z formuláře</span><span class="sxs-lookup"><span data-stu-id="dec56-1063">In a method invocation ([Invocations on boxed instances](expressions.md#invocations-on-boxed-instances)) of one of the forms</span></span>
```csharp
expr . identifier ( )

expr . identifier ( args )

expr . identifier < typeargs > ( )

expr . identifier < typeargs > ( args )
```
<span data-ttu-id="dec56-1064">Pokud normálním zpracování volání najde žádné použitelné metody, je proveden pokus o zpracování konstrukce jako volání metody rozšíření.</span><span class="sxs-lookup"><span data-stu-id="dec56-1064">if the normal processing of the invocation finds no applicable methods, an attempt is made to process the construct as an extension method invocation.</span></span> <span data-ttu-id="dec56-1065">Pokud *expr* nebo některý z *args* má typ kompilace `dynamic`, rozšiřující metody se nepoužije.</span><span class="sxs-lookup"><span data-stu-id="dec56-1065">If *expr* or any of the *args* has compile-time type `dynamic`, extension methods will not apply.</span></span>

<span data-ttu-id="dec56-1066">Cílem je najít nejlepší *type_name* `C`, takže může proběhnout odpovídající volání statické metody:</span><span class="sxs-lookup"><span data-stu-id="dec56-1066">The objective is to find the best *type_name* `C`, so that the corresponding static method invocation can take place:</span></span>
```csharp
C . identifier ( expr )

C . identifier ( expr , args )

C . identifier < typeargs > ( expr )

C . identifier < typeargs > ( expr , args )
```

<span data-ttu-id="dec56-1067">Metody rozšíření `Ci.Mj` je ***oprávněné*** pokud:</span><span class="sxs-lookup"><span data-stu-id="dec56-1067">An extension method `Ci.Mj` is ***eligible*** if:</span></span>

*  <span data-ttu-id="dec56-1068">`Ci` je jiná než obecná, ne vnořené třídy</span><span class="sxs-lookup"><span data-stu-id="dec56-1068">`Ci` is a non-generic, non-nested class</span></span>
*  <span data-ttu-id="dec56-1069">Název `Mj` je *identifikátor*</span><span class="sxs-lookup"><span data-stu-id="dec56-1069">The name of `Mj` is *identifier*</span></span>
*  <span data-ttu-id="dec56-1070">`Mj` je přístupný a použitelný, při použití na argumenty jako statická metoda, jak je uvedeno výše</span><span class="sxs-lookup"><span data-stu-id="dec56-1070">`Mj` is accessible and applicable when applied to the arguments as a static method as shown above</span></span>
*  <span data-ttu-id="dec56-1071">Existuje implicitní identity, odkaz nebo převod na uzavřené určení z *expr* typu první parametr `Mj`.</span><span class="sxs-lookup"><span data-stu-id="dec56-1071">An implicit identity, reference or boxing conversion exists from *expr* to the type of the first parameter of `Mj`.</span></span>

<span data-ttu-id="dec56-1072">Hledání `C` probíhá následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="dec56-1072">The search for `C` proceeds as follows:</span></span>

*  <span data-ttu-id="dec56-1073">Od nejbližší uzavírající deklarace oboru názvů, pokračujte v každé nadřazené deklarace oboru názvů a konče obsahující kompilační jednotky byly provedeny po sobě jdoucích pokusy o vyhledání kandidátských sadu rozšiřujících metod:</span><span class="sxs-lookup"><span data-stu-id="dec56-1073">Starting with the closest enclosing namespace declaration, continuing with each enclosing namespace declaration, and ending with the containing compilation unit, successive attempts are made to find a candidate set of extension methods:</span></span>
   * <span data-ttu-id="dec56-1074">Pokud daný obor názvů nebo kompilace jednotka přímo obsahuje neobecný typ deklarace `Ci` s oprávněné rozšiřující metody `Mj`, pak sada z těchto metod rozšíření je sada Release candidate.</span><span class="sxs-lookup"><span data-stu-id="dec56-1074">If the given namespace or compilation unit directly contains non-generic type declarations `Ci` with eligible extension methods `Mj`, then the set of those extension methods is the candidate set.</span></span>
   * <span data-ttu-id="dec56-1075">Pokud se typy `Ci` importované tímto seznamem *using_static_declarations* a přímo deklarovány v oborech názvů importované tímto seznamem *using_namespace_directive*s v daném oboru názvů nebo kompilace jednotce přímo obsahují metody rozšíření oprávněné `Mj`, pak sada z těchto metod rozšíření je sada Release candidate.</span><span class="sxs-lookup"><span data-stu-id="dec56-1075">If types `Ci` imported by *using_static_declarations* and directly declared in namespaces imported by *using_namespace_directive*s in the given namespace or compilation unit directly contain eligible extension methods `Mj`, then the set of those extension methods is the candidate set.</span></span>
*  <span data-ttu-id="dec56-1076">Pokud není nastaven žádný kandidát nenajde v libovolné nadřazené jednotce prohlášení nebo kompilace obor názvů, dojde k chybě v době kompilace.</span><span class="sxs-lookup"><span data-stu-id="dec56-1076">If no candidate set is found in any enclosing namespace declaration or compilation unit, a compile-time error occurs.</span></span>
*  <span data-ttu-id="dec56-1077">V opačném případě řešení přetížení se použije na verzi Release candidate popsaného v ([rozlišení přetěžování](expressions.md#overload-resolution)).</span><span class="sxs-lookup"><span data-stu-id="dec56-1077">Otherwise, overload resolution is applied to the candidate set as described in ([Overload resolution](expressions.md#overload-resolution)).</span></span> <span data-ttu-id="dec56-1078">Pokud se nenajde žádný jediné nejlepší metody, dojde k chybě kompilace.</span><span class="sxs-lookup"><span data-stu-id="dec56-1078">If no single best method is found, a compile-time error occurs.</span></span>
*  <span data-ttu-id="dec56-1079">`C` je typ, ve kterém je nejlepší metody deklarován jako metody rozšíření.</span><span class="sxs-lookup"><span data-stu-id="dec56-1079">`C` is the type within which the best method is declared as an extension method.</span></span>

<span data-ttu-id="dec56-1080">Pomocí `C` jako cíl, volání metody, které pak zpracovány jako volání statické metody ([kompilace kontrolu dynamické přetížení](expressions.md#compile-time-checking-of-dynamic-overload-resolution)).</span><span class="sxs-lookup"><span data-stu-id="dec56-1080">Using `C` as a target, the method call is then processed as a static method invocation ([Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution)).</span></span>

<span data-ttu-id="dec56-1081">Výše uvedená pravidla znamená, že metody instance přednost metody rozšíření, že rozšiřující metody, které jsou k dispozici v oboru názvů vnitřní deklarace přednost rozšiřující metody, které jsou dostupné na vnější obor názvů deklarace a rozšíření metody s deklarací přímo v oboru názvů přednost metody rozšíření importován Tento stejný obor názvů pomocí direktivy oboru názvů.</span><span class="sxs-lookup"><span data-stu-id="dec56-1081">The preceding rules mean that instance methods take precedence over extension methods, that extension methods available in inner namespace declarations take precedence over extension methods available in outer namespace declarations, and that extension methods declared directly in a namespace take precedence over extension methods imported into that same namespace with a using namespace directive.</span></span> <span data-ttu-id="dec56-1082">Příklad:</span><span class="sxs-lookup"><span data-stu-id="dec56-1082">For example:</span></span>
```csharp
public static class E
{
    public static void F(this object obj, int i) { }

    public static void F(this object obj, string s) { }
}

class A { }

class B
{
    public void F(int i) { }
}

class C
{
    public void F(object obj) { }
}

class X
{
    static void Test(A a, B b, C c) {
        a.F(1);              // E.F(object, int)
        a.F("hello");        // E.F(object, string)

        b.F(1);              // B.F(int)
        b.F("hello");        // E.F(object, string)

        c.F(1);              // C.F(object)
        c.F("hello");        // C.F(object)
    }
}
```

<span data-ttu-id="dec56-1083">V tomto příkladu `B`metoda má přednost před první metodu rozšíření, a `C`metoda má přednost před obě metody rozšíření.</span><span class="sxs-lookup"><span data-stu-id="dec56-1083">In the example, `B`'s method takes precedence over the first extension method, and `C`'s method takes precedence over both extension methods.</span></span>

```csharp
public static class C
{
    public static void F(this int i) { Console.WriteLine("C.F({0})", i); }
    public static void G(this int i) { Console.WriteLine("C.G({0})", i); }
    public static void H(this int i) { Console.WriteLine("C.H({0})", i); }
}

namespace N1
{
    public static class D
    {
        public static void F(this int i) { Console.WriteLine("D.F({0})", i); }
        public static void G(this int i) { Console.WriteLine("D.G({0})", i); }
    }
}

namespace N2
{
    using N1;

    public static class E
    {
        public static void F(this int i) { Console.WriteLine("E.F({0})", i); }
    }

    class Test
    {
        static void Main(string[] args)
        {
            1.F();
            2.G();
            3.H();
        }
    }
}
```

<span data-ttu-id="dec56-1084">Výstup tohoto příkladu je:</span><span class="sxs-lookup"><span data-stu-id="dec56-1084">The output of this example is:</span></span>
```
E.F(1)
D.G(2)
C.H(3)
```
<span data-ttu-id="dec56-1085">`D.G` má přednost před `C.G`, a `E.F` má přednost před obě `D.F` a `C.F`.</span><span class="sxs-lookup"><span data-stu-id="dec56-1085">`D.G` takes precedence over `C.G`, and `E.F` takes precedence over both `D.F` and `C.F`.</span></span>

#### <a name="delegate-invocations"></a><span data-ttu-id="dec56-1086">Vyvolání delegáta</span><span class="sxs-lookup"><span data-stu-id="dec56-1086">Delegate invocations</span></span>

<span data-ttu-id="dec56-1087">Pro vyvolání typu delegáta *primary_expression* z *invocation_expression* musí být hodnota *delegate_type*.</span><span class="sxs-lookup"><span data-stu-id="dec56-1087">For a delegate invocation, the *primary_expression* of the *invocation_expression* must be a value of a *delegate_type*.</span></span> <span data-ttu-id="dec56-1088">Navíc vzhledem k tomu *delegate_type* funkce člena se stejným seznamem parametrů jako *delegate_type*, *delegate_type* musí být příslušný () [Použitelná funkce člena](expressions.md#applicable-function-member)) s ohledem na *argument_list* z *invocation_expression*.</span><span class="sxs-lookup"><span data-stu-id="dec56-1088">Furthermore, considering the *delegate_type* to be a function member with the same parameter list as the *delegate_type*, the *delegate_type* must be applicable ([Applicable function member](expressions.md#applicable-function-member)) with respect to the *argument_list* of the *invocation_expression*.</span></span>

<span data-ttu-id="dec56-1089">Za běhu zpracování volání delegáta formuláře `D(A)`, kde `D` je *primary_expression* z *delegate_type* a `A` je volitelné *argument_list*, se skládá z následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="dec56-1089">The run-time processing of a delegate invocation of the form `D(A)`, where `D` is a *primary_expression* of a *delegate_type* and `A` is an optional *argument_list*, consists of the following steps:</span></span>

*  <span data-ttu-id="dec56-1090">`D` je vyhodnocen.</span><span class="sxs-lookup"><span data-stu-id="dec56-1090">`D` is evaluated.</span></span> <span data-ttu-id="dec56-1091">Je-li toto vyhodnocení způsobí výjimku, jsou spuštěny žádné další kroky.</span><span class="sxs-lookup"><span data-stu-id="dec56-1091">If this evaluation causes an exception, no further steps are executed.</span></span>
*  <span data-ttu-id="dec56-1092">Hodnota `D` je zaškrtnuté políčko platný.</span><span class="sxs-lookup"><span data-stu-id="dec56-1092">The value of `D` is checked to be valid.</span></span> <span data-ttu-id="dec56-1093">Pokud hodnota `D` je `null`, `System.NullReferenceException` je vyvolána, a že jsou provedeny žádné další kroky.</span><span class="sxs-lookup"><span data-stu-id="dec56-1093">If the value of `D` is `null`, a `System.NullReferenceException` is thrown and no further steps are executed.</span></span>
*  <span data-ttu-id="dec56-1094">V opačném případě `D` je odkaz na instanci delegáta.</span><span class="sxs-lookup"><span data-stu-id="dec56-1094">Otherwise, `D` is a reference to a delegate instance.</span></span> <span data-ttu-id="dec56-1095">Volání členské funkce ([kompilace kontrolu dynamické přetížení](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) se provádí na všech volatelných entit v seznamu vyvolání delegáta.</span><span class="sxs-lookup"><span data-stu-id="dec56-1095">Function member invocations ([Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) are performed on each of the callable entities in the invocation list of the delegate.</span></span> <span data-ttu-id="dec56-1096">Volatelný entit skládající se z instance a metoda instance služby je instance pro vyvolání instance obsaženého v entitě volatelná aplikacemi.</span><span class="sxs-lookup"><span data-stu-id="dec56-1096">For callable entities consisting of an instance and instance method, the instance for the invocation is the instance contained in the callable entity.</span></span>

### <a name="element-access"></a><span data-ttu-id="dec56-1097">Přístup k prvkům</span><span class="sxs-lookup"><span data-stu-id="dec56-1097">Element access</span></span>

<span data-ttu-id="dec56-1098">*Element_access* se skládá z *primary_no_array_creation_expression*a po něm "`[`" token a po něm *argument_list*a po něm " `]`"token.</span><span class="sxs-lookup"><span data-stu-id="dec56-1098">An *element_access* consists of a *primary_no_array_creation_expression*, followed by a "`[`" token, followed by an *argument_list*, followed by a "`]`" token.</span></span> <span data-ttu-id="dec56-1099">*Argument_list* obsahuje jeden nebo více *argument*s, oddělené čárkami.</span><span class="sxs-lookup"><span data-stu-id="dec56-1099">The *argument_list* consists of one or more *argument*s, separated by commas.</span></span>

```antlr
element_access
    : primary_no_array_creation_expression '[' expression_list ']'
    ;
```

<span data-ttu-id="dec56-1100">*Argument_list* ze *element_access* nesmí obsahovat `ref` nebo `out` argumenty.</span><span class="sxs-lookup"><span data-stu-id="dec56-1100">The *argument_list* of an *element_access* is not allowed to contain `ref` or `out` arguments.</span></span>

<span data-ttu-id="dec56-1101">*Element_access* dynamicky vázán ([dynamické vazby](expressions.md#dynamic-binding)) Pokud obsahuje nejméně jednu z následujících akcí:</span><span class="sxs-lookup"><span data-stu-id="dec56-1101">An *element_access* is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)) if at least one of the following holds:</span></span>

* <span data-ttu-id="dec56-1102">*Primary_no_array_creation_expression* má typ kompilace `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="dec56-1102">The *primary_no_array_creation_expression* has compile-time type `dynamic`.</span></span>
* <span data-ttu-id="dec56-1103">Nejméně jeden výraz *argument_list* má typ kompilace `dynamic` a *primary_no_array_creation_expression* nemá typ pole.</span><span class="sxs-lookup"><span data-stu-id="dec56-1103">At least one expression of the *argument_list* has compile-time type `dynamic` and the *primary_no_array_creation_expression* does not have an array type.</span></span>

<span data-ttu-id="dec56-1104">V tomto případě kompilátor klasifikuje *element_access* jako hodnotu typu `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="dec56-1104">In this case the compiler classifies the *element_access* as a value of type `dynamic`.</span></span> <span data-ttu-id="dec56-1105">Pravidly dole určit význam *element_access* se pak použijí v době běhu, run-time typu místo kompilaci typu těch, které *primary_no_array_creation_expression*a *argument_list* výrazy, které mají typ kompilace `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="dec56-1105">The rules below to determine the meaning of the *element_access* are then applied at run-time, using the run-time type instead of the compile-time type of those of the *primary_no_array_creation_expression* and *argument_list* expressions which have the compile-time type `dynamic`.</span></span> <span data-ttu-id="dec56-1106">Pokud *primary_no_array_creation_expression* nemá typu v době kompilace `dynamic`, pak přístup k prvkům projde Kontrola času kompilace omezená, jak je popsáno v [kontrolu dynamická kompilace Rozlišení přetěžování](expressions.md#compile-time-checking-of-dynamic-overload-resolution).</span><span class="sxs-lookup"><span data-stu-id="dec56-1106">If the *primary_no_array_creation_expression* does not have compile-time type `dynamic`, then the element access undergoes a limited compile time check as described in [Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution).</span></span>

<span data-ttu-id="dec56-1107">Pokud *primary_no_array_creation_expression* z *element_access* . má hodnotu *array_type*, *element_access* je Array – přístup ([pole přístup](expressions.md#array-access)).</span><span class="sxs-lookup"><span data-stu-id="dec56-1107">If the *primary_no_array_creation_expression* of an *element_access* is a value of an *array_type*, the *element_access* is an array access ([Array access](expressions.md#array-access)).</span></span> <span data-ttu-id="dec56-1108">V opačném případě *primary_no_array_creation_expression* musí být proměnná nebo hodnota třídy, struktury nebo typu rozhraní, který má jeden nebo více členů indexer, v takovém případě *element_access* je přístup indexeru ([přístup indexeru](expressions.md#indexer-access)).</span><span class="sxs-lookup"><span data-stu-id="dec56-1108">Otherwise, the *primary_no_array_creation_expression* must be a variable or value of a class, struct, or interface type that has one or more indexer members, in which case the *element_access* is an indexer access ([Indexer access](expressions.md#indexer-access)).</span></span>

#### <a name="array-access"></a><span data-ttu-id="dec56-1109">Přístup k poli</span><span class="sxs-lookup"><span data-stu-id="dec56-1109">Array access</span></span>

<span data-ttu-id="dec56-1110">Pro přístup k poli *primary_no_array_creation_expression* z *element_access* musí být hodnota *array_type*.</span><span class="sxs-lookup"><span data-stu-id="dec56-1110">For an array access, the *primary_no_array_creation_expression* of the *element_access* must be a value of an *array_type*.</span></span> <span data-ttu-id="dec56-1111">Kromě toho *argument_list* pole přístup nesmí obsahovat pojmenované argumenty. Počet výrazů v *argument_list* musí být stejné jako pořadí *array_type*, a každý výraz musí být typu `int`, `uint`, `long`, `ulong`, nebo musí být implicitně převeditelný na jeden nebo více z těchto typů.</span><span class="sxs-lookup"><span data-stu-id="dec56-1111">Furthermore, the *argument_list* of an array access is not allowed to contain named arguments.The number of expressions in the *argument_list* must be the same as the rank of the *array_type*, and each expression must be of type `int`, `uint`, `long`, `ulong`, or must be implicitly convertible to one or more of these types.</span></span>

<span data-ttu-id="dec56-1112">Výsledek vyhodnocení výrazu přístup k poli je proměnná typu prvku pole, konkrétně prvku pole zvolila hodnoty výrazů v *argument_list*.</span><span class="sxs-lookup"><span data-stu-id="dec56-1112">The result of evaluating an array access is a variable of the element type of the array, namely the array element selected by the value(s) of the expression(s) in the *argument_list*.</span></span>

<span data-ttu-id="dec56-1113">Zpracování za běhu přístup k poli formuláře `P[A]`, kde `P` je *primary_no_array_creation_expression* ze *array_type* a `A` je *argument_list*, se skládá z následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="dec56-1113">The run-time processing of an array access of the form `P[A]`, where `P` is a *primary_no_array_creation_expression* of an *array_type* and `A` is an *argument_list*, consists of the following steps:</span></span>

*  <span data-ttu-id="dec56-1114">`P` je vyhodnocen.</span><span class="sxs-lookup"><span data-stu-id="dec56-1114">`P` is evaluated.</span></span> <span data-ttu-id="dec56-1115">Je-li toto vyhodnocení způsobí výjimku, jsou spuštěny žádné další kroky.</span><span class="sxs-lookup"><span data-stu-id="dec56-1115">If this evaluation causes an exception, no further steps are executed.</span></span>
*  <span data-ttu-id="dec56-1116">Výrazy indexu *argument_list* se vyhodnocují v pořadí zleva doprava.</span><span class="sxs-lookup"><span data-stu-id="dec56-1116">The index expressions of the *argument_list* are evaluated in order, from left to right.</span></span> <span data-ttu-id="dec56-1117">Vyhodnocení výrazu každý index implicitní převod ([implicitních převodů](conversions.md#implicit-conversions)) se provádí na jednu z následujících typů: `int`, `uint`, `long`, `ulong`.</span><span class="sxs-lookup"><span data-stu-id="dec56-1117">Following evaluation of each index expression, an implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)) to one of the following types is performed: `int`, `uint`, `long`, `ulong`.</span></span> <span data-ttu-id="dec56-1118">První typ v tomto seznamu, pro kterou existuje implicitní převod je vybrán.</span><span class="sxs-lookup"><span data-stu-id="dec56-1118">The first type in this list for which an implicit conversion exists is chosen.</span></span> <span data-ttu-id="dec56-1119">Například pokud Indexový výraz je typu `short` pak implicitní převod na `int` provádí, protože implicitní převody z `short` k `int` a z `short` k `long` jsou možné.</span><span class="sxs-lookup"><span data-stu-id="dec56-1119">For instance, if the index expression is of type `short` then an implicit conversion to `int` is performed, since implicit conversions from `short` to `int` and from `short` to `long` are possible.</span></span> <span data-ttu-id="dec56-1120">Pokud limit vyhodnocení výrazu indexu nebo následné implicitní převod způsobí výjimku, žádné další indexové výrazy jsou vyhodnoceny a žádné další provádění kroků.</span><span class="sxs-lookup"><span data-stu-id="dec56-1120">If evaluation of an index expression or the subsequent implicit conversion causes an exception, then no further index expressions are evaluated and no further steps are executed.</span></span>
*  <span data-ttu-id="dec56-1121">Hodnota `P` je zaškrtnuté políčko platný.</span><span class="sxs-lookup"><span data-stu-id="dec56-1121">The value of `P` is checked to be valid.</span></span> <span data-ttu-id="dec56-1122">Pokud hodnota `P` je `null`, `System.NullReferenceException` je vyvolána, a že jsou provedeny žádné další kroky.</span><span class="sxs-lookup"><span data-stu-id="dec56-1122">If the value of `P` is `null`, a `System.NullReferenceException` is thrown and no further steps are executed.</span></span>
*  <span data-ttu-id="dec56-1123">Hodnota každý výraz v *argument_list* je porovnávána s skutečné mezí jednotlivých rozměrů pole instance odkazuje `P`.</span><span class="sxs-lookup"><span data-stu-id="dec56-1123">The value of each expression in the *argument_list* is checked against the actual bounds of each dimension of the array instance referenced by `P`.</span></span> <span data-ttu-id="dec56-1124">Pokud jeden nebo více hodnot je mimo rozsah, `System.IndexOutOfRangeException` je vyvolána, a že jsou provedeny žádné další kroky.</span><span class="sxs-lookup"><span data-stu-id="dec56-1124">If one or more values are out of range, a `System.IndexOutOfRangeException` is thrown and no further steps are executed.</span></span>
*  <span data-ttu-id="dec56-1125">Umístění prvku pole, které jsou uvedena v každém výrazů indexu je vypočítán a toto umístění bude výsledek přístup k poli.</span><span class="sxs-lookup"><span data-stu-id="dec56-1125">The location of the array element given by the index expression(s) is computed, and this location becomes the result of the array access.</span></span>

#### <a name="indexer-access"></a><span data-ttu-id="dec56-1126">Přístup indexeru</span><span class="sxs-lookup"><span data-stu-id="dec56-1126">Indexer access</span></span>

<span data-ttu-id="dec56-1127">Pro přístup indexeru *primary_no_array_creation_expression* z *element_access* musí být proměnná nebo hodnota třídy, struktury nebo typ rozhraní, a tento typ musí implementovat jednu nebo více indexery, které se dají použít s ohledem na *argument_list* z *element_access*.</span><span class="sxs-lookup"><span data-stu-id="dec56-1127">For an indexer access, the *primary_no_array_creation_expression* of the *element_access* must be a variable or value of a class, struct, or interface type, and this type must implement one or more indexers that are applicable with respect to the *argument_list* of the *element_access*.</span></span>

<span data-ttu-id="dec56-1128">Vazba čas zpracování přístupového indexer formuláře `P[A]`, kde `P` je *primary_no_array_creation_expression* třídy, struktury nebo rozhraní typu `T`, a `A` je *argument_list*, se skládá z následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="dec56-1128">The binding-time processing of an indexer access of the form `P[A]`, where `P` is a *primary_no_array_creation_expression* of a class, struct, or interface type `T`, and `A` is an *argument_list*, consists of the following steps:</span></span>

*  <span data-ttu-id="dec56-1129">Sada indexery poskytované `T` je vytvořený.</span><span class="sxs-lookup"><span data-stu-id="dec56-1129">The set of indexers provided by `T` is constructed.</span></span> <span data-ttu-id="dec56-1130">Sada se skládá ze všech indexerů, které jsou deklarované v `T` nebo základního typu `T` , které nejsou `override` deklarace a jsou v aktuálním kontextu k dispozici ([přístup ke členu](basic-concepts.md#member-access)).</span><span class="sxs-lookup"><span data-stu-id="dec56-1130">The set consists of all indexers declared in `T` or a base type of `T` that are not `override` declarations and are accessible in the current context ([Member access](basic-concepts.md#member-access)).</span></span>
*  <span data-ttu-id="dec56-1131">Sada je omezit na tyto indexerů, které jsou použitelné a nikoli podle jinými indexery.</span><span class="sxs-lookup"><span data-stu-id="dec56-1131">The set is reduced to those indexers that are applicable and not hidden by other indexers.</span></span> <span data-ttu-id="dec56-1132">Následující pravidla platí pro každý indexer `S.I` v sadě, kde `S` je typ, ve kterém indexeru `I` je deklarována jako:</span><span class="sxs-lookup"><span data-stu-id="dec56-1132">The following rules are applied to each indexer `S.I` in the set, where `S` is the type in which the indexer `I` is declared:</span></span>
   * <span data-ttu-id="dec56-1133">Pokud `I` se nedá použít s ohledem na `A` ([použitelná funkce člena](expressions.md#applicable-function-member)), pak `I` je odebrán ze sady.</span><span class="sxs-lookup"><span data-stu-id="dec56-1133">If `I` is not applicable with respect to `A` ([Applicable function member](expressions.md#applicable-function-member)), then `I` is removed from the set.</span></span>
   * <span data-ttu-id="dec56-1134">Pokud `I` lze použít s ohledem na `A` ([použitelná funkce člena](expressions.md#applicable-function-member)), pak všechny indexery deklarovaný v základní typ `S` se odeberou ze sady.</span><span class="sxs-lookup"><span data-stu-id="dec56-1134">If `I` is applicable with respect to `A` ([Applicable function member](expressions.md#applicable-function-member)), then all indexers declared in a base type of `S` are removed from the set.</span></span>
   * <span data-ttu-id="dec56-1135">Pokud `I` lze použít s ohledem na `A` ([použitelná funkce člena](expressions.md#applicable-function-member)) a `S` typu třídy je jiné než `object`, všechny indexery deklarované v rozhraní se odeberou ze sady.</span><span class="sxs-lookup"><span data-stu-id="dec56-1135">If `I` is applicable with respect to `A` ([Applicable function member](expressions.md#applicable-function-member)) and `S` is a class type other than `object`, all indexers declared in an interface are removed from the set.</span></span>
*  <span data-ttu-id="dec56-1136">Pokud výslednou sadu Release candidate indexerů je prázdný, pak neexistují žádné použitelné indexery a dojde k chybě vazby čas.</span><span class="sxs-lookup"><span data-stu-id="dec56-1136">If the resulting set of candidate indexers is empty, then no applicable indexers exist, and a binding-time error occurs.</span></span>
*  <span data-ttu-id="dec56-1137">Nejlepší indexer sadu Release candidate indexerů je identifikován pomocí pravidel rozlišení přetížení [rozlišení přetěžování](expressions.md#overload-resolution).</span><span class="sxs-lookup"><span data-stu-id="dec56-1137">The best indexer of the set of candidate indexers is identified using the overload resolution rules of [Overload resolution](expressions.md#overload-resolution).</span></span> <span data-ttu-id="dec56-1138">Pokud jeden nejlepší indexer nelze identifikovat, přístup indexeru je nejednoznačný a dojde k chybě vazby čas.</span><span class="sxs-lookup"><span data-stu-id="dec56-1138">If a single best indexer cannot be identified, the indexer access is ambiguous, and a binding-time error occurs.</span></span>
*  <span data-ttu-id="dec56-1139">Výrazy indexu *argument_list* se vyhodnocují v pořadí zleva doprava.</span><span class="sxs-lookup"><span data-stu-id="dec56-1139">The index expressions of the *argument_list* are evaluated in order, from left to right.</span></span> <span data-ttu-id="dec56-1140">Výsledek zpracování přístup indexeru je výraz jsou klasifikovány jako přístup indexeru.</span><span class="sxs-lookup"><span data-stu-id="dec56-1140">The result of processing the indexer access is an expression classified as an indexer access.</span></span> <span data-ttu-id="dec56-1141">Výraz přístup indexeru odkazuje na indexer nadefinovali v předchozím kroku a má přidruženou instanci výraz `P` a seznam argumentů přidružený `A`.</span><span class="sxs-lookup"><span data-stu-id="dec56-1141">The indexer access expression references the indexer determined in the step above, and has an associated instance expression of `P` and an associated argument list of `A`.</span></span>

<span data-ttu-id="dec56-1142">V závislosti na kontextu, ve kterém se používá, přístup indexeru způsobí, že volání buď *načtení přístupového objektu* nebo *nastavení přístupového objektu* indexeru.</span><span class="sxs-lookup"><span data-stu-id="dec56-1142">Depending on the context in which it is used, an indexer access causes invocation of either the *get accessor* or the *set accessor* of the indexer.</span></span> <span data-ttu-id="dec56-1143">Pokud přístup indexeru je cílem přiřazení, *nastavení přístupového objektu* se vyvolá, aby přiřadí novou hodnotu ([jednoduché přiřazení](expressions.md#simple-assignment)).</span><span class="sxs-lookup"><span data-stu-id="dec56-1143">If the indexer access is the target of an assignment, the *set accessor* is invoked to assign a new value ([Simple assignment](expressions.md#simple-assignment)).</span></span> <span data-ttu-id="dec56-1144">Ve všech ostatních případech *načtení přístupového objektu* se vyvolá k získání aktuální hodnoty ([hodnot výrazů](expressions.md#values-of-expressions)).</span><span class="sxs-lookup"><span data-stu-id="dec56-1144">In all other cases, the *get accessor* is invoked to obtain the current value ([Values of expressions](expressions.md#values-of-expressions)).</span></span>

### <a name="this-access"></a><span data-ttu-id="dec56-1145">Tento přístup</span><span class="sxs-lookup"><span data-stu-id="dec56-1145">This access</span></span>

<span data-ttu-id="dec56-1146">A *this_access* se skládá z vyhrazené slovo `this`.</span><span class="sxs-lookup"><span data-stu-id="dec56-1146">A *this_access* consists of the reserved word `this`.</span></span>

```antlr
this_access
    : 'this'
    ;
```

<span data-ttu-id="dec56-1147">A *this_access* smí obsahovat pouze v *bloku* konstruktor instance, metoda instance nebo přístupový objekt instance.</span><span class="sxs-lookup"><span data-stu-id="dec56-1147">A *this_access* is permitted only in the *block* of an instance constructor, an instance method, or an instance accessor.</span></span> <span data-ttu-id="dec56-1148">Má jednu z následujících význam:</span><span class="sxs-lookup"><span data-stu-id="dec56-1148">It has one of the following meanings:</span></span>

*  <span data-ttu-id="dec56-1149">Když `this` je používán *primary_expression* v rámci konstruktoru instance třídy, je klasifikován jako hodnotu.</span><span class="sxs-lookup"><span data-stu-id="dec56-1149">When `this` is used in a *primary_expression* within an instance constructor of a class, it is classified as a value.</span></span> <span data-ttu-id="dec56-1150">Typ hodnoty je typ instance ([typ instance](classes.md#the-instance-type)) třídy, ve kterém dojde k použití a hodnota je odkaz na objekt, který vytváří.</span><span class="sxs-lookup"><span data-stu-id="dec56-1150">The type of the value is the instance type ([The instance type](classes.md#the-instance-type)) of the class within which the usage occurs, and the value is a reference to the object being constructed.</span></span>
*  <span data-ttu-id="dec56-1151">Když `this` je používán *primary_expression* v rámci metody instance nebo přístupový objekt instance třídy, je klasifikován jako hodnotu.</span><span class="sxs-lookup"><span data-stu-id="dec56-1151">When `this` is used in a *primary_expression* within an instance method or instance accessor of a class, it is classified as a value.</span></span> <span data-ttu-id="dec56-1152">Typ hodnoty je typ instance ([typ instance](classes.md#the-instance-type)) třídy, ve kterém dojde k použití a hodnota je odkaz na objekt, pro kterou byla vyvolána metoda nebo přístupový objekt.</span><span class="sxs-lookup"><span data-stu-id="dec56-1152">The type of the value is the instance type ([The instance type](classes.md#the-instance-type)) of the class within which the usage occurs, and the value is a reference to the object for which the method or accessor was invoked.</span></span>
*  <span data-ttu-id="dec56-1153">Když `this` je používán *primary_expression* v rámci konstruktoru instance struktury, je klasifikován jako proměnnou.</span><span class="sxs-lookup"><span data-stu-id="dec56-1153">When `this` is used in a *primary_expression* within an instance constructor of a struct, it is classified as a variable.</span></span> <span data-ttu-id="dec56-1154">Typ proměnné je typ instance ([typ instance](classes.md#the-instance-type)) struktury, ve kterém dojde k použití a představuje proměnnou struktury vytváří.</span><span class="sxs-lookup"><span data-stu-id="dec56-1154">The type of the variable is the instance type ([The instance type](classes.md#the-instance-type)) of the struct within which the usage occurs, and the variable represents the struct being constructed.</span></span> <span data-ttu-id="dec56-1155">`this` Proměnné konstruktor instance struktury se chová stejně jako `out` parametr typu struct – konkrétně to znamená, že proměnná musí být jednoznačně přiřazena v jednotlivých cestách spuštění instance konstruktor.</span><span class="sxs-lookup"><span data-stu-id="dec56-1155">The `this` variable of an instance constructor of a struct behaves exactly the same as an `out` parameter of the struct type—in particular, this means that the variable must be definitely assigned in every execution path of the instance constructor.</span></span>
*  <span data-ttu-id="dec56-1156">Když `this` je používán *primary_expression* v rámci metody instance nebo přístupový objekt instance struktury, je klasifikován jako proměnnou.</span><span class="sxs-lookup"><span data-stu-id="dec56-1156">When `this` is used in a *primary_expression* within an instance method or instance accessor of a struct, it is classified as a variable.</span></span> <span data-ttu-id="dec56-1157">Typ proměnné je typ instance ([typ instance](classes.md#the-instance-type)) struktury, ve kterém dochází k použití.</span><span class="sxs-lookup"><span data-stu-id="dec56-1157">The type of the variable is the instance type ([The instance type](classes.md#the-instance-type)) of the struct within which the usage occurs.</span></span>
   * <span data-ttu-id="dec56-1158">Pokud metoda nebo přístupový objekt není iterátor ([iterátory](classes.md#iterators)), `this` proměnná představuje strukturu, pro který se vyvolala metodu nebo přistupující objekt a jak se bude chovat stejně jako `ref` parametr typu Struktura.</span><span class="sxs-lookup"><span data-stu-id="dec56-1158">If the method or accessor is not an iterator ([Iterators](classes.md#iterators)), the `this` variable represents the struct for which the method or accessor was invoked, and behaves exactly the same as a `ref` parameter of the struct type.</span></span>
   * <span data-ttu-id="dec56-1159">Pokud metoda nebo přístupový objekt iterátoru, `this` proměnná představuje kopii struktury, pro který byla vyvolána metoda nebo přístupový objekt a chová stejně jako hodnota parametru typu Struktura.</span><span class="sxs-lookup"><span data-stu-id="dec56-1159">If the method or accessor is an iterator, the `this` variable represents a copy of the struct for which the method or accessor was invoked, and behaves exactly the same as a value parameter of the struct type.</span></span>

<span data-ttu-id="dec56-1160">Použití `this` v *primary_expression* v jiném kontextu než jsou výše uvedené je chyba kompilace.</span><span class="sxs-lookup"><span data-stu-id="dec56-1160">Use of `this` in a *primary_expression* in a context other than the ones listed above is a compile-time error.</span></span> <span data-ttu-id="dec56-1161">Zejména, není možné odkazovat na `this` v statická metoda, přístupový objekt statické vlastnosti, nebo *variable_initializer* deklarace pole.</span><span class="sxs-lookup"><span data-stu-id="dec56-1161">In particular, it is not possible to refer to `this` in a static method, a static property accessor, or in a *variable_initializer* of a field declaration.</span></span>

### <a name="base-access"></a><span data-ttu-id="dec56-1162">Základní přístup</span><span class="sxs-lookup"><span data-stu-id="dec56-1162">Base access</span></span>

<span data-ttu-id="dec56-1163">A *base_access* se skládá z vyhrazené slovo `base` následována buď "`.`" token a identifikátor nebo s *argument_list* uzavřeny do hranatých závorek:</span><span class="sxs-lookup"><span data-stu-id="dec56-1163">A *base_access* consists of the reserved word `base` followed by either a "`.`" token and an identifier or an *argument_list* enclosed in square brackets:</span></span>

```antlr
base_access
    : 'base' '.' identifier
    | 'base' '[' expression_list ']'
    ;
```

<span data-ttu-id="dec56-1164">A *base_access* slouží k přístupu k členům základní třídy, které jsou skryté členy podobně pojmenovaných v aktuální třídě nebo struktuře.</span><span class="sxs-lookup"><span data-stu-id="dec56-1164">A *base_access* is used to access base class members that are hidden by similarly named members in the current class or struct.</span></span> <span data-ttu-id="dec56-1165">A *base_access* smí obsahovat pouze v *bloku* konstruktor instance, metoda instance nebo přístupový objekt instance.</span><span class="sxs-lookup"><span data-stu-id="dec56-1165">A *base_access* is permitted only in the *block* of an instance constructor, an instance method, or an instance accessor.</span></span> <span data-ttu-id="dec56-1166">Když `base.I` v třídě nebo struktuře, dojde k `I` musí označení člena základní třídy této třídy nebo struktury.</span><span class="sxs-lookup"><span data-stu-id="dec56-1166">When `base.I` occurs in a class or struct, `I` must denote a member of the base class of that class or struct.</span></span> <span data-ttu-id="dec56-1167">Podobně když `base[E]` ve třídě, vyvolá se v základní třídě musí existovat odpovídající indexer.</span><span class="sxs-lookup"><span data-stu-id="dec56-1167">Likewise, when `base[E]` occurs in a class, an applicable indexer must exist in the base class.</span></span>

<span data-ttu-id="dec56-1168">Během vazby *base_access* výrazy formuláře `base.I` a `base[E]` vyhodnocují stejně, jako kdyby byly napsány `((B)this).I` a `((B)this)[E]`, kde `B` je základní třídou třídy nebo struktury, ve kterém dojde k vytvoření.</span><span class="sxs-lookup"><span data-stu-id="dec56-1168">At binding-time, *base_access* expressions of the form `base.I` and `base[E]` are evaluated exactly as if they were written `((B)this).I` and `((B)this)[E]`, where `B` is the base class of the class or struct in which the construct occurs.</span></span> <span data-ttu-id="dec56-1169">Proto `base.I` a `base[E]` odpovídají `this.I` a `this[E]`, s výjimkou `this` se pohlíží jako instance základní třídy.</span><span class="sxs-lookup"><span data-stu-id="dec56-1169">Thus, `base.I` and `base[E]` correspond to `this.I` and `this[E]`, except `this` is viewed as an instance of the base class.</span></span>

<span data-ttu-id="dec56-1170">Když *base_access* odkazuje na virtuální funkce člena (metoda, vlastnost nebo indexer), určení funkce člena, který má být vyvolán při spuštění ([kompilace kontrolu dynamické přetížení ](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) se změní.</span><span class="sxs-lookup"><span data-stu-id="dec56-1170">When a *base_access* references a virtual function member (a method, property, or indexer), the determination of which function member to invoke at run-time ([Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) is changed.</span></span> <span data-ttu-id="dec56-1171">Člen funkce, která je vyvolána je určen tím, že hledá implementace Většina odvozených ([virtuální metody](classes.md#virtual-methods)) z členské funkce s ohledem na `B` (místo s ohledem na typu za běhu `this`, jako by obvykle v jiných základní přístup).</span><span class="sxs-lookup"><span data-stu-id="dec56-1171">The function member that is invoked is determined by finding the most derived implementation ([Virtual methods](classes.md#virtual-methods)) of the function member with respect to `B` (instead of with respect to the run-time type of `this`, as would be usual in a non-base access).</span></span> <span data-ttu-id="dec56-1172">Díky tomu se v rámci `override` z `virtual` členské funkce *base_access* můžete použít k vyvolání zděděná implementace metody členské funkce.</span><span class="sxs-lookup"><span data-stu-id="dec56-1172">Thus, within an `override` of a `virtual` function member, a *base_access* can be used to invoke the inherited implementation of the function member.</span></span> <span data-ttu-id="dec56-1173">Pokud odkazuje členské funkce *base_access* je abstraktní, dojde k chybě vazby čas.</span><span class="sxs-lookup"><span data-stu-id="dec56-1173">If the function member referenced by a *base_access* is abstract, a binding-time error occurs.</span></span>

### <a name="postfix-increment-and-decrement-operators"></a><span data-ttu-id="dec56-1174">Příponové operátory Inkrementace a dekrementace operátory</span><span class="sxs-lookup"><span data-stu-id="dec56-1174">Postfix increment and decrement operators</span></span>

```antlr
post_increment_expression
    : primary_expression '++'
    ;

post_decrement_expression
    : primary_expression '--'
    ;
```

<span data-ttu-id="dec56-1175">Operand operace přípony zvýšení nebo snížení musí být výraz jsou klasifikovány jako proměnné, přístup k vlastnosti nebo indexeru přístupu.</span><span class="sxs-lookup"><span data-stu-id="dec56-1175">The operand of a postfix increment or decrement operation must be an expression classified as a variable, a property access, or an indexer access.</span></span> <span data-ttu-id="dec56-1176">Výsledkem operace je hodnota stejného typu jako operand.</span><span class="sxs-lookup"><span data-stu-id="dec56-1176">The result of the operation is a value of the same type as the operand.</span></span>

<span data-ttu-id="dec56-1177">Pokud *primary_expression* má typ kompilace `dynamic` pak operátor, který je vázán dynamicky ([dynamické vazby](expressions.md#dynamic-binding)), *post_increment_expression*nebo *post_decrement_expression* má typ kompilace `dynamic` a za běhu pomocí typu za běhu se použijí následující pravidla *primary_expression*.</span><span class="sxs-lookup"><span data-stu-id="dec56-1177">If the *primary_expression* has the compile-time type `dynamic` then the operator is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)), the *post_increment_expression* or *post_decrement_expression* has the compile-time type `dynamic` and the following rules are applied at run-time using the run-time type of the *primary_expression*.</span></span>

<span data-ttu-id="dec56-1178">Je-li zvýšit operand příponový operace dekrementace je vlastnost nebo indexer přístup, vlastnost nebo indexer musí mít obě `get` a `set` přistupujícího objektu.</span><span class="sxs-lookup"><span data-stu-id="dec56-1178">If the operand of a postfix increment or decrement operation is a property or indexer access, the property or indexer must have both a `get` and a `set` accessor.</span></span> <span data-ttu-id="dec56-1179">Pokud to není tento případ, dojde k chybě vazby čas.</span><span class="sxs-lookup"><span data-stu-id="dec56-1179">If this is not the case, a binding-time error occurs.</span></span>

<span data-ttu-id="dec56-1180">Unární operátor rozlišení přetěžování ([unární operátor rozlišení přetěžování](expressions.md#unary-operator-overload-resolution)) se použije k výběru na konkrétní operátor implementace.</span><span class="sxs-lookup"><span data-stu-id="dec56-1180">Unary operator overload resolution ([Unary operator overload resolution](expressions.md#unary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="dec56-1181">Předdefinované `++` a `--` operátory existovat pro následující typy: `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char` , `float`, `double`, `decimal`a jakýkoli typ výčtu.</span><span class="sxs-lookup"><span data-stu-id="dec56-1181">Predefined `++` and `--` operators exist for the following types: `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, `decimal`, and any enum type.</span></span> <span data-ttu-id="dec56-1182">Předdefinované `++` operátory návratová hodnota vytváří přidáním 1 operand a předdefinované `--` operátory návratová hodnota vytváří tak, že se 1 z operand.</span><span class="sxs-lookup"><span data-stu-id="dec56-1182">The predefined `++` operators return the value produced by adding 1 to the operand, and the predefined `--` operators return the value produced by subtracting 1 from the operand.</span></span> <span data-ttu-id="dec56-1183">V `checked` kontextu, pokud výsledek této sčítání a odčítání je mimo rozsah typu výsledku a typ výsledku je integrální typ nebo typ výčtu `System.OverflowException` je vyvolána výjimka.</span><span class="sxs-lookup"><span data-stu-id="dec56-1183">In a `checked` context, if the result of this addition or subtraction is outside the range of the result type and the result type is an integral type or enum type, a `System.OverflowException` is thrown.</span></span>

<span data-ttu-id="dec56-1184">Zpracování za běhu přípony přírůstek nebo snížení operace formuláře `x++` nebo `x--` se skládá z následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="dec56-1184">The run-time processing of a postfix increment or decrement operation of the form `x++` or `x--` consists of the following steps:</span></span>

*   <span data-ttu-id="dec56-1185">Pokud `x` je klasifikován jako proměnnou:</span><span class="sxs-lookup"><span data-stu-id="dec56-1185">If `x` is classified as a variable:</span></span>
    * <span data-ttu-id="dec56-1186">`x` je vyhodnocen pro vytvoření proměnné.</span><span class="sxs-lookup"><span data-stu-id="dec56-1186">`x` is evaluated to produce the variable.</span></span>
    * <span data-ttu-id="dec56-1187">Hodnota `x` se uloží.</span><span class="sxs-lookup"><span data-stu-id="dec56-1187">The value of `x` is saved.</span></span>
    * <span data-ttu-id="dec56-1188">S použitím uložené hodnoty z je vyvolána zvoleném operátorovi `x` jako svůj argument.</span><span class="sxs-lookup"><span data-stu-id="dec56-1188">The selected operator is invoked with the saved value of `x` as its argument.</span></span>
    * <span data-ttu-id="dec56-1189">Hodnota vrácená operátorem je uložen v umístění Dal vyhodnocení `x`.</span><span class="sxs-lookup"><span data-stu-id="dec56-1189">The value returned by the operator is stored in the location given by the evaluation of `x`.</span></span>
    * <span data-ttu-id="dec56-1190">Uložená hodnota `x` výsledek operace.</span><span class="sxs-lookup"><span data-stu-id="dec56-1190">The saved value of `x` becomes the result of the operation.</span></span>
*   <span data-ttu-id="dec56-1191">Pokud `x` klasifikovaný jako vlastnost nebo indexovací člen přístupu:</span><span class="sxs-lookup"><span data-stu-id="dec56-1191">If `x` is classified as a property or indexer access:</span></span>
    * <span data-ttu-id="dec56-1192">Výraz instance (Pokud `x` není `static`) a v seznamu argumentů (Pokud `x` představuje přístup k indexer) přidružené k `x` jsou vyhodnocovány, a výsledky se používají v následné `get` a `set` přístupový objekt volání.</span><span class="sxs-lookup"><span data-stu-id="dec56-1192">The instance expression (if `x` is not `static`) and the argument list (if `x` is an indexer access) associated with `x` are evaluated, and the results are used in the subsequent `get` and `set` accessor invocations.</span></span>
    * <span data-ttu-id="dec56-1193">`get` Přistupující objekt `x` je vyvolána a vrácené hodnoty se uloží.</span><span class="sxs-lookup"><span data-stu-id="dec56-1193">The `get` accessor of `x` is invoked and the returned value is saved.</span></span>
    * <span data-ttu-id="dec56-1194">S použitím uložené hodnoty z je vyvolána zvoleném operátorovi `x` jako svůj argument.</span><span class="sxs-lookup"><span data-stu-id="dec56-1194">The selected operator is invoked with the saved value of `x` as its argument.</span></span>
    * <span data-ttu-id="dec56-1195">`set` Přistupující objekt `x` vyvolání hodnota vrácená operátorem jako jeho `value` argument.</span><span class="sxs-lookup"><span data-stu-id="dec56-1195">The `set` accessor of `x` is invoked with the value returned by the operator as its `value` argument.</span></span>
    * <span data-ttu-id="dec56-1196">Uložená hodnota `x` výsledek operace.</span><span class="sxs-lookup"><span data-stu-id="dec56-1196">The saved value of `x` becomes the result of the operation.</span></span>

<span data-ttu-id="dec56-1197">`++` a `--` operátory také podporují předpony ([předpony Inkrementace a dekrementace operátory](expressions.md#prefix-increment-and-decrement-operators)).</span><span class="sxs-lookup"><span data-stu-id="dec56-1197">The `++` and `--` operators also support prefix notation ([Prefix increment and decrement operators](expressions.md#prefix-increment-and-decrement-operators)).</span></span> <span data-ttu-id="dec56-1198">Obvykle, výsledek `x++` nebo `x--` je hodnota `x` před provedením operace, zatímco výsledek `++x` nebo `--x` je hodnota `x` po provedení této operace.</span><span class="sxs-lookup"><span data-stu-id="dec56-1198">Typically, the result of `x++` or `x--` is the value of `x` before the operation, whereas the result of `++x` or `--x` is the value of `x` after the operation.</span></span> <span data-ttu-id="dec56-1199">V obou případech `x` sám po provedení této operace má stejnou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="dec56-1199">In either case, `x` itself has the same value after the operation.</span></span>

<span data-ttu-id="dec56-1200">`operator ++` Nebo `operator --` implementace lze vyvolat pomocí uplatněna přípona nebo předpona zápisu.</span><span class="sxs-lookup"><span data-stu-id="dec56-1200">An `operator ++` or `operator --` implementation can be invoked using either postfix or prefix notation.</span></span> <span data-ttu-id="dec56-1201">Není možné mít samostatné operátor implementace pro zápisy dva.</span><span class="sxs-lookup"><span data-stu-id="dec56-1201">It is not possible to have separate operator implementations for the two notations.</span></span>

### <a name="the-new-operator"></a><span data-ttu-id="dec56-1202">New – operátor</span><span class="sxs-lookup"><span data-stu-id="dec56-1202">The new operator</span></span>

<span data-ttu-id="dec56-1203">`new` Operátor se používá k vytvoření nové instance typů.</span><span class="sxs-lookup"><span data-stu-id="dec56-1203">The `new` operator is used to create new instances of types.</span></span>

<span data-ttu-id="dec56-1204">Existují tři formy `new` výrazy:</span><span class="sxs-lookup"><span data-stu-id="dec56-1204">There are three forms of `new` expressions:</span></span>

*  <span data-ttu-id="dec56-1205">Výrazy vytváření objektů se používají k vytvoření nové instance typů tříd a typů hodnot.</span><span class="sxs-lookup"><span data-stu-id="dec56-1205">Object creation expressions are used to create new instances of class types and value types.</span></span>
*  <span data-ttu-id="dec56-1206">Výrazy vytvoření pole se používají k vytvoření nové instance typy polí.</span><span class="sxs-lookup"><span data-stu-id="dec56-1206">Array creation expressions are used to create new instances of array types.</span></span>
*  <span data-ttu-id="dec56-1207">Výrazy vytvoření delegáta se používají k vytvoření nové instance delegáta typů.</span><span class="sxs-lookup"><span data-stu-id="dec56-1207">Delegate creation expressions are used to create new instances of delegate types.</span></span>

<span data-ttu-id="dec56-1208">`new` Operátor zahrnuje vytvoření instance typu, ale nutně neznamená dynamické přidělování paměti.</span><span class="sxs-lookup"><span data-stu-id="dec56-1208">The `new` operator implies creation of an instance of a type, but does not necessarily imply dynamic allocation of memory.</span></span> <span data-ttu-id="dec56-1209">Konkrétně se instance typů hodnot vyžadují žádné další paměť nad rámec proměnné, ve kterých se nacházejí, a dojde k žádné dynamické přidělení při `new` slouží k vytvoření instancí typů hodnot.</span><span class="sxs-lookup"><span data-stu-id="dec56-1209">In particular, instances of value types require no additional memory beyond the variables in which they reside, and no dynamic allocations occur when `new` is used to create instances of value types.</span></span>

#### <a name="object-creation-expressions"></a><span data-ttu-id="dec56-1210">Výrazy vytváření objektů</span><span class="sxs-lookup"><span data-stu-id="dec56-1210">Object creation expressions</span></span>

<span data-ttu-id="dec56-1211">*Object_creation_expression* umožňuje vytvořit novou instanci třídy *class_type* nebo *value_type*.</span><span class="sxs-lookup"><span data-stu-id="dec56-1211">An *object_creation_expression* is used to create a new instance of a *class_type* or a *value_type*.</span></span>

```antlr
object_creation_expression
    : 'new' type '(' argument_list? ')' object_or_collection_initializer?
    | 'new' type object_or_collection_initializer
    ;

object_or_collection_initializer
    : object_initializer
    | collection_initializer
    ;
```

<span data-ttu-id="dec56-1212">*Typ* ze *object_creation_expression* musí být *class_type*, *value_type* nebo *type_parameter* .</span><span class="sxs-lookup"><span data-stu-id="dec56-1212">The *type* of an *object_creation_expression* must be a *class_type*, a *value_type* or a *type_parameter*.</span></span> <span data-ttu-id="dec56-1213">*Typ* nemůže být `abstract` *class_type*.</span><span class="sxs-lookup"><span data-stu-id="dec56-1213">The *type* cannot be an `abstract` *class_type*.</span></span>

<span data-ttu-id="dec56-1214">Volitelný *argument_list* ([seznamy argumentů](expressions.md#argument-lists)) je povolený jenom v případě *typ* je *class_type* nebo *struct_ typ*.</span><span class="sxs-lookup"><span data-stu-id="dec56-1214">The optional *argument_list* ([Argument lists](expressions.md#argument-lists)) is permitted only if the *type* is a *class_type* or a *struct_type*.</span></span>

<span data-ttu-id="dec56-1215">Vytvoření výrazu objektu lze vynechat seznam argumentů konstruktoru a uzavírající závorky za předpokladu, že obsahuje objekt inicializátor nebo inicializátor kolekce.</span><span class="sxs-lookup"><span data-stu-id="dec56-1215">An object creation expression can omit the constructor argument list and enclosing parentheses provided it includes an object initializer or collection initializer.</span></span> <span data-ttu-id="dec56-1216">Vynechání seznamu argumentů konstruktoru a uzavírající závorky je ekvivalentní se zadáním prázdným seznamem argumentů.</span><span class="sxs-lookup"><span data-stu-id="dec56-1216">Omitting the constructor argument list and enclosing parentheses is equivalent to specifying an empty argument list.</span></span>

<span data-ttu-id="dec56-1217">Zpracování výrazu vytvoření objektu, který obsahuje objekt inicializátor nebo inicializátor kolekce se skládá z první zpracování konstruktor instance a potom zpracování zadaná pomocí inicializátoru objektů (inicializacíčlenneboelement[ Inicializátory objektu](expressions.md#object-initializers)) nebo inicializátor kolekce ([inicializátory kolekce](expressions.md#collection-initializers)).</span><span class="sxs-lookup"><span data-stu-id="dec56-1217">Processing of an object creation expression that includes an object initializer or collection initializer consists of first processing the instance constructor and then processing the member or element initializations specified by the object initializer ([Object initializers](expressions.md#object-initializers)) or collection initializer ([Collection initializers](expressions.md#collection-initializers)).</span></span>

<span data-ttu-id="dec56-1218">Pokud některý z argumentů volitelné *argument_list* má typ kompilace `dynamic` pak bude *object_creation_expression* dynamicky vázán ([dynamické vazby](expressions.md#dynamic-binding)) a za běhu pomocí typu za běhu z těchto argumentů se použijí následující pravidla *argument_list* , které mají typ času kompilace `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="dec56-1218">If any of the arguments in the optional *argument_list* has the compile-time type `dynamic` then the *object_creation_expression* is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)) and the following rules are applied at run-time using the run-time type of those arguments of the *argument_list* that have the compile time type `dynamic`.</span></span> <span data-ttu-id="dec56-1219">Ale při vytvoření objektu Kontrola času kompilace omezené jak je popsáno v [kompilace kontrolu dynamické přetížení](expressions.md#compile-time-checking-of-dynamic-overload-resolution).</span><span class="sxs-lookup"><span data-stu-id="dec56-1219">However, the object creation undergoes a limited compile time check as described in [Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution).</span></span>

<span data-ttu-id="dec56-1220">Zpracování doba vazby *object_creation_expression* formuláře `new T(A)`, kde `T` je *class_type* nebo *value_type* a `A` je volitelný *argument_list*, se skládá z následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="dec56-1220">The binding-time processing of an *object_creation_expression* of the form `new T(A)`, where `T` is a *class_type* or a *value_type* and `A` is an optional *argument_list*, consists of the following steps:</span></span>

*   <span data-ttu-id="dec56-1221">Pokud `T` je *value_type* a `A` není k dispozici:</span><span class="sxs-lookup"><span data-stu-id="dec56-1221">If `T` is a *value_type* and `A` is not present:</span></span>
    * <span data-ttu-id="dec56-1222">*Object_creation_expression* je vyvolání výchozího konstruktoru.</span><span class="sxs-lookup"><span data-stu-id="dec56-1222">The *object_creation_expression* is a default constructor invocation.</span></span> <span data-ttu-id="dec56-1223">Výsledek *object_creation_expression* je hodnota typu `T`, a to výchozí hodnota pro `T` jak jsou definovány v [typ The System.ValueType](types.md#the-systemvaluetype-type).</span><span class="sxs-lookup"><span data-stu-id="dec56-1223">The result of the *object_creation_expression* is a value of type `T`, namely the default value for `T` as defined in [The System.ValueType type](types.md#the-systemvaluetype-type).</span></span>
*   <span data-ttu-id="dec56-1224">Jinak, pokud `T` je *type_parameter* a `A` není k dispozici:</span><span class="sxs-lookup"><span data-stu-id="dec56-1224">Otherwise, if `T` is a *type_parameter* and `A` is not present:</span></span>
    * <span data-ttu-id="dec56-1225">Pokud žádná hodnota typu omezení nebo omezení konstruktoru ([omezení parametru typu](classes.md#type-parameter-constraints)) byl zadán pro `T`, dojde k chybě vazby čas.</span><span class="sxs-lookup"><span data-stu-id="dec56-1225">If no value type constraint or constructor constraint ([Type parameter constraints](classes.md#type-parameter-constraints)) has been specified for `T`, a binding-time error occurs.</span></span>
    * <span data-ttu-id="dec56-1226">Výsledek *object_creation_expression* je hodnota typu za běhu, svázané parametr typu, jmenovitě výsledek volání výchozího konstruktoru typu.</span><span class="sxs-lookup"><span data-stu-id="dec56-1226">The result of the *object_creation_expression* is a value of the run-time type that the type parameter has been bound to, namely the result of invoking the default constructor of that type.</span></span> <span data-ttu-id="dec56-1227">Run-time typu může být typem odkazu nebo hodnotového typu.</span><span class="sxs-lookup"><span data-stu-id="dec56-1227">The run-time type may be a reference type or a value type.</span></span>
*   <span data-ttu-id="dec56-1228">Jinak, pokud `T` je *class_type* nebo *struct_type*:</span><span class="sxs-lookup"><span data-stu-id="dec56-1228">Otherwise, if `T` is a *class_type* or a *struct_type*:</span></span>
    * <span data-ttu-id="dec56-1229">Pokud `T` je `abstract` *class_type*, dojde k chybě kompilace.</span><span class="sxs-lookup"><span data-stu-id="dec56-1229">If `T` is an `abstract` *class_type*, a compile-time error occurs.</span></span>
    * <span data-ttu-id="dec56-1230">Konstruktor instance, který má být vyvolán je určen pomocí pravidel rozlišení přetížení [rozlišení přetěžování](expressions.md#overload-resolution).</span><span class="sxs-lookup"><span data-stu-id="dec56-1230">The instance constructor to invoke is determined using the overload resolution rules of [Overload resolution](expressions.md#overload-resolution).</span></span> <span data-ttu-id="dec56-1231">Sada konstruktory instancí Release candidate zahrnuje všechny dostupné instance konstruktory deklarované v `T` které se dají použít s ohledem na `A` ([použít funkční člen](expressions.md#applicable-function-member)).</span><span class="sxs-lookup"><span data-stu-id="dec56-1231">The set of candidate instance constructors consists of all accessible instance constructors declared in `T` which are applicable with respect to `A` ([Applicable function member](expressions.md#applicable-function-member)).</span></span> <span data-ttu-id="dec56-1232">Pokud sadu Release candidate konstruktory instancí je prázdná nebo jediný konstruktor instance nejlepší nelze identifikovat, dojde k chybě vazby čas.</span><span class="sxs-lookup"><span data-stu-id="dec56-1232">If the set of candidate instance constructors is empty, or if a single best instance constructor cannot be identified, a binding-time error occurs.</span></span>
    * <span data-ttu-id="dec56-1233">Výsledkem *object_creation_expression* je hodnota typu `T`, konkrétně hodnota vytvořen zavoláním konstruktoru instance nadefinovali v předchozím kroku.</span><span class="sxs-lookup"><span data-stu-id="dec56-1233">The result of the *object_creation_expression* is a value of type `T`, namely the value produced by invoking the instance constructor determined in the step above.</span></span>
*  <span data-ttu-id="dec56-1234">V opačném případě *object_creation_expression* není platný, a dojde k chybě vazby čas.</span><span class="sxs-lookup"><span data-stu-id="dec56-1234">Otherwise, the *object_creation_expression* is invalid, and a binding-time error occurs.</span></span>

<span data-ttu-id="dec56-1235">I v případě, *object_creation_expression* je dynamicky vázán, typ kompilace je stále `T`.</span><span class="sxs-lookup"><span data-stu-id="dec56-1235">Even if the *object_creation_expression* is dynamically bound, the compile-time type is still `T`.</span></span>

<span data-ttu-id="dec56-1236">Zpracování za běhu *object_creation_expression* formuláře `new T(A)`, kde `T` je *class_type* nebo *struct_type* a `A` je volitelný *argument_list*, se skládá z následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="dec56-1236">The run-time processing of an *object_creation_expression* of the form `new T(A)`, where `T` is *class_type* or a *struct_type* and `A` is an optional *argument_list*, consists of the following steps:</span></span>

*   <span data-ttu-id="dec56-1237">Pokud `T` je *class_type*:</span><span class="sxs-lookup"><span data-stu-id="dec56-1237">If `T` is a *class_type*:</span></span>
    * <span data-ttu-id="dec56-1238">Vytvoření nové instance třídy `T` je přidělen.</span><span class="sxs-lookup"><span data-stu-id="dec56-1238">A new instance of class `T` is allocated.</span></span> <span data-ttu-id="dec56-1239">Pokud není k dispozici dostatek paměti k přidělení nové instance `System.OutOfMemoryException` je vyvolána, a že jsou provedeny žádné další kroky.</span><span class="sxs-lookup"><span data-stu-id="dec56-1239">If there is not enough memory available to allocate the new instance, a `System.OutOfMemoryException` is thrown and no further steps are executed.</span></span>
    * <span data-ttu-id="dec56-1240">Všechna pole nové instance jsou inicializovány na výchozí hodnoty ([výchozí hodnoty](variables.md#default-values)).</span><span class="sxs-lookup"><span data-stu-id="dec56-1240">All fields of the new instance are initialized to their default values ([Default values](variables.md#default-values)).</span></span>
    * <span data-ttu-id="dec56-1241">Podle pravidel objektů volání členské funkce je volána konstruktor instance ([kompilace kontrolu dynamické přetížení](expressions.md#compile-time-checking-of-dynamic-overload-resolution)).</span><span class="sxs-lookup"><span data-stu-id="dec56-1241">The instance constructor is invoked according to the rules of function member invocation ([Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution)).</span></span> <span data-ttu-id="dec56-1242">Odkaz na nově přidělenou instanci automaticky předána do konstruktoru instance a instance je přístupný z v rámci tohoto konstruktoru jako `this`.</span><span class="sxs-lookup"><span data-stu-id="dec56-1242">A reference to the newly allocated instance is automatically passed to the instance constructor and the instance can be accessed from within that constructor as `this`.</span></span>
*   <span data-ttu-id="dec56-1243">Pokud `T` je *struct_type*:</span><span class="sxs-lookup"><span data-stu-id="dec56-1243">If `T` is a *struct_type*:</span></span>
    * <span data-ttu-id="dec56-1244">Instance typu `T` vytvoří dočasnou proměnnou místního přidělení.</span><span class="sxs-lookup"><span data-stu-id="dec56-1244">An instance of type `T` is created by allocating a temporary local variable.</span></span> <span data-ttu-id="dec56-1245">Od verze konstruktoru instance *struct_type* je potřeba jednoznačně přiřadit hodnotu každému poli instance vytváří, není inicializace dočasné proměnné, je nezbytné.</span><span class="sxs-lookup"><span data-stu-id="dec56-1245">Since an instance constructor of a *struct_type* is required to definitely assign a value to each field of the instance being created, no initialization of the temporary variable is necessary.</span></span>
    * <span data-ttu-id="dec56-1246">Podle pravidel objektů volání členské funkce je volána konstruktor instance ([kompilace kontrolu dynamické přetížení](expressions.md#compile-time-checking-of-dynamic-overload-resolution)).</span><span class="sxs-lookup"><span data-stu-id="dec56-1246">The instance constructor is invoked according to the rules of function member invocation ([Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution)).</span></span> <span data-ttu-id="dec56-1247">Odkaz na nově přidělenou instanci automaticky předána do konstruktoru instance a instance je přístupný z v rámci tohoto konstruktoru jako `this`.</span><span class="sxs-lookup"><span data-stu-id="dec56-1247">A reference to the newly allocated instance is automatically passed to the instance constructor and the instance can be accessed from within that constructor as `this`.</span></span>

#### <a name="object-initializers"></a><span data-ttu-id="dec56-1248">Inicializátory objektů</span><span class="sxs-lookup"><span data-stu-id="dec56-1248">Object initializers</span></span>

<span data-ttu-id="dec56-1249">***Objektu inicializátoru*** určuje hodnoty pro nula nebo více polí, vlastnosti nebo indexované prvků objektu.</span><span class="sxs-lookup"><span data-stu-id="dec56-1249">An ***object initializer*** specifies values for zero or more fields, properties or indexed elements of an object.</span></span>

```antlr
object_initializer
    : '{' member_initializer_list? '}'
    | '{' member_initializer_list ',' '}'
    ;

member_initializer_list
    : member_initializer (',' member_initializer)*
    ;

member_initializer
    : initializer_target '=' initializer_value
    ;

initializer_target
    : identifier
    | '[' argument_list ']'
    ;

initializer_value
    : expression
    | object_or_collection_initializer
    ;
```

<span data-ttu-id="dec56-1250">Inicializátor objektu se skládá z posloupnost inicializátory členů, ohraničená `{` a `}` tokeny a oddělené čárkami.</span><span class="sxs-lookup"><span data-stu-id="dec56-1250">An object initializer consists of a sequence of member initializers, enclosed by `{` and `}` tokens and separated by commas.</span></span> <span data-ttu-id="dec56-1251">Každý *member_initializer* určuje cíle pro inicializaci.</span><span class="sxs-lookup"><span data-stu-id="dec56-1251">Each *member_initializer* designates a target for the initialization.</span></span> <span data-ttu-id="dec56-1252">*Identifikátor* nutné pojmenovat přístupné pole nebo vlastnost objektu inicializována, že *argument_list* ohraničeno hranaté závorky, musíte zadat argumenty pro indexer přístupné na objekt, který je inicializován.</span><span class="sxs-lookup"><span data-stu-id="dec56-1252">An *identifier* must name an accessible field or property of the object being initialized, whereas an *argument_list* enclosed in square brackets must specify arguments for an accessible indexer on the object being initialized.</span></span> <span data-ttu-id="dec56-1253">Jedná se o chybu pro inicializátoru objektu, který chcete zahrnout více než jeden inicializátor členu pro stejné pole nebo vlastnost.</span><span class="sxs-lookup"><span data-stu-id="dec56-1253">It is an error for an object initializer to include more than one member initializer for the same field or property.</span></span>

<span data-ttu-id="dec56-1254">Každý *initializer_target* následuje znak rovná se a výrazu, inicializátoru objektu nebo inicializátor kolekce.</span><span class="sxs-lookup"><span data-stu-id="dec56-1254">Each *initializer_target* is followed by an equals sign and either an expression, an object initializer or a collection initializer.</span></span> <span data-ttu-id="dec56-1255">Není možné pro výrazy v inicializátoru objektu, který neodkazuje na nově vytvořený objekt, který je inicializace.</span><span class="sxs-lookup"><span data-stu-id="dec56-1255">It is not possible for expressions within the object initializer to refer to the newly created object it is initializing.</span></span>

<span data-ttu-id="dec56-1256">Inicializátor členu, který určuje výraz, až se zpracují znaménko rovná se stejným způsobem jako přiřazení ([jednoduché přiřazení](expressions.md#simple-assignment)) k cíli.</span><span class="sxs-lookup"><span data-stu-id="dec56-1256">A member initializer that specifies an expression after the equals sign is processed in the same way as an assignment ([Simple assignment](expressions.md#simple-assignment)) to the target.</span></span>

<span data-ttu-id="dec56-1257">Inicializátor členu, který určuje inicializátoru objektu po znaménko rovná se ***inicializátor vnořeného objektu***, to znamená inicializace vloženého objektu.</span><span class="sxs-lookup"><span data-stu-id="dec56-1257">A member initializer that specifies an object initializer after the equals sign is a ***nested object initializer***, i.e. an initialization of an embedded object.</span></span> <span data-ttu-id="dec56-1258">Namísto přiřazení nové hodnoty pro pole nebo vlastnost, jsou považovány přiřazení v inicializátoru vnořený objekt za přiřazení pro členy pole nebo vlastnost.</span><span class="sxs-lookup"><span data-stu-id="dec56-1258">Instead of assigning a new value to the field or property, the assignments in the nested object initializer are treated as assignments to members of the field or property.</span></span> <span data-ttu-id="dec56-1259">Inicializátory objektů vnořené nejde použít s typem hodnoty vlastnosti nebo pole jen pro čtení s typem hodnoty.</span><span class="sxs-lookup"><span data-stu-id="dec56-1259">Nested object initializers cannot be applied to properties with a value type, or to read-only fields with a value type.</span></span>

<span data-ttu-id="dec56-1260">Inicializátor členu, který určuje inicializátoru kolekce po inicializaci vložené kolekce znaménko rovná se.</span><span class="sxs-lookup"><span data-stu-id="dec56-1260">A member initializer that specifies a collection initializer after the equals sign is an initialization of an embedded collection.</span></span> <span data-ttu-id="dec56-1261">Namísto přiřazení novou kolekci do cílového pole, vlastnost nebo indexer, jsou prvky uvedené v inicializátoru přidat do kolekce odkazuje cíl.</span><span class="sxs-lookup"><span data-stu-id="dec56-1261">Instead of assigning a new collection to the target field, property or indexer, the elements given in the initializer are added to the collection referenced by the target.</span></span> <span data-ttu-id="dec56-1262">Cíl musí být typu kolekce, který splňuje požadavky uvedené v [inicializátory kolekce](expressions.md#collection-initializers).</span><span class="sxs-lookup"><span data-stu-id="dec56-1262">The target must be of a collection type that satisfies the requirements specified in [Collection initializers](expressions.md#collection-initializers).</span></span>

<span data-ttu-id="dec56-1263">Argumenty, které mají index inicializátor vždy se vyhodnotí pouze jednou.</span><span class="sxs-lookup"><span data-stu-id="dec56-1263">The arguments to an index initializer will always be evaluated exactly once.</span></span> <span data-ttu-id="dec56-1264">Díky tomu se i v případě, že argumenty skončit, získávání nikdy použit (např. z důvodu prázdný inicializátor vnořeného), bude se vyhodnocují hlediska jejich vedlejších účinků.</span><span class="sxs-lookup"><span data-stu-id="dec56-1264">Thus, even if the arguments end up never getting used (e.g. because of an empty nested initializer), they will be evaluated for their side effects.</span></span>

<span data-ttu-id="dec56-1265">Následující třída reprezentuje bod se souřadnicemi dvě:</span><span class="sxs-lookup"><span data-stu-id="dec56-1265">The following class represents a point with two coordinates:</span></span>
```csharp
public class Point
{
    int x, y;

    public int X { get { return x; } set { x = value; } }
    public int Y { get { return y; } set { y = value; } }
}
```

<span data-ttu-id="dec56-1266">Instance `Point` může být vytvořeno a inicializováno následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="dec56-1266">An instance of `Point` can be created and initialized as follows:</span></span>
```csharp
Point a = new Point { X = 0, Y = 1 };
```
<span data-ttu-id="dec56-1267">který má stejný účinek jako</span><span class="sxs-lookup"><span data-stu-id="dec56-1267">which has the same effect as</span></span>
```csharp
Point __a = new Point();
__a.X = 0;
__a.Y = 1; 
Point a = __a;
```
<span data-ttu-id="dec56-1268">kde `__a` je dočasná proměnná jinak neviditelné a nejsou přístupné.</span><span class="sxs-lookup"><span data-stu-id="dec56-1268">where `__a` is an otherwise invisible and inaccessible temporary variable.</span></span> <span data-ttu-id="dec56-1269">Následující třída reprezentuje obdélník vytvořené z dva body:</span><span class="sxs-lookup"><span data-stu-id="dec56-1269">The following class represents a rectangle created from two points:</span></span>
```csharp
public class Rectangle
{
    Point p1, p2;

    public Point P1 { get { return p1; } set { p1 = value; } }
    public Point P2 { get { return p2; } set { p2 = value; } }
}
```

<span data-ttu-id="dec56-1270">Instance `Rectangle` může být vytvořeno a inicializováno následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="dec56-1270">An instance of `Rectangle` can be created and initialized as follows:</span></span>
```csharp
Rectangle r = new Rectangle {
    P1 = new Point { X = 0, Y = 1 },
    P2 = new Point { X = 2, Y = 3 }
};
```
<span data-ttu-id="dec56-1271">který má stejný účinek jako</span><span class="sxs-lookup"><span data-stu-id="dec56-1271">which has the same effect as</span></span>
```csharp
Rectangle __r = new Rectangle();
Point __p1 = new Point();
__p1.X = 0;
__p1.Y = 1;
__r.P1 = __p1;
Point __p2 = new Point();
__p2.X = 2;
__p2.Y = 3;
__r.P2 = __p2; 
Rectangle r = __r;
```
<span data-ttu-id="dec56-1272">kde `__r`, `__p1` a `__p2` jsou dočasné proměnné, které jsou jinak neviditelné a nejsou přístupné.</span><span class="sxs-lookup"><span data-stu-id="dec56-1272">where `__r`, `__p1` and `__p2` are temporary variables that are otherwise invisible and inaccessible.</span></span>

<span data-ttu-id="dec56-1273">Pokud `Rectangle`pro konstruktor přiděluje dvě vložené `Point` instancí</span><span class="sxs-lookup"><span data-stu-id="dec56-1273">If `Rectangle`'s constructor allocates the two embedded `Point` instances</span></span>
```csharp
public class Rectangle
{
    Point p1 = new Point();
    Point p2 = new Point();

    public Point P1 { get { return p1; } }
    public Point P2 { get { return p2; } }
}
```
<span data-ttu-id="dec56-1274">Následující konstruktor umožňuje inicializovat vložený `Point` instance namísto přiřazení nových instancí:</span><span class="sxs-lookup"><span data-stu-id="dec56-1274">the following construct can be used to initialize the embedded `Point` instances instead of assigning new instances:</span></span>
```csharp
Rectangle r = new Rectangle {
    P1 = { X = 0, Y = 1 },
    P2 = { X = 2, Y = 3 }
};
```
<span data-ttu-id="dec56-1275">který má stejný účinek jako</span><span class="sxs-lookup"><span data-stu-id="dec56-1275">which has the same effect as</span></span>
```csharp
Rectangle __r = new Rectangle();
__r.P1.X = 0;
__r.P1.Y = 1;
__r.P2.X = 2;
__r.P2.Y = 3;
Rectangle r = __r;
```

<span data-ttu-id="dec56-1276">Zadaný odpovídající definici jazyka C, v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="dec56-1276">Given an appropriate definition of C, the following example:</span></span>
```csharp
var c = new C {
    x = true,
    y = { a = "Hello" },
    z = { 1, 2, 3 },
    ["x"] = 5,
    [0,0] = { "a", "b" },
    [1,2] = {}
};
```
<span data-ttu-id="dec56-1277">je ekvivalentní k této sérii přiřazení:</span><span class="sxs-lookup"><span data-stu-id="dec56-1277">is equivalent to this series of assignments:</span></span>
```csharp
C __c = new C();
__c.x = true;
__c.y.a = "Hello";
__c.z.Add(1); 
__c.z.Add(2);
__c.z.Add(3);
string __i1 = "x";
__c[__i1] = 5;
int __i2 = 0, __i3 = 0;
__c[__i2,__i3].Add("a");
__c[__i2,__i3].Add("b");
int __i4 = 1, __i5 = 2;
var c = __c;
```
<span data-ttu-id="dec56-1278">kde `__c`atd jsou generované proměnné, které jsou skryté a nepřístupný pro zdrojový kód.</span><span class="sxs-lookup"><span data-stu-id="dec56-1278">where `__c`, etc., are generated variables that are invisible and inaccessible to the source code.</span></span> <span data-ttu-id="dec56-1279">Všimněte si, že argumenty `[0,0]` je Vyhodnocená jenom jednou a argumenty `[1,2]` jsou vyhodnoceny jednou, i když se nikdy používají.</span><span class="sxs-lookup"><span data-stu-id="dec56-1279">Note that the arguments for `[0,0]` are evaluated only once, and the arguments for `[1,2]` are evaluated once even though they are never used.</span></span>

#### <a name="collection-initializers"></a><span data-ttu-id="dec56-1280">Inicializátory kolekce</span><span class="sxs-lookup"><span data-stu-id="dec56-1280">Collection initializers</span></span>

<span data-ttu-id="dec56-1281">Inicializátor kolekce určuje prvků kolekce.</span><span class="sxs-lookup"><span data-stu-id="dec56-1281">A collection initializer specifies the elements of a collection.</span></span>

```antlr
collection_initializer
    : '{' element_initializer_list '}'
    | '{' element_initializer_list ',' '}'
    ;

element_initializer_list
    : element_initializer (',' element_initializer)*
    ;

element_initializer
    : non_assignment_expression
    | '{' expression_list '}'
    ;

expression_list
    : expression (',' expression)*
    ;
```

<span data-ttu-id="dec56-1282">Inicializátor kolekce se skládá z posloupnost inicializátorů prvku, ohraničená `{` a `}` tokeny a oddělené čárkami.</span><span class="sxs-lookup"><span data-stu-id="dec56-1282">A collection initializer consists of a sequence of element initializers, enclosed by `{` and `}` tokens and separated by commas.</span></span> <span data-ttu-id="dec56-1283">Každý prvek inicializátor určuje element, který má být přidána do objektu kolekce, který je inicializován a obsahuje seznam výrazů uzavřená v `{` a `}` tokeny a oddělené čárkami.</span><span class="sxs-lookup"><span data-stu-id="dec56-1283">Each element initializer specifies an element to be added to the collection object being initialized, and consists of a list of expressions enclosed by `{` and `}` tokens and separated by commas.</span></span>  <span data-ttu-id="dec56-1284">Inicializátor prvku jedním výrazem lze napsat bez závorek, ale pak nemůže být výraz přiřazení, aby se zabránilo nejednoznačnosti s inicializátory členů.</span><span class="sxs-lookup"><span data-stu-id="dec56-1284">A single-expression element initializer can be written without braces, but cannot then be an assignment expression, to avoid ambiguity with member initializers.</span></span> <span data-ttu-id="dec56-1285">*Non_assignment_expression* produkčním prostředí je definována v [výraz](expressions.md#expression).</span><span class="sxs-lookup"><span data-stu-id="dec56-1285">The *non_assignment_expression* production is defined in [Expression](expressions.md#expression).</span></span>

<span data-ttu-id="dec56-1286">Následuje příklad výrazu vytvoření objektu, která obsahuje inicializátor kolekce:</span><span class="sxs-lookup"><span data-stu-id="dec56-1286">The following is an example of an object creation expression that includes a collection initializer:</span></span>
```csharp
List<int> digits = new List<int> { 0, 1, 2, 3, 4, 5, 6, 7, 8, 9 };
```

<span data-ttu-id="dec56-1287">Objekt kolekce, do které se použije inicializátoru kolekce musí být typu, který implementuje `System.Collections.IEnumerable` nebo dojde k chybě kompilace.</span><span class="sxs-lookup"><span data-stu-id="dec56-1287">The collection object to which a collection initializer is applied must be of a type that implements `System.Collections.IEnumerable` or a compile-time error occurs.</span></span> <span data-ttu-id="dec56-1288">Pro každý zadaný element v pořadí, vyvolá inicializátor kolekce `Add` metodu na cílovém objektu se seznamem výrazu inicializátor prvku jako seznam argumentů, použití členu vyhledávání a řešení pro každé vyvolání přetížení.</span><span class="sxs-lookup"><span data-stu-id="dec56-1288">For each specified element in order, the collection initializer invokes an `Add` method on the target object with the expression list of the element initializer as argument list, applying normal member lookup and overload resolution for each invocation.</span></span> <span data-ttu-id="dec56-1289">Proto objekt kolekce musí mít použitelnou metodu instance nebo rozšíření s názvem `Add` pro každý prvek inicializátoru.</span><span class="sxs-lookup"><span data-stu-id="dec56-1289">Thus, the collection object must have an applicable instance or extension method with the name `Add` for each element initializer.</span></span>

<span data-ttu-id="dec56-1290">Následující třída reprezentuje kontakt s názvem a seznam telefonních čísel:</span><span class="sxs-lookup"><span data-stu-id="dec56-1290">The following class represents a contact with a name and a list of phone numbers:</span></span>
```csharp
public class Contact
{
    string name;
    List<string> phoneNumbers = new List<string>();

    public string Name { get { return name; } set { name = value; } }

    public List<string> PhoneNumbers { get { return phoneNumbers; } }
}
```

<span data-ttu-id="dec56-1291">A `List<Contact>` může být vytvořeno a inicializováno následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="dec56-1291">A `List<Contact>` can be created and initialized as follows:</span></span>
```csharp
var contacts = new List<Contact> {
    new Contact {
        Name = "Chris Smith",
        PhoneNumbers = { "206-555-0101", "425-882-8080" }
    },
    new Contact {
        Name = "Bob Harris",
        PhoneNumbers = { "650-555-0199" }
    }
};
```
<span data-ttu-id="dec56-1292">který má stejný účinek jako</span><span class="sxs-lookup"><span data-stu-id="dec56-1292">which has the same effect as</span></span>
```csharp
var __clist = new List<Contact>();
Contact __c1 = new Contact();
__c1.Name = "Chris Smith";
__c1.PhoneNumbers.Add("206-555-0101");
__c1.PhoneNumbers.Add("425-882-8080");
__clist.Add(__c1);
Contact __c2 = new Contact();
__c2.Name = "Bob Harris";
__c2.PhoneNumbers.Add("650-555-0199");
__clist.Add(__c2);
var contacts = __clist;
```
<span data-ttu-id="dec56-1293">kde `__clist`, `__c1` a `__c2` jsou dočasné proměnné, které jsou jinak neviditelné a nejsou přístupné.</span><span class="sxs-lookup"><span data-stu-id="dec56-1293">where `__clist`, `__c1` and `__c2` are temporary variables that are otherwise invisible and inaccessible.</span></span>

#### <a name="array-creation-expressions"></a><span data-ttu-id="dec56-1294">Výrazy vytvoření pole</span><span class="sxs-lookup"><span data-stu-id="dec56-1294">Array creation expressions</span></span>

<span data-ttu-id="dec56-1295">*Array_creation_expression* slouží k vytvoření nové instance *array_type*.</span><span class="sxs-lookup"><span data-stu-id="dec56-1295">An *array_creation_expression* is used to create a new instance of an *array_type*.</span></span>

```antlr
array_creation_expression
    : 'new' non_array_type '[' expression_list ']' rank_specifier* array_initializer?
    | 'new' array_type array_initializer
    | 'new' rank_specifier array_initializer
    ;
```

<span data-ttu-id="dec56-1296">Výraz vytvoření pole ve formuláři první přiděluje pole instance typu, který je výsledkem odstraňuje všechny jednotlivé výrazy v seznamu výrazů.</span><span class="sxs-lookup"><span data-stu-id="dec56-1296">An array creation expression of the first form allocates an array instance of the type that results from deleting each of the individual expressions from the expression list.</span></span> <span data-ttu-id="dec56-1297">Například výraz vytvoření pole `new int[10,20]` vytvoří pole instance typu `int[,]`a výraz vytvoření pole `new int[10][,]` vytvoří pole typu `int[][,]`.</span><span class="sxs-lookup"><span data-stu-id="dec56-1297">For example, the array creation expression `new int[10,20]` produces an array instance of type `int[,]`, and the array creation expression `new int[10][,]` produces an array of type `int[][,]`.</span></span> <span data-ttu-id="dec56-1298">Každý výraz v seznamu výrazů musí být typu `int`, `uint`, `long`, nebo `ulong`, nebo implicitně převést na jeden nebo více z těchto typů.</span><span class="sxs-lookup"><span data-stu-id="dec56-1298">Each expression in the expression list must be of type `int`, `uint`, `long`, or `ulong`, or implicitly convertible to one or more of these types.</span></span> <span data-ttu-id="dec56-1299">Hodnota každý výraz určuje délku odpovídající dimenze v nově přidělenou pole instance.</span><span class="sxs-lookup"><span data-stu-id="dec56-1299">The value of each expression determines the length of the corresponding dimension in the newly allocated array instance.</span></span> <span data-ttu-id="dec56-1300">Protože rozměr pole musí být nezáporné, je chyba kompilace mít *constant_expression* se zápornou hodnotou v seznamu výrazů.</span><span class="sxs-lookup"><span data-stu-id="dec56-1300">Since the length of an array dimension must be nonnegative, it is a compile-time error to have a *constant_expression* with a negative value in the expression list.</span></span>

<span data-ttu-id="dec56-1301">S výjimkou v kontextu unsafe ([nezabezpečený kontext](unsafe-code.md#unsafe-contexts)), rozložení polí není zadána.</span><span class="sxs-lookup"><span data-stu-id="dec56-1301">Except in an unsafe context ([Unsafe contexts](unsafe-code.md#unsafe-contexts)), the layout of arrays is unspecified.</span></span>

<span data-ttu-id="dec56-1302">Pokud výraz vytvoření pole ve formuláři první obsahuje inicializátor pole, každý výraz v seznamu výrazů musí být konstanta a počet rozměrů a dimenze délky určený seznam výrazů musí odpovídat názvům inicializátor pole.</span><span class="sxs-lookup"><span data-stu-id="dec56-1302">If an array creation expression of the first form includes an array initializer, each expression in the expression list must be a constant and the rank and dimension lengths specified by the expression list must match those of the array initializer.</span></span>

<span data-ttu-id="dec56-1303">Ve výraz vytvoření pole formuláře druhý nebo třetí řád specifikátor typu nebo řazení zadané pole musí odpovídat inicializátor pole.</span><span class="sxs-lookup"><span data-stu-id="dec56-1303">In an array creation expression of the second or third form, the rank of the specified array type or rank specifier must match that of the array initializer.</span></span> <span data-ttu-id="dec56-1304">Délky jednotlivých dimenze jsou odvozeny z počtu prvků v každém z odpovídajících úrovní vnoření inicializátoru pole.</span><span class="sxs-lookup"><span data-stu-id="dec56-1304">The individual dimension lengths are inferred from the number of elements in each of the corresponding nesting levels of the array initializer.</span></span> <span data-ttu-id="dec56-1305">Proto výraz</span><span class="sxs-lookup"><span data-stu-id="dec56-1305">Thus, the expression</span></span>
```csharp
new int[,] {{0, 1}, {2, 3}, {4, 5}}
```
<span data-ttu-id="dec56-1306">přesně odpovídá</span><span class="sxs-lookup"><span data-stu-id="dec56-1306">exactly corresponds to</span></span>
```csharp
new int[3, 2] {{0, 1}, {2, 3}, {4, 5}}
```

<span data-ttu-id="dec56-1307">Výraz vytvoření pole ve formuláři třetí se označuje jako ***implicitně typované výraz vytvoření pole***.</span><span class="sxs-lookup"><span data-stu-id="dec56-1307">An array creation expression of the third form is referred to as an ***implicitly typed array creation expression***.</span></span> <span data-ttu-id="dec56-1308">S tím rozdílem, že typ elementu pole není uvedena explicitní, ale určena jako nejlépe společný typ je podobné pro druhý formulář ([hledání nejlepší společný typ sada výrazů](expressions.md#finding-the-best-common-type-of-a-set-of-expressions)) sady výrazů v poli inicializátor.</span><span class="sxs-lookup"><span data-stu-id="dec56-1308">It is similar to the second form, except that the element type of the array is not explicitly given, but determined as the best common type ([Finding the best common type of a set of expressions](expressions.md#finding-the-best-common-type-of-a-set-of-expressions)) of the set of expressions in the array initializer.</span></span> <span data-ttu-id="dec56-1309">Pro multidimenzionální pole například jednoho, kde *rank_specifier* obsahuje nejméně jednu čárku, tato sada zahrnuje všechny *výraz*s součástí vnořené *array_initializer*s.</span><span class="sxs-lookup"><span data-stu-id="dec56-1309">For a multidimensional array, i.e., one where the *rank_specifier* contains at least one comma, this set comprises all *expression*s found in nested *array_initializer*s.</span></span>

<span data-ttu-id="dec56-1310">Inicializátory polí jsou popsány dále v [Inicializátory pole](arrays.md#array-initializers).</span><span class="sxs-lookup"><span data-stu-id="dec56-1310">Array initializers are described further in [Array initializers](arrays.md#array-initializers).</span></span>

<span data-ttu-id="dec56-1311">Výsledek vyhodnocení výrazu výraz vytvoření pole je klasifikován jako hodnotu, konkrétně odkaz na nově přidělenou pole instance.</span><span class="sxs-lookup"><span data-stu-id="dec56-1311">The result of evaluating an array creation expression is classified as a value, namely a reference to the newly allocated array instance.</span></span> <span data-ttu-id="dec56-1312">Zpracování za běhu výraz vytvoření pole se skládá z následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="dec56-1312">The run-time processing of an array creation expression consists of the following steps:</span></span>

*  <span data-ttu-id="dec56-1313">Výrazy délka dimenze *expression_list* se vyhodnocují v pořadí zleva doprava.</span><span class="sxs-lookup"><span data-stu-id="dec56-1313">The dimension length expressions of the *expression_list* are evaluated in order, from left to right.</span></span> <span data-ttu-id="dec56-1314">Vyhodnocení každý výraz implicitní převod ([implicitních převodů](conversions.md#implicit-conversions)) se provádí na jednu z následujících typů: `int`, `uint`, `long`, `ulong`.</span><span class="sxs-lookup"><span data-stu-id="dec56-1314">Following evaluation of each expression, an implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)) to one of the following types is performed: `int`, `uint`, `long`, `ulong`.</span></span> <span data-ttu-id="dec56-1315">První typ v tomto seznamu, pro kterou existuje implicitní převod je vybrán.</span><span class="sxs-lookup"><span data-stu-id="dec56-1315">The first type in this list for which an implicit conversion exists is chosen.</span></span> <span data-ttu-id="dec56-1316">Pokud limit vyhodnocení výrazu nebo následné implicitní převod způsobí výjimku, žádné další výrazy jsou vyhodnoceny a jsou provedeny žádné další kroky.</span><span class="sxs-lookup"><span data-stu-id="dec56-1316">If evaluation of an expression or the subsequent implicit conversion causes an exception, then no further expressions are evaluated and no further steps are executed.</span></span>
*  <span data-ttu-id="dec56-1317">Vypočítané hodnoty pro délky dimenzí se ověří následujícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="dec56-1317">The computed values for the dimension lengths are validated as follows.</span></span> <span data-ttu-id="dec56-1318">Pokud jeden nebo více hodnot je menší než nula, `System.OverflowException` je vyvolána, a že jsou provedeny žádné další kroky.</span><span class="sxs-lookup"><span data-stu-id="dec56-1318">If one or more of the values are less than zero, a `System.OverflowException` is thrown and no further steps are executed.</span></span>
*  <span data-ttu-id="dec56-1319">Instance pole s délkou dané dimenze je přidělen.</span><span class="sxs-lookup"><span data-stu-id="dec56-1319">An array instance with the given dimension lengths is allocated.</span></span> <span data-ttu-id="dec56-1320">Pokud není k dispozici dostatek paměti k přidělení nové instance `System.OutOfMemoryException` je vyvolána, a že jsou provedeny žádné další kroky.</span><span class="sxs-lookup"><span data-stu-id="dec56-1320">If there is not enough memory available to allocate the new instance, a `System.OutOfMemoryException` is thrown and no further steps are executed.</span></span>
*  <span data-ttu-id="dec56-1321">Všechny prvky nové instance pole jsou inicializovány na výchozí hodnoty ([výchozí hodnoty](variables.md#default-values)).</span><span class="sxs-lookup"><span data-stu-id="dec56-1321">All elements of the new array instance are initialized to their default values ([Default values](variables.md#default-values)).</span></span>
*  <span data-ttu-id="dec56-1322">Pokud výraz vytvoření pole obsahuje inicializátor pole, každý výraz v inicializátoru pole je vyhodnocen a přiřazen do svého odpovídajícího prvku pole.</span><span class="sxs-lookup"><span data-stu-id="dec56-1322">If the array creation expression contains an array initializer, then each expression in the array initializer is evaluated and assigned to its corresponding array element.</span></span> <span data-ttu-id="dec56-1323">Hodnocení a přiřazení se provádějí v pořadí výrazy jsou napsané v inicializátoru pole – jinými slovy, prvky jsou inicializovány ve vzestupném pořadí index s zvýšení první rozměr nejvíce vpravo.</span><span class="sxs-lookup"><span data-stu-id="dec56-1323">The evaluations and assignments are performed in the order the expressions are written in the array initializer—in other words, elements are initialized in increasing index order, with the rightmost dimension increasing first.</span></span> <span data-ttu-id="dec56-1324">Pokud zkušební daného výrazu nebo následné přiřazení odpovídající prvek pole způsobí výjimku, žádné další prvky jsou inicializovány (a zbývající prvky budou mít výchozí hodnoty).</span><span class="sxs-lookup"><span data-stu-id="dec56-1324">If evaluation of a given expression or the subsequent assignment to the corresponding array element causes an exception, then no further elements are initialized (and the remaining elements will thus have their default values).</span></span>

<span data-ttu-id="dec56-1325">Výraz vytvoření pole umožňuje vytvoření instance pole s prvky typu pole, ale prvky takové pole musí být inicializován ručně.</span><span class="sxs-lookup"><span data-stu-id="dec56-1325">An array creation expression permits instantiation of an array with elements of an array type, but the elements of such an array must be manually initialized.</span></span> <span data-ttu-id="dec56-1326">Například příkaz</span><span class="sxs-lookup"><span data-stu-id="dec56-1326">For example, the statement</span></span>
```csharp
int[][] a = new int[100][];
```
<span data-ttu-id="dec56-1327">Vytvoří jednorozměrné pole 100 elementy typu `int[]`.</span><span class="sxs-lookup"><span data-stu-id="dec56-1327">creates a single-dimensional array with 100 elements of type `int[]`.</span></span> <span data-ttu-id="dec56-1328">Počáteční hodnota každý element je `null`.</span><span class="sxs-lookup"><span data-stu-id="dec56-1328">The initial value of each element is `null`.</span></span> <span data-ttu-id="dec56-1329">Není možné pro stejný výraz vytvoření pole se také vytvořit instanci dílčí pole a příkaz</span><span class="sxs-lookup"><span data-stu-id="dec56-1329">It is not possible for the same array creation expression to also instantiate the sub-arrays, and the statement</span></span>
```csharp
int[][] a = new int[100][5];        // Error
```
<span data-ttu-id="dec56-1330">výsledkem chyba kompilace.</span><span class="sxs-lookup"><span data-stu-id="dec56-1330">results in a compile-time error.</span></span> <span data-ttu-id="dec56-1331">Vytvoření instance dílčí pole je nutné místo toho provést ručně, jako v</span><span class="sxs-lookup"><span data-stu-id="dec56-1331">Instantiation of the sub-arrays must instead be performed manually, as in</span></span>
```csharp
int[][] a = new int[100][];
for (int i = 0; i < 100; i++) a[i] = new int[5];
```

<span data-ttu-id="dec56-1332">Pokud má pole polí "obdélníkový" tvar, který je dílčí pole je všechny stejnou délku, je výhodnější používat vícerozměrné pole.</span><span class="sxs-lookup"><span data-stu-id="dec56-1332">When an array of arrays has a "rectangular" shape, that is when the sub-arrays are all of the same length, it is more efficient to use a multi-dimensional array.</span></span> <span data-ttu-id="dec56-1333">V předchozím příkladu vytvoření instance pole polí vytvoří objekty 101 – jedno pole vnější a 100 dílčí pole.</span><span class="sxs-lookup"><span data-stu-id="dec56-1333">In the example above, instantiation of the array of arrays creates 101 objects—one outer array and 100 sub-arrays.</span></span> <span data-ttu-id="dec56-1334">Naproti tomu</span><span class="sxs-lookup"><span data-stu-id="dec56-1334">In contrast,</span></span>
```csharp
int[,] = new int[100, 5];
```
<span data-ttu-id="dec56-1335">vytvoří pouze jeden objekt, dvourozměrné pole a provede přidělení v jediném příkazu.</span><span class="sxs-lookup"><span data-stu-id="dec56-1335">creates only a single object, a two-dimensional array, and accomplishes the allocation in a single statement.</span></span>

<span data-ttu-id="dec56-1336">Následují příklady vytváření výrazů implicitně typované pole:</span><span class="sxs-lookup"><span data-stu-id="dec56-1336">The following are examples of implicitly typed array creation expressions:</span></span>
```csharp
var a = new[] { 1, 10, 100, 1000 };                       // int[]

var b = new[] { 1, 1.5, 2, 2.5 };                         // double[]

var c = new[,] { { "hello", null }, { "world", "!" } };   // string[,]

var d = new[] { 1, "one", 2, "two" };                     // Error
```

<span data-ttu-id="dec56-1337">Posledního výrazu způsobí chybu kompilace, protože ani `int` ani `string` implicitně převést na druhé a tady je již nejlépe běžné zadejte.</span><span class="sxs-lookup"><span data-stu-id="dec56-1337">The last expression causes a compile-time error because neither `int` nor `string` is implicitly convertible to the other, and so there is no best common type.</span></span> <span data-ttu-id="dec56-1338">Výraz vytvoření pole explicitně musí použít v tomto případě třeba určující typ, který má být `object[]`.</span><span class="sxs-lookup"><span data-stu-id="dec56-1338">An explicitly typed array creation expression must be used in this case, for example specifying the type to be `object[]`.</span></span> <span data-ttu-id="dec56-1339">Můžete také jeden z prvků lze převést na běžné základní typ, který by pak můžou stát typ odvozený element.</span><span class="sxs-lookup"><span data-stu-id="dec56-1339">Alternatively, one of the elements can be cast to a common base type, which would then become the inferred element type.</span></span>

<span data-ttu-id="dec56-1340">Implicitně typovaná pole vytváření výrazů je možné kombinovat s inicializátory anonymních objektů ([anonymní objekt vytváření výrazů](expressions.md#anonymous-object-creation-expressions)) vytvořte anonymně zadané datové struktury.</span><span class="sxs-lookup"><span data-stu-id="dec56-1340">Implicitly typed array creation expressions can be combined with anonymous object initializers ([Anonymous object creation expressions](expressions.md#anonymous-object-creation-expressions)) to create anonymously typed data structures.</span></span> <span data-ttu-id="dec56-1341">Příklad:</span><span class="sxs-lookup"><span data-stu-id="dec56-1341">For example:</span></span>
```csharp
var contacts = new[] {
    new {
        Name = "Chris Smith",
        PhoneNumbers = new[] { "206-555-0101", "425-882-8080" }
    },
    new {
        Name = "Bob Harris",
        PhoneNumbers = new[] { "650-555-0199" }
    }
};
```

#### <a name="delegate-creation-expressions"></a><span data-ttu-id="dec56-1342">Výrazy vytvoření delegáta</span><span class="sxs-lookup"><span data-stu-id="dec56-1342">Delegate creation expressions</span></span>

<span data-ttu-id="dec56-1343">A *delegate_creation_expression* umožňuje vytvořit novou instanci třídy *delegate_type*.</span><span class="sxs-lookup"><span data-stu-id="dec56-1343">A *delegate_creation_expression* is used to create a new instance of a *delegate_type*.</span></span>

```antlr
delegate_creation_expression
    : 'new' delegate_type '(' expression ')'
    ;
```

<span data-ttu-id="dec56-1344">Argument výraz vytvářející delegáta musí být skupinu metod, anonymní funkce nebo hodnota typ času kompilace `dynamic` nebo *delegate_type*.</span><span class="sxs-lookup"><span data-stu-id="dec56-1344">The argument of a delegate creation expression must be a method group, an anonymous function or a value of either the compile time type `dynamic` or a *delegate_type*.</span></span> <span data-ttu-id="dec56-1345">Pokud je argumentem skupinu metod, identifikuje metodu a pro metodu instance objektu, pro který chcete vytvořit delegáta.</span><span class="sxs-lookup"><span data-stu-id="dec56-1345">If the argument is a method group, it identifies the method and, for an instance method, the object for which to create a delegate.</span></span> <span data-ttu-id="dec56-1346">Pokud argument je anonymní funkce přímo definuje parametry a tělo metody delegáta cíle.</span><span class="sxs-lookup"><span data-stu-id="dec56-1346">If the argument is an anonymous function it directly defines the parameters and method body of the delegate target.</span></span> <span data-ttu-id="dec56-1347">Pokud je argument hodnota identifikuje instanci delegáta, které chcete vytvořit kopii.</span><span class="sxs-lookup"><span data-stu-id="dec56-1347">If the argument is a value it identifies a delegate instance of which to create a copy.</span></span>

<span data-ttu-id="dec56-1348">Pokud *výraz* má typ kompilace `dynamic`, *delegate_creation_expression* dynamicky vázán ([dynamické vazby](expressions.md#dynamic-binding)) a následující pravidla platí se za běhu pomocí typu za běhu *výraz*.</span><span class="sxs-lookup"><span data-stu-id="dec56-1348">If the *expression* has the compile-time type `dynamic`, the *delegate_creation_expression* is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)), and the rules below are applied at run-time using the run-time type of the *expression*.</span></span> <span data-ttu-id="dec56-1349">V opačném případě pravidla se použijí v době kompilace.</span><span class="sxs-lookup"><span data-stu-id="dec56-1349">Otherwise the rules are applied at compile-time.</span></span>

<span data-ttu-id="dec56-1350">Vazba čas zpracování *delegate_creation_expression* formuláře `new D(E)`, kde `D` je *delegate_type* a `E` je *výraz* , se skládá z následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="dec56-1350">The binding-time processing of a *delegate_creation_expression* of the form `new D(E)`, where `D` is a *delegate_type* and `E` is an *expression*, consists of the following steps:</span></span>

*  <span data-ttu-id="dec56-1351">Pokud `E` je skupina metodu, výraz vytvoření delegáta je zpracována stejným způsobem jako převod skupin – metoda ([převody skupiny – metoda](conversions.md#method-group-conversions)) z `E` k `D`.</span><span class="sxs-lookup"><span data-stu-id="dec56-1351">If `E` is a method group, the delegate creation expression is processed in the same way as a method group conversion ([Method group conversions](conversions.md#method-group-conversions)) from `E` to `D`.</span></span>
*  <span data-ttu-id="dec56-1352">Pokud `E` je anonymní funkce, výraz vytvoření delegáta je zpracována stejným způsobem jako konverzi anonymní funkce ([anonymní funkce převody](conversions.md#anonymous-function-conversions)) z `E` k `D`.</span><span class="sxs-lookup"><span data-stu-id="dec56-1352">If `E` is an anonymous function, the delegate creation expression is processed in the same way as an anonymous function conversion ([Anonymous function conversions](conversions.md#anonymous-function-conversions)) from `E` to `D`.</span></span>
*  <span data-ttu-id="dec56-1353">Pokud `E` je hodnota, `E` musí být kompatibilní ([delegovat deklarace](delegates.md#delegate-declarations)) s `D`, a výsledkem je odkaz na nově vytvořený delegát typu `D` , který odkazuje na stejnou vyvolání jako seznam `E`.</span><span class="sxs-lookup"><span data-stu-id="dec56-1353">If `E` is a value, `E` must be compatible ([Delegate declarations](delegates.md#delegate-declarations)) with `D`, and the result is a reference to a newly created delegate of type `D` that refers to the same invocation list as `E`.</span></span> <span data-ttu-id="dec56-1354">Pokud `E` není kompatibilní s `D`, dojde k chybě kompilace.</span><span class="sxs-lookup"><span data-stu-id="dec56-1354">If `E` is not compatible with `D`, a compile-time error occurs.</span></span>

<span data-ttu-id="dec56-1355">Zpracování za běhu *delegate_creation_expression* formuláře `new D(E)`, kde `D` je *delegate_type* a `E` je *výraz* , se skládá z následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="dec56-1355">The run-time processing of a *delegate_creation_expression* of the form `new D(E)`, where `D` is a *delegate_type* and `E` is an *expression*, consists of the following steps:</span></span>

*   <span data-ttu-id="dec56-1356">Pokud `E` je skupina metodu, výraz vytvoření delegáta se vyhodnotí jako převod skupin – metoda ([převody skupiny – metoda](conversions.md#method-group-conversions)) z `E` k `D`.</span><span class="sxs-lookup"><span data-stu-id="dec56-1356">If `E` is a method group, the delegate creation expression is evaluated as a method group conversion ([Method group conversions](conversions.md#method-group-conversions)) from `E` to `D`.</span></span>
*   <span data-ttu-id="dec56-1357">Pokud `E` je anonymní funkce, vytvoření delegáta se vyhodnotí jako konverzi anonymní funkce z `E` k `D` ([anonymní funkce převody](conversions.md#anonymous-function-conversions)).</span><span class="sxs-lookup"><span data-stu-id="dec56-1357">If `E` is an anonymous function, the delegate creation is evaluated as an anonymous function conversion from `E` to `D` ([Anonymous function conversions](conversions.md#anonymous-function-conversions)).</span></span>
*   <span data-ttu-id="dec56-1358">Pokud `E` . má hodnotu *delegate_type*:</span><span class="sxs-lookup"><span data-stu-id="dec56-1358">If `E` is a value of a *delegate_type*:</span></span>
    * <span data-ttu-id="dec56-1359">`E` je vyhodnocen.</span><span class="sxs-lookup"><span data-stu-id="dec56-1359">`E` is evaluated.</span></span> <span data-ttu-id="dec56-1360">Je-li toto vyhodnocení způsobí výjimku, jsou spuštěny žádné další kroky.</span><span class="sxs-lookup"><span data-stu-id="dec56-1360">If this evaluation causes an exception, no further steps are executed.</span></span>
    * <span data-ttu-id="dec56-1361">Pokud hodnota `E` je `null`, `System.NullReferenceException` je vyvolána, a že jsou provedeny žádné další kroky.</span><span class="sxs-lookup"><span data-stu-id="dec56-1361">If the value of `E` is `null`, a `System.NullReferenceException` is thrown and no further steps are executed.</span></span>
    * <span data-ttu-id="dec56-1362">Nová instance typu delegáta `D` je přidělen.</span><span class="sxs-lookup"><span data-stu-id="dec56-1362">A new instance of the delegate type `D` is allocated.</span></span> <span data-ttu-id="dec56-1363">Pokud není k dispozici dostatek paměti k přidělení nové instance `System.OutOfMemoryException` je vyvolána, a že jsou provedeny žádné další kroky.</span><span class="sxs-lookup"><span data-stu-id="dec56-1363">If there is not enough memory available to allocate the new instance, a `System.OutOfMemoryException` is thrown and no further steps are executed.</span></span>
    * <span data-ttu-id="dec56-1364">Novou instanci delegáta je inicializována pomocí seznamu vyvolání jako instanci delegáta Dal `E`.</span><span class="sxs-lookup"><span data-stu-id="dec56-1364">The new delegate instance is initialized with the same invocation list as the delegate instance given by `E`.</span></span>

<span data-ttu-id="dec56-1365">Seznamu vyvolání delegáta je určena při delegáta je vytvořena instance a pak zůstává konstantní po celou dobu života delegáta.</span><span class="sxs-lookup"><span data-stu-id="dec56-1365">The invocation list of a delegate is determined when the delegate is instantiated and then remains constant for the entire lifetime of the delegate.</span></span> <span data-ttu-id="dec56-1366">Jinými slovy není možné po vytvoření změnit volatelných entity cílového delegáta.</span><span class="sxs-lookup"><span data-stu-id="dec56-1366">In other words, it is not possible to change the target callable entities of a delegate once it has been created.</span></span> <span data-ttu-id="dec56-1367">Při dvou delegátů jsou zkombinované nebo jeden se odebere z jiného ([delegovat deklarace](delegates.md#delegate-declarations)), Nový delegát výsledky; nemá žádné existující delegáta jeho obsah změnit.</span><span class="sxs-lookup"><span data-stu-id="dec56-1367">When two delegates are combined or one is removed from another ([Delegate declarations](delegates.md#delegate-declarations)), a new delegate results; no existing delegate has its contents changed.</span></span>

<span data-ttu-id="dec56-1368">Není možné k vytvoření delegáta, který odkazuje na vlastnost, indexer, uživatelem definovaný operátor, konstruktor instance, destruktor nebo statický konstruktor.</span><span class="sxs-lookup"><span data-stu-id="dec56-1368">It is not possible to create a delegate that refers to a property, indexer, user-defined operator, instance constructor, destructor, or static constructor.</span></span>

<span data-ttu-id="dec56-1369">Jak je popsáno výše, při vytvoření delegáta z metody skupiny, seznam formálních parametrů a návratový typ delegáta zjistit, které z přetížených metod k výběru.</span><span class="sxs-lookup"><span data-stu-id="dec56-1369">As described above, when a delegate is created from a method group, the formal parameter list and return type of the delegate determine which of the overloaded methods to select.</span></span> <span data-ttu-id="dec56-1370">V příkladu</span><span class="sxs-lookup"><span data-stu-id="dec56-1370">In the example</span></span>
```csharp
delegate double DoubleFunc(double x);

class A
{
    DoubleFunc f = new DoubleFunc(Square);

    static float Square(float x) {
        return x * x;
    }

    static double Square(double x) {
        return x * x;
    }
}
```
<span data-ttu-id="dec56-1371">`A.f` pole je inicializována pomocí delegáta, který odkazuje na druhý `Square` metoda vzhledem k tomu, že tato metoda přesně odpovídá seznamu formálních parametrů a návratový typ `DoubleFunc`.</span><span class="sxs-lookup"><span data-stu-id="dec56-1371">the `A.f` field is initialized with a delegate that refers to the second `Square` method because that method exactly matches the formal parameter list and return type of `DoubleFunc`.</span></span> <span data-ttu-id="dec56-1372">Měl druhý `Square` metoda nebyly k dispozici, za kompilace by došlo k chybě.</span><span class="sxs-lookup"><span data-stu-id="dec56-1372">Had the second `Square` method not been present, a compile-time error would have occurred.</span></span>

#### <a name="anonymous-object-creation-expressions"></a><span data-ttu-id="dec56-1373">Anonymní objekt vytváření výrazů</span><span class="sxs-lookup"><span data-stu-id="dec56-1373">Anonymous object creation expressions</span></span>

<span data-ttu-id="dec56-1374">*Anonymous_object_creation_expression* slouží k vytvoření objektu anonymního typu.</span><span class="sxs-lookup"><span data-stu-id="dec56-1374">An *anonymous_object_creation_expression* is used to create an object of an anonymous type.</span></span>

```antlr
anonymous_object_creation_expression
    : 'new' anonymous_object_initializer
    ;

anonymous_object_initializer
    : '{' member_declarator_list? '}'
    | '{' member_declarator_list ',' '}'
    ;

member_declarator_list
    : member_declarator (',' member_declarator)*
    ;

member_declarator
    : simple_name
    | member_access
    | base_access
    | null_conditional_member_access
    | identifier '=' expression
    ;
```

<span data-ttu-id="dec56-1375">Inicializátor anonymních objektů deklaruje anonymního typu a vrátí instance daného typu.</span><span class="sxs-lookup"><span data-stu-id="dec56-1375">An anonymous object initializer declares an anonymous type and returns an instance of that type.</span></span> <span data-ttu-id="dec56-1376">Anonymní typ není typem nameless třídy, který dědí přímo z `object`.</span><span class="sxs-lookup"><span data-stu-id="dec56-1376">An anonymous type is a nameless class type that inherits directly from `object`.</span></span> <span data-ttu-id="dec56-1377">Členové anonymního typu jsou posloupnost odvodit z inicializátoru anonymní objekt použitý k vytvoření instance typu vlastnosti jen pro čtení.</span><span class="sxs-lookup"><span data-stu-id="dec56-1377">The members of an anonymous type are a sequence of read-only properties inferred from the anonymous object initializer used to create an instance of the type.</span></span> <span data-ttu-id="dec56-1378">Konkrétně inicializátor anonymních objektů ve formátu</span><span class="sxs-lookup"><span data-stu-id="dec56-1378">Specifically, an anonymous object initializer of the form</span></span>
```csharp
new { p1 = e1, p2 = e2, ..., pn = en }
```
<span data-ttu-id="dec56-1379">deklaruje anonymního typu formuláře</span><span class="sxs-lookup"><span data-stu-id="dec56-1379">declares an anonymous type of the form</span></span>
```csharp
class __Anonymous1
{
    private readonly T1 f1;
    private readonly T2 f2;
    ...
    private readonly Tn fn;

    public __Anonymous1(T1 a1, T2 a2, ..., Tn an) {
        f1 = a1;
        f2 = a2;
        ...
        fn = an;
    }

    public T1 p1 { get { return f1; } }
    public T2 p2 { get { return f2; } }
    ...
    public Tn pn { get { return fn; } }

    public override bool Equals(object __o) { ... }
    public override int GetHashCode() { ... }
}
```
<span data-ttu-id="dec56-1380">kde každý `Tx` je typ odpovídající výraz `ex`.</span><span class="sxs-lookup"><span data-stu-id="dec56-1380">where each `Tx` is the type of the corresponding expression `ex`.</span></span> <span data-ttu-id="dec56-1381">Výraz použitý v *member_declarator* musí být typu.</span><span class="sxs-lookup"><span data-stu-id="dec56-1381">The expression used in a *member_declarator* must have a type.</span></span> <span data-ttu-id="dec56-1382">Díky tomu se jedná o chybu kompilace výrazu v *member_declarator* na hodnotu null nebo je anonymní funkce.</span><span class="sxs-lookup"><span data-stu-id="dec56-1382">Thus, it is a compile-time error for an expression in a *member_declarator* to be null or an anonymous function.</span></span> <span data-ttu-id="dec56-1383">Také se jedná o chybu kompilace pro výraz unsafe typu.</span><span class="sxs-lookup"><span data-stu-id="dec56-1383">It is also a compile-time error for the expression to have an unsafe type.</span></span>

<span data-ttu-id="dec56-1384">Názvy anonymního typu a parametru k jeho `Equals` metody jsou automaticky generovány v kompilátoru a se nedá odkazovat v textu programu.</span><span class="sxs-lookup"><span data-stu-id="dec56-1384">The names of an anonymous type and of the parameter to its `Equals` method are automatically generated by the compiler and cannot be referenced in program text.</span></span>

<span data-ttu-id="dec56-1385">Ve stejném programu vytvoří dvě inicializátory anonymních objektů určující posloupnost vlastnosti stejné názvy a typy v době kompilace ve stejném pořadí výskyty stejného anonymního typu.</span><span class="sxs-lookup"><span data-stu-id="dec56-1385">Within the same program, two anonymous object initializers that specify a sequence of properties of the same names and compile-time types in the same order will produce instances of the same anonymous type.</span></span>

<span data-ttu-id="dec56-1386">V příkladu</span><span class="sxs-lookup"><span data-stu-id="dec56-1386">In the example</span></span>
```csharp
var p1 = new { Name = "Lawnmower", Price = 495.00 };
var p2 = new { Name = "Shovel", Price = 26.95 };
p1 = p2;
```
<span data-ttu-id="dec56-1387">přiřazení na posledním řádku je povolen, protože `p1` a `p2` jsou stejné anonymního typu.</span><span class="sxs-lookup"><span data-stu-id="dec56-1387">the assignment on the last line is permitted because `p1` and `p2` are of the same anonymous type.</span></span>

<span data-ttu-id="dec56-1388">`Equals` a `GetHashcode` metody anonymních typů přepište metody zděděné z `object`a jsou definovány z hlediska `Equals` a `GetHashcode` vlastností tak, aby dva výskyty stejného anonymního typu jsou si rovny Pokud a pouze v případě, že jejich vlastnosti jsou stejné.</span><span class="sxs-lookup"><span data-stu-id="dec56-1388">The `Equals` and `GetHashcode` methods on anonymous types override the methods inherited from `object`, and are defined in terms of the `Equals` and `GetHashcode` of the properties, so that two instances of the same anonymous type are equal if and only if all their properties are equal.</span></span>

<span data-ttu-id="dec56-1389">Deklarátor členské lze zkrátit na jednoduchý název ([odvození typu](expressions.md#type-inference)), přístup ke členu ([kompilace kontrolu dynamické přetížení](expressions.md#compile-time-checking-of-dynamic-overload-resolution)), základní přístup ([základní přístup](expressions.md#base-access)) nebo člen null podmíněný přístup ([Null podmíněné výrazy jako inicializátory projekce](expressions.md#null-conditional-expressions-as-projection-initializers)).</span><span class="sxs-lookup"><span data-stu-id="dec56-1389">A member declarator can be abbreviated to a simple name ([Type inference](expressions.md#type-inference)), a member access ([Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution)), a base access ([Base access](expressions.md#base-access)) or a null-conditional member access ([Null-conditional expressions as projection initializers](expressions.md#null-conditional-expressions-as-projection-initializers)).</span></span> <span data-ttu-id="dec56-1390">Tento postup se nazývá ***projekce inicializátor*** a představuje zkratku pro deklaraci a přiřazení k vlastnosti se stejným názvem.</span><span class="sxs-lookup"><span data-stu-id="dec56-1390">This is called a ***projection initializer*** and is shorthand for a declaration of and assignment to a property with the same name.</span></span> <span data-ttu-id="dec56-1391">Konkrétně deklarátory členů formulářů</span><span class="sxs-lookup"><span data-stu-id="dec56-1391">Specifically, member declarators of the forms</span></span>
```csharp
identifier
expr.identifier
```
<span data-ttu-id="dec56-1392">jsou přesně odpovídá následující příkaz, v uvedeném pořadí:</span><span class="sxs-lookup"><span data-stu-id="dec56-1392">are precisely equivalent to the following, respectively:</span></span>
```csharp
identifier = identifier
identifier = expr.identifier
```

<span data-ttu-id="dec56-1393">Proto v inicializátoru projekce *identifikátor* vybere hodnota a pole nebo vlastnost, ke kterému je přiřazena hodnota.</span><span class="sxs-lookup"><span data-stu-id="dec56-1393">Thus, in a projection initializer the *identifier* selects both the value and the field or property to which the value is assigned.</span></span> <span data-ttu-id="dec56-1394">Inicializátor projekce intuitivně, projekty, nejen hodnotu, ale také název hodnoty.</span><span class="sxs-lookup"><span data-stu-id="dec56-1394">Intuitively, a projection initializer projects not just a value, but also the name of the value.</span></span>

### <a name="the-typeof-operator"></a><span data-ttu-id="dec56-1395">Typeof – operátor</span><span class="sxs-lookup"><span data-stu-id="dec56-1395">The typeof operator</span></span>

<span data-ttu-id="dec56-1396">`typeof` Operátor se používá k získání `System.Type` pro typ objektu.</span><span class="sxs-lookup"><span data-stu-id="dec56-1396">The `typeof` operator is used to obtain the `System.Type` object for a type.</span></span>

```antlr
typeof_expression
    : 'typeof' '(' type ')'
    | 'typeof' '(' unbound_type_name ')'
    | 'typeof' '(' 'void' ')'
    ;

unbound_type_name
    : identifier generic_dimension_specifier?
    | identifier '::' identifier generic_dimension_specifier?
    | unbound_type_name '.' identifier generic_dimension_specifier?
    ;

generic_dimension_specifier
    : '<' comma* '>'
    ;

comma
    : ','
    ;
```

<span data-ttu-id="dec56-1397">První formulář *typeof_expression* se skládá z `typeof` – klíčové slovo následované v závorce *typ*.</span><span class="sxs-lookup"><span data-stu-id="dec56-1397">The first form of *typeof_expression* consists of a `typeof` keyword followed by a parenthesized *type*.</span></span> <span data-ttu-id="dec56-1398">Výsledkem výrazu tohoto formuláře je `System.Type` pro zvolený typ objektu.</span><span class="sxs-lookup"><span data-stu-id="dec56-1398">The result of an expression of this form is the `System.Type` object for the indicated type.</span></span> <span data-ttu-id="dec56-1399">Existuje pouze jeden `System.Type` objekt daného typu.</span><span class="sxs-lookup"><span data-stu-id="dec56-1399">There is only one `System.Type` object for any given type.</span></span> <span data-ttu-id="dec56-1400">To znamená, že pro typ `T`, `typeof(T) == typeof(T)` má vždy hodnotu true.</span><span class="sxs-lookup"><span data-stu-id="dec56-1400">This means that for a type `T`, `typeof(T) == typeof(T)` is always true.</span></span> <span data-ttu-id="dec56-1401">*Typ* nemůže být `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="dec56-1401">The *type* cannot be `dynamic`.</span></span>

<span data-ttu-id="dec56-1402">Tedy o druhou podobu *typeof_expression* se skládá z `typeof` – klíčové slovo následované v závorce *unbound_type_name*.</span><span class="sxs-lookup"><span data-stu-id="dec56-1402">The second form of *typeof_expression* consists of a `typeof` keyword followed by a parenthesized *unbound_type_name*.</span></span> <span data-ttu-id="dec56-1403">*Unbound_type_name* je velmi podobný *type_name* ([Namespace a zadejte názvy](basic-concepts.md#namespace-and-type-names)) s tím rozdílem, že *unbound_type_name* obsahuje *generic_dimension_specifier*s kde *type_name* obsahuje *type_argument_list*s.</span><span class="sxs-lookup"><span data-stu-id="dec56-1403">An *unbound_type_name* is very similar to a *type_name* ([Namespace and type names](basic-concepts.md#namespace-and-type-names)) except that an *unbound_type_name* contains *generic_dimension_specifier*s where a *type_name* contains *type_argument_list*s.</span></span> <span data-ttu-id="dec56-1404">Když operand *typeof_expression* je sekvence tokenů, který splňuje gramatiky obou *unbound_type_name* a *type_name*, zejména pokud obsahuje ani *generic_dimension_specifier* ani *type_argument_list*, posloupnost tokeny se považuje za *type_name*.</span><span class="sxs-lookup"><span data-stu-id="dec56-1404">When the operand of a *typeof_expression* is a sequence of tokens that satisfies the grammars of both *unbound_type_name* and *type_name*, namely when it contains neither a *generic_dimension_specifier* nor a *type_argument_list*, the sequence of tokens is considered to be a *type_name*.</span></span> <span data-ttu-id="dec56-1405">Význam *unbound_type_name* je stanoven následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="dec56-1405">The meaning of an *unbound_type_name* is determined as follows:</span></span>

*  <span data-ttu-id="dec56-1406">Převod sekvence tokenů *type_name* nahrazením každého *generic_dimension_specifier* s *type_argument_list* mají stejný počet čárkami a klíčové slovo `object` jako každý *type_argument*.</span><span class="sxs-lookup"><span data-stu-id="dec56-1406">Convert the sequence of tokens to a *type_name* by replacing each *generic_dimension_specifier* with a *type_argument_list* having the same number of commas and the keyword `object` as each *type_argument*.</span></span>
*  <span data-ttu-id="dec56-1407">Vyhodnocení výsledný *type_name*, při ignoruje všechny omezeními parametrů typů.</span><span class="sxs-lookup"><span data-stu-id="dec56-1407">Evaluate the resulting *type_name*, while ignoring all type parameter constraints.</span></span>
*  <span data-ttu-id="dec56-1408">*Unbound_type_name* přeloží na nevázaný parametr generického typu přidružené výsledný konstruovaný typ ([vázaný a nevázaných typy](types.md#bound-and-unbound-types)).</span><span class="sxs-lookup"><span data-stu-id="dec56-1408">The *unbound_type_name* resolves to the unbound generic type associated with the resulting constructed type ([Bound and unbound types](types.md#bound-and-unbound-types)).</span></span>

<span data-ttu-id="dec56-1409">Výsledkem *typeof_expression* je `System.Type` objektu pro výsledný nevázaných obecného typu.</span><span class="sxs-lookup"><span data-stu-id="dec56-1409">The result of the *typeof_expression* is the `System.Type` object for the resulting unbound generic type.</span></span>

<span data-ttu-id="dec56-1410">Třetí forma *typeof_expression* se skládá z `typeof` – klíčové slovo následované v závorce `void` – klíčové slovo.</span><span class="sxs-lookup"><span data-stu-id="dec56-1410">The third form of *typeof_expression* consists of a `typeof` keyword followed by a parenthesized `void` keyword.</span></span> <span data-ttu-id="dec56-1411">Výsledkem výrazu tohoto formuláře je `System.Type` objekt, který reprezentuje neexistence typu.</span><span class="sxs-lookup"><span data-stu-id="dec56-1411">The result of an expression of this form is the `System.Type` object that represents the absence of a type.</span></span> <span data-ttu-id="dec56-1412">Typ objektu vrácený `typeof(void)` se liší od typu objekt vrácený pro libovolného typu.</span><span class="sxs-lookup"><span data-stu-id="dec56-1412">The type object returned by `typeof(void)` is distinct from the type object returned for any type.</span></span> <span data-ttu-id="dec56-1413">Tento objekt speciální typ je užitečné v knihovnách tříd, umožňujících reflexe do metody v jazyce, ve kterém chcete způsob, jak reprezentaci návratový typ jakoukoli metodu, včetně void metody instance těchto metod `System.Type`.</span><span class="sxs-lookup"><span data-stu-id="dec56-1413">This special type object is useful in class libraries that allow reflection onto methods in the language, where those methods wish to have a way to represent the return type of any method, including void methods, with an instance of `System.Type`.</span></span>

<span data-ttu-id="dec56-1414">`typeof` Operátor lze použít pro parametr typu.</span><span class="sxs-lookup"><span data-stu-id="dec56-1414">The `typeof` operator can be used on a type parameter.</span></span> <span data-ttu-id="dec56-1415">Výsledkem je `System.Type` objektu pro typ za běhu, která byla vázána na parametr typu.</span><span class="sxs-lookup"><span data-stu-id="dec56-1415">The result is the `System.Type` object for the run-time type that was bound to the type parameter.</span></span> <span data-ttu-id="dec56-1416">`typeof` Operátoru lze také na konstruovaný typ nebo nevázaný parametr generického typu ([vázaný a nevázaných typy](types.md#bound-and-unbound-types)).</span><span class="sxs-lookup"><span data-stu-id="dec56-1416">The `typeof` operator can also be used on a constructed type or an unbound generic type ([Bound and unbound types](types.md#bound-and-unbound-types)).</span></span> <span data-ttu-id="dec56-1417">`System.Type` Objektu pro nevázaný parametr generického typu není stejný jako `System.Type` objekt typu instance.</span><span class="sxs-lookup"><span data-stu-id="dec56-1417">The `System.Type` object for an unbound generic type is not the same as the `System.Type` object of the instance type.</span></span> <span data-ttu-id="dec56-1418">Typ instance je vždy uzavřený konstruovaný typ v době běhu tak jeho `System.Type` objektu závisí na argumenty typu modulu runtime používá, zatímco nevázaný parametr generického typu nemá žádné argumenty typu.</span><span class="sxs-lookup"><span data-stu-id="dec56-1418">The instance type is always a closed constructed type at run-time so its `System.Type` object depends on the run-time type arguments in use, while the unbound generic type has no type arguments.</span></span>

<span data-ttu-id="dec56-1419">V příkladu</span><span class="sxs-lookup"><span data-stu-id="dec56-1419">The example</span></span>
```csharp
using System;

class X<T>
{
    public static void PrintTypes() {
        Type[] t = {
            typeof(int),
            typeof(System.Int32),
            typeof(string),
            typeof(double[]),
            typeof(void),
            typeof(T),
            typeof(X<T>),
            typeof(X<X<T>>),
            typeof(X<>)
        };
        for (int i = 0; i < t.Length; i++) {
            Console.WriteLine(t[i]);
        }
    }
}

class Test
{
    static void Main() {
        X<int>.PrintTypes();
    }
}
```
<span data-ttu-id="dec56-1420">vytvoří následující výstup:</span><span class="sxs-lookup"><span data-stu-id="dec56-1420">produces the following output:</span></span>
```
System.Int32
System.Int32
System.String
System.Double[]
System.Void
System.Int32
X`1[System.Int32]
X`1[X`1[System.Int32]]
X`1[T]
```

<span data-ttu-id="dec56-1421">Všimněte si, že `int` a `System.Int32` jsou stejného typu.</span><span class="sxs-lookup"><span data-stu-id="dec56-1421">Note that `int` and `System.Int32` are the same type.</span></span>

<span data-ttu-id="dec56-1422">Všimněte si také, že výsledek `typeof(X<>)` není závislý na argument typu, ale výsledek `typeof(X<T>)` nepodporuje.</span><span class="sxs-lookup"><span data-stu-id="dec56-1422">Also note that the result of `typeof(X<>)` does not depend on the type argument but the result of `typeof(X<T>)` does.</span></span>

### <a name="the-checked-and-unchecked-operators"></a><span data-ttu-id="dec56-1423">Operátory zaškrtnuto a nezaškrtnuto</span><span class="sxs-lookup"><span data-stu-id="dec56-1423">The checked and unchecked operators</span></span>

<span data-ttu-id="dec56-1424">`checked` a `unchecked` operátory jsou slouží ke kontrole, ***kontextu kontroly přetečení*** integrálového typu aritmetické operace a převody.</span><span class="sxs-lookup"><span data-stu-id="dec56-1424">The `checked` and `unchecked` operators are used to control the ***overflow checking context*** for integral-type arithmetic operations and conversions.</span></span>

```antlr
checked_expression
    : 'checked' '(' expression ')'
    ;

unchecked_expression
    : 'unchecked' '(' expression ')'
    ;
```

<span data-ttu-id="dec56-1425">`checked` Operátor vyhodnotí uzavřeného výrazu ve zkontrolovaném kontextu a `unchecked` operátor vyhodnotí uzavřeného výrazu v nezkontrolovaném kontextu.</span><span class="sxs-lookup"><span data-stu-id="dec56-1425">The `checked` operator evaluates the contained expression in a checked context, and the `unchecked` operator evaluates the contained expression in an unchecked context.</span></span> <span data-ttu-id="dec56-1426">A *checked_expression* nebo *unchecked_expression* přesně odpovídá *parenthesized_expression* ([výrazech se závorkami](expressions.md#parenthesized-expressions)), s tím rozdílem, že omezením výraz je vyhodnocen v dané kontroly kontextu přetečení.</span><span class="sxs-lookup"><span data-stu-id="dec56-1426">A *checked_expression* or *unchecked_expression* corresponds exactly to a *parenthesized_expression* ([Parenthesized expressions](expressions.md#parenthesized-expressions)), except that the contained expression is evaluated in the given overflow checking context.</span></span>

<span data-ttu-id="dec56-1427">Přetečení kontrola kontextu je možné řídit také prostřednictvím `checked` a `unchecked` příkazy ([příkazy zaškrtnuto a nezaškrtnuto](statements.md#the-checked-and-unchecked-statements)).</span><span class="sxs-lookup"><span data-stu-id="dec56-1427">The overflow checking context can also be controlled through the `checked` and `unchecked` statements ([The checked and unchecked statements](statements.md#the-checked-and-unchecked-statements)).</span></span>

<span data-ttu-id="dec56-1428">Tyto operace jsou ovlivněny kontroly kontextu stanovené přetečení `checked` a `unchecked` operátorů a příkazů:</span><span class="sxs-lookup"><span data-stu-id="dec56-1428">The following operations are affected by the overflow checking context established by the `checked` and `unchecked` operators and statements:</span></span>

*  <span data-ttu-id="dec56-1429">Předdefinované `++` a `--` unárních operátorů ([Příponové operátory Inkrementace a dekrementace operátory](expressions.md#postfix-increment-and-decrement-operators) a [předpony Inkrementace a dekrementace operátory](expressions.md#prefix-increment-and-decrement-operators)), když je celočíselný operand Zadejte.</span><span class="sxs-lookup"><span data-stu-id="dec56-1429">The predefined `++` and `--` unary operators ([Postfix increment and decrement operators](expressions.md#postfix-increment-and-decrement-operators) and [Prefix increment and decrement operators](expressions.md#prefix-increment-and-decrement-operators)), when the operand is of an integral type.</span></span>
*  <span data-ttu-id="dec56-1430">Předdefinované `-` unárního operátoru ([unární operátor minus](expressions.md#unary-minus-operator)), když je operand typu celé číslo.</span><span class="sxs-lookup"><span data-stu-id="dec56-1430">The predefined `-` unary operator ([Unary minus operator](expressions.md#unary-minus-operator)), when the operand is of an integral type.</span></span>
*  <span data-ttu-id="dec56-1431">Předdefinované `+`, `-`, `*`, a `/` binárních operátorů ([aritmetické operátory](expressions.md#arithmetic-operators)), pokud jsou oba operandy integrální typy.</span><span class="sxs-lookup"><span data-stu-id="dec56-1431">The predefined `+`, `-`, `*`, and `/` binary operators ([Arithmetic operators](expressions.md#arithmetic-operators)), when both operands are of integral types.</span></span>
*  <span data-ttu-id="dec56-1432">Explicitních číselných převodů ([explicitních číselných převodů](conversions.md#explicit-numeric-conversions)) z jednoho celočíselného typu na jiný celočíselný typ nebo z `float` nebo `double` na celočíselný typ.</span><span class="sxs-lookup"><span data-stu-id="dec56-1432">Explicit numeric conversions ([Explicit numeric conversions](conversions.md#explicit-numeric-conversions)) from one integral type to another integral type, or from `float` or `double` to an integral type.</span></span>

<span data-ttu-id="dec56-1433">Při jedné z výše uvedených operací vytvoření výsledku, který je příliš velký pro reprezentaci typu cílového, kontext, ve které operaci provádí ovládací prvky výsledné chování:</span><span class="sxs-lookup"><span data-stu-id="dec56-1433">When one of the above operations produce a result that is too large to represent in the destination type, the context in which the operation is performed controls the resulting behavior:</span></span>

*  <span data-ttu-id="dec56-1434">V `checked` kontextu, pokud je operace konstantní výraz ([konstantní výrazy](expressions.md#constant-expressions)), dojde k chybě kompilace.</span><span class="sxs-lookup"><span data-stu-id="dec56-1434">In a `checked` context, if the operation is a constant expression ([Constant expressions](expressions.md#constant-expressions)), a compile-time error occurs.</span></span> <span data-ttu-id="dec56-1435">Jinak, pokud se operace provádí za běhu, `System.OverflowException` je vyvolána výjimka.</span><span class="sxs-lookup"><span data-stu-id="dec56-1435">Otherwise, when the operation is performed at run-time, a `System.OverflowException` is thrown.</span></span>
*  <span data-ttu-id="dec56-1436">V `unchecked` je rozdělená do kontextu, výsledek se zahodí všechny nejvyšším bity, které se nevejdou do cílového typu.</span><span class="sxs-lookup"><span data-stu-id="dec56-1436">In an `unchecked` context, the result is truncated by discarding any high-order bits that do not fit in the destination type.</span></span>

<span data-ttu-id="dec56-1437">Pro nekonstantní výrazy (výrazy, které jsou vyhodnocovány v době běhu), které se nenacházejí žádné `checked` nebo `unchecked` operátory nebo příkazy, je výchozí přetečení kontrola kontextu `unchecked` Pokud externí faktory (jako je například kompilátor přepínače a konfiguraci prostředí spuštění) vyžadují `checked` hodnocení.</span><span class="sxs-lookup"><span data-stu-id="dec56-1437">For non-constant expressions (expressions that are evaluated at run-time) that are not enclosed by any `checked` or `unchecked` operators or statements, the default overflow checking context is `unchecked` unless external factors (such as compiler switches and execution environment configuration) call for `checked` evaluation.</span></span>

<span data-ttu-id="dec56-1438">Pro konstantní výrazy (výrazy, které může být plně vyhodnocen v době kompilace), je vždy kontroly kontextu přetečení výchozí `checked`.</span><span class="sxs-lookup"><span data-stu-id="dec56-1438">For constant expressions (expressions that can be fully evaluated at compile-time), the default overflow checking context is always `checked`.</span></span> <span data-ttu-id="dec56-1439">Pokud není konstantní výraz se explicitně umístí do `unchecked` kontextu přetečení, ke kterým dochází při vyhodnocení za kompilace výrazu vždy způsobit chyby kompilace.</span><span class="sxs-lookup"><span data-stu-id="dec56-1439">Unless a constant expression is explicitly placed in an `unchecked` context, overflows that occur during the compile-time evaluation of the expression always cause compile-time errors.</span></span>

<span data-ttu-id="dec56-1440">Není ovlivněna tělo anonymní funkce `checked` nebo `unchecked` kontextech, ve kterých dojde k anonymní funkce.</span><span class="sxs-lookup"><span data-stu-id="dec56-1440">The body of an anonymous function is not affected by `checked` or `unchecked` contexts in which the anonymous function occurs.</span></span>

<span data-ttu-id="dec56-1441">V příkladu</span><span class="sxs-lookup"><span data-stu-id="dec56-1441">In the example</span></span>
```csharp
class Test
{
    static readonly int x = 1000000;
    static readonly int y = 1000000;

    static int F() {
        return checked(x * y);      // Throws OverflowException
    }

    static int G() {
        return unchecked(x * y);    // Returns -727379968
    }

    static int H() {
        return x * y;               // Depends on default
    }
}
```
<span data-ttu-id="dec56-1442">vzhledem k tomu, že ani jeden z výrazů může být vyhodnocen v době kompilace, jsou hlášeny žádné chyby kompilace.</span><span class="sxs-lookup"><span data-stu-id="dec56-1442">no compile-time errors are reported since neither of the expressions can be evaluated at compile-time.</span></span> <span data-ttu-id="dec56-1443">V době běhu `F` vyvolá metoda výjimku `System.OverflowException`a `G` metoda vrátí-727379968 (nižší 32 bitů výsledek mimo rozsah).</span><span class="sxs-lookup"><span data-stu-id="dec56-1443">At run-time, the `F` method throws a `System.OverflowException`, and the `G` method returns -727379968 (the lower 32 bits of the out-of-range result).</span></span> <span data-ttu-id="dec56-1444">Chování `H` metoda závisí na výchozí přetečení kontrola kontext kompilace, ale je buď stejná jako `F` nebo stejná jako `G`.</span><span class="sxs-lookup"><span data-stu-id="dec56-1444">The behavior of the `H` method depends on the default overflow checking context for the compilation, but it is either the same as `F` or the same as `G`.</span></span>

<span data-ttu-id="dec56-1445">V příkladu</span><span class="sxs-lookup"><span data-stu-id="dec56-1445">In the example</span></span>
```csharp
class Test
{
    const int x = 1000000;
    const int y = 1000000;

    static int F() {
        return checked(x * y);      // Compile error, overflow
    }

    static int G() {
        return unchecked(x * y);    // Returns -727379968
    }

    static int H() {
        return x * y;               // Compile error, overflow
    }
}
```
<span data-ttu-id="dec56-1446">přetečení, ke kterým dochází při vyhodnocování konstantní výrazy u `F` a `H` způsobit chyby kompilace nahlásit, protože jsou výrazy vyhodnocovány v `checked` kontextu.</span><span class="sxs-lookup"><span data-stu-id="dec56-1446">the overflows that occur when evaluating the constant expressions in `F` and `H` cause compile-time errors to be reported because the expressions are evaluated in a `checked` context.</span></span> <span data-ttu-id="dec56-1447">Přetečení také vyvolá se při vyhodnocení konstantního výrazu v `G`, ale protože vyhodnocení se provádí `unchecked` kontextu, není hlášena přetečení.</span><span class="sxs-lookup"><span data-stu-id="dec56-1447">An overflow also occurs when evaluating the constant expression in `G`, but since the evaluation takes place in an `unchecked` context, the overflow is not reported.</span></span>

<span data-ttu-id="dec56-1448">`checked` a `unchecked` operátory ovlivní pouze kontroly kontext pro tyto operace, které jsou obsaženy pomocí textu v rámci přetečení "`(`"a"`)`" tokeny.</span><span class="sxs-lookup"><span data-stu-id="dec56-1448">The `checked` and `unchecked` operators only affect the overflow checking context for those operations that are textually contained within the "`(`" and "`)`" tokens.</span></span> <span data-ttu-id="dec56-1449">Operátory nemají žádný vliv na funkce členy, které jsou vyvolány jako výsledek vyhodnocení výrazu omezením.</span><span class="sxs-lookup"><span data-stu-id="dec56-1449">The operators have no effect on function members that are invoked as a result of evaluating the contained expression.</span></span> <span data-ttu-id="dec56-1450">V příkladu</span><span class="sxs-lookup"><span data-stu-id="dec56-1450">In the example</span></span>
```csharp
class Test
{
    static int Multiply(int x, int y) {
        return x * y;
    }

    static int F() {
        return checked(Multiply(1000000, 1000000));
    }
}
```
<span data-ttu-id="dec56-1451">použití `checked` v `F` nemá vliv na vyhodnocení `x * y` v `Multiply`, takže `x * y` je vyhodnocen v přetečení výchozí kontrola kontextu.</span><span class="sxs-lookup"><span data-stu-id="dec56-1451">the use of `checked` in `F` does not affect the evaluation of `x * y` in `Multiply`, so `x * y` is evaluated in the default overflow checking context.</span></span>

<span data-ttu-id="dec56-1452">`unchecked` Operátor je vhodné při zápisu konstanty z podepsaných integrálních typů v šestnáctkové soustavě.</span><span class="sxs-lookup"><span data-stu-id="dec56-1452">The `unchecked` operator is convenient when writing constants of the signed integral types in hexadecimal notation.</span></span> <span data-ttu-id="dec56-1453">Příklad:</span><span class="sxs-lookup"><span data-stu-id="dec56-1453">For example:</span></span>
```csharp
class Test
{
    public const int AllBits = unchecked((int)0xFFFFFFFF);

    public const int HighBit = unchecked((int)0x80000000);
}
```

<span data-ttu-id="dec56-1454">Obě výše šestnáctkové konstanty jsou typu `uint`.</span><span class="sxs-lookup"><span data-stu-id="dec56-1454">Both of the hexadecimal constants above are of type `uint`.</span></span> <span data-ttu-id="dec56-1455">Protože konstanty jsou mimo `int` v rozsahu, aniž by `unchecked` operátorů, přetypování na `int` byste mohli vytvořit chyby kompilace.</span><span class="sxs-lookup"><span data-stu-id="dec56-1455">Because the constants are outside the `int` range, without the `unchecked` operator, the casts to `int` would produce compile-time errors.</span></span>

<span data-ttu-id="dec56-1456">`checked` a `unchecked` operátorů a příkazů umožňují programátorům ovládat některé aspekty některé číselné výpočty.</span><span class="sxs-lookup"><span data-stu-id="dec56-1456">The `checked` and `unchecked` operators and statements allow programmers to control certain aspects of some numeric calculations.</span></span> <span data-ttu-id="dec56-1457">Chování některé numerické operátory však závisí na datové typy jeho operandů.</span><span class="sxs-lookup"><span data-stu-id="dec56-1457">However, the behavior of some numeric operators depends on their operands' data types.</span></span> <span data-ttu-id="dec56-1458">Například vždy vynásobí dvě desetinná místa má za následek výjimku při přetečení i v rámci explicitně `unchecked` vytvořit.</span><span class="sxs-lookup"><span data-stu-id="dec56-1458">For example, multiplying two decimals always results in an exception on overflow even within an explicitly `unchecked` construct.</span></span> <span data-ttu-id="dec56-1459">Obdobně součin dvou čísel s plovoucí čárkou nikdy výsledky v výjimku při přetečení i v rámci explicitně `checked` vytvořit.</span><span class="sxs-lookup"><span data-stu-id="dec56-1459">Similarly, multiplying two floats never results in an exception on overflow even within an explicitly `checked` construct.</span></span> <span data-ttu-id="dec56-1460">Kromě toho ostatní operátory jsou nikdy ovlivněny režimu kontroly, zda výchozí nebo explicitní.</span><span class="sxs-lookup"><span data-stu-id="dec56-1460">In addition, other operators are never affected by the mode of checking, whether default or explicit.</span></span>

### <a name="default-value-expressions"></a><span data-ttu-id="dec56-1461">Výrazy s výchozími hodnotami</span><span class="sxs-lookup"><span data-stu-id="dec56-1461">Default value expressions</span></span>

<span data-ttu-id="dec56-1462">Výraz výchozí hodnoty se používá k získání výchozí hodnota ([výchozí hodnoty](variables.md#default-values)) typu.</span><span class="sxs-lookup"><span data-stu-id="dec56-1462">A default value expression is used to obtain the default value ([Default values](variables.md#default-values)) of a type.</span></span> <span data-ttu-id="dec56-1463">Výraz výchozí hodnoty se obvykle používá pro parametry typu, protože nemusí být známé, pokud parametr typu je typ hodnoty nebo typ odkazu.</span><span class="sxs-lookup"><span data-stu-id="dec56-1463">Typically a default value expression is used for type parameters, since it may not be known if the type parameter is a value type or a reference type.</span></span> <span data-ttu-id="dec56-1464">(Neexistuje žádný převod z `null` literál na parametr typu. Pokud je parametr typu je znám jako typ odkazu.)</span><span class="sxs-lookup"><span data-stu-id="dec56-1464">(No conversion exists from the `null` literal to a type parameter unless the type parameter is known to be a reference type.)</span></span>

```antlr
default_value_expression
    : 'default' '(' type ')'
    ;
```

<span data-ttu-id="dec56-1465">Pokud *typ* v *default_value_expression* vyhodnotí v době běhu na typ odkazu, výsledkem je `null` převést na typu.</span><span class="sxs-lookup"><span data-stu-id="dec56-1465">If the *type* in a *default_value_expression* evaluates at run-time to a reference type, the result is `null` converted to that type.</span></span> <span data-ttu-id="dec56-1466">Pokud *typ* v *default_value_expression* vyhodnotí v době běhu na typ hodnoty, výsledkem je *value_type*výchozí hodnota ([výchozí Konstruktory](types.md#default-constructors)).</span><span class="sxs-lookup"><span data-stu-id="dec56-1466">If the *type* in a *default_value_expression* evaluates at run-time to a value type, the result is the *value_type*'s default value ([Default constructors](types.md#default-constructors)).</span></span>

<span data-ttu-id="dec56-1467">A *default_value_expression* je konstantní výraz ([konstantní výrazy](expressions.md#constant-expressions)) Pokud je typ typem odkazu nebo parametr typu, který je znám jako typ odkazu ([parametr typu omezení](classes.md#type-parameter-constraints)).</span><span class="sxs-lookup"><span data-stu-id="dec56-1467">A *default_value_expression* is a constant expression ([Constant expressions](expressions.md#constant-expressions)) if the type is a reference type or a type parameter that is known to be a reference type ([Type parameter constraints](classes.md#type-parameter-constraints)).</span></span> <span data-ttu-id="dec56-1468">Kromě toho *default_value_expression* je konstantní výraz, pokud je typ některý z následujících typů hodnot: `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, `decimal`, `bool`, nebo libovolný typ výčtu.</span><span class="sxs-lookup"><span data-stu-id="dec56-1468">In addition, a *default_value_expression* is a constant expression if the type is one of the following value types: `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, `decimal`, `bool`, or any enumeration type.</span></span>


### <a name="nameof-expressions"></a><span data-ttu-id="dec56-1469">Výrazy Nameof</span><span class="sxs-lookup"><span data-stu-id="dec56-1469">Nameof expressions</span></span>

<span data-ttu-id="dec56-1470">A *nameof_expression* slouží k získání název programu entity jako konstanty typu řetězec.</span><span class="sxs-lookup"><span data-stu-id="dec56-1470">A *nameof_expression* is used to obtain the name of a program entity as a constant string.</span></span>

```antlr
nameof_expression
    : 'nameof' '(' named_entity ')'
    ;

named_entity
    : simple_name
    | named_entity_target '.' identifier type_argument_list?
    ;

named_entity_target
    : 'this'
    | 'base'
    | named_entity 
    | predefined_type 
    | qualified_alias_member
    ;
```

<span data-ttu-id="dec56-1471">Gramaticky vzato *named_entity* operand je vždy výraz.</span><span class="sxs-lookup"><span data-stu-id="dec56-1471">Grammatically speaking, the *named_entity* operand is always an expression.</span></span> <span data-ttu-id="dec56-1472">Protože `nameof` není rezervované klíčové slovo, výraz nameof je vždy syntakticky dvojznačný pomocí volání jednoduchý název `nameof`.</span><span class="sxs-lookup"><span data-stu-id="dec56-1472">Because `nameof` is not a reserved keyword, a nameof expression is always syntactically ambiguous with an invocation of the simple name `nameof`.</span></span> <span data-ttu-id="dec56-1473">Kvůli kompatibilitě, pokud vyhledávání názvu ([jednoduché názvy](expressions.md#simple-names)) názvu `nameof` proběhne úspěšně, je považován za výraz *invocation_expression* – bez ohledu na to, zda je při vyvolání právní.</span><span class="sxs-lookup"><span data-stu-id="dec56-1473">For compatibility reasons, if a name lookup ([Simple names](expressions.md#simple-names)) of the name `nameof` succeeds, the expression is treated as an *invocation_expression* -- regardless of whether the invocation is legal.</span></span> <span data-ttu-id="dec56-1474">V opačném případě se jedná *nameof_expression*.</span><span class="sxs-lookup"><span data-stu-id="dec56-1474">Otherwise it is a *nameof_expression*.</span></span>

<span data-ttu-id="dec56-1475">Význam *named_entity* z *nameof_expression* znamená ho jako výraz; to znamená, buď jako *simple_name*, *base_access*  nebo *member_access*.</span><span class="sxs-lookup"><span data-stu-id="dec56-1475">The meaning of the *named_entity* of a *nameof_expression* is the meaning of it as an expression; that is, either as a *simple_name*, a *base_access* or a *member_access*.</span></span> <span data-ttu-id="dec56-1476">Nicméně pokud vyhledávání je popsáno v [jednoduché názvy](expressions.md#simple-names) a [přístup ke členu](expressions.md#member-access) způsobí chybu, protože člen instance nebyla nalezena ve statickém kontextu *nameof_expression*vytváří žádné takové chyby.</span><span class="sxs-lookup"><span data-stu-id="dec56-1476">However, where the lookup described in [Simple names](expressions.md#simple-names) and [Member access](expressions.md#member-access) results in an error because an instance member was found in a static context, a *nameof_expression* produces no such error.</span></span>

<span data-ttu-id="dec56-1477">Je chyba kompilace pro *named_entity* určit skupinu metod. Chcete-li mít *type_argument_list*.</span><span class="sxs-lookup"><span data-stu-id="dec56-1477">It is a compile-time error for a *named_entity* designating a method group to have a *type_argument_list*.</span></span> <span data-ttu-id="dec56-1478">Jedná se chybu čas kompilace *named_entity_target* mít typ `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="dec56-1478">It is a compile time error for a *named_entity_target* to have the type `dynamic`.</span></span>

<span data-ttu-id="dec56-1479">A *nameof_expression* je konstantní výraz typu `string`, a nemá žádný vliv za běhu.</span><span class="sxs-lookup"><span data-stu-id="dec56-1479">A *nameof_expression* is a constant expression of type `string`, and has no effect at runtime.</span></span> <span data-ttu-id="dec56-1480">Konkrétně jeho *named_entity* , není hodnocena a je ignorován pro účely analýzy jednoznačného přiřazení ([obecná pravidla pro jednoduché výrazy](variables.md#general-rules-for-simple-expressions)).</span><span class="sxs-lookup"><span data-stu-id="dec56-1480">Specifically, its *named_entity* is not evaluated, and is ignored for the purposes of definite assignment analysis ([General rules for simple expressions](variables.md#general-rules-for-simple-expressions)).</span></span> <span data-ttu-id="dec56-1481">Její hodnota je poslední identifikátor *named_entity* před volitelné koncový *type_argument_list*, transformovaný následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="dec56-1481">Its value is the last identifier of the *named_entity* before the optional final *type_argument_list*, transformed in the following way:</span></span>

* <span data-ttu-id="dec56-1482">Předpona "`@`", pokud použijete, se odebere.</span><span class="sxs-lookup"><span data-stu-id="dec56-1482">The prefix "`@`", if used, is removed.</span></span>
* <span data-ttu-id="dec56-1483">Každý *unicode_escape_sequence* se transformuje na jeho odpovídající znak Unicode.</span><span class="sxs-lookup"><span data-stu-id="dec56-1483">Each *unicode_escape_sequence* is transformed into its corresponding Unicode character.</span></span>
* <span data-ttu-id="dec56-1484">Žádné *formatting_characters* se odeberou.</span><span class="sxs-lookup"><span data-stu-id="dec56-1484">Any *formatting_characters* are removed.</span></span>

<span data-ttu-id="dec56-1485">Jedná se o stejné transformace použita v [identifikátory](lexical-structure.md#identifiers) při testování rovnosti mezi identifikátory.</span><span class="sxs-lookup"><span data-stu-id="dec56-1485">These are the same transformations applied in [Identifiers](lexical-structure.md#identifiers) when testing equality between identifiers.</span></span>

<span data-ttu-id="dec56-1486">TODO: Příklady</span><span class="sxs-lookup"><span data-stu-id="dec56-1486">TODO: examples</span></span>

### <a name="anonymous-method-expressions"></a><span data-ttu-id="dec56-1487">Výrazy anonymní metoda</span><span class="sxs-lookup"><span data-stu-id="dec56-1487">Anonymous method expressions</span></span>

<span data-ttu-id="dec56-1488">*Anonymous_method_expression* je jedním ze dvou způsobů definování anonymní funkce.</span><span class="sxs-lookup"><span data-stu-id="dec56-1488">An *anonymous_method_expression* is one of two ways of defining an anonymous function.</span></span> <span data-ttu-id="dec56-1489">Ty jsou podrobně popsány v [výrazy anonymní funkce](expressions.md#anonymous-function-expressions).</span><span class="sxs-lookup"><span data-stu-id="dec56-1489">These are further described in [Anonymous function expressions](expressions.md#anonymous-function-expressions).</span></span>

## <a name="unary-operators"></a><span data-ttu-id="dec56-1490">Unární operátory</span><span class="sxs-lookup"><span data-stu-id="dec56-1490">Unary operators</span></span>

<span data-ttu-id="dec56-1491">`?`, `+`, `-`, `!`, `~`, `++`, `--`, Vícesměrového vysílání, a `await` operátory označují jako unární operátory.</span><span class="sxs-lookup"><span data-stu-id="dec56-1491">The `?`, `+`, `-`, `!`, `~`, `++`, `--`, cast, and `await` operators are called the unary operators.</span></span>

```antlr
unary_expression
    : primary_expression
    | null_conditional_expression
    | '+' unary_expression
    | '-' unary_expression
    | '!' unary_expression
    | '~' unary_expression
    | pre_increment_expression
    | pre_decrement_expression
    | cast_expression
    | await_expression
    | unary_expression_unsafe
    ;
```

<span data-ttu-id="dec56-1492">Pokud operand *unary_expression* má typ kompilace `dynamic`, je vázán dynamicky ([dynamické vazby](expressions.md#dynamic-binding)).</span><span class="sxs-lookup"><span data-stu-id="dec56-1492">If the operand of a *unary_expression* has the compile-time type `dynamic`, it is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)).</span></span> <span data-ttu-id="dec56-1493">V tomto případě typ době kompilace *unary_expression* je `dynamic`, a rozlišení je popsáno níže se provede v době běhu pomocí run-time typu operandu.</span><span class="sxs-lookup"><span data-stu-id="dec56-1493">In this case the compile-time type of the *unary_expression* is `dynamic`, and the resolution described below will take place at run-time using the run-time type of the operand.</span></span>

### <a name="null-conditional-operator"></a><span data-ttu-id="dec56-1494">Null – podmíněný operátor</span><span class="sxs-lookup"><span data-stu-id="dec56-1494">Null-conditional operator</span></span>

<span data-ttu-id="dec56-1495">Operátor podmíněného null seznam operací platí pro operand pouze v případě, že operand je jiná než null.</span><span class="sxs-lookup"><span data-stu-id="dec56-1495">The null-conditional operator applies a list of operations to its operand only if that operand is non-null.</span></span> <span data-ttu-id="dec56-1496">Jinak je výsledek použití operátoru `null`.</span><span class="sxs-lookup"><span data-stu-id="dec56-1496">Otherwise the result of applying the operator is `null`.</span></span>

```antlr
null_conditional_expression
    : primary_expression null_conditional_operations
    ;

null_conditional_operations
    : null_conditional_operations? '?' '.' identifier type_argument_list?
    | null_conditional_operations? '?' '[' argument_list ']'
    | null_conditional_operations '.' identifier type_argument_list?
    | null_conditional_operations '[' argument_list ']'
    | null_conditional_operations '(' argument_list? ')'
    ;
```

<span data-ttu-id="dec56-1497">Přístup ke členu a operací přístupu k elementu, (které mohou být samy podmíněných null), jakož i volání může obsahovat seznam operací.</span><span class="sxs-lookup"><span data-stu-id="dec56-1497">The list of operations can include member access and element access operations (which may themselves be null-conditional), as well as invocation.</span></span>

<span data-ttu-id="dec56-1498">Například výraz `a.b?[0]?.c()` je *null_conditional_expression* s *primary_expression* `a.b` a *null_conditional_operations* `?[0]` (přístup k prvkům podmíněných null), `?.c` (člen null podmíněný přístup) a `()` (vyvolání).</span><span class="sxs-lookup"><span data-stu-id="dec56-1498">For example, the expression `a.b?[0]?.c()` is a *null_conditional_expression* with a *primary_expression* `a.b` and *null_conditional_operations* `?[0]` (null-conditional element access), `?.c` (null-conditional member access) and `()` (invocation).</span></span>

<span data-ttu-id="dec56-1499">Pro *null_conditional_expression* `E` s *primary_expression* `P`, umožněte `E0` se výraz získaný pomocí textu odstraněním úvodního `?`ze všech *null_conditional_operations* z `E` , které mají jednu.</span><span class="sxs-lookup"><span data-stu-id="dec56-1499">For a *null_conditional_expression* `E` with a *primary_expression* `P`, let `E0` be the expression obtained by textually removing the leading `?` from each of the *null_conditional_operations* of `E` that have one.</span></span> <span data-ttu-id="dec56-1500">Koncepčně `E0` je výraz, který bude vyhodnocen, pokud žádná z kontroly hodnoty null reprezentována `?`s najít `null`.</span><span class="sxs-lookup"><span data-stu-id="dec56-1500">Conceptually, `E0` is the expression that will be evaluated if none of the null checks represented by the `?`s do find a `null`.</span></span>

<span data-ttu-id="dec56-1501">Navíc umožňují `E1` se výraz získaný pomocí textu odstraněním úvodního `?` z jenom na prvního *null_conditional_operations* v `E`.</span><span class="sxs-lookup"><span data-stu-id="dec56-1501">Also, let `E1` be the expression obtained by textually removing the leading `?` from just the first of the *null_conditional_operations* in `E`.</span></span> <span data-ttu-id="dec56-1502">To může vést k *primární výraz* (pokud existuje pouze jedna `?`) nebo do jiného *null_conditional_expression*.</span><span class="sxs-lookup"><span data-stu-id="dec56-1502">This may lead to a *primary-expression* (if there was just one `?`) or to another *null_conditional_expression*.</span></span>

<span data-ttu-id="dec56-1503">Například pokud `E` je výraz `a.b?[0]?.c()`, pak `E0` je výraz `a.b[0].c()` a `E1` je výraz `a.b[0]?.c()`.</span><span class="sxs-lookup"><span data-stu-id="dec56-1503">For example, if `E` is the expression `a.b?[0]?.c()`, then `E0` is the expression `a.b[0].c()` and `E1` is the expression `a.b[0]?.c()`.</span></span>

<span data-ttu-id="dec56-1504">Pokud `E0` klasifikovaný jako nothing, pak `E` klasifikovaný jako nothing.</span><span class="sxs-lookup"><span data-stu-id="dec56-1504">If `E0` is classified as nothing, then `E` is classified as nothing.</span></span> <span data-ttu-id="dec56-1505">V opačném případě E je klasifikován jako hodnotu.</span><span class="sxs-lookup"><span data-stu-id="dec56-1505">Otherwise E is classified as a value.</span></span>

<span data-ttu-id="dec56-1506">`E0` a `E1` slouží k určení význam `E`:</span><span class="sxs-lookup"><span data-stu-id="dec56-1506">`E0` and `E1` are used to determine the meaning of `E`:</span></span>

*  <span data-ttu-id="dec56-1507">Pokud `E` vyskytuje se jako *statement_expression* význam `E` je stejný jako příkaz</span><span class="sxs-lookup"><span data-stu-id="dec56-1507">If `E` occurs as a *statement_expression* the meaning of `E` is the same as the statement</span></span>

   ```csharp
   if ((object)P != null) E1;
   ```

   <span data-ttu-id="dec56-1508">s tím rozdílem, že P je vyhodnocen pouze jednou.</span><span class="sxs-lookup"><span data-stu-id="dec56-1508">except that P is evaluated only once.</span></span>

*  <span data-ttu-id="dec56-1509">Jinak, pokud `E0` je klasifikován tak, co dojde k chybě kompilace.</span><span class="sxs-lookup"><span data-stu-id="dec56-1509">Otherwise, if `E0` is classified as nothing a compile-time error occurs.</span></span>

*  <span data-ttu-id="dec56-1510">V opačném případě nechat `T0` být typu `E0`.</span><span class="sxs-lookup"><span data-stu-id="dec56-1510">Otherwise, let `T0` be the type of `E0`.</span></span>

   *  <span data-ttu-id="dec56-1511">Pokud `T0` je parametr typu, který není známé jako odkaz na typ nebo hodnotu Null, dojde k chybě kompilace.</span><span class="sxs-lookup"><span data-stu-id="dec56-1511">If `T0` is a type parameter that is not known to be a reference type or a non-nullable value type, a compile-time error occurs.</span></span>

   *  <span data-ttu-id="dec56-1512">Pokud `T0` typu hodnotu Null, je typu `E` je `T0?`a význam `E` je stejný jako</span><span class="sxs-lookup"><span data-stu-id="dec56-1512">If `T0` is a non-nullable value type, then the type of `E` is `T0?`, and the meaning of `E` is the same as</span></span>

      ```csharp
      ((object)P == null) ? (T0?)null : E1
      ```

      <span data-ttu-id="dec56-1513">s tím rozdílem, že `P` se vyhodnotí pouze jednou.</span><span class="sxs-lookup"><span data-stu-id="dec56-1513">except that `P` is evaluated only once.</span></span>

   *  <span data-ttu-id="dec56-1514">V opačném případě je T0 typu E a význam E je stejný jako</span><span class="sxs-lookup"><span data-stu-id="dec56-1514">Otherwise the type of E is T0, and the meaning of E is the same as</span></span>

      ```csharp
      ((object)P == null) ? null : E1
      ```

      <span data-ttu-id="dec56-1515">s tím rozdílem, že `P` se vyhodnotí pouze jednou.</span><span class="sxs-lookup"><span data-stu-id="dec56-1515">except that `P` is evaluated only once.</span></span>

<span data-ttu-id="dec56-1516">Pokud `E1` sama o sobě *null_conditional_expression*, pak tato pravidla se použijí znovu, vnoření testy pro `null` až nebudou existovat žádné další `?`společnosti, a zmenšili jsme výraz úplně dolů primární výraz `E0`.</span><span class="sxs-lookup"><span data-stu-id="dec56-1516">If `E1` is itself a *null_conditional_expression*, then these rules are applied again, nesting the tests for `null` until there are no further `?`'s, and the expression has been reduced all the way down to the primary-expression `E0`.</span></span>

<span data-ttu-id="dec56-1517">Například pokud výraz `a.b?[0]?.c()` vyskytuje se jako příkaz výraz, stejně jako v příkazu:</span><span class="sxs-lookup"><span data-stu-id="dec56-1517">For example, if the expression `a.b?[0]?.c()` occurs as a statement-expression, as in the statement:</span></span>
```csharp
a.b?[0]?.c();
```
<span data-ttu-id="dec56-1518">je ekvivalentní k jeho význam:</span><span class="sxs-lookup"><span data-stu-id="dec56-1518">its meaning is equivalent to:</span></span>
```csharp
if (a.b != null) a.b[0]?.c();
```
<span data-ttu-id="dec56-1519">což znovu je totéž jako:</span><span class="sxs-lookup"><span data-stu-id="dec56-1519">which again is equivalent to:</span></span>
```csharp
if (a.b != null) if (a.b[0] != null) a.b[0].c();
```
<span data-ttu-id="dec56-1520">S tím rozdílem, že `a.b` a `a.b[0]` se vyhodnocují jenom jednou.</span><span class="sxs-lookup"><span data-stu-id="dec56-1520">Except that `a.b` and `a.b[0]` are evaluated only once.</span></span>

<span data-ttu-id="dec56-1521">Pokud k němu dojde v kontextu, pokud jeho hodnota se používá, například:</span><span class="sxs-lookup"><span data-stu-id="dec56-1521">If it occurs in a context where its value is used, as in:</span></span>
```csharp
var x = a.b?[0]?.c();
```
<span data-ttu-id="dec56-1522">a za předpokladu, že typ posledním volání není typu hodnotu Null, je ekvivalentní k jeho význam:</span><span class="sxs-lookup"><span data-stu-id="dec56-1522">and assuming that the type of the final invocation is not a non-nullable value type, its meaning is equivalent to:</span></span>
```csharp
var x = (a.b == null) ? null : (a.b[0] == null) ? null : a.b[0].c();
```
<span data-ttu-id="dec56-1523">S tím rozdílem, že `a.b` a `a.b[0]` se vyhodnocují jenom jednou.</span><span class="sxs-lookup"><span data-stu-id="dec56-1523">except that `a.b` and `a.b[0]` are evaluated only once.</span></span>

#### <a name="null-conditional-expressions-as-projection-initializers"></a><span data-ttu-id="dec56-1524">Null podmíněné výrazy jako inicializátory projekce</span><span class="sxs-lookup"><span data-stu-id="dec56-1524">Null-conditional expressions as projection initializers</span></span>

<span data-ttu-id="dec56-1525">Null – podmíněný výraz je povolen pouze jako *member_declarator* v *anonymous_object_creation_expression* ([anonymní objekt vytváření výrazů](expressions.md#anonymous-object-creation-expressions)) Pokud končí přístup ke členu (volitelně null podmíněné).</span><span class="sxs-lookup"><span data-stu-id="dec56-1525">A null-conditional expression is only allowed as a *member_declarator* in an *anonymous_object_creation_expression* ([Anonymous object creation expressions](expressions.md#anonymous-object-creation-expressions)) if it ends with an (optionally null-conditional) member access.</span></span> <span data-ttu-id="dec56-1526">Tento požadavek gramaticky, může být vyjádřený jako:</span><span class="sxs-lookup"><span data-stu-id="dec56-1526">Grammatically, this requirement can be expressed as:</span></span>

```antlr
null_conditional_member_access
    : primary_expression null_conditional_operations? '?' '.' identifier type_argument_list?
    | primary_expression null_conditional_operations '.' identifier type_argument_list?
    ;
```

<span data-ttu-id="dec56-1527">Toto je zvláštní případ gramatiky *null_conditional_expression* výše.</span><span class="sxs-lookup"><span data-stu-id="dec56-1527">This is a special case of the grammar for *null_conditional_expression* above.</span></span> <span data-ttu-id="dec56-1528">Produkčních prostředích *member_declarator* v [anonymní objekt vytváření výrazů](expressions.md#anonymous-object-creation-expressions) zahrnuje pouze *null_conditional_member_access*.</span><span class="sxs-lookup"><span data-stu-id="dec56-1528">The production for *member_declarator* in [Anonymous object creation expressions](expressions.md#anonymous-object-creation-expressions) then includes only *null_conditional_member_access*.</span></span>

#### <a name="null-conditional-expressions-as-statement-expressions"></a><span data-ttu-id="dec56-1529">Null – podmíněné výrazy jako výrazy příkazu</span><span class="sxs-lookup"><span data-stu-id="dec56-1529">Null-conditional expressions as statement expressions</span></span>

<span data-ttu-id="dec56-1530">Null – podmíněný výraz je povolen pouze jako *statement_expression* ([příkazy výrazů](statements.md#expression-statements)) Pokud končí vyvolání.</span><span class="sxs-lookup"><span data-stu-id="dec56-1530">A null-conditional expression is only allowed as a *statement_expression* ([Expression statements](statements.md#expression-statements)) if it ends with an invocation.</span></span> <span data-ttu-id="dec56-1531">Tento požadavek gramaticky, může být vyjádřený jako:</span><span class="sxs-lookup"><span data-stu-id="dec56-1531">Grammatically, this requirement can be expressed as:</span></span>

```antlr
null_conditional_invocation_expression
    : primary_expression null_conditional_operations '(' argument_list? ')'
    ;
```

<span data-ttu-id="dec56-1532">Toto je zvláštní případ gramatiky *null_conditional_expression* výše.</span><span class="sxs-lookup"><span data-stu-id="dec56-1532">This is a special case of the grammar for *null_conditional_expression* above.</span></span> <span data-ttu-id="dec56-1533">Produkčních prostředích *statement_expression* v [příkazy výrazů](statements.md#expression-statements) zahrnuje pouze *null_conditional_invocation_expression*.</span><span class="sxs-lookup"><span data-stu-id="dec56-1533">The production for *statement_expression* in [Expression statements](statements.md#expression-statements) then includes only *null_conditional_invocation_expression*.</span></span>


### <a name="unary-plus-operator"></a><span data-ttu-id="dec56-1534">Jednočlenný operátor plus</span><span class="sxs-lookup"><span data-stu-id="dec56-1534">Unary plus operator</span></span>

<span data-ttu-id="dec56-1535">Operace formuláře `+x`, unárního operátoru rozlišení přetěžování ([unární operátor rozlišení přetěžování](expressions.md#unary-operator-overload-resolution)) se použije k výběru na konkrétní operátor implementace.</span><span class="sxs-lookup"><span data-stu-id="dec56-1535">For an operation of the form `+x`, unary operator overload resolution ([Unary operator overload resolution](expressions.md#unary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="dec56-1536">Operand je převeden na typ parametru zvoleném operátorovi a typ výsledku je návratový typ operátoru.</span><span class="sxs-lookup"><span data-stu-id="dec56-1536">The operand is converted to the parameter type of the selected operator, and the type of the result is the return type of the operator.</span></span> <span data-ttu-id="dec56-1537">Předdefinované Unární plus operátory jsou:</span><span class="sxs-lookup"><span data-stu-id="dec56-1537">The predefined unary plus operators are:</span></span>

```csharp
int operator +(int x);
uint operator +(uint x);
long operator +(long x);
ulong operator +(ulong x);
float operator +(float x);
double operator +(double x);
decimal operator +(decimal x);
```

<span data-ttu-id="dec56-1538">Pro každý z těchto operátorů výsledkem je jednoduše hodnota operandu.</span><span class="sxs-lookup"><span data-stu-id="dec56-1538">For each of these operators, the result is simply the value of the operand.</span></span>

### <a name="unary-minus-operator"></a><span data-ttu-id="dec56-1539">Unární operátor minus</span><span class="sxs-lookup"><span data-stu-id="dec56-1539">Unary minus operator</span></span>

<span data-ttu-id="dec56-1540">Operace formuláře `-x`, unárního operátoru rozlišení přetěžování ([unární operátor rozlišení přetěžování](expressions.md#unary-operator-overload-resolution)) se použije k výběru na konkrétní operátor implementace.</span><span class="sxs-lookup"><span data-stu-id="dec56-1540">For an operation of the form `-x`, unary operator overload resolution ([Unary operator overload resolution](expressions.md#unary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="dec56-1541">Operand je převeden na typ parametru zvoleném operátorovi a typ výsledku je návratový typ operátoru.</span><span class="sxs-lookup"><span data-stu-id="dec56-1541">The operand is converted to the parameter type of the selected operator, and the type of the result is the return type of the operator.</span></span> <span data-ttu-id="dec56-1542">Předdefinované negace operátory jsou:</span><span class="sxs-lookup"><span data-stu-id="dec56-1542">The predefined negation operators are:</span></span>

*  <span data-ttu-id="dec56-1543">Celé číslo negace:</span><span class="sxs-lookup"><span data-stu-id="dec56-1543">Integer negation:</span></span>

   ```csharp
   int operator -(int x);
   long operator -(long x);
   ```

   <span data-ttu-id="dec56-1544">Výsledkem je vypočítán odečtením `x` od nuly.</span><span class="sxs-lookup"><span data-stu-id="dec56-1544">The result is computed by subtracting `x` from zero.</span></span> <span data-ttu-id="dec56-1545">Pokud hodnota `x` je nejmenší reprezentovatelnou hodnotu typu operandu (-2 ^ 31 pro `int` nebo -2 ^ 63 pro `long`), pak matematické negaci `x` není reprezentovatelná v typu operandu.</span><span class="sxs-lookup"><span data-stu-id="dec56-1545">If the value of `x` is the smallest representable value of the operand type (-2^31 for `int` or -2^63 for `long`), then the mathematical negation of `x` is not representable within the operand type.</span></span> <span data-ttu-id="dec56-1546">Pokud k tomu dojde v rámci `checked` kontextu, `System.OverflowException` je vyvolána, pokud se vyskytuje v `unchecked` kontextu, výsledkem je hodnota operandu a není hlášena přetečení.</span><span class="sxs-lookup"><span data-stu-id="dec56-1546">If this occurs within a `checked` context, a `System.OverflowException` is thrown; if it occurs within an `unchecked` context, the result is the value of the operand and the overflow is not reported.</span></span>

   <span data-ttu-id="dec56-1547">Pokud je operand operátoru negace typu `uint`, je převeden na typ `long`, a typ výsledku je `long`.</span><span class="sxs-lookup"><span data-stu-id="dec56-1547">If the operand of the negation operator is of type `uint`, it is converted to type `long`, and the type of the result is `long`.</span></span> <span data-ttu-id="dec56-1548">Výjimkou je pravidlo, které povoluje `int` hodnota -2147483648 (-2 ^ 31) má být zapsán jako desítkové celé číslo literálu ([literály celých čísel](lexical-structure.md#integer-literals)).</span><span class="sxs-lookup"><span data-stu-id="dec56-1548">An exception is the rule that permits the `int` value -2147483648 (-2^31) to be written as a decimal integer literal ([Integer literals](lexical-structure.md#integer-literals)).</span></span>

   <span data-ttu-id="dec56-1549">Pokud je operand operátoru negace typu `ulong`, dojde k chybě kompilace.</span><span class="sxs-lookup"><span data-stu-id="dec56-1549">If the operand of the negation operator is of type `ulong`, a compile-time error occurs.</span></span> <span data-ttu-id="dec56-1550">Výjimkou je pravidlo, které povoluje `long` hodnota -9223372036854775808 (-2 ^ 63) má být zapsán jako desítkové celé číslo literálu ([literály celých čísel](lexical-structure.md#integer-literals)).</span><span class="sxs-lookup"><span data-stu-id="dec56-1550">An exception is the rule that permits the `long` value -9223372036854775808 (-2^63) to be written as a decimal integer literal ([Integer literals](lexical-structure.md#integer-literals)).</span></span>

*  <span data-ttu-id="dec56-1551">S plovoucí desetinnou čárkou negace:</span><span class="sxs-lookup"><span data-stu-id="dec56-1551">Floating-point negation:</span></span>

   ```csharp
   float operator -(float x);
   double operator -(double x);
   ```

   <span data-ttu-id="dec56-1552">Výsledkem je hodnota `x` jeho symbolem obrácený.</span><span class="sxs-lookup"><span data-stu-id="dec56-1552">The result is the value of `x` with its sign inverted.</span></span> <span data-ttu-id="dec56-1553">Pokud `x` NaN, je vrácená hodnota je také NaN.</span><span class="sxs-lookup"><span data-stu-id="dec56-1553">If `x` is NaN, the result is also NaN.</span></span>

*  <span data-ttu-id="dec56-1554">Desetinné negace:</span><span class="sxs-lookup"><span data-stu-id="dec56-1554">Decimal negation:</span></span>

   ```csharp
   decimal operator -(decimal x);
   ```

   <span data-ttu-id="dec56-1555">Výsledkem je vypočítán odečtením `x` od nuly.</span><span class="sxs-lookup"><span data-stu-id="dec56-1555">The result is computed by subtracting `x` from zero.</span></span> <span data-ttu-id="dec56-1556">Je ekvivalentní k použití unární mínus – operátor typu Decimal negace `System.Decimal`.</span><span class="sxs-lookup"><span data-stu-id="dec56-1556">Decimal negation is equivalent to using the unary minus operator of type `System.Decimal`.</span></span>

### <a name="logical-negation-operator"></a><span data-ttu-id="dec56-1557">Logický operátor negace</span><span class="sxs-lookup"><span data-stu-id="dec56-1557">Logical negation operator</span></span>

<span data-ttu-id="dec56-1558">Operace formuláře `!x`, unárního operátoru rozlišení přetěžování ([unární operátor rozlišení přetěžování](expressions.md#unary-operator-overload-resolution)) se použije k výběru na konkrétní operátor implementace.</span><span class="sxs-lookup"><span data-stu-id="dec56-1558">For an operation of the form `!x`, unary operator overload resolution ([Unary operator overload resolution](expressions.md#unary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="dec56-1559">Operand je převeden na typ parametru zvoleném operátorovi a typ výsledku je návratový typ operátoru.</span><span class="sxs-lookup"><span data-stu-id="dec56-1559">The operand is converted to the parameter type of the selected operator, and the type of the result is the return type of the operator.</span></span> <span data-ttu-id="dec56-1560">Existuje pouze jeden předdefinovaný logický operátor negace:</span><span class="sxs-lookup"><span data-stu-id="dec56-1560">Only one predefined logical negation operator exists:</span></span>
```csharp
bool operator !(bool x);
```

<span data-ttu-id="dec56-1561">Tento operátor vypočítá Logická negace operand: Pokud je operand `true`, výsledkem je `false`.</span><span class="sxs-lookup"><span data-stu-id="dec56-1561">This operator computes the logical negation of the operand: If the operand is `true`, the result is `false`.</span></span> <span data-ttu-id="dec56-1562">Pokud je operand `false`, výsledkem je `true`.</span><span class="sxs-lookup"><span data-stu-id="dec56-1562">If the operand is `false`, the result is `true`.</span></span>

### <a name="bitwise-complement-operator"></a><span data-ttu-id="dec56-1563">Operátor bitového doplňku</span><span class="sxs-lookup"><span data-stu-id="dec56-1563">Bitwise complement operator</span></span>

<span data-ttu-id="dec56-1564">Operace formuláře `~x`, unárního operátoru rozlišení přetěžování ([unární operátor rozlišení přetěžování](expressions.md#unary-operator-overload-resolution)) se použije k výběru na konkrétní operátor implementace.</span><span class="sxs-lookup"><span data-stu-id="dec56-1564">For an operation of the form `~x`, unary operator overload resolution ([Unary operator overload resolution](expressions.md#unary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="dec56-1565">Operand je převeden na typ parametru zvoleném operátorovi a typ výsledku je návratový typ operátoru.</span><span class="sxs-lookup"><span data-stu-id="dec56-1565">The operand is converted to the parameter type of the selected operator, and the type of the result is the return type of the operator.</span></span> <span data-ttu-id="dec56-1566">Předdefinované bitového doplňku operátory jsou:</span><span class="sxs-lookup"><span data-stu-id="dec56-1566">The predefined bitwise complement operators are:</span></span>
```csharp
int operator ~(int x);
uint operator ~(uint x);
long operator ~(long x);
ulong operator ~(ulong x);
```

<span data-ttu-id="dec56-1567">Pro každý z těchto operátorů je výsledek operace bitový doplněk `x`.</span><span class="sxs-lookup"><span data-stu-id="dec56-1567">For each of these operators, the result of the operation is the bitwise complement of `x`.</span></span>

<span data-ttu-id="dec56-1568">Každý typ výčtu `E` implicitně poskytuje následující operátor bitového doplňku:</span><span class="sxs-lookup"><span data-stu-id="dec56-1568">Every enumeration type `E` implicitly provides the following bitwise complement operator:</span></span>

```csharp
E operator ~(E x);
```

<span data-ttu-id="dec56-1569">Výsledek vyhodnocení výrazu `~x`, kde `x` je výraz typu výčtu `E` s podkladovým typem `U`, je stejný jako vaše rozhodnutí vyzkoušet `(E)(~(U)x)`, s tím rozdílem, že převod `E` je vždy provést, protože pokud v `unchecked` kontextu ([operátory zaškrtnuto a nezaškrtnuto](expressions.md#the-checked-and-unchecked-operators)).</span><span class="sxs-lookup"><span data-stu-id="dec56-1569">The result of evaluating `~x`, where `x` is an expression of an enumeration type `E` with an underlying type `U`, is exactly the same as evaluating `(E)(~(U)x)`, except that the conversion to `E` is always performed as if in an `unchecked` context ([The checked and unchecked operators](expressions.md#the-checked-and-unchecked-operators)).</span></span>

### <a name="prefix-increment-and-decrement-operators"></a><span data-ttu-id="dec56-1570">Předpona Inkrementace a dekrementace operátory</span><span class="sxs-lookup"><span data-stu-id="dec56-1570">Prefix increment and decrement operators</span></span>

```antlr
pre_increment_expression
    : '++' unary_expression
    ;

pre_decrement_expression
    : '--' unary_expression
    ;
```

<span data-ttu-id="dec56-1571">Operand operace předponu zvýšení nebo snížení musí být výraz jsou klasifikovány jako proměnné, přístup k vlastnosti nebo indexeru přístupu.</span><span class="sxs-lookup"><span data-stu-id="dec56-1571">The operand of a prefix increment or decrement operation must be an expression classified as a variable, a property access, or an indexer access.</span></span> <span data-ttu-id="dec56-1572">Výsledkem operace je hodnota stejného typu jako operand.</span><span class="sxs-lookup"><span data-stu-id="dec56-1572">The result of the operation is a value of the same type as the operand.</span></span>

<span data-ttu-id="dec56-1573">Je-li zvýšit operand předponu operace dekrementace je vlastnost nebo indexer přístup, vlastnost nebo indexer musí mít obě `get` a `set` přistupujícího objektu.</span><span class="sxs-lookup"><span data-stu-id="dec56-1573">If the operand of a prefix increment or decrement operation is a property or indexer access, the property or indexer must have both a `get` and a `set` accessor.</span></span> <span data-ttu-id="dec56-1574">Pokud to není tento případ, dojde k chybě vazby čas.</span><span class="sxs-lookup"><span data-stu-id="dec56-1574">If this is not the case, a binding-time error occurs.</span></span>

<span data-ttu-id="dec56-1575">Unární operátor rozlišení přetěžování ([unární operátor rozlišení přetěžování](expressions.md#unary-operator-overload-resolution)) se použije k výběru na konkrétní operátor implementace.</span><span class="sxs-lookup"><span data-stu-id="dec56-1575">Unary operator overload resolution ([Unary operator overload resolution](expressions.md#unary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="dec56-1576">Předdefinované `++` a `--` operátory existovat pro následující typy: `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char` , `float`, `double`, `decimal`a jakýkoli typ výčtu.</span><span class="sxs-lookup"><span data-stu-id="dec56-1576">Predefined `++` and `--` operators exist for the following types: `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, `decimal`, and any enum type.</span></span> <span data-ttu-id="dec56-1577">Předdefinované `++` operátory návratová hodnota vytváří přidáním 1 operand a předdefinované `--` operátory návratová hodnota vytváří tak, že se 1 z operand.</span><span class="sxs-lookup"><span data-stu-id="dec56-1577">The predefined `++` operators return the value produced by adding 1 to the operand, and the predefined `--` operators return the value produced by subtracting 1 from the operand.</span></span> <span data-ttu-id="dec56-1578">V `checked` kontextu, pokud výsledek této sčítání a odčítání je mimo rozsah typu výsledku a typ výsledku je integrální typ nebo typ výčtu `System.OverflowException` je vyvolána výjimka.</span><span class="sxs-lookup"><span data-stu-id="dec56-1578">In a `checked` context, if the result of this addition or subtraction is outside the range of the result type and the result type is an integral type or enum type, a `System.OverflowException` is thrown.</span></span>

<span data-ttu-id="dec56-1579">Zpracování za běhu předponu Inkrementace nebo dekrementace operace formuláře `++x` nebo `--x` se skládá z následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="dec56-1579">The run-time processing of a prefix increment or decrement operation of the form `++x` or `--x` consists of the following steps:</span></span>

*   <span data-ttu-id="dec56-1580">Pokud `x` je klasifikován jako proměnnou:</span><span class="sxs-lookup"><span data-stu-id="dec56-1580">If `x` is classified as a variable:</span></span>
    * <span data-ttu-id="dec56-1581">`x` je vyhodnocen pro vytvoření proměnné.</span><span class="sxs-lookup"><span data-stu-id="dec56-1581">`x` is evaluated to produce the variable.</span></span>
    * <span data-ttu-id="dec56-1582">S použitím hodnoty z je vyvolána zvoleném operátorovi `x` jako svůj argument.</span><span class="sxs-lookup"><span data-stu-id="dec56-1582">The selected operator is invoked with the value of `x` as its argument.</span></span>
    * <span data-ttu-id="dec56-1583">Hodnota vrácená operátorem je uložen v umístění Dal vyhodnocení `x`.</span><span class="sxs-lookup"><span data-stu-id="dec56-1583">The value returned by the operator is stored in the location given by the evaluation of `x`.</span></span>
    * <span data-ttu-id="dec56-1584">Hodnota vrácená operátorem změní výsledek operace.</span><span class="sxs-lookup"><span data-stu-id="dec56-1584">The value returned by the operator becomes the result of the operation.</span></span>
*   <span data-ttu-id="dec56-1585">Pokud `x` klasifikovaný jako vlastnost nebo indexovací člen přístupu:</span><span class="sxs-lookup"><span data-stu-id="dec56-1585">If `x` is classified as a property or indexer access:</span></span>
    * <span data-ttu-id="dec56-1586">Výraz instance (Pokud `x` není `static`) a v seznamu argumentů (Pokud `x` představuje přístup k indexer) přidružené k `x` jsou vyhodnocovány, a výsledky se používají v následné `get` a `set` přístupový objekt volání.</span><span class="sxs-lookup"><span data-stu-id="dec56-1586">The instance expression (if `x` is not `static`) and the argument list (if `x` is an indexer access) associated with `x` are evaluated, and the results are used in the subsequent `get` and `set` accessor invocations.</span></span>
    * <span data-ttu-id="dec56-1587">`get` Přistupující objekt `x` je vyvolána.</span><span class="sxs-lookup"><span data-stu-id="dec56-1587">The `get` accessor of `x` is invoked.</span></span>
    * <span data-ttu-id="dec56-1588">Vybraný operátor je volána s hodnotou vrácenou `get` přístupového objektu jako svůj argument.</span><span class="sxs-lookup"><span data-stu-id="dec56-1588">The selected operator is invoked with the value returned by the `get` accessor as its argument.</span></span>
    * <span data-ttu-id="dec56-1589">`set` Přistupující objekt `x` vyvolání hodnota vrácená operátorem jako jeho `value` argument.</span><span class="sxs-lookup"><span data-stu-id="dec56-1589">The `set` accessor of `x` is invoked with the value returned by the operator as its `value` argument.</span></span>
    * <span data-ttu-id="dec56-1590">Hodnota vrácená operátorem změní výsledek operace.</span><span class="sxs-lookup"><span data-stu-id="dec56-1590">The value returned by the operator becomes the result of the operation.</span></span>

<span data-ttu-id="dec56-1591">`++` a `--` také podporují Příponové operátory zápis ([Příponové operátory Inkrementace a dekrementace operátory](expressions.md#postfix-increment-and-decrement-operators)).</span><span class="sxs-lookup"><span data-stu-id="dec56-1591">The `++` and `--` operators also support postfix notation ([Postfix increment and decrement operators](expressions.md#postfix-increment-and-decrement-operators)).</span></span> <span data-ttu-id="dec56-1592">Obvykle, výsledek `x++` nebo `x--` je hodnota `x` před provedením operace, zatímco výsledek `++x` nebo `--x` je hodnota `x` po provedení této operace.</span><span class="sxs-lookup"><span data-stu-id="dec56-1592">Typically, the result of `x++` or `x--` is the value of `x` before the operation, whereas the result of `++x` or `--x` is the value of `x` after the operation.</span></span> <span data-ttu-id="dec56-1593">V obou případech `x` sám po provedení této operace má stejnou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="dec56-1593">In either case, `x` itself has the same value after the operation.</span></span>

<span data-ttu-id="dec56-1594">`operator++` Nebo `operator--` implementace lze vyvolat pomocí uplatněna přípona nebo předpona zápisu.</span><span class="sxs-lookup"><span data-stu-id="dec56-1594">An `operator++` or `operator--` implementation can be invoked using either postfix or prefix notation.</span></span> <span data-ttu-id="dec56-1595">Není možné mít samostatné operátor implementace pro zápisy dva.</span><span class="sxs-lookup"><span data-stu-id="dec56-1595">It is not possible to have separate operator implementations for the two notations.</span></span>

### <a name="cast-expressions"></a><span data-ttu-id="dec56-1596">Výrazy přetypování</span><span class="sxs-lookup"><span data-stu-id="dec56-1596">Cast expressions</span></span>

<span data-ttu-id="dec56-1597">A *cast_expression* slouží k explicitnímu převodu výrazu na daného typu.</span><span class="sxs-lookup"><span data-stu-id="dec56-1597">A *cast_expression* is used to explicitly convert an expression to a given type.</span></span>

```antlr
cast_expression
    : '(' type ')' unary_expression
    ;
```

<span data-ttu-id="dec56-1598">A *cast_expression* formuláře `(T)E`, kde `T` je *typ* a `E` je *unary_expression*, provádí explicitní převod ([explicitních převodů](conversions.md#explicit-conversions)) hodnoty `E` na typ `T`.</span><span class="sxs-lookup"><span data-stu-id="dec56-1598">A *cast_expression* of the form `(T)E`, where `T` is a *type* and `E` is a *unary_expression*, performs an explicit conversion ([Explicit conversions](conversions.md#explicit-conversions)) of the value of `E` to type `T`.</span></span> <span data-ttu-id="dec56-1599">Pokud neexistuje žádný explicitní převod z `E` k `T`, dojde k chybě vazby čas.</span><span class="sxs-lookup"><span data-stu-id="dec56-1599">If no explicit conversion exists from `E` to `T`, a binding-time error occurs.</span></span> <span data-ttu-id="dec56-1600">Jinak výsledkem je hodnota vytvářených explicitní převod.</span><span class="sxs-lookup"><span data-stu-id="dec56-1600">Otherwise, the result is the value produced by the explicit conversion.</span></span> <span data-ttu-id="dec56-1601">Výsledek je vždy klasifikován jako hodnotu, i když `E` označuje proměnnou.</span><span class="sxs-lookup"><span data-stu-id="dec56-1601">The result is always classified as a value, even if `E` denotes a variable.</span></span>

<span data-ttu-id="dec56-1602">Gramatika *cast_expression* vede k určité syntaktické nejednoznačnosti.</span><span class="sxs-lookup"><span data-stu-id="dec56-1602">The grammar for a *cast_expression* leads to certain syntactic ambiguities.</span></span> <span data-ttu-id="dec56-1603">Například výraz `(x)-y` buď možné interpretovat jako *cast_expression* (přetypování z `-y` na typ `x`) nebo jako *additive_expression* v kombinaci s *parenthesized_expression* (které vypočítá hodnotu `x - y)`.</span><span class="sxs-lookup"><span data-stu-id="dec56-1603">For example, the expression `(x)-y` could either be interpreted as a *cast_expression* (a cast of `-y` to type `x`) or as an *additive_expression* combined with a *parenthesized_expression* (which computes the value `x - y)`.</span></span>

<span data-ttu-id="dec56-1604">Chcete-li vyřešit *cast_expression* nejasnosti, existuje následující pravidlo: Pořadí jednoho nebo více *token*s ([prázdné znaky](lexical-structure.md#white-space)) uzavřený v závorkách je považován za spuštění *cast_expression* pouze v případě, že platí alespoň jedna z následujících akcí:</span><span class="sxs-lookup"><span data-stu-id="dec56-1604">To resolve *cast_expression* ambiguities, the following rule exists: A sequence of one or more *token*s ([White space](lexical-structure.md#white-space)) enclosed in parentheses is considered the start of a *cast_expression* only if at least one of the following are true:</span></span>

*  <span data-ttu-id="dec56-1605">Sekvence tokenů je správná gramatika pro *typ*, ale ne pro *výraz*.</span><span class="sxs-lookup"><span data-stu-id="dec56-1605">The sequence of tokens is correct grammar for a *type*, but not for an *expression*.</span></span>
*  <span data-ttu-id="dec56-1606">Posloupnost tokeny je správná gramatika pro *typ*, a hned za pravými závorkami token je token "`~`", token "`!`", token "`(`",  *identifikátor* ([znak – řídicí sekvence Unicode](lexical-structure.md#unicode-character-escape-sequences)), *literálu* ([literály](lexical-structure.md#literals)), nebo všechny *– klíčové slovo*([Klíčová slova](lexical-structure.md#keywords)) s výjimkou `as` a `is`.</span><span class="sxs-lookup"><span data-stu-id="dec56-1606">The sequence of tokens is correct grammar for a *type*, and the token immediately following the closing parentheses is the token "`~`", the token "`!`", the token "`(`", an *identifier* ([Unicode character escape sequences](lexical-structure.md#unicode-character-escape-sequences)), a *literal* ([Literals](lexical-structure.md#literals)), or any *keyword* ([Keywords](lexical-structure.md#keywords)) except `as` and `is`.</span></span>

<span data-ttu-id="dec56-1607">Termín "správná gramatika" nad znamená pouze to, že posloupnost tokeny musí odpovídat konkrétní gramatické produkčního prostředí.</span><span class="sxs-lookup"><span data-stu-id="dec56-1607">The term "correct grammar" above means only that the sequence of tokens must conform to the particular grammatical production.</span></span> <span data-ttu-id="dec56-1608">To konkrétně nebere v úvahu skutečné význam všechny základní identifikátory.</span><span class="sxs-lookup"><span data-stu-id="dec56-1608">It specifically does not consider the actual meaning of any constituent identifiers.</span></span> <span data-ttu-id="dec56-1609">Například pokud `x` a `y` jsou identifikátory, pak `x.y` je správná gramatika pro typ, i v případě `x.y` není ve skutečnosti označení typu.</span><span class="sxs-lookup"><span data-stu-id="dec56-1609">For example, if `x` and `y` are identifiers, then `x.y` is correct grammar for a type, even if `x.y` doesn't actually denote a type.</span></span>

<span data-ttu-id="dec56-1610">Z pravidla mnohoznačnosti vyplývá, že pokud `x` a `y` jsou identifikátory, `(x)y`, `(x)(y)`, a `(x)(-y)` jsou *cast_expression*s, ale `(x)-y` není, i v případě `x` identifikuje typ.</span><span class="sxs-lookup"><span data-stu-id="dec56-1610">From the disambiguation rule it follows that, if `x` and `y` are identifiers, `(x)y`, `(x)(y)`, and `(x)(-y)` are *cast_expression*s, but `(x)-y` is not, even if `x` identifies a type.</span></span> <span data-ttu-id="dec56-1611">Ale pokud `x` je klíčové slovo, který identifikuje předdefinovaný typ (například `int`), pak se všechny čtyři způsoby *cast_expression*s (protože takové klíčové slovo nebylo možné pravděpodobně výraz samostatně).</span><span class="sxs-lookup"><span data-stu-id="dec56-1611">However, if `x` is a keyword that identifies a predefined type (such as `int`), then all four forms are *cast_expression*s (because such a keyword could not possibly be an expression by itself).</span></span>

### <a name="await-expressions"></a><span data-ttu-id="dec56-1612">Výrazy await</span><span class="sxs-lookup"><span data-stu-id="dec56-1612">Await expressions</span></span>

<span data-ttu-id="dec56-1613">Operátor await se používá k vyhodnocení nadřazené asynchronní funkce pozastavit až do dokončení asynchronní operace reprezentovány operandem.</span><span class="sxs-lookup"><span data-stu-id="dec56-1613">The await operator is used to suspend evaluation of the enclosing async function until the asynchronous operation represented by the operand has completed.</span></span>

```antlr
await_expression
    : 'await' unary_expression
    ;
```

<span data-ttu-id="dec56-1614">*Await_expression* je povolený jenom v těle asynchronní funkci ([iterátory](classes.md#iterators)).</span><span class="sxs-lookup"><span data-stu-id="dec56-1614">An *await_expression* is only allowed in the body of an async function ([Iterators](classes.md#iterators)).</span></span> <span data-ttu-id="dec56-1615">V rámci nejbližšího obklopujícího asynchronní funkce *await_expression* nelze provádět v těchto umístěních:</span><span class="sxs-lookup"><span data-stu-id="dec56-1615">Within the nearest enclosing async function, an *await_expression* may not occur in these places:</span></span>

*  <span data-ttu-id="dec56-1616">Uvnitř anonymní funkce. vnořené (bez asynchronní)</span><span class="sxs-lookup"><span data-stu-id="dec56-1616">Inside a nested (non-async) anonymous function</span></span>
*  <span data-ttu-id="dec56-1617">Uvnitř bloku *lock_statement*</span><span class="sxs-lookup"><span data-stu-id="dec56-1617">Inside the block of a *lock_statement*</span></span>
*  <span data-ttu-id="dec56-1618">V nezabezpečeném kontextu.</span><span class="sxs-lookup"><span data-stu-id="dec56-1618">In an unsafe context</span></span>

<span data-ttu-id="dec56-1619">Všimněte si, že *await_expression* nemůže nastat ve většině případů v rámci *query_expression*, protože ty jsou syntakticky transformuje na použití jiných asynchronní výrazy lambda.</span><span class="sxs-lookup"><span data-stu-id="dec56-1619">Note that an *await_expression* cannot occur in most places within a *query_expression*, because those are syntactically transformed to use non-async lambda expressions.</span></span>

<span data-ttu-id="dec56-1620">V asynchronní funkci `await` nelze použít jako identifikátor.</span><span class="sxs-lookup"><span data-stu-id="dec56-1620">Inside of an async function, `await` cannot be used as an identifier.</span></span> <span data-ttu-id="dec56-1621">Proto neexistuje žádná syntaktická byla zjištěna dvojznačnost mezi výrazy await a různé výrazy zahrnující identifikátory.</span><span class="sxs-lookup"><span data-stu-id="dec56-1621">There is therefore no syntactic ambiguity between await-expressions and various expressions involving identifiers.</span></span> <span data-ttu-id="dec56-1622">Mimo asynchronních funkcí `await` funguje jako normální identifikátor.</span><span class="sxs-lookup"><span data-stu-id="dec56-1622">Outside of async functions, `await` acts as a normal identifier.</span></span>

<span data-ttu-id="dec56-1623">Operand *await_expression* je volána ***úloh***.</span><span class="sxs-lookup"><span data-stu-id="dec56-1623">The operand of an *await_expression* is called the ***task***.</span></span> <span data-ttu-id="dec56-1624">Představuje asynchronní operaci, která může nebo nemusí být v době dokončení *await_expression* vyhodnocena.</span><span class="sxs-lookup"><span data-stu-id="dec56-1624">It represents an asynchronous operation that may or may not be complete at the time the *await_expression* is evaluated.</span></span> <span data-ttu-id="dec56-1625">Je účel operátor await k pozastavení provádění nadřazené funkce asynchronní, dokud není dokončen očekávaný úkol a získat jeho výsledek.</span><span class="sxs-lookup"><span data-stu-id="dec56-1625">The purpose of the await operator is to suspend execution of the enclosing async function until the awaited task is complete, and then obtain its outcome.</span></span>

#### <a name="awaitable-expressions"></a><span data-ttu-id="dec56-1626">Očekávatelný výrazy</span><span class="sxs-lookup"><span data-stu-id="dec56-1626">Awaitable expressions</span></span>

<span data-ttu-id="dec56-1627">Úloha výraz await musí být ***očekávatelný***.</span><span class="sxs-lookup"><span data-stu-id="dec56-1627">The task of an await expression is required to be ***awaitable***.</span></span> <span data-ttu-id="dec56-1628">Výraz `t` může používat await, obsahuje jeden z následujících akcí:</span><span class="sxs-lookup"><span data-stu-id="dec56-1628">An expression `t` is awaitable if one of the following holds:</span></span>

*  <span data-ttu-id="dec56-1629">`t` je typu v době kompilace `dynamic`</span><span class="sxs-lookup"><span data-stu-id="dec56-1629">`t` is of compile time type `dynamic`</span></span>
*  <span data-ttu-id="dec56-1630">`t` má dostupné instance nebo rozšiřující metoda volá `GetAwaiter` bez parametrů a žádné parametry typu a návratový typ `A` pro které všechny tyto uchování:</span><span class="sxs-lookup"><span data-stu-id="dec56-1630">`t` has an accessible instance or extension method called `GetAwaiter` with no parameters and no type parameters, and a return type `A` for which all of the following hold:</span></span>
   * <span data-ttu-id="dec56-1631">`A` implementuje rozhraní `System.Runtime.CompilerServices.INotifyCompletion` (nazývaným jako `INotifyCompletion` pro zkrácení)</span><span class="sxs-lookup"><span data-stu-id="dec56-1631">`A` implements the interface `System.Runtime.CompilerServices.INotifyCompletion` (hereafter known as `INotifyCompletion` for brevity)</span></span>
   * <span data-ttu-id="dec56-1632">`A` má vlastnost instance přístupné, čitelné `IsCompleted` typu `bool`</span><span class="sxs-lookup"><span data-stu-id="dec56-1632">`A` has an accessible, readable instance property `IsCompleted` of type `bool`</span></span>
   * <span data-ttu-id="dec56-1633">`A` je dostupná metoda instance `GetResult` bez parametrů a žádné parametry typu</span><span class="sxs-lookup"><span data-stu-id="dec56-1633">`A` has an accessible instance method `GetResult` with no parameters and no type parameters</span></span>

<span data-ttu-id="dec56-1634">Účelem `GetAwaiter` metodou je použít k získání ***awaiter*** pro úlohu.</span><span class="sxs-lookup"><span data-stu-id="dec56-1634">The purpose of the `GetAwaiter` method is to obtain an ***awaiter*** for the task.</span></span> <span data-ttu-id="dec56-1635">Typ `A` je volána ***awaiter typ*** pro výraz await.</span><span class="sxs-lookup"><span data-stu-id="dec56-1635">The type `A` is called the ***awaiter type*** for the await expression.</span></span>

<span data-ttu-id="dec56-1636">Účelem `IsCompleted` vlastnost je určit, pokud úloha je již dokončena.</span><span class="sxs-lookup"><span data-stu-id="dec56-1636">The purpose of the `IsCompleted` property is to determine if the task is already complete.</span></span> <span data-ttu-id="dec56-1637">Pokud ano, není nutné pozastavit hodnocení.</span><span class="sxs-lookup"><span data-stu-id="dec56-1637">If so, there is no need to suspend evaluation.</span></span>

<span data-ttu-id="dec56-1638">Účelem `INotifyCompletion.OnCompleted` metoda, je registrace "pokračování" k úkolu; to znamená delegáta (typu `System.Action`), která bude volána po dokončení úlohy.</span><span class="sxs-lookup"><span data-stu-id="dec56-1638">The purpose of the `INotifyCompletion.OnCompleted` method is to sign up a "continuation" to the task; i.e. a delegate (of type `System.Action`) that will be invoked once the task is complete.</span></span>

<span data-ttu-id="dec56-1639">Účelem `GetResult` metodou je získat výsledek úkolu, jakmile je dokončená.</span><span class="sxs-lookup"><span data-stu-id="dec56-1639">The purpose of the `GetResult` method is to obtain the outcome of the task once it is complete.</span></span> <span data-ttu-id="dec56-1640">Tento výsledek může být úspěšné dokončení, případně se je výsledná hodnota, nebo může být výjimku, která je vyvolána `GetResult` metody.</span><span class="sxs-lookup"><span data-stu-id="dec56-1640">This outcome may be successful completion, possibly with a result value, or it may be an exception which is thrown by the `GetResult` method.</span></span>

#### <a name="classification-of-await-expressions"></a><span data-ttu-id="dec56-1641">Výrazy await klasifikace</span><span class="sxs-lookup"><span data-stu-id="dec56-1641">Classification of await expressions</span></span>

<span data-ttu-id="dec56-1642">Výraz `await t` klasifikovaný stejným způsobem jako výraz `(t).GetAwaiter().GetResult()`.</span><span class="sxs-lookup"><span data-stu-id="dec56-1642">The expression `await t` is classified the same way as the expression `(t).GetAwaiter().GetResult()`.</span></span> <span data-ttu-id="dec56-1643">Proto pokud návratový typ `GetResult` je `void`, *await_expression* klasifikovaný jako nothing.</span><span class="sxs-lookup"><span data-stu-id="dec56-1643">Thus, if the return type of `GetResult` is `void`, the *await_expression* is classified as nothing.</span></span> <span data-ttu-id="dec56-1644">Pokud má návratový typ jiný než void `T`, *await_expression* klasifikovaný jako hodnotu typu `T`.</span><span class="sxs-lookup"><span data-stu-id="dec56-1644">If it has a non-void return type `T`, the *await_expression* is classified as a value of type `T`.</span></span>

#### <a name="runtime-evaluation-of-await-expressions"></a><span data-ttu-id="dec56-1645">Výrazy await vyhodnocení modulu runtime</span><span class="sxs-lookup"><span data-stu-id="dec56-1645">Runtime evaluation of await expressions</span></span>

<span data-ttu-id="dec56-1646">Za běhu, výraz `await t` se vyhodnotí takto:</span><span class="sxs-lookup"><span data-stu-id="dec56-1646">At runtime, the expression `await t` is evaluated as follows:</span></span>

*  <span data-ttu-id="dec56-1647">Awaiter `a` je získanou vyhodnocením výrazu `(t).GetAwaiter()`.</span><span class="sxs-lookup"><span data-stu-id="dec56-1647">An awaiter `a` is obtained by evaluating the expression `(t).GetAwaiter()`.</span></span>
*  <span data-ttu-id="dec56-1648">A `bool` `b` je získanou vyhodnocením výrazu `(a).IsCompleted`.</span><span class="sxs-lookup"><span data-stu-id="dec56-1648">A `bool` `b` is obtained by evaluating the expression `(a).IsCompleted`.</span></span>
*  <span data-ttu-id="dec56-1649">Pokud `b` je `false` pak vyhodnocení závisí na tom, zda `a` implementuje rozhraní `System.Runtime.CompilerServices.ICriticalNotifyCompletion` (nazývaným jako `ICriticalNotifyCompletion` pro zkrácení).</span><span class="sxs-lookup"><span data-stu-id="dec56-1649">If `b` is `false` then evaluation depends on whether `a` implements the interface `System.Runtime.CompilerServices.ICriticalNotifyCompletion` (hereafter known as `ICriticalNotifyCompletion` for brevity).</span></span> <span data-ttu-id="dec56-1650">Tato kontrola se provádí na vazby čas; například v době běhu Pokud `a` má typ času kompilace `dynamic`a v době kompilace, jinak.</span><span class="sxs-lookup"><span data-stu-id="dec56-1650">This check is done at binding time; i.e. at runtime if `a` has the compile time type `dynamic`, and at compile time otherwise.</span></span> <span data-ttu-id="dec56-1651">Umožní `r` označení delegát pokračování ([iterátory](classes.md#iterators)):</span><span class="sxs-lookup"><span data-stu-id="dec56-1651">Let `r` denote the resumption delegate ([Iterators](classes.md#iterators)):</span></span>
    * <span data-ttu-id="dec56-1652">Pokud `a` neimplementuje `ICriticalNotifyCompletion`, pak výraz `(a as (INotifyCompletion)).OnCompleted(r)` vyhodnocena.</span><span class="sxs-lookup"><span data-stu-id="dec56-1652">If `a` does not implement `ICriticalNotifyCompletion`, then the expression `(a as (INotifyCompletion)).OnCompleted(r)` is evaluated.</span></span>
    * <span data-ttu-id="dec56-1653">Pokud `a` implementovat `ICriticalNotifyCompletion`, pak výraz `(a as (ICriticalNotifyCompletion)).UnsafeOnCompleted(r)` vyhodnocena.</span><span class="sxs-lookup"><span data-stu-id="dec56-1653">If `a` does implement `ICriticalNotifyCompletion`, then the expression `(a as (ICriticalNotifyCompletion)).UnsafeOnCompleted(r)` is evaluated.</span></span>
    * <span data-ttu-id="dec56-1654">Vyhodnocení pak pozastavena a ovládací prvek se vrátí aktuální volajícímu asynchronní funkci.</span><span class="sxs-lookup"><span data-stu-id="dec56-1654">Evaluation is then suspended, and control is returned to the current caller of the async function.</span></span>
*  <span data-ttu-id="dec56-1655">Buď ihned po (Pokud `b` byl `true`), nebo na novější vyvolání delegáta obnovení činnosti (Pokud `b` byl `false`), výraz `(a).GetResult()` vyhodnocena.</span><span class="sxs-lookup"><span data-stu-id="dec56-1655">Either immediately after (if `b` was `true`), or upon later invocation of the resumption delegate (if `b` was `false`), the expression `(a).GetResult()` is evaluated.</span></span> <span data-ttu-id="dec56-1656">Pokud vrátí hodnotu, je tato hodnota výsledku *await_expression*.</span><span class="sxs-lookup"><span data-stu-id="dec56-1656">If it returns a value, that value is the result of the *await_expression*.</span></span> <span data-ttu-id="dec56-1657">Výsledek jinak má hodnotu nothing.</span><span class="sxs-lookup"><span data-stu-id="dec56-1657">Otherwise the result is nothing.</span></span>

<span data-ttu-id="dec56-1658">Awaiter implementaci metody rozhraní `INotifyCompletion.OnCompleted` a `ICriticalNotifyCompletion.UnsafeOnCompleted` by se měl delegáta `r` má být volána nejvýše jednou.</span><span class="sxs-lookup"><span data-stu-id="dec56-1658">An awaiter's implementation of the interface methods `INotifyCompletion.OnCompleted` and `ICriticalNotifyCompletion.UnsafeOnCompleted` should cause the delegate `r` to be invoked at most once.</span></span> <span data-ttu-id="dec56-1659">V opačném případě nadřazené funkce asynchronní chování není definováno.</span><span class="sxs-lookup"><span data-stu-id="dec56-1659">Otherwise, the behavior of the enclosing async function is undefined.</span></span>

## <a name="arithmetic-operators"></a><span data-ttu-id="dec56-1660">Aritmetické operátory</span><span class="sxs-lookup"><span data-stu-id="dec56-1660">Arithmetic operators</span></span>

<span data-ttu-id="dec56-1661">`*`, `/`, `%`, `+`, A `-` operátory jsou volány aritmetické operátory.</span><span class="sxs-lookup"><span data-stu-id="dec56-1661">The `*`, `/`, `%`, `+`, and `-` operators are called the arithmetic operators.</span></span>

```antlr
multiplicative_expression
    : unary_expression
    | multiplicative_expression '*' unary_expression
    | multiplicative_expression '/' unary_expression
    | multiplicative_expression '%' unary_expression
    ;

additive_expression
    : multiplicative_expression
    | additive_expression '+' multiplicative_expression
    | additive_expression '-' multiplicative_expression
    ;
```

<span data-ttu-id="dec56-1662">Pokud operand aritmetický operátor má typ kompilace `dynamic`, pak je dynamicky vázán výraz ([dynamické vazby](expressions.md#dynamic-binding)).</span><span class="sxs-lookup"><span data-stu-id="dec56-1662">If an operand of an arithmetic operator has the compile-time type `dynamic`, then the expression is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)).</span></span> <span data-ttu-id="dec56-1663">V tomto případě je kompilaci typu výrazu `dynamic`, a rozlišení je popsáno níže se provede v době běhu pomocí run-time typu těchto operandů, která mají typ kompilace `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="dec56-1663">In this case the compile-time type of the expression is `dynamic`, and the resolution described below will take place at run-time using the run-time type of those operands that have the compile-time type `dynamic`.</span></span>

### <a name="multiplication-operator"></a><span data-ttu-id="dec56-1664">Operátor násobení</span><span class="sxs-lookup"><span data-stu-id="dec56-1664">Multiplication operator</span></span>

<span data-ttu-id="dec56-1665">Operace formuláře `x * y`, binárním operátorem rozlišení přetěžování ([binárním operátorem rozlišení přetěžování](expressions.md#binary-operator-overload-resolution)) se použije k výběru na konkrétní operátor implementace.</span><span class="sxs-lookup"><span data-stu-id="dec56-1665">For an operation of the form `x * y`, binary operator overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="dec56-1666">Operandy jsou převedeny na zvoleném operátorovi typy parametrů a typ výsledku je návratový typ operátoru.</span><span class="sxs-lookup"><span data-stu-id="dec56-1666">The operands are converted to the parameter types of the selected operator, and the type of the result is the return type of the operator.</span></span>

<span data-ttu-id="dec56-1667">Operátory násobení předdefinované jsou uvedeny níže.</span><span class="sxs-lookup"><span data-stu-id="dec56-1667">The predefined multiplication operators are listed below.</span></span> <span data-ttu-id="dec56-1668">Všechny operátory vypočítat součin `x` a `y`.</span><span class="sxs-lookup"><span data-stu-id="dec56-1668">The operators all compute the product of `x` and `y`.</span></span>

*  <span data-ttu-id="dec56-1669">Násobení celé číslo:</span><span class="sxs-lookup"><span data-stu-id="dec56-1669">Integer multiplication:</span></span>

   ```csharp
   int operator *(int x, int y);
   uint operator *(uint x, uint y);
   long operator *(long x, long y);
   ulong operator *(ulong x, ulong y);
   ```

   <span data-ttu-id="dec56-1670">V `checked` kontextu, pokud je mimo rozsah typu výsledku, `System.OverflowException` je vyvolána výjimka.</span><span class="sxs-lookup"><span data-stu-id="dec56-1670">In a `checked` context, if the product is outside the range of the result type, a `System.OverflowException` is thrown.</span></span> <span data-ttu-id="dec56-1671">V `unchecked` kontextu, nezobrazují, přetečení a žádné významné implikovaný bits mimo rozsah typu výsledku se zahodí.</span><span class="sxs-lookup"><span data-stu-id="dec56-1671">In an `unchecked` context, overflows are not reported and any significant high-order bits outside the range of the result type are discarded.</span></span>


*  <span data-ttu-id="dec56-1672">Násobení s plovoucí desetinnou čárkou:</span><span class="sxs-lookup"><span data-stu-id="dec56-1672">Floating-point multiplication:</span></span>

   ```csharp
   float operator *(float x, float y);
   double operator *(double x, double y);
   ```

   <span data-ttu-id="dec56-1673">Produkt se počítá podle pravidel objektů aritmetické IEEE 754.</span><span class="sxs-lookup"><span data-stu-id="dec56-1673">The product is computed according to the rules of IEEE 754 arithmetic.</span></span> <span data-ttu-id="dec56-1674">V následující tabulce jsou uvedeny výsledky všech možných kombinací nenulové hodnoty konečné, nul, nekonečno a NaN.</span><span class="sxs-lookup"><span data-stu-id="dec56-1674">The following table lists the results of all possible combinations of nonzero finite values, zeros, infinities, and NaN's.</span></span> <span data-ttu-id="dec56-1675">V tabulce `x` a `y` jsou konečné kladné hodnoty.</span><span class="sxs-lookup"><span data-stu-id="dec56-1675">In the table, `x` and `y` are positive finite values.</span></span> <span data-ttu-id="dec56-1676">`z` je výsledkem `x * y`.</span><span class="sxs-lookup"><span data-stu-id="dec56-1676">`z` is the result of `x * y`.</span></span> <span data-ttu-id="dec56-1677">Pokud je výsledek příliš velký pro cílový typ `z` je nekonečno.</span><span class="sxs-lookup"><span data-stu-id="dec56-1677">If the result is too large for the destination type, `z` is infinity.</span></span> <span data-ttu-id="dec56-1678">Pokud je výsledek příliš malá pro cílový typ `z` je nula.</span><span class="sxs-lookup"><span data-stu-id="dec56-1678">If the result is too small for the destination type, `z` is zero.</span></span>

   |      |      |      |     |     |      |      |     |
   |:----:|-----:|:----:|:---:|:---:|:----:|:----:|:----|
   |      | <span data-ttu-id="dec56-1679">+ y</span><span class="sxs-lookup"><span data-stu-id="dec56-1679">+y</span></span>   | <span data-ttu-id="dec56-1680">-y</span><span class="sxs-lookup"><span data-stu-id="dec56-1680">-y</span></span>   | <span data-ttu-id="dec56-1681">+0</span><span class="sxs-lookup"><span data-stu-id="dec56-1681">+0</span></span>  | <span data-ttu-id="dec56-1682">-0</span><span class="sxs-lookup"><span data-stu-id="dec56-1682">-0</span></span>  | <span data-ttu-id="dec56-1683">+ inf</span><span class="sxs-lookup"><span data-stu-id="dec56-1683">+inf</span></span> | <span data-ttu-id="dec56-1684">-inf</span><span class="sxs-lookup"><span data-stu-id="dec56-1684">-inf</span></span> | <span data-ttu-id="dec56-1685">NaN</span><span class="sxs-lookup"><span data-stu-id="dec56-1685">NaN</span></span> | 
   | <span data-ttu-id="dec56-1686">+x</span><span class="sxs-lookup"><span data-stu-id="dec56-1686">+x</span></span>   | <span data-ttu-id="dec56-1687">+z</span><span class="sxs-lookup"><span data-stu-id="dec56-1687">+z</span></span>   | <span data-ttu-id="dec56-1688">-z</span><span class="sxs-lookup"><span data-stu-id="dec56-1688">-z</span></span>   | <span data-ttu-id="dec56-1689">+0</span><span class="sxs-lookup"><span data-stu-id="dec56-1689">+0</span></span>  | <span data-ttu-id="dec56-1690">-0</span><span class="sxs-lookup"><span data-stu-id="dec56-1690">-0</span></span>  | <span data-ttu-id="dec56-1691">+ inf</span><span class="sxs-lookup"><span data-stu-id="dec56-1691">+inf</span></span> | <span data-ttu-id="dec56-1692">-inf</span><span class="sxs-lookup"><span data-stu-id="dec56-1692">-inf</span></span> | <span data-ttu-id="dec56-1693">NaN</span><span class="sxs-lookup"><span data-stu-id="dec56-1693">NaN</span></span> | 
   | <span data-ttu-id="dec56-1694">-x</span><span class="sxs-lookup"><span data-stu-id="dec56-1694">-x</span></span>   | <span data-ttu-id="dec56-1695">-z</span><span class="sxs-lookup"><span data-stu-id="dec56-1695">-z</span></span>   | <span data-ttu-id="dec56-1696">+z</span><span class="sxs-lookup"><span data-stu-id="dec56-1696">+z</span></span>   | <span data-ttu-id="dec56-1697">-0</span><span class="sxs-lookup"><span data-stu-id="dec56-1697">-0</span></span>  | <span data-ttu-id="dec56-1698">+0</span><span class="sxs-lookup"><span data-stu-id="dec56-1698">+0</span></span>  | <span data-ttu-id="dec56-1699">-inf</span><span class="sxs-lookup"><span data-stu-id="dec56-1699">-inf</span></span> | <span data-ttu-id="dec56-1700">+ inf</span><span class="sxs-lookup"><span data-stu-id="dec56-1700">+inf</span></span> | <span data-ttu-id="dec56-1701">NaN</span><span class="sxs-lookup"><span data-stu-id="dec56-1701">NaN</span></span> | 
   | <span data-ttu-id="dec56-1702">+0</span><span class="sxs-lookup"><span data-stu-id="dec56-1702">+0</span></span>   | <span data-ttu-id="dec56-1703">+0</span><span class="sxs-lookup"><span data-stu-id="dec56-1703">+0</span></span>   | <span data-ttu-id="dec56-1704">-0</span><span class="sxs-lookup"><span data-stu-id="dec56-1704">-0</span></span>   | <span data-ttu-id="dec56-1705">+0</span><span class="sxs-lookup"><span data-stu-id="dec56-1705">+0</span></span>  | <span data-ttu-id="dec56-1706">-0</span><span class="sxs-lookup"><span data-stu-id="dec56-1706">-0</span></span>  | <span data-ttu-id="dec56-1707">NaN</span><span class="sxs-lookup"><span data-stu-id="dec56-1707">NaN</span></span>  | <span data-ttu-id="dec56-1708">NaN</span><span class="sxs-lookup"><span data-stu-id="dec56-1708">NaN</span></span>  | <span data-ttu-id="dec56-1709">NaN</span><span class="sxs-lookup"><span data-stu-id="dec56-1709">NaN</span></span> | 
   | <span data-ttu-id="dec56-1710">-0</span><span class="sxs-lookup"><span data-stu-id="dec56-1710">-0</span></span>   | <span data-ttu-id="dec56-1711">-0</span><span class="sxs-lookup"><span data-stu-id="dec56-1711">-0</span></span>   | <span data-ttu-id="dec56-1712">+0</span><span class="sxs-lookup"><span data-stu-id="dec56-1712">+0</span></span>   | <span data-ttu-id="dec56-1713">-0</span><span class="sxs-lookup"><span data-stu-id="dec56-1713">-0</span></span>  | <span data-ttu-id="dec56-1714">+0</span><span class="sxs-lookup"><span data-stu-id="dec56-1714">+0</span></span>  | <span data-ttu-id="dec56-1715">NaN</span><span class="sxs-lookup"><span data-stu-id="dec56-1715">NaN</span></span>  | <span data-ttu-id="dec56-1716">NaN</span><span class="sxs-lookup"><span data-stu-id="dec56-1716">NaN</span></span>  | <span data-ttu-id="dec56-1717">NaN</span><span class="sxs-lookup"><span data-stu-id="dec56-1717">NaN</span></span> | 
   | <span data-ttu-id="dec56-1718">+ inf</span><span class="sxs-lookup"><span data-stu-id="dec56-1718">+inf</span></span> | <span data-ttu-id="dec56-1719">+ inf</span><span class="sxs-lookup"><span data-stu-id="dec56-1719">+inf</span></span> | <span data-ttu-id="dec56-1720">-inf</span><span class="sxs-lookup"><span data-stu-id="dec56-1720">-inf</span></span> | <span data-ttu-id="dec56-1721">NaN</span><span class="sxs-lookup"><span data-stu-id="dec56-1721">NaN</span></span> | <span data-ttu-id="dec56-1722">NaN</span><span class="sxs-lookup"><span data-stu-id="dec56-1722">NaN</span></span> | <span data-ttu-id="dec56-1723">+ inf</span><span class="sxs-lookup"><span data-stu-id="dec56-1723">+inf</span></span> | <span data-ttu-id="dec56-1724">-inf</span><span class="sxs-lookup"><span data-stu-id="dec56-1724">-inf</span></span> | <span data-ttu-id="dec56-1725">NaN</span><span class="sxs-lookup"><span data-stu-id="dec56-1725">NaN</span></span> | 
   | <span data-ttu-id="dec56-1726">-inf</span><span class="sxs-lookup"><span data-stu-id="dec56-1726">-inf</span></span> | <span data-ttu-id="dec56-1727">-inf</span><span class="sxs-lookup"><span data-stu-id="dec56-1727">-inf</span></span> | <span data-ttu-id="dec56-1728">+ inf</span><span class="sxs-lookup"><span data-stu-id="dec56-1728">+inf</span></span> | <span data-ttu-id="dec56-1729">NaN</span><span class="sxs-lookup"><span data-stu-id="dec56-1729">NaN</span></span> | <span data-ttu-id="dec56-1730">NaN</span><span class="sxs-lookup"><span data-stu-id="dec56-1730">NaN</span></span> | <span data-ttu-id="dec56-1731">-inf</span><span class="sxs-lookup"><span data-stu-id="dec56-1731">-inf</span></span> | <span data-ttu-id="dec56-1732">+ inf</span><span class="sxs-lookup"><span data-stu-id="dec56-1732">+inf</span></span> | <span data-ttu-id="dec56-1733">NaN</span><span class="sxs-lookup"><span data-stu-id="dec56-1733">NaN</span></span> | 
   | <span data-ttu-id="dec56-1734">NaN</span><span class="sxs-lookup"><span data-stu-id="dec56-1734">NaN</span></span>  | <span data-ttu-id="dec56-1735">NaN</span><span class="sxs-lookup"><span data-stu-id="dec56-1735">NaN</span></span>  | <span data-ttu-id="dec56-1736">NaN</span><span class="sxs-lookup"><span data-stu-id="dec56-1736">NaN</span></span>  | <span data-ttu-id="dec56-1737">NaN</span><span class="sxs-lookup"><span data-stu-id="dec56-1737">NaN</span></span> | <span data-ttu-id="dec56-1738">NaN</span><span class="sxs-lookup"><span data-stu-id="dec56-1738">NaN</span></span> | <span data-ttu-id="dec56-1739">NaN</span><span class="sxs-lookup"><span data-stu-id="dec56-1739">NaN</span></span>  | <span data-ttu-id="dec56-1740">NaN</span><span class="sxs-lookup"><span data-stu-id="dec56-1740">NaN</span></span>  | <span data-ttu-id="dec56-1741">NaN</span><span class="sxs-lookup"><span data-stu-id="dec56-1741">NaN</span></span> | 

*  <span data-ttu-id="dec56-1742">Desetinné násobení:</span><span class="sxs-lookup"><span data-stu-id="dec56-1742">Decimal multiplication:</span></span>

   ```csharp
   decimal operator *(decimal x, decimal y);
   ```

   <span data-ttu-id="dec56-1743">Pokud výsledná hodnota je příliš velká pro reprezentaci v jazyce `decimal` formátu, `System.OverflowException` je vyvolána výjimka.</span><span class="sxs-lookup"><span data-stu-id="dec56-1743">If the resulting value is too large to represent in the `decimal` format, a `System.OverflowException` is thrown.</span></span> <span data-ttu-id="dec56-1744">Pokud hodnota výsledku je příliš malá pro reprezentaci v jazyce `decimal` formátu, výsledek je nula.</span><span class="sxs-lookup"><span data-stu-id="dec56-1744">If the result value is too small to represent in the `decimal` format, the result is zero.</span></span> <span data-ttu-id="dec56-1745">Škálování výsledek před všechny zaokrouhlení je součtem váhy dva operandy.</span><span class="sxs-lookup"><span data-stu-id="dec56-1745">The scale of the result, before any rounding, is the sum of the scales of the two operands.</span></span>

   <span data-ttu-id="dec56-1746">Je ekvivalentní k použití operátor násobení typu Decimal násobení `System.Decimal`.</span><span class="sxs-lookup"><span data-stu-id="dec56-1746">Decimal multiplication is equivalent to using the multiplication operator of type `System.Decimal`.</span></span>


### <a name="division-operator"></a><span data-ttu-id="dec56-1747">Operátor dělení</span><span class="sxs-lookup"><span data-stu-id="dec56-1747">Division operator</span></span>

<span data-ttu-id="dec56-1748">Operace formuláře `x / y`, binárním operátorem rozlišení přetěžování ([binárním operátorem rozlišení přetěžování](expressions.md#binary-operator-overload-resolution)) se použije k výběru na konkrétní operátor implementace.</span><span class="sxs-lookup"><span data-stu-id="dec56-1748">For an operation of the form `x / y`, binary operator overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="dec56-1749">Operandy jsou převedeny na zvoleném operátorovi typy parametrů a typ výsledku je návratový typ operátoru.</span><span class="sxs-lookup"><span data-stu-id="dec56-1749">The operands are converted to the parameter types of the selected operator, and the type of the result is the return type of the operator.</span></span>

<span data-ttu-id="dec56-1750">Předdefinované dělení operátory jsou uvedeny níže.</span><span class="sxs-lookup"><span data-stu-id="dec56-1750">The predefined division operators are listed below.</span></span> <span data-ttu-id="dec56-1751">Všechny operátory compute podíl `x` a `y`.</span><span class="sxs-lookup"><span data-stu-id="dec56-1751">The operators all compute the quotient of `x` and `y`.</span></span>

*  <span data-ttu-id="dec56-1752">Dělení celého čísla:</span><span class="sxs-lookup"><span data-stu-id="dec56-1752">Integer division:</span></span>

   ```csharp
   int operator /(int x, int y);
   uint operator /(uint x, uint y);
   long operator /(long x, long y);
   ulong operator /(ulong x, ulong y);
   ```

   <span data-ttu-id="dec56-1753">Pokud hodnota pravého operandu je nula, `System.DivideByZeroException` je vyvolána výjimka.</span><span class="sxs-lookup"><span data-stu-id="dec56-1753">If the value of the right operand is zero, a `System.DivideByZeroException` is thrown.</span></span>

   <span data-ttu-id="dec56-1754">Rozdělení zaokrouhlí výsledek směrem k nule.</span><span class="sxs-lookup"><span data-stu-id="dec56-1754">The division rounds the result towards zero.</span></span> <span data-ttu-id="dec56-1755">Proto absolutní hodnota výsledku je největší možné číslo, které je menší než absolutní hodnota kvocientu dva operandy.</span><span class="sxs-lookup"><span data-stu-id="dec56-1755">Thus the absolute value of the result is the largest possible integer that is less than or equal to the absolute value of the quotient of the two operands.</span></span> <span data-ttu-id="dec56-1756">Výsledkem může být nula nebo pozitivní při dva operandy mají stejné znaménko a nula nebo záporná, pokud mají dva operandy opačný příznaky.</span><span class="sxs-lookup"><span data-stu-id="dec56-1756">The result is zero or positive when the two operands have the same sign and zero or negative when the two operands have opposite signs.</span></span>

   <span data-ttu-id="dec56-1757">Pokud levý operand je nejmenší reprezentovatelné `int` nebo `long` hodnotu a pravý operand je `-1`, dojde k přetečení.</span><span class="sxs-lookup"><span data-stu-id="dec56-1757">If the left operand is the smallest representable `int` or `long` value and the right operand is `-1`, an overflow occurs.</span></span> <span data-ttu-id="dec56-1758">V `checked` kontextu, způsobí to, že `System.ArithmeticException` (nebo jejich podtřída) Chcete-li být vyvolána.</span><span class="sxs-lookup"><span data-stu-id="dec56-1758">In a `checked` context, this causes a `System.ArithmeticException` (or a subclass thereof) to be thrown.</span></span> <span data-ttu-id="dec56-1759">V `unchecked` kontextu, je definováno implementací, zda `System.ArithmeticException` (nebo jejich podtřída) je vyvolána nebo přetečení doprovází neohlášených výslednou hodnotu, přičemž levého operandu.</span><span class="sxs-lookup"><span data-stu-id="dec56-1759">In an `unchecked` context, it is implementation-defined as to whether a `System.ArithmeticException` (or a subclass thereof) is thrown or the overflow goes unreported with the resulting value being that of the left operand.</span></span>

*  <span data-ttu-id="dec56-1760">Dělení s pohyblivou čárkou:</span><span class="sxs-lookup"><span data-stu-id="dec56-1760">Floating-point division:</span></span>

   ```csharp
   float operator /(float x, float y);
   double operator /(double x, double y);
   ```

   <span data-ttu-id="dec56-1761">Podíl se počítá podle pravidel objektů aritmetické IEEE 754.</span><span class="sxs-lookup"><span data-stu-id="dec56-1761">The quotient is computed according to the rules of IEEE 754 arithmetic.</span></span> <span data-ttu-id="dec56-1762">V následující tabulce jsou uvedeny výsledky všech možných kombinací nenulové hodnoty konečné, nul, nekonečno a NaN.</span><span class="sxs-lookup"><span data-stu-id="dec56-1762">The following table lists the results of all possible combinations of nonzero finite values, zeros, infinities, and NaN's.</span></span> <span data-ttu-id="dec56-1763">V tabulce `x` a `y` jsou konečné kladné hodnoty.</span><span class="sxs-lookup"><span data-stu-id="dec56-1763">In the table, `x` and `y` are positive finite values.</span></span> <span data-ttu-id="dec56-1764">`z` je výsledkem `x / y`.</span><span class="sxs-lookup"><span data-stu-id="dec56-1764">`z` is the result of `x / y`.</span></span> <span data-ttu-id="dec56-1765">Pokud je výsledek příliš velký pro cílový typ `z` je nekonečno.</span><span class="sxs-lookup"><span data-stu-id="dec56-1765">If the result is too large for the destination type, `z` is infinity.</span></span> <span data-ttu-id="dec56-1766">Pokud je výsledek příliš malá pro cílový typ `z` je nula.</span><span class="sxs-lookup"><span data-stu-id="dec56-1766">If the result is too small for the destination type, `z` is zero.</span></span>

   |      |      |      |      |      |      |      |      |
   |:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|
   |      | <span data-ttu-id="dec56-1767">+ y</span><span class="sxs-lookup"><span data-stu-id="dec56-1767">+y</span></span>   | <span data-ttu-id="dec56-1768">-y</span><span class="sxs-lookup"><span data-stu-id="dec56-1768">-y</span></span>   | <span data-ttu-id="dec56-1769">+0</span><span class="sxs-lookup"><span data-stu-id="dec56-1769">+0</span></span>   | <span data-ttu-id="dec56-1770">-0</span><span class="sxs-lookup"><span data-stu-id="dec56-1770">-0</span></span>   | <span data-ttu-id="dec56-1771">+ inf</span><span class="sxs-lookup"><span data-stu-id="dec56-1771">+inf</span></span> | <span data-ttu-id="dec56-1772">-inf</span><span class="sxs-lookup"><span data-stu-id="dec56-1772">-inf</span></span> | <span data-ttu-id="dec56-1773">NaN</span><span class="sxs-lookup"><span data-stu-id="dec56-1773">NaN</span></span>  | 
   | <span data-ttu-id="dec56-1774">+x</span><span class="sxs-lookup"><span data-stu-id="dec56-1774">+x</span></span>   | <span data-ttu-id="dec56-1775">+z</span><span class="sxs-lookup"><span data-stu-id="dec56-1775">+z</span></span>   | <span data-ttu-id="dec56-1776">-z</span><span class="sxs-lookup"><span data-stu-id="dec56-1776">-z</span></span>   | <span data-ttu-id="dec56-1777">+ inf</span><span class="sxs-lookup"><span data-stu-id="dec56-1777">+inf</span></span> | <span data-ttu-id="dec56-1778">-inf</span><span class="sxs-lookup"><span data-stu-id="dec56-1778">-inf</span></span> | <span data-ttu-id="dec56-1779">+0</span><span class="sxs-lookup"><span data-stu-id="dec56-1779">+0</span></span>   | <span data-ttu-id="dec56-1780">-0</span><span class="sxs-lookup"><span data-stu-id="dec56-1780">-0</span></span>   | <span data-ttu-id="dec56-1781">NaN</span><span class="sxs-lookup"><span data-stu-id="dec56-1781">NaN</span></span>  | 
   | <span data-ttu-id="dec56-1782">-x</span><span class="sxs-lookup"><span data-stu-id="dec56-1782">-x</span></span>   | <span data-ttu-id="dec56-1783">-z</span><span class="sxs-lookup"><span data-stu-id="dec56-1783">-z</span></span>   | <span data-ttu-id="dec56-1784">+z</span><span class="sxs-lookup"><span data-stu-id="dec56-1784">+z</span></span>   | <span data-ttu-id="dec56-1785">-inf</span><span class="sxs-lookup"><span data-stu-id="dec56-1785">-inf</span></span> | <span data-ttu-id="dec56-1786">+ inf</span><span class="sxs-lookup"><span data-stu-id="dec56-1786">+inf</span></span> | <span data-ttu-id="dec56-1787">-0</span><span class="sxs-lookup"><span data-stu-id="dec56-1787">-0</span></span>   | <span data-ttu-id="dec56-1788">+0</span><span class="sxs-lookup"><span data-stu-id="dec56-1788">+0</span></span>   | <span data-ttu-id="dec56-1789">NaN</span><span class="sxs-lookup"><span data-stu-id="dec56-1789">NaN</span></span>  | 
   | <span data-ttu-id="dec56-1790">+0</span><span class="sxs-lookup"><span data-stu-id="dec56-1790">+0</span></span>   | <span data-ttu-id="dec56-1791">+0</span><span class="sxs-lookup"><span data-stu-id="dec56-1791">+0</span></span>   | <span data-ttu-id="dec56-1792">-0</span><span class="sxs-lookup"><span data-stu-id="dec56-1792">-0</span></span>   | <span data-ttu-id="dec56-1793">NaN</span><span class="sxs-lookup"><span data-stu-id="dec56-1793">NaN</span></span>  | <span data-ttu-id="dec56-1794">NaN</span><span class="sxs-lookup"><span data-stu-id="dec56-1794">NaN</span></span>  | <span data-ttu-id="dec56-1795">+0</span><span class="sxs-lookup"><span data-stu-id="dec56-1795">+0</span></span>   | <span data-ttu-id="dec56-1796">-0</span><span class="sxs-lookup"><span data-stu-id="dec56-1796">-0</span></span>   | <span data-ttu-id="dec56-1797">NaN</span><span class="sxs-lookup"><span data-stu-id="dec56-1797">NaN</span></span>  | 
   | <span data-ttu-id="dec56-1798">-0</span><span class="sxs-lookup"><span data-stu-id="dec56-1798">-0</span></span>   | <span data-ttu-id="dec56-1799">-0</span><span class="sxs-lookup"><span data-stu-id="dec56-1799">-0</span></span>   | <span data-ttu-id="dec56-1800">+0</span><span class="sxs-lookup"><span data-stu-id="dec56-1800">+0</span></span>   | <span data-ttu-id="dec56-1801">NaN</span><span class="sxs-lookup"><span data-stu-id="dec56-1801">NaN</span></span>  | <span data-ttu-id="dec56-1802">NaN</span><span class="sxs-lookup"><span data-stu-id="dec56-1802">NaN</span></span>  | <span data-ttu-id="dec56-1803">-0</span><span class="sxs-lookup"><span data-stu-id="dec56-1803">-0</span></span>   | <span data-ttu-id="dec56-1804">+0</span><span class="sxs-lookup"><span data-stu-id="dec56-1804">+0</span></span>   | <span data-ttu-id="dec56-1805">NaN</span><span class="sxs-lookup"><span data-stu-id="dec56-1805">NaN</span></span>  | 
   | <span data-ttu-id="dec56-1806">+ inf</span><span class="sxs-lookup"><span data-stu-id="dec56-1806">+inf</span></span> | <span data-ttu-id="dec56-1807">+ inf</span><span class="sxs-lookup"><span data-stu-id="dec56-1807">+inf</span></span> | <span data-ttu-id="dec56-1808">-inf</span><span class="sxs-lookup"><span data-stu-id="dec56-1808">-inf</span></span> | <span data-ttu-id="dec56-1809">+ inf</span><span class="sxs-lookup"><span data-stu-id="dec56-1809">+inf</span></span> | <span data-ttu-id="dec56-1810">-inf</span><span class="sxs-lookup"><span data-stu-id="dec56-1810">-inf</span></span> | <span data-ttu-id="dec56-1811">NaN</span><span class="sxs-lookup"><span data-stu-id="dec56-1811">NaN</span></span>  | <span data-ttu-id="dec56-1812">NaN</span><span class="sxs-lookup"><span data-stu-id="dec56-1812">NaN</span></span>  | <span data-ttu-id="dec56-1813">NaN</span><span class="sxs-lookup"><span data-stu-id="dec56-1813">NaN</span></span>  | 
   | <span data-ttu-id="dec56-1814">-inf</span><span class="sxs-lookup"><span data-stu-id="dec56-1814">-inf</span></span> | <span data-ttu-id="dec56-1815">-inf</span><span class="sxs-lookup"><span data-stu-id="dec56-1815">-inf</span></span> | <span data-ttu-id="dec56-1816">+ inf</span><span class="sxs-lookup"><span data-stu-id="dec56-1816">+inf</span></span> | <span data-ttu-id="dec56-1817">-inf</span><span class="sxs-lookup"><span data-stu-id="dec56-1817">-inf</span></span> | <span data-ttu-id="dec56-1818">+ inf</span><span class="sxs-lookup"><span data-stu-id="dec56-1818">+inf</span></span> | <span data-ttu-id="dec56-1819">NaN</span><span class="sxs-lookup"><span data-stu-id="dec56-1819">NaN</span></span>  | <span data-ttu-id="dec56-1820">NaN</span><span class="sxs-lookup"><span data-stu-id="dec56-1820">NaN</span></span>  | <span data-ttu-id="dec56-1821">NaN</span><span class="sxs-lookup"><span data-stu-id="dec56-1821">NaN</span></span>  | 
   | <span data-ttu-id="dec56-1822">NaN</span><span class="sxs-lookup"><span data-stu-id="dec56-1822">NaN</span></span>  | <span data-ttu-id="dec56-1823">NaN</span><span class="sxs-lookup"><span data-stu-id="dec56-1823">NaN</span></span>  | <span data-ttu-id="dec56-1824">NaN</span><span class="sxs-lookup"><span data-stu-id="dec56-1824">NaN</span></span>  | <span data-ttu-id="dec56-1825">NaN</span><span class="sxs-lookup"><span data-stu-id="dec56-1825">NaN</span></span>  | <span data-ttu-id="dec56-1826">NaN</span><span class="sxs-lookup"><span data-stu-id="dec56-1826">NaN</span></span>  | <span data-ttu-id="dec56-1827">NaN</span><span class="sxs-lookup"><span data-stu-id="dec56-1827">NaN</span></span>  | <span data-ttu-id="dec56-1828">NaN</span><span class="sxs-lookup"><span data-stu-id="dec56-1828">NaN</span></span>  | <span data-ttu-id="dec56-1829">NaN</span><span class="sxs-lookup"><span data-stu-id="dec56-1829">NaN</span></span>  | 

*  <span data-ttu-id="dec56-1830">Dělení desetinného čísla:</span><span class="sxs-lookup"><span data-stu-id="dec56-1830">Decimal division:</span></span>

   ```csharp
   decimal operator /(decimal x, decimal y);
   ```

   <span data-ttu-id="dec56-1831">Pokud hodnota pravého operandu je nula, `System.DivideByZeroException` je vyvolána výjimka.</span><span class="sxs-lookup"><span data-stu-id="dec56-1831">If the value of the right operand is zero, a `System.DivideByZeroException` is thrown.</span></span> <span data-ttu-id="dec56-1832">Pokud výsledná hodnota je příliš velká pro reprezentaci v jazyce `decimal` formátu, `System.OverflowException` je vyvolána výjimka.</span><span class="sxs-lookup"><span data-stu-id="dec56-1832">If the resulting value is too large to represent in the `decimal` format, a `System.OverflowException` is thrown.</span></span> <span data-ttu-id="dec56-1833">Pokud hodnota výsledku je příliš malá pro reprezentaci v jazyce `decimal` formátu, výsledek je nula.</span><span class="sxs-lookup"><span data-stu-id="dec56-1833">If the result value is too small to represent in the `decimal` format, the result is zero.</span></span> <span data-ttu-id="dec56-1834">Škálování výsledek je nejmenší měřítko, které budou zachovány výsledek rovná nejbližší reprezentovatelné desítkovou hodnotu na true matematické výsledek.</span><span class="sxs-lookup"><span data-stu-id="dec56-1834">The scale of the result is the smallest scale that will preserve a result equal to the nearest representable decimal value to the true mathematical result.</span></span>

   <span data-ttu-id="dec56-1835">Dělení desetinného čísla je ekvivalentní k použití operátor dělení typu `System.Decimal`.</span><span class="sxs-lookup"><span data-stu-id="dec56-1835">Decimal division is equivalent to using the division operator of type `System.Decimal`.</span></span>


### <a name="remainder-operator"></a><span data-ttu-id="dec56-1836">Operátor zbytku</span><span class="sxs-lookup"><span data-stu-id="dec56-1836">Remainder operator</span></span>

<span data-ttu-id="dec56-1837">Operace formuláře `x % y`, binárním operátorem rozlišení přetěžování ([binárním operátorem rozlišení přetěžování](expressions.md#binary-operator-overload-resolution)) se použije k výběru na konkrétní operátor implementace.</span><span class="sxs-lookup"><span data-stu-id="dec56-1837">For an operation of the form `x % y`, binary operator overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="dec56-1838">Operandy jsou převedeny na zvoleném operátorovi typy parametrů a typ výsledku je návratový typ operátoru.</span><span class="sxs-lookup"><span data-stu-id="dec56-1838">The operands are converted to the parameter types of the selected operator, and the type of the result is the return type of the operator.</span></span>

<span data-ttu-id="dec56-1839">Předdefinované zbývající operátory jsou uvedeny níže.</span><span class="sxs-lookup"><span data-stu-id="dec56-1839">The predefined remainder operators are listed below.</span></span> <span data-ttu-id="dec56-1840">Všechny operátory vypočítat zbývající části rozdělení mezi `x` a `y`.</span><span class="sxs-lookup"><span data-stu-id="dec56-1840">The operators all compute the remainder of the division between `x` and `y`.</span></span>

*  <span data-ttu-id="dec56-1841">Zbývající celé číslo:</span><span class="sxs-lookup"><span data-stu-id="dec56-1841">Integer remainder:</span></span>

   ```csharp
   int operator %(int x, int y);
   uint operator %(uint x, uint y);
   long operator %(long x, long y);
   ulong operator %(ulong x, ulong y);
   ```

   <span data-ttu-id="dec56-1842">Výsledek `x % y` hodnota vytvořil `x - (x / y) * y`.</span><span class="sxs-lookup"><span data-stu-id="dec56-1842">The result of `x % y` is the value produced by `x - (x / y) * y`.</span></span> <span data-ttu-id="dec56-1843">Pokud `y` je nula, `System.DivideByZeroException` je vyvolána výjimka.</span><span class="sxs-lookup"><span data-stu-id="dec56-1843">If `y` is zero, a `System.DivideByZeroException` is thrown.</span></span>

   <span data-ttu-id="dec56-1844">Pokud levý operand je nejmenší `int` nebo `long` hodnotu a pravý operand je `-1`, `System.OverflowException` je vyvolána výjimka.</span><span class="sxs-lookup"><span data-stu-id="dec56-1844">If the left operand is the smallest `int` or `long` value and the right operand is `-1`, a `System.OverflowException` is thrown.</span></span> <span data-ttu-id="dec56-1845">V žádném případě nemá `x % y` vyvolání výjimky, kde `x / y` nemusí vyvolat výjimku.</span><span class="sxs-lookup"><span data-stu-id="dec56-1845">In no case does `x % y` throw an exception where `x / y` would not throw an exception.</span></span>

*  <span data-ttu-id="dec56-1846">Zbytek s plovoucí desetinnou čárkou:</span><span class="sxs-lookup"><span data-stu-id="dec56-1846">Floating-point remainder:</span></span>

   ```csharp
   float operator %(float x, float y);
   double operator %(double x, double y);
   ```

   <span data-ttu-id="dec56-1847">V následující tabulce jsou uvedeny výsledky všech možných kombinací nenulové hodnoty konečné, nul, nekonečno a NaN.</span><span class="sxs-lookup"><span data-stu-id="dec56-1847">The following table lists the results of all possible combinations of nonzero finite values, zeros, infinities, and NaN's.</span></span> <span data-ttu-id="dec56-1848">V tabulce `x` a `y` jsou konečné kladné hodnoty.</span><span class="sxs-lookup"><span data-stu-id="dec56-1848">In the table, `x` and `y` are positive finite values.</span></span> <span data-ttu-id="dec56-1849">`z` je výsledkem `x % y` a je vypočítán jako `x - n * y`, kde `n` je největší možné číslo, které je menší než nebo rovna hodnotě `x / y`.</span><span class="sxs-lookup"><span data-stu-id="dec56-1849">`z` is the result of `x % y` and is computed as `x - n * y`, where `n` is the largest possible integer that is less than or equal to `x / y`.</span></span> <span data-ttu-id="dec56-1850">Tato metoda výpočetních zbytek je obdobou, který používá pro celočíselné operandy, ale se liší od definice IEEE 754 (ve kterém `n` je celé číslo nejblíže `x / y`).</span><span class="sxs-lookup"><span data-stu-id="dec56-1850">This method of computing the remainder is analogous to that used for integer operands, but differs from the IEEE 754 definition (in which `n` is the integer closest to `x / y`).</span></span>

   |      |      |      |      |      |      |      |      |
   |:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|
   |      | <span data-ttu-id="dec56-1851">+ y</span><span class="sxs-lookup"><span data-stu-id="dec56-1851">+y</span></span>   | <span data-ttu-id="dec56-1852">-y</span><span class="sxs-lookup"><span data-stu-id="dec56-1852">-y</span></span>   | <span data-ttu-id="dec56-1853">+0</span><span class="sxs-lookup"><span data-stu-id="dec56-1853">+0</span></span>   | <span data-ttu-id="dec56-1854">-0</span><span class="sxs-lookup"><span data-stu-id="dec56-1854">-0</span></span>   | <span data-ttu-id="dec56-1855">+ inf</span><span class="sxs-lookup"><span data-stu-id="dec56-1855">+inf</span></span> | <span data-ttu-id="dec56-1856">-inf</span><span class="sxs-lookup"><span data-stu-id="dec56-1856">-inf</span></span> | <span data-ttu-id="dec56-1857">NaN</span><span class="sxs-lookup"><span data-stu-id="dec56-1857">NaN</span></span>  | 
   | <span data-ttu-id="dec56-1858">+x</span><span class="sxs-lookup"><span data-stu-id="dec56-1858">+x</span></span>   | <span data-ttu-id="dec56-1859">+z</span><span class="sxs-lookup"><span data-stu-id="dec56-1859">+z</span></span>   | <span data-ttu-id="dec56-1860">+z</span><span class="sxs-lookup"><span data-stu-id="dec56-1860">+z</span></span>   | <span data-ttu-id="dec56-1861">NaN</span><span class="sxs-lookup"><span data-stu-id="dec56-1861">NaN</span></span>  | <span data-ttu-id="dec56-1862">NaN</span><span class="sxs-lookup"><span data-stu-id="dec56-1862">NaN</span></span>  | <span data-ttu-id="dec56-1863">x</span><span class="sxs-lookup"><span data-stu-id="dec56-1863">x</span></span>    | <span data-ttu-id="dec56-1864">x</span><span class="sxs-lookup"><span data-stu-id="dec56-1864">x</span></span>    | <span data-ttu-id="dec56-1865">NaN</span><span class="sxs-lookup"><span data-stu-id="dec56-1865">NaN</span></span>  | 
   | <span data-ttu-id="dec56-1866">-x</span><span class="sxs-lookup"><span data-stu-id="dec56-1866">-x</span></span>   | <span data-ttu-id="dec56-1867">-z</span><span class="sxs-lookup"><span data-stu-id="dec56-1867">-z</span></span>   | <span data-ttu-id="dec56-1868">-z</span><span class="sxs-lookup"><span data-stu-id="dec56-1868">-z</span></span>   | <span data-ttu-id="dec56-1869">NaN</span><span class="sxs-lookup"><span data-stu-id="dec56-1869">NaN</span></span>  | <span data-ttu-id="dec56-1870">NaN</span><span class="sxs-lookup"><span data-stu-id="dec56-1870">NaN</span></span>  | <span data-ttu-id="dec56-1871">-x</span><span class="sxs-lookup"><span data-stu-id="dec56-1871">-x</span></span>   | <span data-ttu-id="dec56-1872">-x</span><span class="sxs-lookup"><span data-stu-id="dec56-1872">-x</span></span>   | <span data-ttu-id="dec56-1873">NaN</span><span class="sxs-lookup"><span data-stu-id="dec56-1873">NaN</span></span>  | 
   | <span data-ttu-id="dec56-1874">+0</span><span class="sxs-lookup"><span data-stu-id="dec56-1874">+0</span></span>   | <span data-ttu-id="dec56-1875">+0</span><span class="sxs-lookup"><span data-stu-id="dec56-1875">+0</span></span>   | <span data-ttu-id="dec56-1876">+0</span><span class="sxs-lookup"><span data-stu-id="dec56-1876">+0</span></span>   | <span data-ttu-id="dec56-1877">NaN</span><span class="sxs-lookup"><span data-stu-id="dec56-1877">NaN</span></span>  | <span data-ttu-id="dec56-1878">NaN</span><span class="sxs-lookup"><span data-stu-id="dec56-1878">NaN</span></span>  | <span data-ttu-id="dec56-1879">+0</span><span class="sxs-lookup"><span data-stu-id="dec56-1879">+0</span></span>   | <span data-ttu-id="dec56-1880">+0</span><span class="sxs-lookup"><span data-stu-id="dec56-1880">+0</span></span>   | <span data-ttu-id="dec56-1881">NaN</span><span class="sxs-lookup"><span data-stu-id="dec56-1881">NaN</span></span>  | 
   | <span data-ttu-id="dec56-1882">-0</span><span class="sxs-lookup"><span data-stu-id="dec56-1882">-0</span></span>   | <span data-ttu-id="dec56-1883">-0</span><span class="sxs-lookup"><span data-stu-id="dec56-1883">-0</span></span>   | <span data-ttu-id="dec56-1884">-0</span><span class="sxs-lookup"><span data-stu-id="dec56-1884">-0</span></span>   | <span data-ttu-id="dec56-1885">NaN</span><span class="sxs-lookup"><span data-stu-id="dec56-1885">NaN</span></span>  | <span data-ttu-id="dec56-1886">NaN</span><span class="sxs-lookup"><span data-stu-id="dec56-1886">NaN</span></span>  | <span data-ttu-id="dec56-1887">-0</span><span class="sxs-lookup"><span data-stu-id="dec56-1887">-0</span></span>   | <span data-ttu-id="dec56-1888">-0</span><span class="sxs-lookup"><span data-stu-id="dec56-1888">-0</span></span>   | <span data-ttu-id="dec56-1889">NaN</span><span class="sxs-lookup"><span data-stu-id="dec56-1889">NaN</span></span>  | 
   | <span data-ttu-id="dec56-1890">+ inf</span><span class="sxs-lookup"><span data-stu-id="dec56-1890">+inf</span></span> | <span data-ttu-id="dec56-1891">NaN</span><span class="sxs-lookup"><span data-stu-id="dec56-1891">NaN</span></span>  | <span data-ttu-id="dec56-1892">NaN</span><span class="sxs-lookup"><span data-stu-id="dec56-1892">NaN</span></span>  | <span data-ttu-id="dec56-1893">NaN</span><span class="sxs-lookup"><span data-stu-id="dec56-1893">NaN</span></span>  | <span data-ttu-id="dec56-1894">NaN</span><span class="sxs-lookup"><span data-stu-id="dec56-1894">NaN</span></span>  | <span data-ttu-id="dec56-1895">NaN</span><span class="sxs-lookup"><span data-stu-id="dec56-1895">NaN</span></span>  | <span data-ttu-id="dec56-1896">NaN</span><span class="sxs-lookup"><span data-stu-id="dec56-1896">NaN</span></span>  | <span data-ttu-id="dec56-1897">NaN</span><span class="sxs-lookup"><span data-stu-id="dec56-1897">NaN</span></span>  | 
   | <span data-ttu-id="dec56-1898">-inf</span><span class="sxs-lookup"><span data-stu-id="dec56-1898">-inf</span></span> | <span data-ttu-id="dec56-1899">NaN</span><span class="sxs-lookup"><span data-stu-id="dec56-1899">NaN</span></span>  | <span data-ttu-id="dec56-1900">NaN</span><span class="sxs-lookup"><span data-stu-id="dec56-1900">NaN</span></span>  | <span data-ttu-id="dec56-1901">NaN</span><span class="sxs-lookup"><span data-stu-id="dec56-1901">NaN</span></span>  | <span data-ttu-id="dec56-1902">NaN</span><span class="sxs-lookup"><span data-stu-id="dec56-1902">NaN</span></span>  | <span data-ttu-id="dec56-1903">NaN</span><span class="sxs-lookup"><span data-stu-id="dec56-1903">NaN</span></span>  | <span data-ttu-id="dec56-1904">NaN</span><span class="sxs-lookup"><span data-stu-id="dec56-1904">NaN</span></span>  | <span data-ttu-id="dec56-1905">NaN</span><span class="sxs-lookup"><span data-stu-id="dec56-1905">NaN</span></span>  | 
   | <span data-ttu-id="dec56-1906">NaN</span><span class="sxs-lookup"><span data-stu-id="dec56-1906">NaN</span></span>  | <span data-ttu-id="dec56-1907">NaN</span><span class="sxs-lookup"><span data-stu-id="dec56-1907">NaN</span></span>  | <span data-ttu-id="dec56-1908">NaN</span><span class="sxs-lookup"><span data-stu-id="dec56-1908">NaN</span></span>  | <span data-ttu-id="dec56-1909">NaN</span><span class="sxs-lookup"><span data-stu-id="dec56-1909">NaN</span></span>  | <span data-ttu-id="dec56-1910">NaN</span><span class="sxs-lookup"><span data-stu-id="dec56-1910">NaN</span></span>  | <span data-ttu-id="dec56-1911">NaN</span><span class="sxs-lookup"><span data-stu-id="dec56-1911">NaN</span></span>  | <span data-ttu-id="dec56-1912">NaN</span><span class="sxs-lookup"><span data-stu-id="dec56-1912">NaN</span></span>  | <span data-ttu-id="dec56-1913">NaN</span><span class="sxs-lookup"><span data-stu-id="dec56-1913">NaN</span></span>  | 

*  <span data-ttu-id="dec56-1914">Desetinné zbývající:</span><span class="sxs-lookup"><span data-stu-id="dec56-1914">Decimal remainder:</span></span>

   ```csharp
   decimal operator %(decimal x, decimal y);
   ```

   <span data-ttu-id="dec56-1915">Pokud hodnota pravého operandu je nula, `System.DivideByZeroException` je vyvolána výjimka.</span><span class="sxs-lookup"><span data-stu-id="dec56-1915">If the value of the right operand is zero, a `System.DivideByZeroException` is thrown.</span></span> <span data-ttu-id="dec56-1916">Škálování výsledek před všechny zaokrouhlení větší stupnic dva operandy, a znaménko výsledku platí, pokud nenulová, je stejné jako u `x`.</span><span class="sxs-lookup"><span data-stu-id="dec56-1916">The scale of the result, before any rounding, is the larger of the scales of the two operands, and the sign of the result, if non-zero, is the same as that of `x`.</span></span>

   <span data-ttu-id="dec56-1917">Je ekvivalentní k použití operátor zbytku typu Decimal zbytek `System.Decimal`.</span><span class="sxs-lookup"><span data-stu-id="dec56-1917">Decimal remainder is equivalent to using the remainder operator of type `System.Decimal`.</span></span>


### <a name="addition-operator"></a><span data-ttu-id="dec56-1918">Operátor sčítání</span><span class="sxs-lookup"><span data-stu-id="dec56-1918">Addition operator</span></span>

<span data-ttu-id="dec56-1919">Operace formuláře `x + y`, binárním operátorem rozlišení přetěžování ([binárním operátorem rozlišení přetěžování](expressions.md#binary-operator-overload-resolution)) se použije k výběru na konkrétní operátor implementace.</span><span class="sxs-lookup"><span data-stu-id="dec56-1919">For an operation of the form `x + y`, binary operator overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="dec56-1920">Operandy jsou převedeny na zvoleném operátorovi typy parametrů a typ výsledku je návratový typ operátoru.</span><span class="sxs-lookup"><span data-stu-id="dec56-1920">The operands are converted to the parameter types of the selected operator, and the type of the result is the return type of the operator.</span></span>

<span data-ttu-id="dec56-1921">Operátory sčítání předdefinované jsou uvedeny níže.</span><span class="sxs-lookup"><span data-stu-id="dec56-1921">The predefined addition operators are listed below.</span></span> <span data-ttu-id="dec56-1922">Operátory sčítání předdefinované číselné a výčet typů, nalezení součtu dva operandy.</span><span class="sxs-lookup"><span data-stu-id="dec56-1922">For numeric and enumeration types, the predefined addition operators compute the sum of the two operands.</span></span> <span data-ttu-id="dec56-1923">Pokud jeden nebo oba operandy jsou typu String, zřetězení operátory sčítání předdefinované řetězcové vyjádření operandy.</span><span class="sxs-lookup"><span data-stu-id="dec56-1923">When one or both operands are of type string, the predefined addition operators concatenate the string representation of the operands.</span></span>

*  <span data-ttu-id="dec56-1924">Přidání celé číslo:</span><span class="sxs-lookup"><span data-stu-id="dec56-1924">Integer addition:</span></span>

   ```csharp
   int operator +(int x, int y);
   uint operator +(uint x, uint y);
   long operator +(long x, long y);
   ulong operator +(ulong x, ulong y);
   ```

   <span data-ttu-id="dec56-1925">V `checked` kontextu, pokud součet je mimo rozsah typu výsledku, `System.OverflowException` je vyvolána výjimka.</span><span class="sxs-lookup"><span data-stu-id="dec56-1925">In a `checked` context, if the sum is outside the range of the result type, a `System.OverflowException` is thrown.</span></span> <span data-ttu-id="dec56-1926">V `unchecked` kontextu, nezobrazují, přetečení a žádné významné implikovaný bits mimo rozsah typu výsledku se zahodí.</span><span class="sxs-lookup"><span data-stu-id="dec56-1926">In an `unchecked` context, overflows are not reported and any significant high-order bits outside the range of the result type are discarded.</span></span>

*  <span data-ttu-id="dec56-1927">Přidání s plovoucí desetinnou čárkou:</span><span class="sxs-lookup"><span data-stu-id="dec56-1927">Floating-point addition:</span></span>

   ```csharp
   float operator +(float x, float y);
   double operator +(double x, double y);
   ```

   <span data-ttu-id="dec56-1928">Součet je vypočítán podle pravidel objektů aritmetické IEEE 754.</span><span class="sxs-lookup"><span data-stu-id="dec56-1928">The sum is computed according to the rules of IEEE 754 arithmetic.</span></span> <span data-ttu-id="dec56-1929">V následující tabulce jsou uvedeny výsledky všech možných kombinací nenulové hodnoty konečné, nul, nekonečno a NaN.</span><span class="sxs-lookup"><span data-stu-id="dec56-1929">The following table lists the results of all possible combinations of nonzero finite values, zeros, infinities, and NaN's.</span></span> <span data-ttu-id="dec56-1930">V tabulce `x` a `y` jsou omezené nenulové hodnoty, a `z` je výsledkem `x + y`.</span><span class="sxs-lookup"><span data-stu-id="dec56-1930">In the table, `x` and `y` are nonzero finite values, and `z` is the result of `x + y`.</span></span> <span data-ttu-id="dec56-1931">Pokud `x` a `y` mají stejnou velikost, ale opačným znaménka, `z` je kladné nula.</span><span class="sxs-lookup"><span data-stu-id="dec56-1931">If `x` and `y` have the same magnitude but opposite signs, `z` is positive zero.</span></span> <span data-ttu-id="dec56-1932">Pokud `x + y` je příliš velký pro reprezentaci typu cílového `z` je nekonečno s stejné znaménko jako `x + y`.</span><span class="sxs-lookup"><span data-stu-id="dec56-1932">If `x + y` is too large to represent in the destination type, `z` is an infinity with the same sign as `x + y`.</span></span>

   |      |      |      |      |      |      |      |
   |:----:|:----:|:----:|:----:|:----:|:----:|:----:|
   |      | <span data-ttu-id="dec56-1933">y</span><span class="sxs-lookup"><span data-stu-id="dec56-1933">y</span></span>    | <span data-ttu-id="dec56-1934">+0</span><span class="sxs-lookup"><span data-stu-id="dec56-1934">+0</span></span>   | <span data-ttu-id="dec56-1935">-0</span><span class="sxs-lookup"><span data-stu-id="dec56-1935">-0</span></span>   | <span data-ttu-id="dec56-1936">+ inf</span><span class="sxs-lookup"><span data-stu-id="dec56-1936">+inf</span></span> | <span data-ttu-id="dec56-1937">-inf</span><span class="sxs-lookup"><span data-stu-id="dec56-1937">-inf</span></span> | <span data-ttu-id="dec56-1938">NaN</span><span class="sxs-lookup"><span data-stu-id="dec56-1938">NaN</span></span>  | 
   | <span data-ttu-id="dec56-1939">x</span><span class="sxs-lookup"><span data-stu-id="dec56-1939">x</span></span>    | <span data-ttu-id="dec56-1940">z</span><span class="sxs-lookup"><span data-stu-id="dec56-1940">z</span></span>    | <span data-ttu-id="dec56-1941">x</span><span class="sxs-lookup"><span data-stu-id="dec56-1941">x</span></span>    | <span data-ttu-id="dec56-1942">x</span><span class="sxs-lookup"><span data-stu-id="dec56-1942">x</span></span>    | <span data-ttu-id="dec56-1943">+ inf</span><span class="sxs-lookup"><span data-stu-id="dec56-1943">+inf</span></span> | <span data-ttu-id="dec56-1944">-inf</span><span class="sxs-lookup"><span data-stu-id="dec56-1944">-inf</span></span> | <span data-ttu-id="dec56-1945">NaN</span><span class="sxs-lookup"><span data-stu-id="dec56-1945">NaN</span></span>  | 
   | <span data-ttu-id="dec56-1946">+0</span><span class="sxs-lookup"><span data-stu-id="dec56-1946">+0</span></span>   | <span data-ttu-id="dec56-1947">y</span><span class="sxs-lookup"><span data-stu-id="dec56-1947">y</span></span>    | <span data-ttu-id="dec56-1948">+0</span><span class="sxs-lookup"><span data-stu-id="dec56-1948">+0</span></span>   | <span data-ttu-id="dec56-1949">+0</span><span class="sxs-lookup"><span data-stu-id="dec56-1949">+0</span></span>   | <span data-ttu-id="dec56-1950">+ inf</span><span class="sxs-lookup"><span data-stu-id="dec56-1950">+inf</span></span> | <span data-ttu-id="dec56-1951">-inf</span><span class="sxs-lookup"><span data-stu-id="dec56-1951">-inf</span></span> | <span data-ttu-id="dec56-1952">NaN</span><span class="sxs-lookup"><span data-stu-id="dec56-1952">NaN</span></span>  | 
   | <span data-ttu-id="dec56-1953">-0</span><span class="sxs-lookup"><span data-stu-id="dec56-1953">-0</span></span>   | <span data-ttu-id="dec56-1954">y</span><span class="sxs-lookup"><span data-stu-id="dec56-1954">y</span></span>    | <span data-ttu-id="dec56-1955">+0</span><span class="sxs-lookup"><span data-stu-id="dec56-1955">+0</span></span>   | <span data-ttu-id="dec56-1956">-0</span><span class="sxs-lookup"><span data-stu-id="dec56-1956">-0</span></span>   | <span data-ttu-id="dec56-1957">+ inf</span><span class="sxs-lookup"><span data-stu-id="dec56-1957">+inf</span></span> | <span data-ttu-id="dec56-1958">-inf</span><span class="sxs-lookup"><span data-stu-id="dec56-1958">-inf</span></span> | <span data-ttu-id="dec56-1959">NaN</span><span class="sxs-lookup"><span data-stu-id="dec56-1959">NaN</span></span>  | 
   | <span data-ttu-id="dec56-1960">+ inf</span><span class="sxs-lookup"><span data-stu-id="dec56-1960">+inf</span></span> | <span data-ttu-id="dec56-1961">+ inf</span><span class="sxs-lookup"><span data-stu-id="dec56-1961">+inf</span></span> | <span data-ttu-id="dec56-1962">+ inf</span><span class="sxs-lookup"><span data-stu-id="dec56-1962">+inf</span></span> | <span data-ttu-id="dec56-1963">+ inf</span><span class="sxs-lookup"><span data-stu-id="dec56-1963">+inf</span></span> | <span data-ttu-id="dec56-1964">+ inf</span><span class="sxs-lookup"><span data-stu-id="dec56-1964">+inf</span></span> | <span data-ttu-id="dec56-1965">NaN</span><span class="sxs-lookup"><span data-stu-id="dec56-1965">NaN</span></span>  | <span data-ttu-id="dec56-1966">NaN</span><span class="sxs-lookup"><span data-stu-id="dec56-1966">NaN</span></span>  | 
   | <span data-ttu-id="dec56-1967">-inf</span><span class="sxs-lookup"><span data-stu-id="dec56-1967">-inf</span></span> | <span data-ttu-id="dec56-1968">-inf</span><span class="sxs-lookup"><span data-stu-id="dec56-1968">-inf</span></span> | <span data-ttu-id="dec56-1969">-inf</span><span class="sxs-lookup"><span data-stu-id="dec56-1969">-inf</span></span> | <span data-ttu-id="dec56-1970">-inf</span><span class="sxs-lookup"><span data-stu-id="dec56-1970">-inf</span></span> | <span data-ttu-id="dec56-1971">NaN</span><span class="sxs-lookup"><span data-stu-id="dec56-1971">NaN</span></span>  | <span data-ttu-id="dec56-1972">-inf</span><span class="sxs-lookup"><span data-stu-id="dec56-1972">-inf</span></span> | <span data-ttu-id="dec56-1973">NaN</span><span class="sxs-lookup"><span data-stu-id="dec56-1973">NaN</span></span>  | 
   | <span data-ttu-id="dec56-1974">NaN</span><span class="sxs-lookup"><span data-stu-id="dec56-1974">NaN</span></span>  | <span data-ttu-id="dec56-1975">NaN</span><span class="sxs-lookup"><span data-stu-id="dec56-1975">NaN</span></span>  | <span data-ttu-id="dec56-1976">NaN</span><span class="sxs-lookup"><span data-stu-id="dec56-1976">NaN</span></span>  | <span data-ttu-id="dec56-1977">NaN</span><span class="sxs-lookup"><span data-stu-id="dec56-1977">NaN</span></span>  | <span data-ttu-id="dec56-1978">NaN</span><span class="sxs-lookup"><span data-stu-id="dec56-1978">NaN</span></span>  | <span data-ttu-id="dec56-1979">NaN</span><span class="sxs-lookup"><span data-stu-id="dec56-1979">NaN</span></span>  | <span data-ttu-id="dec56-1980">NaN</span><span class="sxs-lookup"><span data-stu-id="dec56-1980">NaN</span></span>  | 

*  <span data-ttu-id="dec56-1981">Desetinné přidání:</span><span class="sxs-lookup"><span data-stu-id="dec56-1981">Decimal addition:</span></span>

   ```csharp
   decimal operator +(decimal x, decimal y);
   ```

   <span data-ttu-id="dec56-1982">Pokud výsledná hodnota je příliš velká pro reprezentaci v jazyce `decimal` formátu, `System.OverflowException` je vyvolána výjimka.</span><span class="sxs-lookup"><span data-stu-id="dec56-1982">If the resulting value is too large to represent in the `decimal` format, a `System.OverflowException` is thrown.</span></span> <span data-ttu-id="dec56-1983">Škálování výsledek před všechny zaokrouhlení, je větší stupnic dva operandy.</span><span class="sxs-lookup"><span data-stu-id="dec56-1983">The scale of the result, before any rounding, is the larger of the scales of the two operands.</span></span>

   <span data-ttu-id="dec56-1984">Je ekvivalentní k použití operátoru sčítání typu Decimal přidání `System.Decimal`.</span><span class="sxs-lookup"><span data-stu-id="dec56-1984">Decimal addition is equivalent to using the addition operator of type `System.Decimal`.</span></span>

*  <span data-ttu-id="dec56-1985">Přidání výčtu.</span><span class="sxs-lookup"><span data-stu-id="dec56-1985">Enumeration addition.</span></span> <span data-ttu-id="dec56-1986">Každý typ výčtu implicitně poskytuje následující předdefinované operátory, kde `E` je typ výčtu a `U` je základní typ `E`:</span><span class="sxs-lookup"><span data-stu-id="dec56-1986">Every enumeration type implicitly provides the following predefined operators, where `E` is the enum type, and `U` is the underlying type of `E`:</span></span>

   ```csharp
   E operator +(E x, U y);
   E operator +(U x, E y);
   ```

   <span data-ttu-id="dec56-1987">V době běhu těchto operátorů jsou vyhodnocovány stejným způsobem jako `(E)((U)x + (U)y)`.</span><span class="sxs-lookup"><span data-stu-id="dec56-1987">At run-time these operators are evaluated exactly as `(E)((U)x + (U)y)`.</span></span>

*  <span data-ttu-id="dec56-1988">Zřetězení řetězců:</span><span class="sxs-lookup"><span data-stu-id="dec56-1988">String concatenation:</span></span>

   ```csharp
   string operator +(string x, string y);
   string operator +(string x, object y);
   string operator +(object x, string y);
   ```

   <span data-ttu-id="dec56-1989">Tato přetížení binárního souboru `+` provádět operátor zřetězení řetězců.</span><span class="sxs-lookup"><span data-stu-id="dec56-1989">These overloads of the binary `+` operator perform string concatenation.</span></span> <span data-ttu-id="dec56-1990">Pokud je operand zřetězení řetězců `null`, je nahrazen prázdný řetězec.</span><span class="sxs-lookup"><span data-stu-id="dec56-1990">If an operand of string concatenation is `null`, an empty string is substituted.</span></span> <span data-ttu-id="dec56-1991">V opačném případě je některý argument jiné než řetězec převést na jeho řetězcovou reprezentaci vyvoláním virtuální `ToString` metody zděděné z typu `object`.</span><span class="sxs-lookup"><span data-stu-id="dec56-1991">Otherwise, any non-string argument is converted to its string representation by invoking the virtual `ToString` method inherited from type `object`.</span></span> <span data-ttu-id="dec56-1992">Pokud `ToString` vrátí `null`, je nahrazen prázdný řetězec.</span><span class="sxs-lookup"><span data-stu-id="dec56-1992">If `ToString` returns `null`, an empty string is substituted.</span></span>

   ```csharp
   using System;
   
   class Test
   {
       static void Main() {
           string s = null;
           Console.WriteLine("s = >" + s + "<");        // displays s = ><
           int i = 1;
           Console.WriteLine("i = " + i);               // displays i = 1
           float f = 1.2300E+15F;
           Console.WriteLine("f = " + f);               // displays f = 1.23E+15
           decimal d = 2.900m;
           Console.WriteLine("d = " + d);               // displays d = 2.900
       }
   }
   ```

   Výsledek operátoru pro zřetězení řetězců je řetězec, který se skládá ze znaků levý operand, následované znaky pravého operandu. Nikdy nevrátí operátoru pro zřetězení řetězců `null` hodnotu. <span data-ttu-id="dec56-1995">A `System.OutOfMemoryException` může být vyvolána, pokud není k dispozici dostatek paměti k přidělení výsledný řetězec.</span><span class="sxs-lookup"><span data-stu-id="dec56-1995">A `System.OutOfMemoryException` may be thrown if there is not enough memory available to allocate the resulting string.</span></span>

*  <span data-ttu-id="dec56-1996">Kombinaci delegátů.</span><span class="sxs-lookup"><span data-stu-id="dec56-1996">Delegate combination.</span></span> <span data-ttu-id="dec56-1997">Každý typ delegáta implicitně poskytuje následující předdefinovaný operátor, kde `D` je typ delegátu:</span><span class="sxs-lookup"><span data-stu-id="dec56-1997">Every delegate type implicitly provides the following predefined operator, where `D` is the delegate type:</span></span>

   ```csharp
   D operator +(D x, D y);
   ```

   <span data-ttu-id="dec56-1998">Binární soubor `+` operátor provádí delegátů, pokud jsou oba operandy typu delegáta `D`.</span><span class="sxs-lookup"><span data-stu-id="dec56-1998">The binary `+` operator performs delegate combination when both operands are of some delegate type `D`.</span></span> <span data-ttu-id="dec56-1999">(Pokud operandy různých delegáta typy, dojde k chybě vazby čas.) Pokud je první operand `null`, výsledkem operace je hodnota druhého operandu (i v případě, že to je také `null`).</span><span class="sxs-lookup"><span data-stu-id="dec56-1999">(If the operands have different delegate types, a binding-time error occurs.) If the first operand is `null`, the result of the operation is the value of the second operand (even if that is also `null`).</span></span> <span data-ttu-id="dec56-2000">Jinak, pokud je druhý operand `null`, pak výsledek operace hodnotu prvního operandu.</span><span class="sxs-lookup"><span data-stu-id="dec56-2000">Otherwise, if the second operand is `null`, then the result of the operation is the value of the first operand.</span></span> <span data-ttu-id="dec56-2001">Výsledek operace v opačném případě se nový delegát instance, která při vyvolání, vyvolá prvním operandem a poté vyvolá druhého operandu.</span><span class="sxs-lookup"><span data-stu-id="dec56-2001">Otherwise, the result of the operation is a new delegate instance that, when invoked, invokes the first operand and then invokes the second operand.</span></span> <span data-ttu-id="dec56-2002">Příklady delegátů, najdete v článku [operátor odčítání](expressions.md#subtraction-operator) a [delegovat vyvolání](delegates.md#delegate-invocation).</span><span class="sxs-lookup"><span data-stu-id="dec56-2002">For examples of delegate combination, see [Subtraction operator](expressions.md#subtraction-operator) and [Delegate invocation](delegates.md#delegate-invocation).</span></span> <span data-ttu-id="dec56-2003">Protože `System.Delegate` není typ delegáta, `operator`  `+` není definována.</span><span class="sxs-lookup"><span data-stu-id="dec56-2003">Since `System.Delegate` is not a delegate type, `operator` `+` is not defined for it.</span></span>

### <a name="subtraction-operator"></a><span data-ttu-id="dec56-2004">Operátor odčítání</span><span class="sxs-lookup"><span data-stu-id="dec56-2004">Subtraction operator</span></span>

<span data-ttu-id="dec56-2005">Operace formuláře `x - y`, binárním operátorem rozlišení přetěžování ([binárním operátorem rozlišení přetěžování](expressions.md#binary-operator-overload-resolution)) se použije k výběru na konkrétní operátor implementace.</span><span class="sxs-lookup"><span data-stu-id="dec56-2005">For an operation of the form `x - y`, binary operator overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="dec56-2006">Operandy jsou převedeny na zvoleném operátorovi typy parametrů a typ výsledku je návratový typ operátoru.</span><span class="sxs-lookup"><span data-stu-id="dec56-2006">The operands are converted to the parameter types of the selected operator, and the type of the result is the return type of the operator.</span></span>

<span data-ttu-id="dec56-2007">Předdefinované odčítání operátory jsou uvedeny níže.</span><span class="sxs-lookup"><span data-stu-id="dec56-2007">The predefined subtraction operators are listed below.</span></span> <span data-ttu-id="dec56-2008">Operátory všechny odečíst `y` z `x`.</span><span class="sxs-lookup"><span data-stu-id="dec56-2008">The operators all subtract `y` from `x`.</span></span>

*  <span data-ttu-id="dec56-2009">Odčítání celé číslo:</span><span class="sxs-lookup"><span data-stu-id="dec56-2009">Integer subtraction:</span></span>

   ```csharp
   int operator -(int x, int y);
   uint operator -(uint x, uint y);
   long operator -(long x, long y);
   ulong operator -(ulong x, ulong y);
   ```

   <span data-ttu-id="dec56-2010">V `checked` kontextu, pokud rozdíl je mimo rozsah typu výsledku, `System.OverflowException` je vyvolána výjimka.</span><span class="sxs-lookup"><span data-stu-id="dec56-2010">In a `checked` context, if the difference is outside the range of the result type, a `System.OverflowException` is thrown.</span></span> <span data-ttu-id="dec56-2011">V `unchecked` kontextu, nezobrazují, přetečení a žádné významné implikovaný bits mimo rozsah typu výsledku se zahodí.</span><span class="sxs-lookup"><span data-stu-id="dec56-2011">In an `unchecked` context, overflows are not reported and any significant high-order bits outside the range of the result type are discarded.</span></span>

*  <span data-ttu-id="dec56-2012">Odčítání s plovoucí desetinnou čárkou:</span><span class="sxs-lookup"><span data-stu-id="dec56-2012">Floating-point subtraction:</span></span>

   ```csharp
   float operator -(float x, float y);
   double operator -(double x, double y);
   ```

   <span data-ttu-id="dec56-2013">Rozdíl je vypočítán podle pravidel objektů aritmetické IEEE 754.</span><span class="sxs-lookup"><span data-stu-id="dec56-2013">The difference is computed according to the rules of IEEE 754 arithmetic.</span></span> <span data-ttu-id="dec56-2014">V následující tabulce jsou uvedeny výsledky všech možných kombinací nenulové hodnoty konečné, nul, nekonečno a hodnoty NaN.</span><span class="sxs-lookup"><span data-stu-id="dec56-2014">The following table lists the results of all possible combinations of nonzero finite values, zeros, infinities, and NaNs.</span></span> <span data-ttu-id="dec56-2015">V tabulce `x` a `y` jsou omezené nenulové hodnoty, a `z` je výsledkem `x - y`.</span><span class="sxs-lookup"><span data-stu-id="dec56-2015">In the table, `x` and `y` are nonzero finite values, and `z` is the result of `x - y`.</span></span> <span data-ttu-id="dec56-2016">Pokud `x` a `y` jsou si rovny, `z` je kladné nula.</span><span class="sxs-lookup"><span data-stu-id="dec56-2016">If `x` and `y` are equal, `z` is positive zero.</span></span> <span data-ttu-id="dec56-2017">Pokud `x - y` je příliš velký pro reprezentaci typu cílového `z` je nekonečno s stejné znaménko jako `x - y`.</span><span class="sxs-lookup"><span data-stu-id="dec56-2017">If `x - y` is too large to represent in the destination type, `z` is an infinity with the same sign as `x - y`.</span></span>

   |      |      |      |      |      |      |     |
   |:----:|:----:|:----:|:----:|:----:|:----:|:---:|
   |      | <span data-ttu-id="dec56-2018">y</span><span class="sxs-lookup"><span data-stu-id="dec56-2018">y</span></span>    | <span data-ttu-id="dec56-2019">+0</span><span class="sxs-lookup"><span data-stu-id="dec56-2019">+0</span></span>   | <span data-ttu-id="dec56-2020">-0</span><span class="sxs-lookup"><span data-stu-id="dec56-2020">-0</span></span>   | <span data-ttu-id="dec56-2021">+ inf</span><span class="sxs-lookup"><span data-stu-id="dec56-2021">+inf</span></span> | <span data-ttu-id="dec56-2022">-inf</span><span class="sxs-lookup"><span data-stu-id="dec56-2022">-inf</span></span> | <span data-ttu-id="dec56-2023">NaN</span><span class="sxs-lookup"><span data-stu-id="dec56-2023">NaN</span></span> | 
   | <span data-ttu-id="dec56-2024">x</span><span class="sxs-lookup"><span data-stu-id="dec56-2024">x</span></span>    | <span data-ttu-id="dec56-2025">z</span><span class="sxs-lookup"><span data-stu-id="dec56-2025">z</span></span>    | <span data-ttu-id="dec56-2026">x</span><span class="sxs-lookup"><span data-stu-id="dec56-2026">x</span></span>    | <span data-ttu-id="dec56-2027">x</span><span class="sxs-lookup"><span data-stu-id="dec56-2027">x</span></span>    | <span data-ttu-id="dec56-2028">-inf</span><span class="sxs-lookup"><span data-stu-id="dec56-2028">-inf</span></span> | <span data-ttu-id="dec56-2029">+ inf</span><span class="sxs-lookup"><span data-stu-id="dec56-2029">+inf</span></span> | <span data-ttu-id="dec56-2030">NaN</span><span class="sxs-lookup"><span data-stu-id="dec56-2030">NaN</span></span> | 
   | <span data-ttu-id="dec56-2031">+0</span><span class="sxs-lookup"><span data-stu-id="dec56-2031">+0</span></span>   | <span data-ttu-id="dec56-2032">-y</span><span class="sxs-lookup"><span data-stu-id="dec56-2032">-y</span></span>   | <span data-ttu-id="dec56-2033">+0</span><span class="sxs-lookup"><span data-stu-id="dec56-2033">+0</span></span>   | <span data-ttu-id="dec56-2034">+0</span><span class="sxs-lookup"><span data-stu-id="dec56-2034">+0</span></span>   | <span data-ttu-id="dec56-2035">-inf</span><span class="sxs-lookup"><span data-stu-id="dec56-2035">-inf</span></span> | <span data-ttu-id="dec56-2036">+ inf</span><span class="sxs-lookup"><span data-stu-id="dec56-2036">+inf</span></span> | <span data-ttu-id="dec56-2037">NaN</span><span class="sxs-lookup"><span data-stu-id="dec56-2037">NaN</span></span> | 
   | <span data-ttu-id="dec56-2038">-0</span><span class="sxs-lookup"><span data-stu-id="dec56-2038">-0</span></span>   | <span data-ttu-id="dec56-2039">-y</span><span class="sxs-lookup"><span data-stu-id="dec56-2039">-y</span></span>   | <span data-ttu-id="dec56-2040">-0</span><span class="sxs-lookup"><span data-stu-id="dec56-2040">-0</span></span>   | <span data-ttu-id="dec56-2041">+0</span><span class="sxs-lookup"><span data-stu-id="dec56-2041">+0</span></span>   | <span data-ttu-id="dec56-2042">-inf</span><span class="sxs-lookup"><span data-stu-id="dec56-2042">-inf</span></span> | <span data-ttu-id="dec56-2043">+ inf</span><span class="sxs-lookup"><span data-stu-id="dec56-2043">+inf</span></span> | <span data-ttu-id="dec56-2044">NaN</span><span class="sxs-lookup"><span data-stu-id="dec56-2044">NaN</span></span> | 
   | <span data-ttu-id="dec56-2045">+ inf</span><span class="sxs-lookup"><span data-stu-id="dec56-2045">+inf</span></span> | <span data-ttu-id="dec56-2046">+ inf</span><span class="sxs-lookup"><span data-stu-id="dec56-2046">+inf</span></span> | <span data-ttu-id="dec56-2047">+ inf</span><span class="sxs-lookup"><span data-stu-id="dec56-2047">+inf</span></span> | <span data-ttu-id="dec56-2048">+ inf</span><span class="sxs-lookup"><span data-stu-id="dec56-2048">+inf</span></span> | <span data-ttu-id="dec56-2049">NaN</span><span class="sxs-lookup"><span data-stu-id="dec56-2049">NaN</span></span>  | <span data-ttu-id="dec56-2050">+ inf</span><span class="sxs-lookup"><span data-stu-id="dec56-2050">+inf</span></span> | <span data-ttu-id="dec56-2051">NaN</span><span class="sxs-lookup"><span data-stu-id="dec56-2051">NaN</span></span> | 
   | <span data-ttu-id="dec56-2052">-inf</span><span class="sxs-lookup"><span data-stu-id="dec56-2052">-inf</span></span> | <span data-ttu-id="dec56-2053">-inf</span><span class="sxs-lookup"><span data-stu-id="dec56-2053">-inf</span></span> | <span data-ttu-id="dec56-2054">-inf</span><span class="sxs-lookup"><span data-stu-id="dec56-2054">-inf</span></span> | <span data-ttu-id="dec56-2055">-inf</span><span class="sxs-lookup"><span data-stu-id="dec56-2055">-inf</span></span> | <span data-ttu-id="dec56-2056">-inf</span><span class="sxs-lookup"><span data-stu-id="dec56-2056">-inf</span></span> | <span data-ttu-id="dec56-2057">NaN</span><span class="sxs-lookup"><span data-stu-id="dec56-2057">NaN</span></span>  | <span data-ttu-id="dec56-2058">NaN</span><span class="sxs-lookup"><span data-stu-id="dec56-2058">NaN</span></span> | 
   | <span data-ttu-id="dec56-2059">NaN</span><span class="sxs-lookup"><span data-stu-id="dec56-2059">NaN</span></span>  | <span data-ttu-id="dec56-2060">NaN</span><span class="sxs-lookup"><span data-stu-id="dec56-2060">NaN</span></span>  | <span data-ttu-id="dec56-2061">NaN</span><span class="sxs-lookup"><span data-stu-id="dec56-2061">NaN</span></span>  | <span data-ttu-id="dec56-2062">NaN</span><span class="sxs-lookup"><span data-stu-id="dec56-2062">NaN</span></span>  | <span data-ttu-id="dec56-2063">NaN</span><span class="sxs-lookup"><span data-stu-id="dec56-2063">NaN</span></span>  | <span data-ttu-id="dec56-2064">NaN</span><span class="sxs-lookup"><span data-stu-id="dec56-2064">NaN</span></span>  | <span data-ttu-id="dec56-2065">NaN</span><span class="sxs-lookup"><span data-stu-id="dec56-2065">NaN</span></span> | 

*  <span data-ttu-id="dec56-2066">Desetinné odčítání:</span><span class="sxs-lookup"><span data-stu-id="dec56-2066">Decimal subtraction:</span></span>

   ```csharp
   decimal operator -(decimal x, decimal y);
   ```

   <span data-ttu-id="dec56-2067">Pokud výsledná hodnota je příliš velká pro reprezentaci v jazyce `decimal` formátu, `System.OverflowException` je vyvolána výjimka.</span><span class="sxs-lookup"><span data-stu-id="dec56-2067">If the resulting value is too large to represent in the `decimal` format, a `System.OverflowException` is thrown.</span></span> <span data-ttu-id="dec56-2068">Škálování výsledek před všechny zaokrouhlení, je větší stupnic dva operandy.</span><span class="sxs-lookup"><span data-stu-id="dec56-2068">The scale of the result, before any rounding, is the larger of the scales of the two operands.</span></span>

   <span data-ttu-id="dec56-2069">Je ekvivalentní k použití operátoru odčítání typu Decimal odčítání `System.Decimal`.</span><span class="sxs-lookup"><span data-stu-id="dec56-2069">Decimal subtraction is equivalent to using the subtraction operator of type `System.Decimal`.</span></span>

*  <span data-ttu-id="dec56-2070">Výčet odčítání.</span><span class="sxs-lookup"><span data-stu-id="dec56-2070">Enumeration subtraction.</span></span> <span data-ttu-id="dec56-2071">Každý typ výčtu implicitně poskytuje následující předdefinovaný operátor, kde `E` je typ výčtu a `U` je základní typ `E`:</span><span class="sxs-lookup"><span data-stu-id="dec56-2071">Every enumeration type implicitly provides the following predefined operator, where `E` is the enum type, and `U` is the underlying type of `E`:</span></span>

   ```csharp
   U operator -(E x, E y);
   ```

   <span data-ttu-id="dec56-2072">Tento operátor se vyhodnotí přesně jako `(U)((U)x - (U)y)`.</span><span class="sxs-lookup"><span data-stu-id="dec56-2072">This operator is evaluated exactly as `(U)((U)x - (U)y)`.</span></span> <span data-ttu-id="dec56-2073">Jinými slovy, operátor, který vypočítá rozdíl mezi pořadové hodnoty `x` a `y`, a typ výsledku je základní typ výčtu.</span><span class="sxs-lookup"><span data-stu-id="dec56-2073">In other words, the operator computes the difference between the ordinal values of `x` and `y`, and the type of the result is the underlying type of the enumeration.</span></span>

   ```csharp
   E operator -(E x, U y);
   ```

   <span data-ttu-id="dec56-2074">Tento operátor se vyhodnotí přesně jako `(E)((U)x - y)`.</span><span class="sxs-lookup"><span data-stu-id="dec56-2074">This operator is evaluated exactly as `(E)((U)x - y)`.</span></span> <span data-ttu-id="dec56-2075">Jinými slovy operátor, který odečte hodnotu od základní typ výčtu, což má za následek hodnotu výčtu.</span><span class="sxs-lookup"><span data-stu-id="dec56-2075">In other words, the operator subtracts a value from the underlying type of the enumeration, yielding a value of the enumeration.</span></span>

*  <span data-ttu-id="dec56-2076">Odebrání delegovat.</span><span class="sxs-lookup"><span data-stu-id="dec56-2076">Delegate removal.</span></span> <span data-ttu-id="dec56-2077">Každý typ delegáta implicitně poskytuje následující předdefinovaný operátor, kde `D` je typ delegátu:</span><span class="sxs-lookup"><span data-stu-id="dec56-2077">Every delegate type implicitly provides the following predefined operator, where `D` is the delegate type:</span></span>

   ```csharp
   D operator -(D x, D y);
   ```

   <span data-ttu-id="dec56-2078">Binární soubor `-` operátor provádí odebrání delegátů, pokud jsou oba operandy typu delegáta `D`.</span><span class="sxs-lookup"><span data-stu-id="dec56-2078">The binary `-` operator performs delegate removal when both operands are of some delegate type `D`.</span></span> <span data-ttu-id="dec56-2079">Pokud operandy typů různých delegátů, dojde k chybě vazby čas.</span><span class="sxs-lookup"><span data-stu-id="dec56-2079">If the operands have different delegate types, a binding-time error occurs.</span></span> <span data-ttu-id="dec56-2080">Pokud je první operand `null`, je výsledek operace `null`.</span><span class="sxs-lookup"><span data-stu-id="dec56-2080">If the first operand is `null`, the result of the operation is `null`.</span></span> <span data-ttu-id="dec56-2081">Jinak, pokud je druhý operand `null`, pak výsledek operace hodnotu prvního operandu.</span><span class="sxs-lookup"><span data-stu-id="dec56-2081">Otherwise, if the second operand is `null`, then the result of the operation is the value of the first operand.</span></span> <span data-ttu-id="dec56-2082">V opačném případě oba operandy představovat vyvolání seznamy ([delegovat deklarace](delegates.md#delegate-declarations)) má jednu nebo více položek a výsledkem je nový seznam vyvolání skládající se z prvního operandu seznam s položkami druhého operandu odebrán z je k dispozici seznam Druhý operand je správné souvislých podseznam při prvním.</span><span class="sxs-lookup"><span data-stu-id="dec56-2082">Otherwise, both operands represent invocation lists ([Delegate declarations](delegates.md#delegate-declarations)) having one or more entries, and the result is a new invocation list consisting of the first operand's list with the second operand's entries removed from it, provided the second operand's list is a proper contiguous sublist of the first's.</span></span>     <span data-ttu-id="dec56-2083">(K určení rovnosti podseznam, jsou porovnány odpovídající položky jako operátor rovnosti delegáta ([delegovat operátory rovnosti](expressions.md#delegate-equality-operators)).) Jinak výsledkem je hodnota levého operandu.</span><span class="sxs-lookup"><span data-stu-id="dec56-2083">(To determine sublist equality, corresponding entries are compared as for the delegate equality operator ([Delegate equality operators](expressions.md#delegate-equality-operators)).) Otherwise, the result is the value of the left operand.</span></span> <span data-ttu-id="dec56-2084">Ani jeden z operandů Zobrazí se změnilo v procesu.</span><span class="sxs-lookup"><span data-stu-id="dec56-2084">Neither of the operands' lists is changed in the process.</span></span> <span data-ttu-id="dec56-2085">Pokud je druhý operand seznamu odpovídá několika dílčích souvislých položky v seznamu je první operand, odpovídající dílčí seznam úplně vpravo souvislých položek, které se odebere.</span><span class="sxs-lookup"><span data-stu-id="dec56-2085">If the second operand's list matches multiple sublists of contiguous entries in the first operand's list, the right-most matching sublist of contiguous entries is removed.</span></span> <span data-ttu-id="dec56-2086">Pokud je odebrání výsledkem je seznam prázdný, výsledkem je `null`.</span><span class="sxs-lookup"><span data-stu-id="dec56-2086">If removal results in an empty list, the result is `null`.</span></span> <span data-ttu-id="dec56-2087">Příklad:</span><span class="sxs-lookup"><span data-stu-id="dec56-2087">For example:</span></span>

   ```csharp
   delegate void D(int x);
   
   class C
   {
       public static void M1(int i) { /* ... */ }
       public static void M2(int i) { /* ... */ }
   }

   class Test
   {
       static void Main() { 
           D cd1 = new D(C.M1);
           D cd2 = new D(C.M2);
           D cd3 = cd1 + cd2 + cd2 + cd1;   // M1 + M2 + M2 + M1
           cd3 -= cd1;                      // => M1 + M2 + M2
   
           cd3 = cd1 + cd2 + cd2 + cd1;     // M1 + M2 + M2 + M1
           cd3 -= cd1 + cd2;                // => M2 + M1
   
           cd3 = cd1 + cd2 + cd2 + cd1;     // M1 + M2 + M2 + M1
           cd3 -= cd2 + cd2;                // => M1 + M1
   
           cd3 = cd1 + cd2 + cd2 + cd1;     // M1 + M2 + M2 + M1
           cd3 -= cd2 + cd1;                // => M1 + M2
   
           cd3 = cd1 + cd2 + cd2 + cd1;     // M1 + M2 + M2 + M1
           cd3 -= cd1 + cd1;                // => M1 + M2 + M2 + M1
       }
   }
   ```

## <a name="shift-operators"></a><span data-ttu-id="dec56-2088">Operátory posunutí</span><span class="sxs-lookup"><span data-stu-id="dec56-2088">Shift operators</span></span>

<span data-ttu-id="dec56-2089">`<<` a `>>` operátory jsou používány k provádění operací posunu bit.</span><span class="sxs-lookup"><span data-stu-id="dec56-2089">The `<<` and `>>` operators are used to perform bit shifting operations.</span></span>

```antlr
shift_expression
    : additive_expression
    | shift_expression '<<' additive_expression
    | shift_expression right_shift additive_expression
    ;
```

<span data-ttu-id="dec56-2090">Pokud operand operátoru *shift_expression* má typ kompilace `dynamic`, a dynamicky je vázán výraz ([dynamické vazby](expressions.md#dynamic-binding)).</span><span class="sxs-lookup"><span data-stu-id="dec56-2090">If an operand of a *shift_expression* has the compile-time type `dynamic`, then the expression is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)).</span></span> <span data-ttu-id="dec56-2091">V tomto případě je kompilaci typu výrazu `dynamic`, a rozlišení je popsáno níže se provede v době běhu pomocí run-time typu těchto operandů, která mají typ kompilace `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="dec56-2091">In this case the compile-time type of the expression is `dynamic`, and the resolution described below will take place at run-time using the run-time type of those operands that have the compile-time type `dynamic`.</span></span>

<span data-ttu-id="dec56-2092">Operace formuláře `x << count` nebo `x >> count`, binárním operátorem rozlišení přetěžování ([binárním operátorem rozlišení přetěžování](expressions.md#binary-operator-overload-resolution)) se použije k výběru na konkrétní operátor implementace.</span><span class="sxs-lookup"><span data-stu-id="dec56-2092">For an operation of the form `x << count` or `x >> count`, binary operator overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="dec56-2093">Operandy jsou převedeny na zvoleném operátorovi typy parametrů a typ výsledku je návratový typ operátoru.</span><span class="sxs-lookup"><span data-stu-id="dec56-2093">The operands are converted to the parameter types of the selected operator, and the type of the result is the return type of the operator.</span></span>

<span data-ttu-id="dec56-2094">Při deklarování přetěžovaného operátoru shift, typ je první operand musí být vždy dané třídy nebo struktury obsahující deklarace operátoru a typu Druhý operand musí být vždy `int`.</span><span class="sxs-lookup"><span data-stu-id="dec56-2094">When declaring an overloaded shift operator, the type of the first operand must always be the class or struct containing the operator declaration, and the type of the second operand must always be `int`.</span></span>

<span data-ttu-id="dec56-2095">Operátory posunutí předdefinované jsou uvedeny níže.</span><span class="sxs-lookup"><span data-stu-id="dec56-2095">The predefined shift operators are listed below.</span></span>

*  <span data-ttu-id="dec56-2096">Posun doleva:</span><span class="sxs-lookup"><span data-stu-id="dec56-2096">Shift left:</span></span>

   ```csharp
   int operator <<(int x, int count);
   uint operator <<(uint x, int count);
   long operator <<(long x, int count);
   ulong operator <<(ulong x, int count);
   ```

   <span data-ttu-id="dec56-2097">`<<` Operátor staffhubu `x` doleva o počet bitů vypočítat, jak je popsáno níže.</span><span class="sxs-lookup"><span data-stu-id="dec56-2097">The `<<` operator shifts `x` left by a number of bits computed as described below.</span></span>

   <span data-ttu-id="dec56-2098">Nejvyšším bits mimo rozsah typu výsledku `x` jsou zahozeny, zbývající bity posunuty vlevo a nižšího řádu prázdný bitové pozice jsou nastaveny na hodnotu nula.</span><span class="sxs-lookup"><span data-stu-id="dec56-2098">The high-order bits outside the range of the result type of `x` are discarded, the remaining bits are shifted left, and the low-order empty bit positions are set to zero.</span></span>

*  <span data-ttu-id="dec56-2099">Posun doprava:</span><span class="sxs-lookup"><span data-stu-id="dec56-2099">Shift right:</span></span>

   ```csharp
   int operator >>(int x, int count);
   uint operator >>(uint x, int count);
   long operator >>(long x, int count);
   ulong operator >>(ulong x, int count);
   ```

   <span data-ttu-id="dec56-2100">`>>` Operátor staffhubu `x` doprava o počet bitů vypočítat, jak je popsáno níže.</span><span class="sxs-lookup"><span data-stu-id="dec56-2100">The `>>` operator shifts `x` right by a number of bits computed as described below.</span></span>

   <span data-ttu-id="dec56-2101">Při `x` je typu `int` nebo `long`, bity nižšího řádu `x` jsou zahozeny, zbývající bity jsou posunuta doprava a implikovaný prázdný bitové pozice jsou nastaveny na nula, pokud `x` je nastaven na nezáporné a nastavte, pokud jeden `x` je záporný.</span><span class="sxs-lookup"><span data-stu-id="dec56-2101">When `x` is of type `int` or `long`, the low-order bits of `x` are discarded, the remaining bits are shifted right, and the high-order empty bit positions are set to zero if `x` is non-negative and set to one if `x` is negative.</span></span>

   <span data-ttu-id="dec56-2102">Když `x` je typu `uint` nebo `ulong`, bity nižšího řádu `x` jsou zahozeny, jsou zbývající bity posunuta doprava a implikovaný prázdný bitové pozice jsou nastaveny na hodnotu nula.</span><span class="sxs-lookup"><span data-stu-id="dec56-2102">When `x` is of type `uint` or `ulong`, the low-order bits of `x` are discarded, the remaining bits are shifted right, and the high-order empty bit positions are set to zero.</span></span>

<span data-ttu-id="dec56-2103">Pro předdefinované operátory počet bitů, chcete-li posunout vypočítává následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="dec56-2103">For the predefined operators, the number of bits to shift is computed as follows:</span></span>

*  <span data-ttu-id="dec56-2104">Pokud typ `x` je `int` nebo `uint`, počet posunů je dán pět bity nižšího řádu `count`.</span><span class="sxs-lookup"><span data-stu-id="dec56-2104">When the type of `x` is `int` or `uint`, the shift count is given by the low-order five bits of `count`.</span></span> <span data-ttu-id="dec56-2105">Jinými slovy, počet posunů je vypočítán z `count & 0x1F`.</span><span class="sxs-lookup"><span data-stu-id="dec56-2105">In other words, the shift count is computed from `count & 0x1F`.</span></span>
*  <span data-ttu-id="dec56-2106">Pokud typ `x` je `long` nebo `ulong`, počet posunů je dán šest bity nižšího řádu `count`.</span><span class="sxs-lookup"><span data-stu-id="dec56-2106">When the type of `x` is `long` or `ulong`, the shift count is given by the low-order six bits of `count`.</span></span> <span data-ttu-id="dec56-2107">Jinými slovy, počet posunů je vypočítán z `count & 0x3F`.</span><span class="sxs-lookup"><span data-stu-id="dec56-2107">In other words, the shift count is computed from `count & 0x3F`.</span></span>

<span data-ttu-id="dec56-2108">Pokud výsledný počet posunů je nula, operátory posunutí jednoduše vrátit hodnotu `x`.</span><span class="sxs-lookup"><span data-stu-id="dec56-2108">If the resulting shift count is zero, the shift operators simply return the value of `x`.</span></span>

<span data-ttu-id="dec56-2109">Operace posunutí nikdy způsobit přetečení a produkuje stejné výsledky v `checked` a `unchecked` kontexty.</span><span class="sxs-lookup"><span data-stu-id="dec56-2109">Shift operations never cause overflows and produce the same results in `checked` and `unchecked` contexts.</span></span>

<span data-ttu-id="dec56-2110">Pokud levý operand `>>` operátor je celočíselný typ se znaménkem, operátor, který provede aritmetický posun přímo ve které se šíří výhody nejvýznamnější bit (bit znaménka) operand na vyšší řád prázdný bitové pozice.</span><span class="sxs-lookup"><span data-stu-id="dec56-2110">When the left operand of the `>>` operator is of a signed integral type, the operator performs an arithmetic shift right wherein the value of the most significant bit (the sign bit) of the operand is propagated to the high-order empty bit positions.</span></span> <span data-ttu-id="dec56-2111">Pokud levý operand `>>` operátor je celočíselný typ bez znaménka, operátor, který provádí logický posun přímo ve které implikovaný prázdný bitové pozice jsou vždy nastaveny na hodnotu nula.</span><span class="sxs-lookup"><span data-stu-id="dec56-2111">When the left operand of the `>>` operator is of an unsigned integral type, the operator performs a logical shift right wherein high-order empty bit positions are always set to zero.</span></span> <span data-ttu-id="dec56-2112">Pokud chcete provést opačný operace, které odvozen z typu operandu, je možné explicitní přetypování.</span><span class="sxs-lookup"><span data-stu-id="dec56-2112">To perform the opposite operation of that inferred from the operand type, explicit casts can be used.</span></span> <span data-ttu-id="dec56-2113">Například pokud `x` je proměnná typu `int`, operace `unchecked((int)((uint)x >> y))` provede logický Posun vpravo `x`.</span><span class="sxs-lookup"><span data-stu-id="dec56-2113">For example, if `x` is a variable of type `int`, the operation `unchecked((int)((uint)x >> y))` performs a logical shift right of `x`.</span></span>

## <a name="relational-and-type-testing-operators"></a><span data-ttu-id="dec56-2114">Operátory relační a typové zkoušky</span><span class="sxs-lookup"><span data-stu-id="dec56-2114">Relational and type-testing operators</span></span>

<span data-ttu-id="dec56-2115">`==`, `!=`, `<`, `>`, `<=`, `>=`, `is` a `as` operátory jsou volány operátory relační a typové zkoušky.</span><span class="sxs-lookup"><span data-stu-id="dec56-2115">The `==`, `!=`, `<`, `>`, `<=`, `>=`, `is` and `as` operators are called the relational and type-testing operators.</span></span>

```antlr
relational_expression
    : shift_expression
    | relational_expression '<' shift_expression
    | relational_expression '>' shift_expression
    | relational_expression '<=' shift_expression
    | relational_expression '>=' shift_expression
    | relational_expression 'is' type
    | relational_expression 'as' type
    ;

equality_expression
    : relational_expression
    | equality_expression '==' relational_expression
    | equality_expression '!=' relational_expression
    ;
```

<span data-ttu-id="dec56-2116">`is` Operátor je popsána v [je operátor](expressions.md#the-is-operator) a `as` operátor je popsána v [operátor as](expressions.md#the-as-operator).</span><span class="sxs-lookup"><span data-stu-id="dec56-2116">The `is` operator is described in [The is operator](expressions.md#the-is-operator) and the `as` operator is described in [The as operator](expressions.md#the-as-operator).</span></span>

<span data-ttu-id="dec56-2117">`==`, `!=`, `<`, `>`, `<=` a `>=` operátory jsou ***operátory porovnání***.</span><span class="sxs-lookup"><span data-stu-id="dec56-2117">The `==`, `!=`, `<`, `>`, `<=` and `>=` operators are ***comparison operators***.</span></span>

<span data-ttu-id="dec56-2118">Pokud má operand operátoru porovnání typů za kompilace `dynamic`, a dynamicky je vázán výraz ([dynamické vazby](expressions.md#dynamic-binding)).</span><span class="sxs-lookup"><span data-stu-id="dec56-2118">If an operand of a comparison operator has the compile-time type `dynamic`, then the expression is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)).</span></span> <span data-ttu-id="dec56-2119">V tomto případě je kompilaci typu výrazu `dynamic`, a rozlišení je popsáno níže se provede v době běhu pomocí run-time typu těchto operandů, která mají typ kompilace `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="dec56-2119">In this case the compile-time type of the expression is `dynamic`, and the resolution described below will take place at run-time using the run-time type of those operands that have the compile-time type `dynamic`.</span></span>

<span data-ttu-id="dec56-2120">Operace formuláře `x` *op* `y`, kde *op* je relační operátor rozlišení přetížení ([binárním operátorem rozlišenípřetěžování](expressions.md#binary-operator-overload-resolution)) se použije k výběru na konkrétní operátor implementace.</span><span class="sxs-lookup"><span data-stu-id="dec56-2120">For an operation of the form `x` *op* `y`, where *op* is a comparison operator, overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="dec56-2121">Operandy jsou převedeny na zvoleném operátorovi typy parametrů a typ výsledku je návratový typ operátoru.</span><span class="sxs-lookup"><span data-stu-id="dec56-2121">The operands are converted to the parameter types of the selected operator, and the type of the result is the return type of the operator.</span></span>

<span data-ttu-id="dec56-2122">Operátory předdefinované porovnání jsou popsány v následujících částech.</span><span class="sxs-lookup"><span data-stu-id="dec56-2122">The predefined comparison operators are described in the following sections.</span></span> <span data-ttu-id="dec56-2123">Všechny předdefinované porovnávací operátory vrací výsledek typu `bool`, jak je popsáno v následující tabulce.</span><span class="sxs-lookup"><span data-stu-id="dec56-2123">All predefined comparison operators return a result of type `bool`, as described in the following table.</span></span>


| <span data-ttu-id="dec56-2124">__Operace__</span><span class="sxs-lookup"><span data-stu-id="dec56-2124">__Operation__</span></span> | <span data-ttu-id="dec56-2125">__výsledek__</span><span class="sxs-lookup"><span data-stu-id="dec56-2125">__Result__</span></span>                                                       |
|---------------|------------------------------------------------------------------|
| `x == y`      | <span data-ttu-id="dec56-2126">`true` Pokud `x` rovná `y`, `false` jinak</span><span class="sxs-lookup"><span data-stu-id="dec56-2126">`true` if `x` is equal to `y`, `false` otherwise</span></span>                 | 
| `x != y`      | <span data-ttu-id="dec56-2127">`true` Pokud `x` není roven `y`, `false` jinak</span><span class="sxs-lookup"><span data-stu-id="dec56-2127">`true` if `x` is not equal to `y`, `false` otherwise</span></span>             | 
| `x < y`       | <span data-ttu-id="dec56-2128">`true` Pokud `x` je menší než `y`, `false` jinak</span><span class="sxs-lookup"><span data-stu-id="dec56-2128">`true` if `x` is less than `y`, `false` otherwise</span></span>                | 
| `x > y`       | <span data-ttu-id="dec56-2129">`true` Pokud `x` je větší než `y`, `false` jinak</span><span class="sxs-lookup"><span data-stu-id="dec56-2129">`true` if `x` is greater than `y`, `false` otherwise</span></span>             | 
| `x <= y`      | <span data-ttu-id="dec56-2130">`true` Pokud `x` je menší než nebo rovna hodnotě `y`, `false` jinak</span><span class="sxs-lookup"><span data-stu-id="dec56-2130">`true` if `x` is less than or equal to `y`, `false` otherwise</span></span>    | 
| `x >= y`      | <span data-ttu-id="dec56-2131">`true` Pokud `x` je větší než nebo rovna hodnotě `y`, `false` jinak</span><span class="sxs-lookup"><span data-stu-id="dec56-2131">`true` if `x` is greater than or equal to `y`, `false` otherwise</span></span> | 

### <a name="integer-comparison-operators"></a><span data-ttu-id="dec56-2132">Operátory porovnání celé číslo</span><span class="sxs-lookup"><span data-stu-id="dec56-2132">Integer comparison operators</span></span>

<span data-ttu-id="dec56-2133">Celé číslo předdefinované operátory porovnání jsou:</span><span class="sxs-lookup"><span data-stu-id="dec56-2133">The predefined integer comparison operators are:</span></span>
```csharp
bool operator ==(int x, int y);
bool operator ==(uint x, uint y);
bool operator ==(long x, long y);
bool operator ==(ulong x, ulong y);

bool operator !=(int x, int y);
bool operator !=(uint x, uint y);
bool operator !=(long x, long y);
bool operator !=(ulong x, ulong y);

bool operator <(int x, int y);
bool operator <(uint x, uint y);
bool operator <(long x, long y);
bool operator <(ulong x, ulong y);

bool operator >(int x, int y);
bool operator >(uint x, uint y);
bool operator >(long x, long y);
bool operator >(ulong x, ulong y);

bool operator <=(int x, int y);
bool operator <=(uint x, uint y);
bool operator <=(long x, long y);
bool operator <=(ulong x, ulong y);

bool operator >=(int x, int y);
bool operator >=(uint x, uint y);
bool operator >=(long x, long y);
bool operator >=(ulong x, ulong y);
```

<span data-ttu-id="dec56-2134">Každý z těchto operátorů porovnání číselných hodnot dvě celočíselné operandy a vrátí `bool` hodnotu, která určuje, zda je konkrétní vztah `true` nebo `false`.</span><span class="sxs-lookup"><span data-stu-id="dec56-2134">Each of these operators compares the numeric values of the two integer operands and returns a `bool` value that indicates whether the particular relation is `true` or `false`.</span></span>

### <a name="floating-point-comparison-operators"></a><span data-ttu-id="dec56-2135">Operátory porovnání s plovoucí desetinnou čárkou</span><span class="sxs-lookup"><span data-stu-id="dec56-2135">Floating-point comparison operators</span></span>

<span data-ttu-id="dec56-2136">Předdefinované s plovoucí desetinnou čárkou porovnávací operátory jsou:</span><span class="sxs-lookup"><span data-stu-id="dec56-2136">The predefined floating-point comparison operators are:</span></span>
```csharp
bool operator ==(float x, float y);
bool operator ==(double x, double y);

bool operator !=(float x, float y);
bool operator !=(double x, double y);

bool operator <(float x, float y);
bool operator <(double x, double y);

bool operator >(float x, float y);
bool operator >(double x, double y);

bool operator <=(float x, float y);
bool operator <=(double x, double y);

bool operator >=(float x, float y);
bool operator >=(double x, double y);
```

<span data-ttu-id="dec56-2137">Operátory porovnání operandy pravidlům standardní IEEE 754:</span><span class="sxs-lookup"><span data-stu-id="dec56-2137">The operators compare the operands according to the rules of the IEEE 754 standard:</span></span>

*  <span data-ttu-id="dec56-2138">Pokud některý operand je NaN, výsledkem je `false` pro všechny operátory s výjimkou `!=`, pro který je výsledkem `true`.</span><span class="sxs-lookup"><span data-stu-id="dec56-2138">If either operand is NaN, the result is `false` for all operators except `!=`, for which the result is `true`.</span></span> <span data-ttu-id="dec56-2139">Pro jakékoli dva operandy `x != y` vždy vytváří stejný výsledek jako `!(x == y)`.</span><span class="sxs-lookup"><span data-stu-id="dec56-2139">For any two operands, `x != y` always produces the same result as `!(x == y)`.</span></span> <span data-ttu-id="dec56-2140">Pokud jeden nebo oba operandy jsou však NaN, `<`, `>`, `<=`, a `>=` operátory záměrně neprodukují stejné výsledky jako Logická negace opačný operátor.</span><span class="sxs-lookup"><span data-stu-id="dec56-2140">However, when one or both operands are NaN, the `<`, `>`, `<=`, and `>=` operators do not produce the same results as the logical negation of the opposite operator.</span></span> <span data-ttu-id="dec56-2141">Například, pokud buď z `x` a `y` NaN, pak je `x < y` je `false`, ale `!(x >= y)` je `true`.</span><span class="sxs-lookup"><span data-stu-id="dec56-2141">For example, if either of `x` and `y` is NaN, then `x < y` is `false`, but `!(x >= y)` is `true`.</span></span>
*  <span data-ttu-id="dec56-2142">Pokud ani jeden operand není NaN, operátory porovnání hodnoty s plovoucí desetinnou čárkou dva operandy s ohledem na řazení</span><span class="sxs-lookup"><span data-stu-id="dec56-2142">When neither operand is NaN, the operators compare the values of the two floating-point operands with respect to the ordering</span></span>

   ```
   -inf < -max < ... < -min < -0.0 == +0.0 < +min < ... < +max < +inf
   ```

   <span data-ttu-id="dec56-2143">kde `min` a `max` jsou nejnižší a nejvyšší kladné konečné hodnoty, které můžou být vyjádřeny v daném formátu s plovoucí desetinnou čárkou.</span><span class="sxs-lookup"><span data-stu-id="dec56-2143">where `min` and `max` are the smallest and largest positive finite values that can be represented in the given floating-point format.</span></span> <span data-ttu-id="dec56-2144">Toto uspořádání významné důsledky jsou:</span><span class="sxs-lookup"><span data-stu-id="dec56-2144">Notable effects of this ordering are:</span></span>
   * <span data-ttu-id="dec56-2145">Kladné a záporné nuly jsou považovány za shodné.</span><span class="sxs-lookup"><span data-stu-id="dec56-2145">Negative and positive zeros are considered equal.</span></span>
   * <span data-ttu-id="dec56-2146">Záporné nekonečno je považován za méně než všechny ostatní hodnoty, ale rovna jiné záporné nekonečno.</span><span class="sxs-lookup"><span data-stu-id="dec56-2146">A negative infinity is considered less than all other values, but equal to another negative infinity.</span></span>
   * <span data-ttu-id="dec56-2147">Kladné nekonečno je považován za větší než všechny ostatní hodnoty, ale rovna jiné kladné nekonečno.</span><span class="sxs-lookup"><span data-stu-id="dec56-2147">A positive infinity is considered greater than all other values, but equal to another positive infinity.</span></span>

### <a name="decimal-comparison-operators"></a><span data-ttu-id="dec56-2148">Desetinné relační operátory</span><span class="sxs-lookup"><span data-stu-id="dec56-2148">Decimal comparison operators</span></span>

<span data-ttu-id="dec56-2149">Předdefinované desítkové porovnávací operátory jsou:</span><span class="sxs-lookup"><span data-stu-id="dec56-2149">The predefined decimal comparison operators are:</span></span>
```csharp
bool operator ==(decimal x, decimal y);
bool operator !=(decimal x, decimal y);
bool operator <(decimal x, decimal y);
bool operator >(decimal x, decimal y);
bool operator <=(decimal x, decimal y);
bool operator >=(decimal x, decimal y);
```

<span data-ttu-id="dec56-2150">Každý z těchto operátorů porovnání číselných hodnot dvě desetinné operandů a vrací `bool` hodnotu, která určuje, zda je konkrétní vztah `true` nebo `false`.</span><span class="sxs-lookup"><span data-stu-id="dec56-2150">Each of these operators compares the numeric values of the two decimal operands and returns a `bool` value that indicates whether the particular relation is `true` or `false`.</span></span> <span data-ttu-id="dec56-2151">Každé desetinné porovnání je ekvivalentní k použití odpovídající relační nebo operátor rovnosti typu `System.Decimal`.</span><span class="sxs-lookup"><span data-stu-id="dec56-2151">Each decimal comparison is equivalent to using the corresponding relational or equality operator of type `System.Decimal`.</span></span>

### <a name="boolean-equality-operators"></a><span data-ttu-id="dec56-2152">Operátory rovnosti Boolean</span><span class="sxs-lookup"><span data-stu-id="dec56-2152">Boolean equality operators</span></span>

<span data-ttu-id="dec56-2153">Rovnost předdefinované logické operátory jsou:</span><span class="sxs-lookup"><span data-stu-id="dec56-2153">The predefined boolean equality operators are:</span></span>
```csharp
bool operator ==(bool x, bool y);
bool operator !=(bool x, bool y);
```

<span data-ttu-id="dec56-2154">Výsledek `==` je `true` Pokud mají oba `x` a `y` jsou `true` nebo pokud `x` a `y` jsou `false`.</span><span class="sxs-lookup"><span data-stu-id="dec56-2154">The result of `==` is `true` if both `x` and `y` are `true` or if both `x` and `y` are `false`.</span></span> <span data-ttu-id="dec56-2155">V opačném případě je výsledek `false`.</span><span class="sxs-lookup"><span data-stu-id="dec56-2155">Otherwise, the result is `false`.</span></span>

<span data-ttu-id="dec56-2156">Výsledek `!=` je `false` Pokud mají oba `x` a `y` jsou `true` nebo pokud `x` a `y` jsou `false`.</span><span class="sxs-lookup"><span data-stu-id="dec56-2156">The result of `!=` is `false` if both `x` and `y` are `true` or if both `x` and `y` are `false`.</span></span> <span data-ttu-id="dec56-2157">V opačném případě je výsledek `true`.</span><span class="sxs-lookup"><span data-stu-id="dec56-2157">Otherwise, the result is `true`.</span></span> <span data-ttu-id="dec56-2158">Když jsou operandy typu `bool`, `!=` operátor vytvoří stejný výsledek jako `^` operátor.</span><span class="sxs-lookup"><span data-stu-id="dec56-2158">When the operands are of type `bool`, the `!=` operator produces the same result as the `^` operator.</span></span>

### <a name="enumeration-comparison-operators"></a><span data-ttu-id="dec56-2159">Operátory porovnání výčet</span><span class="sxs-lookup"><span data-stu-id="dec56-2159">Enumeration comparison operators</span></span>

<span data-ttu-id="dec56-2160">Každý typ výčtu implicitně poskytuje následující předdefinované relační operátory:</span><span class="sxs-lookup"><span data-stu-id="dec56-2160">Every enumeration type implicitly provides the following predefined comparison operators:</span></span>
```csharp
bool operator ==(E x, E y);
bool operator !=(E x, E y);
bool operator <(E x, E y);
bool operator >(E x, E y);
bool operator <=(E x, E y);
bool operator >=(E x, E y);
```

<span data-ttu-id="dec56-2161">Výsledek vyhodnocení výrazu `x op y`, kde `x` a `y` jsou výrazy typu výčtu `E` s podkladovým typem `U`, a `op` je jedním z operátorů porovnání, je stejný jako vyhodnocení `((U)x) op ((U)y)`.</span><span class="sxs-lookup"><span data-stu-id="dec56-2161">The result of evaluating `x op y`, where `x` and `y` are expressions of an enumeration type `E` with an underlying type `U`, and `op` is one of the comparison operators, is exactly the same as evaluating `((U)x) op ((U)y)`.</span></span> <span data-ttu-id="dec56-2162">Jinými slovy operátory porovnání typu výčtu jednoduše porovnat základní integrální hodnoty dva operandy.</span><span class="sxs-lookup"><span data-stu-id="dec56-2162">In other words, the enumeration type comparison operators simply compare the underlying integral values of the two operands.</span></span>

### <a name="reference-type-equality-operators"></a><span data-ttu-id="dec56-2163">Operátory rovnosti pro typ odkazu</span><span class="sxs-lookup"><span data-stu-id="dec56-2163">Reference type equality operators</span></span>

<span data-ttu-id="dec56-2164">Operátory rovnosti předdefinovaný odkaz typu jsou:</span><span class="sxs-lookup"><span data-stu-id="dec56-2164">The predefined reference type equality operators are:</span></span>
```csharp
bool operator ==(object x, object y);
bool operator !=(object x, object y);
```

<span data-ttu-id="dec56-2165">Operátory vrací výsledek porovnání dvou odkazy a zjistí rovnost či jiných rovnosti.</span><span class="sxs-lookup"><span data-stu-id="dec56-2165">The operators return the result of comparing the two references for equality or non-equality.</span></span>

<span data-ttu-id="dec56-2166">Protože předdefinovaných odkazovací operátory rovnosti typu přijmout operandy typu `object`, se vztahují na všechny typy, které nedeklarujte příslušné `operator ==` a `operator !=` členy.</span><span class="sxs-lookup"><span data-stu-id="dec56-2166">Since the predefined reference type equality operators accept operands of type `object`, they apply to all types that do not declare applicable `operator ==` and `operator !=` members.</span></span> <span data-ttu-id="dec56-2167">Naopak všechny příslušné rovnosti uživatelem definované operátory efektivně skrýt předdefinované odkazovací operátory rovnosti typu.</span><span class="sxs-lookup"><span data-stu-id="dec56-2167">Conversely, any applicable user-defined equality operators effectively hide the predefined reference type equality operators.</span></span>

<span data-ttu-id="dec56-2168">Operátory rovnosti typu předdefinovaných referenčních vyžadovat jeden z následujících akcí:</span><span class="sxs-lookup"><span data-stu-id="dec56-2168">The predefined reference type equality operators require one of the following:</span></span>

*  <span data-ttu-id="dec56-2169">Oba operandy hodnotu typu ví *reference_type* nebo literál `null`.</span><span class="sxs-lookup"><span data-stu-id="dec56-2169">Both operands are a value of a type known to be a *reference_type* or the literal `null`.</span></span> <span data-ttu-id="dec56-2170">Kromě toho konverzi explicitní odkaz ([odkaz na explicitní převody](conversions.md#explicit-reference-conversions)) existuje z typu jeden z operandů typu je druhý operand.</span><span class="sxs-lookup"><span data-stu-id="dec56-2170">Furthermore, an explicit reference conversion ([Explicit reference conversions](conversions.md#explicit-reference-conversions)) exists from the type of either operand to the type of the other operand.</span></span>
*  <span data-ttu-id="dec56-2171">Jeden operand je hodnota typu `T` kde `T` je *type_parameter* a druhý operand je literál `null`.</span><span class="sxs-lookup"><span data-stu-id="dec56-2171">One operand is a value of type `T` where `T` is a *type_parameter* and the other operand is the literal `null`.</span></span> <span data-ttu-id="dec56-2172">Kromě toho `T` nemá omezení typu hodnoty.</span><span class="sxs-lookup"><span data-stu-id="dec56-2172">Furthermore `T` does not have the value type constraint.</span></span>

<span data-ttu-id="dec56-2173">Pokud platí jedna z těchto podmínek, dojde k chybě vazby čas.</span><span class="sxs-lookup"><span data-stu-id="dec56-2173">Unless one of these conditions are true, a binding-time error occurs.</span></span> <span data-ttu-id="dec56-2174">Významné důsledky z těchto pravidel jsou:</span><span class="sxs-lookup"><span data-stu-id="dec56-2174">Notable implications of these rules are:</span></span>

*  <span data-ttu-id="dec56-2175">Je chyba vazby za použití předdefinovaných odkazovací operátory rovnosti typ k porovnání dva odkazy, které jsou známé jako jiný v době vazby.</span><span class="sxs-lookup"><span data-stu-id="dec56-2175">It is a binding-time error to use the predefined reference type equality operators to compare two references that are known to be different at binding-time.</span></span> <span data-ttu-id="dec56-2176">Například, pokud doba vazby typy operandů jsou dva typy tříd `A` a `B`a pokud `A` ani `B` je odvozena z jiných, potom by bylo možné pro dva operandy, chcete-li odkazovat na stejný objekt.</span><span class="sxs-lookup"><span data-stu-id="dec56-2176">For example, if the binding-time types of the operands are two class types `A` and `B`, and if neither `A` nor `B` derives from the other, then it would be impossible for the two operands to reference the same object.</span></span> <span data-ttu-id="dec56-2177">Operace proto se považuje za chybu v době vazby.</span><span class="sxs-lookup"><span data-stu-id="dec56-2177">Thus, the operation is considered a binding-time error.</span></span>
*  <span data-ttu-id="dec56-2178">Operátory rovnosti typu předdefinovaných referenčních neumožňují hodnotu operandy typu, který se má porovnat.</span><span class="sxs-lookup"><span data-stu-id="dec56-2178">The predefined reference type equality operators do not permit value type operands to be compared.</span></span> <span data-ttu-id="dec56-2179">Proto pokud typu Struktura deklaruje vlastní operátory rovnosti, není možné porovnat hodnoty tohoto typu struktury.</span><span class="sxs-lookup"><span data-stu-id="dec56-2179">Therefore, unless a struct type declares its own equality operators, it is not possible to compare values of that struct type.</span></span>
*  <span data-ttu-id="dec56-2180">Operátory rovnosti typu předdefinovaných referenčních nikdy nezpůsobí operace zabalení na výskyt svých operandů.</span><span class="sxs-lookup"><span data-stu-id="dec56-2180">The predefined reference type equality operators never cause boxing operations to occur for their operands.</span></span> <span data-ttu-id="dec56-2181">Bylo by provádět tyto operace zabalení, protože odkazy do nově přiděleného zabalený instancí by nutně lišit od jiných odkazů na nemá význam.</span><span class="sxs-lookup"><span data-stu-id="dec56-2181">It would be meaningless to perform such boxing operations, since references to the newly allocated boxed instances would necessarily differ from all other references.</span></span>
*  <span data-ttu-id="dec56-2182">Pokud typ parametru typu operandu `T` je ve srovnání s `null`a za běhu typu `T` je typ hodnoty, je výsledkem porovnání `false`.</span><span class="sxs-lookup"><span data-stu-id="dec56-2182">If an operand of a type parameter type `T` is compared to `null`, and the run-time type of `T` is a value type, the result of the comparison is `false`.</span></span>

<span data-ttu-id="dec56-2183">Následující příklad zkontroluje, zda je argument typu bez omezení parametru typu `null`.</span><span class="sxs-lookup"><span data-stu-id="dec56-2183">The following example checks whether an argument of an unconstrained type parameter type is `null`.</span></span>
```csharp
class C<T>
{
    void F(T x) {
        if (x == null) throw new ArgumentNullException();
        ...
    }
}
```

<span data-ttu-id="dec56-2184">`x == null` Konstrukce smí obsahovat i v případě `T` by mohly představovat typ hodnoty a výsledek je jednoduše definován jako `false` při `T` je typ hodnoty.</span><span class="sxs-lookup"><span data-stu-id="dec56-2184">The `x == null` construct is permitted even though `T` could represent a value type, and the result is simply defined to be `false` when `T` is a value type.</span></span>

<span data-ttu-id="dec56-2185">Operace formuláře `x == y` nebo `x != y`, pokud je k dispozici žádná `operator ==` nebo `operator !=` existuje, rozlišení přetížení operátoru ([binárním operátorem rozlišení přetěžování](expressions.md#binary-operator-overload-resolution)) pravidla, která vybere operátor místo operátor rovnosti typ předdefinovaný odkaz.</span><span class="sxs-lookup"><span data-stu-id="dec56-2185">For an operation of the form `x == y` or `x != y`, if any applicable `operator ==` or `operator !=` exists, the operator overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) rules will select that operator instead of the predefined reference type equality operator.</span></span> <span data-ttu-id="dec56-2186">Je ale vždy možné vybrat operátor rovnosti předdefinovaných referenčních typu explicitně přetypováním jeden nebo oba operandy na typ `object`.</span><span class="sxs-lookup"><span data-stu-id="dec56-2186">However, it is always possible to select the predefined reference type equality operator by explicitly casting one or both of the operands to type `object`.</span></span> <span data-ttu-id="dec56-2187">V příkladu</span><span class="sxs-lookup"><span data-stu-id="dec56-2187">The example</span></span>
```csharp
using System;

class Test
{
    static void Main() {
        string s = "Test";
        string t = string.Copy(s);
        Console.WriteLine(s == t);
        Console.WriteLine((object)s == t);
        Console.WriteLine(s == (object)t);
        Console.WriteLine((object)s == (object)t);
    }
}
```
<span data-ttu-id="dec56-2188">Vytvoří výstup</span><span class="sxs-lookup"><span data-stu-id="dec56-2188">produces the output</span></span>
```
True
False
False
False
```

<span data-ttu-id="dec56-2189">`s` a `t` proměnná odkazuje na dvou rozdílných `string` instancí, který obsahuje stejné znaky.</span><span class="sxs-lookup"><span data-stu-id="dec56-2189">The `s` and `t` variables refer to two distinct `string` instances containing the same characters.</span></span> <span data-ttu-id="dec56-2190">První porovnání výstupy `True` protože předdefinované řetězcové operátor rovnosti ([řetězec operátory rovnosti](expressions.md#string-equality-operators)) je vybrána, když jsou oba operandy typu `string`.</span><span class="sxs-lookup"><span data-stu-id="dec56-2190">The first comparison outputs `True` because the predefined string equality operator ([String equality operators](expressions.md#string-equality-operators)) is selected when both operands are of type `string`.</span></span> <span data-ttu-id="dec56-2191">Všechny zbývající porovnání výstup `False` protože operátor rovnosti předdefinovaných referenčních typů je vybrána, když jeden nebo oba operandy jsou typu `object`.</span><span class="sxs-lookup"><span data-stu-id="dec56-2191">The remaining comparisons all output `False` because the predefined reference type equality operator is selected when one or both of the operands are of type `object`.</span></span>

<span data-ttu-id="dec56-2192">Všimněte si, že výše uvedené postup není smysl pro typy hodnot.</span><span class="sxs-lookup"><span data-stu-id="dec56-2192">Note that the above technique is not meaningful for value types.</span></span> <span data-ttu-id="dec56-2193">V příkladu</span><span class="sxs-lookup"><span data-stu-id="dec56-2193">The example</span></span>
```csharp
class Test
{
    static void Main() {
        int i = 123;
        int j = 123;
        System.Console.WriteLine((object)i == (object)j);
    }
}
```
<span data-ttu-id="dec56-2194">Vypíše `False` protože vytvořit odkazy na dvě samostatné instance položky CAST v poli `int` hodnoty.</span><span class="sxs-lookup"><span data-stu-id="dec56-2194">outputs `False` because the casts create references to two separate instances of boxed `int` values.</span></span>

### <a name="string-equality-operators"></a><span data-ttu-id="dec56-2195">Operátory rovnosti řetězců</span><span class="sxs-lookup"><span data-stu-id="dec56-2195">String equality operators</span></span>

<span data-ttu-id="dec56-2196">Jsou předdefinované řetězcové operátory rovnosti:</span><span class="sxs-lookup"><span data-stu-id="dec56-2196">The predefined string equality operators are:</span></span>
```csharp
bool operator ==(string x, string y);
bool operator !=(string x, string y);
```

<span data-ttu-id="dec56-2197">Dvě `string` hodnoty jsou považovány za shodné, když je splněna jedna z následujících akcí:</span><span class="sxs-lookup"><span data-stu-id="dec56-2197">Two `string` values are considered equal when one of the following is true:</span></span>

*  <span data-ttu-id="dec56-2198">Jsou obě hodnoty `null`.</span><span class="sxs-lookup"><span data-stu-id="dec56-2198">Both values are `null`.</span></span>
*  <span data-ttu-id="dec56-2199">Odkazy na jinou hodnotu než null řetězec instancí, které mají stejné délky a stejné znaky v každé pozici znaku jsou obě hodnoty.</span><span class="sxs-lookup"><span data-stu-id="dec56-2199">Both values are non-null references to string instances that have identical lengths and identical characters in each character position.</span></span>

<span data-ttu-id="dec56-2200">Operátory rovnosti řetězec porovnání hodnoty řetězce místo odkazy na řetězec.</span><span class="sxs-lookup"><span data-stu-id="dec56-2200">The string equality operators compare string values rather than string references.</span></span> <span data-ttu-id="dec56-2201">Když dvě instance samostatné řetězec obsahovat přesně stejnou sekvenci znaků, jsou hodnoty řetězce stejné, ale odkazy se liší.</span><span class="sxs-lookup"><span data-stu-id="dec56-2201">When two separate string instances contain the exact same sequence of characters, the values of the strings are equal, but the references are different.</span></span> <span data-ttu-id="dec56-2202">Jak je popsáno v [operátory rovnosti pro typ odkazu](expressions.md#reference-type-equality-operators), operátory rovnosti reference typu lze porovnat řetězec odkazy místo řetězcové hodnoty.</span><span class="sxs-lookup"><span data-stu-id="dec56-2202">As described in [Reference type equality operators](expressions.md#reference-type-equality-operators), the reference type equality operators can be used to compare string references instead of string values.</span></span>

### <a name="delegate-equality-operators"></a><span data-ttu-id="dec56-2203">Operátory rovnosti delegáta</span><span class="sxs-lookup"><span data-stu-id="dec56-2203">Delegate equality operators</span></span>

<span data-ttu-id="dec56-2204">Každý typ delegáta implicitně poskytuje následující předdefinované relační operátory:</span><span class="sxs-lookup"><span data-stu-id="dec56-2204">Every delegate type implicitly provides the following predefined comparison operators:</span></span>

```csharp
bool operator ==(System.Delegate x, System.Delegate y);
bool operator !=(System.Delegate x, System.Delegate y);
```

<span data-ttu-id="dec56-2205">Instance dvou delegátů jsou považovány za shodné následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="dec56-2205">Two delegate instances are considered equal as follows:</span></span>

*  <span data-ttu-id="dec56-2206">Pokud je jedna instance delegátů `null`, pokud jsou obě jsou shodné `null`.</span><span class="sxs-lookup"><span data-stu-id="dec56-2206">If either of the delegate instances is `null`, they are equal if and only if both are `null`.</span></span>
*  <span data-ttu-id="dec56-2207">Pokud delegáty mají různé run-time typu nikdy jsou stejné.</span><span class="sxs-lookup"><span data-stu-id="dec56-2207">If the delegates have different run-time type they are never equal.</span></span>
*  <span data-ttu-id="dec56-2208">Pokud obě instance delegáta máte seznam volání ([delegovat deklarace](delegates.md#delegate-declarations)), tyto instance jsou stejné, a pouze v případě jejich vyvolání seznamy mají stejnou délku, a každá položka v seznamu vyvolání jeden z rovná (jak je definována níže) na odpovídající položku v pořadí, v seznamu vyvolání druhé strany.</span><span class="sxs-lookup"><span data-stu-id="dec56-2208">If both of the delegate instances have an invocation list ([Delegate declarations](delegates.md#delegate-declarations)), those instances are equal if and only if their invocation lists are the same length, and each entry in one's invocation list is equal (as defined below) to the corresponding entry, in order, in the other's invocation list.</span></span>

<span data-ttu-id="dec56-2209">Rovnost hodnoty položky seznamu vyvolání platí následující pravidla:</span><span class="sxs-lookup"><span data-stu-id="dec56-2209">The following rules govern the equality of invocation list entries:</span></span>

*  <span data-ttu-id="dec56-2210">Pokud obě dvě seznamu položky vyvolání odkazovat na stejný statická metoda pak položky jsou stejné.</span><span class="sxs-lookup"><span data-stu-id="dec56-2210">If two invocation list entries both refer to the same static method then the entries are equal.</span></span>
*  <span data-ttu-id="dec56-2211">Pokud dvě volání položky seznamu oba odkazují na stejný nestatickou metodu na stejném cílovém objektu (definované operátory rovnosti reference) položky jsou stejné.</span><span class="sxs-lookup"><span data-stu-id="dec56-2211">If two invocation list entries both refer to the same non-static method on the same target object (as defined by the reference equality operators) then the entries are equal.</span></span>
*  <span data-ttu-id="dec56-2212">Vyvolání seznamu položek vytvořený ze zkušební verze sémanticky identických *anonymous_method_expression*s nebo *lambda_expression*s (pravděpodobně prázdná) stejnou sadou zachycené vnější proměnné instance se můžou (ale není nutné) musí rovnat.</span><span class="sxs-lookup"><span data-stu-id="dec56-2212">Invocation list entries produced from evaluation of semantically identical *anonymous_method_expression*s or *lambda_expression*s with the same (possibly empty) set of captured outer variable instances are permitted (but not required) to be equal.</span></span>

### <a name="equality-operators-and-null"></a><span data-ttu-id="dec56-2213">Operátory rovnosti a null</span><span class="sxs-lookup"><span data-stu-id="dec56-2213">Equality operators and null</span></span>

<span data-ttu-id="dec56-2214">`==` a `!=` operátory povolit jeden operand bude hodnota typem s možnou hodnotou Null a druhý, aby `null` literálu, i v případě, že pro operaci neexistuje žádný předdefinovaných nebo uživatelem definovaný operátor (v unlifted nebo zrušeno vs. formuláře).</span><span class="sxs-lookup"><span data-stu-id="dec56-2214">The `==` and `!=` operators permit one operand to be a value of a nullable type and the other to be the `null` literal, even if no predefined or user-defined operator (in unlifted or lifted form) exists for the operation.</span></span>

<span data-ttu-id="dec56-2215">Operace jednoho z formuláře</span><span class="sxs-lookup"><span data-stu-id="dec56-2215">For an operation of one of the forms</span></span>
```csharp
x == null
null == x
x != null
null != x
```
<span data-ttu-id="dec56-2216">kde `x` je výraz typu s možnou hodnotou Null, pokud řešení přetížení operátoru ([binárním operátorem rozlišení přetěžování](expressions.md#binary-operator-overload-resolution)) se nedaří najít příslušné operátora, výsledek se místo toho vypočítá ze `HasValue` Vlastnost `x`.</span><span class="sxs-lookup"><span data-stu-id="dec56-2216">where `x` is an expression of a nullable type, if operator overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) fails to find an applicable operator, the result is instead computed from the `HasValue` property of `x`.</span></span> <span data-ttu-id="dec56-2217">Konkrétně první dvě různými formami jsou přeloženy do `!x.HasValue`, a poslední dvě různými formami jsou přeloženy do `x.HasValue`.</span><span class="sxs-lookup"><span data-stu-id="dec56-2217">Specifically, the first two forms are translated into `!x.HasValue`, and last two forms are translated into `x.HasValue`.</span></span>

### <a name="the-is-operator"></a><span data-ttu-id="dec56-2218">Is – operátor</span><span class="sxs-lookup"><span data-stu-id="dec56-2218">The is operator</span></span>

<span data-ttu-id="dec56-2219">`is` Operátor se používá k dynamicky zkontrolujte, jestli je kompatibilní s daným typem run-time typu objektu.</span><span class="sxs-lookup"><span data-stu-id="dec56-2219">The `is` operator is used to dynamically check if the run-time type of an object is compatible with a given type.</span></span> <span data-ttu-id="dec56-2220">Výsledek operace `E is T`, kde `E` je výraz a `T` je typ, je logická hodnota hodnotu, která udává, zda `E` můžete úspěšně převeden na typ `T` převodem odkaz, zabalení převod, nebo unboxingového převodu.</span><span class="sxs-lookup"><span data-stu-id="dec56-2220">The result of the operation `E is T`, where `E` is an expression and `T` is a type, is a boolean value indicating whether `E` can successfully be converted to type `T` by a reference conversion, a boxing conversion, or an unboxing conversion.</span></span> <span data-ttu-id="dec56-2221">Operace se vyhodnotí takto, jakmile byla nahrazena argumentů typu pro všechny parametry typu:</span><span class="sxs-lookup"><span data-stu-id="dec56-2221">The operation is evaluated as follows, after type arguments have been substituted for all type parameters:</span></span>

*  <span data-ttu-id="dec56-2222">Pokud `E` je anonymní funkce, dojde k chybě kompilace</span><span class="sxs-lookup"><span data-stu-id="dec56-2222">If `E` is an anonymous function, a compile-time error occurs</span></span>
*  <span data-ttu-id="dec56-2223">Pokud `E` je skupinu metod nebo `null` literálu, pokud typ `E` je typem odkazu nebo typ připouštějící hodnotu Null a hodnota `E` je null, je výsledek false.</span><span class="sxs-lookup"><span data-stu-id="dec56-2223">If `E` is a method group or the `null` literal, of if the type of `E` is a reference type or a nullable type and the value of `E` is null, the result is false.</span></span>
*  <span data-ttu-id="dec56-2224">V opačném případě nechat `D` představují dynamického typu `E` následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="dec56-2224">Otherwise, let `D` represent the dynamic type of `E` as follows:</span></span>
   * <span data-ttu-id="dec56-2225">Pokud typ `E` je typem odkazu `D` je run-time typu odkazu na instanci podle `E`.</span><span class="sxs-lookup"><span data-stu-id="dec56-2225">If the type of `E` is a reference type, `D` is the run-time type of the instance reference by `E`.</span></span>
   * <span data-ttu-id="dec56-2226">Pokud typ `E` je typ připouštějící hodnotu Null, `D` je základní typ tohoto typu s možnou hodnotou Null.</span><span class="sxs-lookup"><span data-stu-id="dec56-2226">If the type of `E` is a nullable type, `D` is the underlying type of that nullable type.</span></span>
   * <span data-ttu-id="dec56-2227">Pokud typ `E` je typ hodnoty Null, `D` je typ `E`.</span><span class="sxs-lookup"><span data-stu-id="dec56-2227">If the type of `E` is a non-nullable value type, `D` is the type of `E`.</span></span>
*  <span data-ttu-id="dec56-2228">Výsledek operace závisí na `D` a `T` následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="dec56-2228">The result of the operation depends on `D` and `T` as follows:</span></span>
   * <span data-ttu-id="dec56-2229">Pokud `T` je typem odkazu, je výsledek true Pokud `D` a `T` jsou stejného typu, pokud `D` je typem odkazu a implicitní referenční převod z `D` k `T` existuje, nebo pokud `D` je typ hodnoty a převod na uzavřené určení z `D` k `T` existuje.</span><span class="sxs-lookup"><span data-stu-id="dec56-2229">If `T` is a reference type, the result is true if `D` and `T` are the same type, if `D` is a reference type and an implicit reference conversion from `D` to `T` exists, or if `D` is a value type and a boxing conversion from `D` to `T` exists.</span></span>
   * <span data-ttu-id="dec56-2230">Pokud `T` je typ připouštějící hodnotu Null, je výsledek true Pokud `D` je základní typ `T`.</span><span class="sxs-lookup"><span data-stu-id="dec56-2230">If `T` is a nullable type, the result is true if `D` is the underlying type of `T`.</span></span>
   * <span data-ttu-id="dec56-2231">Pokud `T` je typ hodnoty Null, je výsledek true Pokud `D` a `T` jsou stejného typu.</span><span class="sxs-lookup"><span data-stu-id="dec56-2231">If `T` is a non-nullable value type, the result is true if `D` and `T` are the same type.</span></span>
   * <span data-ttu-id="dec56-2232">Výsledkem je, jinak false.</span><span class="sxs-lookup"><span data-stu-id="dec56-2232">Otherwise, the result is false.</span></span>

<span data-ttu-id="dec56-2233">Všimněte si, že nejsou uznán uživatelem definované převody `is` operátor.</span><span class="sxs-lookup"><span data-stu-id="dec56-2233">Note that user defined conversions, are not considered by the `is` operator.</span></span>

### <a name="the-as-operator"></a><span data-ttu-id="dec56-2234">Operátor as</span><span class="sxs-lookup"><span data-stu-id="dec56-2234">The as operator</span></span>

<span data-ttu-id="dec56-2235">`as` Operátor se používá k explicitnímu převodu hodnoty na typ daného odkazu nebo typ připouštějící hodnotu Null.</span><span class="sxs-lookup"><span data-stu-id="dec56-2235">The `as` operator is used to explicitly convert a value to a given reference type or nullable type.</span></span> <span data-ttu-id="dec56-2236">Na rozdíl od výraz přetypování ([výrazy přetypování](expressions.md#cast-expressions)), `as` operátor nikdy nevyvolá výjimku.</span><span class="sxs-lookup"><span data-stu-id="dec56-2236">Unlike a cast expression ([Cast expressions](expressions.md#cast-expressions)), the `as` operator never throws an exception.</span></span> <span data-ttu-id="dec56-2237">Místo toho, pokud zadaný převod není možný, výsledná hodnota je `null`.</span><span class="sxs-lookup"><span data-stu-id="dec56-2237">Instead, if the indicated conversion is not possible, the resulting value is `null`.</span></span>

<span data-ttu-id="dec56-2238">V operaci formuláře `E as T`, `E` musí být výraz a `T` musí být typ odkazu, parametr typu známé jako typ odkazu nebo typ připouštějící hodnotu Null.</span><span class="sxs-lookup"><span data-stu-id="dec56-2238">In an operation of the form `E as T`, `E` must be an expression and `T` must be a reference type, a type parameter known to be a reference type, or a nullable type.</span></span> <span data-ttu-id="dec56-2239">Kromě toho aspoň jednu z následujících musí mít hodnotu true, nebo jinak dojde k chybě kompilace:</span><span class="sxs-lookup"><span data-stu-id="dec56-2239">Furthermore, at least one of the following must be true, or otherwise a compile-time error occurs:</span></span>

*  <span data-ttu-id="dec56-2240">Identita ([Identity převod](conversions.md#identity-conversion)), implicitní s možnou hodnotou Null ([implicitní převody typu s možnou hodnotou Null](conversions.md#implicit-nullable-conversions)), implicitní odkaz ([odkaz na implicitní převody](conversions.md#implicit-reference-conversions)), zabalení ([ Zabalení převody](conversions.md#boxing-conversions)), explicitní s možnou hodnotou Null ([explicitní převody s možnou hodnotou Null](conversions.md#explicit-nullable-conversions)), přímý odkaz ([převody explicitní odkaz](conversions.md#explicit-reference-conversions)), nebo rozbalení ([Rozbalení převody](conversions.md#unboxing-conversions)) existuje převod z `E` k `T`.</span><span class="sxs-lookup"><span data-stu-id="dec56-2240">An identity ([Identity conversion](conversions.md#identity-conversion)), implicit nullable ([Implicit nullable conversions](conversions.md#implicit-nullable-conversions)), implicit reference ([Implicit reference conversions](conversions.md#implicit-reference-conversions)), boxing ([Boxing conversions](conversions.md#boxing-conversions)), explicit nullable ([Explicit nullable conversions](conversions.md#explicit-nullable-conversions)), explicit reference ([Explicit reference conversions](conversions.md#explicit-reference-conversions)), or unboxing ([Unboxing conversions](conversions.md#unboxing-conversions)) conversion exists from `E` to `T`.</span></span>
*  <span data-ttu-id="dec56-2241">Typ `E` nebo `T` je otevřeného typu.</span><span class="sxs-lookup"><span data-stu-id="dec56-2241">The type of `E` or `T` is an open type.</span></span>
*  <span data-ttu-id="dec56-2242">`E` je `null` literálu.</span><span class="sxs-lookup"><span data-stu-id="dec56-2242">`E` is the `null` literal.</span></span>

<span data-ttu-id="dec56-2243">Pokud typ době kompilace `E` není `dynamic`, operace `E as T` vytváří stejný výsledek jako</span><span class="sxs-lookup"><span data-stu-id="dec56-2243">If the compile-time type of `E` is not `dynamic`, the operation `E as T` produces the same result as</span></span>
```csharp
E is T ? (T)(E) : (T)null
```
<span data-ttu-id="dec56-2244">s tím rozdílem, že `E` se jenom vyhodnotí jednou.</span><span class="sxs-lookup"><span data-stu-id="dec56-2244">except that `E` is only evaluated once.</span></span> <span data-ttu-id="dec56-2245">Kompilátor může očekávat, že optimalizace `E as T` provádět maximálně jeden dynamický typ kontrolu na rozdíl od dvou kontroly dynamického typu odvozené od výše uvedených rozšíření.</span><span class="sxs-lookup"><span data-stu-id="dec56-2245">The compiler can be expected to optimize `E as T` to perform at most one dynamic type check as opposed to the two dynamic type checks implied by the expansion above.</span></span>

<span data-ttu-id="dec56-2246">Pokud typ době kompilace `E` je `dynamic`, na rozdíl od operátoru přetypování `as` operátor není vázán dynamicky ([dynamické vazby](expressions.md#dynamic-binding)).</span><span class="sxs-lookup"><span data-stu-id="dec56-2246">If the compile-time type of `E` is `dynamic`, unlike the cast operator the `as` operator is not dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)).</span></span> <span data-ttu-id="dec56-2247">Proto v tomto případě je rozšíření:</span><span class="sxs-lookup"><span data-stu-id="dec56-2247">Therefore the expansion in this case is:</span></span>
```csharp
E is T ? (T)(object)(E) : (T)null
```

<span data-ttu-id="dec56-2248">Všimněte si, že některé převody, jako je například uživatelem definované převody nejsou s `as` operátor a by místo toho provádět pomocí výrazy přetypování.</span><span class="sxs-lookup"><span data-stu-id="dec56-2248">Note that some conversions, such as user defined conversions, are not possible with the `as` operator and should instead be performed using cast expressions.</span></span>

<span data-ttu-id="dec56-2249">V příkladu</span><span class="sxs-lookup"><span data-stu-id="dec56-2249">In the example</span></span>
```csharp
class X
{

    public string F(object o) {
        return o as string;        // OK, string is a reference type
    }

    public T G<T>(object o) where T: Attribute {
        return o as T;             // Ok, T has a class constraint
    }

    public U H<U>(object o) {
        return o as U;             // Error, U is unconstrained 
    }
}
```
<span data-ttu-id="dec56-2250">parametr typu `T` z `G` známý typ odkazu, protože nemá omezení třídy.</span><span class="sxs-lookup"><span data-stu-id="dec56-2250">the type parameter `T` of `G` is known to be a reference type, because it has the class constraint.</span></span> <span data-ttu-id="dec56-2251">Parametr typu `U` z `H` není ale; proto používání `as` operátor v `H` se nepovoluje.</span><span class="sxs-lookup"><span data-stu-id="dec56-2251">The type parameter `U` of `H` is not however; hence the use of the `as` operator in `H` is disallowed.</span></span>

## <a name="logical-operators"></a><span data-ttu-id="dec56-2252">Logické operátory</span><span class="sxs-lookup"><span data-stu-id="dec56-2252">Logical operators</span></span>

<span data-ttu-id="dec56-2253">`&`, `^`, A `|` operátory jsou volány logické operátory.</span><span class="sxs-lookup"><span data-stu-id="dec56-2253">The `&`, `^`, and `|` operators are called the logical operators.</span></span>

```antlr
and_expression
    : equality_expression
    | and_expression '&' equality_expression
    ;

exclusive_or_expression
    : and_expression
    | exclusive_or_expression '^' and_expression
    ;

inclusive_or_expression
    : exclusive_or_expression
    | inclusive_or_expression '|' exclusive_or_expression
    ;
```

<span data-ttu-id="dec56-2254">Pokud operand logického operátoru má typ kompilace `dynamic`, pak je dynamicky vázán výraz ([dynamické vazby](expressions.md#dynamic-binding)).</span><span class="sxs-lookup"><span data-stu-id="dec56-2254">If an operand of a logical operator has the compile-time type `dynamic`, then the expression is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)).</span></span> <span data-ttu-id="dec56-2255">V tomto případě je kompilaci typu výrazu `dynamic`, a rozlišení je popsáno níže se provede v době běhu pomocí run-time typu těchto operandů, která mají typ kompilace `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="dec56-2255">In this case the compile-time type of the expression is `dynamic`, and the resolution described below will take place at run-time using the run-time type of those operands that have the compile-time type `dynamic`.</span></span>

<span data-ttu-id="dec56-2256">Operace formuláře `x op y`, kde `op` je jedním z logické operátory přetížení ([binárním operátorem rozlišení přetěžování](expressions.md#binary-operator-overload-resolution)) se použije k výběru na konkrétní operátor implementace.</span><span class="sxs-lookup"><span data-stu-id="dec56-2256">For an operation of the form `x op y`, where `op` is one of the logical operators, overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="dec56-2257">Operandy jsou převedeny na zvoleném operátorovi typy parametrů a typ výsledku je návratový typ operátoru.</span><span class="sxs-lookup"><span data-stu-id="dec56-2257">The operands are converted to the parameter types of the selected operator, and the type of the result is the return type of the operator.</span></span>

<span data-ttu-id="dec56-2258">Předdefinované logické operátory jsou popsány v následujících částech.</span><span class="sxs-lookup"><span data-stu-id="dec56-2258">The predefined logical operators are described in the following sections.</span></span>

### <a name="integer-logical-operators"></a><span data-ttu-id="dec56-2259">Celé číslo logické operátory</span><span class="sxs-lookup"><span data-stu-id="dec56-2259">Integer logical operators</span></span>

<span data-ttu-id="dec56-2260">Předdefinované celé číslo logické operátory jsou:</span><span class="sxs-lookup"><span data-stu-id="dec56-2260">The predefined integer logical operators are:</span></span>
```csharp
int operator &(int x, int y);
uint operator &(uint x, uint y);
long operator &(long x, long y);
ulong operator &(ulong x, ulong y);

int operator |(int x, int y);
uint operator |(uint x, uint y);
long operator |(long x, long y);
ulong operator |(ulong x, ulong y);

int operator ^(int x, int y);
uint operator ^(uint x, uint y);
long operator ^(long x, long y);
ulong operator ^(ulong x, ulong y);
```

<span data-ttu-id="dec56-2261">`&` Vypočítá bitový operátor logického `AND` dvou operandů `|` vypočítá bitový operátor logického `OR` dvou operandů a `^` vypočítá bitový exkluzivní logický operátor `OR` dvou operandů.</span><span class="sxs-lookup"><span data-stu-id="dec56-2261">The `&` operator computes the bitwise logical `AND` of the two operands, the `|` operator computes the bitwise logical `OR` of the two operands, and the `^` operator computes the bitwise logical exclusive `OR` of the two operands.</span></span> <span data-ttu-id="dec56-2262">Žádné přetečení jsou z těchto operací.</span><span class="sxs-lookup"><span data-stu-id="dec56-2262">No overflows are possible from these operations.</span></span>

### <a name="enumeration-logical-operators"></a><span data-ttu-id="dec56-2263">Výčet logické operátory</span><span class="sxs-lookup"><span data-stu-id="dec56-2263">Enumeration logical operators</span></span>

<span data-ttu-id="dec56-2264">Každý typ výčtu `E` implicitně poskytuje následující předdefinované logické operátory:</span><span class="sxs-lookup"><span data-stu-id="dec56-2264">Every enumeration type `E` implicitly provides the following predefined logical operators:</span></span>

```csharp
E operator &(E x, E y);
E operator |(E x, E y);
E operator ^(E x, E y);
```

<span data-ttu-id="dec56-2265">Výsledek vyhodnocení výrazu `x op y`, kde `x` a `y` jsou výrazy typu výčtu `E` s podkladovým typem `U`, a `op` je jedním z logické operátory, je stejný jako vyhodnocení `(E)((U)x op (U)y)`.</span><span class="sxs-lookup"><span data-stu-id="dec56-2265">The result of evaluating `x op y`, where `x` and `y` are expressions of an enumeration type `E` with an underlying type `U`, and `op` is one of the logical operators, is exactly the same as evaluating `(E)((U)x op (U)y)`.</span></span> <span data-ttu-id="dec56-2266">Logické operátory typu výčtu jinými slovy, stačí provést logická operace s základního typu dva operandy.</span><span class="sxs-lookup"><span data-stu-id="dec56-2266">In other words, the enumeration type logical operators simply perform the logical operation on the underlying type of the two operands.</span></span>

### <a name="boolean-logical-operators"></a><span data-ttu-id="dec56-2267">Logické operátory</span><span class="sxs-lookup"><span data-stu-id="dec56-2267">Boolean logical operators</span></span>

<span data-ttu-id="dec56-2268">Předdefinované logické logické operátory jsou:</span><span class="sxs-lookup"><span data-stu-id="dec56-2268">The predefined boolean logical operators are:</span></span>
```csharp
bool operator &(bool x, bool y);
bool operator |(bool x, bool y);
bool operator ^(bool x, bool y);
```

<span data-ttu-id="dec56-2269">Výsledek `x & y` je `true` Pokud mají oba `x` a `y` jsou `true`.</span><span class="sxs-lookup"><span data-stu-id="dec56-2269">The result of `x & y` is `true` if both `x` and `y` are `true`.</span></span> <span data-ttu-id="dec56-2270">V opačném případě je výsledek `false`.</span><span class="sxs-lookup"><span data-stu-id="dec56-2270">Otherwise, the result is `false`.</span></span>

<span data-ttu-id="dec56-2271">Výsledek `x | y` je `true` Pokud `x` nebo `y` je `true`.</span><span class="sxs-lookup"><span data-stu-id="dec56-2271">The result of `x | y` is `true` if either `x` or `y` is `true`.</span></span> <span data-ttu-id="dec56-2272">V opačném případě je výsledek `false`.</span><span class="sxs-lookup"><span data-stu-id="dec56-2272">Otherwise, the result is `false`.</span></span>

<span data-ttu-id="dec56-2273">Výsledek `x ^ y` je `true` Pokud `x` je `true` a `y` je `false`, nebo `x` je `false` a `y` je `true`.</span><span class="sxs-lookup"><span data-stu-id="dec56-2273">The result of `x ^ y` is `true` if `x` is `true` and `y` is `false`, or `x` is `false` and `y` is `true`.</span></span> <span data-ttu-id="dec56-2274">V opačném případě je výsledek `false`.</span><span class="sxs-lookup"><span data-stu-id="dec56-2274">Otherwise, the result is `false`.</span></span> <span data-ttu-id="dec56-2275">Když jsou operandy typu `bool`, `^` operátor vypočítá stejný výsledek jako `!=` operátor.</span><span class="sxs-lookup"><span data-stu-id="dec56-2275">When the operands are of type `bool`, the `^` operator computes the same result as the `!=` operator.</span></span>

### <a name="nullable-boolean-logical-operators"></a><span data-ttu-id="dec56-2276">Logická logické operátory s povolenou hodnotou Null</span><span class="sxs-lookup"><span data-stu-id="dec56-2276">Nullable boolean logical operators</span></span>

<span data-ttu-id="dec56-2277">Typ s možnou hodnotou Null logická `bool?` může představovat tří hodnot `true`, `false`, a `null`a se koncepčně podobá tři vracející typ použitý pro logické výrazy v jazyce SQL.</span><span class="sxs-lookup"><span data-stu-id="dec56-2277">The nullable boolean type `bool?` can represent three values, `true`, `false`, and `null`, and is conceptually similar to the three-valued type used for boolean expressions in SQL.</span></span> <span data-ttu-id="dec56-2278">Chcete-li zajistit výsledky vytvořené metodou `&` a `|` operátory pro `bool?` operandy jsou konzistentní s logikou s hodnotou tři SQL, následující předdefinované operátory jsou k dispozici:</span><span class="sxs-lookup"><span data-stu-id="dec56-2278">To ensure that the results produced by the `&` and `|` operators for `bool?` operands are consistent with SQL's three-valued logic, the following predefined operators are provided:</span></span>

```csharp
bool? operator &(bool? x, bool? y);
bool? operator |(bool? x, bool? y);
```

<span data-ttu-id="dec56-2279">V následující tabulce jsou uvedeny výsledky vytvořené metodou tyto operátory pro všechny kombinace hodnot `true`, `false`, a `null`.</span><span class="sxs-lookup"><span data-stu-id="dec56-2279">The following table lists the results produced by these operators for all combinations of the values `true`, `false`, and `null`.</span></span>

| `x`     | `y`     | `x & y` | <code>x &#124; y</code> |
|:-------:|:-------:|:-------:|:-------:|
| `true`  | `true`  | `true`  | `true`  | 
| `true`  | `false` | `false` | `true`  | 
| `true`  | `null`  | `null`  | `true`  | 
| `false` | `true`  | `false` | `true`  | 
| `false` | `false` | `false` | `false` | 
| `false` | `null`  | `false` | `null`  | 
| `null`  | `true`  | `null`  | `true`  | 
| `null`  | `false` | `false` | `null`  | 
| `null`  | `null`  | `null`  | `null`  | 

## <a name="conditional-logical-operators"></a><span data-ttu-id="dec56-2280">Podmíněné logické operátory</span><span class="sxs-lookup"><span data-stu-id="dec56-2280">Conditional logical operators</span></span>

<span data-ttu-id="dec56-2281">`&&` a `||` operátory jsou volány podmíněné logické operátory.</span><span class="sxs-lookup"><span data-stu-id="dec56-2281">The `&&` and `||` operators are called the conditional logical operators.</span></span> <span data-ttu-id="dec56-2282">Označují se také jako "short-circuiting" logické operátory.</span><span class="sxs-lookup"><span data-stu-id="dec56-2282">They are also called the "short-circuiting" logical operators.</span></span>

```antlr
conditional_and_expression
    : inclusive_or_expression
    | conditional_and_expression '&&' inclusive_or_expression
    ;

conditional_or_expression
    : conditional_and_expression
    | conditional_or_expression '||' conditional_and_expression
    ;
```

<span data-ttu-id="dec56-2283">`&&` a `||` operátory jsou podmíněné verze `&` a `|` operátory:</span><span class="sxs-lookup"><span data-stu-id="dec56-2283">The `&&` and `||` operators are conditional versions of the `&` and `|` operators:</span></span>

*  <span data-ttu-id="dec56-2284">Operace `x && y` odpovídá operaci `x & y`, s tím rozdílem, že `y` je vyhodnocen pouze v případě `x` není `false`.</span><span class="sxs-lookup"><span data-stu-id="dec56-2284">The operation `x && y` corresponds to the operation `x & y`, except that `y` is evaluated only if `x` is not `false`.</span></span>
*  <span data-ttu-id="dec56-2285">Operace `x || y` odpovídá operaci `x | y`, s tím rozdílem, že `y` je vyhodnocen pouze v případě `x` není `true`.</span><span class="sxs-lookup"><span data-stu-id="dec56-2285">The operation `x || y` corresponds to the operation `x | y`, except that `y` is evaluated only if `x` is not `true`.</span></span>

<span data-ttu-id="dec56-2286">Pokud operand logického operátoru podmíněného má typ kompilace `dynamic`, pak je dynamicky vázán výraz ([dynamické vazby](expressions.md#dynamic-binding)).</span><span class="sxs-lookup"><span data-stu-id="dec56-2286">If an operand of a conditional logical operator has the compile-time type `dynamic`, then the expression is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)).</span></span> <span data-ttu-id="dec56-2287">V tomto případě je kompilaci typu výrazu `dynamic`, a rozlišení je popsáno níže se provede v době běhu pomocí run-time typu těchto operandů, která mají typ kompilace `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="dec56-2287">In this case the compile-time type of the expression is `dynamic`, and the resolution described below will take place at run-time using the run-time type of those operands that have the compile-time type `dynamic`.</span></span>

<span data-ttu-id="dec56-2288">Operace formuláře `x && y` nebo `x || y` zpracování s použitím řešení přetížení ([binárním operátorem rozlišení přetěžování](expressions.md#binary-operator-overload-resolution)) jako kdyby byla zapsána operaci `x & y` nebo `x | y`.</span><span class="sxs-lookup"><span data-stu-id="dec56-2288">An operation of the form `x && y` or `x || y` is processed by applying overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) as if the operation was written `x & y` or `x | y`.</span></span> <span data-ttu-id="dec56-2289">Potom</span><span class="sxs-lookup"><span data-stu-id="dec56-2289">Then,</span></span>

*  <span data-ttu-id="dec56-2290">Pokud se nepodaří určit přetížení k vyhledání jednoho nejlepší operátoru nebo řešení přetížení vybere jeden z předdefinovaných celé číslo logické operátory, dojde k chybě vazby čas.</span><span class="sxs-lookup"><span data-stu-id="dec56-2290">If overload resolution fails to find a single best operator, or if overload resolution selects one of the predefined integer logical operators, a binding-time error occurs.</span></span>
*  <span data-ttu-id="dec56-2291">Jinak, pokud vybraný operátor je jedním z předdefinované logické logické operátory ([logické logické operátory](expressions.md#boolean-logical-operators)) nebo logická logické operátory s povolenou hodnotou Null ([logické operátory s povolenou hodnotou Null logická](expressions.md#nullable-boolean-logical-operators)), operace se nezpracovala, jak je popsáno v [logické podmíněné logické operátory](expressions.md#boolean-conditional-logical-operators).</span><span class="sxs-lookup"><span data-stu-id="dec56-2291">Otherwise, if the selected operator is one of the predefined boolean logical operators ([Boolean logical operators](expressions.md#boolean-logical-operators)) or nullable boolean logical operators ([Nullable boolean logical operators](expressions.md#nullable-boolean-logical-operators)), the operation is processed as described in [Boolean conditional logical operators](expressions.md#boolean-conditional-logical-operators).</span></span>
*  <span data-ttu-id="dec56-2292">V opačném případě zvoleném operátorovi je uživatelem definovaný operátor a bude operace zpracována, jak je popsáno v [podmíněné logické operátory definované uživatelem](expressions.md#user-defined-conditional-logical-operators).</span><span class="sxs-lookup"><span data-stu-id="dec56-2292">Otherwise, the selected operator is a user-defined operator, and the operation is processed as described in [User-defined conditional logical operators](expressions.md#user-defined-conditional-logical-operators).</span></span>

<span data-ttu-id="dec56-2293">Není možné přímo přetížení podmíněné logické operátory.</span><span class="sxs-lookup"><span data-stu-id="dec56-2293">It is not possible to directly overload the conditional logical operators.</span></span> <span data-ttu-id="dec56-2294">Ale protože podmíněné logické operátory jsou vyhodnocovány z hlediska regulární logické operátory, přetíženími pravidelné logických operátorů, s určitými omezeními také považují přetížení podmíněné logických operátorů.</span><span class="sxs-lookup"><span data-stu-id="dec56-2294">However, because the conditional logical operators are evaluated in terms of the regular logical operators, overloads of the regular logical operators are, with certain restrictions, also considered overloads of the conditional logical operators.</span></span> <span data-ttu-id="dec56-2295">Toto je popsáno dále v [podmíněné logické operátory definované uživatelem](expressions.md#user-defined-conditional-logical-operators).</span><span class="sxs-lookup"><span data-stu-id="dec56-2295">This is described further in [User-defined conditional logical operators](expressions.md#user-defined-conditional-logical-operators).</span></span>

### <a name="boolean-conditional-logical-operators"></a><span data-ttu-id="dec56-2296">Logická podmíněné logické operátory</span><span class="sxs-lookup"><span data-stu-id="dec56-2296">Boolean conditional logical operators</span></span>

<span data-ttu-id="dec56-2297">Když operandy `&&` nebo `||` jsou typu `bool`, nebo když jsou jako operandy typů, které nedefinují příslušném `operator &` nebo `operator |`, ale definovat implicitní převod na `bool`, operace se zpracování následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="dec56-2297">When the operands of `&&` or `||` are of type `bool`, or when the operands are of types that do not define an applicable `operator &` or `operator |`, but do define implicit conversions to `bool`, the operation is processed as follows:</span></span>

*  <span data-ttu-id="dec56-2298">Operace `x && y` se vyhodnotí jako `x ? y : false`.</span><span class="sxs-lookup"><span data-stu-id="dec56-2298">The operation `x && y` is evaluated as `x ? y : false`.</span></span> <span data-ttu-id="dec56-2299">Jinými slovy `x` je nejdřív vyhodnotit a převedeny na typ `bool`.</span><span class="sxs-lookup"><span data-stu-id="dec56-2299">In other words, `x` is first evaluated and converted to type `bool`.</span></span> <span data-ttu-id="dec56-2300">Když se poté `x` je `true`, `y` je vyhodnocen a převeden na typ `bool`, a toto řešení je výsledek operace.</span><span class="sxs-lookup"><span data-stu-id="dec56-2300">Then, if `x` is `true`, `y` is evaluated and converted to type `bool`, and this becomes the result of the operation.</span></span> <span data-ttu-id="dec56-2301">V opačném případě je výsledek operace `false`.</span><span class="sxs-lookup"><span data-stu-id="dec56-2301">Otherwise, the result of the operation is `false`.</span></span>
*  <span data-ttu-id="dec56-2302">Operace `x || y` se vyhodnotí jako `x ? true : y`.</span><span class="sxs-lookup"><span data-stu-id="dec56-2302">The operation `x || y` is evaluated as `x ? true : y`.</span></span> <span data-ttu-id="dec56-2303">Jinými slovy `x` je nejdřív vyhodnotit a převedeny na typ `bool`.</span><span class="sxs-lookup"><span data-stu-id="dec56-2303">In other words, `x` is first evaluated and converted to type `bool`.</span></span> <span data-ttu-id="dec56-2304">Když se poté `x` je `true`, je výsledek operace `true`.</span><span class="sxs-lookup"><span data-stu-id="dec56-2304">Then, if `x` is `true`, the result of the operation is `true`.</span></span> <span data-ttu-id="dec56-2305">V opačném případě `y` je vyhodnocen a převeden na typ `bool`, a toto řešení je výsledek operace.</span><span class="sxs-lookup"><span data-stu-id="dec56-2305">Otherwise, `y` is evaluated and converted to type `bool`, and this becomes the result of the operation.</span></span>

### <a name="user-defined-conditional-logical-operators"></a><span data-ttu-id="dec56-2306">Podmíněné logické operátory definované uživatelem</span><span class="sxs-lookup"><span data-stu-id="dec56-2306">User-defined conditional logical operators</span></span>

<span data-ttu-id="dec56-2307">Když operandy `&&` nebo `||` jsou typy, které deklarují příslušném uživatelem definované `operator &` nebo `operator |`, z následujících možností musí mít hodnotu true, pokud `T` je typ, ve kterém je deklarována zvoleném operátorovi:</span><span class="sxs-lookup"><span data-stu-id="dec56-2307">When the operands of `&&` or `||` are of types that declare an applicable user-defined `operator &` or `operator |`, both of the following must be true, where `T` is the type in which the selected operator is declared:</span></span>

*  <span data-ttu-id="dec56-2308">Musí být návratový typ a zadejte každý parametr zvoleném operátorovi `T`.</span><span class="sxs-lookup"><span data-stu-id="dec56-2308">The return type and the type of each parameter of the selected operator must be `T`.</span></span> <span data-ttu-id="dec56-2309">Jinými slovy, musíte vypočítat logický operátor, který `AND` nebo logickém `OR` dvou operandů typu `T`a musí vrátit výsledek typu `T`.</span><span class="sxs-lookup"><span data-stu-id="dec56-2309">In other words, the operator must compute the logical `AND` or the logical `OR` of two operands of type `T`, and must return a result of type `T`.</span></span>
*  <span data-ttu-id="dec56-2310">`T` musí obsahovat deklarace `operator true` a `operator false`.</span><span class="sxs-lookup"><span data-stu-id="dec56-2310">`T` must contain declarations of `operator true` and `operator false`.</span></span>

<span data-ttu-id="dec56-2311">Pokud některý z těchto požadavků není splněno, dojde k chybě vazby – za.</span><span class="sxs-lookup"><span data-stu-id="dec56-2311">A binding-time error occurs if either of these requirements is not satisfied.</span></span> <span data-ttu-id="dec56-2312">V opačném případě `&&` nebo `||` operace se vyhodnocuje na základě kombinace uživatelsky definované `operator true` nebo `operator false` s vybranou uživatelem definovaný operátor:</span><span class="sxs-lookup"><span data-stu-id="dec56-2312">Otherwise, the `&&` or `||` operation is evaluated by combining the user-defined `operator true` or `operator false` with the selected user-defined operator:</span></span>

*  <span data-ttu-id="dec56-2313">Operace `x && y` se vyhodnotí jako `T.false(x) ? x : T.&(x, y)`, kde `T.false(x)` je vyvolání `operator false` deklarované v `T`, a `T.&(x, y)` je vyvolání vybraného `operator &`.</span><span class="sxs-lookup"><span data-stu-id="dec56-2313">The operation `x && y` is evaluated as `T.false(x) ? x : T.&(x, y)`, where `T.false(x)` is an invocation of the `operator false` declared in `T`, and `T.&(x, y)` is an invocation of the selected `operator &`.</span></span> <span data-ttu-id="dec56-2314">Jinými slovy `x` nejdřív vyhodnotit a `operator false` se vyvolá u výsledku k určení, zda `x` je jednoznačně false.</span><span class="sxs-lookup"><span data-stu-id="dec56-2314">In other words, `x` is first evaluated and `operator false` is invoked on the result to determine if `x` is definitely false.</span></span> <span data-ttu-id="dec56-2315">Když se poté `x` je jednoznačně false, je hodnota vypočítaná dříve pro výsledek operace `x`.</span><span class="sxs-lookup"><span data-stu-id="dec56-2315">Then, if `x` is definitely false, the result of the operation is the value previously computed for `x`.</span></span> <span data-ttu-id="dec56-2316">V opačném případě `y` je vyhodnocen a vybraný `operator &` se vyvolá u hodnota vypočítaná dříve pro `x` a hodnotu vypočítat pro `y` vytvoří výsledek operace.</span><span class="sxs-lookup"><span data-stu-id="dec56-2316">Otherwise, `y` is evaluated, and the selected `operator &` is invoked on the value previously computed for `x` and the value computed for `y` to produce the result of the operation.</span></span>
*  <span data-ttu-id="dec56-2317">Operace `x || y` se vyhodnotí jako `T.true(x) ? x : T.|(x, y)`, kde `T.true(x)` je vyvolání `operator true` deklarované v `T`, a `T.|(x,y)` je vyvolání vybraného `operator|`.</span><span class="sxs-lookup"><span data-stu-id="dec56-2317">The operation `x || y` is evaluated as `T.true(x) ? x : T.|(x, y)`, where `T.true(x)` is an invocation of the `operator true` declared in `T`, and `T.|(x,y)` is an invocation of the selected `operator|`.</span></span> <span data-ttu-id="dec56-2318">Jinými slovy `x` nejdřív vyhodnotit a `operator true` se vyvolá u výsledku k určení, zda `x` jednoznačně platí.</span><span class="sxs-lookup"><span data-stu-id="dec56-2318">In other words, `x` is first evaluated and `operator true` is invoked on the result to determine if `x` is definitely true.</span></span> <span data-ttu-id="dec56-2319">Když se poté `x` je jednoznačně true, je hodnota vypočítaná dříve pro výsledek operace `x`.</span><span class="sxs-lookup"><span data-stu-id="dec56-2319">Then, if `x` is definitely true, the result of the operation is the value previously computed for `x`.</span></span> <span data-ttu-id="dec56-2320">V opačném případě `y` je vyhodnocen a vybraný `operator |` se vyvolá u hodnota vypočítaná dříve pro `x` a hodnotu vypočítat pro `y` vytvoří výsledek operace.</span><span class="sxs-lookup"><span data-stu-id="dec56-2320">Otherwise, `y` is evaluated, and the selected `operator |` is invoked on the value previously computed for `x` and the value computed for `y` to produce the result of the operation.</span></span>

<span data-ttu-id="dec56-2321">V některém z těchto operací výraz Dal `x` je jen pro vyhodnotí jednou a výraz Dal `y` je buď není vyhodnocen nebo vyhodnotí přesně jednou.</span><span class="sxs-lookup"><span data-stu-id="dec56-2321">In either of these operations, the expression given by `x` is only evaluated once, and the expression given by `y` is either not evaluated or evaluated exactly once.</span></span>

<span data-ttu-id="dec56-2322">Příklad typu, který implementuje `operator true` a `operator false`, naleznete v tématu [databáze typu boolean](structs.md#database-boolean-type).</span><span class="sxs-lookup"><span data-stu-id="dec56-2322">For an example of a type that implements `operator true` and `operator false`, see [Database boolean type](structs.md#database-boolean-type).</span></span>

## <a name="the-null-coalescing-operator"></a><span data-ttu-id="dec56-2323">Null operátor sloučení</span><span class="sxs-lookup"><span data-stu-id="dec56-2323">The null coalescing operator</span></span>

<span data-ttu-id="dec56-2324">`??` Operátor je volána null operátor sloučení.</span><span class="sxs-lookup"><span data-stu-id="dec56-2324">The `??` operator is called the null coalescing operator.</span></span>

```antlr
null_coalescing_expression
    : conditional_or_expression
    | conditional_or_expression '??' null_coalescing_expression
    ;
```

<span data-ttu-id="dec56-2325">Nulový slučovací výraz ve tvaru `a ?? b` vyžaduje `a` bude typu nebo odkaz na typ připouštějící hodnotu Null.</span><span class="sxs-lookup"><span data-stu-id="dec56-2325">A null coalescing expression of the form `a ?? b` requires `a` to be of a nullable type or reference type.</span></span> <span data-ttu-id="dec56-2326">Pokud `a` je jiná než null, výsledek `a ?? b` je `a`; jinak vrátí hodnotu, výsledek je `b`.</span><span class="sxs-lookup"><span data-stu-id="dec56-2326">If `a` is non-null, the result of `a ?? b` is `a`; otherwise, the result is `b`.</span></span> <span data-ttu-id="dec56-2327">Výsledkem operace `b` pouze tehdy, pokud `a` má hodnotu null.</span><span class="sxs-lookup"><span data-stu-id="dec56-2327">The operation evaluates `b` only if `a` is null.</span></span>

<span data-ttu-id="dec56-2328">Null operátor sloučení je asociativní zprava, což znamená, že operace se seskupují zleva doprava.</span><span class="sxs-lookup"><span data-stu-id="dec56-2328">The null coalescing operator is right-associative, meaning that operations are grouped from right to left.</span></span> <span data-ttu-id="dec56-2329">Například výraz ve tvaru `a ?? b ?? c` se vyhodnotí jako `a ?? (b ?? c)`.</span><span class="sxs-lookup"><span data-stu-id="dec56-2329">For example, an expression of the form `a ?? b ?? c` is evaluated as `a ?? (b ?? c)`.</span></span> <span data-ttu-id="dec56-2330">Obecně platí podmínky výrazu v podobě `E1 ?? E2 ?? ... ?? En` vrátí první operandy, jinou hodnotu než null, nebo hodnotu null, pokud jsou všechny operandy hodnotu null.</span><span class="sxs-lookup"><span data-stu-id="dec56-2330">In general terms, an expression of the form `E1 ?? E2 ?? ... ?? En` returns the first of the operands that is non-null, or null if all operands are null.</span></span>

<span data-ttu-id="dec56-2331">Typ výrazu `a ?? b` závisí na které implicitní převody jsou k dispozici na operandy.</span><span class="sxs-lookup"><span data-stu-id="dec56-2331">The type of the expression `a ?? b` depends on which implicit conversions are available on the operands.</span></span> <span data-ttu-id="dec56-2332">V pořadí podle priority, typu `a ?? b` je `A0`, `A`, nebo `B`, kde `A` je typ `a` (za předpokladu, že `a` má typ), `B` je typ `b` () za předpokladu, že `b` má typ), a `A0` je základní typ `A` Pokud `A` je typ připouštějící hodnotu Null, nebo `A` jinak.</span><span class="sxs-lookup"><span data-stu-id="dec56-2332">In order of preference, the type of `a ?? b` is `A0`, `A`, or `B`, where `A` is the type of `a` (provided that `a` has a type), `B` is the type of `b` (provided that `b` has a type), and `A0` is the underlying type of `A` if `A` is a nullable type, or `A` otherwise.</span></span> <span data-ttu-id="dec56-2333">Konkrétně `a ?? b` zpracování následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="dec56-2333">Specifically, `a ?? b` is processed as follows:</span></span>

*  <span data-ttu-id="dec56-2334">Pokud `A` existuje a není typ připouštějící hodnotu null nebo typ odkazu, dojde k chybě kompilace.</span><span class="sxs-lookup"><span data-stu-id="dec56-2334">If `A` exists and is not a nullable type or a reference type, a compile-time error occurs.</span></span>
*  <span data-ttu-id="dec56-2335">Pokud `b` dynamického výrazu je typ výsledku je `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="dec56-2335">If `b` is a dynamic expression, the result type is `dynamic`.</span></span> <span data-ttu-id="dec56-2336">V době běhu `a` nejprve vyhodnocena.</span><span class="sxs-lookup"><span data-stu-id="dec56-2336">At run-time, `a` is first evaluated.</span></span> <span data-ttu-id="dec56-2337">Pokud `a` nemá hodnotu null, `a` je převést na dynamický, a to se stane výsledek.</span><span class="sxs-lookup"><span data-stu-id="dec56-2337">If `a` is not null, `a` is converted to dynamic, and this becomes the result.</span></span> <span data-ttu-id="dec56-2338">V opačném případě `b` vyhodnocena, a to se stane výsledek.</span><span class="sxs-lookup"><span data-stu-id="dec56-2338">Otherwise, `b` is evaluated, and this becomes the result.</span></span>
*  <span data-ttu-id="dec56-2339">Jinak, pokud `A` existuje a je typ připouštějící hodnotu Null a existuje implicitní převod z `b` k `A0`, typ výsledku je `A0`.</span><span class="sxs-lookup"><span data-stu-id="dec56-2339">Otherwise, if `A` exists and is a nullable type and an implicit conversion exists from `b` to `A0`, the result type is `A0`.</span></span> <span data-ttu-id="dec56-2340">V době běhu `a` nejprve vyhodnocena.</span><span class="sxs-lookup"><span data-stu-id="dec56-2340">At run-time, `a` is first evaluated.</span></span> <span data-ttu-id="dec56-2341">Pokud `a` nemá hodnotu null, `a` je neobalený, aby typ `A0`, a to se stane výsledek.</span><span class="sxs-lookup"><span data-stu-id="dec56-2341">If `a` is not null, `a` is unwrapped to type `A0`, and this becomes the result.</span></span> <span data-ttu-id="dec56-2342">V opačném případě `b` je vyhodnocen a převeden na typ `A0`, a to se stane výsledek.</span><span class="sxs-lookup"><span data-stu-id="dec56-2342">Otherwise, `b` is evaluated and converted to type `A0`, and this becomes the result.</span></span>
*  <span data-ttu-id="dec56-2343">Jinak, pokud `A` existuje a že existuje implicitní převod z `b` k `A`, typ výsledku je `A`.</span><span class="sxs-lookup"><span data-stu-id="dec56-2343">Otherwise, if `A` exists and an implicit conversion exists from `b` to `A`, the result type is `A`.</span></span> <span data-ttu-id="dec56-2344">V době běhu `a` nejprve vyhodnocena.</span><span class="sxs-lookup"><span data-stu-id="dec56-2344">At run-time, `a` is first evaluated.</span></span> <span data-ttu-id="dec56-2345">Pokud `a` nemá hodnotu null, `a` výsledek.</span><span class="sxs-lookup"><span data-stu-id="dec56-2345">If `a` is not null, `a` becomes the result.</span></span> <span data-ttu-id="dec56-2346">V opačném případě `b` je vyhodnocen a převeden na typ `A`, a to se stane výsledek.</span><span class="sxs-lookup"><span data-stu-id="dec56-2346">Otherwise, `b` is evaluated and converted to type `A`, and this becomes the result.</span></span>
*  <span data-ttu-id="dec56-2347">Jinak, pokud `b` má typ `B` a existuje implicitní převod z `a` k `B`, typ výsledku je `B`.</span><span class="sxs-lookup"><span data-stu-id="dec56-2347">Otherwise, if `b` has a type `B` and an implicit conversion exists from `a` to `B`, the result type is `B`.</span></span> <span data-ttu-id="dec56-2348">V době běhu `a` nejprve vyhodnocena.</span><span class="sxs-lookup"><span data-stu-id="dec56-2348">At run-time, `a` is first evaluated.</span></span> <span data-ttu-id="dec56-2349">Pokud `a` nemá hodnotu null, `a` je neobalený, aby typ `A0` (Pokud `A` existuje a může mít hodnotu Null) a převedeny na typ `B`, a toto řešení je výsledek.</span><span class="sxs-lookup"><span data-stu-id="dec56-2349">If `a` is not null, `a` is unwrapped to type `A0` (if `A` exists and is nullable) and converted to type `B`, and this becomes the result.</span></span> <span data-ttu-id="dec56-2350">V opačném případě `b` jsou vyhodnoceny a výsledek.</span><span class="sxs-lookup"><span data-stu-id="dec56-2350">Otherwise, `b` is evaluated and becomes the result.</span></span>
*  <span data-ttu-id="dec56-2351">V opačném případě `a` a `b` jsou nekompatibilní, za kompilace dojde k chybě.</span><span class="sxs-lookup"><span data-stu-id="dec56-2351">Otherwise, `a` and `b` are incompatible, and a compile-time error occurs.</span></span>

## <a name="conditional-operator"></a><span data-ttu-id="dec56-2352">Podmíněný operátor</span><span class="sxs-lookup"><span data-stu-id="dec56-2352">Conditional operator</span></span>

<span data-ttu-id="dec56-2353">`?:` Operátor se nazývá podmiňovací operátor.</span><span class="sxs-lookup"><span data-stu-id="dec56-2353">The `?:` operator is called the conditional operator.</span></span> <span data-ttu-id="dec56-2354">V některých případech také nazývá tříhodnotový operátor.</span><span class="sxs-lookup"><span data-stu-id="dec56-2354">It is at times also called the ternary operator.</span></span>

```antlr
conditional_expression
    : null_coalescing_expression
    | null_coalescing_expression '?' expression ':' expression
    ;
```

<span data-ttu-id="dec56-2355">Podmíněný výraz ve tvaru `b ? x : y` nejprve tuto podmínku vyhodnotí `b`.</span><span class="sxs-lookup"><span data-stu-id="dec56-2355">A conditional expression of the form `b ? x : y` first evaluates the condition `b`.</span></span> <span data-ttu-id="dec56-2356">Když se poté `b` je `true`, `x` jsou vyhodnoceny a výsledek operace.</span><span class="sxs-lookup"><span data-stu-id="dec56-2356">Then, if `b` is `true`, `x` is evaluated and becomes the result of the operation.</span></span> <span data-ttu-id="dec56-2357">V opačném případě `y` jsou vyhodnoceny a výsledek operace.</span><span class="sxs-lookup"><span data-stu-id="dec56-2357">Otherwise, `y` is evaluated and becomes the result of the operation.</span></span> <span data-ttu-id="dec56-2358">Podmíněný výraz nikdy vyhodnotí oba `x` a `y`.</span><span class="sxs-lookup"><span data-stu-id="dec56-2358">A conditional expression never evaluates both `x` and `y`.</span></span>

<span data-ttu-id="dec56-2359">Podmíněný operátor je asociativní zprava, což znamená, že operace se seskupují zleva doprava.</span><span class="sxs-lookup"><span data-stu-id="dec56-2359">The conditional operator is right-associative, meaning that operations are grouped from right to left.</span></span> <span data-ttu-id="dec56-2360">Například výraz ve tvaru `a ? b : c ? d : e` se vyhodnotí jako `a ? b : (c ? d : e)`.</span><span class="sxs-lookup"><span data-stu-id="dec56-2360">For example, an expression of the form `a ? b : c ? d : e` is evaluated as `a ? b : (c ? d : e)`.</span></span>

<span data-ttu-id="dec56-2361">První operand `?:` operator musí být výraz, který lze implicitně převést na `bool`, nebo výraz typu, který implementuje `operator true`.</span><span class="sxs-lookup"><span data-stu-id="dec56-2361">The first operand of the `?:` operator must be an expression that can be implicitly converted to `bool`, or an expression of a type that implements `operator true`.</span></span> <span data-ttu-id="dec56-2362">Pokud ani jeden z těchto požadavků není splněna, dojde k chybě v době kompilace.</span><span class="sxs-lookup"><span data-stu-id="dec56-2362">If neither of these requirements is satisfied, a compile-time error occurs.</span></span>

<span data-ttu-id="dec56-2363">Druhý a třetí operand `x` a `y`, z `?:` operátor řídit typ podmíněného výrazu.</span><span class="sxs-lookup"><span data-stu-id="dec56-2363">The second and third operands, `x` and `y`, of the `?:` operator control the type of the conditional expression.</span></span>

*  <span data-ttu-id="dec56-2364">Pokud `x` má typ `X` a `y` má typ `Y` pak</span><span class="sxs-lookup"><span data-stu-id="dec56-2364">If `x` has type `X` and `y` has type `Y` then</span></span>
   * <span data-ttu-id="dec56-2365">Pokud implicitní převod ([implicitních převodů](conversions.md#implicit-conversions)) existuje z `X` k `Y`, ale ne z `Y` k `X`, pak `Y` je typ podmíněného výrazu.</span><span class="sxs-lookup"><span data-stu-id="dec56-2365">If an implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)) exists from `X` to `Y`, but not from `Y` to `X`, then `Y` is the type of the conditional expression.</span></span>
   * <span data-ttu-id="dec56-2366">Pokud implicitní převod ([implicitních převodů](conversions.md#implicit-conversions)) existuje z `Y` k `X`, ale ne z `X` k `Y`, pak `X` je typ podmíněného výrazu.</span><span class="sxs-lookup"><span data-stu-id="dec56-2366">If an implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)) exists from `Y` to `X`, but not from `X` to `Y`, then `X` is the type of the conditional expression.</span></span>
   * <span data-ttu-id="dec56-2367">V opačném případě se dá určit bez typu výrazu a dojde k chybě kompilace.</span><span class="sxs-lookup"><span data-stu-id="dec56-2367">Otherwise, no expression type can be determined, and a compile-time error occurs.</span></span>
*  <span data-ttu-id="dec56-2368">Pokud pouze jeden z `x` a `y` má typ a obě `x` a `y`, aplikace se implicitně převést na typ, pak je typ podmíněného výrazu.</span><span class="sxs-lookup"><span data-stu-id="dec56-2368">If only one of `x` and `y` has a type, and both `x` and `y`, of are implicitly convertible to that type, then that is the type of the conditional expression.</span></span>
*  <span data-ttu-id="dec56-2369">V opačném případě se dá určit bez typu výrazu a dojde k chybě kompilace.</span><span class="sxs-lookup"><span data-stu-id="dec56-2369">Otherwise, no expression type can be determined, and a compile-time error occurs.</span></span>

<span data-ttu-id="dec56-2370">Zpracování za běhu podmíněného výrazu v podobě `b ? x : y` se skládá z následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="dec56-2370">The run-time processing of a conditional expression of the form `b ? x : y` consists of the following steps:</span></span>

*  <span data-ttu-id="dec56-2371">První, `b` je vyhodnocen a `bool` hodnotu `b` závisí:</span><span class="sxs-lookup"><span data-stu-id="dec56-2371">First, `b` is evaluated, and the `bool` value of `b` is determined:</span></span>
   * <span data-ttu-id="dec56-2372">Pokud implicitní převod z typu `b` k `bool` existuje, tento implicitní převod se provádí za účelem vytvoření `bool` hodnotu.</span><span class="sxs-lookup"><span data-stu-id="dec56-2372">If an implicit conversion from the type of `b` to `bool` exists, then this implicit conversion is performed to produce a `bool` value.</span></span>
   * <span data-ttu-id="dec56-2373">V opačném případě `operator true` definovaný podle typu `b` se vyvolá k vytvoření `bool` hodnotu.</span><span class="sxs-lookup"><span data-stu-id="dec56-2373">Otherwise, the `operator true` defined by the type of `b` is invoked to produce a `bool` value.</span></span>
*  <span data-ttu-id="dec56-2374">Pokud `bool` hodnotu vytvořenou testovaným výše uvedeném kroku je `true`, pak `x` je vyhodnotit a převedeny na typ podmíněného výrazu, a toto řešení je výsledek podmíněného výrazu.</span><span class="sxs-lookup"><span data-stu-id="dec56-2374">If the `bool` value produced by the step above is `true`, then `x` is evaluated and converted to the type of the conditional expression, and this becomes the result of the conditional expression.</span></span>
*  <span data-ttu-id="dec56-2375">V opačném případě `y` je vyhodnotit a převedeny na typ podmíněného výrazu, a to se stane výsledek podmíněného výrazu.</span><span class="sxs-lookup"><span data-stu-id="dec56-2375">Otherwise, `y` is evaluated and converted to the type of the conditional expression, and this becomes the result of the conditional expression.</span></span>

## <a name="anonymous-function-expressions"></a><span data-ttu-id="dec56-2376">Výrazy anonymní funkce</span><span class="sxs-lookup"><span data-stu-id="dec56-2376">Anonymous function expressions</span></span>

<span data-ttu-id="dec56-2377">***Anonymní funkce*** je výraz, který představuje definici metody "in-line".</span><span class="sxs-lookup"><span data-stu-id="dec56-2377">An ***anonymous function*** is an expression that represents an "in-line" method definition.</span></span> <span data-ttu-id="dec56-2378">Anonymní funkce nemá hodnotu nebo typ v a sama o sobě, ale lze převést na kompatibilní typ. strom delegáta nebo výraz.</span><span class="sxs-lookup"><span data-stu-id="dec56-2378">An anonymous function does not have a value or type in and of itself, but is convertible to a compatible delegate or expression tree type.</span></span> <span data-ttu-id="dec56-2379">Vyhodnocení anonymní funkce převodu závisí na cílový typ převodu: Pokud je typ delegátu, převod vyhodnocen na hodnotu delegáta, které se odkazuje na metodu, která definuje anonymní funkce.</span><span class="sxs-lookup"><span data-stu-id="dec56-2379">The evaluation of an anonymous function conversion depends on the target type of the conversion: If it is a delegate type, the conversion evaluates to a delegate value referencing the method which the anonymous function defines.</span></span> <span data-ttu-id="dec56-2380">Pokud je typu stromu výrazu, se vyhodnotí jako převod na strom výrazu, která reprezentuje strukturu těchto metodu jako objektovou strukturu.</span><span class="sxs-lookup"><span data-stu-id="dec56-2380">If it is an expression tree type, the conversion evaluates to an expression tree which represents the structure of the method as an object structure.</span></span>

<span data-ttu-id="dec56-2381">Z historických důvodů jsou mají dvě syntaktické varianty anonymní funkce, a to *lambda_expression*s a *anonymous_method_expression*s.</span><span class="sxs-lookup"><span data-stu-id="dec56-2381">For historical reasons there are two syntactic flavors of anonymous functions, namely *lambda_expression*s and *anonymous_method_expression*s.</span></span> <span data-ttu-id="dec56-2382">Pro téměř všechny účely *lambda_expression*s jsou stručné a expresivní než *anonymous_method_expression*s, která zůstanou v jazyce pro zpětnou kompatibilitu.</span><span class="sxs-lookup"><span data-stu-id="dec56-2382">For almost all purposes, *lambda_expression*s are more concise and expressive than *anonymous_method_expression*s, which remain in the language for backwards compatibility.</span></span>

```antlr
lambda_expression
    : anonymous_function_signature '=>' anonymous_function_body
    ;

anonymous_method_expression
    : 'delegate' explicit_anonymous_function_signature? block
    ;

anonymous_function_signature
    : explicit_anonymous_function_signature
    | implicit_anonymous_function_signature
    ;

explicit_anonymous_function_signature
    : '(' explicit_anonymous_function_parameter_list? ')'
    ;

explicit_anonymous_function_parameter_list
    : explicit_anonymous_function_parameter (',' explicit_anonymous_function_parameter)*
    ;

explicit_anonymous_function_parameter
    : anonymous_function_parameter_modifier? type identifier
    ;

anonymous_function_parameter_modifier
    : 'ref'
    | 'out'
    ;

implicit_anonymous_function_signature
    : '(' implicit_anonymous_function_parameter_list? ')'
    | implicit_anonymous_function_parameter
    ;

implicit_anonymous_function_parameter_list
    : implicit_anonymous_function_parameter (',' implicit_anonymous_function_parameter)*
    ;

implicit_anonymous_function_parameter
    : identifier
    ;

anonymous_function_body
    : expression
    | block
    ;
```

<span data-ttu-id="dec56-2383">`=>` Operátor má stejnou prioritu jako přiřazení (`=`) a je asociativní zprava.</span><span class="sxs-lookup"><span data-stu-id="dec56-2383">The `=>` operator has the same precedence as assignment (`=`) and is right-associative.</span></span>

<span data-ttu-id="dec56-2384">Anonymní funkce s `async` Modifikátor je asynchronní funkci a postupuje pravidel popsaných v [iterátory](classes.md#iterators).</span><span class="sxs-lookup"><span data-stu-id="dec56-2384">An anonymous function with the `async` modifier is an async function and follows the rules described in [Iterators](classes.md#iterators).</span></span>

<span data-ttu-id="dec56-2385">Parametry anonymní funkce v podobě *lambda_expression* může být explicitně nebo implicitně typu.</span><span class="sxs-lookup"><span data-stu-id="dec56-2385">The parameters of an anonymous function in the form of a *lambda_expression* can be explicitly or implicitly typed.</span></span> <span data-ttu-id="dec56-2386">V seznamu parametrů explicitně je výslovně uvedeno každý parametr typu.</span><span class="sxs-lookup"><span data-stu-id="dec56-2386">In an explicitly typed parameter list, the type of each parameter is explicitly stated.</span></span> <span data-ttu-id="dec56-2387">V seznamu implicitně typované parametrů typy parametrů jsou odvozeny z kontextu, ve kterém dochází anonymní funkce – konkrétně, když je anonymní funkce převedena na typ kompatibilní delegáta nebo typu stromu výrazu, který poskytuje typ typy parametrů ([anonymní funkce převody](conversions.md#anonymous-function-conversions)).</span><span class="sxs-lookup"><span data-stu-id="dec56-2387">In an implicitly typed parameter list, the types of the parameters are inferred from the context in which the anonymous function occurs—specifically, when the anonymous function is converted to a compatible delegate type or expression tree type, that type provides the parameter types ([Anonymous function conversions](conversions.md#anonymous-function-conversions)).</span></span>

<span data-ttu-id="dec56-2388">Anonymní funkce s parametrem jeden, implicitně typované můžete vynechat závorky ze seznamu parametrů.</span><span class="sxs-lookup"><span data-stu-id="dec56-2388">In an anonymous function with a single, implicitly typed parameter, the parentheses may be omitted from the parameter list.</span></span> <span data-ttu-id="dec56-2389">Jinými slovy anonymní funkci formuláře</span><span class="sxs-lookup"><span data-stu-id="dec56-2389">In other words, an anonymous function of the form</span></span>
```csharp
( param ) => expr
```
<span data-ttu-id="dec56-2390">lze zkrátit na</span><span class="sxs-lookup"><span data-stu-id="dec56-2390">can be abbreviated to</span></span>
```csharp
param => expr
```

<span data-ttu-id="dec56-2391">Seznam parametrů anonymní funkce v podobě *anonymous_method_expression* je volitelný.</span><span class="sxs-lookup"><span data-stu-id="dec56-2391">The parameter list of an anonymous function in the form of an *anonymous_method_expression* is optional.</span></span> <span data-ttu-id="dec56-2392">Pokud tento parametr zadaný, parametry musí být explicitně určeny typy.</span><span class="sxs-lookup"><span data-stu-id="dec56-2392">If given, the parameters must be explicitly typed.</span></span> <span data-ttu-id="dec56-2393">Pokud ne, je převeden na delegáta se žádné parametry anonymní funkce seznamu neobsahující `out` parametry.</span><span class="sxs-lookup"><span data-stu-id="dec56-2393">If not, the anonymous function is convertible to a delegate with any parameter list not containing `out` parameters.</span></span>

<span data-ttu-id="dec56-2394">A *bloku* tělo anonymní funkce je dostupná ([koncové body a dostupnosti](statements.md#end-points-and-reachability)) Pokud do nedostupný příkaz nedojde anonymní funkce.</span><span class="sxs-lookup"><span data-stu-id="dec56-2394">A *block* body of an anonymous function is reachable ([End points and reachability](statements.md#end-points-and-reachability)) unless the anonymous function occurs inside an unreachable statement.</span></span>

<span data-ttu-id="dec56-2395">Některé příklady anonymní funkce, postupujte podle následujících:</span><span class="sxs-lookup"><span data-stu-id="dec56-2395">Some examples of anonymous functions follow below:</span></span>

```csharp
x => x + 1                              // Implicitly typed, expression body
x => { return x + 1; }                  // Implicitly typed, statement body
(int x) => x + 1                        // Explicitly typed, expression body
(int x) => { return x + 1; }            // Explicitly typed, statement body
(x, y) => x * y                         // Multiple parameters
() => Console.WriteLine()               // No parameters
async (t1,t2) => await t1 + await t2    // Async
delegate (int x) { return x + 1; }      // Anonymous method expression
delegate { return 1 + 1; }              // Parameter list omitted
```

<span data-ttu-id="dec56-2396">Chování *lambda_expression*s a *anonymous_method_expression*s je stejná s výjimkou následujících bodů:</span><span class="sxs-lookup"><span data-stu-id="dec56-2396">The behavior of *lambda_expression*s and *anonymous_method_expression*s is the same except for the following points:</span></span>

*  <span data-ttu-id="dec56-2397">*anonymous_method_expression*s povolit seznam parametrů pro zcela vynechat získávání převoditelnosti na typy v seznamu parametrů hodnot delegátů.</span><span class="sxs-lookup"><span data-stu-id="dec56-2397">*anonymous_method_expression*s permit the parameter list to be omitted entirely, yielding convertibility to delegate types of any list of value parameters.</span></span>
*  <span data-ttu-id="dec56-2398">*lambda_expression*povolit s typy parametrů pro tento parametr vynechán a odvodit, zatímco *anonymous_method_expression*vyžadují s typy parametrů pro výslovně uvedeno.</span><span class="sxs-lookup"><span data-stu-id="dec56-2398">*lambda_expression*s permit parameter types to be omitted and inferred whereas *anonymous_method_expression*s require parameter types to be explicitly stated.</span></span>
*  <span data-ttu-id="dec56-2399">Text *lambda_expression* může být výraz nebo blok příkazů, že text *anonymous_method_expression* musí být blok příkazů.</span><span class="sxs-lookup"><span data-stu-id="dec56-2399">The body of a *lambda_expression* can be an expression or a statement block whereas the body of an *anonymous_method_expression* must be a statement block.</span></span>
*  <span data-ttu-id="dec56-2400">Pouze *lambda_expression*y mají převody na typy stromu výrazů kompatibilní ([typy stromu výrazů](types.md#expression-tree-types)).</span><span class="sxs-lookup"><span data-stu-id="dec56-2400">Only *lambda_expression*s have conversions to compatible expression tree types ([Expression tree types](types.md#expression-tree-types)).</span></span>

### <a name="anonymous-function-signatures"></a><span data-ttu-id="dec56-2401">Anonymní funkce podpisy</span><span class="sxs-lookup"><span data-stu-id="dec56-2401">Anonymous function signatures</span></span>

<span data-ttu-id="dec56-2402">Volitelný *anonymous_function_signature* anonymní funkce definuje název a volitelně typy formálních parametrů pro anonymní funkce.</span><span class="sxs-lookup"><span data-stu-id="dec56-2402">The optional *anonymous_function_signature* of an anonymous function defines the names and optionally the types of the formal parameters for the anonymous function.</span></span> <span data-ttu-id="dec56-2403">Rozsah parametry anonymní funkce je *anonymous_function_body*.</span><span class="sxs-lookup"><span data-stu-id="dec56-2403">The scope of the parameters of the anonymous function is the *anonymous_function_body*.</span></span> <span data-ttu-id="dec56-2404">([Obory](basic-concepts.md#scopes)) společně s seznamu parametrů (Pokud je zadaný) představuje anonymní--tělo metody místa deklarace ([deklarace](basic-concepts.md#declarations)).</span><span class="sxs-lookup"><span data-stu-id="dec56-2404">([Scopes](basic-concepts.md#scopes)) Together with the parameter list (if given) the anonymous-method-body constitutes a declaration space ([Declarations](basic-concepts.md#declarations)).</span></span> <span data-ttu-id="dec56-2405">Proto je chyba kompilace pro název parametru anonymní funkce, která se shodovat s názvem lokální proměnná, lokální konstanta ani parametr, jehož obor zahrnuje *anonymous_method_expression* nebo *lambda_ výraz*.</span><span class="sxs-lookup"><span data-stu-id="dec56-2405">It is thus a compile-time error for the name of a parameter of the anonymous function to match the name of a local variable, local constant or parameter whose scope includes the *anonymous_method_expression* or *lambda_expression*.</span></span>

<span data-ttu-id="dec56-2406">Pokud je anonymní funkce *explicit_anonymous_function_signature*, pak sada delegáta kompatibilní typy a typy stromu výrazů je omezené na ty, které mají stejné typy parametrů a modifikátory ve stejném pořadí.</span><span class="sxs-lookup"><span data-stu-id="dec56-2406">If an anonymous function has an *explicit_anonymous_function_signature*, then the set of compatible delegate types and expression tree types is restricted to those that have the same parameter types and modifiers in the same order.</span></span> <span data-ttu-id="dec56-2407">Na rozdíl od skupiny převody – metoda ([Metoda skupiny převody](conversions.md#method-group-conversions)), odchylku opravné položky k typy parametrů anonymní funkce není podporována.</span><span class="sxs-lookup"><span data-stu-id="dec56-2407">In contrast to method group conversions ([Method group conversions](conversions.md#method-group-conversions)), contra-variance of anonymous function parameter types is not supported.</span></span> <span data-ttu-id="dec56-2408">Pokud anonymní funkce nemá *anonymous_function_signature*, pak sada delegáta kompatibilní typy a typy stromu výrazů je omezené na ty, které nemají `out` parametry.</span><span class="sxs-lookup"><span data-stu-id="dec56-2408">If an anonymous function does not have an *anonymous_function_signature*, then the set of compatible delegate types and expression tree types is restricted to those that have no `out` parameters.</span></span>

<span data-ttu-id="dec56-2409">Všimněte si, že *anonymous_function_signature* nesmí obsahovat atributy nebo pole parametrů.</span><span class="sxs-lookup"><span data-stu-id="dec56-2409">Note that an *anonymous_function_signature* cannot include attributes or a parameter array.</span></span> <span data-ttu-id="dec56-2410">Nicméně *anonymous_function_signature* může být kompatibilní s typem delegáta, jehož seznam parametrů obsahuje pole parametrů.</span><span class="sxs-lookup"><span data-stu-id="dec56-2410">Nevertheless, an *anonymous_function_signature* may be compatible with a delegate type whose parameter list contains a parameter array.</span></span>

<span data-ttu-id="dec56-2411">Nezapomeňte tento převod do typu stromu výrazu, i v případě kompatibilní, může stále selžou v době kompilace ([typy stromu výrazů](types.md#expression-tree-types)).</span><span class="sxs-lookup"><span data-stu-id="dec56-2411">Note also that conversion to an expression tree type, even if compatible, may still fail at compile-time ([Expression tree types](types.md#expression-tree-types)).</span></span>

### <a name="anonymous-function-bodies"></a><span data-ttu-id="dec56-2412">Anonymní funkce těla</span><span class="sxs-lookup"><span data-stu-id="dec56-2412">Anonymous function bodies</span></span>

<span data-ttu-id="dec56-2413">Text (*výraz* nebo *bloku*) anonymní funkce se vztahují následující pravidla:</span><span class="sxs-lookup"><span data-stu-id="dec56-2413">The body (*expression* or *block*) of an anonymous function is subject to the following rules:</span></span>

*  <span data-ttu-id="dec56-2414">Pokud anonymní funkce obsahuje podpis, jsou k dispozici v těle parametry zadaná v signatuře.</span><span class="sxs-lookup"><span data-stu-id="dec56-2414">If the anonymous function includes a signature, the parameters specified in the signature are available in the body.</span></span> <span data-ttu-id="dec56-2415">Pokud anonymní funkce nemá žádný podpis lze převést na typ delegáta nebo typ výrazu s parametry ([anonymní funkce převody](conversions.md#anonymous-function-conversions)), ale parametry nelze získat přístup v textu.</span><span class="sxs-lookup"><span data-stu-id="dec56-2415">If the anonymous function has no signature it can be converted to a delegate type or expression type having parameters ([Anonymous function conversions](conversions.md#anonymous-function-conversions)), but the parameters cannot be accessed in the body.</span></span>
*  <span data-ttu-id="dec56-2416">S výjimkou `ref` nebo `out` parametry zadaná v signatuře (pokud existuje) nejbližšího obklopujícího anonymní funkce, je chyba kompilace pro tělo pro přístup k `ref` nebo `out` parametru.</span><span class="sxs-lookup"><span data-stu-id="dec56-2416">Except for `ref` or `out` parameters specified in the signature (if any) of the nearest enclosing anonymous function, it is a compile-time error for the body to access a `ref` or `out` parameter.</span></span>
*  <span data-ttu-id="dec56-2417">Pokud typ `this` je typ struktury je chyba kompilace pro tělo pro přístup k `this`.</span><span class="sxs-lookup"><span data-stu-id="dec56-2417">When the type of `this` is a struct type, it is a compile-time error for the body to access `this`.</span></span> <span data-ttu-id="dec56-2418">Je hodnota true Určuje, zda je explicitní přístup (jako v `this.x`) nebo implicitní (jako v `x` kde `x` je členem instance struktury).</span><span class="sxs-lookup"><span data-stu-id="dec56-2418">This is true whether the access is explicit (as in `this.x`) or implicit (as in `x` where `x` is an instance member of the struct).</span></span> <span data-ttu-id="dec56-2419">Toto pravidlo jednoduše zakazují takový přístup a nemá vliv, zda člen vyhledávání výsledkem členem struktury.</span><span class="sxs-lookup"><span data-stu-id="dec56-2419">This rule simply prohibits such access and does not affect whether member lookup results in a member of the struct.</span></span>
*  <span data-ttu-id="dec56-2420">Text má přístup do vnější proměnné ([vnější proměnné](expressions.md#outer-variables)) anonymní funkce.</span><span class="sxs-lookup"><span data-stu-id="dec56-2420">The body has access to the outer variables ([Outer variables](expressions.md#outer-variables)) of the anonymous function.</span></span> <span data-ttu-id="dec56-2421">Přístup vnější proměnné odkazovat na instanci proměnné, která je aktivní v okamžiku *lambda_expression* nebo *anonymous_method_expression* vyhodnocena ([hodnocení anonymní funkce výrazy](expressions.md#evaluation-of-anonymous-function-expressions)).</span><span class="sxs-lookup"><span data-stu-id="dec56-2421">Access of an outer variable will reference the instance of the variable that is active at the time the *lambda_expression* or *anonymous_method_expression* is evaluated ([Evaluation of anonymous function expressions](expressions.md#evaluation-of-anonymous-function-expressions)).</span></span>
*  <span data-ttu-id="dec56-2422">Je chyba kompilace pro text tak, aby obsahovala `goto` příkazu `break` příkazu, nebo `continue` jehož cíl je mimo tělo, nebo v těle obsažené anonymní funkce.</span><span class="sxs-lookup"><span data-stu-id="dec56-2422">It is a compile-time error for the body to contain a `goto` statement, `break` statement, or `continue` statement whose target is outside the body or within the body of a contained anonymous function.</span></span>
*  <span data-ttu-id="dec56-2423">A `return` příkaz v těle nevrátí řízení z nejbližšího obklopujícího vyvolání anonymní funkce, nikoli z nadřazeného členské funkce.</span><span class="sxs-lookup"><span data-stu-id="dec56-2423">A `return` statement in the body returns control from an invocation of the nearest enclosing anonymous function, not from the enclosing function member.</span></span> <span data-ttu-id="dec56-2424">Výraz zadaný v `return` příkaz musí být implicitně převoditelná na návratový typ na typ delegáta nebo typu stromu výrazu, ke kterému nejbližšího obklopujícího *lambda_expression* nebo *anonymous_ method_expression* převeden ([anonymní funkce převody](conversions.md#anonymous-function-conversions)).</span><span class="sxs-lookup"><span data-stu-id="dec56-2424">An expression specified in a `return` statement must be implicitly convertible to the return type of the delegate type or expression tree type to which the nearest enclosing *lambda_expression* or *anonymous_method_expression* is converted ([Anonymous function conversions](conversions.md#anonymous-function-conversions)).</span></span>

<span data-ttu-id="dec56-2425">Je explicitně nespecifikovaná, zda neexistuje žádný způsob, jak spustit blok anonymní funkce jiných než prostřednictvím zkušební verze a vyvolání *lambda_expression* nebo *anonymous_method_expression*.</span><span class="sxs-lookup"><span data-stu-id="dec56-2425">It is explicitly unspecified whether there is any way to execute the block of an anonymous function other than through evaluation and invocation of the *lambda_expression* or *anonymous_method_expression*.</span></span> <span data-ttu-id="dec56-2426">Konkrétně se kompilátor můžete implementovat anonymní funkci syntetizační jeden nebo více metod nebo typy.</span><span class="sxs-lookup"><span data-stu-id="dec56-2426">In particular, the compiler may choose to implement an anonymous function by synthesizing one or more named methods or types.</span></span> <span data-ttu-id="dec56-2427">Názvy těchto syntetizovaný prvků musí být vyhrazená pro použití kompilátoru formuláře.</span><span class="sxs-lookup"><span data-stu-id="dec56-2427">The names of any such synthesized elements must be of a form reserved for compiler use.</span></span>

### <a name="overload-resolution-and-anonymous-functions"></a><span data-ttu-id="dec56-2428">Rozlišení přetížení a anonymní funkce</span><span class="sxs-lookup"><span data-stu-id="dec56-2428">Overload resolution and anonymous functions</span></span>

<span data-ttu-id="dec56-2429">Anonymní funkce v seznam argumentů účastnit odvození typu proměnné a řešení přetížení.</span><span class="sxs-lookup"><span data-stu-id="dec56-2429">Anonymous functions in an argument list participate in type inference and overload resolution.</span></span> <span data-ttu-id="dec56-2430">Najdete [odvození typu](expressions.md#type-inference) a [rozlišení přetěžování](expressions.md#overload-resolution) přesná pravidla.</span><span class="sxs-lookup"><span data-stu-id="dec56-2430">Please refer to [Type inference](expressions.md#type-inference) and [Overload resolution](expressions.md#overload-resolution) for the exact rules.</span></span>

<span data-ttu-id="dec56-2431">Následující příklad ukazuje účinek anonymní funkce v řešení přetížení.</span><span class="sxs-lookup"><span data-stu-id="dec56-2431">The following example illustrates the effect of anonymous functions on overload resolution.</span></span>

```csharp
class ItemList<T>: List<T>
{
    public int Sum(Func<T,int> selector) {
        int sum = 0;
        foreach (T item in this) sum += selector(item);
        return sum;
    }

    public double Sum(Func<T,double> selector) {
        double sum = 0;
        foreach (T item in this) sum += selector(item);
        return sum;
    }
}
```

<span data-ttu-id="dec56-2432">`ItemList<T>` Třída má dvě `Sum` metody.</span><span class="sxs-lookup"><span data-stu-id="dec56-2432">The `ItemList<T>` class has two `Sum` methods.</span></span> <span data-ttu-id="dec56-2433">Každý má `selector` argument, který extrahuje hodnotu Součet přes ze seznamu položek.</span><span class="sxs-lookup"><span data-stu-id="dec56-2433">Each takes a `selector` argument, which extracts the value to sum over from a list item.</span></span> <span data-ttu-id="dec56-2434">Extrahovaná hodnota může být buď `int` nebo `double` a výsledný součet je rovněž `int` nebo `double`.</span><span class="sxs-lookup"><span data-stu-id="dec56-2434">The extracted value can be either an `int` or a `double` and the resulting sum is likewise either an `int` or a `double`.</span></span>

<span data-ttu-id="dec56-2435">`Sum` Metody může třeba vypočítat součtů ze seznamu řádky podrobností v pořadí.</span><span class="sxs-lookup"><span data-stu-id="dec56-2435">The `Sum` methods could for example be used to compute sums from a list of detail lines in an order.</span></span>

```csharp
class Detail
{
    public int UnitCount;
    public double UnitPrice;
    ...
}

void ComputeSums() {
    ItemList<Detail> orderDetails = GetOrderDetails(...);
    int totalUnits = orderDetails.Sum(d => d.UnitCount);
    double orderTotal = orderDetails.Sum(d => d.UnitPrice * d.UnitCount);
    ...
}
```

<span data-ttu-id="dec56-2436">V prvním vyvoláním služby `orderDetails.Sum`, obě `Sum` metody jsou použitelné protože anonymní funkce `d => d. UnitCount` je kompatibilní s oběma `Func<Detail,int>` a `Func<Detail,double>`.</span><span class="sxs-lookup"><span data-stu-id="dec56-2436">In the first invocation of `orderDetails.Sum`, both `Sum` methods are applicable because the anonymous function `d => d. UnitCount` is compatible with both `Func<Detail,int>` and `Func<Detail,double>`.</span></span> <span data-ttu-id="dec56-2437">Ale řešení přetížení vybere první `Sum` metoda protože převod na `Func<Detail,int>` je obecně lepší než převod na `Func<Detail,double>`.</span><span class="sxs-lookup"><span data-stu-id="dec56-2437">However, overload resolution picks the first `Sum` method because the conversion to `Func<Detail,int>` is better than the conversion to `Func<Detail,double>`.</span></span>

<span data-ttu-id="dec56-2438">V druhém volání `orderDetails.Sum`, pouze druhý `Sum` metodu je možné použít protože anonymní funkce `d => d.UnitPrice * d.UnitCount` produkuje hodnotu typu `double`.</span><span class="sxs-lookup"><span data-stu-id="dec56-2438">In the second invocation of `orderDetails.Sum`, only the second `Sum` method is applicable because the anonymous function `d => d.UnitPrice * d.UnitCount` produces a value of type `double`.</span></span> <span data-ttu-id="dec56-2439">Proto přetížení rozlišení zvolí druhý `Sum` metodu pro tohoto volání.</span><span class="sxs-lookup"><span data-stu-id="dec56-2439">Thus, overload resolution picks the second `Sum` method for that invocation.</span></span>

### <a name="anonymous-functions-and-dynamic-binding"></a><span data-ttu-id="dec56-2440">Anonymní funkce a dynamické vazby</span><span class="sxs-lookup"><span data-stu-id="dec56-2440">Anonymous functions and dynamic binding</span></span>

<span data-ttu-id="dec56-2441">Anonymní funkce nemůže být příjemce, argument nebo operand dynamicky vázané operace.</span><span class="sxs-lookup"><span data-stu-id="dec56-2441">An anonymous function cannot be a receiver, argument or operand of a dynamically bound operation.</span></span>

### <a name="outer-variables"></a><span data-ttu-id="dec56-2442">Vnější proměnné</span><span class="sxs-lookup"><span data-stu-id="dec56-2442">Outer variables</span></span>

<span data-ttu-id="dec56-2443">Všechny místní proměnná, parametr hodnoty nebo pole parametrů, jehož obor zahrnuje *lambda_expression* nebo *anonymous_method_expression* je volána ***vnější proměnné*** anonymní funkce.</span><span class="sxs-lookup"><span data-stu-id="dec56-2443">Any local variable, value parameter, or parameter array whose scope includes the *lambda_expression* or *anonymous_method_expression* is called an ***outer variable*** of the anonymous function.</span></span> <span data-ttu-id="dec56-2444">V instanci funkce člena třídy `this` hodnoty je považován za hodnotu parametru a vnější proměnný všechny anonymní funkce obsažené v členské funkce.</span><span class="sxs-lookup"><span data-stu-id="dec56-2444">In an instance function member of a class, the `this` value is considered a value parameter and is an outer variable of any anonymous function contained within the function member.</span></span>

#### <a name="captured-outer-variables"></a><span data-ttu-id="dec56-2445">Zachycené vnější proměnné</span><span class="sxs-lookup"><span data-stu-id="dec56-2445">Captured outer variables</span></span>

<span data-ttu-id="dec56-2446">Když vnější proměnná odkazuje anonymní funkce, vnější proměnné se říká, že byly ***zachycené*** anonymní funkce.</span><span class="sxs-lookup"><span data-stu-id="dec56-2446">When an outer variable is referenced by an anonymous function, the outer variable is said to have been ***captured*** by the anonymous function.</span></span> <span data-ttu-id="dec56-2447">Obvykle je omezená na provedení bloku nebo příkazu, ke kterému je přidružené životního cyklu lokální proměnné ([lokální proměnné](variables.md#local-variables)).</span><span class="sxs-lookup"><span data-stu-id="dec56-2447">Ordinarily, the lifetime of a local variable is limited to execution of the block or statement with which it is associated ([Local variables](variables.md#local-variables)).</span></span> <span data-ttu-id="dec56-2448">Však životnost zachycené vnější proměnné je rozšířená alespoň do delegáta nebo strom výrazu, které jsou vytvořené z anonymní funkce je způsobilý pro uvolňování paměti.</span><span class="sxs-lookup"><span data-stu-id="dec56-2448">However, the lifetime of a captured outer variable is extended at least until the delegate or expression tree created from the anonymous function becomes eligible for garbage collection.</span></span>

<span data-ttu-id="dec56-2449">V příkladu</span><span class="sxs-lookup"><span data-stu-id="dec56-2449">In the example</span></span>
```csharp
using System;

delegate int D();

class Test
{
    static D F() {
        int x = 0;
        D result = () => ++x;
        return result;
    }

    static void Main() {
        D d = F();
        Console.WriteLine(d());
        Console.WriteLine(d());
        Console.WriteLine(d());
    }
}
```
<span data-ttu-id="dec56-2450">lokální proměnná `x` zachycena anonymní funkce a životnost `x` rozšířené alespoň do delegáta vrácená `F` stane nárok uvolňování paměti (které nedojde do konce velmi program).</span><span class="sxs-lookup"><span data-stu-id="dec56-2450">the local variable `x` is captured by the anonymous function, and the lifetime of `x` is extended at least until the delegate returned from `F` becomes eligible for garbage collection (which doesn't happen until the very end of the program).</span></span> <span data-ttu-id="dec56-2451">Protože každé vyvolání sady anonymní funkce pracuje na stejnou instanci `x`, výstup v příkladu je:</span><span class="sxs-lookup"><span data-stu-id="dec56-2451">Since each invocation of the anonymous function operates on the same instance of `x`, the output of the example is:</span></span>
```
1
2
3
```

<span data-ttu-id="dec56-2452">Při zachytávání lokální proměnná ani parametr hodnoty anonymní funkcí lokální proměnná ani parametr se již považuje za pevná proměnná ([Fixed a přesunutelný proměnné](unsafe-code.md#fixed-and-moveable-variables)), ale místo toho považováno za přesouvat Proměnná.</span><span class="sxs-lookup"><span data-stu-id="dec56-2452">When a local variable or a value parameter is captured by an anonymous function, the local variable or parameter is no longer considered to be a fixed variable ([Fixed and moveable variables](unsafe-code.md#fixed-and-moveable-variables)), but is instead considered to be a moveable variable.</span></span> <span data-ttu-id="dec56-2453">Proto všechny `unsafe` musíte nejprve použít kód, který přebírá adresu zachycené vnější proměnné `fixed` příkaz vyřešit proměnnou.</span><span class="sxs-lookup"><span data-stu-id="dec56-2453">Thus any `unsafe` code that takes the address of a captured outer variable must first use the `fixed` statement to fix the variable.</span></span>

<span data-ttu-id="dec56-2454">Všimněte si, že na rozdíl od nezachycené proměnnou, zachycené lokální proměnné může být současně vystavena více vláken provádění.</span><span class="sxs-lookup"><span data-stu-id="dec56-2454">Note that unlike an uncaptured variable, a captured local variable can be simultaneously exposed to multiple threads of execution.</span></span>

#### <a name="instantiation-of-local-variables"></a><span data-ttu-id="dec56-2455">Vytvoření instance lokální proměnné</span><span class="sxs-lookup"><span data-stu-id="dec56-2455">Instantiation of local variables</span></span>

<span data-ttu-id="dec56-2456">Místní proměnná se považuje za ***vytvořena instance*** při provádění zadá rozsah proměnné.</span><span class="sxs-lookup"><span data-stu-id="dec56-2456">A local variable is considered to be ***instantiated*** when execution enters the scope of the variable.</span></span> <span data-ttu-id="dec56-2457">Například když je vyvolána následující metodu, místní proměnná `x` je vytvořena instance a inicializován třikrát – jednou pro každou iteraci smyčky.</span><span class="sxs-lookup"><span data-stu-id="dec56-2457">For example, when the following method is invoked, the local variable `x` is instantiated and initialized three times—once for each iteration of the loop.</span></span>

```csharp
static void F() {
    for (int i = 0; i < 3; i++) {
        int x = i * 2 + 1;
        ...
    }
}
```

<span data-ttu-id="dec56-2458">Ale přesunutí deklarace `x` mimo smyčku výsledky v jedné instance `x`:</span><span class="sxs-lookup"><span data-stu-id="dec56-2458">However, moving the declaration of `x` outside the loop results in a single instantiation of `x`:</span></span>
```csharp
static void F() {
    int x;
    for (int i = 0; i < 3; i++) {
        x = i * 2 + 1;
        ...
    }
}
```

<span data-ttu-id="dec56-2459">Při není zachycena, neexistuje žádný způsob, jak sledovat, přesně jak často je vytvořena instance místní proměnné, protože životnosti vytváření instancí je nesouvislý, je možné, že každá instance jednoduše používat stejné umístění úložiště.</span><span class="sxs-lookup"><span data-stu-id="dec56-2459">When not captured, there is no way to observe exactly how often a local variable is instantiated—because the lifetimes of the instantiations are disjoint, it is possible for each instantiation to simply use the same storage location.</span></span> <span data-ttu-id="dec56-2460">Ale když anonymní funkci explicitně zaznamenává místní proměnnou, účinky instanciace stanou zjevnými.</span><span class="sxs-lookup"><span data-stu-id="dec56-2460">However, when an anonymous function captures a local variable, the effects of instantiation become apparent.</span></span>

<span data-ttu-id="dec56-2461">V příkladu</span><span class="sxs-lookup"><span data-stu-id="dec56-2461">The example</span></span>
```csharp
using System;

delegate void D();

class Test
{
    static D[] F() {
        D[] result = new D[3];
        for (int i = 0; i < 3; i++) {
            int x = i * 2 + 1;
            result[i] = () => { Console.WriteLine(x); };
        }
        return result;
    }

    static void Main() {
        foreach (D d in F()) d();
    }
}
```
<span data-ttu-id="dec56-2462">Vytvoří výstup:</span><span class="sxs-lookup"><span data-stu-id="dec56-2462">produces the output:</span></span>
```
1
3
5
```

<span data-ttu-id="dec56-2463">Nicméně, když deklarace `x` je přesunut mimo smyčku:</span><span class="sxs-lookup"><span data-stu-id="dec56-2463">However, when the declaration of `x` is moved outside the loop:</span></span>
```csharp
static D[] F() {
    D[] result = new D[3];
    int x;
    for (int i = 0; i < 3; i++) {
        x = i * 2 + 1;
        result[i] = () => { Console.WriteLine(x); };
    }
    return result;
}
```
<span data-ttu-id="dec56-2464">Výstup bude:</span><span class="sxs-lookup"><span data-stu-id="dec56-2464">the output is:</span></span>
```
5
5
5
```

<span data-ttu-id="dec56-2465">Pokud smyčky for vycházející z deklaruje proměnnou iterace, proměnné samotné se považuje za deklarované mimo smyčku.</span><span class="sxs-lookup"><span data-stu-id="dec56-2465">If a for-loop declares an iteration variable, that variable itself is considered to be declared outside of the loop.</span></span> <span data-ttu-id="dec56-2466">Proto pokud v příkladu se změní na zachycení proměnné iterace, sama:</span><span class="sxs-lookup"><span data-stu-id="dec56-2466">Thus, if the example is changed to capture the iteration variable itself:</span></span>

```csharp
static D[] F() {
    D[] result = new D[3];
    for (int i = 0; i < 3; i++) {
        result[i] = () => { Console.WriteLine(i); };
    }
    return result;
}
```
<span data-ttu-id="dec56-2467">je zachycena pouze jedna instance proměnné iterace, která vytvoří výstup:</span><span class="sxs-lookup"><span data-stu-id="dec56-2467">only one instance of the iteration variable is captured, which produces the output:</span></span>
```
3
3
3
```

<span data-ttu-id="dec56-2468">Je možné pro delegáty anonymní funkce pro sdílení některých zachyceným proměnným, ale mají samostatné instance ostatních.</span><span class="sxs-lookup"><span data-stu-id="dec56-2468">It is possible for anonymous function delegates to share some captured variables yet have separate instances of others.</span></span> <span data-ttu-id="dec56-2469">Například pokud `F` se změní na</span><span class="sxs-lookup"><span data-stu-id="dec56-2469">For example, if `F` is changed to</span></span>
```csharp
static D[] F() {
    D[] result = new D[3];
    int x = 0;
    for (int i = 0; i < 3; i++) {
        int y = 0;
        result[i] = () => { Console.WriteLine("{0} {1}", ++x, ++y); };
    }
    return result;
}
```
<span data-ttu-id="dec56-2470">tři delegáty zachycení stejnou instanci `x` ale samostatným instancím `y`, a zobrazí se výstup:</span><span class="sxs-lookup"><span data-stu-id="dec56-2470">the three delegates capture the same instance of `x` but separate instances of `y`, and the output is:</span></span>
```
1 1
2 1
3 1
```

<span data-ttu-id="dec56-2471">Samostatné anonymní funkce můžete zachytit stejnou instanci vnější proměnná.</span><span class="sxs-lookup"><span data-stu-id="dec56-2471">Separate anonymous functions can capture the same instance of an outer variable.</span></span> <span data-ttu-id="dec56-2472">V tomto příkladu:</span><span class="sxs-lookup"><span data-stu-id="dec56-2472">In the example:</span></span>
```csharp
using System;

delegate void Setter(int value);

delegate int Getter();

class Test
{
    static void Main() {
        int x = 0;
        Setter s = (int value) => { x = value; };
        Getter g = () => { return x; };
        s(5);
        Console.WriteLine(g());
        s(10);
        Console.WriteLine(g());
    }
}
```
<span data-ttu-id="dec56-2473">dvě anonymní funkce capture stejnou instanci lokální proměnná `x`, a jejich komunikace mohla probíhat tedy"" prostřednictvím dané proměnné.</span><span class="sxs-lookup"><span data-stu-id="dec56-2473">the two anonymous functions capture the same instance of the local variable `x`, and they can thus "communicate" through that variable.</span></span> <span data-ttu-id="dec56-2474">Výstup v příkladu je:</span><span class="sxs-lookup"><span data-stu-id="dec56-2474">The output of the example is:</span></span>
```
5
10
```

### <a name="evaluation-of-anonymous-function-expressions"></a><span data-ttu-id="dec56-2475">Vyhodnocování výrazů anonymní funkce</span><span class="sxs-lookup"><span data-stu-id="dec56-2475">Evaluation of anonymous function expressions</span></span>

<span data-ttu-id="dec56-2476">Anonymní funkce `F` vždy musí být převeden na typ delegáta `D` nebo typu stromu výrazu `E`, buď přímo nebo prostřednictvím spuštění výraz vytvářející delegáta `new D(F)`.</span><span class="sxs-lookup"><span data-stu-id="dec56-2476">An anonymous function `F` must always be converted to a delegate type `D` or an expression tree type `E`, either directly or through the execution of a delegate creation expression `new D(F)`.</span></span> <span data-ttu-id="dec56-2477">Tento převod určuje výsledek anonymní funkce, jak je popsáno v [anonymní funkce převody](conversions.md#anonymous-function-conversions).</span><span class="sxs-lookup"><span data-stu-id="dec56-2477">This conversion determines the result of the anonymous function, as described in [Anonymous function conversions](conversions.md#anonymous-function-conversions).</span></span>

## <a name="query-expressions"></a><span data-ttu-id="dec56-2478">Výrazy dotazů</span><span class="sxs-lookup"><span data-stu-id="dec56-2478">Query expressions</span></span>

<span data-ttu-id="dec56-2479">***Výrazy dotazu*** poskytují syntaxi jazyka pro dotazy, které je podobné jako u jazyků relačních a hierarchických dotazů, jako jsou SQL a XQuery.</span><span class="sxs-lookup"><span data-stu-id="dec56-2479">***Query expressions*** provide a language integrated syntax for queries that is similar to relational and hierarchical query languages such as SQL and XQuery.</span></span>

```antlr
query_expression
    : from_clause query_body
    ;

from_clause
    : 'from' type? identifier 'in' expression
    ;

query_body
    : query_body_clauses? select_or_group_clause query_continuation?
    ;

query_body_clauses
    : query_body_clause
    | query_body_clauses query_body_clause
    ;

query_body_clause
    : from_clause
    | let_clause
    | where_clause
    | join_clause
    | join_into_clause
    | orderby_clause
    ;

let_clause
    : 'let' identifier '=' expression
    ;

where_clause
    : 'where' boolean_expression
    ;

join_clause
    : 'join' type? identifier 'in' expression 'on' expression 'equals' expression
    ;

join_into_clause
    : 'join' type? identifier 'in' expression 'on' expression 'equals' expression 'into' identifier
    ;

orderby_clause
    : 'orderby' orderings
    ;

orderings
    : ordering (',' ordering)*
    ;

ordering
    : expression ordering_direction?
    ;

ordering_direction
    : 'ascending'
    | 'descending'
    ;

select_or_group_clause
    : select_clause
    | group_clause
    ;

select_clause
    : 'select' expression
    ;

group_clause
    : 'group' expression 'by' expression
    ;

query_continuation
    : 'into' identifier query_body
    ;
```

<span data-ttu-id="dec56-2480">Výraz dotazu začíná `from` klauzule a končí buď `select` nebo `group` klauzuli.</span><span class="sxs-lookup"><span data-stu-id="dec56-2480">A query expression begins with a `from` clause and ends with either a `select` or `group` clause.</span></span> <span data-ttu-id="dec56-2481">Počáteční `from` klauzule může následovat nula nebo více `from`, `let`, `where`, `join` nebo `orderby` klauzule.</span><span class="sxs-lookup"><span data-stu-id="dec56-2481">The initial `from` clause can be followed by zero or more `from`, `let`, `where`, `join` or `orderby` clauses.</span></span> <span data-ttu-id="dec56-2482">Každý `from` klauzule je generátor Představujeme ***proměnnou rozsahu*** které rozsahy nad elementy ***pořadí***.</span><span class="sxs-lookup"><span data-stu-id="dec56-2482">Each `from` clause is a generator introducing a ***range variable*** which ranges over the elements of a ***sequence***.</span></span> <span data-ttu-id="dec56-2483">Každý `let` klauzule představuje proměnnou rozsahu představující hodnotu vypočítat pomocí předchozích proměnných rozsahu.</span><span class="sxs-lookup"><span data-stu-id="dec56-2483">Each `let` clause introduces a range variable representing a value computed by means of previous range variables.</span></span> <span data-ttu-id="dec56-2484">Každý `where` klauzule je filtr, který vyloučí položky z výsledku.</span><span class="sxs-lookup"><span data-stu-id="dec56-2484">Each `where` clause is a filter that excludes items from the result.</span></span> <span data-ttu-id="dec56-2485">Každý `join` klauzule porovnání zadaného klíče zdrojové sekvence s klíči jiné pořadí, což má za následek odpovídající dvojice.</span><span class="sxs-lookup"><span data-stu-id="dec56-2485">Each `join` clause compares specified keys of the source sequence with keys of another sequence, yielding matching pairs.</span></span> <span data-ttu-id="dec56-2486">Každý `orderby` klauzule změní pořadí položek podle zadaných kritérií. Finální `select` nebo `group` klauzule určuje tvar výsledku z hlediska proměnné rozsahu.</span><span class="sxs-lookup"><span data-stu-id="dec56-2486">Each `orderby` clause reorders items according to specified criteria.The final `select` or `group` clause specifies the shape of the result in terms of the range variables.</span></span> <span data-ttu-id="dec56-2487">A konečně `into` klauzuli lze použít k "splice" dotazy zpracováním výsledků dotazu jako generátor v následných dotazu.</span><span class="sxs-lookup"><span data-stu-id="dec56-2487">Finally, an `into` clause can be used to "splice" queries by treating the results of one query as a generator in a subsequent query.</span></span>

### <a name="ambiguities-in-query-expressions"></a><span data-ttu-id="dec56-2488">Nejednoznačnosti ve výrazech dotazů</span><span class="sxs-lookup"><span data-stu-id="dec56-2488">Ambiguities in query expressions</span></span>

<span data-ttu-id="dec56-2489">Výrazy dotazů obsahují počet "kontextová klíčová slova", to znamená, identifikátory, které mají speciální význam v daném kontextu.</span><span class="sxs-lookup"><span data-stu-id="dec56-2489">Query expressions contain a number of "contextual keywords", i.e., identifiers that have special meaning in a given context.</span></span> <span data-ttu-id="dec56-2490">Konkrétně jde o `from`, `where`, `join`, `on`, `equals`, `into`, `let`, `orderby`, `ascending`, `descending`, `select`, `group` a `by`.</span><span class="sxs-lookup"><span data-stu-id="dec56-2490">Specifically these are `from`, `where`, `join`, `on`, `equals`, `into`, `let`, `orderby`, `ascending`, `descending`, `select`, `group` and `by`.</span></span> <span data-ttu-id="dec56-2491">Aby bylo možné vyhnout se nejasnostem ve výrazech dotazů způsobené smíšené použití těchto identifikátorů jako klíčová slova nebo jednoduché názvy, považují tyto identifikátory klíčová slova při, ke kterým dochází kdekoli ve výrazu dotazu.</span><span class="sxs-lookup"><span data-stu-id="dec56-2491">In order to avoid ambiguities in query expressions caused by mixed use of these identifiers as keywords or simple names, these identifiers are considered keywords when occurring anywhere within a query expression.</span></span>

<span data-ttu-id="dec56-2492">Pro tento účel, výrazu dotazu je libovolný výraz, který začíná na "`from identifier`"následované žádný token s výjimkou"`;`","`=`"nebo"`,`".</span><span class="sxs-lookup"><span data-stu-id="dec56-2492">For this purpose, a query expression is any expression that starts with "`from identifier`" followed by any token except "`;`", "`=`" or "`,`".</span></span>

<span data-ttu-id="dec56-2493">Chcete-li použít tato slova jako identifikátory v rámci výrazu dotazu, mohou mít předponu "`@`" ([identifikátory](lexical-structure.md#identifiers)).</span><span class="sxs-lookup"><span data-stu-id="dec56-2493">In order to use these words as identifiers within a query expression, they can be prefixed with "`@`" ([Identifiers](lexical-structure.md#identifiers)).</span></span>

### <a name="query-expression-translation"></a><span data-ttu-id="dec56-2494">Překlad výrazu dotazu</span><span class="sxs-lookup"><span data-stu-id="dec56-2494">Query expression translation</span></span>

<span data-ttu-id="dec56-2495">Jazyk C# neurčuje sémantika provádění – výrazy dotazů.</span><span class="sxs-lookup"><span data-stu-id="dec56-2495">The C# language does not specify the execution semantics of query expressions.</span></span> <span data-ttu-id="dec56-2496">Místo toho – výrazy dotazů jsou přeloženy do volání metod, které se řídí *vzor výrazu dotazu* ([vzor výrazu dotazu](expressions.md#the-query-expression-pattern)).</span><span class="sxs-lookup"><span data-stu-id="dec56-2496">Rather, query expressions are translated into invocations of methods that adhere to the *query expression pattern* ([The query expression pattern](expressions.md#the-query-expression-pattern)).</span></span> <span data-ttu-id="dec56-2497">Konkrétně – výrazy dotazů jsou přeloženy do volání metod s názvem `Where`, `Select`, `SelectMany`, `Join`, `GroupJoin`, `OrderBy`, `OrderByDescending`, `ThenBy`, `ThenByDescending`, `GroupBy`, a `Cast`. Tyto metody se očekává, mají konkrétní podpisy a typy výsledků, jak je popsáno v [vzor výrazu dotazu](expressions.md#the-query-expression-pattern).</span><span class="sxs-lookup"><span data-stu-id="dec56-2497">Specifically, query expressions are translated into invocations of methods named `Where`, `Select`, `SelectMany`, `Join`, `GroupJoin`, `OrderBy`, `OrderByDescending`, `ThenBy`, `ThenByDescending`, `GroupBy`, and `Cast`.These methods are expected to have particular signatures and result types, as described in [The query expression pattern](expressions.md#the-query-expression-pattern).</span></span> <span data-ttu-id="dec56-2498">Tyto metody mohou být metody instance objektu, která je dotazována nebo rozšiřující metody, které jsou externí vzhledem k objektu, a implementují vlastní provedení dotazu.</span><span class="sxs-lookup"><span data-stu-id="dec56-2498">These methods can be instance methods of the object being queried or extension methods that are external to the object, and they implement the actual execution of the query.</span></span>

<span data-ttu-id="dec56-2499">Překlad z – výrazy dotazů volání metod je syntaktické mapování, která předchází libovolný typ vazby nebo provedl řešení přetížení.</span><span class="sxs-lookup"><span data-stu-id="dec56-2499">The translation from query expressions to method invocations is a syntactic mapping that occurs before any type binding or overload resolution has been performed.</span></span> <span data-ttu-id="dec56-2500">Překlad je zaručeno, že syntakticky správný, ale není zaručena pro vytvoření sémanticky kód jazyka C#.</span><span class="sxs-lookup"><span data-stu-id="dec56-2500">The translation is guaranteed to be syntactically correct, but it is not guaranteed to produce semantically correct C# code.</span></span> <span data-ttu-id="dec56-2501">Překlad výrazů dotazů výsledné volání metod jsou zpracovány jako volání metod regulárních, a to může zase odhalte chyby, například pokud metody neexistují, pokud máte nesprávné typy argumentů, nebo pokud jsou obecné metody, a odvození typu proměnné se nezdaří.</span><span class="sxs-lookup"><span data-stu-id="dec56-2501">Following translation of query expressions, the resulting method invocations are processed as regular method invocations, and this may in turn uncover errors, for example if the methods do not exist, if arguments have wrong types, or if the methods are generic and type inference fails.</span></span>

<span data-ttu-id="dec56-2502">Výraz dotazu je zpracován opakovaně použití následujících překlady, dokud nedojde k žádné další snížení je to možné.</span><span class="sxs-lookup"><span data-stu-id="dec56-2502">A query expression is processed by repeatedly applying the following translations until no further reductions are possible.</span></span> <span data-ttu-id="dec56-2503">Překlady jsou uvedeny v pořadí použití: každá část předpokládá, že byly provedeny překlady v předchozích částech vyčerpávajícím způsobem a po vyčerpání, oddíl nebude později být znovu obrácena pozornost při zpracování stejném výrazu dotazu.</span><span class="sxs-lookup"><span data-stu-id="dec56-2503">The translations are listed in order of application: each section assumes that the translations in the preceding sections have been performed exhaustively, and once exhausted, a section will not later be revisited in the processing of the same query expression.</span></span>

<span data-ttu-id="dec56-2504">Přiřazení k proměnné rozsahu není povolené ve výrazech dotazů.</span><span class="sxs-lookup"><span data-stu-id="dec56-2504">Assignment to range variables is not allowed in query expressions.</span></span> <span data-ttu-id="dec56-2505">Implementace jazyka C# je však povoleno ne vždy vynutit toto omezení, protože to není v některých případech možné schématem syntaktické překlad okomentovat.</span><span class="sxs-lookup"><span data-stu-id="dec56-2505">However a C# implementation is permitted to not always enforce this restriction, since this may sometimes not be possible with the syntactic translation scheme presented here.</span></span>

<span data-ttu-id="dec56-2506">Některé překlady vložení proměnné rozsahu s transparentní identifikátory, označena `*`.</span><span class="sxs-lookup"><span data-stu-id="dec56-2506">Certain translations inject range variables with transparent identifiers denoted by `*`.</span></span> <span data-ttu-id="dec56-2507">Speciální vlastnosti transparentní identifikátorů, které jsou popsány dále v [transparentní identifikátory](expressions.md#transparent-identifiers).</span><span class="sxs-lookup"><span data-stu-id="dec56-2507">The special properties of transparent identifiers are discussed further in [Transparent identifiers](expressions.md#transparent-identifiers).</span></span>

#### <a name="select-and-groupby-clauses-with-continuations"></a><span data-ttu-id="dec56-2508">Klauzule SELECT a groupby s pokračováními</span><span class="sxs-lookup"><span data-stu-id="dec56-2508">Select and groupby clauses with continuations</span></span>

<span data-ttu-id="dec56-2509">Výraz dotazu s pokračováním</span><span class="sxs-lookup"><span data-stu-id="dec56-2509">A query expression with a continuation</span></span>
```csharp
from ... into x ...
```
<span data-ttu-id="dec56-2510">převést na</span><span class="sxs-lookup"><span data-stu-id="dec56-2510">is translated into</span></span>
```csharp
from x in ( from ... ) ...
```

<span data-ttu-id="dec56-2511">Překlady v následujících částech Předpokládejme dotazy nemají `into` pokračování.</span><span class="sxs-lookup"><span data-stu-id="dec56-2511">The translations in the following sections assume that queries have no `into` continuations.</span></span>

<span data-ttu-id="dec56-2512">V příkladu</span><span class="sxs-lookup"><span data-stu-id="dec56-2512">The example</span></span>
```csharp
from c in customers
group c by c.Country into g
select new { Country = g.Key, CustCount = g.Count() }
```
<span data-ttu-id="dec56-2513">převést na</span><span class="sxs-lookup"><span data-stu-id="dec56-2513">is translated into</span></span>
```csharp
from g in
    from c in customers
    group c by c.Country
select new { Country = g.Key, CustCount = g.Count() }
```
<span data-ttu-id="dec56-2514">je konečný překlad</span><span class="sxs-lookup"><span data-stu-id="dec56-2514">the final translation of which is</span></span>
```csharp
customers.
GroupBy(c => c.Country).
Select(g => new { Country = g.Key, CustCount = g.Count() })
```

#### <a name="explicit-range-variable-types"></a><span data-ttu-id="dec56-2515">Typy proměnných explicitní rozsahu</span><span class="sxs-lookup"><span data-stu-id="dec56-2515">Explicit range variable types</span></span>

<span data-ttu-id="dec56-2516">A `from` klauzuli, která explicitně určuje typ proměnné rozsahu</span><span class="sxs-lookup"><span data-stu-id="dec56-2516">A `from` clause that explicitly specifies a range variable type</span></span>
```csharp
from T x in e
```
<span data-ttu-id="dec56-2517">převést na</span><span class="sxs-lookup"><span data-stu-id="dec56-2517">is translated into</span></span>
```csharp
from x in ( e ) . Cast < T > ( )
```

<span data-ttu-id="dec56-2518">A `join` klauzuli, která explicitně určuje typ proměnné rozsahu</span><span class="sxs-lookup"><span data-stu-id="dec56-2518">A `join` clause that explicitly specifies a range variable type</span></span>
```
join T x in e on k1 equals k2
```
<span data-ttu-id="dec56-2519">převést na</span><span class="sxs-lookup"><span data-stu-id="dec56-2519">is translated into</span></span>
```
join x in ( e ) . Cast < T > ( ) on k1 equals k2
```

<span data-ttu-id="dec56-2520">Překlady v následujících částech předpokládá, že dotazy bez explicitního rozsahu se tyto typy proměnných.</span><span class="sxs-lookup"><span data-stu-id="dec56-2520">The translations in the following sections assume that queries have no explicit range variable types.</span></span>

<span data-ttu-id="dec56-2521">V příkladu</span><span class="sxs-lookup"><span data-stu-id="dec56-2521">The example</span></span>
```csharp
from Customer c in customers
where c.City == "London"
select c
```
<span data-ttu-id="dec56-2522">převést na</span><span class="sxs-lookup"><span data-stu-id="dec56-2522">is translated into</span></span>
```csharp
from c in customers.Cast<Customer>()
where c.City == "London"
select c
```
<span data-ttu-id="dec56-2523">je konečný překlad</span><span class="sxs-lookup"><span data-stu-id="dec56-2523">the final translation of which is</span></span>
```csharp
customers.
Cast<Customer>().
Where(c => c.City == "London")
```

<span data-ttu-id="dec56-2524">Jsou užitečné pro dotazování na kolekce, které implementují neobecné typy proměnných explicitní rozsah `IEnumerable` rozhraní, ale ne Obecné `IEnumerable<T>` rozhraní.</span><span class="sxs-lookup"><span data-stu-id="dec56-2524">Explicit range variable types are useful for querying collections that implement the non-generic `IEnumerable` interface, but not the generic `IEnumerable<T>` interface.</span></span> <span data-ttu-id="dec56-2525">V předchozím příkladu to může být v případě, pokud `customers` byly typu `ArrayList`.</span><span class="sxs-lookup"><span data-stu-id="dec56-2525">In the example above, this would be the case if `customers` were of type `ArrayList`.</span></span>

#### <a name="degenerate-query-expressions"></a><span data-ttu-id="dec56-2526">Výrazy degenerovanou dotazů</span><span class="sxs-lookup"><span data-stu-id="dec56-2526">Degenerate query expressions</span></span>

<span data-ttu-id="dec56-2527">Výraz dotazu ve formuláři</span><span class="sxs-lookup"><span data-stu-id="dec56-2527">A query expression of the form</span></span>
```csharp
from x in e select x
```
<span data-ttu-id="dec56-2528">převést na</span><span class="sxs-lookup"><span data-stu-id="dec56-2528">is translated into</span></span>
```csharp
( e ) . Select ( x => x )
```

<span data-ttu-id="dec56-2529">V příkladu</span><span class="sxs-lookup"><span data-stu-id="dec56-2529">The example</span></span>
```csharp
from c in customers
select c
```
<span data-ttu-id="dec56-2530">převést na</span><span class="sxs-lookup"><span data-stu-id="dec56-2530">is translated into</span></span>
```csharp
customers.Select(c => c)
```

<span data-ttu-id="dec56-2531">Výraz degenerovanou dotazu je ten, který triviálně vybere elementy zdroje.</span><span class="sxs-lookup"><span data-stu-id="dec56-2531">A degenerate query expression is one that trivially selects the elements of the source.</span></span> <span data-ttu-id="dec56-2532">Pozdější fáze překladu odebere degenerovanou dotazy zavedené dalších kroků a nahradí je jejich zdroji.</span><span class="sxs-lookup"><span data-stu-id="dec56-2532">A later phase of the translation removes degenerate queries introduced by other translation steps by replacing them with their source.</span></span> <span data-ttu-id="dec56-2533">Je důležité, ale zajistit, aby výsledek dotazu výrazu není nikdy zdrojového objektu samotného, podle typu a identitu zdroje, které by odhalit klientovi dotazu.</span><span class="sxs-lookup"><span data-stu-id="dec56-2533">It is important however to ensure that the result of a query expression is never the source object itself, as that would reveal the type and identity of the source to the client of the query.</span></span> <span data-ttu-id="dec56-2534">Proto tento krok chrání degenerovanou dotazy napsané přímo ve zdrojovém kódu explicitně voláním `Select` ve zdroji.</span><span class="sxs-lookup"><span data-stu-id="dec56-2534">Therefore this step protects degenerate queries written directly in source code by explicitly calling `Select` on the source.</span></span> <span data-ttu-id="dec56-2535">Je pak až implementátorům `Select` a jiných operátorů dotazu pro zajištění, že tyto metody nikdy vrátí samotného objektu zdroje.</span><span class="sxs-lookup"><span data-stu-id="dec56-2535">It is then up to the implementers of `Select` and other query operators to ensure that these methods never return the source object itself.</span></span>

#### <a name="from-let-where-join-and-orderby-clauses"></a><span data-ttu-id="dec56-2536">FROM, where, umožní klauzule join a řadit podle</span><span class="sxs-lookup"><span data-stu-id="dec56-2536">From, let, where, join and orderby clauses</span></span>

<span data-ttu-id="dec56-2537">Výrazu dotazu s druhou `from` klauzuli za nímž následuje `select` – klauzule</span><span class="sxs-lookup"><span data-stu-id="dec56-2537">A query expression with a second `from` clause followed by a `select` clause</span></span>
```csharp
from x1 in e1
from x2 in e2
select v
```
<span data-ttu-id="dec56-2538">převést na</span><span class="sxs-lookup"><span data-stu-id="dec56-2538">is translated into</span></span>
```csharp
( e1 ) . SelectMany( x1 => e2 , ( x1 , x2 ) => v )
```

<span data-ttu-id="dec56-2539">Výraz dotazu s druhou `from` klauzuli za nímž následuje něco jiného než `select` klauzule:</span><span class="sxs-lookup"><span data-stu-id="dec56-2539">A query expression with a second `from` clause followed by something other than a `select` clause:</span></span>

```csharp
from x1 in e1
from x2 in e2
...
```
<span data-ttu-id="dec56-2540">převést na</span><span class="sxs-lookup"><span data-stu-id="dec56-2540">is translated into</span></span>
```csharp
from * in ( e1 ) . SelectMany( x1 => e2 , ( x1 , x2 ) => new { x1 , x2 } )
...
```

<span data-ttu-id="dec56-2541">Výraz dotazu s `let` – klauzule</span><span class="sxs-lookup"><span data-stu-id="dec56-2541">A query expression with a `let` clause</span></span>
```csharp
from x in e
let y = f
...
```
<span data-ttu-id="dec56-2542">převést na</span><span class="sxs-lookup"><span data-stu-id="dec56-2542">is translated into</span></span>
```csharp
from * in ( e ) . Select ( x => new { x , y = f } )
...
```

<span data-ttu-id="dec56-2543">Výraz dotazu s `where` – klauzule</span><span class="sxs-lookup"><span data-stu-id="dec56-2543">A query expression with a `where` clause</span></span>
```csharp
from x in e
where f
...
```
<span data-ttu-id="dec56-2544">převést na</span><span class="sxs-lookup"><span data-stu-id="dec56-2544">is translated into</span></span>
```csharp
from x in ( e ) . Where ( x => f )
...
```

<span data-ttu-id="dec56-2545">Výrazu dotazu s `join` klauzule bez `into` následovaný `select` – klauzule</span><span class="sxs-lookup"><span data-stu-id="dec56-2545">A query expression with a `join` clause without an `into` followed by a `select` clause</span></span>
```csharp
from x1 in e1
join x2 in e2 on k1 equals k2
select v
```
<span data-ttu-id="dec56-2546">převést na</span><span class="sxs-lookup"><span data-stu-id="dec56-2546">is translated into</span></span>
```csharp
( e1 ) . Join( e2 , x1 => k1 , x2 => k2 , ( x1 , x2 ) => v )
```

<span data-ttu-id="dec56-2547">Výrazu dotazu s `join` klauzule bez `into` za nímž následuje něco jiného než `select` – klauzule</span><span class="sxs-lookup"><span data-stu-id="dec56-2547">A query expression with a `join` clause without an `into` followed by something other than a `select` clause</span></span>
```csharp
from x1 in e1
join x2 in e2 on k1 equals k2
...
```
<span data-ttu-id="dec56-2548">převést na</span><span class="sxs-lookup"><span data-stu-id="dec56-2548">is translated into</span></span>
```csharp
from * in ( e1 ) . Join( e2 , x1 => k1 , x2 => k2 , ( x1 , x2 ) => new { x1 , x2 })
...
```

<span data-ttu-id="dec56-2549">Výrazu dotazu s `join` klauzule `into` následovaný `select` – klauzule</span><span class="sxs-lookup"><span data-stu-id="dec56-2549">A query expression with a `join` clause with an `into` followed by a `select` clause</span></span>
```csharp
from x1 in e1
join x2 in e2 on k1 equals k2 into g
select v
```
<span data-ttu-id="dec56-2550">převést na</span><span class="sxs-lookup"><span data-stu-id="dec56-2550">is translated into</span></span>
```csharp
( e1 ) . GroupJoin( e2 , x1 => k1 , x2 => k2 , ( x1 , g ) => v )
```

<span data-ttu-id="dec56-2551">Výrazu dotazu s `join` klauzule `into` za nímž následuje něco jiného než `select` – klauzule</span><span class="sxs-lookup"><span data-stu-id="dec56-2551">A query expression with a `join` clause with an `into` followed by something other than a `select` clause</span></span>
```csharp
from x1 in e1
join x2 in e2 on k1 equals k2 into g
...
```
<span data-ttu-id="dec56-2552">převést na</span><span class="sxs-lookup"><span data-stu-id="dec56-2552">is translated into</span></span>
```csharp
from * in ( e1 ) . GroupJoin( e2 , x1 => k1 , x2 => k2 , ( x1 , g ) => new { x1 , g })
...
```

<span data-ttu-id="dec56-2553">Výraz dotazu pomocí `orderby` – klauzule</span><span class="sxs-lookup"><span data-stu-id="dec56-2553">A query expression with an `orderby` clause</span></span>
```csharp
from x in e
orderby k1 , k2 , ..., kn
...
```
<span data-ttu-id="dec56-2554">převést na</span><span class="sxs-lookup"><span data-stu-id="dec56-2554">is translated into</span></span>
```csharp
from x in ( e ) . 
OrderBy ( x => k1 ) . 
ThenBy ( x => k2 ) .
... .
ThenBy ( x => kn )
...
```

<span data-ttu-id="dec56-2555">Pokud má za výsledek řazení klauzule určuje `descending` směrové, vyvolání `OrderByDescending` nebo `ThenByDescending` místo toho je vytvořen.</span><span class="sxs-lookup"><span data-stu-id="dec56-2555">If an ordering clause specifies a `descending` direction indicator, an invocation of `OrderByDescending` or `ThenByDescending` is produced instead.</span></span>

<span data-ttu-id="dec56-2556">Tyto překlady se předpokládá, že neexistují žádné `let`, `where`, `join` nebo `orderby` klauzule a více než jednu počáteční `from` klauzuli v každém výrazu dotazu.</span><span class="sxs-lookup"><span data-stu-id="dec56-2556">The following translations assume that there are no `let`, `where`, `join` or `orderby` clauses, and no more than the one initial `from` clause in each query expression.</span></span>

<span data-ttu-id="dec56-2557">V příkladu</span><span class="sxs-lookup"><span data-stu-id="dec56-2557">The example</span></span>
```csharp
from c in customers
from o in c.Orders
select new { c.Name, o.OrderID, o.Total }
```
<span data-ttu-id="dec56-2558">převést na</span><span class="sxs-lookup"><span data-stu-id="dec56-2558">is translated into</span></span>
```csharp
customers.
SelectMany(c => c.Orders,
     (c,o) => new { c.Name, o.OrderID, o.Total }
)
```

<span data-ttu-id="dec56-2559">V příkladu</span><span class="sxs-lookup"><span data-stu-id="dec56-2559">The example</span></span>
```csharp
from c in customers
from o in c.Orders
orderby o.Total descending
select new { c.Name, o.OrderID, o.Total }
```
<span data-ttu-id="dec56-2560">převést na</span><span class="sxs-lookup"><span data-stu-id="dec56-2560">is translated into</span></span>
```csharp
from * in customers.
    SelectMany(c => c.Orders, (c,o) => new { c, o })
orderby o.Total descending
select new { c.Name, o.OrderID, o.Total }
```
<span data-ttu-id="dec56-2561">je konečný překlad</span><span class="sxs-lookup"><span data-stu-id="dec56-2561">the final translation of which is</span></span>
```csharp
customers.
SelectMany(c => c.Orders, (c,o) => new { c, o }).
OrderByDescending(x => x.o.Total).
Select(x => new { x.c.Name, x.o.OrderID, x.o.Total })
```
<span data-ttu-id="dec56-2562">kde `x` identifikátor generovaný kompilátorem, který je jinak neviditelné a nejsou přístupné.</span><span class="sxs-lookup"><span data-stu-id="dec56-2562">where `x` is a compiler generated identifier that is otherwise invisible and inaccessible.</span></span>

<span data-ttu-id="dec56-2563">V příkladu</span><span class="sxs-lookup"><span data-stu-id="dec56-2563">The example</span></span>
```csharp
from o in orders
let t = o.Details.Sum(d => d.UnitPrice * d.Quantity)
where t >= 1000
select new { o.OrderID, Total = t }
```
<span data-ttu-id="dec56-2564">převést na</span><span class="sxs-lookup"><span data-stu-id="dec56-2564">is translated into</span></span>
```csharp
from * in orders.
    Select(o => new { o, t = o.Details.Sum(d => d.UnitPrice * d.Quantity) })
where t >= 1000 
select new { o.OrderID, Total = t }
```
<span data-ttu-id="dec56-2565">je konečný překlad</span><span class="sxs-lookup"><span data-stu-id="dec56-2565">the final translation of which is</span></span>
```csharp
orders.
Select(o => new { o, t = o.Details.Sum(d => d.UnitPrice * d.Quantity) }).
Where(x => x.t >= 1000).
Select(x => new { x.o.OrderID, Total = x.t })
```
<span data-ttu-id="dec56-2566">kde `x` identifikátor generovaný kompilátorem, který je jinak neviditelné a nejsou přístupné.</span><span class="sxs-lookup"><span data-stu-id="dec56-2566">where `x` is a compiler generated identifier that is otherwise invisible and inaccessible.</span></span>

<span data-ttu-id="dec56-2567">V příkladu</span><span class="sxs-lookup"><span data-stu-id="dec56-2567">The example</span></span>
```csharp
from c in customers
join o in orders on c.CustomerID equals o.CustomerID
select new { c.Name, o.OrderDate, o.Total }
```
<span data-ttu-id="dec56-2568">převést na</span><span class="sxs-lookup"><span data-stu-id="dec56-2568">is translated into</span></span>
```csharp
customers.Join(orders, c => c.CustomerID, o => o.CustomerID,
    (c, o) => new { c.Name, o.OrderDate, o.Total })
```

<span data-ttu-id="dec56-2569">V příkladu</span><span class="sxs-lookup"><span data-stu-id="dec56-2569">The example</span></span>
```csharp
from c in customers
join o in orders on c.CustomerID equals o.CustomerID into co
let n = co.Count()
where n >= 10
select new { c.Name, OrderCount = n }
```
<span data-ttu-id="dec56-2570">převést na</span><span class="sxs-lookup"><span data-stu-id="dec56-2570">is translated into</span></span>
```csharp
from * in customers.
    GroupJoin(orders, c => c.CustomerID, o => o.CustomerID,
        (c, co) => new { c, co })
let n = co.Count()
where n >= 10 
select new { c.Name, OrderCount = n }
```
<span data-ttu-id="dec56-2571">je konečný překlad</span><span class="sxs-lookup"><span data-stu-id="dec56-2571">the final translation of which is</span></span>
```csharp
customers.
GroupJoin(orders, c => c.CustomerID, o => o.CustomerID,
    (c, co) => new { c, co }).
Select(x => new { x, n = x.co.Count() }).
Where(y => y.n >= 10).
Select(y => new { y.x.c.Name, OrderCount = y.n)
```
<span data-ttu-id="dec56-2572">kde `x` a `y` jsou identifikátory generovaný kompilátorem, které jsou jinak neviditelné a nejsou přístupné.</span><span class="sxs-lookup"><span data-stu-id="dec56-2572">where `x` and `y` are compiler generated identifiers that are otherwise invisible and inaccessible.</span></span>

<span data-ttu-id="dec56-2573">V příkladu</span><span class="sxs-lookup"><span data-stu-id="dec56-2573">The example</span></span>
```csharp
from o in orders
orderby o.Customer.Name, o.Total descending
select o
```
<span data-ttu-id="dec56-2574">je konečný překlad</span><span class="sxs-lookup"><span data-stu-id="dec56-2574">has the final translation</span></span>
```csharp
orders.
OrderBy(o => o.Customer.Name).
ThenByDescending(o => o.Total)
```

#### <a name="select-clauses"></a><span data-ttu-id="dec56-2575">Klauzule FROM</span><span class="sxs-lookup"><span data-stu-id="dec56-2575">Select clauses</span></span>

<span data-ttu-id="dec56-2576">Výraz dotazu ve formuláři</span><span class="sxs-lookup"><span data-stu-id="dec56-2576">A query expression of the form</span></span>
```csharp
from x in e select v
```
<span data-ttu-id="dec56-2577">převést na</span><span class="sxs-lookup"><span data-stu-id="dec56-2577">is translated into</span></span>
```csharp
( e ) . Select ( x => v )
```
<span data-ttu-id="dec56-2578">s výjimkou případu, kdy v identifikátor x, překlad je jednoduše</span><span class="sxs-lookup"><span data-stu-id="dec56-2578">except when v is the identifier x, the translation is simply</span></span>
```csharp
( e )
```

<span data-ttu-id="dec56-2579">Příklad</span><span class="sxs-lookup"><span data-stu-id="dec56-2579">For example</span></span>
```csharp
from c in customers.Where(c => c.City == "London")
select c
```
<span data-ttu-id="dec56-2580">je jednoduše převést na</span><span class="sxs-lookup"><span data-stu-id="dec56-2580">is simply translated into</span></span>
```csharp
customers.Where(c => c.City == "London")
```

#### <a name="groupby-clauses"></a><span data-ttu-id="dec56-2581">Klauzule GroupBy</span><span class="sxs-lookup"><span data-stu-id="dec56-2581">Groupby clauses</span></span>

<span data-ttu-id="dec56-2582">Výraz dotazu ve formuláři</span><span class="sxs-lookup"><span data-stu-id="dec56-2582">A query expression of the form</span></span>
```csharp
from x in e group v by k
```
<span data-ttu-id="dec56-2583">převést na</span><span class="sxs-lookup"><span data-stu-id="dec56-2583">is translated into</span></span>
```csharp
( e ) . GroupBy ( x => k , x => v )
```
<span data-ttu-id="dec56-2584">s výjimkou případu, kdy v identifikátoru je x, překlad</span><span class="sxs-lookup"><span data-stu-id="dec56-2584">except when v is the identifier x, the translation is</span></span>
```csharp
( e ) . GroupBy ( x => k )
```

<span data-ttu-id="dec56-2585">V příkladu</span><span class="sxs-lookup"><span data-stu-id="dec56-2585">The example</span></span>
```csharp
from c in customers
group c.Name by c.Country
```
<span data-ttu-id="dec56-2586">převést na</span><span class="sxs-lookup"><span data-stu-id="dec56-2586">is translated into</span></span>
```csharp
customers.
GroupBy(c => c.Country, c => c.Name)
```

#### <a name="transparent-identifiers"></a><span data-ttu-id="dec56-2587">Transparentní identifikátory</span><span class="sxs-lookup"><span data-stu-id="dec56-2587">Transparent identifiers</span></span>

<span data-ttu-id="dec56-2588">Některé překlady vložení proměnné rozsahu s ***transparentní identifikátory*** udávají `*`.</span><span class="sxs-lookup"><span data-stu-id="dec56-2588">Certain translations inject range variables with ***transparent identifiers*** denoted by `*`.</span></span> <span data-ttu-id="dec56-2589">Transparentní identifikátory nejsou správné jazykové funkce; existují pouze jako přechodný krok v procesu překladu výrazu dotazu.</span><span class="sxs-lookup"><span data-stu-id="dec56-2589">Transparent identifiers are not a proper language feature; they exist only as an intermediate step in the query expression translation process.</span></span>

<span data-ttu-id="dec56-2590">Při překladu dotazu vkládá transparentním identifikátorem, další kroky šířit do anonymní funkce a anonymní objekt inicializátory transparentním identifikátorem.</span><span class="sxs-lookup"><span data-stu-id="dec56-2590">When a query translation injects a transparent identifier, further translation steps propagate the transparent identifier into anonymous functions and anonymous object initializers.</span></span> <span data-ttu-id="dec56-2591">V těchto kontextech transparentní identifikátory mají následující chování:</span><span class="sxs-lookup"><span data-stu-id="dec56-2591">In those contexts, transparent identifiers have the following behavior:</span></span>

*  <span data-ttu-id="dec56-2592">Dojde-li k transparentním identifikátorem jako anonymní funkce, jsou členy přidružených anonymního typu automaticky v oboru v těle anonymní funkce.</span><span class="sxs-lookup"><span data-stu-id="dec56-2592">When a transparent identifier occurs as a parameter in an anonymous function, the members of the associated anonymous type are automatically in scope in the body of the anonymous function.</span></span>
*  <span data-ttu-id="dec56-2593">Pokud člen s transparentním identifikátorem je v oboru, členy, se kterou jsou v oboru také.</span><span class="sxs-lookup"><span data-stu-id="dec56-2593">When a member with a transparent identifier is in scope, the members of that member are in scope as well.</span></span>
*  <span data-ttu-id="dec56-2594">Pokud dojde k transparentním identifikátorem jako deklarátor členu v inicializátoru anonymní objekt, představuje člena s transparentním identifikátorem.</span><span class="sxs-lookup"><span data-stu-id="dec56-2594">When a transparent identifier occurs as a member declarator in an anonymous object initializer, it introduces a member with a transparent identifier.</span></span>
*  <span data-ttu-id="dec56-2595">Ve výše uvedeného postupu překladu jsou transparentní identifikátory vždy zavedli spolu s anonymní typy se záměrem zachycení více proměnných rozsahu jako členy jednoho objektu.</span><span class="sxs-lookup"><span data-stu-id="dec56-2595">In the translation steps described above, transparent identifiers are always introduced together with anonymous types, with the intent of capturing multiple range variables as members of a single object.</span></span> <span data-ttu-id="dec56-2596">Implementace jazyka C# je povoleno používat jiným způsobem než anonymní typy k seskupení více proměnných rozsahu.</span><span class="sxs-lookup"><span data-stu-id="dec56-2596">An implementation of C# is permitted to use a different mechanism than anonymous types to group together multiple range variables.</span></span> <span data-ttu-id="dec56-2597">Následující příklady překlad předpokládají, že anonymní typy se používají a zobrazit průhledné identifikátory lze přeložit okamžitě.</span><span class="sxs-lookup"><span data-stu-id="dec56-2597">The following translation examples assume that anonymous types are used, and show how transparent identifiers can be translated away.</span></span>

<span data-ttu-id="dec56-2598">V příkladu</span><span class="sxs-lookup"><span data-stu-id="dec56-2598">The example</span></span>
```csharp
from c in customers
from o in c.Orders
orderby o.Total descending
select new { c.Name, o.Total }
```
<span data-ttu-id="dec56-2599">převést na</span><span class="sxs-lookup"><span data-stu-id="dec56-2599">is translated into</span></span>
```csharp
from * in customers.
    SelectMany(c => c.Orders, (c,o) => new { c, o })
orderby o.Total descending
select new { c.Name, o.Total }
```

<span data-ttu-id="dec56-2600">což je další přeloženy do</span><span class="sxs-lookup"><span data-stu-id="dec56-2600">which is further translated into</span></span>
```csharp
customers.
SelectMany(c => c.Orders, (c,o) => new { c, o }).
OrderByDescending(* => o.Total).
Select(* => new { c.Name, o.Total })
```
<span data-ttu-id="dec56-2601">což, když transparentní identifikátory jsou vymazány, je ekvivalentní k</span><span class="sxs-lookup"><span data-stu-id="dec56-2601">which, when transparent identifiers are erased, is equivalent to</span></span>
```csharp
customers.
SelectMany(c => c.Orders, (c,o) => new { c, o }).
OrderByDescending(x => x.o.Total).
Select(x => new { x.c.Name, x.o.Total })
```
<span data-ttu-id="dec56-2602">kde `x` identifikátor generovaný kompilátorem, který je jinak neviditelné a nejsou přístupné.</span><span class="sxs-lookup"><span data-stu-id="dec56-2602">where `x` is a compiler generated identifier that is otherwise invisible and inaccessible.</span></span>

<span data-ttu-id="dec56-2603">V příkladu</span><span class="sxs-lookup"><span data-stu-id="dec56-2603">The example</span></span>
```csharp
from c in customers
join o in orders on c.CustomerID equals o.CustomerID
join d in details on o.OrderID equals d.OrderID
join p in products on d.ProductID equals p.ProductID
select new { c.Name, o.OrderDate, p.ProductName }
```
<span data-ttu-id="dec56-2604">převést na</span><span class="sxs-lookup"><span data-stu-id="dec56-2604">is translated into</span></span>
```csharp
from * in customers.
    Join(orders, c => c.CustomerID, o => o.CustomerID, 
        (c, o) => new { c, o })
join d in details on o.OrderID equals d.OrderID
join p in products on d.ProductID equals p.ProductID
select new { c.Name, o.OrderDate, p.ProductName }
```
<span data-ttu-id="dec56-2605">což je dále omezit na</span><span class="sxs-lookup"><span data-stu-id="dec56-2605">which is further reduced to</span></span>
```csharp
customers.
Join(orders, c => c.CustomerID, o => o.CustomerID, (c, o) => new { c, o }).
Join(details, * => o.OrderID, d => d.OrderID, (*, d) => new { *, d }).
Join(products, * => d.ProductID, p => p.ProductID, (*, p) => new { *, p }).
Select(* => new { c.Name, o.OrderDate, p.ProductName })
```
<span data-ttu-id="dec56-2606">je konečný překlad</span><span class="sxs-lookup"><span data-stu-id="dec56-2606">the final translation of which is</span></span>
```csharp
customers.
Join(orders, c => c.CustomerID, o => o.CustomerID,
    (c, o) => new { c, o }).
Join(details, x => x.o.OrderID, d => d.OrderID,
    (x, d) => new { x, d }).
Join(products, y => y.d.ProductID, p => p.ProductID,
    (y, p) => new { y, p }).
Select(z => new { z.y.x.c.Name, z.y.x.o.OrderDate, z.p.ProductName })
```
<span data-ttu-id="dec56-2607">kde `x`, `y`, a `z` jsou identifikátory generovaný kompilátorem, které jsou jinak neviditelné a nejsou přístupné.</span><span class="sxs-lookup"><span data-stu-id="dec56-2607">where `x`, `y`, and `z` are compiler generated identifiers that are otherwise invisible and inaccessible.</span></span>

### <a name="the-query-expression-pattern"></a><span data-ttu-id="dec56-2608">Vzor výrazu dotazu</span><span class="sxs-lookup"><span data-stu-id="dec56-2608">The query expression pattern</span></span>

<span data-ttu-id="dec56-2609">***Vzor výrazu dotazu*** vytváří vzor metody, které implementují typy pro podporu výrazy dotazu.</span><span class="sxs-lookup"><span data-stu-id="dec56-2609">The ***Query expression pattern*** establishes a pattern of methods that types can implement to support query expressions.</span></span> <span data-ttu-id="dec56-2610">Protože výrazy dotazu jsou převedeny na volání metod prostřednictvím syntaktické mapování, typy mají značnou flexibilitu ve způsobu implementace vzorku dotazu výrazu.</span><span class="sxs-lookup"><span data-stu-id="dec56-2610">Because query expressions are translated to method invocations by means of a syntactic mapping, types have considerable flexibility in how they implement the query expression pattern.</span></span> <span data-ttu-id="dec56-2611">Například metody vzor je možné implementovat jako metody instance nebo jako metody rozšíření protože dva mají stejnou syntaxi volání a metody můžete vyžádat delegáty nebo stromů výrazů, protože lze převést na obojí jsou anonymní funkce.</span><span class="sxs-lookup"><span data-stu-id="dec56-2611">For example, the methods of the pattern can be implemented as instance methods or as extension methods because the two have the same invocation syntax, and the methods can request delegates or expression trees because anonymous functions are convertible to both.</span></span>

<span data-ttu-id="dec56-2612">Doporučené tvar obecného typu `C<T>` podporuje vzor výrazu dotazu je uveden níže.</span><span class="sxs-lookup"><span data-stu-id="dec56-2612">The recommended shape of a generic type `C<T>` that supports the query expression pattern is shown below.</span></span> <span data-ttu-id="dec56-2613">Obecný typ se používá k ilustraci správné relace mezi typy parametrů a výsledek, ale je možné implementovat vzor pro obecné typy a také.</span><span class="sxs-lookup"><span data-stu-id="dec56-2613">A generic type is used in order to illustrate the proper relationships between parameter and result types, but it is possible to implement the pattern for non-generic types as well.</span></span>

```csharp
delegate R Func<T1,R>(T1 arg1);

delegate R Func<T1,T2,R>(T1 arg1, T2 arg2);

class C
{
    public C<T> Cast<T>();
}

class C<T> : C
{
    public C<T> Where(Func<T,bool> predicate);

    public C<U> Select<U>(Func<T,U> selector);

    public C<V> SelectMany<U,V>(Func<T,C<U>> selector,
        Func<T,U,V> resultSelector);

    public C<V> Join<U,K,V>(C<U> inner, Func<T,K> outerKeySelector,
        Func<U,K> innerKeySelector, Func<T,U,V> resultSelector);

    public C<V> GroupJoin<U,K,V>(C<U> inner, Func<T,K> outerKeySelector,
        Func<U,K> innerKeySelector, Func<T,C<U>,V> resultSelector);

    public O<T> OrderBy<K>(Func<T,K> keySelector);

    public O<T> OrderByDescending<K>(Func<T,K> keySelector);

    public C<G<K,T>> GroupBy<K>(Func<T,K> keySelector);

    public C<G<K,E>> GroupBy<K,E>(Func<T,K> keySelector,
        Func<T,E> elementSelector);
}

class O<T> : C<T>
{
    public O<T> ThenBy<K>(Func<T,K> keySelector);

    public O<T> ThenByDescending<K>(Func<T,K> keySelector);
}

class G<K,T> : C<T>
{
    public K Key { get; }
}
```

<span data-ttu-id="dec56-2614">Výše uvedené metody pomocí typů obecného delegátu `Func<T1,R>` a `Func<T1,T2,R>`, ale jejich mohl stejně dobře použít jiné typy delegát nebo výraz stromu stejné relace v typy parametrů a výsledek.</span><span class="sxs-lookup"><span data-stu-id="dec56-2614">The methods above use the generic delegate types `Func<T1,R>` and `Func<T1,T2,R>`, but they could equally well have used other delegate or expression tree types with the same relationships in parameter and result types.</span></span>

<span data-ttu-id="dec56-2615">Všimněte si, že doporučená vztah mezi `C<T>` a `O<T>` který zajišťuje, že `ThenBy` a `ThenByDescending` jsou k dispozici pouze na výsledek metody `OrderBy` nebo `OrderByDescending`.</span><span class="sxs-lookup"><span data-stu-id="dec56-2615">Notice the recommended relationship between `C<T>` and `O<T>` which ensures that the `ThenBy` and `ThenByDescending` methods are available only on the result of an `OrderBy` or `OrderByDescending`.</span></span> <span data-ttu-id="dec56-2616">Všimněte si také doporučené tvar výsledku `GroupBy` --sekvence sekvencí, ve kterém má každé vnitřní řady dalších `Key` vlastnost.</span><span class="sxs-lookup"><span data-stu-id="dec56-2616">Also notice the recommended shape of the result of `GroupBy` -- a sequence of sequences, where each inner sequence has an additional `Key` property.</span></span>

<span data-ttu-id="dec56-2617">`System.Linq` Obor názvů obsahuje implementace vzorku dotazu operátor pro libovolný typ, který implementuje `System.Collections.Generic.IEnumerable<T>` rozhraní.</span><span class="sxs-lookup"><span data-stu-id="dec56-2617">The `System.Linq` namespace provides an implementation of the query operator pattern for any type that implements the `System.Collections.Generic.IEnumerable<T>` interface.</span></span>

## <a name="assignment-operators"></a><span data-ttu-id="dec56-2618">Operátory přiřazení</span><span class="sxs-lookup"><span data-stu-id="dec56-2618">Assignment operators</span></span>

<span data-ttu-id="dec56-2619">Operátory přiřazení přiřadí novou hodnotu do proměnné, vlastnosti, události nebo elementu indexeru.</span><span class="sxs-lookup"><span data-stu-id="dec56-2619">The assignment operators assign a new value to a variable, a property, an event, or an indexer element.</span></span>

```antlr
assignment
    : unary_expression assignment_operator expression
    ;

assignment_operator
    : '='
    | '+='
    | '-='
    | '*='
    | '/='
    | '%='
    | '&='
    | '|='
    | '^='
    | '<<='
    | right_shift_assignment
    ;
```

<span data-ttu-id="dec56-2620">Levý operand přiřazení musí být výraz jsou klasifikovány jako proměnné, přístup k vlastnosti, přístup indexeru nebo události přístupu.</span><span class="sxs-lookup"><span data-stu-id="dec56-2620">The left operand of an assignment must be an expression classified as a variable, a property access, an indexer access, or an event access.</span></span>

<span data-ttu-id="dec56-2621">`=` Operátor je volána ***operátor jednoduchého přiřazení***.</span><span class="sxs-lookup"><span data-stu-id="dec56-2621">The `=` operator is called the ***simple assignment operator***.</span></span> <span data-ttu-id="dec56-2622">Přiřadí hodnotu pravý operand elementu proměnná, vlastnost nebo indexer Dal levý operand.</span><span class="sxs-lookup"><span data-stu-id="dec56-2622">It assigns the value of the right operand to the variable, property, or indexer element given by the left operand.</span></span> <span data-ttu-id="dec56-2623">Levý operand operátor jednoduchého přiřazení nemusí být přístup k události (s výjimkou případů popsaných v [události podobné poli](classes.md#field-like-events)).</span><span class="sxs-lookup"><span data-stu-id="dec56-2623">The left operand of the simple assignment operator may not be an event access (except as described in [Field-like events](classes.md#field-like-events)).</span></span> <span data-ttu-id="dec56-2624">Operátor jednoduchého přiřazení je popsána v [jednoduché přiřazení](expressions.md#simple-assignment).</span><span class="sxs-lookup"><span data-stu-id="dec56-2624">The simple assignment operator is described in [Simple assignment](expressions.md#simple-assignment).</span></span>

<span data-ttu-id="dec56-2625">Operátory přiřazení jinak než `=` označují jako operátor ***operátory přiřazení složení***.</span><span class="sxs-lookup"><span data-stu-id="dec56-2625">The assignment operators other than the `=` operator are called the ***compound assignment operators***.</span></span> <span data-ttu-id="dec56-2626">Tyto operátory provádějí uvedené operace na dvou operandech a výslednou hodnotu pak přiřaďte elementu proměnná, vlastnost nebo indexer Dal levý operand.</span><span class="sxs-lookup"><span data-stu-id="dec56-2626">These operators perform the indicated operation on the two operands, and then assign the resulting value to the variable, property, or indexer element given by the left operand.</span></span> <span data-ttu-id="dec56-2627">Složené operátory přiřazení jsou popsány v [složené přiřazení](expressions.md#compound-assignment).</span><span class="sxs-lookup"><span data-stu-id="dec56-2627">The compound assignment operators are described in [Compound assignment](expressions.md#compound-assignment).</span></span>

<span data-ttu-id="dec56-2628">`+=` a `-=` se nazývají operátory s výrazem přístup události jako levý operand *operátory přiřazení pro událost*.</span><span class="sxs-lookup"><span data-stu-id="dec56-2628">The `+=` and `-=` operators with an event access expression as the left operand are called the *event assignment operators*.</span></span> <span data-ttu-id="dec56-2629">Žádný operátor přiřazení je platné s přístupem události jako levý operand.</span><span class="sxs-lookup"><span data-stu-id="dec56-2629">No other assignment operator is valid with an event access as the left operand.</span></span> <span data-ttu-id="dec56-2630">Operátory přiřazení události jsou popsány v [události přiřazení](expressions.md#event-assignment).</span><span class="sxs-lookup"><span data-stu-id="dec56-2630">The event assignment operators are described in [Event assignment](expressions.md#event-assignment).</span></span>

<span data-ttu-id="dec56-2631">Operátory přiřazení jsou asociativní zprava, což znamená, že operace se seskupují zleva doprava.</span><span class="sxs-lookup"><span data-stu-id="dec56-2631">The assignment operators are right-associative, meaning that operations are grouped from right to left.</span></span> <span data-ttu-id="dec56-2632">Například výraz ve tvaru `a = b = c` se vyhodnotí jako `a = (b = c)`.</span><span class="sxs-lookup"><span data-stu-id="dec56-2632">For example, an expression of the form `a = b = c` is evaluated as `a = (b = c)`.</span></span>

### <a name="simple-assignment"></a><span data-ttu-id="dec56-2633">Jednoduché přiřazení</span><span class="sxs-lookup"><span data-stu-id="dec56-2633">Simple assignment</span></span>

<span data-ttu-id="dec56-2634">`=` Operátor se nazývá operátor jednoduchého přiřazení.</span><span class="sxs-lookup"><span data-stu-id="dec56-2634">The `=` operator is called the simple assignment operator.</span></span>

<span data-ttu-id="dec56-2635">Pokud levý operand jednoduché přiřazení má formát `E.P` nebo `E[Ei]` kde `E` má typ kompilace `dynamic`, pak přiřazení je vázán dynamicky ([dynamické vazby](expressions.md#dynamic-binding)).</span><span class="sxs-lookup"><span data-stu-id="dec56-2635">If the left operand of a simple assignment is of the form `E.P` or `E[Ei]` where `E` has the compile-time type `dynamic`, then the assignment is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)).</span></span> <span data-ttu-id="dec56-2636">V tomto případě je typ kompilaci výrazu přiřazení `dynamic`, a rozlišení je popsáno níže se provede v době běhu na základě typu za běhu `E`.</span><span class="sxs-lookup"><span data-stu-id="dec56-2636">In this case the compile-time type of the assignment expression is `dynamic`, and the resolution described below will take place at run-time based on the run-time type of `E`.</span></span>

<span data-ttu-id="dec56-2637">V jednoduché přiřazení pravý operand musí být výraz, který je implicitně převést na typ levého operandu.</span><span class="sxs-lookup"><span data-stu-id="dec56-2637">In a simple assignment, the right operand must be an expression that is implicitly convertible to the type of the left operand.</span></span> <span data-ttu-id="dec56-2638">Operace přiřadí hodnotu pravý operand elementu proměnná, vlastnost nebo indexer Dal levý operand.</span><span class="sxs-lookup"><span data-stu-id="dec56-2638">The operation assigns the value of the right operand to the variable, property, or indexer element given by the left operand.</span></span>

<span data-ttu-id="dec56-2639">Výsledek výrazu jednoduché přiřazení je hodnota přiřazená k levý operand.</span><span class="sxs-lookup"><span data-stu-id="dec56-2639">The result of a simple assignment expression is the value assigned to the left operand.</span></span> <span data-ttu-id="dec56-2640">Výsledek je stejného typu jako levý operand a vždy je klasifikován jako hodnotu.</span><span class="sxs-lookup"><span data-stu-id="dec56-2640">The result has the same type as the left operand and is always classified as a value.</span></span>

<span data-ttu-id="dec56-2641">Pokud levý operand je přístup vlastnost nebo indexer, vlastnost nebo indexovací člen musí mít `set` přistupujícího objektu.</span><span class="sxs-lookup"><span data-stu-id="dec56-2641">If the left operand is a property or indexer access, the property or indexer must have a `set` accessor.</span></span> <span data-ttu-id="dec56-2642">Pokud to není tento případ, dojde k chybě vazby čas.</span><span class="sxs-lookup"><span data-stu-id="dec56-2642">If this is not the case, a binding-time error occurs.</span></span>

<span data-ttu-id="dec56-2643">Zpracování za běhu jednoduché přiřazení formuláře `x = y` se skládá z následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="dec56-2643">The run-time processing of a simple assignment of the form `x = y` consists of the following steps:</span></span>

*  <span data-ttu-id="dec56-2644">Pokud `x` je klasifikován jako proměnnou:</span><span class="sxs-lookup"><span data-stu-id="dec56-2644">If `x` is classified as a variable:</span></span>
   * <span data-ttu-id="dec56-2645">`x` je vyhodnocen pro vytvoření proměnné.</span><span class="sxs-lookup"><span data-stu-id="dec56-2645">`x` is evaluated to produce the variable.</span></span>
   * <span data-ttu-id="dec56-2646">`y` je vyhodnocen a v případě potřeby převeden na typ `x` prostřednictvím implicitního převodu ([implicitních převodů](conversions.md#implicit-conversions)).</span><span class="sxs-lookup"><span data-stu-id="dec56-2646">`y` is evaluated and, if required, converted to the type of `x` through an implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)).</span></span>
   * <span data-ttu-id="dec56-2647">Pokud proměnná Dal `x` je prvek pole *reference_type*, kontrolu za běhu se provádí za účelem zajištění, že hodnotu vypočítat pro `y` je kompatibilní s poli instance `x` je element.</span><span class="sxs-lookup"><span data-stu-id="dec56-2647">If the variable given by `x` is an array element of a *reference_type*, a run-time check is performed to ensure that the value computed for `y` is compatible with the array instance of which `x` is an element.</span></span> <span data-ttu-id="dec56-2648">Kontrola proběhne úspěšně, pokud `y` je `null`, nebo pokud implicitní referenční převod ([odkaz na implicitní převody](conversions.md#implicit-reference-conversions)) z skutečné typu instance odkazované procedurou `y` skutečným prvkem typu pole instance nadřazeného `x`.</span><span class="sxs-lookup"><span data-stu-id="dec56-2648">The check succeeds if `y` is `null`, or if an implicit reference conversion ([Implicit reference conversions](conversions.md#implicit-reference-conversions)) exists from the actual type of the instance referenced by `y` to the actual element type of the array instance containing `x`.</span></span> <span data-ttu-id="dec56-2649">V opačném případě `System.ArrayTypeMismatchException` je vyvolána výjimka.</span><span class="sxs-lookup"><span data-stu-id="dec56-2649">Otherwise, a `System.ArrayTypeMismatchException` is thrown.</span></span>
   * <span data-ttu-id="dec56-2650">Hodnota plynoucí z hodnocení a převod `y` jsou uložena do umístění, Dal po vyhodnocení `x`.</span><span class="sxs-lookup"><span data-stu-id="dec56-2650">The value resulting from the evaluation and conversion of `y` is stored into the location given by the evaluation of `x`.</span></span>
*  <span data-ttu-id="dec56-2651">Pokud `x` klasifikovaný jako vlastnost nebo indexovací člen přístupu:</span><span class="sxs-lookup"><span data-stu-id="dec56-2651">If `x` is classified as a property or indexer access:</span></span>
   * <span data-ttu-id="dec56-2652">Výraz instance (Pokud `x` není `static`) a seznam argumentů (Pokud `x` představuje přístup k indexer) přidružené k `x` jsou vyhodnocovány, a výsledky se používají v následné `set` vyvolání přistupujícího objektu.</span><span class="sxs-lookup"><span data-stu-id="dec56-2652">The instance expression (if `x` is not `static`) and the argument list (if `x` is an indexer access) associated with `x` are evaluated, and the results are used in the subsequent `set` accessor invocation.</span></span>
   * <span data-ttu-id="dec56-2653">`y` je vyhodnocen a v případě potřeby převeden na typ `x` prostřednictvím implicitního převodu ([implicitních převodů](conversions.md#implicit-conversions)).</span><span class="sxs-lookup"><span data-stu-id="dec56-2653">`y` is evaluated and, if required, converted to the type of `x` through an implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)).</span></span>
   * <span data-ttu-id="dec56-2654">`set` Přistupující objekt `x` je volána s hodnotou pro vypočítat `y` jako jeho `value` argument.</span><span class="sxs-lookup"><span data-stu-id="dec56-2654">The `set` accessor of `x` is invoked with the value computed for `y` as its `value` argument.</span></span>

<span data-ttu-id="dec56-2655">Pravidla pole společné odchylky ([kovariance polí](arrays.md#array-covariance)) povolit hodnotu typu pole `A[]` byl odkaz na instanci typu pole `B[]`, pokud existuje implicitní referenční převod z `B` do `A`.</span><span class="sxs-lookup"><span data-stu-id="dec56-2655">The array co-variance rules ([Array covariance](arrays.md#array-covariance)) permit a value of an array type `A[]` to be a reference to an instance of an array type `B[]`, provided an implicit reference conversion exists from `B` to `A`.</span></span> <span data-ttu-id="dec56-2656">Protože tato pravidla přiřazení k prvku pole *reference_type* vyžaduje kontrolu za běhu k zajištění, že přiřazené hodnoty je kompatibilní s poli instance.</span><span class="sxs-lookup"><span data-stu-id="dec56-2656">Because of these rules, assignment to an array element of a *reference_type* requires a run-time check to ensure that the value being assigned is compatible with the array instance.</span></span> <span data-ttu-id="dec56-2657">V příkladu</span><span class="sxs-lookup"><span data-stu-id="dec56-2657">In the example</span></span>
```csharp
string[] sa = new string[10];
object[] oa = sa;

oa[0] = null;               // Ok
oa[1] = "Hello";            // Ok
oa[2] = new ArrayList();    // ArrayTypeMismatchException
```
<span data-ttu-id="dec56-2658">způsobí, že poslední přiřazení `System.ArrayTypeMismatchException` vyvolání, protože instance `ArrayList` nelze ukládat v prvku `string[]`.</span><span class="sxs-lookup"><span data-stu-id="dec56-2658">the last assignment causes a `System.ArrayTypeMismatchException` to be thrown because an instance of `ArrayList` cannot be stored in an element of a `string[]`.</span></span>

<span data-ttu-id="dec56-2659">Pokud vlastnost nebo indexovací člen deklarovaný v *struct_type* cílem přiřazení, výraz instance přidružené vlastnost nebo indexer přístupu musí být zařazeny jako proměnnou.</span><span class="sxs-lookup"><span data-stu-id="dec56-2659">When a property or indexer declared in a *struct_type* is the target of an assignment, the instance expression associated with the property or indexer access must be classified as a variable.</span></span> <span data-ttu-id="dec56-2660">Pokud výraz instance je klasifikován tak hodnotu, dojde k chybě vazby čas.</span><span class="sxs-lookup"><span data-stu-id="dec56-2660">If the instance expression is classified as a value, a binding-time error occurs.</span></span> <span data-ttu-id="dec56-2661">Z důvodu [přístup ke členu](expressions.md#member-access), stejné pravidlo platí také pro pole.</span><span class="sxs-lookup"><span data-stu-id="dec56-2661">Because of [Member access](expressions.md#member-access), the same rule also applies to fields.</span></span>

<span data-ttu-id="dec56-2662">Zadané deklarace:</span><span class="sxs-lookup"><span data-stu-id="dec56-2662">Given the declarations:</span></span>
```csharp
struct Point
{
    int x, y;

    public Point(int x, int y) {
        this.x = x;
        this.y = y;
    }

    public int X {
        get { return x; }
        set { x = value; }
    }

    public int Y {
        get { return y; }
        set { y = value; }
    }
}

struct Rectangle
{
    Point a, b;

    public Rectangle(Point a, Point b) {
        this.a = a;
        this.b = b;
    }

    public Point A {
        get { return a; }
        set { a = value; }
    }

    public Point B {
        get { return b; }
        set { b = value; }
    }
}
```
<span data-ttu-id="dec56-2663">V příkladu</span><span class="sxs-lookup"><span data-stu-id="dec56-2663">in the example</span></span>
```csharp
Point p = new Point();
p.X = 100;
p.Y = 100;
Rectangle r = new Rectangle();
r.A = new Point(10, 10);
r.B = p;
```
<span data-ttu-id="dec56-2664">přiřazení k `p.X`, `p.Y`, `r.A`, a `r.B` se nepovoluje, protože `p` a `r` jsou proměnné.</span><span class="sxs-lookup"><span data-stu-id="dec56-2664">the assignments to `p.X`, `p.Y`, `r.A`, and `r.B` are permitted because `p` and `r` are variables.</span></span> <span data-ttu-id="dec56-2665">V tomto příkladu však</span><span class="sxs-lookup"><span data-stu-id="dec56-2665">However, in the example</span></span>
```csharp
Rectangle r = new Rectangle();
r.A.X = 10;
r.A.Y = 10;
r.B.X = 100;
r.B.Y = 100;
```
<span data-ttu-id="dec56-2666">přiřazení jsou všechny neplatné, protože `r.A` a `r.B` nejsou proměnné.</span><span class="sxs-lookup"><span data-stu-id="dec56-2666">the assignments are all invalid, since `r.A` and `r.B` are not variables.</span></span>

### <a name="compound-assignment"></a><span data-ttu-id="dec56-2667">Složené přiřazení</span><span class="sxs-lookup"><span data-stu-id="dec56-2667">Compound assignment</span></span>

<span data-ttu-id="dec56-2668">Pokud levý operand složeného přiřazení má formát `E.P` nebo `E[Ei]` kde `E` má typ kompilace `dynamic`, pak přiřazení je vázán dynamicky ([dynamické vazby](expressions.md#dynamic-binding)).</span><span class="sxs-lookup"><span data-stu-id="dec56-2668">If the left operand of a compound assignment is of the form `E.P` or `E[Ei]` where `E` has the compile-time type `dynamic`, then the assignment is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)).</span></span> <span data-ttu-id="dec56-2669">V tomto případě je typ kompilaci výrazu přiřazení `dynamic`, a rozlišení je popsáno níže se provede v době běhu na základě typu za běhu `E`.</span><span class="sxs-lookup"><span data-stu-id="dec56-2669">In this case the compile-time type of the assignment expression is `dynamic`, and the resolution described below will take place at run-time based on the run-time type of `E`.</span></span>

<span data-ttu-id="dec56-2670">Operace formuláře `x op= y` zpracování s použitím binárním operátorem rozlišení přetížení ([binárním operátorem rozlišení přetěžování](expressions.md#binary-operator-overload-resolution)) jako kdyby byla zapsána operaci `x op y`.</span><span class="sxs-lookup"><span data-stu-id="dec56-2670">An operation of the form `x op= y` is processed by applying binary operator overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) as if the operation was written `x op y`.</span></span> <span data-ttu-id="dec56-2671">Potom</span><span class="sxs-lookup"><span data-stu-id="dec56-2671">Then,</span></span>

*  <span data-ttu-id="dec56-2672">Pokud je implicitně převést na typ návratového typu zvoleném operátorovi `x`, operace se vyhodnotí jako `x = x op y`, s tím rozdílem, že `x` se vyhodnotí pouze jednou.</span><span class="sxs-lookup"><span data-stu-id="dec56-2672">If the return type of the selected operator is implicitly convertible to the type of `x`, the operation is evaluated as `x = x op y`, except that `x` is evaluated only once.</span></span>
*  <span data-ttu-id="dec56-2673">Jinak, pokud vybraný operátor je předdefinovaný operátor, pokud je výslovně převeditelný na typ návratového typu zvoleném operátorovi `x`a pokud `y` implicitně převést na typ `x` nebo operátor, který je operátor, pak operace se vyhodnotí jako posunu `x = (T)(x op y)`, kde `T` je typ `x`, s tím rozdílem, že `x` se vyhodnotí pouze jednou.</span><span class="sxs-lookup"><span data-stu-id="dec56-2673">Otherwise, if the selected operator is a predefined operator, if the return type of the selected operator is explicitly convertible to the type of `x`, and if `y` is implicitly convertible to the type of `x` or the operator is a shift operator, then the operation is evaluated as `x = (T)(x op y)`, where `T` is the type of `x`, except that `x` is evaluated only once.</span></span>
*  <span data-ttu-id="dec56-2674">V opačném případě složeného přiřazení je neplatný a dojde k chybě vazby čas.</span><span class="sxs-lookup"><span data-stu-id="dec56-2674">Otherwise, the compound assignment is invalid, and a binding-time error occurs.</span></span>

<span data-ttu-id="dec56-2675">Termín "vyhodnotí pouze jednou" znamená, že při vyhodnocení `x op y`, výsledky všech základních výrazy `x` jsou dočasně uloženy a pak znovu použít při provádění přiřazení `x`.</span><span class="sxs-lookup"><span data-stu-id="dec56-2675">The term "evaluated only once" means that in the evaluation of `x op y`, the results of any constituent expressions of `x` are temporarily saved and then reused when performing the assignment to `x`.</span></span> <span data-ttu-id="dec56-2676">Například v přiřazení `A()[B()] += C()`, kde `A` je metody vracející `int[]`, a `B` a `C` jsou metody vracející `int`, jsou metody vyvolány pouze jednou, v pořadí `A`, `B`, `C`.</span><span class="sxs-lookup"><span data-stu-id="dec56-2676">For example, in the assignment `A()[B()] += C()`, where `A` is a method returning `int[]`, and `B` and `C` are methods returning `int`, the methods are invoked only once, in the order `A`, `B`, `C`.</span></span>

<span data-ttu-id="dec56-2677">Pokud levý operand složeného přiřazení je přístup k vlastnosti nebo indexeru přístup, vlastnost nebo indexovací člen musí mít obojí `get` přístupového objektu a `set` přistupujícího objektu.</span><span class="sxs-lookup"><span data-stu-id="dec56-2677">When the left operand of a compound assignment is a property access or indexer access, the property or indexer must have both a `get` accessor and a `set` accessor.</span></span> <span data-ttu-id="dec56-2678">Pokud to není tento případ, dojde k chybě vazby čas.</span><span class="sxs-lookup"><span data-stu-id="dec56-2678">If this is not the case, a binding-time error occurs.</span></span>

<span data-ttu-id="dec56-2679">Druhé pravidlo nad povolení `x op= y` má být vyhodnocen jako `x = (T)(x op y)` v některých kontextech.</span><span class="sxs-lookup"><span data-stu-id="dec56-2679">The second rule above permits `x op= y` to be evaluated as `x = (T)(x op y)` in certain contexts.</span></span> <span data-ttu-id="dec56-2680">Existuje pravidlo tak, aby předdefinované operátory lze používat jako složené operátory, pokud levý operand je typu `sbyte`, `byte`, `short`, `ushort`, nebo `char`.</span><span class="sxs-lookup"><span data-stu-id="dec56-2680">The rule exists such that the predefined operators can be used as compound operators when the left operand is of type `sbyte`, `byte`, `short`, `ushort`, or `char`.</span></span> <span data-ttu-id="dec56-2681">I když oba argumenty jsou jednoho z těchto typů, předdefinované operátory vytvoření výsledku typu `int`, jak je popsáno v [binární číselný propagace](expressions.md#binary-numeric-promotions).</span><span class="sxs-lookup"><span data-stu-id="dec56-2681">Even when both arguments are of one of those types, the predefined operators produce a result of type `int`, as described in [Binary numeric promotions](expressions.md#binary-numeric-promotions).</span></span> <span data-ttu-id="dec56-2682">Proto bez přetypování ji nebude možné přiřadit výsledek levému operandu.</span><span class="sxs-lookup"><span data-stu-id="dec56-2682">Thus, without a cast it would not be possible to assign the result to the left operand.</span></span>

<span data-ttu-id="dec56-2683">Intuitivní efekt pravidla pro předdefinované operátory je jednoduše `x op= y` je povolené, pokud obě z `x op y` a `x = y` nejsou povoleny.</span><span class="sxs-lookup"><span data-stu-id="dec56-2683">The intuitive effect of the rule for predefined operators is simply that `x op= y` is permitted if both of `x op y` and `x = y` are permitted.</span></span> <span data-ttu-id="dec56-2684">V příkladu</span><span class="sxs-lookup"><span data-stu-id="dec56-2684">In the example</span></span>
```csharp
byte b = 0;
char ch = '\0';
int i = 0;

b += 1;             // Ok
b += 1000;          // Error, b = 1000 not permitted
b += i;             // Error, b = i not permitted
b += (byte)i;       // Ok

ch += 1;            // Error, ch = 1 not permitted
ch += (char)1;      // Ok
```
<span data-ttu-id="dec56-2685">intuitivní důvod u každé chyby je, že odpovídající jednoduché přiřazení také bylo chybu.</span><span class="sxs-lookup"><span data-stu-id="dec56-2685">the intuitive reason for each error is that a corresponding simple assignment would also have been an error.</span></span>

<span data-ttu-id="dec56-2686">Zároveň to znamená, že složeného přiřazení, které podporují operace zrušeno vs. operace.</span><span class="sxs-lookup"><span data-stu-id="dec56-2686">This also means that compound assignment operations support lifted operations.</span></span> <span data-ttu-id="dec56-2687">V příkladu</span><span class="sxs-lookup"><span data-stu-id="dec56-2687">In the example</span></span>
```csharp
int? i = 0;
i += 1;             // Ok
```
<span data-ttu-id="dec56-2688">operátor zdvižené `+(int?,int?)` se používá.</span><span class="sxs-lookup"><span data-stu-id="dec56-2688">the lifted operator `+(int?,int?)` is used.</span></span>

### <a name="event-assignment"></a><span data-ttu-id="dec56-2689">Přiřazení události</span><span class="sxs-lookup"><span data-stu-id="dec56-2689">Event assignment</span></span>

<span data-ttu-id="dec56-2690">Pokud levý operand `+=` nebo `-=` operátor je klasifikován tak přístup události, pak je výraz vyhodnocen následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="dec56-2690">If the left operand of a `+=` or `-=` operator is classified as an event access, then the expression is evaluated as follows:</span></span>

*  <span data-ttu-id="dec56-2691">Výraz instance, pokud existuje, události přístupu je vyhodnocen.</span><span class="sxs-lookup"><span data-stu-id="dec56-2691">The instance expression, if any, of the event access is evaluated.</span></span>
*  <span data-ttu-id="dec56-2692">Pravý operand `+=` nebo `-=` operátor je vyhodnocen a, v případě potřeby převeden na typ levého operandu prostřednictvím implicitního převodu ([implicitních převodů](conversions.md#implicit-conversions)).</span><span class="sxs-lookup"><span data-stu-id="dec56-2692">The right operand of the `+=` or `-=` operator is evaluated, and, if required, converted to the type of the left operand through an implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)).</span></span>
*  <span data-ttu-id="dec56-2693">Přístupový objekt události události se vyvolala s seznam argumentů, který se skládá z pravého operandu po vyhodnocení a v případě potřeby převodu.</span><span class="sxs-lookup"><span data-stu-id="dec56-2693">An event accessor of the event is invoked, with argument list consisting of the right operand, after evaluation and, if necessary, conversion.</span></span> <span data-ttu-id="dec56-2694">Pokud se operátor `+=`, `add` přístupového objektu je vyvolán; Pokud operátor, který byl `-=`, `remove` přístupového objektu je vyvolán.</span><span class="sxs-lookup"><span data-stu-id="dec56-2694">If the operator was `+=`, the `add` accessor is invoked; if the operator was `-=`, the `remove` accessor is invoked.</span></span>

<span data-ttu-id="dec56-2695">Výraz přiřazení události nevydává hodnotu.</span><span class="sxs-lookup"><span data-stu-id="dec56-2695">An event assignment expression does not yield a value.</span></span> <span data-ttu-id="dec56-2696">Proto je platný pouze v kontextu výrazu přiřazení událostí *statement_expression* ([příkazy výrazů](statements.md#expression-statements)).</span><span class="sxs-lookup"><span data-stu-id="dec56-2696">Thus, an event assignment expression is valid only in the context of a *statement_expression* ([Expression statements](statements.md#expression-statements)).</span></span>

## <a name="expression"></a><span data-ttu-id="dec56-2697">Výraz</span><span class="sxs-lookup"><span data-stu-id="dec56-2697">Expression</span></span>

<span data-ttu-id="dec56-2698">*Výraz* je buď *non_assignment_expression* nebo *přiřazení*.</span><span class="sxs-lookup"><span data-stu-id="dec56-2698">An *expression* is either a *non_assignment_expression* or an *assignment*.</span></span>

```antlr
expression
    : non_assignment_expression
    | assignment
    ;

non_assignment_expression
    : conditional_expression
    | lambda_expression
    | query_expression
    ;
```

## <a name="constant-expressions"></a><span data-ttu-id="dec56-2699">Výrazy konstant</span><span class="sxs-lookup"><span data-stu-id="dec56-2699">Constant expressions</span></span>

<span data-ttu-id="dec56-2700">A *constant_expression* je výraz, který může být plně vyhodnocen v době kompilace.</span><span class="sxs-lookup"><span data-stu-id="dec56-2700">A *constant_expression* is an expression that can be fully evaluated at compile-time.</span></span>

```antlr
constant_expression
    : expression
    ;
```

<span data-ttu-id="dec56-2701">Musí být konstantní výraz `null` literál nebo hodnotu s jedním z následujících typů: `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char` , `float`, `double`, `decimal`, `bool`, `object`, `string`, nebo libovolný typ výčtu.</span><span class="sxs-lookup"><span data-stu-id="dec56-2701">A constant expression must be the `null` literal or a value with one of  the following types: `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, `decimal`, `bool`, `object`, `string`, or any enumeration type.</span></span> <span data-ttu-id="dec56-2702">Pouze následující konstrukce jsou povoleny v konstantních výrazech:</span><span class="sxs-lookup"><span data-stu-id="dec56-2702">Only the following constructs are permitted in constant expressions:</span></span>

*  <span data-ttu-id="dec56-2703">Literály (včetně `null` literál).</span><span class="sxs-lookup"><span data-stu-id="dec56-2703">Literals (including the `null` literal).</span></span>
*  <span data-ttu-id="dec56-2704">Odkazy na `const` členy třídy a struktury typů.</span><span class="sxs-lookup"><span data-stu-id="dec56-2704">References to `const` members of class and struct types.</span></span>
*  <span data-ttu-id="dec56-2705">Odkazy na členy typů výčtu.</span><span class="sxs-lookup"><span data-stu-id="dec56-2705">References to members of enumeration types.</span></span>
*  <span data-ttu-id="dec56-2706">Odkazy na `const` parametry nebo lokální proměnné</span><span class="sxs-lookup"><span data-stu-id="dec56-2706">References to `const` parameters or local variables</span></span>
*  <span data-ttu-id="dec56-2707">V závorkách dílčí výrazy, které představují samy o sobě konstantní výrazy.</span><span class="sxs-lookup"><span data-stu-id="dec56-2707">Parenthesized sub-expressions, which are themselves constant expressions.</span></span>
*  <span data-ttu-id="dec56-2708">Výrazy přetypování, poskytuje cílový typ je jedním z typů uvedených výše.</span><span class="sxs-lookup"><span data-stu-id="dec56-2708">Cast expressions, provided the target type is one of the types listed above.</span></span>
*  <span data-ttu-id="dec56-2709">`checked` a `unchecked` výrazy</span><span class="sxs-lookup"><span data-stu-id="dec56-2709">`checked` and `unchecked` expressions</span></span>
*  <span data-ttu-id="dec56-2710">Výrazy s výchozími hodnotami</span><span class="sxs-lookup"><span data-stu-id="dec56-2710">Default value expressions</span></span>
*  <span data-ttu-id="dec56-2711">Výrazy Nameof</span><span class="sxs-lookup"><span data-stu-id="dec56-2711">Nameof expressions</span></span>
*  <span data-ttu-id="dec56-2712">Předdefinované `+`, `-`, `!`, a `~` unárních operátorů.</span><span class="sxs-lookup"><span data-stu-id="dec56-2712">The predefined `+`, `-`, `!`, and `~` unary operators.</span></span>
*  <span data-ttu-id="dec56-2713">Předdefinované `+`, `-`, `*`, `/`, `%`, `<<`, `>>`, `&`, `|`, `^`, `&&`, `||`, `==`, `!=`, `<`, `>`, `<=`, a `>=` binární operátory, k dispozici každý operand je typu uvedené výše.</span><span class="sxs-lookup"><span data-stu-id="dec56-2713">The predefined `+`, `-`, `*`, `/`, `%`, `<<`, `>>`, `&`, `|`, `^`, `&&`, `||`, `==`, `!=`, `<`, `>`, `<=`, and `>=` binary operators, provided each operand is of a type listed above.</span></span>
*  <span data-ttu-id="dec56-2714">`?:` Podmiňovací operátor.</span><span class="sxs-lookup"><span data-stu-id="dec56-2714">The `?:` conditional operator.</span></span>

<span data-ttu-id="dec56-2715">Následující převody jsou povoleny v konstantních výrazech:</span><span class="sxs-lookup"><span data-stu-id="dec56-2715">The following conversions are permitted in constant expressions:</span></span>

*  <span data-ttu-id="dec56-2716">Převody identity</span><span class="sxs-lookup"><span data-stu-id="dec56-2716">Identity conversions</span></span>
*  <span data-ttu-id="dec56-2717">Číselné převody</span><span class="sxs-lookup"><span data-stu-id="dec56-2717">Numeric conversions</span></span>
*  <span data-ttu-id="dec56-2718">Výčet převody</span><span class="sxs-lookup"><span data-stu-id="dec56-2718">Enumeration conversions</span></span>
*  <span data-ttu-id="dec56-2719">Konstantní výraz převody</span><span class="sxs-lookup"><span data-stu-id="dec56-2719">Constant expression conversions</span></span>
*  <span data-ttu-id="dec56-2720">Odkaz na implicitní a explicitní převody, za předpokladu, že zdroj převody je konstantní výraz vyhodnocený na hodnotu null.</span><span class="sxs-lookup"><span data-stu-id="dec56-2720">Implicit and explicit reference conversions, provided that the source of the conversions is a constant expression that evaluates to the null value.</span></span>

<span data-ttu-id="dec56-2721">Ostatní převody, včetně zabalení, rozbalení a implicitní převody odkazů z hodnoty null nejsou povolené v konstantních výrazech.</span><span class="sxs-lookup"><span data-stu-id="dec56-2721">Other conversions including boxing, unboxing and implicit reference conversions of non-null values are not permitted in constant expressions.</span></span> <span data-ttu-id="dec56-2722">Příklad:</span><span class="sxs-lookup"><span data-stu-id="dec56-2722">For example:</span></span>
```csharp
class C {
    const object i = 5;         // error: boxing conversion not permitted
    const object str = "hello"; // error: implicit reference conversion
}
```
<span data-ttu-id="dec56-2723">Inicializace i se o chybu, protože je vyžadován převod na uzavřené určení.</span><span class="sxs-lookup"><span data-stu-id="dec56-2723">the initialization of i is an error because a boxing conversion is required.</span></span> <span data-ttu-id="dec56-2724">Inicializace str se o chybu, protože je vyžadován implicitní referenční převod z nenulová hodnota.</span><span class="sxs-lookup"><span data-stu-id="dec56-2724">The initialization of str is an error because an implicit reference conversion from a non-null value is required.</span></span>

<span data-ttu-id="dec56-2725">Pokaždé, když se výraz splňuje všechny požadavky uvedené výše, je výraz vyhodnocen v době kompilace.</span><span class="sxs-lookup"><span data-stu-id="dec56-2725">Whenever an expression fulfills the requirements listed above, the expression is evaluated at compile-time.</span></span> <span data-ttu-id="dec56-2726">To platí i v případě, že je dílčí výraz rozsáhlejšího výrazu, který obsahuje konstrukce, která není konstantní výraz.</span><span class="sxs-lookup"><span data-stu-id="dec56-2726">This is true even if the expression is a sub-expression of a larger expression that contains non-constant constructs.</span></span>

<span data-ttu-id="dec56-2727">Vyhodnocení za kompilace konstantní výrazy používá stejná pravidla jako vyhodnocení výrazů, která není konstantní, s tím rozdílem, kde by být vyvolána hodnocení za běhu výjimku, vyhodnocení za kompilace způsobí chybu kompilace dojde k.</span><span class="sxs-lookup"><span data-stu-id="dec56-2727">The compile-time evaluation of constant expressions uses the same rules as run-time evaluation of non-constant expressions, except that where run-time evaluation would have thrown an exception, compile-time evaluation causes a compile-time error to occur.</span></span>

<span data-ttu-id="dec56-2728">Pokud není konstantní výraz se explicitně umístí do `unchecked` kontextu, přetečení, ke kterým dochází v integrálového typu aritmetické operace a převody během vyhodnocení za kompilace výrazu vždy způsobit chyby kompilace ([Konstantní výrazy](expressions.md#constant-expressions)).</span><span class="sxs-lookup"><span data-stu-id="dec56-2728">Unless a constant expression is explicitly placed in an `unchecked` context, overflows that occur in integral-type arithmetic operations and conversions during the compile-time evaluation of the expression always cause compile-time errors ([Constant expressions](expressions.md#constant-expressions)).</span></span>

<span data-ttu-id="dec56-2729">Konstantní výrazy dojít v kontextech uvedených níže.</span><span class="sxs-lookup"><span data-stu-id="dec56-2729">Constant expressions occur in the contexts listed below.</span></span> <span data-ttu-id="dec56-2730">V těchto kontextech dojde k chybě kompilace Pokud výraz nejde vyhodnotit plně v době kompilace.</span><span class="sxs-lookup"><span data-stu-id="dec56-2730">In these contexts, a compile-time error occurs if an expression cannot be fully evaluated at compile-time.</span></span>

*  <span data-ttu-id="dec56-2731">Deklarace konstant ([konstanty](classes.md#constants)).</span><span class="sxs-lookup"><span data-stu-id="dec56-2731">Constant declarations ([Constants](classes.md#constants)).</span></span>
*  <span data-ttu-id="dec56-2732">Deklarace členů výčtu ([členy výčtu](enums.md#enum-members)).</span><span class="sxs-lookup"><span data-stu-id="dec56-2732">Enumeration member declarations ([Enum members](enums.md#enum-members)).</span></span>
*  <span data-ttu-id="dec56-2733">Výchozí argumenty seznam formálních parametrů ([parametry metody](classes.md#method-parameters))</span><span class="sxs-lookup"><span data-stu-id="dec56-2733">Default arguments of formal parameter lists ([Method parameters](classes.md#method-parameters))</span></span>
*  <span data-ttu-id="dec56-2734">`case` popisky `switch` – příkaz ([příkazu switch](statements.md#the-switch-statement)).</span><span class="sxs-lookup"><span data-stu-id="dec56-2734">`case` labels of a `switch` statement ([The switch statement](statements.md#the-switch-statement)).</span></span>
*  <span data-ttu-id="dec56-2735">`goto case` příkazy ([příkazu goto](statements.md#the-goto-statement)).</span><span class="sxs-lookup"><span data-stu-id="dec56-2735">`goto case` statements ([The goto statement](statements.md#the-goto-statement)).</span></span>
*  <span data-ttu-id="dec56-2736">Dimenze délky ve výraz vytvoření pole ([pole vytváření výrazů](expressions.md#array-creation-expressions)), která obsahuje inicializátor.</span><span class="sxs-lookup"><span data-stu-id="dec56-2736">Dimension lengths in an array creation expression ([Array creation expressions](expressions.md#array-creation-expressions)) that includes an initializer.</span></span>
*  <span data-ttu-id="dec56-2737">Atributy ([atributy](attributes.md)).</span><span class="sxs-lookup"><span data-stu-id="dec56-2737">Attributes ([Attributes](attributes.md)).</span></span>

<span data-ttu-id="dec56-2738">Konverzi implicitní konstantní výraz ([konstantní výraz implicitní převody](conversions.md#implicit-constant-expression-conversions)) umožňuje konstantní výraz typu `int` má být převeden na `sbyte`, `byte`, `short`, `ushort`, `uint`, nebo `ulong`, pokud má konstantní výraz hodnotu v rozsahu cílového typu.</span><span class="sxs-lookup"><span data-stu-id="dec56-2738">An implicit constant expression conversion ([Implicit constant expression conversions](conversions.md#implicit-constant-expression-conversions)) permits a constant expression of type `int` to be converted to `sbyte`, `byte`, `short`, `ushort`, `uint`, or `ulong`, provided the value of the constant expression is within the range of the destination type.</span></span>

## <a name="boolean-expressions"></a><span data-ttu-id="dec56-2739">logické výrazy</span><span class="sxs-lookup"><span data-stu-id="dec56-2739">Boolean expressions</span></span>

<span data-ttu-id="dec56-2740">A *boolean_expression* je výraz, který poskytuje výsledek typu `bool`; buď přímo nebo prostřednictvím použití `operator true` v některých kontextech, jak je uvedeno v následující.</span><span class="sxs-lookup"><span data-stu-id="dec56-2740">A *boolean_expression* is an expression that yields a result of type `bool`; either directly or through application of `operator true` in certain contexts as specified in the following.</span></span>

```antlr
boolean_expression
    : expression
    ;
```

<span data-ttu-id="dec56-2741">Řízení podmíněného výrazu *if_statement* ([if – příkaz](statements.md#the-if-statement)), *while_statement* ([while – příkaz](statements.md#the-while-statement)), *do_statement* ([proveďte příkaz](statements.md#the-do-statement)), nebo *for_statement* ([pro příkaz](statements.md#the-for-statement)) je *boolean_ výraz*.</span><span class="sxs-lookup"><span data-stu-id="dec56-2741">The controlling conditional expression of an *if_statement* ([The if statement](statements.md#the-if-statement)), *while_statement* ([The while statement](statements.md#the-while-statement)), *do_statement* ([The do statement](statements.md#the-do-statement)), or *for_statement* ([The for statement](statements.md#the-for-statement)) is a *boolean_expression*.</span></span> <span data-ttu-id="dec56-2742">Řízení podmíněného výrazu `?:` – operátor ([Podmiňovací operátor](expressions.md#conditional-operator)) následuje stejná pravidla jako *boolean_expression*, ale z důvodu operátoru je sestavení klasifikováno prioritu jako *conditional_or_expression*.</span><span class="sxs-lookup"><span data-stu-id="dec56-2742">The controlling conditional expression of the `?:` operator ([Conditional operator](expressions.md#conditional-operator)) follows the same rules as a *boolean_expression*, but for reasons of operator precedence is classified as a *conditional_or_expression*.</span></span>

<span data-ttu-id="dec56-2743">A *boolean_expression* `E` musí být schopny vytvořit hodnotu typu `bool`, následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="dec56-2743">A *boolean_expression* `E` is required to be able to produce a value of type `bool`, as follows:</span></span>

*  <span data-ttu-id="dec56-2744">Pokud `E` implicitně převést na `bool` pak za běhu se použije implicitní převodu.</span><span class="sxs-lookup"><span data-stu-id="dec56-2744">If `E` is implicitly convertible to `bool` then at runtime that implicit conversion is applied.</span></span>
*  <span data-ttu-id="dec56-2745">V opačném případě unární operátor rozlišení přetěžování ([unární operátor rozlišení přetěžování](expressions.md#unary-operator-overload-resolution)) slouží k vyhledání jedinečný nejlepší implementace operátoru `true` na `E`, a tuto implementaci se použije v době běhu.</span><span class="sxs-lookup"><span data-stu-id="dec56-2745">Otherwise, unary operator overload resolution ([Unary operator overload resolution](expressions.md#unary-operator-overload-resolution)) is used to find a unique best implementation of operator `true` on `E`, and that implementation is applied at runtime.</span></span>
*  <span data-ttu-id="dec56-2746">Pokud se nenajde žádný takový operátor, dojde k chybě vazby čas.</span><span class="sxs-lookup"><span data-stu-id="dec56-2746">If no such operator is found, a binding-time error occurs.</span></span>

<span data-ttu-id="dec56-2747">`DBBool` Typu Struktura v [databáze typu boolean](structs.md#database-boolean-type) poskytuje příklad, typ, který implementuje `operator true` a `operator false`.</span><span class="sxs-lookup"><span data-stu-id="dec56-2747">The `DBBool` struct type in [Database boolean type](structs.md#database-boolean-type) provides an example of a type that implements `operator true` and `operator false`.</span></span>
