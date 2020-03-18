---
ms.openlocfilehash: c1a77d9337ecd16f5ea1c30d18f6422552b0dfb2
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484599"
---
# <a name="ref-local-reassignment"></a>Místní opětovné přiřazení odkazu

V C# 7,3 přidáváme podporu pro převázání referenční místní proměnné ref nebo ref Parameter.

Do sady `assignment_operator`s přidáme následující:

```antlr
assignment_operator
    : '=' 'ref'
    ;
```

Operátor `=ref` se nazývá ***operátor přiřazení ref***. Nejedná se o *složený operátor přiřazení*. Levý operand musí být výraz, který se váže na místní proměnnou ref, parametr ref (jiný než `this`) nebo výstupní parametr. Pravý operand musí být výraz, který je výsledkem určení hodnoty stejného typu jako levý operand.

Pravý operand musí být jednoznačně přiřazen v bodě přiřazení ref.

Když se levý operand připojí k parametru `out`, jedná se o chybu, pokud se tento parametr `out` nejednoznačně přiřadil na začátku operátoru přiřazení ref.

Pokud je levý operand zapisovatelný odkazem (tj. označuje cokoli jiného než `ref readonly` místní nebo `in` parametr), pravý operand musí být zapisovatelnou hodnotou lvalue.

Operátor přiřazení ref vrací l-hodnotu přiřazeného typu. Dá se zapisovat, pokud je levý operand zapisovatelný (tj. není `ref readonly` nebo `in`).

Bezpečnostní pravidla pro tento operátor jsou:

- Pro `e1 = ref e2`pro změnu přiřazení ref musí být `e2` s označením *ref-to-Escape* přinejmenším v rámci rozsahu, který je *bezpečný pro přístup k referenčnímu znaku* `e1`.

Kde *ref-Safe-to-Escape* je definováno v [bezpečí pro typy s odkazem na typ odkazu](../csharp-7.2/span-safety.md)
