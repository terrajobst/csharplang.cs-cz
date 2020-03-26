---
ms.openlocfilehash: f000dda7eeb1c4f17c26f94c326a12a9d0014288
ms.sourcegitcommit: 1e1c7c72b156e2fbc54d6d6ac8d21bca9934d8d2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/26/2020
ms.locfileid: "80281967"
---

# <a name="target-typed-new-expressions"></a>Výrazy `new` s cílovým typem

* Navrženo [x]
* [x] [prototyp](https://github.com/alrz/roslyn/tree/features/target-typed-new)
* [] Implementace
* [] Specifikace

## <a name="summary"></a>Souhrn
[summary]: #summary

Nevyžadovat specifikaci typu pro konstruktory, když je typ znám. 

## <a name="motivation"></a>Motivační
[motivation]: #motivation

Povoluje inicializaci pole bez duplikace typu.
```cs
Dictionary<string, List<int>> field = new() {
    { "item1", new() { 1, 2, 3 } }
};
```

Povolí vynechání typu, když se dá odvodit z použití.
```cs
XmlReader.Create(reader, new() { IgnoreWhitespace = true });
```

Vytvořte instanci objektu bez nutnosti hláskování typu.
```cs
private readonly static object s_syncObj = new();
```

## <a name="specification"></a>Specifikace
[design]: #detailed-design

Nové syntaktické formuláře, *target_typed_new* *object_creation_expression* , jsou přijaty, ve kterém je *typ* volitelný.

```antlr
object_creation_expression
    : 'new' type '(' argument_list? ')' object_or_collection_initializer?
    | 'new' type object_or_collection_initializer
    | target_typed_new
    ;
target_typed_new
    : 'new' '(' argument_list? ')' object_or_collection_initializer?
    ;
```

Výraz *target_typed_new* nemá typ. Existuje však nový *Převod vytvoření objektu* , který je implicitní převod z výrazu, který existuje z *target_typed_new* na každý typ.

Pokud `T` je instance `System.Nullable`, je typ zadaného cílového typu `T``T`nadřízený typ `T0`. Jinak `T0` `T`. Význam výrazu *target_typed_new* , který je převeden na typ `T` je stejný jako význam odpovídající *object_creation_expression* , který specifikuje `T0` jako typ.

Jedná se o chybu při kompilaci, pokud je *target_typed_new* použito jako operand unárního nebo binárního operátoru, nebo pokud se používá, pokud není předmětem *převodu na vytvoření objektu*.

> **Otevření problému:** Pokud chcete, abychom jako cílový typ povolili delegáty a řazené kolekce členů?

Výše uvedená pravidla zahrnují delegáty (typ odkazu) a řazené kolekce členů (typ struktury). I když jsou oba typy constructible, je-li typ odvozen, anonymní funkce nebo literál řazené kolekce členů již lze použít.
```cs
(int a, int b) t = new(1, 2); // "new" is redundant
Action a = new(() => {}); // "new" is redundant

(int a, int b) t = new(); // OK; same as (0, 0)
Action a = new(); // no constructor found
```

### <a name="miscellaneous"></a>Různé

Níže jsou uvedené důsledky této specifikace:

- `throw new()` je povolený (typ cíle je `System.Exception`).
- `new` cílového typu není povolený u binárních operátorů.
- Není povoleno, není-li k dispozici žádný typ k cíli: unární operátory, kolekce `foreach`, v `using`ve formě dekonstrukce ve výrazu `await` jako vlastnost anonymního typu (`new { Prop = new() }`) v příkazu `lock`, v `sizeof`v příkazu `fixed` v příkazu`new().field`v rámci člena přístup (`someDynamic.Method(new())`) v dynamicky odesílané operaci (`is`) v dotazu LINQ jako Operand operátoru `??`, jako levý operand operátoru ,  ...
- Nepovoluje se taky jako `ref`.
- Následující typy typů nejsou povolené jako cíle převodu.
  - **Typy výčtu:** `new()` budou fungovat (jako `new Enum()` funguje jako výchozí hodnota), ale `new(1)` nebudou fungovat, protože výčtové typy nemají konstruktor.
  - **Typy rozhraní:** To bude fungovat stejně jako odpovídající výraz vytvoření pro typy modelu COM.
  - **Typy polí:** pole potřebují pro zadání délky speciální syntaxi.    
  - **dynamické:** nepovolujeme `new dynamic()`, takže nepovolujeme `new()` s `dynamic` jako cílový typ.
  - **řazené kolekce členů:** Mají stejný význam jako vytvoření objektu pomocí nadřazeného typu.
  - Všechny ostatní typy, které nejsou povoleny v *object_creation_expression* jsou vyloučeny také, například typy ukazatelů.   

## <a name="drawbacks"></a>Nevýhody
[drawbacks]: #drawbacks

Došlo k nějakým potížím s cílovým typem `new` vytváření nových kategorií podstatných změn, ale už to máme s `null` a `default`a že nedošlo k významnému problému.

## <a name="alternatives"></a>Alternativy
[alternatives]: #alternatives

Většina stížností na typy, které jsou příliš dlouhé na duplicity při inicializaci pole, se týká *typů argumentů* , které nejsou samotný typ, můžeme odvodit jenom argumenty typu jako `new Dictionary(...)` (nebo podobné) a odvodit argumenty typu místně z argumentů nebo inicializátoru kolekce.

## <a name="questions"></a>Otázky
[questions]: #questions

- Pokud byste měli zakázat použití ve stromech výrazů? žádné
- Jak funkce komunikuje s argumenty `dynamic`? (žádné zvláštní zacházení)
- Jak má IntelliSense pracovat s `new()`? (pouze v případě, že existuje jeden cílový typ)

## <a name="design-meetings"></a>Schůzky pro návrh

- [LDM-2017-10-18](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-10-18.md#100)
- [LDM-2018-05-21](https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-05-21.md)
- [LDM-2018-06-25](https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-06-25.md)
- [LDM-2018-08-22](https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-08-22.md#target-typed-new)
- [LDM-2018-10-17](https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md)
- [LDM-2020-03-25](https://github.com/dotnet/csharplang/blob/master/meetings/2020/LDM-2020-03-25.md)
