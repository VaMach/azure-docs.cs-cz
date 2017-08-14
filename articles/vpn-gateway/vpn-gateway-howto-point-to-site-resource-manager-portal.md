---
title: "Připojení počítače k virtuální síti typu Point-to-Site s použitím ověření certifikátu: Azure Portal | Dokumentace Microsoftu"
description: "Připojte počítač bezpečně ke službě Azure Virtual Network vytvořením připojení brány VPN typu Point-to-Site s použitím ověření certifikátu. Tento článek se týká modelu nasazení Resource Manager a používá Azure Portal."
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: a15ad327-e236-461f-a18e-6dbedbf74943
ms.service: vpn-gateway
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/03/2017
ms.author: cherylmc
ms.translationtype: HT
ms.sourcegitcommit: 8b857b4a629618d84f66da28d46f79c2b74171df
ms.openlocfilehash: 28bc587204be30b1b29da7c2235e347d778b908e
ms.contentlocale: cs-cz
ms.lasthandoff: 08/04/2017

---
# <a name="configure-a-point-to-site-connection-to-a-vnet-using-certificate-authentication-azure-portal"></a>Konfigurace připojení typu Point-to-Site k virtuální síti s použitím ověření certifikátu: Azure Portal

Tento článek ukazuje postup vytvoření virtuální sítě s připojením typu Point-to-Site v modelu nasazení Resource Manager pomocí webu Azure Portal. Tato konfigurace používá certifikáty k ověření připojujícího se klienta. Tuto konfiguraci můžete vytvořit také pomocí jiného nástroje nasazení nebo pro jiný model nasazení, a to výběrem jiné možnosti z následujícího seznamu:

> [!div class="op_single_selector"]
> * [Azure Portal](vpn-gateway-howto-point-to-site-resource-manager-portal.md)
> * [PowerShell](vpn-gateway-howto-point-to-site-rm-ps.md)
> * [Azure Portal (Classic)](vpn-gateway-howto-point-to-site-classic-azure-portal.md)
>
>

Konfigurace Point-to-Site (P2S) umožňuje vytvořit zabezpečené připojení jednotlivých klientských počítačů k virtuální síti. Připojení Point-to-Site jsou užitečná, když se chcete ke své virtuální síti připojit ze vzdáleného umístění, například z domova nebo z místa konání konference, nebo když máte jen několik klientů, kteří se potřebují připojovat k virtuální síti. Připojení k síti VPN typu Point-to-Site je zahájeno z klientského počítače pomocí nativního klienta Windows VPN. Připojovaní klienti používají certifikáty k ověření. 

![Diagram Point-to-Site](./media/vpn-gateway-howto-point-to-site-resource-manager-portal/point-to-site-connection-diagram.png)

Připojení typu Point-to-Site s ověřováním certifikátů vyžadují následující:

* Bránu VPN typu RouteBased.
* Veřejný klíč (soubor .cer) pro kořenový certifikát nahraný do Azure. Ten se považuje za důvěryhodný certifikát a používá se k ověřování.
* Klientský certifikát vygenerovaný z kořenového certifikátu a nainstalovaný na každém klientském počítači, který se bude připojovat. Tento certifikát se používá k ověřování klienta.
* Na každém klientském počítači, který se bude připojovat, musí být vygenerován a nainstalován balíček pro konfiguraci klienta VPN. Balíček pro konfiguraci klienta konfiguruje nativního klienta VPN, který již je v operačním systému, s použitím informací potřebných pro připojení k virtuální síti.

Připojení typu Point-to-Site nevyžadují zařízení VPN ani místní veřejnou IP adresu. Připojení VPN se vytváří přes protokol SSTP (Secure Socket Tunneling Protocol). Na straně serveru podporujeme SSTP verze 1.0, 1.1 a 1.2. Klient rozhodne, která verze se má použít. Pro Windows 8.1 a novější se standardně používá SSTP verze 1.2. 

Další informace o připojení Point-to-Site najdete v části [Nejčastější dotazy týkající se připojení Point-to-Site](#faq) na konci tohoto článku.

### <a name="example"></a>Příklady hodnot

Tyto hodnoty můžete použít k vytvoření testovacího prostředí nebo můžou sloužit k lepšímu pochopení příkladů v tomto článku:

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

## <a name="createvnet"></a>1 – Vytvoření virtuální sítě

Než začnete, ověřte, že máte předplatné Azure. Pokud ještě nemáte předplatné Azure, můžete si aktivovat [výhody pro předplatitele MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details) nebo si zaregistrovat [bezplatný účet](https://azure.microsoft.com/pricing/free-trial). Pokud vytváříte tuto konfiguraci jako cvičení, můžete použít tyto [příklady hodnot](#example).

[!INCLUDE [vpn-gateway-basic-vnet-rm-portal](../../includes/vpn-gateway-basic-vnet-rm-portal-include.md)]

## <a name="address"></a>2 – Určení adresního prostoru a podsítí

Po vytvoření virtuální sítě do ní můžete přidat další adresní prostor a podsítě.

[!INCLUDE [vpn-gateway-additional-address-space](../../includes/vpn-gateway-additional-address-space-include.md)]

## <a name="gatewaysubnet"></a>3 – Přidání podsítě brány

Před připojením virtuální sítě k bráně musíte nejdříve vytvořit podsíť brány pro virtuální síť, ke které se chcete připojit. Služby brány používají IP adresy určené v podsíti brány. Pokud je to možné, vytvořte podsíť brány s použitím bloku CIDR /28 nebo /27, aby byl k dispozici dostatek IP adres pro plnění dalších požadavků na konfiguraci v budoucnu.

Snímky obrazovky v této části slouží jako referenční příklady. Ujistěte se, že používáte rozsah adres GatewaySubnet, který odpovídá požadovaným hodnotám vaší konfigurace.

### <a name="to-create-a-gateway-subnet"></a>Chcete-li vytvořit podsíť brány

[!INCLUDE [vpn-gateway-add-gwsubnet-rm-portal](../../includes/vpn-gateway-add-gwsubnet-rm-portal-include.md)]

## <a name="dns"></a>4 – Určení serveru DNS (volitelné)

Po vytvoření virtuální sítě můžete přidat IP adresu serveru DNS, aby bylo možné zpracovávat překlad názvů. Zadaný server DNS by měl být server DNS, který dokáže přeložit názvy pro prostředky, ke kterým se připojujete.

[!INCLUDE [vpn-gateway-add-dns-rm-portal](../../includes/vpn-gateway-add-dns-rm-portal-include.md)]

## <a name="creategw"></a>5 – Vytvoření brány virtuální sítě

Připojení typu Point-to-Site vyžaduje následující nastavení:

* Typ brány: Síť VPN
* Typ sítě VPN: Založená na trasách

### <a name="to-create-a-virtual-network-gateway"></a>Chcete-li vytvořit bránu virtuální sítě

[!INCLUDE [create a vnet gateway](../../includes/vpn-gateway-add-gw-rm-portal-include.md)]

## <a name="generatecert"></a>6 – Vygenerování certifikátů

Azure používá certifikáty k ověřování klientů VPN pro sítě VPN Point-to-Site. Nahrajete do Azure informace o veřejném klíči kořenového certifikátu. Veřejný klíč se pak bude považovat za důvěryhodný. Klientské certifikáty musí být vygenerované z důvěryhodného kořenového certifikátu a pak nainstalované na každém klientském počítači v úložišti certifikátů v adresáři Certificates-Current User/Personal. Tento certifikát se používá k ověřování klienta při zahájení připojení k virtuální síti. 

Pokud používáte certifikáty podepsané svým držitelem, musí se vytvořit pomocí konkrétních parametrů. Certifikát podepsaný svým držitelem můžete vytvořit podle pokynů pro [PowerShell a Windows 10](vpn-gateway-certificates-point-to-site.md) nebo [MakeCert](vpn-gateway-certificates-point-to-site-makecert.md). Je důležité, abyste při práci s kořenovými certifikáty podepsanými svým držitelem a generování klientských certifikátů z kořenových certifikátů podepsaných svým držitelem postupovali podle pokynů. Jinak certifikáty, které vytvoříte, nebudou kompatibilní s připojeními typu Point-to-Site a zobrazí se chyba připojení.

### <a name="getcer"></a>Krok 1 – Získání souboru .cer pro kořenový certifikát

[!INCLUDE [get the root certificate](../../includes/vpn-gateway-p2s-rootcert-include.md)]

### <a name="generateclientcert"></a>Krok 2 – Vygenerování klientského certifikátu

[!INCLUDE [generates client certificate](../../includes/vpn-gateway-p2s-clientcert-include.md)]

## <a name="addresspool"></a>7 – Přidání fondu adres klienta

Fond adres klienta je rozsah privátních IP adres, který zadáte. Klienti připojující se přes Point-to-Site obdrží IP adresu z tohoto rozsahu. Použijte rozsah privátních IP adres, který se nepřekrývá s místním umístěním, ze kterého se budete připojovat, nebo s virtuální sítí, ke které se chcete připojit.

1. Jakmile je brána virtuální sítě vytvořená, přejděte do části **Nastavení** okna brány virtuální sítě. V části **Nastavení** kliknutím na **Konfigurace Point-to-Site** otevřete okno **Konfigurace**.

  ![Okno Point-to-Site](./media/vpn-gateway-howto-point-to-site-resource-manager-portal/configuration.png)
2. Automaticky vyplněný rozsah můžete odstranit a pak přidat rozsah privátních IP adres, který chcete použít. Kliknutím na **Uložit** ověřte a uložte nastavení.

  ![Fond adres klienta](./media/vpn-gateway-howto-point-to-site-resource-manager-portal/ipaddresspool.png)

## <a name="uploadfile"></a>8 – Nahrání souboru .cer kořenového certifikátu

Po vytvoření brány můžete nahrát soubor .cer (obsahující informace o veřejném klíči) důvěryhodného kořenového certifikátu do Azure. Jakmile je soubor .cer nahraný, Azure ho může použít k ověřování klientů s nainstalovaným klientským certifikátem vygenerovaným z důvěryhodného kořenového certifikátu. Později můžete podle potřeby nahrát další soubory s důvěryhodnými kořenovými certifikáty – celkem až 20. 

1. Certifikáty se přidávají v okně **Konfigurace Point-to-Site** v části **Kořenový certifikát**.  
2. Ujistěte se, že jste kořenový certifikát vyexportovali jako soubor .cer X.509 s kódováním Base-64. Je nutné certifikát exportovat v tomto formátu, abyste ho mohli otevřít v textovém editoru.
3. Otevřete certifikát v textovém editoru, například v Poznámkovém bloku. Při kopírování dat certifikátu se ujistěte, že kopírujete text jako jeden souvislý řádek bez konců řádků nebo odřádkování. Pokud chcete zobrazit konce řádků a odřádkování, může být nutné upravit zobrazení v textovém editoru na Zobrazit symbol / Zobrazit všechny znaky. Zkopírujte pouze tuto část jako jeden souvislý řádek:

  ![Data certifikátu](./media/vpn-gateway-howto-point-to-site-resource-manager-portal/copycert.png)
4. Vložte data certifikátu do pole **Data veřejného certifikátu**. Zadejte **Název** certifikátu a pak klikněte na tlačítko **Uložit**. Můžete přidat až 20 důvěryhodných kořenových certifikátů.

  ![Nahrání certifikátu](./media/vpn-gateway-howto-point-to-site-resource-manager-portal/rootcertupload.png)

## <a name="clientconfig"></a>9 – Instalace balíčku pro konfiguraci klienta VPN

Pokud se chcete připojit k virtuální síti pomocí sítě VPN typu Point-to-Site, musí mít každý klient nainstalovaný balíček pro konfiguraci nativního klienta VPN ve Windows. Konfigurační balíček konfiguruje nativního klienta VPN ve Windows pomocí nastavení potřebných pro připojení k virtuální síti.

V každém klientském počítači můžete použít stejný konfigurační balíček klienta VPN za předpokladu, že jeho verze odpovídá architektuře klienta. Seznam podporovaných klientských operačních systémů naleznete v části [Nejčastější dotazy o připojení Point-to-Site](#faq) na konci tohoto článku.

### <a name="step-1---download-the-client-configuration-package"></a>Krok 1 – Stažení konfiguračního balíčku klienta

1. V okně **Konfigurace Point-to-Site** kliknutím na **Stáhnout klienta VPN** otevřete okno **Stáhnout klienta VPN**. Generování balíčku bude trvat minutu nebo dvě.

  ![Stažení klienta VPN 1](./media/vpn-gateway-howto-point-to-site-resource-manager-portal/downloadvpnclient1.png)
2. Vyberte pro svého klienta správný balíček a pak klikněte na **Stáhnout**. Soubor konfiguračního balíčku uložte. Balíček konfigurace klienta VPN nainstalujete na každý klientský počítač, který se připojuje k virtuální síti.

  ![Stažení klienta VPN 2](./media/vpn-gateway-howto-point-to-site-resource-manager-portal/vpnclient.png)

### <a name="step-2---install-the-client-configuration-package"></a>Krok 2 – Instalace konfiguračního balíčku klienta

1. Zkopírujte konfigurační soubor do počítače, který chcete připojit k virtuální síti. 
2. Dvojím kliknutím na soubor .exe spusťte instalaci balíčku na klientském počítači. Protože jste konfigurační balíček vytvořili vy, není podepsaný a může se zobrazit upozornění. Pokud se zobrazí automaticky otevírané okno filtru Windows SmartScreen, klikněte na **Další informace** (vlevo) a potom na **Přesto spustit**, aby se balíček nainstaloval.
3. Nainstalujte balíček do klientského počítače. Pokud se zobrazí automaticky otevírané okno filtru Windows SmartScreen, klikněte na **Další informace** (vlevo) a potom na **Přesto spustit**, aby se balíček nainstaloval.
4. Na klientském počítači přejděte do části **Nastavení sítě** a klikněte na možnost **Síť VPN**. Připojení k síti VPN zobrazuje název virtuální sítě, ke které se připojuje.

## <a name="installclientcert"></a>10 – Instalace klientského certifikátu

Pokud chcete vytvořit připojení P2S z jiného klientského počítače, než který jste použili k vytvoření klientských certifikátů, budete muset klientský certifikát nainstalovat. Při instalaci klientského certifikátu budete potřebovat heslo, které bylo vytvořeno při jeho exportu. Obvykle stačí jenom dvakrát kliknout na certifikát a nainstalovat ho. Další informace najdete v tématu [Instalace exportovaného klientského certifikátu](vpn-gateway-certificates-point-to-site.md#install).

## <a name="connect"></a>11 – Připojení k Azure

1. Chcete-li se připojit ke své síti VNet, přejděte na klientském počítači na připojení VPN a vyhledejte připojení VPN, které jste vytvořili. Bude mít stejný název jako vaše virtuální síť. Klikněte na **Připojit**. Může se zobrazit místní zpráva týkající se použití certifikátu. Klikněte na **Pokračovat** pro použití zvýšených oprávnění.

2. Připojení spustíte kliknutím na tlačítko **Připojit** na stavové stránce **Připojení**. Pokud uvidíte obrazovku **Výběr certifikátu**, ujistěte se, že zobrazený klientský certifikát je ten, který chcete pro připojení použít. Pokud není, vyberte pomocí šipky rozevíracího seznamu správný certifikát, a poté klikněte na **OK**.

  ![Připojení klienta VPN k Azure](./media/vpn-gateway-howto-point-to-site-resource-manager-portal/clientconnect.png)
3. Vaše připojení bylo vytvořeno.

  ![Vytvořené připojení](./media/vpn-gateway-howto-point-to-site-resource-manager-portal/connected.png)

[!INCLUDE [verifies client certificates](../../includes/vpn-gateway-certificates-verify-client-cert-include.md)]

## <a name="verify"></a>12 – Ověření stavu připojení

1. Chcete-li ověřit, zda je připojení VPN aktivní, v příkazovém řádku se zvýšenými oprávněními spusťte příkaz *ipconfig/all*.
2. Zkontrolujte výsledky. Všimněte si, že IP adresa, kterou jste obdrželi, je jedna z adres z fondu adres klienta VPN připojení Point-to-Site, který jste určili během konfigurace. Výsledky jsou podobné tomuto příkladu:

  ```
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
  ```

## <a name="connectVM"></a>Připojení k virtuálnímu počítači

[!INCLUDE [Connect to a VM](../../includes/vpn-gateway-connect-vm-p2s-include.md)]

## <a name="add"></a>Přidání a odebrání důvěryhodných kořenových certifikátů

Důvěryhodný kořenový certifikát můžete do Azure přidat nebo ho z Azure odebrat. Když odeberete kořenový certifikát, klienti s certifikátem vygenerovaným z tohoto kořenového certifikátu se nebudou moci ověřit a proto ani připojit. Pokud chcete, aby se klient mohl i nadále ověřovat a připojovat, je nutné nainstalovat nový klientský certifikát vygenerovaný z kořenového certifikátu, který Azure považuje za důvěryhodný (je do Azure nahraný).

### <a name="to-add-a-trusted-root-certificate"></a>Přidání důvěryhodného kořenového certifikátu

Do Azure můžete přidat až 20 souborů .cer s důvěryhodnými kořenovými certifikáty. Pokyny najdete v části [Nahrání důvěryhodného kořenového certifikátu](#uploadfile) v tomto článku.

### <a name="to-remove-a-trusted-root-certificate"></a>Odebrání důvěryhodného kořenového certifikátu

1. Chcete-li odebrat důvěryhodný kořenový certifikát, přejděte do okna **Konfigurace Point-to-Site** pro vaši bránu virtuální sítě.
2. V části okna **Kořenový certifikát** vyhledejte certifikát, který chcete odebrat.
3. Klikněte na tři tečky vedle certifikátu a potom klikněte na Odebrat.

## <a name="revokeclient"></a>Odvolání klientského certifikátu

Certifikáty klientů lze odvolat. Seznam odvolaných certifikátů umožňuje selektivně odepřít připojení Point-to-Site na základě jednotlivých klientských certifikátů. To se liší od odebrání důvěryhodného kořenového certifikátu. Pokud odeberete z Azure důvěryhodný kořenový certifikát v souboru .cer, dojde k odvolání přístupu pro všechny klientské certifikáty, které byly tímto odvolaným kořenovým certifikátem vygenerovány nebo podepsány. Odvolání klientského certifikátu namísto kořenového certifikátu umožňuje používat k ověřování další certifikáty vygenerované z kořenového certifikátu.

Běžnou praxí je použití kořenového certifikátu pro řízení přístupu na úrovni týmu nebo organizace, přičemž odvolání klientských certifikátů slouží pro detailní kontrolu přístupu jednotlivých uživatelů.

### <a name="to-revoke-a-client-certificate"></a>Odvolání klientského certifikátu

Klientský certifikát můžete odvolat tím, že přidáte jeho kryptografický otisk do seznamu odvolaných certifikátů.

1. Načtěte kryptografický otisk klientského certifikátu. Další informace najdete v tématu [Postup načtení kryptografického otisku certifikátu](https://msdn.microsoft.com/library/ms734695.aspx).
2. Zkopírujte údaje do textového editoru a smažte všechny mezery, aby vznikl souvislý řetězec.
3. Přejděte do okna **Konfigurace Point-to-Site** brány virtuální sítě. To je stejné okno, které jste použili k [nahrání důvěryhodného kořenového certifikátu](#uploadfile).
4. V části **Odvolané certifikáty** zadejte popisný název certifikátu (nemusí to být CN certifikátu).
5. Zkopírujte řetězec kryptografického otisku a vložte jej do pole **Kryptografický otisk**.
6. Kryptografický otisk se ověří a automaticky přidá do seznamu odvolaných certifikátů. Na obrazovce se zobrazí zpráva informující o probíhající aktualizaci seznamu. 
7. Po dokončení aktualizace už nebude možné certifikát použít k připojení. Klientům, kteří se pokusí připojit pomocí tohoto certifikátu, se zobrazí zpráva s informací o neplatnosti certifikátu.

## <a name="faq"></a>Nejčastější dotazy týkající se připojení Point-to-Site

[!INCLUDE [Point-to-Site FAQ](../../includes/vpn-gateway-point-to-site-faq-include.md)]

## <a name="next-steps"></a>Další kroky
Po dokončení připojení můžete do virtuálních sítí přidávat virtuální počítače. Další informace najdete v tématu [Virtuální počítače](https://docs.microsoft.com/azure/#pivot=services&panel=Compute). Bližší informace o sítích a virtuálních počítačích najdete v tématu s [přehledem sítě virtuálních počítačů s Linuxem v Azure](../virtual-machines/linux/azure-vm-network-overview.md).
