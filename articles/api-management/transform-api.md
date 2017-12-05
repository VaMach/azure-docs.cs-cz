---
title: "Transformace a chránit vaše rozhraní API pomocí Azure API Management | Microsoft Docs"
description: "Seznamte se s možnostmi ochrany rozhraní API pomocí zásad kvót a zásad omezování četnosti."
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
ms.date: 11/19/2017
ms.author: apimpm
ms.openlocfilehash: 772f3828d85c54e7b8bb44c857e555175b7444cc
ms.sourcegitcommit: b854df4fc66c73ba1dd141740a2b348de3e1e028
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/04/2017
---
# <a name="transform-and-protect-your-api"></a>Transformace a chránit vaše rozhraní API 

Tento kurz ukazuje, jak k transformaci vaše rozhraní API, takže neodhalí informace privátní back-end. Například můžete chtít skrýt informace o zásobníku technologie, která běží na back-end. Také můžete chtít skrýt původní adresy URL, které vložit do těla odpovědi HTTP rozhraní API a přesměruje je místo k bráně APIM.

V tomto kurzu také ukazuje, jak snadné je přidat ochranu rozhraní API back-endu tím, že nakonfigurujete omezení četnosti pomocí Azure API Management. Například můžete chtít omezit počet volání rozhraní API je volána, takže ji není Nepromyšlené vývojáři. Další informace najdete v tématu [zásady služby API Management](api-management-policies.md)

V tomto kurzu se naučíte:

> [!div class="checklist"]
> * Transformuje rozhraní API pro pruhu hlavičky odpovědi
> * Nahradí původní adresy URL do těla odpovědi rozhraní API s adresami URL APIM brány
> * Ochrana rozhraní API přidáním zásady omezení četnosti (omezení)
> * Otestovat transformace

![Zásady](./media/transform-api/api-management-management-console.png)

## <a name="prerequisites"></a>Požadavky

+ Dokončete následující rychlý start: [vytvoření instance služby Azure API Management](get-started-create-service-instance.md).
+ Navíc kurzu: [importu a publikování vašeho prvního rozhraní API](import-and-publish.md).
 
[!INCLUDE [api-management-navigate-to-instance.md](../../includes/api-management-navigate-to-instance.md)]

## <a name="transform-an-api-to-strip-response-headers"></a>Transformuje rozhraní API pro pruhu hlavičky odpovědi

V této části ukazuje, jak chcete skrýt hlavičky HTTP, které nechcete zobrazit uživatelům. V tomto příkladu jsou tyto hlavičky odstraněný v odpovědi HTTP:

* **X-používá technologii podle**
* **Verze X AspNet**

### <a name="test-the-original-response"></a>Testování původní odpověď

Chcete-li zobrazit původní odpovědi:

1. Vyberte **rozhraní API** kartě.
2. Klikněte na tlačítko **ukázku konferenční API** ze seznamu rozhraní API.
3. Vyberte **GetSpeakers** operaci.
4. Klikněte **Test** karta nahoře na obrazovce.
5. Stiskněte **odeslat** tlačítko v dolní části obrazovky. 

    Jak můžete vidět původní odpověď vypadá takto:

    ![Zásady](./media/transform-api/original-response.png)

### <a name="set-the-transformation-policy"></a>Nastavení zásad transformace

1. Přejděte k vaší instanci APIM.
2. Vyberte **rozhraní API** kartě.
3. Klikněte na tlačítko **ukázku konferenční API** ze seznamu rozhraní API.
4. Vyberte **všechny operace**.
5. Nahoře na obrazovce vyberte **návrhu** kartě.
6. V **odchozí zpracování** okno, klepněte na trojúhelník (vedle tužky).
7. Vyberte **editor kódu**.
    
     ![Upravit zásady](./media/set-edit-policies/set-edit-policies01.png)
9. Umístěte kurzor uvnitř  **<outbound>**  element.
10. V okně vpravo pod **zásad transformace**, klikněte na tlačítko **+ hlavičky protokolu HTTP sadu** dvakrát (Chcete-li vložit dva fragmenty zásad).

    ![Zásady](./media/transform-api/transform-api.png)
11. Upravit vaše  **<outbound>**  kód vypadat takto:

        <set-header name="X-Powered-By" exists-action="delete" />
        <set-header name="X-AspNet-Version" exists-action="delete" />
                
## <a name="replace-original-urls-in-the-body-of-the-api-response-with-apim-gateway-urls"></a>Nahradí původní adresy URL do těla odpovědi rozhraní API s adresami URL APIM brány

V této části ukazuje, jak chcete skrýt původní adresy URL, které vložit do těla odpovědi HTTP rozhraní API a přesměruje je místo k bráně APIM.

### <a name="test-the-original-response"></a>Testování původní odpověď

Chcete-li zobrazit původní odpovědi:

1. Vyberte **rozhraní API** kartě.
2. Klikněte na tlačítko **ukázku konferenční API** ze seznamu rozhraní API.
3. Vyberte **GetSpeakers** operaci.
4. Klikněte **Test** karta nahoře na obrazovce.
5. Stiskněte **odeslat** tlačítko v dolní části obrazovky. 

    Jak můžete vidět původní odpověď vypadá takto:

    ![Zásady](./media/transform-api/original-response2.png)

### <a name="set-the-transformation-policy"></a>Nastavení zásad transformace

1. Přejděte k vaší instanci APIM.
2. Vyberte **rozhraní API** kartě.
3. Klikněte na tlačítko **ukázku konferenční API** ze seznamu rozhraní API.
4. Vyberte **všechny operace**.
5. Nahoře na obrazovce vyberte **návrhu** kartě.
6. V **odchozí zpracování** okno, klepněte na trojúhelník (vedle tužky).
7. Vyberte **editor kódu**.
8. Umístěte kurzor uvnitř  **<outbound>**  element.
9. V okně vpravo pod **zásad transformace**, klikněte na tlačítko **+ najít a nahradit řetězec v textu**.
10. Upravit vaše **< hledání a nahrazování** kód (v  **<outbound>**  element) k nahraďte adresu URL tak, aby odpovídaly APIM bránu. Například:

        <find-and-replace from="://conferenceapi.azurewebsites.net" to="://apiphany.azure-api.net/conference"/>

## <a name="protect-an-api-by-adding-rate-limit-policy-throttling"></a>Ochrana rozhraní API přidáním zásady omezení četnosti (omezení)

V této části ukazuje, jak přidat ochranu rozhraní API back-endu tím, že nakonfigurujete omezení přenosové rychlosti. Například můžete chtít omezit počet volání rozhraní API je volána, takže ji není Nepromyšlené vývojáři. V tomto příkladu limit je nastaven na 3 volání za 15 sekund pro každé předplatné ID. Po 15 sekund můžete zkusit vývojář volání rozhraní API.

1. Přejděte k vaší instanci APIM.
2. Vyberte **rozhraní API** kartě.
3. Klikněte na tlačítko **ukázku konferenční API** ze seznamu rozhraní API.
4. Vyberte **všechny operace**.
5. Nahoře na obrazovce vyberte **návrhu** kartě.
6. V **zpracování příchozích** okno, klepněte na trojúhelník (vedle tužky).
7. Vyberte **editor kódu**.
8. Umístěte kurzor uvnitř  **<inbound>**  element.
9. V okně vpravo pod **omezení zásady přístupu**, klikněte na tlačítko **+ omezení četnosti volání podle klíče**.
10. Upravit vaše **< rate-limit-by-key** kód (v  **<inbound>**  element) pro následující kód:

        <rate-limit-by-key calls="3" renewal-period="15" counter-key="@(context.Subscription.Id)" />

## <a name="test-the-transformations"></a>Otestovat transformace
        
V tomto okamžiku vaše zásady kódu vypadá takto:

    <policies>
        <inbound>
            <rate-limit-by-key calls="3" renewal-period="15" counter-key="@(context.Subscription.Id)" />
            <base />
        </inbound>
        <backend>
            <base />
        </backend>
        <outbound>
            <set-header name="X-Powered-By" exists-action="delete" />
            <set-header name="X-AspNet-Version" exists-action="delete" />
            <find-and-replace from="://conferenceapi.azurewebsites.net" to="://apiphany.azure-api.net/conference"/>
            <base />
        </outbound>
        <on-error>
            <base />
        </on-error>
    </policies>

Zbývající část tohoto oddílu testy zásad transformace, které jste nastavili v tomto článku.

### <a name="test-the-stripped-response-headers"></a>Testování hlavičky odřapíkovaného odpovědi

1. Přejděte k vaší instanci APIM.
2. Vyberte **rozhraní API** kartě.
3. Klikněte na tlačítko **ukázku konferenční API** ze seznamu rozhraní API.
4. Klikněte **GetSpeakers** operaci.
5. Vyberte **Test** kartě.
6. Stiskněte klávesu **odeslat**.

    Jak vidíte, že hlavičky bylo provedeno oříznutí:

    ![Zásady](./media/transform-api/final-response1.png)

### <a name="test-the-replaced-url"></a>Testování nahrazené adresy URL

1. Přejděte k vaší instanci APIM.
2. Vyberte **rozhraní API** kartě.
3. Klikněte na tlačítko **ukázku konferenční API** ze seznamu rozhraní API.
4. Klikněte **GetSpeakers** operaci.
5. Vyberte **Test** kartě.
6. Stiskněte klávesu **odeslat**.

    Jak můžete vidět adresa URL byla nahrazena.

    ![Zásady](./media/transform-api/final-response2.png)

### <a name="test-the-rate-limit-throttling"></a>Testování omezení četnosti (omezení)

1. Přejděte k vaší instanci APIM.
2. Vyberte **rozhraní API** kartě.
3. Klikněte na tlačítko **ukázku konferenční API** ze seznamu rozhraní API.
4. Klikněte **GetSpeakers** operaci.
5. Vyberte **Test** kartě.
6. Stiskněte klávesu **odeslat** třikrát v řádku.

    Po odeslání žádosti 3krát, získáte **429 příliš mnoho požadavků** odpovědi.
7. Počkejte 15 sekund nebo tak a stiskněte klávesu **odeslat** znovu. Tento čas, měli byste obdržet **200 OK** odpovědi.

    ![Omezování](./media/transform-api/test-throttling.png)

## <a name="video"></a>Video

> [!VIDEO https://channel9.msdn.com/Blogs/AzureApiMgmt/Rate-Limits-and-Quotas/player]
> 
> 

## <a name="next-steps"></a>Další kroky

V tomto kurzu jste se naučili:

> [!div class="checklist"]
> * Transformuje rozhraní API pro pruhu hlavičky odpovědi
> * Nahradí původní adresy URL do těla odpovědi rozhraní API s adresami URL APIM brány
> * Ochrana rozhraní API přidáním zásady omezení četnosti (omezení)
> * Otestovat transformace

Přechodu na další kurz:

> [!div class="nextstepaction"]
> [Sledovat vaše rozhraní API](api-management-howto-use-azure-monitor.md)
