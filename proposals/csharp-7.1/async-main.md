---
ms.openlocfilehash: 405153448d0e3685d6f22725e00d75d9250b3e20
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484795"
---
# <a name="async-main"></a>Asynchronní – hlavní

* Navrženo [x]
* [] Prototyp
* [] Implementace
* [] Specifikace

## <a name="summary"></a>Souhrn
[summary]: #summary

Povolí, aby se `await` používala v metodě Main/EntryPoint aplikace tím, že parametr entryPoint vrátí `Task` / `Task<int>` a bude označen jako `async`.

## <a name="motivation"></a>Motivační
[motivation]: #motivation

Je velmi běžné při učení C#, při psaní nástrojů založených na konzole a při psaní malých testovacích aplikací, které chtějí volat a `await` `async` metody z Main.  Dnes přidáváme úroveň složitosti tím, že vynutíte takové `await`"práce v samostatné asynchronní metodě, což způsobí, že vývojáři budou potřebovat napsat často používaný text, který se dá začít:

```csharp
public static void Main()
{
    MainAsync().GetAwaiter().GetResult();
}

private static async Task MainAsync()
{
    ... // Main body here
}
```

Potřebu tohoto často používaného textu můžeme odebrat a usnadnit tak začátek jednoduše tím, že se to bude `async` tak, aby se v něm mohla použít `await`s.

## <a name="detailed-design"></a>Podrobný návrh
[design]: #detailed-design

Následující signatury jsou aktuálně povolené vstupními poli:

```csharp
static void Main()
static void Main(string[])
static int Main()
static int Main(string[])
```

Rozšíříme seznam povolených vstupního bodu, které se mají zahrnout:

```csharp
static Task Main()
static Task<int> Main()
static Task Main(string[])
static Task<int> Main(string[])
```

Aby nedocházelo k rizikům při kompatibilitě, budou tyto nové signatury považovány za platné vstupní části pouze v případě, že nejsou k dispozici žádná přetížení předchozí sady.
Jazyk/kompilátor nebude vyžadovat, aby byl vstupní bod označený jako `async`, ale očekávalo se, že velká většina použití bude označená jako.

Pokud je jeden z nich identifikován jako EntryPoint, kompilátor syntetizuje skutečnou metodu EntryPoint, která volá jednu z těchto kódovaných metod:
- ```static Task Main()``` způsobí, že kompilátor vygeneruje ekvivalent ```private static void $GeneratedMain() => Main().GetAwaiter().GetResult();```
- ```static Task Main(string[])``` způsobí, že kompilátor vygeneruje ekvivalent ```private static void $GeneratedMain(string[] args) => Main(args).GetAwaiter().GetResult();```
- ```static Task<int> Main()``` způsobí, že kompilátor vygeneruje ekvivalent ```private static int $GeneratedMain() => Main().GetAwaiter().GetResult();```
- ```static Task<int> Main(string[])``` způsobí, že kompilátor vygeneruje ekvivalent ```private static int $GeneratedMain(string[] args) => Main(args).GetAwaiter().GetResult();```

Příklad použití:

```csharp
using System;
using System.Net.Http;

class Test
{
    static async Task Main(string[] args) =>
        Console.WriteLine(await new HttpClient().GetStringAsync(args[0]));
}
```

## <a name="drawbacks"></a>Nevýhody
[drawbacks]: #drawbacks

Hlavní nevýhodou je jednoduše dodatečná složitost podpory dalších podpisů typu EntryPoint.

## <a name="alternatives"></a>Alternativy
[alternatives]: #alternatives

Další uvažované varianty:

Povolení `async void`.  Musíme udržet sémantiku stejnou pro kód, který ho volá přímo, což by pak bylo obtížné, aby vygenerovaný vstupní bod mohl zavolat (žádný úkol nevrátil).  To jsme mohli vyřešit vygenerováním dvou dalších metod, např.

```csharp
public static async void Main()
{
   ... // await code
}
```

stane

```csharp
public static async void Main() => await $MainTask();

private static void $EntrypointMain() => Main().GetAwaiter().GetResult();

private static async Task $MainTask()
{
    ... // await code
}
```

K dispozici jsou také otázky k podpoře využití `async void`.

Použití "MainAsync" namísto "Main" jako názvu.  I když je pro metody vracející úlohy doporučena asynchronní přípona, primárně se jedná o funkce knihovny, které hlavní není, a podpora dalších názvů EntryPoint mimo "Main", nestojí to.

## <a name="unresolved-questions"></a>Nevyřešené dotazy
[unresolved]: #unresolved-questions

neuvedeno

## <a name="design-meetings"></a>Schůzky pro návrh

neuvedeno
