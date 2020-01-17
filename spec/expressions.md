---
ms.openlocfilehash: f61039abd6bd557ac0ea625e6aac1c8bafa57b02
ms.sourcegitcommit: e134bb7058e9848120b93b345f96d6ac0cb8c815
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/17/2020
ms.locfileid: "71704077"
---
# <a name="expressions"></a><span data-ttu-id="08560-101">Výrazy</span><span class="sxs-lookup"><span data-stu-id="08560-101">Expressions</span></span>

<span data-ttu-id="08560-102">Výraz je sekvence operátorů a operandů.</span><span class="sxs-lookup"><span data-stu-id="08560-102">An expression is a sequence of operators and operands.</span></span> <span data-ttu-id="08560-103">Tato kapitola definuje syntaxi, pořadí vyhodnocování operandů a operátorů a význam výrazů.</span><span class="sxs-lookup"><span data-stu-id="08560-103">This chapter defines the syntax, order of evaluation of operands and operators, and meaning of expressions.</span></span>

## <a name="expression-classifications"></a><span data-ttu-id="08560-104">Klasifikace výrazů</span><span class="sxs-lookup"><span data-stu-id="08560-104">Expression classifications</span></span>

<span data-ttu-id="08560-105">Výraz je klasifikován jako jeden z následujících:</span><span class="sxs-lookup"><span data-stu-id="08560-105">An expression is classified as one of the following:</span></span>

*  <span data-ttu-id="08560-106">Hodnota.</span><span class="sxs-lookup"><span data-stu-id="08560-106">A value.</span></span> <span data-ttu-id="08560-107">Každá hodnota má přidružený typ.</span><span class="sxs-lookup"><span data-stu-id="08560-107">Every value has an associated type.</span></span>
*  <span data-ttu-id="08560-108">Proměnná.</span><span class="sxs-lookup"><span data-stu-id="08560-108">A variable.</span></span> <span data-ttu-id="08560-109">Každá proměnná má přidružený typ, konkrétně deklarovaný typ proměnné.</span><span class="sxs-lookup"><span data-stu-id="08560-109">Every variable has an associated type, namely the declared type of the variable.</span></span>
*  <span data-ttu-id="08560-110">Obor názvů.</span><span class="sxs-lookup"><span data-stu-id="08560-110">A namespace.</span></span> <span data-ttu-id="08560-111">Výraz s touto klasifikací se může vyskytovat jenom jako levá strana *member_access* ([přístup ke členům](expressions.md#member-access)).</span><span class="sxs-lookup"><span data-stu-id="08560-111">An expression with this classification can only appear as the left hand side of a *member_access* ([Member access](expressions.md#member-access)).</span></span> <span data-ttu-id="08560-112">V jakémkoli jiném kontextu výraz klasifikovaný jako obor názvů způsobí chybu při kompilaci.</span><span class="sxs-lookup"><span data-stu-id="08560-112">In any other context, an expression classified as a namespace causes a compile-time error.</span></span>
*  <span data-ttu-id="08560-113">Typ.</span><span class="sxs-lookup"><span data-stu-id="08560-113">A type.</span></span> <span data-ttu-id="08560-114">Výraz s touto klasifikací se může vyskytovat jenom jako levá strana *member_access* ([přístup ke členu](expressions.md#member-access)) nebo jako Operand operátoru `as` ([operátor as](expressions.md#the-as-operator)), operátor `is` (operátor[is](expressions.md#the-is-operator)) nebo operátor `typeof` ([operátor typeof](expressions.md#the-typeof-operator)).</span><span class="sxs-lookup"><span data-stu-id="08560-114">An expression with this classification can only appear as the left hand side of a *member_access* ([Member access](expressions.md#member-access)), or as an operand for the `as` operator ([The as operator](expressions.md#the-as-operator)), the `is` operator ([The is operator](expressions.md#the-is-operator)), or the `typeof` operator ([The typeof operator](expressions.md#the-typeof-operator)).</span></span> <span data-ttu-id="08560-115">V jakémkoli jiném kontextu výraz klasifikovaný jako typ způsobí chybu při kompilaci.</span><span class="sxs-lookup"><span data-stu-id="08560-115">In any other context, an expression classified as a type causes a compile-time error.</span></span>
*  <span data-ttu-id="08560-116">Skupina metod, která je sadou přetížených metod, která vyplývají z vyhledávání členů ([vyhledávání členů](expressions.md#member-lookup)).</span><span class="sxs-lookup"><span data-stu-id="08560-116">A method group, which is a set of overloaded methods resulting from a member lookup ([Member lookup](expressions.md#member-lookup)).</span></span> <span data-ttu-id="08560-117">Skupina metod může mít přidružený výraz instance a přidružený seznam argumentů typu.</span><span class="sxs-lookup"><span data-stu-id="08560-117">A method group may have an associated instance expression and an associated type argument list.</span></span> <span data-ttu-id="08560-118">Když je vyvolána metoda instance, výsledek vyhodnocení výrazu instance se zobrazí jako instance reprezentované `this` ([Tento přístup](expressions.md#this-access)).</span><span class="sxs-lookup"><span data-stu-id="08560-118">When an instance method is invoked, the result of evaluating the instance expression becomes the instance represented by `this` ([This access](expressions.md#this-access)).</span></span> <span data-ttu-id="08560-119">Skupina metod je povolena ve *invocation_expression* ([výrazy vyvolání](expressions.md#invocation-expressions)), *delegate_creation_expression* ([výrazy vytvoření delegáta](expressions.md#delegate-creation-expressions)) a jako levá strana operátoru is a lze jej implicitně převést na kompatibilní typ delegáta ([převody skupin metod](conversions.md#method-group-conversions)).</span><span class="sxs-lookup"><span data-stu-id="08560-119">A method group is permitted in an *invocation_expression* ([Invocation expressions](expressions.md#invocation-expressions)) , a *delegate_creation_expression* ([Delegate creation expressions](expressions.md#delegate-creation-expressions)) and as the left hand side of an is operator, and can be implicitly converted to a compatible delegate type ([Method group conversions](conversions.md#method-group-conversions)).</span></span> <span data-ttu-id="08560-120">V jakémkoli jiném kontextu výraz klasifikovaný jako skupina metod způsobí chybu při kompilaci.</span><span class="sxs-lookup"><span data-stu-id="08560-120">In any other context, an expression classified as a method group causes a compile-time error.</span></span>
*  <span data-ttu-id="08560-121">Literál s hodnotou null.</span><span class="sxs-lookup"><span data-stu-id="08560-121">A null literal.</span></span> <span data-ttu-id="08560-122">Výraz s touto klasifikací se dá implicitně převést na typ odkazu nebo na typ s možnou hodnotou null.</span><span class="sxs-lookup"><span data-stu-id="08560-122">An expression with this classification can be implicitly converted to a reference type or nullable type.</span></span>
*  <span data-ttu-id="08560-123">Anonymní funkce.</span><span class="sxs-lookup"><span data-stu-id="08560-123">An anonymous function.</span></span> <span data-ttu-id="08560-124">Výraz s touto klasifikací lze implicitně převést na kompatibilní typ delegáta nebo strom výrazu.</span><span class="sxs-lookup"><span data-stu-id="08560-124">An expression with this classification can be implicitly converted to a compatible delegate type or expression tree type.</span></span>
*  <span data-ttu-id="08560-125">Přístup k vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="08560-125">A property access.</span></span> <span data-ttu-id="08560-126">Každý přístup k vlastnosti má přidružený typ, konkrétně typ vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="08560-126">Every property access has an associated type, namely the type of the property.</span></span> <span data-ttu-id="08560-127">Kromě toho může mít přístup k vlastnosti přidružený výraz instance.</span><span class="sxs-lookup"><span data-stu-id="08560-127">Furthermore, a property access may have an associated instance expression.</span></span> <span data-ttu-id="08560-128">Když je vyvolán přistupující objekt (`get` nebo `set`) k vlastnosti instance, výsledek vyhodnocení výrazu instance se projeví jako instance reprezentované `this` ([Tento přístup](expressions.md#this-access)).</span><span class="sxs-lookup"><span data-stu-id="08560-128">When an accessor (the `get` or `set` block) of an instance property access is invoked, the result of evaluating the instance expression becomes the instance represented by `this` ([This access](expressions.md#this-access)).</span></span>
*  <span data-ttu-id="08560-129">Přístup k události.</span><span class="sxs-lookup"><span data-stu-id="08560-129">An event access.</span></span> <span data-ttu-id="08560-130">Každý přístup k události má přidružený typ, konkrétně typ události.</span><span class="sxs-lookup"><span data-stu-id="08560-130">Every event access has an associated type, namely the type of the event.</span></span> <span data-ttu-id="08560-131">Kromě toho může mít přístup k události přidružený výraz instance.</span><span class="sxs-lookup"><span data-stu-id="08560-131">Furthermore, an event access may have an associated instance expression.</span></span> <span data-ttu-id="08560-132">Přístup k události se může zobrazit jako levý operand operátoru `+=` a `-=` ([přiřazení události](expressions.md#event-assignment)).</span><span class="sxs-lookup"><span data-stu-id="08560-132">An event access may appear as the left hand operand of the `+=` and `-=` operators ([Event assignment](expressions.md#event-assignment)).</span></span> <span data-ttu-id="08560-133">V jakémkoli jiném kontextu výraz klasifikovaný jako přístup k události způsobí chybu při kompilaci.</span><span class="sxs-lookup"><span data-stu-id="08560-133">In any other context, an expression classified as an event access causes a compile-time error.</span></span>
*  <span data-ttu-id="08560-134">Přístup indexeru</span><span class="sxs-lookup"><span data-stu-id="08560-134">An indexer access.</span></span> <span data-ttu-id="08560-135">Každý přístup k indexeru má přidružený typ, konkrétně typ prvku indexeru.</span><span class="sxs-lookup"><span data-stu-id="08560-135">Every indexer access has an associated type, namely the element type of the indexer.</span></span> <span data-ttu-id="08560-136">Kromě toho má k přístupu indexeru přidružený výraz instance a seznam přidružených argumentů.</span><span class="sxs-lookup"><span data-stu-id="08560-136">Furthermore, an indexer access has an associated instance expression and an associated argument list.</span></span> <span data-ttu-id="08560-137">Při vyvolání přístupového objektu (`get` nebo `set`) k vyvolání indexeru se výsledek vyhodnocení výrazu instance stala instancí reprezentovanou `this` ([Tento přístup](expressions.md#this-access)) a výsledkem vyhodnocení seznamu argumentů se bude seznam parametrů vyvolání.</span><span class="sxs-lookup"><span data-stu-id="08560-137">When an accessor (the `get` or `set` block) of an indexer access is invoked, the result of evaluating the instance expression becomes the instance represented by `this` ([This access](expressions.md#this-access)), and the result of evaluating the argument list becomes the parameter list of the invocation.</span></span>
*  <span data-ttu-id="08560-138">Nic</span><span class="sxs-lookup"><span data-stu-id="08560-138">Nothing.</span></span> <span data-ttu-id="08560-139">K tomu dochází, když je výraz voláním metody s návratovým typem `void`.</span><span class="sxs-lookup"><span data-stu-id="08560-139">This occurs when the expression is an invocation of a method with a return type of `void`.</span></span> <span data-ttu-id="08560-140">Výraz klasifikovaný jako Nothing je platný pouze v kontextu *statement_expression* ([příkazy výrazu](statements.md#expression-statements)).</span><span class="sxs-lookup"><span data-stu-id="08560-140">An expression classified as nothing is only valid in the context of a *statement_expression* ([Expression statements](statements.md#expression-statements)).</span></span>

<span data-ttu-id="08560-141">Konečný výsledek výrazu není nikdy obor názvů, typ, skupina metod ani přístup k události.</span><span class="sxs-lookup"><span data-stu-id="08560-141">The final result of an expression is never a namespace, type, method group, or event access.</span></span> <span data-ttu-id="08560-142">Namísto toho, jak je uvedeno výše, jsou tyto kategorie výrazů zprostředkující konstrukce, které jsou povoleny pouze v určitých kontextech.</span><span class="sxs-lookup"><span data-stu-id="08560-142">Rather, as noted above, these categories of expressions are intermediate constructs that are only permitted in certain contexts.</span></span>

<span data-ttu-id="08560-143">Přístup k vlastnosti nebo k indexeru je vždy překlasifikován jako hodnota prostřednictvím vyvolání *přístupového objektu Get* nebo přístupového *objektu set*.</span><span class="sxs-lookup"><span data-stu-id="08560-143">A property access or indexer access is always reclassified as a value by performing an invocation of the *get accessor* or the *set accessor*.</span></span> <span data-ttu-id="08560-144">Konkrétní přistupující objekt je určen kontextem přístup k vlastnosti nebo indexeru: Pokud je přístup cílem přiřazení, je vyvolána přístupová metoda *set* pro přiřazení nové hodnoty ([jednoduché přiřazení](expressions.md#simple-assignment)).</span><span class="sxs-lookup"><span data-stu-id="08560-144">The particular accessor is determined by the context of the property or indexer access: If the access is the target of an assignment, the *set accessor* is invoked to assign a new value ([Simple assignment](expressions.md#simple-assignment)).</span></span> <span data-ttu-id="08560-145">V opačném případě se vyvolá *přístupový objekt get* , který získá aktuální hodnotu ([hodnoty výrazů](expressions.md#values-of-expressions)).</span><span class="sxs-lookup"><span data-stu-id="08560-145">Otherwise, the *get accessor* is invoked to obtain the current value ([Values of expressions](expressions.md#values-of-expressions)).</span></span>

### <a name="values-of-expressions"></a><span data-ttu-id="08560-146">Hodnoty výrazů</span><span class="sxs-lookup"><span data-stu-id="08560-146">Values of expressions</span></span>

<span data-ttu-id="08560-147">Většina konstrukcí, které zahrnují výraz, nakonec vyžadují výraz k označení ***hodnoty***.</span><span class="sxs-lookup"><span data-stu-id="08560-147">Most of the constructs that involve an expression ultimately require the expression to denote a ***value***.</span></span> <span data-ttu-id="08560-148">V takových případech, pokud skutečný výraz označuje obor názvů, typ, skupinu metod nebo nic, dojde k chybě při kompilaci.</span><span class="sxs-lookup"><span data-stu-id="08560-148">In such cases, if the actual expression denotes a namespace, a type, a method group, or nothing, a compile-time error occurs.</span></span> <span data-ttu-id="08560-149">Nicméně pokud výraz označuje přístup k vlastnosti, přístup indexeru nebo proměnnou, hodnota vlastnosti, indexeru nebo proměnné je implicitně nahrazena:</span><span class="sxs-lookup"><span data-stu-id="08560-149">However, if the expression denotes a property access, an indexer access, or a variable, the value of the property, indexer, or variable is implicitly substituted:</span></span>

*  <span data-ttu-id="08560-150">Hodnota proměnné je jednoduše hodnota, která je aktuálně uložena v umístění úložiště identifikovaném proměnnou.</span><span class="sxs-lookup"><span data-stu-id="08560-150">The value of a variable is simply the value currently stored in the storage location identified by the variable.</span></span> <span data-ttu-id="08560-151">Proměnná musí být považována za jednoznačně přiřazenou ([jednoznačné přiřazení](variables.md#definite-assignment)), než se hodnota Získá nebo v opačném případě dojde k chybě při kompilaci.</span><span class="sxs-lookup"><span data-stu-id="08560-151">A variable must be considered definitely assigned ([Definite assignment](variables.md#definite-assignment)) before its value can be obtained, or otherwise a compile-time error occurs.</span></span>
*  <span data-ttu-id="08560-152">Hodnota výrazu přístupu k vlastnosti je získána vyvoláním *přístupového objektu Get* vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="08560-152">The value of a property access expression is obtained by invoking the *get accessor* of the property.</span></span> <span data-ttu-id="08560-153">Pokud vlastnost nemá žádný *přistupující objekt get*, dojde k chybě při kompilaci.</span><span class="sxs-lookup"><span data-stu-id="08560-153">If the property has no *get accessor*, a compile-time error occurs.</span></span> <span data-ttu-id="08560-154">V opačném případě se provede vyvolání člena funkce ([Kontrola dynamického přetěžování při kompilaci](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) a výsledek vyvolání se stává hodnotou výrazu přístupu k vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="08560-154">Otherwise, a function member invocation ([Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) is performed, and the result of the invocation becomes the value of the property access expression.</span></span>
*  <span data-ttu-id="08560-155">Hodnota výrazu přístupu indexeru se získá vyvoláním *přístupové metody Get* indexeru.</span><span class="sxs-lookup"><span data-stu-id="08560-155">The value of an indexer access expression is obtained by invoking the *get accessor* of the indexer.</span></span> <span data-ttu-id="08560-156">Pokud indexer nemá *přístup k Get*, dojde k chybě při kompilaci.</span><span class="sxs-lookup"><span data-stu-id="08560-156">If the indexer has no *get accessor*, a compile-time error occurs.</span></span> <span data-ttu-id="08560-157">V opačném případě je provedeno vyvolání člena funkce ([Kontrola dynamického přetěžování při kompilaci](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) se seznamem argumentů přidruženým k výrazu přístupu indexeru a výsledek vyvolání se stává hodnotou výrazu přístupu indexeru.</span><span class="sxs-lookup"><span data-stu-id="08560-157">Otherwise, a function member invocation ([Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) is performed with the argument list associated with the indexer access expression, and the result of the invocation becomes the value of the indexer access expression.</span></span>

## <a name="static-and-dynamic-binding"></a><span data-ttu-id="08560-158">Statická a dynamická vazba</span><span class="sxs-lookup"><span data-stu-id="08560-158">Static and Dynamic Binding</span></span>

<span data-ttu-id="08560-159">Proces určení významu operace v závislosti na typu nebo hodnotě výrazů prvků (argumenty, operandy, přijímače) se často označují jako ***vazby***.</span><span class="sxs-lookup"><span data-stu-id="08560-159">The process of determining the meaning of an operation based on the type or value of constituent expressions (arguments, operands, receivers) is often referred to as ***binding***.</span></span> <span data-ttu-id="08560-160">Například význam volání metody je určen podle typu přijímače a argumentů.</span><span class="sxs-lookup"><span data-stu-id="08560-160">For instance the meaning of a method call is determined based on the type of the receiver and arguments.</span></span> <span data-ttu-id="08560-161">Význam operátoru je určen na základě typu jeho operandů.</span><span class="sxs-lookup"><span data-stu-id="08560-161">The meaning of an operator is determined based on the type of its operands.</span></span>

<span data-ttu-id="08560-162">Ve C# smyslu operace je obvykle určena v době kompilace na základě typu za kompilace svých základních výrazů.</span><span class="sxs-lookup"><span data-stu-id="08560-162">In C# the meaning of an operation is usually determined at compile-time, based on the compile-time type of its constituent expressions.</span></span> <span data-ttu-id="08560-163">Podobně pokud výraz obsahuje chybu, je zjištěna chyba a hlášena kompilátorem.</span><span class="sxs-lookup"><span data-stu-id="08560-163">Likewise, if an expression contains an error, the error is detected and reported by the compiler.</span></span> <span data-ttu-id="08560-164">Tento přístup je známý jako ***statická vazba***.</span><span class="sxs-lookup"><span data-stu-id="08560-164">This approach is known as ***static binding***.</span></span>

<span data-ttu-id="08560-165">Nicméně pokud je výraz dynamický výraz (tj. má typ `dynamic`), znamená to, že jakákoli vazba, na kterou se podílí, by měla být založena na typu běhu (tj. skutečný typ objektu, který označuje za běhu), nikoli typu, který je v době kompilace.</span><span class="sxs-lookup"><span data-stu-id="08560-165">However, if an expression is a dynamic expression (i.e. has the type `dynamic`) this indicates that any binding that it participates in should be based on its run-time type (i.e. the actual type of the object it denotes at run-time) rather than the type it has at compile-time.</span></span> <span data-ttu-id="08560-166">Vazba takové operace je proto odložena až do doby, kdy má být operace provedena během chodu programu.</span><span class="sxs-lookup"><span data-stu-id="08560-166">The binding of such an operation is therefore deferred until the time where the operation is to be executed during the running of the program.</span></span> <span data-ttu-id="08560-167">Tento postup se označuje jako ***dynamická vazba***.</span><span class="sxs-lookup"><span data-stu-id="08560-167">This is referred to as ***dynamic binding***.</span></span>

<span data-ttu-id="08560-168">Je-li operace dynamicky svázána, je kompilátorem provedena pouze malá nebo žádná kontrola.</span><span class="sxs-lookup"><span data-stu-id="08560-168">When an operation is dynamically bound, little or no checking is performed by the compiler.</span></span> <span data-ttu-id="08560-169">Místo toho, pokud se vazba za běhu nezdařila, jsou chyby hlášeny jako výjimky za běhu.</span><span class="sxs-lookup"><span data-stu-id="08560-169">Instead if the run-time binding fails, errors are reported as exceptions at run-time.</span></span>

<span data-ttu-id="08560-170">Následující operace v C# nástroji jsou předmětem vazby:</span><span class="sxs-lookup"><span data-stu-id="08560-170">The following operations in C# are subject to binding:</span></span>

*  <span data-ttu-id="08560-171">Přístup ke členu: `e.M`</span><span class="sxs-lookup"><span data-stu-id="08560-171">Member access: `e.M`</span></span>
*  <span data-ttu-id="08560-172">Vyvolání metody: `e.M(e1, ..., eN)`</span><span class="sxs-lookup"><span data-stu-id="08560-172">Method invocation: `e.M(e1, ..., eN)`</span></span>
*  <span data-ttu-id="08560-173">Volání delegáta:`e(e1, ..., eN)`</span><span class="sxs-lookup"><span data-stu-id="08560-173">Delegate invocation:`e(e1, ..., eN)`</span></span>
*  <span data-ttu-id="08560-174">Přístup k elementu: `e[e1, ..., eN]`</span><span class="sxs-lookup"><span data-stu-id="08560-174">Element access: `e[e1, ..., eN]`</span></span>
*  <span data-ttu-id="08560-175">Vytvoření objektu: `new C(e1, ..., eN)`</span><span class="sxs-lookup"><span data-stu-id="08560-175">Object creation: `new C(e1, ..., eN)`</span></span>
*  <span data-ttu-id="08560-176">Přetížené unární operátory: `+`, `-`, `!`, `~`, `++`, `--`, `true`, `false`</span><span class="sxs-lookup"><span data-stu-id="08560-176">Overloaded unary operators: `+`, `-`, `!`, `~`, `++`, `--`, `true`, `false`</span></span>
*  <span data-ttu-id="08560-177">Přetížené binární operátory: `+`, `-`, `*`, `/`, `%`, `&`, `&&`, `|`, `||`, `??`, `^`, `<<`, `>>`, `==`,`!=`, `>`, `<`, `>=`, `<=`</span><span class="sxs-lookup"><span data-stu-id="08560-177">Overloaded binary operators: `+`, `-`, `*`, `/`, `%`, `&`, `&&`, `|`, `||`, `??`, `^`, `<<`, `>>`, `==`,`!=`, `>`, `<`, `>=`, `<=`</span></span>
*  <span data-ttu-id="08560-178">Operátory přiřazení: `=`, `+=`, `-=`, `*=`, `/=`, `%=`, `&=`, `|=`, `^=`, `<<=`, `>>=`</span><span class="sxs-lookup"><span data-stu-id="08560-178">Assignment operators: `=`, `+=`, `-=`, `*=`, `/=`, `%=`, `&=`, `|=`, `^=`, `<<=`, `>>=`</span></span>
*  <span data-ttu-id="08560-179">Implicitní a explicitní převody</span><span class="sxs-lookup"><span data-stu-id="08560-179">Implicit and explicit conversions</span></span>

<span data-ttu-id="08560-180">Pokud nejsou zapojeny žádné dynamické výrazy C# , výchozí hodnota je statická vazba, což znamená, že jsou v procesu výběru použity typy výrazů v čase kompilace.</span><span class="sxs-lookup"><span data-stu-id="08560-180">When no dynamic expressions are involved, C# defaults to static binding, which means that the compile-time types of constituent expressions are used in the selection process.</span></span> <span data-ttu-id="08560-181">Pokud je však jeden ze základních výrazů v operacích uvedených výše dynamickým výrazem, operace je místo toho dynamicky svázána.</span><span class="sxs-lookup"><span data-stu-id="08560-181">However, when one of the constituent expressions in the operations listed above is a dynamic expression, the operation is instead dynamically bound.</span></span>

### <a name="binding-time"></a><span data-ttu-id="08560-182">Čas vazby</span><span class="sxs-lookup"><span data-stu-id="08560-182">Binding-time</span></span>

<span data-ttu-id="08560-183">Statická vazba probíhá v době kompilace, zatímco v době běhu probíhá dynamická vazba.</span><span class="sxs-lookup"><span data-stu-id="08560-183">Static binding takes place at compile-time, whereas dynamic binding takes place at run-time.</span></span> <span data-ttu-id="08560-184">V následujících částech pojem ***Doba vazby*** odkazuje buď na čas kompilace, nebo za běhu v závislosti na tom, kdy dojde k vazbě.</span><span class="sxs-lookup"><span data-stu-id="08560-184">In the following sections, the term ***binding-time*** refers to either compile-time or run-time, depending on when the binding takes place.</span></span>

<span data-ttu-id="08560-185">Následující příklad znázorňuje pojem statické a dynamické vazby a dobu vazby:</span><span class="sxs-lookup"><span data-stu-id="08560-185">The following example illustrates the notions of static and dynamic binding and of binding-time:</span></span>
```csharp
object  o = 5;
dynamic d = 5;

Console.WriteLine(5);  // static  binding to Console.WriteLine(int)
Console.WriteLine(o);  // static  binding to Console.WriteLine(object)
Console.WriteLine(d);  // dynamic binding to Console.WriteLine(int)
```

<span data-ttu-id="08560-186">První dvě volání jsou staticky svázána: přetížení `Console.WriteLine` je vybráno na základě typu při kompilaci jejich argumentu.</span><span class="sxs-lookup"><span data-stu-id="08560-186">The first two calls are statically bound: the overload of `Console.WriteLine` is picked based on the compile-time type of their argument.</span></span> <span data-ttu-id="08560-187">Proto je čas vytvoření vazby v čase kompilace.</span><span class="sxs-lookup"><span data-stu-id="08560-187">Thus, the binding-time is compile-time.</span></span>

<span data-ttu-id="08560-188">Třetí volání je dynamicky svázáno: přetížení `Console.WriteLine` je vybráno na základě běhového typu argumentu.</span><span class="sxs-lookup"><span data-stu-id="08560-188">The third call is dynamically bound: the overload of `Console.WriteLine` is picked based on the run-time type of its argument.</span></span> <span data-ttu-id="08560-189">K tomu dochází, protože argument je dynamický výraz – jeho typ při kompilaci je `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="08560-189">This happens because the argument is a dynamic expression -- its compile-time type is `dynamic`.</span></span> <span data-ttu-id="08560-190">Proto je čas vazby za třetí volání za běhu.</span><span class="sxs-lookup"><span data-stu-id="08560-190">Thus, the binding-time for the third call is run-time.</span></span>

### <a name="dynamic-binding"></a><span data-ttu-id="08560-191">Dynamická vazba</span><span class="sxs-lookup"><span data-stu-id="08560-191">Dynamic binding</span></span>

<span data-ttu-id="08560-192">Účelem dynamické vazby je povolit C# programům pracovat s ***dynamickými objekty***, tj. objekty, které nedodržují normální pravidla systému C# typů.</span><span class="sxs-lookup"><span data-stu-id="08560-192">The purpose of dynamic binding is to allow C# programs to interact with ***dynamic objects***, i.e. objects that do not follow the normal rules of the C# type system.</span></span> <span data-ttu-id="08560-193">Dynamické objekty mohou být objekty z jiných programovacích jazyků s různými systémy typů, nebo mohou být objekty, které jsou programově nastaveny k implementaci sémantiky vytváření vazeb pro různé operace.</span><span class="sxs-lookup"><span data-stu-id="08560-193">Dynamic objects may be objects from other programming languages with different types systems, or they may be objects that are programmatically setup to implement their own binding semantics for different operations.</span></span>

<span data-ttu-id="08560-194">Mechanismus, kterým dynamický objekt implementuje svou vlastní sémantiku je definována implementace.</span><span class="sxs-lookup"><span data-stu-id="08560-194">The mechanism by which a dynamic object implements its own semantics is implementation defined.</span></span> <span data-ttu-id="08560-195">Dané rozhraní zadané pro implementaci je znovu definováno – je implementováno dynamickými objekty k signalizaci za C# běhu, které mají speciální sémantiku.</span><span class="sxs-lookup"><span data-stu-id="08560-195">A given interface -- again implementation defined -- is implemented by dynamic objects to signal to the C# run-time that they have special semantics.</span></span> <span data-ttu-id="08560-196">Proto vždy, když jsou operace s dynamickým objektem dynamicky svázány, jejich vlastní sémantika vazby namísto těch C# , které jsou zadány v tomto dokumentu, převezmou.</span><span class="sxs-lookup"><span data-stu-id="08560-196">Thus, whenever operations on a dynamic object are dynamically bound, their own binding semantics, rather than those of C# as specified in this document, take over.</span></span>

<span data-ttu-id="08560-197">I když je účel dynamické vazby umožněna spolupráce s dynamickými objekty, C# umožňuje dynamickou vazbu na všech objektech, bez ohledu na to, zda jsou dynamické.</span><span class="sxs-lookup"><span data-stu-id="08560-197">While the purpose of dynamic binding is to allow interoperation with dynamic objects, C# allows dynamic binding on all objects, whether they are dynamic or not.</span></span> <span data-ttu-id="08560-198">To umožňuje plynulou integraci dynamických objektů, protože výsledky operací na nich nemusejí být dynamické objekty, ale v době kompilace jsou stále typu, který není známý pro programátory.</span><span class="sxs-lookup"><span data-stu-id="08560-198">This allows for a smoother integration of dynamic objects, as the results of operations on them may not themselves be dynamic objects, but are still of a type unknown to the programmer at compile-time.</span></span> <span data-ttu-id="08560-199">Dynamická vazba také může přispět k eliminaci kódu založeného na reflexi, i když žádné nesouvisející objekty nejsou dynamické objekty.</span><span class="sxs-lookup"><span data-stu-id="08560-199">Also dynamic binding can help eliminate error-prone reflection-based code even when no objects involved are dynamic objects.</span></span>

<span data-ttu-id="08560-200">Následující oddíly popisují jednotlivé konstrukce v jazyce přesně tehdy, když je aplikována dynamická vazba, která kontrola doby kompilace – Pokud je použita libovolná a jaké je výsledek kompilace a klasifikace výrazu.</span><span class="sxs-lookup"><span data-stu-id="08560-200">The following sections describe for each construct in the language exactly when dynamic binding is applied, what compile time checking -- if any -- is applied, and what the compile-time result and expression classification is.</span></span>

### <a name="types-of-constituent-expressions"></a><span data-ttu-id="08560-201">Typy výrazů prvků</span><span class="sxs-lookup"><span data-stu-id="08560-201">Types of constituent expressions</span></span>

<span data-ttu-id="08560-202">Je-li operace staticky svázána, typ výrazu elementu (například přijímač, argument, index nebo operand) je vždy považován za typ doby kompilace tohoto výrazu.</span><span class="sxs-lookup"><span data-stu-id="08560-202">When an operation is statically bound, the type of a constituent expression (e.g. a receiver, an argument, an index or an operand) is always considered to be the compile-time type of that expression.</span></span>

<span data-ttu-id="08560-203">Je-li operace dynamicky svázána, je typ výrazu elementu určován různými způsoby v závislosti na typu při kompilaci výrazu prvku:</span><span class="sxs-lookup"><span data-stu-id="08560-203">When an operation is dynamically bound, the type of a constituent expression is determined in different ways depending on the compile-time type of the constituent expression:</span></span>

*  <span data-ttu-id="08560-204">Výraz prvku typu kompilace `dynamic` je považován za typ skutečné hodnoty, ke které je výraz vyhodnocen za běhu.</span><span class="sxs-lookup"><span data-stu-id="08560-204">A constituent expression of compile-time type `dynamic` is considered to have the type of the actual value that the expression evaluates to at runtime</span></span>
*  <span data-ttu-id="08560-205">Výraz prvku, jehož typ kompilace je, se považuje za parametr typu, který má typ, ke kterému je vázán parametr typu za běhu.</span><span class="sxs-lookup"><span data-stu-id="08560-205">A constituent expression whose compile-time type is a type parameter is considered to have the type which the type parameter is bound to at runtime</span></span>
*  <span data-ttu-id="08560-206">V opačném případě je výraz prvku považován za jeho typ při kompilaci.</span><span class="sxs-lookup"><span data-stu-id="08560-206">Otherwise the constituent expression is considered to have its compile-time type.</span></span>

## <a name="operators"></a><span data-ttu-id="08560-207">Operátory</span><span class="sxs-lookup"><span data-stu-id="08560-207">Operators</span></span>

<span data-ttu-id="08560-208">Výrazy jsou vytvořené z ***operandů*** a ***operátorů***.</span><span class="sxs-lookup"><span data-stu-id="08560-208">Expressions are constructed from ***operands*** and ***operators***.</span></span> <span data-ttu-id="08560-209">Operátory výrazu označují, které operace se mají použít u operandů.</span><span class="sxs-lookup"><span data-stu-id="08560-209">The operators of an expression indicate which operations to apply to the operands.</span></span> <span data-ttu-id="08560-210">Příklady operátorů zahrnují `+`, `-`, `*`, `/`a `new`.</span><span class="sxs-lookup"><span data-stu-id="08560-210">Examples of operators include `+`, `-`, `*`, `/`, and `new`.</span></span> <span data-ttu-id="08560-211">Příklady operandů zahrnují literály, pole, místní proměnné a výrazy.</span><span class="sxs-lookup"><span data-stu-id="08560-211">Examples of operands include literals, fields, local variables, and expressions.</span></span>

<span data-ttu-id="08560-212">Existují tři typy operátorů:</span><span class="sxs-lookup"><span data-stu-id="08560-212">There are three kinds of operators:</span></span>

*  <span data-ttu-id="08560-213">Unární operátory.</span><span class="sxs-lookup"><span data-stu-id="08560-213">Unary operators.</span></span> <span data-ttu-id="08560-214">Unární operátory přebírají jeden operand a používají buď zápis předpony (například `--x`) nebo notaci přípony (například `x++`).</span><span class="sxs-lookup"><span data-stu-id="08560-214">The unary operators take one operand and use either prefix notation (such as `--x`) or postfix notation (such as `x++`).</span></span>
*  <span data-ttu-id="08560-215">Binární operátory.</span><span class="sxs-lookup"><span data-stu-id="08560-215">Binary operators.</span></span> <span data-ttu-id="08560-216">Binární operátory přebírají dva operandy a používají zápis vpony (například `x + y`).</span><span class="sxs-lookup"><span data-stu-id="08560-216">The binary operators take two operands and all use infix notation (such as `x + y`).</span></span>
*  <span data-ttu-id="08560-217">Ternární operátor</span><span class="sxs-lookup"><span data-stu-id="08560-217">Ternary operator.</span></span> <span data-ttu-id="08560-218">Existuje pouze jeden Ternární operátor, `?:`; využívá tři operandy a používá notaci vpony (`c ? x : y`).</span><span class="sxs-lookup"><span data-stu-id="08560-218">Only one ternary operator, `?:`, exists; it takes three operands and uses infix notation (`c ? x : y`).</span></span>

<span data-ttu-id="08560-219">Pořadí vyhodnocení operátorů ve výrazu je určeno ***prioritou*** a ***asociativita*** operátorů ([Priorita operátorů a asociativita](expressions.md#operator-precedence-and-associativity)).</span><span class="sxs-lookup"><span data-stu-id="08560-219">The order of evaluation of operators in an expression is determined by the ***precedence*** and ***associativity*** of the operators ([Operator precedence and associativity](expressions.md#operator-precedence-and-associativity)).</span></span>

<span data-ttu-id="08560-220">Operandy ve výrazu jsou vyhodnocovány zleva doprava.</span><span class="sxs-lookup"><span data-stu-id="08560-220">Operands in an expression are evaluated from left to right.</span></span> <span data-ttu-id="08560-221">Například v `F(i) + G(i++) * H(i)`metoda `F` je volána pomocí staré hodnoty `i`, pak metoda `G` je volána se starou hodnotou `i`a nakonec, metoda `H` je volána s novou hodnotou `i`.</span><span class="sxs-lookup"><span data-stu-id="08560-221">For example, in `F(i) + G(i++) * H(i)`, method `F` is called using the old value of `i`, then method `G` is called with the old value of `i`, and, finally, method `H` is called with the new value of `i`.</span></span> <span data-ttu-id="08560-222">To je oddělené od a s prioritou operátoru.</span><span class="sxs-lookup"><span data-stu-id="08560-222">This is separate from and unrelated to operator precedence.</span></span>

<span data-ttu-id="08560-223">Některé operátory mohou být ***přetíženy***.</span><span class="sxs-lookup"><span data-stu-id="08560-223">Certain operators can be ***overloaded***.</span></span> <span data-ttu-id="08560-224">Přetížení operátoru umožňuje zadat uživatelsky definované implementace operátorů pro operace, kde jeden nebo oba operandy jsou uživatelsky definované třídy nebo typu struktury ([Přetěžování operátorů](expressions.md#operator-overloading)).</span><span class="sxs-lookup"><span data-stu-id="08560-224">Operator overloading permits user-defined operator implementations to be specified for operations where one or both of the operands are of a user-defined class or struct type ([Operator overloading](expressions.md#operator-overloading)).</span></span>

### <a name="operator-precedence-and-associativity"></a><span data-ttu-id="08560-225">Priorita operátorů a asociativita</span><span class="sxs-lookup"><span data-stu-id="08560-225">Operator precedence and associativity</span></span>

<span data-ttu-id="08560-226">Pokud výraz obsahuje více operátorů, ***Priorita*** operátorů řídí pořadí, ve kterém jsou jednotlivé operátory vyhodnocovány.</span><span class="sxs-lookup"><span data-stu-id="08560-226">When an expression contains multiple operators, the ***precedence*** of the operators controls the order in which the individual operators are evaluated.</span></span> <span data-ttu-id="08560-227">Například výraz `x + y * z` je vyhodnocen jako `x + (y * z)`, protože operátor `*` má vyšší prioritu než binární `+` operátor.</span><span class="sxs-lookup"><span data-stu-id="08560-227">For example, the expression `x + y * z` is evaluated as `x + (y * z)` because the `*` operator has higher precedence than the binary `+` operator.</span></span> <span data-ttu-id="08560-228">Priorita operátoru je založena na definici své související provozní gramatiky.</span><span class="sxs-lookup"><span data-stu-id="08560-228">The precedence of an operator is established by the definition of its associated grammar production.</span></span> <span data-ttu-id="08560-229">Například *additive_expression* se skládá z sekvence *multiplicative_expression*s oddělenými operátory `+` nebo `-`, takže `+` a `-` operátory budou mít nižší prioritu než `*`, `/`a `%`.</span><span class="sxs-lookup"><span data-stu-id="08560-229">For example, an *additive_expression* consists of a sequence of *multiplicative_expression*s separated by `+` or `-` operators, thus giving the `+` and `-` operators lower precedence than the `*`, `/`, and `%` operators.</span></span>

<span data-ttu-id="08560-230">Následující tabulka shrnuje všechny operátory v pořadí podle priority od nejvyšších po nejnižší:</span><span class="sxs-lookup"><span data-stu-id="08560-230">The following table summarizes all operators in order of precedence from highest to lowest:</span></span>

| <span data-ttu-id="08560-231">__Sekce__</span><span class="sxs-lookup"><span data-stu-id="08560-231">__Section__</span></span>                                                                                   | <span data-ttu-id="08560-232">__Kategorie__</span><span class="sxs-lookup"><span data-stu-id="08560-232">__Category__</span></span>                | <span data-ttu-id="08560-233">__Operátory__</span><span class="sxs-lookup"><span data-stu-id="08560-233">__Operators__</span></span> | 
|-----------------------------------------------------------------------------------------------|-----------------------------|---------------|
| [<span data-ttu-id="08560-234">Primární výrazy</span><span class="sxs-lookup"><span data-stu-id="08560-234">Primary expressions</span></span>](expressions.md#primary-expressions)                                     | <span data-ttu-id="08560-235">Primární</span><span class="sxs-lookup"><span data-stu-id="08560-235">Primary</span></span>                     | <span data-ttu-id="08560-236">`x.y`  `f(x)`  `a[x]`  `x++`  `x--`  `new`  `typeof`  `default`  `checked`  `unchecked`  `delegate`</span><span class="sxs-lookup"><span data-stu-id="08560-236">`x.y`  `f(x)`  `a[x]`  `x++`  `x--`  `new`  `typeof`  `default`  `checked`  `unchecked`  `delegate`</span></span> | 
| [<span data-ttu-id="08560-237">Unární operátory</span><span class="sxs-lookup"><span data-stu-id="08560-237">Unary operators</span></span>](expressions.md#unary-operators)                                             | <span data-ttu-id="08560-238">Unární</span><span class="sxs-lookup"><span data-stu-id="08560-238">Unary</span></span>                       | <span data-ttu-id="08560-239">`+`  `-`  `!`  `~`  `++x`  `--x`  `(T)x`</span><span class="sxs-lookup"><span data-stu-id="08560-239">`+`  `-`  `!`  `~`  `++x`  `--x`  `(T)x`</span></span> | 
| [<span data-ttu-id="08560-240">Aritmetické operátory</span><span class="sxs-lookup"><span data-stu-id="08560-240">Arithmetic operators</span></span>](expressions.md#arithmetic-operators)                                   | <span data-ttu-id="08560-241">Multiplikativní</span><span class="sxs-lookup"><span data-stu-id="08560-241">Multiplicative</span></span>              | <span data-ttu-id="08560-242">`*`  `/`  `%`</span><span class="sxs-lookup"><span data-stu-id="08560-242">`*`  `/`  `%`</span></span> | 
| [<span data-ttu-id="08560-243">Aritmetické operátory</span><span class="sxs-lookup"><span data-stu-id="08560-243">Arithmetic operators</span></span>](expressions.md#arithmetic-operators)                                   | <span data-ttu-id="08560-244">Přičítáním</span><span class="sxs-lookup"><span data-stu-id="08560-244">Additive</span></span>                    | <span data-ttu-id="08560-245">`+`  `-`</span><span class="sxs-lookup"><span data-stu-id="08560-245">`+`  `-`</span></span>      | 
| [<span data-ttu-id="08560-246">Operátory posunutí</span><span class="sxs-lookup"><span data-stu-id="08560-246">Shift operators</span></span>](expressions.md#shift-operators)                                             | <span data-ttu-id="08560-247">SHIFT</span><span class="sxs-lookup"><span data-stu-id="08560-247">Shift</span></span>                       | <span data-ttu-id="08560-248">`<<`  `>>`</span><span class="sxs-lookup"><span data-stu-id="08560-248">`<<`  `>>`</span></span>    | 
| [<span data-ttu-id="08560-249">Relační operátory a operátory testování typů</span><span class="sxs-lookup"><span data-stu-id="08560-249">Relational and type-testing operators</span></span>](expressions.md#relational-and-type-testing-operators) | <span data-ttu-id="08560-250">Relační testování a testování typu</span><span class="sxs-lookup"><span data-stu-id="08560-250">Relational and type testing</span></span> | <span data-ttu-id="08560-251">`<`  `>`  `<=`  `>=`  `is`  `as`</span><span class="sxs-lookup"><span data-stu-id="08560-251">`<`  `>`  `<=`  `>=`  `is`  `as`</span></span> | 
| [<span data-ttu-id="08560-252">Relační operátory a operátory testování typů</span><span class="sxs-lookup"><span data-stu-id="08560-252">Relational and type-testing operators</span></span>](expressions.md#relational-and-type-testing-operators) | <span data-ttu-id="08560-253">Rovnost</span><span class="sxs-lookup"><span data-stu-id="08560-253">Equality</span></span>                    | <span data-ttu-id="08560-254">`==`  `!=`</span><span class="sxs-lookup"><span data-stu-id="08560-254">`==`  `!=`</span></span>    | 
| [<span data-ttu-id="08560-255">Logické operátory</span><span class="sxs-lookup"><span data-stu-id="08560-255">Logical operators</span></span>](expressions.md#logical-operators)                                         | <span data-ttu-id="08560-256">Logický operátor AND</span><span class="sxs-lookup"><span data-stu-id="08560-256">Logical AND</span></span>                 | `&`           | 
| [<span data-ttu-id="08560-257">Logické operátory</span><span class="sxs-lookup"><span data-stu-id="08560-257">Logical operators</span></span>](expressions.md#logical-operators)                                         | <span data-ttu-id="08560-258">Logický operátor XOR</span><span class="sxs-lookup"><span data-stu-id="08560-258">Logical XOR</span></span>                 | `^`           | 
| [<span data-ttu-id="08560-259">Logické operátory</span><span class="sxs-lookup"><span data-stu-id="08560-259">Logical operators</span></span>](expressions.md#logical-operators)                                         | <span data-ttu-id="08560-260">Logický operátor OR</span><span class="sxs-lookup"><span data-stu-id="08560-260">Logical OR</span></span>                  | <code>&#124;</code>           |
| [<span data-ttu-id="08560-261">Podmíněné logické operátory</span><span class="sxs-lookup"><span data-stu-id="08560-261">Conditional logical operators</span></span>](expressions.md#conditional-logical-operators)                 | <span data-ttu-id="08560-262">Podmiňovací operátor AND</span><span class="sxs-lookup"><span data-stu-id="08560-262">Conditional AND</span></span>             | `&&`          | 
| [<span data-ttu-id="08560-263">Podmíněné logické operátory</span><span class="sxs-lookup"><span data-stu-id="08560-263">Conditional logical operators</span></span>](expressions.md#conditional-logical-operators)                 | <span data-ttu-id="08560-264">Podmiňovací operátor OR</span><span class="sxs-lookup"><span data-stu-id="08560-264">Conditional OR</span></span>              | <code>&#124;&#124;</code>          | 
| [<span data-ttu-id="08560-265">Operátor slučování s hodnotou null</span><span class="sxs-lookup"><span data-stu-id="08560-265">The null coalescing operator</span></span>](expressions.md#the-null-coalescing-operator)                   | <span data-ttu-id="08560-266">Nulové sloučení</span><span class="sxs-lookup"><span data-stu-id="08560-266">Null coalescing</span></span>             | `??`          | 
| [<span data-ttu-id="08560-267">Podmíněný operátor</span><span class="sxs-lookup"><span data-stu-id="08560-267">Conditional operator</span></span>](expressions.md#conditional-operator)                                   | <span data-ttu-id="08560-268">Podmiňovací operátor</span><span class="sxs-lookup"><span data-stu-id="08560-268">Conditional</span></span>                 | `?:`          | 
| <span data-ttu-id="08560-269">[Operátory přiřazení](expressions.md#assignment-operators), [anonymní výrazy funkcí](expressions.md#anonymous-function-expressions)</span><span class="sxs-lookup"><span data-stu-id="08560-269">[Assignment operators](expressions.md#assignment-operators), [Anonymous function expressions](expressions.md#anonymous-function-expressions)</span></span>  | <span data-ttu-id="08560-270">Přiřazení a výraz lambda</span><span class="sxs-lookup"><span data-stu-id="08560-270">Assignment and lambda expression</span></span> | <span data-ttu-id="08560-271">`=`  `*=`  `/=`  `%=`  `+=`  `-=`  `<<=`  `>>=`  `&=`  `^=`  <code>&#124;=</code>  `=>`</span><span class="sxs-lookup"><span data-stu-id="08560-271">`=`  `*=`  `/=`  `%=`  `+=`  `-=`  `<<=`  `>>=`  `&=`  `^=`  <code>&#124;=</code>  `=>`</span></span> | 

<span data-ttu-id="08560-272">Když dojde k operandu mezi dvěma operátory se stejnou prioritou, asociativita operátor řídí pořadí, ve kterém jsou operace prováděny:</span><span class="sxs-lookup"><span data-stu-id="08560-272">When an operand occurs between two operators with the same precedence, the associativity of the operators controls the order in which the operations are performed:</span></span>

*  <span data-ttu-id="08560-273">S výjimkou operátorů přiřazení a slučovacího operátoru null jsou všechny binární operátory ***asociativní***, což znamená, že operace jsou prováděny zleva doprava.</span><span class="sxs-lookup"><span data-stu-id="08560-273">Except for the assignment operators and the null coalescing operator, all binary operators are ***left-associative***, meaning that operations are performed from left to right.</span></span> <span data-ttu-id="08560-274">Například `x + y + z` je vyhodnocen jako `(x + y) + z`.</span><span class="sxs-lookup"><span data-stu-id="08560-274">For example, `x + y + z` is evaluated as `(x + y) + z`.</span></span>
*  <span data-ttu-id="08560-275">Operátory přiřazení, operátor sloučení null a podmíněný operátor (`?:`) jsou ***asociativní zprava***, což znamená, že operace jsou prováděny zprava doleva.</span><span class="sxs-lookup"><span data-stu-id="08560-275">The assignment operators, the null coalescing operator and the conditional operator (`?:`) are ***right-associative***, meaning that operations are performed from right to left.</span></span> <span data-ttu-id="08560-276">Například `x = y = z` je vyhodnocen jako `x = (y = z)`.</span><span class="sxs-lookup"><span data-stu-id="08560-276">For example, `x = y = z` is evaluated as `x = (y = z)`.</span></span>

<span data-ttu-id="08560-277">Priority a asociativita lze ovládat pomocí závorek.</span><span class="sxs-lookup"><span data-stu-id="08560-277">Precedence and associativity can be controlled using parentheses.</span></span> <span data-ttu-id="08560-278">Například `x + y * z` nejprve vynásobí `y` `z` a pak výsledek přidá do `x`, ale `(x + y) * z` nejprve přidá `x` a `y` a pak výsledek vynásobí `z`.</span><span class="sxs-lookup"><span data-stu-id="08560-278">For example, `x + y * z` first multiplies `y` by `z` and then adds the result to `x`, but `(x + y) * z` first adds `x` and `y` and then multiplies the result by `z`.</span></span>

### <a name="operator-overloading"></a><span data-ttu-id="08560-279">Přetěžování operátoru</span><span class="sxs-lookup"><span data-stu-id="08560-279">Operator overloading</span></span>

<span data-ttu-id="08560-280">Všechny unární a binární operátory mají předdefinované implementace, které jsou automaticky k dispozici v jakémkoli výrazu.</span><span class="sxs-lookup"><span data-stu-id="08560-280">All unary and binary operators have predefined implementations that are automatically available in any expression.</span></span> <span data-ttu-id="08560-281">Kromě předdefinovaných implementací lze uživatelsky definované implementace začlenit zahrnutím `operator`ch deklarací do tříd a struktur ([Operators](classes.md#operators)).</span><span class="sxs-lookup"><span data-stu-id="08560-281">In addition to the predefined implementations, user-defined implementations can be introduced by including `operator` declarations in classes and structs ([Operators](classes.md#operators)).</span></span> <span data-ttu-id="08560-282">Uživatelsky definované implementace operátoru vždycky mají přednost před předdefinovanými implementacemi operátorů: jenom v případě, že neexistují žádné použitelné uživatelsky definované implementace operátoru, jak je popsáno v tématu [řešení přetížení unárních operátorů](expressions.md#unary-operator-overload-resolution) a [rozlišení přetížení binárního operátoru](expressions.md#binary-operator-overload-resolution).</span><span class="sxs-lookup"><span data-stu-id="08560-282">User-defined operator implementations always take precedence over predefined operator implementations: Only when no applicable user-defined operator implementations exist will the predefined operator implementations be considered, as described in [Unary operator overload resolution](expressions.md#unary-operator-overload-resolution) and [Binary operator overload resolution](expressions.md#binary-operator-overload-resolution).</span></span>

<span data-ttu-id="08560-283">***Přetížené unární operátory*** jsou:</span><span class="sxs-lookup"><span data-stu-id="08560-283">The ***overloadable unary operators*** are:</span></span>
```csharp
+   -   !   ~   ++   --   true   false
```

<span data-ttu-id="08560-284">I když `true` a `false` nejsou používány explicitně ve výrazech (a proto nejsou zahrnuty v tabulce priorit v [prioritě operátorů a asociativita](expressions.md#operator-precedence-and-associativity)), jsou považovány za operátory, protože jsou vyvolány v několika kontextech výrazů: logické výrazy ([logické výrazy](expressions.md#boolean-expressions)) a výrazy zahrnující podmíněný ([podmíněný operátor](expressions.md#conditional-operator)) a podmíněné logické operátory ([Podmíněné logické operátory](expressions.md#conditional-logical-operators)).</span><span class="sxs-lookup"><span data-stu-id="08560-284">Although `true` and `false` are not used explicitly in expressions (and therefore are not included in the precedence table in [Operator precedence and associativity](expressions.md#operator-precedence-and-associativity)), they are considered operators because they are invoked in several expression contexts: boolean expressions ([Boolean expressions](expressions.md#boolean-expressions)) and expressions involving the conditional ([Conditional operator](expressions.md#conditional-operator)), and conditional logical operators ([Conditional logical operators](expressions.md#conditional-logical-operators)).</span></span>

<span data-ttu-id="08560-285">***Přetížené binární operátory*** jsou:</span><span class="sxs-lookup"><span data-stu-id="08560-285">The ***overloadable binary operators*** are:</span></span>
```csharp
+   -   *   /   %   &   |   ^   <<   >>   ==   !=   >   <   >=   <=
```

<span data-ttu-id="08560-286">Pouze operátory uvedené výše mohou být přetíženy.</span><span class="sxs-lookup"><span data-stu-id="08560-286">Only the operators listed above can be overloaded.</span></span> <span data-ttu-id="08560-287">Konkrétně není možné přetížit přístup ke členům, vyvolání metod ani `=`, `&&`, `||`, `??`, `?:`, `=>`, `checked`, `unchecked`, `new`, `typeof`, `default`, `as`a `is`.</span><span class="sxs-lookup"><span data-stu-id="08560-287">In particular, it is not possible to overload member access, method invocation, or the `=`, `&&`, `||`, `??`, `?:`, `=>`, `checked`, `unchecked`, `new`, `typeof`, `default`, `as`, and `is` operators.</span></span>

<span data-ttu-id="08560-288">Při přetížení binárního operátoru je odpovídající operátor přiřazení také implicitně přetížený.</span><span class="sxs-lookup"><span data-stu-id="08560-288">When a binary operator is overloaded, the corresponding assignment operator, if any, is also implicitly overloaded.</span></span> <span data-ttu-id="08560-289">Například přetížení operátoru `*` je také přetížení operátoru `*=`.</span><span class="sxs-lookup"><span data-stu-id="08560-289">For example, an overload of operator `*` is also an overload of operator `*=`.</span></span> <span data-ttu-id="08560-290">Tento postup je podrobněji popsán v tématu [složené přiřazení](expressions.md#compound-assignment).</span><span class="sxs-lookup"><span data-stu-id="08560-290">This is described further in [Compound assignment](expressions.md#compound-assignment).</span></span> <span data-ttu-id="08560-291">Všimněte si, že samotný operátor přiřazení (`=`) nemůže být přetížený.</span><span class="sxs-lookup"><span data-stu-id="08560-291">Note that the assignment operator itself (`=`) cannot be overloaded.</span></span> <span data-ttu-id="08560-292">Přiřazení vždy provádí jednoduchou bitovou kopii hodnoty do proměnné.</span><span class="sxs-lookup"><span data-stu-id="08560-292">An assignment always performs a simple bit-wise copy of a value into a variable.</span></span>

<span data-ttu-id="08560-293">Operace přetypování, jako je například `(T)x`, jsou přetíženy poskytnutím uživatelsky definovaných převodů ([uživatelem definované převody](conversions.md#user-defined-conversions)).</span><span class="sxs-lookup"><span data-stu-id="08560-293">Cast operations, such as `(T)x`, are overloaded by providing user-defined conversions ([User-defined conversions](conversions.md#user-defined-conversions)).</span></span>

<span data-ttu-id="08560-294">Přístup k prvkům, jako je například `a[x]`, není považován za přetížený operátor.</span><span class="sxs-lookup"><span data-stu-id="08560-294">Element access, such as `a[x]`, is not considered an overloadable operator.</span></span> <span data-ttu-id="08560-295">Místo toho je uživatelsky definované indexování podporováno prostřednictvím indexerů ([indexerů](classes.md#indexers)).</span><span class="sxs-lookup"><span data-stu-id="08560-295">Instead, user-defined indexing is supported through indexers ([Indexers](classes.md#indexers)).</span></span>

<span data-ttu-id="08560-296">Ve výrazech jsou operátory odkazovány pomocí zápisu operátorů a v deklaracích jsou operátory odkazovány pomocí funkčního zápisu.</span><span class="sxs-lookup"><span data-stu-id="08560-296">In expressions, operators are referenced using operator notation, and in declarations, operators are referenced using functional notation.</span></span> <span data-ttu-id="08560-297">Následující tabulka ukazuje vztah mezi operátorem a funkčními zápisy pro unární a binární operátory.</span><span class="sxs-lookup"><span data-stu-id="08560-297">The following table shows the relationship between operator and functional notations for unary and binary operators.</span></span> <span data-ttu-id="08560-298">V první položce *op* označuje jakýkoli přetížený operátor unární předpony.</span><span class="sxs-lookup"><span data-stu-id="08560-298">In the first entry, *op* denotes any overloadable unary prefix operator.</span></span> <span data-ttu-id="08560-299">Ve druhé položce *op* označuje unární přípony `++` a `--` operátory.</span><span class="sxs-lookup"><span data-stu-id="08560-299">In the second entry, *op* denotes the unary postfix `++` and `--` operators.</span></span> <span data-ttu-id="08560-300">Třetí položka *op* označuje jakýkoli přetížený binární operátor.</span><span class="sxs-lookup"><span data-stu-id="08560-300">In the third entry, *op* denotes any overloadable binary operator.</span></span>


| <span data-ttu-id="08560-301">__Zápis operátoru__</span><span class="sxs-lookup"><span data-stu-id="08560-301">__Operator notation__</span></span> | <span data-ttu-id="08560-302">__Funkční zápis__</span><span class="sxs-lookup"><span data-stu-id="08560-302">__Functional notation__</span></span> |
|-----------------------|-------------------------|
| `op x`                | `operator op(x)`        | 
| `x op`                | `operator op(x)`        | 
| `x op y`              | `operator op(x,y)`      | 

<span data-ttu-id="08560-303">Deklarace operátoru definované uživatelem vždy vyžadují, aby alespoň jeden z parametrů byl typu třídy nebo struktury, který obsahuje deklaraci operátoru.</span><span class="sxs-lookup"><span data-stu-id="08560-303">User-defined operator declarations always require at least one of the parameters to be of the class or struct type that contains the operator declaration.</span></span> <span data-ttu-id="08560-304">Proto není možné, aby uživatelem definovaný operátor měl stejnou signaturu jako předdefinovaný operátor.</span><span class="sxs-lookup"><span data-stu-id="08560-304">Thus, it is not possible for a user-defined operator to have the same signature as a predefined operator.</span></span>

<span data-ttu-id="08560-305">Deklarace uživatelem definovaných operátorů nemůže měnit syntaxi, prioritu ani asociativita operátoru.</span><span class="sxs-lookup"><span data-stu-id="08560-305">User-defined operator declarations cannot modify the syntax, precedence, or associativity of an operator.</span></span> <span data-ttu-id="08560-306">Například operátor `/` je vždy binárním operátorem, vždy má úroveň priority specifikovanou v [prioritě operátorů a asociativita](expressions.md#operator-precedence-and-associativity)a je vždy asociativní zleva.</span><span class="sxs-lookup"><span data-stu-id="08560-306">For example, the `/` operator is always a binary operator, always has the precedence level specified in [Operator precedence and associativity](expressions.md#operator-precedence-and-associativity), and is always left-associative.</span></span>

<span data-ttu-id="08560-307">I když je možné, aby uživatelsky definovaný operátor prováděl jakékoli výpočty, implementace, které vytváří jiné výsledky než ty, které jsou intuitivní, se důrazně nedoporučuje.</span><span class="sxs-lookup"><span data-stu-id="08560-307">While it is possible for a user-defined operator to perform any computation it pleases, implementations that produce results other than those that are intuitively expected are strongly discouraged.</span></span> <span data-ttu-id="08560-308">Například implementace `operator ==` by měla porovnat dva operandy pro rovnost a vrátit odpovídající výsledek `bool`.</span><span class="sxs-lookup"><span data-stu-id="08560-308">For example, an implementation of `operator ==` should compare the two operands for equality and return an appropriate `bool` result.</span></span>

<span data-ttu-id="08560-309">Popisy jednotlivých operátorů v [primárních výrazech](expressions.md#primary-expressions) prostřednictvím [podmíněných logických operátorů](expressions.md#conditional-logical-operators) určují předdefinované implementace operátorů a všechna další pravidla, která platí pro každý operátor.</span><span class="sxs-lookup"><span data-stu-id="08560-309">The descriptions of individual operators in [Primary expressions](expressions.md#primary-expressions) through [Conditional logical operators](expressions.md#conditional-logical-operators) specify the predefined implementations of the operators and any additional rules that apply to each operator.</span></span> <span data-ttu-id="08560-310">Popisy využívají výrazy ***unárního přetížení operátoru***, ***rozlišení přetížení binárního operátoru***a ***číselnou povýšení***, definice, které se nacházejí v následujících částech.</span><span class="sxs-lookup"><span data-stu-id="08560-310">The descriptions make use of the terms ***unary operator overload resolution***, ***binary operator overload resolution***, and ***numeric promotion***, definitions of which are found in the following sections.</span></span>

### <a name="unary-operator-overload-resolution"></a><span data-ttu-id="08560-311">Unární rozlišení přetížení operátoru</span><span class="sxs-lookup"><span data-stu-id="08560-311">Unary operator overload resolution</span></span>

<span data-ttu-id="08560-312">Operace formuláře `op x` nebo `x op`, kde `op` je přetížený unární operátor a `x` je výraz typu `X`, je zpracován následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="08560-312">An operation of the form `op x` or `x op`, where `op` is an overloadable unary operator, and `x` is an expression of type `X`, is processed as follows:</span></span>

*  <span data-ttu-id="08560-313">Sada kandidátů definovaných uživatelem, které poskytuje `X` pro operaci `operator op(x)` je určena pomocí pravidel [kandidátů definovaných uživatelem](expressions.md#candidate-user-defined-operators).</span><span class="sxs-lookup"><span data-stu-id="08560-313">The set of candidate user-defined operators provided by `X` for the operation `operator op(x)` is determined using the rules of [Candidate user-defined operators](expressions.md#candidate-user-defined-operators).</span></span>
*  <span data-ttu-id="08560-314">Pokud sada kandidátů definovaných uživatelem není prázdná, pak se jedná o sadu kandidátických operátorů pro operaci.</span><span class="sxs-lookup"><span data-stu-id="08560-314">If the set of candidate user-defined operators is not empty, then this becomes the set of candidate operators for the operation.</span></span> <span data-ttu-id="08560-315">V opačném případě předdefinovaná unární `operator op` implementace, včetně jejich vypadlých formulářů, se stanou sadou kandidátů pro operaci.</span><span class="sxs-lookup"><span data-stu-id="08560-315">Otherwise, the predefined unary `operator op` implementations, including their lifted forms, become the set of candidate operators for the operation.</span></span> <span data-ttu-id="08560-316">Předdefinované implementace daného operátoru jsou zadány v popisu operátoru ([primární výrazy](expressions.md#primary-expressions) a [unární operátory](expressions.md#unary-operators)).</span><span class="sxs-lookup"><span data-stu-id="08560-316">The predefined implementations of a given operator are specified in the description of the operator ([Primary expressions](expressions.md#primary-expressions) and [Unary operators](expressions.md#unary-operators)).</span></span>
*  <span data-ttu-id="08560-317">Pravidla rozlišení přetížení [rozlišení přetěžování](expressions.md#overload-resolution) jsou aplikována na sadu kandidátních operátorů pro výběr nejlepšího operátoru s ohledem na seznam argumentů `(x)`a tento operátor se projeví v důsledku procesu rozlišení přetížení.</span><span class="sxs-lookup"><span data-stu-id="08560-317">The overload resolution rules of [Overload resolution](expressions.md#overload-resolution) are applied to the set of candidate operators to select the best operator with respect to the argument list `(x)`, and this operator becomes the result of the overload resolution process.</span></span> <span data-ttu-id="08560-318">Pokud Rozlišení přetěžování nedokáže vybrat jeden nejlepší operátor, dojde k chybě při vazbě.</span><span class="sxs-lookup"><span data-stu-id="08560-318">If overload resolution fails to select a single best operator, a binding-time error occurs.</span></span>

### <a name="binary-operator-overload-resolution"></a><span data-ttu-id="08560-319">Rozlišení přetížení binárního operátoru</span><span class="sxs-lookup"><span data-stu-id="08560-319">Binary operator overload resolution</span></span>

<span data-ttu-id="08560-320">Operace formuláře `x op y`, kde `op` je přetížený binární operátor, `x` je výraz typu `X`a `y` je výraz typu `Y`, je zpracován následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="08560-320">An operation of the form `x op y`, where `op` is an overloadable binary operator, `x` is an expression of type `X`, and `y` is an expression of type `Y`, is processed as follows:</span></span>

*  <span data-ttu-id="08560-321">Je určena sada kandidátů definovaných uživatelem, které jsou poskytovány `X` a `Y` pro `operator op(x,y)` operace.</span><span class="sxs-lookup"><span data-stu-id="08560-321">The set of candidate user-defined operators provided by `X` and `Y` for the operation `operator op(x,y)` is determined.</span></span> <span data-ttu-id="08560-322">Sada se skládá z sjednocení operátorů kandidátů poskytovaných `X` a kandidátních operátorů poskytovaných `Y`, z nichž každá je určena pomocí pravidel [operátorů kandidáta definovaných uživatelem](expressions.md#candidate-user-defined-operators).</span><span class="sxs-lookup"><span data-stu-id="08560-322">The set consists of the union of the candidate operators provided by `X` and the candidate operators provided by `Y`, each determined using the rules of [Candidate user-defined operators](expressions.md#candidate-user-defined-operators).</span></span> <span data-ttu-id="08560-323">Pokud jsou `X` a `Y` stejného typu nebo pokud `X` a `Y` jsou odvozeny ze společného základního typu, pak jsou sdílené kandidáty k dis pouze v kombinované sadě.</span><span class="sxs-lookup"><span data-stu-id="08560-323">If `X` and `Y` are the same type, or if `X` and `Y` are derived from a common base type, then shared candidate operators only occur in the combined set once.</span></span>
*  <span data-ttu-id="08560-324">Pokud sada kandidátů definovaných uživatelem není prázdná, pak se jedná o sadu kandidátických operátorů pro operaci.</span><span class="sxs-lookup"><span data-stu-id="08560-324">If the set of candidate user-defined operators is not empty, then this becomes the set of candidate operators for the operation.</span></span> <span data-ttu-id="08560-325">Jinak se předdefinované binární `operator op` implementace, včetně jejich vypadlých formulářů, stanou sadou kandidátů pro operaci.</span><span class="sxs-lookup"><span data-stu-id="08560-325">Otherwise, the predefined binary `operator op` implementations, including their lifted forms,  become the set of candidate operators for the operation.</span></span> <span data-ttu-id="08560-326">Předdefinované implementace daného operátoru jsou zadány v popisu operátoru ([aritmetické operátory](expressions.md#arithmetic-operators) prostřednictvím [podmíněných logických operátorů](expressions.md#conditional-logical-operators)).</span><span class="sxs-lookup"><span data-stu-id="08560-326">The predefined implementations of a given operator are specified in the description of the operator ([Arithmetic operators](expressions.md#arithmetic-operators) through [Conditional logical operators](expressions.md#conditional-logical-operators)).</span></span> <span data-ttu-id="08560-327">U předdefinovaných operátorů výčtu a delegátů jsou považovány za ty, které jsou definovány výčtovým typem nebo typem delegáta, který je typem pro dobu vazby jednoho z operandů.</span><span class="sxs-lookup"><span data-stu-id="08560-327">For predefined enum and delegate operators, the only operators considered are those defined by an enum or delegate type that is the binding-time type of one of the operands.</span></span>
*  <span data-ttu-id="08560-328">Pravidla rozlišení přetížení [rozlišení přetěžování](expressions.md#overload-resolution) jsou aplikována na sadu kandidátních operátorů pro výběr nejlepšího operátoru s ohledem na seznam argumentů `(x,y)`a tento operátor se projeví v důsledku procesu rozlišení přetížení.</span><span class="sxs-lookup"><span data-stu-id="08560-328">The overload resolution rules of [Overload resolution](expressions.md#overload-resolution) are applied to the set of candidate operators to select the best operator with respect to the argument list `(x,y)`, and this operator becomes the result of the overload resolution process.</span></span> <span data-ttu-id="08560-329">Pokud Rozlišení přetěžování nedokáže vybrat jeden nejlepší operátor, dojde k chybě při vazbě.</span><span class="sxs-lookup"><span data-stu-id="08560-329">If overload resolution fails to select a single best operator, a binding-time error occurs.</span></span>

### <a name="candidate-user-defined-operators"></a><span data-ttu-id="08560-330">Kandidátské operátory definované uživatelem</span><span class="sxs-lookup"><span data-stu-id="08560-330">Candidate user-defined operators</span></span>

<span data-ttu-id="08560-331">Vzhledem k typu `T` a operaci `operator op(A)`, kde `op` je přetížený operátor a `A` je seznam argumentů, sada kandidátů definovaných uživatelem, kterou poskytují `T` pro `operator op(A)`, je určena následovně:</span><span class="sxs-lookup"><span data-stu-id="08560-331">Given a type `T` and an operation `operator op(A)`, where `op` is an overloadable operator and `A` is an argument list, the set of candidate user-defined operators provided by `T` for `operator op(A)` is determined as follows:</span></span>

*  <span data-ttu-id="08560-332">Určete typ `T0`.</span><span class="sxs-lookup"><span data-stu-id="08560-332">Determine the type `T0`.</span></span> <span data-ttu-id="08560-333">Pokud `T` je typ s možnou hodnotou null, `T0` je jeho nadřízený typ, jinak `T0` se rovná `T`.</span><span class="sxs-lookup"><span data-stu-id="08560-333">If `T` is a nullable type, `T0` is its underlying type, otherwise `T0` is equal to `T`.</span></span>
*  <span data-ttu-id="08560-334">U všech deklarací `operator op` v `T0` a všech vydaných forem takových operátorů, pokud je k dispozici alespoň jeden operátor ([příslušný člen funkce](expressions.md#applicable-function-member)) s ohledem na `A`seznamu argumentů, pak sada kandidátních operátorů sestává ze všech takových použitelných operátorů v `T0`.</span><span class="sxs-lookup"><span data-stu-id="08560-334">For all `operator op` declarations in `T0` and all lifted forms of such operators, if at least one operator is applicable ([Applicable function member](expressions.md#applicable-function-member)) with respect to the argument list `A`, then the set of candidate operators consists of all such applicable operators in `T0`.</span></span>
*  <span data-ttu-id="08560-335">V opačném případě, pokud je `T0` `object`, sada kandidát Operators je prázdná.</span><span class="sxs-lookup"><span data-stu-id="08560-335">Otherwise, if `T0` is `object`, the set of candidate operators is empty.</span></span>
*  <span data-ttu-id="08560-336">V opačném případě je množina kandidátních operátorů poskytovaných `T0` sadou kandidátů, kterou poskytuje přímá základní třída `T0`, nebo efektivní základní třídy `T0`, pokud `T0` je parametr typu.</span><span class="sxs-lookup"><span data-stu-id="08560-336">Otherwise, the set of candidate operators provided by `T0` is the set of candidate operators provided by the direct base class of `T0`, or the effective base class of `T0` if `T0` is a type parameter.</span></span>

### <a name="numeric-promotions"></a><span data-ttu-id="08560-337">Číselné propagační akce</span><span class="sxs-lookup"><span data-stu-id="08560-337">Numeric promotions</span></span>

<span data-ttu-id="08560-338">Číselná propagace se skládá z automatického provádění určitých implicitních převodů operandů předdefinovaných unárních a binárních číselných operátorů.</span><span class="sxs-lookup"><span data-stu-id="08560-338">Numeric promotion consists of automatically performing certain implicit conversions of the operands of the predefined unary and binary numeric operators.</span></span> <span data-ttu-id="08560-339">Číselná propagace není odlišným mechanismem, ale místo toho, aby se použilo Rozlišení přetěžování na předdefinované operátory.</span><span class="sxs-lookup"><span data-stu-id="08560-339">Numeric promotion is not a distinct mechanism, but rather an effect of applying overload resolution to the predefined operators.</span></span> <span data-ttu-id="08560-340">Číselná propagace konkrétně neovlivňuje hodnocení uživatelsky definovaných operátorů, i když uživatelsky definované operátory mohou být implementovány tak, aby se projevily podobné účinky.</span><span class="sxs-lookup"><span data-stu-id="08560-340">Numeric promotion specifically does not affect evaluation of user-defined operators, although user-defined operators can be implemented to exhibit similar effects.</span></span>

<span data-ttu-id="08560-341">Jako příklad číselné povýšení zvažte předdefinované implementace binárního operátoru `*`:</span><span class="sxs-lookup"><span data-stu-id="08560-341">As an example of numeric promotion, consider the predefined implementations of the binary `*` operator:</span></span>

```csharp
int operator *(int x, int y);
uint operator *(uint x, uint y);
long operator *(long x, long y);
ulong operator *(ulong x, ulong y);
float operator *(float x, float y);
double operator *(double x, double y);
decimal operator *(decimal x, decimal y);
```

<span data-ttu-id="08560-342">Když se pro tuto sadu operátorů aplikují pravidla rozlišení přetížení ([rozlišení přetížení](expressions.md#overload-resolution)), efekt je vybrat první z operátorů, pro které existují implicitní převody z typů operandů.</span><span class="sxs-lookup"><span data-stu-id="08560-342">When overload resolution rules ([Overload resolution](expressions.md#overload-resolution)) are applied to this set of operators, the effect is to select the first of the operators for which implicit conversions exist from the operand types.</span></span> <span data-ttu-id="08560-343">Například pro operaci `b * s`, kde `b` je `byte` a `s` je `short`, řešení přetížení vybere `operator *(int,int)` jako nejlepší operátor.</span><span class="sxs-lookup"><span data-stu-id="08560-343">For example, for the operation `b * s`, where `b` is a `byte` and `s` is a `short`, overload resolution selects `operator *(int,int)` as the best operator.</span></span> <span data-ttu-id="08560-344">Proto je efekt `b` a `s` převede na `int`a typ výsledku je `int`.</span><span class="sxs-lookup"><span data-stu-id="08560-344">Thus, the effect is that `b` and `s` are converted to `int`, and the type of the result is `int`.</span></span> <span data-ttu-id="08560-345">Stejně tak pro `i * d`operací, kde `i` je `int` a `d` je `double`, řešení přetížení vybere `operator *(double,double)` jako nejlepší operátor.</span><span class="sxs-lookup"><span data-stu-id="08560-345">Likewise, for the operation `i * d`, where `i` is an `int` and `d` is a `double`, overload resolution selects `operator *(double,double)` as the best operator.</span></span>

#### <a name="unary-numeric-promotions"></a><span data-ttu-id="08560-346">Unární číselné propagační akce</span><span class="sxs-lookup"><span data-stu-id="08560-346">Unary numeric promotions</span></span>

<span data-ttu-id="08560-347">Unární číselná propagace nastane u operandů předdefinovaných `+`, `-`a `~` unárních operátorů.</span><span class="sxs-lookup"><span data-stu-id="08560-347">Unary numeric promotion occurs for the operands of the predefined `+`, `-`, and `~` unary operators.</span></span> <span data-ttu-id="08560-348">Unární číselné povýšení se jednoduše skládá z převodu operandů typu `sbyte`, `byte`, `short`, `ushort`nebo `char` na typ `int`.</span><span class="sxs-lookup"><span data-stu-id="08560-348">Unary numeric promotion simply consists of converting operands of type `sbyte`, `byte`, `short`, `ushort`, or `char` to type `int`.</span></span> <span data-ttu-id="08560-349">Kromě toho pro unární operátor `-`, unární číselná propagace, převede operandy typu `uint` na typ `long`.</span><span class="sxs-lookup"><span data-stu-id="08560-349">Additionally, for the unary `-` operator, unary numeric promotion converts operands of type `uint` to type `long`.</span></span>

#### <a name="binary-numeric-promotions"></a><span data-ttu-id="08560-350">Binární číselné propagační akce</span><span class="sxs-lookup"><span data-stu-id="08560-350">Binary numeric promotions</span></span>

<span data-ttu-id="08560-351">Pro operandy předdefinovaných `+`, `-`, `*`, `/`, `%`, `&`, `|`, `^`, `==`, `!=`, `>`, `<`, `>=`a `<=` binárních operátorů dojde k binárnímu číslu povýšení.</span><span class="sxs-lookup"><span data-stu-id="08560-351">Binary numeric promotion occurs for the operands of the predefined `+`, `-`, `*`, `/`, `%`, `&`, `|`, `^`, `==`, `!=`, `>`, `<`, `>=`, and `<=` binary operators.</span></span> <span data-ttu-id="08560-352">Binární číselná propagace implicitně převede oba operandy na společný typ, který, v případě nerelačních operátorů, se také změní na výsledný typ operace.</span><span class="sxs-lookup"><span data-stu-id="08560-352">Binary numeric promotion implicitly converts both operands to a common type which, in case of the non-relational operators, also becomes the result type of the operation.</span></span> <span data-ttu-id="08560-353">Binární číselná propagace se skládá z použití následujících pravidel v pořadí, v jakém se zobrazují:</span><span class="sxs-lookup"><span data-stu-id="08560-353">Binary numeric promotion consists of applying the following rules, in the order they appear here:</span></span>

*  <span data-ttu-id="08560-354">Pokud je jeden operand typu `decimal`, je druhý operand převeden na typ `decimal`nebo dojde k chybě při vazbě, pokud je druhý operand typu `float` nebo `double`.</span><span class="sxs-lookup"><span data-stu-id="08560-354">If either operand is of type `decimal`, the other operand is converted to type `decimal`, or a binding-time error occurs if the other operand is of type `float` or `double`.</span></span>
*  <span data-ttu-id="08560-355">V opačném případě, pokud je jeden operand typu `double`, je druhý operand převeden na typ `double`.</span><span class="sxs-lookup"><span data-stu-id="08560-355">Otherwise, if either operand is of type `double`, the other operand is converted to type `double`.</span></span>
*  <span data-ttu-id="08560-356">V opačném případě, pokud je jeden operand typu `float`, je druhý operand převeden na typ `float`.</span><span class="sxs-lookup"><span data-stu-id="08560-356">Otherwise, if either operand is of type `float`, the other operand is converted to type `float`.</span></span>
*  <span data-ttu-id="08560-357">V opačném případě, pokud je jeden operand typu `ulong`, je druhý operand převeden na typ `ulong`nebo dojde k chybě při vazbě v případě, že druhý operand je typu `sbyte`, `short`, `int`nebo `long`.</span><span class="sxs-lookup"><span data-stu-id="08560-357">Otherwise, if either operand is of type `ulong`, the other operand is converted to type `ulong`, or a binding-time error occurs if the other operand is of type `sbyte`, `short`, `int`, or `long`.</span></span>
*  <span data-ttu-id="08560-358">V opačném případě, pokud je jeden operand typu `long`, je druhý operand převeden na typ `long`.</span><span class="sxs-lookup"><span data-stu-id="08560-358">Otherwise, if either operand is of type `long`, the other operand is converted to type `long`.</span></span>
*  <span data-ttu-id="08560-359">V opačném případě, pokud je jeden operand typu `uint` a druhý operand je typu `sbyte`, `short`nebo `int`, jsou oba operandy převedeny na typ `long`.</span><span class="sxs-lookup"><span data-stu-id="08560-359">Otherwise, if either operand is of type `uint` and the other operand is of type `sbyte`, `short`, or `int`, both operands are converted to type `long`.</span></span>
*  <span data-ttu-id="08560-360">V opačném případě, pokud je jeden operand typu `uint`, je druhý operand převeden na typ `uint`.</span><span class="sxs-lookup"><span data-stu-id="08560-360">Otherwise, if either operand is of type `uint`, the other operand is converted to type `uint`.</span></span>
*  <span data-ttu-id="08560-361">V opačném případě jsou oba operandy převedeny na typ `int`.</span><span class="sxs-lookup"><span data-stu-id="08560-361">Otherwise, both operands are converted to type `int`.</span></span>

<span data-ttu-id="08560-362">Všimněte si, že první pravidlo zakáže všechny operace, které budou kombinovat typ `decimal` s `double` a typy `float`.</span><span class="sxs-lookup"><span data-stu-id="08560-362">Note that the first rule disallows any operations that mix the `decimal` type with the `double` and `float` types.</span></span> <span data-ttu-id="08560-363">Pravidlo následuje ze skutečnosti, že mezi typem `decimal` a typy `double` a `float` nejsou žádné implicitní převody.</span><span class="sxs-lookup"><span data-stu-id="08560-363">The rule follows from the fact that there are no implicit conversions between the `decimal` type and the `double` and `float` types.</span></span>

<span data-ttu-id="08560-364">Všimněte si také, že není možné, aby operand byl typu `ulong`, když je druhý operand typu se znaménkem integrálu.</span><span class="sxs-lookup"><span data-stu-id="08560-364">Also note that it is not possible for an operand to be of type `ulong` when the other operand is of a signed integral type.</span></span> <span data-ttu-id="08560-365">Důvodem je, že neexistuje žádný integrální typ, který může představovat celý rozsah `ulong` a také celočíselné typy se znaménkem.</span><span class="sxs-lookup"><span data-stu-id="08560-365">The reason is that no integral type exists that can represent the full range of `ulong` as well as the signed integral types.</span></span>

<span data-ttu-id="08560-366">V obou výše uvedených případech lze výraz přetypování použít k explicitnímu převodu jednoho operandu na typ, který je kompatibilní s druhým operandem.</span><span class="sxs-lookup"><span data-stu-id="08560-366">In both of the above cases, a cast expression can be used to explicitly convert one operand to a type that is compatible with the other operand.</span></span>

<span data-ttu-id="08560-367">V příkladu</span><span class="sxs-lookup"><span data-stu-id="08560-367">In the example</span></span>
```csharp
decimal AddPercent(decimal x, double percent) {
    return x * (1.0 + percent / 100.0);
}
```
<span data-ttu-id="08560-368">dojde k chybě při vazbě, protože `decimal` nelze vynásobit `double`.</span><span class="sxs-lookup"><span data-stu-id="08560-368">a binding-time error occurs because a `decimal` cannot be multiplied by a `double`.</span></span> <span data-ttu-id="08560-369">Chybu lze vyřešit explicitním převodem druhého operandu na `decimal`, a to následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="08560-369">The error is resolved by explicitly converting the second operand to `decimal`, as follows:</span></span>

```csharp
decimal AddPercent(decimal x, double percent) {
    return x * (decimal)(1.0 + percent / 100.0);
}
```

### <a name="lifted-operators"></a><span data-ttu-id="08560-370">Zrušené operátory</span><span class="sxs-lookup"><span data-stu-id="08560-370">Lifted operators</span></span>

<span data-ttu-id="08560-371">Přenesené ***operátory*** povolují předdefinované a uživatelsky definované operátory, které pracují s typy hodnot, které neumožňují hodnotu null, pro použití s připouštějící formuláře těchto typů.</span><span class="sxs-lookup"><span data-stu-id="08560-371">***Lifted operators*** permit predefined and user-defined operators that operate on non-nullable value types to also be used with nullable forms of those types.</span></span> <span data-ttu-id="08560-372">Přenesené operátory jsou vyrobeny z předdefinovaných a uživatelem definovaných operátorů, které splňují určité požadavky, jak je popsáno v následujícím tématu:</span><span class="sxs-lookup"><span data-stu-id="08560-372">Lifted operators are constructed from predefined and user-defined operators that meet certain requirements, as described in the following:</span></span>

*   <span data-ttu-id="08560-373">Pro unární operátory</span><span class="sxs-lookup"><span data-stu-id="08560-373">For the unary operators</span></span>

    ```csharp
    +  ++  -  --  !  ~
    ```

    <span data-ttu-id="08560-374">Vyzdvižený tvar operátoru existuje, pokud operandy a typy výsledků jsou typy hodnot bez hodnoty null.</span><span class="sxs-lookup"><span data-stu-id="08560-374">a lifted form of an operator exists if the operand and result types are both non-nullable value types.</span></span> <span data-ttu-id="08560-375">Vyzdvižený formulář je vytvořen přidáním jednoho `?` modifikátoru k operandům a typům výsledků.</span><span class="sxs-lookup"><span data-stu-id="08560-375">The lifted form is constructed by adding a single `?` modifier to the operand and result types.</span></span> <span data-ttu-id="08560-376">Vyzdvižený operátor vytvoří hodnotu null, pokud je operand null.</span><span class="sxs-lookup"><span data-stu-id="08560-376">The lifted operator produces a null value if the operand is null.</span></span> <span data-ttu-id="08560-377">V opačném případě převedený operátor zruší zalomení operandu, použije základní operátor a zabalí výsledek.</span><span class="sxs-lookup"><span data-stu-id="08560-377">Otherwise, the lifted operator unwraps the operand, applies the underlying operator, and wraps the result.</span></span>

*   <span data-ttu-id="08560-378">Pro binární operátory</span><span class="sxs-lookup"><span data-stu-id="08560-378">For the binary operators</span></span>

    ```csharp
    +  -  *  /  %  &  |  ^  <<  >>
    ```

    <span data-ttu-id="08560-379">Vyzdvižený tvar operátoru existuje, pokud jsou typy operandů a výsledků všechny typy hodnot bez hodnoty null.</span><span class="sxs-lookup"><span data-stu-id="08560-379">a lifted form of an operator exists if the operand and result types are all non-nullable value types.</span></span> <span data-ttu-id="08560-380">Vyzdvižený formulář je vytvořen přidáním jednoho `?` modifikátoru na každý operand a typ výsledku.</span><span class="sxs-lookup"><span data-stu-id="08560-380">The lifted form is constructed by adding a single `?` modifier to each operand and result type.</span></span> <span data-ttu-id="08560-381">Převedený operátor vytvoří hodnotu null, pokud má jeden nebo oba operandy hodnotu null (výjimku tvoří `&` a `|` operátory `bool?`ho typu, jak je popsáno v tématu [logické logické operátory](expressions.md#boolean-logical-operators)).</span><span class="sxs-lookup"><span data-stu-id="08560-381">The lifted operator produces a null value if one or both operands are null (an exception being the `&` and `|` operators of the `bool?` type, as described in [Boolean logical operators](expressions.md#boolean-logical-operators)).</span></span> <span data-ttu-id="08560-382">V opačném případě Vyzdvižený operátor rozbalí operandy, použije základní operátor a zabalí výsledek.</span><span class="sxs-lookup"><span data-stu-id="08560-382">Otherwise, the lifted operator unwraps the operands, applies the underlying operator, and wraps the result.</span></span>

*   <span data-ttu-id="08560-383">Pro operátory rovnosti</span><span class="sxs-lookup"><span data-stu-id="08560-383">For the equality operators</span></span>

    ```csharp
    ==  !=
    ```

    <span data-ttu-id="08560-384">Vyzdvižený tvar operátoru existuje, pokud typy operandů jsou typy hodnot bez hodnoty null a je-li typ výsledku `bool`.</span><span class="sxs-lookup"><span data-stu-id="08560-384">a lifted form of an operator exists if the operand types are both non-nullable value types and if the result type is `bool`.</span></span> <span data-ttu-id="08560-385">Vyzdvižený formulář je vytvořen přidáním jednoho `?` modifikátoru ke každému typu operandu.</span><span class="sxs-lookup"><span data-stu-id="08560-385">The lifted form is constructed by adding a single `?` modifier to each operand type.</span></span> <span data-ttu-id="08560-386">Předaný operátor považuje dvě hodnoty null za stejné a hodnota null se nerovná žádné hodnotě, která není null.</span><span class="sxs-lookup"><span data-stu-id="08560-386">The lifted operator considers two null values equal, and a null value unequal to any non-null value.</span></span> <span data-ttu-id="08560-387">Pokud jsou oba operandy nenulové, Vyzdvižený operátor rozbalí operandy a aplikuje příslušný operátor, aby vytvořil výsledek `bool`.</span><span class="sxs-lookup"><span data-stu-id="08560-387">If both operands are non-null, the lifted operator unwraps the operands and applies the underlying operator to produce the `bool` result.</span></span>

*   <span data-ttu-id="08560-388">Pro relační operátory</span><span class="sxs-lookup"><span data-stu-id="08560-388">For the relational operators</span></span>

    ```csharp
    <  >  <=  >=
    ```

    <span data-ttu-id="08560-389">Vyzdvižený tvar operátoru existuje, pokud typy operandů jsou typy hodnot bez hodnoty null a je-li typ výsledku `bool`.</span><span class="sxs-lookup"><span data-stu-id="08560-389">a lifted form of an operator exists if the operand types are both non-nullable value types and if the result type is `bool`.</span></span> <span data-ttu-id="08560-390">Vyzdvižený formulář je vytvořen přidáním jednoho `?` modifikátoru ke každému typu operandu.</span><span class="sxs-lookup"><span data-stu-id="08560-390">The lifted form is constructed by adding a single `?` modifier to each operand type.</span></span> <span data-ttu-id="08560-391">Převedený operátor vytvoří hodnotu `false`, pokud jeden nebo oba operandy mají hodnotu null.</span><span class="sxs-lookup"><span data-stu-id="08560-391">The lifted operator produces the value `false` if one or both operands are null.</span></span> <span data-ttu-id="08560-392">V opačném případě převedený operátor rozbalí operandy a aplikuje příslušný operátor, aby vytvořil výsledek `bool`.</span><span class="sxs-lookup"><span data-stu-id="08560-392">Otherwise, the lifted operator unwraps the operands and applies the underlying operator to produce the `bool` result.</span></span>

## <a name="member-lookup"></a><span data-ttu-id="08560-393">Vyhledávání členů</span><span class="sxs-lookup"><span data-stu-id="08560-393">Member lookup</span></span>

<span data-ttu-id="08560-394">Vyhledávání členů je proces, při kterém je určen význam názvu v kontextu typu.</span><span class="sxs-lookup"><span data-stu-id="08560-394">A member lookup is the process whereby the meaning of a name in the context of a type is determined.</span></span> <span data-ttu-id="08560-395">Vyhledávání členů může nastat v rámci vyhodnocení *simple_name* ([jednoduchých názvů](expressions.md#simple-names)) nebo *member_access* ([přístupu členů](expressions.md#member-access)) ve výrazu.</span><span class="sxs-lookup"><span data-stu-id="08560-395">A member lookup can occur as part of evaluating a *simple_name* ([Simple names](expressions.md#simple-names)) or a *member_access* ([Member access](expressions.md#member-access)) in an expression.</span></span> <span data-ttu-id="08560-396">Pokud se *simple_name* nebo *member_access* vyskytne jako *primary_expression* *invocation_expression* ([vyvolání metod](expressions.md#method-invocations)), člen je označován jako vyvolán.</span><span class="sxs-lookup"><span data-stu-id="08560-396">If the *simple_name* or *member_access* occurs as the *primary_expression* of an *invocation_expression* ([Method invocations](expressions.md#method-invocations)), the member is said to be invoked.</span></span>

<span data-ttu-id="08560-397">Pokud je členem metoda nebo událost, nebo pokud se jedná o konstantu, pole nebo vlastnost buď typu delegáta ([Delegáti](delegates.md)), nebo typu `dynamic` ([dynamický typ](types.md#the-dynamic-type)), pak je člen označován jako *nevyvolatelný*.</span><span class="sxs-lookup"><span data-stu-id="08560-397">If a member is a method or event, or if it is a constant, field or property of either a delegate type ([Delegates](delegates.md)) or the type `dynamic` ([The dynamic type](types.md#the-dynamic-type)), then the member is said to be *invocable*.</span></span>

<span data-ttu-id="08560-398">Vyhledávání členů nebere v úvahu nejen název členu, ale také počet parametrů typu, které má člen a zda je člen přístupný.</span><span class="sxs-lookup"><span data-stu-id="08560-398">Member lookup considers not only the name of a member but also the number of type parameters the member has and whether the member is accessible.</span></span> <span data-ttu-id="08560-399">Pro účely vyhledávání členů mají obecné metody a vnořené obecné typy počet parametrů typu uvedených v příslušných deklaracích a všichni ostatní členové mají nulové parametry typu.</span><span class="sxs-lookup"><span data-stu-id="08560-399">For the purposes of member lookup, generic methods and nested generic types have the number of type parameters indicated in their respective declarations and all other members have zero type parameters.</span></span>

<span data-ttu-id="08560-400">Vyhledávání členů názvu `N` s `K` parametry typu v `T` typu je zpracováno následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="08560-400">A member lookup of a name `N` with `K` type parameters in a type `T` is processed as follows:</span></span>

*  <span data-ttu-id="08560-401">Nejprve je určena sada přístupných členů s názvem `N`:</span><span class="sxs-lookup"><span data-stu-id="08560-401">First, a set of accessible members named `N` is determined:</span></span>
    * <span data-ttu-id="08560-402">Pokud je `T` parametr typu, pak je sada sjednocením sad přístupných členů s názvem `N` v každém z typů určených jako primární omezení nebo sekundární omezení ([omezení parametrů typu](classes.md#type-parameter-constraints)) pro `T`, spolu se sadou přístupných členů s názvem `N` v `object`.</span><span class="sxs-lookup"><span data-stu-id="08560-402">If `T` is a type parameter, then the set is the union of the sets of accessible members named `N` in each of the types specified as a primary constraint or secondary constraint ([Type parameter constraints](classes.md#type-parameter-constraints)) for `T`, along with the set of accessible members named `N` in `object`.</span></span>
    * <span data-ttu-id="08560-403">V opačném případě se sada skládá ze všech přístupných členů ([přístupu členů](basic-concepts.md#member-access)) s názvem `N` v `T`, včetně zděděných členů a přístupných členů s názvem `N` v `object`.</span><span class="sxs-lookup"><span data-stu-id="08560-403">Otherwise, the set consists of all accessible ([Member access](basic-concepts.md#member-access)) members named `N` in `T`, including inherited members and the accessible members named `N` in `object`.</span></span> <span data-ttu-id="08560-404">Je-li `T` konstruovaný typ, sada členů je získána nahrazením argumentů typu, jak je popsáno v tématu [Členové konstruovaných typů](classes.md#members-of-constructed-types).</span><span class="sxs-lookup"><span data-stu-id="08560-404">If `T` is a constructed type, the set of members is obtained by substituting type arguments as described in [Members of constructed types](classes.md#members-of-constructed-types).</span></span> <span data-ttu-id="08560-405">Členy, které obsahují modifikátor `override`, jsou ze sady vyloučeny.</span><span class="sxs-lookup"><span data-stu-id="08560-405">Members that include an `override` modifier are excluded from the set.</span></span>
*  <span data-ttu-id="08560-406">Pokud je `K` nula, všechny vnořené typy, jejichž deklarace zahrnují parametry typu, se odeberou.</span><span class="sxs-lookup"><span data-stu-id="08560-406">Next, if `K` is zero, all nested types whose declarations include type parameters are removed.</span></span> <span data-ttu-id="08560-407">Pokud `K` není nula, budou odebrány všechny členy s jiným počtem parametrů typu.</span><span class="sxs-lookup"><span data-stu-id="08560-407">If `K` is not zero, all members with a different number of type parameters are removed.</span></span> <span data-ttu-id="08560-408">Všimněte si, že pokud `K` je nula, metody s parametry typu nejsou odebrány, protože proces odvození typu ([odvození typu](expressions.md#type-inference)) může být schopný odvodit argumenty typu.</span><span class="sxs-lookup"><span data-stu-id="08560-408">Note that when `K` is zero, methods having type parameters are not removed, since the type inference process ([Type inference](expressions.md#type-inference)) might be able to infer the type arguments.</span></span>
*  <span data-ttu-id="08560-409">Dále, pokud je člen *vyvolán*, všechny*nenevyvolatelný* členové budou ze sady odebrány.</span><span class="sxs-lookup"><span data-stu-id="08560-409">Next, if the member is *invoked*, all non-*invocable* members are removed from the set.</span></span>
*  <span data-ttu-id="08560-410">Dále jsou členové, kteří jsou skryti jinými členy, ze sady odebrány.</span><span class="sxs-lookup"><span data-stu-id="08560-410">Next, members that are hidden by other members are removed from the set.</span></span> <span data-ttu-id="08560-411">Pro každý členský `S.M` v sadě, kde `S` je typ, ve kterém je deklarován členský `M`, jsou použita následující pravidla:</span><span class="sxs-lookup"><span data-stu-id="08560-411">For every member `S.M` in the set, where `S` is the type in which the member `M` is declared, the following rules are applied:</span></span>
    * <span data-ttu-id="08560-412">Pokud `M` je konstanta, pole, vlastnost, událost nebo člen výčtu, pak jsou ze sady odebrány všechny členy deklarované v základním typu `S`.</span><span class="sxs-lookup"><span data-stu-id="08560-412">If `M` is a constant, field, property, event, or enumeration member, then all members declared in a base type of `S` are removed from the set.</span></span>
    * <span data-ttu-id="08560-413">Pokud je `M` deklarace typu, pak všechny netypy deklarované v základním typu `S` jsou ze sady odebrány a všechny deklarace typu se stejným počtem parametrů typu, jako `M` deklarované v základním typu `S`, jsou ze sady odebrány.</span><span class="sxs-lookup"><span data-stu-id="08560-413">If `M` is a type declaration, then all non-types declared in a base type of `S` are removed from the set, and all type declarations with the same number of type parameters as `M` declared in a base type of `S` are removed from the set.</span></span>
    * <span data-ttu-id="08560-414">Pokud je `M` metodou, pak všechny členy, které nejsou deklarovány v základním typu `S` jsou ze sady odebrány.</span><span class="sxs-lookup"><span data-stu-id="08560-414">If `M` is a method, then all non-method members declared in a base type of `S` are removed from the set.</span></span>
*  <span data-ttu-id="08560-415">Dále jsou členy rozhraní skryté ze sady odebrány.</span><span class="sxs-lookup"><span data-stu-id="08560-415">Next, interface members that are hidden by class members are removed from the set.</span></span> <span data-ttu-id="08560-416">Tento krok má vliv pouze v případě, že `T` je parametr typu a `T` má platnou základní třídu kromě `object` a neprázdnou sadu platných rozhraní ([omezení parametrů typu](classes.md#type-parameter-constraints)).</span><span class="sxs-lookup"><span data-stu-id="08560-416">This step only has an effect if `T` is a type parameter and `T` has both an effective base class other than `object` and a non-empty effective interface set ([Type parameter constraints](classes.md#type-parameter-constraints)).</span></span> <span data-ttu-id="08560-417">Pro každý členský `S.M` v sadě, kde `S` je typ, ve kterém je deklarován členský `M`, jsou použita následující pravidla, pokud `S` je deklarace třídy jiná než `object`:</span><span class="sxs-lookup"><span data-stu-id="08560-417">For every member `S.M` in the set, where `S` is the type in which the member `M` is declared, the following rules are applied if `S` is a class declaration other than `object`:</span></span>
    * <span data-ttu-id="08560-418">Pokud `M` je konstanta, pole, vlastnost, událost, člen výčtu nebo deklarace typu, pak jsou ze sady odebrány všechny členy deklarované v deklaraci rozhraní.</span><span class="sxs-lookup"><span data-stu-id="08560-418">If `M` is a constant, field, property, event, enumeration member, or type declaration, then all members declared in an interface declaration are removed from the set.</span></span>
    * <span data-ttu-id="08560-419">Pokud je `M` metodou, pak všechny členy, které nejsou deklarovány v deklaraci rozhraní, jsou ze sady odebrány a všechny metody se stejným podpisem jako `M` deklarované v deklaraci rozhraní jsou ze sady odebrány.</span><span class="sxs-lookup"><span data-stu-id="08560-419">If `M` is a method, then all non-method members declared in an interface declaration are removed from the set, and all methods with the same signature as `M` declared in an interface declaration are removed from the set.</span></span>
*  <span data-ttu-id="08560-420">Nakonec, po odebrání skrytých členů, se určí výsledek vyhledávání:</span><span class="sxs-lookup"><span data-stu-id="08560-420">Finally, having removed hidden members, the result of the lookup is determined:</span></span>
    * <span data-ttu-id="08560-421">Pokud se sada skládá z jednoho člena, který není metodou, pak je tento člen výsledkem vyhledávání.</span><span class="sxs-lookup"><span data-stu-id="08560-421">If the set consists of a single member that is not a method, then this member is the result of the lookup.</span></span>
    * <span data-ttu-id="08560-422">V opačném případě, pokud sada obsahuje pouze metody, pak je tato skupina metod výsledkem vyhledávání.</span><span class="sxs-lookup"><span data-stu-id="08560-422">Otherwise, if the set contains only methods, then this group of methods is the result of the lookup.</span></span>
    * <span data-ttu-id="08560-423">V opačném případě je vyhledávání dvojznačné a dojde k chybě při vazbě.</span><span class="sxs-lookup"><span data-stu-id="08560-423">Otherwise, the lookup is ambiguous, and a binding-time error occurs.</span></span>

<span data-ttu-id="08560-424">Pro vyhledávání členů v jiných typech než parametry typu a rozhraní a vyhledávání členů v rozhraních, která jsou výhradně jedinou dědičností (každé rozhraní v řetězu dědičnosti má přesně nula nebo jedno přímé základní rozhraní), je efekt vyhledávacích pravidel pouze odvoditelné členy skryjí základní členy se stejným názvem nebo signaturou.</span><span class="sxs-lookup"><span data-stu-id="08560-424">For member lookups in types other than type parameters and interfaces, and member lookups in interfaces that are strictly single-inheritance (each interface in the inheritance chain has exactly zero or one direct base interface), the effect of the lookup rules is simply that derived members hide base members with the same name or signature.</span></span> <span data-ttu-id="08560-425">Tato vyhledávání s jednou dědičností nejsou nikdy dvojznačná.</span><span class="sxs-lookup"><span data-stu-id="08560-425">Such single-inheritance lookups are never ambiguous.</span></span> <span data-ttu-id="08560-426">Nejednoznačnosti, které mohou nastat při hledání členů v rozhraních vícenásobné dědičnosti, jsou popsány v tématu [přístup ke členu rozhraní](interfaces.md#interface-member-access).</span><span class="sxs-lookup"><span data-stu-id="08560-426">The ambiguities that can possibly arise from member lookups in multiple-inheritance interfaces are described in [Interface member access](interfaces.md#interface-member-access).</span></span>

### <a name="base-types"></a><span data-ttu-id="08560-427">Základní typy</span><span class="sxs-lookup"><span data-stu-id="08560-427">Base types</span></span>

<span data-ttu-id="08560-428">Pro účely vyhledávání členů se typ `T` považuje za následující základní typy:</span><span class="sxs-lookup"><span data-stu-id="08560-428">For purposes of member lookup, a type `T` is considered to have the following base types:</span></span>

*  <span data-ttu-id="08560-429">Pokud je `T` `object`, `T` nemá žádný základní typ.</span><span class="sxs-lookup"><span data-stu-id="08560-429">If `T` is `object`, then `T` has no base type.</span></span>
*  <span data-ttu-id="08560-430">Pokud je `T` *enum_type*, základní typy `T` jsou typy třídy `System.Enum`, `System.ValueType`a `object`.</span><span class="sxs-lookup"><span data-stu-id="08560-430">If `T` is an *enum_type*, the base types of `T` are the class types `System.Enum`, `System.ValueType`, and `object`.</span></span>
*  <span data-ttu-id="08560-431">Pokud je `T` *struct_type*, základní typy `T` jsou typy třídy `System.ValueType` a `object`.</span><span class="sxs-lookup"><span data-stu-id="08560-431">If `T` is a *struct_type*, the base types of `T` are the class types `System.ValueType` and `object`.</span></span>
*  <span data-ttu-id="08560-432">Pokud je `T` *class_type*, základní typy `T` jsou základní třídy `T`, včetně typu třídy `object`.</span><span class="sxs-lookup"><span data-stu-id="08560-432">If `T` is a *class_type*, the base types of `T` are the base classes of `T`, including the class type `object`.</span></span>
*  <span data-ttu-id="08560-433">Pokud je `T` *INTERFACE_TYPE*, základní typy `T` jsou základní rozhraní `T` a typ třídy `object`.</span><span class="sxs-lookup"><span data-stu-id="08560-433">If `T` is an *interface_type*, the base types of `T` are the base interfaces of `T` and the class type `object`.</span></span>
*  <span data-ttu-id="08560-434">Pokud je `T` *array_type*, základní typy `T` jsou typy třídy `System.Array` a `object`.</span><span class="sxs-lookup"><span data-stu-id="08560-434">If `T` is an *array_type*, the base types of `T` are the class types `System.Array` and `object`.</span></span>
*  <span data-ttu-id="08560-435">Pokud je `T` *delegate_type*, základní typy `T` jsou typy třídy `System.Delegate` a `object`.</span><span class="sxs-lookup"><span data-stu-id="08560-435">If `T` is a *delegate_type*, the base types of `T` are the class types `System.Delegate` and `object`.</span></span>

## <a name="function-members"></a><span data-ttu-id="08560-436">Členové funkce</span><span class="sxs-lookup"><span data-stu-id="08560-436">Function members</span></span>

<span data-ttu-id="08560-437">Členy funkce jsou členy, které obsahují spustitelné příkazy.</span><span class="sxs-lookup"><span data-stu-id="08560-437">Function members are members that contain executable statements.</span></span> <span data-ttu-id="08560-438">Členové funkce jsou vždy členy typu a nemohou být členy oborů názvů.</span><span class="sxs-lookup"><span data-stu-id="08560-438">Function members are always members of types and cannot be members of namespaces.</span></span> <span data-ttu-id="08560-439">C#definuje následující kategorie členů funkcí:</span><span class="sxs-lookup"><span data-stu-id="08560-439">C# defines the following categories of function members:</span></span>

*  <span data-ttu-id="08560-440">Metody</span><span class="sxs-lookup"><span data-stu-id="08560-440">Methods</span></span>
*  <span data-ttu-id="08560-441">Vlastnosti</span><span class="sxs-lookup"><span data-stu-id="08560-441">Properties</span></span>
*  <span data-ttu-id="08560-442">Události</span><span class="sxs-lookup"><span data-stu-id="08560-442">Events</span></span>
*  <span data-ttu-id="08560-443">Indexery</span><span class="sxs-lookup"><span data-stu-id="08560-443">Indexers</span></span>
*  <span data-ttu-id="08560-444">Uživatelem definované operátory</span><span class="sxs-lookup"><span data-stu-id="08560-444">User-defined operators</span></span>
*  <span data-ttu-id="08560-445">Konstruktory instancí</span><span class="sxs-lookup"><span data-stu-id="08560-445">Instance constructors</span></span>
*  <span data-ttu-id="08560-446">Statické konstruktory</span><span class="sxs-lookup"><span data-stu-id="08560-446">Static constructors</span></span>
*  <span data-ttu-id="08560-447">Destruktory</span><span class="sxs-lookup"><span data-stu-id="08560-447">Destructors</span></span>

<span data-ttu-id="08560-448">S výjimkou destruktorů a statických konstruktorů (které nelze vyvolat explicitně) jsou příkazy, které jsou obsaženy v členech funkce, spouštěny prostřednictvím vyvolání členů funkce.</span><span class="sxs-lookup"><span data-stu-id="08560-448">Except for destructors and static constructors (which cannot be invoked explicitly), the statements contained in function members are executed through function member invocations.</span></span> <span data-ttu-id="08560-449">Skutečná syntaxe pro zápis člena funkce závisí na konkrétní kategorii člena funkce.</span><span class="sxs-lookup"><span data-stu-id="08560-449">The actual syntax for writing a function member invocation depends on the particular function member category.</span></span>

<span data-ttu-id="08560-450">Seznam argumentů ([seznamy argumentů](expressions.md#argument-lists)) vyvolání člena funkce poskytuje skutečné hodnoty nebo odkazy na proměnné pro parametry člena funkce.</span><span class="sxs-lookup"><span data-stu-id="08560-450">The argument list ([Argument lists](expressions.md#argument-lists)) of a function member invocation provides actual values or variable references for the parameters of the function member.</span></span>

<span data-ttu-id="08560-451">Volání obecných metod mohou využívat odvození typu pro určení sady argumentů typu, které mají být předávány metodě.</span><span class="sxs-lookup"><span data-stu-id="08560-451">Invocations of generic methods may employ type inference to determine the set of type arguments to pass to the method.</span></span> <span data-ttu-id="08560-452">Tento proces je popsán v tématu [odvození typu](expressions.md#type-inference).</span><span class="sxs-lookup"><span data-stu-id="08560-452">This process is described in [Type inference](expressions.md#type-inference).</span></span>

<span data-ttu-id="08560-453">Volání metod, indexerů, operátorů a konstruktorů instancí využívají rozlišení přetěžování k určení, které z kandidátních sad členů funkce mají být vyvolány.</span><span class="sxs-lookup"><span data-stu-id="08560-453">Invocations of methods, indexers, operators and instance constructors employ overload resolution to determine which of a candidate set of function members to invoke.</span></span> <span data-ttu-id="08560-454">Tento proces je popsán v tématu [řešení přetížení](expressions.md#overload-resolution).</span><span class="sxs-lookup"><span data-stu-id="08560-454">This process is described in [Overload resolution](expressions.md#overload-resolution).</span></span>

<span data-ttu-id="08560-455">Po zjištění konkrétního člena funkce v době vytváření vazby, případně prostřednictvím řešení přetížení, je skutečný proces spuštění volání členu funkce popsán v [době kompilace dynamického překladu přetížení](expressions.md#compile-time-checking-of-dynamic-overload-resolution).</span><span class="sxs-lookup"><span data-stu-id="08560-455">Once a particular function member has been identified at binding-time, possibly through overload resolution, the actual run-time process of invoking the function member is described in [Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution).</span></span>

<span data-ttu-id="08560-456">Následující tabulka shrnuje zpracování, které je provedeno v konstrukcích, které zahrnují šest kategorií členů funkce, které lze explicitně vyvolat.</span><span class="sxs-lookup"><span data-stu-id="08560-456">The following table summarizes the processing that takes place in constructs involving the six categories of function members that can be explicitly invoked.</span></span> <span data-ttu-id="08560-457">V tabulce `e`, `x`, `y`a `value` ukazují výrazy klasifikované jako proměnné nebo hodnoty, `T` označuje výraz klasifikovaný jako typ, `F` je jednoduchý název metody a `P` je jednoduchý název vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="08560-457">In the table, `e`, `x`, `y`, and `value` indicate expressions classified as variables or values, `T` indicates an expression classified as a type, `F` is the simple name of a method, and `P` is the simple name of a property.</span></span>


| <span data-ttu-id="08560-458">__Contains__</span><span class="sxs-lookup"><span data-stu-id="08560-458">__Construct__</span></span>     | <span data-ttu-id="08560-459">__Příklad__</span><span class="sxs-lookup"><span data-stu-id="08560-459">__Example__</span></span>    | <span data-ttu-id="08560-460">__Popis__</span><span class="sxs-lookup"><span data-stu-id="08560-460">__Description__</span></span> |
|-------------------|----------------|-----------------|
| <span data-ttu-id="08560-461">Vyvolání metody</span><span class="sxs-lookup"><span data-stu-id="08560-461">Method invocation</span></span> | `F(x,y)`       | <span data-ttu-id="08560-462">Rozlišení přetěžování je použito pro výběr nejlepší metody `F` v nadřazené třídě nebo struktuře.</span><span class="sxs-lookup"><span data-stu-id="08560-462">Overload resolution is applied to select the best method `F` in the containing class or struct.</span></span> <span data-ttu-id="08560-463">Metoda je vyvolána se seznamem argumentů `(x,y)`.</span><span class="sxs-lookup"><span data-stu-id="08560-463">The method is invoked with the argument list `(x,y)`.</span></span> <span data-ttu-id="08560-464">Pokud metoda není `static`, je výraz instance `this`.</span><span class="sxs-lookup"><span data-stu-id="08560-464">If the method is not `static`, the instance expression is `this`.</span></span> | 
|                   | `T.F(x,y)`     | <span data-ttu-id="08560-465">Rozlišení přetěžování je použito pro výběr nejlepší metody `F` v `T`třídy nebo struktury.</span><span class="sxs-lookup"><span data-stu-id="08560-465">Overload resolution is applied to select the best method `F` in the class or struct `T`.</span></span> <span data-ttu-id="08560-466">Pokud není metoda `static`, dojde k chybě při vazbě.</span><span class="sxs-lookup"><span data-stu-id="08560-466">A binding-time error occurs if the method is not `static`.</span></span> <span data-ttu-id="08560-467">Metoda je vyvolána se seznamem argumentů `(x,y)`.</span><span class="sxs-lookup"><span data-stu-id="08560-467">The method is invoked with the argument list `(x,y)`.</span></span> | 
|                   | `e.F(x,y)`     | <span data-ttu-id="08560-468">Rozlišení přetěžování je použito pro výběr nejlepší metody F ve třídě, struktuře nebo rozhraní zadané typem `e`.</span><span class="sxs-lookup"><span data-stu-id="08560-468">Overload resolution is applied to select the best method F in the class, struct, or interface given by the type of `e`.</span></span> <span data-ttu-id="08560-469">Pokud je metoda `static`, dojde k chybě při vazbě.</span><span class="sxs-lookup"><span data-stu-id="08560-469">A binding-time error occurs if the method is `static`.</span></span> <span data-ttu-id="08560-470">Metoda je vyvolána s výrazem instance `e` a seznamem argumentů `(x,y)`.</span><span class="sxs-lookup"><span data-stu-id="08560-470">The method is invoked with the instance expression `e` and the argument list `(x,y)`.</span></span> | 
| <span data-ttu-id="08560-471">Přístup k vlastnosti</span><span class="sxs-lookup"><span data-stu-id="08560-471">Property access</span></span>   | `P`            | <span data-ttu-id="08560-472">Je vyvolán přistupující objekt `get` vlastnosti `P` v nadřazené třídě nebo struktuře.</span><span class="sxs-lookup"><span data-stu-id="08560-472">The `get` accessor of the property `P` in the containing class or struct is invoked.</span></span> <span data-ttu-id="08560-473">Pokud je `P` pouze pro zápis, dojde k chybě při kompilaci.</span><span class="sxs-lookup"><span data-stu-id="08560-473">A compile-time error occurs if `P` is write-only.</span></span> <span data-ttu-id="08560-474">Pokud `P` není `static`, je výraz instance `this`.</span><span class="sxs-lookup"><span data-stu-id="08560-474">If `P` is not `static`, the instance expression is `this`.</span></span> | 
|                   | `P = value`    | <span data-ttu-id="08560-475">Přístupový objekt `set` vlastnosti `P` v nadřazené třídě nebo struktuře je vyvolán se seznamem argumentů `(value)`.</span><span class="sxs-lookup"><span data-stu-id="08560-475">The `set` accessor of the property `P` in the containing class or struct is invoked with the argument list `(value)`.</span></span> <span data-ttu-id="08560-476">Pokud je `P` jen pro čtení, dojde k chybě při kompilaci.</span><span class="sxs-lookup"><span data-stu-id="08560-476">A compile-time error occurs if `P` is read-only.</span></span> <span data-ttu-id="08560-477">Pokud `P` není `static`, je výraz instance `this`.</span><span class="sxs-lookup"><span data-stu-id="08560-477">If `P` is not `static`, the instance expression is `this`.</span></span> | 
|                   | `T.P`          | <span data-ttu-id="08560-478">Je vyvolán přistupující objekt `get` vlastnosti `P` ve třídě nebo struktuře `T`.</span><span class="sxs-lookup"><span data-stu-id="08560-478">The `get` accessor of the property `P` in the class or struct `T` is invoked.</span></span> <span data-ttu-id="08560-479">Pokud `P` není `static` nebo pokud je `P` pouze pro zápis, dojde k chybě v době kompilace.</span><span class="sxs-lookup"><span data-stu-id="08560-479">A compile-time error occurs if `P` is not `static` or if `P` is write-only.</span></span> | 
|                   | `T.P = value`  | <span data-ttu-id="08560-480">Přístupový objekt `set` vlastnosti `P` ve třídě nebo struktuře `T` je vyvolán pomocí `(value)`seznamu argumentů.</span><span class="sxs-lookup"><span data-stu-id="08560-480">The `set` accessor of the property `P` in the class or struct `T` is invoked with the argument list `(value)`.</span></span> <span data-ttu-id="08560-481">Pokud `P` není `static` nebo pokud je `P` jen pro čtení, dojde k chybě při kompilaci.</span><span class="sxs-lookup"><span data-stu-id="08560-481">A compile-time error occurs if `P` is not `static` or if `P` is read-only.</span></span> | 
|                   | `e.P`          | <span data-ttu-id="08560-482">Přístupový objekt `get` vlastnosti `P` ve třídě, struktuře nebo rozhraní určeném typem `e` je vyvolán s výrazem instance `e`.</span><span class="sxs-lookup"><span data-stu-id="08560-482">The `get` accessor of the property `P` in the class, struct, or interface given by the type of `e` is invoked with the instance expression `e`.</span></span> <span data-ttu-id="08560-483">Pokud je `P` `static` nebo pokud je `P` pouze pro zápis, dojde k chybě při vazbě.</span><span class="sxs-lookup"><span data-stu-id="08560-483">A binding-time error occurs if `P` is `static` or if `P` is write-only.</span></span> | 
|                   | `e.P = value`  | <span data-ttu-id="08560-484">Přístupový objekt `set` vlastnosti `P` ve třídě, struktuře nebo rozhraní zadané typem `e` je vyvolán s výrazem instance `e` a seznamem argumentů `(value)`.</span><span class="sxs-lookup"><span data-stu-id="08560-484">The `set` accessor of the property `P` in the class, struct, or interface given by the type of `e` is invoked with the instance expression `e` and the argument list `(value)`.</span></span> <span data-ttu-id="08560-485">Pokud je `P` `static` nebo pokud je `P` jen pro čtení, dojde k chybě při vazbě.</span><span class="sxs-lookup"><span data-stu-id="08560-485">A binding-time error occurs if `P` is `static` or if `P` is read-only.</span></span> | 
| <span data-ttu-id="08560-486">Přístup k události</span><span class="sxs-lookup"><span data-stu-id="08560-486">Event access</span></span>      | `E += value`   | <span data-ttu-id="08560-487">Je vyvolán přistupující objekt `add` `E` události v nadřazené třídě nebo struktuře.</span><span class="sxs-lookup"><span data-stu-id="08560-487">The `add` accessor of the event `E` in the containing class or struct is invoked.</span></span> <span data-ttu-id="08560-488">Pokud `E` není static, je výraz instance `this`.</span><span class="sxs-lookup"><span data-stu-id="08560-488">If `E` is not static, the instance expression is `this`.</span></span> | 
|                   | `E -= value`   | <span data-ttu-id="08560-489">Je vyvolán přistupující objekt `remove` `E` události v nadřazené třídě nebo struktuře.</span><span class="sxs-lookup"><span data-stu-id="08560-489">The `remove` accessor of the event `E` in the containing class or struct is invoked.</span></span> <span data-ttu-id="08560-490">Pokud `E` není static, je výraz instance `this`.</span><span class="sxs-lookup"><span data-stu-id="08560-490">If `E` is not static, the instance expression is `this`.</span></span> | 
|                   | `T.E += value` | <span data-ttu-id="08560-491">Je vyvolán přistupující objekt `add` události `E` ve třídě nebo struktuře `T`.</span><span class="sxs-lookup"><span data-stu-id="08560-491">The `add` accessor of the event `E` in the class or struct `T` is invoked.</span></span> <span data-ttu-id="08560-492">Pokud `E` není statická, dojde k chybě při vazbě.</span><span class="sxs-lookup"><span data-stu-id="08560-492">A binding-time error occurs if `E` is not static.</span></span> | 
|                   | `T.E -= value` | <span data-ttu-id="08560-493">Je vyvolán přistupující objekt `remove` události `E` ve třídě nebo struktuře `T`.</span><span class="sxs-lookup"><span data-stu-id="08560-493">The `remove` accessor of the event `E` in the class or struct `T` is invoked.</span></span> <span data-ttu-id="08560-494">Pokud `E` není statická, dojde k chybě při vazbě.</span><span class="sxs-lookup"><span data-stu-id="08560-494">A binding-time error occurs if `E` is not static.</span></span> | 
|                   | `e.E += value` | <span data-ttu-id="08560-495">Přístupový objekt `add` události `E` ve třídě, struktuře nebo rozhraní zadané typem `e` je vyvolán s výrazem instance `e`.</span><span class="sxs-lookup"><span data-stu-id="08560-495">The `add` accessor of the event `E` in the class, struct, or interface given by the type of `e` is invoked with the instance expression `e`.</span></span> <span data-ttu-id="08560-496">Pokud je `E` statická, dojde k chybě při vazbě.</span><span class="sxs-lookup"><span data-stu-id="08560-496">A binding-time error occurs if `E` is static.</span></span> | 
|                   | `e.E -= value` | <span data-ttu-id="08560-497">Přístupový objekt `remove` události `E` ve třídě, struktuře nebo rozhraní zadané typem `e` je vyvolán s výrazem instance `e`.</span><span class="sxs-lookup"><span data-stu-id="08560-497">The `remove` accessor of the event `E` in the class, struct, or interface given by the type of `e` is invoked with the instance expression `e`.</span></span> <span data-ttu-id="08560-498">Pokud je `E` statická, dojde k chybě při vazbě.</span><span class="sxs-lookup"><span data-stu-id="08560-498">A binding-time error occurs if `E` is static.</span></span> | 
| <span data-ttu-id="08560-499">Přístup indexeru</span><span class="sxs-lookup"><span data-stu-id="08560-499">Indexer access</span></span>    | `e[x,y]`       | <span data-ttu-id="08560-500">Rozlišení přetěžování je použito pro výběr nejlepšího indexeru ve třídě, struktuře nebo rozhraní, které je zadáno pomocí typu e.</span><span class="sxs-lookup"><span data-stu-id="08560-500">Overload resolution is applied to select the best indexer in the class, struct, or interface given by the type of e.</span></span> <span data-ttu-id="08560-501">Přístupový objekt `get` indexeru je vyvolán s výrazem instance `e` a `(x,y)`seznamu argumentů.</span><span class="sxs-lookup"><span data-stu-id="08560-501">The `get` accessor of the indexer is invoked with the instance expression `e` and the argument list `(x,y)`.</span></span> <span data-ttu-id="08560-502">V případě, že je indexer určen pouze pro zápis, dojde k chybě při vazbě.</span><span class="sxs-lookup"><span data-stu-id="08560-502">A binding-time error occurs if the indexer is write-only.</span></span> | 
|                   | `e[x,y] = value` | <span data-ttu-id="08560-503">Rozlišení přetěžování je použito pro výběr nejlepšího indexeru ve třídě, struktuře nebo rozhraní zadaného typem `e`.</span><span class="sxs-lookup"><span data-stu-id="08560-503">Overload resolution is applied to select the best indexer in the class, struct, or interface given by the type of `e`.</span></span> <span data-ttu-id="08560-504">Přístupový objekt `set` indexeru je vyvolán s výrazem instance `e` a `(x,y,value)`seznamu argumentů.</span><span class="sxs-lookup"><span data-stu-id="08560-504">The `set` accessor of the indexer is invoked with the instance expression `e` and the argument list `(x,y,value)`.</span></span> <span data-ttu-id="08560-505">V případě, že je indexer určen jen pro čtení, dojde k chybě při vazbě.</span><span class="sxs-lookup"><span data-stu-id="08560-505">A binding-time error occurs if the indexer is read-only.</span></span> | 
| <span data-ttu-id="08560-506">Vyvolání operátoru</span><span class="sxs-lookup"><span data-stu-id="08560-506">Operator invocation</span></span> | `-x`         | <span data-ttu-id="08560-507">Rozlišení přetěžování je použito pro výběr nejlepšího unárního operátoru ve třídě nebo struktuře dané typem `x`.</span><span class="sxs-lookup"><span data-stu-id="08560-507">Overload resolution is applied to select the best unary operator in the class or struct given by the type of `x`.</span></span> <span data-ttu-id="08560-508">Vybraný operátor je vyvolán se seznamem argumentů `(x)`.</span><span class="sxs-lookup"><span data-stu-id="08560-508">The selected operator is invoked with the argument list `(x)`.</span></span> | 
|                     | `x + y`      | <span data-ttu-id="08560-509">Rozlišení přetěžování je použito pro výběr nejlepšího binárního operátoru v třídách nebo strukturách daných typy `x` a `y`.</span><span class="sxs-lookup"><span data-stu-id="08560-509">Overload resolution is applied to select the best binary operator in the classes or structs given by the types of `x` and `y`.</span></span> <span data-ttu-id="08560-510">Vybraný operátor je vyvolán se seznamem argumentů `(x,y)`.</span><span class="sxs-lookup"><span data-stu-id="08560-510">The selected operator is invoked with the argument list `(x,y)`.</span></span> | 
| <span data-ttu-id="08560-511">Vyvolání konstruktoru instance</span><span class="sxs-lookup"><span data-stu-id="08560-511">Instance constructor invocation</span></span> | `new T(x,y)` | <span data-ttu-id="08560-512">Rozlišení přetěžování je použito pro výběr nejlepšího konstruktoru instance ve třídě nebo struktuře `T`.</span><span class="sxs-lookup"><span data-stu-id="08560-512">Overload resolution is applied to select the best instance constructor in the class or struct `T`.</span></span> <span data-ttu-id="08560-513">Konstruktor instance je vyvolán se seznamem argumentů `(x,y)`.</span><span class="sxs-lookup"><span data-stu-id="08560-513">The instance constructor is invoked with the argument list `(x,y)`.</span></span> | 

### <a name="argument-lists"></a><span data-ttu-id="08560-514">Seznamy argumentů</span><span class="sxs-lookup"><span data-stu-id="08560-514">Argument lists</span></span>

<span data-ttu-id="08560-515">Každý člen funkce a volání delegáta obsahují seznam argumentů, který poskytuje skutečné hodnoty nebo odkazy na proměnné pro parametry člena funkce.</span><span class="sxs-lookup"><span data-stu-id="08560-515">Every function member and delegate invocation includes an argument list which provides actual values or variable references for the parameters of the function member.</span></span> <span data-ttu-id="08560-516">Syntaxe pro určení seznamu argumentů vyvolání člena funkce závisí na kategorii člena funkce:</span><span class="sxs-lookup"><span data-stu-id="08560-516">The syntax for specifying the argument list of a function member invocation depends on the function member category:</span></span>

*  <span data-ttu-id="08560-517">Pro konstruktory instancí, metody, indexery a delegáty jsou argumenty zadány jako *argument_list*, jak je popsáno níže.</span><span class="sxs-lookup"><span data-stu-id="08560-517">For instance constructors, methods, indexers and delegates, the arguments are specified as an *argument_list*, as described below.</span></span> <span data-ttu-id="08560-518">U indexerů při vyvolání `set` přistupující objekt seznam argumentů zahrnuje také výraz zadaný jako pravý operand operátoru přiřazení.</span><span class="sxs-lookup"><span data-stu-id="08560-518">For indexers, when invoking the `set` accessor, the argument list additionally includes the expression specified as the right operand of the assignment operator.</span></span>
*  <span data-ttu-id="08560-519">V případě vlastností je seznam argumentů prázdný při vyvolání přístupového objektu `get` a skládá se z výrazu zadaného jako pravý operand operátoru přiřazení při vyvolání přístupového objektu `set`.</span><span class="sxs-lookup"><span data-stu-id="08560-519">For properties, the argument list is empty when invoking the `get` accessor, and consists of the expression specified as the right operand of the assignment operator when invoking the `set` accessor.</span></span>
*  <span data-ttu-id="08560-520">V případě událostí se seznam argumentů skládá z výrazu zadaného jako pravý operand operátoru `+=` nebo `-=`.</span><span class="sxs-lookup"><span data-stu-id="08560-520">For events, the argument list consists of the expression specified as the right operand of the `+=` or `-=` operator.</span></span>
*  <span data-ttu-id="08560-521">V případě uživatelem definovaných operátorů se seznam argumentů skládá z jednoho operandu unárního operátoru nebo dvou operandů binárního operátoru.</span><span class="sxs-lookup"><span data-stu-id="08560-521">For user-defined operators, the argument list consists of the single operand of the unary operator or the two operands of the binary operator.</span></span>

<span data-ttu-id="08560-522">Argumenty vlastností ([vlastnosti](classes.md#properties)), události ([události](classes.md#events)) a uživatelsky definované operátory ([operátory](classes.md#operators)) jsou vždy předány jako parametry hodnoty ([parametry hodnot](classes.md#value-parameters)).</span><span class="sxs-lookup"><span data-stu-id="08560-522">The arguments of properties ([Properties](classes.md#properties)), events ([Events](classes.md#events)), and user-defined operators ([Operators](classes.md#operators)) are always passed as value parameters ([Value parameters](classes.md#value-parameters)).</span></span> <span data-ttu-id="08560-523">Argumenty indexerů ([indexerů](classes.md#indexers)) jsou vždy předány jako parametry hodnoty ([parametry hodnot](classes.md#value-parameters)) nebo pole parametrů ([pole parametrů](classes.md#parameter-arrays)).</span><span class="sxs-lookup"><span data-stu-id="08560-523">The arguments of indexers ([Indexers](classes.md#indexers)) are always passed as value parameters ([Value parameters](classes.md#value-parameters)) or parameter arrays ([Parameter arrays](classes.md#parameter-arrays)).</span></span> <span data-ttu-id="08560-524">Parametry reference a Output nejsou podporovány pro tyto kategorie členů funkce.</span><span class="sxs-lookup"><span data-stu-id="08560-524">Reference and output parameters are not supported for these categories of function members.</span></span>

<span data-ttu-id="08560-525">Argumenty konstruktoru instance, metody, indexeru nebo delegovaného volání jsou zadány jako *argument_list*:</span><span class="sxs-lookup"><span data-stu-id="08560-525">The arguments of an instance constructor, method, indexer or delegate invocation are specified as an *argument_list*:</span></span>

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

<span data-ttu-id="08560-526">*Argument_list* se skládá z jednoho nebo více *argumentů*, které jsou odděleny čárkami.</span><span class="sxs-lookup"><span data-stu-id="08560-526">An *argument_list* consists of one or more *argument*s, separated by commas.</span></span> <span data-ttu-id="08560-527">Každý argument se skládá z volitelného *argument_name* následovaného *argument_value*.</span><span class="sxs-lookup"><span data-stu-id="08560-527">Each argument consists of an optional  *argument_name* followed by an *argument_value*.</span></span> <span data-ttu-id="08560-528">*Argument* s *argument_name* je označován jako ***pojmenovaný argument***, zatímco *Argument* bez *argument_name* je ***poziční argument***.</span><span class="sxs-lookup"><span data-stu-id="08560-528">An *argument* with an *argument_name* is referred to as a ***named argument***, whereas an *argument* without an *argument_name* is a ***positional argument***.</span></span> <span data-ttu-id="08560-529">Je-li pozice argumentu uvedena po pojmenovaném argumentu v *argument_list*, jedná se o chybu.</span><span class="sxs-lookup"><span data-stu-id="08560-529">It is an error for a positional argument to appear after a named argument in an *argument_list*.</span></span>

<span data-ttu-id="08560-530">*Argument_value* může mít jednu z následujících forem:</span><span class="sxs-lookup"><span data-stu-id="08560-530">The *argument_value* can take one of the following forms:</span></span>

*  <span data-ttu-id="08560-531">*Výraz*, který označuje, že argument je předán jako parametr hodnoty ([parametry hodnoty](classes.md#value-parameters)).</span><span class="sxs-lookup"><span data-stu-id="08560-531">An *expression*, indicating that the argument is passed as a value parameter ([Value parameters](classes.md#value-parameters)).</span></span>
*  <span data-ttu-id="08560-532">Klíčové slovo `ref` následované *variable_reference* ([odkazy na proměnné](variables.md#variable-references)) označující, že argument se předává jako referenční parametr ([referenční parametry](classes.md#reference-parameters)).</span><span class="sxs-lookup"><span data-stu-id="08560-532">The keyword `ref` followed by a *variable_reference* ([Variable references](variables.md#variable-references)), indicating that the argument is passed as a reference parameter ([Reference parameters](classes.md#reference-parameters)).</span></span> <span data-ttu-id="08560-533">Proměnná musí být jednoznačně přiřazena ([jednoznačné přiřazení](variables.md#definite-assignment)) předtím, než může být předána jako parametr reference.</span><span class="sxs-lookup"><span data-stu-id="08560-533">A variable must be definitely assigned ([Definite assignment](variables.md#definite-assignment)) before it can be passed as a reference parameter.</span></span> <span data-ttu-id="08560-534">Klíčové slovo `out` následované *variable_reference* ([odkazy na proměnné](variables.md#variable-references)) označující, že argument se předává jako výstupní parametr ([výstupní parametry](classes.md#output-parameters)).</span><span class="sxs-lookup"><span data-stu-id="08560-534">The keyword `out` followed by a *variable_reference* ([Variable references](variables.md#variable-references)), indicating that the argument is passed as an output parameter ([Output parameters](classes.md#output-parameters)).</span></span> <span data-ttu-id="08560-535">Proměnná se považuje za jednoznačně přiřazená ([jednoznačné přiřazení](variables.md#definite-assignment)) za voláním členů funkce, ve kterém je proměnná předána jako výstupní parametr.</span><span class="sxs-lookup"><span data-stu-id="08560-535">A variable is considered definitely assigned ([Definite assignment](variables.md#definite-assignment)) following a function member invocation in which the variable is passed as an output parameter.</span></span>

#### <a name="corresponding-parameters"></a><span data-ttu-id="08560-536">Odpovídající parametry</span><span class="sxs-lookup"><span data-stu-id="08560-536">Corresponding parameters</span></span>

<span data-ttu-id="08560-537">Pro každý argument v seznamu argumentů musí být příslušný parametr v členu funkce nebo vyvolán delegát.</span><span class="sxs-lookup"><span data-stu-id="08560-537">For each argument in an argument list there has to be a corresponding parameter in the function member or delegate being invoked.</span></span>

<span data-ttu-id="08560-538">Seznam parametrů použitý v následujícím příkladu je určen následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="08560-538">The parameter list used in the following is determined as follows:</span></span>

*  <span data-ttu-id="08560-539">Pro virtuální metody a indexery definované ve třídách je seznam parametrů převzat z nejvíce specifické deklarace nebo přepsání člena funkce, počínaje statickým typem příjemce a hledáním v rámci jeho základních tříd.</span><span class="sxs-lookup"><span data-stu-id="08560-539">For virtual methods and indexers defined in classes, the parameter list is picked from the most specific declaration or override of the function member, starting with the static type of the receiver, and searching through its base classes.</span></span>
*  <span data-ttu-id="08560-540">Pro metody rozhraní a indexery je seznam parametrů vybrán z konkrétní definice člena, počínaje typem rozhraní a hledáním v základních rozhraních.</span><span class="sxs-lookup"><span data-stu-id="08560-540">For interface methods and indexers, the parameter list is picked form the most specific definition of the member, starting with the interface type and searching through the base interfaces.</span></span> <span data-ttu-id="08560-541">Není-li nalezen žádný jedinečný seznam parametrů, je vytvořen seznam parametrů s nepřístupnými jmény a bez volitelných parametrů, aby vyvolání nemohlo používat pojmenované parametry nebo vynechat volitelné argumenty.</span><span class="sxs-lookup"><span data-stu-id="08560-541">If no unique parameter list is found, a parameter list with inaccessible names and no optional parameters is constructed, so that invocations cannot use named parameters or omit optional arguments.</span></span>
*  <span data-ttu-id="08560-542">Pro částečné metody je použit seznam parametrů definující deklaraci částečné metody.</span><span class="sxs-lookup"><span data-stu-id="08560-542">For partial methods, the parameter list of the defining partial method declaration is used.</span></span>
*  <span data-ttu-id="08560-543">Pro všechny ostatní členy a delegáty funkcí je pouze jeden seznam parametrů, který je použit.</span><span class="sxs-lookup"><span data-stu-id="08560-543">For all other function members and delegates there is only a single parameter list, which is the one used.</span></span>

<span data-ttu-id="08560-544">Pozice argumentu nebo parametru je definována jako počet argumentů nebo parametrů předcházejících v seznamu argumentů nebo seznamu parametrů.</span><span class="sxs-lookup"><span data-stu-id="08560-544">The position of an argument or parameter is defined as the number of arguments or parameters preceding it in the argument list or parameter list.</span></span>

<span data-ttu-id="08560-545">Odpovídající parametry pro argumenty členů funkce jsou vytvořeny následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="08560-545">The corresponding parameters for function member arguments are established as follows:</span></span>

*  <span data-ttu-id="08560-546">Argumenty v *argument_list* konstruktory instancí, metody, indexery a Delegáti:</span><span class="sxs-lookup"><span data-stu-id="08560-546">Arguments in the *argument_list* of instance constructors, methods, indexers and delegates:</span></span>
    * <span data-ttu-id="08560-547">Poziční argument, ve kterém se vyskytuje pevný parametr na stejné pozici v seznamu parametrů, odpovídá tomuto parametru.</span><span class="sxs-lookup"><span data-stu-id="08560-547">A positional argument where a fixed parameter occurs at the same position in the parameter list corresponds to that parameter.</span></span>
    * <span data-ttu-id="08560-548">Poziční argument členu funkce s polem parametrů vyvolaným v jeho normálním formuláři odpovídá poli parametrů, které se musí vyskytovat na stejné pozici v seznamu parametrů.</span><span class="sxs-lookup"><span data-stu-id="08560-548">A positional argument of a function member with a parameter array invoked in its normal form corresponds to the parameter  array, which must occur at the same position in the parameter list.</span></span>
    * <span data-ttu-id="08560-549">Poziční argument členu funkce s polem parametrů vyvolaným v rozbaleném formuláři, kde žádný pevný parametr neprobíhá na stejné pozici v seznamu parametrů, odpovídá prvku v poli parametrů.</span><span class="sxs-lookup"><span data-stu-id="08560-549">A positional argument of a function member with a parameter array invoked in its expanded form, where no fixed parameter occurs at the same position in the parameter list, corresponds to an element in the parameter array.</span></span>
    * <span data-ttu-id="08560-550">Pojmenovaný argument odpovídá parametru se stejným názvem v seznamu parametrů.</span><span class="sxs-lookup"><span data-stu-id="08560-550">A named argument corresponds to the parameter of the same name in the parameter list.</span></span>
    * <span data-ttu-id="08560-551">U indexerů při volání `set` přistupující objekt, který je zadaný jako pravý operand operátoru přiřazení, odpovídá implicitnímu `value`mu parametru deklarace přístupového objektu `set`.</span><span class="sxs-lookup"><span data-stu-id="08560-551">For indexers, when invoking the `set` accessor, the expression specified as the right operand of the assignment operator corresponds to the implicit `value` parameter of the `set` accessor declaration.</span></span>
*  <span data-ttu-id="08560-552">V případě vlastností při vyvolání přístupového objektu `get` nejsou žádné argumenty.</span><span class="sxs-lookup"><span data-stu-id="08560-552">For properties, when invoking the `get` accessor there are no arguments.</span></span> <span data-ttu-id="08560-553">Při vyvolání přístupového objektu `set` výraz zadaný jako pravý operand operátoru přiřazení odpovídá implicitnímu parametru `value` deklarace přístupového objektu `set`.</span><span class="sxs-lookup"><span data-stu-id="08560-553">When invoking the `set` accessor, the expression specified as the right operand of the assignment operator corresponds to the implicit `value` parameter of the `set` accessor declaration.</span></span>
*  <span data-ttu-id="08560-554">V případě uživatelem definovaných unárních operátorů (včetně převodů) odpovídá jeden operand jednomu parametru deklarace operátoru.</span><span class="sxs-lookup"><span data-stu-id="08560-554">For user-defined unary operators (including conversions), the single operand corresponds to the single parameter of the operator declaration.</span></span>
*  <span data-ttu-id="08560-555">V případě uživatelem definovaných binárních operátorů odpovídá levý operand prvnímu parametru a pravý operand odpovídá druhému parametru deklarace operátoru.</span><span class="sxs-lookup"><span data-stu-id="08560-555">For user-defined binary operators, the left operand corresponds to the first parameter, and the right operand corresponds to the second parameter of the operator declaration.</span></span>

#### <a name="run-time-evaluation-of-argument-lists"></a><span data-ttu-id="08560-556">Zkušební doba běhu seznamů argumentů</span><span class="sxs-lookup"><span data-stu-id="08560-556">Run-time evaluation of argument lists</span></span>

<span data-ttu-id="08560-557">Během běhu při vyvolání člena funkce ([Kontrola dynamického přetěžování při kompilaci](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) jsou výrazy nebo odkazy na proměnné seznamu argumentů vyhodnocovány v pořadí zleva doprava následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="08560-557">During the run-time processing of a function member invocation ([Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution)), the expressions or variable references of an argument list are evaluated in order, from left to right, as follows:</span></span>

*  <span data-ttu-id="08560-558">Pro parametr hodnoty je vyhodnocen výraz argumentu a je proveden implicitní převod ([implicitní převody](conversions.md#implicit-conversions)) na odpovídající typ parametru.</span><span class="sxs-lookup"><span data-stu-id="08560-558">For a value parameter, the argument expression is evaluated and an implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)) to the corresponding parameter type is performed.</span></span> <span data-ttu-id="08560-559">Výsledná hodnota se stala počáteční hodnotou parametru value ve volání členu funkce.</span><span class="sxs-lookup"><span data-stu-id="08560-559">The resulting value becomes the initial value of the value parameter in the function member invocation.</span></span>
*  <span data-ttu-id="08560-560">Pro odkaz nebo výstupní parametr je vyhodnocen odkaz na proměnnou a výsledné umístění úložiště se bude umístěním úložiště reprezentovaným parametrem ve volání člena funkce.</span><span class="sxs-lookup"><span data-stu-id="08560-560">For a reference or output parameter, the variable reference is evaluated and the resulting storage location becomes the storage location represented by the parameter in the function member invocation.</span></span> <span data-ttu-id="08560-561">Pokud je odkaz na proměnnou zadaný jako odkaz nebo výstupní parametr prvkem pole *reference_type*, je provedena kontrola za běhu, aby se zajistilo, že typ prvku pole je stejný jako typ parametru.</span><span class="sxs-lookup"><span data-stu-id="08560-561">If the variable reference given as a reference or output parameter is an array element of a *reference_type*, a run-time check is performed to ensure that the element type of the array is identical to the type of the parameter.</span></span> <span data-ttu-id="08560-562">Pokud tato kontrolu neproběhne úspěšně, je vyvolána `System.ArrayTypeMismatchException`.</span><span class="sxs-lookup"><span data-stu-id="08560-562">If this check fails, a `System.ArrayTypeMismatchException` is thrown.</span></span>

<span data-ttu-id="08560-563">Metody, indexery a konstruktory instancí mohou deklarovat svůj parametr, který je nejvíce vpravo, aby bylo pole parametrů ([pole parametrů](classes.md#parameter-arrays)).</span><span class="sxs-lookup"><span data-stu-id="08560-563">Methods, indexers, and instance constructors may declare their right-most parameter to be a parameter array ([Parameter arrays](classes.md#parameter-arrays)).</span></span> <span data-ttu-id="08560-564">Tyto členy funkce jsou vyvolány v jejich normálním tvaru nebo v rozbaleném formuláři v závislosti na tom, která z nich je použita ([příslušný člen funkce](expressions.md#applicable-function-member)):</span><span class="sxs-lookup"><span data-stu-id="08560-564">Such function members are invoked either in their normal form or in their expanded form depending on which is applicable ([Applicable function member](expressions.md#applicable-function-member)):</span></span>

*  <span data-ttu-id="08560-565">Je-li člen funkce s polem parametrů vyvolána v normálním tvaru, argument zadaný pro pole parametrů musí být jeden výraz, který je implicitně převeden ([implicitní převod](conversions.md#implicit-conversions)) na typ pole parametrů.</span><span class="sxs-lookup"><span data-stu-id="08560-565">When a function member with a parameter array is invoked in its normal form, the argument given for the parameter array must be a single expression that is implicitly convertible ([Implicit conversions](conversions.md#implicit-conversions)) to the parameter array type.</span></span> <span data-ttu-id="08560-566">V tomto případě pole parametrů funguje přesně jako parametr hodnoty.</span><span class="sxs-lookup"><span data-stu-id="08560-566">In this case, the parameter array acts precisely like a value parameter.</span></span>
*  <span data-ttu-id="08560-567">Pokud je v rozbaleném formuláři vyvolána člen funkce s polem parametrů, musí být volání zadáno nula nebo více pozičních argumentů pro pole parametrů, kde každý argument je výraz, který je implicitně převeden ([implicitní převod](conversions.md#implicit-conversions)) na typ prvku pole parametrů.</span><span class="sxs-lookup"><span data-stu-id="08560-567">When a function member with a parameter array is invoked in its expanded form, the invocation must specify zero or more positional arguments for the parameter array, where each argument is an expression that is implicitly convertible ([Implicit conversions](conversions.md#implicit-conversions)) to the element type of the parameter array.</span></span> <span data-ttu-id="08560-568">V tomto případě vyvolání vytvoří instanci typu pole parametru s délkou odpovídající počtu argumentů, inicializuje prvky instance pole pomocí daných hodnot argumentů a použije nově vytvořenou instanci pole jako aktuální. Argument.</span><span class="sxs-lookup"><span data-stu-id="08560-568">In this case, the invocation creates an instance of the parameter array type with a length corresponding to the number of arguments, initializes the elements of the array instance with the given argument values, and uses the newly created array instance as the actual argument.</span></span>

<span data-ttu-id="08560-569">Výrazy seznamu argumentů jsou vždy vyhodnocovány v pořadí, ve kterém jsou zapsány.</span><span class="sxs-lookup"><span data-stu-id="08560-569">The expressions of an argument list are always evaluated in the order they are written.</span></span> <span data-ttu-id="08560-570">Proto příklad</span><span class="sxs-lookup"><span data-stu-id="08560-570">Thus, the example</span></span>
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
<span data-ttu-id="08560-571">Vytvoří výstup</span><span class="sxs-lookup"><span data-stu-id="08560-571">produces the output</span></span>
```console
x = 0, y = 1, z = 2
x = 4, y = -1, z = 3
```

<span data-ttu-id="08560-572">Pravidla koodchylky pole ([kovariance pole](arrays.md#array-covariance)) povolují hodnotu typu pole, `A[]` být odkazem na instanci typu pole `B[]`za předpokladu, že v `B` na `A`existuje implicitní převod odkazu.</span><span class="sxs-lookup"><span data-stu-id="08560-572">The array co-variance rules ([Array covariance](arrays.md#array-covariance)) permit a value of an array type `A[]` to be a reference to an instance of an array type `B[]`, provided an implicit reference conversion exists from `B` to `A`.</span></span> <span data-ttu-id="08560-573">Z důvodu těchto pravidel platí, že když je prvek pole *reference_type* předán jako odkazový nebo výstupní parametr, je vyžadována kontrola za běhu, aby se zajistilo, že skutečný typ prvku pole je stejný jako parametr.</span><span class="sxs-lookup"><span data-stu-id="08560-573">Because of these rules, when an array element of a *reference_type* is passed as a reference or output parameter, a run-time check is required to ensure that the actual element type of the array is identical to that of the parameter.</span></span> <span data-ttu-id="08560-574">V příkladu</span><span class="sxs-lookup"><span data-stu-id="08560-574">In the example</span></span>
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
<span data-ttu-id="08560-575">druhé vyvolání `F` způsobí vyvolání `System.ArrayTypeMismatchException`, protože skutečný typ prvku `b` je `string` a není `object`.</span><span class="sxs-lookup"><span data-stu-id="08560-575">the second invocation of `F` causes a `System.ArrayTypeMismatchException` to be thrown because the actual element type of `b` is `string` and not `object`.</span></span>

<span data-ttu-id="08560-576">Když je v rozbaleném formuláři vyvolána člen funkce s polem parametrů, je volání zpracováno přesně tak, jako by byl vložen výraz vytvoření pole s inicializátorem pole ([výrazy vytvoření pole](expressions.md#array-creation-expressions)) kolem rozšířených parametrů.</span><span class="sxs-lookup"><span data-stu-id="08560-576">When a function member with a parameter array is invoked in its expanded form, the invocation is processed exactly as if an array creation expression with an array initializer ([Array creation expressions](expressions.md#array-creation-expressions)) was inserted around the expanded parameters.</span></span> <span data-ttu-id="08560-577">Například s ohledem na deklaraci</span><span class="sxs-lookup"><span data-stu-id="08560-577">For example, given the declaration</span></span>
```csharp
void F(int x, int y, params object[] args);
```
<span data-ttu-id="08560-578">následující vyvolání rozšířené formy metody</span><span class="sxs-lookup"><span data-stu-id="08560-578">the following invocations of the expanded form of the method</span></span>
```csharp
F(10, 20);
F(10, 20, 30, 40);
F(10, 20, 1, "hello", 3.0);
```
<span data-ttu-id="08560-579">odpovídat přesně na</span><span class="sxs-lookup"><span data-stu-id="08560-579">correspond exactly to</span></span>
```csharp
F(10, 20, new object[] {});
F(10, 20, new object[] {30, 40});
F(10, 20, new object[] {1, "hello", 3.0});
```

<span data-ttu-id="08560-580">Konkrétně si všimněte, že prázdné pole je vytvořeno, pokud jsou pro pole parametrů zadány nula argumentů.</span><span class="sxs-lookup"><span data-stu-id="08560-580">In particular, note that an empty array is created when there are zero arguments given for the parameter array.</span></span>

<span data-ttu-id="08560-581">Pokud jsou argumenty vynechány od členu funkce s odpovídajícími nepovinnými parametry, výchozí argumenty deklarace člena funkce budou implicitně předány.</span><span class="sxs-lookup"><span data-stu-id="08560-581">When arguments are omitted from a function member with corresponding optional parameters, the default arguments of the function member declaration are implicitly passed.</span></span> <span data-ttu-id="08560-582">Vzhledem k tomu, že jsou vždycky konstantní, jejich hodnocení nebude mít vliv na pořadí vyhodnocení zbývajících argumentů.</span><span class="sxs-lookup"><span data-stu-id="08560-582">Because these are always constant, their evaluation will not impact the evaluation order of the remaining arguments.</span></span>

### <a name="type-inference"></a><span data-ttu-id="08560-583">Odvození typu</span><span class="sxs-lookup"><span data-stu-id="08560-583">Type inference</span></span>

<span data-ttu-id="08560-584">Při volání obecné metody bez určení argumentů typu se proces ***odvození typu*** pokusí odvodit argumenty typu pro volání.</span><span class="sxs-lookup"><span data-stu-id="08560-584">When a generic method is called without specifying type arguments, a ***type inference*** process attempts to infer type arguments for the call.</span></span> <span data-ttu-id="08560-585">Přítomnost odvození typu umožňuje, aby se pro volání obecné metody používala pohodlnější syntaxe a aby programátor nezadal redundantní informace o typu.</span><span class="sxs-lookup"><span data-stu-id="08560-585">The presence of type inference allows a more convenient syntax to be used for calling a generic method, and allows the programmer to avoid specifying redundant type information.</span></span> <span data-ttu-id="08560-586">Například s ohledem na deklaraci metody:</span><span class="sxs-lookup"><span data-stu-id="08560-586">For example, given the method declaration:</span></span>
```csharp
class Chooser
{
    static Random rand = new Random();

    public static T Choose<T>(T first, T second) {
        return (rand.Next(2) == 0)? first: second;
    }
}
```
<span data-ttu-id="08560-587">je možné vyvolat metodu `Choose` bez explicitního určení argumentu typu:</span><span class="sxs-lookup"><span data-stu-id="08560-587">it is possible to invoke the `Choose` method without explicitly specifying a type argument:</span></span>
```csharp
int i = Chooser.Choose(5, 213);                 // Calls Choose<int>

string s = Chooser.Choose("foo", "bar");        // Calls Choose<string>
```

<span data-ttu-id="08560-588">Prostřednictvím odvození typu jsou argumenty typu `int` a `string` určeny z argumentů metody.</span><span class="sxs-lookup"><span data-stu-id="08560-588">Through type inference, the type arguments `int` and `string` are determined from the arguments to the method.</span></span>

<span data-ttu-id="08560-589">K odvození typu dojde jako součást zpracování volání metody v době vazby ([vyvolání metod](expressions.md#method-invocations)) a provádí se před krokem vyřešení přetížení volání.</span><span class="sxs-lookup"><span data-stu-id="08560-589">Type inference occurs as part of the binding-time processing of a method invocation ([Method invocations](expressions.md#method-invocations)) and takes place before the overload resolution step of the invocation.</span></span> <span data-ttu-id="08560-590">Pokud je konkrétní skupina metod určena v volání metody a žádné argumenty typu nejsou zadány jako součást volání metody, odvození typu je aplikováno na každou obecnou metodu ve skupině metod.</span><span class="sxs-lookup"><span data-stu-id="08560-590">When a particular method group is specified in a method invocation, and no type arguments are specified as part of the method invocation, type inference is applied to each generic method in the method group.</span></span> <span data-ttu-id="08560-591">Pokud odvození typu je úspěšné, pak se k určení typů argumentů pro následné řešení přetížení použijí argumenty odvozených typů.</span><span class="sxs-lookup"><span data-stu-id="08560-591">If type inference succeeds, then the inferred type arguments are used to determine the types of arguments for subsequent overload resolution.</span></span> <span data-ttu-id="08560-592">Pokud řešení přetížení zvolí obecnou metodu jako metodu, která má být vyvolána, pak jsou argumenty odvozeného typu použity jako skutečné argumenty typu pro vyvolání.</span><span class="sxs-lookup"><span data-stu-id="08560-592">If overload resolution chooses a generic method as the one to invoke, then the inferred type arguments are used as the actual type arguments for the invocation.</span></span> <span data-ttu-id="08560-593">Pokud je odvození typu pro určitou metodu neúspěšné, tato metoda není součástí řešení přetížení.</span><span class="sxs-lookup"><span data-stu-id="08560-593">If type inference for a particular method fails, that method does not participate in overload resolution.</span></span> <span data-ttu-id="08560-594">Selhání pro odvození typu, v a samotné, nezpůsobí chybu při vazbě.</span><span class="sxs-lookup"><span data-stu-id="08560-594">The failure of type inference, in and of itself, does not cause a binding-time error.</span></span> <span data-ttu-id="08560-595">Nicméně často vede k chybě při vazbě v případě, že řešení přetížení pak nenalezne žádné použitelné metody.</span><span class="sxs-lookup"><span data-stu-id="08560-595">However, it often leads to a binding-time error when overload resolution then fails to find any applicable methods.</span></span>

<span data-ttu-id="08560-596">Pokud se zadaný počet argumentů liší od počtu parametrů v metodě, pak se rozhraní odvození okamžitě nezdařilo.</span><span class="sxs-lookup"><span data-stu-id="08560-596">If the supplied number of arguments is different than the number of parameters in the method, then inference immediately fails.</span></span> <span data-ttu-id="08560-597">V opačném případě Předpokládejme, že obecná metoda má následující signaturu:</span><span class="sxs-lookup"><span data-stu-id="08560-597">Otherwise, assume that the generic method has the following signature:</span></span>
```csharp
Tr M<X1,...,Xn>(T1 x1, ..., Tm xm)
```

<span data-ttu-id="08560-598">Pomocí volání metody formuláře `M(E1...Em)` úlohy typu odvození typu je najít jedinečné argumenty typu `S1...Sn` pro každý parametr typu `X1...Xn` tak, že `M<S1...Sn>(E1...Em)` volání bude platné.</span><span class="sxs-lookup"><span data-stu-id="08560-598">With a method call of the form `M(E1...Em)` the task of type inference is to find unique type arguments `S1...Sn` for each of the type parameters `X1...Xn` so that the call `M<S1...Sn>(E1...Em)` becomes valid.</span></span>

<span data-ttu-id="08560-599">Během procesu odvození každého parametru typu `Xi` je buď *pevně* nastavená na konkrétní typ `Si` nebo není *pevně daná* s přidruženou sadou *hranic*.</span><span class="sxs-lookup"><span data-stu-id="08560-599">During the process of inference each type parameter `Xi` is either *fixed* to a particular type `Si` or *unfixed* with an associated set of *bounds*.</span></span> <span data-ttu-id="08560-600">Každá z mezí je typu `T`.</span><span class="sxs-lookup"><span data-stu-id="08560-600">Each of the bounds is some type `T`.</span></span> <span data-ttu-id="08560-601">Zpočátku je každá proměnná typu `Xi` neopravena s prázdnou sadou mezí.</span><span class="sxs-lookup"><span data-stu-id="08560-601">Initially each type variable `Xi` is unfixed with an empty set of bounds.</span></span>

<span data-ttu-id="08560-602">Odvození typu se provádí ve fázích.</span><span class="sxs-lookup"><span data-stu-id="08560-602">Type inference takes place in phases.</span></span> <span data-ttu-id="08560-603">Každá fáze se pokusí odvodit argumenty typu pro více proměnných typu na základě zjištění předchozí fáze.</span><span class="sxs-lookup"><span data-stu-id="08560-603">Each phase will try to infer type arguments for more type variables based on the findings of the previous phase.</span></span> <span data-ttu-id="08560-604">První fáze vytvoří několik počátečních odvození hranic, zatímco druhá fáze opraví proměnné typu na konkrétní typy a odvodí další meze.</span><span class="sxs-lookup"><span data-stu-id="08560-604">The first phase makes some initial inferences of bounds, whereas the second phase fixes type variables to specific types and infers further bounds.</span></span> <span data-ttu-id="08560-605">Druhá fáze se možná bude muset opakovat několikrát.</span><span class="sxs-lookup"><span data-stu-id="08560-605">The second phase may have to be repeated a number of times.</span></span>

<span data-ttu-id="08560-606">*Poznámka:* Odvození typu bude provedeno nejen při volání obecné metody.</span><span class="sxs-lookup"><span data-stu-id="08560-606">*Note:* Type inference takes place not only when a generic method is called.</span></span> <span data-ttu-id="08560-607">Odvození typu pro převod skupin metod je popsáno v tématu [odvození typu pro převod skupin metod](expressions.md#type-inference-for-conversion-of-method-groups) a nalezení nejlepšího společného typu sady výrazů je popsána v tématu [vyhledání nejlepšího typu](expressions.md#finding-the-best-common-type-of-a-set-of-expressions)sady výrazů.</span><span class="sxs-lookup"><span data-stu-id="08560-607">Type inference for conversion of method groups is described in [Type inference for conversion of method groups](expressions.md#type-inference-for-conversion-of-method-groups) and finding the best common type of a set of expressions is described in [Finding the best common type of a set of expressions](expressions.md#finding-the-best-common-type-of-a-set-of-expressions).</span></span>

#### <a name="the-first-phase"></a><span data-ttu-id="08560-608">První fáze</span><span class="sxs-lookup"><span data-stu-id="08560-608">The first phase</span></span>

<span data-ttu-id="08560-609">Pro každý z argumentů metody `Ei`:</span><span class="sxs-lookup"><span data-stu-id="08560-609">For each of the method arguments `Ei`:</span></span>

*   <span data-ttu-id="08560-610">Je-li `Ei` anonymní funkce, je vytvořena *explicitní odvození typu parametru* ([odvození typu explicitního typu parametru](expressions.md#explicit-parameter-type-inferences)) z `Ei` na `Ti`</span><span class="sxs-lookup"><span data-stu-id="08560-610">If `Ei` is an anonymous function, an *explicit parameter type inference* ([Explicit parameter type inferences](expressions.md#explicit-parameter-type-inferences)) is made from `Ei` to `Ti`</span></span>
*   <span data-ttu-id="08560-611">V opačném případě, pokud má `Ei` typ `U` a `xi` je parametr hodnoty, pak je *odvození dolní hranice* *od* `U` *až* po `Ti`.</span><span class="sxs-lookup"><span data-stu-id="08560-611">Otherwise, if `Ei` has a type `U` and `xi` is a value parameter then a *lower-bound inference* is made *from* `U` *to* `Ti`.</span></span>
*   <span data-ttu-id="08560-612">V opačném případě, pokud má `Ei` typ `U` a `xi` je `ref` nebo parametr `out`, pak je *ze* `U` *na* `Ti`proveden *přesný odvození* .</span><span class="sxs-lookup"><span data-stu-id="08560-612">Otherwise, if `Ei` has a type `U` and `xi` is a `ref` or `out` parameter then an *exact inference* is made *from* `U` *to* `Ti`.</span></span>
*   <span data-ttu-id="08560-613">V opačném případě není pro tento argument provedeno odvození.</span><span class="sxs-lookup"><span data-stu-id="08560-613">Otherwise, no inference is made for this argument.</span></span>


#### <a name="the-second-phase"></a><span data-ttu-id="08560-614">Druhá fáze</span><span class="sxs-lookup"><span data-stu-id="08560-614">The second phase</span></span>

<span data-ttu-id="08560-615">Druhá fáze pokračuje následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="08560-615">The second phase proceeds as follows:</span></span>

*   <span data-ttu-id="08560-616">Všechny proměnné *nefixního* typu `Xi`, které *nezávisí na* ([závislost](expressions.md#dependence)), všechny `Xj` jsou opraveny ([Oprava](expressions.md#fixing)).</span><span class="sxs-lookup"><span data-stu-id="08560-616">All *unfixed* type variables `Xi` which do not *depend on* ([Dependence](expressions.md#dependence)) any `Xj` are fixed ([Fixing](expressions.md#fixing)).</span></span>
*   <span data-ttu-id="08560-617">Pokud žádné takové proměnné typu neexistují, jsou *opraveny* všechny proměnné *nefixního* typu `Xi`, pro které jsou všechny následující podrženy:</span><span class="sxs-lookup"><span data-stu-id="08560-617">If no such type variables exist, all *unfixed* type variables `Xi` are *fixed* for which all of the following hold:</span></span>
    *   <span data-ttu-id="08560-618">Existuje alespoň jedna proměnná typu `Xj`, která závisí na `Xi`</span><span class="sxs-lookup"><span data-stu-id="08560-618">There is at least one type variable `Xj` that depends on `Xi`</span></span>
    *   <span data-ttu-id="08560-619">`Xi` má neprázdnou sadu mezí.</span><span class="sxs-lookup"><span data-stu-id="08560-619">`Xi` has a non-empty set of bounds</span></span>
*   <span data-ttu-id="08560-620">Pokud žádné takové proměnné typu neexistují a existují stále *nepevné* proměnné typu, odvození typu se nezdařilo.</span><span class="sxs-lookup"><span data-stu-id="08560-620">If no such type variables exist and there are still *unfixed* type variables, type inference fails.</span></span>
*   <span data-ttu-id="08560-621">V opačném případě, pokud žádné další *nepevné* proměnné typu existují, odvození typu je úspěšné.</span><span class="sxs-lookup"><span data-stu-id="08560-621">Otherwise, if no further *unfixed* type variables exist, type inference succeeds.</span></span>
*   <span data-ttu-id="08560-622">Jinak platí, že pro všechny argumenty `Ei` s odpovídajícím typem parametru `Ti`, kde *výstupní typy* ([výstupní typy](expressions.md#output-types)) obsahují *nepevné* proměnné typu `Xj` ale *vstupní typy* ([vstupní typy](expressions.md#input-types)) ne, *odvozený typ výstupu* ([odvození typu výstupu](expressions.md#output-type-inferences)) se provádí *z* `Ei` *až* po `Ti`.</span><span class="sxs-lookup"><span data-stu-id="08560-622">Otherwise, for all arguments `Ei` with corresponding parameter type `Ti` where the *output types* ([Output types](expressions.md#output-types)) contain *unfixed* type variables `Xj` but the *input types* ([Input types](expressions.md#input-types)) do not, an *output type inference* ([Output type inferences](expressions.md#output-type-inferences)) is made *from* `Ei` *to* `Ti`.</span></span> <span data-ttu-id="08560-623">Druhá fáze se pak opakuje.</span><span class="sxs-lookup"><span data-stu-id="08560-623">Then the second phase is repeated.</span></span>

#### <a name="input-types"></a><span data-ttu-id="08560-624">Typy vstupu</span><span class="sxs-lookup"><span data-stu-id="08560-624">Input types</span></span>

<span data-ttu-id="08560-625">Pokud `E` je skupina metod nebo implicitně typové anonymní funkce a `T` je typ delegáta nebo strom výrazů, pak všechny typy parametrů `T` jsou *vstupní typy* `E` *s typem* `T`.</span><span class="sxs-lookup"><span data-stu-id="08560-625">If `E` is a method group or implicitly typed anonymous function and `T` is a delegate type or expression tree type then all the parameter types of `T` are *input types* of `E` *with type* `T`.</span></span>

####  <a name="output-types"></a><span data-ttu-id="08560-626">Typy výstupu</span><span class="sxs-lookup"><span data-stu-id="08560-626">Output types</span></span>

<span data-ttu-id="08560-627">Pokud `E` je skupina metod nebo anonymní funkce a `T` je typ delegáta nebo typ stromu výrazů, návratový typ `T` je *výstupní typ* `E` *s typem* `T`.</span><span class="sxs-lookup"><span data-stu-id="08560-627">If `E` is a method group or an anonymous function and `T` is a delegate type or expression tree type then the return type of `T` is an *output type of* `E` *with type* `T`.</span></span>

#### <a name="dependence"></a><span data-ttu-id="08560-628">Drog</span><span class="sxs-lookup"><span data-stu-id="08560-628">Dependence</span></span>

<span data-ttu-id="08560-629">*Nepevný* typ proměnné `Xi` *závisí přímo na* proměnné nepevného typu `Xj` pokud pro některý argument `Ek` s typem `Tk` `Xj` dochází ve *vstupním typu* `Ek` s typem `Tk` a `Xi` se vyskytuje v *typu výstupu* `Ek` s typem `Tk`.</span><span class="sxs-lookup"><span data-stu-id="08560-629">An *unfixed* type variable `Xi` *depends directly on* an unfixed type variable `Xj` if for some argument `Ek` with type `Tk` `Xj` occurs in an *input type* of `Ek` with type `Tk` and `Xi` occurs in an *output type* of `Ek` with type `Tk`.</span></span>

<span data-ttu-id="08560-630">`Xj` *závisí na* `Xi`, pokud `Xj` *závisí přímo na* `Xi` nebo pokud `Xi` závisí *přímo na* `Xk` a `Xk` *závisí na* `Xj`.</span><span class="sxs-lookup"><span data-stu-id="08560-630">`Xj` *depends on* `Xi` if `Xj` *depends directly on* `Xi` or if `Xi` *depends directly on* `Xk` and `Xk` *depends on* `Xj`.</span></span> <span data-ttu-id="08560-631">Proto "závisí na" je přenosný, ale ne reflexivní uzavření "závisí přímo na".</span><span class="sxs-lookup"><span data-stu-id="08560-631">Thus "depends on" is the transitive but not reflexive closure of "depends directly on".</span></span>

#### <a name="output-type-inferences"></a><span data-ttu-id="08560-632">Odvození typu výstupu</span><span class="sxs-lookup"><span data-stu-id="08560-632">Output type inferences</span></span>

<span data-ttu-id="08560-633">*Odvození typu výstupu* je provedeno *z* výrazu `E` *do* typu `T` následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="08560-633">An *output type inference* is made *from* an expression `E` *to* a type `T` in the following way:</span></span>

*  <span data-ttu-id="08560-634">Je-li `E` anonymní funkce s odvozeným návratovým typem `U` ([odvozený návratový typ](expressions.md#inferred-return-type)) a `T` je typu delegáta nebo stromové struktury výrazu s návratovým typem `Tb`, pak *odvozená odvozená odvozená* ([odvozená odvozená](expressions.md#lower-bound-inferences)) je vytvořena *z* `U` *až* `Tb`.</span><span class="sxs-lookup"><span data-stu-id="08560-634">If `E` is an anonymous function with inferred return type  `U` ([Inferred return type](expressions.md#inferred-return-type)) and `T` is a delegate type or expression tree type with return type `Tb`, then a *lower-bound inference* ([Lower-bound inferences](expressions.md#lower-bound-inferences)) is made *from* `U` *to* `Tb`.</span></span>
*  <span data-ttu-id="08560-635">V opačném případě, *Pokud je `E`* jako skupina metod a `T` je typ delegáta nebo typ stromu výrazů s typy parametrů `T1...Tk` a návratovým typem `Tb`a řešení přetěžování `E` s typy `T1...Tk` poskytuje jedinou metodu s návratovým typem `U`, pak *z* `U` *na* `Tb`.</span><span class="sxs-lookup"><span data-stu-id="08560-635">Otherwise, if `E` is a method group and `T` is a delegate type or expression tree type with parameter types `T1...Tk` and return type `Tb`, and overload resolution of `E` with the types `T1...Tk` yields a single method with return type `U`, then a *lower-bound inference* is made *from* `U` *to* `Tb`.</span></span>
*  <span data-ttu-id="08560-636">V opačném případě, pokud `E` je výraz s typem `U`, pak je *odvozeno odvození* *z* `U` *na* `T`.</span><span class="sxs-lookup"><span data-stu-id="08560-636">Otherwise, if `E` is an expression with type `U`, then a *lower-bound inference* is made *from* `U` *to* `T`.</span></span>
*  <span data-ttu-id="08560-637">Jinak nejsou provedeny žádné odvozené.</span><span class="sxs-lookup"><span data-stu-id="08560-637">Otherwise, no inferences are made.</span></span>

#### <a name="explicit-parameter-type-inferences"></a><span data-ttu-id="08560-638">Explicitní odvození typu parametru</span><span class="sxs-lookup"><span data-stu-id="08560-638">Explicit parameter type inferences</span></span>

<span data-ttu-id="08560-639">*Explicitní odvození typu parametru* je provedeno *z* výrazu `E` *do* typu `T` následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="08560-639">An *explicit parameter type inference* is made *from* an expression `E` *to* a type `T` in the following way:</span></span>

*  <span data-ttu-id="08560-640">Pokud je `E` explicitně typové anonymní funkce s typy parametrů `U1...Uk` a `T` je typ delegáta nebo typ stromu výrazů s typy parametrů `V1...Vk` potom pro každý `Ui` je proveden *přesný odvození* ([přesně odvozené](expressions.md#exact-inferences)) *od* `Ui` *k* odpovídajícímu `Vi`.</span><span class="sxs-lookup"><span data-stu-id="08560-640">If `E` is an explicitly typed anonymous function with parameter types `U1...Uk` and `T` is a delegate type or expression tree type with parameter types `V1...Vk` then for each `Ui` an *exact inference* ([Exact inferences](expressions.md#exact-inferences)) is made *from* `Ui` *to* the corresponding `Vi`.</span></span>

#### <a name="exact-inferences"></a><span data-ttu-id="08560-641">Přesná odvozená</span><span class="sxs-lookup"><span data-stu-id="08560-641">Exact inferences</span></span>

<span data-ttu-id="08560-642">*Přesné odvození* *z* typu `U` *na* typ `V` je provedeno následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="08560-642">An *exact inference* *from* a type `U` *to* a type `V` is made as follows:</span></span>

*  <span data-ttu-id="08560-643">Je-li `V` jednou z *neopravených* `Xi` pak `U` přidána do sady přesných hranic pro `Xi`.</span><span class="sxs-lookup"><span data-stu-id="08560-643">If `V` is one of the *unfixed* `Xi` then `U` is added to the set of exact bounds for `Xi`.</span></span>

*  <span data-ttu-id="08560-644">V opačném případě nastaví `V1...Vk` a `U1...Uk` je určeno kontrolou, zda platí některý z následujících případů:</span><span class="sxs-lookup"><span data-stu-id="08560-644">Otherwise, sets `V1...Vk` and `U1...Uk` are determined by checking if any of the following cases apply:</span></span>

   *  <span data-ttu-id="08560-645">`V` je typ pole `V1[...]` a `U` je typ pole `U1[...]` stejného rozměru.</span><span class="sxs-lookup"><span data-stu-id="08560-645">`V` is an array type `V1[...]` and `U` is an array type `U1[...]`  of the same rank</span></span>
   *  <span data-ttu-id="08560-646">`V` je typ `V1?` a `U` je typ `U1?`</span><span class="sxs-lookup"><span data-stu-id="08560-646">`V` is the type `V1?` and `U` is the type `U1?`</span></span>
   *  <span data-ttu-id="08560-647">`V` je konstruovaný typ `C<V1...Vk>`a `U` je konstruovaný typ `C<U1...Uk>`</span><span class="sxs-lookup"><span data-stu-id="08560-647">`V` is a constructed type `C<V1...Vk>`and `U` is a constructed type `C<U1...Uk>`</span></span>

   <span data-ttu-id="08560-648">Pokud se některý z těchto případů použije, je *z* každého `Ui` *k* odpovídajícímu `Vi`proveden *přesný odvození* .</span><span class="sxs-lookup"><span data-stu-id="08560-648">If any of these cases apply then an *exact inference* is made *from* each `Ui` *to* the corresponding `Vi`.</span></span>

*  <span data-ttu-id="08560-649">Jinak nejsou k dispozici žádná odvozená.</span><span class="sxs-lookup"><span data-stu-id="08560-649">Otherwise no inferences are made.</span></span>

#### <a name="lower-bound-inferences"></a><span data-ttu-id="08560-650">Odvození s nižší vazbou</span><span class="sxs-lookup"><span data-stu-id="08560-650">Lower-bound inferences</span></span>

<span data-ttu-id="08560-651">*Odvození s nižší vazbou* *z* typu `U` *na* typ `V` je provedeno následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="08560-651">A *lower-bound inference* *from* a type `U` *to* a type `V` is made as follows:</span></span>

*  <span data-ttu-id="08560-652">Je-li `V` jednou z *neopravených* `Xi` pak `U` přidána do sady dolních mezí pro `Xi`.</span><span class="sxs-lookup"><span data-stu-id="08560-652">If `V` is one of the *unfixed* `Xi` then `U` is added to the set of lower bounds for `Xi`.</span></span>
*  <span data-ttu-id="08560-653">V opačném případě, pokud je `V` typ `V1?`a `U` je typ `U1?` pak je z `U1` na `V1`proveden nižší vázaný objekt pro odvození.</span><span class="sxs-lookup"><span data-stu-id="08560-653">Otherwise, if `V` is the type `V1?`and `U` is the type `U1?` then a lower bound inference is made from `U1` to `V1`.</span></span>
*  <span data-ttu-id="08560-654">V opačném případě nastaví `U1...Uk` a `V1...Vk` je určeno kontrolou, zda platí některý z následujících případů:</span><span class="sxs-lookup"><span data-stu-id="08560-654">Otherwise, sets `U1...Uk` and `V1...Vk` are determined by checking if any of the following cases apply:</span></span>
   *  <span data-ttu-id="08560-655">`V` je typ pole `V1[...]` a `U` je typ pole `U1[...]` (nebo parametr typu, jehož účinný základní typ je `U1[...]`) stejného pořadí.</span><span class="sxs-lookup"><span data-stu-id="08560-655">`V` is an array type `V1[...]` and `U` is an array type `U1[...]` (or a type parameter whose effective base type is `U1[...]`) of the same rank</span></span>
   *  <span data-ttu-id="08560-656">`V` je jedním z `IEnumerable<V1>`, `ICollection<V1>` nebo `IList<V1>` a `U` je jednorozměrné pole typu `U1[]`(nebo parametr typu, jehož účinný základní typ je `U1[]`).</span><span class="sxs-lookup"><span data-stu-id="08560-656">`V` is one of `IEnumerable<V1>`, `ICollection<V1>` or `IList<V1>` and `U` is a one-dimensional array type `U1[]`(or a type parameter whose effective base type is `U1[]`)</span></span>
   *  <span data-ttu-id="08560-657">`V` je konstruovaný typ třídy, struktury, rozhraní nebo delegáta `C<V1...Vk>` a existuje jedinečný typ `C<U1...Uk>` takový, že `U` (nebo, pokud `U` je parametr typu, jeho efektivní základní třída nebo jakýkoli člen jeho efektivní sady rozhraní) je identický, dědí z (přímo nebo nepřímo) nebo implementuje (přímo nebo nepřímo) `C<U1...Uk>`.</span><span class="sxs-lookup"><span data-stu-id="08560-657">`V` is a constructed class, struct, interface or delegate type `C<V1...Vk>` and there is a unique type `C<U1...Uk>` such that `U` (or, if `U` is a type parameter, its effective base class or any member of its effective interface set) is identical to, inherits from (directly or indirectly), or implements (directly or indirectly) `C<U1...Uk>`.</span></span>

      <span data-ttu-id="08560-658">(Omezení jedinečnosti znamená, že v případě `C<T> {} class U: C<X>, C<Y> {}`rozhraní není při odvodit od `U` k `C<T>` provedeno odvození, protože `U1` by mohlo být `X` nebo `Y`.)</span><span class="sxs-lookup"><span data-stu-id="08560-658">(The "uniqueness" restriction means that in the case interface `C<T> {} class U: C<X>, C<Y> {}`, then no inference is made when inferring from `U` to `C<T>` because `U1` could be `X` or `Y`.)</span></span>

   <span data-ttu-id="08560-659">Pokud se některý z těchto případů použije, odvození *od* každého `Ui` *k* odpovídajícímu `Vi` následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="08560-659">If any of these cases apply then an inference is made *from* each `Ui` *to* the corresponding `Vi` as follows:</span></span>

   *  <span data-ttu-id="08560-660">Pokud `Ui` není známý jako odkazový typ, bude proveden *přesný odvození*</span><span class="sxs-lookup"><span data-stu-id="08560-660">If `Ui` is not known to be a reference type then an *exact inference* is made</span></span>
   *  <span data-ttu-id="08560-661">V opačném případě, pokud je `U` typem pole, je provedena *odvození dolní hranice* .</span><span class="sxs-lookup"><span data-stu-id="08560-661">Otherwise, if `U` is an array type then a *lower-bound inference* is made</span></span>
   *  <span data-ttu-id="08560-662">V opačném případě, pokud je `V` `C<V1...Vk>` pak odvození závisí na parametru i-th typu `C`:</span><span class="sxs-lookup"><span data-stu-id="08560-662">Otherwise, if `V` is `C<V1...Vk>` then inference depends on the i-th type parameter of `C`:</span></span>
      *  <span data-ttu-id="08560-663">Pokud je kovariantní, je provedeno *odvození s nižší vazbou* .</span><span class="sxs-lookup"><span data-stu-id="08560-663">If it is covariant then a *lower-bound inference* is made.</span></span>
      *  <span data-ttu-id="08560-664">Pokud je kontravariantní, je provedeno *odvození horní meze* .</span><span class="sxs-lookup"><span data-stu-id="08560-664">If it is contravariant then an *upper-bound inference* is made.</span></span>
      *  <span data-ttu-id="08560-665">Pokud je neutrální, pak je proveden *přesný odvození* .</span><span class="sxs-lookup"><span data-stu-id="08560-665">If it is invariant then an *exact inference* is made.</span></span>
*  <span data-ttu-id="08560-666">Jinak nejsou provedeny žádné odvozené.</span><span class="sxs-lookup"><span data-stu-id="08560-666">Otherwise, no inferences are made.</span></span>

#### <a name="upper-bound-inferences"></a><span data-ttu-id="08560-667">Odvozené vazby na horní hranice</span><span class="sxs-lookup"><span data-stu-id="08560-667">Upper-bound inferences</span></span>

<span data-ttu-id="08560-668">*Odvození horní meze* *z* typu `U` *na* typ `V` je provedeno následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="08560-668">An *upper-bound inference* *from* a type `U` *to* a type `V` is made as follows:</span></span>

*  <span data-ttu-id="08560-669">Je-li `V` jednou z *neopravených* `Xi` pak `U` přidána do sady horních mezí pro `Xi`.</span><span class="sxs-lookup"><span data-stu-id="08560-669">If `V` is one of the *unfixed* `Xi` then `U` is added to the set of upper bounds for `Xi`.</span></span>
*  <span data-ttu-id="08560-670">V opačném případě nastaví `V1...Vk` a `U1...Uk` je určeno kontrolou, zda platí některý z následujících případů:</span><span class="sxs-lookup"><span data-stu-id="08560-670">Otherwise, sets `V1...Vk` and `U1...Uk` are determined by checking if any of the following cases apply:</span></span>
   *  <span data-ttu-id="08560-671">`U` je typ pole `U1[...]` a `V` je typ pole `V1[...]` stejného rozměru.</span><span class="sxs-lookup"><span data-stu-id="08560-671">`U` is an array type `U1[...]` and `V` is an array type `V1[...]` of the same rank</span></span>
   *  <span data-ttu-id="08560-672">`U` je jedním z `IEnumerable<Ue>`, `ICollection<Ue>` nebo `IList<Ue>` a `V` je typ jednorozměrného pole `Ve[]`</span><span class="sxs-lookup"><span data-stu-id="08560-672">`U` is one of `IEnumerable<Ue>`, `ICollection<Ue>` or `IList<Ue>` and `V` is a one-dimensional array type `Ve[]`</span></span>
   *  <span data-ttu-id="08560-673">`U` je typ `U1?` a `V` je typ `V1?`</span><span class="sxs-lookup"><span data-stu-id="08560-673">`U` is the type `U1?` and `V` is the type `V1?`</span></span>
   *  <span data-ttu-id="08560-674">`U` je konstruovaná třída, struktura, rozhraní nebo typ delegáta `C<U1...Uk>` a `V` je třída, struktura, rozhraní nebo typ delegáta, který je totožný s, dědí z (přímo nebo nepřímo) nebo implementuje (přímo nebo nepřímo) jedinečný typ `C<V1...Vk>`</span><span class="sxs-lookup"><span data-stu-id="08560-674">`U` is constructed class, struct, interface or delegate type `C<U1...Uk>` and `V` is a class, struct, interface or delegate type which is identical to, inherits from (directly or indirectly), or implements (directly or indirectly) a unique type `C<V1...Vk>`</span></span>

      <span data-ttu-id="08560-675">(Omezení jedinečnosti znamená, že pokud `interface C<T>{} class V<Z>: C<X<Z>>, C<Y<Z>>{}`, pak se při odvozování od `C<U1>` k `V<Q>`neprovádí žádné odvození.</span><span class="sxs-lookup"><span data-stu-id="08560-675">(The "uniqueness" restriction means that if we have `interface C<T>{} class V<Z>: C<X<Z>>, C<Y<Z>>{}`, then no inference is made when inferring from `C<U1>` to `V<Q>`.</span></span> <span data-ttu-id="08560-676">Odvození není provedeno z `U1` buď do `X<Q>` nebo `Y<Q>`.)</span><span class="sxs-lookup"><span data-stu-id="08560-676">Inferences are not made from `U1` to either `X<Q>` or `Y<Q>`.)</span></span>

   <span data-ttu-id="08560-677">Pokud se některý z těchto případů použije, odvození *od* každého `Ui` *k* odpovídajícímu `Vi` následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="08560-677">If any of these cases apply then an inference is made *from* each `Ui` *to* the corresponding `Vi` as follows:</span></span>
   *  <span data-ttu-id="08560-678">Pokud `Ui` není známý jako odkazový typ, bude proveden *přesný odvození*</span><span class="sxs-lookup"><span data-stu-id="08560-678">If  `Ui` is not known to be a reference type then an *exact inference* is made</span></span>
   *  <span data-ttu-id="08560-679">V opačném případě, pokud je `V` typem pole, je provedena *odvození horní meze* .</span><span class="sxs-lookup"><span data-stu-id="08560-679">Otherwise, if `V` is an array type then an *upper-bound inference* is made</span></span>
   *  <span data-ttu-id="08560-680">V opačném případě, pokud je `U` `C<U1...Uk>` pak odvození závisí na parametru i-th typu `C`:</span><span class="sxs-lookup"><span data-stu-id="08560-680">Otherwise, if `U` is `C<U1...Uk>` then inference depends on the i-th type parameter of `C`:</span></span>
      *  <span data-ttu-id="08560-681">Pokud je kovariantní, pak se provede *odvození horní meze* .</span><span class="sxs-lookup"><span data-stu-id="08560-681">If it is covariant then an *upper-bound inference* is made.</span></span>
      *  <span data-ttu-id="08560-682">Pokud je kontravariantní, je provedeno *odvození s nižší vazbou* .</span><span class="sxs-lookup"><span data-stu-id="08560-682">If it is contravariant then a *lower-bound inference* is made.</span></span>
      *  <span data-ttu-id="08560-683">Pokud je neutrální, pak je proveden *přesný odvození* .</span><span class="sxs-lookup"><span data-stu-id="08560-683">If it is invariant then an *exact inference* is made.</span></span>
*  <span data-ttu-id="08560-684">Jinak nejsou provedeny žádné odvozené.</span><span class="sxs-lookup"><span data-stu-id="08560-684">Otherwise, no inferences are made.</span></span>   

#### <a name="fixing"></a><span data-ttu-id="08560-685">Opravě</span><span class="sxs-lookup"><span data-stu-id="08560-685">Fixing</span></span>

<span data-ttu-id="08560-686">*Nepevná* proměnná typu `Xi` se sadou hranic je *opravena* následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="08560-686">An *unfixed* type variable `Xi` with a set of bounds is *fixed* as follows:</span></span>

*  <span data-ttu-id="08560-687">Sada *typů kandidátů* `Uj` začíná jako sada všech typů v sadě mezí pro `Xi`.</span><span class="sxs-lookup"><span data-stu-id="08560-687">The set of *candidate types* `Uj` starts out as the set of all types in the set of bounds for `Xi`.</span></span>
*  <span data-ttu-id="08560-688">Následně prověříme všechny vazby pro `Xi`: pro každý přesný vázaný `U` `Xi` všechny typy `Uj`, které nejsou identické s `U` jsou ze sady kandidátů odebrány.</span><span class="sxs-lookup"><span data-stu-id="08560-688">We then examine each bound for `Xi` in turn: For each exact bound `U` of `Xi` all types `Uj` which are not identical to `U` are removed from the candidate set.</span></span> <span data-ttu-id="08560-689">Pro každý dolní vázaný `U` `Xi` všechny typy `Uj` na *které není implicitní* převod z `U` ze sady kandidátů odebrán.</span><span class="sxs-lookup"><span data-stu-id="08560-689">For each lower bound `U` of `Xi` all types `Uj` to which there is *not* an implicit conversion from `U` are removed from the candidate set.</span></span> <span data-ttu-id="08560-690">Pro každý horní vázaný `U` `Xi` všechny typy `Uj`, ze *kterých není implicitní* převod na `U` ze sady kandidátů odebrán.</span><span class="sxs-lookup"><span data-stu-id="08560-690">For each upper bound `U` of `Xi` all types `Uj` from which there is *not* an implicit conversion to `U` are removed from the candidate set.</span></span>
*  <span data-ttu-id="08560-691">Pokud je mezi zbývajícími typy kandidátů `Uj` jedinečný typ `V`, ze kterého existuje implicitní převod na všechny jiné typy kandidátů, pak je `Xi` na `V`vyřešen.</span><span class="sxs-lookup"><span data-stu-id="08560-691">If among the remaining candidate types `Uj` there is a unique type `V` from which there is an implicit conversion to all the other candidate types, then `Xi` is fixed to `V`.</span></span>
*  <span data-ttu-id="08560-692">V opačném případě se odvození typu nezdařilo.</span><span class="sxs-lookup"><span data-stu-id="08560-692">Otherwise, type inference fails.</span></span>

#### <a name="inferred-return-type"></a><span data-ttu-id="08560-693">Odvozený návratový typ</span><span class="sxs-lookup"><span data-stu-id="08560-693">Inferred return type</span></span>

<span data-ttu-id="08560-694">Odvozený návratový typ anonymní funkce `F` se používá při odvozování typů a řešení přetížení.</span><span class="sxs-lookup"><span data-stu-id="08560-694">The inferred return type of an anonymous function `F` is used during type inference and overload resolution.</span></span> <span data-ttu-id="08560-695">Odvozený návratový typ lze určit pouze pro anonymní funkci, kde jsou známy všechny typy parametrů, buď protože jsou explicitně uděleny, poskytnuté prostřednictvím konverze anonymní funkce nebo odvozené při odvozování typu v nadřazeném obecném prvku. volání metody.</span><span class="sxs-lookup"><span data-stu-id="08560-695">The inferred return type can only be determined for an anonymous function where all parameter types are known, either because they are explicitly given, provided through an anonymous function conversion or inferred during type inference on an enclosing generic method invocation.</span></span>

<span data-ttu-id="08560-696">***Odvozený typ výsledku*** je určen následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="08560-696">The ***inferred result type*** is determined as follows:</span></span>

*  <span data-ttu-id="08560-697">Pokud je text `F` *výraz* , který má typ, pak odvozený typ výsledku `F` je typ tohoto výrazu.</span><span class="sxs-lookup"><span data-stu-id="08560-697">If the body of `F` is an *expression* that has a type, then the inferred result type of `F` is the type of that expression.</span></span>
*  <span data-ttu-id="08560-698">Pokud je tělo `F` *blok* a sada výrazů v příkazech `return` bloku má nejlepší běžný typ `T` ([hledání nejlepšího společného typu sady výrazů](expressions.md#finding-the-best-common-type-of-a-set-of-expressions)), pak odvozený typ výsledku `F` je `T`.</span><span class="sxs-lookup"><span data-stu-id="08560-698">If the body of `F` is a *block* and the set of expressions in the block's `return` statements has a best common type `T` ([Finding the best common type of a set of expressions](expressions.md#finding-the-best-common-type-of-a-set-of-expressions)), then the inferred result type of `F` is `T`.</span></span>
*  <span data-ttu-id="08560-699">V opačném případě nelze typ výsledku odvodit pro `F`.</span><span class="sxs-lookup"><span data-stu-id="08560-699">Otherwise, a result type cannot be inferred for `F`.</span></span>

<span data-ttu-id="08560-700">***Odvozený návratový typ*** je určen následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="08560-700">The ***inferred return type*** is determined as follows:</span></span>

*  <span data-ttu-id="08560-701">Pokud je `F` asynchronní a tělo `F` buď výraz klasifikovaný jako Nothing ([klasifikace výrazů](expressions.md#expression-classifications)), nebo blok příkazu, pokud žádné návratové příkazy nemají výrazy, odvozený návratový typ je `System.Threading.Tasks.Task`</span><span class="sxs-lookup"><span data-stu-id="08560-701">If `F` is async and the body of `F` is either an expression classified as nothing ([Expression classifications](expressions.md#expression-classifications)), or a statement block where no return statements have expressions, the inferred return type is `System.Threading.Tasks.Task`</span></span>
*  <span data-ttu-id="08560-702">Pokud je `F` asynchronní a má `T`odvozený typ výsledku, odvozený návratový typ je `System.Threading.Tasks.Task<T>`.</span><span class="sxs-lookup"><span data-stu-id="08560-702">If `F` is async and has an inferred result type `T`, the inferred return type is `System.Threading.Tasks.Task<T>`.</span></span>
*  <span data-ttu-id="08560-703">Pokud je `F` neasynchronní a má `T`odvozený typ výsledku, odvozený návratový typ je `T`.</span><span class="sxs-lookup"><span data-stu-id="08560-703">If `F` is non-async and has an inferred result type `T`, the inferred return type is `T`.</span></span>
*  <span data-ttu-id="08560-704">Jinak nelze odvodit návratový typ pro `F`.</span><span class="sxs-lookup"><span data-stu-id="08560-704">Otherwise a return type cannot be inferred for `F`.</span></span>

<span data-ttu-id="08560-705">Jako příklad odvození typu zahrnujícího anonymní funkce zvažte `Select` metodu rozšíření deklarovanou v `System.Linq.Enumerable` třídě:</span><span class="sxs-lookup"><span data-stu-id="08560-705">As an example of type inference involving anonymous functions, consider the `Select` extension method declared in the `System.Linq.Enumerable` class:</span></span>
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

<span data-ttu-id="08560-706">Za předpokladu, že obor názvů `System.Linq` byl importován s klauzulí `using` a pro třídu `Customer` s `Name` vlastností typu `string`, lze metodu `Select` použít k výběru názvů seznamu zákazníků:</span><span class="sxs-lookup"><span data-stu-id="08560-706">Assuming the `System.Linq` namespace was imported with a `using` clause, and given a class `Customer` with a `Name` property of type `string`, the `Select` method can be used to select the names of a list of customers:</span></span>
```csharp
List<Customer> customers = GetCustomerList();
IEnumerable<string> names = customers.Select(c => c.Name);
```

<span data-ttu-id="08560-707">Volání rozšiřující metody ([vyvolání rozšiřujících metod](expressions.md#extension-method-invocations)) `Select` se zpracovává přepsáním vyvolání pro statickou metodu volání:</span><span class="sxs-lookup"><span data-stu-id="08560-707">The extension method invocation ([Extension method invocations](expressions.md#extension-method-invocations)) of `Select` is processed by rewriting the invocation to a static method invocation:</span></span>
```csharp
IEnumerable<string> names = Enumerable.Select(customers, c => c.Name);
```

<span data-ttu-id="08560-708">Vzhledem k tomu, že argumenty typu nebyly explicitně zadány, použije se odvození typu k odvození argumentů typu.</span><span class="sxs-lookup"><span data-stu-id="08560-708">Since type arguments were not explicitly specified, type inference is used to infer the type arguments.</span></span> <span data-ttu-id="08560-709">Nejprve je argument `customers` spojen s parametrem `source`, odvozování `T` `Customer`.</span><span class="sxs-lookup"><span data-stu-id="08560-709">First, the `customers` argument is related to the `source` parameter, inferring `T` to be `Customer`.</span></span> <span data-ttu-id="08560-710">Pak pomocí procesu odvozování anonymního typu funkce, který je popsaný výše, `c` je předaný typ `Customer`a výraz `c.Name` souvisí s návratovým typem `selector` parametru, který odvozuje `S` `string`.</span><span class="sxs-lookup"><span data-stu-id="08560-710">Then, using the anonymous function type inference process described above, `c` is given type `Customer`, and the expression `c.Name` is related to the return type of the `selector` parameter, inferring `S` to be `string`.</span></span> <span data-ttu-id="08560-711">Proto je vyvolání ekvivalentní</span><span class="sxs-lookup"><span data-stu-id="08560-711">Thus, the invocation is equivalent to</span></span>
```csharp
Sequence.Select<Customer,string>(customers, (Customer c) => c.Name)
```
<span data-ttu-id="08560-712">a výsledek je typu `IEnumerable<string>`.</span><span class="sxs-lookup"><span data-stu-id="08560-712">and the result is of type `IEnumerable<string>`.</span></span>

<span data-ttu-id="08560-713">Následující příklad ukazuje, jak odvození typu anonymní funkce umožňuje informace typu "Flow" mezi argumenty v rámci obecného volání metody.</span><span class="sxs-lookup"><span data-stu-id="08560-713">The following example demonstrates how anonymous function type inference allows type information to "flow" between arguments in a generic method invocation.</span></span> <span data-ttu-id="08560-714">Zadaná metoda:</span><span class="sxs-lookup"><span data-stu-id="08560-714">Given the method:</span></span>
```csharp
static Z F<X,Y,Z>(X value, Func<X,Y> f1, Func<Y,Z> f2) {
    return f2(f1(value));
}
```

<span data-ttu-id="08560-715">Odvození typu pro vyvolání:</span><span class="sxs-lookup"><span data-stu-id="08560-715">Type inference for the invocation:</span></span>
```csharp
double seconds = F("1:15:30", s => TimeSpan.Parse(s), t => t.TotalSeconds);
```
<span data-ttu-id="08560-716">pokračuje následujícím způsobem: nejprve argument `"1:15:30"` souvisí s parametrem `value`, odvozování `X` `string`.</span><span class="sxs-lookup"><span data-stu-id="08560-716">proceeds as follows: First, the argument `"1:15:30"` is related to the `value` parameter, inferring `X` to be `string`.</span></span> <span data-ttu-id="08560-717">Pak parametr první anonymní funkce, `s`, má odvozený typ `string`a výraz `TimeSpan.Parse(s)` se vztahuje na návratový typ `f1`, odvodit `Y` `System.TimeSpan`.</span><span class="sxs-lookup"><span data-stu-id="08560-717">Then, the parameter of the first anonymous function, `s`, is given the inferred type `string`, and the expression `TimeSpan.Parse(s)` is related to the return type of `f1`, inferring `Y` to be `System.TimeSpan`.</span></span> <span data-ttu-id="08560-718">Nakonec parametr druhé anonymní funkce, `t`, má odvozený typ `System.TimeSpan`a výraz `t.TotalSeconds` se vztahuje k návratový typ `f2`, odvozuje `Z` `double`.</span><span class="sxs-lookup"><span data-stu-id="08560-718">Finally, the parameter of the second anonymous function, `t`, is given the inferred type `System.TimeSpan`, and the expression `t.TotalSeconds` is related to the return type of `f2`, inferring `Z` to be `double`.</span></span> <span data-ttu-id="08560-719">Proto je výsledek vyvolání typu `double`.</span><span class="sxs-lookup"><span data-stu-id="08560-719">Thus, the result of the invocation is of type `double`.</span></span>

#### <a name="type-inference-for-conversion-of-method-groups"></a><span data-ttu-id="08560-720">Odvození typu pro převod skupin metod</span><span class="sxs-lookup"><span data-stu-id="08560-720">Type inference for conversion of method groups</span></span>

<span data-ttu-id="08560-721">Podobně jako u volání obecných metod musí být odvození typu také použito, pokud je skupina metod `M` obsahující obecnou metodu převedena na daný typ delegáta `D` ([Převod skupin metod](conversions.md#method-group-conversions)).</span><span class="sxs-lookup"><span data-stu-id="08560-721">Similar to calls of generic methods, type inference must also be applied when a method group `M` containing a generic method is converted to a given delegate type `D` ([Method group conversions](conversions.md#method-group-conversions)).</span></span> <span data-ttu-id="08560-722">Daná metoda</span><span class="sxs-lookup"><span data-stu-id="08560-722">Given a method</span></span>
```csharp
Tr M<X1...Xn>(T1 x1 ... Tm xm)
```
<span data-ttu-id="08560-723">a skupina metod `M` přiřadí typu delegáta `D` úlohy typu odvození typu je najít argumenty typu `S1...Sn` tak, aby výraz:</span><span class="sxs-lookup"><span data-stu-id="08560-723">and the method group `M` being assigned to the delegate type `D` the task of type inference is to find type arguments `S1...Sn` so that the expression:</span></span>
```csharp
M<S1...Sn>
```
<span data-ttu-id="08560-724">se bude kompatibilní ([deklarace delegátů](delegates.md#delegate-declarations)) s `D`.</span><span class="sxs-lookup"><span data-stu-id="08560-724">becomes compatible ([Delegate declarations](delegates.md#delegate-declarations)) with `D`.</span></span>

<span data-ttu-id="08560-725">Na rozdíl od algoritmu odvození typu pro volání obecných metod, v tomto případě jsou k dispozici pouze *typy*argumentů, žádné *výrazy*argumentů.</span><span class="sxs-lookup"><span data-stu-id="08560-725">Unlike the type inference algorithm for generic method calls, in this case there are only argument *types*, no argument *expressions*.</span></span> <span data-ttu-id="08560-726">Konkrétně nejsou k dispozici žádné anonymní funkce, a proto není potřeba více fází odvození.</span><span class="sxs-lookup"><span data-stu-id="08560-726">In particular, there are no anonymous functions and hence no need for multiple phases of inference.</span></span>

<span data-ttu-id="08560-727">Místo *toho se všechny* `Xi` považují za *neopravené*a *z* každého typu argumentu `Uj` `D` *k* odpovídajícímu typu parametru `Tj` `M`.</span><span class="sxs-lookup"><span data-stu-id="08560-727">Instead, all `Xi` are considered *unfixed*, and a *lower-bound inference* is made *from* each argument type `Uj` of `D` *to* the corresponding parameter type `Tj` of `M`.</span></span> <span data-ttu-id="08560-728">Pokud nebyla nalezena žádná z `Xi` žádné meze, odvození typu se nezdařilo.</span><span class="sxs-lookup"><span data-stu-id="08560-728">If for any of the `Xi` no bounds were found, type inference fails.</span></span> <span data-ttu-id="08560-729">V opačném případě jsou všechny `Xi` *vyřešeny* odpovídajícím `Si`, které jsou výsledkem odvození typu.</span><span class="sxs-lookup"><span data-stu-id="08560-729">Otherwise, all `Xi` are *fixed* to corresponding `Si`, which are the result of type inference.</span></span>

#### <a name="finding-the-best-common-type-of-a-set-of-expressions"></a><span data-ttu-id="08560-730">Hledání nejlepšího běžného typu sady výrazů</span><span class="sxs-lookup"><span data-stu-id="08560-730">Finding the best common type of a set of expressions</span></span>

<span data-ttu-id="08560-731">V některých případech je třeba odvodit společný typ pro sadu výrazů.</span><span class="sxs-lookup"><span data-stu-id="08560-731">In some cases, a common type needs to be inferred for a set of expressions.</span></span> <span data-ttu-id="08560-732">Konkrétně jsou v tomto způsobu nalezeny typy prvků implicitně typovaného pole a návratové typy anonymních funkcí s poli *Block* .</span><span class="sxs-lookup"><span data-stu-id="08560-732">In particular, the element types of implicitly typed arrays and the return types of anonymous functions with *block* bodies are found in this way.</span></span>

<span data-ttu-id="08560-733">V intuitivním případě s ohledem na sadu výrazů `E1...Em` by toto odvození mělo být ekvivalentní volání metody.</span><span class="sxs-lookup"><span data-stu-id="08560-733">Intuitively, given a set of expressions `E1...Em` this inference should be equivalent to calling a method</span></span>
```csharp
Tr M<X>(X x1 ... X xm)
```
<span data-ttu-id="08560-734">s `Ei` jako argumenty.</span><span class="sxs-lookup"><span data-stu-id="08560-734">with the `Ei` as arguments.</span></span>

<span data-ttu-id="08560-735">Odvození je přesnější a začíná *nepevným* typem proměnné `X`.</span><span class="sxs-lookup"><span data-stu-id="08560-735">More precisely, the inference starts out with an *unfixed* type variable `X`.</span></span> <span data-ttu-id="08560-736">*Odvození typu výstupu* pak *z* každého `Ei` *k* `X`.</span><span class="sxs-lookup"><span data-stu-id="08560-736">*Output type inferences* are then made *from* each `Ei` *to* `X`.</span></span> <span data-ttu-id="08560-737">Nakonec je `X` *opravena* a v případě úspěchu je výsledný typ `S` výsledným nejlépe běžným typem pro výrazy.</span><span class="sxs-lookup"><span data-stu-id="08560-737">Finally, `X` is *fixed* and, if successful, the resulting type `S` is the resulting best common type for the expressions.</span></span> <span data-ttu-id="08560-738">Pokud žádný takový `S` neexistuje, výrazy nemají žádný nejlepší běžný typ.</span><span class="sxs-lookup"><span data-stu-id="08560-738">If no such `S` exists, the expressions have no best common type.</span></span>

### <a name="overload-resolution"></a><span data-ttu-id="08560-739">Rozlišení přetěžování</span><span class="sxs-lookup"><span data-stu-id="08560-739">Overload resolution</span></span>

<span data-ttu-id="08560-740">Rozlišení přetěžování je mechanizmus pro dobu vazby pro výběr nejlepšího člena funkce pro vyvolání daného seznamu argumentů a sady kandidátních členů funkce.</span><span class="sxs-lookup"><span data-stu-id="08560-740">Overload resolution is a binding-time mechanism for selecting the best function member to invoke given an argument list and a set of candidate function members.</span></span> <span data-ttu-id="08560-741">Řešení přetížení vybere člena funkce k vyvolání v následujících odlišných kontextech v rámci C#:</span><span class="sxs-lookup"><span data-stu-id="08560-741">Overload resolution selects the function member to invoke in the following distinct contexts within C#:</span></span>

*  <span data-ttu-id="08560-742">Vyvolání metody s názvem v *invocation_expression* ([vyvolání metod](expressions.md#method-invocations)).</span><span class="sxs-lookup"><span data-stu-id="08560-742">Invocation of a method named in an *invocation_expression* ([Method invocations](expressions.md#method-invocations)).</span></span>
*  <span data-ttu-id="08560-743">Vyvolání konstruktoru instance s názvem v *object_creation_expression* ([výrazy pro vytváření objektů](expressions.md#object-creation-expressions)).</span><span class="sxs-lookup"><span data-stu-id="08560-743">Invocation of an instance constructor named in an *object_creation_expression* ([Object creation expressions](expressions.md#object-creation-expressions)).</span></span>
*  <span data-ttu-id="08560-744">Vyvolání přístupového objektu indexeru prostřednictvím *element_access* ([přístup k elementu](expressions.md#element-access)).</span><span class="sxs-lookup"><span data-stu-id="08560-744">Invocation of an indexer accessor through an *element_access* ([Element access](expressions.md#element-access)).</span></span>
*  <span data-ttu-id="08560-745">Vyvolání předdefinovaného uživatelem definovaného operátoru, na který se odkazuje ve výrazu ([rozlišení přetížení unárního](expressions.md#unary-operator-overload-resolution) operátoru a [rozlišení přetížení binárního operátoru](expressions.md#binary-operator-overload-resolution)).</span><span class="sxs-lookup"><span data-stu-id="08560-745">Invocation of a predefined or user-defined operator referenced in an expression ([Unary operator overload resolution](expressions.md#unary-operator-overload-resolution) and [Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)).</span></span>

<span data-ttu-id="08560-746">Každý z těchto kontextů definuje sadu kandidátních členů a seznam argumentů vlastním jedinečným způsobem, jak je popsáno podrobněji v částech uvedených výše.</span><span class="sxs-lookup"><span data-stu-id="08560-746">Each of these contexts defines the set of candidate function members and the list of arguments in its own unique way, as described in detail in the sections listed above.</span></span> <span data-ttu-id="08560-747">Například sada kandidátů pro vyvolání metody nezahrnuje metody označené `override` ([vyhledávání členů](expressions.md#member-lookup)) a metody v základní třídě nejsou kandidáty, pokud je libovolná metoda v odvozené třídě ([vyvolání metod](expressions.md#method-invocations)).</span><span class="sxs-lookup"><span data-stu-id="08560-747">For example, the set of candidates for a method invocation does not include methods marked `override` ([Member lookup](expressions.md#member-lookup)), and methods in a base class are not candidates if any method in a derived class is applicable ([Method invocations](expressions.md#method-invocations)).</span></span>

<span data-ttu-id="08560-748">Po identifikaci členů kandidáta funkce a seznamu argumentů je výběr nejlepšího člena funkce stejný ve všech případech:</span><span class="sxs-lookup"><span data-stu-id="08560-748">Once the candidate function members and the argument list have been identified, the selection of the best function member is the same in all cases:</span></span>

*  <span data-ttu-id="08560-749">Vzhledem k sadě příslušných členů kandidátních funkcí se nachází nejlepší člen funkce v této sadě.</span><span class="sxs-lookup"><span data-stu-id="08560-749">Given the set of applicable candidate function members, the best function member in that set is located.</span></span> <span data-ttu-id="08560-750">Pokud sada obsahuje pouze jeden člen funkce, pak je tento člen funkce nejlepším členem funkce.</span><span class="sxs-lookup"><span data-stu-id="08560-750">If the set contains only one function member, then that function member is the best function member.</span></span> <span data-ttu-id="08560-751">V opačném případě je nejlepším členem funkce jeden člen funkce, který je lepší než všechny ostatní členy funkce s ohledem na daný seznam argumentů za předpokladu, že každý člen funkce je porovnán se všemi ostatními členy funkce pomocí pravidel v [lepším členu funkce](expressions.md#better-function-member).</span><span class="sxs-lookup"><span data-stu-id="08560-751">Otherwise, the best function member is the one function member that is better than all other function members with respect to the given argument list, provided that each function member is compared to all other function members using the rules in [Better function member](expressions.md#better-function-member).</span></span> <span data-ttu-id="08560-752">Pokud není k dispozici přesně jeden člen funkce, který je lepší než všechny ostatní členy funkce, pak dojde k nejednoznačnému vyvolání člena funkce a dojde k chybě při vazbě.</span><span class="sxs-lookup"><span data-stu-id="08560-752">If there is not exactly one function member that is better than all other function members, then the function member invocation is ambiguous and a binding-time error occurs.</span></span>

<span data-ttu-id="08560-753">V následujících oddílech jsou definovány přesné významy ***platných členů funkce*** a ***lepšího člena funkce***.</span><span class="sxs-lookup"><span data-stu-id="08560-753">The following sections define the exact meanings of the terms ***applicable function member*** and ***better function member***.</span></span>

#### <a name="applicable-function-member"></a><span data-ttu-id="08560-754">Příslušný člen funkce</span><span class="sxs-lookup"><span data-stu-id="08560-754">Applicable function member</span></span>

<span data-ttu-id="08560-755">Člen funkce je označován jako ***příslušný člen funkce*** s ohledem na seznam argumentů `A`, pokud jsou splněny všechny následující podmínky:</span><span class="sxs-lookup"><span data-stu-id="08560-755">A function member is said to be an ***applicable function member*** with respect to an argument list `A` when all of the following are true:</span></span>

*  <span data-ttu-id="08560-756">Každý argument v `A` odpovídá parametru v deklaraci členu funkce, jak je popsáno v [odpovídajících parametrech](expressions.md#corresponding-parameters), a jakýkoli parametr, na který žádný argument neodpovídá. je volitelným parametrem.</span><span class="sxs-lookup"><span data-stu-id="08560-756">Each argument in `A` corresponds to a parameter in the function member declaration as described in [Corresponding parameters](expressions.md#corresponding-parameters), and any parameter to which no argument corresponds is an optional parameter.</span></span>
*  <span data-ttu-id="08560-757">Pro každý argument v `A`je parametr předávaného argumentu argumentu (tj. Value, `ref`nebo `out`) shodný s parametrem předávání odpovídajícího parametru a</span><span class="sxs-lookup"><span data-stu-id="08560-757">For each argument in `A`, the parameter passing mode of the argument (i.e., value, `ref`, or `out`) is identical to the parameter passing mode of the corresponding parameter, and</span></span>
   *  <span data-ttu-id="08560-758">pro parametr hodnoty nebo pole parametrů existuje implicitní převod ([implicitní převody](conversions.md#implicit-conversions)) z argumentu na typ odpovídajícího parametru nebo</span><span class="sxs-lookup"><span data-stu-id="08560-758">for a value parameter or a parameter array, an implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)) exists from the argument to the type of the corresponding parameter, or</span></span>
   *  <span data-ttu-id="08560-759">pro parametr `ref` nebo `out` je typ argumentu totožný s typem odpovídajícího parametru.</span><span class="sxs-lookup"><span data-stu-id="08560-759">for a `ref` or `out` parameter, the type of the argument is identical to the type of the corresponding parameter.</span></span> <span data-ttu-id="08560-760">Po všech případech je parametr `ref` nebo `out` alias pro předaný argument.</span><span class="sxs-lookup"><span data-stu-id="08560-760">After all, a `ref` or `out` parameter is an alias for the argument passed.</span></span>

<span data-ttu-id="08560-761">Pro člena funkce, který obsahuje pole parametrů, pokud je člen funkce použitelný výše uvedenými pravidly, říká se, že se má použít v ***normální podobě***.</span><span class="sxs-lookup"><span data-stu-id="08560-761">For a function member that includes a parameter array, if the function member is applicable by the above rules, it is said to be applicable in its ***normal form***.</span></span> <span data-ttu-id="08560-762">Pokud člen funkce, který obsahuje pole parametrů, není použitelný v normálním tvaru, člen funkce může být použit v ***rozbaleném formátu***:</span><span class="sxs-lookup"><span data-stu-id="08560-762">If a function member that includes a parameter array is not applicable in its normal form, the function member may instead be applicable in its ***expanded form***:</span></span>

*  <span data-ttu-id="08560-763">Rozbalený formulář je vytvořen nahrazením pole parametrů v deklaraci členu funkce s nulovými nebo více parametry hodnot typu prvku pole parametrů tak, aby počet argumentů v seznamu argumentů `A` odpovídat celkovému počtu parametrů.</span><span class="sxs-lookup"><span data-stu-id="08560-763">The expanded form is constructed by replacing the parameter array in the function member declaration with zero or more value parameters of the element type of the parameter array such that the number of arguments in the argument list `A` matches the total number of parameters.</span></span> <span data-ttu-id="08560-764">Pokud má `A` méně argumentů než počet pevných parametrů v deklaraci členu funkce, rozbalená forma členu funkce nemůže být vytvořena a není proto platná.</span><span class="sxs-lookup"><span data-stu-id="08560-764">If `A` has fewer arguments than the number of fixed parameters in the function member declaration, the expanded form of the function member cannot be constructed and is thus not applicable.</span></span>
*  <span data-ttu-id="08560-765">V opačném případě je rozšířený formulář použitelný, pokud pro každý argument v `A` režim předávání parametru argumentu je totožný s parametrem předávání odpovídajícího parametru a</span><span class="sxs-lookup"><span data-stu-id="08560-765">Otherwise, the expanded form is applicable if for each argument in `A` the parameter passing mode of the argument is identical to the parameter passing mode of the corresponding parameter, and</span></span>
   *  <span data-ttu-id="08560-766">pro parametr pevné hodnoty nebo parametr hodnoty, který byl vytvořen rozšířením, existuje implicitní převod ([implicitní převod](conversions.md#implicit-conversions)) z typu argumentu na typ odpovídajícího parametru nebo</span><span class="sxs-lookup"><span data-stu-id="08560-766">for a fixed value parameter or a value parameter created by the expansion, an implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)) exists from the type of the argument to the type of the corresponding parameter, or</span></span>
   *  <span data-ttu-id="08560-767">pro parametr `ref` nebo `out` je typ argumentu totožný s typem odpovídajícího parametru.</span><span class="sxs-lookup"><span data-stu-id="08560-767">for a `ref` or `out` parameter, the type of the argument is identical to the type of the corresponding parameter.</span></span>

#### <a name="better-function-member"></a><span data-ttu-id="08560-768">Lepší člen funkce</span><span class="sxs-lookup"><span data-stu-id="08560-768">Better function member</span></span>

<span data-ttu-id="08560-769">Pro účely určení lepšího člena funkce je vytvořen seznam argumentů, který obsahuje pouze výrazy argumentů v pořadí, v jakém jsou uvedeny v seznamu původní argument.</span><span class="sxs-lookup"><span data-stu-id="08560-769">For the purposes of determining the better function member, a stripped-down argument list A is constructed containing just the argument expressions themselves in the order they appear in the original argument list.</span></span>

<span data-ttu-id="08560-770">Seznamy parametrů pro každý člen kandidátních funkcí jsou konstruovány následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="08560-770">Parameter lists for each of the candidate function members are constructed in the following way:</span></span>

*  <span data-ttu-id="08560-771">Rozšířený formulář se používá v případě, že byl člen funkce použit pouze v rozbaleném formuláři.</span><span class="sxs-lookup"><span data-stu-id="08560-771">The expanded form is used if the function member was applicable only in the expanded form.</span></span>
*  <span data-ttu-id="08560-772">Volitelné parametry bez odpovídajících argumentů se ze seznamu parametrů odeberou.</span><span class="sxs-lookup"><span data-stu-id="08560-772">Optional parameters with no corresponding arguments are removed from the parameter list</span></span>
*  <span data-ttu-id="08560-773">Parametry jsou přeuspořádané tak, že se vyskytují na stejné pozici jako odpovídající argument v seznamu argumentů.</span><span class="sxs-lookup"><span data-stu-id="08560-773">The parameters are reordered so that they occur at the same position as the corresponding argument in the argument list.</span></span>

<span data-ttu-id="08560-774">Předaný seznam argumentů `A` se sadou výrazů argumentů `{E1, E2, ..., En}` a dva příslušné členy funkce `Mp` a `Mq` s typy parametrů `{P1, P2, ..., Pn}` a `{Q1, Q2, ..., Qn}`, `Mp` je definován jako ***lepší člen funkce*** , než `Mq`, pokud</span><span class="sxs-lookup"><span data-stu-id="08560-774">Given an argument list `A` with a set of argument expressions `{E1, E2, ..., En}` and two applicable function members `Mp` and `Mq` with parameter types `{P1, P2, ..., Pn}` and `{Q1, Q2, ..., Qn}`, `Mp` is defined to be a ***better function member*** than `Mq` if</span></span>

*  <span data-ttu-id="08560-775">u každého argumentu není implicitní převod z `Ex` na `Qx` lepší než implicitní převod z `Ex` na `Px`a</span><span class="sxs-lookup"><span data-stu-id="08560-775">for each argument, the implicit conversion from `Ex` to `Qx` is not better than the implicit conversion from `Ex` to `Px`, and</span></span>
*  <span data-ttu-id="08560-776">pro alespoň jeden argument je převod z `Ex` na `Px` lepší než převod z `Ex` na `Qx`.</span><span class="sxs-lookup"><span data-stu-id="08560-776">for at least one argument, the conversion from `Ex` to `Px` is better than the conversion from `Ex` to `Qx`.</span></span>

<span data-ttu-id="08560-777">Pokud se při tomto vyhodnocení použije `Mp` nebo `Mq` v rozbalené formě, `Px` nebo `Qx` odkazuje na parametr v rozbalené formě seznamu parametrů.</span><span class="sxs-lookup"><span data-stu-id="08560-777">When performing this evaluation, if `Mp` or `Mq` is applicable in its expanded form, then `Px` or `Qx` refers to a parameter in the expanded form of the parameter list.</span></span>

<span data-ttu-id="08560-778">V případě, že jsou sekvence typu parametru `{P1, P2, ..., Pn}` a `{Q1, Q2, ..., Qn}` ekvivalentní (to znamená, že každý `Pi` má převod identity na odpovídající `Qi`), jsou použita následující pravidla pro dělení na více parametrů, aby bylo možné určit lepší člen funkce.</span><span class="sxs-lookup"><span data-stu-id="08560-778">In case the parameter type sequences `{P1, P2, ..., Pn}` and `{Q1, Q2, ..., Qn}` are equivalent (i.e. each `Pi` has an identity conversion to the corresponding `Qi`), the following tie-breaking rules are applied, in order, to determine the better function member.</span></span>

*  <span data-ttu-id="08560-779">Pokud je `Mp` neobecnou metodou a `Mq` je obecná metoda, `Mp` je lepší než `Mq`.</span><span class="sxs-lookup"><span data-stu-id="08560-779">If `Mp` is a non-generic method and `Mq` is a generic method, then `Mp` is better than `Mq`.</span></span>
*  <span data-ttu-id="08560-780">V opačném případě platí, že pokud `Mp` použít v normálním tvaru a `Mq` má `params` pole a je použitelné pouze v rozbaleném formuláři, `Mp` je lepší než `Mq`.</span><span class="sxs-lookup"><span data-stu-id="08560-780">Otherwise, if `Mp` is applicable in its normal form and `Mq` has a `params` array and is applicable only in its expanded form, then `Mp` is better than `Mq`.</span></span>
*  <span data-ttu-id="08560-781">V opačném případě, pokud má `Mp` více deklarovaných parametrů než `Mq`, `Mp` je lepší než `Mq`.</span><span class="sxs-lookup"><span data-stu-id="08560-781">Otherwise, if `Mp` has more declared parameters than `Mq`, then `Mp` is better than `Mq`.</span></span> <span data-ttu-id="08560-782">Tato situace může nastat, pokud obě metody mají `params` pole a jsou použitelné pouze v rozbalených formulářích.</span><span class="sxs-lookup"><span data-stu-id="08560-782">This can occur if both methods have `params` arrays and are applicable only in their expanded forms.</span></span>
*  <span data-ttu-id="08560-783">Jinak, pokud všechny parametry `Mp` mají odpovídající argument, zatímco výchozí argumenty je potřeba nahradit aspoň u jednoho volitelného parametru v `Mq` je `Mp` lepší než `Mq`.</span><span class="sxs-lookup"><span data-stu-id="08560-783">Otherwise if all parameters of `Mp` have a corresponding argument whereas default arguments need to be substituted for at least one optional parameter in `Mq` then `Mp` is better than `Mq`.</span></span>
*  <span data-ttu-id="08560-784">Jinak, pokud `Mp` má konkrétnější typy parametrů než `Mq`, `Mp` je lepší než `Mq`.</span><span class="sxs-lookup"><span data-stu-id="08560-784">Otherwise, if `Mp` has more specific parameter types than `Mq`, then `Mp` is better than `Mq`.</span></span> <span data-ttu-id="08560-785">Nechat `{R1, R2, ..., Rn}` a `{S1, S2, ..., Sn}` představovat Nerozbalené typy parametrů `Mp` a `Mq`.</span><span class="sxs-lookup"><span data-stu-id="08560-785">Let `{R1, R2, ..., Rn}` and `{S1, S2, ..., Sn}` represent the uninstantiated and unexpanded parameter types of `Mp` and `Mq`.</span></span> <span data-ttu-id="08560-786">typy parametrů `Mp`jsou konkrétnější než `Mq`v případě, že pro každý parametr `Rx` není méně specifická než `Sx`a pro alespoň jeden parametr je `Rx` více specifických než `Sx`:</span><span class="sxs-lookup"><span data-stu-id="08560-786">`Mp`'s parameter types are more specific than `Mq`'s if, for each parameter, `Rx` is not less specific than `Sx`, and, for at least one parameter, `Rx` is more specific than `Sx`:</span></span>
   *  <span data-ttu-id="08560-787">Parametr typu je méně specifický než parametr bez typu.</span><span class="sxs-lookup"><span data-stu-id="08560-787">A type parameter is less specific than a non-type parameter.</span></span>
   *  <span data-ttu-id="08560-788">Rekurzivně, konstruovaný typ je konkrétnější než jiný konstruovaný typ (se stejným počtem argumentů typu), pokud je alespoň jeden argument typu konkrétnější a žádný argument typu není méně specifický než odpovídající argument typu v druhé.</span><span class="sxs-lookup"><span data-stu-id="08560-788">Recursively, a constructed type is more specific than another constructed type (with the same number of type arguments) if at least one type argument is more specific and no type argument is less specific than the corresponding type argument in the other.</span></span>
   *  <span data-ttu-id="08560-789">Typ pole je konkrétnější než jiný typ pole (se stejným počtem rozměrů), pokud typ prvku prvního je konkrétnější než typ elementu druhé.</span><span class="sxs-lookup"><span data-stu-id="08560-789">An array type is more specific than another array type (with the same number of dimensions) if the element type of the first is more specific than the element type of the second.</span></span>
*  <span data-ttu-id="08560-790">V opačném případě, pokud je jeden člen jako nezdvižený operátor a druhý je předaný operátor, je lepší, než je nezatížený.</span><span class="sxs-lookup"><span data-stu-id="08560-790">Otherwise if one member is a non-lifted operator and  the other is a lifted operator, the non-lifted one is better.</span></span>
*  <span data-ttu-id="08560-791">V opačném případě není ani člen funkce lepší.</span><span class="sxs-lookup"><span data-stu-id="08560-791">Otherwise, neither function member is better.</span></span>

#### <a name="better-conversion-from-expression"></a><span data-ttu-id="08560-792">Lepší převod výrazu</span><span class="sxs-lookup"><span data-stu-id="08560-792">Better conversion from expression</span></span>

<span data-ttu-id="08560-793">Poskytnutý implicitní převod `C1`, který se převede z výrazu `E` na typ `T1`a implicitní převod `C2`, který se převede z výrazu `E` na typ `T2`, `C1` je ***lepší převod*** než `C2`, pokud `E` přesně neodpovídá `T2` a alespoň v jednom z následujících blokování:</span><span class="sxs-lookup"><span data-stu-id="08560-793">Given an implicit conversion `C1` that converts from an expression `E` to a type `T1`, and an implicit conversion `C2` that converts from an expression `E` to a type `T2`, `C1` is a ***better conversion*** than `C2` if `E` does not exactly match `T2` and at least one of the following holds:</span></span>

* <span data-ttu-id="08560-794">`E` přesně odpovídá `T1` ([přesně odpovídající výraz](expressions.md#exactly-matching-expression))</span><span class="sxs-lookup"><span data-stu-id="08560-794">`E` exactly matches `T1` ([Exactly matching Expression](expressions.md#exactly-matching-expression))</span></span>
* <span data-ttu-id="08560-795">`T1` je lepším cílem převodu než `T2` ([cíl pro lepší převod](expressions.md#better-conversion-target)).</span><span class="sxs-lookup"><span data-stu-id="08560-795">`T1` is a better conversion target than `T2` ([Better conversion target](expressions.md#better-conversion-target))</span></span>

#### <a name="exactly-matching-expression"></a><span data-ttu-id="08560-796">Přesně shodný výraz</span><span class="sxs-lookup"><span data-stu-id="08560-796">Exactly matching Expression</span></span>

<span data-ttu-id="08560-797">Při zadání `E` výrazu a `T`typu `E` přesně odpovídat `T`, pokud obsahuje některý z následujících typů:</span><span class="sxs-lookup"><span data-stu-id="08560-797">Given an expression `E` and a type `T`, `E` exactly matches `T` if one of the following holds:</span></span>

*  <span data-ttu-id="08560-798">`E` má `S`typu a na `S` existuje převod identity na `T`</span><span class="sxs-lookup"><span data-stu-id="08560-798">`E` has a type `S`, and an identity conversion exists from `S` to `T`</span></span>
*  <span data-ttu-id="08560-799">`E` je anonymní funkce, `T` je buď typ delegáta `D`, nebo typ stromu výrazu `Expression<D>` a jedna z následujících možností obsahuje:</span><span class="sxs-lookup"><span data-stu-id="08560-799">`E` is an anonymous function, `T` is either a delegate type `D` or an expression tree type `Expression<D>` and one of the following holds:</span></span>
   *  <span data-ttu-id="08560-800">Odvozený návratový typ `X` existuje pro `E` v kontextu seznamu parametrů `D` ([odvozený návratový typ](expressions.md#inferred-return-type)) a převod identity existuje z `X` na návratový typ `D`</span><span class="sxs-lookup"><span data-stu-id="08560-800">An inferred return type `X` exists for `E` in the context of the parameter list of `D` ([Inferred return type](expressions.md#inferred-return-type)), and an identity conversion exists from `X` to the return type of `D`</span></span>
   *  <span data-ttu-id="08560-801">Buď je `E` neasynchronní a `D` má návratový typ `Y` nebo `E` je Async a `D` má návratový typ `Task<Y>`a jeden z následujících typů:</span><span class="sxs-lookup"><span data-stu-id="08560-801">Either `E` is non-async and `D` has a return type `Y` or `E` is async and `D` has a return type `Task<Y>`, and one of the following holds:</span></span>
      * <span data-ttu-id="08560-802">Tělo `E` je výraz, který přesně odpovídá `Y`</span><span class="sxs-lookup"><span data-stu-id="08560-802">The body of `E` is an expression that exactly matches `Y`</span></span>
      * <span data-ttu-id="08560-803">Tělo `E` je blok příkazu, kde každý příkaz return vrací výraz, který přesně odpovídá `Y`</span><span class="sxs-lookup"><span data-stu-id="08560-803">The body of `E` is a statement block where every return statement returns an expression that exactly matches `Y`</span></span>

#### <a name="better-conversion-target"></a><span data-ttu-id="08560-804">Lepší cíl převodu</span><span class="sxs-lookup"><span data-stu-id="08560-804">Better conversion target</span></span>

<span data-ttu-id="08560-805">U dvou různých typů `T1` a `T2`je `T1` cílem lepšího převodu než `T2`, pokud neexistuje implicitní převod z `T2` na `T1` a alespoň jeden z následujících blokování:</span><span class="sxs-lookup"><span data-stu-id="08560-805">Given two different types `T1` and `T2`, `T1` is a better conversion target than `T2` if no implicit conversion from `T2` to `T1` exists, and at least one of the following holds:</span></span>

*  <span data-ttu-id="08560-806">Implicitní převod z `T1` na `T2` existuje.</span><span class="sxs-lookup"><span data-stu-id="08560-806">An implicit conversion from `T1` to `T2` exists</span></span>
*  <span data-ttu-id="08560-807">`T1` je buď typ delegáta `D1` nebo typ stromu výrazu `Expression<D1>`, `T2` je buď typ delegáta `D2`, nebo typ stromu výrazu `Expression<D2>`, `D1` má `S1` návratového typu a jeden z následujících blokování:</span><span class="sxs-lookup"><span data-stu-id="08560-807">`T1` is either a delegate type `D1` or an expression tree type `Expression<D1>`, `T2` is either a delegate type `D2` or an expression tree type `Expression<D2>`, `D1` has a return type `S1` and one of the following holds:</span></span>
   * <span data-ttu-id="08560-808">`D2` vrací typ void</span><span class="sxs-lookup"><span data-stu-id="08560-808">`D2` is void returning</span></span>
   * <span data-ttu-id="08560-809">`D2` má `S2`návratového typu a `S1` je cílem lepšího převodu než `S2`</span><span class="sxs-lookup"><span data-stu-id="08560-809">`D2` has a return type `S2`, and `S1` is a better conversion target than `S2`</span></span>
*  <span data-ttu-id="08560-810">`T1` je `Task<S1>`, `T2` je `Task<S2>`a `S1` je lepším cílem převodu než `S2`</span><span class="sxs-lookup"><span data-stu-id="08560-810">`T1` is `Task<S1>`, `T2` is `Task<S2>`, and `S1` is a better conversion target than `S2`</span></span>
*  <span data-ttu-id="08560-811">`T1` je `S1` nebo `S1?`, kde `S1` je celočíselný typ se znaménkem a `T2` je `S2` nebo `S2?`, kde `S2` je celočíselný typ bez znaménka.</span><span class="sxs-lookup"><span data-stu-id="08560-811">`T1` is `S1` or `S1?` where `S1` is a signed integral type, and `T2` is `S2` or `S2?` where `S2` is an unsigned integral type.</span></span> <span data-ttu-id="08560-812">Zejména:</span><span class="sxs-lookup"><span data-stu-id="08560-812">Specifically:</span></span>
   * <span data-ttu-id="08560-813">`S1` je `sbyte` a `S2` je `byte`, `ushort`, `uint`nebo `ulong`</span><span class="sxs-lookup"><span data-stu-id="08560-813">`S1` is `sbyte` and `S2` is `byte`, `ushort`, `uint`, or `ulong`</span></span>
   * <span data-ttu-id="08560-814">`S1` je `short` a `S2` je `ushort`, `uint`nebo `ulong`</span><span class="sxs-lookup"><span data-stu-id="08560-814">`S1` is `short` and `S2` is `ushort`, `uint`, or `ulong`</span></span>
   * <span data-ttu-id="08560-815">`S1` je `int` a `S2` `uint`nebo `ulong`</span><span class="sxs-lookup"><span data-stu-id="08560-815">`S1` is `int` and `S2` is `uint`, or `ulong`</span></span>
   * <span data-ttu-id="08560-816">`S1` je `long` a `S2` je `ulong`</span><span class="sxs-lookup"><span data-stu-id="08560-816">`S1` is `long` and `S2` is `ulong`</span></span>

#### <a name="overloading-in-generic-classes"></a><span data-ttu-id="08560-817">Přetížení v obecných třídách</span><span class="sxs-lookup"><span data-stu-id="08560-817">Overloading in generic classes</span></span>

<span data-ttu-id="08560-818">I když signatury, které jsou deklarovány, musí být jedinečné, je možné, že nahrazení argumentů typu vede k identickým podpisům.</span><span class="sxs-lookup"><span data-stu-id="08560-818">While signatures as declared must be unique, it is possible that substitution of type arguments results in identical signatures.</span></span> <span data-ttu-id="08560-819">V takových případech se pravidla, která jsou ve výše uvedeném Rozlišení přetěžování, budou vybírat nejvíce konkrétního člena.</span><span class="sxs-lookup"><span data-stu-id="08560-819">In such cases, the tie-breaking rules of overload resolution above will pick the most specific member.</span></span>

<span data-ttu-id="08560-820">Následující příklady znázorňují přetížení, které jsou platné a neplatné podle tohoto pravidla:</span><span class="sxs-lookup"><span data-stu-id="08560-820">The following examples show overloads that are valid and invalid according to this rule:</span></span>

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

### <a name="compile-time-checking-of-dynamic-overload-resolution"></a><span data-ttu-id="08560-821">Kontrola dynamického překladu přetížení při kompilaci</span><span class="sxs-lookup"><span data-stu-id="08560-821">Compile-time checking of dynamic overload resolution</span></span>

<span data-ttu-id="08560-822">Pro většinu dynamicky vázaných operací není sada možných kandidátů na rozlišení v době kompilace neznámá.</span><span class="sxs-lookup"><span data-stu-id="08560-822">For most dynamically bound operations the set of possible candidates for resolution is unknown at compile-time.</span></span> <span data-ttu-id="08560-823">V některých případech je však kandidátská sada známá v době kompilace:</span><span class="sxs-lookup"><span data-stu-id="08560-823">In certain cases, however the candidate set is known at compile-time:</span></span>

*  <span data-ttu-id="08560-824">Volání statických metod s dynamickými argumenty</span><span class="sxs-lookup"><span data-stu-id="08560-824">Static method calls with dynamic arguments</span></span>
*  <span data-ttu-id="08560-825">Metoda instance volá, kde příjemce není dynamický výraz.</span><span class="sxs-lookup"><span data-stu-id="08560-825">Instance method calls where the receiver is not a dynamic expression</span></span>
*  <span data-ttu-id="08560-826">Indexer volá, kde příjemce není dynamický výraz.</span><span class="sxs-lookup"><span data-stu-id="08560-826">Indexer calls where the receiver is not a dynamic expression</span></span>
*  <span data-ttu-id="08560-827">Konstruktor volá s dynamickými argumenty.</span><span class="sxs-lookup"><span data-stu-id="08560-827">Constructor calls with dynamic arguments</span></span>

<span data-ttu-id="08560-828">V těchto případech je k dispozici omezená doba kompilace pro každého kandidáta, aby bylo možné zjistit, zda by některá z nich mohla být použita v době běhu. Tato kontrolu se skládá z následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="08560-828">In these cases a limited compile-time check is performed for each candidate to see if any of them could possibly apply at run-time.This check consists of the following steps:</span></span>

*  <span data-ttu-id="08560-829">Odvození odvození typu: jakýkoliv argument typu, který není závislý přímo ani nepřímo na argumentu typu `dynamic` je odvozený pomocí pravidel [odvození typu](expressions.md#type-inference).</span><span class="sxs-lookup"><span data-stu-id="08560-829">Partial type inference: Any type argument that does not depend directly or indirectly on an argument of type `dynamic` is inferred using the rules of [Type inference](expressions.md#type-inference).</span></span> <span data-ttu-id="08560-830">Zbývající argumenty typu nejsou známy.</span><span class="sxs-lookup"><span data-stu-id="08560-830">The remaining type arguments are unknown.</span></span>
*  <span data-ttu-id="08560-831">Částečná kontrola použitelnosti: použitelnost je kontrolována v závislosti na [platném členu funkce](expressions.md#applicable-function-member), ale ignoruje parametry, jejichž typy jsou neznámé.</span><span class="sxs-lookup"><span data-stu-id="08560-831">Partial applicability check: Applicability is checked according to [Applicable function member](expressions.md#applicable-function-member), but ignoring parameters whose types are unknown.</span></span>
*  <span data-ttu-id="08560-832">Pokud žádný kandidát neprojde tímto testem, dojde k chybě při kompilaci.</span><span class="sxs-lookup"><span data-stu-id="08560-832">If no candidate passes this test, a compile-time error occurs.</span></span>

### <a name="function-member-invocation"></a><span data-ttu-id="08560-833">Vyvolání člena funkce</span><span class="sxs-lookup"><span data-stu-id="08560-833">Function member invocation</span></span>

<span data-ttu-id="08560-834">Tato část popisuje proces, který probíhá za běhu k vyvolání konkrétního člena funkce.</span><span class="sxs-lookup"><span data-stu-id="08560-834">This section describes the process that takes place at run-time to invoke a particular function member.</span></span> <span data-ttu-id="08560-835">Předpokládá se, že proces vytvoření vazby již určil konkrétního člena k vyvolání, pravděpodobně použitím řešení přetížení pro sadu kandidátních členů funkce.</span><span class="sxs-lookup"><span data-stu-id="08560-835">It is assumed that a binding-time process has already determined the particular member to invoke, possibly by applying overload resolution to a set of candidate function members.</span></span>

<span data-ttu-id="08560-836">Pro účely popisu procesu vyvolání jsou členové funkce rozděleni do dvou kategorií:</span><span class="sxs-lookup"><span data-stu-id="08560-836">For purposes of describing the invocation process, function members are divided into two categories:</span></span>

*  <span data-ttu-id="08560-837">Členové statických funkcí.</span><span class="sxs-lookup"><span data-stu-id="08560-837">Static function members.</span></span> <span data-ttu-id="08560-838">Jedná se o konstruktory instancí, statické metody, přistupující objekty statických vlastností a uživatelsky definované operátory.</span><span class="sxs-lookup"><span data-stu-id="08560-838">These are instance constructors, static methods, static property accessors, and user-defined operators.</span></span> <span data-ttu-id="08560-839">Členy statických funkcí jsou vždy nevirtuální.</span><span class="sxs-lookup"><span data-stu-id="08560-839">Static function members are always non-virtual.</span></span>
*  <span data-ttu-id="08560-840">Členové funkce instance.</span><span class="sxs-lookup"><span data-stu-id="08560-840">Instance function members.</span></span> <span data-ttu-id="08560-841">Jedná se o metody instancí, přistupující objekty vlastnosti instance a přistupující objekty indexeru.</span><span class="sxs-lookup"><span data-stu-id="08560-841">These are instance methods, instance property accessors, and indexer accessors.</span></span> <span data-ttu-id="08560-842">Členy funkce instance jsou buď nevirtuální, nebo virtuální a jsou vždy vyvolány na konkrétní instanci.</span><span class="sxs-lookup"><span data-stu-id="08560-842">Instance function members are either non-virtual or virtual, and are always invoked on a particular instance.</span></span> <span data-ttu-id="08560-843">Instance je vypočítána výrazem instance a je zpřístupněna v rámci člena funkce jako `this` ([Tento přístup](expressions.md#this-access)).</span><span class="sxs-lookup"><span data-stu-id="08560-843">The instance is computed by an instance expression, and it becomes accessible within the function member as `this` ([This access](expressions.md#this-access)).</span></span>

<span data-ttu-id="08560-844">Běhové zpracování vyvolání členů funkce se skládá z následujících kroků, kde `M` je člen funkce a, pokud `M` je člen instance, `E` je výraz instance:</span><span class="sxs-lookup"><span data-stu-id="08560-844">The run-time processing of a function member invocation consists of the following steps, where `M` is the function member and, if `M` is an instance member, `E` is the instance expression:</span></span>

*  <span data-ttu-id="08560-845">Pokud je `M` členem statické funkce:</span><span class="sxs-lookup"><span data-stu-id="08560-845">If `M` is a static function member:</span></span>
   * <span data-ttu-id="08560-846">Seznam argumentů je vyhodnocován, jak je popsáno v [seznamech argumentů](expressions.md#argument-lists).</span><span class="sxs-lookup"><span data-stu-id="08560-846">The argument list is evaluated as described in [Argument lists](expressions.md#argument-lists).</span></span>
   * <span data-ttu-id="08560-847">je vyvolána `M`.</span><span class="sxs-lookup"><span data-stu-id="08560-847">`M` is invoked.</span></span>

*  <span data-ttu-id="08560-848">Pokud `M` je člen funkce instance deklarované v *value_type*:</span><span class="sxs-lookup"><span data-stu-id="08560-848">If `M` is an instance function member declared in a *value_type*:</span></span>
   * <span data-ttu-id="08560-849">`E` je vyhodnocena.</span><span class="sxs-lookup"><span data-stu-id="08560-849">`E` is evaluated.</span></span> <span data-ttu-id="08560-850">Pokud toto vyhodnocení způsobí výjimku, nejsou provedeny žádné další kroky.</span><span class="sxs-lookup"><span data-stu-id="08560-850">If this evaluation causes an exception, then no further steps are executed.</span></span>
   * <span data-ttu-id="08560-851">Pokud `E` není klasifikován jako proměnná, je vytvořena dočasná lokální proměnná typu `E`a k této proměnné je přiřazena hodnota `E`.</span><span class="sxs-lookup"><span data-stu-id="08560-851">If `E` is not classified as a variable, then a temporary local variable of `E`'s type is created and the value of `E` is assigned to that variable.</span></span> <span data-ttu-id="08560-852">`E` je pak znovu klasifikován jako odkaz na tuto dočasnou místní proměnnou.</span><span class="sxs-lookup"><span data-stu-id="08560-852">`E` is then reclassified as a reference to that temporary local variable.</span></span> <span data-ttu-id="08560-853">Dočasná proměnná je dostupná jako `this` v rámci `M`, ale ne jiným způsobem.</span><span class="sxs-lookup"><span data-stu-id="08560-853">The temporary variable is accessible as `this` within `M`, but not in any other way.</span></span> <span data-ttu-id="08560-854">Proto pouze v případě, že `E` je hodnota true, může volající sledovat změny, které `M` provede `this`.</span><span class="sxs-lookup"><span data-stu-id="08560-854">Thus, only when `E` is a true variable is it possible for the caller to observe the changes that `M` makes to `this`.</span></span>
   * <span data-ttu-id="08560-855">Seznam argumentů je vyhodnocován, jak je popsáno v [seznamech argumentů](expressions.md#argument-lists).</span><span class="sxs-lookup"><span data-stu-id="08560-855">The argument list is evaluated as described in [Argument lists](expressions.md#argument-lists).</span></span>
   * <span data-ttu-id="08560-856">je vyvolána `M`.</span><span class="sxs-lookup"><span data-stu-id="08560-856">`M` is invoked.</span></span> <span data-ttu-id="08560-857">Proměnná, na kterou `E` odkazuje, se stal proměnnou, na kterou odkazuje `this`.</span><span class="sxs-lookup"><span data-stu-id="08560-857">The variable referenced by `E` becomes the variable referenced by `this`.</span></span>

*  <span data-ttu-id="08560-858">Pokud `M` je člen funkce instance deklarované v *reference_type*:</span><span class="sxs-lookup"><span data-stu-id="08560-858">If `M` is an instance function member declared in a *reference_type*:</span></span>
   * <span data-ttu-id="08560-859">`E` je vyhodnocena.</span><span class="sxs-lookup"><span data-stu-id="08560-859">`E` is evaluated.</span></span> <span data-ttu-id="08560-860">Pokud toto vyhodnocení způsobí výjimku, nejsou provedeny žádné další kroky.</span><span class="sxs-lookup"><span data-stu-id="08560-860">If this evaluation causes an exception, then no further steps are executed.</span></span>
   * <span data-ttu-id="08560-861">Seznam argumentů je vyhodnocován, jak je popsáno v [seznamech argumentů](expressions.md#argument-lists).</span><span class="sxs-lookup"><span data-stu-id="08560-861">The argument list is evaluated as described in [Argument lists](expressions.md#argument-lists).</span></span>
   * <span data-ttu-id="08560-862">Pokud je typ `E` *value_type*, je proveden převod na zabalení ([převody na zabalení](types.md#boxing-conversions)) pro převod `E` na typ `object`a `E` se považuje za typ `object` v následujících krocích.</span><span class="sxs-lookup"><span data-stu-id="08560-862">If the type of `E` is a *value_type*, a boxing conversion ([Boxing conversions](types.md#boxing-conversions)) is performed to convert `E` to type `object`, and `E` is considered to be of type `object` in the following steps.</span></span> <span data-ttu-id="08560-863">V takovém případě by `M` mohla být pouze členem `System.Object`.</span><span class="sxs-lookup"><span data-stu-id="08560-863">In this case, `M` could only be a member of `System.Object`.</span></span>
   * <span data-ttu-id="08560-864">Hodnota `E` je zaškrtnuta jako platná.</span><span class="sxs-lookup"><span data-stu-id="08560-864">The value of `E` is checked to be valid.</span></span> <span data-ttu-id="08560-865">Je-li hodnota `E` `null`, je vyvolána `System.NullReferenceException` a nejsou provedeny žádné další kroky.</span><span class="sxs-lookup"><span data-stu-id="08560-865">If the value of `E` is `null`, a `System.NullReferenceException` is thrown and no further steps are executed.</span></span>
   * <span data-ttu-id="08560-866">Je určena implementace členu funkce k vyvolání:</span><span class="sxs-lookup"><span data-stu-id="08560-866">The function member implementation to invoke is determined:</span></span>
     * <span data-ttu-id="08560-867">Pokud je typ doby vazby `E` rozhraní, člen funkce, který se má vyvolat, je implementace `M` poskytnutý běhovým typem instance, na kterou odkazuje `E`.</span><span class="sxs-lookup"><span data-stu-id="08560-867">If the binding-time type of `E` is an interface, the function member to invoke is the implementation of `M` provided by the run-time type of the instance referenced by `E`.</span></span> <span data-ttu-id="08560-868">Tento člen funkce je určen pomocí pravidel mapování rozhraní ([mapování rozhraní](interfaces.md#interface-mapping)) k určení implementace `M` poskytovaného běhovým typem instance, na kterou odkazuje `E`.</span><span class="sxs-lookup"><span data-stu-id="08560-868">This function member is determined by applying the interface mapping rules ([Interface mapping](interfaces.md#interface-mapping)) to determine the implementation of `M` provided by the run-time type of the instance referenced by `E`.</span></span>
     * <span data-ttu-id="08560-869">V opačném případě, pokud je `M` členem virtuální funkce, je člen funkce k vyvolání implementováno implementací `M` poskytovaných běhovým typem instance, na kterou odkazuje `E`.</span><span class="sxs-lookup"><span data-stu-id="08560-869">Otherwise, if `M` is a virtual function member, the function member to invoke is the implementation of `M` provided by the run-time type of the instance referenced by `E`.</span></span> <span data-ttu-id="08560-870">Tento člen funkce je určen použitím pravidel pro určení nejvíce odvozené implementace ([virtuální metody](classes.md#virtual-methods)) `M` s ohledem na typ modulu runtime instance, na kterou odkazuje `E`.</span><span class="sxs-lookup"><span data-stu-id="08560-870">This function member is determined by applying the rules for determining the most derived implementation ([Virtual methods](classes.md#virtual-methods)) of `M` with respect to the run-time type of the instance referenced by `E`.</span></span>
     * <span data-ttu-id="08560-871">V opačném případě je `M` členem nevirtuální funkce a člen funkce k vyvolání je `M` sám sebe.</span><span class="sxs-lookup"><span data-stu-id="08560-871">Otherwise, `M` is a non-virtual function member, and the function member to invoke is `M` itself.</span></span>
   * <span data-ttu-id="08560-872">Je vyvolána implementace členu funkce určená v kroku výše.</span><span class="sxs-lookup"><span data-stu-id="08560-872">The function member implementation determined in the step above is invoked.</span></span> <span data-ttu-id="08560-873">Objekt odkazovaný `E` se bude objektem, na který odkazuje `this`.</span><span class="sxs-lookup"><span data-stu-id="08560-873">The object referenced by `E` becomes the object referenced by `this`.</span></span>

#### <a name="invocations-on-boxed-instances"></a><span data-ttu-id="08560-874">Vyvolání u zabalených instancí</span><span class="sxs-lookup"><span data-stu-id="08560-874">Invocations on boxed instances</span></span>

<span data-ttu-id="08560-875">Člen funkce implementovaný ve *value_type* lze vyvolat prostřednictvím zabalené instance, která *value_type* v následujících situacích:</span><span class="sxs-lookup"><span data-stu-id="08560-875">A function member implemented in a *value_type* can be invoked through a boxed instance of that *value_type* in the following situations:</span></span>

*  <span data-ttu-id="08560-876">Když je člen funkce `override` metody zděděné z typu `object` a je vyvolán prostřednictvím výrazu instance typu `object`.</span><span class="sxs-lookup"><span data-stu-id="08560-876">When the function member is an `override` of a method inherited from type `object` and is invoked through an instance expression of type `object`.</span></span>
*  <span data-ttu-id="08560-877">Když je člen funkce implementací člena funkce rozhraní a je vyvolán prostřednictvím výrazu instance *INTERFACE_TYPE*.</span><span class="sxs-lookup"><span data-stu-id="08560-877">When the function member is an implementation of an interface function member and is invoked through an instance expression of an *interface_type*.</span></span>
*  <span data-ttu-id="08560-878">Když je člen funkce vyvolán prostřednictvím delegáta.</span><span class="sxs-lookup"><span data-stu-id="08560-878">When the function member is invoked through a delegate.</span></span>

<span data-ttu-id="08560-879">V těchto situacích je zabalená instance považována za obsahující proměnnou *value_type*a tato proměnná se na proměnnou, na kterou `this` odkazuje v rámci vyvolání člena funkce.</span><span class="sxs-lookup"><span data-stu-id="08560-879">In these situations, the boxed instance is considered to contain a variable of the *value_type*, and this variable becomes the variable referenced by `this` within the function member invocation.</span></span> <span data-ttu-id="08560-880">Konkrétně to znamená, že pokud je člen funkce vyvolán na zabalenou instanci, může člen funkce změnit hodnotu obsaženou v zabalené instanci.</span><span class="sxs-lookup"><span data-stu-id="08560-880">In particular, this means that when a function member is invoked on a boxed instance, it is possible for the function member to modify the value contained in the boxed instance.</span></span>

## <a name="primary-expressions"></a><span data-ttu-id="08560-881">Primární výrazy</span><span class="sxs-lookup"><span data-stu-id="08560-881">Primary expressions</span></span>

<span data-ttu-id="08560-882">Primární výrazy obsahují nejjednodušší formy výrazů.</span><span class="sxs-lookup"><span data-stu-id="08560-882">Primary expressions include the simplest forms of expressions.</span></span>

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

<span data-ttu-id="08560-883">Primární výrazy jsou rozděleny mezi *array_creation_expression*s a *primary_no_array_creation_expression*.</span><span class="sxs-lookup"><span data-stu-id="08560-883">Primary expressions are divided between *array_creation_expression*s and *primary_no_array_creation_expression*s.</span></span> <span data-ttu-id="08560-884">Tímto způsobem se s použitím výrazu pro vytvoření pole vytvoří výraz, spíše než jeho výpis spolu s dalšími formuláři jednoduchých výrazů umožňuje, aby gramatika zakázala potenciálně matoucí kód, jako např.</span><span class="sxs-lookup"><span data-stu-id="08560-884">Treating array-creation-expression in this way, rather than listing it along with the other simple expression forms, enables the grammar to disallow potentially confusing code such as</span></span>
```csharp
object o = new int[3][1];
```
<span data-ttu-id="08560-885">které by jinak bylo interpretováno jako</span><span class="sxs-lookup"><span data-stu-id="08560-885">which would otherwise be interpreted as</span></span>
```csharp
object o = (new int[3])[1];
```

### <a name="literals"></a><span data-ttu-id="08560-886">Literály</span><span class="sxs-lookup"><span data-stu-id="08560-886">Literals</span></span>

<span data-ttu-id="08560-887">*Primary_expression* , která se skládá z *literálu* ([literály](lexical-structure.md#literals)), je klasifikována jako hodnota.</span><span class="sxs-lookup"><span data-stu-id="08560-887">A *primary_expression* that consists of a *literal* ([Literals](lexical-structure.md#literals)) is classified as a value.</span></span>


### <a name="interpolated-strings"></a><span data-ttu-id="08560-888">Interpolované řetězce</span><span class="sxs-lookup"><span data-stu-id="08560-888">Interpolated strings</span></span>

<span data-ttu-id="08560-889">*Interpolated_string_expression* se skládá ze `$` znaménka následovaný regulárním nebo doslovném řetězcovým literálem, přičemž jsou v něm otvory oddělené `{` a `}`, které obsahují výrazy a specifikace formátování.</span><span class="sxs-lookup"><span data-stu-id="08560-889">An *interpolated_string_expression* consists of a `$` sign followed by a regular or verbatim string literal, wherein holes, delimited by `{` and `}`, enclose expressions and formatting specifications.</span></span> <span data-ttu-id="08560-890">Výraz interpoled String je výsledkem *interpolated_string_literal* , která byla rozdělena do jednotlivých tokenů, jak je popsáno v [interpolovaná řetězcové literály](lexical-structure.md#interpolated-string-literals).</span><span class="sxs-lookup"><span data-stu-id="08560-890">An interpolated string expression is the result of an *interpolated_string_literal* that has been broken up into individual tokens, as described in [Interpolated string literals](lexical-structure.md#interpolated-string-literals).</span></span>

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

<span data-ttu-id="08560-891">*Constant_expression* v interpolaci musí mít implicitní převod na `int`.</span><span class="sxs-lookup"><span data-stu-id="08560-891">The *constant_expression* in an interpolation must have an implicit conversion to `int`.</span></span>

<span data-ttu-id="08560-892">*Interpolated_string_expression* je klasifikován jako hodnota.</span><span class="sxs-lookup"><span data-stu-id="08560-892">An *interpolated_string_expression* is classified as a value.</span></span> <span data-ttu-id="08560-893">Pokud je okamžitě převedena na `System.IFormattable` nebo `System.FormattableString` pomocí implicitního interpolované řetězcové konverze ([implicitně interpolované převody řetězců](conversions.md#implicit-interpolated-string-conversions)), interpolující řetězcový výraz má tento typ.</span><span class="sxs-lookup"><span data-stu-id="08560-893">If it is immediately converted to `System.IFormattable` or `System.FormattableString` with an implicit interpolated string conversion ([Implicit interpolated string conversions](conversions.md#implicit-interpolated-string-conversions)), the interpolated string expression has that type.</span></span> <span data-ttu-id="08560-894">V opačném případě má typ `string`.</span><span class="sxs-lookup"><span data-stu-id="08560-894">Otherwise, it has the type `string`.</span></span>

<span data-ttu-id="08560-895">Pokud je typ interpolované řetězcové `System.IFormattable` nebo `System.FormattableString`, je význam volání `System.Runtime.CompilerServices.FormattableStringFactory.Create`.</span><span class="sxs-lookup"><span data-stu-id="08560-895">If the type of an interpolated string is `System.IFormattable` or `System.FormattableString`, the meaning is a call to `System.Runtime.CompilerServices.FormattableStringFactory.Create`.</span></span> <span data-ttu-id="08560-896">Pokud je typ `string`, význam výrazu je volání `string.Format`.</span><span class="sxs-lookup"><span data-stu-id="08560-896">If the type is `string`, the meaning of the expression is a call to `string.Format`.</span></span> <span data-ttu-id="08560-897">V obou případech se seznam argumentů volání skládá z řetězcového literálu formátu se zástupnými symboly pro každou interpolaci a argument pro každý výraz odpovídající držitelům umístění.</span><span class="sxs-lookup"><span data-stu-id="08560-897">In both cases, the argument list of the call consists of a format string literal with placeholders for each interpolation, and an argument for each expression corresponding to the place holders.</span></span>

<span data-ttu-id="08560-898">Řetězcový literál formátu je konstruován takto, kde `N` je počet interpolací v *interpolated_string_expression*:</span><span class="sxs-lookup"><span data-stu-id="08560-898">The format string literal is constructed as follows, where `N` is the number of interpolations in the *interpolated_string_expression*:</span></span>

*  <span data-ttu-id="08560-899">Pokud *interpolated_regular_string_whole* nebo *interpolated_verbatim_string_whole* následuje symbol `$`, pak je řetězcový literál formátu tímto tokenem.</span><span class="sxs-lookup"><span data-stu-id="08560-899">If an *interpolated_regular_string_whole* or an *interpolated_verbatim_string_whole* follows the `$` sign, then the format string literal is that token.</span></span>
*  <span data-ttu-id="08560-900">V opačném případě se řetězcový literál formátu skládá z:</span><span class="sxs-lookup"><span data-stu-id="08560-900">Otherwise, the format string literal consists of:</span></span> 
   *  <span data-ttu-id="08560-901">První *interpolated_regular_string_start* nebo *interpolated_verbatim_string_start*</span><span class="sxs-lookup"><span data-stu-id="08560-901">First the *interpolated_regular_string_start* or *interpolated_verbatim_string_start*</span></span>
   *  <span data-ttu-id="08560-902">Pak pro každé číslo `I` z `0` na `N-1`:</span><span class="sxs-lookup"><span data-stu-id="08560-902">Then for each number `I` from `0` to `N-1`:</span></span> 
      * <span data-ttu-id="08560-903">Desítková reprezentace `I`</span><span class="sxs-lookup"><span data-stu-id="08560-903">The decimal representation of `I`</span></span>
      * <span data-ttu-id="08560-904">V případě, že odpovídající *interpolace* má *constant_expression*, `,` (čárka) následovaný desetinným vyjádřením hodnoty *constant_expression*</span><span class="sxs-lookup"><span data-stu-id="08560-904">Then, if the corresponding *interpolation* has a *constant_expression*, a `,` (comma) followed by the decimal representation of the value of the *constant_expression*</span></span>
      * <span data-ttu-id="08560-905">Pak *interpolated_regular_string_mid*, *interpolated_regular_string_end*, *interpolated_verbatim_string_mid* nebo *interpolated_verbatim_string_end* hned za odpovídající interpolaci.</span><span class="sxs-lookup"><span data-stu-id="08560-905">Then the *interpolated_regular_string_mid*, *interpolated_regular_string_end*, *interpolated_verbatim_string_mid* or *interpolated_verbatim_string_end* immediately following the corresponding interpolation.</span></span>

<span data-ttu-id="08560-906">Následující argumenty jsou jednoduše *výrazy* z *interpolace* (pokud existují), v daném pořadí.</span><span class="sxs-lookup"><span data-stu-id="08560-906">The subsequent arguments are simply the *expressions* from the *interpolations* (if any), in order.</span></span>

<span data-ttu-id="08560-907">TODO: příklady</span><span class="sxs-lookup"><span data-stu-id="08560-907">TODO: examples.</span></span>


### <a name="simple-names"></a><span data-ttu-id="08560-908">Jednoduché názvy</span><span class="sxs-lookup"><span data-stu-id="08560-908">Simple names</span></span>

<span data-ttu-id="08560-909">*Simple_name* se skládá z identifikátoru, volitelně následovaný seznamem argumentů typu:</span><span class="sxs-lookup"><span data-stu-id="08560-909">A *simple_name* consists of an identifier, optionally followed by a type argument list:</span></span>

```antlr
simple_name
    : identifier type_argument_list?
    ;
```

<span data-ttu-id="08560-910">*Simple_name* je buď `I` formuláře, nebo `I<A1,...,Ak>`formuláře, kde `I` je jeden identifikátor a `<A1,...,Ak>` je volitelný *type_argument_list*.</span><span class="sxs-lookup"><span data-stu-id="08560-910">A *simple_name* is either of the form `I` or of the form `I<A1,...,Ak>`, where `I` is a single identifier and `<A1,...,Ak>` is an optional *type_argument_list*.</span></span> <span data-ttu-id="08560-911">Pokud není zadaný žádný *type_argument_list* , zvažte `K` na nulu.</span><span class="sxs-lookup"><span data-stu-id="08560-911">When no *type_argument_list* is specified, consider `K` to be zero.</span></span> <span data-ttu-id="08560-912">*Simple_name* je vyhodnocena a klasifikována následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="08560-912">The *simple_name* is evaluated and classified as follows:</span></span>

*  <span data-ttu-id="08560-913">Pokud je `K` nula a *simple_name* se objeví v rámci *bloku* *a v případě*, že prostor deklarace místní[proměnné (nebo](basic-concepts.md#declarations)ohraničujícího *bloku*) obsahuje místní proměnnou, parametr nebo konstantu s názvem `I`, pak *simple_name* odkazuje na tuto místní proměnnou, parametr nebo konstantu a je klasifikována jako proměnná nebo hodnota.</span><span class="sxs-lookup"><span data-stu-id="08560-913">If `K` is zero and the *simple_name* appears within a *block* and if the *block*'s (or an enclosing *block*'s) local variable declaration space ([Declarations](basic-concepts.md#declarations)) contains a local variable, parameter or constant with name `I`, then the *simple_name* refers to that local variable, parameter or constant and is classified as a variable or value.</span></span>
*  <span data-ttu-id="08560-914">Pokud je `K` nula a *simple_name* se objeví v těle deklarace obecné metody a pokud tato deklarace obsahuje parametr typu s názvem `I`, pak *simple_name* odkazuje na tento parametr typu.</span><span class="sxs-lookup"><span data-stu-id="08560-914">If `K` is zero and the *simple_name* appears within the body of a generic method declaration and if that declaration includes a type parameter with name `I`, then the *simple_name* refers to that type parameter.</span></span>
*  <span data-ttu-id="08560-915">V opačném případě pro každý typ instance `T` ([typ instance](classes.md#the-instance-type)), počínaje typem instance bezprostředně ohraničujícího typ deklarace a pokračování s typem instance každé nadřazené třídy nebo deklarace struktury (pokud existuje):</span><span class="sxs-lookup"><span data-stu-id="08560-915">Otherwise, for each instance type `T` ([The instance type](classes.md#the-instance-type)), starting with the instance type of the immediately enclosing type declaration and continuing with the instance type of each enclosing class or struct declaration (if any):</span></span>
   *  <span data-ttu-id="08560-916">Pokud je `K` nula a deklarace `T` obsahuje parametr typu s názvem `I`, pak *simple_name* odkazuje na tento parametr typu.</span><span class="sxs-lookup"><span data-stu-id="08560-916">If `K` is zero and the declaration of `T` includes a type parameter with name `I`, then the *simple_name* refers to that type parameter.</span></span>
   *  <span data-ttu-id="08560-917">V opačném případě, pokud vyhledávání členů ([vyhledávání členů](expressions.md#member-lookup)) `I` v `T` s `K` argumenty typu vytvoří shodu:</span><span class="sxs-lookup"><span data-stu-id="08560-917">Otherwise, if a member lookup ([Member lookup](expressions.md#member-lookup)) of `I` in `T` with `K` type arguments produces a match:</span></span>
      * <span data-ttu-id="08560-918">Pokud je `T` typ instance bezprostředně ohraničujícího typu třídy nebo struktury a vyhledávání identifikuje jednu nebo více metod, výsledkem je skupina metod s přidruženým výrazem instance `this`.</span><span class="sxs-lookup"><span data-stu-id="08560-918">If `T` is the instance type of the immediately enclosing class or struct type and the lookup identifies one or more methods, the result is a method group with an associated instance expression of `this`.</span></span> <span data-ttu-id="08560-919">Pokud byl zadán seznam argumentů typu, je použit při volání obecné metody ([vyvolání metod](expressions.md#method-invocations)).</span><span class="sxs-lookup"><span data-stu-id="08560-919">If a type argument list was specified, it is used in calling a generic method ([Method invocations](expressions.md#method-invocations)).</span></span>
      * <span data-ttu-id="08560-920">V opačném případě, pokud je `T` typ instance bezprostředně ohraničujícího typu třídy nebo struktury, pokud vyhledávání identifikuje člena instance a v případě, že k odkazu dojde v těle konstruktoru instance, metody instance nebo přístupového objektu instance, je výsledek stejný jako členský přístup ([členský přístup](expressions.md#member-access)) formuláře `this.I`.</span><span class="sxs-lookup"><span data-stu-id="08560-920">Otherwise, if `T` is the instance type of the immediately enclosing class or struct type, if the lookup identifies an instance member, and if the reference occurs within the body of an instance constructor, an instance method, or an instance accessor, the result is the same as a member access ([Member access](expressions.md#member-access)) of the form `this.I`.</span></span> <span data-ttu-id="08560-921">K tomu může dojít pouze v případě, že je `K` nula.</span><span class="sxs-lookup"><span data-stu-id="08560-921">This can only happen when `K` is zero.</span></span>
      * <span data-ttu-id="08560-922">V opačném případě je výsledek stejný jako členský přístup ([členský přístup](expressions.md#member-access)) formuláře `T.I` nebo `T.I<A1,...,Ak>`.</span><span class="sxs-lookup"><span data-stu-id="08560-922">Otherwise, the result is the same as a member access ([Member access](expressions.md#member-access)) of the form `T.I` or `T.I<A1,...,Ak>`.</span></span> <span data-ttu-id="08560-923">V tomto případě se jedná o chybu při vazbě *simple_name* pro odkazování na člena instance.</span><span class="sxs-lookup"><span data-stu-id="08560-923">In this case, it is a binding-time error for the *simple_name* to refer to an instance member.</span></span>

*  <span data-ttu-id="08560-924">V opačném případě pro každý `N`oboru názvů počínaje oborem názvů, ve kterém *simple_name* dochází, pokračuje v každém ohraničujícím oboru názvů (pokud existuje) a končí globálním oborem názvů, jsou vyhodnoceny následující kroky, dokud není nalezena entita:</span><span class="sxs-lookup"><span data-stu-id="08560-924">Otherwise, for each namespace `N`, starting with the namespace in which the *simple_name* occurs, continuing with each enclosing namespace (if any), and ending with the global namespace, the following steps are evaluated until an entity is located:</span></span>
   *  <span data-ttu-id="08560-925">Pokud je `K` nula a `I` je název oboru názvů v `N`, pak:</span><span class="sxs-lookup"><span data-stu-id="08560-925">If `K` is zero and `I` is the name of a namespace in `N`, then:</span></span>
      * <span data-ttu-id="08560-926">Pokud se umístění, kde *simple_name* dochází, je uzavřeno deklarací oboru názvů pro `N` a deklarace oboru názvů obsahuje *extern_alias_directive* nebo *using_alias_directive* , které přidruží název `I` k oboru názvů nebo typu, *simple_name* je nejednoznačný a dojde k chybě při kompilaci.</span><span class="sxs-lookup"><span data-stu-id="08560-926">If the location where the *simple_name* occurs is enclosed by a namespace declaration for `N` and the namespace declaration contains an *extern_alias_directive* or *using_alias_directive* that associates the name `I` with a namespace or type, then the *simple_name* is ambiguous and a compile-time error occurs.</span></span>
      * <span data-ttu-id="08560-927">V opačném případě *simple_name* odkazuje na obor názvů s názvem `I` v `N`.</span><span class="sxs-lookup"><span data-stu-id="08560-927">Otherwise, the *simple_name* refers to the namespace named `I` in `N`.</span></span>
   *  <span data-ttu-id="08560-928">Jinak, pokud `N` obsahuje přístupný typ s názvem `I` a `K` parametry typu, pak:</span><span class="sxs-lookup"><span data-stu-id="08560-928">Otherwise, if `N` contains an accessible type having name `I` and `K` type parameters, then:</span></span>
      * <span data-ttu-id="08560-929">Pokud je `K` nula a umístění, kde se *simple_name* vyskytuje, je uzavřeno deklarací oboru názvů pro `N` a deklarace oboru názvů obsahuje *extern_alias_directive* nebo *using_alias_directive* , který přidruží název `I` k oboru názvů nebo typu, *simple_name* je nejednoznačný a dojde k chybě při kompilaci.</span><span class="sxs-lookup"><span data-stu-id="08560-929">If `K` is zero and the location where the *simple_name* occurs is enclosed by a namespace declaration for `N` and the namespace declaration contains an *extern_alias_directive* or *using_alias_directive* that associates the name `I` with a namespace or type, then the *simple_name* is ambiguous and a compile-time error occurs.</span></span>
      * <span data-ttu-id="08560-930">V opačném případě *namespace_or_type_name* odkazuje na typ sestavený pomocí daných argumentů typu.</span><span class="sxs-lookup"><span data-stu-id="08560-930">Otherwise, the *namespace_or_type_name* refers to the type constructed with the given type arguments.</span></span>
   *  <span data-ttu-id="08560-931">V opačném případě, pokud je umístění, kde *simple_name* dojde, uzavřeno deklarací oboru názvů pro `N`:</span><span class="sxs-lookup"><span data-stu-id="08560-931">Otherwise, if the location where the *simple_name* occurs is enclosed by a namespace declaration for `N`:</span></span>
      * <span data-ttu-id="08560-932">Pokud je `K` nula a deklarace oboru názvů obsahuje *extern_alias_directive* nebo *using_alias_directive* , které přidruží název `I` k importovanému oboru názvů nebo typu, pak *simple_name* odkazuje na tento obor názvů nebo typ.</span><span class="sxs-lookup"><span data-stu-id="08560-932">If `K` is zero and the namespace declaration contains an *extern_alias_directive* or *using_alias_directive* that associates the name `I` with an imported namespace or type, then the *simple_name* refers to that namespace or type.</span></span>
      * <span data-ttu-id="08560-933">V opačném případě, pokud deklarace oborů názvů a typů importované pomocí *using_namespace_directive*s a *using_static_directive*s deklarací oboru názvů obsahují přesně jeden přístupný typ nebo statický člen bez přípony s názvem `I` a `K`parametry typu  , pak *simple_name* odkazuje na tento typ nebo člen vytvořený pomocí daných argumentů typu.</span><span class="sxs-lookup"><span data-stu-id="08560-933">Otherwise, if the namespaces and type declarations imported by the *using_namespace_directive*s and *using_static_directive*s of the namespace declaration contain exactly one accessible type or non-extension static member having name `I` and `K` type parameters, then the *simple_name* refers to that type or member constructed with the given type arguments.</span></span>
      * <span data-ttu-id="08560-934">V opačném případě, pokud obory názvů a typy importované pomocí *using_namespace_directive*s deklarací oboru názvů obsahují více než jeden přístupný typ nebo statický člen nerozšiřující metody s názvem `I` a `K` parametry typu, *simple_name* je nejednoznačný a dojde k chybě.</span><span class="sxs-lookup"><span data-stu-id="08560-934">Otherwise, if the namespaces and types imported by the *using_namespace_directive*s of the namespace declaration contain more than one accessible type or non-extension-method static member having name `I` and `K` type parameters, then the *simple_name* is ambiguous and an error occurs.</span></span>

   <span data-ttu-id="08560-935">Všimněte si, že celý krok je přesně rovnoběžný s odpovídajícím krokem při zpracování *namespace_or_type_name* ([obor názvů a názvy typů](basic-concepts.md#namespace-and-type-names)).</span><span class="sxs-lookup"><span data-stu-id="08560-935">Note that this entire step is exactly parallel to the corresponding step in the processing of a *namespace_or_type_name* ([Namespace and type names](basic-concepts.md#namespace-and-type-names)).</span></span>

*  <span data-ttu-id="08560-936">V opačném případě *simple_name* není definován a dojde k chybě při kompilaci.</span><span class="sxs-lookup"><span data-stu-id="08560-936">Otherwise, the *simple_name* is undefined and a compile-time error occurs.</span></span>


### <a name="parenthesized-expressions"></a><span data-ttu-id="08560-937">Výrazy v závorkách</span><span class="sxs-lookup"><span data-stu-id="08560-937">Parenthesized expressions</span></span>

<span data-ttu-id="08560-938">*Parenthesized_expression* se skládá z *výrazu* uzavřeného v závorkách.</span><span class="sxs-lookup"><span data-stu-id="08560-938">A *parenthesized_expression* consists of an *expression* enclosed in parentheses.</span></span>

```antlr
parenthesized_expression
    : '(' expression ')'
    ;
```

<span data-ttu-id="08560-939">*Parenthesized_expression* se vyhodnocuje vyhodnocením *výrazu* v závorkách.</span><span class="sxs-lookup"><span data-stu-id="08560-939">A *parenthesized_expression* is evaluated by evaluating the *expression* within the parentheses.</span></span> <span data-ttu-id="08560-940">Pokud *výraz* v závorkách označuje obor názvů nebo typ, dojde k chybě při kompilaci.</span><span class="sxs-lookup"><span data-stu-id="08560-940">If the *expression* within the parentheses denotes a namespace or type, a compile-time error occurs.</span></span> <span data-ttu-id="08560-941">V opačném případě je výsledek *parenthesized_expression* výsledkem vyhodnocení obsaženého *výrazu*.</span><span class="sxs-lookup"><span data-stu-id="08560-941">Otherwise, the result of the *parenthesized_expression* is the result of the evaluation of the contained *expression*.</span></span>

### <a name="member-access"></a><span data-ttu-id="08560-942">Přístup ke členům</span><span class="sxs-lookup"><span data-stu-id="08560-942">Member access</span></span>

<span data-ttu-id="08560-943">*Member_access* se skládá z *primary_expression*, *predefined_type*nebo *qualified_alias_member*, po kterém následuje token "`.`" následovaný *identifikátorem*, volitelně následovaným *type_argument_list*.</span><span class="sxs-lookup"><span data-stu-id="08560-943">A *member_access* consists of a *primary_expression*, a *predefined_type*, or a *qualified_alias_member*, followed by a "`.`" token, followed by an *identifier*, optionally followed by a *type_argument_list*.</span></span>

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

<span data-ttu-id="08560-944">*Qualified_alias_member* výroba je definována v [kvalifikátorech aliasů oboru názvů](namespaces.md#namespace-alias-qualifiers).</span><span class="sxs-lookup"><span data-stu-id="08560-944">The *qualified_alias_member* production is defined in [Namespace alias qualifiers](namespaces.md#namespace-alias-qualifiers).</span></span>

<span data-ttu-id="08560-945">*Member_access* je buď `E.I` formuláře, nebo `E.I<A1, ..., Ak>`formuláře, kde `E` je primární výraz, `I` je jedním identifikátorem a `<A1, ..., Ak>` je volitelná *type_argument_list*.</span><span class="sxs-lookup"><span data-stu-id="08560-945">A *member_access* is either of the form `E.I` or of the form `E.I<A1, ..., Ak>`, where `E` is a primary-expression, `I` is a single identifier and `<A1, ..., Ak>` is an optional *type_argument_list*.</span></span> <span data-ttu-id="08560-946">Pokud není zadaný žádný *type_argument_list* , zvažte `K` na nulu.</span><span class="sxs-lookup"><span data-stu-id="08560-946">When no *type_argument_list* is specified, consider `K` to be zero.</span></span>

<span data-ttu-id="08560-947">*Member_access* s *primary_expression* typu `dynamic` je dynamicky svázán ([dynamická vazba](expressions.md#dynamic-binding)).</span><span class="sxs-lookup"><span data-stu-id="08560-947">A *member_access* with a *primary_expression* of type `dynamic` is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)).</span></span> <span data-ttu-id="08560-948">V tomto případě kompilátor klasifikuje přístup členů jako vlastnost přístupu typu `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="08560-948">In this case the compiler classifies the member access as a property access of type `dynamic`.</span></span> <span data-ttu-id="08560-949">Níže uvedená pravidla určují význam *member_access* se pak použijí za běhu, a to pomocí běhového typu místo při kompilaci *primary_expression*.</span><span class="sxs-lookup"><span data-stu-id="08560-949">The rules below to determine the meaning of the *member_access* are then applied at run-time, using the run-time type instead of the compile-time type of the *primary_expression*.</span></span> <span data-ttu-id="08560-950">Pokud tato klasifikace za běhu vede na skupinu metod, musí být přístup člena *primary_expression* *invocation_expression*.</span><span class="sxs-lookup"><span data-stu-id="08560-950">If this run-time classification leads to a method group, then the member access must be the *primary_expression* of an *invocation_expression*.</span></span>

<span data-ttu-id="08560-951">*Member_access* je vyhodnocena a klasifikována následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="08560-951">The *member_access* is evaluated and classified as follows:</span></span>

*  <span data-ttu-id="08560-952">Pokud je `K` nula a `E` je obor názvů a `E` obsahuje vnořený obor názvů s názvem `I`, pak výsledkem je tento obor názvů.</span><span class="sxs-lookup"><span data-stu-id="08560-952">If `K` is zero and `E` is a namespace and `E` contains a nested namespace with name `I`, then the result is that namespace.</span></span>
*  <span data-ttu-id="08560-953">V opačném případě, pokud je `E` obor názvů a `E` obsahuje přístupný typ s názvem `I` a `K`parametry typu  , pak je výsledkem typ sestavený pomocí daných argumentů typu.</span><span class="sxs-lookup"><span data-stu-id="08560-953">Otherwise, if `E` is a namespace and `E` contains an accessible type having name `I` and `K` type parameters, then the result is that type constructed with the given type arguments.</span></span>
*  <span data-ttu-id="08560-954">Pokud je `E` *predefined_type* nebo *primary_expression* klasifikované jako typ, pokud `E` není parametr typu a pokud vyhledávání členů ([vyhledávání členů](expressions.md#member-lookup)) `I` v `E` s parametry typu `K` vytvoří shodu, bude `E.I` vyhodnocena a klasifikována takto:</span><span class="sxs-lookup"><span data-stu-id="08560-954">If `E` is a *predefined_type* or a *primary_expression* classified as a type, if `E` is not a type parameter, and if a member lookup ([Member lookup](expressions.md#member-lookup)) of `I` in `E` with `K` type parameters produces a match, then `E.I` is evaluated and classified as follows:</span></span>
   *  <span data-ttu-id="08560-955">Pokud `I` identifikuje typ, pak je výsledkem typ konstruovaný pomocí daných argumentů typu.</span><span class="sxs-lookup"><span data-stu-id="08560-955">If `I` identifies a type, then the result is that type constructed with the given type arguments.</span></span>
   *  <span data-ttu-id="08560-956">Pokud `I` identifikuje jednu nebo více metod, pak je výsledkem skupina metod bez přidruženého výrazu instance.</span><span class="sxs-lookup"><span data-stu-id="08560-956">If `I` identifies one or more methods, then the result is a method group with no associated instance expression.</span></span> <span data-ttu-id="08560-957">Pokud byl zadán seznam argumentů typu, je použit při volání obecné metody ([vyvolání metod](expressions.md#method-invocations)).</span><span class="sxs-lookup"><span data-stu-id="08560-957">If a type argument list was specified, it is used in calling a generic method ([Method invocations](expressions.md#method-invocations)).</span></span>
   *  <span data-ttu-id="08560-958">Pokud `I` identifikuje vlastnost `static`, je výsledkem přístup k vlastnosti bez přidruženého výrazu instance.</span><span class="sxs-lookup"><span data-stu-id="08560-958">If `I` identifies a `static` property, then the result is a property access with no associated instance expression.</span></span>
   *  <span data-ttu-id="08560-959">Pokud `I` identifikuje `static` pole:</span><span class="sxs-lookup"><span data-stu-id="08560-959">If `I` identifies a `static` field:</span></span>
      * <span data-ttu-id="08560-960">Pokud je pole `readonly` a odkaz se vyskytuje mimo statický konstruktor třídy nebo struktury, ve které je pole deklarováno, pak je výsledkem hodnota, konkrétně hodnota statického pole `I` v `E`.</span><span class="sxs-lookup"><span data-stu-id="08560-960">If the field is `readonly` and the reference occurs outside the static constructor of the class or struct in which the field is declared, then the result is a value, namely the value of the static field `I` in `E`.</span></span>
      * <span data-ttu-id="08560-961">V opačném případě je výsledkem proměnná, konkrétně pole static `I` v `E`.</span><span class="sxs-lookup"><span data-stu-id="08560-961">Otherwise, the result is a variable, namely the static field `I` in `E`.</span></span>
   *  <span data-ttu-id="08560-962">Pokud `I` identifikuje událost `static`:</span><span class="sxs-lookup"><span data-stu-id="08560-962">If `I` identifies a `static` event:</span></span>
      * <span data-ttu-id="08560-963">Pokud se odkaz vyskytuje v rámci třídy nebo struktury, ve které je událost deklarována, a událost byla deklarována bez *event_accessor_declarations* ([události](classes.md#events)), pak `E.I` je zpracována přesně tak, jak `I` byly statické pole.</span><span class="sxs-lookup"><span data-stu-id="08560-963">If the reference occurs within the class or struct in which the event is declared, and the event was declared without *event_accessor_declarations* ([Events](classes.md#events)), then `E.I` is processed exactly as if `I` were a static field.</span></span>
      * <span data-ttu-id="08560-964">V opačném případě je výsledkem přístup k události bez přidruženého výrazu instance.</span><span class="sxs-lookup"><span data-stu-id="08560-964">Otherwise, the result is an event access with no associated instance expression.</span></span>
   *  <span data-ttu-id="08560-965">Pokud `I` identifikuje konstantu, pak je výsledkem hodnota, jmenovitě hodnota této konstanty.</span><span class="sxs-lookup"><span data-stu-id="08560-965">If `I` identifies a constant, then the result is a value, namely the value of that constant.</span></span>
    * <span data-ttu-id="08560-966">Pokud `I` identifikuje člen výčtu, pak je výsledkem hodnota, konkrétně hodnota tohoto člena výčtu.</span><span class="sxs-lookup"><span data-stu-id="08560-966">If `I` identifies an enumeration member, then the result is a value, namely the value of that enumeration member.</span></span>
    * <span data-ttu-id="08560-967">V opačném případě je `E.I` neplatný odkaz na člena a dojde k chybě při kompilaci.</span><span class="sxs-lookup"><span data-stu-id="08560-967">Otherwise, `E.I` is an invalid member reference, and a compile-time error occurs.</span></span>
*  <span data-ttu-id="08560-968">Pokud je `E` přístup k vlastnostem, přístup indexeru, proměnná nebo hodnota, typ, který je `T`a vyhledávání členů ([vyhledávání členů](expressions.md#member-lookup)) `I` v `T` s `K` argumenty typu vytvoří shodu a `E.I` je vyhodnocena a klasifikována následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="08560-968">If `E` is a property access, indexer access, variable, or value, the type of which is `T`, and a member lookup ([Member lookup](expressions.md#member-lookup)) of `I` in `T` with `K` type arguments produces a match, then `E.I` is evaluated and classified as follows:</span></span>
   *  <span data-ttu-id="08560-969">Nejdřív, pokud `E` je vlastnost nebo přístup indexeru, se získá hodnota vlastnosti nebo přístupu indexeru ([hodnoty výrazů](expressions.md#values-of-expressions)) a `E` se překlasifikují jako hodnota.</span><span class="sxs-lookup"><span data-stu-id="08560-969">First, if `E` is a property or indexer access, then the value of the property or indexer access is obtained ([Values of expressions](expressions.md#values-of-expressions)) and `E` is reclassified as a value.</span></span>
   *  <span data-ttu-id="08560-970">Pokud `I` identifikuje jednu nebo více metod, pak je výsledkem skupina metod s přidruženým výrazem instance `E`.</span><span class="sxs-lookup"><span data-stu-id="08560-970">If `I` identifies one or more methods, then the result is a method group with an associated instance expression of `E`.</span></span> <span data-ttu-id="08560-971">Pokud byl zadán seznam argumentů typu, je použit při volání obecné metody ([vyvolání metod](expressions.md#method-invocations)).</span><span class="sxs-lookup"><span data-stu-id="08560-971">If a type argument list was specified, it is used in calling a generic method ([Method invocations](expressions.md#method-invocations)).</span></span>
   *  <span data-ttu-id="08560-972">Pokud `I` identifikuje vlastnost instance,</span><span class="sxs-lookup"><span data-stu-id="08560-972">If `I` identifies an instance property,</span></span>
      * <span data-ttu-id="08560-973">Pokud je `E` `this`, `I` identifikuje automaticky implementované vlastnosti ([automaticky implementované vlastnosti](classes.md#automatically-implemented-properties)) bez setter a odkaz se objeví v rámci konstruktoru instance pro třídu nebo typ struktury `T`, pak výsledkem je proměnná, konkrétně skryté pole zálohování pro auto-property dané `I` v instanci `T` dané `this`.</span><span class="sxs-lookup"><span data-stu-id="08560-973">If `E` is `this`, `I` identifies an automatically implemented property ([Automatically implemented properties](classes.md#automatically-implemented-properties)) without a setter, and the reference occurs within an instance constructor for a class or struct type `T`, then the result is a variable, namely the hidden backing field for the auto-property given by `I` in the instance of `T` given by `this`.</span></span>
      * <span data-ttu-id="08560-974">V opačném případě je výsledkem přístup k vlastnosti s přidruženým výrazem instance `E`.</span><span class="sxs-lookup"><span data-stu-id="08560-974">Otherwise, the result is a property access with an associated instance expression of `E`.</span></span>
   *  <span data-ttu-id="08560-975">Pokud je `T` *class_type* a `I` identifikuje pole instance daného *class_type*:</span><span class="sxs-lookup"><span data-stu-id="08560-975">If `T` is a *class_type* and `I` identifies an instance field of that *class_type*:</span></span>
      * <span data-ttu-id="08560-976">Je-li hodnota `E` `null`, je vyvolána `System.NullReferenceException`.</span><span class="sxs-lookup"><span data-stu-id="08560-976">If the value of `E` is `null`, then a `System.NullReferenceException` is thrown.</span></span>
      * <span data-ttu-id="08560-977">V opačném případě, pokud je pole `readonly` a odkaz se objeví mimo konstruktor instance třídy, ve které je pole deklarováno, pak je výsledkem hodnota, konkrétně hodnota pole `I` v objektu, na který odkazuje `E`.</span><span class="sxs-lookup"><span data-stu-id="08560-977">Otherwise, if the field is `readonly` and the reference occurs outside an instance constructor of the class in which the field is declared, then the result is a value, namely the value of the field `I` in the object referenced by `E`.</span></span>
      * <span data-ttu-id="08560-978">V opačném případě je výsledkem proměnná, konkrétně pole `I` v objektu, na který odkazuje `E`.</span><span class="sxs-lookup"><span data-stu-id="08560-978">Otherwise, the result is a variable, namely the field `I` in the object referenced by `E`.</span></span>
   *  <span data-ttu-id="08560-979">Pokud je `T` *struct_type* a `I` identifikuje pole instance daného *struct_type*:</span><span class="sxs-lookup"><span data-stu-id="08560-979">If `T` is a *struct_type* and `I` identifies an instance field of that *struct_type*:</span></span>
      * <span data-ttu-id="08560-980">Pokud `E` je hodnota, nebo pokud je pole `readonly` a odkaz se objeví mimo konstruktor instance struktury, ve které je pole deklarováno, pak je výsledkem hodnota, jmenovitě hodnota pole `I` v instanci struktury dané `E`.</span><span class="sxs-lookup"><span data-stu-id="08560-980">If `E` is a value, or if the field is `readonly` and the reference occurs outside an instance constructor of the struct in which the field is declared, then the result is a value, namely the value of the field `I` in the struct instance given by `E`.</span></span>
      * <span data-ttu-id="08560-981">V opačném případě je výsledkem proměnná, konkrétně pole `I` v instanci struktury dané `E`.</span><span class="sxs-lookup"><span data-stu-id="08560-981">Otherwise, the result is a variable, namely the field `I` in the struct instance given by `E`.</span></span>
   *  <span data-ttu-id="08560-982">Pokud `I` identifikuje událost instance:</span><span class="sxs-lookup"><span data-stu-id="08560-982">If `I` identifies an instance event:</span></span>
      * <span data-ttu-id="08560-983">Pokud se odkaz vyskytuje v rámci třídy nebo struktury, ve které je událost deklarována, a událost byla deklarována bez *event_accessor_declarations* ([události](classes.md#events)) a odkaz se nevyskytuje jako levá strana operátoru `+=` nebo `-=`, pak `E.I` je zpracována přesně tak, jako kdyby `I` pole instance.</span><span class="sxs-lookup"><span data-stu-id="08560-983">If the reference occurs within the class or struct in which the event is declared, and the event was declared without *event_accessor_declarations* ([Events](classes.md#events)), and the reference does not occur as the left-hand side of a `+=` or `-=` operator, then `E.I` is processed exactly as if `I` was an instance field.</span></span>
      * <span data-ttu-id="08560-984">V opačném případě je výsledkem přístup k události s přidruženým výrazem instance `E`.</span><span class="sxs-lookup"><span data-stu-id="08560-984">Otherwise, the result is an event access with an associated instance expression of `E`.</span></span>
*  <span data-ttu-id="08560-985">V opačném případě se pokusí zpracovat `E.I` jako volání metody rozšíření ([vyvolání rozšiřující metody](expressions.md#extension-method-invocations)).</span><span class="sxs-lookup"><span data-stu-id="08560-985">Otherwise, an attempt is made to process `E.I` as an extension method invocation ([Extension method invocations](expressions.md#extension-method-invocations)).</span></span> <span data-ttu-id="08560-986">Pokud se to nepovede, `E.I` je neplatný odkaz na člena a dojde k chybě při vazbě.</span><span class="sxs-lookup"><span data-stu-id="08560-986">If this fails, `E.I` is an invalid member reference, and a binding-time error occurs.</span></span>

#### <a name="identical-simple-names-and-type-names"></a><span data-ttu-id="08560-987">Stejné jednoduché názvy a názvy typů</span><span class="sxs-lookup"><span data-stu-id="08560-987">Identical simple names and type names</span></span>

<span data-ttu-id="08560-988">V případě přístupu člena formuláře `E.I`, je-li `E` jeden identifikátor, a pokud je význam `E` jako *simple_name* ([jednoduché názvy](expressions.md#simple-names)) konstanta, pole, vlastnost, místní proměnná nebo parametr se stejným typem, jako je například význam `E` jako *TYPE_NAME* ([obor názvů a názvy typů](basic-concepts.md#namespace-and-type-names)), pak jsou povoleny oba možné významy `E`.</span><span class="sxs-lookup"><span data-stu-id="08560-988">In a member access of the form `E.I`, if `E` is a single identifier, and if the meaning of `E` as a *simple_name* ([Simple names](expressions.md#simple-names)) is a constant, field, property, local variable, or parameter with the same type as the meaning of `E` as a *type_name* ([Namespace and type names](basic-concepts.md#namespace-and-type-names)), then both possible meanings of `E` are permitted.</span></span> <span data-ttu-id="08560-989">Dvě možné významy `E.I` nejsou nikdy dvojznačné, protože `I` musí být nutně členem typu `E` v obou případech.</span><span class="sxs-lookup"><span data-stu-id="08560-989">The two possible meanings of `E.I` are never ambiguous, since `I` must necessarily be a member of the type `E` in both cases.</span></span> <span data-ttu-id="08560-990">Jinými slovy pravidlo jednoduše povoluje přístup ke statickým členům a vnořeným typům `E`, kde by jinak došlo k chybě při kompilaci.</span><span class="sxs-lookup"><span data-stu-id="08560-990">In other words, the rule simply permits access to the static members and nested types of `E` where a compile-time error would otherwise have occurred.</span></span> <span data-ttu-id="08560-991">Příklad:</span><span class="sxs-lookup"><span data-stu-id="08560-991">For example:</span></span>
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

#### <a name="grammar-ambiguities"></a><span data-ttu-id="08560-992">Nejednoznačnost gramatiky</span><span class="sxs-lookup"><span data-stu-id="08560-992">Grammar ambiguities</span></span>

<span data-ttu-id="08560-993">Výroby pro *simple_name* ([jednoduché názvy](expressions.md#simple-names)) a *member_access* ([přístup do členů](expressions.md#member-access)) mohou vést k nejednoznačnosti v gramatice pro výrazy.</span><span class="sxs-lookup"><span data-stu-id="08560-993">The productions for *simple_name* ([Simple names](expressions.md#simple-names)) and *member_access* ([Member access](expressions.md#member-access)) can give rise to ambiguities in the grammar for expressions.</span></span> <span data-ttu-id="08560-994">Například příkaz:</span><span class="sxs-lookup"><span data-stu-id="08560-994">For example, the statement:</span></span>
```csharp
F(G<A,B>(7));
```
<span data-ttu-id="08560-995">může být interpretován jako volání `F` se dvěma argumenty, `G < A` a `B > (7)`.</span><span class="sxs-lookup"><span data-stu-id="08560-995">could be interpreted as a call to `F` with two arguments, `G < A` and `B > (7)`.</span></span> <span data-ttu-id="08560-996">Alternativně může být interpretován jako volání `F` s jedním argumentem, což je volání obecné metody `G` se dvěma argumenty typu a jedním regulárním argumentem.</span><span class="sxs-lookup"><span data-stu-id="08560-996">Alternatively, it could be interpreted as a call to `F` with one argument, which is a call to a generic method `G` with two type arguments and one regular argument.</span></span>

<span data-ttu-id="08560-997">Pokud je možné sekvenci tokenů analyzovat (v kontextu) jako *simple_name* ([jednoduché názvy](expressions.md#simple-names)), *member_access* ([přístup k přístupu ke členům](expressions.md#member-access)) nebo *pointer_member_access* ([přístup ke členu ukazatele](unsafe-code.md#pointer-member-access)) končící *type_argument_list* ([argumenty typu](types.md#type-arguments)), token se hned po vyzkoušení uzavíracího `>` tokenu posuzuje.</span><span class="sxs-lookup"><span data-stu-id="08560-997">If a sequence of tokens can be parsed (in context) as a *simple_name* ([Simple names](expressions.md#simple-names)), *member_access* ([Member access](expressions.md#member-access)), or *pointer_member_access* ([Pointer member access](unsafe-code.md#pointer-member-access)) ending with a *type_argument_list* ([Type arguments](types.md#type-arguments)), the token immediately following the closing `>` token is examined.</span></span> <span data-ttu-id="08560-998">Pokud je jeden z</span><span class="sxs-lookup"><span data-stu-id="08560-998">If it is one of</span></span>
```csharp
(  )  ]  }  :  ;  ,  .  ?  ==  !=  |  ^
```
<span data-ttu-id="08560-999">pak je *type_argument_list* uchován jako součást *simple_name*, *member_access* nebo *pointer_member_access* a jakékoli další možné analýzy sekvence tokenů jsou zahozeny.</span><span class="sxs-lookup"><span data-stu-id="08560-999">then the *type_argument_list* is retained as part of the *simple_name*, *member_access* or *pointer_member_access* and any other possible parse of the sequence of tokens is discarded.</span></span> <span data-ttu-id="08560-1000">V opačném případě se *type_argument_list* nepovažují za součást *simple_name*, *member_access* nebo *pointer_member_access*, a to i v případě, že není k dispozici žádné jiné možné analýzy sekvence tokenů.</span><span class="sxs-lookup"><span data-stu-id="08560-1000">Otherwise, the *type_argument_list* is not considered to be part of the *simple_name*, *member_access* or *pointer_member_access*, even if there is no other possible parse of the sequence of tokens.</span></span> <span data-ttu-id="08560-1001">Všimněte si, že tato pravidla se při analýze *type_argument_list* v *namespace_or_type_name* ([obor názvů a názvy typů](basic-concepts.md#namespace-and-type-names)) nepoužívají.</span><span class="sxs-lookup"><span data-stu-id="08560-1001">Note that these rules are not applied when parsing a *type_argument_list* in a *namespace_or_type_name* ([Namespace and type names](basic-concepts.md#namespace-and-type-names)).</span></span> <span data-ttu-id="08560-1002">Příkaz</span><span class="sxs-lookup"><span data-stu-id="08560-1002">The statement</span></span>
```csharp
F(G<A,B>(7));
```
<span data-ttu-id="08560-1003">bude podle tohoto pravidla interpretována jako volání `F` s jedním argumentem, což je volání obecné metody `G` se dvěma argumenty typu a jedním regulárním argumentem.</span><span class="sxs-lookup"><span data-stu-id="08560-1003">will, according to this rule, be interpreted as a call to `F` with one argument, which is a call to a generic method `G` with two type arguments and one regular argument.</span></span> <span data-ttu-id="08560-1004">Příkazy</span><span class="sxs-lookup"><span data-stu-id="08560-1004">The statements</span></span>
```csharp
F(G < A, B > 7);
F(G < A, B >> 7);
```
<span data-ttu-id="08560-1005">každý bude interpretován jako volání `F` se dvěma argumenty.</span><span class="sxs-lookup"><span data-stu-id="08560-1005">will each be interpreted as a call to `F` with two arguments.</span></span> <span data-ttu-id="08560-1006">Příkaz</span><span class="sxs-lookup"><span data-stu-id="08560-1006">The statement</span></span>
```csharp
x = F < A > +y;
```
<span data-ttu-id="08560-1007">bude interpretován jako operátor menší než, operátor větší než a unární operátor plus, jako by byl například zápis příkazu `x = (F < A) > (+y)`namísto *simple_name* s *type_argument_list* následovanou binárním operátorem Plus.</span><span class="sxs-lookup"><span data-stu-id="08560-1007">will be interpreted as a less than operator, greater than operator, and unary plus operator, as if the statement had been written `x = (F < A) > (+y)`, instead of as a *simple_name* with a *type_argument_list* followed by a binary plus operator.</span></span> <span data-ttu-id="08560-1008">V příkazu</span><span class="sxs-lookup"><span data-stu-id="08560-1008">In the statement</span></span>
```csharp
x = y is C<T> + z;
```
<span data-ttu-id="08560-1009">tokeny `C<T>` jsou interpretovány jako *namespace_or_type_name* s *type_argument_list*.</span><span class="sxs-lookup"><span data-stu-id="08560-1009">the tokens `C<T>` are interpreted as a *namespace_or_type_name* with a *type_argument_list*.</span></span>

### <a name="invocation-expressions"></a><span data-ttu-id="08560-1010">Výrazy vyvolání</span><span class="sxs-lookup"><span data-stu-id="08560-1010">Invocation expressions</span></span>

<span data-ttu-id="08560-1011">K vyvolání metody se používá *invocation_expression* .</span><span class="sxs-lookup"><span data-stu-id="08560-1011">An *invocation_expression* is used to invoke a method.</span></span>

```antlr
invocation_expression
    : primary_expression '(' argument_list? ')'
    ;
```

<span data-ttu-id="08560-1012">*Invocation_expression* je dynamicky svázán ([dynamická vazba](expressions.md#dynamic-binding)), pokud alespoň jedna z následujících možností obsahuje:</span><span class="sxs-lookup"><span data-stu-id="08560-1012">An *invocation_expression* is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)) if at least one of the following holds:</span></span>

* <span data-ttu-id="08560-1013">*Primary_expression* má `dynamic`typu pro dobu kompilace.</span><span class="sxs-lookup"><span data-stu-id="08560-1013">The *primary_expression* has compile-time type `dynamic`.</span></span>
* <span data-ttu-id="08560-1014">Nejméně jeden argument volitelného *argument_list* má `dynamic` typu pro dobu kompilace a *primary_expression* nemá typ delegáta.</span><span class="sxs-lookup"><span data-stu-id="08560-1014">At least one argument of the optional *argument_list* has compile-time type `dynamic` and the *primary_expression* does not have a delegate type.</span></span>

<span data-ttu-id="08560-1015">V tomto případě kompilátor klasifikuje *invocation_expression* jako hodnotu typu `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="08560-1015">In this case the compiler classifies the *invocation_expression* as a value of type `dynamic`.</span></span> <span data-ttu-id="08560-1016">Níže uvedená pravidla určují význam *invocation_expression* se pak použijí za běhu, a to pomocí běhového typu místo za kompilace pro *primary_expression* a argumentů, které mají typ doby kompilace `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="08560-1016">The rules below to determine the meaning of the *invocation_expression* are then applied at run-time, using the run-time type instead of the compile-time type of those of the *primary_expression* and arguments which have the compile-time type `dynamic`.</span></span> <span data-ttu-id="08560-1017">Pokud *primary_expression* nemá `dynamic`typu za kompilace, pak volání metody bude mít za následek omezené doby kompilace, jak je popsáno v tématu [Kontrola dynamického přetěžování při kompilaci](expressions.md#compile-time-checking-of-dynamic-overload-resolution).</span><span class="sxs-lookup"><span data-stu-id="08560-1017">If the *primary_expression* does not have compile-time type `dynamic`, then the method invocation undergoes a limited compile time check as described in [Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution).</span></span>

<span data-ttu-id="08560-1018">*Primary_expression* *invocation_expression* musí být skupina metod nebo hodnota *delegate_type*.</span><span class="sxs-lookup"><span data-stu-id="08560-1018">The *primary_expression* of an *invocation_expression* must be a method group or a value of a *delegate_type*.</span></span> <span data-ttu-id="08560-1019">Pokud je *primary_expression* skupinou metod, *invocation_expression* je volání metody ([vyvolání](expressions.md#method-invocations)metod).</span><span class="sxs-lookup"><span data-stu-id="08560-1019">If the *primary_expression* is a method group, the *invocation_expression* is a method invocation ([Method invocations](expressions.md#method-invocations)).</span></span> <span data-ttu-id="08560-1020">Pokud je *primary_expression* hodnotou *delegate_type*, *invocation_expression* je volání delegáta ([vyvolání delegátů](expressions.md#delegate-invocations)).</span><span class="sxs-lookup"><span data-stu-id="08560-1020">If the *primary_expression* is a value of a *delegate_type*, the *invocation_expression* is a delegate invocation ([Delegate invocations](expressions.md#delegate-invocations)).</span></span> <span data-ttu-id="08560-1021">Pokud *primary_expression* není ani skupina metod ani hodnota *delegate_type*, dojde k chybě při vazbě.</span><span class="sxs-lookup"><span data-stu-id="08560-1021">If the *primary_expression* is neither a method group nor a value of a *delegate_type*, a binding-time error occurs.</span></span>

<span data-ttu-id="08560-1022">Nepovinné *argument_list* ([seznamy argumentů](expressions.md#argument-lists)) poskytují hodnoty nebo odkazy na proměnné pro parametry metody.</span><span class="sxs-lookup"><span data-stu-id="08560-1022">The optional *argument_list* ([Argument lists](expressions.md#argument-lists)) provides values or variable references for the parameters of the method.</span></span>

<span data-ttu-id="08560-1023">Výsledek vyhodnocení *invocation_expression* je klasifikován následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="08560-1023">The result of evaluating an *invocation_expression* is classified as follows:</span></span>

*  <span data-ttu-id="08560-1024">Pokud *invocation_expression* vyvolá metodu nebo delegáta, který vrací `void`, výsledek není Nothing.</span><span class="sxs-lookup"><span data-stu-id="08560-1024">If the *invocation_expression* invokes a method or delegate that returns `void`, the result is nothing.</span></span> <span data-ttu-id="08560-1025">Výraz, který je klasifikován jako Nothing, je povolen pouze v kontextu *statement_expression* ([příkazy výrazu](statements.md#expression-statements)) nebo jako tělo *lambda_expression* ([výrazy anonymních funkcí](expressions.md#anonymous-function-expressions)).</span><span class="sxs-lookup"><span data-stu-id="08560-1025">An expression that is classified as nothing is permitted only in the context of a *statement_expression* ([Expression statements](statements.md#expression-statements)) or as the body of a *lambda_expression* ([Anonymous function expressions](expressions.md#anonymous-function-expressions)).</span></span> <span data-ttu-id="08560-1026">V opačném případě dojde k chybě při vazbě.</span><span class="sxs-lookup"><span data-stu-id="08560-1026">Otherwise a binding-time error occurs.</span></span>
*  <span data-ttu-id="08560-1027">V opačném případě je výsledkem hodnota typu vráceného metodou nebo delegátem.</span><span class="sxs-lookup"><span data-stu-id="08560-1027">Otherwise, the result is a value of the type returned by the method or delegate.</span></span>

#### <a name="method-invocations"></a><span data-ttu-id="08560-1028">Vyvolání metod</span><span class="sxs-lookup"><span data-stu-id="08560-1028">Method invocations</span></span>

<span data-ttu-id="08560-1029">Pro vyvolání metody musí být *primary_expression* *invocation_expression* skupinou metod.</span><span class="sxs-lookup"><span data-stu-id="08560-1029">For a method invocation, the *primary_expression* of the *invocation_expression* must be a method group.</span></span> <span data-ttu-id="08560-1030">Skupina metod identifikuje jednu metodu, která se má vyvolat, nebo sadu přetížených metod, ze kterých se má vybrat konkrétní metoda, která se má vyvolat.</span><span class="sxs-lookup"><span data-stu-id="08560-1030">The method group identifies the one method to invoke or the set of overloaded methods from which to choose a specific method to invoke.</span></span> <span data-ttu-id="08560-1031">V druhém případě je určení konkrétní metody vyvolání odvozeno z kontextu poskytnutého typy argumentů v *argument_list*.</span><span class="sxs-lookup"><span data-stu-id="08560-1031">In the latter case, determination of the specific method to invoke is based on the context provided by the types of the arguments in the *argument_list*.</span></span>

<span data-ttu-id="08560-1032">Zpracování volání metody formuláře `M(A)`, kde `M` je skupina metod (případně včetně *type_argument_list*) a `A` je volitelný *argument_list*, sestává z následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="08560-1032">The binding-time processing of a method invocation of the form `M(A)`, where `M` is a method group (possibly including a *type_argument_list*), and `A` is an optional *argument_list*, consists of the following steps:</span></span>

*  <span data-ttu-id="08560-1033">Sada kandidátních metod pro vyvolání metody je vytvořena.</span><span class="sxs-lookup"><span data-stu-id="08560-1033">The set of candidate methods for the method invocation is constructed.</span></span> <span data-ttu-id="08560-1034">Pro každou metodu `F` přidruženou ke skupině metod `M`:</span><span class="sxs-lookup"><span data-stu-id="08560-1034">For each method `F` associated with the method group `M`:</span></span>
   *  <span data-ttu-id="08560-1035">Pokud je `F` neobecné, `F` je kandidátem v těchto případech:</span><span class="sxs-lookup"><span data-stu-id="08560-1035">If `F` is non-generic, `F` is a candidate when:</span></span>
      * <span data-ttu-id="08560-1036">`M` nemá žádný seznam argumentů typu a</span><span class="sxs-lookup"><span data-stu-id="08560-1036">`M` has no type argument list, and</span></span>
      * <span data-ttu-id="08560-1037">`F` platí pro `A` ([platný člen funkce](expressions.md#applicable-function-member)).</span><span class="sxs-lookup"><span data-stu-id="08560-1037">`F` is applicable with respect to `A` ([Applicable function member](expressions.md#applicable-function-member)).</span></span>
   *  <span data-ttu-id="08560-1038">Pokud je `F` obecný a `M` nemá žádný typ argumentu, `F` je kandidátem v těchto případech:</span><span class="sxs-lookup"><span data-stu-id="08560-1038">If `F` is generic and `M` has no type argument list, `F` is a candidate when:</span></span>
      * <span data-ttu-id="08560-1039">Odvození typu ([odvození typu](expressions.md#type-inference)) je úspěšné, odvození seznamu argumentů typu pro volání a</span><span class="sxs-lookup"><span data-stu-id="08560-1039">Type inference ([Type inference](expressions.md#type-inference)) succeeds, inferring a list of type arguments for the call, and</span></span>
      * <span data-ttu-id="08560-1040">Jakmile jsou argumenty odvozeného typu nahrazeny odpovídajícími parametry typu metody, všechny vytvořené typy v seznamu parametrů F splní svá omezení ([splňující omezení](types.md#satisfying-constraints)) a seznam parametrů `F` lze použít s ohledem na `A` ([platný člen funkce](expressions.md#applicable-function-member)).</span><span class="sxs-lookup"><span data-stu-id="08560-1040">Once the inferred type arguments are substituted for the corresponding method type parameters, all constructed types in the parameter list of F satisfy their constraints ([Satisfying constraints](types.md#satisfying-constraints)), and the parameter list of `F` is applicable with respect to `A` ([Applicable function member](expressions.md#applicable-function-member)).</span></span>
   *  <span data-ttu-id="08560-1041">Pokud je `F` obecný a `M` obsahuje seznam argumentů typu, `F` je kandidátem v těchto případech:</span><span class="sxs-lookup"><span data-stu-id="08560-1041">If `F` is generic and `M` includes a type argument list, `F` is a candidate when:</span></span>
      * <span data-ttu-id="08560-1042">`F` má stejný počet parametrů typu metody, které byly zadány v seznamu argumentů typu a</span><span class="sxs-lookup"><span data-stu-id="08560-1042">`F` has the same number of method type parameters as were supplied in the type argument list, and</span></span>
      * <span data-ttu-id="08560-1043">Jakmile jsou argumenty typu nahrazeny odpovídajícími parametry typu metody, všechny vytvořené typy v seznamu parametrů F splní svá omezení ([splňující omezení](types.md#satisfying-constraints)) a seznam parametrů `F` lze použít s ohledem na `A` ([platný člen funkce](expressions.md#applicable-function-member)).</span><span class="sxs-lookup"><span data-stu-id="08560-1043">Once the type arguments are substituted for the corresponding method type parameters, all constructed types in the parameter list of F satisfy their constraints ([Satisfying constraints](types.md#satisfying-constraints)), and the parameter list of `F` is applicable with respect to `A` ([Applicable function member](expressions.md#applicable-function-member)).</span></span>
*  <span data-ttu-id="08560-1044">Sada kandidátních metod je zmenšena tak, aby obsahovala pouze metody z nejvíc odvozených typů: pro každou metodu `C.F` v sadě, kde `C` je typ, ve kterém je deklarována metoda `F`, všechny metody deklarované v základním typu `C` jsou ze sady odebrány.</span><span class="sxs-lookup"><span data-stu-id="08560-1044">The set of candidate methods is reduced to contain only methods from the most derived types: For each method `C.F` in the set, where `C` is the type in which the method `F` is declared, all methods declared in a base type of `C` are removed from the set.</span></span> <span data-ttu-id="08560-1045">Kromě toho, pokud `C` je typ třídy jiný než `object`, všechny metody deklarované v typu rozhraní jsou ze sady odebrány.</span><span class="sxs-lookup"><span data-stu-id="08560-1045">Furthermore, if `C` is a class type other than `object`, all methods declared in an interface type are removed from the set.</span></span> <span data-ttu-id="08560-1046">(Toto druhé pravidlo má vliv pouze v případě, že skupina metod byla výsledkem vyhledávání členů u parametru typu, který má platnou základní třídu jinou než objekt a neprázdnou sadu efektivních rozhraní.)</span><span class="sxs-lookup"><span data-stu-id="08560-1046">(This latter rule only has affect when the method group was the result of a member lookup on a type parameter having an effective base class other than object and a non-empty effective interface set.)</span></span>
*  <span data-ttu-id="08560-1047">Pokud je výsledná sada kandidátních metod prázdná, pak je další zpracování v následujících krocích opuštěno a místo toho je proveden pokus o zpracování vyvolání jako volání metody rozšíření ([vyvolání rozšiřující metody](expressions.md#extension-method-invocations)).</span><span class="sxs-lookup"><span data-stu-id="08560-1047">If the resulting set of candidate methods is empty, then further processing along the following steps are abandoned, and instead an attempt is made to process the invocation as an extension method invocation ([Extension method invocations](expressions.md#extension-method-invocations)).</span></span> <span data-ttu-id="08560-1048">Pokud se to nepovede, neexistují žádné použitelné metody a dojde k chybě při vazbě.</span><span class="sxs-lookup"><span data-stu-id="08560-1048">If this fails, then no applicable methods exist, and a binding-time error occurs.</span></span>
*  <span data-ttu-id="08560-1049">Nejlepší metoda sady kandidátních metod je identifikována pomocí pravidel řešení přetížení v rámci [rozlišení přetížení](expressions.md#overload-resolution).</span><span class="sxs-lookup"><span data-stu-id="08560-1049">The best method of the set of candidate methods is identified using the overload resolution rules of [Overload resolution](expressions.md#overload-resolution).</span></span> <span data-ttu-id="08560-1050">Pokud nelze identifikovat jednu nejlepší metodu, volání metody je nejednoznačné a dojde k chybě při vazbě.</span><span class="sxs-lookup"><span data-stu-id="08560-1050">If a single best method cannot be identified, the method invocation is ambiguous, and a binding-time error occurs.</span></span> <span data-ttu-id="08560-1051">Při provádění řešení přetížení jsou parametry obecné metody zváženy po nahrazení argumentů typu (dodaných nebo odvozených) pro odpovídající parametry typu metody.</span><span class="sxs-lookup"><span data-stu-id="08560-1051">When performing overload resolution, the parameters of a generic method are considered after substituting the type arguments (supplied or inferred) for the corresponding method type parameters.</span></span>
*  <span data-ttu-id="08560-1052">Konečné ověření zvolené nejlepší metody je provedeno:</span><span class="sxs-lookup"><span data-stu-id="08560-1052">Final validation of the chosen best method is performed:</span></span>
   * <span data-ttu-id="08560-1053">Metoda je ověřena v kontextu skupiny metod: Pokud je nejlepší metodou statická metoda, musí být skupina metod výsledkem *simple_name* nebo *member_access* prostřednictvím typu.</span><span class="sxs-lookup"><span data-stu-id="08560-1053">The method is validated in the context of the method group: If the best method is a static method, the method group must have resulted from a *simple_name* or a *member_access* through a type.</span></span> <span data-ttu-id="08560-1054">Pokud je nejlepším způsobem metoda instance, skupina metod musí mít výsledek z *simple_name*, *member_access* prostřednictvím proměnné nebo hodnoty nebo *base_access*.</span><span class="sxs-lookup"><span data-stu-id="08560-1054">If the best method is an instance method, the method group must have resulted from a *simple_name*, a *member_access* through a variable or value, or a *base_access*.</span></span> <span data-ttu-id="08560-1055">Pokud ani jedna z těchto požadavků není pravdivá, dojde k chybě při vazbě.</span><span class="sxs-lookup"><span data-stu-id="08560-1055">If neither of these requirements is true, a binding-time error occurs.</span></span>
   * <span data-ttu-id="08560-1056">Pokud je nejlepší metodou obecná metoda, jsou argumenty typu (dodané nebo odvozené) zkontrolovány proti omezením ([vyhovujícím omezením](types.md#satisfying-constraints)) deklarovaným v obecné metodě.</span><span class="sxs-lookup"><span data-stu-id="08560-1056">If the best method is a generic method, the type arguments (supplied or inferred) are checked against the constraints ([Satisfying constraints](types.md#satisfying-constraints)) declared on the generic method.</span></span> <span data-ttu-id="08560-1057">Pokud jakýkoli argument typu nesplňuje odpovídající omezení pro parametr typu, dojde k chybě při vazbě.</span><span class="sxs-lookup"><span data-stu-id="08560-1057">If any type argument does not satisfy the corresponding constraint(s) on the type parameter, a binding-time error occurs.</span></span>

<span data-ttu-id="08560-1058">Jakmile je metoda vybrána a ověřována v době vytvoření vazby výše uvedenými kroky, je skutečné vyvolání za běhu zpracováno podle pravidel volání člena funkce popsaných v tématu [Kontrola dynamického přetěžování při kompilaci](expressions.md#compile-time-checking-of-dynamic-overload-resolution).</span><span class="sxs-lookup"><span data-stu-id="08560-1058">Once a method has been selected and validated at binding-time by the above steps, the actual run-time invocation is processed according to the rules of function member invocation described in [Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution).</span></span>

<span data-ttu-id="08560-1059">Intuitivní efekt výše popsaných pravidel řešení je následující: Chcete-li najít konkrétní metodu vyvolanou voláním metody, začněte typem označeným voláním metody a pokračujte v řetězení dědičnosti, dokud alespoň jedno použitelné, byla nalezena dostupná, nepřepisovaná deklarace metody.</span><span class="sxs-lookup"><span data-stu-id="08560-1059">The intuitive effect of the resolution rules described above is as follows: To locate the particular method invoked by a method invocation, start with the type indicated by the method invocation and proceed up the inheritance chain until at least one applicable, accessible, non-override method declaration is found.</span></span> <span data-ttu-id="08560-1060">Pak proveďte odvození typu a rozlišení přetěžování v sadě použitelných, přístupných metod, které nejsou deklarovány v tomto typu a zavolejte metodu, která je vybrána.</span><span class="sxs-lookup"><span data-stu-id="08560-1060">Then perform type inference and overload resolution on the set of applicable, accessible, non-override methods declared in that type and invoke the method thus selected.</span></span> <span data-ttu-id="08560-1061">Pokud nebyla nalezena žádná metoda, zkuste místo toho zpracovat vyvolání jako volání metody rozšíření.</span><span class="sxs-lookup"><span data-stu-id="08560-1061">If no method was found, try instead to process the invocation as an extension method invocation.</span></span>

#### <a name="extension-method-invocations"></a><span data-ttu-id="08560-1062">Vyvolání metod rozšíření</span><span class="sxs-lookup"><span data-stu-id="08560-1062">Extension method invocations</span></span>

<span data-ttu-id="08560-1063">V volání metody ([vyvolání u zabalených instancí](expressions.md#invocations-on-boxed-instances)) jednoho z formulářů</span><span class="sxs-lookup"><span data-stu-id="08560-1063">In a method invocation ([Invocations on boxed instances](expressions.md#invocations-on-boxed-instances)) of one of the forms</span></span>
```csharp
expr . identifier ( )

expr . identifier ( args )

expr . identifier < typeargs > ( )

expr . identifier < typeargs > ( args )
```
<span data-ttu-id="08560-1064">Pokud normální zpracování vyvolání nenajde žádné použitelné metody, je proveden pokus o zpracování konstrukce jako volání metody rozšíření.</span><span class="sxs-lookup"><span data-stu-id="08560-1064">if the normal processing of the invocation finds no applicable methods, an attempt is made to process the construct as an extension method invocation.</span></span> <span data-ttu-id="08560-1065">Pokud *expr* nebo kterákoli z *argumentů* má typ doby kompilace `dynamic`, metody rozšíření nebudou použity.</span><span class="sxs-lookup"><span data-stu-id="08560-1065">If *expr* or any of the *args* has compile-time type `dynamic`, extension methods will not apply.</span></span>

<span data-ttu-id="08560-1066">Cílem je najít nejlepší *type_name* `C`, aby mohlo probíhat odpovídající volání statické metody:</span><span class="sxs-lookup"><span data-stu-id="08560-1066">The objective is to find the best *type_name* `C`, so that the corresponding static method invocation can take place:</span></span>
```csharp
C . identifier ( expr )

C . identifier ( expr , args )

C . identifier < typeargs > ( expr )

C . identifier < typeargs > ( expr , args )
```

<span data-ttu-id="08560-1067">Metoda rozšíření `Ci.Mj` má ***nárok*** v případě, že:</span><span class="sxs-lookup"><span data-stu-id="08560-1067">An extension method `Ci.Mj` is ***eligible*** if:</span></span>

*  <span data-ttu-id="08560-1068">`Ci` je neobecnou nevnořenou třídou.</span><span class="sxs-lookup"><span data-stu-id="08560-1068">`Ci` is a non-generic, non-nested class</span></span>
*  <span data-ttu-id="08560-1069">Název `Mj` je *identifikátor*</span><span class="sxs-lookup"><span data-stu-id="08560-1069">The name of `Mj` is *identifier*</span></span>
*  <span data-ttu-id="08560-1070">`Mj` je přístupná a použitelná při použití na argumenty jako statická metoda, jak je uvedeno výše.</span><span class="sxs-lookup"><span data-stu-id="08560-1070">`Mj` is accessible and applicable when applied to the arguments as a static method as shown above</span></span>
*  <span data-ttu-id="08560-1071">Implicitní identita, odkaz nebo zabalení převádění existují z *výrazu* na typ prvního parametru `Mj`.</span><span class="sxs-lookup"><span data-stu-id="08560-1071">An implicit identity, reference or boxing conversion exists from *expr* to the type of the first parameter of `Mj`.</span></span>

<span data-ttu-id="08560-1072">Hledání `C` pokračuje následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="08560-1072">The search for `C` proceeds as follows:</span></span>

*  <span data-ttu-id="08560-1073">Počínaje rozhraním nejbližší ohraničující deklarace oboru názvů pokračuje v každé deklaraci ohraničujícího oboru názvů a končí obsahující kompilační jednotka, pro nalezení kandidátních metod rozšíření je možné provést následující pokusy:</span><span class="sxs-lookup"><span data-stu-id="08560-1073">Starting with the closest enclosing namespace declaration, continuing with each enclosing namespace declaration, and ending with the containing compilation unit, successive attempts are made to find a candidate set of extension methods:</span></span>
   * <span data-ttu-id="08560-1074">Pokud zadaný obor názvů nebo kompilační jednotka přímo obsahuje deklarace neobecného typu `Ci` s oprávněnými metodami rozšíření `Mj`, pak sada těchto metod rozšíření je kandidátská sada.</span><span class="sxs-lookup"><span data-stu-id="08560-1074">If the given namespace or compilation unit directly contains non-generic type declarations `Ci` with eligible extension methods `Mj`, then the set of those extension methods is the candidate set.</span></span>
   * <span data-ttu-id="08560-1075">Pokud typy `Ci` importovat pomocí *using_static_declarations* a přímo deklarované v oborech názvů importovaných pomocí *using_namespace_directive*s v daném oboru názvů nebo jednotce kompilace přímo obsahují opravňující metody rozšíření `Mj`, pak sada těchto metod rozšíření je kandidátská sada.</span><span class="sxs-lookup"><span data-stu-id="08560-1075">If types `Ci` imported by *using_static_declarations* and directly declared in namespaces imported by *using_namespace_directive*s in the given namespace or compilation unit directly contain eligible extension methods `Mj`, then the set of those extension methods is the candidate set.</span></span>
*  <span data-ttu-id="08560-1076">Pokud se v žádné ohraničující deklaraci oboru názvů nebo jednotce kompilace nenajde žádná sada kandidátů, dojde k chybě při kompilaci.</span><span class="sxs-lookup"><span data-stu-id="08560-1076">If no candidate set is found in any enclosing namespace declaration or compilation unit, a compile-time error occurs.</span></span>
*  <span data-ttu-id="08560-1077">V opačném případě se rozlišení přetěžování aplikuje na sadu kandidátů, jak je popsáno v tématu ([řešení přetížení](expressions.md#overload-resolution)).</span><span class="sxs-lookup"><span data-stu-id="08560-1077">Otherwise, overload resolution is applied to the candidate set as described in ([Overload resolution](expressions.md#overload-resolution)).</span></span> <span data-ttu-id="08560-1078">Pokud se nenajde žádná nejlepší metoda, dojde k chybě při kompilaci.</span><span class="sxs-lookup"><span data-stu-id="08560-1078">If no single best method is found, a compile-time error occurs.</span></span>
*  <span data-ttu-id="08560-1079">`C` je typ, ve kterém je nejlepší metoda deklarovaná jako metoda rozšíření.</span><span class="sxs-lookup"><span data-stu-id="08560-1079">`C` is the type within which the best method is declared as an extension method.</span></span>

<span data-ttu-id="08560-1080">Použití `C` jako cíle, volání metody je pak zpracováno jako volání statické metody ([Kontrola při kompilaci dynamického přetěžování](expressions.md#compile-time-checking-of-dynamic-overload-resolution)).</span><span class="sxs-lookup"><span data-stu-id="08560-1080">Using `C` as a target, the method call is then processed as a static method invocation ([Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution)).</span></span>

<span data-ttu-id="08560-1081">Předchozí pravidla znamenají, že metody instance mají přednost před rozšiřujícími metodami, tyto metody rozšíření dostupné v deklaracích vnitřních oborů názvů mají přednost před rozšiřujícími metodami dostupnými v deklaracích vnějších oborů názvů a s tímto rozšířením. metody deklarované přímo v oboru názvů mají přednost před metodami rozšíření importovanými do stejného oboru názvů s použitím direktivy Namespace.</span><span class="sxs-lookup"><span data-stu-id="08560-1081">The preceding rules mean that instance methods take precedence over extension methods, that extension methods available in inner namespace declarations take precedence over extension methods available in outer namespace declarations, and that extension methods declared directly in a namespace take precedence over extension methods imported into that same namespace with a using namespace directive.</span></span> <span data-ttu-id="08560-1082">Příklad:</span><span class="sxs-lookup"><span data-stu-id="08560-1082">For example:</span></span>
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

<span data-ttu-id="08560-1083">V příkladu má metoda `B`přednost před první metodou rozšíření a metoda `C`má přednost před oběma metodami rozšíření.</span><span class="sxs-lookup"><span data-stu-id="08560-1083">In the example, `B`'s method takes precedence over the first extension method, and `C`'s method takes precedence over both extension methods.</span></span>

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

<span data-ttu-id="08560-1084">Výstup tohoto příkladu je:</span><span class="sxs-lookup"><span data-stu-id="08560-1084">The output of this example is:</span></span>
```console
E.F(1)
D.G(2)
C.H(3)
```
<span data-ttu-id="08560-1085">`D.G` má přednost před `C.G`a `E.F` má přednost před `D.F` a `C.F`.</span><span class="sxs-lookup"><span data-stu-id="08560-1085">`D.G` takes precedence over `C.G`, and `E.F` takes precedence over both `D.F` and `C.F`.</span></span>

#### <a name="delegate-invocations"></a><span data-ttu-id="08560-1086">Volání delegátů</span><span class="sxs-lookup"><span data-stu-id="08560-1086">Delegate invocations</span></span>

<span data-ttu-id="08560-1087">Pro vyvolání delegáta musí být *primary_expression* *invocation_expression* hodnotou *delegate_type*.</span><span class="sxs-lookup"><span data-stu-id="08560-1087">For a delegate invocation, the *primary_expression* of the *invocation_expression* must be a value of a *delegate_type*.</span></span> <span data-ttu-id="08560-1088">Kromě toho, vzhledem k tomu, že *delegate_type* být člen funkce se stejným seznamem parametrů jako *delegate_type*, *Delegate_type* musí být platný ([platný člen funkce](expressions.md#applicable-function-member)) s ohledem na *argument_list* *invocation_expression*.</span><span class="sxs-lookup"><span data-stu-id="08560-1088">Furthermore, considering the *delegate_type* to be a function member with the same parameter list as the *delegate_type*, the *delegate_type* must be applicable ([Applicable function member](expressions.md#applicable-function-member)) with respect to the *argument_list* of the *invocation_expression*.</span></span>

<span data-ttu-id="08560-1089">Běhové zpracování volání delegáta formuláře `D(A)`, kde `D` je *primary_expression* *delegate_type* a `A` je volitelná *argument_list*, sestává z následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="08560-1089">The run-time processing of a delegate invocation of the form `D(A)`, where `D` is a *primary_expression* of a *delegate_type* and `A` is an optional *argument_list*, consists of the following steps:</span></span>

*  <span data-ttu-id="08560-1090">`D` je vyhodnocena.</span><span class="sxs-lookup"><span data-stu-id="08560-1090">`D` is evaluated.</span></span> <span data-ttu-id="08560-1091">Pokud toto vyhodnocení způsobí výjimku, nejsou provedeny žádné další kroky.</span><span class="sxs-lookup"><span data-stu-id="08560-1091">If this evaluation causes an exception, no further steps are executed.</span></span>
*  <span data-ttu-id="08560-1092">Hodnota `D` je zaškrtnuta jako platná.</span><span class="sxs-lookup"><span data-stu-id="08560-1092">The value of `D` is checked to be valid.</span></span> <span data-ttu-id="08560-1093">Je-li hodnota `D` `null`, je vyvolána `System.NullReferenceException` a nejsou provedeny žádné další kroky.</span><span class="sxs-lookup"><span data-stu-id="08560-1093">If the value of `D` is `null`, a `System.NullReferenceException` is thrown and no further steps are executed.</span></span>
*  <span data-ttu-id="08560-1094">V opačném případě je `D` odkazem na instanci delegáta.</span><span class="sxs-lookup"><span data-stu-id="08560-1094">Otherwise, `D` is a reference to a delegate instance.</span></span> <span data-ttu-id="08560-1095">Volání členů funkce ([Kontrola dynamického přetěžování při kompilaci](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) se provádí na každé z těchto entit v seznamu volání delegáta.</span><span class="sxs-lookup"><span data-stu-id="08560-1095">Function member invocations ([Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) are performed on each of the callable entities in the invocation list of the delegate.</span></span> <span data-ttu-id="08560-1096">Pro volatelné entity skládající se z instance a metody instance je instance vyvolání instance obsažena v entitě, která se nedá volat.</span><span class="sxs-lookup"><span data-stu-id="08560-1096">For callable entities consisting of an instance and instance method, the instance for the invocation is the instance contained in the callable entity.</span></span>

### <a name="element-access"></a><span data-ttu-id="08560-1097">Přístup k prvkům</span><span class="sxs-lookup"><span data-stu-id="08560-1097">Element access</span></span>

<span data-ttu-id="08560-1098">*Element_access* se skládá z *primary_no_array_creation_expression*, po kterém následuje token "`[`" a za ním následuje *argument_list*následovaný tokenem "`]`".</span><span class="sxs-lookup"><span data-stu-id="08560-1098">An *element_access* consists of a *primary_no_array_creation_expression*, followed by a "`[`" token, followed by an *argument_list*, followed by a "`]`" token.</span></span> <span data-ttu-id="08560-1099">*Argument_list* se skládá z jednoho nebo více *argumentů*, které jsou odděleny čárkami.</span><span class="sxs-lookup"><span data-stu-id="08560-1099">The *argument_list* consists of one or more *argument*s, separated by commas.</span></span>

```antlr
element_access
    : primary_no_array_creation_expression '[' expression_list ']'
    ;
```

<span data-ttu-id="08560-1100">*Argument_list* *element_access* nesmí obsahovat argumenty `ref` nebo `out`.</span><span class="sxs-lookup"><span data-stu-id="08560-1100">The *argument_list* of an *element_access* is not allowed to contain `ref` or `out` arguments.</span></span>

<span data-ttu-id="08560-1101">*Element_access* je dynamicky svázán ([dynamická vazba](expressions.md#dynamic-binding)), pokud alespoň jedna z následujících možností obsahuje:</span><span class="sxs-lookup"><span data-stu-id="08560-1101">An *element_access* is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)) if at least one of the following holds:</span></span>

* <span data-ttu-id="08560-1102">*Primary_no_array_creation_expression* má `dynamic`typu pro dobu kompilace.</span><span class="sxs-lookup"><span data-stu-id="08560-1102">The *primary_no_array_creation_expression* has compile-time type `dynamic`.</span></span>
* <span data-ttu-id="08560-1103">Nejméně jeden výraz *argument_list* má `dynamic` typu pro dobu kompilace a *primary_no_array_creation_expression* nemá typ pole.</span><span class="sxs-lookup"><span data-stu-id="08560-1103">At least one expression of the *argument_list* has compile-time type `dynamic` and the *primary_no_array_creation_expression* does not have an array type.</span></span>

<span data-ttu-id="08560-1104">V tomto případě kompilátor klasifikuje *element_access* jako hodnotu typu `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="08560-1104">In this case the compiler classifies the *element_access* as a value of type `dynamic`.</span></span> <span data-ttu-id="08560-1105">Níže uvedená pravidla určují význam *element_access* se pak použijí za běhu, a to pomocí běhového typu místo na základě typu při kompilaci u výrazů *primary_no_array_creation_expression* a *argument_list* , které mají `dynamic`typu pro dobu kompilace.</span><span class="sxs-lookup"><span data-stu-id="08560-1105">The rules below to determine the meaning of the *element_access* are then applied at run-time, using the run-time type instead of the compile-time type of those of the *primary_no_array_creation_expression* and *argument_list* expressions which have the compile-time type `dynamic`.</span></span> <span data-ttu-id="08560-1106">Pokud *primary_no_array_creation_expression* nemá `dynamic`typu za kompilace, pak přístup k elementu se podřídí omezené doby kompilace, jak je popsáno v tématu [Kontrola dynamického přetěžování při kompilaci](expressions.md#compile-time-checking-of-dynamic-overload-resolution).</span><span class="sxs-lookup"><span data-stu-id="08560-1106">If the *primary_no_array_creation_expression* does not have compile-time type `dynamic`, then the element access undergoes a limited compile time check as described in [Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution).</span></span>

<span data-ttu-id="08560-1107">Pokud je *primary_no_array_creation_expression* *element_access* hodnotou *array_type*, *element_access* je přístup k poli ([přístup k poli](expressions.md#array-access)).</span><span class="sxs-lookup"><span data-stu-id="08560-1107">If the *primary_no_array_creation_expression* of an *element_access* is a value of an *array_type*, the *element_access* is an array access ([Array access](expressions.md#array-access)).</span></span> <span data-ttu-id="08560-1108">V opačném případě musí být *primary_no_array_creation_expression* proměnná nebo hodnota třídy, struktury nebo typu rozhraní, která má jednoho nebo více členů indexeru. v takovém případě je *element_access* přístup indexerem ([přístup indexerem](expressions.md#indexer-access)).</span><span class="sxs-lookup"><span data-stu-id="08560-1108">Otherwise, the *primary_no_array_creation_expression* must be a variable or value of a class, struct, or interface type that has one or more indexer members, in which case the *element_access* is an indexer access ([Indexer access](expressions.md#indexer-access)).</span></span>

#### <a name="array-access"></a><span data-ttu-id="08560-1109">Přístup k poli</span><span class="sxs-lookup"><span data-stu-id="08560-1109">Array access</span></span>

<span data-ttu-id="08560-1110">Pro přístup k poli musí být *primary_no_array_creation_expression* *element_access* hodnotou *array_type*.</span><span class="sxs-lookup"><span data-stu-id="08560-1110">For an array access, the *primary_no_array_creation_expression* of the *element_access* must be a value of an *array_type*.</span></span> <span data-ttu-id="08560-1111">Kromě toho *argument_list* k přístupu k poli nemá povoleno obsahovat pojmenované argumenty. Počet výrazů v *argument_list* musí být stejný jako pořadí *array_type*a každý výraz musí být typu `int`, `uint`, `long`, `ulong`nebo musí být implicitně převoditelný na jeden nebo více těchto typů.</span><span class="sxs-lookup"><span data-stu-id="08560-1111">Furthermore, the *argument_list* of an array access is not allowed to contain named arguments.The number of expressions in the *argument_list* must be the same as the rank of the *array_type*, and each expression must be of type `int`, `uint`, `long`, `ulong`, or must be implicitly convertible to one or more of these types.</span></span>

<span data-ttu-id="08560-1112">Výsledek vyhodnocení přístupu k poli je proměnná typu prvku pole, jmenovitým prvkem pole vybraným hodnotami (y) výrazů v *argument_list*.</span><span class="sxs-lookup"><span data-stu-id="08560-1112">The result of evaluating an array access is a variable of the element type of the array, namely the array element selected by the value(s) of the expression(s) in the *argument_list*.</span></span>

<span data-ttu-id="08560-1113">Běhové zpracování přístupu k poli formuláře `P[A]`, kde `P` je *primary_no_array_creation_expression* *array_type* a `A` je *argument_list*, sestává z následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="08560-1113">The run-time processing of an array access of the form `P[A]`, where `P` is a *primary_no_array_creation_expression* of an *array_type* and `A` is an *argument_list*, consists of the following steps:</span></span>

*  <span data-ttu-id="08560-1114">`P` je vyhodnocena.</span><span class="sxs-lookup"><span data-stu-id="08560-1114">`P` is evaluated.</span></span> <span data-ttu-id="08560-1115">Pokud toto vyhodnocení způsobí výjimku, nejsou provedeny žádné další kroky.</span><span class="sxs-lookup"><span data-stu-id="08560-1115">If this evaluation causes an exception, no further steps are executed.</span></span>
*  <span data-ttu-id="08560-1116">Výrazy indexu *argument_list* jsou vyhodnocovány v pořadí zleva doprava.</span><span class="sxs-lookup"><span data-stu-id="08560-1116">The index expressions of the *argument_list* are evaluated in order, from left to right.</span></span> <span data-ttu-id="08560-1117">Po vyhodnocení každého indexového výrazu se provede implicitní převod ([implicitní převody](conversions.md#implicit-conversions)) na jeden z následujících typů: `int`, `uint`, `long``ulong`.</span><span class="sxs-lookup"><span data-stu-id="08560-1117">Following evaluation of each index expression, an implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)) to one of the following types is performed: `int`, `uint`, `long`, `ulong`.</span></span> <span data-ttu-id="08560-1118">První typ v tomto seznamu, pro který existuje implicitní převod.</span><span class="sxs-lookup"><span data-stu-id="08560-1118">The first type in this list for which an implicit conversion exists is chosen.</span></span> <span data-ttu-id="08560-1119">Například pokud je Indexový výraz typu `short` pak je proveden implicitní převod na `int`, protože implicitní převody z `short` na `int` a z `short` na `long` jsou možné.</span><span class="sxs-lookup"><span data-stu-id="08560-1119">For instance, if the index expression is of type `short` then an implicit conversion to `int` is performed, since implicit conversions from `short` to `int` and from `short` to `long` are possible.</span></span> <span data-ttu-id="08560-1120">Pokud vyhodnocení výrazu indexu nebo následný implicitní převod způsobí výjimku, nejsou vyhodnoceny žádné další indexové výrazy a nejsou spuštěny žádné další kroky.</span><span class="sxs-lookup"><span data-stu-id="08560-1120">If evaluation of an index expression or the subsequent implicit conversion causes an exception, then no further index expressions are evaluated and no further steps are executed.</span></span>
*  <span data-ttu-id="08560-1121">Hodnota `P` je zaškrtnuta jako platná.</span><span class="sxs-lookup"><span data-stu-id="08560-1121">The value of `P` is checked to be valid.</span></span> <span data-ttu-id="08560-1122">Je-li hodnota `P` `null`, je vyvolána `System.NullReferenceException` a nejsou provedeny žádné další kroky.</span><span class="sxs-lookup"><span data-stu-id="08560-1122">If the value of `P` is `null`, a `System.NullReferenceException` is thrown and no further steps are executed.</span></span>
*  <span data-ttu-id="08560-1123">Hodnota každého výrazu v *argument_list* je zkontrolována proti skutečným hranicím jednotlivých dimenzí instance pole, na kterou odkazuje `P`.</span><span class="sxs-lookup"><span data-stu-id="08560-1123">The value of each expression in the *argument_list* is checked against the actual bounds of each dimension of the array instance referenced by `P`.</span></span> <span data-ttu-id="08560-1124">Pokud jedna nebo více hodnot je mimo rozsah, je vyvolána `System.IndexOutOfRangeException` a nejsou provedeny žádné další kroky.</span><span class="sxs-lookup"><span data-stu-id="08560-1124">If one or more values are out of range, a `System.IndexOutOfRangeException` is thrown and no further steps are executed.</span></span>
*  <span data-ttu-id="08560-1125">Je vypočítáno umístění elementu pole uvedeného v indexových výrazech a toto umístění se projeví v důsledku přístupu k poli.</span><span class="sxs-lookup"><span data-stu-id="08560-1125">The location of the array element given by the index expression(s) is computed, and this location becomes the result of the array access.</span></span>

#### <a name="indexer-access"></a><span data-ttu-id="08560-1126">Přístup indexeru</span><span class="sxs-lookup"><span data-stu-id="08560-1126">Indexer access</span></span>

<span data-ttu-id="08560-1127">Pro přístup indexeru musí být *primary_no_array_creation_expression* *element_access* proměnné nebo hodnoty třídy, struktury nebo typu rozhraní a tento typ musí implementovat jeden nebo více indexerů, které jsou použitelné v souvislosti s *argument_list* *element_access*.</span><span class="sxs-lookup"><span data-stu-id="08560-1127">For an indexer access, the *primary_no_array_creation_expression* of the *element_access* must be a variable or value of a class, struct, or interface type, and this type must implement one or more indexers that are applicable with respect to the *argument_list* of the *element_access*.</span></span>

<span data-ttu-id="08560-1128">Zpracování přístupu indexeru ve formuláři `P[A]`, kde `P` je *primary_no_array_creation_expression* třídy, struktury nebo typu rozhraní `T`a `A` je *argument_list*, skládá se z následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="08560-1128">The binding-time processing of an indexer access of the form `P[A]`, where `P` is a *primary_no_array_creation_expression* of a class, struct, or interface type `T`, and `A` is an *argument_list*, consists of the following steps:</span></span>

*  <span data-ttu-id="08560-1129">Je vytvořena sada indexerů, které poskytuje `T`.</span><span class="sxs-lookup"><span data-stu-id="08560-1129">The set of indexers provided by `T` is constructed.</span></span> <span data-ttu-id="08560-1130">Sada se skládá ze všech indexerů deklarovaných v `T` nebo ze základního typu `T`, které nejsou `override` deklarací a jsou přístupné v aktuálním kontextu ([přístup ke členům](basic-concepts.md#member-access)).</span><span class="sxs-lookup"><span data-stu-id="08560-1130">The set consists of all indexers declared in `T` or a base type of `T` that are not `override` declarations and are accessible in the current context ([Member access](basic-concepts.md#member-access)).</span></span>
*  <span data-ttu-id="08560-1131">Tato sada se zmenší na ty indexery, které jsou k dispozici, a ne skryté jinými indexery.</span><span class="sxs-lookup"><span data-stu-id="08560-1131">The set is reduced to those indexers that are applicable and not hidden by other indexers.</span></span> <span data-ttu-id="08560-1132">Následující pravidla jsou použita pro každý indexer `S.I` v sadě, kde `S` je typ, ve kterém je deklarován `I` indexeru:</span><span class="sxs-lookup"><span data-stu-id="08560-1132">The following rules are applied to each indexer `S.I` in the set, where `S` is the type in which the indexer `I` is declared:</span></span>
   * <span data-ttu-id="08560-1133">Pokud `I` nelze použít s ohledem na `A` ([platný člen funkce](expressions.md#applicable-function-member)), je `I` odebrána ze sady.</span><span class="sxs-lookup"><span data-stu-id="08560-1133">If `I` is not applicable with respect to `A` ([Applicable function member](expressions.md#applicable-function-member)), then `I` is removed from the set.</span></span>
   * <span data-ttu-id="08560-1134">Pokud je `I` použít s ohledem na `A` ([platný člen funkce](expressions.md#applicable-function-member)), pak jsou ze sady odebrány všechny indexery deklarované v základním typu `S`.</span><span class="sxs-lookup"><span data-stu-id="08560-1134">If `I` is applicable with respect to `A` ([Applicable function member](expressions.md#applicable-function-member)), then all indexers declared in a base type of `S` are removed from the set.</span></span>
   * <span data-ttu-id="08560-1135">Pokud je `I` k dispozici s ohledem na `A` ([příslušný člen funkce](expressions.md#applicable-function-member)) a `S` je typ třídy jiný než `object`, všechny indexery deklarované v rozhraní jsou ze sady odebrány.</span><span class="sxs-lookup"><span data-stu-id="08560-1135">If `I` is applicable with respect to `A` ([Applicable function member](expressions.md#applicable-function-member)) and `S` is a class type other than `object`, all indexers declared in an interface are removed from the set.</span></span>
*  <span data-ttu-id="08560-1136">Pokud je výsledná sada kandidátských indexerů prázdná, neexistují žádné odpovídající indexery a dojde k chybě při vazbě.</span><span class="sxs-lookup"><span data-stu-id="08560-1136">If the resulting set of candidate indexers is empty, then no applicable indexers exist, and a binding-time error occurs.</span></span>
*  <span data-ttu-id="08560-1137">Nejlepší indexer sady kandidátských indexerů se identifikuje pomocí pravidel řešení přetížení v rámci [rozlišení přetížení](expressions.md#overload-resolution).</span><span class="sxs-lookup"><span data-stu-id="08560-1137">The best indexer of the set of candidate indexers is identified using the overload resolution rules of [Overload resolution](expressions.md#overload-resolution).</span></span> <span data-ttu-id="08560-1138">Pokud nelze identifikovat jeden nejlepší indexer, přístup k indexeru je nejednoznačný a dojde k chybě při vazbě.</span><span class="sxs-lookup"><span data-stu-id="08560-1138">If a single best indexer cannot be identified, the indexer access is ambiguous, and a binding-time error occurs.</span></span>
*  <span data-ttu-id="08560-1139">Výrazy indexu *argument_list* jsou vyhodnocovány v pořadí zleva doprava.</span><span class="sxs-lookup"><span data-stu-id="08560-1139">The index expressions of the *argument_list* are evaluated in order, from left to right.</span></span> <span data-ttu-id="08560-1140">Výsledkem zpracování přístupu indexeru je výraz klasifikovaný jako přístup indexeru.</span><span class="sxs-lookup"><span data-stu-id="08560-1140">The result of processing the indexer access is an expression classified as an indexer access.</span></span> <span data-ttu-id="08560-1141">Výraz přístupu indexeru odkazuje na indexer určený v kroku výše a má přidružený výraz instance `P` a seznam přidružených argumentů `A`.</span><span class="sxs-lookup"><span data-stu-id="08560-1141">The indexer access expression references the indexer determined in the step above, and has an associated instance expression of `P` and an associated argument list of `A`.</span></span>

<span data-ttu-id="08560-1142">V závislosti na kontextu, ve kterém se používá, přístup indexeru způsobí vyvolání buď přístupového *objektu Get* , nebo *přístupového objektu* pro indexer.</span><span class="sxs-lookup"><span data-stu-id="08560-1142">Depending on the context in which it is used, an indexer access causes invocation of either the *get accessor* or the *set accessor* of the indexer.</span></span> <span data-ttu-id="08560-1143">Pokud je přístup indexerem cílem přiřazení, vyvolá se *přistupující objekt set* , který přiřadí novou hodnotu ([jednoduché přiřazení](expressions.md#simple-assignment)).</span><span class="sxs-lookup"><span data-stu-id="08560-1143">If the indexer access is the target of an assignment, the *set accessor* is invoked to assign a new value ([Simple assignment](expressions.md#simple-assignment)).</span></span> <span data-ttu-id="08560-1144">Ve všech ostatních případech je vyvolán *přistupující objekt get* , který získá aktuální hodnotu ([hodnoty výrazů](expressions.md#values-of-expressions)).</span><span class="sxs-lookup"><span data-stu-id="08560-1144">In all other cases, the *get accessor* is invoked to obtain the current value ([Values of expressions](expressions.md#values-of-expressions)).</span></span>

### <a name="this-access"></a><span data-ttu-id="08560-1145">Tento přístup</span><span class="sxs-lookup"><span data-stu-id="08560-1145">This access</span></span>

<span data-ttu-id="08560-1146">*This_Access* se skládá z vyhrazeného slova `this`.</span><span class="sxs-lookup"><span data-stu-id="08560-1146">A *this_access* consists of the reserved word `this`.</span></span>

```antlr
this_access
    : 'this'
    ;
```

<span data-ttu-id="08560-1147">*This_Access* je povolen pouze v *bloku* konstruktoru instance, metodě instance nebo přistupujícího objektu instance.</span><span class="sxs-lookup"><span data-stu-id="08560-1147">A *this_access* is permitted only in the *block* of an instance constructor, an instance method, or an instance accessor.</span></span> <span data-ttu-id="08560-1148">Má jeden z následujících významů:</span><span class="sxs-lookup"><span data-stu-id="08560-1148">It has one of the following meanings:</span></span>

*  <span data-ttu-id="08560-1149">Pokud je `this` použito v *primary_expression* v rámci konstruktoru instance třídy, je klasifikována jako hodnota.</span><span class="sxs-lookup"><span data-stu-id="08560-1149">When `this` is used in a *primary_expression* within an instance constructor of a class, it is classified as a value.</span></span> <span data-ttu-id="08560-1150">Typ hodnoty je typ instance ([typ instance](classes.md#the-instance-type)) třídy, ve které dojde k použití, a hodnota je odkaz na vytvořený objekt.</span><span class="sxs-lookup"><span data-stu-id="08560-1150">The type of the value is the instance type ([The instance type](classes.md#the-instance-type)) of the class within which the usage occurs, and the value is a reference to the object being constructed.</span></span>
*  <span data-ttu-id="08560-1151">Pokud se `this` používá v *primary_expression* v rámci metody instance nebo přístupového objektu instance třídy, je klasifikována jako hodnota.</span><span class="sxs-lookup"><span data-stu-id="08560-1151">When `this` is used in a *primary_expression* within an instance method or instance accessor of a class, it is classified as a value.</span></span> <span data-ttu-id="08560-1152">Typ hodnoty je typ instance ([typ instance](classes.md#the-instance-type)) třídy, ve které dojde k použití, a hodnota je odkaz na objekt, pro který byla metoda nebo přistupující objekt vyvolána.</span><span class="sxs-lookup"><span data-stu-id="08560-1152">The type of the value is the instance type ([The instance type](classes.md#the-instance-type)) of the class within which the usage occurs, and the value is a reference to the object for which the method or accessor was invoked.</span></span>
*  <span data-ttu-id="08560-1153">Pokud je `this` použito v *primary_expression* v rámci konstruktoru instance struktury, je klasifikován jako proměnná.</span><span class="sxs-lookup"><span data-stu-id="08560-1153">When `this` is used in a *primary_expression* within an instance constructor of a struct, it is classified as a variable.</span></span> <span data-ttu-id="08560-1154">Typ proměnné je typ instance ([typ instance](classes.md#the-instance-type)) struktury, ve které dojde k použití, a proměnná představuje vytvořenou strukturu.</span><span class="sxs-lookup"><span data-stu-id="08560-1154">The type of the variable is the instance type ([The instance type](classes.md#the-instance-type)) of the struct within which the usage occurs, and the variable represents the struct being constructed.</span></span> <span data-ttu-id="08560-1155">`this` proměnná konstruktoru instance struktury se chová stejně jako parametr `out` typu struktury – konkrétně to znamená, že proměnná musí být jednoznačně přiřazena v každé cestě spuštění konstruktoru instance.</span><span class="sxs-lookup"><span data-stu-id="08560-1155">The `this` variable of an instance constructor of a struct behaves exactly the same as an `out` parameter of the struct type—in particular, this means that the variable must be definitely assigned in every execution path of the instance constructor.</span></span>
*  <span data-ttu-id="08560-1156">Když se `this` používá v *primary_expression* v rámci metody instance nebo přístupového objektu instance struktury, je klasifikován jako proměnná.</span><span class="sxs-lookup"><span data-stu-id="08560-1156">When `this` is used in a *primary_expression* within an instance method or instance accessor of a struct, it is classified as a variable.</span></span> <span data-ttu-id="08560-1157">Typ proměnné je typ instance ([typ instance](classes.md#the-instance-type)) struktury, ve které dojde k použití.</span><span class="sxs-lookup"><span data-stu-id="08560-1157">The type of the variable is the instance type ([The instance type](classes.md#the-instance-type)) of the struct within which the usage occurs.</span></span>
   * <span data-ttu-id="08560-1158">Pokud metoda nebo přistupující objekt není iterátor ([iterátory](classes.md#iterators)), proměnná `this` představuje strukturu, pro kterou byla metoda nebo přistupující objekt vyvolána, a chová se stejně jako parametr `ref` typu struktury.</span><span class="sxs-lookup"><span data-stu-id="08560-1158">If the method or accessor is not an iterator ([Iterators](classes.md#iterators)), the `this` variable represents the struct for which the method or accessor was invoked, and behaves exactly the same as a `ref` parameter of the struct type.</span></span>
   * <span data-ttu-id="08560-1159">Pokud je metoda nebo přistupující objekt iterátor, `this` proměnná představuje kopii struktury, pro kterou byla metoda nebo přistupující objekt vyvolána, a chová se stejně jako parametr hodnoty typu struktury.</span><span class="sxs-lookup"><span data-stu-id="08560-1159">If the method or accessor is an iterator, the `this` variable represents a copy of the struct for which the method or accessor was invoked, and behaves exactly the same as a value parameter of the struct type.</span></span>

<span data-ttu-id="08560-1160">Použití `this` v *primary_expression* v jiném kontextu, než jaké jsou uvedeny výše, je chyba při kompilaci.</span><span class="sxs-lookup"><span data-stu-id="08560-1160">Use of `this` in a *primary_expression* in a context other than the ones listed above is a compile-time error.</span></span> <span data-ttu-id="08560-1161">Konkrétně není možné odkazovat na `this` ve statické metodě, přístupovém objektu statické vlastnosti nebo v *variable_initializer* deklarace pole.</span><span class="sxs-lookup"><span data-stu-id="08560-1161">In particular, it is not possible to refer to `this` in a static method, a static property accessor, or in a *variable_initializer* of a field declaration.</span></span>

### <a name="base-access"></a><span data-ttu-id="08560-1162">Základní přístup</span><span class="sxs-lookup"><span data-stu-id="08560-1162">Base access</span></span>

<span data-ttu-id="08560-1163">*Base_access* se skládá z vyhrazeného slova `base` následovaným tokenem "`.`" a identifikátorem nebo *argument_list* uzavřeným v hranatých závorkách:</span><span class="sxs-lookup"><span data-stu-id="08560-1163">A *base_access* consists of the reserved word `base` followed by either a "`.`" token and an identifier or an *argument_list* enclosed in square brackets:</span></span>

```antlr
base_access
    : 'base' '.' identifier
    | 'base' '[' expression_list ']'
    ;
```

<span data-ttu-id="08560-1164">*Base_access* se používá pro přístup ke členům základní třídy, které jsou skryté podobným názvem členy v aktuální třídě nebo struktuře.</span><span class="sxs-lookup"><span data-stu-id="08560-1164">A *base_access* is used to access base class members that are hidden by similarly named members in the current class or struct.</span></span> <span data-ttu-id="08560-1165">*Base_access* je povolen pouze v *bloku* konstruktoru instance, metodě instance nebo přistupujícího objektu instance.</span><span class="sxs-lookup"><span data-stu-id="08560-1165">A *base_access* is permitted only in the *block* of an instance constructor, an instance method, or an instance accessor.</span></span> <span data-ttu-id="08560-1166">Pokud `base.I` dojde ve třídě nebo struktuře, `I` musí poznamenat člena základní třídy této třídy nebo struktury.</span><span class="sxs-lookup"><span data-stu-id="08560-1166">When `base.I` occurs in a class or struct, `I` must denote a member of the base class of that class or struct.</span></span> <span data-ttu-id="08560-1167">Podobně platí, že pokud `base[E]` dojde ve třídě, musí existovat příslušný indexer v základní třídě.</span><span class="sxs-lookup"><span data-stu-id="08560-1167">Likewise, when `base[E]` occurs in a class, an applicable indexer must exist in the base class.</span></span>

<span data-ttu-id="08560-1168">V době vytváření vazby *base_access* výrazy formuláře `base.I` a `base[E]` jsou vyhodnocovány přesně tak, jak byly napsány `((B)this).I` a `((B)this)[E]`, kde `B` je základní třídou třídy nebo struktury, ve které konstrukce nastávají.</span><span class="sxs-lookup"><span data-stu-id="08560-1168">At binding-time, *base_access* expressions of the form `base.I` and `base[E]` are evaluated exactly as if they were written `((B)this).I` and `((B)this)[E]`, where `B` is the base class of the class or struct in which the construct occurs.</span></span> <span data-ttu-id="08560-1169">Proto `base.I` a `base[E]` odpovídat `this.I` a `this[E]`, s výjimkou `this` je zobrazen jako instance základní třídy.</span><span class="sxs-lookup"><span data-stu-id="08560-1169">Thus, `base.I` and `base[E]` correspond to `this.I` and `this[E]`, except `this` is viewed as an instance of the base class.</span></span>

<span data-ttu-id="08560-1170">Když *base_access* odkazuje na člen virtuální funkce (metoda, vlastnost nebo indexer), určení toho, který člen funkce se má vyvolat za běhu ([Kontrola dynamického přetěžování při kompilaci](expressions.md#compile-time-checking-of-dynamic-overload-resolution)), se změní.</span><span class="sxs-lookup"><span data-stu-id="08560-1170">When a *base_access* references a virtual function member (a method, property, or indexer), the determination of which function member to invoke at run-time ([Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) is changed.</span></span> <span data-ttu-id="08560-1171">Vyvolaný člen funkce je určen tak, že najde největší odvozenou implementaci ([virtuální metody](classes.md#virtual-methods)) člena funkce s ohledem na `B` (místo z hlediska běhového typu `this`, jako by byl obvykle v nezákladním přístupu).</span><span class="sxs-lookup"><span data-stu-id="08560-1171">The function member that is invoked is determined by finding the most derived implementation ([Virtual methods](classes.md#virtual-methods)) of the function member with respect to `B` (instead of with respect to the run-time type of `this`, as would be usual in a non-base access).</span></span> <span data-ttu-id="08560-1172">Proto lze v rámci `override` `virtual` člena funkce použít *base_access* k vyvolání zděděné implementace členu funkce.</span><span class="sxs-lookup"><span data-stu-id="08560-1172">Thus, within an `override` of a `virtual` function member, a *base_access* can be used to invoke the inherited implementation of the function member.</span></span> <span data-ttu-id="08560-1173">Pokud je člen funkce, na který *base_access* odkazuje, abstraktní, dojde k chybě při vazbě.</span><span class="sxs-lookup"><span data-stu-id="08560-1173">If the function member referenced by a *base_access* is abstract, a binding-time error occurs.</span></span>

### <a name="postfix-increment-and-decrement-operators"></a><span data-ttu-id="08560-1174">Operátory přírůstku a snížení přípony</span><span class="sxs-lookup"><span data-stu-id="08560-1174">Postfix increment and decrement operators</span></span>

```antlr
post_increment_expression
    : primary_expression '++'
    ;

post_decrement_expression
    : primary_expression '--'
    ;
```

<span data-ttu-id="08560-1175">Operandem operace zvýšení nebo snížení přípony musí být výraz klasifikovaný jako proměnná, přístup k vlastnosti nebo přístup indexeru.</span><span class="sxs-lookup"><span data-stu-id="08560-1175">The operand of a postfix increment or decrement operation must be an expression classified as a variable, a property access, or an indexer access.</span></span> <span data-ttu-id="08560-1176">Výsledkem operace je hodnota stejného typu jako operand.</span><span class="sxs-lookup"><span data-stu-id="08560-1176">The result of the operation is a value of the same type as the operand.</span></span>

<span data-ttu-id="08560-1177">Pokud má *primary_expression* typ doby kompilace `dynamic` pak je operátor dynamicky svázán ([dynamická vazba](expressions.md#dynamic-binding)), *post_increment_expression* nebo *post_decrement_expression* má typ doby kompilace `dynamic` a následující pravidla jsou použita v době běhu pomocí běhového typu *primary_expression*.</span><span class="sxs-lookup"><span data-stu-id="08560-1177">If the *primary_expression* has the compile-time type `dynamic` then the operator is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)), the *post_increment_expression* or *post_decrement_expression* has the compile-time type `dynamic` and the following rules are applied at run-time using the run-time type of the *primary_expression*.</span></span>

<span data-ttu-id="08560-1178">Pokud je operandem operace zvýšení nebo snížení přípony vlastnost přístup k indexeru, musí mít vlastnost nebo indexer jak `get`, tak přistupující objekt `set`.</span><span class="sxs-lookup"><span data-stu-id="08560-1178">If the operand of a postfix increment or decrement operation is a property or indexer access, the property or indexer must have both a `get` and a `set` accessor.</span></span> <span data-ttu-id="08560-1179">Pokud se nejedná o tento případ, dojde k chybě při vazbě.</span><span class="sxs-lookup"><span data-stu-id="08560-1179">If this is not the case, a binding-time error occurs.</span></span>

<span data-ttu-id="08560-1180">Pro výběr implementace konkrétního operátoru je použito rozlišení přetížení unárního operátoru ([rozlišení přetížení unárního operátoru](expressions.md#unary-operator-overload-resolution)).</span><span class="sxs-lookup"><span data-stu-id="08560-1180">Unary operator overload resolution ([Unary operator overload resolution](expressions.md#unary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="08560-1181">Předdefinované `++` a `--` operátory existují pro následující typy: `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, `decimal`a jakýkoli typ výčtu.</span><span class="sxs-lookup"><span data-stu-id="08560-1181">Predefined `++` and `--` operators exist for the following types: `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, `decimal`, and any enum type.</span></span> <span data-ttu-id="08560-1182">Předdefinované operátory `++` vrátí hodnotu vytvořenou přidáním 1 k operandu a předdefinované `--` operátory vrátí hodnotu vytvořenou odečtením hodnoty 1 od operandu.</span><span class="sxs-lookup"><span data-stu-id="08560-1182">The predefined `++` operators return the value produced by adding 1 to the operand, and the predefined `--` operators return the value produced by subtracting 1 from the operand.</span></span> <span data-ttu-id="08560-1183">V kontextu `checked`, pokud je výsledek tohoto sčítání nebo odčítání mimo rozsah výsledného typu a typ výsledku je celočíselný typ nebo typ výčtu, je vyvolána `System.OverflowException`.</span><span class="sxs-lookup"><span data-stu-id="08560-1183">In a `checked` context, if the result of this addition or subtraction is outside the range of the result type and the result type is an integral type or enum type, a `System.OverflowException` is thrown.</span></span>

<span data-ttu-id="08560-1184">Běhové zpracování přírůstku nebo operace snížení přípon na formuláři `x++` nebo `x--` sestává z následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="08560-1184">The run-time processing of a postfix increment or decrement operation of the form `x++` or `x--` consists of the following steps:</span></span>

*   <span data-ttu-id="08560-1185">Pokud je `x` klasifikován jako proměnná:</span><span class="sxs-lookup"><span data-stu-id="08560-1185">If `x` is classified as a variable:</span></span>
    * <span data-ttu-id="08560-1186">`x` se vyhodnotí, aby se vytvořila proměnná.</span><span class="sxs-lookup"><span data-stu-id="08560-1186">`x` is evaluated to produce the variable.</span></span>
    * <span data-ttu-id="08560-1187">Hodnota `x` je uložena.</span><span class="sxs-lookup"><span data-stu-id="08560-1187">The value of `x` is saved.</span></span>
    * <span data-ttu-id="08560-1188">Vybraný operátor je vyvolán s uloženou hodnotou `x` jako argument.</span><span class="sxs-lookup"><span data-stu-id="08560-1188">The selected operator is invoked with the saved value of `x` as its argument.</span></span>
    * <span data-ttu-id="08560-1189">Hodnota vrácená operátorem je uložena v umístění uvedeném vyhodnocením `x`.</span><span class="sxs-lookup"><span data-stu-id="08560-1189">The value returned by the operator is stored in the location given by the evaluation of `x`.</span></span>
    * <span data-ttu-id="08560-1190">Uložená hodnota `x` se stal výsledkem operace.</span><span class="sxs-lookup"><span data-stu-id="08560-1190">The saved value of `x` becomes the result of the operation.</span></span>
*   <span data-ttu-id="08560-1191">Pokud je `x` klasifikován jako vlastnost nebo přístup indexeru:</span><span class="sxs-lookup"><span data-stu-id="08560-1191">If `x` is classified as a property or indexer access:</span></span>
    * <span data-ttu-id="08560-1192">Výraz instance (Pokud `x` není `static`) a seznam argumentů (Pokud je `x` přístup indexeru) přidružený k `x` jsou vyhodnocovány a výsledky se používají v následných `get` a `set` vyvolání přístupového objektu.</span><span class="sxs-lookup"><span data-stu-id="08560-1192">The instance expression (if `x` is not `static`) and the argument list (if `x` is an indexer access) associated with `x` are evaluated, and the results are used in the subsequent `get` and `set` accessor invocations.</span></span>
    * <span data-ttu-id="08560-1193">Je vyvolán přistupující objekt `get` `x` a vrácená hodnota je uložena.</span><span class="sxs-lookup"><span data-stu-id="08560-1193">The `get` accessor of `x` is invoked and the returned value is saved.</span></span>
    * <span data-ttu-id="08560-1194">Vybraný operátor je vyvolán s uloženou hodnotou `x` jako argument.</span><span class="sxs-lookup"><span data-stu-id="08560-1194">The selected operator is invoked with the saved value of `x` as its argument.</span></span>
    * <span data-ttu-id="08560-1195">Přistupující objekt `set` `x` je vyvolán s hodnotou vrácenou operátorem jako jeho argument `value`.</span><span class="sxs-lookup"><span data-stu-id="08560-1195">The `set` accessor of `x` is invoked with the value returned by the operator as its `value` argument.</span></span>
    * <span data-ttu-id="08560-1196">Uložená hodnota `x` se stal výsledkem operace.</span><span class="sxs-lookup"><span data-stu-id="08560-1196">The saved value of `x` becomes the result of the operation.</span></span>

<span data-ttu-id="08560-1197">Operátory `++` a `--` také podporují zápis předpony ([operátory přírůstku a snížení předpony](expressions.md#prefix-increment-and-decrement-operators)).</span><span class="sxs-lookup"><span data-stu-id="08560-1197">The `++` and `--` operators also support prefix notation ([Prefix increment and decrement operators](expressions.md#prefix-increment-and-decrement-operators)).</span></span> <span data-ttu-id="08560-1198">Výsledkem `x++` nebo `x--` je obvykle hodnota `x` před operací, zatímco výsledek `++x` nebo `--x` je hodnota `x` po operaci.</span><span class="sxs-lookup"><span data-stu-id="08560-1198">Typically, the result of `x++` or `x--` is the value of `x` before the operation, whereas the result of `++x` or `--x` is the value of `x` after the operation.</span></span> <span data-ttu-id="08560-1199">V obou případech má `x` stejnou hodnotu po operaci.</span><span class="sxs-lookup"><span data-stu-id="08560-1199">In either case, `x` itself has the same value after the operation.</span></span>

<span data-ttu-id="08560-1200">Implementaci `operator ++` nebo `operator --` lze vyvolat pomocí přípony nebo zápisu předpon.</span><span class="sxs-lookup"><span data-stu-id="08560-1200">An `operator ++` or `operator --` implementation can be invoked using either postfix or prefix notation.</span></span> <span data-ttu-id="08560-1201">Pro tyto dva zápisy není možné mít implementace samostatného operátoru.</span><span class="sxs-lookup"><span data-stu-id="08560-1201">It is not possible to have separate operator implementations for the two notations.</span></span>

### <a name="the-new-operator"></a><span data-ttu-id="08560-1202">Operátor New</span><span class="sxs-lookup"><span data-stu-id="08560-1202">The new operator</span></span>

<span data-ttu-id="08560-1203">Operátor `new` slouží k vytváření nových instancí typů.</span><span class="sxs-lookup"><span data-stu-id="08560-1203">The `new` operator is used to create new instances of types.</span></span>

<span data-ttu-id="08560-1204">Existují tři formy `new` výrazů:</span><span class="sxs-lookup"><span data-stu-id="08560-1204">There are three forms of `new` expressions:</span></span>

*  <span data-ttu-id="08560-1205">Výrazy vytvoření objektu slouží k vytváření nových instancí typů tříd a hodnot.</span><span class="sxs-lookup"><span data-stu-id="08560-1205">Object creation expressions are used to create new instances of class types and value types.</span></span>
*  <span data-ttu-id="08560-1206">Výrazy vytváření polí slouží k vytváření nových instancí typů polí.</span><span class="sxs-lookup"><span data-stu-id="08560-1206">Array creation expressions are used to create new instances of array types.</span></span>
*  <span data-ttu-id="08560-1207">Výrazy vytvoření delegáta slouží k vytváření nových instancí typů delegátů.</span><span class="sxs-lookup"><span data-stu-id="08560-1207">Delegate creation expressions are used to create new instances of delegate types.</span></span>

<span data-ttu-id="08560-1208">Operátor `new` implikuje vytvoření instance typu, ale nemusí nutně znamenat dynamické přidělování paměti.</span><span class="sxs-lookup"><span data-stu-id="08560-1208">The `new` operator implies creation of an instance of a type, but does not necessarily imply dynamic allocation of memory.</span></span> <span data-ttu-id="08560-1209">Konkrétně instance typů hodnot nevyžadují žádnou další paměť nad rámec, ve kterém se nacházejí, a žádná dynamická alokace nevzniká, když `new` slouží k vytváření instancí typů hodnot.</span><span class="sxs-lookup"><span data-stu-id="08560-1209">In particular, instances of value types require no additional memory beyond the variables in which they reside, and no dynamic allocations occur when `new` is used to create instances of value types.</span></span>

#### <a name="object-creation-expressions"></a><span data-ttu-id="08560-1210">Výrazy pro vytváření objektů</span><span class="sxs-lookup"><span data-stu-id="08560-1210">Object creation expressions</span></span>

<span data-ttu-id="08560-1211">*Object_creation_expression* slouží k vytvoření nové instance *class_type* nebo *value_type*.</span><span class="sxs-lookup"><span data-stu-id="08560-1211">An *object_creation_expression* is used to create a new instance of a *class_type* or a *value_type*.</span></span>

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

<span data-ttu-id="08560-1212">*Typ* *object_creation_expression* musí být *class_type*, *value_type* nebo *type_parameter*.</span><span class="sxs-lookup"><span data-stu-id="08560-1212">The *type* of an *object_creation_expression* must be a *class_type*, a *value_type* or a *type_parameter*.</span></span> <span data-ttu-id="08560-1213">*Typ* nemůže být *class_type*`abstract`.</span><span class="sxs-lookup"><span data-stu-id="08560-1213">The *type* cannot be an `abstract` *class_type*.</span></span>

<span data-ttu-id="08560-1214">Nepovinné *argument_list* ([seznamy argumentů](expressions.md#argument-lists)) jsou povoleny pouze v případě, že je *typ* *class_type* nebo *struct_type*.</span><span class="sxs-lookup"><span data-stu-id="08560-1214">The optional *argument_list* ([Argument lists](expressions.md#argument-lists)) is permitted only if the *type* is a *class_type* or a *struct_type*.</span></span>

<span data-ttu-id="08560-1215">Výraz pro vytvoření objektu může vynechat seznam argumentů konstruktoru a uzavřít závorky, pokud obsahuje inicializátor objektu nebo inicializátor kolekce.</span><span class="sxs-lookup"><span data-stu-id="08560-1215">An object creation expression can omit the constructor argument list and enclosing parentheses provided it includes an object initializer or collection initializer.</span></span> <span data-ttu-id="08560-1216">Vynechání seznamu argumentů konstruktoru a uzavíracích závorek je ekvivalentem zadání prázdného seznamu argumentů.</span><span class="sxs-lookup"><span data-stu-id="08560-1216">Omitting the constructor argument list and enclosing parentheses is equivalent to specifying an empty argument list.</span></span>

<span data-ttu-id="08560-1217">Zpracování výrazu vytvoření objektu, který obsahuje inicializátor objektu nebo inicializátor kolekce, se skládá z prvního zpracování konstruktoru instance a následného zpracování inicializací členů nebo elementů zadaných inicializátorem objektu ([Inicializátory objektů](expressions.md#object-initializers)) nebo inicializátorem kolekce ([inicializátory kolekce](expressions.md#collection-initializers)).</span><span class="sxs-lookup"><span data-stu-id="08560-1217">Processing of an object creation expression that includes an object initializer or collection initializer consists of first processing the instance constructor and then processing the member or element initializations specified by the object initializer ([Object initializers](expressions.md#object-initializers)) or collection initializer ([Collection initializers](expressions.md#collection-initializers)).</span></span>

<span data-ttu-id="08560-1218">Pokud některý z argumentů v nepovinné *argument_list* má typ pro čas kompilace `dynamic` pak je *object_creation_expression* dynamicky svázán ([dynamická vazba](expressions.md#dynamic-binding)) a následující pravidla jsou použita v době běhu pomocí typu za běhu pro tyto argumenty *argument_list* , které mají typ doby kompilace `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="08560-1218">If any of the arguments in the optional *argument_list* has the compile-time type `dynamic` then the *object_creation_expression* is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)) and the following rules are applied at run-time using the run-time type of those arguments of the *argument_list* that have the compile time type `dynamic`.</span></span> <span data-ttu-id="08560-1219">Vytvoření objektu však podchází omezené době kompilace, jak je popsáno v tématu [Kontrola dynamického přetěžování při kompilaci](expressions.md#compile-time-checking-of-dynamic-overload-resolution).</span><span class="sxs-lookup"><span data-stu-id="08560-1219">However, the object creation undergoes a limited compile time check as described in [Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution).</span></span>

<span data-ttu-id="08560-1220">Zpracování *object_creation_expression* `new T(A)`formuláře v čase vazby, kde `T` je *class_type* nebo *value_type* a `A` je volitelná *argument_list*skládá se z následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="08560-1220">The binding-time processing of an *object_creation_expression* of the form `new T(A)`, where `T` is a *class_type* or a *value_type* and `A` is an optional *argument_list*, consists of the following steps:</span></span>

*   <span data-ttu-id="08560-1221">Pokud je `T` *value_type* a `A` není k dispozici:</span><span class="sxs-lookup"><span data-stu-id="08560-1221">If `T` is a *value_type* and `A` is not present:</span></span>
    * <span data-ttu-id="08560-1222">*Object_creation_expression* je výchozí vyvolání konstruktoru.</span><span class="sxs-lookup"><span data-stu-id="08560-1222">The *object_creation_expression* is a default constructor invocation.</span></span> <span data-ttu-id="08560-1223">Výsledek *object_creation_expression* je hodnota typu `T`, konkrétně výchozí hodnota pro `T`, jak je definována v [typu System. ValueType](types.md#the-systemvaluetype-type).</span><span class="sxs-lookup"><span data-stu-id="08560-1223">The result of the *object_creation_expression* is a value of type `T`, namely the default value for `T` as defined in [The System.ValueType type](types.md#the-systemvaluetype-type).</span></span>
*   <span data-ttu-id="08560-1224">V opačném případě, pokud je `T` *type_parameter* a `A` není k dispozici:</span><span class="sxs-lookup"><span data-stu-id="08560-1224">Otherwise, if `T` is a *type_parameter* and `A` is not present:</span></span>
    * <span data-ttu-id="08560-1225">Pokud bylo pro `T`zadáno omezení typu hodnoty nebo omezení konstruktoru ([omezení parametrů typu](classes.md#type-parameter-constraints)), dojde k chybě při vazbě.</span><span class="sxs-lookup"><span data-stu-id="08560-1225">If no value type constraint or constructor constraint ([Type parameter constraints](classes.md#type-parameter-constraints)) has been specified for `T`, a binding-time error occurs.</span></span>
    * <span data-ttu-id="08560-1226">Výsledek *object_creation_expression* je hodnota běhového typu, ke kterému byl parametr typu vázán, konkrétně výsledek vyvolání výchozího konstruktoru tohoto typu.</span><span class="sxs-lookup"><span data-stu-id="08560-1226">The result of the *object_creation_expression* is a value of the run-time type that the type parameter has been bound to, namely the result of invoking the default constructor of that type.</span></span> <span data-ttu-id="08560-1227">Typ běhu může být odkazový typ nebo typ hodnoty.</span><span class="sxs-lookup"><span data-stu-id="08560-1227">The run-time type may be a reference type or a value type.</span></span>
*   <span data-ttu-id="08560-1228">Jinak, pokud je `T` *class_type* nebo *struct_type*:</span><span class="sxs-lookup"><span data-stu-id="08560-1228">Otherwise, if `T` is a *class_type* or a *struct_type*:</span></span>
    * <span data-ttu-id="08560-1229">Pokud je `T` `abstract` *class_type*, dojde k chybě při kompilaci.</span><span class="sxs-lookup"><span data-stu-id="08560-1229">If `T` is an `abstract` *class_type*, a compile-time error occurs.</span></span>
    * <span data-ttu-id="08560-1230">Konstruktor instance, který se má vyvolat, je určen pomocí pravidel řešení přetížení v [rozlišení přetěžování](expressions.md#overload-resolution).</span><span class="sxs-lookup"><span data-stu-id="08560-1230">The instance constructor to invoke is determined using the overload resolution rules of [Overload resolution](expressions.md#overload-resolution).</span></span> <span data-ttu-id="08560-1231">Sada konstruktorů instancí kandidátů se skládá ze všech přístupných konstruktorů instancí deklarovaných v `T`, které platí pro `A` ([platný člen funkce](expressions.md#applicable-function-member)).</span><span class="sxs-lookup"><span data-stu-id="08560-1231">The set of candidate instance constructors consists of all accessible instance constructors declared in `T` which are applicable with respect to `A` ([Applicable function member](expressions.md#applicable-function-member)).</span></span> <span data-ttu-id="08560-1232">Pokud je sada konstruktorů instancí kandidátů prázdná nebo pokud jeden nejlepší konstruktor instance nelze identifikovat, dojde k chybě při vazbě.</span><span class="sxs-lookup"><span data-stu-id="08560-1232">If the set of candidate instance constructors is empty, or if a single best instance constructor cannot be identified, a binding-time error occurs.</span></span>
    * <span data-ttu-id="08560-1233">Výsledek *object_creation_expression* je hodnota typu `T`, konkrétně hodnota vytvořená vyvoláním konstruktoru instance určeného v kroku výše.</span><span class="sxs-lookup"><span data-stu-id="08560-1233">The result of the *object_creation_expression* is a value of type `T`, namely the value produced by invoking the instance constructor determined in the step above.</span></span>
*  <span data-ttu-id="08560-1234">V opačném případě je *object_creation_expression* neplatný a dojde k chybě při vazbě.</span><span class="sxs-lookup"><span data-stu-id="08560-1234">Otherwise, the *object_creation_expression* is invalid, and a binding-time error occurs.</span></span>

<span data-ttu-id="08560-1235">I v případě, že je *object_creation_expression* dynamicky svázán, je typ kompilace stále `T`.</span><span class="sxs-lookup"><span data-stu-id="08560-1235">Even if the *object_creation_expression* is dynamically bound, the compile-time type is still `T`.</span></span>

<span data-ttu-id="08560-1236">Zpracování *object_creation_expression* `new T(A)`formuláře v době běhu, kde `T` *class_type* nebo *struct_type* a `A` je volitelný *argument_list*, skládá se z následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="08560-1236">The run-time processing of an *object_creation_expression* of the form `new T(A)`, where `T` is *class_type* or a *struct_type* and `A` is an optional *argument_list*, consists of the following steps:</span></span>

*   <span data-ttu-id="08560-1237">Pokud je `T` *class_type*:</span><span class="sxs-lookup"><span data-stu-id="08560-1237">If `T` is a *class_type*:</span></span>
    * <span data-ttu-id="08560-1238">Je přidělena nová instance třídy `T`.</span><span class="sxs-lookup"><span data-stu-id="08560-1238">A new instance of class `T` is allocated.</span></span> <span data-ttu-id="08560-1239">Pokud není k dispozici dostatek paměti pro přidělení nové instance, je vyvolána `System.OutOfMemoryException` a nejsou spuštěny žádné další kroky.</span><span class="sxs-lookup"><span data-stu-id="08560-1239">If there is not enough memory available to allocate the new instance, a `System.OutOfMemoryException` is thrown and no further steps are executed.</span></span>
    * <span data-ttu-id="08560-1240">Všechna pole nové instance jsou inicializována na výchozí hodnoty ([výchozí hodnoty](variables.md#default-values)).</span><span class="sxs-lookup"><span data-stu-id="08560-1240">All fields of the new instance are initialized to their default values ([Default values](variables.md#default-values)).</span></span>
    * <span data-ttu-id="08560-1241">Konstruktor instance je vyvolán v souladu s pravidly vyvolání člena funkce ([Kontrola při kompilaci dynamického přetěžování](expressions.md#compile-time-checking-of-dynamic-overload-resolution)).</span><span class="sxs-lookup"><span data-stu-id="08560-1241">The instance constructor is invoked according to the rules of function member invocation ([Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution)).</span></span> <span data-ttu-id="08560-1242">Odkaz na nově přidělenou instanci je automaticky předán konstruktoru instance a k instanci lze v rámci tohoto konstruktoru přistoupit jako `this`.</span><span class="sxs-lookup"><span data-stu-id="08560-1242">A reference to the newly allocated instance is automatically passed to the instance constructor and the instance can be accessed from within that constructor as `this`.</span></span>
*   <span data-ttu-id="08560-1243">Pokud je `T` *struct_type*:</span><span class="sxs-lookup"><span data-stu-id="08560-1243">If `T` is a *struct_type*:</span></span>
    * <span data-ttu-id="08560-1244">Instance typu `T` je vytvořena přidělením dočasné místní proměnné.</span><span class="sxs-lookup"><span data-stu-id="08560-1244">An instance of type `T` is created by allocating a temporary local variable.</span></span> <span data-ttu-id="08560-1245">Vzhledem k tomu, že konstruktor instance *struct_type* je vyžadován k omezení přiřazení hodnoty každému poli instance, kterou vytváříte, není nutné inicializovat dočasnou proměnnou.</span><span class="sxs-lookup"><span data-stu-id="08560-1245">Since an instance constructor of a *struct_type* is required to definitely assign a value to each field of the instance being created, no initialization of the temporary variable is necessary.</span></span>
    * <span data-ttu-id="08560-1246">Konstruktor instance je vyvolán v souladu s pravidly vyvolání člena funkce ([Kontrola při kompilaci dynamického přetěžování](expressions.md#compile-time-checking-of-dynamic-overload-resolution)).</span><span class="sxs-lookup"><span data-stu-id="08560-1246">The instance constructor is invoked according to the rules of function member invocation ([Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution)).</span></span> <span data-ttu-id="08560-1247">Odkaz na nově přidělenou instanci je automaticky předán konstruktoru instance a k instanci lze v rámci tohoto konstruktoru přistoupit jako `this`.</span><span class="sxs-lookup"><span data-stu-id="08560-1247">A reference to the newly allocated instance is automatically passed to the instance constructor and the instance can be accessed from within that constructor as `this`.</span></span>

#### <a name="object-initializers"></a><span data-ttu-id="08560-1248">Inicializátory objektů</span><span class="sxs-lookup"><span data-stu-id="08560-1248">Object initializers</span></span>

<span data-ttu-id="08560-1249">***Inicializátor objektu*** určuje hodnoty pro nula nebo více polí, vlastností nebo indexovaných prvků objektu.</span><span class="sxs-lookup"><span data-stu-id="08560-1249">An ***object initializer*** specifies values for zero or more fields, properties or indexed elements of an object.</span></span>

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

<span data-ttu-id="08560-1250">Inicializátor objektu se skládá z sekvence inicializátorů členů, které jsou uzavřeny `{` a `}` tokeny a odděleny čárkami.</span><span class="sxs-lookup"><span data-stu-id="08560-1250">An object initializer consists of a sequence of member initializers, enclosed by `{` and `}` tokens and separated by commas.</span></span> <span data-ttu-id="08560-1251">Každý *member_initializer* určuje cíl pro inicializaci.</span><span class="sxs-lookup"><span data-stu-id="08560-1251">Each *member_initializer* designates a target for the initialization.</span></span> <span data-ttu-id="08560-1252">*Identifikátor* musí pojmenovat přístupné pole nebo vlastnost objektu, který je inicializován, zatímco *argument_list* uzavřený v hranatých závorkách musí zadat argumenty pro přístupný indexer u objektu, který se inicializuje.</span><span class="sxs-lookup"><span data-stu-id="08560-1252">An *identifier* must name an accessible field or property of the object being initialized, whereas an *argument_list* enclosed in square brackets must specify arguments for an accessible indexer on the object being initialized.</span></span> <span data-ttu-id="08560-1253">Pokud má inicializátor objektu zahrnout více než jeden inicializátor členů pro stejné pole nebo vlastnost, jedná se o chybu.</span><span class="sxs-lookup"><span data-stu-id="08560-1253">It is an error for an object initializer to include more than one member initializer for the same field or property.</span></span>

<span data-ttu-id="08560-1254">Každý *initializer_target* následuje symbolem rovná se a buď výraz, inicializátor objektu nebo inicializátor kolekce.</span><span class="sxs-lookup"><span data-stu-id="08560-1254">Each *initializer_target* is followed by an equals sign and either an expression, an object initializer or a collection initializer.</span></span> <span data-ttu-id="08560-1255">Pro výrazy v inicializátoru objektu není možné odkazovat na nově vytvořený objekt, který inicializuje.</span><span class="sxs-lookup"><span data-stu-id="08560-1255">It is not possible for expressions within the object initializer to refer to the newly created object it is initializing.</span></span>

<span data-ttu-id="08560-1256">Inicializátor člena, který určuje výraz po zpracování znaménka rovná se, se zpracovává stejným způsobem jako přiřazení ([jednoduché přiřazení](expressions.md#simple-assignment)) k cíli.</span><span class="sxs-lookup"><span data-stu-id="08560-1256">A member initializer that specifies an expression after the equals sign is processed in the same way as an assignment ([Simple assignment](expressions.md#simple-assignment)) to the target.</span></span>

<span data-ttu-id="08560-1257">Inicializátor členu, který určuje inicializátor objektu po znaménku rovná se ***vnořeným objektem***, tj. inicializací vloženého objektu.</span><span class="sxs-lookup"><span data-stu-id="08560-1257">A member initializer that specifies an object initializer after the equals sign is a ***nested object initializer***, i.e. an initialization of an embedded object.</span></span> <span data-ttu-id="08560-1258">Namísto přiřazení nové hodnoty k poli nebo vlastnosti jsou přiřazení v inicializátoru vnořeného objektu považována za přiřazení členů pole nebo vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="08560-1258">Instead of assigning a new value to the field or property, the assignments in the nested object initializer are treated as assignments to members of the field or property.</span></span> <span data-ttu-id="08560-1259">Inicializátory vnořených objektů nelze použít pro vlastnosti s typem hodnoty nebo pro pole jen pro čtení s typem hodnoty.</span><span class="sxs-lookup"><span data-stu-id="08560-1259">Nested object initializers cannot be applied to properties with a value type, or to read-only fields with a value type.</span></span>

<span data-ttu-id="08560-1260">Inicializátor člena, který určuje inicializátor kolekce po znaménku rovná se, představuje inicializaci vložené kolekce.</span><span class="sxs-lookup"><span data-stu-id="08560-1260">A member initializer that specifies a collection initializer after the equals sign is an initialization of an embedded collection.</span></span> <span data-ttu-id="08560-1261">Namísto přiřazení nové kolekce k cílovému poli, vlastnosti nebo indexeru jsou prvky zadané v inicializátoru přidány do kolekce, na kterou odkazuje cíl.</span><span class="sxs-lookup"><span data-stu-id="08560-1261">Instead of assigning a new collection to the target field, property or indexer, the elements given in the initializer are added to the collection referenced by the target.</span></span> <span data-ttu-id="08560-1262">Cíl musí být typu kolekce, který splňuje požadavky zadané v [inicializátorech kolekce](expressions.md#collection-initializers).</span><span class="sxs-lookup"><span data-stu-id="08560-1262">The target must be of a collection type that satisfies the requirements specified in [Collection initializers](expressions.md#collection-initializers).</span></span>

<span data-ttu-id="08560-1263">Argumenty inicializátoru indexu budou vždy vyhodnocovány právě jednou.</span><span class="sxs-lookup"><span data-stu-id="08560-1263">The arguments to an index initializer will always be evaluated exactly once.</span></span> <span data-ttu-id="08560-1264">Proto i v případě, že argumenty nejsou nikdy použity (například z důvodu prázdného vnořeného inicializátoru), budou vyhodnoceny pro své vedlejší účinky.</span><span class="sxs-lookup"><span data-stu-id="08560-1264">Thus, even if the arguments end up never getting used (e.g. because of an empty nested initializer), they will be evaluated for their side effects.</span></span>

<span data-ttu-id="08560-1265">Následující třída reprezentuje bod se dvěma souřadnicemi:</span><span class="sxs-lookup"><span data-stu-id="08560-1265">The following class represents a point with two coordinates:</span></span>
```csharp
public class Point
{
    int x, y;

    public int X { get { return x; } set { x = value; } }
    public int Y { get { return y; } set { y = value; } }
}
```

<span data-ttu-id="08560-1266">Instanci `Point` lze vytvořit a inicializovat následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="08560-1266">An instance of `Point` can be created and initialized as follows:</span></span>
```csharp
Point a = new Point { X = 0, Y = 1 };
```
<span data-ttu-id="08560-1267">který má stejný účinek jako</span><span class="sxs-lookup"><span data-stu-id="08560-1267">which has the same effect as</span></span>
```csharp
Point __a = new Point();
__a.X = 0;
__a.Y = 1; 
Point a = __a;
```
<span data-ttu-id="08560-1268">kde `__a` je jinak neviditelná a nepřístupná dočasná proměnná.</span><span class="sxs-lookup"><span data-stu-id="08560-1268">where `__a` is an otherwise invisible and inaccessible temporary variable.</span></span> <span data-ttu-id="08560-1269">Následující třída představuje obdélník vytvořený ze dvou bodů:</span><span class="sxs-lookup"><span data-stu-id="08560-1269">The following class represents a rectangle created from two points:</span></span>
```csharp
public class Rectangle
{
    Point p1, p2;

    public Point P1 { get { return p1; } set { p1 = value; } }
    public Point P2 { get { return p2; } set { p2 = value; } }
}
```

<span data-ttu-id="08560-1270">Instanci `Rectangle` lze vytvořit a inicializovat následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="08560-1270">An instance of `Rectangle` can be created and initialized as follows:</span></span>
```csharp
Rectangle r = new Rectangle {
    P1 = new Point { X = 0, Y = 1 },
    P2 = new Point { X = 2, Y = 3 }
};
```
<span data-ttu-id="08560-1271">který má stejný účinek jako</span><span class="sxs-lookup"><span data-stu-id="08560-1271">which has the same effect as</span></span>
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
<span data-ttu-id="08560-1272">kde `__r`, `__p1` a `__p2` jsou dočasné proměnné, které jsou jinak skryté a nedostupné.</span><span class="sxs-lookup"><span data-stu-id="08560-1272">where `__r`, `__p1` and `__p2` are temporary variables that are otherwise invisible and inaccessible.</span></span>

<span data-ttu-id="08560-1273">Pokud konstruktor `Rectangle`přidělí dvě vložené instance `Point`</span><span class="sxs-lookup"><span data-stu-id="08560-1273">If `Rectangle`'s constructor allocates the two embedded `Point` instances</span></span>
```csharp
public class Rectangle
{
    Point p1 = new Point();
    Point p2 = new Point();

    public Point P1 { get { return p1; } }
    public Point P2 { get { return p2; } }
}
```
<span data-ttu-id="08560-1274">následující konstruktor lze použít k inicializaci vložených instancí `Point` namísto přiřazení nových instancí:</span><span class="sxs-lookup"><span data-stu-id="08560-1274">the following construct can be used to initialize the embedded `Point` instances instead of assigning new instances:</span></span>
```csharp
Rectangle r = new Rectangle {
    P1 = { X = 0, Y = 1 },
    P2 = { X = 2, Y = 3 }
};
```
<span data-ttu-id="08560-1275">který má stejný účinek jako</span><span class="sxs-lookup"><span data-stu-id="08560-1275">which has the same effect as</span></span>
```csharp
Rectangle __r = new Rectangle();
__r.P1.X = 0;
__r.P1.Y = 1;
__r.P2.X = 2;
__r.P2.Y = 3;
Rectangle r = __r;
```

<span data-ttu-id="08560-1276">V důsledku příslušné definice jazyka C, následující příklad:</span><span class="sxs-lookup"><span data-stu-id="08560-1276">Given an appropriate definition of C, the following example:</span></span>
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
<span data-ttu-id="08560-1277">je ekvivalentem této řady přiřazení:</span><span class="sxs-lookup"><span data-stu-id="08560-1277">is equivalent to this series of assignments:</span></span>
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
<span data-ttu-id="08560-1278">kde `__c`atd. jsou generovány proměnné, které jsou neviditelné a nepřístupné ke zdrojovému kódu.</span><span class="sxs-lookup"><span data-stu-id="08560-1278">where `__c`, etc., are generated variables that are invisible and inaccessible to the source code.</span></span> <span data-ttu-id="08560-1279">Všimněte si, že argumenty pro `[0,0]` jsou vyhodnocovány pouze jednou a argumenty pro `[1,2]` jsou vyhodnoceny jednou, i když nejsou nikdy použity.</span><span class="sxs-lookup"><span data-stu-id="08560-1279">Note that the arguments for `[0,0]` are evaluated only once, and the arguments for `[1,2]` are evaluated once even though they are never used.</span></span>

#### <a name="collection-initializers"></a><span data-ttu-id="08560-1280">Inicializátory kolekce</span><span class="sxs-lookup"><span data-stu-id="08560-1280">Collection initializers</span></span>

<span data-ttu-id="08560-1281">Inicializátor kolekce Určuje prvky kolekce.</span><span class="sxs-lookup"><span data-stu-id="08560-1281">A collection initializer specifies the elements of a collection.</span></span>

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

<span data-ttu-id="08560-1282">Inicializátor kolekce se skládá z sekvence inicializátorů elementů, které jsou uzavřeny `{` a `}` tokeny a odděleny čárkami.</span><span class="sxs-lookup"><span data-stu-id="08560-1282">A collection initializer consists of a sequence of element initializers, enclosed by `{` and `}` tokens and separated by commas.</span></span> <span data-ttu-id="08560-1283">Každý inicializátor elementu určuje prvek, který má být přidán do objektu kolekce, který se inicializuje, a skládá se ze seznamu výrazů, které jsou uzavřeny `{` a `}` tokeny a odděleny čárkami.</span><span class="sxs-lookup"><span data-stu-id="08560-1283">Each element initializer specifies an element to be added to the collection object being initialized, and consists of a list of expressions enclosed by `{` and `}` tokens and separated by commas.</span></span>  <span data-ttu-id="08560-1284">Inicializátor elementu s jedním výrazem se dá zapsat bez složených závorek, ale nemůže být výrazem přiřazení, aby nedocházelo k nejednoznačnosti u inicializátorů členů.</span><span class="sxs-lookup"><span data-stu-id="08560-1284">A single-expression element initializer can be written without braces, but cannot then be an assignment expression, to avoid ambiguity with member initializers.</span></span> <span data-ttu-id="08560-1285">Ve [výrazu](expressions.md#expression)je definovaná výroba *non_assignment_expression* .</span><span class="sxs-lookup"><span data-stu-id="08560-1285">The *non_assignment_expression* production is defined in [Expression](expressions.md#expression).</span></span>

<span data-ttu-id="08560-1286">Následuje příklad výrazu vytvoření objektu, který obsahuje inicializátor kolekce:</span><span class="sxs-lookup"><span data-stu-id="08560-1286">The following is an example of an object creation expression that includes a collection initializer:</span></span>
```csharp
List<int> digits = new List<int> { 0, 1, 2, 3, 4, 5, 6, 7, 8, 9 };
```

<span data-ttu-id="08560-1287">Objekt kolekce, pro který je použit inicializátor kolekce, musí být typu, který implementuje `System.Collections.IEnumerable` nebo dojde k chybě při kompilaci.</span><span class="sxs-lookup"><span data-stu-id="08560-1287">The collection object to which a collection initializer is applied must be of a type that implements `System.Collections.IEnumerable` or a compile-time error occurs.</span></span> <span data-ttu-id="08560-1288">U každého zadaného elementu v daném pořadí inicializátor kolekce vyvolá metodu `Add` v cílovém objektu se seznamem výrazů inicializátoru prvku jako seznam argumentů, aplikováním normálního vyhledávání členů a rozlišení přetěžování pro každé vyvolání.</span><span class="sxs-lookup"><span data-stu-id="08560-1288">For each specified element in order, the collection initializer invokes an `Add` method on the target object with the expression list of the element initializer as argument list, applying normal member lookup and overload resolution for each invocation.</span></span> <span data-ttu-id="08560-1289">Proto objekt kolekce musí mít platnou instanci nebo metodu rozšíření s názvem `Add` pro každý inicializátor elementu.</span><span class="sxs-lookup"><span data-stu-id="08560-1289">Thus, the collection object must have an applicable instance or extension method with the name `Add` for each element initializer.</span></span>

<span data-ttu-id="08560-1290">Následující třída reprezentuje kontakt s názvem a seznamem telefonních čísel:</span><span class="sxs-lookup"><span data-stu-id="08560-1290">The following class represents a contact with a name and a list of phone numbers:</span></span>
```csharp
public class Contact
{
    string name;
    List<string> phoneNumbers = new List<string>();

    public string Name { get { return name; } set { name = value; } }

    public List<string> PhoneNumbers { get { return phoneNumbers; } }
}
```

<span data-ttu-id="08560-1291">`List<Contact>` lze vytvořit a inicializovat následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="08560-1291">A `List<Contact>` can be created and initialized as follows:</span></span>
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
<span data-ttu-id="08560-1292">který má stejný účinek jako</span><span class="sxs-lookup"><span data-stu-id="08560-1292">which has the same effect as</span></span>
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
<span data-ttu-id="08560-1293">kde `__clist`, `__c1` a `__c2` jsou dočasné proměnné, které jsou jinak skryté a nedostupné.</span><span class="sxs-lookup"><span data-stu-id="08560-1293">where `__clist`, `__c1` and `__c2` are temporary variables that are otherwise invisible and inaccessible.</span></span>

#### <a name="array-creation-expressions"></a><span data-ttu-id="08560-1294">Výrazy pro vytváření polí</span><span class="sxs-lookup"><span data-stu-id="08560-1294">Array creation expressions</span></span>

<span data-ttu-id="08560-1295">*Array_creation_expression* slouží k vytvoření nové instance *array_type*.</span><span class="sxs-lookup"><span data-stu-id="08560-1295">An *array_creation_expression* is used to create a new instance of an *array_type*.</span></span>

```antlr
array_creation_expression
    : 'new' non_array_type '[' expression_list ']' rank_specifier* array_initializer?
    | 'new' array_type array_initializer
    | 'new' rank_specifier array_initializer
    ;
```

<span data-ttu-id="08560-1296">Výraz vytvoření pole prvního formuláře přiděluje instanci pole typu, který je výsledkem odstranění každého jednotlivého výrazu ze seznamu výrazů.</span><span class="sxs-lookup"><span data-stu-id="08560-1296">An array creation expression of the first form allocates an array instance of the type that results from deleting each of the individual expressions from the expression list.</span></span> <span data-ttu-id="08560-1297">Například výraz vytvoření pole `new int[10,20]` vytvoří instanci pole typu `int[,]`a výraz pro vytvoření pole `new int[10][,]` vytvoří pole typu `int[][,]`.</span><span class="sxs-lookup"><span data-stu-id="08560-1297">For example, the array creation expression `new int[10,20]` produces an array instance of type `int[,]`, and the array creation expression `new int[10][,]` produces an array of type `int[][,]`.</span></span> <span data-ttu-id="08560-1298">Každý výraz v seznamu výrazů musí být typu `int`, `uint`, `long`nebo `ulong`nebo implicitně převést na jeden nebo více těchto typů.</span><span class="sxs-lookup"><span data-stu-id="08560-1298">Each expression in the expression list must be of type `int`, `uint`, `long`, or `ulong`, or implicitly convertible to one or more of these types.</span></span> <span data-ttu-id="08560-1299">Hodnota každého výrazu určuje délku odpovídající dimenze v nově přidělené instanci pole.</span><span class="sxs-lookup"><span data-stu-id="08560-1299">The value of each expression determines the length of the corresponding dimension in the newly allocated array instance.</span></span> <span data-ttu-id="08560-1300">Vzhledem k tomu, že délka dimenze pole musí být nezáporná, jedná se o chybu při kompilaci, která má *constant_expression* se zápornou hodnotou v seznamu výrazů.</span><span class="sxs-lookup"><span data-stu-id="08560-1300">Since the length of an array dimension must be nonnegative, it is a compile-time error to have a *constant_expression* with a negative value in the expression list.</span></span>

<span data-ttu-id="08560-1301">S výjimkou nebezpečného kontextu ([nezabezpečené](unsafe-code.md#unsafe-contexts)kontexty) je rozložení polí Neurčeno.</span><span class="sxs-lookup"><span data-stu-id="08560-1301">Except in an unsafe context ([Unsafe contexts](unsafe-code.md#unsafe-contexts)), the layout of arrays is unspecified.</span></span>

<span data-ttu-id="08560-1302">Pokud výraz pro vytvoření pole prvního formuláře obsahuje inicializátor pole, každý výraz v seznamu výrazů musí být konstanta a délka pořadí a dimenze, které jsou určeny seznamem výrazů, musí odpovídat hodnotám inicializátoru pole.</span><span class="sxs-lookup"><span data-stu-id="08560-1302">If an array creation expression of the first form includes an array initializer, each expression in the expression list must be a constant and the rank and dimension lengths specified by the expression list must match those of the array initializer.</span></span>

<span data-ttu-id="08560-1303">Ve výrazu pro vytvoření pole druhého nebo třetího formuláře musí pořadí zadaného typu pole nebo specifikátoru řazení odpovídat hodnotě inicializátoru pole.</span><span class="sxs-lookup"><span data-stu-id="08560-1303">In an array creation expression of the second or third form, the rank of the specified array type or rank specifier must match that of the array initializer.</span></span> <span data-ttu-id="08560-1304">Jednotlivé délky dimenzí jsou odvozeny z počtu prvků v každé z odpovídajících úrovní vnoření inicializátoru pole.</span><span class="sxs-lookup"><span data-stu-id="08560-1304">The individual dimension lengths are inferred from the number of elements in each of the corresponding nesting levels of the array initializer.</span></span> <span data-ttu-id="08560-1305">Proto výraz</span><span class="sxs-lookup"><span data-stu-id="08560-1305">Thus, the expression</span></span>
```csharp
new int[,] {{0, 1}, {2, 3}, {4, 5}}
```
<span data-ttu-id="08560-1306">přesně odpovídá</span><span class="sxs-lookup"><span data-stu-id="08560-1306">exactly corresponds to</span></span>
```csharp
new int[3, 2] {{0, 1}, {2, 3}, {4, 5}}
```

<span data-ttu-id="08560-1307">Výraz pro vytvoření pole třetího formuláře je označován jako ***implicitně typový výraz vytvoření pole***.</span><span class="sxs-lookup"><span data-stu-id="08560-1307">An array creation expression of the third form is referred to as an ***implicitly typed array creation expression***.</span></span> <span data-ttu-id="08560-1308">Je podobný druhému formuláři s tím rozdílem, že typ elementu pole není explicitně přidělen, ale je určen jako nejlepší společný typ ([hledání nejlepšího společného typu sady výrazů](expressions.md#finding-the-best-common-type-of-a-set-of-expressions)) sady výrazů v inicializátoru pole.</span><span class="sxs-lookup"><span data-stu-id="08560-1308">It is similar to the second form, except that the element type of the array is not explicitly given, but determined as the best common type ([Finding the best common type of a set of expressions](expressions.md#finding-the-best-common-type-of-a-set-of-expressions)) of the set of expressions in the array initializer.</span></span> <span data-ttu-id="08560-1309">U multidimenzionálního pole, tj. jeden, kde *rank_specifier* obsahuje alespoň jednu čárku, tato sada zahrnuje všechny *výrazy*, které byly nalezeny ve vnořených *array_initializer*s.</span><span class="sxs-lookup"><span data-stu-id="08560-1309">For a multidimensional array, i.e., one where the *rank_specifier* contains at least one comma, this set comprises all *expression*s found in nested *array_initializer*s.</span></span>

<span data-ttu-id="08560-1310">Inicializátory polí jsou podrobněji popsány v [inicializátorech polí](arrays.md#array-initializers).</span><span class="sxs-lookup"><span data-stu-id="08560-1310">Array initializers are described further in [Array initializers](arrays.md#array-initializers).</span></span>

<span data-ttu-id="08560-1311">Výsledek vyhodnocení výrazu vytvoření pole je klasifikován jako hodnota, konkrétně odkaz na nově přidělenou instanci pole.</span><span class="sxs-lookup"><span data-stu-id="08560-1311">The result of evaluating an array creation expression is classified as a value, namely a reference to the newly allocated array instance.</span></span> <span data-ttu-id="08560-1312">Zpracování výrazu pro vytvoření pole se skládá z následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="08560-1312">The run-time processing of an array creation expression consists of the following steps:</span></span>

*  <span data-ttu-id="08560-1313">Výrazy délka dimenze *expression_list* jsou vyhodnocovány v pořadí zleva doprava.</span><span class="sxs-lookup"><span data-stu-id="08560-1313">The dimension length expressions of the *expression_list* are evaluated in order, from left to right.</span></span> <span data-ttu-id="08560-1314">Po vyhodnocení každého výrazu je proveden implicitní převod ([implicitní převod](conversions.md#implicit-conversions)) na jeden z následujících typů: `int`, `uint`, `long``ulong`.</span><span class="sxs-lookup"><span data-stu-id="08560-1314">Following evaluation of each expression, an implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)) to one of the following types is performed: `int`, `uint`, `long`, `ulong`.</span></span> <span data-ttu-id="08560-1315">První typ v tomto seznamu, pro který existuje implicitní převod.</span><span class="sxs-lookup"><span data-stu-id="08560-1315">The first type in this list for which an implicit conversion exists is chosen.</span></span> <span data-ttu-id="08560-1316">Pokud vyhodnocení výrazu nebo následný implicitní převod způsobí výjimku, nebudou vyhodnoceny žádné další výrazy a nejsou spuštěny žádné další kroky.</span><span class="sxs-lookup"><span data-stu-id="08560-1316">If evaluation of an expression or the subsequent implicit conversion causes an exception, then no further expressions are evaluated and no further steps are executed.</span></span>
*  <span data-ttu-id="08560-1317">Vypočítané hodnoty pro délky dimenzí jsou ověřeny následujícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="08560-1317">The computed values for the dimension lengths are validated as follows.</span></span> <span data-ttu-id="08560-1318">Pokud je jedna nebo více hodnot menší než nula, je vyvolána `System.OverflowException` a nejsou provedeny žádné další kroky.</span><span class="sxs-lookup"><span data-stu-id="08560-1318">If one or more of the values are less than zero, a `System.OverflowException` is thrown and no further steps are executed.</span></span>
*  <span data-ttu-id="08560-1319">Je přidělena instance pole s danými délkami dimenzí.</span><span class="sxs-lookup"><span data-stu-id="08560-1319">An array instance with the given dimension lengths is allocated.</span></span> <span data-ttu-id="08560-1320">Pokud není k dispozici dostatek paměti pro přidělení nové instance, je vyvolána `System.OutOfMemoryException` a nejsou spuštěny žádné další kroky.</span><span class="sxs-lookup"><span data-stu-id="08560-1320">If there is not enough memory available to allocate the new instance, a `System.OutOfMemoryException` is thrown and no further steps are executed.</span></span>
*  <span data-ttu-id="08560-1321">Všechny prvky nové instance pole jsou inicializovány na výchozí hodnoty ([výchozí hodnoty](variables.md#default-values)).</span><span class="sxs-lookup"><span data-stu-id="08560-1321">All elements of the new array instance are initialized to their default values ([Default values](variables.md#default-values)).</span></span>
*  <span data-ttu-id="08560-1322">Pokud výraz pro vytvoření pole obsahuje inicializátor pole, pak se každý výraz v inicializátoru pole vyhodnotí a přiřadí odpovídajícímu prvku pole.</span><span class="sxs-lookup"><span data-stu-id="08560-1322">If the array creation expression contains an array initializer, then each expression in the array initializer is evaluated and assigned to its corresponding array element.</span></span> <span data-ttu-id="08560-1323">Vyhodnocení a přiřazení jsou prováděny v pořadí, ve kterém jsou výrazy zapsány v inicializátoru pole – jinými slovy, prvky jsou inicializovány ve vzestupném pořadí indexů, přičemž výše uvedená dimenze nejvíce roste.</span><span class="sxs-lookup"><span data-stu-id="08560-1323">The evaluations and assignments are performed in the order the expressions are written in the array initializer—in other words, elements are initialized in increasing index order, with the rightmost dimension increasing first.</span></span> <span data-ttu-id="08560-1324">Pokud vyhodnocení daného výrazu nebo následné přiřazení k odpovídajícímu elementu pole způsobí výjimku, nebudou inicializovány žádné další prvky (a zbývající prvky budou mít výchozí hodnoty).</span><span class="sxs-lookup"><span data-stu-id="08560-1324">If evaluation of a given expression or the subsequent assignment to the corresponding array element causes an exception, then no further elements are initialized (and the remaining elements will thus have their default values).</span></span>

<span data-ttu-id="08560-1325">Výraz pro vytvoření pole povoluje vytvoření instance pole s prvky typu pole, ale prvky takového pole musí být manuálně inicializovány.</span><span class="sxs-lookup"><span data-stu-id="08560-1325">An array creation expression permits instantiation of an array with elements of an array type, but the elements of such an array must be manually initialized.</span></span> <span data-ttu-id="08560-1326">Například příkaz</span><span class="sxs-lookup"><span data-stu-id="08560-1326">For example, the statement</span></span>
```csharp
int[][] a = new int[100][];
```
<span data-ttu-id="08560-1327">Vytvoří jednorozměrné pole s 100 prvky typu `int[]`.</span><span class="sxs-lookup"><span data-stu-id="08560-1327">creates a single-dimensional array with 100 elements of type `int[]`.</span></span> <span data-ttu-id="08560-1328">Počáteční hodnota každého prvku je `null`.</span><span class="sxs-lookup"><span data-stu-id="08560-1328">The initial value of each element is `null`.</span></span> <span data-ttu-id="08560-1329">Je možné, že stejný výraz vytvoření pole také nemůže vytvořit instanci dílčích polí a příkaz</span><span class="sxs-lookup"><span data-stu-id="08560-1329">It is not possible for the same array creation expression to also instantiate the sub-arrays, and the statement</span></span>
```csharp
int[][] a = new int[100][5];        // Error
```
<span data-ttu-id="08560-1330">má za následek chybu při kompilaci.</span><span class="sxs-lookup"><span data-stu-id="08560-1330">results in a compile-time error.</span></span> <span data-ttu-id="08560-1331">Instance dílčích polí musí být provedena ručně, jako v</span><span class="sxs-lookup"><span data-stu-id="08560-1331">Instantiation of the sub-arrays must instead be performed manually, as in</span></span>
```csharp
int[][] a = new int[100][];
for (int i = 0; i < 100; i++) a[i] = new int[5];
```

<span data-ttu-id="08560-1332">Když pole pole má tvar "obdélníkový", který je v případě, že jsou dílčí pole všechny stejné délky, je efektivnější použít multidimenzionální pole.</span><span class="sxs-lookup"><span data-stu-id="08560-1332">When an array of arrays has a "rectangular" shape, that is when the sub-arrays are all of the same length, it is more efficient to use a multi-dimensional array.</span></span> <span data-ttu-id="08560-1333">Ve výše uvedeném příkladu vytváří instance pole polí objekty 101 – jedno vnější pole a 100 dílčích polí.</span><span class="sxs-lookup"><span data-stu-id="08560-1333">In the example above, instantiation of the array of arrays creates 101 objects—one outer array and 100 sub-arrays.</span></span> <span data-ttu-id="08560-1334">Naproti tomu</span><span class="sxs-lookup"><span data-stu-id="08560-1334">In contrast,</span></span>
```csharp
int[,] = new int[100, 5];
```
<span data-ttu-id="08560-1335">vytvoří pouze jeden objekt, dvourozměrné pole a provede přidělení v jednom příkazu.</span><span class="sxs-lookup"><span data-stu-id="08560-1335">creates only a single object, a two-dimensional array, and accomplishes the allocation in a single statement.</span></span>

<span data-ttu-id="08560-1336">Následují příklady implicitně typované výrazy vytváření pole:</span><span class="sxs-lookup"><span data-stu-id="08560-1336">The following are examples of implicitly typed array creation expressions:</span></span>
```csharp
var a = new[] { 1, 10, 100, 1000 };                       // int[]

var b = new[] { 1, 1.5, 2, 2.5 };                         // double[]

var c = new[,] { { "hello", null }, { "world", "!" } };   // string[,]

var d = new[] { 1, "one", 2, "two" };                     // Error
```

<span data-ttu-id="08560-1337">Poslední výraz způsobí chybu při kompilaci, protože není `int` ani `string` implicitně převoditelné na druhý, a proto neexistuje žádný nejlepší společný typ.</span><span class="sxs-lookup"><span data-stu-id="08560-1337">The last expression causes a compile-time error because neither `int` nor `string` is implicitly convertible to the other, and so there is no best common type.</span></span> <span data-ttu-id="08560-1338">V tomto případě musí být použit explicitní výraz pro vytvoření pole, například určení typu, který má být `object[]`.</span><span class="sxs-lookup"><span data-stu-id="08560-1338">An explicitly typed array creation expression must be used in this case, for example specifying the type to be `object[]`.</span></span> <span data-ttu-id="08560-1339">Alternativně je možné jeden z prvků přetypovat na společný základní typ, který by pak představoval odvozený typ elementu.</span><span class="sxs-lookup"><span data-stu-id="08560-1339">Alternatively, one of the elements can be cast to a common base type, which would then become the inferred element type.</span></span>

<span data-ttu-id="08560-1340">Výrazy vytváření implicitně typovaného pole lze kombinovat s Inicializátory anonymních objektů ([výrazy vytváření anonymních objektů](expressions.md#anonymous-object-creation-expressions)) k vytváření anonymních typů datových struktur.</span><span class="sxs-lookup"><span data-stu-id="08560-1340">Implicitly typed array creation expressions can be combined with anonymous object initializers ([Anonymous object creation expressions](expressions.md#anonymous-object-creation-expressions)) to create anonymously typed data structures.</span></span> <span data-ttu-id="08560-1341">Příklad:</span><span class="sxs-lookup"><span data-stu-id="08560-1341">For example:</span></span>
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

#### <a name="delegate-creation-expressions"></a><span data-ttu-id="08560-1342">Výrazy vytváření delegátů</span><span class="sxs-lookup"><span data-stu-id="08560-1342">Delegate creation expressions</span></span>

<span data-ttu-id="08560-1343">*Delegate_creation_expression* slouží k vytvoření nové instance *delegate_type*.</span><span class="sxs-lookup"><span data-stu-id="08560-1343">A *delegate_creation_expression* is used to create a new instance of a *delegate_type*.</span></span>

```antlr
delegate_creation_expression
    : 'new' delegate_type '(' expression ')'
    ;
```

<span data-ttu-id="08560-1344">Argument výrazu vytvoření delegáta musí být skupina metod, anonymní funkce nebo hodnota typu doby kompilace `dynamic` nebo *delegate_type*.</span><span class="sxs-lookup"><span data-stu-id="08560-1344">The argument of a delegate creation expression must be a method group, an anonymous function or a value of either the compile time type `dynamic` or a *delegate_type*.</span></span> <span data-ttu-id="08560-1345">Pokud je argumentem skupina metod, identifikuje metodu a pro metodu instance objekt, pro který chcete vytvořit delegáta.</span><span class="sxs-lookup"><span data-stu-id="08560-1345">If the argument is a method group, it identifies the method and, for an instance method, the object for which to create a delegate.</span></span> <span data-ttu-id="08560-1346">Pokud je argumentem anonymní funkce, přímo definuje parametry a tělo metody v cíli delegáta.</span><span class="sxs-lookup"><span data-stu-id="08560-1346">If the argument is an anonymous function it directly defines the parameters and method body of the delegate target.</span></span> <span data-ttu-id="08560-1347">Pokud je argumentem hodnota, která identifikuje instanci delegáta, pro kterou chcete vytvořit kopii.</span><span class="sxs-lookup"><span data-stu-id="08560-1347">If the argument is a value it identifies a delegate instance of which to create a copy.</span></span>

<span data-ttu-id="08560-1348">Pokud má *výraz* typ doby kompilace `dynamic`, *delegate_creation_expression* je dynamicky svázán ([dynamická vazba](expressions.md#dynamic-binding)) a níže uvedená pravidla jsou použita za běhu pomocí běhového typu *výrazu*.</span><span class="sxs-lookup"><span data-stu-id="08560-1348">If the *expression* has the compile-time type `dynamic`, the *delegate_creation_expression* is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)), and the rules below are applied at run-time using the run-time type of the *expression*.</span></span> <span data-ttu-id="08560-1349">V opačném případě pravidla jsou použita v době kompilace.</span><span class="sxs-lookup"><span data-stu-id="08560-1349">Otherwise the rules are applied at compile-time.</span></span>

<span data-ttu-id="08560-1350">Zpracování *delegate_creation_expression* `new D(E)`formuláře při vytváření vazby, kde `D` je *delegate_type* a `E` je *výraz*, skládá se z následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="08560-1350">The binding-time processing of a *delegate_creation_expression* of the form `new D(E)`, where `D` is a *delegate_type* and `E` is an *expression*, consists of the following steps:</span></span>

*  <span data-ttu-id="08560-1351">Pokud `E` je skupina metod, je výraz vytvoření delegáta zpracován stejným způsobem jako převod skupiny metod ([převody skupin metod](conversions.md#method-group-conversions)) z `E` na `D`.</span><span class="sxs-lookup"><span data-stu-id="08560-1351">If `E` is a method group, the delegate creation expression is processed in the same way as a method group conversion ([Method group conversions](conversions.md#method-group-conversions)) from `E` to `D`.</span></span>
*  <span data-ttu-id="08560-1352">Je-li `E` anonymní funkce, je výraz vytvoření delegáta zpracován stejným způsobem jako konverze anonymní funkce ([anonymní převod funkcí](conversions.md#anonymous-function-conversions)) z `E` na `D`.</span><span class="sxs-lookup"><span data-stu-id="08560-1352">If `E` is an anonymous function, the delegate creation expression is processed in the same way as an anonymous function conversion ([Anonymous function conversions](conversions.md#anonymous-function-conversions)) from `E` to `D`.</span></span>
*  <span data-ttu-id="08560-1353">Pokud `E` je hodnota, `E` musí být kompatibilní ([deklarace delegátů](delegates.md#delegate-declarations)) s `D`a výsledkem je odkaz na nově vytvořený delegát typu `D`, který odkazuje na stejný seznam volání jako `E`.</span><span class="sxs-lookup"><span data-stu-id="08560-1353">If `E` is a value, `E` must be compatible ([Delegate declarations](delegates.md#delegate-declarations)) with `D`, and the result is a reference to a newly created delegate of type `D` that refers to the same invocation list as `E`.</span></span> <span data-ttu-id="08560-1354">Pokud `E` není kompatibilní s `D`, dojde k chybě při kompilaci.</span><span class="sxs-lookup"><span data-stu-id="08560-1354">If `E` is not compatible with `D`, a compile-time error occurs.</span></span>

<span data-ttu-id="08560-1355">Zpracování *delegate_creation_expression* `new D(E)`formuláře v době běhu, kde `D` je *delegate_type* a `E` je *výraz*, skládá se z následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="08560-1355">The run-time processing of a *delegate_creation_expression* of the form `new D(E)`, where `D` is a *delegate_type* and `E` is an *expression*, consists of the following steps:</span></span>

*   <span data-ttu-id="08560-1356">Pokud je `E` skupinou metod, výraz vytvoření delegáta se vyhodnotí jako převod skupiny metod ([převody skupin metod](conversions.md#method-group-conversions)) z `E` na `D`.</span><span class="sxs-lookup"><span data-stu-id="08560-1356">If `E` is a method group, the delegate creation expression is evaluated as a method group conversion ([Method group conversions](conversions.md#method-group-conversions)) from `E` to `D`.</span></span>
*   <span data-ttu-id="08560-1357">Pokud je `E` anonymní funkce, vytvoření delegáta se vyhodnotí jako anonymní konverze funkce z `E` na `D` ([anonymní převody funkcí](conversions.md#anonymous-function-conversions)).</span><span class="sxs-lookup"><span data-stu-id="08560-1357">If `E` is an anonymous function, the delegate creation is evaluated as an anonymous function conversion from `E` to `D` ([Anonymous function conversions](conversions.md#anonymous-function-conversions)).</span></span>
*   <span data-ttu-id="08560-1358">Pokud `E` je hodnota *delegate_type*:</span><span class="sxs-lookup"><span data-stu-id="08560-1358">If `E` is a value of a *delegate_type*:</span></span>
    * <span data-ttu-id="08560-1359">`E` je vyhodnocena.</span><span class="sxs-lookup"><span data-stu-id="08560-1359">`E` is evaluated.</span></span> <span data-ttu-id="08560-1360">Pokud toto vyhodnocení způsobí výjimku, nejsou provedeny žádné další kroky.</span><span class="sxs-lookup"><span data-stu-id="08560-1360">If this evaluation causes an exception, no further steps are executed.</span></span>
    * <span data-ttu-id="08560-1361">Je-li hodnota `E` `null`, je vyvolána `System.NullReferenceException` a nejsou provedeny žádné další kroky.</span><span class="sxs-lookup"><span data-stu-id="08560-1361">If the value of `E` is `null`, a `System.NullReferenceException` is thrown and no further steps are executed.</span></span>
    * <span data-ttu-id="08560-1362">Je přidělena nová instance typu delegáta `D`.</span><span class="sxs-lookup"><span data-stu-id="08560-1362">A new instance of the delegate type `D` is allocated.</span></span> <span data-ttu-id="08560-1363">Pokud není k dispozici dostatek paměti pro přidělení nové instance, je vyvolána `System.OutOfMemoryException` a nejsou spuštěny žádné další kroky.</span><span class="sxs-lookup"><span data-stu-id="08560-1363">If there is not enough memory available to allocate the new instance, a `System.OutOfMemoryException` is thrown and no further steps are executed.</span></span>
    * <span data-ttu-id="08560-1364">Nová instance delegáta se inicializuje se stejným seznamem vyvolání jako instance delegáta zadaná `E`.</span><span class="sxs-lookup"><span data-stu-id="08560-1364">The new delegate instance is initialized with the same invocation list as the delegate instance given by `E`.</span></span>

<span data-ttu-id="08560-1365">Seznam volání delegáta je určen při vytváření instance delegáta a pak zůstává konstantní pro celou dobu života delegáta.</span><span class="sxs-lookup"><span data-stu-id="08560-1365">The invocation list of a delegate is determined when the delegate is instantiated and then remains constant for the entire lifetime of the delegate.</span></span> <span data-ttu-id="08560-1366">Jinými slovy, nemůžete změnit cílovou entitu volat jako delegované, jakmile se vytvoří.</span><span class="sxs-lookup"><span data-stu-id="08560-1366">In other words, it is not possible to change the target callable entities of a delegate once it has been created.</span></span> <span data-ttu-id="08560-1367">Pokud jsou dva Delegáti zkombinováni nebo jeden z nich odebrán z jiné ([deklarace delegátů](delegates.md#delegate-declarations)), nové výsledky delegáta; u žádného existujícího delegáta se změnil jeho obsah.</span><span class="sxs-lookup"><span data-stu-id="08560-1367">When two delegates are combined or one is removed from another ([Delegate declarations](delegates.md#delegate-declarations)), a new delegate results; no existing delegate has its contents changed.</span></span>

<span data-ttu-id="08560-1368">Není možné vytvořit delegáta, který odkazuje na vlastnost, indexer, uživatelsky definovaný operátor, konstruktor instance, destruktor nebo statický konstruktor.</span><span class="sxs-lookup"><span data-stu-id="08560-1368">It is not possible to create a delegate that refers to a property, indexer, user-defined operator, instance constructor, destructor, or static constructor.</span></span>

<span data-ttu-id="08560-1369">Jak je popsáno výše, při vytvoření delegáta ze skupiny metod, seznam formálních parametrů a návratový typ delegáta určují, které z přetížených metod vybrat.</span><span class="sxs-lookup"><span data-stu-id="08560-1369">As described above, when a delegate is created from a method group, the formal parameter list and return type of the delegate determine which of the overloaded methods to select.</span></span> <span data-ttu-id="08560-1370">V příkladu</span><span class="sxs-lookup"><span data-stu-id="08560-1370">In the example</span></span>
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
<span data-ttu-id="08560-1371">pole `A.f` je inicializováno s delegátem, který odkazuje na druhou metodu `Square`, protože tato metoda přesně odpovídá seznamu formálních parametrů a návratový typ `DoubleFunc`.</span><span class="sxs-lookup"><span data-stu-id="08560-1371">the `A.f` field is initialized with a delegate that refers to the second `Square` method because that method exactly matches the formal parameter list and return type of `DoubleFunc`.</span></span> <span data-ttu-id="08560-1372">Nebyla nalezena druhá metoda `Square`, došlo k chybě při kompilaci.</span><span class="sxs-lookup"><span data-stu-id="08560-1372">Had the second `Square` method not been present, a compile-time error would have occurred.</span></span>

#### <a name="anonymous-object-creation-expressions"></a><span data-ttu-id="08560-1373">Výrazy pro vytváření anonymních objektů</span><span class="sxs-lookup"><span data-stu-id="08560-1373">Anonymous object creation expressions</span></span>

<span data-ttu-id="08560-1374">*Anonymous_object_creation_expression* slouží k vytvoření objektu anonymního typu.</span><span class="sxs-lookup"><span data-stu-id="08560-1374">An *anonymous_object_creation_expression* is used to create an object of an anonymous type.</span></span>

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

<span data-ttu-id="08560-1375">Inicializátor anonymního objektu deklaruje anonymní typ a vrátí instanci tohoto typu.</span><span class="sxs-lookup"><span data-stu-id="08560-1375">An anonymous object initializer declares an anonymous type and returns an instance of that type.</span></span> <span data-ttu-id="08560-1376">Anonymní typ je Nameless typ třídy, který dědí přímo z `object`.</span><span class="sxs-lookup"><span data-stu-id="08560-1376">An anonymous type is a nameless class type that inherits directly from `object`.</span></span> <span data-ttu-id="08560-1377">Členové anonymního typu jsou sekvence vlastností jen pro čtení odvozeny od inicializátoru anonymního objektu použitého k vytvoření instance typu.</span><span class="sxs-lookup"><span data-stu-id="08560-1377">The members of an anonymous type are a sequence of read-only properties inferred from the anonymous object initializer used to create an instance of the type.</span></span> <span data-ttu-id="08560-1378">Konkrétně inicializátor anonymních objektů ve formuláři</span><span class="sxs-lookup"><span data-stu-id="08560-1378">Specifically, an anonymous object initializer of the form</span></span>
```csharp
new { p1 = e1, p2 = e2, ..., pn = en }
```
<span data-ttu-id="08560-1379">deklaruje anonymní typ formuláře.</span><span class="sxs-lookup"><span data-stu-id="08560-1379">declares an anonymous type of the form</span></span>
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
<span data-ttu-id="08560-1380">kde každý `Tx` je typ odpovídajícího `ex`výrazů.</span><span class="sxs-lookup"><span data-stu-id="08560-1380">where each `Tx` is the type of the corresponding expression `ex`.</span></span> <span data-ttu-id="08560-1381">Výraz použitý v *member_declarator* musí mít typ.</span><span class="sxs-lookup"><span data-stu-id="08560-1381">The expression used in a *member_declarator* must have a type.</span></span> <span data-ttu-id="08560-1382">Proto se jedná o chybu při kompilaci výrazu v *member_declarator* na hodnotu null nebo na anonymní funkci.</span><span class="sxs-lookup"><span data-stu-id="08560-1382">Thus, it is a compile-time error for an expression in a *member_declarator* to be null or an anonymous function.</span></span> <span data-ttu-id="08560-1383">Také se jedná o chybu při kompilaci, aby výraz měl nezabezpečený typ.</span><span class="sxs-lookup"><span data-stu-id="08560-1383">It is also a compile-time error for the expression to have an unsafe type.</span></span>

<span data-ttu-id="08560-1384">Názvy anonymního typu a parametru jeho `Equals` metody jsou automaticky generovány kompilátorem a nelze na něj odkazovat v textu programu.</span><span class="sxs-lookup"><span data-stu-id="08560-1384">The names of an anonymous type and of the parameter to its `Equals` method are automatically generated by the compiler and cannot be referenced in program text.</span></span>

<span data-ttu-id="08560-1385">V rámci stejného programu budou dva Inicializátory anonymních objektů, které určují sekvenci vlastností se stejnými názvy a typy pro kompilaci ve stejném pořadí, vytvořit instance stejného anonymního typu.</span><span class="sxs-lookup"><span data-stu-id="08560-1385">Within the same program, two anonymous object initializers that specify a sequence of properties of the same names and compile-time types in the same order will produce instances of the same anonymous type.</span></span>

<span data-ttu-id="08560-1386">V příkladu</span><span class="sxs-lookup"><span data-stu-id="08560-1386">In the example</span></span>
```csharp
var p1 = new { Name = "Lawnmower", Price = 495.00 };
var p2 = new { Name = "Shovel", Price = 26.95 };
p1 = p2;
```
<span data-ttu-id="08560-1387">přiřazení na posledním řádku je povoleno, protože `p1` a `p2` jsou stejného anonymního typu.</span><span class="sxs-lookup"><span data-stu-id="08560-1387">the assignment on the last line is permitted because `p1` and `p2` are of the same anonymous type.</span></span>

<span data-ttu-id="08560-1388">Metody `Equals` a `GetHashcode` v anonymních typech přepisují metody zděděné z `object`a jsou definovány v `Equals` a `GetHashcode` vlastností, aby se dvě instance stejného anonymního typu rovnaly pouze v případě, že jsou všechny jejich vlastnosti stejné.</span><span class="sxs-lookup"><span data-stu-id="08560-1388">The `Equals` and `GetHashcode` methods on anonymous types override the methods inherited from `object`, and are defined in terms of the `Equals` and `GetHashcode` of the properties, so that two instances of the same anonymous type are equal if and only if all their properties are equal.</span></span>

<span data-ttu-id="08560-1389">Člen deklarátor může být zkrácen na jednoduchý název ([odvození typu](expressions.md#type-inference)), přístup ke členu ([Kontrola při kompilaci dynamického překladu přetížení](expressions.md#compile-time-checking-of-dynamic-overload-resolution)), základní přístup ([základní přístup](expressions.md#base-access)) nebo hodnota null podmíněného přístupu členů ([podmíněné výrazy s hodnotou null jako Inicializátory projekce](expressions.md#null-conditional-expressions-as-projection-initializers)).</span><span class="sxs-lookup"><span data-stu-id="08560-1389">A member declarator can be abbreviated to a simple name ([Type inference](expressions.md#type-inference)), a member access ([Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution)), a base access ([Base access](expressions.md#base-access)) or a null-conditional member access ([Null-conditional expressions as projection initializers](expressions.md#null-conditional-expressions-as-projection-initializers)).</span></span> <span data-ttu-id="08560-1390">Tento postup se nazývá ***inicializátor projekce*** a je zkrácený pro deklaraci a přiřazení na vlastnost se stejným názvem.</span><span class="sxs-lookup"><span data-stu-id="08560-1390">This is called a ***projection initializer*** and is shorthand for a declaration of and assignment to a property with the same name.</span></span> <span data-ttu-id="08560-1391">Konkrétně členské deklarátory formuláře</span><span class="sxs-lookup"><span data-stu-id="08560-1391">Specifically, member declarators of the forms</span></span>
```csharp
identifier
expr.identifier
```
<span data-ttu-id="08560-1392">jsou přesně stejné jako v následujícím pořadí:</span><span class="sxs-lookup"><span data-stu-id="08560-1392">are precisely equivalent to the following, respectively:</span></span>
```csharp
identifier = identifier
identifier = expr.identifier
```

<span data-ttu-id="08560-1393">Proto v inicializátoru projekce *identifikátor* vybere jak hodnotu, tak pole nebo vlastnost, ke kterým je přiřazena hodnota.</span><span class="sxs-lookup"><span data-stu-id="08560-1393">Thus, in a projection initializer the *identifier* selects both the value and the field or property to which the value is assigned.</span></span> <span data-ttu-id="08560-1394">Intuitivní, projekty inicializátorů projekce nestačí pouze jako hodnota, ale také název hodnoty.</span><span class="sxs-lookup"><span data-stu-id="08560-1394">Intuitively, a projection initializer projects not just a value, but also the name of the value.</span></span>

### <a name="the-typeof-operator"></a><span data-ttu-id="08560-1395">Operátor typeof</span><span class="sxs-lookup"><span data-stu-id="08560-1395">The typeof operator</span></span>

<span data-ttu-id="08560-1396">Operátor `typeof` slouží k získání objektu `System.Type` pro typ.</span><span class="sxs-lookup"><span data-stu-id="08560-1396">The `typeof` operator is used to obtain the `System.Type` object for a type.</span></span>

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

<span data-ttu-id="08560-1397">První forma *typeof_expression* se skládá z klíčového slova `typeof` následovaného *typem*v závorkách.</span><span class="sxs-lookup"><span data-stu-id="08560-1397">The first form of *typeof_expression* consists of a `typeof` keyword followed by a parenthesized *type*.</span></span> <span data-ttu-id="08560-1398">Výsledek výrazu tohoto formuláře je objekt `System.Type` pro zadaný typ.</span><span class="sxs-lookup"><span data-stu-id="08560-1398">The result of an expression of this form is the `System.Type` object for the indicated type.</span></span> <span data-ttu-id="08560-1399">Pro daný typ je k dispozici pouze jeden objekt `System.Type`.</span><span class="sxs-lookup"><span data-stu-id="08560-1399">There is only one `System.Type` object for any given type.</span></span> <span data-ttu-id="08560-1400">To znamená, že pro typ `T``typeof(T) == typeof(T)` je vždy true.</span><span class="sxs-lookup"><span data-stu-id="08560-1400">This means that for a type `T`, `typeof(T) == typeof(T)` is always true.</span></span> <span data-ttu-id="08560-1401">*Typ* nemůže být `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="08560-1401">The *type* cannot be `dynamic`.</span></span>

<span data-ttu-id="08560-1402">Druhá forma *typeof_expression* se skládá z klíčového slova `typeof` následovaného *unbound_type_nameem*v závorkách.</span><span class="sxs-lookup"><span data-stu-id="08560-1402">The second form of *typeof_expression* consists of a `typeof` keyword followed by a parenthesized *unbound_type_name*.</span></span> <span data-ttu-id="08560-1403">*Unbound_type_name* se velmi podobá *TYPE_NAME* ([obor názvů a názvy typů](basic-concepts.md#namespace-and-type-names)) s tím rozdílem, že *unbound_type_name* obsahuje *generic_dimension_specifier*, kde *TYPE_NAME* obsahuje *type_argument_list*s.</span><span class="sxs-lookup"><span data-stu-id="08560-1403">An *unbound_type_name* is very similar to a *type_name* ([Namespace and type names](basic-concepts.md#namespace-and-type-names)) except that an *unbound_type_name* contains *generic_dimension_specifier*s where a *type_name* contains *type_argument_list*s.</span></span> <span data-ttu-id="08560-1404">Pokud je operandem *typeof_expression* sekvence tokenů, které vyhovují gramatikě *unbound_type_name* a *TYPE_NAME*, konkrétně když neobsahuje ani *generic_dimension_specifier* ani *type_argument_list*, sekvence tokenů je považována za *TYPE_NAME*.</span><span class="sxs-lookup"><span data-stu-id="08560-1404">When the operand of a *typeof_expression* is a sequence of tokens that satisfies the grammars of both *unbound_type_name* and *type_name*, namely when it contains neither a *generic_dimension_specifier* nor a *type_argument_list*, the sequence of tokens is considered to be a *type_name*.</span></span> <span data-ttu-id="08560-1405">Význam *unbound_type_name* se určuje takto:</span><span class="sxs-lookup"><span data-stu-id="08560-1405">The meaning of an *unbound_type_name* is determined as follows:</span></span>

*  <span data-ttu-id="08560-1406">Převeďte sekvenci tokenů na *TYPE_NAME* tím, že nahradíte jednotlivé *generic_dimension_specifier* *type_argument_list* mají stejný počet čárek a klíčová slova `object` jako každý *type_argument*.</span><span class="sxs-lookup"><span data-stu-id="08560-1406">Convert the sequence of tokens to a *type_name* by replacing each *generic_dimension_specifier* with a *type_argument_list* having the same number of commas and the keyword `object` as each *type_argument*.</span></span>
*  <span data-ttu-id="08560-1407">Vyhodnotit výsledný *TYPE_NAME*a ignorovat všechna omezení parametrů typu.</span><span class="sxs-lookup"><span data-stu-id="08560-1407">Evaluate the resulting *type_name*, while ignoring all type parameter constraints.</span></span>
*  <span data-ttu-id="08560-1408">*Unbound_type_name* se přeloží na obecný typ bez vazby přidruženého k výslednému konstruovanému typu ([vázané a nevázané typy](types.md#bound-and-unbound-types)).</span><span class="sxs-lookup"><span data-stu-id="08560-1408">The *unbound_type_name* resolves to the unbound generic type associated with the resulting constructed type ([Bound and unbound types](types.md#bound-and-unbound-types)).</span></span>

<span data-ttu-id="08560-1409">Výsledek *typeof_expression* je objekt `System.Type` pro výsledný nevázaný obecný typ.</span><span class="sxs-lookup"><span data-stu-id="08560-1409">The result of the *typeof_expression* is the `System.Type` object for the resulting unbound generic type.</span></span>

<span data-ttu-id="08560-1410">Třetí forma *typeof_expression* se skládá z klíčového slova `typeof` následovaného klíčovým slovem `void` v závorkách.</span><span class="sxs-lookup"><span data-stu-id="08560-1410">The third form of *typeof_expression* consists of a `typeof` keyword followed by a parenthesized `void` keyword.</span></span> <span data-ttu-id="08560-1411">Výsledek výrazu tohoto formuláře je `System.Type` objekt, který představuje absenci typu.</span><span class="sxs-lookup"><span data-stu-id="08560-1411">The result of an expression of this form is the `System.Type` object that represents the absence of a type.</span></span> <span data-ttu-id="08560-1412">Objekt typu vrácený funkcí `typeof(void)` je odlišný od objektu Type vráceného pro libovolný typ.</span><span class="sxs-lookup"><span data-stu-id="08560-1412">The type object returned by `typeof(void)` is distinct from the type object returned for any type.</span></span> <span data-ttu-id="08560-1413">Tento speciální objekt typu je užitečný v knihovnách tříd, které umožňují reflexi do metod v jazyce, kde tyto metody mají mít způsob, jak vyjádřit návratový typ jakékoli metody, včetně metod void, s instancí `System.Type`.</span><span class="sxs-lookup"><span data-stu-id="08560-1413">This special type object is useful in class libraries that allow reflection onto methods in the language, where those methods wish to have a way to represent the return type of any method, including void methods, with an instance of `System.Type`.</span></span>

<span data-ttu-id="08560-1414">Operátor `typeof` lze použít pro parametr typu.</span><span class="sxs-lookup"><span data-stu-id="08560-1414">The `typeof` operator can be used on a type parameter.</span></span> <span data-ttu-id="08560-1415">Výsledkem je objekt `System.Type` pro typ modulu runtime, který byl svázán s parametrem typu.</span><span class="sxs-lookup"><span data-stu-id="08560-1415">The result is the `System.Type` object for the run-time type that was bound to the type parameter.</span></span> <span data-ttu-id="08560-1416">Operátor `typeof` lze také použít pro konstruovaný typ nebo nevázaný obecný typ ([vázané a nevázané typy](types.md#bound-and-unbound-types)).</span><span class="sxs-lookup"><span data-stu-id="08560-1416">The `typeof` operator can also be used on a constructed type or an unbound generic type ([Bound and unbound types](types.md#bound-and-unbound-types)).</span></span> <span data-ttu-id="08560-1417">Objekt `System.Type` pro nevázaný obecný typ není stejný jako objekt `System.Type` typu instance.</span><span class="sxs-lookup"><span data-stu-id="08560-1417">The `System.Type` object for an unbound generic type is not the same as the `System.Type` object of the instance type.</span></span> <span data-ttu-id="08560-1418">Typ instance je vždy uzavřený konstruovaný typ za běhu, takže jeho `System.Type` objekt závisí na používaných argumentech běhového typu, zatímco nevázaný obecný typ nemá žádné argumenty typu.</span><span class="sxs-lookup"><span data-stu-id="08560-1418">The instance type is always a closed constructed type at run-time so its `System.Type` object depends on the run-time type arguments in use, while the unbound generic type has no type arguments.</span></span>

<span data-ttu-id="08560-1419">Příklad</span><span class="sxs-lookup"><span data-stu-id="08560-1419">The example</span></span>
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
<span data-ttu-id="08560-1420">Vytvoří následující výstup:</span><span class="sxs-lookup"><span data-stu-id="08560-1420">produces the following output:</span></span>
```console
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

<span data-ttu-id="08560-1421">Všimněte si, že `int` a `System.Int32` jsou stejného typu.</span><span class="sxs-lookup"><span data-stu-id="08560-1421">Note that `int` and `System.Int32` are the same type.</span></span>

<span data-ttu-id="08560-1422">Všimněte si také, že výsledek `typeof(X<>)` není závislý na argumentu typu, ale výsledkem `typeof(X<T>)`.</span><span class="sxs-lookup"><span data-stu-id="08560-1422">Also note that the result of `typeof(X<>)` does not depend on the type argument but the result of `typeof(X<T>)` does.</span></span>

### <a name="the-checked-and-unchecked-operators"></a><span data-ttu-id="08560-1423">Kontrolované a nezaškrtnuté operátory</span><span class="sxs-lookup"><span data-stu-id="08560-1423">The checked and unchecked operators</span></span>

<span data-ttu-id="08560-1424">Operátory `checked` a `unchecked` slouží k řízení ***kontextu kontroly přetečení*** pro aritmetické operace a převody integrálního typu.</span><span class="sxs-lookup"><span data-stu-id="08560-1424">The `checked` and `unchecked` operators are used to control the ***overflow checking context*** for integral-type arithmetic operations and conversions.</span></span>

```antlr
checked_expression
    : 'checked' '(' expression ')'
    ;

unchecked_expression
    : 'unchecked' '(' expression ')'
    ;
```

<span data-ttu-id="08560-1425">Operátor `checked` vyhodnocuje obsažený výraz v kontrolovaném kontextu a operátor `unchecked` vyhodnocuje obsažený výraz v nekontrolovaném kontextu.</span><span class="sxs-lookup"><span data-stu-id="08560-1425">The `checked` operator evaluates the contained expression in a checked context, and the `unchecked` operator evaluates the contained expression in an unchecked context.</span></span> <span data-ttu-id="08560-1426">*Checked_expression* nebo *unchecked_expression* odpovídá přesně *parenthesized_expression* ([výrazy v závorkách](expressions.md#parenthesized-expressions)) s tím rozdílem, že obsažený výraz je vyhodnocen v daném kontextu kontroly přetečení.</span><span class="sxs-lookup"><span data-stu-id="08560-1426">A *checked_expression* or *unchecked_expression* corresponds exactly to a *parenthesized_expression* ([Parenthesized expressions](expressions.md#parenthesized-expressions)), except that the contained expression is evaluated in the given overflow checking context.</span></span>

<span data-ttu-id="08560-1427">Kontext kontroly přetečení lze také ovládat pomocí příkazů `checked` a `unchecked` ([zaškrtnuté a nezaškrtnuté příkazy](statements.md#the-checked-and-unchecked-statements)).</span><span class="sxs-lookup"><span data-stu-id="08560-1427">The overflow checking context can also be controlled through the `checked` and `unchecked` statements ([The checked and unchecked statements](statements.md#the-checked-and-unchecked-statements)).</span></span>

<span data-ttu-id="08560-1428">Následující operace jsou ovlivněny kontextem kontroly přetečení vytvořeným `checked` a `unchecked` operátory a příkazy:</span><span class="sxs-lookup"><span data-stu-id="08560-1428">The following operations are affected by the overflow checking context established by the `checked` and `unchecked` operators and statements:</span></span>

*  <span data-ttu-id="08560-1429">Předdefinované `++` a `--` unární operátory ([přípony přírůstku a snížení](expressions.md#postfix-increment-and-decrement-operators) operátory a [operátory přírůstku a snížení předpony](expressions.md#prefix-increment-and-decrement-operators)), pokud je operand integrálního typu.</span><span class="sxs-lookup"><span data-stu-id="08560-1429">The predefined `++` and `--` unary operators ([Postfix increment and decrement operators](expressions.md#postfix-increment-and-decrement-operators) and [Prefix increment and decrement operators](expressions.md#prefix-increment-and-decrement-operators)), when the operand is of an integral type.</span></span>
*  <span data-ttu-id="08560-1430">Předdefinovaný `-` unární operátor ([unární operátor mínus](expressions.md#unary-minus-operator)), pokud je operand integrálního typu.</span><span class="sxs-lookup"><span data-stu-id="08560-1430">The predefined `-` unary operator ([Unary minus operator](expressions.md#unary-minus-operator)), when the operand is of an integral type.</span></span>
*  <span data-ttu-id="08560-1431">Předdefinované `+`, `-`, `*`a `/` binární operátory ([aritmetické operátory](expressions.md#arithmetic-operators)), jsou-li oba operandy integrálního typu.</span><span class="sxs-lookup"><span data-stu-id="08560-1431">The predefined `+`, `-`, `*`, and `/` binary operators ([Arithmetic operators](expressions.md#arithmetic-operators)), when both operands are of integral types.</span></span>
*  <span data-ttu-id="08560-1432">Explicitní číselné převody ([explicitní číselné převody](conversions.md#explicit-numeric-conversions)) z jednoho integrálního typu na jiný celočíselný typ nebo z `float` nebo `double` na celočíselný typ.</span><span class="sxs-lookup"><span data-stu-id="08560-1432">Explicit numeric conversions ([Explicit numeric conversions](conversions.md#explicit-numeric-conversions)) from one integral type to another integral type, or from `float` or `double` to an integral type.</span></span>

<span data-ttu-id="08560-1433">Když jedna z výše uvedených operací vytvoří výsledek, který je příliš velký pro reprezentaci v cílovém typu, kontext, ve kterém je operace provedena, řídí výsledné chování:</span><span class="sxs-lookup"><span data-stu-id="08560-1433">When one of the above operations produce a result that is too large to represent in the destination type, the context in which the operation is performed controls the resulting behavior:</span></span>

*  <span data-ttu-id="08560-1434">Pokud je operace v kontextu `checked` konstantní výraz ([konstantní výrazy](expressions.md#constant-expressions)), dojde k chybě při kompilaci.</span><span class="sxs-lookup"><span data-stu-id="08560-1434">In a `checked` context, if the operation is a constant expression ([Constant expressions](expressions.md#constant-expressions)), a compile-time error occurs.</span></span> <span data-ttu-id="08560-1435">V opačném případě, pokud je operace provedena za běhu, je vyvolána `System.OverflowException`.</span><span class="sxs-lookup"><span data-stu-id="08560-1435">Otherwise, when the operation is performed at run-time, a `System.OverflowException` is thrown.</span></span>
*  <span data-ttu-id="08560-1436">V kontextu `unchecked` se výsledek zkrátí tak, že zahodí jakékoli bity s vysokým pořadím, které se nevejdou do cílového typu.</span><span class="sxs-lookup"><span data-stu-id="08560-1436">In an `unchecked` context, the result is truncated by discarding any high-order bits that do not fit in the destination type.</span></span>

<span data-ttu-id="08560-1437">Pro nekonstantní výrazy (výrazy, které jsou vyhodnocovány v době běhu), které nejsou uzavřeny `checked` pomocí operátorů nebo příkazů `unchecked`, je výchozí kontext kontroly přetečení `unchecked`, pokud nechcete volat pro `checked` vyhodnocení externí faktory (jako jsou přepínače kompilátoru a prostředí pro spuštění).</span><span class="sxs-lookup"><span data-stu-id="08560-1437">For non-constant expressions (expressions that are evaluated at run-time) that are not enclosed by any `checked` or `unchecked` operators or statements, the default overflow checking context is `unchecked` unless external factors (such as compiler switches and execution environment configuration) call for `checked` evaluation.</span></span>

<span data-ttu-id="08560-1438">V případě konstantních výrazů (výrazy, které mohou být plně vyhodnocovány v době kompilace) je výchozí kontext kontroly přetečení vždy `checked`.</span><span class="sxs-lookup"><span data-stu-id="08560-1438">For constant expressions (expressions that can be fully evaluated at compile-time), the default overflow checking context is always `checked`.</span></span> <span data-ttu-id="08560-1439">Pokud není konstantní výraz explicitně umístěn v kontextu `unchecked`, přetečení, ke kterým dojde během kompilace výrazu, vždy způsobí chyby při kompilaci.</span><span class="sxs-lookup"><span data-stu-id="08560-1439">Unless a constant expression is explicitly placed in an `unchecked` context, overflows that occur during the compile-time evaluation of the expression always cause compile-time errors.</span></span>

<span data-ttu-id="08560-1440">Tělo anonymní funkce není ovlivněno `checked` nebo `unchecked` kontexty, ve kterých se vyskytuje anonymní funkce.</span><span class="sxs-lookup"><span data-stu-id="08560-1440">The body of an anonymous function is not affected by `checked` or `unchecked` contexts in which the anonymous function occurs.</span></span>

<span data-ttu-id="08560-1441">V příkladu</span><span class="sxs-lookup"><span data-stu-id="08560-1441">In the example</span></span>
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
<span data-ttu-id="08560-1442">nejsou hlášeny žádné chyby při kompilaci, protože není možné vyhodnotit žádné výrazy v době kompilace.</span><span class="sxs-lookup"><span data-stu-id="08560-1442">no compile-time errors are reported since neither of the expressions can be evaluated at compile-time.</span></span> <span data-ttu-id="08560-1443">V době běhu vyvolá metoda `F` `System.OverflowException`a metoda `G` vrátí-727379968 (dolní 32 bitů mimo rozsah).</span><span class="sxs-lookup"><span data-stu-id="08560-1443">At run-time, the `F` method throws a `System.OverflowException`, and the `G` method returns -727379968 (the lower 32 bits of the out-of-range result).</span></span> <span data-ttu-id="08560-1444">Chování metody `H` závisí na výchozím kontextu kontroly přetečení pro kompilaci, ale je buď stejné jako `F`, nebo stejné jako `G`.</span><span class="sxs-lookup"><span data-stu-id="08560-1444">The behavior of the `H` method depends on the default overflow checking context for the compilation, but it is either the same as `F` or the same as `G`.</span></span>

<span data-ttu-id="08560-1445">V příkladu</span><span class="sxs-lookup"><span data-stu-id="08560-1445">In the example</span></span>
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
<span data-ttu-id="08560-1446">k přetečení, ke kterým dochází při vyhodnocení konstantních výrazů v `F` a `H` způsobují chyby při kompilaci, protože výrazy jsou vyhodnocovány v `checked`m kontextu.</span><span class="sxs-lookup"><span data-stu-id="08560-1446">the overflows that occur when evaluating the constant expressions in `F` and `H` cause compile-time errors to be reported because the expressions are evaluated in a `checked` context.</span></span> <span data-ttu-id="08560-1447">K přetečení dojde také při vyhodnocení konstantního výrazu v `G`, ale vzhledem k tomu, že probíhá vyhodnocení v kontextu `unchecked`, přetečení není hlášeno.</span><span class="sxs-lookup"><span data-stu-id="08560-1447">An overflow also occurs when evaluating the constant expression in `G`, but since the evaluation takes place in an `unchecked` context, the overflow is not reported.</span></span>

<span data-ttu-id="08560-1448">Operátory `checked` a `unchecked` ovlivňují pouze kontext kontroly přetečení u těchto operací, které jsou ve formátu "`(`" a "`)`", které jsou v textu obsaženy.</span><span class="sxs-lookup"><span data-stu-id="08560-1448">The `checked` and `unchecked` operators only affect the overflow checking context for those operations that are textually contained within the "`(`" and "`)`" tokens.</span></span> <span data-ttu-id="08560-1449">Operátory nemají žádný vliv na členy funkce, které jsou vyvolány v důsledku vyhodnocení obsaženého výrazu.</span><span class="sxs-lookup"><span data-stu-id="08560-1449">The operators have no effect on function members that are invoked as a result of evaluating the contained expression.</span></span> <span data-ttu-id="08560-1450">V příkladu</span><span class="sxs-lookup"><span data-stu-id="08560-1450">In the example</span></span>
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
<span data-ttu-id="08560-1451">použití `checked` v `F` nemá vliv na vyhodnocení `x * y` v `Multiply`, takže `x * y` je vyhodnocen ve výchozím kontextu kontroly přetečení.</span><span class="sxs-lookup"><span data-stu-id="08560-1451">the use of `checked` in `F` does not affect the evaluation of `x * y` in `Multiply`, so `x * y` is evaluated in the default overflow checking context.</span></span>

<span data-ttu-id="08560-1452">Operátor `unchecked` je vhodný při psaní konstanty podepsaných integrálních typů v šestnáctkovém zápisu.</span><span class="sxs-lookup"><span data-stu-id="08560-1452">The `unchecked` operator is convenient when writing constants of the signed integral types in hexadecimal notation.</span></span> <span data-ttu-id="08560-1453">Příklad:</span><span class="sxs-lookup"><span data-stu-id="08560-1453">For example:</span></span>
```csharp
class Test
{
    public const int AllBits = unchecked((int)0xFFFFFFFF);

    public const int HighBit = unchecked((int)0x80000000);
}
```

<span data-ttu-id="08560-1454">Oba šestnáctkové konstanty jsou typu `uint`.</span><span class="sxs-lookup"><span data-stu-id="08560-1454">Both of the hexadecimal constants above are of type `uint`.</span></span> <span data-ttu-id="08560-1455">Vzhledem k tomu, že konstanty jsou mimo `int` rozsah bez operátoru `unchecked`, přetypování na `int` by způsobilo chyby při kompilaci.</span><span class="sxs-lookup"><span data-stu-id="08560-1455">Because the constants are outside the `int` range, without the `unchecked` operator, the casts to `int` would produce compile-time errors.</span></span>

<span data-ttu-id="08560-1456">Operátory a příkazy `checked` a `unchecked` umožňují programátorům řídit určité aspekty některých číselných výpočtů.</span><span class="sxs-lookup"><span data-stu-id="08560-1456">The `checked` and `unchecked` operators and statements allow programmers to control certain aspects of some numeric calculations.</span></span> <span data-ttu-id="08560-1457">Chování některých numerických operátorů ale závisí na datových typech operandů.</span><span class="sxs-lookup"><span data-stu-id="08560-1457">However, the behavior of some numeric operators depends on their operands' data types.</span></span> <span data-ttu-id="08560-1458">Například násobení dvou desetinných míst vždy způsobí výjimku při přetečení i v rámci explicitního `unchecked` konstrukce.</span><span class="sxs-lookup"><span data-stu-id="08560-1458">For example, multiplying two decimals always results in an exception on overflow even within an explicitly `unchecked` construct.</span></span> <span data-ttu-id="08560-1459">Podobně násobení dvou objektů float nikdy nevede k výjimce přetečení i v rámci explicitního `checked` konstrukce.</span><span class="sxs-lookup"><span data-stu-id="08560-1459">Similarly, multiplying two floats never results in an exception on overflow even within an explicitly `checked` construct.</span></span> <span data-ttu-id="08560-1460">Kromě toho jiné operátory nejsou nikdy ovlivněny režimem kontroly, bez ohledu na to, zda jsou výchozí nebo explicitní.</span><span class="sxs-lookup"><span data-stu-id="08560-1460">In addition, other operators are never affected by the mode of checking, whether default or explicit.</span></span>

### <a name="default-value-expressions"></a><span data-ttu-id="08560-1461">Výrazy výchozích hodnot</span><span class="sxs-lookup"><span data-stu-id="08560-1461">Default value expressions</span></span>

<span data-ttu-id="08560-1462">Výchozí hodnota výrazu je použita k získání výchozí hodnoty ([výchozí hodnoty](variables.md#default-values)) typu.</span><span class="sxs-lookup"><span data-stu-id="08560-1462">A default value expression is used to obtain the default value ([Default values](variables.md#default-values)) of a type.</span></span> <span data-ttu-id="08560-1463">Obvykle je výraz výchozí hodnoty použit pro parametry typu, protože nemusí být znám, pokud je parametr typu hodnotový typ nebo odkazový typ.</span><span class="sxs-lookup"><span data-stu-id="08560-1463">Typically a default value expression is used for type parameters, since it may not be known if the type parameter is a value type or a reference type.</span></span> <span data-ttu-id="08560-1464">(Žádný převod neexistuje z `null` literálu na parametr typu, pokud parametr typu není známý jako odkazový typ.)</span><span class="sxs-lookup"><span data-stu-id="08560-1464">(No conversion exists from the `null` literal to a type parameter unless the type parameter is known to be a reference type.)</span></span>

```antlr
default_value_expression
    : 'default' '(' type ')'
    ;
```

<span data-ttu-id="08560-1465">Pokud *typ* ve *default_value_expression* vyhodnocuje za běhu na odkazový typ, výsledek je `null` převeden na tento typ.</span><span class="sxs-lookup"><span data-stu-id="08560-1465">If the *type* in a *default_value_expression* evaluates at run-time to a reference type, the result is `null` converted to that type.</span></span> <span data-ttu-id="08560-1466">Pokud *typ* ve *default_value_expression* vyhodnocuje za běhu na hodnotový typ, výsledkem je výchozí hodnota *value_type*([výchozí konstruktory](types.md#default-constructors)).</span><span class="sxs-lookup"><span data-stu-id="08560-1466">If the *type* in a *default_value_expression* evaluates at run-time to a value type, the result is the *value_type*'s default value ([Default constructors](types.md#default-constructors)).</span></span>

<span data-ttu-id="08560-1467">*Default_value_expression* je konstantní výraz ([konstantní výrazy](expressions.md#constant-expressions)), pokud je typ odkazový typ nebo parametr typu, který je známý jako odkazový typ ([omezení parametrů typu](classes.md#type-parameter-constraints)).</span><span class="sxs-lookup"><span data-stu-id="08560-1467">A *default_value_expression* is a constant expression ([Constant expressions](expressions.md#constant-expressions)) if the type is a reference type or a type parameter that is known to be a reference type ([Type parameter constraints](classes.md#type-parameter-constraints)).</span></span> <span data-ttu-id="08560-1468">Kromě toho *default_value_expression* je konstantní výraz, pokud je typ jedním z následujících typů hodnot: `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, `decimal`, `bool`nebo jakýkoli typ výčtu.</span><span class="sxs-lookup"><span data-stu-id="08560-1468">In addition, a *default_value_expression* is a constant expression if the type is one of the following value types: `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, `decimal`, `bool`, or any enumeration type.</span></span>


### <a name="nameof-expressions"></a><span data-ttu-id="08560-1469">Výrazy nameof</span><span class="sxs-lookup"><span data-stu-id="08560-1469">Nameof expressions</span></span>

<span data-ttu-id="08560-1470">*Nameof_expression* slouží k získání názvu entity programu jako konstantního řetězce.</span><span class="sxs-lookup"><span data-stu-id="08560-1470">A *nameof_expression* is used to obtain the name of a program entity as a constant string.</span></span>

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

<span data-ttu-id="08560-1471">Gramaticky řečeno, *named_entity* operand je vždy výraz.</span><span class="sxs-lookup"><span data-stu-id="08560-1471">Grammatically speaking, the *named_entity* operand is always an expression.</span></span> <span data-ttu-id="08560-1472">Protože `nameof` není rezervované klíčové slovo, výraz nameof je vždycky syntakticky dvojznačný s voláním jednoduchého názvu `nameof`.</span><span class="sxs-lookup"><span data-stu-id="08560-1472">Because `nameof` is not a reserved keyword, a nameof expression is always syntactically ambiguous with an invocation of the simple name `nameof`.</span></span> <span data-ttu-id="08560-1473">Z důvodu kompatibility se v případě, že vyhledávání názvů ([jednoduché názvy](expressions.md#simple-names)) `nameof` podaří, je výraz považován za *invocation_expression* --bez ohledu na to, zda je vyvolání právní.</span><span class="sxs-lookup"><span data-stu-id="08560-1473">For compatibility reasons, if a name lookup ([Simple names](expressions.md#simple-names)) of the name `nameof` succeeds, the expression is treated as an *invocation_expression* -- regardless of whether the invocation is legal.</span></span> <span data-ttu-id="08560-1474">V opačném případě se jedná o *nameof_expression*.</span><span class="sxs-lookup"><span data-stu-id="08560-1474">Otherwise it is a *nameof_expression*.</span></span>

<span data-ttu-id="08560-1475">Význam *named_entity* *nameof_expression* je jejich význam jako výraz; To znamená buď jako *simple_name*, *base_access* nebo *member_access*.</span><span class="sxs-lookup"><span data-stu-id="08560-1475">The meaning of the *named_entity* of a *nameof_expression* is the meaning of it as an expression; that is, either as a *simple_name*, a *base_access* or a *member_access*.</span></span> <span data-ttu-id="08560-1476">Nicméně pokud vyhledávání popsané v [jednoduchých názvech](expressions.md#simple-names) a [přístupu ke členům](expressions.md#member-access) má za následek chybu, protože člen instance byl nalezen ve statickém kontextu, *nameof_expression* nevytváří žádnou takovou chybu.</span><span class="sxs-lookup"><span data-stu-id="08560-1476">However, where the lookup described in [Simple names](expressions.md#simple-names) and [Member access](expressions.md#member-access) results in an error because an instance member was found in a static context, a *nameof_expression* produces no such error.</span></span>

<span data-ttu-id="08560-1477">Jedná se o chybu při kompilaci *named_entity* určení skupiny metod, která má *type_argument_list*.</span><span class="sxs-lookup"><span data-stu-id="08560-1477">It is a compile-time error for a *named_entity* designating a method group to have a *type_argument_list*.</span></span> <span data-ttu-id="08560-1478">Jedná se o chybu při kompilaci *named_entity_target* , aby `dynamic`typu.</span><span class="sxs-lookup"><span data-stu-id="08560-1478">It is a compile time error for a *named_entity_target* to have the type `dynamic`.</span></span>

<span data-ttu-id="08560-1479">*Nameof_expression* je konstantní výraz typu `string`a nemá žádný vliv za běhu.</span><span class="sxs-lookup"><span data-stu-id="08560-1479">A *nameof_expression* is a constant expression of type `string`, and has no effect at runtime.</span></span> <span data-ttu-id="08560-1480">Konkrétně není vyhodnocena jeho *named_entity* a je ignorována pro účely jednoznačného přiřazení přiřazení ([Obecná pravidla pro jednoduché výrazy](variables.md#general-rules-for-simple-expressions)).</span><span class="sxs-lookup"><span data-stu-id="08560-1480">Specifically, its *named_entity* is not evaluated, and is ignored for the purposes of definite assignment analysis ([General rules for simple expressions](variables.md#general-rules-for-simple-expressions)).</span></span> <span data-ttu-id="08560-1481">Jeho hodnota je poslední identifikátor *named_entity* před volitelným konečným *type_argument_list*transformované následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="08560-1481">Its value is the last identifier of the *named_entity* before the optional final *type_argument_list*, transformed in the following way:</span></span>

* <span data-ttu-id="08560-1482">Je-li použita předpona "`@`", je odebrána.</span><span class="sxs-lookup"><span data-stu-id="08560-1482">The prefix "`@`", if used, is removed.</span></span>
* <span data-ttu-id="08560-1483">Každý *unicode_escape_sequence* se transformuje na příslušný znak Unicode.</span><span class="sxs-lookup"><span data-stu-id="08560-1483">Each *unicode_escape_sequence* is transformed into its corresponding Unicode character.</span></span>
* <span data-ttu-id="08560-1484">Odeberou se všechny *formatting_characters* .</span><span class="sxs-lookup"><span data-stu-id="08560-1484">Any *formatting_characters* are removed.</span></span>

<span data-ttu-id="08560-1485">Jedná se o stejné transformace použité v [identifikátorech](lexical-structure.md#identifiers) při testování rovnosti mezi identifikátory.</span><span class="sxs-lookup"><span data-stu-id="08560-1485">These are the same transformations applied in [Identifiers](lexical-structure.md#identifiers) when testing equality between identifiers.</span></span>

<span data-ttu-id="08560-1486">TODO: příklady</span><span class="sxs-lookup"><span data-stu-id="08560-1486">TODO: examples</span></span>

### <a name="anonymous-method-expressions"></a><span data-ttu-id="08560-1487">Anonymní výrazy metod</span><span class="sxs-lookup"><span data-stu-id="08560-1487">Anonymous method expressions</span></span>

<span data-ttu-id="08560-1488">*Anonymous_method_expression* je jedním ze dvou způsobů definování anonymní funkce.</span><span class="sxs-lookup"><span data-stu-id="08560-1488">An *anonymous_method_expression* is one of two ways of defining an anonymous function.</span></span> <span data-ttu-id="08560-1489">Tyto jsou dále popsány ve [výrazech anonymních funkcí](expressions.md#anonymous-function-expressions).</span><span class="sxs-lookup"><span data-stu-id="08560-1489">These are further described in [Anonymous function expressions](expressions.md#anonymous-function-expressions).</span></span>

## <a name="unary-operators"></a><span data-ttu-id="08560-1490">Unární operátory</span><span class="sxs-lookup"><span data-stu-id="08560-1490">Unary operators</span></span>

<span data-ttu-id="08560-1491">Operátory `?`, `+`, `-`, `!`, `~`, `++`, `--`, přetypování a `await` se nazývají unární operátory.</span><span class="sxs-lookup"><span data-stu-id="08560-1491">The `?`, `+`, `-`, `!`, `~`, `++`, `--`, cast, and `await` operators are called the unary operators.</span></span>

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

<span data-ttu-id="08560-1492">Pokud operand *unary_expression* má `dynamic`typ doby kompilace, je dynamicky svázán ([dynamická vazba](expressions.md#dynamic-binding)).</span><span class="sxs-lookup"><span data-stu-id="08560-1492">If the operand of a *unary_expression* has the compile-time type `dynamic`, it is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)).</span></span> <span data-ttu-id="08560-1493">V tomto případě je typ doby kompilace *unary_expression* `dynamic`a řešení popsané níže bude provedeno za běhu pomocí běhového typu operandu.</span><span class="sxs-lookup"><span data-stu-id="08560-1493">In this case the compile-time type of the *unary_expression* is `dynamic`, and the resolution described below will take place at run-time using the run-time type of the operand.</span></span>

### <a name="null-conditional-operator"></a><span data-ttu-id="08560-1494">Podmíněný operátor s hodnotou null</span><span class="sxs-lookup"><span data-stu-id="08560-1494">Null-conditional operator</span></span>

<span data-ttu-id="08560-1495">Podmíněný operátor s hodnotou null aplikuje seznam operací na operand pouze v případě, že tento operand je jiný než null.</span><span class="sxs-lookup"><span data-stu-id="08560-1495">The null-conditional operator applies a list of operations to its operand only if that operand is non-null.</span></span> <span data-ttu-id="08560-1496">V opačném případě je výsledek použití operátoru `null`.</span><span class="sxs-lookup"><span data-stu-id="08560-1496">Otherwise the result of applying the operator is `null`.</span></span>

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

<span data-ttu-id="08560-1497">Seznam operací může zahrnovat přístup členů a operace přístupu k prvkům (které mohou být samy o hodnotě null) a také vyvolání.</span><span class="sxs-lookup"><span data-stu-id="08560-1497">The list of operations can include member access and element access operations (which may themselves be null-conditional), as well as invocation.</span></span>

<span data-ttu-id="08560-1498">Například výraz `a.b?[0]?.c()` je *null_conditional_expression* s *primary_expression* `a.b` a *null_conditional_operations* `?[0]` (přístup k podmíněnému prvku s hodnotou null) `?.c` (přístup s hodnotou null) a `()` (vyvolání).</span><span class="sxs-lookup"><span data-stu-id="08560-1498">For example, the expression `a.b?[0]?.c()` is a *null_conditional_expression* with a *primary_expression* `a.b` and *null_conditional_operations* `?[0]` (null-conditional element access), `?.c` (null-conditional member access) and `()` (invocation).</span></span>

<span data-ttu-id="08560-1499">U *null_conditional_expression* `E` s *primary_expression* `P`nechte `E0` výraz získaný textovým odebráním počátečního `?` z každého *null_conditional_operations* `E`, který jednu z nich obsahuje.</span><span class="sxs-lookup"><span data-stu-id="08560-1499">For a *null_conditional_expression* `E` with a *primary_expression* `P`, let `E0` be the expression obtained by textually removing the leading `?` from each of the *null_conditional_operations* of `E` that have one.</span></span> <span data-ttu-id="08560-1500">V koncepčním `E0` je výraz, který se vyhodnotí, pokud žádná z kontrol null reprezentovaných `?`nenalezne `null`.</span><span class="sxs-lookup"><span data-stu-id="08560-1500">Conceptually, `E0` is the expression that will be evaluated if none of the null checks represented by the `?`s do find a `null`.</span></span>

<span data-ttu-id="08560-1501">Také `E1` být výraz získaný textovým odebráním počátečních `?` z pouze prvního *null_conditional_operations* v `E`.</span><span class="sxs-lookup"><span data-stu-id="08560-1501">Also, let `E1` be the expression obtained by textually removing the leading `?` from just the first of the *null_conditional_operations* in `E`.</span></span> <span data-ttu-id="08560-1502">To může vést k *primárnímu výrazu* (pokud existuje jen jeden `?`) nebo jinému *null_conditional_expression*.</span><span class="sxs-lookup"><span data-stu-id="08560-1502">This may lead to a *primary-expression* (if there was just one `?`) or to another *null_conditional_expression*.</span></span>

<span data-ttu-id="08560-1503">Například pokud `E` `a.b?[0]?.c()`výrazu, pak `E0` je `a.b[0].c()` výrazu a `E1` je výraz `a.b[0]?.c()`.</span><span class="sxs-lookup"><span data-stu-id="08560-1503">For example, if `E` is the expression `a.b?[0]?.c()`, then `E0` is the expression `a.b[0].c()` and `E1` is the expression `a.b[0]?.c()`.</span></span>

<span data-ttu-id="08560-1504">Pokud je `E0` klasifikován jako Nothing, pak `E` klasifikován jako Nothing.</span><span class="sxs-lookup"><span data-stu-id="08560-1504">If `E0` is classified as nothing, then `E` is classified as nothing.</span></span> <span data-ttu-id="08560-1505">V opačném případě je E klasifikován jako hodnota.</span><span class="sxs-lookup"><span data-stu-id="08560-1505">Otherwise E is classified as a value.</span></span>

<span data-ttu-id="08560-1506">`E0` a `E1` se používají k určení významu `E`:</span><span class="sxs-lookup"><span data-stu-id="08560-1506">`E0` and `E1` are used to determine the meaning of `E`:</span></span>

*  <span data-ttu-id="08560-1507">Pokud k `E` dojde jako *statement_expression* význam `E` je stejný jako příkaz</span><span class="sxs-lookup"><span data-stu-id="08560-1507">If `E` occurs as a *statement_expression* the meaning of `E` is the same as the statement</span></span>

   ```csharp
   if ((object)P != null) E1;
   ```

   <span data-ttu-id="08560-1508">s výjimkou, že P je vyhodnocována pouze jednou.</span><span class="sxs-lookup"><span data-stu-id="08560-1508">except that P is evaluated only once.</span></span>

*  <span data-ttu-id="08560-1509">V opačném případě, pokud je `E0` klasifikována jako Nothing, dojde k chybě při kompilaci.</span><span class="sxs-lookup"><span data-stu-id="08560-1509">Otherwise, if `E0` is classified as nothing a compile-time error occurs.</span></span>

*  <span data-ttu-id="08560-1510">Jinak nechejte `T0` typ `E0`.</span><span class="sxs-lookup"><span data-stu-id="08560-1510">Otherwise, let `T0` be the type of `E0`.</span></span>

   *  <span data-ttu-id="08560-1511">Pokud je `T0` parametr typu, který není známý jako odkazový typ nebo typ hodnoty, který neumožňuje hodnotu null, dojde k chybě při kompilaci.</span><span class="sxs-lookup"><span data-stu-id="08560-1511">If `T0` is a type parameter that is not known to be a reference type or a non-nullable value type, a compile-time error occurs.</span></span>

   *  <span data-ttu-id="08560-1512">Pokud `T0` typ hodnoty, která není null, pak je typ `E` `T0?`a význam `E` je stejný jako</span><span class="sxs-lookup"><span data-stu-id="08560-1512">If `T0` is a non-nullable value type, then the type of `E` is `T0?`, and the meaning of `E` is the same as</span></span>

      ```csharp
      ((object)P == null) ? (T0?)null : E1
      ```

      <span data-ttu-id="08560-1513">s výjimkou, že `P` vyhodnocována pouze jednou.</span><span class="sxs-lookup"><span data-stu-id="08560-1513">except that `P` is evaluated only once.</span></span>

   *  <span data-ttu-id="08560-1514">V opačném případě typ E je T0 a význam E je stejný jako</span><span class="sxs-lookup"><span data-stu-id="08560-1514">Otherwise the type of E is T0, and the meaning of E is the same as</span></span>

      ```csharp
      ((object)P == null) ? null : E1
      ```

      <span data-ttu-id="08560-1515">s výjimkou, že `P` vyhodnocována pouze jednou.</span><span class="sxs-lookup"><span data-stu-id="08560-1515">except that `P` is evaluated only once.</span></span>

<span data-ttu-id="08560-1516">Pokud je `E1` sám o sobě *null_conditional_expression*, pak se tato pravidla aplikují znovu a vnořování testů pro `null`, dokud nejsou žádné další `?`, a výraz byl zmenšen tak, aby byl `E0`na primárním výrazu.</span><span class="sxs-lookup"><span data-stu-id="08560-1516">If `E1` is itself a *null_conditional_expression*, then these rules are applied again, nesting the tests for `null` until there are no further `?`'s, and the expression has been reduced all the way down to the primary-expression `E0`.</span></span>

<span data-ttu-id="08560-1517">Například pokud `a.b?[0]?.c()` výraz nastane jako výraz příkazu, jako v příkazu:</span><span class="sxs-lookup"><span data-stu-id="08560-1517">For example, if the expression `a.b?[0]?.c()` occurs as a statement-expression, as in the statement:</span></span>
```csharp
a.b?[0]?.c();
```
<span data-ttu-id="08560-1518">jeho význam je stejný jako:</span><span class="sxs-lookup"><span data-stu-id="08560-1518">its meaning is equivalent to:</span></span>
```csharp
if (a.b != null) a.b[0]?.c();
```
<span data-ttu-id="08560-1519">který je opět stejný jako:</span><span class="sxs-lookup"><span data-stu-id="08560-1519">which again is equivalent to:</span></span>
```csharp
if (a.b != null) if (a.b[0] != null) a.b[0].c();
```
<span data-ttu-id="08560-1520">S výjimkou, že `a.b` a `a.b[0]` jsou vyhodnocovány pouze jednou.</span><span class="sxs-lookup"><span data-stu-id="08560-1520">Except that `a.b` and `a.b[0]` are evaluated only once.</span></span>

<span data-ttu-id="08560-1521">Pokud k tomu dojde v kontextu, ve kterém je použita jeho hodnota, jako v:</span><span class="sxs-lookup"><span data-stu-id="08560-1521">If it occurs in a context where its value is used, as in:</span></span>
```csharp
var x = a.b?[0]?.c();
```
<span data-ttu-id="08560-1522">a za předpokladu, že typ finálního vyvolání není typ hodnoty, který neumožňuje hodnotu null, jeho význam je ekvivalentní:</span><span class="sxs-lookup"><span data-stu-id="08560-1522">and assuming that the type of the final invocation is not a non-nullable value type, its meaning is equivalent to:</span></span>
```csharp
var x = (a.b == null) ? null : (a.b[0] == null) ? null : a.b[0].c();
```
<span data-ttu-id="08560-1523">s výjimkou, že `a.b` a `a.b[0]` jsou vyhodnocovány pouze jednou.</span><span class="sxs-lookup"><span data-stu-id="08560-1523">except that `a.b` and `a.b[0]` are evaluated only once.</span></span>

#### <a name="null-conditional-expressions-as-projection-initializers"></a><span data-ttu-id="08560-1524">Podmíněné výrazy s hodnotou null jako Inicializátory projekce</span><span class="sxs-lookup"><span data-stu-id="08560-1524">Null-conditional expressions as projection initializers</span></span>

<span data-ttu-id="08560-1525">Podmíněný výraz s hodnotou null je povolen pouze jako *member_declarator* v *anonymous_object_creation_expression* (výrazy pro[vytváření anonymních objektů](expressions.md#anonymous-object-creation-expressions)), pokud je ukončen s přístupem člena (volitelně null-Conditional).</span><span class="sxs-lookup"><span data-stu-id="08560-1525">A null-conditional expression is only allowed as a *member_declarator* in an *anonymous_object_creation_expression* ([Anonymous object creation expressions](expressions.md#anonymous-object-creation-expressions)) if it ends with an (optionally null-conditional) member access.</span></span> <span data-ttu-id="08560-1526">Gramaticky lze tento požadavek vyjádřit takto:</span><span class="sxs-lookup"><span data-stu-id="08560-1526">Grammatically, this requirement can be expressed as:</span></span>

```antlr
null_conditional_member_access
    : primary_expression null_conditional_operations? '?' '.' identifier type_argument_list?
    | primary_expression null_conditional_operations '.' identifier type_argument_list?
    ;
```

<span data-ttu-id="08560-1527">Toto je speciální případ gramatiky pro *null_conditional_expression* výše.</span><span class="sxs-lookup"><span data-stu-id="08560-1527">This is a special case of the grammar for *null_conditional_expression* above.</span></span> <span data-ttu-id="08560-1528">Výroba pro *member_declarator* [výrazy vytváření anonymních objektů](expressions.md#anonymous-object-creation-expressions) pak zahrnuje pouze *null_conditional_member_access*.</span><span class="sxs-lookup"><span data-stu-id="08560-1528">The production for *member_declarator* in [Anonymous object creation expressions](expressions.md#anonymous-object-creation-expressions) then includes only *null_conditional_member_access*.</span></span>

#### <a name="null-conditional-expressions-as-statement-expressions"></a><span data-ttu-id="08560-1529">Podmíněné výrazy s hodnotou null jako výrazy příkazu</span><span class="sxs-lookup"><span data-stu-id="08560-1529">Null-conditional expressions as statement expressions</span></span>

<span data-ttu-id="08560-1530">Podmíněný výraz s hodnotou null je povolen pouze jako *statement_expression* ([příkazy výrazu](statements.md#expression-statements)), pokud končí voláním.</span><span class="sxs-lookup"><span data-stu-id="08560-1530">A null-conditional expression is only allowed as a *statement_expression* ([Expression statements](statements.md#expression-statements)) if it ends with an invocation.</span></span> <span data-ttu-id="08560-1531">Gramaticky lze tento požadavek vyjádřit takto:</span><span class="sxs-lookup"><span data-stu-id="08560-1531">Grammatically, this requirement can be expressed as:</span></span>

```antlr
null_conditional_invocation_expression
    : primary_expression null_conditional_operations '(' argument_list? ')'
    ;
```

<span data-ttu-id="08560-1532">Toto je speciální případ gramatiky pro *null_conditional_expression* výše.</span><span class="sxs-lookup"><span data-stu-id="08560-1532">This is a special case of the grammar for *null_conditional_expression* above.</span></span> <span data-ttu-id="08560-1533">Výroba pro *statement_expression* v [příkazech výrazu](statements.md#expression-statements) pak obsahuje jenom *null_conditional_invocation_expression*.</span><span class="sxs-lookup"><span data-stu-id="08560-1533">The production for *statement_expression* in [Expression statements](statements.md#expression-statements) then includes only *null_conditional_invocation_expression*.</span></span>


### <a name="unary-plus-operator"></a><span data-ttu-id="08560-1534">Jednočlenný operátor plus</span><span class="sxs-lookup"><span data-stu-id="08560-1534">Unary plus operator</span></span>

<span data-ttu-id="08560-1535">Pro účely operace formuláře `+x`se pro výběr konkrétní implementace operátoru použije rozlišení přetížení unárního operátoru ([řešení přetížení unárního operátoru](expressions.md#unary-operator-overload-resolution)).</span><span class="sxs-lookup"><span data-stu-id="08560-1535">For an operation of the form `+x`, unary operator overload resolution ([Unary operator overload resolution](expressions.md#unary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="08560-1536">Operand je převeden na typ parametru vybraného operátoru a typ výsledku je návratový typ operátoru.</span><span class="sxs-lookup"><span data-stu-id="08560-1536">The operand is converted to the parameter type of the selected operator, and the type of the result is the return type of the operator.</span></span> <span data-ttu-id="08560-1537">Předdefinované unární operátory Plus jsou:</span><span class="sxs-lookup"><span data-stu-id="08560-1537">The predefined unary plus operators are:</span></span>

```csharp
int operator +(int x);
uint operator +(uint x);
long operator +(long x);
ulong operator +(ulong x);
float operator +(float x);
double operator +(double x);
decimal operator +(decimal x);
```

<span data-ttu-id="08560-1538">Pro každý z těchto operátorů je výsledkem jednoduše hodnota operandu.</span><span class="sxs-lookup"><span data-stu-id="08560-1538">For each of these operators, the result is simply the value of the operand.</span></span>

### <a name="unary-minus-operator"></a><span data-ttu-id="08560-1539">Unární operátor minus</span><span class="sxs-lookup"><span data-stu-id="08560-1539">Unary minus operator</span></span>

<span data-ttu-id="08560-1540">Pro účely operace formuláře `-x`se pro výběr konkrétní implementace operátoru použije rozlišení přetížení unárního operátoru ([řešení přetížení unárního operátoru](expressions.md#unary-operator-overload-resolution)).</span><span class="sxs-lookup"><span data-stu-id="08560-1540">For an operation of the form `-x`, unary operator overload resolution ([Unary operator overload resolution](expressions.md#unary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="08560-1541">Operand je převeden na typ parametru vybraného operátoru a typ výsledku je návratový typ operátoru.</span><span class="sxs-lookup"><span data-stu-id="08560-1541">The operand is converted to the parameter type of the selected operator, and the type of the result is the return type of the operator.</span></span> <span data-ttu-id="08560-1542">Předdefinované operátory negace jsou:</span><span class="sxs-lookup"><span data-stu-id="08560-1542">The predefined negation operators are:</span></span>

*  <span data-ttu-id="08560-1543">Negace typu Integer:</span><span class="sxs-lookup"><span data-stu-id="08560-1543">Integer negation:</span></span>

   ```csharp
   int operator -(int x);
   long operator -(long x);
   ```

   <span data-ttu-id="08560-1544">Výsledek je vypočítán odčítáním `x` od nuly.</span><span class="sxs-lookup"><span data-stu-id="08560-1544">The result is computed by subtracting `x` from zero.</span></span> <span data-ttu-id="08560-1545">Pokud je hodnota `x` nejmenší reprezentovatelné hodnoty typu operandu (-2 ^ 31 pro `int` nebo-2 ^ 63 pro `long`), pak se matematické negace `x` neprezentují v rámci typu operandu.</span><span class="sxs-lookup"><span data-stu-id="08560-1545">If the value of `x` is the smallest representable value of the operand type (-2^31 for `int` or -2^63 for `long`), then the mathematical negation of `x` is not representable within the operand type.</span></span> <span data-ttu-id="08560-1546">Pokud k tomu dojde v rámci `checked`ho kontextu, je vyvolána `System.OverflowException`; Pokud k tomu dojde v kontextu `unchecked`, je výsledkem hodnota operandu a přetečení není hlášeno.</span><span class="sxs-lookup"><span data-stu-id="08560-1546">If this occurs within a `checked` context, a `System.OverflowException` is thrown; if it occurs within an `unchecked` context, the result is the value of the operand and the overflow is not reported.</span></span>

   <span data-ttu-id="08560-1547">Pokud je Operand operátoru negace typu `uint`, je převeden na typ `long`a typ výsledku je `long`.</span><span class="sxs-lookup"><span data-stu-id="08560-1547">If the operand of the negation operator is of type `uint`, it is converted to type `long`, and the type of the result is `long`.</span></span> <span data-ttu-id="08560-1548">Výjimkou je pravidlo, které povoluje `int` hodnotu-2147483648 (-2 ^ 31), která má být zapsána jako desítkový celočíselný literál ([literály typu Integer](lexical-structure.md#integer-literals)).</span><span class="sxs-lookup"><span data-stu-id="08560-1548">An exception is the rule that permits the `int` value -2147483648 (-2^31) to be written as a decimal integer literal ([Integer literals](lexical-structure.md#integer-literals)).</span></span>

   <span data-ttu-id="08560-1549">Pokud je Operand operátoru negace typu `ulong`, dojde k chybě při kompilaci.</span><span class="sxs-lookup"><span data-stu-id="08560-1549">If the operand of the negation operator is of type `ulong`, a compile-time error occurs.</span></span> <span data-ttu-id="08560-1550">Výjimkou je pravidlo, které povoluje `long` Value-9223372036854775808 (-2 ^ 63) k zápisu jako desítkový celočíselný literál ([literály typu Integer](lexical-structure.md#integer-literals)).</span><span class="sxs-lookup"><span data-stu-id="08560-1550">An exception is the rule that permits the `long` value -9223372036854775808 (-2^63) to be written as a decimal integer literal ([Integer literals](lexical-structure.md#integer-literals)).</span></span>

*  <span data-ttu-id="08560-1551">Negace plovoucí desetinné čárky:</span><span class="sxs-lookup"><span data-stu-id="08560-1551">Floating-point negation:</span></span>

   ```csharp
   float operator -(float x);
   double operator -(double x);
   ```

   <span data-ttu-id="08560-1552">Výsledkem je hodnota `x` se znaménkem, které se obrátí.</span><span class="sxs-lookup"><span data-stu-id="08560-1552">The result is the value of `x` with its sign inverted.</span></span> <span data-ttu-id="08560-1553">Pokud `x` je NaN, výsledek je také NaN.</span><span class="sxs-lookup"><span data-stu-id="08560-1553">If `x` is NaN, the result is also NaN.</span></span>

*  <span data-ttu-id="08560-1554">Desetinné negace:</span><span class="sxs-lookup"><span data-stu-id="08560-1554">Decimal negation:</span></span>

   ```csharp
   decimal operator -(decimal x);
   ```

   <span data-ttu-id="08560-1555">Výsledek je vypočítán odčítáním `x` od nuly.</span><span class="sxs-lookup"><span data-stu-id="08560-1555">The result is computed by subtracting `x` from zero.</span></span> <span data-ttu-id="08560-1556">Desítková negace je ekvivalentní k použití unárního operátoru mínus typu `System.Decimal`.</span><span class="sxs-lookup"><span data-stu-id="08560-1556">Decimal negation is equivalent to using the unary minus operator of type `System.Decimal`.</span></span>

### <a name="logical-negation-operator"></a><span data-ttu-id="08560-1557">Logický operátor negace</span><span class="sxs-lookup"><span data-stu-id="08560-1557">Logical negation operator</span></span>

<span data-ttu-id="08560-1558">Pro účely operace formuláře `!x`se pro výběr konkrétní implementace operátoru použije rozlišení přetížení unárního operátoru ([řešení přetížení unárního operátoru](expressions.md#unary-operator-overload-resolution)).</span><span class="sxs-lookup"><span data-stu-id="08560-1558">For an operation of the form `!x`, unary operator overload resolution ([Unary operator overload resolution](expressions.md#unary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="08560-1559">Operand je převeden na typ parametru vybraného operátoru a typ výsledku je návratový typ operátoru.</span><span class="sxs-lookup"><span data-stu-id="08560-1559">The operand is converted to the parameter type of the selected operator, and the type of the result is the return type of the operator.</span></span> <span data-ttu-id="08560-1560">Existuje pouze jeden předdefinovaný logický operátor negace:</span><span class="sxs-lookup"><span data-stu-id="08560-1560">Only one predefined logical negation operator exists:</span></span>
```csharp
bool operator !(bool x);
```

<span data-ttu-id="08560-1561">Tento operátor vypočítá logickou negaci operandu: Pokud je operand `true`, výsledek je `false`.</span><span class="sxs-lookup"><span data-stu-id="08560-1561">This operator computes the logical negation of the operand: If the operand is `true`, the result is `false`.</span></span> <span data-ttu-id="08560-1562">Pokud je operand `false`, výsledek je `true`.</span><span class="sxs-lookup"><span data-stu-id="08560-1562">If the operand is `false`, the result is `true`.</span></span>

### <a name="bitwise-complement-operator"></a><span data-ttu-id="08560-1563">Operátor bitového doplňku</span><span class="sxs-lookup"><span data-stu-id="08560-1563">Bitwise complement operator</span></span>

<span data-ttu-id="08560-1564">Pro účely operace formuláře `~x`se pro výběr konkrétní implementace operátoru použije rozlišení přetížení unárního operátoru ([řešení přetížení unárního operátoru](expressions.md#unary-operator-overload-resolution)).</span><span class="sxs-lookup"><span data-stu-id="08560-1564">For an operation of the form `~x`, unary operator overload resolution ([Unary operator overload resolution](expressions.md#unary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="08560-1565">Operand je převeden na typ parametru vybraného operátoru a typ výsledku je návratový typ operátoru.</span><span class="sxs-lookup"><span data-stu-id="08560-1565">The operand is converted to the parameter type of the selected operator, and the type of the result is the return type of the operator.</span></span> <span data-ttu-id="08560-1566">Předdefinované bitové operátory doplňku jsou:</span><span class="sxs-lookup"><span data-stu-id="08560-1566">The predefined bitwise complement operators are:</span></span>
```csharp
int operator ~(int x);
uint operator ~(uint x);
long operator ~(long x);
ulong operator ~(ulong x);
```

<span data-ttu-id="08560-1567">Pro každý z těchto operátorů je výsledkem operace bitový doplněk `x`.</span><span class="sxs-lookup"><span data-stu-id="08560-1567">For each of these operators, the result of the operation is the bitwise complement of `x`.</span></span>

<span data-ttu-id="08560-1568">Každý typ výčtu `E` implicitně poskytuje následující bitový operátor doplňku:</span><span class="sxs-lookup"><span data-stu-id="08560-1568">Every enumeration type `E` implicitly provides the following bitwise complement operator:</span></span>

```csharp
E operator ~(E x);
```

<span data-ttu-id="08560-1569">Výsledek vyhodnocení `~x`, kde `x` je výraz typu výčtu `E` s podkladovým typem `U`, je přesně stejný jako vyhodnocení `(E)(~(U)x)`, s tím rozdílem, že převod na `E` je vždy proveden jako v kontextu `unchecked` ([kontrolované a nezaškrtnuté operátory](expressions.md#the-checked-and-unchecked-operators)).</span><span class="sxs-lookup"><span data-stu-id="08560-1569">The result of evaluating `~x`, where `x` is an expression of an enumeration type `E` with an underlying type `U`, is exactly the same as evaluating `(E)(~(U)x)`, except that the conversion to `E` is always performed as if in an `unchecked` context ([The checked and unchecked operators](expressions.md#the-checked-and-unchecked-operators)).</span></span>

### <a name="prefix-increment-and-decrement-operators"></a><span data-ttu-id="08560-1570">Operátory inkrementace a dekrementace předpony</span><span class="sxs-lookup"><span data-stu-id="08560-1570">Prefix increment and decrement operators</span></span>

```antlr
pre_increment_expression
    : '++' unary_expression
    ;

pre_decrement_expression
    : '--' unary_expression
    ;
```

<span data-ttu-id="08560-1571">Operandem operace zvýšení nebo snížení předpony musí být výraz klasifikovaný jako proměnná, přístup k vlastnosti nebo přístup indexeru.</span><span class="sxs-lookup"><span data-stu-id="08560-1571">The operand of a prefix increment or decrement operation must be an expression classified as a variable, a property access, or an indexer access.</span></span> <span data-ttu-id="08560-1572">Výsledkem operace je hodnota stejného typu jako operand.</span><span class="sxs-lookup"><span data-stu-id="08560-1572">The result of the operation is a value of the same type as the operand.</span></span>

<span data-ttu-id="08560-1573">Pokud je operandem operace zvýšení nebo snížení předpony vlastnost nebo přístup indexeru, musí mít vlastnost nebo indexer jak `get`, tak přistupující objekt `set`.</span><span class="sxs-lookup"><span data-stu-id="08560-1573">If the operand of a prefix increment or decrement operation is a property or indexer access, the property or indexer must have both a `get` and a `set` accessor.</span></span> <span data-ttu-id="08560-1574">Pokud se nejedná o tento případ, dojde k chybě při vazbě.</span><span class="sxs-lookup"><span data-stu-id="08560-1574">If this is not the case, a binding-time error occurs.</span></span>

<span data-ttu-id="08560-1575">Pro výběr implementace konkrétního operátoru je použito rozlišení přetížení unárního operátoru ([rozlišení přetížení unárního operátoru](expressions.md#unary-operator-overload-resolution)).</span><span class="sxs-lookup"><span data-stu-id="08560-1575">Unary operator overload resolution ([Unary operator overload resolution](expressions.md#unary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="08560-1576">Předdefinované `++` a `--` operátory existují pro následující typy: `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, `decimal`a jakýkoli typ výčtu.</span><span class="sxs-lookup"><span data-stu-id="08560-1576">Predefined `++` and `--` operators exist for the following types: `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, `decimal`, and any enum type.</span></span> <span data-ttu-id="08560-1577">Předdefinované operátory `++` vrátí hodnotu vytvořenou přidáním 1 k operandu a předdefinované `--` operátory vrátí hodnotu vytvořenou odečtením hodnoty 1 od operandu.</span><span class="sxs-lookup"><span data-stu-id="08560-1577">The predefined `++` operators return the value produced by adding 1 to the operand, and the predefined `--` operators return the value produced by subtracting 1 from the operand.</span></span> <span data-ttu-id="08560-1578">V kontextu `checked`, pokud je výsledek tohoto sčítání nebo odčítání mimo rozsah výsledného typu a typ výsledku je celočíselný typ nebo typ výčtu, je vyvolána `System.OverflowException`.</span><span class="sxs-lookup"><span data-stu-id="08560-1578">In a `checked` context, if the result of this addition or subtraction is outside the range of the result type and the result type is an integral type or enum type, a `System.OverflowException` is thrown.</span></span>

<span data-ttu-id="08560-1579">Zpracování operace zvýšení nebo snížení předpony formuláře `++x` nebo `--x` sestává z následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="08560-1579">The run-time processing of a prefix increment or decrement operation of the form `++x` or `--x` consists of the following steps:</span></span>

*   <span data-ttu-id="08560-1580">Pokud je `x` klasifikován jako proměnná:</span><span class="sxs-lookup"><span data-stu-id="08560-1580">If `x` is classified as a variable:</span></span>
    * <span data-ttu-id="08560-1581">`x` se vyhodnotí, aby se vytvořila proměnná.</span><span class="sxs-lookup"><span data-stu-id="08560-1581">`x` is evaluated to produce the variable.</span></span>
    * <span data-ttu-id="08560-1582">Vybraný operátor je vyvolán s hodnotou `x` jako argument.</span><span class="sxs-lookup"><span data-stu-id="08560-1582">The selected operator is invoked with the value of `x` as its argument.</span></span>
    * <span data-ttu-id="08560-1583">Hodnota vrácená operátorem je uložena v umístění uvedeném vyhodnocením `x`.</span><span class="sxs-lookup"><span data-stu-id="08560-1583">The value returned by the operator is stored in the location given by the evaluation of `x`.</span></span>
    * <span data-ttu-id="08560-1584">Hodnota vrácená operátorem se vrátí k výsledku operace.</span><span class="sxs-lookup"><span data-stu-id="08560-1584">The value returned by the operator becomes the result of the operation.</span></span>
*   <span data-ttu-id="08560-1585">Pokud je `x` klasifikován jako vlastnost nebo přístup indexeru:</span><span class="sxs-lookup"><span data-stu-id="08560-1585">If `x` is classified as a property or indexer access:</span></span>
    * <span data-ttu-id="08560-1586">Výraz instance (Pokud `x` není `static`) a seznam argumentů (Pokud je `x` přístup indexeru) přidružený k `x` jsou vyhodnocovány a výsledky se používají v následných `get` a `set` vyvolání přístupového objektu.</span><span class="sxs-lookup"><span data-stu-id="08560-1586">The instance expression (if `x` is not `static`) and the argument list (if `x` is an indexer access) associated with `x` are evaluated, and the results are used in the subsequent `get` and `set` accessor invocations.</span></span>
    * <span data-ttu-id="08560-1587">Je vyvolán přistupující objekt `get` `x`.</span><span class="sxs-lookup"><span data-stu-id="08560-1587">The `get` accessor of `x` is invoked.</span></span>
    * <span data-ttu-id="08560-1588">Vybraný operátor je vyvolán s hodnotou vrácenou objektem `get` přistupující jako jeho argument.</span><span class="sxs-lookup"><span data-stu-id="08560-1588">The selected operator is invoked with the value returned by the `get` accessor as its argument.</span></span>
    * <span data-ttu-id="08560-1589">Přistupující objekt `set` `x` je vyvolán s hodnotou vrácenou operátorem jako jeho argument `value`.</span><span class="sxs-lookup"><span data-stu-id="08560-1589">The `set` accessor of `x` is invoked with the value returned by the operator as its `value` argument.</span></span>
    * <span data-ttu-id="08560-1590">Hodnota vrácená operátorem se vrátí k výsledku operace.</span><span class="sxs-lookup"><span data-stu-id="08560-1590">The value returned by the operator becomes the result of the operation.</span></span>

<span data-ttu-id="08560-1591">Operátory `++` a `--` také podporují notaci přípon ([operátory přírůstku a snížení přípony](expressions.md#postfix-increment-and-decrement-operators)).</span><span class="sxs-lookup"><span data-stu-id="08560-1591">The `++` and `--` operators also support postfix notation ([Postfix increment and decrement operators](expressions.md#postfix-increment-and-decrement-operators)).</span></span> <span data-ttu-id="08560-1592">Výsledkem `x++` nebo `x--` je obvykle hodnota `x` před operací, zatímco výsledek `++x` nebo `--x` je hodnota `x` po operaci.</span><span class="sxs-lookup"><span data-stu-id="08560-1592">Typically, the result of `x++` or `x--` is the value of `x` before the operation, whereas the result of `++x` or `--x` is the value of `x` after the operation.</span></span> <span data-ttu-id="08560-1593">V obou případech má `x` stejnou hodnotu po operaci.</span><span class="sxs-lookup"><span data-stu-id="08560-1593">In either case, `x` itself has the same value after the operation.</span></span>

<span data-ttu-id="08560-1594">Implementaci `operator++` nebo `operator--` lze vyvolat pomocí přípony nebo zápisu předpon.</span><span class="sxs-lookup"><span data-stu-id="08560-1594">An `operator++` or `operator--` implementation can be invoked using either postfix or prefix notation.</span></span> <span data-ttu-id="08560-1595">Pro tyto dva zápisy není možné mít implementace samostatného operátoru.</span><span class="sxs-lookup"><span data-stu-id="08560-1595">It is not possible to have separate operator implementations for the two notations.</span></span>

### <a name="cast-expressions"></a><span data-ttu-id="08560-1596">Výrazy cast</span><span class="sxs-lookup"><span data-stu-id="08560-1596">Cast expressions</span></span>

<span data-ttu-id="08560-1597">*Cast_expression* slouží k explicitnímu převodu výrazu na daný typ.</span><span class="sxs-lookup"><span data-stu-id="08560-1597">A *cast_expression* is used to explicitly convert an expression to a given type.</span></span>

```antlr
cast_expression
    : '(' type ')' unary_expression
    ;
```

<span data-ttu-id="08560-1598">*Cast_expression* `(T)E`formuláře, kde `T` je *typ* a `E` je *unary_expression*, provede explicitní převod ([explicitní převody](conversions.md#explicit-conversions)) hodnoty `E` na typ `T`.</span><span class="sxs-lookup"><span data-stu-id="08560-1598">A *cast_expression* of the form `(T)E`, where `T` is a *type* and `E` is a *unary_expression*, performs an explicit conversion ([Explicit conversions](conversions.md#explicit-conversions)) of the value of `E` to type `T`.</span></span> <span data-ttu-id="08560-1599">Pokud neexistuje žádný explicitní převod z `E` na `T`, dojde k chybě při vazbě.</span><span class="sxs-lookup"><span data-stu-id="08560-1599">If no explicit conversion exists from `E` to `T`, a binding-time error occurs.</span></span> <span data-ttu-id="08560-1600">V opačném případě je výsledkem hodnota vytvořená explicitním převodem.</span><span class="sxs-lookup"><span data-stu-id="08560-1600">Otherwise, the result is the value produced by the explicit conversion.</span></span> <span data-ttu-id="08560-1601">Výsledek je vždy klasifikován jako hodnota, i když `E` označuje proměnnou.</span><span class="sxs-lookup"><span data-stu-id="08560-1601">The result is always classified as a value, even if `E` denotes a variable.</span></span>

<span data-ttu-id="08560-1602">Gramatika *cast_expression* vede ke konkrétním syntaktickým nejednoznačným.</span><span class="sxs-lookup"><span data-stu-id="08560-1602">The grammar for a *cast_expression* leads to certain syntactic ambiguities.</span></span> <span data-ttu-id="08560-1603">Například výraz `(x)-y` mohl být buď interpretován jako *cast_expression* (přetypování `-y` na typ `x`), nebo jako *additive_expression* v kombinaci s *parenthesized_expression* (který počítá hodnotu `x - y)`.</span><span class="sxs-lookup"><span data-stu-id="08560-1603">For example, the expression `(x)-y` could either be interpreted as a *cast_expression* (a cast of `-y` to type `x`) or as an *additive_expression* combined with a *parenthesized_expression* (which computes the value `x - y)`.</span></span>

<span data-ttu-id="08560-1604">Chcete-li vyřešit *cast_expression* nejednoznačnosti, existuje následující pravidlo: sekvence jednoho nebo více *tokenů*s ([prázdné místo](lexical-structure.md#white-space)) uzavřená v závorkách je považována za začátek *cast_expression* pouze v případě, že je splněna alespoň jedna z následujících hodnot:</span><span class="sxs-lookup"><span data-stu-id="08560-1604">To resolve *cast_expression* ambiguities, the following rule exists: A sequence of one or more *token*s ([White space](lexical-structure.md#white-space)) enclosed in parentheses is considered the start of a *cast_expression* only if at least one of the following are true:</span></span>

*  <span data-ttu-id="08560-1605">Pořadí tokenů je správná gramatika pro *typ*, ale ne pro *výraz*.</span><span class="sxs-lookup"><span data-stu-id="08560-1605">The sequence of tokens is correct grammar for a *type*, but not for an *expression*.</span></span>
*  <span data-ttu-id="08560-1606">Sekvence tokenů je správná pro *typ*a token hned za pravou závorkou je token "`~`", token "`!`", token "`(`", *identifikátor* ([sekvence znaků Unicode](lexical-structure.md#unicode-character-escape-sequences)), *literál* ([literály](lexical-structure.md#literals)), nebo jakékoli *klíčové slovo* ([klíčová slova](lexical-structure.md#keywords)) s výjimkou `as` a `is`.</span><span class="sxs-lookup"><span data-stu-id="08560-1606">The sequence of tokens is correct grammar for a *type*, and the token immediately following the closing parentheses is the token "`~`", the token "`!`", the token "`(`", an *identifier* ([Unicode character escape sequences](lexical-structure.md#unicode-character-escape-sequences)), a *literal* ([Literals](lexical-structure.md#literals)), or any *keyword* ([Keywords](lexical-structure.md#keywords)) except `as` and `is`.</span></span>

<span data-ttu-id="08560-1607">Termín "správná gramatika" výše znamená pouze to, že pořadí tokenů musí odpovídat konkrétní gramatické výrobě.</span><span class="sxs-lookup"><span data-stu-id="08560-1607">The term "correct grammar" above means only that the sequence of tokens must conform to the particular grammatical production.</span></span> <span data-ttu-id="08560-1608">Konkrétně nebere v úvahu skutečný význam jakýchkoli identifikátorů prvků.</span><span class="sxs-lookup"><span data-stu-id="08560-1608">It specifically does not consider the actual meaning of any constituent identifiers.</span></span> <span data-ttu-id="08560-1609">Například pokud `x` a `y` jsou identifikátory, pak `x.y` správné gramatiky pro typ, a to i v případě, že `x.y` ve skutečnosti neoznačuje typ.</span><span class="sxs-lookup"><span data-stu-id="08560-1609">For example, if `x` and `y` are identifiers, then `x.y` is correct grammar for a type, even if `x.y` doesn't actually denote a type.</span></span>

<span data-ttu-id="08560-1610">Následující pravidlo nejednoznačnosti sleduje, že pokud jsou `x` a `y` identifikátory, `(x)y`, `(x)(y)`a `(x)(-y)` *cast_expression s,* ale `(x)-y` není, i když `x` identifikuje typ.</span><span class="sxs-lookup"><span data-stu-id="08560-1610">From the disambiguation rule it follows that, if `x` and `y` are identifiers, `(x)y`, `(x)(y)`, and `(x)(-y)` are *cast_expression*s, but `(x)-y` is not, even if `x` identifies a type.</span></span> <span data-ttu-id="08560-1611">Pokud je však `x` klíčové slovo, které identifikuje předdefinovaný typ (například `int`), pak všechny čtyři formuláře jsou *cast_expression*s (protože takové klíčové slovo by nebylo pravděpodobně výrazem samotné).</span><span class="sxs-lookup"><span data-stu-id="08560-1611">However, if `x` is a keyword that identifies a predefined type (such as `int`), then all four forms are *cast_expression*s (because such a keyword could not possibly be an expression by itself).</span></span>

### <a name="await-expressions"></a><span data-ttu-id="08560-1612">Výrazy await</span><span class="sxs-lookup"><span data-stu-id="08560-1612">Await expressions</span></span>

<span data-ttu-id="08560-1613">Operátor await se používá k pozastavení vyhodnocení ohraničující asynchronní funkce, dokud není dokončena asynchronní operace reprezentované operandem.</span><span class="sxs-lookup"><span data-stu-id="08560-1613">The await operator is used to suspend evaluation of the enclosing async function until the asynchronous operation represented by the operand has completed.</span></span>

```antlr
await_expression
    : 'await' unary_expression
    ;
```

<span data-ttu-id="08560-1614">*Await_expression* je povolený jenom v těle asynchronní funkce ([iterátory](classes.md#iterators)).</span><span class="sxs-lookup"><span data-stu-id="08560-1614">An *await_expression* is only allowed in the body of an async function ([Iterators](classes.md#iterators)).</span></span> <span data-ttu-id="08560-1615">V rámci nejbližší ohraničující asynchronní funkce se *await_expression* nemusí nacházet na těchto místech:</span><span class="sxs-lookup"><span data-stu-id="08560-1615">Within the nearest enclosing async function, an *await_expression* may not occur in these places:</span></span>

*  <span data-ttu-id="08560-1616">Uvnitř vnořené (neasynchronní) anonymní funkce</span><span class="sxs-lookup"><span data-stu-id="08560-1616">Inside a nested (non-async) anonymous function</span></span>
*  <span data-ttu-id="08560-1617">Uvnitř bloku *lock_statement*</span><span class="sxs-lookup"><span data-stu-id="08560-1617">Inside the block of a *lock_statement*</span></span>
*  <span data-ttu-id="08560-1618">V nezabezpečeném kontextu</span><span class="sxs-lookup"><span data-stu-id="08560-1618">In an unsafe context</span></span>

<span data-ttu-id="08560-1619">Všimněte si, že *await_expression* nemůže nastat na většině míst v rámci *query_expression*, protože jsou syntakticky transformované na použití neasynchronních výrazů lambda.</span><span class="sxs-lookup"><span data-stu-id="08560-1619">Note that an *await_expression* cannot occur in most places within a *query_expression*, because those are syntactically transformed to use non-async lambda expressions.</span></span>

<span data-ttu-id="08560-1620">Uvnitř asynchronní funkce nelze `await` použít jako identifikátor.</span><span class="sxs-lookup"><span data-stu-id="08560-1620">Inside of an async function, `await` cannot be used as an identifier.</span></span> <span data-ttu-id="08560-1621">Z tohoto důvodu není k dispozici žádná syntaktická nejednoznačnost mezi výrazy await a různými výrazy zahrnujícími identifikátory.</span><span class="sxs-lookup"><span data-stu-id="08560-1621">There is therefore no syntactic ambiguity between await-expressions and various expressions involving identifiers.</span></span> <span data-ttu-id="08560-1622">Mimo asynchronní funkce `await` funguje jako běžný identifikátor.</span><span class="sxs-lookup"><span data-stu-id="08560-1622">Outside of async functions, `await` acts as a normal identifier.</span></span>

<span data-ttu-id="08560-1623">Operandem *await_expression* se říká ***úkol***.</span><span class="sxs-lookup"><span data-stu-id="08560-1623">The operand of an *await_expression* is called the ***task***.</span></span> <span data-ttu-id="08560-1624">Představuje asynchronní operaci, která může nebo nemusí být dokončena v okamžiku vyhodnocení *await_expression* .</span><span class="sxs-lookup"><span data-stu-id="08560-1624">It represents an asynchronous operation that may or may not be complete at the time the *await_expression* is evaluated.</span></span> <span data-ttu-id="08560-1625">Účelem operátoru await je pozastavení provádění nadřazené asynchronní funkce, dokud není dokončen očekávaný úkol, a pak získá výsledek.</span><span class="sxs-lookup"><span data-stu-id="08560-1625">The purpose of the await operator is to suspend execution of the enclosing async function until the awaited task is complete, and then obtain its outcome.</span></span>

#### <a name="awaitable-expressions"></a><span data-ttu-id="08560-1626">Výrazy await</span><span class="sxs-lookup"><span data-stu-id="08560-1626">Awaitable expressions</span></span>

<span data-ttu-id="08560-1627">Je nutné, aby úkol výrazu await byl možné ***očekávat***.</span><span class="sxs-lookup"><span data-stu-id="08560-1627">The task of an await expression is required to be ***awaitable***.</span></span> <span data-ttu-id="08560-1628">Výraz `t` je await, pokud je k dispozici jeden z následujících:</span><span class="sxs-lookup"><span data-stu-id="08560-1628">An expression `t` is awaitable if one of the following holds:</span></span>

*  <span data-ttu-id="08560-1629">`t` je typ doby kompilace `dynamic`</span><span class="sxs-lookup"><span data-stu-id="08560-1629">`t` is of compile time type `dynamic`</span></span>
*  <span data-ttu-id="08560-1630">`t` má přístupnou metodu instance nebo rozšíření nazvanou `GetAwaiter` bez parametrů a žádné parametry typu a návratový typ `A`, pro který všechna následující blokování:</span><span class="sxs-lookup"><span data-stu-id="08560-1630">`t` has an accessible instance or extension method called `GetAwaiter` with no parameters and no type parameters, and a return type `A` for which all of the following hold:</span></span>
   * <span data-ttu-id="08560-1631">`A` implementuje rozhraní `System.Runtime.CompilerServices.INotifyCompletion` (dále označované jako `INotifyCompletion` pro zkrácení).</span><span class="sxs-lookup"><span data-stu-id="08560-1631">`A` implements the interface `System.Runtime.CompilerServices.INotifyCompletion` (hereafter known as `INotifyCompletion` for brevity)</span></span>
   * <span data-ttu-id="08560-1632">`A` má přístupnou vlastnost instance, která je `IsCompleted` typu `bool`</span><span class="sxs-lookup"><span data-stu-id="08560-1632">`A` has an accessible, readable instance property `IsCompleted` of type `bool`</span></span>
   * <span data-ttu-id="08560-1633">`A` má přístupnou metodu instance `GetResult` bez parametrů a žádné parametry typu.</span><span class="sxs-lookup"><span data-stu-id="08560-1633">`A` has an accessible instance method `GetResult` with no parameters and no type parameters</span></span>

<span data-ttu-id="08560-1634">Účelem metody `GetAwaiter` je získat pro úkol ***operátor await*** .</span><span class="sxs-lookup"><span data-stu-id="08560-1634">The purpose of the `GetAwaiter` method is to obtain an ***awaiter*** for the task.</span></span> <span data-ttu-id="08560-1635">Typ `A` se nazývá ***typ await*** pro výraz await.</span><span class="sxs-lookup"><span data-stu-id="08560-1635">The type `A` is called the ***awaiter type*** for the await expression.</span></span>

<span data-ttu-id="08560-1636">Účelem vlastnosti `IsCompleted` je určit, zda je úloha již dokončena.</span><span class="sxs-lookup"><span data-stu-id="08560-1636">The purpose of the `IsCompleted` property is to determine if the task is already complete.</span></span> <span data-ttu-id="08560-1637">Pokud ano, není nutné pozastavit vyhodnocení.</span><span class="sxs-lookup"><span data-stu-id="08560-1637">If so, there is no need to suspend evaluation.</span></span>

<span data-ttu-id="08560-1638">Účelem metody `INotifyCompletion.OnCompleted` je zaregistrování "pokračování" na úlohu; například delegát (typu `System.Action`), který bude vyvolán po dokončení úkolu.</span><span class="sxs-lookup"><span data-stu-id="08560-1638">The purpose of the `INotifyCompletion.OnCompleted` method is to sign up a "continuation" to the task; i.e. a delegate (of type `System.Action`) that will be invoked once the task is complete.</span></span>

<span data-ttu-id="08560-1639">Účelem metody `GetResult` je získat výsledek úkolu po jeho dokončení.</span><span class="sxs-lookup"><span data-stu-id="08560-1639">The purpose of the `GetResult` method is to obtain the outcome of the task once it is complete.</span></span> <span data-ttu-id="08560-1640">Tento výsledek může být úspěšný, možná s výslednou hodnotou, nebo může být výjimka, která je vyvolána metodou `GetResult`.</span><span class="sxs-lookup"><span data-stu-id="08560-1640">This outcome may be successful completion, possibly with a result value, or it may be an exception which is thrown by the `GetResult` method.</span></span>

#### <a name="classification-of-await-expressions"></a><span data-ttu-id="08560-1641">Klasifikace výrazů await</span><span class="sxs-lookup"><span data-stu-id="08560-1641">Classification of await expressions</span></span>

<span data-ttu-id="08560-1642">Výraz `await t` je klasifikován stejným způsobem jako `(t).GetAwaiter().GetResult()`výrazu.</span><span class="sxs-lookup"><span data-stu-id="08560-1642">The expression `await t` is classified the same way as the expression `(t).GetAwaiter().GetResult()`.</span></span> <span data-ttu-id="08560-1643">Proto pokud je návratový typ `GetResult` `void`, *await_expression* je klasifikován jako Nothing.</span><span class="sxs-lookup"><span data-stu-id="08560-1643">Thus, if the return type of `GetResult` is `void`, the *await_expression* is classified as nothing.</span></span> <span data-ttu-id="08560-1644">Pokud má návratový typ, který není typu void, `T`*await_expression* klasifikován jako hodnota typu `T`.</span><span class="sxs-lookup"><span data-stu-id="08560-1644">If it has a non-void return type `T`, the *await_expression* is classified as a value of type `T`.</span></span>

#### <a name="runtime-evaluation-of-await-expressions"></a><span data-ttu-id="08560-1645">Vyhodnocení za běhu pro výrazy await</span><span class="sxs-lookup"><span data-stu-id="08560-1645">Runtime evaluation of await expressions</span></span>

<span data-ttu-id="08560-1646">Za běhu se výraz `await t` vyhodnotí takto:</span><span class="sxs-lookup"><span data-stu-id="08560-1646">At runtime, the expression `await t` is evaluated as follows:</span></span>

*  <span data-ttu-id="08560-1647">Operátor await `a` se získá vyhodnocením `(t).GetAwaiter()`výrazu.</span><span class="sxs-lookup"><span data-stu-id="08560-1647">An awaiter `a` is obtained by evaluating the expression `(t).GetAwaiter()`.</span></span>
*  <span data-ttu-id="08560-1648">`bool` `b` se získá vyhodnocením `(a).IsCompleted`výrazu.</span><span class="sxs-lookup"><span data-stu-id="08560-1648">A `bool` `b` is obtained by evaluating the expression `(a).IsCompleted`.</span></span>
*  <span data-ttu-id="08560-1649">Pokud je `b` `false` pak bude vyhodnocování záviset na tom, zda `a` implementuje rozhraní `System.Runtime.CompilerServices.ICriticalNotifyCompletion` (dále označované jako `ICriticalNotifyCompletion` pro zkrácení).</span><span class="sxs-lookup"><span data-stu-id="08560-1649">If `b` is `false` then evaluation depends on whether `a` implements the interface `System.Runtime.CompilerServices.ICriticalNotifyCompletion` (hereafter known as `ICriticalNotifyCompletion` for brevity).</span></span> <span data-ttu-id="08560-1650">Tato kontroler se provádí v době vytváření vazby. To znamená za běhu, pokud `a` má typ doby kompilace `dynamic`a v čase kompilace v opačném případě.</span><span class="sxs-lookup"><span data-stu-id="08560-1650">This check is done at binding time; i.e. at runtime if `a` has the compile time type `dynamic`, and at compile time otherwise.</span></span> <span data-ttu-id="08560-1651">Nechejte `r` poznamenat delegáta pokračování ([iterátory](classes.md#iterators)):</span><span class="sxs-lookup"><span data-stu-id="08560-1651">Let `r` denote the resumption delegate ([Iterators](classes.md#iterators)):</span></span>
    * <span data-ttu-id="08560-1652">Pokud `a` neimplementuje `ICriticalNotifyCompletion`, vyhodnotí se výraz `(a as (INotifyCompletion)).OnCompleted(r)`.</span><span class="sxs-lookup"><span data-stu-id="08560-1652">If `a` does not implement `ICriticalNotifyCompletion`, then the expression `(a as (INotifyCompletion)).OnCompleted(r)` is evaluated.</span></span>
    * <span data-ttu-id="08560-1653">Pokud `a` implementuje `ICriticalNotifyCompletion`, vyhodnotí se výraz `(a as (ICriticalNotifyCompletion)).UnsafeOnCompleted(r)`.</span><span class="sxs-lookup"><span data-stu-id="08560-1653">If `a` does implement `ICriticalNotifyCompletion`, then the expression `(a as (ICriticalNotifyCompletion)).UnsafeOnCompleted(r)` is evaluated.</span></span>
    * <span data-ttu-id="08560-1654">Vyhodnocení je pak pozastaveno a řízení je vráceno aktuálnímu volajícímu asynchronní funkce.</span><span class="sxs-lookup"><span data-stu-id="08560-1654">Evaluation is then suspended, and control is returned to the current caller of the async function.</span></span>
*  <span data-ttu-id="08560-1655">Výraz `(a).GetResult()` je vyhodnocený hned po (Pokud `b` `true`) nebo na pozdějším vyvolání delegáta pokračování (Pokud `b` `false`).</span><span class="sxs-lookup"><span data-stu-id="08560-1655">Either immediately after (if `b` was `true`), or upon later invocation of the resumption delegate (if `b` was `false`), the expression `(a).GetResult()` is evaluated.</span></span> <span data-ttu-id="08560-1656">Pokud vrátí hodnotu, je tato hodnota výsledkem *await_expression*.</span><span class="sxs-lookup"><span data-stu-id="08560-1656">If it returns a value, that value is the result of the *await_expression*.</span></span> <span data-ttu-id="08560-1657">V opačném případě je výsledek Nothing.</span><span class="sxs-lookup"><span data-stu-id="08560-1657">Otherwise the result is nothing.</span></span>

<span data-ttu-id="08560-1658">Implementace volání metody rozhraní `INotifyCompletion.OnCompleted` a `ICriticalNotifyCompletion.UnsafeOnCompleted` by měla způsobit, že delegáta `r` být vyvolána nejvíce jednou.</span><span class="sxs-lookup"><span data-stu-id="08560-1658">An awaiter's implementation of the interface methods `INotifyCompletion.OnCompleted` and `ICriticalNotifyCompletion.UnsafeOnCompleted` should cause the delegate `r` to be invoked at most once.</span></span> <span data-ttu-id="08560-1659">V opačném případě chování ohraničující asynchronní funkce není definováno.</span><span class="sxs-lookup"><span data-stu-id="08560-1659">Otherwise, the behavior of the enclosing async function is undefined.</span></span>

## <a name="arithmetic-operators"></a><span data-ttu-id="08560-1660">Aritmetické operátory</span><span class="sxs-lookup"><span data-stu-id="08560-1660">Arithmetic operators</span></span>

<span data-ttu-id="08560-1661">Operátory `*`, `/`, `%`, `+`a `-` se nazývají aritmetické operátory.</span><span class="sxs-lookup"><span data-stu-id="08560-1661">The `*`, `/`, `%`, `+`, and `-` operators are called the arithmetic operators.</span></span>

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

<span data-ttu-id="08560-1662">Pokud má operand aritmetického operátoru `dynamic`typ doby kompilace, pak je výraz dynamicky svázán ([dynamická vazba](expressions.md#dynamic-binding)).</span><span class="sxs-lookup"><span data-stu-id="08560-1662">If an operand of an arithmetic operator has the compile-time type `dynamic`, then the expression is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)).</span></span> <span data-ttu-id="08560-1663">V tomto případě je typ výrazu kompilace `dynamic`a řešení popsané níže bude provedeno za běhu s použitím běhového typu u operandů, které mají typ doby kompilace `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="08560-1663">In this case the compile-time type of the expression is `dynamic`, and the resolution described below will take place at run-time using the run-time type of those operands that have the compile-time type `dynamic`.</span></span>

### <a name="multiplication-operator"></a><span data-ttu-id="08560-1664">Operátor násobení</span><span class="sxs-lookup"><span data-stu-id="08560-1664">Multiplication operator</span></span>

<span data-ttu-id="08560-1665">Pro vybírání implementace konkrétního operátora se použije pro operaci `x * y`formuláře, řešení přetížení binárního operátoru ([rozlišení přetížení binárního operátoru](expressions.md#binary-operator-overload-resolution)).</span><span class="sxs-lookup"><span data-stu-id="08560-1665">For an operation of the form `x * y`, binary operator overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="08560-1666">Operandy jsou převedeny na typy parametrů vybraného operátoru a typ výsledku je návratový typ operátoru.</span><span class="sxs-lookup"><span data-stu-id="08560-1666">The operands are converted to the parameter types of the selected operator, and the type of the result is the return type of the operator.</span></span>

<span data-ttu-id="08560-1667">Předdefinované operátory násobení jsou uvedeny níže.</span><span class="sxs-lookup"><span data-stu-id="08560-1667">The predefined multiplication operators are listed below.</span></span> <span data-ttu-id="08560-1668">Všechny operátory počítají produkt `x` a `y`.</span><span class="sxs-lookup"><span data-stu-id="08560-1668">The operators all compute the product of `x` and `y`.</span></span>

*  <span data-ttu-id="08560-1669">Násobení celočíselného čísla:</span><span class="sxs-lookup"><span data-stu-id="08560-1669">Integer multiplication:</span></span>

   ```csharp
   int operator *(int x, int y);
   uint operator *(uint x, uint y);
   long operator *(long x, long y);
   ulong operator *(ulong x, ulong y);
   ```

   <span data-ttu-id="08560-1670">Pokud je produkt v kontextu `checked` mimo rozsah výsledného typu, je vyvolána `System.OverflowException`.</span><span class="sxs-lookup"><span data-stu-id="08560-1670">In a `checked` context, if the product is outside the range of the result type, a `System.OverflowException` is thrown.</span></span> <span data-ttu-id="08560-1671">V kontextu `unchecked` nejsou přetečení hlášeny a jakékoli významné bity vysokého řádu mimo rozsah výsledného typu jsou zahozeny.</span><span class="sxs-lookup"><span data-stu-id="08560-1671">In an `unchecked` context, overflows are not reported and any significant high-order bits outside the range of the result type are discarded.</span></span>


*  <span data-ttu-id="08560-1672">Násobení plovoucí desetinné čárky:</span><span class="sxs-lookup"><span data-stu-id="08560-1672">Floating-point multiplication:</span></span>

   ```csharp
   float operator *(float x, float y);
   double operator *(double x, double y);
   ```

   <span data-ttu-id="08560-1673">Produkt se vypočítává podle pravidel aritmetické operace IEEE 754.</span><span class="sxs-lookup"><span data-stu-id="08560-1673">The product is computed according to the rules of IEEE 754 arithmetic.</span></span> <span data-ttu-id="08560-1674">V následující tabulce jsou uvedeny výsledky všech možných kombinací nenulových konečných hodnot, nul, nekonečno a NaN.</span><span class="sxs-lookup"><span data-stu-id="08560-1674">The following table lists the results of all possible combinations of nonzero finite values, zeros, infinities, and NaN's.</span></span> <span data-ttu-id="08560-1675">V tabulce `x` a `y` jsou kladné konečné hodnoty.</span><span class="sxs-lookup"><span data-stu-id="08560-1675">In the table, `x` and `y` are positive finite values.</span></span> <span data-ttu-id="08560-1676">`z` je výsledkem `x * y`.</span><span class="sxs-lookup"><span data-stu-id="08560-1676">`z` is the result of `x * y`.</span></span> <span data-ttu-id="08560-1677">Pokud je výsledek pro cílový typ příliš velký, `z` je nekonečno.</span><span class="sxs-lookup"><span data-stu-id="08560-1677">If the result is too large for the destination type, `z` is infinity.</span></span> <span data-ttu-id="08560-1678">Pokud je výsledek pro cílový typ příliš malý, `z` je nula.</span><span class="sxs-lookup"><span data-stu-id="08560-1678">If the result is too small for the destination type, `z` is zero.</span></span>

   |      |      |      |     |     |      |      |     |
   |:----:|-----:|:----:|:---:|:---:|:----:|:----:|:----|
   |      | <span data-ttu-id="08560-1679">\+ y</span><span class="sxs-lookup"><span data-stu-id="08560-1679">+y</span></span>   | <span data-ttu-id="08560-1680">-y</span><span class="sxs-lookup"><span data-stu-id="08560-1680">-y</span></span>   | <span data-ttu-id="08560-1681">+0</span><span class="sxs-lookup"><span data-stu-id="08560-1681">+0</span></span>  | <span data-ttu-id="08560-1682">-0</span><span class="sxs-lookup"><span data-stu-id="08560-1682">-0</span></span>  | <span data-ttu-id="08560-1683">\+ INF</span><span class="sxs-lookup"><span data-stu-id="08560-1683">+inf</span></span> | <span data-ttu-id="08560-1684">– INF</span><span class="sxs-lookup"><span data-stu-id="08560-1684">-inf</span></span> | <span data-ttu-id="08560-1685">NaN</span><span class="sxs-lookup"><span data-stu-id="08560-1685">NaN</span></span> | 
   | <span data-ttu-id="08560-1686">+x</span><span class="sxs-lookup"><span data-stu-id="08560-1686">+x</span></span>   | <span data-ttu-id="08560-1687">+z</span><span class="sxs-lookup"><span data-stu-id="08560-1687">+z</span></span>   | <span data-ttu-id="08560-1688">-z</span><span class="sxs-lookup"><span data-stu-id="08560-1688">-z</span></span>   | <span data-ttu-id="08560-1689">+0</span><span class="sxs-lookup"><span data-stu-id="08560-1689">+0</span></span>  | <span data-ttu-id="08560-1690">-0</span><span class="sxs-lookup"><span data-stu-id="08560-1690">-0</span></span>  | <span data-ttu-id="08560-1691">\+ INF</span><span class="sxs-lookup"><span data-stu-id="08560-1691">+inf</span></span> | <span data-ttu-id="08560-1692">– INF</span><span class="sxs-lookup"><span data-stu-id="08560-1692">-inf</span></span> | <span data-ttu-id="08560-1693">NaN</span><span class="sxs-lookup"><span data-stu-id="08560-1693">NaN</span></span> | 
   | <span data-ttu-id="08560-1694">-x</span><span class="sxs-lookup"><span data-stu-id="08560-1694">-x</span></span>   | <span data-ttu-id="08560-1695">-z</span><span class="sxs-lookup"><span data-stu-id="08560-1695">-z</span></span>   | <span data-ttu-id="08560-1696">+z</span><span class="sxs-lookup"><span data-stu-id="08560-1696">+z</span></span>   | <span data-ttu-id="08560-1697">-0</span><span class="sxs-lookup"><span data-stu-id="08560-1697">-0</span></span>  | <span data-ttu-id="08560-1698">+0</span><span class="sxs-lookup"><span data-stu-id="08560-1698">+0</span></span>  | <span data-ttu-id="08560-1699">– INF</span><span class="sxs-lookup"><span data-stu-id="08560-1699">-inf</span></span> | <span data-ttu-id="08560-1700">\+ INF</span><span class="sxs-lookup"><span data-stu-id="08560-1700">+inf</span></span> | <span data-ttu-id="08560-1701">NaN</span><span class="sxs-lookup"><span data-stu-id="08560-1701">NaN</span></span> | 
   | <span data-ttu-id="08560-1702">+0</span><span class="sxs-lookup"><span data-stu-id="08560-1702">+0</span></span>   | <span data-ttu-id="08560-1703">+0</span><span class="sxs-lookup"><span data-stu-id="08560-1703">+0</span></span>   | <span data-ttu-id="08560-1704">-0</span><span class="sxs-lookup"><span data-stu-id="08560-1704">-0</span></span>   | <span data-ttu-id="08560-1705">+0</span><span class="sxs-lookup"><span data-stu-id="08560-1705">+0</span></span>  | <span data-ttu-id="08560-1706">-0</span><span class="sxs-lookup"><span data-stu-id="08560-1706">-0</span></span>  | <span data-ttu-id="08560-1707">NaN</span><span class="sxs-lookup"><span data-stu-id="08560-1707">NaN</span></span>  | <span data-ttu-id="08560-1708">NaN</span><span class="sxs-lookup"><span data-stu-id="08560-1708">NaN</span></span>  | <span data-ttu-id="08560-1709">NaN</span><span class="sxs-lookup"><span data-stu-id="08560-1709">NaN</span></span> | 
   | <span data-ttu-id="08560-1710">-0</span><span class="sxs-lookup"><span data-stu-id="08560-1710">-0</span></span>   | <span data-ttu-id="08560-1711">-0</span><span class="sxs-lookup"><span data-stu-id="08560-1711">-0</span></span>   | <span data-ttu-id="08560-1712">+0</span><span class="sxs-lookup"><span data-stu-id="08560-1712">+0</span></span>   | <span data-ttu-id="08560-1713">-0</span><span class="sxs-lookup"><span data-stu-id="08560-1713">-0</span></span>  | <span data-ttu-id="08560-1714">+0</span><span class="sxs-lookup"><span data-stu-id="08560-1714">+0</span></span>  | <span data-ttu-id="08560-1715">NaN</span><span class="sxs-lookup"><span data-stu-id="08560-1715">NaN</span></span>  | <span data-ttu-id="08560-1716">NaN</span><span class="sxs-lookup"><span data-stu-id="08560-1716">NaN</span></span>  | <span data-ttu-id="08560-1717">NaN</span><span class="sxs-lookup"><span data-stu-id="08560-1717">NaN</span></span> | 
   | <span data-ttu-id="08560-1718">\+ INF</span><span class="sxs-lookup"><span data-stu-id="08560-1718">+inf</span></span> | <span data-ttu-id="08560-1719">\+ INF</span><span class="sxs-lookup"><span data-stu-id="08560-1719">+inf</span></span> | <span data-ttu-id="08560-1720">– INF</span><span class="sxs-lookup"><span data-stu-id="08560-1720">-inf</span></span> | <span data-ttu-id="08560-1721">NaN</span><span class="sxs-lookup"><span data-stu-id="08560-1721">NaN</span></span> | <span data-ttu-id="08560-1722">NaN</span><span class="sxs-lookup"><span data-stu-id="08560-1722">NaN</span></span> | <span data-ttu-id="08560-1723">\+ INF</span><span class="sxs-lookup"><span data-stu-id="08560-1723">+inf</span></span> | <span data-ttu-id="08560-1724">– INF</span><span class="sxs-lookup"><span data-stu-id="08560-1724">-inf</span></span> | <span data-ttu-id="08560-1725">NaN</span><span class="sxs-lookup"><span data-stu-id="08560-1725">NaN</span></span> | 
   | <span data-ttu-id="08560-1726">– INF</span><span class="sxs-lookup"><span data-stu-id="08560-1726">-inf</span></span> | <span data-ttu-id="08560-1727">– INF</span><span class="sxs-lookup"><span data-stu-id="08560-1727">-inf</span></span> | <span data-ttu-id="08560-1728">\+ INF</span><span class="sxs-lookup"><span data-stu-id="08560-1728">+inf</span></span> | <span data-ttu-id="08560-1729">NaN</span><span class="sxs-lookup"><span data-stu-id="08560-1729">NaN</span></span> | <span data-ttu-id="08560-1730">NaN</span><span class="sxs-lookup"><span data-stu-id="08560-1730">NaN</span></span> | <span data-ttu-id="08560-1731">– INF</span><span class="sxs-lookup"><span data-stu-id="08560-1731">-inf</span></span> | <span data-ttu-id="08560-1732">\+ INF</span><span class="sxs-lookup"><span data-stu-id="08560-1732">+inf</span></span> | <span data-ttu-id="08560-1733">NaN</span><span class="sxs-lookup"><span data-stu-id="08560-1733">NaN</span></span> | 
   | <span data-ttu-id="08560-1734">NaN</span><span class="sxs-lookup"><span data-stu-id="08560-1734">NaN</span></span>  | <span data-ttu-id="08560-1735">NaN</span><span class="sxs-lookup"><span data-stu-id="08560-1735">NaN</span></span>  | <span data-ttu-id="08560-1736">NaN</span><span class="sxs-lookup"><span data-stu-id="08560-1736">NaN</span></span>  | <span data-ttu-id="08560-1737">NaN</span><span class="sxs-lookup"><span data-stu-id="08560-1737">NaN</span></span> | <span data-ttu-id="08560-1738">NaN</span><span class="sxs-lookup"><span data-stu-id="08560-1738">NaN</span></span> | <span data-ttu-id="08560-1739">NaN</span><span class="sxs-lookup"><span data-stu-id="08560-1739">NaN</span></span>  | <span data-ttu-id="08560-1740">NaN</span><span class="sxs-lookup"><span data-stu-id="08560-1740">NaN</span></span>  | <span data-ttu-id="08560-1741">NaN</span><span class="sxs-lookup"><span data-stu-id="08560-1741">NaN</span></span> | 

*  <span data-ttu-id="08560-1742">Násobení desítkových čísel:</span><span class="sxs-lookup"><span data-stu-id="08560-1742">Decimal multiplication:</span></span>

   ```csharp
   decimal operator *(decimal x, decimal y);
   ```

   <span data-ttu-id="08560-1743">Pokud je výsledná hodnota příliš velká pro reprezentaci ve formátu `decimal`, je vyvolána `System.OverflowException`.</span><span class="sxs-lookup"><span data-stu-id="08560-1743">If the resulting value is too large to represent in the `decimal` format, a `System.OverflowException` is thrown.</span></span> <span data-ttu-id="08560-1744">Pokud je výsledná hodnota příliš malá, aby byla reprezentována ve formátu `decimal`, je výsledek nula.</span><span class="sxs-lookup"><span data-stu-id="08560-1744">If the result value is too small to represent in the `decimal` format, the result is zero.</span></span> <span data-ttu-id="08560-1745">Měřítko výsledku, před jakýmkoli zaokrouhlením, je součtem stupnice dvou operandů.</span><span class="sxs-lookup"><span data-stu-id="08560-1745">The scale of the result, before any rounding, is the sum of the scales of the two operands.</span></span>

   <span data-ttu-id="08560-1746">Násobení v desítkovém formátu je ekvivalentem použití operátoru násobení typu `System.Decimal`.</span><span class="sxs-lookup"><span data-stu-id="08560-1746">Decimal multiplication is equivalent to using the multiplication operator of type `System.Decimal`.</span></span>


### <a name="division-operator"></a><span data-ttu-id="08560-1747">Operátor dělení</span><span class="sxs-lookup"><span data-stu-id="08560-1747">Division operator</span></span>

<span data-ttu-id="08560-1748">Pro vybírání implementace konkrétního operátora se použije pro operaci `x / y`formuláře, řešení přetížení binárního operátoru ([rozlišení přetížení binárního operátoru](expressions.md#binary-operator-overload-resolution)).</span><span class="sxs-lookup"><span data-stu-id="08560-1748">For an operation of the form `x / y`, binary operator overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="08560-1749">Operandy jsou převedeny na typy parametrů vybraného operátoru a typ výsledku je návratový typ operátoru.</span><span class="sxs-lookup"><span data-stu-id="08560-1749">The operands are converted to the parameter types of the selected operator, and the type of the result is the return type of the operator.</span></span>

<span data-ttu-id="08560-1750">Předdefinované operátory dělení jsou uvedeny níže.</span><span class="sxs-lookup"><span data-stu-id="08560-1750">The predefined division operators are listed below.</span></span> <span data-ttu-id="08560-1751">Všechny operátory počítají podíl `x` a `y`.</span><span class="sxs-lookup"><span data-stu-id="08560-1751">The operators all compute the quotient of `x` and `y`.</span></span>

*  <span data-ttu-id="08560-1752">Dělení celého čísla:</span><span class="sxs-lookup"><span data-stu-id="08560-1752">Integer division:</span></span>

   ```csharp
   int operator /(int x, int y);
   uint operator /(uint x, uint y);
   long operator /(long x, long y);
   ulong operator /(ulong x, ulong y);
   ```

   <span data-ttu-id="08560-1753">Je-li hodnota pravého operandu nula, je vyvolána `System.DivideByZeroException`.</span><span class="sxs-lookup"><span data-stu-id="08560-1753">If the value of the right operand is zero, a `System.DivideByZeroException` is thrown.</span></span>

   <span data-ttu-id="08560-1754">Dělení zaokrouhlí výsledek směrem k nule.</span><span class="sxs-lookup"><span data-stu-id="08560-1754">The division rounds the result towards zero.</span></span> <span data-ttu-id="08560-1755">Proto absolutní hodnota výsledku je největší možné celé číslo, které je menší než nebo rovno absolutní hodnotě podílu dvou operandů.</span><span class="sxs-lookup"><span data-stu-id="08560-1755">Thus the absolute value of the result is the largest possible integer that is less than or equal to the absolute value of the quotient of the two operands.</span></span> <span data-ttu-id="08560-1756">Výsledkem je nula nebo kladné, pokud dva operandy mají stejné znaménko a nula nebo záporné, pokud mají dva operandy opačné znaménko.</span><span class="sxs-lookup"><span data-stu-id="08560-1756">The result is zero or positive when the two operands have the same sign and zero or negative when the two operands have opposite signs.</span></span>

   <span data-ttu-id="08560-1757">Pokud je levý operand nejmenší reprezentovatelné `int` nebo `long`á hodnota a pravý operand je `-1`, dojde k přetečení.</span><span class="sxs-lookup"><span data-stu-id="08560-1757">If the left operand is the smallest representable `int` or `long` value and the right operand is `-1`, an overflow occurs.</span></span> <span data-ttu-id="08560-1758">V kontextu `checked` to způsobí vyvolání `System.ArithmeticException` (nebo podtříd).</span><span class="sxs-lookup"><span data-stu-id="08560-1758">In a `checked` context, this causes a `System.ArithmeticException` (or a subclass thereof) to be thrown.</span></span> <span data-ttu-id="08560-1759">V kontextu `unchecked` je implementace definovaná jako k tomu, zda je vyvolána `System.ArithmeticException` (nebo podtříd), nebo přetečení se neohlásí výslednou hodnotou levého operandu.</span><span class="sxs-lookup"><span data-stu-id="08560-1759">In an `unchecked` context, it is implementation-defined as to whether a `System.ArithmeticException` (or a subclass thereof) is thrown or the overflow goes unreported with the resulting value being that of the left operand.</span></span>

*  <span data-ttu-id="08560-1760">Dělení plovoucí desetinné čárky:</span><span class="sxs-lookup"><span data-stu-id="08560-1760">Floating-point division:</span></span>

   ```csharp
   float operator /(float x, float y);
   double operator /(double x, double y);
   ```

   <span data-ttu-id="08560-1761">Podíl se vypočítává podle pravidel aritmetické operace IEEE 754.</span><span class="sxs-lookup"><span data-stu-id="08560-1761">The quotient is computed according to the rules of IEEE 754 arithmetic.</span></span> <span data-ttu-id="08560-1762">V následující tabulce jsou uvedeny výsledky všech možných kombinací nenulových konečných hodnot, nul, nekonečno a NaN.</span><span class="sxs-lookup"><span data-stu-id="08560-1762">The following table lists the results of all possible combinations of nonzero finite values, zeros, infinities, and NaN's.</span></span> <span data-ttu-id="08560-1763">V tabulce `x` a `y` jsou kladné konečné hodnoty.</span><span class="sxs-lookup"><span data-stu-id="08560-1763">In the table, `x` and `y` are positive finite values.</span></span> <span data-ttu-id="08560-1764">`z` je výsledkem `x / y`.</span><span class="sxs-lookup"><span data-stu-id="08560-1764">`z` is the result of `x / y`.</span></span> <span data-ttu-id="08560-1765">Pokud je výsledek pro cílový typ příliš velký, `z` je nekonečno.</span><span class="sxs-lookup"><span data-stu-id="08560-1765">If the result is too large for the destination type, `z` is infinity.</span></span> <span data-ttu-id="08560-1766">Pokud je výsledek pro cílový typ příliš malý, `z` je nula.</span><span class="sxs-lookup"><span data-stu-id="08560-1766">If the result is too small for the destination type, `z` is zero.</span></span>

   |      |      |      |      |      |      |      |      |
   |:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|
   |      | <span data-ttu-id="08560-1767">\+ y</span><span class="sxs-lookup"><span data-stu-id="08560-1767">+y</span></span>   | <span data-ttu-id="08560-1768">-y</span><span class="sxs-lookup"><span data-stu-id="08560-1768">-y</span></span>   | <span data-ttu-id="08560-1769">+0</span><span class="sxs-lookup"><span data-stu-id="08560-1769">+0</span></span>   | <span data-ttu-id="08560-1770">-0</span><span class="sxs-lookup"><span data-stu-id="08560-1770">-0</span></span>   | <span data-ttu-id="08560-1771">\+ INF</span><span class="sxs-lookup"><span data-stu-id="08560-1771">+inf</span></span> | <span data-ttu-id="08560-1772">– INF</span><span class="sxs-lookup"><span data-stu-id="08560-1772">-inf</span></span> | <span data-ttu-id="08560-1773">NaN</span><span class="sxs-lookup"><span data-stu-id="08560-1773">NaN</span></span>  | 
   | <span data-ttu-id="08560-1774">+x</span><span class="sxs-lookup"><span data-stu-id="08560-1774">+x</span></span>   | <span data-ttu-id="08560-1775">+z</span><span class="sxs-lookup"><span data-stu-id="08560-1775">+z</span></span>   | <span data-ttu-id="08560-1776">-z</span><span class="sxs-lookup"><span data-stu-id="08560-1776">-z</span></span>   | <span data-ttu-id="08560-1777">\+ INF</span><span class="sxs-lookup"><span data-stu-id="08560-1777">+inf</span></span> | <span data-ttu-id="08560-1778">– INF</span><span class="sxs-lookup"><span data-stu-id="08560-1778">-inf</span></span> | <span data-ttu-id="08560-1779">+0</span><span class="sxs-lookup"><span data-stu-id="08560-1779">+0</span></span>   | <span data-ttu-id="08560-1780">-0</span><span class="sxs-lookup"><span data-stu-id="08560-1780">-0</span></span>   | <span data-ttu-id="08560-1781">NaN</span><span class="sxs-lookup"><span data-stu-id="08560-1781">NaN</span></span>  | 
   | <span data-ttu-id="08560-1782">-x</span><span class="sxs-lookup"><span data-stu-id="08560-1782">-x</span></span>   | <span data-ttu-id="08560-1783">-z</span><span class="sxs-lookup"><span data-stu-id="08560-1783">-z</span></span>   | <span data-ttu-id="08560-1784">+z</span><span class="sxs-lookup"><span data-stu-id="08560-1784">+z</span></span>   | <span data-ttu-id="08560-1785">– INF</span><span class="sxs-lookup"><span data-stu-id="08560-1785">-inf</span></span> | <span data-ttu-id="08560-1786">\+ INF</span><span class="sxs-lookup"><span data-stu-id="08560-1786">+inf</span></span> | <span data-ttu-id="08560-1787">-0</span><span class="sxs-lookup"><span data-stu-id="08560-1787">-0</span></span>   | <span data-ttu-id="08560-1788">+0</span><span class="sxs-lookup"><span data-stu-id="08560-1788">+0</span></span>   | <span data-ttu-id="08560-1789">NaN</span><span class="sxs-lookup"><span data-stu-id="08560-1789">NaN</span></span>  | 
   | <span data-ttu-id="08560-1790">+0</span><span class="sxs-lookup"><span data-stu-id="08560-1790">+0</span></span>   | <span data-ttu-id="08560-1791">+0</span><span class="sxs-lookup"><span data-stu-id="08560-1791">+0</span></span>   | <span data-ttu-id="08560-1792">-0</span><span class="sxs-lookup"><span data-stu-id="08560-1792">-0</span></span>   | <span data-ttu-id="08560-1793">NaN</span><span class="sxs-lookup"><span data-stu-id="08560-1793">NaN</span></span>  | <span data-ttu-id="08560-1794">NaN</span><span class="sxs-lookup"><span data-stu-id="08560-1794">NaN</span></span>  | <span data-ttu-id="08560-1795">+0</span><span class="sxs-lookup"><span data-stu-id="08560-1795">+0</span></span>   | <span data-ttu-id="08560-1796">-0</span><span class="sxs-lookup"><span data-stu-id="08560-1796">-0</span></span>   | <span data-ttu-id="08560-1797">NaN</span><span class="sxs-lookup"><span data-stu-id="08560-1797">NaN</span></span>  | 
   | <span data-ttu-id="08560-1798">-0</span><span class="sxs-lookup"><span data-stu-id="08560-1798">-0</span></span>   | <span data-ttu-id="08560-1799">-0</span><span class="sxs-lookup"><span data-stu-id="08560-1799">-0</span></span>   | <span data-ttu-id="08560-1800">+0</span><span class="sxs-lookup"><span data-stu-id="08560-1800">+0</span></span>   | <span data-ttu-id="08560-1801">NaN</span><span class="sxs-lookup"><span data-stu-id="08560-1801">NaN</span></span>  | <span data-ttu-id="08560-1802">NaN</span><span class="sxs-lookup"><span data-stu-id="08560-1802">NaN</span></span>  | <span data-ttu-id="08560-1803">-0</span><span class="sxs-lookup"><span data-stu-id="08560-1803">-0</span></span>   | <span data-ttu-id="08560-1804">+0</span><span class="sxs-lookup"><span data-stu-id="08560-1804">+0</span></span>   | <span data-ttu-id="08560-1805">NaN</span><span class="sxs-lookup"><span data-stu-id="08560-1805">NaN</span></span>  | 
   | <span data-ttu-id="08560-1806">\+ INF</span><span class="sxs-lookup"><span data-stu-id="08560-1806">+inf</span></span> | <span data-ttu-id="08560-1807">\+ INF</span><span class="sxs-lookup"><span data-stu-id="08560-1807">+inf</span></span> | <span data-ttu-id="08560-1808">– INF</span><span class="sxs-lookup"><span data-stu-id="08560-1808">-inf</span></span> | <span data-ttu-id="08560-1809">\+ INF</span><span class="sxs-lookup"><span data-stu-id="08560-1809">+inf</span></span> | <span data-ttu-id="08560-1810">– INF</span><span class="sxs-lookup"><span data-stu-id="08560-1810">-inf</span></span> | <span data-ttu-id="08560-1811">NaN</span><span class="sxs-lookup"><span data-stu-id="08560-1811">NaN</span></span>  | <span data-ttu-id="08560-1812">NaN</span><span class="sxs-lookup"><span data-stu-id="08560-1812">NaN</span></span>  | <span data-ttu-id="08560-1813">NaN</span><span class="sxs-lookup"><span data-stu-id="08560-1813">NaN</span></span>  | 
   | <span data-ttu-id="08560-1814">– INF</span><span class="sxs-lookup"><span data-stu-id="08560-1814">-inf</span></span> | <span data-ttu-id="08560-1815">– INF</span><span class="sxs-lookup"><span data-stu-id="08560-1815">-inf</span></span> | <span data-ttu-id="08560-1816">\+ INF</span><span class="sxs-lookup"><span data-stu-id="08560-1816">+inf</span></span> | <span data-ttu-id="08560-1817">– INF</span><span class="sxs-lookup"><span data-stu-id="08560-1817">-inf</span></span> | <span data-ttu-id="08560-1818">\+ INF</span><span class="sxs-lookup"><span data-stu-id="08560-1818">+inf</span></span> | <span data-ttu-id="08560-1819">NaN</span><span class="sxs-lookup"><span data-stu-id="08560-1819">NaN</span></span>  | <span data-ttu-id="08560-1820">NaN</span><span class="sxs-lookup"><span data-stu-id="08560-1820">NaN</span></span>  | <span data-ttu-id="08560-1821">NaN</span><span class="sxs-lookup"><span data-stu-id="08560-1821">NaN</span></span>  | 
   | <span data-ttu-id="08560-1822">NaN</span><span class="sxs-lookup"><span data-stu-id="08560-1822">NaN</span></span>  | <span data-ttu-id="08560-1823">NaN</span><span class="sxs-lookup"><span data-stu-id="08560-1823">NaN</span></span>  | <span data-ttu-id="08560-1824">NaN</span><span class="sxs-lookup"><span data-stu-id="08560-1824">NaN</span></span>  | <span data-ttu-id="08560-1825">NaN</span><span class="sxs-lookup"><span data-stu-id="08560-1825">NaN</span></span>  | <span data-ttu-id="08560-1826">NaN</span><span class="sxs-lookup"><span data-stu-id="08560-1826">NaN</span></span>  | <span data-ttu-id="08560-1827">NaN</span><span class="sxs-lookup"><span data-stu-id="08560-1827">NaN</span></span>  | <span data-ttu-id="08560-1828">NaN</span><span class="sxs-lookup"><span data-stu-id="08560-1828">NaN</span></span>  | <span data-ttu-id="08560-1829">NaN</span><span class="sxs-lookup"><span data-stu-id="08560-1829">NaN</span></span>  | 

*  <span data-ttu-id="08560-1830">Desetinné dělení:</span><span class="sxs-lookup"><span data-stu-id="08560-1830">Decimal division:</span></span>

   ```csharp
   decimal operator /(decimal x, decimal y);
   ```

   <span data-ttu-id="08560-1831">Je-li hodnota pravého operandu nula, je vyvolána `System.DivideByZeroException`.</span><span class="sxs-lookup"><span data-stu-id="08560-1831">If the value of the right operand is zero, a `System.DivideByZeroException` is thrown.</span></span> <span data-ttu-id="08560-1832">Pokud je výsledná hodnota příliš velká pro reprezentaci ve formátu `decimal`, je vyvolána `System.OverflowException`.</span><span class="sxs-lookup"><span data-stu-id="08560-1832">If the resulting value is too large to represent in the `decimal` format, a `System.OverflowException` is thrown.</span></span> <span data-ttu-id="08560-1833">Pokud je výsledná hodnota příliš malá, aby byla reprezentována ve formátu `decimal`, je výsledek nula.</span><span class="sxs-lookup"><span data-stu-id="08560-1833">If the result value is too small to represent in the `decimal` format, the result is zero.</span></span> <span data-ttu-id="08560-1834">Měřítko výsledku je nejmenší měřítko, které zachová výsledek, který se rovná nejbližší hodnotě v desítkové soustavě k skutečnému matematickému výsledku.</span><span class="sxs-lookup"><span data-stu-id="08560-1834">The scale of the result is the smallest scale that will preserve a result equal to the nearest representable decimal value to the true mathematical result.</span></span>

   <span data-ttu-id="08560-1835">Desetinná čárka je ekvivalentní použití operátoru dělení typu `System.Decimal`.</span><span class="sxs-lookup"><span data-stu-id="08560-1835">Decimal division is equivalent to using the division operator of type `System.Decimal`.</span></span>


### <a name="remainder-operator"></a><span data-ttu-id="08560-1836">Operátor zbývající</span><span class="sxs-lookup"><span data-stu-id="08560-1836">Remainder operator</span></span>

<span data-ttu-id="08560-1837">Pro vybírání implementace konkrétního operátora se použije pro operaci `x % y`formuláře, řešení přetížení binárního operátoru ([rozlišení přetížení binárního operátoru](expressions.md#binary-operator-overload-resolution)).</span><span class="sxs-lookup"><span data-stu-id="08560-1837">For an operation of the form `x % y`, binary operator overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="08560-1838">Operandy jsou převedeny na typy parametrů vybraného operátoru a typ výsledku je návratový typ operátoru.</span><span class="sxs-lookup"><span data-stu-id="08560-1838">The operands are converted to the parameter types of the selected operator, and the type of the result is the return type of the operator.</span></span>

<span data-ttu-id="08560-1839">Předdefinované zbývající operátory jsou uvedeny níže.</span><span class="sxs-lookup"><span data-stu-id="08560-1839">The predefined remainder operators are listed below.</span></span> <span data-ttu-id="08560-1840">Všechny operátory počítají zbývající část rozdělení mezi `x` a `y`.</span><span class="sxs-lookup"><span data-stu-id="08560-1840">The operators all compute the remainder of the division between `x` and `y`.</span></span>

*  <span data-ttu-id="08560-1841">Celočíselný zbytek:</span><span class="sxs-lookup"><span data-stu-id="08560-1841">Integer remainder:</span></span>

   ```csharp
   int operator %(int x, int y);
   uint operator %(uint x, uint y);
   long operator %(long x, long y);
   ulong operator %(ulong x, ulong y);
   ```

   <span data-ttu-id="08560-1842">Výsledkem `x % y` je hodnota vytvořená `x - (x / y) * y`.</span><span class="sxs-lookup"><span data-stu-id="08560-1842">The result of `x % y` is the value produced by `x - (x / y) * y`.</span></span> <span data-ttu-id="08560-1843">Je-li `y` nula, je vyvolána `System.DivideByZeroException`.</span><span class="sxs-lookup"><span data-stu-id="08560-1843">If `y` is zero, a `System.DivideByZeroException` is thrown.</span></span>

   <span data-ttu-id="08560-1844">Pokud je levý operand nejmenší `int` nebo `long` hodnota a pravý operand je `-1`, je vyvolána `System.OverflowException`.</span><span class="sxs-lookup"><span data-stu-id="08560-1844">If the left operand is the smallest `int` or `long` value and the right operand is `-1`, a `System.OverflowException` is thrown.</span></span> <span data-ttu-id="08560-1845">V žádném případě `x % y` vyvolat výjimku, kde `x / y` by nevyvolal výjimku.</span><span class="sxs-lookup"><span data-stu-id="08560-1845">In no case does `x % y` throw an exception where `x / y` would not throw an exception.</span></span>

*  <span data-ttu-id="08560-1846">Zbytek s plovoucí desetinnou čárkou:</span><span class="sxs-lookup"><span data-stu-id="08560-1846">Floating-point remainder:</span></span>

   ```csharp
   float operator %(float x, float y);
   double operator %(double x, double y);
   ```

   <span data-ttu-id="08560-1847">V následující tabulce jsou uvedeny výsledky všech možných kombinací nenulových konečných hodnot, nul, nekonečno a NaN.</span><span class="sxs-lookup"><span data-stu-id="08560-1847">The following table lists the results of all possible combinations of nonzero finite values, zeros, infinities, and NaN's.</span></span> <span data-ttu-id="08560-1848">V tabulce `x` a `y` jsou kladné konečné hodnoty.</span><span class="sxs-lookup"><span data-stu-id="08560-1848">In the table, `x` and `y` are positive finite values.</span></span> <span data-ttu-id="08560-1849">`z` je výsledkem `x % y` a je vypočítán jako `x - n * y`, kde `n` je největší možné celé číslo, které je menší nebo rovno `x / y`.</span><span class="sxs-lookup"><span data-stu-id="08560-1849">`z` is the result of `x % y` and is computed as `x - n * y`, where `n` is the largest possible integer that is less than or equal to `x / y`.</span></span> <span data-ttu-id="08560-1850">Tato metoda výpočtu zbytku je podobná, jako by se použila pro celočíselné operandy, ale liší se od definice IEEE 754 (v tom, že `n` je celé číslo nejbližší k `x / y`).</span><span class="sxs-lookup"><span data-stu-id="08560-1850">This method of computing the remainder is analogous to that used for integer operands, but differs from the IEEE 754 definition (in which `n` is the integer closest to `x / y`).</span></span>

   |      |      |      |      |      |      |      |      |
   |:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|
   |      | <span data-ttu-id="08560-1851">\+ y</span><span class="sxs-lookup"><span data-stu-id="08560-1851">+y</span></span>   | <span data-ttu-id="08560-1852">-y</span><span class="sxs-lookup"><span data-stu-id="08560-1852">-y</span></span>   | <span data-ttu-id="08560-1853">+0</span><span class="sxs-lookup"><span data-stu-id="08560-1853">+0</span></span>   | <span data-ttu-id="08560-1854">-0</span><span class="sxs-lookup"><span data-stu-id="08560-1854">-0</span></span>   | <span data-ttu-id="08560-1855">\+ INF</span><span class="sxs-lookup"><span data-stu-id="08560-1855">+inf</span></span> | <span data-ttu-id="08560-1856">– INF</span><span class="sxs-lookup"><span data-stu-id="08560-1856">-inf</span></span> | <span data-ttu-id="08560-1857">NaN</span><span class="sxs-lookup"><span data-stu-id="08560-1857">NaN</span></span>  | 
   | <span data-ttu-id="08560-1858">+x</span><span class="sxs-lookup"><span data-stu-id="08560-1858">+x</span></span>   | <span data-ttu-id="08560-1859">+z</span><span class="sxs-lookup"><span data-stu-id="08560-1859">+z</span></span>   | <span data-ttu-id="08560-1860">+z</span><span class="sxs-lookup"><span data-stu-id="08560-1860">+z</span></span>   | <span data-ttu-id="08560-1861">NaN</span><span class="sxs-lookup"><span data-stu-id="08560-1861">NaN</span></span>  | <span data-ttu-id="08560-1862">NaN</span><span class="sxs-lookup"><span data-stu-id="08560-1862">NaN</span></span>  | <span data-ttu-id="08560-1863">x</span><span class="sxs-lookup"><span data-stu-id="08560-1863">x</span></span>    | <span data-ttu-id="08560-1864">x</span><span class="sxs-lookup"><span data-stu-id="08560-1864">x</span></span>    | <span data-ttu-id="08560-1865">NaN</span><span class="sxs-lookup"><span data-stu-id="08560-1865">NaN</span></span>  | 
   | <span data-ttu-id="08560-1866">-x</span><span class="sxs-lookup"><span data-stu-id="08560-1866">-x</span></span>   | <span data-ttu-id="08560-1867">-z</span><span class="sxs-lookup"><span data-stu-id="08560-1867">-z</span></span>   | <span data-ttu-id="08560-1868">-z</span><span class="sxs-lookup"><span data-stu-id="08560-1868">-z</span></span>   | <span data-ttu-id="08560-1869">NaN</span><span class="sxs-lookup"><span data-stu-id="08560-1869">NaN</span></span>  | <span data-ttu-id="08560-1870">NaN</span><span class="sxs-lookup"><span data-stu-id="08560-1870">NaN</span></span>  | <span data-ttu-id="08560-1871">-x</span><span class="sxs-lookup"><span data-stu-id="08560-1871">-x</span></span>   | <span data-ttu-id="08560-1872">-x</span><span class="sxs-lookup"><span data-stu-id="08560-1872">-x</span></span>   | <span data-ttu-id="08560-1873">NaN</span><span class="sxs-lookup"><span data-stu-id="08560-1873">NaN</span></span>  | 
   | <span data-ttu-id="08560-1874">+0</span><span class="sxs-lookup"><span data-stu-id="08560-1874">+0</span></span>   | <span data-ttu-id="08560-1875">+0</span><span class="sxs-lookup"><span data-stu-id="08560-1875">+0</span></span>   | <span data-ttu-id="08560-1876">+0</span><span class="sxs-lookup"><span data-stu-id="08560-1876">+0</span></span>   | <span data-ttu-id="08560-1877">NaN</span><span class="sxs-lookup"><span data-stu-id="08560-1877">NaN</span></span>  | <span data-ttu-id="08560-1878">NaN</span><span class="sxs-lookup"><span data-stu-id="08560-1878">NaN</span></span>  | <span data-ttu-id="08560-1879">+0</span><span class="sxs-lookup"><span data-stu-id="08560-1879">+0</span></span>   | <span data-ttu-id="08560-1880">+0</span><span class="sxs-lookup"><span data-stu-id="08560-1880">+0</span></span>   | <span data-ttu-id="08560-1881">NaN</span><span class="sxs-lookup"><span data-stu-id="08560-1881">NaN</span></span>  | 
   | <span data-ttu-id="08560-1882">-0</span><span class="sxs-lookup"><span data-stu-id="08560-1882">-0</span></span>   | <span data-ttu-id="08560-1883">-0</span><span class="sxs-lookup"><span data-stu-id="08560-1883">-0</span></span>   | <span data-ttu-id="08560-1884">-0</span><span class="sxs-lookup"><span data-stu-id="08560-1884">-0</span></span>   | <span data-ttu-id="08560-1885">NaN</span><span class="sxs-lookup"><span data-stu-id="08560-1885">NaN</span></span>  | <span data-ttu-id="08560-1886">NaN</span><span class="sxs-lookup"><span data-stu-id="08560-1886">NaN</span></span>  | <span data-ttu-id="08560-1887">-0</span><span class="sxs-lookup"><span data-stu-id="08560-1887">-0</span></span>   | <span data-ttu-id="08560-1888">-0</span><span class="sxs-lookup"><span data-stu-id="08560-1888">-0</span></span>   | <span data-ttu-id="08560-1889">NaN</span><span class="sxs-lookup"><span data-stu-id="08560-1889">NaN</span></span>  | 
   | <span data-ttu-id="08560-1890">\+ INF</span><span class="sxs-lookup"><span data-stu-id="08560-1890">+inf</span></span> | <span data-ttu-id="08560-1891">NaN</span><span class="sxs-lookup"><span data-stu-id="08560-1891">NaN</span></span>  | <span data-ttu-id="08560-1892">NaN</span><span class="sxs-lookup"><span data-stu-id="08560-1892">NaN</span></span>  | <span data-ttu-id="08560-1893">NaN</span><span class="sxs-lookup"><span data-stu-id="08560-1893">NaN</span></span>  | <span data-ttu-id="08560-1894">NaN</span><span class="sxs-lookup"><span data-stu-id="08560-1894">NaN</span></span>  | <span data-ttu-id="08560-1895">NaN</span><span class="sxs-lookup"><span data-stu-id="08560-1895">NaN</span></span>  | <span data-ttu-id="08560-1896">NaN</span><span class="sxs-lookup"><span data-stu-id="08560-1896">NaN</span></span>  | <span data-ttu-id="08560-1897">NaN</span><span class="sxs-lookup"><span data-stu-id="08560-1897">NaN</span></span>  | 
   | <span data-ttu-id="08560-1898">– INF</span><span class="sxs-lookup"><span data-stu-id="08560-1898">-inf</span></span> | <span data-ttu-id="08560-1899">NaN</span><span class="sxs-lookup"><span data-stu-id="08560-1899">NaN</span></span>  | <span data-ttu-id="08560-1900">NaN</span><span class="sxs-lookup"><span data-stu-id="08560-1900">NaN</span></span>  | <span data-ttu-id="08560-1901">NaN</span><span class="sxs-lookup"><span data-stu-id="08560-1901">NaN</span></span>  | <span data-ttu-id="08560-1902">NaN</span><span class="sxs-lookup"><span data-stu-id="08560-1902">NaN</span></span>  | <span data-ttu-id="08560-1903">NaN</span><span class="sxs-lookup"><span data-stu-id="08560-1903">NaN</span></span>  | <span data-ttu-id="08560-1904">NaN</span><span class="sxs-lookup"><span data-stu-id="08560-1904">NaN</span></span>  | <span data-ttu-id="08560-1905">NaN</span><span class="sxs-lookup"><span data-stu-id="08560-1905">NaN</span></span>  | 
   | <span data-ttu-id="08560-1906">NaN</span><span class="sxs-lookup"><span data-stu-id="08560-1906">NaN</span></span>  | <span data-ttu-id="08560-1907">NaN</span><span class="sxs-lookup"><span data-stu-id="08560-1907">NaN</span></span>  | <span data-ttu-id="08560-1908">NaN</span><span class="sxs-lookup"><span data-stu-id="08560-1908">NaN</span></span>  | <span data-ttu-id="08560-1909">NaN</span><span class="sxs-lookup"><span data-stu-id="08560-1909">NaN</span></span>  | <span data-ttu-id="08560-1910">NaN</span><span class="sxs-lookup"><span data-stu-id="08560-1910">NaN</span></span>  | <span data-ttu-id="08560-1911">NaN</span><span class="sxs-lookup"><span data-stu-id="08560-1911">NaN</span></span>  | <span data-ttu-id="08560-1912">NaN</span><span class="sxs-lookup"><span data-stu-id="08560-1912">NaN</span></span>  | <span data-ttu-id="08560-1913">NaN</span><span class="sxs-lookup"><span data-stu-id="08560-1913">NaN</span></span>  | 

*  <span data-ttu-id="08560-1914">Desetinná část:</span><span class="sxs-lookup"><span data-stu-id="08560-1914">Decimal remainder:</span></span>

   ```csharp
   decimal operator %(decimal x, decimal y);
   ```

   <span data-ttu-id="08560-1915">Je-li hodnota pravého operandu nula, je vyvolána `System.DivideByZeroException`.</span><span class="sxs-lookup"><span data-stu-id="08560-1915">If the value of the right operand is zero, a `System.DivideByZeroException` is thrown.</span></span> <span data-ttu-id="08560-1916">Měřítko výsledku, před jakýmkoli zaokrouhlením, je větší z škály dvou operandů a znaménko výsledku, pokud je nenulová, je stejné jako `x`.</span><span class="sxs-lookup"><span data-stu-id="08560-1916">The scale of the result, before any rounding, is the larger of the scales of the two operands, and the sign of the result, if non-zero, is the same as that of `x`.</span></span>

   <span data-ttu-id="08560-1917">Desítkový zbytek je ekvivalentem použití operátoru zbytek typu `System.Decimal`.</span><span class="sxs-lookup"><span data-stu-id="08560-1917">Decimal remainder is equivalent to using the remainder operator of type `System.Decimal`.</span></span>


### <a name="addition-operator"></a><span data-ttu-id="08560-1918">Operátor sčítání</span><span class="sxs-lookup"><span data-stu-id="08560-1918">Addition operator</span></span>

<span data-ttu-id="08560-1919">Pro vybírání implementace konkrétního operátora se použije pro operaci `x + y`formuláře, řešení přetížení binárního operátoru ([rozlišení přetížení binárního operátoru](expressions.md#binary-operator-overload-resolution)).</span><span class="sxs-lookup"><span data-stu-id="08560-1919">For an operation of the form `x + y`, binary operator overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="08560-1920">Operandy jsou převedeny na typy parametrů vybraného operátoru a typ výsledku je návratový typ operátoru.</span><span class="sxs-lookup"><span data-stu-id="08560-1920">The operands are converted to the parameter types of the selected operator, and the type of the result is the return type of the operator.</span></span>

<span data-ttu-id="08560-1921">Předdefinované operátory sčítání jsou uvedeny níže.</span><span class="sxs-lookup"><span data-stu-id="08560-1921">The predefined addition operators are listed below.</span></span> <span data-ttu-id="08560-1922">Pro číselné a výčtové typy jsou předdefinované operátory sčítání počítány součtem obou operandů.</span><span class="sxs-lookup"><span data-stu-id="08560-1922">For numeric and enumeration types, the predefined addition operators compute the sum of the two operands.</span></span> <span data-ttu-id="08560-1923">Když je jeden nebo oba operandy typu String, předdefinované operátory sčítání zřetězí řetězcovou reprezentaci operandů.</span><span class="sxs-lookup"><span data-stu-id="08560-1923">When one or both operands are of type string, the predefined addition operators concatenate the string representation of the operands.</span></span>

*  <span data-ttu-id="08560-1924">Sčítání celého čísla:</span><span class="sxs-lookup"><span data-stu-id="08560-1924">Integer addition:</span></span>

   ```csharp
   int operator +(int x, int y);
   uint operator +(uint x, uint y);
   long operator +(long x, long y);
   ulong operator +(ulong x, ulong y);
   ```

   <span data-ttu-id="08560-1925">V kontextu `checked`, pokud je součet mimo rozsah výsledného typu, je vyvolána `System.OverflowException`.</span><span class="sxs-lookup"><span data-stu-id="08560-1925">In a `checked` context, if the sum is outside the range of the result type, a `System.OverflowException` is thrown.</span></span> <span data-ttu-id="08560-1926">V kontextu `unchecked` nejsou přetečení hlášeny a jakékoli významné bity vysokého řádu mimo rozsah výsledného typu jsou zahozeny.</span><span class="sxs-lookup"><span data-stu-id="08560-1926">In an `unchecked` context, overflows are not reported and any significant high-order bits outside the range of the result type are discarded.</span></span>

*  <span data-ttu-id="08560-1927">Přidání plovoucí desetinné čárky:</span><span class="sxs-lookup"><span data-stu-id="08560-1927">Floating-point addition:</span></span>

   ```csharp
   float operator +(float x, float y);
   double operator +(double x, double y);
   ```

   <span data-ttu-id="08560-1928">Součet se vypočítává podle pravidel aritmetické operace IEEE 754.</span><span class="sxs-lookup"><span data-stu-id="08560-1928">The sum is computed according to the rules of IEEE 754 arithmetic.</span></span> <span data-ttu-id="08560-1929">V následující tabulce jsou uvedeny výsledky všech možných kombinací nenulových konečných hodnot, nul, nekonečno a NaN.</span><span class="sxs-lookup"><span data-stu-id="08560-1929">The following table lists the results of all possible combinations of nonzero finite values, zeros, infinities, and NaN's.</span></span> <span data-ttu-id="08560-1930">V tabulce jsou `x` a `y` nenulové konečné hodnoty a `z` je výsledkem `x + y`.</span><span class="sxs-lookup"><span data-stu-id="08560-1930">In the table, `x` and `y` are nonzero finite values, and `z` is the result of `x + y`.</span></span> <span data-ttu-id="08560-1931">Pokud `x` a `y` mají stejnou velikost, ale opačné znaménko, `z` je kladné nula.</span><span class="sxs-lookup"><span data-stu-id="08560-1931">If `x` and `y` have the same magnitude but opposite signs, `z` is positive zero.</span></span> <span data-ttu-id="08560-1932">Pokud je `x + y` příliš velký pro reprezentaci v cílovém typu, `z` je nekonečno se stejným znaménkem jako `x + y`.</span><span class="sxs-lookup"><span data-stu-id="08560-1932">If `x + y` is too large to represent in the destination type, `z` is an infinity with the same sign as `x + y`.</span></span>

   |      |      |      |      |      |      |      |
   |:----:|:----:|:----:|:----:|:----:|:----:|:----:|
   |      | <span data-ttu-id="08560-1933">y</span><span class="sxs-lookup"><span data-stu-id="08560-1933">y</span></span>    | <span data-ttu-id="08560-1934">+0</span><span class="sxs-lookup"><span data-stu-id="08560-1934">+0</span></span>   | <span data-ttu-id="08560-1935">-0</span><span class="sxs-lookup"><span data-stu-id="08560-1935">-0</span></span>   | <span data-ttu-id="08560-1936">\+ INF</span><span class="sxs-lookup"><span data-stu-id="08560-1936">+inf</span></span> | <span data-ttu-id="08560-1937">– INF</span><span class="sxs-lookup"><span data-stu-id="08560-1937">-inf</span></span> | <span data-ttu-id="08560-1938">NaN</span><span class="sxs-lookup"><span data-stu-id="08560-1938">NaN</span></span>  | 
   | <span data-ttu-id="08560-1939">x</span><span class="sxs-lookup"><span data-stu-id="08560-1939">x</span></span>    | <span data-ttu-id="08560-1940">z</span><span class="sxs-lookup"><span data-stu-id="08560-1940">z</span></span>    | <span data-ttu-id="08560-1941">x</span><span class="sxs-lookup"><span data-stu-id="08560-1941">x</span></span>    | <span data-ttu-id="08560-1942">x</span><span class="sxs-lookup"><span data-stu-id="08560-1942">x</span></span>    | <span data-ttu-id="08560-1943">\+ INF</span><span class="sxs-lookup"><span data-stu-id="08560-1943">+inf</span></span> | <span data-ttu-id="08560-1944">– INF</span><span class="sxs-lookup"><span data-stu-id="08560-1944">-inf</span></span> | <span data-ttu-id="08560-1945">NaN</span><span class="sxs-lookup"><span data-stu-id="08560-1945">NaN</span></span>  | 
   | <span data-ttu-id="08560-1946">+0</span><span class="sxs-lookup"><span data-stu-id="08560-1946">+0</span></span>   | <span data-ttu-id="08560-1947">y</span><span class="sxs-lookup"><span data-stu-id="08560-1947">y</span></span>    | <span data-ttu-id="08560-1948">+0</span><span class="sxs-lookup"><span data-stu-id="08560-1948">+0</span></span>   | <span data-ttu-id="08560-1949">+0</span><span class="sxs-lookup"><span data-stu-id="08560-1949">+0</span></span>   | <span data-ttu-id="08560-1950">\+ INF</span><span class="sxs-lookup"><span data-stu-id="08560-1950">+inf</span></span> | <span data-ttu-id="08560-1951">– INF</span><span class="sxs-lookup"><span data-stu-id="08560-1951">-inf</span></span> | <span data-ttu-id="08560-1952">NaN</span><span class="sxs-lookup"><span data-stu-id="08560-1952">NaN</span></span>  | 
   | <span data-ttu-id="08560-1953">-0</span><span class="sxs-lookup"><span data-stu-id="08560-1953">-0</span></span>   | <span data-ttu-id="08560-1954">y</span><span class="sxs-lookup"><span data-stu-id="08560-1954">y</span></span>    | <span data-ttu-id="08560-1955">+0</span><span class="sxs-lookup"><span data-stu-id="08560-1955">+0</span></span>   | <span data-ttu-id="08560-1956">-0</span><span class="sxs-lookup"><span data-stu-id="08560-1956">-0</span></span>   | <span data-ttu-id="08560-1957">\+ INF</span><span class="sxs-lookup"><span data-stu-id="08560-1957">+inf</span></span> | <span data-ttu-id="08560-1958">– INF</span><span class="sxs-lookup"><span data-stu-id="08560-1958">-inf</span></span> | <span data-ttu-id="08560-1959">NaN</span><span class="sxs-lookup"><span data-stu-id="08560-1959">NaN</span></span>  | 
   | <span data-ttu-id="08560-1960">\+ INF</span><span class="sxs-lookup"><span data-stu-id="08560-1960">+inf</span></span> | <span data-ttu-id="08560-1961">\+ INF</span><span class="sxs-lookup"><span data-stu-id="08560-1961">+inf</span></span> | <span data-ttu-id="08560-1962">\+ INF</span><span class="sxs-lookup"><span data-stu-id="08560-1962">+inf</span></span> | <span data-ttu-id="08560-1963">\+ INF</span><span class="sxs-lookup"><span data-stu-id="08560-1963">+inf</span></span> | <span data-ttu-id="08560-1964">\+ INF</span><span class="sxs-lookup"><span data-stu-id="08560-1964">+inf</span></span> | <span data-ttu-id="08560-1965">NaN</span><span class="sxs-lookup"><span data-stu-id="08560-1965">NaN</span></span>  | <span data-ttu-id="08560-1966">NaN</span><span class="sxs-lookup"><span data-stu-id="08560-1966">NaN</span></span>  | 
   | <span data-ttu-id="08560-1967">– INF</span><span class="sxs-lookup"><span data-stu-id="08560-1967">-inf</span></span> | <span data-ttu-id="08560-1968">– INF</span><span class="sxs-lookup"><span data-stu-id="08560-1968">-inf</span></span> | <span data-ttu-id="08560-1969">– INF</span><span class="sxs-lookup"><span data-stu-id="08560-1969">-inf</span></span> | <span data-ttu-id="08560-1970">– INF</span><span class="sxs-lookup"><span data-stu-id="08560-1970">-inf</span></span> | <span data-ttu-id="08560-1971">NaN</span><span class="sxs-lookup"><span data-stu-id="08560-1971">NaN</span></span>  | <span data-ttu-id="08560-1972">– INF</span><span class="sxs-lookup"><span data-stu-id="08560-1972">-inf</span></span> | <span data-ttu-id="08560-1973">NaN</span><span class="sxs-lookup"><span data-stu-id="08560-1973">NaN</span></span>  | 
   | <span data-ttu-id="08560-1974">NaN</span><span class="sxs-lookup"><span data-stu-id="08560-1974">NaN</span></span>  | <span data-ttu-id="08560-1975">NaN</span><span class="sxs-lookup"><span data-stu-id="08560-1975">NaN</span></span>  | <span data-ttu-id="08560-1976">NaN</span><span class="sxs-lookup"><span data-stu-id="08560-1976">NaN</span></span>  | <span data-ttu-id="08560-1977">NaN</span><span class="sxs-lookup"><span data-stu-id="08560-1977">NaN</span></span>  | <span data-ttu-id="08560-1978">NaN</span><span class="sxs-lookup"><span data-stu-id="08560-1978">NaN</span></span>  | <span data-ttu-id="08560-1979">NaN</span><span class="sxs-lookup"><span data-stu-id="08560-1979">NaN</span></span>  | <span data-ttu-id="08560-1980">NaN</span><span class="sxs-lookup"><span data-stu-id="08560-1980">NaN</span></span>  | 

*  <span data-ttu-id="08560-1981">Sčítání desetinných míst:</span><span class="sxs-lookup"><span data-stu-id="08560-1981">Decimal addition:</span></span>

   ```csharp
   decimal operator +(decimal x, decimal y);
   ```

   <span data-ttu-id="08560-1982">Pokud je výsledná hodnota příliš velká pro reprezentaci ve formátu `decimal`, je vyvolána `System.OverflowException`.</span><span class="sxs-lookup"><span data-stu-id="08560-1982">If the resulting value is too large to represent in the `decimal` format, a `System.OverflowException` is thrown.</span></span> <span data-ttu-id="08560-1983">Měřítko výsledku, před jakýmkoli zaokrouhlením, je větší z škály dvou operandů.</span><span class="sxs-lookup"><span data-stu-id="08560-1983">The scale of the result, before any rounding, is the larger of the scales of the two operands.</span></span>

   <span data-ttu-id="08560-1984">Sčítání desetinných míst je ekvivalentní použití operátoru sčítání typu `System.Decimal`.</span><span class="sxs-lookup"><span data-stu-id="08560-1984">Decimal addition is equivalent to using the addition operator of type `System.Decimal`.</span></span>

*  <span data-ttu-id="08560-1985">Sčítání výčtu.</span><span class="sxs-lookup"><span data-stu-id="08560-1985">Enumeration addition.</span></span> <span data-ttu-id="08560-1986">Každý typ výčtu implicitně poskytuje následující předdefinované operátory, kde `E` je typ výčtu a `U` je základní typ `E`:</span><span class="sxs-lookup"><span data-stu-id="08560-1986">Every enumeration type implicitly provides the following predefined operators, where `E` is the enum type, and `U` is the underlying type of `E`:</span></span>

   ```csharp
   E operator +(E x, U y);
   E operator +(U x, E y);
   ```

   <span data-ttu-id="08560-1987">V době běhu jsou tyto operátory vyhodnocovány přesně jako `(E)((U)x + (U)y)`.</span><span class="sxs-lookup"><span data-stu-id="08560-1987">At run-time these operators are evaluated exactly as `(E)((U)x + (U)y)`.</span></span>

*  <span data-ttu-id="08560-1988">Zřetězení řetězců:</span><span class="sxs-lookup"><span data-stu-id="08560-1988">String concatenation:</span></span>

   ```csharp
   string operator +(string x, string y);
   string operator +(string x, object y);
   string operator +(object x, string y);
   ```

   <span data-ttu-id="08560-1989">Tato přetížení operátoru binárního `+` provádějí zřetězení řetězců.</span><span class="sxs-lookup"><span data-stu-id="08560-1989">These overloads of the binary `+` operator perform string concatenation.</span></span> <span data-ttu-id="08560-1990">Je-li operand zřetězení řetězců `null`, je nahrazen prázdný řetězec.</span><span class="sxs-lookup"><span data-stu-id="08560-1990">If an operand of string concatenation is `null`, an empty string is substituted.</span></span> <span data-ttu-id="08560-1991">V opačném případě je libovolný neřetězcový argument převeden na jeho řetězcové vyjádření vyvoláním metody Virtual `ToString` zděděné z `object`typu.</span><span class="sxs-lookup"><span data-stu-id="08560-1991">Otherwise, any non-string argument is converted to its string representation by invoking the virtual `ToString` method inherited from type `object`.</span></span> <span data-ttu-id="08560-1992">Pokud `ToString` vrátí `null`, je nahrazen prázdný řetězec.</span><span class="sxs-lookup"><span data-stu-id="08560-1992">If `ToString` returns `null`, an empty string is substituted.</span></span>

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

   Výsledek operátoru zřetězení řetězce je řetězec, který se skládá z znaků levého operandu následovaného znaky pravého operandu. Operátor zřetězení řetězců nikdy nevrací `null` hodnotu. <span data-ttu-id="08560-1995">`System.OutOfMemoryException` může být vyvolána, pokud není k dispozici dostatek paměti pro přidělení výsledného řetězce.</span><span class="sxs-lookup"><span data-stu-id="08560-1995">A `System.OutOfMemoryException` may be thrown if there is not enough memory available to allocate the resulting string.</span></span>

*  <span data-ttu-id="08560-1996">Kombinace delegátů.</span><span class="sxs-lookup"><span data-stu-id="08560-1996">Delegate combination.</span></span> <span data-ttu-id="08560-1997">Každý typ delegáta implicitně poskytuje následující předdefinovaný operátor, kde `D` je typ delegáta:</span><span class="sxs-lookup"><span data-stu-id="08560-1997">Every delegate type implicitly provides the following predefined operator, where `D` is the delegate type:</span></span>

   ```csharp
   D operator +(D x, D y);
   ```

   <span data-ttu-id="08560-1998">Operátor binárního `+` provádí kombinaci delegáta, pokud jsou oba operandy typu delegáta `D`.</span><span class="sxs-lookup"><span data-stu-id="08560-1998">The binary `+` operator performs delegate combination when both operands are of some delegate type `D`.</span></span> <span data-ttu-id="08560-1999">(Pokud operandy mají odlišné typy delegátů, dojde k chybě při vazbě.) Pokud je první operand `null`, výsledkem operace je hodnota druhého operandu (i v případě, že je také `null`).</span><span class="sxs-lookup"><span data-stu-id="08560-1999">(If the operands have different delegate types, a binding-time error occurs.) If the first operand is `null`, the result of the operation is the value of the second operand (even if that is also `null`).</span></span> <span data-ttu-id="08560-2000">V opačném případě, pokud je druhý operand `null`, pak výsledek operace je hodnota prvního operandu.</span><span class="sxs-lookup"><span data-stu-id="08560-2000">Otherwise, if the second operand is `null`, then the result of the operation is the value of the first operand.</span></span> <span data-ttu-id="08560-2001">V opačném případě je výsledkem operace nová instance delegáta, která při vyvolání vyvolá první operand a potom vyvolá druhý operand.</span><span class="sxs-lookup"><span data-stu-id="08560-2001">Otherwise, the result of the operation is a new delegate instance that, when invoked, invokes the first operand and then invokes the second operand.</span></span> <span data-ttu-id="08560-2002">Příklady kombinace delegátů naleznete v tématu [operátor odčítání](expressions.md#subtraction-operator) a [volání delegáta](delegates.md#delegate-invocation).</span><span class="sxs-lookup"><span data-stu-id="08560-2002">For examples of delegate combination, see [Subtraction operator](expressions.md#subtraction-operator) and [Delegate invocation](delegates.md#delegate-invocation).</span></span> <span data-ttu-id="08560-2003">Vzhledem k tomu, že `System.Delegate` není typu delegáta, `operator` `+` není pro něj definováno.</span><span class="sxs-lookup"><span data-stu-id="08560-2003">Since `System.Delegate` is not a delegate type, `operator` `+` is not defined for it.</span></span>

### <a name="subtraction-operator"></a><span data-ttu-id="08560-2004">Operátor odčítání</span><span class="sxs-lookup"><span data-stu-id="08560-2004">Subtraction operator</span></span>

<span data-ttu-id="08560-2005">Pro vybírání implementace konkrétního operátora se použije pro operaci `x - y`formuláře, řešení přetížení binárního operátoru ([rozlišení přetížení binárního operátoru](expressions.md#binary-operator-overload-resolution)).</span><span class="sxs-lookup"><span data-stu-id="08560-2005">For an operation of the form `x - y`, binary operator overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="08560-2006">Operandy jsou převedeny na typy parametrů vybraného operátoru a typ výsledku je návratový typ operátoru.</span><span class="sxs-lookup"><span data-stu-id="08560-2006">The operands are converted to the parameter types of the selected operator, and the type of the result is the return type of the operator.</span></span>

<span data-ttu-id="08560-2007">Předdefinované operátory odčítání jsou uvedeny níže.</span><span class="sxs-lookup"><span data-stu-id="08560-2007">The predefined subtraction operators are listed below.</span></span> <span data-ttu-id="08560-2008">Všechny operátory odečtou `y` od `x`.</span><span class="sxs-lookup"><span data-stu-id="08560-2008">The operators all subtract `y` from `x`.</span></span>

*  <span data-ttu-id="08560-2009">Celočíselná odčítání:</span><span class="sxs-lookup"><span data-stu-id="08560-2009">Integer subtraction:</span></span>

   ```csharp
   int operator -(int x, int y);
   uint operator -(uint x, uint y);
   long operator -(long x, long y);
   ulong operator -(ulong x, ulong y);
   ```

   <span data-ttu-id="08560-2010">V kontextu `checked`, pokud je rozdíl mimo rozsah výsledného typu, je vyvolána `System.OverflowException`.</span><span class="sxs-lookup"><span data-stu-id="08560-2010">In a `checked` context, if the difference is outside the range of the result type, a `System.OverflowException` is thrown.</span></span> <span data-ttu-id="08560-2011">V kontextu `unchecked` nejsou přetečení hlášeny a jakékoli významné bity vysokého řádu mimo rozsah výsledného typu jsou zahozeny.</span><span class="sxs-lookup"><span data-stu-id="08560-2011">In an `unchecked` context, overflows are not reported and any significant high-order bits outside the range of the result type are discarded.</span></span>

*  <span data-ttu-id="08560-2012">Odčítání plovoucí desetinné čárky:</span><span class="sxs-lookup"><span data-stu-id="08560-2012">Floating-point subtraction:</span></span>

   ```csharp
   float operator -(float x, float y);
   double operator -(double x, double y);
   ```

   <span data-ttu-id="08560-2013">Rozdíl se vypočítává podle pravidel aritmetické operace IEEE 754.</span><span class="sxs-lookup"><span data-stu-id="08560-2013">The difference is computed according to the rules of IEEE 754 arithmetic.</span></span> <span data-ttu-id="08560-2014">V následující tabulce jsou uvedeny výsledky všech možných kombinací nenulových konečných hodnot, nul, nekonečno a hodnoty NaN.</span><span class="sxs-lookup"><span data-stu-id="08560-2014">The following table lists the results of all possible combinations of nonzero finite values, zeros, infinities, and NaNs.</span></span> <span data-ttu-id="08560-2015">V tabulce jsou `x` a `y` nenulové konečné hodnoty a `z` je výsledkem `x - y`.</span><span class="sxs-lookup"><span data-stu-id="08560-2015">In the table, `x` and `y` are nonzero finite values, and `z` is the result of `x - y`.</span></span> <span data-ttu-id="08560-2016">Pokud jsou `x` a `y` stejné, `z` je kladné nula.</span><span class="sxs-lookup"><span data-stu-id="08560-2016">If `x` and `y` are equal, `z` is positive zero.</span></span> <span data-ttu-id="08560-2017">Pokud je `x - y` příliš velký pro reprezentaci v cílovém typu, `z` je nekonečno se stejným znaménkem jako `x - y`.</span><span class="sxs-lookup"><span data-stu-id="08560-2017">If `x - y` is too large to represent in the destination type, `z` is an infinity with the same sign as `x - y`.</span></span>

   |      |      |      |      |      |      |     |
   |:----:|:----:|:----:|:----:|:----:|:----:|:---:|
   |      | <span data-ttu-id="08560-2018">y</span><span class="sxs-lookup"><span data-stu-id="08560-2018">y</span></span>    | <span data-ttu-id="08560-2019">+0</span><span class="sxs-lookup"><span data-stu-id="08560-2019">+0</span></span>   | <span data-ttu-id="08560-2020">-0</span><span class="sxs-lookup"><span data-stu-id="08560-2020">-0</span></span>   | <span data-ttu-id="08560-2021">\+ INF</span><span class="sxs-lookup"><span data-stu-id="08560-2021">+inf</span></span> | <span data-ttu-id="08560-2022">– INF</span><span class="sxs-lookup"><span data-stu-id="08560-2022">-inf</span></span> | <span data-ttu-id="08560-2023">NaN</span><span class="sxs-lookup"><span data-stu-id="08560-2023">NaN</span></span> | 
   | <span data-ttu-id="08560-2024">x</span><span class="sxs-lookup"><span data-stu-id="08560-2024">x</span></span>    | <span data-ttu-id="08560-2025">z</span><span class="sxs-lookup"><span data-stu-id="08560-2025">z</span></span>    | <span data-ttu-id="08560-2026">x</span><span class="sxs-lookup"><span data-stu-id="08560-2026">x</span></span>    | <span data-ttu-id="08560-2027">x</span><span class="sxs-lookup"><span data-stu-id="08560-2027">x</span></span>    | <span data-ttu-id="08560-2028">– INF</span><span class="sxs-lookup"><span data-stu-id="08560-2028">-inf</span></span> | <span data-ttu-id="08560-2029">\+ INF</span><span class="sxs-lookup"><span data-stu-id="08560-2029">+inf</span></span> | <span data-ttu-id="08560-2030">NaN</span><span class="sxs-lookup"><span data-stu-id="08560-2030">NaN</span></span> | 
   | <span data-ttu-id="08560-2031">+0</span><span class="sxs-lookup"><span data-stu-id="08560-2031">+0</span></span>   | <span data-ttu-id="08560-2032">-y</span><span class="sxs-lookup"><span data-stu-id="08560-2032">-y</span></span>   | <span data-ttu-id="08560-2033">+0</span><span class="sxs-lookup"><span data-stu-id="08560-2033">+0</span></span>   | <span data-ttu-id="08560-2034">+0</span><span class="sxs-lookup"><span data-stu-id="08560-2034">+0</span></span>   | <span data-ttu-id="08560-2035">– INF</span><span class="sxs-lookup"><span data-stu-id="08560-2035">-inf</span></span> | <span data-ttu-id="08560-2036">\+ INF</span><span class="sxs-lookup"><span data-stu-id="08560-2036">+inf</span></span> | <span data-ttu-id="08560-2037">NaN</span><span class="sxs-lookup"><span data-stu-id="08560-2037">NaN</span></span> | 
   | <span data-ttu-id="08560-2038">-0</span><span class="sxs-lookup"><span data-stu-id="08560-2038">-0</span></span>   | <span data-ttu-id="08560-2039">-y</span><span class="sxs-lookup"><span data-stu-id="08560-2039">-y</span></span>   | <span data-ttu-id="08560-2040">-0</span><span class="sxs-lookup"><span data-stu-id="08560-2040">-0</span></span>   | <span data-ttu-id="08560-2041">+0</span><span class="sxs-lookup"><span data-stu-id="08560-2041">+0</span></span>   | <span data-ttu-id="08560-2042">– INF</span><span class="sxs-lookup"><span data-stu-id="08560-2042">-inf</span></span> | <span data-ttu-id="08560-2043">\+ INF</span><span class="sxs-lookup"><span data-stu-id="08560-2043">+inf</span></span> | <span data-ttu-id="08560-2044">NaN</span><span class="sxs-lookup"><span data-stu-id="08560-2044">NaN</span></span> | 
   | <span data-ttu-id="08560-2045">\+ INF</span><span class="sxs-lookup"><span data-stu-id="08560-2045">+inf</span></span> | <span data-ttu-id="08560-2046">\+ INF</span><span class="sxs-lookup"><span data-stu-id="08560-2046">+inf</span></span> | <span data-ttu-id="08560-2047">\+ INF</span><span class="sxs-lookup"><span data-stu-id="08560-2047">+inf</span></span> | <span data-ttu-id="08560-2048">\+ INF</span><span class="sxs-lookup"><span data-stu-id="08560-2048">+inf</span></span> | <span data-ttu-id="08560-2049">NaN</span><span class="sxs-lookup"><span data-stu-id="08560-2049">NaN</span></span>  | <span data-ttu-id="08560-2050">\+ INF</span><span class="sxs-lookup"><span data-stu-id="08560-2050">+inf</span></span> | <span data-ttu-id="08560-2051">NaN</span><span class="sxs-lookup"><span data-stu-id="08560-2051">NaN</span></span> | 
   | <span data-ttu-id="08560-2052">– INF</span><span class="sxs-lookup"><span data-stu-id="08560-2052">-inf</span></span> | <span data-ttu-id="08560-2053">– INF</span><span class="sxs-lookup"><span data-stu-id="08560-2053">-inf</span></span> | <span data-ttu-id="08560-2054">– INF</span><span class="sxs-lookup"><span data-stu-id="08560-2054">-inf</span></span> | <span data-ttu-id="08560-2055">– INF</span><span class="sxs-lookup"><span data-stu-id="08560-2055">-inf</span></span> | <span data-ttu-id="08560-2056">– INF</span><span class="sxs-lookup"><span data-stu-id="08560-2056">-inf</span></span> | <span data-ttu-id="08560-2057">NaN</span><span class="sxs-lookup"><span data-stu-id="08560-2057">NaN</span></span>  | <span data-ttu-id="08560-2058">NaN</span><span class="sxs-lookup"><span data-stu-id="08560-2058">NaN</span></span> | 
   | <span data-ttu-id="08560-2059">NaN</span><span class="sxs-lookup"><span data-stu-id="08560-2059">NaN</span></span>  | <span data-ttu-id="08560-2060">NaN</span><span class="sxs-lookup"><span data-stu-id="08560-2060">NaN</span></span>  | <span data-ttu-id="08560-2061">NaN</span><span class="sxs-lookup"><span data-stu-id="08560-2061">NaN</span></span>  | <span data-ttu-id="08560-2062">NaN</span><span class="sxs-lookup"><span data-stu-id="08560-2062">NaN</span></span>  | <span data-ttu-id="08560-2063">NaN</span><span class="sxs-lookup"><span data-stu-id="08560-2063">NaN</span></span>  | <span data-ttu-id="08560-2064">NaN</span><span class="sxs-lookup"><span data-stu-id="08560-2064">NaN</span></span>  | <span data-ttu-id="08560-2065">NaN</span><span class="sxs-lookup"><span data-stu-id="08560-2065">NaN</span></span> | 

*  <span data-ttu-id="08560-2066">Desetinné odčítání:</span><span class="sxs-lookup"><span data-stu-id="08560-2066">Decimal subtraction:</span></span>

   ```csharp
   decimal operator -(decimal x, decimal y);
   ```

   <span data-ttu-id="08560-2067">Pokud je výsledná hodnota příliš velká pro reprezentaci ve formátu `decimal`, je vyvolána `System.OverflowException`.</span><span class="sxs-lookup"><span data-stu-id="08560-2067">If the resulting value is too large to represent in the `decimal` format, a `System.OverflowException` is thrown.</span></span> <span data-ttu-id="08560-2068">Měřítko výsledku, před jakýmkoli zaokrouhlením, je větší z škály dvou operandů.</span><span class="sxs-lookup"><span data-stu-id="08560-2068">The scale of the result, before any rounding, is the larger of the scales of the two operands.</span></span>

   <span data-ttu-id="08560-2069">Desetinné odčítání je ekvivalentem použití operátoru odčítání typu `System.Decimal`.</span><span class="sxs-lookup"><span data-stu-id="08560-2069">Decimal subtraction is equivalent to using the subtraction operator of type `System.Decimal`.</span></span>

*  <span data-ttu-id="08560-2070">Odčítání výčtu.</span><span class="sxs-lookup"><span data-stu-id="08560-2070">Enumeration subtraction.</span></span> <span data-ttu-id="08560-2071">Každý typ výčtu implicitně poskytuje následující předdefinovaný operátor, kde `E` je typ výčtu a `U` je základní typ `E`:</span><span class="sxs-lookup"><span data-stu-id="08560-2071">Every enumeration type implicitly provides the following predefined operator, where `E` is the enum type, and `U` is the underlying type of `E`:</span></span>

   ```csharp
   U operator -(E x, E y);
   ```

   <span data-ttu-id="08560-2072">Tento operátor je vyhodnocen přesně jako `(U)((U)x - (U)y)`.</span><span class="sxs-lookup"><span data-stu-id="08560-2072">This operator is evaluated exactly as `(U)((U)x - (U)y)`.</span></span> <span data-ttu-id="08560-2073">Jinými slovy, operátor vypočítá rozdíl mezi ordinálními hodnotami `x` a `y`a typ výsledku je základní typ výčtu.</span><span class="sxs-lookup"><span data-stu-id="08560-2073">In other words, the operator computes the difference between the ordinal values of `x` and `y`, and the type of the result is the underlying type of the enumeration.</span></span>

   ```csharp
   E operator -(E x, U y);
   ```

   <span data-ttu-id="08560-2074">Tento operátor je vyhodnocen přesně jako `(E)((U)x - y)`.</span><span class="sxs-lookup"><span data-stu-id="08560-2074">This operator is evaluated exactly as `(E)((U)x - y)`.</span></span> <span data-ttu-id="08560-2075">Jinými slovy, operátor odečte hodnotu od základního typu výčtu a vrátí hodnotu výčtu.</span><span class="sxs-lookup"><span data-stu-id="08560-2075">In other words, the operator subtracts a value from the underlying type of the enumeration, yielding a value of the enumeration.</span></span>

*  <span data-ttu-id="08560-2076">Odebrání delegáta.</span><span class="sxs-lookup"><span data-stu-id="08560-2076">Delegate removal.</span></span> <span data-ttu-id="08560-2077">Každý typ delegáta implicitně poskytuje následující předdefinovaný operátor, kde `D` je typ delegáta:</span><span class="sxs-lookup"><span data-stu-id="08560-2077">Every delegate type implicitly provides the following predefined operator, where `D` is the delegate type:</span></span>

   ```csharp
   D operator -(D x, D y);
   ```

   <span data-ttu-id="08560-2078">Operátor binárního `-` provádí odebrání delegáta, pokud jsou oba operandy typu delegáta `D`.</span><span class="sxs-lookup"><span data-stu-id="08560-2078">The binary `-` operator performs delegate removal when both operands are of some delegate type `D`.</span></span> <span data-ttu-id="08560-2079">Pokud mají operandy různé typy delegátů, dojde k chybě při vazbě.</span><span class="sxs-lookup"><span data-stu-id="08560-2079">If the operands have different delegate types, a binding-time error occurs.</span></span> <span data-ttu-id="08560-2080">Pokud je první operand `null`, výsledek operace je `null`.</span><span class="sxs-lookup"><span data-stu-id="08560-2080">If the first operand is `null`, the result of the operation is `null`.</span></span> <span data-ttu-id="08560-2081">V opačném případě, pokud je druhý operand `null`, pak výsledek operace je hodnota prvního operandu.</span><span class="sxs-lookup"><span data-stu-id="08560-2081">Otherwise, if the second operand is `null`, then the result of the operation is the value of the first operand.</span></span> <span data-ttu-id="08560-2082">V opačném případě oba operandy reprezentují seznamy volání ([deklarace delegátů](delegates.md#delegate-declarations)), které mají jednu nebo více položek, a výsledkem je nový seznam vyvolání, který se skládá ze seznamu prvního operandu s odebranými položkami druhého operandu, za předpokladu, že seznam druhý operand je správným souvislým podseznamem prvního prvku.</span><span class="sxs-lookup"><span data-stu-id="08560-2082">Otherwise, both operands represent invocation lists ([Delegate declarations](delegates.md#delegate-declarations)) having one or more entries, and the result is a new invocation list consisting of the first operand's list with the second operand's entries removed from it, provided the second operand's list is a proper contiguous sublist of the first's.</span></span>     <span data-ttu-id="08560-2083">(Chcete-li určit rovnost podseznamu, odpovídající položky jsou porovnány s operátorem rovnosti delegáta ([operátory rovnosti delegátů](expressions.md#delegate-equality-operators)).) V opačném případě je výsledkem hodnota levého operandu.</span><span class="sxs-lookup"><span data-stu-id="08560-2083">(To determine sublist equality, corresponding entries are compared as for the delegate equality operator ([Delegate equality operators](expressions.md#delegate-equality-operators)).) Otherwise, the result is the value of the left operand.</span></span> <span data-ttu-id="08560-2084">V procesu nejsou změněny ani žádné seznamy operandů.</span><span class="sxs-lookup"><span data-stu-id="08560-2084">Neither of the operands' lists is changed in the process.</span></span> <span data-ttu-id="08560-2085">Pokud je v seznamu druhý operand shodný s více podseznamy souvislých položek v seznamu prvního operandu, je odebrán odpovídající podseznam sousedících položek.</span><span class="sxs-lookup"><span data-stu-id="08560-2085">If the second operand's list matches multiple sublists of contiguous entries in the first operand's list, the right-most matching sublist of contiguous entries is removed.</span></span> <span data-ttu-id="08560-2086">Pokud je výsledkem odebrání prázdný seznam, výsledek je `null`.</span><span class="sxs-lookup"><span data-stu-id="08560-2086">If removal results in an empty list, the result is `null`.</span></span> <span data-ttu-id="08560-2087">Příklad:</span><span class="sxs-lookup"><span data-stu-id="08560-2087">For example:</span></span>

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

## <a name="shift-operators"></a><span data-ttu-id="08560-2088">Operátory posunutí</span><span class="sxs-lookup"><span data-stu-id="08560-2088">Shift operators</span></span>

<span data-ttu-id="08560-2089">Operátory `<<` a `>>` se používají k provádění operací bitových posunutí.</span><span class="sxs-lookup"><span data-stu-id="08560-2089">The `<<` and `>>` operators are used to perform bit shifting operations.</span></span>

```antlr
shift_expression
    : additive_expression
    | shift_expression '<<' additive_expression
    | shift_expression right_shift additive_expression
    ;
```

<span data-ttu-id="08560-2090">Pokud má operand *shift_expression* typ doby kompilace `dynamic`, je výraz dynamicky svázán ([dynamická vazba](expressions.md#dynamic-binding)).</span><span class="sxs-lookup"><span data-stu-id="08560-2090">If an operand of a *shift_expression* has the compile-time type `dynamic`, then the expression is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)).</span></span> <span data-ttu-id="08560-2091">V tomto případě je typ výrazu kompilace `dynamic`a řešení popsané níže bude provedeno za běhu s použitím běhového typu u operandů, které mají typ doby kompilace `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="08560-2091">In this case the compile-time type of the expression is `dynamic`, and the resolution described below will take place at run-time using the run-time type of those operands that have the compile-time type `dynamic`.</span></span>

<span data-ttu-id="08560-2092">Pro vybírání implementace konkrétního operátora se použije pro účely operace formuláře `x << count` nebo `x >> count`, rozlišení přetížení binárního[operátoru](expressions.md#binary-operator-overload-resolution).</span><span class="sxs-lookup"><span data-stu-id="08560-2092">For an operation of the form `x << count` or `x >> count`, binary operator overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="08560-2093">Operandy jsou převedeny na typy parametrů vybraného operátoru a typ výsledku je návratový typ operátoru.</span><span class="sxs-lookup"><span data-stu-id="08560-2093">The operands are converted to the parameter types of the selected operator, and the type of the result is the return type of the operator.</span></span>

<span data-ttu-id="08560-2094">Při deklaraci přetíženého operátoru Shift musí být typ prvního operandu vždy třídou nebo strukturou obsahující deklaraci operátoru a typ druhého operandu musí být vždy `int`.</span><span class="sxs-lookup"><span data-stu-id="08560-2094">When declaring an overloaded shift operator, the type of the first operand must always be the class or struct containing the operator declaration, and the type of the second operand must always be `int`.</span></span>

<span data-ttu-id="08560-2095">Předdefinované operátory posunutí jsou uvedeny níže.</span><span class="sxs-lookup"><span data-stu-id="08560-2095">The predefined shift operators are listed below.</span></span>

*  <span data-ttu-id="08560-2096">Posunout doleva:</span><span class="sxs-lookup"><span data-stu-id="08560-2096">Shift left:</span></span>

   ```csharp
   int operator <<(int x, int count);
   uint operator <<(uint x, int count);
   long operator <<(long x, int count);
   ulong operator <<(ulong x, int count);
   ```

   <span data-ttu-id="08560-2097">Operátor `<<` posune `x` doleva o několik bitů vypočítaných podle popisu níže.</span><span class="sxs-lookup"><span data-stu-id="08560-2097">The `<<` operator shifts `x` left by a number of bits computed as described below.</span></span>

   <span data-ttu-id="08560-2098">Vysoké pořadí bitů mimo rozsah výsledného typu `x` jsou zahozeny, zbývající bity budou přesunuty doleva a dolní pořadí prázdných pozic je nastaveno na hodnotu nula.</span><span class="sxs-lookup"><span data-stu-id="08560-2098">The high-order bits outside the range of the result type of `x` are discarded, the remaining bits are shifted left, and the low-order empty bit positions are set to zero.</span></span>

*  <span data-ttu-id="08560-2099">Posun doprava:</span><span class="sxs-lookup"><span data-stu-id="08560-2099">Shift right:</span></span>

   ```csharp
   int operator >>(int x, int count);
   uint operator >>(uint x, int count);
   long operator >>(long x, int count);
   ulong operator >>(ulong x, int count);
   ```

   <span data-ttu-id="08560-2100">Operátor `>>` posune `x` doprava o několik bitů vypočítaných podle popisu níže.</span><span class="sxs-lookup"><span data-stu-id="08560-2100">The `>>` operator shifts `x` right by a number of bits computed as described below.</span></span>

   <span data-ttu-id="08560-2101">Pokud je `x` typu `int` nebo `long`, jsou nezáporné bity `x` zahozeny, zbývající bity se posunují vpravo a vysoké prázdné bitové pozice jsou nastaveny na hodnotu nula, pokud je `x` záporná, a nastavte na jednu, pokud je `x` záporné.</span><span class="sxs-lookup"><span data-stu-id="08560-2101">When `x` is of type `int` or `long`, the low-order bits of `x` are discarded, the remaining bits are shifted right, and the high-order empty bit positions are set to zero if `x` is non-negative and set to one if `x` is negative.</span></span>

   <span data-ttu-id="08560-2102">Pokud je `x` typu `uint` nebo `ulong`, jsou zahozeny dolních `x`ch bitů, zbývající bity se posunou vpravo a horní pozice prázdné bitové pozice je nastavena na hodnotu nula.</span><span class="sxs-lookup"><span data-stu-id="08560-2102">When `x` is of type `uint` or `ulong`, the low-order bits of `x` are discarded, the remaining bits are shifted right, and the high-order empty bit positions are set to zero.</span></span>

<span data-ttu-id="08560-2103">V případě předdefinovaných operátorů se počet bitů na posunutí vypočítá takto:</span><span class="sxs-lookup"><span data-stu-id="08560-2103">For the predefined operators, the number of bits to shift is computed as follows:</span></span>

*  <span data-ttu-id="08560-2104">Pokud je typ `x` `int` nebo `uint`, je počet posunu dán dolním pěti bity `count`.</span><span class="sxs-lookup"><span data-stu-id="08560-2104">When the type of `x` is `int` or `uint`, the shift count is given by the low-order five bits of `count`.</span></span> <span data-ttu-id="08560-2105">Jinými slovy, počet posunutí je vypočítán z `count & 0x1F`.</span><span class="sxs-lookup"><span data-stu-id="08560-2105">In other words, the shift count is computed from `count & 0x1F`.</span></span>
*  <span data-ttu-id="08560-2106">Pokud je typ `x` `long` nebo `ulong`, je počet posunu dán dolním počtem šesti bitů `count`.</span><span class="sxs-lookup"><span data-stu-id="08560-2106">When the type of `x` is `long` or `ulong`, the shift count is given by the low-order six bits of `count`.</span></span> <span data-ttu-id="08560-2107">Jinými slovy, počet posunutí je vypočítán z `count & 0x3F`.</span><span class="sxs-lookup"><span data-stu-id="08560-2107">In other words, the shift count is computed from `count & 0x3F`.</span></span>

<span data-ttu-id="08560-2108">Pokud je počet výsledných posunutí nula, operátory posunutí jednoduše vrátí hodnotu `x`.</span><span class="sxs-lookup"><span data-stu-id="08560-2108">If the resulting shift count is zero, the shift operators simply return the value of `x`.</span></span>

<span data-ttu-id="08560-2109">Operace posunutí nikdy nezpůsobí přetečení a vytvářejí stejné výsledky v `checked` a `unchecked` kontextech.</span><span class="sxs-lookup"><span data-stu-id="08560-2109">Shift operations never cause overflows and produce the same results in `checked` and `unchecked` contexts.</span></span>

<span data-ttu-id="08560-2110">Když je levý operand operátoru `>>` podepsaného integrálního typu, operátor provede aritmetické posunutí vpravo, ve kterém je hodnota nejvýznamnějšího bitu (bit znaménka) operandu rozšířena na prázdné bitové pozice v horním pořadí.</span><span class="sxs-lookup"><span data-stu-id="08560-2110">When the left operand of the `>>` operator is of a signed integral type, the operator performs an arithmetic shift right wherein the value of the most significant bit (the sign bit) of the operand is propagated to the high-order empty bit positions.</span></span> <span data-ttu-id="08560-2111">Je-li levý operand operátoru `>>` celočíselného typu bez znaménka, operátor provede logickou posunutí vpravo ve vysokém pořadí prázdné bitové pozice jsou vždy nastaveny na hodnotu nula.</span><span class="sxs-lookup"><span data-stu-id="08560-2111">When the left operand of the `>>` operator is of an unsigned integral type, the operator performs a logical shift right wherein high-order empty bit positions are always set to zero.</span></span> <span data-ttu-id="08560-2112">Chcete-li provést opačnou operaci, která je odvozena od typu operandu, lze použít explicitní přetypování.</span><span class="sxs-lookup"><span data-stu-id="08560-2112">To perform the opposite operation of that inferred from the operand type, explicit casts can be used.</span></span> <span data-ttu-id="08560-2113">Například pokud `x` je proměnná typu `int`, `unchecked((int)((uint)x >> y))` operace provede logický posun napravo od `x`.</span><span class="sxs-lookup"><span data-stu-id="08560-2113">For example, if `x` is a variable of type `int`, the operation `unchecked((int)((uint)x >> y))` performs a logical shift right of `x`.</span></span>

## <a name="relational-and-type-testing-operators"></a><span data-ttu-id="08560-2114">Relační operátory a operátory testování typů</span><span class="sxs-lookup"><span data-stu-id="08560-2114">Relational and type-testing operators</span></span>

<span data-ttu-id="08560-2115">`==`, `!=`, `<`, `>`, `<=`, `>=`, `is` a `as`, se nazývají relační operátory a operátory testování typu.</span><span class="sxs-lookup"><span data-stu-id="08560-2115">The `==`, `!=`, `<`, `>`, `<=`, `>=`, `is` and `as` operators are called the relational and type-testing operators.</span></span>

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

<span data-ttu-id="08560-2116">Operátor `is` je popsán v [operátoru is](expressions.md#the-is-operator) a operátor `as` je popsán v [operátoru as](expressions.md#the-as-operator).</span><span class="sxs-lookup"><span data-stu-id="08560-2116">The `is` operator is described in [The is operator](expressions.md#the-is-operator) and the `as` operator is described in [The as operator](expressions.md#the-as-operator).</span></span>

<span data-ttu-id="08560-2117">Operátory ***porovnávání***`==`, `!=`, `<`, `>`, `<=` a `>=`.</span><span class="sxs-lookup"><span data-stu-id="08560-2117">The `==`, `!=`, `<`, `>`, `<=` and `>=` operators are ***comparison operators***.</span></span>

<span data-ttu-id="08560-2118">Pokud má operand relačního operátora `dynamic`typ doby kompilace, pak je výraz dynamicky svázán ([dynamická vazba](expressions.md#dynamic-binding)).</span><span class="sxs-lookup"><span data-stu-id="08560-2118">If an operand of a comparison operator has the compile-time type `dynamic`, then the expression is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)).</span></span> <span data-ttu-id="08560-2119">V tomto případě je typ výrazu kompilace `dynamic`a řešení popsané níže bude provedeno za běhu s použitím běhového typu u operandů, které mají typ doby kompilace `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="08560-2119">In this case the compile-time type of the expression is `dynamic`, and the resolution described below will take place at run-time using the run-time type of those operands that have the compile-time type `dynamic`.</span></span>

<span data-ttu-id="08560-2120">Pro provoz formuláře `x` *op* `y`, kde *op* je operátor porovnání, je pro výběr konkrétní implementace operátoru použito rozlišení přetížení ([rozlišení přetěžování binárních operátorů](expressions.md#binary-operator-overload-resolution)).</span><span class="sxs-lookup"><span data-stu-id="08560-2120">For an operation of the form `x` *op* `y`, where *op* is a comparison operator, overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="08560-2121">Operandy jsou převedeny na typy parametrů vybraného operátoru a typ výsledku je návratový typ operátoru.</span><span class="sxs-lookup"><span data-stu-id="08560-2121">The operands are converted to the parameter types of the selected operator, and the type of the result is the return type of the operator.</span></span>

<span data-ttu-id="08560-2122">Předdefinované operátory porovnání jsou popsány v následujících částech.</span><span class="sxs-lookup"><span data-stu-id="08560-2122">The predefined comparison operators are described in the following sections.</span></span> <span data-ttu-id="08560-2123">Všechny předdefinované operátory porovnání vracejí výsledek typu `bool`, jak je popsáno v následující tabulce.</span><span class="sxs-lookup"><span data-stu-id="08560-2123">All predefined comparison operators return a result of type `bool`, as described in the following table.</span></span>


| <span data-ttu-id="08560-2124">__Operace__</span><span class="sxs-lookup"><span data-stu-id="08560-2124">__Operation__</span></span> | <span data-ttu-id="08560-2125">__výsledek__</span><span class="sxs-lookup"><span data-stu-id="08560-2125">__Result__</span></span>                                                       |
|---------------|------------------------------------------------------------------|
| `x == y`      | <span data-ttu-id="08560-2126">`true`, je-li `x` rovna `y``false` jinak</span><span class="sxs-lookup"><span data-stu-id="08560-2126">`true` if `x` is equal to `y`, `false` otherwise</span></span>                 | 
| `x != y`      | <span data-ttu-id="08560-2127">`true`, pokud `x` není rovno `y``false` jinak</span><span class="sxs-lookup"><span data-stu-id="08560-2127">`true` if `x` is not equal to `y`, `false` otherwise</span></span>             | 
| `x < y`       | <span data-ttu-id="08560-2128">`true`, je-li `x` menší než `y`, `false` jinak</span><span class="sxs-lookup"><span data-stu-id="08560-2128">`true` if `x` is less than `y`, `false` otherwise</span></span>                | 
| `x > y`       | <span data-ttu-id="08560-2129">`true`, pokud je `x` větší než `y`, `false` jinak</span><span class="sxs-lookup"><span data-stu-id="08560-2129">`true` if `x` is greater than `y`, `false` otherwise</span></span>             | 
| `x <= y`      | <span data-ttu-id="08560-2130">`true`, pokud `x` je menší nebo rovno `y``false` jinak</span><span class="sxs-lookup"><span data-stu-id="08560-2130">`true` if `x` is less than or equal to `y`, `false` otherwise</span></span>    | 
| `x >= y`      | <span data-ttu-id="08560-2131">`true`, pokud `x` je větší nebo rovno `y``false` jinak</span><span class="sxs-lookup"><span data-stu-id="08560-2131">`true` if `x` is greater than or equal to `y`, `false` otherwise</span></span> | 

### <a name="integer-comparison-operators"></a><span data-ttu-id="08560-2132">Operátory porovnání celých čísel</span><span class="sxs-lookup"><span data-stu-id="08560-2132">Integer comparison operators</span></span>

<span data-ttu-id="08560-2133">Předdefinované celočíselné operátory porovnání jsou:</span><span class="sxs-lookup"><span data-stu-id="08560-2133">The predefined integer comparison operators are:</span></span>
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

<span data-ttu-id="08560-2134">Každý z těchto operátorů porovná číselné hodnoty dvou celočíselných operandů a vrátí hodnotu `bool`, která označuje, zda je konkrétní vztah `true` nebo `false`.</span><span class="sxs-lookup"><span data-stu-id="08560-2134">Each of these operators compares the numeric values of the two integer operands and returns a `bool` value that indicates whether the particular relation is `true` or `false`.</span></span>

### <a name="floating-point-comparison-operators"></a><span data-ttu-id="08560-2135">Operátory porovnání s plovoucí desetinnou čárkou</span><span class="sxs-lookup"><span data-stu-id="08560-2135">Floating-point comparison operators</span></span>

<span data-ttu-id="08560-2136">Předdefinované operátory porovnání s plovoucí desetinnou čárkou jsou:</span><span class="sxs-lookup"><span data-stu-id="08560-2136">The predefined floating-point comparison operators are:</span></span>
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

<span data-ttu-id="08560-2137">Operátory porovnávají operandy podle pravidel standardu IEEE 754:</span><span class="sxs-lookup"><span data-stu-id="08560-2137">The operators compare the operands according to the rules of the IEEE 754 standard:</span></span>

*  <span data-ttu-id="08560-2138">Pokud je jeden z operandů NaN, výsledek je `false` pro všechny operátory s výjimkou `!=`, pro které je výsledek `true`.</span><span class="sxs-lookup"><span data-stu-id="08560-2138">If either operand is NaN, the result is `false` for all operators except `!=`, for which the result is `true`.</span></span> <span data-ttu-id="08560-2139">U všech dvou operandů `x != y` vždy vytvoří stejný výsledek jako `!(x == y)`.</span><span class="sxs-lookup"><span data-stu-id="08560-2139">For any two operands, `x != y` always produces the same result as `!(x == y)`.</span></span> <span data-ttu-id="08560-2140">Pokud je však jeden nebo oba operandy NaN, `<`, `>`, `<=`a `>=` operátory nevytváří stejné výsledky jako logickou negaci protějšího operátoru.</span><span class="sxs-lookup"><span data-stu-id="08560-2140">However, when one or both operands are NaN, the `<`, `>`, `<=`, and `>=` operators do not produce the same results as the logical negation of the opposite operator.</span></span> <span data-ttu-id="08560-2141">Například pokud je jedna z `x` a `y` NaN, pak `x < y` `false`, ale `!(x >= y)` je `true`.</span><span class="sxs-lookup"><span data-stu-id="08560-2141">For example, if either of `x` and `y` is NaN, then `x < y` is `false`, but `!(x >= y)` is `true`.</span></span>
*  <span data-ttu-id="08560-2142">Pokud žádný operand není NaN, operátory porovnávají hodnoty dvou operandů s plovoucí desetinnou čárkou s ohledem na řazení.</span><span class="sxs-lookup"><span data-stu-id="08560-2142">When neither operand is NaN, the operators compare the values of the two floating-point operands with respect to the ordering</span></span>

   ```csharp
   -inf < -max < ... < -min < -0.0 == +0.0 < +min < ... < +max < +inf
   ```

   <span data-ttu-id="08560-2143">kde `min` a `max` jsou nejmenší a největší kladné hodnoty, které lze reprezentovat v daném formátu s plovoucí desetinnou čárkou.</span><span class="sxs-lookup"><span data-stu-id="08560-2143">where `min` and `max` are the smallest and largest positive finite values that can be represented in the given floating-point format.</span></span> <span data-ttu-id="08560-2144">Mezi významné účinky tohoto řazení patří:</span><span class="sxs-lookup"><span data-stu-id="08560-2144">Notable effects of this ordering are:</span></span>
   * <span data-ttu-id="08560-2145">Záporné a kladné nuly se považují za stejné.</span><span class="sxs-lookup"><span data-stu-id="08560-2145">Negative and positive zeros are considered equal.</span></span>
   * <span data-ttu-id="08560-2146">Záporné nekonečno je považováno za méně než všechny ostatní hodnoty, ale je rovno jinému zápornému nekonečnu.</span><span class="sxs-lookup"><span data-stu-id="08560-2146">A negative infinity is considered less than all other values, but equal to another negative infinity.</span></span>
   * <span data-ttu-id="08560-2147">Kladné nekonečno je považováno za větší než všechny ostatní hodnoty, ale je rovno jinému kladnému nekonečnu.</span><span class="sxs-lookup"><span data-stu-id="08560-2147">A positive infinity is considered greater than all other values, but equal to another positive infinity.</span></span>

### <a name="decimal-comparison-operators"></a><span data-ttu-id="08560-2148">Operátory porovnávání desetinných míst</span><span class="sxs-lookup"><span data-stu-id="08560-2148">Decimal comparison operators</span></span>

<span data-ttu-id="08560-2149">Předdefinované operátory porovnávání desetinných míst jsou:</span><span class="sxs-lookup"><span data-stu-id="08560-2149">The predefined decimal comparison operators are:</span></span>
```csharp
bool operator ==(decimal x, decimal y);
bool operator !=(decimal x, decimal y);
bool operator <(decimal x, decimal y);
bool operator >(decimal x, decimal y);
bool operator <=(decimal x, decimal y);
bool operator >=(decimal x, decimal y);
```

<span data-ttu-id="08560-2150">Každý z těchto operátorů porovná číselné hodnoty dvou desetinných operandů a vrátí hodnotu `bool`, která označuje, zda je konkrétní vztah `true` nebo `false`.</span><span class="sxs-lookup"><span data-stu-id="08560-2150">Each of these operators compares the numeric values of the two decimal operands and returns a `bool` value that indicates whether the particular relation is `true` or `false`.</span></span> <span data-ttu-id="08560-2151">Každé desetinné porovnání je ekvivalentní s použitím odpovídajícího relačního nebo operátoru rovnosti typu `System.Decimal`.</span><span class="sxs-lookup"><span data-stu-id="08560-2151">Each decimal comparison is equivalent to using the corresponding relational or equality operator of type `System.Decimal`.</span></span>

### <a name="boolean-equality-operators"></a><span data-ttu-id="08560-2152">Logické operátory rovnosti</span><span class="sxs-lookup"><span data-stu-id="08560-2152">Boolean equality operators</span></span>

<span data-ttu-id="08560-2153">Předdefinované logické operátory rovnosti jsou:</span><span class="sxs-lookup"><span data-stu-id="08560-2153">The predefined boolean equality operators are:</span></span>
```csharp
bool operator ==(bool x, bool y);
bool operator !=(bool x, bool y);
```

<span data-ttu-id="08560-2154">Výsledek `==` je `true`, pokud `x` i `y` `true` nebo `x`.</span><span class="sxs-lookup"><span data-stu-id="08560-2154">The result of `==` is `true` if both `x` and `y` are `true` or if both `x` and `y` are `false`.</span></span> <span data-ttu-id="08560-2155">V opačném případě je výsledek `false`.</span><span class="sxs-lookup"><span data-stu-id="08560-2155">Otherwise, the result is `false`.</span></span>

<span data-ttu-id="08560-2156">Výsledek `!=` je `false`, pokud `x` i `y` `true` nebo `x`.</span><span class="sxs-lookup"><span data-stu-id="08560-2156">The result of `!=` is `false` if both `x` and `y` are `true` or if both `x` and `y` are `false`.</span></span> <span data-ttu-id="08560-2157">V opačném případě je výsledek `true`.</span><span class="sxs-lookup"><span data-stu-id="08560-2157">Otherwise, the result is `true`.</span></span> <span data-ttu-id="08560-2158">Pokud jsou operandy typu `bool`, operátor `!=` vytvoří stejný výsledek jako operátor `^`.</span><span class="sxs-lookup"><span data-stu-id="08560-2158">When the operands are of type `bool`, the `!=` operator produces the same result as the `^` operator.</span></span>

### <a name="enumeration-comparison-operators"></a><span data-ttu-id="08560-2159">Operátory porovnání výčtu</span><span class="sxs-lookup"><span data-stu-id="08560-2159">Enumeration comparison operators</span></span>

<span data-ttu-id="08560-2160">Každý typ výčtu implicitně poskytuje následující předdefinované operátory porovnání:</span><span class="sxs-lookup"><span data-stu-id="08560-2160">Every enumeration type implicitly provides the following predefined comparison operators:</span></span>
```csharp
bool operator ==(E x, E y);
bool operator !=(E x, E y);
bool operator <(E x, E y);
bool operator >(E x, E y);
bool operator <=(E x, E y);
bool operator >=(E x, E y);
```

<span data-ttu-id="08560-2161">Výsledek vyhodnocení `x op y`, kde `x` a `y` jsou výrazy typu výčtu `E` s podkladovým typem `U`a `op` je jedním z relačních operátorů, je přesně stejný jako vyhodnocení `((U)x) op ((U)y)`.</span><span class="sxs-lookup"><span data-stu-id="08560-2161">The result of evaluating `x op y`, where `x` and `y` are expressions of an enumeration type `E` with an underlying type `U`, and `op` is one of the comparison operators, is exactly the same as evaluating `((U)x) op ((U)y)`.</span></span> <span data-ttu-id="08560-2162">Jinými slovy, operátory porovnání typu výčtu jednoduše porovnávají základní celočíselné hodnoty dvou operandů.</span><span class="sxs-lookup"><span data-stu-id="08560-2162">In other words, the enumeration type comparison operators simply compare the underlying integral values of the two operands.</span></span>

### <a name="reference-type-equality-operators"></a><span data-ttu-id="08560-2163">Operátory rovnosti typu odkazu</span><span class="sxs-lookup"><span data-stu-id="08560-2163">Reference type equality operators</span></span>

<span data-ttu-id="08560-2164">Předdefinované operátory rovnosti referenčního typu jsou:</span><span class="sxs-lookup"><span data-stu-id="08560-2164">The predefined reference type equality operators are:</span></span>
```csharp
bool operator ==(object x, object y);
bool operator !=(object x, object y);
```

<span data-ttu-id="08560-2165">Operátory vrátí výsledek porovnání dvou odkazů pro rovnost nebo nerovnost.</span><span class="sxs-lookup"><span data-stu-id="08560-2165">The operators return the result of comparing the two references for equality or non-equality.</span></span>

<span data-ttu-id="08560-2166">Vzhledem k tomu, že předdefinované operátory rovnosti typu odkazu přijímají operandy typu `object`, vztahují se na všechny typy, které nedeklarují příslušné `operator ==` a `operator !=` členy.</span><span class="sxs-lookup"><span data-stu-id="08560-2166">Since the predefined reference type equality operators accept operands of type `object`, they apply to all types that do not declare applicable `operator ==` and `operator !=` members.</span></span> <span data-ttu-id="08560-2167">Naopak všechny použitelné uživatelsky definované operátory rovnosti efektivně skryjí předdefinované operátory rovnosti typu odkazu.</span><span class="sxs-lookup"><span data-stu-id="08560-2167">Conversely, any applicable user-defined equality operators effectively hide the predefined reference type equality operators.</span></span>

<span data-ttu-id="08560-2168">Předdefinované operátory rovnosti referenčního typu vyžadují jednu z následujících možností:</span><span class="sxs-lookup"><span data-stu-id="08560-2168">The predefined reference type equality operators require one of the following:</span></span>

*  <span data-ttu-id="08560-2169">Oba operandy jsou hodnota typu, který je známý jako *reference_type* nebo `null`literálů.</span><span class="sxs-lookup"><span data-stu-id="08560-2169">Both operands are a value of a type known to be a *reference_type* or the literal `null`.</span></span> <span data-ttu-id="08560-2170">Kromě toho existuje explicitní převod odkazu ([explicitní převody odkazů](conversions.md#explicit-reference-conversions)) z typu jednoho operandu na typ druhého operandu.</span><span class="sxs-lookup"><span data-stu-id="08560-2170">Furthermore, an explicit reference conversion ([Explicit reference conversions](conversions.md#explicit-reference-conversions)) exists from the type of either operand to the type of the other operand.</span></span>
*  <span data-ttu-id="08560-2171">Jeden operand je hodnota typu `T`, kde `T` je *type_parameter* a druhý operand je literální `null`.</span><span class="sxs-lookup"><span data-stu-id="08560-2171">One operand is a value of type `T` where `T` is a *type_parameter* and the other operand is the literal `null`.</span></span> <span data-ttu-id="08560-2172">Navíc `T` nemá omezení typu hodnoty.</span><span class="sxs-lookup"><span data-stu-id="08560-2172">Furthermore `T` does not have the value type constraint.</span></span>

<span data-ttu-id="08560-2173">Pokud jedna z těchto podmínek není pravdivá, dojde k chybě při vazbě.</span><span class="sxs-lookup"><span data-stu-id="08560-2173">Unless one of these conditions are true, a binding-time error occurs.</span></span> <span data-ttu-id="08560-2174">Mezi významné důsledky těchto pravidel patří:</span><span class="sxs-lookup"><span data-stu-id="08560-2174">Notable implications of these rules are:</span></span>

*  <span data-ttu-id="08560-2175">Jedná se o chybu při vazbě k použití předdefinovaného operátoru rovnosti referenčního typu pro porovnání dvou odkazů, které se označují jako odlišné v době vytváření vazby.</span><span class="sxs-lookup"><span data-stu-id="08560-2175">It is a binding-time error to use the predefined reference type equality operators to compare two references that are known to be different at binding-time.</span></span> <span data-ttu-id="08560-2176">Například pokud jsou typy v čase vazby operandy dva typy třídy `A` a `B`a pokud není `A` ani `B` odvozeny od druhé, pak by nebylo možné, aby dva operandy odkazovaly na stejný objekt.</span><span class="sxs-lookup"><span data-stu-id="08560-2176">For example, if the binding-time types of the operands are two class types `A` and `B`, and if neither `A` nor `B` derives from the other, then it would be impossible for the two operands to reference the same object.</span></span> <span data-ttu-id="08560-2177">Proto se operace považuje za chybu při vazbě.</span><span class="sxs-lookup"><span data-stu-id="08560-2177">Thus, the operation is considered a binding-time error.</span></span>
*  <span data-ttu-id="08560-2178">Předdefinované operátory rovnosti referenčního typu nepovolují porovnání operandů typu hodnoty.</span><span class="sxs-lookup"><span data-stu-id="08560-2178">The predefined reference type equality operators do not permit value type operands to be compared.</span></span> <span data-ttu-id="08560-2179">Proto pokud typ struktury deklaruje své vlastní operátory rovnosti, není možné porovnat hodnoty tohoto typu struktury.</span><span class="sxs-lookup"><span data-stu-id="08560-2179">Therefore, unless a struct type declares its own equality operators, it is not possible to compare values of that struct type.</span></span>
*  <span data-ttu-id="08560-2180">Předdefinované operátory rovnosti typu odkazu nikdy nezpůsobí operace zabalení pro své operandy.</span><span class="sxs-lookup"><span data-stu-id="08560-2180">The predefined reference type equality operators never cause boxing operations to occur for their operands.</span></span> <span data-ttu-id="08560-2181">To by znamenalo, že by se tyto operace zabalení prováděly, protože odkazy na nově přiřazené zabalené instance by se nutně lišily od všech ostatních odkazů.</span><span class="sxs-lookup"><span data-stu-id="08560-2181">It would be meaningless to perform such boxing operations, since references to the newly allocated boxed instances would necessarily differ from all other references.</span></span>
*  <span data-ttu-id="08560-2182">Pokud je operand typu parametru typu `T` porovnána s `null`a typ `T` za běhu je typ hodnoty, výsledek porovnání je `false`.</span><span class="sxs-lookup"><span data-stu-id="08560-2182">If an operand of a type parameter type `T` is compared to `null`, and the run-time type of `T` is a value type, the result of the comparison is `false`.</span></span>

<span data-ttu-id="08560-2183">Následující příklad ověří, zda je argument typu parametru bez omezení typu `null`.</span><span class="sxs-lookup"><span data-stu-id="08560-2183">The following example checks whether an argument of an unconstrained type parameter type is `null`.</span></span>
```csharp
class C<T>
{
    void F(T x) {
        if (x == null) throw new ArgumentNullException();
        ...
    }
}
```

<span data-ttu-id="08560-2184">`x == null` konstrukce je povolena, i když `T` může představovat typ hodnoty a výsledek je jednoduše definován tak, aby byl `false`, pokud `T` je typ hodnoty.</span><span class="sxs-lookup"><span data-stu-id="08560-2184">The `x == null` construct is permitted even though `T` could represent a value type, and the result is simply defined to be `false` when `T` is a value type.</span></span>

<span data-ttu-id="08560-2185">Pro provoz formuláře `x == y` nebo `x != y`platí, že pokud existují příslušné `operator ==` nebo `operator !=`, pravidla pro rozlišení přetížení operátoru ([řešení přetížení binárního operátoru](expressions.md#binary-operator-overload-resolution)) vybere tento operátor místo předdefinovaného operátoru rovnosti typu odkazu.</span><span class="sxs-lookup"><span data-stu-id="08560-2185">For an operation of the form `x == y` or `x != y`, if any applicable `operator ==` or `operator !=` exists, the operator overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) rules will select that operator instead of the predefined reference type equality operator.</span></span> <span data-ttu-id="08560-2186">Je však vždy možné vybrat předdefinovaný operátor rovnosti typu odkazu explicitním přetypováním jednoho nebo obou operandů na typ `object`.</span><span class="sxs-lookup"><span data-stu-id="08560-2186">However, it is always possible to select the predefined reference type equality operator by explicitly casting one or both of the operands to type `object`.</span></span> <span data-ttu-id="08560-2187">Příklad</span><span class="sxs-lookup"><span data-stu-id="08560-2187">The example</span></span>
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
<span data-ttu-id="08560-2188">Vytvoří výstup</span><span class="sxs-lookup"><span data-stu-id="08560-2188">produces the output</span></span>
```console
True
False
False
False
```

<span data-ttu-id="08560-2189">Proměnné `s` a `t` odkazují na dvě odlišné instance `string`, které obsahují stejné znaky.</span><span class="sxs-lookup"><span data-stu-id="08560-2189">The `s` and `t` variables refer to two distinct `string` instances containing the same characters.</span></span> <span data-ttu-id="08560-2190">První porovnávací výstup `True`, protože je vybrán předdefinovaný operátor rovnosti řetězců ([operátory rovnosti řetězců](expressions.md#string-equality-operators)), pokud jsou oba operandy typu `string`.</span><span class="sxs-lookup"><span data-stu-id="08560-2190">The first comparison outputs `True` because the predefined string equality operator ([String equality operators](expressions.md#string-equality-operators)) is selected when both operands are of type `string`.</span></span> <span data-ttu-id="08560-2191">Zbývající porovnávání všech výstupů `False`, protože jeden nebo oba operandy jsou typu `object`, pokud je vybrán operátor rovnosti typu odkazu.</span><span class="sxs-lookup"><span data-stu-id="08560-2191">The remaining comparisons all output `False` because the predefined reference type equality operator is selected when one or both of the operands are of type `object`.</span></span>

<span data-ttu-id="08560-2192">Všimněte si, že výše uvedená technika není smysluplná pro typy hodnot.</span><span class="sxs-lookup"><span data-stu-id="08560-2192">Note that the above technique is not meaningful for value types.</span></span> <span data-ttu-id="08560-2193">Příklad</span><span class="sxs-lookup"><span data-stu-id="08560-2193">The example</span></span>
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
<span data-ttu-id="08560-2194">výstupy `False`, protože přetypování vytvoří odkazy na dvě samostatné instance zabalené `int` hodnoty.</span><span class="sxs-lookup"><span data-stu-id="08560-2194">outputs `False` because the casts create references to two separate instances of boxed `int` values.</span></span>

### <a name="string-equality-operators"></a><span data-ttu-id="08560-2195">Operátory rovnosti řetězců</span><span class="sxs-lookup"><span data-stu-id="08560-2195">String equality operators</span></span>

<span data-ttu-id="08560-2196">Předdefinované řetězcové operátory rovnosti jsou:</span><span class="sxs-lookup"><span data-stu-id="08560-2196">The predefined string equality operators are:</span></span>
```csharp
bool operator ==(string x, string y);
bool operator !=(string x, string y);
```

<span data-ttu-id="08560-2197">Dvě `string` hodnoty jsou považovány za stejné, pokud je splněna jedna z následujících hodnot:</span><span class="sxs-lookup"><span data-stu-id="08560-2197">Two `string` values are considered equal when one of the following is true:</span></span>

*  <span data-ttu-id="08560-2198">Obě hodnoty jsou `null`.</span><span class="sxs-lookup"><span data-stu-id="08560-2198">Both values are `null`.</span></span>
*  <span data-ttu-id="08560-2199">Obě hodnoty jsou odkazy, které nejsou null, na instance řetězců, které mají stejné délky a stejné znaky v každé pozici znaku.</span><span class="sxs-lookup"><span data-stu-id="08560-2199">Both values are non-null references to string instances that have identical lengths and identical characters in each character position.</span></span>

<span data-ttu-id="08560-2200">Operátory rovnosti řetězců porovnávají řetězcové hodnoty, nikoli odkazy na řetězce.</span><span class="sxs-lookup"><span data-stu-id="08560-2200">The string equality operators compare string values rather than string references.</span></span> <span data-ttu-id="08560-2201">Pokud dvě samostatné instance řetězců obsahují přesně stejnou sekvenci znaků, jsou hodnoty řetězců stejné, ale odkazy se liší.</span><span class="sxs-lookup"><span data-stu-id="08560-2201">When two separate string instances contain the exact same sequence of characters, the values of the strings are equal, but the references are different.</span></span> <span data-ttu-id="08560-2202">Jak je popsáno v tématu [operátory rovnosti typu odkazu](expressions.md#reference-type-equality-operators), lze operátory rovnosti typu odkazu použít k porovnání odkazů na řetězce namísto řetězcových hodnot.</span><span class="sxs-lookup"><span data-stu-id="08560-2202">As described in [Reference type equality operators](expressions.md#reference-type-equality-operators), the reference type equality operators can be used to compare string references instead of string values.</span></span>

### <a name="delegate-equality-operators"></a><span data-ttu-id="08560-2203">Operátory rovnosti delegátů</span><span class="sxs-lookup"><span data-stu-id="08560-2203">Delegate equality operators</span></span>

<span data-ttu-id="08560-2204">Každý typ delegáta implicitně poskytuje následující předdefinované operátory porovnání:</span><span class="sxs-lookup"><span data-stu-id="08560-2204">Every delegate type implicitly provides the following predefined comparison operators:</span></span>

```csharp
bool operator ==(System.Delegate x, System.Delegate y);
bool operator !=(System.Delegate x, System.Delegate y);
```

<span data-ttu-id="08560-2205">Dvě instance delegáta jsou považovány za stejné jako následující:</span><span class="sxs-lookup"><span data-stu-id="08560-2205">Two delegate instances are considered equal as follows:</span></span>

*  <span data-ttu-id="08560-2206">Pokud je jedna z instancí delegátů `null`, jsou shodné pouze v případě, že jsou `null`.</span><span class="sxs-lookup"><span data-stu-id="08560-2206">If either of the delegate instances is `null`, they are equal if and only if both are `null`.</span></span>
*  <span data-ttu-id="08560-2207">Pokud mají delegáty jiný typ běhu, nejsou nikdy rovny.</span><span class="sxs-lookup"><span data-stu-id="08560-2207">If the delegates have different run-time type they are never equal.</span></span>
*  <span data-ttu-id="08560-2208">Pokud obě instance delegáta mají seznam volání ([deklarace delegátů](delegates.md#delegate-declarations)), jsou tyto instance stejné, pokud a mají pouze v případě, že jejich seznamy vyvolání mají stejnou délku, a každá položka v seznamu volání v jednom se rovná (jak je definováno níže) na odpovídající položku v seznamu vyvolání.</span><span class="sxs-lookup"><span data-stu-id="08560-2208">If both of the delegate instances have an invocation list ([Delegate declarations](delegates.md#delegate-declarations)), those instances are equal if and only if their invocation lists are the same length, and each entry in one's invocation list is equal (as defined below) to the corresponding entry, in order, in the other's invocation list.</span></span>

<span data-ttu-id="08560-2209">Následující pravidla určují rovnost položek seznamu volání:</span><span class="sxs-lookup"><span data-stu-id="08560-2209">The following rules govern the equality of invocation list entries:</span></span>

*  <span data-ttu-id="08560-2210">Pokud dvě položky seznamu volání odkazují na stejnou statickou metodu, pak jsou položky stejné.</span><span class="sxs-lookup"><span data-stu-id="08560-2210">If two invocation list entries both refer to the same static method then the entries are equal.</span></span>
*  <span data-ttu-id="08560-2211">Pokud dvě položky seznamu volání odkazují na stejnou nestatickou metodu na stejném cílovém objektu (jak je definováno operátory rovnosti referencí), pak jsou položky stejné.</span><span class="sxs-lookup"><span data-stu-id="08560-2211">If two invocation list entries both refer to the same non-static method on the same target object (as defined by the reference equality operators) then the entries are equal.</span></span>
*  <span data-ttu-id="08560-2212">Položky seznamu volání vytvořené z vyhodnocení sémanticky identické *anonymous_method_expression*s nebo *lambda_expression*s stejnou (možná prázdnou) sadou zachycených vnějších proměnných jsou povolené (ale nevyžadují se), aby se rovnaly.</span><span class="sxs-lookup"><span data-stu-id="08560-2212">Invocation list entries produced from evaluation of semantically identical *anonymous_method_expression*s or *lambda_expression*s with the same (possibly empty) set of captured outer variable instances are permitted (but not required) to be equal.</span></span>

### <a name="equality-operators-and-null"></a><span data-ttu-id="08560-2213">Operátory rovnosti a hodnota null</span><span class="sxs-lookup"><span data-stu-id="08560-2213">Equality operators and null</span></span>

<span data-ttu-id="08560-2214">Operátory `==` a `!=` umožňují, aby jeden operand byl hodnota typu s možnou hodnotou null a druhý pro `null` literálu, i když pro tuto operaci neexistuje žádný předdefinovaný operátor nebo uživatelem definovaný operátor (v vyzdvižené nebo nezatížené formě).</span><span class="sxs-lookup"><span data-stu-id="08560-2214">The `==` and `!=` operators permit one operand to be a value of a nullable type and the other to be the `null` literal, even if no predefined or user-defined operator (in unlifted or lifted form) exists for the operation.</span></span>

<span data-ttu-id="08560-2215">Pro provoz jedné z forem</span><span class="sxs-lookup"><span data-stu-id="08560-2215">For an operation of one of the forms</span></span>
```csharp
x == null
null == x
x != null
null != x
```
<span data-ttu-id="08560-2216">kde `x` je výraz typu s možnou hodnotou null, pokud rozlišení přetížení operátoru ([rozlišení přetížení binárního operátoru](expressions.md#binary-operator-overload-resolution)) nenajde příslušný operátor, výsledek je místo toho vypočítán z vlastnosti `HasValue` `x`.</span><span class="sxs-lookup"><span data-stu-id="08560-2216">where `x` is an expression of a nullable type, if operator overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) fails to find an applicable operator, the result is instead computed from the `HasValue` property of `x`.</span></span> <span data-ttu-id="08560-2217">Konkrétně jsou první dva formuláře přeloženy do `!x.HasValue`a poslední dva formuláře jsou přeloženy do `x.HasValue`.</span><span class="sxs-lookup"><span data-stu-id="08560-2217">Specifically, the first two forms are translated into `!x.HasValue`, and last two forms are translated into `x.HasValue`.</span></span>

### <a name="the-is-operator"></a><span data-ttu-id="08560-2218">Operátor is</span><span class="sxs-lookup"><span data-stu-id="08560-2218">The is operator</span></span>

<span data-ttu-id="08560-2219">Operátor `is` slouží k dynamické kontrole, zda je běhový typ objektu kompatibilní s daným typem.</span><span class="sxs-lookup"><span data-stu-id="08560-2219">The `is` operator is used to dynamically check if the run-time type of an object is compatible with a given type.</span></span> <span data-ttu-id="08560-2220">Výsledek operace `E is T`, kde `E` je výraz a `T` je typ, je logická hodnota označující, zda `E` lze úspěšně převést na typ `T` prostřednictvím převodu odkazu, převodu zabalení nebo převodu rozbalení.</span><span class="sxs-lookup"><span data-stu-id="08560-2220">The result of the operation `E is T`, where `E` is an expression and `T` is a type, is a boolean value indicating whether `E` can successfully be converted to type `T` by a reference conversion, a boxing conversion, or an unboxing conversion.</span></span> <span data-ttu-id="08560-2221">Tato operace je vyhodnocena následujícím způsobem, poté, co byly argumenty typu nahrazeny pro všechny parametry typu:</span><span class="sxs-lookup"><span data-stu-id="08560-2221">The operation is evaluated as follows, after type arguments have been substituted for all type parameters:</span></span>

*  <span data-ttu-id="08560-2222">Pokud je `E` anonymní funkce, dojde k chybě při kompilaci.</span><span class="sxs-lookup"><span data-stu-id="08560-2222">If `E` is an anonymous function, a compile-time error occurs</span></span>
*  <span data-ttu-id="08560-2223">Pokud `E` je skupina metod nebo literál `null`, pokud je typ `E` odkazový typ nebo typ s možnou hodnotou null a hodnota `E` je null, výsledek je false.</span><span class="sxs-lookup"><span data-stu-id="08560-2223">If `E` is a method group or the `null` literal, of if the type of `E` is a reference type or a nullable type and the value of `E` is null, the result is false.</span></span>
*  <span data-ttu-id="08560-2224">V opačném případě nechejte `D` reprezentovat dynamický typ `E` následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="08560-2224">Otherwise, let `D` represent the dynamic type of `E` as follows:</span></span>
   * <span data-ttu-id="08560-2225">Pokud typ `E` je odkazový typ, `D` je běhový typ odkazu instance pomocí `E`.</span><span class="sxs-lookup"><span data-stu-id="08560-2225">If the type of `E` is a reference type, `D` is the run-time type of the instance reference by `E`.</span></span>
   * <span data-ttu-id="08560-2226">Pokud typ `E` je typ s možnou hodnotou null, `D` je základní typ tohoto typu s možnou hodnotou null.</span><span class="sxs-lookup"><span data-stu-id="08560-2226">If the type of `E` is a nullable type, `D` is the underlying type of that nullable type.</span></span>
   * <span data-ttu-id="08560-2227">Pokud typ `E` je typ hodnoty, která není null, `D` je typ `E`.</span><span class="sxs-lookup"><span data-stu-id="08560-2227">If the type of `E` is a non-nullable value type, `D` is the type of `E`.</span></span>
*  <span data-ttu-id="08560-2228">Výsledek operace závisí na `D` a `T` následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="08560-2228">The result of the operation depends on `D` and `T` as follows:</span></span>
   * <span data-ttu-id="08560-2229">Pokud je `T` odkazový typ, výsledek je true, pokud `D` a `T` je stejný typ, pokud `D` je odkazový typ a implicitní převod odkazu z `D` na `T` existuje nebo pokud `D` je typ hodnoty a převod zabalení z `D` na `T` existuje.</span><span class="sxs-lookup"><span data-stu-id="08560-2229">If `T` is a reference type, the result is true if `D` and `T` are the same type, if `D` is a reference type and an implicit reference conversion from `D` to `T` exists, or if `D` is a value type and a boxing conversion from `D` to `T` exists.</span></span>
   * <span data-ttu-id="08560-2230">Pokud `T` je typ s možnou hodnotou null, výsledek je true, pokud `D` je základní typ `T`.</span><span class="sxs-lookup"><span data-stu-id="08560-2230">If `T` is a nullable type, the result is true if `D` is the underlying type of `T`.</span></span>
   * <span data-ttu-id="08560-2231">Pokud `T` typ hodnoty, která není null, výsledek je true, pokud `D` a `T` stejný typ.</span><span class="sxs-lookup"><span data-stu-id="08560-2231">If `T` is a non-nullable value type, the result is true if `D` and `T` are the same type.</span></span>
   * <span data-ttu-id="08560-2232">V opačném případě je výsledkem false.</span><span class="sxs-lookup"><span data-stu-id="08560-2232">Otherwise, the result is false.</span></span>

<span data-ttu-id="08560-2233">Všimněte si, že operátor `is` nebere v úvahu uživatelem definované převody.</span><span class="sxs-lookup"><span data-stu-id="08560-2233">Note that user defined conversions, are not considered by the `is` operator.</span></span>

### <a name="the-as-operator"></a><span data-ttu-id="08560-2234">Operátor as</span><span class="sxs-lookup"><span data-stu-id="08560-2234">The as operator</span></span>

<span data-ttu-id="08560-2235">Operátor `as` slouží k explicitnímu převodu hodnoty na daný typ odkazu nebo na typ s možnou hodnotou null.</span><span class="sxs-lookup"><span data-stu-id="08560-2235">The `as` operator is used to explicitly convert a value to a given reference type or nullable type.</span></span> <span data-ttu-id="08560-2236">Na rozdíl od výrazu přetypování ([výrazy přetypování](expressions.md#cast-expressions)) operátor `as` nikdy nevyvolává výjimku.</span><span class="sxs-lookup"><span data-stu-id="08560-2236">Unlike a cast expression ([Cast expressions](expressions.md#cast-expressions)), the `as` operator never throws an exception.</span></span> <span data-ttu-id="08560-2237">Namísto toho, pokud uvedený převod není možný, je výsledná hodnota `null`.</span><span class="sxs-lookup"><span data-stu-id="08560-2237">Instead, if the indicated conversion is not possible, the resulting value is `null`.</span></span>

<span data-ttu-id="08560-2238">V operaci formuláře `E as T`musí být `E` výrazem a `T` musí být odkazový typ, parametr typu, který je známý jako typ odkazu, nebo typ s možnou hodnotou null.</span><span class="sxs-lookup"><span data-stu-id="08560-2238">In an operation of the form `E as T`, `E` must be an expression and `T` must be a reference type, a type parameter known to be a reference type, or a nullable type.</span></span> <span data-ttu-id="08560-2239">Kromě toho musí být alespoň jedna z následujících podmínek pravdivá nebo jinak dojde k chybě při kompilaci:</span><span class="sxs-lookup"><span data-stu-id="08560-2239">Furthermore, at least one of the following must be true, or otherwise a compile-time error occurs:</span></span>

*  <span data-ttu-id="08560-2240">Identita ([převod identity](conversions.md#identity-conversion)), implicitně připouštějící hodnotu null (implicitní[převody s možnou hodnotou null](conversions.md#implicit-nullable-conversions)), implicitní odkaz ([implicitní převody odkazů](conversions.md#implicit-reference-conversions)), zabalení ([převody zabalení](conversions.md#boxing-conversions)), explicitní připouštějící hodnotu null ([explicitní převody s možnou hodnotou null](conversions.md#explicit-nullable-conversions)), explicitní odkaz ([explicitní převody odkazů](conversions.md#explicit-reference-conversions)) nebo rozbalení (převody `T`na[rozbalení](conversions.md#unboxing-conversions)) existují z `E`</span><span class="sxs-lookup"><span data-stu-id="08560-2240">An identity ([Identity conversion](conversions.md#identity-conversion)), implicit nullable ([Implicit nullable conversions](conversions.md#implicit-nullable-conversions)), implicit reference ([Implicit reference conversions](conversions.md#implicit-reference-conversions)), boxing ([Boxing conversions](conversions.md#boxing-conversions)), explicit nullable ([Explicit nullable conversions](conversions.md#explicit-nullable-conversions)), explicit reference ([Explicit reference conversions](conversions.md#explicit-reference-conversions)), or unboxing ([Unboxing conversions](conversions.md#unboxing-conversions)) conversion exists from `E` to `T`.</span></span>
*  <span data-ttu-id="08560-2241">Typ `E` nebo `T` je otevřený typ.</span><span class="sxs-lookup"><span data-stu-id="08560-2241">The type of `E` or `T` is an open type.</span></span>
*  <span data-ttu-id="08560-2242">`E` je `null` literál.</span><span class="sxs-lookup"><span data-stu-id="08560-2242">`E` is the `null` literal.</span></span>

<span data-ttu-id="08560-2243">Pokud typ `E` doby kompilace není `dynamic`, operace `E as T` vytvoří stejný výsledek jako</span><span class="sxs-lookup"><span data-stu-id="08560-2243">If the compile-time type of `E` is not `dynamic`, the operation `E as T` produces the same result as</span></span>
```csharp
E is T ? (T)(E) : (T)null
```
<span data-ttu-id="08560-2244">s výjimkou `E` je vyhodnocena pouze jednou.</span><span class="sxs-lookup"><span data-stu-id="08560-2244">except that `E` is only evaluated once.</span></span> <span data-ttu-id="08560-2245">Kompilátor je možné očekávat pro optimalizaci `E as T` k provedení maximálně jedné kontroly dynamického typu, a to na rozdíl od dvou kontrol dynamického typu, které jsou odvozené od výše uvedeného rozšíření.</span><span class="sxs-lookup"><span data-stu-id="08560-2245">The compiler can be expected to optimize `E as T` to perform at most one dynamic type check as opposed to the two dynamic type checks implied by the expansion above.</span></span>

<span data-ttu-id="08560-2246">Pokud je typ doby kompilace `E` `dynamic`, na rozdíl od operátoru přetypování není operátor `as` dynamicky svázán ([dynamická vazba](expressions.md#dynamic-binding)).</span><span class="sxs-lookup"><span data-stu-id="08560-2246">If the compile-time type of `E` is `dynamic`, unlike the cast operator the `as` operator is not dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)).</span></span> <span data-ttu-id="08560-2247">Proto rozšíření v tomto případě je:</span><span class="sxs-lookup"><span data-stu-id="08560-2247">Therefore the expansion in this case is:</span></span>
```csharp
E is T ? (T)(object)(E) : (T)null
```

<span data-ttu-id="08560-2248">Všimněte si, že některé převody, například uživatelem definované převody, nejsou možné s operátorem `as` a měly by být provedeny namísto použití výrazů přetypování.</span><span class="sxs-lookup"><span data-stu-id="08560-2248">Note that some conversions, such as user defined conversions, are not possible with the `as` operator and should instead be performed using cast expressions.</span></span>

<span data-ttu-id="08560-2249">V příkladu</span><span class="sxs-lookup"><span data-stu-id="08560-2249">In the example</span></span>
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
<span data-ttu-id="08560-2250">parametr typu `T` `G` je známý jako typ odkazu, protože má omezení třídy.</span><span class="sxs-lookup"><span data-stu-id="08560-2250">the type parameter `T` of `G` is known to be a reference type, because it has the class constraint.</span></span> <span data-ttu-id="08560-2251">Parametr typu `U` `H` není však k dis. proto použití operátoru `as` v `H` není povoleno.</span><span class="sxs-lookup"><span data-stu-id="08560-2251">The type parameter `U` of `H` is not however; hence the use of the `as` operator in `H` is disallowed.</span></span>

## <a name="logical-operators"></a><span data-ttu-id="08560-2252">Logické operátory</span><span class="sxs-lookup"><span data-stu-id="08560-2252">Logical operators</span></span>

<span data-ttu-id="08560-2253">Operátory `&`, `^`a `|` se nazývají logické operátory.</span><span class="sxs-lookup"><span data-stu-id="08560-2253">The `&`, `^`, and `|` operators are called the logical operators.</span></span>

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

<span data-ttu-id="08560-2254">Pokud má operand logického operátoru `dynamic`typ doby kompilace, pak je výraz dynamicky svázán ([dynamická vazba](expressions.md#dynamic-binding)).</span><span class="sxs-lookup"><span data-stu-id="08560-2254">If an operand of a logical operator has the compile-time type `dynamic`, then the expression is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)).</span></span> <span data-ttu-id="08560-2255">V tomto případě je typ výrazu kompilace `dynamic`a řešení popsané níže bude provedeno za běhu s použitím běhového typu u operandů, které mají typ doby kompilace `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="08560-2255">In this case the compile-time type of the expression is `dynamic`, and the resolution described below will take place at run-time using the run-time type of those operands that have the compile-time type `dynamic`.</span></span>

<span data-ttu-id="08560-2256">Pro operaci `x op y`formuláře, kde `op` je jedním z logických operátorů, je pro výběr konkrétní implementace operátoru použito rozlišení přetížení ([rozlišení přetěžování binárních operátorů](expressions.md#binary-operator-overload-resolution)).</span><span class="sxs-lookup"><span data-stu-id="08560-2256">For an operation of the form `x op y`, where `op` is one of the logical operators, overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="08560-2257">Operandy jsou převedeny na typy parametrů vybraného operátoru a typ výsledku je návratový typ operátoru.</span><span class="sxs-lookup"><span data-stu-id="08560-2257">The operands are converted to the parameter types of the selected operator, and the type of the result is the return type of the operator.</span></span>

<span data-ttu-id="08560-2258">Předdefinované logické operátory jsou popsány v následujících částech.</span><span class="sxs-lookup"><span data-stu-id="08560-2258">The predefined logical operators are described in the following sections.</span></span>

### <a name="integer-logical-operators"></a><span data-ttu-id="08560-2259">Logické operátory typu Integer</span><span class="sxs-lookup"><span data-stu-id="08560-2259">Integer logical operators</span></span>

<span data-ttu-id="08560-2260">Předdefinované logické operátory typu Integer jsou:</span><span class="sxs-lookup"><span data-stu-id="08560-2260">The predefined integer logical operators are:</span></span>
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

<span data-ttu-id="08560-2261">Operátor `&` Vypočítá bitový logický `AND` dvou operandů, operátor `|` Vypočítá bitový logický `OR` dvou operandů a operátor `^` Vypočítá bitový logický výhradní `OR` dvou operandů.</span><span class="sxs-lookup"><span data-stu-id="08560-2261">The `&` operator computes the bitwise logical `AND` of the two operands, the `|` operator computes the bitwise logical `OR` of the two operands, and the `^` operator computes the bitwise logical exclusive `OR` of the two operands.</span></span> <span data-ttu-id="08560-2262">Z těchto operací nejsou možné žádné přetečení.</span><span class="sxs-lookup"><span data-stu-id="08560-2262">No overflows are possible from these operations.</span></span>

### <a name="enumeration-logical-operators"></a><span data-ttu-id="08560-2263">Logické operátory výčtu</span><span class="sxs-lookup"><span data-stu-id="08560-2263">Enumeration logical operators</span></span>

<span data-ttu-id="08560-2264">Každý typ výčtu `E` implicitně poskytuje následující předdefinované logické operátory:</span><span class="sxs-lookup"><span data-stu-id="08560-2264">Every enumeration type `E` implicitly provides the following predefined logical operators:</span></span>

```csharp
E operator &(E x, E y);
E operator |(E x, E y);
E operator ^(E x, E y);
```

<span data-ttu-id="08560-2265">Výsledek vyhodnocení `x op y`, kde `x` a `y` jsou výrazy typu výčtu `E` s podkladovým typem `U`a `op` je jedním z logických operátorů, je naprosto stejný jako vyhodnocení `(E)((U)x op (U)y)`.</span><span class="sxs-lookup"><span data-stu-id="08560-2265">The result of evaluating `x op y`, where `x` and `y` are expressions of an enumeration type `E` with an underlying type `U`, and `op` is one of the logical operators, is exactly the same as evaluating `(E)((U)x op (U)y)`.</span></span> <span data-ttu-id="08560-2266">Jinými slovy, logické operátory typu výčtu jednoduše provádějí logickou operaci na základním typu dvou operandů.</span><span class="sxs-lookup"><span data-stu-id="08560-2266">In other words, the enumeration type logical operators simply perform the logical operation on the underlying type of the two operands.</span></span>

### <a name="boolean-logical-operators"></a><span data-ttu-id="08560-2267">Logické operátory</span><span class="sxs-lookup"><span data-stu-id="08560-2267">Boolean logical operators</span></span>

<span data-ttu-id="08560-2268">Předdefinované logické operátory Boolean jsou:</span><span class="sxs-lookup"><span data-stu-id="08560-2268">The predefined boolean logical operators are:</span></span>
```csharp
bool operator &(bool x, bool y);
bool operator |(bool x, bool y);
bool operator ^(bool x, bool y);
```

<span data-ttu-id="08560-2269">Výsledek `x & y` je `true`, pokud jsou `true``x` i `y`.</span><span class="sxs-lookup"><span data-stu-id="08560-2269">The result of `x & y` is `true` if both `x` and `y` are `true`.</span></span> <span data-ttu-id="08560-2270">V opačném případě je výsledek `false`.</span><span class="sxs-lookup"><span data-stu-id="08560-2270">Otherwise, the result is `false`.</span></span>

<span data-ttu-id="08560-2271">Výsledek `x | y` je `true`, pokud je `true`buď `x`, nebo `y`.</span><span class="sxs-lookup"><span data-stu-id="08560-2271">The result of `x | y` is `true` if either `x` or `y` is `true`.</span></span> <span data-ttu-id="08560-2272">V opačném případě je výsledek `false`.</span><span class="sxs-lookup"><span data-stu-id="08560-2272">Otherwise, the result is `false`.</span></span>

<span data-ttu-id="08560-2273">Výsledek `x ^ y` je `true`, pokud je `x` `true` a `y` `false`nebo `x` `false` a `y`.</span><span class="sxs-lookup"><span data-stu-id="08560-2273">The result of `x ^ y` is `true` if `x` is `true` and `y` is `false`, or `x` is `false` and `y` is `true`.</span></span> <span data-ttu-id="08560-2274">V opačném případě je výsledek `false`.</span><span class="sxs-lookup"><span data-stu-id="08560-2274">Otherwise, the result is `false`.</span></span> <span data-ttu-id="08560-2275">Pokud jsou operandy typu `bool`, operátor `^` vypočítá stejný výsledek jako operátor `!=`.</span><span class="sxs-lookup"><span data-stu-id="08560-2275">When the operands are of type `bool`, the `^` operator computes the same result as the `!=` operator.</span></span>

### <a name="nullable-boolean-logical-operators"></a><span data-ttu-id="08560-2276">Logické operátory s možnou hodnotou null</span><span class="sxs-lookup"><span data-stu-id="08560-2276">Nullable boolean logical operators</span></span>

<span data-ttu-id="08560-2277">Typ Boolean s možnou hodnotou null `bool?` může reprezentovat tři hodnoty, `true`, `false`a `null`a je koncepčně podobný typu se třemi hodnotami použitými pro logické výrazy v SQL.</span><span class="sxs-lookup"><span data-stu-id="08560-2277">The nullable boolean type `bool?` can represent three values, `true`, `false`, and `null`, and is conceptually similar to the three-valued type used for boolean expressions in SQL.</span></span> <span data-ttu-id="08560-2278">Pro zajištění, že výsledky vytvořené `&` a `|` operátory pro `bool?` operandy jsou konzistentní s logikou tří hodnot SQL, jsou k dispozici následující předdefinované operátory:</span><span class="sxs-lookup"><span data-stu-id="08560-2278">To ensure that the results produced by the `&` and `|` operators for `bool?` operands are consistent with SQL's three-valued logic, the following predefined operators are provided:</span></span>

```csharp
bool? operator &(bool? x, bool? y);
bool? operator |(bool? x, bool? y);
```

<span data-ttu-id="08560-2279">V následující tabulce jsou uvedeny výsledky, které tyto operátory vygenerovaly pro všechny kombinace hodnot `true`, `false`a `null`.</span><span class="sxs-lookup"><span data-stu-id="08560-2279">The following table lists the results produced by these operators for all combinations of the values `true`, `false`, and `null`.</span></span>

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

## <a name="conditional-logical-operators"></a><span data-ttu-id="08560-2280">Podmíněné logické operátory</span><span class="sxs-lookup"><span data-stu-id="08560-2280">Conditional logical operators</span></span>

<span data-ttu-id="08560-2281">Operátory `&&` a `||` se nazývají Podmíněné logické operátory.</span><span class="sxs-lookup"><span data-stu-id="08560-2281">The `&&` and `||` operators are called the conditional logical operators.</span></span> <span data-ttu-id="08560-2282">Označují se také jako logické operátory "krátkodobého okruhu".</span><span class="sxs-lookup"><span data-stu-id="08560-2282">They are also called the "short-circuiting" logical operators.</span></span>

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

<span data-ttu-id="08560-2283">Operátory `&&` a `||` jsou podmíněné verze operátorů `&` a `|`:</span><span class="sxs-lookup"><span data-stu-id="08560-2283">The `&&` and `||` operators are conditional versions of the `&` and `|` operators:</span></span>

*  <span data-ttu-id="08560-2284">Operace `x && y` odpovídá `x & y`operace s tím rozdílem, že `y` je vyhodnocena pouze v případě, že `x` není `false`.</span><span class="sxs-lookup"><span data-stu-id="08560-2284">The operation `x && y` corresponds to the operation `x & y`, except that `y` is evaluated only if `x` is not `false`.</span></span>
*  <span data-ttu-id="08560-2285">Operace `x || y` odpovídá `x | y`operace s tím rozdílem, že `y` je vyhodnocena pouze v případě, že `x` není `true`.</span><span class="sxs-lookup"><span data-stu-id="08560-2285">The operation `x || y` corresponds to the operation `x | y`, except that `y` is evaluated only if `x` is not `true`.</span></span>

<span data-ttu-id="08560-2286">Pokud má operand podmíněného logického operátoru `dynamic`typ doby kompilace, pak je výraz dynamicky svázán ([dynamická vazba](expressions.md#dynamic-binding)).</span><span class="sxs-lookup"><span data-stu-id="08560-2286">If an operand of a conditional logical operator has the compile-time type `dynamic`, then the expression is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)).</span></span> <span data-ttu-id="08560-2287">V tomto případě je typ výrazu kompilace `dynamic`a řešení popsané níže bude provedeno za běhu s použitím běhového typu u operandů, které mají typ doby kompilace `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="08560-2287">In this case the compile-time type of the expression is `dynamic`, and the resolution described below will take place at run-time using the run-time type of those operands that have the compile-time type `dynamic`.</span></span>

<span data-ttu-id="08560-2288">Operace formuláře `x && y` nebo `x || y` se zpracovává pomocí Rozlišení přetěžování ([rozlišení přetížení binárního operátoru](expressions.md#binary-operator-overload-resolution)), jako kdyby byla operace napsána `x & y` nebo `x | y`.</span><span class="sxs-lookup"><span data-stu-id="08560-2288">An operation of the form `x && y` or `x || y` is processed by applying overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) as if the operation was written `x & y` or `x | y`.</span></span> <span data-ttu-id="08560-2289">Stisknutím</span><span class="sxs-lookup"><span data-stu-id="08560-2289">Then,</span></span>

*  <span data-ttu-id="08560-2290">Pokud rozlišení přetížení nenajde jeden nejlepší operátor, nebo pokud řešení přetížení vybere jeden z předdefinovaných logických logických operátorů, dojde k chybě při vazbě.</span><span class="sxs-lookup"><span data-stu-id="08560-2290">If overload resolution fails to find a single best operator, or if overload resolution selects one of the predefined integer logical operators, a binding-time error occurs.</span></span>
*  <span data-ttu-id="08560-2291">V opačném případě, pokud je vybraný operátor jedním z předdefinovaných logických logických operátorů (logických[logických operátorů](expressions.md#boolean-logical-operators)) nebo s možnou hodnotou null logických operátorů (s[možnou hodnotou null](expressions.md#nullable-boolean-logical-operators)), je operace zpracována, jak je popsáno v [logické](expressions.md#boolean-conditional-logical-operators)</span><span class="sxs-lookup"><span data-stu-id="08560-2291">Otherwise, if the selected operator is one of the predefined boolean logical operators ([Boolean logical operators](expressions.md#boolean-logical-operators)) or nullable boolean logical operators ([Nullable boolean logical operators](expressions.md#nullable-boolean-logical-operators)), the operation is processed as described in [Boolean conditional logical operators](expressions.md#boolean-conditional-logical-operators).</span></span>
*  <span data-ttu-id="08560-2292">V opačném případě je vybraný operátor uživatelem definovaný operátor a operace je zpracována tak, jak je popsáno v [uživatelsky definovaných podmíněných logických operátorech](expressions.md#user-defined-conditional-logical-operators).</span><span class="sxs-lookup"><span data-stu-id="08560-2292">Otherwise, the selected operator is a user-defined operator, and the operation is processed as described in [User-defined conditional logical operators](expressions.md#user-defined-conditional-logical-operators).</span></span>

<span data-ttu-id="08560-2293">Není možné přímo přetížit Podmíněné logické operátory.</span><span class="sxs-lookup"><span data-stu-id="08560-2293">It is not possible to directly overload the conditional logical operators.</span></span> <span data-ttu-id="08560-2294">Vzhledem k tomu, že podmíněné logické operátory jsou vyhodnocovány v souvislosti s běžnými logickými operátory, jsou přetížení běžných logických operátorů, s určitými omezeními, také považována za přetížení podmíněných logických operátorů.</span><span class="sxs-lookup"><span data-stu-id="08560-2294">However, because the conditional logical operators are evaluated in terms of the regular logical operators, overloads of the regular logical operators are, with certain restrictions, also considered overloads of the conditional logical operators.</span></span> <span data-ttu-id="08560-2295">To je podrobněji popsáno v [uživatelsky definovaných podmíněných logických operátorech](expressions.md#user-defined-conditional-logical-operators).</span><span class="sxs-lookup"><span data-stu-id="08560-2295">This is described further in [User-defined conditional logical operators](expressions.md#user-defined-conditional-logical-operators).</span></span>

### <a name="boolean-conditional-logical-operators"></a><span data-ttu-id="08560-2296">Logické Podmíněné logické operátory</span><span class="sxs-lookup"><span data-stu-id="08560-2296">Boolean conditional logical operators</span></span>

<span data-ttu-id="08560-2297">Pokud jsou operandy `&&` nebo `||` typu `bool`nebo pokud jsou operandy typů, které nedefinují příslušné `operator &` nebo `operator |`, ale definují implicitní převody na `bool`, bude operace zpracována následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="08560-2297">When the operands of `&&` or `||` are of type `bool`, or when the operands are of types that do not define an applicable `operator &` or `operator |`, but do define implicit conversions to `bool`, the operation is processed as follows:</span></span>

*  <span data-ttu-id="08560-2298">Operace `x && y` je vyhodnocena jako `x ? y : false`.</span><span class="sxs-lookup"><span data-stu-id="08560-2298">The operation `x && y` is evaluated as `x ? y : false`.</span></span> <span data-ttu-id="08560-2299">Jinými slovy, `x` je nejprve vyhodnocena a převedena na typ `bool`.</span><span class="sxs-lookup"><span data-stu-id="08560-2299">In other words, `x` is first evaluated and converted to type `bool`.</span></span> <span data-ttu-id="08560-2300">Pokud je pak `x` `true`, `y` vyhodnocena a převedena na typ `bool`a to se stalo výsledkem operace.</span><span class="sxs-lookup"><span data-stu-id="08560-2300">Then, if `x` is `true`, `y` is evaluated and converted to type `bool`, and this becomes the result of the operation.</span></span> <span data-ttu-id="08560-2301">V opačném případě je výsledek operace `false`.</span><span class="sxs-lookup"><span data-stu-id="08560-2301">Otherwise, the result of the operation is `false`.</span></span>
*  <span data-ttu-id="08560-2302">Operace `x || y` je vyhodnocena jako `x ? true : y`.</span><span class="sxs-lookup"><span data-stu-id="08560-2302">The operation `x || y` is evaluated as `x ? true : y`.</span></span> <span data-ttu-id="08560-2303">Jinými slovy, `x` je nejprve vyhodnocena a převedena na typ `bool`.</span><span class="sxs-lookup"><span data-stu-id="08560-2303">In other words, `x` is first evaluated and converted to type `bool`.</span></span> <span data-ttu-id="08560-2304">Pokud je pak `x` `true`, výsledek operace je `true`.</span><span class="sxs-lookup"><span data-stu-id="08560-2304">Then, if `x` is `true`, the result of the operation is `true`.</span></span> <span data-ttu-id="08560-2305">V opačném případě je `y` vyhodnocena a převedena na typ `bool`a to se stalo výsledkem operace.</span><span class="sxs-lookup"><span data-stu-id="08560-2305">Otherwise, `y` is evaluated and converted to type `bool`, and this becomes the result of the operation.</span></span>

### <a name="user-defined-conditional-logical-operators"></a><span data-ttu-id="08560-2306">Podmíněné logické operátory definované uživatelem</span><span class="sxs-lookup"><span data-stu-id="08560-2306">User-defined conditional logical operators</span></span>

<span data-ttu-id="08560-2307">Pokud jsou operandy `&&` nebo `||` typu, který deklaruje příslušný `operator &` nebo `operator |`definovaný uživatelem, musí být obě následující hodnoty true, kde `T` je typ, ve kterém je vybraný operátor deklarován:</span><span class="sxs-lookup"><span data-stu-id="08560-2307">When the operands of `&&` or `||` are of types that declare an applicable user-defined `operator &` or `operator |`, both of the following must be true, where `T` is the type in which the selected operator is declared:</span></span>

*  <span data-ttu-id="08560-2308">Návratový typ a typ každého parametru vybraného operátoru musí být `T`.</span><span class="sxs-lookup"><span data-stu-id="08560-2308">The return type and the type of each parameter of the selected operator must be `T`.</span></span> <span data-ttu-id="08560-2309">Jinými slovy, operátor musí vypočítat logický `AND` nebo logický `OR` dvou operandů typu `T`a musí vracet výsledek typu `T`.</span><span class="sxs-lookup"><span data-stu-id="08560-2309">In other words, the operator must compute the logical `AND` or the logical `OR` of two operands of type `T`, and must return a result of type `T`.</span></span>
*  <span data-ttu-id="08560-2310">`T` musí obsahovat deklarace `operator true` a `operator false`.</span><span class="sxs-lookup"><span data-stu-id="08560-2310">`T` must contain declarations of `operator true` and `operator false`.</span></span>

<span data-ttu-id="08560-2311">Pokud některý z těchto požadavků není splněn, dojde k chybě při vazbě.</span><span class="sxs-lookup"><span data-stu-id="08560-2311">A binding-time error occurs if either of these requirements is not satisfied.</span></span> <span data-ttu-id="08560-2312">V opačném případě je operace `&&` nebo `||` vyhodnocována kombinací uživatelsky definovaného `operator true` nebo `operator false` s vybraným uživatelem definovaným operátorem:</span><span class="sxs-lookup"><span data-stu-id="08560-2312">Otherwise, the `&&` or `||` operation is evaluated by combining the user-defined `operator true` or `operator false` with the selected user-defined operator:</span></span>

*  <span data-ttu-id="08560-2313">Operace `x && y` je vyhodnocena jako `T.false(x) ? x : T.&(x, y)`, kde `T.false(x)` je vyvolání `operator false` deklarovaného v `T`a `T.&(x, y)` je vyvolání vybraného `operator &`.</span><span class="sxs-lookup"><span data-stu-id="08560-2313">The operation `x && y` is evaluated as `T.false(x) ? x : T.&(x, y)`, where `T.false(x)` is an invocation of the `operator false` declared in `T`, and `T.&(x, y)` is an invocation of the selected `operator &`.</span></span> <span data-ttu-id="08560-2314">Jinými slovy `x` je nejprve vyhodnocen a `operator false` je vyvolána ve výsledku, aby bylo možné určit, zda je `x` jednoznačně false.</span><span class="sxs-lookup"><span data-stu-id="08560-2314">In other words, `x` is first evaluated and `operator false` is invoked on the result to determine if `x` is definitely false.</span></span> <span data-ttu-id="08560-2315">Pokud je pak `x` jednoznačně false, výsledek operace je hodnota dříve vypočítaná pro `x`.</span><span class="sxs-lookup"><span data-stu-id="08560-2315">Then, if `x` is definitely false, the result of the operation is the value previously computed for `x`.</span></span> <span data-ttu-id="08560-2316">V opačném případě je vyhodnocen `y` a vybraný `operator &` je vyvolán na hodnotu dříve vypočítanou pro `x` a hodnotu vypočítanou pro `y` k získání výsledku operace.</span><span class="sxs-lookup"><span data-stu-id="08560-2316">Otherwise, `y` is evaluated, and the selected `operator &` is invoked on the value previously computed for `x` and the value computed for `y` to produce the result of the operation.</span></span>
*  <span data-ttu-id="08560-2317">Operace `x || y` je vyhodnocena jako `T.true(x) ? x : T.|(x, y)`, kde `T.true(x)` je vyvolání `operator true` deklarovaného v `T`a `T.|(x,y)` je vyvolání vybraného `operator|`.</span><span class="sxs-lookup"><span data-stu-id="08560-2317">The operation `x || y` is evaluated as `T.true(x) ? x : T.|(x, y)`, where `T.true(x)` is an invocation of the `operator true` declared in `T`, and `T.|(x,y)` is an invocation of the selected `operator|`.</span></span> <span data-ttu-id="08560-2318">Jinými slovy `x` je nejprve vyhodnocen a `operator true` je vyvolána ve výsledku, aby bylo možné určit, zda je `x` jednoznačně true.</span><span class="sxs-lookup"><span data-stu-id="08560-2318">In other words, `x` is first evaluated and `operator true` is invoked on the result to determine if `x` is definitely true.</span></span> <span data-ttu-id="08560-2319">Pokud je pak `x` jednoznačně true, výsledek operace je hodnota dříve vypočítaná pro `x`.</span><span class="sxs-lookup"><span data-stu-id="08560-2319">Then, if `x` is definitely true, the result of the operation is the value previously computed for `x`.</span></span> <span data-ttu-id="08560-2320">V opačném případě je vyhodnocen `y` a vybraný `operator |` je vyvolán na hodnotu dříve vypočítanou pro `x` a hodnotu vypočítanou pro `y` k získání výsledku operace.</span><span class="sxs-lookup"><span data-stu-id="08560-2320">Otherwise, `y` is evaluated, and the selected `operator |` is invoked on the value previously computed for `x` and the value computed for `y` to produce the result of the operation.</span></span>

<span data-ttu-id="08560-2321">V některé z těchto operací je výraz zadaný pomocí `x` vyhodnocován pouze jednou a výraz zadaný pomocí `y` buď není vyhodnocen nebo vyhodnocován právě jednou.</span><span class="sxs-lookup"><span data-stu-id="08560-2321">In either of these operations, the expression given by `x` is only evaluated once, and the expression given by `y` is either not evaluated or evaluated exactly once.</span></span>

<span data-ttu-id="08560-2322">Příklad typu, který implementuje `operator true` a `operator false`, naleznete v tématu [Database Boolean Type](structs.md#database-boolean-type).</span><span class="sxs-lookup"><span data-stu-id="08560-2322">For an example of a type that implements `operator true` and `operator false`, see [Database boolean type](structs.md#database-boolean-type).</span></span>

## <a name="the-null-coalescing-operator"></a><span data-ttu-id="08560-2323">Operátor slučování s hodnotou null</span><span class="sxs-lookup"><span data-stu-id="08560-2323">The null coalescing operator</span></span>

<span data-ttu-id="08560-2324">Operátor `??` se nazývá slučovací operátor null.</span><span class="sxs-lookup"><span data-stu-id="08560-2324">The `??` operator is called the null coalescing operator.</span></span>

```antlr
null_coalescing_expression
    : conditional_or_expression
    | conditional_or_expression '??' null_coalescing_expression
    ;
```

<span data-ttu-id="08560-2325">Výraz sloučení null formuláře `a ?? b` vyžaduje, aby `a` být typu s možnou hodnotou null nebo odkazem.</span><span class="sxs-lookup"><span data-stu-id="08560-2325">A null coalescing expression of the form `a ?? b` requires `a` to be of a nullable type or reference type.</span></span> <span data-ttu-id="08560-2326">Pokud `a` není null, výsledek `a ?? b` je `a`; v opačném případě je výsledek `b`.</span><span class="sxs-lookup"><span data-stu-id="08560-2326">If `a` is non-null, the result of `a ?? b` is `a`; otherwise, the result is `b`.</span></span> <span data-ttu-id="08560-2327">Tato operace vyhodnotí `b` pouze v případě, že `a` je null.</span><span class="sxs-lookup"><span data-stu-id="08560-2327">The operation evaluates `b` only if `a` is null.</span></span>

<span data-ttu-id="08560-2328">Operátor slučování s hodnotou null je asociativní zprava, což znamená, že operace jsou seskupeny zprava doleva.</span><span class="sxs-lookup"><span data-stu-id="08560-2328">The null coalescing operator is right-associative, meaning that operations are grouped from right to left.</span></span> <span data-ttu-id="08560-2329">Například výraz `a ?? b ?? c` formuláře je vyhodnocen jako `a ?? (b ?? c)`.</span><span class="sxs-lookup"><span data-stu-id="08560-2329">For example, an expression of the form `a ?? b ?? c` is evaluated as `a ?? (b ?? c)`.</span></span> <span data-ttu-id="08560-2330">Obecně platí, že výraz formuláře `E1 ?? E2 ?? ... ?? En` vrátí první z operandů, které nejsou null, nebo null, pokud jsou všechny operandy NULL.</span><span class="sxs-lookup"><span data-stu-id="08560-2330">In general terms, an expression of the form `E1 ?? E2 ?? ... ?? En` returns the first of the operands that is non-null, or null if all operands are null.</span></span>

<span data-ttu-id="08560-2331">Typ `a ?? b` výrazu závisí na tom, které implicitní převody jsou k dispozici na operandech.</span><span class="sxs-lookup"><span data-stu-id="08560-2331">The type of the expression `a ?? b` depends on which implicit conversions are available on the operands.</span></span> <span data-ttu-id="08560-2332">V upřednostňovaném pořadí je typ `a ?? b` `A0`, `A`nebo `B`, kde `A` je typ `a` (za předpokladu, že `a` má typ), `B` je typ `b` (za předpokladu, že `b` má typ) a `A0` je podkladovým typem `A`, pokud `A` je typ s povolenou hodnotou null nebo `A` jinak.</span><span class="sxs-lookup"><span data-stu-id="08560-2332">In order of preference, the type of `a ?? b` is `A0`, `A`, or `B`, where `A` is the type of `a` (provided that `a` has a type), `B` is the type of `b` (provided that `b` has a type), and `A0` is the underlying type of `A` if `A` is a nullable type, or `A` otherwise.</span></span> <span data-ttu-id="08560-2333">Konkrétně `a ?? b` je zpracována takto:</span><span class="sxs-lookup"><span data-stu-id="08560-2333">Specifically, `a ?? b` is processed as follows:</span></span>

*  <span data-ttu-id="08560-2334">Pokud `A` existuje a není to typ s možnou hodnotou null nebo odkazový typ, dojde k chybě při kompilaci.</span><span class="sxs-lookup"><span data-stu-id="08560-2334">If `A` exists and is not a nullable type or a reference type, a compile-time error occurs.</span></span>
*  <span data-ttu-id="08560-2335">Pokud `b` je dynamický výraz, je typ výsledku `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="08560-2335">If `b` is a dynamic expression, the result type is `dynamic`.</span></span> <span data-ttu-id="08560-2336">V době běhu je nejprve vyhodnocen `a`.</span><span class="sxs-lookup"><span data-stu-id="08560-2336">At run-time, `a` is first evaluated.</span></span> <span data-ttu-id="08560-2337">Pokud `a` není null, `a` je převedena na Dynamic, což se změní na výsledek.</span><span class="sxs-lookup"><span data-stu-id="08560-2337">If `a` is not null, `a` is converted to dynamic, and this becomes the result.</span></span> <span data-ttu-id="08560-2338">V opačném případě se `b` vyhodnotí, a to se stalo výsledkem.</span><span class="sxs-lookup"><span data-stu-id="08560-2338">Otherwise, `b` is evaluated, and this becomes the result.</span></span>
*  <span data-ttu-id="08560-2339">V opačném případě, pokud `A` existuje a jde o typ s možnou hodnotou null a implicitní převod existuje z `b` na `A0`, je typ výsledku `A0`.</span><span class="sxs-lookup"><span data-stu-id="08560-2339">Otherwise, if `A` exists and is a nullable type and an implicit conversion exists from `b` to `A0`, the result type is `A0`.</span></span> <span data-ttu-id="08560-2340">V době běhu je nejprve vyhodnocen `a`.</span><span class="sxs-lookup"><span data-stu-id="08560-2340">At run-time, `a` is first evaluated.</span></span> <span data-ttu-id="08560-2341">Pokud `a` není null, `a` se rozbalí do typu `A0`, což se zobrazí jako výsledek.</span><span class="sxs-lookup"><span data-stu-id="08560-2341">If `a` is not null, `a` is unwrapped to type `A0`, and this becomes the result.</span></span> <span data-ttu-id="08560-2342">V opačném případě je `b` vyhodnocena a převedena na typ `A0`a to se stalo výsledkem.</span><span class="sxs-lookup"><span data-stu-id="08560-2342">Otherwise, `b` is evaluated and converted to type `A0`, and this becomes the result.</span></span>
*  <span data-ttu-id="08560-2343">V opačném případě, pokud `A` existuje a implicitní převod existuje z `b` na `A`, je typ výsledku `A`.</span><span class="sxs-lookup"><span data-stu-id="08560-2343">Otherwise, if `A` exists and an implicit conversion exists from `b` to `A`, the result type is `A`.</span></span> <span data-ttu-id="08560-2344">V době běhu je nejprve vyhodnocen `a`.</span><span class="sxs-lookup"><span data-stu-id="08560-2344">At run-time, `a` is first evaluated.</span></span> <span data-ttu-id="08560-2345">Pokud `a` není null, bude výsledkem `a`.</span><span class="sxs-lookup"><span data-stu-id="08560-2345">If `a` is not null, `a` becomes the result.</span></span> <span data-ttu-id="08560-2346">V opačném případě je `b` vyhodnocena a převedena na typ `A`a to se stalo výsledkem.</span><span class="sxs-lookup"><span data-stu-id="08560-2346">Otherwise, `b` is evaluated and converted to type `A`, and this becomes the result.</span></span>
*  <span data-ttu-id="08560-2347">V opačném případě, pokud má `b` `B` typu a implicitní převod existuje z `a` na `B`, je typ výsledku `B`.</span><span class="sxs-lookup"><span data-stu-id="08560-2347">Otherwise, if `b` has a type `B` and an implicit conversion exists from `a` to `B`, the result type is `B`.</span></span> <span data-ttu-id="08560-2348">V době běhu je nejprve vyhodnocen `a`.</span><span class="sxs-lookup"><span data-stu-id="08560-2348">At run-time, `a` is first evaluated.</span></span> <span data-ttu-id="08560-2349">Pokud `a` není null, `a` je rozbalení do typu `A0` (Pokud `A` existuje a je Nullable) a převedeno na typ `B`a to se stalo výsledkem.</span><span class="sxs-lookup"><span data-stu-id="08560-2349">If `a` is not null, `a` is unwrapped to type `A0` (if `A` exists and is nullable) and converted to type `B`, and this becomes the result.</span></span> <span data-ttu-id="08560-2350">V opačném případě se `b` vyhodnotí a bude výsledkem.</span><span class="sxs-lookup"><span data-stu-id="08560-2350">Otherwise, `b` is evaluated and becomes the result.</span></span>
*  <span data-ttu-id="08560-2351">V opačném případě jsou `a` a `b` nekompatibilní a dojde k chybě při kompilaci.</span><span class="sxs-lookup"><span data-stu-id="08560-2351">Otherwise, `a` and `b` are incompatible, and a compile-time error occurs.</span></span>

## <a name="conditional-operator"></a><span data-ttu-id="08560-2352">Podmíněný operátor</span><span class="sxs-lookup"><span data-stu-id="08560-2352">Conditional operator</span></span>

<span data-ttu-id="08560-2353">Operátor `?:` se nazývá podmíněný operátor.</span><span class="sxs-lookup"><span data-stu-id="08560-2353">The `?:` operator is called the conditional operator.</span></span> <span data-ttu-id="08560-2354">Je někdy také označován jako Ternární operátor.</span><span class="sxs-lookup"><span data-stu-id="08560-2354">It is at times also called the ternary operator.</span></span>

```antlr
conditional_expression
    : null_coalescing_expression
    | null_coalescing_expression '?' expression ':' expression
    ;
```

<span data-ttu-id="08560-2355">Podmíněný výraz formuláře `b ? x : y` nejprve vyhodnotí podmínku `b`.</span><span class="sxs-lookup"><span data-stu-id="08560-2355">A conditional expression of the form `b ? x : y` first evaluates the condition `b`.</span></span> <span data-ttu-id="08560-2356">Když se pak `b` `true`, `x` se vyhodnotí a výsledkem operace se bude.</span><span class="sxs-lookup"><span data-stu-id="08560-2356">Then, if `b` is `true`, `x` is evaluated and becomes the result of the operation.</span></span> <span data-ttu-id="08560-2357">V opačném případě se `y` vyhodnotí a výsledkem operace bude.</span><span class="sxs-lookup"><span data-stu-id="08560-2357">Otherwise, `y` is evaluated and becomes the result of the operation.</span></span> <span data-ttu-id="08560-2358">Podmíněný výraz nikdy nevyhodnotí `x` i `y`.</span><span class="sxs-lookup"><span data-stu-id="08560-2358">A conditional expression never evaluates both `x` and `y`.</span></span>

<span data-ttu-id="08560-2359">Podmíněný operátor je asociativní zprava, což znamená, že operace jsou seskupeny zprava doleva.</span><span class="sxs-lookup"><span data-stu-id="08560-2359">The conditional operator is right-associative, meaning that operations are grouped from right to left.</span></span> <span data-ttu-id="08560-2360">Například výraz `a ? b : c ? d : e` formuláře je vyhodnocen jako `a ? b : (c ? d : e)`.</span><span class="sxs-lookup"><span data-stu-id="08560-2360">For example, an expression of the form `a ? b : c ? d : e` is evaluated as `a ? b : (c ? d : e)`.</span></span>

<span data-ttu-id="08560-2361">Prvním operandem operátoru `?:` musí být výraz, který lze implicitně převést na `bool`nebo výraz typu, který implementuje `operator true`.</span><span class="sxs-lookup"><span data-stu-id="08560-2361">The first operand of the `?:` operator must be an expression that can be implicitly converted to `bool`, or an expression of a type that implements `operator true`.</span></span> <span data-ttu-id="08560-2362">Pokud ani jeden z těchto požadavků není splněn, dojde k chybě při kompilaci.</span><span class="sxs-lookup"><span data-stu-id="08560-2362">If neither of these requirements is satisfied, a compile-time error occurs.</span></span>

<span data-ttu-id="08560-2363">Druhý a třetí operand `x` a `y`ovládacího prvku `?:`, který určuje typ podmíněného výrazu.</span><span class="sxs-lookup"><span data-stu-id="08560-2363">The second and third operands, `x` and `y`, of the `?:` operator control the type of the conditional expression.</span></span>

*  <span data-ttu-id="08560-2364">Pokud je `x` typu `X` a `y` je typu `Y`</span><span class="sxs-lookup"><span data-stu-id="08560-2364">If `x` has type `X` and `y` has type `Y` then</span></span>
   * <span data-ttu-id="08560-2365">Pokud implicitní převod ([implicitní převody](conversions.md#implicit-conversions)) existuje z `X` na `Y`, ale ne z `Y` na `X`, pak `Y` je typ podmíněného výrazu.</span><span class="sxs-lookup"><span data-stu-id="08560-2365">If an implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)) exists from `X` to `Y`, but not from `Y` to `X`, then `Y` is the type of the conditional expression.</span></span>
   * <span data-ttu-id="08560-2366">Pokud implicitní převod ([implicitní převody](conversions.md#implicit-conversions)) existuje z `Y` na `X`, ale ne z `X` na `Y`, pak `X` je typ podmíněného výrazu.</span><span class="sxs-lookup"><span data-stu-id="08560-2366">If an implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)) exists from `Y` to `X`, but not from `X` to `Y`, then `X` is the type of the conditional expression.</span></span>
   * <span data-ttu-id="08560-2367">V opačném případě nelze určit žádný typ výrazu a dojde k chybě při kompilaci.</span><span class="sxs-lookup"><span data-stu-id="08560-2367">Otherwise, no expression type can be determined, and a compile-time error occurs.</span></span>
*  <span data-ttu-id="08560-2368">Pokud je typ pouze jeden z `x` a `y` a `x` i `y`jsou implicitně převoditelné na tento typ, pak to je typ podmíněného výrazu.</span><span class="sxs-lookup"><span data-stu-id="08560-2368">If only one of `x` and `y` has a type, and both `x` and `y`, of are implicitly convertible to that type, then that is the type of the conditional expression.</span></span>
*  <span data-ttu-id="08560-2369">V opačném případě nelze určit žádný typ výrazu a dojde k chybě při kompilaci.</span><span class="sxs-lookup"><span data-stu-id="08560-2369">Otherwise, no expression type can be determined, and a compile-time error occurs.</span></span>

<span data-ttu-id="08560-2370">Běhové zpracování podmíněného výrazu formuláře `b ? x : y` sestává z následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="08560-2370">The run-time processing of a conditional expression of the form `b ? x : y` consists of the following steps:</span></span>

*  <span data-ttu-id="08560-2371">Nejprve je vyhodnocena `b` a je určena hodnota `bool` `b`:</span><span class="sxs-lookup"><span data-stu-id="08560-2371">First, `b` is evaluated, and the `bool` value of `b` is determined:</span></span>
   * <span data-ttu-id="08560-2372">Pokud existuje implicitní převod z typu `b` `bool`, pak je tento implicitní převod proveden pro vytvoření `bool` hodnoty.</span><span class="sxs-lookup"><span data-stu-id="08560-2372">If an implicit conversion from the type of `b` to `bool` exists, then this implicit conversion is performed to produce a `bool` value.</span></span>
   * <span data-ttu-id="08560-2373">V opačném případě je vyvolána `operator true` definovaná typem `b`, aby se vytvořila hodnota `bool`.</span><span class="sxs-lookup"><span data-stu-id="08560-2373">Otherwise, the `operator true` defined by the type of `b` is invoked to produce a `bool` value.</span></span>
*  <span data-ttu-id="08560-2374">Je-li hodnota `bool` vytvořená v kroku výše, je `true`vyhodnocena `x` a převedena na typ podmíněného výrazu, a to se projeví v důsledku podmíněného výrazu.</span><span class="sxs-lookup"><span data-stu-id="08560-2374">If the `bool` value produced by the step above is `true`, then `x` is evaluated and converted to the type of the conditional expression, and this becomes the result of the conditional expression.</span></span>
*  <span data-ttu-id="08560-2375">V opačném případě `y` je vyhodnocena a převedena na typ podmíněného výrazu, a to se projeví v důsledku podmíněného výrazu.</span><span class="sxs-lookup"><span data-stu-id="08560-2375">Otherwise, `y` is evaluated and converted to the type of the conditional expression, and this becomes the result of the conditional expression.</span></span>

## <a name="anonymous-function-expressions"></a><span data-ttu-id="08560-2376">Anonymní výrazy funkcí</span><span class="sxs-lookup"><span data-stu-id="08560-2376">Anonymous function expressions</span></span>

<span data-ttu-id="08560-2377">***Anonymní funkce*** je výraz, který představuje definici metody "in-line".</span><span class="sxs-lookup"><span data-stu-id="08560-2377">An ***anonymous function*** is an expression that represents an "in-line" method definition.</span></span> <span data-ttu-id="08560-2378">Anonymní funkce nemá hodnotu ani typ v rámci sebe, ale je převoditelná na kompatibilního delegáta nebo na typ stromu výrazů.</span><span class="sxs-lookup"><span data-stu-id="08560-2378">An anonymous function does not have a value or type in and of itself, but is convertible to a compatible delegate or expression tree type.</span></span> <span data-ttu-id="08560-2379">Vyhodnocení konverze anonymní funkce závisí na cílovém typu převodu: Pokud se jedná o typ delegáta, převod se vyhodnotí na hodnotu delegáta odkazující na metodu, kterou definuje anonymní funkce.</span><span class="sxs-lookup"><span data-stu-id="08560-2379">The evaluation of an anonymous function conversion depends on the target type of the conversion: If it is a delegate type, the conversion evaluates to a delegate value referencing the method which the anonymous function defines.</span></span> <span data-ttu-id="08560-2380">Pokud se jedná o typ stromu výrazu, převod se vyhodnotí na strom výrazu, který představuje strukturu metody jako strukturu objektu.</span><span class="sxs-lookup"><span data-stu-id="08560-2380">If it is an expression tree type, the conversion evaluates to an expression tree which represents the structure of the method as an object structure.</span></span>

<span data-ttu-id="08560-2381">Z historických důvodů existují dvě syntaktické charakter anonymních funkcí, konkrétně *lambda_expression*s a *anonymous_method_expression*s.</span><span class="sxs-lookup"><span data-stu-id="08560-2381">For historical reasons there are two syntactic flavors of anonymous functions, namely *lambda_expression*s and *anonymous_method_expression*s.</span></span> <span data-ttu-id="08560-2382">Pro téměř všechny účely jsou *lambda_expression*s stručnější a vyjádřeníná od *anonymous_method_expression*s, která zůstávají v jazyce pro zpětnou kompatibilitu.</span><span class="sxs-lookup"><span data-stu-id="08560-2382">For almost all purposes, *lambda_expression*s are more concise and expressive than *anonymous_method_expression*s, which remain in the language for backwards compatibility.</span></span>

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

<span data-ttu-id="08560-2383">Operátor `=>` má stejnou prioritu jako přiřazení (`=`) a je asociativní zprava.</span><span class="sxs-lookup"><span data-stu-id="08560-2383">The `=>` operator has the same precedence as assignment (`=`) and is right-associative.</span></span>

<span data-ttu-id="08560-2384">Anonymní funkce s modifikátorem `async` je asynchronní funkce a postupuje podle pravidel popsaných v [iterátorech](classes.md#iterators).</span><span class="sxs-lookup"><span data-stu-id="08560-2384">An anonymous function with the `async` modifier is an async function and follows the rules described in [Iterators](classes.md#iterators).</span></span>

<span data-ttu-id="08560-2385">Parametry anonymní funkce ve formě *lambda_expression* mohou být explicitně nebo implicitně typované.</span><span class="sxs-lookup"><span data-stu-id="08560-2385">The parameters of an anonymous function in the form of a *lambda_expression* can be explicitly or implicitly typed.</span></span> <span data-ttu-id="08560-2386">V seznamu explicitně typovaného parametru je typ každého parametru explicitně uveden.</span><span class="sxs-lookup"><span data-stu-id="08560-2386">In an explicitly typed parameter list, the type of each parameter is explicitly stated.</span></span> <span data-ttu-id="08560-2387">V seznamu implicitně typovaného parametru jsou typy parametrů odvozeny z kontextu, ve kterém dojde k anonymní funkci – konkrétně, pokud je anonymní funkce převedena na kompatibilní typ delegáta nebo na typ stromu výrazů, tento typ poskytuje typy parametrů ([anonymní převody funkcí](conversions.md#anonymous-function-conversions)).</span><span class="sxs-lookup"><span data-stu-id="08560-2387">In an implicitly typed parameter list, the types of the parameters are inferred from the context in which the anonymous function occurs—specifically, when the anonymous function is converted to a compatible delegate type or expression tree type, that type provides the parameter types ([Anonymous function conversions](conversions.md#anonymous-function-conversions)).</span></span>

<span data-ttu-id="08560-2388">V anonymní funkci s jedním implicitně typovým parametrem mohou být kulaté závorky vynechány v seznamu parametrů.</span><span class="sxs-lookup"><span data-stu-id="08560-2388">In an anonymous function with a single, implicitly typed parameter, the parentheses may be omitted from the parameter list.</span></span> <span data-ttu-id="08560-2389">Jinými slovy, anonymní funkce formuláře</span><span class="sxs-lookup"><span data-stu-id="08560-2389">In other words, an anonymous function of the form</span></span>
```csharp
( param ) => expr
```
<span data-ttu-id="08560-2390">může být zkrácen na</span><span class="sxs-lookup"><span data-stu-id="08560-2390">can be abbreviated to</span></span>
```csharp
param => expr
```

<span data-ttu-id="08560-2391">Seznam parametrů anonymní funkce ve formě *anonymous_method_expression* je nepovinný.</span><span class="sxs-lookup"><span data-stu-id="08560-2391">The parameter list of an anonymous function in the form of an *anonymous_method_expression* is optional.</span></span> <span data-ttu-id="08560-2392">Je-li tento parametr zadán, musí být parametry explicitně zadány.</span><span class="sxs-lookup"><span data-stu-id="08560-2392">If given, the parameters must be explicitly typed.</span></span> <span data-ttu-id="08560-2393">V takovém případě je anonymní funkce převoditelná na delegáta s jakýmkoli seznamem parametrů, který neobsahuje `out` parametry.</span><span class="sxs-lookup"><span data-stu-id="08560-2393">If not, the anonymous function is convertible to a delegate with any parameter list not containing `out` parameters.</span></span>

<span data-ttu-id="08560-2394">Tělo *bloku* anonymní funkce je dosažitelné ([koncové body a dosažitelnost](statements.md#end-points-and-reachability)), pokud anonymní funkce nedochází uvnitř nedosažitelného příkazu.</span><span class="sxs-lookup"><span data-stu-id="08560-2394">A *block* body of an anonymous function is reachable ([End points and reachability](statements.md#end-points-and-reachability)) unless the anonymous function occurs inside an unreachable statement.</span></span>

<span data-ttu-id="08560-2395">Příklady anonymních funkcí jsou následující:</span><span class="sxs-lookup"><span data-stu-id="08560-2395">Some examples of anonymous functions follow below:</span></span>

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

<span data-ttu-id="08560-2396">Chování *lambda_expression*s a *anonymous_method_expression*s se shoduje s výjimkou následujících bodů:</span><span class="sxs-lookup"><span data-stu-id="08560-2396">The behavior of *lambda_expression*s and *anonymous_method_expression*s is the same except for the following points:</span></span>

*  <span data-ttu-id="08560-2397">*anonymous_method_expression*s umožní, aby se seznam parametrů vynechal úplně, a převoditelnosti tak, že bude delegovat typy parametrů hodnot.</span><span class="sxs-lookup"><span data-stu-id="08560-2397">*anonymous_method_expression*s permit the parameter list to be omitted entirely, yielding convertibility to delegate types of any list of value parameters.</span></span>
*  <span data-ttu-id="08560-2398">*lambda_expression*s povoluje, aby byly typy parametrů vynechány a odvozeny, zatímco *anonymous_method_expression*s vyžadují explicitní stanovení typů parametrů.</span><span class="sxs-lookup"><span data-stu-id="08560-2398">*lambda_expression*s permit parameter types to be omitted and inferred whereas *anonymous_method_expression*s require parameter types to be explicitly stated.</span></span>
*  <span data-ttu-id="08560-2399">Tělo *lambda_expression* může být výraz nebo blok příkazu, zatímco tělo *anonymous_method_expression* musí být blok příkazu.</span><span class="sxs-lookup"><span data-stu-id="08560-2399">The body of a *lambda_expression* can be an expression or a statement block whereas the body of an *anonymous_method_expression* must be a statement block.</span></span>
*  <span data-ttu-id="08560-2400">Pouze *lambda_expression*s mají převody na kompatibilní typy stromu výrazů ([typy stromu výrazů](types.md#expression-tree-types)).</span><span class="sxs-lookup"><span data-stu-id="08560-2400">Only *lambda_expression*s have conversions to compatible expression tree types ([Expression tree types](types.md#expression-tree-types)).</span></span>

### <a name="anonymous-function-signatures"></a><span data-ttu-id="08560-2401">Podpisy anonymních funkcí</span><span class="sxs-lookup"><span data-stu-id="08560-2401">Anonymous function signatures</span></span>

<span data-ttu-id="08560-2402">Nepovinný *anonymous_function_signature* anonymní funkce definuje názvy a volitelně typy formálních parametrů anonymní funkce.</span><span class="sxs-lookup"><span data-stu-id="08560-2402">The optional *anonymous_function_signature* of an anonymous function defines the names and optionally the types of the formal parameters for the anonymous function.</span></span> <span data-ttu-id="08560-2403">Rozsah parametrů anonymní funkce je *anonymous_function_body*.</span><span class="sxs-lookup"><span data-stu-id="08560-2403">The scope of the parameters of the anonymous function is the *anonymous_function_body*.</span></span> <span data-ttu-id="08560-2404">([Rozsahy](basic-concepts.md#scopes)) Společně se seznamem parametrů (Pokud je uvedeno), představuje tělo anonymní metody místo deklarace ([deklarace](basic-concepts.md#declarations)).</span><span class="sxs-lookup"><span data-stu-id="08560-2404">([Scopes](basic-concepts.md#scopes)) Together with the parameter list (if given) the anonymous-method-body constitutes a declaration space ([Declarations](basic-concepts.md#declarations)).</span></span> <span data-ttu-id="08560-2405">V důsledku toho je chyba kompilace pro název parametru anonymní funkce tak, aby odpovídala názvu místní proměnné, místní konstanty nebo parametru, jehož obor zahrnuje *anonymous_method_expression* nebo *lambda_expression*.</span><span class="sxs-lookup"><span data-stu-id="08560-2405">It is thus a compile-time error for the name of a parameter of the anonymous function to match the name of a local variable, local constant or parameter whose scope includes the *anonymous_method_expression* or *lambda_expression*.</span></span>

<span data-ttu-id="08560-2406">Pokud má anonymní funkce *explicit_anonymous_function_signature*, sada kompatibilních typů delegátů a typů stromu výrazů je omezená na ty, které mají stejné typy parametrů a modifikátory ve stejném pořadí.</span><span class="sxs-lookup"><span data-stu-id="08560-2406">If an anonymous function has an *explicit_anonymous_function_signature*, then the set of compatible delegate types and expression tree types is restricted to those that have the same parameter types and modifiers in the same order.</span></span> <span data-ttu-id="08560-2407">Na rozdíl od převodů skupin metod ([Převod skupin metod](conversions.md#method-group-conversions)) je kontraindikace Nepodporovaná – variance typů parametrů anonymní funkce.</span><span class="sxs-lookup"><span data-stu-id="08560-2407">In contrast to method group conversions ([Method group conversions](conversions.md#method-group-conversions)), contra-variance of anonymous function parameter types is not supported.</span></span> <span data-ttu-id="08560-2408">Pokud anonymní funkce nemá *anonymous_function_signature*, sada kompatibilních typů delegátů a typů stromu výrazů je omezena na ty, které nemají žádné parametry `out`.</span><span class="sxs-lookup"><span data-stu-id="08560-2408">If an anonymous function does not have an *anonymous_function_signature*, then the set of compatible delegate types and expression tree types is restricted to those that have no `out` parameters.</span></span>

<span data-ttu-id="08560-2409">Všimněte si, že *anonymous_function_signature* nemůže obsahovat atributy ani pole parametrů.</span><span class="sxs-lookup"><span data-stu-id="08560-2409">Note that an *anonymous_function_signature* cannot include attributes or a parameter array.</span></span> <span data-ttu-id="08560-2410">*Anonymous_function_signature* však může být kompatibilní s typem delegáta, jehož seznam parametrů obsahuje pole parametrů.</span><span class="sxs-lookup"><span data-stu-id="08560-2410">Nevertheless, an *anonymous_function_signature* may be compatible with a delegate type whose parameter list contains a parameter array.</span></span>

<span data-ttu-id="08560-2411">Všimněte si také, že převod na typ stromu výrazu, i když je kompatibilní, může stále selhat v době kompilace ([typy stromu výrazů](types.md#expression-tree-types)).</span><span class="sxs-lookup"><span data-stu-id="08560-2411">Note also that conversion to an expression tree type, even if compatible, may still fail at compile-time ([Expression tree types](types.md#expression-tree-types)).</span></span>

### <a name="anonymous-function-bodies"></a><span data-ttu-id="08560-2412">Anonymní orgány funkcí</span><span class="sxs-lookup"><span data-stu-id="08560-2412">Anonymous function bodies</span></span>

<span data-ttu-id="08560-2413">Tělo (*výraz* nebo *blok*) anonymní funkce podléhá následujícím pravidlům:</span><span class="sxs-lookup"><span data-stu-id="08560-2413">The body (*expression* or *block*) of an anonymous function is subject to the following rules:</span></span>

*  <span data-ttu-id="08560-2414">Pokud anonymní funkce obsahuje podpis, jsou v těle k dispozici parametry zadané v podpisu.</span><span class="sxs-lookup"><span data-stu-id="08560-2414">If the anonymous function includes a signature, the parameters specified in the signature are available in the body.</span></span> <span data-ttu-id="08560-2415">Pokud anonymní funkce nemá žádný podpis, může být převedena na typ delegáta nebo typ výrazu s parametry ([anonymní převody funkcí](conversions.md#anonymous-function-conversions)), ale k těmto parametrům nelze přistupovat v těle.</span><span class="sxs-lookup"><span data-stu-id="08560-2415">If the anonymous function has no signature it can be converted to a delegate type or expression type having parameters ([Anonymous function conversions](conversions.md#anonymous-function-conversions)), but the parameters cannot be accessed in the body.</span></span>
*  <span data-ttu-id="08560-2416">S výjimkou parametrů `ref` nebo `out` zadaných v signatuře (pokud existuje) nejbližší nadřazené anonymní funkce se jedná o chybu v době kompilace, pro kterou tělo přistupuje k `ref` nebo `out`mu parametru.</span><span class="sxs-lookup"><span data-stu-id="08560-2416">Except for `ref` or `out` parameters specified in the signature (if any) of the nearest enclosing anonymous function, it is a compile-time error for the body to access a `ref` or `out` parameter.</span></span>
*  <span data-ttu-id="08560-2417">Pokud je typ `this` typem struktury, jedná se o chybu při kompilaci pro tělo pro přístup k `this`.</span><span class="sxs-lookup"><span data-stu-id="08560-2417">When the type of `this` is a struct type, it is a compile-time error for the body to access `this`.</span></span> <span data-ttu-id="08560-2418">To platí bez ohledu na to, zda je přístup explicitní (jako v `this.x`) nebo implicitní (jako v `x`, kde `x` je člen instance struktury).</span><span class="sxs-lookup"><span data-stu-id="08560-2418">This is true whether the access is explicit (as in `this.x`) or implicit (as in `x` where `x` is an instance member of the struct).</span></span> <span data-ttu-id="08560-2419">Toto pravidlo jednoduše zakazuje takový přístup a nemá vliv na to, jestli výsledky hledání členů jsou v členu struktury.</span><span class="sxs-lookup"><span data-stu-id="08560-2419">This rule simply prohibits such access and does not affect whether member lookup results in a member of the struct.</span></span>
*  <span data-ttu-id="08560-2420">Tělo má přístup k vnějším proměnným ([vnějším proměnným](expressions.md#outer-variables)) anonymní funkce.</span><span class="sxs-lookup"><span data-stu-id="08560-2420">The body has access to the outer variables ([Outer variables](expressions.md#outer-variables)) of the anonymous function.</span></span> <span data-ttu-id="08560-2421">Přístup k vnější proměnné bude odkazovat na instanci proměnné, která je aktivní v okamžiku vyhodnocení *lambda_expression* nebo *anonymous_method_expression* ([vyhodnocení výrazů anonymních funkcí](expressions.md#evaluation-of-anonymous-function-expressions)).</span><span class="sxs-lookup"><span data-stu-id="08560-2421">Access of an outer variable will reference the instance of the variable that is active at the time the *lambda_expression* or *anonymous_method_expression* is evaluated ([Evaluation of anonymous function expressions](expressions.md#evaluation-of-anonymous-function-expressions)).</span></span>
*  <span data-ttu-id="08560-2422">Jedná se o chybu při kompilaci pro tělo, které obsahuje příkaz `goto`, příkaz `break` nebo `continue` příkaz, jehož cíl je mimo tělo nebo v těle obsažené anonymní funkce.</span><span class="sxs-lookup"><span data-stu-id="08560-2422">It is a compile-time error for the body to contain a `goto` statement, `break` statement, or `continue` statement whose target is outside the body or within the body of a contained anonymous function.</span></span>
*  <span data-ttu-id="08560-2423">Příkaz `return` v těle vrátí řízení od vyvolání nejbližší nadřazené anonymní funkce, nikoli nadřazeného člena funkce.</span><span class="sxs-lookup"><span data-stu-id="08560-2423">A `return` statement in the body returns control from an invocation of the nearest enclosing anonymous function, not from the enclosing function member.</span></span> <span data-ttu-id="08560-2424">Výraz zadaný v příkazu `return` musí být implicitně převeden na návratový typ typu delegáta nebo stromu výrazu, na který je nejbližší nadřazený *lambda_expression* nebo *Anonymous_method_expression* převeden ([anonymní převod funkcí](conversions.md#anonymous-function-conversions)).</span><span class="sxs-lookup"><span data-stu-id="08560-2424">An expression specified in a `return` statement must be implicitly convertible to the return type of the delegate type or expression tree type to which the nearest enclosing *lambda_expression* or *anonymous_method_expression* is converted ([Anonymous function conversions](conversions.md#anonymous-function-conversions)).</span></span>

<span data-ttu-id="08560-2425">Explicitně neurčuje, zda existuje nějaký způsob, jak spustit blok anonymní funkce jiné než prostřednictvím vyhodnocení a volání *lambda_expression* nebo *anonymous_method_expression*.</span><span class="sxs-lookup"><span data-stu-id="08560-2425">It is explicitly unspecified whether there is any way to execute the block of an anonymous function other than through evaluation and invocation of the *lambda_expression* or *anonymous_method_expression*.</span></span> <span data-ttu-id="08560-2426">Konkrétně se může kompilátor rozhodnout implementovat anonymní funkci vysyntetizující jednu nebo více pojmenovaných metod nebo typů.</span><span class="sxs-lookup"><span data-stu-id="08560-2426">In particular, the compiler may choose to implement an anonymous function by synthesizing one or more named methods or types.</span></span> <span data-ttu-id="08560-2427">Názvy těchto syntetizních prvků musí mít tvar vyhrazený pro použití kompilátorem.</span><span class="sxs-lookup"><span data-stu-id="08560-2427">The names of any such synthesized elements must be of a form reserved for compiler use.</span></span>

### <a name="overload-resolution-and-anonymous-functions"></a><span data-ttu-id="08560-2428">Rozlišení přetěžování a anonymní funkce</span><span class="sxs-lookup"><span data-stu-id="08560-2428">Overload resolution and anonymous functions</span></span>

<span data-ttu-id="08560-2429">Anonymní funkce v seznamu argumentů se účastní odvození typu a řešení přetížení.</span><span class="sxs-lookup"><span data-stu-id="08560-2429">Anonymous functions in an argument list participate in type inference and overload resolution.</span></span> <span data-ttu-id="08560-2430">Přesné pravidla naleznete v tématu věnovaném [odvození typů](expressions.md#type-inference) a [rozlišení přetěžování](expressions.md#overload-resolution) .</span><span class="sxs-lookup"><span data-stu-id="08560-2430">Please refer to [Type inference](expressions.md#type-inference) and [Overload resolution](expressions.md#overload-resolution) for the exact rules.</span></span>

<span data-ttu-id="08560-2431">Následující příklad ukazuje účinek anonymních funkcí na řešení přetížení.</span><span class="sxs-lookup"><span data-stu-id="08560-2431">The following example illustrates the effect of anonymous functions on overload resolution.</span></span>

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

<span data-ttu-id="08560-2432">Třída `ItemList<T>` má dvě metody `Sum`.</span><span class="sxs-lookup"><span data-stu-id="08560-2432">The `ItemList<T>` class has two `Sum` methods.</span></span> <span data-ttu-id="08560-2433">Každá z nich přebírá argument `selector`, který extrahuje hodnotu, která se má sečíst z položky seznamu.</span><span class="sxs-lookup"><span data-stu-id="08560-2433">Each takes a `selector` argument, which extracts the value to sum over from a list item.</span></span> <span data-ttu-id="08560-2434">Extrahovaná hodnota může být buď `int`, nebo `double` a výsledný součet je také buď `int`, nebo `double`.</span><span class="sxs-lookup"><span data-stu-id="08560-2434">The extracted value can be either an `int` or a `double` and the resulting sum is likewise either an `int` or a `double`.</span></span>

<span data-ttu-id="08560-2435">Metody `Sum` mohou být například použity pro výpočet součtů ze seznamu řádků podrobností v objednávce.</span><span class="sxs-lookup"><span data-stu-id="08560-2435">The `Sum` methods could for example be used to compute sums from a list of detail lines in an order.</span></span>

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

<span data-ttu-id="08560-2436">Při prvním vyvolání `orderDetails.Sum`lze použít obě metody `Sum`, protože anonymní funkce `d => d. UnitCount` je kompatibilní s `Func<Detail,int>` i `Func<Detail,double>`.</span><span class="sxs-lookup"><span data-stu-id="08560-2436">In the first invocation of `orderDetails.Sum`, both `Sum` methods are applicable because the anonymous function `d => d. UnitCount` is compatible with both `Func<Detail,int>` and `Func<Detail,double>`.</span></span> <span data-ttu-id="08560-2437">Řešení přetížení však vybere první `Sum` metodu, protože převod na `Func<Detail,int>` je lepší než převod na `Func<Detail,double>`.</span><span class="sxs-lookup"><span data-stu-id="08560-2437">However, overload resolution picks the first `Sum` method because the conversion to `Func<Detail,int>` is better than the conversion to `Func<Detail,double>`.</span></span>

<span data-ttu-id="08560-2438">Ve druhém vyvolání `orderDetails.Sum`lze použít pouze druhou metodu `Sum`, protože anonymní funkce `d => d.UnitPrice * d.UnitCount` vytváří hodnotu typu `double`.</span><span class="sxs-lookup"><span data-stu-id="08560-2438">In the second invocation of `orderDetails.Sum`, only the second `Sum` method is applicable because the anonymous function `d => d.UnitPrice * d.UnitCount` produces a value of type `double`.</span></span> <span data-ttu-id="08560-2439">Proto řešení přetížení vybere druhou metodu `Sum` pro toto vyvolání.</span><span class="sxs-lookup"><span data-stu-id="08560-2439">Thus, overload resolution picks the second `Sum` method for that invocation.</span></span>

### <a name="anonymous-functions-and-dynamic-binding"></a><span data-ttu-id="08560-2440">Anonymní funkce a dynamická vazba</span><span class="sxs-lookup"><span data-stu-id="08560-2440">Anonymous functions and dynamic binding</span></span>

<span data-ttu-id="08560-2441">Anonymní funkce nemůže být přijímač, argument nebo operand dynamicky vázané operace.</span><span class="sxs-lookup"><span data-stu-id="08560-2441">An anonymous function cannot be a receiver, argument or operand of a dynamically bound operation.</span></span>

### <a name="outer-variables"></a><span data-ttu-id="08560-2442">Vnější proměnné</span><span class="sxs-lookup"><span data-stu-id="08560-2442">Outer variables</span></span>

<span data-ttu-id="08560-2443">Jakákoli místní proměnná, parametr hodnoty nebo pole parametrů, jejichž obor zahrnuje *lambda_expression* nebo *anonymous_method_expression* se nazývá ***vnější proměnná*** anonymní funkce.</span><span class="sxs-lookup"><span data-stu-id="08560-2443">Any local variable, value parameter, or parameter array whose scope includes the *lambda_expression* or *anonymous_method_expression* is called an ***outer variable*** of the anonymous function.</span></span> <span data-ttu-id="08560-2444">V členu funkce instance třídy je hodnota `this` považována za parametr hodnoty a je vnější proměnná jakékoli anonymní funkce obsažené v rámci člena funkce.</span><span class="sxs-lookup"><span data-stu-id="08560-2444">In an instance function member of a class, the `this` value is considered a value parameter and is an outer variable of any anonymous function contained within the function member.</span></span>

#### <a name="captured-outer-variables"></a><span data-ttu-id="08560-2445">Zachycené vnější proměnné</span><span class="sxs-lookup"><span data-stu-id="08560-2445">Captured outer variables</span></span>

<span data-ttu-id="08560-2446">Pokud je vnější proměnná odkazována anonymní funkcí, je vnější proměnná označována jako ***zachycena*** anonymní funkcí.</span><span class="sxs-lookup"><span data-stu-id="08560-2446">When an outer variable is referenced by an anonymous function, the outer variable is said to have been ***captured*** by the anonymous function.</span></span> <span data-ttu-id="08560-2447">Obvykle je doba života místní proměnné omezena na provedení bloku nebo příkazu, ke kterému je přidruženo ([místní proměnné](variables.md#local-variables)).</span><span class="sxs-lookup"><span data-stu-id="08560-2447">Ordinarily, the lifetime of a local variable is limited to execution of the block or statement with which it is associated ([Local variables](variables.md#local-variables)).</span></span> <span data-ttu-id="08560-2448">Doba trvání zachycené vnější proměnné se ale rozšířila aspoň na to, dokud se stromové struktury nebo stromu výrazů vytvořené z anonymní funkce neprojeví pro uvolňování paměti.</span><span class="sxs-lookup"><span data-stu-id="08560-2448">However, the lifetime of a captured outer variable is extended at least until the delegate or expression tree created from the anonymous function becomes eligible for garbage collection.</span></span>

<span data-ttu-id="08560-2449">V příkladu</span><span class="sxs-lookup"><span data-stu-id="08560-2449">In the example</span></span>
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
<span data-ttu-id="08560-2450">místní proměnná `x` je zachycena anonymní funkcí a doba `x` je rozšířena alespoň do doby, než se delegát vrácený z `F` stane nárokem na uvolňování paměti (ke kterému nedojde až do konce programu).</span><span class="sxs-lookup"><span data-stu-id="08560-2450">the local variable `x` is captured by the anonymous function, and the lifetime of `x` is extended at least until the delegate returned from `F` becomes eligible for garbage collection (which doesn't happen until the very end of the program).</span></span> <span data-ttu-id="08560-2451">Vzhledem k tomu, že každé vyvolání anonymní funkce funguje na stejné instanci `x`, výstup tohoto příkladu je:</span><span class="sxs-lookup"><span data-stu-id="08560-2451">Since each invocation of the anonymous function operates on the same instance of `x`, the output of the example is:</span></span>
```console
1
2
3
```

<span data-ttu-id="08560-2452">Je-li lokální proměnná nebo parametr hodnoty zachycen anonymní funkcí, místní proměnná nebo parametr již není považována za pevnou proměnnou ([pevné a mobilní proměnné](unsafe-code.md#fixed-and-moveable-variables)), ale místo toho je považována za pohyblivou proměnnou.</span><span class="sxs-lookup"><span data-stu-id="08560-2452">When a local variable or a value parameter is captured by an anonymous function, the local variable or parameter is no longer considered to be a fixed variable ([Fixed and moveable variables](unsafe-code.md#fixed-and-moveable-variables)), but is instead considered to be a moveable variable.</span></span> <span data-ttu-id="08560-2453">Takže jakýkoli `unsafe` kód, který přebírá adresu zachycené vnější proměnné, musí nejprve použít příkaz `fixed` k opravě proměnné.</span><span class="sxs-lookup"><span data-stu-id="08560-2453">Thus any `unsafe` code that takes the address of a captured outer variable must first use the `fixed` statement to fix the variable.</span></span>

<span data-ttu-id="08560-2454">Všimněte si, že na rozdíl od nezachycené proměnné může být zachycená lokální proměnná současně vystavena více vláknům provádění.</span><span class="sxs-lookup"><span data-stu-id="08560-2454">Note that unlike an uncaptured variable, a captured local variable can be simultaneously exposed to multiple threads of execution.</span></span>

#### <a name="instantiation-of-local-variables"></a><span data-ttu-id="08560-2455">Vytváření instancí místních proměnných</span><span class="sxs-lookup"><span data-stu-id="08560-2455">Instantiation of local variables</span></span>

<span data-ttu-id="08560-2456">Místní proměnná je považována za ***instanci*** , pokud provádění vstoupí do oboru proměnné.</span><span class="sxs-lookup"><span data-stu-id="08560-2456">A local variable is considered to be ***instantiated*** when execution enters the scope of the variable.</span></span> <span data-ttu-id="08560-2457">Například při vyvolání následující metody je místní proměnná `x` vytvořena a inicializována třikrát – jednou pro každou iteraci smyčky.</span><span class="sxs-lookup"><span data-stu-id="08560-2457">For example, when the following method is invoked, the local variable `x` is instantiated and initialized three times—once for each iteration of the loop.</span></span>

```csharp
static void F() {
    for (int i = 0; i < 3; i++) {
        int x = i * 2 + 1;
        ...
    }
}
```

<span data-ttu-id="08560-2458">Nicméně přesun deklarace `x` mimo smyčku má za následek jednu instanci `x`:</span><span class="sxs-lookup"><span data-stu-id="08560-2458">However, moving the declaration of `x` outside the loop results in a single instantiation of `x`:</span></span>
```csharp
static void F() {
    int x;
    for (int i = 0; i < 3; i++) {
        x = i * 2 + 1;
        ...
    }
}
```

<span data-ttu-id="08560-2459">Pokud není zachyceno, neexistuje způsob, jak přesně sledovat, jak často je vytvořena instance místní proměnné – protože životnost instancí jsou nesouvislé, je možné, že každá instance bude jednoduše používat stejné umístění úložiště.</span><span class="sxs-lookup"><span data-stu-id="08560-2459">When not captured, there is no way to observe exactly how often a local variable is instantiated—because the lifetimes of the instantiations are disjoint, it is possible for each instantiation to simply use the same storage location.</span></span> <span data-ttu-id="08560-2460">Nicméně, pokud anonymní funkce zachytí místní proměnnou, projeví se zřejmé účinky vytváření instancí.</span><span class="sxs-lookup"><span data-stu-id="08560-2460">However, when an anonymous function captures a local variable, the effects of instantiation become apparent.</span></span>

<span data-ttu-id="08560-2461">Příklad</span><span class="sxs-lookup"><span data-stu-id="08560-2461">The example</span></span>
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
<span data-ttu-id="08560-2462">Vytvoří výstup:</span><span class="sxs-lookup"><span data-stu-id="08560-2462">produces the output:</span></span>
```console
1
3
5
```

<span data-ttu-id="08560-2463">Pokud je však deklarace `x` přesunuta mimo smyčku:</span><span class="sxs-lookup"><span data-stu-id="08560-2463">However, when the declaration of `x` is moved outside the loop:</span></span>
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
<span data-ttu-id="08560-2464">výstup je:</span><span class="sxs-lookup"><span data-stu-id="08560-2464">the output is:</span></span>
```console
5
5
5
```

<span data-ttu-id="08560-2465">Pokud smyčka for deklaruje proměnnou iterace, tato proměnná je považována za deklaraci mimo smyčku.</span><span class="sxs-lookup"><span data-stu-id="08560-2465">If a for-loop declares an iteration variable, that variable itself is considered to be declared outside of the loop.</span></span> <span data-ttu-id="08560-2466">Proto, pokud je změněn příklad pro zachycení samotné proměnné iterace:</span><span class="sxs-lookup"><span data-stu-id="08560-2466">Thus, if the example is changed to capture the iteration variable itself:</span></span>

```csharp
static D[] F() {
    D[] result = new D[3];
    for (int i = 0; i < 3; i++) {
        result[i] = () => { Console.WriteLine(i); };
    }
    return result;
}
```
<span data-ttu-id="08560-2467">je zachycena pouze jedna instance iterační proměnné, která vytváří výstup:</span><span class="sxs-lookup"><span data-stu-id="08560-2467">only one instance of the iteration variable is captured, which produces the output:</span></span>
```console
3
3
3
```

<span data-ttu-id="08560-2468">Je možné, aby delegáti anonymní funkce sdíleli některé zachycené proměnné, ale mají samostatné instance jiných.</span><span class="sxs-lookup"><span data-stu-id="08560-2468">It is possible for anonymous function delegates to share some captured variables yet have separate instances of others.</span></span> <span data-ttu-id="08560-2469">Například pokud se `F` změní na</span><span class="sxs-lookup"><span data-stu-id="08560-2469">For example, if `F` is changed to</span></span>
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
<span data-ttu-id="08560-2470">tři Delegáti zachytí stejnou instanci `x`, ale samostatné instance `y`a výstup je:</span><span class="sxs-lookup"><span data-stu-id="08560-2470">the three delegates capture the same instance of `x` but separate instances of `y`, and the output is:</span></span>
```console
1 1
2 1
3 1
```

<span data-ttu-id="08560-2471">Samostatné anonymní funkce mohou zachytit stejnou instanci vnější proměnné.</span><span class="sxs-lookup"><span data-stu-id="08560-2471">Separate anonymous functions can capture the same instance of an outer variable.</span></span> <span data-ttu-id="08560-2472">V tomto příkladu:</span><span class="sxs-lookup"><span data-stu-id="08560-2472">In the example:</span></span>
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
<span data-ttu-id="08560-2473">Tyto dvě anonymní funkce zachytí stejnou instanci lokální proměnné `x`a mohou tedy "komunikovat" prostřednictvím této proměnné.</span><span class="sxs-lookup"><span data-stu-id="08560-2473">the two anonymous functions capture the same instance of the local variable `x`, and they can thus "communicate" through that variable.</span></span> <span data-ttu-id="08560-2474">Výstup tohoto příkladu je:</span><span class="sxs-lookup"><span data-stu-id="08560-2474">The output of the example is:</span></span>
```console
5
10
```

### <a name="evaluation-of-anonymous-function-expressions"></a><span data-ttu-id="08560-2475">Vyhodnocení anonymních výrazů funkce</span><span class="sxs-lookup"><span data-stu-id="08560-2475">Evaluation of anonymous function expressions</span></span>

<span data-ttu-id="08560-2476">Anonymní funkce `F` musí být vždy převedena na typ delegáta `D` nebo typ stromové struktury výrazu `E`, a to buď přímo, nebo prostřednictvím spuštění výrazu vytvoření delegáta `new D(F)`.</span><span class="sxs-lookup"><span data-stu-id="08560-2476">An anonymous function `F` must always be converted to a delegate type `D` or an expression tree type `E`, either directly or through the execution of a delegate creation expression `new D(F)`.</span></span> <span data-ttu-id="08560-2477">Tento převod Určuje výsledek anonymní funkce, jak je popsáno v tématu [anonymní převody funkcí](conversions.md#anonymous-function-conversions).</span><span class="sxs-lookup"><span data-stu-id="08560-2477">This conversion determines the result of the anonymous function, as described in [Anonymous function conversions](conversions.md#anonymous-function-conversions).</span></span>

## <a name="query-expressions"></a><span data-ttu-id="08560-2478">Výrazy dotazů</span><span class="sxs-lookup"><span data-stu-id="08560-2478">Query expressions</span></span>

<span data-ttu-id="08560-2479">***Výrazy dotazů*** poskytují syntaxi jazyka integrovanou do dotazů, které jsou podobné relačním a hierarchickým dotazovacím jazykům, jako jsou SQL a XQuery.</span><span class="sxs-lookup"><span data-stu-id="08560-2479">***Query expressions*** provide a language integrated syntax for queries that is similar to relational and hierarchical query languages such as SQL and XQuery.</span></span>

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

<span data-ttu-id="08560-2480">Výraz dotazu začíná klauzulí `from` a končí buď klauzulí `select` nebo `group`.</span><span class="sxs-lookup"><span data-stu-id="08560-2480">A query expression begins with a `from` clause and ends with either a `select` or `group` clause.</span></span> <span data-ttu-id="08560-2481">Za počáteční klauzulí `from` může následovat nula nebo více `from`, `let`, `where`, `join` nebo `orderby` klauzulí.</span><span class="sxs-lookup"><span data-stu-id="08560-2481">The initial `from` clause can be followed by zero or more `from`, `let`, `where`, `join` or `orderby` clauses.</span></span> <span data-ttu-id="08560-2482">Každá klauzule `from` je generátor, který zavádí ***proměnnou rozsahu*** , která je v rozsahu nad prvky ***sekvence***.</span><span class="sxs-lookup"><span data-stu-id="08560-2482">Each `from` clause is a generator introducing a ***range variable*** which ranges over the elements of a ***sequence***.</span></span> <span data-ttu-id="08560-2483">Každá klauzule `let` zavádí proměnnou rozsahu představující hodnotu vypočítanou v rámci předchozích proměnných rozsahu.</span><span class="sxs-lookup"><span data-stu-id="08560-2483">Each `let` clause introduces a range variable representing a value computed by means of previous range variables.</span></span> <span data-ttu-id="08560-2484">Každá klauzule `where` je filtr, který vyloučí položky z výsledku.</span><span class="sxs-lookup"><span data-stu-id="08560-2484">Each `where` clause is a filter that excludes items from the result.</span></span> <span data-ttu-id="08560-2485">Každá klauzule `join` porovnává zadané klíče zdrojové sekvence s klíči jiné sekvence a předává dvojice párů.</span><span class="sxs-lookup"><span data-stu-id="08560-2485">Each `join` clause compares specified keys of the source sequence with keys of another sequence, yielding matching pairs.</span></span> <span data-ttu-id="08560-2486">Každá klauzule `orderby` v závislosti na zadaných kritériích seřazení položek. Poslední `select` nebo klauzule `group` určuje tvar výsledku z hodnot proměnných rozsahu.</span><span class="sxs-lookup"><span data-stu-id="08560-2486">Each `orderby` clause reorders items according to specified criteria.The final `select` or `group` clause specifies the shape of the result in terms of the range variables.</span></span> <span data-ttu-id="08560-2487">Nakonec můžete použít klauzuli `into` k "spojování" dotazům tím, že se výsledky jednoho dotazu považují za generátor v následném dotazu.</span><span class="sxs-lookup"><span data-stu-id="08560-2487">Finally, an `into` clause can be used to "splice" queries by treating the results of one query as a generator in a subsequent query.</span></span>

### <a name="ambiguities-in-query-expressions"></a><span data-ttu-id="08560-2488">Nejednoznačné výrazy ve výrazech dotazů</span><span class="sxs-lookup"><span data-stu-id="08560-2488">Ambiguities in query expressions</span></span>

<span data-ttu-id="08560-2489">Výrazy dotazů obsahují počet kontextových klíčových slov, tj. identifikátory, které mají v daném kontextu zvláštní význam.</span><span class="sxs-lookup"><span data-stu-id="08560-2489">Query expressions contain a number of "contextual keywords", i.e., identifiers that have special meaning in a given context.</span></span> <span data-ttu-id="08560-2490">Konkrétně se jedná o `from`, `where`, `join`, `on`, `equals`, `into`, `let`, `orderby`, `ascending`, `descending`, `select`, `group` a `by`.</span><span class="sxs-lookup"><span data-stu-id="08560-2490">Specifically these are `from`, `where`, `join`, `on`, `equals`, `into`, `let`, `orderby`, `ascending`, `descending`, `select`, `group` and `by`.</span></span> <span data-ttu-id="08560-2491">Aby nedocházelo k nejednoznačnosti ve výrazech dotazů způsobených smíšeným použitím těchto identifikátorů jako klíčová slova nebo jednoduché názvy, jsou tyto identifikátory považovány za klíčová slova při výskytu kamkoli v rámci výrazu dotazu.</span><span class="sxs-lookup"><span data-stu-id="08560-2491">In order to avoid ambiguities in query expressions caused by mixed use of these identifiers as keywords or simple names, these identifiers are considered keywords when occurring anywhere within a query expression.</span></span>

<span data-ttu-id="08560-2492">Pro tento účel výraz dotazu je libovolný výraz, který začíná řetězcem "`from identifier`" následovaný jakýmkoli tokenem, s výjimkou "`;`", "`=`" nebo "`,`".</span><span class="sxs-lookup"><span data-stu-id="08560-2492">For this purpose, a query expression is any expression that starts with "`from identifier`" followed by any token except "`;`", "`=`" or "`,`".</span></span>

<span data-ttu-id="08560-2493">Aby bylo možné použít tato slova jako identifikátory v rámci výrazu dotazu, mohou být předponou "`@`" ([identifikátory](lexical-structure.md#identifiers)).</span><span class="sxs-lookup"><span data-stu-id="08560-2493">In order to use these words as identifiers within a query expression, they can be prefixed with "`@`" ([Identifiers](lexical-structure.md#identifiers)).</span></span>

### <a name="query-expression-translation"></a><span data-ttu-id="08560-2494">Překlad výrazu dotazu</span><span class="sxs-lookup"><span data-stu-id="08560-2494">Query expression translation</span></span>

<span data-ttu-id="08560-2495">C# Jazyk neurčuje sémantiku provádění výrazů dotazů.</span><span class="sxs-lookup"><span data-stu-id="08560-2495">The C# language does not specify the execution semantics of query expressions.</span></span> <span data-ttu-id="08560-2496">Místo toho jsou výrazy dotazu přeloženy na vyvolání metod, které vyhovují *vzoru výrazu dotazu* ([vzor výrazu dotazu](expressions.md#the-query-expression-pattern)).</span><span class="sxs-lookup"><span data-stu-id="08560-2496">Rather, query expressions are translated into invocations of methods that adhere to the *query expression pattern* ([The query expression pattern](expressions.md#the-query-expression-pattern)).</span></span> <span data-ttu-id="08560-2497">Výrazy dotazů jsou konkrétně přeloženy na vyvolání metod s názvem `Where`, `Select`, `SelectMany`, `Join`, `GroupJoin`, `OrderBy`, `OrderByDescending`, `ThenBy`, `ThenByDescending`, `GroupBy`a `Cast`. U těchto metod se očekává, že budou mít konkrétní signatury a typy výsledků, jak je popsáno ve [vzoru výrazu dotazu](expressions.md#the-query-expression-pattern).</span><span class="sxs-lookup"><span data-stu-id="08560-2497">Specifically, query expressions are translated into invocations of methods named `Where`, `Select`, `SelectMany`, `Join`, `GroupJoin`, `OrderBy`, `OrderByDescending`, `ThenBy`, `ThenByDescending`, `GroupBy`, and `Cast`.These methods are expected to have particular signatures and result types, as described in [The query expression pattern](expressions.md#the-query-expression-pattern).</span></span> <span data-ttu-id="08560-2498">Tyto metody mohou být instanční metody objektu, na který se dotazuje, nebo metody rozšíření, které jsou pro objekt externí, a implementují skutečné provedení dotazu.</span><span class="sxs-lookup"><span data-stu-id="08560-2498">These methods can be instance methods of the object being queried or extension methods that are external to the object, and they implement the actual execution of the query.</span></span>

<span data-ttu-id="08560-2499">Překlad z výrazů dotazů na vyvolání metody je syntaktické mapování, které se vyskytuje před provedením jakékoli vazby typu nebo vyřešení přetížení.</span><span class="sxs-lookup"><span data-stu-id="08560-2499">The translation from query expressions to method invocations is a syntactic mapping that occurs before any type binding or overload resolution has been performed.</span></span> <span data-ttu-id="08560-2500">Je zaručeno, že překlad bude syntakticky správný, ale není zaručeno vydávat sémanticky správný C# kód.</span><span class="sxs-lookup"><span data-stu-id="08560-2500">The translation is guaranteed to be syntactically correct, but it is not guaranteed to produce semantically correct C# code.</span></span> <span data-ttu-id="08560-2501">Po překladu výrazů dotazů jsou výsledná volání metody zpracovávána jako běžná volání metod, což může vést k chybám při zjištění chyb, například pokud tyto metody neexistují, pokud argumenty mají chybné typy nebo pokud jsou metody obecné a odvození typu se nezdařilo.</span><span class="sxs-lookup"><span data-stu-id="08560-2501">Following translation of query expressions, the resulting method invocations are processed as regular method invocations, and this may in turn uncover errors, for example if the methods do not exist, if arguments have wrong types, or if the methods are generic and type inference fails.</span></span>

<span data-ttu-id="08560-2502">Výraz dotazu se zpracovává opakovaným použitím následujících překladů, dokud nebudou možná žádná další snížení.</span><span class="sxs-lookup"><span data-stu-id="08560-2502">A query expression is processed by repeatedly applying the following translations until no further reductions are possible.</span></span> <span data-ttu-id="08560-2503">Překlady jsou uvedeny v pořadí aplikace: každá část předpokládá, že překlady v předchozích částech byly vyčerpány a po jejím vyčerpání se oddíl nebude později znovu navštěvovat při zpracování stejného výrazu dotazu.</span><span class="sxs-lookup"><span data-stu-id="08560-2503">The translations are listed in order of application: each section assumes that the translations in the preceding sections have been performed exhaustively, and once exhausted, a section will not later be revisited in the processing of the same query expression.</span></span>

<span data-ttu-id="08560-2504">Přiřazení k proměnným rozsahu není ve výrazech dotazů povoleno.</span><span class="sxs-lookup"><span data-stu-id="08560-2504">Assignment to range variables is not allowed in query expressions.</span></span> <span data-ttu-id="08560-2505">Nicméně C# implementace je dovoleno, aby toto omezení vynutilo vždy, protože to může být někdy možné pouze pomocí syntaktického schématu překladu, který je zde zobrazen.</span><span class="sxs-lookup"><span data-stu-id="08560-2505">However a C# implementation is permitted to not always enforce this restriction, since this may sometimes not be possible with the syntactic translation scheme presented here.</span></span>

<span data-ttu-id="08560-2506">Některé překlady vloží proměnné rozsahu s transparentními identifikátory, které jsou označeny `*`.</span><span class="sxs-lookup"><span data-stu-id="08560-2506">Certain translations inject range variables with transparent identifiers denoted by `*`.</span></span> <span data-ttu-id="08560-2507">Speciální vlastnosti transparentních identifikátorů jsou podrobněji popsány v [transparentní identifikátory](expressions.md#transparent-identifiers).</span><span class="sxs-lookup"><span data-stu-id="08560-2507">The special properties of transparent identifiers are discussed further in [Transparent identifiers](expressions.md#transparent-identifiers).</span></span>

#### <a name="select-and-groupby-clauses-with-continuations"></a><span data-ttu-id="08560-2508">Klauzule SELECT a GroupBy s pokračováními</span><span class="sxs-lookup"><span data-stu-id="08560-2508">Select and groupby clauses with continuations</span></span>

<span data-ttu-id="08560-2509">Výraz dotazu s pokračováním</span><span class="sxs-lookup"><span data-stu-id="08560-2509">A query expression with a continuation</span></span>
```csharp
from ... into x ...
```
<span data-ttu-id="08560-2510">je přeloženo do</span><span class="sxs-lookup"><span data-stu-id="08560-2510">is translated into</span></span>
```csharp
from x in ( from ... ) ...
```

<span data-ttu-id="08560-2511">Překlady v následujících částech předpokládají, že dotazy nemají žádné `into` pokračování.</span><span class="sxs-lookup"><span data-stu-id="08560-2511">The translations in the following sections assume that queries have no `into` continuations.</span></span>

<span data-ttu-id="08560-2512">Příklad</span><span class="sxs-lookup"><span data-stu-id="08560-2512">The example</span></span>
```csharp
from c in customers
group c by c.Country into g
select new { Country = g.Key, CustCount = g.Count() }
```
<span data-ttu-id="08560-2513">je přeloženo do</span><span class="sxs-lookup"><span data-stu-id="08560-2513">is translated into</span></span>
```csharp
from g in
    from c in customers
    group c by c.Country
select new { Country = g.Key, CustCount = g.Count() }
```
<span data-ttu-id="08560-2514">konečný překlad, který je</span><span class="sxs-lookup"><span data-stu-id="08560-2514">the final translation of which is</span></span>
```csharp
customers.
GroupBy(c => c.Country).
Select(g => new { Country = g.Key, CustCount = g.Count() })
```

#### <a name="explicit-range-variable-types"></a><span data-ttu-id="08560-2515">Explicitní typy proměnných rozsahu</span><span class="sxs-lookup"><span data-stu-id="08560-2515">Explicit range variable types</span></span>

<span data-ttu-id="08560-2516">Klauzule `from`, která explicitně určuje typ proměnné rozsahu</span><span class="sxs-lookup"><span data-stu-id="08560-2516">A `from` clause that explicitly specifies a range variable type</span></span>
```csharp
from T x in e
```
<span data-ttu-id="08560-2517">je přeloženo do</span><span class="sxs-lookup"><span data-stu-id="08560-2517">is translated into</span></span>
```csharp
from x in ( e ) . Cast < T > ( )
```

<span data-ttu-id="08560-2518">Klauzule `join`, která explicitně určuje typ proměnné rozsahu</span><span class="sxs-lookup"><span data-stu-id="08560-2518">A `join` clause that explicitly specifies a range variable type</span></span>
```csharp
join T x in e on k1 equals k2
```
<span data-ttu-id="08560-2519">je přeloženo do</span><span class="sxs-lookup"><span data-stu-id="08560-2519">is translated into</span></span>
```csharp
join x in ( e ) . Cast < T > ( ) on k1 equals k2
```

<span data-ttu-id="08560-2520">Překlady v následujících částech předpokládají, že dotazy nemají žádné explicitní typy proměnných rozsahu.</span><span class="sxs-lookup"><span data-stu-id="08560-2520">The translations in the following sections assume that queries have no explicit range variable types.</span></span>

<span data-ttu-id="08560-2521">Příklad</span><span class="sxs-lookup"><span data-stu-id="08560-2521">The example</span></span>
```csharp
from Customer c in customers
where c.City == "London"
select c
```
<span data-ttu-id="08560-2522">je přeloženo do</span><span class="sxs-lookup"><span data-stu-id="08560-2522">is translated into</span></span>
```csharp
from c in customers.Cast<Customer>()
where c.City == "London"
select c
```
<span data-ttu-id="08560-2523">konečný překlad, který je</span><span class="sxs-lookup"><span data-stu-id="08560-2523">the final translation of which is</span></span>
```csharp
customers.
Cast<Customer>().
Where(c => c.City == "London")
```

<span data-ttu-id="08560-2524">Explicitní typy proměnných rozsahu jsou užitečné pro dotazování kolekce, které implementují neobecné `IEnumerable` rozhraní, ale ne obecné rozhraní `IEnumerable<T>`.</span><span class="sxs-lookup"><span data-stu-id="08560-2524">Explicit range variable types are useful for querying collections that implement the non-generic `IEnumerable` interface, but not the generic `IEnumerable<T>` interface.</span></span> <span data-ttu-id="08560-2525">V předchozím příkladu by se jednalo o případ, kdy `customers` byly typu `ArrayList`.</span><span class="sxs-lookup"><span data-stu-id="08560-2525">In the example above, this would be the case if `customers` were of type `ArrayList`.</span></span>

#### <a name="degenerate-query-expressions"></a><span data-ttu-id="08560-2526">Degenerovat výrazy dotazů</span><span class="sxs-lookup"><span data-stu-id="08560-2526">Degenerate query expressions</span></span>

<span data-ttu-id="08560-2527">Výraz dotazu formuláře</span><span class="sxs-lookup"><span data-stu-id="08560-2527">A query expression of the form</span></span>
```csharp
from x in e select x
```
<span data-ttu-id="08560-2528">je přeloženo do</span><span class="sxs-lookup"><span data-stu-id="08560-2528">is translated into</span></span>
```csharp
( e ) . Select ( x => x )
```

<span data-ttu-id="08560-2529">Příklad</span><span class="sxs-lookup"><span data-stu-id="08560-2529">The example</span></span>
```csharp
from c in customers
select c
```
<span data-ttu-id="08560-2530">je přeloženo do</span><span class="sxs-lookup"><span data-stu-id="08560-2530">is translated into</span></span>
```csharp
customers.Select(c => c)
```

<span data-ttu-id="08560-2531">Negenerovaný výraz dotazu je ten, který triviální výběr prvků zdroje.</span><span class="sxs-lookup"><span data-stu-id="08560-2531">A degenerate query expression is one that trivially selects the elements of the source.</span></span> <span data-ttu-id="08560-2532">Později v pozdější fázi překladu se odstraní negenerované dotazy, které zavádějí jiné kroky překladu, a nahradí je jejich zdrojem.</span><span class="sxs-lookup"><span data-stu-id="08560-2532">A later phase of the translation removes degenerate queries introduced by other translation steps by replacing them with their source.</span></span> <span data-ttu-id="08560-2533">Je však důležité zajistit, aby výsledek výrazu dotazu nebyl nikdy samotný zdrojový objekt, protože by bylo možné odhalit typ a identitu zdroje pro klienta dotazu.</span><span class="sxs-lookup"><span data-stu-id="08560-2533">It is important however to ensure that the result of a query expression is never the source object itself, as that would reveal the type and identity of the source to the client of the query.</span></span> <span data-ttu-id="08560-2534">Proto tento krok chrání před vygenerováním dotazů napsaných přímo ve zdrojovém kódu explicitním voláním `Select` ve zdroji.</span><span class="sxs-lookup"><span data-stu-id="08560-2534">Therefore this step protects degenerate queries written directly in source code by explicitly calling `Select` on the source.</span></span> <span data-ttu-id="08560-2535">Následně až do implementací `Select` a dalších operátorů dotazů zajistíte, že tyto metody nikdy nevrátí samotný zdrojový objekt.</span><span class="sxs-lookup"><span data-stu-id="08560-2535">It is then up to the implementers of `Select` and other query operators to ensure that these methods never return the source object itself.</span></span>

#### <a name="from-let-where-join-and-orderby-clauses"></a><span data-ttu-id="08560-2536">Klauzule FROM, let, WHERE, Join a OrderBy</span><span class="sxs-lookup"><span data-stu-id="08560-2536">From, let, where, join and orderby clauses</span></span>

<span data-ttu-id="08560-2537">Výraz dotazu s druhou klauzulí `from` následovaný klauzulí `select`</span><span class="sxs-lookup"><span data-stu-id="08560-2537">A query expression with a second `from` clause followed by a `select` clause</span></span>
```csharp
from x1 in e1
from x2 in e2
select v
```
<span data-ttu-id="08560-2538">je přeloženo do</span><span class="sxs-lookup"><span data-stu-id="08560-2538">is translated into</span></span>
```csharp
( e1 ) . SelectMany( x1 => e2 , ( x1 , x2 ) => v )
```

<span data-ttu-id="08560-2539">Výraz dotazu s druhou klauzulí `from` následovaný jinou klauzulí, než je klauzule `select`:</span><span class="sxs-lookup"><span data-stu-id="08560-2539">A query expression with a second `from` clause followed by something other than a `select` clause:</span></span>

```csharp
from x1 in e1
from x2 in e2
...
```
<span data-ttu-id="08560-2540">je přeloženo do</span><span class="sxs-lookup"><span data-stu-id="08560-2540">is translated into</span></span>
```csharp
from * in ( e1 ) . SelectMany( x1 => e2 , ( x1 , x2 ) => new { x1 , x2 } )
...
```

<span data-ttu-id="08560-2541">Výraz dotazu s klauzulí `let`</span><span class="sxs-lookup"><span data-stu-id="08560-2541">A query expression with a `let` clause</span></span>
```csharp
from x in e
let y = f
...
```
<span data-ttu-id="08560-2542">je přeloženo do</span><span class="sxs-lookup"><span data-stu-id="08560-2542">is translated into</span></span>
```csharp
from * in ( e ) . Select ( x => new { x , y = f } )
...
```

<span data-ttu-id="08560-2543">Výraz dotazu s klauzulí `where`</span><span class="sxs-lookup"><span data-stu-id="08560-2543">A query expression with a `where` clause</span></span>
```csharp
from x in e
where f
...
```
<span data-ttu-id="08560-2544">je přeloženo do</span><span class="sxs-lookup"><span data-stu-id="08560-2544">is translated into</span></span>
```csharp
from x in ( e ) . Where ( x => f )
...
```

<span data-ttu-id="08560-2545">Výraz dotazu s klauzulí `join` bez `into` následovaného klauzulí `select`</span><span class="sxs-lookup"><span data-stu-id="08560-2545">A query expression with a `join` clause without an `into` followed by a `select` clause</span></span>
```csharp
from x1 in e1
join x2 in e2 on k1 equals k2
select v
```
<span data-ttu-id="08560-2546">je přeloženo do</span><span class="sxs-lookup"><span data-stu-id="08560-2546">is translated into</span></span>
```csharp
( e1 ) . Join( e2 , x1 => k1 , x2 => k2 , ( x1 , x2 ) => v )
```

<span data-ttu-id="08560-2547">Výraz dotazu s klauzulí `join` bez `into` následovaných jinou klauzulí `select`</span><span class="sxs-lookup"><span data-stu-id="08560-2547">A query expression with a `join` clause without an `into` followed by something other than a `select` clause</span></span>
```csharp
from x1 in e1
join x2 in e2 on k1 equals k2
...
```
<span data-ttu-id="08560-2548">je přeloženo do</span><span class="sxs-lookup"><span data-stu-id="08560-2548">is translated into</span></span>
```csharp
from * in ( e1 ) . Join( e2 , x1 => k1 , x2 => k2 , ( x1 , x2 ) => new { x1 , x2 })
...
```

<span data-ttu-id="08560-2549">Výraz dotazu s klauzulí `join` s `into` následovaný klauzulí `select`</span><span class="sxs-lookup"><span data-stu-id="08560-2549">A query expression with a `join` clause with an `into` followed by a `select` clause</span></span>
```csharp
from x1 in e1
join x2 in e2 on k1 equals k2 into g
select v
```
<span data-ttu-id="08560-2550">je přeloženo do</span><span class="sxs-lookup"><span data-stu-id="08560-2550">is translated into</span></span>
```csharp
( e1 ) . GroupJoin( e2 , x1 => k1 , x2 => k2 , ( x1 , g ) => v )
```

<span data-ttu-id="08560-2551">Výraz dotazu s klauzulí `join` s `into` následovaný jinou jinou než `select` klauzulí</span><span class="sxs-lookup"><span data-stu-id="08560-2551">A query expression with a `join` clause with an `into` followed by something other than a `select` clause</span></span>
```csharp
from x1 in e1
join x2 in e2 on k1 equals k2 into g
...
```
<span data-ttu-id="08560-2552">je přeloženo do</span><span class="sxs-lookup"><span data-stu-id="08560-2552">is translated into</span></span>
```csharp
from * in ( e1 ) . GroupJoin( e2 , x1 => k1 , x2 => k2 , ( x1 , g ) => new { x1 , g })
...
```

<span data-ttu-id="08560-2553">Výraz dotazu s klauzulí `orderby`</span><span class="sxs-lookup"><span data-stu-id="08560-2553">A query expression with an `orderby` clause</span></span>
```csharp
from x in e
orderby k1 , k2 , ..., kn
...
```
<span data-ttu-id="08560-2554">je přeloženo do</span><span class="sxs-lookup"><span data-stu-id="08560-2554">is translated into</span></span>
```csharp
from x in ( e ) . 
OrderBy ( x => k1 ) . 
ThenBy ( x => k2 ) .
... .
ThenBy ( x => kn )
...
```

<span data-ttu-id="08560-2555">Pokud klauzule řazení určuje `descending` směrového indikátoru, je namísto toho vytvořena volání `OrderByDescending` nebo `ThenByDescending`.</span><span class="sxs-lookup"><span data-stu-id="08560-2555">If an ordering clause specifies a `descending` direction indicator, an invocation of `OrderByDescending` or `ThenByDescending` is produced instead.</span></span>

<span data-ttu-id="08560-2556">Následující překlady předpokládají, že nejsou k dispozici žádné `let`, `where`, `join` ani klauzule `orderby` a ne více než jedna `from` počáteční klauzule v každém výrazu dotazu.</span><span class="sxs-lookup"><span data-stu-id="08560-2556">The following translations assume that there are no `let`, `where`, `join` or `orderby` clauses, and no more than the one initial `from` clause in each query expression.</span></span>

<span data-ttu-id="08560-2557">Příklad</span><span class="sxs-lookup"><span data-stu-id="08560-2557">The example</span></span>
```csharp
from c in customers
from o in c.Orders
select new { c.Name, o.OrderID, o.Total }
```
<span data-ttu-id="08560-2558">je přeloženo do</span><span class="sxs-lookup"><span data-stu-id="08560-2558">is translated into</span></span>
```csharp
customers.
SelectMany(c => c.Orders,
     (c,o) => new { c.Name, o.OrderID, o.Total }
)
```

<span data-ttu-id="08560-2559">Příklad</span><span class="sxs-lookup"><span data-stu-id="08560-2559">The example</span></span>
```csharp
from c in customers
from o in c.Orders
orderby o.Total descending
select new { c.Name, o.OrderID, o.Total }
```
<span data-ttu-id="08560-2560">je přeloženo do</span><span class="sxs-lookup"><span data-stu-id="08560-2560">is translated into</span></span>
```csharp
from * in customers.
    SelectMany(c => c.Orders, (c,o) => new { c, o })
orderby o.Total descending
select new { c.Name, o.OrderID, o.Total }
```
<span data-ttu-id="08560-2561">konečný překlad, který je</span><span class="sxs-lookup"><span data-stu-id="08560-2561">the final translation of which is</span></span>
```csharp
customers.
SelectMany(c => c.Orders, (c,o) => new { c, o }).
OrderByDescending(x => x.o.Total).
Select(x => new { x.c.Name, x.o.OrderID, x.o.Total })
```
<span data-ttu-id="08560-2562">kde `x` je identifikátor generovaný kompilátorem, který je jinak neviditelný a nepřístupný.</span><span class="sxs-lookup"><span data-stu-id="08560-2562">where `x` is a compiler generated identifier that is otherwise invisible and inaccessible.</span></span>

<span data-ttu-id="08560-2563">Příklad</span><span class="sxs-lookup"><span data-stu-id="08560-2563">The example</span></span>
```csharp
from o in orders
let t = o.Details.Sum(d => d.UnitPrice * d.Quantity)
where t >= 1000
select new { o.OrderID, Total = t }
```
<span data-ttu-id="08560-2564">je přeloženo do</span><span class="sxs-lookup"><span data-stu-id="08560-2564">is translated into</span></span>
```csharp
from * in orders.
    Select(o => new { o, t = o.Details.Sum(d => d.UnitPrice * d.Quantity) })
where t >= 1000 
select new { o.OrderID, Total = t }
```
<span data-ttu-id="08560-2565">konečný překlad, který je</span><span class="sxs-lookup"><span data-stu-id="08560-2565">the final translation of which is</span></span>
```csharp
orders.
Select(o => new { o, t = o.Details.Sum(d => d.UnitPrice * d.Quantity) }).
Where(x => x.t >= 1000).
Select(x => new { x.o.OrderID, Total = x.t })
```
<span data-ttu-id="08560-2566">kde `x` je identifikátor generovaný kompilátorem, který je jinak neviditelný a nepřístupný.</span><span class="sxs-lookup"><span data-stu-id="08560-2566">where `x` is a compiler generated identifier that is otherwise invisible and inaccessible.</span></span>

<span data-ttu-id="08560-2567">Příklad</span><span class="sxs-lookup"><span data-stu-id="08560-2567">The example</span></span>
```csharp
from c in customers
join o in orders on c.CustomerID equals o.CustomerID
select new { c.Name, o.OrderDate, o.Total }
```
<span data-ttu-id="08560-2568">je přeloženo do</span><span class="sxs-lookup"><span data-stu-id="08560-2568">is translated into</span></span>
```csharp
customers.Join(orders, c => c.CustomerID, o => o.CustomerID,
    (c, o) => new { c.Name, o.OrderDate, o.Total })
```

<span data-ttu-id="08560-2569">Příklad</span><span class="sxs-lookup"><span data-stu-id="08560-2569">The example</span></span>
```csharp
from c in customers
join o in orders on c.CustomerID equals o.CustomerID into co
let n = co.Count()
where n >= 10
select new { c.Name, OrderCount = n }
```
<span data-ttu-id="08560-2570">je přeloženo do</span><span class="sxs-lookup"><span data-stu-id="08560-2570">is translated into</span></span>
```csharp
from * in customers.
    GroupJoin(orders, c => c.CustomerID, o => o.CustomerID,
        (c, co) => new { c, co })
let n = co.Count()
where n >= 10 
select new { c.Name, OrderCount = n }
```
<span data-ttu-id="08560-2571">konečný překlad, který je</span><span class="sxs-lookup"><span data-stu-id="08560-2571">the final translation of which is</span></span>
```csharp
customers.
GroupJoin(orders, c => c.CustomerID, o => o.CustomerID,
    (c, co) => new { c, co }).
Select(x => new { x, n = x.co.Count() }).
Where(y => y.n >= 10).
Select(y => new { y.x.c.Name, OrderCount = y.n)
```
<span data-ttu-id="08560-2572">kde `x` a `y` jsou identifikátory vygenerované kompilátorem, které jsou jinak neviditelné a nedostupné.</span><span class="sxs-lookup"><span data-stu-id="08560-2572">where `x` and `y` are compiler generated identifiers that are otherwise invisible and inaccessible.</span></span>

<span data-ttu-id="08560-2573">Příklad</span><span class="sxs-lookup"><span data-stu-id="08560-2573">The example</span></span>
```csharp
from o in orders
orderby o.Customer.Name, o.Total descending
select o
```
<span data-ttu-id="08560-2574">má konečný překlad</span><span class="sxs-lookup"><span data-stu-id="08560-2574">has the final translation</span></span>
```csharp
orders.
OrderBy(o => o.Customer.Name).
ThenByDescending(o => o.Total)
```

#### <a name="select-clauses"></a><span data-ttu-id="08560-2575">Vybrat klauzule</span><span class="sxs-lookup"><span data-stu-id="08560-2575">Select clauses</span></span>

<span data-ttu-id="08560-2576">Výraz dotazu formuláře</span><span class="sxs-lookup"><span data-stu-id="08560-2576">A query expression of the form</span></span>
```csharp
from x in e select v
```
<span data-ttu-id="08560-2577">je přeloženo do</span><span class="sxs-lookup"><span data-stu-id="08560-2577">is translated into</span></span>
```csharp
( e ) . Select ( x => v )
```
<span data-ttu-id="08560-2578">s výjimkou případů, kdy v je identifikátor x, je překlad jednoduše</span><span class="sxs-lookup"><span data-stu-id="08560-2578">except when v is the identifier x, the translation is simply</span></span>
```csharp
( e )
```

<span data-ttu-id="08560-2579">Příklad</span><span class="sxs-lookup"><span data-stu-id="08560-2579">For example</span></span>
```csharp
from c in customers.Where(c => c.City == "London")
select c
```
<span data-ttu-id="08560-2580">je jednoduše přeložen na</span><span class="sxs-lookup"><span data-stu-id="08560-2580">is simply translated into</span></span>
```csharp
customers.Where(c => c.City == "London")
```

#### <a name="groupby-clauses"></a><span data-ttu-id="08560-2581">Klauzule GroupBy</span><span class="sxs-lookup"><span data-stu-id="08560-2581">Groupby clauses</span></span>

<span data-ttu-id="08560-2582">Výraz dotazu formuláře</span><span class="sxs-lookup"><span data-stu-id="08560-2582">A query expression of the form</span></span>
```csharp
from x in e group v by k
```
<span data-ttu-id="08560-2583">je přeloženo do</span><span class="sxs-lookup"><span data-stu-id="08560-2583">is translated into</span></span>
```csharp
( e ) . GroupBy ( x => k , x => v )
```
<span data-ttu-id="08560-2584">s výjimkou případů, kdy v je identifikátor x, je překlad</span><span class="sxs-lookup"><span data-stu-id="08560-2584">except when v is the identifier x, the translation is</span></span>
```csharp
( e ) . GroupBy ( x => k )
```

<span data-ttu-id="08560-2585">Příklad</span><span class="sxs-lookup"><span data-stu-id="08560-2585">The example</span></span>
```csharp
from c in customers
group c.Name by c.Country
```
<span data-ttu-id="08560-2586">je přeloženo do</span><span class="sxs-lookup"><span data-stu-id="08560-2586">is translated into</span></span>
```csharp
customers.
GroupBy(c => c.Country, c => c.Name)
```

#### <a name="transparent-identifiers"></a><span data-ttu-id="08560-2587">Transparentní identifikátory</span><span class="sxs-lookup"><span data-stu-id="08560-2587">Transparent identifiers</span></span>

<span data-ttu-id="08560-2588">Některé překlady vloží proměnné rozsahu s ***transparentními identifikátory*** , které jsou označeny `*`.</span><span class="sxs-lookup"><span data-stu-id="08560-2588">Certain translations inject range variables with ***transparent identifiers*** denoted by `*`.</span></span> <span data-ttu-id="08560-2589">Transparentní identifikátory nejsou funkcí jazyka správné verze; Existují pouze jako zprostředkující krok v procesu překladu výrazu dotazu.</span><span class="sxs-lookup"><span data-stu-id="08560-2589">Transparent identifiers are not a proper language feature; they exist only as an intermediate step in the query expression translation process.</span></span>

<span data-ttu-id="08560-2590">Když překlad dotazů vloží transparentní identifikátor, další kroky překladu šíří transparentní identifikátor do anonymních funkcí a inicializátorů anonymních objektů.</span><span class="sxs-lookup"><span data-stu-id="08560-2590">When a query translation injects a transparent identifier, further translation steps propagate the transparent identifier into anonymous functions and anonymous object initializers.</span></span> <span data-ttu-id="08560-2591">V těchto kontextech mají transparentní identifikátory následující chování:</span><span class="sxs-lookup"><span data-stu-id="08560-2591">In those contexts, transparent identifiers have the following behavior:</span></span>

*  <span data-ttu-id="08560-2592">Pokud dojde k transparentnímu identifikátoru jako parametr v anonymní funkci, členové přidruženého anonymního typu jsou automaticky v oboru v těle anonymní funkce.</span><span class="sxs-lookup"><span data-stu-id="08560-2592">When a transparent identifier occurs as a parameter in an anonymous function, the members of the associated anonymous type are automatically in scope in the body of the anonymous function.</span></span>
*  <span data-ttu-id="08560-2593">Pokud je člen s transparentním identifikátorem v oboru, členy tohoto člena jsou také v oboru.</span><span class="sxs-lookup"><span data-stu-id="08560-2593">When a member with a transparent identifier is in scope, the members of that member are in scope as well.</span></span>
*  <span data-ttu-id="08560-2594">Když dojde k transparentnímu identifikátoru jako člen deklarátor v inicializátoru anonymního objektu, zavádí člen s transparentním identifikátorem.</span><span class="sxs-lookup"><span data-stu-id="08560-2594">When a transparent identifier occurs as a member declarator in an anonymous object initializer, it introduces a member with a transparent identifier.</span></span>
*  <span data-ttu-id="08560-2595">V krocích překladu výše jsou transparentní identifikátory vždy představeny spolu s anonymními typy s úmyslem zachytávání více proměnných rozsahu jako členů jednoho objektu.</span><span class="sxs-lookup"><span data-stu-id="08560-2595">In the translation steps described above, transparent identifiers are always introduced together with anonymous types, with the intent of capturing multiple range variables as members of a single object.</span></span> <span data-ttu-id="08560-2596">Implementace C# je oprávněná používat jiný mechanismus než anonymní typy pro seskupení více proměnných rozsahu.</span><span class="sxs-lookup"><span data-stu-id="08560-2596">An implementation of C# is permitted to use a different mechanism than anonymous types to group together multiple range variables.</span></span> <span data-ttu-id="08560-2597">V následujících příkladech překladu se předpokládá, že se používají anonymní typy, a ukazuje, jak se dají překládat transparentní identifikátory.</span><span class="sxs-lookup"><span data-stu-id="08560-2597">The following translation examples assume that anonymous types are used, and show how transparent identifiers can be translated away.</span></span>

<span data-ttu-id="08560-2598">Příklad</span><span class="sxs-lookup"><span data-stu-id="08560-2598">The example</span></span>
```csharp
from c in customers
from o in c.Orders
orderby o.Total descending
select new { c.Name, o.Total }
```
<span data-ttu-id="08560-2599">je přeloženo do</span><span class="sxs-lookup"><span data-stu-id="08560-2599">is translated into</span></span>
```csharp
from * in customers.
    SelectMany(c => c.Orders, (c,o) => new { c, o })
orderby o.Total descending
select new { c.Name, o.Total }
```

<span data-ttu-id="08560-2600">který je dále přeložen do</span><span class="sxs-lookup"><span data-stu-id="08560-2600">which is further translated into</span></span>
```csharp
customers.
SelectMany(c => c.Orders, (c,o) => new { c, o }).
OrderByDescending(* => o.Total).
Select(* => new { c.Name, o.Total })
```
<span data-ttu-id="08560-2601">který, když jsou transparentní identifikátory smazány, je ekvivalentní</span><span class="sxs-lookup"><span data-stu-id="08560-2601">which, when transparent identifiers are erased, is equivalent to</span></span>
```csharp
customers.
SelectMany(c => c.Orders, (c,o) => new { c, o }).
OrderByDescending(x => x.o.Total).
Select(x => new { x.c.Name, x.o.Total })
```
<span data-ttu-id="08560-2602">kde `x` je identifikátor generovaný kompilátorem, který je jinak neviditelný a nepřístupný.</span><span class="sxs-lookup"><span data-stu-id="08560-2602">where `x` is a compiler generated identifier that is otherwise invisible and inaccessible.</span></span>

<span data-ttu-id="08560-2603">Příklad</span><span class="sxs-lookup"><span data-stu-id="08560-2603">The example</span></span>
```csharp
from c in customers
join o in orders on c.CustomerID equals o.CustomerID
join d in details on o.OrderID equals d.OrderID
join p in products on d.ProductID equals p.ProductID
select new { c.Name, o.OrderDate, p.ProductName }
```
<span data-ttu-id="08560-2604">je přeloženo do</span><span class="sxs-lookup"><span data-stu-id="08560-2604">is translated into</span></span>
```csharp
from * in customers.
    Join(orders, c => c.CustomerID, o => o.CustomerID, 
        (c, o) => new { c, o })
join d in details on o.OrderID equals d.OrderID
join p in products on d.ProductID equals p.ProductID
select new { c.Name, o.OrderDate, p.ProductName }
```
<span data-ttu-id="08560-2605">což je dále sníženo na</span><span class="sxs-lookup"><span data-stu-id="08560-2605">which is further reduced to</span></span>
```csharp
customers.
Join(orders, c => c.CustomerID, o => o.CustomerID, (c, o) => new { c, o }).
Join(details, * => o.OrderID, d => d.OrderID, (*, d) => new { *, d }).
Join(products, * => d.ProductID, p => p.ProductID, (*, p) => new { *, p }).
Select(* => new { c.Name, o.OrderDate, p.ProductName })
```
<span data-ttu-id="08560-2606">konečný překlad, který je</span><span class="sxs-lookup"><span data-stu-id="08560-2606">the final translation of which is</span></span>
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
<span data-ttu-id="08560-2607">kde `x`, `y`a `z` jsou identifikátory generované kompilátorem, které jsou jinak neviditelné a nedostupné.</span><span class="sxs-lookup"><span data-stu-id="08560-2607">where `x`, `y`, and `z` are compiler generated identifiers that are otherwise invisible and inaccessible.</span></span>

### <a name="the-query-expression-pattern"></a><span data-ttu-id="08560-2608">Vzor výrazu dotazu</span><span class="sxs-lookup"><span data-stu-id="08560-2608">The query expression pattern</span></span>

<span data-ttu-id="08560-2609">***Vzor výrazu dotazu*** vytváří vzor metod, které mohou typy implementovat pro podporu výrazů dotazů.</span><span class="sxs-lookup"><span data-stu-id="08560-2609">The ***Query expression pattern*** establishes a pattern of methods that types can implement to support query expressions.</span></span> <span data-ttu-id="08560-2610">Vzhledem k tomu, že výrazy dotazů jsou přeloženy do vyvolání metod pomocí syntaktického mapování, typy mají značnou flexibilitu v tom, jak implementují vzor výrazu dotazu.</span><span class="sxs-lookup"><span data-stu-id="08560-2610">Because query expressions are translated to method invocations by means of a syntactic mapping, types have considerable flexibility in how they implement the query expression pattern.</span></span> <span data-ttu-id="08560-2611">Například metody vzoru lze implementovat jako metody instance nebo jako metody rozšíření, protože obě mají stejnou syntaxi vyvolání a metody mohou požadovat delegáty nebo stromy výrazů, protože anonymní funkce jsou převoditelné na obojí.</span><span class="sxs-lookup"><span data-stu-id="08560-2611">For example, the methods of the pattern can be implemented as instance methods or as extension methods because the two have the same invocation syntax, and the methods can request delegates or expression trees because anonymous functions are convertible to both.</span></span>

<span data-ttu-id="08560-2612">Doporučený tvar obecného typu `C<T>`, který podporuje vzor výrazu dotazu, je uveden níže.</span><span class="sxs-lookup"><span data-stu-id="08560-2612">The recommended shape of a generic type `C<T>` that supports the query expression pattern is shown below.</span></span> <span data-ttu-id="08560-2613">Obecný typ se používá pro ilustraci správných vztahů mezi typy parametrů a výsledků, ale je možné implementovat také vzor pro neobecné typy.</span><span class="sxs-lookup"><span data-stu-id="08560-2613">A generic type is used in order to illustrate the proper relationships between parameter and result types, but it is possible to implement the pattern for non-generic types as well.</span></span>

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

<span data-ttu-id="08560-2614">Výše uvedené metody používají obecné typy delegátů `Func<T1,R>` a `Func<T1,T2,R>`, ale mohou být stejně vhodné použít jiné typy delegátů nebo strom výrazů se stejnými relacemi v parametrech a typech výsledků.</span><span class="sxs-lookup"><span data-stu-id="08560-2614">The methods above use the generic delegate types `Func<T1,R>` and `Func<T1,T2,R>`, but they could equally well have used other delegate or expression tree types with the same relationships in parameter and result types.</span></span>

<span data-ttu-id="08560-2615">Všimněte si doporučeného vztahu mezi `C<T>` a `O<T>`, který zajišťuje, že metody `ThenBy` a `ThenByDescending` jsou k dispozici pouze pro výsledek `OrderBy` nebo `OrderByDescending`.</span><span class="sxs-lookup"><span data-stu-id="08560-2615">Notice the recommended relationship between `C<T>` and `O<T>` which ensures that the `ThenBy` and `ThenByDescending` methods are available only on the result of an `OrderBy` or `OrderByDescending`.</span></span> <span data-ttu-id="08560-2616">Všimněte si také doporučeného tvaru výsledku `GroupBy`--sekvence sekvencí, kde má každá vnitřní sekvence další vlastnost `Key`.</span><span class="sxs-lookup"><span data-stu-id="08560-2616">Also notice the recommended shape of the result of `GroupBy` -- a sequence of sequences, where each inner sequence has an additional `Key` property.</span></span>

<span data-ttu-id="08560-2617">Obor názvů `System.Linq` poskytuje implementaci vzoru operátoru dotazu pro jakýkoli typ, který implementuje rozhraní `System.Collections.Generic.IEnumerable<T>`.</span><span class="sxs-lookup"><span data-stu-id="08560-2617">The `System.Linq` namespace provides an implementation of the query operator pattern for any type that implements the `System.Collections.Generic.IEnumerable<T>` interface.</span></span>

## <a name="assignment-operators"></a><span data-ttu-id="08560-2618">Operátory přiřazení</span><span class="sxs-lookup"><span data-stu-id="08560-2618">Assignment operators</span></span>

<span data-ttu-id="08560-2619">Operátory přiřazení přiřadí novou hodnotu proměnné, vlastnosti, události nebo prvku indexeru.</span><span class="sxs-lookup"><span data-stu-id="08560-2619">The assignment operators assign a new value to a variable, a property, an event, or an indexer element.</span></span>

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

<span data-ttu-id="08560-2620">Levý operand přiřazení musí být výraz klasifikovaný jako proměnná, přístup k vlastnosti, přístup indexeru nebo přístup k události.</span><span class="sxs-lookup"><span data-stu-id="08560-2620">The left operand of an assignment must be an expression classified as a variable, a property access, an indexer access, or an event access.</span></span>

<span data-ttu-id="08560-2621">Operátor `=` se nazývá ***jednoduchý operátor přiřazení***.</span><span class="sxs-lookup"><span data-stu-id="08560-2621">The `=` operator is called the ***simple assignment operator***.</span></span> <span data-ttu-id="08560-2622">Přiřadí hodnotu pravého operandu proměnné, vlastnosti nebo prvku indexeru, který je dán levým operandem.</span><span class="sxs-lookup"><span data-stu-id="08560-2622">It assigns the value of the right operand to the variable, property, or indexer element given by the left operand.</span></span> <span data-ttu-id="08560-2623">Levý operand operátoru jednoduchého přiřazení nemůže být přístupem k události (s výjimkou popsaných v [událostech podobných poli](classes.md#field-like-events)).</span><span class="sxs-lookup"><span data-stu-id="08560-2623">The left operand of the simple assignment operator may not be an event access (except as described in [Field-like events](classes.md#field-like-events)).</span></span> <span data-ttu-id="08560-2624">Jednoduchý operátor přiřazení je popsán v tématu [jednoduché přiřazení](expressions.md#simple-assignment).</span><span class="sxs-lookup"><span data-stu-id="08560-2624">The simple assignment operator is described in [Simple assignment](expressions.md#simple-assignment).</span></span>

<span data-ttu-id="08560-2625">Operátory přiřazení jiné než operátor `=` se nazývají ***operátory složeného přiřazení***.</span><span class="sxs-lookup"><span data-stu-id="08560-2625">The assignment operators other than the `=` operator are called the ***compound assignment operators***.</span></span> <span data-ttu-id="08560-2626">Tyto operátory provádějí určenou operaci na dvou operandech a následně přiřadí výslednou hodnotu proměnné, vlastnosti nebo prvku indexeru, který je dán levým operandem.</span><span class="sxs-lookup"><span data-stu-id="08560-2626">These operators perform the indicated operation on the two operands, and then assign the resulting value to the variable, property, or indexer element given by the left operand.</span></span> <span data-ttu-id="08560-2627">Složené operátory přiřazení jsou popsány ve [složeném přiřazení](expressions.md#compound-assignment).</span><span class="sxs-lookup"><span data-stu-id="08560-2627">The compound assignment operators are described in [Compound assignment](expressions.md#compound-assignment).</span></span>

<span data-ttu-id="08560-2628">Operátory `+=` a `-=` s výrazem přístupu k události jako levý operand se nazývají *operátory přiřazení události*.</span><span class="sxs-lookup"><span data-stu-id="08560-2628">The `+=` and `-=` operators with an event access expression as the left operand are called the *event assignment operators*.</span></span> <span data-ttu-id="08560-2629">Žádný jiný operátor přiřazení není platný s přístupem k události jako levý operand.</span><span class="sxs-lookup"><span data-stu-id="08560-2629">No other assignment operator is valid with an event access as the left operand.</span></span> <span data-ttu-id="08560-2630">Operátory přiřazení událostí jsou popsány v tématu [přiřazení události](expressions.md#event-assignment).</span><span class="sxs-lookup"><span data-stu-id="08560-2630">The event assignment operators are described in [Event assignment](expressions.md#event-assignment).</span></span>

<span data-ttu-id="08560-2631">Operátory přiřazení jsou asociativní zprava, což znamená, že operace jsou seskupeny zprava doleva.</span><span class="sxs-lookup"><span data-stu-id="08560-2631">The assignment operators are right-associative, meaning that operations are grouped from right to left.</span></span> <span data-ttu-id="08560-2632">Například výraz `a = b = c` formuláře je vyhodnocen jako `a = (b = c)`.</span><span class="sxs-lookup"><span data-stu-id="08560-2632">For example, an expression of the form `a = b = c` is evaluated as `a = (b = c)`.</span></span>

### <a name="simple-assignment"></a><span data-ttu-id="08560-2633">Jednoduché přiřazení</span><span class="sxs-lookup"><span data-stu-id="08560-2633">Simple assignment</span></span>

<span data-ttu-id="08560-2634">Operátor `=` se nazývá jednoduchý operátor přiřazení.</span><span class="sxs-lookup"><span data-stu-id="08560-2634">The `=` operator is called the simple assignment operator.</span></span>

<span data-ttu-id="08560-2635">Pokud je levý operand jednoduchého přiřazení ve formě `E.P` nebo `E[Ei]` kde `E` má `dynamic`typ doby kompilace, pak je přiřazení dynamicky svázáno ([dynamická vazba](expressions.md#dynamic-binding)).</span><span class="sxs-lookup"><span data-stu-id="08560-2635">If the left operand of a simple assignment is of the form `E.P` or `E[Ei]` where `E` has the compile-time type `dynamic`, then the assignment is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)).</span></span> <span data-ttu-id="08560-2636">V tomto případě je typ doby kompilace výrazu přiřazení `dynamic`a řešení popsané níže bude provedeno za běhu na základě typu modulu runtime `E`.</span><span class="sxs-lookup"><span data-stu-id="08560-2636">In this case the compile-time type of the assignment expression is `dynamic`, and the resolution described below will take place at run-time based on the run-time type of `E`.</span></span>

<span data-ttu-id="08560-2637">V jednoduchém přiřazení musí být pravý operand výraz, který lze implicitně převést na typ levého operandu.</span><span class="sxs-lookup"><span data-stu-id="08560-2637">In a simple assignment, the right operand must be an expression that is implicitly convertible to the type of the left operand.</span></span> <span data-ttu-id="08560-2638">Operace přiřadí hodnotu pravého operandu proměnné, vlastnosti nebo prvku indexeru, který je dán levým operandem.</span><span class="sxs-lookup"><span data-stu-id="08560-2638">The operation assigns the value of the right operand to the variable, property, or indexer element given by the left operand.</span></span>

<span data-ttu-id="08560-2639">Výsledkem jednoduchého výrazu přiřazení je hodnota přiřazená k levému operandu.</span><span class="sxs-lookup"><span data-stu-id="08560-2639">The result of a simple assignment expression is the value assigned to the left operand.</span></span> <span data-ttu-id="08560-2640">Výsledek má stejný typ jako levý operand a je vždycky klasifikován jako hodnota.</span><span class="sxs-lookup"><span data-stu-id="08560-2640">The result has the same type as the left operand and is always classified as a value.</span></span>

<span data-ttu-id="08560-2641">Pokud levý operand je vlastnost nebo přístup indexeru, vlastnost nebo indexer musí mít přistupující objekt `set`.</span><span class="sxs-lookup"><span data-stu-id="08560-2641">If the left operand is a property or indexer access, the property or indexer must have a `set` accessor.</span></span> <span data-ttu-id="08560-2642">Pokud se nejedná o tento případ, dojde k chybě při vazbě.</span><span class="sxs-lookup"><span data-stu-id="08560-2642">If this is not the case, a binding-time error occurs.</span></span>

<span data-ttu-id="08560-2643">Běhové zpracování jednoduchého přiřazení formuláře `x = y` sestává z následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="08560-2643">The run-time processing of a simple assignment of the form `x = y` consists of the following steps:</span></span>

*  <span data-ttu-id="08560-2644">Pokud je `x` klasifikován jako proměnná:</span><span class="sxs-lookup"><span data-stu-id="08560-2644">If `x` is classified as a variable:</span></span>
   * <span data-ttu-id="08560-2645">`x` se vyhodnotí, aby se vytvořila proměnná.</span><span class="sxs-lookup"><span data-stu-id="08560-2645">`x` is evaluated to produce the variable.</span></span>
   * <span data-ttu-id="08560-2646">`y` je vyhodnocena a v případě potřeby převedena na typ `x` prostřednictvím implicitního převodu ([implicitní převody](conversions.md#implicit-conversions)).</span><span class="sxs-lookup"><span data-stu-id="08560-2646">`y` is evaluated and, if required, converted to the type of `x` through an implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)).</span></span>
   * <span data-ttu-id="08560-2647">Pokud proměnná zadaná pomocí `x` je prvek pole *reference_type*, je provedena kontrola za běhu, aby se zajistilo, že hodnota vypočítaná pro `y` je kompatibilní s instancí pole, pro kterou `x` je prvek.</span><span class="sxs-lookup"><span data-stu-id="08560-2647">If the variable given by `x` is an array element of a *reference_type*, a run-time check is performed to ensure that the value computed for `y` is compatible with the array instance of which `x` is an element.</span></span> <span data-ttu-id="08560-2648">Pokud je `y` `null`nebo pokud implicitní převod odkazu ([implicitní převody odkazů](conversions.md#implicit-reference-conversions)) existuje ze skutečného typu instance, na kterou odkazuje `y` na skutečný typ elementu instance pole, která obsahuje `x`, bude ověření úspěšné.</span><span class="sxs-lookup"><span data-stu-id="08560-2648">The check succeeds if `y` is `null`, or if an implicit reference conversion ([Implicit reference conversions](conversions.md#implicit-reference-conversions)) exists from the actual type of the instance referenced by `y` to the actual element type of the array instance containing `x`.</span></span> <span data-ttu-id="08560-2649">V opačném případě je vyvolána `System.ArrayTypeMismatchException`.</span><span class="sxs-lookup"><span data-stu-id="08560-2649">Otherwise, a `System.ArrayTypeMismatchException` is thrown.</span></span>
   * <span data-ttu-id="08560-2650">Hodnota, která je výsledkem vyhodnocení a konverze `y`, je uložena do umístění uvedeného vyhodnocením `x`.</span><span class="sxs-lookup"><span data-stu-id="08560-2650">The value resulting from the evaluation and conversion of `y` is stored into the location given by the evaluation of `x`.</span></span>
*  <span data-ttu-id="08560-2651">Pokud je `x` klasifikován jako vlastnost nebo přístup indexeru:</span><span class="sxs-lookup"><span data-stu-id="08560-2651">If `x` is classified as a property or indexer access:</span></span>
   * <span data-ttu-id="08560-2652">Výraz instance (Pokud `x` není `static`) a seznam argumentů (Pokud je `x` přístup indexeru) přidružený k `x` jsou vyhodnocovány a výsledky se používají v následném volání přístupného objektu `set`.</span><span class="sxs-lookup"><span data-stu-id="08560-2652">The instance expression (if `x` is not `static`) and the argument list (if `x` is an indexer access) associated with `x` are evaluated, and the results are used in the subsequent `set` accessor invocation.</span></span>
   * <span data-ttu-id="08560-2653">`y` je vyhodnocena a v případě potřeby převedena na typ `x` prostřednictvím implicitního převodu ([implicitní převody](conversions.md#implicit-conversions)).</span><span class="sxs-lookup"><span data-stu-id="08560-2653">`y` is evaluated and, if required, converted to the type of `x` through an implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)).</span></span>
   * <span data-ttu-id="08560-2654">Přistupující objekt `set` `x` je vyvolán s hodnotou vypočítanou pro `y` jako jeho `value` argument.</span><span class="sxs-lookup"><span data-stu-id="08560-2654">The `set` accessor of `x` is invoked with the value computed for `y` as its `value` argument.</span></span>

<span data-ttu-id="08560-2655">Pravidla koodchylky pole ([kovariance pole](arrays.md#array-covariance)) povolují hodnotu typu pole, `A[]` být odkazem na instanci typu pole `B[]`za předpokladu, že v `B` na `A`existuje implicitní převod odkazu.</span><span class="sxs-lookup"><span data-stu-id="08560-2655">The array co-variance rules ([Array covariance](arrays.md#array-covariance)) permit a value of an array type `A[]` to be a reference to an instance of an array type `B[]`, provided an implicit reference conversion exists from `B` to `A`.</span></span> <span data-ttu-id="08560-2656">Z důvodu těchto pravidel přiřazení k elementu pole *reference_type* vyžaduje kontrolu za běhu, aby se zajistilo, že přiřazená hodnota je kompatibilní s instancí Array.</span><span class="sxs-lookup"><span data-stu-id="08560-2656">Because of these rules, assignment to an array element of a *reference_type* requires a run-time check to ensure that the value being assigned is compatible with the array instance.</span></span> <span data-ttu-id="08560-2657">V příkladu</span><span class="sxs-lookup"><span data-stu-id="08560-2657">In the example</span></span>
```csharp
string[] sa = new string[10];
object[] oa = sa;

oa[0] = null;               // Ok
oa[1] = "Hello";            // Ok
oa[2] = new ArrayList();    // ArrayTypeMismatchException
```
<span data-ttu-id="08560-2658">Poslední přiřazení způsobí vyvolaní `System.ArrayTypeMismatchException`, protože instance `ArrayList` nemůže být uložena v prvku `string[]`.</span><span class="sxs-lookup"><span data-stu-id="08560-2658">the last assignment causes a `System.ArrayTypeMismatchException` to be thrown because an instance of `ArrayList` cannot be stored in an element of a `string[]`.</span></span>

<span data-ttu-id="08560-2659">Je-li vlastnost nebo indexovací člen deklarovaný v *struct_type* cílem přiřazení, výraz instance přidružený k vlastnosti nebo přístupu indexeru musí být klasifikován jako proměnná.</span><span class="sxs-lookup"><span data-stu-id="08560-2659">When a property or indexer declared in a *struct_type* is the target of an assignment, the instance expression associated with the property or indexer access must be classified as a variable.</span></span> <span data-ttu-id="08560-2660">Pokud je výraz instance klasifikován jako hodnota, dojde k chybě při vazbě.</span><span class="sxs-lookup"><span data-stu-id="08560-2660">If the instance expression is classified as a value, a binding-time error occurs.</span></span> <span data-ttu-id="08560-2661">Vzhledem k tomu, že je [členem](expressions.md#member-access), platí stejné pravidlo i pro pole.</span><span class="sxs-lookup"><span data-stu-id="08560-2661">Because of [Member access](expressions.md#member-access), the same rule also applies to fields.</span></span>

<span data-ttu-id="08560-2662">S ohledem na deklarace:</span><span class="sxs-lookup"><span data-stu-id="08560-2662">Given the declarations:</span></span>
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
<span data-ttu-id="08560-2663">v příkladu</span><span class="sxs-lookup"><span data-stu-id="08560-2663">in the example</span></span>
```csharp
Point p = new Point();
p.X = 100;
p.Y = 100;
Rectangle r = new Rectangle();
r.A = new Point(10, 10);
r.B = p;
```
<span data-ttu-id="08560-2664">přiřazení `p.X`, `p.Y`, `r.A`a `r.B` jsou povolena, protože `p` a `r` jsou proměnné.</span><span class="sxs-lookup"><span data-stu-id="08560-2664">the assignments to `p.X`, `p.Y`, `r.A`, and `r.B` are permitted because `p` and `r` are variables.</span></span> <span data-ttu-id="08560-2665">V tomto příkladu se ale</span><span class="sxs-lookup"><span data-stu-id="08560-2665">However, in the example</span></span>
```csharp
Rectangle r = new Rectangle();
r.A.X = 10;
r.A.Y = 10;
r.B.X = 100;
r.B.Y = 100;
```
<span data-ttu-id="08560-2666">přiřazení jsou neplatná, protože `r.A` a `r.B` nejsou proměnné.</span><span class="sxs-lookup"><span data-stu-id="08560-2666">the assignments are all invalid, since `r.A` and `r.B` are not variables.</span></span>

### <a name="compound-assignment"></a><span data-ttu-id="08560-2667">Složené přiřazení</span><span class="sxs-lookup"><span data-stu-id="08560-2667">Compound assignment</span></span>

<span data-ttu-id="08560-2668">Pokud je levý operand složeného přiřazení ve formě `E.P` nebo `E[Ei]` kde `E` má `dynamic`typ doby kompilace, pak je přiřazení dynamicky svázáno ([dynamická vazba](expressions.md#dynamic-binding)).</span><span class="sxs-lookup"><span data-stu-id="08560-2668">If the left operand of a compound assignment is of the form `E.P` or `E[Ei]` where `E` has the compile-time type `dynamic`, then the assignment is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)).</span></span> <span data-ttu-id="08560-2669">V tomto případě je typ doby kompilace výrazu přiřazení `dynamic`a řešení popsané níže bude provedeno za běhu na základě typu modulu runtime `E`.</span><span class="sxs-lookup"><span data-stu-id="08560-2669">In this case the compile-time type of the assignment expression is `dynamic`, and the resolution described below will take place at run-time based on the run-time type of `E`.</span></span>

<span data-ttu-id="08560-2670">Operace `x op= y` formuláře se zpracovává pomocí řešení přetížení binárního operátoru ([rozlišení přetížení binárního operátoru](expressions.md#binary-operator-overload-resolution)), jako kdyby byla operace zapsána `x op y`.</span><span class="sxs-lookup"><span data-stu-id="08560-2670">An operation of the form `x op= y` is processed by applying binary operator overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) as if the operation was written `x op y`.</span></span> <span data-ttu-id="08560-2671">Stisknutím</span><span class="sxs-lookup"><span data-stu-id="08560-2671">Then,</span></span>

*  <span data-ttu-id="08560-2672">Pokud je návratový typ vybraného operátoru implicitně převeden na typ `x`, operace je vyhodnocena jako `x = x op y`, s tím rozdílem, že `x` je vyhodnocena pouze jednou.</span><span class="sxs-lookup"><span data-stu-id="08560-2672">If the return type of the selected operator is implicitly convertible to the type of `x`, the operation is evaluated as `x = x op y`, except that `x` is evaluated only once.</span></span>
*  <span data-ttu-id="08560-2673">V opačném případě, pokud je vybraný operátor předdefinovaným operátorem, pokud je návratový typ vybraného operátoru explicitně převeden na typ `x`a pokud `y` je implicitně převeden na typ `x` nebo je operátor operátor Shift, je operace vyhodnocena jako `x = (T)(x op y)`, kde `T` je typ `x`, s výjimkou toho, že `x` je vyhodnocena pouze jednou.</span><span class="sxs-lookup"><span data-stu-id="08560-2673">Otherwise, if the selected operator is a predefined operator, if the return type of the selected operator is explicitly convertible to the type of `x`, and if `y` is implicitly convertible to the type of `x` or the operator is a shift operator, then the operation is evaluated as `x = (T)(x op y)`, where `T` is the type of `x`, except that `x` is evaluated only once.</span></span>
*  <span data-ttu-id="08560-2674">V opačném případě je složené přiřazení neplatné a dojde k chybě při vazbě.</span><span class="sxs-lookup"><span data-stu-id="08560-2674">Otherwise, the compound assignment is invalid, and a binding-time error occurs.</span></span>

<span data-ttu-id="08560-2675">Termín "vyhodnocený pouze jednou" znamená, že při vyhodnocování `x op y`jsou výsledky jakýchkoliv výrazů `x` dočasně uloženy a následně znovu použity při provádění přiřazení do `x`.</span><span class="sxs-lookup"><span data-stu-id="08560-2675">The term "evaluated only once" means that in the evaluation of `x op y`, the results of any constituent expressions of `x` are temporarily saved and then reused when performing the assignment to `x`.</span></span> <span data-ttu-id="08560-2676">Například v `A()[B()] += C()`přiřazení, kde `A` je metoda vracející `int[]`a `B` a `C` jsou metody vracející `int`, metody jsou vyvolány pouze jednou, v pořadí `A``B``C`.</span><span class="sxs-lookup"><span data-stu-id="08560-2676">For example, in the assignment `A()[B()] += C()`, where `A` is a method returning `int[]`, and `B` and `C` are methods returning `int`, the methods are invoked only once, in the order `A`, `B`, `C`.</span></span>

<span data-ttu-id="08560-2677">Když je levý operand složeného přiřazení přístup k vlastnosti nebo indexer, vlastnost nebo indexer musí mít přistupující objekt `get` a přístupový objekt `set`.</span><span class="sxs-lookup"><span data-stu-id="08560-2677">When the left operand of a compound assignment is a property access or indexer access, the property or indexer must have both a `get` accessor and a `set` accessor.</span></span> <span data-ttu-id="08560-2678">Pokud se nejedná o tento případ, dojde k chybě při vazbě.</span><span class="sxs-lookup"><span data-stu-id="08560-2678">If this is not the case, a binding-time error occurs.</span></span>

<span data-ttu-id="08560-2679">Druhé pravidlo výše umožňuje vyhodnotit `x op= y` v některých kontextech vyhodnotit jako `x = (T)(x op y)`.</span><span class="sxs-lookup"><span data-stu-id="08560-2679">The second rule above permits `x op= y` to be evaluated as `x = (T)(x op y)` in certain contexts.</span></span> <span data-ttu-id="08560-2680">Toto pravidlo existuje, protože předdefinované operátory lze použít jako složené operátory, pokud je levý operand typu `sbyte`, `byte`, `short`, `ushort`nebo `char`.</span><span class="sxs-lookup"><span data-stu-id="08560-2680">The rule exists such that the predefined operators can be used as compound operators when the left operand is of type `sbyte`, `byte`, `short`, `ushort`, or `char`.</span></span> <span data-ttu-id="08560-2681">I v případě, že oba argumenty mají jeden z těchto typů, předdefinované operátory vytvoří výsledek typu `int`, jak je popsáno v tématu [binární číselná propagace](expressions.md#binary-numeric-promotions).</span><span class="sxs-lookup"><span data-stu-id="08560-2681">Even when both arguments are of one of those types, the predefined operators produce a result of type `int`, as described in [Binary numeric promotions](expressions.md#binary-numeric-promotions).</span></span> <span data-ttu-id="08560-2682">Bez přetypování proto by nebylo možné přiřadit výsledek k levému operandu.</span><span class="sxs-lookup"><span data-stu-id="08560-2682">Thus, without a cast it would not be possible to assign the result to the left operand.</span></span>

<span data-ttu-id="08560-2683">Intuitivní efekt pravidla pro předdefinované operátory je jednoduše tím, že `x op= y` je povoleno, pokud jsou povoleny oba `x op y` a `x = y`.</span><span class="sxs-lookup"><span data-stu-id="08560-2683">The intuitive effect of the rule for predefined operators is simply that `x op= y` is permitted if both of `x op y` and `x = y` are permitted.</span></span> <span data-ttu-id="08560-2684">V příkladu</span><span class="sxs-lookup"><span data-stu-id="08560-2684">In the example</span></span>
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
<span data-ttu-id="08560-2685">intuitivním důvodem pro každou chybu je to, že došlo k chybě odpovídající jednoduchého přiřazení.</span><span class="sxs-lookup"><span data-stu-id="08560-2685">the intuitive reason for each error is that a corresponding simple assignment would also have been an error.</span></span>

<span data-ttu-id="08560-2686">To také znamená, že operace složeného přiřazení podporují přenesené operace.</span><span class="sxs-lookup"><span data-stu-id="08560-2686">This also means that compound assignment operations support lifted operations.</span></span> <span data-ttu-id="08560-2687">V příkladu</span><span class="sxs-lookup"><span data-stu-id="08560-2687">In the example</span></span>
```csharp
int? i = 0;
i += 1;             // Ok
```
<span data-ttu-id="08560-2688">je použit předaný operátor `+(int?,int?)`.</span><span class="sxs-lookup"><span data-stu-id="08560-2688">the lifted operator `+(int?,int?)` is used.</span></span>

### <a name="event-assignment"></a><span data-ttu-id="08560-2689">Přiřazení události</span><span class="sxs-lookup"><span data-stu-id="08560-2689">Event assignment</span></span>

<span data-ttu-id="08560-2690">Pokud je levý operand operátoru `+=` nebo `-=` klasifikován jako přístup k události, je výraz vyhodnocen takto:</span><span class="sxs-lookup"><span data-stu-id="08560-2690">If the left operand of a `+=` or `-=` operator is classified as an event access, then the expression is evaluated as follows:</span></span>

*  <span data-ttu-id="08560-2691">Výraz instance, pokud existuje, je vyhodnocen.</span><span class="sxs-lookup"><span data-stu-id="08560-2691">The instance expression, if any, of the event access is evaluated.</span></span>
*  <span data-ttu-id="08560-2692">Pravý operand operátoru `+=` nebo `-=` je vyhodnocen a v případě potřeby převeden na typ levého operandu pomocí implicitního převodu ([implicitní převody](conversions.md#implicit-conversions)).</span><span class="sxs-lookup"><span data-stu-id="08560-2692">The right operand of the `+=` or `-=` operator is evaluated, and, if required, converted to the type of the left operand through an implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)).</span></span>
*  <span data-ttu-id="08560-2693">Přístup k události události je vyvolán s seznamem argumentů, který se skládá z pravého operandu po vyhodnocení a v případě potřeby konverze.</span><span class="sxs-lookup"><span data-stu-id="08560-2693">An event accessor of the event is invoked, with argument list consisting of the right operand, after evaluation and, if necessary, conversion.</span></span> <span data-ttu-id="08560-2694">Pokud byl operátor `+=`, je vyvolán přistupující objekt `add`; Pokud byl operátor `-=`, je vyvolán přistupující objekt `remove`.</span><span class="sxs-lookup"><span data-stu-id="08560-2694">If the operator was `+=`, the `add` accessor is invoked; if the operator was `-=`, the `remove` accessor is invoked.</span></span>

<span data-ttu-id="08560-2695">Výraz přiřazení události nepřinese hodnotu.</span><span class="sxs-lookup"><span data-stu-id="08560-2695">An event assignment expression does not yield a value.</span></span> <span data-ttu-id="08560-2696">Výraz přiřazení události je tedy platný pouze v kontextu *statement_expression* ([příkazy výrazu](statements.md#expression-statements)).</span><span class="sxs-lookup"><span data-stu-id="08560-2696">Thus, an event assignment expression is valid only in the context of a *statement_expression* ([Expression statements](statements.md#expression-statements)).</span></span>

## <a name="expression"></a><span data-ttu-id="08560-2697">Výraz</span><span class="sxs-lookup"><span data-stu-id="08560-2697">Expression</span></span>

<span data-ttu-id="08560-2698">*Výraz* je buď *non_assignment_expression* , nebo *přiřazení*.</span><span class="sxs-lookup"><span data-stu-id="08560-2698">An *expression* is either a *non_assignment_expression* or an *assignment*.</span></span>

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

## <a name="constant-expressions"></a><span data-ttu-id="08560-2699">Výrazy konstant</span><span class="sxs-lookup"><span data-stu-id="08560-2699">Constant expressions</span></span>

<span data-ttu-id="08560-2700">*Constant_expression* je výraz, který lze plně vyhodnotit v době kompilace.</span><span class="sxs-lookup"><span data-stu-id="08560-2700">A *constant_expression* is an expression that can be fully evaluated at compile-time.</span></span>

```antlr
constant_expression
    : expression
    ;
```

<span data-ttu-id="08560-2701">Konstantní výraz musí být `null` literál nebo hodnota s jedním z následujících typů: `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, `decimal`, `bool`, `object`, `string`nebo jakýkoli typ výčtu.</span><span class="sxs-lookup"><span data-stu-id="08560-2701">A constant expression must be the `null` literal or a value with one of  the following types: `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, `decimal`, `bool`, `object`, `string`, or any enumeration type.</span></span> <span data-ttu-id="08560-2702">V konstantních výrazech jsou povoleny pouze následující konstrukce:</span><span class="sxs-lookup"><span data-stu-id="08560-2702">Only the following constructs are permitted in constant expressions:</span></span>

*  <span data-ttu-id="08560-2703">Literály (včetně literálu `null`).</span><span class="sxs-lookup"><span data-stu-id="08560-2703">Literals (including the `null` literal).</span></span>
*  <span data-ttu-id="08560-2704">Odkazy na `const` členy typů třídy a struktury.</span><span class="sxs-lookup"><span data-stu-id="08560-2704">References to `const` members of class and struct types.</span></span>
*  <span data-ttu-id="08560-2705">Odkazy na členy výčtových typů.</span><span class="sxs-lookup"><span data-stu-id="08560-2705">References to members of enumeration types.</span></span>
*  <span data-ttu-id="08560-2706">Odkazy na parametry `const` nebo místní proměnné</span><span class="sxs-lookup"><span data-stu-id="08560-2706">References to `const` parameters or local variables</span></span>
*  <span data-ttu-id="08560-2707">Dílčí výrazy v závorkách, které jsou samotnými konstantními výrazy.</span><span class="sxs-lookup"><span data-stu-id="08560-2707">Parenthesized sub-expressions, which are themselves constant expressions.</span></span>
*  <span data-ttu-id="08560-2708">Výrazy přetypování, za předpokladu, že cílový typ je jeden z výše uvedených typů.</span><span class="sxs-lookup"><span data-stu-id="08560-2708">Cast expressions, provided the target type is one of the types listed above.</span></span>
*  <span data-ttu-id="08560-2709">výrazy `checked` a `unchecked`</span><span class="sxs-lookup"><span data-stu-id="08560-2709">`checked` and `unchecked` expressions</span></span>
*  <span data-ttu-id="08560-2710">Výrazy výchozích hodnot</span><span class="sxs-lookup"><span data-stu-id="08560-2710">Default value expressions</span></span>
*  <span data-ttu-id="08560-2711">Výrazy nameof</span><span class="sxs-lookup"><span data-stu-id="08560-2711">Nameof expressions</span></span>
*  <span data-ttu-id="08560-2712">Předdefinované `+`, `-`, `!`a `~` unární operátory.</span><span class="sxs-lookup"><span data-stu-id="08560-2712">The predefined `+`, `-`, `!`, and `~` unary operators.</span></span>
*  <span data-ttu-id="08560-2713">Předdefinovaná `+`, `-`, `*`, `/`, `%`, `<<`, `>>`, `&`, `|`, `^`, `&&`, `||`, `==`, `!=`, `<`, `>`, `<=`a `>=` binárních operátorů, za předpokladu, že každý operand je typu, který je uvedený výše.</span><span class="sxs-lookup"><span data-stu-id="08560-2713">The predefined `+`, `-`, `*`, `/`, `%`, `<<`, `>>`, `&`, `|`, `^`, `&&`, `||`, `==`, `!=`, `<`, `>`, `<=`, and `>=` binary operators, provided each operand is of a type listed above.</span></span>
*  <span data-ttu-id="08560-2714">Podmíněný operátor `?:`.</span><span class="sxs-lookup"><span data-stu-id="08560-2714">The `?:` conditional operator.</span></span>

<span data-ttu-id="08560-2715">V konstantních výrazech jsou povoleny následující převody:</span><span class="sxs-lookup"><span data-stu-id="08560-2715">The following conversions are permitted in constant expressions:</span></span>

*  <span data-ttu-id="08560-2716">Převody identity</span><span class="sxs-lookup"><span data-stu-id="08560-2716">Identity conversions</span></span>
*  <span data-ttu-id="08560-2717">Číselné převody</span><span class="sxs-lookup"><span data-stu-id="08560-2717">Numeric conversions</span></span>
*  <span data-ttu-id="08560-2718">Převody výčtu</span><span class="sxs-lookup"><span data-stu-id="08560-2718">Enumeration conversions</span></span>
*  <span data-ttu-id="08560-2719">Převody konstantních výrazů</span><span class="sxs-lookup"><span data-stu-id="08560-2719">Constant expression conversions</span></span>
*  <span data-ttu-id="08560-2720">Implicitní a explicitní převody odkazů za předpokladu, že zdrojem převodů je konstantní výraz, který se vyhodnocuje na hodnotu null.</span><span class="sxs-lookup"><span data-stu-id="08560-2720">Implicit and explicit reference conversions, provided that the source of the conversions is a constant expression that evaluates to the null value.</span></span>

<span data-ttu-id="08560-2721">Další převody, včetně zabalení, rozbalení a implicitních převodů odkazů na hodnoty, které nejsou null, nejsou povoleny v konstantních výrazech.</span><span class="sxs-lookup"><span data-stu-id="08560-2721">Other conversions including boxing, unboxing and implicit reference conversions of non-null values are not permitted in constant expressions.</span></span> <span data-ttu-id="08560-2722">Příklad:</span><span class="sxs-lookup"><span data-stu-id="08560-2722">For example:</span></span>
```csharp
class C {
    const object i = 5;         // error: boxing conversion not permitted
    const object str = "hello"; // error: implicit reference conversion
}
```
<span data-ttu-id="08560-2723">Inicializace i je chyba, protože je vyžadován převod zabalení.</span><span class="sxs-lookup"><span data-stu-id="08560-2723">the initialization of i is an error because a boxing conversion is required.</span></span> <span data-ttu-id="08560-2724">Inicializace str je chyba, protože je vyžadován implicitní převod odkazu z hodnoty, která není null.</span><span class="sxs-lookup"><span data-stu-id="08560-2724">The initialization of str is an error because an implicit reference conversion from a non-null value is required.</span></span>

<span data-ttu-id="08560-2725">Pokaždé, když výraz splní požadavky uvedené výše, je výraz vyhodnocen při kompilaci.</span><span class="sxs-lookup"><span data-stu-id="08560-2725">Whenever an expression fulfills the requirements listed above, the expression is evaluated at compile-time.</span></span> <span data-ttu-id="08560-2726">To platí i v případě, že výraz je dílčí výraz většího výrazu, který obsahuje nekonstantní konstrukce.</span><span class="sxs-lookup"><span data-stu-id="08560-2726">This is true even if the expression is a sub-expression of a larger expression that contains non-constant constructs.</span></span>

<span data-ttu-id="08560-2727">Vyhodnocení konstantních výrazů v době kompilace používá stejná pravidla jako vyhodnocení za běhu nekonstantních výrazů, s výjimkou toho, že při vyhodnocení za běhu by došlo k chybě při kompilaci.</span><span class="sxs-lookup"><span data-stu-id="08560-2727">The compile-time evaluation of constant expressions uses the same rules as run-time evaluation of non-constant expressions, except that where run-time evaluation would have thrown an exception, compile-time evaluation causes a compile-time error to occur.</span></span>

<span data-ttu-id="08560-2728">Pokud není konstantní výraz explicitně umístěn v kontextu `unchecked`, přetečení, ke kterým dochází v aritmetických operacích typu integrálního typu a převody během vyhodnocení výrazu, vždy způsobují chyby při kompilaci ([konstantní výrazy](expressions.md#constant-expressions)).</span><span class="sxs-lookup"><span data-stu-id="08560-2728">Unless a constant expression is explicitly placed in an `unchecked` context, overflows that occur in integral-type arithmetic operations and conversions during the compile-time evaluation of the expression always cause compile-time errors ([Constant expressions](expressions.md#constant-expressions)).</span></span>

<span data-ttu-id="08560-2729">Konstantní výrazy se vyskytují v kontextech uvedených níže.</span><span class="sxs-lookup"><span data-stu-id="08560-2729">Constant expressions occur in the contexts listed below.</span></span> <span data-ttu-id="08560-2730">V těchto kontextech dojde k chybě při kompilaci, pokud výraz nemůže být plně vyhodnocen v době kompilace.</span><span class="sxs-lookup"><span data-stu-id="08560-2730">In these contexts, a compile-time error occurs if an expression cannot be fully evaluated at compile-time.</span></span>

*  <span data-ttu-id="08560-2731">Konstantní deklarace ([konstanty](classes.md#constants)).</span><span class="sxs-lookup"><span data-stu-id="08560-2731">Constant declarations ([Constants](classes.md#constants)).</span></span>
*  <span data-ttu-id="08560-2732">Deklarace členů výčtu ([členy výčtu](enums.md#enum-members)).</span><span class="sxs-lookup"><span data-stu-id="08560-2732">Enumeration member declarations ([Enum members](enums.md#enum-members)).</span></span>
*  <span data-ttu-id="08560-2733">Výchozí argumenty formálních seznamů parametrů ([parametry metody](classes.md#method-parameters))</span><span class="sxs-lookup"><span data-stu-id="08560-2733">Default arguments of formal parameter lists ([Method parameters](classes.md#method-parameters))</span></span>
*  <span data-ttu-id="08560-2734">`case` popisky příkazu `switch` ([příkaz switch](statements.md#the-switch-statement)).</span><span class="sxs-lookup"><span data-stu-id="08560-2734">`case` labels of a `switch` statement ([The switch statement](statements.md#the-switch-statement)).</span></span>
*  <span data-ttu-id="08560-2735">příkazy `goto case` ([příkaz goto](statements.md#the-goto-statement)).</span><span class="sxs-lookup"><span data-stu-id="08560-2735">`goto case` statements ([The goto statement](statements.md#the-goto-statement)).</span></span>
*  <span data-ttu-id="08560-2736">Délky dimenzí ve výrazu vytváření pole ([výrazy vytváření pole](expressions.md#array-creation-expressions)), které obsahují inicializátor.</span><span class="sxs-lookup"><span data-stu-id="08560-2736">Dimension lengths in an array creation expression ([Array creation expressions](expressions.md#array-creation-expressions)) that includes an initializer.</span></span>
*  <span data-ttu-id="08560-2737">Atributy ([atributy](attributes.md)).</span><span class="sxs-lookup"><span data-stu-id="08560-2737">Attributes ([Attributes](attributes.md)).</span></span>

<span data-ttu-id="08560-2738">Implicitní převod konstantních výrazů ([implicitní převody konstantních výrazů](conversions.md#implicit-constant-expression-conversions)) umožňuje převést konstantní výraz typu `int` na `sbyte`, `byte`, `short`, `ushort`, `uint`nebo `ulong`, pokud je hodnota konstantního výrazu v rozsahu cílového typu.</span><span class="sxs-lookup"><span data-stu-id="08560-2738">An implicit constant expression conversion ([Implicit constant expression conversions](conversions.md#implicit-constant-expression-conversions)) permits a constant expression of type `int` to be converted to `sbyte`, `byte`, `short`, `ushort`, `uint`, or `ulong`, provided the value of the constant expression is within the range of the destination type.</span></span>

## <a name="boolean-expressions"></a><span data-ttu-id="08560-2739">logické výrazy</span><span class="sxs-lookup"><span data-stu-id="08560-2739">Boolean expressions</span></span>

<span data-ttu-id="08560-2740">*Boolean_expression* je výraz, který vrací výsledek typu `bool`; buď přímo, nebo prostřednictvím aplikace `operator true` v určitých kontextech, jak je uvedeno v následujícím seznamu.</span><span class="sxs-lookup"><span data-stu-id="08560-2740">A *boolean_expression* is an expression that yields a result of type `bool`; either directly or through application of `operator true` in certain contexts as specified in the following.</span></span>

```antlr
boolean_expression
    : expression
    ;
```

<span data-ttu-id="08560-2741">Řízení podmíněného výrazu *if_statement* ([příkaz if](statements.md#the-if-statement)), *while_statement* ([příkaz While](statements.md#the-while-statement)), *do_statement* ([příkaz](statements.md#the-do-statement)do), nebo *for_statement* ([příkaz for](statements.md#the-for-statement)), je *Boolean_expression*.</span><span class="sxs-lookup"><span data-stu-id="08560-2741">The controlling conditional expression of an *if_statement* ([The if statement](statements.md#the-if-statement)), *while_statement* ([The while statement](statements.md#the-while-statement)), *do_statement* ([The do statement](statements.md#the-do-statement)), or *for_statement* ([The for statement](statements.md#the-for-statement)) is a *boolean_expression*.</span></span> <span data-ttu-id="08560-2742">Řízení podmíněného výrazu operátoru `?:` ([podmíněný operátor](expressions.md#conditional-operator)) se řídí stejnými pravidly jako *Boolean_expression*, ale z důvodů priority operátora jsou klasifikovány jako *conditional_or_expression*.</span><span class="sxs-lookup"><span data-stu-id="08560-2742">The controlling conditional expression of the `?:` operator ([Conditional operator](expressions.md#conditional-operator)) follows the same rules as a *boolean_expression*, but for reasons of operator precedence is classified as a *conditional_or_expression*.</span></span>

<span data-ttu-id="08560-2743">*Boolean_expression* `E` je nutné, aby bylo možné vytvořit hodnotu typu `bool`následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="08560-2743">A *boolean_expression* `E` is required to be able to produce a value of type `bool`, as follows:</span></span>

*  <span data-ttu-id="08560-2744">Pokud je `E` implicitně převést na `bool` potom za běhu, který používá implicitní převod.</span><span class="sxs-lookup"><span data-stu-id="08560-2744">If `E` is implicitly convertible to `bool` then at runtime that implicit conversion is applied.</span></span>
*  <span data-ttu-id="08560-2745">V opačném případě se k nalezení jedinečné nejlepší implementace operátoru `true` na `E`používá rozlišení přetížení unárního operátoru ([řešení přetížení unárního operátoru](expressions.md#unary-operator-overload-resolution)) a tato implementace se aplikuje za běhu.</span><span class="sxs-lookup"><span data-stu-id="08560-2745">Otherwise, unary operator overload resolution ([Unary operator overload resolution](expressions.md#unary-operator-overload-resolution)) is used to find a unique best implementation of operator `true` on `E`, and that implementation is applied at runtime.</span></span>
*  <span data-ttu-id="08560-2746">Pokud žádný takový operátor nenajde, dojde k chybě při vazbě.</span><span class="sxs-lookup"><span data-stu-id="08560-2746">If no such operator is found, a binding-time error occurs.</span></span>

<span data-ttu-id="08560-2747">Typ struktury `DBBool` v typu [databáze logická hodnota](structs.md#database-boolean-type) poskytuje příklad typu, který implementuje `operator true` a `operator false`.</span><span class="sxs-lookup"><span data-stu-id="08560-2747">The `DBBool` struct type in [Database boolean type](structs.md#database-boolean-type) provides an example of a type that implements `operator true` and `operator false`.</span></span>
