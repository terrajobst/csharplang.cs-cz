# <a name="exceptions"></a><span data-ttu-id="4f0c6-101">Výjimky</span><span class="sxs-lookup"><span data-stu-id="4f0c6-101">Exceptions</span></span>

<span data-ttu-id="4f0c6-102">Výjimky v jazyce C# zadejte strukturovaných, jednotné a typově bezpečný způsob zpracování systémové úrovni a úrovni aplikace chybové stavy.</span><span class="sxs-lookup"><span data-stu-id="4f0c6-102">Exceptions in C# provide a structured, uniform, and type-safe way of handling both system level and application level error conditions.</span></span> <span data-ttu-id="4f0c6-103">Mechanismus výjimek v jazyce C# je podobný, C++, s několika rozdíly:</span><span class="sxs-lookup"><span data-stu-id="4f0c6-103">The exception mechanism in C# is quite similar to that of C++, with a few important differences:</span></span>

*  <span data-ttu-id="4f0c6-104">V jazyce C#, všechny výjimky musí být reprezentovaný instancí typu třídy odvozené z `System.Exception`.</span><span class="sxs-lookup"><span data-stu-id="4f0c6-104">In C#, all exceptions must be represented by an instance of a class type derived from `System.Exception`.</span></span> <span data-ttu-id="4f0c6-105">V jazyce C++ lze použít libovolnou hodnotu libovolného typu představující výjimku.</span><span class="sxs-lookup"><span data-stu-id="4f0c6-105">In C++, any value of any type can be used to represent an exception.</span></span>
*  <span data-ttu-id="4f0c6-106">V jazyce C# bloku finally ([příkazu try](statements.md#the-try-statement)) můžete použít pro zapsání ukončovací kód, který se spustí v normálním spuštění a výjimečných podmínek.</span><span class="sxs-lookup"><span data-stu-id="4f0c6-106">In C#, a finally block ([The try statement](statements.md#the-try-statement)) can be used to write termination code that executes in both normal execution and exceptional conditions.</span></span> <span data-ttu-id="4f0c6-107">Takový kód je obtížné je napsat bez duplikování kódu v jazyce C++.</span><span class="sxs-lookup"><span data-stu-id="4f0c6-107">Such code is difficult to write in C++ without duplicating code.</span></span>
*  <span data-ttu-id="4f0c6-108">V jazyce C# výjimek na úrovni systému, jako je přetečení, dělení nulou a null přístupů přes ukazatel, jasně definovaných tříd výjimek a jsou na stejné úrovni podmínky chyby na úrovni aplikace.</span><span class="sxs-lookup"><span data-stu-id="4f0c6-108">In C#, system-level exceptions such as overflow, divide-by-zero, and null dereferences have well defined exception classes and are on a par with application-level error conditions.</span></span>

## <a name="causes-of-exceptions"></a><span data-ttu-id="4f0c6-109">Příčinami výjimek</span><span class="sxs-lookup"><span data-stu-id="4f0c6-109">Causes of exceptions</span></span>

<span data-ttu-id="4f0c6-110">Výjimky mohou být vyvolány dvěma různými způsoby.</span><span class="sxs-lookup"><span data-stu-id="4f0c6-110">Exception can be thrown in two different ways.</span></span>

*  <span data-ttu-id="4f0c6-111">A `throw` – příkaz ([příkazu throw](statements.md#the-throw-statement)) vyvolá výjimku, okamžitě a nepodmíněně.</span><span class="sxs-lookup"><span data-stu-id="4f0c6-111">A `throw` statement ([The throw statement](statements.md#the-throw-statement)) throws an exception immediately and unconditionally.</span></span> <span data-ttu-id="4f0c6-112">Ovládací prvek nikdy dosáhne příkazu za příkazem okamžitě `throw`.</span><span class="sxs-lookup"><span data-stu-id="4f0c6-112">Control never reaches the statement immediately following the `throw`.</span></span>
*  <span data-ttu-id="4f0c6-113">Při operaci nelze dokončit, obvykle některých výjimečných podmínek, které vznikají při zpracování příkazů jazyka C# a výrazu způsobit výjimku za určitých okolností.</span><span class="sxs-lookup"><span data-stu-id="4f0c6-113">Certain exceptional conditions that arise during the processing of C# statements and expression cause an exception in certain circumstances when the operation cannot be completed normally.</span></span> <span data-ttu-id="4f0c6-114">Například operace dělení celého čísla ([operátor dělení](expressions.md#division-operator)) vyvolá `System.DivideByZeroException` je-li jmenovatelem nula.</span><span class="sxs-lookup"><span data-stu-id="4f0c6-114">For example, an integer division operation ([Division operator](expressions.md#division-operator)) throws a `System.DivideByZeroException` if the denominator is zero.</span></span> <span data-ttu-id="4f0c6-115">Zobrazit [běžné třídy výjimek](exceptions.md#common-exception-classes) seznam různé výjimky, které se mohou vyskytnout tímto způsobem.</span><span class="sxs-lookup"><span data-stu-id="4f0c6-115">See [Common Exception Classes](exceptions.md#common-exception-classes) for a list of the various exceptions that can occur in this way.</span></span>

## <a name="the-systemexception-class"></a><span data-ttu-id="4f0c6-116">Třídy System.Exception</span><span class="sxs-lookup"><span data-stu-id="4f0c6-116">The System.Exception class</span></span>

<span data-ttu-id="4f0c6-117">`System.Exception` Třída je základní typ pro všechny výjimky.</span><span class="sxs-lookup"><span data-stu-id="4f0c6-117">The `System.Exception` class is the base type of all exceptions.</span></span> <span data-ttu-id="4f0c6-118">Tato třída obsahuje několik významných vlastností, které sdílejí všechny výjimky:</span><span class="sxs-lookup"><span data-stu-id="4f0c6-118">This class has a few notable properties that all exceptions share:</span></span>

*  <span data-ttu-id="4f0c6-119">`Message` je vlastnost jen pro čtení typu `string` , který obsahuje čitelný popis důvod výjimky.</span><span class="sxs-lookup"><span data-stu-id="4f0c6-119">`Message` is a read-only property of type `string` that contains a human-readable description of the reason for the exception.</span></span>
*  <span data-ttu-id="4f0c6-120">`InnerException` je vlastnost jen pro čtení typu `Exception`.</span><span class="sxs-lookup"><span data-stu-id="4f0c6-120">`InnerException` is a read-only property of type `Exception`.</span></span> <span data-ttu-id="4f0c6-121">Pokud její hodnota je jiná než null, odkazuje na výjimku, která způsobila aktuální výjimku – to znamená, že byla aktuální výjimka vyvolána v bloku catch zpracování `InnerException`.</span><span class="sxs-lookup"><span data-stu-id="4f0c6-121">If its value is non-null, it refers to the exception that caused the current exception—that is, the current exception was raised in a catch block handling the `InnerException`.</span></span> <span data-ttu-id="4f0c6-122">Jinak jeho hodnota je null, která znamená, že nebyl touto výjimkou způsobeno výjimkou jiný.</span><span class="sxs-lookup"><span data-stu-id="4f0c6-122">Otherwise, its value is null, indicating that this exception was not caused by another exception.</span></span> <span data-ttu-id="4f0c6-123">Může být libovolný počet objektů výjimek zřetězit tímto způsobem.</span><span class="sxs-lookup"><span data-stu-id="4f0c6-123">The number of exception objects chained together in this manner can be arbitrary.</span></span>

<span data-ttu-id="4f0c6-124">Hodnoty těchto vlastností lze zadat ve volání do konstruktoru instance pro `System.Exception`.</span><span class="sxs-lookup"><span data-stu-id="4f0c6-124">The value of these properties can be specified in calls to the instance constructor for `System.Exception`.</span></span>

## <a name="how-exceptions-are-handled"></a><span data-ttu-id="4f0c6-125">Způsob zpracování výjimek</span><span class="sxs-lookup"><span data-stu-id="4f0c6-125">How exceptions are handled</span></span>

<span data-ttu-id="4f0c6-126">Jsou výjimky zpracovávány `try` – příkaz ([příkazu try](statements.md#the-try-statement)).</span><span class="sxs-lookup"><span data-stu-id="4f0c6-126">Exceptions are handled by a `try` statement ([The try statement](statements.md#the-try-statement)).</span></span>

<span data-ttu-id="4f0c6-127">Když dojde k výjimce, systém vyhledá nejbližší `catch` klauzuli, která dokáže zpracovat výjimky, počítáno od run-time typu výjimky.</span><span class="sxs-lookup"><span data-stu-id="4f0c6-127">When an exception occurs, the system searches for the nearest `catch` clause that can handle the exception, as determined by the run-time type of the exception.</span></span> <span data-ttu-id="4f0c6-128">Nejprve je prohledána aktuální metoda lexikálně uzavírající `try` příkazu a klauzulích přidruženým blokem catch příkazu try jsou považovány za v pořadí.</span><span class="sxs-lookup"><span data-stu-id="4f0c6-128">First, the current method is searched for a lexically enclosing `try` statement, and the associated catch clauses of the try statement are considered in order.</span></span> <span data-ttu-id="4f0c6-129">Pokud se nezdaří, metoda, která volá aktuální metoda prohledána lexikálně uzavírající `try` příkaz, který obklopuje místa volání na aktuální metodu.</span><span class="sxs-lookup"><span data-stu-id="4f0c6-129">If that fails, the method that called the current method is searched for a lexically enclosing `try` statement that encloses the point of the call to the current method.</span></span> <span data-ttu-id="4f0c6-130">Hledání pokračuje, dokud `catch` klauzule se nachází na aktuální výjimku, která dokáže zpracovat pojmenováním třídu výjimky, která má stejné třídy nebo základní třídu typu vyvolání výjimky za běhu.</span><span class="sxs-lookup"><span data-stu-id="4f0c6-130">This search continues until a `catch` clause is found that can handle the current exception, by naming an exception class that is of the same class, or a base class, of the run-time type of the exception being thrown.</span></span> <span data-ttu-id="4f0c6-131">A `catch` klauzuli, která není název třídy výjimek může zpracovávat všechny výjimky.</span><span class="sxs-lookup"><span data-stu-id="4f0c6-131">A `catch` clause that doesn't name an exception class can handle any exception.</span></span>

<span data-ttu-id="4f0c6-132">Po nalezení odpovídající klauzuli catch. systém přechází do řízení je převedeno na první příkaz v klauzuli catch.</span><span class="sxs-lookup"><span data-stu-id="4f0c6-132">Once a matching catch clause is found, the system prepares to transfer control to the first statement of the catch clause.</span></span> <span data-ttu-id="4f0c6-133">Před zahájením provádění v klauzuli catch, systém nejprve provede, v pořadí, všechny `finally` klauzule, které byly přidruženy k další příkazy try, vnořené než ty, které vyvolala výjimku.</span><span class="sxs-lookup"><span data-stu-id="4f0c6-133">Before execution of the catch clause begins, the system first executes, in order, any `finally` clauses that were associated with try statements more nested that than the one that caught the exception.</span></span>

<span data-ttu-id="4f0c6-134">Pokud není nalezen žádný odpovídající klauzuli catch, dojde k jedné ze dvou kroků:</span><span class="sxs-lookup"><span data-stu-id="4f0c6-134">If no matching catch clause is found, one of two things occurs:</span></span>

*  <span data-ttu-id="4f0c6-135">Pokud hledání odpovídající klauzuli catch dosáhne statického konstruktoru ([statické konstruktory](classes.md#static-constructors)) nebo statickém inicializátoru pole, o `System.TypeInitializationException` dojde v okamžiku, která aktivuje volání statického konstruktoru.</span><span class="sxs-lookup"><span data-stu-id="4f0c6-135">If the search for a matching catch clause reaches a static constructor ([Static constructors](classes.md#static-constructors)) or static field initializer, then a `System.TypeInitializationException` is thrown at the point that triggered the invocation of the static constructor.</span></span> <span data-ttu-id="4f0c6-136">Vnitřní výjimka `System.TypeInitializationException` obsahuje výjimku, která byla původně vyvolána.</span><span class="sxs-lookup"><span data-stu-id="4f0c6-136">The inner exception of the `System.TypeInitializationException` contains the exception that was originally thrown.</span></span>
*  <span data-ttu-id="4f0c6-137">Dosáhne-li vyhledat odpovídající klauzule catch kód, který původně zahájeno vlákno, se ukončí spouštění vlákna.</span><span class="sxs-lookup"><span data-stu-id="4f0c6-137">If the search for matching catch clauses reaches the code that initially started the thread, then execution of the thread is terminated.</span></span> <span data-ttu-id="4f0c6-138">Dopad ukončení, je definováno implementací.</span><span class="sxs-lookup"><span data-stu-id="4f0c6-138">The impact of such termination is implementation-defined.</span></span>

<span data-ttu-id="4f0c6-139">Výjimky, ke kterým dochází při spuštění destruktoru stojí za zvláštní pozornost.</span><span class="sxs-lookup"><span data-stu-id="4f0c6-139">Exceptions that occur during destructor execution are worth special mention.</span></span> <span data-ttu-id="4f0c6-140">Pokud dojde k výjimce při spuštění destruktoru a tato výjimka není zachycena, provádění tento destruktor se ukončí a je volán destruktor základní třídy (pokud existuje).</span><span class="sxs-lookup"><span data-stu-id="4f0c6-140">If an exception occurs during destructor execution, and that exception is not caught, then the execution of that destructor is terminated and the destructor of the base class (if any) is called.</span></span> <span data-ttu-id="4f0c6-141">Pokud není žádnou základní třídu (stejně jako v případě třídy `object` typ) nebo pokud neexistuje žádný destruktor základní třídy, pak se zruší výjimku.</span><span class="sxs-lookup"><span data-stu-id="4f0c6-141">If there is no base class (as in the case of the `object` type) or if there is no base class destructor, then the exception is discarded.</span></span>

## <a name="common-exception-classes"></a><span data-ttu-id="4f0c6-142">Společné třídy výjimek</span><span class="sxs-lookup"><span data-stu-id="4f0c6-142">Common Exception Classes</span></span>

<span data-ttu-id="4f0c6-143">Následující výjimky jsou vyvolány pomocí určitých operací C#.</span><span class="sxs-lookup"><span data-stu-id="4f0c6-143">The following exceptions are thrown by certain C# operations.</span></span>

|                                      |                |
|--------------------------------------|----------------|
| `System.ArithmeticException`         | <span data-ttu-id="4f0c6-144">Základní třída pro výjimky, ke kterým dochází při aritmetické operace, jako například `System.DivideByZeroException` a `System.OverflowException`.</span><span class="sxs-lookup"><span data-stu-id="4f0c6-144">A base class for exceptions that occur during arithmetic operations, such as `System.DivideByZeroException` and `System.OverflowException`.</span></span> | 
| `System.ArrayTypeMismatchException`  | <span data-ttu-id="4f0c6-145">Vyvolána, když úložiště do pole se nezdaří, protože skutečný typ elementu uložené není kompatibilní s skutečný typ pole.</span><span class="sxs-lookup"><span data-stu-id="4f0c6-145">Thrown when a store into an array fails because the actual type of the stored element is incompatible with the actual type of the array.</span></span> | 
| `System.DivideByZeroException`       | <span data-ttu-id="4f0c6-146">Vyvolána, když dojde k pokusu o dělení nulou celočíselnou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="4f0c6-146">Thrown when an attempt to divide an integral value by zero occurs.</span></span> | 
| `System.IndexOutOfRangeException`    | <span data-ttu-id="4f0c6-147">Vyvolána výjimka při pokusu o indexování pole prostřednictvím index, který je menší než nula nebo mimo hranice pole.</span><span class="sxs-lookup"><span data-stu-id="4f0c6-147">Thrown when an attempt to index an array via an index that is less than zero or outside the bounds of the array.</span></span> | 
| `System.InvalidCastException`        | <span data-ttu-id="4f0c6-148">Vyvolána, když selže explicitní převod ze základního typu nebo rozhraní pro odvozený typ v době běhu.</span><span class="sxs-lookup"><span data-stu-id="4f0c6-148">Thrown when an explicit conversion from a base type or interface to a derived type fails at run time.</span></span> | 
| `System.NullReferenceException`      | <span data-ttu-id="4f0c6-149">Vyvolána, když `null` odkaz se používá způsobem, který způsobí, že odkazovaný objekt jako povinné.</span><span class="sxs-lookup"><span data-stu-id="4f0c6-149">Thrown when a `null` reference is used in a way that causes the referenced object to be required.</span></span> | 
| `System.OutOfMemoryException`        | <span data-ttu-id="4f0c6-150">Při pokusu o přidělení paměti (přes `new`) se nezdaří.</span><span class="sxs-lookup"><span data-stu-id="4f0c6-150">Thrown when an attempt to allocate memory (via `new`) fails.</span></span> | 
| `System.OverflowException`           | <span data-ttu-id="4f0c6-151">Vyvolána, když v aritmetické operace `checked` kontextu přetečení.</span><span class="sxs-lookup"><span data-stu-id="4f0c6-151">Thrown when an arithmetic operation in a `checked` context overflows.</span></span> | 
| `System.StackOverflowException`      | <span data-ttu-id="4f0c6-152">Vyvolána, když se vyčerpá zásobníku spouštění tak, že příliš mnoho volání metody čekající; obvykle svědčí o velmi podrobné nebo bez vazby rekurze.</span><span class="sxs-lookup"><span data-stu-id="4f0c6-152">Thrown when the execution stack is exhausted by having too many pending method calls; typically indicative of very deep or unbounded recursion.</span></span> | 
| `System.TypeInitializationException` | <span data-ttu-id="4f0c6-153">Vyvolána, když statický konstruktor vyvolá výjimku a ne `catch` existuje klauzule má zachytit.</span><span class="sxs-lookup"><span data-stu-id="4f0c6-153">Thrown when a static constructor throws an exception, and no `catch` clauses exists to catch it.</span></span> | 