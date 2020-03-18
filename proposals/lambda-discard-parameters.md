---
ms.openlocfilehash: 6695664c3d5ca73f66e792b7ce2ec9993aceea05
ms.sourcegitcommit: 42ef673ecc883009c865f8384249881a546df216
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/14/2019
ms.locfileid: "79485152"
---
# <a name="lambda-discard-parameters"></a>Parametry lambda zahození

## <a name="summary"></a>Souhrn

Povolí použití zahození (`_`) jako parametrů výrazů lambda a anonymních metod.
Příklad:
- výrazy lambda: `(_, _) => 0`, `(int _, int _) => 0`
- anonymní metody: `delegate(int _, int _) { return 0; }`

## <a name="motivation"></a>Motivační

Nepoužívané parametry není nutné pojmenovat. Záměr výmětů je jasný, to znamená, že se nepoužívá nebo zahodí.

## <a name="detailed-design"></a>Podrobný návrh

[Parametry metody](https://github.com/dotnet/csharplang/blob/master/spec/classes.md#method-parameters) V seznamu parametrů lambda nebo anonymní metody s více než jedním parametrem s názvem `_`, jsou tyto parametry zahozeny parametry.
Poznámka: Pokud je jeden parametr pojmenován `_` pak se jedná o regulární parametr z důvodů zpětné kompatibility.

Zahození parametrů nezavádí žádné názvy do žádných oborů.
Všimněte si, že to znamená, že nezpůsobí skryté názvy `_` (podtržítka).

[Jednoduché názvy](https://github.com/dotnet/csharplang/blob/master/spec/expressions.md#simple-names) Pokud je `K` nula a *simple_name* se objeví v rámci *bloku* *a v případě*, že prostor deklarace místní[proměnné (nebo](basic-concepts.md#declarations)ohraničujícího *bloku*) obsahuje místní proměnnou, parametr (s výjimkou parametrů zahození) nebo konstantu s názvem `I`, *simple_name* odkazuje na tuto místní proměnnou, parametr nebo konstantu a je klasifikována jako proměnná nebo hodnota.

[Rozsahy](https://github.com/dotnet/csharplang/blob/master/spec/basic-concepts.md#scopes) S výjimkou parametrů zahození je rozsah parametru deklarovaného v *lambda_expression* ([anonymní výrazy funkce](expressions.md#anonymous-function-expressions)) *anonymous_function_body* tohoto *lambda_expression* s výjimkou parametrů zahození, obor parametru deklarovaného v *anonymous_method_expression* ([anonymní výrazy funkce](expressions.md#anonymous-function-expressions)) je *blok* tohoto *anonymous_method_expression*.

## <a name="related-spec-sections"></a>Související oddíly specifikace
- [Odpovídající parametry](https://github.com/dotnet/csharplang/blob/master/spec/expressions.md#corresponding-parameters)
