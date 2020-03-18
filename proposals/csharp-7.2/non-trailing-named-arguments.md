---
ms.openlocfilehash: ac2b233eb703b5eea3bd2dfdbeeadd7494b0c695
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484669"
---
# <a name="non-trailing-named-arguments"></a>Nekoncové pojmenované argumenty

## <a name="summary"></a>Souhrn
[summary]: #summary
Povolí použití pojmenovaných argumentů v nekoncové pozici, pokud se používají na jejich správné pozici. Například: `DoSomething(isEmployed:true, name, age);`.

## <a name="motivation"></a>Motivační
[motivation]: #motivation

Hlavní motivace je zabránit psaní redundantních informací. Je běžné pojmenovat argument, který je literál (například `null`, `true`) pro účely objasnění kódu namísto předávání argumentů mimo pořadí.
To je momentálně zakázáno (`CS1738`), pokud nejsou všechny následující argumenty také pojmenovány.

```csharp
DoSomething(isEmployed:true, name, age); // currently disallowed, even though all arguments are in position
// CS1738 "Named argument specifications must appear after all fixed arguments have been specified"
```

Některé další příklady:
```csharp
public void DoSomething(bool isEmployed, string personName, int personAge) { ... }

DoSomething(isEmployed:true, name, age); // currently CS1738, but would become legal
DoSomething(true, personName:name, age); // currently CS1738, but would become legal
DoSomething(name, isEmployed:true, age); // remains illegal
DoSomething(name, age, isEmployed:true); // remains illegal
DoSomething(true, personAge:age, personName:name); // already legal
```

To by také mohlo fungovat s parametry:
```csharp
public class Task
{
    public static Task When(TaskStatus all, TaskStatus any, params Task[] tasks);
}
Task.When(all: TaskStatus.RanToCompletion, any: TaskStatus.Faulted, task1, task2)
```

## <a name="detailed-design"></a>Podrobný návrh
[design]: #detailed-design

V § 7.5.1 (seznamy argumentů) aktuálně uvedená specifikace uvádí:
> *Argument* s *argumentem-name* je označován jako __pojmenovaný argument__, zatímco *Argument* bez *argumentu-Name* je __poziční argument__. Je-li pozice argumentu uvedena po pojmenovaném argumentu v *seznamu argumentů*, jedná se o chybu.

Účelem tohoto návrhu je odebrat tuto chybu a aktualizovat pravidla pro hledání odpovídajícího parametru argumentu (§ 7.5.1.1):

Argumenty v argumentu – seznam konstruktorů instancí, metod, indexerů a delegátů:
- [existující pravidla]
- Nepojmenovaný argument odpovídá žádnému parametru, je-li za názvem pojmenovaného argumentu nebo pojmenovaným param.

Konkrétně to brání vyvolání `void M(bool a = true, bool b = true, bool c = true, );` s `M(c: false, valueB);`. První argument se používá mimo umístění (argument se používá jako první pozice, ale parametr s názvem "c" je na třetí pozici), takže by měly být pojmenovány následující argumenty.

Jinými slovy, nekoncové pojmenované argumenty jsou povoleny pouze v případě, že název a pozice mají za následek vyhledání stejného odpovídajícího parametru.

## <a name="drawbacks"></a>Nevýhody
[drawbacks]: #drawbacks

Tento návrh exacerbates existující odlišností s pojmenovanými argumenty v řešení přetížení. Příklad:

```csharp
void M(int x, int y) { }
void M<T>(T y, int x) { }

void M2()
{
    M(3, 4);
    M(y: 3, x: 4); // Invokes M(int, int)
    M(y: 3, 4); // Invokes M<T>(T, int)
}
```

Tuto situaci můžete získat ještě dnes zahozením parametrů:

```csharp
void M(int y, int x) { }
void M<T>(int x, T y) { }

void M2()
{
    M(3, 4);
    M(x: 3, y: 4); // Invokes M(int, int)
    M(3, y: 4); // Invokes M<T>(int, T)
}
```

Podobně, pokud máte dvě metody `void M(int a, int b)` a `void M(int x, string y)`, bude chybné vyvolání `M(x: 1, 2)` vyvolat diagnostiku na základě druhého přetížení ("nelze převést z ' int ' na ' String '). Tento problém již existuje, pokud je pojmenovaný argument použit na koncové pozici.

## <a name="alternatives"></a>Alternativy
[alternatives]: #alternatives

Je potřeba vzít v úvahu několik alternativ:

- Stav quo
- Poskytnutí pomoci IDE pro vyplňování všech názvů koncových argumentů při psaní konkrétního názvu uprostřed.

Obě z nich jsou z větší podrobností, protože zavádějí více pojmenovaných argumentů i v případě, že pouze potřebujete pouze jeden název literálu na začátku seznamu argumentů.

## <a name="unresolved-questions"></a>Nevyřešené dotazy
[unresolved]: #unresolved-questions

## <a name="design-meetings"></a>Schůzky pro návrh
[ldm]: #ldm
Tato funkce byla stručně popsána v nástroji LDM v případě 16. května 2017 s schválením v principu (pro přechod na návrh/prototyp). Byla také stručně popsána v červnu 28 2017.

Vztahuje se na úvodní diskuzi https://github.com/dotnet/csharplang/issues/518 vztah k problému s championed https://github.com/dotnet/csharplang/issues/570
