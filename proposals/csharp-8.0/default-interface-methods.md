---
ms.openlocfilehash: b0d0fa70d90f7493c6c23be576155a77cec36cf8
ms.sourcegitcommit: f61a06970fa0562d2e40363fae3948eb168624ca
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/14/2020
ms.locfileid: "79485180"
---
# <a name="default-interface-methods"></a><span data-ttu-id="cde0e-101">výchozí metody rozhraní</span><span class="sxs-lookup"><span data-stu-id="cde0e-101">default interface methods</span></span>

* <span data-ttu-id="cde0e-102">Navrženo [x]</span><span class="sxs-lookup"><span data-stu-id="cde0e-102">[x] Proposed</span></span>
* <span data-ttu-id="cde0e-103">[] Prototyp: [probíhá](https://github.com/dotnet/roslyn/blob/master/docs/features/DefaultInterfaceImplementation.md)</span><span class="sxs-lookup"><span data-stu-id="cde0e-103">[ ] Prototype: [In progress](https://github.com/dotnet/roslyn/blob/master/docs/features/DefaultInterfaceImplementation.md)</span></span>
* <span data-ttu-id="cde0e-104">[] Implementace: žádné</span><span class="sxs-lookup"><span data-stu-id="cde0e-104">[ ] Implementation: None</span></span>
* <span data-ttu-id="cde0e-105">[] Specifikace: probíhá, níže</span><span class="sxs-lookup"><span data-stu-id="cde0e-105">[ ] Specification: In progress, below</span></span>

## <a name="summary"></a><span data-ttu-id="cde0e-106">Souhrn</span><span class="sxs-lookup"><span data-stu-id="cde0e-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="cde0e-107">Přidání podpory pro _metody virtuálních rozšíření_ – metody v rozhraních s konkrétními implementacemi.</span><span class="sxs-lookup"><span data-stu-id="cde0e-107">Add support for _virtual extension methods_ - methods in interfaces with concrete implementations.</span></span> <span data-ttu-id="cde0e-108">Třída nebo struktura, která implementuje takové rozhraní, musí mít jedinou _specifickou_ implementaci pro metodu rozhraní, buď implementovanou třídou nebo strukturou, nebo zděděná z jeho základních tříd nebo rozhraní.</span><span class="sxs-lookup"><span data-stu-id="cde0e-108">A class or struct that implements such an interface is required to have a single _most specific_ implementation for the interface method, either implemented by the class or struct, or inherited from its base classes or interfaces.</span></span> <span data-ttu-id="cde0e-109">Metody virtuálních rozšíření umožňují autorovi rozhraní API přidávat do rozhraní v budoucích verzích metody, aniž by došlo k narušení zdrojové nebo binární kompatibility se stávajícími implementacemi tohoto rozhraní.</span><span class="sxs-lookup"><span data-stu-id="cde0e-109">Virtual extension methods enable an API author to add methods to an interface in future versions without breaking source or binary compatibility with existing implementations of that interface.</span></span>

<span data-ttu-id="cde0e-110">Jsou to podobné jako ["výchozí metody"](http://docs.oracle.com/javase/tutorial/java/IandI/defaultmethods.html)Java.</span><span class="sxs-lookup"><span data-stu-id="cde0e-110">These are similar to Java's ["Default Methods"](http://docs.oracle.com/javase/tutorial/java/IandI/defaultmethods.html).</span></span>

<span data-ttu-id="cde0e-111">(Na základě pravděpodobnější techniky implementace) Tato funkce vyžaduje odpovídající podporu rozhraní příkazového řádku (CLI) nebo CLR.</span><span class="sxs-lookup"><span data-stu-id="cde0e-111">(Based on the likely implementation technique) this feature requires corresponding support in the CLI/CLR.</span></span> <span data-ttu-id="cde0e-112">Programy, které tuto funkci využívají, nemůžou běžet na dřívějších verzích platformy.</span><span class="sxs-lookup"><span data-stu-id="cde0e-112">Programs that take advantage of this feature cannot run on earlier versions of the platform.</span></span>

## <a name="motivation"></a><span data-ttu-id="cde0e-113">Motivační</span><span class="sxs-lookup"><span data-stu-id="cde0e-113">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="cde0e-114">Hlavní motivace pro tuto funkci je</span><span class="sxs-lookup"><span data-stu-id="cde0e-114">The principal motivations for this feature are</span></span>

- <span data-ttu-id="cde0e-115">Výchozí metody rozhraní umožňují autorovi rozhraní API přidávat do rozhraní v budoucích verzích metody, aniž by došlo k narušení zdrojové nebo binární kompatibility se stávajícími implementacemi tohoto rozhraní.</span><span class="sxs-lookup"><span data-stu-id="cde0e-115">Default interface methods enable an API author to add methods to an interface in future versions without breaking source or binary compatibility with existing implementations of that interface.</span></span>
- <span data-ttu-id="cde0e-116">Tato funkce umožňuje C# vzájemnou spolupráci s rozhraními API cílících na [Android (Java)](http://docs.oracle.com/javase/tutorial/java/IandI/defaultmethods.html) a [iOS (SWIFT)](https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/Protocols.html#//apple_ref/doc/uid/TP40014097-CH25-ID267), která podporují podobné funkce.</span><span class="sxs-lookup"><span data-stu-id="cde0e-116">The feature enables C# to interoperate with APIs targeting [Android (Java)](http://docs.oracle.com/javase/tutorial/java/IandI/defaultmethods.html) and [iOS (Swift)](https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/Protocols.html#//apple_ref/doc/uid/TP40014097-CH25-ID267), which support similar features.</span></span>
- <span data-ttu-id="cde0e-117">Jak se zapíná, přidání implementací výchozího rozhraní poskytuje prvky funkce jazyka "vlastnosti" (<https://en.wikipedia.org/wiki/Trait_(computer_programming)>).</span><span class="sxs-lookup"><span data-stu-id="cde0e-117">As it turns out, adding default interface implementations provides the elements of the "traits" language feature (<https://en.wikipedia.org/wiki/Trait_(computer_programming)>).</span></span> <span data-ttu-id="cde0e-118">Poukázali se, že se jedná o výkonnou techniku programování (<http://scg.unibe.ch/archive/papers/Scha03aTraits.pdf>).</span><span class="sxs-lookup"><span data-stu-id="cde0e-118">Traits have proven to be a powerful programming technique (<http://scg.unibe.ch/archive/papers/Scha03aTraits.pdf>).</span></span>

## <a name="detailed-design"></a><span data-ttu-id="cde0e-119">Podrobný návrh</span><span class="sxs-lookup"><span data-stu-id="cde0e-119">Detailed design</span></span>
[design]: #detailed-design

<span data-ttu-id="cde0e-120">Syntaxe pro rozhraní se rozšířila na povolení.</span><span class="sxs-lookup"><span data-stu-id="cde0e-120">The syntax for an interface is extended to permit</span></span>

- <span data-ttu-id="cde0e-121">Deklarace členů, které deklarují konstanty, operátory, statické konstruktory a vnořené typy;</span><span class="sxs-lookup"><span data-stu-id="cde0e-121">member declarations that declare constants, operators, static constructors, and nested types;</span></span>
- <span data-ttu-id="cde0e-122">*tělo* pro metodu nebo indexer, vlastnost nebo přistupující objekt události (tj. "výchozí" implementace);</span><span class="sxs-lookup"><span data-stu-id="cde0e-122">a *body* for a method or indexer, property, or event accessor (that is, a "default" implementation);</span></span>
- <span data-ttu-id="cde0e-123">Deklarace členů, které deklarují statická pole, metody, vlastnosti, indexery a události;</span><span class="sxs-lookup"><span data-stu-id="cde0e-123">member declarations that declare static fields, methods, properties, indexers, and events;</span></span>
- <span data-ttu-id="cde0e-124">Deklarace členů pomocí explicitní syntaxe implementace rozhraní; ani</span><span class="sxs-lookup"><span data-stu-id="cde0e-124">member declarations using the explicit interface implementation syntax; and</span></span>
- <span data-ttu-id="cde0e-125">Explicitní modifikátory přístupu (výchozí přístup je `public`).</span><span class="sxs-lookup"><span data-stu-id="cde0e-125">Explicit access modifiers (the default access is `public`).</span></span>

<span data-ttu-id="cde0e-126">Členové s orgány umožňují, aby rozhraní poskytovalo "výchozí" implementaci pro metodu ve třídách a strukturách, které neposkytují přepsání implementace.</span><span class="sxs-lookup"><span data-stu-id="cde0e-126">Members with bodies permit the interface to provide a "default" implementation for the method in classes and structs that do not provide an overriding implementation.</span></span>

<span data-ttu-id="cde0e-127">Rozhraní nemůžou obsahovat stav instance.</span><span class="sxs-lookup"><span data-stu-id="cde0e-127">Interfaces may not contain instance state.</span></span> <span data-ttu-id="cde0e-128">Zatímco statická pole jsou nyní povolena, pole instance nejsou v rozhraních povolena.</span><span class="sxs-lookup"><span data-stu-id="cde0e-128">While static fields are now permitted, instance fields are not permitted in interfaces.</span></span> <span data-ttu-id="cde0e-129">Automatické vlastnosti instance nejsou v rozhraních podporované, protože by implicitně deklarovaly skryté pole.</span><span class="sxs-lookup"><span data-stu-id="cde0e-129">Instance auto-properties are not supported in interfaces, as they would implicitly declare a hidden field.</span></span>

<span data-ttu-id="cde0e-130">Statické a soukromé metody umožňují užitečný refaktoring a organizaci kódu, který slouží k implementaci veřejného rozhraní API rozhraní.</span><span class="sxs-lookup"><span data-stu-id="cde0e-130">Static and private methods permit useful refactoring and organization of code used to implement the interface's public API.</span></span>

<span data-ttu-id="cde0e-131">Přepsání metody v rozhraní musí používat explicitní syntaxi implementace rozhraní.</span><span class="sxs-lookup"><span data-stu-id="cde0e-131">A method override in an interface must use the explicit interface implementation syntax.</span></span>

<span data-ttu-id="cde0e-132">Deklarace typu třídy, typu struktury nebo výčtového typu v rámci oboru parametru typu, který byl deklarován pomocí *variance_annotation*, je chyba.</span><span class="sxs-lookup"><span data-stu-id="cde0e-132">It is an error to declare a class type, struct type, or enum type within the scope of a type parameter that was declared with a *variance_annotation*.</span></span>  <span data-ttu-id="cde0e-133">Například deklarace `C` níže je chyba.</span><span class="sxs-lookup"><span data-stu-id="cde0e-133">For example, the declaration of `C` below is an error.</span></span>

```csharp
interface IOuter<out T>
{
    class C { } // error: class declaration within the scope of variant type parameter 'T'
}
```

### <a name="concrete-methods-in-interfaces"></a><span data-ttu-id="cde0e-134">Konkrétní metody v rozhraních</span><span class="sxs-lookup"><span data-stu-id="cde0e-134">Concrete methods in interfaces</span></span>

<span data-ttu-id="cde0e-135">Nejjednodušší forma této funkce je schopnost deklarovat *konkrétní metodu* v rozhraní, což je metoda s tělem.</span><span class="sxs-lookup"><span data-stu-id="cde0e-135">The simplest form of this feature is the ability to declare a *concrete method* in an interface, which is a method with a body.</span></span>

```csharp
interface IA
{
    void M() { WriteLine("IA.M"); }
}
```

<span data-ttu-id="cde0e-136">Třída, která implementuje toto rozhraní, nevyžaduje implementaci konkrétní metody.</span><span class="sxs-lookup"><span data-stu-id="cde0e-136">A class that implements this interface need not implement its concrete method.</span></span>

```csharp
class C : IA { } // OK

IA i = new C();
i.M(); // prints "IA.M"
```

<span data-ttu-id="cde0e-137">Konečné přepsání `IA.M` ve třídě `C` je konkrétní metoda `M` deklarovaná v `IA`.</span><span class="sxs-lookup"><span data-stu-id="cde0e-137">The final override for `IA.M` in class `C` is the concrete method `M` declared in `IA`.</span></span> <span data-ttu-id="cde0e-138">Všimněte si, že třída nedědí členy z jeho rozhraní; Tato funkce se nezměnila:</span><span class="sxs-lookup"><span data-stu-id="cde0e-138">Note that a class does not inherit members from its interfaces; that is not changed by this feature:</span></span>

```csharp
new C().M(); // error: class 'C' does not contain a member 'M'
```

<span data-ttu-id="cde0e-139">V rámci instance člena rozhraní `this` má typ ohraničujícího rozhraní.</span><span class="sxs-lookup"><span data-stu-id="cde0e-139">Within an instance member of an interface, `this` has the type of the enclosing interface.</span></span>

### <a name="modifiers-in-interfaces"></a><span data-ttu-id="cde0e-140">Modifikátory v rozhraních</span><span class="sxs-lookup"><span data-stu-id="cde0e-140">Modifiers in interfaces</span></span>

<span data-ttu-id="cde0e-141">Syntaxe pro rozhraní je odlehčena umožňující modifikátory pro členy.</span><span class="sxs-lookup"><span data-stu-id="cde0e-141">The syntax for an interface is relaxed to permit modifiers on its members.</span></span> <span data-ttu-id="cde0e-142">Jsou povoleny následující: `private`, `protected`, `internal`, `public`, `virtual`, `abstract`, `sealed`, `static`, `extern`a `partial`.</span><span class="sxs-lookup"><span data-stu-id="cde0e-142">The following are permitted: `private`, `protected`, `internal`, `public`, `virtual`, `abstract`, `sealed`, `static`, `extern`, and `partial`.</span></span>

> <span data-ttu-id="cde0e-143">***TODO***: Ověřte, jaké další modifikátory existují.</span><span class="sxs-lookup"><span data-stu-id="cde0e-143">***TODO***: check what other modifiers exist.</span></span>

<span data-ttu-id="cde0e-144">Člen rozhraní, jehož deklarace zahrnuje tělo, je `virtual` člen, pokud se nepoužívá modifikátor `sealed` nebo `private`.</span><span class="sxs-lookup"><span data-stu-id="cde0e-144">An interface member whose declaration includes a body is a `virtual` member unless the `sealed` or `private` modifier is used.</span></span> <span data-ttu-id="cde0e-145">Modifikátor `virtual` lze použít u členu funkce, který by byl jinak implicitně `virtual`.</span><span class="sxs-lookup"><span data-stu-id="cde0e-145">The `virtual` modifier may be used on a function member that would otherwise be implicitly `virtual`.</span></span> <span data-ttu-id="cde0e-146">Podobně i když `abstract` je výchozím nastavením pro členy rozhraní bez těla, může být tento modifikátor explicitně udělen.</span><span class="sxs-lookup"><span data-stu-id="cde0e-146">Similarly, although `abstract` is the default on interface members without bodies, that modifier may be given explicitly.</span></span> <span data-ttu-id="cde0e-147">Nevirtuální člen může být deklarován pomocí klíčového slova `sealed`.</span><span class="sxs-lookup"><span data-stu-id="cde0e-147">A non-virtual member may be declared using the `sealed` keyword.</span></span>

<span data-ttu-id="cde0e-148">Jedná se o chybu pro `private` nebo `sealed` členu funkce rozhraní, aby neobsahoval tělo.</span><span class="sxs-lookup"><span data-stu-id="cde0e-148">It is an error for a `private` or `sealed` function member of an interface to have no body.</span></span> <span data-ttu-id="cde0e-149">Člen funkce `private` nesmí mít modifikátor `sealed`.</span><span class="sxs-lookup"><span data-stu-id="cde0e-149">A `private` function member may not have the modifier `sealed`.</span></span>

<span data-ttu-id="cde0e-150">Modifikátory přístupu lze použít pro členy rozhraní všech druhů členů, kteří jsou povoleni.</span><span class="sxs-lookup"><span data-stu-id="cde0e-150">Access modifiers may be used on interface members of all kinds of members that are permitted.</span></span> <span data-ttu-id="cde0e-151">Výchozím nastavením je `public` úroveň přístupu, ale je možné ji výslovně udělit.</span><span class="sxs-lookup"><span data-stu-id="cde0e-151">The access level `public` is the default but it may be given explicitly.</span></span>

> <span data-ttu-id="cde0e-152">***Otevřít problém:*** Musíme přesně určit význam modifikátorů přístupu, jako je `protected` a `internal`, a deklarace a Nepřepisovat je (v odvozeném rozhraní) nebo je implementovat (ve třídě, která implementuje rozhraní).</span><span class="sxs-lookup"><span data-stu-id="cde0e-152">***Open Issue:*** We need to specify the precise meaning of the access modifiers such as `protected` and `internal`, and which declarations do and do not override them (in a derived interface) or implement them (in a class that implements the interface).</span></span>

<span data-ttu-id="cde0e-153">Rozhraní mohou deklarovat `static` členy, včetně vnořených typů, metod, indexerů, vlastností, událostí a statických konstruktorů.</span><span class="sxs-lookup"><span data-stu-id="cde0e-153">Interfaces may declare `static` members, including nested types, methods, indexers, properties, events, and static constructors.</span></span> <span data-ttu-id="cde0e-154">Výchozí úroveň přístupu pro všechny členy rozhraní je `public`.</span><span class="sxs-lookup"><span data-stu-id="cde0e-154">The default access level for all interface members is `public`.</span></span>

<span data-ttu-id="cde0e-155">Rozhraní nemohou deklarovat konstruktory instancí, destruktory a pole.</span><span class="sxs-lookup"><span data-stu-id="cde0e-155">Interfaces may not declare instance constructors, destructors, or fields.</span></span>

> <span data-ttu-id="cde0e-156">***Uzavřený problém:*** Mají se v rozhraní povolit deklarace operátoru?</span><span class="sxs-lookup"><span data-stu-id="cde0e-156">***Closed Issue:*** Should operator declarations be permitted in an interface?</span></span> <span data-ttu-id="cde0e-157">Pravděpodobně neexistují operátory převodu, ale co se týká dalších?</span><span class="sxs-lookup"><span data-stu-id="cde0e-157">Probably not conversion operators, but what about others?</span></span> <span data-ttu-id="cde0e-158">***Rozhodnutí***: operátory jsou povoleny *s výjimkou* operátorů konverze, rovnosti a nerovnosti.</span><span class="sxs-lookup"><span data-stu-id="cde0e-158">***Decision***: Operators are permitted *except* for conversion, equality, and inequality operators.</span></span>

> <span data-ttu-id="cde0e-159">***Uzavřený problém:*** Má `new` být povoleno pro deklarace členů rozhraní, které skrývají členy ze základních rozhraní?</span><span class="sxs-lookup"><span data-stu-id="cde0e-159">***Closed Issue:*** Should `new` be permitted on interface member declarations that hide members from base interfaces?</span></span> <span data-ttu-id="cde0e-160">***Rozhodnutí***: Ano.</span><span class="sxs-lookup"><span data-stu-id="cde0e-160">***Decision***: Yes.</span></span>

> <span data-ttu-id="cde0e-161">***Uzavřený problém:*** V současné době nepovolujeme `partial` na rozhraní nebo jeho členech.</span><span class="sxs-lookup"><span data-stu-id="cde0e-161">***Closed Issue:*** We do not currently permit `partial` on an interface or its members.</span></span> <span data-ttu-id="cde0e-162">To by vyžadovalo samostatný návrh.</span><span class="sxs-lookup"><span data-stu-id="cde0e-162">That would require a separate proposal.</span></span> <span data-ttu-id="cde0e-163">***Rozhodnutí***: Ano.</span><span class="sxs-lookup"><span data-stu-id="cde0e-163">***Decision***: Yes.</span></span> <https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#permit-partial-in-interface>

### <a name="overrides-in-interfaces"></a><span data-ttu-id="cde0e-164">Přepsání v rozhraních</span><span class="sxs-lookup"><span data-stu-id="cde0e-164">Overrides in interfaces</span></span>

<span data-ttu-id="cde0e-165">Deklarace přepisu (tj. ty, které obsahují modifikátor `override`) umožňují programátorovi poskytnout nejvíce specifickou implementaci virtuálního člena v rozhraní, kde kompilátor nebo modul runtime by jinak nenalezl jeden.</span><span class="sxs-lookup"><span data-stu-id="cde0e-165">Override declarations (i.e. those containing the `override` modifier) allow the programmer to provide a most specific implementation of a virtual member in an interface where the compiler or runtime would not otherwise find one.</span></span> <span data-ttu-id="cde0e-166">Umožňuje také zapnout abstraktního člena z rozhraní Super do výchozího člena v odvozeném rozhraní.</span><span class="sxs-lookup"><span data-stu-id="cde0e-166">It also allows turning an abstract member from a super-interface into a default member in a derived interface.</span></span> <span data-ttu-id="cde0e-167">Deklarace přepisu je povolená *explicitně* přepsat konkrétní metodu základního rozhraní tím, že je kvalifikována deklarace s názvem rozhraní (v tomto případě není povolený modifikátor přístupu).</span><span class="sxs-lookup"><span data-stu-id="cde0e-167">An override declaration is permitted to *explicitly* override a particular base interface method by qualifying the declaration with the interface name (no access modifier is permitted in this case).</span></span> <span data-ttu-id="cde0e-168">Implicitní přepsání nejsou povolena.</span><span class="sxs-lookup"><span data-stu-id="cde0e-168">Implicit overrides are not permitted.</span></span>

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

<span data-ttu-id="cde0e-169">Deklarace přepisu v rozhraních nesmí být deklarované `sealed`.</span><span class="sxs-lookup"><span data-stu-id="cde0e-169">Override declarations in interfaces may not be declared `sealed`.</span></span>

<span data-ttu-id="cde0e-170">Veřejné `virtual` členské funkce v rozhraní mohou být v odvozeném rozhraní přepsány explicitně (kvalifikováním názvu v deklaraci přepsání s typem rozhraní, který původně deklaroval metodu a vynecháním modifikátoru přístupu).</span><span class="sxs-lookup"><span data-stu-id="cde0e-170">Public `virtual` function members in an interface may be overridden in a derived interface explicitly (by qualifying the name in the override declaration with the interface type that originally declared the method, and omitting an access modifier).</span></span>

<span data-ttu-id="cde0e-171">`virtual` členů funkce v rozhraní lze pouze přepsat explicitně (ne implicitně) v odvozených rozhraních a členy, které nejsou `public` mohou být implementovány pouze ve třídě nebo struktuře explicitně (ne implicitně).</span><span class="sxs-lookup"><span data-stu-id="cde0e-171">`virtual` function members in an interface may only be overridden explicitly (not implicitly) in derived interfaces, and members that are not `public` may only be implemented in a class or struct explicitly (not implicitly).</span></span> <span data-ttu-id="cde0e-172">V obou případech musí být přepsaný nebo implementovaný člen *přístupný* tam, kde je přepsán.</span><span class="sxs-lookup"><span data-stu-id="cde0e-172">In either case, the overridden or implemented member must be *accessible* where it is overridden.</span></span>

### <a name="reabstraction"></a><span data-ttu-id="cde0e-173">Přeabstrakce</span><span class="sxs-lookup"><span data-stu-id="cde0e-173">Reabstraction</span></span>

<span data-ttu-id="cde0e-174">Virtuální (konkrétní) metoda deklarovaná v rozhraní může být přepsána tak, aby byla abstraktní v odvozeném rozhraní.</span><span class="sxs-lookup"><span data-stu-id="cde0e-174">A virtual (concrete) method declared in an interface may be overridden to be abstract in a derived interface</span></span>

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

<span data-ttu-id="cde0e-175">Modifikátor `abstract` není vyžadován v deklaraci `IB.M` (což je výchozí hodnota v rozhraních), ale je pravděpodobné, že je v deklaraci přepsání dobrým zvykem explicitní.</span><span class="sxs-lookup"><span data-stu-id="cde0e-175">The `abstract` modifier is not required in the declaration of `IB.M` (that is the default in interfaces), but it is probably good practice to be explicit in an override declaration.</span></span>

<span data-ttu-id="cde0e-176">To je užitečné v odvozených rozhraních, kde je výchozí implementace metody nevhodná a je třeba poskytnout vhodnější implementaci implementací tříd.</span><span class="sxs-lookup"><span data-stu-id="cde0e-176">This is useful in derived interfaces where the default implementation of a method is inappropriate and a more appropriate implementation should be provided by implementing classes.</span></span>

> <span data-ttu-id="cde0e-177">***Otevřít problém:*** Má se povolit přeabstrakce?</span><span class="sxs-lookup"><span data-stu-id="cde0e-177">***Open Issue:*** Should reabstraction be permitted?</span></span>

### <a name="the-most-specific-override-rule"></a><span data-ttu-id="cde0e-178">Nejpřesnější pravidlo přepsání</span><span class="sxs-lookup"><span data-stu-id="cde0e-178">The most specific override rule</span></span>

<span data-ttu-id="cde0e-179">Vyžadujeme, aby každé rozhraní a třída měly *nejvíce specifické přepsání* pro všechny virtuální členy v rámci přepsání, která se nacházejí v typu nebo v jeho přímých a nepřímých rozhraních.</span><span class="sxs-lookup"><span data-stu-id="cde0e-179">We require that every interface and class have a *most specific override* for every virtual member among the overrides appearing in the type or its direct and indirect interfaces.</span></span> <span data-ttu-id="cde0e-180">Nejpřesnější *přepsání* je jedinečné přepsání, které je specifičtější než každé jiné přepsání.</span><span class="sxs-lookup"><span data-stu-id="cde0e-180">The *most specific override* is a unique override that is more specific than every other override.</span></span> <span data-ttu-id="cde0e-181">Pokud nedojde k žádnému přepsání, považuje se samotný člen za nejpřesnější přepsání.</span><span class="sxs-lookup"><span data-stu-id="cde0e-181">If there is no override, the member itself is considered the most specific override.</span></span>

<span data-ttu-id="cde0e-182">Jedno přepsání `M1` je považováno za *konkrétnější* než jiné přepsání `M2` pokud je `M1` deklarováno v typu `T1`, `M2` je deklarováno v typu `T2`a buď</span><span class="sxs-lookup"><span data-stu-id="cde0e-182">One override `M1` is considered *more specific* than another override `M2` if `M1` is declared on type `T1`, `M2` is declared on type `T2`, and either</span></span>

1. <span data-ttu-id="cde0e-183">`T1` obsahuje `T2` z jeho přímých nebo nepřímých rozhraní nebo</span><span class="sxs-lookup"><span data-stu-id="cde0e-183">`T1` contains `T2` among its direct or indirect interfaces, or</span></span>
2. <span data-ttu-id="cde0e-184">`T2` je typ rozhraní, ale `T1` není typu rozhraní.</span><span class="sxs-lookup"><span data-stu-id="cde0e-184">`T2` is an interface type but `T1` is not an interface type.</span></span>

<span data-ttu-id="cde0e-185">Příklad:</span><span class="sxs-lookup"><span data-stu-id="cde0e-185">For example:</span></span>

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

<span data-ttu-id="cde0e-186">Nejpřesnější pravidlo přepsání zajišťuje konflikt (tj. nejednoznačnost vyplývající z dědičnosti kosočtverce), která je explicitně řešena programátorem v místě, kde nastane konflikt.</span><span class="sxs-lookup"><span data-stu-id="cde0e-186">The most specific override rule ensures that a conflict (i.e. an ambiguity arising from diamond inheritance) is resolved explicitly by the programmer at the point where the conflict arises.</span></span>

<span data-ttu-id="cde0e-187">Protože podporujeme explicitní abstraktní přepsání v rozhraních, můžeme to udělat i ve třídách.</span><span class="sxs-lookup"><span data-stu-id="cde0e-187">Because we support explicit abstract overrides in interfaces, we could do so in classes as well</span></span>

```csharp
abstract class E : IA, IB, IC // ok
{
    abstract void IA.M();
}
```

> <span data-ttu-id="cde0e-188">***Otevřít problém***: by měla podpora explicitního přepsání rozhraní v třídách?</span><span class="sxs-lookup"><span data-stu-id="cde0e-188">***Open issue***: should we support explicit interface abstract overrides in classes?</span></span>

<span data-ttu-id="cde0e-189">Kromě toho se jedná o chybu, pokud je v deklaraci třídy nejvíce specifické přepsání některé metody rozhraní abstraktním přepsáním, které bylo deklarováno v rozhraní.</span><span class="sxs-lookup"><span data-stu-id="cde0e-189">In addition, it is an error if in a class declaration the most specific override of some interface method is an abstract override that was declared in an interface.</span></span> <span data-ttu-id="cde0e-190">Toto pravidlo se přestavuje pomocí nové terminologie.</span><span class="sxs-lookup"><span data-stu-id="cde0e-190">This is an existing rule restated using the new terminology.</span></span>

```csharp
interface IF
{
    void M();
}
abstract class F : IF { } // error: 'F' does not implement 'IF.M'
```

<span data-ttu-id="cde0e-191">Je možné, že virtuální vlastnost deklarovaná v rozhraní má nejvíce konkrétní přepsání pro svůj přistupující objekt `get` v jednom rozhraní a nejvíce specifické přepsání pro svůj přistupující objekt `set` v jiném rozhraní.</span><span class="sxs-lookup"><span data-stu-id="cde0e-191">It is possible for a virtual property declared in an interface to have a most specific override for its `get` accessor in one interface and a most specific override for its `set` accessor in a different interface.</span></span> <span data-ttu-id="cde0e-192">To je považováno za porušení *nejvíce specifického pravidla přepsání* .</span><span class="sxs-lookup"><span data-stu-id="cde0e-192">This is considered a violation of the *most specific override* rule.</span></span>

### <a name="static-and-private-methods"></a><span data-ttu-id="cde0e-193">metody `static` a `private`</span><span class="sxs-lookup"><span data-stu-id="cde0e-193">`static` and `private` methods</span></span>

<span data-ttu-id="cde0e-194">Vzhledem k tomu, že rozhraní mohou nyní obsahovat spustitelný kód, je užitečné abstraktní společný kód do privátních a statických metod.</span><span class="sxs-lookup"><span data-stu-id="cde0e-194">Because interfaces may now contain executable code, it is useful to abstract common code into private and static methods.</span></span> <span data-ttu-id="cde0e-195">Nyní je v rozhraních povolujeme.</span><span class="sxs-lookup"><span data-stu-id="cde0e-195">We now permit these in interfaces.</span></span>

> <span data-ttu-id="cde0e-196">***Uzavřený problém***: máme podporovat privátní metody?</span><span class="sxs-lookup"><span data-stu-id="cde0e-196">***Closed issue***: Should we support private methods?</span></span> <span data-ttu-id="cde0e-197">Podporujeme statické metody?</span><span class="sxs-lookup"><span data-stu-id="cde0e-197">Should we support static methods?</span></span> <span data-ttu-id="cde0e-198">**Rozhodnutí: Ano**</span><span class="sxs-lookup"><span data-stu-id="cde0e-198">**Decision: YES**</span></span>

> <span data-ttu-id="cde0e-199">***Otevření problému***: mám povolit, aby byly metody rozhraní `protected` nebo `internal` nebo jiný přístup?</span><span class="sxs-lookup"><span data-stu-id="cde0e-199">***Open issue***: should we permit interface methods to be `protected` or `internal` or other access?</span></span> <span data-ttu-id="cde0e-200">Pokud ano, jaké jsou sémantiky?</span><span class="sxs-lookup"><span data-stu-id="cde0e-200">If so, what are the semantics?</span></span> <span data-ttu-id="cde0e-201">Jsou ve výchozím nastavení `virtual`?</span><span class="sxs-lookup"><span data-stu-id="cde0e-201">Are they `virtual` by default?</span></span> <span data-ttu-id="cde0e-202">Pokud ano, existuje způsob, jak je nastavit jako nevirtuální?</span><span class="sxs-lookup"><span data-stu-id="cde0e-202">If so, is there a way to make them non-virtual?</span></span>

> <span data-ttu-id="cde0e-203">***Otevření problému***: Pokud podporujeme statické metody, budeme podporovat (statické) operátory?</span><span class="sxs-lookup"><span data-stu-id="cde0e-203">***Open issue***: If we support static methods, should we support (static) operators?</span></span>

### <a name="base-interface-invocations"></a><span data-ttu-id="cde0e-204">Volání základního rozhraní</span><span class="sxs-lookup"><span data-stu-id="cde0e-204">Base interface invocations</span></span>

<span data-ttu-id="cde0e-205">Kód v typu, který je odvozen z rozhraní s výchozí metodou, může explicitně vyvolat implementaci "Base" tohoto rozhraní.</span><span class="sxs-lookup"><span data-stu-id="cde0e-205">Code in a type that derives from an interface with a default method can explicitly invoke that interface's "base" implementation.</span></span>

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

<span data-ttu-id="cde0e-206">Metoda instance (nestatická) má povoleno vyvolat implementaci přístupné metody instance přímo v přímém základním rozhraní, a to tak, že je pojmenovaná pomocí `base(Type).M`syntaxe.</span><span class="sxs-lookup"><span data-stu-id="cde0e-206">An instance (nonstatic) method is permitted to invoke the implementation of an accessible instance method in a direct base interface nonvirtually by naming it using the syntax `base(Type).M`.</span></span> <span data-ttu-id="cde0e-207">To je užitečné v případě, že přepsání, které je nutné poskytnout kvůli dědění kosočtverce, je vyřešeno delegováním na jednu konkrétní základní implementaci.</span><span class="sxs-lookup"><span data-stu-id="cde0e-207">This is useful when an override that is required to be provided due to diamond inheritance is resolved by delegating to one particular base implementation.</span></span>

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

<span data-ttu-id="cde0e-208">Je-li k `virtual` nebo `abstract` členem pomocí `base(Type).M`syntaxe, je nutné, aby `Type` obsahuje jedinečné *přepsání* pro `M`.</span><span class="sxs-lookup"><span data-stu-id="cde0e-208">When a `virtual` or `abstract` member is accessed using the syntax `base(Type).M`, it is required that `Type` contains a unique *most specific override* for `M`.</span></span>

### <a name="binding-base-clauses"></a><span data-ttu-id="cde0e-209">Klauzule Binding Base</span><span class="sxs-lookup"><span data-stu-id="cde0e-209">Binding base clauses</span></span>

<span data-ttu-id="cde0e-210">Rozhraní nyní obsahují typy.</span><span class="sxs-lookup"><span data-stu-id="cde0e-210">Interfaces now contain types.</span></span>  <span data-ttu-id="cde0e-211">Tyto typy mohou být použity v základní klauzuli jako základní rozhraní.</span><span class="sxs-lookup"><span data-stu-id="cde0e-211">These types may be used in the base clause as base interfaces.</span></span>  <span data-ttu-id="cde0e-212">Při vázání základní klauzule můžeme potřebovat znát sadu základních rozhraní, aby tyto typy navázala (např. aby je bylo možné vyhledat a vyřešit chráněný přístup).</span><span class="sxs-lookup"><span data-stu-id="cde0e-212">When binding a base clause, we may need to know the set of base interfaces to bind those types (e.g. to lookup in them and to resolve protected access).</span></span>  <span data-ttu-id="cde0e-213">Význam základní klauzule rozhraní je proto cyklicky definován.</span><span class="sxs-lookup"><span data-stu-id="cde0e-213">The meaning of an interface's base clause is thus circularly defined.</span></span>  <span data-ttu-id="cde0e-214">Pro přerušení cyklu přidáme nová jazyková pravidla odpovídající podobnému pravidlu, které už je pro třídy zavedené.</span><span class="sxs-lookup"><span data-stu-id="cde0e-214">To break the cycle, we add a new language rules corresponding to a similar rule already in place for classes.</span></span>

<span data-ttu-id="cde0e-215">Při určování významu *interface_base* rozhraní jsou základní rozhraní dočasně považována za prázdná.</span><span class="sxs-lookup"><span data-stu-id="cde0e-215">While determining the meaning of the *interface_base* of an interface, the base interfaces are temporarily assumed to be empty.</span></span> <span data-ttu-id="cde0e-216">Intuitivní to zajistí, že význam základní klauzule nemůže rekurzivně záviset sám na sobě.</span><span class="sxs-lookup"><span data-stu-id="cde0e-216">Intuitively this ensures that the meaning of a base clause cannot recursively depend on itself.</span></span> 

<span data-ttu-id="cde0e-217">**Použili jsme následující pravidla:**</span><span class="sxs-lookup"><span data-stu-id="cde0e-217">**We used to have the following rules:**</span></span>

<span data-ttu-id="cde0e-218">"Je-li třída B odvozena od třídy A, jedná se o chybu při kompilaci, která je v době kompilace závislá na B. Třída **přímo závisí na** své přímé základní třídě (pokud existuje) a **přímo závisí na** ~~**třídě**~~ , ve které je okamžitě vnořena (pokud existuje).</span><span class="sxs-lookup"><span data-stu-id="cde0e-218">"When a class B derives from a class A, it is a compile-time error for A to depend on B. A class **directly depends on** its direct base class (if any) and **directly depends on** the ~~**class**~~ within which it is immediately nested (if any).</span></span> <span data-ttu-id="cde0e-219">V rámci této definice je kompletní sada ~~**tříd**~~ , na kterých je třída závislá, výraz reflexivní a přenosného uzavření **přímo závislý na** vztahu. "</span><span class="sxs-lookup"><span data-stu-id="cde0e-219">Given this definition, the complete set of ~~**classes**~~ upon which a class depends is the reflexive and transitive closure of the **directly depends on** relationship."</span></span>

<span data-ttu-id="cde0e-220">Jedná se o chybu při kompilaci, která rozhraní přímo nebo nepřímo dědí ze samotného.</span><span class="sxs-lookup"><span data-stu-id="cde0e-220">It is a compile-time error for an interface to directly or indirectly inherit from itself.</span></span>
<span data-ttu-id="cde0e-221">**Základní rozhraní** rozhraní jsou explicitní základní rozhraní a jejich základní rozhraní.</span><span class="sxs-lookup"><span data-stu-id="cde0e-221">The **base interfaces** of an interface are the explicit base interfaces and their base interfaces.</span></span> <span data-ttu-id="cde0e-222">Jinými slovy, sada základních rozhraní je kompletní přenosová uzávěra explicitního základního rozhraní, jejich explicitní základní rozhraní a tak dále.</span><span class="sxs-lookup"><span data-stu-id="cde0e-222">In other words, the set of base interfaces is the complete transitive closure of the explicit base interfaces, their explicit base interfaces, and so on.</span></span>

<span data-ttu-id="cde0e-223">**Upravujeme je takto:**</span><span class="sxs-lookup"><span data-stu-id="cde0e-223">**We are adjusting them as follows:**</span></span>

<span data-ttu-id="cde0e-224">Je-li třída B odvozena od třídy A, jedná se o chybu při kompilaci, která je v době kompilace závislá na B. Třída **přímo závisí na** své přímé základní třídě (pokud existuje) a **přímo závisí na** _**typu**_ , ve kterém je okamžitě vnořen (pokud existuje).</span><span class="sxs-lookup"><span data-stu-id="cde0e-224">When a class B derives from a class A, it is a compile-time error for A to depend on B. A class **directly depends on** its direct base class (if any) and **directly depends on** the _**type**_ within which it is immediately nested (if any).</span></span>

<span data-ttu-id="cde0e-225">Když rozhraní IB rozšiřuje rozhraní IA, jedná se o chybu při kompilaci, která má za to, že má pro platformu IA záviset na IB.</span><span class="sxs-lookup"><span data-stu-id="cde0e-225">When an interface IB extends an interface IA, it is a compile-time error for IA to depend on IB.</span></span> <span data-ttu-id="cde0e-226">Rozhraní **přímo závisí na** svých přímých základních rozhraních (pokud existuje) a **přímo závisí na** typu, ve kterém je okamžitě vnořen (pokud existuje).</span><span class="sxs-lookup"><span data-stu-id="cde0e-226">An interface **directly depends on** its direct base interfaces (if any) and **directly depends on** the type within which it is immediately nested (if any).</span></span>

<span data-ttu-id="cde0e-227">S ohledem na tyto definice je kompletní sada **typů** , na jejichž základě je typ závislý, je reflexivní a přenosová uzávěrka **přímo závislá na** vztahu.</span><span class="sxs-lookup"><span data-stu-id="cde0e-227">Given these definitions, the complete set of **types** upon which a type depends is the reflexive and transitive closure of the **directly depends on** relationship.</span></span>

### <a name="effect-on-existing-programs"></a><span data-ttu-id="cde0e-228">Vliv na existující programy</span><span class="sxs-lookup"><span data-stu-id="cde0e-228">Effect on existing programs</span></span>

<span data-ttu-id="cde0e-229">Tady uvedená pravidla mají mít žádný vliv na význam stávajících programů.</span><span class="sxs-lookup"><span data-stu-id="cde0e-229">The rules presented here are intended to have no effect on the meaning of existing programs.</span></span>

<span data-ttu-id="cde0e-230">Příklad 1:</span><span class="sxs-lookup"><span data-stu-id="cde0e-230">Example 1:</span></span>

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

<span data-ttu-id="cde0e-231">Příklad 2:</span><span class="sxs-lookup"><span data-stu-id="cde0e-231">Example 2:</span></span>

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

<span data-ttu-id="cde0e-232">Stejná pravidla poskytují podobné výsledky pro podobnou situaci zahrnující výchozí metody rozhraní:</span><span class="sxs-lookup"><span data-stu-id="cde0e-232">The same rules give similar results to the analogous situation involving default interface methods:</span></span>

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

> <span data-ttu-id="cde0e-233">***Uzavřený problém***: potvrďte, že se jedná o zamýšlené pořadí specifikace.</span><span class="sxs-lookup"><span data-stu-id="cde0e-233">***Closed issue***: confirm that this is an intended consequence of the specification.</span></span> <span data-ttu-id="cde0e-234">**Rozhodnutí: Ano**</span><span class="sxs-lookup"><span data-stu-id="cde0e-234">**Decision: YES**</span></span>

### <a name="runtime-method-resolution"></a><span data-ttu-id="cde0e-235">Rozlišení metod za běhu</span><span class="sxs-lookup"><span data-stu-id="cde0e-235">Runtime method resolution</span></span>

> <span data-ttu-id="cde0e-236">***Uzavřený problém:*** Specifikace by měla popsat algoritmus rozlišení metody modulu runtime na obličej výchozích metod rozhraní.</span><span class="sxs-lookup"><span data-stu-id="cde0e-236">***Closed Issue:*** The spec should describe the runtime method resolution algorithm in the face of interface default methods.</span></span> <span data-ttu-id="cde0e-237">Musíme zajistit, aby sémantika byla konzistentní s sémantikou jazyka, např. které deklarované metody dělají a nepřepisují ani neimplementují metodu `internal`.</span><span class="sxs-lookup"><span data-stu-id="cde0e-237">We need to ensure that the semantics are consistent with the language semantics, e.g. which declared methods do and do not override or implement an `internal` method.</span></span>

### <a name="clr-support-api"></a><span data-ttu-id="cde0e-238">Rozhraní API podpory CLR</span><span class="sxs-lookup"><span data-stu-id="cde0e-238">CLR support API</span></span>

<span data-ttu-id="cde0e-239">Aby mohly kompilátory detekovat při kompilaci pro modul runtime, který podporuje tuto funkci, knihovny pro tyto moduly jsou upraveny tak, aby tuto skutečnost inzerovaly prostřednictvím rozhraní API, které je popsáno v <https://github.com/dotnet/corefx/issues/17116>.</span><span class="sxs-lookup"><span data-stu-id="cde0e-239">In order for compilers to detect when they are compiling for a runtime that supports this feature, libraries for such runtimes are modified to advertise that fact through the API discussed in <https://github.com/dotnet/corefx/issues/17116>.</span></span> <span data-ttu-id="cde0e-240">Přidám</span><span class="sxs-lookup"><span data-stu-id="cde0e-240">We add</span></span>

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

> <span data-ttu-id="cde0e-241">***Otevřít problém***: je to nejlepší název funkce *CLR* ?</span><span class="sxs-lookup"><span data-stu-id="cde0e-241">***Open issue***: Is that the best name for the *CLR* feature?</span></span> <span data-ttu-id="cde0e-242">Funkce CLR má mnohem víc, než jenom to (např. uvolnit omezení ochrany, podporuje přepsání v rozhraních atd.).</span><span class="sxs-lookup"><span data-stu-id="cde0e-242">The CLR feature does much more than just that (e.g. relaxes protection constraints, supports overrides in interfaces, etc).</span></span> <span data-ttu-id="cde0e-243">Možná by se mělo zavolat na konkrétní metody v rozhraních nebo na vlastnosti?</span><span class="sxs-lookup"><span data-stu-id="cde0e-243">Perhaps it should be called something like "concrete methods in interfaces", or "traits"?</span></span>

### <a name="further-areas-to-be-specified"></a><span data-ttu-id="cde0e-244">Další oblasti, které se mají zadat</span><span class="sxs-lookup"><span data-stu-id="cde0e-244">Further areas to be specified</span></span>

- <span data-ttu-id="cde0e-245">[] Bylo užitečné zařadit do katalogu druhy zdrojových a binárních vlivů kompatibility způsobené přidáním výchozích metod rozhraní a přepsáním existujícím rozhraním.</span><span class="sxs-lookup"><span data-stu-id="cde0e-245">[ ] It would be useful to catalog the kinds of source and binary compatibility effects caused by adding default interface methods and overrides to existing interfaces.</span></span>

## <a name="drawbacks"></a><span data-ttu-id="cde0e-246">Nevýhody</span><span class="sxs-lookup"><span data-stu-id="cde0e-246">Drawbacks</span></span>
[drawbacks]: #drawbacks

<span data-ttu-id="cde0e-247">Tento návrh vyžaduje koordinovanou aktualizaci specifikace CLR (pro podporu konkrétních metod v rozhraních a řešení metody).</span><span class="sxs-lookup"><span data-stu-id="cde0e-247">This proposal requires a coordinated update to the CLR specification (to support concrete methods in interfaces and method resolution).</span></span> <span data-ttu-id="cde0e-248">Proto je poměrně "nákladný" a může se jednat o kombinaci s jinými funkcemi, které také předpokládáme, že by to vyžadovalo změny CLR.</span><span class="sxs-lookup"><span data-stu-id="cde0e-248">It is therefore fairly "expensive" and it may be worth doing in combination with other features that we also anticipate would require CLR changes.</span></span>

## <a name="alternatives"></a><span data-ttu-id="cde0e-249">Alternativy</span><span class="sxs-lookup"><span data-stu-id="cde0e-249">Alternatives</span></span>
[alternatives]: #alternatives

<span data-ttu-id="cde0e-250">Žádné.</span><span class="sxs-lookup"><span data-stu-id="cde0e-250">None.</span></span>

## <a name="unresolved-questions"></a><span data-ttu-id="cde0e-251">Nevyřešené dotazy</span><span class="sxs-lookup"><span data-stu-id="cde0e-251">Unresolved questions</span></span>
[unresolved]: #unresolved-questions

- <span data-ttu-id="cde0e-252">Otevřené otázky se zavolají v celém návrhu výše.</span><span class="sxs-lookup"><span data-stu-id="cde0e-252">Open questions are called out throughout the proposal, above.</span></span>
- <span data-ttu-id="cde0e-253">Seznam otevřených otázek najdete také <https://github.com/dotnet/csharplang/issues/406>.</span><span class="sxs-lookup"><span data-stu-id="cde0e-253">See also <https://github.com/dotnet/csharplang/issues/406> for a list of open questions.</span></span>
- <span data-ttu-id="cde0e-254">Aby bylo možné vybrat přesnou metodu, která má být vyvolána, musí být popsána Podrobná specifikace.</span><span class="sxs-lookup"><span data-stu-id="cde0e-254">The detailed specification must describe the resolution mechanism used at runtime to select the precise method to be invoked.</span></span>
- <span data-ttu-id="cde0e-255">Interakce metadat vyprodukovaných novými kompilátory a spotřebovaná staršími kompilátory musí být podrobně využívány.</span><span class="sxs-lookup"><span data-stu-id="cde0e-255">The interaction of metadata produced by new compilers and consumed by older compilers needs to be worked out in detail.</span></span> <span data-ttu-id="cde0e-256">Musíme například zajistit, aby reprezentace metadat, kterou používáme, nezpůsobí přidání výchozí implementace v rozhraní pro přerušení existující třídy, která implementuje toto rozhraní při kompilování starším kompilátorem.</span><span class="sxs-lookup"><span data-stu-id="cde0e-256">For example, we need to ensure that the metadata representation that we use does not cause the addition of a default implementation in an interface to break an existing class that implements that interface when compiled by an older compiler.</span></span> <span data-ttu-id="cde0e-257">To může mít vliv na reprezentace metadat, kterou můžeme použít.</span><span class="sxs-lookup"><span data-stu-id="cde0e-257">This may affect the metadata representation that we can use.</span></span>
- <span data-ttu-id="cde0e-258">Návrh musí zvážit vzájemnou operaci s jinými jazyky a existujícími kompilátory pro jiné jazyky.</span><span class="sxs-lookup"><span data-stu-id="cde0e-258">The design must consider interoperation with other languages and existing compilers for other languages.</span></span>

## <a name="resolved-questions"></a><span data-ttu-id="cde0e-259">Vyřešené otázky</span><span class="sxs-lookup"><span data-stu-id="cde0e-259">Resolved Questions</span></span>

### <a name="abstract-override"></a><span data-ttu-id="cde0e-260">Abstraktní přepsání</span><span class="sxs-lookup"><span data-stu-id="cde0e-260">Abstract Override</span></span>

<span data-ttu-id="cde0e-261">Předchozí koncept specifikace obsahoval možnost "reabstract" zděděné metody:</span><span class="sxs-lookup"><span data-stu-id="cde0e-261">The earlier draft spec contained the ability to "reabstract" an inherited method:</span></span>

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

<span data-ttu-id="cde0e-262">Moje poznámky pro 2017-03-20 ukázaly, že jsme se rozhodli tuto možnost nedovolit.</span><span class="sxs-lookup"><span data-stu-id="cde0e-262">My notes for 2017-03-20 showed that we decided not to allow this.</span></span> <span data-ttu-id="cde0e-263">Existují však alespoň dva případy použití:</span><span class="sxs-lookup"><span data-stu-id="cde0e-263">However, there are at least two use cases for it:</span></span>

1. <span data-ttu-id="cde0e-264">Rozhraní API Java, se kterými se někteří uživatelé této funkce chtějí vzájemně pracovat, závisí na tomto zařízení.</span><span class="sxs-lookup"><span data-stu-id="cde0e-264">The Java APIs, with which some users of this feature hope to interoperate, depend on this facility.</span></span>
2. <span data-ttu-id="cde0e-265">Z tohoto hlediska se výhody pro programování s *vlastnostmi* .</span><span class="sxs-lookup"><span data-stu-id="cde0e-265">Programming with *traits* benefits from this.</span></span> <span data-ttu-id="cde0e-266">Reabstrakce je jedním z prvků funkce jazyka "vlastnosti" (https://en.wikipedia.org/wiki/Trait_(computer_programming)).</span><span class="sxs-lookup"><span data-stu-id="cde0e-266">Reabstraction is one of the elements of the "traits" language feature (https://en.wikipedia.org/wiki/Trait_(computer_programming)).</span></span> <span data-ttu-id="cde0e-267">S třídami jsou povoleny následující:</span><span class="sxs-lookup"><span data-stu-id="cde0e-267">The following is permitted with classes:</span></span>

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

<span data-ttu-id="cde0e-268">Tento kód bohužel nejde Refaktorovat jako sadu rozhraní (vlastností), pokud to není povolené.</span><span class="sxs-lookup"><span data-stu-id="cde0e-268">Unfortunately this code cannot be refactored as a set of interfaces (traits) unless this is permitted.</span></span> <span data-ttu-id="cde0e-269">*Jared principem Greed*by měla být povolená.</span><span class="sxs-lookup"><span data-stu-id="cde0e-269">By the *Jared principle of greed*, it should be permitted.</span></span>

> <span data-ttu-id="cde0e-270">***Uzavřený problém:*** Má se povolit přeabstrakce?</span><span class="sxs-lookup"><span data-stu-id="cde0e-270">***Closed issue:*** Should reabstraction be permitted?</span></span> <span data-ttu-id="cde0e-271">ODPOVÍ Moje poznámky byly chybné.</span><span class="sxs-lookup"><span data-stu-id="cde0e-271">[YES] My notes were wrong.</span></span> <span data-ttu-id="cde0e-272">[Poznámky LDM](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-03-21.md) říká, že v rozhraní je povolena přeabstrakce.</span><span class="sxs-lookup"><span data-stu-id="cde0e-272">The [LDM notes](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-03-21.md) say that reabstraction is permitted in an interface.</span></span> <span data-ttu-id="cde0e-273">Není ve třídě.</span><span class="sxs-lookup"><span data-stu-id="cde0e-273">Not in a class.</span></span>

### <a name="virtual-modifier-vs-sealed-modifier"></a><span data-ttu-id="cde0e-274">Modifikátor Virtual vs – zapečetěný modifikátor</span><span class="sxs-lookup"><span data-stu-id="cde0e-274">Virtual Modifier vs Sealed Modifier</span></span>

<span data-ttu-id="cde0e-275">Z [Aleksey Tsingauz](https://github.com/AlekseyTs):</span><span class="sxs-lookup"><span data-stu-id="cde0e-275">From [Aleksey Tsingauz](https://github.com/AlekseyTs):</span></span>

> <span data-ttu-id="cde0e-276">Rozhodli jste se povolit modifikátory explicitně uvedené u členů rozhraní, pokud není důvod zakázat některé z nich.</span><span class="sxs-lookup"><span data-stu-id="cde0e-276">We decided to allow modifiers explicitly stated on interface members, unless there is a reason to disallow some of them.</span></span> <span data-ttu-id="cde0e-277">Tím se přinese zajímavá otázka kolem virtuální modifikátoru.</span><span class="sxs-lookup"><span data-stu-id="cde0e-277">This brings an interesting question around virtual modifier.</span></span> <span data-ttu-id="cde0e-278">Má se vyžadovat u členů s výchozí implementací?</span><span class="sxs-lookup"><span data-stu-id="cde0e-278">Should it be required on members with default implementation?</span></span>
>
> <span data-ttu-id="cde0e-279">Můžeme říct, že:</span><span class="sxs-lookup"><span data-stu-id="cde0e-279">We could say that:</span></span>
>
> - <span data-ttu-id="cde0e-280">Pokud není určena žádná implementace ani žádná z nich ani žádná z nich ani virtuální ani žádná z nich není zapečetěná, předpokládáme, že je člen abstraktní</span><span class="sxs-lookup"><span data-stu-id="cde0e-280">if there is no implementation and neither virtual, nor sealed are specified, we assume the member is abstract.</span></span>
> - <span data-ttu-id="cde0e-281">Pokud se nezadá žádná implementace, ani žádná abstraktní ani zapečetěná, předpokládáme, že člen je virtuální.</span><span class="sxs-lookup"><span data-stu-id="cde0e-281">if there is an implementation and neither abstract, nor sealed are specified, we assume the member is virtual.</span></span>
> - <span data-ttu-id="cde0e-282">k vytvoření metody ani virtuální ani abstraktní je vyžadován zapečetěný modifikátor.</span><span class="sxs-lookup"><span data-stu-id="cde0e-282">sealed modifier is required to make a method neither virtual, nor abstract.</span></span>
>
> <span data-ttu-id="cde0e-283">Alternativně můžeme říct, že virtuálnímu členu je vyžadován modifikátor Virtual.</span><span class="sxs-lookup"><span data-stu-id="cde0e-283">Alternatively, we could say that virtual modifier is required for a virtual member.</span></span> <span data-ttu-id="cde0e-284">To znamená, že pokud je člen s implementací explicitně označen jako virtuální modifikátor, není to ani virtuální ani abstraktní.</span><span class="sxs-lookup"><span data-stu-id="cde0e-284">I.e, if there is a member with implementation not explicitly marked with virtual modifier, it is neither virtual, nor abstract.</span></span> <span data-ttu-id="cde0e-285">Tento přístup může poskytovat lepší možnosti při přesunu metody z třídy na rozhraní:</span><span class="sxs-lookup"><span data-stu-id="cde0e-285">This approach might provide better experience when a method is moved from a class to an interface:</span></span>
>
> - <span data-ttu-id="cde0e-286">abstraktní metoda zůstává abstraktní.</span><span class="sxs-lookup"><span data-stu-id="cde0e-286">an abstract method stays abstract.</span></span>
> - <span data-ttu-id="cde0e-287">virtuální metoda zůstává virtuální.</span><span class="sxs-lookup"><span data-stu-id="cde0e-287">a virtual method stays virtual.</span></span>
> - <span data-ttu-id="cde0e-288">Metoda bez modifikátoru zůstane ani virtuální ani abstraktní.</span><span class="sxs-lookup"><span data-stu-id="cde0e-288">a method without any modifier stays neither virtual, nor abstract.</span></span>
> - <span data-ttu-id="cde0e-289">zapečetěný modifikátor nelze použít pro metodu, která není přepsána.</span><span class="sxs-lookup"><span data-stu-id="cde0e-289">sealed modifier cannot be applied to a method that is not an override.</span></span>
>
> <span data-ttu-id="cde0e-290">Jak to myslíš?</span><span class="sxs-lookup"><span data-stu-id="cde0e-290">What do you think?</span></span>

> <span data-ttu-id="cde0e-291">***Uzavřený problém:*** Měla by být konkrétní metoda (s implementací) implicitně `virtual`?</span><span class="sxs-lookup"><span data-stu-id="cde0e-291">***Closed Issue:*** Should a concrete method (with implementation) be implicitly `virtual`?</span></span> <span data-ttu-id="cde0e-292">ODPOVÍ</span><span class="sxs-lookup"><span data-stu-id="cde0e-292">[YES]</span></span>

<span data-ttu-id="cde0e-293">***Rozhodnutí:*** Provedená v rámci LDM 2017-04-05:</span><span class="sxs-lookup"><span data-stu-id="cde0e-293">***Decisions:*** Made in the LDM 2017-04-05:</span></span>

1. <span data-ttu-id="cde0e-294">nevirtuální by se měly explicitně vyjádřit prostřednictvím `sealed` nebo `private`.</span><span class="sxs-lookup"><span data-stu-id="cde0e-294">non-virtual should be explicitly expressed through `sealed` or `private`.</span></span>
2. <span data-ttu-id="cde0e-295">`sealed` je klíčové slovo pro členy instance rozhraní s nevirtuálními institucemi.</span><span class="sxs-lookup"><span data-stu-id="cde0e-295">`sealed` is the keyword to make interface instance members with bodies non-virtual</span></span>
3. <span data-ttu-id="cde0e-296">Chceme v rozhraních zakázat všechny modifikátory.</span><span class="sxs-lookup"><span data-stu-id="cde0e-296">We want to allow all modifiers in interfaces</span></span>  
4. <span data-ttu-id="cde0e-297">Výchozí přístupnost pro členy rozhraní je veřejná, včetně vnořených typů.</span><span class="sxs-lookup"><span data-stu-id="cde0e-297">Default accessibility for interface members is public, including nested types</span></span>
5. <span data-ttu-id="cde0e-298">členy soukromých funkcí v rozhraních jsou implicitně zapečetěné a `sealed` nejsou na nich povolena.</span><span class="sxs-lookup"><span data-stu-id="cde0e-298">private function members in interfaces are implicitly sealed, and `sealed` is not permitted on them.</span></span>
6. <span data-ttu-id="cde0e-299">Soukromé třídy (v rozhraních) jsou povolené a můžou být zapečetěné a to znamená, že jsou zapečetěné ve smyslu třídy Sealed.</span><span class="sxs-lookup"><span data-stu-id="cde0e-299">Private classes (in interfaces) are permitted and can be sealed, and that means sealed in the class sense of sealed.</span></span>
7. <span data-ttu-id="cde0e-300">Chybějící dobrý návrh, u rozhraní nebo jejich členů stále není povolena částečná.</span><span class="sxs-lookup"><span data-stu-id="cde0e-300">Absent a good proposal, partial is still not allowed on interfaces or their members.</span></span>

### <a name="binary-compatibility-1"></a><span data-ttu-id="cde0e-301">Binární kompatibilita 1</span><span class="sxs-lookup"><span data-stu-id="cde0e-301">Binary Compatibility 1</span></span>

<span data-ttu-id="cde0e-302">Když knihovna poskytuje výchozí implementaci</span><span class="sxs-lookup"><span data-stu-id="cde0e-302">When a library provides a default implementation</span></span>

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

<span data-ttu-id="cde0e-303">Chápeme, že implementace `I1.M` v `C` je `I1.M`.</span><span class="sxs-lookup"><span data-stu-id="cde0e-303">We understand that the implementation of `I1.M` in `C` is `I1.M`.</span></span> <span data-ttu-id="cde0e-304">Co když je sestavení obsahující `I2` změněno následujícím způsobem a znovu zkompilováno</span><span class="sxs-lookup"><span data-stu-id="cde0e-304">What if the assembly containing `I2` is changed as follows and recompiled</span></span>

```csharp
interface I2 : I1
{
    override void M() { Impl2 }
}
```

<span data-ttu-id="cde0e-305">ale `C` není znovu zkompilován.</span><span class="sxs-lookup"><span data-stu-id="cde0e-305">but `C` is not recompiled.</span></span> <span data-ttu-id="cde0e-306">Co se stane, když se program spustí?</span><span class="sxs-lookup"><span data-stu-id="cde0e-306">What happens when the program is run?</span></span> <span data-ttu-id="cde0e-307">Vyvolání `(C as I1).M()`</span><span class="sxs-lookup"><span data-stu-id="cde0e-307">An invocation of `(C as I1).M()`</span></span>

1. <span data-ttu-id="cde0e-308">Spuštění `I1.M`</span><span class="sxs-lookup"><span data-stu-id="cde0e-308">Runs `I1.M`</span></span>
2. <span data-ttu-id="cde0e-309">Spuštění `I2.M`</span><span class="sxs-lookup"><span data-stu-id="cde0e-309">Runs `I2.M`</span></span>
3. <span data-ttu-id="cde0e-310">Vyvolá určitý druh běhové chyby.</span><span class="sxs-lookup"><span data-stu-id="cde0e-310">Throws some kind of runtime error</span></span>

<span data-ttu-id="cde0e-311">***Rozhodnutí:*** Provedená 2017-04-11: spustí `I2.M`, což je nejednoznačná nejvíc specifická přepsání za běhu.</span><span class="sxs-lookup"><span data-stu-id="cde0e-311">***Decision:*** Made 2017-04-11: Runs `I2.M`, which is the unambiguously most specific override at runtime.</span></span>

### <a name="event-accessors-closed"></a><span data-ttu-id="cde0e-312">Přístupové objekty událostí (uzavřeno)</span><span class="sxs-lookup"><span data-stu-id="cde0e-312">Event accessors (closed)</span></span>

> <span data-ttu-id="cde0e-313">***Uzavřený problém:*** Je možné událost přepsat "konstantní"?</span><span class="sxs-lookup"><span data-stu-id="cde0e-313">***Closed Issue:*** Can an event be overridden "piecewise"?</span></span>

<span data-ttu-id="cde0e-314">Vezměte v úvahu tento případ:</span><span class="sxs-lookup"><span data-stu-id="cde0e-314">Consider this case:</span></span>

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

<span data-ttu-id="cde0e-315">Tato "částečná" implementace události není povolená, protože, jako ve třídě, syntaxe pro deklaraci události nepovoluje jenom jeden přistupující objekt; musí být zadán obojí (nebo ani ani).</span><span class="sxs-lookup"><span data-stu-id="cde0e-315">This "partial" implementation of the event is not permitted because, as in a class, the syntax for an event declaration does not permit only one accessor; both (or neither) must be provided.</span></span> <span data-ttu-id="cde0e-316">Stejnou věc můžete dosáhnout tím, že v syntaxi povolíte abstraktní odebrání přístupového objektu, který by měl být implicitně abstraktní nepřítomností těla:</span><span class="sxs-lookup"><span data-stu-id="cde0e-316">You could accomplish the same thing by permitting the abstract remove accessor in the syntax to be implicitly abstract by the absence of a body:</span></span>

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

<span data-ttu-id="cde0e-317">Všimněte si, že *se jedná o novou (navrhovanou) syntaxi*.</span><span class="sxs-lookup"><span data-stu-id="cde0e-317">Note that *this is a new (proposed) syntax*.</span></span> <span data-ttu-id="cde0e-318">V aktuální gramatice mají přistupující objekty události povinný text.</span><span class="sxs-lookup"><span data-stu-id="cde0e-318">In the current grammar, event accessors have a mandatory body.</span></span>

> <span data-ttu-id="cde0e-319">***Uzavřený problém:*** Může být přístup k události (implicitně) abstraktní vynechání těla, podobně jako metody v rozhraních a přistupující objekty vlastnosti (implicitně) abstraktní vynásobením těla?</span><span class="sxs-lookup"><span data-stu-id="cde0e-319">***Closed Issue:*** Can an event accessor be (implicitly) abstract by the omission of a body, similarly to the way that methods in interfaces and property accessors are (implicitly) abstract by the omission of a body?</span></span>

<span data-ttu-id="cde0e-320">***Rozhodnutí:*** (2017-04-18) ne, deklarace událostí vyžadují konkrétní přistupující objekty (nebo žádné).</span><span class="sxs-lookup"><span data-stu-id="cde0e-320">***Decision:*** (2017-04-18) No, event declarations require both concrete accessors (or neither).</span></span>

### <a name="reabstraction-in-a-class-closed"></a><span data-ttu-id="cde0e-321">Reabstrakce ve třídě (uzavřená)</span><span class="sxs-lookup"><span data-stu-id="cde0e-321">Reabstraction in a Class (closed)</span></span>

<span data-ttu-id="cde0e-322">***Uzavřený problém:*** Měli byste potvrdit, že je povolený (jinak přidání výchozí implementace by to byla zásadní změna):</span><span class="sxs-lookup"><span data-stu-id="cde0e-322">***Closed Issue:*** We should confirm that this is permitted (otherwise adding a default implementation would be a breaking change):</span></span>

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

<span data-ttu-id="cde0e-323">***Rozhodnutí:*** (2017-04-18) Ano, přidáním těla do deklarace člena rozhraní byste neměli přerušit C.</span><span class="sxs-lookup"><span data-stu-id="cde0e-323">***Decision:*** (2017-04-18) Yes, adding a body to an interface member declaration shouldn't break C.</span></span>

### <a name="sealed-override-closed"></a><span data-ttu-id="cde0e-324">Zapečetěné přepsání (uzavřeno)</span><span class="sxs-lookup"><span data-stu-id="cde0e-324">Sealed Override (closed)</span></span>

<span data-ttu-id="cde0e-325">Předchozí otázka implicitně předpokládá, že modifikátor `sealed` lze použít pro `override` v rozhraní.</span><span class="sxs-lookup"><span data-stu-id="cde0e-325">The previous question implicitly assumes that the `sealed` modifier can be applied to an `override` in an interface.</span></span> <span data-ttu-id="cde0e-326">To je v rozporu se specifikací konceptu.</span><span class="sxs-lookup"><span data-stu-id="cde0e-326">This contradicts the draft specification.</span></span> <span data-ttu-id="cde0e-327">Chceme, aby bylo možné zapečetění přepsat?</span><span class="sxs-lookup"><span data-stu-id="cde0e-327">Do we want to permit sealing an override?</span></span> <span data-ttu-id="cde0e-328">Měly by se brát v úvahu zdrojové a binární účinky na zajištění kompatibility.</span><span class="sxs-lookup"><span data-stu-id="cde0e-328">Source and binary compatibility effects of sealing should be considered.</span></span>

> <span data-ttu-id="cde0e-329">***Uzavřený problém:*** Je potřeba, aby bylo přepsání zapečetěné?</span><span class="sxs-lookup"><span data-stu-id="cde0e-329">***Closed Issue:*** Should we permit sealing an override?</span></span>

<span data-ttu-id="cde0e-330">***Rozhodnutí:*** (2017-04-18) pro přepsání v rozhraních se nepovoluje `sealed`.</span><span class="sxs-lookup"><span data-stu-id="cde0e-330">***Decision:*** (2017-04-18) Let's not allowed `sealed` on overrides in interfaces.</span></span> <span data-ttu-id="cde0e-331">Jediným použitím `sealed` u členů rozhraní je učinit nevirtuální v počáteční deklaraci.</span><span class="sxs-lookup"><span data-stu-id="cde0e-331">The only use of `sealed` on interface members is to make them non-virtual in their initial declaration.</span></span>

### <a name="diamond-inheritance-and-classes-closed"></a><span data-ttu-id="cde0e-332">Dědičnost a třídy Diamond (uzavřeno)</span><span class="sxs-lookup"><span data-stu-id="cde0e-332">Diamond inheritance and classes (closed)</span></span>

<span data-ttu-id="cde0e-333">Koncept návrhu ve scénářích dědičnosti Diamond preferuje přepsání třídy na přepsání rozhraní:</span><span class="sxs-lookup"><span data-stu-id="cde0e-333">The draft of the proposal prefers class overrides to interface overrides in diamond inheritance scenarios:</span></span>

> <span data-ttu-id="cde0e-334">Vyžadujeme, aby každé rozhraní a třída měly *nejvíce specifické přepsání* pro každou metodu rozhraní mezi přepsáními v typu nebo přímo a nepřímými rozhraními.</span><span class="sxs-lookup"><span data-stu-id="cde0e-334">We require that every interface and class have a *most specific override* for every interface method among the overrides appearing in the type or its direct and indirect interfaces.</span></span> <span data-ttu-id="cde0e-335">Nejpřesnější *přepsání* je jedinečné přepsání, které je specifičtější než každé jiné přepsání.</span><span class="sxs-lookup"><span data-stu-id="cde0e-335">The *most specific override* is a unique override that is more specific than every other override.</span></span> <span data-ttu-id="cde0e-336">Pokud není žádné přepsání, samotná metoda je považována za nejvíce specifická přepsání.</span><span class="sxs-lookup"><span data-stu-id="cde0e-336">If there is no override, the method itself is considered the most specific override.</span></span>
>
> <span data-ttu-id="cde0e-337">Jedno přepsání `M1` je považováno za *konkrétnější* než jiné přepsání `M2` pokud je `M1` deklarováno v typu `T1`, `M2` je deklarováno v typu `T2`a buď</span><span class="sxs-lookup"><span data-stu-id="cde0e-337">One override `M1` is considered *more specific* than another override `M2` if `M1` is declared on type `T1`, `M2` is declared on type `T2`, and either</span></span>
>
> 1. <span data-ttu-id="cde0e-338">`T1` obsahuje `T2` z jeho přímých nebo nepřímých rozhraní nebo</span><span class="sxs-lookup"><span data-stu-id="cde0e-338">`T1` contains `T2` among its direct or indirect interfaces, or</span></span>
> 2. <span data-ttu-id="cde0e-339">`T2` je typ rozhraní, ale `T1` není typu rozhraní.</span><span class="sxs-lookup"><span data-stu-id="cde0e-339">`T2` is an interface type but `T1` is not an interface type.</span></span>

<span data-ttu-id="cde0e-340">Scénář je tento</span><span class="sxs-lookup"><span data-stu-id="cde0e-340">The scenario is this</span></span>

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

<span data-ttu-id="cde0e-341">Toto chování je potřeba potvrdit (nebo rozhodnout jinak).</span><span class="sxs-lookup"><span data-stu-id="cde0e-341">We should confirm this behavior (or decide otherwise)</span></span>

> <span data-ttu-id="cde0e-342">***Uzavřený problém:*** Potvrďte výše uvedenou specifikaci konceptu pro *většinu specifických přepsání* , protože platí pro smíšené třídy a rozhraní (třída má přednost před rozhraním).</span><span class="sxs-lookup"><span data-stu-id="cde0e-342">***Closed Issue:*** Confirm the draft spec, above, for *most specific override* as it applies to mixed classes and interfaces (a class takes priority over an interface).</span></span> <span data-ttu-id="cde0e-343">Viz třída <https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-19.md#diamonds-with-classes>.</span><span class="sxs-lookup"><span data-stu-id="cde0e-343">See <https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-19.md#diamonds-with-classes>.</span></span>

### <a name="interface-methods-vs-structs-closed"></a><span data-ttu-id="cde0e-344">Metody rozhraní vs struktury (uzavřeno)</span><span class="sxs-lookup"><span data-stu-id="cde0e-344">Interface methods vs structs (closed)</span></span>

<span data-ttu-id="cde0e-345">Existují některé interakce unfortunate mezi výchozími metodami rozhraní a strukturami.</span><span class="sxs-lookup"><span data-stu-id="cde0e-345">There are some unfortunate interactions between default interface methods and structs.</span></span>

```csharp
interface IA
{
    public void M() { }
}
struct S : IA
{
}
```

<span data-ttu-id="cde0e-346">Všimněte si, že členové rozhraní nejsou děděni:</span><span class="sxs-lookup"><span data-stu-id="cde0e-346">Note that interface members are not inherited:</span></span>

```csharp
var s = default(S);
s.M(); // error: 'S' does not contain a member 'M'
```

<span data-ttu-id="cde0e-347">V důsledku toho musí klient začlenit strukturu pro vyvolání metod rozhraní.</span><span class="sxs-lookup"><span data-stu-id="cde0e-347">Consequently, the client must box the struct to invoke interface methods</span></span>

```csharp
IA s = default(S); // an S, boxed
s.M(); // ok
```

<span data-ttu-id="cde0e-348">Zabalení tímto způsobem se zabývalo základními výhodami typu `struct`.</span><span class="sxs-lookup"><span data-stu-id="cde0e-348">Boxing in this way defeats the principal benefits of a `struct` type.</span></span> <span data-ttu-id="cde0e-349">Kromě toho všechny metody mutace nebudou mít žádný zjevné účinek, protože fungují na *zabalenou kopii* struktury:</span><span class="sxs-lookup"><span data-stu-id="cde0e-349">Moreover, any mutation methods will have no apparent effect, because they are operating on a *boxed copy* of the struct:</span></span>

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

> <span data-ttu-id="cde0e-350">***Uzavřený problém:*** Co se dá dělat na tomto:</span><span class="sxs-lookup"><span data-stu-id="cde0e-350">***Closed Issue:*** What can we do about this:</span></span>
>
> 1. <span data-ttu-id="cde0e-351">Zakazuje `struct` dědění výchozí implementace.</span><span class="sxs-lookup"><span data-stu-id="cde0e-351">Forbid a `struct` from inheriting a default implementation.</span></span> <span data-ttu-id="cde0e-352">Všechny metody rozhraní se budou považovat za abstraktní ve `struct`.</span><span class="sxs-lookup"><span data-stu-id="cde0e-352">All interface methods would be treated as abstract in a `struct`.</span></span> <span data-ttu-id="cde0e-353">Pak můžeme chvíli trvat, než se rozhodnete, jak je lépe pracovat.</span><span class="sxs-lookup"><span data-stu-id="cde0e-353">Then we may take time later to decide how to make it work better.</span></span>
> 2. <span data-ttu-id="cde0e-354">Přináší se nějaký druh strategie generování kódu, který zabraňuje zabalení.</span><span class="sxs-lookup"><span data-stu-id="cde0e-354">Come up with some kind of code generation strategy that avoids boxing.</span></span> <span data-ttu-id="cde0e-355">V rámci metody, jako je například `IB.Increment`, typ `this` by mohl být podobají na parametr typu omezený na `IB`.</span><span class="sxs-lookup"><span data-stu-id="cde0e-355">Inside a method like `IB.Increment`, the type of `this` would perhaps be akin to a type parameter constrained to `IB`.</span></span> <span data-ttu-id="cde0e-356">V kombinaci s tímto rozhraním, aby se zabránilo zabalení v volajícím, byly neabstraktní metody děděny z rozhraní.</span><span class="sxs-lookup"><span data-stu-id="cde0e-356">In conjunction with that, to avoid boxing in the caller, non-abstract methods would be inherited from interfaces.</span></span> <span data-ttu-id="cde0e-357">To může výrazně zvýšit fungování kompilátoru a implementace CLR.</span><span class="sxs-lookup"><span data-stu-id="cde0e-357">This may increase compiler and CLR implementation work substantially.</span></span>
> 3. <span data-ttu-id="cde0e-358">Nedělejte si o tom starosti a stačí ho opustit jako Wart.</span><span class="sxs-lookup"><span data-stu-id="cde0e-358">Not worry about it and just leave it as a wart.</span></span>
> 4. <span data-ttu-id="cde0e-359">Další nápady?</span><span class="sxs-lookup"><span data-stu-id="cde0e-359">Other ideas?</span></span>

<span data-ttu-id="cde0e-360">***Rozhodnutí:*** Nedělejte si o tom starosti a stačí ho opustit jako Wart.</span><span class="sxs-lookup"><span data-stu-id="cde0e-360">***Decision:*** Not worry about it and just leave it as a wart.</span></span> <span data-ttu-id="cde0e-361">Viz třída <https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-19.md#structs-and-default-implementations>.</span><span class="sxs-lookup"><span data-stu-id="cde0e-361">See <https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-19.md#structs-and-default-implementations>.</span></span>

### <a name="base-interface-invocations-closed"></a><span data-ttu-id="cde0e-362">Vyvolání základního rozhraní (uzavřeno)</span><span class="sxs-lookup"><span data-stu-id="cde0e-362">Base interface invocations (closed)</span></span>

<span data-ttu-id="cde0e-363">Specifikace konceptu navrhuje syntaxi pro vyvolání základního rozhraní nechte inspirovat pomocí jazyka Java: `Interface.base.M()`.</span><span class="sxs-lookup"><span data-stu-id="cde0e-363">The draft spec suggests a syntax for base interface invocations inspired by Java: `Interface.base.M()`.</span></span> <span data-ttu-id="cde0e-364">Musíme vybrat syntaxi minimálně pro počáteční prototyp.</span><span class="sxs-lookup"><span data-stu-id="cde0e-364">We need to select a syntax, at least for the initial prototype.</span></span> <span data-ttu-id="cde0e-365">Moje oblíbená položka je `base<Interface>.M()`.</span><span class="sxs-lookup"><span data-stu-id="cde0e-365">My favorite is `base<Interface>.M()`.</span></span>

> <span data-ttu-id="cde0e-366">***Uzavřený problém:*** Jaká je syntaxe pro vyvolání základního člena?</span><span class="sxs-lookup"><span data-stu-id="cde0e-366">***Closed Issue:*** What is the syntax for a base member invocation?</span></span>

<span data-ttu-id="cde0e-367">***Rozhodnutí:*** Syntaxe je `base(Interface).M()`.</span><span class="sxs-lookup"><span data-stu-id="cde0e-367">***Decision:*** The syntax is `base(Interface).M()`.</span></span> <span data-ttu-id="cde0e-368">Viz třída <https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-19.md#base-invocation>.</span><span class="sxs-lookup"><span data-stu-id="cde0e-368">See <https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-19.md#base-invocation>.</span></span> <span data-ttu-id="cde0e-369">Rozhraní, které má být pojmenováno, musí být základní rozhraní, ale nemusí se jednat o přímé základní rozhraní.</span><span class="sxs-lookup"><span data-stu-id="cde0e-369">The interface so named must be a base interface, but does not need to be a direct base interface.</span></span>

> <span data-ttu-id="cde0e-370">***Otevřít problém:*** Mají být pro členy třídy povolené vyvolání základního rozhraní?</span><span class="sxs-lookup"><span data-stu-id="cde0e-370">***Open Issue:*** Should base interface invocations be permitted in class members?</span></span>

<span data-ttu-id="cde0e-371">***Rozhodnutí***: Ano.</span><span class="sxs-lookup"><span data-stu-id="cde0e-371">***Decision***: Yes.</span></span> <https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-19.md#base-invocation>

### <a name="overriding-non-public-interface-members-closed"></a><span data-ttu-id="cde0e-372">Přepisování členů neveřejných rozhraní (uzavřeno)</span><span class="sxs-lookup"><span data-stu-id="cde0e-372">Overriding non-public interface members (closed)</span></span>

<span data-ttu-id="cde0e-373">V rozhraní jsou neveřejné členy ze základních rozhraní přepsány pomocí modifikátoru `override`.</span><span class="sxs-lookup"><span data-stu-id="cde0e-373">In an interface, non-public members from base interfaces are overridden using the `override` modifier.</span></span> <span data-ttu-id="cde0e-374">Pokud se jedná o "explicitní" přepsání, které obsahuje název rozhraní obsahujícího člena, je tento modifikátor přístupu vynechán.</span><span class="sxs-lookup"><span data-stu-id="cde0e-374">If it is an "explicit" override that names the interface containing the member, the access modifier is omitted.</span></span>

> <span data-ttu-id="cde0e-375">***Uzavřený problém:*** Pokud se jedná o "implicitní" přepsání, které nejmenuje rozhraní, musí modifikátor přístupu odpovídat?</span><span class="sxs-lookup"><span data-stu-id="cde0e-375">***Closed Issue:*** If it is an "implicit" override that does not name the interface, does the access modifier have to match?</span></span>

<span data-ttu-id="cde0e-376">***Rozhodnutí:*** Pouze veřejné členy mohou být implicitně přepsány a přístup se musí shodovat.</span><span class="sxs-lookup"><span data-stu-id="cde0e-376">***Decision:*** Only public members may be implicitly overridden, and the access must match.</span></span> <span data-ttu-id="cde0e-377">Viz třída <https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-18.md#dim-implementing-a-non-public-interface-member-not-in-list>.</span><span class="sxs-lookup"><span data-stu-id="cde0e-377">See <https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-18.md#dim-implementing-a-non-public-interface-member-not-in-list>.</span></span>

> <span data-ttu-id="cde0e-378">***Otevřít problém:*** Je modifikátor přístupu požadován, je volitelný nebo vynechán v explicitním přepsání, například `override void IB.M() {}`?</span><span class="sxs-lookup"><span data-stu-id="cde0e-378">***Open Issue:*** Is the access modifier required, optional, or omitted on an explicit override such as `override void IB.M() {}`?</span></span>

> <span data-ttu-id="cde0e-379">***Otevřít problém:*** Je `override` požadováno, volitelné nebo vynechání explicitního přepsání, jako je například `void IB.M() {}`?</span><span class="sxs-lookup"><span data-stu-id="cde0e-379">***Open Issue:*** Is `override` required, optional, or omitted on an explicit override such as `void IB.M() {}`?</span></span>

<span data-ttu-id="cde0e-380">Jak provede jedna implementace neveřejného člena rozhraní ve třídě?</span><span class="sxs-lookup"><span data-stu-id="cde0e-380">How does one implement a non-public interface member in a class?</span></span> <span data-ttu-id="cde0e-381">Možná to musíte udělat explicitně?</span><span class="sxs-lookup"><span data-stu-id="cde0e-381">Perhaps it must be done explicitly?</span></span>

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

> <span data-ttu-id="cde0e-382">***Uzavřený problém:*** Jak provede jedna implementace neveřejného člena rozhraní ve třídě?</span><span class="sxs-lookup"><span data-stu-id="cde0e-382">***Closed Issue:*** How does one implement a non-public interface member in a class?</span></span>

<span data-ttu-id="cde0e-383">***Rozhodnutí:*** Neveřejné členy rozhraní lze implementovat pouze explicitně.</span><span class="sxs-lookup"><span data-stu-id="cde0e-383">***Decision:*** You can only implement non-public interface members explicitly.</span></span> <span data-ttu-id="cde0e-384">Viz třída <https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-18.md#dim-implementing-a-non-public-interface-member-not-in-list>.</span><span class="sxs-lookup"><span data-stu-id="cde0e-384">See <https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-18.md#dim-implementing-a-non-public-interface-member-not-in-list>.</span></span>

<span data-ttu-id="cde0e-385">***Rozhodnutí***: pro členy rozhraní není povolené žádné klíčové slovo `override`.</span><span class="sxs-lookup"><span data-stu-id="cde0e-385">***Decision***: No `override` keyword permitted on interface members.</span></span> <https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#does-an-override-in-an-interface-introduce-a-new-member>

### <a name="binary-compatibility-2-closed"></a><span data-ttu-id="cde0e-386">Binární kompatibilita 2 (uzavřeno)</span><span class="sxs-lookup"><span data-stu-id="cde0e-386">Binary Compatibility 2 (closed)</span></span>

<span data-ttu-id="cde0e-387">Vezměte v úvahu následující kód, ve kterém je každý typ v samostatném sestavení.</span><span class="sxs-lookup"><span data-stu-id="cde0e-387">Consider the following code in which each type is in a separate assembly</span></span>

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

<span data-ttu-id="cde0e-388">Chápeme, že implementace `I1.M` v `C` je `I2.M`.</span><span class="sxs-lookup"><span data-stu-id="cde0e-388">We understand that the implementation of `I1.M` in `C` is `I2.M`.</span></span> <span data-ttu-id="cde0e-389">Co když je sestavení obsahující `I3` změněno následujícím způsobem a znovu zkompilováno</span><span class="sxs-lookup"><span data-stu-id="cde0e-389">What if the assembly containing `I3` is changed as follows and recompiled</span></span>

```csharp
interface I3 : I1
{
    override void M() { Impl3 }
}
```

<span data-ttu-id="cde0e-390">ale `C` není znovu zkompilován.</span><span class="sxs-lookup"><span data-stu-id="cde0e-390">but `C` is not recompiled.</span></span> <span data-ttu-id="cde0e-391">Co se stane, když se program spustí?</span><span class="sxs-lookup"><span data-stu-id="cde0e-391">What happens when the program is run?</span></span> <span data-ttu-id="cde0e-392">Vyvolání `(C as I1).M()`</span><span class="sxs-lookup"><span data-stu-id="cde0e-392">An invocation of `(C as I1).M()`</span></span>

1. <span data-ttu-id="cde0e-393">Spuštění `I1.M`</span><span class="sxs-lookup"><span data-stu-id="cde0e-393">Runs `I1.M`</span></span>
2. <span data-ttu-id="cde0e-394">Spuštění `I2.M`</span><span class="sxs-lookup"><span data-stu-id="cde0e-394">Runs `I2.M`</span></span>
3. <span data-ttu-id="cde0e-395">Spuštění `I3.M`</span><span class="sxs-lookup"><span data-stu-id="cde0e-395">Runs `I3.M`</span></span>
4. <span data-ttu-id="cde0e-396">Buď 2 nebo 3, deterministické</span><span class="sxs-lookup"><span data-stu-id="cde0e-396">Either 2 or 3, deterministically</span></span>
5. <span data-ttu-id="cde0e-397">Vyvolá určitý druh běhové výjimky.</span><span class="sxs-lookup"><span data-stu-id="cde0e-397">Throws some kind of runtime exception</span></span>

<span data-ttu-id="cde0e-398">***Rozhodnutí***: vyvolejte výjimku (5).</span><span class="sxs-lookup"><span data-stu-id="cde0e-398">***Decision***: Throw an exception (5).</span></span> <span data-ttu-id="cde0e-399">Viz třída <https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#issues-in-default-interface-methods>.</span><span class="sxs-lookup"><span data-stu-id="cde0e-399">See <https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#issues-in-default-interface-methods>.</span></span>

### <a name="permit-partial-in-interface-closed"></a><span data-ttu-id="cde0e-400">Povolit `partial` v rozhraní?</span><span class="sxs-lookup"><span data-stu-id="cde0e-400">Permit `partial` in interface?</span></span> <span data-ttu-id="cde0e-401">ukončit</span><span class="sxs-lookup"><span data-stu-id="cde0e-401">(closed)</span></span>

<span data-ttu-id="cde0e-402">Vzhledem k tomu, že rozhraní mohou být použita v různých způsobech podobných způsobu použití abstraktních tříd, může být užitečné je deklarovat `partial`.</span><span class="sxs-lookup"><span data-stu-id="cde0e-402">Given that interfaces may be used in ways analogous to the way abstract classes are used, it may be useful to declare them `partial`.</span></span> <span data-ttu-id="cde0e-403">To by bylo obzvlášť užitečné na tváři generátorů.</span><span class="sxs-lookup"><span data-stu-id="cde0e-403">This would be particularly useful in the face of generators.</span></span>

> <span data-ttu-id="cde0e-404">***Návrh:*** Odeberte omezení jazyka, že rozhraní a členové rozhraní nemohou být deklarovány `partial`.</span><span class="sxs-lookup"><span data-stu-id="cde0e-404">***Proposal:*** Remove the language restriction that interfaces and members of interfaces may not be declared `partial`.</span></span>

<span data-ttu-id="cde0e-405">***Rozhodnutí***: Ano.</span><span class="sxs-lookup"><span data-stu-id="cde0e-405">***Decision***: Yes.</span></span> <span data-ttu-id="cde0e-406">Viz třída <https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#permit-partial-in-interface>.</span><span class="sxs-lookup"><span data-stu-id="cde0e-406">See <https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#permit-partial-in-interface>.</span></span>

### <a name="main-in-an-interface-closed"></a><span data-ttu-id="cde0e-407">`Main` v rozhraní?</span><span class="sxs-lookup"><span data-stu-id="cde0e-407">`Main` in an interface?</span></span> <span data-ttu-id="cde0e-408">ukončit</span><span class="sxs-lookup"><span data-stu-id="cde0e-408">(closed)</span></span>

> <span data-ttu-id="cde0e-409">***Otevřít problém:*** Je `static Main` metodou v rozhraní, aby kandidát byl vstupním bodem programu?</span><span class="sxs-lookup"><span data-stu-id="cde0e-409">***Open Issue:*** Is a `static Main` method in an interface a candidate to be the program's entry point?</span></span>

<span data-ttu-id="cde0e-410">***Rozhodnutí***: Ano.</span><span class="sxs-lookup"><span data-stu-id="cde0e-410">***Decision***: Yes.</span></span> <span data-ttu-id="cde0e-411">Viz třída <https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#main-in-an-interface>.</span><span class="sxs-lookup"><span data-stu-id="cde0e-411">See <https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#main-in-an-interface>.</span></span>

### <a name="confirm-intent-to-support-public-non-virtual-methods-closed"></a><span data-ttu-id="cde0e-412">Potvrdit záměr podporovat veřejné nevirtuální metody (uzavřeno)</span><span class="sxs-lookup"><span data-stu-id="cde0e-412">Confirm intent to support public non-virtual methods (closed)</span></span>

<span data-ttu-id="cde0e-413">Můžete si toto rozhodnutí potvrdit (nebo obrátit), aby bylo možné v rozhraní povolit nevirtuální veřejné metody?</span><span class="sxs-lookup"><span data-stu-id="cde0e-413">Can we please confirm (or reverse) our decision to permit non-virtual public methods in an interface?</span></span>

```csharp
interface IA
{
    public sealed void M() { }
}
```

> <span data-ttu-id="cde0e-414">***Částečně uzavřený problém:*** (2017-04-18) Domníváme se, že bude užitečný, ale vrátí se k němu.</span><span class="sxs-lookup"><span data-stu-id="cde0e-414">***Semi-Closed Issue:*** (2017-04-18) We think it is going to be useful, but will come back to it.</span></span> <span data-ttu-id="cde0e-415">Toto je Trip blok s duševním modelem.</span><span class="sxs-lookup"><span data-stu-id="cde0e-415">This is a mental model tripping block.</span></span>

<span data-ttu-id="cde0e-416">***Rozhodnutí***: Ano.</span><span class="sxs-lookup"><span data-stu-id="cde0e-416">***Decision***: Yes.</span></span> <span data-ttu-id="cde0e-417"><https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#confirm-that-we-support-public-non-virtual-methods>.</span><span class="sxs-lookup"><span data-stu-id="cde0e-417"><https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#confirm-that-we-support-public-non-virtual-methods>.</span></span>

### <a name="does-an-override-in-an-interface-introduce-a-new-member-closed"></a><span data-ttu-id="cde0e-418">Zavádí `override` v rozhraní nového člena?</span><span class="sxs-lookup"><span data-stu-id="cde0e-418">Does an `override` in an interface introduce a new member?</span></span> <span data-ttu-id="cde0e-419">ukončit</span><span class="sxs-lookup"><span data-stu-id="cde0e-419">(closed)</span></span>

<span data-ttu-id="cde0e-420">Existuje několik způsobů, jak sledovat, zda deklarace přepsání zavádí nového člena nebo nikoli.</span><span class="sxs-lookup"><span data-stu-id="cde0e-420">There are a few ways to observe whether an override declaration introduces a new member or not.</span></span>

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

> <span data-ttu-id="cde0e-421">***Otevřít problém:*** Zavádí deklarace přepsání v rozhraní nového člena?</span><span class="sxs-lookup"><span data-stu-id="cde0e-421">***Open Issue:*** Does an override declaration in an interface introduce a new member?</span></span> <span data-ttu-id="cde0e-422">ukončit</span><span class="sxs-lookup"><span data-stu-id="cde0e-422">(closed)</span></span>

<span data-ttu-id="cde0e-423">Ve třídě je přepsání metody "Visible" v některém smyslu.</span><span class="sxs-lookup"><span data-stu-id="cde0e-423">In a class, an overriding method is "visible" in some senses.</span></span> <span data-ttu-id="cde0e-424">Například názvy jeho parametrů mají přednost před názvy parametrů v přepsané metodě.</span><span class="sxs-lookup"><span data-stu-id="cde0e-424">For example, the names of its parameters take precedence over the names of parameters in the overridden method.</span></span> <span data-ttu-id="cde0e-425">Může být možné duplikovat toto chování v rozhraní, protože existuje vždy konkrétní přepsání.</span><span class="sxs-lookup"><span data-stu-id="cde0e-425">It may be possible to duplicate that behavior in interfaces, as there is always a most specific override.</span></span> <span data-ttu-id="cde0e-426">Ale chceme toto chování duplikovat?</span><span class="sxs-lookup"><span data-stu-id="cde0e-426">But do we want to duplicate that behavior?</span></span>

<span data-ttu-id="cde0e-427">Je také možné přepsat metodu přepsání?</span><span class="sxs-lookup"><span data-stu-id="cde0e-427">Also, it is possible to "override" an override method?</span></span> <span data-ttu-id="cde0e-428">[Moot]</span><span class="sxs-lookup"><span data-stu-id="cde0e-428">[Moot]</span></span>

<span data-ttu-id="cde0e-429">***Rozhodnutí***: pro členy rozhraní není povolené žádné klíčové slovo `override`.</span><span class="sxs-lookup"><span data-stu-id="cde0e-429">***Decision***: No `override` keyword permitted on interface members.</span></span> <span data-ttu-id="cde0e-430"><https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#does-an-override-in-an-interface-introduce-a-new-member>.</span><span class="sxs-lookup"><span data-stu-id="cde0e-430"><https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#does-an-override-in-an-interface-introduce-a-new-member>.</span></span>

### <a name="properties-with-a-private-accessor-closed"></a><span data-ttu-id="cde0e-431">Vlastnosti s privátním přístupovým objektem (uzavřeno)</span><span class="sxs-lookup"><span data-stu-id="cde0e-431">Properties with a private accessor (closed)</span></span>

<span data-ttu-id="cde0e-432">Říkáme, že soukromé členy nejsou virtuální a že není povolená kombinace virtuálních a privátních.</span><span class="sxs-lookup"><span data-stu-id="cde0e-432">We say that private members are not virtual, and the combination of virtual and private is disallowed.</span></span> <span data-ttu-id="cde0e-433">Ale jak vlastnost s privátním přístupovým objektem?</span><span class="sxs-lookup"><span data-stu-id="cde0e-433">But what about a property with a private accessor?</span></span>

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

<span data-ttu-id="cde0e-434">Je to povoleno?</span><span class="sxs-lookup"><span data-stu-id="cde0e-434">Is this allowed?</span></span> <span data-ttu-id="cde0e-435">Je zde přistupující objekt `set` `virtual` nebo ne?</span><span class="sxs-lookup"><span data-stu-id="cde0e-435">Is the `set` accessor here `virtual` or not?</span></span> <span data-ttu-id="cde0e-436">Dá se přepsat tam, kde je k dispozici?</span><span class="sxs-lookup"><span data-stu-id="cde0e-436">Can it be overridden where it is accessible?</span></span> <span data-ttu-id="cde0e-437">Implementuje následující implicitně pouze přistupující objekt `get`?</span><span class="sxs-lookup"><span data-stu-id="cde0e-437">Does the following implicitly implement only the `get` accessor?</span></span>

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

<span data-ttu-id="cde0e-438">Je následující předpokládaná chyba, protože IA. P. set není Virtual a také proto, že není přístupný?</span><span class="sxs-lookup"><span data-stu-id="cde0e-438">Is the following presumably an error because IA.P.set isn't virtual and also because it isn't accessible?</span></span>

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

<span data-ttu-id="cde0e-439">***Rozhodnutí***: první příklad vypadá jako platný, zatímco poslední ne.</span><span class="sxs-lookup"><span data-stu-id="cde0e-439">***Decision***: The first example looks valid, while the last does not.</span></span> <span data-ttu-id="cde0e-440">To se vyřeší obdobně, jak už to funguje C#.</span><span class="sxs-lookup"><span data-stu-id="cde0e-440">This is resolved analogously to how it already works in C#.</span></span> <https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#properties-with-a-private-accessor>

### <a name="base-interface-invocations-round-2-closed"></a><span data-ttu-id="cde0e-441">Vyvolání základního rozhraní, Round 2 (uzavřeno)</span><span class="sxs-lookup"><span data-stu-id="cde0e-441">Base Interface Invocations, round 2 (closed)</span></span>

<span data-ttu-id="cde0e-442">Naše předchozí "řešení", jak zpracovávat základní volání, ve skutečnosti neposkytují dostatečné expresivity.</span><span class="sxs-lookup"><span data-stu-id="cde0e-442">Our previous "resolution" to how to handle base invocations doesn't actually provide sufficient expressiveness.</span></span> <span data-ttu-id="cde0e-443">Zapíná, že v C# a CLR, na rozdíl od jazyka Java, je nutné zadat rozhraní obsahující deklaraci metody a umístění implementace, kterou chcete vyvolat.</span><span class="sxs-lookup"><span data-stu-id="cde0e-443">It turns out that in C# and the CLR, unlike Java, you need to specify both the interface containing the method declaration and the location of the implementation you want to invoke.</span></span>

<span data-ttu-id="cde0e-444">Navrhuji následující syntaxi pro základní volání v rozhraních.</span><span class="sxs-lookup"><span data-stu-id="cde0e-444">I propose the following syntax for base calls in interfaces.</span></span> <span data-ttu-id="cde0e-445">Nejsem s ním rádi, ale ukazuje, co by syntaxe měla být schopná vyjádřit:</span><span class="sxs-lookup"><span data-stu-id="cde0e-445">I’m not in love with it, but it illustrates what any syntax must be able to express:</span></span>

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

<span data-ttu-id="cde0e-446">Pokud nedochází k nejednoznačnosti, můžete ho napsat snadněji.</span><span class="sxs-lookup"><span data-stu-id="cde0e-446">If there is no ambiguity, you can write it more simply</span></span>

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

<span data-ttu-id="cde0e-447">Nebo</span><span class="sxs-lookup"><span data-stu-id="cde0e-447">Or</span></span>

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

<span data-ttu-id="cde0e-448">Nebo</span><span class="sxs-lookup"><span data-stu-id="cde0e-448">Or</span></span>

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

<span data-ttu-id="cde0e-449">***Rozhodnutí***: rozhoduje o `base(N.I1<T>).M(s)`, že v případě, že máme vazbu vyvolání, může k problému docházet později.</span><span class="sxs-lookup"><span data-stu-id="cde0e-449">***Decision***: Decided on `base(N.I1<T>).M(s)`, conceding that if we have an invocation binding there may be problem here later on.</span></span> <https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-11-14.md#default-interface-implementations>

### <a name="warning-for-struct-not-implementing-default-method-closed"></a><span data-ttu-id="cde0e-450">Upozornění pro strukturu, která neimplementuje výchozí metodu?</span><span class="sxs-lookup"><span data-stu-id="cde0e-450">Warning for struct not implementing default method?</span></span> <span data-ttu-id="cde0e-451">ukončit</span><span class="sxs-lookup"><span data-stu-id="cde0e-451">(closed)</span></span>

<span data-ttu-id="cde0e-452">@vancem výrazy, které bychom měli vážně zvážit, pokud deklarace typu hodnoty nedokáže přepsat určitou metodu rozhraní, a to i v případě, že by zdědily implementaci této metody z rozhraní.</span><span class="sxs-lookup"><span data-stu-id="cde0e-452">@vancem asserts that we should seriously consider producing a warning if a value type declaration fails to override some interface method, even if it would inherit an implementation of that method from an interface.</span></span> <span data-ttu-id="cde0e-453">Protože způsobuje zabalení omezené voláním.</span><span class="sxs-lookup"><span data-stu-id="cde0e-453">Because it causes boxing and undermines constrained calls.</span></span>

<span data-ttu-id="cde0e-454">***Rozhodnutí***: Zdá se, že je něco víc vhodné pro analyzátor.</span><span class="sxs-lookup"><span data-stu-id="cde0e-454">***Decision***: This seems like something more suited for an analyzer.</span></span> <span data-ttu-id="cde0e-455">Zdá se také, že toto upozornění může být vysoké, protože by mohlo být v případě, že není nikdy volána metoda výchozí rozhraní a nebude k dispozici žádný zabalení.</span><span class="sxs-lookup"><span data-stu-id="cde0e-455">It also seems like this warning could be noisy, since it would fire even if the default interface method is never called and no boxing will ever occur.</span></span> <https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#warning-for-struct-not-implementing-default-method>

### <a name="interface-static-constructors-closed"></a><span data-ttu-id="cde0e-456">Statické konstruktory rozhraní (uzavřené)</span><span class="sxs-lookup"><span data-stu-id="cde0e-456">Interface static constructors (closed)</span></span>

<span data-ttu-id="cde0e-457">Kdy se spouštějí statické konstruktory rozhraní?</span><span class="sxs-lookup"><span data-stu-id="cde0e-457">When are interface static constructors run?</span></span>  <span data-ttu-id="cde0e-458">Aktuální koncept rozhraní příkazového řádku navrhuje, aby k němu došlo při použití první statické metody nebo pole.</span><span class="sxs-lookup"><span data-stu-id="cde0e-458">The current CLI draft proposes that it occurs when the first static method or field is accessed.</span></span> <span data-ttu-id="cde0e-459">Pokud žádná z těchto možností není, nemusí se nikdy spustit??</span><span class="sxs-lookup"><span data-stu-id="cde0e-459">If there are neither of those then it might never be run??</span></span>

<span data-ttu-id="cde0e-460">[2018-10-09 tým CLR navrhuje "Přejít na zrcadlo, co dělajíme u ValueType (zjištění cctor přístupu ke každé metodě instance)"]</span><span class="sxs-lookup"><span data-stu-id="cde0e-460">[2018-10-09 The CLR team proposes "Going to mirror what we do for valuetypes (cctor check on access to each instance method)"]</span></span>

<span data-ttu-id="cde0e-461">***Rozhodnutí***: statické konstruktory jsou také spouštěny při vstupu do metod instance, pokud nebyl statický konstruktor `beforefieldinit`, v tomto případě jsou před přístupem k prvnímu statickému poli spouštěny statické konstruktory.</span><span class="sxs-lookup"><span data-stu-id="cde0e-461">***Decision***: Static constructors are also run on entry to instance methods, if the static constructor was not `beforefieldinit`, in which case static constructors are run before access to the first static field.</span></span> <https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#when-are-interface-static-constructors-run>

## <a name="design-meetings"></a><span data-ttu-id="cde0e-462">Schůzky pro návrh</span><span class="sxs-lookup"><span data-stu-id="cde0e-462">Design meetings</span></span>

<span data-ttu-id="cde0e-463">[2017-03-08 poznámky](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-03-08.md) ke schůzce ldm
[2017-03-21. poznámky ke schůzce LDM](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-03-21.md)
[2017-03-23 "chování CLR pro výchozí metody rozhraní"](https://github.com/dotnet/csharplang/blob/master/meetings/2017/CLR-2017-03-23.md)
[poznámky ke schůzce 2017-04-05 LDM](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-05.md)
[2017-04-11](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-11.md) [. poznámky](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-05-17.md) ke schůzce pro schůzku s platformou [LDM
](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-18.md) 2017-04-18 [. poznámky ke](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-05-31.md) schůzce [pro schůzku LDM](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-19.md)
[2017-04-19 LDM Poznámky](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-06-14.md) ke schůzce
[2018-10-17 poznámky](https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md) ke schůzce LDM
[2018-11-14. poznámky ke schůzce LDM](https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-11-14.md)


</span><span class="sxs-lookup"><span data-stu-id="cde0e-463">[2017-03-08 LDM Meeting Notes](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-03-08.md)
[2017-03-21 LDM Meeting Notes](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-03-21.md)
[2017-03-23 meeting "CLR Behavior for Default Interface Methods"](https://github.com/dotnet/csharplang/blob/master/meetings/2017/CLR-2017-03-23.md)
[2017-04-05 LDM Meeting Notes](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-05.md)
[2017-04-11 LDM Meeting Notes](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-11.md)
[2017-04-18 LDM Meeting Notes](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-18.md)
[2017-04-19 LDM Meeting Notes](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-19.md)
[2017-05-17 LDM Meeting Notes](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-05-17.md)
[2017-05-31 LDM Meeting Notes](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-05-31.md)
[2017-06-14 LDM Meeting Notes](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-06-14.md)
[2018-10-17 LDM Meeting Notes](https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md)
[2018-11-14 LDM Meeting Notes](https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-11-14.md)</span></span>
