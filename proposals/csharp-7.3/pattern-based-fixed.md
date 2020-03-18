---
ms.openlocfilehash: 2070cf3b3269585055791adc3427cbd134df444d
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484655"
---
# <a name="pattern-based-fixed-statement"></a>Příkaz `fixed` na základě vzoru

## <a name="summary"></a>Souhrn
[summary]: #summary

Zaveďte vzor, který umožní, aby se typy účastnily `fixed` příkazy. 

## <a name="motivation"></a>Motivační
[motivation]: #motivation

Jazyk poskytuje mechanismus pro připnutí spravovaných dat a získání nativního ukazatele na podkladovou vyrovnávací paměť.

```csharp
fixed(byte* ptr = byteArray)
{
   // ptr is a native pointer to the first element of the array
   // byteArray is protected from being moved/collected by the GC for the duration of this block 
}

```

Sada typů, které se mohou účastnit `fixed`, je pevně zakódované a omezena na pole a `System.String`. Zakódujeme "Special" typy se neškálují, když jsou zavedeny nové primitivní prvky jako `ImmutableArray<T>`, `Span<T>``Utf8String`. 

Kromě toho aktuální řešení pro `System.String` spoléhá na poměrně tuhé rozhraní API. Tvar rozhraní API znamená, že `System.String` je souvislý objekt, který vkládá UTF16 kódovaná data s pevným posunem z hlavičky objektu. Takový přístup se našel problematický v několika návrzích, které by mohly vyžadovat změny v podkladovém rozložení. Je žádoucí, aby bylo možné přepnout na flexibilnější flexibilitu, která odděluje `System.String` objekt od jeho interní reprezentace za účelem nespravovaného interoperability. 

## <a name="detailed-design"></a>Podrobný návrh
[design]: #detailed-design

## <a name="pattern"></a>*Vzor* ##
Životaschopná "pevná", která je založená na vzorovém, musí:
-   Poskytněte spravované odkazy pro připnutí instance a inicializaci ukazatele (Pokud je to ten, co je stejný odkaz).
-   Vyjádřit jednoznačně typ nespravovaného elementu (tj. "char" pro "String")
-   Nařídí chování prázdného případu, pokud neexistuje žádný odkaz na. 
-   Neměl by vytvářet autory rozhraní API pro rozhodování o návrhu, které snížit použití typu mimo `fixed`.

Domnívám se, že výše uvedená by mohla být splněna rozpoznáváním speciálně pojmenovaného člena vracejícího odkaz: `ref [readonly] T GetPinnableReference()`.

Aby bylo možné použít příkaz `fixed`, musí být splněny následující podmínky:

1. Pro typ je k dispozici pouze jeden takový člen.
1. Vrátí `ref` nebo `ref readonly`. (`readonly` je povolená, aby autoři neměnných/ReadOnly typů mohli implementovat vzor bez přidání zapisovatelného rozhraní API, které by bylo možné použít v bezpečném kódu)
1. T je nespravovaný typ.
(vzhledem k tomu, že `T*` se změní na typ ukazatele. Omezení se přirozeně rozšíří, pokud je rozbalený pojem "nespravovaný".
1. Vrátí spravované `nullptr`, když není k dispozici žádná data k připnutí – pravděpodobně nejlevnější způsob, jak vyjádřit vyprázdnění.
(Všimněte si, že řetězec "" vrátí odkaz na \ 0, protože řetězce jsou zakončené hodnotou null).

Případně pro `#3` můžeme výsledek v prázdných případech nechat nedefinovaný nebo specifický pro implementaci. To však může zajistit, že rozhraní API bude bezpečnější a náchylné k zneužití a nezamýšlenému zatížení kompatibility. 

## <a name="translation"></a>*Překlad* ##

```csharp
fixed(byte* ptr = thing)
{ 
    // <BODY>
}
```

se bude jednat o následující pseudokódu (ne všechny C#vyhodnotit v)

```csharp
byte* ptr;
// specially decorated "pinned" IL local slot, not visible to user code.
pinned ref byte _pinned;

try
{
    // NOTE: null check is omitted for value types 
    // NOTE: `thing` is evaluated only once (temporary is introduced if necessary) 
    if (thing != null)
    {
        // obtain and "pin" the reference
        _pinned = ref thing.GetPinnableReference();

        // unsafe cast in IL
        ptr = (byte*)_pinned;
    }
    else
    {
        ptr = default(byte*);
    }

    // <BODY> 
}
finally   // finally can be omitted when not observable
{
    // "unpin" the object
    _pinned = nullptr;
}
```

## <a name="drawbacks"></a>Nevýhody
[drawbacks]: #drawbacks

- GetPinnableReference je určena pouze pro použití v `fixed`, ale nic nebrání jeho použití v bezpečném kódu, takže implementátor musí mít na paměti.

## <a name="alternatives"></a>Alternativy
[alternatives]: #alternatives

Uživatelé mohou zavést GetPinnableReference nebo podobného člena a použít ho jako
 
```csharp
fixed(byte* ptr = thing.GetPinnableReference())
{ 
    // <BODY>
}
```

Pro `System.String` neexistuje žádné řešení, pokud je potřeba alternativní řešení.

## <a name="unresolved-questions"></a>Nevyřešené dotazy
[unresolved]: #unresolved-questions

- [] Chování je ve stavu "Empty". - `nullptr` nebo `undefined`? 
- [] By se měly považovat metody rozšíření? 
- [] Pokud se na `System.String`zjistí vzor, měl by se získat? 

## <a name="design-meetings"></a>Schůzky pro návrh

Žádná ještě není. 
