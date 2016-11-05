---
title: Konfigurace připojení brány VPN typu Point-to-Site ke službě Azure Virtual Network pomocí portálu Azure Portal | Microsoft Docs
description: Připojte se bezpečně ke službě Azure Virtual Network vytvořením připojení brány VPN typu Point-to-Site přes Azure Portal.
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: carmonm
editor: ''
tags: azure-service-management

ms.service: vpn-gateway
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/06/2016
ms.author: cherylmc

---
# Konfigurace připojení typu Point-to-Site k virtuální síti přes Azure Portal
> [!div class="op_single_selector"]
> * [Resource Manager – PowerShell](vpn-gateway-howto-point-to-site-rm-ps.md)
> * [Classic – Azure Portal](vpn-gateway-howto-point-to-site-classic-azure-portal.md)
> * [Classic – klasický portál](vpn-gateway-point-to-site-create.md)
> 
> 

Tento článek vás provede vytvořením virtuální sítě s připojením Point-to-Site v modelu nasazení Classic s použitím portálu Azure Portal. Konfigurace Point-to-Site (P2S) umožňuje vytvořit zabezpečené připojení jednotlivých klientských počítačů k virtuální síti. Připojení P2S je užitečné, když se chcete ke své virtuální síti připojit ze vzdáleného umístění, například z domova nebo z místa konání konference. Nebo když máte jen několik klientů, kteří se potřebují připojovat k virtuální síti.

Připojení typu Point-to-Site k fungování nevyžadují zařízení VPN ani veřejnou IP adresu. Připojení VPN je vytvořeno spuštěním připojení na klientském počítači. Další informace o připojeních typu Point-to-Site najdete v tématech [Služba VPN Gateway – nejčastější dotazy](vpn-gateway-vpn-faq.md#point-to-site-connections) a [Informace o VPN Gateway](vpn-gateway-about-vpngateways.md#point-to-site).

### Modely a metody nasazení pro připojení P2S
[!INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)]

Následující tabulka zobrazuje dva modely nasazení a dostupné nástroje pro každý z nich. Pokud je k dispozici článek, uvádíme na něj odkaz.

[!INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-table-point-to-site-include.md)]

## Základní pracovní postup
![Diagram Point-to-Site](./media/vpn-gateway-howto-point-to-site-rm-ps/p2srm.png "point-to-site")

Následující oddíly vás provedou postupem vytvoření bezpečného připojení typu Point-to-Site k virtuální síti. 

1. Vytvoření virtuální sítě a brány VPN
2. Generování certifikátů
3. Nahrání souboru .cer
4. Generování konfiguračního balíčku klienta VPN
5. Konfigurace klientského počítače
6. Připojení k Azure

### Příklad nastavení
Můžete použít následující příklad nastavení:

* **Název: VNet1**
* **Adresní prostor: 192.168.0.0/16**
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
### Část 1: Vytvoření virtuální sítě
Pokud chcete vytvořit síť VNet přes Azure Portal, použijte následující postup. Snímky obrazovek slouží jen jako příklady. Nezapomeňte hodnoty nahradit vlastními.

1. V prohlížeči přejděte na [portál Azure](http://portal.azure.com) a v případě potřeby se přihlaste pomocí účtu Azure.
2. Klikněte na možnost **Nové**. Do pole **Hledat na Marketplace** zadejte text „Virtuální síť“. Ve vráceném seznamu vyhledejte položku **Virtuální síť** a kliknutím otevřete okno **Virtuální síť**.
   
    ![Okno pro vyhledání virtuální sítě](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/newvnetportal700.png "Search for virtual network blade")
3. U dolního okraje okna virtuální sítě v seznamu **Vybrat model nasazení** vyberte **Klasické** a potom klikněte na **Vytvořit**.
   
    ![Výběr modelu nasazení](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/selectmodel.png "Select deployment model")
4. V okně **Vytvoření virtuální sítě** nakonfigurujte nastavení sítě VNet. V tomto okně přidáte první adresní prostor a jeden rozsah adres podsítě. Po dokončení vytváření sítě VNet můžete přejít zpět a přidat další podsítě a adresní prostory.
   
    ![Okno Vytvořit virtuální síť](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/vnet125.png "Create virtual network blade")
5. V rozevíracím seznamu **Předplatné** zkontrolujte, jestli je vybrané správné předplatné. Předplatná můžete měnit prostřednictvím rozevíracího seznamu.
6. Klikněte na **Skupina prostředků** a vyberte existující skupinu nebo zadáním názvu nové skupiny prostředků vytvořte novou skupinu prostředků. Pokud vytváříte novou skupinu, nazvěte skupinu prostředků v souladu s hodnotami plánované konfigurace. Další informace o skupinách prostředků najdete v článku [Přehled Azure Resource Manageru](../resource-group-overview.md#resource-groups).
7. Potom vyberte nastavení **Umístění** sítě VNet. Umístění určuje, kde budou uložené prostředky nasazené v této síti VNet.
8. Pokud chcete mít k síti VNet snadný přístup na řídicím panelu, vyberte možnost **Připnout na řídicí panel** a potom klikněte na **Vytvořit**.
   
    ![Připnutí na řídicí panel](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/pintodashboard150.png "Pin to dashboard")
9. Po kliknutí na Vytvořit se na řídicím panelu zobrazí dlaždice s informacemi o průběhu vytváření sítě VNet. Obsah dlaždice se v průběhu vytváření sítě VNet mění.
   
    ![dlaždice Vytváří se virtuální síť](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/deploying150.png "Creating virtual network tile")
10. Po vytvoření virtuální sítě můžete přidat IP adresu serveru DNS, aby bylo možné zpracovávat překlad názvů. Otevřete nastavení pro virtuální síť, klikněte na servery DNS a přidejte IP adresu serveru DNS, který chcete použít. Toto nastavení nevytvoří nový server DNS. Je nutné přidat server DNS, s nímž můžou komunikovat vaše prostředky.

Po vytvoření virtuální sítě uvidíte na portálu Azure Classic na stránce sítí v části **Stav** stav **Vytvořená**.

### Část 2: Vytvoření podsítě brány a brány dynamického směrování
V tomto kroku vytvoříte podsíť brány a bránu dynamického směrování. Na portálu Azure Portal pro model nasazení Classic je možné vytvořit podsíť brány a bránu pomocí stejných konfiguračních oken.

1. Na portálu přejděte na virtuální síť, pro kterou chcete vytvořit bránu.
2. V okně pro virtuální síť klikněte v okně **Přehled** v části pro připojení k síti VPN na položku **Brána**.
   
    ![Kliknutím sem vytvoříte bránu.](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/beforegw125.png "Click here to create a gateway")
3. V okně **Nové připojení VPN** vyberte **Point-to-site**.
   
    ![Typ připojení P2S](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/newvpnconnect.png "P2S connection type")
4. Do pole **Klientský adresní prostor** přidejte rozsah IP adres. Z tohoto rozsahu dostanou klienti VPN IP adresu při svém připojení. Odstraňte automaticky vyplněný rozsah a přidejte vlastní.
   
    ![Klientský adresní prostor](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/clientaddress.png "Client address space")
5. Zaškrtněte políčko **Vytvořit bránu hned**.
   
    ![Zaškrtávací políčko Vytvořit bránu hned](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/creategwimm.png "Create gateway immediately")
6. Kliknutím na **Volitelná konfigurace brány** otevřete okno **Konfigurace brány**.
   
    ![Kliknutí na tlačítko Volitelná konfigurace brány](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/optsubnet125.png "Click optional gateway configuration")
7. Klikněte na **Podsíť – Konfigurovat požadované nastavení** a přidejte požadovanou **podsíť brány**. I když je možné vytvořit podsíť brány s minimální velikostí /29, doporučujeme vytvořit větší podsíť, která pojme více adres, tzn. vybrat velikost aspoň /28 nebo /27. Tím vznikne dostatečný prostor pro adresy, který umožní nastavení případných dalších konfigurací v budoucnu.
   
   > [!IMPORTANT]
   > Při práci s podsítěmi brány nepřidružujte skupinu zabezpečení sítě (NSG) k podsíti brány. Pokud byste k této podsíti přidružili skupinu zabezpečení sítě, brána sítě VPN by mohla přestat fungovat podle očekávání. Další informace o skupinách zabezpečení sítě najdete v článku [Co je skupina zabezpečení sítě (NSG)](../virtual-network/virtual-networks-nsg.md).
   > 
   > 
   
    ![Přidání podsítě brány](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/gwsubnet125.png "Add GatewaySubnet")
8. Vyberte **velikost** brány. Jde o hodnotu SKU, se kterou vytvoříte bránu virtuální sítě. Na portálu je výchozí SKU **Basic**. Další informace o SKU brány najdete v tématu [Informace o nastavení VPN Gateway](vpn-gateway-about-vpn-gateway-settings.md#gwsku).
   
    ![Velikost brány](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/gwsize125.png)
9. Vyberte pro bránu **typ směrování**. Konfigurace P2S vyžadují **dynamický** typ směrování. Až dokončíte konfiguraci tohoto okna, klikněte na **OK**.
   
    ![Konfigurace typu směrování](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/routingtype125.png "Configure routing type")
10. V dolní části okna **Nové připojení VPN** klikněte na **OK** – tím začnete bránu virtuální sítě vytvářet. Dokončení operace může trvat až 45 minut. 

## <a name="generatecerts"></a>Oddíl 2 – Generování certifikátů
Azure používá certifikáty k ověřování klientů VPN pro sítě VPN Point-to-Site. Můžete použít soubor .cer z kořenového certifikátu vygenerovaného podnikovým certifikačním řešením nebo certifikát podepsaný svým držitelem (self-signed certificate). V tomto oddílu získáte soubor .cer pro kořenový certifikát a klientský certifikát vygenerovaný z kořenového certifikátu.

### <a name="root"></a>Část 1: Získání souboru .cer pro kořenový certifikát
* Pokud používáte podnikový certifikační systém, získejte soubor .cer pro kořenový certifikát, který chcete použít. 
* Pokud nepoužíváte podnikové certifikační řešení, musíte vygenerovat kořenový certifikát podepsaný svým držitelem. Kroky pro Windows 10 najdete v tématu [Práce s kořenovými certifikáty podepsanými svým držitelem pro konfigurace Point-to-Site](vpn-gateway-certificates-point-to-site.md). Článek vás provede postupem použití nástroje makecert k vygenerování certifikátu podepsaného svým držitelem a jeho následným exportem do souboru .cer.

### Část 2: Vygenerování klientského certifikátu
Můžete buď vygenerovat jedinečný certifikát pro každého klienta, který se připojí, nebo můžete použít stejný certifikát na více klientů. Výhodou generování jedinečných certifikátů pro klienty je možnost v případě potřeby jednotlivý certifikát odvolat. V opačném případě, pokud všichni používají stejný klientský certifikát a zjistíte, že pro jednoho klienta je třeba tento certifikát odvolat, bude nutné vygenerovat a nainstalovat nové certifikáty pro všechny klienty, u nichž je odvolaný certifikát taktéž používán k ověření.

* Pokud používáte podnikové certifikační řešení, vygenerujte klientský certifikát s běžným formátem jména „jmeno@vasedomena.com“, ne ve formátu „název domény\uživatelské jméno“. 
* Používáte-li certifikát podepsaný svým držitelem, naleznete informace o vygenerování certifikátu klienta v tématu [Práce s kořenovými certifikáty podepsanými svými držiteli pro účely konfigurace připojení Point-to-Site](vpn-gateway-certificates-point-to-site.md).

### Část 3: Instalace certifikátu klienta
Nainstalujte certifikát klienta v každém počítači, který se má připojovat k virtuální síti. Klientský certifikát je vyžadován pro ověřování. Instalaci klientského certifikátu je možné buď automatizovat, nebo provádět ručně. Následující kroky vás provedou procesem exportu a ruční instalace klientského certifikátu.

1. Chcete-li exportovat klientský certifikát, použijte nástroj *certmgr.msc*. Klikněte pravým tlačítkem na klientský certifikát, který chcete exportovat, klikněte na **všechny úlohy**, a poté klikněte na **exportovat**.
2. Exportujte klientský certifikát s privátním klíčem. Jedná se o soubor *.pfx*. Ujistěte se, že jste si poznamenali nebo si pamatujete heslo (klíč), které jste pro tento certifikát nastavili.

## <a name="upload"></a>Oddíl 3 – Nahrání souboru .cer
Po vytvoření brány můžete do Azure nahrát soubor .cer pro důvěryhodný kořenový certifikát. Můžete nahrát soubory až 20 kořenových certifikátů. Privátní klíč certifikátu nebudete do Azure nahrávat. Nahraný soubor .cer se v Azure používá k ověřování klientů, kteří se připojují k virtuální síti.

1. V části **Připojení VPN** v okně pro vaši síť VNet klikněte na obrázek **klienti**, čímž otevřete okno **Připojení VPN typu point-to-site**.
   
    ![Klienti](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/clients125.png "Clients")
2. V okně **Připojení VPN typu point-to-site** klikněte na **Správa certifikátů**, čímž otevřete okno **Certifikáty**.<br>
   
    ![Okno Certifikáty](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/ptsmanage.png "Certificates blade")<br><br>
3. V okně **Certifikáty** klikněte na **Nahrát**, čímž otevřete okno **Nahrát certifikát**.<br>
   
    ![Okno pro nahrání certifikátů](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/uploadcerts.png "Upload certificates blade")<br>
4. Klikněte na obrázek složky a vyhledejte soubor .cer. Vyberte soubor a potom klikněte na **OK**. Aktualizujte stránku, aby se nahraný certifikát zobrazil v okně **Certifikáty**.
   
    ![Nahrání certifikátu](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/upload.png "Upload certificate")<br>

## <a name="vpnclientconfig"></a>Oddíl 4 – Generování konfiguračního balíčku klienta VPN
Chcete-li se připojit k virtuální síti, je potřeba nakonfigurovat také klienta VPN. Pro připojení klientského počítače vyžaduje klientský certifikát a správný konfigurační balíček klienta VPN.

Balíček klienta VPN obsahuje informace o konfiguraci pro konfigurování klientského softwaru VPN, který je součástí Windows. Balíček neinstaluje další software. Nastavení jsou specifická pro virtuální síť, ke které se chcete připojit. Seznam podporovaných klientských operačních systémů naleznete v části [Připojení Point-to-Site](vpn-gateway-vpn-faq.md#point-to-site-connections) v tématu Služba VPN Gateway - nejčastější dotazy. 

### Generování konfiguračního balíčku klienta VPN
1. Na portálu Azure Portal klikněte v okně **Přehled** pro vaši síť VNet na grafiku „klienti“ v poli **Připojení VPN**, čímž otevřete okno **Připojení VPN typu point-to-site**.
2. V horní části okna **Připojení VPN typu point-to-site** vyberte balíček ke stažení odpovídající operačnímu systému klienta, na který se bude instalovat:
   
   * U 64bitových klientů vyberte **Klient VPN (64bitový)**.
   * U 32bitových klientů vyberte **Klient VPN (32bitový)**.
     
     ![Stažení balíčku pro konfiguraci klienta VPN](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/dlclient.png "Download VPN client configuration package")<br>
3. Zobrazí se zpráva, že Azure generuje konfigurační balíček klienta VPN pro virtuální síť. Za několik minut se balíček vygeneruje a zobrazí se zpráva v místním počítači, že proběhlo jeho stažení. Soubor konfiguračního balíčku uložte. Nainstalujete ho na každý klientský počítač, který se bude připojovat k virtuální síti metodou P2S.

## Oddíl 5 – Konfigurace klientského počítače
### Část 1: Instalace certifikátu klienta
Každý klientský počítač musí mít klientský certifikát pro ověření. Při instalaci klientského certifikátu budete potřebovat heslo, které bylo vytvořeno při jeho exportu.

1. Zkopírujte soubor .pfx na klientský počítač.
2. Spusťte instalaci poklikáním na soubor .pfx. 

### Část 2: Instalace konfiguračního balíčku klienta VPN
V každém klientském počítači můžete použít stejný konfigurační balíček klienta VPN za předpokladu, že jeho verze odpovídá architektuře klienta.

1. Zkopírujte konfigurační soubor do počítače, který chcete připojit k virtuální síti a dvakrát klikněte na soubor .exe. 
2. Jakmile je balíček nainstalován, můžete zahájit připojení VPN. Konfigurační balíček není podepsán společností Microsoft. Balíček můžete chtít podepsat pomocí podpisové služby vaší organizace, nebo podepsat sami pomocí nástroje [SignTool](http://go.microsoft.com/fwlink/p/?LinkId=699327). Balíček lze použít i bez podepsání. Nicméně pokud balíček není podepsaný, zobrazí se během instalace balíčku upozornění.
3. Na klientském počítači přejděte do části **Nastavení sítě** a klikněte na možnost **Síť VPN**. Zobrazí se příslušné připojení. Zobrazí se název virtuální sítě, ke kterému se bude počítač připojovat. Název bude vypadat nějak takto: 
   
    ![Klient VPN](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/vpn.png "VNet VPN client")

## Oddíl 6 – Připojení k Azure
### Připojení k síti VNet
1. Chcete-li se připojit ke své síti VNet, přejděte na klientském počítači na připojení VPN a vyhledejte připojení VPN, které jste vytvořili. Bude mít stejný název jako vaše virtuální síť. Klikněte na **Připojit**. Může se zobrazit místní zpráva týkající se použití certifikátu. Pokud k tomu dojde, klikněte na možnost **Pokračovat**, abyste použili zvýšená oprávnění. 
2. Připojení spustíte kliknutím na tlačítko **Připojit** na stavové stránce **Připojení**. Pokud uvidíte obrazovku **Výběr certifikátu**, ujistěte se, že zobrazený klientský certifikát je ten, který chcete pro připojení použít. Pokud není, vyberte pomocí šipky rozevíracího seznamu správný certifikát, a poté klikněte na **OK**.
   
    ![Připojení](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/clientconnect.png "VPN client connection")
3. Připojení by nyní mělo být navázáno.
   
    ![Vytvořené připojení](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/connected.png "Connection established")

### Ověření připojení VPN
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

## Další kroky
Můžete přidávat virtuální počítače do svojí virtuální sítě. Viz [Vytvoření vlastního virtuálního počítače](../virtual-machines/virtual-machines-windows-classic-createportal.md).

<!--HONumber=Oct16_HO3-->


