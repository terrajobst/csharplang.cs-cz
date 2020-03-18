---
ms.openlocfilehash: 36f3e6204d12c2569b3a55f3a47f58337e8a08e4
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484627"
---
# <a name="indexing-fixed-fields-should-not-require-pinning-regardless-of-the-movableunmovable-context"></a><span data-ttu-id="cdfb7-101">Indexování `fixed` polí nesmí vyžadovat připnutí bez ohledu na Pohyblivý/nemovitý kontext.</span><span class="sxs-lookup"><span data-stu-id="cdfb7-101">Indexing `fixed` fields should not require pinning regardless of the movable/unmovable context.</span></span> #

<span data-ttu-id="cdfb7-102">Změna má velikost opravy chyby.</span><span class="sxs-lookup"><span data-stu-id="cdfb7-102">The change has the size of a bug fix.</span></span> <span data-ttu-id="cdfb7-103">Může to být v 7,3 a není v konfliktu s jakýmkoli směrem, co dál probereme.</span><span class="sxs-lookup"><span data-stu-id="cdfb7-103">It can be in 7.3 and does not conflict with whatever direction we take further.</span></span>
<span data-ttu-id="cdfb7-104">Tato změna je jenom o tom, jak povolit, aby následující scénář fungoval, i když je `s` mobilní.</span><span class="sxs-lookup"><span data-stu-id="cdfb7-104">This change is only about allowing the following scenario to work even though `s` is moveable.</span></span> <span data-ttu-id="cdfb7-105">Je již platná, pokud `s` není mobilní.</span><span class="sxs-lookup"><span data-stu-id="cdfb7-105">It is already valid when `s` is not moveable.</span></span> 

<span data-ttu-id="cdfb7-106">Poznámka: v obou případech ještě vyžaduje `unsafe` kontext.</span><span class="sxs-lookup"><span data-stu-id="cdfb7-106">NOTE: in either case, it still requires `unsafe` context.</span></span> <span data-ttu-id="cdfb7-107">Je možné číst neinicializovaná data nebo dokonce mimo rozsah.</span><span class="sxs-lookup"><span data-stu-id="cdfb7-107">It is possible to read uninitialized data or even out of range.</span></span> <span data-ttu-id="cdfb7-108">To se nemění.</span><span class="sxs-lookup"><span data-stu-id="cdfb7-108">That is not changing.</span></span>

```csharp
unsafe struct S
{
    public fixed int myFixedField[10];
}

class Program
{
    static S s;

    unsafe static void Main()
    {
        int p = s.myFixedField[5]; // indexing fixed-size array fields would be ok
    }
}
```

<span data-ttu-id="cdfb7-109">Hlavní "výzva", kterou mi vidíte, je vysvětlit, jak ve specifikaci vysvětlovat zmírnění. Zejména, protože následující by stále vyžadoval připnutí.</span><span class="sxs-lookup"><span data-stu-id="cdfb7-109">The main “challenge” that I see here is how to explain the relaxation in the spec. In particular, since the following would still need pinning.</span></span> <span data-ttu-id="cdfb7-110">(protože `s` je mobilní a explicitně používáme pole jako ukazatel)</span><span class="sxs-lookup"><span data-stu-id="cdfb7-110">(because `s` is moveable and we explicitly use the field as a pointer)</span></span>

```csharp
unsafe struct S
{
    public fixed int myFixedField[10];
}

class Program
{
    static S s;

    unsafe static void Main()
    {
        int* ptr = s.myFixedField; // taking a pointer explicitly still requires pinning.
        int p = ptr[5];
    }
}
```

<span data-ttu-id="cdfb7-111">Jedním z důvodů, proč potřebujeme připnout cíl, když je pohyblivý, je artefaktem naší strategie generování kódu. vždycky se převádí na nespravovaný ukazatel, takže uživatel přinutí připnutí prostřednictvím `fixed`ho příkazu.</span><span class="sxs-lookup"><span data-stu-id="cdfb7-111">One reason why we require pinning of the target when it is movable is the artifact of our code generation strategy, - we always convert to an unmanaged pointer and thus force the user to pin via `fixed` statement.</span></span> <span data-ttu-id="cdfb7-112">Převod na nespravovaný není ale při indexování nezbytný.</span><span class="sxs-lookup"><span data-stu-id="cdfb7-112">However, conversion to unmanaged is unnecessary when doing indexing.</span></span> <span data-ttu-id="cdfb7-113">Stejný nebezpečný ukazatel je stejný jako v případě, že máme příjemce ve formě spravovaného ukazatele.</span><span class="sxs-lookup"><span data-stu-id="cdfb7-113">The same unsafe pointer math is equally applicable when we have the receiver in the form of a managed pointer.</span></span> <span data-ttu-id="cdfb7-114">Pokud to provedeme, pak se zprostředkující odkaz spravuje (je sledováno v GC) a připnutí je zbytečné.</span><span class="sxs-lookup"><span data-stu-id="cdfb7-114">If we do that, then the intermediate ref is managed (GC-tracked) and the pinning is unnecessary.</span></span>

<span data-ttu-id="cdfb7-115">Změna https://github.com/dotnet/roslyn/pull/24966 je prototypem PR, který tento požadavek zmírnit.</span><span class="sxs-lookup"><span data-stu-id="cdfb7-115">The change https://github.com/dotnet/roslyn/pull/24966 is a prototype PR that relaxes this requirement.</span></span>
