---
title: "Připojení místní sítě k virtuální síti Azure: Síť VPN typu Site-to-Site (Classic): Portál| Dokumentace Microsoftu"
description: "Postup vytvoření připojení IPsec z vaší místní sítě k virtuální síti Azure přes veřejný internet. Tyto kroky vám pomůžou vytvořit připojení VPN Gateway typu Site-to-Site mezi různými místy pomocí portálu."
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: vpn-gateway
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/15/2017
ms.author: cherylmc
translationtype: Human Translation
ms.sourcegitcommit: afe143848fae473d08dd33a3df4ab4ed92b731fa
ms.openlocfilehash: 061226c76128122b61d2fe135fdfae0bfd7490d7
ms.lasthandoff: 03/17/2017


---
# <a name="create-a-vnet-with-a-site-to-site-connection-using-the-azure-portal-classic"></a>Vytvoření virtuální sítě s připojením typu Site-to-Site pomocí webu Azure Portal (Classic)
> [!div class="op_single_selector"]
> * [Resource Manager – Azure Portal](vpn-gateway-howto-site-to-site-resource-manager-portal.md)
> * [Resource Manager – PowerShell](vpn-gateway-create-site-to-site-rm-powershell.md)
> * [Classic – Azure Portal](vpn-gateway-howto-site-to-site-classic-portal.md)
> * [Classic – portál Azure Classic](vpn-gateway-site-to-site-create.md)
>
>

Tento článek vás provede procesem vytvoření virtuální sítě a připojení brány VPN typu Site-to-Site k místní síti pomocí modelu nasazení Classic a webu Azure Portal. 

Připojení brány VPN typu Site-to-Site (S2S) je připojení přes tunel VPN prostřednictvím protokolu IPsec/IKE (IKEv1 nebo IKEv2). Tento typ připojení vyžaduje místní zařízení sítě VPN, které má přiřazenou veřejnou IP adresu a není umístěné za službou NAT. Připojení typu Site-to-Site můžete použít pro konfigurace mezi různými místy a hybridní konfigurace nebo k připojení virtuálních sítí v různých modelech nasazení.

![Diagram připojení VPN Gateway typu Site-to-Site mezi různými místy](./media/vpn-gateway-howto-site-to-site-resource-manager-portal/site-to-site-diagram.png)

### <a name="deployment-models-and-methods-for-site-to-site-connections"></a>Modely nasazení a metody připojení typu Site-to-Site
[!INCLUDE [deployment models](../../includes/vpn-gateway-deployment-models-include.md)]

Následující tabulka uvádí aktuálně dostupné modely a metody nasazení v konfiguracích Site-to-Site. Když je článek s postupem konfigurace k dispozici, zařadíme do tabulky přímý odkaz na něj.

[!INCLUDE [site-to-site table](../../includes/vpn-gateway-table-site-to-site-include.md)]

#### <a name="additional-configurations"></a>Další konfigurace
Informace o propojení virtuálních sítí bez vytvoření připojení k místnímu umístění najdete v tématu [Konfigurace připojení typu VNet-to-VNet](virtual-networks-configure-vnet-to-vnet-connection.md). Pokud chcete přidat připojení Site-to-Site k virtuální síti, která už připojení má, získáte informace v části [Přidání připojení S2S k virtuální síti s existujícím připojením brány VPN](vpn-gateway-multi-site.md).

## <a name="before-you-begin"></a>Než začnete
Před zahájením konfigurace ověřte, zda máte následující:

* Kompatibilní zařízení VPN a někoho, kdo jej umí nakonfigurovat. Viz [Informace o zařízeních VPN](vpn-gateway-about-vpn-devices.md). Pokud nevíte, jak nakonfigurovat zařízení VPN, nebo neznáte rozsahy IP adres v konfiguraci vaší místní sítě, budete se muset spojit s někým, kdo vám s tím pomůže.
* Veřejnou IP adresu IPv4 pro vaše zařízení VPN. Tato IP adresa nesmí být umístěná za překladem adres (NAT).
* Předplatné Azure. Pokud ještě nemáte předplatné Azure, můžete si aktivovat [výhody pro předplatitele MSDN](http://azure.microsoft.com/pricing/member-offers/msdn-benefits-details) nebo si zaregistrovat [bezplatný účet](http://azure.microsoft.com/pricing/free-trial).
* V současné době je se k zadání sdíleného klíče a vytvoření připojení brány VPN vyžaduje PowerShell. Nainstalujte nejnovější verzi rutin PowerShellu pro správu služeb Azure. Další informace najdete v tématu [Instalace a konfigurace Azure PowerShellu](/powershell/azureps-cmdlets-docs). Při práci s prostředím PowerShell pro tuto konfiguraci se ujistěte, že jej spouštíte jako správce. 

> [!NOTE]
> Při konfiguraci připojení typu Site-to-Site je pro vaše zařízení VPN vyžadována veřejná IP adresa IPv4.                                                                                                                                                                               
>
>

### <a name="values"></a>Ukázkové hodnoty konfigurace pro toto cvičení
Používáte-li tyto kroky jako cvičení, můžete použít ukázkové hodnoty konfigurace:

* **Název virtuální sítě:** TestVNet1
* **Adresní prostor:** 
    * 10.11.0.0/16
    * 10.12.0.0/16 (volitelné pro toto cvičení)
* **Podsítě:**
  * FrontEnd: 10.11.0.0/24
  * BackEnd: 10.12.0.0/24 (volitelné pro toto cvičení)
* **Podsíť brány:** 10.11.255.0/27
* **Skupina prostředků:** TestRG1
* **Umístění:** Východní USA
* **Server DNS:** 8.8.8.8 (volitelné pro toto cvičení)
* **Název místní lokality:** Site2

## <a name="CreatVNet"></a>1. Vytvoření virtuální sítě

Pokud vytváříte virtuální síť pro použití k připojení typu Site-to-Site, budete se muset ujistit, že se adresní prostory, které zadáváte, nepřekrývají s žádnými klientskými adresními prostory pro místní lokality, ke kterým se chcete připojit. Pokud se podsítě překrývají, připojení nebude fungovat správně.

* Pokud již máte virtuální síť vytvořenou, ověřte, zda jsou nastavení kompatibilní s vaším návrhem brány VPN. Věnujte zvláštní pozornost všem podsítím, které by se mohly překrývat s jinými sítěmi. 

* Pokud ještě nemáte virtuální síť, vytvořte si ji. Snímky obrazovek slouží jen jako příklady. Nezapomeňte hodnoty nahradit vlastními.

### <a name="to-create-a-virtual-network"></a>Chcete-li vytvořit virtuální síť

1. V prohlížeči přejděte na [portál Azure](http://portal.azure.com) a v případě potřeby se přihlaste pomocí účtu Azure.
2. Klikněte na možnost **Nové**. Do pole **Hledat na Marketplace** zadejte text „Virtuální síť“. Ve vráceném seznamu vyhledejte položku **Virtuální síť** a kliknutím otevřete okno **Virtuální síť**.

    ![Okno pro vyhledání virtuální sítě](./media/vpn-gateway-howto-site-to-site-classic-portal/newvnetportal700.png)
3. U dolního okraje okna virtuální sítě v seznamu **Vybrat model nasazení** vyberte **Klasické** a potom klikněte na **Vytvořit**.

    ![Výběr modelu nasazení](./media/vpn-gateway-howto-site-to-site-classic-portal/selectmodel.png)
4. V okně **Vytvoření virtuální sítě** nakonfigurujte nastavení sítě VNet. V tomto okně přidáte první adresní prostor a jeden rozsah adres podsítě. Po dokončení vytváření sítě VNet můžete přejít zpět a přidat další podsítě a adresní prostory.

    ![Okno pro vytvoření virtuální sítě](./media/vpn-gateway-howto-site-to-site-classic-portal/createvnet.png "Okno pro vytvoření virtuální sítě")
5. V rozevíracím seznamu **Předplatné** zkontrolujte, jestli je vybrané správné předplatné. Předplatná můžete měnit prostřednictvím rozevíracího seznamu.
6. Klikněte na **Skupina prostředků** a vyberte existující skupinu nebo zadáním názvu nové skupiny prostředků vytvořte novou skupinu prostředků. Další informace o skupinách prostředků najdete v článku [Přehled Azure Resource Manageru](../azure-resource-manager/resource-group-overview.md#resource-groups).
7. Potom vyberte nastavení **Umístění** sítě VNet. Umístění určuje, kde budou uloženy prostředky nasazené v této síti VNet.
8. Pokud chcete mít k síti VNet snadný přístup na řídicím panelu, vyberte možnost **Připnout na řídicí panel** a potom klikněte na **Vytvořit**.

    ![Připnout na řídicí panel](./media/vpn-gateway-howto-site-to-site-classic-portal/pintodashboard150.png "Připnout na řídicí panel")
9. Po kliknutí na Vytvořit se na řídicím panelu zobrazí dlaždice s informacemi o průběhu vytváření virtuální sítě. Obsah dlaždice se v průběhu vytváření sítě VNet mění.

    ![Dlaždice Vytváří se virtuální síť](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/deploying150.png "Vytváří se virtuální síť")

Po vytvoření virtuální sítě uvidíte na portálu Azure Classic na stránce sítí v části **Stav** stav **Vytvořená**.

## <a name="additionaladdress"></a>2. Přidání dalšího adresního prostoru

Po vytvoření virtuální sítě můžete přidat další adresní prostor. Přidání dalšího adresního prostoru se nevyžaduje jako součást konfigurace Site-to-Site, pokud ale potřebujete více adresních prostorů, postupujte následovně:

1. Vyhledejte virtuální sítě na portálu.
2. V okně pro virtuální síť v části **Nastavení** klikněte na **Adresní prostor**.
3. V okně Adresní prostor klikněte na **+Přidat** a zadejte další adresní prostor.
 
## <a name="dns"></a>3. Určení serveru DNS
Nastavení DNS se nevyžaduje jako součást konfigurace Site-to-Site, ale pokud chcete překlad IP adres, server DNS je nezbytný.

Po vytvoření virtuální sítě můžete přidat IP adresu serveru DNS, aby bylo možné zpracovávat překlad názvů. Otevřete nastavení pro virtuální síť, klikněte na servery DNS a přidejte IP adresu serveru DNS, který chcete použít pro překlad IP adres. Toto nastavení neslouží k vytvoření serveru DNS. V tomto příkladu nastavení použijeme veřejný server DNS. Obvykle je vhodné použít privátní server DNS. Je nutné přidat server DNS, s nímž můžou komunikovat vaše prostředky.

1. Vyhledejte virtuální sítě na portálu.
2. V okně pro virtuální síť v části **Nastavení** klikněte na **Servery DNS**.
3. Přidejte server DNS.
4. Uložte nastavení kliknutím na **Uložit** v horní části stránky.
 
## <a name="localsite"></a>4. Konfigurace místní lokality

Místní lokalita obvykle odkazuje na vaše místní umístění. Obsahuje IP adresu zařízení VPN, ke kterému vytvoříte připojení, a rozsah IP adres, které budou do tohoto zařízení VPN směrovány přes bránu VPN.

1. Na portálu přejděte na virtuální síť, pro kterou chcete vytvořit bránu.
2. V okně pro virtuální síť klikněte v okně **Přehled** v části pro připojení VPN na položku **Brána**. Otevře se okno **Nové připojení VPN**.

    ![Kliknutím nakonfigurujte nastavení brány](./media/vpn-gateway-howto-site-to-site-classic-portal/beforegw125.png "Kliknutím nakonfigurujte nastavení brány")
3. V okně **Nové připojení VPN** vyberte **Site-to-Site**.

    ![Klikněte na Site-to-Site](./media/vpn-gateway-howto-site-to-site-classic-portal/site-to-site.png "Site-to-Site")
4. Kliknutím na **Místní lokalita – Konfigurovat požadované nastavení** otevřete okno **Místní lokalita**. Nakonfigurujte nastavení a potom kliknutím na **OK** nastavení uložte.
    - **Název:** Vytvořte název pro místní lokalitu, abyste ji mohli snadno identifikovat.
    - **IP adresa brány VPN:** Toto je veřejná IP adresa zařízení VPN pro vaši místní síť. Zařízení VPN vyžaduje veřejnou IP adresu IPv4. Zadejte platnou veřejnou IP adresu pro zařízení VPN, ke kterému se chcete připojit. IP adresa nemůže být za serverem NAT a musí být dostupná pro Azure.
    - **Klientský adresní prostor:** Vypište rozsahy IP adres, které chcete přes tuto bránu směrovat do místní sítě. Můžete přidat více různých rozsahů adres. Ujistěte se, že se zde zadané rozsahy nepřekrývají s rozsahy jiných sítí, ke kterým se vaše virtuální síť připojuje, nebo s rozsahy adres samotné virtuální sítě.

    ![Místní lokalita](./media/vpn-gateway-howto-site-to-site-classic-portal/localnetworksite.png "Konfigurace místní lokality")

## <a name="gatewaysubnet"></a>5. Konfigurace podsítě brány

Pro bránu VPN je nutné vytvořit podsíť brány. Podsíť brány obsahuje IP adresy, které bude používat služba brány VPN.

1. V okně **Nové připojení VPN** zaškrtněte políčko **Vytvořit bránu hned**. Zobrazí se okno Volitelná konfigurace brány. Pokud políčko nezaškrtnete, okno, ve kterém můžete konfigurovat podsíť brány, se nezobrazí.

    ![Konfigurace brány – Podsíť, velikost, typ směrování](./media/vpn-gateway-howto-site-to-site-classic-portal/optional.png "Konfigurace brány – Podsíť, velikost, typ směrování")
2. Kliknutím na **Volitelná konfigurace brány – Podsíť, velikost a typ směrování** otevřete okno **Konfigurace brány**.
3. V okně **Konfigurace brány** kliknutím na **Podsíť – Konfigurovat požadované nastavení** otevřete okno **Přidat podsíť**.

    ![Konfigurace brány – podsíť brány](./media/vpn-gateway-howto-site-to-site-classic-portal/subnetrequired.png "Konfigurace brány – podsíť brány")
4. V okně **Přidat podsíť** přidejte podsíť brány. Pokud je to možné, při přidávání podsítě brány je nejlepší vytvořit podsíť brány s použitím bloku CIDR /28 nebo /27. Tím se zajistí, že bude k dispozici dostatek IP adres pro plnění dalších požadavků na konfiguraci v budoucnu.  Kliknutím na **OK** uložte nastavení.

    ![Přidání podsítě brány](./media/vpn-gateway-howto-site-to-site-classic-portal/addgwsubnet.png "Přidání podsítě brány")

## <a name="sku"></a>6. Zadání SKU a typu sítě VPN
1. Vyberte **velikost** brány. Jde o hodnotu SKU, se kterou vytvoříte bránu virtuální sítě. Na portálu je výchozí SKU **Basic**. Další informace o SKU brány najdete v tématu [Informace o nastavení VPN Gateway](vpn-gateway-about-vpn-gateway-settings.md#gwsku).

    ![Výběr SKU a typu sítě VPN](./media/vpn-gateway-howto-site-to-site-classic-portal/sku.png "Výběr SKU a typu sítě VPN")
2. Vyberte pro bránu **typ směrování**. Označuje se také jako typ sítě VPN. Je důležité vybrat správný typ brány, protože typ brány nelze měnit. Zařízení VPN musí být kompatibilní s typem směrování, který zvolíte. Další informace o typu sítě VPN najdete v tématu [Informace o nastavení služby VPN Gateway](vpn-gateway-about-vpn-gateway-settings.md#vpntype). Můžete narazit na články, které odkazují na typy sítě VPN RouteBased a PolicyBased. Typ Dynamická odpovídá RouteBased a Statická odpovídá PolicyBased.
3. Kliknutím na **OK** uložte nastavení.
4. V dolní části okna **Nové připojení VPN** klikněte na **OK** – tím začnete bránu virtuální sítě vytvářet. Dokončení operace může trvat až 45 minut.

## <a name="vpndevice"></a>7. Konfigurace zařízení VPN

Pokud potřebujete znát konkrétní informace o konfiguraci a pomoct s konfigurací vašeho zařízení, obraťte se na výrobce zařízení. V článku [Zařízení VPN](vpn-gateway-about-vpn-devices.md) najdete další informace o zařízeních VPN, která dobře fungují s Azure. Zkontrolujte také [Známé problémy s kompatibilitou zařízení](vpn-gateway-about-vpn-devices.md#known) pro zařízení VPN, které chcete použít. 

Při konfiguraci zařízení VPN budete potřebovat IP adresu brány VPN, kterou jste vytvořili. Najdete ji tak, že přejdete do okna **Přehled** pro vaši virtuální síť.

## <a name="CreateConnection"></a>8. Vytvoření připojení
V tomto kroku nastavíte sdílený klíč a vytvoříte připojení. Klíč, který nastavíte, musí být stejný jako klíč, který jste použili v konfiguraci zařízení VPN.

> [!NOTE]
> Tento krok v současné době není k dispozici na webu Azure Portal. Musíte použít verzi rutin Azure PowerShellu pro správu služeb.                                                                                                                                                                             
>
>

### <a name="step-1-connect-to-your-azure-account"></a>Krok 1. Připojení k účtu Azure

1. Otevřete konzolu PowerShellu se zvýšenými oprávněními a připojte se ke svému účtu. Připojení vám usnadní následující ukázka:

        Login-AzureRmAccount
2. Zkontrolujte předplatná pro příslušný účet.

        Get-AzureRmSubscription
3. Máte-li více předplatných, vyberte předplatné, které chcete použít.

        Select-AzureRmSubscription -SubscriptionName "Replace_with_your_subscription_name"
4. Přidejte verzi rutin PowerShellu pro správu služeb.

        Add-AzureAccount

### <a name="step-2-set-the-shared-key-and-create-the-connection"></a>Krok 2. Nastavení sdíleného klíče a vytvoření připojení

Při práci s PowerShellem a modelem nasazení Classic někdy názvy prostředků na portálu neodpovídají názvům, které Azure při použití PowerShellu očekává. Následující kroky vám pomůžou exportovat soubor konfigurace sítě a získat tak přesné hodnoty pro tyto názvy.

1. Vytvořte ve svém počítači adresář a potom do něj exportujte soubor konfigurace sítě. V tomto příkladu se soubor konfigurace sítě exportuje do adresáře C:\AzureNet.

         Get-AzureVNetConfig -ExportToFile C:\AzureNet\NetworkConfig.xml
2. Otevřete soubor konfigurace sítě v editoru XML a zkontrolujte hodnoty „LocalNetworkSite name“ (Název místní síťové lokality) a „VirtualNetworkSite name“ (Název virtuální síťové lokality). Upravte příklad tak, aby odpovídal těmto hodnotám. Pokud zadáváte název, který obsahuje mezery, zadejte hodnotu v jednoduchých uvozovkách.

3. Nastavte sdílený klíč a vytvořte připojení. Hodnota, kterou generujete a zadáváte, je „-SharedKey“. V příkladu jsme použili „abc123“, ale můžete (a měli byste) vygenerovat něco složitějšího. Důležité je, aby hodnota, kterou zde zadáte, byla stejná jako hodnota, kterou jste zadali při konfiguraci zařízení VPN.

        Set-AzureVNetGatewayKey -VNetName 'Group TestRG1 TestVNet1' `
        -LocalNetworkSiteName 'D1BFC9CB_Site2' -SharedKey abc123
Jakmile se připojení vytvoří, výsledkem bude: **Stav: Úspěch**.

## <a name="verify"></a>9. Ověření stavu připojení

[!INCLUDE [vpn-gateway-verify-connection-azureportal-classic](../../includes/vpn-gateway-verify-connection-azureportal-classic-include.md)]

## <a name="next-steps"></a>Další kroky
Po dokončení připojení můžete do virtuálních sítí přidávat virtuální počítače. Další informace najdete v tématu [Virtuální počítače](https://docs.microsoft.com/azure/#pivot=services&panel=Compute).


