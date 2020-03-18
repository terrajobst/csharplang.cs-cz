---
ms.openlocfilehash: 340a1dc5a2eac653458d7d29f74551e5fe28b6d5
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484557"
---
# <a name="operators-should-be-exposed-for-systemintptr-and-systemuintptr"></a><span data-ttu-id="2d5c8-101">Operátory by měly být vystavené pro `System.IntPtr` a `System.UIntPtr`</span><span class="sxs-lookup"><span data-stu-id="2d5c8-101">Operators should be exposed for `System.IntPtr` and `System.UIntPtr`</span></span>

* <span data-ttu-id="2d5c8-102">Navrženo [x]</span><span class="sxs-lookup"><span data-stu-id="2d5c8-102">[x] Proposed</span></span>
* <span data-ttu-id="2d5c8-103">[] Prototyp: Nezahájeno</span><span class="sxs-lookup"><span data-stu-id="2d5c8-103">[ ] Prototype: Not Started</span></span>
* <span data-ttu-id="2d5c8-104">[] Implementace: Nezahájeno</span><span class="sxs-lookup"><span data-stu-id="2d5c8-104">[ ] Implementation: Not Started</span></span>
* <span data-ttu-id="2d5c8-105">[] Specifikace: Nezahájeno</span><span class="sxs-lookup"><span data-stu-id="2d5c8-105">[ ] Specification: Not Started</span></span>

## <a name="summary"></a><span data-ttu-id="2d5c8-106">Souhrn</span><span class="sxs-lookup"><span data-stu-id="2d5c8-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="2d5c8-107">Modul CLR podporuje sadu operátorů pro `System.IntPtr` a typy `System.UIntPtr` (`native int`).</span><span class="sxs-lookup"><span data-stu-id="2d5c8-107">The CLR supports a set of operators for the `System.IntPtr` and `System.UIntPtr` types (`native int`).</span></span> <span data-ttu-id="2d5c8-108">Tyto operátory lze zobrazit v `III.1.5` specifikace Common Language Infrastructure (`ECMA-335`).</span><span class="sxs-lookup"><span data-stu-id="2d5c8-108">These operators can be seen in `III.1.5` of the Common Language Infrastructure specification (`ECMA-335`).</span></span> <span data-ttu-id="2d5c8-109">Nicméně tyto operátory nejsou podporovány nástrojem C#.</span><span class="sxs-lookup"><span data-stu-id="2d5c8-109">However, these operators are not supported by C#.</span></span>

<span data-ttu-id="2d5c8-110">Pro úplnou sadu operátorů podporovaných nástrojem `System.IntPtr` a `System.UIntPtr`by měla být poskytnuta jazyková podpora.</span><span class="sxs-lookup"><span data-stu-id="2d5c8-110">Language support should be provided for the full set of operators supported by `System.IntPtr` and `System.UIntPtr`.</span></span> <span data-ttu-id="2d5c8-111">Tyto operátory jsou: `Add`, `Divide`, `Multiply`, `Remainder`, `Subtract`, `Negate`, `Equals`, `Compare`, `And`, `Not`, `Or`, `XOr`, `ShiftLeft`, `ShiftRight`.</span><span class="sxs-lookup"><span data-stu-id="2d5c8-111">These operators are: `Add`, `Divide`, `Multiply`, `Remainder`, `Subtract`, `Negate`, `Equals`, `Compare`, `And`, `Not`, `Or`, `XOr`, `ShiftLeft`, `ShiftRight`.</span></span>

## <a name="motivation"></a><span data-ttu-id="2d5c8-112">Motivační</span><span class="sxs-lookup"><span data-stu-id="2d5c8-112">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="2d5c8-113">Dnes mohou uživatelé snadno psát C# aplikace cílené na více platforem pomocí různých nástrojů a architektur, například: `Xamarin`, `.NET Core`, `Mono`atd...</span><span class="sxs-lookup"><span data-stu-id="2d5c8-113">Today, users can easily write C# applications targeting multiple platforms using various tools and frameworks, such as: `Xamarin`, `.NET Core`, `Mono`, etc...</span></span>

<span data-ttu-id="2d5c8-114">Při psaní kódu pro různé platformy je často potřeba napsat interoperabilitový kód, který komunikuje s konkrétní cílovou platformou konkrétním způsobem.</span><span class="sxs-lookup"><span data-stu-id="2d5c8-114">When writing cross-platform code, it is often necessary to write interop code that interacts with a particular target platform in a specific manner.</span></span> <span data-ttu-id="2d5c8-115">To může zahrnovat psaní kódu grafiky, volání některých systémových rozhraní API nebo interakce se stávající nativní knihovnou.</span><span class="sxs-lookup"><span data-stu-id="2d5c8-115">This could include writing graphics code, calling some System API, or interacting with an existing native library.</span></span>

<span data-ttu-id="2d5c8-116">Tento kód spolupráce často musí zabývat se popisovači, nespravovanou pamětí nebo dokonce jenom celých čísel specifických pro konkrétní platformu.</span><span class="sxs-lookup"><span data-stu-id="2d5c8-116">This interop code often has to deal with handles, unmanaged memory, or even just platform-specific sized integers.</span></span>

<span data-ttu-id="2d5c8-117">Modul runtime poskytuje podporu pro tím, že definuje sadu operátorů, které lze použít na primitivních typech `native int` (`System.IntPtr`) a `native unsigned int` (`System.UIntPtr`).</span><span class="sxs-lookup"><span data-stu-id="2d5c8-117">The runtime provides support for this by defining a set of operators that can be used on the `native int` (`System.IntPtr`) and `native unsigned int` (`System.UIntPtr`) primitive types.</span></span>

<span data-ttu-id="2d5c8-118">C#Tyto operátory nikdy nepodporovaly, takže uživatelé musí tento problém obejít.</span><span class="sxs-lookup"><span data-stu-id="2d5c8-118">C# has never supported these operators and so users have to work around the issue.</span></span> <span data-ttu-id="2d5c8-119">To často zvyšuje složitost kódu a snižuje udržovatelnost kódu.</span><span class="sxs-lookup"><span data-stu-id="2d5c8-119">This often increases code complexity and lowers code maintainability.</span></span>

<span data-ttu-id="2d5c8-120">V takovém případě by měl jazyk začít podporovat tyto operátory, aby bylo možné lépe podporovat tyto požadavky.</span><span class="sxs-lookup"><span data-stu-id="2d5c8-120">As such, the language should begin to support these operators to help advance the language to better support these requirements.</span></span>

## <a name="detailed-design"></a><span data-ttu-id="2d5c8-121">Podrobný návrh</span><span class="sxs-lookup"><span data-stu-id="2d5c8-121">Detailed design</span></span>
[design]: #detailed-design

<span data-ttu-id="2d5c8-122">Kompletní sada podporovaných operátorů je definována v `III.1.5` specifikace Common Language Infrastructure (`ECMA-335`).</span><span class="sxs-lookup"><span data-stu-id="2d5c8-122">The full set of operators supported are defined in `III.1.5` of the Common Language Infrastructure specification (`ECMA-335`).</span></span> <span data-ttu-id="2d5c8-123">Specifikace je k dispozici zde: [https://www.ecma-international.org/publications/files/ECMA-ST/ECMA-335.pdf](https://www.ecma-international.org/publications/files/ECMA-ST/ECMA-335.pdf).</span><span class="sxs-lookup"><span data-stu-id="2d5c8-123">The specification is available here: [https://www.ecma-international.org/publications/files/ECMA-ST/ECMA-335.pdf](https://www.ecma-international.org/publications/files/ECMA-ST/ECMA-335.pdf).</span></span>

* <span data-ttu-id="2d5c8-124">Souhrn operátorů je k dispozici níže pro usnadnění.</span><span class="sxs-lookup"><span data-stu-id="2d5c8-124">A summary of the operators is provided below for convenience.</span></span>
* <span data-ttu-id="2d5c8-125">Neověřitelné operátory definované specifikací CLI nejsou uvedené a v tuto chvíli nejsou v současnosti součástí tohoto návrhu (i když to může být potřeba zvážit i jejich zvážení).</span><span class="sxs-lookup"><span data-stu-id="2d5c8-125">The unverifiable operators defined by the CLI spec are not listed and are not currently part of this proposal (although it may be worth considering these as well).</span></span>
* <span data-ttu-id="2d5c8-126">Poskytnutí klíčového slova (například `nint` a `nuint`) ani poskytnutí způsobu, jak deklarovat literály pro `System.IntPtr` a `System.UIntPtr` (například 0n), není součástí tohoto návrhu (i když je možné ho zvážit i).</span><span class="sxs-lookup"><span data-stu-id="2d5c8-126">Providing a keyword (such as `nint` and `nuint`) nor providing a way to for literals to be declared for `System.IntPtr` and `System.UIntPtr` (such as 0n) is not part of this proposal (although it may be worth considering these as well).</span></span>

### <a name="unary-plus-operator"></a><span data-ttu-id="2d5c8-127">Unární operátor plus</span><span class="sxs-lookup"><span data-stu-id="2d5c8-127">Unary Plus Operator</span></span>

```csharp
System.IntPtr operator +(System.IntPtr)
```

```csharp
System.UIntPtr operator +(System.UIntPtr)
```

### <a name="unary-minus-operator"></a><span data-ttu-id="2d5c8-128">Unární operátor minus</span><span class="sxs-lookup"><span data-stu-id="2d5c8-128">Unary Minus Operator</span></span>

```csharp
System.IntPtr operator -(System.IntPtr)
```

### <a name="bitwise-complement-operator"></a><span data-ttu-id="2d5c8-129">Operátor bitového doplňku</span><span class="sxs-lookup"><span data-stu-id="2d5c8-129">Bitwise Complement Operator</span></span>

```csharp
System.IntPtr operator ~(System.IntPtr)
```

```csharp
System.UIntPtr operator ~(System.UIntPtr)
```

### <a name="cast-operators"></a><span data-ttu-id="2d5c8-130">Operátory přetypování</span><span class="sxs-lookup"><span data-stu-id="2d5c8-130">Cast Operators</span></span>

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

### <a name="multiplication-operator"></a><span data-ttu-id="2d5c8-131">Operátor násobení</span><span class="sxs-lookup"><span data-stu-id="2d5c8-131">Multiplication Operator</span></span>

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

### <a name="division-operator"></a><span data-ttu-id="2d5c8-132">Operátor dělení</span><span class="sxs-lookup"><span data-stu-id="2d5c8-132">Division Operator</span></span>

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

### <a name="remainder-operator"></a><span data-ttu-id="2d5c8-133">Operátor zbývající</span><span class="sxs-lookup"><span data-stu-id="2d5c8-133">Remainder Operator</span></span>

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

### <a name="addition-operator"></a><span data-ttu-id="2d5c8-134">Operátor sčítání</span><span class="sxs-lookup"><span data-stu-id="2d5c8-134">Addition Operator</span></span>

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

### <a name="subtraction-operator"></a><span data-ttu-id="2d5c8-135">Operátor odčítání</span><span class="sxs-lookup"><span data-stu-id="2d5c8-135">Subtraction Operator</span></span>

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

### <a name="shift-operators"></a><span data-ttu-id="2d5c8-136">Operátory posunutí</span><span class="sxs-lookup"><span data-stu-id="2d5c8-136">Shift Operators</span></span>

```csharp
System.IntPtr operator <<(System.IntPtr, int)
System.IntPtr operator >>(System.IntPtr, int)
```

```csharp
System.UIntPtr operator <<(System.UIntPtr, int)
System.UIntPtr operator >>(System.UIntPtr, int)
```

### <a name="integer-comparison-operators"></a><span data-ttu-id="2d5c8-137">Operátory porovnání celých čísel</span><span class="sxs-lookup"><span data-stu-id="2d5c8-137">Integer Comparison Operators</span></span>

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

### <a name="integer-logical-operators"></a><span data-ttu-id="2d5c8-138">Logické operátory typu Integer</span><span class="sxs-lookup"><span data-stu-id="2d5c8-138">Integer Logical Operators</span></span>

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

## <a name="drawbacks"></a><span data-ttu-id="2d5c8-139">Nevýhody</span><span class="sxs-lookup"><span data-stu-id="2d5c8-139">Drawbacks</span></span>
[drawbacks]: #drawbacks

<span data-ttu-id="2d5c8-140">Skutečné použití těchto operátorů může být malé a omezené na koncové uživatele, kteří zapisují knihovny nižší úrovně nebo kód vzájemné spolupráce.</span><span class="sxs-lookup"><span data-stu-id="2d5c8-140">The actual use of these operators may be small and limited to end-users who are writing lower level libraries or interop code.</span></span> <span data-ttu-id="2d5c8-141">Většina koncových uživatelů by pravděpodobně využívala tyto knihovny nižší úrovně, které by měly mít nativní velikost, popisovače a kód vzájemné spolupráce.</span><span class="sxs-lookup"><span data-stu-id="2d5c8-141">Most end-users would likely be consuming these lower level libraries themselves which would have the native sized integers, handles, and interop code abstracted away.</span></span> <span data-ttu-id="2d5c8-142">V takovém případě by nemuseli mít samotné operátory.</span><span class="sxs-lookup"><span data-stu-id="2d5c8-142">As such, they would not have need of the operators themselves.</span></span>

## <a name="alternatives"></a><span data-ttu-id="2d5c8-143">Alternativy</span><span class="sxs-lookup"><span data-stu-id="2d5c8-143">Alternatives</span></span>
[alternatives]: #alternatives

<span data-ttu-id="2d5c8-144">Rozhraní implementují požadované operátory jejich zapsáním přímo v IL.</span><span class="sxs-lookup"><span data-stu-id="2d5c8-144">Have the framework implement the required operators by writing them directly in IL.</span></span> <span data-ttu-id="2d5c8-145">Kromě toho může modul runtime poskytovat vnitřní podporu pro operátory definované rozhraním, aby bylo možné lépe optimalizovat konečný výkon.</span><span class="sxs-lookup"><span data-stu-id="2d5c8-145">Additionally, the runtime could provide intrinsic support for the operators defined by the framework, so as to better optimize the end performance.</span></span>

## <a name="unresolved-questions"></a><span data-ttu-id="2d5c8-146">Nevyřešené dotazy</span><span class="sxs-lookup"><span data-stu-id="2d5c8-146">Unresolved questions</span></span>
[unresolved]: #unresolved-questions

<span data-ttu-id="2d5c8-147">Jaké části návrhu jsou stále TBD?</span><span class="sxs-lookup"><span data-stu-id="2d5c8-147">What parts of the design are still TBD?</span></span>

## <a name="design-meetings"></a><span data-ttu-id="2d5c8-148">Schůzky pro návrh</span><span class="sxs-lookup"><span data-stu-id="2d5c8-148">Design meetings</span></span>

<span data-ttu-id="2d5c8-149">Odkaz na poznámky k návrhu, které mají vliv na tento návrh, a popište jednu větu pro každou změnu, na kterou vedla.</span><span class="sxs-lookup"><span data-stu-id="2d5c8-149">Link to design notes that affect this proposal, and describe in one sentence for each what changes they led to.</span></span>