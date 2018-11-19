# <a name="conversions"></a>Převody

A ***převod*** umožňuje výraz, který se považují za určitého typu. Převod může způsobit, že výraz daného typu zacházeno jako s jiným typem, nebo může to způsobit bez typu, chcete-li získat typ výrazu. Může být převody ***implicitní*** nebo ***explicitní***, a určuje, jestli se vyžaduje explicitní přetypování. Například převod z typu `int` na typ `long` je implicitní, takže výrazy typu `int` lze implicitně považovat za typ `long`. Opačné převod z typu `long` na typ `int`, je explicitní a proto se vyžaduje explicitní přetypování.

```csharp
int a = 123;
long b = a;         // implicit conversion from int to long
int c = (int) b;    // explicit conversion from long to int
```

Některé převody jsou definovány jazykem. Programy můžou také definovat své vlastní převody ([uživatelem definované převody](conversions.md#user-defined-conversions)).

## <a name="implicit-conversions"></a>Implicitní převody

Následující převody jsou klasifikovány jako implicitní převody:

*  Převody identity
*  Implicitní číselné převody
*  Výčet implicitní převody.
*  Implicitní převod s možnou hodnotou Null
*  Literál null převody
*  Odkaz na implicitní převody
*  Zabalení převody
*  Implicitní převody na dynamický
*  Konstantní výraz implicitní převody
*  Uživatelem definované implicitní převody
*  Převody anonymní funkce
*  Převody skupiny – metoda

Implicitní převod může dojít v různých situacích, včetně volání členské funkce ([kompilace kontrolu dynamické přetížení](expressions.md#compile-time-checking-of-dynamic-overload-resolution)), výrazy přetypování ([výrazy přetypování](expressions.md#cast-expressions)), a přiřazení ([operátory přiřazení](expressions.md#assignment-operators)).

Předdefinované implicitních převodů proběhnout úspěšně a nikdy nezpůsobí výjimky, která je vyvolána. Správně navržená uživatelem definované implicitní převody by měly vykazovat také tyto vlastnosti.

Pro účely převod typy `object` a `dynamic` jsou považovány za ekvivalentní.

Ale dynamické převody ([implicitních převodů dynamické](conversions.md#implicit-dynamic-conversions) a [explicitních převodů dynamické](conversions.md#explicit-dynamic-conversions)) se vztahují pouze na výrazy typu `dynamic` ([dynamického typu](types.md#the-dynamic-type)).

### <a name="identity-conversion"></a>Převod identity

Konverzi identity převede na stejný typ z libovolného typu. Tak, že entita, která už má požadovaný typ může být říká, že lze převést na daný typ existuje tento převod.

*  Protože objektu a dynamicky se považují za ekvivalentní je konverzi identity mezi `object` a `dynamic`a mezi sestavené typy, které jsou stejné při nahrazení všech výskytů `dynamic` s `object`.

### <a name="implicit-numeric-conversions"></a>Implicitní číselné převody

Implicitní číselné převody jsou:

*  Z `sbyte` k `short`, `int`, `long`, `float`, `double`, nebo `decimal`.
*  Z `byte` k `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `float`, `double`, nebo `decimal`.
*  Z `short` k `int`, `long`, `float`, `double`, nebo `decimal`.
*  Z `ushort` k `int`, `uint`, `long`, `ulong`, `float`, `double`, nebo `decimal`.
*  Z `int` k `long`, `float`, `double`, nebo `decimal`.
*  Z `uint` k `long`, `ulong`, `float`, `double`, nebo `decimal`.
*  Z `long` k `float`, `double`, nebo `decimal`.
*  Z `ulong` k `float`, `double`, nebo `decimal`.
*  Z `char` k `ushort`, `int`, `uint`, `long`, `ulong`, `float`, `double`, nebo `decimal`.
*  Z `float` k `double`.

Převody z `int`, `uint`, `long`, nebo `ulong` k `float` a z `long` nebo `ulong` k `double` může způsobit ztrátu přesnosti, ale nikdy příčinou ke ztrátě velikosti. Jiných implicitních číselných převodů nikdy ztraceny žádné informace.

Neexistují žádné implicitní převody na `char` tak hodnoty celočíselných typů nejde automaticky převést na typ `char` typu.

### <a name="implicit-enumeration-conversions"></a>Výčet implicitní převody

Povoluje výčet implicitní převod *decimal_integer_literal* `0` má být převeden na jakýkoli *enum_type* a k libovolnému *nullable_type* jehož Základní typ je *enum_type*. V druhém případě je vyhodnocen převod převedením na základní *enum_type* a balení výsledek ([typy připouštějící hodnotu Null](types.md#nullable-types)).

### <a name="implicit-interpolated-string-conversions"></a>Interpolované řetězce implicitní převody

Implicitní interpolované řetězce převod povolení *interpolated_string_expression* ([interpolovaných řetězců](expressions.md#interpolated-strings)) má být převeden na `System.IFormattable` nebo `System.FormattableString` (která implementuje `System.IFormattable`).

Při použití tohoto převodu řetězcové hodnoty se skládá z interpolovaném řetězci. Místo toho instance `System.FormattableString` je vytvořen, jak je uvedeno v [interpolovaných řetězců](expressions.md#interpolated-strings).

### <a name="implicit-nullable-conversions"></a>Implicitní převod s možnou hodnotou Null

Předdefinované implicitních převodů, které pracují s typy hodnot neumožňující hodnotu lze také s formuláři s možnou hodnotou NULL z těchto typů. Pro každý z předdefinovaných implicitní identity a číselných převodů, které provádějí převod z typu hodnotu Null `S` na typ hodnoty Null `T`, existují následující implicitní převody s možnou hodnotou NULL:

*  Implicitní převod z `S?` k `T?`.
*  Implicitní převod z `S` k `T?`.

Vyhodnocení implicitní převod s možnou hodnotou Null založené na základní převod z `S` k `T` probíhá následujícím způsobem:

*  Pokud je s možnou hodnotou Null převod ze `S?` k `T?`:
    * Pokud zdrojová hodnota má hodnotu null (`HasValue` vlastnost má hodnotu false), výsledkem je hodnota null typu `T?`.
    * V opačném případě se převod vyhodnotí jako rozbalení z `S?` k `S`následovaný základní převod z `S` k `T`a po něm zabalení ([typy připouštějící hodnotu Null](types.md#nullable-types)) z `T` k `T?`.

*  Při převodu s možnou hodnotou NULL z `S` k `T?`, převod se vyhodnotí jako základní převod z `S` k `T` za nímž následuje zabalení z `T` k `T?`.

### <a name="null-literal-conversions"></a>Literál null převody

Implicitní převod `null` literál na libovolný typ s možnou hodnotou Null. Tento převod vytvoří hodnotu null ([typy připouštějící hodnotu Null](types.md#nullable-types)) daného typu s možnou hodnotou Null.

### <a name="implicit-reference-conversions"></a>Odkaz na implicitní převody

Odkaz na implicitní převody jsou:

*  Z libovolného *reference_type* k `object` a `dynamic`.
*  Z libovolného *class_type* `S` k libovolnému *class_type* `T`, k dispozici `S` je odvozen z `T`.
*  Z libovolného *class_type* `S` k libovolnému *interface_type* `T`, k dispozici `S` implementuje `T`.
*  Z libovolného *interface_type* `S` k libovolnému *interface_type* `T`, k dispozici `S` je odvozen z `T`.
*  Z *array_type* `S` s typem elementu `SE` do *array_type* `T` s typem elementu `TE`, pokud jsou splněny všechny z následujících akcí:
    * `S` a `T` se liší pouze v typu elementu. Jinými slovy `S` a `T` mít stejný počet rozměrů.
    * Obě `SE` a `TE` jsou *reference_type*s.
    * Existuje implicitní referenční převod z `SE` k `TE`.
*  Z libovolného *array_type* k `System.Array` a implementuje rozhraní.
*  Jednorozměrné pole typu `S[]` k `System.Collections.Generic.IList<T>` a jeho základní rozhraní za předpokladu, že je implicitní identity nebo odkaz na převod z `S` k `T`.
*  Z libovolného *delegate_type* k `System.Delegate` a implementuje rozhraní.
*  Z literál s hodnotou null k libovolnému *reference_type*.
*  Z libovolného *reference_type* k *reference_type* `T` Pokud má implicitní identity nebo odkaz na převod na *reference_type* `T0` a `T0` má konverzi identity k `T`.
*  Z libovolného *reference_type* na typ rozhraní nebo delegát `T` Pokud má implicitní převod identity nebo odkaz na typ rozhraní nebo delegát `T0` a `T0` je odchylka převoditelné ([ Převod variance](interfaces.md#variance-conversion)) k `T`.
*  Implicitní převody zahrnující parametry typu, které jsou známé jako referenční typy. Zobrazit [implicitních převodů zahrnující parametry typu](conversions.md#implicit-conversions-involving-type-parameters) podrobné informace o implicitní převody zahrnující parametry typu.

Odkaz na implicitní převody jsou tyto převody mezi *reference_type*, které můžete prověřené vždy úspěšné a proto vyžaduje žádné kontroly za běhu.

Převody odkazů, implicitní nebo explicitní, nikdy nezmění referenční identity objektu převodu. Jinými slovy zatímco referenční převod může změnit typ odkazu, se nikdy nemění typem nebo hodnotou objektu, který se odkazuje.

### <a name="boxing-conversions"></a>Zabalení převody

Umožňuje převod na uzavřené určení *value_type* má být implicitně převeden na typ odkazu. Existuje převod na uzavřené určení z libovolného *non_nullable_value_type* k `object` a `dynamic`do `System.ValueType` a k libovolnému *interface_type* implementované *non_ nullable_value_type*. Kromě *enum_type* lze převést na typ `System.Enum`.

Existuje převod na uzavřené určení z *nullable_type* na typ odkazu, pokud a pouze v případě, že převod na uzavřené určení existuje ze základního *non_nullable_value_type* na typ odkazu.

Typ hodnoty je zabalení převod na typ rozhraní `I` se jeho zabalení převod na typ rozhraní `I0` a `I0` má konverzi identity k `I`.

Hodnotový typ má zabalení převod na typ rozhraní `I` Pokud má zabalení převod na typ rozhraní nebo delegát `I0` a `I0` je odchylka převoditelné ([Variance převod](interfaces.md#variance-conversion)) k `I`.

Zabalení hodnotu *non_nullable_value_type* se skládá z přidělování instancí objektu a kopírování *value_type* hodnoty do této instance. Jde použít boxing strukturu na typ `System.ValueType`, protože to je základní třída pro všechny struktury ([dědičnosti](structs.md#inheritance)).

Zabalení hodnotu *nullable_type* probíhá následujícím způsobem:

*  Pokud zdrojová hodnota má hodnotu null (`HasValue` vlastnost má hodnotu false), výsledek je nulový odkaz cílového typu.
*  V opačném případě výsledkem je odkaz na zabalený `T` vytvářené rozbalení a zabalení zdrojovou hodnotou.

Zabalení převody jsou popsány dále v [zabalení převody](types.md#boxing-conversions).

### <a name="implicit-dynamic-conversions"></a>Implicitní převody na dynamický

Implicitní převod dynamických existuje z výrazu typu `dynamic` na libovolný typ `T`. Převod je vázán dynamicky ([dynamické vazby](expressions.md#dynamic-binding)), což znamená, že implicitní převod bude hledat v době běhu z typu za běhu výraz, který má `T`. Pokud se nenajde žádný převod, je vyvolána výjimka za běhu.

Všimněte si, že tento implicitní převod zdánlivě porušuje doporučení na začátku [implicitních převodů](conversions.md#implicit-conversions) implicitní převod by nikdy nezpůsobí výjimku. Ale není převod samostatně, ale *hledání* převodu, která způsobí výjimku. Riziko výjimek za běhu je spojená s používáním dynamické vazby. Pokud dynamické vazby převod není žádoucí, výraz může být nejprve převeden na `object`a potom do požadovaného typu.

Následující příklad ukazuje dynamické implicitní převody:

```csharp
object o  = "object"
dynamic d = "dynamic";

string s1 = o; // Fails at compile-time -- no conversion exists
string s2 = d; // Compiles and succeeds at run-time
int i     = d; // Compiles but fails at run-time -- no conversion exists
```

Přiřazení k `s2` a `i` i využívat implicitních převodů dynamické, kde je vazba operace pozastaveno do za běhu. V době běhu, jsou požadována implicitní převody z typu za běhu `d`  --  `string` --do cílového typu. Převod se nachází na `string` , ale nikoli k `int`.

### <a name="implicit-constant-expression-conversions"></a>Konstantní výraz implicitní převody

Konverzi implicitní konstantní výraz povoluje následující převody:

*  A *constant_expression* ([konstantní výrazy](expressions.md#constant-expressions)) typu `int` lze převést na typ `sbyte`, `byte`, `short`, `ushort`, `uint`, nebo `ulong`, zadaná hodnota *constant_expression* je v rozsahu cílového typu.
*  A *constant_expression* typu `long` lze převést na typ `ulong`, zadaná hodnota *constant_expression* není záporná.

### <a name="implicit-conversions-involving-type-parameters"></a>Implicitní převody zahrnující parametry typu

Existují následující implicitní převody pro daný typ parametru `T`:

*  Z `T` se svou základní třídou efektivní `C`, z `T` na všechny základní třídy `C`a z `T` na libovolném rozhraní implementované `C`. AT za běhu, pokud `T` je typ hodnoty, je proveden převod, protože převod na uzavřené určení. V opačném případě je proveden převod jako implicitní převod odkazu nebo převod identity.
*  Z `T` k typu rozhraní `I` v `T`nastaví efektivní rozhraní a z `T` všechny základní rozhraní `I`. AT za běhu, pokud `T` je typ hodnoty, je proveden převod, protože převod na uzavřené určení. V opačném případě je proveden převod jako implicitní převod odkazu nebo převod identity.
*  Z `T` parametru typu `U`, k dispozici `T` závisí na `U` ([omezení parametru typu](classes.md#type-parameter-constraints)). AT za běhu, pokud `U` je typ hodnoty, pak `T` a `U` nutně jsou stejného typu a převod provést. Jinak, pokud `T` je typ hodnoty, je proveden převod, protože převod na uzavřené určení. V opačném případě je proveden převod jako implicitní převod odkazu nebo převod identity.
*  Z literál s hodnotou null pro `T`, k dispozici `T` je znám jako typ odkazu.
*  Z `T` na typ odkazu `I` Pokud má implicitní převod na typ odkazu `S0` a `S0` má konverzi identity k `S`. V době běhu provádí převod stejným způsobem jako převod `S0`.
*  Z `T` k typu rozhraní `I` Pokud má implicitní převod na typ rozhraní nebo delegát `I0` a `I0` je odchylka převoditelné na `I` ([Variance převod](interfaces.md#variance-conversion) ). AT za běhu, pokud `T` je typ hodnoty, je proveden převod, protože převod na uzavřené určení. V opačném případě je proveden převod jako implicitní převod odkazu nebo převod identity.

Pokud `T` je znám jako typ odkazu ([omezení parametru typu](classes.md#type-parameter-constraints)), všechny výše uvedené převody jsou klasifikovány jako odkaz na implicitní převody ([odkaz na implicitní převody](conversions.md#implicit-reference-conversions)). Pokud `T` není známé jako typ odkazu, převody výše jsou klasifikovány jako zabalení převody ([zabalení převody](conversions.md#boxing-conversions)).

### <a name="user-defined-implicit-conversions"></a>Uživatelem definované implicitní převody

Implicitní převod definovaný uživatelem se skládá z volitelné standardní implicitní převod, za nímž následuje spuštění uživatelem definované implicitní převod operátoru, za nímž následuje jiný volitelné standardní implicitní převod. Přesná pravidla za vaše rozhodnutí vyzkoušet uživatelem definované implicitní převody jsou popsané v [zpracování uživatelem definované implicitní převody](conversions.md#processing-of-user-defined-implicit-conversions).

### <a name="anonymous-function-conversions-and-method-group-conversions"></a>Anonymní funkce převody a převody skupiny – metoda

Anonymní funkce a metody skupiny nemají typy samy o sobě, ale může být implicitně převeden na delegáta typy nebo typy stromu výrazu. Anonymní funkce převody jsou popsány podrobněji [převody anonymní funkce](conversions.md#anonymous-function-conversions) a metoda převody skupin v [Metoda skupiny převody](conversions.md#method-group-conversions).

## <a name="explicit-conversions"></a>Explicitní převody

Následující převody jsou klasifikovány jako explicitní převody:

*  Všechny implicitní převody.
*  Explicitní číselné převody.
*  Výčet explicitní převody.
*  Explicitní převody s možnou hodnotou Null.
*  Odkaz na explicitní převody.
*  Explicitní rozhraní převody.
*  Rozbalení převody.
*  Explicitní převody na dynamický
*  Uživatelem definované explicitní převody.

Explicitní převody může dojít v výrazy přetypování ([výrazy přetypování](expressions.md#cast-expressions)).

Explicitní převody sada obsahuje všechny implicitní převody. To znamená, že jsou povolena výrazy přetypování redundantní.

Explicitní převody, které nejsou implicitní převody jsou převody, které nelze prověřené vždy úspěšné, převody, které se ví, případně dojít ke ztrátě informací a převody mezi doménami dostatečně neliší na explicitní hodnoty typů zápis.

### <a name="explicit-numeric-conversions"></a>Explicitní číselné převody

Explicitní číselné převody jsou převody z *numeric_type* do jiného *numeric_type* pro kterou implicitní převod čísla ([implicitních číselných převodů](conversions.md#implicit-numeric-conversions)) ještě neexistuje:

*  Z `sbyte` k `byte`, `ushort`, `uint`, `ulong`, nebo `char`.
*  Z `byte` k `sbyte` a `char`.
*  Z `short` k `sbyte`, `byte`, `ushort`, `uint`, `ulong`, nebo `char`.
*  Z `ushort` k `sbyte`, `byte`, `short`, nebo `char`.
*  Z `int` k `sbyte`, `byte`, `short`, `ushort`, `uint`, `ulong`, nebo `char`.
*  Z `uint` k `sbyte`, `byte`, `short`, `ushort`, `int`, nebo `char`.
*  Z `long` k `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `ulong`, nebo `char`.
*  Z `ulong` k `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, nebo `char`.
*  Z `char` k `sbyte`, `byte`, nebo `short`.
*  Z `float` k `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, nebo `decimal`.
*  Z `double` k `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, nebo `decimal`.
*  Z `decimal` k `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, nebo `double`.

Protože explicitní převody zahrnují všechny implicitní a explicitní číselné převody, je vždy možné převést z některého *numeric_type* u kteréhokoli jiného *numeric_type* pomocí (výraz přetypování [Výrazy přetypování](expressions.md#cast-expressions)).

Explicitní číselné převody potenciálně dojít ke ztrátě informací nebo pravděpodobně způsobí vyvolání výjimky. Explicitní číselný převod zpracováván následujícím způsobem:

*  Pro převod z celočíselného typu na jiný celočíselný typ, zpracování závisí na kontextu kontroly přetečení ([operátory zaškrtnuto a nezaškrtnuto](expressions.md#the-checked-and-unchecked-operators)) v desetinný převod umístit:
    * V `checked` kontextu, převod je úspěšný, pokud je hodnota zdrojového operandu v rozsahu cílového typu, ale vyvolá výjimku `System.OverflowException` Pokud hodnota zdrojového operandu je mimo rozsah cílového typu.
    * V `unchecked` kontextu, převod vždy úspěšná a probíhá následujícím způsobem.
        * Pokud typ zdroje je větší než cílový typ, pak je rozdělená do zdrojové hodnoty se zahodí jeho "navíc" nejvýznamnější bity. Výsledek je pak považován za hodnotu cílového typu.
        * Pokud typ zdroje je menší než cílový typ, pak zdrojová hodnota je rozšířena o znaménko nebo nulou tak, aby se stejnou velikostí jako typ cíle. Rozšířením znaménka se používá, pokud typ zdroje je podepsaný; nulové rozšíření se používá, pokud se zdrojový typ není podepsaný. Výsledek je pak považován za hodnotu cílového typu.
        * Pokud se stejnou velikostí jako cílový typ je typ zdrojového, zdrojová hodnota je považován za hodnotu cílového typu.
*  Pro konverzi `decimal` na celočíselný typ, zdrojová hodnota zaokrouhlena směrem k nule na nejbližší celočíselnou hodnotu a bude toto celé číslo výsledku převodu. Pokud je výsledný celočíselné hodnoty mimo rozsah cílového typu `System.OverflowException` je vyvolána výjimka.
*  Pro konverzi `float` nebo `double` na celočíselný typ, zpracování závisí na kontextu kontroly přetečení ([operátory zaškrtnuto a nezaškrtnuto](expressions.md#the-checked-and-unchecked-operators)) v desetinný převod umístit:
    * V `checked` kontextu, převod probíhá následujícím způsobem:
        * Pokud je hodnota operandu NaN nebo nekonečno, `System.OverflowException` je vyvolána výjimka.
        * V opačném případě zdrojového operandu je zaokrouhlena směrem k nule na nejbližší celočíselnou hodnotu. Pokud je toto celé číslo v rozsahu cílového typu je tato hodnota výsledku převodu.
        * V opačném případě `System.OverflowException` je vyvolána výjimka.
    * V `unchecked` kontextu, převod vždy úspěšná a probíhá následujícím způsobem.
        * Pokud je hodnota operandu NaN nebo nekonečno, je výsledkem převodu neurčené hodnota cílového typu.
        * V opačném případě zdrojového operandu je zaokrouhlena směrem k nule na nejbližší celočíselnou hodnotu. Pokud je toto celé číslo v rozsahu cílového typu je tato hodnota výsledku převodu.
        * V opačném případě výsledkem převodu je neurčené hodnota cílového typu.
*  Pro převod z `double` k `float`, `double` hodnota je zaokrouhlená na nejbližší `float` hodnotu. Pokud `double` hodnota je příliš malá, aby reprezentovala `float`, výsledek bude kladné nula nebo záporná nula. Pokud `double` hodnota je příliš velký, aby reprezentovala `float`, výsledkem bude nekonečno kladné nebo záporné nekonečno. Pokud `double` je hodnota typu NaN, vrácená hodnota je také NaN.
*  Pro převod z `float` nebo `double` k `decimal`, zdrojová hodnota je převedena na `decimal` vyjádření a zaokrouhlí na nejbližší číslo po 28 desetinné čárky v případě potřeby ([typem decimal](types.md#the-decimal-type)). Pokud zdrojová hodnota je příliš malá, aby reprezentovala `decimal`, výsledkem bude nule. Pokud zdrojová hodnota NaN, nekonečno, nebo příliš velký, aby reprezentovala `decimal`, `System.OverflowException` je vyvolána výjimka.
*  Pro převod z `decimal` k `float` nebo `double`, `decimal` hodnota je zaokrouhlená na nejbližší `double` nebo `float` hodnotu. Přestože tento převod může dojít ke ztrátě přesnosti, nikdy způsobí vyvolání výjimky.

### <a name="explicit-enumeration-conversions"></a>Výčet explicitní převody

Výčet explicitní převody jsou:

*  Z `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, nebo `decimal` do jakékoli *enum_type*.
*  Z libovolného *enum_type* k `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, nebo `decimal`.
*  Z libovolného *enum_type* u kteréhokoli jiného *enum_type*.

Výčet explicitní převod mezi dvěma typy je zpracován považuje všechny zúčastněné *enum_type* jako základní typ, který *enum_type*a pak provádí implicitní nebo explicitní číselný převod mezi typy výsledný. Mějme například *enum_type* `E` s a nadřazený typ `int`, převod z `E` k `byte` zpracovávány jako explicitní číselný převod ([explicitní číselné převody](conversions.md#explicit-numeric-conversions)) z `int` k `byte`a převod z `byte` k `E` zpracovávány jako implicitní převod čísla ([implicitních číselných převodů](conversions.md#implicit-numeric-conversions)) z `byte` k `int`.

### <a name="explicit-nullable-conversions"></a>Explicitní převody s možnou hodnotou Null

***Explicitní převody s možnou hodnotou Null*** povolení předdefinované explicitních převodů, které pracují s typy hodnot neumožňující hodnotu lze použít také s možnou hodnotou Null formuláře z těchto typů. Pro každou z předdefinovaných explicitních převodů, které provádějí převod z typu hodnotu Null `S` na typ hodnoty Null `T` ([Identity převod](conversions.md#identity-conversion), [implicitních číselných převodů](conversions.md#implicit-numeric-conversions), [Výčet implicitní převody](conversions.md#implicit-enumeration-conversions), [explicitních číselných převodů](conversions.md#explicit-numeric-conversions), a [výčet explicitní převody](conversions.md#explicit-enumeration-conversions)), následující Existují převody s možnou hodnotou NULL:

*  Explicitní převod z `S?` k `T?`.
*  Explicitní převod z `S` k `T?`.
*  Explicitní převod z `S?` k `T`.

Hodnocení s možnou hodnotou Null převodu založené na základní převod z `S` k `T` probíhá následujícím způsobem:

*  Pokud je s možnou hodnotou Null převod ze `S?` k `T?`:
    * Pokud zdrojová hodnota má hodnotu null (`HasValue` vlastnost má hodnotu false), výsledkem je hodnota null typu `T?`.
    * V opačném případě se převod vyhodnotí jako rozbalení z `S?` k `S`následovaný základní převod z `S` k `T`následovaný zabalení z `T` k `T?`.
*  Při převodu s možnou hodnotou NULL z `S` k `T?`, převod se vyhodnotí jako základní převod z `S` k `T` za nímž následuje zabalení z `T` k `T?`.
*  Při převodu s možnou hodnotou NULL z `S?` k `T`, převod se vyhodnotí jako rozbalení z `S?` k `S` následovaný základní převod z `S` k `T`.

Všimněte si, že pokus o rozbalení povolenou vyvolá výjimku, pokud je hodnota `null`.

### <a name="explicit-reference-conversions"></a>Odkaz na explicitní převody

Odkaz na explicitní převody jsou:

*  Z `object` a `dynamic` u kteréhokoli jiného *reference_type*.
*  Z libovolného *class_type* `S` k libovolnému *class_type* `T`, k dispozici `S` je základní třídou `T`.
*  Z libovolného *class_type* `S` k libovolnému *interface_type* `T`, k dispozici `S` není zapečetěná a k dispozici `S` neimplementuje `T`.
*  Z libovolného *interface_type* `S` k libovolnému *class_type* `T`, k dispozici `T` není zapečetěná, nebo k dispozici `T` implementuje `S`.
*  Z libovolného *interface_type* `S` k libovolnému *interface_type* `T`, k dispozici `S` není odvozen od `T`.
*  Z *array_type* `S` s typem elementu `SE` do *array_type* `T` s typem elementu `TE`, pokud jsou splněny všechny z následujících akcí:
    * `S` a `T` se liší pouze v typu elementu. Jinými slovy `S` a `T` mít stejný počet rozměrů.
    * Obě `SE` a `TE` jsou *reference_type*s.
    * Explicitní odkaz na převod `SE` k `TE`.
*  Z `System.Array` a rozhraní implementuje k libovolnému *array_type*.
*  Jednorozměrné pole typu `S[]` k `System.Collections.Generic.IList<T>` a jeho základní rozhraní za předpokladu, že je převod explicitní odkaz `S` k `T`.
*  Z `System.Collections.Generic.IList<S>` a její základní rozhraní pro typ jednorozměrné pole `T[]`za předpokladu, že existuje explicitní převod identity nebo odkaz z `S` k `T`.
*  Z `System.Delegate` a rozhraní implementuje k libovolnému *delegate_type*.
*  Typ odkazu na typ odkazu z `T` se jeho konverzi explicitní odkaz na typ odkazu `T0` a `T0` má konverzi identity `T`.
*  Z typu odkazu na typ rozhraní nebo delegát `T` Pokud má konverzi explicitní odkaz na typ rozhraní nebo delegát `T0` a buď `T0` je odchylka převoditelné na `T` nebo `T` je Variance-lze převést na typ `T0` ([Variance převod](interfaces.md#variance-conversion)).
*  Z `D<S1...Sn>` k `D<T1...Tn>` kde `D<X1...Xn>` je typem obecného delegátu `D<S1...Sn>` není kompatibilní s nebo stejný jako `D<T1...Tn>`a pro každý parametr typu `Xi` z `D` obsahuje následující:
    * Pokud `Xi` je neutrální, pak `Si` je stejný jako `Ti`.
    * Pokud `Xi` je kovariantní, pak je implicitní nebo explicitní identity nebo odkaz na převod pro z `Si` k `Ti`.
    * Pokud `Xi` je kontravariantní, pak `Si` a `Ti` jsou buď stejné nebo oba typy odkazů.
*  Explicitní převody zahrnující parametry typu, které jsou známé jako referenční typy. Podrobné informace o explicitních převodů zahrnující parametry typu, najdete v článku [explicitních převodů zahrnující parametry typu](conversions.md#explicit-conversions-involving-type-parameters).

Odkaz na explicitní převody jsou tyto převody mezi – typy odkazů, které vyžadují kontroly za běhu k zajištění, že jsou správné.

Pro konverzi explicitní odkaz na úspěšné v době běhu musí být hodnota zdrojového operandu `null`, nebo skutečný typ objekt odkazovaný zadaným parametrem zdrojový operand musí být typ, který lze převést na typ cílového implicitní odkazem převod ([odkaz na implicitní převody](conversions.md#implicit-reference-conversions)) nebo převod na uzavřené určení ([zabalení převody](conversions.md#boxing-conversions)). Pokud se nezdaří konverzi explicitní odkaz `System.InvalidCastException` je vyvolána výjimka.

Převody odkazů, implicitní nebo explicitní, nikdy nezmění referenční identity objektu převodu. Jinými slovy zatímco referenční převod může změnit typ odkazu, se nikdy nemění typem nebo hodnotou objektu, který se odkazuje.

### <a name="unboxing-conversions"></a>Rozbalení převody

Unboxingového převodu povoluje má být explicitně převeden na typ odkazu *value_type*. Existuje unboxingového převodu z typů `object`, `dynamic` a `System.ValueType` k libovolnému *non_nullable_value_type*a z jakéhokoli *interface_type* k libovolnému *non_ nullable_value_type* , který implementuje *interface_type*. Dále zadejte `System.Enum` může být bez unboxingu na jakýkoli *enum_type*.

Existuje unboxingového převodu z typu odkazu na *nullable_type* pokud existuje unboxingového převodu z typu odkazu k podkladovým *non_nullable_value_type* z  *nullable_type*.

Typ hodnoty `S` má unboxingového převodu z typu rozhraní `I` se jeho unboxingového převodu z typu rozhraní `I0` a `I0` má konverzi identity k `I`.

Typ hodnoty `S` má unboxingového převodu z typu rozhraní `I` Pokud má unboxingového převodu z typu rozhraní nebo delegát `I0` a buď `I0` je odchylka převoditelné na `I` nebo `I`je odchylka převoditelné na `I0` ([Variance převod](interfaces.md#variance-conversion)).

Operace rozbalení se skládá z nejdřív zkontrolovali, že se instance objektu je zabalený hodnotu daný *value_type*a potom kopírování hodnoty z instance. Odkaz s hodnotou null pro rozbalení *nullable_type* vytvoří hodnotu null *nullable_type*. Struktura může být z typ bez unboxingu `System.ValueType`, protože to je základní třída pro všechny struktury ([dědičnosti](structs.md#inheritance)).

Rozbalení převody jsou popsány dále v [rozbalení převody](types.md#unboxing-conversions).

### <a name="explicit-dynamic-conversions"></a>Explicitní převody na dynamický

Existuje explicitní převod dynamických z výrazu typu `dynamic` na libovolný typ `T`. Převod je vázán dynamicky ([dynamické vazby](expressions.md#dynamic-binding)), což znamená, že explicitní převod bude hledat v době běhu z typu za běhu výraz, který má `T`. Pokud se nenajde žádný převod, je vyvolána výjimka za běhu.

Pokud dynamické vazby převod není žádoucí, výraz může být nejprve převeden na `object`a potom do požadovaného typu.

Předpokládejme, že je definován následující třídy:
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

Následující příklad ukazuje explicitní převody dynamické:
```csharp
object o  = "1";
dynamic d = "2";

var c1 = (C)o; // Compiles, but explicit reference conversion fails
var c2 = (C)d; // Compiles and user defined conversion succeeds
```

Nejlepší převod `o` k `C` se nachází v době kompilace bude konverzi explicitní odkaz. To se nezdaří, v době běhu, protože `"1"` není ve skutečnosti `C`. Převod `d` k `C` však jako explicitní převod dynamického, je pozastavený, aby za běhu, ve kterém uživatelem definovaný převod z typu za běhu `d`  --  `string` – na `C` nenajde, a proběhne úspěšně.

### <a name="explicit-conversions-involving-type-parameters"></a>Explicitní převody zahrnující parametry typu

Pro daný typ parametru existují následující explicitní převody `T`:

*  Ze základní třídy efektivní `C` z `T` k `T` a žádné základní třídy `C` k `T`. AT za běhu, pokud `T` je typ hodnoty, je proveden převod, protože unboxingového převodu. V opačném případě je převod proveden, protože explicitní odkaz ani převod identity.
*  Z libovolného typu rozhraní na `T`. AT za běhu, pokud `T` je typ hodnoty, je proveden převod, protože unboxingového převodu. V opačném případě je převod proveden, protože explicitní odkaz ani převod identity.
*  Z `T` k libovolnému *interface_type* `I` Pokud již není implicitní převod z `T` k `I`. AT za běhu, pokud `T` je typ hodnoty, je proveden převod, protože za nímž následuje konverzi explicitní odkaz převod na uzavřené určení. V opačném případě je převod proveden, protože explicitní odkaz ani převod identity.
*  Z parametru typu `U` k `T`, k dispozici `T` závisí na `U` ([omezení parametru typu](classes.md#type-parameter-constraints)). AT za běhu, pokud `U` je typ hodnoty, pak `T` a `U` nutně jsou stejného typu a převod provést. Jinak, pokud `T` je typ hodnoty, je proveden převod, protože unboxingového převodu. V opačném případě je převod proveden, protože explicitní odkaz ani převod identity.

Pokud `T` je známé jako typ odkazu, převody výše jsou všechny klasifikované jako odkaz na explicitní převody ([odkaz na explicitní převody](conversions.md#explicit-reference-conversions)). Pokud `T` není známé jako typ odkazu, převody výše jsou klasifikovány jako rozbalení převody ([rozbalení převody](conversions.md#unboxing-conversions)).

Výše uvedených pravidel není povoleno přímé explicitní převod z parametrem bez omezení typu na typ jiného typu než rozhraní, který může být překvapivé. Důvod pro toto pravidlo je zabránit nejasnostem a ujistěte se, sémantika tyto převody vymazat. Předpokládejme například následující deklaraci:
```csharp
class X<T>
{
    public static long F(T t) {
        return (long)t;                // Error 
    }
}
```

Pokud s přímým přístupem explicitního převodu `t` k `int` byly převody povoleny, snadno očekávat, který `X<int>.F(7)` vracel `7L`. Však stejně, protože standardních číselných převodů považují pouze pokud jsou známé typy jako číselné během vazby. Pokud chcete mít sémantiku clear výše uvedeném příkladu musí být zapsán:
```csharp
class X<T>
{
    public static long F(T t) {
        return (long)(object)t;        // Ok, but will only work when T is long
    }
}
```

Tento kód se zkompiluje teď ale provádění `X<int>.F(7)` by pak vyvolat výjimku v době běhu, protože zabalené `int` přímo se nedá převést `long`.

### <a name="user-defined-explicit-conversions"></a>Uživatelem definované explicitní převody

Uživatelem definované explicitní převod se skládá z volitelné standardní explicitní převod, za nímž následuje spuštění uživatelem definované implicitní nebo explicitní převod operátoru, za nímž následuje jiný volitelné standardní explicitní převod. Přesná pravidla za vaše rozhodnutí vyzkoušet uživatelem definované explicitní převody jsou popsané v [zpracování uživatelem definované explicitní převody](conversions.md#processing-of-user-defined-explicit-conversions).

## <a name="standard-conversions"></a>Standardní převody

Standardní převody jsou tyto předem definované převody, které může být součástí uživatelem definovaný převod.

### <a name="standard-implicit-conversions"></a>Standardní implicitní převody

Následující implicitní převody jsou klasifikovány jako standardní implicitní převody:

*  Převody identity ([Identity převod](conversions.md#identity-conversion))
*  Implicitní číselné převody ([implicitních číselných převodů](conversions.md#implicit-numeric-conversions))
*  Implicitní převod s možnou hodnotou Null ([implicitní převody typu s možnou hodnotou Null](conversions.md#implicit-nullable-conversions))
*  Odkaz na implicitní převody ([odkaz na implicitní převody](conversions.md#implicit-reference-conversions))
*  Zabalení převody ([zabalení převody](conversions.md#boxing-conversions))
*  Konstantní výraz implicitní převody ([implicitních převodů dynamické](conversions.md#implicit-dynamic-conversions))
*  Implicitní převody zahrnující parametry typu ([implicitních převodů zahrnující parametry typu](conversions.md#implicit-conversions-involving-type-parameters))

Standardní implicitní převody výslovně vyloučit implicitní převody definované uživatelem.

### <a name="standard-explicit-conversions"></a>Standardní explicitní převody

Standardní explicitní převody jsou všechny standardní implicitní převody plus podmnožinu explicitních převodů, pro které existuje opačné standardní implicitní převod. Jinými slovy, pokud standardní implicitní existuje převod z typu `A` na typ `B`, pak existuje standardní explicitní převod z typu `A` na typ `B` z typu `B` na typ `A`.

## <a name="user-defined-conversions"></a>Uživatelem definované převody

C# umožňuje rozšířen o předdefinované implicitní a explicitní převody ***uživatelem definované převody***. Uživatelem definované převody jsou zavedené službou deklarace operátorů převodu ([operátory převodu](classes.md#conversion-operators)) v typy třídy a struktury.

### <a name="permitted-user-defined-conversions"></a>Povolené uživatelem definované převody

C# umožňuje pouze určité uživatelem definované převody na deklarovat. Zejména není možné předefinovat již existující implicitní nebo explicitní převod.

Pro typ daného zdroje `S` a cílový typ `T`, pokud `S` nebo `T` jsou typy s možnou hodnotou Null, umožní `S0` a `T0` odkazovat na základní typy, jinak `S0` a `T0` jsou rovno `S` a `T` v uvedeném pořadí. Třídy nebo struktury je povolené pro deklaraci převod z typu zdrojové `S` s cílovým typem `T` pouze v případě, že jsou splněny všechny z následujících akcí:

*  `S0` a `T0` jsou různé typy.
*  Buď `S0` nebo `T0` je typ třídy nebo struktury, ve kterém probíhá deklarace operátoru.
*  Ani `S0` ani `T0` je *interface_type*.
*  S výjimkou uživatelem definované převody neexistuje převod z `S` k `T` nebo z `T` k `S`.

Omezení, které platí pro uživatelem definované převody jsou popsané dále v [operátory převodu](classes.md#conversion-operators).

### <a name="lifted-conversion-operators"></a>Operátory převodu zdvižené

Zadaný operátor uživatelsky definovaný převod, který převede hodnotu Null typu `S` na typ hodnoty Null `T`, ***zrušeno vs. operátor převodu*** existuje které převádí z `S?` k `T?`. Tohoto operátoru převodu zdvižené provádí rozbalení z `S?` k `S` za nímž následuje uživatelsky definovaný převod z `S` k `T` za nímž následuje zabalení z `T` k `T?`, s tím rozdílem, že s hodnotou null Vážíme si toho `S?` převáděn přímo s hodnotou null `T?`.

Operátor převodu zdvižené má stejné implicitní nebo explicitní zařazení jako jeho základní operátor uživatelsky definovaný převod. Termín "uživatelsky definovaný převod" platí pro použití uživatelsky definovaná a zrušeno vs. operátory převodu.

### <a name="evaluation-of-user-defined-conversions"></a>Hodnocení produktu uživatelem definované převody

Uživatelem definovaná konverze z jeho typu, volá se, převede hodnotu ***typ zdroje***, do jiného typu, volá se ***cílový typ***. Centra vyhodnocení uživatelsky definovaný převod na vyhledávání ***nejspecifičtější*** uživatelsky definovaný převod operátor pro konkrétní zdrojovými a cílovými typy. Toto rozhodnutí je rozdělená do několika kroků:

*  Najít sadu tříd a struktur, ze kterého budou považovat za uživatelsky definovaný převod operátorů. Tato sada obsahuje typ zdroje a jejích základních tříd a cílový typ a jejích základních tříd (s implicitní předpokladů, že pouze třídy a struktury můžete deklarovat operátory definované uživatelem, a zda máte typy bez třídy žádné základní třídy). Pro účely tohoto kroku, pokud je zdrojový nebo cílový typ *nullable_type*, jejich základní typ je místo toho použít.
*  Z této sady typů určující, které definované uživatelem a zrušeno vs. operátory převodu se dají použít. Pro operátor převodu fungovaly, musí být možné provést standardní převod ([standardní převody](conversions.md#standard-conversions)) od zdrojového typu na operand musí být typu operátor a je možné provést standardní převod z typu výsledek operátoru do cílového typu.
*  Ze sady použít operátory definované uživatelem, určující, které operátor je jednoznačně nejkonkrétnější. Obecně řečeno nejspecifičtější operátor je operátor, který je typem operandu "nejblíže" typ zdroje a jehož typ výsledku je "co nejblíž koncovým" cílového typu. Uživatelem definovaný převod operátory jsou upřednostňované nad operátory zdvižené převodu. Přesná pravidla pro stanovení nejspecifičtější operátor uživatelsky definovaný převod jsou definovány v následující části.

Po určení nejspecifičtější operátor uživatelsky definovaný převod aktuální provádění uživatelsky definovaný převod zahrnuje až tři kroky:

*  Standardní převod z typu zdroje na operand typu operátoru převodu uživatelem nebo zdvižené provádění nejprve v případě potřeby.
*  V dalším kroku volání operátoru převodu uživatelem nebo zdvižené k provedení převodu.
*  Nakonec pokud je to nutné, provádění standardní převod z typu výsledek operátoru převodu uživatelem nebo zdvižené na cílový typ.

Vyhodnocení uživatelsky definovaný převod nikdy zahrnuje více než jeden operátor uživatelem nebo zdvižené převodu. Jinými slovy, převod z typu `S` na typ `T` nikdy nejprve provede uživatelem definovaná konverze z `S` k `X` a následné provádění uživatelem definovaná konverze z `X` k `T`.

Přesné definice vyhodnocování uživatelsky definované explicitní nebo implicitní převody jsou uvedeny v následujících částech. Ujistěte se, definice pomocí následujících podmínek:

*  Pokud standardní implicitní převod ([standardní implicitní převody](conversions.md#standard-implicit-conversions)) existuje z typu `A` na typ `B`a pokud `A` ani `B` jsou *interface_type*s, pak `A` je označen jako ***vlastněný*** `B`, a `B` se říká, že ***zahrnovat*** `A`.
*  ***Nejvíce včetně typu*** sadu typů je jeden typ, který zahrnuje všechny typy v sadě. Pokud žádné jednoho typu zahrnuje všechny ostatní typy, sada nemá nejvíce včetně typ. V mnohem intuitivnější podmínky nejvíce včetně typ je typ "největší" v sadě – jeden typ, ke kterému všech ostatních typů lze implicitně převést.
*  ***Nejvíce zahrnuta typ*** sadu typů je jeden typ, který je zahrnut ve všech ostatních typů v sadě. Pokud žádný jediný typ je zahrnut ve všech ostatních typů, pak sada má již nejvíce zahrnuta typu. V mnohem intuitivnější podmínky nejvíce encompassed typ je typ "nejmenší" v sadě – jeden typ, který lze implicitně převést na všech ostatních typů.

### <a name="processing-of-user-defined-implicit-conversions"></a>Zpracování uživatelského implicitních převodů

Implicitní převod z typu uživatelem definované `S` na typ `T` zpracování následujícím způsobem:

*  Určete typy `S0` a `T0`. Pokud `S` nebo `T` jsou typy s možnou hodnotou Null, `S0` a `T0` se jejich základní typy, v opačném případě `S0` a `T0` rovnají `S` a `T` v uvedeném pořadí.
*  Najít sadu typů, `D`, ze kterých uživatelsky definovaný převod operátory se budou považovat za. Tato sada se skládá z `S0` (Pokud `S0` je třída nebo struktura), základní třídy `S0` (Pokud `S0` je třída), a `T0` (Pokud `T0` je třída nebo struktura).
*  Najít sadu operátory použitelné uživatelsky definovaná a zdvižené převodu `U`. Tato sada obsahuje operátory definované uživatelem a zdvižené implicitní převod deklaroval třídy nebo struktury v `D` převod z typu včetně `S` na typ vlastněný `T`. Pokud `U` je prázdný, převod není definován a dojde k chybě kompilace.
*  Najít co nejspecifičtější typ. zdroj `SX`, operátorů v `U`:
    * Pokud je libovolná z operátorů v `U` převést `S`, pak `SX` je `S`.
    * V opačném případě `SX` je nejvíce encompassed typ v kombinovanou sadu typů zdrojů operátorů v `U`. Pokud právě jeden nejvíce zahrnuta typ nebyl nalezen, pak převod je nejednoznačný a dojde k chybě kompilace.
*  Najít co nejspecifičtější typ. cílové `TX`, operátorů v `U`:
    * Pokud je libovolná z operátorů v `U` převést na `T`, pak `TX` je `T`.
    * V opačném případě `TX` je nejvíce včetně typu v kombinovanou sadu cílové typy operátorů v `U`. Pokud nelze najít největší včetně právě jeden typ, pak převod je nejednoznačný a dojde k chybě kompilace.
*  Najdete nejspecifičtější operátor převodu:
    * Pokud `U` obsahuje přesně jeden uživatelsky definovaný převod operátor, který převede z `SX` k `TX`, pak toto je nejspecifičtější operátoru převodu.
    * Jinak, pokud `U` obsahuje přesně jeden operátor zdvižené převodu, která převede z `SX` k `TX`, pak toto je nejspecifičtější operátoru převodu.
    * V opačném případě převod je nejednoznačný a dojde k chybě kompilace.
*  Nakonec použijte převod:
    * Pokud `S` není `SX`, pak standardní implicitní převod z `S` k `SX` provádí.
    * Nejspecifičtější operátor převodu je vyvolána k převodu z `SX` k `TX`.
    * Pokud `TX` není `T`, pak standardní implicitní převod z `TX` k `T` provádí.

### <a name="processing-of-user-defined-explicit-conversions"></a>Zpracování uživatelem definované explicitní převody

Uživatelem definované explicitní převod z typu `S` na typ `T` zpracování následujícím způsobem:

*  Určete typy `S0` a `T0`. Pokud `S` nebo `T` jsou typy s možnou hodnotou Null, `S0` a `T0` se jejich základní typy, v opačném případě `S0` a `T0` rovnají `S` a `T` v uvedeném pořadí.
*  Najít sadu typů, `D`, ze kterých uživatelsky definovaný převod operátory se budou považovat za. Tato sada se skládá z `S0` (Pokud `S0` je třída nebo struktura), základní třídy `S0` (Pokud `S0` je třída), `T0` (Pokud `T0` je třída nebo struktura) a základní třídy `T0` (Pokud `T0`je třída).
*  Najít sadu operátory použitelné uživatelsky definovaná a zdvižené převodu `U`. Tato sada sestává z uživatelem definované a zdvižené implicitní nebo explicitní operátory převodu deklaroval třídy nebo struktury v `D` , převést z typu zahrnující nebo vlastněný `S` na typ zahrnující nebo vlastněný `T`. Pokud `U` je prázdný, převod není definován a dojde k chybě kompilace.
*  Najít co nejspecifičtější typ. zdroj `SX`, operátorů v `U`:
    * Pokud je libovolná z operátorů v `U` převést `S`, pak `SX` je `S`.
    * Jinak, pokud je libovolná z operátorů v `U` převést z typů, které zahrnují `S`, pak `SX` je nejvíce encompassed typ v kombinovanou sadu typů zdrojů těchto operátorů. Bez většiny zahrnuta najdete typ, pak převod je nejednoznačný a dojde k chybě kompilace.
    * V opačném případě `SX` je nejvíce včetně typu v kombinovanou sadu typů zdrojů operátorů v `U`. Pokud nelze najít největší včetně právě jeden typ, pak převod je nejednoznačný a dojde k chybě kompilace.
*  Najít co nejspecifičtější typ. cílové `TX`, operátorů v `U`:
    * Pokud je libovolná z operátorů v `U` převést na `T`, pak `TX` je `T`.
    * Jinak, pokud je libovolná z operátorů v `U` převod na typy, které jsou zahrnuté ve `T`, pak `TX` je nejvíce včetně typu v kombinovanou sadu cílové typy těchto operátorů. Pokud nelze najít největší včetně právě jeden typ, pak převod je nejednoznačný a dojde k chybě kompilace.
    * V opačném případě `TX` je nejvíce encompassed typ v kombinovanou sadu cílové typy operátorů v `U`. Bez většiny zahrnuta najdete typ, pak převod je nejednoznačný a dojde k chybě kompilace.
*  Najdete nejspecifičtější operátor převodu:
    * Pokud `U` obsahuje přesně jeden uživatelsky definovaný převod operátor, který převede z `SX` k `TX`, pak toto je nejspecifičtější operátoru převodu.
    * Jinak, pokud `U` obsahuje přesně jeden operátor zdvižené převodu, která převede z `SX` k `TX`, pak toto je nejspecifičtější operátoru převodu.
    * V opačném případě převod je nejednoznačný a dojde k chybě kompilace.
*  Nakonec použijte převod:
    * Pokud `S` není `SX`, pak standardní explicitní převod z `S` k `SX` provádí.
    * Operátor nejspecifičtější uživatelem definovaného převodu je vyvolána k převodu z `SX` k `TX`.
    * Pokud `TX` není `T`, pak standardní explicitní převod z `TX` k `T` provádí.

## <a name="anonymous-function-conversions"></a>Převody anonymní funkce

*Anonymous_method_expression* nebo *lambda_expression* klasifikovaný jako anonymní funkce ([výrazy anonymní funkce](expressions.md#anonymous-function-expressions)). Výraz nemá typ, ale může být implicitně převeden na typ kompatibilní delegáta nebo typu stromu výrazu. Konkrétně anonymní funkci `F` je kompatibilní s typem delegáta `D` k dispozici:

*  Pokud `F` obsahuje *anonymous_function_signature*, pak `D` a `F` mají stejný počet parametrů.
*  Pokud `F` neobsahuje *anonymous_function_signature*, pak `D` může mít nula nebo více parametrů typu, pokud žádný parametr `D` má `out` modifikátor parametru.
*  Pokud `F` má seznam parametrů explicitně, každý parametr `D` má stejný typ a modifikátory jako odpovídající parametr v `F`.
*  Pokud `F` má seznam implicitně typované parametrů, `D` nemá žádné `ref` nebo `out` parametry.
*  Pokud tělo `F` je výraz a buď `D` má `void` návratový typ nebo `F` je asynchronní a `D` má návratový typ `Task`, pak když jednotlivé parametry nástroje `F` je uveden typ odpovídající parametr v `D`, text `F` je platný výraz (wrt [výrazy](expressions.md)), který by byl povolen jako *statement_expression* ([Příkazy výrazů](statements.md#expression-statements)).
*  Pokud tělo `F` je blok příkazů a buď `D` má `void` návratový typ nebo `F` je asynchronní a `D` má návratový typ `Task`, pak když jednotlivé parametry nástroje `F` je uveden typ odpovídající parametr v `D`, text `F` je platný příkaz blok (wrt [bloky](statements.md#blocks)) v které ne `return` příkaz Určuje výraz.
*  Pokud tělo `F` pracuje, výrazem, a *buď* `F` je jiné asynchronní a `D` má návratový typ jiný než void `T`, *nebo* `F` je asynchronní a `D` má návratový typ `Task<T>`, pak když jednotlivé parametry nástroje `F` je uveden typ odpovídající parametr v `D`, text `F` je platný výraz (wrt [ Výrazy](expressions.md)), který je implicitně převést na `T`.
*  Pokud tělo `F` je blok příkazů a *buď* `F` je jiné asynchronní a `D` má návratový typ jiný než void `T`, *nebo* `F` je asynchronní a `D` má návratový typ `Task<T>`, pak když jednotlivé parametry nástroje `F` je uveden typ odpovídající parametr v `D`, text `F` je platný příkaz blok (wrt [bloky ](statements.md#blocks)) s není dostupný koncový bod, ve nichž každý `return` příkaz Určuje výraz, který je implicitně převést na `T`.

Za účelem zkrácení, tato část používá zkratka pro typy úloh `Task` a `Task<T>` ([asynchronních funkcí](classes.md#async-functions)).

Výraz lambda `F` je kompatibilní s typu stromu výrazu `Expression<D>` Pokud `F` kompatibilní s typem delegáta `D`. Všimněte si, že to neplatí pro anonymní metody, pouze výrazy lambda.

Některé výrazy lambda nejde převést na strom typy výrazů: I když převod *existuje*, selže v době kompilace. To je v případě, pokud výraz lambda:

*  Má *bloku* textu
*  Obsahuje operátory jednoduchého a složeného přiřazení
*  Obsahuje výraz dynamické vazby
*  Je asynchronní

Následující příklady použití typu obecného delegátu `Func<A,R>` která představuje funkci, která přebírá argument typu `A` a vrátí hodnotu typu `R`:
```csharp
delegate R Func<A,R>(A arg);
```

V přiřazení
```csharp
Func<int,int> f1 = x => x + 1;                 // Ok

Func<int,double> f2 = x => x + 1;              // Ok

Func<double,int> f3 = x => x + 1;              // Error

Func<int, Task<int>> f4 = async x => x + 1;    // Ok
```
parametry a návratovým typem každý anonymní funkce se stanoví z typu proměnné, ke kterému je přiřazena anonymní funkce.

První přiřazení úspěšně převede anonymní funkce na typ delegáta `Func<int,int>` vzhledem k tomu, že pokud `x` je daný typ `int`, `x+1` je platný výraz, který je implicitně převést na typ `int`.

Podobně, druhé přiřazení úspěšně převede anonymní funkce na typ delegáta `Func<int,double>` protože výsledek `x+1` (typu `int`) je implicitně převést na typ `double`.

Ale třetí přiřazení je chyba kompilace, protože když `x` je daný typ `double`, výsledek `x+1` (typu `double`) není implicitně převést na typ `int`.

Čtvrtý přiřazení úspěšně převede anonymní asynchronní funkce na typ delegáta `Func<int, Task<int>>` protože výsledek `x+1` (typu `int`) implicitně převést na typ výsledku `int` typu úloh `Task<int>`.

Anonymní funkce může ovlivnit rozlišení přetížení a účastnit odvození typu proměnné. Zobrazit [funkce členy](expressions.md#function-members) další podrobnosti.

### <a name="evaluation-of-anonymous-function-conversions-to-delegate-types"></a>Hodnocení produktu anonymní funkce převody na typy delegátů

Převod na typ delegáta anonymní funkce vytvoří instanci delegáta, který odkazuje na anonymní funkce a (pravděpodobně prázdná) sadu zachycené vnější proměnné, které jsou aktivní v době vyhodnocení. Když je vyvolán delegát, provede se tělo anonymní funkce. Kód v těle je prováděn pomocí sady zachycené vnější proměnné odkazovat na delegáta.

Seznam vyvolání delegáta vytvořenými anonymní funkci obsahuje jednu položku. Přesné cílového objektu a cílové metody delegáta nejsou specifikována. Zejména neurčená, jestli je cílový objekt delegáta `null`, `this` hodnotu nadřazeného členské funkce nebo některý objekt.

Převody sémanticky identické anonymní funkce (pravděpodobně prázdná) stejnou sadou zachycené vnější proměnné instance stejné typy delegátů jsou povolené (ale není nutné) se vraťte stejnou instanci delegáta. Termín sémanticky identické se zde používá k znamená, že spuštění anonymní funkce, ve všech případech se vytvoří stejný účinek zadaný stejné argumenty. Toto pravidlo povoluje následující optimalizovat kód.

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

Protože dvou delegátů anonymní funkce mají stejné (prázdné) sadu zachycené vnější proměnné, protože jsou sémanticky stejné anonymní funkce, kompilátor je oprávněny mít delegáty odkazovat na stejnou cílovou metodu. Ve skutečnosti kompilátor smí vracet velmi stejnou instanci delegáta oba výrazy anonymní funkce.

### <a name="evaluation-of-anonymous-function-conversions-to-expression-tree-types"></a>Hodnocení produktu anonymní funkce převody na typy stromu výrazů

Anonymní funkce Převod do typu stromu výrazu vytváří strom výrazu ([typy stromu výrazů](types.md#expression-tree-types)). Přesněji řečeno vyhodnocení anonymní funkce převodu vede k vytváření objektovou strukturu, která reprezentuje strukturu těchto anonymní samotné funkce. Přesné strukturu stromu výrazů, jakož i přesný postup pro vytváření, je definován implementací.

### <a name="implementation-example"></a>Příklad implementace

Tato část popisuje možnou implementaci anonymní funkce převody z hlediska jiné konstrukce jazyka C#. Implementace je zde popsáno, je založená na stejné zásady použít kompilátor jazyka Microsoft C#, ale nejsou v žádném smyslu mandátem implementace, ani je jediné možné. Pouze stručně uvádí jako jejich přesné sémantika je mimo rozsah této specifikace převody na stromy výrazů.

Zbývající část této části obsahuje několik příkladů kódu, který obsahuje anonymní funkce s různými charakteristikami. Pro každý příklad je k dispozici odpovídající překlad do kódu, který používá jenom jiné konstrukce jazyka C#. V příkladech identifikátor `D` se předpokládá, že podle představují následující typ delegátu:
```csharp
public delegate void D();
```

Nejjednodušší forma anonymní funkce je ten, který zachycuje žádné vnější proměnné:
```csharp
class Test
{
    static void F() {
        D d = () => { Console.WriteLine("test"); };
    }
}
```

To lze přeložit na instanci delegáta, který odkazuje na generovaný kompilátorem statické metody, ve kterém je umístí kód anonymní funkce:
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

Anonymní funkce v následujícím příkladu odkazuje na členy instance `this`:
```csharp
class Test
{
    int x;

    void F() {
        D d = () => { Console.WriteLine(x); };
    }
}
```

To lze přeložit na metodu instance generovaný kompilátorem obsahující kód anonymní funkce:
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

V tomto příkladu anonymní funkce zaznamenává místní proměnnou:
```csharp
class Test
{
    void F() {
        int y = 123;
        D d = () => { Console.WriteLine(y); };
    }
}
```

Doba života lokální proměnné musí teď rozšíří do alespoň životnost delegáta anonymní funkce. Toho lze dosáhnout pomocí "zvedání" místní proměnné do pole třídy generovaný kompilátorem. Vytvoření instance lokální proměnné ([instance lokálních proměnných](expressions.md#instantiation-of-local-variables)) pak odpovídá vytvoření instance třídy generované kompilátorem a přístup k místní proměnné odpovídá přístup k poli v instanci třídy generované kompilátorem. Anonymní funkce navíc změní metodu instance třídy generované kompilátorem:
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

Nakonec následující anonymní funkce zachytávání `this` a také dvě lokální proměnné s jinou životnost:
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

Tady je vytvořená třída generovaný kompilátorem příkazu for each blok, ve kterých místní hodnoty jsou zachyceny tak, aby místní hodnoty v různé bloky mají nezávislé životnosti. Instance `__Locals2`, třída generovaný kompilátorem pro vnitřní příkaz blok obsahuje místní proměnná `z` a pole, které odkazuje na instanci `__Locals1`.  Instance `__Locals1`, třída generovaný kompilátorem pro blok vnější příkazů obsahuje místní proměnná `y` a pole, které odkazuje `this` nadřazeného člena funkce. S těmito datovými strukturami, je možné dosáhnout všechna zachytit vnější proměnné prostřednictvím instance `__Local2`, a kód anonymní funkce je tedy možné implementovat jako metodu instance této třídy.

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

Zde použít k zachycení lokálních proměnných stejný postup lze také při převodu anonymní funkce na stromy výrazů: odkazy na objekty generovaný kompilátorem mohou být uloženy ve stromu výrazů a může být přístup k místní proměnné reprezentována jako pole k přistupuje na tyto objekty. Výhodou tohoto přístupu je, že umožňuje "zdvižené" místní proměnné sdílen delegáty a stromů výrazů.

## <a name="method-group-conversions"></a>Převody skupiny – metoda

Implicitní převod ([implicitních převodů](conversions.md#implicit-conversions)) existuje ze skupiny – metoda ([výrazu klasifikace](expressions.md#expression-classifications)) na typ delegáta kompatibilní. Zadaný typ delegáta `D` a výraz `E` , který je klasifikován jako skupinu metod, existuje implicitní převod z `E` k `D` Pokud `E` obsahuje alespoň jednu metodu, která lze použít v jeho (normální formuláře [Použitelná funkce člena](expressions.md#applicable-function-member)) na seznam argumentů vytvořený použitím typů parametrů a modifikátory `D`, jak je popsáno v následujícím.

Kompilace aplikace převodu ze skupiny metoda `E` typu delegátu `D` je popsaná v následující. Všimněte si, že existenci implicitní převod z `E` k `D` nezaručuje, že aplikace za kompilace převodu bude úspěšné bez chyb.

*  Jedinou metodu `M` je vybrána odpovídající volání metody ([volání metod](expressions.md#method-invocations)) ve formátu `E(A)`, s těmito změnami:
    * Seznam argumentů `A` je seznamem výrazů, každý klasifikovaný jako proměnnou a s typem a modifikátor (`ref` nebo `out`) příslušného parametru v *formal_parameter_list* z `D`.
    * Metody Release candidate považovat za jsou jenom metody, které se dají použít v jejich normálním formuláře ([použitelná funkce člena](expressions.md#applicable-function-member)), není nastavení platných pouze v jejich rozšířené formuláře.
*  Pokud algoritmus [volání metod](expressions.md#method-invocations) dojde k chybě, dojde k chybě v době kompilace. V opačném případě algoritmus vytvoří jeden nejlepší metody `M` mají stejný počet parametrů jako `D` a převod se považuje za existovat.
*  Vybrané metody `M` musí být kompatibilní ([delegovat kompatibility](delegates.md#delegate-compatibility)) s typem delegáta `D`, nebo v opačném případě dojde k chybě kompilace.
*  Pokud vybrané metody `M` je metoda instance, výraz instance přidružené k `E` Určuje cílový objekt delegáta.
*  Pokud vybrané metody M metodu rozšíření, které je označené prostřednictvím přístupu ke členu na výraz instance, určuje tento výraz instance cílové objektů delegáta.
*  Výsledkem převodu je hodnota typu `D`, a to nově vytvořený delegát, který odkazuje na vybrané metody a cílový objekt.
*  Všimněte si, že tento proces může vést k vytvoření delegáta, kterého lze metodu rozšíření, pokud algoritmus [volání metod](expressions.md#method-invocations) nepodaří najít metodu instance, ale při zpracování volání úspěšné `E(A)` jako rozšíření volání metody ([volání metod rozšíření](expressions.md#extension-method-invocations)). Proto vytvoří delegát zaznamená metody rozšíření, stejně jako svůj první argument.

Následující příklad ukazuje metodu skupiny převody:
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

Přiřazení `d2` ukazuje, jak je možné k vytvoření delegáta metodě, která má méně odvozené (kontravariantní) typy parametrů a více odvozený návratový typ (kovariantní).

Přiřazení `d3` ukazuje, jak neexistuje žádný převod Pokud metoda není použitelné.

Přiřazení `d4` ukazuje, jak metoda musí být použitelné v podobě normální.

Přiřazení `d5` ukazuje, jak budou moci parametry a návratovým typem delegáta a metoda se liší pouze pro typy odkazů.

Stejně jako všechny ostatní implicitní a explicitní převody, operátor přetypování lze explicitně provést převod metody skupiny. Díky tomu se v příkladu
```csharp
object obj = new EventHandler(myDialog.OkClick);
```
Místo toho může být napsaná
```csharp
object obj = (EventHandler)myDialog.OkClick;
```

Metoda skupiny může ovlivnit řešení přetížení a účastnit odvození typu proměnné. Zobrazit [funkce členy](expressions.md#function-members) další podrobnosti.

Hodnocení za běhu metoda převodu skupiny probíhá následujícím způsobem:

*  Pokud metoda vybrané v době kompilace je metoda instance, nebo metodu rozšíření, která je přístupná jako metodu instance, objekt cílového delegáta je určen z instance výraz přidružený k `E`:
    * Instance výraz je vyhodnocen. Je-li toto vyhodnocení způsobí výjimku, jsou spuštěny žádné další kroky.
    * Pokud má výraz instance *reference_type*, hodnota vypočítaná aplikací výraz instance stane cílového objektu. Pokud je metoda instance vybrané metody a cílový objekt má `null`, `System.NullReferenceException` je vyvolána, a že jsou provedeny žádné další kroky.
    * Pokud má výraz instance *value_type*, operace zabalení ([zabalení převody](types.md#boxing-conversions)) se provádí za účelem převodu hodnoty na objekt, a tento objekt se stane cílového objektu.
*  V opačném případě vybrané metody je součástí volání statické metody, a cílový objekt delegáta má `null`.
*  Nová instance typu delegáta `D` je přidělen. Pokud není k dispozici dostatek paměti k přidělení nové instance `System.OutOfMemoryException` je vyvolána, a že jsou provedeny žádné další kroky.
*  Odkaz na metodu, která byla určena v době kompilace se inicializuje novou instanci delegáta a odkaz k cílovému objektu svého vypočítané výše.
