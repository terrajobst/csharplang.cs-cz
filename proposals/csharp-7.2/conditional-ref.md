---
ms.openlocfilehash: 63dfdfee9ea6c16e162f483aa1298feed297daef
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484697"
---
# <a name="conditional-ref-expressions"></a><span data-ttu-id="8a60e-101">Podmíněné výrazy ref</span><span class="sxs-lookup"><span data-stu-id="8a60e-101">Conditional ref expressions</span></span>

<span data-ttu-id="8a60e-102">Vzor vazby proměnné ref na jeden nebo jiný výraz podmíněně není aktuálně vyhodnotit v C#.</span><span class="sxs-lookup"><span data-stu-id="8a60e-102">The pattern of binding a ref variable to one or another expression conditionally is not currently expressible in C#.</span></span>

<span data-ttu-id="8a60e-103">Typickým alternativním řešením je zavedení metody jako:</span><span class="sxs-lookup"><span data-stu-id="8a60e-103">The typical workaround is to introduce a method like:</span></span>

```csharp
ref T Choice(bool condition, ref T consequence, ref T alternative)
{
    if (condition)
    {
         return ref consequence;
    }
    else
    {
         return ref alternative;
    }
}
```

<span data-ttu-id="8a60e-104">Všimněte si, že se nejedná o přesné nahrazení Ternární, protože všechny argumenty musí být vyhodnoceny na webu volání.</span><span class="sxs-lookup"><span data-stu-id="8a60e-104">Note that this is not an exact replacement of a ternary since all arguments must be evaluated at the call site.</span></span>

<span data-ttu-id="8a60e-105">Následující nebudou fungovat podle očekávání:</span><span class="sxs-lookup"><span data-stu-id="8a60e-105">The following will not work as expected:</span></span>

```csharp
       // will crash with NRE because 'arr[0]' will be executed unconditionally
      ref var r = ref Choice(arr != null, ref arr[0], ref otherArr[0]);
```

<span data-ttu-id="8a60e-106">Navrhovaná syntaxe by vypadala takto:</span><span class="sxs-lookup"><span data-stu-id="8a60e-106">The proposed syntax would look like:</span></span>

```csharp
     <condition> ? ref <consequence> : ref <alternative>;
```

<span data-ttu-id="8a60e-107">Výše uvedený pokus s možností "Choice" lze _správně_ zapsat pomocí odkazu Ternární jako:</span><span class="sxs-lookup"><span data-stu-id="8a60e-107">The above attempt with "Choice" can be _correctly_ written using ref ternary as:</span></span>

```csharp
     ref var r = ref (arr != null ? ref arr[0]: ref otherArr[0]);
```

<span data-ttu-id="8a60e-108">Rozdílem od volby je, že k sobě a alternativním výrazům se přistupoval v _skutečně_ podmíněném způsobu, takže se při ```arr == null``` nezobrazuje chyba.</span><span class="sxs-lookup"><span data-stu-id="8a60e-108">The difference from Choice is that consequence and alternative expressions are accessed in a _truly_ conditional manner, so we do not see a crash if ```arr == null```</span></span>

<span data-ttu-id="8a60e-109">Ternární odkaz je pouze Ternární, pokud je alternativa i i pořadím ReFS.</span><span class="sxs-lookup"><span data-stu-id="8a60e-109">The ternary ref is just a ternary where both alternative and consequence are refs.</span></span> <span data-ttu-id="8a60e-110">Bude přirozeným způsobem vyžadovat, aby byly hodnoty lvalue nebo alternativní operandy.</span><span class="sxs-lookup"><span data-stu-id="8a60e-110">It will naturally require that consequence/alternative operands are LValues.</span></span> <span data-ttu-id="8a60e-111">Bude také vyžadovat, aby v důsledku i v sobě byly typy, které jsou vzájemně převoditelné na sebe.</span><span class="sxs-lookup"><span data-stu-id="8a60e-111">It will also require that consequence and alternative have types that are identity convertible to each other.</span></span>

<span data-ttu-id="8a60e-112">Typ výrazu bude vypočítán podobně jako pro regulární Ternární výraz.</span><span class="sxs-lookup"><span data-stu-id="8a60e-112">The type of the expression will be computed similarly to the one for the regular ternary.</span></span> <span data-ttu-id="8a60e-113">t.</span><span class="sxs-lookup"><span data-stu-id="8a60e-113">I.E.</span></span> <span data-ttu-id="8a60e-114">v případě, že je souběžnost a alternativa převoditelné identity, ale různé typy, budou platit existující pravidla sloučení typů.</span><span class="sxs-lookup"><span data-stu-id="8a60e-114">in a case if consequence and alternative have identity convertible, but different types, the existing type-merging rules will apply.</span></span>

<span data-ttu-id="8a60e-115">Z podmíněných operandů se bude s bezpečným vrácením předpokládat konzervativní přístup.</span><span class="sxs-lookup"><span data-stu-id="8a60e-115">Safe-to-return will be assumed conservatively from the conditional operands.</span></span> <span data-ttu-id="8a60e-116">Pokud buď není bezpečné vracet celou věc, nebudete se moct vrátit.</span><span class="sxs-lookup"><span data-stu-id="8a60e-116">If either is unsafe to return the whole thing is unsafe to return.</span></span>

<span data-ttu-id="8a60e-117">Odkaz Ternární je l-hodnota a jako takový je možné ho předat/přiřadit/vrátit odkazem.</span><span class="sxs-lookup"><span data-stu-id="8a60e-117">Ref ternary is an LValue and as such it can be passed/assigned/returned by reference;</span></span>

```csharp
     // pass by reference
     foo(ref (arr != null ? ref arr[0]: ref otherArr[0]));

     // return by reference
     return ref (arr != null ? ref arr[0]: ref otherArr[0]);
```

<span data-ttu-id="8a60e-118">Je-li to hodnota l-hodnota, lze ji také přiřadit.</span><span class="sxs-lookup"><span data-stu-id="8a60e-118">Being an LValue, it can also be assigned to.</span></span> 

```csharp
    // assign to
    (arr != null ? ref arr[0]: ref otherArr[0]) = 1;
```

<span data-ttu-id="8a60e-119">Odkaz Ternární lze použít také v běžném (nikoli ref) kontextu.</span><span class="sxs-lookup"><span data-stu-id="8a60e-119">Ref ternary can be used in a regular (not ref) context as well.</span></span> <span data-ttu-id="8a60e-120">I když by nedošlo k běžnému použití, protože byste mohli také pouze použít regulární Ternární.</span><span class="sxs-lookup"><span data-stu-id="8a60e-120">Although it would not be common since you could as well just use a regular ternary.</span></span>

```csharp
     int x = (arr != null ? ref arr[0]: ref otherArr[0]);
```


___

<span data-ttu-id="8a60e-121">Poznámky k implementaci:</span><span class="sxs-lookup"><span data-stu-id="8a60e-121">Implementation notes:</span></span> 

<span data-ttu-id="8a60e-122">Složitost implementace by znamenala velikost chyby střední až velké.</span><span class="sxs-lookup"><span data-stu-id="8a60e-122">The complexity of the implementation would seem to be the size of a moderate-to-large bug fix.</span></span> <span data-ttu-id="8a60e-123">-I. E není velmi nákladný.</span><span class="sxs-lookup"><span data-stu-id="8a60e-123">- I.E not very expensive.</span></span>
<span data-ttu-id="8a60e-124">Nevím, že potřebujeme jakékoli změny syntaxe nebo analýzy.</span><span class="sxs-lookup"><span data-stu-id="8a60e-124">I do not think we need any changes to the syntax or parsing.</span></span>
<span data-ttu-id="8a60e-125">Neexistuje žádný vliv na metadata nebo interoperabilitu.</span><span class="sxs-lookup"><span data-stu-id="8a60e-125">There is no effect on metadata or interop.</span></span> <span data-ttu-id="8a60e-126">Tato funkce je zcela založená na výrazu.</span><span class="sxs-lookup"><span data-stu-id="8a60e-126">The feature is completely expression based.</span></span>
<span data-ttu-id="8a60e-127">Žádný vliv na ladění/PDB buď</span><span class="sxs-lookup"><span data-stu-id="8a60e-127">No effect on debugging/PDB either</span></span>
