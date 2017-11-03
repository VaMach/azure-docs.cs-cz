---
title: "Zásady ve službě Azure API Management | Microsoft Docs"
description: "Naučte se vytvářet, upravovat a konfigurovat zásady ve službě API Management."
services: api-management
documentationcenter: 
author: vladvino
manager: erikre
editor: 
ms.assetid: 537e5caf-708b-430e-a83f-72b70af28aa9
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/15/2016
ms.author: apimpm
ms.openlocfilehash: ef4cb447430a613dd519d96dd7732a9349ebba39
ms.sourcegitcommit: 5735491874429ba19607f5f81cd4823e4d8c8206
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/16/2017
---
# <a name="policies-in-azure-api-management"></a>Zásady ve službě Azure API Management
Ve službě Azure API Management zásady jsou vynikající funkcí systému, která vydavatelům umožňuje měnit chování rozhraní API prostřednictvím konfigurace. Zásady představují kolekci příkazů, které jsou prováděny postupně v požadavku nebo odezvy z rozhraní API. Oblíbené příkazy patří převod formátu XML do formátu JSON a četnosti omezení omezit množství příchozích volání od vývojáře. Jsou dostupné ihned mnoho další zásady.

Najdete v článku [referenční informace o zásadách] [ Policy Reference] pro úplný seznam příkazy zásad a jejich nastavení.

Jsou zásady použity uvnitř bránu, která je umístěna mezi příjemce rozhraní API a spravované rozhraní API. Brána přijme všechny požadavky a obvykle předává je do základního rozhraní API v nezměněném stavu. Zásady ale můžete použít změny požadavek příchozí i odchozí odpovědi.

Výrazy zásad můžete použít jako hodnoty atributů nebo textové hodnoty v libovolných zásadách API Management (pokud zásady neurčí jinak). Některé zásady, jako [řízení toku] [ Control flow] a [nastavená proměnná] [ Set variable] jsou založené na výrazech zásad. Další informace najdete v tématu [pokročilé zásady] [ Advanced policies] a [výrazy zásad][Policy expressions].

## <a name="scopes"></a>Postup konfigurace zásad
Zásady je možné nakonfigurovat globálně nebo na rozsah [produktu][Product], [rozhraní API] [ API] nebo [operace][Operation]. Konfigurace zásad, přejděte do editoru zásad na portálu vydavatele.

![Zásady nabídky][policies-menu]

Editor zásad se skládá z tři hlavní části: obor zásady (nahoře), definice zásady, kde jsou zásady upravovat (vlevo) a příkazy seznamu (vpravo):

![Editor zásad][policies-editor]

Chcete-li začít, konfigurace zásad, musíte nejdřív vybrat rozsah, kdy mají zásady platit. Na tomto snímku obrazovky **Starter** produktu je vybrána. Všimněte si, že odmocnina symbol vedle názvu zásady označuje, že je zásada na této úrovni již použita.

![Rozsah][policies-scope]

Vzhledem k tomu, že již byla použita zásada, nastavení se zobrazí v definici zobrazení.

![Konfigurace][policies-configure]

Zásady se zobrazí, jen pro čtení na první. Chcete-li upravit definici kliknutím **konfigurovat zásadu** akce.

![Upravit][policies-edit]

Definice zásady je jednoduchý dokument XML, který popisuje posloupnost příchozí a odchozí příkazy. Soubor XML můžete upravovat přímo v okně definice. Seznam příkazů je uvedený vpravo a příkazy pro aktuální obor jsou povolené a zvýrazní; jak je předvedeno pomocí **omezení četnosti volání** příkaz v výše uvedený snímek obrazovky.

Kliknutím na povoleno příkaz přidá příslušný kód XML v umístění kurzoru v definici zobrazení. 

> [!NOTE]
> Pokud není povoleno zásadami, které chcete přidat, ujistěte se, že jste ve správném oboru pro tuto zásadu. Každý prohlášení o zásadách je určená pro použití v určité obory a zásad oddílů. Chcete-li zkontrolovat zásady části a obory pro zásadu, zkontrolujte **využití** oddíl pro tuto zásadu v [referenční informace o zásadách][Policy Reference].
> 
> 

Úplný seznam příkazy zásad a jejich nastavení jsou k dispozici v [referenční informace o zásadách][Policy Reference].

Například pokud chcete přidat nové prohlášení omezit příchozí požadavky na zadané IP adresy, umístěte kurzor právě do obsah `inbound` – element XML a klikněte na tlačítko **volající omezení IP adres** příkaz.

![Zásady omezení][policies-restrict]

Bude přidáno fragmentu kódu XML do `inbound` element, který poskytuje pokyny o tom, jak nakonfigurovat příkaz.

```xml
<ip-filter action="allow | forbid">
    <address>address</address>
    <address-range from="address" to="address"/>
</ip-filter>
```

Omezit příchozí požadavky a přijímat pouze ty z IP adresy 1.2.3.4 upravte soubor XML následujícím způsobem:

```xml
<ip-filter action="allow">
    <address>1.2.3.4</address>
</ip-filter>
```

![Uložit][policies-save]

Po dokončení konfigurace příkazy pro zásadu, klikněte na tlačítko **Uložit** a změny se rozšíří do rozhraní API správy brány okamžitě.

## <a name="sections"></a>Principy zásad konfigurace
Zásady je řada příkazů, které jsou spouštěny v pořadí pro požadavek a odpověď. Konfigurace je správně rozdělené do `inbound`, `backend`, `outbound`, a `on-error` částech, jak je znázorněno v následující konfiguraci.

```xml
<policies>
  <inbound>
    <!-- statements to be applied to the request go here -->
  </inbound>
  <backend>
    <!-- statements to be applied before the request is forwarded to 
         the backend service go here -->
  </backend>
  <outbound>
    <!-- statements to be applied to the response go here -->
  </outbound>
  <on-error>
    <!-- statements to be applied if there is an error condition go here -->
  </on-error>
</policies> 
```

Pokud dojde k chybě během zpracování požadavku, všechny zbývající kroky `inbound`, `backend`, nebo `outbound` oddíly se přeskočí a provádění přejde na příkazy v `on-error` části. Tím, že příkazy zásad v `on-error` části Chyba můžete zkontrolovat pomocí `context.LastError` vlastnost, zkontrolovat a upravit pomocí odpovědi chyba `set-body` zásady a nakonfigurovat, co se stane, když dojde k chybě. Existují kódy chyb pro vestavěné kroky a chyby, které mohou nastat při zpracovávání příkazy zásad. Další informace najdete v tématu [zpracování chyb v zásady služby API Management](https://msdn.microsoft.com/library/azure/mt629506.aspx).

Vzhledem k tomu, že zásady můžete zadat na různých úrovních (globální, produktu, rozhraní api a operace) konfigurace poskytuje způsob, jak můžete určit pořadí, ve kterém spustit příkazy definice zásady s ohledem na zásady nadřazené. 

Zásady obory jsou vyhodnocovány v uvedeném pořadí.

1. Globální obor
2. Rozsah produktu
3. Rozhraní API oboru
4. Operace oboru

Příkazy v nich vyhodnoceny podle umístění `base` elementu, pokud je k dispozici. Globální zásady nemá nadřazený zásady a pomocí `<base>` element v ní nemá žádný vliv.

Například pokud máte zásadu na globální úrovni a zásada nakonfigurovaná pro rozhraní API, pak vždy, když se používá toto rozhraní API konkrétní obě zásady se použijí. API Management umožňuje deterministickou řazení příkazy kombinované zásad prostřednictvím base element. 

```xml
<policies>
    <inbound>
        <cross-domain />
        <base />
        <find-and-replace from="xyz" to="abc" />
    </inbound>
</policies>
```

V definici zásady příkladu výše `cross-domain` příkaz by provést před všechny vyšší zásady, které by naopak následovat `find-and-replace` zásad. 

Chcete-li zobrazit zásady v aktuálním oboru v editoru zásad, klikněte na tlačítko **přepočítat efektivní zásady pro vybraný obor**.

## <a name="next-steps"></a>Další kroky
Podívejte se na následující videa na výrazech zásad.

> [!VIDEO https://channel9.msdn.com/Blogs/AzureApiMgmt/Policy-Expressions-in-Azure-API-Management/player]
> 
> 

[Policy Reference]: api-management-policy-reference.md
[Product]: api-management-howto-add-products.md
[API]: api-management-howto-add-products.md#add-apis 
[Operation]: api-management-howto-add-operations.md

[Advanced policies]: https://msdn.microsoft.com/library/azure/dn894085.aspx
[Control flow]: https://msdn.microsoft.com/library/azure/dn894085.aspx#choose
[Set variable]: https://msdn.microsoft.com/library/azure/dn894085.aspx#set_variable
[Policy expressions]: https://msdn.microsoft.com/library/azure/dn910913.aspx

[policies-menu]: ./media/api-management-howto-policies/api-management-policies-menu.png
[policies-editor]: ./media/api-management-howto-policies/api-management-policies-editor.png
[policies-scope]: ./media/api-management-howto-policies/api-management-policies-scope.png
[policies-configure]: ./media/api-management-howto-policies/api-management-policies-configure.png
[policies-edit]: ./media/api-management-howto-policies/api-management-policies-edit.png
[policies-restrict]: ./media/api-management-howto-policies/api-management-policies-restrict.png
[policies-save]: ./media/api-management-howto-policies/api-management-policies-save.png
