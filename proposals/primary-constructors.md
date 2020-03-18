---
ms.openlocfilehash: 77c9ffda3a59cc5f3dcc3ec9848600c6c5e03eed
ms.sourcegitcommit: 27487fa0294f4cdb7e5f2478884149e05314fd8a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/23/2019
ms.locfileid: "79484970"
---
# <a name="primary-constructors"></a>Primární konstruktory

* Navrženo [x]
* [] Prototyp: Nezahájeno
* [] Implementace: Nezahájeno
* [] Specifikace: Nezahájeno

## <a name="summary"></a>Souhrn
[summary]: #summary

Třídy mohou mít seznam parametrů a v případě, že mají, jejich specifikace základní třídy může mít seznam argumentů.
Parametry primárního konstruktoru jsou v oboru v celé deklaraci třídy, a pokud jsou zachyceny členem funkce nebo anonymní funkcí, jsou uloženy jako soukromá pole ve třídě.

## <a name="motivation"></a>Motivační
[motivation]: #motivation

V kódu inicializace programu je běžné mít spoustu často. V obecném případě je konkrétní část dat `x` uvedena mnohokrát:

- Jako soukromé pole `_x`
- Jako parametr `x` do konstruktoru
- V `_x = x;` přiřazení pole z parametru v konstruktoru
- Jako vlastnost `X`
- V `x = value;` setter vlastnosti
- Ve vlastnosti getter `return x;`

``` c#
class C
{
    private string _x;
    
    public C(string x)
    {
        _x = x;
    }
    public string X
    {
        get => _x;
        set { if (value == null) throw new NullArgumentException(nameof(X)); _x = value; }
    }
}
```

Pro vlastnosti, které nevyžadují ověřování nebo výpočet, se únavných úkolů dá snížit pomocí automatických vlastností, takže je potřeba deklarovat explicitní pole zálohování pro vlastnost. Pokud ale vaše vlastnost vyžaduje jakýkoli druh logiky nad rámec toho, co poskytuje Automatická vlastnost, je výše uvedená nejlepší.

Primární konstruktory místo toho snižují režii tím, že zadáte argumenty konstruktoru přímo v rozsahu celé třídy a znovu už nemusí komplikovaně nutnost explicitně deklarovat pole pro zálohování. Proto by byl výše uvedený příklad následující:

``` c#
class C(string x)
{
    public string X
    {
        get => x;
        set { if (value == null) throw new NullArgumentException(nameof(X)); x = value; }
    }
}
```

V tomto příkladu primární konstruktor snižuje počet pojmenovaných entit pro `x` od tří do dvou a už nemusí komplikovaně pole `_x` zálohování. Odstraní dvě ze tří členských deklarací (zachovají pouze deklaraci vlastnosti) a sníží celkový počet zmínk `x`/`_x`/`X` od osmi do pěti.


## <a name="detailed-design"></a>Podrobný návrh
[design]: #detailed-design

Třídy mohou mít seznam parametrů:

``` c#
public class C(int i, string s)
{
    ...
}
```

Seznam parametrů způsobí, že konstruktor bude implicitně deklarován pro třídu, se stejnou přístupností jako samotná třída.

``` c#
new C(5, "Hello");
```

Parametry primárního konstruktoru jsou v oboru celé tělo třídy. Pokud jsou zachyceny členem funkce nebo anonymní funkcí, budou uloženy jako soukromá pole ve třídě. Pokud jsou používány pouze během inicializace, nebudou uloženy v objektu.

``` c#
public class C(int i, string s)
{
    int[] a = new int[i]; // i not captured
    public int S => s;    // s captured
}
```

Pokud třída s primárním konstruktorem má specifikaci základní třídy, může mít jeden seznam argumentů. Slouží jako seznam argumentů pro `base(...)` inicializátor implicitně deklarovaného konstruktoru. Pokud není zadaný žádný seznam argumentů, předpokládá se prázdný.

``` c#
public class C(int i, string s) : B(s)
{
    ...
}
```
Třída může mít explicitně definované konstruktory, ale všechny musí používat inicializátor `this(...)`. Tím je zajištěno, že primární konstruktor je vždy volán při vytvoření nové instance.

Všechny Inicializátory v těle třídy se stanou přiřazeními ve vygenerovaném konstruktoru. To znamená, že na rozdíl od jiných tříd se Inicializátory spustí *po* vyvolání základního konstruktoru, ne před. Kromě toho generovaná třída bude obsahovat přiřazení pro inicializaci všech privátních polí, která byla vygenerována pro uložení parametrů primárního konstruktoru, které byly zachyceny členy. Tyto členy se přepíší pro použití privátního pole namísto parametru způsobem podobným uzavření pro výrazy lambda. Vygenerovaná primární pole jsou inicializována jako první a poté jsou vygenerovaná Inicializátory spouštěny v pořadí podle vzhledu třídy.

V předchozím příkladu je účinek deklarace třídy podobný jako při přepsání takto:

``` c#
public class C : B
{
    public C(int i, string s) : base(s)
    {
        __s = s;        // store parameter s for captured use
        a = new int[i]; // initialize a
    }
    int __s; // generated field for capture of s
    
    int[] a;
    public int S => __s; // s replaced with captured __s
}
```

Zachycení má podobná omezení pro zachycení místních proměnných pomocí výrazů lambda. Například parametry `ref` a `out` jsou povoleny v primárních konstruktorech, ale nelze je zachytit na své členské subjekty.


## <a name="drawbacks"></a>Nevýhody
[drawbacks]: #drawbacks

V hrubém pořadí podle významnosti.

* Návrh používá syntaxi, která byla navržena také pro poziční záznamy. Pokud si vyžádali obě funkce, je nutné mít nějaká ubytování. Například byl navržený modifikátor `data` u záznamů.
* Velikost alokace konstruovaných objektů je méně zřejmá, protože kompilátor určuje, zda se má přidělit pole pro parametr primárního konstruktoru na základě úplného textu třídy. Toto riziko je podobné implicitnímu zachycení proměnných pomocí výrazů lambda.
* Běžný pokušení (nebo nechtěného vzoru) může být zachytit "stejný" parametr na více úrovních dědičnosti, protože je předán řetězu konstruktoru namísto explicitního allotting IT chráněného pole v základní třídě, což vede k duplicitním přidělením. pro stejná data v objektech. To je velmi podobné jako dnešní riziko přepsání automatických vlastností pomocí automatických vlastností. 
* Jak bylo navrženo výše, neexistuje žádné místo pro další logiku, která by mohla být obvykle vyjádřena v podinstitucích konstruktoru. Přípona "primárního konstruktoru" níže uvedená adresa.
* Jak je navrženo, sémantika pořadí spouštění se neliší od běžných konstruktorů. To může být způsobeno tím, že je to u některých návrhů rozšíření (zejména "primárních" institucí "primárních konstruktorů") náklady.
* Návrh funguje pouze v případě, že jeden konstruktor může být určen jako primární.
* Neexistuje žádný způsob, jak mít samostatnou přístupnost třídy a primárního konstruktoru. Například v situacích, kdy veřejné konstruktory jsou delegovány na jeden privátní konstruktor "Build-All", který by byl potřeba. V případě potřeby může být pro vás navržena syntaxe později.


## <a name="alternatives"></a>Alternativy
[alternatives]: #alternatives

Fulltextové záznamy mohou být alternativou nebo mohou koexistovat s primárními konstruktory v závislosti na konkrétních událostech. Umožňují *více* zkratky v *menším* počtu scénářů. Takže obě jsou potenciálně užitečné, ale obě můžou být přehnaně důkladné, pokud je nemůžete trochu snadno integrovat.


## <a name="possible-extensions"></a>Možná rozšíření
[extensions]: #possible-extensions

Jedná se o variace nebo doplňky základního návrhu, které mohou být zváženy ve spojení s ní, nebo v pozdější fázi, pokud je to považováno za užitečné.

### <a name="primary-constructor-bodies"></a>Základní subjekty konstruktoru

Samotné konstruktory často obsahují logiku ověřování parametrů nebo jiný kód inicializace netriviální, který nelze vyjádřit jako Inicializátory.

Primární konstruktory mohou být rozšířeny, aby bylo možné zobrazit bloky příkazů přímo v těle třídy. Tyto příkazy by byly vloženy do generovaného konstruktoru v místě, kde se zobrazují mezi inicializací přiřazení, a by tedy byly provedeny s použitím inicializátorů. Příklad:

``` c#
public class C(int i, string s) : B(s)
{
    {
        if (i < 0) throw new ArgumentOutOfRangeException(nameof(i));
    }
    int[] a = new int[i];
    public int S => s;
}
```

### <a name="initializer-fields-and-initializer-functions"></a>Inicializační pole a inicializační funkce

Ve třídě s primárním konstruktorem můžeme vzít v úvahu deklarace polí a metod bez modifikátorů dostupnosti tak, aby byly větší jako lokální proměnné a místní funkce:

* Stejně jako u parametrů primárního konstruktoru je "inicializační pole" zachycena pouze v samotném soukromém poli, pokud byly použity v členech Functions.
* "Inicializační funkce" by se měly považovat za zachycení parametrů primárního konstruktoru a polí inicializátoru, pokud byly použity sami v jiných členech funkce. Pokud není zachycena, mohou být vygenerována lépe, například místními funkcemi.
* Stejně jako parametry primárního konstruktoru by nebyly k dispozici prostřednictvím přístupu členů, ale pouze jako jednoduchý název.

To lze použít pro dočasné proměnné a pomocné funkce, které jsou relevantní pouze pro inicializaci:

``` c#
public class C(string s)
{
    int size = s.Length;             // not captured
    int[] Create() => new int[size]; // not captured
    int[] a = Create();
    ...
}
```

To může být příliš jemné, zejména protože absence modifikátorů dostupnosti jinde znamená `private`. 

### <a name="initializer-statements"></a>Příkazy inicializátoru

Kombinace odmocniny výše uvedeného do rozšíření by mohla jednoduše dovolit příkazy přímo v těle třídy. Tyto příkazy jsou přesně stejné jako výše navržené tělo konstruktoru, s tím rozdílem, že není nutné je uzavřít do `{ }`. Aby byla tato možnost dostatečně užitečná, "místní" proměnné a pomocné funkce by musel být také vyhodnotit na nejvyšší úrovni třídy, způsobem Prozkoumávám v části rozšíření "inicializační pole a inicializační funkce" výše.


### <a name="member-access"></a>Přístup ke členům

Základní návrh zpracovává parametry primárního konstruktoru jako parametry, které mohou být pouze označovány jako jednoduché názvy. Alternativou je umožnění odkazů, jako by se jednalo o soukromá pole, tj. s přístupem členů, a *to i* v případě, že se někdy negenerují jako pole. To by jim mohlo být odkazováno jako `this.x`, když jsou Stínově vytvořeny pomocí místních proměnných a jsou k nim přistupované z jiné instance jako `other.x`.

Pokud se použije na rozšíření "pole inicializátoru a inicializační funkce", tím se také sníží míra, se kterou se tyto hodnoty lišily od běžných privátních členů. Jediným rozdílem by pak bylo, že kompilátor je volné, aby je Elide z objektu, pokud se používá pouze během inicializace.

