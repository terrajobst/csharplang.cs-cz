---
ms.openlocfilehash: 75fcd5b00ea5cac218a9f7809c53b179df97825c
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/25/2019
ms.locfileid: "64488937"
---
# <a name="exceptions"></a>Výjimky

Výjimky v jazyce C# zadejte strukturovaných, jednotné a typově bezpečný způsob zpracování systémové úrovni a úrovni aplikace chybové stavy. Mechanismus výjimek v jazyce C# je podobný, C++, s několika rozdíly:

*  V jazyce C#, všechny výjimky musí být reprezentovaný instancí typu třídy odvozené z `System.Exception`. V jazyce C++ lze použít libovolnou hodnotu libovolného typu představující výjimku.
*  V jazyce C# bloku finally ([příkazu try](statements.md#the-try-statement)) můžete použít pro zapsání ukončovací kód, který se spustí v normálním spuštění a výjimečných podmínek. Takový kód je obtížné je napsat bez duplikování kódu v jazyce C++.
*  V jazyce C# výjimek na úrovni systému, jako je přetečení, dělení nulou a null přístupů přes ukazatel, jasně definovaných tříd výjimek a jsou na stejné úrovni podmínky chyby na úrovni aplikace.

## <a name="causes-of-exceptions"></a>Příčinami výjimek

Výjimky mohou být vyvolány dvěma různými způsoby.

*  A `throw` – příkaz ([příkazu throw](statements.md#the-throw-statement)) vyvolá výjimku, okamžitě a nepodmíněně. Ovládací prvek nikdy dosáhne příkazu za příkazem okamžitě `throw`.
*  Při operaci nelze dokončit, obvykle některých výjimečných podmínek, které vznikají při zpracování příkazů jazyka C# a výrazu způsobit výjimku za určitých okolností. Například operace dělení celého čísla ([operátor dělení](expressions.md#division-operator)) vyvolá `System.DivideByZeroException` je-li jmenovatelem nula. Zobrazit [běžné třídy výjimek](exceptions.md#common-exception-classes) seznam různé výjimky, které se mohou vyskytnout tímto způsobem.

## <a name="the-systemexception-class"></a>Třídy System.Exception

`System.Exception` Třída je základní typ pro všechny výjimky. Tato třída obsahuje několik významných vlastností, které sdílejí všechny výjimky:

*  `Message` je vlastnost jen pro čtení typu `string` , který obsahuje čitelný popis důvod výjimky.
*  `InnerException` je vlastnost jen pro čtení typu `Exception`. Pokud její hodnota je jiná než null, odkazuje na výjimku, která způsobila aktuální výjimku – to znamená, že byla aktuální výjimka vyvolána v bloku catch zpracování `InnerException`. Jinak jeho hodnota je null, která znamená, že nebyl touto výjimkou způsobeno výjimkou jiný. Může být libovolný počet objektů výjimek zřetězit tímto způsobem.

Hodnoty těchto vlastností lze zadat ve volání do konstruktoru instance pro `System.Exception`.

## <a name="how-exceptions-are-handled"></a>Způsob zpracování výjimek

Jsou výjimky zpracovávány `try` – příkaz ([příkazu try](statements.md#the-try-statement)).

Když dojde k výjimce, systém vyhledá nejbližší `catch` klauzuli, která dokáže zpracovat výjimky, počítáno od run-time typu výjimky. Nejprve je prohledána aktuální metoda lexikálně uzavírající `try` příkazu a klauzulích přidruženým blokem catch příkazu try jsou považovány za v pořadí. Pokud se nezdaří, metoda, která volá aktuální metoda prohledána lexikálně uzavírající `try` příkaz, který obklopuje místa volání na aktuální metodu. Hledání pokračuje, dokud `catch` klauzule se nachází na aktuální výjimku, která dokáže zpracovat pojmenováním třídu výjimky, která má stejné třídy nebo základní třídu typu vyvolání výjimky za běhu. A `catch` klauzuli, která není název třídy výjimek může zpracovávat všechny výjimky.

Po nalezení odpovídající klauzuli catch. systém přechází do řízení je převedeno na první příkaz v klauzuli catch. Před zahájením provádění v klauzuli catch, systém nejprve provede, v pořadí, všechny `finally` klauzule, které byly přidruženy k další příkazy try, vnořené než ty, které vyvolala výjimku.

Pokud není nalezen žádný odpovídající klauzuli catch, dojde k jedné ze dvou kroků:

*  Pokud hledání odpovídající klauzuli catch dosáhne statického konstruktoru ([statické konstruktory](classes.md#static-constructors)) nebo statickém inicializátoru pole, o `System.TypeInitializationException` dojde v okamžiku, která aktivuje volání statického konstruktoru. Vnitřní výjimka `System.TypeInitializationException` obsahuje výjimku, která byla původně vyvolána.
*  Dosáhne-li vyhledat odpovídající klauzule catch kód, který původně zahájeno vlákno, se ukončí spouštění vlákna. Dopad ukončení, je definováno implementací.

Výjimky, ke kterým dochází při spuštění destruktoru stojí za zvláštní pozornost. Pokud dojde k výjimce při spuštění destruktoru a tato výjimka není zachycena, provádění tento destruktor se ukončí a je volán destruktor základní třídy (pokud existuje). Pokud není žádnou základní třídu (stejně jako v případě třídy `object` typ) nebo pokud neexistuje žádný destruktor základní třídy, pak se zruší výjimku.

## <a name="common-exception-classes"></a>Společné třídy výjimek

Následující výjimky jsou vyvolány pomocí určitých operací C#.

|                                      |                |
|--------------------------------------|----------------|
| `System.ArithmeticException`         | Základní třída pro výjimky, ke kterým dochází při aritmetické operace, jako například `System.DivideByZeroException` a `System.OverflowException`. | 
| `System.ArrayTypeMismatchException`  | Vyvolána, když úložiště do pole se nezdaří, protože skutečný typ elementu uložené není kompatibilní s skutečný typ pole. | 
| `System.DivideByZeroException`       | Vyvolána, když dojde k pokusu o dělení nulou celočíselnou hodnotu. | 
| `System.IndexOutOfRangeException`    | Vyvolána výjimka při pokusu o indexování pole prostřednictvím index, který je menší než nula nebo mimo hranice pole. | 
| `System.InvalidCastException`        | Vyvolána, když selže explicitní převod ze základního typu nebo rozhraní pro odvozený typ v době běhu. | 
| `System.NullReferenceException`      | Vyvolána, když `null` odkaz se používá způsobem, který způsobí, že odkazovaný objekt jako povinné. | 
| `System.OutOfMemoryException`        | Při pokusu o přidělení paměti (přes `new`) se nezdaří. | 
| `System.OverflowException`           | Vyvolána, když v aritmetické operace `checked` kontextu přetečení. | 
| `System.StackOverflowException`      | Vyvolána, když se vyčerpá zásobníku spouštění tak, že příliš mnoho volání metody čekající; obvykle svědčí o velmi podrobné nebo bez vazby rekurze. | 
| `System.TypeInitializationException` | Vyvolána, když statický konstruktor vyvolá výjimku a ne `catch` existuje klauzule má zachytit. | 
