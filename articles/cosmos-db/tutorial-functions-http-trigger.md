---
title: "Vytvořit aktivační událost INSTEAD HTTP s vazbou vstupní Azure Cosmos DB | Microsoft Docs"
description: "Naučte se používat Azure Functions se aktivace protokolu HTTP k dotazování Azure Cosmos DB."
services: cosmos-db
documentationcenter: 
author: mimig1
manager: jhubbard
ms.assetid: 
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: mvc
ms.date: 09/25/2017
ms.author: mimig
ms.openlocfilehash: 86a660309fd3fd80f10f706ff460af2309c12174
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="create-an-azure-functions-http-trigger-with-an-azure-cosmos-db-input-binding"></a>Vytvořit aktivační událost INSTEAD HTTP funkce Azure s vazbou vstupní Azure Cosmos DB

Azure Cosmos DB je globálně distribuované více modelu databáze, která je schemaless i bez serveru. Funkce Azure je bez serveru výpočetní služba, která umožňuje spustit kód na vyžádání. Spárujte si tyto dvě služby Azure a máte základ pro architekturu bez serveru, která umožňuje zaměřit se na vytváření kvalitních aplikací a nestarat se o zřizování a správa serverů pro výpočetní a musí databáze.

V tomto kurzu vychází kód vytvořené v [Graph API rychlý start pro .NET](create-graph-dotnet.md). V tomto kurzu přidá funkce Azure, který obsahuje [triggeru protokolu HTTP](https://github.com/MicrosoftDocs/azure-docs-pr/azure-functions/functions-bindings-http-webhook.md#http-trigger). Aktivace protokolu HTTP používá Azure DB Cosmos [vstupní vazby](https://github.com/MicrosoftDocs/azure-docs-pr/azure-functions/functions-triggers-bindings.md) k načtení dat z databáze grafu vytvořené v rychlý start. Tato konkrétní HTTP aktivační událost dotazy databázi Cosmos Azure pro data, ale vstupní vazby od Azure Cosmos DB umožňuje načíst vstupní hodnoty dat pro ať funkce vyžaduje.

Tento kurz obsahuje následující úlohy:

> [!div class="checklist"]
> * Vytvoření projektu Azure – funkce 
> * Vytvořit aktivační událost INSTEAD HTTP
> * Publikování funkci Azure
> * Připojení k databázi Azure Cosmos DB funkce Azure

## <a name="prerequisites"></a>Požadavky

- [Visual Studio 2017 verze 15.3](https://www.visualstudio.com/vs/preview/), včetně sady funkcí **Vývoj pro Azure**.

    ![Instalace sady Visual Studio 2017 se sadou funkcí Vývoj pro Azure](./media/tutorial-functions-http-trigger/functions-vs-workloads.png)
    
- Po instalaci nebo upgradu na Visual Studio 2017 verze 15.3, je nutné ručně aktualizovat nástroje Visual Studio 2017 pro Azure Functions. Můžete je aktualizovat z nástrojů **nástroje** nabídky v části **rozšíření a aktualizace...**   >  **Aktualizace** > **sady Visual Studio Marketplace** > **Azure Functions a webové úlohy nástroje**  >  **Aktualizace**.

- Dokončení [vytvoření aplikace .NET pomocí rozhraní Graph API](tutorial-develop-graph-dotnet.md) kurzu nebo get příklad kódu z [azure-cosmos-db-graph-dotnet-getting-started](https://github.com/Azure-Samples/azure-cosmos-db-graph-dotnet-getting-started) úložiště GitHub a sestavte projekt.
 
## <a name="build-a-function-in-visual-studio"></a>Vytvoření funkce v sadě Visual Studio

1. Přidat **Azure Functions** projektu a řešení kliknutím pravým tlačítkem na uzel řešení v **Průzkumníku řešení**, zvolte **přidat**  >   **Nový projekt**. Zvolte **Azure Functions** z tohoto dialogového okna pole a pojmenujte ji **PeopleDataFunctions**.

   ![Do řešení přidat projektu Azure – funkce](./media/tutorial-functions-http-trigger/01-add-function-project.png)

2. Můžete po vytvoření projektu Azure Functions, existuje několik NuGet související aktualizuje a nainstaluje k provedení. 

    a. Pokud chcete mít jistotu, máte nejnovější funkce SDK, použít Správce NuGet a aktualizovat **Microsoft.NET.Sdk.Functions** balíčku. V **Průzkumníku řešení**, klikněte pravým tlačítkem na projekt a vyberte **spravovat balíčky NuGet**. V **nainstalovaná** , vyberte Microsoft.NET.Sdk.Functions a potom klikněte na tlačítko **aktualizace**.

   ![Aktualizovat balíčky NuGet](./media/tutorial-functions-http-trigger/02-update-functions-sdk.png)

    b. V **Procházet** zadejte **azure.graphs** najít **Microsoft.Azure.Graphs** balíček a potom klikněte na **nainstalovat**. Tento balíček obsahuje graf rozhraní API .NET klienta SDK.

   ![Nainstalujte rozhraní Graph API](./media/tutorial-functions-http-trigger/03-add-azure-graphs.png)

    c. V **Procházet** zadejte **mono.csharp** najít **Mono.CSharp** balíček a potom klikněte na **nainstalovat**.

   ![Nainstalujte Mono.CSharp](./media/tutorial-functions-http-trigger/04-add-mono.png)

3. Vaše Průzkumníku řešení teď obsahovat balíčky, které jste nainstalovali, jak je vidět tady. 
   
   V dalším kroku musíme napsat kód, takže přidáme nové **funkce Azure** položku do projektu. 

    a. V **Průzkumníku řešení** klikněte pravým tlačítkem na uzel projektu a pak zvolte **Přidat** > **Nová položka**.   
    b. V **přidat novou položku** dialogovém okně, vyberte **Visual C# položky**, vyberte **funkce Azure**, typ **vyhledávání** jako název projektu a potom Klikněte na tlačítko **přidat**.  
 
   ![Vytvořte novou funkci s názvem vyhledávání](./media/tutorial-functions-http-trigger/05-add-function.png)

4. Funkce Azure bude odpovídat na požadavky HTTP, takže tady je vhodné šablony aktivace protokolu Http.
   
   V **nové funkce Azure** vyberte **triggeru protokolu Http**. Tuto funkci Azure jako "celý otevřené,", chceme, abyste nám nastavit **přístupová práva** k **anonymní**, které umožní všem uživatelům. Klikněte na **OK**.

   ![Sada přístupová práva k anonymní](./media/tutorial-functions-http-trigger/06-http-trigger.png)

5. Po přidání Search.cs projektu funkce Azure, zkopírujte tyto **pomocí** příkazy přes existující příkazy using:

   ```csharp
   using Microsoft.Azure.Documents;
   using Microsoft.Azure.Documents.Client;
   using Microsoft.Azure.Documents.Linq;
   using Microsoft.Azure.Graphs;
   using Microsoft.Azure.WebJobs;
   using Microsoft.Azure.WebJobs.Extensions.Http;
   using Microsoft.Azure.WebJobs.Host;
   using System;
   using System.Collections.Generic;
   using System.Configuration;
   using System.Linq;
   using System.Net;
   using System.Net.Http;
   using System.Threading.Tasks;
   ```

6. Kód třídy funkce Azure v dalším kroku nahraďte kód níže. Kód vyhledá databázi Azure Cosmos DB pomocí rozhraní Graph API pro buď všechny uživatele, nebo pro konkrétní osoby identifikovaný `name` parametr řetězce dotazu.

   ```csharp
   public static class Search
   {
       static string endpoint = ConfigurationManager.AppSettings["Endpoint"];
       static string authKey = ConfigurationManager.AppSettings["AuthKey"];

       [FunctionName("Search")]
       public static async Task<HttpResponseMessage> Run(
           [HttpTrigger(AuthorizationLevel.Anonymous, "get", "post", Route = null)]HttpRequestMessage req,
           TraceWriter log)
       {
           log.Info("C# HTTP trigger function processed a request.");

           // the person objects are free-form in structure
           List<dynamic> results = new List<dynamic>();

           // open the client's connection
           using (DocumentClient client = new DocumentClient(
               new Uri(endpoint),
               authKey,
               new ConnectionPolicy
               {
                   ConnectionMode = ConnectionMode.Direct,
                   ConnectionProtocol = Protocol.Tcp
               }))
           {
               // get a reference to the database the console app created
               Database database = await client.CreateDatabaseIfNotExistsAsync(
                   new Database
                   {
                       Id = "graphdb"
                   });

               // get an instance of the database's graph
               DocumentCollection graph = await client.CreateDocumentCollectionIfNotExistsAsync(
                   UriFactory.CreateDatabaseUri("graphdb"),
                   new DocumentCollection { Id = "graphcollz" },
                   new RequestOptions { OfferThroughput = 1000 });

               // build a gremlin query based on the existence of a name parameter
               string name = req.GetQueryNameValuePairs()
                   .FirstOrDefault(q => string.Compare(q.Key, "name", true) == 0)
                   .Value;

               IDocumentQuery<dynamic> query = (!String.IsNullOrEmpty(name))
                   ? client.CreateGremlinQuery<dynamic>(graph, string.Format("g.V('{0}')", name))
                   : client.CreateGremlinQuery<dynamic>(graph, "g.V()");

               // iterate over all the results and add them to the list
               while (query.HasMoreResults)
                   foreach (dynamic result in await query.ExecuteNextAsync())
                       results.Add(result);
           }

           // return the list with an OK response
           return req.CreateResponse<List<dynamic>>(HttpStatusCode.OK, results);
       }
   }
   ```

   Kód je v podstatě logice stejné připojení jako původní konzolovou aplikaci, která nasadí databázi s jednoduchý dotaz pro načtení odpovídající záznamy.

## <a name="debug-the-azure-function-locally"></a>Ladění místně funkci Azure

Teď, když kód je dokončeno, můžete použít místní ladění nástrojů funkce Azure a emulátoru spustit kód místně to vyzkoušíte.

1. Před kód běží správně, musíte ho nakonfigurovat pro místní spuštění s informace o připojení Azure Cosmos DB. Soubor local.settings.json můžete použít ke konfiguraci funkce Azure pro místní provádění mnohem ve stejné způsob souboru App.config byste použili k konfiguraci původní konzolové aplikace pro provedení.

    K tomu, přidejte následující řádky kódu local.settings.json a poté zkopírujte koncový bod a AuthKey ze souboru App.Config v projektu GraphGetStarted, jak je znázorněno na následujícím obrázku.

   ```json
    "Endpoint": "",
    "AuthKey": ""
    ```

   ![Nastavte koncový bod a autorizační klíč v souboru local.settings.json](./media/tutorial-functions-http-trigger/07-local-functions-settings.png)

2. Změňte počáteční projekt do nové aplikace funkce. V **Průzkumníku řešení**, klikněte pravým tlačítkem na **PeopleDataFunctions**a vyberte **nastavit jako spouštěný projekt**.

3. V **Průzkumníku řešení**, klikněte pravým tlačítkem na **závislosti** v **PeopleDataFunctions** projektu a pak klikněte na **přidat odkaz na**. V seznamu vyberte System.Configuration a pak klikněte na tlačítko **OK**.

3. Teď umožňuje spuštění aplikace. Stisknutím klávesy F5 spusťte místní ladicí nástroj func.exe kódem funkce Azure hostované a připravena k použití.

   Na konci počáteční výstup func.exe vidíte, že je v localhost:7071 hostované funkce Azure. To je užitečné pro testování v klientovi.

   ![Testování klienta](./media/tutorial-functions-http-trigger/08-functions-emulator.png)

4. Chcete-li otestovat funkci Azure, použijte [Visual Studio Code](http://code.visualstudio.com/) s příponou Huachao Maovi [klienta REST](https://marketplace.visualstudio.com/items?itemName=humao.rest-client). Klienta REST nabízí místního nebo vzdáleného schopností požadavku HTTP v jednom klikněte pravým tlačítkem. 

    Chcete-li to provést, vytvořte nový soubor s názvem test funkce locally.http a přidejte následující kód:

    ```http
    get http://localhost:7071/api/Search

    get http://localhost:7071/api/Search?name=ben
   ```

    Nyní klikněte pravým tlačítkem na první řádek kódu a pak vyberte **odeslat požadavek** jak je znázorněno na následujícím obrázku.

   ![Poslat žádost o REST z kódu v sadě Visual Studio](./media/tutorial-functions-http-trigger/09-rest-client-in-vs-code.png)

   Zobrazí se nezpracovaná odpovědi HTTP z hlavičky funkce Azure spuštěna místně, obsah textu JSON, vše.

   ![Odpověď REST](./media/tutorial-functions-http-trigger/10-general-results.png)

5. Nyní vyberte druhý řádek kódu a pak vyberte **odeslat požadavek**. Přidáním `name` parametr řetězce dotazu s hodnotou, který se nachází v databázi jsme můžete filtrovat výsledky vrátí funkce Azure.

   ![Filtrovat výsledky funkce Azure](./media/tutorial-functions-http-trigger/11-search-for-ben.png)

Posledním krokem po funkce Azure je ověřen a zdá se, že funguje správně, je publikovat do služby Azure App Service a nakonfigurovat jej pro spuštění v cloudu.

## <a name="publish-the-azure-function"></a>Publikování funkci Azure

1. V **Průzkumníku řešení**, klikněte pravým tlačítkem na projekt a pak vyberte **publikovat**.

   ![Publikování nový projekt](./media/tutorial-functions-http-trigger/12-publish-function.png)

2. Jsme připraveni publikovat to do cloudu a otestovat ve scénáři veřejně dostupné. V **publikovat** vyberte **aplikaci funkce Azure**, vyberte **vytvořit nový** a vytvoření funkce Azure ve vašem předplatném Azure, potom klikněte na **publikování** .

   ![Vytvořit novou aplikaci Azure – funkce](./media/tutorial-functions-http-trigger/13-publish-panel.png)

3. V **publikovat** dialogové okno, postupujte takto:
   
    a. V **název aplikace**, zadejte jedinečný název funkce.

    b. V **předplatné**, vyberte předplatné Azure používat.
   
    c. V **skupiny prostředků**, vytvořte novou skupinu prostředků a používají stejný název jako název aplikace.
   
    d. Pro **plán služby App Service**, klikněte na tlačítko **nový** vytvořit nový na základě spotřeby plán služby App Service, protože jsme v úmyslu použít metodu platím za použití fakturace pro funkci bez serveru Azure. Použijte výchozí hodnoty na **nakonfigurovat plán služby App Service** a pak klikněte na tlačítko **OK**.
   
    e. Pro **účet úložiště**, také kliknout na tlačítko **nový** k vytvoření nového účtu úložiště pro použití s funkci Azure v případě, že jsme někdy potřebovali podporu pro objekty BLOB, tabulek nebo fronty, které chcete aktivovat provedení další funkce. Použijte výchozí hodnoty na **účet úložiště** a pak klikněte na tlačítko **OK**.

    f. Klikněte **vytvořit** tlačítka v dialogovém okně Vytvořit všechny prostředky ve vašem předplatném Azure. Visual Studio stáhne profil publikování (jednoduchý soubor XML), který používá při příštím publikování kódu funkce Azure.

   ![Vytvořit účet úložiště](./media/tutorial-functions-http-trigger/14-new-function-app.png)

    Visual Studio pak zobrazí stránky publikování, kterou můžete použít, pokud provedete změny funkce a muset znovu ji publikovat. Není nutné provádět žádnou akci nyní na této stránce.

4. Po publikování funkce Azure, můžete přejít na [portál Azure](https://portal.azure.com/) stránky pro funkce Azure. Zde se zobrazí odkaz na funkci Azure **nastavení aplikace**. Otevření tohoto odkazu nakonfigurovat funkci Azure za provozu pro připojení k databázi Azure Cosmos DB s daty osoby.

   ![Zkontrolujte nastavení aplikace](./media/tutorial-functions-http-trigger/15-function-in-portal.png)

5. Stejně jako jste to udělali dříve v souboru App.config v konzolové aplikace a v souboru local.settings.json aplikace funkce Azure, budete muset přidat koncový bod a AuthKey do databáze Azure Cosmos DB publikované funkce. Tímto způsobem není nutné zkontrolujte v konfiguraci kód, který obsahuje klíče – můžete je nakonfigurovat na portálu a ujistěte se, že nejsou uložené ve správě zdrojového kódu. Chcete-li přidat všechny hodnoty, klikněte na tlačítko **přidejte nové nastavení** tlačítko, přidejte **koncový bod** a hodnota ze souboru app.config klikněte **přidejte nové nastavení** znovu a přidejte **AuthKey**  a vlastní hodnotu. Jakmile jste přidali a uložili hodnot, nastavení by měla vypadat podobně jako tento.

   ![Konfigurace koncového bodu a AuthKey](./media/tutorial-functions-http-trigger/16-app-settings.png)

6. Jakmile funkci Azure ve vašem předplatném Azure správně nakonfigurovaný, znovu můžete rozšíření Visual Studio Code REST klienta k dotazování veřejně dostupné URL funkce Azure. Přidat tyto dva řádky kódu do locally.http funkce testu a pak spusťte každý řádek k otestování této funkce. Název funkce v adrese URL nahraďte název funkce.

    ```json
    get https://peoplesearchfunction.azurewebsites.net/api/Search

    get https://peoplesearchfunction.azurewebsites.net/api/Search?name=thomas
    ```

    Funkce odpoví data načtená z databáze Azure Cosmos.

    ![Klient REST použít dotaz funkce Azure](./media/tutorial-functions-http-trigger/17-calling-function-from-code.png)


## <a name="next-steps"></a>Další kroky

V tomto kurzu jste provést následující:

> [!div class="checklist"]
> * Vytvoření projektu Azure – funkce 
> * Vytvořit aktivační událost INSTEAD HTTP
> * Publikovaná funkci Azure
> * Funkce připojení k databázi Azure Cosmos DB

Nyní můžete přejít k části koncepty pro další informace o Cosmos DB.

> [!div class="nextstepaction"]
> [Globální distribuce](distribute-data-globally.md) 

Tento článek je založena na blogu z [Schemaless & bez serveru Novák Gaster](http://www.bradygaster.com/category/%20Serverless%20&%20Schemaless) série blogů. Navštivte svého blogu pro další příspěvky v řadě.
