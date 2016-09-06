<properties
    pageTitle="Jak vytvořit účet DocumentDB | Microsoft Azure"
    description="Vytvoření databáze NoSQL pomocí Azure DocumentDB Pomocí těchto pokynů můžete vytvořit účet DocumentDB a začít vytvářet neuvěřitelně rychlou databázi NoSQL v globálním rozsahu." 
    keywords="vytvoření databáze"
    services="documentdb"
    documentationCenter=""
    authors="mimig1"
    manager="jhubbard"
    editor="monicar"/>

<tags
    ms.service="documentdb"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/25/2016"
    ms.author="mimig"/>

# Jak vytvořit účet DocumentDB na Portálu Azure

> [AZURE.SELECTOR]
- [Azure Portal](documentdb-create-account.md)
- [Azure CLI a ARM](documentdb-automation-resource-manager-cli.md)

Abyste mohli vytvořit databázi pomocí Microsoft Azure DocumentDB, musíte:

- Mít účet Azure. Pokud ještě žádný nemáte, můžete získat [bezplatný účet Azure](https://azure.microsoft.com/free). 
- Vytvořit si účet DocumentDB  

Účet DocumentDB můžete vytvořit přes Portál Azure, s použitím šablon Azure Resource Manageru nebo přes rozhraní příkazového řádku (CLI) Azure. Tento článek popisuje, jak vytvořit účet DocumentDB pomocí webu Azure Portal. Pokud chcete účet vytvořit pomocí Azure Resource Manageru nebo Azure CLI, přečtěte si téma [Automatizace vytváření databázového účtu DocumentDB](documentdb-automation-resource-manager-cli.md).

Je pro vás služba DocumentDB nová? Podívejte se na [toto](https://azure.microsoft.com/documentation/videos/create-documentdb-on-azure/) čtyřminutové video Scotta Hanselmana, kde se dozvíte, jak na online portálu provést nejzákladnější úkoly.

[AZURE.INCLUDE [documentdb-create-dbaccount](../../includes/documentdb-create-dbaccount.md)]

## Další kroky

Nyní když máte účet DocumentDB, je dalším krokem vytvoření databáze DocumentDB. Databázi můžete vytvořit pomocí jednoho z následujících prostředků:

- Portál Azure, jak je popsáno v tématu [Vytvoření databáze DocumentDB pomocí portálu Azure](documentdb-create-database.md)
- Ukázky C# .NET v projektu [DatabaseManagement](https://github.com/Azure/azure-documentdb-net/tree/master/samples/code-samples/DatabaseManagement) v úložišti [azure-documentdb-net](https://github.com/Azure/azure-documentdb-net/tree/master/samples/code-samples) na GitHubu
- Kompletní kurzy: [.NET](documentdb-get-started.md), [.NET MVC](documentdb-dotnet-application.md), [Java](documentdb-java-application.md), [Node.js](documentdb-nodejs-application.md) nebo [Python](documentdb-python-application.md)
- Sady [DocumentDB SDK](documentdb-sdk-dotnet.md). DocumentDB má sady SDK pro rozhraní API .NET, Java, Python, Node.js a JavaScript.


Po vytvoření databáze je nutné do ní [přidat jednu nebo více kolekcí](documentdb-create-collection.md) a do těchto kolekcí pak [přidat dokumenty](documentdb-view-json-document-explorer.md).

Jakmile budete mít v kolekci dokumenty, můžete využít [DocumentDB SQL](documentdb-sql-query.md) ke [spouštění dotazů](documentdb-sql-query.md#executing-queries) na dokumenty pomocí [Průzkumníka dotazů](documentdb-query-collections-query-explorer.md) na portálu, rozhraní [REST API](https://msdn.microsoft.com/library/azure/dn781481.aspx) nebo jedné ze sad [SDK](documentdb-sdk-dotnet.md).

Další informace o DocumentDB najdete v těchto materiálech:

-   [Studijní program DocumentDB](https://azure.microsoft.com/documentation/learning-paths/documentdb/)
-   [Model prostředků a koncepty DocumentDB](documentdb-resources.md)



<!--HONumber=ago16_HO5-->


