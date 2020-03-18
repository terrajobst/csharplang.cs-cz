---
ms.openlocfilehash: 9ed1aa75d581cecbf754a84d1f523c6334b8c0ac
ms.sourcegitcommit: 5278336b61519956240a6f7d83bcb4322019e032
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/10/2020
ms.locfileid: "79485257"
---
# <a name="async-streams"></a><span data-ttu-id="6c87f-101">Asynchronní streamy</span><span class="sxs-lookup"><span data-stu-id="6c87f-101">Async Streams</span></span>

* <span data-ttu-id="6c87f-102">Navrženo [x]</span><span class="sxs-lookup"><span data-stu-id="6c87f-102">[x] Proposed</span></span>
* <span data-ttu-id="6c87f-103">[x] prototyp</span><span class="sxs-lookup"><span data-stu-id="6c87f-103">[x] Prototype</span></span>
* <span data-ttu-id="6c87f-104">[] Implementace</span><span class="sxs-lookup"><span data-stu-id="6c87f-104">[ ] Implementation</span></span>
* <span data-ttu-id="6c87f-105">[] Specifikace</span><span class="sxs-lookup"><span data-stu-id="6c87f-105">[ ] Specification</span></span>

## <a name="summary"></a><span data-ttu-id="6c87f-106">Souhrn</span><span class="sxs-lookup"><span data-stu-id="6c87f-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="6c87f-107">C#má podporu metod iterátoru a asynchronních metod, ale nepodporuje metodu, která je iterátorem a asynchronní metodou.</span><span class="sxs-lookup"><span data-stu-id="6c87f-107">C# has support for iterator methods and async methods, but no support for a method that is both an iterator and an async method.</span></span>  <span data-ttu-id="6c87f-108">Měli byste to napravit tak, že umožníte použití `await` v nové formě iterátoru `async`, což vrátí `IAsyncEnumerable<T>` nebo `IAsyncEnumerator<T>` místo `IEnumerable<T>` nebo `IEnumerator<T>`, a to s `IAsyncEnumerable<T>`m příplatnou v novém `await foreach`.</span><span class="sxs-lookup"><span data-stu-id="6c87f-108">We should rectify this by allowing for `await` to be used in a new form of `async` iterator, one that returns an `IAsyncEnumerable<T>` or `IAsyncEnumerator<T>` rather than an `IEnumerable<T>` or `IEnumerator<T>`, with `IAsyncEnumerable<T>` consumable in a new `await foreach`.</span></span>  <span data-ttu-id="6c87f-109">Rozhraní `IAsyncDisposable` se používá také k povolení asynchronního vyčištění.</span><span class="sxs-lookup"><span data-stu-id="6c87f-109">An `IAsyncDisposable` interface is also used to enable asynchronous cleanup.</span></span>

## <a name="related-discussion"></a><span data-ttu-id="6c87f-110">Související diskuze</span><span class="sxs-lookup"><span data-stu-id="6c87f-110">Related discussion</span></span>
- https://github.com/dotnet/roslyn/issues/261
- https://github.com/dotnet/roslyn/issues/114

## <a name="detailed-design"></a><span data-ttu-id="6c87f-111">Podrobný návrh</span><span class="sxs-lookup"><span data-stu-id="6c87f-111">Detailed design</span></span>
[design]: #detailed-design

## <a name="interfaces"></a><span data-ttu-id="6c87f-112">Rozhraní</span><span class="sxs-lookup"><span data-stu-id="6c87f-112">Interfaces</span></span>

### <a name="iasyncdisposable"></a><span data-ttu-id="6c87f-113">IAsyncDisposable</span><span class="sxs-lookup"><span data-stu-id="6c87f-113">IAsyncDisposable</span></span>

<span data-ttu-id="6c87f-114">Existuje mnohem diskuze o `IAsyncDisposable` (například https://github.com/dotnet/roslyn/issues/114) a o tom, jestli je to dobrý nápad.</span><span class="sxs-lookup"><span data-stu-id="6c87f-114">There has been much discussion of `IAsyncDisposable` (e.g. https://github.com/dotnet/roslyn/issues/114) and whether it's a good idea.</span></span>  <span data-ttu-id="6c87f-115">Je však vyžadován koncept pro přidání v podpoře asynchronních iterátorů.</span><span class="sxs-lookup"><span data-stu-id="6c87f-115">However, it's a required concept to add in support of async iterators.</span></span>  <span data-ttu-id="6c87f-116">Vzhledem k tomu, že bloky `finally` můžou obsahovat `await`a, protože `finally` bloky je potřeba spustit jako součást odstraňování iterátorů, potřebujeme asynchronní vyřazení.</span><span class="sxs-lookup"><span data-stu-id="6c87f-116">Since `finally` blocks may contain `await`s, and since `finally` blocks need to be run as part of disposing of iterators, we need async disposal.</span></span>  <span data-ttu-id="6c87f-117">To je také všeobecně užitečné při mazání prostředků, například při zavírání souborů (vyžadování vyprázdnění), zrušení registrace zpětných volání a poskytnutí způsobu, jak zjistit, kdy byla Odregistrace dokončena atd.</span><span class="sxs-lookup"><span data-stu-id="6c87f-117">It's also just generally useful any time cleaning up of resources might take any period of time, e.g. closing files (requiring flushes), deregistering callbacks and providing a way to know when deregistration has completed, etc.</span></span>

<span data-ttu-id="6c87f-118">Následující rozhraní se přidá do základních knihoven .NET (např. System. Private. CoreLib/System. Runtime):</span><span class="sxs-lookup"><span data-stu-id="6c87f-118">The following interface is added to the core .NET libraries (e.g. System.Private.CoreLib / System.Runtime):</span></span>
```csharp
namespace System
{
    public interface IAsyncDisposable
    {
        ValueTask DisposeAsync();
    }
}
```
<span data-ttu-id="6c87f-119">Stejně jako u `Dispose`, vyvolávání `DisposeAsync` víckrát je přijatelné a následné vyvolání po prvním by se mělo považovat za nops, vrácení asynchronně dokončeně úspěšné úlohy (`DisposeAsync` nemusí být bezpečná pro přístup z více vláken, ale nemusí podporovat souběžné vyvolání).</span><span class="sxs-lookup"><span data-stu-id="6c87f-119">As with `Dispose`, invoking `DisposeAsync` multiple times is acceptable, and subsequent invocations after the first should be treated as nops, returning a synchronously completed successful task (`DisposeAsync` need not be thread-safe, though, and need not support concurrent invocation).</span></span>  <span data-ttu-id="6c87f-120">Další typy mohou implementovat jak `IDisposable`, tak `IAsyncDisposable`a v případě, že to dělají, je obdobně přijatelné vyvolat `Dispose` a pak `DisposeAsync` nebo naopak, ale pouze první by měla být smysluplná a následné vyvolání by měla být NOP.</span><span class="sxs-lookup"><span data-stu-id="6c87f-120">Further, types may implement both `IDisposable` and `IAsyncDisposable`, and if they do, it's similarly acceptable to invoke `Dispose` and then `DisposeAsync` or vice versa, but only the first should be meaningful and subsequent invocations of either should be a nop.</span></span>  <span data-ttu-id="6c87f-121">V takovém případě, pokud typ implementuje obojí, doporučujeme, aby spotřebitelé volali jednou a pouze jednou relevantní metodu založenou na kontextu, `Dispose` v synchronních kontextech a `DisposeAsync` v asynchronních typech.</span><span class="sxs-lookup"><span data-stu-id="6c87f-121">As such, if a type does implement both, consumers are encouraged to call once and only once the more relevant method based on the context, `Dispose` in synchronous contexts and `DisposeAsync` in asynchronous ones.</span></span>

<span data-ttu-id="6c87f-122">(Ponechávám diskuzi o tom, jak `IAsyncDisposable` komunikuje s `using` k samostatné diskuzi.</span><span class="sxs-lookup"><span data-stu-id="6c87f-122">(I'm leaving discussion of how `IAsyncDisposable` interacts with `using` to a separate discussion.</span></span>  <span data-ttu-id="6c87f-123">A pokrytí toho, jak komunikuje s `foreach`, se zpracovává později v tomto návrhu.)</span><span class="sxs-lookup"><span data-stu-id="6c87f-123">And coverage of how it interacts with `foreach` is handled later in this proposal.)</span></span>

<span data-ttu-id="6c87f-124">Uvažované alternativy:</span><span class="sxs-lookup"><span data-stu-id="6c87f-124">Alternatives considered:</span></span>
- <span data-ttu-id="6c87f-125">_`DisposeAsync` přijetí `CancellationToken`_ : i když teoreticky dává smysl, že může dojít ke zrušení jakékoli asynchronní operace, vyřazení je o vyčištění, ukončování objektů, free'ingch prostředcích atd., což obecně není něco, co by mělo být zrušeno. vyčištění je pro práci, která je zrušená, pořád důležité.</span><span class="sxs-lookup"><span data-stu-id="6c87f-125">_`DisposeAsync` accepting a `CancellationToken`_: while in theory it makes sense that anything async can be canceled, disposal is about cleanup, closing things out, free'ing resources, etc., which is generally not something that should be canceled; cleanup is still important for work that's canceled.</span></span>  <span data-ttu-id="6c87f-126">Stejná `CancellationToken`, která způsobila zrušení skutečné práce, by představovala stejný token předaný `DisposeAsync`a `DisposeAsync` bezcenné, protože zrušení práce by způsobilo, `DisposeAsync` být NOP.</span><span class="sxs-lookup"><span data-stu-id="6c87f-126">The same `CancellationToken` that caused the actual work to be canceled would typically be the same token passed to `DisposeAsync`, making `DisposeAsync` worthless because cancellation of the work would cause `DisposeAsync` to be a nop.</span></span>  <span data-ttu-id="6c87f-127">Pokud někdo chce zabránit zablokování čekání na odstranění, může se jim vyhýbat čekání na výsledný `ValueTask`nebo počkat na určitou dobu.</span><span class="sxs-lookup"><span data-stu-id="6c87f-127">If someone wants to avoid being blocked waiting for disposal, they can avoid waiting on the resulting `ValueTask`, or wait on it only for some period of time.</span></span>
- <span data-ttu-id="6c87f-128">_`DisposeAsync` vrácení `Task`_ : teď, když existuje neobecný `ValueTask` a dá se vytvořit ze `IValueTaskSource`, návratová `ValueTask` z `DisposeAsync` umožňuje, aby se existující objekt znovu použil jako příslib představující konečné dokončení asynchronního `DisposeAsync`, a to v případě, že se `Task` dokončí asynchronně.`DisposeAsync`</span><span class="sxs-lookup"><span data-stu-id="6c87f-128">_`DisposeAsync` returning a `Task`_: Now that a non-generic `ValueTask` exists and can be constructed from an `IValueTaskSource`, returning `ValueTask` from `DisposeAsync` allows an existing object to be reused as the promise representing the eventual async completion of `DisposeAsync`, saving a `Task` allocation in the case where `DisposeAsync` completes asynchronously.</span></span>
- <span data-ttu-id="6c87f-129">_Konfigurace `DisposeAsync` s `bool continueOnCapturedContext` (`ConfigureAwait`)_ : i když mohou nastat problémy související s tím, jak je takový koncept vystavený `using`, `foreach`a dalších jazykových konstrukcích, které tuto funkci využívají, z perspektivy rozhraní není ve skutečnosti žádná `await`a a není nutné nic konfigurovat... Uživatelé `ValueTask` můžou tuto možnost spotřebovat, ale chtějí.</span><span class="sxs-lookup"><span data-stu-id="6c87f-129">_Configuring `DisposeAsync` with a `bool continueOnCapturedContext` (`ConfigureAwait`)_: While there may be issues related to how such a concept is exposed to `using`, `foreach`, and other language constructs that consume this, from an interface perspective it's not actually doing any `await`'ing and there's nothing to configure... consumers of the `ValueTask` can consume it however they wish.</span></span>
- <span data-ttu-id="6c87f-130">_`IAsyncDisposable` dědí `IDisposable`_ : vzhledem k tomu, že by měla být použita pouze jedna nebo druhá, nemá smysl vynutit typy pro implementaci obou.</span><span class="sxs-lookup"><span data-stu-id="6c87f-130">_`IAsyncDisposable` inheriting `IDisposable`_:  Since only one or the other should be used, it doesn't make sense to force types to implement both.</span></span>
- <span data-ttu-id="6c87f-131">_`IDisposableAsync` místo `IAsyncDisposable`_ : pojmenováváme, že se jedná o "asynchronní něco", zatímco operace jsou "dokončené Async", takže typy mají "Async" jako předponu a metody mají "Async" jako příponu.</span><span class="sxs-lookup"><span data-stu-id="6c87f-131">_`IDisposableAsync` instead of `IAsyncDisposable`_: We've been following the naming that things/types are an "async something" whereas operations are "done async", so types have "Async" as a prefix and methods have "Async" as a suffix.</span></span>

### <a name="iasyncenumerable--iasyncenumerator"></a><span data-ttu-id="6c87f-132">IAsyncEnumerable / IAsyncEnumerator</span><span class="sxs-lookup"><span data-stu-id="6c87f-132">IAsyncEnumerable / IAsyncEnumerator</span></span>

<span data-ttu-id="6c87f-133">Do základních knihoven .NET jsou přidány dvě rozhraní:</span><span class="sxs-lookup"><span data-stu-id="6c87f-133">Two interfaces are added to the core .NET libraries:</span></span>

```csharp
namespace System.Collections.Generic
{
    public interface IAsyncEnumerable<out T>
    {
        IAsyncEnumerator<T> GetAsyncEnumerator(CancellationToken cancellationToken = default);
    }

    public interface IAsyncEnumerator<out T> : IAsyncDisposable
    {
        ValueTask<bool> MoveNextAsync();
        T Current { get; }
    }
}
```

<span data-ttu-id="6c87f-134">Typickou spotřebou (bez dalších funkcí jazyka) by vypadalo takto:</span><span class="sxs-lookup"><span data-stu-id="6c87f-134">Typical consumption (without additional language features) would look like:</span></span>

```csharp
IAsyncEnumerator<T> enumerator = enumerable.GetAsyncEnumerator();
try
{
    while (await enumerator.MoveNextAsync())
    {
        Use(enumerator.Current);
    }
}
finally { await enumerator.DisposeAsync(); }
```

<span data-ttu-id="6c87f-135">Považované za zahozené možnosti:</span><span class="sxs-lookup"><span data-stu-id="6c87f-135">Discarded options considered:</span></span>
- <span data-ttu-id="6c87f-136">_`Task<bool> MoveNextAsync(); T current { get; }`_ : použití `Task<bool>` by podporovalo použití objektu úlohy v mezipaměti k reprezentaci synchronních, úspěšných volání `MoveNextAsync`, ale pro asynchronní dokončení by se vyžadovalo přidělení.</span><span class="sxs-lookup"><span data-stu-id="6c87f-136">_`Task<bool> MoveNextAsync(); T current { get; }`_: Using `Task<bool>` would support using a cached task object to represent synchronous, successful `MoveNextAsync` calls, but an allocation would still be required for asynchronous completion.</span></span>  <span data-ttu-id="6c87f-137">Vrácením `ValueTask<bool>`umožníme, aby objekt enumerátoru implementoval sám sebe `IValueTaskSource<bool>` a měl by se používat jako zálohování pro `ValueTask<bool>` vrácené z `MoveNextAsync`, což zase umožňuje významně omezené režijní náklady.</span><span class="sxs-lookup"><span data-stu-id="6c87f-137">By returning `ValueTask<bool>`, we enable the enumerator object to itself implement `IValueTaskSource<bool>` and be used as the backing for the `ValueTask<bool>` returned from `MoveNextAsync`, which in turn allows for significantly reduced overheads.</span></span>
- <span data-ttu-id="6c87f-138">_`ValueTask<(bool, T)> MoveNextAsync();`_ : Nepoužívejte jen těžší, ale to znamená, že `T` již nesmí být kovariantní.</span><span class="sxs-lookup"><span data-stu-id="6c87f-138">_`ValueTask<(bool, T)> MoveNextAsync();`_: It's not only harder to consume, but it means that `T` can no longer be covariant.</span></span>
- <span data-ttu-id="6c87f-139">_`ValueTask<T?> TryMoveNextAsync();`_ : nejedná se o kovariantu.</span><span class="sxs-lookup"><span data-stu-id="6c87f-139">_`ValueTask<T?> TryMoveNextAsync();`_: Not covariant.</span></span>
- <span data-ttu-id="6c87f-140">_`Task<T?> TryMoveNextAsync();`_ : nejedná se o kovariantu, přidělení při každém volání atd.</span><span class="sxs-lookup"><span data-stu-id="6c87f-140">_`Task<T?> TryMoveNextAsync();`_: Not covariant, allocations on every call, etc.</span></span>
- <span data-ttu-id="6c87f-141">_`ITask<T?> TryMoveNextAsync();`_ : nejedná se o kovariantu, přidělení při každém volání atd.</span><span class="sxs-lookup"><span data-stu-id="6c87f-141">_`ITask<T?> TryMoveNextAsync();`_: Not covariant, allocations on every call, etc.</span></span>
- <span data-ttu-id="6c87f-142">_`ITask<(bool,T)> TryMoveNextAsync();`_ : nejedná se o kovariantu, přidělení při každém volání atd.</span><span class="sxs-lookup"><span data-stu-id="6c87f-142">_`ITask<(bool,T)> TryMoveNextAsync();`_: Not covariant, allocations on every call, etc.</span></span>
- <span data-ttu-id="6c87f-143">_`Task<bool> TryMoveNextAsync(out T result);`_ : výsledek `out` by se musel nastavit, když operace vrátí synchronní hodnotu, ne, když asynchronně dokončí úlohu potenciálně delší dobu v budoucnosti, a proto neexistuje žádný způsob, jak sdělit výsledek.</span><span class="sxs-lookup"><span data-stu-id="6c87f-143">_`Task<bool> TryMoveNextAsync(out T result);`_: The `out` result would need to be set when the operation returns synchronously, not when it asynchronously completes the task potentially sometime long in the future, at which point there'd be no way to communicate the result.</span></span>
- <span data-ttu-id="6c87f-144">_`IAsyncEnumerator<T>` neimplementuje `IAsyncDisposable`_ : můžeme se rozhodnout oddělit.</span><span class="sxs-lookup"><span data-stu-id="6c87f-144">_`IAsyncEnumerator<T>` not implementing `IAsyncDisposable`_: We could choose to separate these.</span></span>  <span data-ttu-id="6c87f-145">Nicméně v důsledku toho ztěžuje některé další oblasti návrhu, protože kód musí být schopný zabývat se možností, že enumerátor neposkytuje vyřazení, což ztěžuje psaní vzorových pomocníků.</span><span class="sxs-lookup"><span data-stu-id="6c87f-145">However, doing so complicates certain other areas of the proposal, as code must then be able to deal with the possibility that an enumerator doesn't provide disposal, which makes it difficult to write pattern-based helpers.</span></span>  <span data-ttu-id="6c87f-146">Kromě toho bude běžné, že enumerátory budou mít potřebu vyřazení (například každý C# asynchronní iterátor, který má blok finally, většinu věcí vyčíslit data z připojení k síti atd.), a pokud k tomu nedojde, je jednoduché implementovat metodu čistě jako `public ValueTask DisposeAsync() => default(ValueTask);` s minimální další režií.</span><span class="sxs-lookup"><span data-stu-id="6c87f-146">Further, it will be common for enumerators to have a need for disposal (e.g. any C# async iterator that has a finally block, most things enumerating data from a network connection, etc.), and if one doesn't, it is simple to implement the method purely as `public ValueTask DisposeAsync() => default(ValueTask);` with minimal additional overhead.</span></span>
- <span data-ttu-id="6c87f-147">_ `IAsyncEnumerator<T> GetAsyncEnumerator()`: není zadán žádný parametr tokenu zrušení.</span><span class="sxs-lookup"><span data-stu-id="6c87f-147">_ `IAsyncEnumerator<T> GetAsyncEnumerator()`: No cancellation token parameter.</span></span>

#### <a name="viable-alternative"></a><span data-ttu-id="6c87f-148">Životaschopná alternativa:</span><span class="sxs-lookup"><span data-stu-id="6c87f-148">Viable alternative:</span></span>

```csharp
namespace System.Collections.Generic
{
    public interface IAsyncEnumerable<out T>
    {
        IAsyncEnumerator<T> GetAsyncEnumerator();
    }

    public interface IAsyncEnumerator<out T> : IAsyncDisposable
    {
        ValueTask<bool> WaitForNextAsync();
        T TryGetNext(out bool success);
    }
}
```

<span data-ttu-id="6c87f-149">`TryGetNext` se používá ve vnitřní smyčce ke spotřebě položek s jedním voláním rozhraní, pokud jsou k dispozici synchronně.</span><span class="sxs-lookup"><span data-stu-id="6c87f-149">`TryGetNext` is used in an inner loop to consume items with a single interface call as long as they're available synchronously.</span></span>  <span data-ttu-id="6c87f-150">Když další položku nelze získat synchronně, vrátí hodnotu false a kdykoli vrátí hodnotu false, volající musí následně vyvolat `WaitForNextAsync`, aby čekal na zpřístupnění další položky, nebo aby zjistil, že nikdy nebude další položkou.</span><span class="sxs-lookup"><span data-stu-id="6c87f-150">When the next item can't be retrieved synchronously, it returns false, and any time it returns false, a caller must subsequently invoke `WaitForNextAsync` to either wait for the next item to be available or to determine that there will never be another item.</span></span> <span data-ttu-id="6c87f-151">Typickou spotřebou (bez dalších funkcí jazyka) by vypadalo takto:</span><span class="sxs-lookup"><span data-stu-id="6c87f-151">Typical consumption (without additional language features) would look like:</span></span>

```csharp
IAsyncEnumerator<T> enumerator = enumerable.GetAsyncEnumerator();
try
{
    while (await enumerator.WaitForNextAsync())
    {
        while (true)
        {
            int item = enumerator.TryGetNext(out bool success);
            if (!success) break;
            Use(item);
        }
    }
}
finally { await enumerator.DisposeAsync(); }
```

<span data-ttu-id="6c87f-152">Výhodou tohoto je dvě přeložení, jedna z nich a jedna hlavní:</span><span class="sxs-lookup"><span data-stu-id="6c87f-152">The advantage of this is two-fold, one minor and one major:</span></span>
- <span data-ttu-id="6c87f-153">_Podverze: umožňuje enumerátoru podporovat více uživatelů_.</span><span class="sxs-lookup"><span data-stu-id="6c87f-153">_Minor: Allows for an enumerator to support multiple consumers_.</span></span> <span data-ttu-id="6c87f-154">Mohou nastat situace, kdy je to pro enumerátor užitečné pro podporu více souběžných uživatelů.</span><span class="sxs-lookup"><span data-stu-id="6c87f-154">There may be scenarios where it's valuable for an enumerator to support multiple concurrent consumers.</span></span>  <span data-ttu-id="6c87f-155">Tato funkce se nedá dosáhnout, když `MoveNextAsync` a `Current` jsou oddělené tak, že implementace nemůže učinit atomickou spotřebu.</span><span class="sxs-lookup"><span data-stu-id="6c87f-155">That can't be achieved when `MoveNextAsync` and `Current` are separate such that an implementation can't make their usage atomic.</span></span>  <span data-ttu-id="6c87f-156">Naproti tomu tento přístup poskytuje jedinou metodu `TryGetNext`, která podporuje vložení enumerátoru do dalšího a získání další položky, takže enumerátor může v případě potřeby povolit nedělitelnost.</span><span class="sxs-lookup"><span data-stu-id="6c87f-156">In contrast, this approach provides a single method `TryGetNext` that supports pushing the enumerator forward and getting the next item, so the enumerator can enable atomicity if desired.</span></span>  <span data-ttu-id="6c87f-157">Je však možné, že takové scénáře mohou být také povoleny tím, že každý příjemce získá vlastní enumerátor ze sdíleného výčtu.</span><span class="sxs-lookup"><span data-stu-id="6c87f-157">However, it's likely that such scenarios could also be enabled by giving each consumer its own enumerator from a shared enumerable.</span></span>  <span data-ttu-id="6c87f-158">Ještě nepotřebujeme vynutit, aby všechny enumerátory podporovaly souběžné používání, protože by to znamenalo netriviální režijních hlav, které to nevyžadují, což znamená, že se na příjemce rozhraní všeobecně nedaří spoléhat.</span><span class="sxs-lookup"><span data-stu-id="6c87f-158">Further, we don't want to enforce that every enumerator support concurrent usage, as that would add non-trivial overheads to the majority case that doesn't require it, which means a consumer of the interface generally couldn't rely on this any way.</span></span>
- <span data-ttu-id="6c87f-159">_Hlavní: výkon_.</span><span class="sxs-lookup"><span data-stu-id="6c87f-159">_Major: Performance_.</span></span> <span data-ttu-id="6c87f-160">`MoveNextAsync`/`Current` vyžaduje dvě volání rozhraní na operaci, zatímco nejlepší pro `WaitForNextAsync`/`TryGetNext` je, že většina iterací je dokončena synchronně a umožňuje úzkou vnitřní smyčku s `TryGetNext`, takže máme pouze jedno volání rozhraní na operaci.</span><span class="sxs-lookup"><span data-stu-id="6c87f-160">The `MoveNextAsync`/`Current` approach requires two interface calls per operation, whereas the best case for `WaitForNextAsync`/`TryGetNext` is that most iterations complete synchronously, enabling a tight inner loop with `TryGetNext`, such that we only have one interface call per operation.</span></span>  <span data-ttu-id="6c87f-161">To může mít měřitelný dopad v situacích, kdy rozhraní volá toto výpočetní prostředí.</span><span class="sxs-lookup"><span data-stu-id="6c87f-161">This can have a measurable impact in situations where the interface calls dominate the computation.</span></span>

<span data-ttu-id="6c87f-162">Existují však netriviální downsides, včetně významně zvýšené složitosti při jejich využívání, a zvýšení pravděpodobnosti, že při jejich používání dojde k chybám.</span><span class="sxs-lookup"><span data-stu-id="6c87f-162">However, there are non-trivial downsides, including significantly increased complexity when consuming these manually, and an increased chance of introducing bugs when using them.</span></span>  <span data-ttu-id="6c87f-163">A i když se výhody výkonu projeví v mikrosrovnávacích testech, nevěříme, že budou mít dopad na velká většina reálného využití.</span><span class="sxs-lookup"><span data-stu-id="6c87f-163">And while the performance benefits show up in microbenchmarks, we don't believe they'll be impactful in the vast majority of real usage.</span></span>  <span data-ttu-id="6c87f-164">Pokud dojde k jejich zapínání, můžeme současně zavést druhou sadu rozhraní.</span><span class="sxs-lookup"><span data-stu-id="6c87f-164">If it turns out they are, we can introduce a second set of interfaces in a light-up fashion.</span></span>

<span data-ttu-id="6c87f-165">Považované za zahozené možnosti:</span><span class="sxs-lookup"><span data-stu-id="6c87f-165">Discarded options considered:</span></span>
- <span data-ttu-id="6c87f-166">`ValueTask<bool> WaitForNextAsync(); bool TryGetNext(out T result);`: parametry `out` nemohou být kovariantní.</span><span class="sxs-lookup"><span data-stu-id="6c87f-166">`ValueTask<bool> WaitForNextAsync(); bool TryGetNext(out T result);`: `out` parameters can't be covariant.</span></span>  <span data-ttu-id="6c87f-167">Je zde také malý dopad (problém se vzorem try obecně), který se tím nejspíš zavede jako bariéra zápisu za běhu pro výsledky referenčního typu.</span><span class="sxs-lookup"><span data-stu-id="6c87f-167">There's also a small impact here (an issue with the try pattern in general) that this likely incurs a runtime write barrier for reference type results.</span></span>

#### <a name="cancellation"></a><span data-ttu-id="6c87f-168">Zrušení</span><span class="sxs-lookup"><span data-stu-id="6c87f-168">Cancellation</span></span>

<span data-ttu-id="6c87f-169">Existuje několik možných přístupů k podpoře zrušení:</span><span class="sxs-lookup"><span data-stu-id="6c87f-169">There are several possible approaches to supporting cancellation:</span></span>
1. <span data-ttu-id="6c87f-170">`IAsyncEnumerable<T>`/`IAsyncEnumerator<T>` zrušení – nezávislá: `CancellationToken` se nezobrazí kdekoli.</span><span class="sxs-lookup"><span data-stu-id="6c87f-170">`IAsyncEnumerable<T>`/`IAsyncEnumerator<T>` are cancellation-agnostic: `CancellationToken` doesn't appear anywhere.</span></span>  <span data-ttu-id="6c87f-171">Zrušení je dosaženo logicky pečením `CancellationToken` do vyčíslitelného a/nebo enumerátoru jakýmkoli způsobem, například při volání iterátoru, předání `CancellationToken` jako argumentu metodě iterátoru a jeho použití v těle iterátoru, jak je provedeno s jakýmkoli jiným parametrem.</span><span class="sxs-lookup"><span data-stu-id="6c87f-171">Cancellation is achieved by logically baking the `CancellationToken` into the enumerable and/or enumerator in whatever manner is appropriate, e.g. when calling an iterator, passing the `CancellationToken` as an argument to the iterator method and using it in the body of the iterator, as is done with any other parameter.</span></span>
2. <span data-ttu-id="6c87f-172">`IAsyncEnumerator<T>.GetAsyncEnumerator(CancellationToken)`: předáváte `CancellationToken` `GetAsyncEnumerator`a další `MoveNextAsync` operace se dotýkají IT oddělení.</span><span class="sxs-lookup"><span data-stu-id="6c87f-172">`IAsyncEnumerator<T>.GetAsyncEnumerator(CancellationToken)`: You pass a `CancellationToken` to `GetAsyncEnumerator`, and subsequent `MoveNextAsync` operations respect it however it can.</span></span>
3. <span data-ttu-id="6c87f-173">`IAsyncEnumerator<T>.MoveNextAsync(CancellationToken)`: předáte `CancellationToken` každému jednotlivému volání `MoveNextAsync`.</span><span class="sxs-lookup"><span data-stu-id="6c87f-173">`IAsyncEnumerator<T>.MoveNextAsync(CancellationToken)`: You pass a `CancellationToken` to each individual `MoveNextAsync` call.</span></span>
4. <span data-ttu-id="6c87f-174">1 & & 2: oba vložíte `CancellationToken`do výčtu nebo enumerátoru a předáte je `CancellationToken`do `GetAsyncEnumerator`.</span><span class="sxs-lookup"><span data-stu-id="6c87f-174">1 && 2: You both embed `CancellationToken`s into your enumerable/enumerator and pass `CancellationToken`s into `GetAsyncEnumerator`.</span></span>
5. <span data-ttu-id="6c87f-175">1 & & 3: oba vložíte `CancellationToken`y do výčtu nebo enumerátoru a předáte `CancellationToken`do `MoveNextAsync`.</span><span class="sxs-lookup"><span data-stu-id="6c87f-175">1 && 3: You both embed `CancellationToken`s into your enumerable/enumerator and pass `CancellationToken`s into `MoveNextAsync`.</span></span>

<span data-ttu-id="6c87f-176">Z čistě teoretického hlediska je (5) nejvíc robustní, v tom, že (a) `MoveNextAsync` přijímá `CancellationToken` umožňuje nejvíce jemně odstupňovanou kontrolu nad tím, co se zrušilo a (b) `CancellationToken` je pouze jakýkoli jiný typ, který se může předat jako argument iterátorům, vkládat do libovolného typu atd.</span><span class="sxs-lookup"><span data-stu-id="6c87f-176">From a purely theoretical perspective, (5) is the most robust, in that (a) `MoveNextAsync` accepting a `CancellationToken` enables the most fine-grained control over what's canceled, and (b) `CancellationToken` is just any other type that can passed as an argument into iterators, embedded in arbitrary types, etc.</span></span>

<span data-ttu-id="6c87f-177">Existuje však několik problémů s tímto přístupem:</span><span class="sxs-lookup"><span data-stu-id="6c87f-177">However, there are multiple problems with that approach:</span></span>
- <span data-ttu-id="6c87f-178">Jakým způsobem `CancellationToken` předává, aby `GetAsyncEnumerator` převedl do těla iterátoru?</span><span class="sxs-lookup"><span data-stu-id="6c87f-178">How does a `CancellationToken` passed to `GetAsyncEnumerator` make it into the body of the iterator?</span></span>  <span data-ttu-id="6c87f-179">Mohli bychom vystavit nové klíčové slovo `iterator`, na které byste se mohli dostat z, abyste získali přístup k `CancellationToken` předanému `GetEnumerator`. ale) to je spousta dalších strojových strojů, b) vytvoříme ho velmi první občan a c) 99% případ by byl stejný kód při volání iterátoru a volání `GetAsyncEnumerator`. v takovém případě může pouze předat `CancellationToken` jako argument do metody.</span><span class="sxs-lookup"><span data-stu-id="6c87f-179">We could expose a new `iterator` keyword that you could dot off of to get access to the `CancellationToken` passed to `GetEnumerator`, but a) that's a lot of additional machinery, b) we're making it a very first-class citizen, and c) the 99% case would seem to be the same code both calling an iterator and calling `GetAsyncEnumerator` on it, in which case it can just pass the `CancellationToken` as an argument into the method.</span></span>
- <span data-ttu-id="6c87f-180">Jak se předává `CancellationToken` `MoveNextAsync` dostat do těla metody?</span><span class="sxs-lookup"><span data-stu-id="6c87f-180">How does a `CancellationToken` passed to `MoveNextAsync` get into the body of the method?</span></span>  <span data-ttu-id="6c87f-181">To je ještě horší, jako kdyby bylo vystaveno z `iterator` lokálnímu objektu, jeho hodnota se může změnit v rámci await, což znamená, že jakýkoliv kód, který se zaregistruje s tokenem, bude muset zrušit registraci před čekáním a pak znovu zaregistrovat po; je také možné, že by bylo potřeba takové registrace a zrušení registrace při každém `MoveNextAsync` volání, bez ohledu na to, zda je implementováno kompilátorem v iterátoru nebo vývojářem ručně.</span><span class="sxs-lookup"><span data-stu-id="6c87f-181">This is even worse, as if it's exposed off of an `iterator` local object, its value could change across awaits, which means any code that registered with the token would need to unregister from it prior to awaits and then re-register after; it's also potentially quite expensive to need to do such registering and unregistering in every `MoveNextAsync` call, regardless of whether implemented by the compiler in an iterator or by a developer manually.</span></span>
- <span data-ttu-id="6c87f-182">Jak vývojář zruší `foreach` smyčka?</span><span class="sxs-lookup"><span data-stu-id="6c87f-182">How does a developer cancel a `foreach` loop?</span></span>  <span data-ttu-id="6c87f-183">Pokud to provedete tak, že `CancellationToken` přidělíte výčtu výčtů nebo enumerátorů, potřebujeme, aby podporovala `foreach`i nad enumerátory, díky tomu, aby se stali vašimi občany první třídy, a teď je potřeba začít myslet na ekosystém sestavený kolem enumerátorů (např. metody LINQ `WithCancellation`) nebo b) musíme vložit `CancellationToken` do výčtu, aby bylo možné `IAsyncEnumerable<T>`, která by uložil poskytnutý token, a předejte ho do `GetAsyncEnumerator` se zabaleným výčtem, když `GetAsyncEnumerator` ve vrácené struktuře. je vyvolána (ignorování tohoto tokenu).</span><span class="sxs-lookup"><span data-stu-id="6c87f-183">If it's done by giving a `CancellationToken` to an enumerable/enumerator, then either a) we need to support `foreach`'ing over enumerators, which raises them to being first-class citizens, and now you need to start thinking about an ecosystem built up around enumerators (e.g. LINQ methods) or b) we need to embed the `CancellationToken` in the enumerable anyway by having some `WithCancellation` extension method off of `IAsyncEnumerable<T>` that would store the provided token and then pass it into  the wrapped enumerable's `GetAsyncEnumerator` when the `GetAsyncEnumerator` on the returned struct is invoked (ignoring that token).</span></span>  <span data-ttu-id="6c87f-184">Nebo můžete pouze použít `CancellationToken` v těle foreach.</span><span class="sxs-lookup"><span data-stu-id="6c87f-184">Or, you can just use the `CancellationToken` you have in the body of the foreach.</span></span>
- <span data-ttu-id="6c87f-185">Pokud jsou podporovány porozumění dotazů, jak by byl `CancellationToken` poskytnut `GetEnumerator` nebo `MoveNextAsync` předávat do každé klauzule?</span><span class="sxs-lookup"><span data-stu-id="6c87f-185">If/when query comprehensions are supported, how would the `CancellationToken` supplied to `GetEnumerator` or `MoveNextAsync` be passed into each clause?</span></span>  <span data-ttu-id="6c87f-186">Nejjednodušší způsob, jak by měla být klauzule zachytávání, a v takovém případě jakýkoli token je předán `GetAsyncEnumerator`/`MoveNextAsync` je ignorován.</span><span class="sxs-lookup"><span data-stu-id="6c87f-186">The easiest way would simply be for the clause to capture it, at which point whatever token is passed to `GetAsyncEnumerator`/`MoveNextAsync` is ignored.</span></span>

<span data-ttu-id="6c87f-187">Starší verze tohoto dokumentu doporučila (1), ale od té doby jsme přešli na (4).</span><span class="sxs-lookup"><span data-stu-id="6c87f-187">An earlier version of this document recommended (1), but we since switched to (4).</span></span>

<span data-ttu-id="6c87f-188">Dva hlavní problémy s (1):</span><span class="sxs-lookup"><span data-stu-id="6c87f-188">The two main problems with (1):</span></span>
- <span data-ttu-id="6c87f-189">výrobci výčtů zrušit musí implementovat nějaký často používaný text a můžou využít jenom podporu kompilátoru pro asynchronní iterátory k implementaci `IAsyncEnumerator<T> GetAsyncEnumerator(CancellationToken)` metody.</span><span class="sxs-lookup"><span data-stu-id="6c87f-189">producers of cancellable enumerables have to implement some boilerplate, and can only leverage the compiler's support for async-iterators to implement a `IAsyncEnumerator<T> GetAsyncEnumerator(CancellationToken)` method.</span></span>
- <span data-ttu-id="6c87f-190">je možné, že velký počet výrobců by se chtěl místo toho přidat parametr `CancellationToken` do jejich Async-vyčíslitelné signatury, což zabrání uživatelům v předání tokenu zrušení, který chtějí, pokud budou mít `IAsyncEnumerable` typ.</span><span class="sxs-lookup"><span data-stu-id="6c87f-190">it is likely that many producers would be tempted to just add a `CancellationToken` parameter to their async-enumerable signature instead, which will prevent consumers from passing the cancellation token they want when they are given an `IAsyncEnumerable` type.</span></span>

<span data-ttu-id="6c87f-191">Existují dva hlavní scénáře spotřeby:</span><span class="sxs-lookup"><span data-stu-id="6c87f-191">There are two main consumption scenarios:</span></span>
1. <span data-ttu-id="6c87f-192">`await foreach (var i in GetData(token)) ...`, kde příjemce volá metodu asynchronního iterátoru,</span><span class="sxs-lookup"><span data-stu-id="6c87f-192">`await foreach (var i in GetData(token)) ...` where the consumer calls the async-iterator method,</span></span>
2. <span data-ttu-id="6c87f-193">`await foreach (var i in givenIAsyncEnumerable.WithCancellation(token)) ...`, kde se příjemce zabývá daným `IAsyncEnumerable` instancí.</span><span class="sxs-lookup"><span data-stu-id="6c87f-193">`await foreach (var i in givenIAsyncEnumerable.WithCancellation(token)) ...` where the consumer deals with a given `IAsyncEnumerable` instance.</span></span>

<span data-ttu-id="6c87f-194">Zjistili jsme, že přiměřená ohrožení pro podporu obou scénářů způsobem, který je pohodlný pro producenta i spotřebitele asynchronních datových proudů, je použití speciálně nakomentovaného parametru v metodě Async-iterátoru.</span><span class="sxs-lookup"><span data-stu-id="6c87f-194">We find that a reasonable compromise to support both scenarios in a way that is convenient for both producers and consumers of async-streams is to use a specially annotated parameter in the async-iterator method.</span></span> <span data-ttu-id="6c87f-195">Atribut `[EnumeratorCancellation]` se používá pro tento účel.</span><span class="sxs-lookup"><span data-stu-id="6c87f-195">The `[EnumeratorCancellation]` attribute is used for this purpose.</span></span> <span data-ttu-id="6c87f-196">Umístění tohoto atributu pro parametr instruuje kompilátor, že pokud je token předán metodě `GetAsyncEnumerator`, musí být tento token použit místo hodnoty původně předané pro parametr.</span><span class="sxs-lookup"><span data-stu-id="6c87f-196">Placing this attribute on a parameter tells the compiler that if a token is passed to the `GetAsyncEnumerator` method, that token should be used instead of the value originally passed for the parameter.</span></span>

<span data-ttu-id="6c87f-197">Zvažte `IAsyncEnumerable<int> GetData([EnumeratorCancellation] CancellationToken token = default)`.</span><span class="sxs-lookup"><span data-stu-id="6c87f-197">Consider `IAsyncEnumerable<int> GetData([EnumeratorCancellation] CancellationToken token = default)`.</span></span> <span data-ttu-id="6c87f-198">Implementátor této metody může jednoduše použít parametr v těle metody.</span><span class="sxs-lookup"><span data-stu-id="6c87f-198">The implementer of this method can simply use the parameter in the method body.</span></span> <span data-ttu-id="6c87f-199">Příjemce může použít vzorce spotřeby výše:</span><span class="sxs-lookup"><span data-stu-id="6c87f-199">The consumer can use either consumption patterns above:</span></span>
1. <span data-ttu-id="6c87f-200">Použijete-li `GetData(token)`, je token uložen do asynchronního výčtu a bude použit v iteraci,</span><span class="sxs-lookup"><span data-stu-id="6c87f-200">if you use `GetData(token)`, then the token is saved into the async-enumerable and will be used in iteration,</span></span>
2. <span data-ttu-id="6c87f-201">Použijete-li `givenIAsyncEnumerable.WithCancellation(token)`, bude token předaný `GetAsyncEnumerator` nahrazen všemi tokeny uloženými v asynchronním výčtu.</span><span class="sxs-lookup"><span data-stu-id="6c87f-201">if you use `givenIAsyncEnumerable.WithCancellation(token)`, then the token passed to `GetAsyncEnumerator` will supersede any token saved in the async-enumerable.</span></span>

## <a name="foreach"></a><span data-ttu-id="6c87f-202">foreach</span><span class="sxs-lookup"><span data-stu-id="6c87f-202">foreach</span></span>

<span data-ttu-id="6c87f-203">`foreach` bude rozšířena tak, aby podporovala `IAsyncEnumerable<T>` kromě stávající podpory pro `IEnumerable<T>`.</span><span class="sxs-lookup"><span data-stu-id="6c87f-203">`foreach` will be augmented to support `IAsyncEnumerable<T>` in addition to its existing support for `IEnumerable<T>`.</span></span>  <span data-ttu-id="6c87f-204">A bude podporovat ekvivalent `IAsyncEnumerable<T>` jako vzor, pokud jsou relevantní členové veřejně vystaveni, návrat k použití rozhraní přímo, pokud není, aby bylo možné povolit rozšíření založená na struktuře, které se vyhne přidělení, a také použití alternativních awaitables jako návratový typ `MoveNextAsync` a `DisposeAsync`.</span><span class="sxs-lookup"><span data-stu-id="6c87f-204">And it will support the equivalent of `IAsyncEnumerable<T>` as a pattern if the relevant members are exposed publicly, falling back to using the interface directly if not, in order to enable struct-based extensions that avoid allocating as well as using alternative awaitables as the return type of `MoveNextAsync` and `DisposeAsync`.</span></span>

### <a name="syntax"></a><span data-ttu-id="6c87f-205">Syntaxe</span><span class="sxs-lookup"><span data-stu-id="6c87f-205">Syntax</span></span>

<span data-ttu-id="6c87f-206">Pomocí syntaxe:</span><span class="sxs-lookup"><span data-stu-id="6c87f-206">Using the syntax:</span></span>

```csharp
foreach (var i in enumerable)
```

<span data-ttu-id="6c87f-207">C#bude nadále zacházet se `enumerable` jako synchronní výčty, takže i když zveřejňuje relevantní rozhraní API pro asynchronní výčty (vystavení vzoru nebo implementace rozhraní), bude uvažovat pouze o synchronních rozhraních API.</span><span class="sxs-lookup"><span data-stu-id="6c87f-207">C# will continue to treat `enumerable` as a synchronous enumerable, such that even if it exposes the relevant APIs for async enumerables (exposing the pattern or implementing the interface), it will only consider the synchronous APIs.</span></span>

<span data-ttu-id="6c87f-208">Chcete-li vynutit `foreach` pouze v případě, že je třeba použít asynchronní rozhraní API, `await` vložena takto:</span><span class="sxs-lookup"><span data-stu-id="6c87f-208">To force `foreach` to instead only consider the asynchronous APIs, `await` is inserted as follows:</span></span>

```csharp
await foreach (var i in enumerable)
```

<span data-ttu-id="6c87f-209">Neposkytla se žádná syntaxe, která by podporovala buď rozhraní API pro asynchronní nebo synchronizační použití. Vývojář musí zvolit v závislosti na použité syntaxi.</span><span class="sxs-lookup"><span data-stu-id="6c87f-209">No syntax would be provided that would support using either the async or the sync APIs; the developer must choose based on the syntax used.</span></span>

<span data-ttu-id="6c87f-210">Považované za zahozené možnosti:</span><span class="sxs-lookup"><span data-stu-id="6c87f-210">Discarded options considered:</span></span>
- <span data-ttu-id="6c87f-211">_`foreach (var i in await enumerable)`_ : Toto je již platná syntaxe a změna jejího významu by byla zásadní změnou.</span><span class="sxs-lookup"><span data-stu-id="6c87f-211">_`foreach (var i in await enumerable)`_: This is already valid syntax, and changing its meaning would be a breaking change.</span></span>  <span data-ttu-id="6c87f-212">To znamená, že `await` `enumerable`se z něj provede synchronním přechodem a pak synchronně projde to.</span><span class="sxs-lookup"><span data-stu-id="6c87f-212">This means to `await` the `enumerable`, get back something synchronously iterable from it, and then synchronously iterate through that.</span></span>
- <span data-ttu-id="6c87f-213">_`foreach (var i await in enumerable)`, `foreach (var await i in enumerable)``foreach (await var i in enumerable)`_ : Tyto všechny navrhují, že čekáme na další položku, ale v příkazu foreach existují i jiné očekávání, zejména pokud je výčtový `IAsyncDisposable`, bude `await`"jejich asynchronní vyřazení.</span><span class="sxs-lookup"><span data-stu-id="6c87f-213">_`foreach (var i await in enumerable)`, `foreach (var await i in enumerable)`, `foreach (await var i in enumerable)`_: These all suggest that we're awaiting the next item, but there are other awaits involved in foreach, in particular if the enumerable is an `IAsyncDisposable`, we will be `await`'ing its async disposal.</span></span>  <span data-ttu-id="6c87f-214">Tento příkaz await je jako rozsah sady foreach, nikoli pro každý jednotlivý prvek, a proto klíčové slovo `await` zachovává na úrovni `foreach`.</span><span class="sxs-lookup"><span data-stu-id="6c87f-214">That await is as the scope of the foreach rather than for each individual element, and thus the `await` keyword deserves to be at the `foreach` level.</span></span>  <span data-ttu-id="6c87f-215">Další s tím, že je přidružená k `foreach` nám poskytuje způsob, jak popsat `foreach` s jiným termínem, například "await foreach".</span><span class="sxs-lookup"><span data-stu-id="6c87f-215">Further, having it associated with the `foreach` gives us a way to describe the `foreach` with a different term, e.g. a "await foreach".</span></span>  <span data-ttu-id="6c87f-216">Důležitější je však, že existuje hodnota v zvažování `foreach` syntaxe ve stejnou dobu jako `using` syntaxe, aby zůstaly vzájemně konzistentní a `using (await ...)` je již platná syntaxe.</span><span class="sxs-lookup"><span data-stu-id="6c87f-216">But more importantly, there's value in considering `foreach` syntax at the same time as `using` syntax, so that they remain consistent with each other, and `using (await ...)` is already valid syntax.</span></span>
- _`foreach await (var i in enumerable)`_

<span data-ttu-id="6c87f-217">Pořád zvažte:</span><span class="sxs-lookup"><span data-stu-id="6c87f-217">Still to consider:</span></span>
- <span data-ttu-id="6c87f-218">`foreach` dnes nepodporuje iterace prostřednictvím enumerátoru.</span><span class="sxs-lookup"><span data-stu-id="6c87f-218">`foreach` today does not support iterating through an enumerator.</span></span>  <span data-ttu-id="6c87f-219">Očekáváme, že bude častější, že budete mít `IAsyncEnumerator<T>`s využitím, takže se bude považovat za podporu `await foreach` s `IAsyncEnumerable<T>` i `IAsyncEnumerator<T>`.</span><span class="sxs-lookup"><span data-stu-id="6c87f-219">We expect it will be more common to have `IAsyncEnumerator<T>`s handed around, and thus it's tempting to support `await foreach` with both `IAsyncEnumerable<T>` and `IAsyncEnumerator<T>`.</span></span>  <span data-ttu-id="6c87f-220">Ale jakmile tuto podporu přidáte, zavádíme otázku, zda `IAsyncEnumerator<T>` je občanem první třídy a zda musíme mít přetížení kombinátory, které pracují s enumerátory kromě výčtů?</span><span class="sxs-lookup"><span data-stu-id="6c87f-220">But once we add such support, it introduces the question of whether `IAsyncEnumerator<T>` is a first-class citizen, and whether we need to have overloads of combinators that operate on enumerators in addition to enumerables?</span></span>    <span data-ttu-id="6c87f-221">Chceme místo výčtu vyzvat metody pro vrácení čítačů?</span><span class="sxs-lookup"><span data-stu-id="6c87f-221">Do we want to encourage methods to return enumerators rather than enumerables?</span></span> <span data-ttu-id="6c87f-222">Tento postup bychom měli dál diskutovat.</span><span class="sxs-lookup"><span data-stu-id="6c87f-222">We should continue to discuss this.</span></span>  <span data-ttu-id="6c87f-223">Pokud se rozhodnete, že ji nechcete podporovat, můžeme zavést metodu rozšíření `public static IAsyncEnumerable<T> AsEnumerable<T>(this IAsyncEnumerator<T> enumerator);`, která by umožnila stále `foreach`výčtu.</span><span class="sxs-lookup"><span data-stu-id="6c87f-223">If we decide we don't want to support it, we might want to introduce an extension method `public static IAsyncEnumerable<T> AsEnumerable<T>(this IAsyncEnumerator<T> enumerator);` that would allow an enumerator to still be `foreach`'d.</span></span>  <span data-ttu-id="6c87f-224">Pokud se rozhodnete, že ji chceme podporovat, je nutné také rozhodnout, zda `await foreach` být odpovědni za volání `DisposeAsync` na enumerátoru, a odpověď je pravděpodobně "No, měla by být řízení vyřazení zpracována ve stejném znění jako `GetEnumerator`."</span><span class="sxs-lookup"><span data-stu-id="6c87f-224">If we decide we do want to support it, we'll need to also decide on whether the `await foreach` would be responsible for calling `DisposeAsync` on the enumerator, and the answer is likely "no, control over disposal should be handled by whoever called `GetEnumerator`."</span></span>

### <a name="pattern-based-compilation"></a><span data-ttu-id="6c87f-225">Kompilace založená na vzorcích</span><span class="sxs-lookup"><span data-stu-id="6c87f-225">Pattern-based Compilation</span></span>

<span data-ttu-id="6c87f-226">Kompilátor vytvoří propojení s rozhraními API založenými na vzorech, pokud existují, přičemž před použitím rozhraní (vzor může být spokojen s metodami instance nebo metodami rozšíření).</span><span class="sxs-lookup"><span data-stu-id="6c87f-226">The compiler will bind to the pattern-based APIs if they exist, preferring those over using the interface (the pattern may be satisfied with instance methods or extension methods).</span></span>  <span data-ttu-id="6c87f-227">Požadavky pro vzor:</span><span class="sxs-lookup"><span data-stu-id="6c87f-227">The requirements for the pattern are:</span></span>
- <span data-ttu-id="6c87f-228">Vyčíslitelné musí vystavit `GetAsyncEnumerator` metodu, která může být volána bez argumentů a vrátí enumerátor, který splňuje příslušný vzor.</span><span class="sxs-lookup"><span data-stu-id="6c87f-228">The enumerable must expose a `GetAsyncEnumerator` method that may be called with no arguments and that returns an enumerator that meets the relevant pattern.</span></span>
- <span data-ttu-id="6c87f-229">Enumerátor musí vystavit `MoveNextAsync` metodu, která může být volána bez argumentů a vrátí něco, co může být `await`Ed a jehož `GetResult()` vrací `bool`.</span><span class="sxs-lookup"><span data-stu-id="6c87f-229">The enumerator must expose a `MoveNextAsync` method that may be called with no arguments and that returns something which may be `await`ed and whose `GetResult()` returns a `bool`.</span></span>
- <span data-ttu-id="6c87f-230">Enumerátor musí také zveřejnit vlastnost `Current`, jejíž getter vrátí `T` reprezentující druh dat, která jsou vytvářena.</span><span class="sxs-lookup"><span data-stu-id="6c87f-230">The enumerator must also expose `Current` property whose getter returns a `T` representing the kind of data being enumerated.</span></span>
- <span data-ttu-id="6c87f-231">Enumerátor může volitelně vystavit `DisposeAsync` metodu, která může být vyvolána bez argumentů a vrátí něco, co může být `await`Ed a jehož `GetResult()` vrací `void`.</span><span class="sxs-lookup"><span data-stu-id="6c87f-231">The enumerator may optionally expose a `DisposeAsync` method that may be invoked with no arguments and that returns something that can be `await`ed and whose `GetResult()` returns `void`.</span></span>

<span data-ttu-id="6c87f-232">Tento kód:</span><span class="sxs-lookup"><span data-stu-id="6c87f-232">This code:</span></span>

```csharp
var enumerable = ...;
await foreach (T item in enumerable)
{
   ...
}
```

<span data-ttu-id="6c87f-233">je přeložen na ekvivalent:</span><span class="sxs-lookup"><span data-stu-id="6c87f-233">is translated to the equivalent of:</span></span>

```csharp
var enumerable = ...;
var enumerator = enumerable.GetAsyncEnumerator();
try
{
    while (await enumerator.MoveNextAsync())
    {
       T item = enumerator.Current;
       ...
    }
}
finally
{
    await enumerator.DisposeAsync(); // omitted, along with the try/finally, if the enumerator doesn't expose DisposeAsync
}
```

<span data-ttu-id="6c87f-234">Pokud iterace typu nevystavuje správný vzor, budou použita rozhraní.</span><span class="sxs-lookup"><span data-stu-id="6c87f-234">If the iterated type doesn't expose the right pattern, the interfaces will be used.</span></span>

### <a name="configureawait"></a><span data-ttu-id="6c87f-235">ConfigureAwait</span><span class="sxs-lookup"><span data-stu-id="6c87f-235">ConfigureAwait</span></span>

<span data-ttu-id="6c87f-236">Tato kompilace založená na vzorech umožní použít `ConfigureAwait` k použití na všech operátorech await prostřednictvím metody rozšíření `ConfigureAwait`:</span><span class="sxs-lookup"><span data-stu-id="6c87f-236">This pattern-based compilation will allow `ConfigureAwait` to be used on all of the awaits, via a `ConfigureAwait` extension method:</span></span>

```csharp
await foreach (T item in enumerable.ConfigureAwait(false))
{
   ...
}
```

<span data-ttu-id="6c87f-237">Tato akce bude založena na typech, které přidáme také do .NET, což je pravděpodobně System. Threading. Tasks. Extensions. dll:</span><span class="sxs-lookup"><span data-stu-id="6c87f-237">This will be based on types we'll add to .NET as well, likely to System.Threading.Tasks.Extensions.dll:</span></span>

```csharp
// Approximate implementation, omitting arg validation and the like
namespace System.Threading.Tasks
{
    public static class AsyncEnumerableExtensions
    {
        public static ConfiguredAsyncEnumerable<T> ConfigureAwait<T>(this IAsyncEnumerable<T> enumerable, bool continueOnCapturedContext) =>
            new ConfiguredAsyncEnumerable<T>(enumerable, continueOnCapturedContext);

        public struct ConfiguredAsyncEnumerable<T>
        {
            private readonly IAsyncEnumerable<T> _enumerable;
            private readonly bool _continueOnCapturedContext;

            internal ConfiguredAsyncEnumerable(IAsyncEnumerable<T> enumerable, bool continueOnCapturedContext)
            {
                _enumerable = enumerable;
                _continueOnCapturedContext = continueOnCapturedContext;
            }

            public ConfiguredAsyncEnumerator<T> GetAsyncEnumerator() =>
                new ConfiguredAsyncEnumerator<T>(_enumerable.GetAsyncEnumerator(), _continueOnCapturedContext);

            public struct Enumerator
            {
                private readonly IAsyncEnumerator<T> _enumerator;
                private readonly bool _continueOnCapturedContext;

                internal Enumerator(IAsyncEnumerator<T> enumerator, bool continueOnCapturedContext)
                {
                    _enumerator = enumerator;
                    _continueOnCapturedContext = continueOnCapturedContext;
                }

                public ConfiguredValueTaskAwaitable<bool> MoveNextAsync() =>
                    _enumerator.MoveNextAsync().ConfigureAwait(_continueOnCapturedContext);

                public T Current => _enumerator.Current;

                public ConfiguredValueTaskAwaitable DisposeAsync() =>
                    _enumerator.DisposeAsync().ConfigureAwait(_continueOnCapturedContext);
            }
        }
    }
}
```

<span data-ttu-id="6c87f-238">Všimněte si, že tento přístup nebude umožňovat použití `ConfigureAwait` s výčty založenými na vzorcích, ale pak už to není možné, ale v případě, že je `ConfigureAwait` vystavená jenom rozšíření na `Task`/`Task<T>`/`ValueTask`/`ValueTask<T>` a nedá se použít na libovolné neočekávané věci, protože to má smysl jenom při použití na úlohy (řídí chování implementované v rámci podpory pokračování úlohy), a proto nedává smysl při použití modelu, který může být neočekávanými úkoly.</span><span class="sxs-lookup"><span data-stu-id="6c87f-238">Note that this approach will not enable `ConfigureAwait` to be used with pattern-based enumerables, but then again it's already the case that the `ConfigureAwait` is only exposed as an extension on `Task`/`Task<T>`/`ValueTask`/`ValueTask<T>` and can't be applied to arbitrary awaitable things, as it only makes sense when applied to Tasks (it controls a behavior implemented in Task's continuation support), and thus doesn't make sense when using a pattern where the awaitable things may not be tasks.</span></span>  <span data-ttu-id="6c87f-239">Každý, kdo vrací očekávané věci, může v takových pokročilých scénářích poskytnout své vlastní chování.</span><span class="sxs-lookup"><span data-stu-id="6c87f-239">Anyone returning awaitable things can provide their own custom behavior in such advanced scenarios.</span></span>

<span data-ttu-id="6c87f-240">(Pokud můžeme mít nějaký způsob, jak podporovat obor nebo `ConfigureAwait` řešení na úrovni sestavení, pak to nebude nutné.)</span><span class="sxs-lookup"><span data-stu-id="6c87f-240">(If we can come up with some way to support a scope- or assembly-level `ConfigureAwait` solution, then this won't be necessary.)</span></span>

## <a name="async-iterators"></a><span data-ttu-id="6c87f-241">Asynchronní iterátory</span><span class="sxs-lookup"><span data-stu-id="6c87f-241">Async Iterators</span></span>

<span data-ttu-id="6c87f-242">Jazyk/kompilátor bude podporovat vytváření `IAsyncEnumerable<T>`s a `IAsyncEnumerator<T>`s, aby je bylo možné spotřebovat.</span><span class="sxs-lookup"><span data-stu-id="6c87f-242">The language / compiler will support producing `IAsyncEnumerable<T>`s and `IAsyncEnumerator<T>`s in addition to consuming them.</span></span> <span data-ttu-id="6c87f-243">V současné době jazyk podporuje zápis iterátoru jako:</span><span class="sxs-lookup"><span data-stu-id="6c87f-243">Today the language supports writing an iterator like:</span></span>

```csharp
static IEnumerable<int> MyIterator()
{
    try
    {
        for (int i = 0; i < 100; i++)
        {
            Thread.Sleep(1000);
            yield return i;
        }
    }
    finally
    {
        Thread.Sleep(200);
        Console.WriteLine("finally");
    }
}
```

<span data-ttu-id="6c87f-244">ale v těle těchto iterátorů se `await` nedají použít.</span><span class="sxs-lookup"><span data-stu-id="6c87f-244">but `await` can't be used in the body of these iterators.</span></span>  <span data-ttu-id="6c87f-245">Přidáme tuto podporu.</span><span class="sxs-lookup"><span data-stu-id="6c87f-245">We will add that support.</span></span>

### <a name="syntax"></a><span data-ttu-id="6c87f-246">Syntaxe</span><span class="sxs-lookup"><span data-stu-id="6c87f-246">Syntax</span></span>

<span data-ttu-id="6c87f-247">Stávající jazyková podpora pro iterátory odvodí iterátor metody v závislosti na tom, zda obsahuje `yield`s.</span><span class="sxs-lookup"><span data-stu-id="6c87f-247">The existing language support for iterators infers the iterator nature of the method based on whether it contains any `yield`s.</span></span>  <span data-ttu-id="6c87f-248">Totéž bude platit pro asynchronní iterátory.</span><span class="sxs-lookup"><span data-stu-id="6c87f-248">The same will be true for async iterators.</span></span>  <span data-ttu-id="6c87f-249">Takové asynchronní iterátory budou vyohraničeny a odlišeny od synchronních iterátorů prostřednictvím přidání `async` k podpisu a musí také mít `IAsyncEnumerable<T>` nebo `IAsyncEnumerator<T>` jako svůj návratový typ.</span><span class="sxs-lookup"><span data-stu-id="6c87f-249">Such async iterators will be demarcated and differentiated from synchronous iterators via adding `async` to the signature, and must then also have either `IAsyncEnumerable<T>` or `IAsyncEnumerator<T>` as its return type.</span></span>  <span data-ttu-id="6c87f-250">Například výše uvedený příklad může být zapsán jako asynchronní iterátor následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="6c87f-250">For example, the above example could be written as an async iterator as follows:</span></span>

```csharp
static async IAsyncEnumerable<int> MyIterator()
{
    try
    {
        for (int i = 0; i < 100; i++)
        {
            await Task.Delay(1000);
            yield return i;
        }
    }
    finally
    {
        await Task.Delay(200);
        Console.WriteLine("finally");
    }
}
```

<span data-ttu-id="6c87f-251">Uvažované alternativy:</span><span class="sxs-lookup"><span data-stu-id="6c87f-251">Alternatives considered:</span></span>
- <span data-ttu-id="6c87f-252">_Nepoužívá `async` v signatuře_: použití `async` je pravděpodobná kompilátorem, protože používá k určení, zda je `await` v daném kontextu platný.</span><span class="sxs-lookup"><span data-stu-id="6c87f-252">_Not using `async` in the signature_: Using `async` is likely technically required by the compiler, as it uses it to determine whether `await` is valid in that context.</span></span>  <span data-ttu-id="6c87f-253">Ale i v případě, že to není nutné, jsme navázali, že `await` lze použít pouze v metodách označených jako `async`a zdá se, že je důležité zachovat konzistenci.</span><span class="sxs-lookup"><span data-stu-id="6c87f-253">But even if it's not required, we've established that `await` may only be used in methods marked as `async`, and it seems important to keep the consistency.</span></span>
- <span data-ttu-id="6c87f-254">_Povolují se vlastní tvůrci pro `IAsyncEnumerable<T>`_ : to je něco, co můžeme v budoucnu najít, ale strojní zařízení je složité a nepodporujeme ho pro synchronní protějšky.</span><span class="sxs-lookup"><span data-stu-id="6c87f-254">_Enabling custom builders for `IAsyncEnumerable<T>`_:  That's something we could look at for the future, but the machinery is complicated and we don't support that for the synchronous counterparts.</span></span>
- <span data-ttu-id="6c87f-255">Použití _klíčového slova `iterator` v signatuře_: asynchronní iterátory by používaly `async iterator` v signatuře a `yield` lze použít pouze v `async` metodách, které zahrnují `iterator`; `iterator` by pak byly volitelné na synchronních iterátorech.</span><span class="sxs-lookup"><span data-stu-id="6c87f-255">_Having an `iterator` keyword in the signature_: Async iterators would use `async iterator` in the signature, and `yield` could only be used in `async` methods that included `iterator`; `iterator` would then be made optional on synchronous iterators.</span></span>  <span data-ttu-id="6c87f-256">V závislosti na perspektivě má tato výhoda výhodu, že by signatura metody měla být velmi jasné, ať už je povolená `yield` a zda je metoda ve skutečnosti určena pro návrat instancí typu `IAsyncEnumerable<T>` spíše než kompilátor výroby, na základě toho, zda kód používá `yield` nebo ne.</span><span class="sxs-lookup"><span data-stu-id="6c87f-256">Depending on your perspective, this has the benefit of making it very clear by the signature of the method whether `yield` is allowed and whether the method is actually meant to return instances of type `IAsyncEnumerable<T>` rather than the compiler manufacturing one based on whether the code uses `yield` or not.</span></span>  <span data-ttu-id="6c87f-257">Ale liší se od synchronních iterátorů, které nevyžadují a není možné je provést.</span><span class="sxs-lookup"><span data-stu-id="6c87f-257">But it is different from synchronous iterators, which don't and can't be made to require one.</span></span>  <span data-ttu-id="6c87f-258">Navíc někteří vývojáři nevypadají jako další syntaxe.</span><span class="sxs-lookup"><span data-stu-id="6c87f-258">Plus some developers don't like the extra syntax.</span></span>  <span data-ttu-id="6c87f-259">Pokud jsme ho navrhuji od začátku, pravděpodobně jsme to vyžádali, ale v tomto okamžiku existuje mnohem více hodnot v udržování asynchronních iterátorů blízko k synchronizaci iterátorů.</span><span class="sxs-lookup"><span data-stu-id="6c87f-259">If we were designing it from scratch, we'd probably make this required, but at this point there's much more value in keeping async iterators close to sync iterators.</span></span>

## <a name="linq"></a><span data-ttu-id="6c87f-260">LINQ</span><span class="sxs-lookup"><span data-stu-id="6c87f-260">LINQ</span></span>

<span data-ttu-id="6c87f-261">Existuje více než ~ 200 přetížení metod na `System.Linq.Enumerable` třídy, přičemž všechna fungují v `IEnumerable<T>`. Některé z těchto přijetí `IEnumerable<T>`, některé z nich vyprodukuje `IEnumerable<T>`a mnoho z nich.</span><span class="sxs-lookup"><span data-stu-id="6c87f-261">There are over ~200 overloads of methods on the `System.Linq.Enumerable` class, all of which work in terms of `IEnumerable<T>`; some of these accept `IEnumerable<T>`, some of them produce `IEnumerable<T>`, and many do both.</span></span>  <span data-ttu-id="6c87f-262">Přidání podpory LINQ pro `IAsyncEnumerable<T>` by mohlo vést k duplikaci všech těchto přetížení pro jinou ~ 200.</span><span class="sxs-lookup"><span data-stu-id="6c87f-262">Adding LINQ support for `IAsyncEnumerable<T>` would likely entail duplicating all of these overloads for it, for another ~200.</span></span>  <span data-ttu-id="6c87f-263">A vzhledem k tomu, že `IAsyncEnumerator<T>` je pravděpodobně běžnější jako samostatná entita v asynchronním světě, než `IEnumerator<T>` je na synchronním světě, můžeme potenciálně potřebovat další přetížení ~ 200, která pracují s `IAsyncEnumerator<T>`.</span><span class="sxs-lookup"><span data-stu-id="6c87f-263">And since `IAsyncEnumerator<T>` is likely to be more common as a standalone entity in the asynchronous world than `IEnumerator<T>` is in the synchronous world, we could potentially need another ~200 overloads that work with `IAsyncEnumerator<T>`.</span></span>  <span data-ttu-id="6c87f-264">Kromě toho velký počet přetížení vede k predikátům (například `Where`, které přebírají `Func<T, bool>`), a může být žádoucí, aby bylo přetížení založené na `IAsyncEnumerable<T>`, které se zabývat synchronním i asynchronním predikátem (například `Func<T, ValueTask<bool>>` `Func<T, bool>`).</span><span class="sxs-lookup"><span data-stu-id="6c87f-264">Plus, a large number of the overloads deal with predicates (e.g. `Where` that takes a `Func<T, bool>`), and it may be desirable to have `IAsyncEnumerable<T>`-based overloads that deal with both synchronous and asynchronous predicates (e.g. `Func<T, ValueTask<bool>>` in addition to `Func<T, bool>`).</span></span>  <span data-ttu-id="6c87f-265">I když to neplatí pro všechna z nich. ~ 400 nová přetížení, hrubý výpočet je, že by měl být použitelný na polovinu, což znamená další ~ 200 přetížení, celkem ~ 600 nové metody.</span><span class="sxs-lookup"><span data-stu-id="6c87f-265">While this isn't applicable to all of the now ~400 new overloads, a rough calculation is that it'd be applicable to half, which means another ~200 overloads, for a total of ~600 new methods.</span></span>

<span data-ttu-id="6c87f-266">To je rovnoměrné rozmístění počtu rozhraní API, které je možné ještě více při zvážení knihoven rozšíření, jako jsou interaktivní rozšíření (IX).</span><span class="sxs-lookup"><span data-stu-id="6c87f-266">That is a staggering number of APIs, with the potential for even more when extension libraries like Interactive Extensions (Ix) are considered.</span></span>  <span data-ttu-id="6c87f-267">Ale IX už má implementaci mnoha z nich a zdá se, že nebudete mít velký důvod na duplikaci této práce; místo toho doporučujeme komunitě vylepšit IX a doporučit ji, když vývojáři chtějí používat LINQ s `IAsyncEnumerable<T>`.</span><span class="sxs-lookup"><span data-stu-id="6c87f-267">But Ix already has an implementation of many of these, and there doesn't seem to be a great reason to duplicate that work; we should instead help the community improve Ix and recommend it for when developers want to use LINQ with `IAsyncEnumerable<T>`.</span></span>

<span data-ttu-id="6c87f-268">K dispozici je také chyba syntaxe porozumění dotazu.</span><span class="sxs-lookup"><span data-stu-id="6c87f-268">There is also the issue of query comprehension syntax.</span></span>  <span data-ttu-id="6c87f-269">Porovnávací metoda porozumění dotazu by jim umožnila "pracovat" s některými operátory, např. Pokud IX poskytuje následující metody:</span><span class="sxs-lookup"><span data-stu-id="6c87f-269">The pattern-based nature of query comprehensions would allow them to "just work" with some operators, e.g. if Ix provides the following methods:</span></span>

```csharp
public static IAsyncEnumerable<TResult> Select<TSource, TResult>(this IAsyncEnumerable<TSource> source, Func<TSource, TResult> func);
public static IAsyncEnumerable<T> Where(this IAsyncEnumerable<T> source, Func<T, bool> func);
```

<span data-ttu-id="6c87f-270">pak tento C# kód bude "jenom fungovat":</span><span class="sxs-lookup"><span data-stu-id="6c87f-270">then this C# code will "just work":</span></span>

```csharp
IAsyncEnumerable<int> enumerable = ...;
IAsyncEnumerable<int> result = from item in enumerable
                               where item % 2 == 0
                               select item * 2;
```

<span data-ttu-id="6c87f-271">Neexistuje však žádná syntaxe porozumění dotazu, která podporuje použití `await` v klauzulích, takže pokud bylo přidáno IX, například:</span><span class="sxs-lookup"><span data-stu-id="6c87f-271">However, there is no query comprehension syntax that supports using `await` in the clauses, so if Ix added, for example:</span></span>

```csharp
public static IAsyncEnumerable<TResult> Select<TSource, TResult>(this IAsyncEnumerable<TSource> source, Func<TSource, ValueTask<TResult>> func);
```

<span data-ttu-id="6c87f-272">pak by to bylo "jenom fungovat":</span><span class="sxs-lookup"><span data-stu-id="6c87f-272">then this would "just work":</span></span>

```csharp
IAsyncEnumerable<string> result = from url in urls
                                  where item % 2 == 0
                                  select SomeAsyncMethod(item);

async ValueTask<int> SomeAsyncMethod(int item)
{
    await Task.Yield();
    return item * 2;
}
```

<span data-ttu-id="6c87f-273">ale neexistuje žádný způsob, jak ho zapsat pomocí `await` vloženého do klauzule `select`.</span><span class="sxs-lookup"><span data-stu-id="6c87f-273">but there'd be no way to write it with the `await` inline in the `select` clause.</span></span>  <span data-ttu-id="6c87f-274">V rámci samostatného úsilí jsme se mohli podívat na přidání `async { ... }`ch výrazů do jazyka. v tomto bodě bychom mohli dovolit použití v porozumění dotazům a výše uvedené místo toho může být zapsáno jako:</span><span class="sxs-lookup"><span data-stu-id="6c87f-274">As a separate effort, we could look into adding `async { ... }` expressions to the language, at which point we could allow them to be used in query comprehensions and the above could instead be written as:</span></span>

```csharp
IAsyncEnumerable<int> result = from item in enumerable
                               where item % 2 == 0
                               select async
                               {
                                   await Task.Yield();
                                   return item * 2;
                               };
```

<span data-ttu-id="6c87f-275">nebo pokud chcete povolit použití `await` přímo ve výrazech, jako je podpora `async from`.</span><span class="sxs-lookup"><span data-stu-id="6c87f-275">or to enabling `await` to be used directly in expressions, such as by supporting `async from`.</span></span>  <span data-ttu-id="6c87f-276">Je ale nepravděpodobné, že by to mělo vliv na zbytek funkce nastavenou jedním ze způsobů nebo na druhou, a to není zvlášť vysoká hodnota pro investici do té chvíle, takže v tomto návrhu není ještě nic dalšího.</span><span class="sxs-lookup"><span data-stu-id="6c87f-276">However, it's unlikely a design here would impact the rest of the feature set one way or the other, and this isn't a particularly high-value thing to invest in right now, so the proposal is to do nothing additional here right now.</span></span>

## <a name="integration-with-other-asynchronous-frameworks"></a><span data-ttu-id="6c87f-277">Integrace s dalšími asynchronními rozhraními</span><span class="sxs-lookup"><span data-stu-id="6c87f-277">Integration with other asynchronous frameworks</span></span>

<span data-ttu-id="6c87f-278">Integrace s `IObservable<T>` a dalšími asynchronními rozhraními (např. reaktivní streamy) by se měla provádět na úrovni knihovny, nikoli na úrovni jazyka.</span><span class="sxs-lookup"><span data-stu-id="6c87f-278">Integration with `IObservable<T>` and other asynchronous frameworks (e.g. reactive streams) would be done at the library level rather than at the language level.</span></span>  <span data-ttu-id="6c87f-279">Všechna data z `IAsyncEnumerator<T>` lze například publikovat do `IObserver<T>` jednoduše pomocí `await foreach`"ze enumerátoru a `OnNext`" dat do pozorovatele, aby mohla být metoda rozšíření `AsObservable<T>` možná.</span><span class="sxs-lookup"><span data-stu-id="6c87f-279">For example, all of the data from an `IAsyncEnumerator<T>` can be published to an `IObserver<T>` simply by `await foreach`'ing over the enumerator and `OnNext`'ing the data to the observer, so an `AsObservable<T>` extension method is possible.</span></span>  <span data-ttu-id="6c87f-280">Použití `IObservable<T>` v `await foreach` vyžaduje ukládání dat do vyrovnávací paměti (v případě, že se stále zpracovává předchozí položka), ale takový adaptér Push-Pull lze snadno implementovat, aby bylo možné `IObservable<T>` získat z `IAsyncEnumerator<T>`.</span><span class="sxs-lookup"><span data-stu-id="6c87f-280">Consuming an `IObservable<T>` in a `await foreach` requires buffering the data (in case another item is pushed while the previous item is still being processing), but such a push-pull adapter can easily be implemented to enable an `IObservable<T>` to be pulled from with an `IAsyncEnumerator<T>`.</span></span>  <span data-ttu-id="6c87f-281">Atd.  RX/IX již poskytuje prototypy těchto implementací a knihovny jako https://github.com/dotnet/corefx/tree/master/src/System.Threading.Channels poskytují různé druhy datových struktur vyrovnávací paměti.</span><span class="sxs-lookup"><span data-stu-id="6c87f-281">Etc.  Rx/Ix already provide prototypes of such implementations, and libraries like https://github.com/dotnet/corefx/tree/master/src/System.Threading.Channels provide various kinds of buffering data structures.</span></span>  <span data-ttu-id="6c87f-282">Jazyk nemusí být v této fázi zahrnut.</span><span class="sxs-lookup"><span data-stu-id="6c87f-282">The language need not be involved at this stage.</span></span>
