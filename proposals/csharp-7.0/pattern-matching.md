---
ms.openlocfilehash: 3df21c5816be90387a6cd9242e99ba11f43dfd1c
ms.sourcegitcommit: f61a06970fa0562d2e40363fae3948eb168624ca
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/14/2020
ms.locfileid: "79485166"
---
# <a name="pattern-matching-for-c-7"></a>Porovnávání vzorů C# pro 7

Rozšíření pro porovnávání vzorů pro C# zajištění mnoha výhod algebraických datových typů a porovnávání vzorů z funkčních jazyků, ale způsobem, který hladce integruje s chováním základního jazyka. Základní funkce jsou: [typy záznamů](https://github.com/dotnet/csharplang/blob/master/proposals/records.md), které jsou typy, jejichž sémantický význam je popsán tvarem dat; a porovnávání vzorů, což je nový formulář s výrazem, který umožňuje extrémně stručné rozložení s těmito datovými typy. Prvky tohoto přístupu jsou nechte inspirovat souvisejícími funkcemi v programovacích jazycích [F#](https://www.microsoft.com/en-us/research/wp-content/uploads/2016/02/p29-syme.pdf "Rozšiřitelné porovnávání vzorů přes zjednodušený jazyk") a [Scala](https://infoscience.epfl.ch/record/98468/files/MatchingObjectsWithPatterns-TR.pdf "Vyhovující objekty se vzorci").

## <a name="is-expression"></a>Výraz is

Operátor `is` je rozšířen o testování výrazu na základě *vzoru*.

```antlr
relational_expression
    : relational_expression 'is' pattern
    ;
```

Tato forma *relational_expression* je kromě existujících formulářů ve C# specifikaci. Jedná se o chybu při kompilaci, pokud *relational_expression* vlevo od `is` tokenu neurčuje hodnotu nebo nemá typ.

Každý *identifikátor* vzoru zavádí novou místní proměnnou, která je *omezena* na základě `true`ho operátoru `is` (tj. *jednoznačně přiřazený při hodnotě true*).

> Poznámka: existují technicky nejednoznačnost mezi *typem* v `is-expression` a *constant_pattern*, z nichž každá může být platným analýzou kvalifikovaného identifikátoru. Pokusíme se vytvořit propojení jako typ pro kompatibilitu s předchozími verzemi jazyka; jenom v případě, že se to nepovede, můžeme ho vyřešit jako v ostatních kontextech na první nalezenou věc (která musí být buď konstanta, nebo typ). Tato nejednoznačnost je k dispozici pouze na pravé straně výrazu `is`.

## <a name="patterns"></a>Vzory

V operátoru `is` a v *switch_statement* se používají vzory, které vyjadřují tvar dat, ke kterým se budou porovnávat příchozí data. Vzorce mohou být rekurzivní, aby mohly být části dat porovnány s dílčími vzory.

```antlr
pattern
    : declaration_pattern
    | constant_pattern
    | var_pattern
    ;

declaration_pattern
    : type simple_designation
    ;

constant_pattern
    : shift_expression
    ;

var_pattern
    : 'var' simple_designation
    ;
```

> Poznámka: existují technicky nejednoznačnost mezi *typem* v `is-expression` a *constant_pattern*, z nichž každá může být platným analýzou kvalifikovaného identifikátoru. Pokusíme se vytvořit propojení jako typ pro kompatibilitu s předchozími verzemi jazyka; jenom v případě, že se to nepovede, můžeme ho vyřešit jako v ostatních kontextech na první nalezenou věc (která musí být buď konstanta, nebo typ). Tato nejednoznačnost je k dispozici pouze na pravé straně výrazu `is`.

### <a name="declaration-pattern"></a>Vzor deklarace

*Declaration_pattern* testuje, zda je výraz daný typ, a přetypování na tento typ, pokud je test úspěšný. Pokud je *simple_designation* identifikátor, zavádí místní proměnnou daného typu pojmenovaného daným identifikátorem. Tato lokální proměnná je *jednoznačně přiřazena* , pokud je výsledkem operace porovnávání se vzorem hodnota true.

```antlr
declaration_pattern
    : type simple_designation
    ;
```

Sémantika modulu runtime tohoto výrazu je, že testuje typ modulu runtime *relational_expression* operandu na levé straně proti *typu* ve vzoru. Pokud se jedná o tento typ modulu runtime (nebo nějaký podtyp), je výsledek `is operator` `true`. Deklaruje novou místní proměnnou pojmenovanou *identifikátorem* , který je přiřazena hodnota operandu na levé straně při `true`výsledku.

Určité kombinace statického typu na levé straně a daného typu se považují za nekompatibilní a výsledkem je chyba při kompilaci. Hodnota statického typu `E` je označována jako *vzor kompatibilní* s typem `T`, pokud existuje převod identity, implicitní převod odkazu, převod zabalení, explicitní převod odkazu nebo převod rozbalení z `E` na `T`. Jedná se o chybu při kompilaci, pokud výraz typu `E` není kompatibilní se vzorem typu v rámci vzoru typu, se kterým se shoduje.

> Poznámka: [v C# 7,1 jsme toto rozšířili](../csharp-7.1/generics-pattern-match.md) tak, aby povolovala operace porovnávání se vzorem, pokud je typ vstupu nebo typ `T` otevřený. Tento odstavec se nahrazuje tímto:
> 
> Určité kombinace statického typu na levé straně a daného typu se považují za nekompatibilní a výsledkem je chyba při kompilaci. Hodnota statického typu `E` je označována jako *vzor kompatibilní* s typem `T`, pokud existuje převod identity, implicitní převod odkazu, převod zabalení, explicitní převod odkazu nebo převod rozbalení z `E` na `T`**nebo pokud je buď `E` nebo `T` otevřeným typem**. Jedná se o chybu při kompilaci, pokud výraz typu `E` není kompatibilní se vzorem typu v rámci vzoru typu, se kterým se shoduje.

Vzor deklarace je vhodný pro provádění testů typu odkazu v době běhu a nahrazuje idiom

```csharp
var v = expr as Type;
if (v != null) { // code using v }
```

S mírným výstižným

```csharp
if (expr is Type v) { // code using v }
```

Jedná se o chybu, pokud je *typ* hodnota s možnou hodnotou null.

Vzor deklarace lze použít k otestování hodnot typů s možnou hodnotou null: hodnota typu `Nullable<T>` (nebo zabalená `T`) odpovídá vzoru typu `T2 id`, pokud hodnota není null a typ `T2` je `T`nebo některý základní typ nebo rozhraní `T`. Například v fragmentu kódu

```csharp
int? x = 3;
if (x is int v) { // code using v }
```

Podmínka příkazu `if` je `true` za běhu a proměnná `v` obsahuje hodnotu `3` typu `int` uvnitř bloku.

### <a name="constant-pattern"></a>Konstantní vzorek

```antlr
constant_pattern
    : shift_expression
    ;
```

Konstantní vzor testuje hodnotu výrazu s konstantní hodnotou. Konstanta může být libovolný konstantní výraz, jako je například literál, název deklarované `const` proměnné nebo konstanta výčtu nebo výraz `typeof`.

Pokud jsou jak *e* , tak *c* celočíselné typy, vzor se považuje za odpovídající, pokud je výsledek výrazu `e == c` `true`.

V opačném případě se vzor považuje za shodný, pokud `object.Equals(e, c)` vrátí `true`. V tomto případě se jedná o chybu při kompilaci, pokud statický typ *e* není *vzor kompatibilní* s typem konstanty.

### <a name="var-pattern"></a>variabilní vzorek

```antlr
var_pattern
    : 'var' simple_designation
    ;
```

Výraz *e* se shoduje s *var_pattern* vždy. Jinými slovy, porovnávání se *vzorkem var* vždy proběhne úspěšně. Pokud je *simple_designation* identifikátor, pak za běhu je hodnota *e* vázána na nově zavedenou místní proměnnou. Typ lokální proměnné je statický typ *e*.

Pokud se název `var` váže k typu, jedná se o chybu.

## <a name="switch-statement"></a>Příkaz switch

Příkaz `switch` je rozšířen o výběr pro spuštění prvního bloku, který má přidružený vzor, který odpovídá *výrazu přepínače*.

```antlr
switch_label
    : 'case' complex_pattern case_guard? ':'
    | 'case' constant_expression case_guard? ':'
    | 'default' ':'
    ;

case_guard
    : 'when' expression
    ;
```

Pořadí, ve kterém se vzorce shodují, není definováno. Kompilátor je povolený pro porovnávání vzorů mimo pořadí a opakované použití výsledků už odpovídajících vzorů k výpočtu výsledku porovnávání jiných vzorů.

Pokud je přítomen *případ-Guard* , je jeho výraz typu `bool`. Je vyhodnocen jako další podmínka, která musí být splněna, aby byl případ považován za splněný.

Jedná se o chybu, pokud *switch_label* nemůže mít za běhu žádný účinek, protože jeho vzor je subsumed v předchozích případech. [TODO: máme přesnější informace o technikách, které kompilátor potřebuje k dosažení tohoto rozhodnutí.]

Proměnná vzoru deklarovaná v *switch_label* je jednoznačně přiřazena v bloku Case, pokud a pouze v případě, že tento blok případu obsahuje přesně jeden *switch_label*.

[TODO: je potřeba určit, kdy má být *blok přepínače* dosažitelný.]

### <a name="scope-of-pattern-variables"></a>Rozsah proměnných vzoru

Rozsah proměnné deklarované ve vzoru je následující:

- Pokud je vzorek jmenovka Case, pak obor proměnné je *blok případu*.

V opačném případě je proměnná deklarována ve výrazu *is_pattern* a její obor je založen na konstruktoru, který je okamžitě ohraničený výrazem, který obsahuje výraz *is_pattern* následujícím způsobem:

- Pokud je výraz ve výrazu lambda s výrazem těle, jeho rozsah je tělo lambda.
- Pokud je výraz v metodě nebo vlastnosti těle výrazu, jeho obor je tělo metody nebo vlastnosti.
- Pokud je výraz v klauzuli `when` klauzule `catch`, je jeho oborem klauzule `catch`.
- Pokud je výraz ve *iteration_statement*, jeho obor je pouze tento příkaz.
- Jinak, pokud je výraz v nějakém jiném formuláři příkazu, jeho rozsah je obor obsahující příkaz.

Pro účely určení rozsahu se *embedded_statement* považuje za jeho vlastní obor. Například gramatika pro *if_statement* je

``` antlr
if_statement
    : 'if' '(' boolean_expression ')' embedded_statement
    | 'if' '(' boolean_expression ')' embedded_statement 'else' embedded_statement
    ;
```

Takže pokud řízený příkaz *if_statement* deklaruje proměnnou vzoru, je její obor omezen na tento *embedded_statement*:

```csharp
if (x) M(y is var z);
```

V tomto případě je rozsah `z` vloženým příkazem `M(y is var z);`.

Jiné případy jsou chyby z jiných důvodů (například ve výchozí hodnotě parametru nebo atributu, z nichž obě jsou chyby, protože tyto kontexty vyžadují konstantní výraz).

> [V C# 7,3 jsme přidali následující kontexty](../csharp-7.3/expression-variables-in-initializers.md) , ve kterých může být deklarována proměnná vzoru:
> - Pokud je výraz v *inicializátoru konstruktoru*, jeho obor je *inicializátor konstruktoru* a tělo konstruktoru.
> - Pokud je výraz v inicializátoru pole, jeho obor je *equals_value_clause* , ve kterém se zobrazí.
> - Pokud je výraz v klauzuli dotazu, která je určena k překladu do těla výrazu lambda, jeho obor je pouze tento výraz.

## <a name="changes-to-syntactic-disambiguation"></a>Změny syntaktického nejednoznačnosti

Existují situace zahrnující obecné typy, ve kterých C# je gramatika nejednoznačná, a specifikace jazyka říká, jak tyto nejednoznačnosti vyřešit:

> #### <a name="7652-grammar-ambiguities"></a>7.6.5.2 gramatické nejednoznačnosti
> Výroby pro *jednoduché názvy* (§ 7.6.3) a *Member-Access* (§ 7.6.5) mohou vést k nejednoznačnosti v gramatice pro výrazy. Například příkaz:
> ```csharp
> F(G<A,B>(7));
> ```
> může být interpretován jako volání `F` se dvěma argumenty, `G < A` a `B > (7)`. Alternativně může být interpretován jako volání `F` s jedním argumentem, což je volání obecné metody `G` se dvěma argumenty typu a jedním regulárním argumentem.

> V případě, že je možné sekvenci tokenů analyzovat (v kontextu) jako *jednoduchý název* (§ 7.6.3), *přístup k členu* (§ 7.6.5) nebo *ukazatel-Member-Access* (§ 18.5.2) končící na *typ-argument-list* (§ 4.4.1), je přezkoumán token hned po ukončení `>` tokenu. Pokud je jeden z
> ```none
> (  )  ]  }  :  ;  ,  .  ?  ==  !=  |  ^
> ```
> pak je *typ-argument-seznam* uložen jako součást *jednoduchého názvu*, přístupu *člena* nebo *ukazatele-člena* a jakékoli další možné analýzy sekvence tokenů je zahozena. V opačném případě se *seznam argumentů typu* nepovažuje za součást *jednoduchého názvu*, *přístupu k členu* nebo > ukazatel na *člena*, a to i v případě, že není možné analyzovat sekvenci tokenů. Všimněte si, že tato pravidla nejsou aplikována při analýze *typu seznamu-argumentu* v *oboru názvů nebo názvu typu* (§ 3,8). Příkaz
> ```csharp
> F(G<A,B>(7));
> ```
> bude podle tohoto pravidla interpretována jako volání `F` s jedním argumentem, což je volání obecné metody `G` se dvěma argumenty typu a jedním regulárním argumentem. Příkazy
> ```csharp
> F(G < A, B > 7);
> F(G < A, B >> 7);
> ```
> každý bude interpretován jako volání `F` se dvěma argumenty. Příkaz
> ```csharp
> x = F < A > +y;
> ```
> bude interpretován jako operátor menší než, větší než a unární operátor plus, jako by byl například zápis příkazu `x = (F < A) > (+y)`, namísto jako *jednoduchého názvu* se *seznamem argumentů typu* a následovaným binárním operátorem Plus. V příkazu
> ```csharp
> x = y is C<T> + z;
> ```
> tokeny `C<T>` jsou interpretovány jako *název oboru názvů nebo názvu* pomocí *seznamu typu argument-seznam*.

V C# 7 se zavádí několik změn, které tyto pravidla pro odstraňování nejasnosti neumožňují zvládnout složitost jazyka.

### <a name="out-variable-declarations"></a>Deklarace proměnné out

Nyní je možné deklarovat proměnnou v argumentu out:

```csharp
M(out Type name);
```

Typ ale může být obecný: 

```csharp
M(out A<B> name);
```

Vzhledem k tomu, že gramatika jazyka argumentu používá *výraz*, podléhá tomuto kontextu pravidlo pro nejednoznačnosti. V tomto případě uzavírací `>` následuje *identifikátor*, což není jedna z tokenů, která umožňuje, aby byla zpracována jako *seznam argumentů typu*. Proto navrhnem **Přidání *identifikátoru* do sady tokenů, které aktivují nejednoznačnost na *seznam typů argumentů*.**

### <a name="tuples-and-deconstruction-declarations"></a>Řazené kolekce členů a prohlášení o dekonstrukci

Literál řazené kolekce členů funguje v přesně stejném problému. Vzít v úvahu výraz řazené kolekce členů

```csharp
(A < B, C > D, E < F, G > H)
```

V rámci starých C# 6 pravidel pro analýzu seznamu argumentů by se tato analýza mohla analyzovat jako řazená kolekce členů se čtyřmi prvky, počínaje `A < B` jako první. Pokud se však zobrazí na levé straně dekonstrukce, chceme, aby došlo k nejasnostem aktivovaném tokenem *identifikátoru* , jak je popsáno výše:

```csharp
(A<B,C> D, E<F,G> H) = e;
```

Toto je deklarace dekonstrukce, která deklaruje dvě proměnné, první z nich je typu `A<B,C>` a pojmenovaný `D`. Jinými slovy literál řazené kolekce členů obsahuje dva výrazy, z nichž každý je výraz deklarace.

Pro jednoduchost specifikace a kompilátoru navrhuji, aby se tento literál řazené kolekce členů analyzoval jako řazená kolekce členů se dvěma prvky, ať už se zobrazuje, a to bez ohledu na to, jestli se zobrazuje na levé straně přiřazení. To by představovalo přirozený výsledek nejasnosti popsané v předchozí části.

### <a name="pattern-matching"></a>Porovnávání vzorů

Porovnávání vzorů zavádí nový kontext, ve kterém vznikne nejednoznačnost typu výrazu. Dřív byla na pravé straně operátoru `is` typ. Nyní může být typ nebo výraz a pokud se jedná o typ, za kterým může následovat identifikátor. To může technicky změnit význam stávajícího kódu:

```csharp
var x = e is T < A > B;
```

To se dá analyzovat podle pravidel C# 6 jako

```csharp
var x = ((e is T) < A) > B;
```

ale v části pod pravidly C# 7 (s navrhovanou nejednoznačnosti) se analyzují jako

```csharp
var x = e is T<A> B;
```

který deklaruje proměnnou `B` typu `T<A>`. Naštěstí nativní a Roslyn kompilátory mají chybu, při které poskytnou chybu syntaxe v kódu C# 6. Tato konkrétní zásadní změna tedy nezáleží na tom.

Porovnávání vzorů zavádí další tokeny, které by měly vyřídit rozlišení nejednoznačnosti směrem k výběru typu. Následující příklady stávajícího kódu v C# 6 budou přerušeny bez dalších pravidel pro nejednoznačnost:

```csharp
var x = e is A<B> && f;            // &&
var x = e is A<B> || f;            // ||
var x = e is A<B> & f;             // &
var x = e is A<B>[];               // [
```

### <a name="proposed-change-to-the-disambiguation-rule"></a>Navrhovaná změna pravidla nejednoznačnosti

Navrhuji revizi specifikace a změnit seznam nejednoznačných tokenů z

>
```none
(  )  ]  }  :  ;  ,  .  ?  ==  !=  |  ^
```

na

>
```none
(  )  ]  }  :  ;  ,  .  ?  ==  !=  |  ^  &&  ||  &  [
```

A v některých kontextech považujeme *identifikátor* za nejednoznačnost tokenu. Tyto kontexty jsou místo, kde se nejednoznačná sekvence tokenů nachází bezprostředně před jedním z klíčových slov `is`, `case`nebo `out`, nebo při analýze prvního prvku literálu řazené kolekce členů (v takovém případě tokeny předcházejí `(` nebo `:` a identifikátor je následován `,`) nebo následným prvkem literálu řazené kolekce členů.

### <a name="modified-disambiguation-rule"></a>Změněné pravidlo pro nejednoznačnosti

Změněné pravidlo pro nejednoznačnosti by mohlo vypadat nějak takto.

> Pokud je možné sekvenci tokenů analyzovat (v kontextu) jako *jednoduchý název* (§ 7.6.3), *přístup pro member* (§ 7.6.5) nebo *ukazatel-Member-Access* (§ 18.5.2) končící na *seznam argumentů typu* (§ 4.4.1), token hned za ukončovacím `>` token se vyšetří, pokud je
> - Jedna z `(  )  ]  }  :  ;  ,  .  ?  ==  !=  |  ^  &&  ||  &  [`; ani
> - Jeden z relačních operátorů `<  >  <=  >=  is as`; ani
> - Kontextové klíčové slovo dotazu, které se zobrazuje ve výrazu dotazu; ani
> - V některých kontextech považujeme *identifikátor* za nejednoznačný token. Tyto kontexty jsou místo, kde se nejednoznačná sekvence tokenů nachází bezprostředně před jedním z klíčových slov `is`, `case` nebo `out`, nebo při analýze prvního prvku literálu řazené kolekce členů (v takovém případě tokeny předcházejí `(` nebo `:` a identifikátor je následován `,`) nebo následným prvkem literálu řazené kolekce členů.
> 
> Pokud je tento token mezi seznamem nebo identifikátorem v takovém kontextu, pak je *seznam typu argument-seznam* uložen jako součást *jednoduchého názvu*, *přístupu člena* nebo *ukazatele-člena* a jakékoli další možné analýzy sekvence tokenů je zahozena.  V opačném případě *typ argument-seznam* není považován za součást *jednoduchého názvu*, *přístupu k členu* nebo *ukazateli na člena*, a to i v případě, že neexistuje jiné možné analýzy sekvence tokenů. Všimněte si, že tato pravidla nejsou aplikována při analýze *typu seznamu-argumentu* v *oboru názvů nebo názvu typu* (§ 3,8).

### <a name="breaking-changes-due-to-this-proposal"></a>Zásadní změny způsobené tímto návrhem

Z důvodu tohoto navrhovaného pravidla pro zrušení nejednoznačnosti nejsou známy žádné průlomové změny.

### <a name="interesting-examples"></a>Zajímavé příklady

Tady je několik zajímavých výsledků těchto pravidel pro nejednoznačnost:

Výraz `(A < B, C > D)` je řazená kolekce členů se dvěma prvky, každé porovnání.

Výraz `(A<B,C> D, E)` je řazená kolekce členů se dvěma prvky, první z nich je výraz deklarace.

`M(A < B, C > D, E)` volání má tři argumenty.

`M(out A<B,C> D, E)` vyvolání má dva argumenty, přičemž první z nich je deklarace `out`.

Výraz `e is A<B> C` používá výraz deklarace.

Popisek případu `case A<B> C:` používá výraz deklarace.

## <a name="some-examples-of-pattern-matching"></a>Příklady porovnávání vzorů

### <a name="is-as"></a>Je as

Idiom můžeme nahradit

```csharp
var v = expr as Type;   
if (v != null) {
    // code using v
}
```

S mírným výstižným a přímým přístupem

```csharp
if (expr is Type v) {
    // code using v
}
```

### <a name="testing-nullable"></a>Testování s možnou hodnotou null

Idiom můžeme nahradit

```csharp
Type? v = x?.y?.z;
if (v.HasValue) {
    var value = v.GetValueOrDefault();
    // code using value
}
```

S mírným výstižným a přímým přístupem

```csharp
if (x?.y?.z is Type value) {
    // code using value
}
```

### <a name="arithmetic-simplification"></a>Aritmetické zjednodušení

Předpokládejme, že definujeme sadu rekurzivních typů, které reprezentují výrazy (na samostatný návrh):

```csharp
abstract class Expr;
class X() : Expr;
class Const(double Value) : Expr;
class Add(Expr Left, Expr Right) : Expr;
class Mult(Expr Left, Expr Right) : Expr;
class Neg(Expr Value) : Expr;
```

Nyní můžeme definovat funkci pro výpočet (nesnížený) odvození výrazu:

```csharp
Expr Deriv(Expr e)
{
  switch (e) {
    case X(): return Const(1);
    case Const(*): return Const(0);
    case Add(var Left, var Right):
      return Add(Deriv(Left), Deriv(Right));
    case Mult(var Left, var Right):
      return Add(Mult(Deriv(Left), Right), Mult(Left, Deriv(Right)));
    case Neg(var Value):
      return Neg(Deriv(Value));
  }
}
```

Zjednodušený výraz znázorňuje poziční vzory:

```csharp
Expr Simplify(Expr e)
{
  switch (e) {
    case Mult(Const(0), *): return Const(0);
    case Mult(*, Const(0)): return Const(0);
    case Mult(Const(1), var x): return Simplify(x);
    case Mult(var x, Const(1)): return Simplify(x);
    case Mult(Const(var l), Const(var r)): return Const(l*r);
    case Add(Const(0), var x): return Simplify(x);
    case Add(var x, Const(0)): return Simplify(x);
    case Add(Const(var l), Const(var r)): return Const(l+r);
    case Neg(Const(var k)): return Const(-k);
    default: return e;
  }
}
```
