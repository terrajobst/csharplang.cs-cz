---
ms.openlocfilehash: 50f2bd2d0a84064cfe35fe65b9e5c59c052d19ac
ms.sourcegitcommit: 1dbb8e82bed5012a58a3a035bf2c3737ed570d07
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/24/2020
ms.locfileid: "80122952"
---
# <a name="ranges"></a>Rozsahy

## <a name="summary"></a>Souhrn

Tato funkce se týká poskytování dvou nových operátorů, které umožňují vytvářet objekty `System.Index` a `System.Range` a používat je k indexování a řezů kolekcí za běhu.

## <a name="overview"></a>Přehled

### <a name="well-known-types-and-members"></a>Dobře známé typy a členy

Chcete-li použít nové syntaktické formy pro `System.Index` a `System.Range`, mohou být v závislosti na tom, jaké syntaktické tvary použity, pravděpodobně nezbytné nové známé typy a členy.

Chcete-li použít operátor "Hat" (`^`), je nutné zadat následující

```csharp
namespace System
{
    public readonly struct Index
    {
        public Index(int value, bool fromEnd);
    }
}
```

Chcete-li použít typ `System.Index` jako argument v přístupu k elementu pole, je požadován následující člen:

```csharp
int System.Index.GetOffset(int length);
```

Syntaxe `..` pro `System.Range` bude vyžadovat typ `System.Range` a také jeden nebo více následujících členů:

```csharp
namespace System
{
    public readonly struct Range
    {
        public Range(System.Index start, System.Index end);
        public static Range StartAt(System.Index start);
        public static Range EndAt(System.Index end);
        public static Range All { get; }
    }
}
```

Syntaxe `..` umožňuje buď, nebo žádné argumenty, které nemají chybět. Bez ohledu na počet argumentů je konstruktor `Range` vždy dostačující pro použití syntaxe `Range`. Pokud je však přítomen některý z ostatních členů a chybí jeden nebo více argumentů `..`, může být příslušný člen nahrazen.

Nakonec pro hodnotu typu `System.Range` pro použití ve výrazu přístupu k elementu pole musí být k dispozici následující člen:

```csharp
namespace System.Runtime.CompilerServices
{
    public static class RuntimeHelpers
    {
        public static T[] GetSubArray<T>(T[] array, System.Range range);
    }
}
```

### <a name="systemindex"></a>System. index

C#nemá žádný způsob, jak indexovat kolekci z konce, ale místo toho většina indexerů používá výraz "od začátku" nebo "length-i". Zavádíme nový Indexový výraz, který znamená "od konce". Funkce zavede nový unární operátor "Hat". Jeho jeden operand musí být převoditelné na `System.Int32`. Bude snížena na příslušné volání metody továrny `System.Index`.

Gramatiku pro *unary_expression* rozšiřujeme pomocí následujícího formuláře další syntaxe:

```antlr
unary_expression
    : '^' unary_expression
    ;
```

Tento index voláme *od* operátoru end. Předdefinovaný *index od koncových* operátorů je následující:

```csharp
    System.Index operator ^(int fromEnd);
```

Chování tohoto operátoru je definováno pouze pro vstupní hodnoty větší nebo rovny nule.

Příklady:

```csharp
var thirdItem = list[2];                // list[2]
var lastItem = list[^1];                // list[Index.CreateFromEnd(1)]

var multiDimensional = list[3, ^2]      // list[3, Index.CreateFromEnd(2)]
```

#### <a name="systemrange"></a>System. Range

C#nemá žádný syntaktický způsob pro přístup k "rozsahům" nebo "řezů" kolekcí. Obvykle jsou uživatelé nuceni implementovat komplexní struktury pro filtrování/fungování v řezech paměti nebo pro metody LINQ, jako je `list.Skip(5).Take(2)`. Díky přidání `System.Span<T>` a dalších podobných typů je důležitější, aby tento druh operace byl podporován na hlubší úrovni jazyka a modulu runtime a měly rozhraní jednotně.

Jazyk zavádí nový operátor rozsahu `x..y`. Je to binární operátor vpony, který přijímá dva výrazy. Operand může být vynechán (níže uvedené příklady) a musí být převoditelné na `System.Index`. Bude snížena na příslušné volání metody Factory `System.Range`.

C# Gramatická pravidla pro *multiplicative_expression* se nahrazují následujícími písmeny (aby bylo možné zavést novou úroveň priority):

```antlr
range_expression
    : unary_expression
    | range_expression? '..' range_expression?
    ;

multiplicative_expression
    : range_expression
    | multiplicative_expression '*' range_expression
    | multiplicative_expression '/' range_expression
    | multiplicative_expression '%' range_expression
    ;
```

Všechny formuláře *operátoru rozsahu* mají stejnou prioritu. Tato nová skupina priorit je nižší než *unární operátory* a vyšší než *mulitiplicative aritmetické operátory*.

Voláme operátor `..`, který je *operátorem rozsahu*. Předdefinovaný operátor rozsahu může být zhruba srozumitelný, aby odpovídal vyvolání předdefinovaného operátoru tohoto formuláře:

```csharp
    System.Range operator ..(Index start = 0, Index end = ^0);
```

Příklady:

```csharp
var slice1 = list[2..^3];               // list[Range.Create(2, Index.CreateFromEnd(3))]
var slice2 = list[..^3];                // list[Range.ToEnd(Index.CreateFromEnd(3))]
var slice3 = list[2..];                 // list[Range.FromStart(2)]
var slice4 = list[..];                  // list[Range.All]

var multiDimensional = list[1..2, ..]   // list[Range.Create(1, 2), Range.All]
```

Kromě toho by `System.Index` měl mít implicitní převod z `System.Int32`, aby se nemusely přetížit pro kombinování celých čísel a indexů přes multidimenzionální signatury.

## <a name="adding-index-and-range-support-to-existing-library-types"></a>Přidání podpory pro index a rozsah do existujících typů knihoven

### <a name="implicit-index-support"></a>Podpora implicitního indexu

Jazyk poskytne člen indexeru instance s jedním parametrem typu `Index` pro typy, které splňují následující kritéria:

- Typ je vypočítán.
- Typ má přístupný indexer instance, který jako argument přijímá jeden `int`.
- Typ nemá přístupný indexer instancí, který jako první parametr přebírá `Index`. `Index` musí být jediný parametr nebo zbývající parametry musí být nepovinné.

Typ je ***vypočítán*** , pokud má vlastnost s názvem `Length` nebo `Count` s přístupným mechanismem getter a návratovým typem `int`. Jazyk může tuto vlastnost použít k převedení výrazu typu `Index` do `int` v místě výrazu bez nutnosti použít typ `Index` vůbec. V případě, že jsou přítomny `Length` i `Count`, bude preferovaná `Length`. Pro zjednodušení předá návrh bude používat název `Length` k reprezentaci `Count` nebo `Length`.

U takových typů bude jazyk fungovat jako v případě, že je členem formuláře indexer, `T this[Index index]` kde `T` je návratový typ indexeru založeného na `int`, včetně všech poznámek `ref` stylů. Nový člen bude mít stejné `get` a `set` členy s porovnávacím přístupným přístupností jako indexerem `int`. 

Nový indexer bude implementován převodem argumentu typu `Index` do `int` a vyvoláním indexeru založeného na `int`. Pro účely diskuze použijte příklad `receiver[expr]`. Převod `expr` na `int` se projeví takto:

- Pokud má argument formu `^expr2` a typ `expr2` je `int`, bude přeložen na `receiver.Length - expr2`.
- V opačném případě bude převedena jako `expr.GetOffset(receiver.Length)`.

To umožňuje vývojářům používat funkci `Index` u stávajících typů bez nutnosti úprav. Například:

``` csharp
List<char> list = ...;
var value = list[^1]; 

// Gets translated to 
var value = list[list.Count - 1]; 
```

Výrazy `receiver` a `Length` budou podle potřeby vyloučeny, aby se zajistilo, že se všechny vedlejší účinky spustí pouze jednou. Například:

``` csharp
class Collection {
    private int[] _array = new[] { 1, 2, 3 };

    int Length {
        get {
            Console.Write("Length ");
            return _array.Length;
        }
    }

    int this[int index] => _array[index];
}

class SideEffect {
    Collection Get() {
        Console.Write("Get ");
        return new Collection();
    }

    void Use() { 
        int i = Get()[^1];
        Console.WriteLine(i);
    }
}
```

Tento kód vytiskne "Get Length 3".

### <a name="implicit-range-support"></a>Podpora implicitního rozsahu

Jazyk poskytne člen indexeru instance s jedním parametrem typu `Range` pro typy, které splňují následující kritéria:

- Typ je vypočítán.
- Typ má přístupný člen s názvem `Slice`, který má dva parametry typu `int`.
- Typ nemá indexer instancí, který jako první parametr přebírá jeden `Range`. `Range` musí být jediný parametr nebo zbývající parametry musí být nepovinné.

U takových typů se jazyk sváže, jako by byl člen indexeru formuláře `T this[Range range]`, kde `T` je návratový typ `Slice` metody, včetně všech `ref` poznámek ve stylu. Nový člen také bude mít k dispozici přístupnost s `Slice`. 

Pokud je indexer založený na `Range` vázaný na výraz s názvem `receiver`, bude snížen převodem výrazu `Range` na dvě hodnoty, které jsou poté předány metodě `Slice`. Pro účely diskuze použijte příklad `receiver[expr]`.

První argument `Slice` bude získán převodem výrazu zadaného rozsahu následujícím způsobem:

- Pokud má `expr` `expr1..expr2` formuláře (kde se `expr2` může vynechat) a `expr1` má typ `int`, bude vygenerován jako `expr1`.
- Pokud má `expr` `^expr1..expr2` (kde je možné vynechání `expr2`), bude vygenerována jako `receiver.Length - expr1`.
- Pokud má `expr` `..expr2` (kde je možné vynechání `expr2`), bude vygenerována jako `0`.
- V opačném případě bude vygenerována jako `expr.Start.GetOffset(receiver.Length)`.

Tato hodnota se znovu použije při výpočtu druhého argumentu `Slice`. V takovém případě se bude označovat jako `start`. Druhý argument `Slice` bude získán převodem výrazu zadaného rozsahu následujícím způsobem:

- Pokud má `expr` `expr1..expr2` formuláře (kde se `expr1` může vynechat) a `expr2` má typ `int`, bude vygenerován jako `expr2 - start`.
- Pokud má `expr` `expr1..^expr2` (kde je možné vynechání `expr1`), bude vygenerována jako `(receiver.Length - expr2) - start`.
- Pokud má `expr` `expr1..` (kde je možné vynechání `expr1`), bude vygenerována jako `receiver.Length - start`.
- V opačném případě bude vygenerována jako `expr.End.GetOffset(receiver.Length) - start`.

Výrazy `receiver`, `Length`a `expr` budou přecházet podle potřeby, aby se zajistilo, že všechny vedlejší účinky budou provedeny pouze jednou. Například:

``` csharp
class Collection {
    private int[] _array = new[] { 1, 2, 3 };

    int Length {
        get {
            Console.Write("Length ");
            return _array.Length;
        }
    }

    int[] Slice(int start, int length) { 
        var slice = new int[length];
        Array.Copy(_array, start, slice, 0, length);
        return slice;
    }
}

class SideEffect {
    Collection Get() {
        Console.Write("Get ");
        return new Collection();
    }

    void Use() { 
        var array = Get()[0..2];
        Console.WriteLine(array.length);
    }
}
```

Tento kód vytiskne "Get Length 2".

V jazyce se zvláštními případy rozumí následující známé typy: 

- `string`: místo `Slice`se použije `Substring` metody.
- `array`: místo `Slice`se použije `System.Reflection.CompilerServices.GetSubArray` metody.

## <a name="alternatives"></a>Alternativy

Nové operátory (`^` a `..`) jsou syntaktický cukr. Tato funkce může být implementována explicitními voláními metod `System.Index` a `System.Range` továrnou, ale bude mít za následek mnohem více často používaný kód a prostředí bude neintuitivní.

## <a name="il-representation"></a>Reprezentace IL

Tyto dva operátory budou sníženy na pravidelná volání indexerů/metod bez změny v následných vrstvách kompilátoru.

## <a name="runtime-behavior"></a>Chování za běhu

- Kompilátor může optimalizovat indexery pro předdefinované typy, jako jsou pole a řetězce, a snížit indexování na příslušné existující metody.
- `System.Index` bude vyvolána, pokud je vytvořena se zápornou hodnotou.
- `^0` nevyvolává, ale překládá se na délku kolekce/výčtu, do které je předáno.
- `Range.All` je sémanticky ekvivalentní `0..^0`a lze je rozkonstruovat na tyto indexy.

## <a name="considerations"></a>Požadavky

### <a name="detect-indexable-based-on-icollection"></a>Zjistit index založený na rozhraní ICollection

Inspiraci pro toto chování byly inicializátory kolekce. Použití struktury typu k vyjádření, že se rozhodl do funkce. V případě typů inicializátorů kolekce se může přihlásit k funkci implementací rozhraní `IEnumerable` (neobecné).

Tento návrh původně vyžadoval, aby tyto typy implementovaly `ICollection`, aby se daly kvalifikovat jako indexovaná. V takovém případě se vyžaduje několik speciálních případů:

- `ref struct`: tyto typy nemohou implementovat rozhraní, jako `Span<T>` jsou ideální pro podporu indexů a rozsahů. 
- `string`: neimplementuje `ICollection` a přidávání tohoto `interface` má velké náklady.

To znamená, že se už vyžaduje podpora speciálních typů klíčů. Speciální velká a malá písmena `string`a jsou méně zajímavá, protože jazyk funguje v jiných oblastech (`foreach` nižších, konstant atd.). Speciální velká a malá písmena `ref struct` jsou více týkající se toho, že se jedná o zvláštní velká a malá písmena celé třídy typů. Jsou označeny jako Indexovaelné, pokud jednoduše obsahují vlastnost s názvem `Count` s návratovým typem `int`. 

Po zvážení návrhu byl tento návrh normalizován, aby řekněme, že jakýkoli typ, který má vlastnost `Count` / `Length` s návratovým typem `int` je indexovaná. Tím se odstraní všechna speciální velká a malá písmena, a to i pro `string` a pole.

### <a name="detect-just-count"></a>Zjistit počet pouze

Zjišťování názvů vlastností `Count` nebo `Length` ztěžuje návrh bitové konstrukce. Výběr pouze jedné pro standardizaci, i když není dostačující, aby se vyloučil velký počet typů:

- Použít `Length`: vyloučí v podstatě většinu každé kolekce v System. Collections a subnamespaces. Ty jsou obvykle odvozeny od `ICollection` a proto upřednostňují `Count` nad délkou.
- Use `Count`: vyloučí `string`, pole, `Span<T>` a nejvíce `ref struct` založených typů.

Nadbytečné komplikace při prvotní detekci indexovaných typů jsou v jiných aspektech převáženy svými zjednodušeními.

### <a name="choice-of-slice-as-a-name"></a>Volba řezu jako názvu

Název `Slice` byl vybrán jako standardní název pro operace stylu řezu v rozhraní .NET. Počínaje netcoreapp 2.1 jsou všechny typy řezů rozsahu použity pro operace dělení `Slice` názvu. Před netcoreapp 2.1 zatím neexistují žádné příklady vytváření řezů, které by bylo možné v případě příkladu najít. Typy jako `List<T>`, `ArraySegment<T>`, `SortedList<T>` by byly ideální pro vytváření řezů, ale koncept neexistoval při přidání typů. 

Proto je `Slice` jediným příkladem, který byl vybrán jako název.

### <a name="index-target-type-conversion"></a>Převod cílového typu indexu

Dalším způsobem, jak zobrazit transformaci `Index` ve výrazu indexeru, je jako převod cílového typu. Místo vazby, jako kdyby byl členem formuláře `return_type this[Index]`, jazyk místo toho přiřadí cílový typový převod do `int`. 

Tento koncept se dá zobecnit na všechny členské přístupy na základě počtu typů. Vždy, když je výraz s typem `Index` použit jako argument pro vyvolání člena instance a přijímač je možné vyhodnotit, bude mít výraz převod cílového typu na hodnotu `int`. Vyvolání členů, která se vztahují k tomuto převodu, zahrnují metody, indexery, vlastnosti, metody rozšíření atd... Pouze konstruktory jsou vyloučeny, protože nemají žádného přijímače. 

Převod cílového typu se implementuje následovně pro libovolný výraz, který má typ `Index`. Pro účely diskuze můžete použít příklad `receiver[expr]`:

- Pokud má `expr` `^expr2` a typ `expr2` `int`, bude přeložen na `receiver.Length - expr2`.
- V opačném případě bude převedena jako `expr.GetOffset(receiver.Length)`.

Výrazy `receiver` a `Length` budou podle potřeby vyloučeny, aby se zajistilo, že se všechny vedlejší účinky spustí pouze jednou. Například:

``` csharp
class Collection {
    private int[] _array = new[] { 1, 2, 3 };

    int Length {
        get {
            Console.Write("Length ");
            return _array.Length;
        }
    }

    int GetAt(int index) => _array[index];
}

class SideEffect {
    Collection Get() {
        Console.Write("Get ");
        return new Collection();
    }

    void Use() { 
        int i = Get().GetAt(^1);
        Console.WriteLine(i);
    }
}
```

Tento kód vytiskne "Get Length 3". 

Tato funkce bude prospěšná pro každého člena, který má parametr, který představoval index. Například `List<T>.InsertAt`. To má taky možnost záměny, protože jazyk neposkytuje žádné pokyny k tomu, jestli je výraz určený pro indexování. Vše může být převedeno libovolný výraz `Index`, aby `int` při vyvolání člena na základě typu Count. 

Podléhající

- Tento převod je použitelný pouze v případě, že výraz typu `Index` je přímo argumentem člena. Neplatí to pro žádné vnořené výrazy.

## <a name="decisions-made-during-implementation"></a>Rozhodnutí učiněná během implementace

- Všichni členové ve vzoru musí být členy instance.
- Pokud se Metoda Length najde, ale má nesprávný návratový typ, pokračujte v hledání počtu.
- Indexer použitý pro vzor indexu musí mít právě jeden parametr int.
- Metoda řezu použitá pro vzorek rozsahu musí mít přesně dva parametry int.
- Při hledání členů vzorů hledáme původní definice, nikoli konstruované členy.

## <a name="design-meetings"></a>Schůzky pro návrh

- https://github.com/dotnet/csharplang/blob/master/meetings/2019/LDM-2019-04-01.md

## <a name="questions"></a>Otázky
