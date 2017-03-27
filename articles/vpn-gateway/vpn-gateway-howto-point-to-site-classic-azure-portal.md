---
title: "Připojení počítače k virtuální síti Azure typu Point-to-Site: Azure Portal: Classic | Dokumentace Microsoftu"
description: "Připojte se bezpečně k své klasické síti Azure Virtual Network vytvořením připojení brány VPN typu Point-to-Site přes Azure Portal."
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 65e14579-86cf-4d29-a6ac-547ccbd743bd
ms.service: vpn-gateway
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/20/2017
ms.author: cherylmc
translationtype: Human Translation
ms.sourcegitcommit: 0d8472cb3b0d891d2b184621d62830d1ccd5e2e7
ms.openlocfilehash: 92e573d7f3ebfbe41c8012068a8262d6fc324da8
ms.lasthandoff: 03/21/2017


---
# <a name="configure-a-point-to-site-connection-to-a-vnet-using-the-azure-portal-classic"></a>Konfigurace připojení typu Point-to-Site k virtuální síti pomocí webu Azure Portal (Classic)
> [!div class="op_single_selector"]
> * [Resource Manager – Azure Portal](vpn-gateway-howto-point-to-site-resource-manager-portal.md)
> * [Resource Manager – PowerShell](vpn-gateway-howto-point-to-site-rm-ps.md)
> * [Classic – Azure Portal](vpn-gateway-howto-point-to-site-classic-azure-portal.md)
>
>

Konfigurace Point-to-Site (P2S) umožňuje vytvořit zabezpečené připojení jednotlivých klientských počítačů k virtuální síti. P2S je připojení VPN prostřednictvím protokolu SSTP (Secure Socket Tunneling Protocol). Připojení Point-to-Site jsou užitečná, když se chcete ke své virtuální síti připojit ze vzdáleného umístění, například z domova nebo z místa konání konference, nebo když máte jen několik klientů, kteří se potřebují připojovat k virtuální síti. Připojení typu P2S nevyžadují zařízení VPN ani veřejnou IP adresu. Připojení VPN se navazuje ze strany klientského počítače.

Tento článek vás provede vytvořením virtuální sítě s připojením Point-to-Site v modelu nasazení Classic s použitím portálu Azure Portal. Další informace o připojení Point-to-Site najdete v části [Nejčastější dotazy týkající se připojení Point-to-Site](#faq) na konci tohoto článku.


### <a name="deployment-models-and-methods-for-p2s-connections"></a>Modely a metody nasazení pro připojení P2S
[!INCLUDE [deployment models](../../includes/vpn-gateway-deployment-models-include.md)]

Následující tabulka uvádí dva modely nasazení a dostupné metody nasazení pro konfigurace P2S. Když je článek s postupem konfigurace k dispozici, zařadíme do tabulky přímý odkaz na něj.

[!INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-table-point-to-site-include.md)]

## <a name="basic-workflow"></a>Základní pracovní postup
![Diagram Point-to-Site](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/point-to-site-connection-diagram.png)

Následující oddíly vás provedou postupem vytvoření bezpečného připojení typu Point-to-Site k virtuální síti.

1. Vytvoření virtuální sítě a brány VPN
2. Generování certifikátů
3. Nahrání souboru .cer
4. Generování konfiguračního balíčku klienta VPN
5. Konfigurace klientského počítače
6. Připojení k Azure

### <a name="example-settings"></a>Příklad nastavení
Můžete použít následující příklad nastavení:

* **Název: VNet1**
* **Adresní prostor: 192.168.0.0/16**<br>V tomto příkladu používáme pouze jeden adresní prostor. Pro svoji virtuální síť můžete mít více adresních prostorů.
* **Název podsítě: FrontEnd**
* **Rozsah adres podsítě: 192.168.1.0/24**
* **Předplatné:** Ujistěte se, že máte správné předplatné, pokud máte více než jedno.
* **Skupina prostředků: TestRG**
* **Umístění: Východní USA**
* **Typ připojení: Point-to-Site**
* **Klientský adresní prostor: 172.16.201.0/24**. Klienti VPN, kteří se budou k síti VNet připojovat pomocí tohoto připojení Point-to-Site, dostanou IP adresu ze zadaného fondu.
* **Podsíť brány: 192.168.200.0/24**. Podsíť brány musí používat název GatewaySubnet.
* **Velikost:** Vyberte SKU brány, kterou chcete použít.
* **Typ směrování: Dynamické**



## <a name="vnetvpn"></a>Oddíl 1 – Vytvoření virtuální sítě a brány VPN

Než začnete, ověřte, že máte předplatné Azure. Pokud ještě nemáte předplatné Azure, můžete si aktivovat [výhody pro předplatitele MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details) nebo si zaregistrovat [bezplatný účet](https://azure.microsoft.com/pricing/free-trial).
### <a name="createvnet"></a>Část 1: Vytvoření virtuální sítě
Pokud ještě nemáte virtuální síť, vytvořte si ji. Snímky obrazovek slouží jen jako příklady. Nezapomeňte hodnoty nahradit vlastními. Pokud chcete vytvořit virtuální síť přes Azure Portal, použijte následující postup:

1. V prohlížeči přejděte na [portál Azure](http://portal.azure.com) a v případě potřeby se přihlaste pomocí účtu Azure.
2. Klikněte na možnost **Nové**. Do pole **Hledat na Marketplace** zadejte text „Virtuální síť“. Ve vráceném seznamu vyhledejte položku **Virtuální síť** a kliknutím otevřete okno **Virtuální síť**.

    ![Okno pro vyhledání virtuální sítě](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/newvnetportal700.png)
3. U dolního okraje okna virtuální sítě v seznamu **Vybrat model nasazení** vyberte **Klasické** a potom klikněte na **Vytvořit**.

    ![Výběr modelu nasazení](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/selectmodel.png)
4. V okně **Vytvoření virtuální sítě** nakonfigurujte nastavení sítě VNet. V tomto okně přidáte první adresní prostor a jeden rozsah adres podsítě. Po dokončení vytváření sítě VNet můžete přejít zpět a přidat další podsítě a adresní prostory.

    ![Okno Vytvořit virtuální síť](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/vnet125.png)
5. V rozevíracím seznamu **Předplatné** zkontrolujte, jestli je vybrané správné předplatné. Předplatná můžete měnit prostřednictvím rozevíracího seznamu.
6. Klikněte na **Skupina prostředků** a vyberte existující skupinu nebo zadáním názvu nové skupiny prostředků vytvořte novou skupinu prostředků. Pokud vytváříte novou skupinu, nazvěte skupinu prostředků v souladu s hodnotami plánované konfigurace. Další informace o skupinách prostředků najdete v článku [Přehled Azure Resource Manageru](../azure-resource-manager/resource-group-overview.md#resource-groups).
7. Potom vyberte nastavení **Umístění** sítě VNet. Umístění určuje, kde budou uložené prostředky nasazené v této síti VNet.
8. Pokud chcete mít k síti VNet snadný přístup na řídicím panelu, vyberte možnost **Připnout na řídicí panel** a potom klikněte na **Vytvořit**.

    ![Připnutí na řídicí panel](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/pintodashboard150.png)
9. Po kliknutí na Vytvořit se na řídicím panelu zobrazí dlaždice s informacemi o průběhu vytváření sítě VNet. Obsah dlaždice se v průběhu vytváření sítě VNet mění.

    ![dlaždice Vytváří se virtuální síť](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/deploying150.png)
10. Po vytvoření virtuální sítě můžete přidat IP adresu serveru DNS, aby bylo možné zpracovávat překlad názvů. Otevřete nastavení pro virtuální síť, klikněte na servery DNS a přidejte IP adresu serveru DNS, který chcete použít. Toto nastavení nevytvoří nový server DNS. Je nutné přidat server DNS, s nímž můžou komunikovat vaše prostředky.

Po vytvoření virtuální sítě uvidíte na portálu Azure Classic na stránce sítí v části **Stav** stav **Vytvořená**.

### <a name="gateway"></a>Část 2: Vytvoření podsítě brány a brány dynamického směrování
V tomto kroku vytvoříte podsíť brány a bránu dynamického směrování. Na portálu Azure Portal pro model nasazení Classic je možné vytvořit podsíť brány a bránu pomocí stejných konfiguračních oken.

1. Na portálu přejděte na virtuální síť, pro kterou chcete vytvořit bránu.
2. V okně pro virtuální síť klikněte v okně **Přehled** v části pro připojení k síti VPN na položku **Brána**.

    ![Kliknutím vytvoříte podsíť brány](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/beforegw125.png)
3. V okně **Nové připojení VPN** vyberte **Point-to-site**.

    ![Připojení typu Point-to-Site](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/newvpnconnect.png)
4. Do pole **Klientský adresní prostor** přidejte rozsah IP adres. Z tohoto rozsahu dostanou klienti VPN IP adresu při svém připojení. Odstraňte automaticky vyplněný rozsah a přidejte vlastní.

    ![Klientský adresní prostor](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/clientaddress.png)
5. Zaškrtněte políčko **Vytvořit bránu hned**.

    ![Zaškrtávací políčko Vytvořit bránu hned](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/creategwimm.png)
6. Kliknutím na **Volitelná konfigurace brány** otevřete okno **Konfigurace brány**.

    ![Kliknutí na tlačítko Volitelná konfigurace brány](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/optsubnet125.png)
7. Klikněte na **Podsíť – Konfigurovat požadované nastavení** a přidejte požadovanou **podsíť brány**. I když je možné vytvořit podsíť brány s minimální velikostí /29, doporučujeme vytvořit větší podsíť, která pojme více adres, tzn. vybrat velikost aspoň /28 nebo /27. Tím vznikne dostatečný prostor pro adresy, který umožní nastavení případných dalších konfigurací v budoucnu.

   > [!IMPORTANT]
   > Při práci s podsítěmi brány nepřidružujte skupinu zabezpečení sítě (NSG) k podsíti brány. Pokud byste k této podsíti přidružili skupinu zabezpečení sítě, brána sítě VPN by mohla přestat fungovat podle očekávání. Další informace o skupinách zabezpečení sítě najdete v článku [Co je skupina zabezpečení sítě (NSG)](../virtual-network/virtual-networks-nsg.md).
   >
   >

    ![Přidání podsítě brány](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/gwsubnet125.png)
8. Vyberte **velikost** brány. Jde o hodnotu SKU, se kterou vytvoříte bránu virtuální sítě. Na portálu je výchozí SKU **Basic**. Další informace o SKU brány najdete v tématu [Informace o nastavení VPN Gateway](vpn-gateway-about-vpn-gateway-settings.md#gwsku).

    ![Velikost brány](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/gwsize125.png)
9. Vyberte pro bránu **typ směrování**. Konfigurace P2S vyžadují **dynamický** typ směrování. Až dokončíte konfiguraci tohoto okna, klikněte na **OK**.

    ![Konfigurace typu směrování](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/routingtype125.png)
10. V dolní části okna **Nové připojení VPN** klikněte na **OK** – tím začnete bránu virtuální sítě vytvářet. Dokončení operace může trvat až 45 minut.

## <a name="generatecerts"></a>Oddíl 2 – Vytvoření certifikátů
Azure používá certifikáty k ověřování klientů VPN pro sítě VPN Point-to-Site. Po vytvoření kořenového certifikátu vyexportujete data veřejného certifikátu (ne privátní klíč) jako soubor .cer X.509 v kódování Base-64. Pak odešlete data veřejného certifikátu z kořenového certifikátu do Azure.

Každý klientský počítač, který se připojuje k virtuální síti pomocí připojení Point-to-Site, musí mít nainstalovaný klientský certifikát. Klientský certifikát se generuje z kořenového certifikátu a nainstaluje na každý klientský počítač. Pokud není nainstalovaný platný klientský certifikát a klient se pokusí připojit k virtuální síti, ověření se nezdaří.

### <a name="cer"></a>Část 1: Získání veřejného klíče (.cer) pro kořenový certifikát

####<a name="enterprise-certificate"></a>Podnikový certifikát
 
Pokud používáte podnikové řešení, můžete použít stávající řetěz certifikátů. Získejte soubor .cer pro kořenový certifikát, který chcete používat.

####<a name="self-signed-root-certificate"></a>Kořenový certifikát podepsaný svým držitelem

Pokud nepoužíváte podnikové certifikační řešení, musíte vytvořit kořenový certifikát podepsaný svým držitelem. Chcete-li vytvořit certifikát podepsaný svým držitelem, který obsahuje potřebná pole pro ověřování P2S, můžete použít PowerShell. Téma s popisem [vytvoření certifikátu podepsaného svým držitelem pro připojení Point-to-Site pomocí PowerShellu](vpn-gateway-certificates-point-to-site.md) vás provede postupem vytvoření kořenového certifikátu podepsaného svým držitelem.

> [!NOTE]
> Dříve se doporučovalo k vytvoření kořenového certifikátu podepsaného svým držitelem a k vygenerování klientských certifikátů pro připojení Point-to-Site používat makecert. Teď můžete k vytvoření těchto certifikátů použít PowerShell. Jednou z výhod použití PowerShellu je schopnost vytvářet certifikáty SHA-2. Požadované hodnoty najdete v tématu s popisem [vytvoření certifikátu podepsaného svým držitelem pro připojení Point-to-Site pomocí PowerShellu](vpn-gateway-certificates-point-to-site.md).
>
>

#### <a name="to-export-the-public-key-for-a-self-signed-root-certificate"></a>Export veřejného klíče pro kořenový certifikát podepsaný svým držitelem

Připojení Point-to-Site vyžadují, aby byl veřejný klíč (.cer) odeslán do Azure. Následující postup vám pomůže vyexportovat soubor .cer pro váš kořenový certifikát podepsaný svým držitelem.

1. Chcete-li získat soubor .cer z certifikátu, otevřete **certmgr.msc**. Vyhledejte kořenový certifikát podepsaný svým držitelem, obvykle v Certificates - Current User\Personal\Certificates, a klikněte pravým tlačítkem myši. Klikněte na **Všechny úlohy** a potom klikněte na **Exportovat**. Otevře se **Průvodce exportem certifikátu**.
2. V průvodci klikněte na **Další**. Vyberte **Ne, neexportovat privátní klíč** a klikněte na **Další**.
3. Na stránce **Formát souboru pro export** vyberte **X.509, kódování Base-64 (CER)** a klikněte na **Další**. 
4. V části **Soubor pro export** **přejděte** do umístění, do kterého chcete certifikát vyexportovat. V části **Název souboru** zadejte název souboru. Pak klikněte na tlačítko **Další**.
5. Certifikát vyexportujte kliknutím na **Dokončit**. Zobrazí se zpráva o **úspěšném exportu**. Kliknutím na **OK** zavřete průvodce.

### <a name="genclientcert"></a>Část 2: Vygenerování klientského certifikátu

Můžete buď vygenerovat jedinečný certifikát pro každého klienta, který se připojí, nebo můžete použít stejný certifikát na více klientů. Výhodou generování jedinečných certifikátů pro klienty je možnost v případě potřeby jednotlivý certifikát odvolat. V opačném případě, pokud všichni používají stejný klientský certifikát a zjistíte, že pro jednoho klienta je třeba tento certifikát odvolat, bude nutné vygenerovat a nainstalovat nové certifikáty pro všechny klienty, u nichž je odvolaný certifikát taktéž používán k ověření.

####<a name="enterprise-certificate"></a>Podnikový certifikát
- Pokud používáte podnikové certifikační řešení, vygenerujte klientský certifikát s běžným názvem ve formátu name@yourdomain.com (namísto formátu název_domény\uživatelské_jméno).
- Ujistěte se, že je vámi vydaný certifikát založený na šabloně uživatelského certifikátu, která má jako první položku v seznamu používání Ověření klienta místo Přihlášení pomocí čipové karty atd. Certifikát můžete zkontrolovat dvojím kliknutím na klientský certifikát a zobrazením **Podrobnosti > Použití rozšířeného klíče**.

####<a name="self-signed-root-certificate"></a>Kořenový certifikát podepsaný svým držitelem 
Pokud používáte kořenový certifikát podepsaný svým držitelem, přečtěte si v tématu s popisem [vygenerování klientského certifikátu pomocí PowerShellu](vpn-gateway-certificates-point-to-site.md#clientcert) kroky, podle kterých se generuje klientský certifikát kompatibilní s připojeními Point-to-Site.

### <a name="exportclientcert"></a>Část 3: Export klientského certifikátu
Pokud generujete klientský certifikát pomocí kořenového certifikátu podepsaného svým držitelem podle pokynů pro [PowerShell](vpn-gateway-certificates-point-to-site.md#clientcert), automaticky se nainstaluje na počítač, který jste použili k jeho vygenerování. Pokud chcete klientský certifikát nainstalovat do jiného klientského počítače, musíte ho exportovat.

1. Chcete-li exportovat klientský certifikát, otevřete **certmgr.msc**. Klikněte pravým tlačítkem na klientský certifikát, který chcete exportovat, klikněte na **všechny úlohy**, a poté klikněte na **exportovat**. Otevře se **Průvodce exportem certifikátu**.
2. V průvodci klikněte na **Další**, vyberte **Ano, exportovat privátní klíč** a klikněte na **Další**.
3. Na stránce **Formát souboru pro export** ponechte vybrané výchozí nastavení. Ujistěte se, že je vybrána možnost **Zahrnout všechny certifikáty na cestě k certifikátu, pokud je to možné**. Pak klikněte na tlačítko **Další**.
4. Na stránce **Zabezpečení** je nutné chránit soukromý klíč. Pokud vyberete použití hesla, ujistěte se, že jste si poznamenali nebo si pamatujete heslo, které jste pro tento certifikát nastavili. Pak klikněte na tlačítko **Další**.
5. V části **Soubor pro export** **přejděte** do umístění, do kterého chcete certifikát vyexportovat. V části **Název souboru** zadejte název souboru. Pak klikněte na tlačítko **Další**.
6. Certifikát vyexportujte kliknutím na **Dokončit**.

## <a name="upload"></a>Část 3 – Nahrání souboru .cer kořenového certifikátu
Po vytvoření brány můžete do Azure nahrát soubor .cer pro důvěryhodný kořenový certifikát. Můžete nahrát soubory až 20 kořenových certifikátů. Privátní klíč kořenového certifikátu nebudete nahrávat do Azure. Nahraný soubor .cer se v Azure používá k ověřování klientů, kteří se připojují k virtuální síti.

1. V části **Připojení VPN** v okně pro vaši síť VNet klikněte na obrázek **klienti**, čímž otevřete okno **Připojení VPN typu point-to-site**.

    ![Klienti](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/clients125.png)
2. V okně **Připojení VPN typu point-to-site** klikněte na **Správa certifikátů**, čímž otevřete okno **Certifikáty**.<br>

    ![Okno Certifikáty](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/ptsmanage.png)<br><br>
3. V okně **Certifikáty** klikněte na **Nahrát**, čímž otevřete okno **Nahrát certifikát**.<br>

    ![Okno pro nahrání certifikátů](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/uploadcerts.png)<br>
4. Klikněte na obrázek složky a vyhledejte soubor .cer. Vyberte soubor a potom klikněte na **OK**. Aktualizujte stránku, aby se nahraný certifikát zobrazil v okně **Certifikáty**.

    ![Nahrání certifikátu](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/upload.png)<br>

## <a name="vpnclientconfig"></a>Oddíl 4 – Generování konfiguračního balíčku klienta VPN
Chcete-li se připojit k virtuální síti, je potřeba nakonfigurovat také klienta VPN. Pro připojení klientského počítače vyžaduje klientský certifikát a správný konfigurační balíček klienta VPN.

Balíček klienta VPN obsahuje informace o konfiguraci pro konfigurování klientského softwaru VPN, který je součástí Windows. Balíček neinstaluje další software. Nastavení jsou specifická pro virtuální síť, ke které se chcete připojit. Seznam podporovaných klientských operačních systémů naleznete v části [Nejčastější dotazy o připojení Point-to-Site](#faq) na konci tohoto článku.

### <a name="to-generate-the-vpn-client-configuration-package"></a>Generování konfiguračního balíčku klienta VPN
1. Na portálu Azure Portal klikněte v okně **Přehled** pro vaši síť VNet na grafiku „klienti“ v poli **Připojení VPN**, čímž otevřete okno **Připojení VPN typu point-to-site**.
2. V horní části okna **Připojení VPN typu point-to-site** vyberte balíček ke stažení odpovídající operačnímu systému klienta, na který se bude instalovat:

   * U 64bitových klientů vyberte **Klient VPN (64bitový)**.
   * U 32bitových klientů vyberte **Klient VPN (32bitový)**.

     ![Stažení balíčku pro konfiguraci klienta VPN](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/dlclient.png)<br>
3. Zobrazí se zpráva, že Azure generuje konfigurační balíček klienta VPN pro virtuální síť. Za několik minut se balíček vygeneruje a zobrazí se zpráva v místním počítači, že proběhlo jeho stažení. Soubor konfiguračního balíčku uložte. Nainstalujete ho na každý klientský počítač, který se bude připojovat k virtuální síti metodou P2S.

## <a name="clientconfiguration"></a>Část 5 – Konfigurace klientského počítače
### <a name="part-1-install-an-exported-client-certificate"></a>Část 1: Instalace exportovaného klientského certifikátu

Pokud chcete vytvořit připojení P2S z jiného klientského počítače, než který jste použili k vytvoření klientských certifikátů, budete muset klientský certifikát nainstalovat. Při instalaci klientského certifikátu budete potřebovat heslo, které bylo vytvořeno při jeho exportu.

1. Vyhledejte a zkopírujte soubor *.pfx* na klientský počítač. Na klientském počítači spusťte instalaci dvojím kliknutím na soubor *.pfx*. Ponechte **Umístění úložiště** jako **Aktuální uživatel** a pak klikněte na **Další**.
2. Na stránce **Importovaný soubor** neprovádějte žádné změny. Klikněte na **Další**.
3. Na stránce **Ochrana privátního klíče** zadejte heslo pro certifikát, pokud jste ho použili, nebo ověřte správnost instalujícího objektu zabezpečení, a pak klikněte na **Další**.
4. Na stránce **Úložiště certifikátů** ponechte výchozí umístění a pak klikněte na **Další**.
5. Klikněte na **Finish** (Dokončit). V okně **Upozornění zabezpečení** pro instalaci certifikátu klikněte na **Ano**. Na tlačítko Ano můžete s klidem kliknout, protože jste certifikát generovali vy. Certifikát se teď úspěšně naimportoval.

### <a name="part-2-install-the-vpn-client-configuration-package"></a>Část 2: Instalace konfiguračního balíčku klienta VPN
V každém klientském počítači můžete použít stejný konfigurační balíček klienta VPN za předpokladu, že jeho verze odpovídá architektuře klienta.

1. Zkopírujte konfigurační soubor do počítače, který chcete připojit k virtuální síti. 
2. Dvojím kliknutím na soubor .exe spusťte instalaci balíčku na klientském počítači. Konfigurační balíček není podepsán, protože jste ho vytvořili vy. Proto se může zobrazit upozornění. Pokud se zobrazí automaticky otevírané okno filtru Windows SmartScreen, klikněte na **Další informace** (vlevo) a potom na **Přesto spustit**, aby se balíček nainstaloval.
3. Na klientském počítači přejděte do části **Nastavení sítě** a klikněte na možnost **Síť VPN**. Zobrazí se příslušné připojení. Zobrazí se název virtuální sítě, ke kterému se bude počítač připojovat. Název bude vypadat nějak takto:

    ![Klient VPN](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/vpn.png)

## <a name="connect"></a>Část 6 – Připojení k Azure
### <a name="connect-to-your-vnet"></a>Připojení k síti VNet
1. Chcete-li se připojit ke své síti VNet, přejděte na klientském počítači na připojení VPN a vyhledejte připojení VPN, které jste vytvořili. Bude mít stejný název jako vaše virtuální síť. Klikněte na **Připojit**. Může se zobrazit místní zpráva týkající se použití certifikátu. Pokud k tomu dojde, klikněte na možnost **Pokračovat**, abyste použili zvýšená oprávnění.
2. Připojení spustíte kliknutím na tlačítko **Připojit** na stavové stránce **Připojení**. Pokud uvidíte obrazovku **Výběr certifikátu**, ujistěte se, že zobrazený klientský certifikát je ten, který chcete pro připojení použít. Pokud není, vyberte pomocí šipky rozevíracího seznamu správný certifikát, a poté klikněte na **OK**.

    ![Připojení klienta VPN](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/clientconnect.png)
3. Připojení by nyní mělo být navázáno.

    ![Vytvořené připojení](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/connected.png)

> [!NOTE]
> Pokud používáte certifikát vydaný řešením CA organizace a máte problémy s ověřováním, zkontrolujte pořadí ověřování na klientském certifikátu. Pořadí seznamu ověřování můžete zkontrolovat dvojím kliknutím na klientský certifikát a přechodem na **Podrobnosti > Použití rozšířeného klíče**. Ujistěte se, že je na prvním místě seznamu položka Ověření klienta. Pokud tomu tak není, musíte vydat klientský certifikát založený na šabloně uživatele, která má Ověření klienta jako první položku seznamu. 
>
>

### <a name="verify-the-vpn-connection"></a>Ověření připojení VPN
1. Chcete-li ověřit, zda je připojení VPN aktivní, v příkazovém řádku se zvýšenými oprávněními spusťte příkaz *ipconfig/all*.
2. Zkontrolujte výsledky. Všimněte si, že IP adresa, kterou jste obdrželi, je jedna z adres z rozsahu adres připojení typu Point-to-Site, který jste určili během vytváření vaší virtuální sítě. Výsledek by se měl podobat následujícímu příkladu:

Příklad:

    PPP adapter VNet1:
        Connection-specific DNS Suffix .:
        Description.....................: VNet1
        Physical Address................:
        DHCP Enabled....................: No
        Autoconfiguration Enabled.......: Yes
        IPv4 Address....................: 192.168.130.2(Preferred)
        Subnet Mask.....................: 255.255.255.255
        Default Gateway.................:
        NetBIOS over Tcpip..............: Enabled

## <a name="add"></a>Přidání a odebrání důvěryhodných kořenových certifikátů

Důvěryhodný kořenový certifikát můžete do Azure přidat nebo ho z Azure odebrat. Pokud odeberete důvěryhodný certifikát, nebude již možné se pomocí klientských certifikátů vygenerovaných z tohoto kořenového certifikátu připojit k Azure prostřednictvím připojení Point-to-Site. Pokud chcete, aby se klienti mohli i nadále připojovat, je nutné nainstalovat nový certifikát, který je vygenerován z certifikátu, který Azure považuje za důvěryhodný.

### <a name="to-add-a-trusted-root-certificate"></a>Přidání důvěryhodného kořenového certifikátu

Do Azure můžete přidat až 20 souborů .cer s důvěryhodnými kořenovými certifikáty. Pokyny najdete v [části 3 – Nahrání souboru .cer kořenového certifikátu](#upload).

### <a name="to-remove-a-trusted-root-certificate"></a>Odebrání důvěryhodného kořenového certifikátu


1. V části **Připojení VPN** v okně pro vaši síť VNet klikněte na obrázek **klienti**, čímž otevřete okno **Připojení VPN typu point-to-site**.

    ![Klienti](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/clients125.png)
2. V okně **Připojení VPN typu point-to-site** klikněte na **Správa certifikátů**, čímž otevřete okno **Certifikáty**.<br>

    ![Okno Certifikáty](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/ptsmanage.png)<br><br>
3. V okně **Certifikáty** klikněte na tři tečky vedle certifikátu, který chcete odebrat, a potom klikněte na **Odstranit**.

     ![Odstranění kořenového certifikátu](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/deleteroot.png)<br>


## <a name="revokeclient"></a>Odvolání klientského certifikátu
Certifikáty klientů lze odvolat. Seznam odvolaných certifikátů umožňuje selektivně odepřít připojení Point-to-Site na základě jednotlivých klientských certifikátů. To se liší od odebrání důvěryhodného kořenového certifikátu. Pokud odeberete z Azure důvěryhodný kořenový certifikát v souboru .cer, dojde k odvolání přístupu pro všechny klientské certifikáty, které byly tímto odvolaným kořenovým certifikátem vygenerovány nebo podepsány. Odvolání klientského certifikátu namísto kořenového certifikátu umožňuje používat k ověřování pro připojení typu Point-to-Site další certifikáty vygenerované z kořenového certifikátu.

Běžnou praxí je použití kořenového certifikátu pro řízení přístupu na úrovni týmu nebo organizace, přičemž odvolání klientských certifikátů slouží pro detailní kontrolu přístupu jednotlivých uživatelů.

### <a name="to-revoke-a-client-certificate"></a>Odvolání klientského certifikátu

Klientský certifikát můžete odvolat tím, že přidáte jeho kryptografický otisk do seznamu odvolaných certifikátů.

1. Načtěte kryptografický otisk klientského certifikátu. Další informace najdete v tématu [Postup: Načtení kryptografického otisku certifikátu](https://msdn.microsoft.com/library/ms734695.aspx).
2. Zkopírujte údaje do textového editoru a smažte všechny mezery, aby vznikl souvislý řetězec.
3. Přejděte do okna **název_klasické_virtuální_sítě > Připojení VPN typu Point-to-Site > Certifikáty** a klikněte na **Seznam odvolaných certifikátů**. Otevře se okno Seznam odvolaných certifikátů. 
4. V okně **Seznam odvolaných certifikátů** kliknutím na **+ Přidat certifikát** otevřete okno **Přidat certifikát do seznamu odvolaných certifikátů**.
5. V okně **Přidat certifikát do seznamu odvolaných certifikátů** vložte kryptografický otisk certifikátu jako souvislý řádek textu bez mezer. Klikněte na **OK** v dolní části okna.
6. Po dokončení aktualizace už nebude možné certifikát použít k připojení. Klientům, kteří se pokusí připojit pomocí tohoto certifikátu, se zobrazí zpráva s informací o neplatnosti certifikátu.


## <a name="faq"></a>Nejčastější dotazy týkající se připojení Point-to-Site

[!INCLUDE [Point-to-Site FAQ](../../includes/vpn-gateway-point-to-site-faq-include.md)]

## <a name="next-steps"></a>Další kroky
Po dokončení připojení můžete do virtuálních sítí přidávat virtuální počítače. Další informace najdete v tématu [Virtuální počítače](https://docs.microsoft.com/azure/#pivot=services&panel=Compute). Bližší informace o sítích a virtuálních počítačích najdete v tématu s [přehledem sítě virtuálních počítačů s Linuxem v Azure](../virtual-machines/virtual-machines-linux-azure-vm-network-overview.md).

