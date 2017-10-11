---
title: "SKU brány starší virtuální síť Azure | Microsoft Docs"
description: "Starý virtuální sítě SKU brány."
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager,azure-service-management
ms.assetid: 
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/01/2017
ms.author: cherylmc
ms.openlocfilehash: 3b2126b1ecd1613950bbf311ae08fafd4af0d51f
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="working-with-virtual-network-gateway-skus-legacy-skus"></a>Práce s SKU (starší verze SKU) brány virtuální sítě

Tento článek obsahuje informace o starší verze (starý) brány virtuální sítě SKU. Starší verze SKU i nadále fungovat v obou modelech nasazení pro brány sítě VPN, které již byly vytvořeny. Classic brány VPN dál používat starší verze SKU pro existující brány i pro nové brány. Při vytváření nového správce prostředků VPN Gateway, použijte nové SKU brány. Informace o nové SKU najdete v tématu [o službě VPN Gateway](vpn-gateway-about-vpngateways.md).

## <a name="gwsku"></a>SKU brány

[!INCLUDE [Legacy gateway SKUs](../../includes/vpn-gateway-gwsku-legacy-include.md)]

## <a name="agg"></a>Odhadovaná agregovaná propustnost podle SKU

[!INCLUDE [Aggregated throughput by legacy SKU](../../includes/vpn-gateway-table-gwtype-legacy-aggtput-include.md)]

## <a name="config"></a>Podporované konfigurace podle typu SKU a síť VPN

[!INCLUDE [Table requirements for old SKUs](../../includes/vpn-gateway-table-requirements-legacy-sku-include.md)]

## <a name="resize"></a>Změnit velikost brány (změnit a skladová položka brány)

Můžete změnit velikost skladová položka brány v rámci stejné řada SKU. Například pokud máte standardní SKU, můžete změnit velikost na HighPerformance SKU. Nelze změnit velikost vašich bran VPN mezi SKU staré a nové rodiny SKU. Například nelze přejít z standardní SKU VpnGw2 SKU. 

Ke změně velikosti skladová položka brány pro model nasazení classic, použijte následující příkaz:

```powershell
Resize-AzureVirtualNetworkGateway -GatewayId <Gateway ID> -GatewaySKU HighPerformance
```

Ke změně velikosti skladová položka brány pro model nasazení Resource Manager, použijte následující příkaz:

```powershell
$gw = Get-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg
Resize-AzureRmVirtualNetworkGateway -VirtualNetworkGateway $gw -GatewaySku HighPerformance
```

## <a name="migrate"></a>Migrace na nové bráně SKU

Pokud pracujete s modelem nasazení Resource Manager, můžete migrovat do nového SKU brány. Pokud pracujete s modelem nasazení classic, nemůže se migrovat do nového SKU a místo toho musíte dál používat starší verze SKU.

[!INCLUDE [Migrate SKU](../../includes/vpn-gateway-migrate-legacy-sku-include.md)]

## <a name="next-steps"></a>Další kroky

Další informace o nové SKU brány najdete v tématu [SKU brány](vpn-gateway-about-vpngateways.md#gwsku).

Další informace o nastavení konfigurace najdete v tématu [nastavení konfigurace o službě VPN Gateway](vpn-gateway-about-vpn-gateway-settings.md).