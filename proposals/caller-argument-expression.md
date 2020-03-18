---
ms.openlocfilehash: fa3326bf69c83b6042b1db7b5567fd5c28d6f81a
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484522"
---
# <a name="callerargumentexpression"></a><span data-ttu-id="6ab07-101">CallerArgumentExpression</span><span class="sxs-lookup"><span data-stu-id="6ab07-101">CallerArgumentExpression</span></span>

* <span data-ttu-id="6ab07-102">Navrženo [x]</span><span class="sxs-lookup"><span data-stu-id="6ab07-102">[x] Proposed</span></span>
* <span data-ttu-id="6ab07-103">[] Prototyp: Nezahájeno</span><span class="sxs-lookup"><span data-stu-id="6ab07-103">[ ] Prototype: Not Started</span></span>
* <span data-ttu-id="6ab07-104">[] Implementace: Nezahájeno</span><span class="sxs-lookup"><span data-stu-id="6ab07-104">[ ] Implementation: Not Started</span></span>
* <span data-ttu-id="6ab07-105">[] Specifikace: Nezahájeno</span><span class="sxs-lookup"><span data-stu-id="6ab07-105">[ ] Specification: Not Started</span></span>

## <a name="summary"></a><span data-ttu-id="6ab07-106">Souhrn</span><span class="sxs-lookup"><span data-stu-id="6ab07-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="6ab07-107">Umožněte vývojářům zachytit výrazy předané metodě, aby se povolily lepší chybové zprávy v rozhraních API pro diagnostiku a testování a aby se snížily úhozy.</span><span class="sxs-lookup"><span data-stu-id="6ab07-107">Allow developers to capture the expressions passed to a method, to enable better error messages in diagnostic/testing APIs and reduce keystrokes.</span></span>

## <a name="motivation"></a><span data-ttu-id="6ab07-108">Motivační</span><span class="sxs-lookup"><span data-stu-id="6ab07-108">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="6ab07-109">Pokud se ověření kontrolního výrazu nebo argumentu nepovede, vývojář chce co nejvíce informovat o tom, kde a proč selhal.</span><span class="sxs-lookup"><span data-stu-id="6ab07-109">When an assertion or argument validation fails, the developer wants to know as much as possible about where and why it failed.</span></span> <span data-ttu-id="6ab07-110">Nicméně současná rozhraní API pro diagnostiku to zcela neusnadňuje.</span><span class="sxs-lookup"><span data-stu-id="6ab07-110">However, today's diagnostic APIs do not fully facilitate this.</span></span> <span data-ttu-id="6ab07-111">Vezměte v úvahu následující metodu:</span><span class="sxs-lookup"><span data-stu-id="6ab07-111">Consider the following method:</span></span>

```csharp
T Single<T>(this T[] array)
{
    Debug.Assert(array != null);
    Debug.Assert(array.Length == 1);

    return array[0];
}
```

<span data-ttu-id="6ab07-112">Pokud jeden z kontrolních výrazů selže, bude v trasování zásobníku k dispozici pouze název souboru, číslo řádku a název metody.</span><span class="sxs-lookup"><span data-stu-id="6ab07-112">When one of the asserts fail, only the filename, line number, and method name will be provided in the stack trace.</span></span> <span data-ttu-id="6ab07-113">Vývojář nebude moci zjistit, u kterého kontrolního výrazu došlo k chybě, a to tak, že bude muset otevřít soubor a přejít na zadané číslo řádku a zjistit, co se pokazilo.</span><span class="sxs-lookup"><span data-stu-id="6ab07-113">The developer will not be able to tell which assert failed from this information-- (s)he will have to open the file and navigate to the provided line number to see what went wrong.</span></span>

<span data-ttu-id="6ab07-114">Toto je také rozhraní pro testování důvodů, které musí poskytovat řadu metod vyhodnocení.</span><span class="sxs-lookup"><span data-stu-id="6ab07-114">This is also the reason testing frameworks have to provide a variety of assert methods.</span></span> <span data-ttu-id="6ab07-115">Pomocí xUnit `Assert.True` a `Assert.False` se často nepoužívají, protože neposkytují dostatek kontextu o tom, co selhalo.</span><span class="sxs-lookup"><span data-stu-id="6ab07-115">With xUnit, `Assert.True` and `Assert.False` are not frequently used because they do not provide enough context about what failed.</span></span>

<span data-ttu-id="6ab07-116">I když je tato situace pro ověřování argumentu lepší, protože vývojářům se zobrazují názvy neplatných argumentů, vývojář musí tyto názvy předat výjimkám ručně.</span><span class="sxs-lookup"><span data-stu-id="6ab07-116">While the situation is a bit better for argument validation because the names of invalid arguments are shown to the developer, the developer must pass these names to exceptions manually.</span></span> <span data-ttu-id="6ab07-117">Pokud byl výše uvedený příklad přepsaný pro použití tradičního ověřování argumentu místo `Debug.Assert`, by vypadal jako</span><span class="sxs-lookup"><span data-stu-id="6ab07-117">If the above example were rewritten to use traditional argument validation instead of `Debug.Assert`, it would look like</span></span>

```csharp
T Single<T>(this T[] array)
{
    if (array == null)
    {
        throw new ArgumentNullException(nameof(array));
    }

    if (array.Length != 1)
    {
        throw new ArgumentException("Array must contain a single element.", nameof(array));
    }

    return array[0];
}
```

<span data-ttu-id="6ab07-118">Všimněte si, že `nameof(array)` musí být předány každé výjimce, i když již není jasné z kontextu, který je argument neplatný.</span><span class="sxs-lookup"><span data-stu-id="6ab07-118">Notice that `nameof(array)` must be passed to each exception, although it's already clear from context which argument is invalid.</span></span>

## <a name="detailed-design"></a><span data-ttu-id="6ab07-119">Podrobný návrh</span><span class="sxs-lookup"><span data-stu-id="6ab07-119">Detailed design</span></span>
[design]: #detailed-design

<span data-ttu-id="6ab07-120">Ve výše uvedených příkladech, včetně řetězce `"array != null"` nebo `"array.Length == 1"` ve zprávě kontrolního výrazu, by mohl vývojář určit, co se nepovedlo.</span><span class="sxs-lookup"><span data-stu-id="6ab07-120">In the above examples, including the string `"array != null"` or `"array.Length == 1"` in the assert message would help the developer determine what failed.</span></span> <span data-ttu-id="6ab07-121">Zadejte `CallerArgumentExpression`: Jedná se o atribut, který může rozhraní použít k získání řetězce přidruženého k konkrétnímu argumentu metody.</span><span class="sxs-lookup"><span data-stu-id="6ab07-121">Enter `CallerArgumentExpression`: it's an attribute the framework can use to obtain the string associated with a particular method argument.</span></span> <span data-ttu-id="6ab07-122">Přidáme ho do `Debug.Assert` například</span><span class="sxs-lookup"><span data-stu-id="6ab07-122">We would add it to `Debug.Assert` like so</span></span>

```csharp
public static class Debug
{
    public static void Assert(bool condition, [CallerArgumentExpression("condition")] string message = null);
}
```

<span data-ttu-id="6ab07-123">Zdrojový kód v předchozím příkladu by zůstal stejný.</span><span class="sxs-lookup"><span data-stu-id="6ab07-123">The source code in the above example would stay the same.</span></span> <span data-ttu-id="6ab07-124">Nicméně kód, který kompilátor skutečně generuje, by odpovídal</span><span class="sxs-lookup"><span data-stu-id="6ab07-124">However, the code the compiler actually emits would correspond to</span></span>

```csharp
T Single<T>(this T[] array)
{
    Debug.Assert(array != null, "array != null");
    Debug.Assert(array.Length == 1, "array.Length == 1");

    return array[0];
}
```

<span data-ttu-id="6ab07-125">Kompilátor speciálně rozpoznává atribut u `Debug.Assert`.</span><span class="sxs-lookup"><span data-stu-id="6ab07-125">The compiler specially recognizes the attribute on `Debug.Assert`.</span></span> <span data-ttu-id="6ab07-126">Předá řetězec přidružený k argumentu uvedenému v konstruktoru atributu (v tomto případě `condition`) na webu volání.</span><span class="sxs-lookup"><span data-stu-id="6ab07-126">It passes the string associated with the argument referred to in the attribute's constructor (in this case, `condition`) at the call site.</span></span> <span data-ttu-id="6ab07-127">Pokud některý z tvrzení selže, vývojář se zobrazí podmínka, která byla nepravdivá, a bude mít za to, který z nich se nezdařil.</span><span class="sxs-lookup"><span data-stu-id="6ab07-127">When either assert fails, the developer will be shown the condition that was false and will know which one failed.</span></span>

<span data-ttu-id="6ab07-128">Pro ověřování argumentu se atribut nedá použít přímo, ale dá se použít prostřednictvím pomocné třídy:</span><span class="sxs-lookup"><span data-stu-id="6ab07-128">For argument validation, the attribute cannot be used directly, but can be made use of through a helper class:</span></span>

```csharp
public static class Verify
{
    public static void Argument(bool condition, string message, [CallerArgumentExpression("condition")] string conditionExpression = null)
    {
        if (!condition) throw new ArgumentException(message: message, paramName: conditionExpression);
    }

    public static void InRange(int argument, int low, int high,
        [CallerArgumentExpression("argument")] string argumentExpression = null,
        [CallerArgumentExpression("low")] string lowExpression = null,
        [CallerArgumentExpression("high")] string highExpression = null)
    {
        if (argument < low)
        {
            throw new ArgumentOutOfRangeException(paramName: argumentExpression,
                message: $"{argumentExpression} ({argument}) cannot be less than {lowExpression} ({low}).");
        }

        if (argument > high)
        {
            throw new ArgumentOutOfRangeException(paramName: argumentExpression,
                message: $"{argumentExpression} ({argument}) cannot be greater than {highExpression} ({high}).");
        }
    }

    public static void NotNull<T>(T argument, [CallerArgumentExpression("argument")] string argumentExpression = null)
        where T : class
    {
        if (argument == null) throw new ArgumentNullException(paramName: argumentExpression);
    }
}

T Single<T>(this T[] array)
{
    Verify.NotNull(array); // paramName: "array"
    Verify.Argument(array.Length == 1, "Array must contain a single element."); // paramName: "array.Length == 1"

    return array[0];
}

T ElementAt(this T[] array, int index)
{
    Verify.NotNull(array); // paramName: "array"
    // paramName: "index"
    // message: "index (-1) cannot be less than 0 (0).", or
    //          "index (6) cannot be greater than array.Length - 1 (5)."
    Verify.InRange(index, 0, array.Length - 1);

    return array[index];
}
```

<span data-ttu-id="6ab07-129">Návrh na přidání takové pomocné třídy do architektury probíhá na https://github.com/dotnet/corefx/issues/17068.</span><span class="sxs-lookup"><span data-stu-id="6ab07-129">A proposal to add such a helper class to the framework is underway at https://github.com/dotnet/corefx/issues/17068.</span></span> <span data-ttu-id="6ab07-130">Pokud byla tato funkce jazyka implementovaná, návrh se dá aktualizovat tak, aby tuto funkci využil.</span><span class="sxs-lookup"><span data-stu-id="6ab07-130">If this language feature was implemented, the proposal could be updated to take advantage of this feature.</span></span>

### <a name="extension-methods"></a><span data-ttu-id="6ab07-131">Metody rozšíření</span><span class="sxs-lookup"><span data-stu-id="6ab07-131">Extension methods</span></span>

<span data-ttu-id="6ab07-132">V parametru `this` v metodě rozšíření může být odkazováno pomocí `CallerArgumentExpression`.</span><span class="sxs-lookup"><span data-stu-id="6ab07-132">The `this` parameter in an extension method may be referenced by `CallerArgumentExpression`.</span></span> <span data-ttu-id="6ab07-133">Příklad:</span><span class="sxs-lookup"><span data-stu-id="6ab07-133">For example:</span></span>

```csharp
public static void ShouldBe<T>(this T @this, T expected, [CallerArgumentExpression("this")] string thisExpression = null) {}

contestant.Points.ShouldBe(1337); // thisExpression: "contestant.Points"
```

<span data-ttu-id="6ab07-134">`thisExpression` dostane výraz odpovídající objektu před tečkou.</span><span class="sxs-lookup"><span data-stu-id="6ab07-134">`thisExpression` will receive the expression corresponding to the object before the dot.</span></span> <span data-ttu-id="6ab07-135">Pokud je volána se syntaxí statické metody, např. `Ext.ShouldBe(contestant.Points, 1337)`, bude se chovat, jako by první parametr nebyl označen `this`.</span><span class="sxs-lookup"><span data-stu-id="6ab07-135">If it's called with static method syntax, e.g. `Ext.ShouldBe(contestant.Points, 1337)`, it will behave as if first parameter wasn't marked `this`.</span></span>

<span data-ttu-id="6ab07-136">Vždy by měl být výraz odpovídající parametru `this`.</span><span class="sxs-lookup"><span data-stu-id="6ab07-136">There should always be an expression corresponding to the `this` parameter.</span></span> <span data-ttu-id="6ab07-137">I v případě, že instance třídy volá metodu rozšíření sama o sobě, např. `this.Single()` z typu kolekce, `this` je vyžadovaná kompilátorem, aby se `"this"` předala.</span><span class="sxs-lookup"><span data-stu-id="6ab07-137">Even if an instance of a class calls an extension method on itself, e.g. `this.Single()` from inside a collection type, the `this` is mandated by the compiler so `"this"` will get passed.</span></span> <span data-ttu-id="6ab07-138">Pokud se toto pravidlo v budoucnu změní, můžeme zvážit předání `null` nebo prázdného řetězce.</span><span class="sxs-lookup"><span data-stu-id="6ab07-138">If this rule is changed in the future, we can consider passing `null` or the empty string.</span></span>

### <a name="extra-details"></a><span data-ttu-id="6ab07-139">Další podrobnosti</span><span class="sxs-lookup"><span data-stu-id="6ab07-139">Extra details</span></span>

- <span data-ttu-id="6ab07-140">Podobně jako u ostatních atributů `Caller*`, jako je například `CallerMemberName`, tento atribut lze použít pouze u parametrů s výchozími hodnotami.</span><span class="sxs-lookup"><span data-stu-id="6ab07-140">Like the other `Caller*` attributes, such as `CallerMemberName`, this attribute may only be used on parameters with default values.</span></span>
- <span data-ttu-id="6ab07-141">Je povoleno více parametrů označených pomocí `CallerArgumentExpression`, jak je uvedeno výše.</span><span class="sxs-lookup"><span data-stu-id="6ab07-141">Multiple parameters marked with `CallerArgumentExpression` are permitted, as shown above.</span></span>
- <span data-ttu-id="6ab07-142">Obor názvů atributu bude `System.Runtime.CompilerServices`.</span><span class="sxs-lookup"><span data-stu-id="6ab07-142">The attribute's namespace will be `System.Runtime.CompilerServices`.</span></span>
- <span data-ttu-id="6ab07-143">Pokud je k dispozici `null` nebo řetězec, který není názvem parametru (např. `"notAParameterName"`), kompilátor bude předávat prázdný řetězec.</span><span class="sxs-lookup"><span data-stu-id="6ab07-143">If `null` or a string that is not a parameter name (e.g. `"notAParameterName"`) is provided, the compiler will pass in an empty string.</span></span>

## <a name="drawbacks"></a><span data-ttu-id="6ab07-144">Nevýhody</span><span class="sxs-lookup"><span data-stu-id="6ab07-144">Drawbacks</span></span>
[drawbacks]: #drawbacks

- <span data-ttu-id="6ab07-145">Osoby, které vědí, jak použít dekompilátory, budou moci zobrazit některý zdrojový kód na webech volání metod označených tímto atributem.</span><span class="sxs-lookup"><span data-stu-id="6ab07-145">People who know how to use decompilers will be able to see some of the source code at call sites for methods marked with this attribute.</span></span> <span data-ttu-id="6ab07-146">Pro uzavřený software se může jednat o nežádoucí nebo neočekávané chování.</span><span class="sxs-lookup"><span data-stu-id="6ab07-146">This may be undesirable/unexpected for closed-source software.</span></span>

- <span data-ttu-id="6ab07-147">I když se nejedná o chybu samotné funkce, může to mít za následek to, že už existuje rozhraní `Debug.Assert` API, které v současné době používá jenom `bool`.</span><span class="sxs-lookup"><span data-stu-id="6ab07-147">Although this is not a flaw in the feature itself, a source of concern may be that there exists a `Debug.Assert` API today that only takes a `bool`.</span></span> <span data-ttu-id="6ab07-148">I v případě, že přetížení přebírající zprávu má svůj druhý parametr označený pomocí tohoto atributu a je povinná, kompilátor stále vybere zprávu No-Message One v řešení přetížení.</span><span class="sxs-lookup"><span data-stu-id="6ab07-148">Even if the overload taking a message had its second parameter marked with this attribute and made optional, the compiler would still pick the no-message one in overload resolution.</span></span> <span data-ttu-id="6ab07-149">Proto by bylo nutné odebrat přetížení bez zpráv, aby bylo možné tuto funkci využít, což je binární (i když ne zdroj) zásadní změna.</span><span class="sxs-lookup"><span data-stu-id="6ab07-149">Therefore, the no-message overload would have to be removed to take advantage of this feature, which would be a binary (although not source) breaking change.</span></span>

## <a name="alternatives"></a><span data-ttu-id="6ab07-150">Alternativy</span><span class="sxs-lookup"><span data-stu-id="6ab07-150">Alternatives</span></span>
[alternatives]: #alternatives

- <span data-ttu-id="6ab07-151">Pokud je schopen zobrazit zdrojový kód na webech volání metody, které používají tento atribut jako problém, můžeme nastavit, aby účinky tohoto atributu byly.</span><span class="sxs-lookup"><span data-stu-id="6ab07-151">If being able to see source code at call sites for methods that use this attribute proves to be a problem, we can make the attribute's effects opt-in.</span></span> <span data-ttu-id="6ab07-152">Vývojáři ji umožní prostřednictvím atributu `[assembly: EnableCallerArgumentExpression]` pro celé sestavení, které jsou vloženy do `AssemblyInfo.cs`.</span><span class="sxs-lookup"><span data-stu-id="6ab07-152">Developers will enable it through an assembly-wide `[assembly: EnableCallerArgumentExpression]` attribute they put in `AssemblyInfo.cs`.</span></span>
  - <span data-ttu-id="6ab07-153">V případě, že účinky atributu nejsou povoleny, volání metod označených atributem by nepředstavovala chybu, aby existující metody mohly použít atribut a udržovat kompatibilitu zdrojů.</span><span class="sxs-lookup"><span data-stu-id="6ab07-153">In the case the attribute's effects are not enabled, calling methods marked with the attribute would not be an error, to allow existing methods to use the attribute and maintain source compatibility.</span></span> <span data-ttu-id="6ab07-154">Atribut by však byl ignorován a metoda by byla volána s libovolnou výchozí hodnotou.</span><span class="sxs-lookup"><span data-stu-id="6ab07-154">However, the attribute would be ignored and the method would be called with whatever default value was provided.</span></span>

```csharp
// Assembly1

void Foo(string bar); // V1
void Foo(string bar, string barExpression = "not provided"); // V2
void Foo(string bar, [CallerArgumentExpression("bar")] string barExpression = "not provided"); // V3

// Assembly2

Foo(a); // V1: Compiles to Foo(a), V2, V3: Compiles to Foo(a, "not provided")
Foo(a, "provided"); // V2, V3: Compiles to Foo(a, "provided")

// Assembly3

[assembly: EnableCallerArgumentExpression]

Foo(a); // V1: Compiles to Foo(a), V2: Compiles to Foo(a, "not provided"), V3: Compiles to Foo(a, "a")
Foo(a, "provided"); // V2, V3: Compiles to Foo(a, "provided")
```

- <span data-ttu-id="6ab07-155">Aby nedošlo k[ drawbacks] [potížím s binární kompatibilitou] pokaždé, když chceme do `Debug.Assert`přidat nové informace o volajícím, může alternativní řešení přidat strukturu `CallerInfo` do rozhraní, které obsahuje všechny nezbytné informace o volajícím.</span><span class="sxs-lookup"><span data-stu-id="6ab07-155">To prevent the [binary compatibility problem][drawbacks] from occurring every time we want to add new caller info to `Debug.Assert`, an alternative solution would be to add a `CallerInfo` struct to the framework that contains all the necessary information about the caller.</span></span>

```csharp
struct CallerInfo
{
    public string MemberName { get; set; }
    public string TypeName { get; set; }
    public string Namespace { get; set; }
    public string FullTypeName { get; set; }
    public string FilePath { get; set; }
    public int LineNumber { get; set; }
    public int ColumnNumber { get; set; }
    public Type Type { get; set; }
    public MethodBase Method { get; set; }
    public string[] ArgumentExpressions { get; set; }
}

[Flags]
enum CallerInfoOptions
{
    MemberName = 1, TypeName = 2, ...
}

public static class Debug
{
    public static void Assert(bool condition,
        // If a flag is not set here, the corresponding CallerInfo member is not populated by the caller, so it's
        // pay-for-play friendly.
        [CallerInfo(CallerInfoOptions.FilePath | CallerInfoOptions.Method | CallerInfoOptions.ArgumentExpressions)] CallerInfo callerInfo = default(CallerInfo))
    {
        string filePath = callerInfo.FilePath;
        MethodBase method = callerInfo.Method;
        string conditionExpression = callerInfo.ArgumentExpressions[0];
        ...
    }
}

class Bar
{
    void Foo()
    {
        Debug.Assert(false);

        // Translates to:

        var callerInfo = new CallerInfo();
        callerInfo.FilePath = @"C:\Bar.cs";
        callerInfo.Method = MethodBase.GetCurrentMethod();
        callerInfo.ArgumentExpressions = new string[] { "false" };
        Debug.Assert(false, callerInfo);
    }
}
```

<span data-ttu-id="6ab07-156">Tento návrh byl původně navržený na https://github.com/dotnet/csharplang/issues/87.</span><span class="sxs-lookup"><span data-stu-id="6ab07-156">This was originally proposed at https://github.com/dotnet/csharplang/issues/87.</span></span>

<span data-ttu-id="6ab07-157">Tento přístup má několik nevýhody:</span><span class="sxs-lookup"><span data-stu-id="6ab07-157">There are a few disadvantages of this approach:</span></span>

- <span data-ttu-id="6ab07-158">Bez ohledu na to, jak je to možné, můžete určit, které vlastnosti budete potřebovat, ale přesto může snížit výkon tím, že přidělením pole pro výrazy nebo volání `MethodBase.GetCurrentMethod` i když kontrolní výraz projde.</span><span class="sxs-lookup"><span data-stu-id="6ab07-158">Despite being pay-for-play friendly by allowing you to specify which properties you need, it could still hurt perf significantly by allocating an array for the expressions/calling `MethodBase.GetCurrentMethod` even when the assert passes.</span></span>

- <span data-ttu-id="6ab07-159">Kromě toho při předávání nového příznaku `CallerInfo` atributu nebude zásadní změna, `Debug.Assert` nebude zaručeno, že ve skutečnosti obdrží tento nový parametr z webů volání, které jsou zkompilovány proti staré verzi metody.</span><span class="sxs-lookup"><span data-stu-id="6ab07-159">Additionally, while passing a new flag to the `CallerInfo` attribute won't be a breaking change, `Debug.Assert` won't be guaranteed to actually receive that new parameter from call sites that compiled against an old version of the method.</span></span>

## <a name="unresolved-questions"></a><span data-ttu-id="6ab07-160">Nevyřešené dotazy</span><span class="sxs-lookup"><span data-stu-id="6ab07-160">Unresolved questions</span></span>
[unresolved]: #unresolved-questions

<span data-ttu-id="6ab07-161">Bude doplněno</span><span class="sxs-lookup"><span data-stu-id="6ab07-161">TBD</span></span>

## <a name="design-meetings"></a><span data-ttu-id="6ab07-162">Schůzky pro návrh</span><span class="sxs-lookup"><span data-stu-id="6ab07-162">Design meetings</span></span>

<span data-ttu-id="6ab07-163">neuvedeno</span><span class="sxs-lookup"><span data-stu-id="6ab07-163">N/A</span></span>
