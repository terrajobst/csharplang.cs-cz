---
ms.openlocfilehash: 6088a5cd41926d828013f1b8e5736fd2b7939e44
ms.sourcegitcommit: da452002c3f472165a0e1fa7759f494cc703ae31
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/27/2019
ms.locfileid: "79485040"
---
# <a name="compile-time-enforcement-of-safety-for-ref-like-types"></a>Doba kompilace vynucování zabezpečení pro typy s odkazem na typ odkazu

## <a name="introduction"></a>Úvod

Hlavním důvodem pro další bezpečnostní pravidla při práci s typy jako `Span<T>` a `ReadOnlySpan<T>` je, že tyto typy musí být omezeny na zásobník spouštění.
 
Existují dva důvody, proč `Span<T>` a podobné typy musí být typy pouze zásobníku.

1. `Span<T>` je sémanticky strukturou obsahující odkaz a rozsah-`(ref T data, int length)`. Bez ohledu na skutečnou implementaci by zápis do takové struktury nepředstavoval atomii. Souběžná odtrhlina této struktury by vedla k tomu, že `length` neodpovídají `data`, což způsobuje přístup mimo rozsah a narušení bezpečnosti typů, což by nakonec mohlo vést k poškození haldy GC v zdánlivě bezpečném kódu.
2. Některé implementace `Span<T>` doslova obsahují spravovaný ukazatel v jednom z jeho polí. Spravované ukazatele nejsou podporovány jako pole objektů haldy a kódu, který spravuje za účelem vložení spravovaného ukazatele na haldu GC obvykle k chybě v době běhu.

Pokud jsou instance `Span<T>` omezené na existenci pouze zásobníku spuštění, budou všechny výše uvedené problémy zmírnit. 

K dalšímu problému dojde z důvodu složení. Obecně je žádoucí vytvořit složitější datové typy, které by mohly vkládat `Span<T>` a `ReadOnlySpan<T>` instance. Takové složené typy by musely být struktury a měly by sdílet všechna rizika a požadavky `Span<T>`. V důsledku toho by se tady popsaná pravidla zabezpečení měla zobrazit v celém rozsahu **_typů podobných typu ref_** .

[Specifikace jazyka konceptu](#draft-language-specification) je určena k zajištění toho, aby hodnoty typu odkazu byly pouze v zásobníku.

## <a name="generalized-ref-like-types-in-source-code"></a>Zobecněné typy `ref-like` ve zdrojovém kódu

`ref-like` struktury jsou explicitně označeny ve zdrojovém kódu pomocí modifikátoru `ref`:

```csharp
ref struct TwoSpans<T>
{
    // can have ref-like instance fields
    public Span<T> first;
    public Span<T> second;
} 

// error: arrays of ref-like types are not allowed. 
TwoSpans<T>[] arr = null;

```

Určení struktury jako ref jako umožní, aby struktura měla pole instance s odkazem, a také všechny požadavky na typy s odkazem, které se vztahují na strukturu. 

## <a name="metadata-representation-or-ref-like-structs"></a>Reprezentace metadat nebo struktury podobné odkazům

Struktury podobné odkazu budou označeny atributem **System. Runtime. CompilerServices. IsRefLikeAttribute** .

Atribut se přidá do běžných základních knihoven, jako je `mscorlib`. V případě, že atribut není k dispozici, kompilátor vygeneruje interní, podobně jako jiné atributy Embedded-na vyžádání, jako je například `IsReadOnlyAttribute`.

Bude provedena další míra, která brání použití struktur typu ref v kompilátorech, které nejsou obeznámeny s pravidly zabezpečení (zahrnuje C# kompilátory před rozhraním, ve kterém je tato funkce implementována). 

Neexistují žádné další vhodné alternativy, které fungují ve starších kompilátorech bez obsluhy, atribut `Obsolete` se známým řetězcem bude přidán do všech struktur podobně jako referenčních. Kompilátory, které vědí, jak použít typy ref, budou tuto konkrétní formu `Obsolete`ignorovat.

Typická reprezentace metadat:

```csharp
    [IsRefLike]
    [Obsolete("Types with embedded references are not supported in this version of your compiler.")]
    public struct TwoSpans<T>
    {
       // . . . .
    }
```

Poznámka: není to cílem, aby to bylo možné provést, protože jakékoli použití typů s odkazem na staré kompilátory nefunguje 100%. To je obtížné dosáhnout a není bezpodmínečně nutné. Například by byl vždy způsob, jak získat `Obsolete` pomocí dynamického kódu, nebo například vytváření pole typů s odkazem pomocí reflexe.

Zejména pokud chce uživatel skutečně umístit atribut `Obsolete` nebo `Deprecated` na typ podobný odkazem, nebudete mít žádnou volbu, než vygenerujeme předdefinovanou hodnotu, protože atribut `Obsolete` nelze použít více než jednou.  

## <a name="examples"></a>Příklady:

```csharp
SpanLikeType M1(ref SpanLikeType x, Span<byte> y)
{
    // this is all valid, unconcerned with stack-referring stuff
    var local = new SpanLikeType(y);
    x = local;
    return x;
}

void Test1(ref SpanLikeType param1, Span<byte> param2)
{
    Span<byte> stackReferring1 = stackalloc byte[10];
    var stackReferring2 = new SpanLikeType(stackReferring1);

    // this is allowed
    stackReferring2 = M1(ref stackReferring2, stackReferring1);

    // this is NOT allowed
    stackReferring2 = M1(ref param1, stackReferring1);

    // this is NOT allowed
    param1 = M1(ref stackReferring2, stackReferring1);

    // this is NOT allowed
    param2 = stackReferring1.Slice(10);

    // this is allowed
    param1 = new SpanLikeType(param2);

    // this is allowed
    stackReferring2 = param1;
}

ref SpanLikeType M2(ref SpanLikeType x)
{
    return ref x;
}

ref SpanLikeType Test2(ref SpanLikeType param1, Span<byte> param2)
{
    Span<byte> stackReferring1 = stackalloc byte[10];
    var stackReferring2 = new SpanLikeType(stackReferring1);

    ref var stackReferring3 = M2(ref stackReferring2);

    // this is allowed
    stackReferring3 = M1(ref stackReferring2, stackReferring1);

    // this is allowed
    M2(ref stackReferring3) = stackReferring2;

    // this is NOT allowed
    M1(ref param1) = stackReferring2;

    // this is NOT allowed
    param1 = stackReferring3;

    // this is NOT allowed
    return ref stackReferring3;

    // this is allowed
    return ref param1;
}

```

----------------

## <a name="draft-language-specification"></a>Specifikace jazyka konceptu

Níže popisujeme sadu pravidel zabezpečení pro typy s odkazem (`ref struct`), aby se zajistilo, že hodnoty těchto typů nastávají pouze v zásobníku. Jiná, jednodušší sada pravidel zabezpečení by byla možná, pokud národní prostředí nelze předat odkazem. Tato specifikace by také umožnila bezpečné opětovné přiřazení místních hodnot ref.

### <a name="overview"></a>Přehled

K jednotlivým výrazům přiřadíme v době kompilace koncept toho, s jakým rozsahem je povolený příkaz k úniku a "bezpečnému řídicímu panelu". Podobně platí, že pro každou lvalue udržujeme koncept, na který je odkaz, na který se vztahuje odkaz, ke kterému se dá odkazovat pomocí příkazu "ref-Safe-to-Escape". Pro daný výraz l-hodnoty se mohou lišit.

Jedná se o podobnou funkci "bezpečného vrácení" lokálních hodnot typu ref, ale je přesnější. Místo toho, aby se "bezpečné" vrácené položky v rámci výrazu zavedly pouze bez ohledu na to, zda (nebo ne) může být řídicí Metoda uvozena jako celek, záznamy typu bezpečného k úniku, které mají rozsah Základní bezpečnostní mechanismus se vynutil následujícím způsobem. Vzhledem k přiřazení z výrazu E1 s oborem typu "bezpečného", s rozsahem "S1", výrazem (lvalue), který je typu Safe-to-Escape S2, se jedná o chybu, pokud je S2 širší rozsah než S1. Podle konstrukce jsou dva obory S1 a S2 v relaci vnořování, protože právní výraz je vždy bezpečný – vrácen z nějakého oboru ohraničujícího výraz.

V době, kdy je to dostačující pro účely analýzy, pro podporu pouze dvou oborů – externě na metodu a rozsahu nejvyšší úrovně metody. Důvodem je, že hodnoty typu ref s vnitřními obory nelze vytvořit a lokální hodnoty REF nepodporují opakované přiřazení. Tato pravidla však mohou podporovat více než dvě úrovně rozsahu.

Přesná pravidla pro výpočet stavu *bezpečných a vrácených* výrazů a pravidla upravující legalitu výrazů sledují.

### <a name="ref-safe-to-escape"></a>ref-Safe-to-Escape

Typ *ref-Safe-to-Escape* je obor, ohraničující výraz l-hodnota, na který je bezpečné pro odkaz na lvalue pro Escape. Pokud je tento obor celou metodou, říkáme, že odkaz na lvalue je *bezpečný pro návrat* z metody.

### <a name="safe-to-escape"></a>bezpečné – řídicí

Typ *Safe-to-Escape* je obor, ve kterém je uveden výraz, na který je bezpečná hodnota pro únik hodnoty. Pokud je tento obor celou metodou, říkáme, že hodnotu lze *bezpečně vrátit* z metody.

Výraz, jehož typ není typu `ref struct`, je *bezpečným návratem* z celé nadřazené metody. V opačném případě odkazujeme na níže uvedená pravidla.

#### <a name="parameters"></a>Parametry

L-hodnota určující formální parametr je typu *ref-Safe-to-Escape* (odkazem) následujícím způsobem:
- Pokud je parametr `ref`, `out`nebo `in` parametr, jedná se o *bezpečný* přístup z celé metody (např. pomocí příkazu `return ref`); případech
- Je-li parametr `this` parametrem struktury, jedná se o *bezpečný* přístup z oboru na nejvyšší úrovni metody (ale ne z celé samotné metody); [Ukázka](#struct-this-escape)
- V opačném případě parametr je parametr hodnoty a jedná se o *bezpečný přístup k* hlavnímu panelu na nejvyšší úrovni metody (ale ne z samotné metody).

Výraz, který je hodnotou rvalue určující použití formálního parametru, je z celé metody (například pomocí příkazu `return`) *zabezpečený řídicí* znak (podle hodnoty). To platí i pro parametr `this`.

#### <a name="locals"></a>Místní hodnoty

L-hodnota označující místní proměnnou je *ref-Safe-to-Escape* (odkazem) následujícím způsobem:
- Pokud proměnná je `ref` proměnná, pak je její *bezpečný-na řídicí* znak z jeho inicializačního výrazu převedena na číslo, které je bezpečné – bez *odkazu* ; případech
- Proměnná je typově *bezpečná-to-Escape* oboru, ve kterém byla deklarována.

Výraz, který je hodnotou rvalue označující použití lokální proměnné, je *zabezpečený řídicím* znakem (podle hodnoty) následujícím způsobem:
- Ale obecné pravidlo, které je výše, místní, jehož typ není `ref struct` typ je *bezpečný – návrat* z celé nadřazené metody.
- Pokud je proměnná proměnnou iterace `foreach` smyčky, je rozsah " *bezpečného* " ovládacího prvku proměnné stejný jako *bezpečný-řídící* znak výrazu `foreach` smyčky.
- Místní typ `ref struct` a neinicializované v bodě deklarace je *bezpečným návratem* z celé nadřazené metody.
- V opačném případě je typ proměnné `ref struct` typ a deklarace proměnné vyžaduje inicializátor. Rozsah *bezpečného řídicího* panelu proměnné je stejný jako *bezpečný-řídicí* znak jeho inicializátoru.

#### <a name="field-reference"></a>Odkaz na pole

L-hodnota označující odkaz na pole, `e.F`, je *ref-Safe-to-Escape* (odkazem) následujícím způsobem:
- Pokud je `e` typu odkazu, jedná se o *bezpečný přístup k znaku* z celé metody; případech
- Pokud `e` je hodnotový typ, jeho *ref-to-* Escape je pořízeno z `e`typu " *bezpečného* " bez odkazů.

Rvalue označující odkaz na pole, `e.F`, má rozsah *bezpečných uvozovacích znaků* , který je stejný jako *bezpečný-řídicí* znak `e`.

#### <a name="operators-including-"></a>Operátory, včetně `?:`

Aplikace uživatelsky definovaného operátoru je považována za vyvolání metody.

U operátoru, který má za následek rvalue, jako je například `e1 + e2` nebo `c ? e1 : e2`, je *bezpečného řídicího* panelu výsledku nejužším oborem v rámci *bezpečného řídicího* panelu operandů operátoru.  Jako důsledek pro unární operátor, který poskytuje rvalue, jako je například `+e`, *bezpečné-řídicí znaky* výsledku je " *bezpečné* " řídicí znaky operandu.

Pro operátor, který vrací hodnotu lvalue, například `c ? ref e1 : ref e2`
- *argument REF-to-Escape* výsledku je nejužším oborem v rámci typu ref- *to-Escape* operandů operátoru.
- *bezpečné řídicí znaky* operandů musí souhlasit a to je *bezpečné-řídicí znaky* výsledné hodnoty lvalue.

#### <a name="method-invocation"></a>Vyvolání metody

L-hodnota, která je výsledkem volání metody vracené odkazem, `e1.M(e2, ...)` je typově *bezpečná-to-Escape* nejnižší z následujících rozsahů:
- Celá ohraničující metoda
- *bezpečné-řídicí znaky* všech výrazů argumentu `ref` a `out` (kromě přijímače)
- Pro každý parametr `in` metody, pokud je k dispozici odpovídající výraz, který je l-hodnota, jeho *ref-to-Escape*, jinak nejbližší nadřazený rozsah
- *bezpečné-řídicí znaky* všech výrazů argumentů (včetně přijímače)

> Poznámka: poslední odrážka je nutná ke zpracování kódu, jako např.
> ```csharp
> var sp = new Span(...)
> return ref sp[0];
> ```
> nebo
> ```csharp
> return ref M(sp, 0);
> ```

Hodnota rvalue, která je výsledkem volání metody `e1.M(e2, ...)`, je *bezpečná-to-Escape* od nejmenšího z následujících oborů:
- Celá ohraničující metoda
- *bezpečné-řídicí znaky* všech výrazů argumentů (včetně přijímače)

#### <a name="an-rvalue"></a>Rvalue
Rvalue je z nejbližšího nadřazeného oboru *zabezpečený proti odkazům na řídicí znaky* . K tomu dochází například při vyvolání, například `M(ref d.Length)`, kde `d` je typu `dynamic`. Je také konzistentní s (a případně subsumes) našeho zpracování argumentů odpovídajících parametrům `in`. *

#### <a name="property-invocations"></a>Vyvolání vlastností

Vyvolání vlastnosti (buď `get`, nebo `set`) se považuje za metody vyvolání podkladové metody výše uvedenými pravidly.

#### `stackalloc`

Výraz stackalloc je rvalue, který je *bezpečný-k úniku* do rozsahu nejvyšší úrovně metody (ale ne z celé samotné metody).

#### <a name="constructor-invocations"></a>Vyvolání konstruktoru

Výraz `new`, který vyvolá konstruktor, dodržuje stejná pravidla jako volání metody, která je považována za to, že vrací typ, který je vytvořen.

V případě *bezpečného* připojení není širší než nejmenší z *bezpečných řídicích znaků* všech argumentů a operandů výrazů inicializátoru objektu, rekurzivně, pokud je k dispozici inicializátor. 

#### <a name="span-constructor"></a>Span – konstruktor
Jazyk spoléhá na `Span<T>` nemá konstruktor následujícího tvaru:

```csharp
void Example(ref int x)
{
    // Create a span of length one
    var span = new Span<int>(ref x); 
}
```

Takový konstruktor zpřístupňuje `Span<T>`, které se používají jako pole Nerozlišená z pole `ref`. Bezpečnostní pravidla popsaná v tomto dokumentu závisí na `ref` polích, která nejsou platnou konstrukcí v C#nebo .NET.

#### <a name="default-expressions"></a>výrazy `default`

Výraz `default` je z celé nadřazené metody *bezpečný-na řídicí* znak.

## <a name="language-constraints"></a>Jazyková omezení

Chceme zajistit, aby žádná `ref` místní proměnná a žádná proměnná typu `ref struct` neodkazovala na paměť zásobníku nebo proměnné, které už nejsou aktivní. Proto máme následující omezení jazyka:

- Ani parametr ref ani místní ani parametr ani parametr ani parametr ani lokální typ `ref struct` nelze převolat do lambda nebo místní funkce.

- Parametr ref ani parametr `ref struct` typu nemůže být argumentem metody iterátoru nebo metody `async`.

- Lokální odkaz ani místní typ `ref struct` nesmí být v rozsahu v místě příkazu `yield return` nebo výrazu `await`.

- Typ `ref struct` nesmí být použit jako argument typu nebo jako typ prvku v typu řazené kolekce členů.

- Typ `ref struct` nemůže být deklarovaným typem pole, s tím rozdílem, že se může jednat o deklarovaný typ pole instance jiného `ref struct`.

- Typ `ref struct` nesmí být typem elementu pole.

- Hodnota typu `ref struct` nesmí být zabalená:
  - Neexistuje žádný převod typu `ref struct` na `object` typu nebo na typ `System.ValueType`.
  - Typ `ref struct` nemůže být deklarovaný pro implementaci libovolného rozhraní.
  - Žádná metoda instance deklarovaná v `object` ani v `System.ValueType`, ale není přepsána v `ref struct` typu, může být volána s příjemcem daného `ref struct` typu.
  - Převodem metody na typ delegáta nemůže být zachycena žádná metoda instance typu `ref struct`.

- Pro `ref e1 = ref e2`pro změnu přiřazení ref musí být `e2` s označením *ref-to-Escape* přinejmenším v rámci rozsahu, který je *bezpečný pro přístup k referenčnímu znaku* `e1`.

- Pro příkaz ref Return `return ref e1`musí být `e1` typu *ref-to-Escape* z celé metody bezpečný na *řídicí* znak. (TODO: budeme také potřebovat pravidlo, které `e1` musí být *bezpečné-k Escape* z celé metody, nebo jestli je redundantní?)

- V případě návratového příkazu `return e1`*musí být* *bezpečná-to-Escape* `e1` z celé metody.

- U `e1 = e2`přiřazení, pokud typ `e1` je `ref struct` typ, pak musí být *bezpečná-řídicí sekvence* `e2` aspoň jako síť s *bezpečným únikem* `e1`.

- Pro vyvolání metody, pokud existuje argument `ref` nebo `out` `ref struct`ho typu (včetně přijímače) s *bezpečným řídicím* znakem E1, nemůže mít žádný argument (včetně přijímače) užší *bezpečný přístup k řídicímu* panelu než E1. [Ukázka](#method-arguments-must-match)

- Lokální funkce nebo anonymní funkce nesmí odkazovat na místní nebo parametr typu `ref struct` deklarovaného v ohraničujícím oboru.

> ***Otevřít problém:*** Potřebujeme nějaké pravidlo, které nám umožní vytvořit chybu, když se bude potřebovat překládat hodnoty zásobníku `ref struct`ho typu na výraz await, například v kódu.
> ```csharp
> Foo(new Span<int>(...), await e2);
> ```

## <a name="explanations"></a>Odůvodněn
Tato vysvětlení a ukázky vám pomůžou vysvětlit, proč existuje celá řada výše uvedených pravidel zabezpečení.

### <a name="method-arguments-must-match"></a>Argumenty metody se musí shodovat.
Při vyvolání metody, kde je `out`, `ref` parametr, který je `ref struct`, včetně přijímače, musí mít všechny `ref struct` stejnou dobu života. To je nezbytné, C# protože musí učinit všechna rozhodnutí o tom, že se jedná o bezpečnost životního cyklu na základě informací dostupných v signatuře metody a životnosti hodnot na webu volání. 

Pokud existují `ref` parametry, které jsou `ref struct` pak je reálnou možnost, že by mohly prohodit obsah. Proto je na webu volání nutné zajistit, aby všechny tyto **případné** swapy byly kompatibilní. Pokud jazyk vynutil, pak bude umožňovat špatný kód podobný následujícímu.

```csharp
void M1(ref Span<int> s1)
{
    Span<int> s2 = stackalloc int[1];
    Swap(ref s1, ref s2);
}

void Swap(ref Span<int> x, ref int Span<int> y)
{
    // This will effectively assign the stackalloc to the s1 parameter and allow it
    // to escape to the caller of M1
    ref x = ref y; 
}
```

Omezení na přijímači je nezbytné, protože Přestože žádný z jeho obsahu není bezpečný pro přístup z více důvodů, může uložit zadané hodnoty. To znamená, že se neshodnou životností můžete vytvořit bezpečnostní otvor typu následujícím způsobem:

```csharp
ref struct S
{
    public Span<int> Span;

    public void Set(Span<int> span)
    {
        Span = span;
    }
}

void Broken(ref S s)
{
    Span<int> span = stackalloc int[1];

    // The result of a stackalloc is now stored in s.Span and escaped to the caller
    // of Broken
    s.Set(span); 
}
```

### <a name="struct-this-escape"></a>Strukturovat tento řídicí znak
Pokud je v rozsahu pravidla zabezpečení, hodnota `this` v členu instance je modelovaná jako parametr členu. Nyní `struct` typ `this` je ve skutečnosti `ref S`, kde `class` je jednoduše `S` (pro členy `class / struct` s názvem S). 

Zatím `this` má odlišná pravidla uvozovacích parametrů než jiné parametry `ref`. Konkrétně se nejedná o bezpečný a řídicí znak, zatímco jiné parametry jsou:

```csharp
ref struct S
{ 
    int Field;

    // Illegal because this isn't safe to escape as ref
    ref int Get() => ref Field;

    // Legal
    ref int GetParam(ref int p) => ref p;
}
```

Důvodem pro toto omezení je fakt, že se při vyvolání `struct` členů něco udělat. Existují některá pravidla, která je potřeba odpracovat s ohledem na vyvolání členů v `struct`ch členech, u kterých je příjemce rvalue. Ale to je velmi obtížné. 

Důvod tohoto omezení je ve skutečnosti o vyvolání rozhraní. Konkrétně se jedná o to, zda by následující vzorek neměl nebo neměl být zkompilován;

```csharp
interface I1
{
    ref int Get();
}

ref int Use<T>(T p)
    where T : I1
{
    return ref p.Get();
}
```

Vezměte v úvahu případ, kdy je `T` vytvořena instance jako `struct`. Pokud je parametr `this` zabezpečený proti odkazu, pak vrácení `p.Get` může ukazovat na zásobník (konkrétně se může jednat o pole v instanci typu `T`). To znamená, že jazyk nemůže způsobit, že by se tato ukázka mohla kompilovat, protože by mohla vracet `ref` do umístění zásobníku. Na druhé straně, pokud `this` není bezpečné-pro-Escape a pak `p.Get` nemůže odkazovat na zásobník, a proto je bezpečné ho vrátit. 

Důvodem je, že uvozovací znaky `this` v `struct` jsou opravdu všechny o rozhraních. Je možné, že je to pro práci naprosto, ale má na trhu volno. Návrh nakonec vychází z přístupnosti flexibilního vytváření rozhraní. 

I když je to možné, můžeme to zmírnit i v budoucnu. 

## <a name="future-considerations"></a>Budoucí požadavky

### <a name="length-one-spant-over-ref-values"></a>Délku jednoho rozsahu\<T > přes ref Values
I když v současné době ještě není, existují případy, kdy by bylo možné vytvořit délku jedné instance `Span<T>` instance s hodnotou by měla být přínosná:

```csharp
void RefExample()
{
    int x = ...;

    // Today creating a length one Span<int> requires a stackalloc and a new 
    // local
    Span<int> span1 = stackalloc [] { x };
    Use(span1);
    x = span1[0]; 

    // Simpler to just allow length one span
    var span2 = new Span<int>(ref x);
    Use(span2);
}
```

Tato funkce získá přísnější omezení, Pokud zavedeme omezení na [vyrovnávací paměti s pevnou velikostí](https://github.com/dotnet/csharplang/blob/master/proposals/fixed-sized-buffers.md) , protože by to mohlo způsobit `Span<T>` instancí ještě větší. 

Pokud je někdy potřeba přejít k této cestě, může to tento jazyk zajišťovat tím, že zajistí, že se tyto instance `Span<T>` jenom dolů. To znamená, že se pouze někdy jednalo o *bezpečnou cestu* k oboru, ve kterém byly vytvořeny. To zajistí, že jazyk nikdy neměl považovat `ref` hodnotu uvozovací metodu pomocí `ref struct` návrat nebo pole `ref struct`. To by mohlo také vyžadovat další změny pro rozpoznání takových konstruktorů, jako zachycení `ref` parametr tímto způsobem.
