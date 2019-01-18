---
ms.openlocfilehash: 08c14d9ef2afe30580f456995066c141653ede92
ms.sourcegitcommit: 3fc033b6e98ed7ecdf46a85c79b00a3a3ddcf963
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/18/2019
ms.locfileid: "47229912"
---
# <a name="delegates"></a>Delegáty

Delegáty umožňují scénáře další jazyky, jako je například C++, Pascal a Modula – mít řešit pomocí ukazatele na funkce. Na rozdíl od ukazatelů na funkce C++ ale delegáti jsou plně objektově orientované a na rozdíl od C++ ukazatelů na členské funkce, delegáti zapouzdřit instance objektu a metody.

Deklarace delegáta definuje třídu, která je odvozena od třídy `System.Delegate`. Instanci delegáta zapouzdřuje seznam vyvolání, což je seznam jedné nebo několika metod, z nichž každý se označuje jako volatelné entity. Pro instanci metody, volatelných entity se skládá z instance a metodu pro tuto instanci. Pro statické metody volatelných entity se skládá z právě metodu. Vyvolání delegáta instance s odpovídající sadu argumentů způsobí, že se všechny entity volatelných delegáta má být volána s danou sadu argumentů.

Zajímavé a užitečné vlastnictví instanci delegáta je, že ho neznáte nebo záleží tříd, metod, které zapouzdřuje; vše, co je důležité je, že tyto metody být kompatibilní ([delegovat deklarace](delegates.md#delegate-declarations)) s typem delegáta. Díky tomu delegáti dokonale hodí pro vyvolání "anonymní".

## <a name="delegate-declarations"></a>Deklarace delegáta

A *delegate_declaration* je *type_declaration* ([typ deklarace](namespaces.md#type-declarations)), který deklaruje nový typ delegáta.

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

Je chyba kompilace pro stejný modifikátor se zobrazí více než jednou v deklaraci delegáta.

`new` Modifikátor je povolené jenom u delegátů deklarované v rámci jiného typu, v takovém případě určuje, že takové delegáta skryje zděděný člen se stejným názvem, jak je popsáno v [new – modifikátor](classes.md#the-new-modifier).

`public`, `protected`, `internal`, A `private` řídit modifikátory přístupnosti typu delegáta. V závislosti na kontextu, ve kterém dochází k deklaraci delegáta, nemůže mu umožnit některé tyto modifikátory ([deklarovaná přístupnost](basic-concepts.md#declared-accessibility)).

Název typu delegáta je *identifikátor*.

Volitelný *formal_parameter_list* Určuje parametry delegáta a *typ* určuje návratový typ delegáta.

Volitelný *variant_type_parameter_list* ([seznamy parametru typu Variant](interfaces.md#variant-type-parameter-lists)) určuje parametry typu delegátu, samotného.

Návratový typ typ delegáta musí být buď `void`, nebo bezpečný výstup ([Variance bezpečnosti](interfaces.md#variance-safety)).

Všechny typy formálních parametrů typu delegáta musí odpovídat vstup typově bezpečné. Kromě toho některé `out` nebo `ref` typy parametrů musí být také výstup typově bezpečné. Poznámka: to dokonce i `out` parametry musejí být vstup typově bezpečný, kvůli omezením platformy pro základní spuštění.

Typy delegátů v jazyce C# jsou název ekvivalentní, není strukturálně ekvivalentní. Dva typy různých delegáta, které mají stejný parametr konkrétně obsahuje seznam a vrátit typ jsou považovány za delegáta různé typy. Však může instance dvou různých ale strukturálně ekvivalentní delegujících typů porovnat jako rovnocenné ([delegovat operátory rovnosti](expressions.md#delegate-equality-operators)).

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

Metody `A.M1` a `B.M1 `musí být kompatibilní s typy delegáta `D1` a `D2` , protože mají stejný typ a seznam parametrů vracet; však tyto typy delegátů jsou dva různé typy, takže už nejsou zaměnitelné. Metody `B.M2`, `B.M3`, a `B.M4` nejsou kompatibilní s typy delegáta `D1` a `D2`, protože mají různé typy vrácené hodnoty nebo seznamy parametrů.

Podobně jako ostatní deklarace obecného typu se musí předávat argumenty typu a vytvořte typ vytvořeného delegáta. Typy parametrů a návratový typ delegáta konstruovaný typ jsou vytvořené pro každý z parametrů typu v deklaraci delegáta nahraďte argument typu pro typ vytvořeného delegátu. Výsledný návratový typ a typy parametrů se používají při určování, které metody jsou kompatibilní s typem delegáta vytvořený. Příklad:

```csharp
delegate bool Predicate<T>(T value);

class X
{
    static bool F(int i) {...}
    static bool G(string s) {...}
}
```

Metoda `X.F` kompatibilní s typem delegáta `Predicate<int>` a metodu `X.G` kompatibilní s typem delegáta `Predicate<string>` .

Jediný způsob, jak deklarovat typ delegáta je prostřednictvím *delegate_declaration*. Typ delegáta je typu třídy, který je odvozen z `System.Delegate`. Typy delegátů jsou implicitně `sealed`, takže není povoleno pro odvození libovolný typ z typu delegáta. Také není povolený pro odvození třídy nedelegující typ z `System.Delegate`. Všimněte si, že `System.Delegate` je sám není typ delegáta; je typ třídy, ze které jsou odvozeny všechny typy delegátů.

C# poskytuje zvláštní syntaxi pro delegáta instance a vyvolání. Kromě vytváření instancí všechny operace, který lze použít na třídu nebo instanci třídy lze také použít delegát třídy nebo instance, v uvedeném pořadí. Zejména je možné získat přístup ke členům `System.Delegate` typu pomocí syntaxe přístup obvykle člena.

Sadu metod, které jsou zapouzdřena objektem instanci delegáta se nazývá seznam volání. Když je vytvořena instance delegáta ([delegovat kompatibility](delegates.md#delegate-compatibility)) z jedné metody zapouzdřuje metody a jeho vyvolávacím seznamu obsahuje pouze jednu položku. Ale kombinaci dvě instance s jinou hodnotu než null delegáta seznamy volání jsou zřetězeny – v pořadí vlevo operandem pak pravý operand – tvoří nový seznam vyvolání, která obsahuje dvě nebo více položek.

Delegáti jsou kombinované pomocí binárního souboru `+` ([operátor sčítání](expressions.md#addition-operator)) a `+=` operátory ([složené přiřazení](expressions.md#compound-assignment)). Delegát je možné odebrat ze kombinaci delegátů pomocí binárního souboru `-` ([operátor odčítání](expressions.md#subtraction-operator)) a `-=` operátory ([složené přiřazení](expressions.md#compound-assignment)). Delegáty lze porovnání rovnosti ([delegovat operátory rovnosti](expressions.md#delegate-equality-operators)).

Následující příklad ukazuje vytvoření instance počtu delegátů a uvádí jejich odpovídající volání:

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

Když `cd1` a `cd2` jsou vytvořena instance, každá zapouzdření jednu metodu. Když `cd3` je vytvořena instance, má seznam vyvolání ze dvou způsobů `M1` a `M2`v tomto pořadí. `cd4`v seznamu vyvolání obsahuje `M1`, `M2`, a `M1`v tomto pořadí. Nakonec `cd5`společnosti obsahuje seznam vyvolání `M1`, `M2`, `M1`, `M1`, a `M2`v tomto pořadí. Další příklady kombinování delegátů (také odebrat jako), najdete v článku [delegovat vyvolání](delegates.md#delegate-invocation).

## <a name="delegate-compatibility"></a>Delegát kompatibility

Metoda nebo delegát `M` je ***kompatibilní*** s typem delegáta `D` Pokud jsou splněny všechny z následujících akcí:

*  `D` a `M` mít stejný počet parametrů a každý parametr `D` má stejnou `ref` nebo `out` modifikátory jako odpovídající parametr v `M`.
*  Pro každý parametr hodnoty (parametr bez `ref` nebo `out` modifikátor), konverzi identity ([Identity převod](conversions.md#identity-conversion)) nebo implicitní převod odkazu ([odkaz na implicitní převody](conversions.md#implicit-reference-conversions)) z parametrů typu v `D` do odpovídajícího parametru typu v `M`.
*  Pro každou `ref` nebo `out` parametr, parametr typu v `D` je stejný jako typ parametru v `M`.
*  Existuje identity nebo implicitní referenční převod z návratového typu `M` na návratový typ `D`.

## <a name="delegate-instantiation"></a>Vytvoření instance delegáta

Se vytvoří instanci delegáta *delegate_creation_expression* ([delegovat vytváření výrazů](expressions.md#delegate-creation-expressions)) nebo převod na typ delegáta. Nově vytvořený delegát instance se pak odkazuje na buď:

*  Statická metoda, která odkazuje *delegate_creation_expression*, nebo
*  Cílový objekt (což nesmí být `null`) a instanční metodu odkazuje *delegate_creation_expression*, nebo
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

Po vytvoření instance, instance delegátů vždy odkazovat na stejném cílovém objektu a metody. Mějte na paměti, když dvou delegátů jsou zkombinované nebo jeden se odebere z jiného nové výsledky delegáta s vlastním seznamu vyvolání; vyvolání seznam delegátů kombinaci, nebo odebrání zůstanou beze změny.

## <a name="delegate-invocation"></a>Volání delegáta

C# poskytuje zvláštní syntaxi pro volání delegáta. Když uživatel vyvolá instanci delegáta jinou hodnotu než null, jehož seznamu vyvolání obsahuje jednu položku, vyvolá jednu metodu s byl zadán a vrátí stejnou hodnotu jako odkazovaný stejné argumenty metody. (Viz [delegáta volání](expressions.md#delegate-invocations) podrobné informace o volání delegáta.) Pokud dojde k výjimce při volání těchto delegáta a tato výjimka není zachycena v metodě, která byla vyvolána, vyhledávání pro klauzuli catch výjimky pokračuje v metodě, která volá delegáta, jako by měla tato metoda volána přímo označuje metodu, ke kterému delegovat.

Vyvolání delegáta instance, jejíž volání seznam obsahuje několik záznamů pokračuje tak, že každá z metod v seznamu vyvolání synchronně, vyvolá v pořadí. Každá metoda tedy volána je předán stejnou sadu argumentů, protože byl zadán pro instanci delegáta. Pokud taková volání delegáta obsahuje odkaz na parametry ([odkazovat na parametry](classes.md#reference-parameters)), dojde k každé volání metody s odkazem na stejnou proměnnou, budou změny na tuto proměnnou podle jedné metody v seznamu vyvolání je viditelné pro další metody vyvolání seznamu dolů. Pokud volání delegáta zahrnuje výstupní parametry nebo návratovou hodnotu, jejich konečnou hodnotu budou přicházet z vyvolání delegáta poslední v seznamu.

Pokud dojde k výjimce během zpracování volání těchto delegáta a tato výjimka není zachycena v metodě, která byla vyvolána, vyhledávání pro klauzuli catch výjimka pokračuje v metodě, která volá se, že delegát a jakékoli metody níže nejsou vyvolány seznamu vyvolání.

Pokus o vyvolání instanci delegáta, jehož hodnota je null za následek výjimku typu `System.NullReferenceException`.

Následující příklad ukazuje, jak vytvořit instanci, kombinovat, odebrat nebo vyvoláte:

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

Jak je znázorněno v příkazu `cd3 += cd1;`, delegát může být k dispozici v seznamu vyvolání více než jednou. V takovém případě stačí vyvolá se jednou za výskyt. V seznamu vyvolání takovou situaci při odebrání tohoto delegátu posledního výskytu v seznamu vyvolání je skutečně odebrat.

Bezprostředně před provedením poslední příkaz `cd3 -= cd1;`, delegát `cd3` odkazuje na seznam prázdný volání. Chcete-li odebrat delegáta z prázdného seznamu (nebo odebrat neexistující delegáta formu neprázdného seznamu), není to chyba.

Výstup vytvořený je:

```
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
