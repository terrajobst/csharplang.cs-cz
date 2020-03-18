---
ms.openlocfilehash: 392d932459ff0a4cb0d6d32c1606f73f9b913c68
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484452"
---
# <a name="covariant-return-types"></a>kovariantní návratové typy

* Navrženo [x]
* [] Prototyp: Nezahájeno
* [] Implementace: Nezahájeno
* [] Specifikace: Nezahájeno

## <a name="summary"></a>Souhrn
[summary]: #summary

Podporuje _kovariantní návratové typy_. Konkrétně umožněte přepsání metody, aby měl více odvozený odkazový typ než metoda, kterou Přepisuje.

## <a name="motivation"></a>Motivační
[motivation]: #motivation

Je to společný vzor v kódu, který musí být vymysleli různými názvy metod, aby bylo možné obejít omezení jazyka, které přepisy musí vracet stejný typ jako Potlačená metoda. Viz níže pro příklad ze základu kódu Roslyn.

## <a name="detailed-design"></a>Podrobný návrh
[design]: #detailed-design

Podporuje _kovariantní návratové typy_. Konkrétně umožněte přepsání metody, aby měl více odvozený odkazový typ než metoda, kterou Přepisuje. To by mělo platit pro metody a vlastnosti a podporuje se v třídách a rozhraních.

To by bylo užitečné ve výrobním modelu. Například v základu kódu Roslyn bychom měli

``` cs
class Compilation ...
{
    virtual Compilation WithOptions(Options options)...
}
```

``` cs
class CSharpCompilation : Compilation
{
    override CSharpCompilation WithOptions(Options options)...
}
```

Implementace by mohla být pro kompilátor, aby vygeneroval přepsání metody jako "novou" metodu, která skrývá metodu základní třídy, spolu s _metodou mostu_ , která implementuje metodu základní třídy se voláním metody odvozené třídy.

## <a name="drawbacks"></a>Nevýhody
[drawbacks]: #drawbacks

- [] Každá změna jazyka musí platit pro sebe sama.
- [] Měli byste zajistit, aby výkon byl přiměřený, i v případě hlubokých hierarchií dědičnosti.
- [] Měli byste zajistit, aby artefakty strategie překladu neovlivnily sémantiku jazyka, ani když spotřebovávají nové IL ze starých kompilátorů.

## <a name="alternatives"></a>Alternativy
[alternatives]: #alternatives

Tato jazyková pravidla můžeme mírně zmírnit, aby bylo možné, ve zdroji,

```csharp
abstract class Cloneable
{
    public abstract Cloneable Clone();
}

class Digit : Cloneable
{
    public override Cloneable Clone()
    {
        return this.Clone();
    }

    public new Digit Clone() // Error: 'Digit' already defines a member called 'Clone' with the same parameter types
    {
        return this;
    }
}
```

## <a name="unresolved-questions"></a>Nevyřešené dotazy
[unresolved]: #unresolved-questions

- [] Jak budou rozhraní API, která byla zkompilována pro použití této funkce, fungovat ve starších verzích jazyka?

## <a name="design-meetings"></a>Schůzky pro návrh

Žádná ještě není. Došlo k nějaké diskuzi na <https://github.com/dotnet/roslyn/issues/357>.