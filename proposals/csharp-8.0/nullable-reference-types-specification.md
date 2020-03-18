---
ms.openlocfilehash: aecbf4298053debdb479ad3aaf6e3db468918455
ms.sourcegitcommit: 02b535d712cc9e022290e14d9e0324508b28f231
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/26/2020
ms.locfileid: "79485208"
---
# <a name="nullable-reference-types-specification"></a>Specifikace typů odkazů s možnou hodnotou null

Jedná se o probíhající práci – několik částí chybí nebo jsou neúplné. ***

## <a name="syntax"></a>Syntaxe

### <a name="nullable-reference-types"></a>Odkazové typy s možnou hodnotou null

Typy odkazů s možnou hodnotou null mají stejnou syntaxi `T?` jako krátká forma typu hodnot s možnou hodnotou null, ale nemají odpovídající dlouhý tvar.

Pro účely specifikace je aktuální `nullable_type`ovaná výroba přejmenována na `nullable_value_type`a přidána `nullable_reference_type` produkce:

```antlr
reference_type
    : ...
    | nullable_reference_type
    ;
    
nullable_reference_type
    : non_nullable_reference_type '?'
    ;
    
non_nullable_reference_type
    : type
    ;
```

`non_nullable_reference_type` v `nullable_reference_type` musí být odkazový typ, který nemůže mít hodnotu null (třída, rozhraní, delegát nebo pole), nebo parametr typu, který je omezený na odkaz typu, který neumožňuje hodnotu null (prostřednictvím omezení `class` nebo jiné třídy než `object`).

Typy odkazů s možnou hodnotou null se nemůžou vyskytovat v následujících umístěních:

- jako základní třída nebo rozhraní
- jako příjemce `member_access`
- jako `type` v `object_creation_expression`
- jako `delegate_type` v `delegate_creation_expression`
- jako `type` v `is_expression`, `catch_clause` nebo `type_pattern`
- jako `interface` v plně kvalifikovaném názvu člena rozhraní

U `nullable_reference_type`, kde je kontext anotace s možnou hodnotou null zakázán, se zobrazí upozornění.

### <a name="nullable-class-constraint"></a>Omezení třídy s možnou hodnotou null

Omezení `class` má `class?`protějšek s možnou hodnotou null:

```antlr
primary_constraint
    : ...
    | 'class' '?'
    ;
```

### <a name="the-null-forgiving-operator"></a>Operátor null-striktní

Operátor po opravě `!` se nazývá operátor null-striktní.

```antlr
primary_expression
    : ...
    | null_forgiving_expression
    ;
    
null_forgiving_expression
    : primary_expression '!'
    ;
```

`primary_expression` musí být typu odkazu.  

Operátor `!` přípon nemá žádný běhový efekt – vyhodnotí výsledek podkladového výrazu. Jeho jedinou rolí je změnit stav null výrazu a omezit upozornění na základě jeho použití.

### <a name="nullable-implicitly-typed-local-variables"></a>implicitně typované lokální proměnné s možnou hodnotou null

`var` odvodí typ s poznámkami pro typy odkazů.
Například v `var s = "";` `var` je odvozen jako `string?`.

### <a name="nullable-compiler-directives"></a>Nullable – direktivy kompilátoru

direktivy `#nullable` určují kontexty anotace a upozornění s možnou hodnotou null.

```antlr
pp_directive
    : ...
    | pp_nullable
    ;
    
pp_nullable
    : whitespace? '#' whitespace? 'nullable' whitespace nullable_action pp_new_line
    ;
    
nullable_action
    : 'disable'
    | 'enable'
    | 'restore'
    ;
```

direktivy `#pragma warning` jsou rozbalené, aby bylo možné změnit kontext varování s možnou hodnotou null a povolit, aby byla jednotlivá upozornění zapnutá, i když jsou ve výchozím nastavení zakázaná:

```antlr
pragma_warning_body
    : ...
    | 'warning' whitespace nullable_action whitespace 'nullable'
    ;

warning_action
    : ...
    | 'enable'
    ;
```

Všimněte si, že nová forma `pragma_warning_body` používá `nullable_action`, nikoli `warning_action`.

## <a name="nullable-contexts"></a>Kontexty s možnou hodnotou null

Každý řádek zdrojového kódu má *kontext anotace s možnou hodnotou null* a *výstražný kontext s možnou hodnotou null*. Tyto ovládací prvky mají vliv na to, jestli jsou anotace s možnou hodnotou null, a zda jsou zadány upozornění Kontext poznámky daného řádku je buď *zakázán* , nebo *povolen*. Kontext varování daného řádku je buď *zakázán* , nebo *povolen*.

Oba kontexty lze zadat na úrovni projektu (mimo C# zdrojový kód) nebo kdekoli v rámci zdrojového souboru prostřednictvím `#nullable` direktiv před procesorem. Pokud nejsou k dispozici žádná nastavení na úrovni projektu, je výchozí nastavení pro oba kontexty *zakázáno*.

Direktiva `#nullable` řídí kontexty poznámek a upozornění v rámci zdrojového textu a má přednost před nastavením na úrovni projektu.

Direktiva nastavuje kontexty, které ovládací prvky řídí pro následné řádky kódu, dokud ji nepřepisuje jiná direktiva nebo dokud nekončí konec zdrojového souboru.

Účinek direktiv je následující:

- `#nullable disable`: nastavuje anotaci s možnou hodnotou null a kontexty upozornění na *zakázáno* .
- `#nullable enable`: nastaví pro *povolenou* anotaci a kontexty upozornění s možnou hodnotou null.
- `#nullable restore`: obnoví nastavení projektu s možnou anotací a kontexty upozornění.
- `#nullable disable annotations`: nastaví kontext anotace s možnou hodnotou null na *disabled* .
- `#nullable enable annotations`: nastaví kontext anotace s možnou hodnotou null na *povoleno* .
- `#nullable restore annotations`: obnoví kontext anotace s možnou hodnotou null na nastavení projektu.
- `#nullable disable warnings`: nastaví výstražný kontext s možnou hodnotou null na *disabled* .
- `#nullable enable warnings`: nastaví výstražný kontext s možnou hodnotou null na *povoleno* .
- `#nullable restore warnings`: obnoví kontext upozornění s možnou hodnotou null na nastavení projektu.

## <a name="nullability-of-types"></a>Hodnota null typů

Daný typ může mít jednu ze čtyř nullabilities: *oblivious*, která není *null*, *Nullable* a *Unknown*. 

*Nenullelné* a *neznámé* typy mohou způsobit upozornění, pokud je jim přiřazena potenciální `null` hodnota. *Oblivious* a typy s *možnou hodnotou null* jsou ale "*null" přiřazovatelné*a můžou mít k nim přiřazené `null` hodnoty bez upozornění. 

*Oblivious* a *nenulové* typy lze odkázat nebo přiřadit bez upozornění. Hodnoty s *možnou hodnotou null* a *neznámými* typy jsou však "*nenáročné na hodnotu null*" a mohou způsobit upozornění při přečtení nebo přiřazení bez správné kontroly hodnoty null. 

*Výchozí stav null* typu pro získávání hodnoty null je "možná null". Výchozí stav null typu nenulových hodnot, který není null, je "NOT NULL".

Typ typu a kontext anotace s možnou hodnotou null se vyskytuje v určení jeho hodnoty null:

- `S` typ hodnoty, který není null, je vždycky *nenull* .
- Typ hodnoty s možnou hodnotou null `S?` je vždycky *null* .
- Neoznačený odkazový typ `C` v kontextu *zakázané* poznámky je *oblivious*
- Typ odkazu, který není označený `C` v kontextu *povolené* poznámky, není *null* .
- Typ odkazu s možnou hodnotou null `C?` může *mít hodnotu null* (může se ale v kontextu *zakázané* poznámky vracet).

Parametry typu navíc berou v úvahu svá omezení:

- Parametr typu `T`, kde všechna omezení (pokud existují) mají buď typy s*možnou hodnotou* null (Nullable a *Unknown*), nebo je omezení `class?` *neznámé* .
- Parametr typu `T`, kde nejméně jedno omezení je buď *oblivious* , nebo není *null* , nebo jedno z `struct` nebo omezení `class` je
    - *oblivious* v kontextu *zakázané* poznámky
    - v *povoleném* kontextu anotace není možné *mít hodnotu null* .
- Parametr typu s možnou hodnotou null `T?`, kde nejméně jedno omezení `T`je *oblivious* nebo není *null* nebo jedno z `struct` nebo `class` omezení je
    - hodnota *null* v kontextu *zakázané* poznámky (ale je výsledkem upozornění)
    - *Nullable* v *povoleném* kontextu anotace

Pro parametr typu `T`je `T?` povoleno pouze v případě, že je `T` znám jako typ hodnoty nebo se jedná o typ odkazu.

### <a name="oblivious-vs-nonnullable"></a>Oblivious vs – nemožnost null

`type` se považuje za výskyt v daném kontextu poznámky, pokud je poslední token typu v rámci daného kontextu.

Zda daný typ odkazu `C` ve zdrojovém kódu je interpretován jako oblivious nebo není null, závisí na kontextu poznámky tohoto zdrojového kódu. Po navázání se však považuje za součást tohoto typu a "při jejich" přenosu ", např. při nahrazování argumentů obecného typu. Je to tak, jak je Poznámka jako `?` typu, ale neviditelná.

## <a name="constraints"></a>Omezení

Typy odkazů s možnou hodnotou null lze použít jako obecná omezení. Kromě toho `object` nyní platí jako explicitní omezení. Neexistence omezení je nyní ekvivalentní omezení `object?` (namísto `object`), ale (na rozdíl od `object` před) `object?` není zakázáno jako explicitní omezení.

`class?` je nové omezení označující "možný typ odkazu s možnou hodnotou null", zatímco `class` označuje "typ odkazu, který neumožňuje hodnotu null".

Hodnota null argumentu typu nebo omezení nemá vliv na to, jestli typ splňuje omezení, s výjimkou případů, kdy to již dnes nevyhovuje omezení `struct`. Pokud však argument typu nesplňuje požadavky na hodnotu NULL omezení, může být zadáno upozornění.

## <a name="null-state-and-null-tracking"></a>Nulový stav a sledování hodnoty null

Každý výraz v daném zdrojovém umístění má *stav null*, který označuje, zda se má potenciálně vyhodnotit jako hodnota null. Stav null je buď "NOT NULL" nebo "pravděpodobně null". Stav null se používá k určení, zda by mělo být zadáno upozornění na hodnotu null – nezabezpečené převody a odkazy.

### <a name="null-tracking-for-variables"></a>Sledování hodnoty null pro proměnné

Pro určité výrazy zaznamenání proměnných nebo vlastností je stav null sledován mezi výskyty na základě přiřazení k těmto hodnotám, v nich byly provedeny testy a tok řízení mezi nimi. To se podobá tomu, jak je u proměnných sledováno jednoznačné přiřazení. Sledované výrazy jsou ty z následujícího formuláře:

```antlr
tracked_expression
    : simple_name
    | this
    | base
    | tracked_expression '.' identifier
    ;
```

Kde identifikátory označují pole nebo vlastnosti.

***Popsat přechody stavu s hodnotou null podobně jako u jednoznačného přiřazení***

### <a name="null-state-for-expressions"></a>Nulový stav pro výrazy

Nulový stav výrazu je odvozen z jeho formuláře a typu a z nulového stavu proměnných, které jsou v něm zapojeny.

### <a name="literals"></a>Literály

Nulová stav literálu `null` je "možná null". Stav null literálu `default`, který je převáděn na typ, který je známý jako nehodnotný typ hodnoty, který není null, je "možná null". Nulový stav jakéhokoli jiného literálu je "NOT NULL".

### <a name="simple-names"></a>Jednoduché názvy

Není-li `simple_name` klasifikován jako hodnota, má stav null hodnotu not null. V opačném případě se jedná o sledovaný výraz a jeho stav null je jeho sledovaný stav null v tomto zdrojovém umístění.

### <a name="member-access"></a>Přístup ke členům

Není-li `member_access` klasifikován jako hodnota, má stav null hodnotu not null. Jinak, pokud se jedná o sledovaný výraz, jeho stav null je jeho sledovaný stav null v tomto zdrojovém umístění. V opačném případě je stav null výchozím stavem null.

### <a name="invocation-expressions"></a>Výrazy vyvolání

Pokud `invocation_expression` vyvolá člena deklarovaného s jedním nebo více atributy pro speciální chování null, je stav null určen těmito atributy. V opačném případě je nulový stav výrazu výchozím prázdným stavem typu.

### <a name="element-access"></a>Přístup k prvkům

Pokud `element_access` vyvolá indexer deklarovaný s jedním nebo více atributy pro speciální chování null, je stav null určen těmito atributy. V opačném případě je nulový stav výrazu výchozím prázdným stavem typu.

### <a name="base-access"></a>Základní přístup

Pokud `B` označuje základní typ nadřazeného typu, `base.I` má stejný nulový stav jako `((B)this).I` a `base[E]` má stejný stav null jako `((B)this)[E]`.

### <a name="default-expressions"></a>Výchozí výrazy

`default(T)` má nulový stav "non-null", pokud je `T` znám jako neprázdný typ hodnoty. V opačném případě má nulový stav "možná null".

### <a name="null-conditional-expressions"></a>Podmíněné výrazy s hodnotou null

`null_conditional_expression` má nulový stav "možná null".

### <a name="cast-expressions"></a>Výrazy cast

Pokud výraz cast `(T)E` vyvolá uživatelsky definovaný převod, pak je nulový stav výrazu výchozím stavem null pro daný typ. V opačném případě, pokud `T` má hodnotu null (*Nullable* nebo *Unknown*), je stav null "možná null". V opačném případě je stav null stejný jako stav null `E`.

### <a name="await-expressions"></a>Výrazy await

Stav null `await E` je výchozím stavem null typu.

### <a name="the-as-operator"></a>Operátor `as`

Výraz `as` má nulový stav "možná null".

### <a name="the-null-coalescing-operator"></a>Operátor slučování null

`E1 ?? E2` má stejný stav null jako `E2`

### <a name="the-conditional-operator"></a>Podmíněný operátor

Nulová stav `E1 ? E2 : E3` je "NOT NULL", pokud je nulový stav obou `E2` a `E3` "NOT NULL". V opačném případě je to "možná hodnota null".

### <a name="query-expressions"></a>Výrazy dotazů

Stav null výrazu dotazu je výchozím stavem null typu.

### <a name="assignment-operators"></a>Operátory přiřazení

`E1 = E2` a `E1 op= E2` mají stejný stav null jako `E2` po použití jakýchkoli implicitních převodů.

### <a name="unary-and-binary-operators"></a>Unární a binární operátory

Pokud unární nebo binární operátor vyvolá uživatelsky definovaný operátor deklarovaný s jedním nebo více atributy pro speciální chování null, je stav null určen těmito atributy. V opačném případě je nulový stav výrazu výchozím prázdným stavem typu.

***Pro binární `+` přes řetězce a delegáty něco nějakého?***

### <a name="expressions-that-propagate-null-state"></a>Výrazy, které rozšiřují stav null

`(E)`, `checked(E)` a `unchecked(E)` mají stejný stav null jako `E`.

### <a name="expressions-that-are-never-null"></a>Výrazy, které nemají hodnotu null

Stav null následujících formulářů výrazů je vždy "NOT NULL":

- přístup k `this`
- Interpolované řetězce
- výrazy `new` (objekt, delegát, anonymní objekty a vytváření polí)
- výrazy `typeof`
- výrazy `nameof`
- anonymní funkce (anonymní metody a lambda výrazy)
- výrazy s hodnotou null – striktní
- výrazy `is`

## <a name="type-inference"></a>Odvození typu

### <a name="type-inference-for-var"></a>Odvození typu pro `var`

Odvozený typ pro lokální proměnné deklarované pomocí `var` je informován stavem null inicializačního výrazu.

```csharp
var x = E;
```

Pokud typ `E` je typ odkazu s možnou hodnotou null `C?` a stav null `E` není null, pak je `C`typ odvozený pro `x`. V opačném případě odvozený typ je typ `E`.

Hodnota null typu odvozená pro `x` je určena výše, jak je popsáno výše v závislosti na kontextu poznámky `var`, stejně jako v případě, že byl typ zadán explicitně na této pozici.

### <a name="type-inference-for-var"></a>Odvození typu pro `var?`

Odvozený typ pro lokální proměnné deklarované pomocí `var?` je nezávislý na nulovém stavu inicializačního výrazu.

```csharp
var? x = E;
```

Pokud typ `T` `E` je typ hodnoty s možnou hodnotou null nebo typ odkazu s možnou hodnotou null, je `T`odvozený typ `x`. V opačném případě, pokud je `T` typ hodnoty, která není null `S` je odvozený typ `S?`. V opačném případě, pokud je `T` odkazový typ, který není null `C` je odvozený typ `C?`. V opačném případě je deklarace neplatná.

Hodnota null typu odvozeného pro `x` je vždy *null*.

### <a name="generic-type-inference"></a>Odvození obecného typu

Odvození obecného typu je vylepšeno, aby bylo možné určit, zda odvozené typy odkazů mají mít hodnotu null nebo ne. Toto je nejlepší úsilí, které nezahrnuje a sám o sobě nepřinese upozornění, ale může vést k upozorněním s možnou hodnotou null, pokud jsou odvozeny typy vybraného přetížení aplikovány na argumenty.

Odvození typu nespoléhá na kontext poznámky pro příchozí typy. Místo toho je odvozený `type`, který získá svůj vlastní kontext poznámky z místa, kde by byl, pokud byl vyjádřen explicitně. Tato podtržítka představuje roli pro odvození typu jako pohodlí pro to, co byste mohli sami napsat.

Přesněji je kontext poznámky pro odvozený typ argumentu kontextem tokenu, který by následoval za parametrem `<...>`ho typu. měl by se jednat o jeden. například název obecné metody, která je volána. Pro výrazy dotazů, které jsou přeloženy na taková volání, je kontext pořízen z počátečního klíčového slova klauzule dotazu, ze které je volání vygenerováno.

### <a name="the-first-phase"></a>První fáze

Typy odkazů s možnou hodnotou null se zanášejí do hranic z počátečních výrazů, jak je popsáno níže. Kromě toho jsou zavedeny dva nové druhy mezí, totiž `null` a `default`. Jejich účelem je přecházet z výskytů `null` nebo `default` ve vstupních výrazech, což může způsobit, že odvozený typ bude mít hodnotu null, i když by jinak nevznikl. To funguje i *pro typy s možnou hodnotou null* , které jsou vylepšené pro vyzvednutí "hodnoty null" v procesu odvození.

Určení toho, co je možné přidat v první fázi, je vylepšeno následujícím způsobem:

Pokud má argument `Ei` odkazový typ, typ `U` použitý pro odvození závisí na stavu null `Ei` a také na deklarovaném typu:
- Pokud deklarovaný typ je typ odkazu, který není `U0`, nebo typ odkazu s možnou hodnotou null `U0?` potom
    - Pokud je nulový stav `Ei` "NOT NULL", pak `U` je `U0`
    - Pokud je stav null `Ei` "možná null", pak `U` je `U0?`
- Jinak, pokud `Ei` má deklarovaný typ, `U` je tento typ
- Jinak, pokud je `Ei` `null` pak je `U` speciálním vázaným `null`
- Jinak, pokud je `Ei` `default` pak je `U` speciálním vázaným `default`
- Jinak není provedeno odvození.

### <a name="exact-upper-bound-and-lower-bound-inferences"></a>Přesná, shora vázaná a s nižšími nároky na odvozená

V odvozených *od* typu `U` *k* typu `V`, pokud `V` je typ odkazu s možnou hodnotou null `V0?`, pak `V0` používá se místo `V` v následujících klauzulích.
- Je-li `V` jednou z neopravených proměnných typu, `U` je přidána jako přesné, horní nebo dolní mez jako před
- V opačném případě, pokud je `U` `null` nebo `default`, není provedeno odvození
- V opačném případě, pokud je `U` `U0?`typu odkazu s možnou hodnotou null, je místo `U` v následujících klauzulích použit `U0`.

Podstata je, že hodnota null, která se vztahuje přímo k jedné z nepevných proměnných, se zachová do jejich hranic. Pro odvození, která se dále přesměrují do zdrojového a cílového typu na druhé straně, je hodnota null ignorována. Může nebo nemusí odpovídat, ale pokud ne, bude upozornění vydáno později, pokud je vybráno a použito přetížení.

### <a name="fixing"></a>Opravě

Specifikace v současné době neprovádí dobrou úlohu popisující, co se stane, když je mezi sebou identita více, ale liší se. K tomu může dojít mezi `object` a `dynamic`, mezi typy řazené kolekce členů, které se liší pouze v názvech prvků, mezi typy sestavenými z nich a nyní mezi `C` a `C?` pro typy odkazů.

Kromě toho je potřeba rozšířit "hodnotu null" ze vstupních výrazů na typ výsledku. 

Pro zpracování těchto řešení přidáváme další fáze pro opravu, která je teď:

1. Shromážděte všechny typy ve všech hranicích jako kandidáty, přičemž odebrání `?` ze všech, které jsou typy odkazů s možnou hodnotou null.
2. Eliminujte kandidáty na základě požadavků přesně, dolních a horních mezí (udržení `null` a `default` hranic).
3. Eliminujte kandidáty, které nemají implicitní převod na všechny ostatní kandidáty.
4. Pokud zbývající kandidáti nemají všechny převody identity na sebe navzájem, pak se typ odvození nezdařil.
5. *Sloučit* zbývající kandidáty, jak je popsáno níže
6. Pokud výsledný kandidát je odkazový typ nebo typ hodnoty, která není null, a *všechny* přesné hranice nebo *jakékoli* dolní meze jsou typy hodnot s možnou hodnotou null, typy odkazů s možnou hodnotou null, `null` nebo `default`a `?` je přidána do výsledného kandidáta, takže je typ hodnoty s možnou hodnotou null nebo odkaz.

*Sloučení* je popsáno mezi dvěma typy kandidátů. Je tranzitivní a komutativníý, takže kandidáty se můžou sloučit do libovolného pořadí se stejným konečným výsledkem. Není definováno, pokud nejsou dva typy kandidátů vzájemně převoditelné.

Funkce *Merge* přijímá dva typy kandidátů a směr ( *+* nebo *-* ):

- *Merge*(`T`, `T`, *d*) = t
- *Merge*(`S`, `T?`, *+* ) = *Merge*(`S?`, `T`, *+* ) = *Merge*(`S`, `T`, *+* )`?`
- *Sloučení*(`S`, `T?`, *-* ) = *sloučení*(`S?`, `T`, *-* ) = *sloučení*(`S`, `T`, *-* )
- *Merge*(`C<S1,...,Sn>`, `C<T1,...,Tn>`, *+* ) = `C<`*sloučení*(`S1`, `T1`, *d1*)`,...,`*sloučení*(`Sn`, `Tn`, *DN*)`>`, *kde*
    - `di` =  *+* , pokud je parametr ' th type ' `C<...>` `i`kovariantní
    - `di` =  *-* , pokud je parametr ' th type ' `C<...>` `i`kontraindikace nebo invariantní
- *Merge*(`C<S1,...,Sn>`, `C<T1,...,Tn>`, *-* ) = `C<`*sloučení*(`S1`, `T1`, *d1*)`,...,`*sloučení*(`Sn`, `Tn`, *DN*)`>`, *kde*
    - `di` =  *-* , pokud je parametr ' th type ' `C<...>` `i`kovariantní
    - `di` =  *+* , pokud je parametr ' th type ' `C<...>` `i`kontraindikace nebo invariantní
- *Merge*(`(S1 s1,..., Sn sn)`, `(T1 t1,..., Tn tn)`, *d*) = `(`*sloučení*(`S1`, `T1`, *d*)`n1,...,`*sloučení*(`Sn`, `Tn`, *d*) `nn)`, *kde*
    - `ni` chybí, pokud se `si` a `ti` liší, nebo pokud chybí obojí.
    - `ni` je `si`, pokud `si` a `ti` stejné
- *Merge*(`object`, `dynamic`) = *Merge*(`dynamic`, `object`) = `dynamic`

## <a name="warnings"></a>Upozornění

### <a name="potential-null-assignment"></a>Potenciální null přiřazení

### <a name="potential-null-dereference"></a>Potenciální zpětný odkaz na hodnotu null

### <a name="constraint-nullability-mismatch"></a>Neshoda omezení hodnoty null

### <a name="nullable-types-in-disabled-annotation-context"></a>Typy s možnou hodnotou null v kontextu zakázané poznámky

## <a name="attributes-for-special-null-behavior"></a>Atributy pro speciální chování null


