---
ms.openlocfilehash: 2e4a3a32def6900797c151264c984378b09b4988
ms.sourcegitcommit: 5983461e05be62f39c77383cb7857539518cb04f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/15/2019
ms.locfileid: "79485124"
---

# <a name="discriminated-unions--enum-class"></a>Rozlišené sjednocení/`enum class`

`enum class`ES je nový druh deklarace typu, který se někdy označuje jako rozlišené sjednocení, kde každá z nich má daný typ a každá instance se nepřekrývá.

`enum class` je definována pomocí následující syntaxe:

```antlr
enum_class
    : 'partial'? 'enum class' identifier type_parameter_list? type_parameter_constraints_clause* 
      '{' enum_class_body '}'
    ;

enum_class_body
    : enum_class_cases?
    | enum_class_cases ','
    ;

enum_class_cases
    : enum_class_case
    | enum_class_case ',' enum_class_cases
    ;

enum_class_case
    : enum_class
    | class_declaration
    | identifier type_parameter_list? '(' formal_parameter_list? ')'
    | identifier
    ;

```

Ukázková syntaxe:

```C#
enum class Shape
{
    Rectangle(float Width, float Length),
    Circle(float Radius),
}
```

## <a name="semantics"></a>Sémantiku

Definice `enum class` definuje kořenový typ, což je abstraktní třída se stejným názvem jako deklarace `enum class` a sada členů, z nichž každý má typ, který je podtypem kořenového typu. Pokud existuje více definic `partial enum class`, budou všichni členové považováni za členy definice třídy výčtu. Na rozdíl od uživatelsky definované definice abstraktní třídy je typ kořene `enum class` ve výchozím nastavení částečný a definuje tak, aby měl výchozí *soukromý* konstruktor bez parametrů.

Všimněte si, že vzhledem k tomu, že je kořenový typ definován jako částečná abstraktní třída, mohou být přidány i částečné definice *kořenového typu* , kde jsou povoleny standardní formáty syntaxe pro tělo třídy.
Nicméně žádné typy nemůžou přímo dědit z kořenového typu v žádné deklaraci, kromě těch, které jsou zadané jako `enum class` členy. Kromě toho nejsou pro kořenový typ povoleny žádné uživatelsky definované konstruktory.

Existují čtyři typy deklarací `enum class` členů:

* Jednoduché členy třídy

* komplexní členové třídy

* `enum class` členů

* členy hodnoty.

### <a name="simple-class-members"></a>Jednoduché členy třídy

Jednoduchá deklarace člena třídy definuje novou vnořenou třídu "Record" (úmyslně nedefinovanou v tomto dokumentu) se stejným názvem. Vnořená třída dědí z kořenového typu.

Dle výše uvedeného ukázkového kódu,

```C#
enum class Shape
{
    Rectangle(float Width, float Length),
    Circle(float Radius)
}
```

deklarace `enum class` má sémantiku ekvivalentní následující deklaraci.

```C#
partial abstract class Shape
{
    public data class Rectangle(float Width, float Length) : Shape,
    public data class Circle(float Radius) : Shape
}
```

### <a name="complex-class-members"></a>Komplexní členové třídy

Celou deklaraci třídy můžete také vnořit pod `enum class` deklaraci. Bude považována za vnořenou třídu kořenového typu. Syntaxe umožňuje jakékoli deklarace třídy, ale je vyžadována pro komplexní člen třídy, aby dědil z deklarace přímo obsahující `enum class`. 

### <a name="enum-class-members"></a>`enum class` členů

`enum classes` může být vnořen do sebe navzájem, např.

```C#
enum class Expr
{
    enum class Binary
    {
        Addition(Expr left, Expr right),
        Multiplication(Expr left, Expr right)
    }
}
```

To je téměř totožné s sémantikou `enum class`nejvyšší úrovně, s tím rozdílem, že vnořená třída výčtu definuje vnořený kořenový typ a vše pod vnořenou třídou výčtu je podtypem vnořeného typu root, namísto kořenového typu nejvyšší úrovně.

```C#
partial abstract class Expr
{
    partial abstract class Binary : Expr
    {
        public data class Addition(Expr left, Expr right) : Binary,
        public data class Multiplication(Expr left, Expr right) : Binary
    }
}
```

### <a name="value-members"></a>Členové hodnoty

`enum classes` mohou obsahovat také členy hodnot. Členové hodnoty definují statické vlastnosti pouze pro získání na kořenovém typu, které vracejí také typ root, např.

```C#
enum class Color
{
    Red,
    Green
}
```

má vlastnosti, které jsou ekvivalentní

```C#
partial abstract class Color
{
    public static Color Red => ...;
    public static Color Green => ...;
}
```

Kompletní sémantika se považuje za podrobnosti implementace, ale je zaručeno, že se pro každou vlastnost vrátí jedna jedinečná instance a stejná instance se vrátí při opakovaném vyvolání.


### <a name="switch-expression-and-patterns"></a>Přepnout výraz a vzory

Existují některé navržené úpravy pro porovnávání vzorů a výraz přepínače pro zpracování `enum classes`. Výrazy Switch mohou již odpovídat typům pomocí vzorce proměnné, ale v současné době pro typy odkazů nejsou považovány za dokončené žádné sady přepínacích zbraní ve výrazu Switch, s výjimkou porovnávání proti statickému typu argumentu nebo podtypu.

Výrazy přepínače by se změnily tak, že pokud je kořenový typ `enum class` statickým typem argumentu pro výraz Switch a existuje sada vzorů, které odpovídají všem členům výčtu, pak bude přepínač považován za vyčerpávající.

Vzhledem k tomu, že členy hodnoty nejsou konstanty a nedefinují nové statické typy, nelze je aktuálně porovnat podle vzoru. Aby to bylo možné, byl přidán nový vzor s použitím syntaxe konstanty Pattern, aby bylo možné porovnat s `enum class`mi členy hodnoty. Shoda je definována tak, aby byla úspěšná, pokud a pouze v případě, že se argument shoduje se vzorem a hodnota vrácená členem `enum class` hodnota by odkazovala na stejnou hodnotu, přestože implementace není nutná k provedení této kontroly.


## <a name="open-questions"></a>Otevřené otázky

- [] Co znamená společný typ algoritmu o `enum class` členů? Je tento platný kód?
    * `var x = b ? new Shape.Rectangle(...) : new Shape.Circle(...)`

- [] Přidání nového vzoru pouze pro členy hodnoty se zdají být těžké. Je k dispozici obecnější konstrukce verzí, která dává smysl?
    - [] Členové hodnoty také nemají správnou mapu konstrukce paralelní vnořené třídy z tohoto důvodu

- [] Je přepínání s argumentem, který zaručuje, že `enum class` statický typ by měl být konstantní?

- [] By existoval způsob, jak zajistit, aby `enum class`y ES nebyly považovány za dokončené ve výrazu Switch? Předpona s `virtual`?

- [] Jaké modifikátory by měly být povolené na `enum class`?