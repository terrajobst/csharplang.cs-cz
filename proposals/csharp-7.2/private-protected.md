---
ms.openlocfilehash: 6cf489595654236c18edee94c0af380e605c9571
ms.sourcegitcommit: f61a06970fa0562d2e40363fae3948eb168624ca
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/14/2020
ms.locfileid: "79485173"
---
# <a name="private-protected"></a>private protected

* Navrženo [x]
* [x] prototyp: [dokončeno](https://github.com/dotnet/roslyn/blob/master/docs/features/private-protected.md)
* [x] implementace: [dokončeno](https://github.com/dotnet/roslyn/blob/master/docs/features/private-protected.md)
* [x] specifikace: [dokončeno](#detailed-design)

## <a name="summary"></a>Souhrn
[summary]: #summary

Vystavit `protectedAndInternal` úroveň přístupnosti CLR v C# jako `private protected`.

## <a name="motivation"></a>Motivační
[motivation]: #motivation

Existuje mnoho okolností, kdy rozhraní API obsahuje členy, kteří mají být implementováni a používány podtřídami obsaženými v sestavení, které poskytuje typ. I když CLR poskytuje úroveň přístupnosti pro tento účel, není k dispozici C#v. V důsledku toho jsou vlastníci rozhraní API nuceni použít `internal` Protection a samoobslužný nebo vlastní analyzátor, nebo používat `protected` s další dokumentací, což vysvětluje, že když se člen objeví ve veřejné dokumentaci pro daný typ, není určen jako součást veřejného rozhraní API.  Příklady naleznete v tématu Členové Roslyn `CSharpCompilationOptions` jejichž názvy začínají na `Common`.

Přímý poskytnutí podpory pro tuto úroveň přístupu v C# nástroji umožňuje, aby tyto okolnosti byly vyjádřeny přirozeně v jazyce.

## <a name="detailed-design"></a>Podrobný návrh
[design]: #detailed-design

### <a name="private-protected-access-modifier"></a>`private protected` modifikátor přístupu

Navrhujeme přidat novou kombinaci modifikátoru přístupu `private protected` (která se může objevit v libovolném pořadí mezi modifikátory). To se mapuje na pojem CLR protectedAndInternal a vypůjčí stejnou syntaxi, která se aktuálně používá v [ C++/CLI](https://docs.microsoft.com/cpp/dotnet/how-to-define-and-consume-classes-and-structs-cpp-cli#BKMK_Member_visibility).

Člen deklarovaný `private protected` může být k dispozici v rámci podtřídy jeho kontejneru, pokud je tato podtřída ve stejném sestavení jako člen.

Tuto specifikaci jazyka upravujeme následujícím způsobem (tučně se doplňují). Čísla oddílů nejsou uvedená níže, protože se mohou lišit v závislosti na tom, která verze specifikace je integrována do.

-----

> Deklarovaná přístupnost člena může být jedna z následujících:
- Public, který je vybrán zahrnutím veřejného modifikátoru v deklaraci členu. Intuitivní význam veřejné aplikace je "přístup není omezený".
- Protected, který je vybrán zahrnutím chráněného modifikátoru v deklaraci členu. Intuitivní význam ochrany je "přístup omezený na obsahující třídu nebo typy odvozené z nadřazené třídy".
- Interní, který je vybrán zahrnutím interního modifikátoru v deklaraci členu. Intuitivní význam interní je "přístup omezený na toto sestavení".
- Chráněná interní, která je vybrána tak, že zahrne chráněný i interní modifikátor v deklaraci členu. Intuitivní význam chráněného interního rozhraní je "přístupný v rámci tohoto sestavení" a také typy odvozené z obsahující třídy ".
- **Private Protected, který je vybrán zahrnutím privátního a chráněného modifikátoru v deklaraci členu. Intuitivní význam Private Protected je "přístupný v rámci tohoto sestavení" podle typů odvozených z obsahující třídy ".**

-----

> V závislosti na kontextu, ve kterém je deklarace členů provedena, jsou povoleny pouze určité typy deklarovaného usnadnění. Kromě toho, když deklarace člena neobsahuje žádné modifikátory přístupu, určuje kontext, ve kterém se deklarace provede, výchozí deklarovaná přístupnost. 
- Obory názvů mají implicitně veřejnou deklaraci přístupnosti. U deklarací oboru názvů nejsou povoleny žádné modifikátory přístupu.
- Typy deklarované přímo v kompilačních jednotkách nebo oborech názvů (na rozdíl od jiných typů) mohou mít veřejné nebo interní deklarované přístupnosti a výchozí hodnoty pro interní deklarovanou přístupnost.
- Členové třídy mohou mít kterýkoli z pěti druhů deklarovaného přístupnosti a výchozích možností jako soukromé deklarované dostupnosti. [Poznámka: typ deklarovaný jako člen třídy může mít kterýkoli z pěti druhů deklarovaného přístupnosti, zatímco typ deklarovaný jako člen oboru názvů může mít jenom veřejný nebo interní deklarovaný přístup. Koncová Poznámka]
- Členové struktury mohou mít veřejné, interní nebo soukromé deklarované přístupnosti a výchozí hodnoty jako soukromé deklarované přístupnosti, protože struktury jsou implicitně zapečetěné. Členy struktury představené ve struktuře (to znamená, že není zděděná touto strukturou) nemůžou*mít chráněnou* ~~nebo~~ chráněnou interní**nebo soukromou chráněnou** přístupnost. [Poznámka: typ deklarovaný jako člen struktury může mít veřejné, interní nebo soukromé deklarované přístupnosti, zatímco typ deklarovaný jako člen oboru názvů může mít jenom veřejný nebo interní deklarovaný přístup. Koncová Poznámka]
- Členové rozhraní mají implicitně deklaraci Public deklarovaného přístupnosti. V deklaracích členů rozhraní nejsou povoleny žádné modifikátory přístupu.
- Členové výčtu mají implicitně veřejnou deklaraci přístupnosti. Pro deklarace členů výčtu nejsou povoleny žádné modifikátory přístupu.

-----

> Doména přístupnosti vnořeného člena M deklarovaného v typu T v rámci programu P je definována takto (s označením, že M samotný může být typ):
- Pokud je deklarovaná přístupnost M veřejná, doména přístupnosti M je doména přístupnosti T.
- Pokud je deklarovaná přístupnost M chráněná jako interní, nechť D je sjednocení textu programu P a text programu libovolného typu odvozeného z T, který je deklarovaný mimo P. Doména přístupnosti M je průsečík domény přístupnosti T s D.
- **Pokud je deklarovaná přístupnost M chráněná jako soukromá, nechť D je průnik textu programu P a textu programu libovolného typu odvozeného z T. Doména přístupnosti M je průsečík domény přístupnosti T s D.**
- Pokud je deklarovaná přístupnost M chráněná, nechť D je sjednocení textu programu T a text programu libovolného typu odvozeného z T. Doména přístupnosti M je průsečík domény přístupnosti T s D.
- Pokud je deklarovaná přístupnost M interní, doména přístupnosti M je průsečík domény přístupnosti T s textem programu P.
- Pokud je deklarovaná přístupnost M soukromá, doména přístupnosti M je text programu T.

-----

> Je-li chráněn **nebo soukromý** člen instance přístupný mimo text programu třídy, ve které je deklarována, a je-li k chráněnému členu instance použit přístup mimo text programu, ve kterém je deklarován, přístup proběhne v rámci deklarace třídy, která je odvozena od třídy, ve které je deklarována. Kromě toho je nutné přístup provést prostřednictvím instance daného typu odvozené třídy nebo z něj vytvořeného typu třídy. Toto omezení zabrání jedné odvozené třídě v přístupu k chráněným členům jiných odvozených tříd, a to i v případě, že jsou členové děděni ze stejné základní třídy.

-----

> Povolené modifikátory povoleného přístupu a výchozí přístup pro deklaraci typu závisí na kontextu, ve kterém se deklarace provádí (§ 9.5.2):
- Typy deklarované v jednotkách kompilace nebo oborech názvů můžou mít veřejný nebo interní přístup. Výchozím nastavením je interní přístup.
- Typy deklarované ve třídách můžou mít veřejný, chráněný interní, **soukromý**, chráněný, interní nebo soukromý přístup. Výchozí hodnota je privátní přístup.
- Typy deklarované ve strukturách mohou mít veřejný, interní nebo privátní přístup. Výchozí hodnota je privátní přístup.

-----

> Deklarace statické třídy podléhá následujícím omezením:
- Statická třída nesmí zahrnovat uzavřený nebo abstraktní modifikátor. (Nicméně vzhledem k tomu, že statická třída nemůže být vytvořena nebo odvozena z, se chová, jako by byla zapečetěná a abstraktní.)
- Statická třída nesmí obsahovat specifikaci základní třídy (§ 16.2.5) a nemůže explicitně zadat základní třídu nebo seznam implementovaných rozhraní. Statická třída implicitně dědí z objektu Type.
- Statická třída musí obsahovat pouze statické členy (§ 16.4.8). [Poznámka: všechny konstanty a vnořené typy jsou klasifikovány jako statické členy. Koncová Poznámka]
- Statická třída nesmí mít členy s chráněnou **, soukromou chráněnou** nebo chráněnou interní deklarovanou přístupností.

> Jedná se o chybu při kompilaci, která porušuje některá z těchto omezení. 

-----

> Deklarace členu třídy může mít jeden z ~~pěti~~**možných typů** deklarovaného přístupnosti (§ 9.5.2): Public, **Private Protected**, Protected Internal, Protected, Internal nebo Private. Výjimkou je chráněná interní **a privátní chráněná** kombinace**s**, jedná se o chybu při kompilaci k určení více než jednoho modifikátoru přístupu. Když deklarace členu třídy neobsahuje žádné modifikátory přístupu, předpokládá se typ Private.

-----

> Nevnořené typy mohou mít veřejné nebo interní deklarované přístupnosti, které mají ve výchozím nastavení interní deklaraci přístupnosti. Vnořené typy mohou mít tyto formuláře deklarovaného přístupnosti a navíc jednu nebo více dalších forem deklarovaného usnadnění, v závislosti na tom, zda je obsažený typ třída nebo struktura:
- Vnořený typ, který je deklarován ve třídě, může mít ~~pět~~**šest** forem deklarovaného přístupnosti (Public, **Private Protected**, Protected Internal, Protected, Internal nebo Private) a, podobně jako ostatní členy třídy, ve výchozím nastavení soukromé deklarované přístupnosti.
- Vnořený typ deklarovaný ve struktuře může mít jakékoli tři formy deklarovaného přístupnosti (veřejné, interní nebo soukromé) a, podobně jako ostatní členy struktury, výchozí nastavení soukromé deklarované dostupnosti.

-----

> Metoda přepsaná deklarací přepisu je označována jako přepsaná základní metoda pro metodu override M deklarovanou ve třídě C, přepsaná základní metoda je určena kontrolou každého typu základní třídy C, počínaje přímým typem základní třídy C a pokračuje se každý po sobě jdoucí typ základní třídy, dokud v daném typu základní třídy je umístěna alespoň jedna přístupná metoda, která má stejný podpis jako M po nahrazení argumentů typu. Pro účely nalezení přepsané základní metody je metoda považována za přístupná, pokud je veřejná, pokud je chráněná, pokud je chráněná interní, nebo v případě, že **se jedná o interní** **nebo privátní chráněný** a deklarovaný ve stejném programu jako C.

-----

> Použití modifikátorů přístupového objektu se řídí následujícími omezeními:
- Modifikátor přístupového objektu se nedá použít v rozhraní nebo v explicitní implementaci člena rozhraní.
- U vlastnosti nebo indexeru, který nemá modifikátor přepisu, je povolen modifikátor přistupujícího objektu pouze v případě, že vlastnost nebo indexer má přistupující objekt get i set a pak je povolen pouze v jednom z těchto přístupových objektů.
- V případě vlastnosti nebo indexeru, který obsahuje modifikátor přepsání, musí přistupující objekt odpovídat modifikátoru přistupujícího objektu (pokud existuje) přepsaného objektu.
- Modifikátor přístupového objektu musí deklarovat přístupnost, která je striktně přísnější než deklarovaná přístupnost vlastnosti nebo samotného indexeru. Bude přesný:
  - Pokud má vlastnost nebo indexeru deklarovanou přístupnost veřejné, může být modifikátor přístupu buď **privátní**, chráněný interní, interní, chráněný nebo soukromý.
  - Pokud má vlastnost nebo indexeru deklarovanou přístupnost chráněného interního typu, může být modifikátor přístupu buď **privátní**, chráněný nebo soukromý.
  - Pokud vlastnost nebo indexer má deklaraci přístupnosti jako interní nebo chráněnou, modifikátor přístupu musí být **buď privátní, nebo** soukromý.
  - **Pokud má vlastnost nebo indexeru deklarovanou přístupnost privátního chráněného objektu, modifikátor přístupu musí být privátní.**
  - Pokud vlastnost nebo indexer má deklaraci přístupnosti Private, nelze použít modifikátor přístupového objektu.

-----

> Vzhledem k tomu, že dědičnost není pro struktury podporovaná, deklarovaná přístupnost člena struktury nemůže být chráněná, **soukromá**nebo chráněná interní.

-----

## <a name="drawbacks"></a>Nevýhody
[drawbacks]: #drawbacks

Stejně jako u libovolné jazykové funkce musíme dotazovat, jestli se další složitost tohoto jazyka znovu vyplácí v další čistotě, která je k dispozici pro tělo C# programů, které by mohly využít výhod této funkce.

## <a name="alternatives"></a>Alternativy
[alternatives]: #alternatives

Alternativou je zřízení rozhraní API, které kombinuje atribut a analyzátor. Atribut je umístěn programátorem na `internal` člen, který označuje, že člen je určen k použití pouze v podtříd a analyzátor kontroluje, zda jsou tato omezení nahlášena. 

## <a name="unresolved-questions"></a>Nevyřešené dotazy
[unresolved]: #unresolved-questions

Implementace je převážně kompletní. Jedinou otevřenou pracovní položkou je koncept odpovídající specifikace pro jazyk VB.

## <a name="design-meetings"></a>Schůzky pro návrh

Bude doplněno
