---
ms.openlocfilehash: d162d4b6a489032dcdfca9094a39d88fd03d4013
ms.sourcegitcommit: 892af9016b3317a8fae12d195014dc38ba51cf16
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/01/2019
ms.locfileid: "71704100"
---
# <a name="delegates"></a><span data-ttu-id="470e1-101">Delegáty</span><span class="sxs-lookup"><span data-stu-id="470e1-101">Delegates</span></span>

<span data-ttu-id="470e1-102">Delegáti umožňují scénáře, které jsou adresovány C++s ukazateli na funkce jinými jazyky – například, Pascal a modul.</span><span class="sxs-lookup"><span data-stu-id="470e1-102">Delegates enable scenarios that other languages—such as C++, Pascal, and Modula -- have addressed with function pointers.</span></span> <span data-ttu-id="470e1-103">Na rozdíl C++ od ukazatelů na funkce jsou však delegáti plně orientované na objekty a na rozdíl C++ od ukazatelů na členské funkce, delegáty zapouzdřují instanci objektu i metodu.</span><span class="sxs-lookup"><span data-stu-id="470e1-103">Unlike C++ function pointers, however, delegates are fully object oriented, and unlike C++ pointers to member functions, delegates encapsulate both an object instance and a method.</span></span>

<span data-ttu-id="470e1-104">Deklarace delegáta definuje třídu, která je odvozena od třídy `System.Delegate`.</span><span class="sxs-lookup"><span data-stu-id="470e1-104">A delegate declaration defines a class that is derived from the class `System.Delegate`.</span></span> <span data-ttu-id="470e1-105">Instance delegáta zapouzdřuje seznam vyvolání, což je seznam jedné nebo více metod, z nichž každý je označován jako volatelné entity.</span><span class="sxs-lookup"><span data-stu-id="470e1-105">A delegate instance encapsulates an invocation list, which is a list of one or more methods, each of which is referred to as a callable entity.</span></span> <span data-ttu-id="470e1-106">Pro metody instance se volatelné entity skládají z instance a metody v dané instanci.</span><span class="sxs-lookup"><span data-stu-id="470e1-106">For instance methods, a callable entity consists of an instance and a method on that instance.</span></span> <span data-ttu-id="470e1-107">U statických metod se volatelné entity skládá jenom z metody.</span><span class="sxs-lookup"><span data-stu-id="470e1-107">For static methods, a callable entity consists of just a method.</span></span> <span data-ttu-id="470e1-108">Vyvolání instance delegáta s vhodnou sadou argumentů způsobí, že každá z entit volání delegáta bude vyvolána s danou sadou argumentů.</span><span class="sxs-lookup"><span data-stu-id="470e1-108">Invoking a delegate instance with an appropriate set of arguments causes each of the delegate's callable entities to be invoked with the given set of arguments.</span></span>

<span data-ttu-id="470e1-109">Zajímavou a užitečnou vlastností instance delegáta je, že neznají ani nezáleží na třídách metod, které zapouzdřují. to vše je v tom, že tyto metody jsou kompatibilní ([deklarace delegátů](delegates.md#delegate-declarations)) s typem delegáta.</span><span class="sxs-lookup"><span data-stu-id="470e1-109">An interesting and useful property of a delegate instance is that it does not know or care about the classes of the methods it encapsulates; all that matters is that those methods be compatible ([Delegate declarations](delegates.md#delegate-declarations)) with the delegate's type.</span></span> <span data-ttu-id="470e1-110">Díky tomu jsou delegáti dokonale uzpůsobeni pro volání "anonymous".</span><span class="sxs-lookup"><span data-stu-id="470e1-110">This makes delegates perfectly suited for "anonymous" invocation.</span></span>

## <a name="delegate-declarations"></a><span data-ttu-id="470e1-111">Deklarace delegátů</span><span class="sxs-lookup"><span data-stu-id="470e1-111">Delegate declarations</span></span>

<span data-ttu-id="470e1-112">*Delegate_declaration* je *type_declaration* ([deklarace typu](namespaces.md#type-declarations)), který deklaruje nový typ delegáta.</span><span class="sxs-lookup"><span data-stu-id="470e1-112">A *delegate_declaration* is a *type_declaration* ([Type declarations](namespaces.md#type-declarations)) that declares a new delegate type.</span></span>

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

<span data-ttu-id="470e1-113">Jedná se o chybu při kompilaci, aby se stejný modifikátor v deklaraci delegáta zobrazoval víckrát.</span><span class="sxs-lookup"><span data-stu-id="470e1-113">It is a compile-time error for the same modifier to appear multiple times in a delegate declaration.</span></span>

<span data-ttu-id="470e1-114">Modifikátor `new` je povolen pouze u delegátů deklarovaných v jiném typu. v takovém případě určuje, že tento delegát skrývá zděděný člen se stejným názvem, jak je popsáno v tématu [nový modifikátor](classes.md#the-new-modifier).</span><span class="sxs-lookup"><span data-stu-id="470e1-114">The `new` modifier is only permitted on delegates declared within another type, in which case it specifies that such a delegate hides an inherited member by the same name, as described in [The new modifier](classes.md#the-new-modifier).</span></span>

<span data-ttu-id="470e1-115">Modifikátory `public`, `protected`, `internal` a `private` řídí přístupnost typu delegáta.</span><span class="sxs-lookup"><span data-stu-id="470e1-115">The `public`, `protected`, `internal`, and `private` modifiers control the accessibility of the delegate type.</span></span> <span data-ttu-id="470e1-116">V závislosti na kontextu, ve kterém dojde k deklaraci delegáta, nemusí být některé z těchto modifikátorů povoleny ([deklarovaná přístupnost](basic-concepts.md#declared-accessibility)).</span><span class="sxs-lookup"><span data-stu-id="470e1-116">Depending on the context in which the delegate declaration occurs, some of these modifiers may not be permitted ([Declared accessibility](basic-concepts.md#declared-accessibility)).</span></span>

<span data-ttu-id="470e1-117">Název typu delegáta je *identifikátor*.</span><span class="sxs-lookup"><span data-stu-id="470e1-117">The delegate's type name is *identifier*.</span></span>

<span data-ttu-id="470e1-118">Volitelné *formal_parameter_list* Určuje parametry delegáta a *Typ označuje návratový* typ delegátu.</span><span class="sxs-lookup"><span data-stu-id="470e1-118">The optional *formal_parameter_list* specifies the parameters of the delegate, and *return_type* indicates the return type of the delegate.</span></span>

<span data-ttu-id="470e1-119">Volitelné *variant_type_parameter_list* ([seznamy parametrů typu variant](interfaces.md#variant-type-parameter-lists)) určuje parametry typu samotného delegáta.</span><span class="sxs-lookup"><span data-stu-id="470e1-119">The optional *variant_type_parameter_list* ([Variant type parameter lists](interfaces.md#variant-type-parameter-lists)) specifies the type parameters to the delegate itself.</span></span>

<span data-ttu-id="470e1-120">Návratový typ typu delegáta musí být buď `void`, nebo bezpečný pro výstup ([zabezpečení odchylky](interfaces.md#variance-safety)).</span><span class="sxs-lookup"><span data-stu-id="470e1-120">The return type of a delegate type must be either `void`, or output-safe ([Variance safety](interfaces.md#variance-safety)).</span></span>

<span data-ttu-id="470e1-121">Všechny formální typy parametrů typu delegáta musí být bezpečné pro vstup.</span><span class="sxs-lookup"><span data-stu-id="470e1-121">All the formal parameter types of a delegate type must be input-safe.</span></span> <span data-ttu-id="470e1-122">Kromě toho musí být všechny typy parametrů `out` nebo `ref` také bezpečné pro výstup.</span><span class="sxs-lookup"><span data-stu-id="470e1-122">Additionally, any `out` or `ref` parameter types must also be output-safe.</span></span> <span data-ttu-id="470e1-123">Všimněte si, že i parametry `out` jsou nutné k bezpečnému vstupu z důvodu omezení základní spouštěcí platformy.</span><span class="sxs-lookup"><span data-stu-id="470e1-123">Note that even `out` parameters are required to be input-safe, due to a limitation of the underlying execution platform.</span></span>

<span data-ttu-id="470e1-124">Typy delegátů C# v jsou ekvivalentní název, ne strukturální ekvivalent.</span><span class="sxs-lookup"><span data-stu-id="470e1-124">Delegate types in C# are name equivalent, not structurally equivalent.</span></span> <span data-ttu-id="470e1-125">Konkrétně dva různé typy delegátů, které mají stejný seznam parametrů a návratový typ, se považují za jiné typy delegátů.</span><span class="sxs-lookup"><span data-stu-id="470e1-125">Specifically, two different delegate types that have the same parameter lists and return type are considered different delegate types.</span></span> <span data-ttu-id="470e1-126">Nicméně instance dvou jedinečných, ale strukturovaných ekvivalentních typů delegátů, mohou porovnat jako EQUAL ([operátory rovnosti delegátů](expressions.md#delegate-equality-operators)).</span><span class="sxs-lookup"><span data-stu-id="470e1-126">However, instances of two distinct but structurally equivalent delegate types may compare as equal ([Delegate equality operators](expressions.md#delegate-equality-operators)).</span></span>

<span data-ttu-id="470e1-127">Příklad:</span><span class="sxs-lookup"><span data-stu-id="470e1-127">For example:</span></span>

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

<span data-ttu-id="470e1-128">Metody `A.M1` a `B.M1` jsou kompatibilní s typy delegátů `D1` a `D2`, protože mají stejný návratový typ a seznam parametrů; Tyto typy delegátů jsou však dva různé typy, takže nejsou zaměnitelné.</span><span class="sxs-lookup"><span data-stu-id="470e1-128">The methods `A.M1` and `B.M1` are compatible with both the delegate types `D1` and `D2` , since they have the same return type and parameter list; however, these delegate types are two different types, so they are not interchangeable.</span></span> <span data-ttu-id="470e1-129">Metody `B.M2`, `B.M3` a `B.M4` jsou nekompatibilní s typy delegátů `D1` a `D2`, protože mají odlišné návratové typy nebo seznamy parametrů.</span><span class="sxs-lookup"><span data-stu-id="470e1-129">The methods `B.M2`, `B.M3`, and `B.M4` are incompatible with the delegate types `D1` and `D2`, since they have different return types or parameter lists.</span></span>

<span data-ttu-id="470e1-130">Podobně jako jiné deklarace obecného typu musí být předány argumenty typu pro vytvoření typu konstruovaného delegáta.</span><span class="sxs-lookup"><span data-stu-id="470e1-130">Like other generic type declarations, type arguments must be given to create a constructed delegate type.</span></span> <span data-ttu-id="470e1-131">Typy parametrů a návratový typ konstruovaného typu delegáta jsou vytvořeny nahrazením pro každý parametr typu v deklaraci delegáta odpovídajícím argumentem typu vytvořeného typu delegáta.</span><span class="sxs-lookup"><span data-stu-id="470e1-131">The parameter types and return type of a constructed delegate type are created by substituting, for each type parameter in the delegate declaration, the corresponding type argument of the constructed delegate type.</span></span> <span data-ttu-id="470e1-132">Výsledný návratový typ a typy parametrů se používají při určování, které metody jsou kompatibilní s vytvořeným typem delegáta.</span><span class="sxs-lookup"><span data-stu-id="470e1-132">The resulting return type and parameter types are used in determining what methods are compatible with a constructed delegate type.</span></span> <span data-ttu-id="470e1-133">Příklad:</span><span class="sxs-lookup"><span data-stu-id="470e1-133">For example:</span></span>

```csharp
delegate bool Predicate<T>(T value);

class X
{
    static bool F(int i) {...}
    static bool G(string s) {...}
}
```

<span data-ttu-id="470e1-134">Metoda `X.F` je kompatibilní s typem delegáta `Predicate<int>` a metoda `X.G` je kompatibilní s typem delegáta `Predicate<string>`.</span><span class="sxs-lookup"><span data-stu-id="470e1-134">The method `X.F` is compatible with the delegate type `Predicate<int>` and the method `X.G` is compatible with the delegate type `Predicate<string>` .</span></span>

<span data-ttu-id="470e1-135">Jediným způsobem, jak deklarovat typ delegáta, je prostřednictvím *delegate_declaration*.</span><span class="sxs-lookup"><span data-stu-id="470e1-135">The only way to declare a delegate type is via a *delegate_declaration*.</span></span> <span data-ttu-id="470e1-136">Typ delegáta je typ třídy, která je odvozena z `System.Delegate`.</span><span class="sxs-lookup"><span data-stu-id="470e1-136">A delegate type is a class type that is derived from `System.Delegate`.</span></span> <span data-ttu-id="470e1-137">Typy delegátů jsou implicitně `sealed`, takže není přípustné odvozovat žádný typ z typu delegáta.</span><span class="sxs-lookup"><span data-stu-id="470e1-137">Delegate types are implicitly `sealed`, so it is not permissible to derive any type from a delegate type.</span></span> <span data-ttu-id="470e1-138">Není také přípustné odvodit typ třídy bez delegáta z `System.Delegate`.</span><span class="sxs-lookup"><span data-stu-id="470e1-138">It is also not permissible to derive a non-delegate class type from `System.Delegate`.</span></span> <span data-ttu-id="470e1-139">Všimněte si, že `System.Delegate` není samotný typ delegáta. je to typ třídy, ze které jsou odvozeny všechny typy delegátů.</span><span class="sxs-lookup"><span data-stu-id="470e1-139">Note that `System.Delegate` is not itself a delegate type; it is a class type from which all delegate types are derived.</span></span>

<span data-ttu-id="470e1-140">C#poskytuje speciální syntaxi pro vytvoření instance delegáta a vyvolání.</span><span class="sxs-lookup"><span data-stu-id="470e1-140">C# provides special syntax for delegate instantiation and invocation.</span></span> <span data-ttu-id="470e1-141">S výjimkou vytváření instancí lze všechny operace, které lze použít na instanci třídy nebo třídy, použít také pro třídu nebo instanci delegáta v uvedeném pořadí.</span><span class="sxs-lookup"><span data-stu-id="470e1-141">Except for instantiation, any operation that can be applied to a class or class instance can also be applied to a delegate class or instance, respectively.</span></span> <span data-ttu-id="470e1-142">Konkrétně je možné přistupovat ke členům typu `System.Delegate` prostřednictvím běžné syntaxe přístupu členů.</span><span class="sxs-lookup"><span data-stu-id="470e1-142">In particular, it is possible to access members of the `System.Delegate` type via the usual member access syntax.</span></span>

<span data-ttu-id="470e1-143">Sada metod zapouzdřovaná instancí delegáta se nazývá seznam vyvolání.</span><span class="sxs-lookup"><span data-stu-id="470e1-143">The set of methods encapsulated by a delegate instance is called an invocation list.</span></span> <span data-ttu-id="470e1-144">Při vytvoření instance delegáta ([Kompatibilita s delegováním](delegates.md#delegate-compatibility)) z jediné metody zapouzdří tuto metodu a seznam volání obsahuje pouze jednu položku.</span><span class="sxs-lookup"><span data-stu-id="470e1-144">When a delegate instance is created ([Delegate compatibility](delegates.md#delegate-compatibility)) from a single method, it encapsulates that method, and its invocation list contains only one entry.</span></span> <span data-ttu-id="470e1-145">Nicméně pokud jsou kombinovány dvě instance delegátů, které nejsou null, jsou jejich seznamy volání zřetězeny – v levém operandu objednávky pak pravý operand – pro vytvoření nového seznamu volání, který obsahuje dvě nebo více položek.</span><span class="sxs-lookup"><span data-stu-id="470e1-145">However, when two non-null delegate instances are combined, their invocation lists are concatenated -- in the order left operand then right operand -- to form a new invocation list, which contains two or more entries.</span></span>

<span data-ttu-id="470e1-146">Delegáti jsou kombinováni pomocí binárního `+` ([operátor sčítání](expressions.md#addition-operator)) a operátorů `+=` ([složené přiřazení](expressions.md#compound-assignment)).</span><span class="sxs-lookup"><span data-stu-id="470e1-146">Delegates are combined using the binary `+` ([Addition operator](expressions.md#addition-operator)) and `+=` operators ([Compound assignment](expressions.md#compound-assignment)).</span></span> <span data-ttu-id="470e1-147">Delegáta lze odebrat z kombinace delegátů pomocí binárního `-` ([operátor odčítání](expressions.md#subtraction-operator)) a operátorů `-=` ([složené přiřazení](expressions.md#compound-assignment)).</span><span class="sxs-lookup"><span data-stu-id="470e1-147">A delegate can be removed from a combination of delegates, using the binary `-` ([Subtraction operator](expressions.md#subtraction-operator)) and `-=` operators ([Compound assignment](expressions.md#compound-assignment)).</span></span> <span data-ttu-id="470e1-148">Delegáty lze porovnávat s rovností ([delegované operátory rovnosti](expressions.md#delegate-equality-operators)).</span><span class="sxs-lookup"><span data-stu-id="470e1-148">Delegates can be compared for equality ([Delegate equality operators](expressions.md#delegate-equality-operators)).</span></span>

<span data-ttu-id="470e1-149">Následující příklad ukazuje vytváření instancí řady delegátů a jejich odpovídající seznamy volání:</span><span class="sxs-lookup"><span data-stu-id="470e1-149">The following example shows the instantiation of a number of delegates, and their corresponding invocation lists:</span></span>

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

<span data-ttu-id="470e1-150">Při vytváření instance `cd1` a `cd2` všechny zapouzdřují jednu metodu.</span><span class="sxs-lookup"><span data-stu-id="470e1-150">When `cd1` and `cd2` are instantiated, they each encapsulate one method.</span></span> <span data-ttu-id="470e1-151">Pokud je vytvořena instance `cd3`, má seznam volání dvou metod, `M1` a `M2`, v tomto pořadí.</span><span class="sxs-lookup"><span data-stu-id="470e1-151">When `cd3` is instantiated, it has an invocation list of two methods, `M1` and `M2`, in that order.</span></span> <span data-ttu-id="470e1-152">seznam volání `cd4` obsahuje `M1`, `M2` a `M1` v tomto pořadí.</span><span class="sxs-lookup"><span data-stu-id="470e1-152">`cd4`'s invocation list contains `M1`, `M2`, and `M1`, in that order.</span></span> <span data-ttu-id="470e1-153">Seznam volání `cd5` obsahuje `M1`, `M2`, `M1`, `M1` a `M2` v tomto pořadí.</span><span class="sxs-lookup"><span data-stu-id="470e1-153">Finally, `cd5`'s invocation list contains `M1`, `M2`, `M1`, `M1`, and `M2`, in that order.</span></span> <span data-ttu-id="470e1-154">Další příklady kombinování delegátů (stejně jako odebírání) najdete v tématu [delegování volání](delegates.md#delegate-invocation).</span><span class="sxs-lookup"><span data-stu-id="470e1-154">For more examples of combining (as well as removing) delegates, see [Delegate invocation](delegates.md#delegate-invocation).</span></span>

## <a name="delegate-compatibility"></a><span data-ttu-id="470e1-155">Kompatibilita delegáta</span><span class="sxs-lookup"><span data-stu-id="470e1-155">Delegate compatibility</span></span>

<span data-ttu-id="470e1-156">Metoda nebo delegát `M` je ***kompatibilní*** s typem delegáta `D`, pokud jsou splněny všechny následující podmínky:</span><span class="sxs-lookup"><span data-stu-id="470e1-156">A method or delegate `M` is ***compatible*** with a delegate type `D` if all of the following are true:</span></span>

*  <span data-ttu-id="470e1-157">`D` a `M` mají stejný počet parametrů a každý parametr v `D` má stejné modifikátory `ref` nebo `out` jako odpovídající parametr v `M`.</span><span class="sxs-lookup"><span data-stu-id="470e1-157">`D` and `M` have the same number of parameters, and each parameter in `D` has the same `ref` or `out` modifiers as the corresponding parameter in `M`.</span></span>
*  <span data-ttu-id="470e1-158">Pro každý parametr hodnoty (parametr bez modifikátoru `ref` nebo `out`) existuje převod identity ([převod identity](conversions.md#identity-conversion)) nebo implicitní převod odkazu ([implicitní převody odkazů](conversions.md#implicit-reference-conversions)) z typu parametru v `D` na odpovídající typ parametru v `M`.</span><span class="sxs-lookup"><span data-stu-id="470e1-158">For each value parameter (a parameter with no `ref` or `out` modifier), an identity conversion ([Identity conversion](conversions.md#identity-conversion)) or implicit reference conversion ([Implicit reference conversions](conversions.md#implicit-reference-conversions)) exists from the parameter type in `D` to the corresponding parameter type in `M`.</span></span>
*  <span data-ttu-id="470e1-159">Pro každý parametr `ref` nebo `out` je typ parametru v `D` stejný jako typ parametru v `M`.</span><span class="sxs-lookup"><span data-stu-id="470e1-159">For each `ref` or `out` parameter, the parameter type in `D` is the same as the parameter type in `M`.</span></span>
*  <span data-ttu-id="470e1-160">Identita nebo implicitní převod odkazu existují z návratového typu `M` do návratového typu `D`.</span><span class="sxs-lookup"><span data-stu-id="470e1-160">An identity or implicit reference conversion exists from the return type of `M` to the return type of `D`.</span></span>

## <a name="delegate-instantiation"></a><span data-ttu-id="470e1-161">Instance delegáta</span><span class="sxs-lookup"><span data-stu-id="470e1-161">Delegate instantiation</span></span>

<span data-ttu-id="470e1-162">Instance delegáta je vytvořena pomocí *delegate_creation_expression* ([výrazy pro vytvoření delegáta](expressions.md#delegate-creation-expressions)) nebo převod na typ delegáta.</span><span class="sxs-lookup"><span data-stu-id="470e1-162">An instance of a delegate is created by a *delegate_creation_expression* ([Delegate creation expressions](expressions.md#delegate-creation-expressions)) or a conversion to a delegate type.</span></span> <span data-ttu-id="470e1-163">Nově vytvořená instance delegáta pak odkazuje na jednu z těchto:</span><span class="sxs-lookup"><span data-stu-id="470e1-163">The newly created delegate instance then refers to either:</span></span>

*  <span data-ttu-id="470e1-164">Statická metoda, na kterou se odkazuje v *delegate_creation_expression*, nebo</span><span class="sxs-lookup"><span data-stu-id="470e1-164">The static method referenced in the *delegate_creation_expression*, or</span></span>
*  <span data-ttu-id="470e1-165">Cílový objekt (nemůže být `null`) a metoda instance, na kterou se odkazuje v *delegate_creation_expression*, nebo</span><span class="sxs-lookup"><span data-stu-id="470e1-165">The target object (which cannot be `null`) and instance method referenced in the *delegate_creation_expression*, or</span></span>
*  <span data-ttu-id="470e1-166">Jiný delegát.</span><span class="sxs-lookup"><span data-stu-id="470e1-166">Another delegate.</span></span>

<span data-ttu-id="470e1-167">Příklad:</span><span class="sxs-lookup"><span data-stu-id="470e1-167">For example:</span></span>

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

<span data-ttu-id="470e1-168">Po vytvoření instance Delegáti instance vždy odkazují na stejný cílový objekt a metodu.</span><span class="sxs-lookup"><span data-stu-id="470e1-168">Once instantiated, delegate instances always refer to the same target object and method.</span></span> <span data-ttu-id="470e1-169">Pamatujte na to, že pokud jsou dva Delegáti zkombinováni nebo když je jeden z nich odebraný, nové delegáty mají za následek vlastní seznam volání. seznamy volání delegátů, které se kombinují nebo odeberou, zůstávají beze změny.</span><span class="sxs-lookup"><span data-stu-id="470e1-169">Remember, when two delegates are combined, or one is removed from another, a new delegate results with its own invocation list; the invocation lists of the delegates combined or removed remain unchanged.</span></span>

## <a name="delegate-invocation"></a><span data-ttu-id="470e1-170">Volání delegáta</span><span class="sxs-lookup"><span data-stu-id="470e1-170">Delegate invocation</span></span>

<span data-ttu-id="470e1-171">C#poskytuje speciální syntaxi pro vyvolání delegáta.</span><span class="sxs-lookup"><span data-stu-id="470e1-171">C# provides special syntax for invoking a delegate.</span></span> <span data-ttu-id="470e1-172">Pokud je vyvolána nenulová instance delegáta, jejíž seznam volání obsahuje jednu položku, vyvolá jednu metodu se stejnými argumenty, která byla zadána, a vrátí stejnou hodnotu jako metoda, která je odkazována na metodu.</span><span class="sxs-lookup"><span data-stu-id="470e1-172">When a non-null delegate instance whose invocation list contains one entry is invoked, it invokes the one method with the same arguments it was given, and returns the same value as the referred to method.</span></span> <span data-ttu-id="470e1-173">(Další informace o vyvolání delegáta najdete v tématu věnovaném [voláním delegáta](expressions.md#delegate-invocations) .) Pokud dojde k výjimce během vyvolání takového delegáta a tato výjimka není zachycena v metodě, která byla vyvolána, hledání klauzule catch pro výjimku pokračuje v metodě, která se nazývá delegát, jako by tato metoda měla přímý odkaz na Metoda, na kterou se odkazuje tento delegát.</span><span class="sxs-lookup"><span data-stu-id="470e1-173">(See [Delegate invocations](expressions.md#delegate-invocations) for detailed information on delegate invocation.) If an exception occurs during the invocation of such a delegate, and that exception is not caught within the method that was invoked, the search for an exception catch clause continues in the method that called the delegate, as if that method had directly called the method to which that delegate referred.</span></span>

<span data-ttu-id="470e1-174">Vyvolání instance delegáta, jejíž seznam volání obsahuje více položek, se pokračuje vyvoláním každé z metod v seznamu vyvolání, synchronně v daném pořadí.</span><span class="sxs-lookup"><span data-stu-id="470e1-174">Invocation of a delegate instance whose invocation list contains multiple entries proceeds by invoking each of the methods in the invocation list, synchronously, in order.</span></span> <span data-ttu-id="470e1-175">Každá metoda, která má být volána, je předána stejnou sadou argumentů, jako byla předána instanci delegáta.</span><span class="sxs-lookup"><span data-stu-id="470e1-175">Each method so called is passed the same set of arguments as was given to the delegate instance.</span></span> <span data-ttu-id="470e1-176">Pokud takové vyvolání delegáta zahrnuje referenční parametry ([referenční parametry](classes.md#reference-parameters)), jednotlivé metody vyvolání budou provedeny s odkazem na stejnou proměnnou. změny v této proměnné podle jedné metody v seznamu volání budou viditelné i pro metody v rozevíracím seznamu vyvolání.</span><span class="sxs-lookup"><span data-stu-id="470e1-176">If such a delegate invocation includes reference parameters ([Reference parameters](classes.md#reference-parameters)), each method invocation will occur with a reference to the same variable; changes to that variable by one method in the invocation list will be visible to methods further down the invocation list.</span></span> <span data-ttu-id="470e1-177">Pokud vyvolání delegáta zahrnuje výstupní parametry nebo návratovou hodnotu, jejich konečná hodnota bude pocházet z volání posledního delegáta v seznamu.</span><span class="sxs-lookup"><span data-stu-id="470e1-177">If the delegate invocation includes output parameters or a return value, their final value will come from the invocation of the last delegate in the list.</span></span>

<span data-ttu-id="470e1-178">Pokud dojde k výjimce během zpracování vyvolání takového delegáta a tato výjimka není zachycena v metodě, která byla vyvolána, hledání klauzule catch pro výjimku pokračuje v metodě, která se nazývá delegát, a dalšími metodami mimo jiné. seznam volání není vyvolán.</span><span class="sxs-lookup"><span data-stu-id="470e1-178">If an exception occurs during processing of the invocation of such a delegate, and that exception is not caught within the method that was invoked, the search for an exception catch clause continues in the method that called the delegate, and any methods further down the invocation list are not invoked.</span></span>

<span data-ttu-id="470e1-179">Při pokusu o vyvolání instance delegáta, jejíž hodnota je null, dojde k výjimce typu `System.NullReferenceException`.</span><span class="sxs-lookup"><span data-stu-id="470e1-179">Attempting to invoke a delegate instance whose value is null results in an exception of type `System.NullReferenceException`.</span></span>

<span data-ttu-id="470e1-180">Následující příklad ukazuje, jak vytvořit instance, kombinovat, odebrat a vyvolat delegáty:</span><span class="sxs-lookup"><span data-stu-id="470e1-180">The following example shows how to instantiate, combine, remove, and invoke delegates:</span></span>

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

<span data-ttu-id="470e1-181">Jak je znázorněno v příkazu `cd3 += cd1;`, delegát může být přítomen v seznamu volání několikrát.</span><span class="sxs-lookup"><span data-stu-id="470e1-181">As shown in the statement `cd3 += cd1;`, a delegate can be present in an invocation list multiple times.</span></span> <span data-ttu-id="470e1-182">V tomto případě je jednoduše vyvolána jednou pro každý výskyt.</span><span class="sxs-lookup"><span data-stu-id="470e1-182">In this case, it is simply invoked once per occurrence.</span></span> <span data-ttu-id="470e1-183">V seznamu volání, jako je například, když je tento delegát odebrán, je poslední výskyt v seznamu volání ten, který je skutečně odebraný.</span><span class="sxs-lookup"><span data-stu-id="470e1-183">In an invocation list such as this, when that delegate is removed, the last occurrence in the invocation list is the one actually removed.</span></span>

<span data-ttu-id="470e1-184">Bezprostředně před provedením závěrečného příkazu, `cd3 -= cd1;`, delegát `cd3` odkazuje na prázdný seznam volání.</span><span class="sxs-lookup"><span data-stu-id="470e1-184">Immediately prior to the execution of the final statement, `cd3 -= cd1;`, the delegate `cd3` refers to an empty invocation list.</span></span> <span data-ttu-id="470e1-185">Pokus o odebrání delegáta z prázdného seznamu (nebo pro odebrání neexistujícího delegáta ze seznamu, který není prázdný) není chyba.</span><span class="sxs-lookup"><span data-stu-id="470e1-185">Attempting to remove a delegate from an empty list (or to remove a non-existent delegate from a non-empty list) is not an error.</span></span>

<span data-ttu-id="470e1-186">Vytvářený výstup je:</span><span class="sxs-lookup"><span data-stu-id="470e1-186">The output produced is:</span></span>

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
