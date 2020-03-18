---
ms.openlocfilehash: b0d0fa70d90f7493c6c23be576155a77cec36cf8
ms.sourcegitcommit: f61a06970fa0562d2e40363fae3948eb168624ca
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/14/2020
ms.locfileid: "79485180"
---
# <a name="default-interface-methods"></a>výchozí metody rozhraní

* Navrženo [x]
* [] Prototyp: [probíhá](https://github.com/dotnet/roslyn/blob/master/docs/features/DefaultInterfaceImplementation.md)
* [] Implementace: žádné
* [] Specifikace: probíhá, níže

## <a name="summary"></a>Souhrn
[summary]: #summary

Přidání podpory pro _metody virtuálních rozšíření_ – metody v rozhraních s konkrétními implementacemi. Třída nebo struktura, která implementuje takové rozhraní, musí mít jedinou _specifickou_ implementaci pro metodu rozhraní, buď implementovanou třídou nebo strukturou, nebo zděděná z jeho základních tříd nebo rozhraní. Metody virtuálních rozšíření umožňují autorovi rozhraní API přidávat do rozhraní v budoucích verzích metody, aniž by došlo k narušení zdrojové nebo binární kompatibility se stávajícími implementacemi tohoto rozhraní.

Jsou to podobné jako ["výchozí metody"](http://docs.oracle.com/javase/tutorial/java/IandI/defaultmethods.html)Java.

(Na základě pravděpodobnější techniky implementace) Tato funkce vyžaduje odpovídající podporu rozhraní příkazového řádku (CLI) nebo CLR. Programy, které tuto funkci využívají, nemůžou běžet na dřívějších verzích platformy.

## <a name="motivation"></a>Motivační
[motivation]: #motivation

Hlavní motivace pro tuto funkci je

- Výchozí metody rozhraní umožňují autorovi rozhraní API přidávat do rozhraní v budoucích verzích metody, aniž by došlo k narušení zdrojové nebo binární kompatibility se stávajícími implementacemi tohoto rozhraní.
- Tato funkce umožňuje C# vzájemnou spolupráci s rozhraními API cílících na [Android (Java)](http://docs.oracle.com/javase/tutorial/java/IandI/defaultmethods.html) a [iOS (SWIFT)](https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/Protocols.html#//apple_ref/doc/uid/TP40014097-CH25-ID267), která podporují podobné funkce.
- Jak se zapíná, přidání implementací výchozího rozhraní poskytuje prvky funkce jazyka "vlastnosti" (<https://en.wikipedia.org/wiki/Trait_(computer_programming)>). Poukázali se, že se jedná o výkonnou techniku programování (<http://scg.unibe.ch/archive/papers/Scha03aTraits.pdf>).

## <a name="detailed-design"></a>Podrobný návrh
[design]: #detailed-design

Syntaxe pro rozhraní se rozšířila na povolení.

- Deklarace členů, které deklarují konstanty, operátory, statické konstruktory a vnořené typy;
- *tělo* pro metodu nebo indexer, vlastnost nebo přistupující objekt události (tj. "výchozí" implementace);
- Deklarace členů, které deklarují statická pole, metody, vlastnosti, indexery a události;
- Deklarace členů pomocí explicitní syntaxe implementace rozhraní; ani
- Explicitní modifikátory přístupu (výchozí přístup je `public`).

Členové s orgány umožňují, aby rozhraní poskytovalo "výchozí" implementaci pro metodu ve třídách a strukturách, které neposkytují přepsání implementace.

Rozhraní nemůžou obsahovat stav instance. Zatímco statická pole jsou nyní povolena, pole instance nejsou v rozhraních povolena. Automatické vlastnosti instance nejsou v rozhraních podporované, protože by implicitně deklarovaly skryté pole.

Statické a soukromé metody umožňují užitečný refaktoring a organizaci kódu, který slouží k implementaci veřejného rozhraní API rozhraní.

Přepsání metody v rozhraní musí používat explicitní syntaxi implementace rozhraní.

Deklarace typu třídy, typu struktury nebo výčtového typu v rámci oboru parametru typu, který byl deklarován pomocí *variance_annotation*, je chyba.  Například deklarace `C` níže je chyba.

```csharp
interface IOuter<out T>
{
    class C { } // error: class declaration within the scope of variant type parameter 'T'
}
```

### <a name="concrete-methods-in-interfaces"></a>Konkrétní metody v rozhraních

Nejjednodušší forma této funkce je schopnost deklarovat *konkrétní metodu* v rozhraní, což je metoda s tělem.

```csharp
interface IA
{
    void M() { WriteLine("IA.M"); }
}
```

Třída, která implementuje toto rozhraní, nevyžaduje implementaci konkrétní metody.

```csharp
class C : IA { } // OK

IA i = new C();
i.M(); // prints "IA.M"
```

Konečné přepsání `IA.M` ve třídě `C` je konkrétní metoda `M` deklarovaná v `IA`. Všimněte si, že třída nedědí členy z jeho rozhraní; Tato funkce se nezměnila:

```csharp
new C().M(); // error: class 'C' does not contain a member 'M'
```

V rámci instance člena rozhraní `this` má typ ohraničujícího rozhraní.

### <a name="modifiers-in-interfaces"></a>Modifikátory v rozhraních

Syntaxe pro rozhraní je odlehčena umožňující modifikátory pro členy. Jsou povoleny následující: `private`, `protected`, `internal`, `public`, `virtual`, `abstract`, `sealed`, `static`, `extern`a `partial`.

> ***TODO***: Ověřte, jaké další modifikátory existují.

Člen rozhraní, jehož deklarace zahrnuje tělo, je `virtual` člen, pokud se nepoužívá modifikátor `sealed` nebo `private`. Modifikátor `virtual` lze použít u členu funkce, který by byl jinak implicitně `virtual`. Podobně i když `abstract` je výchozím nastavením pro členy rozhraní bez těla, může být tento modifikátor explicitně udělen. Nevirtuální člen může být deklarován pomocí klíčového slova `sealed`.

Jedná se o chybu pro `private` nebo `sealed` členu funkce rozhraní, aby neobsahoval tělo. Člen funkce `private` nesmí mít modifikátor `sealed`.

Modifikátory přístupu lze použít pro členy rozhraní všech druhů členů, kteří jsou povoleni. Výchozím nastavením je `public` úroveň přístupu, ale je možné ji výslovně udělit.

> ***Otevřít problém:*** Musíme přesně určit význam modifikátorů přístupu, jako je `protected` a `internal`, a deklarace a Nepřepisovat je (v odvozeném rozhraní) nebo je implementovat (ve třídě, která implementuje rozhraní).

Rozhraní mohou deklarovat `static` členy, včetně vnořených typů, metod, indexerů, vlastností, událostí a statických konstruktorů. Výchozí úroveň přístupu pro všechny členy rozhraní je `public`.

Rozhraní nemohou deklarovat konstruktory instancí, destruktory a pole.

> ***Uzavřený problém:*** Mají se v rozhraní povolit deklarace operátoru? Pravděpodobně neexistují operátory převodu, ale co se týká dalších? ***Rozhodnutí***: operátory jsou povoleny *s výjimkou* operátorů konverze, rovnosti a nerovnosti.

> ***Uzavřený problém:*** Má `new` být povoleno pro deklarace členů rozhraní, které skrývají členy ze základních rozhraní? ***Rozhodnutí***: Ano.

> ***Uzavřený problém:*** V současné době nepovolujeme `partial` na rozhraní nebo jeho členech. To by vyžadovalo samostatný návrh. ***Rozhodnutí***: Ano. <https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#permit-partial-in-interface>

### <a name="overrides-in-interfaces"></a>Přepsání v rozhraních

Deklarace přepisu (tj. ty, které obsahují modifikátor `override`) umožňují programátorovi poskytnout nejvíce specifickou implementaci virtuálního člena v rozhraní, kde kompilátor nebo modul runtime by jinak nenalezl jeden. Umožňuje také zapnout abstraktního člena z rozhraní Super do výchozího člena v odvozeném rozhraní. Deklarace přepisu je povolená *explicitně* přepsat konkrétní metodu základního rozhraní tím, že je kvalifikována deklarace s názvem rozhraní (v tomto případě není povolený modifikátor přístupu). Implicitní přepsání nejsou povolena.

```csharp
interface IA
{
    void M() { WriteLine("IA.M"); }
}
interface IB : IA
{
    override void IA.M() { WriteLine("IB.M"); } // explicitly named
}
interface IC : IA
{
    override void M() { WriteLine("IC.M"); } // implicitly named
}
```

Deklarace přepisu v rozhraních nesmí být deklarované `sealed`.

Veřejné `virtual` členské funkce v rozhraní mohou být v odvozeném rozhraní přepsány explicitně (kvalifikováním názvu v deklaraci přepsání s typem rozhraní, který původně deklaroval metodu a vynecháním modifikátoru přístupu).

`virtual` členů funkce v rozhraní lze pouze přepsat explicitně (ne implicitně) v odvozených rozhraních a členy, které nejsou `public` mohou být implementovány pouze ve třídě nebo struktuře explicitně (ne implicitně). V obou případech musí být přepsaný nebo implementovaný člen *přístupný* tam, kde je přepsán.

### <a name="reabstraction"></a>Přeabstrakce

Virtuální (konkrétní) metoda deklarovaná v rozhraní může být přepsána tak, aby byla abstraktní v odvozeném rozhraní.

```csharp
interface IA
{
    void M() { WriteLine("IA.M"); }
}
interface IB : IA
{
    abstract void IA.M();
}
class C : IB { } // error: class 'C' does not implement 'IA.M'.
```

Modifikátor `abstract` není vyžadován v deklaraci `IB.M` (což je výchozí hodnota v rozhraních), ale je pravděpodobné, že je v deklaraci přepsání dobrým zvykem explicitní.

To je užitečné v odvozených rozhraních, kde je výchozí implementace metody nevhodná a je třeba poskytnout vhodnější implementaci implementací tříd.

> ***Otevřít problém:*** Má se povolit přeabstrakce?

### <a name="the-most-specific-override-rule"></a>Nejpřesnější pravidlo přepsání

Vyžadujeme, aby každé rozhraní a třída měly *nejvíce specifické přepsání* pro všechny virtuální členy v rámci přepsání, která se nacházejí v typu nebo v jeho přímých a nepřímých rozhraních. Nejpřesnější *přepsání* je jedinečné přepsání, které je specifičtější než každé jiné přepsání. Pokud nedojde k žádnému přepsání, považuje se samotný člen za nejpřesnější přepsání.

Jedno přepsání `M1` je považováno za *konkrétnější* než jiné přepsání `M2` pokud je `M1` deklarováno v typu `T1`, `M2` je deklarováno v typu `T2`a buď

1. `T1` obsahuje `T2` z jeho přímých nebo nepřímých rozhraní nebo
2. `T2` je typ rozhraní, ale `T1` není typu rozhraní.

Příklad:

```csharp
interface IA
{
    void M() { WriteLine("IA.M"); }
}
interface IB : IA
{
    void IA.M() { WriteLine("IB.M"); }
}
interface IC : IA
{
    void IA.M() { WriteLine("IC.M"); }
}
interface ID : IB, IC { } // error: no most specific override for 'IA.M'
abstract class C : IB, IC { } // error: no most specific override for 'IA.M'
abstract class D : IA, IB, IC // ok
{
    public abstract void M();
}

```

Nejpřesnější pravidlo přepsání zajišťuje konflikt (tj. nejednoznačnost vyplývající z dědičnosti kosočtverce), která je explicitně řešena programátorem v místě, kde nastane konflikt.

Protože podporujeme explicitní abstraktní přepsání v rozhraních, můžeme to udělat i ve třídách.

```csharp
abstract class E : IA, IB, IC // ok
{
    abstract void IA.M();
}
```

> ***Otevřít problém***: by měla podpora explicitního přepsání rozhraní v třídách?

Kromě toho se jedná o chybu, pokud je v deklaraci třídy nejvíce specifické přepsání některé metody rozhraní abstraktním přepsáním, které bylo deklarováno v rozhraní. Toto pravidlo se přestavuje pomocí nové terminologie.

```csharp
interface IF
{
    void M();
}
abstract class F : IF { } // error: 'F' does not implement 'IF.M'
```

Je možné, že virtuální vlastnost deklarovaná v rozhraní má nejvíce konkrétní přepsání pro svůj přistupující objekt `get` v jednom rozhraní a nejvíce specifické přepsání pro svůj přistupující objekt `set` v jiném rozhraní. To je považováno za porušení *nejvíce specifického pravidla přepsání* .

### <a name="static-and-private-methods"></a>metody `static` a `private`

Vzhledem k tomu, že rozhraní mohou nyní obsahovat spustitelný kód, je užitečné abstraktní společný kód do privátních a statických metod. Nyní je v rozhraních povolujeme.

> ***Uzavřený problém***: máme podporovat privátní metody? Podporujeme statické metody? **Rozhodnutí: Ano**

> ***Otevření problému***: mám povolit, aby byly metody rozhraní `protected` nebo `internal` nebo jiný přístup? Pokud ano, jaké jsou sémantiky? Jsou ve výchozím nastavení `virtual`? Pokud ano, existuje způsob, jak je nastavit jako nevirtuální?

> ***Otevření problému***: Pokud podporujeme statické metody, budeme podporovat (statické) operátory?

### <a name="base-interface-invocations"></a>Volání základního rozhraní

Kód v typu, který je odvozen z rozhraní s výchozí metodou, může explicitně vyvolat implementaci "Base" tohoto rozhraní.

```csharp
interface I0
{
   void M() { Console.WriteLine("I0"); }
}
interface I1 : I0
{
   override void M() { Console.WriteLine("I1"); }
}
interface I2 : I0
{
   override void M() { Console.WriteLine("I2"); }
}
interface I3 : I1, I2
{
   // an explicit override that invoke's a base interface's default method
   void I0.M() { I2.base.M(); }
}

```

Metoda instance (nestatická) má povoleno vyvolat implementaci přístupné metody instance přímo v přímém základním rozhraní, a to tak, že je pojmenovaná pomocí `base(Type).M`syntaxe. To je užitečné v případě, že přepsání, které je nutné poskytnout kvůli dědění kosočtverce, je vyřešeno delegováním na jednu konkrétní základní implementaci.

```csharp
interface IA
{
    void M() { WriteLine("IA.M"); }
}
interface IB : IA
{
    override void IA.M() { WriteLine("IB.M"); }
}
interface IC : IA
{
    override void IA.M() { WriteLine("IC.M"); }
}

class D : IA, IB, IC
{
    void IA.M() { base(IB).M(); }
}
```

Je-li k `virtual` nebo `abstract` členem pomocí `base(Type).M`syntaxe, je nutné, aby `Type` obsahuje jedinečné *přepsání* pro `M`.

### <a name="binding-base-clauses"></a>Klauzule Binding Base

Rozhraní nyní obsahují typy.  Tyto typy mohou být použity v základní klauzuli jako základní rozhraní.  Při vázání základní klauzule můžeme potřebovat znát sadu základních rozhraní, aby tyto typy navázala (např. aby je bylo možné vyhledat a vyřešit chráněný přístup).  Význam základní klauzule rozhraní je proto cyklicky definován.  Pro přerušení cyklu přidáme nová jazyková pravidla odpovídající podobnému pravidlu, které už je pro třídy zavedené.

Při určování významu *interface_base* rozhraní jsou základní rozhraní dočasně považována za prázdná. Intuitivní to zajistí, že význam základní klauzule nemůže rekurzivně záviset sám na sobě. 

**Použili jsme následující pravidla:**

"Je-li třída B odvozena od třídy A, jedná se o chybu při kompilaci, která je v době kompilace závislá na B. Třída **přímo závisí na** své přímé základní třídě (pokud existuje) a **přímo závisí na** ~~**třídě**~~ , ve které je okamžitě vnořena (pokud existuje). V rámci této definice je kompletní sada ~~**tříd**~~ , na kterých je třída závislá, výraz reflexivní a přenosného uzavření **přímo závislý na** vztahu. "

Jedná se o chybu při kompilaci, která rozhraní přímo nebo nepřímo dědí ze samotného.
**Základní rozhraní** rozhraní jsou explicitní základní rozhraní a jejich základní rozhraní. Jinými slovy, sada základních rozhraní je kompletní přenosová uzávěra explicitního základního rozhraní, jejich explicitní základní rozhraní a tak dále.

**Upravujeme je takto:**

Je-li třída B odvozena od třídy A, jedná se o chybu při kompilaci, která je v době kompilace závislá na B. Třída **přímo závisí na** své přímé základní třídě (pokud existuje) a **přímo závisí na** _**typu**_ , ve kterém je okamžitě vnořen (pokud existuje).

Když rozhraní IB rozšiřuje rozhraní IA, jedná se o chybu při kompilaci, která má za to, že má pro platformu IA záviset na IB. Rozhraní **přímo závisí na** svých přímých základních rozhraních (pokud existuje) a **přímo závisí na** typu, ve kterém je okamžitě vnořen (pokud existuje).

S ohledem na tyto definice je kompletní sada **typů** , na jejichž základě je typ závislý, je reflexivní a přenosová uzávěrka **přímo závislá na** vztahu.

### <a name="effect-on-existing-programs"></a>Vliv na existující programy

Tady uvedená pravidla mají mít žádný vliv na význam stávajících programů.

Příklad 1:

```csharp
interface IA
{
    void M();
}
class C: IA // Error: IA.M has no concrete most specific override in C
{
    public static void M() { } // method unrelated to 'IA.M' because static
}
```

Příklad 2:

```csharp
interface IA
{
    void M();
}
class Base: IA
{
    void IA.M() { }
}
class Derived: Base, IA // OK, all interface members have a concrete most specific override
{
    private void M() { } // method unrelated to 'IA.M' because private
}
```

Stejná pravidla poskytují podobné výsledky pro podobnou situaci zahrnující výchozí metody rozhraní:

```csharp
interface IA
{
    void M() { }
}
class Derived: IA // OK, all interface members have a concrete most specific override
{
    private void M() { } // method unrelated to 'IA.M' because private
}
```

> ***Uzavřený problém***: potvrďte, že se jedná o zamýšlené pořadí specifikace. **Rozhodnutí: Ano**

### <a name="runtime-method-resolution"></a>Rozlišení metod za běhu

> ***Uzavřený problém:*** Specifikace by měla popsat algoritmus rozlišení metody modulu runtime na obličej výchozích metod rozhraní. Musíme zajistit, aby sémantika byla konzistentní s sémantikou jazyka, např. které deklarované metody dělají a nepřepisují ani neimplementují metodu `internal`.

### <a name="clr-support-api"></a>Rozhraní API podpory CLR

Aby mohly kompilátory detekovat při kompilaci pro modul runtime, který podporuje tuto funkci, knihovny pro tyto moduly jsou upraveny tak, aby tuto skutečnost inzerovaly prostřednictvím rozhraní API, které je popsáno v <https://github.com/dotnet/corefx/issues/17116>. Přidám

```csharp
namespace System.Runtime.CompilerServices
{
    public static class RuntimeFeature
    {
        // Presence of the field indicates runtime support
        public const string DefaultInterfaceImplementation = nameof(DefaultInterfaceImplementation);
    }
}
```

> ***Otevřít problém***: je to nejlepší název funkce *CLR* ? Funkce CLR má mnohem víc, než jenom to (např. uvolnit omezení ochrany, podporuje přepsání v rozhraních atd.). Možná by se mělo zavolat na konkrétní metody v rozhraních nebo na vlastnosti?

### <a name="further-areas-to-be-specified"></a>Další oblasti, které se mají zadat

- [] Bylo užitečné zařadit do katalogu druhy zdrojových a binárních vlivů kompatibility způsobené přidáním výchozích metod rozhraní a přepsáním existujícím rozhraním.

## <a name="drawbacks"></a>Nevýhody
[drawbacks]: #drawbacks

Tento návrh vyžaduje koordinovanou aktualizaci specifikace CLR (pro podporu konkrétních metod v rozhraních a řešení metody). Proto je poměrně "nákladný" a může se jednat o kombinaci s jinými funkcemi, které také předpokládáme, že by to vyžadovalo změny CLR.

## <a name="alternatives"></a>Alternativy
[alternatives]: #alternatives

Žádné.

## <a name="unresolved-questions"></a>Nevyřešené dotazy
[unresolved]: #unresolved-questions

- Otevřené otázky se zavolají v celém návrhu výše.
- Seznam otevřených otázek najdete také <https://github.com/dotnet/csharplang/issues/406>.
- Aby bylo možné vybrat přesnou metodu, která má být vyvolána, musí být popsána Podrobná specifikace.
- Interakce metadat vyprodukovaných novými kompilátory a spotřebovaná staršími kompilátory musí být podrobně využívány. Musíme například zajistit, aby reprezentace metadat, kterou používáme, nezpůsobí přidání výchozí implementace v rozhraní pro přerušení existující třídy, která implementuje toto rozhraní při kompilování starším kompilátorem. To může mít vliv na reprezentace metadat, kterou můžeme použít.
- Návrh musí zvážit vzájemnou operaci s jinými jazyky a existujícími kompilátory pro jiné jazyky.

## <a name="resolved-questions"></a>Vyřešené otázky

### <a name="abstract-override"></a>Abstraktní přepsání

Předchozí koncept specifikace obsahoval možnost "reabstract" zděděné metody:

```csharp
interface IA
{
    void M();
}
interface IB : IA
{
    override void M() { }
}
interface IC : IB
{
    override void M(); // make it abstract again
}
```

Moje poznámky pro 2017-03-20 ukázaly, že jsme se rozhodli tuto možnost nedovolit. Existují však alespoň dva případy použití:

1. Rozhraní API Java, se kterými se někteří uživatelé této funkce chtějí vzájemně pracovat, závisí na tomto zařízení.
2. Z tohoto hlediska se výhody pro programování s *vlastnostmi* . Reabstrakce je jedním z prvků funkce jazyka "vlastnosti" (https://en.wikipedia.org/wiki/Trait_(computer_programming)). S třídami jsou povoleny následující:

```csharp
public abstract class Base
{
    public abstract void M();
}
public abstract class A : Base
{
    public override void M() { }
}
public abstract class B : A
{
    public override abstract void M(); // reabstract Base.M
}
```

Tento kód bohužel nejde Refaktorovat jako sadu rozhraní (vlastností), pokud to není povolené. *Jared principem Greed*by měla být povolená.

> ***Uzavřený problém:*** Má se povolit přeabstrakce? ODPOVÍ Moje poznámky byly chybné. [Poznámky LDM](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-03-21.md) říká, že v rozhraní je povolena přeabstrakce. Není ve třídě.

### <a name="virtual-modifier-vs-sealed-modifier"></a>Modifikátor Virtual vs – zapečetěný modifikátor

Z [Aleksey Tsingauz](https://github.com/AlekseyTs):

> Rozhodli jste se povolit modifikátory explicitně uvedené u členů rozhraní, pokud není důvod zakázat některé z nich. Tím se přinese zajímavá otázka kolem virtuální modifikátoru. Má se vyžadovat u členů s výchozí implementací?
>
> Můžeme říct, že:
>
> - Pokud není určena žádná implementace ani žádná z nich ani žádná z nich ani virtuální ani žádná z nich není zapečetěná, předpokládáme, že je člen abstraktní
> - Pokud se nezadá žádná implementace, ani žádná abstraktní ani zapečetěná, předpokládáme, že člen je virtuální.
> - k vytvoření metody ani virtuální ani abstraktní je vyžadován zapečetěný modifikátor.
>
> Alternativně můžeme říct, že virtuálnímu členu je vyžadován modifikátor Virtual. To znamená, že pokud je člen s implementací explicitně označen jako virtuální modifikátor, není to ani virtuální ani abstraktní. Tento přístup může poskytovat lepší možnosti při přesunu metody z třídy na rozhraní:
>
> - abstraktní metoda zůstává abstraktní.
> - virtuální metoda zůstává virtuální.
> - Metoda bez modifikátoru zůstane ani virtuální ani abstraktní.
> - zapečetěný modifikátor nelze použít pro metodu, která není přepsána.
>
> Jak to myslíš?

> ***Uzavřený problém:*** Měla by být konkrétní metoda (s implementací) implicitně `virtual`? ODPOVÍ

***Rozhodnutí:*** Provedená v rámci LDM 2017-04-05:

1. nevirtuální by se měly explicitně vyjádřit prostřednictvím `sealed` nebo `private`.
2. `sealed` je klíčové slovo pro členy instance rozhraní s nevirtuálními institucemi.
3. Chceme v rozhraních zakázat všechny modifikátory.  
4. Výchozí přístupnost pro členy rozhraní je veřejná, včetně vnořených typů.
5. členy soukromých funkcí v rozhraních jsou implicitně zapečetěné a `sealed` nejsou na nich povolena.
6. Soukromé třídy (v rozhraních) jsou povolené a můžou být zapečetěné a to znamená, že jsou zapečetěné ve smyslu třídy Sealed.
7. Chybějící dobrý návrh, u rozhraní nebo jejich členů stále není povolena částečná.

### <a name="binary-compatibility-1"></a>Binární kompatibilita 1

Když knihovna poskytuje výchozí implementaci

```csharp
interface I1
{
    void M() { Impl1 }
}
interface I2 : I1
{
}
class C : I2
{
}
```

Chápeme, že implementace `I1.M` v `C` je `I1.M`. Co když je sestavení obsahující `I2` změněno následujícím způsobem a znovu zkompilováno

```csharp
interface I2 : I1
{
    override void M() { Impl2 }
}
```

ale `C` není znovu zkompilován. Co se stane, když se program spustí? Vyvolání `(C as I1).M()`

1. Spuštění `I1.M`
2. Spuštění `I2.M`
3. Vyvolá určitý druh běhové chyby.

***Rozhodnutí:*** Provedená 2017-04-11: spustí `I2.M`, což je nejednoznačná nejvíc specifická přepsání za běhu.

### <a name="event-accessors-closed"></a>Přístupové objekty událostí (uzavřeno)

> ***Uzavřený problém:*** Je možné událost přepsat "konstantní"?

Vezměte v úvahu tento případ:

```csharp
public interface I1
{
    event T e1;
}
public interface I2 : I1
{
    override event T
    {
        add { }
        // error: "remove" accessor missing
    }
}
```

Tato "částečná" implementace události není povolená, protože, jako ve třídě, syntaxe pro deklaraci události nepovoluje jenom jeden přistupující objekt; musí být zadán obojí (nebo ani ani). Stejnou věc můžete dosáhnout tím, že v syntaxi povolíte abstraktní odebrání přístupového objektu, který by měl být implicitně abstraktní nepřítomností těla:

```csharp
public interface I1
{
    event T e1;
}
public interface I2 : I1
{
    override event T
    {
        add { }
        remove; // implicitly abstract
    }
}
```

Všimněte si, že *se jedná o novou (navrhovanou) syntaxi*. V aktuální gramatice mají přistupující objekty události povinný text.

> ***Uzavřený problém:*** Může být přístup k události (implicitně) abstraktní vynechání těla, podobně jako metody v rozhraních a přistupující objekty vlastnosti (implicitně) abstraktní vynásobením těla?

***Rozhodnutí:*** (2017-04-18) ne, deklarace událostí vyžadují konkrétní přistupující objekty (nebo žádné).

### <a name="reabstraction-in-a-class-closed"></a>Reabstrakce ve třídě (uzavřená)

***Uzavřený problém:*** Měli byste potvrdit, že je povolený (jinak přidání výchozí implementace by to byla zásadní změna):

```csharp
interface I1
{
    void M() { }
}
abstract class C : I1
{
    public abstract void M(); // implement I1.M with an abstract method in C
}
```

***Rozhodnutí:*** (2017-04-18) Ano, přidáním těla do deklarace člena rozhraní byste neměli přerušit C.

### <a name="sealed-override-closed"></a>Zapečetěné přepsání (uzavřeno)

Předchozí otázka implicitně předpokládá, že modifikátor `sealed` lze použít pro `override` v rozhraní. To je v rozporu se specifikací konceptu. Chceme, aby bylo možné zapečetění přepsat? Měly by se brát v úvahu zdrojové a binární účinky na zajištění kompatibility.

> ***Uzavřený problém:*** Je potřeba, aby bylo přepsání zapečetěné?

***Rozhodnutí:*** (2017-04-18) pro přepsání v rozhraních se nepovoluje `sealed`. Jediným použitím `sealed` u členů rozhraní je učinit nevirtuální v počáteční deklaraci.

### <a name="diamond-inheritance-and-classes-closed"></a>Dědičnost a třídy Diamond (uzavřeno)

Koncept návrhu ve scénářích dědičnosti Diamond preferuje přepsání třídy na přepsání rozhraní:

> Vyžadujeme, aby každé rozhraní a třída měly *nejvíce specifické přepsání* pro každou metodu rozhraní mezi přepsáními v typu nebo přímo a nepřímými rozhraními. Nejpřesnější *přepsání* je jedinečné přepsání, které je specifičtější než každé jiné přepsání. Pokud není žádné přepsání, samotná metoda je považována za nejvíce specifická přepsání.
>
> Jedno přepsání `M1` je považováno za *konkrétnější* než jiné přepsání `M2` pokud je `M1` deklarováno v typu `T1`, `M2` je deklarováno v typu `T2`a buď
>
> 1. `T1` obsahuje `T2` z jeho přímých nebo nepřímých rozhraní nebo
> 2. `T2` je typ rozhraní, ale `T1` není typu rozhraní.

Scénář je tento

```csharp
interface IA
{
    void M();
}
interface IB : IA
{
    override void M() { WriteLine("IB"); }
}
class Base : IA
{
    void IA.M() { WriteLine("Base"); }
}
class Derived : Base, IB // allowed?
{
    static void Main()
    {
        Ia a = new Derived();
        a.M();           // what does it do?
    }
}
```

Toto chování je potřeba potvrdit (nebo rozhodnout jinak).

> ***Uzavřený problém:*** Potvrďte výše uvedenou specifikaci konceptu pro *většinu specifických přepsání* , protože platí pro smíšené třídy a rozhraní (třída má přednost před rozhraním). Viz třída <https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-19.md#diamonds-with-classes>.

### <a name="interface-methods-vs-structs-closed"></a>Metody rozhraní vs struktury (uzavřeno)

Existují některé interakce unfortunate mezi výchozími metodami rozhraní a strukturami.

```csharp
interface IA
{
    public void M() { }
}
struct S : IA
{
}
```

Všimněte si, že členové rozhraní nejsou děděni:

```csharp
var s = default(S);
s.M(); // error: 'S' does not contain a member 'M'
```

V důsledku toho musí klient začlenit strukturu pro vyvolání metod rozhraní.

```csharp
IA s = default(S); // an S, boxed
s.M(); // ok
```

Zabalení tímto způsobem se zabývalo základními výhodami typu `struct`. Kromě toho všechny metody mutace nebudou mít žádný zjevné účinek, protože fungují na *zabalenou kopii* struktury:

```csharp
interface IB
{
    public void Increment() { P += 1; }
    public int P { get; set; }
}
struct T : IB
{
    public int P { get; set; } // auto-property
}

T t = default(T);
Console.WriteLine(t.P); // prints 0
(t as IB).Increment();
Console.WriteLine(t.P); // prints 0
```

> ***Uzavřený problém:*** Co se dá dělat na tomto:
>
> 1. Zakazuje `struct` dědění výchozí implementace. Všechny metody rozhraní se budou považovat za abstraktní ve `struct`. Pak můžeme chvíli trvat, než se rozhodnete, jak je lépe pracovat.
> 2. Přináší se nějaký druh strategie generování kódu, který zabraňuje zabalení. V rámci metody, jako je například `IB.Increment`, typ `this` by mohl být podobají na parametr typu omezený na `IB`. V kombinaci s tímto rozhraním, aby se zabránilo zabalení v volajícím, byly neabstraktní metody děděny z rozhraní. To může výrazně zvýšit fungování kompilátoru a implementace CLR.
> 3. Nedělejte si o tom starosti a stačí ho opustit jako Wart.
> 4. Další nápady?

***Rozhodnutí:*** Nedělejte si o tom starosti a stačí ho opustit jako Wart. Viz třída <https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-19.md#structs-and-default-implementations>.

### <a name="base-interface-invocations-closed"></a>Vyvolání základního rozhraní (uzavřeno)

Specifikace konceptu navrhuje syntaxi pro vyvolání základního rozhraní nechte inspirovat pomocí jazyka Java: `Interface.base.M()`. Musíme vybrat syntaxi minimálně pro počáteční prototyp. Moje oblíbená položka je `base<Interface>.M()`.

> ***Uzavřený problém:*** Jaká je syntaxe pro vyvolání základního člena?

***Rozhodnutí:*** Syntaxe je `base(Interface).M()`. Viz třída <https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-19.md#base-invocation>. Rozhraní, které má být pojmenováno, musí být základní rozhraní, ale nemusí se jednat o přímé základní rozhraní.

> ***Otevřít problém:*** Mají být pro členy třídy povolené vyvolání základního rozhraní?

***Rozhodnutí***: Ano. <https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-19.md#base-invocation>

### <a name="overriding-non-public-interface-members-closed"></a>Přepisování členů neveřejných rozhraní (uzavřeno)

V rozhraní jsou neveřejné členy ze základních rozhraní přepsány pomocí modifikátoru `override`. Pokud se jedná o "explicitní" přepsání, které obsahuje název rozhraní obsahujícího člena, je tento modifikátor přístupu vynechán.

> ***Uzavřený problém:*** Pokud se jedná o "implicitní" přepsání, které nejmenuje rozhraní, musí modifikátor přístupu odpovídat?

***Rozhodnutí:*** Pouze veřejné členy mohou být implicitně přepsány a přístup se musí shodovat. Viz třída <https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-18.md#dim-implementing-a-non-public-interface-member-not-in-list>.

> ***Otevřít problém:*** Je modifikátor přístupu požadován, je volitelný nebo vynechán v explicitním přepsání, například `override void IB.M() {}`?

> ***Otevřít problém:*** Je `override` požadováno, volitelné nebo vynechání explicitního přepsání, jako je například `void IB.M() {}`?

Jak provede jedna implementace neveřejného člena rozhraní ve třídě? Možná to musíte udělat explicitně?

```csharp
interface IA
{
    internal void MI();
    protected void MP();
}
class C : IA
{
    // are these implementations?
    internal void MI() {}
    protected void MP() {}
}
```

> ***Uzavřený problém:*** Jak provede jedna implementace neveřejného člena rozhraní ve třídě?

***Rozhodnutí:*** Neveřejné členy rozhraní lze implementovat pouze explicitně. Viz třída <https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-18.md#dim-implementing-a-non-public-interface-member-not-in-list>.

***Rozhodnutí***: pro členy rozhraní není povolené žádné klíčové slovo `override`. <https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#does-an-override-in-an-interface-introduce-a-new-member>

### <a name="binary-compatibility-2-closed"></a>Binární kompatibilita 2 (uzavřeno)

Vezměte v úvahu následující kód, ve kterém je každý typ v samostatném sestavení.

```csharp
interface I1
{
    void M() { Impl1 }
}
interface I2 : I1
{
    override void M() { Impl2 }
}
interface I3 : I1
{
}
class C : I2, I3
{
}
```

Chápeme, že implementace `I1.M` v `C` je `I2.M`. Co když je sestavení obsahující `I3` změněno následujícím způsobem a znovu zkompilováno

```csharp
interface I3 : I1
{
    override void M() { Impl3 }
}
```

ale `C` není znovu zkompilován. Co se stane, když se program spustí? Vyvolání `(C as I1).M()`

1. Spuštění `I1.M`
2. Spuštění `I2.M`
3. Spuštění `I3.M`
4. Buď 2 nebo 3, deterministické
5. Vyvolá určitý druh běhové výjimky.

***Rozhodnutí***: vyvolejte výjimku (5). Viz třída <https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#issues-in-default-interface-methods>.

### <a name="permit-partial-in-interface-closed"></a>Povolit `partial` v rozhraní? ukončit

Vzhledem k tomu, že rozhraní mohou být použita v různých způsobech podobných způsobu použití abstraktních tříd, může být užitečné je deklarovat `partial`. To by bylo obzvlášť užitečné na tváři generátorů.

> ***Návrh:*** Odeberte omezení jazyka, že rozhraní a členové rozhraní nemohou být deklarovány `partial`.

***Rozhodnutí***: Ano. Viz třída <https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#permit-partial-in-interface>.

### <a name="main-in-an-interface-closed"></a>`Main` v rozhraní? ukončit

> ***Otevřít problém:*** Je `static Main` metodou v rozhraní, aby kandidát byl vstupním bodem programu?

***Rozhodnutí***: Ano. Viz třída <https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#main-in-an-interface>.

### <a name="confirm-intent-to-support-public-non-virtual-methods-closed"></a>Potvrdit záměr podporovat veřejné nevirtuální metody (uzavřeno)

Můžete si toto rozhodnutí potvrdit (nebo obrátit), aby bylo možné v rozhraní povolit nevirtuální veřejné metody?

```csharp
interface IA
{
    public sealed void M() { }
}
```

> ***Částečně uzavřený problém:*** (2017-04-18) Domníváme se, že bude užitečný, ale vrátí se k němu. Toto je Trip blok s duševním modelem.

***Rozhodnutí***: Ano. <https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#confirm-that-we-support-public-non-virtual-methods>.

### <a name="does-an-override-in-an-interface-introduce-a-new-member-closed"></a>Zavádí `override` v rozhraní nového člena? ukončit

Existuje několik způsobů, jak sledovat, zda deklarace přepsání zavádí nového člena nebo nikoli.

```csharp
interface IA
{
    void M(int x) { }
}
interface IB : IA
{
    override void M(int y) { }
}
interface IC : IB
{
    static void M2()
    {
        M(y: 3); // permitted?
    }
    override void IB.M(int z) { } // permitted? What does it override?
}
```

> ***Otevřít problém:*** Zavádí deklarace přepsání v rozhraní nového člena? ukončit

Ve třídě je přepsání metody "Visible" v některém smyslu. Například názvy jeho parametrů mají přednost před názvy parametrů v přepsané metodě. Může být možné duplikovat toto chování v rozhraní, protože existuje vždy konkrétní přepsání. Ale chceme toto chování duplikovat?

Je také možné přepsat metodu přepsání? [Moot]

***Rozhodnutí***: pro členy rozhraní není povolené žádné klíčové slovo `override`. <https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#does-an-override-in-an-interface-introduce-a-new-member>.

### <a name="properties-with-a-private-accessor-closed"></a>Vlastnosti s privátním přístupovým objektem (uzavřeno)

Říkáme, že soukromé členy nejsou virtuální a že není povolená kombinace virtuálních a privátních. Ale jak vlastnost s privátním přístupovým objektem?

```csharp
interface IA
{
    public virtual int P
    {
        get => 3;
        private set => { }
    }
}
```

Je to povoleno? Je zde přistupující objekt `set` `virtual` nebo ne? Dá se přepsat tam, kde je k dispozici? Implementuje následující implicitně pouze přistupující objekt `get`?

```csharp
class C : IA
{
    public int P
    {
        get => 4;
        set { }
    }
}
```

Je následující předpokládaná chyba, protože IA. P. set není Virtual a také proto, že není přístupný?

```csharp
class C : IA
{
    int IA.P
    {
        get => 4;
        set { }
    }
}
```

***Rozhodnutí***: první příklad vypadá jako platný, zatímco poslední ne. To se vyřeší obdobně, jak už to funguje C#. <https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#properties-with-a-private-accessor>

### <a name="base-interface-invocations-round-2-closed"></a>Vyvolání základního rozhraní, Round 2 (uzavřeno)

Naše předchozí "řešení", jak zpracovávat základní volání, ve skutečnosti neposkytují dostatečné expresivity. Zapíná, že v C# a CLR, na rozdíl od jazyka Java, je nutné zadat rozhraní obsahující deklaraci metody a umístění implementace, kterou chcete vyvolat.

Navrhuji následující syntaxi pro základní volání v rozhraních. Nejsem s ním rádi, ale ukazuje, co by syntaxe měla být schopná vyjádřit:

```csharp
interface I1 { void M(); }
interface I2 { void M(); }
interface I3 : I1, I2 { void I1.M() { } void I2.M() { } }
interface I4 : I1, I2 { void I1.M() { } void I2.M() { } }
interface I5 : I3, I4
{
    void I1.M()
    {
        base<I3>(I1).M(); // calls I3's implementation of I1.M
        base<I4>(I1).M(); // calls I4's implementation of I1.M
    }
    void I2.M()
    {
        base<I3>(I2).M(); // calls I3's implementation of I2.M
        base<I4>(I2).M(); // calls I4's implementation of I2.M
    }
}
```

Pokud nedochází k nejednoznačnosti, můžete ho napsat snadněji.

```csharp
interface I1 { void M(); }
interface I3 : I1 { void I1.M() { } }
interface I4 : I1 { void I1.M() { } }
interface I5 : I3, I4
{
    void I1.M()
    {
        base<I3>.M(); // calls I3's implementation of I1.M
        base<I4>.M(); // calls I4's implementation of I1.M
    }
}
```

Nebo

```csharp
interface I1 { void M(); }
interface I2 { void M(); }
interface I3 : I1, I2 { void I1.M() { } void I2.M() { } }
interface I5 : I3
{
    void I1.M()
    {
        base(I1).M(); // calls I3's implementation of I1.M
    }
    void I2.M()
    {
        base(I2).M(); // calls I3's implementation of I2.M
    }
}
```

Nebo

```csharp
interface I1 { void M(); }
interface I3 : I1 { void I1.M() { } }
interface I5 : I3
{
    void I1.M()
    {
        base.M(); // calls I3's implementation of I1.M
    }
}
```

***Rozhodnutí***: rozhoduje o `base(N.I1<T>).M(s)`, že v případě, že máme vazbu vyvolání, může k problému docházet později. <https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-11-14.md#default-interface-implementations>

### <a name="warning-for-struct-not-implementing-default-method-closed"></a>Upozornění pro strukturu, která neimplementuje výchozí metodu? ukončit

@vancem výrazy, které bychom měli vážně zvážit, pokud deklarace typu hodnoty nedokáže přepsat určitou metodu rozhraní, a to i v případě, že by zdědily implementaci této metody z rozhraní. Protože způsobuje zabalení omezené voláním.

***Rozhodnutí***: Zdá se, že je něco víc vhodné pro analyzátor. Zdá se také, že toto upozornění může být vysoké, protože by mohlo být v případě, že není nikdy volána metoda výchozí rozhraní a nebude k dispozici žádný zabalení. <https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#warning-for-struct-not-implementing-default-method>

### <a name="interface-static-constructors-closed"></a>Statické konstruktory rozhraní (uzavřené)

Kdy se spouštějí statické konstruktory rozhraní?  Aktuální koncept rozhraní příkazového řádku navrhuje, aby k němu došlo při použití první statické metody nebo pole. Pokud žádná z těchto možností není, nemusí se nikdy spustit??

[2018-10-09 tým CLR navrhuje "Přejít na zrcadlo, co dělajíme u ValueType (zjištění cctor přístupu ke každé metodě instance)"]

***Rozhodnutí***: statické konstruktory jsou také spouštěny při vstupu do metod instance, pokud nebyl statický konstruktor `beforefieldinit`, v tomto případě jsou před přístupem k prvnímu statickému poli spouštěny statické konstruktory. <https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#when-are-interface-static-constructors-run>

## <a name="design-meetings"></a>Schůzky pro návrh

[2017-03-08 poznámky](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-03-08.md) ke schůzce ldm
[2017-03-21. poznámky ke schůzce LDM](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-03-21.md)
[2017-03-23 "chování CLR pro výchozí metody rozhraní"](https://github.com/dotnet/csharplang/blob/master/meetings/2017/CLR-2017-03-23.md)
[poznámky ke schůzce 2017-04-05 LDM](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-05.md)
[2017-04-11](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-11.md) [. poznámky](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-05-17.md) ke schůzce pro schůzku s platformou [LDM
](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-18.md) 2017-04-18 [. poznámky ke](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-05-31.md) schůzce [pro schůzku LDM](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-19.md)
[2017-04-19 LDM Poznámky](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-06-14.md) ke schůzce
[2018-10-17 poznámky](https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md) ke schůzce LDM
[2018-11-14. poznámky ke schůzce LDM](https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-11-14.md)



