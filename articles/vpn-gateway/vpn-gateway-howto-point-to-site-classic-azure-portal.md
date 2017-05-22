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
ms.date: 05/03/2017
ms.author: cherylmc
ms.translationtype: Human Translation
ms.sourcegitcommit: e72275ffc91559a30720a2b125fbd3d7703484f0
ms.openlocfilehash: add8cf22430c9e7af47e6d3c242fbd25797dd099
ms.contentlocale: cs-cz
ms.lasthandoff: 05/05/2017


---
# <a name="configure-a-point-to-site-connection-to-a-vnet-using-the-azure-portal-classic"></a>Konfigurace připojení typu Point-to-Site k virtuální síti pomocí webu Azure Portal (Classic)

[!INCLUDE [deployment models](../../includes/vpn-gateway-classic-deployment-model-include.md)]

Tento článek ukazuje postup vytvoření virtuální sítě s připojením typu Point-to-Site v modelu nasazení Classic pomocí webu Azure Portal. Tuto konfiguraci můžete vytvořit také pomocí jiného nástroje nasazení nebo pro jiný model nasazení, a to výběrem jiné možnosti z následujícího seznamu:

> [!div class="op_single_selector"]
> * [Resource Manager – Azure Portal](vpn-gateway-howto-point-to-site-resource-manager-portal.md)
> * [Resource Manager – PowerShell](vpn-gateway-howto-point-to-site-rm-ps.md)
> * [Classic – Azure Portal](vpn-gateway-howto-point-to-site-classic-azure-portal.md)
>
>

Konfigurace Point-to-Site (P2S) umožňuje vytvořit zabezpečené připojení jednotlivých klientských počítačů k virtuální síti. P2S je připojení VPN prostřednictvím protokolu SSTP (Secure Socket Tunneling Protocol). Připojení Point-to-Site jsou užitečná, když se chcete ke své virtuální síti připojit ze vzdáleného umístění, například z domova nebo z místa konání konference, nebo když máte jen několik klientů, kteří se potřebují připojovat k virtuální síti. Připojení typu P2S nevyžadují zařízení VPN ani veřejnou IP adresu. Připojení VPN se navazuje ze strany klientského počítače. Další informace o připojení Point-to-Site najdete v části [Nejčastější dotazy týkající se připojení Point-to-Site](#faq) na konci tohoto článku.

![Diagram Point-to-Site](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/point-to-site-connection-diagram.png)

### <a name="example-settings"></a>Příklad nastavení

Tyto hodnoty můžete použít k vytvoření testovacího prostředí nebo můžou sloužit k lepšímu pochopení příkladů v tomto článku:

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
6. Klikněte na **Skupina prostředků** a vyberte existující skupinu nebo zadáním názvu nové skupiny prostředků vytvořte novou skupinu prostředků. Pokud vytváříte novou skupinu prostředků, nazvěte skupinu prostředků v souladu s hodnotami plánované konfigurace. Další informace o skupinách prostředků najdete v článku [Přehled Azure Resource Manageru](../azure-resource-manager/resource-group-overview.md#resource-groups).
7. Potom vyberte nastavení **Umístění** sítě VNet. Umístění určuje, kde budou uloženy prostředky nasazené v této síti VNet.
8. Pokud chcete mít k síti VNet snadný přístup na řídicím panelu, vyberte možnost **Připnout na řídicí panel** a potom klikněte na **Vytvořit**.

  ![Připnutí na řídicí panel](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/pintodashboard150.png)
9. Po kliknutí na Vytvořit se na řídicím panelu zobrazí dlaždice s informacemi o průběhu vytváření sítě VNet. Obsah dlaždice se v průběhu vytváření sítě VNet mění.

  ![dlaždice Vytváří se virtuální síť](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/deploying150.png)
10. Po vytvoření virtuální sítě uvidíte na portálu Azure Classic na stránce sítí v části **Stav** stav **Vytvořená**.
11. Přidejte server DNS (volitelné). Po vytvoření virtuální sítě můžete přidat IP adresu serveru DNS pro překlad IP adres. Zadaný server DNS by měl být ten, který dokáže překládat IP adresy pro prostředky ve vaší virtuální síti.<br>Pokud chcete přidat server DNS, otevřete nastavení pro virtuální síť, klikněte na servery DNS a přidejte IP adresu serveru DNS, který chcete použít. Balíček pro konfiguraci klienta, který vygenerujete v pozdějším kroku, bude obsahovat IP adresy serverů DNS, které zadáte v tomto nastavení. Pokud bude v budoucnu potřeba aktualizovat seznam serverů DNS, můžete vygenerovat a nainstalovat nové balíčky pro konfiguraci klienta VPN odrážející aktualizovaný seznam.

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
7. Klikněte na **Podsíť – Konfigurovat požadované nastavení** a přidejte požadovanou **podsíť brány**. I když je možné vytvořit podsíť brány s minimální velikostí /29, doporučujeme vytvořit větší podsíť, která pojme více adres, tzn. vybrat velikost aspoň /28 nebo /27. Tím vznikne dostatečný prostor pro adresy, který umožní nastavení případných dalších konfigurací v budoucnu. Při práci s podsítěmi brány nepřidružujte skupinu zabezpečení sítě (NSG) k podsíti brány. Pokud byste k této podsíti přidružili skupinu zabezpečení sítě, brána sítě VPN by mohla přestat fungovat podle očekávání.

  ![Přidání podsítě brány](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/gwsubnet125.png)
8. Vyberte **velikost** brány. Velikost je skladová položka brány pro vaši bránu virtuální sítě. Na portálu je výchozí SKU **Basic**. Další informace o SKU brány najdete v tématu [Informace o nastavení VPN Gateway](vpn-gateway-about-vpn-gateway-settings.md#gwsku).

  ![Velikost brány](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/gwsize125.png)
9. Vyberte pro bránu **typ směrování**. Konfigurace P2S vyžadují **dynamický** typ směrování. Až dokončíte konfiguraci tohoto okna, klikněte na **OK**.

  ![Konfigurace typu směrování](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/routingtype125.png)
10. V dolní části okna **Nové připojení VPN** klikněte na **OK** – tím začnete bránu virtuální sítě vytvářet. Dokončení brány sítě VPN může trvat až 45 minut.

## <a name="generatecerts"></a>Oddíl 2 – Vytvoření certifikátů

Azure používá certifikáty k ověřování klientů VPN pro sítě VPN Point-to-Site.

### <a name="cer"></a>Část 1: Získání veřejného klíče (.cer) pro kořenový certifikát

[!INCLUDE [vpn-gateway-basic-vnet-rm-portal](../../includes/vpn-gateway-p2s-rootcert-include.md)]

### <a name="genclientcert"></a>Část 2: Vygenerování klientského certifikátu

[!INCLUDE [vpn-gateway-basic-vnet-rm-portal](../../includes/vpn-gateway-p2s-clientcert-include.md)]

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

## <a name="vpnclientconfig"></a>Část 4 – Konfigurace klienta

Pokud se chcete připojit k virtuální síti pomocí sítě VPN Point-to-Site, musí mít každý klient nainstalovaný konfigurační balíček klienta VPN. Balíček neinstaluje klienta VPN. V každém klientském počítači můžete použít stejný konfigurační balíček klienta VPN za předpokladu, že jeho verze odpovídá architektuře klienta. Seznam podporovaných klientských operačních systémů naleznete v části [Nejčastější dotazy o připojení Point-to-Site](#faq) na konci tohoto článku.

Konfigurační balíček konfiguruje nativního klienta VPN ve Windows pomocí nastavení nutných k připojení k virtuální síti, a pokud jste pro virtuální síť zadali server DNS, obsahuje IP adresu serveru DNS, který klient použije pro překlad IP adres. Pokud později po vygenerování balíčku pro konfiguraci klienta změníte zadaný server DNS, nezapomeňte vygenerovat nový balíček pro konfiguraci klienta, který nainstalujete na klientských počítačích.

### <a name="part-1-generate-and-install-the-vpn-client-configuration-package"></a>Část 1: Generování a instalace balíčku pro konfiguraci klienta VPN

1. Na portálu Azure Portal klikněte v okně **Přehled** pro vaši síť VNet na grafiku „klienti“ v poli **Připojení VPN**, čímž otevřete okno **Připojení VPN typu point-to-site**.
2. V horní části okna **Připojení VPN typu point-to-site** vyberte balíček ke stažení odpovídající operačnímu systému klienta, na který se bude instalovat:

  * U 64bitových klientů vyberte **Klient VPN (64bitový)**.
  * U 32bitových klientů vyberte **Klient VPN (32bitový)**.

  ![Stažení balíčku pro konfiguraci klienta VPN](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/dlclient.png)<br>
3. Jakmile je balíček vygenerovaný, stáhněte ho a nainstalujte na klientský počítač. Pokud se zobrazí automaticky otevírané okno filtru SmartScreen, klikněte na **Další informace** a potom na **Přesto spustit**. Můžete také balíček uložit k instalaci na další klientské počítače.

### <a name="part-2-install-the-client-certificate"></a>Část 2: Instalace klientského certifikátu

Pokud chcete vytvořit připojení P2S z jiného klientského počítače, než který jste použili k vytvoření klientských certifikátů, budete muset klientský certifikát nainstalovat. Při instalaci klientského certifikátu budete potřebovat heslo, které bylo vytvořeno při jeho exportu. Obvykle stačí jenom dvakrát kliknout na certifikát a nainstalovat ho. Další informace najdete v tématu [Instalace exportovaného klientského certifikátu](vpn-gateway-certificates-point-to-site.md#install).

## <a name="connect"></a>Část 5 – Připojení k Azure

### <a name="connect-to-your-vnet"></a>Připojení k síti VNet

1. Chcete-li se připojit ke své síti VNet, přejděte na klientském počítači na připojení VPN a vyhledejte připojení VPN, které jste vytvořili. Bude mít stejný název jako vaše virtuální síť. Klikněte na **Připojit**. Může se zobrazit místní zpráva týkající se použití certifikátu. Pokud k tomu dojde, klikněte na možnost **Pokračovat**, abyste použili zvýšená oprávnění.
2. Připojení spustíte kliknutím na tlačítko **Připojit** na stavové stránce **Připojení**. Pokud uvidíte obrazovku **Výběr certifikátu**, ujistěte se, že zobrazený klientský certifikát je ten, který chcete pro připojení použít. Pokud není, vyberte pomocí šipky rozevíracího seznamu správný certifikát, a poté klikněte na **OK**.

  ![Připojení klienta VPN](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/clientconnect.png)
3. Vaše připojení bylo vytvořeno.

  ![Vytvořené připojení](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/connected.png)

Pokud máte potíže s připojením, zkontrolujte následující:

- Otevřete **správu uživatelských certifikátů** a přejděte do umístění **Důvěryhodné kořenové certifikační autority\Certifikáty**. Zkontrolujte, že je kořenový certifikát uvedený. Aby ověřování fungovalo, musí být kořenový certifikát přítomný. Pokud exportujete klientský certifikát .pfx s výchozím nastavením Zahrnout všechny certifikáty na cestě k certifikátu, pokud je to možné, vyexportují se také informace o kořenovém certifikátu. Při instalaci klientského certifikátu se pak do klientského počítače nainstaluje i kořenový certifikát. 

- Pokud používáte certifikát vydaný řešením CA organizace a máte problémy s ověřováním, zkontrolujte pořadí ověřování na klientském certifikátu. Pořadí seznamu ověřování můžete zkontrolovat dvojím kliknutím na klientský certifikát a přechodem na **Podrobnosti > Použití rozšířeného klíče**. Ujistěte se, že je na prvním místě seznamu položka Ověření klienta. Pokud tomu tak není, musíte vydat klientský certifikát založený na šabloně uživatele, která má Ověření klienta jako první položku seznamu. 

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

 
 Pokud máte potíže s připojením k virtuálnímu počítači přes P2S, pomocí příkazu ipconfig zkontrolujte IPv4 adresu přiřazenou adaptéru Ethernet na počítači, ze kterého se připojujete. Pokud je IP adresa v rámci rozsahu adres virtuální sítě, ke které se připojujete, nebo v rámci rozsahu adres VPNClientAddressPool, tato situace se označuje jako překrývající se adresní prostor. Když se adresní prostor tímto způsobem překrývá, síťový provoz nemá přístup do Azure a zůstane v místní síti. Pokud se adresní prostory vaší sítě nepřekrývají a vy se pořád nemůžete připojit k virtuálnímu počítači, přečtěte si [řešení potíží s Připojením ke vzdálené ploše pro virtuální počítač](../virtual-machines/windows/troubleshoot-rdp-connection.md).

## <a name="connectVM"></a>Připojení k virtuálnímu počítači

[!INCLUDE [Connect to a VM](../../includes/vpn-gateway-connect-vm-p2s-classic-include.md)]

## <a name="add"></a>Přidání a odebrání důvěryhodných kořenových certifikátů

Důvěryhodný kořenový certifikát můžete do Azure přidat nebo ho z Azure odebrat. Když odeberete kořenový certifikát, klienti s certifikátem vygenerovaným z tohoto kořenového certifikátu se nebudou moci ověřit a proto ani připojit. Pokud chcete, aby se klient mohl i nadále ověřovat a připojovat, je nutné nainstalovat nový klientský certifikát vygenerovaný z kořenového certifikátu, který Azure považuje za důvěryhodný (je do Azure nahraný).

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
Po dokončení připojení můžete do virtuálních sítí přidávat virtuální počítače. Další informace najdete v tématu [Virtuální počítače](https://docs.microsoft.com/azure/#pivot=services&panel=Compute). Bližší informace o sítích a virtuálních počítačích najdete v tématu s [přehledem sítě virtuálních počítačů s Linuxem v Azure](../virtual-machines/linux/azure-vm-network-overview.md).

