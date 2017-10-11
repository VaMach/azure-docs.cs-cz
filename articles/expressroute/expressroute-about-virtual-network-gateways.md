---
title: "O brány virtuální sítě ExpressRoute | Microsoft Docs"
description: "Další informace o brány virtuální sítě pro ExpressRoute."
services: expressroute
documentationcenter: na
author: cherylmc
manager: carmonm
editor: 
tags: azure-resource-manager, azure-service-management
ms.assetid: 7e0d9658-bc00-45b0-848f-f7a6da648635
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/05/2017
ms.author: cherylmc
ms.openlocfilehash: a6363fa380d0bab05d7500141cc6019d1d3f68b8
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="about-virtual-network-gateways-for-expressroute"></a>O branách virtuálních sítí pro ExpressRoute
Brána virtuální sítě se používá k posílání síťového provozu mezi virtuálními sítěmi Azure a místními umístěními. Při konfiguraci připojení ExpressRoute, musíte vytvořit a konfigurovat bránu virtuální sítě a připojení brány virtuální sítě.

Při vytváření brány virtuální sítě je třeba zadat několik nastavení. Jeden z požadovaných nastavení určuje, zda brány se použije pro provoz ExpressRoute nebo VPN typu Site-to-Site. V modelu nasazení Resource Manager, je nastavení '-GatewayType'.

Pokud síťový provoz se odesílají na privátní připojení, je použít typ brány, ExpressRoute. Ten se označuje také jako brána ExpressRoute. Pokud síťový provoz posílání šifrovaných přes veřejný Internet, je použít typ brány, Vpn. Ten se označuje také jako brána VPN. Připojení typu Site-to-Site, Point-to-Site a VNet-to-VNet používají bránu VPN.

Každá virtuální síť může mít pouze jednu bránu virtuální sítě pro každý typ brány. Například můžete mít jednu bránu virtuální sítě, která má typ -GatewayType Vpn a jednu, který má typ -GatewayType ExpressRoute. Tento článek se zaměřuje na bránu virtuální sítě.

## <a name="gwsku"></a>SKU brány
[!INCLUDE [expressroute-gwsku-include](../../includes/expressroute-gwsku-include.md)]

Pokud chcete upgradovat bránu výkonnější skladová položka brány, ve většině případů, můžete použít rutinu prostředí PowerShell, změny velikosti-AzureRmVirtualNetworkGateway'. To bude fungovat při upgradu Standard a HighPerformance SKU. Chcete-li upgradovat na UltraPerformance SKU, musíte ale bránu znovu vytvořte.

### <a name="aggthroughput"></a>Odhadovaná agregovaná propustnost podle skladová položka brány
Následující tabulka ukazuje typy brány a odhadovanou agregovanou propustnost. Tato tabulka platí pro model nasazení Resource Manager i pro klasický model.

[!INCLUDE [expressroute-table-aggthroughput](../../includes/expressroute-table-aggtput-include.md)]

> [!IMPORTANT]
> Propustnost aplikace závisí na několika faktorech, například na začátku do konce latenci a počet tok přenosů dat, které se aplikace spustí. Čísla v tabulce představují horní limit theorectically může aplikace dosáhnout v prostředí ideální. 
> 
>

## <a name="resources"></a>Rutiny prostředí PowerShell a rozhraní REST API
Další zdroje technických informací a specifickou syntaxi požadavky při použití rozhraní REST API a rutin prostředí PowerShell pro konfiguraci brány virtuální sítě najdete na následujících stránkách:

| **Classic** | **Resource Manager** |
| --- | --- |
| [PowerShell](https://msdn.microsoft.com/library/mt270335.aspx) |[PowerShell](https://msdn.microsoft.com/library/mt163510.aspx) |
| [REST API](https://msdn.microsoft.com/library/jj154113.aspx) |[REST API](https://msdn.microsoft.com/library/mt163859.aspx) |

## <a name="next-steps"></a>Další kroky
V tématu [přehled ExpressRoute](expressroute-introduction.md) pro další informace o konfiguracích dostupné připojení. 

