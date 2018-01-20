---
title: "Import a publikování vašeho prvního rozhraní API v Azure API Management | Microsoft Docs"
description: "Zjistěte, jak importovat a publikování vašeho prvního rozhraní API pomocí rozhraní API Management."
services: api-management
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.custom: mvc
ms.topic: tutorial
ms.date: 11/15/2017
ms.author: apimpm
ms.openlocfilehash: ffe5ee95c66eee7dccd25a1afd2fe639cbc273f5
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/19/2018
---
# <a name="import-and-publish-your-first-api"></a>Import a publikování vašeho prvního rozhraní API 

Tento kurz ukazuje, jak importovat rozhraní API back-end "OpenAPI specification", které se nacházejí v http://conferenceapi.azurewebsites.net?format=json. Toto rozhraní API back-end je od společnosti Microsoft a je hostovaná v Azure. 

Po importu do správy rozhraní API (APIM) rozhraní API back-end rozhraní API APIM stane průčelí za pro rozhraní API back-end. Během importu rozhraní API back-end zdroji rozhraní API a rozhraní API APIM jsou identické. APIM umožňuje přizpůsobit průčelí za podle vašich potřeb bez zásahu do rozhraní API back-end. Další informace najdete v tématu [transformace a chránit vaše rozhraní API](transform-api.md). 

V tomto kurzu se naučíte:

> [!div class="checklist"]
> * Import vašeho prvního rozhraní API
> * Test rozhraní API na portálu Azure
> * Testování rozhraní API v portálu pro vývojáře

![Nové rozhraní API](./media/api-management-get-started/created-api.png)

## <a name="prerequisites"></a>Požadavky

Dokončete následující rychlý start: [vytvoření instance služby Azure API Management](get-started-create-service-instance.md).

[!INCLUDE [api-management-navigate-to-instance.md](../../includes/api-management-navigate-to-instance.md)]

## <a name="create-api"></a>Importu a publikovat rozhraní API back-end

V této části ukazuje, jak importovat a publikovat backendu OpenAPI specifikace rozhraní API.
 
1. Vyberte **rozhraní API** z pod **API MANAGEMENT**.
2. Vyberte **OpenAPI specifikace** ze seznamu.

    ![Vytvoření rozhraní API](./media/api-management-get-started/create-api.png)

    Rozhraní API hodnoty můžete nastavit při vytváření nebo později tak, že přejdete do **nastavení** kartě.  

    |Nastavení|Hodnota|Popis|
    |---|---|---|
    |**Specifikace OpenAPI**|http://conferenceapi.azurewebsites.net?format=json|Odkazuje na službu implementace rozhraní API. Rozhraní API správy předá požadavky na tuto adresu.|
    |**Zobrazovaný název**|*Rozhraní API konferenční Demo*|Pokud jste po zadání adresy URL služby stiskněte klávesu tab, APIM vyplní toto pole, podle toho, co je v kódu json. <br/>Tento název se zobrazí na portálu pro vývojáře.|
    |**Název**|*demo-conference-api*|Poskytuje jedinečný název pro rozhraní API. <br/>Pokud jste po zadání adresy URL služby stiskněte klávesu tab, APIM vyplní toto pole, podle toho, co je v kódu json.|
    |**Popis**|Zadejte volitelný popis rozhraní API.|Pokud jste po zadání adresy URL služby stiskněte klávesu tab, APIM vyplní toto pole, podle toho, co je v kódu json.|
    |**Přípona adresy URL rozhraní API**|*conference*|Přípona se připojuje k základní adresu URL pro službu správy rozhraní API. API Management odlišuje rozhraní API podle jejich přípona a proto přípona musí být jedinečný pro každé rozhraní API pro daného vydavatele.|
    |**Schéma adresy URL**|*HTTPS*|Určuje protokoly, které lze použít pro přístup k rozhraní API. |
    |**Produkty**|*Unlimited*| Rozhraní API publikujete tím, že přidružíte rozhraní API s produktem. Volitelně můžete přidat toto nové rozhraní API pro určitý produkt, zadejte název produktu. Tento krok můžete přidat rozhraní API k více produktům opakuje vícekrát.<br/>Produkty jsou přidružení jeden nebo více rozhraní API. Může obsahovat několik rozhraní API a poskytněte jim pro vývojáře prostřednictvím portálu pro vývojáře. Vývojáři musí nejdřív přihlásit k odběru produktu získat přístup k rozhraní API. Pokud se přihlášení k odběru získají předplatné klíč, který je vhodný pro jakéhokoli rozhraní API v produktu. Pokud jste vytvořili instanci APIM, jste správcem již, takže jsou přihlášení k odběru každého produktu ve výchozím nastavení.<br/> Ve výchozím nastavení, každá instance API Management obsahuje dva ukázkové produkty: **Starter** a **neomezený**. |
3. Vyberte **Vytvořit**.

## <a name="test-the-new-apim-api-in-the-azure-portal"></a>Testování nového rozhraní API APIM na portálu Azure

Operace lze volat přímo z portálu Azure, který představuje pohodlný způsob pro zobrazení a testování operací v rozhraní API.  
1. Vyberte rozhraní API, které jste vytvořili v předchozím kroku.
2. Stiskněte **Test** kartě.  ![Rozhraní API testu](./media/api-management-get-started/test-api.png)
3. Klikněte na **GetSpeakers**.
    Na stránce se zobrazí pole pro parametry dotazu, ale v takovém případě nemáme žádné. Na stránce také zobrazuje pole hlavičky. Jednu z hlaviček "Ocp-Apim-Subscription-Key", je pro předplatné klíč produktu, který je přidružen toto rozhraní API. Klíč se vyplní automaticky.
4. Stiskněte **Odeslat**.

    Back-end odpoví **200 OK** a některá data.

## <a name="call-operation"></a>Volání operace z portálu pro vývojáře

Operace lze také volat **portál pro vývojáře** k testování rozhraní API. 

1. Vybrat rozhraní API, kterou jste vytvořili v "Import a publikovat rozhraní API back-end" krok.
2. Stiskněte klávesu **portál pro vývojáře**.

    ![Testování v portálu pro vývojáře](./media/api-management-get-started/developer-portal.png)

    Otevře webu "Portál pro vývojáře".
3. Vyberte **rozhraní API**.
4. Vyberte **ukázku konferenční rozhraní API**.
5. Klikněte na tlačítko **GetSpeakers**.
    
    Na stránce se zobrazí pole pro parametry dotazu, ale v takovém případě nemáme žádné. Na stránce také zobrazuje pole hlavičky. Jednu z hlaviček "Ocp-Apim-Subscription-Key", je pro předplatné klíč produktu, který je přidružen toto rozhraní API. Pokud jste vytvořili instanci APIM, jste správcem již, tak klíč se vyplní automaticky.
6. Stiskněte klávesu **vyzkoušet**.
7. Stiskněte **Odeslat**.
    
    Po vyvolání operace portál pro vývojáře zobrazí **Stav odpovědi**, **Hlavičky odpovědi** a jakýkoli **Obsah odpovědi**.

## <a name="next-steps"></a>Další kroky

V tomto kurzu jste se naučili:

> [!div class="checklist"]
> * Import vašeho prvního rozhraní API
> * Test rozhraní API na portálu Azure
> * Testování rozhraní API v portálu pro vývojáře

Přechodu na další kurz:

> [!div class="nextstepaction"]
> [Vytvoření a publikování produktu](api-management-howto-add-products.md)
