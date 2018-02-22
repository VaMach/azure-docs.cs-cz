---
title: "Vytvoření instance sledovací proces sítě Azure | Microsoft Docs"
description: "Tato stránka obsahuje kroky k vytvoření instance sledovací proces sítě pomocí portálu a REST API služby Azure"
services: network-watcher
documentationcenter: na
author: jimdial
manager: timlt
editor: 
ms.assetid: b1314119-0b87-4f4d-b44c-2c4d0547fb76
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: jdial
ms.openlocfilehash: fb1e3f9a93d18e949cb42ac0a4e09129cfe414f6
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/21/2018
---
# <a name="create-an-azure-network-watcher-instance"></a>Vytvoření instance sledovací proces sítě Azure

Sledovací proces sítě je místní služba, která umožňuje sledovat a diagnostikovat podmínky na úrovni scénář sítě, do a z Azure. Scénář úrovně monitorování umožňuje diagnostikovat problémy na úrovni zobrazení koncová sítě. Diagnostika sítě a k dispozici sledovací proces sítě vizualizace nástroje vám pomůžou pochopit, diagnostikovat a získáte přehled o k síti v Azure.

> [!NOTE]
> Sledovací proces sítě aktuálně podporuje pouze rozhraní příkazového řádku 1.0, pokyny pro vytvoření nové instance sledovací proces sítě se poskytuje pro rozhraní příkazového řádku 1.0.

## <a name="create-a-network-watcher-in-the-portal"></a>Vytvoření sledovací proces sítě v portálu.

Přejděte na **všechny služby** > **sítě** > **sítě sledovacích procesů**. Můžete vybrat všechny odběry, které chcete povolit sledovací proces sítě pro. Tato akce vytvoří sledovací proces sítě v každé oblasti, která je k dispozici.

![Vytvoření sledovací proces sítě][1]

Když povolíte sledovací proces sítě pomocí portálu, název instance sledovací proces sítě automaticky nastaví NetworkWatcher_region_name kde region_name odpovídá oblasti Azure, podporou instance.  Například sledovací proces sítě povolené v oblasti západní centrální USA bude mít název NetworkWatcher_westcentralus

Kromě toho instance sledovací proces sítě se automaticky přidá do skupinu prostředků s názvem NetworkWatcherRG.  Tato skupina prostředků bude vytvořen, pokud ještě neexistuje.

Pokud chcete přizpůsobit název instance sledovací proces sítě a skupina prostředků je umístěn do, můžete použít prostředí Powershell, REST API nebo ARMClient metod popsaných níže.  V každé možnosti musí existovat skupině prostředků než umístíte sledovací proces sítě do ní.  

## <a name="create-a-network-watcher-with-powershell"></a>Vytvoření sledovací proces sítě pomocí prostředí PowerShell

Chcete-li vytvořit instanci sledovací proces sítě, spusťte v následujícím příkladu:

```powershell
New-AzureRmNetworkWatcher -Name "NetworkWatcher_westcentralus" -ResourceGroupName "NetworkWatcherRG" -Location "West Central US"
```

## <a name="create-a-network-watcher-with-the-rest-api"></a>Vytvoření sledovací proces sítě pomocí rozhraní REST API

ARMclient se používá k volání rozhraní REST API pomocí prostředí PowerShell. ARMClient se nachází na chocolatey v [ARMClient na Chocolatey](https://chocolatey.org/packages/ARMClient)

### <a name="log-in-with-armclient"></a>Přihlaste se pomocí ARMClient

```powerShell
armclient login
```

### <a name="create-the-network-watcher"></a>Vytvoření sledovací proces sítě

```powershell
$subscriptionId = '<subscription id>'
$networkWatcherName = '<name of network watcher>'
$resourceGroupName = '<resource group name>'
$apiversion = "2016-09-01"
$requestBody = @"
{
'location': 'West Central US'
}
"@

armclient put "https://management.azure.com/subscriptions/${subscriptionId}/resourceGroups/${resourceGroupName}/providers/Microsoft.Network/networkWatchers/${networkWatcherName}?api-version=${api-version}" $requestBody
```

## <a name="next-steps"></a>Další postup

Teď, když máte instanci sledovací proces sítě, informace o funkcích, které jsou k dispozici:

* [Topologie](network-watcher-topology-overview.md)
* [Zachytávání paketů](network-watcher-packet-capture-overview.md)
* [Ověření IP toku](network-watcher-ip-flow-verify-overview.md)
* [Další směrování](network-watcher-next-hop-overview.md)
* [Zobrazení skupin zabezpečení](network-watcher-security-group-view-overview.md)
* [Protokolování toku NSG](network-watcher-nsg-flow-logging-overview.md)
* [Řešení potíží virtuální síťová brána](network-watcher-troubleshoot-overview.md)

Po vytvoření instance sledovací proces sítě zachycení balíčku lze nastavit pomocí následujících článek: [vytvořit zaznamenání výstrahy spouštěná paketu](network-watcher-alert-triggered-packet-capture.md)

[1]: ./media/network-watcher-create/figure1.png











