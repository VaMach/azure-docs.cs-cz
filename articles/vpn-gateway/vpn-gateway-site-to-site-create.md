---
title: "Připojení místní sítě k virtuální síti Azure: Síť VPN typu Site-to-Site: Portál Classic | Dokumentace Microsoftu"
description: "Postup vytvoření připojení IPsec z vaší místní sítě k virtuální síti Azure přes veřejný internet. Tyto kroky vám pomůžou vytvořit připojení VPN Gateway typu Site-to-Site mezi různými místy pomocí portálu Classic a modelu nasazení Classic."
services: vpn-gateway
documentationcenter: 
author: cherylmc
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 024ecb29-64de-4ff1-84f1-1a45a8595f0b
ms.service: vpn-gateway
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/23/2017
ms.author: cherylmc
translationtype: Human Translation
ms.sourcegitcommit: eea00841708212789e14fa8717d83dd81d472bac
ms.openlocfilehash: 835968ec5b540890dbe8644038ab7f63b0721847


---
# <a name="create-a-vnet-with-a-site-to-site-connection-using-the-classic-portal"></a>Vytvoření virtuální sítě s připojením typu Site-to-Site pomocí klasického portálu
> [!div class="op_single_selector"]
> * [Resource Manager – Azure Portal](vpn-gateway-howto-site-to-site-resource-manager-portal.md)
> * [Resource Manager – PowerShell](vpn-gateway-create-site-to-site-rm-powershell.md)
> * [Classic – portál Azure Classic](vpn-gateway-site-to-site-create.md)
> 
> 

Tento článek vás provede procesem vytvoření virtuální sítě a připojení služby VPN Gateway typu Site-to-Site k místní síti pomocí modelu nasazení Classic a portálu Classic. Připojení typu Site-to-Site lze použít pro konfigurace mezi různými místy a pro hybridní konfigurace.

![Diagram připojení VPN Gateway typu Site-to-Site mezi různými místy](./media/vpn-gateway-site-to-site-create/site-to-site-connection-diagram.png)

### <a name="deployment-models-and-methods-for-site-to-site-connections"></a>Modely nasazení a metody připojení typu Site-to-Site
[!INCLUDE [deployment models](../../includes/vpn-gateway-deployment-models-include.md)]

Následující tabulka uvádí aktuálně dostupné modely a metody nasazení v konfiguracích Site-to-Site. Když je článek s postupem konfigurace k dispozici, zařadíme do tabulky přímý odkaz na něj.

[!INCLUDE [vpn-gateway-table-site-to-site-table](../../includes/vpn-gateway-table-site-to-site-include.md)]

#### <a name="additional-configurations"></a>Další konfigurace
Informace o propojení virtuálních sítí najdete v tématu [Konfigurace připojení typu VNet-to-VNet pro model nasazení Classic](virtual-networks-configure-vnet-to-vnet-connection.md). Pokud chcete přidat připojení Site-to-Site k virtuální síti, která už připojení má, získáte informace v části [Přidání připojení S2S k virtuální síti s existujícím připojením brány VPN](vpn-gateway-multi-site.md).

## <a name="before-you-begin"></a>Než začnete
Před zahájením konfigurace ověřte, zda máte následující.

* Kompatibilní zařízení VPN a někoho, kdo jej umí nakonfigurovat. Viz [Informace o zařízeních VPN](vpn-gateway-about-vpn-devices.md). Pokud nevíte, jak nakonfigurovat zařízení VPN, nebo neznáte rozsahy IP adres v konfiguraci vaší místní sítě, budete se muset spojit s někým, kdo vám s tím pomůže.
* Veřejnou IP adresu pro vaše zařízení VPN. Tato IP adresa nesmí být umístěná za překladem adres (NAT).
* Předplatné Azure. Pokud ještě nemáte předplatné Azure, můžete si aktivovat [výhody pro předplatitele MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details) nebo si zaregistrovat [bezplatný účet](https://azure.microsoft.com/pricing/free-trial).

## <a name="a-namecreatevnetacreate-your-virtual-network"></a><a name="CreateVNet"></a>Vytvoření virtuální sítě
1. Přihlaste se do [portálu Azure Classic](https://manage.windowsazure.com/).
2. V levém dolním rohu obrazovky klikněte na **Nový**. V podokně navigace klikněte na **Network Services**, a poté klikněte na **Virtual Network**. Kliknutím na **Vytvořit vlastní** spustíte průvodce konfigurací.
3. Pokud chcete vytvořit virtuální síť, zadejte konfigurační nastavení na následujících stránkách:

## <a name="a-namedetailsavirtual-network-details-page"></a><a name="Details"></a>Stránka Podrobnosti virtuální sítě
Zadejte následující informace:

* **Název**: Zadejte název virtuální sítě. Například *EastUSVNet*. Tento název virtuální sítě budete používat při nasazování virtuálních počítačů a instancí PaaS, proto jej nenastavujte příliš komplikovaný.
* **Umístění**: Umístění přímo souvisí s fyzickým umístěním (oblastí), kde chcete mít umístěné prostředky (virtuální počítače). Například pokud chcete, aby virtuální počítače, které do této virtuální sítě nasadíte, byly fyzicky umístěné ve *východní USA*, vyberte toto umístění. Oblast přidruženou k virtuální síti nebude možné po jejím vytvoření změnit.

## <a name="a-namednsadns-servers-and-vpn-connectivity-page"></a><a name="DNS"></a>Servery DNS a stránka Připojení VPN
Zadejte následující informace, a poté klikněte na šipku Další vpravo dole.

* **Servery DNS**: Zadejte název serveru DNS a IP adresu, nebo vyberte dříve zaregistrovaný server DNS z místní nabídky. Toto nastavení neslouží k vytvoření serveru DNS. Umožňuje určit server DNS, který chcete použít pro překlad názvů pro tuto virtuální síť.
* **Konfigurace sítě Site-to-Site VPN**: Zaškrtněte políčko **Konfigurovat síť Site-to-Site VPN**.
* **Místní síť**: Místní síť představuje váš fyzický místní server. Můžete vybrat místní síť, kterou jste vytvořili dříve, nebo můžete vytvořit novou místní síť. Pokud však vyberete použití místní sítě, kterou jste vytvořili dříve, přejděte na stránku konfigurace **Místní sítě** a ověřte, že IP adresa zařízení VPN (veřejná adresa IPv4) pro příslušné zařízení VPN je správná.

## <a name="a-nameconnectivityasite-to-site-connectivity-page"></a><a name="Connectivity"></a>Stránka Připojení typu Site-to-Site
Pokud vytváříte novou místní síť, zobrazí se stránka **Připojení typu Site-to-Site**. Chcete-li použít místní síť, kterou jste vytvořili dříve, tato stránka se v průvodci nezobrazí, a můžete přejít k dalšímu oddílu.

Zadejte následující informace, a poté klikněte na šipku Další.

* **Název**: Název, který chcete použít pro místní server.
* **IP adresa zařízení VPN**: Veřejná IP adresa IPv4 vašeho místního zařízení VPN, které používáte pro připojení k Azure. Zařízení VPN nesmí být umístěné za překladem adres (NAT).
* **Adresní prostor**: Zahrnuje počáteční IP adresu a CIDR (počet adres). Určete rozsah(y) adres, které chcete posílat přes bránu virtuální sítě na svůj místní server. Pokud cílová IP adresa spadá do rozsahu, který zde určíte, je směrována přes bránu virtuální sítě.
* **Přidání adresního prostoru**: Pokud máte více rozsahů adres, které chcete posílat přes bránu virtuální sítě, určete každý další rozsah adres. Přidat nebo odebrat rozsahy můžete i později na stránce **Místní síť**.

## <a name="a-nameaddressavirtual-network-address-spaces-page"></a><a name="Address"></a>Stránka Adresní prostory virtuální sítě
Zadejte rozsah adres, které chcete použít pro vaši virtuální síť. Toto jsou dynamické IP adresy (vyhrazené IP adresy), které budou přiřazené pro virtuální počítače a další instance rolí, které nasadíte do této virtuální sítě.

Je obzvlášť důležité vybrat rozsah, který se nepřekrývá s žádným z rozsahů, které se používají ve vaší místní síti. Budete se muset domluvit se správcem vaší sítě. Správce vaší sítě možná bude muset vyčlenit z adresního prostoru místní sítě rozsah IP adres, které budete moci použít pro virtuální síť.

Zadejte následující informace, a poté kliknutím na značku zaškrtnutí vpravo dole nakonfigurujete svoji síť.

* **Adresní prostor**: Zahrnuje počáteční IP adresu a počet adres. Ověřte, že se zadané adresní prostory nepřekrývají s adresními prostory ve vaší místní síti.
* **Přidání podsítě**: Zahrnuje počáteční IP adresu a počet adres. Dodatečné podsítě nejsou vyžadovány, ale možná budete chtít vytvořit samostatnou podsíť pro virtuální počítače, které budou mít statické vyhrazené IP adresy. Nebo možná budete chtít mít virtuální počítače v podsíti oddělené od dalších instancí rolí.
* **Přidat podsíť brány**: Kliknutím přidáte podsíť brány. Podsíť brány se používá pouze pro bránu virtuální sítě a pro tuto konfiguraci je vyžadována.

Kliknutím na značku zaškrtnutí v dolní části stránky zahájíte vytváření virtuální sítě. Po dokončení uvidíte stav **Vytvořená** v části **Stav** na stránce **Sítě** na portálu Azure Classic. Po vytvoření virtuální sítě můžete nakonfigurovat vlastní bránu virtuální sítě.

[!INCLUDE [vpn-gateway-no-nsg](../../includes/vpn-gateway-no-nsg-include.md)]

## <a name="a-namevnetgatewayaconfigure-your-virtual-network-gateway"></a><a name="VNetGateway"></a>Konfigurace brány virtuální sítě
Nakonfigurujte bránu virtuální sítě a vytvořte bezpečné připojení typu Site-to-Site. Viz [Konfigurace brány virtuální sítě v portálu Azure Classic](vpn-gateway-configure-vpn-gateway-mp.md).

## <a name="next-steps"></a>Další kroky
 Po dokončení připojení můžete do virtuálních sítí přidávat virtuální počítače. Další informace najdete v tématu [Virtuální počítače](https://docs.microsoft.com/azure/#pivot=services&panel=Compute).




<!--HONumber=Jan17_HO4-->


