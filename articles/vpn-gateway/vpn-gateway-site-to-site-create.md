<properties
   pageTitle="Vytvoření virtuální sítě s připojením typu Site-to-Site ke službě VPN Gateway pomocí portálu Azure Classic | Microsoft Azure"
   description="Vytvoření virtuální sítě s připojením typu Site-to-Site ke službě VPN Gateway pro konfigurace mezi různými místy a pro hybridní konfigurace pomocí modelu nasazení Classic."
   services="vpn-gateway"
   documentationCenter=""
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-service-management"/>

<tags
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/09/2016"
   ms.author="cherylmc"/>

# Vytvoření virtuální sítě s připojením VPN typu Site-to-Site pomocí portálu Azure Classic

> [AZURE.SELECTOR]
- [Azure Portal](vpn-gateway-howto-site-to-site-resource-manager-portal.md)
- [Portál Azure Classic](vpn-gateway-site-to-site-create.md)
- [PowerShell – Resource Manager](vpn-gateway-create-site-to-site-rm-powershell.md)


Tento článek vás provede procesem vytvoření virtuální sítě a připojení VPN typu Site-to-Site k místní síti. Připojení typu Site-to-Site lze použít pro konfigurace mezi různými místy a pro hybridní konfigurace. Tento článek se týká modelu nasazení Classic a používá portál Azure Classic. V současné době nelze vytvořit end-to-end konfiguraci Site-to-Site pro model klasického nasazení pomocí portálu Azure.


**O modelech nasazení Azure**

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)] 

## Diagram připojení
 
![Diagram Site-to-site](./media/vpn-gateway-site-to-site-create/site2site.png "site-to-site")

**Modely nasazení a nástroje pro připojení typu Site-to-Site**

[AZURE.INCLUDE [vpn-gateway-table-site-to-site](../../includes/vpn-gateway-table-site-to-site-include.md)]

Informace o propojení virtuálních sítí bez vytvoření připojení k místnímu umístění najdete v tématu [Konfigurace připojení typu VNet-to-VNet pro model nasazení Classic](virtual-networks-configure-vnet-to-vnet-connection.md). Další typy nastavení připojení najdete v článku [Topologie připojení ke službě VPN Gateway](vpn-gateway-topology.md).

 
## Než začnete

Před zahájením konfigurace ověřte, zda máte následující.

- Kompatibilní zařízení VPN a někoho, kdo jej umí nakonfigurovat. Viz [Informace o zařízeních VPN](vpn-gateway-about-vpn-devices.md). Pokud nevíte jak nakonfigurovat zařízení VPN, nebo neznáte rozsahy IP adres v konfiguraci vaší místní sítě, budete se muset spojit s někým, kdo vám s tím pomůže.

-  Veřejnou IP adresu pro vaše zařízení VPN. Tato IP adresa nesmí být umístěná za překladem adres (NAT).

- Předplatné Azure. Pokud ještě nemáte předplatné Azure, můžete si aktivovat [výhody pro předplatitele MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) nebo si zaregistrovat [bezplatný účet](https://azure.microsoft.com/pricing/free-trial/).


## Vytvoření virtuální sítě

1. Přihlaste se do [portálu Azure Classic](https://manage.windowsazure.com/).

2. V levém dolním rohu obrazovky klikněte na **Nový**. V podokně navigace klikněte na **Network Services**, a poté klikněte na **Virtual Network**. Kliknutím na **Vytvořit vlastní** spustíte průvodce konfigurací.

3. Na následujících stránkách vyplňte informace a vytvořte tak virtuální síť.

## Stránka Podrobnosti virtuální sítě

Zadejte následující informace.

- **Název**: Zadejte název virtuální sítě. Například *EastUSVNet*. Tento název virtuální sítě budete používat při nasazování virtuálních počítačů a instancí PaaS, proto jej nenastavujte příliš komplikovaný.
- **Umístění**: Umístění přímo souvisí s fyzickým umístěním (oblastí), kde chcete mít umístěné prostředky (virtuální počítače). Například pokud chcete, aby virtuální počítače, které do této virtuální sítě nasadíte, byly fyzicky umístěné ve *východní USA*, vyberte toto umístění. Oblast přidruženou k virtuální síti nebude možné po jejím vytvoření změnit.

## Servery DNS a stránka Připojení VPN

Zadejte následující informace, a poté klikněte na šipku Další vpravo dole.

- **Servery DNS**: Zadejte název serveru DNS a IP adresu, nebo vyberte dříve zaregistrovaný server DNS z místní nabídky. Toto nastavení neslouží k vytvoření serveru DNS. Umožňuje určit server DNS, který chcete použít pro překlad názvů pro tuto virtuální síť.
- **Konfigurace sítě Site-to-Site VPN**: Zaškrtněte políčko **Konfigurovat síť Site-to-Site VPN**.
- **Místní síť**: Místní síť představuje váš fyzický místní server. Můžete vybrat místní síť, kterou jste vytvořili dříve, nebo můžete vytvořit novou místní síť. Nicméně pokud vyberete použití místní sítě, kterou jste vytvořili dříve, budete chtít přejít na stránku konfigurace **Místní sítě** a ujistit se, že IP adresa zařízení VPN (veřejná adresa IPv4) pro zařízení VPN, které používáte pro toto připojení, je správná.

## Stránka Připojení typu Site-to-Site

Pokud vytváříte novou místní síť, zobrazí se stránka **Připojení typu Site-to-Site**. Chcete-li použít místní síť, kterou jste vytvořili dříve, tato stránka se v průvodci nezobrazí, a můžete přejít k dalšímu oddílu.

Zadejte následující informace, a poté klikněte na šipku Další.

-   **Název**: Název, který chcete použít pro místní server.
-   **IP adresa zařízení VPN**: Toto je veřejná IP adresa IPv4 vašeho místního zařízení VPN, které budete používat pro připojení k Azure. Zařízení VPN nesmí být umístěné za překladem adres (NAT).
-   **Adresní prostor**: Zahrnuje počáteční IP adresu a CIDR (počet adres). Zde určíte rozsah(y) adres, které chcete posílat přes bránu virtuální sítě na svůj místní server. Pokud cílová IP adresa spadá do rozsahu, který zde určíte, je směrována přes bránu virtuální sítě.
-   **Přidání adresního prostoru**: Pokud máte více rozsahů adres, které chcete posílat přes bránu virtuální sítě, zde určíte každý další rozsah adres. Přidat nebo odebrat rozsahy můžete i později na stránce **Místní síť**.

## Stránka Adresní prostory virtuální sítě

Zadejte rozsah adres, které chcete použít pro vaši virtuální síť. Toto jsou dynamické IP adresy (vyhrazené IP adresy), které budou přiřazené pro virtuální počítače a další instance rolí, které nasadíte do této virtuální sítě.

Je obzvlášť důležité vybrat rozsah, který se nepřekrývá s žádným z rozsahů, které se používají ve vaší místní síti. Budete se muset domluvit se správcem vaší sítě. Správce vaší sítě možná bude muset vyčlenit z adresního prostoru místní sítě rozsah IP adres, které budete moci použít pro virtuální síť.

Zadejte následující informace, a poté kliknutím na značku zaškrtnutí vpravo dole nakonfigurujete svoji síť.

- **Adresní prostor**: Zahrnuje počáteční IP adresu a počet adres. Ověřte, že se zadané adresní prostory nepřekrývají s adresními prostory ve vaší místní síti.
- **Přidání podsítě**: Zahrnuje počáteční IP adresu a počet adres. Dodatečné podsítě nejsou vyžadovány, ale možná budete chtít vytvořit samostatnou podsíť pro virtuální počítače, které budou mít statické vyhrazené IP adresy. Nebo možná budete chtít mít virtuální počítače v podsíti oddělené od dalších instancí rolí.
- **Přidat podsíť brány**: Kliknutím přidáte podsíť brány. Podsíť brány se používá pouze pro bránu virtuální sítě a pro tuto konfiguraci je vyžadována.

Kliknutím na značku zaškrtnutí v dolní části stránky zahájíte vytváření virtuální sítě. Po dokončení uvidíte stav **Vytvořená** v části **Stav** na stránce **Sítě** na portálu Azure Classic. Po vytvoření virtuální sítě můžete nakonfigurovat vlastní bránu virtuální sítě.

[AZURE.INCLUDE [vpn-gateway-no-nsg](../../includes/vpn-gateway-no-nsg-include.md)] 

## Konfigurace brány virtuální sítě

Dále nakonfigurujete bránu virtuální sítě a tak vytvoříte bezpečné připojení typu Site-to-Site. Viz [Konfigurace brány virtuální sítě v portálu Azure Classic](vpn-gateway-configure-vpn-gateway-mp.md).

## Další kroky

Po dokončení připojení můžete do virtuálních sítí přidávat virtuální počítače. Další informace najdete v dokumentaci ke službě [Virtual Machines](https://azure.microsoft.com/documentation/services/virtual-machines/).



<!--HONumber=Aug16_HO4-->


