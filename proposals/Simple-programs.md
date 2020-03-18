---
ms.openlocfilehash: b9697fc1d772ba59ed3b1de339a5a3d4eb24b1bd
ms.sourcegitcommit: 36b028f4d6e88bd7d4a843c6d384d1b63cc73334
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/19/2020
ms.locfileid: "79485222"
---
# <a name="simple-programs"></a>Jednoduché programy

* Navrženo [x]
* [x] prototyp: spuštěno
* [] Implementace: Nezahájeno
* [] Specifikace: Nezahájeno

## <a name="summary"></a>Souhrn
[summary]: #summary

Povolí sekvenci *příkazů* , které se mají provést přímo před *namespace_member_declaration*s *compilation_unit* (tj. zdrojový soubor).

Sémantika znamená, že pokud je taková sekvence *příkazů* přítomna, bude vyvolána následující deklarace typu (modulo) skutečný název typu a název metody.

``` c#
static class Program
{
    static async Task Main()
    {
        // statements
    }
}
```

Viz také https://github.com/dotnet/csharplang/issues/3117.

## <a name="motivation"></a>Motivační
[motivation]: #motivation

K dispozici je určité množství často používaného textu, a to i v případě, že je potřeba explicitní `Main` metoda. Zdá se, že se dostanete na základě jazyka učení a srozumitelnosti programu. Hlavním cílem této funkce je tedy dovolit programům bez C# zbytečného vynechání kolem nich, a to v případě učících a srozumitelnosti kódu.

## <a name="detailed-design"></a>Podrobný návrh
[design]: #detailed-design

### <a name="syntax"></a>Syntaxe

Jedinou další syntaxí je povolení sekvence *příkazů*v kompilační jednotce těsně před *namespace_member_declaration*s:

``` antlr
compilation_unit
    : extern_alias_directive* using_directive* global_attributes? statement* namespace_member_declaration*
    ;
```

Ve všech, ale v jednom *compilation_unit* *příkaz*s musí být všechny deklarace místní funkce. 

Příklad:

``` c#
// File 1 - any statements
if (args.Length == 0
    || !int.TryParse(args[0], out int n)
    || n < 0) return;
Console.WriteLine(Fib(n).curr);

// File 2 - only local functions
(int curr, int prev) Fib(int i)
{
    if (i == 0) return (1, 0);
    var (curr, prev) = Fib(i - 1);
    return (curr + prev, curr);
}
```

### <a name="semantics"></a>Sémantiku

Pokud jsou jakékoli příkazy nejvyšší úrovně přítomny v jakékoli kompilační jednotce programu, je význam, jako kdyby byly kombinovány v těle bloku `Main` metody `Program` třídy v globálním oboru názvů následujícím způsobem:

``` c#
static class Program
{
    static async Task Main()
    {
        // File 1 statements
        // File 2 local functions
        // ...
    }
}
```

Všimněte si, že názvy "program" a "Main" jsou používány pouze pro ilustraci, skutečné názvy používané kompilátorem jsou závislé na implementaci a ani typ ani na metodu nelze odkazovat pomocí názvu ze zdrojového kódu.

Metoda je určena jako vstupní bod programu. Explicitně deklarované metody, které by mohly být považovány za kandidáty vstupního bodu, se ignorují. V případě, že k tomu dojde, je hlášeno upozornění. Je-li zadán `-main:<type>` přepínač kompilátoru, jedná se o chybu.

Pokud některá jednotka kompilace obsahuje příkazy jiné než deklarace místní funkce, napřed příkazy z této jednotky kompilace nastávají. To způsobí, že je právní pro místní funkce v jednom souboru, aby odkazovaly na lokální proměnné v jiném. Pořadí příspěvků příkazu (které by měly být místní funkce) z jiných kompilačních jednotek, není definováno.

Asynchronní operace jsou povoleny v příkazech nejvyšší úrovně do rozsahu, v němž jsou povoleny v příkazech v rámci běžné metody asynchronního vstupního bodu. Nejsou však požadovány, pokud `await` výrazy a jiné asynchronní operace vynechány, není vytvořeno žádné upozornění. Místo toho je podpis generované metody vstupního bodu ekvivalentní 
``` c#
    static void Main()
```

Výše uvedený příklad by vrátil následující `$Main` deklaraci metody:

``` c#
static class $Program
{
    static void $Main()
    {
        // Statements from File 1
        if (args.Length == 0
            || !int.TryParse(args[0], out int n)
            || n < 0) return;
        Console.WriteLine(Fib(n).curr);
        
        // Local functions from File 2
        (int curr, int prev) Fib(int i)
        {
            if (i == 0) return (1, 0);
            var (curr, prev) = Fib(i - 1);
            return (curr + prev, curr);
        }
    }
}
```

Ve stejnou dobu jako příklad:
``` c#
// File 1
await System.Threading.Tasks.Task.Delay(1000);
System.Console.WriteLine("Hi!");
```

by to vedlo k těmto akcím:
``` c#
static class $Program
{
    static async Task $Main()
    {
        // Statements from File 1
        await System.Threading.Tasks.Task.Delay(1000);
        System.Console.WriteLine("Hi!");
    }
}
```

### <a name="scope-of-top-level-local-variables-and-local-functions"></a>Rozsah místních proměnných nejvyšší úrovně a místních funkcí

I když jsou lokální proměnné a funkce nejvyšší úrovně "zabalené" do generované metody vstupního bodu, měly by být stále v rozsahu celého programu.
Pro účely vyhodnocení jednoduchého názvu po dosažení globálního oboru názvů:
- Nejprve se provede pokus o vyhodnocení názvu v rámci generované metody vstupního bodu a jenom v případě, že se tento pokus nezdaří. 
- Vyhodnocování "regular" v rámci deklarace globálního oboru názvů je provedeno. 

To může vést k vytvoření stínové kopie oborů názvů a typů deklarovaných v globálním oboru názvů a také k vytváření stínů importovaných názvů.

Pokud se k vyhodnocení jednoduchého názvu dochází mimo příkazy nejvyšší úrovně a vyhodnocení má za následek místní proměnnou nebo funkci nejvyšší úrovně, která by měla vést k chybě.

Tímto způsobem chráníme naši budoucí schopnost lépe řešit "funkce nejvyšší úrovně" (scénář 2 v https://github.com/dotnet/csharplang/issues/3117)a je možné poskytnout užitečnou diagnostiku uživatelům, kteří se omylem domnívají, že jsou podporované.

