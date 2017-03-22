---
title: "Připojení počítače k virtuální síti Azure typu Point-to-Site: portál Azure Classic| Dokumentace Microsoftu"
description: "Připojte se bezpečně k své klasické síti Azure Virtual Network vytvořením připojení brány VPN typu Point-to-Site přes portál Azure Classic."
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 4f5668a5-9b3d-4d60-88bb-5d16524068e0
ms.service: vpn-gateway
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/08/2017
ms.author: cherylmc
translationtype: Human Translation
ms.sourcegitcommit: cfe4957191ad5716f1086a1a332faf6a52406770
ms.openlocfilehash: ffd5690853a7344c7122940ad9676d1903ba2d4a
ms.lasthandoff: 03/09/2017


---
# <a name="configure-a-point-to-site-connection-to-a-vnet-using-the-classic-portal-classic"></a>Konfigurace připojení typu Point-to-Site k virtuální síti pomocí klasického portálu (Classic)
> [!div class="op_single_selector"]
> * [Resource Manager – Azure Portal](vpn-gateway-howto-point-to-site-resource-manager-portal.md)
> * [Resource Manager – PowerShell](vpn-gateway-howto-point-to-site-rm-ps.md)
> * [Classic – Azure Portal](vpn-gateway-howto-point-to-site-classic-azure-portal.md)
> * [Classic – portál Azure Classic](vpn-gateway-point-to-site-create.md)
> 
> 

Konfigurace Point-to-Site (P2S) umožňuje vytvořit zabezpečené připojení jednotlivých klientských počítačů k virtuální síti. P2S je připojení VPN prostřednictvím protokolu SSTP (Secure Socket Tunneling Protocol). Připojení Point-to-Site jsou užitečná, když se chcete ke své virtuální síti připojit ze vzdáleného umístění, například z domova nebo z místa konání konference, nebo když máte jen několik klientů, kteří se potřebují připojovat k virtuální síti. Připojení typu P2S nevyžadují zařízení VPN ani veřejnou IP adresu. Připojení VPN se navazuje ze strany klientského počítače.

Tento článek vás provede vytvořením virtuální sítě s připojením Point-to-Site v klasickém modelu nasazení s použitím klasického portálu. Další informace o připojení Point-to-Site najdete v části [Nejčastější dotazy týkající se připojení Point-to-Site](#faq) na konci tohoto článku.



### <a name="deployment-models-and-methods-for-p2s-connections"></a>Modely a metody nasazení pro připojení P2S
[!INCLUDE [deployment models](../../includes/vpn-gateway-deployment-models-include.md)]

Následující tabulka uvádí dva modely nasazení a dostupné metody nasazení pro konfigurace P2S. Když je článek s postupem konfigurace k dispozici, zařadíme do tabulky přímý odkaz na něj.

[!INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-table-point-to-site-include.md)]

## <a name="basic-workflow"></a>Základní pracovní postup
![Diagram-Point-to-Site](./media/vpn-gateway-point-to-site-create/p2sclassic.png "point-to-site")

Následující kroky vás provedou postupem vytvoření bezpečného připojení typu Point-to-Site k virtuální síti. Konfigurace pro připojení typu Point-to-Site je rozdělena do čtyř oddílů. Pořadí, ve kterém nakonfigurujete každý z těchto oddílů, je důležité. Nevynechávejte kroky ani nepřeskakujte o několik kroků dopředu.

* **Oddíl 1** Vytvoření virtuální sítě a brány VPN.
* **Oddíl 2** Vytvoření certifikátů, které použijete pro ověření, a jejich nahrání.
* **Oddíl 3** Export a instalace klientských certifikátů.
* **Oddíl 4** Konfigurace klienta VPN.



## <a name="vnetvpn"></a>Oddíl 1 – Vytvoření virtuální sítě a brány VPN

Než začnete, ověřte, že máte předplatné Azure. Pokud ještě nemáte předplatné Azure, můžete si aktivovat [výhody pro předplatitele MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details) nebo si zaregistrovat [bezplatný účet](https://azure.microsoft.com/pricing/free-trial).

### <a name="part-1-create-a-virtual-network"></a>Část 1: Vytvoření virtuální sítě
1. Přihlaste se do [portálu Azure Classic](https://manage.windowsazure.com). Tyto kroky používají portál Classic, nikoli Azure Portal. V současné době nelze připojení P2S vytvořit pomocí webu Azure Portal.
2. V levém dolním rohu obrazovky klikněte na **Nový**. V podokně navigace klikněte na **Network Services**, a poté klikněte na **Virtual Network**. Kliknutím na **Vytvořit vlastní** spustíte průvodce konfigurací.
3. Na stránce **Podrobnosti virtuální sítě** zadejte následující informace, a poté klikněte na šipku Další vpravo dole.
   
   * **Název**: Zadejte název virtuální sítě. Například „VNet1“. To je název, na který se budete odkazovat při nasazování virtuálních počítačů a této virtuální sítě.
   * **Umístění**: Umístění přímo souvisí s fyzickým umístěním (oblastí), kde chcete mít umístěné prostředky (virtuální počítače). Například pokud chcete, aby virtuální počítače, které do této virtuální sítě nasadíte, byly fyzicky umístěné ve východní USA, vyberte toto umístění. Oblast přidruženou k virtuální síti nebude možné po jejím vytvoření změnit.
4. Na stránce **Servery DNS a připojení VPN** zadejte následující informace, a poté klikněte na šipku Další vpravo dole.
   
   * **Servery DNS**: Zadejte název serveru DNS a IP adresu, nebo vyberte dříve zaregistrovaný server DNS z místní nabídky. Toto nastavení neslouží k vytvoření serveru DNS. Umožňuje určit server DNS, který chcete použít pro překlad názvů pro tuto virtuální síť. Chcete-li používat výchozí službu překladu IP adres Azure, ponechte tuto sekci prázdnou.
   * **Konfigurování sítě Point-to-Site VPN**: Zaškrtněte toto políčko.
5. Na stránce **Připojení typu Point-to-Site** zadejte rozsah IP adres, ze kterého klienti sítě VPN obdrží po připojení IP adresu. Existuje několik pravidel ohledně rozsahů adres, které lze zadat. Je velmi důležité ověřit, že se rozsah, který zadáte, nepřekrývá s žádným z rozsahů ve vaší místní síti.
6. Zadejte následující informace, a poté klikněte na šipku Další.
   
   * **Adresní prostor**: Zahrnuje počáteční IP adresu a CIDR (počet adres).
   * **Přidání adresního prostoru**: Adresní prostor přidejte pouze pokud to vyžaduje návrh vaší sítě.
7. Na stránce **Adresní prostory virtuální sítě** zadejte rozsah adres, které chcete použít pro vaši virtuální síť. Toto jsou dynamické IP adresy (vyhrazené IP adresy), které budou přiřazené pro virtuální počítače a další instance rolí, které nasadíte do této virtuální sítě.<br><br>Je obzvlášť důležité vybrat rozsah, který se nepřekrývá s žádným z rozsahů, které se používají ve vaší místní síti. Budete se muset domluvit se správcem vaší sítě, který možná bude muset vyčlenit z adresního prostoru místní sítě rozsah IP adres, které budete moci použít pro virtuální síť.
8. Zadejte následující informace, a poté kliknutím na značku zaškrtnutí zahájíte vytváření virtuální sítě.
   
   * **Adresní prostor**: Přidejte rozsah interních IP adres, které chcete používat pro tuto virtuální síť, včetně počáteční IP adresy a počtu. Je důležité vybrat rozsah, který se nepřekrývá s žádným z rozsahů, které se používají ve vaší místní síti. 
   * **Přidání podsítě**: Dodatečné podsítě nejsou vyžadovány, ale možná budete chtít vytvořit samostatnou podsíť pro virtuální počítače, které budou mít statické vyhrazené IP adresy. Nebo možná budete chtít mít virtuální počítače v podsíti oddělené od dalších instancí rolí.
   * **Přidání podsítě brány**: Podsíť brány je vyžadována pro síť Point-to-Site VPN. Klikněte pro přidání podsítě brány. Podsíť brány se používá pouze pro bránu virtuální sítě.
9. Po vytvoření virtuální sítě uvidíte na portálu Azure Classic na stránce sítí v části **Stav** stav **Vytvořená**. Po vytvoření virtuální sítě můžete vytvořit bránu dynamického směrování.

### <a name="part-2-create-a-dynamic-routing-gateway"></a>Část 2: Vytvoření brány dynamického směrování
Typ brány musí být nakonfigurován jako dynamický. Brány statického směrování nebudou s touto funkcí fungovat.

1. V klasickém portálu Azure na stránce **Sítě** klikněte na virtuální síť, kterou jste právě vytvořili, a přejděte na stránku **Řídicí panel**.
2. V dolní části stránky **Řídicí panel** klikněte na tlačítko **Vytvořit bránu**. Zobrazí se zpráva s dotazem **Opravdu chcete vytvořit bránu pro virtuální síť „VNet1“**. Kliknutím na **Ano** zahájíte proces vytváření brány. Vytvoření brány může trvat až 45 minut.

## <a name="generate"></a>Oddíl 2 – Generování a nahrávání certifikátů
Azure používá certifikáty k ověřování klientů VPN pro sítě VPN Point-to-Site. Po vytvoření kořenového certifikátu vyexportujete data veřejného certifikátu (ne privátní klíč) jako soubor .cer X.509 v kódování Base-64. Pak odešlete data veřejného certifikátu z kořenového certifikátu do Azure.

Každý klientský počítač, který se připojuje k virtuální síti pomocí připojení Point-to-Site, musí mít nainstalovaný klientský certifikát. Klientský certifikát se generuje z kořenového certifikátu a nainstaluje na každý klientský počítač. Pokud není nainstalovaný platný klientský certifikát a klient se pokusí připojit k virtuální síti, ověření se nezdaří.

V této části provedete následující:

* Získejte soubor .cer pro kořenový certifikát. Může jím být buď kořenový certifikát podepsaný svým držitelem nebo můžete použít podnikový certifikační systém.
* Nahrajte soubor .cer do Azure.
* Vygenerujte klientské certifikáty.

### <a name="root"></a>Část 1: Získání souboru .cer pro kořenový certifikát


Pokud používáte podnikové řešení, můžete použít stávající řetěz certifikátů. Získejte soubor .cer pro kořenový certifikát, který chcete používat.


Pokud nepoužíváte podnikové certifikační řešení, musíte vytvořit kořenový certifikát podepsaný svým držitelem. Chcete-li vytvořit kořenový certifikát podepsaný svým držitelem, který obsahuje potřebná pole pro ověřování P2S, můžete použít PowerShell. Téma s popisem [vytvoření kořenového certifikátu podepsaného svým držitelem pro připojení Point-to-Site pomocí PowerShellu](vpn-gateway-certificates-point-to-site.md) vás provede postupem vytvoření kořenového certifikátu podepsaného svým držitelem.

> [!NOTE]
> Dříve se doporučovalo k vytvoření kořenového certifikátu podepsaného svým držitelem a k vygenerování klientských certifikátů pro připojení Point-to-Site používat makecert. Teď můžete k vytvoření těchto certifikátů použít PowerShell. Jednou z výhod použití PowerShellu je schopnost vytvářet certifikáty SHA-2. Požadované hodnoty najdete v tématu s popisem [vytvoření kořenového certifikátu podepsaného svým držitelem pro připojení Point-to-Site pomocí PowerShellu](vpn-gateway-certificates-point-to-site.md).
>
>


#### <a name="to-export-the-public-key-for-a-self-signed-root-certificate"></a>Export veřejného klíče pro kořenový certifikát podepsaný svým držitelem

Připojení Point-to-Site vyžadují, aby byl veřejný klíč (.cer) odeslán do Azure. Následující postup vám pomůže vyexportovat soubor .cer pro váš kořenový certifikát podepsaný svým držitelem.

1. Chcete-li získat soubor .cer z certifikátu, otevřete **certmgr.msc**. Vyhledejte kořenový certifikát podepsaný svým držitelem, obvykle v Certificates - Current User\Personal\Certificates, a klikněte pravým tlačítkem myši. Klikněte na **Všechny úlohy** a potom klikněte na **Exportovat**. Otevře se **Průvodce exportem certifikátu**.
2. V průvodci klikněte na **Další**. Vyberte **Ne, neexportovat privátní klíč** a klikněte na **Další**.
3. Na stránce **Formát souboru pro export** vyberte **X.509, kódování Base-64 (CER)** a klikněte na **Další**. 
4. V části **Soubor pro export** **přejděte** do umístění, do kterého chcete certifikát vyexportovat. V části **Název souboru** zadejte název souboru. Pak klikněte na tlačítko **Další**.
5. Certifikát vyexportujte kliknutím na **Dokončit**. Zobrazí se zpráva o **úspěšném exportu**. Kliknutím na **OK** zavřete průvodce.

### <a name="upload"></a>Část 2: Nahrání souboru .cer s kořenovým certifikátem na portál Azure Classic

Přidejte do Azure důvěryhodný certifikát. Pokud do Azure přidáte soubor X.509 zakódovaný ve formátu Base64 (.cer), oznamujete tím Azure, že má důvěřovat kořenovému certifikátu, který tento soubor představuje. Můžete nahrát soubory až 20 kořenových certifikátů. Privátní klíč kořenového certifikátu nebudete nahrávat do Azure. Nahraný soubor .cer se v Azure používá k ověřování klientů, kteří se připojují k virtuální síti.

1. V portálu Azure Classic na stránce **Certifikáty** pro vaši virtuální síť klikněte na **Nahrát kořenový certifikát**.
2. Na stránce **Nahrání certifikátu** vyhledejte soubor .cer kořenového certifikátu, a poté klikněte na značku zaškrtnutí.

### <a name="createclientcert"></a>Část 3: Vygenerování klientského certifikátu
Můžete buď vygenerovat jedinečný certifikát pro každého klienta, který se připojí, nebo můžete použít stejný certifikát na více klientů. Výhodou generování jedinečných certifikátů pro klienty je možnost v případě potřeby jednotlivý certifikát odvolat. V opačném případě, pokud všichni používají stejný klientský certifikát a zjistíte, že pro jednoho klienta je třeba tento certifikát odvolat, bude nutné vygenerovat a nainstalovat nové certifikáty pro všechny klienty, u nichž je odvolaný certifikát taktéž používán k ověření.

####<a name="enterprise-certificate"></a>Podnikový certifikát
- Pokud používáte podnikové certifikační řešení, vygenerujte klientský certifikát s běžným názvem ve formátu name@yourdomain.com (namísto formátu název_domény\uživatelské_jméno).
- Ujistěte se, že je vámi vydaný certifikát založený na šabloně uživatelského certifikátu, která má jako první položku v seznamu používání Ověření klienta místo Přihlášení pomocí čipové karty atd. Certifikát můžete zkontrolovat dvojím kliknutím na klientský certifikát a zobrazením **Podrobnosti > Použití rozšířeného klíče**.

####<a name="self-signed-root-root-certificate"></a>Kořenový certifikát podepsaný svým držitelem 
Pokud používáte kořenový certifikát podepsaný svým držitelem, přečtěte si v tématu s popisem [vygenerování klientského certifikátu pomocí PowerShellu](vpn-gateway-certificates-point-to-site.md#clientcert) kroky, podle kterých se generuje klientský certifikát kompatibilní s připojeními Point-to-Site.


## <a name="installclientcert"></a>Oddíl 3 – Export a instalace klientského certifikátu

Po vygenerování klientského certifikátu je nutné certifikát exportovat jako soubor .pfx a nainstalovat jej na klientském počítači. Každý klientský počítač musí mít klientský certifikát pro ověření. Instalaci klientského certifikátu můžete buď automatizovat, nebo můžete certifikát nainstalovat ručně. Následující kroky vás provedou procesem exportu a ruční instalace klientského certifikátu.

### <a name="export-the-client-certificate"></a>Export klientského certifikátu

1. Chcete-li exportovat klientský certifikát, otevřete **certmgr.msc**. Klikněte pravým tlačítkem na klientský certifikát, který chcete exportovat, klikněte na **všechny úlohy**, a poté klikněte na **exportovat**. Otevře se **Průvodce exportem certifikátu**.
2. V průvodci klikněte na **Další**, vyberte **Ano, exportovat privátní klíč** a klikněte na **Další**.
3. Na stránce **Formát souboru pro export** můžete ponechat vybrané výchozí nastavení. Pak klikněte na tlačítko **Další**. 
4. Na stránce **Zabezpečení** je nutné chránit soukromý klíč. Pokud vyberete použití hesla, ujistěte se, že jste si poznamenali nebo si pamatujete heslo, které jste pro tento certifikát nastavili. Pak klikněte na tlačítko **Další**.
5. V části **Soubor pro export** **přejděte** do umístění, do kterého chcete certifikát vyexportovat. V části **Název souboru** zadejte název souboru. Pak klikněte na tlačítko **Další**.
6. Certifikát vyexportujte kliknutím na **Dokončit**.

### <a name="install-the-client-certificate"></a>Instalace klientského certifikátu

Při instalaci klientského certifikátu budete potřebovat heslo, které bylo vytvořeno při jeho exportu.

1. Vyhledejte a zkopírujte soubor *.pfx* na klientský počítač. Na klientském počítači spusťte instalaci dvojím kliknutím na soubor *.pfx*. Ponechte **Umístění úložiště** jako **Aktuální uživatel** a pak klikněte na **Další**.
2. Na stránce **Importovaný soubor** neprovádějte žádné změny. Klikněte na **Další**.
3. Na stránce **Ochrana privátního klíče** zadejte heslo pro certifikát, pokud jste ho použili, nebo ověřte správnost instalujícího objektu zabezpečení, a pak klikněte na **Další**.
4. Na stránce **Úložiště certifikátů** ponechte výchozí umístění a pak klikněte na **Další**.
5. Klikněte na **Finish** (Dokončit). V okně **Upozornění zabezpečení** pro instalaci certifikátu klikněte na **Ano**. Na tlačítko Ano můžete s klidem kliknout, protože jste certifikát generovali vy. Certifikát se teď úspěšně naimportoval.

## <a name="vpnclientconfig"></a>Oddíl 4 – Konfigurace klienta VPN
Chcete-li se připojit k virtuální síti, je potřeba nakonfigurovat také klienta VPN. Klient pro úspěšné připojení vyžaduje klientský certifikát a správný konfigurační balíček klienta VPN. Klienta VPN nakonfigurujete pomocí následujících kroků ve správném pořadí.

### <a name="part-1-create-the-vpn-client-configuration-package"></a>Část 1: Vytvoření konfiguračního balíčku klienta VPN
1. Na portálu Azure Classic na stránce **Řídicí panel** u vaší virtuální sítě přejděte do rychlé nabídky , přejděte do nabídky rychlého přehledu v pravém horním rohu. Balíček klienta VPN obsahuje informace o konfiguraci pro konfigurování klientského softwaru VPN, který je součástí Windows. Balíček neinstaluje další software. Nastavení jsou specifická pro virtuální síť, ke které se chcete připojit. Seznam podporovaných klientských operačních systémů naleznete v části [Nejčastější dotazy o připojení Point-to-Site](#faq) na konci tohoto článku.<br><br>Vyberte balíček ke stažení, který odpovídá operačnímu systému klienta, na kterém bude nainstalován.
   
   * Pro 32bitové klienty vyberte **Stáhnout 32bitovou verzi balíčku klienta VPN**.
   * Pro 64bitové klienty vyberte **Stáhnout 64bitovou verzi balíčku klienta VPN**.
2. Vytvoření vašeho klientského balíčku bude trvat několik minut. Když je balíček hotový, můžete soubor stáhnout. Soubor *.exe*, který stáhnete, můžete bezpečně uložit na místním počítači.
3. Po vygenerování a stažení balíčku klienta VPN z portálu Azure Classic můžete balíček klienta nainstalovat na klientský počítač, ze kterého se chcete připojit k virtuální síti. Pokud plánujete nainstalovat balíček klienta VPN na více klientských počítačů, ujistěte se, že je na každém z nich nainstalovaný také klientský certifikát.

### <a name="part-2-install-the-vpn-configuration-package-on-the-client"></a>Část 2: Instalace konfiguračního balíčku sítě VPN na klienta
1. Zkopírujte konfigurační soubor do počítače, který chcete připojit k virtuální síti a dvakrát klikněte na soubor .exe. 
2. Jakmile je balíček nainstalován, můžete zahájit připojení VPN. Konfigurační balíček není podepsán společností Microsoft. Balíček můžete chtít podepsat pomocí podpisové služby vaší organizace, nebo podepsat sami pomocí nástroje [SignTool](http://go.microsoft.com/fwlink/p/?LinkId=699327). Balíček lze použít i bez podepsání. Nicméně pokud balíček není podepsaný, zobrazí se během instalace balíčku upozornění.
3. Na klientském počítači přejděte do části **Nastavení sítě** a klikněte na možnost **Síť VPN**. Zobrazí se příslušné připojení. Bude uveden název virtuální sítě, ke kterému se bude počítač připojovat. Název bude vypadat podobně jako tento: 
   
    ![Klient VPN](./media/vpn-gateway-point-to-site-create/vpn.png "Klient VPN")

### <a name="part-3-connect-to-azure"></a>Část 3: Připojení k Azure
1. Chcete-li se připojit ke své síti VNet, přejděte na klientském počítači na připojení VPN a vyhledejte připojení VPN, které jste vytvořili. Bude mít stejný název jako vaše virtuální síť. Klikněte na **Připojit**. Může se zobrazit místní zpráva týkající se použití certifikátu. Pokud k tomu dojde, klikněte na možnost **Pokračovat**, abyste použili zvýšená oprávnění. 
2. Připojení spustíte kliknutím na tlačítko **Připojit** na stavové stránce **Připojení**. Pokud uvidíte obrazovku **Výběr certifikátu**, ujistěte se, že zobrazený klientský certifikát je ten, který chcete pro připojení použít. Pokud není, vyberte pomocí šipky rozevíracího seznamu správný certifikát, a poté klikněte na **OK**.
   
    ![Klient VPN 2](./media/vpn-gateway-point-to-site-create/clientconnect.png "Připojení klienta VPN")
3. Připojení by nyní mělo být navázáno.
   
    ![Klient VPN 3](./media/vpn-gateway-point-to-site-create/connected.png "Připojení klienta VPN 2")

> [!NOTE]
> Pokud používáte certifikát vydaný řešením CA organizace a máte problémy s ověřováním, zkontrolujte pořadí ověřování na klientském certifikátu. Pořadí seznamu ověřování můžete zkontrolovat dvojím kliknutím na klientský certifikát a přechodem na **Podrobnosti > Použití rozšířeného klíče**. Ujistěte se, že je na prvním místě seznamu položka Ověření klienta. Pokud tomu tak není, musíte vydat klientský certifikát založený na šabloně uživatele, která má Ověření klienta jako první položku seznamu. 
>
>

### <a name="part-4-verify-the-vpn-connection"></a>Část 4: Ověření připojení VPN
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

## <a name="faq"></a>Nejčastější dotazy týkající se připojení Point-to-Site

[!INCLUDE [Point-to-Site FAQ](../../includes/vpn-gateway-point-to-site-faq-include.md)]

## <a name="next-steps"></a>Další kroky

Po dokončení připojení můžete do virtuálních sítí přidávat virtuální počítače. Další informace najdete v tématu [Virtuální počítače](https://docs.microsoft.com/azure/#pivot=services&panel=Compute). Bližší informace o sítích a virtuálních počítačích najdete v tématu s [přehledem sítě virtuálních počítačů s Linuxem v Azure](../virtual-machines/virtual-machines-linux-azure-vm-network-overview.md).


