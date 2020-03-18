---
ms.openlocfilehash: 340a1dc5a2eac653458d7d29f74551e5fe28b6d5
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484557"
---
# <a name="operators-should-be-exposed-for-systemintptr-and-systemuintptr"></a>Operátory by měly být vystavené pro `System.IntPtr` a `System.UIntPtr`

* Navrženo [x]
* [] Prototyp: Nezahájeno
* [] Implementace: Nezahájeno
* [] Specifikace: Nezahájeno

## <a name="summary"></a>Souhrn
[summary]: #summary

Modul CLR podporuje sadu operátorů pro `System.IntPtr` a typy `System.UIntPtr` (`native int`). Tyto operátory lze zobrazit v `III.1.5` specifikace Common Language Infrastructure (`ECMA-335`). Nicméně tyto operátory nejsou podporovány nástrojem C#.

Pro úplnou sadu operátorů podporovaných nástrojem `System.IntPtr` a `System.UIntPtr`by měla být poskytnuta jazyková podpora. Tyto operátory jsou: `Add`, `Divide`, `Multiply`, `Remainder`, `Subtract`, `Negate`, `Equals`, `Compare`, `And`, `Not`, `Or`, `XOr`, `ShiftLeft`, `ShiftRight`.

## <a name="motivation"></a>Motivační
[motivation]: #motivation

Dnes mohou uživatelé snadno psát C# aplikace cílené na více platforem pomocí různých nástrojů a architektur, například: `Xamarin`, `.NET Core`, `Mono`atd...

Při psaní kódu pro různé platformy je často potřeba napsat interoperabilitový kód, který komunikuje s konkrétní cílovou platformou konkrétním způsobem. To může zahrnovat psaní kódu grafiky, volání některých systémových rozhraní API nebo interakce se stávající nativní knihovnou.

Tento kód spolupráce často musí zabývat se popisovači, nespravovanou pamětí nebo dokonce jenom celých čísel specifických pro konkrétní platformu.

Modul runtime poskytuje podporu pro tím, že definuje sadu operátorů, které lze použít na primitivních typech `native int` (`System.IntPtr`) a `native unsigned int` (`System.UIntPtr`).

C#Tyto operátory nikdy nepodporovaly, takže uživatelé musí tento problém obejít. To často zvyšuje složitost kódu a snižuje udržovatelnost kódu.

V takovém případě by měl jazyk začít podporovat tyto operátory, aby bylo možné lépe podporovat tyto požadavky.

## <a name="detailed-design"></a>Podrobný návrh
[design]: #detailed-design

Kompletní sada podporovaných operátorů je definována v `III.1.5` specifikace Common Language Infrastructure (`ECMA-335`). Specifikace je k dispozici zde: [https://www.ecma-international.org/publications/files/ECMA-ST/ECMA-335.pdf](https://www.ecma-international.org/publications/files/ECMA-ST/ECMA-335.pdf).

* Souhrn operátorů je k dispozici níže pro usnadnění.
* Neověřitelné operátory definované specifikací CLI nejsou uvedené a v tuto chvíli nejsou v současnosti součástí tohoto návrhu (i když to může být potřeba zvážit i jejich zvážení).
* Poskytnutí klíčového slova (například `nint` a `nuint`) ani poskytnutí způsobu, jak deklarovat literály pro `System.IntPtr` a `System.UIntPtr` (například 0n), není součástí tohoto návrhu (i když je možné ho zvážit i).

### <a name="unary-plus-operator"></a>Unární operátor plus

```csharp
System.IntPtr operator +(System.IntPtr)
```

```csharp
System.UIntPtr operator +(System.UIntPtr)
```

### <a name="unary-minus-operator"></a>Unární operátor minus

```csharp
System.IntPtr operator -(System.IntPtr)
```

### <a name="bitwise-complement-operator"></a>Operátor bitového doplňku

```csharp
System.IntPtr operator ~(System.IntPtr)
```

```csharp
System.UIntPtr operator ~(System.UIntPtr)
```

### <a name="cast-operators"></a>Operátory přetypování

```csharp
explicit operator sbyte(System.IntPtr)               // Truncate
explicit operator short(System.IntPtr)               // Truncate
explicit operator int(System.IntPtr)                 // Truncate
explicit operator long(System.IntPtr)                // Sign Extend

explicit operator byte(System.IntPtr)                // Truncate
explicit operator ushort(System.IntPtr)              // Truncate
explicit operator uint(System.IntPtr)                // Truncate
explicit operator ulong(System.IntPtr)               // Zero Extend

explicit operator System.IntPtr(int)                 // Sign Extend
explicit operator System.IntPtr(long)                // Truncate

explicit operator System.IntPtr(uint)                // Sign Extend
explicit operator System.IntPtr(ulong)               // Truncate

explicit operator System.IntPtr(System.IntPtr)
explicit operator System.IntPtr(System.UIntPtr)
```

```csharp
explicit operator sbyte(System.UIntPtr)               // Truncate
explicit operator short(System.UIntPtr)               // Truncate
explicit operator int(System.UIntPtr)                 // Truncate
explicit operator long(System.UIntPtr)                // Sign Extend

explicit operator byte(System.UIntPtr)                // Truncate
explicit operator ushort(System.UIntPtr)              // Truncate
explicit operator uint(System.UIntPtr)                // Truncate
explicit operator ulong(System.UIntPtr)               // Zero Extend

explicit operator System.UIntPtr(int)                 // Zero Extend
explicit operator System.UIntPtr(long)                // Truncate

explicit operator System.UIntPtr(uint)                // Zero Extend
explicit operator System.UIntPtr(ulong)               // Truncate

explicit operator System.UIntPtr(System.IntPtr)
explicit operator System.UIntPtr(System.UIntPtr)
```

### <a name="multiplication-operator"></a>Operátor násobení

```csharp
System.IntPtr operator *(int, System.IntPtr)
System.IntPtr operator *(System.IntPtr, int)
System.IntPtr operator *(System.IntPtr, System.IntPtr)
```

```csharp
System.UIntPtr operator *(uint, System.UIntPtr)
System.UIntPtr operator *(System.UIntPtr, uint)
System.UIntPtr operator *(System.UIntPtr, System.UIntPtr)
```

### <a name="division-operator"></a>Operátor dělení

```csharp
System.IntPtr operator /(int, System.IntPtr)
System.IntPtr operator /(System.IntPtr, int)
System.IntPtr operator /(System.IntPtr, System.IntPtr)
```

```csharp
System.UIntPtr operator /(uint, System.UIntPtr)
System.UIntPtr operator /(System.UIntPtr, uint)
System.UIntPtr operator /(System.UIntPtr, System.UIntPtr)
```

### <a name="remainder-operator"></a>Operátor zbývající

```csharp
System.IntPtr operator %(int, System.IntPtr)
System.IntPtr operator %(System.IntPtr, int)
System.IntPtr operator %(System.IntPtr, System.IntPtr)
```

```csharp
System.UIntPtr operator %(uint, System.UIntPtr)
System.UIntPtr operator %(System.UIntPtr, uint)
System.UIntPtr operator %(System.UIntPtr, System.UIntPtr)
```

### <a name="addition-operator"></a>Operátor sčítání

```csharp
System.IntPtr operator +(int, System.IntPtr)
System.IntPtr operator +(System.IntPtr, int)
System.IntPtr operator +(System.IntPtr, System.IntPtr)
```

```csharp
System.UIntPtr operator +(uint, System.UIntPtr)
System.UIntPtr operator +(System.UIntPtr, uint)
System.UIntPtr operator +(System.UIntPtr, System.UIntPtr)
```

### <a name="subtraction-operator"></a>Operátor odčítání

```csharp
System.IntPtr operator -(int, System.IntPtr)
System.IntPtr operator -(System.IntPtr, int)
System.IntPtr operator -(System.IntPtr, System.IntPtr)
```

```csharp
System.UIntPtr operator -(uint, System.UIntPtr)
System.UIntPtr operator -(System.UIntPtr, uint)
System.UIntPtr operator -(System.UIntPtr, System.UIntPtr)
```

### <a name="shift-operators"></a>Operátory posunutí

```csharp
System.IntPtr operator <<(System.IntPtr, int)
System.IntPtr operator >>(System.IntPtr, int)
```

```csharp
System.UIntPtr operator <<(System.UIntPtr, int)
System.UIntPtr operator >>(System.UIntPtr, int)
```

### <a name="integer-comparison-operators"></a>Operátory porovnání celých čísel

```csharp
bool operator ==(int, System.IntPtr)
bool operator ==(System.IntPtr, int)
bool operator ==(System.IntPtr, System.IntPtr)

bool operator !=(int, System.IntPtr)
bool operator !=(System.IntPtr, int)
bool operator !=(System.IntPtr, System.IntPtr)

bool operator  <(int, System.IntPtr)
bool operator  <(System.IntPtr, int)
bool operator  <(System.IntPtr, System.IntPtr)

bool operator  >(int, System.IntPtr)
bool operator  >(System.IntPtr, int)
bool operator  >(System.IntPtr, System.IntPtr)

bool operator <=(int, System.IntPtr)
bool operator <=(System.IntPtr, int)
bool operator <=(System.IntPtr, System.IntPtr)

bool operator >=(int, System.IntPtr)
bool operator >=(System.IntPtr, int)
bool operator >=(System.IntPtr, System.IntPtr)
```

```csharp
bool operator ==(uint, System.UIntPtr)
bool operator ==(System.UIntPtr, uint)
bool operator ==(System.UIntPtr, System.UIntPtr)

bool operator !=(uint, System.UIntPtr)
bool operator !=(System.UIntPtr, uint)
bool operator !=(System.UIntPtr, System.UIntPtr)

bool operator  <(uint, System.UIntPtr)
bool operator  <(System.UIntPtr, uint)
bool operator  <(System.UIntPtr, System.UIntPtr)

bool operator  >(uint, System.UIntPtr)
bool operator  >(System.UIntPtr, uint)
bool operator  >(System.UIntPtr, System.UIntPtr)

bool operator <=(uint, System.UIntPtr)
bool operator <=(System.UIntPtr, uint)
bool operator <=(System.UIntPtr, System.UIntPtr)

bool operator >=(uint, System.UIntPtr)
bool operator >=(System.UIntPtr, uint)
bool operator >=(System.UIntPtr, System.UIntPtr)
```

### <a name="integer-logical-operators"></a>Logické operátory typu Integer

```csharp
System.IntPtr operator &(int, System.IntPtr)
System.IntPtr operator &(System.IntPtr, int)
System.IntPtr operator &(System.IntPtr, System.IntPtr)

System.IntPtr operator |(int, System.IntPtr)
System.IntPtr operator |(System.IntPtr, int)
System.IntPtr operator |(System.IntPtr, System.IntPtr)

System.IntPtr operator ^(int, System.IntPtr)
System.IntPtr operator ^(System.IntPtr, int)
System.IntPtr operator ^(System.IntPtr, System.IntPtr)
```

```csharp
System.UIntPtr operator &(uint, System.UIntPtr)
System.UIntPtr operator &(System.UIntPtr, uint)
System.UIntPtr operator &(System.UIntPtr, System.UIntPtr)

System.UIntPtr operator |(uint, System.UIntPtr)
System.UIntPtr operator |(System.UIntPtr, uint)
System.UIntPtr operator |(System.UIntPtr, System.UIntPtr)

System.UIntPtr operator ^(uint, System.UIntPtr)
System.UIntPtr operator ^(System.UIntPtr, uint)
System.UIntPtr operator ^(System.UIntPtr, System.UIntPtr)
```

## <a name="drawbacks"></a>Nevýhody
[drawbacks]: #drawbacks

Skutečné použití těchto operátorů může být malé a omezené na koncové uživatele, kteří zapisují knihovny nižší úrovně nebo kód vzájemné spolupráce. Většina koncových uživatelů by pravděpodobně využívala tyto knihovny nižší úrovně, které by měly mít nativní velikost, popisovače a kód vzájemné spolupráce. V takovém případě by nemuseli mít samotné operátory.

## <a name="alternatives"></a>Alternativy
[alternatives]: #alternatives

Rozhraní implementují požadované operátory jejich zapsáním přímo v IL. Kromě toho může modul runtime poskytovat vnitřní podporu pro operátory definované rozhraním, aby bylo možné lépe optimalizovat konečný výkon.

## <a name="unresolved-questions"></a>Nevyřešené dotazy
[unresolved]: #unresolved-questions

Jaké části návrhu jsou stále TBD?

## <a name="design-meetings"></a>Schůzky pro návrh

Odkaz na poznámky k návrhu, které mají vliv na tento návrh, a popište jednu větu pro každou změnu, na kterou vedla.