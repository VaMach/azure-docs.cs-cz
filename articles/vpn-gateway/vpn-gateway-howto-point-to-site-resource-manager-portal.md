---
title: "Připojení počítače k virtuální síti typu Point-to-Site: Portál | Dokumentace Microsoftu"
description: "Připojte se bezpečně ke službě Azure Virtual Network vytvořením připojení brány VPN typu Point-to-Site pomocí Resource Manageru a webu Azure Portal."
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
ms.date: 04/10/2017
ms.author: cherylmc
translationtype: Human Translation
ms.sourcegitcommit: 785d3a8920d48e11e80048665e9866f16c514cf7
ms.openlocfilehash: d559151e49b4c74cef8cb0bf6452436152d2b7fe
ms.lasthandoff: 04/12/2017


---
# <a name="configure-a-point-to-site-connection-to-a-vnet-using-the-azure-portal"></a>Konfigurace připojení typu Point-to-Site k virtuální síti přes Azure Portal
> [!div class="op_single_selector"]
> * [Resource Manager – Azure Portal](vpn-gateway-howto-point-to-site-resource-manager-portal.md)
> * [Resource Manager – PowerShell](vpn-gateway-howto-point-to-site-rm-ps.md)
> * [Classic – Azure Portal](vpn-gateway-howto-point-to-site-classic-azure-portal.md)
> 
> 

Konfigurace Point-to-Site (P2S) umožňuje vytvořit zabezpečené připojení jednotlivých klientských počítačů k virtuální síti. P2S je připojení VPN prostřednictvím protokolu SSTP (Secure Socket Tunneling Protocol). Připojení Point-to-Site jsou užitečná, když se chcete ke své virtuální síti připojit ze vzdáleného umístění, například z domova nebo z místa konání konference, nebo když máte jen několik klientů, kteří se potřebují připojovat k virtuální síti. Připojení typu P2S nevyžadují zařízení VPN ani veřejnou IP adresu. Připojení VPN se navazuje ze strany klientského počítače.

Tento článek vás provede vytvořením virtuální sítě s připojením Point-to-Site s použitím webu Azure Portal. Další informace o připojení Point-to-Site najdete v části [Nejčastější dotazy týkající se připojení Point-to-Site](#faq) na konci tohoto článku.

### <a name="deployment-models-and-methods-for-p2s-connections"></a>Modely a metody nasazení pro připojení P2S
[!INCLUDE [deployment models](../../includes/vpn-gateway-deployment-models-include.md)]

Následující tabulka uvádí dva modely nasazení a dostupné metody nasazení pro konfigurace P2S. Když je článek s postupem konfigurace k dispozici, zařadíme do tabulky přímý odkaz na něj.

[!INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-table-point-to-site-include.md)]

## <a name="basic-workflow"></a>Základní pracovní postup
![Diagram Point-to-Site](./media/vpn-gateway-howto-point-to-site-resource-manager-portal/point-to-site-connection-diagram.png)

### <a name="example"></a>Příklady hodnot
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

## <a name="createvnet"></a>Část 1 – Vytvoření virtuální sítě
Než začnete, ověřte, že máte předplatné Azure. Pokud ještě nemáte předplatné Azure, můžete si aktivovat [výhody pro předplatitele MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details) nebo si zaregistrovat [bezplatný účet](https://azure.microsoft.com/pricing/free-trial). Pokud vytváříte tuto konfiguraci jako cvičení, můžete použít tyto [příklady hodnot](#example).

[!INCLUDE [vpn-gateway-basic-vnet-rm-portal](../../includes/vpn-gateway-basic-vnet-rm-portal-include.md)]

## <a name="address"></a>Část 2 – Určení adresního prostoru a podsítí
Po vytvoření virtuální sítě do ní můžete přidat další adresní prostor a podsítě.

[!INCLUDE [vpn-gateway-additional-address-space](../../includes/vpn-gateway-additional-address-space-include.md)]

## <a name="gatewaysubnet"></a>Část 3 – Přidání podsítě brány

Před připojením virtuální sítě k bráně musíte nejdříve vytvořit podsíť brány pro virtuální síť, ke které se chcete připojit. Služby brány používají IP adresy určené v podsíti brány. Pokud je to možné, vytvořte podsíť brány s použitím bloku CIDR /28 nebo /27, aby byl k dispozici dostatek IP adres pro plnění dalších požadavků na konfiguraci v budoucnu.

Snímky obrazovky v této části slouží jako referenční příklady. Ujistěte se, že používáte rozsah adres GatewaySubnet, který odpovídá požadovaným hodnotám vaší konfigurace.

###<a name="to-create-a-gateway-subnet"></a>Chcete-li vytvořit podsíť brány

[!INCLUDE [vpn-gateway-add-gwsubnet-rm-portal](../../includes/vpn-gateway-add-gwsubnet-rm-portal-include.md)]

## <a name="dns"></a>Část 4 – Určení serveru DNS (volitelné)

DNS se pro připojení Point-to-Site nevyžaduje. Pokud ale chcete umožnit překlad IP adres pro prostředky nasazované do vaší virtuální sítě, měli byste určit server DNS. Toto nastavení umožňuje určit server DNS, který chcete použít pro překlad IP adres pro tuto virtuální síť. Neslouží k vytvoření serveru DNS.

[!INCLUDE [vpn-gateway-add-dns-rm-portal](../../includes/vpn-gateway-add-dns-rm-portal-include.md)]

## <a name="creategw"></a>Část 5 – Vytvoření brány virtuální sítě
Připojení typu Point-to-Site vyžaduje následující nastavení:

* Typ brány: Síť VPN
* Typ sítě VPN: Založená na trasách

### <a name="to-create-a-virtual-network-gateway"></a>Chcete-li vytvořit bránu virtuální sítě
[!INCLUDE [vpn-gateway-add-gw-rm-portal](../../includes/vpn-gateway-add-gw-rm-portal-include.md)]

## <a name="generatecert"></a>Část 6 – Generování certifikátů

Azure používá certifikáty k ověřování klientů VPN pro sítě VPN Point-to-Site. Po vytvoření kořenového certifikátu vyexportujete data veřejného certifikátu (ne privátní klíč) jako soubor .cer X.509 v kódování Base-64. Pak odešlete data veřejného certifikátu z kořenového certifikátu do Azure.

Každý klientský počítač, který se připojuje k virtuální síti pomocí připojení Point-to-Site, musí mít nainstalovaný klientský certifikát. Klientský certifikát se generuje z kořenového certifikátu a nainstaluje na každý klientský počítač. Pokud není nainstalovaný platný klientský certifikát a klient se pokusí připojit k virtuální síti, ověření se nezdaří.

### <a name="getcer"></a>Krok 1 – Získání souboru .cer pro kořenový certifikát

#### <a name="enterprise-certificate"></a>Podnikový certifikát
 
Pokud používáte podnikové řešení, můžete použít stávající řetěz certifikátů. Získejte soubor .cer pro kořenový certifikát, který chcete používat.

#### <a name="self-signed-root-certificate"></a>Kořenový certifikát podepsaný svým držitelem

Pokud nepoužíváte podnikové certifikační řešení, musíte vytvořit kořenový certifikát podepsaný svým držitelem. Chcete-li vytvořit kořenový certifikát podepsaný svým držitelem, který obsahuje potřebná pole pro ověřování P2S, můžete použít PowerShell. Téma s popisem [vytvoření kořenového certifikátu podepsaného svým držitelem pro připojení Point-to-Site pomocí PowerShellu](vpn-gateway-certificates-point-to-site.md) vás provede postupem vytvoření kořenového certifikátu podepsaného svým držitelem.

> [!NOTE]
> Dříve se doporučovalo k vytvoření kořenového certifikátu podepsaného svým držitelem a k vygenerování klientských certifikátů pro připojení Point-to-Site používat makecert. Teď můžete k vytvoření těchto certifikátů použít PowerShell. Jednou z výhod použití PowerShellu je schopnost vytvářet certifikáty SHA-2. Požadované hodnoty najdete v tématu s popisem [vytvoření kořenového certifikátu podepsaného svým držitelem pro připojení Point-to-Site pomocí PowerShellu](vpn-gateway-certificates-point-to-site.md).
>
>


#### <a name="to-export-the-public-key-for-a-self-signed-root-certificate"></a>Export veřejného klíče pro kořenový certifikát podepsaný svým držitelem

Připojení Point-to-Site vyžadují, aby byl veřejný klíč (.cer) odeslán do Azure. Následující postup vám pomůže vyexportovat soubor .cer pro váš kořenový certifikát podepsaný svým držitelem.

1. Chcete-li získat soubor .cer z certifikátu, otevřete **správu uživatelských certifikátů**.
2. Vyhledejte v umístění Certifikáty – Aktuální uživatel\Osobní\Certifikáty kořenový certifikát P2SRootCert podepsaný svým držitelem a klikněte pravým tlačítkem myši. Klikněte na **Všechny úlohy** a potom klikněte na **Exportovat** a otevřete **Průvodce exportem certifikátu**.
3. V průvodci klikněte na **Další**. Vyberte **Ne, neexportovat privátní klíč** a klikněte na **Další**.
4. Na stránce **Formát souboru pro export** vyberte **X.509, kódování Base-64 (CER)** a pak klikněte na **Další**. 
5. Na stránce **Soubor pro export** přejděte na jednotku C:, vytvořte podadresář s názvem cert a vyberte ho. Pojmenujte soubor certifikátu P2SRootCert.cer a pak klikněte na **Uložit**. 
6. Certifikát vyexportujte kliknutím na **Další** a pak na **Dokončit**. Zobrazí se zpráva o **úspěšném exportu**. Kliknutím na **OK** zavřete průvodce.

### <a name="generateclientcert"></a>Krok 2 – Vygenerování klientského certifikátu
Můžete buď vygenerovat jedinečný certifikát pro každého klienta, nebo můžete použít stejný certifikát na více klientů. Výhodou generování jedinečných certifikátů pro klienty je možnost jednotlivý certifikát odvolat. V opačném případě, pokud všichni používají stejný klientský certifikát a vy ho potřebujete odvolat, bude nutné vygenerovat a nainstalovat nové certifikáty pro všechny klienty, kteří používají tento certifikát k ověření.

#### <a name="enterprise-certificate"></a>Podnikový certifikát
- Pokud používáte podnikové certifikační řešení, vygenerujte klientský certifikát s běžným názvem ve formátu name@yourdomain.com (namísto formátu název_domény\uživatelské_jméno).
- Ujistěte se, že je certifikát založený na šabloně uživatelského certifikátu, která má jako první položku v seznamu používání Ověření klienta místo Přihlášení pomocí čipové karty atd. Certifikát můžete zkontrolovat dvojím kliknutím na klientský certifikát a zobrazením **Podrobnosti > Použití rozšířeného klíče**.

#### <a name="self-signed-root-certificate"></a>Kořenový certifikát podepsaný svým držitelem 
Pokud používáte kořenový certifikát podepsaný svým držitelem, přečtěte si v tématu s popisem [vygenerování klientského certifikátu pomocí PowerShellu](vpn-gateway-certificates-point-to-site.md#clientcert) kroky, podle kterých se generuje klientský certifikát kompatibilní s připojeními Point-to-Site.


### <a name="exportclientcert"></a>Krok 3 – Export klientského certifikátu
Pokud generujete klientský certifikát pomocí kořenového certifikátu podepsaného svým držitelem podle pokynů pro [PowerShell](vpn-gateway-certificates-point-to-site.md#clientcert), automaticky se nainstaluje na počítač, který jste použili k jeho vygenerování. Pokud chcete klientský certifikát nainstalovat do jiného klientského počítače, musíte ho exportovat.
 
1. Pokud chcete exportovat klientský certifikát, otevřete **správu uživatelských certifikátů**. Klikněte pravým tlačítkem na klientský certifikát, který chcete exportovat, klikněte na **Všechny úlohy**, a poté klikněte na **Exportovat**, aby se otevřel **Průvodce exportem certifikátu**.
2. V průvodci klikněte na **Další**, vyberte **Ano, exportovat privátní klíč** a klikněte na **Další**.
3. Na stránce **Formát souboru pro export** ponechte vybrané výchozí nastavení. Zkontrolujte, že je vybraná možnost **Zahrnout všechny certifikáty na cestě k certifikátu, pokud je to možné**, aby se vyexportovaly také informace o požadovaném kořenovém certifikátu. Pak klikněte na **Další**.
4. Na stránce **Zabezpečení** je nutné chránit soukromý klíč. Pokud vyberete použití hesla, ujistěte se, že jste si poznamenali nebo si pamatujete heslo, které jste pro tento certifikát nastavili. Pak klikněte na **Další**.
5. V části **Soubor pro export** **přejděte** do umístění, do kterého chcete certifikát vyexportovat. V části **Název souboru** zadejte název souboru. Pak klikněte na **Další**.
6. Certifikát vyexportujte kliknutím na **Dokončit**.   

## <a name="addresspool"></a>Část 7 – Přidání fondu adres klienta
1. Jakmile je brána virtuální sítě vytvořená, přejděte do části **Nastavení** okna brány virtuální sítě. V části **Nastavení** kliknutím na **Konfigurace Point-to-Site** otevřete okno **Konfigurace**.
   
    ![Okno Point-to-Site](./media/vpn-gateway-howto-point-to-site-resource-manager-portal/configuration.png)
2. **Fond adres** je fond IP adres, ze kterého získavají IP adresu klienti, kteří se připojí. Přidejte fond adres a pak klikněte na **Uložit**.
   
    ![Fond adres klienta](./media/vpn-gateway-howto-point-to-site-resource-manager-portal/ipaddresspool.png)

## <a name="uploadfile"></a>Část 8 – Nahrání souboru .cer kořenového certifikátu
Po vytvoření brány můžete do Azure nahrát soubor .cer pro důvěryhodný kořenový certifikát. Můžete nahrát soubory až 20 kořenových certifikátů. Privátní klíč kořenového certifikátu nebudete nahrávat do Azure. Nahraný soubor .cer se v Azure používá k ověřování klientů, kteří se připojují k virtuální síti.

1. Certifikáty se přidávají v okně **Konfigurace Point-to-Site** v části **Kořenový certifikát**.  
2. Ujistěte se, že jste kořenový certifikát vyexportovali jako soubor .cer X.509 s kódováním Base-64. Je nutné certifikát exportovat v tomto formátu, abyste ho mohli otevřít v textovém editoru.
3. Otevřete certifikát v textovém editoru, například v Poznámkovém bloku. Zkopírujte pouze tuto část jako jeden souvislý řádek:
   
    ![Data certifikátu](./media/vpn-gateway-howto-point-to-site-resource-manager-portal/copycert.png)

    > [!NOTE]
    > Při kopírování dat certifikátu se ujistěte, že kopírujete text jako jeden souvislý řádek bez konců řádků nebo odřádkování. Pokud chcete zobrazit konce řádků a odřádkování, může být nutné upravit zobrazení v textovém editoru na Zobrazit symbol / Zobrazit všechny znaky.                                                                                                                                                                            
    >
    >

4. Vložte data certifikátu do pole **Data veřejného certifikátu**. Zadejte **Název** certifikátu a pak klikněte na tlačítko **Uložit**. Můžete přidat až 20 důvěryhodných kořenových certifikátů.
   
    ![Nahrání certifikátu](./media/vpn-gateway-howto-point-to-site-resource-manager-portal/rootcertupload.png)

## <a name="clientconfig"></a>Část 9 – Stažení a instalace balíčku pro konfiguraci klienta VPN
Klienti připojení k Azure pomocí P2S musí mít nainstalovaný certifikát klienta i balíček pro konfiguraci klienta VPN. Pro klienty se systémem Windows jsou k dispozici balíčky pro konfiguraci klientů VPN. 

Balíček klienta VPN obsahuje informace o konfiguraci softwaru klienta VPN, který je součástí Windows. Konfigurace je specifická pro síť VPN, ke které se chcete připojit. Balíček neinstaluje další software.

V každém klientském počítači můžete použít stejný konfigurační balíček klienta VPN za předpokladu, že jeho verze odpovídá architektuře klienta.

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


## <a name="installclientcert"></a>Část 10 – Instalace exportovaného klientského certifikátu

Pokud chcete vytvořit připojení P2S z jiného klientského počítače, než který jste použili k vytvoření klientských certifikátů, budete muset klientský certifikát nainstalovat. Při instalaci klientského certifikátu budete potřebovat heslo, které bylo vytvořeno při jeho exportu.

1. Vyhledejte a zkopírujte soubor *.pfx* na klientský počítač. Na klientském počítači spusťte instalaci dvojím kliknutím na soubor *.pfx*. Ponechte **Umístění úložiště** jako **Aktuální uživatel** a pak klikněte na **Další**.
2. Na stránce **Importovaný soubor** neprovádějte žádné změny. Klikněte na **Další**.
3. Na stránce **Ochrana privátního klíče** zadejte heslo pro certifikát nebo ověřte správnost objektu zabezpečení, a pak klikněte na **Další**.
4. Na stránce **Úložiště certifikátů** ponechte výchozí umístění a pak klikněte na **Další**.
5. Klikněte na **Finish** (Dokončit). V okně **Upozornění zabezpečení** pro instalaci certifikátu klikněte na **Ano**. Na tlačítko Ano můžete s klidem kliknout, protože jste certifikát generovali vy. Certifikát se teď úspěšně naimportoval.

## <a name="connect"></a>Část 11 – Připojení k Azure
 
 1. Chcete-li se připojit ke své síti VNet, přejděte na klientském počítači na připojení VPN a vyhledejte připojení VPN, které jste vytvořili. Bude mít stejný název jako vaše virtuální síť. Klikněte na **Připojit**. Může se zobrazit místní zpráva týkající se použití certifikátu. Klikněte na **Pokračovat** pro použití zvýšených oprávnění.

2. Připojení spustíte kliknutím na tlačítko **Připojit** na stavové stránce **Připojení**. Pokud uvidíte obrazovku **Výběr certifikátu**, ujistěte se, že zobrazený klientský certifikát je ten, který chcete pro připojení použít. Pokud není, vyberte pomocí šipky rozevíracího seznamu správný certifikát, a poté klikněte na **OK**.
   
    ![Připojení klienta VPN k Azure](./media/vpn-gateway-howto-point-to-site-resource-manager-portal/clientconnect.png)
3. Vaše připojení bylo vytvořeno.
   
    ![Vytvořené připojení](./media/vpn-gateway-howto-point-to-site-resource-manager-portal/connected.png)

Pokud máte potíže s připojením, zkontrolujte následující položky:

- Otevřete **správu uživatelských certifikátů** a přejděte do umístění **Důvěryhodné kořenové certifikační autority\Certifikáty**. Zkontrolujte, že je kořenový certifikát uvedený. Aby ověřování fungovalo, musí být kořenový certifikát přítomný. Pokud exportujete klientský certifikát .pfx s výchozím nastavením Zahrnout všechny certifikáty na cestě k certifikátu, pokud je to možné, vyexportují se také informace o kořenovém certifikátu. Při instalaci klientského certifikátu se pak do klientského počítače nainstaluje i kořenový certifikát. 

- Pokud používáte certifikát vydaný řešením CA organizace a máte problémy s ověřováním, zkontrolujte pořadí ověřování na klientském certifikátu. Pořadí seznamu ověřování můžete zkontrolovat dvojím kliknutím na klientský certifikát a přechodem na **Podrobnosti > Použití rozšířeného klíče**. Ujistěte se, že je na prvním místě seznamu položka Ověření klienta. Pokud tomu tak není, musíte vydat klientský certifikát založený na šabloně uživatele, která má Ověření klienta jako první položku seznamu.


## <a name="verify"></a>Část 12 – Ověření připojení
1. Chcete-li ověřit, zda je připojení VPN aktivní, v příkazovém řádku se zvýšenými oprávněními spusťte příkaz *ipconfig/all*.
2. Zkontrolujte výsledky. Všimněte si, že IP adresa, kterou jste obdrželi, je jedna z adres z fondu adres klienta VPN připojení Point-to-Site, který jste určili během konfigurace. Výsledky jsou podobné tomuto příkladu:
   
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


Pokud máte potíže s připojením k virtuálnímu počítači přes P2S, pomocí příkazu ipconfig zkontrolujte IPv4 adresu přiřazenou adaptéru Ethernet na počítači, ze kterého se připojujete. Pokud je IP adresa v rámci rozsahu adres virtuální sítě, ke které se připojujete, nebo v rámci rozsahu adres VPNClientAddressPool, tato situace se označuje jako překrývající se adresní prostor. Když se adresní prostor tímto způsobem překrývá, síťový provoz nemá přístup do Azure a zůstane v místní síti. Pokud se adresní prostory vaší sítě nepřekrývají a vy se pořád nemůžete připojit k virtuálnímu počítači, přečtěte si [řešení potíží s Připojením ke vzdálené ploše pro virtuální počítač](../virtual-machines/windows/troubleshoot-rdp-connection.md).

## <a name="add"></a>Přidání a odebrání důvěryhodných kořenových certifikátů
Důvěryhodný kořenový certifikát můžete do Azure přidat nebo ho z Azure odebrat. Pokud odeberete důvěryhodný certifikát, nebude již možné se pomocí klientských certifikátů vygenerovaných z tohoto kořenového certifikátu připojit k Azure prostřednictvím připojení Point-to-Site. Pokud chcete, aby se klienti mohli i nadále připojovat, je nutné nainstalovat nový certifikát, který je vygenerován z certifikátu, který Azure považuje za důvěryhodný.

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



