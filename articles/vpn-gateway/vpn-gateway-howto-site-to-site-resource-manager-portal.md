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
ms.date: 01/23/2017
ms.author: cherylmc
translationtype: Human Translation
ms.sourcegitcommit: afe143848fae473d08dd33a3df4ab4ed92b731fa
ms.openlocfilehash: 37311ea17004d8c5cfe9bfc9360c3972e39fb7f5
ms.lasthandoff: 03/17/2017


---
# <a name="create-a-vnet-with-a-site-to-site-connection-using-the-azure-portal"></a>Vytvoření virtuální sítě s připojením typu Site-to-Site pomocí webu Azure Portal
> [!div class="op_single_selector"]
> * [Resource Manager – Azure Portal](vpn-gateway-howto-site-to-site-resource-manager-portal.md)
> * [Resource Manager – PowerShell](vpn-gateway-create-site-to-site-rm-powershell.md)
> * [Classic – Azure Portal](vpn-gateway-howto-site-to-site-classic-portal.md)
> * [Classic – portál Azure Classic](vpn-gateway-site-to-site-create.md)
>
>

Tento článek vás provede procesem vytvoření virtuální sítě a připojení ke službě VPN Gateway typu Site-to-Site k místní síti pomocí modelu nasazení Azure Resource Manager a webu Azure Portal. Připojení typu Site-to-Site lze použít pro konfigurace mezi různými místy a pro hybridní konfigurace.

![Diagram připojení VPN Gateway typu Site-to-Site mezi různými místy](./media/vpn-gateway-howto-site-to-site-resource-manager-portal/site-to-site-diagram.png)

### <a name="deployment-models-and-methods-for-site-to-site-connections"></a>Modely nasazení a metody připojení typu Site-to-Site
[!INCLUDE [deployment models](../../includes/vpn-gateway-deployment-models-include.md)]

Následující tabulka uvádí aktuálně dostupné modely a metody nasazení v konfiguracích Site-to-Site. Když je článek s postupem konfigurace k dispozici, zařadíme do tabulky přímý odkaz na něj.

[!INCLUDE [site-to-site table](../../includes/vpn-gateway-table-site-to-site-include.md)]

#### <a name="additional-configurations"></a>Další konfigurace
Informace o propojení virtuálních sítí bez vytvoření připojení k místnímu umístění najdete v tématu [Konfigurace připojení typu VNet-to-VNet](vpn-gateway-vnet-vnet-rm-ps.md). Pokud chcete přidat připojení Site-to-Site k virtuální síti, která už připojení má, získáte informace v části [Přidání připojení S2S k virtuální síti s existujícím připojením brány VPN](vpn-gateway-howto-multi-site-to-site-resource-manager-portal.md).

## <a name="before-you-begin"></a>Než začnete
Před zahájením konfigurace ověřte, zda máte následující:

* Kompatibilní zařízení VPN a někoho, kdo jej umí nakonfigurovat. Viz [Informace o zařízeních VPN](vpn-gateway-about-vpn-devices.md). Pokud nevíte, jak nakonfigurovat zařízení VPN, nebo neznáte rozsahy IP adres v konfiguraci vaší místní sítě, budete se muset spojit s někým, kdo vám s tím pomůže.
* Veřejnou IP adresu pro vaše zařízení VPN. Tato IP adresa nesmí být umístěná za překladem adres (NAT).
* Předplatné Azure. Pokud ještě nemáte předplatné Azure, můžete si aktivovat [výhody pro předplatitele MSDN](http://azure.microsoft.com/pricing/member-offers/msdn-benefits-details) nebo si zaregistrovat [bezplatný účet](http://azure.microsoft.com/pricing/free-trial).

### <a name="values"></a>Ukázkové hodnoty konfigurace pro toto cvičení
Používáte-li tyto kroky jako cvičení, můžete použít ukázkové hodnoty konfigurace:

* **Název virtuální sítě:** TestVNet1
* **Adresní prostor:** 10.11.0.0/16 a 10.12.0.0/16
* **Podsítě:**
  * FrontEnd: 10.11.0.0/24
  * BackEnd: 10.12.0.0/24
  * GatewaySubnet: 10.12.255.0/27
* **Skupina prostředků:** TestRG1
* **Umístění:** Východní USA
* **Server DNS:** 8.8.8.8
* **Název brány:** VNet1GW
* **Veřejná IP adresa:** VNet1GWIP
* **Typ sítě VPN:** Trasová
* **Typ připojení:** Site-to-Site (protokol IPsec)
* **Typ brány:** Síť VPN
* **Název brány místní sítě:** Site2
* **Název připojení:** VNet1toSite2

## <a name="CreatVNet"></a>1. Vytvoření virtuální sítě
Pokud již máte virtuální síť vytvořenou, ověřte, zda jsou nastavení kompatibilní s vaším návrhem brány VPN. Věnujte zvláštní pozornost všem podsítím, které by se mohly překrývat s jinými sítěmi. Pokud se podsítě překrývají, připojení nebude fungovat správně. Pokud je vaše virtuální síť nakonfigurována se správným nastavením, můžete začít s kroky v oddílu [Určení serveru DNS](#dns).

### <a name="to-create-a-virtual-network"></a>Chcete-li vytvořit virtuální síť
[!INCLUDE [vpn-gateway-basic-vnet-rm-portal](../../includes/vpn-gateway-basic-vnet-rm-portal-include.md)]

## <a name="subnets"></a>2. Přidání dalšího adresního prostoru a podsítí
Po vytvoření virtuální sítě do ní můžete přidat další adresní prostor a podsítě.

[!INCLUDE [vpn-gateway-additional-address-space](../../includes/vpn-gateway-additional-address-space-include.md)]

## <a name="dns"></a>3. Určení serveru DNS
### <a name="to-specify-a-dns-server"></a>Chcete-li určit server DNS
[!INCLUDE [vpn-gateway-add-dns-rm-portal](../../includes/vpn-gateway-add-dns-rm-portal-include.md)]

## <a name="gatewaysubnet"></a>4. Vytvoření podsítě brány
Před připojením virtuální sítě k bráně musíte nejdříve vytvořit podsíť brány pro virtuální síť, ke které se chcete připojit. Pokud je to možné, je nejlepší vytvořit podsíť brány s použitím bloku CIDR /28 nebo /27, aby byl k dispozici dostatek IP adres pro plnění dalších požadavků na konfiguraci v budoucnu.

Pokud vytváříte tuto konfiguraci jako cvičení, při vytváření podsítě brány použijte tyto [hodnoty](#values).

### <a name="to-create-a-gateway-subnet"></a>Chcete-li vytvořit podsíť brány
[!INCLUDE [vpn-gateway-add-gwsubnet-rm-portal](../../includes/vpn-gateway-add-gwsubnet-rm-portal-include.md)]

## <a name="VNetGateway"></a>5. Vytvoření brány virtuální sítě
Pokud vytváříte tuto konfiguraci jako cvičení, při vytváření brány můžete použít [ukázkové hodnoty konfigurace](#values).

### <a name="to-create-a-virtual-network-gateway"></a>Chcete-li vytvořit bránu virtuální sítě
[!INCLUDE [vpn-gateway-add-gw-rm-portal](../../includes/vpn-gateway-add-gw-rm-portal-include.md)]

## <a name="LocalNetworkGateway"></a>6. Vytvoření brány místní sítě
Brána místní sítě odkazuje na vaše místní umístění. Zadejte pro bránu místní sítě název, pomocí kterého se na ni bude Azure odkazovat. 

Pokud vytváříte tuto konfiguraci jako cvičení, při vytváření brány můžete použít [ukázkové hodnoty konfigurace](#values).

### <a name="to-create-a-local-network-gateway"></a>Chcete-li vytvořit bránu místní sítě
[!INCLUDE [vpn-gateway-add-lng-rm-portal](../../includes/vpn-gateway-add-lng-rm-portal-include.md)]

## <a name="VPNDevice"></a>7. Konfigurace zařízení VPN
[!INCLUDE [vpn-gateway-configure-vpn-device-rm](../../includes/vpn-gateway-configure-vpn-device-rm-include.md)]

## <a name="CreateConnection"></a>8. Vytvoření připojení VPN typu Site-to-Site
Vytvořte připojení VPN typu Site-to-Site mezi bránou virtuální sítě a zařízením VPN. Nezapomeňte hodnoty nahradit vlastními. Sdílený klíč se musí shodovat s hodnotou, kterou jste použili pro konfiguraci zařízení VPN. 

Před zahájením této části ověřte, že vytvoření brány virtuální sítě a bran místní sítě proběhlo úspěšně. Pokud vytváříte tuto konfiguraci jako cvičení, při vytváření připojení použijte tyto [hodnoty](#values).

### <a name="to-create-the-vpn-connection"></a>Chcete-li vytvořit připojení VPN
[!INCLUDE [vpn-gateway-add-site-to-site-connection-rm-portal](../../includes/vpn-gateway-add-site-to-site-connection-rm-portal-include.md)]

## <a name="VerifyConnection"></a>9. Ověření připojení VPN
Připojení VPN můžete ověřit v portálu nebo pomocí prostředí PowerShell.

[!INCLUDE [vpn-gateway-verify-connection-rm](../../includes/vpn-gateway-verify-connection-rm-include.md)]

## <a name="next-steps"></a>Další kroky
*  Po dokončení připojení můžete do virtuálních sítí přidávat virtuální počítače. Další informace najdete v tématu [Virtuální počítače](https://docs.microsoft.com/azure/#pivot=services&panel=Compute).
*  Informace o protokolu BGP najdete v tématech [Přehled protokolu BGP](vpn-gateway-bgp-overview.md) a [Postup při konfiguraci protokolu BGP](vpn-gateway-bgp-resource-manager-ps.md).


