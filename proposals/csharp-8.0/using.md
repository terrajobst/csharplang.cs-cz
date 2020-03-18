---
ms.openlocfilehash: 91afbc3e3412049cd183c36c8035f1862c520413
ms.sourcegitcommit: da1180f7eacdd5067b32d291a76e6764159e00fe
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/05/2019
ms.locfileid: "79485054"
---
# <a name="pattern-based-using-and-using-declarations"></a>"založené na vzorcích" a pomocí deklarací "

## <a name="summary"></a>Souhrn

Tento jazyk přidá dvě nové funkce kolem příkazu `using`, aby bylo možné zjednodušit správu prostředků: `using` by měl kromě `IDisposable` a přidání deklarace `using` do tohoto jazyka rozpoznat vzor na jedno použití.

## <a name="motivation"></a>Motivační

Příkaz `using` je účinným nástrojem pro správu prostředků dnes, ale vyžaduje poměrně bitovou procedury. Metody, které mají mnoho prostředků ke správě, můžou syntakticky zabřednete dolů pomocí řady příkazů `using`. Tato zátěže syntaxe je dostatečná, protože většina zásad stylu kódování má explicitně výjimku kolem složených závorek pro tento scénář. 

Deklarace `using` v tomto případě odstraní většinu procedury a získá C# na základě dalších jazyků, které zahrnují bloky správy prostředků. `using` založené na vzorech navíc umožňuje vývojářům rozšířit sadu typů, které se tady můžou zúčastnit. V mnoha případech odebrání nutnosti vytvářet typy obálek, které existují pouze k povolení použití hodnot v příkazu `using`. 

Společně tyto funkce umožňují vývojářům zjednodušit a rozšířit scénáře, kde je možné `using` použít.

## <a name="detailed-design"></a>Podrobný návrh 

### <a name="using-declaration"></a>using – deklarace

Jazyk umožní přidání `using` do místní deklarace proměnné. Taková deklarace bude mít stejný účinek jako deklarace proměnné v příkazu `using` ve stejném umístění.

```csharp
if (...) 
{ 
   using FileStream f = new FileStream(@"C:\users\jaredpar\using.md");
   // statements
}

// Equivalent to 
if (...) 
{ 
   using (FileStream f = new FileStream(@"C:\users\jaredpar\using.md")) 
   {
    // statements
   }
}
```

Doba života `using` místní bude rozšířena na konec oboru, ve kterém je deklarována. `using` národní prostředí bude poté uvolněno v opačném pořadí, ve kterém jsou deklarovány. 

```csharp
{ 
    using var f1 = new FileStream("...");
    using var f2 = new FileStream("..."), f3 = new FileStream("...");
    ...
    // Dispose f3
    // Dispose f2 
    // Dispose f1
}
```

Neexistují žádná omezení `goto`ani žádná jiná konstrukce toku ovládacích prvků na tvář deklarace `using`. Místo toho kód funguje stejně jako v případě ekvivalentního příkazu `using`:

```csharp
{
    using var f1 = new FileStream("...");
  target:
    using var f2 = new FileStream("...");
    if (someCondition) 
    {
        // Causes f2 to be disposed but has no effect on f1
        goto target;
    }
}
```

Místní deklarace v `using` místní deklaraci budou implicitně jen pro čtení. To odpovídá chování místních hodnot deklarovaných v příkazu `using`. 

Gramatika jazyka pro `using` deklarace bude následující:

```antlr
local-using-declaration:
  using type using-declarators

using-declarators:
  using-declarator
  using-declarators , using-declarator
  
using-declarator:
  identifier = expression
```

Omezení týkající se `using` deklarace:

- Se nemusí nacházet přímo uvnitř `case` popisku, ale místo toho musí být uvnitř bloku uvnitř `case` popisku.
- Nemusí se zobrazit jako součást deklarace proměnné `out`. 
- Musí mít inicializátor pro každý deklarátor.
- Místní typ musí být implicitně převoditelný na `IDisposable` nebo musí splňovat `using` vzor.

### <a name="pattern-based-using"></a>založené na vzorcích pomocí

Jazyk přidá pojem vzor na jedno použití: to je typ, který má přístupnou metodu instance `Dispose`. Typy, které vyhovují vzorům na více než jedno, mohou být součástí příkazu `using` nebo deklarace, aniž by bylo nutné implementovat `IDisposable`. 

```csharp
class Resource
{ 
    public void Dispose() { ... }
}

using (var r = new Resource())
{
    // statements
}
```

To umožní vývojářům využít `using` v mnoha nových scénářích:

- `ref struct`: tyto typy nemůžou implementovat rozhraní ještě dnes, takže se nemůžou podílet na příkazech `using`.
- Metody rozšíření umožní vývojářům rozšířit typy v jiných sestaveních k účasti v `using`ch příkazech.

V situaci, kdy je možné typ implicitně převést na `IDisposable` a také se doplňuje vzorem na jedno místo, `IDisposable` přednostně. I když tato akce trvá opačný přístup k `foreach` (upřednostňovaný vzor přes rozhraní), je nezbytné pro zpětnou kompatibilitu.

V tomto případě platí stejná omezení z tradičního příkazu `using`. místní proměnné deklarované v `using` jsou jen pro čtení, `null` hodnota nezpůsobí vyvolání výjimky atd... Generování kódu se bude lišit pouze v tom, že před voláním metody Dispose nebude přetypování na `IDisposable`.

```csharp
{
      Resource r = new Resource();
      try {
            // statements
      }
      finally {
            if (resource != null) resource.Dispose();
      }
}
```

Aby bylo možné přizpůsobit vzor na více než jedno, `Dispose` metoda musí být přístupná, bez parametrů a musí mít `void` návratový typ. Neexistují žádná další omezení. To explicitně znamená, že zde lze použít rozšiřující metody.

## <a name="considerations"></a>Požadavky

### <a name="case-labels-without-blocks"></a>jmenovky Case bez bloků

`using declaration` je nepřípustný přímo v rámci `case` popisku, protože se jedná o komplikace kolem skutečné životnosti. Jedním z možných řešení je jednoduše poskytnout stejnou životnost jako `out var` ve stejném umístění. Pro implementaci funkcí se považovala za zvláštní složitost a snadno se rozhlasí (stačí přidat blok na `case` popisku), který tuto trasu neodůvodňuje.

## <a name="future-expansions"></a>Budoucí rozšíření

### <a name="fixed-locals"></a>pevná národní prostředí

Příkaz `fixed` obsahuje všechny vlastnosti `using` příkazů, které motivují schopnost mít `using` národní prostředí. Mělo by se zvážit, že by se tato funkce roz`fixed`a i na místní prostředí. Pravidla doby platnosti a řazení by se měla použít stejně dobře pro `using` a `fixed`.
