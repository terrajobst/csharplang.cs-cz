---
ms.openlocfilehash: fa3326bf69c83b6042b1db7b5567fd5c28d6f81a
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484522"
---
# <a name="callerargumentexpression"></a>CallerArgumentExpression

* Navrženo [x]
* [] Prototyp: Nezahájeno
* [] Implementace: Nezahájeno
* [] Specifikace: Nezahájeno

## <a name="summary"></a>Souhrn
[summary]: #summary

Umožněte vývojářům zachytit výrazy předané metodě, aby se povolily lepší chybové zprávy v rozhraních API pro diagnostiku a testování a aby se snížily úhozy.

## <a name="motivation"></a>Motivační
[motivation]: #motivation

Pokud se ověření kontrolního výrazu nebo argumentu nepovede, vývojář chce co nejvíce informovat o tom, kde a proč selhal. Nicméně současná rozhraní API pro diagnostiku to zcela neusnadňuje. Vezměte v úvahu následující metodu:

```csharp
T Single<T>(this T[] array)
{
    Debug.Assert(array != null);
    Debug.Assert(array.Length == 1);

    return array[0];
}
```

Pokud jeden z kontrolních výrazů selže, bude v trasování zásobníku k dispozici pouze název souboru, číslo řádku a název metody. Vývojář nebude moci zjistit, u kterého kontrolního výrazu došlo k chybě, a to tak, že bude muset otevřít soubor a přejít na zadané číslo řádku a zjistit, co se pokazilo.

Toto je také rozhraní pro testování důvodů, které musí poskytovat řadu metod vyhodnocení. Pomocí xUnit `Assert.True` a `Assert.False` se často nepoužívají, protože neposkytují dostatek kontextu o tom, co selhalo.

I když je tato situace pro ověřování argumentu lepší, protože vývojářům se zobrazují názvy neplatných argumentů, vývojář musí tyto názvy předat výjimkám ručně. Pokud byl výše uvedený příklad přepsaný pro použití tradičního ověřování argumentu místo `Debug.Assert`, by vypadal jako

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

Všimněte si, že `nameof(array)` musí být předány každé výjimce, i když již není jasné z kontextu, který je argument neplatný.

## <a name="detailed-design"></a>Podrobný návrh
[design]: #detailed-design

Ve výše uvedených příkladech, včetně řetězce `"array != null"` nebo `"array.Length == 1"` ve zprávě kontrolního výrazu, by mohl vývojář určit, co se nepovedlo. Zadejte `CallerArgumentExpression`: Jedná se o atribut, který může rozhraní použít k získání řetězce přidruženého k konkrétnímu argumentu metody. Přidáme ho do `Debug.Assert` například

```csharp
public static class Debug
{
    public static void Assert(bool condition, [CallerArgumentExpression("condition")] string message = null);
}
```

Zdrojový kód v předchozím příkladu by zůstal stejný. Nicméně kód, který kompilátor skutečně generuje, by odpovídal

```csharp
T Single<T>(this T[] array)
{
    Debug.Assert(array != null, "array != null");
    Debug.Assert(array.Length == 1, "array.Length == 1");

    return array[0];
}
```

Kompilátor speciálně rozpoznává atribut u `Debug.Assert`. Předá řetězec přidružený k argumentu uvedenému v konstruktoru atributu (v tomto případě `condition`) na webu volání. Pokud některý z tvrzení selže, vývojář se zobrazí podmínka, která byla nepravdivá, a bude mít za to, který z nich se nezdařil.

Pro ověřování argumentu se atribut nedá použít přímo, ale dá se použít prostřednictvím pomocné třídy:

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

Návrh na přidání takové pomocné třídy do architektury probíhá na https://github.com/dotnet/corefx/issues/17068. Pokud byla tato funkce jazyka implementovaná, návrh se dá aktualizovat tak, aby tuto funkci využil.

### <a name="extension-methods"></a>Metody rozšíření

V parametru `this` v metodě rozšíření může být odkazováno pomocí `CallerArgumentExpression`. Příklad:

```csharp
public static void ShouldBe<T>(this T @this, T expected, [CallerArgumentExpression("this")] string thisExpression = null) {}

contestant.Points.ShouldBe(1337); // thisExpression: "contestant.Points"
```

`thisExpression` dostane výraz odpovídající objektu před tečkou. Pokud je volána se syntaxí statické metody, např. `Ext.ShouldBe(contestant.Points, 1337)`, bude se chovat, jako by první parametr nebyl označen `this`.

Vždy by měl být výraz odpovídající parametru `this`. I v případě, že instance třídy volá metodu rozšíření sama o sobě, např. `this.Single()` z typu kolekce, `this` je vyžadovaná kompilátorem, aby se `"this"` předala. Pokud se toto pravidlo v budoucnu změní, můžeme zvážit předání `null` nebo prázdného řetězce.

### <a name="extra-details"></a>Další podrobnosti

- Podobně jako u ostatních atributů `Caller*`, jako je například `CallerMemberName`, tento atribut lze použít pouze u parametrů s výchozími hodnotami.
- Je povoleno více parametrů označených pomocí `CallerArgumentExpression`, jak je uvedeno výše.
- Obor názvů atributu bude `System.Runtime.CompilerServices`.
- Pokud je k dispozici `null` nebo řetězec, který není názvem parametru (např. `"notAParameterName"`), kompilátor bude předávat prázdný řetězec.

## <a name="drawbacks"></a>Nevýhody
[drawbacks]: #drawbacks

- Osoby, které vědí, jak použít dekompilátory, budou moci zobrazit některý zdrojový kód na webech volání metod označených tímto atributem. Pro uzavřený software se může jednat o nežádoucí nebo neočekávané chování.

- I když se nejedná o chybu samotné funkce, může to mít za následek to, že už existuje rozhraní `Debug.Assert` API, které v současné době používá jenom `bool`. I v případě, že přetížení přebírající zprávu má svůj druhý parametr označený pomocí tohoto atributu a je povinná, kompilátor stále vybere zprávu No-Message One v řešení přetížení. Proto by bylo nutné odebrat přetížení bez zpráv, aby bylo možné tuto funkci využít, což je binární (i když ne zdroj) zásadní změna.

## <a name="alternatives"></a>Alternativy
[alternatives]: #alternatives

- Pokud je schopen zobrazit zdrojový kód na webech volání metody, které používají tento atribut jako problém, můžeme nastavit, aby účinky tohoto atributu byly. Vývojáři ji umožní prostřednictvím atributu `[assembly: EnableCallerArgumentExpression]` pro celé sestavení, které jsou vloženy do `AssemblyInfo.cs`.
  - V případě, že účinky atributu nejsou povoleny, volání metod označených atributem by nepředstavovala chybu, aby existující metody mohly použít atribut a udržovat kompatibilitu zdrojů. Atribut by však byl ignorován a metoda by byla volána s libovolnou výchozí hodnotou.

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

- Aby nedošlo k[ drawbacks] [potížím s binární kompatibilitou] pokaždé, když chceme do `Debug.Assert`přidat nové informace o volajícím, může alternativní řešení přidat strukturu `CallerInfo` do rozhraní, které obsahuje všechny nezbytné informace o volajícím.

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

Tento návrh byl původně navržený na https://github.com/dotnet/csharplang/issues/87.

Tento přístup má několik nevýhody:

- Bez ohledu na to, jak je to možné, můžete určit, které vlastnosti budete potřebovat, ale přesto může snížit výkon tím, že přidělením pole pro výrazy nebo volání `MethodBase.GetCurrentMethod` i když kontrolní výraz projde.

- Kromě toho při předávání nového příznaku `CallerInfo` atributu nebude zásadní změna, `Debug.Assert` nebude zaručeno, že ve skutečnosti obdrží tento nový parametr z webů volání, které jsou zkompilovány proti staré verzi metody.

## <a name="unresolved-questions"></a>Nevyřešené dotazy
[unresolved]: #unresolved-questions

Bude doplněno

## <a name="design-meetings"></a>Schůzky pro návrh

neuvedeno
