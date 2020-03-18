---
ms.openlocfilehash: 63dfdfee9ea6c16e162f483aa1298feed297daef
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484697"
---
# <a name="conditional-ref-expressions"></a>Podmíněné výrazy ref

Vzor vazby proměnné ref na jeden nebo jiný výraz podmíněně není aktuálně vyhodnotit v C#.

Typickým alternativním řešením je zavedení metody jako:

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

Všimněte si, že se nejedná o přesné nahrazení Ternární, protože všechny argumenty musí být vyhodnoceny na webu volání.

Následující nebudou fungovat podle očekávání:

```csharp
       // will crash with NRE because 'arr[0]' will be executed unconditionally
      ref var r = ref Choice(arr != null, ref arr[0], ref otherArr[0]);
```

Navrhovaná syntaxe by vypadala takto:

```csharp
     <condition> ? ref <consequence> : ref <alternative>;
```

Výše uvedený pokus s možností "Choice" lze _správně_ zapsat pomocí odkazu Ternární jako:

```csharp
     ref var r = ref (arr != null ? ref arr[0]: ref otherArr[0]);
```

Rozdílem od volby je, že k sobě a alternativním výrazům se přistupoval v _skutečně_ podmíněném způsobu, takže se při ```arr == null``` nezobrazuje chyba.

Ternární odkaz je pouze Ternární, pokud je alternativa i i pořadím ReFS. Bude přirozeným způsobem vyžadovat, aby byly hodnoty lvalue nebo alternativní operandy. Bude také vyžadovat, aby v důsledku i v sobě byly typy, které jsou vzájemně převoditelné na sebe.

Typ výrazu bude vypočítán podobně jako pro regulární Ternární výraz. t. v případě, že je souběžnost a alternativa převoditelné identity, ale různé typy, budou platit existující pravidla sloučení typů.

Z podmíněných operandů se bude s bezpečným vrácením předpokládat konzervativní přístup. Pokud buď není bezpečné vracet celou věc, nebudete se moct vrátit.

Odkaz Ternární je l-hodnota a jako takový je možné ho předat/přiřadit/vrátit odkazem.

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
```

Odkaz Ternární lze použít také v běžném (nikoli ref) kontextu. I když by nedošlo k běžnému použití, protože byste mohli také pouze použít regulární Ternární.

```csharp
     int x = (arr != null ? ref arr[0]: ref otherArr[0]);
```


___

Poznámky k implementaci: 

Složitost implementace by znamenala velikost chyby střední až velké. -I. E není velmi nákladný.
Nevím, že potřebujeme jakékoli změny syntaxe nebo analýzy.
Neexistuje žádný vliv na metadata nebo interoperabilitu. Tato funkce je zcela založená na výrazu.
Žádný vliv na ladění/PDB buď
