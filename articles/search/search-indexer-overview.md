<properties
    pageTitle="Indexery ve službě Azure Search | Microsoft Azure | Hostovaná cloudová vyhledávací služba"
    description="Procházejte databázi Azure SQL, DocumentDB nebo úložiště Azure, extrahujte prohledávatelná data a naplňte jimi index služby Azure Search."
    services="search"
    documentationCenter=""
    authors="HeidiSteen"
    manager="mblythe"
    editor=""
    tags="azure-portal"/>

<tags
    ms.service="search"
    ms.devlang="na"
    ms.workload="search"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.date="04/14/2016"
    ms.author="heidist"/>

# Indexery ve službě Azure Search
> [AZURE.SELECTOR]
- [Přehled](search-indexer-overview.md)
- [Portál](search-import-data-portal.md)
- [Azure SQL](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers-2015-02-28.md)
- [DocumentDB](../documentdb/documentdb-search-indexer.md)
- [Blob Storage (Preview)](search-howto-indexing-azure-blob-storage.md)
- [Table Storage (Preview)](search-howto-indexing-azure-tables.md)

 **Indexer** ve službě Azure Search je prohledávací modul, který extrahuje prohledávatelná data a metadata z externího zdroje dat a naplní index na základě mapování pole-pole mezi indexem a zdrojem dat. Tento přístup se někdy nazývá model Pull, protože služba si vyžádá data, aniž byste museli psát kód, který by vložil data do indexu bez vyžádání.

Můžete použít indexer jako jediný prostředek přijímání dat, nebo můžete použít kombinaci postupů, kdy se indexer použije k načtení jenom některých polí v indexu.

Indexery se můžou spouštět na vyžádání nebo podle pravidelného plánu aktualizace dat, která může probíhat až každých 15 minut. Častější aktualizace vyžadují model Push, který aktualizuje data současně ve službě Azure Search i v externím zdroji dat.

Indexer získává data ze **zdroje dat**, který obsahuje informace, jako je například připojovací řetězec. Aktuálně jsou podporované tyto zdroje dat:

- [Azure SQL Database](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers-2015-02-28.md) (nebo SQL Server na virtuálním počítači Azure)
- [DocumentDB](../documentdb/documentdb-search-indexer.md)
- [Úložiště objektů Azure Blob](search-howto-indexing-azure-blob-storage.md) (Aktuálně ve verzi Preview. Extrahuje text z formátu PDF, dokumentů Office, HTML a XML.)
- [Azure Table Storage](search-howto-indexing-azure-tables.md) (Aktuálně ve verzi Preview)

Zdroje dat se konfigurují a spravují nezávisle na indexerech, které je používají, což znamená, že několik indexerů může používat zdroj dat k načtení více indexů současně. 

Sada [.NET SDK](https://msdn.microsoft.com/library/azure/microsoft.azure.search.iindexersoperations.aspx) i rozhraní [REST API služby](https://msdn.microsoft.com/library/azure/dn946891.aspx) podporují správu indexerů a zdrojů dat. 

Když používáte průvodce **Import dat**, můžete taky indexer nakonfigurovat na portálu. V tématu [Začínáme se službou Azure Search na portálu](search-get-started-portal) najdete rychlý kurz, který pomocí ukázkových dat a indexeru DocumentDB vytvoří a načte index pomocí průvodce.






<!--HONumber=Jun16_HO2-->


