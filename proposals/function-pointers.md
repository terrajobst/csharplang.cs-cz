---
ms.openlocfilehash: d9080202f9413f8beb80db222d47f5fc082ae641
ms.sourcegitcommit: f3170512e7a3193efbcea52ec330648375e36915
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/11/2020
ms.locfileid: "79485502"
---
# <a name="function-pointers"></a>Ukazatele na funkce

## <a name="summary"></a>Souhrn

Tento návrh poskytuje jazykové konstrukce, které zveřejňují operační kódy IL, ke kterým momentálně nelze efektivně přistupovat, nebo C# vůbec, v dnešní době: `ldftn` a `calli`. Tyto operační kódy IL můžou být důležité v kódu s vysokým výkonem a vývojáři potřebují účinný způsob, jak k nim přistupovat.

## <a name="motivation"></a>Motivační

Motivace a pozadí této funkce jsou popsány v následujícím problému (jako je potenciální implementace funkce):

https://github.com/dotnet/csharplang/issues/191

Toto je alternativní návrh návrhu pro [vnitřní objekty kompilátoru](https://github.com/dotnet/csharplang/blob/master/proposals/intrinsics.md) .

## <a name="detailed-design"></a>Podrobný návrh

### <a name="function-pointers"></a>Ukazatele na funkce

Jazyk umožní deklaraci ukazatelů na funkce pomocí syntaxe `delegate*`. Úplná syntaxe je podrobněji popsána v následující části, ale je určena pro použití syntaxe `Func` a `Action` deklarace typu.

``` csharp
unsafe class Example {
    void Example(Action<int> a, delegate*<int, void> f) {
        a(42);
        f(42);
    }
}
```

Tyto typy jsou reprezentovány pomocí typu ukazatele na funkci, jak je uvedeno v ECMA-335. To znamená, že volání `delegate*` bude používat `calli`, kde vyvolání `delegate` bude používat `callvirt` na `Invoke` metodě.
Syntakticky, i když je volání identické pro obě konstrukce.

Definice těchto ukazatelů metody ECMA-335 zahrnuje konvenci volání jako součást signatury typu (oddíl 7,1).
Výchozí konvence volání bude `managed`. Alternativní formuláře lze určit přidáním vhodného modifikátoru za `delegate*` syntaxe: `managed`, `cdecl`, `stdcall`, `thiscall`nebo `unmanaged`. Příklad:

``` csharp
// This method will be invoked using the cdecl calling convention
delegate* cdecl<int, int>;

// This method will be invoked using the stdcall calling convention
delegate* stdcall<int, int>;
```

Převody mezi typy `delegate*` se provádí na základě jejich signatury, včetně konvence volání.

``` csharp
unsafe class Example {
    void Conversions() {
        delegate*<int, int, int> p1 = ...;
        delegate* managed<int, int, int> p2 = ...;
        delegate* cdecl<int, int, int> p3 = ...;

        p1 = p2; // okay p1 and p2 have compatible signatures
        Console.WriteLine(p2 == p1); // True
        p2 = p3; // error: calling conventions are incompatible
    }
}
```

Typ `delegate*` je typ ukazatele, což znamená, že má všechny možnosti a omezení standardního typu ukazatele:

- Platné pouze v kontextu `unsafe`.
- Metody, které obsahují parametr `delegate*` nebo návratový typ, lze volat pouze z kontextu `unsafe`.
- Nelze převést na `object`.
- Nelze použít jako obecný argument.
- Může implicitně převést `delegate*` na `void*`.
- Lze explicitně převést z `void*` na `delegate*`.

Podléhající

- Vlastní atributy nelze použít pro `delegate*` ani žádný z jejích elementů.
- Parametr `delegate*` nelze označit jako `params`
- `delegate*` typ má všechna omezení normálního typu ukazatele.

### <a name="function-pointer-syntax"></a>Syntaxe ukazatele na funkci

Úplná syntaxe ukazatele na funkci je reprezentovaná následující gramatikou:

```antlr
pointer_type
    : ...
    | funcptr_type
    ;

funcptr_type
    : 'delegate' '*' calling_convention? '<' (funcptr_parameter_modifier? type ',')* funcptr_return_modifier? return_type '>'
    ;

calling_convention
    : 'cdecl'
    | 'managed'
    | 'stdcall'
    | 'thiscall'
    | 'unmanaged'
    ;

funcptr_parameter_modifier
    : 'ref'
    | 'out'
    | 'in'
    ;

funcptr_return_modifier
    : 'ref'
    | 'ref readonly'
    ;
```

Konvence volání `unmanaged` představuje výchozí konvenci volání pro nativní kód na aktuální platformě a je kódována jako WINAPI.
Všechny `calling_convention`s jsou kontextová klíčová slova, pokud předcházejí `delegate*`.

``` csharp
delegate int Func1(string s);
delegate Func1 Func2(Func1 f);

// Function pointer equivalent without calling convention
delegate*<string, int>;
delegate*<delegate*<string, int>, delegate*<string, int>>;

// Function pointer equivalent with calling convention
delegate* managed<string, int>;
delegate*<delegate* managed<string, int>, delegate*<string, int>>;
```

### <a name="function-pointer-conversions"></a>Převody ukazatelů na funkce

V nezabezpečeném kontextu je sada dostupných implicitních převodů (implicitní převody) rozšířena tak, aby zahrnovala následující implicitní převody ukazatelů:
- [_Existující převody_](https://github.com/dotnet/csharplang/blob/master/spec/unsafe-code.md#pointer-conversions)
- Z _funcptr\_typ_ `F0` na jiný _typ\_funcptr_ `F1`, pokud jsou splněné všechny následující podmínky:
    - `F0` a `F1` mají stejný počet parametrů, přičemž každý parametr `D0n` v `F0` má stejné `ref`, `out`nebo `in` modifikátory jako odpovídající parametr `D1n` v `F1`.
    - Pro každý parametr hodnoty (parametr bez `ref`, `out`nebo modifikátor `in`), konverze identity, implicitní převod odkazu nebo implicitní převod ukazatele existují z typu parametru v `F0` k odpovídajícímu typu parametru v `F1`.
    - Pro každý `ref`, `out`nebo parametr `in` je typ parametru v `F0` stejný jako odpovídající typ parametru v `F1`.
    - Pokud je návratový typ podle hodnoty (No `ref` ani `ref readonly`), existuje identita, implicitní odkaz nebo implicitní převod ukazatele z návratového typu `F1` na návratový typ `F0`.
    - Pokud je návratový typ odkazem (`ref` nebo `ref readonly`), návratový typ a modifikátory `ref` `F1` jsou stejné jako návratový typ a `ref` modifikátory `F0`.
    - Konvence volání `F0` je stejná jako konvence volání `F1`.

### <a name="allow-address-of-to-target-methods"></a>Povolení adres pro cílové metody

Skupiny metod budou nyní povoleny jako argumenty pro výraz adresy. Typ takového výrazu bude `delegate*`, který má ekvivalentní signaturu cílové metody a spravované konvence volání:

``` csharp
unsafe class Util {
    public static void Log() { }

    void Use() {
        delegate*<void> ptr1 = &Util.Log;

        // Error: type "delegate*<void>" not compatible with "delegate*<int>";
        delegate*<int> ptr2 = &Util.Log;

        // Okay. Conversion to void* is always allowed.
        void* v = &Util.Log;
   }
}
```

V nezabezpečeném kontextu je metoda `M` kompatibilní s typem ukazatele funkce `F` Pokud jsou splněné všechny následující podmínky:
- `M` a `F` mají stejný počet parametrů, přičemž každý parametr v `D` má jako odpovídající parametr v `out`stejné modifikátory `ref`, `in` nebo `F`.
- Pro každý parametr hodnoty (parametr bez `ref`, `out`nebo modifikátor `in`), konverze identity, implicitní převod odkazu nebo implicitní převod ukazatele existují z typu parametru v `M` k odpovídajícímu typu parametru v `F`.
- Pro každý `ref`, `out`nebo parametr `in` je typ parametru v `M` stejný jako odpovídající typ parametru v `F`.
- Pokud je návratový typ podle hodnoty (No `ref` ani `ref readonly`), existuje identita, implicitní odkaz nebo implicitní převod ukazatele z návratového typu `F` na návratový typ `M`.
- Pokud je návratový typ odkazem (`ref` nebo `ref readonly`), návratový typ a modifikátory `ref` `F` jsou stejné jako návratový typ a `ref` modifikátory `M`.
- Konvence volání `M` je stejná jako konvence volání `F`.
- `M` je statická metoda.

V nezabezpečeném kontextu existuje implicitní převod z výrazu adresy, jehož cílem je skupina metod `E` na kompatibilní typ ukazatele na funkci `F` Pokud `E` obsahuje alespoň jednu metodu, která je použita v běžné formě na seznam argumentů konstruovaný pomocí typů parametrů a modifikátorů `F`, jak je popsáno v následujícím tématu.
- `M` je vybrána jedna metoda, která odpovídá vyvolání metody `E(A)` formuláře s těmito úpravami:
   - Seznam argumentů `A` je seznam výrazů, každý klasifikovaný jako proměnná a s typem a modifikátorem (`ref`, `out`nebo `in`) odpovídajícího _formálního\_parametru\_seznam_ `D`.
   - Kandidátské metody jsou pouze metody, které jsou použity pouze v jejich normálním tvaru, nikoli v rozbalených formulářích.
- Pokud algoritmus vyvolání metody vyvolá chybu, dojde k chybě při kompilaci. V opačném případě algoritmus vytvoří jednu nejlepší metodu `M` se stejným počtem parametrů jako `F` a převod je považován za existující.
- Vybraná metoda `M` musí být kompatibilní (jak je definováno výše) s typem ukazatele na funkci `F`. V opačném případě dojde k chybě při kompilaci.
- Výsledkem převodu je ukazatel na funkci typu `F`.

Implicitní převod existuje z výrazu adresy, jehož cílem je skupina metod `E` `void*`, pokud je v `E`jenom jedna statická metoda `M`.
Pokud existuje jedna statická metoda, pak je `M`jedinou nejlepší metodou z `E`.
V opačném případě dojde k chybě při kompilaci.

To znamená, že vývojáři mohou záviset na pravidlech rozlišení přetížení pro práci ve spojení s operátorem adresy:

``` csharp
unsafe class Util {
    public static void Log() { }
    public static void Log(string p1) { }
    public static void Log(int i) { };

    void Use() {
        delegate*<void> a1 = &Log; // Log()
        delegate*<int, void> a2 = &Log; // Log(int i)

        // Error: ambiguous conversion from method group Log to "void*"
        void* v = &Log;
    }
```

Operátor address-of bude implementován pomocí instrukcí `ldftn`.

Omezení této funkce:

- Platí pouze pro metody označené jako `static`.
- Místní funkce, které nejsou`static`, se nedají použít v `&`. Podrobnosti implementace těchto metod jsou záměrně Nespecifikovány jazykem. To zahrnuje to, jestli jsou statické vs. instance, nebo přesně to, s jakou signaturou se generují.

### <a name="better-function-member"></a>Lepší člen funkce

Specifikace členů lepší funkce se změní tak, aby zahrnovala následující řádek:

> `delegate*` je konkrétnější než `void*`

To znamená, že je možné přetížit na `void*` a `delegate*` a stále sensibly použít operátor address-of.

## <a name="open-issues"></a>Otevřené problémy

### <a name="nativecallableattribute"></a>NativeCallableAttribute

Toto je atribut používaný modulem CLR, aby při vyvolání nedocházelo ke spravovanému nativnímu prologu. Metody označené tímto atributem lze volat pouze z nativního kódu, nikoli spravované (nemohou volat metody, vytvořit delegáta atd.). Atribut není pro mscorlib specifický; modul runtime zpracuje všechny atributy s tímto názvem se stejnou sémantikou.

Je možné, že modul runtime a jazyk společně spolupracují, aby to plně podporoval. Jazyk by se mohl rozvažovat za `static` členů s atributem `NativeCallable` jako `delegate*` se zadanou konvencí volání.

``` csharp
unsafe class NativeCallableExample {
    [NativeCallable(CallingConvention.CDecl)]
    static void CloseHandle(IntPtr p) => Marshal.FreeHGlobal(p);

    void Use() {
        delegate*<IntPtr, void> p1 = &CloseHandle; // Error: Invalid calling convention

        delegate* cdecl<IntPtr, void> p2 = &CloseHandle; // Okay
    }
}

```

Dále by byl jazyk nejspíš chtít:

- Označte jakákoli spravovaná volání metody označené pomocí `NativeCallable` jako chybu. Vzhledem k tomu, že funkce nemůže být vyvolána ze spravovaného kódu, kompilátor by měl vývojářům zabránit v pokusu o takové vyvolání.
- Zabraňuje převodům skupin metod na `delegate`, když je metoda označena pomocí `NativeCallable`.

Tato není nutná k podpoře `NativeCallable` i když. Kompilátor může podporovat atribut `NativeCallable`, protože používá existující syntaxi. Aby bylo možné tento program přetypování na správný podpis `delegate*`, je třeba přetypovat na `void*`. To by nebylo co nejhorší než podpora dnes.

``` csharp
void* v = &CloseHandle;
delegate* cdecl<IntPtr, bool> f1 = (delegate* cdecl<IntPtr, bool>)v;
```

### <a name="extensible-set-of-unmanaged-calling-conventions"></a>Rozšiřitelná sada nespravovaných konvencí volání

Sada nespravovaných konvencí volání podporovaná aktuálními kódováními ECMA-335 je zastaralá. Viděli jsme žádosti o přidání podpory pro více konvencí nespravovaného volání, například:

- [vectorcall](https://docs.microsoft.com/cpp/cpp/vectorcall) https://github.com/dotnet/coreclr/issues/12120
- StdCall s explicitním tímto https://github.com/dotnet/coreclr/pull/23974#issuecomment-482991750

Návrh této funkce by měl v budoucnu umožňovat rozšiřování sady nespravovaných konvencí volání. Tyto problémy zahrnují omezené místo pro konvence volání pro kódování (12 z 16 hodnot, které jsou pořízeny v `IMAGE_CEE_CS_CALLCONV_MASK`) a počet míst, která je potřeba retušovat, aby bylo možné přidat novou konvenci volání. Potenciálním řešením je zavést nové kódování, které představuje konvenci volání pomocí [`System.Runtime.InteropServices.CallingConvention`](https://docs.microsoft.com/en-us/dotnet/api/system.runtime.interopservices.callingconvention) Enum.

Pro referenci https://github.com/llvm/llvm-project/blob/master/llvm/include/llvm/IR/CallingConv.h obsahuje seznam konvencí volání, které podporuje LLVM. I když je pravděpodobné, že .NET bude někdy potřebovat podporu všech těchto, ukazuje, že prostor konvencí volání je velmi bohatý.

## <a name="considerations"></a>Požadavky

### <a name="allow-instance-methods"></a>Povolení metod instancí

Návrh se dá rozšířit tak, aby podporoval metody instance, využitím `EXPLICITTHIS` konvence volání CLI (s názvem `instance` C# v kódu). Tato forma ukazatelů na funkce rozhraní příkazového řádku vloží parametr `this` jako explicitní první parametr syntaxe ukazatele na funkci.

``` csharp
unsafe class Instance {
    void Use() {
        delegate* instance<Instance, string> f = &ToString;
        f(this);
    }
}
```

To je zvuk, ale přidává k návrhu nějaké komplikace. Zejména vzhledem k tomu, že ukazatele na funkce, které se liší konvencí volání `instance` a `managed` by byly nekompatibilní, i když jsou oba případy použity k C# vyvolání spravovaných metod se stejnou signaturou. V každém případě se v každém případě považuje za užitečné, aby existovala jednoduchá práce: použijte `static` místní funkci.

``` csharp
unsafe class Instance {
    void Use() {
        static string toString(Instance i) = i.ToString();
        delgate*<Instance, string> f = &toString;
        f(this);
    }
}
```

### <a name="dont-require-unsafe-at-declaration"></a>Nevyžadovat bezpečné v deklaraci

Místo vyžadování `unsafe` při každém použití `delegate*`ji vyžadovat pouze v místě, kde je skupina metod převedena na `delegate*`. Zde jsou uvedené základní bezpečnostní problémy, které jsou v provozu (s vědomím, že toto sestavení nelze uvolnit, pokud je hodnota aktivní). Vyžadování `unsafe` v jiných umístěních je možné zobrazit jako nadměrné.

Tímto způsobem byl návrh původně zamýšlen. Ale výsledná jazyková pravidla jsou velmi neužitečná. Není možné skrýt fakt, že se jedná o hodnotu ukazatele a že se uchovává i bez klíčového slova `unsafe`. Například převod na `object` nelze povolit, nemůže být členem `class`atd... C# Návrh je vyžadován `unsafe` pro všechny použití ukazatelů, a proto tento návrh následuje.

Vývojáři budou mít stále možnost předcházet _zabezpečenou_ obálku nad `delegate*`mi hodnotami stejným způsobem jako v dnešní době pro normální typy ukazatelů. Vezměte v úvahu:

``` csharp
unsafe struct Action {
    delegate*<void> _ptr;

    Action(delegate*<void> ptr) => _ptr = ptr;
    public void Invoke() => _ptr();
}
```

### <a name="using-delegates"></a>Použití delegátů

Místo použití nového prvku syntaxe `delegate*`jednoduše použijte existující typy `delegate` s `*` za typ:

``` csharp
Func<object, object, bool>* ptr = &object.ReferenceEquals;
```

Zpracování konvence volání lze provést zadáním poznámky k `delegate` typů s atributem, který určuje `CallingConvention` hodnotu. Chybějící atribut by znamenal spravovanou konvenci volání.

Kódování tohoto v IL je problematické. Nadřazená hodnota musí být reprezentovaná jako ukazatel, ale také musí:

1. Mají jedinečný typ pro povolení přetížení s různými typy ukazatelů na funkce.
1. Být ekvivalentní pro účely OHI napříč hranicemi sestavení.

Poslední bod je zvláště problematický. To znamená, že každé sestavení, které používá `Func<int>*` musí kódovat ekvivalentní typ v metadatech, přestože `Func<int>*` je definováno v sestavení, ale neřídí.
Navíc jakýkoli jiný typ, který je definován s názvem `System.Func<T>` v sestavení, které není mscorlib, musí být jiný než verze definovaná v mscorlib.

Jedna z možností, které se prozkoumala, byl takový ukazatel, který se vyvolal jako `mod_req(Func<int>) void*`. Tato činnost nefunguje, i když `mod_req` nemůže vytvořit vazby k `TypeSpec` a tudíž nemůže cílit na obecné vytváření instancí.

### <a name="named-function-pointers"></a>Pojmenované ukazatele funkcí

Syntaxe ukazatele na funkci může být náročná, zejména ve složitých případech, jako jsou vnořené ukazatele na funkce. Namísto toho, aby vývojáři nastavili podpis pokaždé, když jazyk může pojmenovat deklarace ukazatelů na funkce, jak je provedeno `delegate`.

``` csharp
func* void Action();

unsafe class NamedExample {
    void M(Action a) {
        a();
    }
}
```

Zde je uvedená část problému. základní primitivní rozhraní CLI nemá názvy, proto by byl čistě C# vynálezný a vyžaduje, aby bylo možné povolit bitovou kopii metadat. To je doable, ale jedná se o důležitou práci. V podstatě je potřeba C# , aby měl k typu def tabulky čistě pro tyto názvy.

I když byly prověřeny argumenty pojmenovaných funkcí, které jsme zjistili, že by mohly být stejné i pro řadu dalších scénářů. Například by bylo vhodné deklarovat pojmenované řazené kolekce členů a snížit tak nutnost zadávat úplný podpis ve všech případech.

``` csharp
(int x, int y) Point;

class NamedTupleExample {
    void M(Point p) {
        Console.WriteLine(p.x);
    }
}
```

Po diskuzi se rozhodli, že nepovolíte pojmenovanou deklaraci `delegate*`ch typů. Pokud zjistíme, že je potřeba na základě zpětné vazby na používání ze zákaznického oddělení, prozkoumáme řešení pojmenování, které funguje pro ukazatele na funkce, řazené kolekce členů, obecné typy atd... To je pravděpodobně podobné jako u jiných návrhů, jako je plná podpora `typedef` v jazyce.

## <a name="future-considerations"></a>Budoucí požadavky

### <a name="static-local-functions"></a>statické místní funkce

To odkazuje na [Návrh](https://github.com/dotnet/csharplang/issues/1565) , který povolí modifikátor `static` na místních funkcích. Tato funkce by měla být zaručená, že se bude generovat jako `static` a s přesným podpisem zadaným ve zdrojovém kódu. Tato funkce by měla být platným argumentem pro `&`, protože neobsahuje žádné z problémů, které místní funkce mají dnes

### <a name="static-delegates"></a>statické Delegáti

To odkazuje na [Návrh](https://github.com/dotnet/csharplang/issues/302) , který umožňuje deklaraci `delegate` typů, které mohou odkazovat pouze na `static` členy. Výhodou toho, že tyto instance `delegate` můžou být ve scénářích citlivých na výkon a lepší přidělit.

Pokud je funkce ukazatele na funkci implementována, `static delegate` návrh pravděpodobně bude zavřen. Navrhovanou výhodou této funkce je přidělení volného místa. Nedávno se ale zjistilo, že v důsledku uvolňování sestavení není možné dosáhnout jejich posledního šetření. Musí existovat silný popisovač od `static delegate` k metodě, na kterou odkazuje, aby bylo sestavení uvolněno z něj.

Aby bylo možné zachovat všechny `static delegate` instance, bude nutné přidělit novému popisovači, který spustí čítač pro cíle návrhu. Existovaly některé návrhy, u kterých by bylo možné přidělení vytvořit v rámci jednoho přidělení na pracovišti, ale to bylo složité a nemohlo by to působit za obchod.

To znamená, že se vývojářům v podstatě musí rozhodnout mezi těmito kompromisy:

1. Bezpečnost na tváři uvolnění sestavení: to vyžaduje přidělení, takže `delegate` je už dostatečná možnost.
1. Žádná bezpečnost při uvolňování sestavení: použijte `delegate*`. To lze zabalit do `struct`, aby bylo možné použití mimo `unsafe` kontext ve zbytku kódu.
