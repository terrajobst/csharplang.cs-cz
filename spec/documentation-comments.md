---
ms.openlocfilehash: 2026fc1bf9d3576b967cbc2e9a670aa44b7eab3a
ms.sourcegitcommit: 892af9016b3317a8fae12d195014dc38ba51cf16
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/01/2019
ms.locfileid: "71704024"
---
# <a name="documentation-comments"></a><span data-ttu-id="86ec6-101">Komentáře dokumentace</span><span class="sxs-lookup"><span data-stu-id="86ec6-101">Documentation comments</span></span>

<span data-ttu-id="86ec6-102">C#poskytuje mechanismus pro programátory k dokumentování kódu pomocí speciální syntaxe komentářů, která obsahuje text XML.</span><span class="sxs-lookup"><span data-stu-id="86ec6-102">C# provides a mechanism for programmers to document their code using a special comment syntax that contains XML text.</span></span> <span data-ttu-id="86ec6-103">V souborech zdrojového kódu lze použít komentáře s určitým formulářem k tomu, aby nástroj vytvořil XML z těchto komentářů a prvky zdrojového kódu, které předcházejí.</span><span class="sxs-lookup"><span data-stu-id="86ec6-103">In source code files, comments having a certain form can be used to direct a tool to produce XML from those comments and the source code elements, which they precede.</span></span> <span data-ttu-id="86ec6-104">Komentáře používající takovou syntaxi se nazývají ***dokumentační komentáře***.</span><span class="sxs-lookup"><span data-stu-id="86ec6-104">Comments using such syntax are called ***documentation comments***.</span></span> <span data-ttu-id="86ec6-105">Musí bezprostředně předcházet uživatelsky definovanému typu (například třída, delegát nebo rozhraní) nebo člen (například pole, událost, vlastnost nebo metoda).</span><span class="sxs-lookup"><span data-stu-id="86ec6-105">They must immediately precede a user-defined type (such as a class, delegate, or interface) or a member (such as a field, event, property, or method).</span></span> <span data-ttu-id="86ec6-106">Nástroj pro generování XML se nazývá ***generátor dokumentace***.</span><span class="sxs-lookup"><span data-stu-id="86ec6-106">The XML generation tool is called the ***documentation generator***.</span></span> <span data-ttu-id="86ec6-107">(Tento generátor může být, ale nemusí být samotným C# kompilátorem.) Výstup vyprodukovaný v dokumentaci generátoru se nazývá ***soubor dokumentace***.</span><span class="sxs-lookup"><span data-stu-id="86ec6-107">(This generator could be, but need not be, the C# compiler itself.) The output produced by the documentation generator is called the ***documentation file***.</span></span> <span data-ttu-id="86ec6-108">Soubor dokumentace slouží jako vstup do ***prohlížeče dokumentace***; nástroj určený k vytvoření určitého způsobu vizuálního zobrazení informací o typu a související dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="86ec6-108">A documentation file is used as input to a ***documentation viewer***; a tool intended to produce some sort of visual display of type information and its associated documentation.</span></span>

<span data-ttu-id="86ec6-109">Tato specifikace navrhuje sadu značek pro použití v dokumentačních komentářích, ale použití těchto značek není vyžadováno a jiné značky mohou být použity v případě potřeby, pokud jsou dodržena pravidla ve správném formátu XML.</span><span class="sxs-lookup"><span data-stu-id="86ec6-109">This specification suggests a set of tags to be used in documentation comments, but use of these tags is not required, and other tags may be used if desired, as long the rules of well-formed XML are followed.</span></span>

## <a name="introduction"></a><span data-ttu-id="86ec6-110">Úvod</span><span class="sxs-lookup"><span data-stu-id="86ec6-110">Introduction</span></span>

<span data-ttu-id="86ec6-111">Komentáře s speciálním formulářem lze použít k tomu, aby nástroj vytvořil XML z těchto komentářů a prvky zdrojového kódu, které předcházejí.</span><span class="sxs-lookup"><span data-stu-id="86ec6-111">Comments having a special form can be used to direct a tool to produce XML from those comments and the source code elements, which they precede.</span></span> <span data-ttu-id="86ec6-112">Jedná se o Jednořádkový komentář, který začíná třemi lomítky (`///`) nebo oddělenými komentáři, které začínají lomítkem a dvěma hvězdičkami (`/**`).</span><span class="sxs-lookup"><span data-stu-id="86ec6-112">Such comments are single-line comments that start with three slashes (`///`), or delimited comments that start with a slash and two stars (`/**`).</span></span> <span data-ttu-id="86ec6-113">Musí bezprostředně předcházet uživatelem definovaný typ (například třída, delegát nebo rozhraní) nebo člen (například pole, událost, vlastnost nebo metoda), ke kterým mají anotace.</span><span class="sxs-lookup"><span data-stu-id="86ec6-113">They must immediately precede a user-defined type (such as a class, delegate, or interface) or a member (such as a field, event, property, or method) that they annotate.</span></span> <span data-ttu-id="86ec6-114">Oddíly atributů ([specifikace atributů](attributes.md#attribute-specification)) se považují za součást deklarací, takže dokumentační komentáře musí předcházet atributům použitým pro typ nebo člen.</span><span class="sxs-lookup"><span data-stu-id="86ec6-114">Attribute sections ([Attribute specification](attributes.md#attribute-specification)) are considered part of declarations, so documentation comments must precede attributes applied to a type or member.</span></span>

<span data-ttu-id="86ec6-115">__Syntaktick__</span><span class="sxs-lookup"><span data-stu-id="86ec6-115">__Syntax:__</span></span>

```antlr
single_line_doc_comment
    : '///' input_character*
    ;

delimited_doc_comment
    : '/**' delimited_comment_section* asterisk+ '/'
    ;
```

<span data-ttu-id="86ec6-116">Pokud je v *single_line_doc_comment* *mezera* , která následuje za znaky `///` na každé *single_line_doc_comment*s aktuální *single_line_doc_comment*, pak tento *prázdný znak* ve výstupu XML není obsažen znak.</span><span class="sxs-lookup"><span data-stu-id="86ec6-116">In a *single_line_doc_comment*, if there is a *whitespace* character following the `///` characters on each of the *single_line_doc_comment*s adjacent to the current *single_line_doc_comment*, then that *whitespace* character is not included in the XML output.</span></span>

<span data-ttu-id="86ec6-117">V komentáři s oddělovači, pokud je první neprázdný znak na druhém řádku hvězdičkou a stejný vzor volitelných prázdných znaků a znak hvězdičky se opakuje na začátku každého řádku v rámci objektu s oddělovači. znaky opakovaného vzoru nejsou zahrnuty ve výstupu XML.</span><span class="sxs-lookup"><span data-stu-id="86ec6-117">In a delimited-doc-comment, if the first non-whitespace character on the second line is an asterisk and the same pattern of optional whitespace characters and an asterisk character is repeated at the beginning of each of the line within the delimited-doc-comment, then the characters of the repeated pattern are not included in the XML output.</span></span> <span data-ttu-id="86ec6-118">Vzor může obsahovat prázdné znaky po znaku hvězdičky a také před znakem hvězdičky.</span><span class="sxs-lookup"><span data-stu-id="86ec6-118">The pattern may include whitespace characters after, as well as before, the asterisk character.</span></span>

<span data-ttu-id="86ec6-119">__Příklad:__</span><span class="sxs-lookup"><span data-stu-id="86ec6-119">__Example:__</span></span>

```csharp
/// <summary>Class <c>Point</c> models a point in a two-dimensional
/// plane.</summary>
///
public class Point 
{
    /// <summary>method <c>draw</c> renders the point.</summary>
    void draw() {...}
}
```

<span data-ttu-id="86ec6-120">Text v dokumentačních komentářích musí být ve správném formátu podle pravidel XML (https://www.w3.org/TR/REC-xml).</span><span class="sxs-lookup"><span data-stu-id="86ec6-120">The text within documentation comments must be well formed according to the rules of XML (https://www.w3.org/TR/REC-xml).</span></span> <span data-ttu-id="86ec6-121">Pokud je kód XML chybně vytvořen, je vygenerováno upozornění a soubor dokumentace bude obsahovat komentář, který říká, že došlo k chybě.</span><span class="sxs-lookup"><span data-stu-id="86ec6-121">If the XML is ill formed, a warning is generated and the documentation file will contain a comment saying that an error was encountered.</span></span>

<span data-ttu-id="86ec6-122">I když je vývojářům zdarma vytvořit vlastní sadu značek, doporučovaná sada je definována v [doporučených značkách](documentation-comments.md#recommended-tags).</span><span class="sxs-lookup"><span data-stu-id="86ec6-122">Although developers are free to create their own set of tags, a recommended set is defined in [Recommended tags](documentation-comments.md#recommended-tags).</span></span> <span data-ttu-id="86ec6-123">Některé z doporučených značek mají zvláštní význam:</span><span class="sxs-lookup"><span data-stu-id="86ec6-123">Some of the recommended tags have special meanings:</span></span>

*  <span data-ttu-id="86ec6-124">Tato `<param>` značka se používá k popisu parametrů.</span><span class="sxs-lookup"><span data-stu-id="86ec6-124">The `<param>` tag is used to describe parameters.</span></span> <span data-ttu-id="86ec6-125">Je-li použita taková značka, musí generátor dokumentace ověřit, zda zadaný parametr existuje a zda jsou všechny parametry popsány v dokumentaci dokumentace.</span><span class="sxs-lookup"><span data-stu-id="86ec6-125">If such a tag is used, the documentation generator must verify that the specified parameter exists and that all parameters are described in documentation comments.</span></span> <span data-ttu-id="86ec6-126">Pokud takové ověření neproběhne úspěšně, generátor dokumentace vydá upozornění.</span><span class="sxs-lookup"><span data-stu-id="86ec6-126">If such verification fails, the documentation generator issues a warning.</span></span>
*  <span data-ttu-id="86ec6-127">`cref` Atribut lze připojit k libovolné značce k poskytnutí odkazu na prvek kódu.</span><span class="sxs-lookup"><span data-stu-id="86ec6-127">The `cref` attribute can be attached to any tag to provide a reference to a code element.</span></span> <span data-ttu-id="86ec6-128">Generátor dokumentace musí ověřit, zda tento prvek kódu existuje.</span><span class="sxs-lookup"><span data-stu-id="86ec6-128">The documentation generator must verify that this code element exists.</span></span> <span data-ttu-id="86ec6-129">Pokud se ověření nepovede, generátor dokumentace vydá upozornění.</span><span class="sxs-lookup"><span data-stu-id="86ec6-129">If the verification fails, the documentation generator issues a warning.</span></span> <span data-ttu-id="86ec6-130">Při hledání názvu popsanýho v `cref` atributu musí generátor dokumentace respektovat viditelnost oboru názvů `using` podle příkazů, které se nacházejí v rámci zdrojového kódu.</span><span class="sxs-lookup"><span data-stu-id="86ec6-130">When looking for a name described in a `cref` attribute, the documentation generator must respect namespace visibility according to `using` statements appearing within the source code.</span></span> <span data-ttu-id="86ec6-131">U elementů kódu, které jsou obecné, nelze použít normální obecnou syntaxi (`List<T>`tj. ""), protože vytváří neplatný kód XML.</span><span class="sxs-lookup"><span data-stu-id="86ec6-131">For code elements that are generic, the normal generic syntax (that is, "`List<T>`") cannot be used because it produces invalid XML.</span></span> <span data-ttu-id="86ec6-132">Složené závorky lze použít místo závorek (`List{T}`tj. ""), případně lze použít syntaxi Escape XML (tj`List&lt;T&gt;`.).</span><span class="sxs-lookup"><span data-stu-id="86ec6-132">Braces can be used instead of brackets (that is, "`List{T}`"), or the XML escape syntax can be used (that is, "`List&lt;T&gt;`").</span></span>
*  <span data-ttu-id="86ec6-133">`<summary>` Značku má použít prohlížeč dokumentace pro zobrazení dalších informací o typu nebo členu.</span><span class="sxs-lookup"><span data-stu-id="86ec6-133">The `<summary>` tag is intended to be used by a documentation viewer to display additional information about a type or member.</span></span>
*  <span data-ttu-id="86ec6-134">`<include>` Značka obsahuje informace z externího souboru XML.</span><span class="sxs-lookup"><span data-stu-id="86ec6-134">The `<include>` tag includes information from an external XML file.</span></span>

<span data-ttu-id="86ec6-135">Pozorně si všimněte, že soubor dokumentace neposkytuje úplné informace o typu a členech (například neobsahuje žádné informace o typu).</span><span class="sxs-lookup"><span data-stu-id="86ec6-135">Note carefully that the documentation file does not provide full information about the type and members (for example, it does not contain any type information).</span></span> <span data-ttu-id="86ec6-136">Chcete-li získat informace o typu nebo členu, je třeba použít soubor dokumentace ve spojení s reflexí pro skutečný typ nebo člen.</span><span class="sxs-lookup"><span data-stu-id="86ec6-136">To get such information about a type or member, the documentation file must be used in conjunction with reflection on the actual type or member.</span></span>

## <a name="recommended-tags"></a><span data-ttu-id="86ec6-137">Doporučené značky</span><span class="sxs-lookup"><span data-stu-id="86ec6-137">Recommended tags</span></span>

<span data-ttu-id="86ec6-138">Generátor dokumentace musí přijmout a zpracovat všechny značky, které jsou platné podle pravidel XML.</span><span class="sxs-lookup"><span data-stu-id="86ec6-138">The documentation generator must accept and process any tag that is valid according to the rules of XML.</span></span> <span data-ttu-id="86ec6-139">Následující značky poskytují běžně používané funkce v dokumentaci uživatele.</span><span class="sxs-lookup"><span data-stu-id="86ec6-139">The following tags provide commonly used functionality in user documentation.</span></span> <span data-ttu-id="86ec6-140">(Samozřejmě jsou možné další značky.)</span><span class="sxs-lookup"><span data-stu-id="86ec6-140">(Of course, other tags are possible.)</span></span>


| <span data-ttu-id="86ec6-141">__Inteligentní__</span><span class="sxs-lookup"><span data-stu-id="86ec6-141">__Tag__</span></span>          | <span data-ttu-id="86ec6-142">__Section__</span><span class="sxs-lookup"><span data-stu-id="86ec6-142">__Section__</span></span>                                            | <span data-ttu-id="86ec6-143">__Účel__</span><span class="sxs-lookup"><span data-stu-id="86ec6-143">__Purpose__</span></span>                                            |
|------------------|--------------------------------------------------------|--------------------------------------------------------|
| `<c>`            | [`<c>`](documentation-comments.md#c)                   | <span data-ttu-id="86ec6-144">Nastavení textu v písmu podobného kódu</span><span class="sxs-lookup"><span data-stu-id="86ec6-144">Set text in a code-like font</span></span>                           | 
| `<code>`         | [`<code>`](documentation-comments.md#code)             | <span data-ttu-id="86ec6-145">Nastavení jednoho nebo více řádků zdrojového kódu nebo výstupu programu</span><span class="sxs-lookup"><span data-stu-id="86ec6-145">Set one or more lines of source code or program output</span></span> |
| `<example>`      | [`<example>`](documentation-comments.md#example)       | <span data-ttu-id="86ec6-146">Označení příkladu</span><span class="sxs-lookup"><span data-stu-id="86ec6-146">Indicate an example</span></span>                                    |
| `<exception>`    | [`<exception>`](documentation-comments.md#exception)   | <span data-ttu-id="86ec6-147">Určuje výjimky, které může metoda vyvolat.</span><span class="sxs-lookup"><span data-stu-id="86ec6-147">Identifies the exceptions a method can throw</span></span>           |
| `<include>`      | [`<include>`](documentation-comments.md#include)       | <span data-ttu-id="86ec6-148">Zahrnuje XML z externího souboru.</span><span class="sxs-lookup"><span data-stu-id="86ec6-148">Includes XML from an external file</span></span>                     |
| `<list>`         | [`<list>`](documentation-comments.md#list)             | <span data-ttu-id="86ec6-149">Vytvoření seznamu nebo tabulky</span><span class="sxs-lookup"><span data-stu-id="86ec6-149">Create a list or table</span></span>                                 |
| `<para>`         | [`<para>`](documentation-comments.md#para)             | <span data-ttu-id="86ec6-150">Povolit přidání struktury do textu</span><span class="sxs-lookup"><span data-stu-id="86ec6-150">Permit structure to be added to text</span></span>                   |
| `<param>`        | [`<param>`](documentation-comments.md#param)           | <span data-ttu-id="86ec6-151">Popište parametr pro metodu nebo konstruktor.</span><span class="sxs-lookup"><span data-stu-id="86ec6-151">Describe a parameter for a method or constructor</span></span>       |
| `<paramref>`     | [`<paramref>`](documentation-comments.md#paramref)     | <span data-ttu-id="86ec6-152">Určení, že slovo je název parametru</span><span class="sxs-lookup"><span data-stu-id="86ec6-152">Identify that a word is a parameter name</span></span>               |
| `<permission>`   | [`<permission>`](documentation-comments.md#permission) | <span data-ttu-id="86ec6-153">Dokumentuje přístupnost člena v zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="86ec6-153">Document the security accessibility of a member</span></span>        |
| `<remarks>`      | [`<remarks>`](documentation-comments.md#remarks)       | <span data-ttu-id="86ec6-154">Popište Další informace o typu.</span><span class="sxs-lookup"><span data-stu-id="86ec6-154">Describe additional information about a type</span></span>           |
| `<returns>`      | [`<returns>`](documentation-comments.md#returns)       | <span data-ttu-id="86ec6-155">Popisuje návratovou hodnotu metody.</span><span class="sxs-lookup"><span data-stu-id="86ec6-155">Describe the return value of a method</span></span>                  |
| `<see>`          | [`<see>`](documentation-comments.md#see)               | <span data-ttu-id="86ec6-156">Zadat odkaz</span><span class="sxs-lookup"><span data-stu-id="86ec6-156">Specify a link</span></span>                                         |
| `<seealso>`      | [`<seealso>`](documentation-comments.md#seealso)       | <span data-ttu-id="86ec6-157">Generovat položku viz také</span><span class="sxs-lookup"><span data-stu-id="86ec6-157">Generate a See Also entry</span></span>                              |
| `<summary>`      | [`<summary>`](documentation-comments.md#summary)       | <span data-ttu-id="86ec6-158">Popis typu nebo členu typu</span><span class="sxs-lookup"><span data-stu-id="86ec6-158">Describe a type or a member of a type</span></span>                  |
| `<value>`        | [`<value>`](documentation-comments.md#value)           | <span data-ttu-id="86ec6-159">Popsat vlastnost</span><span class="sxs-lookup"><span data-stu-id="86ec6-159">Describe a property</span></span>                                    |
| `<typeparam>`    |                                                        | <span data-ttu-id="86ec6-160">Popsat parametr obecného typu</span><span class="sxs-lookup"><span data-stu-id="86ec6-160">Describe a generic type parameter</span></span>                      |
| `<typeparamref>` |                                                        | <span data-ttu-id="86ec6-161">Určení, že slovo je názvem parametru typu</span><span class="sxs-lookup"><span data-stu-id="86ec6-161">Identify that a word is a type parameter name</span></span>          |

### `<c>`

<span data-ttu-id="86ec6-162">Tato značka poskytuje mechanismus pro indikaci, že fragment textu v rámci popisu by měl být nastaven ve speciálním písmu, například, které se používá pro blok kódu.</span><span class="sxs-lookup"><span data-stu-id="86ec6-162">This tag provides a mechanism to indicate that a fragment of text within a description should be set in a special font such as that used for a block of code.</span></span> <span data-ttu-id="86ec6-163">Pro řádky vlastního kódu použijte `<code>` ([`<code>`](documentation-comments.md#code)).</span><span class="sxs-lookup"><span data-stu-id="86ec6-163">For lines of actual code, use `<code>` ([`<code>`](documentation-comments.md#code)).</span></span>

<span data-ttu-id="86ec6-164">__Syntaktick__</span><span class="sxs-lookup"><span data-stu-id="86ec6-164">__Syntax:__</span></span>

```xml
<c>text</c>
```

<span data-ttu-id="86ec6-165">__Příklad:__</span><span class="sxs-lookup"><span data-stu-id="86ec6-165">__Example:__</span></span>

```csharp
/// <summary>Class <c>Point</c> models a point in a two-dimensional
/// plane.</summary>

public class Point 
{
    // ...
}
```

### `<code>`

<span data-ttu-id="86ec6-166">Tato značka se používá k nastavení jednoho nebo více řádků zdrojového kódu nebo výstupu programu v některém speciálním písmu.</span><span class="sxs-lookup"><span data-stu-id="86ec6-166">This tag is used to set one or more lines of source code or program output in some special font.</span></span> <span data-ttu-id="86ec6-167">Pro malé fragmenty kódu v mluveném komentáři[`<c>`](documentation-comments.md#c)použijte `<c>` ().</span><span class="sxs-lookup"><span data-stu-id="86ec6-167">For small code fragments in narrative, use `<c>` ([`<c>`](documentation-comments.md#c)).</span></span>

<span data-ttu-id="86ec6-168">__Syntaktick__</span><span class="sxs-lookup"><span data-stu-id="86ec6-168">__Syntax:__</span></span>

```xml
<code>source code or program output</code>
```

<span data-ttu-id="86ec6-169">__Příklad:__</span><span class="sxs-lookup"><span data-stu-id="86ec6-169">__Example:__</span></span>

```csharp
/// <summary>This method changes the point's location by
///    the given x- and y-offsets.
/// <example>For example:
/// <code>
///    Point p = new Point(3,5);
///    p.Translate(-1,3);
/// </code>
/// results in <c>p</c>'s having the value (2,8).
/// </example>
/// </summary>

public void Translate(int xor, int yor) {
    X += xor;
    Y += yor;
}   
```

### `<example>`

<span data-ttu-id="86ec6-170">Tato značka umožňuje ukázkový kód v rámci komentáře, který určuje, jak lze použít metodu nebo jiného člena knihovny.</span><span class="sxs-lookup"><span data-stu-id="86ec6-170">This tag allows example code within a comment, to specify how a method or other library member may be used.</span></span> <span data-ttu-id="86ec6-171">Obvykle by to zahrnovalo i použití značky `<code>` ([`<code>`](documentation-comments.md#code)).</span><span class="sxs-lookup"><span data-stu-id="86ec6-171">Ordinarily, this would also involve use of the tag `<code>` ([`<code>`](documentation-comments.md#code)) as well.</span></span>

<span data-ttu-id="86ec6-172">__Syntaktick__</span><span class="sxs-lookup"><span data-stu-id="86ec6-172">__Syntax:__</span></span>

```xml
<example>description</example>
```

<span data-ttu-id="86ec6-173">__Příklad:__</span><span class="sxs-lookup"><span data-stu-id="86ec6-173">__Example:__</span></span>

<span data-ttu-id="86ec6-174">Příklad `<code>` naleznete[`<code>`](documentation-comments.md#code)v tématu ().</span><span class="sxs-lookup"><span data-stu-id="86ec6-174">See `<code>` ([`<code>`](documentation-comments.md#code)) for an example.</span></span>

### `<exception>`

<span data-ttu-id="86ec6-175">Tato značka poskytuje způsob, jak zdokumentovat výjimky, které může metoda vyvolat.</span><span class="sxs-lookup"><span data-stu-id="86ec6-175">This tag provides a way to document the exceptions a method can throw.</span></span>

<span data-ttu-id="86ec6-176">__Syntaktick__</span><span class="sxs-lookup"><span data-stu-id="86ec6-176">__Syntax:__</span></span>

```xml
<exception cref="member">description</exception>
```

<span data-ttu-id="86ec6-177">kde</span><span class="sxs-lookup"><span data-stu-id="86ec6-177">where</span></span>

* <span data-ttu-id="86ec6-178">`member`je název člena.</span><span class="sxs-lookup"><span data-stu-id="86ec6-178">`member` is the name of a member.</span></span> <span data-ttu-id="86ec6-179">Generátor dokumentace kontroluje, zda daný člen existuje a překládá `member` se na název kanonického elementu v souboru dokumentace.</span><span class="sxs-lookup"><span data-stu-id="86ec6-179">The documentation generator checks that the given member exists and translates `member` to the canonical element name in the documentation file.</span></span>
* <span data-ttu-id="86ec6-180">`description`je popis okolností, v nichž je vyvolána výjimka.</span><span class="sxs-lookup"><span data-stu-id="86ec6-180">`description` is a description of the circumstances in which the exception is thrown.</span></span>

<span data-ttu-id="86ec6-181">__Příklad:__</span><span class="sxs-lookup"><span data-stu-id="86ec6-181">__Example:__</span></span>

```csharp
public class DataBaseOperations
{
    /// <exception cref="MasterFileFormatCorruptException"></exception>
    /// <exception cref="MasterFileLockedOpenException"></exception>
    public static void ReadRecord(int flag) {
        if (flag == 1)
            throw new MasterFileFormatCorruptException();
        else if (flag == 2)
            throw new MasterFileLockedOpenException();
        // ...
    } 
}
```

### `<include>`

<span data-ttu-id="86ec6-182">Tato značka umožňuje zahrnout informace z dokumentu XML, který je externí pro soubor zdrojového kódu.</span><span class="sxs-lookup"><span data-stu-id="86ec6-182">This tag allows including information from an XML document that is external to the source code file.</span></span> <span data-ttu-id="86ec6-183">Externím souborem musí být dokument XML ve správném formátu a k tomuto dokumentu se použije výraz XPath, který určuje, co XML z tohoto dokumentu má zahrnout.</span><span class="sxs-lookup"><span data-stu-id="86ec6-183">The external file must be a well-formed XML document, and an XPath expression is applied to that document to specify what XML from that document to include.</span></span> <span data-ttu-id="86ec6-184">`<include>` Značka se pak nahradí vybraným XML z externího dokumentu.</span><span class="sxs-lookup"><span data-stu-id="86ec6-184">The `<include>` tag is then replaced with the selected XML from the external document.</span></span>

<span data-ttu-id="86ec6-185">__Syntaktick__</span><span class="sxs-lookup"><span data-stu-id="86ec6-185">__Syntax:__</span></span>

```xml
<include file="filename" path="xpath" />
```

<span data-ttu-id="86ec6-186">kde</span><span class="sxs-lookup"><span data-stu-id="86ec6-186">where</span></span>

* <span data-ttu-id="86ec6-187">`filename`je název souboru externího souboru XML.</span><span class="sxs-lookup"><span data-stu-id="86ec6-187">`filename` is the file name of an external XML file.</span></span> <span data-ttu-id="86ec6-188">Název souboru je interpretován relativně k souboru, který obsahuje značku include.</span><span class="sxs-lookup"><span data-stu-id="86ec6-188">The file name is interpreted relative to the file that contains the include tag.</span></span>
* <span data-ttu-id="86ec6-189">`xpath`je výraz XPath, který vybírá některé XML v externím souboru XML.</span><span class="sxs-lookup"><span data-stu-id="86ec6-189">`xpath` is an XPath expression that selects some of the XML in the external XML file.</span></span>

<span data-ttu-id="86ec6-190">__Příklad:__</span><span class="sxs-lookup"><span data-stu-id="86ec6-190">__Example:__</span></span>

<span data-ttu-id="86ec6-191">Pokud zdrojový kód obsahoval deklaraci jako:</span><span class="sxs-lookup"><span data-stu-id="86ec6-191">If the source code contained a declaration like:</span></span>

```csharp
/// <include file="docs.xml" path='extradoc/class[@name="IntList"]/*' />
public class IntList { ... }
```

<span data-ttu-id="86ec6-192">a externí soubor docs. XML má následující obsah:</span><span class="sxs-lookup"><span data-stu-id="86ec6-192">and the external file "docs.xml" had the following contents:</span></span>

```xml
<?xml version="1.0"?>
<extradoc>
  <class name="IntList">
     <summary>
        Contains a list of integers.
     </summary>
  </class>
  <class name="StringList">
     <summary>
        Contains a list of integers.
     </summary>
  </class>
</extradoc>
```

<span data-ttu-id="86ec6-193">pak je stejná dokumentace ve výstupu, jako by obsahovala zdrojový kód:</span><span class="sxs-lookup"><span data-stu-id="86ec6-193">then the same documentation is output as if the source code contained:</span></span>

```csharp
/// <summary>
///    Contains a list of integers.
/// </summary>
public class IntList { ... }
```

### `<list>`

<span data-ttu-id="86ec6-194">Tato značka slouží k vytvoření seznamu nebo tabulky položek.</span><span class="sxs-lookup"><span data-stu-id="86ec6-194">This tag is used to create a list or table of items.</span></span> <span data-ttu-id="86ec6-195">Může obsahovat `<listheader>` blok pro definování řádku záhlaví buď tabulky, nebo seznamu definic.</span><span class="sxs-lookup"><span data-stu-id="86ec6-195">It may contain a `<listheader>` block to define the heading row of either a table or definition list.</span></span> <span data-ttu-id="86ec6-196">(Při definování tabulky je nutné zadat pouze záznam pro `term` v hlavičce.)</span><span class="sxs-lookup"><span data-stu-id="86ec6-196">(When defining a table, only an entry for `term` in the heading need be supplied.)</span></span>

<span data-ttu-id="86ec6-197">Každá položka v seznamu je určena `<item>` blokem.</span><span class="sxs-lookup"><span data-stu-id="86ec6-197">Each item in the list is specified with an `<item>` block.</span></span> <span data-ttu-id="86ec6-198">Při vytváření seznamu definic musí být zadán `term` obojí `description` a.</span><span class="sxs-lookup"><span data-stu-id="86ec6-198">When creating a definition list, both `term` and `description` must be specified.</span></span> <span data-ttu-id="86ec6-199">U tabulky, seznamu s odrážkami nebo číslovaného seznamu je však třeba `description` zadat pouze parametr.</span><span class="sxs-lookup"><span data-stu-id="86ec6-199">However, for a table, bulleted list, or numbered list, only `description` need be specified.</span></span>

<span data-ttu-id="86ec6-200">__Syntaktick__</span><span class="sxs-lookup"><span data-stu-id="86ec6-200">__Syntax:__</span></span>

```xml
<list type="bullet" | "number" | "table">
   <listheader>
      <term>term</term>
      <description>*description*</description>
   </listheader>
   <item>
      <term>term</term>
      <description>*description*</description>
   </item>
    ...
   <item>
      <term>term</term>
      <description>description</description>
   </item>
</list>
```

<span data-ttu-id="86ec6-201">kde</span><span class="sxs-lookup"><span data-stu-id="86ec6-201">where</span></span>

* <span data-ttu-id="86ec6-202">`term`je výraz, který má být definován, jehož definice `description`je v.</span><span class="sxs-lookup"><span data-stu-id="86ec6-202">`term` is the term to define, whose definition is in `description`.</span></span>
* <span data-ttu-id="86ec6-203">`description`je buď položka v seznamu odrážek nebo číslovaný seznam, nebo definice `term`.</span><span class="sxs-lookup"><span data-stu-id="86ec6-203">`description` is either an item in a bullet or numbered list, or the definition of a `term`.</span></span>

<span data-ttu-id="86ec6-204">__Příklad:__</span><span class="sxs-lookup"><span data-stu-id="86ec6-204">__Example:__</span></span>

```csharp
public class MyClass
{
    /// <summary>Here is an example of a bulleted list:
    /// <list type="bullet">
    /// <item>
    /// <description>Item 1.</description>
    /// </item>
    /// <item>
    /// <description>Item 2.</description>
    /// </item>
    /// </list>
    /// </summary>
    public static void Main () {
        // ...
    }
}
```

### `<para>`

<span data-ttu-id="86ec6-205">Tato značka je určena pro použití uvnitř jiných značek, například `<summary>` ([`<remarks>`](documentation-comments.md#remarks)) nebo `<returns>` ([`<returns>`](documentation-comments.md#returns)), a umožňuje přidání struktury do textu.</span><span class="sxs-lookup"><span data-stu-id="86ec6-205">This tag is for use inside other tags, such as `<summary>` ([`<remarks>`](documentation-comments.md#remarks)) or `<returns>` ([`<returns>`](documentation-comments.md#returns)), and permits structure to be added to text.</span></span>

<span data-ttu-id="86ec6-206">__Syntaktick__</span><span class="sxs-lookup"><span data-stu-id="86ec6-206">__Syntax:__</span></span>

```xml
<para>content</para>
```

<span data-ttu-id="86ec6-207">kde `content` je text odstavce.</span><span class="sxs-lookup"><span data-stu-id="86ec6-207">where `content` is the text of the paragraph.</span></span>

<span data-ttu-id="86ec6-208">__Příklad:__</span><span class="sxs-lookup"><span data-stu-id="86ec6-208">__Example:__</span></span>

```csharp
/// <summary>This is the entry point of the Point class testing program.
/// <para>This program tests each method and operator, and
/// is intended to be run after any non-trivial maintenance has
/// been performed on the Point class.</para></summary>
public static void Main() {
    // ...
}
```

### `<param>`

<span data-ttu-id="86ec6-209">Tato značka se používá k popisu parametru pro metodu, konstruktor nebo indexer.</span><span class="sxs-lookup"><span data-stu-id="86ec6-209">This tag is used to describe a parameter for a method, constructor, or indexer.</span></span>

<span data-ttu-id="86ec6-210">__Syntaktick__</span><span class="sxs-lookup"><span data-stu-id="86ec6-210">__Syntax:__</span></span>

```xml
<param name="name">description</param>
```

<span data-ttu-id="86ec6-211">kde</span><span class="sxs-lookup"><span data-stu-id="86ec6-211">where</span></span>

* <span data-ttu-id="86ec6-212">`name`je název parametru.</span><span class="sxs-lookup"><span data-stu-id="86ec6-212">`name` is the name of the parameter.</span></span>
* <span data-ttu-id="86ec6-213">`description`je popis parametru.</span><span class="sxs-lookup"><span data-stu-id="86ec6-213">`description` is a description of the parameter.</span></span>

<span data-ttu-id="86ec6-214">__Příklad:__</span><span class="sxs-lookup"><span data-stu-id="86ec6-214">__Example:__</span></span>

```csharp
/// <summary>This method changes the point's location to
///    the given coordinates.</summary>
/// <param name="xor">the new x-coordinate.</param>
/// <param name="yor">the new y-coordinate.</param>
public void Move(int xor, int yor) {
    X = xor;
    Y = yor;
}
```

### `<paramref>`

<span data-ttu-id="86ec6-215">Tato značka slouží k označení, že slovo je parametrem.</span><span class="sxs-lookup"><span data-stu-id="86ec6-215">This tag is used to indicate that a word is a parameter.</span></span> <span data-ttu-id="86ec6-216">Soubor dokumentace může být zpracován pro naformátování tohoto parametru nějakým odlišným způsobem.</span><span class="sxs-lookup"><span data-stu-id="86ec6-216">The documentation file can be processed to format this parameter in some distinct way.</span></span>

<span data-ttu-id="86ec6-217">__Syntaktick__</span><span class="sxs-lookup"><span data-stu-id="86ec6-217">__Syntax:__</span></span>

```xml
<paramref name="name"/>
```

<span data-ttu-id="86ec6-218">kde `name` je název parametru.</span><span class="sxs-lookup"><span data-stu-id="86ec6-218">where `name` is the name of the parameter.</span></span>

<span data-ttu-id="86ec6-219">__Příklad:__</span><span class="sxs-lookup"><span data-stu-id="86ec6-219">__Example:__</span></span>

```csharp
/// <summary>This constructor initializes the new Point to
///    (<paramref name="xor"/>,<paramref name="yor"/>).</summary>
/// <param name="xor">the new Point's x-coordinate.</param>
/// <param name="yor">the new Point's y-coordinate.</param>

public Point(int xor, int yor) {
    X = xor;
    Y = yor;
}
```

### `<permission>`

<span data-ttu-id="86ec6-220">Tato značka umožňuje zdokumentování zabezpečení člena.</span><span class="sxs-lookup"><span data-stu-id="86ec6-220">This tag allows the security accessibility of a member to be documented.</span></span>

<span data-ttu-id="86ec6-221">__Syntaktick__</span><span class="sxs-lookup"><span data-stu-id="86ec6-221">__Syntax:__</span></span>

```xml
<permission cref="member">description</permission>
```

<span data-ttu-id="86ec6-222">kde</span><span class="sxs-lookup"><span data-stu-id="86ec6-222">where</span></span>

* <span data-ttu-id="86ec6-223">`member`je název člena.</span><span class="sxs-lookup"><span data-stu-id="86ec6-223">`member` is the name of a member.</span></span> <span data-ttu-id="86ec6-224">Generátor dokumentace kontroluje, zda daný prvek kódu existuje, a překládá *člena* na kanonický název elementu v souboru dokumentace.</span><span class="sxs-lookup"><span data-stu-id="86ec6-224">The documentation generator checks that the given code element exists and translates *member* to the canonical element name in the documentation file.</span></span>
* <span data-ttu-id="86ec6-225">`description`je popis přístupu ke členu.</span><span class="sxs-lookup"><span data-stu-id="86ec6-225">`description` is a description of the access to the member.</span></span>

<span data-ttu-id="86ec6-226">__Příklad:__</span><span class="sxs-lookup"><span data-stu-id="86ec6-226">__Example:__</span></span>

```csharp
/// <permission cref="System.Security.PermissionSet">Everyone can
/// access this method.</permission>

public static void Test() {
    // ...
}
```

### `<remarks>`

<span data-ttu-id="86ec6-227">Tato značka slouží k zadání dalších informací o typu.</span><span class="sxs-lookup"><span data-stu-id="86ec6-227">This tag is used to specify extra information about a type.</span></span> <span data-ttu-id="86ec6-228">(Pomocí `<summary>` ([`<summary>`](documentation-comments.md#summary)) Popište samotný typ a členy typu.)</span><span class="sxs-lookup"><span data-stu-id="86ec6-228">(Use `<summary>` ([`<summary>`](documentation-comments.md#summary)) to describe the type itself and the members of a type.)</span></span>

<span data-ttu-id="86ec6-229">__Syntaktick__</span><span class="sxs-lookup"><span data-stu-id="86ec6-229">__Syntax:__</span></span>

```xml
<remarks>description</remarks>
```

<span data-ttu-id="86ec6-230">kde `description` je text přeznačení.</span><span class="sxs-lookup"><span data-stu-id="86ec6-230">where `description` is the text of the remark.</span></span>

<span data-ttu-id="86ec6-231">__Příklad:__</span><span class="sxs-lookup"><span data-stu-id="86ec6-231">__Example:__</span></span>

```csharp
/// <summary>Class <c>Point</c> models a point in a 
/// two-dimensional plane.</summary>
/// <remarks>Uses polar coordinates</remarks>
public class Point 
{
    // ...
}
```

### `<returns>`

<span data-ttu-id="86ec6-232">Tato značka se používá k popisu návratové hodnoty metody.</span><span class="sxs-lookup"><span data-stu-id="86ec6-232">This tag is used to describe the return value of a method.</span></span>

<span data-ttu-id="86ec6-233">__Syntaktick__</span><span class="sxs-lookup"><span data-stu-id="86ec6-233">__Syntax:__</span></span>

```xml
<returns>description</returns>
```

<span data-ttu-id="86ec6-234">kde `description` je popis návratové hodnoty.</span><span class="sxs-lookup"><span data-stu-id="86ec6-234">where `description` is a description of the return value.</span></span>

<span data-ttu-id="86ec6-235">__Příklad:__</span><span class="sxs-lookup"><span data-stu-id="86ec6-235">__Example:__</span></span>

```csharp
/// <summary>Report a point's location as a string.</summary>
/// <returns>A string representing a point's location, in the form (x,y),
///    without any leading, trailing, or embedded whitespace.</returns>
public override string ToString() {
    return "(" + X + "," + Y + ")";
}
```

### `<see>`

<span data-ttu-id="86ec6-236">Tato značka umožňuje zadat odkaz v rámci textu.</span><span class="sxs-lookup"><span data-stu-id="86ec6-236">This tag allows a link to be specified within text.</span></span> <span data-ttu-id="86ec6-237">Pomocí `<seealso>` [(`<seealso>`](documentation-comments.md#seealso)) označíte text, který se má zobrazit v části Viz také.</span><span class="sxs-lookup"><span data-stu-id="86ec6-237">Use `<seealso>` ([`<seealso>`](documentation-comments.md#seealso)) to indicate text that is to appear in a See Also section.</span></span>

<span data-ttu-id="86ec6-238">__Syntaktick__</span><span class="sxs-lookup"><span data-stu-id="86ec6-238">__Syntax:__</span></span>

```xml
<see cref="member"/>
```

<span data-ttu-id="86ec6-239">kde `member` je název člena.</span><span class="sxs-lookup"><span data-stu-id="86ec6-239">where `member` is the name of a member.</span></span> <span data-ttu-id="86ec6-240">Generátor dokumentace kontroluje, zda daný prvek kódu existuje a mění *člena* na název elementu v generovaném souboru dokumentace.</span><span class="sxs-lookup"><span data-stu-id="86ec6-240">The documentation generator checks that the given code element exists and changes *member* to the element name in the generated documentation file.</span></span>

<span data-ttu-id="86ec6-241">__Příklad:__</span><span class="sxs-lookup"><span data-stu-id="86ec6-241">__Example:__</span></span>

```csharp
/// <summary>This method changes the point's location to
///    the given coordinates.</summary>
/// <see cref="Translate"/>
public void Move(int xor, int yor) {
    X = xor;
    Y = yor;
}

/// <summary>This method changes the point's location by
///    the given x- and y-offsets.
/// </summary>
/// <see cref="Move"/>
public void Translate(int xor, int yor) {
    X += xor;
    Y += yor;
}
```

### `<seealso>`

<span data-ttu-id="86ec6-242">Tato značka umožňuje vygenerovat položku v části Viz také.</span><span class="sxs-lookup"><span data-stu-id="86ec6-242">This tag allows an entry to be generated for the See Also section.</span></span> <span data-ttu-id="86ec6-243">Pomocí `<see>` [(`<see>`](documentation-comments.md#see)) můžete zadat odkaz v rámci textu.</span><span class="sxs-lookup"><span data-stu-id="86ec6-243">Use `<see>` ([`<see>`](documentation-comments.md#see)) to specify a link from within text.</span></span>

<span data-ttu-id="86ec6-244">__Syntaktick__</span><span class="sxs-lookup"><span data-stu-id="86ec6-244">__Syntax:__</span></span>

```xml
<seealso cref="member"/>
```

<span data-ttu-id="86ec6-245">kde `member` je název člena.</span><span class="sxs-lookup"><span data-stu-id="86ec6-245">where `member` is the name of a member.</span></span> <span data-ttu-id="86ec6-246">Generátor dokumentace kontroluje, zda daný prvek kódu existuje a mění *člena* na název elementu v generovaném souboru dokumentace.</span><span class="sxs-lookup"><span data-stu-id="86ec6-246">The documentation generator checks that the given code element exists and changes *member* to the element name in the generated documentation file.</span></span>

<span data-ttu-id="86ec6-247">__Příklad:__</span><span class="sxs-lookup"><span data-stu-id="86ec6-247">__Example:__</span></span>

```csharp
/// <summary>This method determines whether two Points have the same
///    location.</summary>
/// <seealso cref="operator=="/>
/// <seealso cref="operator!="/>
public override bool Equals(object o) {
    // ...
}
```

### `<summary>`

Tato značka se dá použít k popisu typu nebo členu typu. <span data-ttu-id="86ec6-249">Použijte `<remarks>` [(`<remarks>`](documentation-comments.md#remarks)) pro popis samotného typu.</span><span class="sxs-lookup"><span data-stu-id="86ec6-249">Use `<remarks>` ([`<remarks>`](documentation-comments.md#remarks)) to describe the type itself.</span></span>

<span data-ttu-id="86ec6-250">__Syntaktick__</span><span class="sxs-lookup"><span data-stu-id="86ec6-250">__Syntax:__</span></span>

```xml
<summary>description</summary>
```

<span data-ttu-id="86ec6-251">kde `description` je souhrn typu nebo členu.</span><span class="sxs-lookup"><span data-stu-id="86ec6-251">where `description` is a summary of the type or member.</span></span>

<span data-ttu-id="86ec6-252">__Příklad:__</span><span class="sxs-lookup"><span data-stu-id="86ec6-252">__Example:__</span></span>

```csharp
/// <summary>This constructor initializes the new Point to (0,0).</summary>
public Point() : this(0,0) {
}
```

### `<value>`

<span data-ttu-id="86ec6-253">Tato značka umožňuje popsat vlastnost.</span><span class="sxs-lookup"><span data-stu-id="86ec6-253">This tag allows a property to be described.</span></span>

<span data-ttu-id="86ec6-254">__Syntaktick__</span><span class="sxs-lookup"><span data-stu-id="86ec6-254">__Syntax:__</span></span>

```xml
<value>property description</value>
```

<span data-ttu-id="86ec6-255">kde `property description` je popis vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="86ec6-255">where `property description` is a description for the property.</span></span>

<span data-ttu-id="86ec6-256">__Příklad:__</span><span class="sxs-lookup"><span data-stu-id="86ec6-256">__Example:__</span></span>

```csharp
/// <value>Property <c>X</c> represents the point's x-coordinate.</value>
public int X
{
    get { return x; }
    set { x = value; }
}
```

### `<typeparam>`

<span data-ttu-id="86ec6-257">Tato značka se používá k popisu parametru obecného typu pro třídu, strukturu, rozhraní, delegáta nebo metodu.</span><span class="sxs-lookup"><span data-stu-id="86ec6-257">This tag is used to describe a generic type parameter for a class, struct, interface, delegate, or method.</span></span>

<span data-ttu-id="86ec6-258">__Syntaktick__</span><span class="sxs-lookup"><span data-stu-id="86ec6-258">__Syntax:__</span></span>

```xml
<typeparam name="name">description</typeparam>
```

<span data-ttu-id="86ec6-259">kde `name` je název parametru typu a `description` je jeho popis.</span><span class="sxs-lookup"><span data-stu-id="86ec6-259">where `name` is the name of the type parameter, and `description` is its description.</span></span>

<span data-ttu-id="86ec6-260">__Příklad:__</span><span class="sxs-lookup"><span data-stu-id="86ec6-260">__Example:__</span></span>

```csharp
/// <summary>A generic list class.</summary>
/// <typeparam name="T">The type stored by the list.</typeparam>
public class MyList<T> {
    ...
}
```

### `<typeparamref>`

<span data-ttu-id="86ec6-261">Tato značka slouží k označení, že slovo je parametr typu.</span><span class="sxs-lookup"><span data-stu-id="86ec6-261">This tag is used to indicate that a word is a type parameter.</span></span> <span data-ttu-id="86ec6-262">Soubor dokumentace může být zpracován pro naformátování tohoto parametru typu nějakým odlišným způsobem.</span><span class="sxs-lookup"><span data-stu-id="86ec6-262">The documentation file can be processed to format this type parameter in some distinct way.</span></span>

<span data-ttu-id="86ec6-263">__Syntaktick__</span><span class="sxs-lookup"><span data-stu-id="86ec6-263">__Syntax:__</span></span>

```xml
<typeparamref name="name"/>
```

<span data-ttu-id="86ec6-264">kde `name` je název parametru typu.</span><span class="sxs-lookup"><span data-stu-id="86ec6-264">where `name` is the name of the type parameter.</span></span>

<span data-ttu-id="86ec6-265">__Příklad:__</span><span class="sxs-lookup"><span data-stu-id="86ec6-265">__Example:__</span></span>

```csharp
/// <summary>This method fetches data and returns a list of <typeparamref name="T"/>.</summary>
/// <param name="query">query to execute</param>
public List<T> FetchData<T>(string query) {
    ...
}
```

## <a name="processing-the-documentation-file"></a><span data-ttu-id="86ec6-266">Zpracovává se soubor dokumentace.</span><span class="sxs-lookup"><span data-stu-id="86ec6-266">Processing the documentation file</span></span>

<span data-ttu-id="86ec6-267">Generátor dokumentace generuje řetězec ID pro každý prvek ve zdrojovém kódu, který je označen komentářem k dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="86ec6-267">The documentation generator generates an ID string for each element in the source code that is tagged with a documentation comment.</span></span> <span data-ttu-id="86ec6-268">Tento řetězec IDENTIFIKÁTORu jednoznačně identifikuje zdrojový element.</span><span class="sxs-lookup"><span data-stu-id="86ec6-268">This ID string uniquely identifies a source element.</span></span> <span data-ttu-id="86ec6-269">Prohlížeč dokumentace může použít řetězec ID k identifikaci odpovídající položky metadat nebo reflexe, na kterou se dokumentace vztahuje.</span><span class="sxs-lookup"><span data-stu-id="86ec6-269">A documentation viewer can use an ID string to identify the corresponding metadata/reflection item to which the documentation applies.</span></span>

<span data-ttu-id="86ec6-270">Soubor dokumentace není hierarchická reprezentace zdrojového kódu; místo toho je seznam bez stromové struktury s generovaným řetězcem ID pro každý prvek.</span><span class="sxs-lookup"><span data-stu-id="86ec6-270">The documentation file is not a hierarchical representation of the source code; rather, it is a flat list with a generated ID string for each element.</span></span>

### <a name="id-string-format"></a><span data-ttu-id="86ec6-271">Formát řetězce ID</span><span class="sxs-lookup"><span data-stu-id="86ec6-271">ID string format</span></span>

<span data-ttu-id="86ec6-272">Generátor dokumentace při generování řetězců ID dodržuje následující pravidla:</span><span class="sxs-lookup"><span data-stu-id="86ec6-272">The documentation generator observes the following rules when it generates the ID strings:</span></span>

*  <span data-ttu-id="86ec6-273">V řetězci není vložen prázdný znak.</span><span class="sxs-lookup"><span data-stu-id="86ec6-273">No white space is placed in the string.</span></span>

*  <span data-ttu-id="86ec6-274">První část řetězce identifikuje druh dokumentovaného člena přes jeden znak následovaný dvojtečkou.</span><span class="sxs-lookup"><span data-stu-id="86ec6-274">The first part of the string identifies the kind of member being documented, via a single character followed by a colon.</span></span> <span data-ttu-id="86ec6-275">Jsou definovány následující typy členů:</span><span class="sxs-lookup"><span data-stu-id="86ec6-275">The following kinds of members are defined:</span></span>

   | <span data-ttu-id="86ec6-276">__Optické__</span><span class="sxs-lookup"><span data-stu-id="86ec6-276">__Character__</span></span> | <span data-ttu-id="86ec6-277">__Popis__</span><span class="sxs-lookup"><span data-stu-id="86ec6-277">__Description__</span></span>                                             |
   |---------------|-------------------------------------------------------------|
   | <span data-ttu-id="86ec6-278">E</span><span class="sxs-lookup"><span data-stu-id="86ec6-278">E</span></span>             | <span data-ttu-id="86ec6-279">Událost</span><span class="sxs-lookup"><span data-stu-id="86ec6-279">Event</span></span>                                                       |
   | <span data-ttu-id="86ec6-280">F</span><span class="sxs-lookup"><span data-stu-id="86ec6-280">F</span></span>             | <span data-ttu-id="86ec6-281">Pole</span><span class="sxs-lookup"><span data-stu-id="86ec6-281">Field</span></span>                                                       |
   | <span data-ttu-id="86ec6-282">M</span><span class="sxs-lookup"><span data-stu-id="86ec6-282">M</span></span>             | <span data-ttu-id="86ec6-283">Metoda (včetně konstruktorů, destruktorů a operátorů)</span><span class="sxs-lookup"><span data-stu-id="86ec6-283">Method (including constructors, destructors, and operators)</span></span> |
   | <span data-ttu-id="86ec6-284">Ne</span><span class="sxs-lookup"><span data-stu-id="86ec6-284">N</span></span>             | <span data-ttu-id="86ec6-285">Obor názvů</span><span class="sxs-lookup"><span data-stu-id="86ec6-285">Namespace</span></span>                                                   |
   | <span data-ttu-id="86ec6-286">P</span><span class="sxs-lookup"><span data-stu-id="86ec6-286">P</span></span>             | <span data-ttu-id="86ec6-287">Vlastnost (včetně indexerů)</span><span class="sxs-lookup"><span data-stu-id="86ec6-287">Property (including indexers)</span></span>                               |
   | <span data-ttu-id="86ec6-288">T</span><span class="sxs-lookup"><span data-stu-id="86ec6-288">T</span></span>             | <span data-ttu-id="86ec6-289">Typ (například třída, delegát, výčet, rozhraní a struktura)</span><span class="sxs-lookup"><span data-stu-id="86ec6-289">Type (such as class, delegate, enum, interface, and struct)</span></span> |
   | <span data-ttu-id="86ec6-290">!</span><span class="sxs-lookup"><span data-stu-id="86ec6-290">!</span></span>             | <span data-ttu-id="86ec6-291">Chybový řetězec; zbytek řetězce poskytuje informace o chybě.</span><span class="sxs-lookup"><span data-stu-id="86ec6-291">Error string; the rest of the string provides information about the error.</span></span> <span data-ttu-id="86ec6-292">Například generátor dokumentace generuje informace o chybě pro odkazy, které nelze přeložit.</span><span class="sxs-lookup"><span data-stu-id="86ec6-292">For example, the documentation generator generates error information for links that cannot be resolved.</span></span> |

*  <span data-ttu-id="86ec6-293">Druhá část řetězce je plně kvalifikovaný název elementu začínajícího v kořenu oboru názvů.</span><span class="sxs-lookup"><span data-stu-id="86ec6-293">The second part of the string is the fully qualified name of the element, starting at the root of the namespace.</span></span> <span data-ttu-id="86ec6-294">Název elementu, jeho uzavírací typ (y) a obor názvů jsou odděleny tečkami.</span><span class="sxs-lookup"><span data-stu-id="86ec6-294">The name of the element, its enclosing type(s), and namespace are separated by periods.</span></span> <span data-ttu-id="86ec6-295">Pokud má název položky vlastní tečky, nahradí `#(U+0023)` se znaky.</span><span class="sxs-lookup"><span data-stu-id="86ec6-295">If the name of the item itself has periods, they are replaced by `#(U+0023)` characters.</span></span> <span data-ttu-id="86ec6-296">(Předpokládá se, že žádný element nemá tento znak v názvu.)</span><span class="sxs-lookup"><span data-stu-id="86ec6-296">(It is assumed that no element has this character in its name.)</span></span>
*  <span data-ttu-id="86ec6-297">Pro metody a vlastnosti s argumenty následuje seznam argumentů uzavřený v závorkách.</span><span class="sxs-lookup"><span data-stu-id="86ec6-297">For methods and properties with arguments, the argument list follows, enclosed in parentheses.</span></span> <span data-ttu-id="86ec6-298">V případě bez argumentů jsou závorky vynechány.</span><span class="sxs-lookup"><span data-stu-id="86ec6-298">For those without arguments, the parentheses are omitted.</span></span> <span data-ttu-id="86ec6-299">Argumenty jsou odděleny čárkami.</span><span class="sxs-lookup"><span data-stu-id="86ec6-299">The arguments are separated by commas.</span></span> <span data-ttu-id="86ec6-300">Kódování každého argumentu je stejné jako signatura CLI následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="86ec6-300">The encoding of each argument is the same as a CLI signature, as follows:</span></span>
   *  <span data-ttu-id="86ec6-301">Argumenty jsou reprezentovány podle názvu jejich dokumentace, který je založen na jejich plně kvalifikovaném názvu, který je upraven následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="86ec6-301">Arguments are represented by their documentation name, which is based on their fully qualified name, modified as follows:</span></span>
      * <span data-ttu-id="86ec6-302">Argumenty, které reprezentují obecné typy, `` ` `` mají připojený znak (zpětný) následovaný počtem parametrů typu.</span><span class="sxs-lookup"><span data-stu-id="86ec6-302">Arguments that represent generic types have an appended `` ` `` (backtick) character followed by the number of type parameters</span></span>
      * <span data-ttu-id="86ec6-303">Argumenty, `out` které mají `ref` modifikátor or, `@` mají následující název typu.</span><span class="sxs-lookup"><span data-stu-id="86ec6-303">Arguments having the `out` or `ref` modifier have an `@` following their type name.</span></span> <span data-ttu-id="86ec6-304">Argumenty předané hodnotou nebo prostřednictvím `params` nemají žádný speciální zápis.</span><span class="sxs-lookup"><span data-stu-id="86ec6-304">Arguments passed by value or via `params` have no special notation.</span></span>
      * <span data-ttu-id="86ec6-305">Argumenty, které jsou pole, jsou `[lowerbound:size, ... , lowerbound:size]` reprezentovány, kde počet čárek je méně než jedna a dolní meze a velikost jednotlivých dimenzí, pokud jsou známy, jsou reprezentovány v desítkové soustavě.</span><span class="sxs-lookup"><span data-stu-id="86ec6-305">Arguments that are arrays are represented as `[lowerbound:size, ... , lowerbound:size]` where the number of commas is the rank less one, and the lower bounds and size of each dimension, if known, are represented in decimal.</span></span> <span data-ttu-id="86ec6-306">Pokud není zadaná dolní mez nebo velikost, je vynechána.</span><span class="sxs-lookup"><span data-stu-id="86ec6-306">If a lower bound or size is not specified, it is omitted.</span></span> <span data-ttu-id="86ec6-307">Pokud je spodní mez a velikost pro konkrétní dimenzi vynechána, `:` je vynechána také hodnota.</span><span class="sxs-lookup"><span data-stu-id="86ec6-307">If the lower bound and size for a particular dimension are omitted, the `:` is omitted as well.</span></span> <span data-ttu-id="86ec6-308">Vícenásobná pole jsou reprezentována `[]` jednou na úrovni.</span><span class="sxs-lookup"><span data-stu-id="86ec6-308">Jagged arrays are represented by one `[]` per level.</span></span>
      * <span data-ttu-id="86ec6-309">Argumenty, které mají jiné typy ukazatelů než void, jsou reprezentovány pomocí `*` následujícího názvu typu.</span><span class="sxs-lookup"><span data-stu-id="86ec6-309">Arguments that have pointer types other than void are represented using a `*` following the type name.</span></span> <span data-ttu-id="86ec6-310">Ukazatel void je reprezentován pomocí názvu `System.Void`typu.</span><span class="sxs-lookup"><span data-stu-id="86ec6-310">A void pointer is represented using a type name of `System.Void`.</span></span>
      * <span data-ttu-id="86ec6-311">Argumenty, které odkazují na parametry obecného typu definované u typů, jsou zakódovány pomocí `` ` `` znaku (počátečního) následovaného indexem parametru typu s nulovým základem.</span><span class="sxs-lookup"><span data-stu-id="86ec6-311">Arguments that refer to generic type parameters defined on types are encoded using the `` ` `` (backtick) character followed by the zero-based index of the type parameter.</span></span>
      * <span data-ttu-id="86ec6-312">Argumenty, které používají parametry obecného typu definované v metodách, používají dvojité přetržení ``` `` ``` namísto `` ` `` použití pro typy.</span><span class="sxs-lookup"><span data-stu-id="86ec6-312">Arguments that use generic type parameters defined in methods use a double-backtick ``` `` ``` instead of the `` ` `` used for types.</span></span>
      * <span data-ttu-id="86ec6-313">Argumenty, které odkazují na konstruované obecné typy, jsou zakódovány pomocí obecného typu `{`následovaného čárkami odděleným seznamem argumentů typu a `}`následovány.</span><span class="sxs-lookup"><span data-stu-id="86ec6-313">Arguments that refer to constructed generic types are encoded using the generic type, followed by `{`, followed by a comma-separated list of type arguments, followed by `}`.</span></span>

### <a name="id-string-examples"></a><span data-ttu-id="86ec6-314">Příklady řetězců ID</span><span class="sxs-lookup"><span data-stu-id="86ec6-314">ID string examples</span></span>

<span data-ttu-id="86ec6-315">Následující příklady každé ukazuje fragment C# kódu, společně s řetězcem ID vytvořeným z každého zdrojového elementu, který může mít komentář k dokumentaci:</span><span class="sxs-lookup"><span data-stu-id="86ec6-315">The following examples each show a fragment of C# code, along with the ID string produced from each source element capable of having a documentation comment:</span></span>

*  <span data-ttu-id="86ec6-316">Typy jsou reprezentovány pomocí jejich plně kvalifikovaného názvu, rozšiřované o obecné informace:</span><span class="sxs-lookup"><span data-stu-id="86ec6-316">Types are represented using their fully qualified name, augmented with generic information:</span></span>

   ```csharp
   enum Color { Red, Blue, Green }

   namespace Acme
   {
       interface IProcess {...}

       struct ValueType {...}

       class Widget: IProcess
       {
           public class NestedClass {...}
           public interface IMenuItem {...}
           public delegate void Del(int i);
           public enum Direction { North, South, East, West }
       }

       class MyList<T>
       {
           class Helper<U,V> {...}
       }
   }

   "T:Color"
   "T:Acme.IProcess"
   "T:Acme.ValueType"
   "T:Acme.Widget"
   "T:Acme.Widget.NestedClass"
   "T:Acme.Widget.IMenuItem"
   "T:Acme.Widget.Del"
   "T:Acme.Widget.Direction"
   "T:Acme.MyList`1"
   "T:Acme.MyList`1.Helper`2"
   ```

*  <span data-ttu-id="86ec6-317">Pole jsou reprezentována jejich plně kvalifikovaným názvem:</span><span class="sxs-lookup"><span data-stu-id="86ec6-317">Fields are represented by their fully qualified name:</span></span>

   ```csharp
   namespace Acme
   {
       struct ValueType
       {
           private int total;
       }
   
       class Widget: IProcess
       {
           public class NestedClass
           {
               private int value;
           }
   
           private string message;
           private static Color defaultColor;
           private const double PI = 3.14159;
           protected readonly double monthlyAverage;
           private long[] array1;
           private Widget[,] array2;
           private unsafe int *pCount;
           private unsafe float **ppValues;
       }
   }

   "F:Acme.ValueType.total"
   "F:Acme.Widget.NestedClass.value"
   "F:Acme.Widget.message"
   "F:Acme.Widget.defaultColor"
   "F:Acme.Widget.PI"
   "F:Acme.Widget.monthlyAverage"
   "F:Acme.Widget.array1"
   "F:Acme.Widget.array2"
   "F:Acme.Widget.pCount"
   "F:Acme.Widget.ppValues"
   ```

*  <span data-ttu-id="86ec6-318">Konstruktory.</span><span class="sxs-lookup"><span data-stu-id="86ec6-318">Constructors.</span></span>

   ```csharp
   namespace Acme
   {
       class Widget: IProcess
       {
           static Widget() {...}
           public Widget() {...}
           public Widget(string s) {...}
       }
   }

   "M:Acme.Widget.#cctor"
   "M:Acme.Widget.#ctor"
   "M:Acme.Widget.#ctor(System.String)"
   ```

*  <span data-ttu-id="86ec6-319">Destruktory.</span><span class="sxs-lookup"><span data-stu-id="86ec6-319">Destructors.</span></span>

   ```csharp
   namespace Acme
   {
       class Widget: IProcess
       {
           ~Widget() {...}
       }
   }
   
   "M:Acme.Widget.Finalize"
   ```

*  <span data-ttu-id="86ec6-320">Způsobů.</span><span class="sxs-lookup"><span data-stu-id="86ec6-320">Methods.</span></span>

   ```csharp
   namespace Acme
   {
       struct ValueType
       {
           public void M(int i) {...}
       }

       class Widget: IProcess
       {
           public class NestedClass
           {
               public void M(int i) {...}
           }

           public static void M0() {...}
           public void M1(char c, out float f, ref ValueType v) {...}
           public void M2(short[] x1, int[,] x2, long[][] x3) {...}
           public void M3(long[][] x3, Widget[][,,] x4) {...}
           public unsafe void M4(char *pc, Color **pf) {...}
           public unsafe void M5(void *pv, double *[][,] pd) {...}
           public void M6(int i, params object[] args) {...}
       }

       class MyList<T>
       {
           public void Test(T t) { }
       }

       class UseList
       {
           public void Process(MyList<int> list) { }
           public MyList<T> GetValues<T>(T inputValue) { return null; }
       }
   }

   "M:Acme.ValueType.M(System.Int32)"
   "M:Acme.Widget.NestedClass.M(System.Int32)"
   "M:Acme.Widget.M0"
   "M:Acme.Widget.M1(System.Char,System.Single@,Acme.ValueType@)"
   "M:Acme.Widget.M2(System.Int16[],System.Int32[0:,0:],System.Int64[][])"
   "M:Acme.Widget.M3(System.Int64[][],Acme.Widget[0:,0:,0:][])"
   "M:Acme.Widget.M4(System.Char*,Color**)"
   "M:Acme.Widget.M5(System.Void*,System.Double*[0:,0:][])"
   "M:Acme.Widget.M6(System.Int32,System.Object[])"
   "M:Acme.MyList`1.Test(`0)"
   "M:Acme.UseList.Process(Acme.MyList{System.Int32})"
   "M:Acme.UseList.GetValues``(``0)"
   ```

*  <span data-ttu-id="86ec6-321">Vlastnosti a indexery.</span><span class="sxs-lookup"><span data-stu-id="86ec6-321">Properties and indexers.</span></span>

   ```csharp
   namespace Acme
   {
       class Widget: IProcess
       {
           public int Width { get {...} set {...} }
           public int this[int i] { get {...} set {...} }
           public int this[string s, int i] { get {...} set {...} }
       }
   }

   "P:Acme.Widget.Width"
   "P:Acme.Widget.Item(System.Int32)"
   "P:Acme.Widget.Item(System.String,System.Int32)"
   ```

*  <span data-ttu-id="86ec6-322">Událost.</span><span class="sxs-lookup"><span data-stu-id="86ec6-322">Events.</span></span>

   ```csharp
   namespace Acme
   {
       class Widget: IProcess
       {
           public event Del AnEvent;
       }
   }

   "E:Acme.Widget.AnEvent"
   ```

*  <span data-ttu-id="86ec6-323">Unární operátory.</span><span class="sxs-lookup"><span data-stu-id="86ec6-323">Unary operators.</span></span>

   ```csharp
   namespace Acme
   {
       class Widget: IProcess
       {
           public static Widget operator+(Widget x) {...}
       }
   }

   "M:Acme.Widget.op_UnaryPlus(Acme.Widget)"
   ```

   <span data-ttu-id="86ec6-324">Kompletní sada názvů funkcí unárního operátoru je `op_UnaryPlus`následující:, `op_UnaryNegation`, `op_LogicalNot`, `op_Decrement` `op_Increment` `op_OnesComplement`,,, `op_True` `op_False`a.</span><span class="sxs-lookup"><span data-stu-id="86ec6-324">The complete set of unary operator function names used is as follows: `op_UnaryPlus`, `op_UnaryNegation`, `op_LogicalNot`, `op_OnesComplement`, `op_Increment`, `op_Decrement`, `op_True`, and `op_False`.</span></span>

*  <span data-ttu-id="86ec6-325">Binární operátory.</span><span class="sxs-lookup"><span data-stu-id="86ec6-325">Binary operators.</span></span>

   ```csharp
   namespace Acme
   {
       class Widget: IProcess
       {
           public static Widget operator+(Widget x1, Widget x2) {...}
       }
   }

   "M:Acme.Widget.op_Addition(Acme.Widget,Acme.Widget)"
   ```

   <span data-ttu-id="86ec6-326">`op_Addition`Kompletní sada názvů funkcí binárního operátoru je následující:, `op_LeftShift` `op_BitwiseOr` `op_Multiply` `op_BitwiseAnd` `op_Division` `op_Subtraction`,,, `op_Modulus`,,, `op_ExclusiveOr`,, `op_RightShift`, `op_Equality`, ,,`op_LessThan`, a`op_GreaterThan`. `op_LessThanOrEqual` `op_Inequality` `op_GreaterThanOrEqual`</span><span class="sxs-lookup"><span data-stu-id="86ec6-326">The complete set of binary operator function names used is as follows: `op_Addition`, `op_Subtraction`, `op_Multiply`, `op_Division`, `op_Modulus`, `op_BitwiseAnd`, `op_BitwiseOr`, `op_ExclusiveOr`, `op_LeftShift`, `op_RightShift`, `op_Equality`, `op_Inequality`, `op_LessThan`, `op_LessThanOrEqual`, `op_GreaterThan`, and `op_GreaterThanOrEqual`.</span></span>

*  <span data-ttu-id="86ec6-327">Operátory převodu mají koncový znak`~`"" následovaný návratovým typem.</span><span class="sxs-lookup"><span data-stu-id="86ec6-327">Conversion operators have a trailing "`~`" followed by the return type.</span></span>

   ```csharp
   namespace Acme
   {
       class Widget: IProcess
       {
           public static explicit operator int(Widget x) {...}
           public static implicit operator long(Widget x) {...}
       }
   }

   "M:Acme.Widget.op_Explicit(Acme.Widget)~System.Int32"
   "M:Acme.Widget.op_Implicit(Acme.Widget)~System.Int64"
   ```

## <a name="an-example"></a><span data-ttu-id="86ec6-328">Příklad</span><span class="sxs-lookup"><span data-stu-id="86ec6-328">An example</span></span>

### <a name="c-source-code"></a><span data-ttu-id="86ec6-329">C#zdrojový kód</span><span class="sxs-lookup"><span data-stu-id="86ec6-329">C# source code</span></span>

<span data-ttu-id="86ec6-330">Následující příklad ukazuje zdrojový kód `Point` třídy:</span><span class="sxs-lookup"><span data-stu-id="86ec6-330">The following example shows the source code of a `Point` class:</span></span>

```csharp
namespace Graphics
{

/// <summary>Class <c>Point</c> models a point in a two-dimensional plane.
/// </summary>
public class Point 
{

    /// <summary>Instance variable <c>x</c> represents the point's
    ///    x-coordinate.</summary>
    private int x;

    /// <summary>Instance variable <c>y</c> represents the point's
    ///    y-coordinate.</summary>
    private int y;

    /// <value>Property <c>X</c> represents the point's x-coordinate.</value>
    public int X
    {
        get { return x; }
        set { x = value; }
    }

    /// <value>Property <c>Y</c> represents the point's y-coordinate.</value>
    public int Y
    {
        get { return y; }
        set { y = value; }
    }

    /// <summary>This constructor initializes the new Point to
    ///    (0,0).</summary>
    public Point() : this(0,0) {}

    /// <summary>This constructor initializes the new Point to
    ///    (<paramref name="xor"/>,<paramref name="yor"/>).</summary>
    /// <param><c>xor</c> is the new Point's x-coordinate.</param>
    /// <param><c>yor</c> is the new Point's y-coordinate.</param>
    public Point(int xor, int yor) {
        X = xor;
        Y = yor;
    }

    /// <summary>This method changes the point's location to
    ///    the given coordinates.</summary>
    /// <param><c>xor</c> is the new x-coordinate.</param>
    /// <param><c>yor</c> is the new y-coordinate.</param>
    /// <see cref="Translate"/>
    public void Move(int xor, int yor) {
        X = xor;
        Y = yor;
    }

    /// <summary>This method changes the point's location by
    ///    the given x- and y-offsets.
    /// <example>For example:
    /// <code>
    ///    Point p = new Point(3,5);
    ///    p.Translate(-1,3);
    /// </code>
    /// results in <c>p</c>'s having the value (2,8).
    /// </example>
    /// </summary>
    /// <param><c>xor</c> is the relative x-offset.</param>
    /// <param><c>yor</c> is the relative y-offset.</param>
    /// <see cref="Move"/>
    public void Translate(int xor, int yor) {
        X += xor;
        Y += yor;
    }

    /// <summary>This method determines whether two Points have the same
    ///    location.</summary>
    /// <param><c>o</c> is the object to be compared to the current object.
    /// </param>
    /// <returns>True if the Points have the same location and they have
    ///    the exact same type; otherwise, false.</returns>
    /// <seealso cref="operator=="/>
    /// <seealso cref="operator!="/>
    public override bool Equals(object o) {
        if (o == null) {
            return false;
        }

        if (this == o) {
            return true;
        }

        if (GetType() == o.GetType()) {
            Point p = (Point)o;
            return (X == p.X) && (Y == p.Y);
        }
        return false;
    }

    /// <summary>Report a point's location as a string.</summary>
    /// <returns>A string representing a point's location, in the form (x,y),
    ///    without any leading, training, or embedded whitespace.</returns>
    public override string ToString() {
        return "(" + X + "," + Y + ")";
    }

    /// <summary>This operator determines whether two Points have the same
    ///    location.</summary>
    /// <param><c>p1</c> is the first Point to be compared.</param>
    /// <param><c>p2</c> is the second Point to be compared.</param>
    /// <returns>True if the Points have the same location and they have
    ///    the exact same type; otherwise, false.</returns>
    /// <seealso cref="Equals"/>
    /// <seealso cref="operator!="/>
    public static bool operator==(Point p1, Point p2) {
        if ((object)p1 == null || (object)p2 == null) {
            return false;
        }

        if (p1.GetType() == p2.GetType()) {
            return (p1.X == p2.X) && (p1.Y == p2.Y);
        }

        return false;
    }

    /// <summary>This operator determines whether two Points have the same
    ///    location.</summary>
    /// <param><c>p1</c> is the first Point to be compared.</param>
    /// <param><c>p2</c> is the second Point to be compared.</param>
    /// <returns>True if the Points do not have the same location and the
    ///    exact same type; otherwise, false.</returns>
    /// <seealso cref="Equals"/>
    /// <seealso cref="operator=="/>
    public static bool operator!=(Point p1, Point p2) {
        return !(p1 == p2);
    }

    /// <summary>This is the entry point of the Point class testing
    /// program.
    /// <para>This program tests each method and operator, and
    /// is intended to be run after any non-trivial maintenance has
    /// been performed on the Point class.</para></summary>
    public static void Main() {
        // class test code goes here
    }
}
}
```

### <a name="resulting-xml"></a><span data-ttu-id="86ec6-331">Výsledný kód XML</span><span class="sxs-lookup"><span data-stu-id="86ec6-331">Resulting XML</span></span>

<span data-ttu-id="86ec6-332">Zde je výstup vyprodukovaný jedním generátorem dokumentace, pokud je daný zdrojový kód třídy `Point`, zobrazený výše:</span><span class="sxs-lookup"><span data-stu-id="86ec6-332">Here is the output produced by one documentation generator when given the source code for class `Point`, shown above:</span></span>

```xml
<?xml version="1.0"?>
<doc>
    <assembly>
        <name>Point</name>
    </assembly>
    <members>
        <member name="T:Graphics.Point">
            <summary>Class <c>Point</c> models a point in a two-dimensional
            plane.
            </summary>
        </member>

        <member name="F:Graphics.Point.x">
            <summary>Instance variable <c>x</c> represents the point's
            x-coordinate.</summary>
        </member>

        <member name="F:Graphics.Point.y">
            <summary>Instance variable <c>y</c> represents the point's
            y-coordinate.</summary>
        </member>

        <member name="M:Graphics.Point.#ctor">
            <summary>This constructor initializes the new Point to
        (0,0).</summary>
        </member>

        <member name="M:Graphics.Point.#ctor(System.Int32,System.Int32)">
            <summary>This constructor initializes the new Point to
            (<paramref name="xor"/>,<paramref name="yor"/>).</summary>
            <param><c>xor</c> is the new Point's x-coordinate.</param>
            <param><c>yor</c> is the new Point's y-coordinate.</param>
        </member>

        <member name="M:Graphics.Point.Move(System.Int32,System.Int32)">
            <summary>This method changes the point's location to
            the given coordinates.</summary>
            <param><c>xor</c> is the new x-coordinate.</param>
            <param><c>yor</c> is the new y-coordinate.</param>
            <see cref="M:Graphics.Point.Translate(System.Int32,System.Int32)"/>
        </member>

        <member
            name="M:Graphics.Point.Translate(System.Int32,System.Int32)">
            <summary>This method changes the point's location by
            the given x- and y-offsets.
            <example>For example:
            <code>
            Point p = new Point(3,5);
            p.Translate(-1,3);
            </code>
            results in <c>p</c>'s having the value (2,8).
            </example>
            </summary>
            <param><c>xor</c> is the relative x-offset.</param>
            <param><c>yor</c> is the relative y-offset.</param>
            <see cref="M:Graphics.Point.Move(System.Int32,System.Int32)"/>
        </member>

        <member name="M:Graphics.Point.Equals(System.Object)">
            <summary>This method determines whether two Points have the same
            location.</summary>
            <param><c>o</c> is the object to be compared to the current
            object.
            </param>
            <returns>True if the Points have the same location and they have
            the exact same type; otherwise, false.</returns>
            <seealso
      cref="M:Graphics.Point.op_Equality(Graphics.Point,Graphics.Point)"/>
            <seealso
      cref="M:Graphics.Point.op_Inequality(Graphics.Point,Graphics.Point)"/>
        </member>

        <member name="M:Graphics.Point.ToString">
            <summary>Report a point's location as a string.</summary>
            <returns>A string representing a point's location, in the form
            (x,y),
            without any leading, training, or embedded whitespace.</returns>
        </member>

        <member
       name="M:Graphics.Point.op_Equality(Graphics.Point,Graphics.Point)">
            <summary>This operator determines whether two Points have the
            same
            location.</summary>
            <param><c>p1</c> is the first Point to be compared.</param>
            <param><c>p2</c> is the second Point to be compared.</param>
            <returns>True if the Points have the same location and they have
            the exact same type; otherwise, false.</returns>
            <seealso cref="M:Graphics.Point.Equals(System.Object)"/>
            <seealso
     cref="M:Graphics.Point.op_Inequality(Graphics.Point,Graphics.Point)"/>
        </member>

        <member
      name="M:Graphics.Point.op_Inequality(Graphics.Point,Graphics.Point)">
            <summary>This operator determines whether two Points have the
            same
            location.</summary>
            <param><c>p1</c> is the first Point to be compared.</param>
            <param><c>p2</c> is the second Point to be compared.</param>
            <returns>True if the Points do not have the same location and
            the
            exact same type; otherwise, false.</returns>
            <seealso cref="M:Graphics.Point.Equals(System.Object)"/>
            <seealso
      cref="M:Graphics.Point.op_Equality(Graphics.Point,Graphics.Point)"/>
        </member>

        <member name="M:Graphics.Point.Main">
            <summary>This is the entry point of the Point class testing
            program.
            <para>This program tests each method and operator, and
            is intended to be run after any non-trivial maintenance has
            been performed on the Point class.</para></summary>
        </member>

        <member name="P:Graphics.Point.X">
            <value>Property <c>X</c> represents the point's
            x-coordinate.</value>
        </member>

        <member name="P:Graphics.Point.Y">
            <value>Property <c>Y</c> represents the point's
            y-coordinate.</value>
        </member>
    </members>
</doc>
```
