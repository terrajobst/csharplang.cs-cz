---
ms.openlocfilehash: b8d975a8fc95af6980feaae6be35160646fe2cd2
ms.sourcegitcommit: 32abf01f2e43be29114bfcf8f8ed1cc4e3eaded2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/07/2019
ms.locfileid: "79485138"
---
# <a name="permit-stackalloc-in-nested-contexts"></a>Povolit `stackalloc` ve vnořených kontextech

### <a name="stack-allocation"></a>Přidělení zásobníku

Změnou [*alokace zásobníku*](https://github.com/dotnet/csharplang/blob/master/spec/unsafe-code.md#stack-allocation) oddílu specifikace C# jazyka se dá uvolnit místo, kde se může zobrazit výraz `stackalloc`. Odstraníme

``` antlr
local_variable_initializer_unsafe
    : stackalloc_initializer
    ;

stackalloc_initializer
    : 'stackalloc' unmanaged_type '[' expression ']'
    ;
```

a nahraďte je

``` antlr
primary_no_array_creation_expression
    : stackalloc_initializer
    ;

stackalloc_initializer
    : 'stackalloc' unmanaged_type '[' expression? ']' array_initializer?
    | 'stackalloc' '[' expression? ']' array_initializer
    ;
```

Všimněte si, že přidání *array_initializer* do *stackalloc_initializer* (a vytvoření výrazu indexu volitelné) bylo [rozšíření v C# 7,3](https://github.com/dotnet/csharplang/blob/master/proposals/csharp-7.3/stackalloc-array-initializers.md) a není popsané zde.

*Typ elementu* `stackalloc` výrazu je *unmanaged_type* s názvem ve výrazu stackalloc, pokud existuje, nebo společný typ mezi prvky *array_initializer* jinak.

Typ *stackalloc_initializer* s *typem elementu* `K` závisí na jeho syntaktickém kontextu:
- Pokud se *stackalloc_initializer* zobrazí přímo jako *local_variable_initializer* příkazu *local_variable_declaration* nebo *for_initializer*, pak jeho typ je `K*`.
- V opačném případě je jeho typ `System.Span<K>`.

### <a name="stackalloc-conversion"></a>Stackalloc převod

*Převod stackalloc* je nový vestavěný implicitní převod z výrazu. Pokud je typ *stackalloc_initializer* `K*`, existuje implicitní *převod stackalloc* z *stackalloc_initializer* na typ `System.Span<K>`.
