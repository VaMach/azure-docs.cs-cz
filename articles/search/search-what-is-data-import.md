<properties
    pageTitle="Nahrání dat ve službě Azure Search | Microsoft Azure | Hostovaná cloudová vyhledávací služba"
    description="Naučte se nahrát data do indexu ve službě Azure Search."
    services="search"
    documentationCenter=""
    authors="ashmaka"
    manager=""
    editor=""
    tags=""/>

<tags
    ms.service="search"
    ms.devlang="NA"
    ms.workload="search"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.date="05/31/2016"
    ms.author="ashmaka"/>

# Nahrání dat do služby Azure Search
> [AZURE.SELECTOR]
- [Přehled](search-what-is-data-import.md)
- [.NET](search-import-data-dotnet.md)
- [REST](search-import-data-rest-api.md)


## Modely nahrávání dat ve službě Azure Search
Existují dva způsoby, jak index služby Azure Search naplnit daty. První možností je ručně vložit (Push) data do indexu pomocí rozhraní [REST API](search-import-data-rest-api.md) nebo sady [.NET SDK](search-import-data-dotnet.md) služby Azure Search. Druhou možností je [nasměrovat některý podporovaný zdroj dat](search-indexer-overview.md) na index Azure Search a nechat službu Azure Search, aby automaticky získala (Pull) data do vyhledávací služby.

Ačkoli tento průvodce nabízí pokyny, které se týkají jenom nahrání dat pomocí modelu Push (to je podporováno pouze v rozhraní [REST API](search-import-data-rest-api.md) a sadě [.NET SDK](search-import-data-dotnet.md)), níže najdete taky další informace o modelu Pull.

### Vložení dat do indexu

Tento postup znamená programové odeslání dat do služby Azure Search, aby byla dostupná pro hledání. U aplikací vyžadujících velmi nízkou latenci (pokud například potřebujete, aby operace hledání byly synchronizované s dynamickými databázemi zásob) je model Push vaší jedinou možností.

Data můžete vkládat do indexu pomocí rozhraní [REST API](https://msdn.microsoft.com/library/azure/dn798930.aspx) nebo sady [.NET SDK](search-import-data-dotnet.md). Vkládání dat prostřednictvím portálu není aktuálně podporováno.

Tento přístup je flexibilnější než model Pull, protože můžete nahrávat dokumenty samostatně nebo v dávkách (jednotlivé dávky můžou obsahovat až 1000 dokumentů nebo 16 MB, podle toho, kterého omezení dosáhnete dříve). Model Push vám taky umožňuje nahrát dokumenty do služby Azure Search bez ohledu na to, kde se data nacházejí.

### Získání dat do indexu

Model Pull prochází podporovaný zdroj dat a automaticky nahrává data do indexu Azure Search. Indexery sledují změny a odstranění ve stávajících dokumentech a rozpoznávají nové dokumenty, a díky tomu není potřeba aktivně spravovat data v indexu.

Ve službě Azure Search je tato schopnost implementovaná prostřednictvím *indexerů*, které jsou aktuálně dostupné pro [Blob Storage (Preview)](search-howto-indexing-azure-blob-storage.md), [DocumentDB](http://aka.ms/documentdb-search-indexer), [databáze Azure SQL a SQL Server na virtuálních počítačích Azure](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers-2015-02-28.md).

Funkce indexeru je zpřístupněná na [Portálu Azure](search-import-data-portal.md) a v rozhraní [REST API](https://msdn.microsoft.com/library/azure/dn946891.aspx).



<!--HONumber=Jun16_HO2-->


