---
ms.openlocfilehash: 90001cf3d48f216787fc65e59166ec57c5d0ca34
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/25/2019
ms.locfileid: "64488802"
---
# <a name="unsafe-code"></a>Nebezpečný kód

Základní jazyk C#, jak jsou definovány v předchozích kapitol, se liší zejména z jazyka C a C++ v jeho vynechání ukazatele jako datový typ. Místo toho jazyk C# poskytuje odkazů a schopnost vytvářet objekty, které se spravují přes systému uvolňování paměti. Tento návrh spolu s dalšími funkcemi, díky C# mnohem bezpečnější jazyka C nebo C++. V základním jazyce C# jazyce není jednoduše možné mít neinicializované proměnné, ukazatel "nepropojená" nebo výraz, který indexuje pole nad rámec jeho hranice. Celé kategorie chyb, který pravidelně mor C a programy v jazyce C++ jsou tedy vyloučeny.

Zatímco prakticky každé konstrukce typu ukazatele v jazyce C nebo C++ protějšek typu odkazu v jazyce C#, jsou však situace, ve kterém přístup k typy ukazatelů bude nezbytné. Například propojení s základního operačního systému, přístup k zařízení mapované paměti nebo implementaci kritického pro čas algoritmus nemusí být možné nebo praktické bez přístupu k ukazateli. Chcete-li tyto potřeby řeší, C# poskytuje schopnost psát ***nezabezpečený kód***.

Nezabezpečený kód je možné deklarovat a fungovat u ukazatelů, provádět převody mezi ukazatele a integrálními typy převzít adresu proměnné a tak dále. V tom smyslu psaní nezabezpečený kód je mnohem psaní kódu jazyka C v rámci programu v jazyce C#.

Nezabezpečený kód je ve skutečnosti "bezpečné" funkce z pohledu vývojářů a uživatelů. Nezabezpečený kód musí být přehledně označen modifikátorem `unsafe`, takže vývojáři funkce nelze používat potenciálně nebezpečné omylem a prováděcí modul funguje a jak můžete zajistit, že nezabezpečený kód nelze provést v prostředí nedůvěryhodný.

## <a name="unsafe-contexts"></a>Nezabezpečený kontext

Nezabezpečený funkce jazyka C# jsou k dispozici pouze v kontextu unsafe. Je zavedený nezabezpečený kontext včetně `unsafe` modifikátor v deklaraci typu nebo člena, nebo když *unsafe_statement*:

*  Může obsahovat deklaraci třídy, struktury, rozhraní nebo delegáta `unsafe` modifikátor, ve kterém případ celý textový rozsahu deklarace tohoto typu (včetně těla třídy, struktury nebo rozhraní) se považuje za nezabezpečený kontext.
*  Může obsahovat deklaraci pole, metody, vlastnosti, události, indexer, – operátor, konstruktor instance, – destruktor, nebo statický konstruktor `unsafe` modifikátor, ve kterém je případ celý textový rozsah této deklarace člena považovány za nebezpečné kontext.
*  *Unsafe_statement* umožňuje použít v nezabezpečeném kontextu *bloku*. Celý textový rozsahu přidruženého *bloku* se považuje za nezabezpečený kontext.

Níže se zobrazují výroby přidružené gramatiky.

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

`unsafe` modifikátor zadaného v deklaraci struktury způsobí, že se celý textový rozsahu deklarace struktury se nezabezpečený kontext. Proto je možné deklarovat `Left` a `Right` pole typu ukazatel. Výše uvedený příklad také zapsat.

```csharp
public struct Node
{
    public int Value;
    public unsafe Node* Left;
    public unsafe Node* Right;
}
```

Tady `unsafe` modifikátorů v deklaracích pole způsobit, že tyto deklarace, aby bylo považováno za nezabezpečený kontext.

Kromě zřízení nezabezpečený kontext, tak umožňuje použití typů ukazatele `unsafe` modifikátor nemá žádný vliv na typ nebo člen. V příkladu

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

`unsafe` modifikátor `F` metoda ve `A` jednoduše způsobí, že textové rozsah `F` stane nezabezpečený kontext, ve které je možné nezabezpečené funkce jazyka. V přepsání z `F` v `B`, není nutné znovu zadat `unsafe` modifikátor – Pokud, samozřejmě `F` metoda ve `B` samotný potřebuje přístup k funkcím unsafe.

Situace se mírně liší, když typ ukazatele je součástí podpis metody

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

Tady protože `F`podpis obsahuje typ ukazatele, to je možné zapsat jen v nezabezpečeném kontextu. Však může být zavedeno nezabezpečeném kontextu provedením buď celá třída velmi nebezpečné, stejně jako v případě v `A`, nebo tak `unsafe` modifikátor v deklaraci metody, stejně jako v případě v `B`.

## <a name="pointer-types"></a>Typy ukazatelů

V nezabezpečeném kontextu *typ* ([typy](types.md)) může být *pointer_type* a také *value_type* nebo *reference_type* . Ale *pointer_type* mohou být využity také v `typeof` výrazu ([anonymní objekt vytváření výrazů](expressions.md#anonymous-object-creation-expressions)) mimo nezabezpečeném kontextu. v důsledku použití není unsafe.

```antlr
type_unsafe
    : pointer_type
    ;
```

A *pointer_type* je zapsán jako *unmanaged_type* nebo klíčové slovo `void`a po něm `*` token:

```antlr
pointer_type
    : unmanaged_type '*'
    | 'void' '*'
    ;

unmanaged_type
    : type
    ;
```

Typ určený před `*` na ukazatel typu nazývá ***referenční typ*** typu ukazatele. Představuje typ proměnné, na kterou ukazuje hodnotu typu ukazatel.

Na rozdíl od odkazy (hodnoty typy odkazů) nebudou pro účely systémem uvolňování ukazatele – systému uvolňování paměti je vůbec nezná ukazatele a data, na který odkazují. Z tohoto důvodu ukazatel není povolené tak, aby odkazoval na odkaz nebo na strukturu, která obsahuje odkazy, a musí být typu referenční ukazatel *unmanaged_type*.

*Unmanaged_type* je libovolný typ, který není *reference_type* nebo konstruovaný typ. a neobsahuje *reference_type* nebo konstruovaný typ pole na libovolné úrovni vnoření. Jinými slovy *unmanaged_type* je jedním z následujících akcí:

*  `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, `decimal`, nebo `bool`.
*  Žádné *enum_type*.
*  Any *pointer_type*.
*  Všechny uživatelem definované *struct_type* , který není konstruovaný typ a obsahuje pole *unmanaged_type*pouze s.

Intuitivní pravidlo pro kombinování ukazatele a reference je, že referents odkazů (objektů) jsou povolené tak, aby obsahovala ukazatele, ale tak, aby obsahovala odkazy nejsou povoleny referents ukazatelů.

V následující tabulce jsou uvedeny některé příklady typů ukazatelů:

| __Příklad__ | __Popis__                               |
|-------------|-----------------------------------------------|
| `byte*`     | ukazatel na `byte`                             |
| `char*`     | ukazatel na `char`                             |
| `int**`     | Ukazatel na ukazatel na `int`                   |
| `int*[]`    | Jednorozměrné pole ukazatelů na `int` |
| `void*`     | Ukazatel na neznámý typ.                       |

Pro danou implementaci všechny typy ukazatelů musí mít stejné velikosti a reprezentace.

Na rozdíl od jazyka C a C++, když jsou deklarovány většího počtu ukazatelů ve stejné deklaraci v jazyce C# `*` je zapsán spolu s základní typ, ne jako předpona interpunkci na názvů jednotlivých ukazatelů. Příklad

```csharp
int* pi, pj;    // NOT as int *pi, *pj;
```

Hodnota ukazatele s typem `T*` představuje adresu proměnné typu `T`. Operátor dereference ukazatele `*` ([dereferenci ukazatele](unsafe-code.md#pointer-indirection)) slouží pro přístup k této proměnné. Mějme například proměnná `P` typu `int*`, výraz `*P` označuje `int` nalezenou v adrese obsažené v proměnné `P`.

Jako odkaz na objekt může být ukazatel `null`. Použití operátoru dereference na `null` ukazatel výsledkem chování definované implementací. Ukazatel s hodnotou `null` je reprezentována všechny bity nula.

`void*` Typ představuje ukazatel na neznámého typu. Protože referenční typ není znám, operátor dereference nelze použít na ukazatel typu `void*`, ani žádné aritmetický provést na takový ukazatel. Ale ukazatel typu `void*` může být převeden na jiný typ ukazatele (a naopak).

Typy ukazatelů jsou samostatné kategorie typů. Na rozdíl od typy hodnot a odkazové typy, typy ukazatelů nedědí z `object` a neexistuje žádná možnost převodu mezi typy ukazatelů a `object`. Zejména, zabalení a rozbalení ([zabalení a rozbalení](types.md#boxing-and-unboxing)) nejsou podporovány pro ukazatele. Však povoleno převody mezi typy ukazatelů různých a mezi typy ukazatele a integrálními typy. To je popsáno v [převody ukazatele](unsafe-code.md#pointer-conversions).

A *pointer_type* nelze použít jako argument typu ([vytvořený typy](types.md#constructed-types)) a odvození typu ([odvození typu](expressions.md#type-inference)) je neúspěšná na volání obecné metody, která bude mít odvodit typ argumentu na typ ukazatele.

A *pointer_type* může sloužit jako typ pole s modifikátorem volatile ([pole s modifikátorem Volatile](classes.md#volatile-fields)).

I když ukazatele mohou být předány jako `ref` nebo `out` parametry, díky tomu může způsobit nedefinované chování, protože ukazatel může dobře nastavit tak, aby odkazoval na místní proměnná, která již existuje návratu volané metody nebo pevné objektu, na který sloužící k odkazovaní, už nebude vyřešen. Příklad:

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

Metoda může vrátit hodnotu typu a typ může být ukazatel. Například když dán ukazatel souvislý sekvence `int`s, počet prvků tohoto pořadí a některé další `int` hodnoty, následující metodu vrátí adresu této hodnoty v pořadí, pokud je nalezena shoda; v opačném případě vrátí `null`:

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

V nezabezpečeném kontextu jsou k dispozici pro provozování na ukazatelích několik konstruktorů:

*  `*` Operátor může být použit provést dereferenci ukazatele ([dereferenci ukazatele](unsafe-code.md#pointer-indirection)).
*  `->` Operátor může být použit pro přístup ke členu struktury prostřednictvím ukazatele ([přístupu k členovi](unsafe-code.md#pointer-member-access)).
*  `[]` Operátor může použít k indexování ukazatel ([přístup k prvkům ukazatel](unsafe-code.md#pointer-element-access)).
*  `&` Operátor může být použit pro získání adresy proměnné ([operátoru address-of](unsafe-code.md#the-address-of-operator)).
*  `++` a `--` lze operátory Inkrementace a dekrementace ukazatelů ([ukazatel Inkrementace a dekrementace](unsafe-code.md#pointer-increment-and-decrement)).
*  `+` a `-` operátory lze provést aritmetiku ukazatele ([aritmetické operace ukazatele](unsafe-code.md#pointer-arithmetic)).
*  `==`, `!=`, `<`, `>`, `<=`, A `=>` operátory může použít k porovnání ukazatelů ([porovnání ukazatelů](unsafe-code.md#pointer-comparison)).
*  `stackalloc` Operátor může být použit k přidělení paměti ze zásobníku volání ([pevných vyrovnávacích pamětí velikost](unsafe-code.md#fixed-size-buffers)).
*  `fixed` Příkaz se dá použít dočasně vyřešit proměnnou tak můžete získat adresu ([příkazu fixed](unsafe-code.md#the-fixed-statement)).

## <a name="fixed-and-moveable-variables"></a>Oprava a přesunutelný proměnné

Operátor address-of ([operátoru address-of](unsafe-code.md#the-address-of-operator)) a `fixed` – příkaz ([příkazu fixed](unsafe-code.md#the-fixed-statement)) proměnné rozdělit do dvou kategorií: ***Oprava proměnné*** a ***přesunutelný proměnné***.

Oprava proměnné se nacházejí v umístění úložiště, které jsou ovlivněny operace systému uvolňování paměti. (Pevné proměnné příklady lokální proměnné, parametry s hodnotou a proměnných vytvořené pomocí přesměrování ukazatele.) Na druhé straně přesunutelný proměnné se nacházejí v umístění úložiště, které jsou v souladu s přemístění nebo vyřazení systémem uvolňování. (Příklady přesunutelný proměnných zahrnout pole objektů a prvky pole.)

`&` – Operátor ([operátoru address-of](unsafe-code.md#the-address-of-operator)) umožňuje adresu pevná proměnná ho získat bez omezení. Ale protože přesunutelný proměnná je v souladu s přemístění nebo vyřazení modulem garbage collector, adresy přesunutelný proměnné je možné získat pomocí `fixed` – příkaz ([příkazu fixed](unsafe-code.md#the-fixed-statement)) a tuto adresu zůstane platný pouze po dobu trvání, která `fixed` příkazu.

Přesně řečeno je pevná proměnná jednu z následujících akcí:

*  Proměnné vyplývající z *simple_name* ([jednoduché názvy](expressions.md#simple-names)), který odkazuje na místní proměnná nebo parametr hodnoty, pokud není zachycena proměnné anonymní funkce.
*  Proměnné vyplývající z *member_access* ([přístup ke členu](expressions.md#member-access)) ve formátu `V.I`, kde `V` je pevná proměnná *struct_type*.
*  Proměnné vyplývající z *pointer_indirection_expression* ([dereferenci ukazatele](unsafe-code.md#pointer-indirection)) ve formátu `*P`, *pointer_member_access* ([Přístupu k členovi](unsafe-code.md#pointer-member-access)) ve formátu `P->I`, nebo *pointer_element_access* ([přístup k prvkům ukazatel](unsafe-code.md#pointer-element-access)) ve formátu `P[E]`.

Všechny ostatní proměnné jsou klasifikovány jako přesunutelný proměnné.

Všimněte si, že statické pole je klasifikován tak přesunutelný proměnné. Všimněte si také, že `ref` nebo `out` parametr klasifikovaný jako proměnnou přesunutelný i v případě, že je argument zadaný pro parametr pevná proměnná. Nakonec Upozorňujeme, že proměnné vytvořené metodou odkazován ukazatelem je vždy jsou klasifikovány jako pevná proměnná.

## <a name="pointer-conversions"></a>Převody ukazatele

V nezabezpečeném kontextu, sady k dispozici implicitní převody ([implicitních převodů](conversions.md#implicit-conversions)) je rozšířen o následující převody implicitní ukazatel:

*  Z libovolného *pointer_type* typu `void*`.
*  Z `null` literál k libovolnému *pointer_type*.

Kromě toho v nezabezpečeném kontextu, sady k dispozici explicitní převody ([explicitních převodů](conversions.md#explicit-conversions)) je rozšířen o následující převody explicitních ukazatele:

*  Z libovolného *pointer_type* u kteréhokoli jiného *pointer_type*.
*  Z `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, nebo `ulong` k libovolnému *pointer_type*.
*  Z libovolného *pointer_type* k `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, nebo `ulong`.

Nakonec v nezabezpečeném kontextu, sadu standardních implicitní převody ([standardní implicitní převody](conversions.md#standard-implicit-conversions)) zahrnuje následující převod ukazatelů:

*  Z libovolného *pointer_type* typu `void*`.

Převody mezi typy ukazatelů, dva nikdy nezmění hodnotu skutečné ukazatele. Jinými slovy převod z typu jeden ukazatel na jiný nemá žádný vliv na základní adrese dán ukazatel.

Pokud jeden typ ukazatele je převeden na jiný, v případě, že výsledný ukazatel není pro typ odkazovala na správně zarovnán, chování není definováno, pokud je přistoupit přes ukazatel výsledku. Obecně je přenositelný koncept "správně zarovnané": Pokud na ukazatel na typ `A` správně zarovnán ukazatele na typ `B`, který pak je správně zarovnán ukazatele na typ `C`, pak ukazatel na typ `A`správně zarovnán ukazatele na typ `C`.

Zvažte následující případ, ve kterém se proměnná s jednoho typu přistupuje přes ukazatel na jiný typ:

```csharp
char c = 'A';
char* pc = &c;
void* pv = pc;
int* pi = (int*)pv;
int i = *pi;         // undefined
*pi = 123456;        // undefined
```

Pokud typ ukazatele převést na ukazatel bajt, výsledek body na nejnižší bajt adresovaný proměnné. Postupné přírůstky výsledku, až do velikosti proměnné, zobrazit odkazy na zbývající bajty proměnné. Například následující metoda zobrazí každých osm bajtů v typu double jako šestnáctkovou hodnotu:

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

Výstup vytvořený samozřejmě závisí na ukládání významných bajtů.

Mapování mezi ukazatele a celá čísla jsou definované implementací. Ale na 32 * a 64-bit CPU architektury pomocí lineárního adresního prostoru, převody ukazatelů na nebo z celočíselných typů chovají obvykle úplně stejně jako převody `uint` nebo `ulong` hodnoty, resp. do nebo z těchto celočíselných typů.

### <a name="pointer-arrays"></a>Pole ukazatelů

V nezabezpečeném kontextu lze sestavit pole ukazatelů. Pro pole ukazatelů jsou povolené jenom některé z převody, které se vztahují na jiné typy polí:

*  Implicitní referenční převod ([odkaz na implicitní převody](conversions.md#implicit-reference-conversions)) z jakékoli *array_type* k `System.Array` a také implementuje rozhraní se vztahuje na ukazatel pole. Ale žádný pokus o přístup k prvkům pole pomocí `System.Array` nebo rozhraní implementuje způsobí výjimku za běhu, protože nejsou převoditelné na typy ukazatelů `object`.
*  Odkazovat na implicitní a explicitní převody ([odkaz na implicitní převody](conversions.md#implicit-reference-conversions), [odkaz na explicitní převody](conversions.md#explicit-reference-conversions)) z typu jednorozměrné pole `S[]` k `System.Collections.Generic.IList<T>` a jeho obecné základní rozhraní se nikdy nepoužívejte k polím ukazatel, protože typy ukazatelů nelze použít jako argumenty typu a nejsou žádné převody z typů ukazatele na typech bez ukazatele typy.
*  Převod explicitní odkaz ([odkaz na explicitní převody](conversions.md#explicit-reference-conversions)) z `System.Array` a rozhraní implementuje k libovolnému *array_type* platí pro pole ukazatel.
*  Odkaz na explicitní převody ([převody explicitní odkaz](conversions.md#explicit-reference-conversions)) z `System.Collections.Generic.IList<S>` a její základní rozhraní pro typ jednorozměrné pole `T[]` nikdy platí pro pole ukazatel, protože nemůže být typy ukazatelů použít jako argumenty typu a nejsou žádné převody z typů ukazatele na typech bez ukazatele typy.

Tato omezení znamenají, že rozšíření pro `foreach` příkaz v polích podle [příkazu foreach](statements.md#the-foreach-statement) nejde použít u polí ukazatele. Místo toho příkaz foreach formuláře

```csharp
foreach (V v in x) embedded_statement
```

Pokud typ `x` je typ pole formuláře `T[,,...,]`, `N` je počet rozměrů minus 1 a `T` nebo `V` je typ ukazatele, rozbalení vnořené pro smyčky následujícím způsobem:

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

Proměnné `a`, `i0`, `i1`,..., `iN` nejsou přístupné nebo viditelné pro `x` nebo *embedded_statement* nebo jiný zdrojový kód programu. Proměnná `v` je jen pro čtení v vloženým příkazem. Pokud není k dispozici explicitní převod ([převody ukazatele](unsafe-code.md#pointer-conversions)) z `T` (typ prvku) pro `V`, je vytvořen chybu a jsou přesměrováni žádné další kroky. Pokud `x` má hodnotu `null`, `System.NullReferenceException` je vyvolána v době běhu.

## <a name="pointers-in-expressions"></a>Ukazatele ve výrazech

V nezabezpečeném kontextu výraz může přinést výsledek typu ukazatele, ale mimo nezabezpečený kontext je chyba kompilace pro výraz typu ukazatele. Přesně řečeno mimo nezabezpečený kontext kompilace dojde k chybě s případné *simple_name* ([jednoduché názvy](expressions.md#simple-names)), *member_access* ([přístup ke členu ](expressions.md#member-access)), *invocation_expression* ([výrazy typu Invocation](expressions.md#invocation-expressions)), nebo *element_access* ([přístup k prvkům](expressions.md#element-access)) je typ ukazatele.

V nezabezpečeném kontextu *primary_no_array_creation_expression* ([primární výrazy](expressions.md#primary-expressions)) a *unary_expression* ([unárních operátorů](expressions.md#unary-operators)) produkční povolit následující další konstrukcí:

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

Tyto konstruktory jsou popsány v následujících částech. Priorita a asociativita operátorů nebezpečné odvozené od gramatiky.

### <a name="pointer-indirection"></a>Dereferenci ukazatele

A *pointer_indirection_expression* se skládá z hvězdičku (`*`) následované *unary_expression*.

```antlr
pointer_indirection_expression
    : '*' unary_expression
    ;
```

Unární `*` operátor označuje dereferenci ukazatele a slouží k získání proměnné, na který ukazatel ukazuje. Výsledek vyhodnocení výrazu `*P`, kde `P` je výraz typu ukazatele `T*`, je proměnná typu `T`. Je chyba kompilace použít unární `*` operátoru ve výrazu typu `void*` nebo výraz, který není typu ukazatel.

Účinek použití unární `*` operátor `null` ukazatel, je definováno implementací. Především neexistuje žádná záruka, že tato operace vyvolá `System.NullReferenceException`.

Pokud má přiřazenou neplatnou hodnotu na ukazatel, chování unární `*` operátor není definován. Mezi neplatné hodnoty pro přístup přes ukazatel pomocí unární `*` operátor jsou adresy pro typ ukazuje, nesprávně zarovnána (viz příklad v [převody ukazatele](unsafe-code.md#pointer-conversions)) a adresu proměnné po konci svého životního cyklu.

Pro účely analýzy jednoznačného přiřazení produkovaný proměnnou vyhodnocení výrazu ve formátu `*P` se považuje za původně přiřazené ([původně přiřazený proměnné](variables.md#initially-assigned-variables)).

### <a name="pointer-member-access"></a>Přístupu k členovi

A *pointer_member_access* se skládá z *primary_expression*a po něm "`->`" token a po něm *identifikátor* a volitelné *type_argument_list*.

```antlr
pointer_member_access
    : primary_expression '->' identifier
    ;
```

V přístupu ke členu ukazatel formuláře `P->I`, `P` musí být výraz ukazatele typu jiného než `void*`, a `I` musí označení přístupný člen typu, na který `P` body.

Přístup ke členu ukazatel formuláře `P->I` se vyhodnotí přesně jako `(*P).I`. Popis operátor dereference ukazatele (`*`), najdete v článku [dereferenci ukazatele](unsafe-code.md#pointer-indirection). Popis operátor přístupu členů (`.`), najdete v článku [přístup ke členu](expressions.md#member-access).

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

`->` operátor se používá pro přístup k polím a vyvolání metody struktury prostřednictvím ukazatele. Protože operace `P->I` přesně odpovídá `(*P).I`, `Main` metoda může stejně dobře být napsán takto:

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

### <a name="pointer-element-access"></a>Přístup k prvkům ukazatele

A *pointer_element_access* se skládá z *primary_no_array_creation_expression* následovaný výraz uzavřený do "`[`"a"`]`".

```antlr
pointer_element_access
    : primary_no_array_creation_expression '[' expression ']'
    ;
```

V přístup k prvkům ukazatel formuláře `P[E]`, `P` musí být výraz ukazatele typu jiného než `void*`, a `E` musí být výraz, který lze implicitně převést na `int`, `uint`, `long`, nebo `ulong`.

Přístup k prvkům ukazatel formuláře `P[E]` se vyhodnotí přesně jako `*(P + E)`. Popis operátor dereference ukazatele (`*`), najdete v článku [dereferenci ukazatele](unsafe-code.md#pointer-indirection). Popis ukazatele operátor sčítání (`+`), najdete v článku [aritmetické operace ukazatele](unsafe-code.md#pointer-arithmetic).

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

přístup k prvkům ukazatel slouží k inicializaci vyrovnávací paměti pro znaky v `for` smyčky. Protože operace `P[E]` přesně odpovídá `*(P + E)`, v příkladu může stejně dobře být napsán takto:

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

Operátor přístupu k elementu ukazatel nekontroluje celočíselných chyby a chování při přístupu k celočíselných element není definován. To je stejný jako C a C++.

### <a name="the-address-of-operator"></a>Operátor address-of

*Addressof_expression* se skládá z znak ampersand (`&`) následované *unary_expression*.

```antlr
addressof_expression
    : '&' unary_expression
    ;
```

Zadaný výraz `E` je typu `T` a je klasifikován jako pevná proměnná ([Fixed a přesunutelný proměnné](unsafe-code.md#fixed-and-moveable-variables)), konstrukce `&E` vypočítá adresy proměnné Dal `E`. Typ výsledku je `T*` a je klasifikován jako hodnotu. Pokud dojde k chybě kompilace `E` není jsou klasifikovány jako proměnnou, pokud `E` je klasifikován jako místní proměnná jen pro čtení, nebo pokud `E` označuje přesunutelný proměnné. V posledním případě fixed – příkaz ([příkazu fixed](unsafe-code.md#the-fixed-statement)) lze dočasně "Opravit" Proměnná před získáním adresy. Jak je uvedeno v [přístup ke členu](expressions.md#member-access), vně konstruktor instance nebo statický konstruktor pro struktury nebo třídy, která definuje, aplikace `readonly` pole, toto pole je považován za hodnotu, není proměnná. V důsledku toho nelze vytvářet jeho adresu. Podobně nelze vytvářet adresu konstanty.

`&` Operátor nevyžaduje, aby její argument představoval jednoznačně přiřazené, ale následující `&` operace, proměnné, ke které se použije operátor, který je považován za jednoznačně přiřazené v cestě provádění dojde k operaci. Je zodpovědností programátorovi, aby se ujistěte, že správné inicializace proměnné ve skutečnosti proběhnout v této situaci.

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

`i` je považován za jednoznačně přiřazené následující `&i` operace, která slouží k inicializaci `p`. Přiřazení `*p` platit inicializuje `i`, ale zahrnutí tato inicializace zodpovídá programátor a žádná chyba kompilace ke kterým by byl odebrán přiřazení.

Pravidla pro jednoznačného přiřazení `&` operátor existovat tak, že se můžete vyhnout redundantní inicializace místních proměnných. Například mnoho externí rozhraní API využít ukazatel na strukturu, která je vyplněna pomocí rozhraní API. Volání těchto rozhraní API, typicky pass adresy proměnné místní struktury a bez pravidla, by bylo zapotřebí redundantní inicializaci proměnné struktury.

### <a name="pointer-increment-and-decrement"></a>Ukazatel přírůstek a snížení

V nezabezpečeném kontextu `++` a `--` operátory ([Příponové operátory Inkrementace a dekrementace operátory](expressions.md#postfix-increment-and-decrement-operators) a [předpony Inkrementace a dekrementace operátory](expressions.md#prefix-increment-and-decrement-operators)) lze použít na ukazatel proměnné typů všechny s výjimkou `void*`. Díky tomu se pro každý typ ukazatele `T*`, implicitně jsou definovány následující operátory:

```csharp
T* operator ++(T* x);
T* operator --(T* x);
```

Operátory produkuje stejné výsledky jako `x + 1` a `x - 1`v uvedeném pořadí ([aritmetické operace ukazatele](unsafe-code.md#pointer-arithmetic)). Jinými slovy, pro proměnné ukazatele typu `T*`, `++` přidá operátor `sizeof(T)` na adrese obsažené v proměnné a `--` operátor odečte `sizeof(T)` v adrese obsažené v proměnné.

Pokud ukazatel zvýšení nebo snížení došlo k přetečení domény typ ukazatele, výsledkem je definováno implementací, ale se budou vytvářet žádné výjimky.

### <a name="pointer-arithmetic"></a>Aritmetika ukazatele

V nezabezpečeném kontextu `+` a `-` operátory ([operátor sčítání](expressions.md#addition-operator) a [operátor odčítání](expressions.md#subtraction-operator)) lze použít u hodnot všechny typy ukazatelů, s výjimkou `void*`. Díky tomu se pro každý typ ukazatele `T*`, implicitně jsou definovány následující operátory:

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

Zadaný výraz `P` typu ukazatele `T*` a výraz `N` typu `int`, `uint`, `long`, nebo `ulong`, výrazy `P + N` a `N + P` compute Hodnota ukazatele typu `T*` , která je výsledkem sečtení `N * sizeof(T)` adresu uvedenou ve `P`. Obdobně výraz `P - N` vypočítá hodnotu ukazatele typu `T*` , že výsledky daných `N * sizeof(T)` z adresy poskytnuté `P`.

Zadaný dvou výrazů `P` a `Q`, typu ukazatele `T*`, výraz `P - Q` vypočítá rozdíl mezi adresami zadanými `P` a `Q` a tento rozdíl vydělí `sizeof(T)`. Typ výsledku je vždy `long`. V důsledku toho `P - Q` je vypočítán jako `((long)(P) - (long)(Q)) / sizeof(T)`.

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

které vytvoří výstup:

```
p - q = -14
q - p = 14
```

Pokud Přetečení aritmetické operace ukazatele domény typ ukazatele, výsledek je oříznutá. podporuje definovanou implementací, ale se budou vytvářet žádné výjimky.

### <a name="pointer-comparison"></a>Porovnání ukazatelů

V nezabezpečeném kontextu `==`, `!=`, `<`, `>`, `<=`, a `=>` operátory ([relační a typové zkoušky operátory](expressions.md#relational-and-type-testing-operators)) lze použít u hodnot všech typy ukazatelů. Operátory porovnání ukazatele jsou:

```csharp
bool operator ==(void* x, void* y);
bool operator !=(void* x, void* y);
bool operator <(void* x, void* y);
bool operator >(void* x, void* y);
bool operator <=(void* x, void* y);
bool operator >=(void* x, void* y);
```

Vzhledem k tomu, že existuje implicitní převod z libovolného typu ukazatel na `void*` typ operandy libovolný typ ukazatele lze porovnat pomocí těchto operátorů. Relační operátory porovnávají adresy poskytnuté dva operandy, jako by byly celých čísel bez znaménka.

### <a name="the-sizeof-operator"></a>Sizeof – operátor

`sizeof` Operátor vrátí počet bajtů obsazena proměnnou daného typu. Typ určený jako operand `sizeof` musí být *unmanaged_type* ([typy ukazatelů](unsafe-code.md#pointer-types)).

```antlr
sizeof_expression
    : 'sizeof' '(' unmanaged_type ')'
    ;
```

Výsledkem `sizeof` operátor je hodnota typu `int`. Pro některé předdefinované typy, `sizeof` operátor vrací konstantní hodnoty, jak je znázorněno v následující tabulce.


| __Výraz__   | __výsledek__ |
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

Pro všechny ostatní typy, výsledek `sizeof` operátor je definováno implementací a je klasifikován jako hodnotu, není konstanta.

Pořadí, ve kterém jsou členy zkomprimována do struktury není zadána.

Pro účely zarovnání může nepojmenované odsazení na začátek struktury, v rámci struktury a na konci struktury. Obsah bity jako odsazení jsou neurčité.

Při použití na operand má typ struktura, výsledek je celkový počet bajtů v proměnnou daného typu, včetně žádné odsazení.

## <a name="the-fixed-statement"></a>Fixed – příkaz

V nezabezpečeném kontextu *embedded_statement* ([příkazy](statements.md)) produkční povoluje Další konstrukce, `fixed` příkaz, který se používá na "Opravit" přesunutelný proměnnou tak, aby jeho po dobu trvání příkaz konstantní adresu.

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

Každý *fixed_pointer_declarator* deklaruje místní proměnnou daný *pointer_type* a inicializuje tuto místní proměnnou s adresou počítají tak, že odpovídající *fixed_ pointer_initializer*. Místní proměnná deklarovaná ve `fixed` příkaz je dostupný v libovolném *fixed_pointer_initializer*s, ke kterým dochází k pravému deklarace danou proměnnou a v *embedded_statement* z `fixed` příkazu. Lokální proměnná deklarovaná příkazem `fixed` příkaz je považován za jen pro čtení. Chyba kompilace v případě vloženým příkazem se pokusí upravit tuto místní proměnnou (prostřednictvím přiřazení nebo `++` a `--` operátory) nebo předat ji jako `ref` nebo `out` parametru.

A *fixed_pointer_initializer* může být jedna z následujících akcí:

*  Token "`&`" následované *variable_reference* ([přesné pravidla pro určování jednoznačného přiřazení](variables.md#precise-rules-for-determining-definite-assignment)) přesunutelný proměnné ([Fixed a přesunutelný proměnné](unsafe-code.md#fixed-and-moveable-variables)) nespravovaný typ `T`, zadaný typ `T*` implicitně převést na typ ukazatele, který je uveden v `fixed` příkazu. V takovém případě inicializátoru vypočítá adresy dané proměnné a proměnná je zaručeno, že zůstanou na pevnou adresu po dobu trvání `fixed` příkazu.
*  Výraz *array_type* elementy nespravovaným typem `T`, zadaný typ `T*` implicitně převést na typ ukazatele, který je uveden v `fixed` příkazu. V takovém případě inicializátoru vypočítá adresu první prvek v poli, a je zaručeno, že celého pole zůstanou na pevnou adresu po dobu trvání `fixed` příkazu. Pokud výraz pole má hodnotu null nebo pole nemá nulovým počtem elementů, vypočítá inicializátoru adresu roven nule.
*  Výraz typu `string`, zadaný typ `char*` implicitně převést na typ ukazatele, který je uveden v `fixed` příkazu. V takovém případě inicializátoru vypočítá adresu prvního znaku v řetězci, a celý řetězec je zaručeno, že zůstanou na pevnou adresu po dobu trvání `fixed` příkazu. Chování `fixed` příkaz je definováno implementací pokud řetězcový výraz má hodnotu null.
*  A *simple_name* nebo *member_access* , která odkazuje na člen vyrovnávací paměti pevné velikosti přesunutelný proměnná, zadaný typ člena vyrovnávací paměti pevné velikosti je implicitně převést na zadaný typ ukazatele v `fixed` příkazu. V takovém případě inicializátoru vypočítá ukazatel na první prvek vyrovnávací paměti pevné velikosti ([ve výrazech pevnou velikost vyrovnávací paměti](unsafe-code.md#fixed-size-buffers-in-expressions)), a vyrovnávací paměti pevné velikosti je zaručeno, že zůstanou na pevnou adresu po dobu trvání `fixed`příkazu.

Pro každou adresu počítají tak, že *fixed_pointer_initializer* `fixed` prohlášení zajišťuje, že proměnná odkazuje na adresu není v souladu s přemístění nebo vyřazení pomocí systému uvolňování paměti po dobu trvání `fixed` příkazu. Například, pokud adresa počítají tak, že *fixed_pointer_initializer* odkazuje na pole objektu, nebo element pole instance `fixed` příkaz zaručuje, že není přemístění obsahující instanci objektu nebo odstraněny po celou dobu životnosti příkazu.

Je programátorovi povinností ujistit se, že ukazatele vytvořil `fixed` příkazy nepřežije nad rámec spuštění těchto příkazů. Například když ukazatele vytvořil `fixed` příkazy jsou předány do rozhraní API pro externí, zodpovídá programátor Ujistěte se, že rozhraní API pro zachování paměti těchto ukazatelů.

Oprava objekty může způsobit fragmentace haldy (protože nelze jej přesunout). Z tohoto důvodu byste opravit objekty pouze v případě, že je nezbytně nutné a pak jenom za nejkratší dobu nejvíce času.

V příkladu

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

ukazuje použití několika `fixed` příkazu. První příkaz oprav a získá adresu statické pole, druhý příkaz opravy a získá adresu pole instance a třetí příkaz oprav a získá adresu k elementu pole. V každém případě by byl chybně použit standardní `&` operátor vzhledem k tomu, že proměnné jsou klasifikovány jako přesunutelný proměnné.

Čtvrtý `fixed` podobného výsledku na třetí vytvoří příkaz v předchozím příkladu.

Tento příklad `fixed` používá příkaz `string`:

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

V nezabezpečeném kontextu pole prvků jednorozměrná pole jsou uloženy ve vzestupném pořadí index, počínaje indexem `0` a konče index `Length - 1`. Pro vícerozměrná pole, pole, které prvky jsou uloženy tak, aby se nejdřív zvyšují indexy rozměr nejvíce vpravo pak dimenze, na další a tak dále ponecháno na levé straně. V rámci `fixed` příkaz, který získá ukazatel `p` do pole instance `a`, od hodnoty ukazatele `p` k `p + a.Length - 1` představují adresy prvků v poli. Obdobně proměnné od `p[0]` k `p[a.Length - 1]` zastupují elementy skutečné pole. Zadaný způsob, ve kterém jsou uložené pole, jako by šlo lineární jsme lze považovat všechny dimenze pole.

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

které vytvoří výstup:

```
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

`fixed` prohlášení se používá k opravit pole, aby jeho adresy může být předán metodu, která přijímá ukazatel.

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

fixed – příkaz se používá k vyřešení vyrovnávací paměti pevné velikosti struktury tak jeho adresy může sloužit jako ukazatel.

A `char*` hodnotu vytvořený po opravě instanci řetězce, vždy odkazuje na řetězec zakončený hodnotou null. V rámci příkazu fixed, který získá ukazatel `p` instanci řetězec `s`, od hodnoty ukazatele `p` k `p + s.Length - 1` představují adresy znaků v řetězci a hodnota ukazatele `p + s.Length` vždy odkazuje na znak null (znak s hodnotou `'\0'`).

Změny objektů typu spravované prostřednictvím pevné odkazy můžete za následek nedefinované chování. Například protože řetězce jsou neměnné, je programátora povinností ujistit se, že znaky odkazuje ukazatel na řetězec pevné délky nezmění.

Automatické ukončení hodnotou null řetězců je obzvláště užitečná při volání externí rozhraní API, která očekávají řetězce "Ve stylu jazyka C". Všimněte si však, že smí obsahovat instanci řetězce obsahující znaky s hodnotou null. Pokud tyto znaky s hodnotou null, bude při považován za zakončený hodnotou null zobrazit ořezané řetězec `char*`.

## <a name="fixed-size-buffers"></a>Vyrovnávací paměti pevné velikosti

Vyrovnávací paměti pevné velikosti slouží k deklaraci pole v řádku "Stylu C" jako členy struktury a jsou užitečné hlavně pro propojení s nespravované rozhraní API.

### <a name="fixed-size-buffer-declarations"></a>Deklarace vyrovnávací paměti pevné velikosti

A ***pevné velikosti vyrovnávací paměti*** je člen, který představuje úložiště pro vyrovnávací paměť pevné délky proměnných daného typu. Deklarace vyrovnávací paměti pevné velikosti představuje jeden nebo více vyrovnávací paměti pevné velikosti typu daného elementu. Vyrovnávací paměti pevné velikosti jsou povolené jenom v deklaracích struktury a může dojít pouze v kontextu unsafe ([nezabezpečený kontext](unsafe-code.md#unsafe-contexts)).

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

Deklarace vyrovnávací paměti pevné velikosti může zahrnovat sadu atributů ([atributy](attributes.md)), `new` modifikátor ([modifikátory](classes.md#modifiers)), platnou kombinaci čtyři přístupu modifikátory přístupu ([typu parametry a omezením](classes.md#type-parameters-and-constraints)) a `unsafe` modifikátor ([nezabezpečený kontext](unsafe-code.md#unsafe-contexts)). Atributy a modifikátory platí pro všechny členy deklarované deklarací vyrovnávací paměti pevné velikosti. Jedná se o chybu pro stejný modifikátor objevit více než jednou v deklaraci vyrovnávací paměti pevné velikosti.

Deklarace vyrovnávací paměti pevné velikosti není dovoleno zahrnout `static` modifikátor.

Typ elementu deklaraci vyrovnávací paměti pevné velikosti vyrovnávací paměti určuje typ elementu vyrovnávací paměti zavedeným deklarací. Typ prvku vyrovnávací paměti musí být jeden z předdefinovaných typů `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, nebo `bool`.

Typ prvku vyrovnávací paměti následuje seznam deklarátorů vyrovnávací paměti pevné velikosti, z nichž každý představuje nového člena. Deklarátor vyrovnávací paměti pevné velikosti se skládá z identifikátor, který pojmenovává člen, za nímž následuje konstantní výraz uzavřený do `[` a `]` tokeny. Konstantní výraz označuje počet prvků v členu zavedené tento deklarátor vyrovnávací paměti pevné velikosti. Typ konstantního výrazu musí být implicitně převést na typ `int`, a hodnota musí být nenulové kladné celé číslo.

Je zaručeno, že prvky vyrovnávací paměti pevné velikosti rozloží postupně v paměti.

Deklarace vyrovnávací paměti pevné velikosti, která deklaruje několik vyrovnávací paměti pevné velikosti je ekvivalentní více deklarací deklaraci jeden pevné velikosti vyrovnávací paměti s stejné atributy a typy prvků. Příklad

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

Člen vyhledávání ([operátory](expressions.md#operators)) s pevnou velikostí vyrovnávací paměti člen pokračuje úplně stejně jako člen vyhledávací pole.

Vyrovnávací paměť pevné velikosti můžete odkazovat pomocí výrazu *simple_name* ([odvození typu](expressions.md#type-inference)) nebo *member_access* ([kompilace kontrolu řešení přetížení dynamické](expressions.md#compile-time-checking-of-dynamic-overload-resolution)).

Při odkazování na člena vyrovnávací paměti pevné velikosti jako jednoduchý název, efekt je stejný jako přístup ke členu formuláře `this.I`, kde `I` je člen vyrovnávací paměti pevné velikosti.

V přístupu ke členu formuláře `E.I`, pokud `E` je typu Struktura a člen vyhledávání `I` v tom, že typ struktury identifikuje členem pevné velikosti, pak `E.I` je vyhodnocen utajované následujícím způsobem:

*  Pokud výraz `E.I` nedojde v nezabezpečeném kontextu, dojde k chybě kompilace.
*  Pokud `E` je klasifikován jako hodnotu a dojde k chybě kompilace.
*  Jinak, pokud `E` přesunutelný proměnná ([Fixed a přesunutelný proměnné](unsafe-code.md#fixed-and-moveable-variables)) a výraz `E.I` není *fixed_pointer_initializer* ([pevné příkaz](unsafe-code.md#the-fixed-statement)), dojde k chybě kompilace.
*  V opačném případě `E` odkazuje pevná proměnná a výsledek výrazu je ukazatel na první prvek člen vyrovnávací paměti pevné velikosti `I` v `E`. Výsledek je typu `S*`, kde `S` je typ prvku `I`a je klasifikován jako hodnotu.

Následné prvky vyrovnávací paměti pevné velikosti lze přistupovat pomocí ukazatele operace od prvního prvku. Na rozdíl od přístup k polím přístup k prvkům vyrovnávací paměti pevné velikosti je nebezpečné operace a není zaškrtnuté políčko rozsahu.

Následující příklad deklaruje a používá strukturu se členem vyrovnávací paměti pevné velikosti.

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

### <a name="definite-assignment-checking"></a>Kontrola jednoznačného přiřazení

Vyrovnávací paměti pevné velikosti nejsou v souladu s Kontrola jednoznačného přiřazení ([jednoznačného přiřazení](variables.md#definite-assignment)), a členy vyrovnávací paměti pevné velikosti jsou ignorovány pro účely kontroly proměnné typu Struktura jednoznačného přiřazení.

Do vnějšího obsahující proměnné struktury člena vyrovnávací paměti pevné velikosti je statická proměnná, proměnnou instance instanci třídy nebo k elementu pole, prvky vyrovnávací paměti pevné velikosti jsou automaticky inicializovány na výchozí hodnoty ([Výchozí hodnoty](variables.md#default-values)). Ve všech ostatních případech původní obsah vyrovnávací paměti pevné velikosti není definováno.

## <a name="stack-allocation"></a>Přidělení zásobníku

V nezabezpečeném kontextu deklarace lokální proměnné ([místní deklarace proměnné](statements.md#local-variable-declarations)) může obsahovat inicializátor přidělení zásobníku, která přidělí paměť ze zásobníku volání.

```antlr
local_variable_initializer_unsafe
    : stackalloc_initializer
    ;

stackalloc_initializer
    : 'stackalloc' unmanaged_type '[' expression ']'
    ;
```

*Unmanaged_type* Určuje typ položky, které se uloží do nově přiděleného umístění a *výraz* označuje číslo, které z těchto položek. Společně tyto zadejte velikost požadované alokace. Protože velikost přidělení zásobníku nemůže být záporná, je chyba kompilace můžete určit počet položek jako *constant_expression* vyhodnocenou nečíselnou na zápornou hodnotu.

Inicializátoru přidělení zásobníku ve tvaru `stackalloc T[E]` vyžaduje `T` bude nespravovaným typem ([typy ukazatelů](unsafe-code.md#pointer-types)) a `E` bude výraz typu `int`. Konstrukce přiděluje `E * sizeof(T)` bajtů z volání zásobníku a vrátí ukazatel typu `T*`, do nově přiděleného bloku. Pokud `E` je hodnota záporná, pak chování není definováno. Pokud `E` je nula, pak je provedena bez přidělení a Vrácený ukazatel je definováno implementací. Pokud není k dispozici dostatek paměti k přidělení bloku určité velikosti, `System.StackOverflowException` je vyvolána výjimka.

Obsah nově přidělenou paměť není definován.

Inicializátory přidělení zásobníku nejsou povolené v `catch` nebo `finally` bloky ([příkazu try](statements.md#the-try-statement)).

Neexistuje žádný způsob, jak explicitně uvolnit paměť přidělena pomocí `stackalloc`. Po návratu této členské funkce automaticky zahodí všechny bloky paměti přiděleny vytvořené během provádění členské funkce. To odpovídá `alloca` funkce, rozšíření běžně vyskytují v implementace jazyka C a C++.

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

`stackalloc` inicializátoru je používán `IntToString` metoda přidělit vyrovnávací paměť v zásobníku 16 znaků. Vyrovnávací paměť se automaticky zruší po návratu metody.

## <a name="dynamic-memory-allocation"></a>Dynamické přidělení paměti

S výjimkou `stackalloc` operátor C# poskytuje žádné předdefinované konstrukce pro správu shromážděných paměti bez uvolnění paměti. Tyto služby jsou obvykle poskytuje podpůrné knihovny tříd nebo importovat přímo od základního operačního systému. Například `Memory` třídy níže ukazuje, jak funkce haldy základního operačního systému by mohly být přístupné z C#:

```csharp
using System;
using System.Runtime.InteropServices;

public unsafe class Memory
{
    // Handle for the process heap. This handle is used in all calls to the
    // HeapXXX APIs in the methods below.
    static int ph = GetProcessHeap();

    // Private instance constructor to prevent instantiation.
    private Memory() {}

    // Allocates a memory block of the given size. The allocated memory is
    // automatically initialized to zero.
    public static void* Alloc(int size) {
        void* result = HeapAlloc(ph, HEAP_ZERO_MEMORY, size);
        if (result == null) throw new OutOfMemoryException();
        return result;
    }

    // Copies count bytes from src to dst. The source and destination
    // blocks are permitted to overlap.
    public static void Copy(void* src, void* dst, int count) {
        byte* ps = (byte*)src;
        byte* pd = (byte*)dst;
        if (ps > pd) {
            for (; count != 0; count--) *pd++ = *ps++;
        }
        else if (ps < pd) {
            for (ps += count, pd += count; count != 0; count--) *--pd = *--ps;
        }
    }

    // Frees a memory block.
    public static void Free(void* block) {
        if (!HeapFree(ph, 0, block)) throw new InvalidOperationException();
    }

    // Re-allocates a memory block. If the reallocation request is for a
    // larger size, the additional region of memory is automatically
    // initialized to zero.
    public static void* ReAlloc(void* block, int size) {
        void* result = HeapReAlloc(ph, HEAP_ZERO_MEMORY, block, size);
        if (result == null) throw new OutOfMemoryException();
        return result;
    }

    // Returns the size of a memory block.
    public static int SizeOf(void* block) {
        int result = HeapSize(ph, 0, block);
        if (result == -1) throw new InvalidOperationException();
        return result;
    }

    // Heap API flags
    const int HEAP_ZERO_MEMORY = 0x00000008;

    // Heap API functions
    [DllImport("kernel32")]
    static extern int GetProcessHeap();

    [DllImport("kernel32")]
    static extern void* HeapAlloc(int hHeap, int flags, int size);

    [DllImport("kernel32")]
    static extern bool HeapFree(int hHeap, int flags, void* block);

    [DllImport("kernel32")]
    static extern void* HeapReAlloc(int hHeap, int flags, void* block, int size);

    [DllImport("kernel32")]
    static extern int HeapSize(int hHeap, int flags, void* block);
}
```

Příklad, který se používá `Memory` třídy jsou vypsáni níže:

```csharp
class Test
{
    static void Main() {
        unsafe {
            byte* buffer = (byte*)Memory.Alloc(256);
            try {
                for (int i = 0; i < 256; i++) buffer[i] = (byte)i;
                byte[] array = new byte[256];
                fixed (byte* p = array) Memory.Copy(buffer, p, 256); 
            }
            finally {
                Memory.Free(buffer);
            }
            for (int i = 0; i < 256; i++) Console.WriteLine(array[i]);
        }
    }
}
```

V příkladu se přiděluje 256 bajtů paměti prostřednictvím `Memory.Alloc` a inicializuje blok paměti s hodnotami zvýšení od 0 do 255. Potom přiděluje pole bajtů 256 element a používá `Memory.Copy` chcete zkopírovat obsah bloku paměti do bajtového pole. Nakonec blok paměti je uvolněna pomocí `Memory.Free` a obsah bajtové pole je výstup na konzole.
