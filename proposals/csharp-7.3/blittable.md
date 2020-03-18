---
ms.openlocfilehash: 11e9d21bda9e69be692c5c5f5aee80c2da1894ab
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484620"
---
# <a name="unmanaged-type-constraint"></a>Omezení nespravovaného typu

## <a name="summary"></a>Souhrn
[summary]: #summary

Funkce nespravovaného omezení poskytuje vynucení jazyka pro třídu typů, která je známá jako "nespravované typy C# " ve specifikaci jazyka.  To je definováno v sekci 18,2 jako typ, který není odkazový typ a neobsahuje pole referenčního typu na jakékoli úrovni vnořování.  

## <a name="motivation"></a>Motivační
[motivation]: #motivation

Primární motivace je usnadnit vytváření kódu spolupráce na nízké úrovni v C#. Nespravované typy jsou jedním ze základních stavebních bloků pro interoperabilitu kód, ale chybějící podpora v obecných typech znemožňuje vytváření opakovaně použitelných rutin napříč všemi nespravovanými typy. Místo toho mohou vývojáři vytvářet stejný kód kotlového talíře pro každý nespravovaný typ v knihovně:

```csharp
int Hash(Point point) { ... } 
int Hash(TimeSpan timeSpan) { ... } 
```

Pro povolení tohoto typu scénáře bude jazyk zavádět nové omezení: nespravované:

```csharp
void Hash<T>(T value) where T : unmanaged
{
    ...
}
```

Toto omezení může být splněno pouze typy, které se vejdou do definice nespravovaného typu C# ve specifikaci jazyka. Dalším způsobem, jak to zjistit, je, že typ splňuje nespravovaná omezení pokud může být použit také jako ukazatel. 

```csharp
Hash(new Point()); // Okay 
Hash(42); // Okay
Hash("hello") // Error: Type string does not satisfy the unmanaged constraint
```

Parametry typu s nespravovaným omezením můžou využívat všechny funkce, které jsou k dispozici pro nespravované typy: ukazatelé, pevné atd... 

```csharp
void Hash<T>(T value) where T : unmanaged
{
    // Okay
    fixed (T* p = &value) 
    { 
        ...
    }
}
```

Toto omezení také umožní efektivní převody mezi strukturovanými daty a proudy bajtů. Toto je operace, která je společná v síťových zásobníkech a vrstvách serializace:

```csharp
Span<byte> Convert<T>(ref T value) where T : unmanaged 
{
    ...
}
```

Tyto rutiny jsou výhodné, protože jsou v době kompilace prokazatelně bezpečné a přidělení je bezplatné.  Autoři spolupracují dnes nemůžou to udělat (i když se nachází ve vrstvě, kde je výkon kritický).  Místo toho musí spoléhat na přidělení rutin, které mají náročné kontroly za běhu, aby ověřily hodnoty správně nespravované.

## <a name="detailed-design"></a>Podrobný návrh
[design]: #detailed-design

Jazyk zavede nové omezení s názvem `unmanaged`. Aby bylo toto omezení splněno, musí být typem struktura a všechna pole typu musí být rozdělena do jedné z následujících kategorií:

- Mít typ `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, `decimal`, `bool`, `IntPtr` nebo `UIntPtr`.
- Být libovolný typ `enum`.
- Být typu ukazatel.
- Musí se jednat o uživatelsky definovanou strukturu, která satsifies omezení `unmanaged`.

Tato omezení musí splňovat i pole instance generovaná kompilátorem, jako jsou například tyto automaticky implementované vlastnosti. 

Příklad:

```csharp
// Unmanaged type
struct Point 
{ 
    int X;
    int Y {get; set;}
}

// Not an unmanaged type
struct Student 
{ 
    string FirstName;
    string LastName;
}
``` 

Omezení `unmanaged` nelze kombinovat s `struct`, `class` ani `new()`. Toto omezení je odvozeno od faktu, že `unmanaged` implikuje `struct` takže ostatní omezení nedělají smysl.

Omezení `unmanaged` není vynutilo modulem CLR, pouze podle jazyka. Aby nedocházelo k neoprávněnému použití v jiných jazycích, metody, které mají toto omezení, budou chráněny pomocí mod-REQ. To zabrání jiným jazykům v použití argumentů typu, které nejsou nespravovanými typy.

Token `unmanaged` v omezení není klíčové slovo, ani kontextové klíčové slovo. Místo toho je třeba `var` v tom, že se vyhodnotí v tomto umístění a bude mít jednu z těchto hodnot:

- Vytvoření vazby na uživatelem definovaný nebo odkazovaný typ s názvem `unmanaged`: Tato akce bude zpracována stejně jako jakékoli jiné omezení pojmenovaného typu. 
- Vazba bez typu: Tato akce bude interpretována jako omezení `unmanaged`.

V případě, že existuje typ s názvem `unmanaged` a je k dispozici bez kvalifikace v aktuálním kontextu, nebude možné použít omezení `unmanaged`. To paralelně pravidla obklopující `var` a uživatelsky definované typy se stejným názvem. 

## <a name="drawbacks"></a>Nevýhody
[drawbacks]: #drawbacks

Hlavní nevýhodou této funkce je, že obsluhuje malý počet vývojářů: typicky nízká úroveň autorů knihoven nebo architektur.  Proto je u malého počtu vývojářů útrata úžasného jazyka. 

Tyto architektury jsou ale často základem pro většinu aplikací .NET.  Proto výkon/správnost služby WINS na této úrovni může mít na ekosystém .NET vliv na Ripple.  Díky tomu se tato funkce zvažuje i u omezené cílové skupiny.

## <a name="alternatives"></a>Alternativy
[alternatives]: #alternatives

Je potřeba vzít v úvahu několik alternativ:

- Stav quo: funkce není odůvodněná na své vlastní vlastnosti a vývojáři dál používají chování implicitního výslovného souhlasu.

## <a name="questions"></a>Otázky
[quesions]: #questions

### <a name="metadata-representation"></a>Reprezentace metadat

F# Jazyk kóduje omezení v souboru signatury, což znamená C# , že nelze znovu použít jejich reprezentaci. Pro toto omezení bude nutné vybrat nový atribut. Kromě toho musí být metoda, která má toto omezení, chráněná pomocí mod-REQ.

### <a name="blittable-vs-unmanaged"></a>Nepřenositelný vs. nespravovaný
F# Jazyk má velmi [podobnou funkci](https://docs.microsoft.com/dotnet/articles/fsharp/language-reference/generics/constraints) , která používá nespravovaný klíčová slova. Název přenositeli pochází z použití v Midori.  Možná budete chtít, aby tady vypadala priorita a místo toho používala nespravované. 

**Řešení** Jazyk, který se rozhodne použít jako nespravovaný 

### <a name="verifier"></a>Verifier

Je nutné aktualizovat ověřovatel/modul runtime, aby bylo možné pochopit použití ukazatelů na Obecné parametry typu?  Nebo může fungovat jenom tak, jak je beze změn?

**Řešení** Nepotřebujete žádné změny. Všechny typy ukazatelů jsou jednoduše neověřené. 

## <a name="design-meetings"></a>Schůzky pro návrh

neuvedeno
