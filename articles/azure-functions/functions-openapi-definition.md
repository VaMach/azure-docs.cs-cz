---
title: "Vytvořit definici OpenAPI pro funkci | Microsoft Docs"
description: "Vytvořte definici OpenAPI, která umožňuje jiným aplikacím a službám volání funkce v Azure."
services: functions
keywords: "OpenAPI, Swagger, cloudové aplikace, cloudové služby"
documentationcenter: 
author: mgblythe
manager: cfowler
editor: 
ms.assetid: 
ms.service: functions
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 08/25/2017
ms.author: mblythe; glenga
ms.custom: mvc
ms.openlocfilehash: a196df5b4ab47b234b48594da45cd4d72f604086
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="create-an-openapi-definition-for-a-function"></a>Vytvořit definici OpenAPI pro funkci
Rozhraní REST API popsané často pomocí definici OpenAPI (dříve označovaný jako [Swagger](http://swagger.io/) souboru). Tato definice obsahuje informace o jaké operace jsou k dispozici v rozhraní API a jak by měla strukturovaná data požadavku a odpovědi pro rozhraní API.

V tomto kurzu vytvoříte funkci, která určuje, zda je nákladově efektivní nouzové opravy na turbínu větru. Pak vytvoříte definici OpenAPI pro funkce aplikace tak, aby funkce lze volat z jiných aplikací a služeb.

V tomto kurzu se naučíte:

> [!div class="checklist"]
> * Vytvoří funkci, v Azure
> * Generovat definici OpenAPI pomocí nástrojů OpenAPI
> * Upravit definici k poskytování dalších metadat
> * Testování definici voláním funkce

## <a name="create-a-function-app"></a>Vytvoření Function App

K hostování provádění funkcí musíte mít aplikaci Function App. Umožňuje aplikaci funkce skupiny funkce jako jednotku logiku pro snadnější správu, nasazení, škálování a sdílení prostředků. 

[!INCLUDE [Create function app Azure portal](../../includes/functions-create-function-app-portal.md)]

[!INCLUDE [functions-portal-favorite-function-apps](../../includes/functions-portal-favorite-function-apps.md)]


## <a name="create-the-function"></a>Vytvoření funkce

Tento kurz používá protokolu HTTP aktivované funkce, která přebírá dva parametry: odhadovaný čas vytvořit turbínu opravit (v hodinách); a kapacitu turbína (v kilowatthodinách). Funkce pak vypočítá, budou náklady na tom, kolik opravy, a kolik výnosy turbíně může změnit v období 24 hodin.

1. Rozbalte funkce aplikace, klikněte na tlačítko  **+**  vedle položky **funkce**, klikněte na tlačítko **HTTPTrigger** šablony. Zadejte `TurbineRepair` pro funkci **název** a klikněte na tlačítko **vytvořit**.

    ![Okno aplikace funkce, funkce +](media/functions-openapi-definition/add-function.png)

1. Nahraďte obsah souboru run.csx následující kód a potom klikněte na **Uložit**:

    ```c#
    using System.Net;

    const double revenuePerkW = 0.12; 
    const double technicianCost = 250; 
    const double turbineCost = 100;

    public static async Task<HttpResponseMessage> Run(HttpRequestMessage req, TraceWriter log)
    {   

        //Get request body
        dynamic data = await req.Content.ReadAsAsync<object>();
        int hours = data.hours;
        int capacity = data.capacity;

        //Formulas to calculate revenue and cost
        double revenueOpportunity = capacity * revenuePerkW * 24;  
        double costToFix = (hours * technicianCost) +  turbineCost;
        string repairTurbine;

        if (revenueOpportunity > costToFix){
            repairTurbine = "Yes";
        }
        else {
            repairTurbine = "No";
        }

        return req.CreateResponse(HttpStatusCode.OK, new{
            message = repairTurbine,
            revenueOpportunity = "$"+ revenueOpportunity,
            costToFix = "$"+ costToFix         
        }); 
    }
    ```
    Tento kód funkce vrátí zprávě `Yes` nebo `No` označíte, zda nouzové opravy je nákladově efektivní, a také možnost výnosy představující turbíně a náklady opravit turbíně. 

1. Chcete-li otestovat funkci, klikněte na tlačítko **testování** na pravém rozbalte karta testu. Zadejte následující hodnotu pro **text žádosti**a potom klikněte na **spustit**.

    ```json
    {
    "hours": "6",
    "capacity": "2500"
    }
    ```

    ![Testování funkce na portálu Azure](media/functions-openapi-definition/test-function.png)

    Následující hodnota je vrácena v textu odpovědi.

    ```json
    {"message":"Yes","revenueOpportunity":"$7200","costToFix":"$1600"}
    ```

Nyní máte funkci, která určuje nákladová efektivnost nouzový opravy. Dále generovat a upravit definici OpenAPI pro funkce aplikace.

## <a name="generate-the-openapi-definition"></a>Generovat definici OpenAPI

Nyní můžete generovat definici OpenAPI. Tuto definici mohou být využita jinými technologiemi společnosti Microsoft, jako jsou aplikace API [PowerApps](functions-powerapps-scenario.md) a [Microsoft Flow](../azure-functions/app-service-export-api-to-powerapps-and-flow.md), jak dobře třetí strany nástroje pro vývojáře, například [Postman](https://www.getpostman.com/docs/importing_swagger) a [mnoho další balíčky](http://swagger.io/tools/).

1. Vyberte pouze *příkazy* , podporuje rozhraní API (v tomto případu příspěvku). Díky tomu čisticí generovaného definice rozhraní API.

    1. Na **integrací** kartě novou funkci triggeru protokolu HTTP, změna **metody HTTP povoleno** k **vybrané metody**

    1. V **metody HTTP vybrané**, zrušte zaškrtnutí políčka, každý s výjimkou **POST**.

        ![Vybrané metody HTTP](media/functions-openapi-definition/selected-http-methods.png)
        
1. Klikněte na název aplikace – funkce (jako je **funkce. ukázku energie**) > **funkce** > **definice rozhraní API**.

    ![Definice rozhraní API.](media/functions-openapi-definition/api-definition.png)

1. Na **definice rozhraní API** , klikněte na **funkce**.

    ![Zdroj definice rozhraní API](media/functions-openapi-definition/api-definition-source.png)

    Tento krok povoluje sada možností OpenAPI pro funkce aplikace, včetně koncový bod pro hostování soubor OpenAPI z domény aplikaci funkce, vložené kopie [OpenAPI Editor](http://editor.swagger.io)a generátor šablony definice rozhraní API.

1. Klikněte na tlačítko **šablony definice rozhraní API generovat** > **Uložit**.

    ![Generovat šablonu definice rozhraní API](media/functions-openapi-definition/generate-template.png)

    Azure kontroluje funkce aplikace pro funkce triggeru protokolu HTTP a používá informace v functions.json k vygenerování definici OpenAPI. Zde je definice, aby se vygenerovala:

    ```yaml
    swagger: '2.0'
    info:
    title: function-demo-energy.azurewebsites.net
    version: 1.0.0
    host: function-demo-energy.azurewebsites.net
    basePath: /
    schemes:
    - https
    - http
    paths:
    /api/TurbineRepair:
        post:
        operationId: /api/TurbineRepair/post
        produces: []
        consumes: []
        parameters: []
        description: >-
            Replace with Operation Object
            #http://swagger.io/specification/#operationObject
        responses:
            '200':
            description: Success operation
        security:
            - apikeyQuery: []
    definitions: {}
    securityDefinitions:
    apikeyQuery:
        type: apiKey
        name: code
        in: query
    ```

    Tuto definici se označuje jako _šablony_ protože vyžaduje další metadata, aby se úplná OpenAPI definice. Upravte definici v dalším kroku.

## <a name="modify-the-openapi-definition"></a>Upravit definici OpenAPI
Teď, když máte definice šablony, upravte ji, aby poskytovala další metadata týkající se operace rozhraní API a datové struktury. V **definice rozhraní API**, odstraňte generovaného definice z `post` k dolnímu okraji definici, vložte obsah níže a klikněte na tlačítko **Uložit**.

```yaml
    post:
      operationId: CalculateCosts
      description: Determines if a technician should be sent for repair
      summary: Calculates costs
      x-ms-summary: Calculates costs
      x-ms-visibility: important
      produces:
        - application/json
      consumes:
        - application/json
      parameters:
        - name: body
          in: body
          description: Hours and capacity used to calculate costs
          x-ms-summary: Hours and capacity
          x-ms-visibility: important
          required: true
          schema:
            type: object
            properties:
              hours:
                description: The amount of effort in hours required to conduct repair
                type: number
                x-ms-summary: Hours
                x-ms-visibility: important
              capacity:
                description: The max output of a turbine in kilowatts
                type: number
                x-ms-summary: Capacity
                x-ms-visibility: important
      responses:
        200:
          description: Message with cost and revenue numbers
          x-ms-summary: Message
          schema:
           type: object
           properties:
            message:
              type: string
              description: Returns Yes or No depending on calculations
              x-ms-summary: Message 
            revenueOpportunity:
              type: string
              description: The revenue opportunity cost
              x-ms-summary: RevenueOpportunity 
            costToFix:
              type: string
              description: The cost in $ to fix the turbine
              x-ms-summary: CostToFix
      security:
        - apikeyQuery: []
definitions: {}
securityDefinitions:
  apikeyQuery:
    type: apiKey
    name: code
    in: query
```

V takovém případě může jenom vkládání v aktualizované metadata, ale je důležité si uvědomit, typy změny, které jsme provedli pro výchozí šablony:

+ Zadat, že rozhraní API vytváří a používá data ve formátu JSON.

+ Zadat požadované parametry, jejich názvy a datové typy.

+ Zadané návratové hodnoty pro úspěšné odpovědi, s jejich názvy a datové typy.

+ Zadat popisný souhrny a popisy pro rozhraní API a příslušné operace a parametry. To je důležité pro uživatele, kteří budou používat tuto funkci.

+ Přidat x-ms souhrn a x-ms viditelnost, které se používají v uživatelském rozhraní pro Microsoft Flow a Logic Apps. Další informace najdete v tématu [OpenAPI rozšíření pro vlastní rozhraní API v Microsoft Flow](https://preview.flow.microsoft.com/documentation/customapi-how-to-swagger/).

> [!NOTE]
> Definice zabezpečení jsme ponechaná na výchozí metodu ověřování klíč rozhraní API. Tato část definice by změnit, pokud jste použili jiný typ ověřování.

Další informace o definování operace rozhraní API najdete v tématu [specifikace otevřené rozhraní API](https://swagger.io/specification/#operationObject).

## <a name="test-the-openapi-definition"></a>Testování definici OpenAPI
Než použijete definice rozhraní API, je vhodné pro testování v uživatelském rozhraní funkce Azure.

1. Na **spravovat** kartě funkce v části **klíče hostitele**, kopie **výchozí** klíč.

    ![Zkopírujte klíč rozhraní API](media/functions-openapi-definition/copy-api-key.png)

    > [!NOTE]
    >Použít tento klíč pro testování, a můžete ji použít i při volání rozhraní API z aplikace nebo služby.

1. Přejděte zpět na definici rozhraní API: **funkce. ukázku energie** > **funkce** > **definice rozhraní API**.

1. V pravém podokně klikněte na **změna ověřování**, zadejte klíč rozhraní API, který jste zkopírovali a klikněte na tlačítko **ověřit**.

    ![Ověření s klíčem rozhraní API](media/functions-openapi-definition/authenticate-api-key.png)

1. Posuňte se dolů a klikněte na tlačítko **zkuste tuto operaci**.

    ![Zkuste tuto operaci](media/functions-openapi-definition/try-operation.png)

1. Zadejte hodnoty pro **hodin** a **kapacity**.

    ![Zadejte parametry](media/functions-openapi-definition/parameters.png)

    Všimněte si, jak rozhraní používá popisy z definice rozhraní API.

1. Klikněte na tlačítko **poslat žádost o**, klikněte **poměrně** karta výstup zobrazíte.

    ![Odeslání požadavku](media/functions-openapi-definition/send-request.png)

## <a name="next-steps"></a>Další kroky

V tomto kurzu jste se naučili:

> [!div class="checklist"]
> * Vytvoří funkci, v Azure
> * Generovat definici OpenAPI pomocí nástrojů OpenAPI
> * Upravit definici k poskytování dalších metadat
> * Testování definici voláním funkce

Přejít na další téma se dozvíte, jak vytvořit aplikaci PowerApps, která používá OpenAPI definici, kterou jste vytvořili.
> [!div class="nextstepaction"]
> [Volání funkce z PowerApps](functions-powerapps-scenario.md)
