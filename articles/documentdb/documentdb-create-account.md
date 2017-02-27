---
title: "Jak vytvořit účet DocumentDB | Microsoft Docs"
description: "Vytvořte databázi NoSQL pomocí Azure DocumentDB. Pomocí těchto pokynů můžete vytvořit účet DocumentDB a začít vytvářet neuvěřitelně rychlou databázi NoSQL s globálním škálováním."
keywords: "vytvoření databáze"
services: documentdb
documentationcenter: 
author: mimig1
manager: jhubbard
editor: monicar
ms.assetid: 0e7f8488-7bb7-463e-b6fd-3ae91a02c03a
ms.service: documentdb
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 02/10/2017
ms.author: mimig
translationtype: Human Translation
ms.sourcegitcommit: 3dbf57923e104715d34d1131d16acb6bc6c51637
ms.openlocfilehash: 1f7faf166c1ddab8b25fb6a1fe4cc432b8a47432


---
# <a name="how-to-create-a-documentdb-nosql-account-using-the-azure-portal"></a>Jak vytvořit účet NoSQL DocumentDB na webu Azure Portal
> [!div class="op_single_selector"]
> * [Azure Portal](documentdb-create-account.md)
> * [Rozhraní příkazového řádku Azure a Azure Resource Manager](documentdb-automation-resource-manager-cli.md)
>
>

Pokud chcete vytvořit databázi pomocí Microsoft Azure DocumentDB, musíte:

* Mít účet Azure. Pokud ještě žádný nemáte, můžete získat [bezplatný účet Azure](https://azure.microsoft.com/free).
* Vytvořit účet DocumentDB.  

Účet DocumentDB můžete vytvořit pomocí webu Azure Portal, šablon Azure Resource Manageru nebo rozhraní příkazového řádku (CLI) Azure. Tento článek popisuje, jak vytvořit účet DocumentDB pomocí webu Azure Portal. Pokud chcete účet vytvořit pomocí Azure Resource Manageru nebo rozhraní příkazového řádku Azure, přečtěte si téma věnované [automatizaci vytváření databázových účtů DocumentDB](documentdb-automation-resource-manager-cli.md).

Je pro vás služba DocumentDB nová? Podívejte se na [toto](https://azure.microsoft.com/documentation/videos/create-documentdb-on-azure/) čtyřminutové video Scotta Hanselmana, kde se dozvíte, jak na online portálu provádět nejzákladnější úkoly.

1. Přihlaste se na web [Azure Portal ](https://portal.azure.com/).
2. Na navigačním panelu vlevo klikněte na **Nový**, klikněte na **Databáze** a pak na **NoSQL (DocumentDB)**.

   ![Snímek obrazovky webu Azure Portal se zvýrazněním položek Další služby a NoSQL (DocumentDB)](./media/documentdb-create-account/create-nosql-db-databases-json-tutorial-1.png)  
3. V okně **Nový účet** zadejte požadovanou konfiguraci účtu DocumentDB.

    ![Snímek obrazovky s oknem Nový DocumentDB](./media/documentdb-create-account/create-nosql-db-databases-json-tutorial-2.png)

   * Do pole **ID** zadejte název pro identifikaci účtu DocumentDB.  Po ověření **ID** se v poli **ID** zobrazí zelená značka zaškrtnutí. Hodnota **ID** se v rámci identifikátoru URI stane názvem hostitele. **ID** smí obsahovat jenom malá písmena, číslice a znak spojovníku a musí se skládat ze 3 až 50 znaků. Všimněte si, že ke zvolenému názvu koncového bodu se připojí text *documents.azure.com* Výsledkem bude koncový bod účtu DocumentDB.
   * V poli **Rozhraní API NoSQL** vyberte programovací model, který se má použít:

     * **DocumentDB**: Rozhraní API DocumentDB je dostupné prostřednictvím sad .NET, Java, Node.js, Python a JavaScript [SDK](documentdb-sdk-dotnet.md) a také prostřednictvím HTTP [REST](https://msdn.microsoft.com/library/azure/dn781481.aspx) a nabízí programový přístup ke všem funkcím DocumentDB.
     * **MongoDB**: DocumentDB také nabízí [podporu na úrovni protokolu](documentdb-protocol-mongodb.md) pro rozhraní API **MongoDB**. Pokud použijete možnost rozhraní API MongoDB, můžete ke komunikaci s DocumentDB využít stávající sady SDK a [nástroje](documentdb-mongodb-mongochef.md) MongoDB. Stávající aplikace MongoDB můžete [přesunout](documentdb-import-data.md) do DocumentDB bez [jakékoli změny kódu](documentdb-connect-mongodb-account.md) a využívat výhod plně spravované databáze jako služby s neomezeným škálováním, globální replikací a dalšími funkcemi.
   * V poli **Předplatné** vyberte předplatné Azure, se kterým chcete tento účet DocumentDB používat. Pokud má váš účet jenom jedno předplatné, ve výchozím nastavením se vybere tento účet.
   * V poli **Skupina prostředků** vyberte nebo vytvořte skupinu prostředků pro účet DocumentDB.  Ve výchozím nastavení se vytvoří nová skupina prostředků. Další informace najdete v článku [Použití webu Azure Portal ke správě prostředků Azure](../azure-portal/resource-group-portal.md).
   * K určení zeměpisného umístění, ve kterém chcete účet DocumentDB hostovat, použijte **Umístění**.
4. Po nakonfigurování možností nového účtu DocumentDB klikněte na **Vytvořit**. Ke kontrole stavu nasazení použijte centrum oznámení.  

   ![Rychlé vytváření databází – snímek obrazovky s centrem oznámení, na kterém je vidět vytváření účtu DocumentDB](./media/documentdb-create-account/create-nosql-db-databases-json-tutorial-4.png)  

   ![Snímek obrazovky s centrem oznámení, na kterém je vidět úspěšné vytvoření účtu DocumentDB a jeho nasazení do skupiny prostředků – oznámení online tvůrce databází](./media/documentdb-create-account/create-nosql-db-databases-json-tutorial-5.png)
5. Účet DocumentDB je po vytvoření připravený k použití s výchozím nastavením. Výchozí konzistence účtu DocumentDB je nastavená na možnost **Relace**.  Výchozí konzistenci můžete upravit kliknutím na **Výchozí konzistence** v nabídce prostředků. Další informace o úrovních konzistence, které nabízí DocumentDB, najdete v tématu [Úrovně konzistence v DocumentDB](documentdb-consistency-levels.md).

   ![Snímek obrazovky okna Skupina prostředků – začátek vývoje aplikace](./media/documentdb-create-account/create-nosql-db-databases-json-tutorial-6.png)  

   ![Snímek obrazovky okna Úroveň konzistence – konzistence relace](./media/documentdb-create-account/create-nosql-db-databases-json-tutorial-7.png)  

[How to: Create a DocumentDB account]: #Howto
[Next steps]: #NextSteps


## <a name="next-steps"></a>Další kroky
Teď máte účet DocumentDB a dalším krokem je vytvoření kolekce a databáze DocumentDB.

K vytvoření nové kolekce a databáze můžete použít jeden z následujících prostředků:

* Azure Portal, jak je popsáno v tématu [Vytvoření kolekce DocumentDB pomocí webu Azure Portal](documentdb-create-collection.md).
* Kompletní kurzy, které zahrnují ukázková data: [.NET](documentdb-get-started.md), [.NET MVC](documentdb-dotnet-application.md), [Java](documentdb-java-application.md), [Node.js](documentdb-nodejs-application.md) nebo [Python](documentdb-python-application.md).
* Ukázkový kód [.NET](documentdb-dotnet-samples.md#database-examples), [Node.js](documentdb-nodejs-samples.md#database-examples) nebo [Python](documentdb-python-samples.md#database-examples), který je dostupný na GitHubu.
* Sady [.NET](documentdb-sdk-dotnet.md), [.NET Core](documentdb-sdk-dotnet-core.md), [Node.js](documentdb-sdk-node.md), [Java](documentdb-sdk-java.md), [Python](documentdb-sdk-python.md) a [REST](https://msdn.microsoft.com/library/azure/mt489072.aspx).

Po vytvoření databáze a kolekce je nutné do kolekcí [přidat dokumenty](documentdb-view-json-document-explorer.md).

Jakmile máte v kolekci dokumenty, můžete [DocumentDB SQL](documentdb-sql-query.md) využít ke [spouštění dotazů](documentdb-sql-query.md#executing-sql-queries) nad těmito dokumenty. Ke spouštění dotazů je možné využít [Průzkumníka dotazů](documentdb-query-collections-query-explorer.md) na portálu, rozhraní [REST API](https://msdn.microsoft.com/library/azure/dn781481.aspx) nebo jednu ze sad [SDK](documentdb-sdk-dotnet.md).

### <a name="learn-more"></a>Další informace
Další informace o DocumentDB najdete v těchto materiálech:

* [Studijní program pro DocumentDB](https://azure.microsoft.com/documentation/learning-paths/documentdb/)
* [Koncepty a hierarchický model prostředků DocumentDB](documentdb-resources.md)



<!--HONumber=Feb17_HO3-->


