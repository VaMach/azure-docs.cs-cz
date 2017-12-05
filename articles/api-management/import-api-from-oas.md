---
title: "Import specifikace OpenAPI pomocí portálu Azure | Microsoft Docs"
description: "Zjistěte, jak importovat specifikace OpenAPI s API Management."
services: api-management
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 11/22/2017
ms.author: apimpm
ms.openlocfilehash: f0c77c6e959ca99698b3ea704756a6abf36147f3
ms.sourcegitcommit: b854df4fc66c73ba1dd141740a2b348de3e1e028
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/04/2017
---
# <a name="import-an-openapi-specification"></a>Import specifikace OpenAPI

Tento článek ukazuje, jak importovat rozhraní API back-end "OpenAPI specification", které se nacházejí v http://conferenceapi.azurewebsites.net?format=json. Toto rozhraní API back-end je od společnosti Microsoft a je hostovaná v Azure. Článek také ukazuje, jak otestovat APIM rozhraní API.

V tomto článku se dozvíte, jak:

> [!div class="checklist"]
> * Import rozhraní API back-end "OpenAPI specification"
> * Test rozhraní API na portálu Azure
> * Testování rozhraní API v portálu pro vývojáře

## <a name="prerequisites"></a>Požadavky

Dokončete následující rychlý start: [vytvoření instance služby Azure API Management](get-started-create-service-instance.md)

[!INCLUDE [api-management-navigate-to-instance.md](../../includes/api-management-navigate-to-instance.md)]

## <a name="create-api"></a>Importu a publikovat rozhraní API back-end

1. Vyberte **rozhraní API** z pod **API MANAGEMENT**.
2. Vyberte **OpenAPI specifikace** z **přidání nového rozhraní API** seznamu.
    ![Specifikace OpenAPI](./media/import-api-from-oas/oas-api.png)
3. Zadejte odpovídající nastavení. Všechny hodnoty rozhraní API můžete nastavit při vytváření. Alternativně můžete nastavit některé z nich později přechodem na **nastavení** kartě. <br/> Pokud vyberete **kartě** některé (nebo všechny) pole získat naplněna s informacemi ze zadané služby back-end.

    ![Vytvoření rozhraní API](./media/api-management-get-started/create-api.png)

    |Nastavení|Hodnota|Popis|
    |---|---|---|
    |**Specifikace OpenAPI**|http://conferenceapi.azurewebsites.NET?Format=JSON|Odkazuje na službu implementace rozhraní API. Rozhraní API správy předá požadavky na tuto adresu.|
    |**Zobrazovaný název**|*Rozhraní API konferenční Demo*|Pokud jste po zadání adresy URL služby stiskněte klávesu tab, APIM vyplní toto pole, podle toho, co je v kódu json. <br/>Tento název se zobrazí na portálu pro vývojáře.|
    |**Název**|*Demo konference – rozhraní api*|Poskytuje jedinečný název pro rozhraní API. <br/>Pokud jste po zadání adresy URL služby stiskněte klávesu tab, APIM vyplní toto pole, podle toho, co je v kódu json.|
    |**Popis**|Zadejte volitelný popis rozhraní API.|Pokud jste po zadání adresy URL služby stiskněte klávesu tab, APIM vyplní toto pole, podle toho, co je v kódu json.|
    |**Přípona adresy URL rozhraní API**|*konference*|Přípona se připojuje k základní adresu URL pro službu správy rozhraní API. API Management odlišuje rozhraní API podle jejich přípona a proto přípona musí být jedinečný pro každé rozhraní API pro daného vydavatele.|
    |**Schéma adresy URL**|*PROTOKOL HTTPS*|Určuje protokoly, které lze použít pro přístup k rozhraní API. |
    |**Produkty**|*Unlimited*| Rozhraní API publikujete tím, že přidružíte rozhraní API s produktem. Volitelně můžete přidat toto nové rozhraní API pro určitý produkt, zadejte název produktu. Tento krok můžete přidat rozhraní API k více produktům opakuje vícekrát.<br/>Produkty jsou přidružení jeden nebo více rozhraní API. Může obsahovat několik rozhraní API a poskytněte jim pro vývojáře prostřednictvím portálu pro vývojáře. Vývojáři musí nejdřív přihlásit k odběru produktu získat přístup k rozhraní API. Pokud se přihlášení k odběru získají předplatné klíč, který je vhodný pro jakéhokoli rozhraní API v produktu. Pokud jste vytvořili instanci APIM, jste správcem již, takže jsou přihlášení k odběru každého produktu ve výchozím nastavení.<br/> Ve výchozím nastavení, každá instance API Management obsahuje dva ukázkové produkty: **Starter** a **neomezený**. |

4. Vyberte **Vytvořit**.

## <a name="test-the-new-apim-api-in-the-azure-portal"></a>Testování nového rozhraní API APIM na portálu Azure

Operace lze volat přímo z portálu Azure, který představuje pohodlný způsob pro zobrazení a testování operací v rozhraní API.  

1. Vyberte rozhraní API, které jste vytvořili v předchozím kroku.
2. Stiskněte **Test** kartě.

    ![Rozhraní API testu](./media/api-management-get-started/test-api.png)
1. Klikněte na **GetSpeakers**.

    Na stránce se zobrazí pole pro parametry dotazu, ale v takovém případě nemáme žádné. Na stránce také zobrazuje pole hlavičky. Jednu z hlaviček "Ocp-Apim-Subscription-Key", je pro předplatné klíč produktu, který je přidružen toto rozhraní API. Pokud jste vytvořili instanci APIM, jste správcem již, tak klíč se vyplní automaticky. 
4. Stiskněte klávesu **odeslat**.

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
7. Stiskněte klávesu **odeslat**.
    
    Po vyvolání operace portál pro vývojáře zobrazí **Stav odpovědi**, **Hlavičky odpovědi** a jakýkoli **Obsah odpovědi**.

[!INCLUDE [api-management-navigate-to-instance.md](../../includes/api-management-append-apis.md)]

[!INCLUDE [api-management-define-api-topics.md](../../includes/api-management-define-api-topics.md)]

## <a name="next-steps"></a>Další kroky

> [!div class="nextstepaction"]
> [Transformace a chránit publikované rozhraní API](transform-api.md)
