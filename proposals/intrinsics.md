---
ms.openlocfilehash: ac4c8760e3b6a0934f01ae634f666af60aa1c0fe
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484564"
---
# <a name="compiler-intrinsics"></a>Vnitřní funkce kompilátoru

## <a name="summary"></a>Souhrn

Tento návrh poskytuje jazykové konstrukce, které zveřejňují operační kódy IL nízké úrovně, které momentálně nelze efektivně přistupovat, nebo vůbec: `ldftn`, `ldvirtftn`, `ldtoken` a `calli`. Tyto operační kódy nízké úrovně můžou být důležité v kódu s vysokým výkonem a vývojáři potřebují účinný způsob, jak k nim přistupovat.

## <a name="motivation"></a>Motivační

Motivace a pozadí této funkce jsou popsány v následujícím problému (jako je potenciální implementace funkce): 

https://github.com/dotnet/csharplang/issues/191

Tento alternativní návrh návrhu se zobrazí po kontrole implementace prototypu původního návrhu @msjabby a také při použití v rámci významné základní znakové sady. Tento návrh byl proveden se značným vstupem z @mjsabby, @tmat a @jkotas.

## <a name="detailed-design"></a>Podrobný návrh 

### <a name="allow-address-of-to-target-methods"></a>Povolení adresy pro cílové metody

Skupiny metod budou nyní povoleny jako argumenty pro výraz adresy. Typ takového výrazu bude `void*`. 

``` csharp
class Util { 
    public static void Log() { } 
}

// ldftn Util.Log
void* ptr = &Util.Log; 
```

V tomto případě neexistuje žádný převod delegáta. jediným mechanismem pro filtrování členů ve skupině metod je statický přístup k instanci. Pokud to nemůže rozlišovat členy, dojde k chybě při kompilaci.

``` csharp
class Util { 
    public void Log() { } 
    public void Log(string p1) { } 
    public static void Log(int i) { };
}

unsafe {
    // Error: Method group Log has more than one applicable candidate.
    void* ptr1 = &Log; 

    // Okay: only one static member to consider here.
    void* ptr2 = &Util.Log;
}
```

Výraz AddressOf v tomto kontextu bude implementován následujícím způsobem:

- ldftn: Pokud je metoda nevirtuální.
- ldvirtftn: když je metoda virtuální.

Omezení této funkce:

- Metody instance se dají zadat jenom při použití výrazu vyvolání pro hodnotu.
- V `&`nelze použít místní funkce. Podrobnosti implementace těchto metod jsou záměrně Nespecifikovány jazykem. To zahrnuje to, jestli jsou statické vs. instance, nebo přesně to, s jakou signaturou se generují.

### <a name="handleof"></a>handleof

Kontextové klíčové slovo `handleof` převede pole, člen nebo typ do odpovídajícího typu `RuntimeHandle` pomocí instrukce `ldtoken`. Přesný typ výrazu bude záviset na typu názvu v `handleof`:

- pole: `RuntimeFieldHandle`
- Zadejte: `RuntimeTypeHandle`
- Metoda: `RuntimeMethodHandle`

Argumenty `handleof` jsou stejné jako `nameof`. Musí se jednat o jednoduchý název, kvalifikovaný název, přístup ke členům, základní přístup se zadaným členem nebo tento přístup se zadaným členem. Výraz argumentu identifikuje definici kódu, ale není nikdy vyhodnocena.

Výraz `handleof` je vyhodnocen za běhu a má návratový typ `RuntimeHandle`. To lze provést v bezpečném kódu i v případě nebezpečného. 

``` 
RuntimeHandle stringHandle = handleof(string);
```

Omezení této funkce:

- Ve výrazu `handleof` nelze použít vlastnosti.
- Výraz `handleof` nelze použít, je-li v oboru existující název `handleof`. Například typ, obor názvů atd...

### <a name="calli"></a>Calli

Kompilátor přidá podporu pro nový typ funkce `extern`, který se efektivně převede do `.calli` instrukcí. Atribut extern bude označen atributem následujícího tvaru:

``` csharp
[AttributeUsage(AttributeTargets.Method)]
public sealed class CallIndirectAttribute : Attribute
{
    public CallingConvention CallingConvention { get; }
    public CallIndirectAttribute(CallingConvention callingConvention)
    {
        CallingConvention = callingConvention;
    }
}
```

To umožňuje vývojářům definovat metody v následujícím tvaru:

``` csharp
[CallIndirect(CallingConvention.Cdecl)]
static extern int MapValue(string s, void *ptr);

unsafe {
    var i = MapValue("42", &int.Parse);
    Console.WriteLine(i);
}
```

Omezení pro metodu, která má použit atribut `CallIndirect`:

- Atribut `DllImport` nelze vytvořit.
- Nemůže být obecná.

## <a name="open-issues"></a>Otevřené problémy

### <a name="callingconvention"></a>CallingConvention

`CallIndirectAttribute`, jak je navrženo, používá výčet `CallingConvention`, který postrádá položku pro konvence spravovaného volání. Výčet musí být rozšířen tak, aby zahrnoval tuto konvenci volání, nebo aby atribut mohl použít jiný přístup.

## <a name="considerations"></a>Požadavky

### <a name="disambiguating-method-groups"></a>Nejednoznačnost skupin metod

K dispozici je diskuze o funkcích, které by usnadnily nejednoznačnost skupin metod předaných do výrazu adresy. Například pokud chcete přidat prvky podpisu do syntaxe:

``` csharp
class Util {
    public static void Log() { ... }
    public static void Log(string) { ... }
}

unsafe {
    // Error: ambiguous Log
    void *ptr1 = &Util.Log;

    // Use Util.Log();
    void *ptr2 = &Util.Log();
}
```

To bylo odmítnuto, protože se nepovedlo udělat nějaký přesvědčivý případ, a tady se nedá předcházet jednoduché syntaxe. K dispozici je také poměrně rovnou dopředné řešení: jednoduché definujte jinou metodu, která je jednoznačná a používá C# kód pro volání požadované funkce. 

``` csharp
class Workaround {
    public static void LocalLog() => Util.Log();
}
unsafe { 
    void* ptr = &Workaround.LocalLog;
}
```

To je ještě jednodušší, pokud `static` místní funkce zadejte jazyk. Rozhlasná operace by mohla být definována ve stejné funkci, která používala nejednoznačnou operaci adresy:

``` csharp
unsafe { 
    static void LocalLog() => Util.Log();
    void* ptr = &Workaround.LocalLog;
}
```

### <a name="loadtypetokenint32"></a>LoadTypeTokenInt32

Původní návrh povolený pro tokeny metadat se načítá jako hodnoty `int` v době kompilace. V podstatě je `tokenof`, které mají stejné argumenty jako `handleof`, ale vyhodnocují se v době kompilace do `int` konstanty. 

Tato chyba byla odmítnuta, protože způsobuje významné problémy s přepsáními IL (z nichž má mnoho rozhraní .NET). Tato přezapisovače často manipuluje s tabulkami metadat způsobem, který by mohl tyto hodnoty neověřit. Neexistuje žádný rozumný způsob, jakým by tato přezapisovače aktualizovala tyto hodnoty, když jsou uloženy jako jednoduché hodnoty `int`.

Základní nápad, který má mít neprůhledný popisovač pro položky metadat, bude nadále prozkoumávat tým modulu runtime. 

## <a name="future-considerations"></a>Budoucí požadavky

### <a name="static-local-functions"></a>statické místní funkce

To odkazuje na [Návrh](https://github.com/dotnet/csharplang/issues/1565) , který povolí modifikátor `static` na místních funkcích. Tato funkce by měla být zaručená, že se bude generovat jako `static` a s přesným podpisem zadaným ve zdrojovém kódu. Tato funkce by měla být platným argumentem pro `&`, protože neobsahuje žádné z problémů, které místní funkce dnes mají.

### <a name="nativecallableattribute"></a>NativeCallableAttribute

Modul CLR má funkci, která umožňuje vygenerovat spravované metody takovým způsobem, že jsou přímo z nativního kódu. To se provádí přidáním `NativeCallableAttribute` do metod. Taková metoda se dá volat jenom z nativního kódu, takže musí v signatuře obsahovat jenom přenositelné typy. Volání ze spravovaného kódu vede k chybě za běhu. 

Tato funkce by vypadala dobře s tímto návrhem, protože to umožní:

- Předání funkce definované ve spravovaném kódu do nativního kódu jako ukazatele na funkci (přes adresu) bez režie ve spravovaném nebo nativním kódu. 
- Modul runtime může zavést chyby webu pro tyto funkce ve spravovaném kódu a zabránit tak jejich vyvolání v době kompilace.




