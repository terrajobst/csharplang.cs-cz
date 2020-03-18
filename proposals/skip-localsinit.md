---
ms.openlocfilehash: 52b43abd2d8fb56088a68c7169289a63c43ce96f
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484487"
---
# <a name="suppress-emitting-of-localsinit-flag"></a>Potlačit vygenerování příznaku `localsinit`

* Navrženo [x]
* [] Prototyp: Nezahájeno
* [] Implementace: Nezahájeno
* [] Specifikace: Nezahájeno

## <a name="summary"></a>Souhrn
[summary]: #summary

Umožňuje potlačit generování příznaku `localsinit` prostřednictvím atributu `SkipLocalsInitAttribute`. 

## <a name="motivation"></a>Motivační
[motivation]: #motivation


### <a name="background"></a>Pozadí
Podle specifikace CLR nejsou místní proměnné, které neobsahují odkazy, inicializovány na konkrétní hodnotu pomocí virtuálního počítače/JIT. Čtení z takových proměnných bez inicializace je typově bezpečné, ale v opačném případě je chování nedefinované a specifické pro implementaci. Obvykle neinicializovaná národní prostředí obsahují jakékoli hodnoty, které byly ponechány v paměti, která je nyní obsazena rámcem zásobníku. To může vést k nedeterministickému chování a těžko reprodukování chyb. 

Existují dva způsoby přiřazení místní proměnné: 
- uložením hodnoty nebo 
- zadáním příznaku `localsinit`, který vynutí, aby se všechny položky, které jsou přidělené, vynutily na základě nulové hodnoty paměti, která je nastavená na nulu: to zahrnuje místní proměnné `stackalloc` a    

Použití neinicializovaných dat se nedoporučuje a v ověřitelném kódu není povoleno. I když může být možné prokázat, že se jedná o prostředky analýzy toků, je povolený, aby ověřovací algoritmus byl konzervativní, a jednoduše vyžadoval, aby byl nastaven `localsinit`.

C# Historicky vygeneruje `localsinit` příznak u všech metod, které deklaruje národní prostředí.

Při C# použití analýzy s určitou možností přiřazení, která je přísnější než specifikace CLR (C# musí také zvážit rozsah místních hodnot), není bezpodmínečně zaručeno, aby výsledný kód byl formálně ověřitelný:
- CLR a C# pravidla nemusí souhlasit s tím, zda je jako argument předávána místní jako `out` argumentu `use`.
- CLR a C# pravidla nesmí souhlasit s zpracováním podmíněných větví, pokud jsou podmínky známy (konstantní šíření).
- Modul CLR může také jednoduše vyžadovat `localinits`, protože to je povoleno.  

### <a name="problem"></a>Problém
V aplikaci s vysokým výkonem může být možné poznamenat náklady na vynucenou nulovou inicializaci. Je obzvláště patrné, když se používá `stackalloc`.

V některých případech může kompilátor JIT Elide počáteční nulovou inicializaci jednotlivých národních prostředí, když je taková inicializace "usmrcena" při následném přiřazení. Ne všechny nepovolují kompilátory JIT a taková optimalizace mají omezení. Neposkytuje vám `stackalloc`.

K ilustraci, že je problém skutečný – je známá chyba, pokud metoda, která neobsahuje žádné `IL` místní, nemá příznak `localsinit`. Tato chyba je již zneužita uživateli tím, že do takových metod zapisuje `stackalloc` do takových metod – úmyslně se vyhnout nákladům na inicializaci. To je navzdory tomu, že absence `IL`ch lokálních hodnot je nestabilní metrika a může se lišit v závislosti na změnách v strategii CodeGen. Tato chyba by měla být opravena a uživatelé by měli získat podrobnější a spolehlivý způsob, jak příznak potlačit. 

## <a name="detailed-design"></a>Podrobný návrh

Umožňuje zadat `System.Runtime.CompilerServices.SkipLocalsInitAttribute` jako způsob, jak říct kompilátoru, aby negeneroval `localsinit` příznak.
 
Konečný výsledek této akce bude, že místní hodnoty nemusí být inicializovány kompilátorem JIT, což je ve většině případů nepozorovatelné C#.  
Kromě toho, že `stackalloc` data nebudou inicializována nulou. To je jednoznačně pozorovatelný, ale také je to nejvíce motivující scénář.

Povolené a rozpoznané cíle atributů jsou: `Method`, `Property`, `Module`, `Class`, `Struct`, `Interface`, `Constructor`. Nicméně kompilátor nebude vyžadovat, aby byl tento atribut definován se uvedenými cíli, ani bude mít na starosti sestavení, které je atribut definován. 

Když je atribut zadán v kontejneru (`class`, `module`, obsahující metodu pro vnořenou metodu,...), příznak ovlivňuje všechny metody obsažené v kontejneru.

Syntetizované metody dědí příznak z logického kontejneru nebo vlastníka. 

Příznak ovlivňuje pouze strategii CodeGen pro skutečné tělo metody. t. příznak nemá žádný vliv na abstraktní metody a není šířen na přepsání/implementaci metod.

Toto je explicitně **_funkce kompilátoru_** , **_nikoli funkce jazyka_** .  
Podobně jako u přepínačů příkazového řádku kompilátoru ovládací prvky funkce řídí podrobnosti implementace konkrétní strategie CODEGEN a nemusí být požadovány C# specifikací.

## <a name="drawbacks"></a>Nevýhody
[drawbacks]: #drawbacks

- Staré/jiné kompilátory nesmí respektovat atribut.
Ignorování atributu je kompatibilní s chováním. Výsledkem může být jen mírné dosažení výkonu.

- Kód bez příznaku `localinits` může aktivovat selhání ověřování.
Uživatelé, kteří žádají o tuto funkci, jsou obecně nedotčeny možností ověření. 
 
- Použití atributu na vyšších úrovních, než je individuální metoda, má nelokální efekt, který je možné pozorovat při použití `stackalloc`. Tento scénář je ale nejvíce požadovaný.

## <a name="alternatives"></a>Alternativy
[alternatives]: #alternatives

- vynechání příznaku `localinits`, pokud je metoda deklarovaná v kontextu `unsafe`. To může způsobit, že dojde ke změně tichého a nebezpečného chování z deterministického na nedeterministické v případě `stackalloc`.

- vždy vynechejte příznak `localinits`.
Ještě horší než výše.

- vynechejte příznak `localinits`, pokud se v těle metody nepoužívá `stackalloc`.
Neřeší nejvíc požadovaný scénář a může neověřit kód bez možnosti vrácení zpět.

## <a name="unresolved-questions"></a>Nevyřešené dotazy
[unresolved]: #unresolved-questions

- Měl by se tento atribut ve skutečnosti emitovat do metadat? 

## <a name="design-meetings"></a>Schůzky pro návrh

Žádná ještě není. 