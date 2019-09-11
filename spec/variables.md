---
ms.openlocfilehash: a01cf9387b8dc47de036bf0bd1496c19a441d81c
ms.sourcegitcommit: 7f7fc6e9e195e51b7ff8229aeaa70aa9fbbb63cb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/10/2019
ms.locfileid: "70876794"
---
# <a name="variables"></a><span data-ttu-id="1a442-101">Proměnné</span><span class="sxs-lookup"><span data-stu-id="1a442-101">Variables</span></span>

<span data-ttu-id="1a442-102">Proměnné reprezentují umístění úložiště.</span><span class="sxs-lookup"><span data-stu-id="1a442-102">Variables represent storage locations.</span></span> <span data-ttu-id="1a442-103">Každá proměnná má typ, který určuje, jaké hodnoty mohou být uloženy v proměnné.</span><span class="sxs-lookup"><span data-stu-id="1a442-103">Every variable has a type that determines what values can be stored in the variable.</span></span> <span data-ttu-id="1a442-104">C#je typově bezpečný jazyk a C# kompilátor garantuje, že hodnoty uložené v proměnných jsou vždy příslušného typu.</span><span class="sxs-lookup"><span data-stu-id="1a442-104">C# is a type-safe language, and the C# compiler guarantees that values stored in variables are always of the appropriate type.</span></span> <span data-ttu-id="1a442-105">Hodnotu proměnné lze změnit pomocí přiřazení nebo `++` pomocí operátorů a. `--`</span><span class="sxs-lookup"><span data-stu-id="1a442-105">The value of a variable can be changed through assignment or through use of the `++` and `--` operators.</span></span>

<span data-ttu-id="1a442-106">Aby bylo možné získat jeho hodnotu, musí být proměnná ***jednoznačně přiřazená*** ([jednoznačné přiřazení](variables.md#definite-assignment)).</span><span class="sxs-lookup"><span data-stu-id="1a442-106">A variable must be ***definitely assigned*** ([Definite assignment](variables.md#definite-assignment)) before its value can be obtained.</span></span>

<span data-ttu-id="1a442-107">Jak je popsáno v následujících oddílech, jsou proměnné buď ***zpočátku přiřazené*** , nebo ***zpočátku nepřiřazené***.</span><span class="sxs-lookup"><span data-stu-id="1a442-107">As described in the following sections, variables are either ***initially assigned*** or ***initially unassigned***.</span></span> <span data-ttu-id="1a442-108">Původně přiřazená proměnná má jasně definovanou počáteční hodnotu a je vždy považována za jednoznačně přiřazenou.</span><span class="sxs-lookup"><span data-stu-id="1a442-108">An initially assigned variable has a well-defined initial value and is always considered definitely assigned.</span></span> <span data-ttu-id="1a442-109">Počáteční Nepřiřazená proměnná nemá počáteční hodnotu.</span><span class="sxs-lookup"><span data-stu-id="1a442-109">An initially unassigned variable has no initial value.</span></span> <span data-ttu-id="1a442-110">Pro původně nepřiřazenou proměnnou, která má být považována za jednoznačně přiřazenou v určitém umístění, musí být přiřazení proměnné provedeno v každé možné cestě provádění, která vede k tomuto umístění.</span><span class="sxs-lookup"><span data-stu-id="1a442-110">For an initially unassigned variable to be considered definitely assigned at a certain location, an assignment to the variable must occur in every possible execution path leading to that location.</span></span>

## <a name="variable-categories"></a><span data-ttu-id="1a442-111">Kategorie proměnných</span><span class="sxs-lookup"><span data-stu-id="1a442-111">Variable categories</span></span>

<span data-ttu-id="1a442-112">C#definuje sedm kategorií proměnných: statické proměnné, proměnné instancí, prvky pole, parametry hodnot, referenční parametry, výstupní parametry a místní proměnné.</span><span class="sxs-lookup"><span data-stu-id="1a442-112">C# defines seven categories of variables: static variables, instance variables, array elements, value parameters, reference parameters, output parameters, and local variables.</span></span> <span data-ttu-id="1a442-113">Níže uvedené části popisují každou z těchto kategorií.</span><span class="sxs-lookup"><span data-stu-id="1a442-113">The sections that follow describe each of these categories.</span></span>

<span data-ttu-id="1a442-114">V příkladu</span><span class="sxs-lookup"><span data-stu-id="1a442-114">In the example</span></span>
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
<span data-ttu-id="1a442-115">`x`je statická proměnná, `y` je proměnná instance, `v[0]` je elementem pole, `a` je parametrem hodnoty, `b` je parametrem odkazu, `c` je výstupní parametr a `i` je místní proměnná. .</span><span class="sxs-lookup"><span data-stu-id="1a442-115">`x` is a static variable, `y` is an instance variable, `v[0]` is an array element, `a` is a value parameter, `b` is a reference parameter, `c` is an output parameter, and `i` is a local variable.</span></span>

### <a name="static-variables"></a><span data-ttu-id="1a442-116">Statické proměnné</span><span class="sxs-lookup"><span data-stu-id="1a442-116">Static variables</span></span>

<span data-ttu-id="1a442-117">Pole deklarované s `static` modifikátorem se nazývá ***statická proměnná***.</span><span class="sxs-lookup"><span data-stu-id="1a442-117">A field declared with the `static` modifier is called a ***static variable***.</span></span> <span data-ttu-id="1a442-118">Statická proměnná se vyskytuje před provedením statického konstruktoru ([statických konstruktorů](classes.md#static-constructors)) pro svůj nadřazený typ a přestane existovat, pokud přidružená aplikační doména přestane existovat.</span><span class="sxs-lookup"><span data-stu-id="1a442-118">A static variable comes into existence before execution of the static constructor ([Static constructors](classes.md#static-constructors)) for its containing type, and ceases to exist when the associated application domain ceases to exist.</span></span>

<span data-ttu-id="1a442-119">Počáteční hodnota statické proměnné je výchozí hodnota ([výchozí hodnoty](variables.md#default-values)) typu proměnné.</span><span class="sxs-lookup"><span data-stu-id="1a442-119">The initial value of a static variable is the default value ([Default values](variables.md#default-values)) of the variable's type.</span></span>

<span data-ttu-id="1a442-120">Pro účely jednoznačné kontroly přiřazení je statická proměnná považována za původně přiřazenou.</span><span class="sxs-lookup"><span data-stu-id="1a442-120">For purposes of definite assignment checking, a static variable is considered initially assigned.</span></span>

### <a name="instance-variables"></a><span data-ttu-id="1a442-121">Proměnné instance</span><span class="sxs-lookup"><span data-stu-id="1a442-121">Instance variables</span></span>

<span data-ttu-id="1a442-122">Pole deklarované bez `static` modifikátoru se nazývá ***instance proměnné***.</span><span class="sxs-lookup"><span data-stu-id="1a442-122">A field declared without the `static` modifier is called an ***instance variable***.</span></span>

#### <a name="instance-variables-in-classes"></a><span data-ttu-id="1a442-123">Proměnné instancí ve třídách</span><span class="sxs-lookup"><span data-stu-id="1a442-123">Instance variables in classes</span></span>

<span data-ttu-id="1a442-124">Instance proměnné třídy se vyskytuje, když je vytvořena nová instance této třídy, a přestane existovat, pokud neexistují žádné odkazy na tuto instanci a destruktor instance (pokud existuje).</span><span class="sxs-lookup"><span data-stu-id="1a442-124">An instance variable of a class comes into existence when a new instance of that class is created, and ceases to exist when there are no references to that instance and the instance's destructor (if any) has executed.</span></span>

<span data-ttu-id="1a442-125">Počáteční hodnota proměnné instance třídy je výchozí hodnota ([výchozí hodnoty](variables.md#default-values)) typu proměnné.</span><span class="sxs-lookup"><span data-stu-id="1a442-125">The initial value of an instance variable of a class is the default value ([Default values](variables.md#default-values)) of the variable's type.</span></span>

<span data-ttu-id="1a442-126">Pro účely jednoznačné kontroly přiřazení je proměnná instance třídy považována za původně přiřazenou.</span><span class="sxs-lookup"><span data-stu-id="1a442-126">For the purpose of definite assignment checking, an instance variable of a class is considered initially assigned.</span></span>

#### <a name="instance-variables-in-structs"></a><span data-ttu-id="1a442-127">Proměnné instancí ve strukturách</span><span class="sxs-lookup"><span data-stu-id="1a442-127">Instance variables in structs</span></span>

<span data-ttu-id="1a442-128">Proměnná instance struktury má přesně stejnou životnost jako proměnná struktury, ke které patří.</span><span class="sxs-lookup"><span data-stu-id="1a442-128">An instance variable of a struct has exactly the same lifetime as the struct variable to which it belongs.</span></span> <span data-ttu-id="1a442-129">Jinými slovy, pokud je proměnná typu struktury v výskytu nebo přestane existovat, tak je také nutné proměnné instance struktury.</span><span class="sxs-lookup"><span data-stu-id="1a442-129">In other words, when a variable of a struct type comes into existence or ceases to exist, so too do the instance variables of the struct.</span></span>

<span data-ttu-id="1a442-130">Počáteční stav přiřazení proměnné instance struktury je stejný jako objekt obsahující proměnnou struktury.</span><span class="sxs-lookup"><span data-stu-id="1a442-130">The initial assignment state of an instance variable of a struct is the same as that of the containing struct variable.</span></span> <span data-ttu-id="1a442-131">Jinými slovy, pokud je proměnná struktury považována za původně přiřazenou, tedy příliš jsou její proměnné instance a pokud je proměnná struktury považována za původně nepřiřazenou, její proměnné instance jsou stejně nepřiřazeny.</span><span class="sxs-lookup"><span data-stu-id="1a442-131">In other words, when a struct variable is considered initially assigned, so too are its instance variables, and when a struct variable is considered initially unassigned, its instance variables are likewise unassigned.</span></span>

### <a name="array-elements"></a><span data-ttu-id="1a442-132">Prvky pole</span><span class="sxs-lookup"><span data-stu-id="1a442-132">Array elements</span></span>

<span data-ttu-id="1a442-133">Prvky pole přicházejí v případě, že je vytvořena instance pole, a přestanou existovat, pokud žádné odkazy na tuto instanci pole neexistují.</span><span class="sxs-lookup"><span data-stu-id="1a442-133">The elements of an array come into existence when an array instance is created, and cease to exist when there are no references to that array instance.</span></span>

<span data-ttu-id="1a442-134">Počáteční hodnota každého prvku pole je výchozí hodnota ([výchozí hodnoty](variables.md#default-values)) typu prvků pole.</span><span class="sxs-lookup"><span data-stu-id="1a442-134">The initial value of each of the elements of an array is the default value ([Default values](variables.md#default-values)) of the type of the array elements.</span></span>

<span data-ttu-id="1a442-135">Pro účely jednoznačné kontroly přiřazení je prvek pole považován za původně přiřazený.</span><span class="sxs-lookup"><span data-stu-id="1a442-135">For the purpose of definite assignment checking, an array element is considered initially assigned.</span></span>

### <a name="value-parameters"></a><span data-ttu-id="1a442-136">Parametry hodnoty</span><span class="sxs-lookup"><span data-stu-id="1a442-136">Value parameters</span></span>

<span data-ttu-id="1a442-137">Parametr deklarovaný bez `ref` modifikátoru nebo `out` je ***parametrem hodnoty***.</span><span class="sxs-lookup"><span data-stu-id="1a442-137">A parameter declared without a `ref` or `out` modifier is a ***value parameter***.</span></span>

<span data-ttu-id="1a442-138">Parametr hodnoty se vyskytuje v případě volání členu funkce (metoda, konstruktor instance, přistupující objekt nebo operátor) nebo anonymní funkce, do které parametr patří, a je inicializován s hodnotou argumentu uvedeného v vyvolání.</span><span class="sxs-lookup"><span data-stu-id="1a442-138">A value parameter comes into existence upon invocation of the function member (method, instance constructor, accessor, or operator) or anonymous function to which the parameter belongs, and is initialized with the value of the argument given in the invocation.</span></span> <span data-ttu-id="1a442-139">Parametr hodnoty obvykle po vrácení členu funkce nebo anonymní funkce přestane existovat.</span><span class="sxs-lookup"><span data-stu-id="1a442-139">A value parameter normally ceases to exist upon return of the function member or anonymous function.</span></span> <span data-ttu-id="1a442-140">Pokud je však parametr value zachycen anonymní funkcí ([anonymními výrazy funkce](expressions.md#anonymous-function-expressions)), bude jeho životní cyklus prodloužen alespoň do doby, než je vytvořen delegát nebo strom výrazů vytvořený z této anonymní funkce, který je vhodný pro uvolňování paměti.</span><span class="sxs-lookup"><span data-stu-id="1a442-140">However, if the value parameter is captured by an anonymous function ([Anonymous function expressions](expressions.md#anonymous-function-expressions)), its life time extends at least until the delegate or expression tree created from that anonymous function is eligible for garbage collection.</span></span>

<span data-ttu-id="1a442-141">Pro účely jednoznačné kontroly přiřazení je parametr hodnoty považován za původně přiřazený.</span><span class="sxs-lookup"><span data-stu-id="1a442-141">For the purpose of definite assignment checking, a value parameter is considered initially assigned.</span></span>

### <a name="reference-parameters"></a><span data-ttu-id="1a442-142">Parametry odkazu</span><span class="sxs-lookup"><span data-stu-id="1a442-142">Reference parameters</span></span>

<span data-ttu-id="1a442-143">Parametr deklarovaný s `ref` modifikátorem je ***parametr odkazu***.</span><span class="sxs-lookup"><span data-stu-id="1a442-143">A parameter declared with a `ref` modifier is a ***reference parameter***.</span></span>

<span data-ttu-id="1a442-144">Parametr reference nevytváří nové umístění úložiště.</span><span class="sxs-lookup"><span data-stu-id="1a442-144">A reference parameter does not create a new storage location.</span></span> <span data-ttu-id="1a442-145">Místo toho parametr reference představuje stejné umístění úložiště jako proměnná zadaná jako argument členu funkce nebo anonymní volání funkce.</span><span class="sxs-lookup"><span data-stu-id="1a442-145">Instead, a reference parameter represents the same storage location as the variable given as the argument in the function member or anonymous function invocation.</span></span> <span data-ttu-id="1a442-146">Proto hodnota referenčního parametru je vždy stejná jako podkladová proměnná.</span><span class="sxs-lookup"><span data-stu-id="1a442-146">Thus, the value of a reference parameter is always the same as the underlying variable.</span></span>

<span data-ttu-id="1a442-147">Následující pravidla přiřazení platí pro referenční parametry.</span><span class="sxs-lookup"><span data-stu-id="1a442-147">The following definite assignment rules apply to reference parameters.</span></span> <span data-ttu-id="1a442-148">Všimněte si různých pravidel pro výstupní parametry popsaných v tématu [výstupní parametry](variables.md#output-parameters).</span><span class="sxs-lookup"><span data-stu-id="1a442-148">Note the different rules for output parameters described in [Output parameters](variables.md#output-parameters).</span></span>

*  <span data-ttu-id="1a442-149">Proměnná musí být jednoznačně přiřazena ([jednoznačné přiřazení](variables.md#definite-assignment)) předtím, než může být předána jako parametr reference v členu funkce nebo volání delegáta.</span><span class="sxs-lookup"><span data-stu-id="1a442-149">A variable must be definitely assigned ([Definite assignment](variables.md#definite-assignment)) before it can be passed as a reference parameter in a function member or delegate invocation.</span></span>
*  <span data-ttu-id="1a442-150">V rámci členu funkce nebo anonymní funkce je referenční parametr považován za původně přiřazený.</span><span class="sxs-lookup"><span data-stu-id="1a442-150">Within a function member or anonymous function, a reference parameter is considered initially assigned.</span></span>

<span data-ttu-id="1a442-151">V rámci metody instance nebo přístupového objektu instance typu `this` struktury se klíčové slovo chová přesně jako referenční parametr typu struktury ([Tento přístup](expressions.md#this-access)).</span><span class="sxs-lookup"><span data-stu-id="1a442-151">Within an instance method or instance accessor of a struct type, the `this` keyword behaves exactly as a reference parameter of the struct type ([This access](expressions.md#this-access)).</span></span>

### <a name="output-parameters"></a><span data-ttu-id="1a442-152">Výstupní parametry</span><span class="sxs-lookup"><span data-stu-id="1a442-152">Output parameters</span></span>

<span data-ttu-id="1a442-153">Parametr deklarovaný s `out` modifikátorem je ***výstupní parametr***.</span><span class="sxs-lookup"><span data-stu-id="1a442-153">A parameter declared with an `out` modifier is an ***output parameter***.</span></span>

<span data-ttu-id="1a442-154">Výstupní parametr nevytváří nové umístění úložiště.</span><span class="sxs-lookup"><span data-stu-id="1a442-154">An output parameter does not create a new storage location.</span></span> <span data-ttu-id="1a442-155">Namísto toho parametr Output představuje stejné umístění úložiště jako proměnná zadaná jako argument členu funkce nebo volání delegáta.</span><span class="sxs-lookup"><span data-stu-id="1a442-155">Instead, an output parameter represents the same storage location as the variable given as the argument in the function member or delegate invocation.</span></span> <span data-ttu-id="1a442-156">Proto hodnota výstupní parametr je vždy stejná jako podkladová proměnná.</span><span class="sxs-lookup"><span data-stu-id="1a442-156">Thus, the value of an output parameter is always the same as the underlying variable.</span></span>

<span data-ttu-id="1a442-157">Následující pravidla přiřazení se vztahují na výstupní parametry.</span><span class="sxs-lookup"><span data-stu-id="1a442-157">The following definite assignment rules apply to output parameters.</span></span> <span data-ttu-id="1a442-158">Poznamenejte si různá pravidla pro referenční parametry popsané v [parametrech reference](variables.md#reference-parameters).</span><span class="sxs-lookup"><span data-stu-id="1a442-158">Note the different rules for reference parameters described in [Reference parameters](variables.md#reference-parameters).</span></span>

*  <span data-ttu-id="1a442-159">Proměnná nemusí být jednoznačně přiřazena, než může být předána jako výstupní parametr v členu funkce nebo volání delegáta.</span><span class="sxs-lookup"><span data-stu-id="1a442-159">A variable need not be definitely assigned before it can be passed as an output parameter in a function member or delegate invocation.</span></span>
*  <span data-ttu-id="1a442-160">Po normálním dokončení členu funkce nebo delegáta delegáta se v této cestě provádění považuje každá proměnná, která byla předána jako výstupní parametr.</span><span class="sxs-lookup"><span data-stu-id="1a442-160">Following the normal completion of a function member or delegate invocation, each variable that was passed as an output parameter is considered assigned in that execution path.</span></span>
*  <span data-ttu-id="1a442-161">V rámci členu funkce nebo anonymní funkce je výstupní parametr považován za původně nepřiřazený.</span><span class="sxs-lookup"><span data-stu-id="1a442-161">Within a function member or anonymous function, an output parameter is considered initially unassigned.</span></span>
*  <span data-ttu-id="1a442-162">Každému výstupnímu parametru členu funkce nebo anonymní funkce se musí jednoznačně přiřadit ([jednoznačné přiřazení](variables.md#definite-assignment)) před tím, než se člen funkce nebo anonymní funkce vrátí běžným způsobem.</span><span class="sxs-lookup"><span data-stu-id="1a442-162">Every output parameter of a function member or anonymous function must be definitely assigned ([Definite assignment](variables.md#definite-assignment)) before the function member or anonymous function returns normally.</span></span>

<span data-ttu-id="1a442-163">V rámci konstruktoru instance typu `this` struktury se klíčové slovo chová přesně jako výstupní parametr typu struktury ([Tento přístup](expressions.md#this-access)).</span><span class="sxs-lookup"><span data-stu-id="1a442-163">Within an instance constructor of a struct type, the `this` keyword behaves exactly as an output parameter of the struct type ([This access](expressions.md#this-access)).</span></span>

### <a name="local-variables"></a><span data-ttu-id="1a442-164">Místní proměnné</span><span class="sxs-lookup"><span data-stu-id="1a442-164">Local variables</span></span>

<span data-ttu-id="1a442-165">***Lokální proměnná*** je deklarována pomocí *local_variable_declaration*, ke kterému může dojít v *bloku*, *for_statement*, *switch_statement* nebo *using_statement*; nebo *foreach_statement* nebo *specific_catch_clause* pro *try_statement*.</span><span class="sxs-lookup"><span data-stu-id="1a442-165">A ***local variable*** is declared by a *local_variable_declaration*, which may occur in a *block*, a *for_statement*, a *switch_statement* or a *using_statement*; or by a *foreach_statement* or a *specific_catch_clause* for a *try_statement*.</span></span>

<span data-ttu-id="1a442-166">Doba života místní proměnné je část provádění programu, během které je zaručeno, že je úložiště rezervováno pro něj.</span><span class="sxs-lookup"><span data-stu-id="1a442-166">The lifetime of a local variable is the portion of program execution during which storage is guaranteed to be reserved for it.</span></span> <span data-ttu-id="1a442-167">Tato doba života sahá aspoň od vstupu do *bloku*, *for_statement*, *switch_statement*, *using_statement*, *foreach_statement*nebo *specific_catch_clause* , ke kterému je přidružená, až do provádění tohoto *bloku*, *for_statement*, *switch_statement*, *using_statement*, *foreach_statement*nebo *specific_catch_clause* končí jakýmkoli způsobem.</span><span class="sxs-lookup"><span data-stu-id="1a442-167">This lifetime extends at least from entry into the *block*, *for_statement*, *switch_statement*, *using_statement*, *foreach_statement*, or *specific_catch_clause* with which it is associated, until execution of that *block*, *for_statement*, *switch_statement*, *using_statement*, *foreach_statement*, or *specific_catch_clause* ends in any way.</span></span> <span data-ttu-id="1a442-168">(Vstup do uzavřeného *bloku* nebo volání metody pozastaví, ale ne na konec, spuštění aktuálního *bloku*, *for_statement*, *switch_statement*, *using_statement*, *foreach_statement*nebo *specific_ catch_clause*.) Pokud je lokální proměnná zachycena anonymní funkcí ([zaznamenanými vnějšími proměnnými](expressions.md#captured-outer-variables)), její životnost přesahuje alespoň do doby, než se vytvoří delegát nebo strom výrazů vytvořeného z anonymní funkce, spolu s dalšími objekty, které jsou na odkaz zachycená proměnná, která je vhodná pro uvolňování paměti.</span><span class="sxs-lookup"><span data-stu-id="1a442-168">(Entering an enclosed *block* or calling a method suspends, but does not end, execution of the current *block*, *for_statement*, *switch_statement*, *using_statement*, *foreach_statement*, or *specific_catch_clause*.) If the local variable is captured by an anonymous function ([Captured outer variables](expressions.md#captured-outer-variables)), its lifetime extends at least until the delegate or expression tree created from the anonymous function, along with any other objects that come to reference the captured variable, are eligible for garbage collection.</span></span>

<span data-ttu-id="1a442-169">Pokud se nadřazený *blok*, *for_statement*, *switch_statement*, *using_statement*, *foreach_statement*nebo *specific_catch_clause* zadá rekurzivně, vytvoří se nová instance místní proměnné. čas a jeho *local_variable_initializer*, pokud jsou nějaké, se vyhodnotí pokaždé.</span><span class="sxs-lookup"><span data-stu-id="1a442-169">If the parent *block*, *for_statement*, *switch_statement*, *using_statement*, *foreach_statement*, or *specific_catch_clause* is entered recursively, a new instance of the local variable is created each time, and its *local_variable_initializer*, if any, is evaluated each time.</span></span>

<span data-ttu-id="1a442-170">Lokální proměnná zavedená *local_variable_declaration* se neinicializuje automaticky, takže nemá žádnou výchozí hodnotu.</span><span class="sxs-lookup"><span data-stu-id="1a442-170">A local variable introduced by a *local_variable_declaration* is not automatically initialized and thus has no default value.</span></span> <span data-ttu-id="1a442-171">Pro účely jednoznačné kontroly přiřazení je místní proměnná zavedená *local_variable_declaration* považována za původně nepřiřazenou.</span><span class="sxs-lookup"><span data-stu-id="1a442-171">For the purpose of definite assignment checking, a local variable introduced by a *local_variable_declaration* is considered initially unassigned.</span></span> <span data-ttu-id="1a442-172">*Local_variable_declaration* může obsahovat *local_variable_initializer*. v takovém případě se proměnná považuje za jednoznačně přiřazenou až po inicializaci výrazu ([příkazy deklarace](variables.md#declaration-statements)).</span><span class="sxs-lookup"><span data-stu-id="1a442-172">A *local_variable_declaration* may include a *local_variable_initializer*, in which case the variable is considered definitely assigned only after the initializing expression ([Declaration statements](variables.md#declaration-statements)).</span></span>

<span data-ttu-id="1a442-173">V rámci rozsahu místní proměnné zavedené *local_variable_declaration*se jedná o chybu při kompilaci, která odkazuje na tuto místní proměnnou v textové pozici, která předchází své *local_variable_declarator*.</span><span class="sxs-lookup"><span data-stu-id="1a442-173">Within the scope of a local variable introduced by a *local_variable_declaration*, it is a compile-time error to refer to that local variable in a textual position that precedes its *local_variable_declarator*.</span></span> <span data-ttu-id="1a442-174">Pokud je deklarace lokální proměnné implicitní ([místní deklarace proměnných](statements.md#local-variable-declarations)), je také chyba pro odkazování na proměnnou v rámci jejího *local_variable_declarator*.</span><span class="sxs-lookup"><span data-stu-id="1a442-174">If the local variable declaration is implicit ([Local variable declarations](statements.md#local-variable-declarations)), it is also an error to refer to the variable within its *local_variable_declarator*.</span></span>

<span data-ttu-id="1a442-175">Místní proměnná zavedená *foreach_statement* nebo *specific_catch_clause* se považuje za jednoznačně přiřazenou v celém oboru.</span><span class="sxs-lookup"><span data-stu-id="1a442-175">A local variable introduced by a *foreach_statement* or a *specific_catch_clause* is considered definitely assigned in its entire scope.</span></span>

<span data-ttu-id="1a442-176">Skutečná doba života místní proměnné je závislá na implementaci.</span><span class="sxs-lookup"><span data-stu-id="1a442-176">The actual lifetime of a local variable is implementation-dependent.</span></span> <span data-ttu-id="1a442-177">Kompilátor může například staticky určit, že místní proměnná v bloku je použita pouze pro malou část tohoto bloku.</span><span class="sxs-lookup"><span data-stu-id="1a442-177">For example, a compiler might statically determine that a local variable in a block is only used for a small portion of that block.</span></span> <span data-ttu-id="1a442-178">Pomocí této analýzy může kompilátor vygenerovat kód, který má za následek, že úložiště proměnných má kratší dobu života, než je blok, který ho obsahuje.</span><span class="sxs-lookup"><span data-stu-id="1a442-178">Using this analysis, the compiler could generate code that results in the variable's storage having a shorter lifetime than its containing block.</span></span>

<span data-ttu-id="1a442-179">Úložiště, na které odkazuje místní referenční proměnná, se uvolní nezávisle na době života této místní referenční proměnné ([Automatická správa paměti](basic-concepts.md#automatic-memory-management)).</span><span class="sxs-lookup"><span data-stu-id="1a442-179">The storage referred to by a local reference variable is reclaimed independently of the lifetime of that local reference variable ([Automatic memory management](basic-concepts.md#automatic-memory-management)).</span></span>

## <a name="default-values"></a><span data-ttu-id="1a442-180">Výchozí hodnoty</span><span class="sxs-lookup"><span data-stu-id="1a442-180">Default values</span></span>

<span data-ttu-id="1a442-181">Následující kategorie proměnných jsou automaticky inicializovány na výchozí hodnoty:</span><span class="sxs-lookup"><span data-stu-id="1a442-181">The following categories of variables are automatically initialized to their default values:</span></span>

*  <span data-ttu-id="1a442-182">Statické proměnné.</span><span class="sxs-lookup"><span data-stu-id="1a442-182">Static variables.</span></span>
*  <span data-ttu-id="1a442-183">Proměnné instance instancí třídy.</span><span class="sxs-lookup"><span data-stu-id="1a442-183">Instance variables of class instances.</span></span>
*  <span data-ttu-id="1a442-184">Prvky pole.</span><span class="sxs-lookup"><span data-stu-id="1a442-184">Array elements.</span></span>

<span data-ttu-id="1a442-185">Výchozí hodnota proměnné závisí na typu proměnné a je určena následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="1a442-185">The default value of a variable depends on the type of the variable and is determined as follows:</span></span>

*  <span data-ttu-id="1a442-186">Pro proměnnou *value_type*je výchozí hodnota shodná s hodnotou vypočítanou výchozím konstruktorem *value_type*([výchozí konstruktory](types.md#default-constructors)).</span><span class="sxs-lookup"><span data-stu-id="1a442-186">For a variable of a *value_type*, the default value is the same as the value computed by the *value_type*'s default constructor ([Default constructors](types.md#default-constructors)).</span></span>
*  <span data-ttu-id="1a442-187">Pro proměnnou *reference_type*je `null`výchozí hodnota.</span><span class="sxs-lookup"><span data-stu-id="1a442-187">For a variable of a *reference_type*, the default value is `null`.</span></span>

<span data-ttu-id="1a442-188">Inicializace na výchozí hodnoty je obvykle prováděna tím, že správce paměti nebo systém uvolňování paměti inicializuje paměť na všechny bity – nula, než je přiděleno k použití.</span><span class="sxs-lookup"><span data-stu-id="1a442-188">Initialization to default values is typically done by having the memory manager or garbage collector initialize memory to all-bits-zero before it is allocated for use.</span></span> <span data-ttu-id="1a442-189">Z tohoto důvodu je vhodné použít všechny bity-Zero k vyjádření nulového odkazu.</span><span class="sxs-lookup"><span data-stu-id="1a442-189">For this reason, it is convenient to use all-bits-zero to represent the null reference.</span></span>

## <a name="definite-assignment"></a><span data-ttu-id="1a442-190">Jednoznačné přiřazení</span><span class="sxs-lookup"><span data-stu-id="1a442-190">Definite assignment</span></span>

<span data-ttu-id="1a442-191">V daném umístění v kódu spustitelného členu funkce je proměnná označena jako ***jednoznačně přiřazená*** , pokud kompilátor může prokázat konkrétní statickou analýzu toku ([přesné pravidlo pro určení jednoznačného přiřazení](variables.md#precise-rules-for-determining-definite-assignment)), že proměnná byl automaticky inicializován nebo byl cílem alespoň jednoho přiřazení.</span><span class="sxs-lookup"><span data-stu-id="1a442-191">At a given location in the executable code of a function member, a variable is said to be ***definitely assigned*** if the compiler can prove, by a particular static flow analysis ([Precise rules for determining definite assignment](variables.md#precise-rules-for-determining-definite-assignment)), that the variable has been automatically initialized or has been the target of at least one assignment.</span></span> <span data-ttu-id="1a442-192">Pravidla jednoznačného přiřazení jsou neformálně uvedená:</span><span class="sxs-lookup"><span data-stu-id="1a442-192">Informally stated, the rules of definite assignment are:</span></span>

*  <span data-ttu-id="1a442-193">Původně přiřazená proměnná ([původně přiřazené proměnné](variables.md#initially-assigned-variables)) se vždycky považuje za jednoznačně přiřazenou.</span><span class="sxs-lookup"><span data-stu-id="1a442-193">An initially assigned variable ([Initially assigned variables](variables.md#initially-assigned-variables)) is always considered definitely assigned.</span></span>
*  <span data-ttu-id="1a442-194">Původně Nepřiřazená proměnná ([zpočátku nepřiřazené proměnné](variables.md#initially-unassigned-variables)) se považuje za jednoznačně přiřazenou v daném umístění, pokud všechny možné cesty spuštění, které vede k tomuto umístění, obsahují alespoň jednu z následujících možností:</span><span class="sxs-lookup"><span data-stu-id="1a442-194">An initially unassigned variable ([Initially unassigned variables](variables.md#initially-unassigned-variables)) is considered definitely assigned at a given location if all possible execution paths leading to that location contain at least one of the following:</span></span>
    * <span data-ttu-id="1a442-195">Jednoduché přiřazení ([jednoduché přiřazení](expressions.md#simple-assignment)), ve kterém je proměnná levý operand.</span><span class="sxs-lookup"><span data-stu-id="1a442-195">A simple assignment ([Simple assignment](expressions.md#simple-assignment)) in which the variable is the left operand.</span></span>
    * <span data-ttu-id="1a442-196">Výraz vyvolání ([výrazy vyvolání](expressions.md#invocation-expressions)) nebo výraz pro vytvoření objektu (výrazy pro[vytvoření objektu](expressions.md#object-creation-expressions)), který tuto proměnnou předává jako výstupní parametr.</span><span class="sxs-lookup"><span data-stu-id="1a442-196">An invocation expression ([Invocation expressions](expressions.md#invocation-expressions)) or object creation expression ([Object creation expressions](expressions.md#object-creation-expressions)) that passes the variable as an output parameter.</span></span>
    * <span data-ttu-id="1a442-197">Místní proměnná, deklaraci lokální proměnné ([deklarace místní proměnné](statements.md#local-variable-declarations)), která obsahuje inicializátor proměnné.</span><span class="sxs-lookup"><span data-stu-id="1a442-197">For a local variable, a local variable declaration ([Local variable declarations](statements.md#local-variable-declarations)) that includes a variable initializer.</span></span>

<span data-ttu-id="1a442-198">Formální specifikace uvedená výše neformálních pravidel je popsána v [úvodně přiřazených proměnných](variables.md#initially-assigned-variables), [počátečních nepřiřazených proměnných](variables.md#initially-unassigned-variables)a [přesných pravidlech pro určení jednoznačného přiřazení](variables.md#precise-rules-for-determining-definite-assignment).</span><span class="sxs-lookup"><span data-stu-id="1a442-198">The formal specification underlying the above informal rules is described in [Initially assigned variables](variables.md#initially-assigned-variables), [Initially unassigned variables](variables.md#initially-unassigned-variables), and [Precise rules for determining definite assignment](variables.md#precise-rules-for-determining-definite-assignment).</span></span>

<span data-ttu-id="1a442-199">Přesné stavy přiřazení proměnných instance proměnné *struct_type* jsou sledovány jednotlivě a společně.</span><span class="sxs-lookup"><span data-stu-id="1a442-199">The definite assignment states of instance variables of a *struct_type* variable are tracked individually as well as collectively.</span></span> <span data-ttu-id="1a442-200">Kromě výše uvedených pravidel platí následující pravidla pro proměnné *struct_type* a jejich proměnné instance:</span><span class="sxs-lookup"><span data-stu-id="1a442-200">In addition to the rules above, the following rules apply to *struct_type* variables and their instance variables:</span></span>

*  <span data-ttu-id="1a442-201">Proměnná instance se považuje za jednoznačně přiřazenou, pokud je její obsahující proměnná *struct_type* považována za jednoznačně přiřazenou.</span><span class="sxs-lookup"><span data-stu-id="1a442-201">An instance variable is considered definitely assigned if its containing *struct_type* variable is considered definitely assigned.</span></span>
*  <span data-ttu-id="1a442-202">Proměnná *struct_type* se považuje za jednoznačně přiřazenou, pokud je každá z jejích proměnných instance považována za jednoznačně přiřazenou.</span><span class="sxs-lookup"><span data-stu-id="1a442-202">A *struct_type* variable is considered definitely assigned if each of its instance variables is considered definitely assigned.</span></span>

<span data-ttu-id="1a442-203">Jednoznačné přiřazení je požadavek v následujících kontextech:</span><span class="sxs-lookup"><span data-stu-id="1a442-203">Definite assignment is a requirement in the following contexts:</span></span>

*  <span data-ttu-id="1a442-204">Proměnná musí být jednoznačně přiřazena na každém místě, kde je jeho hodnota získána.</span><span class="sxs-lookup"><span data-stu-id="1a442-204">A variable must be definitely assigned at each location where its value is obtained.</span></span> <span data-ttu-id="1a442-205">Tím se zajistí, že nedefinované hodnoty nebudou nikdy provedeny.</span><span class="sxs-lookup"><span data-stu-id="1a442-205">This ensures that undefined values never occur.</span></span> <span data-ttu-id="1a442-206">Výskyt proměnné ve výrazu je považován za získání hodnoty proměnné, s výjimkou případů, kdy</span><span class="sxs-lookup"><span data-stu-id="1a442-206">The occurrence of a variable in an expression is considered to obtain the value of the variable, except when</span></span>
    * <span data-ttu-id="1a442-207">proměnná je levý operand jednoduchého přiřazení,</span><span class="sxs-lookup"><span data-stu-id="1a442-207">the variable is the left operand of a simple assignment,</span></span>
    * <span data-ttu-id="1a442-208">proměnná se předává jako výstupní parametr nebo</span><span class="sxs-lookup"><span data-stu-id="1a442-208">the variable is passed as an output parameter, or</span></span>
    * <span data-ttu-id="1a442-209">proměnná je *struct_type* proměnná a nastane jako levý operand přístupu ke členu.</span><span class="sxs-lookup"><span data-stu-id="1a442-209">the variable is a *struct_type* variable and occurs as the left operand of a member access.</span></span>
*  <span data-ttu-id="1a442-210">Proměnná musí být jednoznačně přiřazena na každém místě, kde je předána jako parametr reference.</span><span class="sxs-lookup"><span data-stu-id="1a442-210">A variable must be definitely assigned at each location where it is passed as a reference parameter.</span></span> <span data-ttu-id="1a442-211">Tím je zajištěno, že vyvolaná člen funkce může uvažovat o počátečním přiřazeném parametru odkazu.</span><span class="sxs-lookup"><span data-stu-id="1a442-211">This ensures that the function member being invoked can consider the reference parameter initially assigned.</span></span>
*  <span data-ttu-id="1a442-212">Všechny výstupní parametry členu funkce musí být jednoznačně přiřazeny v každém umístění, kde vrací člen funkce (prostřednictvím `return` příkazu nebo prostřednictvím provádění, které se blíží konci těla člena funkce).</span><span class="sxs-lookup"><span data-stu-id="1a442-212">All output parameters of a function member must be definitely assigned at each location where the function member returns (through a `return` statement or through execution reaching the end of the function member body).</span></span> <span data-ttu-id="1a442-213">Tím se zajistí, že členové funkce nebudou vracet nedefinované hodnoty v parametrech Output, takže Kompilátor považuje volání člena funkce, které přebírá proměnnou jako výstupní parametr ekvivalentní přiřazení k proměnné.</span><span class="sxs-lookup"><span data-stu-id="1a442-213">This ensures that function members do not return undefined values in output parameters, thus enabling the compiler to consider a function member invocation that takes a variable as an output parameter equivalent to an assignment to the variable.</span></span>
*  <span data-ttu-id="1a442-214">Proměnná konstruktoru instance struct_type musí být jednoznačně přiřazena v každém umístění, kde se konstruktor instance vrátí. `this`</span><span class="sxs-lookup"><span data-stu-id="1a442-214">The `this` variable of a *struct_type* instance constructor must be definitely assigned at each location where that instance constructor returns.</span></span>

### <a name="initially-assigned-variables"></a><span data-ttu-id="1a442-215">Původně přiřazené proměnné</span><span class="sxs-lookup"><span data-stu-id="1a442-215">Initially assigned variables</span></span>

<span data-ttu-id="1a442-216">Následující kategorie proměnných jsou klasifikovány jako původně přiřazené:</span><span class="sxs-lookup"><span data-stu-id="1a442-216">The following categories of variables are classified as initially assigned:</span></span>

*  <span data-ttu-id="1a442-217">Statické proměnné.</span><span class="sxs-lookup"><span data-stu-id="1a442-217">Static variables.</span></span>
*  <span data-ttu-id="1a442-218">Proměnné instance instancí třídy.</span><span class="sxs-lookup"><span data-stu-id="1a442-218">Instance variables of class instances.</span></span>
*  <span data-ttu-id="1a442-219">Proměnné instance původně přiřazených proměnných struktury.</span><span class="sxs-lookup"><span data-stu-id="1a442-219">Instance variables of initially assigned struct variables.</span></span>
*  <span data-ttu-id="1a442-220">Prvky pole.</span><span class="sxs-lookup"><span data-stu-id="1a442-220">Array elements.</span></span>
*  <span data-ttu-id="1a442-221">Parametry hodnoty.</span><span class="sxs-lookup"><span data-stu-id="1a442-221">Value parameters.</span></span>
*  <span data-ttu-id="1a442-222">Parametry odkazu</span><span class="sxs-lookup"><span data-stu-id="1a442-222">Reference parameters.</span></span>
*  <span data-ttu-id="1a442-223">Proměnné deklarované v `catch` klauzuli `foreach` nebo příkazu.</span><span class="sxs-lookup"><span data-stu-id="1a442-223">Variables declared in a `catch` clause or a `foreach` statement.</span></span>

### <a name="initially-unassigned-variables"></a><span data-ttu-id="1a442-224">Původně nepřiřazené proměnné</span><span class="sxs-lookup"><span data-stu-id="1a442-224">Initially unassigned variables</span></span>

<span data-ttu-id="1a442-225">Následující kategorie proměnných jsou klasifikovány jako původně nepřiřazené:</span><span class="sxs-lookup"><span data-stu-id="1a442-225">The following categories of variables are classified as initially unassigned:</span></span>

*  <span data-ttu-id="1a442-226">Proměnné instance počátečních nepřiřazených proměnných struktury.</span><span class="sxs-lookup"><span data-stu-id="1a442-226">Instance variables of initially unassigned struct variables.</span></span>
*  <span data-ttu-id="1a442-227">Výstupní parametry, včetně `this` proměnné konstruktory instance struktury.</span><span class="sxs-lookup"><span data-stu-id="1a442-227">Output parameters, including the `this` variable of struct instance constructors.</span></span>
*  <span data-ttu-id="1a442-228">Místní proměnné, s výjimkou těch, `catch` které jsou deklarovány v klauzuli `foreach` nebo příkazu.</span><span class="sxs-lookup"><span data-stu-id="1a442-228">Local variables, except those declared in a `catch` clause or a `foreach` statement.</span></span>

### <a name="precise-rules-for-determining-definite-assignment"></a><span data-ttu-id="1a442-229">Přesné pravidla pro určení jednoznačného přiřazení</span><span class="sxs-lookup"><span data-stu-id="1a442-229">Precise rules for determining definite assignment</span></span>

<span data-ttu-id="1a442-230">Aby bylo možné určit, že je každá použitá proměnná jednoznačně přiřazená, kompilátor musí použít proces, který je ekvivalentní k tomu popsanému v této části.</span><span class="sxs-lookup"><span data-stu-id="1a442-230">In order to determine that each used variable is definitely assigned, the compiler must use a process that is equivalent to the one described in this section.</span></span>

<span data-ttu-id="1a442-231">Kompilátor zpracovává tělo každého členu funkce, který má jednu nebo více počátečních nepřiřazených proměnných.</span><span class="sxs-lookup"><span data-stu-id="1a442-231">The compiler processes the body of each function member that has one or more initially unassigned variables.</span></span> <span data-ttu-id="1a442-232">Pro každou původně nepřiřazenou proměnnou *v*kompilátor *určuje v každém* z následujících bodů člena funkce ***jednoznačný stav přiřazení*** :</span><span class="sxs-lookup"><span data-stu-id="1a442-232">For each initially unassigned variable *v*, the compiler determines a ***definite assignment state*** for *v* at each of the following points in the function member:</span></span>

*  <span data-ttu-id="1a442-233">Na začátku každého příkazu</span><span class="sxs-lookup"><span data-stu-id="1a442-233">At the beginning of each statement</span></span>
*  <span data-ttu-id="1a442-234">Na koncovém bodu ([koncové body a dosažitelnost](statements.md#end-points-and-reachability)) každého příkazu</span><span class="sxs-lookup"><span data-stu-id="1a442-234">At the end point ([End points and reachability](statements.md#end-points-and-reachability)) of each statement</span></span>
*  <span data-ttu-id="1a442-235">U každého oblouku, který přenáší řízení na jiný příkaz nebo na koncový bod příkazu</span><span class="sxs-lookup"><span data-stu-id="1a442-235">On each arc which transfers control to another statement or to the end point of a statement</span></span>
*  <span data-ttu-id="1a442-236">Na začátku každého výrazu</span><span class="sxs-lookup"><span data-stu-id="1a442-236">At the beginning of each expression</span></span>
*  <span data-ttu-id="1a442-237">Na konci každého výrazu</span><span class="sxs-lookup"><span data-stu-id="1a442-237">At the end of each expression</span></span>

<span data-ttu-id="1a442-238">Určitý stav přiřazení *v* aplikaci může být:</span><span class="sxs-lookup"><span data-stu-id="1a442-238">The definite assignment state of *v* can be either:</span></span>

*  <span data-ttu-id="1a442-239">Jednoznačně přiřazené.</span><span class="sxs-lookup"><span data-stu-id="1a442-239">Definitely assigned.</span></span> <span data-ttu-id="1a442-240">To znamená, že u všech možných toků řízení k tomuto bodu *v* je přiřazena hodnota.</span><span class="sxs-lookup"><span data-stu-id="1a442-240">This indicates that on all possible control flows to this point, *v* has been assigned a value.</span></span>
*  <span data-ttu-id="1a442-241">Není jednoznačně přiřazeno.</span><span class="sxs-lookup"><span data-stu-id="1a442-241">Not definitely assigned.</span></span> <span data-ttu-id="1a442-242">Pro stav proměnné na konci výrazu typu `bool`může stav proměnné, která není jednoznačně přiřazená, být (ale nemusí nutně) patřit do jednoho z následujících podřízených stavů:</span><span class="sxs-lookup"><span data-stu-id="1a442-242">For the state of a variable at the end of an expression of type `bool`, the state of a variable that isn't definitely assigned may (but doesn't necessarily) fall into one of the following sub-states:</span></span>
    * <span data-ttu-id="1a442-243">Po hodnotě true se přiřadí výraz s omezením.</span><span class="sxs-lookup"><span data-stu-id="1a442-243">Definitely assigned after true expression.</span></span> <span data-ttu-id="1a442-244">Tento stav označuje, že hodnota *v* je jednoznačně přiřazená, pokud se logický výraz vyhodnotí jako true, ale není nutně přiřazený, pokud se logický výraz vyhodnotí jako false.</span><span class="sxs-lookup"><span data-stu-id="1a442-244">This state indicates that *v* is definitely assigned if the boolean expression evaluated as true, but is not necessarily assigned if the boolean expression evaluated as false.</span></span>
    * <span data-ttu-id="1a442-245">Po omezení se přiřadí po nepravdivém výrazu.</span><span class="sxs-lookup"><span data-stu-id="1a442-245">Definitely assigned after false expression.</span></span> <span data-ttu-id="1a442-246">Tento stav označuje, že hodnota *v* je jednoznačně přiřazená, pokud se logický výraz vyhodnotí jako false, ale není nutně přiřazený, pokud se logický výraz vyhodnotí jako true.</span><span class="sxs-lookup"><span data-stu-id="1a442-246">This state indicates that *v* is definitely assigned if the boolean expression evaluated as false, but is not necessarily assigned if the boolean expression evaluated as true.</span></span>

<span data-ttu-id="1a442-247">Následující pravidla určují, jak se určuje stav proměnné *v* jednotlivých umístěních.</span><span class="sxs-lookup"><span data-stu-id="1a442-247">The following rules govern how the state of a variable *v* is determined at each location.</span></span>

#### <a name="general-rules-for-statements"></a><span data-ttu-id="1a442-248">Obecná pravidla pro příkazy</span><span class="sxs-lookup"><span data-stu-id="1a442-248">General rules for statements</span></span>

*  <span data-ttu-id="1a442-249">*v* není jednoznačně přiřazen na začátku těla členu funkce.</span><span class="sxs-lookup"><span data-stu-id="1a442-249">*v* is not definitely assigned at the beginning of a function member body.</span></span>
*  <span data-ttu-id="1a442-250">*v* je jednoznačně přiřazen na začátku libovolného nedosažitelného příkazu.</span><span class="sxs-lookup"><span data-stu-id="1a442-250">*v* is definitely assigned at the beginning of any unreachable statement.</span></span>
*  <span data-ttu-id="1a442-251">Určitý stav přiřazení *v* v na začátku jakéhokoli jiného příkazu je určen kontrolou omezeného stavu přiřazení *v* u všech přenosů toku řízení, které cílí na začátek tohoto příkazu.</span><span class="sxs-lookup"><span data-stu-id="1a442-251">The definite assignment state of *v* at the beginning of any other statement is determined by checking the definite assignment state of *v* on all control flow transfers that target the beginning of that statement.</span></span> <span data-ttu-id="1a442-252">If (a pouze if) *v* se jednoznačně přiřazují u všech takových přenosů toku řízení, pak je *v* systému jednoznačně přiřazen na začátku prohlášení.</span><span class="sxs-lookup"><span data-stu-id="1a442-252">If (and only if) *v* is definitely assigned on all such control flow transfers, then *v* is definitely assigned at the beginning of the statement.</span></span> <span data-ttu-id="1a442-253">Sada možných přenosů toků řízení je určena stejným způsobem jako při ověřování dostupnosti příkazů ([koncových bodů a dostupnosti](statements.md#end-points-and-reachability)).</span><span class="sxs-lookup"><span data-stu-id="1a442-253">The set of possible control flow transfers is determined in the same way as for checking statement reachability ([End points and reachability](statements.md#end-points-and-reachability)).</span></span>
*  <span data-ttu-id="1a442-254">Jednoznačný stav přiřazení *v v* koncovém bodě `checked`bloku,, `lock` `foreach` `do` `while` `unchecked` `if`,,,, `for`,,, `using` nebo`switch`příkaz je určen kontrolou omezeného stavu přiřazení *v* v u všech přenosů toku řízení, které cílí na koncový bod tohoto příkazu.</span><span class="sxs-lookup"><span data-stu-id="1a442-254">The definite assignment state of *v* at the end point of a block, `checked`, `unchecked`, `if`, `while`, `do`, `for`, `foreach`, `lock`, `using`, or `switch` statement is determined by checking the definite assignment state of *v* on all control flow transfers that target the end point of that statement.</span></span> <span data-ttu-id="1a442-255">Pokud je *v* jednoznačně přiřazeno pro všechny takové přenosy toku řízení, pak *v* je jednoznačně přiřazeno na koncový bod příkazu.</span><span class="sxs-lookup"><span data-stu-id="1a442-255">If *v* is definitely assigned on all such control flow transfers, then *v* is definitely assigned at the end point of the statement.</span></span> <span data-ttu-id="1a442-256">Případech *v* není jednoznačně přiřazen na koncový bod příkazu.</span><span class="sxs-lookup"><span data-stu-id="1a442-256">Otherwise; *v* is not definitely assigned at the end point of the statement.</span></span> <span data-ttu-id="1a442-257">Sada možných přenosů toků řízení je určena stejným způsobem jako při ověřování dostupnosti příkazů ([koncových bodů a dostupnosti](statements.md#end-points-and-reachability)).</span><span class="sxs-lookup"><span data-stu-id="1a442-257">The set of possible control flow transfers is determined in the same way as for checking statement reachability ([End points and reachability](statements.md#end-points-and-reachability)).</span></span>

#### <a name="block-statements-checked-and-unchecked-statements"></a><span data-ttu-id="1a442-258">Příkazy bloku, zkontrolované a nezaškrtnuté příkazy</span><span class="sxs-lookup"><span data-stu-id="1a442-258">Block statements, checked, and unchecked statements</span></span>

<span data-ttu-id="1a442-259">Nekonečný stav přiřazení v *v ovládacím* prvku přenáší do prvního příkazu seznamu příkazů v bloku (nebo na koncový bod bloku, pokud je seznam příkazů prázdný) stejný jako u zvláštního příkazu přiřazení *v před blok* . , `checked` nebo`unchecked` příkaz.</span><span class="sxs-lookup"><span data-stu-id="1a442-259">The definite assignment state of *v* on the control transfer to the first statement of the statement list in the block (or to the end point of the block, if the statement list is empty) is the same as the definite assignment statement of *v* before the block, `checked`, or `unchecked` statement.</span></span>

#### <a name="expression-statements"></a><span data-ttu-id="1a442-260">Příkazy výrazu</span><span class="sxs-lookup"><span data-stu-id="1a442-260">Expression statements</span></span>

<span data-ttu-id="1a442-261">Pro příkaz výrazu *stmt* , který se skládá z výrazu *expr*:</span><span class="sxs-lookup"><span data-stu-id="1a442-261">For an expression statement *stmt* that consists of the expression *expr*:</span></span>

*  <span data-ttu-id="1a442-262">*v* má stejný jednoznačný stav přiřazení na začátku *výrazu* jako na začátku *stmt*.</span><span class="sxs-lookup"><span data-stu-id="1a442-262">*v* has the same definite assignment state at the beginning of *expr* as at the beginning of *stmt*.</span></span>
*  <span data-ttu-id="1a442-263">Pokud je *v* případě jednoznačně přiřazeno na konci *výrazu expr*, je jednoznačně přiřazen na koncový bod *stmt*; případech neomezeně se nepřiřazuje na koncový bod *stmt*.</span><span class="sxs-lookup"><span data-stu-id="1a442-263">If *v* if definitely assigned at the end of *expr*, it is definitely assigned at the end point of *stmt*; otherwise; it is not definitely assigned at the end point of *stmt*.</span></span>

#### <a name="declaration-statements"></a><span data-ttu-id="1a442-264">Příkazy deklarace</span><span class="sxs-lookup"><span data-stu-id="1a442-264">Declaration statements</span></span>

*  <span data-ttu-id="1a442-265">Pokud *stmt* je příkaz deklarace bez inicializátorů, pak *v* má stejný jednoznačný stav přiřazení na koncovém bodu *stmt* jako na začátku *stmt*.</span><span class="sxs-lookup"><span data-stu-id="1a442-265">If *stmt* is a declaration statement without initializers, then *v* has the same definite assignment state at the end point of *stmt* as at the beginning of *stmt*.</span></span>
*  <span data-ttu-id="1a442-266">Pokud je *stmt* příkazem deklarace s inicializátory, pak je stanovený stav přiřazení *v v v* , jako by *stmt* byl seznam příkazů s jedním příkazem přiřazení pro každou deklaraci s inicializátorem (v pořadí deklarace).</span><span class="sxs-lookup"><span data-stu-id="1a442-266">If *stmt* is a declaration statement with initializers, then the definite assignment state for *v* is determined as if *stmt* were a statement list, with one assignment statement for each declaration with an initializer (in the order of declaration).</span></span>

#### <a name="if-statements"></a><span data-ttu-id="1a442-267">Příkazy if</span><span class="sxs-lookup"><span data-stu-id="1a442-267">If statements</span></span>

<span data-ttu-id="1a442-268">Pro příkaz stmt ve tvaru: `if`</span><span class="sxs-lookup"><span data-stu-id="1a442-268">For an `if` statement *stmt* of the form:</span></span>
```csharp
if ( expr ) then_stmt else else_stmt
```

*  <span data-ttu-id="1a442-269">*v* má stejný jednoznačný stav přiřazení na začátku *výrazu* jako na začátku *stmt*.</span><span class="sxs-lookup"><span data-stu-id="1a442-269">*v* has the same definite assignment state at the beginning of *expr* as at the beginning of *stmt*.</span></span>
*  <span data-ttu-id="1a442-270">Pokud je *v* systému jednoznačně přiřazen na konci *výrazu expr*, pak je jednoznačně přiřazen k přenosu toku řízení do *then_stmt* a buď *else_stmt* , nebo do koncového bodu *stmt* , pokud neexistuje klauzule else.</span><span class="sxs-lookup"><span data-stu-id="1a442-270">If *v* is definitely assigned at the end of *expr*, then it is definitely assigned on the control flow transfer to *then_stmt* and to either *else_stmt* or to the end-point of *stmt* if there is no else clause.</span></span>
*  <span data-ttu-id="1a442-271">Pokud *v* má na konci výrazu výraz "s omezením" jednoznačně přiřazený po hodnotě *true, pak*je jednoznačně přiřazený k přenosu toku řízení do *then_stmt*a není jednoznačně přiřazený k přenosu toku řízení *else_. stmt* nebo na koncový bod *stmt* , pokud neexistuje klauzule else.</span><span class="sxs-lookup"><span data-stu-id="1a442-271">If *v* has the state "definitely assigned after true expression" at the end of *expr*, then it is definitely assigned on the control flow transfer to *then_stmt*, and not definitely assigned on the control flow transfer to either *else_stmt* or to the end-point of *stmt* if there is no else clause.</span></span>
*  <span data-ttu-id="1a442-272">Pokud *v* má stav "po nepravdivém výrazu" po nepravdivém výrazu "na *konci výrazu", pak*je jednoznačně přiřazen k přenosu toku řízení do *else_stmt*a není jednoznačně přiřazen k přenosu toku řízení do *then_stmt* .</span><span class="sxs-lookup"><span data-stu-id="1a442-272">If *v* has the state "definitely assigned after false expression" at the end of *expr*, then it is definitely assigned on the control flow transfer to *else_stmt*, and not definitely assigned on the control flow transfer to *then_stmt*.</span></span> <span data-ttu-id="1a442-273">V případě, že je v konečném bodě *stmt* k dispozici, je jednoznačně přiřazená, pokud je k dispozici pouze v případě, že je na koncovém bodu *then_stmt*.</span><span class="sxs-lookup"><span data-stu-id="1a442-273">It is definitely assigned at the end-point of *stmt* if and only if it is definitely assigned at the end-point of *then_stmt*.</span></span>
*  <span data-ttu-id="1a442-274">V opačném případě je *v* tomto případě považována za neomezenou hodnotu pro přenos toku řízení do *then_stmt* nebo *else_stmt*, nebo do koncového bodu *stmt* , pokud neexistuje klauzule else.</span><span class="sxs-lookup"><span data-stu-id="1a442-274">Otherwise, *v* is considered not definitely assigned on the control flow transfer to either the *then_stmt* or *else_stmt*, or to the end-point of *stmt* if there is no else clause.</span></span>

#### <a name="switch-statements"></a><span data-ttu-id="1a442-275">Příkazy Switch</span><span class="sxs-lookup"><span data-stu-id="1a442-275">Switch statements</span></span>

<span data-ttu-id="1a442-276">V příkazu stmt *s výrazem řízení výrazu:* `switch`</span><span class="sxs-lookup"><span data-stu-id="1a442-276">In a `switch` statement *stmt* with a controlling expression *expr*:</span></span>

*  <span data-ttu-id="1a442-277">Určitý stav přiřazení *v v* na začátku *výrazu* je stejný jako stav *v* na začátku *stmt*.</span><span class="sxs-lookup"><span data-stu-id="1a442-277">The definite assignment state of *v* at the beginning of *expr* is the same as the state of *v* at the beginning of *stmt*.</span></span>
*  <span data-ttu-id="1a442-278">Určitý stav přiřazení *v* v je přenos toku řízení do seznamu příkazů bloku, který je k dispozici, je stejný jako jednoznačný stav přiřazení *v* v na konci *výrazu expr*.</span><span class="sxs-lookup"><span data-stu-id="1a442-278">The definite assignment state of *v* on the control flow transfer to a reachable switch block statement list is the same as the definite assignment state of *v* at the end of *expr*.</span></span>

#### <a name="while-statements"></a><span data-ttu-id="1a442-279">Příkazy while</span><span class="sxs-lookup"><span data-stu-id="1a442-279">While statements</span></span>

<span data-ttu-id="1a442-280">Pro příkaz stmt ve formátu: `while`</span><span class="sxs-lookup"><span data-stu-id="1a442-280">For a `while` statement *stmt* of the form:</span></span>
```csharp
while ( expr ) while_body
```

*  <span data-ttu-id="1a442-281">*v* má stejný jednoznačný stav přiřazení na začátku *výrazu* jako na začátku *stmt*.</span><span class="sxs-lookup"><span data-stu-id="1a442-281">*v* has the same definite assignment state at the beginning of *expr* as at the beginning of *stmt*.</span></span>
*  <span data-ttu-id="1a442-282">Pokud je *v* systému jednoznačně přiřazen na konci *výrazu expr*, pak je jednoznačně přiřazen k přenosu toku řízení do *while_body* a koncovému bodu *stmt*.</span><span class="sxs-lookup"><span data-stu-id="1a442-282">If *v* is definitely assigned at the end of *expr*, then it is definitely assigned on the control flow transfer to *while_body* and to the end point of *stmt*.</span></span>
*  <span data-ttu-id="1a442-283">Pokud *v* má na konci výrazu výraz "s omezením" jednoznačně přiřazený po hodnotě *true, pak*je jednoznačně přiřazený k přenosu toku řízení do *while_body*, ale ne jednoznačně přiřazený na koncový bod *stmt*.</span><span class="sxs-lookup"><span data-stu-id="1a442-283">If *v* has the state "definitely assigned after true expression" at the end of *expr*, then it is definitely assigned on the control flow transfer to *while_body*, but not definitely assigned at the end-point of *stmt*.</span></span>
*  <span data-ttu-id="1a442-284">Pokud *v* má stav "po nepravdivém výrazu" po nepravdivém výrazu *", pak*je jednoznačně přiřazen k přenosu toku řízení na koncový bod *stmt*, ale ne jednoznačně přiřazený k přenosu toku *řízení do _body*.</span><span class="sxs-lookup"><span data-stu-id="1a442-284">If *v* has the state "definitely assigned after false expression" at the end of *expr*, then it is definitely assigned on the control flow transfer to the end point of *stmt*, but not definitely assigned on the control flow transfer to *while_body*.</span></span>

#### <a name="do-statements"></a><span data-ttu-id="1a442-285">Do – příkazy</span><span class="sxs-lookup"><span data-stu-id="1a442-285">Do statements</span></span>

<span data-ttu-id="1a442-286">Pro příkaz stmt ve formátu: `do`</span><span class="sxs-lookup"><span data-stu-id="1a442-286">For a `do` statement *stmt* of the form:</span></span>
```csharp
do do_body while ( expr ) ;
```

*  <span data-ttu-id="1a442-287">*v* má stejný určitý stav přiřazení pro přenos toku řízení od začátku *stmt* do *do_body* jako na začátku *stmt*.</span><span class="sxs-lookup"><span data-stu-id="1a442-287">*v* has the same definite assignment state on the control flow transfer from the beginning of *stmt* to *do_body* as at the beginning of *stmt*.</span></span>
*  <span data-ttu-id="1a442-288">*v* má stejný jednoznačný stav přiřazení na začátku *výrazu* , jako na koncovém bodu *do_body*.</span><span class="sxs-lookup"><span data-stu-id="1a442-288">*v* has the same definite assignment state at the beginning of *expr* as at the end point of *do_body*.</span></span>
*  <span data-ttu-id="1a442-289">Pokud je *v* systému jednoznačně přiřazen na konci *výrazu expr*, pak je jednoznačně přiřazen k přenosu toku řízení do koncového bodu *stmt*.</span><span class="sxs-lookup"><span data-stu-id="1a442-289">If *v* is definitely assigned at the end of *expr*, then it is definitely assigned on the control flow transfer to the end point of *stmt*.</span></span>
*  <span data-ttu-id="1a442-290">Pokud *v* má stav "po nepravdivém výrazu" po nepravdivém výrazu "na *konci výrazu", pak*je jednoznačně přiřazen k přenosu toku řízení do koncového bodu *stmt*.</span><span class="sxs-lookup"><span data-stu-id="1a442-290">If *v* has the state "definitely assigned after false expression" at the end of *expr*, then it is definitely assigned on the control flow transfer to the end point of *stmt*.</span></span>

#### <a name="for-statements"></a><span data-ttu-id="1a442-291">Příkazy for</span><span class="sxs-lookup"><span data-stu-id="1a442-291">For statements</span></span>

<span data-ttu-id="1a442-292">Kontrola jednoznačného přiřazení pro `for` příkaz formuláře:</span><span class="sxs-lookup"><span data-stu-id="1a442-292">Definite assignment checking for a `for` statement of the form:</span></span>
```csharp
for ( for_initializer ; for_condition ; for_iterator ) embedded_statement
```
<span data-ttu-id="1a442-293">je proveden jako při zápisu příkazu:</span><span class="sxs-lookup"><span data-stu-id="1a442-293">is done as if the statement were written:</span></span>
```csharp
{
    for_initializer ;
    while ( for_condition ) {
        embedded_statement ;
        for_iterator ;
    }
}
```

<span data-ttu-id="1a442-294">Pokud se *for_condition* z `for` příkazu vynechá, vyhodnocování jednoznačného přiřazení pokračuje, jako kdyby se `true` for_condition nahradilo ve výše uvedeném rozšíření.</span><span class="sxs-lookup"><span data-stu-id="1a442-294">If the *for_condition* is omitted from the `for` statement, then evaluation of definite assignment proceeds as if *for_condition* were replaced with `true` in the above expansion.</span></span>

#### <a name="break-continue-and-goto-statements"></a><span data-ttu-id="1a442-295">Příkazy Break, Continue a goto</span><span class="sxs-lookup"><span data-stu-id="1a442-295">Break, continue, and goto statements</span></span>

<span data-ttu-id="1a442-296">Určitý stav přiřazení *v* v na přenosu toku řízení `break`, který je způsoben příkazem, nebo `goto` , `continue`je stejný jako jednoznačný stav přiřazení *v* na začátku příkazu.</span><span class="sxs-lookup"><span data-stu-id="1a442-296">The definite assignment state of *v* on the control flow transfer caused by a `break`, `continue`, or `goto` statement is the same as the definite assignment state of *v* at the beginning of the statement.</span></span>

#### <a name="throw-statements"></a><span data-ttu-id="1a442-297">Příkazy throw</span><span class="sxs-lookup"><span data-stu-id="1a442-297">Throw statements</span></span>

<span data-ttu-id="1a442-298">Pro příkaz *stmt* ve formuláři</span><span class="sxs-lookup"><span data-stu-id="1a442-298">For a statement *stmt* of the form</span></span>
```csharp
throw expr ;
```

<span data-ttu-id="1a442-299">Určitý stav přiřazení *v v* na začátku *výrazu* je stejný jako jednoznačný stav přiřazení *v* na začátku *stmt*.</span><span class="sxs-lookup"><span data-stu-id="1a442-299">The definite assignment state of *v* at the beginning of *expr* is the same as the definite assignment state of *v* at the beginning of *stmt*.</span></span>

#### <a name="return-statements"></a><span data-ttu-id="1a442-300">Příkazy Return</span><span class="sxs-lookup"><span data-stu-id="1a442-300">Return statements</span></span>

<span data-ttu-id="1a442-301">Pro příkaz *stmt* ve formuláři</span><span class="sxs-lookup"><span data-stu-id="1a442-301">For a statement *stmt* of the form</span></span>
```csharp
return expr ;
```

*  <span data-ttu-id="1a442-302">Určitý stav přiřazení *v v* na začátku *výrazu* je stejný jako jednoznačný stav přiřazení *v* na začátku *stmt*.</span><span class="sxs-lookup"><span data-stu-id="1a442-302">The definite assignment state of *v* at the beginning of *expr* is the same as the definite assignment state of *v* at the beginning of *stmt*.</span></span>
*  <span data-ttu-id="1a442-303">Pokud *v* je výstupní parametr, musí být jednoznačně přiřazen buď:</span><span class="sxs-lookup"><span data-stu-id="1a442-303">If *v* is an output parameter, then it must be definitely assigned either:</span></span>
    * <span data-ttu-id="1a442-304">Po *výrazu*</span><span class="sxs-lookup"><span data-stu-id="1a442-304">after *expr*</span></span>
    * <span data-ttu-id="1a442-305">nebo na `finally` konci bloku `try` - nebo`try` ,který`return` uzavře příkaz. - `finally` `catch` - `finally`</span><span class="sxs-lookup"><span data-stu-id="1a442-305">or at the end of the `finally` block of a `try`-`finally` or `try`-`catch`-`finally` that encloses the `return` statement.</span></span>

<span data-ttu-id="1a442-306">Pro příkaz stmt ve formátu:</span><span class="sxs-lookup"><span data-stu-id="1a442-306">For a statement stmt of the form:</span></span>
```csharp
return ;
```

*  <span data-ttu-id="1a442-307">Pokud *v* je výstupní parametr, musí být jednoznačně přiřazen buď:</span><span class="sxs-lookup"><span data-stu-id="1a442-307">If *v* is an output parameter, then it must be definitely assigned either:</span></span>
    * <span data-ttu-id="1a442-308">před *stmt*</span><span class="sxs-lookup"><span data-stu-id="1a442-308">before *stmt*</span></span>
    * <span data-ttu-id="1a442-309">nebo na `finally` konci bloku `try` - nebo`try` ,který`return` uzavře příkaz. - `finally` `catch` - `finally`</span><span class="sxs-lookup"><span data-stu-id="1a442-309">or at the end of the `finally` block of a `try`-`finally` or `try`-`catch`-`finally` that encloses the `return` statement.</span></span>

#### <a name="try-catch-statements"></a><span data-ttu-id="1a442-310">Příkazy try-catch</span><span class="sxs-lookup"><span data-stu-id="1a442-310">Try-catch statements</span></span>

<span data-ttu-id="1a442-311">Pro příkaz *stmt* ve formátu:</span><span class="sxs-lookup"><span data-stu-id="1a442-311">For a statement *stmt* of the form:</span></span>
```csharp
try try_block
catch(...) catch_block_1
...
catch(...) catch_block_n
```

*  <span data-ttu-id="1a442-312">Určitý stav přiřazení *v v* na začátku *try_block* je stejný jako jednoznačný stav přiřazení *v* na začátku *stmt*.</span><span class="sxs-lookup"><span data-stu-id="1a442-312">The definite assignment state of *v* at the beginning of *try_block* is the same as the definite assignment state of *v* at the beginning of *stmt*.</span></span>
*  <span data-ttu-id="1a442-313">Stanovený stav přiřazení *v* v na začátku *catch_block_i* (pro libovolný *i*) je stejný jako jednoznačný stav přiřazení *v* na začátku *stmt*.</span><span class="sxs-lookup"><span data-stu-id="1a442-313">The definite assignment state of *v* at the beginning of *catch_block_i* (for any *i*) is the same as the definite assignment state of *v* at the beginning of *stmt*.</span></span>
*  <span data-ttu-id="1a442-314">Určitý stav přiřazení *v* v koncových bodech *stmt* je jednoznačně přiřazený, pokud (a pouze pokud) *v* se jednoznačně přiřazují v koncovém bodě *try_block* a každé *catch_block_i* (pro každý z 1 do *n.* ).</span><span class="sxs-lookup"><span data-stu-id="1a442-314">The definite assignment state of *v* at the end-point of *stmt* is definitely assigned if (and only if) *v* is definitely assigned at the end-point of *try_block* and every *catch_block_i* (for every *i* from 1 to *n*).</span></span>

#### <a name="try-finally-statements"></a><span data-ttu-id="1a442-315">Try-finally – příkazy</span><span class="sxs-lookup"><span data-stu-id="1a442-315">Try-finally statements</span></span>

<span data-ttu-id="1a442-316">Pro příkaz stmt ve formátu: `try`</span><span class="sxs-lookup"><span data-stu-id="1a442-316">For a `try` statement *stmt* of the form:</span></span>
```csharp
try try_block finally finally_block
```

*  <span data-ttu-id="1a442-317">Určitý stav přiřazení *v v* na začátku *try_block* je stejný jako jednoznačný stav přiřazení *v* na začátku *stmt*.</span><span class="sxs-lookup"><span data-stu-id="1a442-317">The definite assignment state of *v* at the beginning of *try_block* is the same as the definite assignment state of *v* at the beginning of *stmt*.</span></span>
*  <span data-ttu-id="1a442-318">Určitý stav přiřazení *v v* na začátku *finally_block* je stejný jako jednoznačný stav přiřazení *v* na začátku *stmt*.</span><span class="sxs-lookup"><span data-stu-id="1a442-318">The definite assignment state of *v* at the beginning of *finally_block* is the same as the definite assignment state of *v* at the beginning of *stmt*.</span></span>
*  <span data-ttu-id="1a442-319">Určitý stav přiřazení *v* v koncovém bodě *stmt* je jednoznačně přiřazený, pokud (a pouze pokud) alespoň jedna z následujících možností:</span><span class="sxs-lookup"><span data-stu-id="1a442-319">The definite assignment state of *v* at the end-point of *stmt* is definitely assigned if (and only if) at least one of the following is true:</span></span>
    * <span data-ttu-id="1a442-320">*v* je jednoznačně přiřazený na koncový bod *try_block* .</span><span class="sxs-lookup"><span data-stu-id="1a442-320">*v* is definitely assigned at the end-point of *try_block*</span></span>
    * <span data-ttu-id="1a442-321">*v* je jednoznačně přiřazený na koncový bod *finally_block* .</span><span class="sxs-lookup"><span data-stu-id="1a442-321">*v* is definitely assigned at the end-point of *finally_block*</span></span>

<span data-ttu-id="1a442-322">Pokud je `goto` proveden přenos toku řízení (například příkaz), který začíná v *try_block*a končí mimo *try_block*, pak v je také v případě, že je v rámci tohoto přenosu toku řízení, *v* případě jednoznačně přiřazené k koncovému bodu *finally_block*.</span><span class="sxs-lookup"><span data-stu-id="1a442-322">If a control flow transfer (for example, a `goto` statement) is made that begins within *try_block*, and ends outside of *try_block*, then *v* is also considered definitely assigned on that control flow transfer if *v* is definitely assigned at the end-point of *finally_block*.</span></span> <span data-ttu-id="1a442-323">(Nejedná se o pouze v případě, že – pokud je *v* systému jednoznačně přiřazen jiný důvod pro přenos toku řízení, pak je stále považován za jednoznačně přiřazený.)</span><span class="sxs-lookup"><span data-stu-id="1a442-323">(This is not an only if—if *v* is definitely assigned for another reason on this control flow transfer, then it is still considered definitely assigned.)</span></span>

#### <a name="try-catch-finally-statements"></a><span data-ttu-id="1a442-324">Try-catch-finally – příkazy</span><span class="sxs-lookup"><span data-stu-id="1a442-324">Try-catch-finally statements</span></span>

<span data-ttu-id="1a442-325">Analýza jednoznačného přiřazení pro `try` - `catch` příkazformuláře-: `finally`</span><span class="sxs-lookup"><span data-stu-id="1a442-325">Definite assignment analysis for a `try`-`catch`-`finally` statement of the form:</span></span>
```csharp
try try_block
catch(...) catch_block_1
...
catch(...) catch_block_n
finally *finally_block*
```
<span data-ttu-id="1a442-326">je proveden, jako kdyby byl příkaz `try` `finally` - příkazem ohraničujícím `try` - `catch` příkaz:</span><span class="sxs-lookup"><span data-stu-id="1a442-326">is done as if the statement were a `try`-`finally` statement enclosing a `try`-`catch` statement:</span></span>
```csharp
try {
    try try_block
    catch(...) catch_block_1
    ...
    catch(...) catch_block_n
}
finally finally_block
```

<span data-ttu-id="1a442-327">Následující příklad ukazuje, jak různé bloky `try` příkazu ([příkaz try](statements.md#the-try-statement)) ovlivňují jednoznačné přiřazení.</span><span class="sxs-lookup"><span data-stu-id="1a442-327">The following example demonstrates how the different blocks of a `try` statement ([The try statement](statements.md#the-try-statement)) affect definite assignment.</span></span>
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

#### <a name="foreach-statements"></a><span data-ttu-id="1a442-328">Příkazy foreach</span><span class="sxs-lookup"><span data-stu-id="1a442-328">Foreach statements</span></span>

<span data-ttu-id="1a442-329">Pro příkaz stmt ve formátu: `foreach`</span><span class="sxs-lookup"><span data-stu-id="1a442-329">For a `foreach` statement *stmt* of the form:</span></span>
```csharp
foreach ( type identifier in expr ) embedded_statement
```

*  <span data-ttu-id="1a442-330">Určitý stav přiřazení *v v* na začátku *výrazu* je stejný jako stav *v* na začátku *stmt*.</span><span class="sxs-lookup"><span data-stu-id="1a442-330">The definite assignment state of *v* at the beginning of *expr* is the same as the state of *v* at the beginning of *stmt*.</span></span>
*  <span data-ttu-id="1a442-331">Určitý stav přiřazení *v* v v případě přenosů toku řízení do *embedded_statement* nebo na koncový bod *stmt* je stejný jako stav *v* elementu na konci *výrazu expr*.</span><span class="sxs-lookup"><span data-stu-id="1a442-331">The definite assignment state of *v* on the control flow transfer to *embedded_statement* or to the end point of *stmt* is the same as the state of *v* at the end of *expr*.</span></span>

#### <a name="using-statements"></a><span data-ttu-id="1a442-332">Příkazy using</span><span class="sxs-lookup"><span data-stu-id="1a442-332">Using statements</span></span>

<span data-ttu-id="1a442-333">Pro příkaz stmt ve formátu: `using`</span><span class="sxs-lookup"><span data-stu-id="1a442-333">For a `using` statement *stmt* of the form:</span></span>
```csharp
using ( resource_acquisition ) embedded_statement
```

*  <span data-ttu-id="1a442-334">Určitý stav přiřazení *v v* na začátku *resource_acquisition* je stejný jako stav *v* v na začátku *stmt*.</span><span class="sxs-lookup"><span data-stu-id="1a442-334">The definite assignment state of *v* at the beginning of *resource_acquisition* is the same as the state of *v* at the beginning of *stmt*.</span></span>
*  <span data-ttu-id="1a442-335">Určitý stav přiřazení *v* v v případě přenosu toku řízení do *embedded_statement* je stejný jako stav *v* v na konci *resource_acquisition*.</span><span class="sxs-lookup"><span data-stu-id="1a442-335">The definite assignment state of *v* on the control flow transfer to *embedded_statement* is the same as the state of *v* at the end of *resource_acquisition*.</span></span>

#### <a name="lock-statements"></a><span data-ttu-id="1a442-336">Příkazy LOCK</span><span class="sxs-lookup"><span data-stu-id="1a442-336">Lock statements</span></span>

<span data-ttu-id="1a442-337">Pro příkaz stmt ve formátu: `lock`</span><span class="sxs-lookup"><span data-stu-id="1a442-337">For a `lock` statement *stmt* of the form:</span></span>
```csharp
lock ( expr ) embedded_statement
```

*  <span data-ttu-id="1a442-338">Určitý stav přiřazení *v v* na začátku *výrazu* je stejný jako stav *v* na začátku *stmt*.</span><span class="sxs-lookup"><span data-stu-id="1a442-338">The definite assignment state of *v* at the beginning of *expr* is the same as the state of *v* at the beginning of *stmt*.</span></span>
*  <span data-ttu-id="1a442-339">Určitý stav přiřazení *v* v v případě přenosu toku řízení do *embedded_statement* je stejný jako stav *v* elementu na konci *výrazu expr*.</span><span class="sxs-lookup"><span data-stu-id="1a442-339">The definite assignment state of *v* on the control flow transfer to *embedded_statement* is the same as the state of *v* at the end of *expr*.</span></span>

#### <a name="yield-statements"></a><span data-ttu-id="1a442-340">Příkazy yield</span><span class="sxs-lookup"><span data-stu-id="1a442-340">Yield statements</span></span>

<span data-ttu-id="1a442-341">Pro příkaz stmt ve formátu: `yield return`</span><span class="sxs-lookup"><span data-stu-id="1a442-341">For a `yield return` statement *stmt* of the form:</span></span>
```csharp
yield return expr ;
```

*  <span data-ttu-id="1a442-342">Určitý stav přiřazení *v v* na začátku *výrazu* je stejný jako stav *v* na začátku *stmt*.</span><span class="sxs-lookup"><span data-stu-id="1a442-342">The definite assignment state of *v* at the beginning of *expr* is the same as the state of *v* at the beginning of *stmt*.</span></span>
*  <span data-ttu-id="1a442-343">Jednoznačný stav přiřazení *v v* na konci *stmt* je stejný jako stav *v* elementu na konci *výrazu expr*.</span><span class="sxs-lookup"><span data-stu-id="1a442-343">The definite assignment state of *v* at the end of *stmt* is the same as the state of *v* at the end of *expr*.</span></span>
*  <span data-ttu-id="1a442-344">`yield break` Příkaz nemá žádný vliv na určitý stav přiřazení.</span><span class="sxs-lookup"><span data-stu-id="1a442-344">A `yield break` statement has no effect on the definite assignment state.</span></span>

#### <a name="general-rules-for-simple-expressions"></a><span data-ttu-id="1a442-345">Obecná pravidla pro jednoduché výrazy</span><span class="sxs-lookup"><span data-stu-id="1a442-345">General rules for simple expressions</span></span>

<span data-ttu-id="1a442-346">Následující pravidlo se vztahuje na tyto druhy výrazů: literály ([literály](expressions.md#literals)), jednoduché názvy ([jednoduché názvy](expressions.md#simple-names)), výrazy přístupu členů ([přístup členů](expressions.md#member-access)), neindexovaných základních výrazů přístupu ([základní přístup](expressions.md#base-access)), `typeof`výrazy ([operátor typeof](expressions.md#the-typeof-operator)), výchozí hodnoty výrazy ([výrazy výchozích hodnot](expressions.md#default-value-expressions)) a `nameof` výrazy ([výrazy nameof](expressions.md#nameof-expressions)).</span><span class="sxs-lookup"><span data-stu-id="1a442-346">The following rule applies to these kinds of expressions: literals ([Literals](expressions.md#literals)), simple names ([Simple names](expressions.md#simple-names)), member access expressions ([Member access](expressions.md#member-access)), non-indexed base access expressions ([Base access](expressions.md#base-access)), `typeof` expressions ([The typeof operator](expressions.md#the-typeof-operator)), default value expressions ([Default value expressions](expressions.md#default-value-expressions)) and `nameof` expressions ([Nameof expressions](expressions.md#nameof-expressions)).</span></span>

*  <span data-ttu-id="1a442-347">Absolutní stav přiřazení *v* rámci na konci takového výrazu je stejný jako jednoznačný stav přiřazení *v* na začátku výrazu.</span><span class="sxs-lookup"><span data-stu-id="1a442-347">The definite assignment state of *v* at the end of such an expression is the same as the definite assignment state of *v* at the beginning of the expression.</span></span>

#### <a name="general-rules-for-expressions-with-embedded-expressions"></a><span data-ttu-id="1a442-348">Obecná pravidla pro výrazy s vloženými výrazy</span><span class="sxs-lookup"><span data-stu-id="1a442-348">General rules for expressions with embedded expressions</span></span>

<span data-ttu-id="1a442-349">Následující pravidla se vztahují na tyto typy výrazů: výrazy v závorkách ([výrazy v závorkách](expressions.md#parenthesized-expressions)), výrazy přístupu k prvkům ([přístup k prvkům](expressions.md#element-access)), základní výrazy přístupu s indexováním ([základní přístup](expressions.md#base-access)), zvýšení a snížení výrazů ([operátory přírůstku a snížení přípony](expressions.md#postfix-increment-and-decrement-operators), [operátory přírůstku a snížení předpony](expressions.md#prefix-increment-and-decrement-operators)), výrazy přetypování `+`( `-`[výrazy přetypování](expressions.md#cast-expressions)), Unární,, `~`, `*`výrazy, binární `+` `-` ,`*`, ,,`%`,,,, ,`>`,, `>=` `/` `<<` `>>` `<` `<=` `==`, ,`is`,, ,`&` [](expressions.md#shift-operators)[](expressions.md#arithmetic-operators), výrazy(aritmetickéoperátory,operátory`^` posunutí, relační testování a testování typu `|` `!=` `as` [ operátory](expressions.md#relational-and-type-testing-operators), [logické operátory](expressions.md#logical-operators)), výrazy složeného přiřazení ( `checked` [složené přiřazení](expressions.md#compound-assignment)) a `unchecked` výrazy ([kontrolované a nezaškrtnuté operátory](expressions.md#the-checked-and-unchecked-operators)), plus pole a delegát výrazy vytváření ([operátor New](expressions.md#the-new-operator)).</span><span class="sxs-lookup"><span data-stu-id="1a442-349">The following rules apply to these kinds of expressions: parenthesized expressions ([Parenthesized expressions](expressions.md#parenthesized-expressions)), element access expressions ([Element access](expressions.md#element-access)), base access expressions with indexing ([Base access](expressions.md#base-access)), increment and decrement expressions ([Postfix increment and decrement operators](expressions.md#postfix-increment-and-decrement-operators), [Prefix increment and decrement operators](expressions.md#prefix-increment-and-decrement-operators)), cast expressions ([Cast expressions](expressions.md#cast-expressions)), unary `+`, `-`, `~`, `*` expressions, binary `+`, `-`, `*`, `/`, `%`, `<<`, `>>`, `<`, `<=`, `>`, `>=`, `==`, `!=`, `is`, `as`, `&`, `|`, `^` expressions ([Arithmetic operators](expressions.md#arithmetic-operators), [Shift operators](expressions.md#shift-operators), [Relational and type-testing operators](expressions.md#relational-and-type-testing-operators), [Logical operators](expressions.md#logical-operators)), compound assignment expressions ([Compound assignment](expressions.md#compound-assignment)), `checked` and `unchecked` expressions ([The checked and unchecked operators](expressions.md#the-checked-and-unchecked-operators)), plus array and delegate creation expressions ([The new operator](expressions.md#the-new-operator)).</span></span>

<span data-ttu-id="1a442-350">Každý z těchto výrazů má jeden nebo více dílčích výrazů, které jsou bezpodmínečně vyhodnoceny v pevně uvedeném pořadí.</span><span class="sxs-lookup"><span data-stu-id="1a442-350">Each of these expressions has one or more sub-expressions that are unconditionally evaluated in a fixed order.</span></span> <span data-ttu-id="1a442-351">Například binární `%` operátor vyhodnocuje levou stranu operátora a pak pravou stranu.</span><span class="sxs-lookup"><span data-stu-id="1a442-351">For example, the binary `%` operator evaluates the left hand side of the operator, then the right hand side.</span></span> <span data-ttu-id="1a442-352">Operace indexování vyhodnotí indexovaný výraz a pak vyhodnotí všechny indexové výrazy v pořadí zleva doprava.</span><span class="sxs-lookup"><span data-stu-id="1a442-352">An indexing operation evaluates the indexed expression, and then evaluates each of the index expressions, in order from left to right.</span></span> <span data-ttu-id="1a442-353">Výraz *expr*, který má dílčí výrazy *E1, E2,..., EN*, vyhodnocený v tomto pořadí:</span><span class="sxs-lookup"><span data-stu-id="1a442-353">For an expression *expr*, which has sub-expressions *e1, e2, ..., eN*, evaluated in that order:</span></span>

*  <span data-ttu-id="1a442-354">Určitý stav přiřazení *v v* na začátku *E1* je stejný jako určitý stav přiřazení na začátku *výrazu expr*.</span><span class="sxs-lookup"><span data-stu-id="1a442-354">The definite assignment state of *v* at the beginning of *e1* is the same as the definite assignment state at the beginning of *expr*.</span></span>
*  <span data-ttu-id="1a442-355">Stanovený stav přiřazení *v v* na začátku *EI* (*i* větší než jeden) je stejný jako u jednoznačného stavu přiřazení na konci předchozího dílčího výrazu.</span><span class="sxs-lookup"><span data-stu-id="1a442-355">The definite assignment state of *v* at the beginning of *ei* (*i* greater than one) is the same as the definite assignment state at the end of the previous sub-expression.</span></span>
*  <span data-ttu-id="1a442-356">Určitý stav přiřazení *v v* na konci *výrazu* je stejný jako jednoznačný stav přiřazení na konci *EN*</span><span class="sxs-lookup"><span data-stu-id="1a442-356">The definite assignment state of *v* at the end of *expr* is the same as the definite assignment state at the end of *eN*</span></span>

#### <a name="invocation-expressions-and-object-creation-expressions"></a><span data-ttu-id="1a442-357">Výrazy vyvolání a výrazy pro vytváření objektů</span><span class="sxs-lookup"><span data-stu-id="1a442-357">Invocation expressions and object creation expressions</span></span>

<span data-ttu-id="1a442-358">*Výraz výrazu vyvolání* formuláře:</span><span class="sxs-lookup"><span data-stu-id="1a442-358">For an invocation expression *expr* of the form:</span></span>
```csharp
primary_expression ( arg1 , arg2 , ... , argN )
```
<span data-ttu-id="1a442-359">nebo výraz pro vytvoření objektu ve formátu:</span><span class="sxs-lookup"><span data-stu-id="1a442-359">or an object creation expression of the form:</span></span>
```csharp
new type ( arg1 , arg2 , ... , argN )
```

*  <span data-ttu-id="1a442-360">Pro výraz vyvolání je jednoznačný stav přiřazení *v v* před *primary_expression* shodný se stavem *v* před *výrazem*.</span><span class="sxs-lookup"><span data-stu-id="1a442-360">For an invocation expression, the definite assignment state of *v* before *primary_expression* is the same as the state of *v* before *expr*.</span></span>
*  <span data-ttu-id="1a442-361">U výrazu vyvolání je jednoznačný stav přiřazení *v v* před *arg1* stejný jako stav *v v* po *primary_expression*.</span><span class="sxs-lookup"><span data-stu-id="1a442-361">For an invocation expression, the definite assignment state of *v* before *arg1* is the same as the state of *v* after *primary_expression*.</span></span>
*  <span data-ttu-id="1a442-362">V případě výrazu pro vytvoření objektu je jednoznačný stav přiřazení *v v* před *arg1* stejný jako stav *v* před *výrazem*.</span><span class="sxs-lookup"><span data-stu-id="1a442-362">For an object creation expression, the definite assignment state of *v* before *arg1* is the same as the state of *v* before *expr*.</span></span>
*  <span data-ttu-id="1a442-363">U každého argumentu *Argi*je stanovený jednoznačný stav přiřazení *v v* po *Argi* pravidla pro normální výrazy, přičemž se ignorují `ref` všechny `out` nebo modifikátory.</span><span class="sxs-lookup"><span data-stu-id="1a442-363">For each argument *argi*, the definite assignment state of *v* after *argi* is determined by the normal expression rules, ignoring any `ref` or `out` modifiers.</span></span>
*  <span data-ttu-id="1a442-364">U každého argumentu *Argi* pro libovolný *i* větší než jeden je určený stav přiřazení *v* před *Argi* stejný jako stav *v* za předchozí *arg*.</span><span class="sxs-lookup"><span data-stu-id="1a442-364">For each argument *argi* for any *i* greater than one, the definite assignment state of *v* before *argi* is the same as the state of *v* after the previous *arg*.</span></span>
*  <span data-ttu-id="1a442-365">Pokud je proměnná *v* předána jako `out` argument (tj. argument formuláře `out v`) v některém z argumentů, pak je stav *v v* za *výraz* jednoznačně přiřazen.</span><span class="sxs-lookup"><span data-stu-id="1a442-365">If the variable *v* is passed as an `out` argument (i.e., an argument of the form `out v`) in any of the arguments, then the state of *v* after *expr* is definitely assigned.</span></span> <span data-ttu-id="1a442-366">Případech stav *v v* po *výrazu expr* je stejný jako stav *v* za po *argn*.</span><span class="sxs-lookup"><span data-stu-id="1a442-366">Otherwise; the state of *v* after *expr* is the same as the state of *v* after *argN*.</span></span>
*  <span data-ttu-id="1a442-367">Pro Inicializátory pole ([výrazy vytváření pole](expressions.md#array-creation-expressions)), Inicializátory objektů ([Inicializátory objektů](expressions.md#object-initializers)), inicializátory kolekce ([inicializátory kolekce](expressions.md#collection-initializers)) a Inicializátory anonymních objektů ([vytváření anonymních objektů výrazy](expressions.md#anonymous-object-creation-expressions)), jednoznačný stav přiřazení je určen rozšířením, které jsou definovány ve smyslu.</span><span class="sxs-lookup"><span data-stu-id="1a442-367">For array initializers ([Array creation expressions](expressions.md#array-creation-expressions)), object initializers ([Object initializers](expressions.md#object-initializers)), collection initializers ([Collection initializers](expressions.md#collection-initializers)) and anonymous object initializers ([Anonymous object creation expressions](expressions.md#anonymous-object-creation-expressions)), the definite assignment state is determined by the expansion that these constructs are defined in terms of.</span></span>

#### <a name="simple-assignment-expressions"></a><span data-ttu-id="1a442-368">Výrazy jednoduchého přiřazení</span><span class="sxs-lookup"><span data-stu-id="1a442-368">Simple assignment expressions</span></span>

<span data-ttu-id="1a442-369">Výraz *expr* formuláře `w = expr_rhs`:</span><span class="sxs-lookup"><span data-stu-id="1a442-369">For an expression *expr* of the form `w = expr_rhs`:</span></span>

*  <span data-ttu-id="1a442-370">Určitý stav přiřazení *v* rámci před *expr_rhs* je stejný jako jednoznačný stav přiřazení *v* před *výrazem*.</span><span class="sxs-lookup"><span data-stu-id="1a442-370">The definite assignment state of *v* before *expr_rhs* is the same as the definite assignment state of *v* before *expr*.</span></span>
*  <span data-ttu-id="1a442-371">Jednoznačný stav přiřazení *v v* po *výrazu* určuje:</span><span class="sxs-lookup"><span data-stu-id="1a442-371">The definite assignment state of *v* after *expr* is determined by:</span></span>
   * <span data-ttu-id="1a442-372">Je-li *w* stejná proměnná jako *v*, pak je jednoznačný stav přiřazení *v v* After *expr* jednoznačně přiřazen.</span><span class="sxs-lookup"><span data-stu-id="1a442-372">If *w* is the same variable as *v*, then the definite assignment state of *v* after *expr* is definitely assigned.</span></span>
   * <span data-ttu-id="1a442-373">V opačném případě, pokud je přiřazení provedeno v rámci konstruktoru instance typu struktury, pokud je *w* přístup k vlastnosti určení automaticky implementované vlastnosti *P* na instanci, kterou vytváříte, a *v* je skryté pole zálohování *P*, pak bude jednoznačný stav přiřazení *v v* After *expr* jednoznačně přiřazen.</span><span class="sxs-lookup"><span data-stu-id="1a442-373">Otherwise, if the assignment occurs within the instance constructor of a struct type, if *w* is a property access designating an automatically implemented property *P* on the instance being constructed and *v* is the hidden backing field of *P*, then the definite assignment state of *v* after *expr* is definitely assigned.</span></span>
   * <span data-ttu-id="1a442-374">V opačném případě bude jednoznačný stav přiřazení *v v* After *expr* stejný jako jednoznačný stav přiřazení *v* po *expr_rhs*.</span><span class="sxs-lookup"><span data-stu-id="1a442-374">Otherwise, the definite assignment state of *v* after *expr* is the same as the definite assignment state of *v* after *expr_rhs*.</span></span>

#### <a name="-conditional-and-expressions"></a><span data-ttu-id="1a442-375">& & (podmíněné a) výrazy</span><span class="sxs-lookup"><span data-stu-id="1a442-375">&& (conditional AND) expressions</span></span>

<span data-ttu-id="1a442-376">Výraz *expr* formuláře `expr_first && expr_second`:</span><span class="sxs-lookup"><span data-stu-id="1a442-376">For an expression *expr* of the form `expr_first && expr_second`:</span></span>

*  <span data-ttu-id="1a442-377">Určitý stav přiřazení *v* rámci před *expr_first* je stejný jako jednoznačný stav přiřazení *v* před *výrazem*.</span><span class="sxs-lookup"><span data-stu-id="1a442-377">The definite assignment state of *v* before *expr_first* is the same as the definite assignment state of *v* before *expr*.</span></span>
*  <span data-ttu-id="1a442-378">Skutečný stav přiřazení *v v* před *expr_second* je jednoznačně přiřazený, pokud je stav *v v* po *expr_first* buď jednoznačně přiřazen, nebo "jednoznačně přiřazeno po výrazu true".</span><span class="sxs-lookup"><span data-stu-id="1a442-378">The definite assignment state of *v* before *expr_second* is definitely assigned if the state of *v* after *expr_first* is either definitely assigned or "definitely assigned after true expression".</span></span> <span data-ttu-id="1a442-379">V opačném případě se nepřiřazují jako neomezeně.</span><span class="sxs-lookup"><span data-stu-id="1a442-379">Otherwise, it is not definitely assigned.</span></span>
*  <span data-ttu-id="1a442-380">Jednoznačný stav přiřazení *v v* po *výrazu* určuje:</span><span class="sxs-lookup"><span data-stu-id="1a442-380">The definite assignment state of *v* after *expr* is determined by:</span></span>
    * <span data-ttu-id="1a442-381">Pokud *expr_first* `false`je konstantní výraz s hodnotou, pak bude mít jednoznačný stav přiřazení *v v* After *expr* stejný jako pro určitý stav přiřazení *v* za za *expr_first*.</span><span class="sxs-lookup"><span data-stu-id="1a442-381">If *expr_first* is a constant expression with the value `false`, then the definite assignment state of *v* after *expr* is the same as the definite assignment state of *v* after *expr_first*.</span></span>
    * <span data-ttu-id="1a442-382">V opačném případě, pokud je stav *v v* po *expr_first* přiřazení jednoznačně přiřazen, je stav *v v* po jednoznačně přiřazeném *výrazu* .</span><span class="sxs-lookup"><span data-stu-id="1a442-382">Otherwise, if the state of *v* after *expr_first* is definitely assigned, then the state of *v* after *expr* is definitely assigned.</span></span>
    * <span data-ttu-id="1a442-383">V opačném případě platí, že pokud je stav *v v* po *expr_secondně* přiřazený, a stav *v* po po *expr_first* je "jednoznačně přiřazený po nepravdivém výrazu", pak je stav *v v* After *expr* jednoznačně přiřazení.</span><span class="sxs-lookup"><span data-stu-id="1a442-383">Otherwise, if the state of *v* after *expr_second* is definitely assigned, and the state of *v* after *expr_first* is "definitely assigned after false expression", then the state of *v* after *expr* is definitely assigned.</span></span>
    * <span data-ttu-id="1a442-384">V opačném případě platí, že *Pokud je stav* *v v* po *expr_second* jednoznačně přiřazený nebo "jednoznačně přiřazený po hodnotě true", pak je stav *v* po výrazu "jednoznačně přiřazený po výrazu true".</span><span class="sxs-lookup"><span data-stu-id="1a442-384">Otherwise, if the state of *v* after *expr_second* is definitely assigned or "definitely assigned after true expression", then the state of *v* after *expr* is "definitely assigned after true expression".</span></span>
    * <span data-ttu-id="1a442-385">V opačném případě, pokud je stav *v* po po *expr_first* "jednoznačně přiřazený po nepravdivém výrazu", a stav *v v* After *expr_second* je "jednoznačně přiřazený po nepravdivém výrazu", pak stav *v* za  *výraz* je "jednoznačně přiřazený po nepravdivém výrazu".</span><span class="sxs-lookup"><span data-stu-id="1a442-385">Otherwise, if the state of *v* after *expr_first* is "definitely assigned after false expression", and the state of *v* after *expr_second* is "definitely assigned after false expression", then the state of *v* after *expr* is "definitely assigned after false expression".</span></span>
    * <span data-ttu-id="1a442-386">V opačném případě stav *v* případě, kdy *výraz* není jednoznačně přiřazen.</span><span class="sxs-lookup"><span data-stu-id="1a442-386">Otherwise, the state of *v* after *expr* is not definitely assigned.</span></span>

<span data-ttu-id="1a442-387">V příkladu</span><span class="sxs-lookup"><span data-stu-id="1a442-387">In the example</span></span>
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
<span data-ttu-id="1a442-388">proměnná `i` se považuje za jednoznačně přiřazenou v jednom z vložených příkazů `if` příkazu, ale ne v druhé.</span><span class="sxs-lookup"><span data-stu-id="1a442-388">the variable `i` is considered definitely assigned in one of the embedded statements of an `if` statement but not in the other.</span></span> <span data-ttu-id="1a442-389">V příkazu v metodě `F`je proměnná `i` jednoznačně přiřazena v prvním vloženém příkazu, protože provádění výrazu `(i = y)` vždy předchází provedení tohoto vloženého příkazu. `if`</span><span class="sxs-lookup"><span data-stu-id="1a442-389">In the `if` statement in method `F`, the variable `i` is definitely assigned in the first embedded statement because execution of the expression `(i = y)` always precedes execution of this embedded statement.</span></span> <span data-ttu-id="1a442-390">Naopak proměnná `i` není jednoznačně přiřazena v druhém vloženém příkazu, protože `x >= 0` by mohla být testována na false, výsledkem je, že proměnná `i` je Nepřiřazená.</span><span class="sxs-lookup"><span data-stu-id="1a442-390">In contrast, the variable `i` is not definitely assigned in the second embedded statement, since `x >= 0` might have tested false, resulting in the variable `i` being unassigned.</span></span>

#### <a name="-conditional-or-expressions"></a><span data-ttu-id="1a442-391">|| (podmíněné nebo) výrazy</span><span class="sxs-lookup"><span data-stu-id="1a442-391">|| (conditional OR) expressions</span></span>

<span data-ttu-id="1a442-392">Výraz *expr* formuláře `expr_first || expr_second`:</span><span class="sxs-lookup"><span data-stu-id="1a442-392">For an expression *expr* of the form `expr_first || expr_second`:</span></span>

*  <span data-ttu-id="1a442-393">Určitý stav přiřazení *v* rámci před *expr_first* je stejný jako jednoznačný stav přiřazení *v* před *výrazem*.</span><span class="sxs-lookup"><span data-stu-id="1a442-393">The definite assignment state of *v* before *expr_first* is the same as the definite assignment state of *v* before *expr*.</span></span>
*  <span data-ttu-id="1a442-394">Určitý stav přiřazení *v v* před *expr_second* je jednoznačně přiřazený, pokud je stav *v v* po *expr_first* buď jednoznačně přiřazen, nebo "jednoznačně přiřazený po false výrazu".</span><span class="sxs-lookup"><span data-stu-id="1a442-394">The definite assignment state of *v* before *expr_second* is definitely assigned if the state of *v* after *expr_first* is either definitely assigned or "definitely assigned after false expression".</span></span> <span data-ttu-id="1a442-395">V opačném případě se nepřiřazují jako neomezeně.</span><span class="sxs-lookup"><span data-stu-id="1a442-395">Otherwise, it is not definitely assigned.</span></span>
*  <span data-ttu-id="1a442-396">Jednoznačné příkazy přiřazení *v v* After *expr* určují:</span><span class="sxs-lookup"><span data-stu-id="1a442-396">The definite assignment statement of *v* after *expr* is determined by:</span></span>
    * <span data-ttu-id="1a442-397">Pokud *expr_first* `true`je konstantní výraz s hodnotou, pak bude mít jednoznačný stav přiřazení *v v* After *expr* stejný jako pro určitý stav přiřazení *v* za za *expr_first*.</span><span class="sxs-lookup"><span data-stu-id="1a442-397">If *expr_first* is a constant expression with the value `true`, then the definite assignment state of *v* after *expr* is the same as the definite assignment state of *v* after *expr_first*.</span></span>
    * <span data-ttu-id="1a442-398">V opačném případě, pokud je stav *v v* po *expr_first* přiřazení jednoznačně přiřazen, je stav *v v* po jednoznačně přiřazeném *výrazu* .</span><span class="sxs-lookup"><span data-stu-id="1a442-398">Otherwise, if the state of *v* after *expr_first* is definitely assigned, then the state of *v* after *expr* is definitely assigned.</span></span>
    * <span data-ttu-id="1a442-399">V opačném případě platí, že pokud je stav *v v* po *expr_secondně* přiřazený, a stav *v* po po *expr_first* je "jednoznačně přiřazený po true Expression", pak je stav *v v* After *expr* jednoznačně přiřazení.</span><span class="sxs-lookup"><span data-stu-id="1a442-399">Otherwise, if the state of *v* after *expr_second* is definitely assigned, and the state of *v* after *expr_first* is "definitely assigned after true expression", then the state of *v* after *expr* is definitely assigned.</span></span>
    * <span data-ttu-id="1a442-400">V opačném případě platí, že pokud je stav *v v* po *expr_second* jednoznačně přiřazený nebo "jednoznačně přiřazený po nepravdivém výrazu", pak je stav *v v* After *expr* "jednoznačně přiřazený po nepravdivém výrazu".</span><span class="sxs-lookup"><span data-stu-id="1a442-400">Otherwise, if the state of *v* after *expr_second* is definitely assigned or "definitely assigned after false expression", then the state of *v* after *expr* is "definitely assigned after false expression".</span></span>
    * <span data-ttu-id="1a442-401">V opačném případě, pokud je stav *v* po po *expr_first* "jednoznačně přiřazený za true Expression", a stav *v v* After *expr_second* je "jednoznačně přiřazený za true Expression", pak stav *v v* After *expr* je "jednoznačně přiřazeno po výrazu true".</span><span class="sxs-lookup"><span data-stu-id="1a442-401">Otherwise, if the state of *v* after *expr_first* is "definitely assigned after true expression", and the state of *v* after *expr_second* is "definitely assigned after true expression", then the state of *v* after *expr* is "definitely assigned after true expression".</span></span>
    * <span data-ttu-id="1a442-402">V opačném případě stav *v* případě, kdy *výraz* není jednoznačně přiřazen.</span><span class="sxs-lookup"><span data-stu-id="1a442-402">Otherwise, the state of *v* after *expr* is not definitely assigned.</span></span>

<span data-ttu-id="1a442-403">V příkladu</span><span class="sxs-lookup"><span data-stu-id="1a442-403">In the example</span></span>
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
<span data-ttu-id="1a442-404">proměnná `i` se považuje za jednoznačně přiřazenou v jednom z vložených příkazů `if` příkazu, ale ne v druhé.</span><span class="sxs-lookup"><span data-stu-id="1a442-404">the variable `i` is considered definitely assigned in one of the embedded statements of an `if` statement but not in the other.</span></span> <span data-ttu-id="1a442-405">V příkazu v metodě `G`je proměnná `i` jednoznačně přiřazena v druhém vloženém příkazu, protože provádění výrazu `(i = y)` vždy předchází provedení tohoto vloženého příkazu. `if`</span><span class="sxs-lookup"><span data-stu-id="1a442-405">In the `if` statement in method `G`, the variable `i` is definitely assigned in the second embedded statement because execution of the expression `(i = y)` always precedes execution of this embedded statement.</span></span> <span data-ttu-id="1a442-406">V opačném případě proměnná `i` není jednoznačně přiřazena v prvním vloženém příkazu, protože `x >= 0` by mohla být testována na hodnotu true, výsledkem `i` je, že proměnná je Nepřiřazená.</span><span class="sxs-lookup"><span data-stu-id="1a442-406">In contrast, the variable `i` is not definitely assigned in the first embedded statement, since `x >= 0` might have tested true, resulting in the variable `i` being unassigned.</span></span>

#### <a name="-logical-negation-expressions"></a><span data-ttu-id="1a442-407">!</span><span class="sxs-lookup"><span data-stu-id="1a442-407">!</span></span> <span data-ttu-id="1a442-408">(logické negace) – výrazy</span><span class="sxs-lookup"><span data-stu-id="1a442-408">(logical negation) expressions</span></span>

<span data-ttu-id="1a442-409">Výraz *expr* formuláře `! expr_operand`:</span><span class="sxs-lookup"><span data-stu-id="1a442-409">For an expression *expr* of the form `! expr_operand`:</span></span>

*  <span data-ttu-id="1a442-410">Určitý stav přiřazení *v* rámci před *expr_operand* je stejný jako jednoznačný stav přiřazení *v* před *výrazem*.</span><span class="sxs-lookup"><span data-stu-id="1a442-410">The definite assignment state of *v* before *expr_operand* is the same as the definite assignment state of *v* before *expr*.</span></span>
*  <span data-ttu-id="1a442-411">Jednoznačný stav přiřazení *v v* po *výrazu* určuje:</span><span class="sxs-lookup"><span data-stu-id="1a442-411">The definite assignment state of *v* after *expr* is determined by:</span></span>
    * <span data-ttu-id="1a442-412">Pokud je stav *v* po za \* expr_operand \* jednoznačně přiřazený, pak je stav *v v* po jednoznačně přiřazeném *výrazu* .</span><span class="sxs-lookup"><span data-stu-id="1a442-412">If the state of *v* after \*expr_operand \*is definitely assigned, then the state of *v* after *expr* is definitely assigned.</span></span>
    * <span data-ttu-id="1a442-413">Pokud stav *v v* After \* expr_operand \* není jednoznačně přiřazen, pak stav *v v* po *výrazu* není jednoznačně přiřazen.</span><span class="sxs-lookup"><span data-stu-id="1a442-413">If the state of *v* after \*expr_operand \*is not definitely assigned, then the state of *v* after *expr* is not definitely assigned.</span></span>
    * <span data-ttu-id="1a442-414">Pokud stav *v v* After \* expr_operand \* je "jednoznačně přiřazený po nepravdivém výrazu", pak je stav *v v* After *expr* "jednoznačně přiřazený po výrazu true".</span><span class="sxs-lookup"><span data-stu-id="1a442-414">If the state of *v* after \*expr_operand \*is "definitely assigned after false expression", then the state of *v* after *expr* is "definitely assigned after true expression".</span></span>
    * <span data-ttu-id="1a442-415">Pokud je stav *v v* After \* expr_operand \* "po true Expression" "" jednoznačně přiřazen ", pak je stav v *po výrazu* " jednoznačně přiřazený po nepravdivém výrazu ".</span><span class="sxs-lookup"><span data-stu-id="1a442-415">If the state of *v* after \*expr_operand \*is "definitely assigned after true expression", then the state of *v* after *expr* is "definitely assigned after false expression".</span></span>

#### <a name="-null-coalescing-expressions"></a><span data-ttu-id="1a442-416">??</span><span class="sxs-lookup"><span data-stu-id="1a442-416">??</span></span> <span data-ttu-id="1a442-417">(slučovací výrazy s hodnotou null)</span><span class="sxs-lookup"><span data-stu-id="1a442-417">(null coalescing) expressions</span></span>

<span data-ttu-id="1a442-418">Výraz *expr* formuláře `expr_first ?? expr_second`:</span><span class="sxs-lookup"><span data-stu-id="1a442-418">For an expression *expr* of the form `expr_first ?? expr_second`:</span></span>

*  <span data-ttu-id="1a442-419">Určitý stav přiřazení *v* rámci před *expr_first* je stejný jako jednoznačný stav přiřazení *v* před *výrazem*.</span><span class="sxs-lookup"><span data-stu-id="1a442-419">The definite assignment state of *v* before *expr_first* is the same as the definite assignment state of *v* before *expr*.</span></span>
*  <span data-ttu-id="1a442-420">Určitý stav přiřazení *v* rámci před *expr_second* je stejný jako jednoznačný stav přiřazení *v* za za *expr_first*.</span><span class="sxs-lookup"><span data-stu-id="1a442-420">The definite assignment state of *v* before *expr_second* is the same as the definite assignment state of *v* after *expr_first*.</span></span>
*  <span data-ttu-id="1a442-421">Jednoznačné příkazy přiřazení *v v* After *expr* určují:</span><span class="sxs-lookup"><span data-stu-id="1a442-421">The definite assignment statement of *v* after *expr* is determined by:</span></span>
    * <span data-ttu-id="1a442-422">Pokud *expr_first* je konstantní výraz ([konstantní výrazy](expressions.md#constant-expressions)) s hodnotou null, pak je stav *v v* After *expr* stejný jako stav *v v* za *expr_second*.</span><span class="sxs-lookup"><span data-stu-id="1a442-422">If *expr_first* is a constant expression ([Constant expressions](expressions.md#constant-expressions)) with value null, then the state of *v* after *expr* is the same as the state of *v* after *expr_second*.</span></span>
*  <span data-ttu-id="1a442-423">V opačném případě je stav *v v* After *expr* stejný jako jednoznačný stav přiřazení *v* po *expr_first*.</span><span class="sxs-lookup"><span data-stu-id="1a442-423">Otherwise, the state of *v* after *expr* is the same as the definite assignment state of *v* after *expr_first*.</span></span>

#### <a name="-conditional-expressions"></a><span data-ttu-id="1a442-424">?: (podmíněné) výrazy</span><span class="sxs-lookup"><span data-stu-id="1a442-424">?: (conditional) expressions</span></span>

<span data-ttu-id="1a442-425">Výraz *expr* formuláře `expr_cond ? expr_true : expr_false`:</span><span class="sxs-lookup"><span data-stu-id="1a442-425">For an expression *expr* of the form `expr_cond ? expr_true : expr_false`:</span></span>

*  <span data-ttu-id="1a442-426">Určitý stav přiřazení *v* rámci před *expr_cond* je stejný jako stav *v* před *výrazem*.</span><span class="sxs-lookup"><span data-stu-id="1a442-426">The definite assignment state of *v* before *expr_cond* is the same as the state of *v* before *expr*.</span></span>
*  <span data-ttu-id="1a442-427">Určitý stav přiřazení *v v* před *expr_true* se jednoznačně přiřadí, pokud a pouze v případě, že je k dispozici jeden z následujících:</span><span class="sxs-lookup"><span data-stu-id="1a442-427">The definite assignment state of *v* before *expr_true* is definitely assigned if and only if one of the following holds:</span></span>
    * <span data-ttu-id="1a442-428">*expr_cond* je konstantní výraz s hodnotou.`false`</span><span class="sxs-lookup"><span data-stu-id="1a442-428">*expr_cond* is a constant expression with the value `false`</span></span>
    * <span data-ttu-id="1a442-429">stav *v v* po po *expr_cond* je jednoznačně přiřazený nebo "jednoznačně přiřazeno po výrazu true".</span><span class="sxs-lookup"><span data-stu-id="1a442-429">the state of *v* after *expr_cond* is definitely assigned or "definitely assigned after true expression".</span></span>
*  <span data-ttu-id="1a442-430">Určitý stav přiřazení *v v* před *expr_false* se jednoznačně přiřadí, pokud a pouze v případě, že je k dispozici jeden z následujících:</span><span class="sxs-lookup"><span data-stu-id="1a442-430">The definite assignment state of *v* before *expr_false* is definitely assigned if and only if one of the following holds:</span></span>
    * <span data-ttu-id="1a442-431">*expr_cond* je konstantní výraz s hodnotou.`true`</span><span class="sxs-lookup"><span data-stu-id="1a442-431">*expr_cond* is a constant expression with the value `true`</span></span>
*  <span data-ttu-id="1a442-432">stav *v v* po *expr_cond* je jednoznačně přiřazený nebo "jednoznačně přiřazený po nepravdivém výrazu".</span><span class="sxs-lookup"><span data-stu-id="1a442-432">the state of *v* after *expr_cond* is definitely assigned or "definitely assigned after false expression".</span></span>
*  <span data-ttu-id="1a442-433">Jednoznačný stav přiřazení *v v* po *výrazu* určuje:</span><span class="sxs-lookup"><span data-stu-id="1a442-433">The definite assignment state of *v* after *expr* is determined by:</span></span>
    * <span data-ttu-id="1a442-434">Pokud *expr_cond* je konstantní výraz ([konstantní výrazy](expressions.md#constant-expressions)) s hodnotou `true` , pak stav *v v* po *výrazu expr* je stejný jako stav *v* za *expr_true*.</span><span class="sxs-lookup"><span data-stu-id="1a442-434">If *expr_cond* is a constant expression ([Constant expressions](expressions.md#constant-expressions)) with value `true` then the state of *v* after *expr* is the same as the state of *v* after *expr_true*.</span></span>
    * <span data-ttu-id="1a442-435">V opačném případě, pokud *expr_cond* je konstantní výraz ([konstantní výrazy](expressions.md#constant-expressions)) `false` s hodnotou, pak stav *v v* After *expr* je stejný jako stav *v* za za *expr_false*.</span><span class="sxs-lookup"><span data-stu-id="1a442-435">Otherwise, if *expr_cond* is a constant expression ([Constant expressions](expressions.md#constant-expressions)) with value `false` then the state of *v* after *expr* is the same as the state of *v* after *expr_false*.</span></span>
    * <span data-ttu-id="1a442-436">V opačném případě, pokud je stav *v v* po *expr_trueně* přiřazený a stav *v v* po, kdy *expr_false* je jednoznačně přiřazený, pak je stav *v v* After *expr* jednoznačně přiřazený.</span><span class="sxs-lookup"><span data-stu-id="1a442-436">Otherwise, if the state of *v* after *expr_true* is definitely assigned and the state of *v* after *expr_false* is definitely assigned, then the state of *v* after *expr* is definitely assigned.</span></span>
    * <span data-ttu-id="1a442-437">V opačném případě stav *v* případě, kdy *výraz* není jednoznačně přiřazen.</span><span class="sxs-lookup"><span data-stu-id="1a442-437">Otherwise, the state of *v* after *expr* is not definitely assigned.</span></span>

#### <a name="anonymous-functions"></a><span data-ttu-id="1a442-438">Anonymní funkce</span><span class="sxs-lookup"><span data-stu-id="1a442-438">Anonymous functions</span></span>

<span data-ttu-id="1a442-439">Výraz *lambda_expression* nebo *anonymous_method_expression* *expr* *s tělo (* buď *blok* nebo *výraz*):</span><span class="sxs-lookup"><span data-stu-id="1a442-439">For a *lambda_expression* or *anonymous_method_expression* *expr* with a body (either *block* or *expression*) *body*:</span></span>

*  <span data-ttu-id="1a442-440">Určitý stav přiřazení vnější proměnné *v* před *textovým tělem* je stejný jako stav *v* před *výrazem*.</span><span class="sxs-lookup"><span data-stu-id="1a442-440">The definite assignment state of an outer variable *v* before *body* is the same as the state of *v* before *expr*.</span></span> <span data-ttu-id="1a442-441">To znamená, že je z kontextu anonymní funkce zděděný pouze stav přiřazení vnějších proměnných.</span><span class="sxs-lookup"><span data-stu-id="1a442-441">That is, definite assignment state of outer variables is inherited from the context of the anonymous function.</span></span>
*  <span data-ttu-id="1a442-442">Jednoznačný stav přiřazení vnější proměnné *v* po *výrazu expr* je stejný jako stav *v* před *výrazem*.</span><span class="sxs-lookup"><span data-stu-id="1a442-442">The definite assignment state of an outer variable *v* after *expr* is the same as the state of *v* before *expr*.</span></span>

<span data-ttu-id="1a442-443">Příklad</span><span class="sxs-lookup"><span data-stu-id="1a442-443">The example</span></span>
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
<span data-ttu-id="1a442-444">vygeneruje chybu při kompilaci, protože `max` není jednoznačně přiřazen, kde je deklarace anonymní funkce deklarována.</span><span class="sxs-lookup"><span data-stu-id="1a442-444">generates a compile-time error since `max` is not definitely assigned where the anonymous function is declared.</span></span> <span data-ttu-id="1a442-445">Příklad</span><span class="sxs-lookup"><span data-stu-id="1a442-445">The example</span></span>
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
<span data-ttu-id="1a442-446">také generuje chybu při kompilaci, protože přiřazení k `n` v anonymní funkci nemá žádný vliv na jednoznačný `n` stav přiřazení vně anonymní funkce.</span><span class="sxs-lookup"><span data-stu-id="1a442-446">also generates a compile-time error since the assignment to `n` in the anonymous function has no affect on the definite assignment state of `n` outside the anonymous function.</span></span>

## <a name="variable-references"></a><span data-ttu-id="1a442-447">Odkazy na proměnné</span><span class="sxs-lookup"><span data-stu-id="1a442-447">Variable references</span></span>

<span data-ttu-id="1a442-448">*Variable_reference* je *výraz* , který je klasifikován jako proměnná.</span><span class="sxs-lookup"><span data-stu-id="1a442-448">A *variable_reference* is an *expression* that is classified as a variable.</span></span> <span data-ttu-id="1a442-449">*Variable_reference* označuje umístění úložiště, ke kterému lze získat pøístup pro načtení aktuální hodnoty a uložení nové hodnoty.</span><span class="sxs-lookup"><span data-stu-id="1a442-449">A *variable_reference* denotes a storage location that can be accessed both to fetch the current value and to store a new value.</span></span>

```antlr
variable_reference
    : expression
    ;
```

<span data-ttu-id="1a442-450">V jazyce C C++a je *variable_reference* označován jako *l*-hodnota.</span><span class="sxs-lookup"><span data-stu-id="1a442-450">In C and C++, a *variable_reference* is known as an *lvalue*.</span></span>

## <a name="atomicity-of-variable-references"></a><span data-ttu-id="1a442-451">Nedělitelnost odkazů na proměnné</span><span class="sxs-lookup"><span data-stu-id="1a442-451">Atomicity of variable references</span></span>

<span data-ttu-id="1a442-452">Čtení a zápisy následujících datových typů jsou atomické: `bool` `byte`, `char` `ushort` `short` `sbyte`,,,,, `uint`, `int`, `float`a odkazové typy.</span><span class="sxs-lookup"><span data-stu-id="1a442-452">Reads and writes of the following data types are atomic: `bool`, `char`, `byte`, `sbyte`, `short`, `ushort`, `uint`, `int`, `float`, and reference types.</span></span> <span data-ttu-id="1a442-453">Kromě toho jsou operace čtení a zápisy typů výčtu s nadřízeným typem v předchozím seznamu také atomické.</span><span class="sxs-lookup"><span data-stu-id="1a442-453">In addition, reads and writes of enum types with an underlying type in the previous list are also atomic.</span></span> <span data-ttu-id="1a442-454">Čtení a zápisy jiných typů, včetně `long` `double`, `ulong`, a `decimal`i uživatelsky definovaných typů, nejsou zaručeny jako atomické.</span><span class="sxs-lookup"><span data-stu-id="1a442-454">Reads and writes of other types, including `long`, `ulong`, `double`, and `decimal`, as well as user-defined types, are not guaranteed to be atomic.</span></span> <span data-ttu-id="1a442-455">Kromě funkcí v knihovně, které jsou navrženy pro tento účel, neexistuje žádná záruka atomické úpravy pro čtení a zápis, například v případě zvýšení nebo snížení.</span><span class="sxs-lookup"><span data-stu-id="1a442-455">Aside from the library functions designed for that purpose, there is no guarantee of atomic read-modify-write, such as in the case of increment or decrement.</span></span>

