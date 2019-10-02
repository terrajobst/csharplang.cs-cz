---
ms.openlocfilehash: 300d5fc2a2fadd98472d73c122226146605b01dd
ms.sourcegitcommit: 892af9016b3317a8fae12d195014dc38ba51cf16
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/01/2019
ms.locfileid: "71703973"
---
# <a name="introduction"></a>Úvod

C#(vyslovit "viz Sharp") je jednoduchý, moderní, objektově orientovaný programovací jazyk, který je typově bezpečný. C#má své kořeny v řadě jazyků C a bude se okamžitě seznámit s programátory v jazyce C++c, a Java. C#je standardizovaná ECMA mezinárodními jako standard ***ECMA-334*** a normou ISO/IEC jako standard ***iso/IEC 23270*** . C# Kompilátor od microsoftu pro .NET Framework je vyhovující implementaci obou těchto standardů.

C#je objektově orientovaný jazyk, ale C# dále zahrnuje podporu pro programování ***orientované na komponenty*** . Moderní návrh softwaru se stále spoléhá na softwarové komponenty ve formě integrovaných a samoobslužných balíčků funkcí. Klíč k takovým součástem je, že prezentují programovací model s vlastnostmi, metodami a událostmi. mají atributy, které poskytují deklarativní informace o komponentě. a obsahují vlastní dokumentaci. C#poskytuje jazykové konstrukce, které přímo podporují tyto koncepce, a C# vytváří tak velmi přirozený jazyk pro vytváření a používání softwarových komponent.

Několik C# funkcí pomáhá při konstrukci robustních a odolných aplikací: ***Uvolňování*** paměti automaticky uvolňuje paměť obsazené nepoužívanými objekty; ***zpracování výjimek*** poskytuje strukturovaný a rozšiřitelný přístup k detekci a obnovení chyb. a ***typově bezpečný*** návrh tohoto jazyka znemožňuje čtení z neinicializovaných proměnných, pro indexaci polí nad rámec jejich hranic nebo pro provedení nezaškrtnutých přetypování typu.

C#má ***jednotný systém typů***. Všechny C# typy, včetně primitivních typů `int` , jako jsou a `double`, dědí z jednoho `object` kořenového typu. Všechny typy tedy sdílí sadu běžných operací a hodnoty libovolného typu mohou být uloženy, přepravovány a provozovány konzistentním způsobem. Kromě toho C# podporuje uživatelsky definované typy odkazů i typy hodnot, což umožňuje dynamické přidělování objektů a také vložené úložiště lehkých struktur.

Aby bylo zajištěno, že se programy a knihovny můžou v průběhu času vyvíjejí kompatibilním způsobem, je mnohem zdůrazněno, že C# se v návrhu používá ***Správa verzí*** C#. Řada programovacích jazyků platíte jenom malým pozornostům tohoto problému. v důsledku toho jsou programy napsané v těchto jazycích častěji využívány, pokud jsou zavedeny novější verze závislých knihoven. C#Aspekty návrhu, které byly přímo ovlivněny aspekty správy verzí, zahrnují samostatné `virtual` a `override` modifikátory, pravidla pro řešení přetížení metod a podporu explicitních deklarací členů rozhraní.

Zbytek této kapitoly popisuje základní funkce C# jazyka. I když později kapitoly popisují pravidla a výjimky v detailním a někdy matematickém způsobu, tato kapitola usiluje o přehlednost a zkrácení na úkor úplnosti. Záměrem je poskytnout čtenářům Úvod k jazyku, který usnadní psaní dřívějších programů a čtení dalších kapitol.

## <a name="hello-world"></a>Hello World

Program "Hello, World" se tradičně používá k zavedení programovacího jazyka. Tady je C#:

```csharp
using System;

class Hello
{
    static void Main() {
        Console.WriteLine("Hello, World");
    }
}
```

C#zdrojové soubory mají obvykle příponu `.cs`souboru. Za předpokladu, že program "Hello, World" je uložen `hello.cs`v souboru, program může být zkompilován s kompilátorem společnosti Microsoft C# pomocí příkazového řádku
```console
csc hello.cs
```
který vytváří spustitelné sestavení s názvem `hello.exe`. Výstup vytvořený touto aplikací při spuštění je
```console
Hello, World
```

Program "Hello, World" začíná `using` direktivou, která odkazuje na `System` obor názvů. Obory názvů poskytují hierarchické prostředky pro C# uspořádání programů a knihoven. Obory názvů obsahují `System` typy a jiné obory názvů, například obor názvů obsahuje počet typů, jako je `Console` například třída odkazovaná v programu, a řadu `IO` dalších oborů názvů, například a `Collections`. `using` Direktiva, která odkazuje na daný obor názvů, umožňuje nekvalifikované použití typů, které jsou členy tohoto oboru názvů. Z `using` důvodu direktivy může program použít `Console.WriteLine` jako zkrácený pro `System.Console.WriteLine`.

Třída deklarovaná programem "Hello, World" má jednoho člena, metodu s názvem `Main`. `Hello` Metoda je deklarována `static` s modifikátorem. `Main` I když metody instance mohou odkazovat na konkrétní ohraničující objekt instance pomocí klíčového `this`slova, statické metody pracují bez odkazů na konkrétní objekt. Podle konvence, statická metoda s `Main` názvem slouží jako vstupní bod programu.

Výstup programu je vytvořen `WriteLine` metodou `Console` třídy v `System` oboru názvů. Tato třída je poskytována knihovnami tříd .NET Framework, které jsou ve výchozím nastavení automaticky odkazovány kompilátorem společnosti C# Microsoft. Všimněte si C# , že sám nemá samostatnou běhovou knihovnu. Místo toho je .NET Framework běhová knihovna C#.

## <a name="program-structure"></a>Struktura programu

Klíčovou organizační koncepcí v C# nástroji jsou ***programy***, ***obory názvů***, ***typy***, ***členy***a ***sestavení***. C#programy se skládají z jednoho nebo více zdrojových souborů. Programy deklarují typy, které obsahují členy a mohou být uspořádány do oborů názvů. Příklady typů jsou třídy a rozhraní. Příklady členů jsou pole, metody, vlastnosti a události. Když C# jsou programy kompilovány, jsou fyzicky zabaleny do sestavení. Sestavení `.exe` mají obvykle příponu souboru nebo `.dll`v závislosti na tom, zda implementují ***aplikace*** nebo ***knihovny***.

Příklad

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
deklaruje třídu s názvem `Stack` v oboru názvů s názvem. `Acme.Collections` Plně kvalifikovaný název této třídy je `Acme.Collections.Stack`. Třída obsahuje několik členů: pole s `top`názvem, dvě metody pojmenované `Push` a `Pop`a vnořená třída s názvem `Entry`. Třída dále obsahuje tři členy: pole s názvem `next`, pole s názvem `data`a konstruktor. `Entry` Za předpokladu, že zdrojový kód příkladu je uložen v souboru `acme.cs`, příkazový řádek

```console
csc /t:library acme.cs
```
zkompiluje příklad jako knihovnu (kód bez `Main` vstupního bodu) a vytvoří sestavení s názvem. `acme.dll`

Sestavení obsahují spustitelný kód ve formě instrukcí pro ***převodní jazyk*** (IL) a symbolické informace ve formě ***metadat***. Před provedením je kód IL v sestavení automaticky převeden na kód specifický pro procesor pomocí kompilátoru JIT (just-in-time) modulu CLR (Common Language Runtime) .NET.

Vzhledem k tomu, že sestavení je samo-popisující jednotku funkcí obsahující kód i metadata, není nutné `#include` direktivy a hlavičkové soubory v. C# Veřejné typy a členy, které jsou obsaženy v konkrétním sestavení, jsou v C# programu k dispozici jednoduše odkazem na toto sestavení při kompilování programu. Například tento program používá `Acme.Collections.Stack` třídu `acme.dll` ze sestavení:

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
Pokud je program `test.cs`uložen v souboru, když `test.cs` je zkompilován, `acme.dll` `/r` sestavení lze odkazovat pomocí možnosti kompilátoru:

```console
csc /r:acme.dll test.cs
```
Tím se vytvoří spustitelné sestavení s `test.exe`názvem, které při spuštění vytvoří výstup:

```console
100
10
1
```
C#umožňuje, aby byl zdrojový text programu uložen v několika zdrojových souborech. Když je zkompilován program pro C# více souborů, všechny zdrojové soubory jsou zpracovávány společně a zdrojové soubory mohou volně odkazovat, a to koncepční, protože všechny zdrojové soubory byly před zpracováním sloučeny do jednoho velkého souboru. Dopředné deklarace nejsou nikdy C# potřeba v důsledku nedostatečných výjimek, pořadí deklarací je nedůležité. C#neomezuje zdrojový soubor na deklaraci pouze jednoho veřejného typu, ani nevyžaduje název zdrojového souboru, aby odpovídal typu deklarovanému ve zdrojovém souboru.

## <a name="types-and-variables"></a>Typy a proměnné

Existují dva druhy typů v C#: ***typy hodnot*** a ***typy odkazů***. Proměnné typů hodnot přímo obsahují svá data, zatímco proměnné typu odkazu ukládají odkazy na jejich data, přičemž ta se označují jako objekty. U typů odkazů je možné, aby dvě proměnné odkazovaly na stejný objekt a bylo tak možné, aby operace s jednou proměnnou ovlivnily objekt, na který je odkazováno z jiné proměnné. S typy hodnot mají proměnné, které mají svou vlastní kopii dat, a není možné, aby operace na jednom byly ovlivněny druhou (kromě případu `ref` a `out` proměnných parametrů).

C#typy hodnot jsou dále rozděleny na ***jednoduché typy***, ***výčtové typy***, ***typy struktury***a ***typy s možnou hodnotou null***a C#odkazové typy jsou dále rozděleny do ***typů tříd***, ***typů rozhraní***, ***pole typy***a ***typy delegátů***.

Následující tabulka poskytuje přehled C#systému typů.

| __Kategorie__    |                 | __Popis__ |
|-----------------|-----------------|-----------------|
| Typy hodnot     | Jednoduché typy    | Podepsané integrály `sbyte`: `short`, `int`,,`long` |
|                 |                 | Unsigned integrál: `byte`, `ushort`, `uint`,`ulong` |
|                 |                 | Znaky Unicode:`char` |
|                 |                 | Plovoucí desetinná čárka IEEE: `float`,`double` |
|                 |                 | Desetinné číslo s vysokou přesností:`decimal` |
|                 |                 | Datového`bool` |
|                 | Výčtové typy      | Uživatelem definované typy formuláře`enum E {...}` |
|                 | Typy struktury    | Uživatelem definované typy formuláře`struct S {...}` |
|                 | Typy s povolenou hodnotou Null  | Rozšíření všech ostatních typů hodnot s `null` hodnotou |
| Typy odkazů | Typy tříd     | Nejvyšší základní třída všech ostatních typů:`object` |
|                 |                 | Řetězce Unicode:`string` |
|                 |                 | Uživatelem definované typy formuláře`class C {...}` |
|                 | Typy rozhraní | Uživatelem definované typy formuláře`interface I {...}` |
|                 | Typy polí     | Jednoduché a multidimenzionální, například `int[]` a`int[,]` |
|                 | Typy delegátů  | Uživatelem definované typy formuláře, např.`delegate int  D(...)` |

Osm integrálních typů poskytuje podporu pro 8bitové, 16bitové, 32 a 64 hodnoty v podepsané nebo nepodepsané formě.

Dva typy `float` s plovoucí desetinnou čárkou a `double`jsou reprezentovány pomocí 32 formátů IEEE 754 s jednoduchou přesností a 64.

`decimal` Typ je 128 datový typ, který je vhodný pro finanční a peněžní výpočty.

C#typ se používá k reprezentaci logických hodnot – hodnot, které `true` jsou buď nebo `false`. `bool`

Zpracování znaků a řetězců v C# nástroji používá kódování Unicode. Typ představuje jednotku kódu UTF-16 `string` a typ představuje sekvenci jednotek kódu UTF-16. `char`

Následující tabulka shrnuje C#číselné typy.


| __Kategorie__      | __Bit__ | __Typ__  | __Rozsah/přesnost__ |
|-------------------|----------|-----------|---------------------|
| Podepsaný integrál   | 8        | `sbyte`   | -128...127 |
|                   | 16       | `short`   | -32 768... 32, 767 |
|                   | 32       | `int`     | -2147483648... 2, 147, 483, 647 |
|                   | 64       | `long`    | -9223372036854775808... 9, 223, 372, 036, 854, 0,75, 807 |
| Integrál bez znaménka | 8        | `byte`    | 0... 255 |
|                   | 16       | `ushort`  | 0... 65, 535 |
|                   | 32       | `uint`    | 0... 4, 294, 967, 295 |
|                   | 64       | `ulong`   | 0... 18, 446, 744, 073, 709, 551, 615 |
| Číslo s plovoucí desetinnou čárkou    | 32       | `float`   | 1,5 × 10 ^ − 45 až 3,4 × 10 ^ 38, 7 – přesnost číslic |
|                   | 64       | `double`  | 5,0 × 10 ^ − 324 a 1,7 × 10 ^ 308, přesnost na 15 číslic |
| Decimal           | 128      | `decimal` | 1,0 × 10 ^ − 28 až 7,9 × 10 ^ 28, 28 číslic – přesnost |

C#programy používají ***deklarace typů*** k vytváření nových typů. Deklarace typu Určuje název a členy nového typu. C#Pět kategorií typů je uživatelsky definované: typy tříd, typy struktury, typy rozhraní, výčtové typy a typy delegátů.

Typ třídy definuje datovou strukturu obsahující datové členy (pole) a členy funkce (metody, vlastnosti a další). Typy tříd podporují jednu dědičnost a polymorfismus, mechanismy, které mohou odvozené třídy roztáhnout a specializovat základní třídy.

Typ struktury je podobný typu třídy v tom, že představuje strukturu s datovými členy a členy funkce. Nicméně na rozdíl od tříd, struktury jsou typy hodnot a nevyžadují přidělení haldy. Typy struktury nepodporují uživatelem zadanou dědičnost a všechny typy struktury implicitně dědí z typu `object`.

Typ rozhraní definuje kontrakt jako pojmenovanou sadu členů veřejné funkce. Třída nebo struktura, která implementuje rozhraní, musí poskytovat implementace členů funkce rozhraní. Rozhraní může dědit z více základních rozhraní a třída nebo struktura může implementovat více rozhraní.

Typ delegáta představuje odkazy na metody s konkrétním seznamem parametrů a návratovým typem. Delegáti umožňují zacházet s metodami jako s entitami, které lze přiřadit proměnným a předávat jako parametry. Delegáti jsou podobní pojmu ukazatelů na funkce nalezené v některých jiných jazycích, ale na rozdíl od ukazatelů na funkce jsou delegáti objektově orientované a typově bezpečné.

Třídy, struktura, rozhraní a typy delegátů podporují obecné typy, kde je lze parametrizovanit s jinými typy.

Typ výčtu je odlišný typ s pojmenovanými konstantami. Každý typ výčtu má podkladový typ, který musí být jedním z osmi integrálních typů. Sada hodnot typu výčtu je stejná jako sada hodnot základního typu.

C#podporuje jedno a multidimenzionální pole libovolného typu. Na rozdíl od typů uvedených výše nemusí být typy polí deklarovány dříve, než mohou být použity. Místo toho jsou typy polí konstruovány pomocí názvu typu s hranatými závorkami. `int[]` Například je `int` `int` `int[][]` jednorozměrné pole `int[,]` , je dvourozměrné pole, a je jednorozměrné pole jednorozměrného pole v rámci. `int`

Typy s možnou hodnotou null také nemusí být deklarovány dříve, než mohou být použity. Pro každý typ `T` hodnoty, která není null, existuje odpovídající typ `T?`s možnou hodnotou null, který může `null`obsahovat další hodnotu. Například je typ, který může obsahovat libovolné celé číslo 32 nebo hodnotu `null`. `int?`

C#systém typů je sjednocený tak, že hodnota libovolného typu může být zpracována jako objekt. Každý typ C# přímo nebo nepřímo je odvozen z `object` typu třídy a `object` je nejvyšší základní třídou všech typů. Hodnoty typů odkazů se považují za objekty pouhým zobrazením hodnot jako typu `object`. Hodnoty typů hodnot se považují za objekty prováděním operací ***zabalení*** a ***rozbalení*** . V následujícím příkladu `int` je hodnota převedena na `object` a zpět na `int`.

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
Je-li hodnota typu hodnoty převedena na typ `object`, je přidělena instance objektu, která se také označuje jako "pole", pro uchování hodnoty a hodnota je zkopírována do tohoto pole. Naopak, pokud `object` je odkaz přetypování na typ hodnoty, je provedena kontrolu, že odkazovaný objekt je pole správného typu hodnoty, a pokud je ověření úspěšné, je hodnota v poli zkopírována.

C#Sjednocený typ systému efektivně znamená, že typy hodnot se můžou stát objekty na vyžádání. Z důvodu sjednocení, knihovny pro obecné účely, které používají typ `object` , lze použít s oběma typy odkazů i s typy hodnot.

Existuje několik druhů ***proměnných*** v C#, včetně polí, prvků pole, místních proměnných a parametrů. Proměnné reprezentují umístění úložiště a každá proměnná má typ, který určuje, jaké hodnoty mohou být uloženy v proměnné, jak je znázorněno v následující tabulce.


| __Typ proměnné__    | __Možný obsah__ |
|-------------------------|-----------------------|
| Typ hodnoty, která není null | Hodnota, která má přesný typ |
| Typ hodnoty s možnou hodnotou null     | Hodnota null nebo hodnota daného přesného typu |
| `object`                | Odkaz s hodnotou null, odkaz na objekt libovolného typu odkazu nebo odkaz na zabalenou hodnotu libovolného typu hodnoty |
| Typ třídy              | Odkaz s hodnotou null, odkaz na instanci tohoto typu třídy nebo odkaz na instanci třídy odvozené z tohoto typu třídy |
| Typ rozhraní          | Odkaz s hodnotou null, odkaz na instanci typu třídy, který implementuje tento typ rozhraní, nebo odkaz na zabalenou hodnotu typu hodnoty, který implementuje tento typ rozhraní |
| Typ pole              | Odkaz s hodnotou null, odkaz na instanci tohoto typu pole nebo odkaz na instanci kompatibilního typu pole |
| Typ delegáta           | Odkaz s hodnotou null nebo odkaz na instanci tohoto typu delegáta |

## <a name="expressions"></a>Výrazy

***Výrazy*** jsou vytvořené z ***operandů*** a ***operátorů***. Operátory výrazu označují, které operace se mají použít u operandů. Příklady operátorů zahrnují `+`, `-`, `*` `/`, a .`new` Příklady operandů zahrnují literály, pole, místní proměnné a výrazy.

Pokud výraz obsahuje více operátorů, ***Priorita*** operátorů řídí pořadí, ve kterém jsou jednotlivé operátory vyhodnocovány. Například výraz `x + y * z` je vyhodnocen jako `x + (y * z)` , protože `*` operátor má vyšší prioritu než `+` operátor.

Většina operátorů může být ***přetížená***. Přetížení operátoru umožňuje zadat uživatelsky definované implementace operátorů pro operace, u nichž jeden nebo oba operandy jsou uživatelsky definovaný typ třídy nebo struktury.

Následující tabulka shrnuje C#operátory, které obsahují kategorie operátora v pořadí podle priority od nejvyšších po nejnižší. Operátory ve stejné kategorii mají stejnou prioritu.


| __Kategorie__                     | __Vyjádření__    | __Popis__ |
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
|                                  | `typeof(T)`       | Získat `System.Type` objekt pro`T` |
|                                  | `checked(x)`      | Vyhodnocení výrazu ve zkontrolovaném kontextu |
|                                  | `unchecked(x)`    | Vyhodnocení výrazu v nezkontrolovaném kontextu |
|                                  | `default(T)`      | Získat výchozí hodnotu typu`T` |
|                                  | `delegate {...}`  | Anonymní funkce (anonymní metoda) |
| Unární                            | `+x`              | Identita |
|                                  | `-x`              | Negace |
|                                  | `!x`              | Logická negace |
|                                  | `~x`              | Bitová negace. |
|                                  | `++x`             | Preinkrementace |
|                                  | `--x`             | Predekrementace |
|                                  | `(T)x`            | Explicitně převést `x` na typ`T` |
|                                  | `await x`         | Asynchronní čekání `x` na dokončení |
| Multiplikativní                   | `x * y`           | Násobení |
|                                  | `x / y`           | Dělení |
|                                  | `x % y`           | Zbytek |
| Přičítáním                         | `x + y`           | Sčítání, řetězení řetězců, kombinování delegátů |
|                                  | `x - y`           | Odčítání, odebrání delegátů |
| SHIFT                            | `x << y`          | Posun doleva |
|                                  | `x >> y`          | Posun doprava |
| Relační testování a testování typu      | `x < y`           | Je menší než |
|                                  | `x > y`           | Je větší než |
|                                  | `x <= y`          | Menší nebo rovno |
|                                  | `x >= y`          | Větší nebo rovno |
|                                  | `x is T`          | Vrátí `true` ,`T`Pokud `x` je, jinak`false` |
|                                  | `x as T`          | Typ `x` se vrátí `T`jako nebo `null` Pokud `x` není`T` |
| Rovnost                         | `x == y`          | Rovno      |
|                                  | `x != y`          | Nerovná se |
| Logický operátor AND                      | `x & y`           | Celočíselná bitová a logická logická hodnota |
| Logický operátor XOR                      | `x ^ y`           | Bitový operátor XOR celého čísla, logická hodnota operátoru XOR |
| Logický operátor OR                       | <code>x &#124; y</code> | Bitový operátor OR celého čísla, logická hodnota operátoru OR |
| Podmiňovací operátor AND                  | `x && y`          | Vyhodnotí `x`pouzev případě, že je `y``true` |
| Podmiňovací operátor OR                   | <code>x &#124;&#124; y</code> | Vyhodnotí `x`pouzev případě, že je `y``false` |
| Nulové sloučení                  | `x ?? y`          | Vyhodnotí jako `x` v `null` `x` případě, že je, do jiného `y` |
| Podmiňovací operátor                      | `x ? y : z`       | `x` `true` `z` Vyhodnotí`x` , pokud je, pokud je `y``false` |
| Přiřazení nebo anonymní funkce | `x = y`           | Přiřazení |
|                                  | `x op= y`         | Složené přiřazení; podporované operátory jsou `*=` `/=` `%=` `+=` `-=` `<<=` `>>=` `&=` `^=`<code>&#124;=</code> |
|                                  | `(T x) => y`      | Anonymní funkce (výraz lambda) |

## <a name="statements"></a>Příkazy

Akce programu jsou vyjádřeny pomocí ***příkazů***. C#podporuje několik různých druhů příkazů, jejichž počet je definován z podmínek vložených příkazů.

***Blok*** povoluje zápis více příkazů v kontextech, kde je povolen jediný příkaz. Blok obsahuje seznam příkazů zapsaných mezi oddělovači `{` a. `}`

***Příkazy deklarace*** slouží k deklaraci lokálních proměnných a konstant.

***Příkazy výrazů*** se používají k vyhodnocení výrazů. Výrazy, které lze použít jako příkazy, zahrnují vyvolání metod, přidělení objektů pomocí `new` operátoru, přiřazení pomocí `=` operátorů a složené operátory přiřazení, zvýšení a snížení provozu pomocí `++`operátory and a await. `--`

***Příkazy výběru*** se používají k výběru jednoho z několika možných příkazů pro spuštění na základě hodnoty nějakého výrazu. V této skupině jsou `if` příkazy a. `switch`

***Příkazy iterace*** se používají k opakovanému provedení vloženého příkazu. V této `while`skupině jsou `foreach` příkazy, `do`, `for`a.

***Příkazy skoku*** slouží k přenosu ovládacího prvku. V této skupině jsou `break`příkazy, `continue`, `goto`, `throw`, `return`a. `yield`

`try`... příkaz slouží k zachycení výjimek, ke kterým dojde během provádění bloku, `try`a... `catch` `finally` příkaz slouží k určení výsledného kódu, který je vždy spuštěn, zda došlo k výjimce nebo nikoli.

Příkazy `checked` a`unchecked` slouží k řízení kontextu kontroly přetečení pro aritmetické operace a převody integrálního typu.

`lock` Příkaz slouží k získání zámku vzájemného vyloučení pro daný objekt, spuštění příkazu a uvolnění zámku.

`using` Příkaz slouží k získání prostředku, spuštění příkazu a následnému uvolnění tohoto prostředku.

Níže jsou uvedeny příklady každého typu příkazu.

__Deklarace místních proměnných__

```csharp
static void Main() {
   int a;
   int b = 2, c = 3;
   a = 1;
   Console.WriteLine(a + b + c);
}
```


__Deklarace místní konstanty__

```csharp
static void Main() {
    const float pi = 3.1415927f;
    const int r = 25;
    Console.WriteLine(pi * r * r);
}
```


__Příkaz výrazu__

```csharp
static void Main() {
    int i;
    i = 123;                // Expression statement
    Console.WriteLine(i);   // Expression statement
    i++;                    // Expression statement
    Console.WriteLine(i);   // Expression statement
}
```

__`if`vydá__

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


__`switch`vydá__

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

__`while`vydá__

```csharp
static void Main(string[] args) {
    int i = 0;
    while (i < args.Length) {
        Console.WriteLine(args[i]);
        i++;
    }
}
```


__`do`vydá__

```csharp
static void Main() {
    string s;
    do {
        s = Console.ReadLine();
        if (s != null) Console.WriteLine(s);
    } while (s != null);
}
```

__`for`vydá__

```csharp
static void Main(string[] args) {
    for (int i = 0; i < args.Length; i++) {
        Console.WriteLine(args[i]);
    }
}
```

__`foreach`vydá__

```csharp
static void Main(string[] args) {
    foreach (string s in args) {
        Console.WriteLine(s);
    }
}
```

__`break`vydá__

```csharp
static void Main() {
    while (true) {
        string s = Console.ReadLine();
        if (s == null) break;
        Console.WriteLine(s);
    }
}
```

__`continue`vydá__

```csharp
static void Main(string[] args) {
    for (int i = 0; i < args.Length; i++) {
        if (args[i].StartsWith("/")) continue;
        Console.WriteLine(args[i]);
    }
}
```

__`goto`vydá__

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

__`return`vydá__

```csharp
static int Add(int a, int b) {
    return a + b;
}

static void Main() {
    Console.WriteLine(Add(1, 2));
    return;
}
```

__`yield`vydá__

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

__`throw`příkazy `try` a__

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

__`checked`příkazy `unchecked` a__

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

__`lock`vydá__

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

__`using`vydá__

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

***Třídy*** jsou základem C#typů. Třída je datová struktura, která kombinuje stav (pole) a akce (metody a další členy funkce) v jedné jednotce. Třída poskytuje definici pro dynamicky vytvořené ***instance*** třídy, označované také jako ***objekty***. Třídy podporují ***Dědičnost*** a ***polymorfismus***, mechanismy, kterými mohou ***odvozené třídy*** roztáhnout a specializovat ***základní třídy***.

Nové třídy jsou vytvářeny pomocí deklarací třídy. Deklarace třídy začíná hlavičkou, která určuje atributy a modifikátory třídy, název třídy, základní třídu (Pokud je daná) a rozhraní implementovaná třídou. Pod hlavičkou následuje tělo třídy, které se skládá ze seznamu deklarací členů napsaných mezi oddělovači `{` a. `}`

Následuje deklarace jednoduché třídy s názvem `Point`:

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
Instance tříd jsou vytvořeny pomocí `new` operátoru, který přiděluje paměť pro novou instanci, vyvolá konstruktor pro inicializaci instance a vrátí odkaz na instanci. Následující příkazy vytvoří dva `Point` objekty a ukládají odkazy na tyto objekty ve dvou proměnných:

```csharp
Point p1 = new Point(0, 0);
Point p2 = new Point(10, 20);
```
Paměť obsazená objektem je automaticky uvolněna, pokud se objekt již nepoužívá. Není ani možné explicitně zrušit přidělení objektů v C#.

### <a name="members"></a>Members

Členy třídy jsou buď ***statické členy*** , nebo ***členy instance***. Statické členy patří ke třídám a členy instance patří do objektů (instance tříd).

Následující tabulka poskytuje přehled druhů členů, které třída může obsahovat.


| __Člen__   | __Popis__ |
|------------  |-----------------|
| Konstanty    | Konstantní hodnoty přidružené ke třídě |
| Fields (Pole)       | Proměnné třídy |
| Metody      | Výpočty a akce, které mohou být provedeny třídou |
| properties   | Akce spojené s čtením a zápisem s názvem vlastnosti třídy |
| Indexery     | Akce přidružené k indexování instancí třídy, jako je pole |
| Duration       | Oznámení, která mohou být vygenerována třídou |
| Operátory    | Převody a operátory výrazů podporované třídou |
| Konstruktory | Akce vyžadované pro inicializaci instancí třídy nebo samotné třídy |
| Destruktory  | Akce, které se mají provést před tím, než se instance třídy trvale zahodí |
| Typy        | Vnořené typy deklarované třídou |

### <a name="accessibility"></a>Přístupnost

Každý člen třídy má přidruženou přístupnost, která řídí oblasti textu programu, které mají přístup k členu. Existuje pět možných forem usnadnění přístupu. Tyto jsou shrnuté v následující tabulce.


| __Usnadnění__    | __Smyslu__ |
|----------------------|-----------------|
| `public`             | Přístup není omezený |
| `protected`          | Přístup omezený na tuto třídu nebo třídy odvozené z této třídy |
| `internal`           | Přístup omezený na tento program |
| `protected internal` | Přístup omezený na tento program nebo třídy odvozené z této třídy |
| `private`            | Přístup omezený na tuto třídu |

### <a name="type-parameters"></a>Parametry typu

Definice třídy může určovat sadu parametrů typu za názvem třídy a lomenými závorkami ohraničující seznam názvů parametrů typu. Parametry typu lze použít v těle deklarací třídy k definování členů třídy. V následujícím příkladu parametry `Pair` typu jsou `TFirst` a `TSecond`:

```csharp
public class Pair<TFirst,TSecond>
{
    public TFirst First;
    public TSecond Second;
}
```
Typ třídy, která je deklarována pro přijetí parametrů typu, se nazývá typ obecné třídy. Typy struktury, rozhraní a delegátů můžou být také obecné.

Při použití obecné třídy je nutné zadat argumenty typu pro každý z parametrů typu:

```csharp
Pair<int,string> pair = new Pair<int,string> { First = 1, Second = "two" };
int i = pair.First;     // TFirst is int
string s = pair.Second; // TSecond is string
```
Obecný typ s poskytnutými argumenty typu, jako `Pair<int,string>` je například výše, se označuje jako konstruovaný typ.

### <a name="base-classes"></a>Základní třídy

Deklarace třídy může specifikovat základní třídu za názvem třídy a parametry typu s dvojtečkou a názvem základní třídy. Vynechání specifikace základní třídy je stejné jako odvození z typu `object`. V následujícím příkladu `Point3D` je základní třída třídy `Point` `Point` a základní třída `object`:

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
Třída dědí členy své základní třídy. Dědičnost znamená, že třída implicitně obsahuje všechny členy své základní třídy, s výjimkou instancí a statických konstruktorů a destruktorů základní třídy. Odvozená třída může přidat nové členy do těch, které dědí, ale nemůže odebrat definici zděděného člena. V předchozím příkladu `Point3D` `x` dědí pole a `y` z `Point`a každá `Point3D` instance obsahuje tři pole, `x`, `y`a `z`.

Implicitní převod existuje z typu třídy na libovolný z jeho základních typů třídy. Proto proměnná typu třídy může odkazovat na instanci této třídy nebo instanci jakékoli odvozené třídy. Například s ohledem na předchozí deklarace třídy může proměnná typu `Point` odkazovat buď na `Point` , nebo `Point3D`:

```csharp
Point a = new Point(10, 20);
Point b = new Point3D(10, 20, 30);
```

### <a name="fields"></a>Fields (Pole)

Pole je proměnná, která je přidružena ke třídě nebo s instancí třídy.

Pole deklarované s `static` modifikátorem definuje ***statické pole***. Statické pole identifikuje právě jedno umístění úložiště. Bez ohledu na to, kolik instancí třídy je vytvořeno, existuje pouze jedna kopie statického pole.

Pole deklarované bez `static` modifikátoru definuje ***pole instance***. Každá instance třídy obsahuje samostatnou kopii všech polí instance této třídy.

V následujícím příkladu má `Color` každá instance třídy samostatnou kopii `r`polí `Black`instance, `g`a `b` `Red`, ale je k dispozici pouze jedna kopie, `White`,, `Green` a`Blue` statická pole:

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
Jak je znázorněno v předchozím příkladu, ***pole jen pro čtení*** mohou být deklarována `readonly` s modifikátorem. Přiřazení k `readonly` poli může být provedeno pouze v rámci deklarace pole nebo v konstruktoru ve stejné třídě.

### <a name="methods"></a>Metody

***Metoda*** je člen, který implementuje výpočet nebo akci, kterou lze provést pomocí objektu nebo třídy. ***Statické metody*** jsou k dispozici prostřednictvím třídy. ***Metody instance*** jsou k dispozici prostřednictvím instancí třídy.

Metody mají (pravděpodobně prázdný) seznam ***parametrů***, které reprezentují hodnoty nebo odkazy na proměnné předané metodě a ***návratový typ***, který určuje typ počítané hodnoty a vrácený metodou. Návratový typ metody je `void` , pokud nevrací hodnotu.

Podobně jako typy mohou metody mít také sadu parametrů typu, pro které argumenty typu musí být zadány při volání metody. Na rozdíl od typů lze argumenty typu často odvodit z argumentů volání metody a nemusí být explicitně předány.

***Signatura*** metody musí být jedinečná ve třídě, ve které je metoda deklarovaná. Signatura metody se skládá z názvu metody, počtu parametrů typu a počtu, modifikátorů a typů jeho parametrů. Podpis metody nezahrnuje návratový typ.

#### <a name="parameters"></a>Parametry

Parametry slouží k předání hodnot nebo odkazů na proměnné metody. Parametry metody získají jejich skutečné hodnoty z ***argumentů*** , které jsou zadány při volání metody. Existují čtyři typy parametrů: parametry hodnoty, parametry odkazu, výstupní parametry a pole parametrů.

***Parametr hodnoty*** se používá pro předávání vstupních parametrů. Parametr hodnoty odpovídá místní proměnné, která vrací počáteční hodnotu z argumentu předaného pro parametr. Úpravy parametru hodnoty neovlivňují argument, který byl předán parametru.

Parametry hodnoty mohou být volitelné, zadáním výchozí hodnoty, aby bylo možné vynechat odpovídající argumenty.

***Parametr reference*** se používá pro předávání vstupních i výstupních parametrů. Argument předaný parametru reference musí být proměnná a během provádění metody představuje parametr reference stejné umístění úložiště jako proměnná argumentu. Parametr reference je deklarován s `ref` modifikátorem. Následující příklad ukazuje použití `ref` parametrů.

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
***Výstupní parametr*** se používá pro předávání výstupního parametru. Výstupní parametr je podobný referenčnímu parametru s tím rozdílem, že počáteční hodnota argumentu zadaného volajícího je neimportovaná. Výstupní parametr je deklarován s `out` modifikátorem. Následující příklad ukazuje použití `out` parametrů.

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
***Pole parametrů*** povoluje proměnný počet argumentů, které mají být předány metodě. Pole parametrů je deklarováno s `params` modifikátorem. Pouze poslední parametr metody může být pole parametrů a typ pole parametrů musí být jednorozměrné pole. Metody `Write` a`WriteLine` třídy jsou osvědčenými příklady použití pole parametrů. `System.Console` Jsou deklarovány následujícím způsobem.

```csharp
public class Console
{
    public static void Write(string fmt, params object[] args) {...}
    public static void WriteLine(string fmt, params object[] args) {...}
    ...
}
```
V rámci metody, která používá pole parametrů, se pole parametru chová stejně jako regulární parametr typu pole. Při vyvolání metody s parametrem pole je však možné předat buď jeden argument typu pole parametru, nebo libovolný počet argumentů typu prvku pole parametrů. V druhém případě je instance pole automaticky vytvořena a inicializována s danými argumenty. Tento příklad

```csharp
Console.WriteLine("x={0} y={1} z={2}", x, y, z);
```
je ekvivalentem zápisu následujícího.

```csharp
string s = "x={0} y={1} z={2}";
object[] args = new object[3];
args[0] = x;
args[1] = y;
args[2] = z;
Console.WriteLine(s, args);
```

#### <a name="method-body-and-local-variables"></a>Tělo metody a místní proměnné

Tělo metody Určuje příkazy, které mají být provedeny při volání metody.

Tělo metody může deklarovat proměnné, které jsou specifické pro vyvolání metody. Tyto proměnné se nazývají ***místní proměnné***. Místní deklarace proměnné Určuje název typu, název proměnné a pravděpodobně počáteční hodnotu. Následující příklad deklaruje místní proměnnou `i` s počáteční hodnotou nula a místní proměnnou `j` bez počáteční hodnoty.

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
C#aby bylo možné získat hodnotu, musí být místní proměnná ***jednoznačně přiřazena*** . Například pokud deklarace předchozí `i` nezahrnuje počáteční hodnotu, kompilátor by nahlásil chybu pro následné použití `i` , protože `i` by se v těchto bodech v programu nedala jednoznačně přiřadit.

Metoda může použít `return` příkazy pro vrácení řízení volajícímu. V metodách, `void`které `return` vracejí, příkazy nemohou určovat výraz. V metodě, která vrací non`void`- `return` , příkazy musí obsahovat výraz, který vypočítá vrácenou hodnotu.

#### <a name="static-and-instance-methods"></a>Statické a instanční metody

Metoda deklarovaná s `static` modifikátorem je ***statická metoda***. Statická metoda nepracuje na konkrétní instanci a může přímo přistupovat ke statickým členům.

Metoda deklarovaná bez `static` modifikátoru je ***Metoda instance***. Metoda instance pracuje na konkrétní instanci a může přistupovat ke statickým i instancím členů. Instance, na které byla vyvolána metoda instance, může být explicitně k dispozici jako `this`. V případě, že se odkazuje na `this` statickou metodu, se jedná o chybu.

Následující `Entity` třída má členy statických i instancí.

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
Každá `Entity` instance obsahuje sériové číslo (a předpokládá se, že některé další informace nejsou zde uvedeny). `Entity` Konstruktor (který je jako metoda instance) Inicializuje novou instanci s dalším dostupným sériovým číslem. Vzhledem k tomu, že je konstruktor členem instance, je povolen přístup `serialNo` k poli instance i k `nextSerialNo` statickému poli.

Statické metody `SetNextSerialNo`amůžou přistupovat ke `serialNo` statickému poli, ale při přímém přístupu k poli instance by to byla chyba. `nextSerialNo` `GetNextSerialNo`

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
Všimněte si, `SetNextSerialNo` že `GetNextSerialNo` statické metody a jsou vyvolány `GetSerialNo` ve třídě, zatímco metoda instance je vyvolána na instancích třídy.

#### <a name="virtual-override-and-abstract-methods"></a>Virtuální, přepisování a abstraktní metody

Pokud deklarace metody instance obsahuje `virtual` modifikátor, metoda je označována jako ***virtuální metoda***. Pokud není `virtual` k dispozici žádný modifikátor, metoda je označována jako ***nevirtuální metoda***.

Když je vyvolána virtuální metoda, je ***typ běhu*** instance, pro kterou probíhá vyvolání, určuje vlastní implementaci metody, která má být vyvolána. V nevirtuálním volání metody je ***Typ doby kompilace*** instance určujícím faktorem.

Virtuální metoda může být ***přepsána*** v odvozené třídě. Když deklarace metody instance obsahuje `override` modifikátor, metoda přepíše zděděnou virtuální metodu se stejnou signaturou. Zatímco deklarace virtuální metody zavádí novou metodu, deklarace metody přepsání specializuje existující zděděnou virtuální metodu tím, že poskytuje novou implementaci této metody.

***Abstraktní*** metoda je virtuální metoda bez implementace. Abstraktní metoda je deklarována s `abstract` modifikátorem a je povolena pouze ve třídě, která je deklarována `abstract`také. Abstraktní metoda musí být přepsána v každé neabstraktní odvozené třídě.

Následující příklad deklaruje abstraktní `Expression`třídu,, která představuje uzel stromu výrazu, a tři odvozené třídy, `Constant`, `VariableReference`, a `Operation`, které implementují uzly stromu výrazů pro konstanty, proměnnou odkazy a aritmetické operace. (To je podobné, ale nelze je zaměňovat s typy stromu výrazů představenými ve [stromových typech výrazů](types.md#expression-tree-types)).

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
K modelování aritmetických výrazů lze použít předchozí čtyři třídy. Například pomocí instancí těchto tříd může být výraz `x + 3` reprezentován následujícím způsobem.

```csharp
Expression e = new Operation(
    new VariableReference("x"),
    '+',
    new Constant(3));
```
Metoda instance je vyvolána pro vyhodnocení `double` daného výrazu a vytvoření hodnoty. `Expression` `Evaluate` Metoda přebírá jako argument a `Hashtable` , který obsahuje názvy proměnných (jako klíče záznamů) a hodnoty (jako hodnoty položek). `Evaluate` Metoda je virtuální abstraktní metoda, což znamená, že neabstraktní odvozené třídy musí přepsat, aby poskytovala skutečnou implementaci.

`Constant` Implementacejednoduševrátí`Evaluate` uloženou konstantu. Implementace `VariableReference`objektu vyhledá název proměnné v zatřiďovací tabulce a vrátí výslednou hodnotu. Implementace nejprve vyhodnotí levý a pravý operand (rekurzivním `Evaluate` voláním metod) a poté provede danou aritmetickou operaci. `Operation`

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

#### <a name="method-overloading"></a>Přetížení metody

***Přetížení*** metody umožňuje, aby více metod ve stejné třídě měl stejný název, pokud mají jedinečné podpisy. Při kompilování volání přetížené metody kompilátor používá ***řešení přetížení*** k určení konkrétní metody, která má být vyvolána. Řešení přetížení najde jednu metodu, která nejlépe odpovídá argumentům, nebo hlásí chybu, pokud nelze najít žádnou nejlepší shodu. Následující příklad ukazuje rozlišení přetížení v platnosti. Komentář pro každé vyvolání v `Main` metodě ukazuje, která metoda je skutečně vyvolána.

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
Jak je znázorněno v příkladu, konkrétní metodu lze vždy vybrat explicitním přetypováním argumentů na přesné typy parametrů a/nebo explicitním zadáním argumentů typu.

### <a name="other-function-members"></a>Další členové funkcí

Členy, které obsahují spustitelný kód, jsou souhrnně označovány jako ***Členové funkce*** třídy. Předchozí část popisuje metody, které jsou hlavním typem členů funkce. Tato část popisuje další druhy členů funkce, které C#podporuje: konstruktory, vlastnosti, indexery, události, operátory a destruktory.

Následující kód ukazuje obecnou třídu s názvem `List<T>`, která implementuje zvětšený seznam objektů. Třída obsahuje několik příkladů nejběžnějších druhů členů funkce.


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

C#podporuje jak instance, tak statické konstruktory. ***Konstruktor instance*** je člen, který implementuje akce vyžadované pro inicializaci instance třídy. ***Statický konstruktor*** je člen, který implementuje akce vyžadované k inicializaci samotné třídy při prvním načtení.

Konstruktor je deklarovaný jako metoda bez návratového typu a stejný název jako obsahující třída. Pokud deklarace konstruktoru obsahuje `static` modifikátor, deklaruje statický konstruktor. V opačném případě deklaruje konstruktor instance.

Konstruktory instancí mohou být přetíženy. Například `List<T>` třída deklaruje dva konstruktory instancí, jednu bez parametrů a jednu, která `int` přijímá parametr. Konstruktory instancí jsou vyvolány `new` pomocí operátoru. Následující příkazy přidělují dvě `List<string>` instance pomocí každého konstruktoru `List` třídy.

```csharp
List<string> list1 = new List<string>();
List<string> list2 = new List<string>(10);
```
Na rozdíl od jiných členů nejsou konstruktory instancí zděděny a třída nemá žádné konstruktory instancí jiné než ty, které jsou ve třídě skutečně deklarovány. Pokud pro třídu není zadán konstruktor instance, bude automaticky zadáno prázdné číslo bez parametrů.

#### <a name="properties"></a>properties

***Vlastnosti*** jsou přirozené rozšíření polí. Oba se nazývají členové s přidruženými typy a syntaxe pro přístup k polím a vlastnostem je stejná. Na rozdíl od polí ale vlastnosti neoznačují umístění úložiště. Místo toho mají vlastnosti ***přistupující objekty*** , které určují příkazy, které mají být provedeny, když jsou jejich hodnoty čteny nebo zapsány.

Vlastnost je deklarována jako pole s tím rozdílem, že deklarace končí pomocí `get` přístupového objektu nebo `set` přístupového objektu napsaného `{` mezi oddělovači `}` a místo ukončení středníkem. `get` Vlastnost, která má přistupující objekt `set` i přistupující objekt `get` , je ***vlastnost pro čtení i zápis***, vlastnost, která má pouze přistupující objekt, je ***vlastnost jen pro čtení***a vlastnost, která má pouze `set` přistupující objekt, je. ***vlastnost pouze pro zápis***.

`get` Přistupující objekt odpovídá metodě bez parametrů s návratovou hodnotou typu vlastnosti. S výjimkou cíle přiřazení, při odkazování na vlastnost ve výrazu, `get` je vyvolána přístup k vlastnosti pro výpočet hodnoty vlastnosti.

Přistupující objekt odpovídá metodě s jediným parametrem s názvem `value` a bez návratového typu. `set` Když je na vlastnost odkazováno jako na cíl přiřazení nebo jako operand `++` nebo `--`, `set` je objekt přistupující vyvolán s argumentem, který poskytuje novou hodnotu.

Třída deklaruje dvě vlastnosti `Count` `Capacity`, které jsou jen pro čtení a pro čtení i zápis, v uvedeném pořadí. `List<T>` Následuje příklad použití těchto vlastností.

```csharp
List<string> names = new List<string>();
names.Capacity = 100;            // Invokes set accessor
int i = names.Count;             // Invokes get accessor
int j = names.Capacity;          // Invokes get accessor
```
Podobně jako pole a metody C# podporuje vlastnosti instance i statické vlastnosti. Statické vlastnosti jsou deklarovány s `static` modifikátorem a vlastnosti instance jsou deklarovány bez ní.

Přístupové objekty vlastnosti mohou být virtuální. Pokud deklarace vlastnosti obsahuje `virtual`modifikátor, `abstract`nebo `override` , vztahuje se na přístupový objekt (y) vlastnosti.

#### <a name="indexers"></a>Indexery

***Indexer*** je člen, který umožňuje, aby objekty byly indexovány stejným způsobem jako pole. Indexer je deklarován jako vlastnost s tím rozdílem, že název člena je `this` následován seznamem parametrů napsaným mezi `[` oddělovači a `]`. Parametry jsou k dispozici v přistupujícím objektu indexeru. Podobně jako u vlastností mohou být indexery pro čtení i zápis, jen pro čtení a přístup k nástroji indexeru, které mohou být virtuální.

Třída deklaruje jeden indexer pro čtení a zápis, který `int` přebírá parametr. `List` Indexer umožňuje indexovat `List` instance s `int` hodnotami. Například

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
Indexery mohou být přetíženy, což znamená, že třída může deklarovat více indexerů za předpokladu, že počet nebo typy jejich parametrů se liší.

#### <a name="events"></a>Duration

***Událost*** je člen, který umožňuje třídě nebo objektu poskytovat oznámení. Událost je deklarována jako pole s tím rozdílem, že deklarace `event` zahrnuje klíčové slovo a typ musí být delegovaný typ.

V rámci třídy, která deklaruje člena události, se událost chová stejně jako pole typu delegáta (za předpokladu, že událost není abstraktní a nedeklaruje přistupující objekty). Pole ukládá odkaz na delegáta, který představuje obslužné rutiny událostí, které byly přidány do události. Pokud nejsou k dispozici žádné obslužné rutiny událostí `null`, pole je.

Třída deklaruje jeden člen události s názvem `Changed`, který indikuje, že do seznamu byla přidána nová položka. `List<T>` Událost je vyvolána `OnChanged` virtuální metodou, která nejprve kontroluje, zda je `null` událost (to znamená, že nejsou přítomny žádné obslužné rutiny). `Changed` Pojem vyvolání události je přesně shodný s voláním delegáta reprezentovaného událostí, takže neexistují žádné speciální jazykové konstrukce pro vyvolání událostí.

Klienti reagují na události prostřednictvím ***obslužných rutin událostí***. Obslužné rutiny událostí jsou připojeny `+=` pomocí operátoru a odebrány `-=` pomocí operátoru. Následující příklad připojí obslužnou rutinu události k `Changed` události. `List<string>`

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
Pro pokročilé scénáře, kde je požadováno řízení základního úložiště události, může deklarace události explicitně poskytnout `add` a `remove` přistupující objekty, které `set` jsou poněkud podobné přístupovému objektu vlastnosti.

#### <a name="operators"></a>Operátory

***Operátor*** je člen, který definuje význam použití konkrétního operátoru výrazu na instance třídy. Lze definovat tři typy operátorů: unární operátory, binární operátory a operátory převodu. Všechny operátory musí být deklarovány `public` jako `static`a.

Třída deklaruje dva operátory `operator==` `List` , a, a poskytuje tak nový význam pro výrazy, které tyto operátory aplikují na instance. `operator!=` `List<T>` Konkrétně operátory definují rovnost dvou `List<T>` instancí jako porovnávání každého z obsažených objektů pomocí jejich `Equals` metod. Následující příklad používá `==` operátor k porovnání dvou `List<int>` instancí.

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

První `Console.WriteLine` výstupy `True` , protože dva seznamy obsahují stejný počet objektů se stejnými hodnotami ve stejném pořadí. `a` `List<int>` Nebyl definován `operator==`, `False` první `Console.WriteLine` by měl výstup, protože a`b` odkazují na různé instance. `List<T>`

#### <a name="destructors"></a>Destruktory

***Destruktor*** je člen, který implementuje akce vyžadované k destrukcií instance třídy. Destruktory nemohou mít parametry, nemohou mít modifikátory dostupnosti a nelze je volat explicitně. Destruktor instance je vyvolán automaticky během uvolňování paměti.

Uvolňování paměti je povolená rozsáhlá Zeměpisná šířka při rozhodování o shromažďování objektů a spouštění destruktorů. Konkrétně načasování volání destruktoru není deterministické a v jakémkoli vlákně mohou být provedeny destruktory. Z těchto a dalších důvodů by třídy měly implementovat destruktory pouze v případě, že žádná jiná řešení nejsou proveditelná.

`using` Příkaz poskytuje lepší přístup k zničení objektu.

## <a name="structs"></a>Struktury

Podobně jako třídy ***struktury*** jsou datové struktury, které mohou obsahovat datové členy a členy funkce, ale na rozdíl od tříd, struktury jsou typy hodnot a nevyžadují přidělení haldy. Proměnná typu struktury přímo ukládá data struktury, zatímco proměnná typu třídy ukládá odkaz na dynamicky přidělený objekt. Typy struktury nepodporují uživatelem zadanou dědičnost a všechny typy struktury implicitně dědí z typu `object`.

Struktury jsou zvláště užitečné pro malé datové struktury, které mají sémantiku hodnot. Komplexní čísla, body v systému souřadnic nebo páry klíč-hodnota ve slovníku jsou všechny dobrými příklady struktur. Použití struktur namísto tříd pro malé datové struktury může mít velký rozdíl v počtu přidělení paměti, které aplikace provádí. Například následující program vytvoří a inicializuje pole 100 bodů. V `Point` případě 101, že jsou implementovány jako třídy, jsou vytvořeny instance samostatných objektů – jeden pro pole a jeden pro prvky 100.

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
Alternativou je vytvoření `Point` struktury.

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
Nyní je vytvořena instance pouze jednoho objektu – jeden pro pole – a `Point` instance jsou uloženy v poli do pole.

Konstruktory struktury jsou vyvolány `new` pomocí operátoru, ale to neznamená, že se paměť přiděluje. Namísto dynamického přidělování objektu a vrácení odkazu na něj konstruktor struktury jednoduše vrátí hodnotu samotné struktury (obvykle v dočasném umístění v zásobníku) a tato hodnota se pak podle potřeby zkopíruje.

U tříd je možné, aby dvě proměnné odkazovaly na stejný objekt a bylo tak možné, aby operace na jedné proměnné ovlivnily objekt, na který je odkazováno jinou proměnnou. Pomocí struktur mají proměnné, které mají svou vlastní kopii dat, a není možné, aby operace na jednom z nich ovlivnily druhou. Například výstup vyprodukovaný následujícím fragmentem kódu závisí na tom, zda `Point` je třída nebo struktura.

```csharp
Point a = new Point(10, 10);
Point b = a;
a.x = 20;
Console.WriteLine(b.x);
```
Pokud `Point` je třída, výstup je `20` , protože `a` a `b` odkazuje na stejný objekt. Pokud `Point` je struktura, výstup je `10` , protože přiřazení `a` pro `b` vytvoří kopii hodnoty a tato kopie není ovlivněna následným přiřazením k `a.x`.

Předchozí příklad zvýrazní dvě omezení struktury. Nejprve kopírování celé struktury je obvykle méně efektivní než kopírování odkazu na objekt, takže předávání parametrů hodnot a parametrů může být dražší s strukturami, než s typem odkazu. Sekunda s `ref` výjimkou `out` parametrů a není možné vytvořit odkazy na struktury, které pravidla jejich používání vymění v řadě situací.

## <a name="arrays"></a>Pole

***Pole*** je datová struktura, která obsahuje počet proměnných, které jsou dostupné prostřednictvím počítaných indexů. Proměnné obsažené v poli, označované také jako ***prvky*** pole, jsou všechny stejného typu a tento typ se nazývá ***typ elementu*** pole.

Typy polí jsou odkazové typy a deklarace proměnné pole jednoduše nastaví volné místo pro odkaz na instanci pole. Skutečné instance pole jsou vytvářeny dynamicky za běhu pomocí `new` operátoru. Operace určuje délku nové instance pole, která je poté opravena po dobu života instance. `new` Indexy prvků rozsahu pole od `0` do. `Length - 1` Operátor automaticky inicializuje prvky pole na výchozí hodnotu, což je například nula pro všechny číselné typy a `null` pro všechny typy odkazů. `new`

Následující příklad vytvoří pole `int` prvků, inicializuje pole a vytiskne obsah pole.

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
Tento příklad vytvoří a pracuje na jednorozměrném ***poli***. C#podporuje rovněž ***multidimenzionální pole***. Počet rozměrů typu pole, označovaný také jako ***rozměr*** typu pole, je jedna plus počet čárek napsaných mezi hranatými závorkami typu pole. Následující příklad přiděluje jednorozměrné, dvourozměrné a trojrozměrné pole.

```csharp
int[] a1 = new int[10];
int[,] a2 = new int[10, 5];
int[,,] a3 = new int[10, 5, 2];
```
Pole obsahuje 10 prvků `a2` , pole obsahuje 50 (10 × 5 `a3` ) prvků a pole obsahuje 100 (10 × 5 × 2) prvků. `a1`

Typ elementu pole může být libovolný typ, včetně typu pole. Pole s prvky typu pole se někdy nazývá ***vícenásobné pole*** , protože délky polí elementů nemusí být stejné. Následující příklad přiděluje pole polí pro `int`:

```csharp
int[][] a = new int[3][];
a[0] = new int[10];
a[1] = new int[5];
a[2] = new int[20];
```
První řádek vytvoří pole se třemi prvky, každý typ `int[]` a každý s počáteční `null`hodnotou. Následující řádky poté inicializují tři prvky s odkazy na jednotlivé instance pole s různou délkou.

Operátor umožňuje zadat počáteční hodnoty prvků pole pomocí ***inicializátoru pole***, což je seznam výrazů zapsaných mezi oddělovači `{` a. `}` `new` Následující příklad přiděluje a inicializuje `int[]` se třemi prvky.

```csharp
int[] a = new int[] {1, 2, 3};
```
Všimněte si, že délka pole je odvozena od počtu výrazů mezi `{` a. `}` Deklarace lokální proměnné a pole lze dále zkrátit tak, aby se typ pole nemuselo přestavit.

```csharp
int[] a = {1, 2, 3};
```
Oba předchozí příklady jsou ekvivalentní následujícímu:

```csharp
int[] t = new int[3];
t[0] = 1;
t[1] = 2;
t[2] = 3;
int[] a = t;
```
## <a name="interfaces"></a>Rozhraní

***Rozhraní*** definuje kontrakt, který může být implementován pomocí tříd a struktur. Rozhraní může obsahovat metody, vlastnosti, události a indexery. Rozhraní neposkytuje implementace členů, které definuje – určuje pouze členy, které musí být poskytnuty třídami nebo strukturami, které implementují rozhraní.

Rozhraní mohou využívat ***vícenásobnou dědičnost***. V následujícím příkladu rozhraní `IComboBox` dědí `ITextBox` z a `IListBox`.

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
Třídy a struktury mohou implementovat více rozhraní. V následujícím příkladu třída `EditBox` implementuje `IDataBound`i `IControl` .

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
Pokud třída nebo struktura implementuje konkrétní rozhraní, instance této třídy nebo struktury lze implicitně převést na tento typ rozhraní. Například

```csharp
EditBox editBox = new EditBox();
IControl control = editBox;
IDataBound dataBound = editBox;
```
V případech, kdy instance není staticky známá pro implementaci konkrétního rozhraní, lze použít dynamické přetypování typu. Například následující příkazy používají přetypování dynamického typu k získání implementace objektu `IControl` a `IDataBound` rozhraní. Vzhledem k tomu, že skutečný typ objektu `EditBox`je, přetypování je úspěšné.

```csharp
object obj = new EditBox();
IControl control = (IControl)obj;
IDataBound dataBound = (IDataBound)obj;
```
V `EditBox` předchozí `public` třídě `Paint` je metoda `IControl` `IDataBound` z rozhraní a Metodazrozhraníimplementovánapomocíčlenů.`Bind` C#podporuje také ***explicitní implementace členů rozhraní***, pomocí kterých se třída nebo struktura může vyhnout vytváření členů `public`. Explicitní implementace člena rozhraní je zapsána pomocí plně kvalifikovaného názvu člena rozhraní. Například `EditBox` třída může `IControl.Paint` implementovat metody a `IDataBound.Bind` pomocí explicitní implementace členů rozhraní následujícím způsobem.

```csharp
public class EditBox: IControl, IDataBound
{
    void IControl.Paint() {...}
    void IDataBound.Bind(Binder b) {...}
}
```
K explicitním členům rozhraní lze přistupovat pouze prostřednictvím typu rozhraní. Například implementaci `IControl.Paint` poskytovanou předchozí `EditBox` třídou lze `EditBox` vyvolat pouze první `IControl` převod odkazu na typ rozhraní.

```csharp
EditBox editBox = new EditBox();
editBox.Paint();                        // Error, no such method
IControl control = editBox;
control.Paint();                        // Ok
```

## <a name="enums"></a>Výčty

***Typ výčtu*** je jedinečný typ hodnoty se sadou pojmenovaných konstant. Následující příklad deklaruje a používá `Color` typ výčtu pojmenovaný se třemi konstantními hodnotami, `Red`, `Green`a `Blue`.

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
Každý typ výčtu má odpovídající celočíselný typ nazvaný ***základní typ*** výčtového typu. Typ výčtu, který explicitně nedeklaruje nadřízený typ, má podkladový typ `int`. Formát úložiště typu výčtu a rozsah možných hodnot jsou určeny podle jeho základního typu. Sada hodnot, na jejichž základě může typ výčtu pobírat, není omezena členy výčtu. Konkrétně jakákoli hodnota základního typu výčtu může být převedena na typ výčtu a je odlišná platná hodnota tohoto typu výčtu.

Následující příklad deklaruje typ výčtu s názvem `Alignment` s podkladovým `sbyte`typem.

```csharp
enum Alignment: sbyte
{
    Left = -1,
    Center = 0,
    Right = 1
}
```
Jak je znázorněno v předchozím příkladu, deklarace člena výčtu může zahrnovat konstantní výraz, který určuje hodnotu člena. Hodnota konstanty pro každého člena výčtu musí být v rozsahu nadřazeného typu výčtu. Když deklarace člena výčtu explicitně neurčí hodnotu, členovi se předává hodnota nula (Pokud se jedná o první člen v typu výčtu) nebo hodnota dříve předcházejícího člena výčtu plus jedna.

Hodnoty výčtu lze převést na integrální hodnoty a naopak pomocí přetypování typů. Například

```csharp
int i = (int)Color.Blue;        // int i = 2;
Color c = (Color)2;             // Color c = Color.Blue;
```
Výchozí hodnota libovolného typu výčtu je celočíselná hodnota nula převedená na typ výčtu. V případech, kdy jsou proměnné automaticky inicializovány na výchozí hodnotu, je tato hodnota předána proměnným typům výčtu. Aby byla výchozí hodnota typu výčtu snadno dostupná, literál `0` implicitně převede na libovolný typ výčtu. Proto jsou povoleny následující.

```csharp
Color c = 0;
```

## <a name="delegates"></a>Delegáty

***Typ delegáta*** představuje odkazy na metody s konkrétním seznamem parametrů a návratovým typem. Delegáti umožňují zacházet s metodami jako s entitami, které lze přiřadit proměnným a předávat jako parametry. Delegáti jsou podobní pojmu ukazatelů na funkce nalezené v některých jiných jazycích, ale na rozdíl od ukazatelů na funkce jsou delegáti objektově orientované a typově bezpečné.

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
Instance `Function` typu delegáta může odkazovat na jakoukoli metodu, která `double` přebírá argument a vrací `double` hodnotu. Metoda použije daný `Function` prvek `double[]` pro prvky a vrátí výsledek. `double[]` `Apply` `Main` V `double[]`metodě sepoužívákpoužitítřírůznýchfunkcína.`Apply`

Delegát může odkazovat buď na statickou metodu (například `Square` nebo `Math.Sin` v předchozím příkladu), nebo na metodu `m.Multiply` instance (například v předchozím příkladu). Delegát, který odkazuje na metodu instance, také odkazuje na konkrétní objekt a když je metoda instance vyvolána prostřednictvím delegáta, bude `this` tento objekt ve volání.

Delegáty je také možné vytvořit pomocí anonymních funkcí, což jsou "vložené metody", které jsou vytvářeny průběžně. Anonymní funkce mohou zobrazit místní proměnné okolních metod. Proto je příklad násobitele výše možné napsat snadněji bez použití `Multiplier` třídy:

```csharp
double[] doubles =  Apply(a, (double x) => x * 2.0);
```
Zajímavou a užitečnou vlastností delegáta je, že neznají ani nezáleží na třídě metody, na kterou odkazuje; všechny tyto věci jsou, že odkazovaná metoda má stejné parametry a návratový typ jako delegát.

## <a name="attributes"></a>Atributy

Typy, členy a další entity v C# programu podporují modifikátory, které řídí určité aspekty jejich chování. Například přístupnost metody je `public`řízena pomocí modifikátorů, `protected`, `internal`a `private` . C#generalizuje tuto funkci tak, aby uživatelsky definované typy deklarativních informací mohly být připojeny k programovým entitám a načteny v době běhu. Programy určují tyto dodatečné deklarativní informace definováním a použitím ***atributů***.

Následující příklad deklaruje `HelpAttribute` atribut, který lze umístit do entit programu, aby poskytoval odkazy na jeho přidruženou dokumentaci.

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
Všechny třídy atributů jsou odvozeny `System.Attribute` ze základní třídy poskytnuté .NET Framework. Atributy lze použít zadáním jejich názvu spolu s případnými argumenty uvnitř hranatých závorek těsně před přidruženou deklarací. Pokud název atributu končí `Attribute`, Tato část názvu může být vynechána při odkazování na atribut. `HelpAttribute` Atribut lze například použít následujícím způsobem.

```csharp
[Help("http://msdn.microsoft.com/.../MyClass.htm")]
public class Widget
{
    [Help("http://msdn.microsoft.com/.../MyClass.htm", Topic = "Display")]
    public void Display(string text) {}
}
```
Tento `HelpAttribute` příklad připojí `Widget` ke tříděa`HelpAttribute` druhé k metoděvetřídě.`Display` Veřejné konstruktory třídy atributu určují informace, které je nutné zadat při připojení atributu k entitě programu. Další informace lze poskytnout odkazem na veřejné vlastnosti pro čtení a zápis třídy atributu (například odkaz na `Topic` vlastnost dříve).

Následující příklad ukazuje, jak lze načíst informace o atributu pro danou entitu programu v době běhu pomocí reflexe.

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
Pokud je požadován konkrétní atribut prostřednictvím reflexe, konstruktor třídy atributu je vyvolán s informacemi poskytnutými ve zdroji programu a výslednou instancí atributu je vrácen. Pokud byly k dispozici další informace prostřednictvím vlastností, jsou tyto vlastnosti nastaveny na zadané hodnoty před vrácením instance atributu.
