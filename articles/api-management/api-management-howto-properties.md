---
title: "Použití vlastností v zásadách Azure API Management"
description: "Další informace o použití vlastnosti zásad Azure API Management."
services: api-management
documentationcenter: 
author: steved0x
manager: erikre
editor: 
ms.assetid: 6f39b00f-cf6e-4cef-9bf2-1f89202c0bc0
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/15/2016
ms.author: apimpm
ms.openlocfilehash: 3b0fe2a300038e13cc488bdb4f50f8be270ea8f4
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-use-properties-in-azure-api-management-policies"></a>Použití vlastností v zásadách Azure API Management
Zásady služby API Management jsou vynikající funkcí systému, která vydavatelům umožňuje měnit chování rozhraní API prostřednictvím konfigurace. Zásady představují kolekci příkazů, které se postupně provádí na základě požadavku nebo odezvy z rozhraní API. Příkazy zásad se dá vytvořit pomocí literálu textové hodnoty, výrazy zásad a vlastnosti. 

Každá instance služby API Management má vlastnosti kolekci dvojic klíč/hodnota, které jsou globální v instanci služby. Tyto vlastnosti můžete použít ke správě konstantní hodnoty řetězce ve všech konfigurací rozhraní API a zásady. Každou vlastnost má následující atributy.

| Atribut | Typ | Popis |
| --- | --- | --- |
| Name (Název) |Řetězec |Název vlastnosti Může obsahovat pouze písmena, číslice, období, pomlčky a podtržítka. |
| Hodnota |Řetězec |Hodnota vlastnosti. Se nesmí být prázdný a skládat jenom z prázdných znaků. |
| Tajný kód |Logická hodnota |Určuje, zda hodnota je tajný klíč a zašifrovat nebo ne. |
| Značky |Pole řetězců |Volitelné značky, pokud je zadaný, mohou být použity k filtrování seznamu vlastností. |

Vlastnosti jsou nakonfigurované na portálu vydavatele na **vlastnosti** kartě. V následujícím příkladu jsou tři vlastnosti nastavené.

![Vlastnosti][api-management-properties]

Vlastnost hodnoty mohou obsahovat řetězcové literály a [výrazy zásad](https://msdn.microsoft.com/library/azure/dn910913.aspx). V následující tabulce jsou uvedeny předchozí tři ukázkové vlastnosti a jejich atributy. Hodnota `ExpressionProperty` výrazu zásad, která vrací řetězec obsahující aktuální datum a čas. Vlastnost `ContosoHeaderValue` je označena jako tajný klíč, takže jeho hodnota se nezobrazí.

| Name (Název) | Hodnota | Tajný kód | Značky |
| --- | --- | --- | --- |
| ContosoHeader |trackingId |False |Contoso |
| ContosoHeaderValue |•••••••••••••••••••••• |True |Contoso |
| ExpressionProperty |@(DateTime.Now.ToString()) |False | |

## <a name="to-use-a-property"></a>Chcete-li použít vlastnost
Chcete-li použít vlastnost v zásadách, označte název vlastnosti uvnitř pár dvojité složené závorky jako `{{ContosoHeader}}`, jak je znázorněno v následujícím příkladu.

```xml
<set-header name="{{ContosoHeader}}" exists-action="override">
  <value>{{ContosoHeaderValue}}</value>
</set-header>
```

V tomto příkladu `ContosoHeader` slouží jako název záhlaví v `set-header` zásady, a `ContosoHeaderValue` slouží jako hodnotu této hlavičky. Pokud tato zásada se vyhodnotí během požadavku nebo odpovědi k bráně správy rozhraní API `{{ContosoHeader}}` a `{{ContosoHeaderValue}}` nahradí se jejich hodnoty odpovídající vlastnost.

Vlastnosti lze použít jako dokončení atribut nebo element hodnoty, jak je znázorněno v předchozím příkladu, ale můžete také měly být vložen do nebo v kombinaci s část výrazu literálovou, jak je znázorněno v následujícím příkladu:`<set-header name = "CustomHeader{{ContosoHeader}}" ...>`

Vlastnosti může také obsahovat výrazy zásad. V následujícím příkladu `ExpressionProperty` se používá.

```xml
<set-header name="CustomHeader" exists-action="override">
    <value>{{ExpressionProperty}}</value>
</set-header>
```

Pokud tato zásada je vyhodnocena, `{{ExpressionProperty}}` se nahradí jeho hodnota: `@(DateTime.Now.ToString())`. Vzhledem k tomu, že hodnota je výraz zásady, je tento výraz vyhodnocen a zásady pokračuje v jeho spuštění.

Tuto funkci můžete otestovat v portálu pro vývojáře voláním operace, která má zásady s vlastností v oboru. V následujícím příkladu je volána operace dva předchozí příklad `set-header` zásady s vlastnostmi. Všimněte si, že odpověď obsahuje dva vlastní hlavičky, které byly nakonfigurovány pomocí vlastnosti zásad.

![Portál pro vývojáře][api-management-send-results]

Pokud si prohlédnete [trasování API Inspector](api-management-howto-api-inspector.md) pro volání, která zahrnuje předchozí dva ukázkové zásady s vlastnostmi, uvidíte dvě `set-header` zásad hodnoty vlastností vložit a také vyhodnocení výrazu zásad pro vlastnost, která obsahovala výraz zásady.

![Trasování API Inspector][api-management-api-inspector-trace]

Všimněte si, že při hodnot vlastností mohou obsahovat výrazy zásad, nesmí obsahovat hodnoty vlastností, ostatní vlastnosti. Pokud text obsahující odkaz na vlastnost se používá pro hodnotu vlastnosti, jako `Property value text {{MyProperty}}`, že odkaz na vlastnost nebude nahrazen a budou zahrnuty jako součást hodnoty vlastnosti.

## <a name="to-create-a-property"></a>K vytvoření vlastnosti
Chcete-li vytvořit vlastnosti, klikněte na tlačítko **přidat vlastnost** na **vlastnosti** kartě.

![Přidat vlastnost][api-management-properties-add-property-menu]

**Název** a **hodnotu** jsou požadované hodnoty. Pokud hodnota této vlastnosti je tajný klíč, podívejte se **Toto je tajný klíč** zaškrtávací políčko. Zadejte jednu nebo více značek volitelné usnadní uspořádání vlastnosti a klikněte na tlačítko **Uložit**.

![Přidat vlastnost][api-management-properties-add-property]

Při uložení nové vlastnosti **vyhledávání vlastnost** textové pole se vyplní název nové vlastnosti a zobrazí se nové vlastnosti. Chcete-li zobrazit všechny vlastnosti, zrušte **vyhledávání vlastnost** textové pole a stiskněte klávesu enter.

![Vlastnosti][api-management-properties-property-saved]

Informace o vytvoření vlastnosti pomocí rozhraní REST API najdete v tématu [vytvoření vlastnosti pomocí rozhraní REST API](https://msdn.microsoft.com/library/azure/mt651775.aspx#Put).

## <a name="to-edit-a-property"></a>Chcete-li upravit vlastnosti
Chcete-li upravit vlastnosti, klikněte na tlačítko **upravit** vedle vlastnosti, které chcete upravit.

![Upravit vlastnost][api-management-properties-edit]

Proveďte požadované změny a klikněte na tlačítko **Uložit**. Pokud změníte název vlastnosti, všechny zásady, které odkazují na tuto vlastnost se automaticky aktualizují na použití nového názvu.

![Upravit vlastnost][api-management-properties-edit-property]

Informace o úpravách vlastnosti pomocí rozhraní REST API najdete v tématu [upravit vlastnosti pomocí rozhraní REST API](https://msdn.microsoft.com/library/azure/mt651775.aspx#Patch).

## <a name="to-delete-a-property"></a>K odstranění vlastnosti
Chcete-li odstranit vlastnost, klikněte na tlačítko **odstranit** vedle vlastnost odstranit.

![Odstranit vlastnost][api-management-properties-delete]

Klikněte na tlačítko **Ano, odstraňte jej** k potvrzení.

![Potvrzení odstranění][api-management-delete-confirm]

> [!IMPORTANT]
> Pokud se všechny zásady odkazuje vlastnost, nebude možné úspěšně ho odstranit, dokud neodeberete vlastnost ze všech zásad, které ho používají.
> 
> 

Informace o odstranění vlastnosti pomocí rozhraní REST API najdete v tématu [odstranit vlastnost pomocí rozhraní REST API](https://msdn.microsoft.com/library/azure/mt651775.aspx#Delete).

## <a name="to-search-and-filter-properties"></a>Vyhledávat a filtrovat vlastnosti
**Vlastnosti** karta zahrnuje vyhledávání a filtrování funkcí, které vám pomohou při správě vaší vlastnosti. Chcete-li filtrovat seznam vlastností podle názvu vlastnosti, zadejte hledaný termín v **vyhledávání vlastnost** textové pole. Chcete-li zobrazit všechny vlastnosti, zrušte **vyhledávání vlastnost** textové pole a stiskněte klávesu enter.

![Search][api-management-properties-search]

Chcete-li filtrovat seznam vlastností podle hodnoty značky, zadejte jednu nebo více značek do **filtrovat podle značky** textové pole. Chcete-li zobrazit všechny vlastnosti, zrušte **filtrovat podle značky** textové pole a stiskněte klávesu enter.

![Filtr][api-management-properties-filter]

## <a name="next-steps"></a>Další kroky
* Další informace o práci se zásadami
  * [Zásady ve službě API Management](api-management-howto-policies.md)
  * [referenční příručce o zásadách](https://msdn.microsoft.com/library/azure/dn894081.aspx)
  * [Výrazy zásad](https://msdn.microsoft.com/library/azure/dn910913.aspx)

## <a name="watch-a-video-overview"></a>Podívejte se na video s přehledem
> [!VIDEO https://channel9.msdn.com/Blogs/AzureApiMgmt/Use-Properties-in-Policies/player]
> 
> 

[api-management-properties]: ./media/api-management-howto-properties/api-management-properties.png
[api-management-properties-add-property]: ./media/api-management-howto-properties/api-management-properties-add-property.png
[api-management-properties-edit-property]: ./media/api-management-howto-properties/api-management-properties-edit-property.png
[api-management-properties-add-property-menu]: ./media/api-management-howto-properties/api-management-properties-add-property-menu.png
[api-management-properties-property-saved]: ./media/api-management-howto-properties/api-management-properties-property-saved.png
[api-management-properties-delete]: ./media/api-management-howto-properties/api-management-properties-delete.png
[api-management-properties-edit]: ./media/api-management-howto-properties/api-management-properties-edit.png
[api-management-delete-confirm]: ./media/api-management-howto-properties/api-management-delete-confirm.png
[api-management-properties-search]: ./media/api-management-howto-properties/api-management-properties-search.png
[api-management-send-results]: ./media/api-management-howto-properties/api-management-send-results.png
[api-management-properties-filter]: ./media/api-management-howto-properties/api-management-properties-filter.png
[api-management-api-inspector-trace]: ./media/api-management-howto-properties/api-management-api-inspector-trace.png

