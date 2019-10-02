---
ms.openlocfilehash: d162d4b6a489032dcdfca9094a39d88fd03d4013
ms.sourcegitcommit: 892af9016b3317a8fae12d195014dc38ba51cf16
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/01/2019
ms.locfileid: "71704100"
---
# <a name="delegates"></a>Delegáty

Delegáti umožňují scénáře, které jsou adresovány C++s ukazateli na funkce jinými jazyky – například, Pascal a modul. Na rozdíl C++ od ukazatelů na funkce jsou však delegáti plně orientované na objekty a na rozdíl C++ od ukazatelů na členské funkce, delegáty zapouzdřují instanci objektu i metodu.

Deklarace delegáta definuje třídu, která je odvozena od třídy `System.Delegate`. Instance delegáta zapouzdřuje seznam vyvolání, což je seznam jedné nebo více metod, z nichž každý je označován jako volatelné entity. Pro metody instance se volatelné entity skládají z instance a metody v dané instanci. U statických metod se volatelné entity skládá jenom z metody. Vyvolání instance delegáta s vhodnou sadou argumentů způsobí, že každá z entit volání delegáta bude vyvolána s danou sadou argumentů.

Zajímavou a užitečnou vlastností instance delegáta je, že neznají ani nezáleží na třídách metod, které zapouzdřují. to vše je v tom, že tyto metody jsou kompatibilní ([deklarace delegátů](delegates.md#delegate-declarations)) s typem delegáta. Díky tomu jsou delegáti dokonale uzpůsobeni pro volání "anonymous".

## <a name="delegate-declarations"></a>Deklarace delegátů

*Delegate_declaration* je *type_declaration* ([deklarace typu](namespaces.md#type-declarations)), který deklaruje nový typ delegáta.

```antlr
delegate_declaration
    : attributes? delegate_modifier* 'delegate' return_type
      identifier variant_type_parameter_list?
      '(' formal_parameter_list? ')' type_parameter_constraints_clause* ';'
    ;

delegate_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    | delegate_modifier_unsafe
    ;
```

Jedná se o chybu při kompilaci, aby se stejný modifikátor v deklaraci delegáta zobrazoval víckrát.

Modifikátor `new` je povolen pouze u delegátů deklarovaných v jiném typu. v takovém případě určuje, že tento delegát skrývá zděděný člen se stejným názvem, jak je popsáno v tématu [nový modifikátor](classes.md#the-new-modifier).

Modifikátory `public`, `protected`, `internal` a `private` řídí přístupnost typu delegáta. V závislosti na kontextu, ve kterém dojde k deklaraci delegáta, nemusí být některé z těchto modifikátorů povoleny ([deklarovaná přístupnost](basic-concepts.md#declared-accessibility)).

Název typu delegáta je *identifikátor*.

Volitelné *formal_parameter_list* Určuje parametry delegáta a *Typ označuje návratový* typ delegátu.

Volitelné *variant_type_parameter_list* ([seznamy parametrů typu variant](interfaces.md#variant-type-parameter-lists)) určuje parametry typu samotného delegáta.

Návratový typ typu delegáta musí být buď `void`, nebo bezpečný pro výstup ([zabezpečení odchylky](interfaces.md#variance-safety)).

Všechny formální typy parametrů typu delegáta musí být bezpečné pro vstup. Kromě toho musí být všechny typy parametrů `out` nebo `ref` také bezpečné pro výstup. Všimněte si, že i parametry `out` jsou nutné k bezpečnému vstupu z důvodu omezení základní spouštěcí platformy.

Typy delegátů C# v jsou ekvivalentní název, ne strukturální ekvivalent. Konkrétně dva různé typy delegátů, které mají stejný seznam parametrů a návratový typ, se považují za jiné typy delegátů. Nicméně instance dvou jedinečných, ale strukturovaných ekvivalentních typů delegátů, mohou porovnat jako EQUAL ([operátory rovnosti delegátů](expressions.md#delegate-equality-operators)).

Příklad:

```csharp
delegate int D1(int i, double d);

class A
{
    public static int M1(int a, double b) {...}
}

class B
{
    delegate int D2(int c, double d);
    public static int M1(int f, double g) {...}
    public static void M2(int k, double l) {...}
    public static int M3(int g) {...}
    public static void M4(int g) {...}
}
```

Metody `A.M1` a `B.M1` jsou kompatibilní s typy delegátů `D1` a `D2`, protože mají stejný návratový typ a seznam parametrů; Tyto typy delegátů jsou však dva různé typy, takže nejsou zaměnitelné. Metody `B.M2`, `B.M3` a `B.M4` jsou nekompatibilní s typy delegátů `D1` a `D2`, protože mají odlišné návratové typy nebo seznamy parametrů.

Podobně jako jiné deklarace obecného typu musí být předány argumenty typu pro vytvoření typu konstruovaného delegáta. Typy parametrů a návratový typ konstruovaného typu delegáta jsou vytvořeny nahrazením pro každý parametr typu v deklaraci delegáta odpovídajícím argumentem typu vytvořeného typu delegáta. Výsledný návratový typ a typy parametrů se používají při určování, které metody jsou kompatibilní s vytvořeným typem delegáta. Příklad:

```csharp
delegate bool Predicate<T>(T value);

class X
{
    static bool F(int i) {...}
    static bool G(string s) {...}
}
```

Metoda `X.F` je kompatibilní s typem delegáta `Predicate<int>` a metoda `X.G` je kompatibilní s typem delegáta `Predicate<string>`.

Jediným způsobem, jak deklarovat typ delegáta, je prostřednictvím *delegate_declaration*. Typ delegáta je typ třídy, která je odvozena z `System.Delegate`. Typy delegátů jsou implicitně `sealed`, takže není přípustné odvozovat žádný typ z typu delegáta. Není také přípustné odvodit typ třídy bez delegáta z `System.Delegate`. Všimněte si, že `System.Delegate` není samotný typ delegáta. je to typ třídy, ze které jsou odvozeny všechny typy delegátů.

C#poskytuje speciální syntaxi pro vytvoření instance delegáta a vyvolání. S výjimkou vytváření instancí lze všechny operace, které lze použít na instanci třídy nebo třídy, použít také pro třídu nebo instanci delegáta v uvedeném pořadí. Konkrétně je možné přistupovat ke členům typu `System.Delegate` prostřednictvím běžné syntaxe přístupu členů.

Sada metod zapouzdřovaná instancí delegáta se nazývá seznam vyvolání. Při vytvoření instance delegáta ([Kompatibilita s delegováním](delegates.md#delegate-compatibility)) z jediné metody zapouzdří tuto metodu a seznam volání obsahuje pouze jednu položku. Nicméně pokud jsou kombinovány dvě instance delegátů, které nejsou null, jsou jejich seznamy volání zřetězeny – v levém operandu objednávky pak pravý operand – pro vytvoření nového seznamu volání, který obsahuje dvě nebo více položek.

Delegáti jsou kombinováni pomocí binárního `+` ([operátor sčítání](expressions.md#addition-operator)) a operátorů `+=` ([složené přiřazení](expressions.md#compound-assignment)). Delegáta lze odebrat z kombinace delegátů pomocí binárního `-` ([operátor odčítání](expressions.md#subtraction-operator)) a operátorů `-=` ([složené přiřazení](expressions.md#compound-assignment)). Delegáty lze porovnávat s rovností ([delegované operátory rovnosti](expressions.md#delegate-equality-operators)).

Následující příklad ukazuje vytváření instancí řady delegátů a jejich odpovídající seznamy volání:

```csharp
delegate void D(int x);

class C
{
    public static void M1(int i) {...}
    public static void M2(int i) {...}

}

class Test
{
    static void Main() {
        D cd1 = new D(C.M1);      // M1
        D cd2 = new D(C.M2);      // M2
        D cd3 = cd1 + cd2;        // M1 + M2
        D cd4 = cd3 + cd1;        // M1 + M2 + M1
        D cd5 = cd4 + cd3;        // M1 + M2 + M1 + M1 + M2
    }

}
```

Při vytváření instance `cd1` a `cd2` všechny zapouzdřují jednu metodu. Pokud je vytvořena instance `cd3`, má seznam volání dvou metod, `M1` a `M2`, v tomto pořadí. seznam volání `cd4` obsahuje `M1`, `M2` a `M1` v tomto pořadí. Seznam volání `cd5` obsahuje `M1`, `M2`, `M1`, `M1` a `M2` v tomto pořadí. Další příklady kombinování delegátů (stejně jako odebírání) najdete v tématu [delegování volání](delegates.md#delegate-invocation).

## <a name="delegate-compatibility"></a>Kompatibilita delegáta

Metoda nebo delegát `M` je ***kompatibilní*** s typem delegáta `D`, pokud jsou splněny všechny následující podmínky:

*  `D` a `M` mají stejný počet parametrů a každý parametr v `D` má stejné modifikátory `ref` nebo `out` jako odpovídající parametr v `M`.
*  Pro každý parametr hodnoty (parametr bez modifikátoru `ref` nebo `out`) existuje převod identity ([převod identity](conversions.md#identity-conversion)) nebo implicitní převod odkazu ([implicitní převody odkazů](conversions.md#implicit-reference-conversions)) z typu parametru v `D` na odpovídající typ parametru v `M`.
*  Pro každý parametr `ref` nebo `out` je typ parametru v `D` stejný jako typ parametru v `M`.
*  Identita nebo implicitní převod odkazu existují z návratového typu `M` do návratového typu `D`.

## <a name="delegate-instantiation"></a>Instance delegáta

Instance delegáta je vytvořena pomocí *delegate_creation_expression* ([výrazy pro vytvoření delegáta](expressions.md#delegate-creation-expressions)) nebo převod na typ delegáta. Nově vytvořená instance delegáta pak odkazuje na jednu z těchto:

*  Statická metoda, na kterou se odkazuje v *delegate_creation_expression*, nebo
*  Cílový objekt (nemůže být `null`) a metoda instance, na kterou se odkazuje v *delegate_creation_expression*, nebo
*  Jiný delegát.

Příklad:

```csharp
delegate void D(int x);

class C
{
    public static void M1(int i) {...}
    public void M2(int i) {...}
}

class Test
{
    static void Main() { 
        D cd1 = new D(C.M1);        // static method
        C t = new C();
        D cd2 = new D(t.M2);        // instance method
        D cd3 = new D(cd2);        // another delegate
    }
}
```

Po vytvoření instance Delegáti instance vždy odkazují na stejný cílový objekt a metodu. Pamatujte na to, že pokud jsou dva Delegáti zkombinováni nebo když je jeden z nich odebraný, nové delegáty mají za následek vlastní seznam volání. seznamy volání delegátů, které se kombinují nebo odeberou, zůstávají beze změny.

## <a name="delegate-invocation"></a>Volání delegáta

C#poskytuje speciální syntaxi pro vyvolání delegáta. Pokud je vyvolána nenulová instance delegáta, jejíž seznam volání obsahuje jednu položku, vyvolá jednu metodu se stejnými argumenty, která byla zadána, a vrátí stejnou hodnotu jako metoda, která je odkazována na metodu. (Další informace o vyvolání delegáta najdete v tématu věnovaném [voláním delegáta](expressions.md#delegate-invocations) .) Pokud dojde k výjimce během vyvolání takového delegáta a tato výjimka není zachycena v metodě, která byla vyvolána, hledání klauzule catch pro výjimku pokračuje v metodě, která se nazývá delegát, jako by tato metoda měla přímý odkaz na Metoda, na kterou se odkazuje tento delegát.

Vyvolání instance delegáta, jejíž seznam volání obsahuje více položek, se pokračuje vyvoláním každé z metod v seznamu vyvolání, synchronně v daném pořadí. Každá metoda, která má být volána, je předána stejnou sadou argumentů, jako byla předána instanci delegáta. Pokud takové vyvolání delegáta zahrnuje referenční parametry ([referenční parametry](classes.md#reference-parameters)), jednotlivé metody vyvolání budou provedeny s odkazem na stejnou proměnnou. změny v této proměnné podle jedné metody v seznamu volání budou viditelné i pro metody v rozevíracím seznamu vyvolání. Pokud vyvolání delegáta zahrnuje výstupní parametry nebo návratovou hodnotu, jejich konečná hodnota bude pocházet z volání posledního delegáta v seznamu.

Pokud dojde k výjimce během zpracování vyvolání takového delegáta a tato výjimka není zachycena v metodě, která byla vyvolána, hledání klauzule catch pro výjimku pokračuje v metodě, která se nazývá delegát, a dalšími metodami mimo jiné. seznam volání není vyvolán.

Při pokusu o vyvolání instance delegáta, jejíž hodnota je null, dojde k výjimce typu `System.NullReferenceException`.

Následující příklad ukazuje, jak vytvořit instance, kombinovat, odebrat a vyvolat delegáty:

```csharp
using System;

delegate void D(int x);

class C
{
    public static void M1(int i) {
        Console.WriteLine("C.M1: " + i);
    }

    public static void M2(int i) {
        Console.WriteLine("C.M2: " + i);
    }

    public void M3(int i) {
        Console.WriteLine("C.M3: " + i);
    }
}

class Test
{
    static void Main() { 
        D cd1 = new D(C.M1);
        cd1(-1);                // call M1

        D cd2 = new D(C.M2);
        cd2(-2);                // call M2

        D cd3 = cd1 + cd2;
        cd3(10);                // call M1 then M2

        cd3 += cd1;
        cd3(20);                // call M1, M2, then M1

        C c = new C();
        D cd4 = new D(c.M3);
        cd3 += cd4;
        cd3(30);                // call M1, M2, M1, then M3

        cd3 -= cd1;             // remove last M1
        cd3(40);                // call M1, M2, then M3

        cd3 -= cd4;
        cd3(50);                // call M1 then M2

        cd3 -= cd2;
        cd3(60);                // call M1

        cd3 -= cd2;             // impossible removal is benign
        cd3(60);                // call M1

        cd3 -= cd1;             // invocation list is empty so cd3 is null

        cd3(70);                // System.NullReferenceException thrown

        cd3 -= cd1;             // impossible removal is benign
    }
}
```

Jak je znázorněno v příkazu `cd3 += cd1;`, delegát může být přítomen v seznamu volání několikrát. V tomto případě je jednoduše vyvolána jednou pro každý výskyt. V seznamu volání, jako je například, když je tento delegát odebrán, je poslední výskyt v seznamu volání ten, který je skutečně odebraný.

Bezprostředně před provedením závěrečného příkazu, `cd3 -= cd1;`, delegát `cd3` odkazuje na prázdný seznam volání. Pokus o odebrání delegáta z prázdného seznamu (nebo pro odebrání neexistujícího delegáta ze seznamu, který není prázdný) není chyba.

Vytvářený výstup je:

```console
C.M1: -1
C.M2: -2
C.M1: 10
C.M2: 10
C.M1: 20
C.M2: 20
C.M1: 20
C.M1: 30
C.M2: 30
C.M1: 30
C.M3: 30
C.M1: 40
C.M2: 40
C.M3: 40
C.M1: 50
C.M2: 50
C.M1: 60
C.M1: 60
```
