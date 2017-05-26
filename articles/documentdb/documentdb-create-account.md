---
title: "Vytvoření účtu služby Azure Cosmos DB | Dokumentace Microsoftu"
description: "Vytvořte databázi NoSQL pomocí služby Azure Cosmos DB. Pomocí těchto pokynů můžete vytvořit účet služby Azure Cosmos DB a začít vytvářet neuvěřitelně rychlou databázi NoSQL s globálním škálováním."
keywords: "vytvoření databáze"
services: cosmosdb
documentationcenter: 
author: mimig1
manager: jhubbard
editor: monicar
ms.assetid: 0e7f8488-7bb7-463e-b6fd-3ae91a02c03a
ms.service: cosmosdb
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 02/17/2017
ms.author: mimig
redirect_url: https://aka.ms/acdbnetqa
ROBOTS: NOINDEX, NOFOLLOW
ms.translationtype: Human Translation
ms.sourcegitcommit: 71fea4a41b2e3a60f2f610609a14372e678b7ec4
ms.openlocfilehash: 13621d942f2880f4dd1523315f43099eca2607d8
ms.contentlocale: cs-cz
ms.lasthandoff: 05/10/2017


---
# <a name="how-to-create-an-azure-cosmos-db-nosql-account-using-the-azure-portal"></a>Vytvoření účtu služby Azure Cosmos DB NoSQL pomocí webu Azure Portal
> [!div class="op_single_selector"]
> * [Azure Portal](documentdb-create-account.md)
> * [Azure CLI 1.0](documentdb-automation-resource-manager-cli-nodejs.md)
> * [Azure CLI 2.0](documentdb-automation-resource-manager-cli.md)
> * [Azure Powershell](documentdb-manage-account-with-powershell.md)

Pokud chcete vytvořit databázi pomocí služby Microsoft Azure Cosmos DB, musíte:

* Mít účet Azure. Pokud ještě žádný nemáte, můžete získat [bezplatný účet Azure](https://azure.microsoft.com/free).
* Vytvořit účet služby Azure Cosmos DB.  

Účet služby Azure Cosmos DB můžete vytvořit pomocí webu Azure Portal, šablon Azure Resource Manageru nebo rozhraní příkazového řádku (CLI) Azure. Tento článek popisuje, jak vytvořit účet služby Azure Cosmos DB pomocí webu Azure Portal. Pokud chcete účet vytvořit pomocí Azure Resource Manageru nebo Azure CLI, přečtěte si téma věnované [automatizaci vytváření účtů databáze Azure Cosmos DB](documentdb-automation-resource-manager-cli.md).

Zatím službu Azure Cosmos DB neznáte? Podívejte se na [toto](https://azure.microsoft.com/documentation/videos/create-documentdb-on-azure/) čtyřminutové video Scotta Hanselmana, kde se dozvíte, jak na online portálu provádět nejzákladnější úkoly.

1. Přihlaste se k webu [Azure Portal](https://portal.azure.com/).
2. Na navigačním panelu vlevo klikněte postupně na **Nový**, **Databáze** a pak na **Azure Cosmos DB**.

   ![Snímek obrazovky webu Azure Portal se zvýrazněním položek Další služby a NoSQL (Azure Cosmos DB)](./media/documentdb-create-account/create-nosql-db-databases-json-tutorial-1.png)  
3. V okně **Nový účet** zadejte požadovanou konfiguraci účtu služby Azure Cosmos DB.

    ![Snímek obrazovky s oknem Nová služba Azure Cosmos DB](./media/documentdb-create-account/create-nosql-db-databases-json-tutorial-2.png)

   * Do pole **ID** zadejte název pro identifikaci účtu služby Azure Cosmos DB.  Po ověření **ID** se v poli **ID** zobrazí zelená značka zaškrtnutí. Hodnota **ID** se v rámci identifikátoru URI stane názvem hostitele. **ID** smí obsahovat jenom malá písmena, číslice a znak spojovníku a musí se skládat ze 3 až 50 znaků. Všimněte si, že ke zvolenému názvu koncového bodu se připojí text *documents.azure.com*. Výsledkem bude koncový bod účtu služby Azure Cosmos DB.
   * V poli **Rozhraní API NoSQL** vyberte programovací model, který se má použít:

     * **DocumentDB**: Rozhraní API DocumentDB je dostupné prostřednictvím sad .NET, Java, Node.js, Python a JavaScript [SDK](documentdb-sdk-dotnet.md) a také prostřednictvím HTTP [REST](https://msdn.microsoft.com/library/azure/dn781481.aspx) a nabízí programový přístup ke všem funkcím DocumentDB.
     * **MongoDB**: DocumentDB také nabízí [podporu na úrovni protokolu](documentdb-protocol-mongodb.md) pro rozhraní API **MongoDB**. Pokud použijete možnost rozhraní API MongoDB, můžete ke komunikaci s DocumentDB využít stávající sady SDK a [nástroje](documentdb-mongodb-mongochef.md) MongoDB. Stávající aplikace MongoDB můžete [přesunout](documentdb-import-data.md) do DocumentDB bez [jakékoli změny kódu](documentdb-connect-mongodb-account.md) a využívat výhod plně spravované databáze jako služby s neomezeným škálováním, globální replikací a dalšími funkcemi.
   * V poli **Předplatné** vyberte předplatné Azure, se kterým chcete tento účet služby Azure Cosmos DB používat. Pokud má váš účet jenom jedno předplatné, ve výchozím nastavením se vybere tento účet.
   * V poli **Skupina prostředků** vyberte nebo vytvořte skupinu prostředků pro účet služby Azure Cosmos DB.  Ve výchozím nastavení se vytvoří nová skupina prostředků. Další informace najdete v článku [Použití webu Azure Portal ke správě prostředků Azure](../azure-portal/resource-group-portal.md).
   * K určení zeměpisného umístění, ve kterém chcete účet služby Azure Cosmos DB hostovat, použijte **Umístění**.
4. Po nakonfigurování možností nového účtu služby Azure Cosmos DB klikněte na **Vytvořit**. Ke kontrole stavu nasazení použijte centrum oznámení.  

   ![Rychlé vytváření databází – snímek obrazovky s centrem oznámení, na kterém je vidět vytváření účtu služby Azure Cosmos DB](./media/documentdb-create-account/create-nosql-db-databases-json-tutorial-4.png)  

   ![Snímek obrazovky s centrem oznámení, na kterém je vidět úspěšné vytvoření účtu služby Azure Cosmos DB a jeho nasazení do skupiny prostředků – oznámení online tvůrce databází](./media/documentdb-create-account/create-nosql-db-databases-json-tutorial-5.png)
5. Účet služby Azure Cosmos DB je po vytvoření připravený k použití s výchozím nastavením. Výchozí konzistence účtu služby Azure Cosmos DB je nastavena na možnost **Relace**.  Výchozí konzistenci můžete upravit kliknutím na **Výchozí konzistence** v nabídce prostředků. Další informace o úrovních konzistence, které služba Azure Cosmos DB nabízí, najdete v tématu [Úrovně konzistence ve službě Azure Cosmos DB](documentdb-consistency-levels.md).

   ![Snímek obrazovky okna Skupina prostředků – začátek vývoje aplikace](./media/documentdb-create-account/create-nosql-db-databases-json-tutorial-6.png)  

   ![Snímek obrazovky okna Úroveň konzistence – konzistence relace](./media/documentdb-create-account/create-nosql-db-databases-json-tutorial-7.png)  

[How to: Create an Azure Cosmos DB account]: #Howto
[Next steps]: #NextSteps


## <a name="next-steps"></a>Další kroky
Teď, když máte účet služby Azure Cosmos DB, je dalším krokem vytvoření kolekce a databáze Azure Cosmos DB.

K vytvoření nové kolekce a databáze můžete použít jeden z následujících prostředků:

* Azure Portal, jak je popsáno v tématu [Vytvoření kolekce Azure Cosmos DB pomocí webu Azure Portal](documentdb-create-collection.md).
* Kompletní kurzy, které zahrnují ukázková data: [.NET](documentdb-get-started.md), [.NET MVC](documentdb-dotnet-application.md), [Java](documentdb-java-application.md), [Node.js](documentdb-nodejs-application.md) nebo [Python](documentdb-python-application.md).
* Ukázkový kód [.NET](documentdb-dotnet-samples.md#database-examples), [Node.js](documentdb-nodejs-samples.md#database-examples) nebo [Python](documentdb-python-samples.md#database-examples), který je dostupný na GitHubu.
* Sady [.NET](documentdb-sdk-dotnet.md), [.NET Core](documentdb-sdk-dotnet-core.md), [Node.js](documentdb-sdk-node.md), [Java](documentdb-sdk-java.md), [Python](documentdb-sdk-python.md) a [REST](https://msdn.microsoft.com/library/azure/mt489072.aspx).

Po vytvoření databáze a kolekce je nutné do kolekcí [přidat dokumenty](documentdb-view-json-document-explorer.md).

Jakmile máte v kolekci dokumenty, můžete [DocumentDB SQL](documentdb-sql-query.md) využít ke [spouštění dotazů](documentdb-sql-query.md#ExecutingSqlQueries) nad těmito dokumenty. Ke spouštění dotazů je možné využít [Průzkumníka dotazů](documentdb-query-collections-query-explorer.md) na portálu, rozhraní [REST API](https://msdn.microsoft.com/library/azure/dn781481.aspx) nebo jednu ze sad [SDK](documentdb-sdk-dotnet.md).

### <a name="learn-more"></a>Další informace
Další informace o službě Azure Cosmos DB najdete v těchto zdrojích informací:

* [Úvod do služby Azure Cosmos DB](../cosmos-db/introduction.md)

