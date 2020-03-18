---
ms.openlocfilehash: 36f3e6204d12c2569b3a55f3a47f58337e8a08e4
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484627"
---
# <a name="indexing-fixed-fields-should-not-require-pinning-regardless-of-the-movableunmovable-context"></a>Indexování `fixed` polí nesmí vyžadovat připnutí bez ohledu na Pohyblivý/nemovitý kontext. #

Změna má velikost opravy chyby. Může to být v 7,3 a není v konfliktu s jakýmkoli směrem, co dál probereme.
Tato změna je jenom o tom, jak povolit, aby následující scénář fungoval, i když je `s` mobilní. Je již platná, pokud `s` není mobilní. 

Poznámka: v obou případech ještě vyžaduje `unsafe` kontext. Je možné číst neinicializovaná data nebo dokonce mimo rozsah. To se nemění.

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

Hlavní "výzva", kterou mi vidíte, je vysvětlit, jak ve specifikaci vysvětlovat zmírnění. Zejména, protože následující by stále vyžadoval připnutí. (protože `s` je mobilní a explicitně používáme pole jako ukazatel)

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

Jedním z důvodů, proč potřebujeme připnout cíl, když je pohyblivý, je artefaktem naší strategie generování kódu. vždycky se převádí na nespravovaný ukazatel, takže uživatel přinutí připnutí prostřednictvím `fixed`ho příkazu. Převod na nespravovaný není ale při indexování nezbytný. Stejný nebezpečný ukazatel je stejný jako v případě, že máme příjemce ve formě spravovaného ukazatele. Pokud to provedeme, pak se zprostředkující odkaz spravuje (je sledováno v GC) a připnutí je zbytečné.

Změna https://github.com/dotnet/roslyn/pull/24966 je prototypem PR, který tento požadavek zmírnit.
