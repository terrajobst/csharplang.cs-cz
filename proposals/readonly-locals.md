---
ms.openlocfilehash: 922353d043653ddb651534a01f3fb98f85cd756e
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484480"
---
# <a name="readonly-locals-and-parameters"></a>lokální hodnoty a parametry jen pro čtení

* Navrženo [x]
* [] Prototyp
* [] Implementace
* [] Specifikace

## <a name="summary"></a>Souhrn
[summary]: #summary

Umožňuje, aby se lokální hodnoty a parametry pomohly označit jako jen pro čtení, aby nedošlo k neomezené mutace těchto národních prostředí a parametrů.

## <a name="motivation"></a>Motivační
[motivation]: #motivation

V dnešní době lze klíčové slovo `readonly` použít u polí. to má vliv na to, že pole může být zapsáno pouze během konstrukce (statické konstrukce v případě statického pole nebo konstrukce instance v případě pole instance), což pomáhá vývojářům vyhnout se chybám omylem při náhodném přepsání stavu, který by neměl být změněn. Ale pole nejsou jenom vývojáři, aby měli jistotu, že se hodnoty nepoužijí. Konkrétně je běžné vytvořit místní proměnnou pro uložení dočasného stavu a neúmyslná aktualizace tohoto dočasného stavu může mít za následek chybné výpočty a další takové chyby, zejména v případě, že jsou tato "národní prostředí" zachycena ve výrazech lambda, v tom případě jsou převolána do polí, ale již není dnes označena jako `readonly`.

## <a name="detailed-design"></a>Podrobný návrh
[design]: #detailed-design

Národní prostředí bude možné opatřit poznámkami jako `readonly` i v případě, že kompilátor zajistí, že jsou nastaveni pouze v okamžiku deklarace (Některá národní prostředí C# jsou již implicitně určena jen pro čtení, jako je například proměnná iterace ve smyčce foreach nebo použitá proměnná v bloku using, ale v současné době vývojář nemá schopnost označit jiné národní prostředí jako `readonly`). Taková `readonly`ová národní prostředí musí mít inicializátor:

```csharp
readonly long maxBytesToDelete = (stream.LimitBytes - stream.MaxBytes) / 10;
...
maxBytesToDelete = 0; // Error: can't assign to readonly locals outside of declaration
```

A jako zkrácený pro `readonly var`mohou být použity existující kontextové klíčové slovo `let`, např.

```csharp
let maxBytesToDelete = (stream.LimitBytes - stream.MaxBytes) / 10;
...
maxBytesToDelete = 0; // Error: can't assign to readonly locals outside of declaration
```

Neexistují žádná zvláštní omezení pro to, co může být inicializátorem, a může to být cokoli, co je aktuálně platné jako inicializátor pro národní prostředí, např.

```csharp
readonly T data = arg1 ?? arg2;
```

`readonly` v lokálních hodnotách je obzvláště užitečná při práci s lambda a uzávěry. Když anonymní metoda nebo lambda přistupuje k místnímu stavu z ohraničujícího oboru, je tento stav zachycen do uzavření kompilátorem, který je reprezentován "zobrazovací třídou".  Každé "místní" zachycené pole je v této třídě, a to proto, že kompilátor generuje toto pole vaším jménem, nemáte žádnou příležitost k tomu, aby ji napsal jako `readonly`, aby nedošlo k chybnému zápisu do "místní" (v uvozovkách, protože to není místní, alespoň ve výsledném jazyce MSIL). V `readonly` národní prostředí může kompilátor zabránit v zápisu do místní, což je obzvláště důležité ve scénářích, které se týkají multithreadingy, kdy chybný zápis by mohl mít za následek nebezpečnou, ale vzácnou chybu souběžného vyhledávání.

```csharp
readonly long index = ...;
Parallel.ForEach(data, item => {
    T element = item[index];
    index = 0; // Error: can't assign to readonly locals outside of declaration
});
```

V rámci speciální formy místních parametrů budou také anotace `readonly`. To by mělo mít žádný vliv na to, co volající metody může předat parametr (stejně jako není žádné omezení, které hodnoty mohou být uloženy do pole `readonly`), ale stejně jako u jakýchkoli `readonly` místních, kompilátor zakáže zápis kódu do parametru po deklaraci, což znamená, že tělo metody je zakázáno zapisovat do parametru.

```csharp
public void Update(readonly int index = 0) // Default values are ok though not required
{
    ...
    index = 0; // Error: can't assign to readonly parameters
    ...
}
```

parametry `readonly` neovlivňují signaturu nebo metadata vypouštěné kompilátorem pro danou metodu a jednoduše ovlivňují, jak kompilátor zpracovává kompilaci těla metody. Například základní virtuální metoda může mít parametr `readonly` a tento parametr by mohl zapisovat do přepsání.

Stejně jako u polí jsou `readonly` pro lokální hodnoty a parametry omezené, ovlivnění umístění úložiště, ale ne bez jakýchkoli průjezdů, což by mělo vliv na graf objektů. Nicméně stejně jako u polí, volání metody na `readonly` místní nebo parametr struct, ve skutečnosti vytvoří kopii struktury a zavolá metodu na kopii, aby nedošlo k vnitřní mutace `this`.

`readonly` lokální hodnoty a parametry nelze předat jako `ref` nebo `out` argumenty, pokud `ref readonly` i/dokud není podporována.

## <a name="alternatives"></a>Alternativy
[alternatives]: #alternatives

- `val` lze použít jako alternativní zkratku pro `let`.

## <a name="unresolved-questions"></a>Nevyřešené dotazy
[unresolved]: #unresolved-questions

- `readonly ref` / `ref readonly` / `readonly ref readonly`: Mám na začátku otázky, jak s tímto návrhem pracovat `ref readonly` jako samostatný.
- Tento návrh netýká struktur jen pro čtení nebo neměnných typů. To je ponecháno pro samostatný návrh.

## <a name="design-meetings"></a>Schůzky pro návrh

- Stručný popis: 21. ledna 2015 (<https://github.com/dotnet/roslyn/issues/98>)
