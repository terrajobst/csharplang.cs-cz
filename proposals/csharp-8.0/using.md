---
ms.openlocfilehash: 91afbc3e3412049cd183c36c8035f1862c520413
ms.sourcegitcommit: da1180f7eacdd5067b32d291a76e6764159e00fe
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/05/2019
ms.locfileid: "79485054"
---
# <a name="pattern-based-using-and-using-declarations"></a><span data-ttu-id="7c823-101">"založené na vzorcích" a pomocí deklarací "</span><span class="sxs-lookup"><span data-stu-id="7c823-101">"pattern-based using" and "using declarations"</span></span>

## <a name="summary"></a><span data-ttu-id="7c823-102">Souhrn</span><span class="sxs-lookup"><span data-stu-id="7c823-102">Summary</span></span>

<span data-ttu-id="7c823-103">Tento jazyk přidá dvě nové funkce kolem příkazu `using`, aby bylo možné zjednodušit správu prostředků: `using` by měl kromě `IDisposable` a přidání deklarace `using` do tohoto jazyka rozpoznat vzor na jedno použití.</span><span class="sxs-lookup"><span data-stu-id="7c823-103">The language will add two new capabilities around the `using` statement in order to make resource management simpler: `using` should recognize a disposable pattern in addition to `IDisposable` and add a `using` declaration to the language.</span></span>

## <a name="motivation"></a><span data-ttu-id="7c823-104">Motivační</span><span class="sxs-lookup"><span data-stu-id="7c823-104">Motivation</span></span>

<span data-ttu-id="7c823-105">Příkaz `using` je účinným nástrojem pro správu prostředků dnes, ale vyžaduje poměrně bitovou procedury.</span><span class="sxs-lookup"><span data-stu-id="7c823-105">The `using` statement is an effective tool for resource management today but it requires quite a bit of ceremony.</span></span> <span data-ttu-id="7c823-106">Metody, které mají mnoho prostředků ke správě, můžou syntakticky zabřednete dolů pomocí řady příkazů `using`.</span><span class="sxs-lookup"><span data-stu-id="7c823-106">Methods that have a number of resources to manage can get syntactically bogged down with a series of `using` statements.</span></span> <span data-ttu-id="7c823-107">Tato zátěže syntaxe je dostatečná, protože většina zásad stylu kódování má explicitně výjimku kolem složených závorek pro tento scénář.</span><span class="sxs-lookup"><span data-stu-id="7c823-107">This syntax burden is enough that most coding style guidelines explicitly have an exception around braces for this scenario.</span></span> 

<span data-ttu-id="7c823-108">Deklarace `using` v tomto případě odstraní většinu procedury a získá C# na základě dalších jazyků, které zahrnují bloky správy prostředků.</span><span class="sxs-lookup"><span data-stu-id="7c823-108">The `using` declaration removes much of the ceremony here and gets C# on par with other languages that include resource management blocks.</span></span> <span data-ttu-id="7c823-109">`using` založené na vzorech navíc umožňuje vývojářům rozšířit sadu typů, které se tady můžou zúčastnit.</span><span class="sxs-lookup"><span data-stu-id="7c823-109">Additionally the pattern-based `using` lets developers expand the set of types that can participate here.</span></span> <span data-ttu-id="7c823-110">V mnoha případech odebrání nutnosti vytvářet typy obálek, které existují pouze k povolení použití hodnot v příkazu `using`.</span><span class="sxs-lookup"><span data-stu-id="7c823-110">In many cases removing the need to create wrapper types that only exist to allow for a values use in a `using` statement.</span></span> 

<span data-ttu-id="7c823-111">Společně tyto funkce umožňují vývojářům zjednodušit a rozšířit scénáře, kde je možné `using` použít.</span><span class="sxs-lookup"><span data-stu-id="7c823-111">Together these features allow developers to simplify and expand the scenarios where `using` can be applied.</span></span>

## <a name="detailed-design"></a><span data-ttu-id="7c823-112">Podrobný návrh</span><span class="sxs-lookup"><span data-stu-id="7c823-112">Detailed Design</span></span> 

### <a name="using-declaration"></a><span data-ttu-id="7c823-113">using – deklarace</span><span class="sxs-lookup"><span data-stu-id="7c823-113">using declaration</span></span>

<span data-ttu-id="7c823-114">Jazyk umožní přidání `using` do místní deklarace proměnné.</span><span class="sxs-lookup"><span data-stu-id="7c823-114">The language will allow for `using` to be added to a local variable declaration.</span></span> <span data-ttu-id="7c823-115">Taková deklarace bude mít stejný účinek jako deklarace proměnné v příkazu `using` ve stejném umístění.</span><span class="sxs-lookup"><span data-stu-id="7c823-115">Such a declaration will have the same effect as declaring the variable in a `using` statement at the same location.</span></span>

```csharp
if (...) 
{ 
   using FileStream f = new FileStream(@"C:\users\jaredpar\using.md");
   // statements
}

// Equivalent to 
if (...) 
{ 
   using (FileStream f = new FileStream(@"C:\users\jaredpar\using.md")) 
   {
    // statements
   }
}
```

<span data-ttu-id="7c823-116">Doba života `using` místní bude rozšířena na konec oboru, ve kterém je deklarována.</span><span class="sxs-lookup"><span data-stu-id="7c823-116">The lifetime of a `using` local will extend to the end of the scope in which it is declared.</span></span> <span data-ttu-id="7c823-117">`using` národní prostředí bude poté uvolněno v opačném pořadí, ve kterém jsou deklarovány.</span><span class="sxs-lookup"><span data-stu-id="7c823-117">The `using` locals will then be disposed in the reverse order in which they are declared.</span></span> 

```csharp
{ 
    using var f1 = new FileStream("...");
    using var f2 = new FileStream("..."), f3 = new FileStream("...");
    ...
    // Dispose f3
    // Dispose f2 
    // Dispose f1
}
```

<span data-ttu-id="7c823-118">Neexistují žádná omezení `goto`ani žádná jiná konstrukce toku ovládacích prvků na tvář deklarace `using`.</span><span class="sxs-lookup"><span data-stu-id="7c823-118">There are no restrictions around `goto`, or any other control flow construct in the face of a `using` declaration.</span></span> <span data-ttu-id="7c823-119">Místo toho kód funguje stejně jako v případě ekvivalentního příkazu `using`:</span><span class="sxs-lookup"><span data-stu-id="7c823-119">Instead the code acts just as it would for the equivalent `using` statement:</span></span>

```csharp
{
    using var f1 = new FileStream("...");
  target:
    using var f2 = new FileStream("...");
    if (someCondition) 
    {
        // Causes f2 to be disposed but has no effect on f1
        goto target;
    }
}
```

<span data-ttu-id="7c823-120">Místní deklarace v `using` místní deklaraci budou implicitně jen pro čtení.</span><span class="sxs-lookup"><span data-stu-id="7c823-120">A local declared in a `using` local declaration will be implicitly read-only.</span></span> <span data-ttu-id="7c823-121">To odpovídá chování místních hodnot deklarovaných v příkazu `using`.</span><span class="sxs-lookup"><span data-stu-id="7c823-121">This matches the behavior of locals declared in a `using` statement.</span></span> 

<span data-ttu-id="7c823-122">Gramatika jazyka pro `using` deklarace bude následující:</span><span class="sxs-lookup"><span data-stu-id="7c823-122">The language grammar for `using` declarations will be the following:</span></span>

```antlr
local-using-declaration:
  using type using-declarators

using-declarators:
  using-declarator
  using-declarators , using-declarator
  
using-declarator:
  identifier = expression
```

<span data-ttu-id="7c823-123">Omezení týkající se `using` deklarace:</span><span class="sxs-lookup"><span data-stu-id="7c823-123">Restrictions around `using` declaration:</span></span>

- <span data-ttu-id="7c823-124">Se nemusí nacházet přímo uvnitř `case` popisku, ale místo toho musí být uvnitř bloku uvnitř `case` popisku.</span><span class="sxs-lookup"><span data-stu-id="7c823-124">May not appear directly inside a `case` label but instead must be within a block inside the `case` label.</span></span>
- <span data-ttu-id="7c823-125">Nemusí se zobrazit jako součást deklarace proměnné `out`.</span><span class="sxs-lookup"><span data-stu-id="7c823-125">May not appear as part of an `out` variable declaration.</span></span> 
- <span data-ttu-id="7c823-126">Musí mít inicializátor pro každý deklarátor.</span><span class="sxs-lookup"><span data-stu-id="7c823-126">Must have an initializer for each declarator.</span></span>
- <span data-ttu-id="7c823-127">Místní typ musí být implicitně převoditelný na `IDisposable` nebo musí splňovat `using` vzor.</span><span class="sxs-lookup"><span data-stu-id="7c823-127">The local type must be implicitly convertible to `IDisposable` or fulfill the `using` pattern.</span></span>

### <a name="pattern-based-using"></a><span data-ttu-id="7c823-128">založené na vzorcích pomocí</span><span class="sxs-lookup"><span data-stu-id="7c823-128">pattern-based using</span></span>

<span data-ttu-id="7c823-129">Jazyk přidá pojem vzor na jedno použití: to je typ, který má přístupnou metodu instance `Dispose`.</span><span class="sxs-lookup"><span data-stu-id="7c823-129">The language will add the notion of a disposable pattern: that is a type which has an accessible `Dispose` instance method.</span></span> <span data-ttu-id="7c823-130">Typy, které vyhovují vzorům na více než jedno, mohou být součástí příkazu `using` nebo deklarace, aniž by bylo nutné implementovat `IDisposable`.</span><span class="sxs-lookup"><span data-stu-id="7c823-130">Types which fit the disposable pattern can participate in a `using` statement or declaration without being required to implement `IDisposable`.</span></span> 

```csharp
class Resource
{ 
    public void Dispose() { ... }
}

using (var r = new Resource())
{
    // statements
}
```

<span data-ttu-id="7c823-131">To umožní vývojářům využít `using` v mnoha nových scénářích:</span><span class="sxs-lookup"><span data-stu-id="7c823-131">This will allow developers to leverage `using` in a number of new scenarios:</span></span>

- <span data-ttu-id="7c823-132">`ref struct`: tyto typy nemůžou implementovat rozhraní ještě dnes, takže se nemůžou podílet na příkazech `using`.</span><span class="sxs-lookup"><span data-stu-id="7c823-132">`ref struct`: These types can't implement interfaces today and hence can't participate in `using` statements.</span></span>
- <span data-ttu-id="7c823-133">Metody rozšíření umožní vývojářům rozšířit typy v jiných sestaveních k účasti v `using`ch příkazech.</span><span class="sxs-lookup"><span data-stu-id="7c823-133">Extension methods will allow developers to augment types in other assemblies to participate in `using` statements.</span></span>

<span data-ttu-id="7c823-134">V situaci, kdy je možné typ implicitně převést na `IDisposable` a také se doplňuje vzorem na jedno místo, `IDisposable` přednostně.</span><span class="sxs-lookup"><span data-stu-id="7c823-134">In the situation where a type can be implicitly converted to `IDisposable` and also fits the disposable pattern, then `IDisposable` will be preferred.</span></span> <span data-ttu-id="7c823-135">I když tato akce trvá opačný přístup k `foreach` (upřednostňovaný vzor přes rozhraní), je nezbytné pro zpětnou kompatibilitu.</span><span class="sxs-lookup"><span data-stu-id="7c823-135">While this takes the opposite approach of `foreach` (pattern preferred over interface) it is necessary for backwards compatibility.</span></span>

<span data-ttu-id="7c823-136">V tomto případě platí stejná omezení z tradičního příkazu `using`. místní proměnné deklarované v `using` jsou jen pro čtení, `null` hodnota nezpůsobí vyvolání výjimky atd... Generování kódu se bude lišit pouze v tom, že před voláním metody Dispose nebude přetypování na `IDisposable`.</span><span class="sxs-lookup"><span data-stu-id="7c823-136">The same restrictions from a traditional `using` statement apply here as well: local variables declared in the `using` are read-only, a `null` value will not cause an exception to be thrown, etc ... The code generation will be different only in that there will not be a cast to `IDisposable` before calling Dispose:</span></span>

```csharp
{
      Resource r = new Resource();
      try {
            // statements
      }
      finally {
            if (resource != null) resource.Dispose();
      }
}
```

<span data-ttu-id="7c823-137">Aby bylo možné přizpůsobit vzor na více než jedno, `Dispose` metoda musí být přístupná, bez parametrů a musí mít `void` návratový typ.</span><span class="sxs-lookup"><span data-stu-id="7c823-137">In order to fit the disposable pattern the `Dispose` method must be accessible, parameterless and have a `void` return type.</span></span> <span data-ttu-id="7c823-138">Neexistují žádná další omezení.</span><span class="sxs-lookup"><span data-stu-id="7c823-138">There are no other restrictions.</span></span> <span data-ttu-id="7c823-139">To explicitně znamená, že zde lze použít rozšiřující metody.</span><span class="sxs-lookup"><span data-stu-id="7c823-139">This explicitly means that extension methods can be used here.</span></span>

## <a name="considerations"></a><span data-ttu-id="7c823-140">Požadavky</span><span class="sxs-lookup"><span data-stu-id="7c823-140">Considerations</span></span>

### <a name="case-labels-without-blocks"></a><span data-ttu-id="7c823-141">jmenovky Case bez bloků</span><span class="sxs-lookup"><span data-stu-id="7c823-141">case labels without blocks</span></span>

<span data-ttu-id="7c823-142">`using declaration` je nepřípustný přímo v rámci `case` popisku, protože se jedná o komplikace kolem skutečné životnosti.</span><span class="sxs-lookup"><span data-stu-id="7c823-142">A `using declaration` is illegal directly inside a `case` label due to complications around its actual lifetime.</span></span> <span data-ttu-id="7c823-143">Jedním z možných řešení je jednoduše poskytnout stejnou životnost jako `out var` ve stejném umístění.</span><span class="sxs-lookup"><span data-stu-id="7c823-143">One potential solution is to simply give it the same lifetime as an `out var` in the same location.</span></span> <span data-ttu-id="7c823-144">Pro implementaci funkcí se považovala za zvláštní složitost a snadno se rozhlasí (stačí přidat blok na `case` popisku), který tuto trasu neodůvodňuje.</span><span class="sxs-lookup"><span data-stu-id="7c823-144">It was deemed the extra complexity to the feature implementation and the ease of the work around (just add a block to the `case` label) didn't justify taking this route.</span></span>

## <a name="future-expansions"></a><span data-ttu-id="7c823-145">Budoucí rozšíření</span><span class="sxs-lookup"><span data-stu-id="7c823-145">Future Expansions</span></span>

### <a name="fixed-locals"></a><span data-ttu-id="7c823-146">pevná národní prostředí</span><span class="sxs-lookup"><span data-stu-id="7c823-146">fixed locals</span></span>

<span data-ttu-id="7c823-147">Příkaz `fixed` obsahuje všechny vlastnosti `using` příkazů, které motivují schopnost mít `using` národní prostředí.</span><span class="sxs-lookup"><span data-stu-id="7c823-147">A `fixed` statement has all of the properties of `using` statements that motivated the ability to have `using` locals.</span></span> <span data-ttu-id="7c823-148">Mělo by se zvážit, že by se tato funkce roz`fixed`a i na místní prostředí.</span><span class="sxs-lookup"><span data-stu-id="7c823-148">Consideration should be given to extending this feature to `fixed` locals as well.</span></span> <span data-ttu-id="7c823-149">Pravidla doby platnosti a řazení by se měla použít stejně dobře pro `using` a `fixed`.</span><span class="sxs-lookup"><span data-stu-id="7c823-149">The lifetime and ordering rules should apply equally well for `using` and `fixed` here.</span></span>
