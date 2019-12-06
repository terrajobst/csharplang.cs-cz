---
ms.openlocfilehash: 4d6d28a3127bc701867afe157aa5496377a06f69
ms.sourcegitcommit: 63d276488c9770a565fd787020783ffc1d2af9d6
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/05/2019
ms.locfileid: "74868001"
---
# <a name="conversions"></a>Převody

***Převod*** umožňuje, aby byl výraz považován za konkrétní typ. Převod může způsobit, že se výraz daného typu bude považovat za jiný typ, nebo může způsobit, že se pro získání typu výraz bez typu. Převody mohou být ***implicitní*** nebo ***explicitní***a tato funkce určuje, zda je vyžadováno explicitní přetypování. Například převod z typu `int` na typ `long` je implicitní, takže výrazy typu `int` lze implicitně považovat za typ `long`. Opačný převod, od typu `long` na typ `int`, je explicitní, takže je potřeba explicitní přetypování.

```csharp
int a = 123;
long b = a;         // implicit conversion from int to long
int c = (int) b;    // explicit conversion from long to int
```

Některé převody jsou definovány jazykem. Programy mohou také definovat vlastní převody ([uživatelem definované převody](conversions.md#user-defined-conversions)).

## <a name="implicit-conversions"></a>Implicitní převody

Následující převody jsou klasifikovány jako implicitní převody:

*  Převody identity
*  Implicitní číselné převody
*  Implicitní převody výčtu
*  Implicitně interpolované převody řetězců
*  Implicitní převody s možnou hodnotou null
*  Nulové převody literálů
*  Implicitní převody odkazů
*  Převody zabalení
*  Implicitní dynamické převody
*  Implicitní převody konstantních výrazů
*  Uživatelem definované implicitní převody
*  Anonymní převody funkcí
*  Převody skupin metod

Implicitní převody mohou nastat v různých situacích, včetně vyvolání členů funkce ([Kontrola dynamického přetěžování při kompilaci](expressions.md#compile-time-checking-of-dynamic-overload-resolution)), výrazů přetypování ([výrazů přetypování](expressions.md#cast-expressions)) a přiřazení ([operátory přiřazení](expressions.md#assignment-operators)).

Předem definované implicitní převody jsou vždy úspěšné a nikdy nezpůsobí vyvolání výjimek. I správně navržené uživatelem definované implicitní převody by se měly projevit i na těchto vlastnostech.

Pro účely převodu jsou typy `object` a `dynamic` považovány za ekvivalentní.

Nicméně dynamické převody ([implicitní dynamické převody](conversions.md#implicit-dynamic-conversions) a [explicitní dynamické převody](conversions.md#explicit-dynamic-conversions)) platí pouze pro výrazy typu `dynamic` ([dynamický typ](types.md#the-dynamic-type)).

### <a name="identity-conversion"></a>Převod identity

Převod identity se převede z libovolného typu na stejný typ. Tento převod existuje tak, aby entita, která už má požadovaný typ, mohla být převoditelné na tento typ.

*  Vzhledem k tomu, že `object` a `dynamic` jsou považovány za ekvivalentní, existuje převod identity mezi `object` a `dynamic`a mezi konstruovanými typy, které jsou stejné při nahrazování všech výskytů `dynamic` s `object`.

### <a name="implicit-numeric-conversions"></a>Implicitní číselné převody

Implicitní číselné převody jsou:

*  Z `sbyte` na `short`, `int`, `long`, `float`, `double`nebo `decimal`.
*  Z `byte` na `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `float`, `double`nebo `decimal`.
*  Z `short` na `int`, `long`, `float`, `double`nebo `decimal`.
*  Z `ushort` na `int`, `uint`, `long`, `ulong`, `float`, `double`nebo `decimal`.
*  Z `int` `long`, `float`, `double`nebo `decimal`.
*  Z `uint` na `long`, `ulong`, `float`, `double`nebo `decimal`.
*  Z `long` `float`, `double`nebo `decimal`.
*  Z `ulong` `float`, `double`nebo `decimal`.
*  Z `char` na `ushort`, `int``uint`, `long`, `ulong`, `float`, `double`nebo `decimal`.
*  Z `float` `double`.

Převody z `int`, `uint`, `long`nebo `ulong` na `float` a z `long` nebo `ulong` na `double` mohou způsobit ztrátu přesnosti, ale nikdy nezpůsobí ztrátu velikosti. Ostatní implicitní číselné převody nikdy neztratí žádné informace.

Neexistují žádné implicitní převody na typ `char`, takže hodnoty ostatních integrálních typů nejsou automaticky převedeny na typ `char`.

### <a name="implicit-enumeration-conversions"></a>Implicitní převody výčtu

Implicitní převod výčtu umožňuje *decimal_integer_literal* `0` převést na jakýkoli *enum_type* a na všechny *nullable_type* jejichž základní typ je *enum_type*. V druhém případě je převod vyhodnocen převodem na podkladovou *enum_type* a zabalením výsledku ([typy s možnou hodnotou null](types.md#nullable-types)).

### <a name="implicit-interpolated-string-conversions"></a>Implicitně interpolované převody řetězců

Implicitně interpolovaná konverze řetězce umožňuje převést *interpolated_string_expression* ([interpolované řetězce](expressions.md#interpolated-strings)) na `System.IFormattable` nebo `System.FormattableString` (což implementuje `System.IFormattable`).

Při použití tohoto převodu se hodnota řetězce neskládá z interpolované řetězce. Namísto toho je vytvořena instance `System.FormattableString`, jak je popsáno dále v [interpolovaná řetězce](expressions.md#interpolated-strings).

### <a name="implicit-nullable-conversions"></a>Implicitní převody s možnou hodnotou null

Předdefinované implicitní převody, které pracují s typy hodnot, které neumožňují hodnotu null, lze také použít s připouštějící formuláře těchto typů. Pro každé z předdefinovaných implicitních a numerických převodů, které se převádějí z typu hodnoty, která není null, `S` na `T`typu hodnot, který neumožňuje hodnotu null, existují následující implicitní převody s možnou hodnotou null:

*  Implicitní převod z `S?` na `T?`.
*  Implicitní převod z `S` na `T?`.

Vyhodnocení implicitního převodu s možnou hodnotou null na základě základní konverze z `S` na `T` pokračuje následujícím způsobem:

*  Pokud je převod s možnou hodnotou null z `S?` na `T?`:
    * Pokud má zdrojová hodnota hodnotu null (`HasValue` vlastnost je false), výsledkem je hodnota null typu `T?`.
    * V opačném případě je převod vyhodnocen jako rozbalení z `S?` na `S`a za ním následuje základní převod z `S` na `T`a za který následuje obtékání ([typy s možnou hodnotou null](types.md#nullable-types)) z `T` na `T?`.

*  Pokud je převod s možnou hodnotou null z `S` na `T?`, převod se vyhodnotí jako základní převod z `S` na `T` následovaný zalomením z `T` na `T?`.

### <a name="null-literal-conversions"></a>Nulové převody literálů

Implicitní převod existuje z `null` literálu na libovolný typ s možnou hodnotou null. Tento převod vytvoří hodnotu null ([typy s možnou hodnotou](types.md#nullable-types)null) daného typu s možnou hodnotou null.

### <a name="implicit-reference-conversions"></a>Implicitní převody odkazů

Implicitní převody odkazů:

*  Z libovolného *reference_type* na `object` a `dynamic`.
*  Z jakéhokoli *class_type* `S` na všechny *class_type* `T`je zadaný `S` odvozený od `T`.
*  Z jakéhokoli *class_type* `S` na všechny *interface_type* `T`zadané `S` implementuje `T`.
*  Z jakéhokoli *interface_type* `S` na všechny *INTERFACE_TYPE* `T`je zadaný `S` odvozený od `T`.
*  Z *array_type* `S` s typem elementu `SE` do *array_type* `T` s typem prvku `TE`, za předpokladu, že jsou splněny všechny následující podmínky:
    * `S` a `T` se liší pouze v typu prvku. Jinými slovy, `S` a `T` mají stejný počet rozměrů.
    * `SE` i `TE` jsou *reference_type*s.
    * Implicitní převod odkazu existuje z `SE` na `TE`.
*  Z jakéhokoli *array_type* `System.Array` a rozhraní, které implementuje.
*  Z jednorozměrného typu pole `S[]` `System.Collections.Generic.IList<T>` a jeho základních rozhraní za předpokladu, že existuje implicitní identita nebo převod odkazu z `S` na `T`.
*  Z jakéhokoli *delegate_type* `System.Delegate` a rozhraní, které implementuje.
*  Z nulového literálu na libovolný *reference_type*.
*  Z libovolného *reference_type* na *reference_type* `T`, pokud má implicitní identitu nebo převod referencí na *reference_type* `T0` a `T0` má převod identity na `T`.
*  Z libovolného *reference_type* na typ rozhraní nebo delegáta `T`, pokud má implicitní identitu nebo převod odkazu na typ rozhraní nebo delegáta `T0` a `T0` je variance-konvertibilní ([Převod variance](interfaces.md#variance-conversion)) na `T`.
*  Implicitní převody zahrnující parametry typu, které jsou označovány jako odkazové typy. Další podrobnosti o implicitních převodech týkajících se parametrů typu naleznete v tématu [implicitní převody zahrnující parametry typu](conversions.md#implicit-conversions-involving-type-parameters) .

Implicitní převody odkazů jsou tyto převody mezi *reference_type*s, které mohou být prověřeny tak, aby byly vždy úspěšné, a proto nevyžadují žádné kontroly za běhu.

Převody odkazů, implicitní nebo explicitní, nikdy nezmění referenční identitu převáděného objektu. Jinými slovy, zatímco převod odkazu může změnit typ odkazu, nikdy nemění typ nebo hodnotu objektu, na který je odkazováno.

### <a name="boxing-conversions"></a>Převody zabalení

Převod zabalení umožňuje *value_type* implicitně převést na typ odkazu. Převod zabalení existuje z libovolného *non_nullable_value_type* na `object` a `dynamic`, na `System.ValueType` a na všechny *INTERFACE_TYPE* implementované *non_nullable_value_type*. Kromě toho je možné *enum_type* převést na typ `System.Enum`.

Převod zabalení existuje z *nullable_type* na odkazový typ, pokud a pouze v případě, že v podkladovém *non_nullable_value_type* existuje převod zabalení na odkazový typ.

Typ hodnoty má převod zabalení na typ rozhraní `I`, pokud obsahuje převod zabalení na typ rozhraní `I0` a `I0` má převod identity na `I`.

Typ hodnoty má převod zabalení na typ rozhraní `I`, pokud má převod zabalení na typ rozhraní nebo delegáta `I0` a `I0` je převoditelné Variance ([Převod variance](interfaces.md#variance-conversion)) na `I`.

Zabalení hodnoty *non_nullable_value_type* se skládá z přidělení instance objektu a zkopírování *value_type* hodnoty do této instance. Struktura může být zabalená do typu `System.ValueType`, protože se jedná o základní třídu pro všechny struktury ([Dědičnost](structs.md#inheritance)).

Zabalení hodnoty *nullable_type* pokračuje následujícím způsobem:

*  Pokud má zdrojová hodnota hodnotu null (`HasValue` vlastnost je false), výsledek je odkaz s hodnotou null cílového typu.
*  V opačném případě je výsledkem odkaz na zabalenou `T` vytvořenou rozbalením a zabalením zdrojové hodnoty.

Převody zabalení jsou podrobněji popsány v tématu [převody zabalení](types.md#boxing-conversions).

### <a name="implicit-dynamic-conversions"></a>Implicitní dynamické převody

Implicitní dynamický převod existuje ve výrazu typu `dynamic` na libovolný typ `T`. Převod je dynamicky svázán ([dynamická vazba](expressions.md#dynamic-binding)), což znamená, že implicitní převod bude vyžádán za běhu z běhového typu výrazu do `T`. Pokud není nalezen žádný převod, je vyvolána výjimka za běhu.

Všimněte si, že tento implicitní převod zdánlivě porušuje Rady na začátku [implicitních převodů](conversions.md#implicit-conversions) , že implicitní převod by nikdy neměl způsobovat výjimku. Nejedná se však o samotný převod, ale *hledání* převodu, který způsobuje výjimku. Riziko běhových výjimek je podstatou použití dynamické vazby. Pokud dynamická vazba převodu není žádoucí, výraz může být nejprve převeden na `object`a následně na požadovaný typ.

Následující příklad ilustruje implicitní dynamické převody:

```csharp
object o  = "object"
dynamic d = "dynamic";

string s1 = o; // Fails at compile-time -- no conversion exists
string s2 = d; // Compiles and succeeds at run-time
int i     = d; // Compiles but fails at run-time -- no conversion exists
```

Přiřazení pro `s2` a `i` používají implicitní dynamické převody, kde je vazba operací pozastavena až do doby běhu. V době běhu jsou implicitní převody požadovány z běhového typu `d` -- `string`--do cílového typu. Byl nalezen převod pro `string`, ale ne pro `int`.

### <a name="implicit-constant-expression-conversions"></a>Implicitní převody konstantních výrazů

Implicitní převod konstantního výrazu povoluje následující převody:

*  *Constant_expression* ([konstantní výrazy](expressions.md#constant-expressions)) typu `int` lze převést na typ `sbyte`, `byte`, `short`, `ushort`, `uint`nebo `ulong`, pokud je hodnota *constant_expression* v rozsahu cílového typu.
*  *Constant_expression* typu `long` lze převést na typ `ulong`za předpokladu, že hodnota *constant_expression* není záporná.

### <a name="implicit-conversions-involving-type-parameters"></a>Implicitní převody zahrnující parametry typu

Pro daný parametr typu existují následující implicitní převody `T`:

*  Z `T` na jeho efektivní základní třídu `C`, od `T` k jakékoli základní třídě `C`a od `T` k jakémukoli rozhraní implementovanému `C`. V době běhu, pokud je `T` typ hodnoty, je převod proveden jako převod zabalení. V opačném případě je převod proveden jako implicitní převod odkazu nebo převod identity.
*  Z `T` na typ rozhraní `I` v efektivní sadě rozhraní `T`a ze `T` na jakékoli základní rozhraní `I`. V době běhu, pokud je `T` typ hodnoty, je převod proveden jako převod zabalení. V opačném případě je převod proveden jako implicitní převod odkazu nebo převod identity.
*  Z `T` na parametr typu `U`zadaný `T` závisí na `U` ([omezení parametrů typu](classes.md#type-parameter-constraints)). V době běhu, pokud je `U` typ hodnoty, pak `T` a `U` jsou nutně stejného typu a není proveden žádný převod. V opačném případě, pokud je `T` typ hodnoty, je převod proveden jako převod zabalení. V opačném případě je převod proveden jako implicitní převod odkazu nebo převod identity.
*  Z literálu s hodnotou null na `T`je zadaný `T` známý jako odkazový typ.
*  Z `T` na odkazový typ `I`, pokud má implicitní převod na typ odkazu `S0` a `S0` má převod identity na `S`. V době běhu je převod proveden stejným způsobem jako převod na `S0`.
*  Z `T` na typ rozhraní `I`, pokud má implicitní převod na rozhraní nebo typ delegáta `I0` a `I0` je variance-konvertibilní na `I` ([Převod variance](interfaces.md#variance-conversion)). V době běhu, pokud je `T` typ hodnoty, je převod proveden jako převod zabalení. V opačném případě je převod proveden jako implicitní převod odkazu nebo převod identity.

Pokud je známo, že je `T` odkazový typ ([omezení parametru typu](classes.md#type-parameter-constraints)), jsou všechny převody klasifikované jako implicitní převody odkazů ([implicitní převody odkazů](conversions.md#implicit-reference-conversions)). Pokud `T` není známý jako typ odkazu, převody uvedené výše jsou klasifikovány jako převody zabalení ([převody zabalení](conversions.md#boxing-conversions)).

### <a name="user-defined-implicit-conversions"></a>Uživatelem definované implicitní převody

Uživatelem definovaný implicitní převod se skládá z volitelného standardního implicitního převodu, za nímž následuje spuštění uživatelsky definované implicitního operátoru převodu následovaný jiným volitelným standardním implicitním převodem. Přesná pravidla pro vyhodnocení uživatelem definovaných implicitních převodů jsou popsány ve [zpracování uživatelem definovaných implicitních převodů](conversions.md#processing-of-user-defined-implicit-conversions).

### <a name="anonymous-function-conversions-and-method-group-conversions"></a>Anonymní převody funkcí a převody skupin metod

Anonymní funkce a skupiny metod nemají typy v a samotném, ale mohou být implicitně převedeny na typy delegátů nebo strom výrazů. Anonymní převody funkcí jsou podrobněji popsány v tématu [anonymní převody funkcí](conversions.md#anonymous-function-conversions) a převody skupin metod v tématu [převody skupin metod](conversions.md#method-group-conversions).

## <a name="explicit-conversions"></a>Explicitní převody

Následující převody jsou klasifikovány jako explicitní převody:

*  Všechny implicitní převody.
*  Explicitní číselné převody.
*  Explicitní převody výčtu.
*  Explicitní převody s možnou hodnotou null.
*  Explicitní převody odkazů
*  Explicitní převody rozhraní.
*  Převody rozbalení.
*  Explicitní dynamické převody
*  Uživatelem definované explicitní převody.

Explicitní převody mohou nastat ve výrazech přetypování ([výrazy přetypování](expressions.md#cast-expressions)).

Sada explicitních převodů zahrnuje všechny implicitní převody. To znamená, že jsou povoleny nadbytečné výrazy přetypování.

Explicitní převody, které nejsou implicitními převody, jsou převody, které nemohou být prověřeny tak, aby byly vždy úspěšné, převody, které mohou ztratit informace, a převody mezi doménami typů, které jsou dostatečně rozdílné jako explicitní. zápis.

### <a name="explicit-numeric-conversions"></a>Explicitní číselné převody

Explicitní číselné převody jsou převody z *numeric_type* na jiný *numeric_type* , pro které implicitní číselný převod ([implicitní číselné převody](conversions.md#implicit-numeric-conversions)) ještě neexistuje:

*  Z `sbyte` na `byte`, `ushort`, `uint`, `ulong`nebo `char`.
*  Z `byte` `sbyte` a `char`.
*  Z `short` na `sbyte`, `byte`, `ushort`, `uint`, `ulong`nebo `char`.
*  Z `ushort` `sbyte`, `byte`, `short`nebo `char`.
*  Z `int` na `sbyte`, `byte`, `short`, `ushort`, `uint`, `ulong`nebo `char`.
*  Z `uint` na `sbyte`, `byte`, `short`, `ushort`, `int`nebo `char`.
*  Z `long` na `sbyte`, `byte``short`, `ushort`, `int`, `uint`, `ulong`nebo `char`.
*  Z `ulong` na `sbyte`, `byte``short`, `ushort`, `int`, `uint`, `long`nebo `char`.
*  Z `char` `sbyte`, `byte`nebo `short`.
*  Z `float` na `sbyte`, `byte``short`, `ushort``int``uint`, `long`, `ulong`, `char`, `decimal`nebo.
*  Z `double` `sbyte`, `byte``short`, `ushort``int``uint`, `long`, `ulong`, `char`, `float`, `decimal`nebo.
*  Z `decimal` `sbyte`, `byte``short`, `ushort``int``uint`, `long`, `ulong`, `char`, `float`, `double`nebo.

Vzhledem k tomu, že explicitní převody zahrnují všechny implicitní a explicitní číselné převody, je vždy možné převést z libovolného *numeric_type* na jakékoli jiné *numeric_type* pomocí výrazu přetypování ([výrazy přetypování](expressions.md#cast-expressions)).

Explicitní číselné převody pravděpodobně ztratí informace nebo mohou způsobit vyvolání výjimek. Explicitní číselný převod je zpracován následujícím způsobem:

*  Pro převod z celočíselného typu na jiný integrálový typ zpracování závisí na kontextu kontroly přetečení ([kontrolované a nezaškrtnuté operátory](expressions.md#the-checked-and-unchecked-operators)), ve kterých se převod provádí:
    * V kontextu `checked` je převod úspěšný, pokud hodnota zdrojového operandu spadá do rozsahu cílového typu, ale vyvolá `System.OverflowException`, pokud je hodnota zdrojového operandu mimo rozsah cílového typu.
    * V kontextu `unchecked` je převod vždy úspěšný a pokračuje následujícím způsobem.
        * Pokud je typ zdroje větší než cílový typ, je zdrojová hodnota zkrácena tím, že zahodí "extra" nejvýznamnější bity. Výsledek je pak zpracován jako hodnota cílového typu.
        * Pokud je typ zdroje menší než cílový typ, pak je zdrojová hodnota buď znaménko, nebo nula – rozšířená, aby měla stejnou velikost jako cílový typ. Podpisové rozšíření se používá, pokud je typ zdroje podepsaný; Pokud je zdrojový typ bez znaménka, je použita nulová přípona. Výsledek je pak zpracován jako hodnota cílového typu.
        * Pokud má typ zdroje stejnou velikost jako cílový typ, pak je zdrojová hodnota zpracována jako hodnota cílového typu.
*  Pro převod z `decimal` na celočíselný typ je zdrojová hodnota zaokrouhlena směrem k nule na nejbližší celočíselnou hodnotu a tato integrální hodnota se bude výsledkem převodu. Pokud výsledná celočíselná hodnota je mimo rozsah cílového typu, je vyvolána `System.OverflowException`.
*  Pro převod z `float` nebo `double` na celočíselný typ je zpracování závislé na kontextu kontroly přetečení ([kontrolované a nezaškrtnuté operátory](expressions.md#the-checked-and-unchecked-operators)), ve kterých se převod provádí:
    * V kontextu `checked` převod pokračuje následujícím způsobem:
        * Pokud je hodnota operandu NaN nebo Infinite, je vyvolána `System.OverflowException`.
        * V opačném případě je zdrojový operand zaokrouhlen směrem k nule na nejbližší celočíselnou hodnotu. Pokud je tato celočíselná hodnota v rozsahu cílového typu, pak je tato hodnota výsledkem převodu.
        * V opačném případě je vyvolána `System.OverflowException`.
    * V kontextu `unchecked` je převod vždy úspěšný a pokračuje následujícím způsobem.
        * Pokud je hodnota operandu NaN nebo Infinite, výsledek převodu je nespecifikovaná hodnota cílového typu.
        * V opačném případě je zdrojový operand zaokrouhlen směrem k nule na nejbližší celočíselnou hodnotu. Pokud je tato celočíselná hodnota v rozsahu cílového typu, pak je tato hodnota výsledkem převodu.
        * V opačném případě je výsledkem převodu nespecifikovaná hodnota cílového typu.
*  Pro převod z `double` na `float`se hodnota `double` zaokrouhluje na nejbližší `float` hodnotu. Pokud je hodnota `double` příliš malá, aby byla reprezentována jako `float`, výsledkem bude kladné nula nebo záporné nula. Pokud je hodnota `double` příliš velká, aby reprezentovala jako `float`, výsledkem bude kladné nekonečno nebo záporné nekonečno. Pokud je hodnota `double` NaN, výsledek je také NaN.
*  Pro převod z `float` nebo `double` na `decimal`je zdrojová hodnota převedena na `decimal` reprezentaci a zaokrouhlena na nejbližší číslo po 28 desetinné místo, pokud je to požadováno ([typ Decimal](types.md#the-decimal-type)). Pokud je zdrojová hodnota příliš malá, aby byla reprezentována jako `decimal`, výsledkem bude nula. Pokud je zdrojová hodnota NaN, Infinite nebo příliš velká, aby představovala jako `decimal`, je vyvolána `System.OverflowException`.
*  Pro převod z `decimal` na `float` nebo `double`se hodnota `decimal` zaokrouhluje na nejbližší `double` nebo `float` hodnotu. I když tento převod může přijít o přesnost, nikdy nezpůsobí vyvolání výjimky.

### <a name="explicit-enumeration-conversions"></a>Explicitní převody výčtu

Explicitní převody výčtu jsou:

*  Z `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`*nebo `decimal` na enum_type.*
*  Z libovolného *enum_type* na `sbyte`, `byte``short`, `ushort``int``uint`, `long`, `ulong`, `char`, `float`, `double`, `decimal`nebo.
*  Z libovolného *enum_type* na jakékoli jiné *enum_type*.

Explicitní převod výčtu mezi dvěma typy je zpracováván tím, že se všechny účastnící *enum_type* jako podkladový typ, který *enum_type*a následně provádí implicitní nebo explicitní číselný převod mezi výslednými typy. Například vzhledem k *enum_type* `E` a základního typu `int`je převod z `E` na `byte` zpracován jako explicitní číselný převod ([explicitní číselné převody](conversions.md#explicit-numeric-conversions)) z `int` na `byte`a převod z `byte` na `E` je zpracován jako implicitní číselný převod ([implicitní číselné převody](conversions.md#implicit-numeric-conversions)) z `byte` na `int`.

### <a name="explicit-nullable-conversions"></a>Explicitní převody s možnou hodnotou null

***Explicitní převody s možnou hodnotou null*** umožňují předdefinovaným explicitním převodům, které pracují s typy hodnot, které neumožňují hodnotu null, použity také s hodnotou null formulářů těchto typů Pro každé z předdefinovaných explicitních převodů, které převádějí typ hodnoty, která není null, `S` na typ hodnoty, který neumožňuje hodnotu null `T` ([převod identity](conversions.md#identity-conversion), [implicitní číselné převody](conversions.md#implicit-numeric-conversions), [implicitní převody výčtu](conversions.md#implicit-enumeration-conversions), [explicitní číselné převody](conversions.md#explicit-numeric-conversions)a [explicitní převody výčtu](conversions.md#explicit-enumeration-conversions)), existují následující převody s možnou hodnotou null:

*  Explicitní převod z `S?` na `T?`.
*  Explicitní převod z `S` na `T?`.
*  Explicitní převod z `S?` na `T`.

Vyhodnocení převodu s možnou hodnotou null na základě základní konverze z `S` na `T` pokračuje následujícím způsobem:

*  Pokud je převod s možnou hodnotou null z `S?` na `T?`:
    * Pokud má zdrojová hodnota hodnotu null (`HasValue` vlastnost je false), výsledkem je hodnota null typu `T?`.
    * V opačném případě se převod vyhodnotí jako rozbalení z `S?` na `S`a za ním následuje základní převod z `S` na `T`a za ním i zalomení z `T` na `T?`.
*  Pokud je převod s možnou hodnotou null z `S` na `T?`, převod se vyhodnotí jako základní převod z `S` na `T` následovaný zalomením z `T` na `T?`.
*  Pokud je převod s možnou hodnotou null z `S?` na `T`, převod se vyhodnotí jako rozbalení z `S?` na `S` následované podkladovým převodem z `S` na `T`.

Všimněte si, že pokus o rozbalení hodnoty s možnou hodnotou null vyvolá výjimku, pokud je hodnota `null`.

### <a name="explicit-reference-conversions"></a>Explicitní převody odkazů

Explicitní převody odkazů:

*  Z `object` a `dynamic` na jiné *reference_type*.
*  Z jakéhokoli *class_type* `S` na všechny *class_type* `T`jsou zadané `S` základní třídou `T`.
*  Z jakéhokoli *class_type* `S` na jakýkoli *interface_type* `T`zadaný `S` není zapečetěný a poskytnutý `S` neimplementuje `T`.
*  Z jakéhokoli *interface_type* `S` na jakýkoli *class_type* `T`zadaný `T` není zapečetěný ani poskytnutý `T` implementuje `S`.
*  Z jakéhokoli *interface_type* `S` na jakýkoli *INTERFACE_TYPE* `T`, poskytnuté `S` nejsou odvozeny od `T`.
*  Z *array_type* `S` s typem elementu `SE` do *array_type* `T` s typem prvku `TE`, za předpokladu, že jsou splněny všechny následující podmínky:
    * `S` a `T` se liší pouze v typu prvku. Jinými slovy, `S` a `T` mají stejný počet rozměrů.
    * `SE` i `TE` jsou *reference_type*s.
    * Pro `TE`existuje explicitní převod odkazu z `SE`.
*  Z `System.Array` a rozhraní, která implementuje pro jakékoli *array_type*.
*  Z jednorozměrného typu pole `S[]` `System.Collections.Generic.IList<T>` a jeho základních rozhraní za předpokladu, že existuje explicitní referenční převod z `S` na `T`.
*  Z `System.Collections.Generic.IList<S>` a jeho základních rozhraní pro typ jednorozměrného pole `T[]`za předpokladu, že existuje explicitní identita nebo převod odkazu z `S` na `T`.
*  Z `System.Delegate` a rozhraní, která implementuje pro jakékoli *delegate_type*.
*  Z typu odkazu na typ odkazu `T`, pokud má explicitní referenční převod na typ odkazu `T0` a `T0` má `T`převodu identity.
*  Z odkazového typu na typ rozhraní nebo delegáta `T`, pokud má explicitní referenční převod na typ rozhraní nebo delegáta `T0` a buď `T0` je variance-konvertibilní na `T` nebo `T` je variance-konvertibilní na `T0` ([Převod variance](interfaces.md#variance-conversion)).
*  Z `D<S1...Sn>` `D<T1...Tn>`, kde `D<X1...Xn>` je obecný typ delegáta, `D<S1...Sn>` není kompatibilní s `D<T1...Tn>`nebo totožný s `Xi` a pro každý parametr typu `D` následující blokování:
    * Pokud je `Xi` invariantní, `Si` je stejný jako `Ti`.
    * Pokud `Xi` je kovariantní, pak existuje implicitní nebo explicitní identita nebo převod odkazu z `Si` na `Ti`.
    * Pokud je `Xi` kontravariantní, pak `Si` a `Ti` jsou buď identické, nebo oba typy odkazů.
*  Explicitní převody týkající se parametrů typu, které jsou známé jako odkazové typy. Další podrobnosti o explicitních převodech týkajících se parametrů typu naleznete v tématu [explicitní převody zahrnující parametry typu](conversions.md#explicit-conversions-involving-type-parameters).

Explicitní převod odkazů je převod mezi typy odkazů, které vyžadují kontroly za běhu, aby bylo zajištěno jejich správnosti.

Aby explicitní převod referencí mohl být v době běhu úspěšný, musí být hodnota zdrojového operandu `null`, nebo skutečný typ objektu, na který odkazuje zdrojový operand, musí být typ, který lze převést na cílový typ pomocí implicitního převodu odkazu ([implicitní převody odkazů](conversions.md#implicit-reference-conversions)) nebo převodu zabalení ([převody zabalení](conversions.md#boxing-conversions)). Pokud dojde k chybě explicitního převodu odkazu, je vyvolána `System.InvalidCastException`.

Převody odkazů, implicitní nebo explicitní, nikdy nezmění referenční identitu převáděného objektu. Jinými slovy, zatímco převod odkazu může změnit typ odkazu, nikdy nemění typ nebo hodnotu objektu, na který je odkazováno.

### <a name="unboxing-conversions"></a>Převody rozbalení

Převod rozbalení povoluje odkazový typ, který se explicitně převede na *value_type*. Převod rozbalení existuje z typů `object`, `dynamic` a `System.ValueType` do jakéhokoli *non_nullable_value_type*a z jakéhokoli *INTERFACE_TYPE* na *non_nullable_value_type* , který implementuje *INTERFACE_TYPE*. Kromě toho `System.Enum` typ může být nezabalený do *enum_type*.

Převod rozbalení existuje z typu odkazu na *nullable_type* , pokud převod rozbalení existuje z typu odkazu na podkladovou *non_nullable_value_type* *nullable_type*.

Typ hodnoty `S` má zabalení konverze z typu rozhraní `I`, pokud obsahuje převod rozbalení z typu rozhraní `I0` a `I0` má převod identity na `I`.

Typ hodnoty `S` má zabalení konverze z typu rozhraní `I` Pokud má převod rozbalení z rozhraní nebo typu delegáta `I0` a buď `I0` je variance-konvertibilní na `I` nebo `I` je variance-konvertibilní na `I0` ([Převod variance](interfaces.md#variance-conversion)).

Operace rozbalení se skládá z první kontroly, zda je instance objektu zabalenou hodnotou daného *value_type*a následným zkopírováním hodnoty z instance. Rozbalení odkazu s hodnotou null na *nullable_type* vytvoří hodnotu null *nullable_type*. Struktura může být z typu `System.ValueType`neohraničena, protože se jedná o základní třídu pro všechny struktury ([Dědičnost](structs.md#inheritance)).

Převody rozbalení jsou podrobněji popsány v tématu [převody rozbalení](types.md#unboxing-conversions).

### <a name="explicit-dynamic-conversions"></a>Explicitní dynamické převody

Explicitní dynamický převod existuje ve výrazu typu `dynamic` na libovolný typ `T`. Převod je dynamicky svázán ([dynamická vazba](expressions.md#dynamic-binding)), což znamená, že explicitní převod bude vyžádán za běhu z běhového typu výrazu do `T`. Pokud není nalezen žádný převod, je vyvolána výjimka za běhu.

Pokud dynamická vazba převodu není žádoucí, výraz může být nejprve převeden na `object`a následně na požadovaný typ.

Předpokládejme, že je definována následující třída:
```csharp
class C
{
    int i;

    public C(int i) { this.i = i; }

    public static explicit operator C(string s) 
    {
        return new C(int.Parse(s));
    }
}
```

Následující příklad ukazuje explicitní dynamické převody:
```csharp
object o  = "1";
dynamic d = "2";

var c1 = (C)o; // Compiles, but explicit reference conversion fails
var c2 = (C)d; // Compiles and user defined conversion succeeds
```

Nejlepší převod `o` na `C` byl nalezen v době kompilace, aby byl explicitním převodem odkazu. Tím dojde k chybě v době běhu, protože `"1"` není ve skutečnosti `C`. Konverze `d` na `C`, jako explicitní dynamický převod, je pozastavena za běhu, kde je nalezen uživatelem definovaný převod z běhového typu `d` -- `string`--na `C` a úspěch.

### <a name="explicit-conversions-involving-type-parameters"></a>Explicitní převody týkající se parametrů typu

Pro daný parametr typu existují následující explicitní převody `T`:

*  Od efektivní základní třídy `C` `T` do `T` a z jakékoli základní třídy `C` až `T`. V době běhu, pokud je `T` typ hodnoty, je převod proveden jako převod rozbalení. V opačném případě je převod proveden jako explicitní převod referencí nebo převod identity.
*  Z libovolného typu rozhraní `T`. V době běhu, pokud je `T` typ hodnoty, je převod proveden jako převod rozbalení. V opačném případě je převod proveden jako explicitní převod referencí nebo převod identity.
*  Z `T` na jakékoli *interface_type* `I` nejsou k dispozici již implicitní převod z `T` na `I`. V době běhu, pokud je `T` typ hodnoty, je převod proveden jako převod zabalení následovaný explicitním převodem odkazu. V opačném případě je převod proveden jako explicitní převod referencí nebo převod identity.
*  Z parametru typu `U` do `T`je zadaný `T` závislý na `U` ([omezení parametrů typu](classes.md#type-parameter-constraints)). V době běhu, pokud je `U` typ hodnoty, pak `T` a `U` jsou nutně stejného typu a není proveden žádný převod. V opačném případě, pokud je `T` typ hodnoty, je převod proveden jako zabalení převodu. V opačném případě je převod proveden jako explicitní převod referencí nebo převod identity.

Pokud je `T` známý jako typ odkazu, výše uvedené převody jsou klasifikovány jako explicitní převody odkazů ([explicitní převody odkazů](conversions.md#explicit-reference-conversions)). Pokud `T` není známý jako typ odkazu, převody uvedené výše jsou klasifikovány jako převody rozbalení ([převody rozbalení](conversions.md#unboxing-conversions)).

Výše uvedená pravidla nepovolují přímý explicitní převod z neomezeného parametru typu na typ, který není typu rozhraní, který může být překvapivé. Důvodem pro toto pravidlo je zabránit nejasnostem a zajistit, aby sémantika takových převodů byla jasná. Předpokládejme například následující deklaraci:
```csharp
class X<T>
{
    public static long F(T t) {
        return (long)t;                // Error 
    }
}
```

Pokud byla povolená přímá explicitní konverze `t` na `int`, může jedna z nich jednoduše očekávat, že `X<int>.F(7)` vrátí `7L`. Nicméně to neplatí, protože standardní číselné převody jsou zváženy pouze v případě, že typy jsou známy v době vytváření vazby. Aby se sémantika jasně vymazala, je nutné, aby byl napsaný výše uvedený příklad:
```csharp
class X<T>
{
    public static long F(T t) {
        return (long)(object)t;        // Ok, but will only work when T is long
    }
}
```

Tento kód nyní zkompiluje, ale provede `X<int>.F(7)` by pak vyvolal výjimku za běhu, protože zabalený `int` nelze převést přímo na `long`.

### <a name="user-defined-explicit-conversions"></a>Uživatelem definované explicitní převody

Uživatelem definovaný explicitní převod se skládá z volitelného standardního explicitního převodu, za nímž následuje spuštění uživatelsky definovaného implicitního nebo explicitního operátoru převodu následovaný jiným volitelným standardním explicitním převodem. Přesná pravidla pro vyhodnocení uživatelem definovaných explicitních převodů jsou popsány ve [zpracování uživatelem definovaných explicitních převodů](conversions.md#processing-of-user-defined-explicit-conversions).

## <a name="standard-conversions"></a>Standardní převody

Standardní převody jsou takové předdefinované převody, které mohou nastat jako součást uživatelsky definovaného převodu.

### <a name="standard-implicit-conversions"></a>Standardní implicitní převody

Následující implicitní převody jsou klasifikovány jako standardní implicitní převody:

*  Převody identity ([převod identity](conversions.md#identity-conversion))
*  Implicitní číselné převody ([implicitní číselné převody](conversions.md#implicit-numeric-conversions))
*  Implicitní převody povolující hodnotu null ([implicitní převody s možnou hodnotou null](conversions.md#implicit-nullable-conversions))
*  Implicitní převody odkazů ([implicitní převody odkazů](conversions.md#implicit-reference-conversions))
*  Převody zabalení ([převody zabalení](conversions.md#boxing-conversions))
*  Implicitní převody konstantních výrazů ([implicitní dynamické převody](conversions.md#implicit-dynamic-conversions))
*  Implicitní převody zahrnující parametry typu ([implicitní převody zahrnující parametry typu](conversions.md#implicit-conversions-involving-type-parameters))

Standardní implicitní převody konkrétně vylučují uživatelem definované implicitní převody.

### <a name="standard-explicit-conversions"></a>Standardní explicitní převody

Standardní explicitní převody jsou všechny standardní implicitní převody a podmnožina explicitních převodů, pro které existuje opačný standardní implicitní převod. Jinými slovy, pokud standardní implicitní převod existuje z typu `A` na `B`typu, pak standardní explicitní převod existuje z typu `A` na typ `B` a z typu `B` na typ `A`.

## <a name="user-defined-conversions"></a>Uživatelem definované převody

C#umožňuje rozšířit předdefinované implicitní a explicitní převody na základě ***uživatelsky definovaných převodů***. Uživatelsky definované převody jsou zavedeny deklarováním operátorů převodu ([operátory převodu](classes.md#conversion-operators)) v typech třídy a struktury.

### <a name="permitted-user-defined-conversions"></a>Povolené uživatelsky definované převody

C#povoluje deklaraci určitých uživatelsky definovaných převodů. Konkrétně není možné předefinovat již existující implicitní nebo explicitní převod.

Pro daný typ zdroje `S` a cílový typ `T`, pokud `S` nebo `T` jsou typy s možnou hodnotou null, nechejte `S0` a `T0` odkazují na jejich podkladové typy, jinak `S0` a `T0` jsou rovny `S` a `T` v uvedeném pořadí. Třída nebo struktura je oprávněna deklarovat převod ze zdrojového typu `S` na cílový typ `T` pouze v případě, že jsou splněny všechny následující podmínky:

*  `S0` a `T0` jsou různé typy.
*  `S0` nebo `T0` je typ třídy nebo struktury, ve které se provádí deklarace operátoru.
*  Ani `S0` ani `T0` není *INTERFACE_TYPE*.
*  S výjimkou uživatelem definovaných převodů neexistuje převod z `S` na `T` nebo z `T` na `S`.

Omezení, která platí pro uživatelem definované převody, jsou podrobněji popsána v části [operátory převodu](classes.md#conversion-operators).

### <a name="lifted-conversion-operators"></a>Zrušené operátory převodu

Předaný uživatelsky definovaný operátor převodu, který se převede z typu hodnoty, která není null, `S` na `T`typu hodnoty, který není null, existuje převedený ***operátor převodu*** , který se převede z `S?` na `T?`. Tento operátor převedený převod provádí rozbalení z `S?` na `S` následovaný uživatelem definovaným převodem z `S` na `T` následovaným zalomením z `T` na `T?`, s tím rozdílem, že hodnota null `S?` převádí přímo na `T?`vracející hodnotu null.

Předaný operátor převodu má stejnou implicitní nebo explicitní klasifikaci jako svůj základní uživatelsky definovaný operátor převodu. Pojem "uživatelsky definovaný převod" se vztahuje na použití uživatelsky definovaných i převedených operátorů převodu.

### <a name="evaluation-of-user-defined-conversions"></a>Vyhodnocení uživatelem definovaných převodů

Uživatelsky definovaný převod převede hodnotu z jejího typu označovaného jako ***typ zdroje***na jiný typ, který se nazývá ***cílový typ***. Vyhodnocení uživatelem definovaných převodových Center při hledání ***nejpřesnější uživatelsky*** definovaného operátoru převodu pro konkrétní zdrojové a cílové typy. Toto určení je rozdělené do několika kroků:

*  Hledání sady tříd a struktur, ze kterých se budou brát v úvahu uživatelsky definované operátory převodu. Tato sada se skládá ze zdrojového typu a jeho základních tříd a cílového typu a jeho základních tříd (s implicitními předpoklady, které pouze třídy a struktury mohou deklarovat uživatelsky definované operátory a které typy bez třídy nemají žádné základní třídy). Pro účely tohoto kroku, pokud je zdroj nebo cílový typ *nullable_type*, místo toho se použije jejich podkladový typ.
*  Z této sady typů určete, které uživatelsky definované a přenesené operátory převodu jsou platné. Pro operátor převodu musí být možné provést standardní převod ([standardní převody](conversions.md#standard-conversions)) ze zdrojového typu na typ operandu operátoru a musí být možné provést standardní převod z výsledného typu operátoru na cílový typ.
*  Ze sady příslušných uživatelsky definovaných operátorů určete, který operátor je jednoznačně určený. Obecně platí, že nejpřesnější operátor je operátor, jehož typ operandu je "nejbližší" ke zdrojovému typu a jehož výsledný typ je "nejbližší" cílovému typu. Uživatelsky definované operátory převodu jsou upřednostňovány přes přenesené operátory převodu. Přesná pravidla pro určení nejpřesnější uživatelsky definovaného operátoru převodu jsou definována v následujících oddílech.

Po identifikaci uživatelsky definovaného operátoru převodu, který představuje uživatelsky definovaný převod, se zobrazí až tři kroky:

*  Nejprve v případě potřeby proveďte standardní převod ze zdrojového typu na typ operandu uživatelem definovaného nebo převedené operátoru převodu.
*  V dalším kroku vyvoláte uživatelem definovaný nebo převedený operátor převodu k provedení převodu.
*  Nakonec v případě potřeby proveďte standardní převod z typu výsledku operátoru převodu definovaného uživatelem nebo převedený na cílový typ.

Vyhodnocení uživatelsky definovaného převodu nikdy nezahrnuje více než jednoho uživatelsky definovaného nebo přecházejícího operátoru převodu. Jinými slovy převod typu `S` na typ `T` nikdy nespustí uživatelem definovaný převod z `S` na `X` a pak provede uživatelem definovaný převod z `X` na `T`.

Přesné definice vyhodnocení uživatelem definovaných implicitních nebo explicitních převodů jsou uvedeny v následujících oddílech. Definice využívají následující pojmy:

*  Pokud standardní implicitní převod ([standardní implicitní převody](conversions.md#standard-implicit-conversions)) existuje z typu `A` na typ `B`a v případě, že ani `A` ani `B` nejsou *INTERFACE_TYPE*s, pak `A` se bude ***považovat za `B`a*** `B` se ***říká `A`.***
*  ***Nejvíce zahrnuje typ*** v sadě typů je jeden typ, který zahrnuje všechny ostatní typy v sadě. Pokud žádný jednotlivý typ neobsahuje všechny ostatní typy, pak sada neobsahuje žádné typy, které by zahrnovaly. Ve více intuitivních případech je největším typem v množině "největší" typ v sadě – jeden typ, na který lze všechny ostatní typy implicitně převést.
*  ***Nejvýznamnější typ*** v sadě typů je jeden typ, který je zahrnut ve všech ostatních typech v sadě. Pokud žádný jednotlivý typ není zahrnut ve všech ostatních typech, pak sada nemá nejvíce zahrnující typ. Ve více intuitivních výrazech se nejčastěji zahrnuje typ "nejmenší" typ v sadě – jeden typ, který lze implicitně převést na každý z ostatních typů.

### <a name="processing-of-user-defined-implicit-conversions"></a>Zpracování implicitních převodů definovaných uživatelem

Uživatelem definovaný implicitní převod z typu `S` na typ `T` je zpracován následujícím způsobem:

*  Určete typy `S0` a `T0`. Pokud `S` nebo `T` jsou typy s možnou hodnotou null, `S0` a `T0` jsou jejich podkladové typy, jinak `S0` a `T0` jsou rovny `S` a `T` v uvedeném pořadí.
*  Najde sadu typů `D`, ze kterých se budou brát v úvahu uživatelsky definované operátory převodu. Tato sada se skládá z `S0` (Pokud `S0` je třída nebo struktura), základní třídy `S0` (Pokud `S0` je třída) a `T0` (Pokud `T0` je třída nebo struktura).
*  Najděte sadu příslušných uživatelsky definovaných a přenesených operátorů převodu `U`. Tato sada se skládá z uživatelem definovaných a převedených implicitních operátorů převodu deklarovaných třídami nebo strukturami v `D`, které převádějí z typu, který zahrnuje `S` na typ, který je zahrnut `T`. Pokud je `U` prázdné, převod není definován a dojde k chybě při kompilaci.
*  Vyhledá nejvíce konkrétní typ zdroje `SX`operátory v `U`:
    * Pokud některý z operátorů v `U` převést z `S`, `SX` `S`.
    * V opačném případě `SX` je nejčastěji zahrnující typ v kombinované sadě zdrojových typů operátorů v `U`. Pokud nelze nalézt přesně jeden z výše zahrnutého typu, převod je nejednoznačný a dojde k chybě při kompilaci.
*  Vyhledá nejvíce konkrétní cílový typ `TX`operátory v `U`:
    * Pokud některý z operátorů v `U` převést na `T`, `TX` `T`.
    * V opačném případě je `TX` nejběžnějším typem v kombinované sadě cílových typů operátorů v `U`. Pokud se přesně jeden typ, který sestává, nedá najít, převod je nejednoznačný a dojde k chybě při kompilaci.
*  Najít nejvíce konkrétního operátora převodu:
    * Pokud `U` obsahuje právě jeden uživatelsky definovaný operátor převodu, který se převede z `SX` na `TX`, pak toto je nejpřesnější operátor převodu.
    * V opačném případě, pokud `U` obsahuje právě jeden převedený operátor převodu, který se převede z `SX` na `TX`, pak toto je nejpřesnější operátor převodu.
    * V opačném případě převod je nejednoznačný a dojde k chybě při kompilaci.
*  Nakonec použít převod:
    * Pokud `S` není `SX`, je proveden standardní implicitní převod z `S` na `SX`.
    * Nejpřesnější operátor převodu je vyvolán pro převod z `SX` na `TX`.
    * Pokud `TX` není `T`, je proveden standardní implicitní převod z `TX` na `T`.

### <a name="processing-of-user-defined-explicit-conversions"></a>Zpracování explicitních převodů definovaných uživatelem

Uživatelem definovaný explicitní převod z typu `S` na typ `T` je zpracován následujícím způsobem:

*  Určete typy `S0` a `T0`. Pokud `S` nebo `T` jsou typy s možnou hodnotou null, `S0` a `T0` jsou jejich podkladové typy, jinak `S0` a `T0` jsou rovny `S` a `T` v uvedeném pořadí.
*  Najde sadu typů `D`, ze kterých se budou brát v úvahu uživatelsky definované operátory převodu. Tato sada se skládá z `S0` (Pokud `S0` je třída nebo struktura), základní třídy `S0` (Pokud `S0` je třída), `T0` (Pokud `T0` je třída nebo struktura) a základní třídy `T0` (Pokud `T0` je třída).
*  Najděte sadu příslušných uživatelsky definovaných a přenesených operátorů převodu `U`. Tato sada se skládá z uživatelem definovaných a převedených implicitních nebo explicitních operátorů převodu deklarovaných třídami nebo strukturami v `D`, které převádějí z typu zahrnujícího nebo zahrnutého `S` na typ, který zahrnuje nebo zahrnuje `T`. Pokud je `U` prázdné, převod není definován a dojde k chybě při kompilaci.
*  Vyhledá nejvíce konkrétní typ zdroje `SX`operátory v `U`:
    * Pokud některý z operátorů v `U` převést z `S`, `SX` `S`.
    * V opačném případě, pokud některý z operátorů v `U` převést z typů, které zahrnují `S`, pak `SX` je nejčastěji zahrnující typ v kombinované sadě zdrojových typů těchto operátorů. Pokud se nenajde žádný z výše zahrnutého typu, převod je nejednoznačný a dojde k chybě při kompilaci.
    * V opačném případě je `SX` nejběžnějším typem v kombinované sadě zdrojových typů operátorů v `U`. Pokud se přesně jeden typ, který sestává, nedá najít, převod je nejednoznačný a dojde k chybě při kompilaci.
*  Vyhledá nejvíce konkrétní cílový typ `TX`operátory v `U`:
    * Pokud některý z operátorů v `U` převést na `T`, `TX` `T`.
    * V opačném případě, pokud některý z operátorů v `U` převést na typy, které jsou zahrnuté v `T`, pak `TX` je nejčastěji zahrnující typ v kombinované sadě cílových typů těchto operátorů. Pokud se přesně jeden typ, který sestává, nedá najít, převod je nejednoznačný a dojde k chybě při kompilaci.
    * V opačném případě `TX` je nejčastěji zahrnující typ v kombinované sadě cílových typů operátorů v `U`. Pokud se nenajde žádný z výše zahrnutého typu, převod je nejednoznačný a dojde k chybě při kompilaci.
*  Najít nejvíce konkrétního operátora převodu:
    * Pokud `U` obsahuje právě jeden uživatelsky definovaný operátor převodu, který se převede z `SX` na `TX`, pak toto je nejpřesnější operátor převodu.
    * V opačném případě, pokud `U` obsahuje právě jeden převedený operátor převodu, který se převede z `SX` na `TX`, pak toto je nejpřesnější operátor převodu.
    * V opačném případě převod je nejednoznačný a dojde k chybě při kompilaci.
*  Nakonec použít převod:
    * Pokud `S` není `SX`, provede se standardní explicitní převod z `S` na `SX`.
    * Nejpřesnější uživatelsky definovaný operátor převodu je vyvolán pro převod z `SX` na `TX`.
    * Pokud `TX` není `T`, provede se standardní explicitní převod z `TX` na `T`.

## <a name="anonymous-function-conversions"></a>Anonymní převody funkcí

*Anonymous_method_expression* nebo *lambda_expression* jsou klasifikovány jako anonymní funkce ([výrazy anonymní funkce](expressions.md#anonymous-function-expressions)). Výraz nemá typ, ale lze jej implicitně převést na kompatibilní typ delegáta nebo strom výrazu. Konkrétně anonymní funkce `F` je kompatibilní s typem delegáta `D` poskytnutá:

*  Pokud `F` obsahuje *anonymous_function_signature*, `D` a `F` mají stejný počet parametrů.
*  Pokud `F` neobsahuje *anonymous_function_signature*, pak `D` může mít nula nebo více parametrů jakéhokoliv typu, pokud žádný parametr `D` nemá modifikátor `out` parametru.
*  Pokud `F` má explicitně typový seznam parametrů, každý parametr v `D` má stejný typ a modifikátory jako odpovídající parametr v `F`.
*  Pokud `F` má implicitně typový seznam parametrů, `D` nemá žádné `ref` ani parametry `out`.
*  Pokud je tělo `F` výrazem a buď má `D` `void` návratový typ, nebo je `F` asynchronní a `D` návratového typu, pak když je každý parametr `Task`přiřazen typu odpovídajícího parametru v `F`, tělo `D`je platným výrazem (WRT [výrazy](expressions.md)), který by byl povolen jako *`F`* ([příkazy výrazu](statements.md#expression-statements)).
*  Pokud je tělo `F` blok příkazu a buď má `D` `void` návratový typ, nebo je `F` asynchronní a `D` návratového typu, pak když je každý parametr `Task`přiřazen typu odpovídajícího parametru v `F`, tělo `D`je platným blokem příkazu ( [bloky](statements.md#blocks)WRT), ve kterém žádný příkaz `F` neurčuje výraz.
*  Pokud je text `F` výraz a *buď* `F` je neasynchronní a `D` je návratový `T`typ jiný než void, *nebo* `F` je Async a `D` má návratový typ `Task<T>`, pak když je každý parametr `F` dán typem odpovídajícího parametru v `D`, tělo `F` je platný výraz (WRT [výrazy](expressions.md)), který je implicitně převeden na `T`.
*  Pokud je text `F` blok příkazu a *buď* `F` je neasynchronní a `D` má návratový typ, který není void `T`, *nebo* `F` je Async a `D` má návratový typ `Task<T>`, pak když každý parametr `F` má přidělen typ odpovídajícího parametru v `D`, tělo `F` je platným blokem příkazu ( [bloky](statements.md#blocks)WRT) s nedosažitelným koncovým bodem, ve kterém každý příkaz `return` určuje výraz, který je implicitně převeden na `T`.

Pro účely zkrácení Tato část používá krátký tvar typů úloh `Task` a `Task<T>` ([asynchronní funkce](classes.md#async-functions)).

Výraz lambda `F` je kompatibilní s typem stromu výrazu `Expression<D>`, pokud `F` je kompatibilní s typem delegáta `D`. Všimněte si, že to neplatí pro anonymní metody, pouze lambda výrazy.

Některé výrazy lambda nelze převést na typy stromu výrazů: i když převod *existuje*, při kompilaci dojde k chybě. Toto je případ, pokud výraz lambda:

*  Má tělo *bloku*
*  Obsahuje operátory jednoduchého nebo složeného přiřazení.
*  Obsahuje dynamicky vázaný výraz
*  Je asynchronní

Níže uvedené příklady používají typ obecného delegáta `Func<A,R>`, který představuje funkci, která přebírá argument typu `A` a vrací hodnotu typu `R`:
```csharp
delegate R Func<A,R>(A arg);
```

V přiřazeních
```csharp
Func<int,int> f1 = x => x + 1;                 // Ok

Func<int,double> f2 = x => x + 1;              // Ok

Func<double,int> f3 = x => x + 1;              // Error

Func<int, Task<int>> f4 = async x => x + 1;    // Ok
```
parametry a návratové typy jednotlivých anonymních funkcí jsou určeny z typu proměnné, ke které je přiřazena anonymní funkce.

První přiřazení úspěšně převede anonymní funkci na typ delegáta `Func<int,int>`, protože pokud `x` předaný typ `int`, `x+1` je platný výraz, který lze implicitně převést na typ `int`.

Stejně tak druhé přiřazení úspěšně převede anonymní funkci na typ delegáta `Func<int,double>`, protože výsledek `x+1` (typu `int`) je implicitně převeden na typ `double`.

Třetí přiřazení je ale chyba při kompilaci, protože když `x` je předaný typ `double`, není výsledek `x+1` (typu `double`) implicitně převoditelný na typ `int`.

Čtvrté přiřazení úspěšně převede anonymní asynchronní funkci na typ delegáta `Func<int, Task<int>>`, protože výsledek `x+1` (typu `int`) je implicitně převeden na typ výsledku `int` typu úlohy `Task<int>`.

Anonymní funkce mohou ovlivnit rozlišení přetížení a účastnit se odvození typu. Další podrobnosti viz [Členové funkce](expressions.md#function-members) .

### <a name="evaluation-of-anonymous-function-conversions-to-delegate-types"></a>Vyhodnocení anonymních převodů funkcí na typy delegátů

Převod anonymní funkce na typ delegáta vytvoří instanci delegáta, která odkazuje na anonymní funkci a (případně prázdnou) sadu zachycených vnějších proměnných, které jsou aktivní v době vyhodnocení. Když je vyvolán delegát, je provedeno tělo anonymní funkce. Kód v těle je proveden pomocí sady zachycených vnějších proměnných, na které odkazuje delegát.

Seznam volání delegáta vytvořeného z anonymní funkce obsahuje jednu položku. Přesný cílový objekt a cílová metoda delegáta nejsou určeny. Konkrétně není určeno, zda je cílový objekt delegáta `null`, `this` hodnota ohraničujícího člena funkce nebo jiný objekt.

Převody sémanticky identických anonymních funkcí se stejnou (možná prázdnou) sadou zachycených instancí vnějších proměnných na stejné typy delegátů jsou povoleny (ale nejsou požadovány) pro vrácení stejné instance delegáta. Pojem sémanticky totožné se zde používá, aby bylo možné, že provádění anonymních funkcí bude ve všech případech dávat stejné důsledky pro stejné argumenty. Toto pravidlo povoluje optimalizaci kódu, jako je například následující.

```csharp
delegate double Function(double x);

class Test
{
    static double[] Apply(double[] a, Function f) {
        double[] result = new double[a.Length];
        for (int i = 0; i < a.Length; i++) result[i] = f(a[i]);
        return result;
    }

    static void F(double[] a, double[] b) {
        a = Apply(a, (double x) => Math.Sin(x));
        b = Apply(b, (double y) => Math.Sin(y));
        ...
    }
}
```

Vzhledem k tomu, že dva Delegáti anonymních funkcí mají stejnou (prázdnou) sadu zachycených vnějších proměnných a vzhledem k tomu, že anonymní funkce jsou sémanticky identické, kompilátor má oprávnění odkazovat na stejnou cílovou metodu. Kompilátor je však povolen pro vrácení stejné instance delegáta z anonymních výrazů Functions.

### <a name="evaluation-of-anonymous-function-conversions-to-expression-tree-types"></a>Vyhodnocení anonymních převodů funkcí na typy stromu výrazů

Převod anonymní funkce na typ stromu výrazu vytvoří strom výrazu ([typy stromu výrazů](types.md#expression-tree-types)). Přesněji, vyhodnocení konverze anonymní funkce vede k konstrukci struktury objektů, která představuje strukturu samotné anonymní funkce. Přesná struktura stromu výrazu a také přesný proces pro jeho vytvoření jsou definovány implementací.

### <a name="implementation-example"></a>Příklad implementace

Tato část popisuje možnou implementaci anonymních převodů funkcí v souvislosti s C# jinými konstrukcemi. Zde popsaná implementace je založena na stejných zásadách, které používá kompilátor společnosti C# Microsoft, ale nejedná se o udělenou implementaci, ani o to, že to není jediné. Krátce se zmiňuje o převodech na stromy výrazů, protože jejich Přesná sémantika je mimo rozsah této specifikace.

Zbývající část tohoto oddílu obsahuje několik příkladů kódu, které obsahují anonymní funkce s různými charakteristikami. Pro každý příklad je k dispozici odpovídající překlad kódu, který používá C# pouze jiné konstrukce. V příkladech se k identifikátoru `D` předpokládá, že představuje následující typ delegáta:
```csharp
public delegate void D();
```

Nejjednodušší forma anonymní funkce je taková, která zachytává žádné vnější proměnné:
```csharp
class Test
{
    static void F() {
        D d = () => { Console.WriteLine("test"); };
    }
}
```

To lze přeložit na instanci delegáta, která odkazuje na statickou metodu generovanou kompilátorem, ve které je umístěn kód anonymní funkce:
```csharp
class Test
{
    static void F() {
        D d = new D(__Method1);
    }

    static void __Method1() {
        Console.WriteLine("test");
    }
}
```

V následujícím příkladu anonymní funkce odkazuje na členy instance `this`:
```csharp
class Test
{
    int x;

    void F() {
        D d = () => { Console.WriteLine(x); };
    }
}
```

To lze přeložit na metodu instance generovanou kompilátorem, která obsahuje kód anonymní funkce:
```csharp
class Test
{
    int x;

    void F() {
        D d = new D(__Method1);
    }

    void __Method1() {
        Console.WriteLine(x);
    }
}
```

V tomto příkladu anonymní funkce zachytí místní proměnnou:
```csharp
class Test
{
    void F() {
        int y = 123;
        D d = () => { Console.WriteLine(y); };
    }
}
```

Životnost místní proměnné se teď musí rozšířit aspoň na dobu života delegáta anonymní funkce. To lze dosáhnout použitím "zdvihacího" místní proměnné do pole třídy generované kompilátorem. Vytvoření instance místní proměnné ([vytváření instancí místních proměnných](expressions.md#instantiation-of-local-variables)) pak odpovídá vytvoření instance třídy generované kompilátorem a přístup k místní proměnné odpovídá přístupu k poli v instanci třídy generované kompilátorem. Anonymní funkce se navíc stávají metodou instance třídy generované kompilátorem:
```csharp
class Test
{
    void F() {
        __Locals1 __locals1 = new __Locals1();
        __locals1.y = 123;
        D d = new D(__locals1.__Method1);
    }

    class __Locals1
    {
        public int y;

        public void __Method1() {
            Console.WriteLine(y);
        }
    }
}
```

Nakonec následující anonymní funkce zachytí `this` a také dvě místní proměnné s různými životnostmi:
```csharp
class Test
{
    int x;

    void F() {
        int y = 123;
        for (int i = 0; i < 10; i++) {
            int z = i * 2;
            D d = () => { Console.WriteLine(x + y + z); };
        }
    }
}
```

Tady je vytvořena třída vygenerovaná kompilátorem pro každý blok příkazu, ve kterém jsou lokální hodnoty zachyceny tak, že místní proměnné v různých blocích mohou mít nezávislé životnosti. Instance `__Locals2`, třída vygenerovaná kompilátorem pro blok vnitřního příkazu, obsahuje místní proměnnou `z` a pole, které odkazuje na instanci `__Locals1`.  Instance `__Locals1`, třída vygenerovaná kompilátorem pro blok vnějšího příkazu, obsahuje místní proměnnou `y` a pole, které odkazuje `this` ohraničujícího člena funkce. Pomocí těchto datových struktur je možné dosáhnout všech zachycených vnějších proměnných prostřednictvím instance `__Local2`a kód anonymní funkce lze proto implementovat jako metodu instance této třídy.

```csharp
class Test
{
    void F() {
        __Locals1 __locals1 = new __Locals1();
        __locals1.__this = this;
        __locals1.y = 123;
        for (int i = 0; i < 10; i++) {
            __Locals2 __locals2 = new __Locals2();
            __locals2.__locals1 = __locals1;
            __locals2.z = i * 2;
            D d = new D(__locals2.__Method1);
        }
    }

    class __Locals1
    {
        public Test __this;
        public int y;
    }

    class __Locals2
    {
        public __Locals1 __locals1;
        public int z;

        public void __Method1() {
            Console.WriteLine(__locals1.__this.x + __locals1.y + z);
        }
    }
}
```

Stejný postup, který se používá pro zachycení místních proměnných, se dá použít i při převodu anonymních funkcí na stromy výrazů: odkazy na objekty vygenerované kompilátorem můžou být uložené ve stromu výrazů a přístup k místním proměnným může být. je reprezentovaná jako přístup k poli u těchto objektů. Výhodou tohoto přístupu je to, že umožňuje sdílení "" vyzdvižených "místních proměnných mezi delegáty a stromy výrazů.

## <a name="method-group-conversions"></a>Převody skupin metod

Implicitní převod ([implicitní převody](conversions.md#implicit-conversions)) existuje ze skupiny metod ([klasifikace výrazů](expressions.md#expression-classifications)) na kompatibilní typ delegáta. Vzhledem k typu delegáta `D` a výrazu `E`, který je klasifikován jako skupina metod, existuje implicitní převod z `E` na `D`, pokud `E` obsahuje alespoň jednu metodu, která je použita v normálním formuláři ([příslušný člen funkce](expressions.md#applicable-function-member)) na seznam argumentů konstruovaných pomocí typů parametrů a modifikátorů `D`, jak je popsáno v následujícím tématu.

Aplikace v době kompilace převodu ze skupiny metod `E` na typ delegáta `D` je popsána v následujícím tématu. Počítejte s tím, že existence implicitního převodu z `E` na `D` nezaručuje, že při kompilaci dojde k úspěšnému provedení převodu bez chyby.

*  Je vybrána jedna metoda `M` odpovídající vyvolání metody ([vyvolání metod](expressions.md#method-invocations)) `E(A)`formuláře s následujícími změnami:
    * Seznam argumentů `A` je seznam výrazů, každý klasifikovaný jako proměnná a s typem a modifikátorem (`ref` nebo `out`) odpovídajícího parametru v *formal_parameter_list* `D`.
    * Uvažované metody kandidáta jsou pouze metody, které jsou použitelné v jejich normálním formátu ([platný člen funkce](expressions.md#applicable-function-member)), nikoli ty, které platí pouze v rozbalených formulářích.
*  Pokud algoritmus [vyvolání metody](expressions.md#method-invocations) vyvolá chybu, dojde k chybě při kompilaci. V opačném případě algoritmus vytvoří jednu nejlepší metodu `M` se stejným počtem parametrů jako `D` a převod je považován za existující.
*  Vybraná metoda `M` musí být kompatibilní s typem[delegáta](delegates.md#delegate-compatibility)`D`nebo v opačném případě dojde k chybě při kompilaci.
*  Pokud je vybraná metoda `M` metoda instance, výraz instance přidružený k `E` Určuje cílový objekt delegáta.
*  Pokud je vybraná metoda M rozšíření rozšiřující metoda, která je označena pomocí přístupu člena k výrazu instance, tento výraz instance Určuje cílový objekt delegáta.
*  Výsledkem převodu je hodnota typu `D`, konkrétně nově vytvořený delegát, který odkazuje na vybranou metodu a cílový objekt.
*  Všimněte si, že tento proces může vést k vytvoření delegáta metody rozšíření, pokud algoritmus [vyvolání metody](expressions.md#method-invocations) nenalezne metodu instance, ale úspěch zpracovává vyvolání `E(A)` jako volání metody rozšíření ([vyvolání rozšiřující metody](expressions.md#extension-method-invocations)). Takto vytvořený delegát zachytí metodu rozšíření i její první argument.

Následující příklad ukazuje převody skupin metod:
```csharp
delegate string D1(object o);

delegate object D2(string s);

delegate object D3();

delegate string D4(object o, params object[] a);

delegate string D5(int i);

class Test
{
    static string F(object o) {...}

    static void G() {
        D1 d1 = F;            // Ok
        D2 d2 = F;            // Ok
        D3 d3 = F;            // Error -- not applicable
        D4 d4 = F;            // Error -- not applicable in normal form
        D5 d5 = F;            // Error -- applicable but not compatible

    }
}
```

Přiřazení `d1` implicitně převede skupinu metod `F` na hodnotu typu `D1`.

Přiřazení `d2` ukazuje, jak je možné vytvořit delegáta pro metodu, která má méně odvozené (kontravariantní) typy parametrů a další odvozené (kovariantní) návratový typ.

Přiřazení `d3` ukazuje, jak žádný převod neexistuje, pokud metoda není k dispozici.

Přiřazení `d4` ukazuje, jak se metoda musí použít v normálním tvaru.

Přiřazení `d5` ukazuje, jak se mohou parametry a návratové typy delegáta a metody lišit pouze pro typy odkazů.

Stejně jako u všech ostatních implicitních a explicitních převodů lze operátor přetypování použít k explicitnímu provedení převodu skupiny metody. Proto příklad
```csharp
object obj = new EventHandler(myDialog.OkClick);
```
místo toho je možné zapisovat.
```csharp
object obj = (EventHandler)myDialog.OkClick;
```

Skupiny metod mohou ovlivnit rozlišení přetížení a účastnit se odvození typu. Další podrobnosti viz [Členové funkce](expressions.md#function-members) .

Vyhodnocení převodu skupiny metod metodou Run-Time pokračuje následujícím způsobem:

*  Pokud je metoda vybraná v době kompilace metoda instance, nebo se jedná o metodu rozšíření, která je k dispozici jako metoda instance, je cílový objekt delegáta určen z výrazu instance přidruženého k `E`:
    * Výraz instance je vyhodnocen. Pokud toto vyhodnocení způsobí výjimku, nejsou provedeny žádné další kroky.
    * Pokud je výraz instance *reference_type*, hodnota počítaná výrazem instance se stala cílovým objektem. Pokud je vybraná metoda metoda instance a cílový objekt je `null`, je vyvolána `System.NullReferenceException` a nejsou provedeny žádné další kroky.
    * Pokud je výraz instance *value_type*, je provedena operace zabalení ([převody zabalení](types.md#boxing-conversions)) pro převod hodnoty na objekt a tento objekt se stává cílovým objektem.
*  V opačném případě je vybraná metoda součástí volání statické metody a cílový objekt delegáta je `null`.
*  Je přidělena nová instance typu delegáta `D`. Pokud není k dispozici dostatek paměti pro přidělení nové instance, je vyvolána `System.OutOfMemoryException` a nejsou spuštěny žádné další kroky.
*  Nová instance delegáta je inicializována s odkazem na metodu, která byla určena v době kompilace, a odkaz na cílový objekt vypočítaný výše.
