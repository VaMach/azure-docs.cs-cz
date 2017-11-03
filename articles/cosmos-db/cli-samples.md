---
title: "Ukázky rozhraní příkazového řádku Azure pro Azure Cosmos DB | Microsoft Docs"
description: "Ukázek Azure CLI - vytvářet a spravovat účty Azure Cosmos DB, databáze, kontejnery, oblasti a brány firewall."
services: cosmos-db
author: mimig1
manager: jhubbard
editor: 
tags: azure-service-management
ms.assetid: 
ms.service: cosmos-db
ms.custom: mvc
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: database
ms.date: 06/07/2017
ms.author: mimig
ms.openlocfilehash: 189034b049e776a3b929930be937ec60bc1db241
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="azure-cli-samples-for-azure-cosmos-db"></a>Ukázek Azure CLI pro Azure Cosmos DB

Následující tabulka obsahuje odkazy na ukázkové skripty rozhraní příkazového řádku Azure pro Azure Cosmos DB. Jsou k dispozici v referenční stránky pro všechny Cosmos DB rozhraní příkazového řádku Azure [referenční informace o Azure CLI 2.0](https://docs.microsoft.com/cli/azure/cosmosdb).

| |  |
|---|---|
|**Vytvoření účtu Azure Cosmos DB, databáze a kontejnery**||
|[Vytvoření účtu DocumentDB rozhraní API, grafu nebo tabulky API](scripts/create-database-account-collections-cli.md?toc=%2fcli%2fazure%2ftoc.json)| Vytvoří jeden účet Azure Cosmos DB rozhraní API, databáze a kontejner pro použití s DocumentDB, grafu nebo tabulky rozhraní API. |
| [Vytvoření účtu MongoDB rozhraní API](scripts/create-mongodb-database-account-cli.md?toc=%2fcli%2fazure%2ftoc.json) | Vytvoří jeden účet, databázi a kolekci rozhraní API služby Azure Cosmos DB MongoDB. |
|**Škálování Azure Cosmos DB**||
| [Propustnost kontejneru měřítka](scripts/scale-collection-throughput-cli.md?toc=%2fcli%2fazure%2ftoc.json) | Změní zřízené througput do kontejneru.|
|[Replikaci databáze účtu Azure Cosmos DB do několika oblastí a konfigurace priorit převzetí služeb při selhání](scripts/scale-multiregion-cli.md?toc=%2fcli%2fazure%2ftoc.json)|Globálně replikuje data účtu do několika oblastí s prioritou zadaný převzetí služeb při selhání.|
|**Zabezpečení Azure Cosmos DB**||
| [Získání klíče účtu](scripts/secure-get-account-key-cli.md?toc=%2fcli%2fazure%2ftoc.json) | Získá hlavní zápisu primární a sekundární klíče a primární a sekundární klíče jen pro čtení pro účet.|
| [Získat připojovací řetězec MongoDB](scripts/secure-mongo-connection-string-cli.md?toc=%2fcli%2fazure%2ftoc.json) | Získá připojovací řetězec pro připojení k účtu Azure Cosmos DB aplikace MongoDB.|
|[Obnovit klíče účtu](scripts/secure-regenerate-key-cli.md?toc=%2fcli%2fazure%2ftoc.json)|Znovu vygeneruje klíč hlavní nebo jen pro čtení pro účet.|
|[Vytvoření brány firewall](scripts/create-firewall-cli.md?toc=%2fcli%2fazure%2ftoc.json)| Vytvoří příchozí IP zásadu řízení přístupu omezit přístup k účtu z schválené sadu počítačů nebo cloudových služeb.|
|**Vysoká dostupnost, zotavení po havárii, zálohování a obnovení**||
|[Konfigurace zásad převzetí služeb při selhání](scripts/ha-failover-policy-cli.md?toc=%2fcli%2fazure%2ftoc.json)|Nastaví prioritu převzetí služeb při selhání z každou oblast, ve kterém se replikuje účet.|
|**Připojení k prostředkům Azure Cosmos DB**||
|[Webovou aplikaci připojit k databázi Azure Cosmos](../app-service/scripts/app-service-cli-app-service-documentdb.md?toc=%2fcli%2fazure%2ftoc.json)|Vytvořit a připojit databázi Azure Cosmos DB a webové aplikace Azure.|
|||
