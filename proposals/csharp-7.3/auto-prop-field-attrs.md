---
ms.openlocfilehash: 2e054c629f71ae37b112300905c3106f9ce977e8
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484662"
---
# <a name="auto-implemented-property-field-targeted-attributes"></a><span data-ttu-id="4d2fe-101">Automaticky implementované pole vlastností – cílové atributy</span><span class="sxs-lookup"><span data-stu-id="4d2fe-101">Auto-Implemented Property Field-Targeted Attributes</span></span>

## <a name="summary"></a><span data-ttu-id="4d2fe-102">Souhrn</span><span class="sxs-lookup"><span data-stu-id="4d2fe-102">Summary</span></span>
[summary]: #summary

<span data-ttu-id="4d2fe-103">Tato funkce je v úmyslu dovolit vývojářům použít atributy přímo na zálohovacích polích automaticky implementovaných vlastností.</span><span class="sxs-lookup"><span data-stu-id="4d2fe-103">This feature intends to allow developers to apply attributes directly to the backing fields of auto-implemented properties.</span></span>

## <a name="motivation"></a><span data-ttu-id="4d2fe-104">Motivační</span><span class="sxs-lookup"><span data-stu-id="4d2fe-104">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="4d2fe-105">V současné době není možné použít atributy u zálohovaných polí automaticky implementovaných vlastností.</span><span class="sxs-lookup"><span data-stu-id="4d2fe-105">Currently it is not possible to apply attributes to the backing fields of auto-implemented properties.</span></span>  <span data-ttu-id="4d2fe-106">V případech, kdy vývojář musí použít atribut cílící na pole, jsou nuceni deklarovat pole ručně a použít syntaxi podrobnějších vlastností.</span><span class="sxs-lookup"><span data-stu-id="4d2fe-106">In those cases where the developer must use a field-targeting attribute they are forced to declare the field manually and use the more verbose property syntax.</span></span>  <span data-ttu-id="4d2fe-107">S ohledem C# na to, že u generovaných polí zálohování jsou vždycky podporované atributy cílené na pole pro události, které mají smysl roztáhnout stejnou funkci na jejich vlastnost kin.</span><span class="sxs-lookup"><span data-stu-id="4d2fe-107">Given that C# has always supported field-targeted attributes on the generated backing field for events it makes sense to extend the same functionality to their property kin.</span></span>

## <a name="detailed-design"></a><span data-ttu-id="4d2fe-108">Podrobný návrh</span><span class="sxs-lookup"><span data-stu-id="4d2fe-108">Detailed design</span></span>
[design]: #detailed-design

<span data-ttu-id="4d2fe-109">V krátkém případě by byly právní C# a nevzniklé upozornění:</span><span class="sxs-lookup"><span data-stu-id="4d2fe-109">In short, the following would be legal C# and not produce a warning:</span></span>

```csharp
[Serializable]
public class Foo 
{
    [field: NonSerialized]
    public string MySecret { get; set; }
}
```

<span data-ttu-id="4d2fe-110">Výsledkem je, že se atributy cílené na pole aplikují na pole pro zálohování generované kompilátorem:</span><span class="sxs-lookup"><span data-stu-id="4d2fe-110">This would result in the field-targeted attributes being applied to the compiler-generated backing field:</span></span>

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

<span data-ttu-id="4d2fe-111">Jak už bylo zmíněno, přináší to paritu C# s syntaxí události z 1,0, protože následující je již právní a chová se podle očekávání:</span><span class="sxs-lookup"><span data-stu-id="4d2fe-111">As mentioned, this brings parity with event syntax from C# 1.0 as the following is already legal and behaves as expected:</span></span>

```csharp
[Serializable]
public class Foo
{
    [field: NonSerialized]
    public event EventHandler MyEvent;
}
```

## <a name="drawbacks"></a><span data-ttu-id="4d2fe-112">Nevýhody</span><span class="sxs-lookup"><span data-stu-id="4d2fe-112">Drawbacks</span></span>
[drawbacks]: #drawbacks

<span data-ttu-id="4d2fe-113">Při implementaci této změny existují dvě potenciální nevýhody:</span><span class="sxs-lookup"><span data-stu-id="4d2fe-113">There are two potential drawbacks to implementing this change:</span></span>

1. <span data-ttu-id="4d2fe-114">Pokus o použití atributu na pole automaticky implementované vlastnosti vytvoří upozornění kompilátoru, že atributy v tomto bloku budou ignorovány.</span><span class="sxs-lookup"><span data-stu-id="4d2fe-114">Attempting to apply an attribute to the field of an auto-implemented property produces a compiler warning that the attributes in that block will be ignored.</span></span>  <span data-ttu-id="4d2fe-115">Pokud byl kompilátor změněn tak, aby podporoval tyto atributy, byly aplikovány na pole zálohování v následné rekompilaci, což by mohlo změnit chování programu za běhu.</span><span class="sxs-lookup"><span data-stu-id="4d2fe-115">If the compiler were changed to support those attributes they would be applied to the backing field on a subsequent recompilation which could alter the behavior of the program at runtime.</span></span>
1. <span data-ttu-id="4d2fe-116">Kompilátor aktuálně neověřuje cíle AttributeUsage atributů při pokusu o jejich použití na pole automaticky implementované vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="4d2fe-116">The compiler does not currently validate the AttributeUsage targets of the attributes when attempting to apply them to the field of the auto-implemented property.</span></span>  <span data-ttu-id="4d2fe-117">Pokud byl kompilátor změněn tak, aby podporoval atributy cílené na pole, a příslušný atribut nelze použít na pole, kompilátor by vygeneroval chybu namísto upozornění a přerušil sestavení.</span><span class="sxs-lookup"><span data-stu-id="4d2fe-117">If the compiler were changed to support field-targeted attributes and the attribute in question cannot be applied to a field the compiler would emit an error instead of a warning, breaking the build.</span></span>

## <a name="alternatives"></a><span data-ttu-id="4d2fe-118">Alternativy</span><span class="sxs-lookup"><span data-stu-id="4d2fe-118">Alternatives</span></span>
[alternatives]: #alternatives

## <a name="unresolved-questions"></a><span data-ttu-id="4d2fe-119">Nevyřešené dotazy</span><span class="sxs-lookup"><span data-stu-id="4d2fe-119">Unresolved questions</span></span>
[unresolved]: #unresolved-questions

## <a name="design-meetings"></a><span data-ttu-id="4d2fe-120">Schůzky pro návrh</span><span class="sxs-lookup"><span data-stu-id="4d2fe-120">Design meetings</span></span>
