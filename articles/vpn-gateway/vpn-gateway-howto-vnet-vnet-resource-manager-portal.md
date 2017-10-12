---
title: "Připojení virtuální sítě Azure k jiné virtuální síti: Portál | Dokumentace Microsoftu"
description: "Vytvořte připojení brány VPN mezi virtuálními sítěmi pomocí Resource Manageru a webu Azure Portal."
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: a7015cfc-764b-46a1-bfac-043d30a275df
ms.service: vpn-gateway
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/02/2017
ms.author: cherylmc
ms.openlocfilehash: cbbae06d4e5774d5fdf7bcce65dc7efddaa5f280
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="configure-a-vnet-to-vnet-vpn-gateway-connection-using-the-azure-portal"></a>Konfigurace propojení brány VPN typu VNet-to-VNet pomocí webu Azure Portal

Tento článek ukazuje, jak vytvořit připojení brány VPN mezi virtuálními sítěmi. Virtuální sítě se můžou nacházet ve stejné oblasti nebo v různých oblastech a můžou patřit do stejného předplatného nebo do různých předplatných. Pokud připojujete virtuální sítě z různých předplatných, tato předplatná nemusí být přidružená ke stejnému tenantovi Active Directory. 

Postupy v tomto článku se týkají modelu nasazení Resource Manager a používají Azure Portal. Tuto konfiguraci můžete vytvořit také pomocí jiného nástroje nasazení nebo pro jiný model nasazení, a to výběrem jiné možnosti z následujícího seznamu:

> [!div class="op_single_selector"]
> * [Azure Portal](vpn-gateway-howto-vnet-vnet-resource-manager-portal.md)
> * [PowerShell](vpn-gateway-vnet-vnet-rm-ps.md)
> * [Azure CLI](vpn-gateway-howto-vnet-vnet-cli.md)
> * [Azure Portal (Classic)](vpn-gateway-howto-vnet-vnet-portal-classic.md)
> * [Propojení různých modelů nasazení – Azure Portal](vpn-gateway-connect-different-deployment-models-portal.md)
> * [Propojení různých modelů nasazení – PowerShell](vpn-gateway-connect-different-deployment-models-powershell.md)
>
>

![Diagram v2v](./media/vpn-gateway-howto-vnet-vnet-resource-manager-portal/v2vrmps.png)

Propojení virtuální sítě s jinou virtuální sítí (VNet-to-VNet) je podobné propojení virtuální sítě s místním serverem. Oba typy připojení využívají bránu VPN k poskytnutí zabezpečeného tunelového propojení prostřednictvím protokolu IPsec/IKE. Pokud se virtuální sítě nacházejí ve stejné oblasti, můžete uvažovat o jejich propojení vytvořením partnerského vztahu virtuálních sítí. Partnerské vztahy virtuálních sítí nepoužívají bránu VPN. Další informace najdete v tématu [Partnerské vztahy virtuálních sítí](../virtual-network/virtual-network-peering-overview.md).

Komunikaci typu VNet-to-VNet můžete kombinovat s konfiguracemi s více servery. Díky tomu je možné vytvářet topologie sítí, ve kterých se používá propojování více míst i propojování virtuálních sítí, jak je znázorněno v následujícím schématu:

![Informace o připojeních](./media/vpn-gateway-howto-vnet-vnet-resource-manager-portal/aboutconnections.png "Informace o připojeních")

### <a name="why-connect-virtual-networks"></a>Proč propojovat virtuální sítě?

Virtuální sítě může být vhodné propojit z následujících důvodů:

* **Geografická redundance napříč oblastmi a geografická přítomnost**
  
  * Můžete nastavit vlastní geografickou replikaci nebo synchronizaci se zabezpečeným připojením bez procházení koncovými body připojenými k internetu.
  * Pomocí Azure Traffic Manageru a služby Load Balancer je možné vytvářet úlohy s vysokou dostupností s geografickou redundancí nad několika oblastmi Azure. Jedním z důležitých příkladů je nastavení technologie SQL Always On se skupinami dostupnosti nad několika oblastmi Azure.
* **Regionální vícevrstvé aplikace s izolací nebo administrativní hranicí**
  
  * V rámci stejné oblasti můžete vytvářet vícevrstvé aplikace s několika virtuálními sítěmi propojenými z důvodu izolace nebo požadavků na správu.

Další informace o propojeních VNet-to-VNet najdete v části [Nejčastější dotazy týkající se propojení VNet-to-VNet](#faq) na konci tohoto článku. Všimněte si, že pokud jsou vaše virtuální sítě v různých předplatných, nelze vytvořit připojení na portálu. Můžete použít [PowerShell](vpn-gateway-vnet-vnet-rm-ps.md) nebo [rozhraní příkazového řádku](vpn-gateway-howto-vnet-vnet-cli.md).

### <a name="values"></a>Příklady nastavení

Používáte-li tyto kroky jako cvičení, můžete použít ukázkové hodnoty nastavení. Pro účely tohoto příkladu použijeme pro jednotlivé virtuální sítě více adresních prostorů. Konfigurace VNet-to-VNet nicméně použití více adresních prostorů nevyžadují.

**Hodnoty pro virtuální síť TestVNet1:**

* Název virtuální sítě: TestVNet1
* Adresní prostor: 10.11.0.0/16
  * Název podsítě: FrontEnd
  * Rozsah adres podsítě: 10.11.0.0/24
* Skupina prostředků: TestRG1
* Umístění: Východní USA
* Adresní prostor: 10.12.0.0/16
  * Název podsítě: BackEnd
  * Rozsah adres podsítě: 10.12.0.0/24
* Název podsítě brány: GatewaySubnet (na portálu se sám vyplní)
  * Rozsah adres podsítě brány: 10.11.255.0/27
* Server DNS: Použijte IP adresu svého serveru DNS.
* Název brány virtuální sítě: TestVNet1GW
* Typ brány: Síť VPN
* Typ sítě VPN: Založená na trasách
* SKU: Vyberte skladovou jednotku (SKU) brány, kterou chcete použít.
* Název veřejné IP adresy: TestVNet1GWIP
* Hodnoty připojení:
  * Název: TestVNet1toTestVNet4
  * Sdílený klíč: Sdílený klíč si můžete vytvořit sami. V tomto příkladu použijeme abc123. Důležité je, aby se při vytváření propojení virtuálních sítí tato hodnota shodovala.

**Hodnoty pro virtuální síť TestVNet4:**

* Název virtuální sítě: TestVNet4
* Adresní prostor: 10.41.0.0/16
  * Název podsítě: FrontEnd
  * Rozsah adres podsítě: 10.41.0.0/24
* Skupina prostředků: TestRG1
* Umístění: Západní USA
* Adresní prostor: 10.42.0.0/16
  * Název podsítě: BackEnd
  * Rozsah adres podsítě: 10.42.0.0/24
* Název podsítě brány: GatewaySubnet (na portálu se sám vyplní)
  * Rozsah adres podsítě brány: 10.41.255.0/27
* Server DNS: Použijte IP adresu svého serveru DNS.
* Název brány virtuální sítě: TestVNet4GW
* Typ brány: Síť VPN
* Typ sítě VPN: Založená na trasách
* SKU: Vyberte skladovou jednotku (SKU) brány, kterou chcete použít.
* Název veřejné IP adresy: TestVNet4GWIP
* Hodnoty připojení:
  * Název: TestVNet4toTestVNet1
  * Sdílený klíč: Sdílený klíč si můžete vytvořit sami. V tomto příkladu použijeme abc123. Důležité je, aby se při vytváření propojení virtuálních sítí tato hodnota shodovala.

## <a name="CreatVNet"></a>1. Vytvoření a konfigurace virtuální sítě TestVNet1
Pokud již máte virtuální síť vytvořenou, ověřte, zda jsou nastavení kompatibilní s vaším návrhem brány VPN. Věnujte zvláštní pozornost všem podsítím, které by se mohly překrývat s jinými sítěmi. Pokud se podsítě překrývají, připojení nebude fungovat správně. Pokud je vaše virtuální síť nakonfigurována se správným nastavením, můžete začít s kroky v oddílu [Určení serveru DNS](#dns).

### <a name="to-create-a-virtual-network"></a>Chcete-li vytvořit virtuální síť
[!INCLUDE [vpn-gateway-basic-vnet-rm-portal](../../includes/vpn-gateway-basic-vnet-rm-portal-include.md)]

## <a name="subnets"></a>2. Přidání dalšího adresního prostoru a vytvoření podsítí
Po vytvoření virtuální sítě můžete přidat další adresní prostor a vytvořit podsítě.

[!INCLUDE [vpn-gateway-additional-address-space](../../includes/vpn-gateway-additional-address-space-include.md)]

## <a name="gatewaysubnet"></a>3. Vytvoření podsítě brány
Před připojením virtuální sítě k bráně musíte nejdříve vytvořit podsíť brány pro virtuální síť, ke které se chcete připojit. Pokud je to možné, je nejlepší vytvořit podsíť brány s použitím bloku CIDR /28 nebo /27, aby byl k dispozici dostatek IP adres pro plnění dalších požadavků na konfiguraci v budoucnu.

Pokud vytváříte tuto konfiguraci jako cvičení, při vytváření podsítě brány použijte tyto [příklady nastavení](#values).

[!INCLUDE [vpn-gateway-no-nsg](../../includes/vpn-gateway-no-nsg-include.md)]

### <a name="to-create-a-gateway-subnet"></a>Chcete-li vytvořit podsíť brány
[!INCLUDE [vpn-gateway-add-gwsubnet-rm-portal](../../includes/vpn-gateway-add-gwsubnet-rm-portal-include.md)]

## <a name="dns"></a>4. Určení serveru DNS (volitelné)
DNS není pro připojení VNet-to-VNet vyžadováno. Pokud ale chcete umožnit překlad IP adres pro prostředky nasazované do vaší virtuální sítě, měli byste určit server DNS. Toto nastavení umožňuje určit server DNS, který chcete použít pro překlad IP adres pro tuto virtuální síť. Neslouží k vytvoření serveru DNS.

[!INCLUDE [vpn-gateway-add-dns-rm-portal](../../includes/vpn-gateway-add-dns-rm-portal-include.md)]

## <a name="VNetGateway"></a>5. Vytvoření brány virtuální sítě
V tomto kroku vytvoříte bránu virtuální sítě pro svou virtuální síť. Vytvoření brány může obvykle trvat 45 minut nebo déle, a to v závislosti na vybrané skladové jednotce (SKU) brány. Pokud vytváříte tuto konfiguraci jako cvičení, můžete použít tyto [příklady nastavení](#values).

### <a name="to-create-a-virtual-network-gateway"></a>Chcete-li vytvořit bránu virtuální sítě
[!INCLUDE [vpn-gateway-add-gw-rm-portal](../../includes/vpn-gateway-add-gw-rm-portal-include.md)]

## <a name="CreateTestVNet4"></a>6. Vytvoření a konfigurace virtuální sítě TestVNet4
Po konfiguraci virtuální sítě TestVNet1 vytvořte virtuální síť TestVNet4 opakováním předchozích kroků a nahrazením hodnot za hodnoty virtuální sítě TestVNet4. Není nutné s konfigurací virtuální sítě TestVNet4 čekat na dokončení vytváření brány virtuální sítě pro TestVNet1. Pokud používáte vlastní hodnoty, zajistěte, aby se adresní prostory nepřekrývaly s žádnou z virtuálních sítí, ke kterým se chcete připojit.

## <a name="TestVNet1Connection"></a>7. Konfigurace připojení virtuální sítě TestVNet1
Po dokončení vytváření bran virtuálních sítí pro TestVNet1 a TestVNet4 můžete vytvořit připojení bran virtuálních sítí. V této části vytvoříte připojení z virtuální sítě TestVNet1 k virtuální síti TestVNet4. Tyto kroky fungují pouze u virtuálních sítí ve stejném předplatném. Pokud jsou vaše virtuální sítě v různých předplatných, musíte k vytvoření propojení použít PowerShell. Podrobnosti najdete v článku o [PowerShellu](vpn-gateway-vnet-vnet-rm-ps.md).

1. V části **Všechny prostředky** přejděte do brány virtuální sítě pro vaši virtuální síť. Například **TestVNet1GW**. Kliknutím na **TestVNet1GW** otevřete okno brány virtuální sítě.
   
    ![Okno Připojení](./media/vpn-gateway-howto-vnet-vnet-resource-manager-portal/settings_connection.png "Okno Připojení")
2. Kliknutím na **+Přidat** otevřete okno **Přidat připojení**.
3. V okně **Přidat připojení** do pole Název zadejte název připojení. Například **TestVNet1toTestVNet4**.
   
    ![Název připojení](./media/vpn-gateway-howto-vnet-vnet-resource-manager-portal/v1tov4.png "Název připojení")
4. Jako **Typ připojení** vyberte z rozevíracího seznamu **VNet-to-VNet**.
5. Pole **První brána virtuální sítě** je vyplněné automaticky, protože toto připojení vytváříte ze zadané brány virtuální sítě.
6. Pole **Druhá brána virtuální sítě** představuje bránu virtuální sítě, ke které chcete vytvořit připojení. Kliknutím na **Vybrat jinou bránu virtuální sítě** otevřete okno **Vybrat bránu virtuální sítě**.
   
    ![Přidání připojení](./media/vpn-gateway-howto-vnet-vnet-resource-manager-portal/add_connection.png "Přidání připojení")
7. Prohlédněte si brány virtuálních sítí uvedené v tomto okně. Všimněte si, že jsou uvedené pouze brány virtuálních sítí v rámci vašeho předplatného. Pokud se chcete připojit k bráně virtuální sítě, která není ve vašem předplatném, použijte k tomu [článek k prostředí PowerShell](vpn-gateway-vnet-vnet-rm-ps.md). 
8. Klikněte na bránu virtuální sítě, ke které se chcete připojit.
9. Do pole **Sdílený klíč** zadejte sdílený klíč pro vaše připojení. Tento klíč si můžete vygenerovat nebo vytvořit sami. V připojení typu Site-to-Site by použitý klíč byl úplně stejný pro místní zařízení i pro připojení brány virtuální sítě. Tady platí podobný přístup, akorát se místo připojování k zařízení VPN připojujete k další bráně virtuální sítě.
   
    ![Sdílený klíč](./media/vpn-gateway-howto-vnet-vnet-resource-manager-portal/sharedkey.png "Sdílený klíč")
10. Uložte změny kliknutím na **OK** v dolní části okna.

## <a name="TestVNet4Connection"></a>8. Konfigurace připojení virtuální sítě TestVNet4
Dále vytvoříte připojení z virtuální sítě TestVNet4 k virtuální síti TestVNet1. Použijte stejnou metodu, kterou jste použili k vytvoření připojení z virtuální sítě TestVNet1 k virtuální síti TestVNet4. Ujistěte se, že používáte stejný sdílený klíč.

## <a name="VerifyConnection"></a>9. Ověření stavu připojení
Ověřte připojení. U každé brány virtuální sítě postupujte následovně:

1. Vyhledejte okno pro příslušnou bránu virtuální sítě. Například **TestVNet4GW**. 
2. V okně brány virtuální sítě klikněte na **Připojení** a zobrazte okno připojení pro bránu virtuální sítě.

Prohlédněte si připojení a ověřte stav. Jakmile je připojení vytvořeno, uvidíte jako hodnoty stavu **Úspěch** a **Připojeno**.

![Úspěšné](./media/vpn-gateway-howto-vnet-vnet-resource-manager-portal/connected.png "Úspěšné")

Na jednotlivá připojení můžete dvakrát kliknout a zobrazit tak další informace o připojení.

![Základy](./media/vpn-gateway-howto-vnet-vnet-resource-manager-portal/essentials.png "Základy")

## <a name="faq"></a>Nejčastější dotazy týkající se propojení VNet-to-VNet
Projděte si Nejčastější dotazy, kde najdete další informace o propojeních VNet-to-VNet.

[!INCLUDE [vpn-gateway-vnet-vnet-faq](../../includes/vpn-gateway-faq-vnet-vnet-include.md)]

## <a name="next-steps"></a>Další kroky
Po dokončení připojení můžete do virtuálních sítí přidávat virtuální počítače. Další informace najdete v [dokumentaci ke službě Virtual Machines](https://docs.microsoft.com/azure/#pivot=services&panel=Compute).
