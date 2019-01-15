# <a name="documentation-comments"></a><span data-ttu-id="13797-101">Dokumentační komentáře</span><span class="sxs-lookup"><span data-stu-id="13797-101">Documentation comments</span></span>

<span data-ttu-id="13797-102">C# poskytuje mechanismus pro programátory zdokumentujte svůj kód pomocí syntaxe speciální komentář, který obsahuje XML text.</span><span class="sxs-lookup"><span data-stu-id="13797-102">C# provides a mechanism for programmers to document their code using a special comment syntax that contains XML text.</span></span> <span data-ttu-id="13797-103">V souborech zdrojového kódu komentáře s formuláři určité umožňuje přímé nástroj k vytvoření XML z těchto komentářů a zdrojové elementy kódu, které jsou předcházet.</span><span class="sxs-lookup"><span data-stu-id="13797-103">In source code files, comments having a certain form can be used to direct a tool to produce XML from those comments and the source code elements, which they precede.</span></span> <span data-ttu-id="13797-104">Komentáře pomocí těchto syntaxe se nazývají ***komentáře k dokumentaci***.</span><span class="sxs-lookup"><span data-stu-id="13797-104">Comments using such syntax are called ***documentation comments***.</span></span> <span data-ttu-id="13797-105">Musí bezprostředně předcházet uživatelem definovaný typ (například třída, delegát nebo rozhraní) nebo jako člen (například pole, události, vlastnost nebo metoda).</span><span class="sxs-lookup"><span data-stu-id="13797-105">They must immediately precede a user-defined type (such as a class, delegate, or interface) or a member (such as a field, event, property, or method).</span></span> <span data-ttu-id="13797-106">Nástroj pro generování XML je volána ***dokumentaci generátor***.</span><span class="sxs-lookup"><span data-stu-id="13797-106">The XML generation tool is called the ***documentation generator***.</span></span> <span data-ttu-id="13797-107">(Tento generátor může být, ale nemusí být kompilátor jazyka C#, samotný.) Výstup vytvořené generátorem dokumentace je volána ***soubor dokumentace***.</span><span class="sxs-lookup"><span data-stu-id="13797-107">(This generator could be, but need not be, the C# compiler itself.) The output produced by the documentation generator is called the ***documentation file***.</span></span> <span data-ttu-id="13797-108">Soubor dokumentace se používá jako vstup pro ***dokumentaci prohlížeč***; nástroj určených k vytvoření nějaký druh vizuálního zobrazení informací o typu a její související dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="13797-108">A documentation file is used as input to a ***documentation viewer***; a tool intended to produce some sort of visual display of type information and its associated documentation.</span></span>

<span data-ttu-id="13797-109">Tato specifikace navrhuje sadu značky pro dokumentační komentáře, ale použijte tyto značky se nevyžaduje a další značky mohou být použity v případě potřeby, následovaných dlouho pravidla ve správném formátu XML.</span><span class="sxs-lookup"><span data-stu-id="13797-109">This specification suggests a set of tags to be used in documentation comments, but use of these tags is not required, and other tags may be used if desired, as long the rules of well-formed XML are followed.</span></span>

## <a name="introduction"></a><span data-ttu-id="13797-110">Úvod</span><span class="sxs-lookup"><span data-stu-id="13797-110">Introduction</span></span>

<span data-ttu-id="13797-111">Komentáře, které mají speciální tvar slouží ke směrování nástroj k vytvoření XML z těchto komentářů a zdrojové elementy kódu, které jsou předcházet.</span><span class="sxs-lookup"><span data-stu-id="13797-111">Comments having a special form can be used to direct a tool to produce XML from those comments and the source code elements, which they precede.</span></span> <span data-ttu-id="13797-112">Takové komentáře jsou Jednořádkové komentáře, které začínají s třemi lomítky (`///`), nebo oddělených komentáře, které začínají lomítkem a dvě hvězdičky (`/**`).</span><span class="sxs-lookup"><span data-stu-id="13797-112">Such comments are single-line comments that start with three slashes (`///`), or delimited comments that start with a slash and two stars (`/**`).</span></span> <span data-ttu-id="13797-113">Musí bezprostředně předcházet uživatelem definovaný typ (například třída, delegát nebo rozhraní) nebo člen (například pole, události, vlastnost nebo metoda), který se opatřovat je poznámkami.</span><span class="sxs-lookup"><span data-stu-id="13797-113">They must immediately precede a user-defined type (such as a class, delegate, or interface) or a member (such as a field, event, property, or method) that they annotate.</span></span> <span data-ttu-id="13797-114">Atribut oddíly ([specifikace atributu](attributes.md#attribute-specification)) jsou považovány za součást deklarace, takže komentáře dokumentace musí předcházet atributy použité na typ nebo člen.</span><span class="sxs-lookup"><span data-stu-id="13797-114">Attribute sections ([Attribute specification](attributes.md#attribute-specification)) are considered part of declarations, so documentation comments must precede attributes applied to a type or member.</span></span>

<span data-ttu-id="13797-115">__Syntaxe:__</span><span class="sxs-lookup"><span data-stu-id="13797-115">__Syntax:__</span></span>

```antlr
single_line_doc_comment
    : '///' input_character*
    ;

delimited_doc_comment
    : '/**' delimited_comment_section* asterisk+ '/'
    ;
```

<span data-ttu-id="13797-116">V *single_line_doc_comment*, pokud je *prázdné znaky* následující znak `///` znaků ve všech *single_line_doc_comment*s sousední na aktuální *single_line_doc_comment*, pak, která *prázdné znaky* znak není součástí výstupu XML.</span><span class="sxs-lookup"><span data-stu-id="13797-116">In a *single_line_doc_comment*, if there is a *whitespace* character following the `///` characters on each of the *single_line_doc_comment*s adjacent to the current *single_line_doc_comment*, then that *whitespace* character is not included in the XML output.</span></span>

<span data-ttu-id="13797-117">V oddělených doc – komentáři Pokud je první neprázdný znak na druhém řádku hvězdičku a stejný vzor nepovinné prázdné znaky a znak hvězdičky se opakuje na začátku každého řádku v oddělených doc – komentáře potom znaků opakované vzoru nejsou součástí výstupu XML.</span><span class="sxs-lookup"><span data-stu-id="13797-117">In a delimited-doc-comment, if the first non-whitespace character on the second line is an asterisk and the same pattern of optional whitespace characters and an asterisk character is repeated at the beginning of each of the line within the delimited-doc-comment, then the characters of the repeated pattern are not included in the XML output.</span></span> <span data-ttu-id="13797-118">Vzor může obsahovat prázdné znaky, po, stejně jako před znak hvězdičky.</span><span class="sxs-lookup"><span data-stu-id="13797-118">The pattern may include whitespace characters after, as well as before, the asterisk character.</span></span>

<span data-ttu-id="13797-119">__Příklad:__</span><span class="sxs-lookup"><span data-stu-id="13797-119">__Example:__</span></span>

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

<span data-ttu-id="13797-120">Text v rámci komentáře dokumentace musí být správně utvořena podle pravidel XML (https://www.w3.org/TR/REC-xml).</span><span class="sxs-lookup"><span data-stu-id="13797-120">The text within documentation comments must be well formed according to the rules of XML (https://www.w3.org/TR/REC-xml).</span></span> <span data-ttu-id="13797-121">Pokud kód XML má výplně formát, je vygenerováno upozornění a dokumentaci soubor bude obsahovat komentář informacemi o tom, že došlo k chybě.</span><span class="sxs-lookup"><span data-stu-id="13797-121">If the XML is ill formed, a warning is generated and the documentation file will contain a comment saying that an error was encountered.</span></span>

<span data-ttu-id="13797-122">Přestože vývojáře k vytvoření vlastních sad značek, doporučených sada je definována v [doporučené značky](documentation-comments.md#recommended-tags).</span><span class="sxs-lookup"><span data-stu-id="13797-122">Although developers are free to create their own set of tags, a recommended set is defined in [Recommended tags](documentation-comments.md#recommended-tags).</span></span> <span data-ttu-id="13797-123">Některé doporučené značky mají zvláštní význam:</span><span class="sxs-lookup"><span data-stu-id="13797-123">Some of the recommended tags have special meanings:</span></span>

*  <span data-ttu-id="13797-124">`<param>` Značka se používá k popisu parametrů.</span><span class="sxs-lookup"><span data-stu-id="13797-124">The `<param>` tag is used to describe parameters.</span></span> <span data-ttu-id="13797-125">Pokud tato značka se používá, generátor dokumentaci musíte ověřit, že zadaný parametr existuje a že všechny parametry jsou popsané v komentáře k dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="13797-125">If such a tag is used, the documentation generator must verify that the specified parameter exists and that all parameters are described in documentation comments.</span></span> <span data-ttu-id="13797-126">Pokud se ověření nezdaří, generátor dokumentace ke službě vydá upozornění.</span><span class="sxs-lookup"><span data-stu-id="13797-126">If such verification fails, the documentation generator issues a warning.</span></span>
*  <span data-ttu-id="13797-127">`cref` Atribut lze připojit ke každé značce poskytnout odkaz na prvek kódu.</span><span class="sxs-lookup"><span data-stu-id="13797-127">The `cref` attribute can be attached to any tag to provide a reference to a code element.</span></span> <span data-ttu-id="13797-128">Dokumentace ke službě generátor musí ověřte, zda tento prvek kódu existuje.</span><span class="sxs-lookup"><span data-stu-id="13797-128">The documentation generator must verify that this code element exists.</span></span> <span data-ttu-id="13797-129">Pokud se ověření nezdaří, generátor dokumentace ke službě vydá upozornění.</span><span class="sxs-lookup"><span data-stu-id="13797-129">If the verification fails, the documentation generator issues a warning.</span></span> <span data-ttu-id="13797-130">Pokud hledáte podle názvu `cref` atribut, generátor dokumentaci musí dodržovat viditelnosti oboru názvů podle `using` příkazy uvedené v rámci zdrojového kódu.</span><span class="sxs-lookup"><span data-stu-id="13797-130">When looking for a name described in a `cref` attribute, the documentation generator must respect namespace visibility according to `using` statements appearing within the source code.</span></span> <span data-ttu-id="13797-131">Pro prvky kódu, které jsou obecné, normální Obecná syntaxe (tedy "`List<T>`") nelze použít, protože vytváří neplatný kód XML.</span><span class="sxs-lookup"><span data-stu-id="13797-131">For code elements that are generic, the normal generic syntax (that is, "`List<T>`") cannot be used because it produces invalid XML.</span></span> <span data-ttu-id="13797-132">Složené závorky, je možné použít místo hranaté závorky (to znamená, "`List{T}`"), nebo je možné řídicí syntaxe jazyka XML (tedy "`List&lt;T&gt;`").</span><span class="sxs-lookup"><span data-stu-id="13797-132">Braces can be used instead of brackets (that is, "`List{T}`"), or the XML escape syntax can be used (that is, "`List&lt;T&gt;`").</span></span>
*  <span data-ttu-id="13797-133">`<summary>` Značka je určena pro použití podle dokumentace k prohlížeči a zobrazte další informace o typu nebo členu.</span><span class="sxs-lookup"><span data-stu-id="13797-133">The `<summary>` tag is intended to be used by a documentation viewer to display additional information about a type or member.</span></span>
*  <span data-ttu-id="13797-134">`<include>` Značka obsahuje informace z externího souboru XML.</span><span class="sxs-lookup"><span data-stu-id="13797-134">The `<include>` tag includes information from an external XML file.</span></span>

<span data-ttu-id="13797-135">Pozorně si, že soubor dokumentace neposkytuje úplné informace o typu a členů (například neobsahuje žádné informace o typu).</span><span class="sxs-lookup"><span data-stu-id="13797-135">Note carefully that the documentation file does not provide full information about the type and members (for example, it does not contain any type information).</span></span> <span data-ttu-id="13797-136">Pokud chcete získat informace o typu nebo členu, musí použít soubor dokumentace ve spojení s reflexí na skutečný typ nebo člen.</span><span class="sxs-lookup"><span data-stu-id="13797-136">To get such information about a type or member, the documentation file must be used in conjunction with reflection on the actual type or member.</span></span>

## <a name="recommended-tags"></a><span data-ttu-id="13797-137">Doporučené značky</span><span class="sxs-lookup"><span data-stu-id="13797-137">Recommended tags</span></span>

<span data-ttu-id="13797-138">Generátor dokumentaci musíte přijmout a zpracovat všechny značky, který je platný podle pravidel XML.</span><span class="sxs-lookup"><span data-stu-id="13797-138">The documentation generator must accept and process any tag that is valid according to the rules of XML.</span></span> <span data-ttu-id="13797-139">Následující značky poskytuje běžně používané funkce v dokumentaci pro uživatele.</span><span class="sxs-lookup"><span data-stu-id="13797-139">The following tags provide commonly used functionality in user documentation.</span></span> <span data-ttu-id="13797-140">(Další značky jsou samozřejmě možné.)</span><span class="sxs-lookup"><span data-stu-id="13797-140">(Of course, other tags are possible.)</span></span>


| <span data-ttu-id="13797-141">__Značka__</span><span class="sxs-lookup"><span data-stu-id="13797-141">__Tag__</span></span>          | <span data-ttu-id="13797-142">__Oddíl__</span><span class="sxs-lookup"><span data-stu-id="13797-142">__Section__</span></span>                                            | <span data-ttu-id="13797-143">__Účel__</span><span class="sxs-lookup"><span data-stu-id="13797-143">__Purpose__</span></span>                                            |
|------------------|--------------------------------------------------------|--------------------------------------------------------|
| `<c>`            | [`<c>`](documentation-comments.md#c)                   | <span data-ttu-id="13797-144">Nastavit text v kódu jako písma</span><span class="sxs-lookup"><span data-stu-id="13797-144">Set text in a code-like font</span></span>                           | 
| `<code>`         | [`<code>`](documentation-comments.md#code)             | <span data-ttu-id="13797-145">Nastavit jeden nebo více řádků zdrojového kódu nebo výstup programu.</span><span class="sxs-lookup"><span data-stu-id="13797-145">Set one or more lines of source code or program output</span></span> |
| `<example>`      | [`<example>`](documentation-comments.md#example)       | <span data-ttu-id="13797-146">Označení příklad</span><span class="sxs-lookup"><span data-stu-id="13797-146">Indicate an example</span></span>                                    |
| `<exception>`    | [`<exception>`](documentation-comments.md#exception)   | <span data-ttu-id="13797-147">Identifikuje výjimky, které můžou vyvolat metodu</span><span class="sxs-lookup"><span data-stu-id="13797-147">Identifies the exceptions a method can throw</span></span>           |
| `<include>`      | [`<include>`](documentation-comments.md#include)       | <span data-ttu-id="13797-148">Obsahuje XML z externího souboru</span><span class="sxs-lookup"><span data-stu-id="13797-148">Includes XML from an external file</span></span>                     |
| `<list>`         | [`<list>`](documentation-comments.md#list)             | <span data-ttu-id="13797-149">Vytvořit seznam nebo tabulku</span><span class="sxs-lookup"><span data-stu-id="13797-149">Create a list or table</span></span>                                 |
| `<para>`         | [`<para>`](documentation-comments.md#para)             | <span data-ttu-id="13797-150">Povolit struktury mají být přidány do textu</span><span class="sxs-lookup"><span data-stu-id="13797-150">Permit structure to be added to text</span></span>                   |
| `<param>`        | [`<param>`](documentation-comments.md#param)           | <span data-ttu-id="13797-151">Popis parametru metoda nebo konstruktor</span><span class="sxs-lookup"><span data-stu-id="13797-151">Describe a parameter for a method or constructor</span></span>       |
| `<paramref>`     | [`<paramref>`](documentation-comments.md#paramref)     | <span data-ttu-id="13797-152">Zjistit, slovo je název parametru</span><span class="sxs-lookup"><span data-stu-id="13797-152">Identify that a word is a parameter name</span></span>               |
| `<permission>`   | [`<permission>`](documentation-comments.md#permission) | <span data-ttu-id="13797-153">Dokument přístupností člena</span><span class="sxs-lookup"><span data-stu-id="13797-153">Document the security accessibility of a member</span></span>        |
| `<remark>`       | [`<remark>`](documentation-comments.md#remark)         | <span data-ttu-id="13797-154">Popisují další informace o typu</span><span class="sxs-lookup"><span data-stu-id="13797-154">Describe additional information about a type</span></span>           |
| `<returns>`      | [`<returns>`](documentation-comments.md#returns)       | <span data-ttu-id="13797-155">Popište návratovou hodnotu metody</span><span class="sxs-lookup"><span data-stu-id="13797-155">Describe the return value of a method</span></span>                  |
| `<see>`          | [`<see>`](documentation-comments.md#see)               | <span data-ttu-id="13797-156">Zadejte odkaz</span><span class="sxs-lookup"><span data-stu-id="13797-156">Specify a link</span></span>                                         |
| `<seealso>`      | [`<seealso>`](documentation-comments.md#seealso)       | <span data-ttu-id="13797-157">Generovat položku v části Viz také</span><span class="sxs-lookup"><span data-stu-id="13797-157">Generate a See Also entry</span></span>                              |
| `<summary>`      | [`<summary>`](documentation-comments.md#summary)       | <span data-ttu-id="13797-158">Popisují typ nebo člen typu</span><span class="sxs-lookup"><span data-stu-id="13797-158">Describe a type or a member of a type</span></span>                  |
| `<value>`        | [`<value>`](documentation-comments.md#value)           | <span data-ttu-id="13797-159">Popis vlastnosti</span><span class="sxs-lookup"><span data-stu-id="13797-159">Describe a property</span></span>                                    |
| `<typeparam>`    |                                                        | <span data-ttu-id="13797-160">Popis parametru obecného typu</span><span class="sxs-lookup"><span data-stu-id="13797-160">Describe a generic type parameter</span></span>                      |
| `<typeparamref>` |                                                        | <span data-ttu-id="13797-161">Zjistit, je slovo typu parametru názvu</span><span class="sxs-lookup"><span data-stu-id="13797-161">Identify that a word is a type parameter name</span></span>          |

### `<c>`

<span data-ttu-id="13797-162">Tato značka poskytuje mechanismus pro označení, že fragment textu v popisu musí být nastaveno v speciální písmo. například, který používá pro blok kódu.</span><span class="sxs-lookup"><span data-stu-id="13797-162">This tag provides a mechanism to indicate that a fragment of text within a description should be set in a special font such as that used for a block of code.</span></span> <span data-ttu-id="13797-163">Řádky skutečný kód, použijte `<code>` ([`<code>`](documentation-comments.md#code)).</span><span class="sxs-lookup"><span data-stu-id="13797-163">For lines of actual code, use `<code>` ([`<code>`](documentation-comments.md#code)).</span></span>

<span data-ttu-id="13797-164">__Syntaxe:__</span><span class="sxs-lookup"><span data-stu-id="13797-164">__Syntax:__</span></span>

```xml
<c>text</c>
```

<span data-ttu-id="13797-165">__Příklad:__</span><span class="sxs-lookup"><span data-stu-id="13797-165">__Example:__</span></span>

```csharp
/// <summary>Class <c>Point</c> models a point in a two-dimensional
/// plane.</summary>

public class Point 
{
    // ...
}
```

### `<code>`

<span data-ttu-id="13797-166">Tato značka se používá k nastavení jeden nebo více řádků zdrojového kódu nebo výstup programu některé speciální písmem.</span><span class="sxs-lookup"><span data-stu-id="13797-166">This tag is used to set one or more lines of source code or program output in some special font.</span></span> <span data-ttu-id="13797-167">Malý kód fragmenty v příběh, použijte `<c>` ([`<c>`](documentation-comments.md#c)).</span><span class="sxs-lookup"><span data-stu-id="13797-167">For small code fragments in narrative, use `<c>` ([`<c>`](documentation-comments.md#c)).</span></span>

<span data-ttu-id="13797-168">__Syntaxe:__</span><span class="sxs-lookup"><span data-stu-id="13797-168">__Syntax:__</span></span>

```xml
<code>source code or program output</code>
```

<span data-ttu-id="13797-169">__Příklad:__</span><span class="sxs-lookup"><span data-stu-id="13797-169">__Example:__</span></span>

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

<span data-ttu-id="13797-170">Tato značka umožňuje ukázkový kód v komentáři, chcete-li určit, jak lze metodu nebo jiný člen knihovny.</span><span class="sxs-lookup"><span data-stu-id="13797-170">This tag allows example code within a comment, to specify how a method or other library member may be used.</span></span> <span data-ttu-id="13797-171">Obvykle to by také zahrnovat použití značky `<code>` ([`<code>`](documentation-comments.md#code)) i.</span><span class="sxs-lookup"><span data-stu-id="13797-171">Ordinarily, this would also involve use of the tag `<code>` ([`<code>`](documentation-comments.md#code)) as well.</span></span>

<span data-ttu-id="13797-172">__Syntaxe:__</span><span class="sxs-lookup"><span data-stu-id="13797-172">__Syntax:__</span></span>

```xml
<example>description</example>
```

<span data-ttu-id="13797-173">__Příklad:__</span><span class="sxs-lookup"><span data-stu-id="13797-173">__Example:__</span></span>

<span data-ttu-id="13797-174">Zobrazit `<code>` ([`<code>`](documentation-comments.md#code)) příklad.</span><span class="sxs-lookup"><span data-stu-id="13797-174">See `<code>` ([`<code>`](documentation-comments.md#code)) for an example.</span></span>

### `<exception>`

<span data-ttu-id="13797-175">Tato značka poskytuje způsob, jak dokumentovat výjimky, které můžou vyvolat metodu.</span><span class="sxs-lookup"><span data-stu-id="13797-175">This tag provides a way to document the exceptions a method can throw.</span></span>

<span data-ttu-id="13797-176">__Syntaxe:__</span><span class="sxs-lookup"><span data-stu-id="13797-176">__Syntax:__</span></span>

```xml
<exception cref="member">description</exception>
```

<span data-ttu-id="13797-177">kde</span><span class="sxs-lookup"><span data-stu-id="13797-177">where</span></span>

* <span data-ttu-id="13797-178">`member` je název člena.</span><span class="sxs-lookup"><span data-stu-id="13797-178">`member` is the name of a member.</span></span> <span data-ttu-id="13797-179">Dokumentace ke službě generátor kontroluje, zda daný člen existuje a přeloží `member` do názvu canonical prvku v souboru dokumentace.</span><span class="sxs-lookup"><span data-stu-id="13797-179">The documentation generator checks that the given member exists and translates `member` to the canonical element name in the documentation file.</span></span>
* <span data-ttu-id="13797-180">`description` je popis okolnosti, ve kterých je vyvolána výjimka.</span><span class="sxs-lookup"><span data-stu-id="13797-180">`description` is a description of the circumstances in which the exception is thrown.</span></span>

<span data-ttu-id="13797-181">__Příklad:__</span><span class="sxs-lookup"><span data-stu-id="13797-181">__Example:__</span></span>

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

<span data-ttu-id="13797-182">Tato značka umožňuje, včetně informací z dokumentu XML, který je externí vzhledem k souboru se zdrojovým kódem.</span><span class="sxs-lookup"><span data-stu-id="13797-182">This tag allows including information from an XML document that is external to the source code file.</span></span> <span data-ttu-id="13797-183">Externí soubor musí být ve správném formátu dokumentu XML a výraz XPath platí pro daný dokument k určení, jaké XML z dokumentu zahrnout.</span><span class="sxs-lookup"><span data-stu-id="13797-183">The external file must be a well-formed XML document, and an XPath expression is applied to that document to specify what XML from that document to include.</span></span> <span data-ttu-id="13797-184">`<include>` Značky se pak nahradí vybrané XML z externího dokumentu.</span><span class="sxs-lookup"><span data-stu-id="13797-184">The `<include>` tag is then replaced with the selected XML from the external document.</span></span>

<span data-ttu-id="13797-185">__Syntaxe:__</span><span class="sxs-lookup"><span data-stu-id="13797-185">__Syntax:__</span></span>

```
<include file="filename" path="xpath" />
```

<span data-ttu-id="13797-186">kde</span><span class="sxs-lookup"><span data-stu-id="13797-186">where</span></span>

* <span data-ttu-id="13797-187">`filename` je název souboru z externího souboru XML.</span><span class="sxs-lookup"><span data-stu-id="13797-187">`filename` is the file name of an external XML file.</span></span> <span data-ttu-id="13797-188">Název souboru je relativní vzhledem k souboru, který obsahuje značku include interpretován.</span><span class="sxs-lookup"><span data-stu-id="13797-188">The file name is interpreted relative to the file that contains the include tag.</span></span>
* <span data-ttu-id="13797-189">`xpath` je výraz XPath, který vybere část souboru XML v externím souboru XML.</span><span class="sxs-lookup"><span data-stu-id="13797-189">`xpath` is an XPath expression that selects some of the XML in the external XML file.</span></span>

<span data-ttu-id="13797-190">__Příklad:__</span><span class="sxs-lookup"><span data-stu-id="13797-190">__Example:__</span></span>

<span data-ttu-id="13797-191">Pokud zdrojový kód obsahuje deklarace, jako jsou:</span><span class="sxs-lookup"><span data-stu-id="13797-191">If the source code contained a declaration like:</span></span>

```csharp
/// <include file="docs.xml" path='extradoc/class[@name="IntList"]/*' />
public class IntList { ... }
```

<span data-ttu-id="13797-192">a externího souboru "docs.xml" měl následující obsah:</span><span class="sxs-lookup"><span data-stu-id="13797-192">and the external file "docs.xml" had the following contents:</span></span>

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

<span data-ttu-id="13797-193">pak stejné dokumentace je výstup jako by obsahovala zdrojového kódu:</span><span class="sxs-lookup"><span data-stu-id="13797-193">then the same documentation is output as if the source code contained:</span></span>

```csharp
/// <summary>
///    Contains a list of integers.
/// </summary>
public class IntList { ... }
```

### `<list>`

<span data-ttu-id="13797-194">Tato značka se používá k vytvoření seznamu nebo tabulce položek.</span><span class="sxs-lookup"><span data-stu-id="13797-194">This tag is used to create a list or table of items.</span></span> <span data-ttu-id="13797-195">Může obsahovat `<listheader>` bloku k definování řádek záhlaví tabulky nebo definice seznamu.</span><span class="sxs-lookup"><span data-stu-id="13797-195">It may contain a `<listheader>` block to define the heading row of either a table or definition list.</span></span> <span data-ttu-id="13797-196">(Při definování tabulku pouze záznam pro `term` v záhlaví musí být zadán.)</span><span class="sxs-lookup"><span data-stu-id="13797-196">(When defining a table, only an entry for `term` in the heading need be supplied.)</span></span>

<span data-ttu-id="13797-197">Každá položka v seznamu není zadán s `<item>` bloku.</span><span class="sxs-lookup"><span data-stu-id="13797-197">Each item in the list is specified with an `<item>` block.</span></span> <span data-ttu-id="13797-198">Při vytváření definice seznamu, obě `term` a `description` musí být zadán.</span><span class="sxs-lookup"><span data-stu-id="13797-198">When creating a definition list, both `term` and `description` must be specified.</span></span> <span data-ttu-id="13797-199">Nicméně pro tabulku, seznam s odrážkami nebo číslovaný seznam pouze `description` musí být zadán.</span><span class="sxs-lookup"><span data-stu-id="13797-199">However, for a table, bulleted list, or numbered list, only `description` need be specified.</span></span>

<span data-ttu-id="13797-200">__Syntaxe:__</span><span class="sxs-lookup"><span data-stu-id="13797-200">__Syntax:__</span></span>

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

<span data-ttu-id="13797-201">kde</span><span class="sxs-lookup"><span data-stu-id="13797-201">where</span></span>

* <span data-ttu-id="13797-202">`term` je termín k definování, jejichž definice není v `description`.</span><span class="sxs-lookup"><span data-stu-id="13797-202">`term` is the term to define, whose definition is in `description`.</span></span>
* <span data-ttu-id="13797-203">`description` Položka odrážkami nebo číslovaný seznam nebo definici `term`.</span><span class="sxs-lookup"><span data-stu-id="13797-203">`description` is either an item in a bullet or numbered list, or the definition of a `term`.</span></span>

<span data-ttu-id="13797-204">__Příklad:__</span><span class="sxs-lookup"><span data-stu-id="13797-204">__Example:__</span></span>

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

<span data-ttu-id="13797-205">Tato značka je určen pro použití v jiné značky, například `<summary>` ([`<remark>`](documentation-comments.md#remark)) nebo `<returns>` ([`<returns>`](documentation-comments.md#returns)) a povoluje struktury mají být přidány do textu.</span><span class="sxs-lookup"><span data-stu-id="13797-205">This tag is for use inside other tags, such as `<summary>` ([`<remark>`](documentation-comments.md#remark)) or `<returns>` ([`<returns>`](documentation-comments.md#returns)), and permits structure to be added to text.</span></span>

<span data-ttu-id="13797-206">__Syntaxe:__</span><span class="sxs-lookup"><span data-stu-id="13797-206">__Syntax:__</span></span>

```xml
<para>content</para>
```

<span data-ttu-id="13797-207">kde `content` je text odstavce.</span><span class="sxs-lookup"><span data-stu-id="13797-207">where `content` is the text of the paragraph.</span></span>

<span data-ttu-id="13797-208">__Příklad:__</span><span class="sxs-lookup"><span data-stu-id="13797-208">__Example:__</span></span>

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

<span data-ttu-id="13797-209">Tato značka se používá k popisu parametru pro metodu, konstruktor nebo indexeru.</span><span class="sxs-lookup"><span data-stu-id="13797-209">This tag is used to describe a parameter for a method, constructor, or indexer.</span></span>

<span data-ttu-id="13797-210">__Syntaxe:__</span><span class="sxs-lookup"><span data-stu-id="13797-210">__Syntax:__</span></span>

```xml
<param name="name">description</param>
```

<span data-ttu-id="13797-211">kde</span><span class="sxs-lookup"><span data-stu-id="13797-211">where</span></span>

* <span data-ttu-id="13797-212">`name` je název parametru.</span><span class="sxs-lookup"><span data-stu-id="13797-212">`name` is the name of the parameter.</span></span>
* <span data-ttu-id="13797-213">`description` představuje popis parametru.</span><span class="sxs-lookup"><span data-stu-id="13797-213">`description` is a description of the parameter.</span></span>

<span data-ttu-id="13797-214">__Příklad:__</span><span class="sxs-lookup"><span data-stu-id="13797-214">__Example:__</span></span>

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

<span data-ttu-id="13797-215">Tato značka se používá k označení, že je slovo parametru.</span><span class="sxs-lookup"><span data-stu-id="13797-215">This tag is used to indicate that a word is a parameter.</span></span> <span data-ttu-id="13797-216">Soubor dokumentace mohou být zpracovány k nějakým způsobem odlišné formátování tento parametr.</span><span class="sxs-lookup"><span data-stu-id="13797-216">The documentation file can be processed to format this parameter in some distinct way.</span></span>

<span data-ttu-id="13797-217">__Syntaxe:__</span><span class="sxs-lookup"><span data-stu-id="13797-217">__Syntax:__</span></span>

```xml
<paramref name="name"/>
```

<span data-ttu-id="13797-218">kde `name` je název parametru.</span><span class="sxs-lookup"><span data-stu-id="13797-218">where `name` is the name of the parameter.</span></span>

<span data-ttu-id="13797-219">__Příklad:__</span><span class="sxs-lookup"><span data-stu-id="13797-219">__Example:__</span></span>

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

<span data-ttu-id="13797-220">Tato značka umožňuje přístupností člena chcete zdokumentovat.</span><span class="sxs-lookup"><span data-stu-id="13797-220">This tag allows the security accessibility of a member to be documented.</span></span>

<span data-ttu-id="13797-221">__Syntaxe:__</span><span class="sxs-lookup"><span data-stu-id="13797-221">__Syntax:__</span></span>

```xml
<permission cref="member">description</permission>
```

<span data-ttu-id="13797-222">kde</span><span class="sxs-lookup"><span data-stu-id="13797-222">where</span></span>

* <span data-ttu-id="13797-223">`member` je název člena.</span><span class="sxs-lookup"><span data-stu-id="13797-223">`member` is the name of a member.</span></span> <span data-ttu-id="13797-224">Dokumentace ke službě generátor kontroluje, zda daný prvek kódu existuje a přeloží *člen* do názvu canonical prvku v souboru dokumentace.</span><span class="sxs-lookup"><span data-stu-id="13797-224">The documentation generator checks that the given code element exists and translates *member* to the canonical element name in the documentation file.</span></span>
* <span data-ttu-id="13797-225">`description` je popis přístup ke členu.</span><span class="sxs-lookup"><span data-stu-id="13797-225">`description` is a description of the access to the member.</span></span>

<span data-ttu-id="13797-226">__Příklad:__</span><span class="sxs-lookup"><span data-stu-id="13797-226">__Example:__</span></span>

```csharp
/// <permission cref="System.Security.PermissionSet">Everyone can
/// access this method.</permission>

public static void Test() {
    // ...
}
```

### `<remark>`

<span data-ttu-id="13797-227">Tato značka se používá k určení dalších informací o typu.</span><span class="sxs-lookup"><span data-stu-id="13797-227">This tag is used to specify extra information about a type.</span></span> <span data-ttu-id="13797-228">(Použití `<summary>` ([`<summary>`](documentation-comments.md#summary)) k popisu samotný datový typ a členy typu.)</span><span class="sxs-lookup"><span data-stu-id="13797-228">(Use `<summary>` ([`<summary>`](documentation-comments.md#summary)) to describe the type itself and the members of a type.)</span></span>

<span data-ttu-id="13797-229">__Syntaxe:__</span><span class="sxs-lookup"><span data-stu-id="13797-229">__Syntax:__</span></span>

```xml
<remark>description</remark>
```

<span data-ttu-id="13797-230">kde `description` je text příspěvku.</span><span class="sxs-lookup"><span data-stu-id="13797-230">where `description` is the text of the remark.</span></span>

<span data-ttu-id="13797-231">__Příklad:__</span><span class="sxs-lookup"><span data-stu-id="13797-231">__Example:__</span></span>

```csharp
/// <summary>Class <c>Point</c> models a point in a 
/// two-dimensional plane.</summary>
/// <remark>Uses polar coordinates</remark>
public class Point 
{
    // ...
}
```

### `<returns>`

<span data-ttu-id="13797-232">Tato značka se používá k popisu návratovou hodnotu metody.</span><span class="sxs-lookup"><span data-stu-id="13797-232">This tag is used to describe the return value of a method.</span></span>

<span data-ttu-id="13797-233">__Syntaxe:__</span><span class="sxs-lookup"><span data-stu-id="13797-233">__Syntax:__</span></span>

```xml
<returns>description</returns>
```

<span data-ttu-id="13797-234">kde `description` je popis návratovou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="13797-234">where `description` is a description of the return value.</span></span>

<span data-ttu-id="13797-235">__Příklad:__</span><span class="sxs-lookup"><span data-stu-id="13797-235">__Example:__</span></span>

```csharp
/// <summary>Report a point's location as a string.</summary>
/// <returns>A string representing a point's location, in the form (x,y),
///    without any leading, trailing, or embedded whitespace.</returns>
public override string ToString() {
    return "(" + X + "," + Y + ")";
}
```

### `<see>`

<span data-ttu-id="13797-236">Tato značka umožňuje odkazu na specifikovaný v textu.</span><span class="sxs-lookup"><span data-stu-id="13797-236">This tag allows a link to be specified within text.</span></span> <span data-ttu-id="13797-237">Použití `<seealso>` ([`<seealso>`](documentation-comments.md#seealso)) k označení textu, který se zobrazí v části Viz také.</span><span class="sxs-lookup"><span data-stu-id="13797-237">Use `<seealso>` ([`<seealso>`](documentation-comments.md#seealso)) to indicate text that is to appear in a See Also section.</span></span>

<span data-ttu-id="13797-238">__Syntaxe:__</span><span class="sxs-lookup"><span data-stu-id="13797-238">__Syntax:__</span></span>

```xml
<see cref="member"/>
```

<span data-ttu-id="13797-239">kde `member` je název člena.</span><span class="sxs-lookup"><span data-stu-id="13797-239">where `member` is the name of a member.</span></span> <span data-ttu-id="13797-240">Dokumentace ke službě generátor kontroluje, zda daný prvek kódu existuje a změní *člen* do názvu prvku v souboru vygenerovaná dokumentace.</span><span class="sxs-lookup"><span data-stu-id="13797-240">The documentation generator checks that the given code element exists and changes *member* to the element name in the generated documentation file.</span></span>

<span data-ttu-id="13797-241">__Příklad:__</span><span class="sxs-lookup"><span data-stu-id="13797-241">__Example:__</span></span>

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

<span data-ttu-id="13797-242">Tato značka umožňuje záznam, tím se vygeneruje pro části Viz také.</span><span class="sxs-lookup"><span data-stu-id="13797-242">This tag allows an entry to be generated for the See Also section.</span></span> <span data-ttu-id="13797-243">Použití `<see>` ([`<see>`](documentation-comments.md#see)) zadat odkaz v rámci textu.</span><span class="sxs-lookup"><span data-stu-id="13797-243">Use `<see>` ([`<see>`](documentation-comments.md#see)) to specify a link from within text.</span></span>

<span data-ttu-id="13797-244">__Syntaxe:__</span><span class="sxs-lookup"><span data-stu-id="13797-244">__Syntax:__</span></span>

```xml
<seealso cref="member"/>
```

<span data-ttu-id="13797-245">kde `member` je název člena.</span><span class="sxs-lookup"><span data-stu-id="13797-245">where `member` is the name of a member.</span></span> <span data-ttu-id="13797-246">Dokumentace ke službě generátor kontroluje, zda daný prvek kódu existuje a změní *člen* do názvu prvku v souboru vygenerovaná dokumentace.</span><span class="sxs-lookup"><span data-stu-id="13797-246">The documentation generator checks that the given code element exists and changes *member* to the element name in the generated documentation file.</span></span>

<span data-ttu-id="13797-247">__Příklad:__</span><span class="sxs-lookup"><span data-stu-id="13797-247">__Example:__</span></span>

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

Toto klíčové slovo lze použít k popisu typ nebo člen typu. <span data-ttu-id="13797-249">Použití `<remark>` ([`<remark>`](documentation-comments.md#remark)) k popisu samotného typu.</span><span class="sxs-lookup"><span data-stu-id="13797-249">Use `<remark>` ([`<remark>`](documentation-comments.md#remark)) to describe the type itself.</span></span>

<span data-ttu-id="13797-250">__Syntaxe:__</span><span class="sxs-lookup"><span data-stu-id="13797-250">__Syntax:__</span></span>

```xml
<summary>description</summary>
```

<span data-ttu-id="13797-251">kde `description` je uveden seznam tento typ nebo člen.</span><span class="sxs-lookup"><span data-stu-id="13797-251">where `description` is a summary of the type or member.</span></span>

<span data-ttu-id="13797-252">__Příklad:__</span><span class="sxs-lookup"><span data-stu-id="13797-252">__Example:__</span></span>

```csharp
/// <summary>This constructor initializes the new Point to (0,0).</summary>
public Point() : this(0,0) {
}
```

### `<value>`

<span data-ttu-id="13797-253">Tato značka umožňuje vlastnost, která má být popsány.</span><span class="sxs-lookup"><span data-stu-id="13797-253">This tag allows a property to be described.</span></span>

<span data-ttu-id="13797-254">__Syntaxe:__</span><span class="sxs-lookup"><span data-stu-id="13797-254">__Syntax:__</span></span>

```xml
<value>property description</value>
```

<span data-ttu-id="13797-255">kde `property description` je pro vlastnost Popis.</span><span class="sxs-lookup"><span data-stu-id="13797-255">where `property description` is a description for the property.</span></span>

<span data-ttu-id="13797-256">__Příklad:__</span><span class="sxs-lookup"><span data-stu-id="13797-256">__Example:__</span></span>

```csharp
/// <value>Property <c>X</c> represents the point's x-coordinate.</value>
public int X
{
    get { return x; }
    set { x = value; }
}
```

### `<typeparam>`

<span data-ttu-id="13797-257">Tato značka se používá k popisu parametru obecného typu třídy, struktury, rozhraní, delegáta nebo metoda.</span><span class="sxs-lookup"><span data-stu-id="13797-257">This tag is used to describe a generic type parameter for a class, struct, interface, delegate, or method.</span></span>

<span data-ttu-id="13797-258">__Syntaxe:__</span><span class="sxs-lookup"><span data-stu-id="13797-258">__Syntax:__</span></span>

```xml
<typeparam name="name">description</typeparam>
```

<span data-ttu-id="13797-259">kde `name` je název parametru typu a `description` je její popis.</span><span class="sxs-lookup"><span data-stu-id="13797-259">where `name` is the name of the type parameter, and `description` is its description.</span></span>

<span data-ttu-id="13797-260">__Příklad:__</span><span class="sxs-lookup"><span data-stu-id="13797-260">__Example:__</span></span>

```csharp
/// <summary>A generic list class.</summary>
/// <typeparam name="T">The type stored by the list.</typeparam>
public class MyList<T> {
    ...
}
```

### `<typeparamref>`

<span data-ttu-id="13797-261">Tato značka se používá k označení, že slovo je parametr typu.</span><span class="sxs-lookup"><span data-stu-id="13797-261">This tag is used to indicate that a word is a type parameter.</span></span> <span data-ttu-id="13797-262">Soubor dokumentace mohou být zpracovány k nějakým způsobem odlišné formátování tento parametr typu.</span><span class="sxs-lookup"><span data-stu-id="13797-262">The documentation file can be processed to format this type parameter in some distinct way.</span></span>

<span data-ttu-id="13797-263">__Syntaxe:__</span><span class="sxs-lookup"><span data-stu-id="13797-263">__Syntax:__</span></span>

```xml
<typeparamref name="name"/>
```

<span data-ttu-id="13797-264">kde `name` je název parametru typu.</span><span class="sxs-lookup"><span data-stu-id="13797-264">where `name` is the name of the type parameter.</span></span>

<span data-ttu-id="13797-265">__Příklad:__</span><span class="sxs-lookup"><span data-stu-id="13797-265">__Example:__</span></span>

```csharp
/// <summary>This method fetches data and returns a list of <typeparamref name="T"/>.</summary>
/// <param name="query">query to execute</param>
public List<T> FetchData<T>(string query) {
    ...
}
```

## <a name="processing-the-documentation-file"></a><span data-ttu-id="13797-266">Zpracování souboru dokumentace</span><span class="sxs-lookup"><span data-stu-id="13797-266">Processing the documentation file</span></span>

<span data-ttu-id="13797-267">Dokumentace ke službě generátor generuje řetězec ID pro každý prvek ve zdrojovém kódu, který se Dokumentační komentář označené.</span><span class="sxs-lookup"><span data-stu-id="13797-267">The documentation generator generates an ID string for each element in the source code that is tagged with a documentation comment.</span></span> <span data-ttu-id="13797-268">Tento řetězec ID jednoznačně identifikuje prvek zdroje.</span><span class="sxs-lookup"><span data-stu-id="13797-268">This ID string uniquely identifies a source element.</span></span> <span data-ttu-id="13797-269">Dokumentace k prohlížeči slouží jako řetězec ID k identifikaci odpovídající položku metadat/reflexe, ke kterému se vztahuje na dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="13797-269">A documentation viewer can use an ID string to identify the corresponding metadata/reflection item to which the documentation applies.</span></span>

<span data-ttu-id="13797-270">Soubor dokumentace není Hierarchická reprezentace zdrojového kódu; Místo toho je seznam bez stromové struktury s řetězcem vygenerované ID pro každý prvek.</span><span class="sxs-lookup"><span data-stu-id="13797-270">The documentation file is not a hierarchical representation of the source code; rather, it is a flat list with a generated ID string for each element.</span></span>

### <a name="id-string-format"></a><span data-ttu-id="13797-271">Formát ID řetězce</span><span class="sxs-lookup"><span data-stu-id="13797-271">ID string format</span></span>

<span data-ttu-id="13797-272">Dokumentace ke službě generátor dodržuje následující pravidla při generování ID řetězce:</span><span class="sxs-lookup"><span data-stu-id="13797-272">The documentation generator observes the following rules when it generates the ID strings:</span></span>

*  <span data-ttu-id="13797-273">Žádné jiné mezery, nachází v řetězci.</span><span class="sxs-lookup"><span data-stu-id="13797-273">No white space is placed in the string.</span></span>

*  <span data-ttu-id="13797-274">První část řetězce identifikuje typ členu je zdokumentovaná prostřednictvím rutiny jeden znak následovaný dvojtečkou.</span><span class="sxs-lookup"><span data-stu-id="13797-274">The first part of the string identifies the kind of member being documented, via a single character followed by a colon.</span></span> <span data-ttu-id="13797-275">Jsou definovány následující druhy členů:</span><span class="sxs-lookup"><span data-stu-id="13797-275">The following kinds of members are defined:</span></span>

   | <span data-ttu-id="13797-276">__Znak__</span><span class="sxs-lookup"><span data-stu-id="13797-276">__Character__</span></span> | <span data-ttu-id="13797-277">__Popis__</span><span class="sxs-lookup"><span data-stu-id="13797-277">__Description__</span></span>                                             |
   |---------------|-------------------------------------------------------------|
   | <span data-ttu-id="13797-278">E</span><span class="sxs-lookup"><span data-stu-id="13797-278">E</span></span>             | <span data-ttu-id="13797-279">Událost</span><span class="sxs-lookup"><span data-stu-id="13797-279">Event</span></span>                                                       |
   | <span data-ttu-id="13797-280">F</span><span class="sxs-lookup"><span data-stu-id="13797-280">F</span></span>             | <span data-ttu-id="13797-281">Pole</span><span class="sxs-lookup"><span data-stu-id="13797-281">Field</span></span>                                                       |
   | <span data-ttu-id="13797-282">M</span><span class="sxs-lookup"><span data-stu-id="13797-282">M</span></span>             | <span data-ttu-id="13797-283">(Včetně konstruktory, destruktory a operátory) – metoda</span><span class="sxs-lookup"><span data-stu-id="13797-283">Method (including constructors, destructors, and operators)</span></span> |
   | <span data-ttu-id="13797-284">N</span><span class="sxs-lookup"><span data-stu-id="13797-284">N</span></span>             | <span data-ttu-id="13797-285">Obor názvů</span><span class="sxs-lookup"><span data-stu-id="13797-285">Namespace</span></span>                                                   |
   | <span data-ttu-id="13797-286">P</span><span class="sxs-lookup"><span data-stu-id="13797-286">P</span></span>             | <span data-ttu-id="13797-287">Vlastnosti (včetně indexery)</span><span class="sxs-lookup"><span data-stu-id="13797-287">Property (including indexers)</span></span>                               |
   | <span data-ttu-id="13797-288">T</span><span class="sxs-lookup"><span data-stu-id="13797-288">T</span></span>             | <span data-ttu-id="13797-289">Typ (například třída, delegát, výčtu, rozhraní a struktury)</span><span class="sxs-lookup"><span data-stu-id="13797-289">Type (such as class, delegate, enum, interface, and struct)</span></span> |
   | <span data-ttu-id="13797-290">!</span><span class="sxs-lookup"><span data-stu-id="13797-290">!</span></span>             | <span data-ttu-id="13797-291">Řetězec chyby; zbývající řetězec poskytuje informace o této chybě.</span><span class="sxs-lookup"><span data-stu-id="13797-291">Error string; the rest of the string provides information about the error.</span></span> <span data-ttu-id="13797-292">Například dokumentace generátor generuje informace o chybě pro odkazy, které nelze rozpoznat.</span><span class="sxs-lookup"><span data-stu-id="13797-292">For example, the documentation generator generates error information for links that cannot be resolved.</span></span> |

*  <span data-ttu-id="13797-293">Druhá část řetězce je plně kvalifikovaný název elementu, spouštění v kořenovém oboru názvů.</span><span class="sxs-lookup"><span data-stu-id="13797-293">The second part of the string is the fully qualified name of the element, starting at the root of the namespace.</span></span> <span data-ttu-id="13797-294">Název elementu, jeho nadřazené typy a obor názvů jsou odděleny tečkami.</span><span class="sxs-lookup"><span data-stu-id="13797-294">The name of the element, its enclosing type(s), and namespace are separated by periods.</span></span> <span data-ttu-id="13797-295">Pokud má název samotné položky období, budou nahrazeny `#(U+0023)` znaků.</span><span class="sxs-lookup"><span data-stu-id="13797-295">If the name of the item itself has periods, they are replaced by `#(U+0023)` characters.</span></span> <span data-ttu-id="13797-296">(Předpokládá se, že žádný element nemá tento znak v názvu.)</span><span class="sxs-lookup"><span data-stu-id="13797-296">(It is assumed that no element has this character in its name.)</span></span>
*  <span data-ttu-id="13797-297">Pro metody a vlastnosti s argumenty, pomocí následujícího seznamu argument uzavřen v závorkách.</span><span class="sxs-lookup"><span data-stu-id="13797-297">For methods and properties with arguments, the argument list follows, enclosed in parentheses.</span></span> <span data-ttu-id="13797-298">Pro ty bez argumentů jsou vynechány závorky.</span><span class="sxs-lookup"><span data-stu-id="13797-298">For those without arguments, the parentheses are omitted.</span></span> <span data-ttu-id="13797-299">Argumenty jsou odděleny čárkami.</span><span class="sxs-lookup"><span data-stu-id="13797-299">The arguments are separated by commas.</span></span> <span data-ttu-id="13797-300">Kódování každý argument je stejný jako rozhraní příkazového řádku, následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="13797-300">The encoding of each argument is the same as a CLI signature, as follows:</span></span>
   *  <span data-ttu-id="13797-301">Argumenty jsou reprezentovány podle názvu jejich dokumentaci, která je založena na jejich plně kvalifikovanému názvu upraveny následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="13797-301">Arguments are represented by their documentation name, which is based on their fully qualified name, modified as follows:</span></span>
      * <span data-ttu-id="13797-302">Argumenty, které představují obecné typy mají připojený `` ` `` (prvními) následovaný počet parametrů typu</span><span class="sxs-lookup"><span data-stu-id="13797-302">Arguments that represent generic types have an appended `` ` `` (backtick) character followed by the number of type parameters</span></span>
      * <span data-ttu-id="13797-303">Argumenty s `out` nebo `ref` mít modifikátor `@` po jejich název typu.</span><span class="sxs-lookup"><span data-stu-id="13797-303">Arguments having the `out` or `ref` modifier have an `@` following their type name.</span></span> <span data-ttu-id="13797-304">Argumenty předány podle hodnoty nebo prostřednictvím `params` mít žádná zvláštní zápis.</span><span class="sxs-lookup"><span data-stu-id="13797-304">Arguments passed by value or via `params` have no special notation.</span></span>
      * <span data-ttu-id="13797-305">Argumenty, které jsou pole jsou reprezentovány ve formě `[lowerbound:size, ... , lowerbound:size]` kde počet čárky je řád méně jeden a dolní meze a velikosti jednotlivých rozměrů, pokud jsou známé, jsou reprezentovány v desítkové soustavě.</span><span class="sxs-lookup"><span data-stu-id="13797-305">Arguments that are arrays are represented as `[lowerbound:size, ... , lowerbound:size]` where the number of commas is the rank less one, and the lower bounds and size of each dimension, if known, are represented in decimal.</span></span> <span data-ttu-id="13797-306">Pokud není zadán dolní mez nebo velikost, je vynechán.</span><span class="sxs-lookup"><span data-stu-id="13797-306">If a lower bound or size is not specified, it is omitted.</span></span> <span data-ttu-id="13797-307">Pokud jsou vynechány dolní mez a velikosti pro konkrétní dimenzi, `:` je také vynechán.</span><span class="sxs-lookup"><span data-stu-id="13797-307">If the lower bound and size for a particular dimension are omitted, the `:` is omitted as well.</span></span> <span data-ttu-id="13797-308">Vícenásobná pole jsou reprezentované pomocí jedné `[]` na úroveň.</span><span class="sxs-lookup"><span data-stu-id="13797-308">Jagged arrays are represented by one `[]` per level.</span></span>
      * <span data-ttu-id="13797-309">Argumenty, které mají ukazatel typy než void se vyjadřují pomocí `*` za názvem typu.</span><span class="sxs-lookup"><span data-stu-id="13797-309">Arguments that have pointer types other than void are represented using a `*` following the type name.</span></span> <span data-ttu-id="13797-310">Ukazatel void je reprezentována pomocí názvu typu `System.Void`.</span><span class="sxs-lookup"><span data-stu-id="13797-310">A void pointer is represented using a type name of `System.Void`.</span></span>
      * <span data-ttu-id="13797-311">Argumenty, které odkazují na parametry obecného typu, které jsou definovány pro typy jsou zakódovány pomocí `` ` `` (prvními) následovaný z nuly vycházející index parametru typu.</span><span class="sxs-lookup"><span data-stu-id="13797-311">Arguments that refer to generic type parameters defined on types are encoded using the `` ` `` (backtick) character followed by the zero-based index of the type parameter.</span></span>
      * <span data-ttu-id="13797-312">Argumenty, které používají parametry obecného typu, které jsou definovány v metodách pomocí double prvními ``` `` ``` místo `` ` `` použít pro typy.</span><span class="sxs-lookup"><span data-stu-id="13797-312">Arguments that use generic type parameters defined in methods use a double-backtick ``` `` ``` instead of the `` ` `` used for types.</span></span>
      * <span data-ttu-id="13797-313">Argumenty, které odkazují na sestavené obecné typy jsou zakódovány pomocí obecného typu, za nímž následuje `{`, za nímž následuje čárkou oddělený seznam argumentů, za nímž následuje `}`.</span><span class="sxs-lookup"><span data-stu-id="13797-313">Arguments that refer to constructed generic types are encoded using the generic type, followed by `{`, followed by a comma-separated list of type arguments, followed by `}`.</span></span>

### <a name="id-string-examples"></a><span data-ttu-id="13797-314">Příklady řetězec ID</span><span class="sxs-lookup"><span data-stu-id="13797-314">ID string examples</span></span>

<span data-ttu-id="13797-315">Následující příklady ukazují fragment kódu jazyka C#, společně s řetězcem ID získané z jednotlivých zdrojových elementů schopné s Dokumentační komentář:</span><span class="sxs-lookup"><span data-stu-id="13797-315">The following examples each show a fragment of C# code, along with the ID string produced from each source element capable of having a documentation comment:</span></span>

*  <span data-ttu-id="13797-316">Typy jsou reprezentovány pomocí jejich plně kvalifikovanému názvu, doplněné o obecné informace:</span><span class="sxs-lookup"><span data-stu-id="13797-316">Types are represented using their fully qualified name, augmented with generic information:</span></span>

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

*  <span data-ttu-id="13797-317">Pole jsou reprezentovány pomocí jejich plně kvalifikovanému názvu:</span><span class="sxs-lookup"><span data-stu-id="13797-317">Fields are represented by their fully qualified name:</span></span>

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

*  <span data-ttu-id="13797-318">Konstruktory.</span><span class="sxs-lookup"><span data-stu-id="13797-318">Constructors.</span></span>

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

*  <span data-ttu-id="13797-319">Destruktory.</span><span class="sxs-lookup"><span data-stu-id="13797-319">Destructors.</span></span>

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

*  <span data-ttu-id="13797-320">Metody.</span><span class="sxs-lookup"><span data-stu-id="13797-320">Methods.</span></span>

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

*  <span data-ttu-id="13797-321">Vlastnostmi a indexery.</span><span class="sxs-lookup"><span data-stu-id="13797-321">Properties and indexers.</span></span>

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

*  <span data-ttu-id="13797-322">události.</span><span class="sxs-lookup"><span data-stu-id="13797-322">Events.</span></span>

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

*  <span data-ttu-id="13797-323">Unární operátory.</span><span class="sxs-lookup"><span data-stu-id="13797-323">Unary operators.</span></span>

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

   <span data-ttu-id="13797-324">Kompletní sadu unární operátor funkce názvů používaných vypadá takto: `op_UnaryPlus`, `op_UnaryNegation`, `op_LogicalNot`, `op_OnesComplement`, `op_Increment`, `op_Decrement`, `op_True`, a `op_False`.</span><span class="sxs-lookup"><span data-stu-id="13797-324">The complete set of unary operator function names used is as follows: `op_UnaryPlus`, `op_UnaryNegation`, `op_LogicalNot`, `op_OnesComplement`, `op_Increment`, `op_Decrement`, `op_True`, and `op_False`.</span></span>

*  <span data-ttu-id="13797-325">Binární operátory.</span><span class="sxs-lookup"><span data-stu-id="13797-325">Binary operators.</span></span>

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

   <span data-ttu-id="13797-326">Kompletní sadu názvů funkcí binární operátor používá vypadá takto: `op_Addition`, `op_Subtraction`, `op_Multiply`, `op_Division`, `op_Modulus`, `op_BitwiseAnd`, `op_BitwiseOr`, `op_ExclusiveOr`, `op_LeftShift`, `op_RightShift`, `op_Equality`, `op_Inequality`, `op_LessThan`, `op_LessThanOrEqual`, `op_GreaterThan`, a `op_GreaterThanOrEqual`.</span><span class="sxs-lookup"><span data-stu-id="13797-326">The complete set of binary operator function names used is as follows: `op_Addition`, `op_Subtraction`, `op_Multiply`, `op_Division`, `op_Modulus`, `op_BitwiseAnd`, `op_BitwiseOr`, `op_ExclusiveOr`, `op_LeftShift`, `op_RightShift`, `op_Equality`, `op_Inequality`, `op_LessThan`, `op_LessThanOrEqual`, `op_GreaterThan`, and `op_GreaterThanOrEqual`.</span></span>

*  <span data-ttu-id="13797-327">Operátory převodu mít koncový znak "`~`" následované návratovým typem.</span><span class="sxs-lookup"><span data-stu-id="13797-327">Conversion operators have a trailing "`~`" followed by the return type.</span></span>

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

## <a name="an-example"></a><span data-ttu-id="13797-328">Příklad</span><span class="sxs-lookup"><span data-stu-id="13797-328">An example</span></span>

### <a name="c-source-code"></a><span data-ttu-id="13797-329">Zdrojový kód C#</span><span class="sxs-lookup"><span data-stu-id="13797-329">C# source code</span></span>

<span data-ttu-id="13797-330">Následující příklad ukazuje, zdrojový kód `Point` třídy:</span><span class="sxs-lookup"><span data-stu-id="13797-330">The following example shows the source code of a `Point` class:</span></span>

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

### <a name="resulting-xml"></a><span data-ttu-id="13797-331">Výsledný XML</span><span class="sxs-lookup"><span data-stu-id="13797-331">Resulting XML</span></span>

<span data-ttu-id="13797-332">Zde je výstup vytvořený z jednoho dokumentaci generátoru při zdrojový kód pro třídu `Point`, je uveden výše:</span><span class="sxs-lookup"><span data-stu-id="13797-332">Here is the output produced by one documentation generator when given the source code for class `Point`, shown above:</span></span>

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
