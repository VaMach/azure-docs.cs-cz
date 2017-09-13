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
ms.date: 08/10/2017
ms.author: cherylmc
ms.translationtype: HT
ms.sourcegitcommit: 9569f94d736049f8a0bb61beef0734050ecf2738
ms.openlocfilehash: cc9018d95ffce3b5b4a5ee20d5c78a2122e0223e
ms.contentlocale: cs-cz
ms.lasthandoff: 08/31/2017

---
# <a name="configure-a-point-to-site-connection-to-a-vnet-using-certificate-authentication-azure-portal"></a>Konfigurace připojení typu Point-to-Site k virtuální síti s použitím ověření certifikátu: Azure Portal

Tento článek ukazuje postup vytvoření virtuální sítě s připojením typu Point-to-Site v modelu nasazení Resource Manager pomocí webu Azure Portal. Tato konfigurace používá certifikáty k ověření připojujícího se klienta. Tuto konfiguraci můžete vytvořit také pomocí jiného nástroje nasazení nebo pro jiný model nasazení, a to výběrem jiné možnosti z následujícího seznamu:

> [!div class="op_single_selector"]
> * [Azure Portal](vpn-gateway-howto-point-to-site-resource-manager-portal.md)
> * [PowerShell](vpn-gateway-howto-point-to-site-rm-ps.md)
> * [Azure Portal (Classic)](vpn-gateway-howto-point-to-site-classic-azure-portal.md)
>
>

Brána VPN typu Point-to-Site (P2S) umožňuje vytvořit zabezpečené připojení k virtuální síti z jednotlivých klientských počítačů. Připojení VPN typu Point-to-Site jsou užitečná, když se chcete ke své virtuální síti připojit ze vzdáleného umístění, například při práci z domova nebo z místa konání konference. Síť VPN P2S je také užitečným řešením nahrazujícím síť VPN Site-to-Site, pokud máte pouze několik klientů, kteří se potřebují připojit k virtuální síti. 

P2S používá protokol SSTP (Secure Socket Tunneling Protocol), což je protokol VPN založený na protokolu SSL. Připojení VPN P2S se vytváří jeho zahájením z klientského počítače.

![Diagram Point-to-Site](./media/vpn-gateway-howto-point-to-site-resource-manager-portal/point-to-site-connection-diagram.png)

Připojení typu Point-to-Site s ověřováním certifikátů vyžadují následující:

* Bránu VPN typu RouteBased.
* Veřejný klíč (soubor .cer) pro kořenový certifikát nahraný do Azure. Jakmile je certifikát nahraný, považuje za důvěryhodný certifikát a používá se k ověřování.
* Klientský certifikát vygenerovaný z kořenového certifikátu a nainstalovaný na každém klientském počítači, který se bude k virtuální síti připojovat. Tento certifikát se používá k ověřování klienta.
* Konfigurační balíček klienta VPN. Konfigurační balíček klienta VPN obsahuje informace potřebné pro připojení klienta k virtuální síti. Balíček konfiguruje stávajícího klienta VPN nativního pro operační systém Windows. Každý klient, který se připojuje, musí být nakonfigurovaný pomocí konfiguračního balíčku.

Připojení typu Point-to-Site nevyžadují zařízení VPN ani místní veřejnou IP adresu. Připojení VPN se vytváří přes protokol SSTP (Secure Socket Tunneling Protocol). Na straně serveru podporujeme SSTP verze 1.0, 1.1 a 1.2. Klient rozhodne, která verze se má použít. Pro Windows 8.1 a novější se standardně používá SSTP verze 1.2.

Další informace o připojení Point-to-Site najdete v části [Nejčastější dotazy týkající se připojení Point-to-Site](#faq) na konci tohoto článku.

#### <a name="example"></a>Příklady hodnot

Tyto hodnoty můžete použít k vytvoření testovacího prostředí nebo můžou sloužit k lepšímu pochopení příkladů v tomto článku:

* **Název virtuální sítě:** VNet1
* **Adresní prostor:** 192.168.0.0/16<br>V tomto příkladu používáme pouze jeden adresní prostor. Pro svoji virtuální síť můžete mít více adresních prostorů.
* **Název podsítě:** FrontEnd
* **Rozsah adres podsítě:** 192.168.1.0/24
* **Předplatné:** Ujistěte se, že máte správné předplatné, pokud máte více než jedno.
* **Skupina prostředků:** TestRG
* **Umístění:** Východní USA
* **Podsíť brány:** 192.168.200.0/24<br>
* **Server DNS:** (volitelné) IP adresa serveru DNS, který chcete používat pro překlad názvů.
* **Název brány virtuální sítě:** VNet1GW
* **Typ brány:** Síť VPN
* **Typ sítě VPN:** Založená na trasách
* **Název veřejné IP adresy:** VNet1GWpip
* **Typ připojení:** Point-to-Site
* **Fond adres klienta:** 172.16.201.0/24<br>Klienti VPN, kteří se budou k virtuální síti připojovat pomocí tohoto připojení typu Point-to-Site, získají IP adresu ze zadaného fondu adres klienta.

## <a name="createvnet"></a>1. Vytvoření virtuální sítě

Než začnete, ověřte, že máte předplatné Azure. Pokud ještě nemáte předplatné Azure, můžete si aktivovat [výhody pro předplatitele MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details) nebo si zaregistrovat [bezplatný účet](https://azure.microsoft.com/pricing/free-trial).

[!INCLUDE [Basic Point-to-Site VNet](../../includes/vpn-gateway-basic-p2s-vnet-rm-portal-include.md)]

## <a name="gatewaysubnet"></a>2. Přidání podsítě brány

Před připojením virtuální sítě k bráně musíte nejdříve vytvořit podsíť brány pro virtuální síť, ke které se chcete připojit. Služby brány používají IP adresy určené v podsíti brány. Pokud je to možné, vytvořte podsíť brány s použitím bloku CIDR /28 nebo /27, aby byl k dispozici dostatek IP adres pro plnění dalších požadavků na konfiguraci v budoucnu.

[!INCLUDE [vpn-gateway-add-gwsubnet-rm-portal](../../includes/vpn-gateway-add-gwsubnet-p2s-rm-portal-include.md)]

## <a name="dns"></a>3. Určení serveru DNS (volitelné)

Po vytvoření virtuální sítě můžete přidat IP adresu serveru DNS, aby bylo možné zpracovávat překlad názvů. Server DNS je pro tuto konfiguraci volitelný, ale vyžaduje se, pokud chcete překlad názvů. Zadání hodnoty nevytvoří nový server DNS. Server DNS, jehož IP adresu zadáte, by měl být server DNS, který dokáže přeložit názvy pro prostředky, ke kterým se připojujete. V tomto příkladu jsme použili privátní IP adresu, ale je pravděpodobné, že to není IP adresa vašeho serveru DNS. Je potřeba, abyste použili svoje vlastní hodnoty.

[!INCLUDE [vpn-gateway-add-dns-rm-portal](../../includes/vpn-gateway-add-dns-rm-portal-include.md)]

## <a name="creategw"></a>4. Vytvoření brány virtuální sítě

[!INCLUDE [create-gateway](../../includes/vpn-gateway-add-gw-p2s-rm-portal-include.md)]

## <a name="generatecert"></a>5. Generování certifikátů

Azure používá certifikáty k ověřování klientů, kteří se připojují k virtuální síti přes připojení VPN typu Point-to-Site. Jakmile získáte kořenový certifikát, [nahrajete](#uploadfile) do Azure informace o veřejném klíči. Azure pak kořenový certifikát považuje za důvěryhodný pro připojení k virtuální síti přes Point-to-Site. Z kořenového certifikátu také generujete klientské certifikáty, které pak nainstalujete na každém klientském počítači. Klientský certifikát se používá k ověřování klienta při zahájení připojení k virtuální síti. 

### <a name="getcer"></a>1. Získání souboru .cer pro kořenový certifikát

[!INCLUDE [root-certificate](../../includes/vpn-gateway-p2s-rootcert-include.md)]

### <a name="generateclientcert"></a>2. Vygenerování klientského certifikátu

[!INCLUDE [generate-client-cert](../../includes/vpn-gateway-p2s-clientcert-include.md)]

## <a name="addresspool"></a>6. Přidání fondu adres klienta

Fond adres klienta je rozsah privátních IP adres, který zadáte. Klienti připojující se přes síť VPN typu Point-to-Site obdrží IP adresu z tohoto rozsahu. Použijte rozsah privátních IP adres, který se nepřekrývá s místním umístěním, ze kterého se připojujete, ani s virtuální sítí, ke které se chcete připojit.

1. Jakmile je brána virtuální sítě vytvořená, přejděte do části **Nastavení** na stránce brány virtuální sítě. V části **Nastavení** kliknutím na **Konfigurace Point-to-Site** otevřete stránku **Konfigurace Point-to-Site**.

  ![Stránka Point-to-Site](./media/vpn-gateway-howto-point-to-site-resource-manager-portal/gatewayblade.png)
2. Na stránce **Konfigurace Point-to-Site** můžete odstranit automaticky vyplněný rozsah a pak přidat rozsah privátních IP adres, který chcete použít. Kliknutím na **Uložit** ověřte a uložte nastavení.

  ![Fond adres klienta](./media/vpn-gateway-howto-point-to-site-resource-manager-portal/ipaddresspool.png)

## <a name="uploadfile"></a>7. Nahrání dat veřejného klíče kořenového certifikátu

Po vytvoření brány nahrajete informace o veřejném klíči kořenového certifikátu do Azure. Jakmile jsou data veřejného certifikátu nahraná, Azure ho může použít k ověřování klientů s nainstalovaným klientským certifikátem vygenerovaným z důvěryhodného kořenového certifikátu. Můžete nahrát další důvěryhodné kořenové certifikáty – celkem až 20.

1. Certifikáty se přidávají na stránce **Konfigurace Point-to-Site** v části **Kořenový certifikát**.  
2. Ujistěte se, že jste kořenový certifikát vyexportovali jako soubor .cer X.509 s kódováním Base-64. Je nutné certifikát exportovat v tomto formátu, abyste ho mohli otevřít v textovém editoru.
3. Otevřete certifikát v textovém editoru, například v Poznámkovém bloku. Při kopírování dat certifikátu se ujistěte, že kopírujete text jako jeden souvislý řádek bez konců řádků nebo odřádkování. Pokud chcete zobrazit konce řádků a odřádkování, může být nutné upravit zobrazení v textovém editoru na Zobrazit symbol / Zobrazit všechny znaky. Zkopírujte pouze tuto část jako jeden souvislý řádek:

  ![Data certifikátu](./media/vpn-gateway-howto-point-to-site-resource-manager-portal/copycert.png)
4. Vložte data certifikátu do pole **Data veřejného certifikátu**. Zadejte **Název** certifikátu a pak klikněte na tlačítko **Uložit**. Můžete přidat až 20 důvěryhodných kořenových certifikátů.

  ![Nahrání certifikátu](./media/vpn-gateway-howto-point-to-site-resource-manager-portal/rootcertupload.png)

## <a name="clientconfig"></a>8. Vygenerování a instalace konfiguračního balíčku klienta VPN

Pro připojení k virtuální síti pomocí sítě VPN typu Point-to-Site musí mít každý klient nainstalovaný konfigurační balíček klienta, který konfiguruje nativního klienta VPN s použitím nastavení a souborů potřebných pro připojení k virtuální síti. Konfigurační balíček klienta VPN konfiguruje nativního klienta VPN ve Windows, neinstaluje nového nebo jiného klienta VPN.

V každém klientském počítači můžete použít stejný konfigurační balíček klienta VPN za předpokladu, že jeho verze odpovídá architektuře klienta. Seznam podporovaných klientských operačních systémů naleznete v části [Nejčastější dotazy o připojení Point-to-Site](#faq) na konci tohoto článku.

### <a name="1-generate-and-download-the-client-configuration-package"></a>1. Vygenerování a stažení konfiguračního balíčku klienta

1. Na stránce **Konfigurace Point-to-Site** kliknutím na **Stáhnout klienta VPN** otevřete stránku **Stáhnout klienta VPN**. Generování balíčku bude trvat minutu nebo dvě.

  ![Stažení klienta VPN 1](./media/vpn-gateway-howto-point-to-site-resource-manager-portal/downloadvpnclient1.png)
2. Vyberte pro svého klienta správný balíček a pak klikněte na **Stáhnout**. Soubor konfiguračního balíčku uložte. Balíček konfigurace klienta VPN nainstalujete na každý klientský počítač, který se připojuje k virtuální síti.

  ![Stažení klienta VPN 2](./media/vpn-gateway-howto-point-to-site-resource-manager-portal/vpnclient.png)

### <a name="2-install-the-client-configuration-package"></a>2. Instalace konfiguračního balíčku klienta

1. Zkopírujte konfigurační soubor do počítače, který chcete připojit k virtuální síti. 
2. Dvojím kliknutím na soubor .exe spusťte instalaci balíčku na klientském počítači. Protože jste konfigurační balíček vytvořili vy, není podepsaný a může se zobrazit upozornění. Pokud se zobrazí automaticky otevírané okno filtru Windows SmartScreen, klikněte na **Další informace** (vlevo) a potom na **Přesto spustit**, aby se balíček nainstaloval.
3. Nainstalujte balíček do klientského počítače. Pokud se zobrazí automaticky otevírané okno filtru Windows SmartScreen, klikněte na **Další informace** (vlevo) a potom na **Přesto spustit**, aby se balíček nainstaloval.
4. Na klientském počítači přejděte do části **Nastavení sítě** a klikněte na možnost **Síť VPN**. Připojení k síti VPN zobrazuje název virtuální sítě, ke které se připojuje.

## <a name="installclientcert"></a>9. Instalace exportovaného klientského certifikátu

Pokud chcete vytvořit připojení P2S z jiného klientského počítače, než který jste použili k vytvoření klientských certifikátů, budete muset klientský certifikát nainstalovat. Při instalaci klientského certifikátu budete potřebovat heslo, které bylo vytvořeno při jeho exportu. Obvykle stačí jenom dvakrát kliknout na certifikát a nainstalovat ho.

Zkontrolujte, že se klientský certifikát vyexportoval jako soubor .pfx spolu s celým řetězem certifikátů (to je výchozí nastavení). Jinak informace o kořenovém certifikátu na klientském počítači nejsou a klient se nebude moct správně ověřit. Další informace najdete v tématu [Instalace exportovaného klientského certifikátu](vpn-gateway-certificates-point-to-site.md#install).

## <a name="connect"></a>10. Připojení k Azure

1. Chcete-li se připojit ke své síti VNet, přejděte na klientském počítači na připojení VPN a vyhledejte připojení VPN, které jste vytvořili. Bude mít stejný název jako vaše virtuální síť. Klikněte na **Připojit**. Může se zobrazit místní zpráva týkající se použití certifikátu. Klikněte na **Pokračovat** pro použití zvýšených oprávnění.

2. Připojení spustíte kliknutím na tlačítko **Připojit** na stavové stránce **Připojení**. Pokud uvidíte obrazovku **Výběr certifikátu**, ujistěte se, že zobrazený klientský certifikát je ten, který chcete pro připojení použít. Pokud není, vyberte pomocí šipky rozevíracího seznamu správný certifikát, a poté klikněte na **OK**.

  ![Připojení klienta VPN k Azure](./media/vpn-gateway-howto-point-to-site-resource-manager-portal/clientconnect.png)
3. Vaše připojení bylo vytvořeno.

  ![Vytvořené připojení](./media/vpn-gateway-howto-point-to-site-resource-manager-portal/connected.png)

#### <a name="troubleshooting-p2s-connections"></a>Řešení potíží s připojeními P2S

[!INCLUDE [verifies client certificates](../../includes/vpn-gateway-certificates-verify-client-cert-include.md)]

## <a name="verify"></a>11. Ověření stavu připojení

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

1. Pokud chcete odebrat důvěryhodný kořenový certifikát, přejděte na stránku **Konfigurace Point-to-Site** pro vaši bránu virtuální sítě.
2. V části stránky **Kořenový certifikát** vyhledejte certifikát, který chcete odebrat.
3. Klikněte na tři tečky vedle certifikátu a potom klikněte na Odebrat.

## <a name="revokeclient"></a>Odvolání klientského certifikátu

Certifikáty klientů lze odvolat. Seznam odvolaných certifikátů umožňuje selektivně odepřít připojení Point-to-Site na základě jednotlivých klientských certifikátů. To se liší od odebrání důvěryhodného kořenového certifikátu. Pokud odeberete z Azure důvěryhodný kořenový certifikát v souboru .cer, dojde k odvolání přístupu pro všechny klientské certifikáty, které byly tímto odvolaným kořenovým certifikátem vygenerovány nebo podepsány. Odvolání klientského certifikátu namísto kořenového certifikátu umožňuje používat k ověřování další certifikáty vygenerované z kořenového certifikátu.

Běžnou praxí je použití kořenového certifikátu pro řízení přístupu na úrovni týmu nebo organizace, přičemž odvolání klientských certifikátů slouží pro detailní kontrolu přístupu jednotlivých uživatelů.

### <a name="to-revoke-a-client-certificate"></a>Odvolání klientského certifikátu

Klientský certifikát můžete odvolat tím, že přidáte jeho kryptografický otisk do seznamu odvolaných certifikátů.

1. Načtěte kryptografický otisk klientského certifikátu. Další informace najdete v tématu [Postup načtení kryptografického otisku certifikátu](https://msdn.microsoft.com/library/ms734695.aspx).
2. Zkopírujte údaje do textového editoru a smažte všechny mezery, aby vznikl souvislý řetězec.
3. Přejděte na stránku **Konfigurace Point-to-Site** brány virtuální sítě. To je stejná stránka, kterou jste použili k [nahrání důvěryhodného kořenového certifikátu](#uploadfile).
4. V části **Odvolané certifikáty** zadejte popisný název certifikátu (nemusí to být CN certifikátu).
5. Zkopírujte řetězec kryptografického otisku a vložte jej do pole **Kryptografický otisk**.
6. Kryptografický otisk se ověří a automaticky přidá do seznamu odvolaných certifikátů. Na obrazovce se zobrazí zpráva informující o probíhající aktualizaci seznamu. 
7. Po dokončení aktualizace už nebude možné certifikát použít k připojení. Klientům, kteří se pokusí připojit pomocí tohoto certifikátu, se zobrazí zpráva s informací o neplatnosti certifikátu.

## <a name="faq"></a>Nejčastější dotazy týkající se připojení Point-to-Site

[!INCLUDE [Point-to-Site FAQ](../../includes/vpn-gateway-faq-point-to-site-include.md)]

## <a name="next-steps"></a>Další kroky
Po dokončení připojení můžete do virtuálních sítí přidávat virtuální počítače. Další informace najdete v tématu [Virtuální počítače](https://docs.microsoft.com/azure/#pivot=services&panel=Compute). Bližší informace o sítích a virtuálních počítačích najdete v tématu s [přehledem sítě virtuálních počítačů s Linuxem v Azure](../virtual-machines/linux/azure-vm-network-overview.md).
