---
ms.openlocfilehash: aa0c233e7739ae7a0a7752251aae6f89df18ba93
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484515"
---
# <a name="c-language-proposals"></a>C#Návrhy jazyka

Jazykové návrhy jsou živými dokumenty, které popisují aktuální uvažujete o dané funkci jazyka.

Návrhy můžou být *aktivní*, *neaktivní*, *zamítnuté* nebo *dokončené*. *Aktivní* návrhy jsou uloženy přímo ve složce návrhy, *neaktivní* a *odmítnuté* návrhy jsou uloženy v [neaktivních](proposals/inactive) a [odmítnutých](proposals/rejected) podsložkách a *hotové* návrhy jsou archivovány ve složce, která odpovídá jazykové verzi, ve které jsou součástí.

## <a name="lifetime-of-a-proposal"></a>Doba platnosti návrhu

Návrh začne svůj životní cyklus, když tým pro návrh jazyka rozhodne, že může být pro daný jazyk dobrý den. Obvykle se aktivuje jako *aktivní*, ale pokud chceme zachytit nápad, aniž by k tomu chtěli pracovat hned teď, návrh se může také vystavovat v *neaktivní* podsložce. Návrhy se můžou dokonce uskutečnit přímo v *zamítnutém* stavu, pokud chceme udělat záznam o něco, co neplánujeme. Například pokud není možné implementovat oblíbenou a opakovanou žádost, můžeme ji zachytit jako odmítnutý návrh.

Návrh může začít jako nápad v [diskuzi](https://github.com/dotnet/csharplang/labels/Discussion)nebo může pocházet z diskusí na schůzi s návrhem jazyka nebo přicházejí z mnoha dalších front. Hlavní věc je, že tým návrhu si zdá, že by měl být hotový a že je někdo, kdo je ochotn ho zapsat. Obvykle člen návrhového týmu jazyka přiřadí sám sebe jako Champion pro funkci, která je sledována [problémem Champion](https://github.com/dotnet/csharplang/labels/Proposal%20champion). Champion zodpovídá za přesunutí návrhu prostřednictvím procesu návrhu.

Návrh je *aktivní* , pokud se předá návrh a implementace směrem k nadcházející verzi. Po úplném *dokončení*, tj. implementace byla sloučena do vydané verze a byla zadána funkce, přesunuta do podadresáře odpovídajícího jeho vydané verzi.

Pokud se funkce nestane pravděpodobně, že by ji bylo možné napravit do jazyka vůbec, např. vzhledem k tomu, že se nejedná o neproveditelné, nezdá se, že by se přidala dostatečná hodnota, nebo jen to není pro daný jazyk správné, bude [odmítnut](proposals/rejected). Pokud má funkce přiměřenou příslib, ale v současné době není nastavena priorita, může být deklarována jako [neaktivní](proposals/inactive) , aby nedocházelo k hlavní složce. Je zcela jemný pokuta k tomu, aby fungovala v neaktivních nebo odmítnutých návrzích, a jejich obnoven později. Kategorie jsou zde, aby odrážely aktuální záměr návrhu.

## <a name="nature-of-a-proposal"></a>Povaha návrhu

Návrh by měl následovat po [šabloně návrhu](proposal-template.md). Dobrý návrh by měl:

- Připadá k obecnému duchu a estetickému jazyku.
- Nelze zavést nestejnou alternativní syntaxi pro existující funkce.
- Přidejte spoustu hodnot pro zřetelnou skupinu uživatelů.
- Nedávejte významně ke složitosti jazyka, zejména pro nové uživatele.  

## <a name="discussion-of-proposals"></a>Diskuze o návrzích

Zpětná vazba a diskuze dojde v [problémech diskuze](https://github.com/dotnet/csharplang/labels/Discussion). Když se do složky návrhy přidá nový návrh, měla by být oznámena problémem diskuze Champion nebo autorem návrhu. 

 