---
ms.openlocfilehash: dbea611280a644adc25247b9887986e129c59b68
ms.sourcegitcommit: a5e393b018b04dfa55aae0000290ca087b508495
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/14/2019
ms.locfileid: "72310369"
---
# <a name="unsafe-code"></a>Nebezpečný kód

Základní C# jazyk definovaný v předchozích kapitolách se liší hlavně od jazyka C a C++ při vynechání ukazatelů jako datového typu. Místo toho C# poskytuje odkazy a možnost vytvářet objekty, které jsou spravovány systémem uvolňování paměti. Tento návrh, společně s jinými funkcemi, C# je mnohem bezpečnější než jazyk C nebo. C++ V základním C# jazyce není jednoduše možné mít neinicializovaná proměnnou, ukazatel "dangling" nebo výraz, který indexuje pole nad rámec jeho hranic. Všechny kategorie chyb, které rutinně Plague C a C++ programy jsou tak eliminovány.

I když je prakticky každá konstrukce typu ukazatele v C++ jazyce C nebo má protějšek typu C#odkazu v, existují situace, kdy je možné, že přístup k typům ukazatelů bude nezbytný. Například propojení s podkladovým operačním systémem, přístup k zařízení mapované paměti nebo implementace algoritmu, který je časově kritický, nemusí být možné nebo praktické bez přístupu k ukazatelům. Aby bylo možné tuto potřebu C# vyřešit, poskytuje možnost psát ***nezabezpečený kód***.

V nebezpečném kódu je možné deklarovat a pracovat na ukazatelích pro provádění převodů mezi ukazateli a integrálními typy, pro převzetí adresy proměnných a tak dále. Ve smyslu je psaní nebezpečného kódu podobně jako psaní kódu jazyka C v C# rámci programu.

Nezabezpečený kód je ve skutečnosti "bezpečnou" funkcí z perspektivy vývojářů i uživatelů. Nezabezpečený kód musí být jasně označený modifikátorem `unsafe`, takže vývojáři nemůžou nepoužívat nezabezpečené funkce omylem a prováděcí modul funguje, aby se zajistilo, že nezabezpečený kód nemůže být spuštěn v nedůvěryhodném prostředí.

## <a name="unsafe-contexts"></a>Nezabezpečené kontexty

Nezabezpečené funkce nástroje C# jsou k dispozici pouze v nebezpečných kontextech. Nezabezpečený kontext je zaveden zahrnutím modifikátoru `unsafe` v deklaraci typu nebo členu nebo použitím *unsafe_statement*:

*  Deklarace třídy, struktury, rozhraní nebo delegáta může obsahovat modifikátor @no__t 0. v takovém případě je celý textový rozsah deklarace typu (včetně těla třídy, struktury nebo rozhraní) považován za nezabezpečený kontext.
*  Deklarace pole, metody, vlastnosti, události, indexeru, operátora, konstruktoru instance, destruktoru nebo statického konstruktoru může obsahovat modifikátor `unsafe`. v takovém případě je celý textový rozsah deklarace člena považován za nezabezpečený kontext.
*  *Unsafe_statement* umožňuje použití nebezpečného kontextu v rámci *bloku*. Celý textový rozsah asociovaného *bloku* je považován za nezabezpečený kontext.

Níže jsou uvedeny související gramatické výroby.

```antlr
class_modifier_unsafe
    : 'unsafe'
    ;

struct_modifier_unsafe
    : 'unsafe'
    ;

interface_modifier_unsafe
    : 'unsafe'
    ;

delegate_modifier_unsafe
    : 'unsafe'
    ;

field_modifier_unsafe
    : 'unsafe'
    ;

method_modifier_unsafe
    : 'unsafe'
    ;

property_modifier_unsafe
    : 'unsafe'
    ;

event_modifier_unsafe
    : 'unsafe'
    ;

indexer_modifier_unsafe
    : 'unsafe'
    ;

operator_modifier_unsafe
    : 'unsafe'
    ;

constructor_modifier_unsafe
    : 'unsafe'
    ;

destructor_declaration_unsafe
    : attributes? 'extern'? 'unsafe'? '~' identifier '(' ')' destructor_body
    | attributes? 'unsafe'? 'extern'? '~' identifier '(' ')' destructor_body
    ;

static_constructor_modifiers_unsafe
    : 'extern'? 'unsafe'? 'static'
    | 'unsafe'? 'extern'? 'static'
    | 'extern'? 'static' 'unsafe'?
    | 'unsafe'? 'static' 'extern'?
    | 'static' 'extern'? 'unsafe'?
    | 'static' 'unsafe'? 'extern'?
    ;

embedded_statement_unsafe
    : unsafe_statement
    | fixed_statement
    ;

unsafe_statement
    : 'unsafe' block
    ;
```

V příkladu

```csharp
public unsafe struct Node
{
    public int Value;
    public Node* Left;
    public Node* Right;
}
```

Modifikátor `unsafe` určený v deklaraci struktury způsobí, že celý textový rozsah deklarace struktury se stane nebezpečným kontextem. Proto je možné deklarovat pole `Left` a `Right` jako typ ukazatele. Lze také zapsat příklad výše.

```csharp
public struct Node
{
    public int Value;
    public unsafe Node* Left;
    public unsafe Node* Right;
}
```

V tomto případě modifikátory `unsafe` v deklaracích polí způsobí, že tyto deklarace budou považovány za nebezpečné kontexty.

Kromě vytvoření nebezpečného kontextu, což umožňuje použití typů ukazatelů, modifikátor `unsafe` nemá žádný vliv na typ nebo člen. V příkladu

```csharp
public class A
{
    public unsafe virtual void F() {
        char* p;
        ...
    }
}

public class B: A
{
    public override void F() {
        base.F();
        ...
    }
}
```

Modifikátor `unsafe` v metodě `F` v `A` způsobí, že se textový rozsah `F` stane nebezpečným kontextem, ve kterém lze použít nebezpečné funkce jazyka. V přepsání `F` v `B` není nutné znovu zadávat modifikátor `unsafe` – Pokud by nedošlo k použití metody `F` v `B`, musí mít přístup k nebezpečným funkcím.

Tato situace je mírně odlišná, pokud je typ ukazatele součástí signatury metody.

```csharp
public unsafe class A
{
    public virtual void F(char* p) {...}
}

public class B: A
{
    public unsafe override void F(char* p) {...}
}
```

Z toho důvodu, že signatura `F` zahrnuje typ ukazatele, může být zapsána pouze v nezabezpečeném kontextu. Nezabezpečený kontext však lze začlenit buď tak, že celou třídu není bezpečná, jak je to v případě `A`, nebo zahrnutím modifikátoru `unsafe` v deklaraci metody, jako je například případ v `B`.

## <a name="pointer-types"></a>Typy ukazatelů

V nezabezpečeném kontextu může být *typ* ([typy](types.md)) *pointer_type* a také *value_type* nebo *reference_type*. Ve výrazu `typeof` ([výrazy vytváření anonymních objektů](expressions.md#anonymous-object-creation-expressions)) ale můžete použít i *pointer_type* mimo nezabezpečený kontext, protože takové použití není bezpečné.

```antlr
type_unsafe
    : pointer_type
    ;
```

*Pointer_type* je zapsán jako *unmanaged_type* nebo klíčové slovo `void`, následované tokenem `*`:

```antlr
pointer_type
    : unmanaged_type '*'
    | 'void' '*'
    ;

unmanaged_type
    : type
    ;
```

Typ zadaný před `*` v typu ukazatele se nazývá ***typ referenční*** typu ukazatele. Představuje typ proměnné, do které hodnota typu ukazatele odkazuje.

Na rozdíl od odkazů (hodnoty typů odkazů) ukazatele nejsou sledovány systémem uvolňování paměti – systém uvolňování paměti nemá žádné znalosti ukazatelů a dat, na která odkazují. Z tohoto důvodu ukazatel není povolen odkazování na odkaz nebo na strukturu, která obsahuje odkazy, a typ referenční ukazatele musí být *unmanaged_type*.

*Unmanaged_type* je libovolný typ, který není *reference_type* nebo konstruovaný typ a neobsahuje pole *reference_type* nebo vytvořená v žádné úrovni vnoření. Jinými slovy je *unmanaged_type* jedna z následujících:

*  `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, 0, 1 nebo 2.
*  Libovolný *enum_type*.
*  Libovolný *pointer_type*.
*  Jakékoli uživatelsky definované *struct_type* , které není konstruovaným typem a obsahuje pouze pole *unmanaged_type*.

Intuitivní pravidlo pro kombinování ukazatelů a odkazů je, že referents odkazy (objekty) mají obsahovat ukazatele, ale referents ukazatelů nemají oprávnění obsahovat odkazy.

V následující tabulce jsou uvedeny některé příklady typů ukazatelů:

| __Příklad__ | __Popis__                               |
|-------------|-----------------------------------------------|
| `byte*`     | Ukazatel na `byte`                             |
| `char*`     | Ukazatel na `char`                             |
| `int**`     | Ukazatel na ukazatel na `int`                   |
| `int*[]`    | Jednorozměrné pole ukazatelů na `int` |
| `void*`     | Ukazatel na neznámý typ                       |

Pro danou implementaci musí mít všechny typy ukazatelů stejnou velikost a reprezentace.

Na rozdíl od jazyka C++C a, pokud je více ukazatelů deklarováno ve stejné deklaraci C# , v `*` je zapsáno pouze s podkladovým typem, nikoli jako prefix punctuator u každého názvu ukazatele. Příklad

```csharp
int* pi, pj;    // NOT as int *pi, *pj;
```

Hodnota ukazatele typu `T*` představuje adresu proměnné typu `T`. Operátor dereference ukazatele `*` ([Indirekce ukazatele](unsafe-code.md#pointer-indirection)) se dá použít pro přístup k této proměnné. Například s ohledem na proměnnou `P` typu `int*`, výraz `*P` označuje proměnnou `int` nalezenou na adrese obsažené v `P`.

Podobně jako odkaz na objekt může být ukazatel `null`. Použití operátoru dereference na ukazatel `null` vede k chování definovanému implementací. Ukazatel s hodnotou `null` je reprezentován všemi-bity-Zero.

Typ `void*` představuje ukazatel na neznámý typ. Vzhledem k tomu, že typ referenční je neznámý, nelze operátor dereference použít na ukazatel typu `void*`, ani na takový ukazatel nesmí být provedena žádná aritmetická operace. Ukazatel typu `void*` však lze přetypovat na jakýkoli jiný typ ukazatele (a naopak).

Typy ukazatelů jsou samostatnou kategorií typů. Na rozdíl od typů odkazů a typů hodnot nedědí typy ukazatelů od `object` a mezi typy ukazatelů a `object` neexistují žádné převody. Konkrétně zabalení a rozbalení (zabalení[a rozbalení](types.md#boxing-and-unboxing)) nejsou pro ukazatele podporovány. Převody jsou však povoleny mezi různými typy ukazatelů a mezi typy ukazatelů a celočíselnými typy. Tento postup je popsaný v tématu [převody ukazatelů](unsafe-code.md#pointer-conversions).

*Pointer_type* nelze použít jako argument typu ([konstruované typy](types.md#constructed-types)) a odvození typu ([odvození typu](expressions.md#type-inference)) se nedaří u volání obecných metod, které by byly odvozeny argument typu jako typ ukazatele.

*Pointer_type* se dá použít jako typ pole volatile ([pole](classes.md#volatile-fields)s stálým typem).

I když mohou být ukazatele předány jako parametry `ref` nebo `out`, může to vést k nedefinovanému chování, protože ukazatel může být dobře nastaven tak, aby odkazoval na místní proměnnou, která již neexistuje, je-li volána metoda, nebo pevný objekt, na který se používá k nasměrování. , již není vyřešen. Příklad:

```csharp
using System;

class Test
{
    static int value = 20;

    unsafe static void F(out int* pi1, ref int* pi2) {
        int i = 10;
        pi1 = &i;

        fixed (int* pj = &value) {
            // ...
            pi2 = pj;
        }
    }

    static void Main() {
        int i = 10;
        unsafe {
            int* px1;
            int* px2 = &i;

            F(out px1, ref px2);

            Console.WriteLine("*px1 = {0}, *px2 = {1}",
                *px1, *px2);    // undefined behavior
        }
    }
}
```

Metoda může vracet hodnotu nějakého typu a tento typ může být ukazatel. Například pokud je předána ukazatel na souvislou sekvenci `int`s, počet prvků sekvence a jinou hodnotu `int`, následující metoda vrátí adresu této hodnoty v této sekvenci, pokud dojde ke shodě; v opačném případě vrátí `null`:

```csharp
unsafe static int* Find(int* pi, int size, int value) {
    for (int i = 0; i < size; ++i) {
        if (*pi == value) 
            return pi;
        ++pi;
    }
    return null;
}
```

V nezabezpečeném kontextu jsou k dispozici několik konstrukcí pro provoz na ukazatelích:

*  Operátor `*` lze použít k provedení dereference ukazatele ([dereference ukazatele](unsafe-code.md#pointer-indirection)).
*  Operátor `->` lze použít pro přístup k členu struktury prostřednictvím ukazatele ([Přístup člena ukazatele](unsafe-code.md#pointer-member-access)).
*  Operátor `[]` lze použít k indexování ukazatele ([přístup k prvku ukazatele](unsafe-code.md#pointer-element-access)).
*  Operátor `&` lze použít k získání adresy proměnné ([operátor address-of](unsafe-code.md#the-address-of-operator)).
*  Operátory `++` a `--` lze použít k zvýšení a snížení ukazatelů ([zvýšení a snížení ukazatele](unsafe-code.md#pointer-increment-and-decrement)).
*  Operátory `+` a `-` lze použít k provádění aritmetického ukazatele ([aritmetický ukazatel](unsafe-code.md#pointer-arithmetic)).
*  Pro porovnání ukazatelů ([porovnání ukazatelů](unsafe-code.md#pointer-comparison)) se dají použít operátory `==`, `!=`, `<`, `>`, `<=` a `=>`.
*  Operátor `stackalloc` lze použít k přidělení paměti ze zásobníku volání ([vyrovnávací paměti pevné velikosti](unsafe-code.md#fixed-size-buffers)).
*  Příkaz `fixed` lze použít k dočasné opravě proměnné tak, aby se její adresa mohla získat ([příkaz fixed](unsafe-code.md#the-fixed-statement)).

## <a name="fixed-and-moveable-variables"></a>Pevné a mobilní proměnné

Operátor address-of ([operátor address-of](unsafe-code.md#the-address-of-operator)) a příkaz `fixed` ([příkaz fixed](unsafe-code.md#the-fixed-statement)) dělí proměnné do dvou kategorií: ***pevné proměnné*** a ***přenosné proměnné***.

Pevné proměnné jsou umístěny v umístěních úložiště, která nejsou ovlivněna operací uvolňování paměti. (Příklady pevných proměnných zahrnují lokální proměnné, hodnoty parametrů a proměnné, které jsou vytvořeny pomocí přesměrování ukazatelů.) Na druhé straně se přenosné proměnné nacházejí v umístěních úložiště, která se vztahují k přemístění nebo vyřazení systémem uvolňování paměti. (Příklady pohyblivých proměnných zahrnují pole v objektech a prvcích polí.)

Operátor `&` ([operátor address-of](unsafe-code.md#the-address-of-operator)) umožňuje, aby se adresa pevné proměnné získala bez omezení. Vzhledem k tomu, že je pohyblivá proměnná předmětem přemístění nebo likvidace systémem uvolňování paměti, adresa pohyblivé proměnné se dá získat jenom pomocí příkazu `fixed` ([příkaz fixed](unsafe-code.md#the-fixed-statement)) a tato adresa zůstává platná jenom pro Doba trvání tohoto příkazu `fixed`.

V přesném smyslu je pevná proměnná jedna z následujících:

*  Proměnná, která je výsledkem *simple_name* ([jednoduché názvy](expressions.md#simple-names)), která odkazuje na místní proměnnou nebo parametr hodnoty, pokud proměnná není zachycena anonymní funkcí.
*  Proměnná, která je výsledkem *member_access* ([členský přístup](expressions.md#member-access)) formuláře `V.I`, kde `V` je pevná proměnná *struct_type*.
*  Proměnná, která vyplývají z *pointer_indirection_expression* ([dereference ukazatele](unsafe-code.md#pointer-indirection)) formuláře `*P`, *pointer_member_access* ([přístup ke členu ukazatele](unsafe-code.md#pointer-member-access)) formuláře `P->I` nebo *pointer_element_access* ( [Přístup k elementu ukazatele](unsafe-code.md#pointer-element-access)) formuláře `P[E]`.

Všechny ostatní proměnné jsou klasifikovány jako přenosné proměnné.

Všimněte si, že statické pole je klasifikované jako mobilní proměnná. Všimněte si také, že parametr `ref` nebo `out` je klasifikován jako mobilní proměnná, a to i v případě, že argument zadaný pro parametr je pevná proměnná. Nakonec si všimněte, že proměnná vytvořená pomocí přesměrování ukazatele je vždy klasifikována jako pevná proměnná.

## <a name="pointer-conversions"></a>Převody ukazatele

V nezabezpečeném kontextu je sada dostupných implicitních převodů ([implicitní převody](conversions.md#implicit-conversions)) rozšířena tak, aby zahrnovala následující implicitní převody ukazatelů:

*  Z libovolného *pointer_type* na typ `void*`.
*  Z literálu `null` pro libovolný *pointer_type*.

Kromě toho v nezabezpečeném kontextu je sada dostupných explicitních převodů ([explicitní převody](conversions.md#explicit-conversions)) rozšířena tak, aby zahrnovala následující explicitní převody ukazatelů:

*  Z libovolného *pointer_typeu* na jakýkoli jiný *pointer_type*.
*  Z `sbyte` `byte`, `short`, `ushort`, `int`, `uint`, `long` nebo `ulong` pro všechny *pointer_type*.
*  Z libovolného *pointer_type* na `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long` nebo `ulong`.

V nezabezpečeném kontextu sada standardních implicitních převodů ([standardní implicitní převody](conversions.md#standard-implicit-conversions)) obsahuje následující převod ukazatele:

*  Z libovolného *pointer_type* na typ `void*`.

Převody mezi dvěma typy ukazatelů nikdy nezmění skutečnou hodnotu ukazatele. Jinými slovy, převod z jednoho typu ukazatele na jiný nemá žádný vliv na podkladovou adresu určenou ukazatelem.

Když je jeden typ ukazatele převeden na jiný, pokud výsledný ukazatel není správně zarovnán pro typ Point-to, chování není definováno, pokud je výsledek zpětně odkazován. Obecně platí, že koncept "správně zarovnán" je přenosný: Pokud je ukazatel na typ `A` správně zarovnán pro ukazatel na typ `B`, který je zase správně zarovnán pro ukazatel na typ `C`, pak je ukazatel na typ `A` správně zarovnán pro ukazatel na typ `C`.

Vezměte v úvahu následující případ, ve kterém je k proměnné s jedním typem přistupované prostřednictvím ukazatele na jiný typ:

```csharp
char c = 'A';
char* pc = &c;
void* pv = pc;
int* pi = (int*)pv;
int i = *pi;         // undefined
*pi = 123456;        // undefined
```

Když je typ ukazatele převeden na ukazatel na Byte, výsledek odkazuje na nejnižší adresovaný bajt proměnné. Po sobě jdoucí zvýšení výsledku, až po velikost proměnné, vrátí ukazatel na zbývající bajty této proměnné. Například následující metoda zobrazí každý z osmi bajtů v hodnotě Double jako šestnáctkovou hodnotu:

```csharp
using System;

class Test
{
    unsafe static void Main() {
      double d = 123.456e23;
        unsafe {
           byte* pb = (byte*)&d;
            for (int i = 0; i < sizeof(double); ++i)
               Console.Write("{0:X2} ", *pb++);
            Console.WriteLine();
        }
    }
}
```

Vytvářený výstup samozřejmě závisí na endian.

Mapování mezi ukazateli a celými čísly jsou definovaná implementací. Nicméně u architektur PROCESORů 32 * a 64 s lineárním adresním prostorem se převody ukazatelů na nebo z integrálních typů obvykle chovají stejně jako převody hodnot `uint` nebo `ulong`, v uvedeném pořadí, do nebo z těchto integrálních typů.

### <a name="pointer-arrays"></a>Pole ukazatelů

V nezabezpečeném kontextu mohou být vytvořena pole ukazatelů. V polích ukazatelů jsou povoleny pouze některé převody, které platí pro jiné typy polí:

*  Implicitní převod odkazu ([implicitní převod odkazů](conversions.md#implicit-reference-conversions)) z libovolného *array_type* na `System.Array` a rozhraní, které implementuje, platí také pro pole ukazatelů. Nicméně jakýkoliv pokus o přístup k prvkům pole pomocí `System.Array` nebo rozhraní, které implementuje, způsobí výjimku za běhu, protože typy ukazatelů nelze převést na `object`.
*  Implicitní a explicitní převody odkazů ([implicitní převody odkazů](conversions.md#implicit-reference-conversions), [explicitní převody odkazů](conversions.md#explicit-reference-conversions)) z jednorozměrného pole typu `S[]` na `System.Collections.Generic.IList<T>` a jeho Obecná základní rozhraní se nikdy nevztahují na pole ukazatelů, vzhledem k tomu, že typy ukazatelů nelze použít jako argumenty typu a nedochází k žádným převodům z typů ukazatelů na typy bez ukazatelů.
*  Explicitní převod odkazu ([explicitní převody odkazů](conversions.md#explicit-reference-conversions)) z `System.Array` a rozhraní, které implementuje na libovolný *array_type* , se vztahuje na pole ukazatelů.
*  Explicitní převody odkazů ([explicitní převody odkazů](conversions.md#explicit-reference-conversions)) z `System.Collections.Generic.IList<S>` a jeho základních rozhraní na typ jednorozměrného pole `T[]` nikdy neplatí pro pole ukazatelů, protože typy ukazatelů nelze použít jako argumenty typu a existují žádné převody z typů ukazatele na typy bez ukazatele.

Tato omezení znamenají, že rozšíření pro příkaz `foreach` nad poli popsanými v [příkazu foreach](statements.md#the-foreach-statement) nelze použít na pole ukazatelů. Místo toho příkaz foreach formuláře

```csharp
foreach (V v in x) embedded_statement
```

kde typ `x` je typ pole formuláře `T[,,...,]`, `N` je počet dimenzí minus 1 a `T` nebo `V` je typ ukazatele, rozbalený pomocí vnořených smyček, jak je znázorněno níže:

```csharp
{
    T[,,...,] a = x;
    for (int i0 = a.GetLowerBound(0); i0 <= a.GetUpperBound(0); i0++)
    for (int i1 = a.GetLowerBound(1); i1 <= a.GetUpperBound(1); i1++)
    ...
    for (int iN = a.GetLowerBound(N); iN <= a.GetUpperBound(N); iN++) {
        V v = (V)a.GetValue(i0,i1,...,iN);
        embedded_statement
    }
}
```

Proměnné `a`, `i0` `i1`,..., `iN` nejsou viditelné ani dostupné pro `x` nebo *embedded_statement* nebo jakýkoli jiný zdrojový kód tohoto programu. Proměnná `v` je v vloženém příkazu jen pro čtení. Pokud není k dispozici explicitní převod ([převody ukazatelů](unsafe-code.md#pointer-conversions)) z `T` (typ elementu) na `V`, vytvoří se chyba a neprovádí se žádné další kroky. Pokud `x` má hodnotu `null`, je vyvolána `System.NullReferenceException` v době běhu.

## <a name="pointers-in-expressions"></a>Ukazatelé ve výrazech

V nezabezpečeném kontextu může výraz vracet výsledek typu ukazatele, ale mimo nezabezpečený kontext, jedná se o chybu při kompilaci, aby výraz byl typu ukazatele. V přesném termínu mimo nezabezpečený kontext dojde k chybě při kompilaci, pokud jakýkoli *simple_name* ([jednoduché názvy](expressions.md#simple-names)), *member_access* ([přístup členů](expressions.md#member-access)), *invocation_expression* ([výrazy vyvolání](expressions.md#invocation-expressions)), nebo  *element_access* ([přístup k prvkům](expressions.md#element-access)) je typ ukazatele.

V nezabezpečeném kontextu umožňují výroba *primary_no_array_creation_expression* ([Primary Expressions](expressions.md#primary-expressions)) a *unary_expression* ([unární operátory](expressions.md#unary-operators)) následující dodatečné konstrukce:

```antlr
primary_no_array_creation_expression_unsafe
    : pointer_member_access
    | pointer_element_access
    | sizeof_expression
    ;

unary_expression_unsafe
    : pointer_indirection_expression
    | addressof_expression
    ;
```

Tyto konstrukce jsou popsány v následujících částech. Přednost a asociativita nebezpečných operátorů je odvozena gramatikou.

### <a name="pointer-indirection"></a>Indirekce ukazatele

*Pointer_indirection_expression* se skládá z hvězdičky (`*`) následovaných *unary_expression*.

```antlr
pointer_indirection_expression
    : '*' unary_expression
    ;
```

Unární operátor `*` označuje nepřímý odkaz na ukazatel a používá se k získání proměnné, na kterou ukazatel ukazuje. Výsledek vyhodnocení `*P`, kde `P` je výrazem typu ukazatele `T*`, je proměnná typu `T`. Jedná se o chybu při kompilaci pro použití unárního operátoru `*` na výraz typu `void*` nebo na výraz, který není typu ukazatele.

Účinek použití unárního operátoru `*` na ukazatel `null` je definován implementací. Konkrétně není nijak zaručeno, že tato operace vyvolá `System.NullReferenceException`.

Pokud je k ukazateli přiřazena neplatná hodnota, chování unárního operátoru `*` není definováno. Mezi neplatné hodnoty pro přesměrování ukazatele pomocí unárního operátoru `*` jsou adresy nevhodně zarovnané na typ, na který se odkazuje (viz příklad [Převod ukazatelů](unsafe-code.md#pointer-conversions)) a adresa proměnné po konci své životnosti.

Pro účely analýzy jednoznačného přiřazení je proměnná vytvořená vyhodnocením výrazu ve formátu `*P` považována za původně přiřazenou ([původně přiřazené proměnné](variables.md#initially-assigned-variables)).

### <a name="pointer-member-access"></a>Přístup ke členu ukazatele

*Pointer_member_access* se skládá z *primary_expression*, po kterém následuje token "`->`", následovaný *identifikátorem* a volitelnou *type_argument_list*.

```antlr
pointer_member_access
    : primary_expression '->' identifier
    ;
```

V přístupu člena ukazatele ve formě `P->I` musí být `P` výrazem jiného typu ukazatele než `void*` a `I` musí poznamenat přístupný člen typu, na který `P` body.

Přístup člena ukazatele `P->I` se vyhodnocuje přesně jako `(*P).I`. Popis operátoru dereference ukazatele (`*`) naleznete v tématu [dereference ukazatele](unsafe-code.md#pointer-indirection). Popis operátoru přístupu ke členu (`.`) najdete v tématu [Member Access](expressions.md#member-access).

V příkladu

```csharp
using System;

struct Point
{
    public int x;
    public int y;

    public override string ToString() {
        return "(" + x + "," + y + ")";
    }
}

class Test
{
    static void Main() {
        Point point;
        unsafe {
            Point* p = &point;
            p->x = 10;
            p->y = 20;
            Console.WriteLine(p->ToString());
        }
    }
}
```

operátor `->` se používá pro přístup k polím a vyvolání metody struktury prostřednictvím ukazatele. Vzhledem k tomu, že operace `P->I` je přesně ekvivalentní `(*P).I`, může být metoda `Main` také zapsána stejně:

```csharp
class Test
{
    static void Main() {
        Point point;
        unsafe {
            Point* p = &point;
            (*p).x = 10;
            (*p).y = 20;
            Console.WriteLine((*p).ToString());
        }
    }
}
```

### <a name="pointer-element-access"></a>Přístup k prvku ukazatele

*Pointer_element_access* se skládá z *primary_no_array_creation_expression* následovaný výrazem uzavřeným v "`[`" a "`]`".

```antlr
pointer_element_access
    : primary_no_array_creation_expression '[' expression ']'
    ;
```

Ve formuláři s ukazatelem ve formě `P[E]` `P` musí být výrazem jiného typu ukazatele než `void*` a `E` musí být výraz, který lze implicitně převést na `int`, `uint`, `long` nebo `ulong`.

Přístup k prvku formuláře `P[E]` se vyhodnocuje přesně jako `*(P + E)`. Popis operátoru dereference ukazatele (`*`) naleznete v tématu [dereference ukazatele](unsafe-code.md#pointer-indirection). Popis operátoru sčítání ukazatelů (`+`) naleznete v tématu [aritmetický ukazatel](unsafe-code.md#pointer-arithmetic).

V příkladu

```csharp
class Test
{
    static void Main() {
        unsafe {
            char* p = stackalloc char[256];
            for (int i = 0; i < 256; i++) p[i] = (char)i;
        }
    }
}
```

přístup k prvku ukazatele se používá k inicializaci vyrovnávací paměti znaků ve smyčce `for`. Vzhledem k tomu, že operace `P[E]` je přesně ekvivalentní `*(P + E)`, může být příklad stejně dobře zapsaný:

```csharp
class Test
{
    static void Main() {
        unsafe {
            char* p = stackalloc char[256];
            for (int i = 0; i < 256; i++) *(p + i) = (char)i;
        }
    }
}
```

Operátor přístupu k elementu ukazatele nekontroluje chyby mimo hranice a chování při přístupu k elementu, který je mimo rozsah, není definované. To je stejné jako v jazyce C C++a.

### <a name="the-address-of-operator"></a>Operátor address-of

*Addressof_expression* se skládá z ampersandu (`&`) následovaného *unary_expression*.

```antlr
addressof_expression
    : '&' unary_expression
    ;
```

Vzhledem k výrazu `E`, který je typu `T` a klasifikován jako pevná proměnná ([pevné a mobilní proměnné](unsafe-code.md#fixed-and-moveable-variables)), konstrukce `&E` vypočítá adresu proměnné dané `E`. Typ výsledku je `T*` a je klasifikován jako hodnota. K chybě při kompilaci dojde, pokud `E` není klasifikován jako proměnná, pokud je `E` klasifikován jako lokální proměnná jen pro čtení, nebo pokud `E` označuje pohyblivou proměnnou. V posledním případě lze pomocí příkazu fixed ([příkaz fixed](unsafe-code.md#the-fixed-statement)) dočasně "opravit" proměnnou před získáním její adresy. Jak je uvedeno v [přístupu ke členu](expressions.md#member-access), mimo konstruktor instance nebo statický konstruktor pro strukturu nebo třídu, která definuje pole `readonly`, toto pole se považuje za hodnotu, nikoli proměnnou. V takovém případě se adresa nedá vzít. Obdobně nelze adresu konstanty považovat.

Operátor `&` nevyžaduje jednoznačně přiřazený argument, ale za operací `&` je proměnná, na kterou je operátor použit, považována za jednoznačně přiřazenou v cestě provádění, ve které k operaci dojde. Je zodpovědností programátora, aby se zajistilo, že v této situaci bude provedeno správné inicializaci proměnné.

V příkladu

```csharp
using System;

class Test
{
    static void Main() {
        int i;
        unsafe {
            int* p = &i;
            *p = 123;
        }
        Console.WriteLine(i);
    }
}
```

`i` se považuje za jednoznačně přiřazenou za operaci `&i` použitou k inicializaci `p`. Přiřazení k `*p` v účinnosti inicializuje `i`, ale zahrnutí této inicializace je zodpovědností programátora a při odebrání přiřazení by nedocházelo k žádné chybě při kompilaci.

Pravidla jednoznačného přiřazení pro operátor `&` existují tak, aby bylo možné zabránit redundantní inicializaci místních proměnných. Mnoho externích rozhraní API například převezme ukazatel na strukturu, která je vyplněna rozhraním API. Volání těchto rozhraní API obvykle předávají adresu místní proměnné struktury a bez pravidla, která by vyžadovala redundantní inicializaci proměnné struct.

### <a name="pointer-increment-and-decrement"></a>Zvýšení a snížení ukazatele

V nezabezpečeném kontextu lze použít operátory `++` a `--` (operátory[přírůstku a snížení](expressions.md#postfix-increment-and-decrement-operators) [předpony a modifikátory přírůstku a snížení](expressions.md#prefix-increment-and-decrement-operators)), s výjimkou `void*`. Proto pro každý typ ukazatele `T*` jsou implicitně definovány následující operátory:

```csharp
T* operator ++(T* x);
T* operator --(T* x);
```

Operátory vytváří stejné výsledky jako `x + 1` a `x - 1` v uvedeném pořadí ([aritmetický ukazatel](unsafe-code.md#pointer-arithmetic)). Jinými slovy, pro proměnnou typu `T*` operátor `++` přidá `sizeof(T)` na adresu obsaženou v proměnné a operátor `--` odečte `sizeof(T)` od adresy obsažené v proměnné.

Pokud operace zvýšení nebo snížení ukazatele přetéká v doméně typu ukazatele, výsledek je definován implementací, ale nejsou vyprodukovány žádné výjimky.

### <a name="pointer-arithmetic"></a>Aritmetika ukazatele

V nezabezpečeném kontextu lze použít operátory `+` a `-` ([operátor sčítání](expressions.md#addition-operator) a [odčítání](expressions.md#subtraction-operator)) na hodnoty všech typů ukazatelů s výjimkou `void*`. Proto pro každý typ ukazatele `T*` jsou implicitně definovány následující operátory:

```csharp
T* operator +(T* x, int y);
T* operator +(T* x, uint y);
T* operator +(T* x, long y);
T* operator +(T* x, ulong y);

T* operator +(int x, T* y);
T* operator +(uint x, T* y);
T* operator +(long x, T* y);
T* operator +(ulong x, T* y);

T* operator -(T* x, int y);
T* operator -(T* x, uint y);
T* operator -(T* x, long y);
T* operator -(T* x, ulong y);

long operator -(T* x, T* y);
```

Vzhledem k výrazu `P` typu ukazatele `T*` a výrazu `N` typu `int`, `uint`, `long` nebo `ulong`, výrazy `P + N` a `N + P` počítají hodnotu ukazatele typu `T*`, která je výsledkem přidání 0 na adresu. předána 1. Podobně výraz `P - N` vypočítá hodnotu ukazatele typu `T*`, který je výsledkem odečítání `N * sizeof(T)` z adresy zadané `P`.

U dvou výrazů, `P` a `Q` typu ukazatele `T*`, vypočítá výraz `P - Q` rozdíl mezi adresami zadanými `P` a `Q` a pak tento rozdíl vydělí `sizeof(T)`. Typ výsledku je vždycky `long`. V důsledku toho je `P - Q` vypočítána jako `((long)(P) - (long)(Q)) / sizeof(T)`.

Příklad:

```csharp
using System;

class Test
{
    static void Main() {
        unsafe {
            int* values = stackalloc int[20];
            int* p = &values[1];
            int* q = &values[15];
            Console.WriteLine("p - q = {0}", p - q);
            Console.WriteLine("q - p = {0}", q - p);
        }
    }
}
```

který vytváří výstup:

```console
p - q = -14
q - p = 14
```

Pokud aritmetická operace ukazatele přetéká doménu typu ukazatele, výsledek je zkrácen v způsobem definovaném implementací, ale žádné výjimky se nevytvoří.

### <a name="pointer-comparison"></a>Porovnání ukazatelů

V nezabezpečeném kontextu lze použít operátory `==`, `!=`, `<`, `>`, `<=` a `=>` ([relační a operátor testování typu](expressions.md#relational-and-type-testing-operators)), a to pro hodnoty všech typů ukazatelů. Operátory porovnání ukazatelů jsou:

```csharp
bool operator ==(void* x, void* y);
bool operator !=(void* x, void* y);
bool operator <(void* x, void* y);
bool operator >(void* x, void* y);
bool operator <=(void* x, void* y);
bool operator >=(void* x, void* y);
```

Vzhledem k tomu, že implicitní převod existuje z libovolného typu ukazatele na typ `void*`, operandy libovolného typu ukazatele lze porovnat pomocí těchto operátorů. Relační operátory porovnávají adresy zadané pomocí dvou operandů, jako kdyby byla celá čísla bez znaménka.

### <a name="the-sizeof-operator"></a>Operátor sizeof

Operátor `sizeof` vrátí počet bajtů obsazených proměnnou daného typu. Typ zadaný jako operand pro `sizeof` musí být *unmanaged_type* ([typy ukazatelů](unsafe-code.md#pointer-types)).

```antlr
sizeof_expression
    : 'sizeof' '(' unmanaged_type ')'
    ;
```

Výsledek operátoru `sizeof` je hodnota typu `int`. Pro určité předdefinované typy operátor `sizeof` vrací konstantní hodnotu, jak je znázorněno v následující tabulce.


| __Vyjádření__   | __Vyústit__ |
|------------------|------------|
| `sizeof(sbyte)`  | `1`        |
| `sizeof(byte)`   | `1`        |
| `sizeof(short)`  | `2`        |
| `sizeof(ushort)` | `2`        |
| `sizeof(int)`    | `4`        |
| `sizeof(uint)`   | `4`        |
| `sizeof(long)`   | `8`        |
| `sizeof(ulong)`  | `8`        |
| `sizeof(char)`   | `2`        |
| `sizeof(float)`  | `4`        |
| `sizeof(double)` | `8`        |
| `sizeof(bool)`   | `1`        |

U všech ostatních typů je výsledkem operátoru `sizeof` definovaná implementace a je klasifikována jako hodnota, nikoli konstanta.

Pořadí, ve kterém jsou členové zabaleni do struktury, není určeno.

Pro účely zarovnání může být nepojmenované odsazení na začátku struktury, v rámci struktury a na konci struktury. Obsah bitů použitý jako výplň je neurčitý.

Při použití na operand, který má typ struktury, je výsledkem celkový počet bajtů v proměnné tohoto typu, včetně jakéhokoli odsazení.

## <a name="the-fixed-statement"></a>Příkaz fixed

V nezabezpečeném kontextu produkční prostředí *embedded_statement* ([Statements](statements.md)) umožňuje další konstrukci, příkaz `fixed`, který se používá k "opravě" pohyblivé proměnné, aby její adresa zůstala konstantní po dobu trvání příkazu. .

```antlr
fixed_statement
    : 'fixed' '(' pointer_type fixed_pointer_declarators ')' embedded_statement
    ;

fixed_pointer_declarators
    : fixed_pointer_declarator (','  fixed_pointer_declarator)*
    ;

fixed_pointer_declarator
    : identifier '=' fixed_pointer_initializer
    ;

fixed_pointer_initializer
    : '&' variable_reference
    | expression
    ;
```

Každý *fixed_pointer_declarator* deklaruje místní proměnnou daného *pointer_type* a inicializuje tuto místní proměnnou s adresou vypočítanou odpovídajícím *fixed_pointer_initializer*. Lokální proměnná deklarovaná v příkazu `fixed` je přístupná v jakémkoli *fixed_pointer_initializeru*, ke kterému dochází napravo od deklarace proměnné, a v *embedded_statement* příkazu `fixed`. Lokální proměnná deklarovaná příkazem `fixed` je považována za jen pro čtení. K chybě při kompilaci dojde v případě, že se vložený příkaz pokusí změnit tuto místní proměnnou (přes přiřazení nebo operátory `++` a `--`) nebo předat jako parametr `ref` nebo `out`.

*Fixed_pointer_initializer* může být jedna z následujících:

*  Token "`&`" následovaný *variable_reference* ([přesné pravidlo pro určení jednoznačného přiřazení](variables.md#precise-rules-for-determining-definite-assignment)) k pohyblivé proměnné ([pevné a mobilní proměnné](unsafe-code.md#fixed-and-moveable-variables)) nespravovaného typu `T` za předpokladu, že typ `T*` je implicitně převoditelné na typ ukazatele uvedený v příkazu `fixed`. V tomto případě inicializátor vypočítá adresu dané proměnné a proměnná je zaručena, aby zůstala na pevné adrese po dobu trvání příkazu `fixed`.
*  Výraz *array_type* s elementy nespravovaného typu `T`, za předpokladu, že typ `T*` je implicitně převeden na typ ukazatele uvedený v příkazu `fixed`. V tomto případě inicializátor vypočítá adresu prvního prvku v poli a celé pole je zaručeno, že zůstane na pevné adrese po dobu trvání příkazu `fixed`. Pokud je výraz pole null nebo pokud má pole nulové prvky, inicializátor vypočítá adresu rovnou nule.
*  Výraz typu `string`, pokud je typ `char*` implicitně převoditelné na typ ukazatele uvedený v příkazu `fixed`. V tomto případě inicializátor vypočítá adresu prvního znaku v řetězci a celý řetězec zaručuje, že zůstane na pevné adrese po dobu trvání příkazu `fixed`. Chování příkazu `fixed` je definováno implementací, pokud má řetězcový výraz hodnotu null.
*  *Simple_name* nebo *member_access* , který odkazuje na člen vyrovnávací paměti s pevnou velikostí pohyblivé proměnné za předpokladu, že typ člena vyrovnávací paměti pevné velikosti je implicitně převeden na typ ukazatele uvedený v příkazu `fixed`. V tomto případě inicializátor vypočítá ukazatel na první prvek vyrovnávací paměti pevné velikosti ([vyrovnávací paměti pevné velikosti ve výrazech](unsafe-code.md#fixed-size-buffers-in-expressions)) a vyrovnávací paměť pevné velikosti je zaručena, aby zůstala na pevné adrese po dobu trvání příkazu `fixed`.

Pro každou adresu, která je vypočítána *fixed_pointer_initializer* , příkaz `fixed` zajistí, že proměnná odkazovaná adresou nepodléhá přemístění nebo vyřazení systémem uvolňování paměti po dobu trvání příkazu `fixed`. Například pokud adresa vypočítaná pomocí *fixed_pointer_initializer* odkazuje na pole objektu nebo prvku instance pole, příkaz `fixed` zaručuje, že se instance obsahujícího objektu nebude přecházet nebo není odstraněna během doba života příkazu

Je zodpovědností programátora, aby se zajistilo, že ukazatelé vytvořené příkazy `fixed` nepřekročí provádění těchto příkazů. Například pokud jsou ukazatele vytvořené pomocí příkazu `fixed` předány externím rozhraním API, je úkolem programátora zajistit, aby rozhraní API nezůstala žádná paměť těchto ukazatelů.

Pevné objekty můžou způsobit fragmentaci haldy (protože se nedají přesunout). Z tohoto důvodu by objekty měly být opraveny pouze v případě nezbytně nutné a pak pouze pro nejkratší možné množství času.

Příklad

```csharp
class Test
{
    static int x;
    int y;

    unsafe static void F(int* p) {
        *p = 1;
    }

    static void Main() {
        Test t = new Test();
        int[] a = new int[10];
        unsafe {
            fixed (int* p = &x) F(p);
            fixed (int* p = &t.y) F(p);
            fixed (int* p = &a[0]) F(p);
            fixed (int* p = a) F(p);
        }
    }
}
```

ukazuje několik použití příkazu `fixed`. První příkaz opraví a získá adresu statického pole, druhý příkaz opraví a získá adresu pole instance a třetí příkaz opraví a získá adresu elementu pole. V každém případě by došlo k chybě při použití regulárního operátoru `&`, protože proměnné jsou klasifikovány jako přenosné proměnné.

Čtvrtý příkaz `fixed` v předchozím příkladu vytvoří podobný výsledek jako třetí.

Tento příklad příkazu `fixed` používá `string`:

```csharp
class Test
{
    static string name = "xx";

    unsafe static void F(char* p) {
        for (int i = 0; p[i] != '\0'; ++i)
            Console.WriteLine(p[i]);
    }

    static void Main() {
        unsafe {
            fixed (char* p = name) F(p);
            fixed (char* p = "xx") F(p);
        }
    }
}
```

V nezabezpečené prvky pole kontextu jednorozměrného pole jsou uloženy ve vzestupném pořadí indexu, počínaje indexem `0` a končící indexem `Length - 1`. U multidimenzionálních polí jsou prvky pole uloženy tak, aby byly nejprve zvyšovány indexy pravého rozměru, potom následující levá dimenze a tak dále doleva. V rámci příkazu `fixed`, který získá ukazatel `p` na instanci pole `a`, hodnoty ukazatele od `p` do `p + a.Length - 1` představuje adresy prvků v poli. Podobně proměnné od `p[0]` do `p[a.Length - 1]` reprezentují skutečné prvky pole. S ohledem na způsob, jakým jsou pole uložena, můžeme považovat pole libovolné dimenze, jako kdyby byla lineární.

Příklad:

```csharp
using System;

class Test
{
    static void Main() {
        int[,,] a = new int[2,3,4];
        unsafe {
            fixed (int* p = a) {
                for (int i = 0; i < a.Length; ++i)    // treat as linear
                    p[i] = i;
            }
        }

        for (int i = 0; i < 2; ++i)
            for (int j = 0; j < 3; ++j) {
                for (int k = 0; k < 4; ++k)
                    Console.Write("[{0},{1},{2}] = {3,2} ", i, j, k, a[i,j,k]);
                Console.WriteLine();
            }
    }
}
```

který vytváří výstup:

```console
[0,0,0] =  0 [0,0,1] =  1 [0,0,2] =  2 [0,0,3] =  3
[0,1,0] =  4 [0,1,1] =  5 [0,1,2] =  6 [0,1,3] =  7
[0,2,0] =  8 [0,2,1] =  9 [0,2,2] = 10 [0,2,3] = 11
[1,0,0] = 12 [1,0,1] = 13 [1,0,2] = 14 [1,0,3] = 15
[1,1,0] = 16 [1,1,1] = 17 [1,1,2] = 18 [1,1,3] = 19
[1,2,0] = 20 [1,2,1] = 21 [1,2,2] = 22 [1,2,3] = 23
```

V příkladu

```csharp
class Test
{
    unsafe static void Fill(int* p, int count, int value) {
        for (; count != 0; count--) *p++ = value;
    }

    static void Main() {
        int[] a = new int[100];
        unsafe {
            fixed (int* p = a) Fill(p, 100, -1);
        }
    }
}
```

příkaz `fixed` slouží k opravě pole tak, aby jeho adresa mohla být předána metodě, která přebírá ukazatel.

V tomto příkladu:

```csharp
unsafe struct Font
{
    public int size;
    public fixed char name[32];
}

class Test
{
    unsafe static void PutString(string s, char* buffer, int bufSize) {
        int len = s.Length;
        if (len > bufSize) len = bufSize;
        for (int i = 0; i < len; i++) buffer[i] = s[i];
        for (int i = len; i < bufSize; i++) buffer[i] = (char)0;
    }

    Font f;

    unsafe static void Main()
    {
        Test test = new Test();
        test.f.size = 10;
        fixed (char* p = test.f.name) {
            PutString("Times New Roman", p, 32);
        }
    }
}
```

příkaz fixed se používá k opravě vyrovnávací paměti pevné velikosti struktury, aby její adresa mohla být použita jako ukazatel.

Hodnota `char*` vytvořená opravou instance řetězce vždy odkazuje na řetězec zakončený hodnotou null. V rámci příkazu fixed, který získá ukazatel `p` do instance řetězce `s`, hodnoty ukazatele od `p` do `p + s.Length - 1` reprezentují adresy znaků v řetězci a hodnota ukazatele `p + s.Length` vždy odkazuje na znak null ( znak s hodnotou `'\0'`).

Změna objektů spravovaného typu prostřednictvím pevných ukazatelů může mít za následek nedefinované chování. Například vzhledem k tomu, že řetězce jsou neměnné, je úkolem programátora zajistit, aby se znaky odkazované ukazatelem na pevný řetězec nezměnily.

Automatické ukončení řetězce s hodnotou null je zvláště pohodlné při volání externích rozhraní API, která očekávají řetězce "C-Style". Všimněte si však, že instance řetězce má povoleno obsahovat znaky null. Pokud jsou tyto znaky null k dispozici, řetězec se zkrátí, pokud je zpracován jako zakončené znakem null `char*`.

## <a name="fixed-size-buffers"></a>Vyrovnávací paměti pevné velikosti

Vyrovnávací paměti pevné velikosti slouží k deklaraci "Style" v řádkových polích jako členů struktury a jsou primárně užitečné pro propojení s nespravovanými rozhraními API.

### <a name="fixed-size-buffer-declarations"></a>Deklarace vyrovnávací paměti pevné velikosti

***Vyrovnávací paměť pevné velikosti*** je člen, který představuje úložiště pro vyrovnávací paměť pevné délky proměnných daného typu. Deklarace vyrovnávací paměti pevné velikosti zavádí jednu nebo více vyrovnávacích pamětí s pevnou velikostí daného typu elementu. Vyrovnávací paměti pevné velikosti jsou povoleny pouze v deklaracích struktury a mohou být provedeny pouze v nezabezpečených kontextech ([nebezpečné kontexty](unsafe-code.md#unsafe-contexts)).

```antlr
struct_member_declaration_unsafe
    : fixed_size_buffer_declaration
    ;

fixed_size_buffer_declaration
    : attributes? fixed_size_buffer_modifier* 'fixed' buffer_element_type fixed_size_buffer_declarator+ ';'
    ;

fixed_size_buffer_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    | 'unsafe'
    ;

buffer_element_type
    : type
    ;

fixed_size_buffer_declarator
    : identifier '[' constant_expression ']'
    ;
```

Deklarace vyrovnávací paměti s pevnou velikostí může zahrnovat sadu atributů ([atributů](attributes.md)), modifikátor `new` ([modifikátory](classes.md#modifiers)), platnou kombinaci čtyř modifikátorů přístupu ([parametry typu a omezení](classes.md#type-parameters-and-constraints)) a modifikátoru `unsafe` ([nezabezpečené kontexty](unsafe-code.md#unsafe-contexts)). Atributy a modifikátory se vztahují na všechny členy deklarované deklarací vyrovnávací paměti s pevnou velikostí. Jedná se o chybu, aby se stejný modifikátor zobrazoval víckrát v deklaraci vyrovnávací paměti s pevnou velikostí.

Deklarace vyrovnávací paměti s pevnou velikostí není povolená pro zahrnutí modifikátoru `static`.

Typ elementu bufferu pro deklaraci vyrovnávací paměti s pevnou velikostí určuje typ prvku vyrovnávací paměti, kterou deklarace zavedla. Typ elementu buffer musí být jeden z předdefinovaných typů `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, 0 nebo 1.

Typ elementu bufferu následuje seznam deklarátory vyrovnávací paměti s pevnou velikostí, z nichž každá zavádí nového člena. Vyrovnávací paměť pevné velikosti deklarátor se skládá z identifikátoru, který člen pojmenovává, následovaný konstantním výrazem uzavřeným v tokenech `[` a `]`. Konstantní výraz označuje počet prvků v členu zavedený deklarátor vyrovnávací paměti s pevnou velikostí. Typ konstantního výrazu musí být implicitně převeden na typ `int` a hodnota musí být kladné celé číslo, které není nula.

Prvky vyrovnávací paměti s pevnou velikostí jsou zaručeny tak, aby byly rozloženy postupně v paměti.

Deklarace vyrovnávací paměti s pevnou velikostí, která deklaruje více vyrovnávacích pamětí s pevnou velikostí, je ekvivalentní více deklaracím jediné deklarace vyrovnávací paměti s pevnou velikostí se stejnými atributy a typy prvků. Příklad

```csharp
unsafe struct A
{
   public fixed int x[5], y[10], z[100];
}
```

je ekvivalentem

```csharp
unsafe struct A
{
   public fixed int x[5];
   public fixed int y[10];
   public fixed int z[100];
}
```

### <a name="fixed-size-buffers-in-expressions"></a>Vyrovnávací paměti pevné velikosti ve výrazech

Vyhledávání členů ([operátoři](expressions.md#operators)) člena vyrovnávací paměti pevné velikosti pokračuje přesně stejně jako vyhledávání členů pole.

Vyrovnávací paměť pevné velikosti může být ve výrazu odkazována pomocí *simple_name* ([odvození typu](expressions.md#type-inference)) nebo *member_access* ([Kontrola při kompilaci dynamického překladu přetížení](expressions.md#compile-time-checking-of-dynamic-overload-resolution)).

Když se na člena vyrovnávací paměti pevné velikosti odkazuje jako na jednoduchý název, je efekt stejný jako členský přístup ve formátu `this.I`, kde `I` je členem vyrovnávací paměti pevné velikosti.

V případě členství ve formuláři `E.I`, pokud `E` je typu struktury a vyhledávání členů `I` v tomto typu struktury identifikuje člena pevné velikosti, pak `E.I` je vyhodnocen jako následující:

*  Pokud výraz `E.I` nenastane v nezabezpečeném kontextu, dojde k chybě při kompilaci.
*  Pokud je `E` klasifikována jako hodnota, dojde k chybě při kompilaci.
*  V opačném případě, pokud `E` je pohyblivá proměnná ([pevné a mobilní proměnné](unsafe-code.md#fixed-and-moveable-variables)) a výraz `E.I` není *fixed_pointer_initializer* ([příkaz fixed](unsafe-code.md#the-fixed-statement)), dojde k chybě při kompilaci.
*  V opačném případě `E` odkazuje na pevnou proměnnou a výsledek výrazu je ukazatel na první prvek člena vyrovnávací paměti pevné velikosti `I` v `E`. Výsledek je typu `S*`, kde `S` je typ prvku `I` a je klasifikován jako hodnota.

Následující prvky vyrovnávací paměti s pevnou velikostí lze použít při operacích s ukazateli z prvního prvku. Přístup k prvkům vyrovnávací paměti s pevnou velikostí je na rozdíl od přístupu k polím nebezpečná operace a není zaškrtnuta rozsah.

Následující příklad deklaruje a používá strukturu s členem vyrovnávací paměti s pevnou velikostí.

```csharp
unsafe struct Font
{
    public int size;
    public fixed char name[32];
}

class Test
{
    unsafe static void PutString(string s, char* buffer, int bufSize) {
        int len = s.Length;
        if (len > bufSize) len = bufSize;
        for (int i = 0; i < len; i++) buffer[i] = s[i];
        for (int i = len; i < bufSize; i++) buffer[i] = (char)0;
    }

    unsafe static void Main()
    {
        Font f;
        f.size = 10;
        PutString("Times New Roman", f.name, 32);
    }
}
```

### <a name="definite-assignment-checking"></a>Kontrola přiřazení na určitou dobu

Vyrovnávací paměti s pevnou velikostí nepodléhají jednoznačné kontrole přiřazení ([jednoznačné přiřazení](variables.md#definite-assignment)) a členy vyrovnávací paměti pevné velikosti jsou ignorovány pro účely jednoznačné kontroly přiřazení proměnných typu struktury.

Pokud je nejvzdálenější Proměnná obsahující proměnnou struktury člena vyrovnávací paměti pevné velikosti statická proměnná, proměnná instance třídy nebo element pole, prvky vyrovnávací paměti pevné velikosti jsou automaticky inicializovány na výchozí hodnoty ([Výchozí hodnota hodnoty](variables.md#default-values)). Ve všech ostatních případech není počáteční obsah vyrovnávací paměti s pevnou velikostí definován.

## <a name="stack-allocation"></a>Přidělení zásobníku

V nezabezpečeném kontextu může deklarace místní proměnné ([deklarace místní proměnné](statements.md#local-variable-declarations)) zahrnovat inicializátor přidělení zásobníku, který přiděluje paměť ze zásobníku volání.

```antlr
local_variable_initializer_unsafe
    : stackalloc_initializer
    ;

stackalloc_initializer
    : 'stackalloc' unmanaged_type '[' expression ']'
    ;
```

*Unmanaged_type* označuje typ položek, které budou uloženy v nově přiděleném umístění, a *výraz* označuje počet těchto položek. Společně, určují velikost požadované alokace. Vzhledem k tomu, že velikost přidělení zásobníku nemůže být záporná, jedná se o chybu při kompilaci, která určuje počet položek jako *constant_expression* , který se vyhodnotí jako záporná hodnota.

Inicializátor přidělení zásobníku ve formátu `stackalloc T[E]` vyžaduje, aby `T` byl nespravovaného typu ([typy ukazatelů](unsafe-code.md#pointer-types)) a `E` jako výraz typu `int`. Konstrukce přiděluje `E * sizeof(T)` bajtů ze zásobníku volání a vrátí ukazatel typu `T*` do nově přiděleného bloku. Pokud je `E` záporná hodnota, chování není definováno. Pokud je hodnota `E` nulová, není provedena žádná alokace a vrácený ukazatel je definován implementací. Pokud není k dispozici dostatek paměti pro přidělení bloku dané velikosti, je vyvolána `System.StackOverflowException`.

Obsah nově přidělené paměti není definován.

Inicializátory přidělení zásobníku nejsou povolené v blocích `catch` nebo `finally` ([příkaz try](statements.md#the-try-statement)).

Neexistuje žádný způsob, jak explicitně uvolnit přidělenou paměť pomocí `stackalloc`. Všechny bloky paměti přidělené zásobníku vytvořené během provádění členu funkce jsou automaticky zahozeny, když tento člen funkce vrátí. To odpovídá funkci `alloca`, rozšíření se běžně našlo v jazyce C a C++ implementací.

V příkladu

```csharp
using System;

class Test
{
    static string IntToString(int value) {
        int n = value >= 0? value: -value;
        unsafe {
            char* buffer = stackalloc char[16];
            char* p = buffer + 16;
            do {
                *--p = (char)(n % 10 + '0');
                n /= 10;
            } while (n != 0);
            if (value < 0) *--p = '-';
            return new string(p, 0, (int)(buffer + 16 - p));
        }
    }

    static void Main() {
        Console.WriteLine(IntToString(12345));
        Console.WriteLine(IntToString(-999));
    }
}
```

v metodě `IntToString` se k přidělení vyrovnávací paměti 16 znaků v zásobníku používá inicializátor `stackalloc`. Vyrovnávací paměť se automaticky zahodí, když se metoda vrátí.

## <a name="dynamic-memory-allocation"></a>Dynamické přidělování paměti

S výjimkou operátoru `stackalloc` C# neposkytuje žádné předdefinované konstrukce pro správu paměti, která není v paměti pro uvolňování paměti. Tyto služby jsou obvykle poskytovány prostřednictvím podpory knihoven tříd nebo naimportované přímo z podkladového operačního systému. Například níže uvedená třída `Memory` ukazuje, jak může být k dispozici funkce haldy základního operačního systému z C#:

```csharp
using System;
using System.Runtime.InteropServices;

public static unsafe class Memory
{
    // Handle for the process heap. This handle is used in all calls to the
    // HeapXXX APIs in the methods below.
    private static readonly IntPtr s_heap = GetProcessHeap();

    // Allocates a memory block of the given size. The allocated memory is
    // automatically initialized to zero.
    public static void* Alloc(int size)
    {
        void* result = HeapAlloc(s_heap, HEAP_ZERO_MEMORY, (UIntPtr)size);
        if (result == null) throw new OutOfMemoryException();
        return result;
    }

    // Copies count bytes from src to dst. The source and destination
    // blocks are permitted to overlap.
    public static void Copy(void* src, void* dst, int count)
    {
        byte* ps = (byte*)src;
        byte* pd = (byte*)dst;
        if (ps > pd)
        {
            for (; count != 0; count--) *pd++ = *ps++;
        }
        else if (ps < pd)
        {
            for (ps += count, pd += count; count != 0; count--) *--pd = *--ps;
        }
    }

    // Frees a memory block.
    public static void Free(void* block)
    {
        if (!HeapFree(s_heap, 0, block)) throw new InvalidOperationException();
    }

    // Re-allocates a memory block. If the reallocation request is for a
    // larger size, the additional region of memory is automatically
    // initialized to zero.
    public static void* ReAlloc(void* block, int size)
    {
        void* result = HeapReAlloc(s_heap, HEAP_ZERO_MEMORY, block, (UIntPtr)size);
        if (result == null) throw new OutOfMemoryException();
        return result;
    }

    // Returns the size of a memory block.
    public static int SizeOf(void* block)
    {
        int result = (int)HeapSize(s_heap, 0, block);
        if (result == -1) throw new InvalidOperationException();
        return result;
    }

    // Heap API flags
    private const int HEAP_ZERO_MEMORY = 0x00000008;

    // Heap API functions
    [DllImport("kernel32")]
    private static extern IntPtr GetProcessHeap();

    [DllImport("kernel32")]
    private static extern void* HeapAlloc(IntPtr hHeap, int flags, UIntPtr size);

    [DllImport("kernel32")]
    private static extern bool HeapFree(IntPtr hHeap, int flags, void* block);

    [DllImport("kernel32")]
    private static extern void* HeapReAlloc(IntPtr hHeap, int flags, void* block, UIntPtr size);

    [DllImport("kernel32")]
    private static extern UIntPtr HeapSize(IntPtr hHeap, int flags, void* block);
}
```

Příklad, který používá třídu `Memory`, je uveden níže:

```csharp
class Test
{
    static unsafe void Main()
    {
        byte* buffer = null;
        try
        {
            const int Size = 256;
            buffer = (byte*)Memory.Alloc(Size);
            for (int i = 0; i < Size; i++) buffer[i] = (byte)i;
            byte[] array = new byte[Size];
            fixed (byte* p = array) Memory.Copy(buffer, p, Size);
            for (int i = 0; i < Size; i++) Console.WriteLine(array[i]);
        }
        finally
        {
            if (buffer != null) Memory.Free(buffer);
        }
    }
}
```

Příklad přiděluje 256 bajtů paměti prostřednictvím `Memory.Alloc` a inicializuje blok paměti hodnotami, které se zvyšují od 0 do 255. Poté přidělí pole bajtů elementu 256 a používá `Memory.Copy` ke zkopírování obsahu bloku paměti do pole bajtů. Nakonec je blok paměti uvolněn pomocí `Memory.Free` a obsah pole bajtů je výstupem v konzole.
