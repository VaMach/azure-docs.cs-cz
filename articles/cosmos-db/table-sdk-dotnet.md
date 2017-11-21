---
title: "Tabulky Azure CosmosDB rozhraní API .NET SDK & prostředky | Microsoft Docs"
description: "Další informace o Cosmos DB tabulky rozhraní API služby Azure včetně data vydání, vyřazení dat a změny provedené mezi každou verzi."
services: cosmos-db
documentationcenter: .net
author: rnagpal
manager: jhubbard
editor: cgronlun
ms.assetid: 
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 11/20/2017
ms.author: mimig
ms.openlocfilehash: 9dc0f5140a538c3a359dd90b74de822dc163fd70
ms.sourcegitcommit: 1d8612a3c08dc633664ed4fb7c65807608a9ee20
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/20/2017
---
# <a name="azure-cosmos-db-table-net-api-download-and-release-notes"></a>Azure Cosmos DB tabulky .NET API: Stažení a poznámky k verzi
> [!div class="op_single_selector"]
> * [.NET](table-sdk-dotnet.md)
> * [Java](table-sdk-java.md)
> * [Node.js](table-sdk-nodejs.md)
> * [Python](table-sdk-python.md)

|   |   |
|---|---|
|**Stažení sady SDK**|[NuGet](https://aka.ms/acdbtablenuget)|
|**Dokumentaci k rozhraní API**|[Referenční dokumentace rozhraní API .NET](https://aka.ms/acdbtableapiref)|
|**Rychlý start**|[Azure Cosmos DB: Sestavení aplikace pomocí rozhraní .NET a Table API](create-table-dotnet.md)|
|**Kurz**|[Azure Cosmos DB: Vývoj s tabulkou rozhraní API v rozhraní .NET](tutorial-develop-table-dotnet.md)|
|**Aktuální podporovaných prostředí**|[Microsoft .NET Framework 4.5](https://www.microsoft.com/download/details.aspx?id=30653)|

> [!IMPORTANT]
> Pokud jste vytvořili účet tabulky rozhraní API ve verzi Preview, vytvořte [nový účet rozhraní API tabulky](create-table-dotnet.md#create-a-database-account) pro práci s všeobecně dostupná SDK API tabulky.
>

## <a name="release-notes"></a>Poznámky k verzi

### <a name="a-name100100"></a><a name="1.0.0"/>1.0.0
* Obecné dostupnosti verze

### <a name="a-name010-preview090-preview"></a><a name="0.1.0-preview"/>0.9.0-Preview
* Verze preview počáteční

## <a name="release-and-retirement-dates"></a>Datum vydání a vyřazování z provozu
Společnost Microsoft poskytuje oznámení alespoň **dobu 12 měsíců** předem vyřazení sady SDK k funkce smooth přechodu na novější nebo podporované verzi.

Nové funkce a funkce a optimalizace, jsou přidány pouze v aktuální sadě SDK, jako takový se doporučuje, aby vždy upgradu na nejnovější verze sady SDK v míře. 

Služba odmítne všechny požadavky pro Azure DB Cosmos pomocí vyřazeno sady SDK.
<br/>

| Verze | Datum vydání | Datum vyřazení |
| --- | --- | --- |
| [1.0.0](#1.0.0) |15 listopadu 2017|--- |
| [0.9.0-Preview](#0.1.0-preview) |11 listopadu 2017 |--- |

## <a name="troubleshooting"></a>Řešení potíží

Pokud dojde k chybě 

```
Unable to resolve dependency 'Microsoft.Azure.Storage.Common'. Source(s) used: 'nuget.org', 
'CliFallbackFolder', 'Microsoft Visual Studio Offline Packages', 'Microsoft Azure Service Fabric SDK'`
```

Při pokusu o použití balíčku Microsoft.Azure.CosmosDB.Table NuGet, máte dvě možnosti vyřešit problém:

* Pomocí konzoly Správa balíček nainstalovat Microsoft.Azure.CosmosDB.Table balíček a jeho závislé součásti. Chcete-li to provést, zadejte následující příkaz v konzole Správce balíčků pro vaše řešení. 
    ```
    Install-Package Microsoft.Azure.CosmosDB.Table -IncludePrerelease
    ```
    
* Nainstalujte balíček Microsoft.Azure.Storage.Common Nuget před instalací Microsoft.Azure.CosmosDB.Table se pomocí vaší upřednostňované nástroj pro správu balíček Nuget.

## <a name="faq"></a>Nejčastější dotazy

[!INCLUDE [cosmos-db-sdk-faq](../../includes/cosmos-db-sdk-faq.md)]

## <a name="see-also"></a>Viz také
Další informace o rozhraní API služby Azure DB Cosmos tabulky, najdete v části [Úvod do rozhraní API služby Azure Cosmos DB tabulky](table-introduction.md). 
