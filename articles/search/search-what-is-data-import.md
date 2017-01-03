---
title: "Odesílání dat ve službě Azure Search | Dokumentace Microsoftu"
description: "Naučte se nahrát data do indexu ve službě Azure Search."
services: search
documentationcenter: 
author: ashmaka
manager: jhubbard
editor: 
tags: 
ms.assetid: aa8d47c1-4ae6-4209-a8ce-48d5a9474707
ms.service: search
ms.devlang: NA
ms.workload: search
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.date: 12/09/2016
ms.author: ashmaka
translationtype: Human Translation
ms.sourcegitcommit: 72cc0d9ff35ff656a6134b52812b64c39a295a6f
ms.openlocfilehash: 53786d60d9971d9f976bf0f3ef4e40346c3101f4


---
# <a name="upload-data-to-azure-search"></a>Nahrání dat do služby Azure Search
> [!div class="op_single_selector"]
> * [Přehled](search-what-is-data-import.md)
> * [.NET](search-import-data-dotnet.md)
> * [REST](search-import-data-rest-api.md)
> 
> 

## <a name="data-upload-models-in-azure-search"></a>Modely nahrávání dat ve službě Azure Search
Existují dva způsoby, jak index služby Azure Search naplnit daty. První možností je ručně vložit (Push) data do indexu pomocí rozhraní [REST API](search-import-data-rest-api.md) nebo sady [.NET SDK](search-import-data-dotnet.md) služby Azure Search. Druhou možností je [nasměrovat některý podporovaný zdroj dat](search-indexer-overview.md) na index Azure Search a nechat službu Azure Search, aby automaticky získala (Pull) data do vyhledávací služby.

Ačkoli tento průvodce nabízí pokyny, které se týkají jenom nahrání dat pomocí modelu Push (to je podporováno pouze v rozhraní [REST API](search-import-data-rest-api.md) a sadě [.NET SDK](search-import-data-dotnet.md)), níže najdete taky další informace o modelu Pull.

## <a name="push-data-to-an-index"></a>Vložení dat do indexu
Tento postup znamená programové odeslání dat do služby Azure Search, aby byla dostupná pro hledání. U aplikací vyžadujících velmi nízkou latenci (pokud například potřebujete, aby operace hledání byly synchronizované s dynamickými databázemi zásob) je model Push vaší jedinou možností.

Data můžete vkládat do indexu pomocí rozhraní [REST API](https://docs.microsoft.com/rest/api/searchservice/AddUpdate-or-Delete-Documents) nebo sady [.NET SDK](search-import-data-dotnet.md). Vkládání dat prostřednictvím portálu není aktuálně podporováno.

Tento přístup je flexibilnější než model Pull, protože můžete nahrávat dokumenty samostatně nebo v dávkách (jednotlivé dávky můžou obsahovat až 1000 dokumentů nebo 16 MB, podle toho, kterého omezení dosáhnete dříve). Model Push vám taky umožňuje nahrát dokumenty do služby Azure Search bez ohledu na to, kde se data nacházejí.

## <a name="pull-data-into-an-index"></a>Získání dat do indexu
Model Pull prochází podporovaný zdroj dat a automaticky nahrává data do indexu Azure Search. Indexery sledují změny a odstranění ve stávajících dokumentech a rozpoznávají nové dokumenty, a díky tomu není potřeba aktivně spravovat data v indexu.

Ve službě Azure Search je tato schopnost implementovaná prostřednictvím *indexerů*, které jsou aktuálně dostupné pro [Blob Storage (Preview)](search-howto-indexing-azure-blob-storage.md), [DocumentDB](http://aka.ms/documentdb-search-indexer), [databáze Azure SQL a SQL Server na virtuálních počítačích Azure](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers.md).

Funkce indexeru je zpřístupněná na webu [Azure Portal](search-import-data-portal.md) a v rozhraní [REST API](https://docs.microsoft.com/rest/api/searchservice/Indexer-operations).




<!--HONumber=Jan17_HO1-->


