---
ms.openlocfilehash: 8bc4bf6310fb8a8457beee167f18d30aaca10a8e
ms.sourcegitcommit: 7f7fc6e9e195e51b7ff8229aeaa70aa9fbbb63cb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/10/2019
ms.locfileid: "70876891"
---
# <a name="introduction"></a><span data-ttu-id="0e09e-101">Úvod</span><span class="sxs-lookup"><span data-stu-id="0e09e-101">Introduction</span></span>

<span data-ttu-id="0e09e-102">C#(vyslovit "viz Sharp") je jednoduchý, moderní, objektově orientovaný programovací jazyk, který je typově bezpečný.</span><span class="sxs-lookup"><span data-stu-id="0e09e-102">C# (pronounced "See Sharp") is a simple, modern, object-oriented, and type-safe programming language.</span></span> <span data-ttu-id="0e09e-103">C#má své kořeny v řadě jazyků C a bude se okamžitě seznámit s programátory v jazyce C++c, a Java.</span><span class="sxs-lookup"><span data-stu-id="0e09e-103">C# has its roots in the C family of languages and will be immediately familiar to C, C++, and Java programmers.</span></span> <span data-ttu-id="0e09e-104">C#je standardizovaná ECMA mezinárodními jako standard ***ECMA-334*** a normou ISO/IEC jako standard ***iso/IEC 23270*** .</span><span class="sxs-lookup"><span data-stu-id="0e09e-104">C# is standardized by ECMA International as the ***ECMA-334*** standard and by ISO/IEC as the ***ISO/IEC 23270*** standard.</span></span> <span data-ttu-id="0e09e-105">C# Kompilátor od microsoftu pro .NET Framework je vyhovující implementaci obou těchto standardů.</span><span class="sxs-lookup"><span data-stu-id="0e09e-105">Microsoft's C# compiler for the .NET Framework is a conforming implementation of both of these standards.</span></span>

<span data-ttu-id="0e09e-106">C#je objektově orientovaný jazyk, ale C# dále zahrnuje podporu pro programování ***orientované na komponenty*** .</span><span class="sxs-lookup"><span data-stu-id="0e09e-106">C# is an object-oriented language, but C# further includes support for ***component-oriented*** programming.</span></span> <span data-ttu-id="0e09e-107">Moderní návrh softwaru se stále spoléhá na softwarové komponenty ve formě integrovaných a samoobslužných balíčků funkcí.</span><span class="sxs-lookup"><span data-stu-id="0e09e-107">Contemporary software design increasingly relies on software components in the form of self-contained and self-describing packages of functionality.</span></span> <span data-ttu-id="0e09e-108">Klíč k takovým součástem je, že prezentují programovací model s vlastnostmi, metodami a událostmi. mají atributy, které poskytují deklarativní informace o komponentě. a obsahují vlastní dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="0e09e-108">Key to such components is that they present a programming model with properties, methods, and events; they have attributes that provide declarative information about the component; and they incorporate their own documentation.</span></span> <span data-ttu-id="0e09e-109">C#poskytuje jazykové konstrukce, které přímo podporují tyto koncepce, a C# vytváří tak velmi přirozený jazyk pro vytváření a používání softwarových komponent.</span><span class="sxs-lookup"><span data-stu-id="0e09e-109">C# provides language constructs to directly support these concepts, making C# a very natural language in which to create and use software components.</span></span>

<span data-ttu-id="0e09e-110">Několik C# funkcí pomáhá při konstrukci robustních a odolných aplikací: ***Uvolňování*** paměti automaticky uvolňuje paměť obsazené nepoužívanými objekty; ***zpracování výjimek*** poskytuje strukturovaný a rozšiřitelný přístup k detekci a obnovení chyb. a ***typově bezpečný*** návrh tohoto jazyka znemožňuje čtení z neinicializovaných proměnných, pro indexaci polí nad rámec jejich hranic nebo pro provedení nezaškrtnutých přetypování typu.</span><span class="sxs-lookup"><span data-stu-id="0e09e-110">Several C# features aid in the construction of robust and durable applications: ***Garbage collection*** automatically reclaims memory occupied by unused objects; ***exception handling*** provides a structured and extensible approach to error detection and recovery; and the ***type-safe*** design of the language makes it impossible to read from uninitialized variables, to index arrays beyond their bounds, or to perform unchecked type casts.</span></span>

<span data-ttu-id="0e09e-111">C#má ***jednotný systém typů***.</span><span class="sxs-lookup"><span data-stu-id="0e09e-111">C# has a ***unified type system***.</span></span> <span data-ttu-id="0e09e-112">Všechny C# typy, včetně primitivních typů `int` , jako jsou a `double`, dědí z jednoho `object` kořenového typu.</span><span class="sxs-lookup"><span data-stu-id="0e09e-112">All C# types, including primitive types such as `int` and `double`, inherit from a single root `object` type.</span></span> <span data-ttu-id="0e09e-113">Všechny typy tedy sdílí sadu běžných operací a hodnoty libovolného typu mohou být uloženy, přepravovány a provozovány konzistentním způsobem.</span><span class="sxs-lookup"><span data-stu-id="0e09e-113">Thus, all types share a set of common operations, and values of any type can be stored, transported, and operated upon in a consistent manner.</span></span> <span data-ttu-id="0e09e-114">Kromě toho C# podporuje uživatelsky definované typy odkazů i typy hodnot, což umožňuje dynamické přidělování objektů a také vložené úložiště lehkých struktur.</span><span class="sxs-lookup"><span data-stu-id="0e09e-114">Furthermore, C# supports both user-defined reference types and value types, allowing dynamic allocation of objects as well as in-line storage of lightweight structures.</span></span>

<span data-ttu-id="0e09e-115">Aby bylo zajištěno, že se programy a knihovny můžou v průběhu času vyvíjejí kompatibilním způsobem, je mnohem zdůrazněno, že C# se v návrhu používá ***Správa verzí*** C#.</span><span class="sxs-lookup"><span data-stu-id="0e09e-115">To ensure that C# programs and libraries can evolve over time in a compatible manner, much emphasis has been placed on ***versioning*** in C#'s design.</span></span> <span data-ttu-id="0e09e-116">Řada programovacích jazyků platíte jenom malým pozornostům tohoto problému. v důsledku toho jsou programy napsané v těchto jazycích častěji využívány, pokud jsou zavedeny novější verze závislých knihoven.</span><span class="sxs-lookup"><span data-stu-id="0e09e-116">Many programming languages pay little attention to this issue, and, as a result, programs written in those languages break more often than necessary when newer versions of dependent libraries are introduced.</span></span> <span data-ttu-id="0e09e-117">C#Aspekty návrhu, které byly přímo ovlivněny aspekty správy verzí, zahrnují samostatné `virtual` a `override` modifikátory, pravidla pro řešení přetížení metod a podporu explicitních deklarací členů rozhraní.</span><span class="sxs-lookup"><span data-stu-id="0e09e-117">Aspects of C#'s design that were directly influenced by versioning considerations include the separate `virtual` and `override` modifiers, the rules for method overload resolution, and support for explicit interface member declarations.</span></span>

<span data-ttu-id="0e09e-118">Zbytek této kapitoly popisuje základní funkce C# jazyka.</span><span class="sxs-lookup"><span data-stu-id="0e09e-118">The rest of this chapter describes the essential features of the C# language.</span></span> <span data-ttu-id="0e09e-119">I když později kapitoly popisují pravidla a výjimky v detailním a někdy matematickém způsobu, tato kapitola usiluje o přehlednost a zkrácení na úkor úplnosti.</span><span class="sxs-lookup"><span data-stu-id="0e09e-119">Although later chapters describe rules and exceptions in a detail-oriented and sometimes mathematical manner, this chapter strives for clarity and brevity at the expense of completeness.</span></span> <span data-ttu-id="0e09e-120">Záměrem je poskytnout čtenářům Úvod k jazyku, který usnadní psaní dřívějších programů a čtení dalších kapitol.</span><span class="sxs-lookup"><span data-stu-id="0e09e-120">The intent is to provide the reader with an introduction to the language that will facilitate the writing of early programs and the reading of later chapters.</span></span>

## <a name="hello-world"></a><span data-ttu-id="0e09e-121">Hello World</span><span class="sxs-lookup"><span data-stu-id="0e09e-121">Hello world</span></span>

<span data-ttu-id="0e09e-122">Program "Hello, World" se tradičně používá k zavedení programovacího jazyka.</span><span class="sxs-lookup"><span data-stu-id="0e09e-122">The "Hello, World" program is traditionally used to introduce a programming language.</span></span> <span data-ttu-id="0e09e-123">Tady je C#:</span><span class="sxs-lookup"><span data-stu-id="0e09e-123">Here it is in C#:</span></span>

```csharp
using System;

class Hello
{
    static void Main() {
        Console.WriteLine("Hello, World");
    }
}
```

<span data-ttu-id="0e09e-124">C#zdrojové soubory mají obvykle příponu `.cs`souboru.</span><span class="sxs-lookup"><span data-stu-id="0e09e-124">C# source files typically have the file extension `.cs`.</span></span> <span data-ttu-id="0e09e-125">Za předpokladu, že program "Hello, World" je uložen `hello.cs`v souboru, program může být zkompilován s kompilátorem společnosti Microsoft C# pomocí příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="0e09e-125">Assuming that the "Hello, World" program is stored in the file `hello.cs`, the program can be compiled with the Microsoft C# compiler using the command line</span></span>
```
csc hello.cs
```
<span data-ttu-id="0e09e-126">který vytváří spustitelné sestavení s názvem `hello.exe`.</span><span class="sxs-lookup"><span data-stu-id="0e09e-126">which produces an executable assembly named `hello.exe`.</span></span> <span data-ttu-id="0e09e-127">Výstup vytvořený touto aplikací při spuštění je</span><span class="sxs-lookup"><span data-stu-id="0e09e-127">The output produced by this application when it is run is</span></span>
```
Hello, World
```

<span data-ttu-id="0e09e-128">Program "Hello, World" začíná `using` direktivou, která odkazuje na `System` obor názvů.</span><span class="sxs-lookup"><span data-stu-id="0e09e-128">The "Hello, World" program starts with a `using` directive that references the `System` namespace.</span></span> <span data-ttu-id="0e09e-129">Obory názvů poskytují hierarchické prostředky pro C# uspořádání programů a knihoven.</span><span class="sxs-lookup"><span data-stu-id="0e09e-129">Namespaces provide a hierarchical means of organizing C# programs and libraries.</span></span> <span data-ttu-id="0e09e-130">Obory názvů obsahují `System` typy a jiné obory názvů, například obor názvů obsahuje počet typů, jako je `Console` například třída odkazovaná v programu, a řadu `IO` dalších oborů názvů, například a `Collections`.</span><span class="sxs-lookup"><span data-stu-id="0e09e-130">Namespaces contain types and other namespaces—for example, the `System` namespace contains a number of types, such as the `Console` class referenced in the program, and a number of other namespaces, such as `IO` and `Collections`.</span></span> <span data-ttu-id="0e09e-131">`using` Direktiva, která odkazuje na daný obor názvů, umožňuje nekvalifikované použití typů, které jsou členy tohoto oboru názvů.</span><span class="sxs-lookup"><span data-stu-id="0e09e-131">A `using` directive that references a given namespace enables unqualified use of the types that are members of that namespace.</span></span> <span data-ttu-id="0e09e-132">Z `using` důvodu direktivy může program použít `Console.WriteLine` jako zkrácený pro `System.Console.WriteLine`.</span><span class="sxs-lookup"><span data-stu-id="0e09e-132">Because of the `using` directive, the program can use `Console.WriteLine` as shorthand for `System.Console.WriteLine`.</span></span>

<span data-ttu-id="0e09e-133">Třída deklarovaná programem "Hello, World" má jednoho člena, metodu s názvem `Main`. `Hello`</span><span class="sxs-lookup"><span data-stu-id="0e09e-133">The `Hello` class declared by the "Hello, World" program has a single member, the method named `Main`.</span></span> <span data-ttu-id="0e09e-134">Metoda je deklarována `static` s modifikátorem. `Main`</span><span class="sxs-lookup"><span data-stu-id="0e09e-134">The `Main` method is declared with the `static` modifier.</span></span> <span data-ttu-id="0e09e-135">I když metody instance mohou odkazovat na konkrétní ohraničující objekt instance pomocí klíčového `this`slova, statické metody pracují bez odkazů na konkrétní objekt.</span><span class="sxs-lookup"><span data-stu-id="0e09e-135">While instance methods can reference a particular enclosing object instance using the keyword `this`, static methods operate without reference to a particular object.</span></span> <span data-ttu-id="0e09e-136">Podle konvence, statická metoda s `Main` názvem slouží jako vstupní bod programu.</span><span class="sxs-lookup"><span data-stu-id="0e09e-136">By convention, a static method named `Main` serves as the entry point of a program.</span></span>

<span data-ttu-id="0e09e-137">Výstup programu je vytvořen `WriteLine` metodou `Console` třídy v `System` oboru názvů.</span><span class="sxs-lookup"><span data-stu-id="0e09e-137">The output of the program is produced by the `WriteLine` method of the `Console` class in the `System` namespace.</span></span> <span data-ttu-id="0e09e-138">Tato třída je poskytována knihovnami tříd .NET Framework, které jsou ve výchozím nastavení automaticky odkazovány kompilátorem společnosti C# Microsoft.</span><span class="sxs-lookup"><span data-stu-id="0e09e-138">This class is provided by the .NET Framework class libraries, which, by default, are automatically referenced by the Microsoft C# compiler.</span></span> <span data-ttu-id="0e09e-139">Všimněte si C# , že sám nemá samostatnou běhovou knihovnu.</span><span class="sxs-lookup"><span data-stu-id="0e09e-139">Note that C# itself does not have a separate runtime library.</span></span> <span data-ttu-id="0e09e-140">Místo toho je .NET Framework běhová knihovna C#.</span><span class="sxs-lookup"><span data-stu-id="0e09e-140">Instead, the .NET Framework is the runtime library of C#.</span></span>

## <a name="program-structure"></a><span data-ttu-id="0e09e-141">Struktura programu</span><span class="sxs-lookup"><span data-stu-id="0e09e-141">Program structure</span></span>

<span data-ttu-id="0e09e-142">Klíčovou organizační koncepcí v C# nástroji jsou ***programy***, ***obory názvů***, ***typy***, ***členy***a ***sestavení***.</span><span class="sxs-lookup"><span data-stu-id="0e09e-142">The key organizational concepts in C# are ***programs***, ***namespaces***, ***types***, ***members***, and ***assemblies***.</span></span> <span data-ttu-id="0e09e-143">C#programy se skládají z jednoho nebo více zdrojových souborů.</span><span class="sxs-lookup"><span data-stu-id="0e09e-143">C# programs consist of one or more source files.</span></span> <span data-ttu-id="0e09e-144">Programy deklarují typy, které obsahují členy a mohou být uspořádány do oborů názvů.</span><span class="sxs-lookup"><span data-stu-id="0e09e-144">Programs declare types, which contain members and can be organized into namespaces.</span></span> <span data-ttu-id="0e09e-145">Příklady typů jsou třídy a rozhraní.</span><span class="sxs-lookup"><span data-stu-id="0e09e-145">Classes and interfaces are examples of types.</span></span> <span data-ttu-id="0e09e-146">Příklady členů jsou pole, metody, vlastnosti a události.</span><span class="sxs-lookup"><span data-stu-id="0e09e-146">Fields, methods, properties, and events are examples of members.</span></span> <span data-ttu-id="0e09e-147">Když C# jsou programy kompilovány, jsou fyzicky zabaleny do sestavení.</span><span class="sxs-lookup"><span data-stu-id="0e09e-147">When C# programs are compiled, they are physically packaged into assemblies.</span></span> <span data-ttu-id="0e09e-148">Sestavení `.exe` mají obvykle příponu souboru nebo `.dll`v závislosti na tom, zda implementují ***aplikace*** nebo ***knihovny***.</span><span class="sxs-lookup"><span data-stu-id="0e09e-148">Assemblies typically have the file extension `.exe` or `.dll`, depending on whether they implement ***applications*** or ***libraries***.</span></span>

<span data-ttu-id="0e09e-149">Příklad</span><span class="sxs-lookup"><span data-stu-id="0e09e-149">The example</span></span>

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
<span data-ttu-id="0e09e-150">deklaruje třídu s názvem `Stack` v oboru názvů s názvem. `Acme.Collections`</span><span class="sxs-lookup"><span data-stu-id="0e09e-150">declares a class named `Stack` in a namespace called `Acme.Collections`.</span></span> <span data-ttu-id="0e09e-151">Plně kvalifikovaný název této třídy je `Acme.Collections.Stack`.</span><span class="sxs-lookup"><span data-stu-id="0e09e-151">The fully qualified name of this class is `Acme.Collections.Stack`.</span></span> <span data-ttu-id="0e09e-152">Třída obsahuje několik členů: pole s `top`názvem, dvě metody pojmenované `Push` a `Pop`a vnořená třída s názvem `Entry`.</span><span class="sxs-lookup"><span data-stu-id="0e09e-152">The class contains several members: a field named `top`, two methods named `Push` and `Pop`, and a nested class named `Entry`.</span></span> <span data-ttu-id="0e09e-153">Třída dále obsahuje tři členy: pole s názvem `next`, pole s názvem `data`a konstruktor. `Entry`</span><span class="sxs-lookup"><span data-stu-id="0e09e-153">The `Entry` class further contains three members: a field named `next`, a field named `data`, and a constructor.</span></span> <span data-ttu-id="0e09e-154">Za předpokladu, že zdrojový kód příkladu je uložen v souboru `acme.cs`, příkazový řádek</span><span class="sxs-lookup"><span data-stu-id="0e09e-154">Assuming that the source code of the example is stored in the file `acme.cs`, the command line</span></span>

```
csc /t:library acme.cs
```
<span data-ttu-id="0e09e-155">zkompiluje příklad jako knihovnu (kód bez `Main` vstupního bodu) a vytvoří sestavení s názvem. `acme.dll`</span><span class="sxs-lookup"><span data-stu-id="0e09e-155">compiles the example as a library (code without a `Main` entry point) and produces an assembly named `acme.dll`.</span></span>

<span data-ttu-id="0e09e-156">Sestavení obsahují spustitelný kód ve formě instrukcí pro ***převodní jazyk*** (IL) a symbolické informace ve formě ***metadat***.</span><span class="sxs-lookup"><span data-stu-id="0e09e-156">Assemblies contain executable code in the form of ***Intermediate Language*** (IL) instructions, and symbolic information in the form of ***metadata***.</span></span> <span data-ttu-id="0e09e-157">Před provedením je kód IL v sestavení automaticky převeden na kód specifický pro procesor pomocí kompilátoru JIT (just-in-time) modulu CLR (Common Language Runtime) .NET.</span><span class="sxs-lookup"><span data-stu-id="0e09e-157">Before it is executed, the IL code in an assembly is automatically converted to processor-specific code by the Just-In-Time (JIT) compiler of .NET Common Language Runtime.</span></span>

<span data-ttu-id="0e09e-158">Vzhledem k tomu, že sestavení je samo-popisující jednotku funkcí obsahující kód i metadata, není nutné `#include` direktivy a hlavičkové soubory v. C#</span><span class="sxs-lookup"><span data-stu-id="0e09e-158">Because an assembly is a self-describing unit of functionality containing both code and metadata, there is no need for `#include` directives and header files in C#.</span></span> <span data-ttu-id="0e09e-159">Veřejné typy a členy, které jsou obsaženy v konkrétním sestavení, jsou v C# programu k dispozici jednoduše odkazem na toto sestavení při kompilování programu.</span><span class="sxs-lookup"><span data-stu-id="0e09e-159">The public types and members contained in a particular assembly are made available in a C# program simply by referencing that assembly when compiling the program.</span></span> <span data-ttu-id="0e09e-160">Například tento program používá `Acme.Collections.Stack` třídu `acme.dll` ze sestavení:</span><span class="sxs-lookup"><span data-stu-id="0e09e-160">For example, this program uses the `Acme.Collections.Stack` class from the `acme.dll` assembly:</span></span>

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
<span data-ttu-id="0e09e-161">Pokud je program `test.cs`uložen v souboru, když `test.cs` je zkompilován, `acme.dll` `/r` sestavení lze odkazovat pomocí možnosti kompilátoru:</span><span class="sxs-lookup"><span data-stu-id="0e09e-161">If the program is stored in the file `test.cs`, when `test.cs` is compiled, the `acme.dll` assembly can be referenced using the compiler's `/r` option:</span></span>

```
csc /r:acme.dll test.cs
```
<span data-ttu-id="0e09e-162">Tím se vytvoří spustitelné sestavení s `test.exe`názvem, které při spuštění vytvoří výstup:</span><span class="sxs-lookup"><span data-stu-id="0e09e-162">This creates an executable assembly named `test.exe`, which, when run, produces the output:</span></span>

```
100
10
1
```
<span data-ttu-id="0e09e-163">C#umožňuje, aby byl zdrojový text programu uložen v několika zdrojových souborech.</span><span class="sxs-lookup"><span data-stu-id="0e09e-163">C# permits the source text of a program to be stored in several source files.</span></span> <span data-ttu-id="0e09e-164">Když je zkompilován program pro C# více souborů, všechny zdrojové soubory jsou zpracovávány společně a zdrojové soubory mohou volně odkazovat, a to koncepční, protože všechny zdrojové soubory byly před zpracováním sloučeny do jednoho velkého souboru.</span><span class="sxs-lookup"><span data-stu-id="0e09e-164">When a multi-file C# program is compiled, all of the source files are processed together, and the source files can freely reference each other—conceptually, it is as if all the source files were concatenated into one large file before being processed.</span></span> <span data-ttu-id="0e09e-165">Dopředné deklarace nejsou nikdy C# potřeba v důsledku nedostatečných výjimek, pořadí deklarací je nedůležité.</span><span class="sxs-lookup"><span data-stu-id="0e09e-165">Forward declarations are never needed in C# because, with very few exceptions, declaration order is insignificant.</span></span> <span data-ttu-id="0e09e-166">C#neomezuje zdrojový soubor na deklaraci pouze jednoho veřejného typu, ani nevyžaduje název zdrojového souboru, aby odpovídal typu deklarovanému ve zdrojovém souboru.</span><span class="sxs-lookup"><span data-stu-id="0e09e-166">C# does not limit a source file to declaring only one public type nor does it require the name of the source file to match a type declared in the source file.</span></span>

## <a name="types-and-variables"></a><span data-ttu-id="0e09e-167">Typy a proměnné</span><span class="sxs-lookup"><span data-stu-id="0e09e-167">Types and variables</span></span>

<span data-ttu-id="0e09e-168">Existují dva druhy typů v C#: ***typy hodnot*** a ***typy odkazů***.</span><span class="sxs-lookup"><span data-stu-id="0e09e-168">There are two kinds of types in C#: ***value types*** and ***reference types***.</span></span> <span data-ttu-id="0e09e-169">Proměnné typů hodnot přímo obsahují svá data, zatímco proměnné typu odkazu ukládají odkazy na jejich data, přičemž ta se označují jako objekty.</span><span class="sxs-lookup"><span data-stu-id="0e09e-169">Variables of value types directly contain their data whereas variables of reference types store references to their data, the latter being known as objects.</span></span> <span data-ttu-id="0e09e-170">U typů odkazů je možné, aby dvě proměnné odkazovaly na stejný objekt a bylo tak možné, aby operace s jednou proměnnou ovlivnily objekt, na který je odkazováno z jiné proměnné.</span><span class="sxs-lookup"><span data-stu-id="0e09e-170">With reference types, it is possible for two variables to reference the same object and thus possible for operations on one variable to affect the object referenced by the other variable.</span></span> <span data-ttu-id="0e09e-171">S typy hodnot mají proměnné, které mají svou vlastní kopii dat, a není možné, aby operace na jednom byly ovlivněny druhou (kromě případu `ref` a `out` proměnných parametrů).</span><span class="sxs-lookup"><span data-stu-id="0e09e-171">With value types, the variables each have their own copy of the data, and it is not possible for operations on one to affect the other (except in the case of `ref` and `out` parameter variables).</span></span>

<span data-ttu-id="0e09e-172">C#typy hodnot jsou dále rozděleny na ***jednoduché typy***, ***výčtové typy***, ***typy struktury***a ***typy s možnou hodnotou null***a C#odkazové typy jsou dále rozděleny do ***typů tříd***, ***typů rozhraní***, ***pole typy***a ***typy delegátů***.</span><span class="sxs-lookup"><span data-stu-id="0e09e-172">C#'s value types are further divided into ***simple types***, ***enum types***, ***struct types***, and ***nullable types***, and C#'s reference types are further divided into ***class types***, ***interface types***, ***array types***, and ***delegate types***.</span></span>

<span data-ttu-id="0e09e-173">Následující tabulka poskytuje přehled C#systému typů.</span><span class="sxs-lookup"><span data-stu-id="0e09e-173">The following table provides an overview of C#'s type system.</span></span>

| <span data-ttu-id="0e09e-174">__Kategorie__</span><span class="sxs-lookup"><span data-stu-id="0e09e-174">__Category__</span></span>    |                 | <span data-ttu-id="0e09e-175">__Popis__</span><span class="sxs-lookup"><span data-stu-id="0e09e-175">__Description__</span></span> |
|-----------------|-----------------|-----------------|
| <span data-ttu-id="0e09e-176">Typy hodnot</span><span class="sxs-lookup"><span data-stu-id="0e09e-176">Value types</span></span>     | <span data-ttu-id="0e09e-177">Jednoduché typy</span><span class="sxs-lookup"><span data-stu-id="0e09e-177">Simple types</span></span>    | <span data-ttu-id="0e09e-178">Podepsané integrály `sbyte`: `short`, `int`,,`long`</span><span class="sxs-lookup"><span data-stu-id="0e09e-178">Signed integral: `sbyte`, `short`, `int`, `long`</span></span> |
|                 |                 | <span data-ttu-id="0e09e-179">Unsigned integrál: `byte`, `ushort`, `uint`,`ulong`</span><span class="sxs-lookup"><span data-stu-id="0e09e-179">Unsigned integral: `byte`, `ushort`, `uint`, `ulong`</span></span> |
|                 |                 | <span data-ttu-id="0e09e-180">Znaky Unicode:`char`</span><span class="sxs-lookup"><span data-stu-id="0e09e-180">Unicode characters: `char`</span></span> |
|                 |                 | <span data-ttu-id="0e09e-181">Plovoucí desetinná čárka IEEE: `float`,`double`</span><span class="sxs-lookup"><span data-stu-id="0e09e-181">IEEE floating point: `float`, `double`</span></span> |
|                 |                 | <span data-ttu-id="0e09e-182">Desetinné číslo s vysokou přesností:`decimal`</span><span class="sxs-lookup"><span data-stu-id="0e09e-182">High-precision decimal: `decimal`</span></span> |
|                 |                 | <span data-ttu-id="0e09e-183">Datového`bool`</span><span class="sxs-lookup"><span data-stu-id="0e09e-183">Boolean: `bool`</span></span> |
|                 | <span data-ttu-id="0e09e-184">Výčtové typy</span><span class="sxs-lookup"><span data-stu-id="0e09e-184">Enum types</span></span>      | <span data-ttu-id="0e09e-185">Uživatelem definované typy formuláře`enum E {...}`</span><span class="sxs-lookup"><span data-stu-id="0e09e-185">User-defined types of the form `enum E {...}`</span></span> |
|                 | <span data-ttu-id="0e09e-186">Typy struktury</span><span class="sxs-lookup"><span data-stu-id="0e09e-186">Struct types</span></span>    | <span data-ttu-id="0e09e-187">Uživatelem definované typy formuláře`struct S {...}`</span><span class="sxs-lookup"><span data-stu-id="0e09e-187">User-defined types of the form `struct S {...}`</span></span> |
|                 | <span data-ttu-id="0e09e-188">Typy s povolenou hodnotou Null</span><span class="sxs-lookup"><span data-stu-id="0e09e-188">Nullable types</span></span>  | <span data-ttu-id="0e09e-189">Rozšíření všech ostatních typů hodnot s `null` hodnotou</span><span class="sxs-lookup"><span data-stu-id="0e09e-189">Extensions of all other value types with a `null` value</span></span> |
| <span data-ttu-id="0e09e-190">Typy odkazů</span><span class="sxs-lookup"><span data-stu-id="0e09e-190">Reference types</span></span> | <span data-ttu-id="0e09e-191">Typy tříd</span><span class="sxs-lookup"><span data-stu-id="0e09e-191">Class types</span></span>     | <span data-ttu-id="0e09e-192">Nejvyšší základní třída všech ostatních typů:`object`</span><span class="sxs-lookup"><span data-stu-id="0e09e-192">Ultimate base class of all other types: `object`</span></span> |
|                 |                 | <span data-ttu-id="0e09e-193">Řetězce Unicode:`string`</span><span class="sxs-lookup"><span data-stu-id="0e09e-193">Unicode strings: `string`</span></span> |
|                 |                 | <span data-ttu-id="0e09e-194">Uživatelem definované typy formuláře`class C {...}`</span><span class="sxs-lookup"><span data-stu-id="0e09e-194">User-defined types of the form `class C {...}`</span></span> |
|                 | <span data-ttu-id="0e09e-195">Typy rozhraní</span><span class="sxs-lookup"><span data-stu-id="0e09e-195">Interface types</span></span> | <span data-ttu-id="0e09e-196">Uživatelem definované typy formuláře`interface I {...}`</span><span class="sxs-lookup"><span data-stu-id="0e09e-196">User-defined types of the form `interface I {...}`</span></span> |
|                 | <span data-ttu-id="0e09e-197">Typy polí</span><span class="sxs-lookup"><span data-stu-id="0e09e-197">Array types</span></span>     | <span data-ttu-id="0e09e-198">Jednoduché a multidimenzionální, například `int[]` a`int[,]`</span><span class="sxs-lookup"><span data-stu-id="0e09e-198">Single- and multi-dimensional, for example, `int[]` and `int[,]`</span></span> |
|                 | <span data-ttu-id="0e09e-199">Typy delegátů</span><span class="sxs-lookup"><span data-stu-id="0e09e-199">Delegate types</span></span>  | <span data-ttu-id="0e09e-200">Uživatelem definované typy formuláře, např.`delegate int  D(...)`</span><span class="sxs-lookup"><span data-stu-id="0e09e-200">User-defined types of the form e.g. `delegate int  D(...)`</span></span> |

<span data-ttu-id="0e09e-201">Osm integrálních typů poskytuje podporu pro 8bitové, 16bitové, 32 a 64 hodnoty v podepsané nebo nepodepsané formě.</span><span class="sxs-lookup"><span data-stu-id="0e09e-201">The eight integral types provide support for 8-bit, 16-bit, 32-bit, and 64-bit values in signed or unsigned form.</span></span>

<span data-ttu-id="0e09e-202">Dva typy `float` s plovoucí desetinnou čárkou a `double`jsou reprezentovány pomocí 32 formátů IEEE 754 s jednoduchou přesností a 64.</span><span class="sxs-lookup"><span data-stu-id="0e09e-202">The two floating point types, `float` and `double`, are represented using the 32-bit single-precision and 64-bit double-precision IEEE 754 formats.</span></span>

<span data-ttu-id="0e09e-203">`decimal` Typ je 128 datový typ, který je vhodný pro finanční a peněžní výpočty.</span><span class="sxs-lookup"><span data-stu-id="0e09e-203">The `decimal` type is a 128-bit data type suitable for financial and monetary calculations.</span></span>

<span data-ttu-id="0e09e-204">C#typ se používá k reprezentaci logických hodnot – hodnot, které `true` jsou buď nebo `false`. `bool`</span><span class="sxs-lookup"><span data-stu-id="0e09e-204">C#'s `bool` type is used to represent boolean values—values that are either `true` or `false`.</span></span>

<span data-ttu-id="0e09e-205">Zpracování znaků a řetězců v C# nástroji používá kódování Unicode.</span><span class="sxs-lookup"><span data-stu-id="0e09e-205">Character and string processing in C# uses Unicode encoding.</span></span> <span data-ttu-id="0e09e-206">Typ představuje jednotku kódu UTF-16 `string` a typ představuje sekvenci jednotek kódu UTF-16. `char`</span><span class="sxs-lookup"><span data-stu-id="0e09e-206">The `char` type represents a UTF-16 code unit, and the `string` type represents a sequence of UTF-16 code units.</span></span>

<span data-ttu-id="0e09e-207">Následující tabulka shrnuje C#číselné typy.</span><span class="sxs-lookup"><span data-stu-id="0e09e-207">The following table summarizes C#'s numeric types.</span></span>


| <span data-ttu-id="0e09e-208">__Kategorie__</span><span class="sxs-lookup"><span data-stu-id="0e09e-208">__Category__</span></span>      | <span data-ttu-id="0e09e-209">__Bit__</span><span class="sxs-lookup"><span data-stu-id="0e09e-209">__Bits__</span></span> | <span data-ttu-id="0e09e-210">__Typ__</span><span class="sxs-lookup"><span data-stu-id="0e09e-210">__Type__</span></span>  | <span data-ttu-id="0e09e-211">__Rozsah/přesnost__</span><span class="sxs-lookup"><span data-stu-id="0e09e-211">__Range/Precision__</span></span> |
|-------------------|----------|-----------|---------------------|
| <span data-ttu-id="0e09e-212">Podepsaný integrál</span><span class="sxs-lookup"><span data-stu-id="0e09e-212">Signed integral</span></span>   | <span data-ttu-id="0e09e-213">8</span><span class="sxs-lookup"><span data-stu-id="0e09e-213">8</span></span>        | `sbyte`   | <span data-ttu-id="0e09e-214">-128...127</span><span class="sxs-lookup"><span data-stu-id="0e09e-214">-128...127</span></span> |
|                   | <span data-ttu-id="0e09e-215">16</span><span class="sxs-lookup"><span data-stu-id="0e09e-215">16</span></span>       | `short`   | <span data-ttu-id="0e09e-216">-32 768... 32, 767</span><span class="sxs-lookup"><span data-stu-id="0e09e-216">-32,768...32,767</span></span> |
|                   | <span data-ttu-id="0e09e-217">32</span><span class="sxs-lookup"><span data-stu-id="0e09e-217">32</span></span>       | `int`     | <span data-ttu-id="0e09e-218">-2147483648... 2, 147, 483, 647</span><span class="sxs-lookup"><span data-stu-id="0e09e-218">-2,147,483,648...2,147,483,647</span></span> |
|                   | <span data-ttu-id="0e09e-219">64</span><span class="sxs-lookup"><span data-stu-id="0e09e-219">64</span></span>       | `long`    | <span data-ttu-id="0e09e-220">-9223372036854775808... 9, 223, 372, 036, 854, 0,75, 807</span><span class="sxs-lookup"><span data-stu-id="0e09e-220">-9,223,372,036,854,775,808...9,223,372,036,854,775,807</span></span> |
| <span data-ttu-id="0e09e-221">Integrál bez znaménka</span><span class="sxs-lookup"><span data-stu-id="0e09e-221">Unsigned integral</span></span> | <span data-ttu-id="0e09e-222">8</span><span class="sxs-lookup"><span data-stu-id="0e09e-222">8</span></span>        | `byte`    | <span data-ttu-id="0e09e-223">0... 255</span><span class="sxs-lookup"><span data-stu-id="0e09e-223">0...255</span></span> |
|                   | <span data-ttu-id="0e09e-224">16</span><span class="sxs-lookup"><span data-stu-id="0e09e-224">16</span></span>       | `ushort`  | <span data-ttu-id="0e09e-225">0... 65, 535</span><span class="sxs-lookup"><span data-stu-id="0e09e-225">0...65,535</span></span> |
|                   | <span data-ttu-id="0e09e-226">32</span><span class="sxs-lookup"><span data-stu-id="0e09e-226">32</span></span>       | `uint`    | <span data-ttu-id="0e09e-227">0... 4, 294, 967, 295</span><span class="sxs-lookup"><span data-stu-id="0e09e-227">0...4,294,967,295</span></span> |
|                   | <span data-ttu-id="0e09e-228">64</span><span class="sxs-lookup"><span data-stu-id="0e09e-228">64</span></span>       | `ulong`   | <span data-ttu-id="0e09e-229">0... 18, 446, 744, 073, 709, 551, 615</span><span class="sxs-lookup"><span data-stu-id="0e09e-229">0...18,446,744,073,709,551,615</span></span> |
| <span data-ttu-id="0e09e-230">Plovoucí desetinná čárka</span><span class="sxs-lookup"><span data-stu-id="0e09e-230">Floating point</span></span>    | <span data-ttu-id="0e09e-231">32</span><span class="sxs-lookup"><span data-stu-id="0e09e-231">32</span></span>       | `float`   | <span data-ttu-id="0e09e-232">1,5 × 10 ^ − 45 až 3,4 × 10 ^ 38, 7 – přesnost číslic</span><span class="sxs-lookup"><span data-stu-id="0e09e-232">1.5 × 10^−45 to 3.4 × 10^38, 7-digit precision</span></span> |
|                   | <span data-ttu-id="0e09e-233">64</span><span class="sxs-lookup"><span data-stu-id="0e09e-233">64</span></span>       | `double`  | <span data-ttu-id="0e09e-234">5,0 × 10 ^ − 324 a 1,7 × 10 ^ 308, přesnost na 15 číslic</span><span class="sxs-lookup"><span data-stu-id="0e09e-234">5.0 × 10^−324 to 1.7 × 10^308, 15-digit precision</span></span> |
| <span data-ttu-id="0e09e-235">Desetinné číslo</span><span class="sxs-lookup"><span data-stu-id="0e09e-235">Decimal</span></span>           | <span data-ttu-id="0e09e-236">128</span><span class="sxs-lookup"><span data-stu-id="0e09e-236">128</span></span>      | `decimal` | <span data-ttu-id="0e09e-237">1,0 × 10 ^ − 28 až 7,9 × 10 ^ 28, 28 číslic – přesnost</span><span class="sxs-lookup"><span data-stu-id="0e09e-237">1.0 × 10^−28 to 7.9 × 10^28, 28-digit precision</span></span> |

<span data-ttu-id="0e09e-238">C#programy používají ***deklarace typů*** k vytváření nových typů.</span><span class="sxs-lookup"><span data-stu-id="0e09e-238">C# programs use ***type declarations*** to create new types.</span></span> <span data-ttu-id="0e09e-239">Deklarace typu Určuje název a členy nového typu.</span><span class="sxs-lookup"><span data-stu-id="0e09e-239">A type declaration specifies the name and the members of the new type.</span></span> <span data-ttu-id="0e09e-240">C#Pět kategorií typů je uživatelsky definované: typy tříd, typy struktury, typy rozhraní, výčtové typy a typy delegátů.</span><span class="sxs-lookup"><span data-stu-id="0e09e-240">Five of C#'s categories of types are user-definable: class types, struct types, interface types, enum types, and delegate types.</span></span>

<span data-ttu-id="0e09e-241">Typ třídy definuje datovou strukturu obsahující datové členy (pole) a členy funkce (metody, vlastnosti a další).</span><span class="sxs-lookup"><span data-stu-id="0e09e-241">A class type defines a data structure that contains data members (fields) and function members (methods, properties, and others).</span></span> <span data-ttu-id="0e09e-242">Typy tříd podporují jednu dědičnost a polymorfismus, mechanismy, které mohou odvozené třídy roztáhnout a specializovat základní třídy.</span><span class="sxs-lookup"><span data-stu-id="0e09e-242">Class types support single inheritance and polymorphism, mechanisms whereby derived classes can extend and specialize base classes.</span></span>

<span data-ttu-id="0e09e-243">Typ struktury je podobný typu třídy v tom, že představuje strukturu s datovými členy a členy funkce.</span><span class="sxs-lookup"><span data-stu-id="0e09e-243">A struct type is similar to a class type in that it represents a structure with data members and function members.</span></span> <span data-ttu-id="0e09e-244">Nicméně na rozdíl od tříd, struktury jsou typy hodnot a nevyžadují přidělení haldy.</span><span class="sxs-lookup"><span data-stu-id="0e09e-244">However, unlike classes, structs are value types and do not require heap allocation.</span></span> <span data-ttu-id="0e09e-245">Typy struktury nepodporují uživatelem zadanou dědičnost a všechny typy struktury implicitně dědí z typu `object`.</span><span class="sxs-lookup"><span data-stu-id="0e09e-245">Struct types do not support user-specified inheritance, and all struct types implicitly inherit from type `object`.</span></span>

<span data-ttu-id="0e09e-246">Typ rozhraní definuje kontrakt jako pojmenovanou sadu členů veřejné funkce.</span><span class="sxs-lookup"><span data-stu-id="0e09e-246">An interface type defines a contract as a named set of public function members.</span></span> <span data-ttu-id="0e09e-247">Třída nebo struktura, která implementuje rozhraní, musí poskytovat implementace členů funkce rozhraní.</span><span class="sxs-lookup"><span data-stu-id="0e09e-247">A class or struct that implements an interface must provide implementations of the interface's function members.</span></span> <span data-ttu-id="0e09e-248">Rozhraní může dědit z více základních rozhraní a třída nebo struktura může implementovat více rozhraní.</span><span class="sxs-lookup"><span data-stu-id="0e09e-248">An interface may inherit from multiple base interfaces, and a class or struct may implement multiple interfaces.</span></span>

<span data-ttu-id="0e09e-249">Typ delegáta představuje odkazy na metody s konkrétním seznamem parametrů a návratovým typem.</span><span class="sxs-lookup"><span data-stu-id="0e09e-249">A delegate type represents references to methods with a particular parameter list and return type.</span></span> <span data-ttu-id="0e09e-250">Delegáti umožňují zacházet s metodami jako s entitami, které lze přiřadit proměnným a předávat jako parametry.</span><span class="sxs-lookup"><span data-stu-id="0e09e-250">Delegates make it possible to treat methods as entities that can be assigned to variables and passed as parameters.</span></span> <span data-ttu-id="0e09e-251">Delegáti jsou podobní pojmu ukazatelů na funkce nalezené v některých jiných jazycích, ale na rozdíl od ukazatelů na funkce jsou delegáti objektově orientované a typově bezpečné.</span><span class="sxs-lookup"><span data-stu-id="0e09e-251">Delegates are similar to the concept of function pointers found in some other languages, but unlike function pointers, delegates are object-oriented and type-safe.</span></span>

<span data-ttu-id="0e09e-252">Třídy, struktura, rozhraní a typy delegátů podporují obecné typy, kde je lze parametrizovanit s jinými typy.</span><span class="sxs-lookup"><span data-stu-id="0e09e-252">Class, struct, interface and delegate types all support generics, whereby they can be parameterized with other types.</span></span>

<span data-ttu-id="0e09e-253">Typ výčtu je odlišný typ s pojmenovanými konstantami.</span><span class="sxs-lookup"><span data-stu-id="0e09e-253">An enum type is a distinct type with named constants.</span></span> <span data-ttu-id="0e09e-254">Každý typ výčtu má podkladový typ, který musí být jedním z osmi integrálních typů.</span><span class="sxs-lookup"><span data-stu-id="0e09e-254">Every enum type has an underlying type, which must be one of the eight integral types.</span></span> <span data-ttu-id="0e09e-255">Sada hodnot typu výčtu je stejná jako sada hodnot základního typu.</span><span class="sxs-lookup"><span data-stu-id="0e09e-255">The set of values of an enum type is the same as the set of values of the underlying type.</span></span>

<span data-ttu-id="0e09e-256">C#podporuje jedno a multidimenzionální pole libovolného typu.</span><span class="sxs-lookup"><span data-stu-id="0e09e-256">C# supports single- and multi-dimensional arrays of any type.</span></span> <span data-ttu-id="0e09e-257">Na rozdíl od typů uvedených výše nemusí být typy polí deklarovány dříve, než mohou být použity.</span><span class="sxs-lookup"><span data-stu-id="0e09e-257">Unlike the types listed above, array types do not have to be declared before they can be used.</span></span> <span data-ttu-id="0e09e-258">Místo toho jsou typy polí konstruovány pomocí názvu typu s hranatými závorkami.</span><span class="sxs-lookup"><span data-stu-id="0e09e-258">Instead, array types are constructed by following a type name with square brackets.</span></span> <span data-ttu-id="0e09e-259">`int[]` Například je `int` `int` `int[][]` jednorozměrné pole `int[,]` , je dvourozměrné pole, a je jednorozměrné pole jednorozměrného pole v rámci. `int`</span><span class="sxs-lookup"><span data-stu-id="0e09e-259">For example, `int[]` is a single-dimensional array of `int`, `int[,]` is a two-dimensional array of `int`, and `int[][]` is a single-dimensional array of single-dimensional arrays of `int`.</span></span>

<span data-ttu-id="0e09e-260">Typy s možnou hodnotou null také nemusí být deklarovány dříve, než mohou být použity.</span><span class="sxs-lookup"><span data-stu-id="0e09e-260">Nullable types also do not have to be declared before they can be used.</span></span> <span data-ttu-id="0e09e-261">Pro každý typ `T` hodnoty, která není null, existuje odpovídající typ `T?`s možnou hodnotou null, který může `null`obsahovat další hodnotu.</span><span class="sxs-lookup"><span data-stu-id="0e09e-261">For each non-nullable value type `T` there is a corresponding nullable type `T?`, which can hold an additional value `null`.</span></span> <span data-ttu-id="0e09e-262">Například je typ, který může obsahovat libovolné celé číslo 32 nebo hodnotu `null`. `int?`</span><span class="sxs-lookup"><span data-stu-id="0e09e-262">For instance, `int?` is a type that can hold any 32 bit integer or the value `null`.</span></span>

<span data-ttu-id="0e09e-263">C#systém typů je sjednocený tak, že hodnota libovolného typu může být zpracována jako objekt.</span><span class="sxs-lookup"><span data-stu-id="0e09e-263">C#'s type system is unified such that a value of any type can be treated as an object.</span></span> <span data-ttu-id="0e09e-264">Každý typ C# přímo nebo nepřímo je odvozen z `object` typu třídy a `object` je nejvyšší základní třídou všech typů.</span><span class="sxs-lookup"><span data-stu-id="0e09e-264">Every type in C# directly or indirectly derives from the `object` class type, and `object` is the ultimate base class of all types.</span></span> <span data-ttu-id="0e09e-265">Hodnoty typů odkazů se považují za objekty pouhým zobrazením hodnot jako typu `object`.</span><span class="sxs-lookup"><span data-stu-id="0e09e-265">Values of reference types are treated as objects simply by viewing the values as type `object`.</span></span> <span data-ttu-id="0e09e-266">Hodnoty typů hodnot se považují za objekty prováděním operací ***zabalení*** a ***rozbalení*** .</span><span class="sxs-lookup"><span data-stu-id="0e09e-266">Values of value types are treated as objects by performing ***boxing*** and ***unboxing*** operations.</span></span> <span data-ttu-id="0e09e-267">V následujícím příkladu `int` je hodnota převedena na `object` a zpět na `int`.</span><span class="sxs-lookup"><span data-stu-id="0e09e-267">In the following example, an `int` value is converted to `object` and back again to `int`.</span></span>

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
<span data-ttu-id="0e09e-268">Je-li hodnota typu hodnoty převedena na typ `object`, je přidělena instance objektu, která se také označuje jako "pole", pro uchování hodnoty a hodnota je zkopírována do tohoto pole.</span><span class="sxs-lookup"><span data-stu-id="0e09e-268">When a value of a value type is converted to type `object`, an object instance, also called a "box," is allocated to hold the value, and the value is copied into that box.</span></span> <span data-ttu-id="0e09e-269">Naopak, pokud `object` je odkaz přetypování na typ hodnoty, je provedena kontrolu, že odkazovaný objekt je pole správného typu hodnoty, a pokud je ověření úspěšné, je hodnota v poli zkopírována.</span><span class="sxs-lookup"><span data-stu-id="0e09e-269">Conversely, when an `object` reference is cast to a value type, a check is made that the referenced object is a box of the correct value type, and, if the check succeeds, the value in the box is copied out.</span></span>

<span data-ttu-id="0e09e-270">C#Sjednocený typ systému efektivně znamená, že typy hodnot se můžou stát objekty na vyžádání.</span><span class="sxs-lookup"><span data-stu-id="0e09e-270">C#'s unified type system effectively means that value types can become objects "on demand."</span></span> <span data-ttu-id="0e09e-271">Z důvodu sjednocení, knihovny pro obecné účely, které používají typ `object` , lze použít s oběma typy odkazů i s typy hodnot.</span><span class="sxs-lookup"><span data-stu-id="0e09e-271">Because of the unification, general-purpose libraries that use type `object` can be used with both reference types and value types.</span></span>

<span data-ttu-id="0e09e-272">Existuje několik druhů ***proměnných*** v C#, včetně polí, prvků pole, místních proměnných a parametrů.</span><span class="sxs-lookup"><span data-stu-id="0e09e-272">There are several kinds of ***variables*** in C#, including fields, array elements, local variables, and parameters.</span></span> <span data-ttu-id="0e09e-273">Proměnné reprezentují umístění úložiště a každá proměnná má typ, který určuje, jaké hodnoty mohou být uloženy v proměnné, jak je znázorněno v následující tabulce.</span><span class="sxs-lookup"><span data-stu-id="0e09e-273">Variables represent storage locations, and every variable has a type that determines what values can be stored in the variable, as shown by the following table.</span></span>


| <span data-ttu-id="0e09e-274">__Typ proměnné__</span><span class="sxs-lookup"><span data-stu-id="0e09e-274">__Type of Variable__</span></span>    | <span data-ttu-id="0e09e-275">__Možný obsah__</span><span class="sxs-lookup"><span data-stu-id="0e09e-275">__Possible Contents__</span></span> |
|-------------------------|-----------------------|
| <span data-ttu-id="0e09e-276">Typ hodnoty, která není null</span><span class="sxs-lookup"><span data-stu-id="0e09e-276">Non-nullable value type</span></span> | <span data-ttu-id="0e09e-277">Hodnota, která má přesný typ</span><span class="sxs-lookup"><span data-stu-id="0e09e-277">A value of that exact type</span></span> |
| <span data-ttu-id="0e09e-278">Typ hodnoty s možnou hodnotou null</span><span class="sxs-lookup"><span data-stu-id="0e09e-278">Nullable value type</span></span>     | <span data-ttu-id="0e09e-279">Hodnota null nebo hodnota daného přesného typu</span><span class="sxs-lookup"><span data-stu-id="0e09e-279">A null value or a value of that exact type</span></span> |
| `object`                | <span data-ttu-id="0e09e-280">Odkaz s hodnotou null, odkaz na objekt libovolného typu odkazu nebo odkaz na zabalenou hodnotu libovolného typu hodnoty</span><span class="sxs-lookup"><span data-stu-id="0e09e-280">A null reference, a reference to an object of any reference type, or a reference to a boxed value of any value type</span></span> |
| <span data-ttu-id="0e09e-281">Typ třídy</span><span class="sxs-lookup"><span data-stu-id="0e09e-281">Class type</span></span>              | <span data-ttu-id="0e09e-282">Odkaz s hodnotou null, odkaz na instanci tohoto typu třídy nebo odkaz na instanci třídy odvozené z tohoto typu třídy</span><span class="sxs-lookup"><span data-stu-id="0e09e-282">A null reference, a reference to an instance of that class type, or a reference to an instance of a class derived from that class type</span></span> |
| <span data-ttu-id="0e09e-283">Typ rozhraní</span><span class="sxs-lookup"><span data-stu-id="0e09e-283">Interface type</span></span>          | <span data-ttu-id="0e09e-284">Odkaz s hodnotou null, odkaz na instanci typu třídy, který implementuje tento typ rozhraní, nebo odkaz na zabalenou hodnotu typu hodnoty, který implementuje tento typ rozhraní</span><span class="sxs-lookup"><span data-stu-id="0e09e-284">A null reference, a reference to an instance of a class type that implements that interface type, or a reference to a boxed value of a value type that implements that interface type</span></span> |
| <span data-ttu-id="0e09e-285">Typ pole</span><span class="sxs-lookup"><span data-stu-id="0e09e-285">Array type</span></span>              | <span data-ttu-id="0e09e-286">Odkaz s hodnotou null, odkaz na instanci tohoto typu pole nebo odkaz na instanci kompatibilního typu pole</span><span class="sxs-lookup"><span data-stu-id="0e09e-286">A null reference, a reference to an instance of that array type, or a reference to an instance of a compatible array type</span></span> |
| <span data-ttu-id="0e09e-287">Typ delegáta</span><span class="sxs-lookup"><span data-stu-id="0e09e-287">Delegate type</span></span>           | <span data-ttu-id="0e09e-288">Odkaz s hodnotou null nebo odkaz na instanci tohoto typu delegáta</span><span class="sxs-lookup"><span data-stu-id="0e09e-288">A null reference or a reference to an instance of that delegate type</span></span> |

## <a name="expressions"></a><span data-ttu-id="0e09e-289">Výrazy</span><span class="sxs-lookup"><span data-stu-id="0e09e-289">Expressions</span></span>

<span data-ttu-id="0e09e-290">***Výrazy*** jsou vytvořené z ***operandů*** a ***operátorů***.</span><span class="sxs-lookup"><span data-stu-id="0e09e-290">***Expressions*** are constructed from ***operands*** and ***operators***.</span></span> <span data-ttu-id="0e09e-291">Operátory výrazu označují, které operace se mají použít u operandů.</span><span class="sxs-lookup"><span data-stu-id="0e09e-291">The operators of an expression indicate which operations to apply to the operands.</span></span> <span data-ttu-id="0e09e-292">Příklady operátorů zahrnují `+`, `-`, `*` `/`, a .`new`</span><span class="sxs-lookup"><span data-stu-id="0e09e-292">Examples of operators include `+`, `-`, `*`, `/`, and `new`.</span></span> <span data-ttu-id="0e09e-293">Příklady operandů zahrnují literály, pole, místní proměnné a výrazy.</span><span class="sxs-lookup"><span data-stu-id="0e09e-293">Examples of operands include literals, fields, local variables, and expressions.</span></span>

<span data-ttu-id="0e09e-294">Pokud výraz obsahuje více operátorů, ***Priorita*** operátorů řídí pořadí, ve kterém jsou jednotlivé operátory vyhodnocovány.</span><span class="sxs-lookup"><span data-stu-id="0e09e-294">When an expression contains multiple operators, the ***precedence*** of the operators controls the order in which the individual operators are evaluated.</span></span> <span data-ttu-id="0e09e-295">Například výraz `x + y * z` je vyhodnocen jako `x + (y * z)` , protože `*` operátor má vyšší prioritu než `+` operátor.</span><span class="sxs-lookup"><span data-stu-id="0e09e-295">For example, the expression `x + y * z` is evaluated as `x + (y * z)` because the `*` operator has higher precedence than the `+` operator.</span></span>

<span data-ttu-id="0e09e-296">Většina operátorů může být ***přetížená***.</span><span class="sxs-lookup"><span data-stu-id="0e09e-296">Most operators can be ***overloaded***.</span></span> <span data-ttu-id="0e09e-297">Přetížení operátoru umožňuje zadat uživatelsky definované implementace operátorů pro operace, u nichž jeden nebo oba operandy jsou uživatelsky definovaný typ třídy nebo struktury.</span><span class="sxs-lookup"><span data-stu-id="0e09e-297">Operator overloading permits user-defined operator implementations to be specified for operations where one or both of the operands are of a user-defined class or struct type.</span></span>

<span data-ttu-id="0e09e-298">Následující tabulka shrnuje C#operátory, které obsahují kategorie operátora v pořadí podle priority od nejvyšších po nejnižší.</span><span class="sxs-lookup"><span data-stu-id="0e09e-298">The following table summarizes C#'s operators, listing the operator categories in order of precedence from highest to lowest.</span></span> <span data-ttu-id="0e09e-299">Operátory ve stejné kategorii mají stejnou prioritu.</span><span class="sxs-lookup"><span data-stu-id="0e09e-299">Operators in the same category have equal precedence.</span></span>


| <span data-ttu-id="0e09e-300">__Kategorie__</span><span class="sxs-lookup"><span data-stu-id="0e09e-300">__Category__</span></span>                     | <span data-ttu-id="0e09e-301">__Vyjádření__</span><span class="sxs-lookup"><span data-stu-id="0e09e-301">__Expression__</span></span>    | <span data-ttu-id="0e09e-302">__Popis__</span><span class="sxs-lookup"><span data-stu-id="0e09e-302">__Description__</span></span> |
|----------------------------------|-------------------|-----------------|
| <span data-ttu-id="0e09e-303">Primární</span><span class="sxs-lookup"><span data-stu-id="0e09e-303">Primary</span></span>                          | `x.m`             | <span data-ttu-id="0e09e-304">Přístup ke členům</span><span class="sxs-lookup"><span data-stu-id="0e09e-304">Member access</span></span> |
|                                  | `x(...)`          | <span data-ttu-id="0e09e-305">Vyvolání metod a delegátů</span><span class="sxs-lookup"><span data-stu-id="0e09e-305">Method and delegate invocation</span></span> |
|                                  | `x[...]`          | <span data-ttu-id="0e09e-306">Přístup k poli a indexeru</span><span class="sxs-lookup"><span data-stu-id="0e09e-306">Array and indexer access</span></span> |
|                                  | `x++`             | <span data-ttu-id="0e09e-307">Postinkrementace</span><span class="sxs-lookup"><span data-stu-id="0e09e-307">Post-increment</span></span> |
|                                  | `x--`             | <span data-ttu-id="0e09e-308">Postdekrementace</span><span class="sxs-lookup"><span data-stu-id="0e09e-308">Post-decrement</span></span> |
|                                  | `new T(...)`      | <span data-ttu-id="0e09e-309">Vytvoření objektu a delegátu</span><span class="sxs-lookup"><span data-stu-id="0e09e-309">Object and delegate creation</span></span> |
|                                  | `new T(...){...}` | <span data-ttu-id="0e09e-310">Vytvoření objektu s inicializátorem</span><span class="sxs-lookup"><span data-stu-id="0e09e-310">Object creation with initializer</span></span> |
|                                  | `new {...}`       | <span data-ttu-id="0e09e-311">Inicializátor anonymních objektů</span><span class="sxs-lookup"><span data-stu-id="0e09e-311">Anonymous object initializer</span></span> |
|                                  | `new T[...]`      | <span data-ttu-id="0e09e-312">Vytvoření pole</span><span class="sxs-lookup"><span data-stu-id="0e09e-312">Array creation</span></span> |
|                                  | `typeof(T)`       | <span data-ttu-id="0e09e-313">Získat `System.Type` objekt pro`T`</span><span class="sxs-lookup"><span data-stu-id="0e09e-313">Obtain `System.Type` object for `T`</span></span> |
|                                  | `checked(x)`      | <span data-ttu-id="0e09e-314">Vyhodnocení výrazu ve zkontrolovaném kontextu</span><span class="sxs-lookup"><span data-stu-id="0e09e-314">Evaluate expression in checked context</span></span> |
|                                  | `unchecked(x)`    | <span data-ttu-id="0e09e-315">Vyhodnocení výrazu v nezkontrolovaném kontextu</span><span class="sxs-lookup"><span data-stu-id="0e09e-315">Evaluate expression in unchecked context</span></span> |
|                                  | `default(T)`      | <span data-ttu-id="0e09e-316">Získat výchozí hodnotu typu`T`</span><span class="sxs-lookup"><span data-stu-id="0e09e-316">Obtain default value of type `T`</span></span> |
|                                  | `delegate {...}`  | <span data-ttu-id="0e09e-317">Anonymní funkce (anonymní metoda)</span><span class="sxs-lookup"><span data-stu-id="0e09e-317">Anonymous function (anonymous method)</span></span> |
| <span data-ttu-id="0e09e-318">Unární</span><span class="sxs-lookup"><span data-stu-id="0e09e-318">Unary</span></span>                            | `+x`              | <span data-ttu-id="0e09e-319">Identita</span><span class="sxs-lookup"><span data-stu-id="0e09e-319">Identity</span></span> |
|                                  | `-x`              | <span data-ttu-id="0e09e-320">Negace</span><span class="sxs-lookup"><span data-stu-id="0e09e-320">Negation</span></span> |
|                                  | `!x`              | <span data-ttu-id="0e09e-321">Logická negace</span><span class="sxs-lookup"><span data-stu-id="0e09e-321">Logical negation</span></span> |
|                                  | `~x`              | <span data-ttu-id="0e09e-322">Bitová negace.</span><span class="sxs-lookup"><span data-stu-id="0e09e-322">Bitwise negation</span></span> |
|                                  | `++x`             | <span data-ttu-id="0e09e-323">Preinkrementace</span><span class="sxs-lookup"><span data-stu-id="0e09e-323">Pre-increment</span></span> |
|                                  | `--x`             | <span data-ttu-id="0e09e-324">Predekrementace</span><span class="sxs-lookup"><span data-stu-id="0e09e-324">Pre-decrement</span></span> |
|                                  | `(T)x`            | <span data-ttu-id="0e09e-325">Explicitně převést `x` na typ`T`</span><span class="sxs-lookup"><span data-stu-id="0e09e-325">Explicitly convert `x` to type `T`</span></span> |
|                                  | `await x`         | <span data-ttu-id="0e09e-326">Asynchronní čekání `x` na dokončení</span><span class="sxs-lookup"><span data-stu-id="0e09e-326">Asynchronously wait for `x` to complete</span></span> |
| <span data-ttu-id="0e09e-327">Multiplikativní</span><span class="sxs-lookup"><span data-stu-id="0e09e-327">Multiplicative</span></span>                   | `x * y`           | <span data-ttu-id="0e09e-328">Násobení</span><span class="sxs-lookup"><span data-stu-id="0e09e-328">Multiplication</span></span> |
|                                  | `x / y`           | <span data-ttu-id="0e09e-329">Dělení</span><span class="sxs-lookup"><span data-stu-id="0e09e-329">Division</span></span> |
|                                  | `x % y`           | <span data-ttu-id="0e09e-330">Zbytek</span><span class="sxs-lookup"><span data-stu-id="0e09e-330">Remainder</span></span> |
| <span data-ttu-id="0e09e-331">Přičítáním</span><span class="sxs-lookup"><span data-stu-id="0e09e-331">Additive</span></span>                         | `x + y`           | <span data-ttu-id="0e09e-332">Sčítání, řetězení řetězců, kombinování delegátů</span><span class="sxs-lookup"><span data-stu-id="0e09e-332">Addition, string concatenation, delegate combination</span></span> |
|                                  | `x - y`           | <span data-ttu-id="0e09e-333">Odčítání, odebrání delegátů</span><span class="sxs-lookup"><span data-stu-id="0e09e-333">Subtraction, delegate removal</span></span> |
| <span data-ttu-id="0e09e-334">SHIFT</span><span class="sxs-lookup"><span data-stu-id="0e09e-334">Shift</span></span>                            | `x << y`          | <span data-ttu-id="0e09e-335">Posun doleva</span><span class="sxs-lookup"><span data-stu-id="0e09e-335">Shift left</span></span> |
|                                  | `x >> y`          | <span data-ttu-id="0e09e-336">Posun doprava</span><span class="sxs-lookup"><span data-stu-id="0e09e-336">Shift right</span></span> |
| <span data-ttu-id="0e09e-337">Relační testování a testování typu</span><span class="sxs-lookup"><span data-stu-id="0e09e-337">Relational and type testing</span></span>      | `x < y`           | <span data-ttu-id="0e09e-338">Menší než</span><span class="sxs-lookup"><span data-stu-id="0e09e-338">Less than</span></span> |
|                                  | `x > y`           | <span data-ttu-id="0e09e-339">Větší než</span><span class="sxs-lookup"><span data-stu-id="0e09e-339">Greater than</span></span> |
|                                  | `x <= y`          | <span data-ttu-id="0e09e-340">Menší nebo rovno</span><span class="sxs-lookup"><span data-stu-id="0e09e-340">Less than or equal</span></span> |
|                                  | `x >= y`          | <span data-ttu-id="0e09e-341">Větší nebo rovno</span><span class="sxs-lookup"><span data-stu-id="0e09e-341">Greater than or equal</span></span> |
|                                  | `x is T`          | <span data-ttu-id="0e09e-342">Vrátí `true` ,`T`Pokud `x` je, jinak`false`</span><span class="sxs-lookup"><span data-stu-id="0e09e-342">Return `true` if `x` is a `T`, `false` otherwise</span></span> |
|                                  | `x as T`          | <span data-ttu-id="0e09e-343">Typ `x` se vrátí `T`jako nebo `null` Pokud `x` není`T`</span><span class="sxs-lookup"><span data-stu-id="0e09e-343">Return `x` typed as `T`, or `null` if `x` is not a `T`</span></span> |
| <span data-ttu-id="0e09e-344">Rovnost</span><span class="sxs-lookup"><span data-stu-id="0e09e-344">Equality</span></span>                         | `x == y`          | <span data-ttu-id="0e09e-345">Rovno</span><span class="sxs-lookup"><span data-stu-id="0e09e-345">Equal</span></span>      |
|                                  | `x != y`          | <span data-ttu-id="0e09e-346">Nerovná se</span><span class="sxs-lookup"><span data-stu-id="0e09e-346">Not equal</span></span> |
| <span data-ttu-id="0e09e-347">Logický operátor AND</span><span class="sxs-lookup"><span data-stu-id="0e09e-347">Logical AND</span></span>                      | `x & y`           | <span data-ttu-id="0e09e-348">Celočíselná bitová a logická logická hodnota</span><span class="sxs-lookup"><span data-stu-id="0e09e-348">Integer bitwise AND, boolean logical AND</span></span> |
| <span data-ttu-id="0e09e-349">Logický operátor XOR</span><span class="sxs-lookup"><span data-stu-id="0e09e-349">Logical XOR</span></span>                      | `x ^ y`           | <span data-ttu-id="0e09e-350">Bitový operátor XOR celého čísla, logická hodnota operátoru XOR</span><span class="sxs-lookup"><span data-stu-id="0e09e-350">Integer bitwise XOR, boolean logical XOR</span></span> |
| <span data-ttu-id="0e09e-351">Logický operátor OR</span><span class="sxs-lookup"><span data-stu-id="0e09e-351">Logical OR</span></span>                       | <code>x &#124; y</code> | <span data-ttu-id="0e09e-352">Bitový operátor OR celého čísla, logická hodnota operátoru OR</span><span class="sxs-lookup"><span data-stu-id="0e09e-352">Integer bitwise OR, boolean logical OR</span></span> |
| <span data-ttu-id="0e09e-353">Podmiňovací operátor AND</span><span class="sxs-lookup"><span data-stu-id="0e09e-353">Conditional AND</span></span>                  | `x && y`          | <span data-ttu-id="0e09e-354">Vyhodnotí `x`pouzev případě, že je `y``true`</span><span class="sxs-lookup"><span data-stu-id="0e09e-354">Evaluates `y` only if `x` is `true`</span></span> |
| <span data-ttu-id="0e09e-355">Podmiňovací operátor OR</span><span class="sxs-lookup"><span data-stu-id="0e09e-355">Conditional OR</span></span>                   | <code>x &#124;&#124; y</code> | <span data-ttu-id="0e09e-356">Vyhodnotí `x`pouzev případě, že je `y``false`</span><span class="sxs-lookup"><span data-stu-id="0e09e-356">Evaluates `y` only if `x` is `false`</span></span> |
| <span data-ttu-id="0e09e-357">Nulové sloučení</span><span class="sxs-lookup"><span data-stu-id="0e09e-357">Null coalescing</span></span>                  | `x ?? y`          | <span data-ttu-id="0e09e-358">Vyhodnotí jako `x` v `null` `x` případě, že je, do jiného `y`</span><span class="sxs-lookup"><span data-stu-id="0e09e-358">Evaluates to `y` if `x` is `null`, to `x` otherwise</span></span> |
| <span data-ttu-id="0e09e-359">Podmiňovací operátor</span><span class="sxs-lookup"><span data-stu-id="0e09e-359">Conditional</span></span>                      | `x ? y : z`       | <span data-ttu-id="0e09e-360">`x` `true` `z` Vyhodnotí`x` , pokud je, pokud je `y``false`</span><span class="sxs-lookup"><span data-stu-id="0e09e-360">Evaluates `y` if `x` is `true`, `z` if `x` is `false`</span></span> |
| <span data-ttu-id="0e09e-361">Přiřazení nebo anonymní funkce</span><span class="sxs-lookup"><span data-stu-id="0e09e-361">Assignment or anonymous function</span></span> | `x = y`           | <span data-ttu-id="0e09e-362">Přiřazení</span><span class="sxs-lookup"><span data-stu-id="0e09e-362">Assignment</span></span> |
|                                  | `x op= y`         | <span data-ttu-id="0e09e-363">Složené přiřazení; podporované operátory jsou `*=` `/=` `%=` `+=` `-=` `<<=` `>>=` `&=` `^=`<code>&#124;=</code></span><span class="sxs-lookup"><span data-stu-id="0e09e-363">Compound assignment; supported operators are `*=` `/=` `%=` `+=` `-=` `<<=` `>>=` `&=` `^=` <code>&#124;=</code></span></span> |
|                                  | `(T x) => y`      | <span data-ttu-id="0e09e-364">Anonymní funkce (výraz lambda)</span><span class="sxs-lookup"><span data-stu-id="0e09e-364">Anonymous function (lambda expression)</span></span> |

## <a name="statements"></a><span data-ttu-id="0e09e-365">Příkazy</span><span class="sxs-lookup"><span data-stu-id="0e09e-365">Statements</span></span>

<span data-ttu-id="0e09e-366">Akce programu jsou vyjádřeny pomocí ***příkazů***.</span><span class="sxs-lookup"><span data-stu-id="0e09e-366">The actions of a program are expressed using ***statements***.</span></span> <span data-ttu-id="0e09e-367">C#podporuje několik různých druhů příkazů, jejichž počet je definován z podmínek vložených příkazů.</span><span class="sxs-lookup"><span data-stu-id="0e09e-367">C# supports several different kinds of statements, a number of which are defined in terms of embedded statements.</span></span>

<span data-ttu-id="0e09e-368">***Blok*** povoluje zápis více příkazů v kontextech, kde je povolen jediný příkaz.</span><span class="sxs-lookup"><span data-stu-id="0e09e-368">A ***block*** permits multiple statements to be written in contexts where a single statement is allowed.</span></span> <span data-ttu-id="0e09e-369">Blok obsahuje seznam příkazů zapsaných mezi oddělovači `{` a. `}`</span><span class="sxs-lookup"><span data-stu-id="0e09e-369">A block consists of a list of statements written between the delimiters `{` and `}`.</span></span>

<span data-ttu-id="0e09e-370">***Příkazy deklarace*** slouží k deklaraci lokálních proměnných a konstant.</span><span class="sxs-lookup"><span data-stu-id="0e09e-370">***Declaration statements*** are used to declare local variables and constants.</span></span>

<span data-ttu-id="0e09e-371">***Příkazy výrazů*** se používají k vyhodnocení výrazů.</span><span class="sxs-lookup"><span data-stu-id="0e09e-371">***Expression statements*** are used to evaluate expressions.</span></span> <span data-ttu-id="0e09e-372">Výrazy, které lze použít jako příkazy, zahrnují vyvolání metod, přidělení objektů pomocí `new` operátoru, přiřazení pomocí `=` operátorů a složené operátory přiřazení, zvýšení a snížení provozu pomocí `++`operátory and a await. `--`</span><span class="sxs-lookup"><span data-stu-id="0e09e-372">Expressions that can be used as statements include method invocations, object allocations using the `new` operator, assignments using `=` and the compound assignment operators, increment and decrement operations using the `++` and `--` operators and await expressions.</span></span>

<span data-ttu-id="0e09e-373">***Příkazy výběru*** se používají k výběru jednoho z několika možných příkazů pro spuštění na základě hodnoty nějakého výrazu.</span><span class="sxs-lookup"><span data-stu-id="0e09e-373">***Selection statements*** are used to select one of a number of possible statements for execution based on the value of some expression.</span></span> <span data-ttu-id="0e09e-374">V této skupině jsou `if` příkazy a. `switch`</span><span class="sxs-lookup"><span data-stu-id="0e09e-374">In this group are the `if` and `switch` statements.</span></span>

<span data-ttu-id="0e09e-375">***Příkazy iterace*** se používají k opakovanému provedení vloženého příkazu.</span><span class="sxs-lookup"><span data-stu-id="0e09e-375">***Iteration statements*** are used to repeatedly execute an embedded statement.</span></span> <span data-ttu-id="0e09e-376">V této `while`skupině jsou `foreach` příkazy, `do`, `for`a.</span><span class="sxs-lookup"><span data-stu-id="0e09e-376">In this group are the `while`, `do`, `for`, and `foreach` statements.</span></span>

<span data-ttu-id="0e09e-377">***Příkazy skoku*** slouží k přenosu ovládacího prvku.</span><span class="sxs-lookup"><span data-stu-id="0e09e-377">***Jump statements*** are used to transfer control.</span></span> <span data-ttu-id="0e09e-378">V této skupině jsou `break`příkazy, `continue`, `goto`, `throw`, `return`a. `yield`</span><span class="sxs-lookup"><span data-stu-id="0e09e-378">In this group are the `break`, `continue`, `goto`, `throw`, `return`, and `yield` statements.</span></span>

<span data-ttu-id="0e09e-379">`try`... příkaz slouží k zachycení výjimek, ke kterým dojde během provádění bloku, `try`a... `catch` `finally` příkaz slouží k určení výsledného kódu, který je vždy spuštěn, zda došlo k výjimce nebo nikoli.</span><span class="sxs-lookup"><span data-stu-id="0e09e-379">The `try`...`catch` statement is used to catch exceptions that occur during execution of a block, and the `try`...`finally` statement is used to specify finalization code that is always executed, whether an exception occurred or not.</span></span>

<span data-ttu-id="0e09e-380">Příkazy `checked` a`unchecked` slouží k řízení kontextu kontroly přetečení pro aritmetické operace a převody integrálního typu.</span><span class="sxs-lookup"><span data-stu-id="0e09e-380">The `checked` and `unchecked` statements are used to control the overflow checking context for integral-type arithmetic operations and conversions.</span></span>

<span data-ttu-id="0e09e-381">`lock` Příkaz slouží k získání zámku vzájemného vyloučení pro daný objekt, spuštění příkazu a uvolnění zámku.</span><span class="sxs-lookup"><span data-stu-id="0e09e-381">The `lock` statement is used to obtain the mutual-exclusion lock for a given object, execute a statement, and then release the lock.</span></span>

<span data-ttu-id="0e09e-382">`using` Příkaz slouží k získání prostředku, spuštění příkazu a následnému uvolnění tohoto prostředku.</span><span class="sxs-lookup"><span data-stu-id="0e09e-382">The `using` statement is used to obtain a resource, execute a statement, and then dispose of that resource.</span></span>

<span data-ttu-id="0e09e-383">Níže jsou uvedeny příklady každého typu příkazu.</span><span class="sxs-lookup"><span data-stu-id="0e09e-383">Below are examples of each kind of statement</span></span>

<span data-ttu-id="0e09e-384">__Deklarace místních proměnných__</span><span class="sxs-lookup"><span data-stu-id="0e09e-384">__Local variable declarations__</span></span>

```csharp
static void Main() {
   int a;
   int b = 2, c = 3;
   a = 1;
   Console.WriteLine(a + b + c);
}
```


<span data-ttu-id="0e09e-385">__Deklarace místní konstanty__</span><span class="sxs-lookup"><span data-stu-id="0e09e-385">__Local constant declaration__</span></span>

```csharp
static void Main() {
    const float pi = 3.1415927f;
    const int r = 25;
    Console.WriteLine(pi * r * r);
}
```


<span data-ttu-id="0e09e-386">__Příkaz výrazu__</span><span class="sxs-lookup"><span data-stu-id="0e09e-386">__Expression statement__</span></span>

```csharp
static void Main() {
    int i;
    i = 123;                // Expression statement
    Console.WriteLine(i);   // Expression statement
    i++;                    // Expression statement
    Console.WriteLine(i);   // Expression statement
}
```

<span data-ttu-id="0e09e-387">__`if`vydá__</span><span class="sxs-lookup"><span data-stu-id="0e09e-387">__`if` statement__</span></span>

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


<span data-ttu-id="0e09e-388">__`switch`vydá__</span><span class="sxs-lookup"><span data-stu-id="0e09e-388">__`switch` statement__</span></span>

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

<span data-ttu-id="0e09e-389">__`while`vydá__</span><span class="sxs-lookup"><span data-stu-id="0e09e-389">__`while` statement__</span></span>

```csharp
static void Main(string[] args) {
    int i = 0;
    while (i < args.Length) {
        Console.WriteLine(args[i]);
        i++;
    }
}
```


<span data-ttu-id="0e09e-390">__`do`vydá__</span><span class="sxs-lookup"><span data-stu-id="0e09e-390">__`do` statement__</span></span>

```csharp
static void Main() {
    string s;
    do {
        s = Console.ReadLine();
        if (s != null) Console.WriteLine(s);
    } while (s != null);
}
```

<span data-ttu-id="0e09e-391">__`for`vydá__</span><span class="sxs-lookup"><span data-stu-id="0e09e-391">__`for` statement__</span></span>

```csharp
static void Main(string[] args) {
    for (int i = 0; i < args.Length; i++) {
        Console.WriteLine(args[i]);
    }
}
```

<span data-ttu-id="0e09e-392">__`foreach`vydá__</span><span class="sxs-lookup"><span data-stu-id="0e09e-392">__`foreach` statement__</span></span>

```csharp
static void Main(string[] args) {
    foreach (string s in args) {
        Console.WriteLine(s);
    }
}
```

<span data-ttu-id="0e09e-393">__`break`vydá__</span><span class="sxs-lookup"><span data-stu-id="0e09e-393">__`break` statement__</span></span>

```csharp
static void Main() {
    while (true) {
        string s = Console.ReadLine();
        if (s == null) break;
        Console.WriteLine(s);
    }
}
```

<span data-ttu-id="0e09e-394">__`continue`vydá__</span><span class="sxs-lookup"><span data-stu-id="0e09e-394">__`continue` statement__</span></span>

```csharp
static void Main(string[] args) {
    for (int i = 0; i < args.Length; i++) {
        if (args[i].StartsWith("/")) continue;
        Console.WriteLine(args[i]);
    }
}
```

<span data-ttu-id="0e09e-395">__`goto`vydá__</span><span class="sxs-lookup"><span data-stu-id="0e09e-395">__`goto` statement__</span></span>

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

<span data-ttu-id="0e09e-396">__`return`vydá__</span><span class="sxs-lookup"><span data-stu-id="0e09e-396">__`return` statement__</span></span>

```csharp
static int Add(int a, int b) {
    return a + b;
}

static void Main() {
    Console.WriteLine(Add(1, 2));
    return;
}
```

<span data-ttu-id="0e09e-397">__`yield`vydá__</span><span class="sxs-lookup"><span data-stu-id="0e09e-397">__`yield` statement__</span></span>

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

<span data-ttu-id="0e09e-398">__`throw`příkazy `try` a__</span><span class="sxs-lookup"><span data-stu-id="0e09e-398">__`throw` and `try` statements__</span></span>

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

<span data-ttu-id="0e09e-399">__`checked`příkazy `unchecked` a__</span><span class="sxs-lookup"><span data-stu-id="0e09e-399">__`checked` and `unchecked` statements__</span></span>

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

<span data-ttu-id="0e09e-400">__`lock`vydá__</span><span class="sxs-lookup"><span data-stu-id="0e09e-400">__`lock` statement__</span></span>

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

<span data-ttu-id="0e09e-401">__`using`vydá__</span><span class="sxs-lookup"><span data-stu-id="0e09e-401">__`using` statement__</span></span>

```csharp
static void Main() {
    using (TextWriter w = File.CreateText("test.txt")) {
        w.WriteLine("Line one");
        w.WriteLine("Line two");
        w.WriteLine("Line three");
    }
}
```

## <a name="classes-and-objects"></a><span data-ttu-id="0e09e-402">Třídy a objekty</span><span class="sxs-lookup"><span data-stu-id="0e09e-402">Classes and objects</span></span>

<span data-ttu-id="0e09e-403">***Třídy*** jsou základem C#typů.</span><span class="sxs-lookup"><span data-stu-id="0e09e-403">***Classes*** are the most fundamental of C#'s types.</span></span> <span data-ttu-id="0e09e-404">Třída je datová struktura, která kombinuje stav (pole) a akce (metody a další členy funkce) v jedné jednotce.</span><span class="sxs-lookup"><span data-stu-id="0e09e-404">A class is a data structure that combines state (fields) and actions (methods and other function members) in a single unit.</span></span> <span data-ttu-id="0e09e-405">Třída poskytuje definici pro dynamicky vytvořené ***instance*** třídy, označované také jako ***objekty***.</span><span class="sxs-lookup"><span data-stu-id="0e09e-405">A class provides a definition for dynamically created ***instances*** of the class, also known as ***objects***.</span></span> <span data-ttu-id="0e09e-406">Třídy podporují ***Dědičnost*** a ***polymorfismus***, mechanismy, kterými mohou ***odvozené třídy*** roztáhnout a specializovat ***základní třídy***.</span><span class="sxs-lookup"><span data-stu-id="0e09e-406">Classes support ***inheritance*** and ***polymorphism***, mechanisms whereby ***derived classes*** can extend and specialize ***base classes***.</span></span>

<span data-ttu-id="0e09e-407">Nové třídy jsou vytvářeny pomocí deklarací třídy.</span><span class="sxs-lookup"><span data-stu-id="0e09e-407">New classes are created using class declarations.</span></span> <span data-ttu-id="0e09e-408">Deklarace třídy začíná hlavičkou, která určuje atributy a modifikátory třídy, název třídy, základní třídu (Pokud je daná) a rozhraní implementovaná třídou.</span><span class="sxs-lookup"><span data-stu-id="0e09e-408">A class declaration starts with a header that specifies the attributes and modifiers of the class, the name of the class, the base class (if given), and the interfaces implemented by the class.</span></span> <span data-ttu-id="0e09e-409">Pod hlavičkou následuje tělo třídy, které se skládá ze seznamu deklarací členů napsaných mezi oddělovači `{` a. `}`</span><span class="sxs-lookup"><span data-stu-id="0e09e-409">The header is followed by the class body, which consists of a list of member declarations written between the delimiters `{` and `}`.</span></span>

<span data-ttu-id="0e09e-410">Následuje deklarace jednoduché třídy s názvem `Point`:</span><span class="sxs-lookup"><span data-stu-id="0e09e-410">The following is a declaration of a simple class named `Point`:</span></span>

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
<span data-ttu-id="0e09e-411">Instance tříd jsou vytvořeny pomocí `new` operátoru, který přiděluje paměť pro novou instanci, vyvolá konstruktor pro inicializaci instance a vrátí odkaz na instanci.</span><span class="sxs-lookup"><span data-stu-id="0e09e-411">Instances of classes are created using the `new` operator, which allocates memory for a new instance, invokes a constructor to initialize the instance, and returns a reference to the instance.</span></span> <span data-ttu-id="0e09e-412">Následující příkazy vytvoří dva `Point` objekty a ukládají odkazy na tyto objekty ve dvou proměnných:</span><span class="sxs-lookup"><span data-stu-id="0e09e-412">The following statements create two `Point` objects and store references to those objects in two variables:</span></span>

```csharp
Point p1 = new Point(0, 0);
Point p2 = new Point(10, 20);
```
<span data-ttu-id="0e09e-413">Paměť obsazená objektem je automaticky uvolněna, pokud se objekt již nepoužívá.</span><span class="sxs-lookup"><span data-stu-id="0e09e-413">The memory occupied by an object is automatically reclaimed when the object is no longer in use.</span></span> <span data-ttu-id="0e09e-414">Není ani možné explicitně zrušit přidělení objektů v C#.</span><span class="sxs-lookup"><span data-stu-id="0e09e-414">It is neither necessary nor possible to explicitly deallocate objects in C#.</span></span>

### <a name="members"></a><span data-ttu-id="0e09e-415">Členové</span><span class="sxs-lookup"><span data-stu-id="0e09e-415">Members</span></span>

<span data-ttu-id="0e09e-416">Členy třídy jsou buď ***statické členy*** , nebo ***členy instance***.</span><span class="sxs-lookup"><span data-stu-id="0e09e-416">The members of a class are either ***static members*** or ***instance members***.</span></span> <span data-ttu-id="0e09e-417">Statické členy patří ke třídám a členy instance patří do objektů (instance tříd).</span><span class="sxs-lookup"><span data-stu-id="0e09e-417">Static members belong to classes, and instance members belong to objects (instances of classes).</span></span>

<span data-ttu-id="0e09e-418">Následující tabulka poskytuje přehled druhů členů, které třída může obsahovat.</span><span class="sxs-lookup"><span data-stu-id="0e09e-418">The following table provides an overview of the kinds of members a class can contain.</span></span>


| <span data-ttu-id="0e09e-419">__Člen__</span><span class="sxs-lookup"><span data-stu-id="0e09e-419">__Member__</span></span>   | <span data-ttu-id="0e09e-420">__Popis__</span><span class="sxs-lookup"><span data-stu-id="0e09e-420">__Description__</span></span> |
|------------  |-----------------|
| <span data-ttu-id="0e09e-421">Konstanty</span><span class="sxs-lookup"><span data-stu-id="0e09e-421">Constants</span></span>    | <span data-ttu-id="0e09e-422">Konstantní hodnoty přidružené ke třídě</span><span class="sxs-lookup"><span data-stu-id="0e09e-422">Constant values associated with the class</span></span> |
| <span data-ttu-id="0e09e-423">Pole</span><span class="sxs-lookup"><span data-stu-id="0e09e-423">Fields</span></span>       | <span data-ttu-id="0e09e-424">Proměnné třídy</span><span class="sxs-lookup"><span data-stu-id="0e09e-424">Variables of the class</span></span> |
| <span data-ttu-id="0e09e-425">Metody</span><span class="sxs-lookup"><span data-stu-id="0e09e-425">Methods</span></span>      | <span data-ttu-id="0e09e-426">Výpočty a akce, které mohou být provedeny třídou</span><span class="sxs-lookup"><span data-stu-id="0e09e-426">Computations and actions that can be performed by the class</span></span> |
| <span data-ttu-id="0e09e-427">Vlastnosti</span><span class="sxs-lookup"><span data-stu-id="0e09e-427">Properties</span></span>   | <span data-ttu-id="0e09e-428">Akce spojené s čtením a zápisem s názvem vlastnosti třídy</span><span class="sxs-lookup"><span data-stu-id="0e09e-428">Actions associated with reading and writing named properties of the class</span></span> |
| <span data-ttu-id="0e09e-429">Indexery</span><span class="sxs-lookup"><span data-stu-id="0e09e-429">Indexers</span></span>     | <span data-ttu-id="0e09e-430">Akce přidružené k indexování instancí třídy, jako je pole</span><span class="sxs-lookup"><span data-stu-id="0e09e-430">Actions associated with indexing instances of the class like an array</span></span> |
| <span data-ttu-id="0e09e-431">Události</span><span class="sxs-lookup"><span data-stu-id="0e09e-431">Events</span></span>       | <span data-ttu-id="0e09e-432">Oznámení, která mohou být vygenerována třídou</span><span class="sxs-lookup"><span data-stu-id="0e09e-432">Notifications that can be generated by the class</span></span> |
| <span data-ttu-id="0e09e-433">Operátory</span><span class="sxs-lookup"><span data-stu-id="0e09e-433">Operators</span></span>    | <span data-ttu-id="0e09e-434">Převody a operátory výrazů podporované třídou</span><span class="sxs-lookup"><span data-stu-id="0e09e-434">Conversions and expression operators supported by the class</span></span> |
| <span data-ttu-id="0e09e-435">Konstruktory</span><span class="sxs-lookup"><span data-stu-id="0e09e-435">Constructors</span></span> | <span data-ttu-id="0e09e-436">Akce vyžadované pro inicializaci instancí třídy nebo samotné třídy</span><span class="sxs-lookup"><span data-stu-id="0e09e-436">Actions required to initialize instances of the class or the class itself</span></span> |
| <span data-ttu-id="0e09e-437">Destruktory</span><span class="sxs-lookup"><span data-stu-id="0e09e-437">Destructors</span></span>  | <span data-ttu-id="0e09e-438">Akce, které se mají provést před tím, než se instance třídy trvale zahodí</span><span class="sxs-lookup"><span data-stu-id="0e09e-438">Actions to perform before instances of the class are permanently discarded</span></span> |
| <span data-ttu-id="0e09e-439">Typy</span><span class="sxs-lookup"><span data-stu-id="0e09e-439">Types</span></span>        | <span data-ttu-id="0e09e-440">Vnořené typy deklarované třídou</span><span class="sxs-lookup"><span data-stu-id="0e09e-440">Nested types declared by the class</span></span> |

### <a name="accessibility"></a><span data-ttu-id="0e09e-441">Usnadnění</span><span class="sxs-lookup"><span data-stu-id="0e09e-441">Accessibility</span></span>

<span data-ttu-id="0e09e-442">Každý člen třídy má přidruženou přístupnost, která řídí oblasti textu programu, které mají přístup k členu.</span><span class="sxs-lookup"><span data-stu-id="0e09e-442">Each member of a class has an associated accessibility, which controls the regions of program text that are able to access the member.</span></span> <span data-ttu-id="0e09e-443">Existuje pět možných forem usnadnění přístupu.</span><span class="sxs-lookup"><span data-stu-id="0e09e-443">There are five possible forms of accessibility.</span></span> <span data-ttu-id="0e09e-444">Tyto jsou shrnuté v následující tabulce.</span><span class="sxs-lookup"><span data-stu-id="0e09e-444">These are summarized in the following table.</span></span>


| <span data-ttu-id="0e09e-445">__Usnadnění__</span><span class="sxs-lookup"><span data-stu-id="0e09e-445">__Accessibility__</span></span>    | <span data-ttu-id="0e09e-446">__Smyslu__</span><span class="sxs-lookup"><span data-stu-id="0e09e-446">__Meaning__</span></span> |
|----------------------|-----------------|
| `public`             | <span data-ttu-id="0e09e-447">Přístup není omezený</span><span class="sxs-lookup"><span data-stu-id="0e09e-447">Access not limited</span></span> |
| `protected`          | <span data-ttu-id="0e09e-448">Přístup omezený na tuto třídu nebo třídy odvozené z této třídy</span><span class="sxs-lookup"><span data-stu-id="0e09e-448">Access limited to this class or classes derived from this class</span></span> |
| `internal`           | <span data-ttu-id="0e09e-449">Přístup omezený na tento program</span><span class="sxs-lookup"><span data-stu-id="0e09e-449">Access limited to this program</span></span> |
| `protected internal` | <span data-ttu-id="0e09e-450">Přístup omezený na tento program nebo třídy odvozené z této třídy</span><span class="sxs-lookup"><span data-stu-id="0e09e-450">Access limited to this program or classes derived from this class</span></span> |
| `private`            | <span data-ttu-id="0e09e-451">Přístup omezený na tuto třídu</span><span class="sxs-lookup"><span data-stu-id="0e09e-451">Access limited to this class</span></span> |

### <a name="type-parameters"></a><span data-ttu-id="0e09e-452">Parametry typu</span><span class="sxs-lookup"><span data-stu-id="0e09e-452">Type parameters</span></span>

<span data-ttu-id="0e09e-453">Definice třídy může určovat sadu parametrů typu za názvem třídy a lomenými závorkami ohraničující seznam názvů parametrů typu.</span><span class="sxs-lookup"><span data-stu-id="0e09e-453">A class definition may specify a set of type parameters by following the class name with angle brackets enclosing a list of type parameter names.</span></span> <span data-ttu-id="0e09e-454">Parametry typu lze použít v těle deklarací třídy k definování členů třídy.</span><span class="sxs-lookup"><span data-stu-id="0e09e-454">The type parameters can the be used in the body of the class declarations to define the members of the class.</span></span> <span data-ttu-id="0e09e-455">V následujícím příkladu parametry `Pair` typu jsou `TFirst` a `TSecond`:</span><span class="sxs-lookup"><span data-stu-id="0e09e-455">In the following example, the type parameters of `Pair` are `TFirst` and `TSecond`:</span></span>

```csharp
public class Pair<TFirst,TSecond>
{
    public TFirst First;
    public TSecond Second;
}
```
<span data-ttu-id="0e09e-456">Typ třídy, která je deklarována pro přijetí parametrů typu, se nazývá typ obecné třídy.</span><span class="sxs-lookup"><span data-stu-id="0e09e-456">A class type that is declared to take type parameters is called a generic class type.</span></span> <span data-ttu-id="0e09e-457">Typy struktury, rozhraní a delegátů můžou být také obecné.</span><span class="sxs-lookup"><span data-stu-id="0e09e-457">Struct, interface and delegate types can also be generic.</span></span>

<span data-ttu-id="0e09e-458">Při použití obecné třídy je nutné zadat argumenty typu pro každý z parametrů typu:</span><span class="sxs-lookup"><span data-stu-id="0e09e-458">When the generic class is used, type arguments must be provided for each of the type parameters:</span></span>

```csharp
Pair<int,string> pair = new Pair<int,string> { First = 1, Second = "two" };
int i = pair.First;     // TFirst is int
string s = pair.Second; // TSecond is string
```
<span data-ttu-id="0e09e-459">Obecný typ s poskytnutými argumenty typu, jako `Pair<int,string>` je například výše, se označuje jako konstruovaný typ.</span><span class="sxs-lookup"><span data-stu-id="0e09e-459">A generic type with type arguments provided, like `Pair<int,string>` above, is called a constructed type.</span></span>

### <a name="base-classes"></a><span data-ttu-id="0e09e-460">Základní třídy</span><span class="sxs-lookup"><span data-stu-id="0e09e-460">Base classes</span></span>

<span data-ttu-id="0e09e-461">Deklarace třídy může specifikovat základní třídu za názvem třídy a parametry typu s dvojtečkou a názvem základní třídy.</span><span class="sxs-lookup"><span data-stu-id="0e09e-461">A class declaration may specify a base class by following the class name and type parameters with a colon and the name of the base class.</span></span> <span data-ttu-id="0e09e-462">Vynechání specifikace základní třídy je stejné jako odvození z typu `object`.</span><span class="sxs-lookup"><span data-stu-id="0e09e-462">Omitting a base class specification is the same as deriving from type `object`.</span></span> <span data-ttu-id="0e09e-463">V následujícím příkladu `Point3D` je základní třída třídy `Point` `Point` a základní třída `object`:</span><span class="sxs-lookup"><span data-stu-id="0e09e-463">In the following example, the base class of `Point3D` is `Point`, and the base class of `Point` is `object`:</span></span>

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
<span data-ttu-id="0e09e-464">Třída dědí členy své základní třídy.</span><span class="sxs-lookup"><span data-stu-id="0e09e-464">A class inherits the members of its base class.</span></span> <span data-ttu-id="0e09e-465">Dědičnost znamená, že třída implicitně obsahuje všechny členy své základní třídy, s výjimkou instancí a statických konstruktorů a destruktorů základní třídy.</span><span class="sxs-lookup"><span data-stu-id="0e09e-465">Inheritance means that a class implicitly contains all members of its base class, except for the instance and static constructors, and the destructors of the base class.</span></span> <span data-ttu-id="0e09e-466">Odvozená třída může přidat nové členy do těch, které dědí, ale nemůže odebrat definici zděděného člena.</span><span class="sxs-lookup"><span data-stu-id="0e09e-466">A derived class can add new members to those it inherits, but it cannot remove the definition of an inherited member.</span></span> <span data-ttu-id="0e09e-467">V předchozím příkladu `Point3D` `x` dědí pole a `y` z `Point`a každá `Point3D` instance obsahuje tři pole, `x`, `y`a `z`.</span><span class="sxs-lookup"><span data-stu-id="0e09e-467">In the previous example, `Point3D` inherits the `x` and `y` fields from `Point`, and every `Point3D` instance contains three fields, `x`, `y`, and `z`.</span></span>

<span data-ttu-id="0e09e-468">Implicitní převod existuje z typu třídy na libovolný z jeho základních typů třídy.</span><span class="sxs-lookup"><span data-stu-id="0e09e-468">An implicit conversion exists from a class type to any of its base class types.</span></span> <span data-ttu-id="0e09e-469">Proto proměnná typu třídy může odkazovat na instanci této třídy nebo instanci jakékoli odvozené třídy.</span><span class="sxs-lookup"><span data-stu-id="0e09e-469">Therefore, a variable of a class type can reference an instance of that class or an instance of any derived class.</span></span> <span data-ttu-id="0e09e-470">Například s ohledem na předchozí deklarace třídy může proměnná typu `Point` odkazovat buď na `Point` , nebo `Point3D`:</span><span class="sxs-lookup"><span data-stu-id="0e09e-470">For example, given the previous class declarations, a variable of type `Point` can reference either a `Point` or a `Point3D`:</span></span>

```csharp
Point a = new Point(10, 20);
Point b = new Point3D(10, 20, 30);
```

### <a name="fields"></a><span data-ttu-id="0e09e-471">Pole</span><span class="sxs-lookup"><span data-stu-id="0e09e-471">Fields</span></span>

<span data-ttu-id="0e09e-472">Pole je proměnná, která je přidružena ke třídě nebo s instancí třídy.</span><span class="sxs-lookup"><span data-stu-id="0e09e-472">A field is a variable that is associated with a class or with an instance of a class.</span></span>

<span data-ttu-id="0e09e-473">Pole deklarované s `static` modifikátorem definuje ***statické pole***.</span><span class="sxs-lookup"><span data-stu-id="0e09e-473">A field declared with the `static` modifier defines a ***static field***.</span></span> <span data-ttu-id="0e09e-474">Statické pole identifikuje právě jedno umístění úložiště.</span><span class="sxs-lookup"><span data-stu-id="0e09e-474">A static field identifies exactly one storage location.</span></span> <span data-ttu-id="0e09e-475">Bez ohledu na to, kolik instancí třídy je vytvořeno, existuje pouze jedna kopie statického pole.</span><span class="sxs-lookup"><span data-stu-id="0e09e-475">No matter how many instances of a class are created, there is only ever one copy of a static field.</span></span>

<span data-ttu-id="0e09e-476">Pole deklarované bez `static` modifikátoru definuje ***pole instance***.</span><span class="sxs-lookup"><span data-stu-id="0e09e-476">A field declared without the `static` modifier defines an ***instance field***.</span></span> <span data-ttu-id="0e09e-477">Každá instance třídy obsahuje samostatnou kopii všech polí instance této třídy.</span><span class="sxs-lookup"><span data-stu-id="0e09e-477">Every instance of a class contains a separate copy of all the instance fields of that class.</span></span>

<span data-ttu-id="0e09e-478">V následujícím příkladu má `Color` každá instance třídy samostatnou kopii `r`polí `Black`instance, `g`a `b` `Red`, ale je k dispozici pouze jedna kopie, `White`,, `Green` a`Blue` statická pole:</span><span class="sxs-lookup"><span data-stu-id="0e09e-478">In the following example, each instance of the `Color` class has a separate copy of the `r`, `g`, and `b` instance fields, but there is only one copy of the `Black`, `White`, `Red`, `Green`, and `Blue` static fields:</span></span>

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
<span data-ttu-id="0e09e-479">Jak je znázorněno v předchozím příkladu, ***pole jen pro čtení*** mohou být deklarována `readonly` s modifikátorem.</span><span class="sxs-lookup"><span data-stu-id="0e09e-479">As shown in the previous example, ***read-only fields*** may be declared with a `readonly` modifier.</span></span> <span data-ttu-id="0e09e-480">Přiřazení k `readonly` poli může být provedeno pouze v rámci deklarace pole nebo v konstruktoru ve stejné třídě.</span><span class="sxs-lookup"><span data-stu-id="0e09e-480">Assignment to a `readonly` field can only occur as part of the field's declaration or in a constructor in the same class.</span></span>

### <a name="methods"></a><span data-ttu-id="0e09e-481">Metody</span><span class="sxs-lookup"><span data-stu-id="0e09e-481">Methods</span></span>

<span data-ttu-id="0e09e-482">***Metoda*** je člen, který implementuje výpočet nebo akci, kterou lze provést pomocí objektu nebo třídy.</span><span class="sxs-lookup"><span data-stu-id="0e09e-482">A ***method*** is a member that implements a computation or action that can be performed by an object or class.</span></span> <span data-ttu-id="0e09e-483">***Statické metody*** jsou k dispozici prostřednictvím třídy.</span><span class="sxs-lookup"><span data-stu-id="0e09e-483">***Static methods*** are accessed through the class.</span></span> <span data-ttu-id="0e09e-484">***Metody instance*** jsou k dispozici prostřednictvím instancí třídy.</span><span class="sxs-lookup"><span data-stu-id="0e09e-484">***Instance methods*** are accessed through instances of the class.</span></span>

<span data-ttu-id="0e09e-485">Metody mají (pravděpodobně prázdný) seznam ***parametrů***, které reprezentují hodnoty nebo odkazy na proměnné předané metodě a ***návratový typ***, který určuje typ počítané hodnoty a vrácený metodou.</span><span class="sxs-lookup"><span data-stu-id="0e09e-485">Methods have a (possibly empty) list of ***parameters***, which represent values or variable references passed to the method, and a ***return type***, which specifies the type of the value computed and returned by the method.</span></span> <span data-ttu-id="0e09e-486">Návratový typ metody je `void` , pokud nevrací hodnotu.</span><span class="sxs-lookup"><span data-stu-id="0e09e-486">A method's return type is `void` if it does not return a value.</span></span>

<span data-ttu-id="0e09e-487">Podobně jako typy mohou metody mít také sadu parametrů typu, pro které argumenty typu musí být zadány při volání metody.</span><span class="sxs-lookup"><span data-stu-id="0e09e-487">Like types, methods may also have a set of type parameters, for which type arguments must be specified when the method is called.</span></span> <span data-ttu-id="0e09e-488">Na rozdíl od typů lze argumenty typu často odvodit z argumentů volání metody a nemusí být explicitně předány.</span><span class="sxs-lookup"><span data-stu-id="0e09e-488">Unlike types, the type arguments can often be inferred from the arguments of a method call and need not be explicitly given.</span></span>

<span data-ttu-id="0e09e-489">***Signatura*** metody musí být jedinečná ve třídě, ve které je metoda deklarovaná.</span><span class="sxs-lookup"><span data-stu-id="0e09e-489">The ***signature*** of a method must be unique in the class in which the method is declared.</span></span> <span data-ttu-id="0e09e-490">Signatura metody se skládá z názvu metody, počtu parametrů typu a počtu, modifikátorů a typů jeho parametrů.</span><span class="sxs-lookup"><span data-stu-id="0e09e-490">The signature of a method consists of the name of the method, the number of type parameters and the number, modifiers, and types of its parameters.</span></span> <span data-ttu-id="0e09e-491">Podpis metody nezahrnuje návratový typ.</span><span class="sxs-lookup"><span data-stu-id="0e09e-491">The signature of a method does not include the return type.</span></span>

#### <a name="parameters"></a><span data-ttu-id="0e09e-492">Parametry</span><span class="sxs-lookup"><span data-stu-id="0e09e-492">Parameters</span></span>

<span data-ttu-id="0e09e-493">Parametry slouží k předání hodnot nebo odkazů na proměnné metody.</span><span class="sxs-lookup"><span data-stu-id="0e09e-493">Parameters are used to pass values or variable references to methods.</span></span> <span data-ttu-id="0e09e-494">Parametry metody získají jejich skutečné hodnoty z ***argumentů*** , které jsou zadány při volání metody.</span><span class="sxs-lookup"><span data-stu-id="0e09e-494">The parameters of a method get their actual values from the ***arguments*** that are specified when the method is invoked.</span></span> <span data-ttu-id="0e09e-495">Existují čtyři typy parametrů: parametry hodnoty, parametry odkazu, výstupní parametry a pole parametrů.</span><span class="sxs-lookup"><span data-stu-id="0e09e-495">There are four kinds of parameters: value parameters, reference parameters, output parameters, and parameter arrays.</span></span>

<span data-ttu-id="0e09e-496">***Parametr hodnoty*** se používá pro předávání vstupních parametrů.</span><span class="sxs-lookup"><span data-stu-id="0e09e-496">A ***value parameter*** is used for input parameter passing.</span></span> <span data-ttu-id="0e09e-497">Parametr hodnoty odpovídá místní proměnné, která vrací počáteční hodnotu z argumentu předaného pro parametr.</span><span class="sxs-lookup"><span data-stu-id="0e09e-497">A value parameter corresponds to a local variable that gets its initial value from the argument that was passed for the parameter.</span></span> <span data-ttu-id="0e09e-498">Úpravy parametru hodnoty neovlivňují argument, který byl předán parametru.</span><span class="sxs-lookup"><span data-stu-id="0e09e-498">Modifications to a value parameter do not affect the argument that was passed for the parameter.</span></span>

<span data-ttu-id="0e09e-499">Parametry hodnoty mohou být volitelné, zadáním výchozí hodnoty, aby bylo možné vynechat odpovídající argumenty.</span><span class="sxs-lookup"><span data-stu-id="0e09e-499">Value parameters can be optional, by specifying a default value so that corresponding arguments can be omitted.</span></span>

<span data-ttu-id="0e09e-500">***Parametr reference*** se používá pro předávání vstupních i výstupních parametrů.</span><span class="sxs-lookup"><span data-stu-id="0e09e-500">A ***reference parameter*** is used for both input and output parameter passing.</span></span> <span data-ttu-id="0e09e-501">Argument předaný parametru reference musí být proměnná a během provádění metody představuje parametr reference stejné umístění úložiště jako proměnná argumentu.</span><span class="sxs-lookup"><span data-stu-id="0e09e-501">The argument passed for a reference parameter must be a variable, and during execution of the method, the reference parameter represents the same storage location as the argument variable.</span></span> <span data-ttu-id="0e09e-502">Parametr reference je deklarován s `ref` modifikátorem.</span><span class="sxs-lookup"><span data-stu-id="0e09e-502">A reference parameter is declared with the `ref` modifier.</span></span> <span data-ttu-id="0e09e-503">Následující příklad ukazuje použití `ref` parametrů.</span><span class="sxs-lookup"><span data-stu-id="0e09e-503">The following example shows the use of `ref` parameters.</span></span>

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
<span data-ttu-id="0e09e-504">***Výstupní parametr*** se používá pro předávání výstupního parametru.</span><span class="sxs-lookup"><span data-stu-id="0e09e-504">An ***output parameter*** is used for output parameter passing.</span></span> <span data-ttu-id="0e09e-505">Výstupní parametr je podobný referenčnímu parametru s tím rozdílem, že počáteční hodnota argumentu zadaného volajícího je neimportovaná.</span><span class="sxs-lookup"><span data-stu-id="0e09e-505">An output parameter is similar to a reference parameter except that the initial value of the caller-provided argument is unimportant.</span></span> <span data-ttu-id="0e09e-506">Výstupní parametr je deklarován s `out` modifikátorem.</span><span class="sxs-lookup"><span data-stu-id="0e09e-506">An output parameter is declared with the `out` modifier.</span></span> <span data-ttu-id="0e09e-507">Následující příklad ukazuje použití `out` parametrů.</span><span class="sxs-lookup"><span data-stu-id="0e09e-507">The following example shows the use of `out` parameters.</span></span>

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
<span data-ttu-id="0e09e-508">***Pole parametrů*** povoluje proměnný počet argumentů, které mají být předány metodě.</span><span class="sxs-lookup"><span data-stu-id="0e09e-508">A ***parameter array*** permits a variable number of arguments to be passed to a method.</span></span> <span data-ttu-id="0e09e-509">Pole parametrů je deklarováno s `params` modifikátorem.</span><span class="sxs-lookup"><span data-stu-id="0e09e-509">A parameter array is declared with the `params` modifier.</span></span> <span data-ttu-id="0e09e-510">Pouze poslední parametr metody může být pole parametrů a typ pole parametrů musí být jednorozměrné pole.</span><span class="sxs-lookup"><span data-stu-id="0e09e-510">Only the last parameter of a method can be a parameter array, and the type of a parameter array must be a single-dimensional array type.</span></span> <span data-ttu-id="0e09e-511">Metody `Write` a`WriteLine` třídy jsou osvědčenými příklady použití pole parametrů. `System.Console`</span><span class="sxs-lookup"><span data-stu-id="0e09e-511">The `Write` and `WriteLine` methods of the `System.Console` class are good examples of parameter array usage.</span></span> <span data-ttu-id="0e09e-512">Jsou deklarovány následujícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="0e09e-512">They are declared as follows.</span></span>

```csharp
public class Console
{
    public static void Write(string fmt, params object[] args) {...}
    public static void WriteLine(string fmt, params object[] args) {...}
    ...
}
```
<span data-ttu-id="0e09e-513">V rámci metody, která používá pole parametrů, se pole parametru chová stejně jako regulární parametr typu pole.</span><span class="sxs-lookup"><span data-stu-id="0e09e-513">Within a method that uses a parameter array, the parameter array behaves exactly like a regular parameter of an array type.</span></span> <span data-ttu-id="0e09e-514">Při vyvolání metody s parametrem pole je však možné předat buď jeden argument typu pole parametru, nebo libovolný počet argumentů typu prvku pole parametrů.</span><span class="sxs-lookup"><span data-stu-id="0e09e-514">However, in an invocation of a method with a parameter array, it is possible to pass either a single argument of the parameter array type or any number of arguments of the element type of the parameter array.</span></span> <span data-ttu-id="0e09e-515">V druhém případě je instance pole automaticky vytvořena a inicializována s danými argumenty.</span><span class="sxs-lookup"><span data-stu-id="0e09e-515">In the latter case, an array instance is automatically created and initialized with the given arguments.</span></span> <span data-ttu-id="0e09e-516">Tento příklad</span><span class="sxs-lookup"><span data-stu-id="0e09e-516">This example</span></span>

```csharp
Console.WriteLine("x={0} y={1} z={2}", x, y, z);
```
<span data-ttu-id="0e09e-517">je ekvivalentem zápisu následujícího.</span><span class="sxs-lookup"><span data-stu-id="0e09e-517">is equivalent to writing the following.</span></span>

```csharp
string s = "x={0} y={1} z={2}";
object[] args = new object[3];
args[0] = x;
args[1] = y;
args[2] = z;
Console.WriteLine(s, args);
```

#### <a name="method-body-and-local-variables"></a><span data-ttu-id="0e09e-518">Tělo metody a místní proměnné</span><span class="sxs-lookup"><span data-stu-id="0e09e-518">Method body and local variables</span></span>

<span data-ttu-id="0e09e-519">Tělo metody Určuje příkazy, které mají být provedeny při volání metody.</span><span class="sxs-lookup"><span data-stu-id="0e09e-519">A method's body specifies the statements to execute when the method is invoked.</span></span>

<span data-ttu-id="0e09e-520">Tělo metody může deklarovat proměnné, které jsou specifické pro vyvolání metody.</span><span class="sxs-lookup"><span data-stu-id="0e09e-520">A method body can declare variables that are specific to the invocation of the method.</span></span> <span data-ttu-id="0e09e-521">Tyto proměnné se nazývají ***místní proměnné***.</span><span class="sxs-lookup"><span data-stu-id="0e09e-521">Such variables are called ***local variables***.</span></span> <span data-ttu-id="0e09e-522">Místní deklarace proměnné Určuje název typu, název proměnné a pravděpodobně počáteční hodnotu.</span><span class="sxs-lookup"><span data-stu-id="0e09e-522">A local variable declaration specifies a type name, a variable name, and possibly an initial value.</span></span> <span data-ttu-id="0e09e-523">Následující příklad deklaruje místní proměnnou `i` s počáteční hodnotou nula a místní proměnnou `j` bez počáteční hodnoty.</span><span class="sxs-lookup"><span data-stu-id="0e09e-523">The following example declares a local variable `i` with an initial value of zero and a local variable `j` with no initial value.</span></span>

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
<span data-ttu-id="0e09e-524">C#aby bylo možné získat hodnotu, musí být místní proměnná ***jednoznačně přiřazena*** .</span><span class="sxs-lookup"><span data-stu-id="0e09e-524">C# requires a local variable to be ***definitely assigned*** before its value can be obtained.</span></span> <span data-ttu-id="0e09e-525">Například pokud deklarace předchozí `i` nezahrnuje počáteční hodnotu, kompilátor by nahlásil chybu pro následné použití `i` , protože `i` by se v těchto bodech v programu nedala jednoznačně přiřadit.</span><span class="sxs-lookup"><span data-stu-id="0e09e-525">For example, if the declaration of the previous `i` did not include an initial value, the compiler would report an error for the subsequent usages of `i` because `i` would not be definitely assigned at those points in the program.</span></span>

<span data-ttu-id="0e09e-526">Metoda může použít `return` příkazy pro vrácení řízení volajícímu.</span><span class="sxs-lookup"><span data-stu-id="0e09e-526">A method can use `return` statements to return control to its caller.</span></span> <span data-ttu-id="0e09e-527">V metodách, `void`které `return` vracejí, příkazy nemohou určovat výraz.</span><span class="sxs-lookup"><span data-stu-id="0e09e-527">In a method returning `void`, `return` statements cannot specify an expression.</span></span> <span data-ttu-id="0e09e-528">V metodě, která vrací non`void`- `return` , příkazy musí obsahovat výraz, který vypočítá vrácenou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="0e09e-528">In a method returning non-`void`, `return` statements must include an expression that computes the return value.</span></span>

#### <a name="static-and-instance-methods"></a><span data-ttu-id="0e09e-529">Statické a instanční metody</span><span class="sxs-lookup"><span data-stu-id="0e09e-529">Static and instance methods</span></span>

<span data-ttu-id="0e09e-530">Metoda deklarovaná s `static` modifikátorem je ***statická metoda***.</span><span class="sxs-lookup"><span data-stu-id="0e09e-530">A method declared with a `static` modifier is a ***static method***.</span></span> <span data-ttu-id="0e09e-531">Statická metoda nepracuje na konkrétní instanci a může přímo přistupovat ke statickým členům.</span><span class="sxs-lookup"><span data-stu-id="0e09e-531">A static method does not operate on a specific instance and can only directly access static members.</span></span>

<span data-ttu-id="0e09e-532">Metoda deklarovaná bez `static` modifikátoru je ***Metoda instance***.</span><span class="sxs-lookup"><span data-stu-id="0e09e-532">A method declared without a `static` modifier is an ***instance method***.</span></span> <span data-ttu-id="0e09e-533">Metoda instance pracuje na konkrétní instanci a může přistupovat ke statickým i instancím členů.</span><span class="sxs-lookup"><span data-stu-id="0e09e-533">An instance method operates on a specific instance and can access both static and instance members.</span></span> <span data-ttu-id="0e09e-534">Instance, na které byla vyvolána metoda instance, může být explicitně k dispozici jako `this`.</span><span class="sxs-lookup"><span data-stu-id="0e09e-534">The instance on which an instance method was invoked can be explicitly accessed as `this`.</span></span> <span data-ttu-id="0e09e-535">V případě, že se odkazuje na `this` statickou metodu, se jedná o chybu.</span><span class="sxs-lookup"><span data-stu-id="0e09e-535">It is an error to refer to `this` in a static method.</span></span>

<span data-ttu-id="0e09e-536">Následující `Entity` třída má členy statických i instancí.</span><span class="sxs-lookup"><span data-stu-id="0e09e-536">The following `Entity` class has both static and instance members.</span></span>

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
<span data-ttu-id="0e09e-537">Každá `Entity` instance obsahuje sériové číslo (a předpokládá se, že některé další informace nejsou zde uvedeny).</span><span class="sxs-lookup"><span data-stu-id="0e09e-537">Each `Entity` instance contains a serial number (and presumably some other information that is not shown here).</span></span> <span data-ttu-id="0e09e-538">`Entity` Konstruktor (který je jako metoda instance) Inicializuje novou instanci s dalším dostupným sériovým číslem.</span><span class="sxs-lookup"><span data-stu-id="0e09e-538">The `Entity` constructor (which is like an instance method) initializes the new instance with the next available serial number.</span></span> <span data-ttu-id="0e09e-539">Vzhledem k tomu, že je konstruktor členem instance, je povolen přístup `serialNo` k poli instance i k `nextSerialNo` statickému poli.</span><span class="sxs-lookup"><span data-stu-id="0e09e-539">Because the constructor is an instance member, it is permitted to access both the `serialNo` instance field and the `nextSerialNo` static field.</span></span>

<span data-ttu-id="0e09e-540">Statické metody `SetNextSerialNo`amůžou přistupovat ke `serialNo` statickému poli, ale při přímém přístupu k poli instance by to byla chyba. `nextSerialNo` `GetNextSerialNo`</span><span class="sxs-lookup"><span data-stu-id="0e09e-540">The `GetNextSerialNo` and `SetNextSerialNo` static methods can access the `nextSerialNo` static field, but it would be an error for them to directly access the `serialNo` instance field.</span></span>

<span data-ttu-id="0e09e-541">Následující příklad ukazuje použití `Entity` třídy.</span><span class="sxs-lookup"><span data-stu-id="0e09e-541">The following example shows the use of the `Entity` class.</span></span>

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
<span data-ttu-id="0e09e-542">Všimněte si, `SetNextSerialNo` že `GetNextSerialNo` statické metody a jsou vyvolány `GetSerialNo` ve třídě, zatímco metoda instance je vyvolána na instancích třídy.</span><span class="sxs-lookup"><span data-stu-id="0e09e-542">Note that the `SetNextSerialNo` and `GetNextSerialNo` static methods are invoked on the class whereas the `GetSerialNo` instance method is invoked on instances of the class.</span></span>

#### <a name="virtual-override-and-abstract-methods"></a><span data-ttu-id="0e09e-543">Virtuální, přepisování a abstraktní metody</span><span class="sxs-lookup"><span data-stu-id="0e09e-543">Virtual, override, and abstract methods</span></span>

<span data-ttu-id="0e09e-544">Pokud deklarace metody instance obsahuje `virtual` modifikátor, metoda je označována jako ***virtuální metoda***.</span><span class="sxs-lookup"><span data-stu-id="0e09e-544">When an instance method declaration includes a `virtual` modifier, the method is said to be a ***virtual method***.</span></span> <span data-ttu-id="0e09e-545">Pokud není `virtual` k dispozici žádný modifikátor, metoda je označována jako ***nevirtuální metoda***.</span><span class="sxs-lookup"><span data-stu-id="0e09e-545">When no `virtual` modifier is present, the method is said to be a ***non-virtual method***.</span></span>

<span data-ttu-id="0e09e-546">Když je vyvolána virtuální metoda, je ***typ běhu*** instance, pro kterou probíhá vyvolání, určuje vlastní implementaci metody, která má být vyvolána.</span><span class="sxs-lookup"><span data-stu-id="0e09e-546">When a virtual method is invoked, the ***run-time type*** of the instance for which that invocation takes place determines the actual method implementation to invoke.</span></span> <span data-ttu-id="0e09e-547">V nevirtuálním volání metody je ***Typ doby kompilace*** instance určujícím faktorem.</span><span class="sxs-lookup"><span data-stu-id="0e09e-547">In a nonvirtual method invocation, the ***compile-time type*** of the instance is the determining factor.</span></span>

<span data-ttu-id="0e09e-548">Virtuální metoda může být ***přepsána*** v odvozené třídě.</span><span class="sxs-lookup"><span data-stu-id="0e09e-548">A virtual method can be ***overridden*** in a derived class.</span></span> <span data-ttu-id="0e09e-549">Když deklarace metody instance obsahuje `override` modifikátor, metoda přepíše zděděnou virtuální metodu se stejnou signaturou.</span><span class="sxs-lookup"><span data-stu-id="0e09e-549">When an instance method declaration includes an `override` modifier, the method overrides an inherited virtual method with the same signature.</span></span> <span data-ttu-id="0e09e-550">Zatímco deklarace virtuální metody zavádí novou metodu, deklarace metody přepsání specializuje existující zděděnou virtuální metodu tím, že poskytuje novou implementaci této metody.</span><span class="sxs-lookup"><span data-stu-id="0e09e-550">Whereas a virtual method declaration introduces a new method, an override method declaration specializes an existing inherited virtual method by providing a new implementation of that method.</span></span>

<span data-ttu-id="0e09e-551">***Abstraktní*** metoda je virtuální metoda bez implementace.</span><span class="sxs-lookup"><span data-stu-id="0e09e-551">An ***abstract*** method is a virtual method with no implementation.</span></span> <span data-ttu-id="0e09e-552">Abstraktní metoda je deklarována s `abstract` modifikátorem a je povolena pouze ve třídě, která je deklarována `abstract`také.</span><span class="sxs-lookup"><span data-stu-id="0e09e-552">An abstract method is declared with the `abstract` modifier and is permitted only in a class that is also declared `abstract`.</span></span> <span data-ttu-id="0e09e-553">Abstraktní metoda musí být přepsána v každé neabstraktní odvozené třídě.</span><span class="sxs-lookup"><span data-stu-id="0e09e-553">An abstract method must be overridden in every non-abstract derived class.</span></span>

<span data-ttu-id="0e09e-554">Následující příklad deklaruje abstraktní `Expression`třídu,, která představuje uzel stromu výrazu, a tři odvozené třídy, `Constant`, `VariableReference`, a `Operation`, které implementují uzly stromu výrazů pro konstanty, proměnnou odkazy a aritmetické operace.</span><span class="sxs-lookup"><span data-stu-id="0e09e-554">The following example declares an abstract class, `Expression`, which represents an expression tree node, and three derived classes, `Constant`, `VariableReference`, and `Operation`, which implement expression tree nodes for constants, variable references, and arithmetic operations.</span></span> <span data-ttu-id="0e09e-555">(To je podobné, ale nelze je zaměňovat s typy stromu výrazů představenými ve [stromových typech výrazů](types.md#expression-tree-types)).</span><span class="sxs-lookup"><span data-stu-id="0e09e-555">(This is similar to, but not to be confused with the expression tree types introduced in [Expression tree types](types.md#expression-tree-types)).</span></span>

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
<span data-ttu-id="0e09e-556">K modelování aritmetických výrazů lze použít předchozí čtyři třídy.</span><span class="sxs-lookup"><span data-stu-id="0e09e-556">The previous four classes can be used to model arithmetic expressions.</span></span> <span data-ttu-id="0e09e-557">Například pomocí instancí těchto tříd může být výraz `x + 3` reprezentován následujícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="0e09e-557">For example, using instances of these classes, the expression `x + 3` can be represented as follows.</span></span>

```csharp
Expression e = new Operation(
    new VariableReference("x"),
    '+',
    new Constant(3));
```
<span data-ttu-id="0e09e-558">Metoda instance je vyvolána pro vyhodnocení `double` daného výrazu a vytvoření hodnoty. `Expression` `Evaluate`</span><span class="sxs-lookup"><span data-stu-id="0e09e-558">The `Evaluate` method of an `Expression` instance is invoked to evaluate the given expression and produce a `double` value.</span></span> <span data-ttu-id="0e09e-559">Metoda přebírá jako argument a `Hashtable` , který obsahuje názvy proměnných (jako klíče záznamů) a hodnoty (jako hodnoty položek).</span><span class="sxs-lookup"><span data-stu-id="0e09e-559">The method takes as an argument a `Hashtable` that contains variable names (as keys of the entries) and values (as values of the entries).</span></span> <span data-ttu-id="0e09e-560">`Evaluate` Metoda je virtuální abstraktní metoda, což znamená, že neabstraktní odvozené třídy musí přepsat, aby poskytovala skutečnou implementaci.</span><span class="sxs-lookup"><span data-stu-id="0e09e-560">The `Evaluate` method is a virtual abstract method, meaning that non-abstract derived classes must override it to provide an actual implementation.</span></span>

<span data-ttu-id="0e09e-561">`Constant` Implementacejednoduševrátí`Evaluate` uloženou konstantu.</span><span class="sxs-lookup"><span data-stu-id="0e09e-561">A `Constant`'s implementation of `Evaluate` simply returns the stored constant.</span></span> <span data-ttu-id="0e09e-562">Implementace `VariableReference`objektu vyhledá název proměnné v zatřiďovací tabulce a vrátí výslednou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="0e09e-562">A `VariableReference`'s implementation looks up the variable name in the hashtable and returns the resulting value.</span></span> <span data-ttu-id="0e09e-563">Implementace nejprve vyhodnotí levý a pravý operand (rekurzivním `Evaluate` voláním metod) a poté provede danou aritmetickou operaci. `Operation`</span><span class="sxs-lookup"><span data-stu-id="0e09e-563">An `Operation`'s implementation first evaluates the left and right operands (by recursively invoking their `Evaluate` methods) and then performs the given arithmetic operation.</span></span>

<span data-ttu-id="0e09e-564">Následující program používá `Expression` třídy pro vyhodnocení výrazu `x * (y + 2)` pro různé hodnoty `x` a `y`.</span><span class="sxs-lookup"><span data-stu-id="0e09e-564">The following program uses the `Expression` classes to evaluate the expression `x * (y + 2)` for different values of `x` and `y`.</span></span>

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

#### <a name="method-overloading"></a><span data-ttu-id="0e09e-565">Přetížení metody</span><span class="sxs-lookup"><span data-stu-id="0e09e-565">Method overloading</span></span>

<span data-ttu-id="0e09e-566">***Přetížení*** metody umožňuje, aby více metod ve stejné třídě měl stejný název, pokud mají jedinečné podpisy.</span><span class="sxs-lookup"><span data-stu-id="0e09e-566">Method ***overloading*** permits multiple methods in the same class to have the same name as long as they have unique signatures.</span></span> <span data-ttu-id="0e09e-567">Při kompilování volání přetížené metody kompilátor používá ***řešení přetížení*** k určení konkrétní metody, která má být vyvolána.</span><span class="sxs-lookup"><span data-stu-id="0e09e-567">When compiling an invocation of an overloaded method, the compiler uses ***overload resolution*** to determine the specific method to invoke.</span></span> <span data-ttu-id="0e09e-568">Řešení přetížení najde jednu metodu, která nejlépe odpovídá argumentům, nebo hlásí chybu, pokud nelze najít žádnou nejlepší shodu.</span><span class="sxs-lookup"><span data-stu-id="0e09e-568">Overload resolution finds the one method that best matches the arguments or reports an error if no single best match can be found.</span></span> <span data-ttu-id="0e09e-569">Následující příklad ukazuje rozlišení přetížení v platnosti.</span><span class="sxs-lookup"><span data-stu-id="0e09e-569">The following example shows overload resolution in effect.</span></span> <span data-ttu-id="0e09e-570">Komentář pro každé vyvolání v `Main` metodě ukazuje, která metoda je skutečně vyvolána.</span><span class="sxs-lookup"><span data-stu-id="0e09e-570">The comment for each invocation in the `Main` method shows which method is actually invoked.</span></span>

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
<span data-ttu-id="0e09e-571">Jak je znázorněno v příkladu, konkrétní metodu lze vždy vybrat explicitním přetypováním argumentů na přesné typy parametrů a/nebo explicitním zadáním argumentů typu.</span><span class="sxs-lookup"><span data-stu-id="0e09e-571">As shown by the example, a particular method can always be selected by explicitly casting the arguments to the exact parameter types and/or explicitly supplying type arguments.</span></span>

### <a name="other-function-members"></a><span data-ttu-id="0e09e-572">Další členové funkcí</span><span class="sxs-lookup"><span data-stu-id="0e09e-572">Other function members</span></span>

<span data-ttu-id="0e09e-573">Členy, které obsahují spustitelný kód, jsou souhrnně označovány jako ***Členové funkce*** třídy.</span><span class="sxs-lookup"><span data-stu-id="0e09e-573">Members that contain executable code are collectively known as the ***function members*** of a class.</span></span> <span data-ttu-id="0e09e-574">Předchozí část popisuje metody, které jsou hlavním typem členů funkce.</span><span class="sxs-lookup"><span data-stu-id="0e09e-574">The preceding section describes methods, which are the primary kind of function members.</span></span> <span data-ttu-id="0e09e-575">Tato část popisuje další druhy členů funkce, které C#podporuje: konstruktory, vlastnosti, indexery, události, operátory a destruktory.</span><span class="sxs-lookup"><span data-stu-id="0e09e-575">This section describes the other kinds of function members supported by C#: constructors, properties, indexers, events, operators, and destructors.</span></span>

<span data-ttu-id="0e09e-576">Následující kód ukazuje obecnou třídu s názvem `List<T>`, která implementuje zvětšený seznam objektů.</span><span class="sxs-lookup"><span data-stu-id="0e09e-576">The following code shows a generic class called `List<T>`, which implements a growable list of objects.</span></span> <span data-ttu-id="0e09e-577">Třída obsahuje několik příkladů nejběžnějších druhů členů funkce.</span><span class="sxs-lookup"><span data-stu-id="0e09e-577">The class contains several examples of the most common kinds of function members.</span></span>


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

#### <a name="constructors"></a><span data-ttu-id="0e09e-578">Konstruktory</span><span class="sxs-lookup"><span data-stu-id="0e09e-578">Constructors</span></span>

<span data-ttu-id="0e09e-579">C#podporuje jak instance, tak statické konstruktory.</span><span class="sxs-lookup"><span data-stu-id="0e09e-579">C# supports both instance and static constructors.</span></span> <span data-ttu-id="0e09e-580">***Konstruktor instance*** je člen, který implementuje akce vyžadované pro inicializaci instance třídy.</span><span class="sxs-lookup"><span data-stu-id="0e09e-580">An ***instance constructor*** is a member that implements the actions required to initialize an instance of a class.</span></span> <span data-ttu-id="0e09e-581">***Statický konstruktor*** je člen, který implementuje akce vyžadované k inicializaci samotné třídy při prvním načtení.</span><span class="sxs-lookup"><span data-stu-id="0e09e-581">A ***static constructor*** is a member that implements the actions required to initialize a class itself when it is first loaded.</span></span>

<span data-ttu-id="0e09e-582">Konstruktor je deklarovaný jako metoda bez návratového typu a stejný název jako obsahující třída.</span><span class="sxs-lookup"><span data-stu-id="0e09e-582">A constructor is declared like a method with no return type and the same name as the containing class.</span></span> <span data-ttu-id="0e09e-583">Pokud deklarace konstruktoru obsahuje `static` modifikátor, deklaruje statický konstruktor.</span><span class="sxs-lookup"><span data-stu-id="0e09e-583">If a constructor declaration includes a `static` modifier, it declares a static constructor.</span></span> <span data-ttu-id="0e09e-584">V opačném případě deklaruje konstruktor instance.</span><span class="sxs-lookup"><span data-stu-id="0e09e-584">Otherwise, it declares an instance constructor.</span></span>

<span data-ttu-id="0e09e-585">Konstruktory instancí mohou být přetíženy.</span><span class="sxs-lookup"><span data-stu-id="0e09e-585">Instance constructors can be overloaded.</span></span> <span data-ttu-id="0e09e-586">Například `List<T>` třída deklaruje dva konstruktory instancí, jednu bez parametrů a jednu, která `int` přijímá parametr.</span><span class="sxs-lookup"><span data-stu-id="0e09e-586">For example, the `List<T>` class declares two instance constructors, one with no parameters and one that takes an `int` parameter.</span></span> <span data-ttu-id="0e09e-587">Konstruktory instancí jsou vyvolány `new` pomocí operátoru.</span><span class="sxs-lookup"><span data-stu-id="0e09e-587">Instance constructors are invoked using the `new` operator.</span></span> <span data-ttu-id="0e09e-588">Následující příkazy přidělují dvě `List<string>` instance pomocí každého konstruktoru `List` třídy.</span><span class="sxs-lookup"><span data-stu-id="0e09e-588">The following statements allocate two `List<string>` instances using each of the constructors of the `List` class.</span></span>

```csharp
List<string> list1 = new List<string>();
List<string> list2 = new List<string>(10);
```
<span data-ttu-id="0e09e-589">Na rozdíl od jiných členů nejsou konstruktory instancí zděděny a třída nemá žádné konstruktory instancí jiné než ty, které jsou ve třídě skutečně deklarovány.</span><span class="sxs-lookup"><span data-stu-id="0e09e-589">Unlike other members, instance constructors are not inherited, and a class has no instance constructors other than those actually declared in the class.</span></span> <span data-ttu-id="0e09e-590">Pokud pro třídu není zadán konstruktor instance, bude automaticky zadáno prázdné číslo bez parametrů.</span><span class="sxs-lookup"><span data-stu-id="0e09e-590">If no instance constructor is supplied for a class, then an empty one with no parameters is automatically provided.</span></span>

#### <a name="properties"></a><span data-ttu-id="0e09e-591">Vlastnosti</span><span class="sxs-lookup"><span data-stu-id="0e09e-591">Properties</span></span>

<span data-ttu-id="0e09e-592">***Vlastnosti*** jsou přirozené rozšíření polí.</span><span class="sxs-lookup"><span data-stu-id="0e09e-592">***Properties*** are a natural extension of fields.</span></span> <span data-ttu-id="0e09e-593">Oba se nazývají členové s přidruženými typy a syntaxe pro přístup k polím a vlastnostem je stejná.</span><span class="sxs-lookup"><span data-stu-id="0e09e-593">Both are named members with associated types, and the syntax for accessing fields and properties is the same.</span></span> <span data-ttu-id="0e09e-594">Na rozdíl od polí ale vlastnosti neoznačují umístění úložiště.</span><span class="sxs-lookup"><span data-stu-id="0e09e-594">However, unlike fields, properties do not denote storage locations.</span></span> <span data-ttu-id="0e09e-595">Místo toho mají vlastnosti ***přistupující objekty*** , které určují příkazy, které mají být provedeny, když jsou jejich hodnoty čteny nebo zapsány.</span><span class="sxs-lookup"><span data-stu-id="0e09e-595">Instead, properties have ***accessors*** that specify the statements to be executed when their values are read or written.</span></span>

<span data-ttu-id="0e09e-596">Vlastnost je deklarována jako pole s tím rozdílem, že deklarace končí pomocí `get` přístupového objektu nebo `set` přístupového objektu napsaného `{` mezi oddělovači `}` a místo ukončení středníkem.</span><span class="sxs-lookup"><span data-stu-id="0e09e-596">A property is declared like a field, except that the declaration ends with a `get` accessor and/or a `set` accessor written between the delimiters `{` and `}` instead of ending in a semicolon.</span></span> <span data-ttu-id="0e09e-597">`get` Vlastnost, která má přistupující objekt `set` i přistupující objekt `get` , je ***vlastnost pro čtení i zápis***, vlastnost, která má pouze přistupující objekt, je ***vlastnost jen pro čtení***a vlastnost, která má pouze `set` přistupující objekt, je. ***vlastnost pouze pro zápis***.</span><span class="sxs-lookup"><span data-stu-id="0e09e-597">A property that has both a `get` accessor and a `set` accessor is a ***read-write property***, a property that has only a `get` accessor is a ***read-only property***, and a property that has only a `set` accessor is a ***write-only property***.</span></span>

<span data-ttu-id="0e09e-598">`get` Přistupující objekt odpovídá metodě bez parametrů s návratovou hodnotou typu vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="0e09e-598">A `get` accessor corresponds to a parameterless method with a return value of the property type.</span></span> <span data-ttu-id="0e09e-599">S výjimkou cíle přiřazení, při odkazování na vlastnost ve výrazu, `get` je vyvolána přístup k vlastnosti pro výpočet hodnoty vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="0e09e-599">Except as the target of an assignment, when a property is referenced in an expression, the `get` accessor of the property is invoked to compute the value of the property.</span></span>

<span data-ttu-id="0e09e-600">Přistupující objekt odpovídá metodě s jediným parametrem s názvem `value` a bez návratového typu. `set`</span><span class="sxs-lookup"><span data-stu-id="0e09e-600">A `set` accessor corresponds to a method with a single parameter named `value` and no return type.</span></span> <span data-ttu-id="0e09e-601">Když je na vlastnost odkazováno jako na cíl přiřazení nebo jako operand `++` nebo `--`, `set` je objekt přistupující vyvolán s argumentem, který poskytuje novou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="0e09e-601">When a property is referenced as the target of an assignment or as the operand of `++` or `--`, the `set` accessor is invoked with an argument that provides the new value.</span></span>

<span data-ttu-id="0e09e-602">Třída deklaruje dvě vlastnosti `Count` `Capacity`, které jsou jen pro čtení a pro čtení i zápis, v uvedeném pořadí. `List<T>`</span><span class="sxs-lookup"><span data-stu-id="0e09e-602">The `List<T>` class declares two properties, `Count` and `Capacity`, which are read-only and read-write, respectively.</span></span> <span data-ttu-id="0e09e-603">Následuje příklad použití těchto vlastností.</span><span class="sxs-lookup"><span data-stu-id="0e09e-603">The following is an example of use of these properties.</span></span>

```csharp
List<string> names = new List<string>();
names.Capacity = 100;            // Invokes set accessor
int i = names.Count;             // Invokes get accessor
int j = names.Capacity;          // Invokes get accessor
```
<span data-ttu-id="0e09e-604">Podobně jako pole a metody C# podporuje vlastnosti instance i statické vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="0e09e-604">Similar to fields and methods, C# supports both instance properties and static properties.</span></span> <span data-ttu-id="0e09e-605">Statické vlastnosti jsou deklarovány s `static` modifikátorem a vlastnosti instance jsou deklarovány bez ní.</span><span class="sxs-lookup"><span data-stu-id="0e09e-605">Static properties are declared with the `static` modifier, and instance properties are declared without it.</span></span>

<span data-ttu-id="0e09e-606">Přístupové objekty vlastnosti mohou být virtuální.</span><span class="sxs-lookup"><span data-stu-id="0e09e-606">The accessor(s) of a property can be virtual.</span></span> <span data-ttu-id="0e09e-607">Pokud deklarace vlastnosti obsahuje `virtual`modifikátor, `abstract`nebo `override` , vztahuje se na přístupový objekt (y) vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="0e09e-607">When a property declaration includes a `virtual`, `abstract`, or `override` modifier, it applies to the accessor(s) of the property.</span></span>

#### <a name="indexers"></a><span data-ttu-id="0e09e-608">Indexery</span><span class="sxs-lookup"><span data-stu-id="0e09e-608">Indexers</span></span>

<span data-ttu-id="0e09e-609">***Indexer*** je člen, který umožňuje, aby objekty byly indexovány stejným způsobem jako pole.</span><span class="sxs-lookup"><span data-stu-id="0e09e-609">An ***indexer*** is a member that enables objects to be indexed in the same way as an array.</span></span> <span data-ttu-id="0e09e-610">Indexer je deklarován jako vlastnost s tím rozdílem, že název člena je `this` následován seznamem parametrů napsaným mezi `[` oddělovači a `]`.</span><span class="sxs-lookup"><span data-stu-id="0e09e-610">An indexer is declared like a property except that the name of the member is `this` followed by a parameter list written between the delimiters `[` and `]`.</span></span> <span data-ttu-id="0e09e-611">Parametry jsou k dispozici v přistupujícím objektu indexeru.</span><span class="sxs-lookup"><span data-stu-id="0e09e-611">The parameters are available in the accessor(s) of the indexer.</span></span> <span data-ttu-id="0e09e-612">Podobně jako u vlastností mohou být indexery pro čtení i zápis, jen pro čtení a přístup k nástroji indexeru, které mohou být virtuální.</span><span class="sxs-lookup"><span data-stu-id="0e09e-612">Similar to properties, indexers can be read-write, read-only, and write-only, and the accessor(s) of an indexer can be virtual.</span></span>

<span data-ttu-id="0e09e-613">Třída deklaruje jeden indexer pro čtení a zápis, který `int` přebírá parametr. `List`</span><span class="sxs-lookup"><span data-stu-id="0e09e-613">The `List` class declares a single read-write indexer that takes an `int` parameter.</span></span> <span data-ttu-id="0e09e-614">Indexer umožňuje indexovat `List` instance s `int` hodnotami.</span><span class="sxs-lookup"><span data-stu-id="0e09e-614">The indexer makes it possible to index `List` instances with `int` values.</span></span> <span data-ttu-id="0e09e-615">Příklad</span><span class="sxs-lookup"><span data-stu-id="0e09e-615">For example</span></span>

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
<span data-ttu-id="0e09e-616">Indexery mohou být přetíženy, což znamená, že třída může deklarovat více indexerů za předpokladu, že počet nebo typy jejich parametrů se liší.</span><span class="sxs-lookup"><span data-stu-id="0e09e-616">Indexers can be overloaded, meaning that a class can declare multiple indexers as long as the number or types of their parameters differ.</span></span>

#### <a name="events"></a><span data-ttu-id="0e09e-617">Události</span><span class="sxs-lookup"><span data-stu-id="0e09e-617">Events</span></span>

<span data-ttu-id="0e09e-618">***Událost*** je člen, který umožňuje třídě nebo objektu poskytovat oznámení.</span><span class="sxs-lookup"><span data-stu-id="0e09e-618">An ***event*** is a member that enables a class or object to provide notifications.</span></span> <span data-ttu-id="0e09e-619">Událost je deklarována jako pole s tím rozdílem, že deklarace `event` zahrnuje klíčové slovo a typ musí být delegovaný typ.</span><span class="sxs-lookup"><span data-stu-id="0e09e-619">An event is declared like a field except that the declaration includes an `event` keyword and the type must be a delegate type.</span></span>

<span data-ttu-id="0e09e-620">V rámci třídy, která deklaruje člena události, se událost chová stejně jako pole typu delegáta (za předpokladu, že událost není abstraktní a nedeklaruje přistupující objekty).</span><span class="sxs-lookup"><span data-stu-id="0e09e-620">Within a class that declares an event member, the event behaves just like a field of a delegate type (provided the event is not abstract and does not declare accessors).</span></span> <span data-ttu-id="0e09e-621">Pole ukládá odkaz na delegáta, který představuje obslužné rutiny událostí, které byly přidány do události.</span><span class="sxs-lookup"><span data-stu-id="0e09e-621">The field stores a reference to a delegate that represents the event handlers that have been added to the event.</span></span> <span data-ttu-id="0e09e-622">Pokud nejsou k dispozici žádné obslužné rutiny událostí `null`, pole je.</span><span class="sxs-lookup"><span data-stu-id="0e09e-622">If no event handles are present, the field is `null`.</span></span>

<span data-ttu-id="0e09e-623">Třída deklaruje jeden člen události s názvem `Changed`, který indikuje, že do seznamu byla přidána nová položka. `List<T>`</span><span class="sxs-lookup"><span data-stu-id="0e09e-623">The `List<T>` class declares a single event member called `Changed`, which indicates that a new item has been added to the list.</span></span> <span data-ttu-id="0e09e-624">Událost je vyvolána `OnChanged` virtuální metodou, která nejprve kontroluje, zda je `null` událost (to znamená, že nejsou přítomny žádné obslužné rutiny). `Changed`</span><span class="sxs-lookup"><span data-stu-id="0e09e-624">The `Changed` event is raised by the `OnChanged` virtual method, which first checks whether the event is `null` (meaning that no handlers are present).</span></span> <span data-ttu-id="0e09e-625">Pojem vyvolání události je přesně shodný s voláním delegáta reprezentovaného událostí, takže neexistují žádné speciální jazykové konstrukce pro vyvolání událostí.</span><span class="sxs-lookup"><span data-stu-id="0e09e-625">The notion of raising an event is precisely equivalent to invoking the delegate represented by the event—thus, there are no special language constructs for raising events.</span></span>

<span data-ttu-id="0e09e-626">Klienti reagují na události prostřednictvím ***obslužných rutin událostí***.</span><span class="sxs-lookup"><span data-stu-id="0e09e-626">Clients react to events through ***event handlers***.</span></span> <span data-ttu-id="0e09e-627">Obslužné rutiny událostí jsou připojeny `+=` pomocí operátoru a odebrány `-=` pomocí operátoru.</span><span class="sxs-lookup"><span data-stu-id="0e09e-627">Event handlers are attached using the `+=` operator and removed using the `-=` operator.</span></span> <span data-ttu-id="0e09e-628">Následující příklad připojí obslužnou rutinu události k `Changed` události. `List<string>`</span><span class="sxs-lookup"><span data-stu-id="0e09e-628">The following example attaches an event handler to the `Changed` event of a `List<string>`.</span></span>

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
<span data-ttu-id="0e09e-629">Pro pokročilé scénáře, kde je požadováno řízení základního úložiště události, může deklarace události explicitně poskytnout `add` a `remove` přistupující objekty, které `set` jsou poněkud podobné přístupovému objektu vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="0e09e-629">For advanced scenarios where control of the underlying storage of an event is desired, an event declaration can explicitly provide `add` and `remove` accessors, which are somewhat similar to the `set` accessor of a property.</span></span>

#### <a name="operators"></a><span data-ttu-id="0e09e-630">Operátory</span><span class="sxs-lookup"><span data-stu-id="0e09e-630">Operators</span></span>

<span data-ttu-id="0e09e-631">***Operátor*** je člen, který definuje význam použití konkrétního operátoru výrazu na instance třídy.</span><span class="sxs-lookup"><span data-stu-id="0e09e-631">An ***operator*** is a member that defines the meaning of applying a particular expression operator to instances of a class.</span></span> <span data-ttu-id="0e09e-632">Lze definovat tři typy operátorů: unární operátory, binární operátory a operátory převodu.</span><span class="sxs-lookup"><span data-stu-id="0e09e-632">Three kinds of operators can be defined: unary operators, binary operators, and conversion operators.</span></span> <span data-ttu-id="0e09e-633">Všechny operátory musí být deklarovány `public` jako `static`a.</span><span class="sxs-lookup"><span data-stu-id="0e09e-633">All operators must be declared as `public` and `static`.</span></span>

<span data-ttu-id="0e09e-634">Třída deklaruje dva operátory `operator==` `List` , a, a poskytuje tak nový význam pro výrazy, které tyto operátory aplikují na instance. `operator!=` `List<T>`</span><span class="sxs-lookup"><span data-stu-id="0e09e-634">The `List<T>` class declares two operators, `operator==` and `operator!=`, and thus gives new meaning to expressions that apply those operators to `List` instances.</span></span> <span data-ttu-id="0e09e-635">Konkrétně operátory definují rovnost dvou `List<T>` instancí jako porovnávání každého z obsažených objektů pomocí jejich `Equals` metod.</span><span class="sxs-lookup"><span data-stu-id="0e09e-635">Specifically, the operators define equality of two `List<T>` instances as comparing each of the contained objects using their `Equals` methods.</span></span> <span data-ttu-id="0e09e-636">Následující příklad používá `==` operátor k porovnání dvou `List<int>` instancí.</span><span class="sxs-lookup"><span data-stu-id="0e09e-636">The following example uses the `==` operator to compare two `List<int>` instances.</span></span>

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

<span data-ttu-id="0e09e-637">První `Console.WriteLine` výstupy `True` , protože dva seznamy obsahují stejný počet objektů se stejnými hodnotami ve stejném pořadí.</span><span class="sxs-lookup"><span data-stu-id="0e09e-637">The first `Console.WriteLine` outputs `True` because the two lists contain the same number of objects with the same values in the same order.</span></span> <span data-ttu-id="0e09e-638">`a` `List<int>` Nebyl definován `operator==`, `False` první `Console.WriteLine` by měl výstup, protože a`b` odkazují na různé instance. `List<T>`</span><span class="sxs-lookup"><span data-stu-id="0e09e-638">Had `List<T>` not defined `operator==`, the first `Console.WriteLine` would have output `False` because `a` and `b` reference different `List<int>` instances.</span></span>

#### <a name="destructors"></a><span data-ttu-id="0e09e-639">Destruktory</span><span class="sxs-lookup"><span data-stu-id="0e09e-639">Destructors</span></span>

<span data-ttu-id="0e09e-640">***Destruktor*** je člen, který implementuje akce vyžadované k destrukcií instance třídy.</span><span class="sxs-lookup"><span data-stu-id="0e09e-640">A ***destructor*** is a member that implements the actions required to destruct an instance of a class.</span></span> <span data-ttu-id="0e09e-641">Destruktory nemohou mít parametry, nemohou mít modifikátory dostupnosti a nelze je volat explicitně.</span><span class="sxs-lookup"><span data-stu-id="0e09e-641">Destructors cannot have parameters, they cannot have accessibility modifiers, and they cannot be invoked explicitly.</span></span> <span data-ttu-id="0e09e-642">Destruktor instance je vyvolán automaticky během uvolňování paměti.</span><span class="sxs-lookup"><span data-stu-id="0e09e-642">The destructor for an instance is invoked automatically during garbage collection.</span></span>

<span data-ttu-id="0e09e-643">Uvolňování paměti je povolená rozsáhlá Zeměpisná šířka při rozhodování o shromažďování objektů a spouštění destruktorů.</span><span class="sxs-lookup"><span data-stu-id="0e09e-643">The garbage collector is allowed wide latitude in deciding when to collect objects and run destructors.</span></span> <span data-ttu-id="0e09e-644">Konkrétně načasování volání destruktoru není deterministické a v jakémkoli vlákně mohou být provedeny destruktory.</span><span class="sxs-lookup"><span data-stu-id="0e09e-644">Specifically, the timing of destructor invocations is not deterministic, and destructors may be executed on any thread.</span></span> <span data-ttu-id="0e09e-645">Z těchto a dalších důvodů by třídy měly implementovat destruktory pouze v případě, že žádná jiná řešení nejsou proveditelná.</span><span class="sxs-lookup"><span data-stu-id="0e09e-645">For these and other reasons, classes should implement destructors only when no other solutions are feasible.</span></span>

<span data-ttu-id="0e09e-646">`using` Příkaz poskytuje lepší přístup k zničení objektu.</span><span class="sxs-lookup"><span data-stu-id="0e09e-646">The `using` statement provides a better approach to object destruction.</span></span>

## <a name="structs"></a><span data-ttu-id="0e09e-647">Struktury</span><span class="sxs-lookup"><span data-stu-id="0e09e-647">Structs</span></span>

<span data-ttu-id="0e09e-648">Podobně jako třídy ***struktury*** jsou datové struktury, které mohou obsahovat datové členy a členy funkce, ale na rozdíl od tříd, struktury jsou typy hodnot a nevyžadují přidělení haldy.</span><span class="sxs-lookup"><span data-stu-id="0e09e-648">Like classes, ***structs*** are data structures that can contain data members and function members, but unlike classes, structs are value types and do not require heap allocation.</span></span> <span data-ttu-id="0e09e-649">Proměnná typu struktury přímo ukládá data struktury, zatímco proměnná typu třídy ukládá odkaz na dynamicky přidělený objekt.</span><span class="sxs-lookup"><span data-stu-id="0e09e-649">A variable of a struct type directly stores the data of the struct, whereas a variable of a class type stores a reference to a dynamically allocated object.</span></span> <span data-ttu-id="0e09e-650">Typy struktury nepodporují uživatelem zadanou dědičnost a všechny typy struktury implicitně dědí z typu `object`.</span><span class="sxs-lookup"><span data-stu-id="0e09e-650">Struct types do not support user-specified inheritance, and all struct types implicitly inherit from type `object`.</span></span>

<span data-ttu-id="0e09e-651">Struktury jsou zvláště užitečné pro malé datové struktury, které mají sémantiku hodnot.</span><span class="sxs-lookup"><span data-stu-id="0e09e-651">Structs are particularly useful for small data structures that have value semantics.</span></span> <span data-ttu-id="0e09e-652">Komplexní čísla, body v systému souřadnic nebo páry klíč-hodnota ve slovníku jsou všechny dobrými příklady struktur.</span><span class="sxs-lookup"><span data-stu-id="0e09e-652">Complex numbers, points in a coordinate system, or key-value pairs in a dictionary are all good examples of structs.</span></span> <span data-ttu-id="0e09e-653">Použití struktur namísto tříd pro malé datové struktury může mít velký rozdíl v počtu přidělení paměti, které aplikace provádí.</span><span class="sxs-lookup"><span data-stu-id="0e09e-653">The use of structs rather than classes for small data structures can make a large difference in the number of memory allocations an application performs.</span></span> <span data-ttu-id="0e09e-654">Například následující program vytvoří a inicializuje pole 100 bodů.</span><span class="sxs-lookup"><span data-stu-id="0e09e-654">For example, the following program creates and initializes an array of 100 points.</span></span> <span data-ttu-id="0e09e-655">V `Point` případě 101, že jsou implementovány jako třídy, jsou vytvořeny instance samostatných objektů – jeden pro pole a jeden pro prvky 100.</span><span class="sxs-lookup"><span data-stu-id="0e09e-655">With `Point` implemented as a class, 101 separate objects are instantiated—one for the array and one each for the 100 elements.</span></span>

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
<span data-ttu-id="0e09e-656">Alternativou je vytvoření `Point` struktury.</span><span class="sxs-lookup"><span data-stu-id="0e09e-656">An alternative is to make `Point` a struct.</span></span>

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
<span data-ttu-id="0e09e-657">Nyní je vytvořena instance pouze jednoho objektu – jeden pro pole – a `Point` instance jsou uloženy v poli do pole.</span><span class="sxs-lookup"><span data-stu-id="0e09e-657">Now, only one object is instantiated—the one for the array—and the `Point` instances are stored in-line in the array.</span></span>

<span data-ttu-id="0e09e-658">Konstruktory struktury jsou vyvolány `new` pomocí operátoru, ale to neznamená, že se paměť přiděluje.</span><span class="sxs-lookup"><span data-stu-id="0e09e-658">Struct constructors are invoked with the `new` operator, but that does not imply that memory is being allocated.</span></span> <span data-ttu-id="0e09e-659">Namísto dynamického přidělování objektu a vrácení odkazu na něj konstruktor struktury jednoduše vrátí hodnotu samotné struktury (obvykle v dočasném umístění v zásobníku) a tato hodnota se pak podle potřeby zkopíruje.</span><span class="sxs-lookup"><span data-stu-id="0e09e-659">Instead of dynamically allocating an object and returning a reference to it, a struct constructor simply returns the struct value itself (typically in a temporary location on the stack), and this value is then copied as necessary.</span></span>

<span data-ttu-id="0e09e-660">U tříd je možné, aby dvě proměnné odkazovaly na stejný objekt a bylo tak možné, aby operace na jedné proměnné ovlivnily objekt, na který je odkazováno jinou proměnnou.</span><span class="sxs-lookup"><span data-stu-id="0e09e-660">With classes, it is possible for two variables to reference the same object and thus possible for operations on one variable to affect the object referenced by the other variable.</span></span> <span data-ttu-id="0e09e-661">Pomocí struktur mají proměnné, které mají svou vlastní kopii dat, a není možné, aby operace na jednom z nich ovlivnily druhou.</span><span class="sxs-lookup"><span data-stu-id="0e09e-661">With structs, the variables each have their own copy of the data, and it is not possible for operations on one to affect the other.</span></span> <span data-ttu-id="0e09e-662">Například výstup vyprodukovaný následujícím fragmentem kódu závisí na tom, zda `Point` je třída nebo struktura.</span><span class="sxs-lookup"><span data-stu-id="0e09e-662">For example, the output produced by the following code fragment depends on whether `Point` is a class or a struct.</span></span>

```csharp
Point a = new Point(10, 10);
Point b = a;
a.x = 20;
Console.WriteLine(b.x);
```
<span data-ttu-id="0e09e-663">Pokud `Point` je třída, výstup je `20` , protože `a` a `b` odkazuje na stejný objekt.</span><span class="sxs-lookup"><span data-stu-id="0e09e-663">If `Point` is a class, the output is `20` because `a` and `b` reference the same object.</span></span> <span data-ttu-id="0e09e-664">Pokud `Point` je struktura, výstup je `10` , protože přiřazení `a` pro `b` vytvoří kopii hodnoty a tato kopie není ovlivněna následným přiřazením k `a.x`.</span><span class="sxs-lookup"><span data-stu-id="0e09e-664">If `Point` is a struct, the output is `10` because the assignment of `a` to `b` creates a copy of the value, and this copy is unaffected by the subsequent assignment to `a.x`.</span></span>

<span data-ttu-id="0e09e-665">Předchozí příklad zvýrazní dvě omezení struktury.</span><span class="sxs-lookup"><span data-stu-id="0e09e-665">The previous example highlights two of the limitations of structs.</span></span> <span data-ttu-id="0e09e-666">Nejprve kopírování celé struktury je obvykle méně efektivní než kopírování odkazu na objekt, takže předávání parametrů hodnot a parametrů může být dražší s strukturami, než s typem odkazu.</span><span class="sxs-lookup"><span data-stu-id="0e09e-666">First, copying an entire struct is typically less efficient than copying an object reference, so assignment and value parameter passing can be more expensive with structs than with reference types.</span></span> <span data-ttu-id="0e09e-667">Sekunda s `ref` výjimkou `out` parametrů a není možné vytvořit odkazy na struktury, které pravidla jejich používání vymění v řadě situací.</span><span class="sxs-lookup"><span data-stu-id="0e09e-667">Second, except for `ref` and `out` parameters, it is not possible to create references to structs, which rules out their usage in a number of situations.</span></span>

## <a name="arrays"></a><span data-ttu-id="0e09e-668">Pole</span><span class="sxs-lookup"><span data-stu-id="0e09e-668">Arrays</span></span>

<span data-ttu-id="0e09e-669">***Pole*** je datová struktura, která obsahuje počet proměnných, které jsou dostupné prostřednictvím počítaných indexů.</span><span class="sxs-lookup"><span data-stu-id="0e09e-669">An ***array*** is a data structure that contains a number of variables that are accessed through computed indices.</span></span> <span data-ttu-id="0e09e-670">Proměnné obsažené v poli, označované také jako ***prvky*** pole, jsou všechny stejného typu a tento typ se nazývá ***typ elementu*** pole.</span><span class="sxs-lookup"><span data-stu-id="0e09e-670">The variables contained in an array, also called the ***elements*** of the array, are all of the same type, and this type is called the ***element type*** of the array.</span></span>

<span data-ttu-id="0e09e-671">Typy polí jsou odkazové typy a deklarace proměnné pole jednoduše nastaví volné místo pro odkaz na instanci pole.</span><span class="sxs-lookup"><span data-stu-id="0e09e-671">Array types are reference types, and the declaration of an array variable simply sets aside space for a reference to an array instance.</span></span> <span data-ttu-id="0e09e-672">Skutečné instance pole jsou vytvářeny dynamicky za běhu pomocí `new` operátoru.</span><span class="sxs-lookup"><span data-stu-id="0e09e-672">Actual array instances are created dynamically at run-time using the `new` operator.</span></span> <span data-ttu-id="0e09e-673">Operace určuje délku nové instance pole, která je poté opravena po dobu života instance. `new`</span><span class="sxs-lookup"><span data-stu-id="0e09e-673">The `new` operation specifies the ***length*** of the new array instance, which is then fixed for the lifetime of the instance.</span></span> <span data-ttu-id="0e09e-674">Indexy prvků rozsahu pole od `0` do. `Length - 1`</span><span class="sxs-lookup"><span data-stu-id="0e09e-674">The indices of the elements of an array range from `0` to `Length - 1`.</span></span> <span data-ttu-id="0e09e-675">Operátor automaticky inicializuje prvky pole na výchozí hodnotu, což je například nula pro všechny číselné typy a `null` pro všechny typy odkazů. `new`</span><span class="sxs-lookup"><span data-stu-id="0e09e-675">The `new` operator automatically initializes the elements of an array to their default value, which, for example, is zero for all numeric types and `null` for all reference types.</span></span>

<span data-ttu-id="0e09e-676">Následující příklad vytvoří pole `int` prvků, inicializuje pole a vytiskne obsah pole.</span><span class="sxs-lookup"><span data-stu-id="0e09e-676">The following example creates an array of `int` elements, initializes the array, and prints out the contents of the array.</span></span>

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
<span data-ttu-id="0e09e-677">Tento příklad vytvoří a pracuje na jednorozměrném ***poli***.</span><span class="sxs-lookup"><span data-stu-id="0e09e-677">This example creates and operates on a ***single-dimensional array***.</span></span> <span data-ttu-id="0e09e-678">C#podporuje rovněž ***multidimenzionální pole***.</span><span class="sxs-lookup"><span data-stu-id="0e09e-678">C# also supports ***multi-dimensional arrays***.</span></span> <span data-ttu-id="0e09e-679">Počet rozměrů typu pole, označovaný také jako ***rozměr*** typu pole, je jedna plus počet čárek napsaných mezi hranatými závorkami typu pole.</span><span class="sxs-lookup"><span data-stu-id="0e09e-679">The number of dimensions of an array type, also known as the ***rank*** of the array type, is one plus the number of commas written between the square brackets of the array type.</span></span> <span data-ttu-id="0e09e-680">Následující příklad přiděluje jednorozměrné, dvourozměrné a trojrozměrné pole.</span><span class="sxs-lookup"><span data-stu-id="0e09e-680">The following example allocates a one-dimensional, a two-dimensional, and a three-dimensional array.</span></span>

```csharp
int[] a1 = new int[10];
int[,] a2 = new int[10, 5];
int[,,] a3 = new int[10, 5, 2];
```
<span data-ttu-id="0e09e-681">Pole obsahuje 10 prvků `a2` , pole obsahuje 50 (10 × 5 `a3` ) prvků a pole obsahuje 100 (10 × 5 × 2) prvků. `a1`</span><span class="sxs-lookup"><span data-stu-id="0e09e-681">The `a1` array contains 10 elements, the `a2` array contains 50 (10 × 5) elements, and the `a3` array contains 100 (10 × 5 × 2) elements.</span></span>

<span data-ttu-id="0e09e-682">Typ elementu pole může být libovolný typ, včetně typu pole.</span><span class="sxs-lookup"><span data-stu-id="0e09e-682">The element type of an array can be any type, including an array type.</span></span> <span data-ttu-id="0e09e-683">Pole s prvky typu pole se někdy nazývá ***vícenásobné pole*** , protože délky polí elementů nemusí být stejné.</span><span class="sxs-lookup"><span data-stu-id="0e09e-683">An array with elements of an array type is sometimes called a ***jagged array*** because the lengths of the element arrays do not all have to be the same.</span></span> <span data-ttu-id="0e09e-684">Následující příklad přiděluje pole polí pro `int`:</span><span class="sxs-lookup"><span data-stu-id="0e09e-684">The following example allocates an array of arrays of `int`:</span></span>

```csharp
int[][] a = new int[3][];
a[0] = new int[10];
a[1] = new int[5];
a[2] = new int[20];
```
<span data-ttu-id="0e09e-685">První řádek vytvoří pole se třemi prvky, každý typ `int[]` a každý s počáteční `null`hodnotou.</span><span class="sxs-lookup"><span data-stu-id="0e09e-685">The first line creates an array with three elements, each of type `int[]` and each with an initial value of `null`.</span></span> <span data-ttu-id="0e09e-686">Následující řádky poté inicializují tři prvky s odkazy na jednotlivé instance pole s různou délkou.</span><span class="sxs-lookup"><span data-stu-id="0e09e-686">The subsequent lines then initialize the three elements with references to individual array instances of varying lengths.</span></span>

<span data-ttu-id="0e09e-687">Operátor umožňuje zadat počáteční hodnoty prvků pole pomocí ***inicializátoru pole***, což je seznam výrazů zapsaných mezi oddělovači `{` a. `}` `new`</span><span class="sxs-lookup"><span data-stu-id="0e09e-687">The `new` operator permits the initial values of the array elements to be specified using an ***array initializer***, which is a list of expressions written between the delimiters `{` and `}`.</span></span> <span data-ttu-id="0e09e-688">Následující příklad přiděluje a inicializuje `int[]` se třemi prvky.</span><span class="sxs-lookup"><span data-stu-id="0e09e-688">The following example allocates and initializes an `int[]` with three elements.</span></span>

```csharp
int[] a = new int[] {1, 2, 3};
```
<span data-ttu-id="0e09e-689">Všimněte si, že délka pole je odvozena od počtu výrazů mezi `{` a. `}`</span><span class="sxs-lookup"><span data-stu-id="0e09e-689">Note that the length of the array is inferred from the number of expressions between `{` and `}`.</span></span> <span data-ttu-id="0e09e-690">Deklarace lokální proměnné a pole lze dále zkrátit tak, aby se typ pole nemuselo přestavit.</span><span class="sxs-lookup"><span data-stu-id="0e09e-690">Local variable and field declarations can be shortened further such that the array type does not have to be restated.</span></span>

```csharp
int[] a = {1, 2, 3};
```
<span data-ttu-id="0e09e-691">Oba předchozí příklady jsou ekvivalentní následujícímu:</span><span class="sxs-lookup"><span data-stu-id="0e09e-691">Both of the previous examples are equivalent to the following:</span></span>

```csharp
int[] t = new int[3];
t[0] = 1;
t[1] = 2;
t[2] = 3;
int[] a = t;
```
## <a name="interfaces"></a><span data-ttu-id="0e09e-692">Rozhraní</span><span class="sxs-lookup"><span data-stu-id="0e09e-692">Interfaces</span></span>

<span data-ttu-id="0e09e-693">***Rozhraní*** definuje kontrakt, který může být implementován pomocí tříd a struktur.</span><span class="sxs-lookup"><span data-stu-id="0e09e-693">An ***interface*** defines a contract that can be implemented by classes and structs.</span></span> <span data-ttu-id="0e09e-694">Rozhraní může obsahovat metody, vlastnosti, události a indexery.</span><span class="sxs-lookup"><span data-stu-id="0e09e-694">An interface can contain methods, properties, events, and indexers.</span></span> <span data-ttu-id="0e09e-695">Rozhraní neposkytuje implementace členů, které definuje – určuje pouze členy, které musí být poskytnuty třídami nebo strukturami, které implementují rozhraní.</span><span class="sxs-lookup"><span data-stu-id="0e09e-695">An interface does not provide implementations of the members it defines—it merely specifies the members that must be supplied by classes or structs that implement the interface.</span></span>

<span data-ttu-id="0e09e-696">Rozhraní mohou využívat ***vícenásobnou dědičnost***.</span><span class="sxs-lookup"><span data-stu-id="0e09e-696">Interfaces may employ ***multiple inheritance***.</span></span> <span data-ttu-id="0e09e-697">V následujícím příkladu rozhraní `IComboBox` dědí `ITextBox` z a `IListBox`.</span><span class="sxs-lookup"><span data-stu-id="0e09e-697">In the following example, the interface `IComboBox` inherits from both `ITextBox` and `IListBox`.</span></span>

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
<span data-ttu-id="0e09e-698">Třídy a struktury mohou implementovat více rozhraní.</span><span class="sxs-lookup"><span data-stu-id="0e09e-698">Classes and structs can implement multiple interfaces.</span></span> <span data-ttu-id="0e09e-699">V následujícím příkladu třída `EditBox` implementuje `IDataBound`i `IControl` .</span><span class="sxs-lookup"><span data-stu-id="0e09e-699">In the following example, the class `EditBox` implements both `IControl` and `IDataBound`.</span></span>

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
<span data-ttu-id="0e09e-700">Pokud třída nebo struktura implementuje konkrétní rozhraní, instance této třídy nebo struktury lze implicitně převést na tento typ rozhraní.</span><span class="sxs-lookup"><span data-stu-id="0e09e-700">When a class or struct implements a particular interface, instances of that class or struct can be implicitly converted to that interface type.</span></span> <span data-ttu-id="0e09e-701">Příklad</span><span class="sxs-lookup"><span data-stu-id="0e09e-701">For example</span></span>

```csharp
EditBox editBox = new EditBox();
IControl control = editBox;
IDataBound dataBound = editBox;
```
<span data-ttu-id="0e09e-702">V případech, kdy instance není staticky známá pro implementaci konkrétního rozhraní, lze použít dynamické přetypování typu.</span><span class="sxs-lookup"><span data-stu-id="0e09e-702">In cases where an instance is not statically known to implement a particular interface, dynamic type casts can be used.</span></span> <span data-ttu-id="0e09e-703">Například následující příkazy používají přetypování dynamického typu k získání implementace objektu `IControl` a `IDataBound` rozhraní.</span><span class="sxs-lookup"><span data-stu-id="0e09e-703">For example, the following statements use dynamic type casts to obtain an object's `IControl` and `IDataBound` interface implementations.</span></span> <span data-ttu-id="0e09e-704">Vzhledem k tomu, že skutečný typ objektu `EditBox`je, přetypování je úspěšné.</span><span class="sxs-lookup"><span data-stu-id="0e09e-704">Because the actual type of the object is `EditBox`, the casts succeed.</span></span>

```csharp
object obj = new EditBox();
IControl control = (IControl)obj;
IDataBound dataBound = (IDataBound)obj;
```
<span data-ttu-id="0e09e-705">V `EditBox` předchozí `public` třídě `Paint` je metoda `IControl` `IDataBound` z rozhraní a Metodazrozhraníimplementovánapomocíčlenů.`Bind`</span><span class="sxs-lookup"><span data-stu-id="0e09e-705">In the previous `EditBox` class, the `Paint` method from the `IControl` interface and the `Bind` method from the `IDataBound` interface are implemented using `public` members.</span></span> <span data-ttu-id="0e09e-706">C#podporuje také ***explicitní implementace členů rozhraní***, pomocí kterých se třída nebo struktura může vyhnout vytváření členů `public`.</span><span class="sxs-lookup"><span data-stu-id="0e09e-706">C# also supports ***explicit interface member implementations***, using which the class or struct can avoid making the members `public`.</span></span> <span data-ttu-id="0e09e-707">Explicitní implementace člena rozhraní je zapsána pomocí plně kvalifikovaného názvu člena rozhraní.</span><span class="sxs-lookup"><span data-stu-id="0e09e-707">An explicit interface member implementation is written using the fully qualified interface member name.</span></span> <span data-ttu-id="0e09e-708">Například `EditBox` třída může `IControl.Paint` implementovat metody a `IDataBound.Bind` pomocí explicitní implementace členů rozhraní následujícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="0e09e-708">For example, the `EditBox` class could implement the `IControl.Paint` and `IDataBound.Bind` methods using explicit interface member implementations as follows.</span></span>

```csharp
public class EditBox: IControl, IDataBound
{
    void IControl.Paint() {...}
    void IDataBound.Bind(Binder b) {...}
}
```
<span data-ttu-id="0e09e-709">K explicitním členům rozhraní lze přistupovat pouze prostřednictvím typu rozhraní.</span><span class="sxs-lookup"><span data-stu-id="0e09e-709">Explicit interface members can only be accessed via the interface type.</span></span> <span data-ttu-id="0e09e-710">Například implementaci `IControl.Paint` poskytovanou předchozí `EditBox` třídou lze `EditBox` vyvolat pouze první `IControl` převod odkazu na typ rozhraní.</span><span class="sxs-lookup"><span data-stu-id="0e09e-710">For example, the implementation of `IControl.Paint` provided by the previous `EditBox` class can only be invoked by first converting the `EditBox` reference to the `IControl` interface type.</span></span>

```csharp
EditBox editBox = new EditBox();
editBox.Paint();                        // Error, no such method
IControl control = editBox;
control.Paint();                        // Ok
```

## <a name="enums"></a><span data-ttu-id="0e09e-711">Výčty</span><span class="sxs-lookup"><span data-stu-id="0e09e-711">Enums</span></span>

<span data-ttu-id="0e09e-712">***Typ výčtu*** je jedinečný typ hodnoty se sadou pojmenovaných konstant.</span><span class="sxs-lookup"><span data-stu-id="0e09e-712">An ***enum type*** is a distinct value type with a set of named constants.</span></span> <span data-ttu-id="0e09e-713">Následující příklad deklaruje a používá `Color` typ výčtu pojmenovaný se třemi konstantními hodnotami, `Red`, `Green`a `Blue`.</span><span class="sxs-lookup"><span data-stu-id="0e09e-713">The following example declares and uses an enum type named `Color` with three constant values, `Red`, `Green`, and `Blue`.</span></span>

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
<span data-ttu-id="0e09e-714">Každý typ výčtu má odpovídající celočíselný typ nazvaný ***základní typ*** výčtového typu.</span><span class="sxs-lookup"><span data-stu-id="0e09e-714">Each enum type has a corresponding integral type called the ***underlying type*** of the enum type.</span></span> <span data-ttu-id="0e09e-715">Typ výčtu, který explicitně nedeklaruje nadřízený typ, má podkladový typ `int`.</span><span class="sxs-lookup"><span data-stu-id="0e09e-715">An enum type that does not explicitly declare an underlying type has an underlying type of `int`.</span></span> <span data-ttu-id="0e09e-716">Formát úložiště typu výčtu a rozsah možných hodnot jsou určeny podle jeho základního typu.</span><span class="sxs-lookup"><span data-stu-id="0e09e-716">An enum type's storage format and range of possible values are determined by its underlying type.</span></span> <span data-ttu-id="0e09e-717">Sada hodnot, na jejichž základě může typ výčtu pobírat, není omezena členy výčtu.</span><span class="sxs-lookup"><span data-stu-id="0e09e-717">The set of values that an enum type can take on is not limited by its enum members.</span></span> <span data-ttu-id="0e09e-718">Konkrétně jakákoli hodnota základního typu výčtu může být převedena na typ výčtu a je odlišná platná hodnota tohoto typu výčtu.</span><span class="sxs-lookup"><span data-stu-id="0e09e-718">In particular, any value of the underlying type of an enum can be cast to the enum type and is a distinct valid value of that enum type.</span></span>

<span data-ttu-id="0e09e-719">Následující příklad deklaruje typ výčtu s názvem `Alignment` s podkladovým `sbyte`typem.</span><span class="sxs-lookup"><span data-stu-id="0e09e-719">The following example declares an enum type named `Alignment` with an underlying type of `sbyte`.</span></span>

```csharp
enum Alignment: sbyte
{
    Left = -1,
    Center = 0,
    Right = 1
}
```
<span data-ttu-id="0e09e-720">Jak je znázorněno v předchozím příkladu, deklarace člena výčtu může zahrnovat konstantní výraz, který určuje hodnotu člena.</span><span class="sxs-lookup"><span data-stu-id="0e09e-720">As shown by the previous example, an enum member declaration can include a constant expression that specifies the value of the member.</span></span> <span data-ttu-id="0e09e-721">Hodnota konstanty pro každého člena výčtu musí být v rozsahu nadřazeného typu výčtu.</span><span class="sxs-lookup"><span data-stu-id="0e09e-721">The constant value for each enum member must be in the range of the underlying type of the enum.</span></span> <span data-ttu-id="0e09e-722">Když deklarace člena výčtu explicitně neurčí hodnotu, členovi se předává hodnota nula (Pokud se jedná o první člen v typu výčtu) nebo hodnota dříve předcházejícího člena výčtu plus jedna.</span><span class="sxs-lookup"><span data-stu-id="0e09e-722">When an enum member declaration does not explicitly specify a value, the member is given the value zero (if it is the first member in the enum type) or the value of the textually preceding enum member plus one.</span></span>

<span data-ttu-id="0e09e-723">Hodnoty výčtu lze převést na integrální hodnoty a naopak pomocí přetypování typů.</span><span class="sxs-lookup"><span data-stu-id="0e09e-723">Enum values can be converted to integral values and vice versa using type casts.</span></span> <span data-ttu-id="0e09e-724">Příklad</span><span class="sxs-lookup"><span data-stu-id="0e09e-724">For example</span></span>

```csharp
int i = (int)Color.Blue;        // int i = 2;
Color c = (Color)2;             // Color c = Color.Blue;
```
<span data-ttu-id="0e09e-725">Výchozí hodnota libovolného typu výčtu je celočíselná hodnota nula převedená na typ výčtu.</span><span class="sxs-lookup"><span data-stu-id="0e09e-725">The default value of any enum type is the integral value zero converted to the enum type.</span></span> <span data-ttu-id="0e09e-726">V případech, kdy jsou proměnné automaticky inicializovány na výchozí hodnotu, je tato hodnota předána proměnným typům výčtu.</span><span class="sxs-lookup"><span data-stu-id="0e09e-726">In cases where variables are automatically initialized to a default value, this is the value given to variables of enum types.</span></span> <span data-ttu-id="0e09e-727">Aby byla výchozí hodnota typu výčtu snadno dostupná, literál `0` implicitně převede na libovolný typ výčtu.</span><span class="sxs-lookup"><span data-stu-id="0e09e-727">In order for the default value of an enum type to be easily available, the literal `0` implicitly converts to any enum type.</span></span> <span data-ttu-id="0e09e-728">Proto jsou povoleny následující.</span><span class="sxs-lookup"><span data-stu-id="0e09e-728">Thus, the following is permitted.</span></span>

```csharp
Color c = 0;
```

## <a name="delegates"></a><span data-ttu-id="0e09e-729">Delegáty</span><span class="sxs-lookup"><span data-stu-id="0e09e-729">Delegates</span></span>

<span data-ttu-id="0e09e-730">***Typ delegáta*** představuje odkazy na metody s konkrétním seznamem parametrů a návratovým typem.</span><span class="sxs-lookup"><span data-stu-id="0e09e-730">A ***delegate type*** represents references to methods with a particular parameter list and return type.</span></span> <span data-ttu-id="0e09e-731">Delegáti umožňují zacházet s metodami jako s entitami, které lze přiřadit proměnným a předávat jako parametry.</span><span class="sxs-lookup"><span data-stu-id="0e09e-731">Delegates make it possible to treat methods as entities that can be assigned to variables and passed as parameters.</span></span> <span data-ttu-id="0e09e-732">Delegáti jsou podobní pojmu ukazatelů na funkce nalezené v některých jiných jazycích, ale na rozdíl od ukazatelů na funkce jsou delegáti objektově orientované a typově bezpečné.</span><span class="sxs-lookup"><span data-stu-id="0e09e-732">Delegates are similar to the concept of function pointers found in some other languages, but unlike function pointers, delegates are object-oriented and type-safe.</span></span>

<span data-ttu-id="0e09e-733">Následující příklad deklaruje a používá typ delegáta s názvem `Function`.</span><span class="sxs-lookup"><span data-stu-id="0e09e-733">The following example declares and uses a delegate type named `Function`.</span></span>

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
<span data-ttu-id="0e09e-734">Instance `Function` typu delegáta může odkazovat na jakoukoli metodu, která `double` přebírá argument a vrací `double` hodnotu.</span><span class="sxs-lookup"><span data-stu-id="0e09e-734">An instance of the `Function` delegate type can reference any method that takes a `double` argument and returns a `double` value.</span></span> <span data-ttu-id="0e09e-735">Metoda použije daný `Function` prvek `double[]` pro prvky a vrátí výsledek. `double[]` `Apply`</span><span class="sxs-lookup"><span data-stu-id="0e09e-735">The `Apply` method applies a given `Function` to the elements of a `double[]`, returning a `double[]` with the results.</span></span> <span data-ttu-id="0e09e-736">`Main` V `double[]`metodě sepoužívákpoužitítřírůznýchfunkcína.`Apply`</span><span class="sxs-lookup"><span data-stu-id="0e09e-736">In the `Main` method, `Apply` is used to apply three different functions to a `double[]`.</span></span>

<span data-ttu-id="0e09e-737">Delegát může odkazovat buď na statickou metodu (například `Square` nebo `Math.Sin` v předchozím příkladu), nebo na metodu `m.Multiply` instance (například v předchozím příkladu).</span><span class="sxs-lookup"><span data-stu-id="0e09e-737">A delegate can reference either a static method (such as `Square` or `Math.Sin` in the previous example) or an instance method (such as `m.Multiply` in the previous example).</span></span> <span data-ttu-id="0e09e-738">Delegát, který odkazuje na metodu instance, také odkazuje na konkrétní objekt a když je metoda instance vyvolána prostřednictvím delegáta, bude `this` tento objekt ve volání.</span><span class="sxs-lookup"><span data-stu-id="0e09e-738">A delegate that references an instance method also references a particular object, and when the instance method is invoked through the delegate, that object becomes `this` in the invocation.</span></span>

<span data-ttu-id="0e09e-739">Delegáty je také možné vytvořit pomocí anonymních funkcí, což jsou "vložené metody", které jsou vytvářeny průběžně.</span><span class="sxs-lookup"><span data-stu-id="0e09e-739">Delegates can also be created using anonymous functions, which are "inline methods" that are created on the fly.</span></span> <span data-ttu-id="0e09e-740">Anonymní funkce mohou zobrazit místní proměnné okolních metod.</span><span class="sxs-lookup"><span data-stu-id="0e09e-740">Anonymous functions can see the local variables of the surrounding methods.</span></span> <span data-ttu-id="0e09e-741">Proto je příklad násobitele výše možné napsat snadněji bez použití `Multiplier` třídy:</span><span class="sxs-lookup"><span data-stu-id="0e09e-741">Thus, the multiplier example above can be written more easily without using a `Multiplier` class:</span></span>

```csharp
double[] doubles =  Apply(a, (double x) => x * 2.0);
```
<span data-ttu-id="0e09e-742">Zajímavou a užitečnou vlastností delegáta je, že neznají ani nezáleží na třídě metody, na kterou odkazuje; všechny tyto věci jsou, že odkazovaná metoda má stejné parametry a návratový typ jako delegát.</span><span class="sxs-lookup"><span data-stu-id="0e09e-742">An interesting and useful property of a delegate is that it does not know or care about the class of the method it references; all that matters is that the referenced method has the same parameters and return type as the delegate.</span></span>

## <a name="attributes"></a><span data-ttu-id="0e09e-743">Atributy</span><span class="sxs-lookup"><span data-stu-id="0e09e-743">Attributes</span></span>

<span data-ttu-id="0e09e-744">Typy, členy a další entity v C# programu podporují modifikátory, které řídí určité aspekty jejich chování.</span><span class="sxs-lookup"><span data-stu-id="0e09e-744">Types, members, and other entities in a C# program support modifiers that control certain aspects of their behavior.</span></span> <span data-ttu-id="0e09e-745">Například přístupnost metody je `public`řízena pomocí modifikátorů, `protected`, `internal`a `private` .</span><span class="sxs-lookup"><span data-stu-id="0e09e-745">For example, the accessibility of a method is controlled using the `public`, `protected`, `internal`, and `private` modifiers.</span></span> <span data-ttu-id="0e09e-746">C#generalizuje tuto funkci tak, aby uživatelsky definované typy deklarativních informací mohly být připojeny k programovým entitám a načteny v době běhu.</span><span class="sxs-lookup"><span data-stu-id="0e09e-746">C# generalizes this capability such that user-defined types of declarative information can be attached to program entities and retrieved at run-time.</span></span> <span data-ttu-id="0e09e-747">Programy určují tyto dodatečné deklarativní informace definováním a použitím ***atributů***.</span><span class="sxs-lookup"><span data-stu-id="0e09e-747">Programs specify this additional declarative information by defining and using ***attributes***.</span></span>

<span data-ttu-id="0e09e-748">Následující příklad deklaruje `HelpAttribute` atribut, který lze umístit do entit programu, aby poskytoval odkazy na jeho přidruženou dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="0e09e-748">The following example declares a `HelpAttribute` attribute that can be placed on program entities to provide links to their associated documentation.</span></span>

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
<span data-ttu-id="0e09e-749">Všechny třídy atributů jsou odvozeny `System.Attribute` ze základní třídy poskytnuté .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="0e09e-749">All attribute classes derive from the `System.Attribute` base class provided by the .NET Framework.</span></span> <span data-ttu-id="0e09e-750">Atributy lze použít zadáním jejich názvu spolu s případnými argumenty uvnitř hranatých závorek těsně před přidruženou deklarací.</span><span class="sxs-lookup"><span data-stu-id="0e09e-750">Attributes can be applied by giving their name, along with any arguments, inside square brackets just before the associated declaration.</span></span> <span data-ttu-id="0e09e-751">Pokud název atributu končí `Attribute`, Tato část názvu může být vynechána při odkazování na atribut.</span><span class="sxs-lookup"><span data-stu-id="0e09e-751">If an attribute's name ends in `Attribute`, that part of the name can be omitted when the attribute is referenced.</span></span> <span data-ttu-id="0e09e-752">`HelpAttribute` Atribut lze například použít následujícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="0e09e-752">For example, the `HelpAttribute` attribute can be used as follows.</span></span>

```csharp
[Help("http://msdn.microsoft.com/.../MyClass.htm")]
public class Widget
{
    [Help("http://msdn.microsoft.com/.../MyClass.htm", Topic = "Display")]
    public void Display(string text) {}
}
```
<span data-ttu-id="0e09e-753">Tento `HelpAttribute` příklad připojí `Widget` ke tříděa`HelpAttribute` druhé k metoděvetřídě.`Display`</span><span class="sxs-lookup"><span data-stu-id="0e09e-753">This example attaches a `HelpAttribute` to the `Widget` class and another `HelpAttribute` to the `Display` method in the class.</span></span> <span data-ttu-id="0e09e-754">Veřejné konstruktory třídy atributu určují informace, které je nutné zadat při připojení atributu k entitě programu.</span><span class="sxs-lookup"><span data-stu-id="0e09e-754">The public constructors of an attribute class control the information that must be provided when the attribute is attached to a program entity.</span></span> <span data-ttu-id="0e09e-755">Další informace lze poskytnout odkazem na veřejné vlastnosti pro čtení a zápis třídy atributu (například odkaz na `Topic` vlastnost dříve).</span><span class="sxs-lookup"><span data-stu-id="0e09e-755">Additional information can be provided by referencing public read-write properties of the attribute class (such as the reference to the `Topic` property previously).</span></span>

<span data-ttu-id="0e09e-756">Následující příklad ukazuje, jak lze načíst informace o atributu pro danou entitu programu v době běhu pomocí reflexe.</span><span class="sxs-lookup"><span data-stu-id="0e09e-756">The following example shows how attribute information for a given program entity can be retrieved at run-time using reflection.</span></span>

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
<span data-ttu-id="0e09e-757">Pokud je požadován konkrétní atribut prostřednictvím reflexe, konstruktor třídy atributu je vyvolán s informacemi poskytnutými ve zdroji programu a výslednou instancí atributu je vrácen.</span><span class="sxs-lookup"><span data-stu-id="0e09e-757">When a particular attribute is requested through reflection, the constructor for the attribute class is invoked with the information provided in the program source, and the resulting attribute instance is returned.</span></span> <span data-ttu-id="0e09e-758">Pokud byly k dispozici další informace prostřednictvím vlastností, jsou tyto vlastnosti nastaveny na zadané hodnoty před vrácením instance atributu.</span><span class="sxs-lookup"><span data-stu-id="0e09e-758">If additional information was provided through properties, those properties are set to the given values before the attribute instance is returned.</span></span>
