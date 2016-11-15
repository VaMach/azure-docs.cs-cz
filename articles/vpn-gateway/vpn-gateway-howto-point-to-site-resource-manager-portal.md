---
title: "Konfigurace připojení brány VPN typu Point-to-Site k virtuální síti pomocí modelu nasazení Resource Manager a webu Azure Portal | Dokumentace Microsoftu"
description: "Připojte se bezpečně ke službě Azure Virtual Network vytvořením připojení brány VPN typu Point-to-Site pomocí Resource Manageru a webu Azure Portal."
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: carmonm
editor: 
tags: azure-resource-manager
ms.assetid: a15ad327-e236-461f-a18e-6dbedbf74943
ms.service: vpn-gateway
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/17/2016
ms.author: cherylmc
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: aeebed0c733b9fbac964cdc532ff9d364683609b


---
# <a name="configure-a-pointtosite-connection-to-a-vnet-using-the-azure-portal"></a>Konfigurace připojení typu Point-to-Site k virtuální síti pomocí webu Azure Portal
> [!div class="op_single_selector"]
> * [Resource Manager – Azure Portal](vpn-gateway-howto-point-to-site-resource-manager-portal.md)
> * [Resource Manager – PowerShell](vpn-gateway-howto-point-to-site-rm-ps.md)
> * [Classic – Azure Portal](vpn-gateway-howto-point-to-site-classic-azure-portal.md)
> 
> 

Konfigurace Point-to-Site (P2S) umožňuje vytvořit zabezpečené připojení jednotlivých klientských počítačů k virtuální síti. Připojení P2S je užitečné, když se chcete ke své virtuální síti připojit ze vzdáleného umístění, například z domova nebo z místa konání konference, nebo když máte jen několik klientů, kteří se potřebují připojovat k virtuální síti. 

Připojení typu Point-to-Site k fungování nevyžadují zařízení VPN ani veřejnou IP adresu. Připojení VPN je vytvořeno spuštěním připojení na klientském počítači. Další informace o připojeních typu Point-to-Site najdete v tématech [Služba VPN Gateway – nejčastější dotazy](vpn-gateway-vpn-faq.md#point-to-site-connections) a [Plánování a navrhování](vpn-gateway-plan-design.md).

Tento článek vás provede vytvořením virtuální sítě s připojením typu Point-to-Site v modelu nasazení Resource Manager pomocí webu Azure Portal.

### <a name="deployment-models-and-methods-for-p2s-connections"></a>Modely a metody nasazení pro připojení P2S
[!INCLUDE [deployment models](../../includes/vpn-gateway-deployment-models-include.md)]

Následující tabulka uvádí dva modely nasazení a dostupné metody nasazení pro konfigurace P2S. Když je článek s postupem konfigurace k dispozici, zařadíme do tabulky přímý odkaz na něj.

[!INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-table-point-to-site-include.md)]

## <a name="basic-workflow"></a>Základní pracovní postup
![Diagram Point-to-Site](./media/vpn-gateway-howto-point-to-site-rm-ps/p2srm.png "point-to-site")

### <a name="a-nameexampleaexample-values"></a><a name="example"></a>Příklady hodnot
* **Název: VNet1**
* **Adresní prostor: 192.168.0.0/16**<br>V tomto příkladu používáme pouze jeden adresní prostor. Pro svoji virtuální síť můžete mít více adresních prostorů.
* **Název podsítě: FrontEnd**
* **Rozsah adres podsítě: 192.168.1.0/24**
* **Předplatné:** Ujistěte se, že máte správné předplatné, pokud máte více než jedno.
* **Skupina prostředků: TestRG**
* **Umístění: Východní USA**
* **GatewaySubnet: 192.168.200.0/24**
* **Název brány virtuální sítě: VNet1GW**
* **Typ brány: Síť VPN**
* **Typ sítě VPN: Založená na trasách**
* **Veřejná IP adresa: VNet1GWpip**
* **Typ připojení: Point-to-Site**
* **Fond adres klienta: 172.16.201.0/24**<br>Klienti VPN, kteří se budou k virtuální síti připojovat pomocí tohoto připojení typu Point-to-Site, získají IP adresu ze zadaného fondu adres klienta.

## <a name="before-beginning"></a>Před zahájením
* Ověřte, že máte předplatné Azure. Pokud ještě nemáte předplatné Azure, můžete si aktivovat [výhody pro předplatitele MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) nebo si zaregistrovat [bezplatný účet](https://azure.microsoft.com/pricing/free-trial/).

## <a name="a-namecreatevnetapart-1-create-a-virtual-network"></a><a name="createvnet"></a>Část 1 – Vytvoření virtuální sítě
Pokud vytváříte tuto konfiguraci jako cvičení, můžete použít tyto [příklady hodnot](#example).

[!INCLUDE [vpn-gateway-basic-vnet-rm-portal](../../includes/vpn-gateway-basic-vnet-rm-portal-include.md)]

### <a name="2-add-additional-address-space-and-subnets"></a>2. Přidání dalšího adresního prostoru a podsítí
Po vytvoření virtuální sítě do ní můžete přidat další adresní prostor a podsítě.

[!INCLUDE [vpn-gateway-additional-address-space](../../includes/vpn-gateway-additional-address-space-include.md)]

### <a name="3-create-a-gateway-subnet"></a>3. Vytvoření podsítě brány
Před připojením virtuální sítě k bráně musíte nejdříve vytvořit podsíť brány pro virtuální síť, ke které se chcete připojit. Pokud je to možné, je nejlepší vytvořit podsíť brány s použitím bloku CIDR /28 nebo /27, aby byl k dispozici dostatek IP adres pro plnění dalších požadavků na konfiguraci v budoucnu.

Snímky obrazovky v této části slouží jako referenční příklady. Ujistěte se, že používáte rozsah adres GatewaySubnet, který odpovídá požadovaným hodnotám vaší konfigurace.

#### <a name="to-create-a-gateway-subnet"></a>Chcete-li vytvořit podsíť brány
[!INCLUDE [vpn-gateway-add-gwsubnet-rm-portal](../../includes/vpn-gateway-add-gwsubnet-rm-portal-include.md)]

### <a name="a-namednsa4-specify-a-dns-server-optional"></a><a name="dns"></a>4. Určení serveru DNS (volitelné)
[!INCLUDE [vpn-gateway-add-dns-rm-portal](../../includes/vpn-gateway-add-dns-rm-portal-include.md)]

## <a name="a-namecreategwapart-2-create-a-virtual-network-gateway"></a><a name="creategw"></a>Část 2 – Vytvoření brány virtuální sítě
Připojení typu Point-to-Site vyžaduje následující nastavení:

* Typ brány: Síť VPN
* Typ sítě VPN: Založená na trasách

### <a name="to-create-a-virtual-network-gateway"></a>Chcete-li vytvořit bránu virtuální sítě
[!INCLUDE [vpn-gateway-add-gw-rm-portal](../../includes/vpn-gateway-add-gw-rm-portal-include.md)]

## <a name="a-namegeneratecertapart-3-generate-certificates"></a><a name="generatecert"></a>Část 3 – Generování certifikátů
Azure používá certifikáty k ověřování klientů VPN pro sítě VPN Point-to-Site. Data veřejného certifikátu (ne privátní klíč) exportujete jako soubor .cer X.509 s kódováním Base-64 buď z kořenového certifikátu vygenerovaného podnikovým certifikačním řešením, nebo z certifikátu podepsaného svým držitelem. Pak naimportujete data veřejného certifikátu z kořenového certifikátu do Azure. Dále je potřeba vygenerovat klientský certifikát z kořenového certifikátu pro klienty. Každý klient, který se chce připojit k virtuální síti pomocí připojení P2S, musí mít nainstalovaný klientský certifikát vygenerovaný z kořenového certifikátu.

### <a name="a-namegetcera1-obtain-the-cer-file-for-the-root-certificate"></a><a name="getcer"></a>1. Získání souboru .cer pro kořenový certifikát
Pokud používáte podnikové řešení, můžete použít stávající řetěz certifikátů. Pokud nepoužíváte podnikové řešení certifikační autority, můžete vytvořit kořenový certifikát podepsaný svým držitelem. Jedním ze způsobů vytvoření certifikátu podepsaného svým držitelem je použití nástroje makecert.

* Pokud používáte podnikový certifikační systém, získejte soubor .cer pro kořenový certifikát, který chcete použít. 
* Pokud nepoužíváte podnikové certifikační řešení, musíte vygenerovat kořenový certifikát podepsaný svým držitelem. Kroky pro Windows 10 najdete v tématu [Práce s kořenovými certifikáty podepsanými svým držitelem pro konfigurace Point-to-Site](vpn-gateway-certificates-point-to-site.md).

1. Pokud chcete získat soubor .cer z certifikátu, otevřete soubor **certmgr.msc** a vyhledejte kořenový certifikát. Klikněte pravým tlačítkem na kořenový certifikát podepsaný svým držitelem, klikněte na **Všechny úkoly** a potom na **Exportovat**. Otevře se **Průvodce exportem certifikátu**.
2. V průvodci klikněte na **Další**, vyberte **Ne, neexportovat privátní klíč** a klikněte na **Další**.
3. Na stránce **Formát souboru pro export** vyberte **X.509, kódování Base-64 (CER).** Pak klikněte na **Další**. 
4. V části **Soubor pro export** **přejděte** do umístění, do kterého chcete certifikát vyexportovat. V části **Název souboru** zadejte název souboru. Pak klikněte na tlačítko **Další**.
5. Certifikát vyexportujte kliknutím na **Dokončit**.

### <a name="a-namegenerateclientcerta2-generate-a-client-certificate"></a><a name="generateclientcert"></a>2. Vygenerování klientského certifikátu
Můžete buď vygenerovat jedinečný certifikát pro každého klienta, který se připojí, nebo můžete použít stejný certifikát na více klientů. Výhodou generování jedinečných certifikátů pro klienty je možnost v případě potřeby jednotlivý certifikát odvolat. V opačném případě, pokud všichni používají stejný klientský certifikát a zjistíte, že pro jednoho klienta je třeba tento certifikát odvolat, bude nutné vygenerovat a nainstalovat nové certifikáty pro všechny klienty, u nichž je odvolaný certifikát taktéž používán k ověření.

* Pokud používáte podnikové certifikační řešení, vygenerujte klientský certifikát s běžným názvem ve formátu 'name@yourdomain.com', (namísto formátu název domény\uživatelské jméno). 
* Používáte-li certifikát podepsaný svým držitelem, naleznete informace o vygenerování certifikátu klienta v tématu [Práce s kořenovými certifikáty podepsanými svými držiteli pro účely konfigurace připojení Point-to-Site](vpn-gateway-certificates-point-to-site.md).

### <a name="a-nameexportclientcerta3-export-the-client-certificate"></a><a name="exportclientcert"></a>3. Export klientského certifikátu
Klientský certifikát je vyžadován pro ověřování. Po vygenerování klientský certifikát vyexportujte. Vyexportovaný klientský certifikát se později nainstaluje na každý klientský počítač.

1. Chcete-li exportovat klientský certifikát, použijte nástroj *certmgr.msc*. Klikněte pravým tlačítkem na klientský certifikát, který chcete exportovat, klikněte na **všechny úlohy**, a poté klikněte na **exportovat**.
2. Exportujte klientský certifikát s privátním klíčem. Jedná se o soubor *.pfx*. Ujistěte se, že jste si poznamenali nebo si pamatujete heslo (klíč), které jste pro tento certifikát nastavili.

## <a name="a-nameaddresspoolapart-4-add-the-client-address-pool"></a><a name="addresspool"></a>Část 4 – Přidání fondu adres klienta
1. Jakmile je brána virtuální sítě vytvořená, přejděte do části **Nastavení** okna brány virtuální sítě. V části **Nastavení** kliknutím na **Konfigurace Point-to-Site** otevřete okno **Konfigurace**.
   
    ![okno point-to-site](./media/vpn-gateway-howto-point-to-site-resource-manager-portal/configuration.png "point to site blade")
2. **Fond adres** je fond IP adres, ze kterého získají IP adresu klienti, kteří se připojí. Přidejte fond adres a pak klikněte na **Uložit**.
   
    ![fond adres klienta](./media/vpn-gateway-howto-point-to-site-resource-manager-portal/addresspool.png "client address pool")

## <a name="a-nameuploadfileapart-5-upload-the-root-certificate-cer-file"></a><a name="uploadfile"></a>Část 5 – Nahrání souboru .cer kořenového certifikátu
Po vytvoření brány můžete do Azure nahrát soubor .cer pro důvěryhodný kořenový certifikát. Můžete nahrát soubory až 20 kořenových certifikátů. Privátní klíč kořenového certifikátu nebudete nahrávat do Azure. Nahraný soubor .cer se v Azure používá k ověřování klientů, kteří se připojují k virtuální síti.

1. Přejděte do okna **Konfigurace Point-to-Site**. Soubory .cer přidáte v části **Kořenový certifikát** tohoto okna.
   
    ![okno point-to-site](./media/vpn-gateway-howto-point-to-site-resource-manager-portal/rootcert.png "point to site blade")
2. Ujistěte se, že jste kořenový certifikát vyexportovali jako soubor .cer X.509 s kódováním Base-64. Je nutné jej exportovat v tomto formátu, abyste certifikát mohli otevřít v textovém editoru.
3. Otevřete certifikát v textovém editoru, například v Poznámkovém bloku. Zkopírujte pouze tuto část:
   
    ![data certifikátu](./media/vpn-gateway-howto-point-to-site-resource-manager-portal/copycert.png "certificate data")
4. Vložte data certifikátu do části portálu **Data veřejného certifikátu**. V místě pro **Název** uveďte název certifikátu a pak klikněte na **Uložit**. Můžete přidat až 20 důvěryhodných kořenových certifikátů.
   
    ![nahrání certifikátu](./media/vpn-gateway-howto-point-to-site-resource-manager-portal/uploadcert.png "certificate upload")

## <a name="a-nameclientconfigapart-6-download-and-install-the-vpn-client-configuration-package"></a><a name="clientconfig"></a>Část 6 – Stažení a instalace balíčku pro konfiguraci klienta VPN
Klienti připojení k Azure pomocí P2S musí mít nainstalovaný certifikát klienta i balíček pro konfiguraci klienta VPN. Pro klienty se systémem Windows jsou k dispozici balíčky pro konfiguraci klientů VPN. 

Balíček klienta VPN obsahuje informace o konfiguraci softwaru klienta VPN, který je součástí Windows. Konfigurace je specifická pro síť VPN, ke které se chcete připojit. Balíček neinstaluje další software. Další informace viz téma [Služba VPN Gateway – nejčastější dotazy](vpn-gateway-vpn-faq.md#point-to-site-connections).

1. V okně **Konfigurace Point-to-Site** kliknutím na **Stáhnout klienta VPN** otevřete okno **Stáhnout klienta VPN**.
   
    ![stažení klienta VPN](./media/vpn-gateway-howto-point-to-site-resource-manager-portal/downloadclient.png "VPN client download")
2. Vyberte pro svého klienta správný balíček a pak klikněte na **Stáhnout**. Pro 64bitové klienty vyberte **AMD64**. Pro 32bitové klienty vyberte **x86**.
3. Nainstalujte balíček do klientského počítače. Pokud se zobrazí automaticky otevírané okno filtru SmartScreen, klikněte na **Další informace** a potom na **Přesto spustit**, aby se balíček nainstaloval.
4. Na klientském počítači přejděte do části **Nastavení sítě** a klikněte na možnost **Síť VPN**. Zobrazí se příslušné připojení. Bude uveden název virtuální sítě, ke které se bude počítač připojovat. Název bude vypadat podobně jako tento příklad: 
   
    ![Klient VPN](./media/vpn-gateway-howto-point-to-site-resource-manager-portal/vpn.png "VPN client")

## <a name="a-nameinstallclientcertapart-7-install-the-client-certificate"></a><a name="installclientcert"></a>Část 7 – Instalace klientského certifikátu
Každý klientský počítač musí mít klientský certifikát pro ověření. Při instalaci klientského certifikátu budete potřebovat heslo, které bylo vytvořeno při jeho exportu.

1. Zkopírujte soubor .pfx na klientský počítač.
2. Spusťte instalaci poklikáním na soubor .pfx. Neměňte umístění instalace.

## <a name="a-nameconnectapart-8-connect-to-azure"></a><a name="connect"></a>Část 8 – Připojení k Azure
1. Chcete-li se připojit ke své síti VNet, přejděte na klientském počítači na připojení VPN a vyhledejte připojení VPN, které jste vytvořili. Bude mít stejný název jako vaše virtuální síť. Klikněte na **Připojit**. Může se zobrazit místní zpráva týkající se použití certifikátu. Pokud k tomu dojde, klikněte na možnost **Pokračovat**, abyste použili zvýšená oprávnění. 
2. Připojení spustíte kliknutím na tlačítko **Připojit** na stavové stránce **Připojení**. Pokud uvidíte obrazovku **Výběr certifikátu**, ujistěte se, že zobrazený klientský certifikát je ten, který chcete pro připojení použít. Pokud není, vyberte pomocí šipky rozevíracího seznamu správný certifikát, a poté klikněte na **OK**.
   
    ![Klient VPN 2](./media/vpn-gateway-howto-point-to-site-rm-ps/clientconnect.png "VPN client connection")
3. Připojení by nyní mělo být navázáno.
   
    ![Klient VPN 3](./media/vpn-gateway-howto-point-to-site-rm-ps/connected.png "VPN client connection 2")

## <a name="a-nameverifyapart-9-verify-your-connection"></a><a name="verify"></a>Část 9 – Ověření připojení
1. Chcete-li ověřit, zda je připojení VPN aktivní, v příkazovém řádku se zvýšenými oprávněními spusťte příkaz *ipconfig/all*.
2. Zkontrolujte výsledky. Všimněte si, že IP adresa, kterou jste obdrželi, je jedna z adres z fondu adres klienta VPN připojení Point-to-Site, který jste určili během konfigurace. Výsledek by se měl podobat následujícímu příkladu:
   
        PPP adapter VNet1:
            Connection-specific DNS Suffix .:
            Description.....................: VNet1
            Physical Address................:
            DHCP Enabled....................: No
            Autoconfiguration Enabled.......: Yes
            IPv4 Address....................: 172.16.201.3(Preferred)
            Subnet Mask.....................: 255.255.255.255
            Default Gateway.................:
            NetBIOS over Tcpip..............: Enabled

## <a name="a-nameaddato-add-or-remove-trusted-root-certificates"></a><a name="add"></a>Přidání a odebrání důvěryhodných kořenových certifikátů
Důvěryhodný kořenový certifikát můžete z Azure odebrat. Pokud odeberete důvěryhodný certifikát, nebude již možné se pomocí klientských certifikátů vygenerovaných z tohoto kořenového certifikátu připojit k Azure prostřednictvím připojení Point-to-Site. Pokud chcete, aby se klienti mohli i nadále připojovat, je nutné nainstalovat nový certifikát, který je vygenerován z certifikátu, který Azure považuje za důvěryhodný.

Seznam odvolaných klientských certifikátů můžete spravovat v okně **Konfigurace Point-to-Site**. To je okno, které jste použili k [nahrání důvěryhodného kořenového certifikátu](#uploadfile).

## <a name="a-namerevokeclientato-manage-the-list-of-revoked-client-certificates"></a><a name="revokeclient"></a>Správa seznamu odvolaných klientských certifikátů
Certifikáty klientů lze odvolat. Seznam odvolaných certifikátů umožňuje selektivně odepřít připojení Point-to-Site na základě jednotlivých klientských certifikátů. Pokud odeberete z Azure kořenový certifikát v souboru .cer, dojde k odvolání přístupu pro všechny klientské certifikáty, které byly tímto odvolaným kořenovým certifikátem vygenerovány nebo podepsány. Pokud chcete odvolat konkrétní klientský (nikoli kořenový) certifikát, můžete tak učinit. Při použití tohoto postupu zůstanou ostatní certifikáty, které byly vygenerovány z daného kořenového certifikátu, nadále v platnosti. 

Běžnou praxí je použití kořenového certifikátu pro řízení přístupu na úrovni týmu nebo organizace, přičemž odvolání klientských certifikátů slouží pro detailní kontrolu přístupu jednotlivých uživatelů.

Seznam odvolaných klientských certifikátů můžete spravovat v okně **Konfigurace Point-to-Site**. To je okno, které jste použili k [nahrání důvěryhodného kořenového certifikátu](#uploadfile).

## <a name="next-steps"></a>Další kroky
Do virtuální sítě můžete přidat virtuální počítač. Kroky jsou uvedeny v tématu [Vytvoření virtuálního počítače](../virtual-machines/virtual-machines-windows-hero-tutorial.md).




<!--HONumber=Nov16_HO2-->


