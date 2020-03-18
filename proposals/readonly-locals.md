---
ms.openlocfilehash: 922353d043653ddb651534a01f3fb98f85cd756e
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484480"
---
# <a name="readonly-locals-and-parameters"></a><span data-ttu-id="fa4e4-101">lokální hodnoty a parametry jen pro čtení</span><span class="sxs-lookup"><span data-stu-id="fa4e4-101">readonly locals and parameters</span></span>

* <span data-ttu-id="fa4e4-102">Navrženo [x]</span><span class="sxs-lookup"><span data-stu-id="fa4e4-102">[x] Proposed</span></span>
* <span data-ttu-id="fa4e4-103">[] Prototyp</span><span class="sxs-lookup"><span data-stu-id="fa4e4-103">[ ] Prototype</span></span>
* <span data-ttu-id="fa4e4-104">[] Implementace</span><span class="sxs-lookup"><span data-stu-id="fa4e4-104">[ ] Implementation</span></span>
* <span data-ttu-id="fa4e4-105">[] Specifikace</span><span class="sxs-lookup"><span data-stu-id="fa4e4-105">[ ] Specification</span></span>

## <a name="summary"></a><span data-ttu-id="fa4e4-106">Souhrn</span><span class="sxs-lookup"><span data-stu-id="fa4e4-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="fa4e4-107">Umožňuje, aby se lokální hodnoty a parametry pomohly označit jako jen pro čtení, aby nedošlo k neomezené mutace těchto národních prostředí a parametrů.</span><span class="sxs-lookup"><span data-stu-id="fa4e4-107">Allow locals and parameters to be annotated as readonly in order to prevent shallow mutation of those locals and parameters.</span></span>

## <a name="motivation"></a><span data-ttu-id="fa4e4-108">Motivační</span><span class="sxs-lookup"><span data-stu-id="fa4e4-108">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="fa4e4-109">V dnešní době lze klíčové slovo `readonly` použít u polí. to má vliv na to, že pole může být zapsáno pouze během konstrukce (statické konstrukce v případě statického pole nebo konstrukce instance v případě pole instance), což pomáhá vývojářům vyhnout se chybám omylem při náhodném přepsání stavu, který by neměl být změněn.</span><span class="sxs-lookup"><span data-stu-id="fa4e4-109">Today, the `readonly` keyword can be applied to fields; this has the effect of ensuring that a field can only be written to during construction (static construction in the case of a static field, or instance construction in the case of an instance field), which helps developers avoid mistakes by accidentally overwriting state which should not be modified.</span></span> <span data-ttu-id="fa4e4-110">Ale pole nejsou jenom vývojáři, aby měli jistotu, že se hodnoty nepoužijí.</span><span class="sxs-lookup"><span data-stu-id="fa4e4-110">But fields aren't the only places developers want to ensure that values aren't mutated.</span></span> <span data-ttu-id="fa4e4-111">Konkrétně je běžné vytvořit místní proměnnou pro uložení dočasného stavu a neúmyslná aktualizace tohoto dočasného stavu může mít za následek chybné výpočty a další takové chyby, zejména v případě, že jsou tato "národní prostředí" zachycena ve výrazech lambda, v tom případě jsou převolána do polí, ale již není dnes označena jako `readonly`.</span><span class="sxs-lookup"><span data-stu-id="fa4e4-111">In particular, it's common to create a local variable to store temporary state, and accidentally updating that temporary state can result in erroneous calculations and other such bugs, especially when such "locals" are captured in lambdas, at which point they are lifted to fields, but there's no way today to mark such lifted fields as `readonly`.</span></span>

## <a name="detailed-design"></a><span data-ttu-id="fa4e4-112">Podrobný návrh</span><span class="sxs-lookup"><span data-stu-id="fa4e4-112">Detailed design</span></span>
[design]: #detailed-design

<span data-ttu-id="fa4e4-113">Národní prostředí bude možné opatřit poznámkami jako `readonly` i v případě, že kompilátor zajistí, že jsou nastaveni pouze v okamžiku deklarace (Některá národní prostředí C# jsou již implicitně určena jen pro čtení, jako je například proměnná iterace ve smyčce foreach nebo použitá proměnná v bloku using, ale v současné době vývojář nemá schopnost označit jiné národní prostředí jako `readonly`).</span><span class="sxs-lookup"><span data-stu-id="fa4e4-113">Locals will be annotatable as `readonly` as well, with the compiler ensuring that they're only set at the time of declaration (certain locals in C# are already implicitly readonly, such as the iteration variable in a 'foreach' loop or the used variable in a 'using' block, but currently a developer has no ability to mark other locals as `readonly`).</span></span> <span data-ttu-id="fa4e4-114">Taková `readonly`ová národní prostředí musí mít inicializátor:</span><span class="sxs-lookup"><span data-stu-id="fa4e4-114">Such `readonly` locals must have an initializer:</span></span>

```csharp
readonly long maxBytesToDelete = (stream.LimitBytes - stream.MaxBytes) / 10;
...
maxBytesToDelete = 0; // Error: can't assign to readonly locals outside of declaration
```

<span data-ttu-id="fa4e4-115">A jako zkrácený pro `readonly var`mohou být použity existující kontextové klíčové slovo `let`, např.</span><span class="sxs-lookup"><span data-stu-id="fa4e4-115">And as shorthand for `readonly var`, the existing contextual keyword `let` may be used, e.g.</span></span>

```csharp
let maxBytesToDelete = (stream.LimitBytes - stream.MaxBytes) / 10;
...
maxBytesToDelete = 0; // Error: can't assign to readonly locals outside of declaration
```

<span data-ttu-id="fa4e4-116">Neexistují žádná zvláštní omezení pro to, co může být inicializátorem, a může to být cokoli, co je aktuálně platné jako inicializátor pro národní prostředí, např.</span><span class="sxs-lookup"><span data-stu-id="fa4e4-116">There are no special constraints for what the initializer can be, and can be anything currently valid as an initializer for locals, e.g.</span></span>

```csharp
readonly T data = arg1 ?? arg2;
```

<span data-ttu-id="fa4e4-117">`readonly` v lokálních hodnotách je obzvláště užitečná při práci s lambda a uzávěry.</span><span class="sxs-lookup"><span data-stu-id="fa4e4-117">`readonly` on locals is particularly valuable when working with lambdas and closures.</span></span> <span data-ttu-id="fa4e4-118">Když anonymní metoda nebo lambda přistupuje k místnímu stavu z ohraničujícího oboru, je tento stav zachycen do uzavření kompilátorem, který je reprezentován "zobrazovací třídou".</span><span class="sxs-lookup"><span data-stu-id="fa4e4-118">When an anonymous method or lambda accesses local state from the enclosing scope, that state is captured into a closure by the compiler, which is represented by a "display class."</span></span>  <span data-ttu-id="fa4e4-119">Každé "místní" zachycené pole je v této třídě, a to proto, že kompilátor generuje toto pole vaším jménem, nemáte žádnou příležitost k tomu, aby ji napsal jako `readonly`, aby nedošlo k chybnému zápisu do "místní" (v uvozovkách, protože to není místní, alespoň ve výsledném jazyce MSIL).</span><span class="sxs-lookup"><span data-stu-id="fa4e4-119">Each "local" that's captured is a field in this class, yet because the compiler is generating this field on your behalf, you have no opportunity to annotate it as `readonly` in order to prevent the lambda from erroneously writing to the "local" (in quotes because it's really not a local, at least not in the resulting MSIL).</span></span> <span data-ttu-id="fa4e4-120">V `readonly` národní prostředí může kompilátor zabránit v zápisu do místní, což je obzvláště důležité ve scénářích, které se týkají multithreadingy, kdy chybný zápis by mohl mít za následek nebezpečnou, ale vzácnou chybu souběžného vyhledávání.</span><span class="sxs-lookup"><span data-stu-id="fa4e4-120">With `readonly` locals, the compiler can prevent the lambda from writing to local, which is particularly valuable in scenarios involving multithreading where an erroneous write could result in a dangerous but rare and hard-to-find concurrency bug.</span></span>

```csharp
readonly long index = ...;
Parallel.ForEach(data, item => {
    T element = item[index];
    index = 0; // Error: can't assign to readonly locals outside of declaration
});
```

<span data-ttu-id="fa4e4-121">V rámci speciální formy místních parametrů budou také anotace `readonly`.</span><span class="sxs-lookup"><span data-stu-id="fa4e4-121">As a special form of local, parameters will also be annotatable as `readonly`.</span></span> <span data-ttu-id="fa4e4-122">To by mělo mít žádný vliv na to, co volající metody může předat parametr (stejně jako není žádné omezení, které hodnoty mohou být uloženy do pole `readonly`), ale stejně jako u jakýchkoli `readonly` místních, kompilátor zakáže zápis kódu do parametru po deklaraci, což znamená, že tělo metody je zakázáno zapisovat do parametru.</span><span class="sxs-lookup"><span data-stu-id="fa4e4-122">This would have no effect on what the caller of the method is able to pass to the parameter (just as there's no constraint on what values may be stored into a `readonly` field), but as with any `readonly` local, the compiler would prohibit code from writing to the parameter after declaration, which means the body of the method is prohibited from writing to the parameter.</span></span>

```csharp
public void Update(readonly int index = 0) // Default values are ok though not required
{
    ...
    index = 0; // Error: can't assign to readonly parameters
    ...
}
```

<span data-ttu-id="fa4e4-123">parametry `readonly` neovlivňují signaturu nebo metadata vypouštěné kompilátorem pro danou metodu a jednoduše ovlivňují, jak kompilátor zpracovává kompilaci těla metody.</span><span class="sxs-lookup"><span data-stu-id="fa4e4-123">`readonly` parameters do not affect the signature/metadata emitted by the compiler for that method, and simply affect how the compiler handles the compilation of the method's body.</span></span> <span data-ttu-id="fa4e4-124">Například základní virtuální metoda může mít parametr `readonly` a tento parametr by mohl zapisovat do přepsání.</span><span class="sxs-lookup"><span data-stu-id="fa4e4-124">Thus, for example, a base virtual method could have a `readonly` parameter, and that parameter could be writable in an override.</span></span>

<span data-ttu-id="fa4e4-125">Stejně jako u polí jsou `readonly` pro lokální hodnoty a parametry omezené, ovlivnění umístění úložiště, ale ne bez jakýchkoli průjezdů, což by mělo vliv na graf objektů.</span><span class="sxs-lookup"><span data-stu-id="fa4e4-125">As with fields, `readonly` for locals and parameters is shallow, affecting the storage location but not transitively affecting the object graph.</span></span> <span data-ttu-id="fa4e4-126">Nicméně stejně jako u polí, volání metody na `readonly` místní nebo parametr struct, ve skutečnosti vytvoří kopii struktury a zavolá metodu na kopii, aby nedošlo k vnitřní mutace `this`.</span><span class="sxs-lookup"><span data-stu-id="fa4e4-126">However, also as with fields, calling a method on a `readonly` local/parameter struct will actually make a copy of the struct and call the method on the copy, in order to avoid internal mutation of `this`.</span></span>

<span data-ttu-id="fa4e4-127">`readonly` lokální hodnoty a parametry nelze předat jako `ref` nebo `out` argumenty, pokud `ref readonly` i/dokud není podporována.</span><span class="sxs-lookup"><span data-stu-id="fa4e4-127">`readonly` locals and parameters can't be passed as `ref` or `out` arguments, unless/until `ref readonly` is also supported.</span></span>

## <a name="alternatives"></a><span data-ttu-id="fa4e4-128">Alternativy</span><span class="sxs-lookup"><span data-stu-id="fa4e4-128">Alternatives</span></span>
[alternatives]: #alternatives

- <span data-ttu-id="fa4e4-129">`val` lze použít jako alternativní zkratku pro `let`.</span><span class="sxs-lookup"><span data-stu-id="fa4e4-129">`val` could be used as an alternative shorthand to `let`.</span></span>

## <a name="unresolved-questions"></a><span data-ttu-id="fa4e4-130">Nevyřešené dotazy</span><span class="sxs-lookup"><span data-stu-id="fa4e4-130">Unresolved questions</span></span>
[unresolved]: #unresolved-questions

- <span data-ttu-id="fa4e4-131">`readonly ref` / `ref readonly` / `readonly ref readonly`: Mám na začátku otázky, jak s tímto návrhem pracovat `ref readonly` jako samostatný.</span><span class="sxs-lookup"><span data-stu-id="fa4e4-131">`readonly ref` / `ref readonly` / `readonly ref readonly`: I've left the question of how to handle `ref readonly` as separate from this proposal.</span></span>
- <span data-ttu-id="fa4e4-132">Tento návrh netýká struktur jen pro čtení nebo neměnných typů.</span><span class="sxs-lookup"><span data-stu-id="fa4e4-132">This proposal does not tackle readonly structs / immutable types.</span></span> <span data-ttu-id="fa4e4-133">To je ponecháno pro samostatný návrh.</span><span class="sxs-lookup"><span data-stu-id="fa4e4-133">That is left for a separate proposal.</span></span>

## <a name="design-meetings"></a><span data-ttu-id="fa4e4-134">Schůzky pro návrh</span><span class="sxs-lookup"><span data-stu-id="fa4e4-134">Design meetings</span></span>

- <span data-ttu-id="fa4e4-135">Stručný popis: 21. ledna 2015 (<https://github.com/dotnet/roslyn/issues/98>)</span><span class="sxs-lookup"><span data-stu-id="fa4e4-135">Briefly discussed on Jan 21, 2015 (<https://github.com/dotnet/roslyn/issues/98>)</span></span>
