---
ms.openlocfilehash: 9647fff40a1e45bef917f140612ae4e91abea958
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484571"
---
# <a name="lambda-attributes"></a><span data-ttu-id="a3630-101">Atributy lambda</span><span class="sxs-lookup"><span data-stu-id="a3630-101">Lambda Attributes</span></span>

* <span data-ttu-id="a3630-102">Navrženo [x]</span><span class="sxs-lookup"><span data-stu-id="a3630-102">[x] Proposed</span></span>
* <span data-ttu-id="a3630-103">[] Prototyp</span><span class="sxs-lookup"><span data-stu-id="a3630-103">[ ] Prototype</span></span>
* <span data-ttu-id="a3630-104">[] Implementace</span><span class="sxs-lookup"><span data-stu-id="a3630-104">[ ] Implementation</span></span>
* <span data-ttu-id="a3630-105">[] Specifikace</span><span class="sxs-lookup"><span data-stu-id="a3630-105">[ ] Specification</span></span>

## <a name="summary"></a><span data-ttu-id="a3630-106">Souhrn</span><span class="sxs-lookup"><span data-stu-id="a3630-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="a3630-107">Povolí použití atributů pro výrazy lambda (a anonymní metody) a pro parametry lambda/anonymní, protože mohou být v pravidelných metodách.</span><span class="sxs-lookup"><span data-stu-id="a3630-107">Allow attributes to be applied to lambdas (and anonymous methods) and to lambda / anonymous method parameters, as they can be on regular methods.</span></span>

## <a name="motivation"></a><span data-ttu-id="a3630-108">Motivační</span><span class="sxs-lookup"><span data-stu-id="a3630-108">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="a3630-109">Dvě primární motivace:</span><span class="sxs-lookup"><span data-stu-id="a3630-109">Two primary motivations:</span></span>

1. <span data-ttu-id="a3630-110">K poskytnutí metadat pro analyzátory v době kompilace.</span><span class="sxs-lookup"><span data-stu-id="a3630-110">To provide metadata visible to analyzers at compile-time.</span></span>
2. <span data-ttu-id="a3630-111">Pro zajištění viditelnosti metadat pro reflexi a nástroje za běhu.</span><span class="sxs-lookup"><span data-stu-id="a3630-111">To provide metadata visible to reflection and tooling at run-time.</span></span>

<span data-ttu-id="a3630-112">Jako příklad (1): pro kód citlivý na výkon je užitečné mít v případě, že je možné mít k dispozici analyzátor, který je v případě, že jsou uzavírány a Delegáti přiděleni pro výrazy lambda, které se blíží stavu.</span><span class="sxs-lookup"><span data-stu-id="a3630-112">As an example of (1): For performance-sensitive code, it is helpful to be able to have an analyzer that flags when closures and delegates are being allocated for lambdas that close over state.</span></span>  <span data-ttu-id="a3630-113">Vývojáři takového kódu se často přestanou přecházet ze svého způsobu, aby se zabránilo zachycení jakéhokoli stavu, takže kompilátor může vygenerovat statickou metodu a pro tuto metodu delegáta s možností ukládání do mezipaměti, nebo vývojář zajistí, že jediný stav, který je uzavřený, je `this`, a to tak, aby kompilátor nemusel přidělit objekt ukončení.</span><span class="sxs-lookup"><span data-stu-id="a3630-113">Often a developer of such code will go out of his or her way to avoid capturing any state, so that the compiler can generate a static method and a cacheable delegate for the method, or the developer will ensure that the only state being closed over is `this`, allowing the compiler at least to avoid allocating a closure object.</span></span>  <span data-ttu-id="a3630-114">Ale bez podpory jazyků pro omezení toho, co by mohlo být zachyceno, je příliš snadné omylem blízko stavu.</span><span class="sxs-lookup"><span data-stu-id="a3630-114">But, without language support for limiting what may be captured, it is all too easy to accidentally close over state.</span></span>  <span data-ttu-id="a3630-115">To je užitečné, pokud vývojář může opatřit výrazy lambda s atributy, aby označovali, jaký stav je dovoleno zavřít, například:</span><span class="sxs-lookup"><span data-stu-id="a3630-115">It would be valuable if a developer could annotate lambdas with attributes to indicate what state they're allowed to close over, for example:</span></span>

```csharp
[CaptureNone] // can't close over any instance state
[CaptureThis] // can only capture `this` and no other instance state
[CaptureAny] // can close over any instance state
```

<span data-ttu-id="a3630-116">Analyzátor se pak může zapsat na příznak při nesprávném zachycení stavu, například:</span><span class="sxs-lookup"><span data-stu-id="a3630-116">Then an analyzer can be written to flag when state is captured incorrectly, for example:</span></span>

```csharp
var results = collection.Select([CaptureNone](i) => Process(item)); // Analyzer error: [CaptureNone] lambdas captures `this`
...
private U Process(T item) { ... }
```

## <a name="detailed-design"></a><span data-ttu-id="a3630-117">Podrobný návrh</span><span class="sxs-lookup"><span data-stu-id="a3630-117">Detailed design</span></span>
[design]: #detailed-design

- <span data-ttu-id="a3630-118">Pomocí stejné syntaxe atributu jako u běžných metod lze atributy použít na začátku lambda nebo anonymní metody, například:</span><span class="sxs-lookup"><span data-stu-id="a3630-118">Using the same attribute syntax as on normal methods, attributes may be applied at the beginning of a lambda or anonymous method, for example:</span></span>

```csharp
[SomeAttribute(...)] () => { ... }
[SomeAttribute(...)] delegate (int i) { ... }
```

- <span data-ttu-id="a3630-119">Aby nedocházelo k nejednoznačnosti, pokud se atribut vztahuje na metodu lambda nebo na jeden z argumentů, atributy mohou být použity pouze v případě, že se Parens používají kolem argumentů, například:</span><span class="sxs-lookup"><span data-stu-id="a3630-119">To avoid ambiguity as to whether an attribute applies to the lambda method or to one of the arguments, attributes may only be used when parens are used around any arguments, for example:</span></span>

```csharp
[SomeAttribute] i => { ... } // ERROR
[SomeAttribute] (i) => { ... } // Ok
[SomeAttribute] (int i) => { ... } // Ok
```

- <span data-ttu-id="a3630-120">V případě anonymních metod nejsou Parens potřeba pro použití atributu pro metodu před klíčovým slovem `delegate`, například:</span><span class="sxs-lookup"><span data-stu-id="a3630-120">With anonymous methods, parens are not needed in order to apply an attribute to the method before the `delegate` keyword, for example:</span></span>

```csharp
[SomeAttribute] delegate { ... } // Ok
[SomeAttribute] delegate (int i) => { ... } // Ok
```

- <span data-ttu-id="a3630-121">Lze použít více atributů, a to buď prostřednictvím standardní syntaxe oddělené čárkami, nebo prostřednictvím syntaxe úplného atributu, například:</span><span class="sxs-lookup"><span data-stu-id="a3630-121">Multiple attributes may be applied, either via standard comma-delimited syntax or via full-attribute syntax, for example:</span></span>

```csharp
[FirstAttribute, SecondAttribute] (i) => { ... } // Ok
[FirstAttribute] [SecondAttribute] (i) => { .... } // Ok
```

- <span data-ttu-id="a3630-122">Atributy mohou být použity pro parametry anonymní metody nebo lambda, ale pouze v případě, že se Parens používají kolem argumentů, například:</span><span class="sxs-lookup"><span data-stu-id="a3630-122">Attributes may be applied to the parameters to an anonymous method or lambda, but only when parens are used around any arguments, for example:</span></span>

```csharp
[SomeAttribute] i => { ... } // ERROR
([SomeAttribute] i) => { .... } // Ok
([SomeAttribute] int i) => { ... } // Ok
([SomeAttribute] i, [SomeOtherAttribute] j) => { ... } // Ok
```

- <span data-ttu-id="a3630-123">Pro parametry anonymní metody nebo výrazu lambda lze použít více atributů, a to buď pomocí syntaxe oddělených čárkami, nebo úplných atributů, například:</span><span class="sxs-lookup"><span data-stu-id="a3630-123">Multiple attributes may be applied to the parameters of an anonymous method or lambda, using either the comma-delimited or full-attribute syntax, for example:</span></span>

```csharp
([FirstAttribute, SecondAttribute] i) => { ... } // Ok
([FirstAttribute] [SecondAttribute] i) => { ... } // Ok
```

- <span data-ttu-id="a3630-124">pro výrazy lambda lze také použít cílené atributy `return`, například:</span><span class="sxs-lookup"><span data-stu-id="a3630-124">`return`-targeted attributes may also be used on lambdas, for example:</span></span>

```csharp
([return: SomeAttribute] (i) => { ... }) // Ok
```

- <span data-ttu-id="a3630-125">Kompilátor výstupuje atributy na vygenerovanou metodu a argumenty do těchto metod, jako by to bylo pro jakoukoliv jinou metodu.</span><span class="sxs-lookup"><span data-stu-id="a3630-125">The compiler outputs the attributes onto the generated method and arguments to those methods as it would for any other method.</span></span>

## <a name="drawbacks"></a><span data-ttu-id="a3630-126">Nevýhody</span><span class="sxs-lookup"><span data-stu-id="a3630-126">Drawbacks</span></span>
[drawbacks]: #drawbacks

<span data-ttu-id="a3630-127">neuvedeno</span><span class="sxs-lookup"><span data-stu-id="a3630-127">n/a</span></span>

## <a name="alternatives"></a><span data-ttu-id="a3630-128">Alternativy</span><span class="sxs-lookup"><span data-stu-id="a3630-128">Alternatives</span></span>
[alternatives]: #alternatives

<span data-ttu-id="a3630-129">neuvedeno</span><span class="sxs-lookup"><span data-stu-id="a3630-129">n/a</span></span>

## <a name="unresolved-questions"></a><span data-ttu-id="a3630-130">Nevyřešené dotazy</span><span class="sxs-lookup"><span data-stu-id="a3630-130">Unresolved questions</span></span>
[unresolved]: #unresolved-questions

<span data-ttu-id="a3630-131">neuvedeno</span><span class="sxs-lookup"><span data-stu-id="a3630-131">n/a</span></span>

## <a name="design-meetings"></a><span data-ttu-id="a3630-132">Schůzky pro návrh</span><span class="sxs-lookup"><span data-stu-id="a3630-132">Design meetings</span></span>

<span data-ttu-id="a3630-133">neuvedeno</span><span class="sxs-lookup"><span data-stu-id="a3630-133">n/a</span></span>