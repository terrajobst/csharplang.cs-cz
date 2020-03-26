---
ms.openlocfilehash: 54ae4ffabde6dca49b7e6bfb626d65837eabc8f5
ms.sourcegitcommit: 1e1c7c72b156e2fbc54d6d6ac8d21bca9934d8d2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/26/2020
ms.locfileid: "80281941"
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

Pouze jeden *compilation_unit* může mít příkaz s *příkazem*. 

Příklad:

``` c#
if (System.Environment.CommandLine.Length == 0
    || !int.TryParse(System.Environment.CommandLine, out int n)
    || n < 0) return;
Console.WriteLine(Fib(n).curr);

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
        // statements
    }
}
```

Všimněte si, že názvy "program" a "Main" jsou používány pouze pro ilustraci, skutečné názvy používané kompilátorem jsou závislé na implementaci a ani typ ani na metodu nelze odkazovat pomocí názvu ze zdrojového kódu.

Metoda je určena jako vstupní bod programu. Explicitně deklarované metody, které by mohly být považovány za kandidáty vstupního bodu, se ignorují. V případě, že k tomu dojde, je hlášeno upozornění. Pokud existují příkazy na nejvyšší úrovni, při určení `-main:<type>` přepínač kompilátoru se jedná o chybu.

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
        if (System.Environment.CommandLine.Length == 0
            || !int.TryParse(System.Environment.CommandLine, out int n)
            || n < 0) return;
        Console.WriteLine(Fib(n).curr);
        
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
await System.Threading.Tasks.Task.Delay(1000);
System.Console.WriteLine("Hi!");
```

by to vedlo k těmto akcím:
``` c#
static class $Program
{
    static async Task $Main()
    {
        await System.Threading.Tasks.Task.Delay(1000);
        System.Console.WriteLine("Hi!");
    }
}
```

### <a name="scope-of-top-level-local-variables-and-local-functions"></a>Rozsah místních proměnných nejvyšší úrovně a místních funkcí

I když jsou lokální proměnné a funkce nejvyšší úrovně "zabalené" do generované metody vstupního bodu, měly by být stále v rozsahu v rámci programu v každé kompilační jednotce.
Pro účely vyhodnocení jednoduchého názvu po dosažení globálního oboru názvů:
- Nejprve se provede pokus o vyhodnocení názvu v rámci generované metody vstupního bodu a jenom v případě, že se tento pokus nezdaří. 
- Vyhodnocování "regular" v rámci deklarace globálního oboru názvů je provedeno. 

To může vést k vytvoření stínové kopie oborů názvů a typů deklarovaných v globálním oboru názvů a také k vytváření stínů importovaných názvů.

Pokud se k vyhodnocení jednoduchého názvu dochází mimo příkazy nejvyšší úrovně a vyhodnocení má za následek místní proměnnou nebo funkci nejvyšší úrovně, která by měla vést k chybě.

Tímto způsobem chráníme naši budoucí schopnost lépe řešit "funkce nejvyšší úrovně" (scénář 2 v https://github.com/dotnet/csharplang/issues/3117)a je možné poskytnout užitečnou diagnostiku uživatelům, kteří se omylem domnívají, že jsou podporované.

