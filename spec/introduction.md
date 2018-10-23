# <a name="introduction"></a>Úvod

C# (čteno "v tématu Sharp") je jednoduchý, moderní, objektově orientované a bezpečnost typů programovací jazyk. C# má jeho kořeny řady jazyků C a bude okamžitě znát programátory C, C++ nebo Java. C# je standardizované společnosti ECMA International jako ***ECMA 334*** standardní a ISO/IEC jako ***23270 ISO/IEC*** standard. Společnosti Microsoft C# kompilátor pro rozhraní .NET Framework je vyhovující implementace obou těchto standardů.

C# je objektově orientovaný jazyk, ale jazyka C# dále zahrnuje podporu pro ***komponenty objektově orientovaný*** programování. Návrh moderní softwaru stále spoléhá na softwarové komponenty v podobě samostatné a popisující samy sebe balíčky funkcí. Klíčem k takové součásti je, že představují programovací model s vlastnosti, metody a události; mají atributy, které poskytují deklarativní informace o komponentě; a zahrnují vlastní dokumentace. C# zajišťuje, že jazyk konstrukce pro přímo podporují tyto koncepty, pak C# velmi přirozeného jazyka, ve kterém chcete vytvořit a používat softwarové součásti.

Několik funkcí jazyka C# pomáhají při vytváření robustních a odolných aplikací: ***uvolňování*** automaticky uvolní paměť obsazena nevyužité objekty. ***zpracování výjimek*** přináší strukturovaných a rozšiřitelné přístup k detekce chyb a obnovení; a ***zajišťující bezpečnost typů*** návrh jazyka znemožňuje čtení z neinicializovaného proměnné, do indexu pole, mimo jejich rozsah nebo provést přetypování není zaškrtnuto.

C# má ***unified systém typů***. Všechny typy C#, včetně primitivní typy, jako například `int` a `double`, dědit z jednoho kořene `object` typu. Proto sdílejí sadu běžných operací pro všechny typy a hodnoty libovolného typu lze ukládat, přenášet a provozován konzistentním způsobem. Kromě toho C# podporuje uživatelem definované referenční typy a typy hodnot, povolení dynamické přidělování objektů a úložiště v řádku zjednodušené struktur.

Pokud chcete mít jistotu, že aplikace C# a knihovny může v průběhu času vyvíjejí kompatibilní způsobem, se nachází mnoho důraz na ***správy verzí*** v C# pro návrh. Mnoho programovacích jazyků věnovat trochu tento problém, a v důsledku toho jsou zavedeny programy napsané v těchto jazyků přerušení častěji, než pokud je novější verze závislé knihovny. Aspekty návrhu v C#, které byly přímo ovlivňován aspekty správy verzí jsou jako samostatná `virtual` a `override` modifikátory, pravidla pro řešení přetížení metody a podpora deklarací členů explicitního rozhraní.

Zbývající část tato kapitola popisuje základní funkce jazyka C#. I když dalších kapitolách popisují orientované na podrobnosti a někdy matematické způsobem pravidel a výjimek, tato kapitola usiluje jasné a zkrácení za cenu úplnost. Účelem je poskytnout Úvod do jazyka, který usnadní psaní programů časná a čtení dalších kapitolách čtecí modul.

## <a name="hello-world"></a>Ahoj světe

Program "Hello, World" tradičně slouží k uvození programovací jazyk. Tady je v jazyce C#:

```csharp
using System;

class Hello
{
    static void Main() {
        Console.WriteLine("Hello, World");
    }
}
```

Zdrojové soubory jazyka C# obvykle mají příponu `.cs`. Za předpokladu, že program "Hello, World" je uložen v souboru `hello.cs`, může být program zkompilován s pomocí příkazového řádku kompilátoru jazyka Microsoft C#
```
csc hello.cs
```
produkuje spustitelného sestavení s názvem `hello.exe`. Je výstup vytvořeného touto aplikací při spuštění
```
Hello, World
```

Program "Hello, World" začíná `using` direktiva, která odkazuje `System` oboru názvů. Obory názvů umožňují hierarchické uspořádání programy jazyka C# a knihovny. Obory názvů obsahují typy a jiných oborech názvů – například `System` obor názvů obsahuje několik typů, například `Console` třída odkazovaná v programu a několik jiných oborech názvů, jako například `IO` a `Collections`. A `using` umožňuje direktiva, která odkazuje na daný obor názvů nekvalifikované použití typů, které jsou členy tohoto oboru názvů. Z důvodu `using` direktiv, můžete použít program `Console.WriteLine` jako zkratka pro `System.Console.WriteLine`.

`Hello` Třídy deklarované jako programem "Hello, World" obsahuje jeden člen metodu s názvem `Main`. `Main` Metoda je deklarována s `static` modifikátor. Zatímco instanční metody může odkazovat na konkrétní nadřazeného objektu instanci pomocí klíčového slova `this`, statické metody fungovat bez ohledu na konkrétní objekt. Podle konvence statickou metodu s názvem `Main` slouží jako vstupní bod programu.

Výstup programu je vytvořen `WriteLine` metodu `Console` třídy v `System` oboru názvů. Tato třída poskytuje knihovny tříd rozhraní .NET Framework, které ve výchozím nastavení, jsou automaticky odkazována pomocí kompilátoru jazyka Microsoft C#. Všimněte si, že C# sám nemá samostatné běhové knihovny. Místo toho rozhraní .NET Framework je knihovna runtime jazyka C#.

## <a name="program-structure"></a>Struktura programu

Klíčové koncepty organizace v jazyce C# jsou ***programy***, ***obory názvů***, ***typy***, ***členy***, a ***sestavení***. Programy jazyka C# se skládají z jednoho nebo více zdrojových souborů. Programy deklarovat typy, které obsahují členy a mohou být uspořádány do oborů názvů. Třídy a rozhraní jsou příklady typů. Pole, metody, vlastnosti a události jsou příklady členů. Když jsou kompilovány programy jazyka C#, jsou fyzicky zabaleny do sestavení. Sestavení obvykle mít příponu souboru `.exe` nebo `.dll`, v závislosti na tom, jestli je implementovat ***aplikací*** nebo ***knihovny***.

V příkladu

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
deklaruje třídu s názvem `Stack` v obor názvů s názvem `Acme.Collections`. Plně kvalifikovaný název této třídy je `Acme.Collections.Stack`. Třída obsahuje několik členů: pole s názvem `top`, dvě metody s názvem `Push` a `Pop`a vnořené třídy s názvem `Entry`. `Entry` Další třídy obsahuje tři členy: pole s názvem `next`, pole s názvem `data`a konstruktor. Za předpokladu, že zdrojový kód v příkladu je uložen v souboru `acme.cs`, příkazového řádku

```
csc /t:library acme.cs
```
zkompiluje v příkladu jako knihovna (bez kódu `Main` vstupního bodu) a vytváří sestavení s názvem `acme.dll`.

Sestavení obsahující spustitelný kód ve formě ***Intermediate Language*** instrukcí (IL) a symbolické informace ve formě ***metadat***. Předtím, než se spustí, kód IL v sestavení automaticky převést do kódu specifického pro procesor kompilátorem za běhu (JIT) modulu .NET Common Language Runtime.

Protože sestavení je jednotka funkce obsahující kód a metadata popisující samy sebe, není nutné pro `#include` direktivy a soubory hlaviček v jazyce C#. Veřejné typy a členy, které jsou obsažené v konkrétní sestavení jsou k dispozici v programu v jazyce C# jednoduše tak, že odkazující na toto sestavení při kompilaci programu. Například používá tento program `Acme.Collections.Stack` třídy z `acme.dll` sestavení:

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
Pokud program je uložený v souboru `test.cs`, když `test.cs` kompilaci, `acme.dll` sestavení lze odkazovat pomocí kompilátoru `/r` možnost:

```
csc /r:acme.dll test.cs
```
Tím se vytvoří sestavení spustitelného souboru s názvem `test.exe`, který při spuštění vytvoří výstup:

```
100
10
1
```
C# umožňuje text zdrojového programu, který bude uložen do několika zdrojových souborů. Když je zkompilován více soubory programu v C#, jsou společně zpracovány všechny zdrojové soubory a zdrojové soubory můžete odkazovat na volně mezi sebou, koncepčně, je, jako kdyby byly všechny zdrojové soubory zřetězeny do jednoho velkého souboru před zpracováním. Dopředné deklarace jsou potřeba nikdy v jazyce C#, protože s několika málo výjimkami je deklarace pořadí je neplatné. C# neomezuje zdrojový soubor k deklarování pouze jeden veřejný typ taky je potřeba, aby název zdrojového souboru tak, aby odpovídaly typu deklarovaného ve zdrojovém souboru.

## <a name="types-and-variables"></a>Typy a proměnné

Existují dva druhy typů v jazyce C#: ***typů hodnot*** a ***referenční typy***. Proměnné typů hodnoty přímo obsahovat svá data, zatímco proměnné typů odkazu ukládají odkazy na svá data, ten se označuje jako objekty. V případě typů odkazu je možné pro dvě proměnné odkazovat na stejný objekt a proto je možné pro operace v rámci jedné proměnné ovlivňovat objekt odkazovaný jinou proměnnou. S typy hodnot proměnných každý vlastní kopii dat a není možné pro operace se na nich se má vliv na jinou (s výjimkou v případě třídy `ref` a `out` proměnných parametrů).

C# pro typy hodnot se dále dělí do ***jednoduché typy***, ***typy výčtu***, ***typy struktury***, a ***typy připouštějící hodnotu Null***a referenční dokumentace jazyka C#. typy se dále dělí do ***typy tříd***, ***typy rozhraní***, ***pole typů***, a ***typy delegátů***.

Následující tabulka obsahuje přehled o typu systému C#.

| __Kategorie__    |                 | __Popis__ |
|-----------------|-----------------|-----------------|
| Typy hodnot     | Jednoduché typy    | Podepsané celé číslo: `sbyte`, `short`, `int`, `long` |
|                 |                 | Celočíselný typ bez znaménka: `byte`, `ushort`, `uint`, `ulong` |
|                 |                 | Znaky Unicode: `char` |
|                 |                 | S plovoucí desetinnou čárkou IEEE: `float`, `double` |
|                 |                 | Desetinná přesnost vysoce: `decimal` |
|                 |                 | Logická hodnota: `bool` |
|                 | Výčtové typy      | Uživatelem definované typy formuláře `enum E {...}` |
|                 | Typy struktury    | Uživatelem definované typy formuláře `struct S {...}` |
|                 | Typy s povolenou hodnotou Null  | Rozšíření všechny ostatní typy hodnot s `null` hodnota |
| Odkazové typy | Typy tříd     | Ultimate základní třídy pro všechny ostatní typy: `object` |
|                 |                 | Řetězce Unicode: `string` |
|                 |                 | Uživatelem definované typy formuláře `class C {...}` |
|                 | Typy rozhraní | Uživatelem definované typy formuláře `interface I {...}` |
|                 | Typy polí     | Jeden – a s multidimenzionálním, například `int[]` a `int[,]` |
|                 | Typy delegátů  | Uživatelem definované typy ve formátu například `delegate int  D(...)` |

Osm integrální typy poskytují podporu pro hodnoty 8bitové, 16 bitů, 32bitová verze a 64bitová verze v podobě nebo bez znaménka.

Dvě s plovoucí desetinnou čárkou bodu typy, `float` a `double`, jsou reprezentovány pomocí 32-bit jednoduchou přesností a 64bitové dvojitou přesností IEEE 754 formátů.

`decimal` Typ je typ 128bitových dat. vhodný pro výpočty finančních a přepočty měn.

C# `bool` typ se používá k reprezentaci logické hodnoty – hodnoty, které jsou buď `true` nebo `false`.

Znakové a řetězcové zpracování v jazyce C# používá kódování Unicode. `char` Typ představuje jednotku kódu UTF-16 a `string` typ představuje posloupnost jednotkami kódu kódování UTF-16.

Následující tabulka shrnuje C# pro číselné typy.


| __Kategorie__      | __Služba BITS__ | __Typ__  | __Rozsah a přesnost__ |
|-------------------|----------|-----------|---------------------|
| Podepsané celé číslo   | 8        | `sbyte`   | -128... 127 |
|                   | 16       | `short`   | -32, 768... 32, 767 |
|                   | 32       | `int`     | -2,147,483, 648... 2 147, 483, 647 |
|                   | 64       | `long`    | -9,223,372,036,854,775, 808... 9, 223, 372, 036, 854 775, 807 |
| Celočíselný typ bez znaménka | 8        | `byte`    | 0... 255 |
|                   | 16       | `ushort`  | 0... 65 535 |
|                   | 32       | `uint`    | 0... 4 294 967 295 |
|                   | 64       | `ulong`   | 0... 18,446,744,073,709,551,615 |
| Plovoucí desetinná čárka    | 32       | `float`   | 1,5 × 10 ^ −45 3.4 × 10 ^ 38, 7 číslicemi přesnosti |
|                   | 64       | `double`  | 5.0 × 10 ^ −324 1.7 × 10 ^ 308, 15 číslicemi přesnosti |
| Desetinné číslo           | 128      | `decimal` | 1.0 × 10 ^ −28 7.9 × 10 ^ 28, 28 číslicemi přesnosti |

C# programy používají ***typ deklarace*** pro vytvoření nových typů. Deklarace typu Určuje název a členy nového typu. Pět C# pro kategorie typů jsou definovaných uživatelem: Třída typy, typy struktury, rozhraní typy, výčtové typy a typy delegátů.

Typ třídy definuje datová struktura, která obsahuje data (pole) a funkční členy (metody, vlastnosti a ostatní). Typy tříd podporují jednoduché dědičnosti a polymorfismu, mechanismy jeho prostřednictvím odvozené třídy můžete rozšířit a specialize základní třídy.

Typ struktury je podobný typu třídy, představuje strukturu dat a funkční členy. Na rozdíl od tříd však struktury jsou typy hodnot a nevyžadují přidělení haldy. Typy struktury nepodporují dědičnosti zadané uživatelem, a všechny typy struktury implicitně dědí z typu `object`.

Typ rozhraní definuje kontrakt jako pojmenovanou sadu funkce veřejné členy. Třída nebo struktura, která implementuje rozhraní musí poskytnout implementace členů rozhraní funkce. Rozhraní může dědit z více základních rozhraní a třídy nebo struktury může implementovat více rozhraní.

Typ delegáta představuje odkazy na metody se seznamem konkrétních parametrů a návratovým typem. Delegáty umožňují považovat za entity, které může být přiřazena k proměnné a předány jako parametry metod. Delegáti jsou podobný koncept ukazatelů na funkce v některých jiných jazycích, ale na rozdíl od ukazatelů na funkce, Delegáti jsou objektově orientované a typově bezpečné.

Třídy, struktury, rozhraní a delegátů typů všechny obecné typy podpory, které mohou být parametrizovány s jinými typy.

Typ výčtu je odlišný typ se pojmenovaných konstant. Každý typ výčtu se základní typ, který musí mít jednu z osmi integrální typy. Sadu hodnot typu výčtu je stejný jako sadu hodnot ze základního typu.

C# podporuje jeden a více trojrozměrným pole libovolného typu. Na rozdíl od výše uvedené typy typy polí nemusí být deklarované před použitím. Místo toho typy pole jsou vytvořeny podle názvu typu do složených závorek. Například `int[]` je jednorozměrné pole `int`, `int[,]` je dvourozměrné pole `int`, a `int[][]` je jednorozměrné pole jednorozměrné pole `int`.

Typy s možnou hodnotou Null také nemají předtím, než je možné deklarovat. Pro jednotlivé typy neumožňující hodnotu `T` neexistuje odpovídající typ s možnou hodnotou Null `T?`, který může obsahovat další hodnotu `null`. Například `int?` je typ, který může pojmout všechny 32bitové celé číslo nebo hodnota `null`.

Systém typů jazyka C# je jednotný tak, aby jako objekt lze považovat hodnotu libovolného typu. Všechny typy v jazyce C# přímo nebo nepřímo odvozuje od `object` typ, třídy a `object` je ultimate základní třída všech typů. Hodnoty odkazové typy jsou považovány za objekty jednoduše tak, že zobrazení hodnoty jako typ `object`. Hodnoty typy hodnot jsou považovány za objekty pomocí provádí ***zabalení*** a ***rozbalení*** operace. V následujícím příkladu `int` hodnota je převedena na `object` a zpět znovu na `int`.

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
Pokud je hodnota typu hodnoty převést na typ `object`, instanci objektu, také nazývané "pole", je přidělená k uchování hodnoty a zkopírování hodnoty do tohoto pole. Naopak, když `object` odkaz je přetypován na typ hodnoty, který je pole správnou hodnotu typu odkazovaného objektu se provede kontrola a, případě zaškrtnutí bude úspěšné, je hodnota v poli zkopírována.

Jednotného typu systému C# efektivně znamená, že typy hodnot se může stát objekty "na vyžádání." Z důvodu sjednocení pro obecné účely knihoven, které používají typ `object` jde použít s typy odkazů a typy hodnot.

Existuje několik typů z ***proměnné*** v jazyce C#, včetně polí, prvky pole, místní proměnné a parametry. Umístění úložiště představují proměnné a každá proměnná má typ, který určuje, jaké hodnoty můžou být uložené v proměnné, jak je znázorněno v následující tabulce.


| __Typ proměnné__    | __Je to možné obsah__ |
|-------------------------|-----------------------|
| Typ neumožňující hodnotu | Hodnota tohoto přesné typu |
| Typ s možnou hodnotou Null     | Hodnotu null nebo hodnota přesně podle tohoto typu |
| `object`                | Odkaz s hodnotou null, odkaz na objekt typu odkazu nebo odkaz na o zabalenou hodnotu libovolný typ hodnoty |
| Typ třídy.              | Odkaz s hodnotou null, odkaz na instanci daného typu třídy nebo odkaz na instanci třídy odvozené z typu třídy |
| Typ rozhraní          | Odkaz s hodnotou null, odkaz na instanci typu třídy, která implementuje rozhraní typu nebo odkaz zabalené hodnoty hodnotového typu, který implementuje rozhraní typu |
| Typ pole              | Odkaz s hodnotou null, odkaz na instanci daného typu pole nebo odkaz na instanci typu kompatibilní pole |
| Typ delegáta           | Odkaz s hodnotou null nebo odkaz na instanci daného typu delegáta |

## <a name="expressions"></a>Výrazy

***Výrazy*** se vytvářejí na základě ***operandy*** a ***operátory***. Operátory výrazu označují operace, které chcete použít pro operandy. Příklady operátorů `+`, `-`, `*`, `/`, a `new`. Příklady operandy: literály, pole, místní proměnné a výrazy.

Pokud výraz obsahuje více operátorů ***prioritu*** operátorů určuje pořadí, ve kterém jsou jednotlivé operátory vyhodnocovány. Například výraz `x + y * z` se vyhodnotí jako `x + (y * z)` protože `*` má vyšší prioritu než operátor `+` operátor.

Většina operátory mohou být ***přetížené***. Přetížení operátoru umožňuje uživatelem definovaný operátor implementace pro operace, kde jeden nebo oba operandy jsou třídy nebo struktury typu uživatelem definované.

Následující tabulka shrnuje operátory jazyka C# společnosti, výpis kategorie operátor v pořadí dle priority od nejvyšší k nejnižší. Operátory ve stejné kategorii mají stejnou prioritu.


| __Kategorie__                     | __Výraz__    | __Popis__ |
|----------------------------------|-------------------|-----------------|
| Primární                          | `x.m`             | Přístup ke členům |
|                                  | `x(...)`          | Vyvolání metod a delegátů |
|                                  | `x[...]`          | Přístup k poli a indexeru |
|                                  | `x++`             | Postinkrementace |
|                                  | `x--`             | Postdekrementace |
|                                  | `new T(...)`      | Vytvoření objektu a delegátu |
|                                  | `new T(...){...}` | Vytvoření objektu s inicializátorem |
|                                  | `new {...}`       | Inicializátor anonymních objektů |
|                                  | `new T[...]`      | Vytvoření pole |
|                                  | `typeof(T)`       | Získat `System.Type` objekt pro `T` |
|                                  | `checked(x)`      | Vyhodnocení výrazu ve zkontrolovaném kontextu |
|                                  | `unchecked(x)`    | Vyhodnocení výrazu v nezkontrolovaném kontextu |
|                                  | `default(T)`      | Získat výchozí hodnotu typu `T` |
|                                  | `delegate {...}`  | Anonymní funkce (anonymní metoda) |
| Unární                            | `+x`              | Identita |
|                                  | `-x`              | Negace |
|                                  | `!x`              | Logická negace |
|                                  | `~x`              | Bitová negace. |
|                                  | `++x`             | Preinkrementace |
|                                  | `--x`             | Predekrementace |
|                                  | `(T)x`            | Explicitně převést `x` na typ `T` |
|                                  | `await x`         | Asynchronně počkejte `x` k dokončení |
| Násobení                   | `x * y`           | Násobení |
|                                  | `x / y`           | Dělení |
|                                  | `x % y`           | Zbytek |
| Additive                         | `x + y`           | Sčítání, řetězení řetězců, kombinování delegátů |
|                                  | `x - y`           | Odčítání, odebrání delegátů |
| SHIFT                            | `x << y`          | Posun doleva |
|                                  | `x >> y`          | Posun doprava |
| Relační a typové zkoušky      | `x < y`           | Menší než |
|                                  | `x > y`           | Větší než |
|                                  | `x <= y`          | Menší nebo rovno |
|                                  | `x >= y`          | Větší nebo rovno |
|                                  | `x is T`          | Vrátí `true` Pokud `x` je `T`, `false` jinak |
|                                  | `x as T`          | Vrátí `x` zadán jako `T`, nebo `null` Pokud `x` není `T` |
| Rovnost                         | `x == y`          | Rovno      |
|                                  | `x != y`          | Nerovná se |
| Logický operátor AND                      | `x & y`           | Celočíselné bitové a logických logický operátor AND |
| Logický operátor XOR                      | `x ^ y`           | Bitový operátor XOR celého čísla, logická hodnota operátoru XOR |
| Logický operátor OR                       | <code>x &#124; y</code> | Bitový operátor OR celého čísla, logická hodnota operátoru OR |
| Podmiňovací operátor AND                  | `x && y`          | Vyhodnotí `y` pouze tehdy, pokud `x` je `true` |
| Podmiňovací operátor OR                   | <code>x &#124;&#124; y</code> | Vyhodnotí `y` pouze tehdy, pokud `x` je `false` |
| Nulové sloučení                  | `X ?? y`          | Vyhodnotí jako `y` Pokud `x` je `null`do `x` jinak |
| Podmiňovací operátor                      | `x ? y : z`       | Vyhodnotí `y` Pokud `x` je `true`, `z` Pokud `x` je `false` |
| Přiřazení nebo anonymní funkce | `x = y`           | Přiřazení |
|                                  | `x op= y`         | Složené přiřazení. podporované operátory jsou `*=` `/=` `%=` `+=` `-=` `<<=` `>>=` `&=` `^=` <code>&#124;=</code> |
|                                  | `(T x) => y`      | Anonymní funkce (výraz lambda) |

## <a name="statements"></a>Příkazy

Akce programu jsou vyjádřeny pomocí ***příkazy***. C# podporuje několik různých druhů příkazů, číslo, které jsou definovány z hlediska vložených příkazů.

A ***bloku*** povoluje více příkazů, které má být zapsán v kontextech, kde je povoleno jeden příkaz. Blok obsahuje seznam příkazů, které jsou napsané mezi oddělovači `{` a `}`.

***Příkazy deklarace*** se používají k deklarování místní proměnné a konstanty.

***Příkazy výrazů*** se používají k vyhodnocení výrazů. Volání metod, obsahovat výrazy, které lze použít jako příkazy přidělení pomocí objektu `new` operátora, přiřazení pomocí `=` a operátory složeného přiřazení, operace Inkrementace a dekrementace pomocí `++`a `--` operátory a výrazy await.

***Příkazy výběru*** se používají k výběru jedním z možných příkazů pro spuštění na základě hodnoty některých výrazu. V této skupině jsou `if` a `switch` příkazy.

***Příkazy iterace*** se používají k vloženým příkazem spouštět opakovaně. V této skupině jsou `while`, `do`, `for`, a `foreach` příkazy.

***Jump – příkazy*** slouží k předání řízení. V této skupině jsou `break`, `continue`, `goto`, `throw`, `return`, a `yield` příkazy.

`try`... `catch` prohlášení se používá k zachycení výjimek, ke kterým dochází při provádění bloku, a `try`... `finally` prohlášení se používá k určení dokončovacího kódu, který je vždy spuštěn, zda došlo k výjimce, nebo ne.

`checked` a `unchecked` příkazy se používají k řízení kontroly kontext pro integrálového typu aritmetické operace a převody přetečení.

`lock` Prohlášení se používá k získání zámku vzájemné vyloučení pro daný objekt, provedení příkazu a uvolněte zámek.

`using` Prohlášení se používá k získání prostředku, provedení příkazu a potom tento prostředek.

Níže jsou příklady každý druh – příkaz

__Místní deklarace proměnné__

```csharp
static void Main() {
   int a;
   int b = 2, c = 3;
   a = 1;
   Console.WriteLine(a + b + c);
}
```


__Místní deklarace konstanty__

```csharp
static void Main() {
    const float pi = 3.1415927f;
    const int r = 25;
    Console.WriteLine(pi * r * r);
}
```


__příkaz výrazu__

```csharp
static void Main() {
    int i;
    i = 123;                // Expression statement
    Console.WriteLine(i);   // Expression statement
    i++;                    // Expression statement
    Console.WriteLine(i);   // Expression statement
}
```

__`if` – Příkaz__

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


__`switch` – Příkaz__

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

__`while` – Příkaz__

```csharp
static void Main(string[] args) {
    int i = 0;
    while (i < args.Length) {
        Console.WriteLine(args[i]);
        i++;
    }
}
```


__`do` – Příkaz__

```csharp
static void Main() {
    string s;
    do {
        s = Console.ReadLine();
        if (s != null) Console.WriteLine(s);
    } while (s != null);
}
```

__`for` – Příkaz__

```csharp
static void Main(string[] args) {
    for (int i = 0; i < args.Length; i++) {
        Console.WriteLine(args[i]);
    }
}
```

__`foreach` – Příkaz__

```csharp
static void Main(string[] args) {
    foreach (string s in args) {
        Console.WriteLine(s);
    }
}
```

__`break` – Příkaz__

```csharp
static void Main() {
    while (true) {
        string s = Console.ReadLine();
        if (s == null) break;
        Console.WriteLine(s);
    }
}
```

__`continue` – Příkaz__

```csharp
static void Main(string[] args) {
    for (int i = 0; i < args.Length; i++) {
        if (args[i].StartsWith("/")) continue;
        Console.WriteLine(args[i]);
    }
}
```

__`goto` – Příkaz__

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

__`return` – Příkaz__

```csharp
static int Add(int a, int b) {
    return a + b;
}

static void Main() {
    Console.WriteLine(Add(1, 2));
    return;
}
```

__`yield` – Příkaz__

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

__`throw` a `try` příkazy__

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

__`checked` a `unchecked` příkazy__

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

__`lock` – Příkaz__

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

__`using` – Příkaz__

```csharp
static void Main() {
    using (TextWriter w = File.CreateText("test.txt")) {
        w.WriteLine("Line one");
        w.WriteLine("Line two");
        w.WriteLine("Line three");
    }
}
```

## <a name="classes-and-objects"></a>Třídy a objekty

***Třídy*** jsou nejvíce základní typy jazyka C#. Třída je datová struktura, která kombinuje stavů (pole) a akcí (metody a další funkce členů) v jediné jednotce. Třída obsahuje definici pro dynamicky vytvořené ***instance*** třídy, označované také jako ***objekty***. Třídy podpory ***dědičnosti*** a ***polymorfismus***, mechanismy kterým ***odvozené třídy*** můžete rozšířit a specialize ***základních tříd***.

Nové třídy jsou vytvořené pomocí deklarace tříd. Deklarace třídy začíná hlavičku, která určuje atributy a modifikátory třídy, název třídy, základní třídy (Pokud je zadaný) a rozhraní implementované třídy. Záhlaví je následována tělo třídy, které se skládá ze seznamu deklarací členů napsané mezi oddělovači `{` a `}`.

Následuje deklaraci jednoduchou třídu pojmenovanou `Point`:

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
Jsou vytvořeny pomocí instance třídy `new` operátor, který přiděluje paměť pro novou instanci, vyvolá konstruktor k inicializaci instance a vrátí odkaz na instanci. Následující příkazy vytvoří dvě `Point` objektů a ukládají odkazy na tyto objekty ve dvou proměnných:

```
Point p1 = new Point(0, 0);
Point p2 = new Point(10, 20);
```
Paměť obsazena objekt je automaticky převzaty při objektu je již používán. To není nezbytné ani možné explicitně uvolnit objekty v jazyce C#.

### <a name="members"></a>Členové

Členy třídy jsou buď ***statické členy*** nebo ***členy instance***. Statické členy patří do třídy a členy instance patřit k objektům (instance třídy).

Následující tabulka obsahuje přehled druhy členů, které mohou obsahovat třídu.


| __Člen__   | __Popis__ |
|------------  |-----------------|
| Konstanty    | Konstantní hodnoty, které jsou přidružené k třídě |
| Pole       | Proměnné třídy |
| Metody      | Výpočtů a akcí, které lze provést pomocí třídy |
| Vlastnosti   | Akce přidružené k čtení a zápis s názvem vlastnosti třídy |
| Indexery     | Akce přidružené k indexování instancí třídy jako pole |
| Události       | Oznámení, která mohou být generovány třídy |
| Operátory    | Převody a podporovaných třídou operátory výrazů |
| Konstruktory | Akce potřebné k inicializaci instance třídy nebo vlastní třídy |
| Destruktory  | Akce k provedení před instancí třídy budou trvale odstraněny |
| Typy        | Vnořené typy deklarované pomocí třídy |

### <a name="accessibility"></a>Usnadnění

Každý člen třídy má přidružené usnadnění přístupu, který řídí oblasti textem programu, které budou mít přístup k členu. Existuje pět možné formy usnadnění přístupu. Ty jsou shrnuté v následující tabulce.


| __Usnadnění__    | __Význam__ |
|----------------------|-----------------|
| `public`             | Přístup mimo jiné |
| `protected`          | Přístup pouze pro tuto třídu nebo třídy odvozené z této třídy |
| `internal`           | Přístup omezen na tento program |
| `protected internal` | Přístup omezen na tento program nebo třídy odvozené z této třídy |
| `private`            | Přístup pouze pro tuto třídu |

### <a name="type-parameters"></a>Parametry typu

Definice třídy může určit sadu parametrů typu podle názvu třídy s ostré závorky uzavírající seznam názvy parametrů typů. Parametry typu, můžete použít v těle deklarace tříd k definování členy třídy. V následujícím příkladu, parametry typu `Pair` jsou `TFirst` a `TSecond`:

```csharp
public class Pair<TFirst,TSecond>
{
    public TFirst First;
    public TSecond Second;
}
```
Typ třídy, který je deklarován mít parametry typu se nazývá typu obecné třídy. Typy struktury, rozhraní a delegátů mohou být obecný.

Při použití obecné třídy musí být uvedeny argumentů typu pro jednotlivé parametry typu:

```csharp
Pair<int,string> pair = new Pair<int,string> { First = 1, Second = "two" };
int i = pair.First;     // TFirst is int
string s = pair.Second; // TSecond is string
```
Obecný typ s argumenty typů, které jsou k dispozici, jako je třeba `Pair<int,string>
    ` výš, se nazývá konstruovaný typ.

### <a name="base-classes"></a>Základní třídy

Deklarace třídy může určovat základní třídu pomocí následujících parametrů názvem a typem třídy pomocí dvojtečku a název základní třídy. Vynechání specifikace základní třídy je stejný jako odvozený od typu `object`. V následujícím příkladu základní třída `Point3D` je `Point`a základní třídu `Point` je `object`:

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
Třída dědí členy své základní třídy. Dědičnost znamená, že třídy implicitně obsahuje všechny členy své základní třídy, s výjimkou instance a statické konstruktory a destruktory základní třídy. Odvozené třídy můžete přidat nové členy pro ty, které se dědí, ale nemůže odstranit definici zděděného člena. V předchozím příkladu `Point3D` dědí `x` a `y` pole z `Point`a každý `Point3D` instance obsahuje tři pole `x`, `y`, a `z`.

Implicitní převod existuje z typu třídy pro některé typy jejího základní třídy. Proměnné typu třídy. proto odkazovat instance této třídy nebo instance všechny odvozené třídy. Například dány předchozí deklarace třídy, proměnné typu `Point` odkazovat buď `Point` nebo `Point3D`:

```csharp
Point a = new Point(10, 20);
Point b = new Point3D(10, 20, 30);
```

### <a name="fields"></a>Pole

Pole je proměnná, která souvisí s třídou nebo s instancí třídy.

Pole deklarované s `static` modifikátor definuje ***statické pole***. Statické pole identifikuje přesně jednoho umístění úložiště. Bez ohledu na to, kolik instancí třídy jsou vytvořeny je někdy pouze jednu kopii tohoto statické pole.

Pole deklarované bez `static` modifikátor definuje ***pole instance***. Každá instance třídy obsahuje kopii všechna pole instancí této třídy.

V následujícím příkladu, každá instance `Color` třída má samostatnou kopii `r`, `g`, a `b` instance pole, ale existuje pouze jedna kopie `Black`, `White`, `Red`, `Green`, a `Blue` statická pole:

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
Jak je znázorněno v předchozím příkladu ***pole jen pro čtení*** mohou být deklarovány s `readonly` modifikátor. Přiřazení `readonly` pole se můžou vyskytnout jenom jako součást deklarace pole nebo v konstruktoru ve stejné třídě.

### <a name="methods"></a>Metody

A ***metoda*** je člen, který implementuje výpočtu nebo akce, která může provádět k objektu nebo třídě. ***Statické metody*** jsou přístupné prostřednictvím třídy. ***Instance metody*** jsou přístupné prostřednictvím instance třídy.

Metody mají (pravděpodobně prázdná) seznam ***parametry***, které představují hodnoty nebo odkazy na proměnné předaný metodě a ***návratový typ***, která určuje typ hodnoty vypočítané a vrácené Metoda. Návratový typ metody je `void` Pokud nevrací hodnotu.

Podobně jako typy metody mají také sadu parametrů typu, pro které musí být zadán argumenty typu při volání metody. Na rozdíl od typů argumenty typu často jde odvodit z argumentů volání metody a nemusí být explicitně uvedena.

***Podpis*** metody musí být jedinečný ve třídě, ve kterém je deklarována metodu. Podpis metody se skládá z názvu metody, počet parametrů typu a číslo, modifikátory a typy ze svých parametrů. Podpis metody neobsahuje návratovým typem.

#### <a name="parameters"></a>Parametry

Parametry se používají k předávání hodnot nebo proměnné odkazy na metody. Parametry metody získávají skutečné hodnoty z ***argumenty*** , které jsou určeny při vyvolání metody. Existují čtyři druhy parametry: hodnoty, parametry, parametry odkazů, výstupní parametry a pole parametrů.

A ***parametr hodnoty*** slouží k předávání vstupních parametrů. Hodnota parametru odpovídá místní proměnná, která se získá z argumentu, který byl předán parametr počáteční hodnoty. Úpravy parametru hodnoty nemají vliv na argument, který byl předán parametr.

Parametry s hodnotou může být volitelný, a zadat výchozí hodnotu tak, aby odpovídající argumenty lze vynechat.

A ***odkazovat na parametr*** se používá pro vstup a výstup předávání parametrů. Argument předaný pro referenční parametr musí být proměnná a při provádění metody referenční parametr představuje stejné úložiště jako argument proměnné. Parametr odkazu je deklarována s `ref` modifikátor. Následující příklad ukazuje použití `ref` parametry.

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
***Výstupní parametr*** slouží k předávání parametru výstupu. Výstupní parametr je podobný parametr odkazu, s tím rozdílem, že počáteční hodnota argumentu, pokud volající není důležitý. Výstupní parametr je deklarována s `out` modifikátor. Následující příklad ukazuje použití `out` parametry.

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
A ***pole parametrů*** povoluje proměnlivý počet argumentů, které mají být předána metodě. Pole parametrů je deklarována s `params` modifikátor. Poslední parametr metody může být pole parametrů a typ pole parametrů musí být typu jednorozměrné pole. `Write` a `WriteLine` metody `System.Console` třídy jsou dobrým příkladem použití pole parametrů. Jsou deklarovány následujícím způsobem.

```csharp
public class Console
{
    public static void Write(string fmt, params object[] args) {...}
    public static void WriteLine(string fmt, params object[] args) {...}
    ...
}
```
Pole parametrů se v rámci metody, která používá pole parametrů, chová stejně jako regulární parametr typu pole. Ve volání metody s polem parametrů, je možné předat buď jeden argument typu pole parametru nebo libovolný počet argumentů typu prvku pole parametrů. V druhém případě pole instance je automaticky vytvořen a inicializován pomocí dané argumenty. V tomto příkladu

```csharp
Console.WriteLine("x={0} y={1} z={2}", x, y, z);
```
je ekvivalentní zápisu následující.

```csharp
string s = "x={0} y={1} z={2}";
object[] args = new object[3];
args[0] = x;
args[1] = y;
args[2] = z;
Console.WriteLine(s, args);
```

#### <a name="method-body-and-local-variables"></a>Tělo metody a lokální proměnné

Tělo metody určuje příkazy ke spuštění při vyvolání metody.

Tělo metody můžete deklarovat proměnné, které jsou specifické pro vyvolání metody. Tyto proměnné jsou volány ***lokální proměnné***. Místní deklarace proměnné Určuje název typu, název proměnné a případně na počáteční hodnotu. Následující příklad deklaruje místní proměnnou `i` s počáteční hodnotou nula a místní proměnnou `j` s žádná počáteční hodnota.

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
C# vyžaduje místní proměnnou ***jednoznačně přiřazena*** před její hodnotu lze získat. Například pokud deklarace předchozí `i` počáteční hodnotu neobsahuje, kompilátor by oznámit chybu pro následné použití `i` protože `i` nemusí být jednoznačně přiřazena v těchto bodech v programu.

Můžete použít metodu `return` příkazy vrátí řízení volajícímu. Do metody vracející `void`, `return` příkazy nemohou zadat výraz. Metoda vrací jinou hodnotu než`void`, `return` příkazů musí obsahovat výraz, který vypočítá návratovou hodnotu.

#### <a name="static-and-instance-methods"></a>Statické a instance metody

Metoda deklarována s `static` modifikátor ***statickou metodu***. Statická metoda nefunguje na konkrétní instanci a pouze přímý přístup k statické členy.

Metoda deklarovaná bez `static` modifikátor ***metodu instance***. Metodu instance funguje na konkrétní instanci, můžete přístup ke statické a členy instance. Instance, ve kterém byla vyvolána metoda instance je explicitně přístupná jako `this`. Jedná se o chybu k odkazování na `this` uvnitř statické metody.

Následující `Entity` má statické třídy a členy instance.

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
Každý `Entity` instance obsahuje sériové číslo (a pravděpodobně některé další informace, které tady není ukázaný). `Entity` Konstruktoru (což je jako metoda instance) inicializuje novou instanci s další dostupné sériové číslo. Protože konstruktor je členem instance, je povoleno přístup i `serialNo` pole instance a `nextSerialNo` statické pole.

`GetNextSerialNo` a `SetNextSerialNo` statické metody se dostanete `nextSerialNo` statické pole, ale bude k chybě pro ně umožňuje přímý přístup k `serialNo` pole instance.

Následující příklad ukazuje použití `Entity` třídy.

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
Všimněte si, že `SetNextSerialNo` a `GetNextSerialNo` statické metody jsou vyvolány ve třídě, kdežto `GetSerialNo` metodu instance se vyvolá u instance třídy.

#### <a name="virtual-override-and-abstract-methods"></a>Virtual, override a abstraktní metody

Při deklaraci instance metody obsahuje `virtual` modifikátor, metoda se říká, že ***virtuální metoda***. Pokud ne `virtual` Modifikátor je k dispozici, metodu se říká, že ***nevirtuální metoda***.

Při vyvolání virtuální metody, ***run-time typu*** instance, pro který přebírá tento vyvolání místo určuje implementaci skutečné metoda k vyvolání. Ve volání nevirtuální metody ***typu v době kompilace*** instance je určujícím faktorem.

Virtuální metoda může být ***přepsat*** v odvozené třídě. Při deklaraci instance metody obsahuje `override` Modifikátor metody přepsání zděděné virtuální metody se stejným podpisem. Vzhledem k tomu virtuální metoda deklarace zavádí nové metody, specializuje deklaraci metody přepsání existující zděděnou virtuální metodu tím, že poskytuje novou implementaci této metody.

***Abstraktní*** je virtuální metoda bez implementace. Abstraktní metoda je deklarována s `abstract` modifikátor a smí obsahovat pouze ve třídě, která je také deklarován `abstract`. Abstraktní metody musí být přepsána v každé třídě odvozené neabstraktní.

Následující příklad deklaruje abstraktní třídu, `Expression`, který představuje uzel stromu výrazu a tři odvozené třídy, `Constant`, `VariableReference`, a `Operation`, které implementují uzly stromu výrazů konstant, proměnných Reference a aritmetické operace. (Je to podobné, ale neměl by se zaměňovat s typy výrazů stromu počínaje [typy stromu výrazů](types.md#expression-tree-types)).

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
Předchozí čtyři třídy lze použít k modelování aritmetických výrazech. Například použití instance těchto tříd, výraz `x + 3` můžou být vyjádřeny následujícím způsobem.

```csharp
Expression e = new Operation(
    new VariableReference("x"),
    '+',
    new Constant(3));
```
`Evaluate` Metodu `Expression` instance se vyvolá, aby vyhodnotit tento výraz a vytvářet `double` hodnotu. Tato metoda přebírá jako argument `Hashtable` , který obsahuje názvy proměnných (jako klíče položky) a hodnoty (jako hodnoty položek). `Evaluate` Je virtuální abstraktní metoda, to znamená neabstraktní odvozené třídy musí přepsat je k dispozici skutečné implementace.

A `Constant`vaší implementace `Evaluate` jednoduše vrací uložené – konstanta. A `VariableReference`vaší implementace vyhledá název proměnné v zatřiďovací tabulky a vrátí výslednou hodnotu. `Operation`Na provádění nejprve vyhodnotí jako operandy vlevo a vpravo (vyvoláním rekurzivně jejich `Evaluate` metody) a potom provede daný aritmetické operace.

Následující program používá `Expression` třídy pro vyhodnocení výrazu `x * (y + 2)` pro různé hodnoty `x` a `y`.

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

#### <a name="method-overloading"></a>Přetěžování metody

Metoda ***přetížení*** povoluje více metod ve stejné třídě, aby mají stejný název tak dlouho, dokud mají jedinečný podpis. Při kompilaci vyvolání přetěžované metody, kterou kompilátor používá ***rozlišení přetěžování*** určit konkrétní metodu chce volat. Řešení přetížení vyhledá jednu metodu, nejlépe odpovídá argumenty nebo hlásí chybu, pokud lze najít žádné jediné nejlepší shodu. Následující příklad ukazuje přetížení v platnosti. Komentář pro každé vyvolání v `Main` metoda ukazuje, jakou metodu ve skutečnosti je vyvolána.

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
Jak je znázorněno v příkladu, konkrétní metody lze vybrat vždy explicitně použití argumentů na typy parametrů přesný nebo explicitně zadávání argumentů typu.

### <a name="other-function-members"></a>Ostatní členové – funkce

Členy, které obsahují spustitelného kódu jsou souhrnně označovány jako ***funkce členy*** třídy. Předchozí část popisuje metody, které jsou primární druh členy funkce. Tato část popisuje další druhy členů funkce podporované v jazyce C#: konstruktory, vlastnosti, indexery, události, operátory a destruktory.

Následující kód ukazuje obecnou třídu volá `List<T>`, který implementuje growable seznam objektů. Třída obsahuje několik příkladů nejběžnější druhy členů funkce.


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

#### <a name="constructors"></a>Konstruktory

C# podporuje instance a statické konstruktory. ***Konstruktor instance*** je člen, který implementuje akce potřebné k inicializaci instance třídy. A ***statický konstruktor*** je člen, který implementuje akce potřebné k inicializaci třídy samotný při prvním načtení.

Konstruktor je deklarován jako metoda bez návratového typu a stejný název jako třídu obsahující. Pokud obsahuje deklaraci konstruktoru `static` modifikátor, deklaruje statický konstruktor. V opačném případě deklaruje konstruktor instance.

Konstruktory instancí můžou být přetížené. Například `List<T>
` třída deklaruje dva konstruktory instancí, jednu s žádné parametry a ten, který přebírá `int` parametru. Konstruktory instancí jsou vyvolány pomocí `new` operátor. Následující příkazy přidělit dvě `List<string>
` instance pomocí všech konstruktory `List` třídy.

```csharp
List<string> list1 = new List<string>();
List<string> list2 = new List<string>(10);
```
Na rozdíl od jiných členů nejsou zděděné konstruktory instancí a třída nemá žádné konstruktory instance než tyto skutečně deklarovaná ve třídě. Pokud žádný konstruktor instance není zadána pro třídu, pak prázdná bez parametrů je automaticky zadáno.

#### <a name="properties"></a>Vlastnosti

***Vlastnosti*** jsou přirozené rozšíření polí. Jsou pojmenované členy s přidružené typy a syntaxe pro přístup k vlastnosti a pole je stejná. Na rozdíl od pole, ale vlastnosti nejsou označení umístění úložiště. Místo toho mají vlastnosti ***přistupující objekty*** , které určují příkazy ke se spustí při jejich hodnoty jsou číst nebo zapisovat.

Vlastnost je deklarován jako pole, s tím rozdílem, že deklarace končí `get` přístupového objektu a/nebo `set` přistupující objekt napsané mezi oddělovači `{` a `}` místo končí středníkem. Vlastnost, která má oba `get` přístupového objektu a `set` přístupový objekt není ***čtení a zápis vlastnosti***, vlastnost, která má pouze `get` přístupový objekt není ***vlastnost jen pro čtení***a vlastnost, která má pouze `set` přístupový objekt není ***vlastnost jen pro zápis***.

A `get` přistupující objekt odpovídající konstruktor bez parametrů metody s návratovou hodnotou typu vlastnosti. Při odkazování na vlastnost ve výrazu, s výjimkou případů cílem přiřazení, `get` přistupující objekt vlastnosti je vyvolána k výpočtu hodnoty vlastnosti.

A `set` přistupující objekt odpovídá metodu s jedním parametrem s názvem `value` a bez návratového typu. Když je jako cíl přiřazení nebo jako operand odkazuje vlastnost `++` nebo `--`, `set` přístupového objektu je volána s argumentem, který obsahuje novou hodnotu.

`List<T>
` Třída deklaruje dvě vlastnosti `Count` a `Capacity`, které jsou v uvedeném pořadí jen pro čtení a čtení i zápis. Následuje příklad použití těchto vlastností.

```csharp
List<string> names = new List<string>();
names.Capacity = 100;            // Invokes set accessor
int i = names.Count;             // Invokes get accessor
int j = names.Capacity;          // Invokes get accessor
```
Podobně jako k polím a metodám, C# podporuje vlastnosti instance a statické vlastnosti. Statické vlastnosti jsou deklarovány pomocí `static` modifikátor a vlastnosti instance jsou deklarovány bez něj.

Accessor(s) vlastnost může být virtuální. Pokud obsahuje deklaraci vlastnosti `virtual`, `abstract`, nebo `override` modifikátor, se vztahuje na accessor(s) vlastnost.

#### <a name="indexers"></a>Indexery

***Indexer*** je člen, který umožňuje objekty, které mají být indexovány v stejným způsobem jako pole. Indexer je deklarován jako vlastnost, s tím rozdílem, že je název člena `this` za nímž následuje seznam parametrů napsané mezi oddělovači `[` a `]`. Parametry jsou k dispozici v accessor(s) indexeru. Podobně jako u vlastnosti, indexery mohou být pro čtení i zápis, jen pro čtení a jen pro zápis a accessor(s) indexer může být virtuální.

`List` Třída deklaruje jednu indexeru pro čtení i zápis, která přebírá `int` parametru. Indexer umožňuje index `List` instance s `int` hodnoty. Příklad

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
Indexery mohou být přetíženy, což znamená, že třídy lze deklarovat několik indexerů jako číslo nebo typů jejich parametrů se liší.

#### <a name="events"></a>Události

***Události*** je člen, který umožňuje třídu nebo objektu pro sdělení. Událost je deklarován jako pole, s tím rozdílem, že obsahuje prohlášení `event` – klíčové slovo a typ musí být typu delegáta.

V rámci třídy, které deklaruje člen události události se chová stejně jako pole s typem delegáta (za předpokladu, události není abstraktní a nedeklaruje přistupující objekty). Toto pole obsahuje odkaz na delegáta, který představuje obslužné rutiny událostí, které byly přidány k této události. Pokud neexistují žádné obslužné rutiny událostí, je pole `null`.

`List<T>
` Třída deklaruje člen jedna událost s názvem `Changed`, což znamená, že se do seznamu přidá nová položka. `Changed` Vyvolá událost `OnChanged` virtuální metody, které nejprve zkontroluje, zda je událost `null` (to znamená, že jsou k dispozici žádné obslužné rutiny). Pojem vyvolání události je přesně ekvivalentní k vyvolání delegáta představované událostí – to znamená, nejsou žádné zvláštní jazykovým konstrukcím pro vyvolání události.

Klienti reagovat na události prostřednictvím ***obslužné rutiny událostí***. Obslužné rutiny událostí se připojují pomocí `+=` operátor a odebrané pomocí `-=` operátor. Následující příklad připojí obslužnou rutinu události pro `Changed` události `List<string>
`.

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
Pro pokročilé scénáře, kde je žádoucí ovládací prvek pro použité úložiště událostí, můžete explicitně uvést deklaraci události `add` a `remove` přístupové objekty, které jsou poněkud podobně jako `set` přistupujícího objektu vlastnosti.

#### <a name="operators"></a>Operátory

***Operátor*** je člen, který definuje význam použití operátoru konkrétní výraz do instance třídy. Je možné definovat tři typy operátorů: unární operátory, binární operátory a operátory převodu. Všechny operátory musí být deklarována jako `public` a `static`.

`List<T>
` Třída deklaruje dva operátory `operator==` a `operator!=`a proto poskytuje nový význam pro výrazy, které se vztahují na tyto operátory `List` instancí. Konkrétně definovat operátory rovnosti dvou `List<T>
` instance vyjádřený jako porovnání všech obsažených objektů pomocí jejich `Equals` metody. V následujícím příkladu `==` operátor pro porovnání dvou `List<int>
` instancí.

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

První `Console.WriteLine` výstupy `True` vzhledem k tomu, že oba seznamy obsahují stejný počet objektů se stejnými hodnotami ve stejném pořadí. Měl `List<T>
` není definována `operator==`, první `Console.WriteLine` by mít výstup `False` protože `a` a `b` odkaz na jiný `List<int>
` instancí.

#### <a name="destructors"></a>Destruktory

A ***destruktor*** je člen, který implementuje akce potřebné k destrukci instance třídy. Destruktory, nemohou mít parametry nemohou mít modifikátory a nemůže být volány explicitně. Destruktor pro instanci je automaticky vyvolána při uvolnění.

Uvolňování paměti je povolená široké šířky při rozhodování, kdy se mají shromáždit objekty a spusťte destruktory. Konkrétně není deterministický. časování volání destruktoru a destruktory mohou být provedeny v libovolném vlákně. Pro tyto a další důvody třídy by měly implementovat destruktory pouze v případě, že žádná jiná řešení jsou vhodná.

`using` Příkaz poskytuje lepší přístup ke zničení objektu.

## <a name="structs"></a>Struktury

Třídy, jako jsou ***struktury*** datové struktury, které mohou obsahovat datové členy a funkční členy jsou, ale na rozdíl od tříd, struktur jsou typy hodnot a nevyžadují přidělení haldy. Proměnné typu Struktura přímo ukládá datové struktury, že proměnné typu třída uchovává odkaz na do dynamicky alokovaného objektu. Typy struktury nepodporují dědičnosti zadané uživatelem, a všechny typy struktury implicitně dědí z typu `object`.

Struktury jsou zvláště užitečná pro malé datové struktury, které mají hodnotu sémantiku. Komplexní čísla, body v souřadnicovém systému nebo páry klíč hodnota do slovníku jsou všechny dobrým příkladem struktury. Použití struktur, místo třídy pro malé datové struktury mohou přinést důležité velký počet přidělení paměti aplikace provádí. Například následující program vytvoří a inicializuje pole 100 bodů. S `Point` implementován jako třída, 101 samostatné objekty jsou vytvořena instance – jednu pro pole a jeden pro 100 elementů.

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
Alternativou je, aby `Point` struktury.

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
Nyní pouze jeden objekt je vytvořena instance – jednu pro pole – a `Point` instance jsou uložené v řádku v poli.

Konstruktory struktury jsou vyvolány pomocí `new` operátoru, ale to neznamená, že paměť přidělena. Namísto dynamického přidělování objektu a vrací se odkaz na to jednoduše vrací hodnotu – struktura (obvykle do dočasného umístění v zásobníku), konstruktor struktury a tato hodnota se pak zkopíruje podle potřeby.

Pomocí třídy je možné pro dvě proměnné odkazovat na stejný objekt a proto je možné pro operace v rámci jedné proměnné ovlivňovat objekt odkazovaný jinou proměnnou. Struktury proměnné každý mají své vlastní kopii dat, a není možné pro operace se na nich se má vliv na jinou. Například výstup vytvořený podle následující fragment kódu závisí na tom, zda `Point` třída nebo struktura.

```csharp
Point a = new Point(10, 10);
Point b = a;
a.x = 20;
Console.WriteLine(b.x);
```
Pokud `Point` je třída, výstup je `20` protože `a` a `b` odkazovat na stejný objekt. Pokud `Point` je struktura, výstup je `10` protože přiřazení `a` k `b` vytvoří kopii hodnoty, a tuto kopii není ovlivněn následné přiřazení `a.x`.

Předchozí příklad ukazuje dvě omezení struktury. Kopírování celé struktury je poprvé, obvykle méně efektivní než odkaz na objekt, kopírování, předávání přiřazení a hodnota parametru může být dražší s struktury než s typy odkazů. Druhý, s výjimkou `ref` a `out` parametry, není možné vytvořit odkazy na struktury, který vylučuje jejich použití v mnoha situacích.

## <a name="arrays"></a>Pole

***Pole*** je datová struktura, která obsahuje několik proměnných, které jsou přístupné prostřednictvím vypočítané indexy. Proměnné, které jsou obsaženy v poli, tzv. ***prvky*** pole, jsou všechny stejného typu a tento typ se nazývá ***typ elementu*** pole.

Typy pole jsou typy odkazů a deklarace proměnné pole jednoduše vyčleňuje místo odkazu na pole instance. Skutečné pole instance v dynamicky se vytvářejí s využitím za běhu `new` operátor. `new` Operace určovala ***délka*** nové instance pole, které se potom jsme opravili po dobu životnosti instance. Indexy prvků v rozsahu pole z `0` k `Length - 1`. `new` Operátor automaticky inicializuje prvků pole na výchozí hodnoty, které je třeba nulu pro všechny číselné typy a `null` pro všechny typy odkazů.

Následující příklad vytvoří pole `int` prvky, inicializuje pole a vytiskne obsah pole.

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
Tento příklad vytvoří a pracuje ***jednorozměrné pole***. C# rovněž podporuje ***vícerozměrných polí***. Zadejte počet rozměrů pole, označované také jako ***pořadí*** typ pole je jednu plus počet čárek mezi hranaté závorky pole zapsána. V následujícím příkladu se přiděluje jednorozměrný dvourozměrném a trojrozměrného pole.

```csharp
int[] a1 = new int[10];
int[,] a2 = new int[10, 5];
int[,,] a3 = new int[10, 5, 2];
```
`a1` Pole obsahuje 10 prvků `a2` pole obsahuje 50 (10 × 5) elementy a `a3` 100 (10 × 5 × 2) obsahuje pole prvků.

Typ elementu pole může být libovolného typu, včetně typu pole. Někdy se označuje jako pole s prvky typu pole ***vícenásobného pole*** protože ne všechny délky pole elementu mají být stejné. V následujícím příkladu se přiděluje pole polí `int`:

```csharp
int[][] a = new int[3][];
a[0] = new int[10];
a[1] = new int[5];
a[2] = new int[20];
```
První řádek vytvoří pole se třemi prvky, každý typ `int[]` a každý s počáteční hodnotou `null`. Následující řádky následně inicializujete tři prvky s odkazy na jednotlivá pole instancí z různých délek.

`new` Operátor umožňuje počáteční hodnoty prvků pole zadat pomocí ***inicializátor pole***, což je seznamem výrazů napsané mezi oddělovači `{` a `}`. V následujícím příkladu se přiděluje a inicializuje `int[]` s tři elementy.

```csharp
int[] a = new int[] {1, 2, 3};
```
Všimněte si, že délka pole je odvozen z počet výrazů mezi `{` a `}`. Lokální proměnná a pole deklarace lze zkrátit další tak, aby typ pole nemusí být revidovat.

```csharp
int[] a = {1, 2, 3};
```
Obě předchozí příklady jsou ekvivalentní následujícímu zápisu:

```csharp
int[] t = new int[3];
t[0] = 1;
t[1] = 2;
t[2] = 3;
int[] a = t;
```
## <a name="interfaces"></a>Rozhraní

***Rozhraní*** definuje kontrakt, který může být implementována třídy a struktury. Rozhraní může obsahovat metody, vlastnosti, události a indexery. Rozhraní neposkytuje implementace členů definuje – pouze Určuje členy, které je třeba dodat ze třídy nebo struktury, které implementují rozhraní.

Rozhraní může využívat ***vícenásobná dědičnost***. V následujícím příkladu, rozhraní `IComboBox` zdědí vlastnosti z obou `ITextBox` a `IListBox`.

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
Třídy a struktury mohou implementovat více rozhraní. V následujícím příkladu třída `EditBox` implementuje oba `IControl` a `IDataBound`.

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
Pokud konkrétní rozhraní implementuje, třídy nebo struktury, instance této třídy nebo struktury lze implicitně převést na tento typ rozhraní. Příklad

```csharp
EditBox editBox = new EditBox();
IControl control = editBox;
IDataBound dataBound = editBox;
```
V případech, kde není instance znám staticky implementovat určité rozhraní je možné dynamického přetypování. Například následující příkazy použijte k získání objektu dynamického přetypování `IControl` a `IDataBound` implementace rozhraní. Protože je skutečný typ objektu `EditBox`, úspěšné položky CAST.

```csharp
object obj = new EditBox();
IControl control = (IControl)obj;
IDataBound dataBound = (IDataBound)obj;
```
V předchozím `EditBox` třídy, `Paint` metodu z `IControl` rozhraní a `Bind` metodu z `IDataBound` rozhraní jsou implementovány pomocí `public` členy. C# rovněž podporuje ***implementace explicitního rozhraní členských***, který horizontálních oddílů pomocí třídy nebo struktury může předejděte tomu, aby členové `public`. Implementace explicitního rozhraní člen je zapsáno s použitím rozhraní plně kvalifikovaný název člena. Například `EditBox` třída může implementovat `IControl.Paint` a `IDataBound.Bind` metod pomocí explicitní implementace členů rozhraní následujícím způsobem.

```csharp
public class EditBox: IControl, IDataBound
{
    void IControl.Paint() {...}
    void IDataBound.Bind(Binder b) {...}
}
```
Explicitní rozhraní členy jsou přístupné pouze prostřednictvím typu rozhraní. Například provádění `IControl.Paint` poskytované předchozí `EditBox` třídy lze vyvolat pouze první převedením `EditBox` odkaz `IControl` typu rozhraní.

```csharp
EditBox editBox = new EditBox();
editBox.Paint();                        // Error, no such method
IControl control = editBox;
control.Paint();                        // Ok
```

## <a name="enums"></a>Výčty

***Typ výčtu*** je typ odlišné hodnoty se sadou pojmenovaných konstant. Následující příklad deklaruje a používá typ výčtu s názvem `Color` pomocí tří hodnot konstanty `Red`, `Green`, a `Blue`.

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
Každý typ enum má odpovídající integrální typ s názvem ***nadřízený typ*** typu výčtu. Typ výčtu, která nedeklaruje explicitně nadřízený typ má základní typ `int`. Formát úložiště a rozsah možných hodnot typu výčtu jsou určeny jeho nadřízeného typu. Sadu hodnot, které může přijmout typ výčtu není omezena jeho členy výčtu. Konkrétně se libovolná hodnota základního typu výčtu lze převést na typ výčtu a odlišné platná hodnota daného typu výčtu.

Následující příklad deklaruje typ výčtu s názvem `Alignment` pomocí základního typu `sbyte`.

```csharp
enum Alignment: sbyte
{
    Left = -1,
    Center = 0,
    Right = 1
}
```
Jak je znázorněno v předchozím příkladu, může obsahovat deklaraci člena výčtu konstantní výraz, který určuje hodnotu člena. Konstantní hodnoty pro každého člena výčtu musí být v rozsahu od základního typu výčtu. Při deklaraci člena výčtu explicitně neurčí hodnotu, se klíči přiřadí člen hodnotu nula (Pokud je první člen v typu výčtu) nebo hodnotu textový předchozí člen výčtu plus jedna.

Hodnoty výčtu lze převést na integrální hodnoty a naopak pomocí přetypování. Příklad

```csharp
int i = (int)Color.Blue;        // int i = 2;
Color c = (Color)2;             // Color c = Color.Blue;
```
Výchozí hodnota typu výčtu je integrální hodnotu nula, převést na typ výčtu. V případech, kdy proměnné jsou automaticky inicializovány na výchozí hodnotu jedná se o hodnotu k proměnné typů výčtu. Aby výchozí hodnota typu výčtu být snadno k dispozici, literál `0` implicitně převede na libovolný typ výčtu. Proto následující je povolený.

```csharp
Color c = 0;
```

## <a name="delegates"></a>Delegáty

A ***typ delegáta*** seznamu představuje odkazy na metody pomocí konkrétních parametrů a návratový typ. Delegáty umožňují považovat za entity, které může být přiřazena k proměnné a předány jako parametry metod. Delegáti jsou podobný koncept ukazatelů na funkce v některých jiných jazycích, ale na rozdíl od ukazatelů na funkce, Delegáti jsou objektově orientované a typově bezpečné.

Následující příklad deklaruje a používá typ delegáta s názvem `Function`.

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
Instance `Function` typ delegáta může odkazovat na jakoukoli metodu, která přijímá `double` argument a vrátí `double` hodnotu. `Apply` Postup se vztahuje dané `Function` na elementy `double[]`, vrácení `double[]` s výsledky. V `Main` metody `Apply` slouží k použití tří různých funkcí `double[]`.

Delegát může odkazovat na statickou metodu (například `Square` nebo `Math.Sin` v předchozím příkladu) nebo metodu instance (například `m.Multiply` v předchozím příkladu). Delegát, který odkazuje na metodu instance také odkazuje na konkrétní objekt, a když uživatel vyvolá metodu instance prostřednictvím delegáta, tento objekt se stane `this` ve volání.

Delegáty lze vytvořit také pomocí anonymní funkce, které jsou "inline metod", které jsou vytvořeny v reálném čase. Anonymní funkce můžete zobrazit místní proměnné okolního metody. Proto je výše uvedený příklad multiplikátor zapisovat snadněji bez použití `Multiplier` třídy:

```csharp
double[] doubles =  Apply(a, (double x) => x * 2.0);
```
Zajímavé a užitečné vlastnictví delegáta je, že ho neznáte nebo postaral o třídě, na které odkazuje; metody vše, co je důležité je, že odkazované metody má stejné parametry a návratový typ jako delegát.

## <a name="attributes"></a>Atributy

Typy, členy a dalších entit v programu v jazyce C# podporují modifikátory, které ovládat některé aspekty jejich chování. Například pro usnadnění metody je řízen pomocí `public`, `protected`, `internal`, a `private` modifikátory. C# umožňuje zobecnit tuto funkci tak, uživatelem definované typy deklarativní informací můžete připojit k programu entity a načíst v době běhu. Programy zadejte tyto dodatečné informace deklarativní definice a používání ***atributy***.

Následující příklad deklaruje `HelpAttribute` atribut, který může být umístěn v programu entity, které obsahují odkazy na jejich související dokumentaci.

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
Všechny třídy atributu odvozovat `System.Attribute` základní třídy, které poskytuje rozhraní .NET Framework. Atributy lze použít zadáním jeho názvu, spolu s žádné argumenty, v hranatých závorkách těsně před deklaraci přidružené. Pokud název atributu končí na `Attribute`, část názvu lze vynechat, pokud se odkazuje atribut. Například `HelpAttribute` atribut je možné následujícím způsobem.

```csharp
[Help("http://msdn.microsoft.com/.../MyClass.htm")]
public class Widget
{
    [Help("http://msdn.microsoft.com/.../MyClass.htm", Topic = "Display")]
    public void Display(string text) {}
}
```
Tento příklad připojí `HelpAttribute` k `Widget` třídy a další `HelpAttribute` k `Display` metody ve třídě. Veřejné konstruktory třídu atributu řídit informace, které musí být zadaná, když je připojena k entitě programu. Další informace lze zadat pomocí odkazu na veřejné čtení a zápis vlastnosti třídy atributů (jako je například odkaz na `Topic` vlastnost dříve).

Následující příklad ukazuje, jak můžete načíst informace o atributu pro daný program entitu v době běhu pomocí operace reflection.

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
Konkrétní atribut je žádost prostřednictvím reflexe, konstruktor pro třídu atributu je vyvolán pomocí informací uvedených ve zdrojovém programu a vrátí se výsledná instance atributu. Pokud Další informace se poskytovaly prostřednictvím vlastností, tyto vlastnosti jsou nastavené na dané hodnoty před vrácením instance atributu.
