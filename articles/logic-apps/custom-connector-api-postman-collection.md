---
title: "Popisují vlastní rozhraní API a konektorů s Postman - Azure Logic Apps | Microsoft Docs"
description: "Vytváření kolekcí Postman popsat, skupiny a uspořádání vlastní rozhraní API a konektorů"
author: ecfan
manager: anneta
editor: 
services: logic-apps
documentationcenter: 
ms.assetid: 
ms.service: logic-apps
ms.workload: logic-apps
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/22/2017
ms.author: LADocs; estfan
ms.openlocfilehash: 072d544e5d29c4abb3d69651e53cfb33d5e0c9e9
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="describe-custom-apis-and-custom-connectors-with-postman"></a>Popisují vlastní rozhraní API a vlastní konektory s Postman

Chcete-li vývoj [vlastní rozhraní API](../logic-apps/logic-apps-create-api-app.md) a [vlastní konektory](../logic-apps/custom-connector-overview.md) rychlejší a snadnější, můžete vytvořit [Postman](https://www.getpostman.com/) kolekcí, nikoli OpenAPI dokumenty, které popisují vaše Rozhraní API a konektorů. Postman kolekce nápovědy uspořádání a seskupení souvisejících s žádostí o rozhraní API. Můžete například Postman při vytváření konektory pro Azure Logic Apps, Microsoft Flow nebo PowerApps společnosti Microsoft. 

Tento kurz ukazuje, jak vytvořit [Postman kolekce](https://www.getpostman.com/docs/postman/collections/creating_collections) pomocí [zjistit rozhraní API jazyka](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/56f30ceeeda5650db055a3c7) v [Azure Analýza textu kognitivní služby](https://azure.microsoft.com/services/cognitive-services/text-analytics/) jako v příkladu. Toto rozhraní API identifikuje jazyka, postojích a klíče frází jako text, který je předat do rozhraní API.

## <a name="prerequisites"></a>Požadavky

* Pokud jste ještě Postman, [nainstalujte aplikaci Postman](https://www.getpostman.com/apps).

* Předplatné Azure. Pokud nemáte předplatné, můžete začít s [bezplatný účet Azure](https://azure.microsoft.com/free/). Jinak, zaregistrujte si [předplatné s průběžnými platbami](https://azure.microsoft.com/pricing/purchase-options/).

* Pokud máte předplatné Azure, zaregistrujte se pro rozhraní API, analýza textu pomocí [úloze 1 zde](../cognitive-services/text-analytics/how-tos/text-analytics-how-to-signup.md). 

## <a name="create-a-postman-collection"></a>Vytvořte kolekci Postman

Před vytvořením kolekce, vytvořte požadavek HTTP pro svůj koncový bod rozhraní API. 

### <a name="create-an-http-request-for-your-api"></a>Vytvořit požadavek HTTP pro vaše rozhraní API

1. Otevřete aplikaci Postman, takže můžete vytvořit [požadavek HTTP](https://www.getpostman.com/docs/postman/sending_api_requests/requests) pro svůj koncový bod rozhraní API. Další informace najdete v tématu Postman [požadavky dokumentaci](https://www.getpostman.com/docs/postman/sending_api_requests/requests).

   1. Na **Tvůrce** , vyberte metodu HTTP, zadejte adresu URL požadavku pro koncový bod rozhraní API a vyberte ověřovací protokol, pokud existuje. 
   Až budete připraveni, zvolte **parametry**.

      V tomto kurzu můžete použít tato nastavení v tomto příkladu:

      ![Vytvořit žádost o: "Metoda HTTP", "URL požadavku", "Autorizace"](./media/custom-connector-api-postman-collection/01-create-api-http-request.png)

      | Parametr | Navrhovaná hodnota | 
      | --------- | --------------- | 
      | **Metoda HTTP** | POST | 
      | **Adresa URL požadavku** | `https://westus.api.cognitive.microsoft.com/text/analytics/v2.0/languages` | | 
      | **Autorizace** | "Žádné ověření" | | 
      ||| 

   2. Teď můžete zadat páry klíč hodnota, které chcete použít jako parametry dotazu nebo cestu v adrese URL žádosti. Tyto položky postman společně spojuje do řetězce dotazu.
   Až budete hotoví, zvolte **hlavičky**.

      ![Žádost o dál: "Parametry"](./media/custom-connector-api-postman-collection/02-create-api-http-request-params.png)

      | Parametr | Navrhovaná hodnota | 
      | --------- | --------------- | 
      | **Parametry** | **Klíč**: "numberOfLanguagesToDetect" </br>**Hodnota**: "1" | 
      ||| 

   3. Zadejte páry klíč hodnota hlavičky žádosti. 
   Pro název hlavičky zadejte všechny řetězec, který chcete. Pro společné hlavičky HTTP můžete vybrat z rozevíracího seznamu. Až budete hotoví, zvolte **textu**. 
   
      ![Žádost o dál: "Hlavičky"](./media/custom-connector-api-postman-collection/03-create-api-http-request-header.png)

      | Parametr | Hodnota | 
      | --------- | ----- | 
      | **Záhlaví** | **Klíč**: "Ocp-Apim-Subscription-Key" </br>**Hodnota**: *vaše API-subscription-key*, které můžete najít ve vašem účtu kognitivní služby <p>**Klíč**: "Content-Type" </br> **Hodnota**: "application/json" | 
      ||| 

   4. Zadejte obsah, který chcete odeslat v textu požadavku. 
   Pokud chcete zkontrolovat, že žádost funguje tak, že získávání odpověď zpět, zvolte **odeslat**. 
   
      ![Žádost o dál: "Text"](./media/custom-connector-api-postman-collection/04-create-api-http-request-body.png)

      | Parametr | Navrhovaná hodnota | 
      | --------- | --------------- |    
      | **Text** | ```{"documents": [{ "id": "1", "text": "Hello World"}]}``` | 
      ||| 

      Pole odpovědi obsahuje úplnou odpověď z rozhraní API, včetně výsledek nebo došlo k chybě, pokud existuje.

      ![Získání odezvy požadavku](./media/custom-connector-api-postman-collection/05-create-api-http-request-response.png)

2. Po jste jste zkontrolovali, funguje vaše žádost o uložit do kolekce Postman vaši žádost. 

   1. Zvolte **Uložit**. 
      
      ![Zvolte "Uložit"](./media/custom-connector-api-postman-collection/06a-save-request.png)
 
   2. V části **uložení žádosti o**, poskytovat **název požadavku** a volitelně **požadavku popis**. 

      > [!NOTE]
      > Tyto hodnoty používá vlastní konektor později. Proto se ujistěte, že zadáte stejné hodnoty, které použijete později pro operaci souhrnné a popis vlastní rozhraní API.

   3. Zvolte **+ vytvoření kolekce** a zadejte název kolekce. 
   Zvolte zaškrtávací značku, která vytvoří složku kolekce, pak zvolte **uložit do *vaše Postman kolekce name***.

      ![Uložení žádosti](./media/custom-connector-api-postman-collection/06b-save-request.png)

### <a name="save-the-request-response"></a>Uložit odpověď na žádost

Po uložení vaši žádost, můžete uložit do odpovědi. Tímto způsobem odpovědi se zobrazí jako příklad při načítání žádost později.

1. Výše okno odpovědi zvolte **uložit odpovědi**. 

   ![Uložit odpovědi](./media/custom-connector-api-postman-collection/07-create-api-http-request-save-response.png)

   Vlastní konektory podporují pouze jedna odpověď na žádost. 
   Pokud jste uložili více odpovědí na žádost, použije se pouze první z nich.

2. Zadejte název vaší příklad a vyberte **uložit příklad**.

3. Pokračujte ve vytváření kolekce Postman s žádné další požadavky a odpovědi.

### <a name="export-your-postman-collection"></a>Exportovat kolekci Postman

1. Když jste hotovi, exportujte do souboru JSON vaší kolekce.

   ![Exportovat kolekci](./media/custom-connector-api-postman-collection/08-export-http-request.png)

2. Vyberte **kolekce v1** exportovat formátu a přejděte do umístění, kam chcete uložit soubor JSON.

   > [!NOTE]
   > V současné době pouze V1 funguje pro vlastní konektory.

   ![Vyberte formát exportu: "Kolekce v1"](./media/custom-connector-api-postman-collection/09-export-format.png)
   
Teď můžete tuto kolekci Postman pro vytvoření vlastní rozhraní API a konektorů. Po exportu kolekce můžete importovat soubor JSON do aplikace logiky, tok nebo PowerApps.

> [!IMPORTANT]
> Pokud vytvoříte vlastní konektor z kolekce Postman, ujistěte se, že odeberete `Content-type` záhlaví z akce a aktivační události. Cílová služba, například tok, automaticky přidá hlavičku. Odeberte také ověřovacím hlavičkám v `securityDefintions` části, akce a aktivační události.

## <a name="next-steps"></a>Další kroky

* [Služba Logic Apps: Zaregistrovat vlastní konektory](../logic-apps/logic-apps-custom-connector-register.md)
* [Postup: Registrace vaší konektoru](https://ms.flow.microsoft.com/documentation/register-custom-api/#register-your-custom-connector)
* [PowerApps: Registrace vaší konektoru](https://powerapps.microsoft.com/tutorials/register-custom-api/#register-your-custom-connector)
* [Vlastní konektor – nejčastější dotazy](../logic-apps/custom-connector-faq.md)
