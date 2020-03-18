---
ms.openlocfilehash: ecdad8c863d0695bc901e4d96d9ca3decbc248eb
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484613"
---
# <a name="nullable-reference-types-in-c"></a><span data-ttu-id="957c2-101">Typy odkazů s možnou hodnotou null vC#</span><span class="sxs-lookup"><span data-stu-id="957c2-101">Nullable reference types in C#</span></span> #

<span data-ttu-id="957c2-102">Cílem této funkce je:</span><span class="sxs-lookup"><span data-stu-id="957c2-102">The goal of this feature is to:</span></span>

* <span data-ttu-id="957c2-103">Umožňuje vývojářům vyjádřit, zda je proměnná, parametr nebo výsledek odkazového typu na hodnotu null.</span><span class="sxs-lookup"><span data-stu-id="957c2-103">Allow developers to express whether a variable, parameter or result of a reference type is intended to be null or not.</span></span>
* <span data-ttu-id="957c2-104">Poskytněte upozornění, když se tyto proměnné, parametry a výsledky nepoužijí v souladu s tímto záměrem.</span><span class="sxs-lookup"><span data-stu-id="957c2-104">Provide warnings when such variables, parameters and results are not used according to that intent.</span></span>

## <a name="expression-of-intent"></a><span data-ttu-id="957c2-105">Výraz záměru</span><span class="sxs-lookup"><span data-stu-id="957c2-105">Expression of intent</span></span>

<span data-ttu-id="957c2-106">Jazyk již obsahuje syntaxi `T?` pro typy hodnot.</span><span class="sxs-lookup"><span data-stu-id="957c2-106">The language already contains the `T?` syntax for value types.</span></span> <span data-ttu-id="957c2-107">Tuto syntaxi je jednoduché roztáhnout na odkazové typy.</span><span class="sxs-lookup"><span data-stu-id="957c2-107">It is straightforward to extend this syntax to reference types.</span></span>

<span data-ttu-id="957c2-108">Předpokládá se, že záměr takového ne`T`ho odkazového typu je, že by měl mít hodnotu, která není null.</span><span class="sxs-lookup"><span data-stu-id="957c2-108">It is assumed that the intent of an unadorned reference type `T` is for it to be non-null.</span></span>

## <a name="checking-of-nullable-references"></a><span data-ttu-id="957c2-109">Kontrola odkazů s možnou hodnotou null</span><span class="sxs-lookup"><span data-stu-id="957c2-109">Checking of nullable references</span></span>

<span data-ttu-id="957c2-110">Analýza toku sleduje proměnné odkazů s možnou hodnotou null.</span><span class="sxs-lookup"><span data-stu-id="957c2-110">A flow analysis tracks nullable reference variables.</span></span> <span data-ttu-id="957c2-111">Pokud se analýza domnívá, že by neměla mít hodnotu null (například po kontrole nebo přiřazení), jejich hodnota se považuje za odkaz, který není null.</span><span class="sxs-lookup"><span data-stu-id="957c2-111">Where the analysis deems that they would not be null (e.g. after a check or an assignment), their value will be considered a non-null reference.</span></span>

<span data-ttu-id="957c2-112">Odkaz s možnou hodnotou null může být také explicitně považován za nenulový s příponou `x!` operátor (operátor "Dammit"), pokud analýza toku nemůže vytvořit situaci, která není null, na kterou vývojář ví.</span><span class="sxs-lookup"><span data-stu-id="957c2-112">A nullable reference can also explicitly be treated as non-null with the postfix `x!` operator (the "dammit" operator), for when flow analysis cannot establish a non-null situation that the developer knows is there.</span></span>

<span data-ttu-id="957c2-113">V opačném případě se zobrazí upozornění, pokud je odkaz s možnou hodnotou null nebo převeden na typ, který není null.</span><span class="sxs-lookup"><span data-stu-id="957c2-113">Otherwise, a warning is given if a nullable reference is dereferenced, or is converted to a non-null type.</span></span>

<span data-ttu-id="957c2-114">Při převodu z `S[]` na `T?[]` a z `S?[]` na `T[]`se zobrazí upozornění.</span><span class="sxs-lookup"><span data-stu-id="957c2-114">A warning is given when converting from `S[]` to `T?[]` and from `S?[]` to `T[]`.</span></span>

<span data-ttu-id="957c2-115">Při převodu z `C<S>` na `C<T?>` se zobrazí upozornění s výjimkou toho, že parametr typu je kovariantní (`out`), a při převodu z `C<S?>` na `C<T>` s výjimkou toho, že parametr typu je kontravariantní (`in`).</span><span class="sxs-lookup"><span data-stu-id="957c2-115">A warning is given when converting from `C<S>` to `C<T?>` except when the type parameter is covariant (`out`), and when converting from `C<S?>` to `C<T>` except when the type parameter is contravariant (`in`).</span></span>

<span data-ttu-id="957c2-116">Pokud parametr typu má omezení bez hodnoty null, je dána výstraha na `C<T?>`.</span><span class="sxs-lookup"><span data-stu-id="957c2-116">A warning is given on `C<T?>` if the type parameter has non-null constraints.</span></span> 

## <a name="checking-of-non-null-references"></a><span data-ttu-id="957c2-117">Kontrola odkazů, které nejsou null</span><span class="sxs-lookup"><span data-stu-id="957c2-117">Checking of non-null references</span></span>

<span data-ttu-id="957c2-118">Je zadáno upozornění, pokud je literál null přiřazen proměnné, která není null nebo předána jako parametr, který není null.</span><span class="sxs-lookup"><span data-stu-id="957c2-118">A warning is given if a null literal is assigned to a non-null variable or passed as a non-null parameter.</span></span>

<span data-ttu-id="957c2-119">V případě, že konstruktor explicitně neinicializuje odkazová pole, která nejsou null, je také poskytnuto upozornění.</span><span class="sxs-lookup"><span data-stu-id="957c2-119">A warning is also given if a constructor does not explicitly initialize non-null reference fields.</span></span>

<span data-ttu-id="957c2-120">Nelze dostatečně sledovat, zda jsou inicializovány všechny prvky pole nenulových odkazů.</span><span class="sxs-lookup"><span data-stu-id="957c2-120">We cannot adequately track that all elements of an array of non-null references are initialized.</span></span> <span data-ttu-id="957c2-121">Nicméně jsme mohli vydat upozornění, pokud není přiřazen žádný prvek nově vytvořeného pole před čtením nebo předáním pole.</span><span class="sxs-lookup"><span data-stu-id="957c2-121">However, we could issue a warning if no element of a newly created array is assigned to before the array is read from or passed on.</span></span> <span data-ttu-id="957c2-122">To může zvládnout běžný případ bez vysokého šumu.</span><span class="sxs-lookup"><span data-stu-id="957c2-122">That might handle the common case without being too noisy.</span></span>

<span data-ttu-id="957c2-123">Musíme rozhodnout, jestli `default(T)` vygeneruje upozornění, nebo se prostě považuje za typ `T?`.</span><span class="sxs-lookup"><span data-stu-id="957c2-123">We need to decide whether `default(T)` generates a warning, or is simply treated as being of the type `T?`.</span></span>

## <a name="metadata-representation"></a><span data-ttu-id="957c2-124">Reprezentace metadat</span><span class="sxs-lookup"><span data-stu-id="957c2-124">Metadata representation</span></span>

<span data-ttu-id="957c2-125">Doplňky s hodnotou null by měly být reprezentovány v metadatech jako atributy.</span><span class="sxs-lookup"><span data-stu-id="957c2-125">Nullability adornments should be represented in metadata as attributes.</span></span> <span data-ttu-id="957c2-126">To znamená, že kompilátory nižší úrovně je budou ignorovat.</span><span class="sxs-lookup"><span data-stu-id="957c2-126">This means that downlevel compilers will ignore them.</span></span>

<span data-ttu-id="957c2-127">Musíme se rozhodnout, jestli jsou zahrnuté jenom anotace s možnou hodnotou null, nebo jestli se v sestavení objevila hodnota, která není null.</span><span class="sxs-lookup"><span data-stu-id="957c2-127">We need to decide if only nullable annotations are included, or there's also some indication of whether non-null was "on" in the assembly.</span></span>

## <a name="generics"></a><span data-ttu-id="957c2-128">Obecné typy</span><span class="sxs-lookup"><span data-stu-id="957c2-128">Generics</span></span>

<span data-ttu-id="957c2-129">Pokud parametr typu `T` má omezení nepovolující hodnotu null, je v rámci svého rozsahu považován za nenulovou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="957c2-129">If a type parameter `T` has non-nullable constraints, it is treated as non-nullable within its scope.</span></span>

<span data-ttu-id="957c2-130">Pokud parametr typu není omezen nebo má pouze omezení s možnou hodnotou null, je tato situace poněkud složitější: to znamená, že odpovídající argument typu může mít *buď* možnou hodnotu null, nebo nemůže mít hodnotu null.</span><span class="sxs-lookup"><span data-stu-id="957c2-130">If a type parameter is unconstrained or has only nullable constraints, the situation is a little more complex: this means that the corresponding type argument could be *either* nullable or non-nullable.</span></span> <span data-ttu-id="957c2-131">V takovém případě je bezpečné v takovém případě zacházet s parametrem *typu jako s* možnou hodnotou null a nepovolenou hodnotou null a poskytuje upozornění, když je porušena.</span><span class="sxs-lookup"><span data-stu-id="957c2-131">The safe thing to do in that situation is to treat the type parameter as *both* nullable and non-nullable, giving warnings when either is violated.</span></span> 

<span data-ttu-id="957c2-132">Je vhodné zvážit, zda by měla být povolena omezení explicitních odkazů s možnou hodnotou null.</span><span class="sxs-lookup"><span data-stu-id="957c2-132">It is worth considering whether explicit nullable reference constraints should be allowed.</span></span> <span data-ttu-id="957c2-133">Upozorňujeme však, že nemůžeme mít *implicitní* typy odkazů s možnou hodnotou null v určitých případech (zděděná omezení).</span><span class="sxs-lookup"><span data-stu-id="957c2-133">Note, however, that we cannot avoid having nullable reference types *implicitly* be constraints in certain cases (inherited constraints).</span></span>

<span data-ttu-id="957c2-134">Omezení `class` má hodnotu jinou než null.</span><span class="sxs-lookup"><span data-stu-id="957c2-134">The `class` constraint is non-null.</span></span> <span data-ttu-id="957c2-135">Můžeme zvážit, zda by mělo být `class?` platné omezení hodnoty null, které označuje "typ odkazu s možnou hodnotou null".</span><span class="sxs-lookup"><span data-stu-id="957c2-135">We can consider whether `class?` should be a valid nullable constraint denoting "nullable reference type".</span></span>

## <a name="type-inference"></a><span data-ttu-id="957c2-136">Odvození typu</span><span class="sxs-lookup"><span data-stu-id="957c2-136">Type inference</span></span>

<span data-ttu-id="957c2-137">Pokud je typ přispívajícího typu odkaz na typ, který je odvozen jako typ odkazu na hodnotu Nullable, výsledný typ by měl mít hodnotu null.</span><span class="sxs-lookup"><span data-stu-id="957c2-137">In type inference, if a contributing type is a nullable reference type, the resulting type should be nullable.</span></span> <span data-ttu-id="957c2-138">Jinými slovy, je rozšířena hodnota null.</span><span class="sxs-lookup"><span data-stu-id="957c2-138">In other words, nullness is propagated.</span></span>

<span data-ttu-id="957c2-139">Měli byste zvážit, zda `null` literál jako podílový výraz by měl přispět k hodnotě null.</span><span class="sxs-lookup"><span data-stu-id="957c2-139">We should consider whether the `null` literal as a participating expression should contribute nullness.</span></span> <span data-ttu-id="957c2-140">Ještě ne: u hodnotových typů vede k chybě, zatímco pro odkazové typy se hodnota null úspěšně převede na jednoduchý typ.</span><span class="sxs-lookup"><span data-stu-id="957c2-140">It doesn't today: for value types it leads to an error, whereas for reference types the null successfully converts to the plain type.</span></span> 

```csharp
string? n = "world";
var x = b ? "Hello" : n; // string?
var y = b ? "Hello" : null; // string? or error
var z = b ? 7 : null; // Error today, could be int?
```

## <a name="breaking-changes"></a><span data-ttu-id="957c2-141">Změny způsobující chyby</span><span class="sxs-lookup"><span data-stu-id="957c2-141">Breaking changes</span></span>

<span data-ttu-id="957c2-142">Upozornění, která nejsou null, jsou zjevnými zásadními změnami stávajícího kódu a měly by být doprovázeny mechanismem pro výslovný souhlas.</span><span class="sxs-lookup"><span data-stu-id="957c2-142">Non-null warnings are an obvious breaking change on existing code, and should be accompanied with an opt-in mechanism.</span></span>

<span data-ttu-id="957c2-143">Méně zjevně, upozornění z typů s možnou hodnotou null (jak je popsáno výše) jsou zásadní změnou stávajícího kódu v určitých situacích, kdy je hodnota null implicitní:</span><span class="sxs-lookup"><span data-stu-id="957c2-143">Less obviously, warnings from nullable types (as described above) are a breaking change on existing code in certain scenarios where the nullability is implicit:</span></span>

* <span data-ttu-id="957c2-144">Parametry neomezeného typu budou považovány za implicitně s možnou hodnotou null, takže je přiřadíte k `object` nebo k přístupu, např. `ToString` budou vracet upozornění.</span><span class="sxs-lookup"><span data-stu-id="957c2-144">Unconstrained type parameters will be treated as implicitly nullable, so assigning them to `object` or accessing e.g. `ToString` will yield warnings.</span></span>
* <span data-ttu-id="957c2-145">Pokud odvození typu odvodí hodnotu null z `null` výrazy, pak existující kód někdy bude vracet hodnotu null, nikoli nenullable typy, což může vést k novým upozorněním.</span><span class="sxs-lookup"><span data-stu-id="957c2-145">if type inference infers nullness from `null` expressions, then existing code will sometimes yield nullable rather than non-nullable types, which can lead to new warnings.</span></span>

<span data-ttu-id="957c2-146">Proto musí být upozornění s možnou hodnotou null také volitelná.</span><span class="sxs-lookup"><span data-stu-id="957c2-146">So nullable warnings also need to be optional</span></span>

<span data-ttu-id="957c2-147">Nakonec přidání poznámek do existujícího rozhraní API bude zásadní změnou u uživatelů, kteří se při upgradu knihovny přihlásili k upozorněním.</span><span class="sxs-lookup"><span data-stu-id="957c2-147">Finally, adding annotations to an existing API will be a breaking change to users who have opted in to warnings, when they upgrade the library.</span></span> <span data-ttu-id="957c2-148">Tato možnost je moc volitelná a může se odhlásit. "Chci opravit chyby, ale nemůžu se pustit na nové poznámky"</span><span class="sxs-lookup"><span data-stu-id="957c2-148">This, too, merits the ability to opt in or out. "I want the bug fixes, but I am not ready to deal with their new annotations"</span></span>

<span data-ttu-id="957c2-149">V souhrnu je potřeba, abyste byli schopni se rozhodnout nebo odsouhlasit:</span><span class="sxs-lookup"><span data-stu-id="957c2-149">In summary, you need to be able to opt in/out of:</span></span>
* <span data-ttu-id="957c2-150">Upozornění s možnou hodnotou null</span><span class="sxs-lookup"><span data-stu-id="957c2-150">Nullable warnings</span></span>
* <span data-ttu-id="957c2-151">Upozornění jiná než null</span><span class="sxs-lookup"><span data-stu-id="957c2-151">Non-null warnings</span></span>
* <span data-ttu-id="957c2-152">Upozornění z poznámek v jiných souborech</span><span class="sxs-lookup"><span data-stu-id="957c2-152">Warnings from annotations in other files</span></span>

<span data-ttu-id="957c2-153">Členitost výslovných odchylek navrhuje model podobný analyzátoru, kde swaths kódu může začínat a odsouhlasit pomocí direktiv pragma a úrovní závažnosti, které uživatel může vybrat.</span><span class="sxs-lookup"><span data-stu-id="957c2-153">The granularity of the opt-in suggests an analyzer-like model, where swaths of code can opt in and out with pragmas and severity levels can be chosen by the user.</span></span> <span data-ttu-id="957c2-154">Kromě toho možnosti pro každou knihovnu ("ignorovat poznámky z JSON.NET, dokud nebudete připraveni na zabývat se), mohou být vyhodnotit v kódu jako atributy.</span><span class="sxs-lookup"><span data-stu-id="957c2-154">Additionally, per-library options ("ignore the annotations from JSON.NET until I'm ready to deal with the fall out") may be expressible in code as attributes.</span></span>

<span data-ttu-id="957c2-155">Pro úspěch a užitečnost této funkce je rozhodující návrh možností přihlášení a přechodů.</span><span class="sxs-lookup"><span data-stu-id="957c2-155">The design of the opt-in/transition experience is crucial to the success and usefulness of this feature.</span></span> <span data-ttu-id="957c2-156">Musíme zajistit, aby:</span><span class="sxs-lookup"><span data-stu-id="957c2-156">We need to make sure that:</span></span>

* <span data-ttu-id="957c2-157">Uživatelé mohou postupně přijmout možnost kontroly hodnoty null, protože chtějí</span><span class="sxs-lookup"><span data-stu-id="957c2-157">Users can adopt nullability checking gradually as they want to</span></span>
* <span data-ttu-id="957c2-158">Autoři knihovny můžou přidávat anotace s hodnotou null bez obav zákazníků.</span><span class="sxs-lookup"><span data-stu-id="957c2-158">Library authors can add nullability annotations without fear of breaking customers</span></span>
* <span data-ttu-id="957c2-159">Navzdory těmto možnostem není smysl "konfigurace Nightmare".</span><span class="sxs-lookup"><span data-stu-id="957c2-159">Despite these, there is not a sense of "configuration nightmare"</span></span>

## <a name="tweaks"></a><span data-ttu-id="957c2-160">Sekce</span><span class="sxs-lookup"><span data-stu-id="957c2-160">Tweaks</span></span>

<span data-ttu-id="957c2-161">Můžeme zvážit, že nepoužíváme `?` anotace na lokálních, ale jenom na základě toho, jestli se používají v souladu s tím, co se jim přiřadí.</span><span class="sxs-lookup"><span data-stu-id="957c2-161">We could consider not using the `?` annotations on locals, but just observing whether they are used in accordance with what gets assigned to them.</span></span> <span data-ttu-id="957c2-162">Tohle nechci; Myslím, že chceme, aby lidé vyjádřili svůj záměr.</span><span class="sxs-lookup"><span data-stu-id="957c2-162">I don't favor this; I think we should uniformly let people express their intent.</span></span>

<span data-ttu-id="957c2-163">V parametrech můžeme považovat za zkrácený `T! x`, který automaticky vygeneruje kontrolu null za běhu.</span><span class="sxs-lookup"><span data-stu-id="957c2-163">We could consider a shorthand `T! x` on parameters, that auto-generates a runtime null check.</span></span>

<span data-ttu-id="957c2-164">Některé vzory na obecných typech, jako je například `FirstOrDefault` nebo `TryGet`, mají mírně divné chování s argumenty typu bez hodnoty null, protože explicitně přinesou výchozí hodnoty v určitých situacích.</span><span class="sxs-lookup"><span data-stu-id="957c2-164">Certain patterns on generic types, such as `FirstOrDefault` or `TryGet`, have slightly weird behavior with non-nullable type arguments, because they explicitly yield default values in certain situations.</span></span> <span data-ttu-id="957c2-165">Můžeme se pokusit nuance systém typů, aby se lépe pokryl.</span><span class="sxs-lookup"><span data-stu-id="957c2-165">We could try to nuance the type system to accommodate these better.</span></span> <span data-ttu-id="957c2-166">Můžeme například `?` u nezarovnaných parametrů typu, i když argument typu již může mít hodnotu null.</span><span class="sxs-lookup"><span data-stu-id="957c2-166">For instance, we could allow `?` on unconstrained type parameters, even though the type argument could already be nullable.</span></span> <span data-ttu-id="957c2-167">Pochybnosti, že stojí za to, a vede k weirdnessům souvisejícím s interakcí s typy *hodnot* s možnou hodnotou null.</span><span class="sxs-lookup"><span data-stu-id="957c2-167">I doubt that it is worth it, and it leads to weirdness related to interaction with nullable *value* types.</span></span> 

## <a name="nullable-value-types"></a><span data-ttu-id="957c2-168">Typy hodnot s povolenou hodnotou Null</span><span class="sxs-lookup"><span data-stu-id="957c2-168">Nullable value types</span></span>

<span data-ttu-id="957c2-169">U typů hodnot s možnou hodnotou null můžeme zvážit přijetí některých z výše uvedených sémantik.</span><span class="sxs-lookup"><span data-stu-id="957c2-169">We could consider adopting some of the above semantics for nullable value types as well.</span></span>

<span data-ttu-id="957c2-170">Již jsme uvedli odvození typu, kde můžeme odvodit `int?` z `(7, null)`namísto pouhého poskytnutí chyby.</span><span class="sxs-lookup"><span data-stu-id="957c2-170">We already mentioned type inference, where we could infer `int?` from `(7, null)`, instead of just giving an error.</span></span>

<span data-ttu-id="957c2-171">Další příležitostí je použít analýzu toku na typy hodnot s možnou hodnotou null.</span><span class="sxs-lookup"><span data-stu-id="957c2-171">Another opportunity is to apply the flow analysis to nullable value types.</span></span> <span data-ttu-id="957c2-172">V případě, že jsou považovány za nenulové, můžeme ve skutečnosti použít jako typ, který nepovoluje hodnotu null, konkrétní způsoby (například přístup ke členům).</span><span class="sxs-lookup"><span data-stu-id="957c2-172">When they are deemed non-null, we could actually allow using as the non-nullable type in certain ways (e.g. member access).</span></span> <span data-ttu-id="957c2-173">Musíme být jenom opatrní, aby pro účely kompatibility s možnou hodnotou, která je *už* v typu hodnot s možnou hodnotou null, mohla stačit akce.</span><span class="sxs-lookup"><span data-stu-id="957c2-173">We just have to be careful that the things that you can *already* do on a nullable value type will be preferred, for back compat reasons.</span></span>
