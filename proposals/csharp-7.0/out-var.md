---
ms.openlocfilehash: a0d80afc47e9f0073237db9b8d7a4f0b045c1b0b
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484508"
---
# <a name="out-variable-declarations"></a><span data-ttu-id="66496-101">Deklarace proměnné out</span><span class="sxs-lookup"><span data-stu-id="66496-101">Out variable declarations</span></span>

<span data-ttu-id="66496-102">Funkce *deklarace out proměnné* umožňuje deklaraci proměnné v umístění, které je předávána jako `out` argument.</span><span class="sxs-lookup"><span data-stu-id="66496-102">The *out variable declaration* feature enables a variable to be declared at the location that it is being passed as an `out` argument.</span></span>

```antlr
argument_value
    : 'out' type identifier
    | ...
    ;
```

<span data-ttu-id="66496-103">Proměnná deklarovaná tímto způsobem se nazývá *Výstupní proměnná*.</span><span class="sxs-lookup"><span data-stu-id="66496-103">A variable declared this way is called an *out variable*.</span></span> <span data-ttu-id="66496-104">Jako typ proměnné můžete použít klíčové slovo `var`.</span><span class="sxs-lookup"><span data-stu-id="66496-104">You may use the contextual keyword `var` for the variable's type.</span></span> <span data-ttu-id="66496-105">Obor bude stejný jako *Proměnná Pattern* zavedená pomocí porovnávání vzorů.</span><span class="sxs-lookup"><span data-stu-id="66496-105">The scope will be the same as for a *pattern-variable* introduced via pattern-matching.</span></span>

<span data-ttu-id="66496-106">Podle specifikace jazyka (přístup k elementu 7.6.7 oddílu) argument-seznam element-Access (Indexový výraz) neobsahuje argumenty ref nebo out.</span><span class="sxs-lookup"><span data-stu-id="66496-106">According to Language Specification (section 7.6.7 Element access) the argument-list of an element-access (indexing expression) does not contain ref or out arguments.</span></span> <span data-ttu-id="66496-107">Jsou však povoleny kompilátorem pro různé scénáře, například indexery deklarované v metadatech, které přijímají `out`.</span><span class="sxs-lookup"><span data-stu-id="66496-107">However, they are permitted by the compiler for various scenarios, for example indexers declared in metadata that accept `out`.</span></span>

<span data-ttu-id="66496-108">V rámci rozsahu místní proměnné zavedené argument_value se jedná o chybu při kompilaci, která odkazuje na tuto místní proměnnou na textové pozici, která předchází jeho deklaraci.</span><span class="sxs-lookup"><span data-stu-id="66496-108">Within the scope of a local variable introduced by an argument_value, it is a compile-time error to refer to that local variable in a textual position that precedes its declaration.</span></span>

<span data-ttu-id="66496-109">Je také chyba odkazování na implicitně typované proměnné (§ 8.5.1) do stejného seznamu argumentů, který okamžitě obsahuje svou deklaraci.</span><span class="sxs-lookup"><span data-stu-id="66496-109">It is also an error to reference an implicitly-typed (§8.5.1) out variable in the same argument list that immediately contains its declaration.</span></span>

<span data-ttu-id="66496-110">Rozlišení přetěžování je změněno následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="66496-110">Overload resolution is modified as follows:</span></span>

<span data-ttu-id="66496-111">Přidáme nový převod:</span><span class="sxs-lookup"><span data-stu-id="66496-111">We add a new conversion:</span></span>

> <span data-ttu-id="66496-112">Existuje *Převod z výrazu* z implicitně typované deklarace proměnné na každý typ.</span><span class="sxs-lookup"><span data-stu-id="66496-112">There is a *conversion from expression* from an implicitly-typed out variable declaration to every type.</span></span>

<span data-ttu-id="66496-113">Lze</span><span class="sxs-lookup"><span data-stu-id="66496-113">Also</span></span>

> <span data-ttu-id="66496-114">Typ argumentu explicitně typovaného objektu je deklarovaný typ.</span><span class="sxs-lookup"><span data-stu-id="66496-114">The type of an explicitly-typed out variable argument is the declared type.</span></span>

<span data-ttu-id="66496-115">a</span><span class="sxs-lookup"><span data-stu-id="66496-115">and</span></span>

> <span data-ttu-id="66496-116">Argument proměnné s implicitním typem není typu.</span><span class="sxs-lookup"><span data-stu-id="66496-116">An implicitly-typed out variable argument has no type.</span></span>

<span data-ttu-id="66496-117">*Konverze z výrazu* z implicitně typované deklarace proměnné není považována za lepší, než je jakýkoli jiný *Převod výrazu*.</span><span class="sxs-lookup"><span data-stu-id="66496-117">The *conversion from expression* from an implicitly-typed out variable declaration is not considered better than any other *conversion from expression*.</span></span>

<span data-ttu-id="66496-118">Typ implicitně typované proměnné je typ odpovídajícího parametru v signatuře metody vybrané řešením přetížení.</span><span class="sxs-lookup"><span data-stu-id="66496-118">The type of an implicitly-typed out variable is the type of the corresponding parameter in the signature of the method selected by overload resolution.</span></span>

<span data-ttu-id="66496-119">K reprezentaci deklarace v argumentu out var se přidá nový uzel syntaxe `DeclarationExpressionSyntax`.</span><span class="sxs-lookup"><span data-stu-id="66496-119">The new syntax node `DeclarationExpressionSyntax` is added to represent the declaration in an out var argument.</span></span>
