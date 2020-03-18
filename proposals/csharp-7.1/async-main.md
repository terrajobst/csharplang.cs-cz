---
ms.openlocfilehash: 405153448d0e3685d6f22725e00d75d9250b3e20
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484795"
---
# <a name="async-main"></a><span data-ttu-id="688fc-101">Asynchronní – hlavní</span><span class="sxs-lookup"><span data-stu-id="688fc-101">Async Main</span></span>

* <span data-ttu-id="688fc-102">Navrženo [x]</span><span class="sxs-lookup"><span data-stu-id="688fc-102">[x] Proposed</span></span>
* <span data-ttu-id="688fc-103">[] Prototyp</span><span class="sxs-lookup"><span data-stu-id="688fc-103">[ ] Prototype</span></span>
* <span data-ttu-id="688fc-104">[] Implementace</span><span class="sxs-lookup"><span data-stu-id="688fc-104">[ ] Implementation</span></span>
* <span data-ttu-id="688fc-105">[] Specifikace</span><span class="sxs-lookup"><span data-stu-id="688fc-105">[ ] Specification</span></span>

## <a name="summary"></a><span data-ttu-id="688fc-106">Souhrn</span><span class="sxs-lookup"><span data-stu-id="688fc-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="688fc-107">Povolí, aby se `await` používala v metodě Main/EntryPoint aplikace tím, že parametr entryPoint vrátí `Task` / `Task<int>` a bude označen jako `async`.</span><span class="sxs-lookup"><span data-stu-id="688fc-107">Allow `await` to be used in an application's Main / entrypoint method by allowing the entrypoint to return `Task` / `Task<int>` and be marked `async`.</span></span>

## <a name="motivation"></a><span data-ttu-id="688fc-108">Motivační</span><span class="sxs-lookup"><span data-stu-id="688fc-108">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="688fc-109">Je velmi běžné při učení C#, při psaní nástrojů založených na konzole a při psaní malých testovacích aplikací, které chtějí volat a `await` `async` metody z Main.</span><span class="sxs-lookup"><span data-stu-id="688fc-109">It is very common when learning C#, when writing console-based utilities, and when writing small test apps to want to call and `await` `async` methods from Main.</span></span>  <span data-ttu-id="688fc-110">Dnes přidáváme úroveň složitosti tím, že vynutíte takové `await`"práce v samostatné asynchronní metodě, což způsobí, že vývojáři budou potřebovat napsat často používaný text, který se dá začít:</span><span class="sxs-lookup"><span data-stu-id="688fc-110">Today we add a level of complexity here by forcing such `await`'ing to be done in a separate async method, which causes developers to need to write boilerplate like the following just to get started:</span></span>

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

<span data-ttu-id="688fc-111">Potřebu tohoto často používaného textu můžeme odebrat a usnadnit tak začátek jednoduše tím, že se to bude `async` tak, aby se v něm mohla použít `await`s.</span><span class="sxs-lookup"><span data-stu-id="688fc-111">We can remove the need for this boilerplate and make it easier to get started simply by allowing Main itself to be `async` such that `await`s can be used in it.</span></span>

## <a name="detailed-design"></a><span data-ttu-id="688fc-112">Podrobný návrh</span><span class="sxs-lookup"><span data-stu-id="688fc-112">Detailed design</span></span>
[design]: #detailed-design

<span data-ttu-id="688fc-113">Následující signatury jsou aktuálně povolené vstupními poli:</span><span class="sxs-lookup"><span data-stu-id="688fc-113">The following signatures are currently allowed entrypoints:</span></span>

```csharp
static void Main()
static void Main(string[])
static int Main()
static int Main(string[])
```

<span data-ttu-id="688fc-114">Rozšíříme seznam povolených vstupního bodu, které se mají zahrnout:</span><span class="sxs-lookup"><span data-stu-id="688fc-114">We extend the list of allowed entrypoints to include:</span></span>

```csharp
static Task Main()
static Task<int> Main()
static Task Main(string[])
static Task<int> Main(string[])
```

<span data-ttu-id="688fc-115">Aby nedocházelo k rizikům při kompatibilitě, budou tyto nové signatury považovány za platné vstupní části pouze v případě, že nejsou k dispozici žádná přetížení předchozí sady.</span><span class="sxs-lookup"><span data-stu-id="688fc-115">To avoid compatibility risks, these new signatures will only be considered as valid entrypoints if no overloads of the previous set are present.</span></span>
<span data-ttu-id="688fc-116">Jazyk/kompilátor nebude vyžadovat, aby byl vstupní bod označený jako `async`, ale očekávalo se, že velká většina použití bude označená jako.</span><span class="sxs-lookup"><span data-stu-id="688fc-116">The language / compiler will not require that the entrypoint be marked as `async`, though we expect the vast majority of uses will be marked as such.</span></span>

<span data-ttu-id="688fc-117">Pokud je jeden z nich identifikován jako EntryPoint, kompilátor syntetizuje skutečnou metodu EntryPoint, která volá jednu z těchto kódovaných metod:</span><span class="sxs-lookup"><span data-stu-id="688fc-117">When one of these is identified as the entrypoint, the compiler will synthesize an actual entrypoint method that calls one of these coded methods:</span></span>
- <span data-ttu-id="688fc-118">```static Task Main()``` způsobí, že kompilátor vygeneruje ekvivalent ```private static void $GeneratedMain() => Main().GetAwaiter().GetResult();```</span><span class="sxs-lookup"><span data-stu-id="688fc-118">```static Task Main()``` will result in the compiler emitting the equivalent of ```private static void $GeneratedMain() => Main().GetAwaiter().GetResult();```</span></span>
- <span data-ttu-id="688fc-119">```static Task Main(string[])``` způsobí, že kompilátor vygeneruje ekvivalent ```private static void $GeneratedMain(string[] args) => Main(args).GetAwaiter().GetResult();```</span><span class="sxs-lookup"><span data-stu-id="688fc-119">```static Task Main(string[])``` will result in the compiler emitting the equivalent of ```private static void $GeneratedMain(string[] args) => Main(args).GetAwaiter().GetResult();```</span></span>
- <span data-ttu-id="688fc-120">```static Task<int> Main()``` způsobí, že kompilátor vygeneruje ekvivalent ```private static int $GeneratedMain() => Main().GetAwaiter().GetResult();```</span><span class="sxs-lookup"><span data-stu-id="688fc-120">```static Task<int> Main()``` will result in the compiler emitting the equivalent of ```private static int $GeneratedMain() => Main().GetAwaiter().GetResult();```</span></span>
- <span data-ttu-id="688fc-121">```static Task<int> Main(string[])``` způsobí, že kompilátor vygeneruje ekvivalent ```private static int $GeneratedMain(string[] args) => Main(args).GetAwaiter().GetResult();```</span><span class="sxs-lookup"><span data-stu-id="688fc-121">```static Task<int> Main(string[])``` will result in the compiler emitting the equivalent of ```private static int $GeneratedMain(string[] args) => Main(args).GetAwaiter().GetResult();```</span></span>

<span data-ttu-id="688fc-122">Příklad použití:</span><span class="sxs-lookup"><span data-stu-id="688fc-122">Example usage:</span></span>

```csharp
using System;
using System.Net.Http;

class Test
{
    static async Task Main(string[] args) =>
        Console.WriteLine(await new HttpClient().GetStringAsync(args[0]));
}
```

## <a name="drawbacks"></a><span data-ttu-id="688fc-123">Nevýhody</span><span class="sxs-lookup"><span data-stu-id="688fc-123">Drawbacks</span></span>
[drawbacks]: #drawbacks

<span data-ttu-id="688fc-124">Hlavní nevýhodou je jednoduše dodatečná složitost podpory dalších podpisů typu EntryPoint.</span><span class="sxs-lookup"><span data-stu-id="688fc-124">The main drawback is simply the additional complexity of supporting additional entrypoint signatures.</span></span>

## <a name="alternatives"></a><span data-ttu-id="688fc-125">Alternativy</span><span class="sxs-lookup"><span data-stu-id="688fc-125">Alternatives</span></span>
[alternatives]: #alternatives

<span data-ttu-id="688fc-126">Další uvažované varianty:</span><span class="sxs-lookup"><span data-stu-id="688fc-126">Other variants considered:</span></span>

<span data-ttu-id="688fc-127">Povolení `async void`.</span><span class="sxs-lookup"><span data-stu-id="688fc-127">Allowing `async void`.</span></span>  <span data-ttu-id="688fc-128">Musíme udržet sémantiku stejnou pro kód, který ho volá přímo, což by pak bylo obtížné, aby vygenerovaný vstupní bod mohl zavolat (žádný úkol nevrátil).</span><span class="sxs-lookup"><span data-stu-id="688fc-128">We need to keep the semantics the same for code calling it directly, which would then make it difficult for a generated entrypoint to call it (no Task returned).</span></span>  <span data-ttu-id="688fc-129">To jsme mohli vyřešit vygenerováním dvou dalších metod, např.</span><span class="sxs-lookup"><span data-stu-id="688fc-129">We could solve this by generating two other methods, e.g.</span></span>

```csharp
public static async void Main()
{
   ... // await code
}
```

<span data-ttu-id="688fc-130">stane</span><span class="sxs-lookup"><span data-stu-id="688fc-130">becomes</span></span>

```csharp
public static async void Main() => await $MainTask();

private static void $EntrypointMain() => Main().GetAwaiter().GetResult();

private static async Task $MainTask()
{
    ... // await code
}
```

<span data-ttu-id="688fc-131">K dispozici jsou také otázky k podpoře využití `async void`.</span><span class="sxs-lookup"><span data-stu-id="688fc-131">There are also concerns around encouraging usage of `async void`.</span></span>

<span data-ttu-id="688fc-132">Použití "MainAsync" namísto "Main" jako názvu.</span><span class="sxs-lookup"><span data-stu-id="688fc-132">Using "MainAsync" instead of "Main" as the name.</span></span>  <span data-ttu-id="688fc-133">I když je pro metody vracející úlohy doporučena asynchronní přípona, primárně se jedná o funkce knihovny, které hlavní není, a podpora dalších názvů EntryPoint mimo "Main", nestojí to.</span><span class="sxs-lookup"><span data-stu-id="688fc-133">While the async suffix is recommended for Task-returning methods, that's primarily about library functionality, which Main is not, and supporting additional entrypoint names beyond "Main" is not worth it.</span></span>

## <a name="unresolved-questions"></a><span data-ttu-id="688fc-134">Nevyřešené dotazy</span><span class="sxs-lookup"><span data-stu-id="688fc-134">Unresolved questions</span></span>
[unresolved]: #unresolved-questions

<span data-ttu-id="688fc-135">neuvedeno</span><span class="sxs-lookup"><span data-stu-id="688fc-135">n/a</span></span>

## <a name="design-meetings"></a><span data-ttu-id="688fc-136">Schůzky pro návrh</span><span class="sxs-lookup"><span data-stu-id="688fc-136">Design meetings</span></span>

<span data-ttu-id="688fc-137">neuvedeno</span><span class="sxs-lookup"><span data-stu-id="688fc-137">n/a</span></span>
