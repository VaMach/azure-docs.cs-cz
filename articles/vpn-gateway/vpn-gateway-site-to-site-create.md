---
title: Vytvoření virtuální sítě s připojením typu Site-to-Site ke službě VPN Gateway pomocí portálu Azure Classic | Microsoft Docs
description: Vytvoření virtuální sítě s připojením typu Site-to-Site ke službě VPN Gateway pro konfigurace mezi různými místy a pro hybridní konfigurace pomocí modelu nasazení Classic.
services: vpn-gateway
documentationcenter: ''
author: cherylmc
manager: carmonm
editor: ''
tags: azure-service-management

ms.service: vpn-gateway
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/31/2016
ms.author: cherylmc

---
# Vytvoření virtuální sítě s připojením typu Site-to-Site pomocí portálu Azure Classic
> [!div class="op_single_selector"]
> * [Resource Manager – Azure Portal](vpn-gateway-howto-site-to-site-resource-manager-portal.md)
> * [Resource Manager – PowerShell](vpn-gateway-create-site-to-site-rm-powershell.md)
> * [Classic – klasický portál](vpn-gateway-site-to-site-create.md)
> 
> 

Tento článek vás provede procesem vytvoření virtuální sítě a připojení VPN typu Site-to-Site k místní síti pomocí **modelu nasazení Classic** a portálu Classic. Připojení typu Site-to-Site lze použít pro konfigurace mezi různými místy a pro hybridní konfigurace. V současné době nelze vytvořit end-to-end konfiguraci Site-to-Site pro model klasického nasazení pomocí webu Azure Portal.

![Diagram Site-to-site](./media/vpn-gateway-site-to-site-create/site2site.png "site-to-site")

### Modely nasazení a nástroje pro připojení typu Site-to-Site
[!INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)]

[!INCLUDE [vpn-gateway-table-site-to-site](../../includes/vpn-gateway-table-site-to-site-include.md)]

Informace o propojení virtuálních sítí najdete v tématu [Konfigurace připojení typu VNet-to-VNet pro model nasazení Classic](virtual-networks-configure-vnet-to-vnet-connection.md). 

## Než začnete
Před zahájením konfigurace ověřte, zda máte následující.

* Kompatibilní zařízení VPN a někoho, kdo jej umí nakonfigurovat. Viz [Informace o zařízeních VPN](vpn-gateway-about-vpn-devices.md). Pokud nevíte, jak nakonfigurovat zařízení VPN, nebo neznáte rozsahy IP adres v konfiguraci vaší místní sítě, budete se muset spojit s někým, kdo vám s tím pomůže.
* Veřejnou IP adresu pro vaše zařízení VPN. Tato IP adresa nesmí být umístěná za překladem adres (NAT).
* Předplatné Azure. Pokud ještě nemáte předplatné Azure, můžete si aktivovat [výhody pro předplatitele MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) nebo si zaregistrovat [bezplatný účet](https://azure.microsoft.com/pricing/free-trial/).

## Vytvoření virtuální sítě
1. Přihlaste se do [portálu Azure Classic](https://manage.windowsazure.com/).
2. V levém dolním rohu obrazovky klikněte na **Nový**. V podokně navigace klikněte na **Network Services**, a poté klikněte na **Virtual Network**. Kliknutím na **Vytvořit vlastní** spustíte průvodce konfigurací.
3. Pokud chcete vytvořit virtuální síť, zadejte konfigurační nastavení na následujících stránkách:

## Stránka Podrobnosti virtuální sítě
Zadejte následující informace:

* **Název**: Zadejte název virtuální sítě. Například *EastUSVNet*. Tento název virtuální sítě budete používat při nasazování virtuálních počítačů a instancí PaaS, proto jej nenastavujte příliš komplikovaný.
* **Umístění**: Umístění přímo souvisí s fyzickým umístěním (oblastí), kde chcete mít umístěné prostředky (virtuální počítače). Například pokud chcete, aby virtuální počítače, které do této virtuální sítě nasadíte, byly fyzicky umístěné ve *východní USA*, vyberte toto umístění. Oblast přidruženou k virtuální síti nebude možné po jejím vytvoření změnit.

## Servery DNS a stránka Připojení VPN
Zadejte následující informace, a poté klikněte na šipku Další vpravo dole.

* **Servery DNS**: Zadejte název serveru DNS a IP adresu, nebo vyberte dříve zaregistrovaný server DNS z místní nabídky. Toto nastavení neslouží k vytvoření serveru DNS. Umožňuje určit server DNS, který chcete použít pro překlad názvů pro tuto virtuální síť.
* **Konfigurace sítě Site-to-Site VPN**: Zaškrtněte políčko **Konfigurovat síť Site-to-Site VPN**.
* **Místní síť**: Místní síť představuje váš fyzický místní server. Můžete vybrat místní síť, kterou jste vytvořili dříve, nebo můžete vytvořit novou místní síť. Pokud však vyberete použití místní sítě, kterou jste vytvořili dříve, přejděte na stránku konfigurace **Místní sítě** a ověřte, že IP adresa zařízení VPN (veřejná adresa IPv4) pro příslušné zařízení VPN je správná.

## Stránka Připojení typu Site-to-Site
Pokud vytváříte novou místní síť, zobrazí se stránka **Připojení typu Site-to-Site**. Chcete-li použít místní síť, kterou jste vytvořili dříve, tato stránka se v průvodci nezobrazí, a můžete přejít k dalšímu oddílu.

Zadejte následující informace, a poté klikněte na šipku Další.

* **Název**: Název, který chcete použít pro místní server.
* **IP adresa zařízení VPN**: Veřejná IP adresa IPv4 vašeho místního zařízení VPN, které používáte pro připojení k Azure. Zařízení VPN nesmí být umístěné za překladem adres (NAT).
* **Adresní prostor**: Zahrnuje počáteční IP adresu a CIDR (počet adres). Určete rozsah(y) adres, které chcete posílat přes bránu virtuální sítě na svůj místní server. Pokud cílová IP adresa spadá do rozsahu, který zde určíte, je směrována přes bránu virtuální sítě.
* **Přidání adresního prostoru**: Pokud máte více rozsahů adres, které chcete posílat přes bránu virtuální sítě, určete každý další rozsah adres. Přidat nebo odebrat rozsahy můžete i později na stránce **Místní síť**.

## Stránka Adresní prostory virtuální sítě
Zadejte rozsah adres, které chcete použít pro vaši virtuální síť. Toto jsou dynamické IP adresy (vyhrazené IP adresy), které budou přiřazené pro virtuální počítače a další instance rolí, které nasadíte do této virtuální sítě.

Je obzvlášť důležité vybrat rozsah, který se nepřekrývá s žádným z rozsahů, které se používají ve vaší místní síti. Budete se muset domluvit se správcem vaší sítě. Správce vaší sítě možná bude muset vyčlenit z adresního prostoru místní sítě rozsah IP adres, které budete moci použít pro virtuální síť.

Zadejte následující informace, a poté kliknutím na značku zaškrtnutí vpravo dole nakonfigurujete svoji síť.

* **Adresní prostor**: Zahrnuje počáteční IP adresu a počet adres. Ověřte, že se zadané adresní prostory nepřekrývají s adresními prostory ve vaší místní síti.
* **Přidání podsítě**: Zahrnuje počáteční IP adresu a počet adres. Dodatečné podsítě nejsou vyžadovány, ale možná budete chtít vytvořit samostatnou podsíť pro virtuální počítače, které budou mít statické vyhrazené IP adresy. Nebo možná budete chtít mít virtuální počítače v podsíti oddělené od dalších instancí rolí.
* **Přidat podsíť brány**: Kliknutím přidáte podsíť brány. Podsíť brány se používá pouze pro bránu virtuální sítě a pro tuto konfiguraci je vyžadována.

Kliknutím na značku zaškrtnutí v dolní části stránky zahájíte vytváření virtuální sítě. Po dokončení uvidíte stav **Vytvořená** v části **Stav** na stránce **Sítě** na portálu Azure Classic. Po vytvoření virtuální sítě můžete nakonfigurovat vlastní bránu virtuální sítě.

[!INCLUDE [vpn-gateway-no-nsg](../../includes/vpn-gateway-no-nsg-include.md)]

## Konfigurace brány virtuální sítě
Nakonfigurujte bránu virtuální sítě a vytvořte bezpečné připojení typu Site-to-Site. Viz [Konfigurace brány virtuální sítě v portálu Azure Classic](vpn-gateway-configure-vpn-gateway-mp.md).

## Další kroky
Po dokončení připojení můžete do virtuálních sítí přidávat virtuální počítače. Další informace najdete v dokumentaci ke službě [Virtual Machines](https://azure.microsoft.com/documentation/services/virtual-machines/).

<!--HONumber=Sep16_HO3-->


