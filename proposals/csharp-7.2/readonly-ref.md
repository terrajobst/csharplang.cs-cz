---
ms.openlocfilehash: 39fb0aab5e8bb0d422f25fd2e92ab3d8256d3f59
ms.sourcegitcommit: b8f1103eb686c5d82e294837c9386d9b667da292
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/29/2019
ms.locfileid: "79485110"
---
# <a name="readonly-references"></a>Odkazy jen pro čtení

* Navrženo [x]
* [x] prototyp
* [x] implementace: spuštěno
* [] Specifikace: Nezahájeno

## <a name="summary"></a>Souhrn
[summary]: #summary

Funkce "odkazy jen pro čtení" je ve skutečnosti skupina funkcí, které využívají efektivitu předávání proměnných podle odkazu, ale bez odhalení dat do úprav:  
- parametry `in`
- `ref readonly` vrátí
- `readonly` struktury
- metody rozšíření `ref`/`in`
- `ref readonly` národní prostředí
- `ref` podmíněné výrazy

## <a name="passing-arguments-as-readonly-references"></a>Předávání argumentů jako odkazů jen pro čtení.

K dispozici je existující návrh, který se dotýká tohoto tématu https://github.com/dotnet/roslyn/issues/115 jako speciální případ parametrů ReadOnly, aniž by došlo k mnoha podrobnostem.
Tady bych chtěl potvrdit, že nápad sám o sobě není příliš nový.

### <a name="motivation"></a>Motivační

Před touto funkcí C# neexistoval účinný způsob, jak vyjádřit potřebu předat proměnné struktury do volání metody pro účely ReadOnly bez úmyslné úpravy. Hodnota argumentu Regular by pass zahrnuje kopírování, které přidává nepotřebné náklady.  Který řídí, aby uživatelé použili argument-REF předávající a spoléhají na komentáře/dokumentaci, aby označovali, že volaný by data neměla být ovlivněna. Nejedná se o dobré řešení z mnoha důvodů.  
Příklady jsou řady-Vector/matic Math Operators v grafických knihovnách, jako je například rozhraní [XNA](https://msdn.microsoft.com/library/bb194944.aspx) , se nazývají referenční operandy čistě z důvodů výkonu. V samotném kompilátoru Roslyn existuje kód, který používá struktury k zamezení přidělení a pak je předává odkazům, aby nedocházelo ke kopírování nákladů.

### <a name="solution-in-parameters"></a>Řešení (`in` parametry)

Podobně jako u parametrů `out` jsou `in` parametry předány jako spravované odkazy s dalšími zárukami od volaného.  
Na rozdíl od parametrů `out`, které _musí_ před jakýmkoli jiným použitím přiřazovat volaný, `in` parametry nelze přiřadit volaným.

V důsledku toho `in` parametry umožňují efektivitu předávání nepřímých argumentů bez vystavení argumentů pro mutace volanými.

### <a name="declaring-in-parameters"></a>Deklarace `in`ch parametrů

parametry `in` jsou deklarovány pomocí klíčového slova `in` jako modifikátor v podpisu parametru.

Pro všechny účely se parametr `in` považuje za `readonly` proměnnou. Většina omezení pro použití parametrů `in` v rámci metody je stejná jako u polí `readonly`.

> Ve skutečnosti může představovat parametr `in` `readonly` pole. Podobnost omezení nepředstavuje kodopad.

Například pole parametru `in`, který má typ struktury, jsou rekurzivně klasifikovány jako `readonly` proměnné.

```csharp
static Vector3 Add (in Vector3 v1, in Vector3 v2)
{
    // not OK!!
    v1 = default(Vector3);

    // not OK!!
    v1.X = 0;

    // not OK!!
    foo(ref v1.X);

    // OK
    return new Vector3(v1.X + v2.X, v1.Y + v2.Y, v1.Z + v2.Z);
}
```

- parametry `in` jsou povoleny kdekoli, kde jsou povoleny běžné parametry ByVal. Patří sem indexery, operátory (včetně převodů), delegáti, výrazy lambda, místní funkce.

> ```csharp
>  (in int x) => x                                                     // lambda expression  
>  TValue this[in TKey index];                                         // indexer
>  public static Vector3 operator +(in Vector3 x, in Vector3 y) => ... // operator
>  ```

- `in` není povolený v kombinaci s `out` nebo s cokoli, na které `out` nekombinuje.

- Nepovoluje se přetížení na `ref`/`out`/ch `in`ch rozdílech.

- Je povoleno přetížit na běžných a `in`ch rozdílech.

- Pro účely OHI (přetížení, skrytí, implementace) se `in` chová podobně jako `out` parametr.
Platí všechna stejná pravidla.
Například přepsání metody bude muset odpovídat parametrům `in` s parametry `in` typu, který je převoditelné pomocí identity.

- Pro účely převodů skupin Delegate/lambda/Method se `in` chová podobně jako `out` parametr.
Výrazy lambda a použitelné převody skupin metod budou muset odpovídat `in` parametrům cílového delegáta s `in` parametry typu, který se dá převést na identitu.

- Pro účely obecné odchylky jsou parametry `in` nevariantní.

> Poznámka: nejsou k dispozici žádná upozornění na parametry `in`, které mají typ odkazu nebo primitivní typy.
Může být obecně bezúčelné, ale v některých případech musí uživatel nebo chtít předat primitivní prvky jako `in`. Příklady – přepsání obecné metody, jako je například `Method(in T param)`, když `T` nahradila `int`nebo když mají metody jako `Volatile.Read(in int location)`
>
> Je možné mít analyzátor, který upozorňuje v případech neefektivního použití parametrů `in`, ale pravidla pro takovou analýzu by byla příliš Přibližná jako součást specifikace jazyka.

### <a name="use-of-in-at-call-sites-in-arguments"></a>Použití `in` na webech volání. (`in` argumenty)

Existují dva způsoby, jak předat argumenty `in`m parametrům.

#### <a name="in-arguments-can-match-in-parameters"></a>argumenty `in` můžou odpovídat parametrům `in`:

Argument s modifikátorem `in` na webu volání může odpovídat parametrům `in`.

```csharp
int x = 1;

void M1<T>(in T x)
{
  // . . .
}

var x = M1(in x);  // in argument to a method

class D
{
    public string this[in Guid index];
}

D dictionary = . . . ;
var y = dictionary[in Guid.Empty]; // in argument to an indexer
```

- Argument `in` musí být _čitelný_ LValue (*).
Příklad: `M1(in 42)` je neplatný

> (*) Pojem [l-hodnota/rvalue](https://en.wikipedia.org/wiki/Value_(computer_science)#lrvalue) se liší mezi jazyky.  
V tomto případě je v důsledku hodnoty LValue I výraz, který představuje umístění, na které lze odkazovat přímo.
A RValue označuje výraz, který poskytuje dočasný výsledek, který není trvale trvalý.  

- Konkrétně je platný pro předání `readonly` polí, `in` parametrů nebo jiných formálně `readonly` proměnných jako argumentů `in`.
Příklad: `dictionary[in Guid.Empty]` je právní. `Guid.Empty` je statické pole jen pro čtení.

- `in` argument musí mít typ parametru, který lze _převést_ na typ parametru.
Příklad: `M1<object>(in Guid.Empty)` je neplatný. `Guid.Empty` není _převoditelné ze identity_ na `object`

Motivace výše uvedených pravidel je taková, že `in` argumenty zaručují _alias_ proměnné argumentu. Volaný vždy obdrží přímý odkaz na stejné umístění, které je reprezentované argumentem.

- v případech, kdy je `in` argumentů nutné přecházet z zásobníku z důvodu `await` výrazů, které jsou použity jako operandy stejného volání, je chování stejné jako u `out` a `ref`ch argumentů – Pokud proměnná nemůže být začínat způsobem transparentním způsobem, je hlášena chyba.

Příklady:
1. `M1(in staticField, await SomethingAsync())` je platný.
`staticField` je statické pole, ke kterému lze přistupovat více než jednou bez pozorovatelných vedlejších účinků. Proto lze zadat jak pořadí vedlejších účinků, tak i požadavky na alias.
2. `M1(in RefReturningMethod(), await SomethingAsync())` vytvoří chybu.
`RefReturningMethod()` je metoda, kterou vrací `ref`. Volání metody může mít pozorovatelící vedlejší účinky, proto musí být vyhodnocen před `SomethingAsync()` operandem. Výsledek vyvolání je však odkaz, který nemůže být zachován přes `await` bod pozastavení, který neumožňuje požadavek na přímý odkaz.

> Poznámka: Chyby při zakrývání zásobníku se považují za omezení specifická pro konkrétní implementaci. Proto nemají vliv na rozlišení přetěžování ani odvození výrazu lambda.

#### <a name="ordinary-byval-arguments-can-match-in-parameters"></a>Běžné argumenty ByVal můžou odpovídat parametrům `in`:

Parametry `in` nemohou odpovídat regulárním argumentům bez modifikátorů. V takovém případě mají argumenty stejné odlehčené omezení jako běžné argumenty ByVal.

Motivací pro tento scénář je, že `in` parametry v rozhraních API mohou mít za následek nepohodlí pro uživatele, pokud argumenty nelze předat jako přímý odkaz-ex: literály, vypočítané nebo výsledky `await`-Ed nebo argumenty, které se mají považovat za konkrétnější typy.  
Všechny tyto případy mají triviální řešení ukládání hodnoty argumentu do dočasného lokálního typu a předání tohoto místního jako argumentu `in`.  
Aby nedocházelo k tomu, že tento často používaný kompilátor kódu, může v případě potřeby provádět stejnou transformaci, když `in` modifikátor není přítomen na webu volání.  

V některých případech, jako jsou například vyvolání operátorů nebo metody rozšíření `in`, neexistuje žádná syntaktická možnost zadat `in` vůbec. Tato samostatně vyžaduje určení chování běžných argumentů ByVal, pokud odpovídají `in` parametrům.

Zejména jde o toto:

- předání rvalue je neplatné.
V takovém případě se předává odkaz na dočasnou hodnotu.
Příklad:
```csharp
Print("hello");      // not an error.

void Print<T>(in T x)
{
  //. . .
}
```

- implicitní převody jsou povoleny.

> To je vlastně zvláštní případ předávání RValue.  

V takovém případě se předává odkaz na dočasný podíl převedený na hodnotu.
Příklad:
```csharp
Print<int>(Short.MaxValue)     // not an error.
```

- v případě přijímače metody rozšíření `in` (na rozdíl od `ref` metod rozšíření) jsou povoleny rvalue nebo implicitní _převody tohoto argumentu_ .
V takovém případě se předává odkaz na dočasný podíl převedený na hodnotu.
Příklad:
```csharp
public static IEnumerable<T> Concat<T>(in this (IEnumerable<T>, IEnumerable<T>) arg)  => . . .;

("aa", "bb").Concat<char>()    // not an error.
```
Další informace o `ref`/`in` metody rozšíření jsou k dispozici dále v tomto dokumentu.

- vynechání argumentu z důvodu `await` operandů by v případě potřeby mohlo proložení "podle hodnoty".
Ve scénářích, kde není možné zadat přímý odkaz na argument, není možné, protože se jedná o `await` kopie hodnoty argumentu je mimo místo.  
Příklad:
```csharp
M1(RefReturningMethod(), await SomethingAsync())   // not an error.
```
Vzhledem k tomu, že výsledek vedlejšího vyvolání je odkaz, který nelze zachovat v rámci `await`ho pozastavení, bude místo toho uložen dočasný obsah obsahující skutečnou hodnotu (jako by to byl běžný případ parametrů ByVal).

#### <a name="omitted-optional-arguments"></a>Vynechané nepovinné argumenty

Je povoleno, aby parametr `in` určil výchozí hodnotu. Který nastaví odpovídající argument jako volitelný.

Vynechání volitelného argumentu na webu volání má za následek předání výchozí hodnoty přes dočasné.

```csharp
Print("hello");      // not an error, same as
Print("hello", c: Color.Black);

void Print(string s, in Color c = Color.Black)
{
    // . . .
}
```

### <a name="aliasing-behavior-in-general"></a>Obecné chování při vytváření aliasů

Stejně jako proměnné `ref` a `out`, `in` proměnné jsou odkazy nebo aliasy na existující umístění.

I když volaný není povolen k zápisu do nich, čtení `in` parametr může sledovat různé hodnoty jako vedlejší účinek jiných hodnocení.

Příklad:

```C#
static Vector3 v = Vector3.UnitY;

static void Main()
{
    Test(v);
}

static void Test(in Vector3 v1)
{
    Debug.Assert(v1 == Vector3.UnitY);
    // changes v1 deterministically (no races required)
    ChangeV();
    Debug.Assert(v1 == Vector3.UnitX);
}

static void ChangeV()
{
    v = Vector3.UnitX;
}
```

### <a name="in-parameters-and-capturing-of-local-variables"></a>`in` parametry a zachytávání místních proměnných.  
Pro účely zachycení lambda nebo asynchronního zachytávání `in` parametry se chovají stejně jako parametry `out` a `ref`.

- parametry `in` se nedají zachytit v uzávěrce.
- parametry `in` nejsou povolené v metodách iterátoru.
- parametry `in` nejsou povolené v asynchronních metodách.

### <a name="temporary-variables"></a>Dočasné proměnné.  
Některé použití parametrů `in` předávání může vyžadovat nepřímé použití dočasné místní proměnné:  
- argumenty `in` jsou vždy předány jako přímé aliasy, když se používá web Call `in`. V takovém případě se v takovém případě nikdy nepoužívá.
- argumenty `in` nejsou vyžadovány pro přímé aliasy, pokud volání-web nepoužívá `in`. Pokud argument není l-hodnota, lze použít dočasné.
- parametr `in` může mít výchozí hodnotu. Pokud je odpovídající argument vynechán na webu volání, je výchozí hodnota předána prostřednictvím dočasné.
- argumenty `in` můžou mít implicitní převody, včetně těch, které nezachovávají identitu. V těchto případech se používá dočasný.
- přijímače běžných volání struktur nemůžou být zapisovatelné hodnoty lvalue (**existující případ!** ). V těchto případech se používá dočasný.

Doba života argumentu dočasné objekty odpovídá nejbližšímu rozsahu, který zahrnuje rozsah volání – Web.

Formální doba života dočasných proměnných je sémanticky významná ve scénářích, které se týkají řídicích analýz proměnných vrácených odkazem.

### <a name="metadata-representation-of-in-parameters"></a>Reprezentace metadat `in` parametrů
Při použití `System.Runtime.CompilerServices.IsReadOnlyAttribute` pro parametr ByRef to znamená, že parametr je `in` parametr.

Kromě toho, pokud je metoda *abstraktní* nebo *virtuální*, musí mít signatura takových parametrů (a pouze takové parametry) `modreq[System.Runtime.InteropServices.InAttribute]`.

**Motivace**: Tato akce zajistí, že v případě metody přepisu nebo implementace parametrů `in` se shodují.

Stejné požadavky platí pro `Invoke` metody v delegátech.

**Motivace**: je potřeba zajistit, aby existující kompilátory při vytváření nebo přiřazování delegátů nemohly jednoduše ignorovat `readonly`.

## <a name="returning-by-readonly-reference"></a>Vrací se odkazem jen pro čtení.

### <a name="motivation"></a>Motivační
Motivace této dílčí funkce je přibližně symetricky k důvodům pro parametry `in` – zamezení kopírování, ale na návratové straně. Před touto funkcí byly v metodě nebo indexeru dvě možnosti: 1) vrácení odkazem a zpřístupnění možných mutací nebo 2) vrátí hodnotu, která má za následek kopírování.

### <a name="solution-ref-readonly-returns"></a>Řešení (`ref readonly` vrátí)  
Funkce umožňuje členovi vracet proměnné podle odkazu bez jejich vystavení pro mutace.

### <a name="declaring-ref-readonly-returning-members"></a>Deklarace `ref readonly` vracející členy

Kombinace modifikátorů `ref readonly` pro návratový podpis slouží k označení toho, že člen vrací odkaz jen pro čtení.

Pro všechny účely se `ref readonly` člen považuje za `readonly` proměnnou, podobně jako `readonly` pole a `in` parametry.

Například pole člena `ref readonly`, který má typ struktury, jsou rekurzivně klasifikována jako `readonly` proměnné. – Je povoleno je předat jako argumenty `in`, ale ne jako argumenty `ref` nebo `out`.

```csharp
ref readonly Guid Method1()
{
}

Method2(in Method1()); // valid. Can pass as `in` argument.

Method3(ref Method1()); // not valid. Cannot pass as `ref` argument
```

- na stejných místech jsou povolené `ref readonly` vrácení `ref` jsou povolené.
Patří sem indexery, delegáti, výrazy lambda a místní funkce.

- Není povoleno přetížení v `ref`/`ref readonly`/rozdíly.

- Je povoleno přetížení na běžných ByVal a `ref readonly` vrácení rozdílů.

- Pro účely OHI (přetížení, skrytí, implementace) `ref readonly` je podobná, ale liší se od `ref`.
Například metoda a, která přepisuje `ref readonly` jeden, musí být `ref readonly` a musí být typu, který je převoditelný identitou.

- Pro účely převodů skupin Delegate/lambda nebo method je `ref readonly` podobná, ale liší se od `ref`.
Výrazy lambda a příslušné metody převodu skupiny metod musí odpovídat `ref readonly` vrácení cílového delegáta s `ref readonly` vrácení typu, který je převoditelnou identitou.

- Pro účely obecné odchylky je `ref readonly` vrácení nevariantní.

> Poznámka: k dispozici nejsou žádná upozornění na `ref readonly` vrátí, která mají typ odkazu nebo primitivní typy.
Může být obecně bezúčelné, ale v některých případech musí uživatel nebo chtít předat primitivní prvky jako `in`. Příklady – přepsání obecné metody, jako je `ref readonly T Method()`, když `T` nahradila `int`.
>
>Je možné mít analyzátor, který upozorňuje v případech neefektivního použití `ref readonly` vrátí, ale pravidla pro takovou analýzu by byla příliš Přibližná jako součást specifikace jazyka.

### <a name="returning-from-ref-readonly-members"></a>Vracení z `ref readonly`ch členů
V těle metody je syntaxe stejná jako s běžným návratovým argumentem. `readonly` bude odvozena z nadřazené metody.

Motivace je taková, že `return ref readonly <expression>` není zbytečný a umožňuje pouze neshoda na `readonly` část, která by vždycky vedla k chybám.
`ref` se ale vyžaduje pro konzistenci s jinými scénáři, kdy je něco předáno pomocí striktního aliasu vs. podle hodnoty.

> Na rozdíl od případu s parametry `in` `ref readonly` vrátí nikdy návrat prostřednictvím místní kopie. Vzhledem k tomu, že by se kopírování po vrácení takového postupu v případě, že by tento postup zastavilo, bezúčelné a nebezpečné. Proto `ref readonly` vrátí vždy přímé odkazy.

Příklad:

```csharp
struct ImmutableArray<T>
{
    private readonly T[] array;

    public ref readonly T ItemRef(int i)
    {
        // returning a readonly reference to an array element
        return ref this.array[i];
    }
}

```

- Argumentem `return ref` musí být l-hodnota (**stávající pravidlo**).
- Argument `return ref` musí být "bezpečné pro návrat" (**stávající pravidlo**).
- V členu `ref readonly` není _nutné zapisovat_ do argumentu `return ref`.
Například tento člen může vracet REF pole jen pro čtení nebo jeden z jeho `in` parametrů.

### <a name="safe-to-return-rules"></a>V bezpečí můžete vracet pravidla.
Normální zabezpečení pro návratová pravidla pro odkazy se vztahují také na odkazy jen pro čtení.

Všimněte si, že `ref readonly` lze získat z obyčejné `ref` lokální/parametr/Return, ale nikoli jiným způsobem. V opačném případě je zabezpečení `ref readonly`ch vrácení odvozeno stejným způsobem jako u regulárních `ref`ch vrácených.

Vzhledem k tomu, že rvalue se dá předat jako parametr `in` a vrátí se jako `ref readonly` potřebujeme ještě jedno další pravidlo – **rvalue se nedá bezpečně vrátit odkazem**.

> Zvažte situaci, kdy se RValue předává do parametru `in` prostřednictvím kopie a pak se vrátí zpět ve formě `ref readonly`. V kontextu volajícího je výsledkem takového vyvolání odkaz na místní data, protože to není bezpečné.
> Jakmile rvalue není bezpečné vracet, stávající pravidlo `#6` už tento případ zpracovává.

Příklad:
```csharp
ref readonly Vector3 Test1()
{
    // can pass an RValue as "in" (via a temp copy)
    // but the result is not safe to return
    // because the RValue argument was not safe to return by reference
    return ref Test2(default(Vector3));
}

ref readonly Vector3 Test2(in Vector3 r)
{
    // this is ok, r is returnable
    return ref r;
}
```

Aktualizované `safe to return` pravidla:

1.  **odkazy na proměnné na haldě se bezpečně vracejí**
2.  **parametry ref/in jsou bezpečné pro návrat**
`in` parametrů přirozeně se dají vracet jenom jako jen pro čtení.
3.  **výstupní parametry se dají bezpečně vrátit** (ale musí se jednoznačně přiřadit, protože už dnes platí).
4.  **pole struktury instancí je bezpečné, aby se vracely, dokud se příjemce bezpečně vrátí.**
5.  **možnost this není bezpečná pro návrat ze členů struktury.**
6.  **odkaz, vrácený z jiné metody, se může bezpečně vrátit, pokud všechny odkazy/objekty předané této metodě jako formální parametry byly bezpečně vraceny.** 
*konkrétně není důležité, pokud se příjemce může bezpečně vrátit bez ohledu na to, jestli je příjemce strukturou, třídou nebo typem jako parametr obecného typu.*
7.  **Rvalue nelze bezpečně vrátit odkazem.** 
*konkrétně rvalue je bezpečné předat jako parametry.*

> Poznámka: existují další pravidla týkající se bezpečnosti návratů, která přicházejí do hry, když jsou zapojeny typy s odkazem na typ ref a změna počtu odkazů.
> Pravidla platí stejně pro `ref` a `ref readonly` členy, a proto zde nejsou zmíněny.

### <a name="aliasing-behavior"></a>Chování při vytváření aliasů.
`ref readonly` členové poskytují stejné chování při vytváření aliasů jako běžné `ref` členy (s výjimkou nejenom pro čtení).
Proto pro účely zachycení výrazů lambda, asynchronní, iterátory, překrývání zásobníku atd... platí stejná omezení. t. z důvodu neschopnosti zachytit skutečné odkazy a z důvodu vedlejších účinků pro vyhodnocení členů takové scénáře nejsou povoleny.

> Je povolen a vyžadován pro vytvoření kopie, když `ref readonly` návrat je přijímačem běžných metod struktury, které `this` jako standardní zapisovatelný odkaz. Historicky ve všech případech, kde jsou tato volání použita pro proměnnou jen pro čtení, je vytvořena místní kopie.

### <a name="metadata-representation"></a>Reprezentace metadat
Pokud je použita `System.Runtime.CompilerServices.IsReadOnlyAttribute` na vrácení návratové metody ByRef, znamená to, že metoda vrátí odkaz jen pro čtení.

Kromě toho musí mít signatura výsledku takových metod (a pouze tyto metody) `modreq[System.Runtime.CompilerServices.IsReadOnlyAttribute]`.

**Motivace**: je potřeba zajistit, aby existující kompilátory nemohly ignorovat `readonly` při volání metod s `ref readonly`.

## <a name="readonly-structs"></a>Struktury jen pro čtení
Stručná funkce, která vytváří `this` parametr všech členů instance struktury, s výjimkou konstruktorů, `in` parametr.

### <a name="motivation"></a>Motivační
Kompilátor musí předpokládat, že jakékoli volání metody v instanci struktury může změnit instanci. Do metody jako `this` parametr je ve skutečnosti předán zapisovatelný odkaz, který toto chování plně povoluje. Chcete-li takové vyvolání pro proměnné `readonly` použít, jsou volání použita pro dočasné kopie. To může být neintuitivní a někdy nutí uživatele opustit `readonly` z důvodů výkonu.  
Příklad: https://codeblog.jonskeet.uk/2014/07/16/micro-optimization-the-surprising-inefficiency-of-readonly-fields/

Po přidání podpory pro parametry `in` a `ref readonly` vrátí problém s kopírováním obrannou linií, protože proměnné jen pro čtení se budou běžnější.

### <a name="solution"></a>Řešení
Povolí modifikátor `readonly` v deklaracích struktury, což by vedlo k tomu, že `this` zpracováván jako `in` parametr u všech metod instance struktury s výjimkou konstruktorů.

```csharp
static void Test(in Vector3 v1)
{
    // no need to make a copy of v1 since Vector3 is a readonly struct
    System.Console.WriteLine(v1.ToString());
}

readonly struct Vector3
{
    . . .

    public override string ToString()
    {
        // not OK!!  `this` is an `in` parameter
        foo(ref this.X);

        // OK
        return $"X: {X}, Y: {Y}, Z: {Z}";
    }
}
```

### <a name="restrictions-on-members-of-readonly-struct"></a>Omezení členů struktury jen pro čtení
- Pole instance struktury jen pro čtení musí být jen pro čtení.  
**Motivace:** lze zapisovat pouze externě, ale ne prostřednictvím členů.
- Instance autoproperties struktury jen pro čtení musí být jen pro získání.  
**Motivace:** důsledek omezení pro pole instance.
- Struktura ReadOnly nemůže deklarovat události podobné poli.  
**Motivace:** důsledek omezení pro pole instance.

### <a name="metadata-representation"></a>Reprezentace metadat
Pokud je použita `System.Runtime.CompilerServices.IsReadOnlyAttribute` pro typ hodnoty, znamená to, že typ je `readonly struct`.

Zejména jde o toto:
-  Identita typu `IsReadOnlyAttribute` je neimportovaná. Ve skutečnosti může být vložen kompilátorem v nadřazeném sestavení v případě potřeby.

## <a name="refin-extension-methods"></a>metody rozšíření `ref`/`in`
Ve skutečnosti existuje stávající návrh (https://github.com/dotnet/roslyn/issues/165) a odpovídající prototyp žádosti o přijetí změn (https://github.com/dotnet/roslyn/pull/15650).
Chci jenom potvrdit, že tento nápad není zcela nový. Je však zde relevantní, protože `ref readonly` elegantním způsobem odebírá contentious problém těchto metod – co dělat s přijímači RValue.

Obecný nápad umožňuje rozšiřujícím metodám přebírat parametr `this` odkazem, pokud je známý jako typ struktury.

```csharp
public static void Extension(ref this Guid self)
{
    // do something
}
```

Důvody pro zápis takových rozšiřujících metod jsou primárně:  
1.  Vyhněte se kopírováním, když je přijímač velkou strukturou
2.  Povolení metod rozšíření pro struktury

Důvody, proč nechci tuto třídu u tříd povolit  
1.  Má velmi omezený účel.
2.  Došlo by k přerušení dlouhého stálého nevarianty, kterou volání metody nemůže zapnout, aby se ne`null` příjemce po vyvolání `null`.
> Ve skutečnosti se v současné době ne`null` proměnná nestane `null`, pokud _explicitně_ nepřiřazuje nebo nepředává `ref` nebo `out`.
> To může významně vyhodnotit čitelnost nebo jiné formy "může to být tato" hodnota "null".
3.  Bylo by těžké sjednotit se sémantikou "vyhodnotit jednou" pro podmíněné přístupy s hodnotou null.
Příklad: `obj.stringField?.RefExtension(...)` – je třeba zachytit kopii `stringField`, aby byla hodnota null smysluplná, ale přiřazení `this` uvnitř RefExtension se neprojeví zpět do pole.

Schopnost deklarovat metody rozšíření ve **strukturách** , které přijímají první argument odkazem, byl dlouhotrvající požadavek. Jedním z aspektů blokování bylo "co se stane, když příjemce není LValue?".

- Existuje předchůdce, který může být také volán jako statická metoda (někdy jde o jediný způsob, jak vyřešit nejednoznačnost). Mělo by to být, že přijímače RValue by měly být nepovolené.
- Na druhé straně existuje postup, jak vyvolat volání při kopírování v podobných situacích, kdy jsou zapojeny metody instance struktury.

Důvod, proč existuje implicitní kopírování, je, protože většina metod struktury ve skutečnosti nemění strukturu, ale nedokáže ji indikovat. Z toho důvodu by bylo praktické řešení pouze udělat při vyvolání kopie, ale tento postup je znám pro poškození výkonu a způsobuje chyby.

Nyní s dostupností parametrů `in` je možné, že rozšíření signalizuje záměr. Conundrum je proto možné vyřešit vyžadováním rozšíření `ref`, která mají být volána s zapisovatelnou přijímači, zatímco rozšíření `in` povolují v případě potřeby implicitní kopírování.

```csharp
// this can be called on either RValue or an LValue
public static void Reader(in this Guid self)
{
    // do something nonmutating.
    WriteLine(self == default(Guid));
}

// this can be called only on an LValue
public static void Mutator(ref this Guid self)
{
    // can mutate self
    self = new Guid();
}
```

### <a name="in-extensions-and-generics"></a>`in` rozšíření a obecné typy.
Účelem `ref`ch rozšiřujících metod je použití přijímače přímo nebo vyvoláním obdobných členů. Proto jsou rozšíření `ref this T` povolena, pokud je `T` omezené na strukturu.

Na druhé straně `in` rozšiřující metody existují specificky pro omezení implicitního kopírování. Nicméně jakékoli použití parametru `in T` bude nutné provést prostřednictvím členu rozhraní. Vzhledem k tomu, že všechny členy rozhraní jsou považovány za mutace, takové použití by vyžadovalo kopii. – Místo zmenšení kopírování by tento efekt byl opakem. Proto `in this T` není povoleno, pokud `T` je parametr obecného typu bez ohledu na omezení.

### <a name="valid-kinds-of-extension-methods-recap"></a>Platný druh rozšiřujících metod (rekapitulace):
Nyní jsou povoleny následující formuláře deklarace `this` v metodě rozšíření:
1) `this T arg` – regulární rozšíření ByVal (**existující případ**)
- T může být jakýkoli typ, včetně typů odkazů nebo parametrů typu.
Instance bude po volání stejnou proměnnou.
Povoluje implicitní převody _tohoto typu konverze argumentu_ .
Lze ji volat v rvalue.

- `in this T self` - `in` rozšíření.
T musí být skutečným typem struktury.
Instance bude po volání stejnou proměnnou.
Povoluje implicitní převody _tohoto typu konverze argumentu_ .
Může být volána v rvalue (v případě potřeby může být vyvolána v dočasném případě).

- `ref this T self` - `ref` rozšíření.
T musí být typ struktury nebo parametr obecného typu, který je omezený na strukturu.
Instance může být zapsána voláním.
Povoluje pouze převody identity.
Musí být volána pro zapisovatelnou LValue. (nikdy se nevolá přes Temp).

## <a name="readonly-ref-locals"></a>Lokální hodnoty REF ReadOnly.

### <a name="motivation"></a>Motivační.
Po zavlečení `ref readonly` členů bylo jasné, že se nepoužily k párování s vhodným druhem místní. Vyhodnocení člena může vytvořit nebo sledovat vedlejší účinky, proto pokud by výsledek měl být použit více než jednou, je nutné jej uložit. Běžné `ref` národní prostředí vám k tomu nemůžeme, protože jim nelze přiřadit odkaz na `readonly`.   

### <a name="solution"></a>Řešení.
Umožňuje deklarovat `ref readonly` národní prostředí. Toto je nový druh `ref`ch národních prostředí, která nelze zapsat. V důsledku `ref readonly` národní prostředí mohou přijímat odkazy na proměnné jen pro čtení bez vystavení těchto proměnných pro zápisy.

### <a name="declaring-and-using-ref-readonly-locals"></a>Deklarování a používání `ref readonly`ch lokálních hodnot.

Syntaxe takových lokálních hodnot používá modifikátory `ref readonly` v lokalitě deklarací (v tomto konkrétním pořadí). Podobně jako u běžných `ref`ch lokálních hodnot musí být `ref readonly` lokálních hodnot inicializovány v deklaraci. Na rozdíl od běžných `ref` místních hodnot můžou `ref readonly` místních hodnot odkazovat na `readonly` hodnoty lvalue, jako jsou `in` parametry, `readonly`ch polí, `ref readonly`ch metod.

Pro všechny účely se jako `readonly` proměnná považuje `ref readonly` Local. Většina omezení pro použití je stejná jako u `readonly` polí nebo `in` parametrů.

Například pole parametru `in`, který má typ struktury, jsou rekurzivně klasifikovány jako `readonly` proměnné.   

```csharp
static readonly ref Vector3 M1() => . . .

static readonly ref Vector3 M1_Trace()
{
    // OK
    ref readonly var r1 = ref M1();

    // Not valid. Need an LValue
    ref readonly Vector3 r2 = ref default(Vector3);

    // Not valid. r1 is readonly.
    Mutate(ref r1);

    // OK.
    Print(in r1);

    // OK.
    return ref r1;
}
```

### <a name="restrictions-on-use-of-ref-readonly-locals"></a>Omezení použití `ref readonly`ch národních prostředí
S výjimkou jejich `readonly` povahy se `ref readonly` místních se chovají jako běžné `ref` národní prostředí a řídí se přesně stejnými omezeními.  
Například omezení týkající se zachycení v uzávěrech, deklarace v `async`ch metod nebo `safe-to-return` analýzy se rovnoměrně vztahují na `ref readonly` národní prostředí.

## <a name="ternary-ref-expressions-aka-conditional-lvalues"></a>Ternární `ref` výrazy (neboli "podmíněný hodnoty lvalue")

### <a name="motivation"></a>Motivační
Použití `ref` a `ref readonly`, která se zveřejňují, je nutné, aby taková národní prostředí byla inicializována s jednou nebo jinou cílovou proměnnou na základě podmínky.

Typickým řešením je zavést metodu jako:

```csharp
ref T Choice(bool condition, ref T consequence, ref T alternative)
{
    if (condition)
    {
         return ref consequence;
    }
    else
    {
         return ref alternative;
    }
}
```

Všimněte si, že `Choice` není přesným nahrazením Ternární, protože _všechny_ argumenty musí být vyhodnoceny na webu volání, což vedlo k neintuitivnímu chování a chybám.

Následující nebudou fungovat podle očekávání:

```csharp
    // will crash with NRE because 'arr[0]' will be executed unconditionally
    ref var r = ref Choice(arr != null, ref arr[0], ref otherArr[0]);
```

### <a name="solution"></a>Řešení
Povolí zvláštní druh podmíněného výrazu, který se vyhodnotí jako odkaz na jeden z argumentů LValue na základě podmínky.

### <a name="using-ref-ternary-expression"></a>Použití výrazu `ref` Ternární

Syntaxe `ref`ho charakteru podmíněného výrazu je `<condition> ? ref <consequence> : ref <alternative>;`

Stejně jako u běžného podmíněného výrazu `<consequence>` nebo `<alternative>` vyhodnocen v závislosti na výsledku výrazu logické podmínky.

Na rozdíl od obyčejného podmíněného výrazu `ref` podmíněný výraz:
- vyžaduje, aby se `<consequence>` a `<alternative>` hodnoty lvalue.
- `ref` samotného podmíněného výrazu je l-hodnota a
- `ref` podmíněný výraz je zapisovatelný, pokud jsou `<consequence>` i `<alternative>` zapisovatelné hodnoty lvalue

Příklady:  
`ref` Ternární je l-hodnota a jako taková může být předána/přiřazena/vrácena odkazem;
```csharp
     // pass by reference
     foo(ref (arr != null ? ref arr[0]: ref otherArr[0]));

     // return by reference
     return ref (arr != null ? ref arr[0]: ref otherArr[0]);
```

Je-li to hodnota l-hodnota, lze ji také přiřadit.
```csharp
     // assign to
     (arr != null ? ref arr[0]: ref otherArr[0]) = 1;

     // error. readOnlyField is readonly and thus conditional expression is readonly
     (arr != null ? ref arr[0]: ref obj.readOnlyField) = 1;
```

Dá se použít jako přijímač volání metody a pokud je to potřeba, můžete v případě potřeby vynechat kopírování.
```csharp
     // no copies
     (arr != null ? ref arr[0]: ref otherArr[0]).StructMethod();

     // invoked on a copy.
     // The receiver is `readonly` because readOnlyField is readonly.
     (arr != null ? ref arr[0]: ref obj.readOnlyField).StructMethod();

     // no copies. `ReadonlyStructMethod` is a method on a `readonly` struct
     // and can be invoked directly on a readonly receiver
     (arr != null ? ref arr[0]: ref obj.readOnlyField).ReadonlyStructMethod();
```

`ref` Ternární lze použít také v běžném (nikoli ref) kontextu.
```csharp
     // only an example
     // a regular ternary could work here just the same
     int x = (arr != null ? ref arr[0]: ref otherArr[0]);
```

### <a name="drawbacks"></a>Nevýhody
[drawbacks]: #drawbacks

Vidím dva hlavní argumenty s rozšířenou podporou pro odkazy a odkazy jen pro čtení:

1) Problémy, které jsou zde vyřešeny, jsou velmi staré. Proč je teď už náhle vyřešte, zejména proto, že by vám nepomohl existující kód?

Jak jsme našli C# a používali .NET v nových doménách, některé problémy se výrazně projeví.  
Jako příklady prostředí, která jsou z hlediska výpočetních režijních nákladů důležitější než průměr, můžu seznam

* scénáře cloudu nebo Datacenter, kde se účtují výpočty a reakce, je konkurenční výhoda.
* Hry/VR/AR s požadavky tichého v reálném čase na latenci     

Tato funkce nebrání žádné z existujících pevností, jako je například bezpečnost typů, a přitom umožňuje nižší režijní náklady v některých běžných scénářích.

2) Můžeme rozumně zaručit, že volaný bude při výslovnýí kontraktů `readonly` do smlouvy přehrajte pravidla?

Při použití `out`máme podobný vztah důvěryhodnosti. Nesprávná implementace `out` může způsobit nespecifikované chování, ale ve skutečnosti to nastane jenom zřídka.  

Provádění formálních ověřovacích pravidel, která jsou známá `ref readonly`, by dále zmírnily problém důvěryhodnosti.

### <a name="alternatives"></a>Alternativy
[alternatives]: #alternatives

Hlavní konkurenční návrh je skutečně "nedělat nic".

### <a name="unresolved-questions"></a>Nevyřešené dotazy
[unresolved]: #unresolved-questions

### <a name="design-meetings"></a>Schůzky pro návrh

https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-02-22.md https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-03-01.md https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-08-28.md https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-09-25.md https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-09-27.md
