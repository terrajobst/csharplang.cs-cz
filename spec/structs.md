# <a name="structs"></a><span data-ttu-id="8fe13-101">Struktury</span><span class="sxs-lookup"><span data-stu-id="8fe13-101">Structs</span></span>

<span data-ttu-id="8fe13-102">Struktury jsou podobné třídy, které představují datové struktury, které mohou obsahovat datové členy a funkční členy.</span><span class="sxs-lookup"><span data-stu-id="8fe13-102">Structs are similar to classes in that they represent data structures that can contain data members and function members.</span></span> <span data-ttu-id="8fe13-103">Na rozdíl od tříd však struktury jsou typy hodnot a nevyžadují přidělení haldy.</span><span class="sxs-lookup"><span data-stu-id="8fe13-103">However, unlike classes, structs are value types and do not require heap allocation.</span></span> <span data-ttu-id="8fe13-104">Proměnné typu Struktura přímo obsahuje datové struktury, že proměnné typu třídy obsahuje odkaz na data, druhá možnost známé jako objekt.</span><span class="sxs-lookup"><span data-stu-id="8fe13-104">A variable of a struct type directly contains the data of the struct, whereas a variable of a class type contains a reference to the data, the latter known as an object.</span></span>

<span data-ttu-id="8fe13-105">Struktury jsou zvláště užitečná pro malé datové struktury, které mají hodnotu sémantiku.</span><span class="sxs-lookup"><span data-stu-id="8fe13-105">Structs are particularly useful for small data structures that have value semantics.</span></span> <span data-ttu-id="8fe13-106">Komplexní čísla, body v souřadnicovém systému nebo páry klíč hodnota do slovníku jsou všechny dobrým příkladem struktury.</span><span class="sxs-lookup"><span data-stu-id="8fe13-106">Complex numbers, points in a coordinate system, or key-value pairs in a dictionary are all good examples of structs.</span></span> <span data-ttu-id="8fe13-107">Je klíč pro tyto datové struktury, ke kterým mají několik datových členů, nevyžadují použití dědičnosti nebo referenční identity a že můžete pohodlně prováděny pomocí sémantiky hodnota, kde přiřazení kopíruje hodnotu namísto odkazu.</span><span class="sxs-lookup"><span data-stu-id="8fe13-107">Key to these data structures is that they have few data members, that they do not require use of inheritance or referential identity, and that they can be conveniently implemented using value semantics where assignment copies the value instead of the reference.</span></span>

<span data-ttu-id="8fe13-108">Jak je popsáno v [jednoduché typy](types.md#simple-types), jednoduché typy, které jsou k dispozici v jazyce C#, jako například `int`, `double`, a `bool`, jsou ve skutečnosti všechny typy struktury.</span><span class="sxs-lookup"><span data-stu-id="8fe13-108">As described in [Simple types](types.md#simple-types), the simple types provided by C#, such as `int`, `double`, and `bool`, are in fact all struct types.</span></span> <span data-ttu-id="8fe13-109">Stejně jako tyto předdefinované typy jsou struktury, je také možné použít struktury a přetěžování pro implementaci nový "základní" typy v jazyce C#.</span><span class="sxs-lookup"><span data-stu-id="8fe13-109">Just as these predefined types are structs, it is also possible to use structs and operator overloading to implement new "primitive" types in the C# language.</span></span> <span data-ttu-id="8fe13-110">Na konci této kapitole jsou uvedeny dva příklady těchto typů ([struktura příklady](structs.md#struct-examples)).</span><span class="sxs-lookup"><span data-stu-id="8fe13-110">Two examples of such types are given at the end of this chapter ([Struct examples](structs.md#struct-examples)).</span></span>

## <a name="struct-declarations"></a><span data-ttu-id="8fe13-111">Deklarace struktury</span><span class="sxs-lookup"><span data-stu-id="8fe13-111">Struct declarations</span></span>

<span data-ttu-id="8fe13-112">A *struct_declaration* je *type_declaration* ([typ deklarace](namespaces.md#type-declarations)), který deklaruje novou strukturu:</span><span class="sxs-lookup"><span data-stu-id="8fe13-112">A *struct_declaration* is a *type_declaration* ([Type declarations](namespaces.md#type-declarations)) that declares a new struct:</span></span>

```antlr
struct_declaration
    : attributes? struct_modifier* 'partial'? 'struct' identifier type_parameter_list?
      struct_interfaces? type_parameter_constraints_clause* struct_body ';'?
    ;
```

<span data-ttu-id="8fe13-113">A *struct_declaration* se skládá z volitelné sadu *atributy* ([atributy](attributes.md)) následovaný volitelná sada *struct_modifier*s ([struktura modifikátory](structs.md#struct-modifiers)), následovaným volitelnou `partial` modifikátor, za nímž následuje klíčové slovo `struct` a *identifikátor* , která pojmenuje struktury, za nímž následuje volitelné *type_parameter_list* specifikace ([parametry typu](classes.md#type-parameters)), následovaným volitelnou *struct_interfaces* specifikace ([Částečný modifikátor](structs.md#partial-modifier))), následovaným volitelnou *type_parameter_constraints_clause*s specifikace ([omezení parametru typu](classes.md#type-parameter-constraints)) a po něm *struct_body* ([struktury textu](structs.md#struct-body)), volitelně za nímž následuje středníkem.</span><span class="sxs-lookup"><span data-stu-id="8fe13-113">A *struct_declaration* consists of an optional set of *attributes* ([Attributes](attributes.md)), followed by an optional set of *struct_modifier*s ([Struct modifiers](structs.md#struct-modifiers)), followed by an optional `partial` modifier, followed by the keyword `struct` and an *identifier* that names the struct, followed by an optional *type_parameter_list* specification ([Type parameters](classes.md#type-parameters)), followed by an optional *struct_interfaces* specification ([Partial modifier](structs.md#partial-modifier)) ), followed by an optional *type_parameter_constraints_clause*s specification ([Type parameter constraints](classes.md#type-parameter-constraints)), followed by a *struct_body* ([Struct body](structs.md#struct-body)), optionally followed by a semicolon.</span></span>

### <a name="struct-modifiers"></a><span data-ttu-id="8fe13-114">Modifikátory – struktura</span><span class="sxs-lookup"><span data-stu-id="8fe13-114">Struct modifiers</span></span>

<span data-ttu-id="8fe13-115">A *struct_declaration* může volitelně zahrnovat posloupnost modifikátory struktury:</span><span class="sxs-lookup"><span data-stu-id="8fe13-115">A *struct_declaration* may optionally include a sequence of struct modifiers:</span></span>

```antlr
struct_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    | struct_modifier_unsafe
    ;
```

<span data-ttu-id="8fe13-116">Je chyba kompilace pro stejný modifikátor objevit více než jednou v deklaraci struktury.</span><span class="sxs-lookup"><span data-stu-id="8fe13-116">It is a compile-time error for the same modifier to appear multiple times in a struct declaration.</span></span>

<span data-ttu-id="8fe13-117">Modifikátory deklarace struktury mají stejný význam jako deklarace třídy ([třídy deklarací](classes.md#class-declarations)).</span><span class="sxs-lookup"><span data-stu-id="8fe13-117">The modifiers of a struct declaration have the same meaning as those of a class declaration ([Class declarations](classes.md#class-declarations)).</span></span>

### <a name="partial-modifier"></a><span data-ttu-id="8fe13-118">Částečný modifikátor</span><span class="sxs-lookup"><span data-stu-id="8fe13-118">Partial modifier</span></span>

<span data-ttu-id="8fe13-119">`partial` Modifikátor znamená, že to *struct_declaration* je částečný typ deklarace.</span><span class="sxs-lookup"><span data-stu-id="8fe13-119">The `partial` modifier indicates that this *struct_declaration* is a partial type declaration.</span></span> <span data-ttu-id="8fe13-120">Více deklaracích částečné struktury se stejným názvem v rámci nadřazeného oboru názvů nebo typ deklarace se dá tvoří jednu deklaraci struktury, dle pravidel uvedených v [částečné typy](classes.md#partial-types).</span><span class="sxs-lookup"><span data-stu-id="8fe13-120">Multiple partial struct declarations with the same name within an enclosing namespace or type declaration combine to form one struct declaration, following the rules specified in [Partial types](classes.md#partial-types).</span></span>

### <a name="struct-interfaces"></a><span data-ttu-id="8fe13-121">Struktura rozhraní</span><span class="sxs-lookup"><span data-stu-id="8fe13-121">Struct interfaces</span></span>

<span data-ttu-id="8fe13-122">Může obsahovat deklaraci struktury *struct_interfaces* specifikace, ve kterém případ struct se říká, že přímo implementaci typů dané rozhraní.</span><span class="sxs-lookup"><span data-stu-id="8fe13-122">A struct declaration may include a *struct_interfaces* specification, in which case the struct is said to directly implement the given interface types.</span></span>

```antlr
struct_interfaces
    : ':' interface_type_list
    ;
```

<span data-ttu-id="8fe13-123">Implementace rozhraní jsou popsány dále v [rozhraní implementace](interfaces.md#interface-implementations).</span><span class="sxs-lookup"><span data-stu-id="8fe13-123">Interface implementations are discussed further in [Interface implementations](interfaces.md#interface-implementations).</span></span>

### <a name="struct-body"></a><span data-ttu-id="8fe13-124">Text – struktura</span><span class="sxs-lookup"><span data-stu-id="8fe13-124">Struct body</span></span>

<span data-ttu-id="8fe13-125">*Struct_body* struktury definuje členy struktury.</span><span class="sxs-lookup"><span data-stu-id="8fe13-125">The *struct_body* of a struct defines the members of the struct.</span></span>

```antlr
struct_body
    : '{' struct_member_declaration* '}'
    ;
```

## <a name="struct-members"></a><span data-ttu-id="8fe13-126">Členy struktury</span><span class="sxs-lookup"><span data-stu-id="8fe13-126">Struct members</span></span>

<span data-ttu-id="8fe13-127">Členové struktury obsahovat členy zavedených v jeho *struct_member_declaration*s a členy zděděné z typu `System.ValueType`.</span><span class="sxs-lookup"><span data-stu-id="8fe13-127">The members of a struct consist of the members introduced by its *struct_member_declaration*s and the members inherited from the type `System.ValueType`.</span></span>

```antlr
struct_member_declaration
    : constant_declaration
    | field_declaration
    | method_declaration
    | property_declaration
    | event_declaration
    | indexer_declaration
    | operator_declaration
    | constructor_declaration
    | static_constructor_declaration
    | type_declaration
    | struct_member_declaration_unsafe
    ;
```

<span data-ttu-id="8fe13-128">S výjimkou rozdílů, které jste si poznamenali v [třídou a strukturou rozdíly](structs.md#class-and-struct-differences), popisy členy třídy, které jsou součástí [členy třídy](classes.md#class-members) prostřednictvím [iterátory](classes.md#iterators) použít – struktura také členy.</span><span class="sxs-lookup"><span data-stu-id="8fe13-128">Except for the differences noted in [Class and struct differences](structs.md#class-and-struct-differences), the descriptions of class members provided in [Class members](classes.md#class-members) through [Iterators](classes.md#iterators) apply to struct members as well.</span></span>

## <a name="class-and-struct-differences"></a><span data-ttu-id="8fe13-129">Třídy a struktury rozdíly</span><span class="sxs-lookup"><span data-stu-id="8fe13-129">Class and struct differences</span></span>

<span data-ttu-id="8fe13-130">Struktury se liší od tříd v několika důležitých směrech –:</span><span class="sxs-lookup"><span data-stu-id="8fe13-130">Structs differ from classes in several important ways:</span></span>

*  <span data-ttu-id="8fe13-131">Struktury jsou typy hodnot ([hodnota sémantiku](structs.md#value-semantics)).</span><span class="sxs-lookup"><span data-stu-id="8fe13-131">Structs are value types ([Value semantics](structs.md#value-semantics)).</span></span>
*  <span data-ttu-id="8fe13-132">Všechny typy struktury implicitně dědí z třídy `System.ValueType` ([dědičnosti](structs.md#inheritance)).</span><span class="sxs-lookup"><span data-stu-id="8fe13-132">All struct types implicitly inherit from the class `System.ValueType` ([Inheritance](structs.md#inheritance)).</span></span>
*  <span data-ttu-id="8fe13-133">Přiřazení proměnné typu Struktura se vytvoří kopie přiřazené hodnoty ([přiřazení](structs.md#assignment)).</span><span class="sxs-lookup"><span data-stu-id="8fe13-133">Assignment to a variable of a struct type creates a copy of the value being assigned ([Assignment](structs.md#assignment)).</span></span>
*  <span data-ttu-id="8fe13-134">Výchozí hodnota struktury je hodnotu vytvořenou testovaným nastavení všechna pole typu hodnota na výchozí hodnoty a referenční dokumentace všech polí typu `null` ([výchozí hodnoty](structs.md#default-values)).</span><span class="sxs-lookup"><span data-stu-id="8fe13-134">The default value of a struct is the value produced by setting all value type fields to their default value and all reference type fields to `null` ([Default values](structs.md#default-values)).</span></span>
*  <span data-ttu-id="8fe13-135">Operace zabalení a rozbalení se používají pro převod mezi typy struktury a `object` ([zabalení a rozbalení](structs.md#boxing-and-unboxing)).</span><span class="sxs-lookup"><span data-stu-id="8fe13-135">Boxing and unboxing operations are used to convert between a struct type and `object` ([Boxing and unboxing](structs.md#boxing-and-unboxing)).</span></span>
*  <span data-ttu-id="8fe13-136">Význam `this` se liší pro struktury ([tento přístup](expressions.md#this-access)).</span><span class="sxs-lookup"><span data-stu-id="8fe13-136">The meaning of `this` is different for structs ([This access](expressions.md#this-access)).</span></span>
*  <span data-ttu-id="8fe13-137">Zahrnout proměnné inicializátory nejsou povolené instance pole deklarace pro strukturu ([Inicializátory pole](structs.md#field-initializers)).</span><span class="sxs-lookup"><span data-stu-id="8fe13-137">Instance field declarations for a struct are not permitted to include variable initializers ([Field initializers](structs.md#field-initializers)).</span></span>
*  <span data-ttu-id="8fe13-138">Struktura není povolená pro deklaraci konstruktor instance bez parametrů ([konstruktory](structs.md#constructors)).</span><span class="sxs-lookup"><span data-stu-id="8fe13-138">A struct is not permitted to declare a parameterless instance constructor ([Constructors](structs.md#constructors)).</span></span>
*  <span data-ttu-id="8fe13-139">Chcete-li deklarovat destruktor není povolená struktury ([destruktory](structs.md#destructors)).</span><span class="sxs-lookup"><span data-stu-id="8fe13-139">A struct is not permitted to declare a destructor ([Destructors](structs.md#destructors)).</span></span>

### <a name="value-semantics"></a><span data-ttu-id="8fe13-140">Sémantika hodnoty</span><span class="sxs-lookup"><span data-stu-id="8fe13-140">Value semantics</span></span>

<span data-ttu-id="8fe13-141">Struktury jsou typy hodnot ([typů hodnot](types.md#value-types)) a se říká, že mají hodnotu sémantiku.</span><span class="sxs-lookup"><span data-stu-id="8fe13-141">Structs are value types ([Value types](types.md#value-types)) and are said to have value semantics.</span></span> <span data-ttu-id="8fe13-142">Třídy, na druhé straně jsou odkazové typy ([referenční typy](types.md#reference-types)) a mají často odkazové sémantiky.</span><span class="sxs-lookup"><span data-stu-id="8fe13-142">Classes, on the other hand, are reference types ([Reference types](types.md#reference-types)) and are said to have reference semantics.</span></span>

<span data-ttu-id="8fe13-143">Proměnné typu Struktura přímo obsahuje datové struktury, že proměnné typu třídy obsahuje odkaz na data, druhá možnost známé jako objekt.</span><span class="sxs-lookup"><span data-stu-id="8fe13-143">A variable of a struct type directly contains the data of the struct, whereas a variable of a class type contains a reference to the data, the latter known as an object.</span></span> <span data-ttu-id="8fe13-144">Pokud struktura `B` obsahuje pole instance typu `A` a `A` je typ struktury je chyba kompilace pro `A` závisí na `B` nebo typ vytvořený z `B`.</span><span class="sxs-lookup"><span data-stu-id="8fe13-144">When a struct `B` contains an instance field of type `A` and `A` is a struct type, it is a compile-time error for `A` to depend on `B` or a type constructed from `B`.</span></span> <span data-ttu-id="8fe13-145">Struktura `X` ***přímo závisí na*** struktura `Y` Pokud `X` obsahuje pole instance typu `Y`.</span><span class="sxs-lookup"><span data-stu-id="8fe13-145">A struct `X` ***directly depends on*** a struct `Y` if `X` contains an instance field of type `Y`.</span></span> <span data-ttu-id="8fe13-146">Při této definici, kompletní sadu struktury, na kterém závisí struktura je přenositelný uzavření ***přímo závisí na*** vztah.</span><span class="sxs-lookup"><span data-stu-id="8fe13-146">Given this definition, the complete set of structs upon which a struct depends is the transitive closure of the ***directly depends on*** relationship.</span></span>  <span data-ttu-id="8fe13-147">Příklad</span><span class="sxs-lookup"><span data-stu-id="8fe13-147">For example</span></span>
```csharp
struct Node
{
    int data;
    Node next; // error, Node directly depends on itself
}
```
<span data-ttu-id="8fe13-148">se o chybu, protože `Node` obsahuje pole instance vlastního typu.</span><span class="sxs-lookup"><span data-stu-id="8fe13-148">is an error because `Node` contains an instance field of its own type.</span></span>  <span data-ttu-id="8fe13-149">Další příklad</span><span class="sxs-lookup"><span data-stu-id="8fe13-149">Another example</span></span>
```csharp
struct A { B b; }

struct B { C c; }

struct C { A a; }
```
<span data-ttu-id="8fe13-150">se o chybu, protože jednotlivé typy `A`, `B`, a `C` závisí na sebe navzájem.</span><span class="sxs-lookup"><span data-stu-id="8fe13-150">is an error because each of the types `A`, `B`, and `C` depend on each other.</span></span>

<span data-ttu-id="8fe13-151">Pomocí třídy je možné pro dvě proměnné odkazovat na stejný objekt a proto možná pro operace v rámci jedné proměnné ovlivňovat objekt odkazovaný jinou proměnnou.</span><span class="sxs-lookup"><span data-stu-id="8fe13-151">With classes, it is possible for two variables to reference the same object, and thus possible for operations on one variable to affect the object referenced by the other variable.</span></span> <span data-ttu-id="8fe13-152">Struktury, proměnné každý mají své vlastní kopii dat (s výjimkou v případě třídy `ref` a `out` proměnných parametrů), a není možné pro operace se na nich se má vliv na jinou.</span><span class="sxs-lookup"><span data-stu-id="8fe13-152">With structs, the variables each have their own copy of the data (except in the case of `ref` and `out` parameter variables), and it is not possible for operations on one to affect the other.</span></span> <span data-ttu-id="8fe13-153">Navíc vzhledem k tomu struktury nejsou typy odkazů, není možné pro hodnoty typu struktura bude `null`.</span><span class="sxs-lookup"><span data-stu-id="8fe13-153">Furthermore, because structs are not reference types, it is not possible for values of a struct type to be `null`.</span></span>

<span data-ttu-id="8fe13-154">Zadané deklarace</span><span class="sxs-lookup"><span data-stu-id="8fe13-154">Given the declaration</span></span>
```csharp
struct Point
{
    public int x, y;

    public Point(int x, int y) {
        this.x = x;
        this.y = y;
    }
}
```
<span data-ttu-id="8fe13-155">fragment kódu</span><span class="sxs-lookup"><span data-stu-id="8fe13-155">the code fragment</span></span>
```csharp
Point a = new Point(10, 10);
Point b = a;
a.x = 100;
System.Console.WriteLine(b.x);
```
<span data-ttu-id="8fe13-156">Vypíše hodnotu `10`.</span><span class="sxs-lookup"><span data-stu-id="8fe13-156">outputs the value `10`.</span></span> <span data-ttu-id="8fe13-157">Přiřazení `a` k `b` vytvoří kopii hodnoty, a `b` je tedy nebudou výpadkem ovlivněny přiřazení `a.x`.</span><span class="sxs-lookup"><span data-stu-id="8fe13-157">The assignment of `a` to `b` creates a copy of the value, and `b` is thus unaffected by the assignment to `a.x`.</span></span> <span data-ttu-id="8fe13-158">Měl `Point` místo toho byla deklarována jako třída, výstup by měl `100` protože `a` a `b` by odkazovat na stejný objekt.</span><span class="sxs-lookup"><span data-stu-id="8fe13-158">Had `Point` instead been declared as a class, the output would be `100` because `a` and `b` would reference the same object.</span></span>

### <a name="inheritance"></a><span data-ttu-id="8fe13-159">Dědičnost</span><span class="sxs-lookup"><span data-stu-id="8fe13-159">Inheritance</span></span>

<span data-ttu-id="8fe13-160">Všechny typy struktury implicitně dědí z třídy `System.ValueType`, který zase dědí z třídy `object`.</span><span class="sxs-lookup"><span data-stu-id="8fe13-160">All struct types implicitly inherit from the class `System.ValueType`, which, in turn, inherits from class `object`.</span></span> <span data-ttu-id="8fe13-161">Deklarace struktury mohou zadat seznamu implementovaných rozhraní, ale není možné určit základní třídu pro deklaraci struktury.</span><span class="sxs-lookup"><span data-stu-id="8fe13-161">A struct declaration may specify a list of implemented interfaces, but it is not possible for a struct declaration to specify a base class.</span></span>

<span data-ttu-id="8fe13-162">Typy struktury jsou nikdy abstraktní a jsou implicitně jsou vždycky zapečetěné.</span><span class="sxs-lookup"><span data-stu-id="8fe13-162">Struct types are never abstract and are always implicitly sealed.</span></span> <span data-ttu-id="8fe13-163">`abstract` a `sealed` proto nejsou povolené modifikátory. v deklaraci struktury.</span><span class="sxs-lookup"><span data-stu-id="8fe13-163">The `abstract` and `sealed` modifiers are therefore not permitted in a struct declaration.</span></span>

<span data-ttu-id="8fe13-164">Protože dědičnost se nepodporuje pro struktury, nemůže být deklarovaná přístupnost člena struktury `protected` nebo `protected internal`.</span><span class="sxs-lookup"><span data-stu-id="8fe13-164">Since inheritance isn't supported for structs, the declared accessibility of a struct member cannot be `protected` or `protected internal`.</span></span>

<span data-ttu-id="8fe13-165">Funkce členy struktury nemůžou být `abstract` nebo `virtual`a `override` Modifikátor je povolen pouze k přepsání metod zděděných z `System.ValueType`.</span><span class="sxs-lookup"><span data-stu-id="8fe13-165">Function members in a struct cannot be `abstract` or `virtual`, and the `override` modifier is allowed only to override methods inherited from `System.ValueType`.</span></span>

### <a name="assignment"></a><span data-ttu-id="8fe13-166">Přiřazení</span><span class="sxs-lookup"><span data-stu-id="8fe13-166">Assignment</span></span>

<span data-ttu-id="8fe13-167">Přiřazení proměnné typu Struktura vytvoří kopii přiřazené hodnoty.</span><span class="sxs-lookup"><span data-stu-id="8fe13-167">Assignment to a variable of a struct type creates a copy of the value being assigned.</span></span> <span data-ttu-id="8fe13-168">Tím se liší od přiřazení k proměnné typu třídy, která kopíruje odkaz, ale ne identifikovaný odkaz na objekt.</span><span class="sxs-lookup"><span data-stu-id="8fe13-168">This differs from assignment to a variable of a class type, which copies the reference but not the object identified by the reference.</span></span>

<span data-ttu-id="8fe13-169">Podobně jako přiřazení, pokud struktury je předán jako parametr hodnoty nebo vrátí jako výsledek členské funkce, je vytvořena kopie struktury.</span><span class="sxs-lookup"><span data-stu-id="8fe13-169">Similar to an assignment, when a struct is passed as a value parameter or returned as the result of a function member, a copy of the struct is created.</span></span> <span data-ttu-id="8fe13-170">Struktura může být předány podle odkazu na člen funkce pomocí `ref` nebo `out` parametru.</span><span class="sxs-lookup"><span data-stu-id="8fe13-170">A struct may be passed by reference to a function member using a `ref` or `out` parameter.</span></span>

<span data-ttu-id="8fe13-171">Pokud vlastnost nebo indexovací člen struktury je cílem přiřazení, musí být výraz instance spojené s přístupem k vlastnosti nebo indexeru zařazeny jako proměnnou.</span><span class="sxs-lookup"><span data-stu-id="8fe13-171">When a property or indexer of a struct is the target of an assignment, the instance expression associated with the property or indexer access must be classified as a variable.</span></span> <span data-ttu-id="8fe13-172">Pokud výraz instance je klasifikován tak hodnotu, dojde k chybě v době kompilace.</span><span class="sxs-lookup"><span data-stu-id="8fe13-172">If the instance expression is classified as a value, a compile-time error occurs.</span></span> <span data-ttu-id="8fe13-173">To je podrobně popsán dále v [jednoduché přiřazení](expressions.md#simple-assignment).</span><span class="sxs-lookup"><span data-stu-id="8fe13-173">This is described in further detail in [Simple assignment](expressions.md#simple-assignment).</span></span>

### <a name="default-values"></a><span data-ttu-id="8fe13-174">Výchozí hodnoty</span><span class="sxs-lookup"><span data-stu-id="8fe13-174">Default values</span></span>

<span data-ttu-id="8fe13-175">Jak je popsáno v [výchozí hodnoty](variables.md#default-values), několik druhů proměnné jsou automaticky inicializovány na jejich výchozí hodnota při jejich vytváření.</span><span class="sxs-lookup"><span data-stu-id="8fe13-175">As described in [Default values](variables.md#default-values), several kinds of variables are automatically initialized to their default value when they are created.</span></span> <span data-ttu-id="8fe13-176">Pro proměnné typů třídy a další typy odkazů, tato výchozí hodnota je `null`.</span><span class="sxs-lookup"><span data-stu-id="8fe13-176">For variables of class types and other reference types, this default value is `null`.</span></span> <span data-ttu-id="8fe13-177">Ale protože struktury jsou typy hodnot, které nelze `null`, výchozí hodnota struktury je hodnotu vytvořenou testovaným nastavení všechna pole typu hodnota na výchozí hodnoty a referenční dokumentace všech polí typu `null`.</span><span class="sxs-lookup"><span data-stu-id="8fe13-177">However, since structs are value types that cannot be `null`, the default value of a struct is the value produced by setting all value type fields to their default value and all reference type fields to `null`.</span></span>

<span data-ttu-id="8fe13-178">Odkazující na `Point` struktury deklarovaného výše v příkladu</span><span class="sxs-lookup"><span data-stu-id="8fe13-178">Referring to the `Point` struct declared above, the example</span></span>
```csharp
Point[] a = new Point[100];
```
<span data-ttu-id="8fe13-179">Inicializuje každý `Point` jako pole k hodnotě vytvořený tak, že nastavíte `x` a `y` pole na hodnotu nula.</span><span class="sxs-lookup"><span data-stu-id="8fe13-179">initializes each `Point` in the array to the value produced by setting the `x` and `y` fields to zero.</span></span>

<span data-ttu-id="8fe13-180">Výchozí hodnota struktury odpovídá hodnotě vrácené výchozí konstruktor třídy struktury ([výchozí konstruktory](types.md#default-constructors)).</span><span class="sxs-lookup"><span data-stu-id="8fe13-180">The default value of a struct corresponds to the value returned by the default constructor of the struct ([Default constructors](types.md#default-constructors)).</span></span> <span data-ttu-id="8fe13-181">Na rozdíl od třídy struktury není oprávněn deklarovat konstruktor instance bez parametrů.</span><span class="sxs-lookup"><span data-stu-id="8fe13-181">Unlike a class, a struct is not permitted to declare a parameterless instance constructor.</span></span> <span data-ttu-id="8fe13-182">Místo toho každých struktura má implicitně konstruktor instance bez parametrů, která vždy vrátí hodnotu, která je výsledkem nastavení všechna pole typu hodnota na výchozí hodnoty a referenční dokumentace všech polí typu `null`.</span><span class="sxs-lookup"><span data-stu-id="8fe13-182">Instead, every struct implicitly has a parameterless instance constructor which always returns the value that results from setting all value type fields to their default value and all reference type fields to `null`.</span></span>

<span data-ttu-id="8fe13-183">Struktury by se měly navrhovat vzít v úvahu výchozího stavu inicializace platném stavu.</span><span class="sxs-lookup"><span data-stu-id="8fe13-183">Structs should be designed to consider the default initialization state a valid state.</span></span> <span data-ttu-id="8fe13-184">V příkladu</span><span class="sxs-lookup"><span data-stu-id="8fe13-184">In the example</span></span>
```csharp
using System;

struct KeyValuePair
{
    string key;
    string value;

    public KeyValuePair(string key, string value) {
        if (key == null || value == null) throw new ArgumentException();
        this.key = key;
        this.value = value;
    }
}
```
<span data-ttu-id="8fe13-185">uživatelské instance konstruktoru chrání proti hodnoty null, pouze pokud je explicitně volána.</span><span class="sxs-lookup"><span data-stu-id="8fe13-185">the user-defined instance constructor protects against null values only where it is explicitly called.</span></span> <span data-ttu-id="8fe13-186">V případech, kde `KeyValuePair` proměnná je v souladu s výchozí hodnotou inicializace `key` a `value` pole bude mít hodnotu null a struct musí být připravena ke zpracování tohoto stavu.</span><span class="sxs-lookup"><span data-stu-id="8fe13-186">In cases where a `KeyValuePair` variable is subject to default value initialization, the `key` and `value` fields will be null, and the struct must be prepared to handle this state.</span></span>

### <a name="boxing-and-unboxing"></a><span data-ttu-id="8fe13-187">Zabalení a rozbalení</span><span class="sxs-lookup"><span data-stu-id="8fe13-187">Boxing and unboxing</span></span>

<span data-ttu-id="8fe13-188">Hodnotu typu třídy lze převést na typ `object` nebo k typu rozhraní, která je implementována ve třídě jednoduše díky tomu, že odkaz na jiný typ v době kompilace.</span><span class="sxs-lookup"><span data-stu-id="8fe13-188">A value of a class type can be converted to type `object` or to an interface type that is implemented by the class simply by treating the reference as another type at compile-time.</span></span> <span data-ttu-id="8fe13-189">Obdobně hodnotu typu `object` nebo hodnotu rozhraní typu lze převést zpět na typ třídy beze změny odkaz (ale samozřejmě typu modulu runtime je vyžadována kontrola v tomto případě).</span><span class="sxs-lookup"><span data-stu-id="8fe13-189">Likewise, a value of type `object` or a value of an interface type can be converted back to a class type without changing the reference (but of course a run-time type check is required in this case).</span></span>

<span data-ttu-id="8fe13-190">Protože struktury nejsou typy odkazů, jsou tyto operace pro typy struktury implementováno jinak.</span><span class="sxs-lookup"><span data-stu-id="8fe13-190">Since structs are not reference types, these operations are implemented differently for struct types.</span></span> <span data-ttu-id="8fe13-191">Pokud je hodnota typu Struktura převést na typ `object` nebo na typ rozhraní implementovaný struktury zabalení operace probíhá.</span><span class="sxs-lookup"><span data-stu-id="8fe13-191">When a value of a struct type is converted to type `object` or to an interface type that is implemented by the struct, a boxing operation takes place.</span></span> <span data-ttu-id="8fe13-192">Podobně když je hodnota typu `object` nebo hodnotu typu rozhraní je převést zpět na typu Struktura, probíhá operace rozbalení.</span><span class="sxs-lookup"><span data-stu-id="8fe13-192">Likewise, when a value of type `object` or a value of an interface type is converted back to a struct type, an unboxing operation takes place.</span></span> <span data-ttu-id="8fe13-193">Klíčovým rozdílem mezi ze stejné operace na typy tříd je, že zabalení a rozbalení zkopírujete příslušnou hodnotu struktury do nebo z pevně určené instance.</span><span class="sxs-lookup"><span data-stu-id="8fe13-193">A key difference from the same operations on class types is that boxing and unboxing copies the struct value either into or out of the boxed instance.</span></span> <span data-ttu-id="8fe13-194">Proto po operaci zabalení a rozbalení provedené změny nezabalené struktury se neprojeví v zabalený struktury.</span><span class="sxs-lookup"><span data-stu-id="8fe13-194">Thus, following a boxing or unboxing operation, changes made to the unboxed struct are not reflected in the boxed struct.</span></span>

<span data-ttu-id="8fe13-195">Při přepsání typu Struktura virtuální metody zděděné z `System.Object` (například `Equals`, `GetHashCode`, nebo `ToString`), volání virtuální metody prostřednictvím instance typu Struktura nezpůsobí zabalení dochází.</span><span class="sxs-lookup"><span data-stu-id="8fe13-195">When a struct type overrides a virtual method inherited from `System.Object` (such as `Equals`, `GetHashCode`, or `ToString`), invocation of the virtual method through an instance of the struct type does not cause boxing to occur.</span></span> <span data-ttu-id="8fe13-196">To platí i v případě, že struktura slouží jako parametr typu a dojde k vyvolání prostřednictvím instance typu parametru typu.</span><span class="sxs-lookup"><span data-stu-id="8fe13-196">This is true even when the struct is used as a type parameter and the invocation occurs through an instance of the type parameter type.</span></span> <span data-ttu-id="8fe13-197">Příklad:</span><span class="sxs-lookup"><span data-stu-id="8fe13-197">For example:</span></span>
```csharp
using System;

struct Counter
{
    int value;

    public override string ToString() {
        value++;
        return value.ToString();
    }
}

class Program
{
    static void Test<T>() where T: new() {
        T x = new T();
        Console.WriteLine(x.ToString());
        Console.WriteLine(x.ToString());
        Console.WriteLine(x.ToString());
    }

    static void Main() {
        Test<Counter>();
    }
}
```

<span data-ttu-id="8fe13-198">Výstup programu je:</span><span class="sxs-lookup"><span data-stu-id="8fe13-198">The output of the program is:</span></span>
```
1
2
3
```

<span data-ttu-id="8fe13-199">I když je špatný styl `ToString` mít vedlejší účinky, příklad ukazuje, že došlo k žádné zabalení pro tři volání `x.ToString()`.</span><span class="sxs-lookup"><span data-stu-id="8fe13-199">Although it is bad style for `ToString` to have side effects, the example demonstrates that no boxing occurred for the three invocations of `x.ToString()`.</span></span>

<span data-ttu-id="8fe13-200">Podobně nikdy implicitně zabalení dochází při přístupu k členovi na parametr typu s omezením.</span><span class="sxs-lookup"><span data-stu-id="8fe13-200">Similarly, boxing never implicitly occurs when accessing a member on a constrained type parameter.</span></span> <span data-ttu-id="8fe13-201">Předpokládejme například, že rozhraní `ICounter` obsahuje metodu `Increment` který můžete použít třeba hodnotu změnit.</span><span class="sxs-lookup"><span data-stu-id="8fe13-201">For example, suppose an interface `ICounter` contains a method `Increment` which can be used to modify a value.</span></span> <span data-ttu-id="8fe13-202">Pokud `ICounter` je použitý jako omezení, provádění `Increment` metoda je volána s odkazem na proměnnou, která `Increment` byla volána pro nikdy zabalený kopírování.</span><span class="sxs-lookup"><span data-stu-id="8fe13-202">If `ICounter` is used as a constraint, the implementation of the `Increment` method is called with a reference to the variable that `Increment` was called on, never a boxed copy.</span></span>

```csharp
using System;

interface ICounter
{
    void Increment();
}

struct Counter: ICounter
{
    int value;

    public override string ToString() {
        return value.ToString();
    }

    void ICounter.Increment() {
        value++;
    }
}

class Program
{
    static void Test<T>() where T: ICounter, new() {
        T x = new T();
        Console.WriteLine(x);
        x.Increment();                    // Modify x
        Console.WriteLine(x);
        ((ICounter)x).Increment();        // Modify boxed copy of x
        Console.WriteLine(x);
    }

    static void Main() {
        Test<Counter>();
    }
}
```

<span data-ttu-id="8fe13-203">První volání `Increment` změní hodnoty v proměnné `x`.</span><span class="sxs-lookup"><span data-stu-id="8fe13-203">The first call to `Increment` modifies the value in the variable `x`.</span></span> <span data-ttu-id="8fe13-204">Toto není ekvivalentní k druhé volání `Increment`, které mění hodnotu v zabalený kopie `x`.</span><span class="sxs-lookup"><span data-stu-id="8fe13-204">This is not equivalent to the second call to `Increment`, which modifies the value in a boxed copy of `x`.</span></span> <span data-ttu-id="8fe13-205">Proto je výstup programu:</span><span class="sxs-lookup"><span data-stu-id="8fe13-205">Thus, the output of the program is:</span></span>
```
0
1
1
```

<span data-ttu-id="8fe13-206">Další podrobnosti o zabalení a rozbalení najdete [zabalení a rozbalení](types.md#boxing-and-unboxing).</span><span class="sxs-lookup"><span data-stu-id="8fe13-206">For further details on boxing and unboxing, see [Boxing and unboxing](types.md#boxing-and-unboxing).</span></span>

### <a name="meaning-of-this"></a><span data-ttu-id="8fe13-207">Význam tohoto objektu</span><span class="sxs-lookup"><span data-stu-id="8fe13-207">Meaning of this</span></span>

<span data-ttu-id="8fe13-208">V rámci konstruktor instance nebo instance funkce člena třídy `this` klasifikovaný jako hodnotu.</span><span class="sxs-lookup"><span data-stu-id="8fe13-208">Within an instance constructor or instance function member of a class, `this` is classified as a value.</span></span> <span data-ttu-id="8fe13-209">Proto při `this` slouží k odkazování na instanci pro který byla vyvolána členské funkce, není možné přiřadit `this` v členské funkce třídy.</span><span class="sxs-lookup"><span data-stu-id="8fe13-209">Thus, while `this` can be used to refer to the instance for which the function member was invoked, it is not possible to assign to `this` in a function member of a class.</span></span>

<span data-ttu-id="8fe13-210">V rámci konstruktoru instance struktury `this` odpovídá `out` parametr typu Struktura a v rámci funkce členem instance struktury, `this` odpovídá `ref` parametr typu Struktura.</span><span class="sxs-lookup"><span data-stu-id="8fe13-210">Within an instance constructor of a struct, `this` corresponds to an `out` parameter of the struct type, and within an instance function member of a struct, `this` corresponds to a `ref` parameter of the struct type.</span></span> <span data-ttu-id="8fe13-211">V obou případech `this` je klasifikován jako proměnnou, a je možné změnit celé struktury, pro kterou členské funkce se vyvolala přiřazením `this` nebo předáním jako `ref` nebo `out` parametru.</span><span class="sxs-lookup"><span data-stu-id="8fe13-211">In both cases, `this` is classified as a variable, and it is possible to modify the entire struct for which the function member was invoked by assigning to `this` or by passing this as a `ref` or `out` parameter.</span></span>

### <a name="field-initializers"></a><span data-ttu-id="8fe13-212">Inicializátory pole</span><span class="sxs-lookup"><span data-stu-id="8fe13-212">Field initializers</span></span>

<span data-ttu-id="8fe13-213">Jak je popsáno v [výchozí hodnoty](structs.md#default-values), výchozí hodnota struktury se skládá z hodnotu, která je výsledkem nastavení všechna pole typu hodnota na výchozí hodnoty a referenční dokumentace všech polí typu `null`.</span><span class="sxs-lookup"><span data-stu-id="8fe13-213">As described in [Default values](structs.md#default-values), the default value of a struct consists of the value that results from setting all value type fields to their default value and all reference type fields to `null`.</span></span> <span data-ttu-id="8fe13-214">Z tohoto důvodu struktura neumožňuje deklarace pole instance na obsahovat inicializátory proměnné.</span><span class="sxs-lookup"><span data-stu-id="8fe13-214">For this reason, a struct does not permit instance field declarations to include variable initializers.</span></span> <span data-ttu-id="8fe13-215">Toto omezení platí pouze pro pole instance.</span><span class="sxs-lookup"><span data-stu-id="8fe13-215">This restriction applies only to instance fields.</span></span> <span data-ttu-id="8fe13-216">Statická pole struktury mohou obsahovat inicializátory proměnné.</span><span class="sxs-lookup"><span data-stu-id="8fe13-216">Static fields of a struct are permitted to include variable initializers.</span></span>

<span data-ttu-id="8fe13-217">V příkladu</span><span class="sxs-lookup"><span data-stu-id="8fe13-217">The example</span></span>
```csharp
struct Point
{
    public int x = 1;  // Error, initializer not permitted
    public int y = 1;  // Error, initializer not permitted
}
```
<span data-ttu-id="8fe13-218">je v chybě, protože deklarace pole instance obsahovat inicializátory proměnné.</span><span class="sxs-lookup"><span data-stu-id="8fe13-218">is in error because the instance field declarations include variable initializers.</span></span>

### <a name="constructors"></a><span data-ttu-id="8fe13-219">Konstruktory</span><span class="sxs-lookup"><span data-stu-id="8fe13-219">Constructors</span></span>

<span data-ttu-id="8fe13-220">Na rozdíl od třídy struktury není oprávněn deklarovat konstruktor instance bez parametrů.</span><span class="sxs-lookup"><span data-stu-id="8fe13-220">Unlike a class, a struct is not permitted to declare a parameterless instance constructor.</span></span> <span data-ttu-id="8fe13-221">Místo toho každých struktury implicitně obsahuje konstruktor instance bez parametrů, která vždy vrátí hodnotu, která je výsledkem nastavení všechna pole na výchozí hodnoty a referenční dokumentace všech typ pole na hodnotu null ([výchozí konstruktory](types.md#default-constructors)).</span><span class="sxs-lookup"><span data-stu-id="8fe13-221">Instead, every struct implicitly has a parameterless instance constructor which always returns the value that results from setting all value type fields to their default value and all reference type fields to null ([Default constructors](types.md#default-constructors)).</span></span> <span data-ttu-id="8fe13-222">Struktury můžete deklarovat konstruktory instancí s parametry.</span><span class="sxs-lookup"><span data-stu-id="8fe13-222">A struct can declare instance constructors having parameters.</span></span> <span data-ttu-id="8fe13-223">Příklad</span><span class="sxs-lookup"><span data-stu-id="8fe13-223">For example</span></span>
```csharp
struct Point
{
    int x, y;

    public Point(int x, int y) {
        this.x = x;
        this.y = y;
    }
}
```

<span data-ttu-id="8fe13-224">Uvedené výše uvedené prohlášení, příkazy</span><span class="sxs-lookup"><span data-stu-id="8fe13-224">Given the above declaration, the statements</span></span>
```csharp
Point p1 = new Point();
Point p2 = new Point(0, 0);
```
<span data-ttu-id="8fe13-225">obě vytváří `Point` s `x` a `y` inicializována na nulovou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="8fe13-225">both create a `Point` with `x` and `y` initialized to zero.</span></span>

<span data-ttu-id="8fe13-226">Konstruktor instance struktury není dovoleno zahrnují inicializátoru konstruktoru formuláře `base(...)`.</span><span class="sxs-lookup"><span data-stu-id="8fe13-226">A struct instance constructor is not permitted to include a constructor initializer of the form `base(...)`.</span></span>

<span data-ttu-id="8fe13-227">Pokud konstruktor instance struktury neurčuje inicializátoru konstruktoru `this` odpovídá proměnné `out` parametr typu struktury a podobně jako `out` parametr, `this` musí být jednoznačně přiřazena () [Jednoznačného přiřazení](variables.md#definite-assignment)) na jakémkoliv místě, kde konstruktor vrátí.</span><span class="sxs-lookup"><span data-stu-id="8fe13-227">If the struct instance constructor doesn't specify a constructor initializer, the `this` variable corresponds to an `out` parameter of the struct type, and similar to an `out` parameter, `this` must be definitely assigned ([Definite assignment](variables.md#definite-assignment)) at every location where the constructor returns.</span></span> <span data-ttu-id="8fe13-228">Pokud konstruktor instance struktury určuje inicializátoru konstruktoru `this` odpovídá proměnné `ref` parametr typu struktury a podobně jako `ref` parametr, `this` se považuje za jednoznačně přiřazené v vstupem do těla konstruktoru.</span><span class="sxs-lookup"><span data-stu-id="8fe13-228">If the struct instance constructor specifies a constructor initializer, the `this` variable corresponds to a `ref` parameter of the struct type, and similar to a `ref` parameter, `this` is considered definitely assigned on entry to the constructor body.</span></span> <span data-ttu-id="8fe13-229">Zvažte implementaci konstruktoru instance níže:</span><span class="sxs-lookup"><span data-stu-id="8fe13-229">Consider the instance constructor implementation below:</span></span>
```csharp
struct Point
{
    int x, y;

    public int X {
        set { x = value; }
    }

    public int Y {
        set { y = value; }
    }

    public Point(int x, int y) {
        X = x;        // error, this is not yet definitely assigned
        Y = y;        // error, this is not yet definitely assigned
    }
}
```

<span data-ttu-id="8fe13-230">Žádné členskou funkci instance (včetně přístupové objekty set vlastnosti `X` a `Y`) nelze volat, dokud všechna pole struktury vytváří jednoznačně přiřazena.</span><span class="sxs-lookup"><span data-stu-id="8fe13-230">No instance member function (including the set accessors for the properties `X` and `Y`) can be called until all fields of the struct being constructed have been definitely assigned.</span></span> <span data-ttu-id="8fe13-231">Jedinou výjimkou zahrnuje automaticky implementované vlastnosti ([automaticky implementované vlastnosti](classes.md#automatically-implemented-properties)).</span><span class="sxs-lookup"><span data-stu-id="8fe13-231">The only exception involves automatically implemented properties ([Automatically implemented properties](classes.md#automatically-implemented-properties)).</span></span> <span data-ttu-id="8fe13-232">Pravidla jednoznačného přiřazení ([jednoduché přiřazení výrazy](variables.md#simple-assignment-expressions)) konkrétně s výjimkou přiřazení na automatickou vlastnost typu struktury v rámci konstruktoru instance tohoto typu struktury: taková přiřazení se považuje za jednoznačného přiřazení skryté pomocným polem vlastnosti automaticky.</span><span class="sxs-lookup"><span data-stu-id="8fe13-232">The definite assignment rules ([Simple assignment expressions](variables.md#simple-assignment-expressions)) specifically exempt assignment to an auto-property of a struct type within an instance constructor of that struct type: such an assignment is considered a definite assignment of the hidden backing field of the auto-property.</span></span> <span data-ttu-id="8fe13-233">Proto následující může:</span><span class="sxs-lookup"><span data-stu-id="8fe13-233">Thus, the following is allowed:</span></span>

```csharp
struct Point
{
    public int X { get; set; }
    public int Y { get; set; }

    public Point(int x, int y) {
        X = x;      // allowed, definitely assigns backing field
        Y = y;      // allowed, definitely assigns backing field
    }
```

### <a name="destructors"></a><span data-ttu-id="8fe13-234">Destruktory</span><span class="sxs-lookup"><span data-stu-id="8fe13-234">Destructors</span></span>

<span data-ttu-id="8fe13-235">Chcete-li deklarovat destruktor není povolena struktury.</span><span class="sxs-lookup"><span data-stu-id="8fe13-235">A struct is not permitted to declare a destructor.</span></span>

### <a name="static-constructors"></a><span data-ttu-id="8fe13-236">Statické konstruktory</span><span class="sxs-lookup"><span data-stu-id="8fe13-236">Static constructors</span></span>

<span data-ttu-id="8fe13-237">Statické konstruktory pro struktury pomocí většiny stejná pravidla jako u třídy.</span><span class="sxs-lookup"><span data-stu-id="8fe13-237">Static constructors for structs follow most of the same rules as for classes.</span></span> <span data-ttu-id="8fe13-238">Provedení statický konstruktor pro typ struktury se aktivuje první z následujících událostí v rámci domény aplikace:</span><span class="sxs-lookup"><span data-stu-id="8fe13-238">The execution of a static constructor for a struct type is triggered by the first of the following events to occur within an application domain:</span></span>

*  <span data-ttu-id="8fe13-239">Statický člen typu Struktura se odkazuje.</span><span class="sxs-lookup"><span data-stu-id="8fe13-239">A static member of the struct type is referenced.</span></span>
*  <span data-ttu-id="8fe13-240">Explicitně deklarované konstruktoru typu Struktura se nazývá.</span><span class="sxs-lookup"><span data-stu-id="8fe13-240">An explicitly declared constructor of the struct type is called.</span></span>

<span data-ttu-id="8fe13-241">Vytváření výchozích hodnot ([výchozí hodnoty](structs.md#default-values)) struktury typy neaktivuje statický konstruktor.</span><span class="sxs-lookup"><span data-stu-id="8fe13-241">The creation of default values ([Default values](structs.md#default-values)) of struct types does not trigger the static constructor.</span></span> <span data-ttu-id="8fe13-242">(Příkladem je počáteční hodnota elementů v matici.)</span><span class="sxs-lookup"><span data-stu-id="8fe13-242">(An example of this is the initial value of elements in an array.)</span></span>

## <a name="struct-examples"></a><span data-ttu-id="8fe13-243">Příklady – struktura</span><span class="sxs-lookup"><span data-stu-id="8fe13-243">Struct examples</span></span>

<span data-ttu-id="8fe13-244">Následující příklad zobrazuje dvě důležité příklady použití `struct` typy a vytvoří typy, které je možné podobně předdefinované typy jazyka, ale s upravenou sémantiku.</span><span class="sxs-lookup"><span data-stu-id="8fe13-244">The following shows two significant examples of using `struct` types to create types that can be used similarly to the predefined types of the language, but with modified semantics.</span></span>

### <a name="database-integer-type"></a><span data-ttu-id="8fe13-245">Celočíselný typ databáze</span><span class="sxs-lookup"><span data-stu-id="8fe13-245">Database integer type</span></span>

<span data-ttu-id="8fe13-246">`DBInt` Celočíselného typu, který může představovat úplnou sadu hodnot, které implementuje strukturu níže `int` typu a navíc další stav, který určuje neznámou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="8fe13-246">The `DBInt` struct below implements an integer type that can represent the complete set of values of the `int` type, plus an additional state that indicates an unknown value.</span></span> <span data-ttu-id="8fe13-247">Typ s těmito charakteristikami se běžně používá v databázích.</span><span class="sxs-lookup"><span data-stu-id="8fe13-247">A type with these characteristics is commonly used in databases.</span></span>

```csharp
using System;

public struct DBInt
{
    // The Null member represents an unknown DBInt value.

    public static readonly DBInt Null = new DBInt();

    // When the defined field is true, this DBInt represents a known value
    // which is stored in the value field. When the defined field is false,
    // this DBInt represents an unknown value, and the value field is 0.

    int value;
    bool defined;

    // Private instance constructor. Creates a DBInt with a known value.

    DBInt(int value) {
        this.value = value;
        this.defined = true;
    }

    // The IsNull property is true if this DBInt represents an unknown value.

    public bool IsNull { get { return !defined; } }

    // The Value property is the known value of this DBInt, or 0 if this
    // DBInt represents an unknown value.

    public int Value { get { return value; } }

    // Implicit conversion from int to DBInt.

    public static implicit operator DBInt(int x) {
        return new DBInt(x);
    }

    // Explicit conversion from DBInt to int. Throws an exception if the
    // given DBInt represents an unknown value.

    public static explicit operator int(DBInt x) {
        if (!x.defined) throw new InvalidOperationException();
        return x.value;
    }

    public static DBInt operator +(DBInt x) {
        return x;
    }

    public static DBInt operator -(DBInt x) {
        return x.defined ? -x.value : Null;
    }

    public static DBInt operator +(DBInt x, DBInt y) {
        return x.defined && y.defined? x.value + y.value: Null;
    }

    public static DBInt operator -(DBInt x, DBInt y) {
        return x.defined && y.defined? x.value - y.value: Null;
    }

    public static DBInt operator *(DBInt x, DBInt y) {
        return x.defined && y.defined? x.value * y.value: Null;
    }

    public static DBInt operator /(DBInt x, DBInt y) {
        return x.defined && y.defined? x.value / y.value: Null;
    }

    public static DBInt operator %(DBInt x, DBInt y) {
        return x.defined && y.defined? x.value % y.value: Null;
    }

    public static DBBool operator ==(DBInt x, DBInt y) {
        return x.defined && y.defined? x.value == y.value: DBBool.Null;
    }

    public static DBBool operator !=(DBInt x, DBInt y) {
        return x.defined && y.defined? x.value != y.value: DBBool.Null;
    }

    public static DBBool operator >(DBInt x, DBInt y) {
        return x.defined && y.defined? x.value > y.value: DBBool.Null;
    }

    public static DBBool operator <(DBInt x, DBInt y) {
        return x.defined && y.defined? x.value < y.value: DBBool.Null;
    }

    public static DBBool operator >=(DBInt x, DBInt y) {
        return x.defined && y.defined? x.value >= y.value: DBBool.Null;
    }

    public static DBBool operator <=(DBInt x, DBInt y) {
        return x.defined && y.defined? x.value <= y.value: DBBool.Null;
    }

    public override bool Equals(object obj) {
        if (!(obj is DBInt)) return false;
        DBInt x = (DBInt)obj;
        return value == x.value && defined == x.defined;
    }

    public override int GetHashCode() {
        return value;
    }

    public override string ToString() {
        return defined? value.ToString(): "DBInt.Null";
    }
}
```

### <a name="database-boolean-type"></a><span data-ttu-id="8fe13-248">Logický typ databáze</span><span class="sxs-lookup"><span data-stu-id="8fe13-248">Database boolean type</span></span>

<span data-ttu-id="8fe13-249">`DBBool` Níže struktura implementuje logický typ s hodnotou tři.</span><span class="sxs-lookup"><span data-stu-id="8fe13-249">The `DBBool` struct below implements a three-valued logical type.</span></span> <span data-ttu-id="8fe13-250">Možné hodnoty tohoto typu jsou `DBBool.True`, `DBBool.False`, a `DBBool.Null`, kde `Null` člen určuje neznámou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="8fe13-250">The possible values of this type are `DBBool.True`, `DBBool.False`, and `DBBool.Null`, where the `Null` member indicates an unknown value.</span></span> <span data-ttu-id="8fe13-251">Tyto tři vracející logické typy se běžně používají v databázích.</span><span class="sxs-lookup"><span data-stu-id="8fe13-251">Such three-valued logical types are commonly used in databases.</span></span>

```csharp
using System;

public struct DBBool
{
    // The three possible DBBool values.

    public static readonly DBBool Null = new DBBool(0);
    public static readonly DBBool False = new DBBool(-1);
    public static readonly DBBool True = new DBBool(1);

    // Private field that stores -1, 0, 1 for False, Null, True.

    sbyte value;

    // Private instance constructor. The value parameter must be -1, 0, or 1.

    DBBool(int value) {
        this.value = (sbyte)value;
    }

    // Properties to examine the value of a DBBool. Return true if this
    // DBBool has the given value, false otherwise.

    public bool IsNull { get { return value == 0; } }

    public bool IsFalse { get { return value < 0; } }

    public bool IsTrue { get { return value > 0; } }

    // Implicit conversion from bool to DBBool. Maps true to DBBool.True and
    // false to DBBool.False.

    public static implicit operator DBBool(bool x) {
        return x? True: False;
    }

    // Explicit conversion from DBBool to bool. Throws an exception if the
    // given DBBool is Null, otherwise returns true or false.

    public static explicit operator bool(DBBool x) {
        if (x.value == 0) throw new InvalidOperationException();
        return x.value > 0;
    }

    // Equality operator. Returns Null if either operand is Null, otherwise
    // returns True or False.

    public static DBBool operator ==(DBBool x, DBBool y) {
        if (x.value == 0 || y.value == 0) return Null;
        return x.value == y.value? True: False;
    }

    // Inequality operator. Returns Null if either operand is Null, otherwise
    // returns True or False.

    public static DBBool operator !=(DBBool x, DBBool y) {
        if (x.value == 0 || y.value == 0) return Null;
        return x.value != y.value? True: False;
    }

    // Logical negation operator. Returns True if the operand is False, Null
    // if the operand is Null, or False if the operand is True.

    public static DBBool operator !(DBBool x) {
        return new DBBool(-x.value);
    }

    // Logical AND operator. Returns False if either operand is False,
    // otherwise Null if either operand is Null, otherwise True.

    public static DBBool operator &(DBBool x, DBBool y) {
        return new DBBool(x.value < y.value? x.value: y.value);
    }

    // Logical OR operator. Returns True if either operand is True, otherwise
    // Null if either operand is Null, otherwise False.

    public static DBBool operator |(DBBool x, DBBool y) {
        return new DBBool(x.value > y.value? x.value: y.value);
    }

    // Definitely true operator. Returns true if the operand is True, false
    // otherwise.

    public static bool operator true(DBBool x) {
        return x.value > 0;
    }

    // Definitely false operator. Returns true if the operand is False, false
    // otherwise.

    public static bool operator false(DBBool x) {
        return x.value < 0;
    }

    public override bool Equals(object obj) {
        if (!(obj is DBBool)) return false;
        return value == ((DBBool)obj).value;
    }

    public override int GetHashCode() {
        return value;
    }

    public override string ToString() {
        if (value > 0) return "DBBool.True";
        if (value < 0) return "DBBool.False";
        return "DBBool.Null";
    }
}
```
