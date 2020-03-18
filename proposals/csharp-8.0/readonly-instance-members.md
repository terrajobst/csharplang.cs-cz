---
ms.openlocfilehash: d0bb80305ccc755a555cf47a1d010fc4cb9a7bcd
ms.sourcegitcommit: 5688b13e66dd77b224a1223338de9e3b1f66d7f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/13/2020
ms.locfileid: "79485516"
---
# <a name="readonly-instance-members"></a>Členové instance ReadOnly

Championed problém: <https://github.com/dotnet/csharplang/issues/1710>

## <a name="summary"></a>Souhrn
[summary]: #summary

Poskytněte způsob, jak určit členy jednotlivých instancí ve struktuře, neměnit stav, stejným způsobem, jako `readonly struct` nemění členy instance stav.

Je potřeba poznamenat, že `readonly instance member`! = `pure instance member`. Člen instance `pure` garantuje, že žádný stav nebude změněn. Člen instance `readonly` garantuje pouze to, že stav instance nebude změněn.

Všechny členy instance u `readonly struct` lze považovat za implicitně `readonly instance members`. Explicitní `readonly instance members` deklarované ve strukturách, které nejsou jen pro čtení, se chovají stejným způsobem. Například by pořád vytvořili skryté kopie, pokud jste volali člen instance (na aktuální instanci nebo v poli instance), který sám o sobě není jen pro čtení.

## <a name="motivation"></a>Motivační
[motivation]: #motivation

V dnešní době mají uživatelé možnost vytvářet `readonly struct` typy, které kompilátor vynutil, že všechna pole jsou jen pro čtení (a podle rozšíření, která nemění členy instance). Existují však situace, kdy máte existující rozhraní API, které zpřístupňuje dostupná pole nebo obsahuje kombinaci obdobných a nepřiměřeně nevyhovujících členů. Za těchto okolností nelze typ označit jako `readonly` (může se jednat o zásadní změnu).

Tato situace obvykle nemá mnohem dopad, s výjimkou parametrů `in`. S parametry `in` pro struktury, které nejsou jen pro čtení, kompilátor vytvoří kopii parametru pro každé vyvolání člena instance, protože nemůže zaručit, že vyvolání neupravuje vnitřní stav. To může vést k velkému množství kopií a horšímu celkovému výkonu, než kdyby jste právě prošli strukturu přímo podle hodnoty. Příklad naleznete v tomto kódu v [sharplab](https://sharplab.io/#v2:CYLg1APgAgDABFAjAbgLACgNQMxwM4AuATgK4DGBcAagKYUD2RATBgN4ZycK4BmANvQCGlAB5p0XbnH5DKAT3GSOXHNIHC4AGRoA7AOYEAFgGUAjiUFEawZZ3YTJXPTQK3H9x54QB2OAAoROAAqOBEASjgwNy8YvzlguDkwxS8AXzd09EysXCgmOABhOA8VXnVKAFk/AEsdajoCRnyAN0E+EhoIks8oX1b2mgA6bX0jMwsrYEi4fo7h3QMTc0trFM5M1KA==) .

Některé další scénáře, ve kterých mohou nastat skryté kopie, zahrnují `static readonly fields` a `literals`. Pokud jsou v budoucnu podporované, `blittable constants` by se ukončily na stejném člunu; To znamená, že všechny aktuálně vyžadují úplnou kopii (při vyvolání člena instance), pokud struktura není označena `readonly`.

## <a name="design"></a>Návrh
[design]: #design

Umožní uživateli určit, že je člen instance sám sebe, `readonly` a neupravuje stav instance (se všemi odpovídajícími ověřeními provedenými kompilátorem). Příklad:

```csharp
public struct Vector2
{
    public float x;
    public float y;

    public readonly float GetLengthReadonly()
    {
        return MathF.Sqrt(LengthSquared);
    }

    public float GetLength()
    {
        return MathF.Sqrt(LengthSquared);
    }

    public readonly float GetLengthIllegal()
    {
        var tmp = MathF.Sqrt(LengthSquared);

        x = tmp;    // Compiler error, cannot write x
        y = tmp;    // Compiler error, cannot write y

        return tmp;
    }

    public float LengthSquared
    {
        readonly get
        {
            return (x * x) +
                   (y * y);
        }
    }
}

public static class MyClass
{
    public static float ExistingBehavior(in Vector2 vector)
    {
        // This code causes a hidden copy, the compiler effectively emits:
        //    var tmpVector = vector;
        //    return tmpVector.GetLength();
        //
        // This is done because the compiler doesn't know that `GetLength()`
        // won't mutate `vector`.

        return vector.GetLength();
    }

    public static float ReadonlyBehavior(in Vector2 vector)
    {
        // This code is emitted exactly as listed. There are no hidden
        // copies as the `readonly` modifier indicates that the method
        // won't mutate `vector`.

        return vector.GetLengthReadonly();
    }
}
```

Pro přistupující objekty vlastnosti lze použít jen pro čtení k označení, že `this` nebudou v přístupovém objektu vhodné. Následující příklady mají metody setter pro čtení, protože tyto přistupující objekty mění stav pole člen, ale nemění hodnotu tohoto pole člena.

```csharp
public int Prop1
{
    readonly get
    {
        return this._store["Prop1"];
    }
    readonly set
    {
        this._store["Prop1"] = value;
    }
}
```

Při použití `readonly` na syntaxi vlastnosti znamená, že všechny přistupující objekty jsou `readonly`.

```csharp
public readonly int Prop2
{
    get
    {
        return this._store["Prop2"];
    }
    set
    {
        this._store["Prop2"] = value;
    }
}
```

Vlastnost ReadOnly lze použít pouze pro přistupující objekty, které neodpovídají nadřazenému typu.

```csharp
public int Prop3
{
    readonly get
    {
        return this._prop3;
    }
    set
    {
        this._prop3 = value;
    }
}
```

U některých automaticky implementovaných vlastností se dá použít jen pro čtení, ale nemá smysluplný efekt. Kompilátor bude považovat všechny automaticky implementované metody getter jako jen pro čtení, bez ohledu na to, jestli je přítomné klíčové slovo `readonly`.

```csharp
// Allowed
public readonly int Prop4 { get; }
public int Prop5 { readonly get; }
public int Prop6 { readonly get; set; }

// Not allowed
public readonly int Prop7 { get; set; }
public int Prop8 { get; readonly set; }
```

ReadOnly lze použít pro ručně implementované události, ale ne pro události podobné poli. Vlastnost ReadOnly nelze použít na jednotlivé přístupové objekty událostí (přidat/odebrat).

```csharp
// Allowed
public readonly event Action<EventArgs> Event1
{
    add { }
    remove { }
}

// Not allowed
public readonly event Action<EventArgs> Event2;
public event Action<EventArgs> Event3
{
    readonly add { }
    readonly remove { }
}
public static readonly event Event4
{
    add { }
    remove { }
}
```

Některé další příklady syntaxe:

* Expression těle members: `public readonly float ExpressionBodiedMember => (x * x) + (y * y);`
* Obecná omezení: `public static readonly void GenericMethod<T>(T value) where T : struct { }`

Kompilátor vygeneroval člen instance, jako obvykle, a by také vygeneroval rozpoznaný atribut Compiler označující, že člen instance nemění stav. To efektivně způsobí, že se skrytý `this` parametr `in T` namísto `ref T`.

To umožní uživateli bezpečně zavolat metodu dané instance, aniž by museli vytvořit kopii.

Tato omezení by zahrnovala:

* Modifikátor `readonly` nelze použít pro statické metody, konstruktory nebo destruktory.
* Modifikátor `readonly` nelze použít na delegáty.
* Modifikátor `readonly` nelze použít pro členy třídy nebo rozhraní.

## <a name="drawbacks"></a>Nevýhody
[drawbacks]: #drawbacks

Existují stejné nevýhody jako v současnosti s `readonly struct`mi metodami. Určitý kód stále může způsobit skryté kopie.

## <a name="notes"></a>Poznámky:
[notes]: #notes

Může být také možné použít atribut nebo jiné klíčové slovo.

Tento návrh se trochu týká (ale je více podmnožinou) `functional purity` a/nebo `constant expressions`, z nichž obě mají nějaké existující návrhy.
