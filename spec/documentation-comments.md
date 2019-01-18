---
ms.openlocfilehash: c9f8417dc68153f02ceb72bb1d51f3615f3c4961
ms.sourcegitcommit: 3fc033b6e98ed7ecdf46a85c79b00a3a3ddcf963
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/18/2019
ms.locfileid: "54272043"
---
# <a name="documentation-comments"></a>Dokumentační komentáře

C# poskytuje mechanismus pro programátory zdokumentujte svůj kód pomocí syntaxe speciální komentář, který obsahuje XML text. V souborech zdrojového kódu komentáře s formuláři určité umožňuje přímé nástroj k vytvoření XML z těchto komentářů a zdrojové elementy kódu, které jsou předcházet. Komentáře pomocí těchto syntaxe se nazývají ***komentáře k dokumentaci***. Musí bezprostředně předcházet uživatelem definovaný typ (například třída, delegát nebo rozhraní) nebo jako člen (například pole, události, vlastnost nebo metoda). Nástroj pro generování XML je volána ***dokumentaci generátor***. (Tento generátor může být, ale nemusí být kompilátor jazyka C#, samotný.) Výstup vytvořené generátorem dokumentace je volána ***soubor dokumentace***. Soubor dokumentace se používá jako vstup pro ***dokumentaci prohlížeč***; nástroj určených k vytvoření nějaký druh vizuálního zobrazení informací o typu a její související dokumentaci.

Tato specifikace navrhuje sadu značky pro dokumentační komentáře, ale použijte tyto značky se nevyžaduje a další značky mohou být použity v případě potřeby, následovaných dlouho pravidla ve správném formátu XML.

## <a name="introduction"></a>Úvod

Komentáře, které mají speciální tvar slouží ke směrování nástroj k vytvoření XML z těchto komentářů a zdrojové elementy kódu, které jsou předcházet. Takové komentáře jsou Jednořádkové komentáře, které začínají s třemi lomítky (`///`), nebo oddělených komentáře, které začínají lomítkem a dvě hvězdičky (`/**`). Musí bezprostředně předcházet uživatelem definovaný typ (například třída, delegát nebo rozhraní) nebo člen (například pole, události, vlastnost nebo metoda), který se opatřovat je poznámkami. Atribut oddíly ([specifikace atributu](attributes.md#attribute-specification)) jsou považovány za součást deklarace, takže komentáře dokumentace musí předcházet atributy použité na typ nebo člen.

__Syntaxe:__

```antlr
single_line_doc_comment
    : '///' input_character*
    ;

delimited_doc_comment
    : '/**' delimited_comment_section* asterisk+ '/'
    ;
```

V *single_line_doc_comment*, pokud je *prázdné znaky* následující znak `///` znaků ve všech *single_line_doc_comment*s sousední na aktuální *single_line_doc_comment*, pak, která *prázdné znaky* znak není součástí výstupu XML.

V oddělených doc – komentáři Pokud je první neprázdný znak na druhém řádku hvězdičku a stejný vzor nepovinné prázdné znaky a znak hvězdičky se opakuje na začátku každého řádku v oddělených doc – komentáře potom znaků opakované vzoru nejsou součástí výstupu XML. Vzor může obsahovat prázdné znaky, po, stejně jako před znak hvězdičky.

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

Text v rámci komentáře dokumentace musí být správně utvořena podle pravidel XML (https://www.w3.org/TR/REC-xml). Pokud kód XML má výplně formát, je vygenerováno upozornění a dokumentaci soubor bude obsahovat komentář informacemi o tom, že došlo k chybě.

Přestože vývojáře k vytvoření vlastních sad značek, doporučených sada je definována v [doporučené značky](documentation-comments.md#recommended-tags). Některé doporučené značky mají zvláštní význam:

*  `<param>` Značka se používá k popisu parametrů. Pokud tato značka se používá, generátor dokumentaci musíte ověřit, že zadaný parametr existuje a že všechny parametry jsou popsané v komentáře k dokumentaci. Pokud se ověření nezdaří, generátor dokumentace ke službě vydá upozornění.
*  `cref` Atribut lze připojit ke každé značce poskytnout odkaz na prvek kódu. Dokumentace ke službě generátor musí ověřte, zda tento prvek kódu existuje. Pokud se ověření nezdaří, generátor dokumentace ke službě vydá upozornění. Pokud hledáte podle názvu `cref` atribut, generátor dokumentaci musí dodržovat viditelnosti oboru názvů podle `using` příkazy uvedené v rámci zdrojového kódu. Pro prvky kódu, které jsou obecné, normální Obecná syntaxe (tedy "`List<T>`") nelze použít, protože vytváří neplatný kód XML. Složené závorky, je možné použít místo hranaté závorky (to znamená, "`List{T}`"), nebo je možné řídicí syntaxe jazyka XML (tedy "`List&lt;T&gt;`").
*  `<summary>` Značka je určena pro použití podle dokumentace k prohlížeči a zobrazte další informace o typu nebo členu.
*  `<include>` Značka obsahuje informace z externího souboru XML.

Pozorně si, že soubor dokumentace neposkytuje úplné informace o typu a členů (například neobsahuje žádné informace o typu). Pokud chcete získat informace o typu nebo členu, musí použít soubor dokumentace ve spojení s reflexí na skutečný typ nebo člen.

## <a name="recommended-tags"></a>Doporučené značky

Generátor dokumentaci musíte přijmout a zpracovat všechny značky, který je platný podle pravidel XML. Následující značky poskytuje běžně používané funkce v dokumentaci pro uživatele. (Další značky jsou samozřejmě možné.)


| __Značka__          | __Oddíl__                                            | __Účel__                                            |
|------------------|--------------------------------------------------------|--------------------------------------------------------|
| `<c>`            | [`<c>`](documentation-comments.md#c)                   | Nastavit text v kódu jako písma                           | 
| `<code>`         | [`<code>`](documentation-comments.md#code)             | Nastavit jeden nebo více řádků zdrojového kódu nebo výstup programu. |
| `<example>`      | [`<example>`](documentation-comments.md#example)       | Označení příklad                                    |
| `<exception>`    | [`<exception>`](documentation-comments.md#exception)   | Identifikuje výjimky, které můžou vyvolat metodu           |
| `<include>`      | [`<include>`](documentation-comments.md#include)       | Obsahuje XML z externího souboru                     |
| `<list>`         | [`<list>`](documentation-comments.md#list)             | Vytvořit seznam nebo tabulku                                 |
| `<para>`         | [`<para>`](documentation-comments.md#para)             | Povolit struktury mají být přidány do textu                   |
| `<param>`        | [`<param>`](documentation-comments.md#param)           | Popis parametru metoda nebo konstruktor       |
| `<paramref>`     | [`<paramref>`](documentation-comments.md#paramref)     | Zjistit, slovo je název parametru               |
| `<permission>`   | [`<permission>`](documentation-comments.md#permission) | Dokument přístupností člena        |
| `<remark>`       | [`<remark>`](documentation-comments.md#remark)         | Popisují další informace o typu           |
| `<returns>`      | [`<returns>`](documentation-comments.md#returns)       | Popište návratovou hodnotu metody                  |
| `<see>`          | [`<see>`](documentation-comments.md#see)               | Zadejte odkaz                                         |
| `<seealso>`      | [`<seealso>`](documentation-comments.md#seealso)       | Generovat položku v části Viz také                              |
| `<summary>`      | [`<summary>`](documentation-comments.md#summary)       | Popisují typ nebo člen typu                  |
| `<value>`        | [`<value>`](documentation-comments.md#value)           | Popis vlastnosti                                    |
| `<typeparam>`    |                                                        | Popis parametru obecného typu                      |
| `<typeparamref>` |                                                        | Zjistit, je slovo typu parametru názvu          |

### `<c>`

Tato značka poskytuje mechanismus pro označení, že fragment textu v popisu musí být nastaveno v speciální písmo. například, který používá pro blok kódu. Řádky skutečný kód, použijte `<code>` ([`<code>`](documentation-comments.md#code)).

__Syntaxe:__

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

Tato značka se používá k nastavení jeden nebo více řádků zdrojového kódu nebo výstup programu některé speciální písmem. Malý kód fragmenty v příběh, použijte `<c>` ([`<c>`](documentation-comments.md#c)).

__Syntaxe:__

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

Tato značka umožňuje ukázkový kód v komentáři, chcete-li určit, jak lze metodu nebo jiný člen knihovny. Obvykle to by také zahrnovat použití značky `<code>` ([`<code>`](documentation-comments.md#code)) i.

__Syntaxe:__

```xml
<example>description</example>
```

__Příklad:__

Zobrazit `<code>` ([`<code>`](documentation-comments.md#code)) příklad.

### `<exception>`

Tato značka poskytuje způsob, jak dokumentovat výjimky, které můžou vyvolat metodu.

__Syntaxe:__

```xml
<exception cref="member">description</exception>
```

kde

* `member` je název člena. Dokumentace ke službě generátor kontroluje, zda daný člen existuje a přeloží `member` do názvu canonical prvku v souboru dokumentace.
* `description` je popis okolnosti, ve kterých je vyvolána výjimka.

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

Tato značka umožňuje, včetně informací z dokumentu XML, který je externí vzhledem k souboru se zdrojovým kódem. Externí soubor musí být ve správném formátu dokumentu XML a výraz XPath platí pro daný dokument k určení, jaké XML z dokumentu zahrnout. `<include>` Značky se pak nahradí vybrané XML z externího dokumentu.

__Syntaxe:__

```
<include file="filename" path="xpath" />
```

kde

* `filename` je název souboru z externího souboru XML. Název souboru je relativní vzhledem k souboru, který obsahuje značku include interpretován.
* `xpath` je výraz XPath, který vybere část souboru XML v externím souboru XML.

__Příklad:__

Pokud zdrojový kód obsahuje deklarace, jako jsou:

```csharp
/// <include file="docs.xml" path='extradoc/class[@name="IntList"]/*' />
public class IntList { ... }
```

a externího souboru "docs.xml" měl následující obsah:

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

pak stejné dokumentace je výstup jako by obsahovala zdrojového kódu:

```csharp
/// <summary>
///    Contains a list of integers.
/// </summary>
public class IntList { ... }
```

### `<list>`

Tato značka se používá k vytvoření seznamu nebo tabulce položek. Může obsahovat `<listheader>` bloku k definování řádek záhlaví tabulky nebo definice seznamu. (Při definování tabulku pouze záznam pro `term` v záhlaví musí být zadán.)

Každá položka v seznamu není zadán s `<item>` bloku. Při vytváření definice seznamu, obě `term` a `description` musí být zadán. Nicméně pro tabulku, seznam s odrážkami nebo číslovaný seznam pouze `description` musí být zadán.

__Syntaxe:__

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

* `term` je termín k definování, jejichž definice není v `description`.
* `description` Položka odrážkami nebo číslovaný seznam nebo definici `term`.

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

Tato značka je určen pro použití v jiné značky, například `<summary>` ([`<remark>`](documentation-comments.md#remark)) nebo `<returns>` ([`<returns>`](documentation-comments.md#returns)) a povoluje struktury mají být přidány do textu.

__Syntaxe:__

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

Tato značka se používá k popisu parametru pro metodu, konstruktor nebo indexeru.

__Syntaxe:__

```xml
<param name="name">description</param>
```

kde

* `name` je název parametru.
* `description` představuje popis parametru.

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

Tato značka se používá k označení, že je slovo parametru. Soubor dokumentace mohou být zpracovány k nějakým způsobem odlišné formátování tento parametr.

__Syntaxe:__

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

Tato značka umožňuje přístupností člena chcete zdokumentovat.

__Syntaxe:__

```xml
<permission cref="member">description</permission>
```

kde

* `member` je název člena. Dokumentace ke službě generátor kontroluje, zda daný prvek kódu existuje a přeloží *člen* do názvu canonical prvku v souboru dokumentace.
* `description` je popis přístup ke členu.

__Příklad:__

```csharp
/// <permission cref="System.Security.PermissionSet">Everyone can
/// access this method.</permission>

public static void Test() {
    // ...
}
```

### `<remark>`

Tato značka se používá k určení dalších informací o typu. (Použití `<summary>` ([`<summary>`](documentation-comments.md#summary)) k popisu samotný datový typ a členy typu.)

__Syntaxe:__

```xml
<remark>description</remark>
```

kde `description` je text příspěvku.

__Příklad:__

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

Tato značka se používá k popisu návratovou hodnotu metody.

__Syntaxe:__

```xml
<returns>description</returns>
```

kde `description` je popis návratovou hodnotu.

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

Tato značka umožňuje odkazu na specifikovaný v textu. Použití `<seealso>` ([`<seealso>`](documentation-comments.md#seealso)) k označení textu, který se zobrazí v části Viz také.

__Syntaxe:__

```xml
<see cref="member"/>
```

kde `member` je název člena. Dokumentace ke službě generátor kontroluje, zda daný prvek kódu existuje a změní *člen* do názvu prvku v souboru vygenerovaná dokumentace.

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

Tato značka umožňuje záznam, tím se vygeneruje pro části Viz také. Použití `<see>` ([`<see>`](documentation-comments.md#see)) zadat odkaz v rámci textu.

__Syntaxe:__

```xml
<seealso cref="member"/>
```

kde `member` je název člena. Dokumentace ke službě generátor kontroluje, zda daný prvek kódu existuje a změní *člen* do názvu prvku v souboru vygenerovaná dokumentace.

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

Toto klíčové slovo lze použít k popisu typ nebo člen typu. Použití `<remark>` ([`<remark>`](documentation-comments.md#remark)) k popisu samotného typu.

__Syntaxe:__

```xml
<summary>description</summary>
```

kde `description` je uveden seznam tento typ nebo člen.

__Příklad:__

```csharp
/// <summary>This constructor initializes the new Point to (0,0).</summary>
public Point() : this(0,0) {
}
```

### `<value>`

Tato značka umožňuje vlastnost, která má být popsány.

__Syntaxe:__

```xml
<value>property description</value>
```

kde `property description` je pro vlastnost Popis.

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

Tato značka se používá k popisu parametru obecného typu třídy, struktury, rozhraní, delegáta nebo metoda.

__Syntaxe:__

```xml
<typeparam name="name">description</typeparam>
```

kde `name` je název parametru typu a `description` je její popis.

__Příklad:__

```csharp
/// <summary>A generic list class.</summary>
/// <typeparam name="T">The type stored by the list.</typeparam>
public class MyList<T> {
    ...
}
```

### `<typeparamref>`

Tato značka se používá k označení, že slovo je parametr typu. Soubor dokumentace mohou být zpracovány k nějakým způsobem odlišné formátování tento parametr typu.

__Syntaxe:__

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

## <a name="processing-the-documentation-file"></a>Zpracování souboru dokumentace

Dokumentace ke službě generátor generuje řetězec ID pro každý prvek ve zdrojovém kódu, který se Dokumentační komentář označené. Tento řetězec ID jednoznačně identifikuje prvek zdroje. Dokumentace k prohlížeči slouží jako řetězec ID k identifikaci odpovídající položku metadat/reflexe, ke kterému se vztahuje na dokumentaci.

Soubor dokumentace není Hierarchická reprezentace zdrojového kódu; Místo toho je seznam bez stromové struktury s řetězcem vygenerované ID pro každý prvek.

### <a name="id-string-format"></a>Formát ID řetězce

Dokumentace ke službě generátor dodržuje následující pravidla při generování ID řetězce:

*  Žádné jiné mezery, nachází v řetězci.

*  První část řetězce identifikuje typ členu je zdokumentovaná prostřednictvím rutiny jeden znak následovaný dvojtečkou. Jsou definovány následující druhy členů:

   | __Znak__ | __Popis__                                             |
   |---------------|-------------------------------------------------------------|
   | E             | Událost                                                       |
   | F             | Pole                                                       |
   | M             | (Včetně konstruktory, destruktory a operátory) – metoda |
   | N             | Obor názvů                                                   |
   | P             | Vlastnosti (včetně indexery)                               |
   | T             | Typ (například třída, delegát, výčtu, rozhraní a struktury) |
   | !             | Řetězec chyby; zbývající řetězec poskytuje informace o této chybě. Například dokumentace generátor generuje informace o chybě pro odkazy, které nelze rozpoznat. |

*  Druhá část řetězce je plně kvalifikovaný název elementu, spouštění v kořenovém oboru názvů. Název elementu, jeho nadřazené typy a obor názvů jsou odděleny tečkami. Pokud má název samotné položky období, budou nahrazeny `#(U+0023)` znaků. (Předpokládá se, že žádný element nemá tento znak v názvu.)
*  Pro metody a vlastnosti s argumenty, pomocí následujícího seznamu argument uzavřen v závorkách. Pro ty bez argumentů jsou vynechány závorky. Argumenty jsou odděleny čárkami. Kódování každý argument je stejný jako rozhraní příkazového řádku, následujícím způsobem:
   *  Argumenty jsou reprezentovány podle názvu jejich dokumentaci, která je založena na jejich plně kvalifikovanému názvu upraveny následujícím způsobem:
      * Argumenty, které představují obecné typy mají připojený `` ` `` (prvními) následovaný počet parametrů typu
      * Argumenty s `out` nebo `ref` mít modifikátor `@` po jejich název typu. Argumenty předány podle hodnoty nebo prostřednictvím `params` mít žádná zvláštní zápis.
      * Argumenty, které jsou pole jsou reprezentovány ve formě `[lowerbound:size, ... , lowerbound:size]` kde počet čárky je řád méně jeden a dolní meze a velikosti jednotlivých rozměrů, pokud jsou známé, jsou reprezentovány v desítkové soustavě. Pokud není zadán dolní mez nebo velikost, je vynechán. Pokud jsou vynechány dolní mez a velikosti pro konkrétní dimenzi, `:` je také vynechán. Vícenásobná pole jsou reprezentované pomocí jedné `[]` na úroveň.
      * Argumenty, které mají ukazatel typy než void se vyjadřují pomocí `*` za názvem typu. Ukazatel void je reprezentována pomocí názvu typu `System.Void`.
      * Argumenty, které odkazují na parametry obecného typu, které jsou definovány pro typy jsou zakódovány pomocí `` ` `` (prvními) následovaný z nuly vycházející index parametru typu.
      * Argumenty, které používají parametry obecného typu, které jsou definovány v metodách pomocí double prvními ``` `` ``` místo `` ` `` použít pro typy.
      * Argumenty, které odkazují na sestavené obecné typy jsou zakódovány pomocí obecného typu, za nímž následuje `{`, za nímž následuje čárkou oddělený seznam argumentů, za nímž následuje `}`.

### <a name="id-string-examples"></a>Příklady řetězec ID

Následující příklady ukazují fragment kódu jazyka C#, společně s řetězcem ID získané z jednotlivých zdrojových elementů schopné s Dokumentační komentář:

*  Typy jsou reprezentovány pomocí jejich plně kvalifikovanému názvu, doplněné o obecné informace:

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

*  Pole jsou reprezentovány pomocí jejich plně kvalifikovanému názvu:

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

*  Metody.

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

*  Vlastnostmi a indexery.

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

*  události.

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

   Kompletní sadu unární operátor funkce názvů používaných vypadá takto: `op_UnaryPlus`, `op_UnaryNegation`, `op_LogicalNot`, `op_OnesComplement`, `op_Increment`, `op_Decrement`, `op_True`, a `op_False`.

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

   Kompletní sadu názvů funkcí binární operátor používá vypadá takto: `op_Addition`, `op_Subtraction`, `op_Multiply`, `op_Division`, `op_Modulus`, `op_BitwiseAnd`, `op_BitwiseOr`, `op_ExclusiveOr`, `op_LeftShift`, `op_RightShift`, `op_Equality`, `op_Inequality`, `op_LessThan`, `op_LessThanOrEqual`, `op_GreaterThan`, a `op_GreaterThanOrEqual`.

*  Operátory převodu mít koncový znak "`~`" následované návratovým typem.

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

### <a name="c-source-code"></a>Zdrojový kód C#

Následující příklad ukazuje, zdrojový kód `Point` třídy:

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

### <a name="resulting-xml"></a>Výsledný XML

Zde je výstup vytvořený z jednoho dokumentaci generátoru při zdrojový kód pro třídu `Point`, je uveden výše:

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
