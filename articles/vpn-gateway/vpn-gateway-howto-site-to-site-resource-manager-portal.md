<properties
   pageTitle="Vytvoření virtuální sítě s připojením VPN typu Site-to-Site pomocí Azure Resource Manageru a webu Azure Portal | Microsoft Azure"
   description="Tento článek vás provede procesem vytvoření virtuální sítě pomocí modelu Resource Manageru a jejím propojením s vaší místní sítí pomocí připojení S2S brány VPN."
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="05/13/2016"
   ms.author="cherylmc"/>

# Vytvoření virtuální sítě s připojením VPN typu Site-to-Site pomocí webu Azure Portal a Azure Resource Manageru

> [AZURE.SELECTOR]
- [Azure Portal](vpn-gateway-howto-site-to-site-resource-manager-portal.md)
- [Portál Azure Classic](vpn-gateway-site-to-site-create.md)
- [PowerShell – Resource Manager](vpn-gateway-create-site-to-site-rm-powershell.md)


Tento článek vás provede procesem vytvoření virtuální sítě a připojení VPN typu Site-to-Site k místní síti pomocí modelu nasazení Azure Resource Manager a webu Azure Portal. V následujících krocích vytvoříte virtuální síť a přidáte podsíť brány, bránu, místní lokalitu a připojení. Kromě toho budete muset konfigurovat své zařízení VPN. 



**O modelech nasazení Azure**

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)] 

## Diagram připojení

![Site-to-Site](./media/vpn-gateway-howto-site-to-site-resource-manager-portal/site2site.png)

**Modely nasazení a nástroje pro připojení typu Site-to-Site**

[AZURE.INCLUDE [vpn-gateway-table-site-to-site-table](../../includes/vpn-gateway-table-site-to-site-include.md)] 

Informace o propojení virtuálních sítí bez vytvoření připojení k místnímu umístění najdete v tématu [Konfigurace připojení typu VNet-to-VNet](vpn-gateway-vnet-vnet-rm-ps.md). Další typy nastavení připojení najdete v článku [Topologie připojení ke službě VPN Gateway](vpn-gateway-topology.md).

## Než začnete

Před zahájením konfigurace ověřte, zda máte následující:

- Kompatibilní zařízení VPN a někoho, kdo jej umí nakonfigurovat. Viz [Informace o zařízeních VPN](vpn-gateway-about-vpn-devices.md). Pokud nevíte jak nakonfigurovat zařízení VPN, nebo neznáte rozsahy IP adres v konfiguraci vaší místní sítě, budete se muset spojit s někým, kdo vám s tím pomůže.

- Veřejnou IP adresu pro vaše zařízení VPN. Tato IP adresa nesmí být umístěná za překladem adres (NAT).
    
- Předplatné Azure. Pokud ještě nemáte předplatné Azure, můžete si aktivovat [výhody pro předplatitele MSDN](http://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) nebo si zaregistrovat [bezplatný účet](http://azure.microsoft.com/pricing/free-trial/).

### <a name="values"></a>Ukázkové hodnoty konfigurace pro toto cvičení


Používáte-li tyto kroky jako cvičení, můžete použít ukázkové hodnoty konfigurace:

- Název virtuální sítě: TestVNet1
- Adresní prostor: 10.11.0.0/16 a 10.12.0.0/16
- Podsítě: 
    - FrontEnd: 10.11.0.0/24
    - BackEnd: 10.12.0.0/24
    - GatewaySubnet: 10.12.255.0/27
- Skupina prostředků: TestRG1
- Umístění: Východní USA
- Server DNS: 8.8.8.8
- Název brány: VNet1GW
- Veřejná IP adresa: VNet1GWIP
- Typ sítě VPN: Trasová
- Typ připojení: Site-to-Site (protokol IPsec)
- Typ brány: Síť VPN
- Název brány místní sítě: Site2
- Název připojení: VNet1toSite2



## 1. Vytvoření virtuální sítě 

Pokud jste již vytvořili virtuální síť, ověřte, že je její nastavení kompatibilní s návrhem vaší brány VPN. Zaměřte se především na podsítě, které se mohou překrývat s jinými sítěmi. Budou-li se podsítě překrývat, vaše připojení nebude fungovat správně. Když ověříte, že je vaše virtuální síť nakonfigurována se správným nastavením, můžete začít s kroky v oddílu [Určení serveru DNS](#dns).

### Chcete-li vytvořit virtuální síť

[AZURE.INCLUDE [vpn-gateway-basic-vnet-rm-portal](../../includes/vpn-gateway-basic-vnet-rm-portal-include.md)]  

## 2. Přidání dalšího adresního prostoru a podsítí

Po vytvoření virtuální sítě do ní můžete přidat další adresní prostor a podsítě.

[AZURE.INCLUDE [vpn-gateway-additional-address-space](../../includes/vpn-gateway-additional-address-space-include.md)] 

## <a name="dns"></a>3. Určení serveru DNS

Pokud vytváříte tuto konfiguraci jako cvičení, při určování serveru DNS použijte tyto [hodnoty](#values).

### Chcete-li určit server DNS

[AZURE.INCLUDE [vpn-gateway-add-dns-rm-portal](../../includes/vpn-gateway-add-dns-rm-portal-include.md)]

## 4. Vytvoření podsítě brány

Před připojením virtuální sítě k bráně musíte nejdříve vytvořit podsíť brány pro virtuální síť, ke které se chcete připojit. Vytvořená podsíť brány musí mít název *GatewaySubnet*, jinak nebude správně fungovat. 

Předpona podsítě brány pro některé konfigurace vyžaduje velikost podsítě /28 nebo větší, aby mohla pojmout množství IP adres ve fondu. To znamená, že předpona podsítě brány musí být /28, /27, /26 atd. V této fázi můžete chtít vytvořit větší podsíť, aby v budoucnu mohla pojmout případné dodatečné konfigurace.

Pokud vytváříte tuto konfiguraci jako cvičení, při vytváření podsítě brány použijte tyto [hodnoty](#values).

### Chcete-li vytvořit podsíť brány

[AZURE.INCLUDE [vpn-gateway-no-nsg](../../includes/vpn-gateway-no-nsg-include.md)] 

[AZURE.INCLUDE [vpn-gateway-add-gwsubnet-rm-portal](../../includes/vpn-gateway-add-gwsubnet-rm-portal-include.md)]

## 5. Vytvoření brány virtuální sítě

Pokud vytváříte tuto konfiguraci jako cvičení, při vytváření brány použijte tyto [hodnoty](#values).

### Chcete-li vytvořit bránu virtuální sítě

[AZURE.INCLUDE [vpn-gateway-add-gw-rm-portal](../../includes/vpn-gateway-add-gw-rm-portal-include.md)]

## 6. Vytvoření brány místní sítě

*Brána místní sítě* odkazuje na vaše místní umístění. Bráně místní sítě dáte název, pomocí kterého se na ni bude Azure odkazovat. 

Pokud vytváříte tuto konfiguraci jako cvičení, při přidávání místní lokality použijte tyto [hodnoty](#values).

### Chcete-li vytvořit bránu místní sítě

[AZURE.INCLUDE [vpn-gateway-add-lng-rm-portal](../../includes/vpn-gateway-add-lng-rm-portal-include.md)]

## 7. Konfigurace zařízení VPN

[AZURE.INCLUDE [vpn-gateway-configure-vpn-device-rm](../../includes/vpn-gateway-configure-vpn-device-rm-include.md)]

## 8. Vytvoření připojení VPN typu Site-to-Site

Dále vytvoříte připojení VPN typu Site-to-Site mezi bránou virtuální sítě a zařízením VPN. Nezapomeňte hodnoty nahradit vlastními. Sdílený klíč se musí shodovat s hodnotou, kterou jste použili pro konfiguraci zařízení VPN. 

Před zahájením této části ověřte, že vytvoření brány virtuální sítě a bran místní sítě proběhlo úspěšně. Pokud vytváříte tuto konfiguraci jako cvičení, při vytváření připojení použijte tyto [hodnoty](#values).

### Chcete-li vytvořit připojení VPN


[AZURE.INCLUDE [vpn-gateway-add-site-to-site-connection-rm-portal](../../includes/vpn-gateway-add-site-to-site-connection-rm-portal-include.md)]

## 9. Ověření připojení VPN

Připojení VPN můžete ověřit v portálu nebo pomocí prostředí PowerShell.

[AZURE.INCLUDE [vpn-gateway-verify-connection-rm](../../includes/vpn-gateway-verify-connection-rm-include.md)]

## Další kroky

- Po dokončení připojení můžete do virtuálních sítí přidávat virtuální počítače. Další informace najdete v [naučné stezce](https://azure.microsoft.com/documentation/learning-paths/virtual-machines) pro virtuální počítače.

- Informace o protokolu BGP najdete v tématech [Přehled protokolu BGP](vpn-gateway-bgp-overview.md) a [Postup při konfiguraci protokolu BGP](vpn-gateway-bgp-resource-manager-ps.md).



<!--HONumber=Jun16_HO2-->


