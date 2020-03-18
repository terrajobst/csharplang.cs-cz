---
ms.openlocfilehash: a8822137c85f449444ed675c6f2912315c041691
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484473"
---
# <a name="static-delegates"></a>Statické Delegáti

* Navrženo [x]
* [] Prototyp: Nezahájeno
* [] Implementace: Nezahájeno
* [] Specifikace: Nezahájeno

## <a name="summary"></a>Souhrn
[summary]: #summary

Poskytněte pro C# jazyk základní a zjednodušenou funkci zpětného volání pro obecné účely.

## <a name="motivation"></a>Motivační
[motivation]: #motivation

V dnešní době mají uživatelé možnost vytvářet zpětná volání pomocí `System.Delegate`ho typu. Jsou to však poměrně těžké (například vyžadovat přidělení haldy a vždy mají za zpracování zřetězení zpětného volání).

Kromě toho `System.Delegate` neposkytuje nejlepší spolupráci s nespravovanými ukazateli na funkce, konkrétně v případě nešíření a vyžadování zařazování, kdykoli IT přechází ze spravovaného a nespravovaného ohraničení.

U několika menších vylepšení můžeme poskytnout nový typ delegáta, který je odlehčený, obecný a spolupracuje s nativním kódem.

## <a name="detailed-design"></a>Podrobný návrh
[design]: #detailed-design

Jedna by mohla deklarovat statický delegát přes následující:

```C#
static delegate int Func()
```

Jedna by mohla dále atributem deklarace s podobným `System.Runtime.InteropServices.UnmanagedFunctionPointer`, aby bylo možné řídit konvence volání, zařazování řetězců a nastavit poslední chybu chování. Poznámka: použití samotného `System.Runtime.InteropServices.UnmanagedFunctionPointer` nebude fungovat, protože je použitelné pouze pro delegáty.

Deklarace by byla přeložena do interní reprezentace kompilátorem, který je podobný následujícímu.

```C#
struct <Func>e__StaticDelegate
{
    IntPtr pFunction;

    static int WellKnownCompilerName();
}
```

To znamená, že je interně reprezentovaná strukturou, která má jednoho člena typu `IntPtr` (taková struktura je přenositelná a neposkytuje žádné přidělení haldy):
* Člen obsahuje adresu funkce, která má být zpětné volání.
* Typ deklaruje metodu, která odpovídá signatuře metody zpětného volání.
* Název struktury by neměl být User-constructible (stejně jako u jiných interně generovaných záložních struktur).
 * Například: vyrovnávací paměti pevné velikosti generují strukturu s názvem ve formátu `<name>e__FixedBuffer` (`<` a `>` jsou součástí identifikátoru a constructible se, že identifikátor není v C#, ale stále funguje v Il).
* Název deklarace metody by měl být známý název, který se používá ve všech typech statických delegátů (díky tomu může kompilátor znát název, který se má při určování podpisu najít).

Hodnota statického delegáta může být vázána pouze na statickou metodu, která odpovídá signatuře zpětného volání.

Zřetězení zpětných volání není podporováno.

Vyvolání zpětného volání by bylo implementováno `calli` instrukcí.

## <a name="drawbacks"></a>Nevýhody
[drawbacks]: #drawbacks

Statické Delegáti nefungují se stávajícími rozhraními API, která používají běžné delegáty (jeden by musel zabalit tohoto statického delegáta do pravidelného delegáta stejné signatury).
* Vzhledem k tomu, že `System.Delegate` je interně zastoupen jako sada `object` a `IntPtr` polí (http://source.dot.net/#System.Private.CoreLib/src/System/Delegate.cs), bylo možné umožnit implicitní převod statického delegáta na `System.Delegate`, který má odpovídající signaturu metody. Je také možné poskytnout explicitní převod v opačném směru za předpokladu, že `System.Delegate` vyhovuje všem požadavkům statického delegáta.

Pro zajištění snadného použití statického delegáta v rámci základní architektury by se vyžadovala další práce.

## <a name="alternatives"></a>Alternativy
[alternatives]: #alternatives

Bude doplněno

## <a name="unresolved-questions"></a>Nevyřešené dotazy
[unresolved]: #unresolved-questions

Jaké části návrhu jsou stále TBD?

## <a name="design-meetings"></a>Schůzky pro návrh

Odkaz na poznámky k návrhu, které mají vliv na tento návrh, a popište jednu větu pro každou změnu, na kterou vedla.


