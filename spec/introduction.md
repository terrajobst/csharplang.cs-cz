---
ms.openlocfilehash: db10046af5d635b430951679a448e23680b18b87
ms.sourcegitcommit: a19fac74c01a6c3da67d38b2f79527145d4edcbc
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/09/2019
ms.locfileid: "59426809"
---
# <a name="introduction"></a><span data-ttu-id="fb7c7-101">Úvod</span><span class="sxs-lookup"><span data-stu-id="fb7c7-101">Introduction</span></span>

<span data-ttu-id="fb7c7-102">C# (čteno "v tématu Sharp") je jednoduchý, moderní, objektově orientované a bezpečnost typů programovací jazyk.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-102">C# (pronounced "See Sharp") is a simple, modern, object-oriented, and type-safe programming language.</span></span> <span data-ttu-id="fb7c7-103">C# má jeho kořeny řady jazyků C a bude okamžitě znát programátory C, C++ nebo Java.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-103">C# has its roots in the C family of languages and will be immediately familiar to C, C++, and Java programmers.</span></span> <span data-ttu-id="fb7c7-104">C# je standardizované společnosti ECMA International jako ***ECMA 334*** standardní a ISO/IEC jako ***23270 ISO/IEC*** standard.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-104">C# is standardized by ECMA International as the ***ECMA-334*** standard and by ISO/IEC as the ***ISO/IEC 23270*** standard.</span></span> <span data-ttu-id="fb7c7-105">Společnosti Microsoft C# kompilátor pro rozhraní .NET Framework je vyhovující implementace obou těchto standardů.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-105">Microsoft's C# compiler for the .NET Framework is a conforming implementation of both of these standards.</span></span>

<span data-ttu-id="fb7c7-106">C# je objektově orientovaný jazyk, ale jazyka C# dále zahrnuje podporu pro ***komponenty objektově orientovaný*** programování.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-106">C# is an object-oriented language, but C# further includes support for ***component-oriented*** programming.</span></span> <span data-ttu-id="fb7c7-107">Návrh moderní softwaru stále spoléhá na softwarové komponenty v podobě samostatné a popisující samy sebe balíčky funkcí.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-107">Contemporary software design increasingly relies on software components in the form of self-contained and self-describing packages of functionality.</span></span> <span data-ttu-id="fb7c7-108">Klíčem k takové součásti je, že představují programovací model s vlastnosti, metody a události; mají atributy, které poskytují deklarativní informace o komponentě; a zahrnují vlastní dokumentace.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-108">Key to such components is that they present a programming model with properties, methods, and events; they have attributes that provide declarative information about the component; and they incorporate their own documentation.</span></span> <span data-ttu-id="fb7c7-109">C# zajišťuje, že jazyk konstrukce pro přímo podporují tyto koncepty, pak C# velmi přirozeného jazyka, ve kterém chcete vytvořit a používat softwarové součásti.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-109">C# provides language constructs to directly support these concepts, making C# a very natural language in which to create and use software components.</span></span>

<span data-ttu-id="fb7c7-110">Několik C# funkce podpory ve vytváření robustních a odolných aplikací: ***Uvolňování paměti*** automaticky uvolní paměť obsazena nevyužité objekty. ***zpracování výjimek*** přináší strukturovaných a rozšiřitelné přístup k detekce chyb a obnovení; a ***zajišťující bezpečnost typů*** návrh jazyka znemožňuje čtení z neinicializovaného proměnné k indexování pole, mimo jejich rozsah nebo k provádění zaškrtnuté políčko typu přetypování.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-110">Several C# features aid in the construction of robust and durable applications: ***Garbage collection*** automatically reclaims memory occupied by unused objects; ***exception handling*** provides a structured and extensible approach to error detection and recovery; and the ***type-safe*** design of the language makes it impossible to read from uninitialized variables, to index arrays beyond their bounds, or to perform unchecked type casts.</span></span>

<span data-ttu-id="fb7c7-111">C# má ***unified systém typů***.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-111">C# has a ***unified type system***.</span></span> <span data-ttu-id="fb7c7-112">Všechny typy C#, včetně primitivní typy, jako například `int` a `double`, dědit z jednoho kořene `object` typu.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-112">All C# types, including primitive types such as `int` and `double`, inherit from a single root `object` type.</span></span> <span data-ttu-id="fb7c7-113">Proto sdílejí sadu běžných operací pro všechny typy a hodnoty libovolného typu lze ukládat, přenášet a provozován konzistentním způsobem.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-113">Thus, all types share a set of common operations, and values of any type can be stored, transported, and operated upon in a consistent manner.</span></span> <span data-ttu-id="fb7c7-114">Kromě toho C# podporuje uživatelem definované referenční typy a typy hodnot, povolení dynamické přidělování objektů a úložiště v řádku zjednodušené struktur.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-114">Furthermore, C# supports both user-defined reference types and value types, allowing dynamic allocation of objects as well as in-line storage of lightweight structures.</span></span>

<span data-ttu-id="fb7c7-115">Pokud chcete mít jistotu, že aplikace C# a knihovny může v průběhu času vyvíjejí kompatibilní způsobem, se nachází mnoho důraz na ***správy verzí*** v C# pro návrh.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-115">To ensure that C# programs and libraries can evolve over time in a compatible manner, much emphasis has been placed on ***versioning*** in C#'s design.</span></span> <span data-ttu-id="fb7c7-116">Mnoho programovacích jazyků věnovat trochu tento problém, a v důsledku toho jsou zavedeny programy napsané v těchto jazyků přerušení častěji, než pokud je novější verze závislé knihovny.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-116">Many programming languages pay little attention to this issue, and, as a result, programs written in those languages break more often than necessary when newer versions of dependent libraries are introduced.</span></span> <span data-ttu-id="fb7c7-117">Aspekty návrhu v C#, které byly přímo ovlivňován aspekty správy verzí jsou jako samostatná `virtual` a `override` modifikátory, pravidla pro řešení přetížení metody a podpora deklarací členů explicitního rozhraní.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-117">Aspects of C#'s design that were directly influenced by versioning considerations include the separate `virtual` and `override` modifiers, the rules for method overload resolution, and support for explicit interface member declarations.</span></span>

<span data-ttu-id="fb7c7-118">Zbývající část tato kapitola popisuje základní funkce jazyka C#.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-118">The rest of this chapter describes the essential features of the C# language.</span></span> <span data-ttu-id="fb7c7-119">I když dalších kapitolách popisují orientované na podrobnosti a někdy matematické způsobem pravidel a výjimek, tato kapitola usiluje jasné a zkrácení za cenu úplnost.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-119">Although later chapters describe rules and exceptions in a detail-oriented and sometimes mathematical manner, this chapter strives for clarity and brevity at the expense of completeness.</span></span> <span data-ttu-id="fb7c7-120">Účelem je poskytnout Úvod do jazyka, který usnadní psaní programů časná a čtení dalších kapitolách čtecí modul.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-120">The intent is to provide the reader with an introduction to the language that will facilitate the writing of early programs and the reading of later chapters.</span></span>

## <a name="hello-world"></a><span data-ttu-id="fb7c7-121">Ahoj světe</span><span class="sxs-lookup"><span data-stu-id="fb7c7-121">Hello world</span></span>

<span data-ttu-id="fb7c7-122">Program "Hello, World" tradičně slouží k uvození programovací jazyk.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-122">The "Hello, World" program is traditionally used to introduce a programming language.</span></span> <span data-ttu-id="fb7c7-123">Tady je v jazyce C#:</span><span class="sxs-lookup"><span data-stu-id="fb7c7-123">Here it is in C#:</span></span>

```csharp
using System;

class Hello
{
    static void Main() {
        Console.WriteLine("Hello, World");
    }
}
```

<span data-ttu-id="fb7c7-124">Zdrojové soubory jazyka C# obvykle mají příponu `.cs`.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-124">C# source files typically have the file extension `.cs`.</span></span> <span data-ttu-id="fb7c7-125">Za předpokladu, že program "Hello, World" je uložen v souboru `hello.cs`, může být program zkompilován s pomocí příkazového řádku kompilátoru jazyka Microsoft C#</span><span class="sxs-lookup"><span data-stu-id="fb7c7-125">Assuming that the "Hello, World" program is stored in the file `hello.cs`, the program can be compiled with the Microsoft C# compiler using the command line</span></span>
```
csc hello.cs
```
<span data-ttu-id="fb7c7-126">produkuje spustitelného sestavení s názvem `hello.exe`.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-126">which produces an executable assembly named `hello.exe`.</span></span> <span data-ttu-id="fb7c7-127">Je výstup vytvořeného touto aplikací při spuštění</span><span class="sxs-lookup"><span data-stu-id="fb7c7-127">The output produced by this application when it is run is</span></span>
```
Hello, World
```

<span data-ttu-id="fb7c7-128">Program "Hello, World" začíná `using` direktiva, která odkazuje `System` oboru názvů.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-128">The "Hello, World" program starts with a `using` directive that references the `System` namespace.</span></span> <span data-ttu-id="fb7c7-129">Obory názvů umožňují hierarchické uspořádání programy jazyka C# a knihovny.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-129">Namespaces provide a hierarchical means of organizing C# programs and libraries.</span></span> <span data-ttu-id="fb7c7-130">Obory názvů obsahují typy a jiných oborech názvů – například `System` obor názvů obsahuje několik typů, například `Console` třída odkazovaná v programu a několik jiných oborech názvů, jako například `IO` a `Collections`.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-130">Namespaces contain types and other namespaces—for example, the `System` namespace contains a number of types, such as the `Console` class referenced in the program, and a number of other namespaces, such as `IO` and `Collections`.</span></span> <span data-ttu-id="fb7c7-131">A `using` umožňuje direktiva, která odkazuje na daný obor názvů nekvalifikované použití typů, které jsou členy tohoto oboru názvů.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-131">A `using` directive that references a given namespace enables unqualified use of the types that are members of that namespace.</span></span> <span data-ttu-id="fb7c7-132">Z důvodu `using` direktiv, můžete použít program `Console.WriteLine` jako zkratka pro `System.Console.WriteLine`.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-132">Because of the `using` directive, the program can use `Console.WriteLine` as shorthand for `System.Console.WriteLine`.</span></span>

<span data-ttu-id="fb7c7-133">`Hello` Třídy deklarované jako programem "Hello, World" obsahuje jeden člen metodu s názvem `Main`.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-133">The `Hello` class declared by the "Hello, World" program has a single member, the method named `Main`.</span></span> <span data-ttu-id="fb7c7-134">`Main` Metoda je deklarována s `static` modifikátor.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-134">The `Main` method is declared with the `static` modifier.</span></span> <span data-ttu-id="fb7c7-135">Zatímco instanční metody může odkazovat na konkrétní nadřazeného objektu instanci pomocí klíčového slova `this`, statické metody fungovat bez ohledu na konkrétní objekt.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-135">While instance methods can reference a particular enclosing object instance using the keyword `this`, static methods operate without reference to a particular object.</span></span> <span data-ttu-id="fb7c7-136">Podle konvence statickou metodu s názvem `Main` slouží jako vstupní bod programu.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-136">By convention, a static method named `Main` serves as the entry point of a program.</span></span>

<span data-ttu-id="fb7c7-137">Výstup programu je vytvořen `WriteLine` metodu `Console` třídy v `System` oboru názvů.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-137">The output of the program is produced by the `WriteLine` method of the `Console` class in the `System` namespace.</span></span> <span data-ttu-id="fb7c7-138">Tato třída poskytuje knihovny tříd rozhraní .NET Framework, které ve výchozím nastavení, jsou automaticky odkazována pomocí kompilátoru jazyka Microsoft C#.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-138">This class is provided by the .NET Framework class libraries, which, by default, are automatically referenced by the Microsoft C# compiler.</span></span> <span data-ttu-id="fb7c7-139">Všimněte si, že C# sám nemá samostatné běhové knihovny.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-139">Note that C# itself does not have a separate runtime library.</span></span> <span data-ttu-id="fb7c7-140">Místo toho rozhraní .NET Framework je knihovna runtime jazyka C#.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-140">Instead, the .NET Framework is the runtime library of C#.</span></span>

## <a name="program-structure"></a><span data-ttu-id="fb7c7-141">Struktura programu</span><span class="sxs-lookup"><span data-stu-id="fb7c7-141">Program structure</span></span>

<span data-ttu-id="fb7c7-142">Klíčové koncepty organizace v jazyce C# jsou ***programy***, ***obory názvů***, ***typy***, ***členy***, a ***sestavení***.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-142">The key organizational concepts in C# are ***programs***, ***namespaces***, ***types***, ***members***, and ***assemblies***.</span></span> <span data-ttu-id="fb7c7-143">Programy jazyka C# se skládají z jednoho nebo více zdrojových souborů.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-143">C# programs consist of one or more source files.</span></span> <span data-ttu-id="fb7c7-144">Programy deklarovat typy, které obsahují členy a mohou být uspořádány do oborů názvů.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-144">Programs declare types, which contain members and can be organized into namespaces.</span></span> <span data-ttu-id="fb7c7-145">Třídy a rozhraní jsou příklady typů.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-145">Classes and interfaces are examples of types.</span></span> <span data-ttu-id="fb7c7-146">Pole, metody, vlastnosti a události jsou příklady členů.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-146">Fields, methods, properties, and events are examples of members.</span></span> <span data-ttu-id="fb7c7-147">Když jsou kompilovány programy jazyka C#, jsou fyzicky zabaleny do sestavení.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-147">When C# programs are compiled, they are physically packaged into assemblies.</span></span> <span data-ttu-id="fb7c7-148">Sestavení obvykle mít příponu souboru `.exe` nebo `.dll`, v závislosti na tom, jestli je implementovat ***aplikací*** nebo ***knihovny***.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-148">Assemblies typically have the file extension `.exe` or `.dll`, depending on whether they implement ***applications*** or ***libraries***.</span></span>

<span data-ttu-id="fb7c7-149">V příkladu</span><span class="sxs-lookup"><span data-stu-id="fb7c7-149">The example</span></span>

```csharp
using System;

namespace Acme.Collections
{
    public class Stack
    {
        Entry top;

        public void Push(object data) {
            top = new Entry(top, data);
        }

        public object Pop() {
            if (top == null) throw new InvalidOperationException();
            object result = top.data;
            top = top.next;
            return result;
        }

        class Entry
        {
            public Entry next;
            public object data;
    
            public Entry(Entry next, object data) {
                this.next = next;
                this.data = data;
            }
        }
    }
}
```
<span data-ttu-id="fb7c7-150">deklaruje třídu s názvem `Stack` v obor názvů s názvem `Acme.Collections`.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-150">declares a class named `Stack` in a namespace called `Acme.Collections`.</span></span> <span data-ttu-id="fb7c7-151">Plně kvalifikovaný název této třídy je `Acme.Collections.Stack`.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-151">The fully qualified name of this class is `Acme.Collections.Stack`.</span></span> <span data-ttu-id="fb7c7-152">Třída obsahuje několik členů: pole s názvem `top`, dvě metody s názvem `Push` a `Pop`a vnořené třídy s názvem `Entry`.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-152">The class contains several members: a field named `top`, two methods named `Push` and `Pop`, and a nested class named `Entry`.</span></span> <span data-ttu-id="fb7c7-153">`Entry` Další třídy obsahuje tři členy: pole s názvem `next`, pole s názvem `data`a konstruktor.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-153">The `Entry` class further contains three members: a field named `next`, a field named `data`, and a constructor.</span></span> <span data-ttu-id="fb7c7-154">Za předpokladu, že zdrojový kód v příkladu je uložen v souboru `acme.cs`, příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="fb7c7-154">Assuming that the source code of the example is stored in the file `acme.cs`, the command line</span></span>

```
csc /t:library acme.cs
```
<span data-ttu-id="fb7c7-155">zkompiluje v příkladu jako knihovna (bez kódu `Main` vstupního bodu) a vytváří sestavení s názvem `acme.dll`.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-155">compiles the example as a library (code without a `Main` entry point) and produces an assembly named `acme.dll`.</span></span>

<span data-ttu-id="fb7c7-156">Sestavení obsahující spustitelný kód ve formě ***Intermediate Language*** instrukcí (IL) a symbolické informace ve formě ***metadat***.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-156">Assemblies contain executable code in the form of ***Intermediate Language*** (IL) instructions, and symbolic information in the form of ***metadata***.</span></span> <span data-ttu-id="fb7c7-157">Předtím, než se spustí, kód IL v sestavení automaticky převést do kódu specifického pro procesor kompilátorem za běhu (JIT) modulu .NET Common Language Runtime.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-157">Before it is executed, the IL code in an assembly is automatically converted to processor-specific code by the Just-In-Time (JIT) compiler of .NET Common Language Runtime.</span></span>

<span data-ttu-id="fb7c7-158">Protože sestavení je jednotka funkce obsahující kód a metadata popisující samy sebe, není nutné pro `#include` direktivy a soubory hlaviček v jazyce C#.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-158">Because an assembly is a self-describing unit of functionality containing both code and metadata, there is no need for `#include` directives and header files in C#.</span></span> <span data-ttu-id="fb7c7-159">Veřejné typy a členy, které jsou obsažené v konkrétní sestavení jsou k dispozici v programu v jazyce C# jednoduše tak, že odkazující na toto sestavení při kompilaci programu.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-159">The public types and members contained in a particular assembly are made available in a C# program simply by referencing that assembly when compiling the program.</span></span> <span data-ttu-id="fb7c7-160">Například používá tento program `Acme.Collections.Stack` třídy z `acme.dll` sestavení:</span><span class="sxs-lookup"><span data-stu-id="fb7c7-160">For example, this program uses the `Acme.Collections.Stack` class from the `acme.dll` assembly:</span></span>

```csharp
using System;
using Acme.Collections;

class Test
{
    static void Main() {
        Stack s = new Stack();
        s.Push(1);
        s.Push(10);
        s.Push(100);
        Console.WriteLine(s.Pop());
        Console.WriteLine(s.Pop());
        Console.WriteLine(s.Pop());
    }
}
```
<span data-ttu-id="fb7c7-161">Pokud program je uložený v souboru `test.cs`, když `test.cs` kompilaci, `acme.dll` sestavení lze odkazovat pomocí kompilátoru `/r` možnost:</span><span class="sxs-lookup"><span data-stu-id="fb7c7-161">If the program is stored in the file `test.cs`, when `test.cs` is compiled, the `acme.dll` assembly can be referenced using the compiler's `/r` option:</span></span>

```
csc /r:acme.dll test.cs
```
<span data-ttu-id="fb7c7-162">Tím se vytvoří sestavení spustitelného souboru s názvem `test.exe`, který při spuštění vytvoří výstup:</span><span class="sxs-lookup"><span data-stu-id="fb7c7-162">This creates an executable assembly named `test.exe`, which, when run, produces the output:</span></span>

```
100
10
1
```
<span data-ttu-id="fb7c7-163">C# umožňuje text zdrojového programu, který bude uložen do několika zdrojových souborů.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-163">C# permits the source text of a program to be stored in several source files.</span></span> <span data-ttu-id="fb7c7-164">Když je zkompilován více soubory programu v C#, jsou společně zpracovány všechny zdrojové soubory a zdrojové soubory můžete odkazovat na volně mezi sebou, koncepčně, je, jako kdyby byly všechny zdrojové soubory zřetězeny do jednoho velkého souboru před zpracováním.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-164">When a multi-file C# program is compiled, all of the source files are processed together, and the source files can freely reference each other—conceptually, it is as if all the source files were concatenated into one large file before being processed.</span></span> <span data-ttu-id="fb7c7-165">Dopředné deklarace jsou potřeba nikdy v jazyce C#, protože s několika málo výjimkami je deklarace pořadí je neplatné.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-165">Forward declarations are never needed in C# because, with very few exceptions, declaration order is insignificant.</span></span> <span data-ttu-id="fb7c7-166">C# neomezuje zdrojový soubor k deklarování pouze jeden veřejný typ taky je potřeba, aby název zdrojového souboru tak, aby odpovídaly typu deklarovaného ve zdrojovém souboru.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-166">C# does not limit a source file to declaring only one public type nor does it require the name of the source file to match a type declared in the source file.</span></span>

## <a name="types-and-variables"></a><span data-ttu-id="fb7c7-167">Typy a proměnné</span><span class="sxs-lookup"><span data-stu-id="fb7c7-167">Types and variables</span></span>

<span data-ttu-id="fb7c7-168">Existují dva druhy typů v jazyce C#: ***typů hodnot*** a ***referenční typy***.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-168">There are two kinds of types in C#: ***value types*** and ***reference types***.</span></span> <span data-ttu-id="fb7c7-169">Proměnné typů hodnoty přímo obsahovat svá data, zatímco proměnné typů odkazu ukládají odkazy na svá data, ten se označuje jako objekty.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-169">Variables of value types directly contain their data whereas variables of reference types store references to their data, the latter being known as objects.</span></span> <span data-ttu-id="fb7c7-170">V případě typů odkazu je možné pro dvě proměnné odkazovat na stejný objekt a proto je možné pro operace v rámci jedné proměnné ovlivňovat objekt odkazovaný jinou proměnnou.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-170">With reference types, it is possible for two variables to reference the same object and thus possible for operations on one variable to affect the object referenced by the other variable.</span></span> <span data-ttu-id="fb7c7-171">S typy hodnot proměnných každý vlastní kopii dat a není možné pro operace se na nich se má vliv na jinou (s výjimkou v případě třídy `ref` a `out` proměnných parametrů).</span><span class="sxs-lookup"><span data-stu-id="fb7c7-171">With value types, the variables each have their own copy of the data, and it is not possible for operations on one to affect the other (except in the case of `ref` and `out` parameter variables).</span></span>

<span data-ttu-id="fb7c7-172">C# pro typy hodnot se dále dělí do ***jednoduché typy***, ***typy výčtu***, ***typy struktury***, a ***typy připouštějící hodnotu Null***a referenční dokumentace jazyka C#. typy se dále dělí do ***typy tříd***, ***typy rozhraní***, ***pole typů***, a ***typy delegátů***.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-172">C#'s value types are further divided into ***simple types***, ***enum types***, ***struct types***, and ***nullable types***, and C#'s reference types are further divided into ***class types***, ***interface types***, ***array types***, and ***delegate types***.</span></span>

<span data-ttu-id="fb7c7-173">Následující tabulka obsahuje přehled o typu systému C#.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-173">The following table provides an overview of C#'s type system.</span></span>

| __<span data-ttu-id="fb7c7-174">Kategorie</span><span class="sxs-lookup"><span data-stu-id="fb7c7-174">Category</span></span>__    |                 | __<span data-ttu-id="fb7c7-175">Popis</span><span class="sxs-lookup"><span data-stu-id="fb7c7-175">Description</span></span>__ |
|-----------------|-----------------|-----------------|
| <span data-ttu-id="fb7c7-176">Typy hodnot</span><span class="sxs-lookup"><span data-stu-id="fb7c7-176">Value types</span></span>     | <span data-ttu-id="fb7c7-177">Jednoduché typy</span><span class="sxs-lookup"><span data-stu-id="fb7c7-177">Simple types</span></span>    | <span data-ttu-id="fb7c7-178">Podepsané celé číslo: `sbyte`, `short`, `int`,</span><span class="sxs-lookup"><span data-stu-id="fb7c7-178">Signed integral: `sbyte`, `short`, `int`,</span></span> `long` |
|                 |                 | <span data-ttu-id="fb7c7-179">Celočíselný typ bez znaménka: `byte`, `ushort`, `uint`,</span><span class="sxs-lookup"><span data-stu-id="fb7c7-179">Unsigned integral: `byte`, `ushort`, `uint`,</span></span> `ulong` |
|                 |                 | <span data-ttu-id="fb7c7-180">Znaky Unicode:</span><span class="sxs-lookup"><span data-stu-id="fb7c7-180">Unicode characters:</span></span> `char` |
|                 |                 | <span data-ttu-id="fb7c7-181">S plovoucí desetinnou čárkou IEEE: `float`,</span><span class="sxs-lookup"><span data-stu-id="fb7c7-181">IEEE floating point: `float`,</span></span> `double` |
|                 |                 | <span data-ttu-id="fb7c7-182">Desetinná přesnost vysoce:</span><span class="sxs-lookup"><span data-stu-id="fb7c7-182">High-precision decimal:</span></span> `decimal` |
|                 |                 | <span data-ttu-id="fb7c7-183">Logická hodnota:</span><span class="sxs-lookup"><span data-stu-id="fb7c7-183">Boolean:</span></span> `bool` |
|                 | <span data-ttu-id="fb7c7-184">Výčtové typy</span><span class="sxs-lookup"><span data-stu-id="fb7c7-184">Enum types</span></span>      | <span data-ttu-id="fb7c7-185">Uživatelem definované typy formuláře</span><span class="sxs-lookup"><span data-stu-id="fb7c7-185">User-defined types of the form</span></span> `enum E {...}` |
|                 | <span data-ttu-id="fb7c7-186">Typy struktury</span><span class="sxs-lookup"><span data-stu-id="fb7c7-186">Struct types</span></span>    | <span data-ttu-id="fb7c7-187">Uživatelem definované typy formuláře</span><span class="sxs-lookup"><span data-stu-id="fb7c7-187">User-defined types of the form</span></span> `struct S {...}` |
|                 | <span data-ttu-id="fb7c7-188">Typy s povolenou hodnotou Null</span><span class="sxs-lookup"><span data-stu-id="fb7c7-188">Nullable types</span></span>  | <span data-ttu-id="fb7c7-189">Rozšíření všechny ostatní typy hodnot s `null` hodnota</span><span class="sxs-lookup"><span data-stu-id="fb7c7-189">Extensions of all other value types with a `null` value</span></span> |
| <span data-ttu-id="fb7c7-190">Odkazové typy</span><span class="sxs-lookup"><span data-stu-id="fb7c7-190">Reference types</span></span> | <span data-ttu-id="fb7c7-191">Typy tříd</span><span class="sxs-lookup"><span data-stu-id="fb7c7-191">Class types</span></span>     | <span data-ttu-id="fb7c7-192">Ultimate základní třídy pro všechny ostatní typy:</span><span class="sxs-lookup"><span data-stu-id="fb7c7-192">Ultimate base class of all other types:</span></span> `object` |
|                 |                 | <span data-ttu-id="fb7c7-193">Řetězce Unicode:</span><span class="sxs-lookup"><span data-stu-id="fb7c7-193">Unicode strings:</span></span> `string` |
|                 |                 | <span data-ttu-id="fb7c7-194">Uživatelem definované typy formuláře</span><span class="sxs-lookup"><span data-stu-id="fb7c7-194">User-defined types of the form</span></span> `class C {...}` |
|                 | <span data-ttu-id="fb7c7-195">Typy rozhraní</span><span class="sxs-lookup"><span data-stu-id="fb7c7-195">Interface types</span></span> | <span data-ttu-id="fb7c7-196">Uživatelem definované typy formuláře</span><span class="sxs-lookup"><span data-stu-id="fb7c7-196">User-defined types of the form</span></span> `interface I {...}` |
|                 | <span data-ttu-id="fb7c7-197">Typy polí</span><span class="sxs-lookup"><span data-stu-id="fb7c7-197">Array types</span></span>     | <span data-ttu-id="fb7c7-198">Jeden – a s multidimenzionálním, například `int[]` a</span><span class="sxs-lookup"><span data-stu-id="fb7c7-198">Single- and multi-dimensional, for example, `int[]` and</span></span> `int[,]` |
|                 | <span data-ttu-id="fb7c7-199">Typy delegátů</span><span class="sxs-lookup"><span data-stu-id="fb7c7-199">Delegate types</span></span>  | <span data-ttu-id="fb7c7-200">Uživatelem definované typy ve formátu například</span><span class="sxs-lookup"><span data-stu-id="fb7c7-200">User-defined types of the form e.g.</span></span> `delegate int  D(...)` |

<span data-ttu-id="fb7c7-201">Osm integrální typy poskytují podporu pro hodnoty 8bitové, 16 bitů, 32bitová verze a 64bitová verze v podobě nebo bez znaménka.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-201">The eight integral types provide support for 8-bit, 16-bit, 32-bit, and 64-bit values in signed or unsigned form.</span></span>

<span data-ttu-id="fb7c7-202">Dvě s plovoucí desetinnou čárkou bodu typy, `float` a `double`, jsou reprezentovány pomocí 32-bit jednoduchou přesností a 64bitové dvojitou přesností IEEE 754 formátů.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-202">The two floating point types, `float` and `double`, are represented using the 32-bit single-precision and 64-bit double-precision IEEE 754 formats.</span></span>

<span data-ttu-id="fb7c7-203">`decimal` Typ je typ 128bitových dat. vhodný pro výpočty finančních a přepočty měn.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-203">The `decimal` type is a 128-bit data type suitable for financial and monetary calculations.</span></span>

<span data-ttu-id="fb7c7-204">C# `bool` typ se používá k reprezentaci logické hodnoty – hodnoty, které jsou buď `true` nebo `false`.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-204">C#'s `bool` type is used to represent boolean values—values that are either `true` or `false`.</span></span>

<span data-ttu-id="fb7c7-205">Znakové a řetězcové zpracování v jazyce C# používá kódování Unicode.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-205">Character and string processing in C# uses Unicode encoding.</span></span> <span data-ttu-id="fb7c7-206">`char` Typ představuje jednotku kódu UTF-16 a `string` typ představuje posloupnost jednotkami kódu kódování UTF-16.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-206">The `char` type represents a UTF-16 code unit, and the `string` type represents a sequence of UTF-16 code units.</span></span>

<span data-ttu-id="fb7c7-207">Následující tabulka shrnuje C# pro číselné typy.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-207">The following table summarizes C#'s numeric types.</span></span>


| __<span data-ttu-id="fb7c7-208">Kategorie</span><span class="sxs-lookup"><span data-stu-id="fb7c7-208">Category</span></span>__      | __<span data-ttu-id="fb7c7-209">Bity</span><span class="sxs-lookup"><span data-stu-id="fb7c7-209">Bits</span></span>__ | __<span data-ttu-id="fb7c7-210">Type</span><span class="sxs-lookup"><span data-stu-id="fb7c7-210">Type</span></span>__  | __<span data-ttu-id="fb7c7-211">Rozsah a přesnost</span><span class="sxs-lookup"><span data-stu-id="fb7c7-211">Range/Precision</span></span>__ |
|-------------------|----------|-----------|---------------------|
| <span data-ttu-id="fb7c7-212">Podepsané celé číslo</span><span class="sxs-lookup"><span data-stu-id="fb7c7-212">Signed integral</span></span>   | <span data-ttu-id="fb7c7-213">8</span><span class="sxs-lookup"><span data-stu-id="fb7c7-213">8</span></span>        | `sbyte`   | <span data-ttu-id="fb7c7-214">-128...127</span><span class="sxs-lookup"><span data-stu-id="fb7c7-214">-128...127</span></span> |
|                   | <span data-ttu-id="fb7c7-215">16</span><span class="sxs-lookup"><span data-stu-id="fb7c7-215">16</span></span>       | `short`   | <span data-ttu-id="fb7c7-216">-32,768...32,767</span><span class="sxs-lookup"><span data-stu-id="fb7c7-216">-32,768...32,767</span></span> |
|                   | <span data-ttu-id="fb7c7-217">32</span><span class="sxs-lookup"><span data-stu-id="fb7c7-217">32</span></span>       | `int`     | <span data-ttu-id="fb7c7-218">-2,147,483,648...2,147,483,647</span><span class="sxs-lookup"><span data-stu-id="fb7c7-218">-2,147,483,648...2,147,483,647</span></span> |
|                   | <span data-ttu-id="fb7c7-219">64</span><span class="sxs-lookup"><span data-stu-id="fb7c7-219">64</span></span>       | `long`    | <span data-ttu-id="fb7c7-220">-9,223,372,036,854,775,808...9,223,372,036,854,775,807</span><span class="sxs-lookup"><span data-stu-id="fb7c7-220">-9,223,372,036,854,775,808...9,223,372,036,854,775,807</span></span> |
| <span data-ttu-id="fb7c7-221">Celočíselný typ bez znaménka</span><span class="sxs-lookup"><span data-stu-id="fb7c7-221">Unsigned integral</span></span> | <span data-ttu-id="fb7c7-222">8</span><span class="sxs-lookup"><span data-stu-id="fb7c7-222">8</span></span>        | `byte`    | <span data-ttu-id="fb7c7-223">0...255</span><span class="sxs-lookup"><span data-stu-id="fb7c7-223">0...255</span></span> |
|                   | <span data-ttu-id="fb7c7-224">16</span><span class="sxs-lookup"><span data-stu-id="fb7c7-224">16</span></span>       | `ushort`  | <span data-ttu-id="fb7c7-225">0...65,535</span><span class="sxs-lookup"><span data-stu-id="fb7c7-225">0...65,535</span></span> |
|                   | <span data-ttu-id="fb7c7-226">32</span><span class="sxs-lookup"><span data-stu-id="fb7c7-226">32</span></span>       | `uint`    | <span data-ttu-id="fb7c7-227">0...4,294,967,295</span><span class="sxs-lookup"><span data-stu-id="fb7c7-227">0...4,294,967,295</span></span> |
|                   | <span data-ttu-id="fb7c7-228">64</span><span class="sxs-lookup"><span data-stu-id="fb7c7-228">64</span></span>       | `ulong`   | <span data-ttu-id="fb7c7-229">0...18,446,744,073,709,551,615</span><span class="sxs-lookup"><span data-stu-id="fb7c7-229">0...18,446,744,073,709,551,615</span></span> |
| <span data-ttu-id="fb7c7-230">Číslo s plovoucí desetinnou čárkou</span><span class="sxs-lookup"><span data-stu-id="fb7c7-230">Floating point</span></span>    | <span data-ttu-id="fb7c7-231">32</span><span class="sxs-lookup"><span data-stu-id="fb7c7-231">32</span></span>       | `float`   | <span data-ttu-id="fb7c7-232">1,5 × 10 ^ −45 3.4 × 10 ^ 38, 7 číslicemi přesnosti</span><span class="sxs-lookup"><span data-stu-id="fb7c7-232">1.5 × 10^−45 to 3.4 × 10^38, 7-digit precision</span></span> |
|                   | <span data-ttu-id="fb7c7-233">64</span><span class="sxs-lookup"><span data-stu-id="fb7c7-233">64</span></span>       | `double`  | <span data-ttu-id="fb7c7-234">5.0 × 10 ^ −324 1.7 × 10 ^ 308, 15 číslicemi přesnosti</span><span class="sxs-lookup"><span data-stu-id="fb7c7-234">5.0 × 10^−324 to 1.7 × 10^308, 15-digit precision</span></span> |
| <span data-ttu-id="fb7c7-235">Desetinné číslo</span><span class="sxs-lookup"><span data-stu-id="fb7c7-235">Decimal</span></span>           | <span data-ttu-id="fb7c7-236">128</span><span class="sxs-lookup"><span data-stu-id="fb7c7-236">128</span></span>      | `decimal` | <span data-ttu-id="fb7c7-237">1.0 × 10 ^ −28 7.9 × 10 ^ 28, 28 číslicemi přesnosti</span><span class="sxs-lookup"><span data-stu-id="fb7c7-237">1.0 × 10^−28 to 7.9 × 10^28, 28-digit precision</span></span> |

<span data-ttu-id="fb7c7-238">C# programy používají ***typ deklarace*** pro vytvoření nových typů.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-238">C# programs use ***type declarations*** to create new types.</span></span> <span data-ttu-id="fb7c7-239">Deklarace typu Určuje název a členy nového typu.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-239">A type declaration specifies the name and the members of the new type.</span></span> <span data-ttu-id="fb7c7-240">Pět C# pro kategorie typů jsou definovaných uživatelem: Třída typy, typy struktury, rozhraní typy, výčtové typy a typy delegátů.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-240">Five of C#'s categories of types are user-definable: class types, struct types, interface types, enum types, and delegate types.</span></span>

<span data-ttu-id="fb7c7-241">Typ třídy definuje datová struktura, která obsahuje data (pole) a funkční členy (metody, vlastnosti a ostatní).</span><span class="sxs-lookup"><span data-stu-id="fb7c7-241">A class type defines a data structure that contains data members (fields) and function members (methods, properties, and others).</span></span> <span data-ttu-id="fb7c7-242">Typy tříd podporují jednoduché dědičnosti a polymorfismu, mechanismy jeho prostřednictvím odvozené třídy můžete rozšířit a specialize základní třídy.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-242">Class types support single inheritance and polymorphism, mechanisms whereby derived classes can extend and specialize base classes.</span></span>

<span data-ttu-id="fb7c7-243">Typ struktury je podobný typu třídy, představuje strukturu dat a funkční členy.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-243">A struct type is similar to a class type in that it represents a structure with data members and function members.</span></span> <span data-ttu-id="fb7c7-244">Na rozdíl od tříd však struktury jsou typy hodnot a nevyžadují přidělení haldy.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-244">However, unlike classes, structs are value types and do not require heap allocation.</span></span> <span data-ttu-id="fb7c7-245">Typy struktury nepodporují dědičnosti zadané uživatelem, a všechny typy struktury implicitně dědí z typu `object`.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-245">Struct types do not support user-specified inheritance, and all struct types implicitly inherit from type `object`.</span></span>

<span data-ttu-id="fb7c7-246">Typ rozhraní definuje kontrakt jako pojmenovanou sadu funkce veřejné členy.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-246">An interface type defines a contract as a named set of public function members.</span></span> <span data-ttu-id="fb7c7-247">Třída nebo struktura, která implementuje rozhraní musí poskytnout implementace členů rozhraní funkce.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-247">A class or struct that implements an interface must provide implementations of the interface's function members.</span></span> <span data-ttu-id="fb7c7-248">Rozhraní může dědit z více základních rozhraní a třídy nebo struktury může implementovat více rozhraní.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-248">An interface may inherit from multiple base interfaces, and a class or struct may implement multiple interfaces.</span></span>

<span data-ttu-id="fb7c7-249">Typ delegáta představuje odkazy na metody se seznamem konkrétních parametrů a návratovým typem.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-249">A delegate type represents references to methods with a particular parameter list and return type.</span></span> <span data-ttu-id="fb7c7-250">Delegáty umožňují považovat za entity, které může být přiřazena k proměnné a předány jako parametry metod.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-250">Delegates make it possible to treat methods as entities that can be assigned to variables and passed as parameters.</span></span> <span data-ttu-id="fb7c7-251">Delegáti jsou podobný koncept ukazatelů na funkce v některých jiných jazycích, ale na rozdíl od ukazatelů na funkce, Delegáti jsou objektově orientované a typově bezpečné.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-251">Delegates are similar to the concept of function pointers found in some other languages, but unlike function pointers, delegates are object-oriented and type-safe.</span></span>

<span data-ttu-id="fb7c7-252">Třídy, struktury, rozhraní a delegátů typů všechny obecné typy podpory, které mohou být parametrizovány s jinými typy.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-252">Class, struct, interface and delegate types all support generics, whereby they can be parameterized with other types.</span></span>

<span data-ttu-id="fb7c7-253">Typ výčtu je odlišný typ se pojmenovaných konstant.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-253">An enum type is a distinct type with named constants.</span></span> <span data-ttu-id="fb7c7-254">Každý typ výčtu se základní typ, který musí mít jednu z osmi integrální typy.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-254">Every enum type has an underlying type, which must be one of the eight integral types.</span></span> <span data-ttu-id="fb7c7-255">Sadu hodnot typu výčtu je stejný jako sadu hodnot ze základního typu.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-255">The set of values of an enum type is the same as the set of values of the underlying type.</span></span>

<span data-ttu-id="fb7c7-256">C# podporuje jeden a více trojrozměrným pole libovolného typu.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-256">C# supports single- and multi-dimensional arrays of any type.</span></span> <span data-ttu-id="fb7c7-257">Na rozdíl od výše uvedené typy typy polí nemusí být deklarované před použitím.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-257">Unlike the types listed above, array types do not have to be declared before they can be used.</span></span> <span data-ttu-id="fb7c7-258">Místo toho typy pole jsou vytvořeny podle názvu typu do složených závorek.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-258">Instead, array types are constructed by following a type name with square brackets.</span></span> <span data-ttu-id="fb7c7-259">Například `int[]` je jednorozměrné pole `int`, `int[,]` je dvourozměrné pole `int`, a `int[][]` je jednorozměrné pole jednorozměrné pole `int`.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-259">For example, `int[]` is a single-dimensional array of `int`, `int[,]` is a two-dimensional array of `int`, and `int[][]` is a single-dimensional array of single-dimensional arrays of `int`.</span></span>

<span data-ttu-id="fb7c7-260">Typy s možnou hodnotou Null také nemají předtím, než je možné deklarovat.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-260">Nullable types also do not have to be declared before they can be used.</span></span> <span data-ttu-id="fb7c7-261">Pro jednotlivé typy neumožňující hodnotu `T` neexistuje odpovídající typ s možnou hodnotou Null `T?`, který může obsahovat další hodnotu `null`.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-261">For each non-nullable value type `T` there is a corresponding nullable type `T?`, which can hold an additional value `null`.</span></span> <span data-ttu-id="fb7c7-262">Například `int?` je typ, který může pojmout všechny 32bitové celé číslo nebo hodnota `null`.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-262">For instance, `int?` is a type that can hold any 32 bit integer or the value `null`.</span></span>

<span data-ttu-id="fb7c7-263">Systém typů jazyka C# je jednotný tak, aby jako objekt lze považovat hodnotu libovolného typu.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-263">C#'s type system is unified such that a value of any type can be treated as an object.</span></span> <span data-ttu-id="fb7c7-264">Všechny typy v jazyce C# přímo nebo nepřímo odvozuje od `object` typ, třídy a `object` je ultimate základní třída všech typů.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-264">Every type in C# directly or indirectly derives from the `object` class type, and `object` is the ultimate base class of all types.</span></span> <span data-ttu-id="fb7c7-265">Hodnoty odkazové typy jsou považovány za objekty jednoduše tak, že zobrazení hodnoty jako typ `object`.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-265">Values of reference types are treated as objects simply by viewing the values as type `object`.</span></span> <span data-ttu-id="fb7c7-266">Hodnoty typy hodnot jsou považovány za objekty pomocí provádí ***zabalení*** a ***rozbalení*** operace.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-266">Values of value types are treated as objects by performing ***boxing*** and ***unboxing*** operations.</span></span> <span data-ttu-id="fb7c7-267">V následujícím příkladu `int` hodnota je převedena na `object` a zpět znovu na `int`.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-267">In the following example, an `int` value is converted to `object` and back again to `int`.</span></span>

```csharp
using System;

class Test
{
    static void Main() {
        int i = 123;
        object o = i;          // Boxing
        int j = (int)o;        // Unboxing
    }
}
```
<span data-ttu-id="fb7c7-268">Pokud je hodnota typu hodnoty převést na typ `object`, instanci objektu, také nazývané "pole", je přidělená k uchování hodnoty a zkopírování hodnoty do tohoto pole.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-268">When a value of a value type is converted to type `object`, an object instance, also called a "box," is allocated to hold the value, and the value is copied into that box.</span></span> <span data-ttu-id="fb7c7-269">Naopak, když `object` odkaz je přetypován na typ hodnoty, který je pole správnou hodnotu typu odkazovaného objektu se provede kontrola a, případě zaškrtnutí bude úspěšné, je hodnota v poli zkopírována.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-269">Conversely, when an `object` reference is cast to a value type, a check is made that the referenced object is a box of the correct value type, and, if the check succeeds, the value in the box is copied out.</span></span>

<span data-ttu-id="fb7c7-270">Jednotného typu systému C# efektivně znamená, že typy hodnot se může stát objekty "na vyžádání."</span><span class="sxs-lookup"><span data-stu-id="fb7c7-270">C#'s unified type system effectively means that value types can become objects "on demand."</span></span> <span data-ttu-id="fb7c7-271">Z důvodu sjednocení pro obecné účely knihoven, které používají typ `object` jde použít s typy odkazů a typy hodnot.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-271">Because of the unification, general-purpose libraries that use type `object` can be used with both reference types and value types.</span></span>

<span data-ttu-id="fb7c7-272">Existuje několik typů z ***proměnné*** v jazyce C#, včetně polí, prvky pole, místní proměnné a parametry.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-272">There are several kinds of ***variables*** in C#, including fields, array elements, local variables, and parameters.</span></span> <span data-ttu-id="fb7c7-273">Umístění úložiště představují proměnné a každá proměnná má typ, který určuje, jaké hodnoty můžou být uložené v proměnné, jak je znázorněno v následující tabulce.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-273">Variables represent storage locations, and every variable has a type that determines what values can be stored in the variable, as shown by the following table.</span></span>


| __<span data-ttu-id="fb7c7-274">Typ proměnné</span><span class="sxs-lookup"><span data-stu-id="fb7c7-274">Type of Variable</span></span>__    | __<span data-ttu-id="fb7c7-275">Je to možné obsah</span><span class="sxs-lookup"><span data-stu-id="fb7c7-275">Possible Contents</span></span>__ |
|-------------------------|-----------------------|
| <span data-ttu-id="fb7c7-276">Typ neumožňující hodnotu</span><span class="sxs-lookup"><span data-stu-id="fb7c7-276">Non-nullable value type</span></span> | <span data-ttu-id="fb7c7-277">Hodnota tohoto přesné typu</span><span class="sxs-lookup"><span data-stu-id="fb7c7-277">A value of that exact type</span></span> |
| <span data-ttu-id="fb7c7-278">Typ s možnou hodnotou Null</span><span class="sxs-lookup"><span data-stu-id="fb7c7-278">Nullable value type</span></span>     | <span data-ttu-id="fb7c7-279">Hodnotu null nebo hodnota přesně podle tohoto typu</span><span class="sxs-lookup"><span data-stu-id="fb7c7-279">A null value or a value of that exact type</span></span> |
| `object`                | <span data-ttu-id="fb7c7-280">Odkaz s hodnotou null, odkaz na objekt typu odkazu nebo odkaz na o zabalenou hodnotu libovolný typ hodnoty</span><span class="sxs-lookup"><span data-stu-id="fb7c7-280">A null reference, a reference to an object of any reference type, or a reference to a boxed value of any value type</span></span> |
| <span data-ttu-id="fb7c7-281">Typ třídy.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-281">Class type</span></span>              | <span data-ttu-id="fb7c7-282">Odkaz s hodnotou null, odkaz na instanci daného typu třídy nebo odkaz na instanci třídy odvozené z typu třídy</span><span class="sxs-lookup"><span data-stu-id="fb7c7-282">A null reference, a reference to an instance of that class type, or a reference to an instance of a class derived from that class type</span></span> |
| <span data-ttu-id="fb7c7-283">Typ rozhraní</span><span class="sxs-lookup"><span data-stu-id="fb7c7-283">Interface type</span></span>          | <span data-ttu-id="fb7c7-284">Odkaz s hodnotou null, odkaz na instanci typu třídy, která implementuje rozhraní typu nebo odkaz zabalené hodnoty hodnotového typu, který implementuje rozhraní typu</span><span class="sxs-lookup"><span data-stu-id="fb7c7-284">A null reference, a reference to an instance of a class type that implements that interface type, or a reference to a boxed value of a value type that implements that interface type</span></span> |
| <span data-ttu-id="fb7c7-285">Typ pole</span><span class="sxs-lookup"><span data-stu-id="fb7c7-285">Array type</span></span>              | <span data-ttu-id="fb7c7-286">Odkaz s hodnotou null, odkaz na instanci daného typu pole nebo odkaz na instanci typu kompatibilní pole</span><span class="sxs-lookup"><span data-stu-id="fb7c7-286">A null reference, a reference to an instance of that array type, or a reference to an instance of a compatible array type</span></span> |
| <span data-ttu-id="fb7c7-287">Typ delegáta</span><span class="sxs-lookup"><span data-stu-id="fb7c7-287">Delegate type</span></span>           | <span data-ttu-id="fb7c7-288">Odkaz s hodnotou null nebo odkaz na instanci daného typu delegáta</span><span class="sxs-lookup"><span data-stu-id="fb7c7-288">A null reference or a reference to an instance of that delegate type</span></span> |

## <a name="expressions"></a><span data-ttu-id="fb7c7-289">Výrazy</span><span class="sxs-lookup"><span data-stu-id="fb7c7-289">Expressions</span></span>

<span data-ttu-id="fb7c7-290">***Výrazy*** se vytvářejí na základě ***operandy*** a ***operátory***.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-290">***Expressions*** are constructed from ***operands*** and ***operators***.</span></span> <span data-ttu-id="fb7c7-291">Operátory výrazu označují operace, které chcete použít pro operandy.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-291">The operators of an expression indicate which operations to apply to the operands.</span></span> <span data-ttu-id="fb7c7-292">Příklady operátorů `+`, `-`, `*`, `/`, a `new`.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-292">Examples of operators include `+`, `-`, `*`, `/`, and `new`.</span></span> <span data-ttu-id="fb7c7-293">Příklady operandy: literály, pole, místní proměnné a výrazy.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-293">Examples of operands include literals, fields, local variables, and expressions.</span></span>

<span data-ttu-id="fb7c7-294">Pokud výraz obsahuje více operátorů ***prioritu*** operátorů určuje pořadí, ve kterém jsou jednotlivé operátory vyhodnocovány.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-294">When an expression contains multiple operators, the ***precedence*** of the operators controls the order in which the individual operators are evaluated.</span></span> <span data-ttu-id="fb7c7-295">Například výraz `x + y * z` se vyhodnotí jako `x + (y * z)` protože `*` má vyšší prioritu než operátor `+` operátor.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-295">For example, the expression `x + y * z` is evaluated as `x + (y * z)` because the `*` operator has higher precedence than the `+` operator.</span></span>

<span data-ttu-id="fb7c7-296">Většina operátory mohou být ***přetížené***.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-296">Most operators can be ***overloaded***.</span></span> <span data-ttu-id="fb7c7-297">Přetížení operátoru umožňuje uživatelem definovaný operátor implementace pro operace, kde jeden nebo oba operandy jsou třídy nebo struktury typu uživatelem definované.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-297">Operator overloading permits user-defined operator implementations to be specified for operations where one or both of the operands are of a user-defined class or struct type.</span></span>

<span data-ttu-id="fb7c7-298">Následující tabulka shrnuje operátory jazyka C# společnosti, výpis kategorie operátor v pořadí dle priority od nejvyšší k nejnižší.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-298">The following table summarizes C#'s operators, listing the operator categories in order of precedence from highest to lowest.</span></span> <span data-ttu-id="fb7c7-299">Operátory ve stejné kategorii mají stejnou prioritu.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-299">Operators in the same category have equal precedence.</span></span>


| __<span data-ttu-id="fb7c7-300">Kategorie</span><span class="sxs-lookup"><span data-stu-id="fb7c7-300">Category</span></span>__                     | __<span data-ttu-id="fb7c7-301">Výraz</span><span class="sxs-lookup"><span data-stu-id="fb7c7-301">Expression</span></span>__    | __<span data-ttu-id="fb7c7-302">Popis</span><span class="sxs-lookup"><span data-stu-id="fb7c7-302">Description</span></span>__ |
|----------------------------------|-------------------|-----------------|
| <span data-ttu-id="fb7c7-303">Primární</span><span class="sxs-lookup"><span data-stu-id="fb7c7-303">Primary</span></span>                          | `x.m`             | <span data-ttu-id="fb7c7-304">Přístup ke členům</span><span class="sxs-lookup"><span data-stu-id="fb7c7-304">Member access</span></span> |
|                                  | `x(...)`          | <span data-ttu-id="fb7c7-305">Vyvolání metod a delegátů</span><span class="sxs-lookup"><span data-stu-id="fb7c7-305">Method and delegate invocation</span></span> |
|                                  | `x[...]`          | <span data-ttu-id="fb7c7-306">Přístup k poli a indexeru</span><span class="sxs-lookup"><span data-stu-id="fb7c7-306">Array and indexer access</span></span> |
|                                  | `x++`             | <span data-ttu-id="fb7c7-307">Postinkrementace</span><span class="sxs-lookup"><span data-stu-id="fb7c7-307">Post-increment</span></span> |
|                                  | `x--`             | <span data-ttu-id="fb7c7-308">Postdekrementace</span><span class="sxs-lookup"><span data-stu-id="fb7c7-308">Post-decrement</span></span> |
|                                  | `new T(...)`      | <span data-ttu-id="fb7c7-309">Vytvoření objektu a delegátu</span><span class="sxs-lookup"><span data-stu-id="fb7c7-309">Object and delegate creation</span></span> |
|                                  | `new T(...){...}` | <span data-ttu-id="fb7c7-310">Vytvoření objektu s inicializátorem</span><span class="sxs-lookup"><span data-stu-id="fb7c7-310">Object creation with initializer</span></span> |
|                                  | `new {...}`       | <span data-ttu-id="fb7c7-311">Inicializátor anonymních objektů</span><span class="sxs-lookup"><span data-stu-id="fb7c7-311">Anonymous object initializer</span></span> |
|                                  | `new T[...]`      | <span data-ttu-id="fb7c7-312">Vytvoření pole</span><span class="sxs-lookup"><span data-stu-id="fb7c7-312">Array creation</span></span> |
|                                  | `typeof(T)`       | <span data-ttu-id="fb7c7-313">Získat `System.Type` objekt pro</span><span class="sxs-lookup"><span data-stu-id="fb7c7-313">Obtain `System.Type` object for</span></span> `T` |
|                                  | `checked(x)`      | <span data-ttu-id="fb7c7-314">Vyhodnocení výrazu ve zkontrolovaném kontextu</span><span class="sxs-lookup"><span data-stu-id="fb7c7-314">Evaluate expression in checked context</span></span> |
|                                  | `unchecked(x)`    | <span data-ttu-id="fb7c7-315">Vyhodnocení výrazu v nezkontrolovaném kontextu</span><span class="sxs-lookup"><span data-stu-id="fb7c7-315">Evaluate expression in unchecked context</span></span> |
|                                  | `default(T)`      | <span data-ttu-id="fb7c7-316">Získat výchozí hodnotu typu</span><span class="sxs-lookup"><span data-stu-id="fb7c7-316">Obtain default value of type</span></span> `T` |
|                                  | `delegate {...}`  | <span data-ttu-id="fb7c7-317">Anonymní funkce (anonymní metoda)</span><span class="sxs-lookup"><span data-stu-id="fb7c7-317">Anonymous function (anonymous method)</span></span> |
| <span data-ttu-id="fb7c7-318">Unární</span><span class="sxs-lookup"><span data-stu-id="fb7c7-318">Unary</span></span>                            | `+x`              | <span data-ttu-id="fb7c7-319">Identita</span><span class="sxs-lookup"><span data-stu-id="fb7c7-319">Identity</span></span> |
|                                  | `-x`              | <span data-ttu-id="fb7c7-320">Negace</span><span class="sxs-lookup"><span data-stu-id="fb7c7-320">Negation</span></span> |
|                                  | `!x`              | <span data-ttu-id="fb7c7-321">Logická negace</span><span class="sxs-lookup"><span data-stu-id="fb7c7-321">Logical negation</span></span> |
|                                  | `~x`              | <span data-ttu-id="fb7c7-322">Bitová negace.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-322">Bitwise negation</span></span> |
|                                  | `++x`             | <span data-ttu-id="fb7c7-323">Preinkrementace</span><span class="sxs-lookup"><span data-stu-id="fb7c7-323">Pre-increment</span></span> |
|                                  | `--x`             | <span data-ttu-id="fb7c7-324">Predekrementace</span><span class="sxs-lookup"><span data-stu-id="fb7c7-324">Pre-decrement</span></span> |
|                                  | `(T)x`            | <span data-ttu-id="fb7c7-325">Explicitně převést `x` na typ</span><span class="sxs-lookup"><span data-stu-id="fb7c7-325">Explicitly convert `x` to type</span></span> `T` |
|                                  | `await x`         | <span data-ttu-id="fb7c7-326">Asynchronně počkejte `x` k dokončení</span><span class="sxs-lookup"><span data-stu-id="fb7c7-326">Asynchronously wait for `x` to complete</span></span> |
| <span data-ttu-id="fb7c7-327">Násobení</span><span class="sxs-lookup"><span data-stu-id="fb7c7-327">Multiplicative</span></span>                   | `x * y`           | <span data-ttu-id="fb7c7-328">Násobení</span><span class="sxs-lookup"><span data-stu-id="fb7c7-328">Multiplication</span></span> |
|                                  | `x / y`           | <span data-ttu-id="fb7c7-329">Dělení</span><span class="sxs-lookup"><span data-stu-id="fb7c7-329">Division</span></span> |
|                                  | `x % y`           | <span data-ttu-id="fb7c7-330">Zbytek</span><span class="sxs-lookup"><span data-stu-id="fb7c7-330">Remainder</span></span> |
| <span data-ttu-id="fb7c7-331">Additive</span><span class="sxs-lookup"><span data-stu-id="fb7c7-331">Additive</span></span>                         | `x + y`           | <span data-ttu-id="fb7c7-332">Sčítání, řetězení řetězců, kombinování delegátů</span><span class="sxs-lookup"><span data-stu-id="fb7c7-332">Addition, string concatenation, delegate combination</span></span> |
|                                  | `x - y`           | <span data-ttu-id="fb7c7-333">Odčítání, odebrání delegátů</span><span class="sxs-lookup"><span data-stu-id="fb7c7-333">Subtraction, delegate removal</span></span> |
| <span data-ttu-id="fb7c7-334">SHIFT</span><span class="sxs-lookup"><span data-stu-id="fb7c7-334">Shift</span></span>                            | `x << y`          | <span data-ttu-id="fb7c7-335">Posun doleva</span><span class="sxs-lookup"><span data-stu-id="fb7c7-335">Shift left</span></span> |
|                                  | `x >> y`          | <span data-ttu-id="fb7c7-336">Posun doprava</span><span class="sxs-lookup"><span data-stu-id="fb7c7-336">Shift right</span></span> |
| <span data-ttu-id="fb7c7-337">Relační a typové zkoušky</span><span class="sxs-lookup"><span data-stu-id="fb7c7-337">Relational and type testing</span></span>      | `x < y`           | <span data-ttu-id="fb7c7-338">Menší než</span><span class="sxs-lookup"><span data-stu-id="fb7c7-338">Less than</span></span> |
|                                  | `x > y`           | <span data-ttu-id="fb7c7-339">Větší než</span><span class="sxs-lookup"><span data-stu-id="fb7c7-339">Greater than</span></span> |
|                                  | `x <= y`          | <span data-ttu-id="fb7c7-340">Menší nebo rovno</span><span class="sxs-lookup"><span data-stu-id="fb7c7-340">Less than or equal</span></span> |
|                                  | `x >= y`          | <span data-ttu-id="fb7c7-341">Větší nebo rovno</span><span class="sxs-lookup"><span data-stu-id="fb7c7-341">Greater than or equal</span></span> |
|                                  | `x is T`          | <span data-ttu-id="fb7c7-342">Vrátí `true` Pokud `x` je `T`, `false` jinak</span><span class="sxs-lookup"><span data-stu-id="fb7c7-342">Return `true` if `x` is a `T`, `false` otherwise</span></span> |
|                                  | `x as T`          | <span data-ttu-id="fb7c7-343">Vrátí `x` zadán jako `T`, nebo `null` Pokud `x` není</span><span class="sxs-lookup"><span data-stu-id="fb7c7-343">Return `x` typed as `T`, or `null` if `x` is not a</span></span> `T` |
| <span data-ttu-id="fb7c7-344">Rovnost</span><span class="sxs-lookup"><span data-stu-id="fb7c7-344">Equality</span></span>                         | `x == y`          | <span data-ttu-id="fb7c7-345">Rovno</span><span class="sxs-lookup"><span data-stu-id="fb7c7-345">Equal</span></span>      |
|                                  | `x != y`          | <span data-ttu-id="fb7c7-346">Nerovná se</span><span class="sxs-lookup"><span data-stu-id="fb7c7-346">Not equal</span></span> |
| <span data-ttu-id="fb7c7-347">Logický operátor AND</span><span class="sxs-lookup"><span data-stu-id="fb7c7-347">Logical AND</span></span>                      | `x & y`           | <span data-ttu-id="fb7c7-348">Celočíselné bitové a logických logický operátor AND</span><span class="sxs-lookup"><span data-stu-id="fb7c7-348">Integer bitwise AND, boolean logical AND</span></span> |
| <span data-ttu-id="fb7c7-349">Logický operátor XOR</span><span class="sxs-lookup"><span data-stu-id="fb7c7-349">Logical XOR</span></span>                      | `x ^ y`           | <span data-ttu-id="fb7c7-350">Bitový operátor XOR celého čísla, logická hodnota operátoru XOR</span><span class="sxs-lookup"><span data-stu-id="fb7c7-350">Integer bitwise XOR, boolean logical XOR</span></span> |
| <span data-ttu-id="fb7c7-351">Logický operátor OR</span><span class="sxs-lookup"><span data-stu-id="fb7c7-351">Logical OR</span></span>                       | <code>x &#124; y</code> | <span data-ttu-id="fb7c7-352">Bitový operátor OR celého čísla, logická hodnota operátoru OR</span><span class="sxs-lookup"><span data-stu-id="fb7c7-352">Integer bitwise OR, boolean logical OR</span></span> |
| <span data-ttu-id="fb7c7-353">Podmiňovací operátor AND</span><span class="sxs-lookup"><span data-stu-id="fb7c7-353">Conditional AND</span></span>                  | `x && y`          | <span data-ttu-id="fb7c7-354">Vyhodnotí `y` pouze tehdy, pokud `x` je</span><span class="sxs-lookup"><span data-stu-id="fb7c7-354">Evaluates `y` only if `x` is</span></span> `true` |
| <span data-ttu-id="fb7c7-355">Podmiňovací operátor OR</span><span class="sxs-lookup"><span data-stu-id="fb7c7-355">Conditional OR</span></span>                   | <code>x &#124;&#124; y</code> | <span data-ttu-id="fb7c7-356">Vyhodnotí `y` pouze tehdy, pokud `x` je</span><span class="sxs-lookup"><span data-stu-id="fb7c7-356">Evaluates `y` only if `x` is</span></span> `false` |
| <span data-ttu-id="fb7c7-357">Nulové sloučení</span><span class="sxs-lookup"><span data-stu-id="fb7c7-357">Null coalescing</span></span>                  | `x ?? y`          | <span data-ttu-id="fb7c7-358">Vyhodnotí jako `y` Pokud `x` je `null`do `x` jinak</span><span class="sxs-lookup"><span data-stu-id="fb7c7-358">Evaluates to `y` if `x` is `null`, to `x` otherwise</span></span> |
| <span data-ttu-id="fb7c7-359">Podmiňovací operátor</span><span class="sxs-lookup"><span data-stu-id="fb7c7-359">Conditional</span></span>                      | `x ? y : z`       | <span data-ttu-id="fb7c7-360">Vyhodnotí `y` Pokud `x` je `true`, `z` Pokud `x` je</span><span class="sxs-lookup"><span data-stu-id="fb7c7-360">Evaluates `y` if `x` is `true`, `z` if `x` is</span></span> `false` |
| <span data-ttu-id="fb7c7-361">Přiřazení nebo anonymní funkce</span><span class="sxs-lookup"><span data-stu-id="fb7c7-361">Assignment or anonymous function</span></span> | `x = y`           | <span data-ttu-id="fb7c7-362">Přiřazení</span><span class="sxs-lookup"><span data-stu-id="fb7c7-362">Assignment</span></span> |
|                                  | `x op= y`         | <span data-ttu-id="fb7c7-363">Složené přiřazení. podporované operátory jsou</span><span class="sxs-lookup"><span data-stu-id="fb7c7-363">Compound assignment; supported operators are</span></span> `*=` `/=` `%=` `+=` `-=` `<<=` `>>=` `&=` `^=` <code>&#124;=</code> |
|                                  | `(T x) => y`      | <span data-ttu-id="fb7c7-364">Anonymní funkce (výraz lambda)</span><span class="sxs-lookup"><span data-stu-id="fb7c7-364">Anonymous function (lambda expression)</span></span> |

## <a name="statements"></a><span data-ttu-id="fb7c7-365">Příkazy</span><span class="sxs-lookup"><span data-stu-id="fb7c7-365">Statements</span></span>

<span data-ttu-id="fb7c7-366">Akce programu jsou vyjádřeny pomocí ***příkazy***.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-366">The actions of a program are expressed using ***statements***.</span></span> <span data-ttu-id="fb7c7-367">C# podporuje několik různých druhů příkazů, číslo, které jsou definovány z hlediska vložených příkazů.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-367">C# supports several different kinds of statements, a number of which are defined in terms of embedded statements.</span></span>

<span data-ttu-id="fb7c7-368">A ***bloku*** povoluje více příkazů, které má být zapsán v kontextech, kde je povoleno jeden příkaz.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-368">A ***block*** permits multiple statements to be written in contexts where a single statement is allowed.</span></span> <span data-ttu-id="fb7c7-369">Blok obsahuje seznam příkazů, které jsou napsané mezi oddělovači `{` a `}`.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-369">A block consists of a list of statements written between the delimiters `{` and `}`.</span></span>

<span data-ttu-id="fb7c7-370">***Příkazy deklarace*** se používají k deklarování místní proměnné a konstanty.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-370">***Declaration statements*** are used to declare local variables and constants.</span></span>

<span data-ttu-id="fb7c7-371">***Příkazy výrazů*** se používají k vyhodnocení výrazů.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-371">***Expression statements*** are used to evaluate expressions.</span></span> <span data-ttu-id="fb7c7-372">Volání metod, obsahovat výrazy, které lze použít jako příkazy přidělení pomocí objektu `new` operátora, přiřazení pomocí `=` a operátory složeného přiřazení, operace Inkrementace a dekrementace pomocí `++`a `--` operátory a výrazy await.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-372">Expressions that can be used as statements include method invocations, object allocations using the `new` operator, assignments using `=` and the compound assignment operators, increment and decrement operations using the `++` and `--` operators and await expressions.</span></span>

<span data-ttu-id="fb7c7-373">***Příkazy výběru*** se používají k výběru jedním z možných příkazů pro spuštění na základě hodnoty některých výrazu.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-373">***Selection statements*** are used to select one of a number of possible statements for execution based on the value of some expression.</span></span> <span data-ttu-id="fb7c7-374">V této skupině jsou `if` a `switch` příkazy.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-374">In this group are the `if` and `switch` statements.</span></span>

<span data-ttu-id="fb7c7-375">***Příkazy iterace*** se používají k vloženým příkazem spouštět opakovaně.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-375">***Iteration statements*** are used to repeatedly execute an embedded statement.</span></span> <span data-ttu-id="fb7c7-376">V této skupině jsou `while`, `do`, `for`, a `foreach` příkazy.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-376">In this group are the `while`, `do`, `for`, and `foreach` statements.</span></span>

<span data-ttu-id="fb7c7-377">***Jump – příkazy*** slouží k předání řízení.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-377">***Jump statements*** are used to transfer control.</span></span> <span data-ttu-id="fb7c7-378">V této skupině jsou `break`, `continue`, `goto`, `throw`, `return`, a `yield` příkazy.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-378">In this group are the `break`, `continue`, `goto`, `throw`, `return`, and `yield` statements.</span></span>

<span data-ttu-id="fb7c7-379">`try`... `catch` prohlášení se používá k zachycení výjimek, ke kterým dochází při provádění bloku, a `try`... `finally` prohlášení se používá k určení dokončovacího kódu, který je vždy spuštěn, zda došlo k výjimce, nebo ne.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-379">The `try`...`catch` statement is used to catch exceptions that occur during execution of a block, and the `try`...`finally` statement is used to specify finalization code that is always executed, whether an exception occurred or not.</span></span>

<span data-ttu-id="fb7c7-380">`checked` a `unchecked` příkazy se používají k řízení kontroly kontext pro integrálového typu aritmetické operace a převody přetečení.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-380">The `checked` and `unchecked` statements are used to control the overflow checking context for integral-type arithmetic operations and conversions.</span></span>

<span data-ttu-id="fb7c7-381">`lock` Prohlášení se používá k získání zámku vzájemné vyloučení pro daný objekt, provedení příkazu a uvolněte zámek.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-381">The `lock` statement is used to obtain the mutual-exclusion lock for a given object, execute a statement, and then release the lock.</span></span>

<span data-ttu-id="fb7c7-382">`using` Prohlášení se používá k získání prostředku, provedení příkazu a potom tento prostředek.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-382">The `using` statement is used to obtain a resource, execute a statement, and then dispose of that resource.</span></span>

<span data-ttu-id="fb7c7-383">Níže jsou příklady každý druh – příkaz</span><span class="sxs-lookup"><span data-stu-id="fb7c7-383">Below are examples of each kind of statement</span></span>

__<span data-ttu-id="fb7c7-384">Místní deklarace proměnné</span><span class="sxs-lookup"><span data-stu-id="fb7c7-384">Local variable declarations</span></span>__

```csharp
static void Main() {
   int a;
   int b = 2, c = 3;
   a = 1;
   Console.WriteLine(a + b + c);
}
```


__<span data-ttu-id="fb7c7-385">Místní deklarace konstanty</span><span class="sxs-lookup"><span data-stu-id="fb7c7-385">Local constant declaration</span></span>__

```csharp
static void Main() {
    const float pi = 3.1415927f;
    const int r = 25;
    Console.WriteLine(pi * r * r);
}
```


__<span data-ttu-id="fb7c7-386">příkaz výrazu</span><span class="sxs-lookup"><span data-stu-id="fb7c7-386">Expression statement</span></span>__

```csharp
static void Main() {
    int i;
    i = 123;                // Expression statement
    Console.WriteLine(i);   // Expression statement
    i++;                    // Expression statement
    Console.WriteLine(i);   // Expression statement
}
```

__`if` <span data-ttu-id="fb7c7-387">– Příkaz</span><span class="sxs-lookup"><span data-stu-id="fb7c7-387">statement</span></span>__

```csharp
static void Main(string[] args) {
    if (args.Length == 0) {
        Console.WriteLine("No arguments");
    }
    else {
        Console.WriteLine("One or more arguments");
    }
}
```


__`switch` <span data-ttu-id="fb7c7-388">– Příkaz</span><span class="sxs-lookup"><span data-stu-id="fb7c7-388">statement</span></span>__

```csharp
static void Main(string[] args) {
    int n = args.Length;
    switch (n) {
        case 0:
            Console.WriteLine("No arguments");
            break;
        case 1:
            Console.WriteLine("One argument");
            break;
        default:
            Console.WriteLine("{0} arguments", n);
            break;
    }
}
```

__`while` <span data-ttu-id="fb7c7-389">– Příkaz</span><span class="sxs-lookup"><span data-stu-id="fb7c7-389">statement</span></span>__

```csharp
static void Main(string[] args) {
    int i = 0;
    while (i < args.Length) {
        Console.WriteLine(args[i]);
        i++;
    }
}
```


__`do` <span data-ttu-id="fb7c7-390">– Příkaz</span><span class="sxs-lookup"><span data-stu-id="fb7c7-390">statement</span></span>__

```csharp
static void Main() {
    string s;
    do {
        s = Console.ReadLine();
        if (s != null) Console.WriteLine(s);
    } while (s != null);
}
```

__`for` <span data-ttu-id="fb7c7-391">– Příkaz</span><span class="sxs-lookup"><span data-stu-id="fb7c7-391">statement</span></span>__

```csharp
static void Main(string[] args) {
    for (int i = 0; i < args.Length; i++) {
        Console.WriteLine(args[i]);
    }
}
```

__`foreach` <span data-ttu-id="fb7c7-392">– Příkaz</span><span class="sxs-lookup"><span data-stu-id="fb7c7-392">statement</span></span>__

```csharp
static void Main(string[] args) {
    foreach (string s in args) {
        Console.WriteLine(s);
    }
}
```

__`break` <span data-ttu-id="fb7c7-393">– Příkaz</span><span class="sxs-lookup"><span data-stu-id="fb7c7-393">statement</span></span>__

```csharp
static void Main() {
    while (true) {
        string s = Console.ReadLine();
        if (s == null) break;
        Console.WriteLine(s);
    }
}
```

__`continue` <span data-ttu-id="fb7c7-394">– Příkaz</span><span class="sxs-lookup"><span data-stu-id="fb7c7-394">statement</span></span>__

```csharp
static void Main(string[] args) {
    for (int i = 0; i < args.Length; i++) {
        if (args[i].StartsWith("/")) continue;
        Console.WriteLine(args[i]);
    }
}
```

__`goto` <span data-ttu-id="fb7c7-395">– Příkaz</span><span class="sxs-lookup"><span data-stu-id="fb7c7-395">statement</span></span>__

```csharp
static void Main(string[] args) {
    int i = 0;
    goto check;
    loop:
    Console.WriteLine(args[i++]);
    check:
    if (i < args.Length) goto loop;
}
```

__`return` <span data-ttu-id="fb7c7-396">– Příkaz</span><span class="sxs-lookup"><span data-stu-id="fb7c7-396">statement</span></span>__

```csharp
static int Add(int a, int b) {
    return a + b;
}

static void Main() {
    Console.WriteLine(Add(1, 2));
    return;
}
```

__`yield` <span data-ttu-id="fb7c7-397">– Příkaz</span><span class="sxs-lookup"><span data-stu-id="fb7c7-397">statement</span></span>__

```csharp
static IEnumerable<int> Range(int from, int to) {
    for (int i = from; i < to; i++) {
        yield return i;
    }
    yield break;
}

static void Main() {
    foreach (int x in Range(-10,10)) {
        Console.WriteLine(x);
    }
}
```

__`throw` <span data-ttu-id="fb7c7-398">a `try` příkazy</span><span class="sxs-lookup"><span data-stu-id="fb7c7-398">and `try` statements</span></span>__

```csharp
static double Divide(double x, double y) {
    if (y == 0) throw new DivideByZeroException();
    return x / y;
}

static void Main(string[] args) {
    try {
        if (args.Length != 2) {
            throw new Exception("Two numbers required");
        }
        double x = double.Parse(args[0]);
        double y = double.Parse(args[1]);
        Console.WriteLine(Divide(x, y));
    }
    catch (Exception e) {
        Console.WriteLine(e.Message);
    }
    finally {
        Console.WriteLine("Good bye!");
    }
}
```

__`checked` <span data-ttu-id="fb7c7-399">a `unchecked` příkazy</span><span class="sxs-lookup"><span data-stu-id="fb7c7-399">and `unchecked` statements</span></span>__

```csharp
static void Main() {
    int i = int.MaxValue;
    checked {
        Console.WriteLine(i + 1);        // Exception
    }
    unchecked {
        Console.WriteLine(i + 1);        // Overflow
    }
}
```

__`lock` <span data-ttu-id="fb7c7-400">– Příkaz</span><span class="sxs-lookup"><span data-stu-id="fb7c7-400">statement</span></span>__

```csharp
class Account
{
    decimal balance;
    public void Withdraw(decimal amount) {
        lock (this) {
            if (amount > balance) {
                throw new Exception("Insufficient funds");
            }
            balance -= amount;
        }
    }
}
```

__`using` <span data-ttu-id="fb7c7-401">– Příkaz</span><span class="sxs-lookup"><span data-stu-id="fb7c7-401">statement</span></span>__

```csharp
static void Main() {
    using (TextWriter w = File.CreateText("test.txt")) {
        w.WriteLine("Line one");
        w.WriteLine("Line two");
        w.WriteLine("Line three");
    }
}
```

## <a name="classes-and-objects"></a><span data-ttu-id="fb7c7-402">Třídy a objekty</span><span class="sxs-lookup"><span data-stu-id="fb7c7-402">Classes and objects</span></span>

<span data-ttu-id="fb7c7-403">***Třídy*** jsou nejvíce základní typy jazyka C#.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-403">***Classes*** are the most fundamental of C#'s types.</span></span> <span data-ttu-id="fb7c7-404">Třída je datová struktura, která kombinuje stavů (pole) a akcí (metody a další funkce členů) v jediné jednotce.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-404">A class is a data structure that combines state (fields) and actions (methods and other function members) in a single unit.</span></span> <span data-ttu-id="fb7c7-405">Třída obsahuje definici pro dynamicky vytvořené ***instance*** třídy, označované také jako ***objekty***.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-405">A class provides a definition for dynamically created ***instances*** of the class, also known as ***objects***.</span></span> <span data-ttu-id="fb7c7-406">Třídy podpory ***dědičnosti*** a ***polymorfismus***, mechanismy kterým ***odvozené třídy*** můžete rozšířit a specialize ***základních tříd***.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-406">Classes support ***inheritance*** and ***polymorphism***, mechanisms whereby ***derived classes*** can extend and specialize ***base classes***.</span></span>

<span data-ttu-id="fb7c7-407">Nové třídy jsou vytvořené pomocí deklarace tříd.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-407">New classes are created using class declarations.</span></span> <span data-ttu-id="fb7c7-408">Deklarace třídy začíná hlavičku, která určuje atributy a modifikátory třídy, název třídy, základní třídy (Pokud je zadaný) a rozhraní implementované třídy.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-408">A class declaration starts with a header that specifies the attributes and modifiers of the class, the name of the class, the base class (if given), and the interfaces implemented by the class.</span></span> <span data-ttu-id="fb7c7-409">Záhlaví je následována tělo třídy, které se skládá ze seznamu deklarací členů napsané mezi oddělovači `{` a `}`.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-409">The header is followed by the class body, which consists of a list of member declarations written between the delimiters `{` and `}`.</span></span>

<span data-ttu-id="fb7c7-410">Následuje deklaraci jednoduchou třídu pojmenovanou `Point`:</span><span class="sxs-lookup"><span data-stu-id="fb7c7-410">The following is a declaration of a simple class named `Point`:</span></span>

```csharp
public class Point
{
    public int x, y;

    public Point(int x, int y) {
        this.x = x;
        this.y = y;
    }
}
```
<span data-ttu-id="fb7c7-411">Jsou vytvořeny pomocí instance třídy `new` operátor, který přiděluje paměť pro novou instanci, vyvolá konstruktor k inicializaci instance a vrátí odkaz na instanci.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-411">Instances of classes are created using the `new` operator, which allocates memory for a new instance, invokes a constructor to initialize the instance, and returns a reference to the instance.</span></span> <span data-ttu-id="fb7c7-412">Následující příkazy vytvoří dvě `Point` objektů a ukládají odkazy na tyto objekty ve dvou proměnných:</span><span class="sxs-lookup"><span data-stu-id="fb7c7-412">The following statements create two `Point` objects and store references to those objects in two variables:</span></span>

```
Point p1 = new Point(0, 0);
Point p2 = new Point(10, 20);
```
<span data-ttu-id="fb7c7-413">Paměť obsazena objekt je automaticky převzaty při objektu je již používán.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-413">The memory occupied by an object is automatically reclaimed when the object is no longer in use.</span></span> <span data-ttu-id="fb7c7-414">To není nezbytné ani možné explicitně uvolnit objekty v jazyce C#.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-414">It is neither necessary nor possible to explicitly deallocate objects in C#.</span></span>

### <a name="members"></a><span data-ttu-id="fb7c7-415">Členové</span><span class="sxs-lookup"><span data-stu-id="fb7c7-415">Members</span></span>

<span data-ttu-id="fb7c7-416">Členy třídy jsou buď ***statické členy*** nebo ***členy instance***.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-416">The members of a class are either ***static members*** or ***instance members***.</span></span> <span data-ttu-id="fb7c7-417">Statické členy patří do třídy a členy instance patřit k objektům (instance třídy).</span><span class="sxs-lookup"><span data-stu-id="fb7c7-417">Static members belong to classes, and instance members belong to objects (instances of classes).</span></span>

<span data-ttu-id="fb7c7-418">Následující tabulka obsahuje přehled druhy členů, které mohou obsahovat třídu.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-418">The following table provides an overview of the kinds of members a class can contain.</span></span>


| __<span data-ttu-id="fb7c7-419">Člen</span><span class="sxs-lookup"><span data-stu-id="fb7c7-419">Member</span></span>__   | __<span data-ttu-id="fb7c7-420">Popis</span><span class="sxs-lookup"><span data-stu-id="fb7c7-420">Description</span></span>__ |
|------------  |-----------------|
| <span data-ttu-id="fb7c7-421">Konstanty</span><span class="sxs-lookup"><span data-stu-id="fb7c7-421">Constants</span></span>    | <span data-ttu-id="fb7c7-422">Konstantní hodnoty, které jsou přidružené k třídě</span><span class="sxs-lookup"><span data-stu-id="fb7c7-422">Constant values associated with the class</span></span> |
| <span data-ttu-id="fb7c7-423">Pole</span><span class="sxs-lookup"><span data-stu-id="fb7c7-423">Fields</span></span>       | <span data-ttu-id="fb7c7-424">Proměnné třídy</span><span class="sxs-lookup"><span data-stu-id="fb7c7-424">Variables of the class</span></span> |
| <span data-ttu-id="fb7c7-425">Metody</span><span class="sxs-lookup"><span data-stu-id="fb7c7-425">Methods</span></span>      | <span data-ttu-id="fb7c7-426">Výpočtů a akcí, které lze provést pomocí třídy</span><span class="sxs-lookup"><span data-stu-id="fb7c7-426">Computations and actions that can be performed by the class</span></span> |
| <span data-ttu-id="fb7c7-427">Vlastnosti</span><span class="sxs-lookup"><span data-stu-id="fb7c7-427">Properties</span></span>   | <span data-ttu-id="fb7c7-428">Akce přidružené k čtení a zápis s názvem vlastnosti třídy</span><span class="sxs-lookup"><span data-stu-id="fb7c7-428">Actions associated with reading and writing named properties of the class</span></span> |
| <span data-ttu-id="fb7c7-429">Indexery</span><span class="sxs-lookup"><span data-stu-id="fb7c7-429">Indexers</span></span>     | <span data-ttu-id="fb7c7-430">Akce přidružené k indexování instancí třídy jako pole</span><span class="sxs-lookup"><span data-stu-id="fb7c7-430">Actions associated with indexing instances of the class like an array</span></span> |
| <span data-ttu-id="fb7c7-431">Události</span><span class="sxs-lookup"><span data-stu-id="fb7c7-431">Events</span></span>       | <span data-ttu-id="fb7c7-432">Oznámení, která mohou být generovány třídy</span><span class="sxs-lookup"><span data-stu-id="fb7c7-432">Notifications that can be generated by the class</span></span> |
| <span data-ttu-id="fb7c7-433">Operátory</span><span class="sxs-lookup"><span data-stu-id="fb7c7-433">Operators</span></span>    | <span data-ttu-id="fb7c7-434">Převody a podporovaných třídou operátory výrazů</span><span class="sxs-lookup"><span data-stu-id="fb7c7-434">Conversions and expression operators supported by the class</span></span> |
| <span data-ttu-id="fb7c7-435">Konstruktory</span><span class="sxs-lookup"><span data-stu-id="fb7c7-435">Constructors</span></span> | <span data-ttu-id="fb7c7-436">Akce potřebné k inicializaci instance třídy nebo vlastní třídy</span><span class="sxs-lookup"><span data-stu-id="fb7c7-436">Actions required to initialize instances of the class or the class itself</span></span> |
| <span data-ttu-id="fb7c7-437">Destruktory</span><span class="sxs-lookup"><span data-stu-id="fb7c7-437">Destructors</span></span>  | <span data-ttu-id="fb7c7-438">Akce k provedení před instancí třídy budou trvale odstraněny</span><span class="sxs-lookup"><span data-stu-id="fb7c7-438">Actions to perform before instances of the class are permanently discarded</span></span> |
| <span data-ttu-id="fb7c7-439">Typy</span><span class="sxs-lookup"><span data-stu-id="fb7c7-439">Types</span></span>        | <span data-ttu-id="fb7c7-440">Vnořené typy deklarované pomocí třídy</span><span class="sxs-lookup"><span data-stu-id="fb7c7-440">Nested types declared by the class</span></span> |

### <a name="accessibility"></a><span data-ttu-id="fb7c7-441">Usnadnění</span><span class="sxs-lookup"><span data-stu-id="fb7c7-441">Accessibility</span></span>

<span data-ttu-id="fb7c7-442">Každý člen třídy má přidružené usnadnění přístupu, který řídí oblasti textem programu, které budou mít přístup k členu.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-442">Each member of a class has an associated accessibility, which controls the regions of program text that are able to access the member.</span></span> <span data-ttu-id="fb7c7-443">Existuje pět možné formy usnadnění přístupu.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-443">There are five possible forms of accessibility.</span></span> <span data-ttu-id="fb7c7-444">Ty jsou shrnuté v následující tabulce.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-444">These are summarized in the following table.</span></span>


| __<span data-ttu-id="fb7c7-445">Usnadnění</span><span class="sxs-lookup"><span data-stu-id="fb7c7-445">Accessibility</span></span>__    | __<span data-ttu-id="fb7c7-446">Význam</span><span class="sxs-lookup"><span data-stu-id="fb7c7-446">Meaning</span></span>__ |
|----------------------|-----------------|
| `public`             | <span data-ttu-id="fb7c7-447">Přístup mimo jiné</span><span class="sxs-lookup"><span data-stu-id="fb7c7-447">Access not limited</span></span> |
| `protected`          | <span data-ttu-id="fb7c7-448">Přístup pouze pro tuto třídu nebo třídy odvozené z této třídy</span><span class="sxs-lookup"><span data-stu-id="fb7c7-448">Access limited to this class or classes derived from this class</span></span> |
| `internal`           | <span data-ttu-id="fb7c7-449">Přístup omezen na tento program</span><span class="sxs-lookup"><span data-stu-id="fb7c7-449">Access limited to this program</span></span> |
| `protected internal` | <span data-ttu-id="fb7c7-450">Přístup omezen na tento program nebo třídy odvozené z této třídy</span><span class="sxs-lookup"><span data-stu-id="fb7c7-450">Access limited to this program or classes derived from this class</span></span> |
| `private`            | <span data-ttu-id="fb7c7-451">Přístup pouze pro tuto třídu</span><span class="sxs-lookup"><span data-stu-id="fb7c7-451">Access limited to this class</span></span> |

### <a name="type-parameters"></a><span data-ttu-id="fb7c7-452">Parametry typu</span><span class="sxs-lookup"><span data-stu-id="fb7c7-452">Type parameters</span></span>

<span data-ttu-id="fb7c7-453">Definice třídy může určit sadu parametrů typu podle názvu třídy s ostré závorky uzavírající seznam názvy parametrů typů.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-453">A class definition may specify a set of type parameters by following the class name with angle brackets enclosing a list of type parameter names.</span></span> <span data-ttu-id="fb7c7-454">Parametry typu, můžete použít v těle deklarace tříd k definování členy třídy.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-454">The type parameters can the be used in the body of the class declarations to define the members of the class.</span></span> <span data-ttu-id="fb7c7-455">V následujícím příkladu, parametry typu `Pair` jsou `TFirst` a `TSecond`:</span><span class="sxs-lookup"><span data-stu-id="fb7c7-455">In the following example, the type parameters of `Pair` are `TFirst` and `TSecond`:</span></span>

```csharp
public class Pair<TFirst,TSecond>
{
    public TFirst First;
    public TSecond Second;
}
```
<span data-ttu-id="fb7c7-456">Typ třídy, který je deklarován mít parametry typu se nazývá typu obecné třídy.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-456">A class type that is declared to take type parameters is called a generic class type.</span></span> <span data-ttu-id="fb7c7-457">Typy struktury, rozhraní a delegátů mohou být obecný.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-457">Struct, interface and delegate types can also be generic.</span></span>

<span data-ttu-id="fb7c7-458">Při použití obecné třídy musí být uvedeny argumentů typu pro jednotlivé parametry typu:</span><span class="sxs-lookup"><span data-stu-id="fb7c7-458">When the generic class is used, type arguments must be provided for each of the type parameters:</span></span>

```csharp
Pair<int,string> pair = new Pair<int,string> { First = 1, Second = "two" };
int i = pair.First;     // TFirst is int
string s = pair.Second; // TSecond is string
```
<span data-ttu-id="fb7c7-459">Obecný typ s argumenty typů, které jsou k dispozici, jako je třeba `Pair<int,string>
    ` výš, se nazývá konstruovaný typ.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-459">A generic type with type arguments provided, like `Pair<int,string>
    ` above, is called a constructed type.</span></span>

### <a name="base-classes"></a><span data-ttu-id="fb7c7-460">Základní třídy</span><span class="sxs-lookup"><span data-stu-id="fb7c7-460">Base classes</span></span>

<span data-ttu-id="fb7c7-461">Deklarace třídy může určovat základní třídu pomocí následujících parametrů názvem a typem třídy pomocí dvojtečku a název základní třídy.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-461">A class declaration may specify a base class by following the class name and type parameters with a colon and the name of the base class.</span></span> <span data-ttu-id="fb7c7-462">Vynechání specifikace základní třídy je stejný jako odvozený od typu `object`.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-462">Omitting a base class specification is the same as deriving from type `object`.</span></span> <span data-ttu-id="fb7c7-463">V následujícím příkladu základní třída `Point3D` je `Point`a základní třídu `Point` je `object`:</span><span class="sxs-lookup"><span data-stu-id="fb7c7-463">In the following example, the base class of `Point3D` is `Point`, and the base class of `Point` is `object`:</span></span>

```csharp
public class Point
{
    public int x, y;

    public Point(int x, int y) {
        this.x = x;
        this.y = y;
    }
}

public class Point3D: Point
{
    public int z;

    public Point3D(int x, int y, int z): base(x, y) {
        this.z = z;
    }
}
```
<span data-ttu-id="fb7c7-464">Třída dědí členy své základní třídy.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-464">A class inherits the members of its base class.</span></span> <span data-ttu-id="fb7c7-465">Dědičnost znamená, že třídy implicitně obsahuje všechny členy své základní třídy, s výjimkou instance a statické konstruktory a destruktory základní třídy.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-465">Inheritance means that a class implicitly contains all members of its base class, except for the instance and static constructors, and the destructors of the base class.</span></span> <span data-ttu-id="fb7c7-466">Odvozené třídy můžete přidat nové členy pro ty, které se dědí, ale nemůže odstranit definici zděděného člena.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-466">A derived class can add new members to those it inherits, but it cannot remove the definition of an inherited member.</span></span> <span data-ttu-id="fb7c7-467">V předchozím příkladu `Point3D` dědí `x` a `y` pole z `Point`a každý `Point3D` instance obsahuje tři pole `x`, `y`, a `z`.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-467">In the previous example, `Point3D` inherits the `x` and `y` fields from `Point`, and every `Point3D` instance contains three fields, `x`, `y`, and `z`.</span></span>

<span data-ttu-id="fb7c7-468">Implicitní převod existuje z typu třídy pro některé typy jejího základní třídy.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-468">An implicit conversion exists from a class type to any of its base class types.</span></span> <span data-ttu-id="fb7c7-469">Proměnné typu třídy. proto odkazovat instance této třídy nebo instance všechny odvozené třídy.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-469">Therefore, a variable of a class type can reference an instance of that class or an instance of any derived class.</span></span> <span data-ttu-id="fb7c7-470">Například dány předchozí deklarace třídy, proměnné typu `Point` odkazovat buď `Point` nebo `Point3D`:</span><span class="sxs-lookup"><span data-stu-id="fb7c7-470">For example, given the previous class declarations, a variable of type `Point` can reference either a `Point` or a `Point3D`:</span></span>

```csharp
Point a = new Point(10, 20);
Point b = new Point3D(10, 20, 30);
```

### <a name="fields"></a><span data-ttu-id="fb7c7-471">Pole</span><span class="sxs-lookup"><span data-stu-id="fb7c7-471">Fields</span></span>

<span data-ttu-id="fb7c7-472">Pole je proměnná, která souvisí s třídou nebo s instancí třídy.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-472">A field is a variable that is associated with a class or with an instance of a class.</span></span>

<span data-ttu-id="fb7c7-473">Pole deklarované s `static` modifikátor definuje ***statické pole***.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-473">A field declared with the `static` modifier defines a ***static field***.</span></span> <span data-ttu-id="fb7c7-474">Statické pole identifikuje přesně jednoho umístění úložiště.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-474">A static field identifies exactly one storage location.</span></span> <span data-ttu-id="fb7c7-475">Bez ohledu na to, kolik instancí třídy jsou vytvořeny je někdy pouze jednu kopii tohoto statické pole.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-475">No matter how many instances of a class are created, there is only ever one copy of a static field.</span></span>

<span data-ttu-id="fb7c7-476">Pole deklarované bez `static` modifikátor definuje ***pole instance***.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-476">A field declared without the `static` modifier defines an ***instance field***.</span></span> <span data-ttu-id="fb7c7-477">Každá instance třídy obsahuje kopii všechna pole instancí této třídy.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-477">Every instance of a class contains a separate copy of all the instance fields of that class.</span></span>

<span data-ttu-id="fb7c7-478">V následujícím příkladu, každá instance `Color` třída má samostatnou kopii `r`, `g`, a `b` instance pole, ale existuje pouze jedna kopie `Black`, `White`, `Red`, `Green`, a `Blue` statická pole:</span><span class="sxs-lookup"><span data-stu-id="fb7c7-478">In the following example, each instance of the `Color` class has a separate copy of the `r`, `g`, and `b` instance fields, but there is only one copy of the `Black`, `White`, `Red`, `Green`, and `Blue` static fields:</span></span>

```csharp
public class Color
{
    public static readonly Color Black = new Color(0, 0, 0);
    public static readonly Color White = new Color(255, 255, 255);
    public static readonly Color Red = new Color(255, 0, 0);
    public static readonly Color Green = new Color(0, 255, 0);
    public static readonly Color Blue = new Color(0, 0, 255);
    private byte r, g, b;

    public Color(byte r, byte g, byte b) {
        this.r = r;
        this.g = g;
        this.b = b;
    }
}
```
<span data-ttu-id="fb7c7-479">Jak je znázorněno v předchozím příkladu ***pole jen pro čtení*** mohou být deklarovány s `readonly` modifikátor.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-479">As shown in the previous example, ***read-only fields*** may be declared with a `readonly` modifier.</span></span> <span data-ttu-id="fb7c7-480">Přiřazení `readonly` pole se můžou vyskytnout jenom jako součást deklarace pole nebo v konstruktoru ve stejné třídě.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-480">Assignment to a `readonly` field can only occur as part of the field's declaration or in a constructor in the same class.</span></span>

### <a name="methods"></a><span data-ttu-id="fb7c7-481">Metody</span><span class="sxs-lookup"><span data-stu-id="fb7c7-481">Methods</span></span>

<span data-ttu-id="fb7c7-482">A ***metoda*** je člen, který implementuje výpočtu nebo akce, která může provádět k objektu nebo třídě.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-482">A ***method*** is a member that implements a computation or action that can be performed by an object or class.</span></span> <span data-ttu-id="fb7c7-483">***Statické metody*** jsou přístupné prostřednictvím třídy.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-483">***Static methods*** are accessed through the class.</span></span> <span data-ttu-id="fb7c7-484">***Instance metody*** jsou přístupné prostřednictvím instance třídy.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-484">***Instance methods*** are accessed through instances of the class.</span></span>

<span data-ttu-id="fb7c7-485">Metody mají (pravděpodobně prázdná) seznam ***parametry***, které představují hodnoty nebo odkazy na proměnné předaný metodě a ***návratový typ***, která určuje typ hodnoty vypočítané a vrácené Metoda.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-485">Methods have a (possibly empty) list of ***parameters***, which represent values or variable references passed to the method, and a ***return type***, which specifies the type of the value computed and returned by the method.</span></span> <span data-ttu-id="fb7c7-486">Návratový typ metody je `void` Pokud nevrací hodnotu.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-486">A method's return type is `void` if it does not return a value.</span></span>

<span data-ttu-id="fb7c7-487">Podobně jako typy metody mají také sadu parametrů typu, pro které musí být zadán argumenty typu při volání metody.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-487">Like types, methods may also have a set of type parameters, for which type arguments must be specified when the method is called.</span></span> <span data-ttu-id="fb7c7-488">Na rozdíl od typů argumenty typu často jde odvodit z argumentů volání metody a nemusí být explicitně uvedena.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-488">Unlike types, the type arguments can often be inferred from the arguments of a method call and need not be explicitly given.</span></span>

<span data-ttu-id="fb7c7-489">***Podpis*** metody musí být jedinečný ve třídě, ve kterém je deklarována metodu.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-489">The ***signature*** of a method must be unique in the class in which the method is declared.</span></span> <span data-ttu-id="fb7c7-490">Podpis metody se skládá z názvu metody, počet parametrů typu a číslo, modifikátory a typy ze svých parametrů.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-490">The signature of a method consists of the name of the method, the number of type parameters and the number, modifiers, and types of its parameters.</span></span> <span data-ttu-id="fb7c7-491">Podpis metody neobsahuje návratovým typem.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-491">The signature of a method does not include the return type.</span></span>

#### <a name="parameters"></a><span data-ttu-id="fb7c7-492">Parametry</span><span class="sxs-lookup"><span data-stu-id="fb7c7-492">Parameters</span></span>

<span data-ttu-id="fb7c7-493">Parametry se používají k předávání hodnot nebo proměnné odkazy na metody.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-493">Parameters are used to pass values or variable references to methods.</span></span> <span data-ttu-id="fb7c7-494">Parametry metody získávají skutečné hodnoty z ***argumenty*** , které jsou určeny při vyvolání metody.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-494">The parameters of a method get their actual values from the ***arguments*** that are specified when the method is invoked.</span></span> <span data-ttu-id="fb7c7-495">Existují čtyři druhy parametry: hodnoty, parametry, parametry odkazů, výstupní parametry a pole parametrů.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-495">There are four kinds of parameters: value parameters, reference parameters, output parameters, and parameter arrays.</span></span>

<span data-ttu-id="fb7c7-496">A ***parametr hodnoty*** slouží k předávání vstupních parametrů.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-496">A ***value parameter*** is used for input parameter passing.</span></span> <span data-ttu-id="fb7c7-497">Hodnota parametru odpovídá místní proměnná, která se získá z argumentu, který byl předán parametr počáteční hodnoty.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-497">A value parameter corresponds to a local variable that gets its initial value from the argument that was passed for the parameter.</span></span> <span data-ttu-id="fb7c7-498">Úpravy parametru hodnoty nemají vliv na argument, který byl předán parametr.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-498">Modifications to a value parameter do not affect the argument that was passed for the parameter.</span></span>

<span data-ttu-id="fb7c7-499">Parametry s hodnotou může být volitelný, a zadat výchozí hodnotu tak, aby odpovídající argumenty lze vynechat.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-499">Value parameters can be optional, by specifying a default value so that corresponding arguments can be omitted.</span></span>

<span data-ttu-id="fb7c7-500">A ***odkazovat na parametr*** se používá pro vstup a výstup předávání parametrů.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-500">A ***reference parameter*** is used for both input and output parameter passing.</span></span> <span data-ttu-id="fb7c7-501">Argument předaný pro referenční parametr musí být proměnná a při provádění metody referenční parametr představuje stejné úložiště jako argument proměnné.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-501">The argument passed for a reference parameter must be a variable, and during execution of the method, the reference parameter represents the same storage location as the argument variable.</span></span> <span data-ttu-id="fb7c7-502">Parametr odkazu je deklarována s `ref` modifikátor.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-502">A reference parameter is declared with the `ref` modifier.</span></span> <span data-ttu-id="fb7c7-503">Následující příklad ukazuje použití `ref` parametry.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-503">The following example shows the use of `ref` parameters.</span></span>

```csharp
using System;

class Test
{
    static void Swap(ref int x, ref int y) {
        int temp = x;
        x = y;
        y = temp;
    }

    static void Main() {
        int i = 1, j = 2;
        Swap(ref i, ref j);
        Console.WriteLine("{0} {1}", i, j);            // Outputs "2 1"
    }
}
```
<span data-ttu-id="fb7c7-504">***Výstupní parametr*** slouží k předávání parametru výstupu.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-504">An ***output parameter*** is used for output parameter passing.</span></span> <span data-ttu-id="fb7c7-505">Výstupní parametr je podobný parametr odkazu, s tím rozdílem, že počáteční hodnota argumentu, pokud volající není důležitý.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-505">An output parameter is similar to a reference parameter except that the initial value of the caller-provided argument is unimportant.</span></span> <span data-ttu-id="fb7c7-506">Výstupní parametr je deklarována s `out` modifikátor.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-506">An output parameter is declared with the `out` modifier.</span></span> <span data-ttu-id="fb7c7-507">Následující příklad ukazuje použití `out` parametry.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-507">The following example shows the use of `out` parameters.</span></span>

```csharp
using System;

class Test
{
    static void Divide(int x, int y, out int result, out int remainder) {
        result = x / y;
        remainder = x % y;
    }

    static void Main() {
        int res, rem;
        Divide(10, 3, out res, out rem);
        Console.WriteLine("{0} {1}", res, rem);    // Outputs "3 1"
    }
}
```
<span data-ttu-id="fb7c7-508">A ***pole parametrů*** povoluje proměnlivý počet argumentů, které mají být předána metodě.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-508">A ***parameter array*** permits a variable number of arguments to be passed to a method.</span></span> <span data-ttu-id="fb7c7-509">Pole parametrů je deklarována s `params` modifikátor.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-509">A parameter array is declared with the `params` modifier.</span></span> <span data-ttu-id="fb7c7-510">Poslední parametr metody může být pole parametrů a typ pole parametrů musí být typu jednorozměrné pole.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-510">Only the last parameter of a method can be a parameter array, and the type of a parameter array must be a single-dimensional array type.</span></span> <span data-ttu-id="fb7c7-511">`Write` a `WriteLine` metody `System.Console` třídy jsou dobrým příkladem použití pole parametrů.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-511">The `Write` and `WriteLine` methods of the `System.Console` class are good examples of parameter array usage.</span></span> <span data-ttu-id="fb7c7-512">Jsou deklarovány následujícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-512">They are declared as follows.</span></span>

```csharp
public class Console
{
    public static void Write(string fmt, params object[] args) {...}
    public static void WriteLine(string fmt, params object[] args) {...}
    ...
}
```
<span data-ttu-id="fb7c7-513">Pole parametrů se v rámci metody, která používá pole parametrů, chová stejně jako regulární parametr typu pole.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-513">Within a method that uses a parameter array, the parameter array behaves exactly like a regular parameter of an array type.</span></span> <span data-ttu-id="fb7c7-514">Ve volání metody s polem parametrů, je možné předat buď jeden argument typu pole parametru nebo libovolný počet argumentů typu prvku pole parametrů.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-514">However, in an invocation of a method with a parameter array, it is possible to pass either a single argument of the parameter array type or any number of arguments of the element type of the parameter array.</span></span> <span data-ttu-id="fb7c7-515">V druhém případě pole instance je automaticky vytvořen a inicializován pomocí dané argumenty.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-515">In the latter case, an array instance is automatically created and initialized with the given arguments.</span></span> <span data-ttu-id="fb7c7-516">V tomto příkladu</span><span class="sxs-lookup"><span data-stu-id="fb7c7-516">This example</span></span>

```csharp
Console.WriteLine("x={0} y={1} z={2}", x, y, z);
```
<span data-ttu-id="fb7c7-517">je ekvivalentní zápisu následující.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-517">is equivalent to writing the following.</span></span>

```csharp
string s = "x={0} y={1} z={2}";
object[] args = new object[3];
args[0] = x;
args[1] = y;
args[2] = z;
Console.WriteLine(s, args);
```

#### <a name="method-body-and-local-variables"></a><span data-ttu-id="fb7c7-518">Tělo metody a lokální proměnné</span><span class="sxs-lookup"><span data-stu-id="fb7c7-518">Method body and local variables</span></span>

<span data-ttu-id="fb7c7-519">Tělo metody určuje příkazy ke spuštění při vyvolání metody.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-519">A method's body specifies the statements to execute when the method is invoked.</span></span>

<span data-ttu-id="fb7c7-520">Tělo metody můžete deklarovat proměnné, které jsou specifické pro vyvolání metody.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-520">A method body can declare variables that are specific to the invocation of the method.</span></span> <span data-ttu-id="fb7c7-521">Tyto proměnné jsou volány ***lokální proměnné***.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-521">Such variables are called ***local variables***.</span></span> <span data-ttu-id="fb7c7-522">Místní deklarace proměnné Určuje název typu, název proměnné a případně na počáteční hodnotu.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-522">A local variable declaration specifies a type name, a variable name, and possibly an initial value.</span></span> <span data-ttu-id="fb7c7-523">Následující příklad deklaruje místní proměnnou `i` s počáteční hodnotou nula a místní proměnnou `j` s žádná počáteční hodnota.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-523">The following example declares a local variable `i` with an initial value of zero and a local variable `j` with no initial value.</span></span>

```csharp
using System;

class Squares
{
    static void Main() {
        int i = 0;
        int j;
        while (i < 10) {
            j = i * i;
            Console.WriteLine("{0} x {0} = {1}", i, j);
            i = i + 1;
        }
    }
}
```
<span data-ttu-id="fb7c7-524">C# vyžaduje místní proměnnou ***jednoznačně přiřazena*** před její hodnotu lze získat.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-524">C# requires a local variable to be ***definitely assigned*** before its value can be obtained.</span></span> <span data-ttu-id="fb7c7-525">Například pokud deklarace předchozí `i` počáteční hodnotu neobsahuje, kompilátor by oznámit chybu pro následné použití `i` protože `i` nemusí být jednoznačně přiřazena v těchto bodech v programu.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-525">For example, if the declaration of the previous `i` did not include an initial value, the compiler would report an error for the subsequent usages of `i` because `i` would not be definitely assigned at those points in the program.</span></span>

<span data-ttu-id="fb7c7-526">Můžete použít metodu `return` příkazy vrátí řízení volajícímu.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-526">A method can use `return` statements to return control to its caller.</span></span> <span data-ttu-id="fb7c7-527">Do metody vracející `void`, `return` příkazy nemohou zadat výraz.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-527">In a method returning `void`, `return` statements cannot specify an expression.</span></span> <span data-ttu-id="fb7c7-528">Metoda vrací jinou hodnotu než`void`, `return` příkazů musí obsahovat výraz, který vypočítá návratovou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-528">In a method returning non-`void`, `return` statements must include an expression that computes the return value.</span></span>

#### <a name="static-and-instance-methods"></a><span data-ttu-id="fb7c7-529">Statické a instance metody</span><span class="sxs-lookup"><span data-stu-id="fb7c7-529">Static and instance methods</span></span>

<span data-ttu-id="fb7c7-530">Metoda deklarována s `static` modifikátor ***statickou metodu***.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-530">A method declared with a `static` modifier is a ***static method***.</span></span> <span data-ttu-id="fb7c7-531">Statická metoda nefunguje na konkrétní instanci a pouze přímý přístup k statické členy.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-531">A static method does not operate on a specific instance and can only directly access static members.</span></span>

<span data-ttu-id="fb7c7-532">Metoda deklarovaná bez `static` modifikátor ***metodu instance***.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-532">A method declared without a `static` modifier is an ***instance method***.</span></span> <span data-ttu-id="fb7c7-533">Metodu instance funguje na konkrétní instanci, můžete přístup ke statické a členy instance.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-533">An instance method operates on a specific instance and can access both static and instance members.</span></span> <span data-ttu-id="fb7c7-534">Instance, ve kterém byla vyvolána metoda instance je explicitně přístupná jako `this`.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-534">The instance on which an instance method was invoked can be explicitly accessed as `this`.</span></span> <span data-ttu-id="fb7c7-535">Jedná se o chybu k odkazování na `this` uvnitř statické metody.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-535">It is an error to refer to `this` in a static method.</span></span>

<span data-ttu-id="fb7c7-536">Následující `Entity` má statické třídy a členy instance.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-536">The following `Entity` class has both static and instance members.</span></span>

```csharp
class Entity
{
    static int nextSerialNo;
    int serialNo;

    public Entity() {
        serialNo = nextSerialNo++;
    }

    public int GetSerialNo() {
        return serialNo;
    }

    public static int GetNextSerialNo() {
        return nextSerialNo;
    }

    public static void SetNextSerialNo(int value) {
        nextSerialNo = value;
    }
}
```
<span data-ttu-id="fb7c7-537">Každý `Entity` instance obsahuje sériové číslo (a pravděpodobně některé další informace, které tady není ukázaný).</span><span class="sxs-lookup"><span data-stu-id="fb7c7-537">Each `Entity` instance contains a serial number (and presumably some other information that is not shown here).</span></span> <span data-ttu-id="fb7c7-538">`Entity` Konstruktoru (což je jako metoda instance) inicializuje novou instanci s další dostupné sériové číslo.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-538">The `Entity` constructor (which is like an instance method) initializes the new instance with the next available serial number.</span></span> <span data-ttu-id="fb7c7-539">Protože konstruktor je členem instance, je povoleno přístup i `serialNo` pole instance a `nextSerialNo` statické pole.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-539">Because the constructor is an instance member, it is permitted to access both the `serialNo` instance field and the `nextSerialNo` static field.</span></span>

<span data-ttu-id="fb7c7-540">`GetNextSerialNo` a `SetNextSerialNo` statické metody se dostanete `nextSerialNo` statické pole, ale bude k chybě pro ně umožňuje přímý přístup k `serialNo` pole instance.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-540">The `GetNextSerialNo` and `SetNextSerialNo` static methods can access the `nextSerialNo` static field, but it would be an error for them to directly access the `serialNo` instance field.</span></span>

<span data-ttu-id="fb7c7-541">Následující příklad ukazuje použití `Entity` třídy.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-541">The following example shows the use of the `Entity` class.</span></span>

```csharp
using System;

class Test
{
    static void Main() {
        Entity.SetNextSerialNo(1000);
        Entity e1 = new Entity();
        Entity e2 = new Entity();
        Console.WriteLine(e1.GetSerialNo());           // Outputs "1000"
        Console.WriteLine(e2.GetSerialNo());           // Outputs "1001"
        Console.WriteLine(Entity.GetNextSerialNo());   // Outputs "1002"
    }
}
```
<span data-ttu-id="fb7c7-542">Všimněte si, že `SetNextSerialNo` a `GetNextSerialNo` statické metody jsou vyvolány ve třídě, kdežto `GetSerialNo` metodu instance se vyvolá u instance třídy.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-542">Note that the `SetNextSerialNo` and `GetNextSerialNo` static methods are invoked on the class whereas the `GetSerialNo` instance method is invoked on instances of the class.</span></span>

#### <a name="virtual-override-and-abstract-methods"></a><span data-ttu-id="fb7c7-543">Virtual, override a abstraktní metody</span><span class="sxs-lookup"><span data-stu-id="fb7c7-543">Virtual, override, and abstract methods</span></span>

<span data-ttu-id="fb7c7-544">Při deklaraci instance metody obsahuje `virtual` modifikátor, metoda se říká, že ***virtuální metoda***.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-544">When an instance method declaration includes a `virtual` modifier, the method is said to be a ***virtual method***.</span></span> <span data-ttu-id="fb7c7-545">Pokud ne `virtual` Modifikátor je k dispozici, metodu se říká, že ***nevirtuální metoda***.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-545">When no `virtual` modifier is present, the method is said to be a ***non-virtual method***.</span></span>

<span data-ttu-id="fb7c7-546">Při vyvolání virtuální metody, ***run-time typu*** instance, pro který přebírá tento vyvolání místo určuje implementaci skutečné metoda k vyvolání.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-546">When a virtual method is invoked, the ***run-time type*** of the instance for which that invocation takes place determines the actual method implementation to invoke.</span></span> <span data-ttu-id="fb7c7-547">Ve volání nevirtuální metody ***typu v době kompilace*** instance je určujícím faktorem.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-547">In a nonvirtual method invocation, the ***compile-time type*** of the instance is the determining factor.</span></span>

<span data-ttu-id="fb7c7-548">Virtuální metoda může být ***přepsat*** v odvozené třídě.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-548">A virtual method can be ***overridden*** in a derived class.</span></span> <span data-ttu-id="fb7c7-549">Při deklaraci instance metody obsahuje `override` Modifikátor metody přepsání zděděné virtuální metody se stejným podpisem.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-549">When an instance method declaration includes an `override` modifier, the method overrides an inherited virtual method with the same signature.</span></span> <span data-ttu-id="fb7c7-550">Vzhledem k tomu virtuální metoda deklarace zavádí nové metody, specializuje deklaraci metody přepsání existující zděděnou virtuální metodu tím, že poskytuje novou implementaci této metody.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-550">Whereas a virtual method declaration introduces a new method, an override method declaration specializes an existing inherited virtual method by providing a new implementation of that method.</span></span>

<span data-ttu-id="fb7c7-551">***Abstraktní*** je virtuální metoda bez implementace.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-551">An ***abstract*** method is a virtual method with no implementation.</span></span> <span data-ttu-id="fb7c7-552">Abstraktní metoda je deklarována s `abstract` modifikátor a smí obsahovat pouze ve třídě, která je také deklarován `abstract`.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-552">An abstract method is declared with the `abstract` modifier and is permitted only in a class that is also declared `abstract`.</span></span> <span data-ttu-id="fb7c7-553">Abstraktní metody musí být přepsána v každé třídě odvozené neabstraktní.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-553">An abstract method must be overridden in every non-abstract derived class.</span></span>

<span data-ttu-id="fb7c7-554">Následující příklad deklaruje abstraktní třídu, `Expression`, který představuje uzel stromu výrazu a tři odvozené třídy, `Constant`, `VariableReference`, a `Operation`, které implementují uzly stromu výrazů konstant, proměnných Reference a aritmetické operace.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-554">The following example declares an abstract class, `Expression`, which represents an expression tree node, and three derived classes, `Constant`, `VariableReference`, and `Operation`, which implement expression tree nodes for constants, variable references, and arithmetic operations.</span></span> <span data-ttu-id="fb7c7-555">(Je to podobné, ale neměl by se zaměňovat s typy výrazů stromu počínaje [typy stromu výrazů](types.md#expression-tree-types)).</span><span class="sxs-lookup"><span data-stu-id="fb7c7-555">(This is similar to, but not to be confused with the expression tree types introduced in [Expression tree types](types.md#expression-tree-types)).</span></span>

```csharp
using System;
using System.Collections;

public abstract class Expression
{
    public abstract double Evaluate(Hashtable vars);
}

public class Constant: Expression
{
    double value;

    public Constant(double value) {
        this.value = value;
    }

    public override double Evaluate(Hashtable vars) {
        return value;
    }
}

public class VariableReference: Expression
{
    string name;

    public VariableReference(string name) {
        this.name = name;
    }

    public override double Evaluate(Hashtable vars) {
        object value = vars[name];
        if (value == null) {
            throw new Exception("Unknown variable: " + name);
        }
        return Convert.ToDouble(value);
    }
}

public class Operation: Expression
{
    Expression left;
    char op;
    Expression right;

    public Operation(Expression left, char op, Expression right) {
        this.left = left;
        this.op = op;
        this.right = right;
    }

    public override double Evaluate(Hashtable vars) {
        double x = left.Evaluate(vars);
        double y = right.Evaluate(vars);
        switch (op) {
            case '+': return x + y;
            case '-': return x - y;
            case '*': return x * y;
            case '/': return x / y;
        }
        throw new Exception("Unknown operator");
    }
}
```
<span data-ttu-id="fb7c7-556">Předchozí čtyři třídy lze použít k modelování aritmetických výrazech.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-556">The previous four classes can be used to model arithmetic expressions.</span></span> <span data-ttu-id="fb7c7-557">Například použití instance těchto tříd, výraz `x + 3` můžou být vyjádřeny následujícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-557">For example, using instances of these classes, the expression `x + 3` can be represented as follows.</span></span>

```csharp
Expression e = new Operation(
    new VariableReference("x"),
    '+',
    new Constant(3));
```
<span data-ttu-id="fb7c7-558">`Evaluate` Metodu `Expression` instance se vyvolá, aby vyhodnotit tento výraz a vytvářet `double` hodnotu.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-558">The `Evaluate` method of an `Expression` instance is invoked to evaluate the given expression and produce a `double` value.</span></span> <span data-ttu-id="fb7c7-559">Tato metoda přebírá jako argument `Hashtable` , který obsahuje názvy proměnných (jako klíče položky) a hodnoty (jako hodnoty položek).</span><span class="sxs-lookup"><span data-stu-id="fb7c7-559">The method takes as an argument a `Hashtable` that contains variable names (as keys of the entries) and values (as values of the entries).</span></span> <span data-ttu-id="fb7c7-560">`Evaluate` Je virtuální abstraktní metoda, to znamená neabstraktní odvozené třídy musí přepsat je k dispozici skutečné implementace.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-560">The `Evaluate` method is a virtual abstract method, meaning that non-abstract derived classes must override it to provide an actual implementation.</span></span>

<span data-ttu-id="fb7c7-561">A `Constant`vaší implementace `Evaluate` jednoduše vrací uložené – konstanta.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-561">A `Constant`'s implementation of `Evaluate` simply returns the stored constant.</span></span> <span data-ttu-id="fb7c7-562">A `VariableReference`vaší implementace vyhledá název proměnné v zatřiďovací tabulky a vrátí výslednou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-562">A `VariableReference`'s implementation looks up the variable name in the hashtable and returns the resulting value.</span></span> <span data-ttu-id="fb7c7-563">`Operation`Na provádění nejprve vyhodnotí jako operandy vlevo a vpravo (vyvoláním rekurzivně jejich `Evaluate` metody) a potom provede daný aritmetické operace.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-563">An `Operation`'s implementation first evaluates the left and right operands (by recursively invoking their `Evaluate` methods) and then performs the given arithmetic operation.</span></span>

<span data-ttu-id="fb7c7-564">Následující program používá `Expression` třídy pro vyhodnocení výrazu `x * (y + 2)` pro různé hodnoty `x` a `y`.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-564">The following program uses the `Expression` classes to evaluate the expression `x * (y + 2)` for different values of `x` and `y`.</span></span>

```csharp
using System;
using System.Collections;

class Test
{
    static void Main() {
        Expression e = new Operation(
            new VariableReference("x"),
            '*',
            new Operation(
                new VariableReference("y"),
                '+',
                new Constant(2)
            )
        );
        Hashtable vars = new Hashtable();
        vars["x"] = 3;
        vars["y"] = 5;
        Console.WriteLine(e.Evaluate(vars));        // Outputs "21"
        vars["x"] = 1.5;
        vars["y"] = 9;
        Console.WriteLine(e.Evaluate(vars));        // Outputs "16.5"
    }
}
```

#### <a name="method-overloading"></a><span data-ttu-id="fb7c7-565">Přetěžování metody</span><span class="sxs-lookup"><span data-stu-id="fb7c7-565">Method overloading</span></span>

<span data-ttu-id="fb7c7-566">Metoda ***přetížení*** povoluje více metod ve stejné třídě, aby mají stejný název tak dlouho, dokud mají jedinečný podpis.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-566">Method ***overloading*** permits multiple methods in the same class to have the same name as long as they have unique signatures.</span></span> <span data-ttu-id="fb7c7-567">Při kompilaci vyvolání přetěžované metody, kterou kompilátor používá ***rozlišení přetěžování*** určit konkrétní metodu chce volat.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-567">When compiling an invocation of an overloaded method, the compiler uses ***overload resolution*** to determine the specific method to invoke.</span></span> <span data-ttu-id="fb7c7-568">Řešení přetížení vyhledá jednu metodu, nejlépe odpovídá argumenty nebo hlásí chybu, pokud lze najít žádné jediné nejlepší shodu.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-568">Overload resolution finds the one method that best matches the arguments or reports an error if no single best match can be found.</span></span> <span data-ttu-id="fb7c7-569">Následující příklad ukazuje přetížení v platnosti.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-569">The following example shows overload resolution in effect.</span></span> <span data-ttu-id="fb7c7-570">Komentář pro každé vyvolání v `Main` metoda ukazuje, jakou metodu ve skutečnosti je vyvolána.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-570">The comment for each invocation in the `Main` method shows which method is actually invoked.</span></span>

```csharp
class Test
{
    static void F() {
        Console.WriteLine("F()");
    }

    static void F(object x) {
        Console.WriteLine("F(object)");
    }

    static void F(int x) {
        Console.WriteLine("F(int)");
    }

    static void F(double x) {
        Console.WriteLine("F(double)");
    }

    static void F<T>(T x) {
        Console.WriteLine("F<T>(T)");
    }

    static void F(double x, double y) {
        Console.WriteLine("F(double, double)");
    }

    static void Main() {
        F();                 // Invokes F()
        F(1);                // Invokes F(int)
        F(1.0);              // Invokes F(double)
        F("abc");            // Invokes F(object)
        F((double)1);        // Invokes F(double)
        F((object)1);        // Invokes F(object)
        F<int>(1);           // Invokes F<T>(T)
        F(1, 1);             // Invokes F(double, double)
    }
}
```
<span data-ttu-id="fb7c7-571">Jak je znázorněno v příkladu, konkrétní metody lze vybrat vždy explicitně použití argumentů na typy parametrů přesný nebo explicitně zadávání argumentů typu.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-571">As shown by the example, a particular method can always be selected by explicitly casting the arguments to the exact parameter types and/or explicitly supplying type arguments.</span></span>

### <a name="other-function-members"></a><span data-ttu-id="fb7c7-572">Ostatní členové – funkce</span><span class="sxs-lookup"><span data-stu-id="fb7c7-572">Other function members</span></span>

<span data-ttu-id="fb7c7-573">Členy, které obsahují spustitelného kódu jsou souhrnně označovány jako ***funkce členy*** třídy.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-573">Members that contain executable code are collectively known as the ***function members*** of a class.</span></span> <span data-ttu-id="fb7c7-574">Předchozí část popisuje metody, které jsou primární druh členy funkce.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-574">The preceding section describes methods, which are the primary kind of function members.</span></span> <span data-ttu-id="fb7c7-575">Tato část popisuje další druhy členů funkce podporované v jazyce C#: konstruktory, vlastnosti, indexery, události, operátory a destruktory.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-575">This section describes the other kinds of function members supported by C#: constructors, properties, indexers, events, operators, and destructors.</span></span>

<span data-ttu-id="fb7c7-576">Následující kód ukazuje obecnou třídu volá `List<T>`, který implementuje growable seznam objektů.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-576">The following code shows a generic class called `List<T>`, which implements a growable list of objects.</span></span> <span data-ttu-id="fb7c7-577">Třída obsahuje několik příkladů nejběžnější druhy členů funkce.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-577">The class contains several examples of the most common kinds of function members.</span></span>


```csharp
public class List<T> {
    // Constant...
    const int defaultCapacity = 4;

    // Fields...
    T[] items;
    int count;

    // Constructors...
    public List(int capacity = defaultCapacity) {
        items = new T[capacity];
    }

    // Properties...
    public int Count {
        get { return count; }
    }
    public int Capacity {
        get {
            return items.Length;
        }
        set {
            if (value < count) value = count;
            if (value != items.Length) {
                T[] newItems = new T[value];
                Array.Copy(items, 0, newItems, 0, count);
                items = newItems;
            }
        }
    }

    // Indexer...
    public T this[int index] {
        get {
            return items[index];
        }
        set {
            items[index] = value;
            OnChanged();
        }
    }

    // Methods...
    public void Add(T item) {
        if (count == Capacity) Capacity = count * 2;
        items[count] = item;
        count++;
        OnChanged();
    }
    protected virtual void OnChanged() {
        if (Changed != null) Changed(this, EventArgs.Empty);
    }
    public override bool Equals(object other) {
        return Equals(this, other as List<T>);
    }
    static bool Equals(List<T> a, List<T> b) {
        if (a == null) return b == null;
        if (b == null || a.count != b.count) return false;
        for (int i = 0; i < a.count; i++) {
            if (!object.Equals(a.items[i], b.items[i])) {
                return false;
            }
        }
        return true;
    }

    // Event...
    public event EventHandler Changed;

    // Operators...
    public static bool operator ==(List<T> a, List<T> b) {
        return Equals(a, b);
    }
    public static bool operator !=(List<T> a, List<T> b) {
        return !Equals(a, b);
    }
}
```

#### <a name="constructors"></a><span data-ttu-id="fb7c7-578">Konstruktory</span><span class="sxs-lookup"><span data-stu-id="fb7c7-578">Constructors</span></span>

<span data-ttu-id="fb7c7-579">C# podporuje instance a statické konstruktory.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-579">C# supports both instance and static constructors.</span></span> <span data-ttu-id="fb7c7-580">***Konstruktor instance*** je člen, který implementuje akce potřebné k inicializaci instance třídy.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-580">An ***instance constructor*** is a member that implements the actions required to initialize an instance of a class.</span></span> <span data-ttu-id="fb7c7-581">A ***statický konstruktor*** je člen, který implementuje akce potřebné k inicializaci třídy samotný při prvním načtení.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-581">A ***static constructor*** is a member that implements the actions required to initialize a class itself when it is first loaded.</span></span>

<span data-ttu-id="fb7c7-582">Konstruktor je deklarován jako metoda bez návratového typu a stejný název jako třídu obsahující.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-582">A constructor is declared like a method with no return type and the same name as the containing class.</span></span> <span data-ttu-id="fb7c7-583">Pokud obsahuje deklaraci konstruktoru `static` modifikátor, deklaruje statický konstruktor.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-583">If a constructor declaration includes a `static` modifier, it declares a static constructor.</span></span> <span data-ttu-id="fb7c7-584">V opačném případě deklaruje konstruktor instance.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-584">Otherwise, it declares an instance constructor.</span></span>

<span data-ttu-id="fb7c7-585">Konstruktory instancí můžou být přetížené.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-585">Instance constructors can be overloaded.</span></span> <span data-ttu-id="fb7c7-586">Například `List<T>
` třída deklaruje dva konstruktory instancí, jednu s žádné parametry a ten, který přebírá `int` parametru.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-586">For example, the `List<T>
` class declares two instance constructors, one with no parameters and one that takes an `int` parameter.</span></span> <span data-ttu-id="fb7c7-587">Konstruktory instancí jsou vyvolány pomocí `new` operátor.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-587">Instance constructors are invoked using the `new` operator.</span></span> <span data-ttu-id="fb7c7-588">Následující příkazy přidělit dvě `List<string>
` instance pomocí všech konstruktory `List` třídy.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-588">The following statements allocate two `List<string>
` instances using each of the constructors of the `List` class.</span></span>

```csharp
List<string> list1 = new List<string>();
List<string> list2 = new List<string>(10);
```
<span data-ttu-id="fb7c7-589">Na rozdíl od jiných členů nejsou zděděné konstruktory instancí a třída nemá žádné konstruktory instance než tyto skutečně deklarovaná ve třídě.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-589">Unlike other members, instance constructors are not inherited, and a class has no instance constructors other than those actually declared in the class.</span></span> <span data-ttu-id="fb7c7-590">Pokud žádný konstruktor instance není zadána pro třídu, pak prázdná bez parametrů je automaticky zadáno.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-590">If no instance constructor is supplied for a class, then an empty one with no parameters is automatically provided.</span></span>

#### <a name="properties"></a><span data-ttu-id="fb7c7-591">Vlastnosti</span><span class="sxs-lookup"><span data-stu-id="fb7c7-591">Properties</span></span>

<span data-ttu-id="fb7c7-592">***Vlastnosti*** jsou přirozené rozšíření polí.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-592">***Properties*** are a natural extension of fields.</span></span> <span data-ttu-id="fb7c7-593">Jsou pojmenované členy s přidružené typy a syntaxe pro přístup k vlastnosti a pole je stejná.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-593">Both are named members with associated types, and the syntax for accessing fields and properties is the same.</span></span> <span data-ttu-id="fb7c7-594">Na rozdíl od pole, ale vlastnosti nejsou označení umístění úložiště.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-594">However, unlike fields, properties do not denote storage locations.</span></span> <span data-ttu-id="fb7c7-595">Místo toho mají vlastnosti ***přistupující objekty*** , které určují příkazy ke se spustí při jejich hodnoty jsou číst nebo zapisovat.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-595">Instead, properties have ***accessors*** that specify the statements to be executed when their values are read or written.</span></span>

<span data-ttu-id="fb7c7-596">Vlastnost je deklarován jako pole, s tím rozdílem, že deklarace končí `get` přístupového objektu a/nebo `set` přistupující objekt napsané mezi oddělovači `{` a `}` místo končí středníkem.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-596">A property is declared like a field, except that the declaration ends with a `get` accessor and/or a `set` accessor written between the delimiters `{` and `}` instead of ending in a semicolon.</span></span> <span data-ttu-id="fb7c7-597">Vlastnost, která má oba `get` přístupového objektu a `set` přístupový objekt není ***čtení a zápis vlastnosti***, vlastnost, která má pouze `get` přístupový objekt není ***vlastnost jen pro čtení***a vlastnost, která má pouze `set` přístupový objekt není ***vlastnost jen pro zápis***.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-597">A property that has both a `get` accessor and a `set` accessor is a ***read-write property***, a property that has only a `get` accessor is a ***read-only property***, and a property that has only a `set` accessor is a ***write-only property***.</span></span>

<span data-ttu-id="fb7c7-598">A `get` přistupující objekt odpovídající konstruktor bez parametrů metody s návratovou hodnotou typu vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-598">A `get` accessor corresponds to a parameterless method with a return value of the property type.</span></span> <span data-ttu-id="fb7c7-599">Při odkazování na vlastnost ve výrazu, s výjimkou případů cílem přiřazení, `get` přistupující objekt vlastnosti je vyvolána k výpočtu hodnoty vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-599">Except as the target of an assignment, when a property is referenced in an expression, the `get` accessor of the property is invoked to compute the value of the property.</span></span>

<span data-ttu-id="fb7c7-600">A `set` přistupující objekt odpovídá metodu s jedním parametrem s názvem `value` a bez návratového typu.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-600">A `set` accessor corresponds to a method with a single parameter named `value` and no return type.</span></span> <span data-ttu-id="fb7c7-601">Když je jako cíl přiřazení nebo jako operand odkazuje vlastnost `++` nebo `--`, `set` přístupového objektu je volána s argumentem, který obsahuje novou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-601">When a property is referenced as the target of an assignment or as the operand of `++` or `--`, the `set` accessor is invoked with an argument that provides the new value.</span></span>

<span data-ttu-id="fb7c7-602">`List<T>
` Třída deklaruje dvě vlastnosti `Count` a `Capacity`, které jsou v uvedeném pořadí jen pro čtení a čtení i zápis.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-602">The `List<T>
` class declares two properties, `Count` and `Capacity`, which are read-only and read-write, respectively.</span></span> <span data-ttu-id="fb7c7-603">Následuje příklad použití těchto vlastností.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-603">The following is an example of use of these properties.</span></span>

```csharp
List<string> names = new List<string>();
names.Capacity = 100;            // Invokes set accessor
int i = names.Count;             // Invokes get accessor
int j = names.Capacity;          // Invokes get accessor
```
<span data-ttu-id="fb7c7-604">Podobně jako k polím a metodám, C# podporuje vlastnosti instance a statické vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-604">Similar to fields and methods, C# supports both instance properties and static properties.</span></span> <span data-ttu-id="fb7c7-605">Statické vlastnosti jsou deklarovány pomocí `static` modifikátor a vlastnosti instance jsou deklarovány bez něj.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-605">Static properties are declared with the `static` modifier, and instance properties are declared without it.</span></span>

<span data-ttu-id="fb7c7-606">Accessor(s) vlastnost může být virtuální.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-606">The accessor(s) of a property can be virtual.</span></span> <span data-ttu-id="fb7c7-607">Pokud obsahuje deklaraci vlastnosti `virtual`, `abstract`, nebo `override` modifikátor, se vztahuje na accessor(s) vlastnost.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-607">When a property declaration includes a `virtual`, `abstract`, or `override` modifier, it applies to the accessor(s) of the property.</span></span>

#### <a name="indexers"></a><span data-ttu-id="fb7c7-608">Indexery</span><span class="sxs-lookup"><span data-stu-id="fb7c7-608">Indexers</span></span>

<span data-ttu-id="fb7c7-609">***Indexer*** je člen, který umožňuje objekty, které mají být indexovány v stejným způsobem jako pole.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-609">An ***indexer*** is a member that enables objects to be indexed in the same way as an array.</span></span> <span data-ttu-id="fb7c7-610">Indexer je deklarován jako vlastnost, s tím rozdílem, že je název člena `this` za nímž následuje seznam parametrů napsané mezi oddělovači `[` a `]`.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-610">An indexer is declared like a property except that the name of the member is `this` followed by a parameter list written between the delimiters `[` and `]`.</span></span> <span data-ttu-id="fb7c7-611">Parametry jsou k dispozici v accessor(s) indexeru.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-611">The parameters are available in the accessor(s) of the indexer.</span></span> <span data-ttu-id="fb7c7-612">Podobně jako u vlastnosti, indexery mohou být pro čtení i zápis, jen pro čtení a jen pro zápis a accessor(s) indexer může být virtuální.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-612">Similar to properties, indexers can be read-write, read-only, and write-only, and the accessor(s) of an indexer can be virtual.</span></span>

<span data-ttu-id="fb7c7-613">`List` Třída deklaruje jednu indexeru pro čtení i zápis, která přebírá `int` parametru.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-613">The `List` class declares a single read-write indexer that takes an `int` parameter.</span></span> <span data-ttu-id="fb7c7-614">Indexer umožňuje index `List` instance s `int` hodnoty.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-614">The indexer makes it possible to index `List` instances with `int` values.</span></span> <span data-ttu-id="fb7c7-615">Příklad</span><span class="sxs-lookup"><span data-stu-id="fb7c7-615">For example</span></span>

```csharp
List<string> names = new List<string>();
names.Add("Liz");
names.Add("Martha");
names.Add("Beth");
for (int i = 0; i < names.Count; i++) {
    string s = names[i];
    names[i] = s.ToUpper();
}
```
<span data-ttu-id="fb7c7-616">Indexery mohou být přetíženy, což znamená, že třídy lze deklarovat několik indexerů jako číslo nebo typů jejich parametrů se liší.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-616">Indexers can be overloaded, meaning that a class can declare multiple indexers as long as the number or types of their parameters differ.</span></span>

#### <a name="events"></a><span data-ttu-id="fb7c7-617">Události</span><span class="sxs-lookup"><span data-stu-id="fb7c7-617">Events</span></span>

<span data-ttu-id="fb7c7-618">***Události*** je člen, který umožňuje třídu nebo objektu pro sdělení.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-618">An ***event*** is a member that enables a class or object to provide notifications.</span></span> <span data-ttu-id="fb7c7-619">Událost je deklarován jako pole, s tím rozdílem, že obsahuje prohlášení `event` – klíčové slovo a typ musí být typu delegáta.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-619">An event is declared like a field except that the declaration includes an `event` keyword and the type must be a delegate type.</span></span>

<span data-ttu-id="fb7c7-620">V rámci třídy, které deklaruje člen události události se chová stejně jako pole s typem delegáta (za předpokladu, události není abstraktní a nedeklaruje přistupující objekty).</span><span class="sxs-lookup"><span data-stu-id="fb7c7-620">Within a class that declares an event member, the event behaves just like a field of a delegate type (provided the event is not abstract and does not declare accessors).</span></span> <span data-ttu-id="fb7c7-621">Toto pole obsahuje odkaz na delegáta, který představuje obslužné rutiny událostí, které byly přidány k této události.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-621">The field stores a reference to a delegate that represents the event handlers that have been added to the event.</span></span> <span data-ttu-id="fb7c7-622">Pokud neexistují žádné obslužné rutiny událostí, je pole `null`.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-622">If no event handles are present, the field is `null`.</span></span>

<span data-ttu-id="fb7c7-623">`List<T>
` Třída deklaruje člen jedna událost s názvem `Changed`, což znamená, že se do seznamu přidá nová položka.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-623">The `List<T>
` class declares a single event member called `Changed`, which indicates that a new item has been added to the list.</span></span> <span data-ttu-id="fb7c7-624">`Changed` Vyvolá událost `OnChanged` virtuální metody, které nejprve zkontroluje, zda je událost `null` (to znamená, že jsou k dispozici žádné obslužné rutiny).</span><span class="sxs-lookup"><span data-stu-id="fb7c7-624">The `Changed` event is raised by the `OnChanged` virtual method, which first checks whether the event is `null` (meaning that no handlers are present).</span></span> <span data-ttu-id="fb7c7-625">Pojem vyvolání události je přesně ekvivalentní k vyvolání delegáta představované událostí – to znamená, nejsou žádné zvláštní jazykovým konstrukcím pro vyvolání události.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-625">The notion of raising an event is precisely equivalent to invoking the delegate represented by the event—thus, there are no special language constructs for raising events.</span></span>

<span data-ttu-id="fb7c7-626">Klienti reagovat na události prostřednictvím ***obslužné rutiny událostí***.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-626">Clients react to events through ***event handlers***.</span></span> <span data-ttu-id="fb7c7-627">Obslužné rutiny událostí se připojují pomocí `+=` operátor a odebrané pomocí `-=` operátor.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-627">Event handlers are attached using the `+=` operator and removed using the `-=` operator.</span></span> <span data-ttu-id="fb7c7-628">Následující příklad připojí obslužnou rutinu události pro `Changed` události `List<string>
`.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-628">The following example attaches an event handler to the `Changed` event of a `List<string>
`.</span></span>

```csharp
using System;

class Test
{
    static int changeCount;

    static void ListChanged(object sender, EventArgs e) {
        changeCount++;
    }

    static void Main() {
        List<string> names = new List<string>();
        names.Changed += new EventHandler(ListChanged);
        names.Add("Liz");
        names.Add("Martha");
        names.Add("Beth");
        Console.WriteLine(changeCount);        // Outputs "3"
    }
}
```
<span data-ttu-id="fb7c7-629">Pro pokročilé scénáře, kde je žádoucí ovládací prvek pro použité úložiště událostí, můžete explicitně uvést deklaraci události `add` a `remove` přístupové objekty, které jsou poněkud podobně jako `set` přistupujícího objektu vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-629">For advanced scenarios where control of the underlying storage of an event is desired, an event declaration can explicitly provide `add` and `remove` accessors, which are somewhat similar to the `set` accessor of a property.</span></span>

#### <a name="operators"></a><span data-ttu-id="fb7c7-630">Operátory</span><span class="sxs-lookup"><span data-stu-id="fb7c7-630">Operators</span></span>

<span data-ttu-id="fb7c7-631">***Operátor*** je člen, který definuje význam použití operátoru konkrétní výraz do instance třídy.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-631">An ***operator*** is a member that defines the meaning of applying a particular expression operator to instances of a class.</span></span> <span data-ttu-id="fb7c7-632">Je možné definovat tři typy operátorů: unární operátory, binární operátory a operátory převodu.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-632">Three kinds of operators can be defined: unary operators, binary operators, and conversion operators.</span></span> <span data-ttu-id="fb7c7-633">Všechny operátory musí být deklarována jako `public` a `static`.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-633">All operators must be declared as `public` and `static`.</span></span>

<span data-ttu-id="fb7c7-634">`List<T>
` Třída deklaruje dva operátory `operator==` a `operator!=`a proto poskytuje nový význam pro výrazy, které se vztahují na tyto operátory `List` instancí.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-634">The `List<T>
` class declares two operators, `operator==` and `operator!=`, and thus gives new meaning to expressions that apply those operators to `List` instances.</span></span> <span data-ttu-id="fb7c7-635">Konkrétně definovat operátory rovnosti dvou `List<T>
` instance vyjádřený jako porovnání všech obsažených objektů pomocí jejich `Equals` metody.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-635">Specifically, the operators define equality of two `List<T>
` instances as comparing each of the contained objects using their `Equals` methods.</span></span> <span data-ttu-id="fb7c7-636">V následujícím příkladu `==` operátor pro porovnání dvou `List<int>
` instancí.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-636">The following example uses the `==` operator to compare two `List<int>
` instances.</span></span>

```csharp
using System;

class Test
{
    static void Main() {
        List<int> a = new List<int>();
        a.Add(1);
        a.Add(2);
        List<int> b = new List<int>();
        b.Add(1);
        b.Add(2);
        Console.WriteLine(a == b);        // Outputs "True"
        b.Add(3);
        Console.WriteLine(a == b);        // Outputs "False"
    }
}
```

<span data-ttu-id="fb7c7-637">První `Console.WriteLine` výstupy `True` vzhledem k tomu, že oba seznamy obsahují stejný počet objektů se stejnými hodnotami ve stejném pořadí.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-637">The first `Console.WriteLine` outputs `True` because the two lists contain the same number of objects with the same values in the same order.</span></span> <span data-ttu-id="fb7c7-638">Měl `List<T>
` není definována `operator==`, první `Console.WriteLine` by mít výstup `False` protože `a` a `b` odkaz na jiný `List<int>
` instancí.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-638">Had `List<T>
` not defined `operator==`, the first `Console.WriteLine` would have output `False` because `a` and `b` reference different `List<int>
` instances.</span></span>

#### <a name="destructors"></a><span data-ttu-id="fb7c7-639">Destruktory</span><span class="sxs-lookup"><span data-stu-id="fb7c7-639">Destructors</span></span>

<span data-ttu-id="fb7c7-640">A ***destruktor*** je člen, který implementuje akce potřebné k destrukci instance třídy.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-640">A ***destructor*** is a member that implements the actions required to destruct an instance of a class.</span></span> <span data-ttu-id="fb7c7-641">Destruktory, nemohou mít parametry nemohou mít modifikátory a nemůže být volány explicitně.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-641">Destructors cannot have parameters, they cannot have accessibility modifiers, and they cannot be invoked explicitly.</span></span> <span data-ttu-id="fb7c7-642">Destruktor pro instanci je automaticky vyvolána při uvolnění.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-642">The destructor for an instance is invoked automatically during garbage collection.</span></span>

<span data-ttu-id="fb7c7-643">Uvolňování paměti je povolená široké šířky při rozhodování, kdy se mají shromáždit objekty a spusťte destruktory.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-643">The garbage collector is allowed wide latitude in deciding when to collect objects and run destructors.</span></span> <span data-ttu-id="fb7c7-644">Konkrétně není deterministický. časování volání destruktoru a destruktory mohou být provedeny v libovolném vlákně.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-644">Specifically, the timing of destructor invocations is not deterministic, and destructors may be executed on any thread.</span></span> <span data-ttu-id="fb7c7-645">Pro tyto a další důvody třídy by měly implementovat destruktory pouze v případě, že žádná jiná řešení jsou vhodná.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-645">For these and other reasons, classes should implement destructors only when no other solutions are feasible.</span></span>

<span data-ttu-id="fb7c7-646">`using` Příkaz poskytuje lepší přístup ke zničení objektu.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-646">The `using` statement provides a better approach to object destruction.</span></span>

## <a name="structs"></a><span data-ttu-id="fb7c7-647">Struktury</span><span class="sxs-lookup"><span data-stu-id="fb7c7-647">Structs</span></span>

<span data-ttu-id="fb7c7-648">Třídy, jako jsou ***struktury*** datové struktury, které mohou obsahovat datové členy a funkční členy jsou, ale na rozdíl od tříd, struktur jsou typy hodnot a nevyžadují přidělení haldy.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-648">Like classes, ***structs*** are data structures that can contain data members and function members, but unlike classes, structs are value types and do not require heap allocation.</span></span> <span data-ttu-id="fb7c7-649">Proměnné typu Struktura přímo ukládá datové struktury, že proměnné typu třída uchovává odkaz na do dynamicky alokovaného objektu.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-649">A variable of a struct type directly stores the data of the struct, whereas a variable of a class type stores a reference to a dynamically allocated object.</span></span> <span data-ttu-id="fb7c7-650">Typy struktury nepodporují dědičnosti zadané uživatelem, a všechny typy struktury implicitně dědí z typu `object`.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-650">Struct types do not support user-specified inheritance, and all struct types implicitly inherit from type `object`.</span></span>

<span data-ttu-id="fb7c7-651">Struktury jsou zvláště užitečná pro malé datové struktury, které mají hodnotu sémantiku.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-651">Structs are particularly useful for small data structures that have value semantics.</span></span> <span data-ttu-id="fb7c7-652">Komplexní čísla, body v souřadnicovém systému nebo páry klíč hodnota do slovníku jsou všechny dobrým příkladem struktury.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-652">Complex numbers, points in a coordinate system, or key-value pairs in a dictionary are all good examples of structs.</span></span> <span data-ttu-id="fb7c7-653">Použití struktur, místo třídy pro malé datové struktury mohou přinést důležité velký počet přidělení paměti aplikace provádí.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-653">The use of structs rather than classes for small data structures can make a large difference in the number of memory allocations an application performs.</span></span> <span data-ttu-id="fb7c7-654">Například následující program vytvoří a inicializuje pole 100 bodů.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-654">For example, the following program creates and initializes an array of 100 points.</span></span> <span data-ttu-id="fb7c7-655">S `Point` implementován jako třída, 101 samostatné objekty jsou vytvořena instance – jednu pro pole a jeden pro 100 elementů.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-655">With `Point` implemented as a class, 101 separate objects are instantiated—one for the array and one each for the 100 elements.</span></span>

```csharp
class Point
{
    public int x, y;

    public Point(int x, int y) {
        this.x = x;
        this.y = y;
    }
}

class Test
{
    static void Main() {
        Point[] points = new Point[100];
        for (int i = 0; i < 100; i++) points[i] = new Point(i, i);
    }
}
```
<span data-ttu-id="fb7c7-656">Alternativou je, aby `Point` struktury.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-656">An alternative is to make `Point` a struct.</span></span>

```csharp
struct Point
{
    public int x, y;

    public Point(int x, int y) {
        this.x = x;
        this.y = y;
    }
}
```
<span data-ttu-id="fb7c7-657">Nyní pouze jeden objekt je vytvořena instance – jednu pro pole – a `Point` instance jsou uložené v řádku v poli.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-657">Now, only one object is instantiated—the one for the array—and the `Point` instances are stored in-line in the array.</span></span>

<span data-ttu-id="fb7c7-658">Konstruktory struktury jsou vyvolány pomocí `new` operátoru, ale to neznamená, že paměť přidělena.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-658">Struct constructors are invoked with the `new` operator, but that does not imply that memory is being allocated.</span></span> <span data-ttu-id="fb7c7-659">Namísto dynamického přidělování objektu a vrací se odkaz na to jednoduše vrací hodnotu – struktura (obvykle do dočasného umístění v zásobníku), konstruktor struktury a tato hodnota se pak zkopíruje podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-659">Instead of dynamically allocating an object and returning a reference to it, a struct constructor simply returns the struct value itself (typically in a temporary location on the stack), and this value is then copied as necessary.</span></span>

<span data-ttu-id="fb7c7-660">Pomocí třídy je možné pro dvě proměnné odkazovat na stejný objekt a proto je možné pro operace v rámci jedné proměnné ovlivňovat objekt odkazovaný jinou proměnnou.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-660">With classes, it is possible for two variables to reference the same object and thus possible for operations on one variable to affect the object referenced by the other variable.</span></span> <span data-ttu-id="fb7c7-661">Struktury proměnné každý mají své vlastní kopii dat, a není možné pro operace se na nich se má vliv na jinou.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-661">With structs, the variables each have their own copy of the data, and it is not possible for operations on one to affect the other.</span></span> <span data-ttu-id="fb7c7-662">Například výstup vytvořený podle následující fragment kódu závisí na tom, zda `Point` třída nebo struktura.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-662">For example, the output produced by the following code fragment depends on whether `Point` is a class or a struct.</span></span>

```csharp
Point a = new Point(10, 10);
Point b = a;
a.x = 20;
Console.WriteLine(b.x);
```
<span data-ttu-id="fb7c7-663">Pokud `Point` je třída, výstup je `20` protože `a` a `b` odkazovat na stejný objekt.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-663">If `Point` is a class, the output is `20` because `a` and `b` reference the same object.</span></span> <span data-ttu-id="fb7c7-664">Pokud `Point` je struktura, výstup je `10` protože přiřazení `a` k `b` vytvoří kopii hodnoty, a tuto kopii není ovlivněn následné přiřazení `a.x`.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-664">If `Point` is a struct, the output is `10` because the assignment of `a` to `b` creates a copy of the value, and this copy is unaffected by the subsequent assignment to `a.x`.</span></span>

<span data-ttu-id="fb7c7-665">Předchozí příklad ukazuje dvě omezení struktury.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-665">The previous example highlights two of the limitations of structs.</span></span> <span data-ttu-id="fb7c7-666">Kopírování celé struktury je poprvé, obvykle méně efektivní než odkaz na objekt, kopírování, předávání přiřazení a hodnota parametru může být dražší s struktury než s typy odkazů.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-666">First, copying an entire struct is typically less efficient than copying an object reference, so assignment and value parameter passing can be more expensive with structs than with reference types.</span></span> <span data-ttu-id="fb7c7-667">Druhý, s výjimkou `ref` a `out` parametry, není možné vytvořit odkazy na struktury, který vylučuje jejich použití v mnoha situacích.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-667">Second, except for `ref` and `out` parameters, it is not possible to create references to structs, which rules out their usage in a number of situations.</span></span>

## <a name="arrays"></a><span data-ttu-id="fb7c7-668">Pole</span><span class="sxs-lookup"><span data-stu-id="fb7c7-668">Arrays</span></span>

<span data-ttu-id="fb7c7-669">***Pole*** je datová struktura, která obsahuje několik proměnných, které jsou přístupné prostřednictvím vypočítané indexy.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-669">An ***array*** is a data structure that contains a number of variables that are accessed through computed indices.</span></span> <span data-ttu-id="fb7c7-670">Proměnné, které jsou obsaženy v poli, tzv. ***prvky*** pole, jsou všechny stejného typu a tento typ se nazývá ***typ elementu*** pole.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-670">The variables contained in an array, also called the ***elements*** of the array, are all of the same type, and this type is called the ***element type*** of the array.</span></span>

<span data-ttu-id="fb7c7-671">Typy pole jsou typy odkazů a deklarace proměnné pole jednoduše vyčleňuje místo odkazu na pole instance.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-671">Array types are reference types, and the declaration of an array variable simply sets aside space for a reference to an array instance.</span></span> <span data-ttu-id="fb7c7-672">Skutečné pole instance v dynamicky se vytvářejí s využitím za běhu `new` operátor.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-672">Actual array instances are created dynamically at run-time using the `new` operator.</span></span> <span data-ttu-id="fb7c7-673">`new` Operace určovala ***délka*** nové instance pole, které se potom jsme opravili po dobu životnosti instance.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-673">The `new` operation specifies the ***length*** of the new array instance, which is then fixed for the lifetime of the instance.</span></span> <span data-ttu-id="fb7c7-674">Indexy prvků v rozsahu pole z `0` k `Length - 1`.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-674">The indices of the elements of an array range from `0` to `Length - 1`.</span></span> <span data-ttu-id="fb7c7-675">`new` Operátor automaticky inicializuje prvků pole na výchozí hodnoty, které je třeba nulu pro všechny číselné typy a `null` pro všechny typy odkazů.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-675">The `new` operator automatically initializes the elements of an array to their default value, which, for example, is zero for all numeric types and `null` for all reference types.</span></span>

<span data-ttu-id="fb7c7-676">Následující příklad vytvoří pole `int` prvky, inicializuje pole a vytiskne obsah pole.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-676">The following example creates an array of `int` elements, initializes the array, and prints out the contents of the array.</span></span>

```csharp
using System;

class Test
{
    static void Main() {
        int[] a = new int[10];
        for (int i = 0; i < a.Length; i++) {
            a[i] = i * i;
        }
        for (int i = 0; i < a.Length; i++) {
            Console.WriteLine("a[{0}] = {1}", i, a[i]);
        }
    }
}
```
<span data-ttu-id="fb7c7-677">Tento příklad vytvoří a pracuje ***jednorozměrné pole***.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-677">This example creates and operates on a ***single-dimensional array***.</span></span> <span data-ttu-id="fb7c7-678">C# rovněž podporuje ***vícerozměrných polí***.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-678">C# also supports ***multi-dimensional arrays***.</span></span> <span data-ttu-id="fb7c7-679">Zadejte počet rozměrů pole, označované také jako ***pořadí*** typ pole je jednu plus počet čárek mezi hranaté závorky pole zapsána.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-679">The number of dimensions of an array type, also known as the ***rank*** of the array type, is one plus the number of commas written between the square brackets of the array type.</span></span> <span data-ttu-id="fb7c7-680">V následujícím příkladu se přiděluje jednorozměrný dvourozměrném a trojrozměrného pole.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-680">The following example allocates a one-dimensional, a two-dimensional, and a three-dimensional array.</span></span>

```csharp
int[] a1 = new int[10];
int[,] a2 = new int[10, 5];
int[,,] a3 = new int[10, 5, 2];
```
<span data-ttu-id="fb7c7-681">`a1` Pole obsahuje 10 prvků `a2` pole obsahuje 50 (10 × 5) elementy a `a3` 100 (10 × 5 × 2) obsahuje pole prvků.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-681">The `a1` array contains 10 elements, the `a2` array contains 50 (10 × 5) elements, and the `a3` array contains 100 (10 × 5 × 2) elements.</span></span>

<span data-ttu-id="fb7c7-682">Typ elementu pole může být libovolného typu, včetně typu pole.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-682">The element type of an array can be any type, including an array type.</span></span> <span data-ttu-id="fb7c7-683">Někdy se označuje jako pole s prvky typu pole ***vícenásobného pole*** protože ne všechny délky pole elementu mají být stejné.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-683">An array with elements of an array type is sometimes called a ***jagged array*** because the lengths of the element arrays do not all have to be the same.</span></span> <span data-ttu-id="fb7c7-684">V následujícím příkladu se přiděluje pole polí `int`:</span><span class="sxs-lookup"><span data-stu-id="fb7c7-684">The following example allocates an array of arrays of `int`:</span></span>

```csharp
int[][] a = new int[3][];
a[0] = new int[10];
a[1] = new int[5];
a[2] = new int[20];
```
<span data-ttu-id="fb7c7-685">První řádek vytvoří pole se třemi prvky, každý typ `int[]` a každý s počáteční hodnotou `null`.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-685">The first line creates an array with three elements, each of type `int[]` and each with an initial value of `null`.</span></span> <span data-ttu-id="fb7c7-686">Následující řádky následně inicializujete tři prvky s odkazy na jednotlivá pole instancí z různých délek.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-686">The subsequent lines then initialize the three elements with references to individual array instances of varying lengths.</span></span>

<span data-ttu-id="fb7c7-687">`new` Operátor umožňuje počáteční hodnoty prvků pole zadat pomocí ***inicializátor pole***, což je seznamem výrazů napsané mezi oddělovači `{` a `}`.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-687">The `new` operator permits the initial values of the array elements to be specified using an ***array initializer***, which is a list of expressions written between the delimiters `{` and `}`.</span></span> <span data-ttu-id="fb7c7-688">V následujícím příkladu se přiděluje a inicializuje `int[]` s tři elementy.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-688">The following example allocates and initializes an `int[]` with three elements.</span></span>

```csharp
int[] a = new int[] {1, 2, 3};
```
<span data-ttu-id="fb7c7-689">Všimněte si, že délka pole je odvozen z počet výrazů mezi `{` a `}`.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-689">Note that the length of the array is inferred from the number of expressions between `{` and `}`.</span></span> <span data-ttu-id="fb7c7-690">Lokální proměnná a pole deklarace lze zkrátit další tak, aby typ pole nemusí být revidovat.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-690">Local variable and field declarations can be shortened further such that the array type does not have to be restated.</span></span>

```csharp
int[] a = {1, 2, 3};
```
<span data-ttu-id="fb7c7-691">Obě předchozí příklady jsou ekvivalentní následujícímu zápisu:</span><span class="sxs-lookup"><span data-stu-id="fb7c7-691">Both of the previous examples are equivalent to the following:</span></span>

```csharp
int[] t = new int[3];
t[0] = 1;
t[1] = 2;
t[2] = 3;
int[] a = t;
```
## <a name="interfaces"></a><span data-ttu-id="fb7c7-692">Rozhraní</span><span class="sxs-lookup"><span data-stu-id="fb7c7-692">Interfaces</span></span>

<span data-ttu-id="fb7c7-693">***Rozhraní*** definuje kontrakt, který může být implementována třídy a struktury.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-693">An ***interface*** defines a contract that can be implemented by classes and structs.</span></span> <span data-ttu-id="fb7c7-694">Rozhraní může obsahovat metody, vlastnosti, události a indexery.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-694">An interface can contain methods, properties, events, and indexers.</span></span> <span data-ttu-id="fb7c7-695">Rozhraní neposkytuje implementace členů definuje – pouze Určuje členy, které je třeba dodat ze třídy nebo struktury, které implementují rozhraní.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-695">An interface does not provide implementations of the members it defines—it merely specifies the members that must be supplied by classes or structs that implement the interface.</span></span>

<span data-ttu-id="fb7c7-696">Rozhraní může využívat ***vícenásobná dědičnost***.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-696">Interfaces may employ ***multiple inheritance***.</span></span> <span data-ttu-id="fb7c7-697">V následujícím příkladu, rozhraní `IComboBox` zdědí vlastnosti z obou `ITextBox` a `IListBox`.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-697">In the following example, the interface `IComboBox` inherits from both `ITextBox` and `IListBox`.</span></span>

```csharp
interface IControl
{
    void Paint();
}

interface ITextBox: IControl
{
    void SetText(string text);
}

interface IListBox: IControl
{
    void SetItems(string[] items);
}

interface IComboBox: ITextBox, IListBox {}
```
<span data-ttu-id="fb7c7-698">Třídy a struktury mohou implementovat více rozhraní.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-698">Classes and structs can implement multiple interfaces.</span></span> <span data-ttu-id="fb7c7-699">V následujícím příkladu třída `EditBox` implementuje oba `IControl` a `IDataBound`.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-699">In the following example, the class `EditBox` implements both `IControl` and `IDataBound`.</span></span>

```csharp
interface IDataBound
{
    void Bind(Binder b);
}

public class EditBox: IControl, IDataBound
{
    public void Paint() {...}
    public void Bind(Binder b) {...}
}
```
<span data-ttu-id="fb7c7-700">Pokud konkrétní rozhraní implementuje, třídy nebo struktury, instance této třídy nebo struktury lze implicitně převést na tento typ rozhraní.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-700">When a class or struct implements a particular interface, instances of that class or struct can be implicitly converted to that interface type.</span></span> <span data-ttu-id="fb7c7-701">Příklad</span><span class="sxs-lookup"><span data-stu-id="fb7c7-701">For example</span></span>

```csharp
EditBox editBox = new EditBox();
IControl control = editBox;
IDataBound dataBound = editBox;
```
<span data-ttu-id="fb7c7-702">V případech, kde není instance znám staticky implementovat určité rozhraní je možné dynamického přetypování.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-702">In cases where an instance is not statically known to implement a particular interface, dynamic type casts can be used.</span></span> <span data-ttu-id="fb7c7-703">Například následující příkazy použijte k získání objektu dynamického přetypování `IControl` a `IDataBound` implementace rozhraní.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-703">For example, the following statements use dynamic type casts to obtain an object's `IControl` and `IDataBound` interface implementations.</span></span> <span data-ttu-id="fb7c7-704">Protože je skutečný typ objektu `EditBox`, úspěšné položky CAST.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-704">Because the actual type of the object is `EditBox`, the casts succeed.</span></span>

```csharp
object obj = new EditBox();
IControl control = (IControl)obj;
IDataBound dataBound = (IDataBound)obj;
```
<span data-ttu-id="fb7c7-705">V předchozím `EditBox` třídy, `Paint` metodu z `IControl` rozhraní a `Bind` metodu z `IDataBound` rozhraní jsou implementovány pomocí `public` členy.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-705">In the previous `EditBox` class, the `Paint` method from the `IControl` interface and the `Bind` method from the `IDataBound` interface are implemented using `public` members.</span></span> <span data-ttu-id="fb7c7-706">C# rovněž podporuje ***implementace explicitního rozhraní členských***, který horizontálních oddílů pomocí třídy nebo struktury může předejděte tomu, aby členové `public`.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-706">C# also supports ***explicit interface member implementations***, using which the class or struct can avoid making the members `public`.</span></span> <span data-ttu-id="fb7c7-707">Implementace explicitního rozhraní člen je zapsáno s použitím rozhraní plně kvalifikovaný název člena.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-707">An explicit interface member implementation is written using the fully qualified interface member name.</span></span> <span data-ttu-id="fb7c7-708">Například `EditBox` třída může implementovat `IControl.Paint` a `IDataBound.Bind` metod pomocí explicitní implementace členů rozhraní následujícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-708">For example, the `EditBox` class could implement the `IControl.Paint` and `IDataBound.Bind` methods using explicit interface member implementations as follows.</span></span>

```csharp
public class EditBox: IControl, IDataBound
{
    void IControl.Paint() {...}
    void IDataBound.Bind(Binder b) {...}
}
```
<span data-ttu-id="fb7c7-709">Explicitní rozhraní členy jsou přístupné pouze prostřednictvím typu rozhraní.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-709">Explicit interface members can only be accessed via the interface type.</span></span> <span data-ttu-id="fb7c7-710">Například provádění `IControl.Paint` poskytované předchozí `EditBox` třídy lze vyvolat pouze první převedením `EditBox` odkaz `IControl` typu rozhraní.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-710">For example, the implementation of `IControl.Paint` provided by the previous `EditBox` class can only be invoked by first converting the `EditBox` reference to the `IControl` interface type.</span></span>

```csharp
EditBox editBox = new EditBox();
editBox.Paint();                        // Error, no such method
IControl control = editBox;
control.Paint();                        // Ok
```

## <a name="enums"></a><span data-ttu-id="fb7c7-711">Výčty</span><span class="sxs-lookup"><span data-stu-id="fb7c7-711">Enums</span></span>

<span data-ttu-id="fb7c7-712">***Typ výčtu*** je typ odlišné hodnoty se sadou pojmenovaných konstant.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-712">An ***enum type*** is a distinct value type with a set of named constants.</span></span> <span data-ttu-id="fb7c7-713">Následující příklad deklaruje a používá typ výčtu s názvem `Color` pomocí tří hodnot konstanty `Red`, `Green`, a `Blue`.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-713">The following example declares and uses an enum type named `Color` with three constant values, `Red`, `Green`, and `Blue`.</span></span>

```csharp
using System;

enum Color
{
    Red,
    Green,
    Blue
}

class Test
{
    static void PrintColor(Color color) {
        switch (color) {
            case Color.Red:
                Console.WriteLine("Red");
                break;
            case Color.Green:
                Console.WriteLine("Green");
                break;
            case Color.Blue:
                Console.WriteLine("Blue");
                break;
            default:
                Console.WriteLine("Unknown color");
                break;
        }
    }

    static void Main() {
        Color c = Color.Red;
        PrintColor(c);
        PrintColor(Color.Blue);
    }
}
```
<span data-ttu-id="fb7c7-714">Každý typ enum má odpovídající integrální typ s názvem ***nadřízený typ*** typu výčtu.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-714">Each enum type has a corresponding integral type called the ***underlying type*** of the enum type.</span></span> <span data-ttu-id="fb7c7-715">Typ výčtu, která nedeklaruje explicitně nadřízený typ má základní typ `int`.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-715">An enum type that does not explicitly declare an underlying type has an underlying type of `int`.</span></span> <span data-ttu-id="fb7c7-716">Formát úložiště a rozsah možných hodnot typu výčtu jsou určeny jeho nadřízeného typu.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-716">An enum type's storage format and range of possible values are determined by its underlying type.</span></span> <span data-ttu-id="fb7c7-717">Sadu hodnot, které může přijmout typ výčtu není omezena jeho členy výčtu.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-717">The set of values that an enum type can take on is not limited by its enum members.</span></span> <span data-ttu-id="fb7c7-718">Konkrétně se libovolná hodnota základního typu výčtu lze převést na typ výčtu a odlišné platná hodnota daného typu výčtu.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-718">In particular, any value of the underlying type of an enum can be cast to the enum type and is a distinct valid value of that enum type.</span></span>

<span data-ttu-id="fb7c7-719">Následující příklad deklaruje typ výčtu s názvem `Alignment` pomocí základního typu `sbyte`.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-719">The following example declares an enum type named `Alignment` with an underlying type of `sbyte`.</span></span>

```csharp
enum Alignment: sbyte
{
    Left = -1,
    Center = 0,
    Right = 1
}
```
<span data-ttu-id="fb7c7-720">Jak je znázorněno v předchozím příkladu, může obsahovat deklaraci člena výčtu konstantní výraz, který určuje hodnotu člena.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-720">As shown by the previous example, an enum member declaration can include a constant expression that specifies the value of the member.</span></span> <span data-ttu-id="fb7c7-721">Konstantní hodnoty pro každého člena výčtu musí být v rozsahu od základního typu výčtu.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-721">The constant value for each enum member must be in the range of the underlying type of the enum.</span></span> <span data-ttu-id="fb7c7-722">Při deklaraci člena výčtu explicitně neurčí hodnotu, se klíči přiřadí člen hodnotu nula (Pokud je první člen v typu výčtu) nebo hodnotu textový předchozí člen výčtu plus jedna.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-722">When an enum member declaration does not explicitly specify a value, the member is given the value zero (if it is the first member in the enum type) or the value of the textually preceding enum member plus one.</span></span>

<span data-ttu-id="fb7c7-723">Hodnoty výčtu lze převést na integrální hodnoty a naopak pomocí přetypování.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-723">Enum values can be converted to integral values and vice versa using type casts.</span></span> <span data-ttu-id="fb7c7-724">Příklad</span><span class="sxs-lookup"><span data-stu-id="fb7c7-724">For example</span></span>

```csharp
int i = (int)Color.Blue;        // int i = 2;
Color c = (Color)2;             // Color c = Color.Blue;
```
<span data-ttu-id="fb7c7-725">Výchozí hodnota typu výčtu je integrální hodnotu nula, převést na typ výčtu.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-725">The default value of any enum type is the integral value zero converted to the enum type.</span></span> <span data-ttu-id="fb7c7-726">V případech, kdy proměnné jsou automaticky inicializovány na výchozí hodnotu jedná se o hodnotu k proměnné typů výčtu.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-726">In cases where variables are automatically initialized to a default value, this is the value given to variables of enum types.</span></span> <span data-ttu-id="fb7c7-727">Aby výchozí hodnota typu výčtu být snadno k dispozici, literál `0` implicitně převede na libovolný typ výčtu.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-727">In order for the default value of an enum type to be easily available, the literal `0` implicitly converts to any enum type.</span></span> <span data-ttu-id="fb7c7-728">Proto následující je povolený.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-728">Thus, the following is permitted.</span></span>

```csharp
Color c = 0;
```

## <a name="delegates"></a><span data-ttu-id="fb7c7-729">Delegáty</span><span class="sxs-lookup"><span data-stu-id="fb7c7-729">Delegates</span></span>

<span data-ttu-id="fb7c7-730">A ***typ delegáta*** seznamu představuje odkazy na metody pomocí konkrétních parametrů a návratový typ.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-730">A ***delegate type*** represents references to methods with a particular parameter list and return type.</span></span> <span data-ttu-id="fb7c7-731">Delegáty umožňují považovat za entity, které může být přiřazena k proměnné a předány jako parametry metod.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-731">Delegates make it possible to treat methods as entities that can be assigned to variables and passed as parameters.</span></span> <span data-ttu-id="fb7c7-732">Delegáti jsou podobný koncept ukazatelů na funkce v některých jiných jazycích, ale na rozdíl od ukazatelů na funkce, Delegáti jsou objektově orientované a typově bezpečné.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-732">Delegates are similar to the concept of function pointers found in some other languages, but unlike function pointers, delegates are object-oriented and type-safe.</span></span>

<span data-ttu-id="fb7c7-733">Následující příklad deklaruje a používá typ delegáta s názvem `Function`.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-733">The following example declares and uses a delegate type named `Function`.</span></span>

```csharp
using System;

delegate double Function(double x);

class Multiplier
{
    double factor;

    public Multiplier(double factor) {
        this.factor = factor;
    }

    public double Multiply(double x) {
        return x * factor;
    }
}

class Test
{
    static double Square(double x) {
        return x * x;
    }

    static double[] Apply(double[] a, Function f) {
        double[] result = new double[a.Length];
        for (int i = 0; i < a.Length; i++) result[i] = f(a[i]);
        return result;
    }

    static void Main() {
        double[] a = {0.0, 0.5, 1.0};
        double[] squares = Apply(a, Square);
        double[] sines = Apply(a, Math.Sin);
        Multiplier m = new Multiplier(2.0);
        double[] doubles =  Apply(a, m.Multiply);
    }
}
```
<span data-ttu-id="fb7c7-734">Instance `Function` typ delegáta může odkazovat na jakoukoli metodu, která přijímá `double` argument a vrátí `double` hodnotu.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-734">An instance of the `Function` delegate type can reference any method that takes a `double` argument and returns a `double` value.</span></span> <span data-ttu-id="fb7c7-735">`Apply` Postup se vztahuje dané `Function` na elementy `double[]`, vrácení `double[]` s výsledky.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-735">The `Apply` method applies a given `Function` to the elements of a `double[]`, returning a `double[]` with the results.</span></span> <span data-ttu-id="fb7c7-736">V `Main` metody `Apply` slouží k použití tří různých funkcí `double[]`.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-736">In the `Main` method, `Apply` is used to apply three different functions to a `double[]`.</span></span>

<span data-ttu-id="fb7c7-737">Delegát může odkazovat na statickou metodu (například `Square` nebo `Math.Sin` v předchozím příkladu) nebo metodu instance (například `m.Multiply` v předchozím příkladu).</span><span class="sxs-lookup"><span data-stu-id="fb7c7-737">A delegate can reference either a static method (such as `Square` or `Math.Sin` in the previous example) or an instance method (such as `m.Multiply` in the previous example).</span></span> <span data-ttu-id="fb7c7-738">Delegát, který odkazuje na metodu instance také odkazuje na konkrétní objekt, a když uživatel vyvolá metodu instance prostřednictvím delegáta, tento objekt se stane `this` ve volání.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-738">A delegate that references an instance method also references a particular object, and when the instance method is invoked through the delegate, that object becomes `this` in the invocation.</span></span>

<span data-ttu-id="fb7c7-739">Delegáty lze vytvořit také pomocí anonymní funkce, které jsou "inline metod", které jsou vytvořeny v reálném čase.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-739">Delegates can also be created using anonymous functions, which are "inline methods" that are created on the fly.</span></span> <span data-ttu-id="fb7c7-740">Anonymní funkce můžete zobrazit místní proměnné okolního metody.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-740">Anonymous functions can see the local variables of the surrounding methods.</span></span> <span data-ttu-id="fb7c7-741">Proto je výše uvedený příklad multiplikátor zapisovat snadněji bez použití `Multiplier` třídy:</span><span class="sxs-lookup"><span data-stu-id="fb7c7-741">Thus, the multiplier example above can be written more easily without using a `Multiplier` class:</span></span>

```csharp
double[] doubles =  Apply(a, (double x) => x * 2.0);
```
<span data-ttu-id="fb7c7-742">Zajímavé a užitečné vlastnictví delegáta je, že ho neznáte nebo postaral o třídě, na které odkazuje; metody vše, co je důležité je, že odkazované metody má stejné parametry a návratový typ jako delegát.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-742">An interesting and useful property of a delegate is that it does not know or care about the class of the method it references; all that matters is that the referenced method has the same parameters and return type as the delegate.</span></span>

## <a name="attributes"></a><span data-ttu-id="fb7c7-743">Atributy</span><span class="sxs-lookup"><span data-stu-id="fb7c7-743">Attributes</span></span>

<span data-ttu-id="fb7c7-744">Typy, členy a dalších entit v programu v jazyce C# podporují modifikátory, které ovládat některé aspekty jejich chování.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-744">Types, members, and other entities in a C# program support modifiers that control certain aspects of their behavior.</span></span> <span data-ttu-id="fb7c7-745">Například pro usnadnění metody je řízen pomocí `public`, `protected`, `internal`, a `private` modifikátory.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-745">For example, the accessibility of a method is controlled using the `public`, `protected`, `internal`, and `private` modifiers.</span></span> <span data-ttu-id="fb7c7-746">C# umožňuje zobecnit tuto funkci tak, uživatelem definované typy deklarativní informací můžete připojit k programu entity a načíst v době běhu.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-746">C# generalizes this capability such that user-defined types of declarative information can be attached to program entities and retrieved at run-time.</span></span> <span data-ttu-id="fb7c7-747">Programy zadejte tyto dodatečné informace deklarativní definice a používání ***atributy***.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-747">Programs specify this additional declarative information by defining and using ***attributes***.</span></span>

<span data-ttu-id="fb7c7-748">Následující příklad deklaruje `HelpAttribute` atribut, který může být umístěn v programu entity, které obsahují odkazy na jejich související dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-748">The following example declares a `HelpAttribute` attribute that can be placed on program entities to provide links to their associated documentation.</span></span>

```csharp
using System;

public class HelpAttribute: Attribute
{
    string url;
    string topic;

    public HelpAttribute(string url) {
        this.url = url;
    }

    public string Url {
        get { return url; }
    }

    public string Topic {
        get { return topic; }
        set { topic = value; }
    }
}
```
<span data-ttu-id="fb7c7-749">Všechny třídy atributu odvozovat `System.Attribute` základní třídy, které poskytuje rozhraní .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-749">All attribute classes derive from the `System.Attribute` base class provided by the .NET Framework.</span></span> <span data-ttu-id="fb7c7-750">Atributy lze použít zadáním jeho názvu, spolu s žádné argumenty, v hranatých závorkách těsně před deklaraci přidružené.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-750">Attributes can be applied by giving their name, along with any arguments, inside square brackets just before the associated declaration.</span></span> <span data-ttu-id="fb7c7-751">Pokud název atributu končí na `Attribute`, část názvu lze vynechat, pokud se odkazuje atribut.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-751">If an attribute's name ends in `Attribute`, that part of the name can be omitted when the attribute is referenced.</span></span> <span data-ttu-id="fb7c7-752">Například `HelpAttribute` atribut je možné následujícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-752">For example, the `HelpAttribute` attribute can be used as follows.</span></span>

```csharp
[Help("http://msdn.microsoft.com/.../MyClass.htm")]
public class Widget
{
    [Help("http://msdn.microsoft.com/.../MyClass.htm", Topic = "Display")]
    public void Display(string text) {}
}
```
<span data-ttu-id="fb7c7-753">Tento příklad připojí `HelpAttribute` k `Widget` třídy a další `HelpAttribute` k `Display` metody ve třídě.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-753">This example attaches a `HelpAttribute` to the `Widget` class and another `HelpAttribute` to the `Display` method in the class.</span></span> <span data-ttu-id="fb7c7-754">Veřejné konstruktory třídu atributu řídit informace, které musí být zadaná, když je připojena k entitě programu.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-754">The public constructors of an attribute class control the information that must be provided when the attribute is attached to a program entity.</span></span> <span data-ttu-id="fb7c7-755">Další informace lze zadat pomocí odkazu na veřejné čtení a zápis vlastnosti třídy atributů (jako je například odkaz na `Topic` vlastnost dříve).</span><span class="sxs-lookup"><span data-stu-id="fb7c7-755">Additional information can be provided by referencing public read-write properties of the attribute class (such as the reference to the `Topic` property previously).</span></span>

<span data-ttu-id="fb7c7-756">Následující příklad ukazuje, jak můžete načíst informace o atributu pro daný program entitu v době běhu pomocí operace reflection.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-756">The following example shows how attribute information for a given program entity can be retrieved at run-time using reflection.</span></span>

```csharp
using System;
using System.Reflection;

class Test
{
    static void ShowHelp(MemberInfo member) {
        HelpAttribute a = Attribute.GetCustomAttribute(member,
            typeof(HelpAttribute)) as HelpAttribute;
        if (a == null) {
            Console.WriteLine("No help for {0}", member);
        }
        else {
            Console.WriteLine("Help for {0}:", member);
            Console.WriteLine("  Url={0}, Topic={1}", a.Url, a.Topic);
        }
    }

    static void Main() {
        ShowHelp(typeof(Widget));
        ShowHelp(typeof(Widget).GetMethod("Display"));
    }
}
```
<span data-ttu-id="fb7c7-757">Konkrétní atribut je žádost prostřednictvím reflexe, konstruktor pro třídu atributu je vyvolán pomocí informací uvedených ve zdrojovém programu a vrátí se výsledná instance atributu.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-757">When a particular attribute is requested through reflection, the constructor for the attribute class is invoked with the information provided in the program source, and the resulting attribute instance is returned.</span></span> <span data-ttu-id="fb7c7-758">Pokud Další informace se poskytovaly prostřednictvím vlastností, tyto vlastnosti jsou nastavené na dané hodnoty před vrácením instance atributu.</span><span class="sxs-lookup"><span data-stu-id="fb7c7-758">If additional information was provided through properties, those properties are set to the given values before the attribute instance is returned.</span></span>
