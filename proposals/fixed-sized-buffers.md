---
ms.openlocfilehash: b51f27b2f58fd19851c80beb9cedcbd32b80b165
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484585"
---
# <a name="fixed-sized-buffers"></a>Vyrovnávací paměti pevné velikosti

* Navrženo [x]
* [] Prototyp: Nezahájeno
* [] Implementace: Nezahájeno
* [] Specifikace: Nezahájeno

## <a name="summary"></a>Souhrn
[summary]: #summary

Poskytněte obecný a bezpečný mechanismus pro deklarování vyrovnávacích pamětí s pevnou velikostí do C# jazyka.

## <a name="motivation"></a>Motivační
[motivation]: #motivation

V současné době uživatelé mají možnost vytvářet vyrovnávací paměti s pevnou velikostí v nezabezpečeném kontextu. To však vyžaduje, aby uživatel mohl pracovat s ukazateli, ručně provádět kontroly hranic a podporuje pouze omezenou sadu typů (`bool`, `byte`, `char`, `short`, `int`, `long`, `sbyte`, `ushort`, `uint`, `ulong`, `float`a `double`).

Nejběžnější stížnost je, že vyrovnávací paměti s pevnou velikostí nelze indexovat v bezpečném kódu. Neschopnost používat další typy jsou sekundy.

V několika menších vylepšeních jsme mohli poskytnout obecné vyrovnávací paměti s pevnou velikostí, které podporují libovolný typ, mohou být použity v bezpečném kontextu a provedeny automatické kontroly hranic.

## <a name="detailed-design"></a>Podrobný návrh
[design]: #detailed-design

Jedna z nich by mohla deklarovat zabezpečenou vyrovnávací paměť s pevnou velikostí pomocí následujících možností:

```csharp
public fixed DXGI_RGB GammaCurve[1025];
```

Deklarace by byla přeložena do interní reprezentace kompilátorem, který je podobný následujícímu.

```csharp
[FixedBuffer(typeof(DXGI_RGB), 1024)]
public ConsoleApp1.<Buffer>e__FixedBuffer_1024<DXGI_RGB> GammaCurve;

// Pack = 0 is the default packing and should result in indexable layout.
[CompilerGenerated, UnsafeValueType, StructLayout(LayoutKind.Sequential, Pack = 0)]
struct <Buffer>e__FixedBuffer_1024<T>
{
    private T _e0;
    private T _e1;
    // _e2 ... _e1023
    private T _e1024;

    public ref T this[int index] => ref (uint)index <= 1024u ?
                                         ref RefAdd<T>(ref _e0, index):
                                         throw new IndexOutOfRange();
}
```

Vzhledem k tomu, že vyrovnávací paměti s pevnou velikostí již nevyžadují použití `fixed`, má smysl, aby byl povolen libovolný typ elementu.  

> Poznámka: `fixed` se pořád podporuje, ale jenom v případě, že je typ elementu `blittable`

## <a name="drawbacks"></a>Nevýhody
[drawbacks]: #drawbacks

* Mohlo dojít k problémům s zpětnou kompatibilitou, ale vzhledem k tomu, že existující vyrovnávací paměti s pevnou velikostí fungují pouze s výběrem primitivních typů, je vhodné, aby kompilátor pokračoval "Just-buffer", pokud uživatel považuje pevnou vyrovnávací paměť jako ukazatele.
* Nekompatibilní konstrukce mohou vyžadovat použití mírně odlišného kódování `v2` pro skrytí polí ze starého kompilátoru.
* Balení není správně definované v specifikaci IL pro obecné typy. I když by měl tento přístup fungovat, bude se vycházet z nedokumentované reakce. Měli bychom tuto dokumentaci dělat a zajistěte, aby další nepovolují kompilátory JIT, jako je mono, měly stejné chování.
* Zadání samostatného typu pro každou délku (případně další pro `readonly` pole, pokud se podporuje) bude mít vliv na metadata. Bude vázán podle počtu polí různých velikostí v dané aplikaci.
* `ref` Math nelze formálně ověřit (protože je nebezpečný). Budeme muset najít způsob, jak aktualizovat pravidla ověřování, abychom věděli, že naše použití je v pořádku.

## <a name="alternatives"></a>Alternativy
[alternatives]: #alternatives

Manuálně deklarujte své struktury a pomocí nebezpečného kódu Sestavujte indexery.

## <a name="unresolved-questions"></a>Nevyřešené dotazy
[unresolved]: #unresolved-questions

- Měli bychom `readonly`?  (s indexerem jen pro čtení)
- je potřeba, abychom povolili Inicializátory polí?
- je klíčové slovo `fixed` nutné?
- `foreach`?
- pouze pole instancí v strukturách?

## <a name="design-meetings"></a>Schůzky pro návrh

Odkaz na poznámky k návrhu, které mají vliv na tento návrh, a popište jednu větu pro každou změnu, na kterou vedla.