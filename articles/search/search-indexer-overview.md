<properties
    pageTitle="Indexery ve službě Azure Search | Microsoft Azure | Hostovaná cloudová vyhledávací služba"
    description="Procházejte databázi Azure SQL, DocumentDB nebo úložiště Azure, extrahujte prohledávatelná data a naplňte jimi index služby Azure Search."
    services="search"
    documentationCenter=""
    authors="HeidiSteen"
    manager="jhubbard"
    editor=""
    tags="azure-portal"/>

<tags
    ms.service="search"
    ms.devlang="na"
    ms.workload="search"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.date="08/08/2016"
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

Indexery můžete spouštět na vyžádání nebo podle pravidelného plánu aktualizace dat, která může probíhat až každých 15 minut. Častější aktualizace vyžadují model Push, který aktualizuje data současně ve službě Azure Search i v externím zdroji dat.

## Přístupy k vytváření a správě indexerů

Všeobecně dostupné indexery jako SQL Azure nebo DocumentDB můžete vytvořit a spravovat pomocí těchto přístupů:

- [Portál > Průvodce importem dat ](search-get-started-portal.md)
- [Rozhraní API služby REST](https://msdn.microsoft.com/library/azure/dn946891.aspx)
- [.NET SDK](https://msdn.microsoft.com/library/azure/microsoft.azure.search.iindexersoperations.aspx)

Indexery verze Preview, jako je třeba Azure Blob Storage nebo Table Storage, vyžadují kód a rozhraní API verze Preview, třeba [rozhraní API Azure Search Preview REST pro indexery](search-api-indexers-2015-02-28-preview.md). Pro funkce verze Preview většinou nejsou dostupné nástroje portálu.

## Postup základní konfigurace

Indexery můžou nabízet funkce, které jsou jedinečné pro daný zdroj dat. Z toho důvodu se budou některé aspekty konfigurace indexeru nebo zdroje dat lišit podle typu indexeru. Všechny indexery ale sdílejí stejné základní složení a požadavky. Níže najdete popis kroků společných pro všechny indexery.

### Krok 1: Vytvoření indexu

Indexer automatizuje některé úkoly související s příjmem dat, ale vytváření indexu k nim nepatří. K základním požadavkům patří předdefinovaný index s poli, která odpovídají polím v externím zdroji dat. Další informace o strukturování indexu najdete v tématu [Vytvoření indexu (rozhraní API Azure Search REST)](https://msdn.microsoft.com/library/azure/dn798941.aspx).

### Krok 2: Vytvoření zdroje dat

Indexer získává data ze **zdroje dat**, který obsahuje informace, jako je například připojovací řetězec. Aktuálně jsou podporované tyto zdroje dat:

- [Azure SQL Database nebo SQL Server na virtuálním počítači Azure](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers-2015-02-28.md)
- [DocumentDB](../documentdb/documentdb-search-indexer.md)
- Služba [Azure Blob Storage (Preview)](search-howto-indexing-azure-blob-storage.md), která slouží k extrakci textu z dokumentů PDF, dokumentů Office, HTML nebo XML
- [Azure Table Storage (Preview)](search-howto-indexing-azure-tables.md)

Zdroje dat se konfigurují a spravují nezávisle na indexerech, které je používají, což znamená, že několik indexerů může používat zdroj dat k načtení více indexů současně. 

### Krok 3: Vytvoření a naplánování indexeru

Definice indexeru je konstrukce, která určuje index, zdroj dat a plán. Indexer můžete odkazovat na zdroj dat z jiné služby, pokud se tento zdroj dat nachází ve stejném předplatném. Další informace o strukturování indexeru najdete v tématu [Vytvoření indexeru (rozhraní API Azure Search REST)](https://msdn.microsoft.com/library/azure/dn946899.aspx).

## Další kroky

Teď jste získali základní představu. V dalším kroku se zaměříme na požadavky a úlohy specifické pro různé typy zdrojů dat.

- [Azure SQL Database nebo SQL Server na virtuálním počítači Azure](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers-2015-02-28.md)
- [DocumentDB](../documentdb/documentdb-search-indexer.md)
- Služba [Azure Blob Storage (Preview)](search-howto-indexing-azure-blob-storage.md), která slouží k extrakci textu z dokumentů PDF, dokumentů Office, HTML nebo XML
- [Azure Table Storage (Preview)](search-howto-indexing-azure-tables.md)
- [Indexování objektů blob CSV pomocí indexeru Azure Search Blob (Preview)](search-howto-index-csv-blobs.md)
- [Indexování objektů blob JSON pomocí indexeru Azure Search Blob (Preview)](search-howto-index-json-blobs.md)




<!--HONumber=sep16_HO2-->


