---
title: "Připojení místní sítě k virtuální síti Azure: Síť VPN typu Site-to-Site: Portál | Dokumentace Microsoftu"
description: "Postup vytvoření připojení IPsec z vaší místní sítě k virtuální síti Azure přes veřejný internet. Tyto kroky vám pomůžou vytvořit připojení VPN Gateway typu Site-to-Site mezi různými místy pomocí portálu."
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 827a4db7-7fa5-4eaf-b7e1-e1518c51c815
ms.service: vpn-gateway
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/04/2017
ms.author: cherylmc
translationtype: Human Translation
ms.sourcegitcommit: 785d3a8920d48e11e80048665e9866f16c514cf7
ms.openlocfilehash: d6f4caebeeced1286f24dd5fcb4f5fc7d8591785
ms.lasthandoff: 04/12/2017


---
# <a name="create-a-site-to-site-connection-in-the-azure-portal"></a>Vytvoření připojení typu Site-to-Site na webu Azure Portal

Připojení brány VPN typu Site-to-Site (S2S) je připojení přes tunel VPN prostřednictvím protokolu IPsec/IKE (IKEv1 nebo IKEv2). Tento typ připojení vyžaduje místní zařízení sítě VPN, které má přiřazenou veřejnou IP adresu a není umístěné za službou NAT. Připojení typu Site-to-Site lze použít pro konfigurace mezi různými místy a pro hybridní konfigurace.

![Diagram připojení VPN Gateway typu Site-to-Site mezi různými místy](./media/vpn-gateway-howto-site-to-site-resource-manager-portal/site-to-site-diagram.png)

Tento článek vás provede procesem vytvoření virtuální sítě a připojení ke službě VPN Gateway typu Site-to-Site k místní síti pomocí modelu nasazení Azure Resource Manager a webu Azure Portal. Tuto konfiguraci můžete také vytvořit pomocí jiných nástrojů nasazení nebo pro model nasazení Classic, a to výběrem jiné možnosti z následujícího seznamu:

> [!div class="op_single_selector"]
> * [Resource Manager – Azure Portal](vpn-gateway-howto-site-to-site-resource-manager-portal.md)
> * [Resource Manager – PowerShell](vpn-gateway-create-site-to-site-rm-powershell.md)
> * [Classic – Azure Portal](vpn-gateway-howto-site-to-site-classic-portal.md)
> * [Classic – portál Classic](vpn-gateway-site-to-site-create.md)
>
>


#### <a name="additional-configurations"></a>Další konfigurace
Informace o propojení virtuálních sítí bez vytvoření připojení k místnímu umístění najdete v tématu [Konfigurace připojení typu VNet-to-VNet](vpn-gateway-vnet-vnet-rm-ps.md). Pokud chcete přidat připojení Site-to-Site k virtuální síti, která už připojení má, získáte informace v části [Přidání připojení S2S k virtuální síti s existujícím připojením brány VPN](vpn-gateway-howto-multi-site-to-site-resource-manager-portal.md).

## <a name="before-you-begin"></a>Než začnete

[!INCLUDE [deployment models](../../includes/vpn-gateway-deployment-models-include.md)]

Před zahájením konfigurace ověřte, zda máte následující:

* Kompatibilní zařízení VPN a někoho, kdo jej umí nakonfigurovat. Viz [Informace o zařízeních VPN](vpn-gateway-about-vpn-devices.md).
* Pokud neznáte rozsahy IP adres ve vaší místní síti, budete se muset spojit s někým, kdo vám s tím pomůže. Připojení Site-to-Site nemůže mít překrývající se adresní prostory.
* Veřejnou IP adresu pro vaše zařízení VPN. Tato IP adresa nesmí být umístěná za překladem adres (NAT).
* Předplatné Azure. Pokud ještě nemáte předplatné Azure, můžete si aktivovat [výhody pro předplatitele MSDN](http://azure.microsoft.com/pricing/member-offers/msdn-benefits-details) nebo si zaregistrovat [bezplatný účet](http://azure.microsoft.com/pricing/free-trial).

### <a name="values"></a>Příklady hodnot
Používáte-li tyto kroky jako cvičení, můžete použít následující ukázkové hodnoty:

* **Název virtuální sítě:** TestVNet1
* **Adresní prostor:** 
    * 10.11.0.0/16
    * 10.12.0.0/16 (volitelné pro toto cvičení)
* **Podsítě:**
  * FrontEnd: 10.11.0.0/24
  * BackEnd: 10.12.0.0/24 (volitelné pro toto cvičení)
  * GatewaySubnet: 10.11.255.0/27
* **Skupina prostředků:** TestRG1
* **Umístění:** Východní USA
* **Server DNS:** IP adresa vašeho serveru DNS
* **Název brány virtuální sítě:** VNet1GW
* **Veřejná IP adresa:** VNet1GWIP
* **Typ sítě VPN:** Trasová
* **Typ připojení:** Site-to-Site (protokol IPsec)
* **Typ brány:** Síť VPN
* **Název brány místní sítě:** Site2
* **Název připojení:** VNet1toSite2

## <a name="CreatVNet"></a>1. Vytvoření virtuální sítě

[!INCLUDE [vpn-gateway-basic-vnet-rm-portal](../../includes/vpn-gateway-basic-vnet-s2s-rm-portal-include.md)]

## <a name="dns"></a>2. Určení serveru DNS
DNS se pro připojení Site-to-Site nevyžaduje. Pokud ale chcete umožnit překlad IP adres pro prostředky nasazované do vaší virtuální sítě, měli byste určit server DNS. Toto nastavení umožňuje určit server DNS, který chcete použít pro překlad IP adres pro tuto virtuální síť. Neslouží k vytvoření serveru DNS.

[!INCLUDE [vpn-gateway-add-dns-rm-portal](../../includes/vpn-gateway-add-dns-rm-portal-include.md)]

## <a name="gatewaysubnet"></a>3. Vytvoření podsítě brány
Pro bránu VPN je nutné vytvořit podsíť brány. Podsíť brány obsahuje IP adresy, které bude používat služba brány VPN. Pokud je to možné, vytvořte podsíť brány pomocí bloku CIDR /28 nebo /27. Tím se zajistí, že bude k dispozici dostatek IP adres pro uspokojení případných dalších funkcí brány v budoucnu.

[!INCLUDE [vpn-gateway-add-gwsubnet-rm-portal](../../includes/vpn-gateway-add-gwsubnet-s2s-rm-portal-include.md)]

## <a name="VNetGateway"></a>4. Vytvoření brány virtuální sítě

[!INCLUDE [vpn-gateway-add-gw-s2s-rm-portal](../../includes/vpn-gateway-add-gw-s2s-rm-portal-include.md)]

## <a name="LocalNetworkGateway"></a>5. Vytvoření brány místní sítě
Brána místní sítě odkazuje na vaše místní umístění. Nastavení, která zadáte pro bránu místní sítě, určují adresní prostory, které jsou směrovány do vašeho místního zařízení VPN.

[!INCLUDE [vpn-gateway-add-lng-s2s-rm-portal](../../includes/vpn-gateway-add-lng-s2s-rm-portal-include.md)]

## <a name="VPNDevice"></a>6. Konfigurace zařízení VPN
[!INCLUDE [vpn-gateway-configure-vpn-device-rm](../../includes/vpn-gateway-configure-vpn-device-rm-include.md)]

## <a name="CreateConnection"></a>7. Vytvoření připojení VPN typu Site-to-Site

V tomto kroku vytvoříte připojení VPN typu Site-to-Site mezi bránou virtuální sítě a místním zařízením VPN. Před zahájením této části ověřte, že vytvoření brány virtuální sítě a bran místní sítě proběhlo úspěšně.

[!INCLUDE [vpn-gateway-add-site-to-site-connection-rm-portal](../../includes/vpn-gateway-add-site-to-site-connection-s2s-rm-portal-include.md)]

## <a name="VerifyConnection"></a>8. Ověření připojení VPN

[!INCLUDE [Azure portal](../../includes/vpn-gateway-verify-connection-portal-rm-include.md)]

## <a name="next-steps"></a>Další kroky
*  Po dokončení připojení můžete do virtuálních sítí přidávat virtuální počítače. Další informace najdete v tématu [Virtuální počítače](https://docs.microsoft.com/azure/#pivot=services&panel=Compute).
*  Informace o protokolu BGP najdete v tématech [Přehled protokolu BGP](vpn-gateway-bgp-overview.md) a [Postup při konfiguraci protokolu BGP](vpn-gateway-bgp-resource-manager-ps.md).


