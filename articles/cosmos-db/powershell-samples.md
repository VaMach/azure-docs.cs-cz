---
title: "Ukázky pro Azure Cosmos DB Azure PowerShell | Microsoft Docs"
description: "Ukázek Azure PowerShell - skripty, které vám pomůžou vytvářet a spravovat účty pro Azure Cosmos DB."
services: cosmos-db
author: mimig1
manager: jhubbard
editor: 
tags: azure-service-management
ms.assetid: 
ms.service: cosmos-db
ms.custom: mvc
ms.devlang: na
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: database
ms.date: 05/10/2017
ms.author: mimig
ms.openlocfilehash: 7698e03c0dc8d1c6d1e926f45e903a909bfd0c93
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="azure-powershell-samples-for-azure-cosmos-db"></a>Ukázek Azure PowerShell pro Azure Cosmos DB

Následující tabulka obsahuje odkazy na ukázkové skripty prostředí Azure PowerShell pro Azure Cosmos DB.

| |  |
|---|---|
|**Vytvoření účtu Azure Cosmos DB**||
|[Vytvoření účtu DocumentDB rozhraní API](scripts/create-database-account-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Vytvoří jeden účet Azure Cosmos DB používat s rozhraním API pro DocumentDB. |
|**Škálování Azure Cosmos DB**||
|[Replikaci účtu Azure Cosmos DB do několika oblastí a konfigurace priorit převzetí služeb při selhání](scripts/scale-multiregion-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)|Globálně replikuje data účtu do několika oblastí s prioritou zadaný převzetí služeb při selhání.|
|**Zabezpečení Azure Cosmos DB**||
| [Získání klíče účtu](scripts/secure-get-account-key-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Získá hlavní zápisu primární a sekundární klíče a primární a sekundární klíče jen pro čtení pro účet.|
| [Získat připojovací řetězec MongoDB](scripts/secure-mongo-connection-string-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Získá připojovací řetězec pro připojení k účtu Azure Cosmos DB aplikace MongoDB.|
|[Obnovit klíče účtu](scripts/secure-regenerate-key-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)|Znovu vygeneruje klíč hlavní nebo jen pro čtení pro účet.|
|[Vytvoření brány firewall](scripts/create-firewall-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Vytvoří příchozí IP zásadu řízení přístupu omezit přístup k účtu z schválené sadu počítačů nebo cloudových služeb.|
|**Vysoká dostupnost, zotavení po havárii, zálohování a obnovení**||
|[Konfigurace zásad převzetí služeb při selhání](scripts/ha-failover-policy-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)|Nastaví prioritu převzetí služeb při selhání z každou oblast, ve kterém se replikuje účet.|
|||