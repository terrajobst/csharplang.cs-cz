---
ms.openlocfilehash: adf81842e3c763c7bbdd3f10bb884dc1207b9099
ms.sourcegitcommit: 0489cb64b7dfb328813d757f4d447a15b85a5851
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/11/2019
ms.locfileid: "70912440"
---
# <a name="documentation-comments"></a>Komentáře dokumentace

C#poskytuje mechanismus pro programátory k dokumentování kódu pomocí speciální syntaxe komentářů, která obsahuje text XML. V souborech zdrojového kódu lze použít komentáře s určitým formulářem k tomu, aby nástroj vytvořil XML z těchto komentářů a prvky zdrojového kódu, které předcházejí. Komentáře používající takovou syntaxi se nazývají ***dokumentační komentáře***. Musí bezprostředně předcházet uživatelsky definovanému typu (například třída, delegát nebo rozhraní) nebo člen (například pole, událost, vlastnost nebo metoda). Nástroj pro generování XML se nazývá ***generátor dokumentace***. (Tento generátor může být, ale nemusí být samotným C# kompilátorem.) Výstup vyprodukovaný v dokumentaci generátoru se nazývá ***soubor dokumentace***. Soubor dokumentace slouží jako vstup do ***prohlížeče dokumentace***; nástroj určený k vytvoření určitého způsobu vizuálního zobrazení informací o typu a související dokumentaci.

Tato specifikace navrhuje sadu značek pro použití v dokumentačních komentářích, ale použití těchto značek není vyžadováno a jiné značky mohou být použity v případě potřeby, pokud jsou dodržena pravidla ve správném formátu XML.

## <a name="introduction"></a>Úvod

Komentáře s speciálním formulářem lze použít k tomu, aby nástroj vytvořil XML z těchto komentářů a prvky zdrojového kódu, které předcházejí. Jedná se o Jednořádkový komentář, který začíná třemi lomítky (`///`) nebo oddělenými komentáři, které začínají lomítkem a dvěma hvězdičkami (`/**`). Musí bezprostředně předcházet uživatelem definovaný typ (například třída, delegát nebo rozhraní) nebo člen (například pole, událost, vlastnost nebo metoda), ke kterým mají anotace. Oddíly atributů ([specifikace atributů](attributes.md#attribute-specification)) se považují za součást deklarací, takže dokumentační komentáře musí předcházet atributům použitým pro typ nebo člen.

__Syntaktick__

```antlr
single_line_doc_comment
    : '///' input_character*
    ;

delimited_doc_comment
    : '/**' delimited_comment_section* asterisk+ '/'
    ;
```

V *single_line_doc_comment*, pokud `///` je znak *mezery* za znaky v každé z *single_line_doc_comment*s aktuální *single_line_doc_comment*, pakve výstupu XML není obsažen prázdný znak.

V komentáři s oddělovači, pokud je první neprázdný znak na druhém řádku hvězdičkou a stejný vzor volitelných prázdných znaků a znak hvězdičky se opakuje na začátku každého řádku v rámci objektu s oddělovači. znaky opakovaného vzoru nejsou zahrnuty ve výstupu XML. Vzor může obsahovat prázdné znaky po znaku hvězdičky a také před znakem hvězdičky.

__Příklad:__

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

Text v dokumentačních komentářích musí být ve správném formátu podle pravidel XML (https://www.w3.org/TR/REC-xml). Pokud je kód XML chybně vytvořen, je vygenerováno upozornění a soubor dokumentace bude obsahovat komentář, který říká, že došlo k chybě.

I když je vývojářům zdarma vytvořit vlastní sadu značek, doporučovaná sada je definována v [doporučených značkách](documentation-comments.md#recommended-tags). Některé z doporučených značek mají zvláštní význam:

*  Tato `<param>` značka se používá k popisu parametrů. Je-li použita taková značka, musí generátor dokumentace ověřit, zda zadaný parametr existuje a zda jsou všechny parametry popsány v dokumentaci dokumentace. Pokud takové ověření neproběhne úspěšně, generátor dokumentace vydá upozornění.
*  `cref` Atribut lze připojit k libovolné značce k poskytnutí odkazu na prvek kódu. Generátor dokumentace musí ověřit, zda tento prvek kódu existuje. Pokud se ověření nepovede, generátor dokumentace vydá upozornění. Při hledání názvu popsanýho v `cref` atributu musí generátor dokumentace respektovat viditelnost oboru názvů `using` podle příkazů, které se nacházejí v rámci zdrojového kódu. U elementů kódu, které jsou obecné, nelze použít normální obecnou syntaxi (`List<T>`tj. ""), protože vytváří neplatný kód XML. Složené závorky lze použít místo závorek (`List{T}`tj. ""), případně lze použít syntaxi Escape XML (tj`List&lt;T&gt;`.).
*  `<summary>` Značku má použít prohlížeč dokumentace pro zobrazení dalších informací o typu nebo členu.
*  `<include>` Značka obsahuje informace z externího souboru XML.

Pozorně si všimněte, že soubor dokumentace neposkytuje úplné informace o typu a členech (například neobsahuje žádné informace o typu). Chcete-li získat informace o typu nebo členu, je třeba použít soubor dokumentace ve spojení s reflexí pro skutečný typ nebo člen.

## <a name="recommended-tags"></a>Doporučené značky

Generátor dokumentace musí přijmout a zpracovat všechny značky, které jsou platné podle pravidel XML. Následující značky poskytují běžně používané funkce v dokumentaci uživatele. (Samozřejmě jsou možné další značky.)


| __Inteligentní__          | __Section__                                            | __Účel__                                            |
|------------------|--------------------------------------------------------|--------------------------------------------------------|
| `<c>`            | [`<c>`](documentation-comments.md#c)                   | Nastavení textu v písmu podobného kódu                           | 
| `<code>`         | [`<code>`](documentation-comments.md#code)             | Nastavení jednoho nebo více řádků zdrojového kódu nebo výstupu programu |
| `<example>`      | [`<example>`](documentation-comments.md#example)       | Označení příkladu                                    |
| `<exception>`    | [`<exception>`](documentation-comments.md#exception)   | Určuje výjimky, které může metoda vyvolat.           |
| `<include>`      | [`<include>`](documentation-comments.md#include)       | Zahrnuje XML z externího souboru.                     |
| `<list>`         | [`<list>`](documentation-comments.md#list)             | Vytvoření seznamu nebo tabulky                                 |
| `<para>`         | [`<para>`](documentation-comments.md#para)             | Povolit přidání struktury do textu                   |
| `<param>`        | [`<param>`](documentation-comments.md#param)           | Popište parametr pro metodu nebo konstruktor.       |
| `<paramref>`     | [`<paramref>`](documentation-comments.md#paramref)     | Určení, že slovo je název parametru               |
| `<permission>`   | [`<permission>`](documentation-comments.md#permission) | Dokumentuje přístupnost člena v zabezpečení.        |
| `<remarks>`      | [`<remarks>`](documentation-comments.md#remarks)       | Popište Další informace o typu.           |
| `<returns>`      | [`<returns>`](documentation-comments.md#returns)       | Popisuje návratovou hodnotu metody.                  |
| `<see>`          | [`<see>`](documentation-comments.md#see)               | Zadat odkaz                                         |
| `<seealso>`      | [`<seealso>`](documentation-comments.md#seealso)       | Generovat položku viz také                              |
| `<summary>`      | [`<summary>`](documentation-comments.md#summary)       | Popis typu nebo členu typu                  |
| `<value>`        | [`<value>`](documentation-comments.md#value)           | Popsat vlastnost                                    |
| `<typeparam>`    |                                                        | Popsat parametr obecného typu                      |
| `<typeparamref>` |                                                        | Určení, že slovo je názvem parametru typu          |

### `<c>`

Tato značka poskytuje mechanismus pro indikaci, že fragment textu v rámci popisu by měl být nastaven ve speciálním písmu, například, které se používá pro blok kódu. Pro řádky vlastního kódu použijte `<code>` ([`<code>`](documentation-comments.md#code)).

__Syntaktick__

```xml
<c>text</c>
```

__Příklad:__

```csharp
/// <summary>Class <c>Point</c> models a point in a two-dimensional
/// plane.</summary>

public class Point 
{
    // ...
}
```

### `<code>`

Tato značka se používá k nastavení jednoho nebo více řádků zdrojového kódu nebo výstupu programu v některém speciálním písmu. Pro malé fragmenty kódu v mluveném komentáři[`<c>`](documentation-comments.md#c)použijte `<c>` ().

__Syntaktick__

```xml
<code>source code or program output</code>
```

__Příklad:__

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

Tato značka umožňuje ukázkový kód v rámci komentáře, který určuje, jak lze použít metodu nebo jiného člena knihovny. Obvykle by to zahrnovalo i použití značky `<code>` ([`<code>`](documentation-comments.md#code)).

__Syntaktick__

```xml
<example>description</example>
```

__Příklad:__

Příklad `<code>` naleznete[`<code>`](documentation-comments.md#code)v tématu ().

### `<exception>`

Tato značka poskytuje způsob, jak zdokumentovat výjimky, které může metoda vyvolat.

__Syntaktick__

```xml
<exception cref="member">description</exception>
```

kde

* `member`je název člena. Generátor dokumentace kontroluje, zda daný člen existuje a překládá `member` se na název kanonického elementu v souboru dokumentace.
* `description`je popis okolností, v nichž je vyvolána výjimka.

__Příklad:__

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

Tato značka umožňuje zahrnout informace z dokumentu XML, který je externí pro soubor zdrojového kódu. Externím souborem musí být dokument XML ve správném formátu a k tomuto dokumentu se použije výraz XPath, který určuje, co XML z tohoto dokumentu má zahrnout. `<include>` Značka se pak nahradí vybraným XML z externího dokumentu.

__Syntaktick__

```
<include file="filename" path="xpath" />
```

kde

* `filename`je název souboru externího souboru XML. Název souboru je interpretován relativně k souboru, který obsahuje značku include.
* `xpath`je výraz XPath, který vybírá některé XML v externím souboru XML.

__Příklad:__

Pokud zdrojový kód obsahoval deklaraci jako:

```csharp
/// <include file="docs.xml" path='extradoc/class[@name="IntList"]/*' />
public class IntList { ... }
```

a externí soubor docs. XML má následující obsah:

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

pak je stejná dokumentace ve výstupu, jako by obsahovala zdrojový kód:

```csharp
/// <summary>
///    Contains a list of integers.
/// </summary>
public class IntList { ... }
```

### `<list>`

Tato značka slouží k vytvoření seznamu nebo tabulky položek. Může obsahovat `<listheader>` blok pro definování řádku záhlaví buď tabulky, nebo seznamu definic. (Při definování tabulky je nutné zadat pouze záznam pro `term` v hlavičce.)

Každá položka v seznamu je určena `<item>` blokem. Při vytváření seznamu definic musí být zadán `term` obojí `description` a. U tabulky, seznamu s odrážkami nebo číslovaného seznamu je však třeba `description` zadat pouze parametr.

__Syntaktick__

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

kde

* `term`je výraz, který má být definován, jehož definice `description`je v.
* `description`je buď položka v seznamu odrážek nebo číslovaný seznam, nebo definice `term`.

__Příklad:__

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

Tato značka je určena pro použití uvnitř jiných značek, například `<summary>` ([`<remarks>`](documentation-comments.md#remarks)) nebo `<returns>` ([`<returns>`](documentation-comments.md#returns)), a umožňuje přidání struktury do textu.

__Syntaktick__

```xml
<para>content</para>
```

kde `content` je text odstavce.

__Příklad:__

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

Tato značka se používá k popisu parametru pro metodu, konstruktor nebo indexer.

__Syntaktick__

```xml
<param name="name">description</param>
```

kde

* `name`je název parametru.
* `description`je popis parametru.

__Příklad:__

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

Tato značka slouží k označení, že slovo je parametrem. Soubor dokumentace může být zpracován pro naformátování tohoto parametru nějakým odlišným způsobem.

__Syntaktick__

```xml
<paramref name="name"/>
```

kde `name` je název parametru.

__Příklad:__

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

Tato značka umožňuje zdokumentování zabezpečení člena.

__Syntaktick__

```xml
<permission cref="member">description</permission>
```

kde

* `member`je název člena. Generátor dokumentace kontroluje, zda daný prvek kódu existuje, a překládá *člena* na kanonický název elementu v souboru dokumentace.
* `description`je popis přístupu ke členu.

__Příklad:__

```csharp
/// <permission cref="System.Security.PermissionSet">Everyone can
/// access this method.</permission>

public static void Test() {
    // ...
}
```

### `<remarks>`

Tato značka slouží k zadání dalších informací o typu. (Pomocí `<summary>` ([`<summary>`](documentation-comments.md#summary)) Popište samotný typ a členy typu.)

__Syntaktick__

```xml
<remarks>description</remarks>
```

kde `description` je text přeznačení.

__Příklad:__

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

Tato značka se používá k popisu návratové hodnoty metody.

__Syntaktick__

```xml
<returns>description</returns>
```

kde `description` je popis návratové hodnoty.

__Příklad:__

```csharp
/// <summary>Report a point's location as a string.</summary>
/// <returns>A string representing a point's location, in the form (x,y),
///    without any leading, trailing, or embedded whitespace.</returns>
public override string ToString() {
    return "(" + X + "," + Y + ")";
}
```

### `<see>`

Tato značka umožňuje zadat odkaz v rámci textu. Pomocí `<seealso>` [(`<seealso>`](documentation-comments.md#seealso)) označíte text, který se má zobrazit v části Viz také.

__Syntaktick__

```xml
<see cref="member"/>
```

kde `member` je název člena. Generátor dokumentace kontroluje, zda daný prvek kódu existuje a mění *člena* na název elementu v generovaném souboru dokumentace.

__Příklad:__

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

Tato značka umožňuje vygenerovat položku v části Viz také. Pomocí `<see>` [(`<see>`](documentation-comments.md#see)) můžete zadat odkaz v rámci textu.

__Syntaktick__

```xml
<seealso cref="member"/>
```

kde `member` je název člena. Generátor dokumentace kontroluje, zda daný prvek kódu existuje a mění *člena* na název elementu v generovaném souboru dokumentace.

__Příklad:__

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

Tato značka se dá použít k popisu typu nebo členu typu. Použijte `<remarks>` [(`<remarks>`](documentation-comments.md#remarks)) pro popis samotného typu.

__Syntaktick__

```xml
<summary>description</summary>
```

kde `description` je souhrn typu nebo členu.

__Příklad:__

```csharp
/// <summary>This constructor initializes the new Point to (0,0).</summary>
public Point() : this(0,0) {
}
```

### `<value>`

Tato značka umožňuje popsat vlastnost.

__Syntaktick__

```xml
<value>property description</value>
```

kde `property description` je popis vlastnosti.

__Příklad:__

```csharp
/// <value>Property <c>X</c> represents the point's x-coordinate.</value>
public int X
{
    get { return x; }
    set { x = value; }
}
```

### `<typeparam>`

Tato značka se používá k popisu parametru obecného typu pro třídu, strukturu, rozhraní, delegáta nebo metodu.

__Syntaktick__

```xml
<typeparam name="name">description</typeparam>
```

kde `name` je název parametru typu a `description` je jeho popis.

__Příklad:__

```csharp
/// <summary>A generic list class.</summary>
/// <typeparam name="T">The type stored by the list.</typeparam>
public class MyList<T> {
    ...
}
```

### `<typeparamref>`

Tato značka slouží k označení, že slovo je parametr typu. Soubor dokumentace může být zpracován pro naformátování tohoto parametru typu nějakým odlišným způsobem.

__Syntaktick__

```xml
<typeparamref name="name"/>
```

kde `name` je název parametru typu.

__Příklad:__

```csharp
/// <summary>This method fetches data and returns a list of <typeparamref name="T"/>.</summary>
/// <param name="query">query to execute</param>
public List<T> FetchData<T>(string query) {
    ...
}
```

## <a name="processing-the-documentation-file"></a>Zpracovává se soubor dokumentace.

Generátor dokumentace generuje řetězec ID pro každý prvek ve zdrojovém kódu, který je označen komentářem k dokumentaci. Tento řetězec IDENTIFIKÁTORu jednoznačně identifikuje zdrojový element. Prohlížeč dokumentace může použít řetězec ID k identifikaci odpovídající položky metadat nebo reflexe, na kterou se dokumentace vztahuje.

Soubor dokumentace není hierarchická reprezentace zdrojového kódu; místo toho je seznam bez stromové struktury s generovaným řetězcem ID pro každý prvek.

### <a name="id-string-format"></a>Formát řetězce ID

Generátor dokumentace při generování řetězců ID dodržuje následující pravidla:

*  V řetězci není vložen prázdný znak.

*  První část řetězce identifikuje druh dokumentovaného člena přes jeden znak následovaný dvojtečkou. Jsou definovány následující typy členů:

   | __Optické__ | __Popis__                                             |
   |---------------|-------------------------------------------------------------|
   | E             | Událost                                                       |
   | F             | Pole                                                       |
   | M             | Metoda (včetně konstruktorů, destruktorů a operátorů) |
   | N             | Obor názvů                                                   |
   | P             | Vlastnost (včetně indexerů)                               |
   | T             | Typ (například třída, delegát, výčet, rozhraní a struktura) |
   | !             | Chybový řetězec; zbytek řetězce poskytuje informace o chybě. Například generátor dokumentace generuje informace o chybě pro odkazy, které nelze přeložit. |

*  Druhá část řetězce je plně kvalifikovaný název elementu začínajícího v kořenu oboru názvů. Název elementu, jeho uzavírací typ (y) a obor názvů jsou odděleny tečkami. Pokud má název položky vlastní tečky, nahradí `#(U+0023)` se znaky. (Předpokládá se, že žádný element nemá tento znak v názvu.)
*  Pro metody a vlastnosti s argumenty následuje seznam argumentů uzavřený v závorkách. V případě bez argumentů jsou závorky vynechány. Argumenty jsou odděleny čárkami. Kódování každého argumentu je stejné jako signatura CLI následujícím způsobem:
   *  Argumenty jsou reprezentovány podle názvu jejich dokumentace, který je založen na jejich plně kvalifikovaném názvu, který je upraven následujícím způsobem:
      * Argumenty, které reprezentují obecné typy, `` ` `` mají připojený znak (zpětný) následovaný počtem parametrů typu.
      * Argumenty, `out` které mají `ref` modifikátor or, `@` mají následující název typu. Argumenty předané hodnotou nebo prostřednictvím `params` nemají žádný speciální zápis.
      * Argumenty, které jsou pole, jsou `[lowerbound:size, ... , lowerbound:size]` reprezentovány, kde počet čárek je méně než jedna a dolní meze a velikost jednotlivých dimenzí, pokud jsou známy, jsou reprezentovány v desítkové soustavě. Pokud není zadaná dolní mez nebo velikost, je vynechána. Pokud je spodní mez a velikost pro konkrétní dimenzi vynechána, `:` je vynechána také hodnota. Vícenásobná pole jsou reprezentována `[]` jednou na úrovni.
      * Argumenty, které mají jiné typy ukazatelů než void, jsou reprezentovány pomocí `*` následujícího názvu typu. Ukazatel void je reprezentován pomocí názvu `System.Void`typu.
      * Argumenty, které odkazují na parametry obecného typu definované u typů, jsou zakódovány pomocí `` ` `` znaku (počátečního) následovaného indexem parametru typu s nulovým základem.
      * Argumenty, které používají parametry obecného typu definované v metodách, používají dvojité přetržení ``` `` ``` namísto `` ` `` použití pro typy.
      * Argumenty, které odkazují na konstruované obecné typy, jsou zakódovány pomocí obecného typu `{`následovaného čárkami odděleným seznamem argumentů typu a `}`následovány.

### <a name="id-string-examples"></a>Příklady řetězců ID

Následující příklady každé ukazuje fragment C# kódu, společně s řetězcem ID vytvořeným z každého zdrojového elementu, který může mít komentář k dokumentaci:

*  Typy jsou reprezentovány pomocí jejich plně kvalifikovaného názvu, rozšiřované o obecné informace:

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

*  Pole jsou reprezentována jejich plně kvalifikovaným názvem:

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

*  Konstruktory.

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

*  Destruktory.

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

*  Způsobů.

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

*  Vlastnosti a indexery.

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

*  Událost.

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

*  Unární operátory.

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

   Kompletní sada názvů funkcí unárního operátoru je `op_UnaryPlus`následující:, `op_UnaryNegation`, `op_LogicalNot`, `op_Decrement` `op_Increment` `op_OnesComplement`,,, `op_True` `op_False`a.

*  Binární operátory.

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

   `op_Addition`Kompletní sada názvů funkcí binárního operátoru je následující:, `op_LeftShift` `op_BitwiseOr` `op_Multiply` `op_BitwiseAnd` `op_Division` `op_Subtraction`,,, `op_Modulus`,,, `op_ExclusiveOr`,, `op_RightShift`, `op_Equality`, ,,`op_LessThan`, a`op_GreaterThan`. `op_LessThanOrEqual` `op_Inequality` `op_GreaterThanOrEqual`

*  Operátory převodu mají koncový znak`~`"" následovaný návratovým typem.

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

## <a name="an-example"></a>Příklad

### <a name="c-source-code"></a>C#zdrojový kód

Následující příklad ukazuje zdrojový kód `Point` třídy:

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

### <a name="resulting-xml"></a>Výsledný kód XML

Zde je výstup vyprodukovaný jedním generátorem dokumentace, pokud je daný zdrojový kód třídy `Point`, zobrazený výše:

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
