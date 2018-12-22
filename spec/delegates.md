# <a name="delegates"></a><span data-ttu-id="c681d-101">Delegáty</span><span class="sxs-lookup"><span data-stu-id="c681d-101">Delegates</span></span>

<span data-ttu-id="c681d-102">Delegáty umožňují scénáře další jazyky, jako je například C++, Pascal a Modula – mít řešit pomocí ukazatele na funkce.</span><span class="sxs-lookup"><span data-stu-id="c681d-102">Delegates enable scenarios that other languages—such as C++, Pascal, and Modula -- have addressed with function pointers.</span></span> <span data-ttu-id="c681d-103">Na rozdíl od ukazatelů na funkce C++ ale delegáti jsou plně objektově orientované a na rozdíl od C++ ukazatelů na členské funkce, delegáti zapouzdřit instance objektu a metody.</span><span class="sxs-lookup"><span data-stu-id="c681d-103">Unlike C++ function pointers, however, delegates are fully object oriented, and unlike C++ pointers to member functions, delegates encapsulate both an object instance and a method.</span></span>

<span data-ttu-id="c681d-104">Deklarace delegáta definuje třídu, která je odvozena od třídy `System.Delegate`.</span><span class="sxs-lookup"><span data-stu-id="c681d-104">A delegate declaration defines a class that is derived from the class `System.Delegate`.</span></span> <span data-ttu-id="c681d-105">Instanci delegáta zapouzdřuje seznam vyvolání, což je seznam jedné nebo několika metod, z nichž každý se označuje jako volatelné entity.</span><span class="sxs-lookup"><span data-stu-id="c681d-105">A delegate instance encapsulates an invocation list, which is a list of one or more methods, each of which is referred to as a callable entity.</span></span> <span data-ttu-id="c681d-106">Pro instanci metody, volatelných entity se skládá z instance a metodu pro tuto instanci.</span><span class="sxs-lookup"><span data-stu-id="c681d-106">For instance methods, a callable entity consists of an instance and a method on that instance.</span></span> <span data-ttu-id="c681d-107">Pro statické metody volatelných entity se skládá z právě metodu.</span><span class="sxs-lookup"><span data-stu-id="c681d-107">For static methods, a callable entity consists of just a method.</span></span> <span data-ttu-id="c681d-108">Vyvolání delegáta instance s odpovídající sadu argumentů způsobí, že se všechny entity volatelných delegáta má být volána s danou sadu argumentů.</span><span class="sxs-lookup"><span data-stu-id="c681d-108">Invoking a delegate instance with an appropriate set of arguments causes each of the delegate's callable entities to be invoked with the given set of arguments.</span></span>

<span data-ttu-id="c681d-109">Zajímavé a užitečné vlastnictví instanci delegáta je, že ho neznáte nebo záleží tříd, metod, které zapouzdřuje; vše, co je důležité je, že tyto metody být kompatibilní ([delegovat deklarace](delegates.md#delegate-declarations)) s typem delegáta.</span><span class="sxs-lookup"><span data-stu-id="c681d-109">An interesting and useful property of a delegate instance is that it does not know or care about the classes of the methods it encapsulates; all that matters is that those methods be compatible ([Delegate declarations](delegates.md#delegate-declarations)) with the delegate's type.</span></span> <span data-ttu-id="c681d-110">Díky tomu delegáti dokonale hodí pro vyvolání "anonymní".</span><span class="sxs-lookup"><span data-stu-id="c681d-110">This makes delegates perfectly suited for "anonymous" invocation.</span></span>

## <a name="delegate-declarations"></a><span data-ttu-id="c681d-111">Deklarace delegáta</span><span class="sxs-lookup"><span data-stu-id="c681d-111">Delegate declarations</span></span>

<span data-ttu-id="c681d-112">A *delegate_declaration* je *type_declaration* ([typ deklarace](namespaces.md#type-declarations)), který deklaruje nový typ delegáta.</span><span class="sxs-lookup"><span data-stu-id="c681d-112">A *delegate_declaration* is a *type_declaration* ([Type declarations](namespaces.md#type-declarations)) that declares a new delegate type.</span></span>

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

<span data-ttu-id="c681d-113">Je chyba kompilace pro stejný modifikátor se zobrazí více než jednou v deklaraci delegáta.</span><span class="sxs-lookup"><span data-stu-id="c681d-113">It is a compile-time error for the same modifier to appear multiple times in a delegate declaration.</span></span>

<span data-ttu-id="c681d-114">`new` Modifikátor je povolené jenom u delegátů deklarované v rámci jiného typu, v takovém případě určuje, že takové delegáta skryje zděděný člen se stejným názvem, jak je popsáno v [new – modifikátor](classes.md#the-new-modifier).</span><span class="sxs-lookup"><span data-stu-id="c681d-114">The `new` modifier is only permitted on delegates declared within another type, in which case it specifies that such a delegate hides an inherited member by the same name, as described in [The new modifier](classes.md#the-new-modifier).</span></span>

<span data-ttu-id="c681d-115">`public`, `protected`, `internal`, A `private` řídit modifikátory přístupnosti typu delegáta.</span><span class="sxs-lookup"><span data-stu-id="c681d-115">The `public`, `protected`, `internal`, and `private` modifiers control the accessibility of the delegate type.</span></span> <span data-ttu-id="c681d-116">V závislosti na kontextu, ve kterém dochází k deklaraci delegáta, nemůže mu umožnit některé tyto modifikátory ([deklarovaná přístupnost](basic-concepts.md#declared-accessibility)).</span><span class="sxs-lookup"><span data-stu-id="c681d-116">Depending on the context in which the delegate declaration occurs, some of these modifiers may not be permitted ([Declared accessibility](basic-concepts.md#declared-accessibility)).</span></span>

<span data-ttu-id="c681d-117">Název typu delegáta je *identifikátor*.</span><span class="sxs-lookup"><span data-stu-id="c681d-117">The delegate's type name is *identifier*.</span></span>

<span data-ttu-id="c681d-118">Volitelný *formal_parameter_list* Určuje parametry delegáta a *typ* určuje návratový typ delegáta.</span><span class="sxs-lookup"><span data-stu-id="c681d-118">The optional *formal_parameter_list* specifies the parameters of the delegate, and *return_type* indicates the return type of the delegate.</span></span>

<span data-ttu-id="c681d-119">Volitelný *variant_type_parameter_list* ([seznamy parametru typu Variant](interfaces.md#variant-type-parameter-lists)) určuje parametry typu delegátu, samotného.</span><span class="sxs-lookup"><span data-stu-id="c681d-119">The optional *variant_type_parameter_list* ([Variant type parameter lists](interfaces.md#variant-type-parameter-lists)) specifies the type parameters to the delegate itself.</span></span>

<span data-ttu-id="c681d-120">Návratový typ typ delegáta musí být buď `void`, nebo bezpečný výstup ([Variance bezpečnosti](interfaces.md#variance-safety)).</span><span class="sxs-lookup"><span data-stu-id="c681d-120">The return type of a delegate type must be either `void`, or output-safe ([Variance safety](interfaces.md#variance-safety)).</span></span>

<span data-ttu-id="c681d-121">Všechny typy formálních parametrů typu delegáta musí odpovídat vstup typově bezpečné.</span><span class="sxs-lookup"><span data-stu-id="c681d-121">All the formal parameter types of a delegate type must be input-safe.</span></span> <span data-ttu-id="c681d-122">Kromě toho některé `out` nebo `ref` typy parametrů musí být také výstup typově bezpečné.</span><span class="sxs-lookup"><span data-stu-id="c681d-122">Additionally, any `out` or `ref` parameter types must also be output-safe.</span></span> <span data-ttu-id="c681d-123">Poznámka: to dokonce i `out` parametry musejí být vstup typově bezpečný, kvůli omezením platformy pro základní spuštění.</span><span class="sxs-lookup"><span data-stu-id="c681d-123">Note that even `out` parameters are required to be input-safe, due to a limitation of the underlying execution platform.</span></span>

<span data-ttu-id="c681d-124">Typy delegátů v jazyce C# jsou název ekvivalentní, není strukturálně ekvivalentní.</span><span class="sxs-lookup"><span data-stu-id="c681d-124">Delegate types in C# are name equivalent, not structurally equivalent.</span></span> <span data-ttu-id="c681d-125">Dva typy různých delegáta, které mají stejný parametr konkrétně obsahuje seznam a vrátit typ jsou považovány za delegáta různé typy.</span><span class="sxs-lookup"><span data-stu-id="c681d-125">Specifically, two different delegate types that have the same parameter lists and return type are considered different delegate types.</span></span> <span data-ttu-id="c681d-126">Však může instance dvou různých ale strukturálně ekvivalentní delegujících typů porovnat jako rovnocenné ([delegovat operátory rovnosti](expressions.md#delegate-equality-operators)).</span><span class="sxs-lookup"><span data-stu-id="c681d-126">However, instances of two distinct but structurally equivalent delegate types may compare as equal ([Delegate equality operators](expressions.md#delegate-equality-operators)).</span></span>

<span data-ttu-id="c681d-127">Příklad:</span><span class="sxs-lookup"><span data-stu-id="c681d-127">For example:</span></span>

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

<span data-ttu-id="c681d-128">Metody `A.M1` a `B.M1 `musí být kompatibilní s typy delegáta `D1` a `D2` , protože mají stejný typ a seznam parametrů vracet; však tyto typy delegátů jsou dva různé typy, takže už nejsou zaměnitelné.</span><span class="sxs-lookup"><span data-stu-id="c681d-128">The methods `A.M1` and `B.M1 `are compatible with both the delegate types `D1` and `D2` , since they have the same return type and parameter list; however, these delegate types are two different types, so they are not interchangeable.</span></span> <span data-ttu-id="c681d-129">Metody `B.M2`, `B.M3`, a `B.M4` nejsou kompatibilní s typy delegáta `D1` a `D2`, protože mají různé typy vrácené hodnoty nebo seznamy parametrů.</span><span class="sxs-lookup"><span data-stu-id="c681d-129">The methods `B.M2`, `B.M3`, and `B.M4` are incompatible with the delegate types `D1` and `D2`, since they have different return types or parameter lists.</span></span>

<span data-ttu-id="c681d-130">Podobně jako ostatní deklarace obecného typu se musí předávat argumenty typu a vytvořte typ vytvořeného delegáta.</span><span class="sxs-lookup"><span data-stu-id="c681d-130">Like other generic type declarations, type arguments must be given to create a constructed delegate type.</span></span> <span data-ttu-id="c681d-131">Typy parametrů a návratový typ delegáta konstruovaný typ jsou vytvořené pro každý z parametrů typu v deklaraci delegáta nahraďte argument typu pro typ vytvořeného delegátu.</span><span class="sxs-lookup"><span data-stu-id="c681d-131">The parameter types and return type of a constructed delegate type are created by substituting, for each type parameter in the delegate declaration, the corresponding type argument of the constructed delegate type.</span></span> <span data-ttu-id="c681d-132">Výsledný návratový typ a typy parametrů se používají při určování, které metody jsou kompatibilní s typem delegáta vytvořený.</span><span class="sxs-lookup"><span data-stu-id="c681d-132">The resulting return type and parameter types are used in determining what methods are compatible with a constructed delegate type.</span></span> <span data-ttu-id="c681d-133">Příklad:</span><span class="sxs-lookup"><span data-stu-id="c681d-133">For example:</span></span>

```csharp
delegate bool Predicate<T>(T value);

class X
{
    static bool F(int i) {...}
    static bool G(string s) {...}
}
```

<span data-ttu-id="c681d-134">Metoda `X.F` kompatibilní s typem delegáta `Predicate<int>` a metodu `X.G` kompatibilní s typem delegáta `Predicate<string>` .</span><span class="sxs-lookup"><span data-stu-id="c681d-134">The method `X.F` is compatible with the delegate type `Predicate<int>` and the method `X.G` is compatible with the delegate type `Predicate<string>` .</span></span>

<span data-ttu-id="c681d-135">Jediný způsob, jak deklarovat typ delegáta je prostřednictvím *delegate_declaration*.</span><span class="sxs-lookup"><span data-stu-id="c681d-135">The only way to declare a delegate type is via a *delegate_declaration*.</span></span> <span data-ttu-id="c681d-136">Typ delegáta je typu třídy, který je odvozen z `System.Delegate`.</span><span class="sxs-lookup"><span data-stu-id="c681d-136">A delegate type is a class type that is derived from `System.Delegate`.</span></span> <span data-ttu-id="c681d-137">Typy delegátů jsou implicitně `sealed`, takže není povoleno pro odvození libovolný typ z typu delegáta.</span><span class="sxs-lookup"><span data-stu-id="c681d-137">Delegate types are implicitly `sealed`, so it is not permissible to derive any type from a delegate type.</span></span> <span data-ttu-id="c681d-138">Také není povolený pro odvození třídy nedelegující typ z `System.Delegate`.</span><span class="sxs-lookup"><span data-stu-id="c681d-138">It is also not permissible to derive a non-delegate class type from `System.Delegate`.</span></span> <span data-ttu-id="c681d-139">Všimněte si, že `System.Delegate` je sám není typ delegáta; je typ třídy, ze které jsou odvozeny všechny typy delegátů.</span><span class="sxs-lookup"><span data-stu-id="c681d-139">Note that `System.Delegate` is not itself a delegate type; it is a class type from which all delegate types are derived.</span></span>

<span data-ttu-id="c681d-140">C# poskytuje zvláštní syntaxi pro delegáta instance a vyvolání.</span><span class="sxs-lookup"><span data-stu-id="c681d-140">C# provides special syntax for delegate instantiation and invocation.</span></span> <span data-ttu-id="c681d-141">Kromě vytváření instancí všechny operace, který lze použít na třídu nebo instanci třídy lze také použít delegát třídy nebo instance, v uvedeném pořadí.</span><span class="sxs-lookup"><span data-stu-id="c681d-141">Except for instantiation, any operation that can be applied to a class or class instance can also be applied to a delegate class or instance, respectively.</span></span> <span data-ttu-id="c681d-142">Zejména je možné získat přístup ke členům `System.Delegate` typu pomocí syntaxe přístup obvykle člena.</span><span class="sxs-lookup"><span data-stu-id="c681d-142">In particular, it is possible to access members of the `System.Delegate` type via the usual member access syntax.</span></span>

<span data-ttu-id="c681d-143">Sadu metod, které jsou zapouzdřena objektem instanci delegáta se nazývá seznam volání.</span><span class="sxs-lookup"><span data-stu-id="c681d-143">The set of methods encapsulated by a delegate instance is called an invocation list.</span></span> <span data-ttu-id="c681d-144">Když je vytvořena instance delegáta ([delegovat kompatibility](delegates.md#delegate-compatibility)) z jedné metody zapouzdřuje metody a jeho vyvolávacím seznamu obsahuje pouze jednu položku.</span><span class="sxs-lookup"><span data-stu-id="c681d-144">When a delegate instance is created ([Delegate compatibility](delegates.md#delegate-compatibility)) from a single method, it encapsulates that method, and its invocation list contains only one entry.</span></span> <span data-ttu-id="c681d-145">Ale kombinaci dvě instance s jinou hodnotu než null delegáta seznamy volání jsou zřetězeny – v pořadí vlevo operandem pak pravý operand – tvoří nový seznam vyvolání, která obsahuje dvě nebo více položek.</span><span class="sxs-lookup"><span data-stu-id="c681d-145">However, when two non-null delegate instances are combined, their invocation lists are concatenated -- in the order left operand then right operand -- to form a new invocation list, which contains two or more entries.</span></span>

<span data-ttu-id="c681d-146">Delegáti jsou kombinované pomocí binárního souboru `+` ([operátor sčítání](expressions.md#addition-operator)) a `+=` operátory ([složené přiřazení](expressions.md#compound-assignment)).</span><span class="sxs-lookup"><span data-stu-id="c681d-146">Delegates are combined using the binary `+` ([Addition operator](expressions.md#addition-operator)) and `+=` operators ([Compound assignment](expressions.md#compound-assignment)).</span></span> <span data-ttu-id="c681d-147">Delegát je možné odebrat ze kombinaci delegátů pomocí binárního souboru `-` ([operátor odčítání](expressions.md#subtraction-operator)) a `-=` operátory ([složené přiřazení](expressions.md#compound-assignment)).</span><span class="sxs-lookup"><span data-stu-id="c681d-147">A delegate can be removed from a combination of delegates, using the binary `-` ([Subtraction operator](expressions.md#subtraction-operator)) and `-=` operators ([Compound assignment](expressions.md#compound-assignment)).</span></span> <span data-ttu-id="c681d-148">Delegáty lze porovnání rovnosti ([delegovat operátory rovnosti](expressions.md#delegate-equality-operators)).</span><span class="sxs-lookup"><span data-stu-id="c681d-148">Delegates can be compared for equality ([Delegate equality operators](expressions.md#delegate-equality-operators)).</span></span>

<span data-ttu-id="c681d-149">Následující příklad ukazuje vytvoření instance počtu delegátů a uvádí jejich odpovídající volání:</span><span class="sxs-lookup"><span data-stu-id="c681d-149">The following example shows the instantiation of a number of delegates, and their corresponding invocation lists:</span></span>

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

<span data-ttu-id="c681d-150">Když `cd1` a `cd2` jsou vytvořena instance, každá zapouzdření jednu metodu.</span><span class="sxs-lookup"><span data-stu-id="c681d-150">When `cd1` and `cd2` are instantiated, they each encapsulate one method.</span></span> <span data-ttu-id="c681d-151">Když `cd3` je vytvořena instance, má seznam vyvolání ze dvou způsobů `M1` a `M2`v tomto pořadí.</span><span class="sxs-lookup"><span data-stu-id="c681d-151">When `cd3` is instantiated, it has an invocation list of two methods, `M1` and `M2`, in that order.</span></span> <span data-ttu-id="c681d-152">`cd4`v seznamu vyvolání obsahuje `M1`, `M2`, a `M1`v tomto pořadí.</span><span class="sxs-lookup"><span data-stu-id="c681d-152">`cd4`'s invocation list contains `M1`, `M2`, and `M1`, in that order.</span></span> <span data-ttu-id="c681d-153">Nakonec `cd5`společnosti obsahuje seznam vyvolání `M1`, `M2`, `M1`, `M1`, a `M2`v tomto pořadí.</span><span class="sxs-lookup"><span data-stu-id="c681d-153">Finally, `cd5`'s invocation list contains `M1`, `M2`, `M1`, `M1`, and `M2`, in that order.</span></span> <span data-ttu-id="c681d-154">Další příklady kombinování delegátů (také odebrat jako), najdete v článku [delegovat vyvolání](delegates.md#delegate-invocation).</span><span class="sxs-lookup"><span data-stu-id="c681d-154">For more examples of combining (as well as removing) delegates, see [Delegate invocation](delegates.md#delegate-invocation).</span></span>

## <a name="delegate-compatibility"></a><span data-ttu-id="c681d-155">Delegát kompatibility</span><span class="sxs-lookup"><span data-stu-id="c681d-155">Delegate compatibility</span></span>

<span data-ttu-id="c681d-156">Metoda nebo delegát `M` je ***kompatibilní*** s typem delegáta `D` Pokud jsou splněny všechny z následujících akcí:</span><span class="sxs-lookup"><span data-stu-id="c681d-156">A method or delegate `M` is ***compatible*** with a delegate type `D` if all of the following are true:</span></span>

*  <span data-ttu-id="c681d-157">`D` a `M` mít stejný počet parametrů a každý parametr `D` má stejnou `ref` nebo `out` modifikátory jako odpovídající parametr v `M`.</span><span class="sxs-lookup"><span data-stu-id="c681d-157">`D` and `M` have the same number of parameters, and each parameter in `D` has the same `ref` or `out` modifiers as the corresponding parameter in `M`.</span></span>
*  <span data-ttu-id="c681d-158">Pro každý parametr hodnoty (parametr bez `ref` nebo `out` modifikátor), konverzi identity ([Identity převod](conversions.md#identity-conversion)) nebo implicitní převod odkazu ([odkaz na implicitní převody](conversions.md#implicit-reference-conversions)) z parametrů typu v `D` do odpovídajícího parametru typu v `M`.</span><span class="sxs-lookup"><span data-stu-id="c681d-158">For each value parameter (a parameter with no `ref` or `out` modifier), an identity conversion ([Identity conversion](conversions.md#identity-conversion)) or implicit reference conversion ([Implicit reference conversions](conversions.md#implicit-reference-conversions)) exists from the parameter type in `D` to the corresponding parameter type in `M`.</span></span>
*  <span data-ttu-id="c681d-159">Pro každou `ref` nebo `out` parametr, parametr typu v `D` je stejný jako typ parametru v `M`.</span><span class="sxs-lookup"><span data-stu-id="c681d-159">For each `ref` or `out` parameter, the parameter type in `D` is the same as the parameter type in `M`.</span></span>
*  <span data-ttu-id="c681d-160">Existuje identity nebo implicitní referenční převod z návratového typu `M` na návratový typ `D`.</span><span class="sxs-lookup"><span data-stu-id="c681d-160">An identity or implicit reference conversion exists from the return type of `M` to the return type of `D`.</span></span>

## <a name="delegate-instantiation"></a><span data-ttu-id="c681d-161">Vytvoření instance delegáta</span><span class="sxs-lookup"><span data-stu-id="c681d-161">Delegate instantiation</span></span>

<span data-ttu-id="c681d-162">Se vytvoří instanci delegáta *delegate_creation_expression* ([delegovat vytváření výrazů](expressions.md#delegate-creation-expressions)) nebo převod na typ delegáta.</span><span class="sxs-lookup"><span data-stu-id="c681d-162">An instance of a delegate is created by a *delegate_creation_expression* ([Delegate creation expressions](expressions.md#delegate-creation-expressions)) or a conversion to a delegate type.</span></span> <span data-ttu-id="c681d-163">Nově vytvořený delegát instance se pak odkazuje na buď:</span><span class="sxs-lookup"><span data-stu-id="c681d-163">The newly created delegate instance then refers to either:</span></span>

*  <span data-ttu-id="c681d-164">Statická metoda, která odkazuje *delegate_creation_expression*, nebo</span><span class="sxs-lookup"><span data-stu-id="c681d-164">The static method referenced in the *delegate_creation_expression*, or</span></span>
*  <span data-ttu-id="c681d-165">Cílový objekt (což nesmí být `null`) a instanční metodu odkazuje *delegate_creation_expression*, nebo</span><span class="sxs-lookup"><span data-stu-id="c681d-165">The target object (which cannot be `null`) and instance method referenced in the *delegate_creation_expression*, or</span></span>
*  <span data-ttu-id="c681d-166">Jiný delegát.</span><span class="sxs-lookup"><span data-stu-id="c681d-166">Another delegate.</span></span>

<span data-ttu-id="c681d-167">Příklad:</span><span class="sxs-lookup"><span data-stu-id="c681d-167">For example:</span></span>

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

<span data-ttu-id="c681d-168">Po vytvoření instance, instance delegátů vždy odkazovat na stejném cílovém objektu a metody.</span><span class="sxs-lookup"><span data-stu-id="c681d-168">Once instantiated, delegate instances always refer to the same target object and method.</span></span> <span data-ttu-id="c681d-169">Mějte na paměti, když dvou delegátů jsou zkombinované nebo jeden se odebere z jiného nové výsledky delegáta s vlastním seznamu vyvolání; vyvolání seznam delegátů kombinaci, nebo odebrání zůstanou beze změny.</span><span class="sxs-lookup"><span data-stu-id="c681d-169">Remember, when two delegates are combined, or one is removed from another, a new delegate results with its own invocation list; the invocation lists of the delegates combined or removed remain unchanged.</span></span>

## <a name="delegate-invocation"></a><span data-ttu-id="c681d-170">Volání delegáta</span><span class="sxs-lookup"><span data-stu-id="c681d-170">Delegate invocation</span></span>

<span data-ttu-id="c681d-171">C# poskytuje zvláštní syntaxi pro volání delegáta.</span><span class="sxs-lookup"><span data-stu-id="c681d-171">C# provides special syntax for invoking a delegate.</span></span> <span data-ttu-id="c681d-172">Když uživatel vyvolá instanci delegáta jinou hodnotu než null, jehož seznamu vyvolání obsahuje jednu položku, vyvolá jednu metodu s byl zadán a vrátí stejnou hodnotu jako odkazovaný stejné argumenty metody.</span><span class="sxs-lookup"><span data-stu-id="c681d-172">When a non-null delegate instance whose invocation list contains one entry is invoked, it invokes the one method with the same arguments it was given, and returns the same value as the referred to method.</span></span> <span data-ttu-id="c681d-173">(Viz [delegáta volání](expressions.md#delegate-invocations) podrobné informace o volání delegáta.) Pokud dojde k výjimce při volání těchto delegáta a tato výjimka není zachycena v metodě, která byla vyvolána, vyhledávání pro klauzuli catch výjimky pokračuje v metodě, která volá delegáta, jako by měla tato metoda volána přímo označuje metodu, ke kterému delegovat.</span><span class="sxs-lookup"><span data-stu-id="c681d-173">(See [Delegate invocations](expressions.md#delegate-invocations) for detailed information on delegate invocation.) If an exception occurs during the invocation of such a delegate, and that exception is not caught within the method that was invoked, the search for an exception catch clause continues in the method that called the delegate, as if that method had directly called the method to which that delegate referred.</span></span>

<span data-ttu-id="c681d-174">Vyvolání delegáta instance, jejíž volání seznam obsahuje několik záznamů pokračuje tak, že každá z metod v seznamu vyvolání synchronně, vyvolá v pořadí.</span><span class="sxs-lookup"><span data-stu-id="c681d-174">Invocation of a delegate instance whose invocation list contains multiple entries proceeds by invoking each of the methods in the invocation list, synchronously, in order.</span></span> <span data-ttu-id="c681d-175">Každá metoda tedy volána je předán stejnou sadu argumentů, protože byl zadán pro instanci delegáta.</span><span class="sxs-lookup"><span data-stu-id="c681d-175">Each method so called is passed the same set of arguments as was given to the delegate instance.</span></span> <span data-ttu-id="c681d-176">Pokud taková volání delegáta obsahuje odkaz na parametry ([odkazovat na parametry](classes.md#reference-parameters)), dojde k každé volání metody s odkazem na stejnou proměnnou, budou změny na tuto proměnnou podle jedné metody v seznamu vyvolání je viditelné pro další metody vyvolání seznamu dolů.</span><span class="sxs-lookup"><span data-stu-id="c681d-176">If such a delegate invocation includes reference parameters ([Reference parameters](classes.md#reference-parameters)), each method invocation will occur with a reference to the same variable; changes to that variable by one method in the invocation list will be visible to methods further down the invocation list.</span></span> <span data-ttu-id="c681d-177">Pokud volání delegáta zahrnuje výstupní parametry nebo návratovou hodnotu, jejich konečnou hodnotu budou přicházet z vyvolání delegáta poslední v seznamu.</span><span class="sxs-lookup"><span data-stu-id="c681d-177">If the delegate invocation includes output parameters or a return value, their final value will come from the invocation of the last delegate in the list.</span></span>

<span data-ttu-id="c681d-178">Pokud dojde k výjimce během zpracování volání těchto delegáta a tato výjimka není zachycena v metodě, která byla vyvolána, vyhledávání pro klauzuli catch výjimka pokračuje v metodě, která volá se, že delegát a jakékoli metody níže nejsou vyvolány seznamu vyvolání.</span><span class="sxs-lookup"><span data-stu-id="c681d-178">If an exception occurs during processing of the invocation of such a delegate, and that exception is not caught within the method that was invoked, the search for an exception catch clause continues in the method that called the delegate, and any methods further down the invocation list are not invoked.</span></span>

<span data-ttu-id="c681d-179">Pokus o vyvolání instanci delegáta, jehož hodnota je null za následek výjimku typu `System.NullReferenceException`.</span><span class="sxs-lookup"><span data-stu-id="c681d-179">Attempting to invoke a delegate instance whose value is null results in an exception of type `System.NullReferenceException`.</span></span>

<span data-ttu-id="c681d-180">Následující příklad ukazuje, jak vytvořit instanci, kombinovat, odebrat nebo vyvoláte:</span><span class="sxs-lookup"><span data-stu-id="c681d-180">The following example shows how to instantiate, combine, remove, and invoke delegates:</span></span>

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

<span data-ttu-id="c681d-181">Jak je znázorněno v příkazu `cd3 += cd1;`, delegát může být k dispozici v seznamu vyvolání více než jednou.</span><span class="sxs-lookup"><span data-stu-id="c681d-181">As shown in the statement `cd3 += cd1;`, a delegate can be present in an invocation list multiple times.</span></span> <span data-ttu-id="c681d-182">V takovém případě stačí vyvolá se jednou za výskyt.</span><span class="sxs-lookup"><span data-stu-id="c681d-182">In this case, it is simply invoked once per occurrence.</span></span> <span data-ttu-id="c681d-183">V seznamu vyvolání takovou situaci při odebrání tohoto delegátu posledního výskytu v seznamu vyvolání je skutečně odebrat.</span><span class="sxs-lookup"><span data-stu-id="c681d-183">In an invocation list such as this, when that delegate is removed, the last occurrence in the invocation list is the one actually removed.</span></span>

<span data-ttu-id="c681d-184">Bezprostředně před provedením poslední příkaz `cd3 -= cd1;`, delegát `cd3` odkazuje na seznam prázdný volání.</span><span class="sxs-lookup"><span data-stu-id="c681d-184">Immediately prior to the execution of the final statement, `cd3 -= cd1;`, the delegate `cd3` refers to an empty invocation list.</span></span> <span data-ttu-id="c681d-185">Chcete-li odebrat delegáta z prázdného seznamu (nebo odebrat neexistující delegáta formu neprázdného seznamu), není to chyba.</span><span class="sxs-lookup"><span data-stu-id="c681d-185">Attempting to remove a delegate from an empty list (or to remove a non-existent delegate from a non-empty list) is not an error.</span></span>

<span data-ttu-id="c681d-186">Výstup vytvořený je:</span><span class="sxs-lookup"><span data-stu-id="c681d-186">The output produced is:</span></span>

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
