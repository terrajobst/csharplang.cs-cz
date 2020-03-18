---
ms.openlocfilehash: 52b43abd2d8fb56088a68c7169289a63c43ce96f
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484487"
---
# <a name="suppress-emitting-of-localsinit-flag"></a><span data-ttu-id="0440c-101">Potlačit vygenerování příznaku `localsinit`</span><span class="sxs-lookup"><span data-stu-id="0440c-101">Suppress emitting of `localsinit` flag.</span></span>

* <span data-ttu-id="0440c-102">Navrženo [x]</span><span class="sxs-lookup"><span data-stu-id="0440c-102">[x] Proposed</span></span>
* <span data-ttu-id="0440c-103">[] Prototyp: Nezahájeno</span><span class="sxs-lookup"><span data-stu-id="0440c-103">[ ] Prototype: Not Started</span></span>
* <span data-ttu-id="0440c-104">[] Implementace: Nezahájeno</span><span class="sxs-lookup"><span data-stu-id="0440c-104">[ ] Implementation: Not Started</span></span>
* <span data-ttu-id="0440c-105">[] Specifikace: Nezahájeno</span><span class="sxs-lookup"><span data-stu-id="0440c-105">[ ] Specification: Not Started</span></span>

## <a name="summary"></a><span data-ttu-id="0440c-106">Souhrn</span><span class="sxs-lookup"><span data-stu-id="0440c-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="0440c-107">Umožňuje potlačit generování příznaku `localsinit` prostřednictvím atributu `SkipLocalsInitAttribute`.</span><span class="sxs-lookup"><span data-stu-id="0440c-107">Allow suppressing emit of `localsinit` flag via `SkipLocalsInitAttribute` attribute.</span></span> 

## <a name="motivation"></a><span data-ttu-id="0440c-108">Motivační</span><span class="sxs-lookup"><span data-stu-id="0440c-108">Motivation</span></span>
[motivation]: #motivation


### <a name="background"></a><span data-ttu-id="0440c-109">Pozadí</span><span class="sxs-lookup"><span data-stu-id="0440c-109">Background</span></span>
<span data-ttu-id="0440c-110">Podle specifikace CLR nejsou místní proměnné, které neobsahují odkazy, inicializovány na konkrétní hodnotu pomocí virtuálního počítače/JIT.</span><span class="sxs-lookup"><span data-stu-id="0440c-110">Per CLR spec local variables that do not contain references are not initialized to a particular value by the VM/JIT.</span></span> <span data-ttu-id="0440c-111">Čtení z takových proměnných bez inicializace je typově bezpečné, ale v opačném případě je chování nedefinované a specifické pro implementaci.</span><span class="sxs-lookup"><span data-stu-id="0440c-111">Reading from such variables without initialization is type-safe, but otherwise the behavior is undefined and implementation specific.</span></span> <span data-ttu-id="0440c-112">Obvykle neinicializovaná národní prostředí obsahují jakékoli hodnoty, které byly ponechány v paměti, která je nyní obsazena rámcem zásobníku.</span><span class="sxs-lookup"><span data-stu-id="0440c-112">Typically uninitialized locals contain whatever values were left in the memory that is now occupied by the stack frame.</span></span> <span data-ttu-id="0440c-113">To může vést k nedeterministickému chování a těžko reprodukování chyb.</span><span class="sxs-lookup"><span data-stu-id="0440c-113">That could lead to nondeterministic behavior and hard to reproduce bugs.</span></span> 

<span data-ttu-id="0440c-114">Existují dva způsoby přiřazení místní proměnné:</span><span class="sxs-lookup"><span data-stu-id="0440c-114">There are two ways to "assign" a local variable:</span></span> 
- <span data-ttu-id="0440c-115">uložením hodnoty nebo</span><span class="sxs-lookup"><span data-stu-id="0440c-115">by storing a value or</span></span> 
- <span data-ttu-id="0440c-116">zadáním příznaku `localsinit`, který vynutí, aby se všechny položky, které jsou přidělené, vynutily na základě nulové hodnoty paměti, která je nastavená na nulu: to zahrnuje místní proměnné `stackalloc` a</span><span class="sxs-lookup"><span data-stu-id="0440c-116">by specifying `localsinit` flag which forces everything that is allocated form the local memory pool to be zero-initialized NOTE: this includes both local variables and `stackalloc` data.</span></span>    

<span data-ttu-id="0440c-117">Použití neinicializovaných dat se nedoporučuje a v ověřitelném kódu není povoleno.</span><span class="sxs-lookup"><span data-stu-id="0440c-117">Use of uninitialized data is discouraged and is not allowed in verifiable code.</span></span> <span data-ttu-id="0440c-118">I když může být možné prokázat, že se jedná o prostředky analýzy toků, je povolený, aby ověřovací algoritmus byl konzervativní, a jednoduše vyžadoval, aby byl nastaven `localsinit`.</span><span class="sxs-lookup"><span data-stu-id="0440c-118">While it might be possible to prove that by the means of flow analysis, it is permitted for the verification algorithm to be conservative and simply require that `localsinit` is set.</span></span>

<span data-ttu-id="0440c-119">C# Historicky vygeneruje `localsinit` příznak u všech metod, které deklaruje národní prostředí.</span><span class="sxs-lookup"><span data-stu-id="0440c-119">Historically C# compiler emits `localsinit` flag on all methods that declare locals.</span></span>

<span data-ttu-id="0440c-120">Při C# použití analýzy s určitou možností přiřazení, která je přísnější než specifikace CLR (C# musí také zvážit rozsah místních hodnot), není bezpodmínečně zaručeno, aby výsledný kód byl formálně ověřitelný:</span><span class="sxs-lookup"><span data-stu-id="0440c-120">While C# employs definite-assignment analysis which is more strict than what CLR spec would require (C# also needs to consider scoping of locals), it is not strictly guaranteed that the resulting code would be formally verifiable:</span></span>
- <span data-ttu-id="0440c-121">CLR a C# pravidla nemusí souhlasit s tím, zda je jako argument předávána místní jako `out` argumentu `use`.</span><span class="sxs-lookup"><span data-stu-id="0440c-121">CLR and C# rules may not agree on whether passing a local as `out` argument is a `use`.</span></span>
- <span data-ttu-id="0440c-122">CLR a C# pravidla nesmí souhlasit s zpracováním podmíněných větví, pokud jsou podmínky známy (konstantní šíření).</span><span class="sxs-lookup"><span data-stu-id="0440c-122">CLR and C# rules may not agree on treatment of conditional branches when conditions are known (constant propagation).</span></span>
- <span data-ttu-id="0440c-123">Modul CLR může také jednoduše vyžadovat `localinits`, protože to je povoleno.</span><span class="sxs-lookup"><span data-stu-id="0440c-123">CLR could as well simply require `localinits`, since that is permitted.</span></span>  

### <a name="problem"></a><span data-ttu-id="0440c-124">Problém</span><span class="sxs-lookup"><span data-stu-id="0440c-124">Problem</span></span>
<span data-ttu-id="0440c-125">V aplikaci s vysokým výkonem může být možné poznamenat náklady na vynucenou nulovou inicializaci.</span><span class="sxs-lookup"><span data-stu-id="0440c-125">In high-performance application the cost of forced zero-initialization could be noticeable.</span></span> <span data-ttu-id="0440c-126">Je obzvláště patrné, když se používá `stackalloc`.</span><span class="sxs-lookup"><span data-stu-id="0440c-126">It is particularly noticeable when `stackalloc` is used.</span></span>

<span data-ttu-id="0440c-127">V některých případech může kompilátor JIT Elide počáteční nulovou inicializaci jednotlivých národních prostředí, když je taková inicializace "usmrcena" při následném přiřazení.</span><span class="sxs-lookup"><span data-stu-id="0440c-127">In some cases JIT can elide initial zero-initialization of individual locals when such initialization is "killed" by subsequent assignments.</span></span> <span data-ttu-id="0440c-128">Ne všechny nepovolují kompilátory JIT a taková optimalizace mají omezení.</span><span class="sxs-lookup"><span data-stu-id="0440c-128">Not all JITs do this and such optimization has limits.</span></span> <span data-ttu-id="0440c-129">Neposkytuje vám `stackalloc`.</span><span class="sxs-lookup"><span data-stu-id="0440c-129">It does not help with `stackalloc`.</span></span>

<span data-ttu-id="0440c-130">K ilustraci, že je problém skutečný – je známá chyba, pokud metoda, která neobsahuje žádné `IL` místní, nemá příznak `localsinit`.</span><span class="sxs-lookup"><span data-stu-id="0440c-130">To illustrate that the problem is real - there is a known bug where a method not containing any `IL` locals would not have `localsinit` flag.</span></span> <span data-ttu-id="0440c-131">Tato chyba je již zneužita uživateli tím, že do takových metod zapisuje `stackalloc` do takových metod – úmyslně se vyhnout nákladům na inicializaci.</span><span class="sxs-lookup"><span data-stu-id="0440c-131">The bug is already being exploited by users by putting `stackalloc` into such methods - intentionally to avoid initialization costs.</span></span> <span data-ttu-id="0440c-132">To je navzdory tomu, že absence `IL`ch lokálních hodnot je nestabilní metrika a může se lišit v závislosti na změnách v strategii CodeGen.</span><span class="sxs-lookup"><span data-stu-id="0440c-132">That is despite the fact that absence of `IL` locals is an unstable metric and may vary depending on changes in codegen strategy.</span></span> <span data-ttu-id="0440c-133">Tato chyba by měla být opravena a uživatelé by měli získat podrobnější a spolehlivý způsob, jak příznak potlačit.</span><span class="sxs-lookup"><span data-stu-id="0440c-133">The bug should be fixed and users should get a more documented and reliable way of suppressing the flag.</span></span> 

## <a name="detailed-design"></a><span data-ttu-id="0440c-134">Podrobný návrh</span><span class="sxs-lookup"><span data-stu-id="0440c-134">Detailed design</span></span>

<span data-ttu-id="0440c-135">Umožňuje zadat `System.Runtime.CompilerServices.SkipLocalsInitAttribute` jako způsob, jak říct kompilátoru, aby negeneroval `localsinit` příznak.</span><span class="sxs-lookup"><span data-stu-id="0440c-135">Allow specifying `System.Runtime.CompilerServices.SkipLocalsInitAttribute` as a way to tell the compiler to not emit `localsinit` flag.</span></span>
 
<span data-ttu-id="0440c-136">Konečný výsledek této akce bude, že místní hodnoty nemusí být inicializovány kompilátorem JIT, což je ve většině případů nepozorovatelné C#.</span><span class="sxs-lookup"><span data-stu-id="0440c-136">The end result of this will be that the locals may not be zero-initialized by the JIT, which is in most cases unobservable in C#.</span></span>  
<span data-ttu-id="0440c-137">Kromě toho, že `stackalloc` data nebudou inicializována nulou.</span><span class="sxs-lookup"><span data-stu-id="0440c-137">In addition to that `stackalloc` data will not be zero-initialized.</span></span> <span data-ttu-id="0440c-138">To je jednoznačně pozorovatelný, ale také je to nejvíce motivující scénář.</span><span class="sxs-lookup"><span data-stu-id="0440c-138">That is definitely observable, but also is the most motivating scenario.</span></span>

<span data-ttu-id="0440c-139">Povolené a rozpoznané cíle atributů jsou: `Method`, `Property`, `Module`, `Class`, `Struct`, `Interface`, `Constructor`.</span><span class="sxs-lookup"><span data-stu-id="0440c-139">Permitted and recognized attribute targets are: `Method`, `Property`, `Module`, `Class`, `Struct`, `Interface`, `Constructor`.</span></span> <span data-ttu-id="0440c-140">Nicméně kompilátor nebude vyžadovat, aby byl tento atribut definován se uvedenými cíli, ani bude mít na starosti sestavení, které je atribut definován.</span><span class="sxs-lookup"><span data-stu-id="0440c-140">However compiler will not require that attribute is defined with the listed targets nor it will care in which assembly the attribute is defined.</span></span> 

<span data-ttu-id="0440c-141">Když je atribut zadán v kontejneru (`class`, `module`, obsahující metodu pro vnořenou metodu,...), příznak ovlivňuje všechny metody obsažené v kontejneru.</span><span class="sxs-lookup"><span data-stu-id="0440c-141">When attribute is specified on a container (`class`, `module`, containing method for a nested method, ...), the flag affects all methods contained within the container.</span></span>

<span data-ttu-id="0440c-142">Syntetizované metody dědí příznak z logického kontejneru nebo vlastníka.</span><span class="sxs-lookup"><span data-stu-id="0440c-142">Synthesized methods "inherit" the flag from the logical container/owner.</span></span> 

<span data-ttu-id="0440c-143">Příznak ovlivňuje pouze strategii CodeGen pro skutečné tělo metody.</span><span class="sxs-lookup"><span data-stu-id="0440c-143">The flag affects only codegen strategy for actual method bodies.</span></span> <span data-ttu-id="0440c-144">t.</span><span class="sxs-lookup"><span data-stu-id="0440c-144">I.E.</span></span> <span data-ttu-id="0440c-145">příznak nemá žádný vliv na abstraktní metody a není šířen na přepsání/implementaci metod.</span><span class="sxs-lookup"><span data-stu-id="0440c-145">the flag has no effect on abstract methods and is not propagated to overriding/implementing methods.</span></span>

<span data-ttu-id="0440c-146">Toto je explicitně **_funkce kompilátoru_** , **_nikoli funkce jazyka_** .</span><span class="sxs-lookup"><span data-stu-id="0440c-146">This is explicitly a **_compiler feature_** and **_not a language feature_**.</span></span>  
<span data-ttu-id="0440c-147">Podobně jako u přepínačů příkazového řádku kompilátoru ovládací prvky funkce řídí podrobnosti implementace konkrétní strategie CODEGEN a nemusí být požadovány C# specifikací.</span><span class="sxs-lookup"><span data-stu-id="0440c-147">Similarly to compiler command line switches the feature controls implementation details of a particular codegen strategy and does not need to be required by the C# spec.</span></span>

## <a name="drawbacks"></a><span data-ttu-id="0440c-148">Nevýhody</span><span class="sxs-lookup"><span data-stu-id="0440c-148">Drawbacks</span></span>
[drawbacks]: #drawbacks

- <span data-ttu-id="0440c-149">Staré/jiné kompilátory nesmí respektovat atribut.</span><span class="sxs-lookup"><span data-stu-id="0440c-149">Old/other compilers may not honor the attribute.</span></span>
<span data-ttu-id="0440c-150">Ignorování atributu je kompatibilní s chováním.</span><span class="sxs-lookup"><span data-stu-id="0440c-150">Ignoring the attribute is compatible behavior.</span></span> <span data-ttu-id="0440c-151">Výsledkem může být jen mírné dosažení výkonu.</span><span class="sxs-lookup"><span data-stu-id="0440c-151">Only may result in a slight perf hit.</span></span>

- <span data-ttu-id="0440c-152">Kód bez příznaku `localinits` může aktivovat selhání ověřování.</span><span class="sxs-lookup"><span data-stu-id="0440c-152">The code without `localinits` flag may trigger verification failures.</span></span>
<span data-ttu-id="0440c-153">Uživatelé, kteří žádají o tuto funkci, jsou obecně nedotčeny možností ověření.</span><span class="sxs-lookup"><span data-stu-id="0440c-153">Users that ask for this feature are generally unconcerned with verifiability.</span></span> 
 
- <span data-ttu-id="0440c-154">Použití atributu na vyšších úrovních, než je individuální metoda, má nelokální efekt, který je možné pozorovat při použití `stackalloc`.</span><span class="sxs-lookup"><span data-stu-id="0440c-154">Applying the attribute at higher levels than an individual method has nonlocal effect, which is observable when `stackalloc` is used.</span></span> <span data-ttu-id="0440c-155">Tento scénář je ale nejvíce požadovaný.</span><span class="sxs-lookup"><span data-stu-id="0440c-155">Yet, this is the most requested scenario.</span></span>

## <a name="alternatives"></a><span data-ttu-id="0440c-156">Alternativy</span><span class="sxs-lookup"><span data-stu-id="0440c-156">Alternatives</span></span>
[alternatives]: #alternatives

- <span data-ttu-id="0440c-157">vynechání příznaku `localinits`, pokud je metoda deklarovaná v kontextu `unsafe`.</span><span class="sxs-lookup"><span data-stu-id="0440c-157">omit `localinits` flag when method is declared in `unsafe` context.</span></span> <span data-ttu-id="0440c-158">To může způsobit, že dojde ke změně tichého a nebezpečného chování z deterministického na nedeterministické v případě `stackalloc`.</span><span class="sxs-lookup"><span data-stu-id="0440c-158">That could cause silent and dangerous behavior change from deterministic to nondeterministic in a case of `stackalloc`.</span></span>

- <span data-ttu-id="0440c-159">vždy vynechejte příznak `localinits`.</span><span class="sxs-lookup"><span data-stu-id="0440c-159">omit `localinits` flag always.</span></span>
<span data-ttu-id="0440c-160">Ještě horší než výše.</span><span class="sxs-lookup"><span data-stu-id="0440c-160">Even worse than above.</span></span>

- <span data-ttu-id="0440c-161">vynechejte příznak `localinits`, pokud se v těle metody nepoužívá `stackalloc`.</span><span class="sxs-lookup"><span data-stu-id="0440c-161">omit `localinits` flag unless `stackalloc` is used in the method body.</span></span>
<span data-ttu-id="0440c-162">Neřeší nejvíc požadovaný scénář a může neověřit kód bez možnosti vrácení zpět.</span><span class="sxs-lookup"><span data-stu-id="0440c-162">Does not address the most requested scenario and may turn code unverifiable with no option to revert that back.</span></span>

## <a name="unresolved-questions"></a><span data-ttu-id="0440c-163">Nevyřešené dotazy</span><span class="sxs-lookup"><span data-stu-id="0440c-163">Unresolved questions</span></span>
[unresolved]: #unresolved-questions

- <span data-ttu-id="0440c-164">Měl by se tento atribut ve skutečnosti emitovat do metadat?</span><span class="sxs-lookup"><span data-stu-id="0440c-164">Should the attribute be actually emitted to metadata?</span></span> 

## <a name="design-meetings"></a><span data-ttu-id="0440c-165">Schůzky pro návrh</span><span class="sxs-lookup"><span data-stu-id="0440c-165">Design meetings</span></span>

<span data-ttu-id="0440c-166">Žádná ještě není.</span><span class="sxs-lookup"><span data-stu-id="0440c-166">None yet.</span></span> 