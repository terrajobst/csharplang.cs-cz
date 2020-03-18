---
ms.openlocfilehash: 2e054c629f71ae37b112300905c3106f9ce977e8
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484662"
---
# <a name="auto-implemented-property-field-targeted-attributes"></a>Automaticky implementované pole vlastností – cílové atributy

## <a name="summary"></a>Souhrn
[summary]: #summary

Tato funkce je v úmyslu dovolit vývojářům použít atributy přímo na zálohovacích polích automaticky implementovaných vlastností.

## <a name="motivation"></a>Motivační
[motivation]: #motivation

V současné době není možné použít atributy u zálohovaných polí automaticky implementovaných vlastností.  V případech, kdy vývojář musí použít atribut cílící na pole, jsou nuceni deklarovat pole ručně a použít syntaxi podrobnějších vlastností.  S ohledem C# na to, že u generovaných polí zálohování jsou vždycky podporované atributy cílené na pole pro události, které mají smysl roztáhnout stejnou funkci na jejich vlastnost kin.

## <a name="detailed-design"></a>Podrobný návrh
[design]: #detailed-design

V krátkém případě by byly právní C# a nevzniklé upozornění:

```csharp
[Serializable]
public class Foo 
{
    [field: NonSerialized]
    public string MySecret { get; set; }
}
```

Výsledkem je, že se atributy cílené na pole aplikují na pole pro zálohování generované kompilátorem:

```csharp
[Serializable]
public class Foo 
{
    [NonSerialized]
    private string _mySecretBackingField;
    
    public string MySecret
    {
        get { return _mySecretBackingField; }
        set { _mySecretBackingField = value; }
    }
}
```

Jak už bylo zmíněno, přináší to paritu C# s syntaxí události z 1,0, protože následující je již právní a chová se podle očekávání:

```csharp
[Serializable]
public class Foo
{
    [field: NonSerialized]
    public event EventHandler MyEvent;
}
```

## <a name="drawbacks"></a>Nevýhody
[drawbacks]: #drawbacks

Při implementaci této změny existují dvě potenciální nevýhody:

1. Pokus o použití atributu na pole automaticky implementované vlastnosti vytvoří upozornění kompilátoru, že atributy v tomto bloku budou ignorovány.  Pokud byl kompilátor změněn tak, aby podporoval tyto atributy, byly aplikovány na pole zálohování v následné rekompilaci, což by mohlo změnit chování programu za běhu.
1. Kompilátor aktuálně neověřuje cíle AttributeUsage atributů při pokusu o jejich použití na pole automaticky implementované vlastnosti.  Pokud byl kompilátor změněn tak, aby podporoval atributy cílené na pole, a příslušný atribut nelze použít na pole, kompilátor by vygeneroval chybu namísto upozornění a přerušil sestavení.

## <a name="alternatives"></a>Alternativy
[alternatives]: #alternatives

## <a name="unresolved-questions"></a>Nevyřešené dotazy
[unresolved]: #unresolved-questions

## <a name="design-meetings"></a>Schůzky pro návrh
