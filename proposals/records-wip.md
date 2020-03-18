---
ms.openlocfilehash: 25756c1811d5e6dc97512ce70f99ab7fefa91c4a
ms.sourcegitcommit: 2a6dffb60718065ece95df75e1cc7eb509e48a8d
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/01/2020
ms.locfileid: "79485236"
---
# <a name="records-work-in-progress"></a>Zaznamenává probíhající práci.

Na rozdíl od ostatních návrhů se nejedná o návrh sám o sobě, ale průběh práce navržený k zaznamenání rozhodnutí o návrhu konsensu pro funkci záznamů. Podrobnosti specifikace budou přidány podle potřeby pro řešení otázek.

Syntaxe pro záznam je navržena tak, aby se přidala takto:

```antlr
class_declaration
    : attributes? class_modifiers? 'partial'? 'class' identifier type_parameter_list?
      parameter_list? type_parameter_constraints_clauses? class_body
    ;

struct_declaration
    : attributes? struct_modifiers? 'partial'? 'struct' identifier type_parameter_list?
      parameter_list? struct_interfaces? type_parameter_constraints_clauses? struct_body
    ;

class_body
    : '{' class_member_declarations? '}'
    | ';'
    ;

struct_body
    : '{' struct_members_declarations? '}'
    | ';'
    ;
```

`attributes` bez terminálu taky umožní nový kontextový atribut `data`.

Třída (struct) deklarovaná se seznamem parametrů nebo modifikátorem `data` se označuje jako třída záznamu (struktura záznamu), z nichž jeden je typ záznamu.

Deklarace typu záznamu bez seznamu parametrů a modifikátoru `data` je chybná.

## <a name="members-of-a-record-type"></a>Členové typu záznamu

Kromě členů deklarovaných v těle třídy má typ záznamu následující další členy:

### <a name="primary-constructor"></a>Primární konstruktor

Typ záznamu má veřejný konstruktor, jehož signatura odpovídá parametrům hodnoty deklarace typu. Označuje se jako primární konstruktor pro daný typ a způsobí, že implicitně deklarovaný výchozí konstruktor bude potlačen. Jedná se o chybu pro primární konstruktor a konstruktor se stejným podpisem již ve třídě existuje.
Za běhu primárního konstruktoru 

1. spustí Inicializátory pole instance uvedené v těle třídy; a potom vyvolá konstruktor základní třídy bez argumentů.

1. Inicializuje pole zálohování generované kompilátorem pro vlastnosti odpovídající parametrům hodnoty (pokud jsou tyto vlastnosti zadány kompilátorem, viz [syntetizované vlastnosti](#Synthesized Properties))


[] TODO: přidat syntaxi a specifikaci základního volání pro výběr základního konstruktoru prostřednictvím řešení přetížení

### <a name="properties"></a>Vlastnosti

Pro každý parametr záznamu deklarace typu záznamu je k dispozici odpovídající člen veřejné vlastnosti, jehož název a typ jsou přijímány z deklarace parametru value. Pokud žádná konkrétní (tj. neabstraktní) vlastnost s přístupovým objektem Get a s tímto názvem a typem je explicitně deklarována nebo zděděna, je vytvořena kompilátorem následujícím způsobem:

Pro strukturu záznamu nebo třídu záznamu:

* Je vytvořena veřejná vlastnost pouze pro získání. Jeho hodnota je inicializována během konstrukce s hodnotou odpovídajícího primárního parametru konstruktoru. Všechna "párové" zděděná přístupová metoda get abstraktních vlastností je přepsána.

### <a name="equality-members"></a>Členové rovnosti

Typy záznamů poskytují syntetizované implementace následujících metod:

* `object.GetHashCode()` přepsat, pokud není zapečetěné nebo zadáno uživatelem
* `object.Equals(object)` přepsat, pokud není zapečetěné nebo zadáno uživatelem
* `T Equals(T)` metoda, kde `T` je aktuální typ

`T Equals(T)` je určen k provádění rovnosti hodnot, porovnává vlastnost se stejným názvem jako každý parametr primárního konstruktoru s odpovídající vlastností druhého typu.
`object.Equals` provádí ekvivalent

```C#
override Equals(object o) => Equals(o as T);
```
